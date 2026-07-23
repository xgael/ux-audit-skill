# Patrones de interacción — estados, performance percibida, forms, mobile

Mecánica de interacción de alto retorno. Complementa `heuristics.md` (qué auditar) con el
CÓMO exacto de los patrones que más se repiten. El copy de estos patrones vive en `writing.md`.

---

## A. Matriz de estados — toda vista diseña sus ~7 estados
Una pantalla no está terminada cuando se ve bien con datos; está terminada cuando cubre:

1. **Ideal** (con datos) — el que todos diseñan.
2. **Loading** (primera carga) — skeleton, no spinner mudo (ver §B).
3. **First-use empty** — nunca hubo datos → onboarding + CTA (ver `writing.md` §4).
4. **Vaciado** — el usuario procesó todo → reconocimiento + siguiente acción.
5. **Sin resultados** — búsqueda/filtro sin match → limpiar/ajustar.
6. **Error** — con causa + Reintentar; visualmente DISTINTO del vacío.
7. **Parcial/degradado** — parte cargó, parte falló; conexión lenta; permisos limitados.

Al auditar: pedile a cada vista sus 7 estados. Los que falten son hallazgos (error que se ve
como vacío = [H]; empty sin CTA = [M]).

---

## B. Performance percibida — reglas por duración
Los umbrales de NN/g (0.1s / 1s / 10s) + Doherty (<400ms):

| Espera | Qué mostrar |
|---|---|
| **< 100 ms** | Nada. Se percibe instantáneo. |
| **100 ms – 1 s** | Nada o transición sutil. **NO spinner** (un spinner que aparece y desaparece en 300 ms = flash que distrae). |
| **1 – 10 s** | **Skeleton** que imita la forma del contenido (mejor percepción que spinner y sin ansiedad de "¿se colgó?"). Si es una acción puntual: spinner CON label ("Guardando…"). |
| **> 10 s** | Barra de **progreso real** + estimado + **Cancelar**. Considerar moverlo a background + notificar al terminar. |

Reglas:
- **Optimistic UI primero**: si la operación casi siempre triunfa (toggle, like, rename), aplicá
  el cambio YA y revertí con toast+Undo si falla. La espera desaparece.
- **Skeleton bien hecho**: misma forma/tamaño que el contenido real → **cero layout shift** al
  hidratarse (CLS). Shimmer sutil. No esqueletos genéricos que no matchean.
- **Debounce el indicador**: mostrá el loading solo si la espera supera ~300-500 ms.
- Espera **incierta se siente más larga** que espera conocida → siempre comunicar progreso o
  estimado en operaciones largas; nunca "Espere…" indefinido.
- **No bloquear la UI entera** por una región que carga (loading por sección, no overlay global).
- Percepción > velocidad real: responder al INPUT en <100 ms (botón se marca presionado, item
  aparece optimista) aunque el server tarde.

---

## C. Formularios — mecánica grado Baymard
(El copy de forms está en `writing.md` §5; los constraints por tipo en SKILL.md/heuristics.)

- **Timing de validación — "reward early, punish late"**: NO marques error mientras el usuario
  todavía teclea el campo por primera vez; validá al **blur** (o al submit). PERO: una vez que un
  campo ya está en error, re-validá **en vivo** (keystroke) para que el error desaparezca apenas
  se corrige. Confirmación positiva inmediata (check verde) sí puede ser en vivo.
- **Error summary arriba en forms largos** (GOV.UK): lista de errores con **links que enfocan** el
  campo; además del error inline en cada campo. En submit fallido: scroll+focus al primer error.
- **Labels top-aligned** (Wroblewski): más rápidas que a la izquierda; imprescindible en mobile.
- **`autocomplete` correcto en TODO campo estándar**: `name`, `email`, `tel`,
  `street-address`/`address-line1`, `postal-code`, `cc-number`, `cc-exp`, `new-password`/
  `current-password`, `one-time-code`… El autofill del navegador elimina el 80% del tecleo — un
  form sin `autocomplete` es fricción gratis. + `inputMode` para el teclado móvil correcto.
- **Una columna** siempre (multi-columna rompe el orden de escaneo). Agrupar con
  fieldset/secciones; ~4-7 campos por grupo (chunking).
- **Single vs multi-step**: form corto → una página. Flujo complejo → pasos con **indicador de
  progreso** (paso 2 de 4, nombres de pasos) + **guardado parcial** + volver atrás sin perder
  datos. GOV.UK "one thing per page" para flujos de alto estrés.
- **Formateo automático forgiving**: espacios de tarjeta cada 4 dígitos, teléfono con guiones —
  el sistema formatea, el usuario solo teclea (Postel). Jamás rechazar por espacios/guiones.
