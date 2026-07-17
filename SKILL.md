---
name: ux-audit
description: >-
  Auditoría UX + endurecimiento (hardening) de un producto existente. Úsala para
  "auditar UX", "mejorar/revisar la UX", "usability audit", "heuristic evaluation",
  "arreglar la UX de estos módulos", "UX pass", "endurecer la experiencia". Corre una
  evaluación heurística por módulo (Nielsen + Apple HIG + Material + GOV.UK + WCAG),
  prioriza hallazgos, y los arregla por PATRÓN sistémico (primitivas compartidas) en
  vez de pantalla por pantalla. NO es para diseñar UI nueva desde cero (para eso está
  ui-ux-pro-max) — es para auditar y remediar UX de algo ya construido.
---

# UX Audit + Hardening

Método probado para revisar y mejorar la UX de un producto ya construido, de forma
sistémica. La idea central: **arreglar el patrón, no la pantalla** — un fallo de UX casi
siempre se repite en N pantallas; se corrige una vez (primitiva/componente compartido) y
se cablea en todas.

Referencias (cargalas al auditar/arreglar):
- **`references/heuristics.md`** — heurísticas Nielsen (verbatim), principios de compañías top
  (Apple HIG, Material, GOV.UK, Polaris, Carbon, Fluent), Laws of UX, WCAG, tabla de fixes canónicos,
  checklist de ~20 ítems verificables, y fuentes.
- **`references/books.md`** — canon de libros de diseño (Norman, Krug, Tufte, Few, Wurman/LATCH, Ware,
  Kahneman, Weinschenk, Lidwell, Bringhurst, Cooper, Wroblewski, Alexander…) con sus principios
  accionables + una sección de **diseño conductual / Psychology of UX** (§E: Fogg B=MAP, Hooked,
  nudge/EAST, Cialdini, sesgos y **deceptive/dark patterns** con su ética) + un **"Playbook de
  claridad"**. Consultá esto cuando el problema sea de densidad/organización/legibilidad, o de
  **motivación/comportamiento** (onboarding, retención, conversión, consentimiento).
- **`references/writing.md`** — UX writing: microcopy, botones/CTAs, el canon de **mensajes de
  error**, empty states con CTA, copy de formularios. Consultá esto al tocar CUALQUIER texto visible.
- **`references/patterns.md`** — mecánica de patrones: **matriz de ~7 estados** por vista,
  **performance percibida** (reglas por duración: skeleton vs spinner vs progreso), **formularios
  grado Baymard** (timing de validación, autocomplete, error summary) y **mobile/touch** (thumb
  zone, targets, gestos, hover).
- **`references/audit-scripts.md`** — checks **automatizables** (Playwright + axe): contraste,
  focus, targets, scroll-x, autocomplete, consola. Correlo ANTES del fan-out de la Fase 1.
- **`references/flows.md`** — el eje temporal: **onboarding** (aha moment/activation, learn-by-doing,
  tours ≤4 pasos, checklist con endowed progress), **microinteracciones** (Saffer:
  trigger→rules→feedback→loops&modes, duraciones), y **journey mapping** de auditoría (tabla por
  tarea crítica, peak-end, fricción acumulada, handoffs). Consultalo para el cognitive walkthrough
  y todo lo que cruce pantallas o pase en el tiempo.
- **`references/admin-tables.md`** — **tablas enterprise** (filtros con chips, bulk con alcance,
  inline edit, paginación vs infinite scroll, estado preservado, columnas) y patrones de
  **admin-app** (disabled-vs-hidden por permisos, dirty state, audit trail). Para ERPs/back-office.
- **`references/ai-ux.md`** — features con **IA/LLM**: streaming y esperas, incertidumbre/confianza,
  human-in-the-loop (output editable, acciones de agente deshacibles), affordances de prompt +
  checklist AI-UX.
- **`references/metrics.md`** — probar que el fix **mejoró**: task success/time/errores, HEART,
  SUS/SEQ, instrumentación mínima, **Core Web Vitals** (LCP/INP/CLS como UX) y disciplina de
  **A/B** (guardrails, novelty, cuándo NO testear). Usalo en Fase 3 y para abrir la wave
  siguiente con el delta.
- **`references/templates.md`** — prompts verbatim para subagentes (auditor y wiring) + esqueleto
  del reporte con plan de remediación y scorecard. Usalos tal cual — la deriva entre corridas
  hace incomparables los hallazgos.
