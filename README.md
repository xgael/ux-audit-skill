# ux-audit — Claude Code skill

Auditoría UX + **hardening** (endurecimiento) de un producto ya construido, de forma
sistémica. La idea central: **arreglar el patrón, no la pantalla** — un fallo de UX casi
siempre se repite en N pantallas; se corrige una vez (primitiva/componente compartido) y
se cablea en todas.

No es para diseñar UI nueva desde cero (para eso hay skills de diseño). Es para **auditar
y remediar** la UX de algo que ya existe.

## Qué hace

Un método de 3 fases:

1. **Auditoría** — evaluación heurística por módulo (fan-out de subagentes por área),
   contra Nielsen + Apple HIG + Material + GOV.UK + WCAG. Hallazgos priorizados
   `[H/M/L] problema — file:línea — fix`.
2. **Remediación** — arreglar por **patrón sistémico** con primitivas compartidas
   (undo real, estados loading/error+reintentar/vacío, pop-up-safe, formatters únicos,
   tokens semánticos, ⌘K, match al modelo mental…).
3. **Verificación** — build/lint/test + ver en la app (sobre todo cambios visuales).

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
