# Eficiencia de flujo — interaction cost, consolidación, redundancias

Optimizar el TRABAJO del usuario: menos pasos, menos saltos, menos re-tecleo, para la misma
tarea. Marco: **interaction cost** (NN/g) = suma de esfuerzo físico + mental (clicks, scrolls,
cambios de vista, esperas, recordar, decidir) hasta el objetivo. El rediseño gana si BAJA el
costo total de la tarea — no si "se ve más limpio".

---

## A. Auditoría de costo de interacción (GOMS/KLM casero)
Para cada tarea crítica (las de Fase 1), contá sobre el flujo REAL:

| Contador | Qué es | Señal de alarma |
|---|---|---|
| **Pasos** | acciones discretas (click, tecleo, selección) | tarea diaria >10 pasos |
| **Saltos de vista** | navegaciones/modales/pestañas que cambian contexto | >2 por tarea |
| **Re-tecleos** | datos que el usuario escribe y el sistema YA sabía | cualquiera = excise |
| **Ping-pong** | volver a una vista ya visitada en la misma tarea (lista↔detalle↔lista) | >1 ida y vuelta |
| **Esperas** | cargas entre pasos (se suman al costo) | >2 s por transición |
| **Decisiones** | opciones a evaluar por paso (Hick) | menús largos a mitad de flujo |
| **Manos** | alternancia teclado↔mouse en flujos de captura | cada cambio cuesta ~0.4 s |

Registrá la fila por tarea (baseline) → rediseñá → contá de nuevo. "De 14 pasos y 4 vistas a
6 pasos y 1 vista" es un hallazgo/resultado defendible; "más ágil" no.

**Los 7 desperdicios (Lean adaptado a UX)** — nombralos al auditar:
1. **Re-captura** — teclear lo que el sistema sabe (el peor).
2. **Transporte** — copiar/pegar entre pantallas del MISMO producto.
3. **Movimiento** — navegar de más (menús profundos para lo diario, architecture §A).
4. **Espera** — cargas/confirmaciones entre pasos.
5. **Sobre-proceso** — confirmaciones/aprobaciones que no protegen nada (usar Undo, no confirm).
6. **Inventario** — trabajo a medias sin guardar (sin drafts → miedo → re-trabajo).
7. **Defectos** — errores de captura que se corrigen después (validación tardía, patterns §C).

