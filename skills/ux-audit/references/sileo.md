# Feedback vivo — el modelo Dynamic Island, implementado con Sileo

Sileo (`npm i sileo` — sileo.aaryan.design) es parte del stack por defecto de nuestros
productos. **No es un clon de sonner**: es una isla dinámica web — un pill que morfea entre
estados con física de resorte, igual que el Dynamic Island de Apple. Usarlo solo para
"Guardado ✓" es desperdiciar la primitiva. Este doc: el modelo UX de Apple (por qué),
las reglas trasladadas a web (el contrato), el API de Sileo (cómo), las primitivas
canónicas (qué construir) y los checks auditables (qué buscar en Fase 1).

---

## A. El modelo Dynamic Island / Live Activities (Apple) — por qué funciona

La idea central de Apple: **el estado continuo tiene UN hogar fijo que morfea, en vez de
N banners apilados**. Una Live Activity es para tareas con inicio y fin definidos
(minutos a un par de horas, máx 8h) — no para eventos one-shot (eso es una notificación)
ni contenido estático (eso es un widget).

Principios (HIG + WWDC23 "Design dynamic Live Activities"):

1. **Estado continuo ≠ notificación.** El progreso de algo en curso se actualiza EN EL
   MISMO lugar; jamás un banner nuevo por tick. Update solo cuando el contenido cambió
   de verdad — si el estado es el mismo, la UI no se mueve.
2. **Tres tamaños, jerarquía de atención.** *Minimal* (ícono + un dato: el Timer muestra
   el tiempo restante, no un ícono estático) → *compact* (dos elementos que se leen como
   UNA sola información) → *expanded* (touch-and-hold: detalle + controles, manteniendo
   la posición relativa de los elementos para que expandir se sienta predecible).
3. **Tap = deep link al contexto exacto.** "Take people directly to related details and
   actions — don't make them navigate." Nunca a la home.
4. **Alertar = expandir lo existente, no crear otro aviso.** Una actualización crítica
   expande/ilumina la isla; jamás se duplica con un push por el mismo evento.
5. **Presupuesto de atención.** ActivityKit impone budget por hora a los updates
   (priority 10 gasta budget, priority 5 no) — updates silenciosos baratos, updates que
   piden atención racionados. El usuario puede apagar las frecuentes por app.
6. **Staleness de primera clase.** `staleDate` marca contenido viejo ("sin conexión")
   en vez de congelar una barra de progreso que miente.
7. **Estados de cierre con gracia.** Al terminar: update final que persiste como resumen
   (recomendado 15–30 min — el rideshare deja el resumen del viaje para propina/recibo)
   y luego se va. Descartar la UI **nunca cancela la tarea** ("swipe no cancela la pizza").
8. **Concurrencia acotada.** La isla muestra máx 2 actividades (por `relevanceScore`);
   HIG prefiere UNA actividad que rota entre eventos a varias en paralelo.
9. **Contenido esencial, sin ads.** "Don't use a Live Activity to display ads or
   promotions." Nada sensible a nivel vistazo; máximo UN control interactivo en compact.
10. **Motion continuo, no modal.** El contenedor morfea entre tamaños (spring), los
    contadores transicionan numéricamente, ≤2s de animación; sin animación en Always-On.

Ejemplos que anclan el modelo:

| Experiencia | Qué muestra | Interacción |
|---|---|---|
| Timer | Tiempo restante (hasta en minimal) | Tap → Clock; hold → pausar/cancelar |
| Uber/Lyft | Buscando → asignado → llegando (ETA vivo) | Tap → viaje; botón "contactar chofer" |
| DoorDash | Etapa del pedido + countdown | Tap → tracking; botón "check-in pickup" |
| Deportes | Marcador vivo; UNA actividad rota entre partidos | Alert-expand al gol |
| Música | Carátula + waveform animado | Hold → scrubber + controles |
| Face ID / AirPods / carga | Morph de estado puro, auto-dismiss | Ninguna — puro feedback |
| Vuelos (Flighty) | Gate/estado; resumen post-aterrizaje | Tap → pase de abordar |

---

## B. El contrato trasladado a web (lo que auditamos y construimos)

1. **Ruteo por duración** (cruza con `patterns.md` §B): mutación instantánea → toast
   transitorio con autodismiss; operación con inicio/fin y progreso (job, import, upload,
   deploy, bulk) → UNA entrada viva que se actualiza in place (`sileo.promise` morfea
   loading→resultado en el mismo pill); >10s → progreso real + Cancelar.
2. **Accionable o no existe**: el toast que refiere a una entidad lleva `button` que
   navega al contexto exacto (drawer/detalle), no aviso decorativo.
