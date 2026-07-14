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
  accionables + un **"Playbook de claridad"** de 10 movimientos para que el usuario procese
  información limpia y clara. Consultá esto cuando el problema sea de densidad/organización/legibilidad.

## Cuándo usarla vs no
- **Sí**: producto/módulos existentes con UX inconsistente o cruda; "revisa y mejora la UX".
- **No** (usa otra): diseñar UI/estilo/paleta desde cero → `ui-ux-pro-max`; gráficas → `dataviz`;
  diseño de un artifact → `artifact-design`; checklist de calidad React/TSX → `vercel:react-best-practices`.

## El método — 3 fases

### Fase 1 — Auditoría (evaluación heurística, por módulo)
1. Mapea el alcance: lista rutas/páginas/módulos (`router`, `pages/`, sidebar dinámico).
2. **Fan-out**: un subagente por ÁREA (agrupa módulos afines). Cada uno lee las páginas + su
   client/estilos y evalúa contra el checklist de `references/heuristics.md`. Devuelve hallazgos
   `[H/M/L] problema — file:line — fix concreto`, rankeados. (Correr en background y sintetizar.)
   - Si un subagente muere con 0 tool-uses o texto basura, **relánzalo** — es un fallo transitorio.
3. Evalúa cada pantalla contra las dimensiones: (1) inputs/fricción, (2) flujo/pasos, (3) cobertura
   de estados (loading/vacío/error/éxito), (4) feedback, (5) descubribilidad, (6) consistencia con
   el design system, (7) responsive/a11y.
4. Sintetiza: **agrupa por PATRÓN sistémico** (qué se repite en N pantallas) antes que por pantalla.
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
2. **Fan-out por área** para el wiring (mecánico). Dale a cada subagente un recipe explícito + una
   página ya arreglada como referencia. Mantén los edits chicos (los agentes se cuelgan en rewrites
   grandes). Modelo rápido (fable) sirve para el wiring mecánico.
3. Respeta el design system existente (tokens, toasts, convenciones). Precedencia: palabras del
   usuario → sistema del proyecto → tus decisiones.

### Fase 3 — Verificación
- Tras CADA lote: typecheck/build + lint. Al final: tests (unit + e2e si hay) y, para cambios
  visuales, **levantar la app y verlo** (no solo compilar).
- Commit por lote con mensaje que explique el patrón arreglado. Rama → PR → merge si el usuario lo pide.

## Severidad
- **H**: rompe/confunde/bloquea la tarea, o pérdida de datos (undo falso, error disfrazado de vacío,
  checkout que el popup-blocker mata).
- **M**: fricción real (sin filtros en lista larga, formato inconsistente, sin recordar en vez de recordar).
- **L**: pulido (hex vs token, i18n, aria-live).

## Formato de salida (auditoría)
```
### <pantalla> (ruta)
- [H/M/L] <hallazgo> — `archivo:línea` — fix: <arreglo concreto>
```
Al final: `## TOP 3 <área>` con los de mayor impacto. Sin adulaciones, sin scope creep, solo señal accionable.

## Anti-patrones (qué NO hacer)
- Arreglar pantalla por pantalla lo que es un patrón transversal.
- Marcar "listo" sin verificar en la app corriendo (sobre todo cambios visuales).
- Reescribir lógica que funciona por estética; el hardening es de UX, no refactor gratis.
- Romper el design system con estilos/hex ad-hoc.