## B. Consolidación de vistas — cuándo 3 pantallas deben ser 1
**Señales de fragmentación** (el hallazgo que describís):
- Para completar UNA decisión el usuario abre 2-3 vistas y **memoriza** valores entre ellas
  (working memory como clipboard = fallo grave: recognition over recall, NN#6).
- Dos pantallas con el 70% de los mismos datos y acciones (duplicadas de origen).
- El flujo diario cruza módulos que SIEMPRE se usan juntos (facturar: cliente → productos →
  precios en 3 lugares).
- Ping-pong lista↔detalle para comparar o editar en serie.

**Patrones de consolidación** (elegir por tarea, no por gusto):
- **Master-detail en una vista**: lista + panel de detalle lado a lado (o drawer) — se navega
  la lista SIN perderla; edición en serie sin ping-pong. El fix #1 para lista↔detalle.
- **Edición en contexto**: inline edit / popover sobre la lista para cambios de 1-2 campos —
  no viajar a una pantalla "Editar" entera (admin-tables §A).
- **Workspace de tarea**: para LA tarea núcleo del rol (capturar pedidos, despachar tickets),
  una vista que junta TODO lo necesario (datos + acciones + contexto) aunque "pertenezca" a
  3 módulos — la organización sigue a la TAREA, no al schema (LATCH por tarea).
- **Vista comparativa**: si el usuario abre N detalles para comparar → tabla/side-by-side con
  los campos que compara.
- **Wizard → formulario único** cuando son ≤6-8 campos sin dependencias: los pasos solo
  agregan clicks y ocultan el panorama. (Inverso: form gigante con dependencias → wizard;
  patterns §C.)
- **Preview integrado**: si para verificar el resultado hay que salir (abrir el PDF, ver el
  sitio), embebé el preview al lado de la edición.

**Contrapeso — cuándo NO consolidar** (el anti-mega-pantalla):
- Consolidar ≠ amontonar: si la vista resultante mezcla 3 TAREAS distintas (no 3 vistas de la
  misma tarea) → viola señal/ruido y Hick; la unidad de consolidación es LA TAREA.
- Audiencias distintas (operador vs gerente) → vistas distintas aunque el dato sea el mismo.
- Si la consolidación exige sacrificar los 7 estados o el responsive → progressive disclosure
  dentro de la vista (paneles colapsables, tabs internas) antes que re-fragmentar.

## C. Reordenar bloques — layout que sigue al flujo
- **El orden visual = el orden de trabajo**: los campos/bloques en la secuencia en que el
  usuario los completa/consulta (que el formulario espeje el documento fuente si captura de
  papel/PDF — misma secuencia, mismos nombres).
- **Lo frecuente cerca y grande** (Fitts): la acción de cada 2 minutos no vive en un menú de
  3 niveles ni en la otra esquina de la pantalla; acciones sobre un item, PEGADAS al item.
- **Juntos los que trabajan juntos** (Gestalt proximidad, mapping de Norman): el total cerca
  de las líneas que lo generan; el botón guardar donde termina el flujo del ojo (fin del form,
  o sticky si el form es largo).
- **Defaults del contexto**: fecha=hoy, sucursal=la del usuario, cliente=el último usado en
  esta sesión — cada default correcto borra un paso (Cooper: la computadora trabaja).
- **Teclado de punta a punta** en flujos de captura intensiva: Tab en orden lógico, Enter
  avanza/guarda, atajos en acciones repetidas — el operador de captura no debe tocar el mouse.
- **Bulk sobre repetición**: si el usuario repite la misma acción N veces seguidas (aprobar,
  etiquetar, cerrar), falta bulk action o "aplicar a siguientes" (admin-tables §A).

## D. Detectores de redundancia (olfato)
1. El usuario tiene **una hoja de cálculo/nota al lado** del sistema → el sistema no le da esa
   vista (el workaround ES el requerimiento).
2. **Copy-paste interno**: entre pantallas propias = transporte; el dato debería fluir solo.
3. Dos módulos con CRUD del mismo objeto con reglas levemente distintas → consolidar el
   canónico (architecture §A: un lugar por cosa).
4. Campos que nadie llena / columnas que nadie mira → candidatos a morir (80/20, Lidwell) —
   verificá con datos/preguntando antes de matar.
5. Reportes que se piden "para pasarlos a Excel y ahí sí trabajarlos" → la vista de trabajo
   real no existe.
6. Confirmaciones en cadena ("¿Seguro? → ¿Realmente seguro?") → una sola, o mejor Undo.

## E. En el reporte
- Hallazgo de eficiencia: `[M/H] tarea "{X}": {n} pasos, {m} vistas, {k} re-tecleos — fix:
  {consolidación/reorden/defaults} → estimado {n'} pasos, {m'} vista(s)`.
- La consolidación de vistas grande puede ameritar **[R] remake** (architecture §B) — mismos
  criterios y proceso; el inventario de paridad es por TAREAS, ya lo tenés del conteo.
- Métrica post-fix natural: time-on-task y pasos (metrics §A) — este tipo de fix es el que más
  fácil demuestra su valor.

## Fuentes
- NN/g — Interaction cost — https://www.nngroup.com/articles/interaction-cost-definition/
- Card, Moran & Newell — GOMS/KLM (*The Psychology of Human-Computer Interaction*)
- Cooper — excise (*About Face*, books §B) · Lidwell — 80/20, performance load (books §C)
- Raskin — *The Humane Interface* (books §B): modos causan errores, habituación (confirm
  repetido no protege → Undo), monotonía (UNA vía canónica por tarea), atención sagrada
- NN/g — Workflow expectations en apps enterprise — https://www.nngroup.com/articles/enterprise-workflows/