- **`references/architecture.md`** — los 2 escalamientos: **IA/navegación** (inventario de rutas,
  huérfanas, LATCH, amplitud vs profundidad, reestructurar sin romper memoria muscular) y
  **remake [R]** (criterios para declarar que un objeto no se parcha — se rehace: densidad de
  [H], mismatch estructural, costo, deuda de origen — y el proceso de demolición segura con
  inventario de paridad).
- **`references/resilience.md`** — UX cuando el mundo falla: **offline/red intermitente** (cola,
  retry con backoff), **idempotencia** (doble click ≠ doble cobro), **sesión que expira sin
  perder trabajo**, security UX (password managers, OTP, passkeys) y **permisos en contexto**
  con primer. Checklist de 7 pruebas rompibles.
- **`references/craft.md`** — detectar amateurismo visual y corregirlo con tokens/escalas
  (Refactoring UI): espaciado 4/8, jerarquía por peso+color, grises con sistema, elevación,
  alineación, motion con física. Checklist de 8 ítems; los fixes son de ESCALA, no de pantalla.
- **`references/platform.md`** — **notificaciones** con presupuesto de interrupción (batching,
  granularidad, badge honesto), **settings** (el mejor es el que no existe), **URL-as-state**
  (toda vista compartible/bookmarkeable) y **convenciones nativas** iOS/Android/desktop.
- **`references/efficiency.md`** — bajar el **interaction cost**: conteo GOMS/KLM por tarea
  (pasos/saltos/re-tecleos/ping-pong), los 7 desperdicios Lean-UX, **consolidación de vistas**
  (cuándo 3 pantallas deben ser 1: master-detail, workspace de tarea, wizard→form) y su
  contrapeso anti-mega-pantalla, reorden de bloques que sigue al flujo, detectores de
  redundancia (la hoja de cálculo al lado = el requerimiento).

## Cuándo usarla vs no
- **Sí**: producto/módulos existentes con UX inconsistente o cruda; "revisa y mejora la UX".
- **No** (usa otra): diseñar UI/estilo/paleta desde cero → `ui-ux-pro-max`; gráficas → `dataviz`;
  diseño de un artifact → `artifact-design`; checklist de calidad React/TSX → `vercel:react-best-practices`.

## El método — 3 fases

### Fase 1 — Auditoría (heurística por módulo + walkthrough por tarea)
1. Mapea el alcance: lista rutas/páginas/módulos (`router`, `pages/`, sidebar dinámico) **y las
   2-3 tareas críticas del usuario** (las que definen el producto: crear X, cobrar, publicar…).
   Con el inventario en mano, pasá los checks de IA de `references/architecture.md` §A
   (huérfanas, LATCH, clicks-a-tarea-crítica, etiquetas) — un fallo de ESTRUCTURA condiciona
   toda la auditoría posterior y se reporta como hallazgo estructural propio.
2. **Checks automáticos primero** (si la app corre): ejecutá el script de
   `references/audit-scripts.md` — contraste/focus/targets/scroll-x/autocomplete salen gratis y
   con evidencia; los agentes no gastan juicio en lo medible.
3. **Fan-out**: un subagente por ÁREA (agrupa módulos afines), con el **prompt template A de
   `references/templates.md`** rellenado (rutas, design system, dimensiones extra si hay
   tablas/IA). Cada uno lee las páginas + su client/estilos y evalúa contra el checklist de
   `references/heuristics.md`. Devuelve hallazgos `[H/M/L] problema — file:line — fix concreto`,
   rankeados. (Correr en background y sintetizar.)
   - Si un subagente muere con 0 tool-uses o texto basura, **relánzalo** — es un fallo transitorio.
4. **Cognitive walkthrough de las tareas críticas** (cross-pantalla, complementa lo anterior):
   simulá un usuario PRIMERIZO recorriendo cada tarea de punta a punta y en cada paso preguntá:
   (a) ¿sabría que este es el paso correcto hacia su objetivo? (b) ¿VE el control para hacerlo?
   (c) ¿el label conecta con su intención (trigger words)? (d) ¿el feedback le confirma que
   avanzó? Un fallo aquí suele ser [H] aunque cada pantalla individual "cumpla" el checklist —
   la heurística audita pantallas, el walkthrough audita el CAMINO. Mientras recorrés, **contá
   el costo** (`references/efficiency.md` §A): pasos, saltos de vista, re-tecleos, ping-pong —
   el conteo convierte "se siente pesado" en un hallazgo defendible y su fix suele ser
   consolidación/reorden (§B-C). Si la tarea cruza ≥3 pantallas o ≥2 contextos
   (web→email→web), formalizalo con la tabla de journey de `references/flows.md` §C (peak-end,
   fricción acumulada, handoffs). Si el producto tiene signup/primer uso, auditá el onboarding
   contra `references/flows.md` §A.
