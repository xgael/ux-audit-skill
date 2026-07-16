# Templates operativos — prompts de fan-out y reporte

Plantillas verbatim para que cada corrida sea consistente. Rellená `{…}` y no improvises la
estructura: la deriva entre subagentes hace incomparables los hallazgos.

## A. Prompt para subagente auditor (Fase 1, uno por ÁREA)

```
Auditá la UX del área "{ÁREA}" de {producto}. Solo AUDITORÍA — no edites nada.

Páginas/rutas: {lista de rutas con sus archivos page/client}
Design system: {tokens/toasts/componentes compartidos y dónde viven}
Contexto: {stack, idioma del producto, usuarios objetivo}

Para CADA pantalla evaluá contra estas dimensiones:
1. Inputs y fricción (validación por tipo, límites reales, forgiving formats)
2. Flujo/pasos (excise, re-tecleo, handoffs)
3. Estados: ideal/loading/vacío-primerizo/vaciado/sin-resultados/error/parcial
   — el error NUNCA puede verse como vacío; skeleton en cargas
4. Feedback (ninguna acción silenciosa; undo en destructivas)
5. Descubribilidad (auto-evidente, trigger words, clickabilidad)
6. Consistencia (formatters únicos, tokens, un término = un concepto)
7. Responsive/a11y (focus visible, labels, contraste, targets, scroll-x)
8. Copy (botones verbo+objeto, errores qué+por qué+cómo, empty con CTA)
{si hay tablas: 9. Tabla enterprise (filtros con chips, bulk con alcance,
paginación con estado preservado, columnas por tarea)}
{si hay IA: 10. AI-UX (streaming/cancelar, output editable, acciones deshacibles,
fuentes navegables)}

Formato de salida EXACTO, rankeado por severidad, sin adulación ni scope creep:
### {pantalla} ({ruta})
- [H|M|L] {problema concreto} — `{archivo}:{línea}` — fix: {arreglo concreto}

Severidad: H = rompe/bloquea tarea o pierde datos o dark pattern;
M = fricción real recurrente; L = pulido.
Al final: "## TOP 3 {ÁREA}" con los 3 de mayor impacto×frecuencia
y "## PATRONES" con lo que se repite en ≥2 pantallas (candidatos a primitiva).
```

## B. Prompt para subagente de wiring (Fase 2, mecánico)

```
Cableá el patrón "{PRIMITIVA}" en el área "{ÁREA}". Cambios CHICOS y mecánicos.

Referencia ya arreglada: {archivo de la página patrón — imitala}
Primitiva/helper: {ruta del componente/hook compartido y su API}
Páginas a cablear: {lista}

Reglas: no reescribas lógica que funciona; respetá tokens/convenciones del proyecto;
un edit por página; si una página no encaja en el patrón, reportala en vez de forzarla.
Al terminar: corré {typecheck/build} y reportá qué páginas quedaron y cuáles no con motivo.
```

## C. Esqueleto del reporte de auditoría

```markdown
# Auditoría UX — {producto} — wave {N} — {fecha}

## Alcance
{módulos auditados} · {tareas críticas recorridas} · checks automáticos: {sí/no, script}

## Hallazgos sistémicos (fix-once → arregla-N)   ← LA sección que importa
1. [H] {patrón} — aparece en {N} pantallas ({lista}) — primitiva: {qué construir}
...

## Hallazgos por módulo
### {módulo}
- [H|M|L] {problema} — `{archivo}:{línea}` — fix: {...}

## Journey de tareas críticas
{tabla acciones/dudas/emoción/evidencia por tarea — solo si cruza ≥3 pantallas}

## Plan de remediación
| Orden | Qué | Tipo | Esfuerzo | Arregla |
|---|---|---|---|---|
| 1 | {primitiva} | sistémico-H | {S/M/L} | {N pantallas} |
| 2 | {quick win} | puntual-H | trivial | {1} |

## Métricas (metrics.md)
{tarea crítica} → {métrica} → baseline: {valor o "sin instrumentar — agregar evento X"}

## Scorecard
| Módulo | Estados | Feedback | Forms | A11y | Copy | Tabla | AI |
|---|---|---|---|---|---|---|---|
| {mod} | ✅/🟡/❌ | ... |
```

## D. Reglas de uso
- El scorecard de la wave N es el INPUT de la wave N+1 (abrí con el delta).
- Los TOP 3 por área + los sistémicos van al usuario como resumen ejecutivo; el detalle queda
  en el reporte.
- Si un subagente devuelve hallazgos sin `archivo:línea` o sin fix concreto, pedile la
  corrección antes de sintetizar — hallazgo no accionable = ruido.
