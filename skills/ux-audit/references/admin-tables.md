# Tablas y admin-apps — data grids, listados, permisos

El 80% de un ERP/admin/back-office ES una tabla. Consultá esto al auditar cualquier listado,
grid o módulo de administración. Complementa `patterns.md` (estados/forms) y `heuristics.md` #7.

---

## A. Anatomía de una tabla enterprise bien hecha

**Encontrar** (la tarea #1 en una tabla):
- **Búsqueda + filtros + orden** visibles sobre la tabla (no escondidos en un menú). Filtros
  aplicados = **chips removibles** ("Estado: activo ×") — se ve QUÉ está filtrado y se quita en
  un click. Botón "Limpiar filtros" si hay ≥2.
- **Resaltar el match** de la búsqueda dentro de las filas (mental matching).
- Orden: indicador de columna activa + dirección SIEMPRE visible; click en header = ciclo
  asc/desc; orden por default con sentido para la tarea (recientes primero, no alfabético
  arbitrario).
- **Conteo de resultados** siempre ("47 facturas · 3 filtradas") — sin él, el filtro es una caja
  negra.

**Leer**:
- Alineación: texto a la izquierda, **números a la derecha** (comparables) con **tabular
  figures**; fechas/formatos con el formatter único del proyecto.
- Columnas por la TAREA, no por el schema de la DB (nadie escanea UUID). La primera columna =
  el identificador que la persona reconoce (nombre, folio) y suele ser el link al detalle.
- **Densidad conmutable** (compacta/normal/cómoda) en tablas de trabajo intensivo; padding
  consistente; zebra striping o separadores finos en tablas largas.
- Truncado con tooltip/expand, no wrap que rompe el ritmo vertical; header **sticky** al
  scrollear; primera columna sticky si hay scroll horizontal.
- Estado del registro con **preatentivo** (chip color+texto, no solo palabra — Ware).

**Actuar**:
- **Bulk actions**: checkbox por fila + "seleccionar todo" (con aviso "las 200 de esta página
  vs las 1.450 totales" — el clásico bug de selección). La action bar aparece AL seleccionar
  (espacio limpio hasta entonces) y dice el alcance: "Eliminar 12".
- **Acciones por fila**: 1-2 primarias visibles + resto en menú ⋯. En touch NO ocultas tras
  hover (`patterns.md` §D). Toda destructiva → Undo o confirm (SKILL.md primitivas).
- **Inline edit** para 1 campo (doble-click o click en celda editable con affordance al hover);
  edición de N campos → modal/side sheet (heurística #21). Enter guarda, Esc cancela, feedback
  de guardado por celda.
- **Row → detalle**: fila clickeable si existe vista de detalle (y que se note). Para detalle
  RICO (multi-tab, formularios, matrices) prefiere un **side sheet / drawer** (panel deslizante,
  usualmente desde la derecha) sobre la **expansión inline hacia abajo**: la expansión inline
  empuja las filas de abajo, desplaza la lista (layout shift/CLS), obliga a scrollear entre la
  fila y su detalle, y no da límite claro de "estoy editando ESTE registro". El drawer preserva
  el contexto de la lista, enfoca la tarea, tiene cierre obvio (Esc/backdrop/X) y no reflowa la
  tabla. Reserva la expandable-row solo para un **quick-view** de 1-2 datos; en cuanto haya tabs
  o un formulario, es drawer. El drawer es un diálogo: `role="dialog"` + `aria-modal`, focus-trap
  y foco restaurado al disparador (misma primitiva que un modal, ver heurística #21).

**Escalar**:
- **Paginación > infinite scroll en admin-apps**: el operador necesita posición estable
  ("estaba en la página 3"), footer alcanzable y volver tras editar SIN perder su lugar.
  Infinite scroll solo para feeds de consumo. Tamaño de página configurable (25/50/100).
- **Preservar estado**: filtros/orden/página/densidad/columnas sobreviven navegar al detalle y
  volver (URL params o storage) — perder los filtros al volver = hallazgo [M] clásico.
- **Columnas gestionables** en tablas anchas: mostrar/ocultar/reordenar + guardar la vista;
  vistas guardadas con nombre para flujos repetidos ("Pendientes de pago").
- Export (CSV/Excel) donde el usuario es operador — pop-up-safe (síncrono en el gesto).

**Estados** (matriz de `patterns.md` §A aplicada): skeleton de FILAS en carga (no spinner
global); vacío-primerizo con CTA crear/importar; sin-resultados con "limpiar filtros"; error
con Reintentar ≠ vacío; y **la tabla nunca salta** al llegar datos (CLS).

---

## B. Patrones de admin-app (más allá de la tabla)

- **Disabled vs hidden por permisos**: si el usuario NUNCA podrá (rol), **ocultá**; si podría
  (falta un paso, plan, estado), mostrá **deshabilitado + razón** (tooltip/hint "Requiere rol
  admin"). Un botón disabled sin explicación = hacer pensar (Krug). Nunca la opción fantasma
  que al click da 403 — el permiso se resuelve ANTES de pintar.
- **Guardar**: formularios de config largos → botón sticky + **dirty state** visible ("Cambios
  sin guardar") + warning al salir con cambios (o autosave con "Guardado ✓" + historial).
- **Detalle**: side sheet/panel para consultar sin perder la lista; página completa para editar
  largo (misma lógica que modal vs página, heurística #21).
- **Audit trail visible** donde hay dinero/legal: quién cambió qué y cuándo — confianza y
  soporte (y matchea el Undo con soft-delete).
- **Jerarquía de acciones globales**: UNA primaria por vista ("+ Nueva factura"), consistente
  en posición (arriba-derecha) en TODOS los módulos (consistencia interna > creatividad).
- **Números con contexto** en dashboards de admin (Few): KPI con vs-periodo-anterior/meta;
  click en el KPI lleva a la tabla filtrada que lo explica (drill-down, no dead-end).

## Fuentes
- Pencil & Paper — Enterprise data tables — https://www.pencilandpaper.io/articles/ux-pattern-analysis-enterprise-data-tables
- Stéphanie Walter — Complex data tables — https://stephaniewalter.design/blog/essential-resources-design-complex-data-tables/
- UX Booth — User-friendly data tables — https://uxbooth.com/articles/designing-user-friendly-data-tables/
- Carbon / Polaris data table guidelines — https://carbondesignsystem.com/components/data-table/usage/ · https://polaris.shopify.com