5. Evalúa cada pantalla contra las dimensiones: (1) inputs/fricción, (2) flujo/pasos, (3) cobertura
   de estados (matriz de ~7 en `references/patterns.md` §A), (4) feedback, (5) descubribilidad,
   (6) consistencia con el design system, (7) responsive/a11y, (8) copy (`references/writing.md` §7).
6. Sintetiza: **agrupa por PATRÓN sistémico** (qué se repite en N pantallas) antes que por pantalla.
   Prioriza los patrones sistémicos — arreglar uno arregla docenas.

### Fase 2 — Remediación (hardening)
1. **Primero las primitivas** (fix-once → arregla-N). Construye/repara los componentes y helpers
   compartidos; luego cablea las páginas. Primitivas canónicas (ver `references/heuristics.md` §Fixes):
   - **Undo real** en toda acción destructiva (soft-delete + endpoint `restaurar`, `onUndo` que
     realmente restaura). Donde el undo sea imposible (valor enmascarado, hard-delete), usa confirm.
   - **Estados explícitos** loading / error+Reintentar / vacío (hook `useAsyncData` + `<AsyncBoundary>`).
     El error NUNCA debe verse igual que "sin datos".
   - **Pop-up-safe**: abrir pestañas (checkout, PDF, descargas) SÍNCRONO dentro del gesto del click
     (`openResolvedUrl`), no tras un `await`.
   - **Formato único** (moneda/fecha) — un helper, no `toLocaleString` ad-hoc.
   - **Tokens semánticos** danger/success/warning (soft + strong) con override dark; nada de hex
     hardcodeado que rompa dark mode.
   - **Command palette ⌘K** en vez de un buscador decorativo.
   - **Match al modelo mental**: dato temporal → calendario (no lista); jerárquico → árbol; etc.
   - **Inputs restringidos y validados por tipo** (prevención de error, no solo mensaje): cada
     campo acota su entrada al dominio que corresponde y NO acepta basura —
     teléfono sólo dígitos/`+`/espacios/guiones (`inputMode="tel"` + filtrar/patrón), nombre sin
     `@ * / <>` ni dígitos fuera de lugar, email `type="email"` + validación, número
     `inputMode="numeric"` con `min`/`max` reales, monto ≥0 y decimales controlados, %
     `0<v≤100`. **Los límites deben ENFORZARSE de verdad**: `max`/`min` de un `<input number>`
     son advisory (dejan teclear fuera de rango) y `maxLength` no aplica a `type=number` → validar
     en el submit y/o `maxLength` en text; el clásico "pide máx 8 pero deja poner más" es esto.
     Feedback inline (`aria-invalid` + mensaje) y submit deshabilitado si es inválido. Matiz
     **forgiving format** (Postel): aceptá variaciones razonables y normalizá (teléfono con o sin
     guiones), no rechaces por un espacio. Reusá el helper de validación del proyecto
     (`lib/validation.ts`: `validateField`/`filterInput`) si existe. Nota: esto es UX (prevención);
     NO reemplaza la validación de servidor (seguridad).
   - **Crear/editar en modal, no formulario inline**: el botón "Nuevo/Crear" abre un modal
     (o side sheet) con el formulario — NO despliega un form debajo que empuja la lista y
     mezcla crear con explorar. El modal enfoca la tarea, preserva el contexto de la lista y
     da límite claro de guardar/cancelar. Reutilizá el modal compartido (con focus-trap,
     `aria-modal`, Esc y restaurar foco). Matiz: para 1 campo trivial (inline edit) o flujos
     multi-paso (página propia) el modal puede no ser lo ideal.
2. **Fan-out por área** para el wiring (mecánico), con el **prompt template B de
   `references/templates.md`**: recipe explícito + una página ya arreglada como referencia.
   Mantén los edits chicos (los agentes se cuelgan en rewrites grandes). Modelo rápido (fable)
   sirve para el wiring mecánico.
