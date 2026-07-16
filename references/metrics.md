# Métricas UX — probar que el fix mejoró algo

La Fase 3 verifica que compila y se ve bien; esto verifica que **mejoró**. Elegí 2-3 métricas
ANTES de remediar (baseline) y compará después. Sin baseline no hay mejora demostrable, solo
opinión.

## A. Qué medir — por tarea crítica (las de la Fase 1)
Las 3 de ISO 9241 (efectividad/eficiencia/satisfacción), operacionalizadas:
- **Task success rate** — % que completa la tarea sin ayuda. LA métrica; si solo medís una,
  esta.
- **Time-on-task** — tiempo hasta completar (mediana, no promedio — outliers mienten).
- **Error rate** — validaciones fallidas, reintentos, correcciones por tarea.
- **Abandono por paso** — funnel de la tarea; el paso donde se cae la gente = el hallazgo
  confirmado por datos.

## B. Frameworks
- **HEART** (Google) — elegí UNA fila por proyecto, no las cinco:
  | Dimensión | Señal ejemplo |
  |---|---|
  | **H**appiness | encuesta in-app, CSAT, NPS |
  | **E**ngagement | frecuencia/profundidad de uso por semana |
  | **A**doption | % que usa la feature nueva |
  | **R**etention | % que vuelve (D7/D30) |
  | **T**ask success | éxito/tiempo/errores (ver §A) |
  Método goals → signals → metrics: primero el objetivo, luego la señal, recién entonces el número.
- **SUS** (System Usability Scale) — 10 preguntas Likert, score 0-100; **68 = promedio**, ≥80 =
  bueno. Barato para before/after de una wave de hardening (misma escala, mismo módulo).
- **SEQ** (Single Ease Question) — "¿Qué tan fácil fue esta tarea?" 1-7 justo después de la
  tarea. Aún más barato; por-tarea, no por-producto.
- **Activation rate** (flows §A) — % de signups que llega al aha; LA métrica de onboarding.

## C. Instrumentación mínima (sin plataforma de analytics)
- Eventos en los pasos de las tareas críticas: `task_started`, `task_completed`, `task_error`,
  `task_abandoned` (+ timestamp) — con eso salen success/time/funnel. Una tabla `events` basta.
- Contadores de fricción barata: submits inválidos por form, reintentos de carga, uso de Undo
  (¿la gente borra por error?), búsquedas sin resultados (¿qué buscan que no está?).
- Errores de consola/red en producción (ya se auditan en `audit-scripts.md` — dejarlos medidos).
- **Test de usabilidad barato** (Krug, books §B): 3 usuarios × tareas críticas, antes y después
  de la wave — cualitativo pero suficiente para confirmar que el patrón arreglado ya no traba.

## D. Core Web Vitals — la performance ES experiencia
Los tres números de Google que miden UX percibida (medilos en campo, no solo Lighthouse):
- **LCP** (Largest Contentful Paint) ≤ **2.5 s** — cuánto tarda en verse lo principal.
- **INP** (Interaction to Next Paint) ≤ **200 ms** — cuánto tarda la UI en responder a un
  click/tecla (reemplazó a FID). INP alto = "la app se siente pesada" aunque cargue rápido.
- **CLS** (Cumulative Layout Shift) ≤ **0.1** — cuánto salta el layout (skeletons sin
  dimensiones fijas, imágenes sin aspect-ratio, banners que empujan = culpables típicos).
Umbral "good" = percentil 75 de usuarios reales. Un INP de 500 ms en "Guardar" es un hallazgo
[M/H] de UX, no un ticket de infra. Auditá con DevTools Performance + `web-vitals` npm.

## E. Experimentación sin autoengaño (A/B)
Cuándo el fix amerita test en vez de shiparlo directo — y cómo no mentirse:
- **No todo se testea**: bugs obvios, fixes de accesibilidad, dark patterns → se arreglan y ya.
  A/B es para cambios donde el resultado es genuinamente incierto y el tráfico alcanza.
- **Hipótesis y métrica ANTES** ("mover el CTA sube task-success del checkout") — elegir la
  métrica después de ver datos = p-hacking casero.
- **Guardrail metrics**: junto a la métrica objetivo, vigilá las que NO deben empeorar
  (conversión ↑ pero retención D30 ↓ = te estás endeudando). Errores, tiempo de tarea, soporte.
- **Novelty effect**: las primeras 1-2 semanas miden curiosidad, no valor — esperá a que la
  novedad decaiga antes de declarar victoria (mínimo un ciclo completo de uso).
- **Sample size honesto**: con tráfico chico (la mayoría de los admin/ERP internos) el A/B no
  converge — usá el test de usabilidad barato de Krug (3-5 usuarios) + métricas before/after
  de §A en su lugar. Un A/B sin poder estadístico es teatro.
- **Un cambio por test**: si la variante cambia 5 cosas, ganar no te dice qué funcionó (y el
  siguiente "rediseño total" no podrá reusar el aprendizaje).

## F. En el reporte
Cerrá la auditoría con: métrica elegida por tarea crítica + baseline (si existe) + qué evento
falta instrumentar. En la wave siguiente, abrí con el delta. Regla: **un fix sistémico [H] debe
mover una métrica de §A**; si no hay forma de que la mueva, cuestioná su prioridad.

## Fuentes
- HEART — https://research.google/pubs/measuring-the-user-experience-on-a-large-scale-user-centered-metrics-for-web-applications/
- SUS — https://measuringu.com/sus/ · SEQ — https://measuringu.com/seq10/
- NN/g — usability metrics — https://www.nngroup.com/articles/usability-metrics/
- Core Web Vitals — https://web.dev/articles/vitals · INP — https://web.dev/articles/inp
- Trustworthy A/B — Kohavi, *Trustworthy Online Controlled Experiments* (el libro de la disciplina)
