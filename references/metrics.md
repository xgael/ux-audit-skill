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

## D. En el reporte
Cerrá la auditoría con: métrica elegida por tarea crítica + baseline (si existe) + qué evento
falta instrumentar. En la wave siguiente, abrí con el delta. Regla: **un fix sistémico [H] debe
mover una métrica de §A**; si no hay forma de que la mueva, cuestioná su prioridad.

## Fuentes
- HEART — https://research.google/pubs/measuring-the-user-experience-on-a-large-scale-user-centered-metrics-for-web-applications/
- SUS — https://measuringu.com/sus/ · SEQ — https://measuringu.com/seq10/
- NN/g — usability metrics — https://www.nngroup.com/articles/usability-metrics/