3. **Alert = morph, no toast nuevo**: la transición crítica (falló el job, requiere
   input) cambia el estado del pill existente; los ticks no-críticos actualizan sin
   pedir atención. Presupuesto de interrupción = `platform.md` §A.
4. **Cierre con gracia + Undo**: el resumen final persiste dismissible con la acción
   terminal (Ver resultado / Descargar / **Undo**) — la ventana post-acción de Apple ES
   nuestra ventana de undo. Descartar el toast jamás cancela la operación; cancelar es
   un botón explícito.
5. **Heartbeat honesto**: si el feed vivo (poll/SSE) pierde señal N segundos, la UI lo
   dice ("actualizado hace 45s") en vez de congelar datos que mienten.
6. **Concurrencia acotada**: burst de eventos → agrupar/batchear (una entrada que rota o
   un contador), no N pills apilados. Sileo NO limita los visibles — el límite lo pone
   nuestro código.
7. **Esencial, un control, sin promos**: máximo un botón por toast; nada de marketing en
   la capa de status; valores sensibles redactados a nivel vistazo.
8. **Motion**: el morph ya lo trae Sileo (spring 600ms); respetar
   `prefers-reduced-motion` con CSS (`--sileo-duration`), nada de re-crear toasts para
   "animar" cambios.

---

## C. Sileo — API esencial (v0.1.x)

```jsx
import { sileo, Toaster } from "sileo";   // CSS auto-importado; peer: react>=18, dep: motion

<Toaster position="top-right" offset={{ top: 20 }} theme="system"
         options={{ fill: "#171717", roundness: 14, styles: { title: "text-white!" } }} />
```

Métodos — todos devuelven el `id` (string) del toast:

```ts
sileo.success|error|warning|info|action(opts)   // action = con botón
sileo.show(opts)                                // ⚠️ sin type → estado success, no neutral
sileo.promise(promesa, { loading, success, error, action?, position? })  // morfea in place
sileo.dismiss(id); sileo.clear(position?)
```

`SileoOptions` (por toast, o defaults globales vía `<Toaster options>`):

| Campo | Tipo/Default | Nota |
|---|---|---|
| `title` / `description` | string / ReactNode | description acepta JSX |
| `duration` | `6000` \| `null` | `null` = persistente (errores con acción) |
| `button` | `{ title, onClick }` | UN botón; navega o deshace |
| `icon` | ReactNode | reemplaza el ícono de estado |
| `fill` | `"#FFFFFF"` ⚠️ | color del pill SVG — blanco sobre blanco = invisible |
| `roundness` | `16` | radio px |
| `autopilot` | `true` \| `false` \| `{ expand, collapse }` | auto expande (150ms) y colapsa (4000ms) |
| `position` | del Toaster | override por toast |
| `styles` | `{ title, description, badge, button }` | clases → `[data-sileo-*]`; necesitan `!` (important) |

`sileo.promise` — la primitiva isla (un pill, tres estados):

```ts
sileo.promise(createUser(data), {
  loading: { title: "Creando cuenta…" },                       // duration null implícito
  success: (u) => ({ title: `Bienvenido, ${u.name}` }),
  error:   (e) => ({ title: "Falló el registro", description: e.message }),
  action:  (u) => ({ title: "Cuenta creada", button: { … } }), // reemplaza success, con botón
});
```

Styling — tokens CSS (override global; states en oklch):

```css
:root { --sileo-state-success|loading|error|warning|info|action: …;
        --sileo-width: 350px; --sileo-height: 40px; --sileo-duration: 600ms; }
```

**Gotchas** (v0.1.x, API joven):
- `fill` default blanco → siempre setear fill de marca o `theme` en el Toaster.
- **Brandear con `fill` + `--sileo-state-*` + `styles`, NO con el prop `theme`** cuando el
  producto tiene tema propio — `theme` apunta al esquema de la página e invierte la
  superficie del pill.
- Sin límite de visibles ni queue → el batching es responsabilidad nuestra (§B.6).
- Sin update-by-id público fuera de `promise`; sin callbacks de dismiss; sin API de
  progreso (para % real: `description` custom o barra propia en la vista).
- Hover pausa timers (bien); no hay expand-on-hover tipo stack de sonner.

---

## D. Primitivas canónicas (fix-once → arregla-N)

**D1. Mutación reversible → `sileo.promise` + Undo** (reemplaza el `confirm()` bloqueante;
cruza con SKILL.md Fase 2 "Undo real"). Referencia real: `ArchiveCallButton.tsx` (topjets).

```tsx
await sileo.promise(doArchive(id), {
  loading: { title: "Archivando…" },
  success: { title: "Llamada archivada",
             button: { title: "Undo", onClick: () => restore(id) } },  // restaura DE VERDAD
  error: (e) => ({ title: e instanceof Error ? e.message : "No se pudo archivar" }),
});
```

