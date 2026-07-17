# Plataforma y atención — notificaciones, settings, URLs, nativo

La capa que rodea a las pantallas: cómo el producto interrumpe, se configura, se comparte y
respeta las convenciones de su plataforma.

---

## A. Notificaciones — presupuesto de interrupción
La atención del usuario es un presupuesto finito; cada notificación gasta goodwill. La fatiga
de notificaciones termina en "desactivar todo" — y ahí perdiste el canal.

- **Canal según urgencia**: bloquea la tarea → modal/inline; requiere acción pronto → push/
  badge; informativo → centro de notificaciones/digest; marketing → email con opt-in APARTE
  (jamás mezclado con las transaccionales).
- **Batching**: 5 eventos del mismo tipo = UNA notificación agrupada ("5 comentarios nuevos"),
  no cinco. Digest diario/semanal para lo no urgente, configurable.
- **Accionable o no existe**: la notificación dice qué pasó Y qué hacer, y el tap aterriza en
  el contexto exacto (deep link a la cosa, no a la home).
- **Quiet hours y granularidad**: silenciar por horario y POR TIPO ("menciones sí, resúmenes
  no") — el opt-out granular previene el opt-out total. Respetar el modo No molestar del OS.
- **Badge honesto**: el contador rojo refleja cosas que REQUIEREN al usuario; badge perpetuo en
  9+ = ruido entrenado a ignorar (y ansiedad gratis).
- Estado sincronizado: lo leído en un dispositivo se apaga en todos.

## B. Settings — la filosofía Apple
- **El mejor setting es el que no existe**: defaults tan correctos que el 95% jamás abre
  Settings. Cada opción nueva = una decisión que el equipo le delegó al usuario (Tesler). Antes
  de agregar un toggle, preguntá: ¿no podemos decidir nosotros bien?
- Si existe: **agrupado por tarea del usuario** (no por módulo interno), buscable (settings
  largos sin búsqueda = laberinto), con el valor actual visible en el listado (no "Notificaciones
  >" a ciegas).
- Cambios aplican al instante con Undo/restaurar default — no "Guardar" global que se olvida.
  "Restaurar valores por defecto" siempre disponible, por sección.
- Lo peligroso (borrar cuenta, transferir) al fondo, en "zona de peligro" visualmente separada,
  con fricción proporcional (confirm con nombre, no un toggle alegre).

## C. Deep links y URL-as-state
- **Toda vista con identidad tiene URL**: detalle, tab activo, filtros aplicados, página,
  búsqueda → en la URL (query params). Test: copiá la URL en otra ventana — ¿aterriza en lo
  MISMO que veías? Si no, el estado es rehén de la sesión.
- Beneficios en cadena: compartible ("mirá esta factura" = un link), bookmarkeable, el Back
  funciona (Krug), el refresh no pierde nada, y el soporte recibe links exactos en vez de
  "andá a Ventas y filtrá por…".
- Deep links que sobreviven login: URL profunda sin sesión → login → **aterrizar en la URL
  original** (no en el dashboard — el clásico link muerto de email).
- URLs legibles y estables: `/facturas/1043`, no `/view?id=8f3a...` — la URL es UI (trunk test
  §: ¿dónde estoy?). Rutas viejas redirigen (architecture §A).

## D. Convenciones nativas vs web (Jakob a nivel plataforma)
La app debe comportarse como las demás de SU plataforma — el usuario trae memoria muscular.
- **iOS** (HIG): back por swipe desde el borde SIEMPRE; tab bar abajo (no hamburguesa);
  share sheet nativo (no un share custom); pull-to-refresh; alerts nativos para lo destructivo
  (rojo, orden de botones del sistema); SF Symbols; safe areas; háptica con moderación.
- **Android** (Material): back del sistema (gesto/botón) NUNCA roto ni reinterpretado;
  Material You/dynamic color donde aplique; FAB para LA acción primaria; snackbar (no toast
  custom); patrones de permisos del OS.
- **Web app de escritorio**: ⌘K, atajos de teclado documentados (tooltip con el shortcut),
  click derecho no secuestrado sin razón, múltiples pestañas del mismo producto sin romperse,
  y los estándar del browser intactos (Back, refresh, zoom, buscar-en-página).
- La misma feature puede necesitar TRES formas (bottom sheet en iOS, dialog en Android, popover
  en desktop) — consistencia con la plataforma > consistencia interna pixel-perfect (GOV.UK:
  "be consistent, not uniform").
- Anti-patrón: web app que importa patrones móviles al desktop (hamburguesa con pantalla de
  sobra, bottom-nav en 27") o viceversa (hover-only en touch, patterns §D).

## E. Checklist plataforma/atención (auditable)
1. Notificaciones agrupadas por tipo + opt-out granular + quiet hours; badge = accionables reales.
2. Tap/click en notificación → contexto exacto (deep link), no la home.
3. Copiar URL de una vista filtrada y abrirla en incógnito → mismo estado tras login.
4. Defaults revisados: ¿cuántos settings existen solo porque nadie decidió?
5. Settings buscables, valor visible, aplican al instante, restaurables.
6. iOS: swipe-back funciona en TODA pantalla. Android: back del sistema jamás roto. Desktop:
   atajos + Back del browser intactos.
7. Zona de peligro separada y con fricción proporcional.

## Fuentes
- Apple HIG — https://developer.apple.com/design/human-interface-guidelines
- Material 3 — https://m3.material.io
- NN/g — notificaciones y atención — https://www.nngroup.com/articles/push-notification/
- URLs as UI (Nielsen, 1999 — sigue vigente) — https://www.nngroup.com/articles/url-as-ui/
- GOV.UK — consistent not uniform — https://www.gov.uk/guidance/government-design-principles