- **No pedir lo que no se necesita** (goodwill): cada campo extra baja conversión. Billing =
  shipping por default (checkbox). No repreguntar lo que ya se sabe (Cooper: excise).
  Arma formal: **question protocol** (Jarrett, books §B) — por cada campo, ¿quién usa la
  respuesta y para qué? Sin dueño → el campo muere.
- **Control correcto por cardinalidad**: 2-5 opciones visibles → radio/segmented (no dropdown);
  ~7+ conocidas → **select con búsqueda (combobox type-to-filter)**, NUNCA un `<select>` plano
  largo que obliga a scrollear y leer todo (elegir "Manager" entre 40 empleados en un `<select>`
  es fricción real); fecha de nacimiento → campos de texto (no calendario); fecha de cita →
  calendario (modelo mental). Regla operativa: `<select>` con >~7 `<option>` = candidato a
  combobox. Para campos de **valor libre con sugerencias** (un puesto que puede no estar en el
  catálogo) el `<datalist>` nativo es la opción lazy correcta: type-to-search + acepta texto
  arbitrario, sin librería. El combobox debe abrir la lista al foco (no solo al teclear), filtrar
  en vivo, permitir limpiar la selección y mostrar "Sin resultados". Construílo una vez como
  primitiva compartida y reusalo — no un `<select>` distinto por pantalla.
- **Campos condicionales / dependientes** — cuando el valor de un campo vuelve a otro
  **imposible o sin sentido**, deshabilitá Y **limpiá** el dependiente (no lo dejes con un valor
  viejo que se enviaría), con un hint que diga por qué. Ej.: contrato "indefinido" → "fin de
  contrato" deshabilitado + vaciado ("No aplica en indefinidos"); método de pago "efectivo" →
  campos de tarjeta ocultos. Es prevención de error (Nielsen #5) + match al modelo mental: la UI
  refleja qué combinaciones son válidas antes de que el usuario (o el submit) choque con ellas.
  Preferí **deshabilitar visible** (enseña la relación) sobre ocultar cuando el campo es parte
  esperada del formulario; ocultá solo si el campo es irrelevante en ese modo. Enforzalo también
  en submit/servidor — el disabled del cliente es UX, no validación.
- **Submit**: label específico ("Crear cuenta", no "Enviar"). Preferí botón **habilitado que
  valida al click y enfoca el primer error** sobre botón deshabilitado mudo (el disabled no dice
  POR QUÉ no se puede — si lo usás, mostrá la razón al lado). Deshabilitá DURANTE el submit
  (doble-click = doble registro) con spinner+label en el botón.

---

## D. Mobile / touch
- **Thumb zone** (Hoober: ~49% usa una mano): acciones primarias y frecuentes en la **mitad
  inferior** (bottom nav, FAB, botones de acción); acciones destructivas FUERA de la zona fácil
  del pulgar (esquina superior) para evitar toques accidentales.
- **Targets**: mínimo 44×44 pt (Apple) / 48×48 dp (Material); WCAG 2.5.8 exige ≥24 px. Y
  **espacio entre targets** — dos iconos de 48px pegados fallan igual (fat finger).
- **Gestos nunca como único camino**: swipe-to-delete, pull-to-refresh, long-press son
  invisibles (no descubribles) → siempre con alternativa visible (botón, menú). Nada crítico
  detrás de un gesto custom.
- **Hover no existe en touch**: ninguna acción/info disponible SOLO en hover (menús que aparecen
  al pasar el mouse, tooltips esenciales, botones de fila ocultos). Alternativa: visible siempre
  en touch o tras tap.
- **Teclado correcto**: `inputMode`/`type` por campo (tel/email/numeric/url) — teclear un email
  con teclado default = fricción. `autocomplete` + `one-time-code` para OTP.
- **Minimizar tecleo**: en mobile todo tecleo cuesta el triple → pickers, steppers, defaults,
  geolocalización, escanear tarjeta, autofill.
- **No bloquear zoom** (`user-scalable=no` = anti-a11y); respetar safe areas (notch);
  formularios con label top y campos 100% de ancho.
- La restricción mobile **obliga a priorizar** (Krug/Wroblewski): si en mobile sobra, quizá en
  desktop también.

## Fuentes
- NN/g Response Times (0.1/1/10 s) — https://www.nngroup.com/articles/response-times-3-important-limits/
- NN/g Skeleton screens / progress indicators — https://www.nngroup.com/articles/progress-indicators/
- Baymard Institute (forms/checkout research) — https://baymard.com/blog
- GOV.UK error summary + one-thing-per-page — https://design-system.service.gov.uk/components/error-summary/
- Luke Wroblewski, *Web Form Design* — labels, validación inline
- Steven Hoober, thumb zone — https://www.uxmatters.com/mt/archives/2013/02/how-do-users-really-hold-mobile-devices.php
- HTML autocomplete attrs — https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/autocomplete
