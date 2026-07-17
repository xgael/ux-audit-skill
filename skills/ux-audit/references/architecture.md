# Arquitectura de información y remakes — cuando parchar no alcanza

Dos escalamientos por encima del fix de patrón: (A) la **estructura** del producto está mal
(navegación, jerarquía, organización de módulos) y (B) un **objeto** (pantalla/módulo/flujo)
incumple tanto que parcharlo es tirar esfuerzo — se declara **[R] remake**.

---

## A. Auditoría de arquitectura de información (IA)

El trunk test (Krug) audita si me oriento EN una página; esto audita si el mapa mismo está bien.
Los 4 sistemas del polar bear (Rosenfeld/Morville): **organización, etiquetado, navegación,
búsqueda**.

**Inventario primero** (mecánico, hazlo con el router):
- Lista completa de rutas + profundidad + desde dónde se llega a cada una.
- **Huérfanas**: pantallas sin entrada desde la nav (solo por URL/link enterrado) — o se
  integran o se matan.
- **Duplicadas**: dos caminos que hacen lo mismo con nombre distinto (rompe "un término = un
  concepto").

**Checks de estructura**:
1. **¿Organizada por LATCH correcto?** (Wurman): la agrupación matchea cómo el usuario BUSCA —
   temporal→Time, catálogo→Category, jerárquico→Hierarchy. Módulos agrupados por cómo está
   construido el sistema (por tabla de DB, por equipo que lo hizo) en vez de por tarea del
   usuario = hallazgo estructural clásico.
2. **Amplitud vs profundidad**: nav plana de 15 ítems sin agrupar (viola Miller/chunking) o
   jerarquía de 4 niveles para llegar a lo cotidiano (viola eficiencia). Regla: lo FRECUENTE a
   ≤2 clicks; lo raro puede vivir hondo. Medí clicks-a-tarea-crítica.
3. **Etiquetas en idioma del usuario**: nombres de secciones = trigger words del dominio
   ("Facturas", no "Gestión documental"). Test barato: tapá los iconos, ¿los labels solos
   predicen qué hay adentro?
4. **La nav refleja prioridad**: lo más usado primero/más visible; lo admin/config al final.
   Orden alfabético en nav principal = nadie decidió.
5. **Un lugar para cada cosa**: si una feature vive en 2 menús o un dato se edita en 2
   pantallas con reglas distintas → decidir el canónico y redirigir el resto.

**Reestructurar sin romper** (la nav es memoria muscular — Krug: "mess with the site map, mess
with their recall"):
- Cambios de ruta con **redirects** (viejas URLs siguen funcionando — bookmarks, historial).
- Renombres de sección: mantené el término viejo como alias en búsqueda/⌘K una temporada.
- Movimientos grandes de a UNO por wave, no big-bang de toda la nav; anunciá el cambio in-app
  la primera vez ("Facturas ahora vive en Finanzas").
- Actualizá TODOS los caminos al mover (breadcrumbs, links internos, docs, empty states que
  apuntan ahí).

---

## B. Remake — cuándo y cómo demoler un objeto

### Criterio [R] — se declara remake cuando se cumplen ≥2:
1. **Densidad de fallo**: ≥4-5 hallazgos [H] concentrados en el mismo objeto (la pantalla es el
   patrón).
2. **Mismatch estructural**: la representación entera contradice el modelo mental (lista plana
   para datos de calendario; wizard de 6 pasos para una acción de 1; tabla para algo que es un
   canvas) — el fix no es un parche, es OTRO componente.
3. **Costo invertido**: parchar los hallazgos cuesta ≥ que rehacer la vista sobre las
   primitivas ya construidas.
4. **Deuda de origen**: el objeto quedó de una época pre-design-system y cada parche lo aleja
   más de la consistencia (mantener el Franken-objeto cuesta cada wave).

Si solo aplica 1 criterio → sigue siendo patch por patrón. El remake es la excepción
justificada, no la vía rápida. **Anti-patrón**: declarar [R] por estética ("no me gusta cómo
se ve") — remake se justifica por TAREA rota, no por gusto.

### Proceso de remake (dentro de la skill, NO es rediseño de marca)
Límite con `ui-ux-pro-max`: acá NO se inventa lenguaje visual nuevo — se **reconstruye el
objeto con el design system existente** y las primitivas de la Fase 2. Si el proyecto no tiene
design system o el usuario quiere nueva identidad → eso es ui-ux-pro-max, no esto.

1. **Inventario de paridad** (del objeto viejo): qué TAREAS resuelve hoy (no qué features
   tiene — un remake es la oportunidad de matar excise, no de clonarlo). Lista: tareas + datos
   que muestra + acciones + permisos + estados. Confirmá con el usuario qué se conserva, qué
   se mata y qué faltaba.
2. **Contrato intacto**: API/datos/backend NO se tocan en el mismo movimiento (un cambio a la
   vez — si el backend también está mal, wave aparte). El remake es de la capa de vista/flujo.
3. **Spec de 10 líneas antes de codear**: modelo mental elegido (LATCH/representación), layout
   de estados (los ~7), primitivas que usa, tareas críticas y sus caminos. El spec sale de los
   hallazgos de la auditoría — cada [H] del objeto viejo debe tener respuesta explícita en el
   nuevo.
4. **Construcción**: el objeto nuevo usa TODAS las primitivas compartidas (undo, AsyncBoundary,
   formatters, tokens, validación) — un remake que reintroduce hex sueltos y toasts propios es
   un fracaso doble.
5. **Verificación de paridad**: recorré el inventario del paso 1 tarea por tarea en el objeto
   nuevo (no "se ve mejor" — "las 9 tareas se completan, 3 con menos pasos"). Walkthrough de
   primerizo + métricas de `metrics.md` si hay baseline.
6. **Switch**: reemplazo directo si el módulo es chico; feature flag / ruta paralela
   (`/facturas-v2` interna) solo si el objeto es crítico y el equipo quiere validar con
   usuarios reales primero. Borrá el viejo al confirmar — dos versiones vivas = deuda doble.

### En el reporte
- `[R] <objeto> — remake justificado por criterios {1,2,4} — spec: <10 líneas> — paridad: <N tareas>`
- El plan de remediación los lista después de las primitivas sistémicas (las primitivas se
  construyen primero — el remake las consume).

## Fuentes
- Rosenfeld, Morville & Arango — *Information Architecture* (polar bear), 4 sistemas
- Wurman — LATCH (`books.md` §A) · Krug — trunk test, site map y recall (`books.md` §B)
- NN/g — IA vs navegación — https://www.nngroup.com/articles/ia-vs-navigation/
- NN/g — card sorting / tree testing — https://www.nngroup.com/articles/card-sorting-definition/