Optimistic UI primero (`patterns.md` §B): si casi siempre triunfa, aplicá ya y revertí
con toast+Undo si falla. Confirm modal solo donde el undo es imposible (hard-delete,
valor enmascarado).

**D2. Live console / heartbeat** (eventos de negocio en tiempo real). Referencia real:
`LiveNotifications.tsx` + `pulse-store.ts` (topjets). Mecánica:

- Poll a endpoint auth-gated (~45s + al `focus` de la ventana) o SSE; publica a un
  pub/sub chico que el badge del sidebar consume.
- **Primer sample = baseline** — establece estado sin disparar nada (cero toast storm al
  cargar). Solo lo NUEVO respecto del sample anterior dispara toast.
- Cada evento → `sileo.action` con botón que navega al contexto exacto ("Nueva
  solicitud" → Review → `/admin/brokers/{id}`).
- Eventos por tiempo ("cita en <30 min"): trackear ids ya disparados en un ref.
- Honestidad: sin señal N ticks → marcar stale (§B.5), no congelar el badge.

**D3. Proceso largo / background job**: arranca con `sileo.promise` (pill loading
persistente); al terminar, `action` con "Ver resultado". Si supera ~10s o el usuario
navega, el estado vive ADEMÁS en la vista (barra real + Cancelar — `patterns.md` §B);
el toast es el anuncio, no el único registro. Falla → `error` con `duration: null` +
botón Reintentar (un error que se autodescarta con acción pendiente = hallazgo).

**D4. Branding**: un `<AppToaster>` propio que envuelve `<Toaster>` con `fill` de
superficie de marca, `roundness` del sistema y `styles` con los tokens del proyecto
(referencia: `AppToaster.tsx` topjets — crema/tinta/dorado). Los estados semánticos vía
`--sileo-state-*` en el CSS global. UNA sola instancia, en el layout raíz.

---

## E. Checks auditables (Fase 1 — dimensión 4 Feedback)

1. ¿`confirm()`/`window.confirm` bloqueante donde cabe optimistic + promise+Undo? → [M];
   si además el "undo" prometido no restaura de verdad → [H] (undo falso).
2. Mutación silenciosa (sin toast ni cambio visible) → [H] (heuristics #1).
3. Operación con inicio/fin que reporta por toasts SUELTOS por tick, en vez de un
   `promise` que morfea in place → [M].
4. Error con acción pendiente en toast con autodismiss (se va solo a los 6s) → [M/H];
   error de FORM en toast en vez de inline junto al campo → [M] (writing.md §3).
5. Toast que refiere a entidad sin `button` de deep link → [M] (aviso decorativo).
6. Toast storm al cargar la vista (falta baseline en el poller) → [M].
7. Burst sin batching (5 eventos = 5 pills) → [M] (`platform.md` §A: 5 = UNA agrupada).
8. Dismiss del toast que cancela/rompe la operación subyacente → [H].
9. Feed "vivo" que congela datos sin indicador de staleness → [M].
10. Branding: `fill` default blanco invisible, `theme` invirtiendo superficie, hex
    hardcodeado en `styles` en vez de tokens → [L/M].
11. Contenido promocional en la capa de status → [H] (dark pattern territory).

## F. Anti-patrones

- **Sileo como sonner clone**: solo `success`/`error` tras guardar, sin promise, sin
  botones, sin live console — desperdicio de la primitiva (y del morph).
- Toast-por-tick de progreso; N pills apilados para un solo proceso.
- Undo decorativo (botón que no restaura) — peor que no ofrecerlo.
- Duplicar el mismo evento en toast + badge + email sin presupuesto (`platform.md` §A).
- Toast como único registro de algo importante (si expiró, el usuario no tiene dónde
  verlo → necesita vista/centro propio).
- Re-crear toasts para simular animación de update (el morph existe: `promise`).

## Fuentes

- Apple HIG — Live Activities — https://developer.apple.com/design/human-interface-guidelines/live-activities
- ActivityKit — displaying live data / push updates & budget — https://developer.apple.com/documentation/activitykit
- WWDC23 — Design dynamic Live Activities — https://developer.apple.com/videos/play/wwdc2023/10194/
- Dynamic Island animations dissected — https://uxdesign.cc/apples-dynamic-island-animations-dissected-into-4-principles-b3bfdf546d9a
- Sileo docs — https://sileo.aaryan.design/docs (+ /docs/api, /docs/api/toaster, /docs/styling)
- Sileo source — https://github.com/hiaaryan/sileo
- Referencias internas: `patterns.md` §B (performance percibida), `platform.md` §A
  (presupuesto de interrupción), `writing.md` §3 (copy de errores).
