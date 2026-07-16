# ux-audit — Claude Code skill

Auditoría UX + **hardening** (endurecimiento) de un producto ya construido, de forma
sistémica. La idea central: **arreglar el patrón, no la pantalla** — un fallo de UX casi
siempre se repite en N pantallas; se corrige una vez (primitiva/componente compartido) y
se cablea en todas.

No es para diseñar UI nueva desde cero (para eso hay skills de diseño). Es para **auditar
y remediar** la UX de algo que ya existe.

## Qué hace

Un método de 3 fases:

1. **Auditoría** — checks automáticos (Playwright + axe) + evaluación heurística por módulo
   (fan-out de subagentes por área) contra Nielsen + Apple HIG + Material + GOV.UK + WCAG,
   + **cognitive walkthrough** de las tareas críticas (audita el camino, no solo las
   pantallas). Hallazgos priorizados `[H/M/L] problema — file:línea — fix` con severidad
   **impacto × frecuencia × persistencia**.
2. **Remediación** — arreglar por **patrón sistémico** con primitivas compartidas
   (undo real, estados loading/error+reintentar/vacío, pop-up-safe, formatters únicos,
   tokens semánticos, ⌘K, match al modelo mental…), en orden sistémico-H → quick wins → M.
3. **Verificación** — build/lint/test + ver en la app (sobre todo cambios visuales) +
   scorecard por módulo entre waves.

## Qué incluye

- **`SKILL.md`** — el método, cuándo usarla, severidad, formato de salida, anti-patrones.
- **`references/heuristics.md`** — heurísticas de Nielsen (verbatim), principios de
  compañías top (Apple HIG, Material 3, GOV.UK, Shopify Polaris, IBM Carbon, Microsoft
  Fluent), Laws of UX, WCAG (POUR + criterios clave), tabla de fixes canónicos, checklist
  de ~30 ítems verificables en la app (incluye chequeos de Krug —trunk test, clickabilidad,
  trigger words— y de ética conductual —anti dark patterns—), y fuentes.
- **`references/books.md`** — canon de libros de diseño (Norman, **Krug** —sección
  completa: 3 leyes, cómo la gente escanea, trunk test, reservoir of goodwill, testing
  barato—, Tufte, Few, Wurman/LATCH, Ware, Kahneman, Weinschenk, Lidwell, Bringhurst,
  Cooper, Wroblewski, Tidwell, Alexander…) con principios accionables, una sección de
  **diseño conductual / Psychology of UX** (§E: Fogg B=MAP, Hooked, nudge/EAST, Cialdini,
  sesgos cognitivos y **deceptive/dark patterns** con su ética) y un **"Playbook de
  claridad"**.
- **`references/writing.md`** — UX writing: microcopy, botones/CTAs, canon de mensajes de
  error (qué+por qué+cómo seguir, sin culpa), empty states con CTA, copy de formularios.
- **`references/patterns.md`** — mecánica de patrones: matriz de ~7 estados por vista,
  performance percibida (skeleton vs spinner vs progreso por duración, optimistic UI),
  formularios grado Baymard (timing de validación, `autocomplete`, error summary) y
  mobile/touch (thumb zone, targets 44pt, gestos, hover).
- **`references/audit-scripts.md`** — checks automatizables con Playwright + axe-core
  (contraste, focus visible, targets, scroll-x, placeholder-como-label, autocomplete,
  errores de consola) + tabla de qué es automatizable y qué requiere juicio.
- **`references/flows.md`** — el eje temporal de la UX: onboarding (aha moment/activation,
  learn-by-doing, tours ≤4 pasos, checklist con endowed progress), microinteracciones
  (Saffer: trigger→rules→feedback→loops&modes) y journey mapping de auditoría (tabla por
  tarea crítica, peak-end, fricción acumulada, handoffs entre contextos).
- **`references/admin-tables.md`** — tablas enterprise (filtros con chips, bulk actions con
  alcance, inline edit, paginación con estado preservado, columnas gestionables) y patrones
  de admin-app (disabled-vs-hidden por permisos, dirty state, audit trail).
- **`references/ai-ux.md`** — UX de features con IA/LLM: streaming y esperas, incertidumbre
  y confianza, human-in-the-loop (output editable, acciones de agente deshacibles con log),
  affordances de prompt, checklist AI-UX auditable.
- **`references/metrics.md`** — probar que el fix mejoró: task success/tiempo/errores,
  HEART, SUS/SEQ, instrumentación mínima y test barato de Krug antes/después.
- **`references/templates.md`** — prompts verbatim para los subagentes (auditor y wiring) y
  esqueleto del reporte de wave (sistémicos, plan de remediación, métricas, scorecard).
- **`references/architecture.md`** — arquitectura de información (inventario de rutas,
  huérfanas, LATCH, amplitud vs profundidad, reestructurar sin romper memoria muscular) y
  el **remake [R]**: criterios disciplinados para declarar que un objeto no se parcha — se
  rehace — y el proceso de demolición segura (inventario de paridad, contrato intacto, spec,
  verificación tarea por tarea, switch).
- **`references/resilience.md`** — UX cuando el mundo falla: offline/red intermitente,
  idempotencia (doble click ≠ doble cobro), sesión que expira sin perder trabajo, security
  UX (password managers, OTP, passkeys) y permisos del sistema pedidos en contexto.
- **`references/craft.md`** — craft visual auditable (canon: Refactoring UI): escala de
  espaciado 4/8, jerarquía por peso+color, grises con sistema, elevación y z-index nombrado,
  alineación óptica, motion con física — fixes de escala/token, no de pantalla.
- **`references/platform.md`** — notificaciones con presupuesto de interrupción, settings
  ("el mejor setting es el que no existe"), deep links y URL-as-state, convenciones nativas
  iOS/Android/desktop.
- **`references/efficiency.md`** — eficiencia de flujo: interaction cost con conteo GOMS/KLM
  por tarea (pasos, saltos de vista, re-tecleos, ping-pong), los 7 desperdicios Lean-UX,
  consolidación de vistas (master-detail, workspace de tarea, wizard→form — y cuándo NO),
  reorden de bloques que sigue al flujo de trabajo y detectores de redundancia.

Además, `references/metrics.md` incluye Core Web Vitals (LCP/INP/CLS como métricas de UX) y
disciplina de A/B testing (guardrails, novelty effect, cuándo NO testear), y
`references/books.md` cierra con un mapa de **fuentes maestras** para seguir nutriendo la
skill (Baymard, growth.design, Mobbin, WWDC, GOV.UK Service Manual, PAIR…).

## Instalar

Como skill personal (disponible en todos tus proyectos):

```bash
git clone https://github.com/xgael/ux-audit-skill ~/.claude/skills/ux-audit
```

O como skill de proyecto (solo un repo):

```bash
git clone https://github.com/xgael/ux-audit-skill .claude/skills/ux-audit
```

Reiniciá Claude Code y aparece como `/ux-audit`.

## Usar

```
/ux-audit
```

o en lenguaje natural: "audita y mejora la UX de estos módulos", "usability audit",
"endurece la experiencia".

## Fuentes

Heurísticas y principios tomados de sus documentos canónicos (NN/g, Apple HIG, Material 3,
GOV.UK, Shopify Polaris, IBM Carbon, Microsoft Fluent, W3C WCAG, Laws of UX), del canon de
libros de diseño y de la rama de diseño conductual (Fogg Behavior Model, Cialdini, Nudge de
Thaler/Sunstein, deceptive patterns de Brignull) citados en `references/books.md`. Las URLs
están al pie de cada referencia.

## Licencia

MIT.