3. Respeta el design system existente (tokens, toasts, convenciones). Precedencia: palabras del
   usuario → sistema del proyecto → tus decisiones.

### Fase 3 — Verificación
- Tras CADA lote: typecheck/build + lint. Al final: tests (unit + e2e si hay) y, para cambios
  visuales, **levantar la app y verlo** (no solo compilar).
- **Impacto medible** (`references/metrics.md`): por tarea crítica, dejá métrica elegida +
  baseline (o el evento que falta instrumentar). Regla: un fix sistémico [H] debe poder mover
  task-success/tiempo/errores — si no, cuestioná su prioridad. La wave siguiente abre con el delta.
- Commit por lote con mensaje que explique el patrón arreglado. Rama → PR → merge si el usuario lo pide.

## Severidad y priorización
Antes de asignar severidad, diagnosticá la **altitud** del hallazgo (Garrett, 5 planos —
books §B): ¿surface (craft), skeleton (layout/patterns), structure (IA/interacción), scope
(feature sobra/falta) o strategy? El plano dicta el fix — pulir un botón no arregla una vista
que no debería existir; N hallazgos de surface en el mismo objeto suelen ser UNO de structure
(→ candidato [R]).

La severidad no es a ojo — es **impacto × frecuencia × persistencia**:
- **Impacto**: ¿bloquea/rompe la tarea o pierde datos, o solo estorba?
- **Frecuencia**: ¿lo sufre todo usuario en el flujo principal, o un caso raro?
- **Persistencia**: ¿molesta CADA vez, o se aprende a esquivar tras la primera?

- **H**: rompe/confunde/bloquea la tarea, o pérdida de datos (undo falso, error disfrazado de vacío,
  checkout que el popup-blocker mata, dark pattern). Alto impacto aunque la frecuencia sea media.
- **M**: fricción real y recurrente (sin filtros en lista larga, formato inconsistente, validación
  que castiga, empty state muerto).
- **L**: pulido (hex vs token, i18n, aria-live) o de baja frecuencia/persistencia.
- **[R] remake**: el objeto entero incumple — parcharlo es tirar esfuerzo. Se declara SOLO con
  ≥2 criterios de `references/architecture.md` §B (densidad de [H], mismatch estructural, costo
  parche ≥ rehacer, deuda de origen) y lleva spec + inventario de paridad. Nunca por estética.

**Orden de remediación** (cuadrante impacto × esfuerzo):
1. Sistémico + H (una primitiva arregla N pantallas graves) — siempre primero.
2. **Quick wins**: H/M puntuales de esfuerzo trivial (un label, un autocomplete) — intercalalos,
   dan momentum y goodwill.
3. **Remakes [R]** — después de las primitivas (el objeto nuevo las consume, no las duplica);
   proceso completo en `references/architecture.md` §B: paridad → contrato intacto → spec →
   construir → verificar paridad → switch y borrar el viejo.
4. Sistémico + M. 5. Puntual + M. Los L al final o como byproduct del wiring.

**Scorecard entre waves** (auditorías por etapas): mantené una tabla módulo × dimensión
(estados/feedback/forms/a11y/copy: ✅/🟡/❌) al final del reporte — la próxima wave arranca de ahí
y se ve el progreso.

## Formato de salida (auditoría)
```
### <pantalla> (ruta)
- [H/M/L] <hallazgo> — `archivo:línea` — fix: <arreglo concreto>
```
Al final: `## TOP 3 <área>` con los de mayor impacto. Sin adulaciones, sin scope creep, solo señal
accionable. Para el reporte completo de una wave usá el esqueleto C de `references/templates.md`
(sistémicos → por módulo → journey → plan → métricas → scorecard).

## Anti-patrones (qué NO hacer)
- Arreglar pantalla por pantalla lo que es un patrón transversal.
- Marcar "listo" sin verificar en la app corriendo (sobre todo cambios visuales).
- Reescribir lógica que funciona por estética; el hardening es de UX, no refactor gratis.
  (La excepción disciplinada es el **[R] remake** con sus criterios y proceso —
  `references/architecture.md` §B; sin criterios cumplidos, es este anti-patrón.)
- Romper el design system con estilos/hex ad-hoc.
- Reestructurar navegación/rutas sin redirects ni alias — rompe la memoria muscular y los
  bookmarks (architecture.md §A).
