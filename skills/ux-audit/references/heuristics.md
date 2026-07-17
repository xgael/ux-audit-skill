# Referencia UX — heurísticas, principios de compañías top, Laws of UX, WCAG, fixes

Fuente de verdad para la Fase 1 (auditar) y Fase 2 (arreglar). Cada ítem trae su fuente.
Al auditar, recorré una pantalla contra §1 y §7; al arreglar, usá §6.
Profundidad por tema: copy/errores/empty states → `writing.md` · estados/performance
percibida/forms/mobile → `patterns.md` · checks corribles por script → `audit-scripts.md` ·
onboarding/microinteracciones/journey mapping → `flows.md` · tablas/admin → `admin-tables.md` ·
features con IA → `ai-ux.md` · impacto medible → `metrics.md` · prompts/reporte → `templates.md` ·
IA/navegación y remakes [R] → `architecture.md` · red/sesión/seguridad/permisos →
`resilience.md` · craft visual (espaciado/tipo/color/motion) → `craft.md` ·
notificaciones/settings/URLs/nativo → `platform.md` · interaction cost/consolidación de
vistas/redundancias → `efficiency.md` ·
psicología/comportamiento, canon de libros y fuentes maestras → `books.md`.

---

## 1. Nielsen Norman Group — 10 heurísticas de usabilidad (columna vertebral)
Verbatim + cómo verificarlo en una web-app.

1. **Visibility of System Status** — mantener al usuario informado con feedback oportuno.
   → *Check*: toda carga tiene loading; toda acción tiene feedback (toast/estado); nada "silencioso".
2. **Match Between System and the Real World** — hablar el idioma del usuario, sin jerga.
   → *Check*: labels en el idioma del producto; dato temporal→calendario; nombres que la persona reconoce (no `webhook config`).
3. **User Control and Freedom** — "salida de emergencia": deshacer/cancelar.
   → *Check*: toda acción destructiva es **reversible (Undo)** o se **confirma**; se puede cancelar/volver.
4. **Consistency and Standards** — mismas palabras/acciones significan lo mismo; convenciones de plataforma.
   → *Check*: un solo sistema de toasts, botones, formato de moneda/fecha; tokens, no hex sueltos.
5. **Error Prevention** — prevenir el error, no solo mostrarlo.
   → *Check*: validación inline (fin>inicio), deshabilitar submit inválido, bloquear borrar registros legales.
6. **Recognition Rather than Recall** — minimizar memoria: opciones visibles.
   → *Check*: command palette ⌘K, defaults sensatos, mostrar en vez de exigir teclear/recordar IDs.
7. **Flexibility and Efficiency of Use** — atajos para expertos sin estorbar a novatos.
   → *Check*: ⌘K, bulk actions, filtros/orden, teclado.
8. **Aesthetic and Minimalist Design** — sin info irrelevante; progressive disclosure.
   → *Check*: revelar detalle bajo demanda (panel/expand), no volcar todo de golpe.
9. **Help Users Recognize, Diagnose, Recover from Errors** — mensajes en lenguaje claro + solución.
   → *Check*: estado de error con causa + botón **Reintentar**; nunca un error que se ve como "vacío".
10. **Help and Documentation** — idealmente innecesaria; si hace falta, accesible y contextual.
   → *Check*: hints inline, tooltips, estado "configurar" explicado.

Fuente: https://www.nngroup.com/articles/ten-usability-heuristics/

**Matiz (Krug) — "Clarity trumps consistency":** si un pequeño quiebre de consistencia (NN#4) hace
algo **mucho más claro**, elegí la claridad. La consistencia sirve al entendimiento, no es dogma.
Y el desempate de toda decisión de UX: si algo **te hace pensar** aunque sea un segundo, está mal.

---

## 2. Laws of UX (psicología aplicada — subconjunto de mayor palanca)
- **Jakob's Law** — la gente espera que tu app funcione como las demás que ya usa → seguí convenciones (⌘K, calendario, iconografía estándar).
- **Fitts's Law** — tiempo de alcance ∝ distancia/tamaño → targets grandes y cercanos; acciones primarias visibles y amplias.
- **Hick's Law** — más opciones = más tiempo de decisión → reducir/agrupar opciones; defaults.
- **Doherty Threshold** — productividad se dispara con respuesta <400ms → feedback optimista inmediato; skeletons; no bloquear.
- **Miller's Law** — ~7±2 en memoria de trabajo → **chunking**: agrupar campos/nav en bloques.
- **Aesthetic-Usability Effect** — lo bonito se percibe como más usable → el pulido (tokens, spacing) sí importa.
- **Peak-End Rule** — la experiencia se juzga por el pico y el final → cuidar momentos clave (checkout, confirmaciones) y el cierre.
- **Postel's Law (robustez)** — aceptá inputs flexibles, producí salida conservadora → **forgiving formats** (teléfonos/fechas con o sin formato).
- **Tesler's Law** — la complejidad es irreducible: alguien la absorbe → que la absorba el sistema, no el usuario.

Fuente: https://lawsofux.com/

---

## 3. Apple — Human Interface Guidelines
Pilares (iOS): **Clarity** (legibilidad, iconografía nítida, foco en el contenido), **Deference** (la UI cede
ante el contenido; cromo mínimo), **Depth** (jerarquía visual + transiciones que dan sentido de lugar).
Principios de diseño HIG: **Aesthetic Integrity**, **Consistency**, **Direct Manipulation** (manipular
directo el contenido, con resultado inmediato), **Feedback**, **Metaphors**, **User Control**.
Además foundations: **Accessibility** y **Privacy** como base, no extras.
→ *Aplicar*: manipulación directa + feedback inmediato (optimistic UI); deferencia (que el dato mande, cromo
sobrio); control del usuario (undo). Fuente: https://developer.apple.com/design/human-interface-guidelines

---

## 4. Google — Material Design 3
Principios: adaptable, personal y expresivo, con **accesibilidad** transversal.
Guía de interacción de alto valor para auditar:
- **States** — todo control interactivo tiene estados definidos (enabled/hover/focus/pressed/disabled/error).
- **Motion** — duración + easing con propósito (transición que orienta, no decora); respetar `reduced-motion`.
- **Elevation / tonal** — jerarquía por superficie/tono, coherente en light/dark.
- **Adaptive layout** — responsive real por breakpoints; contenido ancho scrollea en su contenedor.
- **Color roles** — color semántico por rol (no hex sueltos).
→ *Aplicar*: estados completos en controles; motion con `prefers-reduced-motion`; color por token/rol.
Fuente: https://m3.material.io/foundations

---

## 5. GOV.UK — Design Principles (referencia de oro en servicios)
1. Start with user needs. 2. Do less. 3. Design with data. 4. **Do the hard work to make it simple.**
5. Iterate. Then iterate again. 6. This is for everyone (accesibilidad/inclusión). 7. Understand context.
8. Build services, not websites. 9. **Be consistent, not uniform.** 10. Make things open.
→ *Aplicar*: #4 (absorber complejidad por el usuario), #6 (a11y no negociable), #9 (patrones compartidos
con flexibilidad). Fuente: https://www.gov.uk/guidance/government-design-principles

---

## 6. Sistemas de compañías (para SaaS/admin/enterprise)
- **Shopify Polaris** — crafted (calidad en el detalle), considerate (pensar el contexto/errores),
  empowering (que el usuario logre su tarea con confianza); **content**: claro, conciso, consistente; voz
  activa; los botones dicen exactamente qué pasa. Fuente: https://polaris.shopify.com
- **IBM Carbon** — open, inclusive, consistent; design tokens + patrones reutilizables a escala.
  Fuente: https://carbondesignsystem.com
- **Microsoft Fluent 2** — coherente multiplataforma, accesible, con design tokens y estados.
  Fuente: https://fluent2.microsoft.design

---

## 7. Accesibilidad — WCAG (POUR) + criterios clave para apps
Cuatro principios: **Perceivable, Operable, Understandable, Robust**.
Criterios de mayor retorno al auditar:
- **1.4.3 Contrast (mínimo)** — texto AA 4.5:1 (3:1 grande). *Check en light Y dark.*
- **2.1.1 Keyboard** — todo operable por teclado.
- **2.4.7 Focus Visible** — foco de teclado siempre visible.
- **4.1.2 Name, Role, Value** — controles con nombre/rol accesible (labels, `aria-*`).
- **4.1.3 Status Messages** — cambios de estado anunciados a lectores (`role="status"`/`aria-live`).
Fuente: https://www.w3.org/WAI/WCAG22/quickref/

---

## 8. Fixes canónicos (primitivas fix-once → arregla-N)
Mapa problema→técnica→heurística. Estas son las reutilizables entre proyectos.

| Problema típico | Técnica (nombre) | Heurística |
|---|---|---|
| Borrado sin retorno / "Deshacer" falso | **Optimistic UI + Undo** (soft-delete + restaurar) | NN#3, Apple User Control |
| Fallo de carga se ve como "sin datos" | **UI state handling** (loading/error+Reintentar/vacío) — hook `useAsyncData` + `<AsyncBoundary>` | NN#1, NN#9 |
| Pestaña bloqueada por popup (checkout/PDF/descarga) | Abrir ventana **síncrona en el gesto** (`openResolvedUrl`) antes del `await` | NN#1 |
| Formatos inconsistentes (moneda/fecha) | **Formatter único** (`formatMXN`, etc.) | NN#4 |
| Hex hardcodeado rompe dark mode | **Design tokens semánticos** soft/strong + override dark | NN#4, Material color roles |
| Buscador decorativo | **Command palette ⌘K** | NN#6, NN#7, Jakob |
| Lista para dato temporal | **Match al modelo mental** (calendario/kanban/árbol) | NN#2 |
| Submit inválido permitido | **Error prevention** (validación inline, disable, guardas) | NN#5 |
| Input acepta basura (letras en teléfono, `@`/`*` en nombre, texto donde va número) | **Constrain al dominio del campo**: `inputMode`/`type` correcto + patrón/filtrado + normalizar (forgiving) | NN#5, Postel |
| `max`/`maxLength` "advisory" que se puede exceder (ej. "máx 8" pero teclea más) | **Enforzar límites de verdad** (validar en submit; `max` de `type=number` NO capea el tecleo) | NN#5 |
| Todo el detalle de golpe | **Progressive disclosure** (panel/expand bajo demanda) | NN#8, Miller |
| Form de crear/editar que se despliega inline y empuja la lista | **Modal / dialog focalizado** (o side sheet) con focus-trap, `aria-modal`, Esc y restaurar foco | NN#1, NN#8, Apple/Material dialogs |
| Detalle de fila (multi-tab / form / matriz) que se expande **inline hacia abajo** empujando la lista (layout shift, scroll fila↔detalle, sin límite claro) | **Side sheet / drawer** deslizante que preserva la lista y enfoca el registro; `role="dialog"`+`aria-modal`+focus-trap+foco restaurado. Expandable-row solo para quick-view de 1-2 datos | NN#1, NN#8, Apple/Material dialogs |
| Confirms modales para todo | **Undo optimista** > confirm (confirm solo si es irreversible) | NN#3 |
| Spinner mudo para lectores | `role="status"` / `aria-live` | WCAG 4.1.3 |
| En página profunda no sé dónde estoy / cómo volver | **Trunk test**: persistent nav + nombre de página + "you are here"/breadcrumbs + búsqueda | NN#1, Krug |
| Nombre de la página ≠ link que la abrió | **Nombrar cada página** y que coincida con el link cliqueado | NN#2, Krug |
| Happy talk / muro de instrucciones que nadie lee | **Omit needless words**: matar relleno promocional + hacer auto-explicativo | NN#8, Krug 3ª ley |
| CTA/opción que obliga a "pensar" qué hace o dónde va | **Auto-evidente**: label/affordance que quita el signo de interrogación | Krug 1ª ley, NN#6 |
| Ventana nueva / SPA rompe el botón Back; sin salida clara | **No romper el Back** (feature más usada) + **Home siempre visible** como salida de emergencia | Krug, NN#3 |
| Links/botones no se ven clickeables (flat design plano) | **Affordance de clickabilidad** (links parecen links) + preservar estado **visitado/no-visitado** | Krug, Norman signifiers |
| Label ambiguo → el usuario no sabe a dónde lleva (pogo-sticking) | **Trigger words / information scent**: nombrar con las palabras del objetivo del usuario | Krug (Pirolli/Card), NN#2 |
| Onboarding/tarea larga que se abandona por falta de momentum | **Endowed progress + Zeigarnik**: barra que arranca adelantada, checklist de setup | Behavioral, goal-gradient |
| Demasiada fricción/decisión en la acción deseada | **Subir la Ability + default útil** (Fogg B=MAP; nudge), no empujar motivación | Fogg, Hick, nudge |
| Cancelar/darse de baja es más difícil que suscribirse (roach motel) | **Simetría de esfuerzo**: cancelar tan fácil como entrar; sin confirmshaming | Deceptive patterns, NN#3 |
| Escasez/urgencia falsa, costos ocultos, renovación silenciosa | **Honestidad**: escasez real; costos y renovación visibles ANTES de pagar | Deceptive patterns, NN#1 |

---

## 9. Checklist accionable (verificable en la app corriendo) — top ~20
Recorré cada pantalla:
1. Toda acción destructiva es **reversible (Undo)** o **confirmada**; nada de "Deshacer" que no restaura.
2. Cada carga tiene **loading**, y el **error** se ve distinto del **vacío** y trae **Reintentar**.
3. Ninguna acción es **silenciosa**: éxito y error dan feedback (toast/estado).
4. Abrir pestañas externas (pago/PDF/descarga) NO lo mata el popup-blocker (síncrono en el gesto).
5. **Un** formato de moneda/fecha en toda la app.
6. Colores por **token semántico**; se ve bien en light **y** dark.
7. Listas largas tienen **búsqueda/filtro/orden**; hay **bulk actions** donde aplica.
8. Hay **⌘K** o navegación rápida; no un buscador falso.
9. La representación **matchea el modelo mental** (calendario para fechas, etc.).
10. Formularios: **validación inline**, defaults sensatos, submit deshabilitado si inválido, forgiving formats.
11. Estados de control completos (hover/focus/pressed/disabled/error).
12. Foco de teclado **visible**; todo operable por teclado.
13. Controles con **label/rol** accesible; spinners con `aria-live`.
14. **Contraste AA** en ambos temas.
15. Contenido ancho scrollea en **su** contenedor (el body no scrollea de lado).
16. Copy en el **idioma del producto**, sin restos de plantilla/branding ajeno.
17. Nombres por lo que la persona reconoce, no por cómo está construido el sistema.
18. Empty-states con **CTA** (qué hacer), no texto muerto.
19. `prefers-reduced-motion` respetado.
20. Sin **claims falsos** (p.ej. "cifrado" cuando no lo está) — honestidad en el copy.
21. Los botones **Crear/Nuevo/Editar abren un modal** (o side sheet) con el formulario, no un form
    que se despliega inline empujando la lista. Excepciones: edición de 1 campo (inline) o flujo
    multi-paso (página propia). El modal debe tener focus-trap, `aria-modal`, cerrar con Esc y
    devolver el foco al disparador.
22. **Cada input acota su dominio y NO acepta basura**: teléfono sólo dígitos/`+`/`-`/espacio
    (`inputMode="tel"`), nombre sin `@ * /` ni números fuera de lugar, email `type=email`, número
    `inputMode=numeric` con `min`/`max`, monto ≥0, `%` entre 0 y 100. Con feedback inline
    (`aria-invalid`) y submit deshabilitado si es inválido.
23. **Los límites se enforzan de verdad**, no son advisory: probá teclear MÁS del máximo (ej. "máx 8
    caracteres") y valores fuera de rango — el `max` de `<input type=number>` no capea el tecleo y
    `maxLength` no aplica a number, así que debe haber validación real en submit. Forgiving format:
    aceptar variaciones razonables y normalizar (no rechazar por un espacio o un guión).
24. **Trunk test** (Krug): parate en una página profunda al azar — ¿se ve el **site ID**, el
    **nombre de la página** (y coincide con el link que la abrió), la **sección/"you are here"**
    (estado activo/breadcrumbs) y la **búsqueda**? Si no me oriento en 3 s, falla.
25. **Auto-evidente / omit needless words** (Krug 1ª y 3ª ley): cada CTA y opción se entiende sin
    pensar (sin signos de interrogación); el copy es escaneable (headings, párrafos cortos,
    viñetas, términos clave resaltados) y **sin happy talk ni instrucciones que nadie lee** —
    lo que requería instrucción se rediseña para ser auto-explicativo.
26. **Reservoir of goodwill** (Krug): recorré la tarea buscando **fugas** — precios/envío/soporte
    escondidos, formato rígido que castiga al usuario, pedir datos innecesarios, hype en el camino,
    diseño amateur. Cada fricción drena la reserva; ante la duda, disculparse/ayudar a recuperarse.
27. **Back nunca se rompe** (Krug): el botón atrás es la función más usada; probá que funcione tras
    cada acción y que no se abran ventanas/pestañas nuevas gratis. **Home siempre visible y obvio**
    como salida de emergencia. La nav persistente aparece en toda página (versión mínima en forms).
28. **Clickabilidad obvia** (Krug/Norman): links parecen links y botones parecen botones — el flat
    design no debe borrar las pistas; **links visitados ≠ no visitados**; targets táctiles no
    diminutos en mobile.
29. **Trigger words + búsqueda** (Krug): los labels usan las **palabras del objetivo del usuario**
    (information scent), no jerga interna; y hay **buscador disponible y simple** para los usuarios
    search-dominant.
30. **Sin deceptive/dark patterns** (ética conductual): cancelar/rechazar es tan fácil como
    aceptar (no **roach motel**), sin **confirmshaming**, costos y renovaciones visibles **antes**
    de pagar, consentimiento **simétrico** y sin casillas-trampa de doble negación. Extiende #20.
31. **Palancas conductuales al servicio del usuario** (no en su contra): defaults útiles, progreso
    endógeno (endowed progress/Zeigarnik) en onboarding, prueba social y escasez **reales**; subir
    la *ability* (menos fricción/pasos) antes que empujar motivación (Fogg B=MAP).

---

## 10. Fuentes
- NN/g 10 heurísticas — https://www.nngroup.com/articles/ten-usability-heuristics/
- Laws of UX — https://lawsofux.com/
- Apple HIG — https://developer.apple.com/design/human-interface-guidelines
- Material Design 3 — https://m3.material.io/foundations
- GOV.UK Design Principles — https://www.gov.uk/guidance/government-design-principles
- Shopify Polaris — https://polaris.shopify.com
- IBM Carbon — https://carbondesignsystem.com
- Microsoft Fluent 2 — https://fluent2.microsoft.design
- WCAG 2.2 quickref — https://www.w3.org/WAI/WCAG22/quickref/
- Steve Krug, *Don't Make Me Think, Revisited* — ver `references/books.md` §B (3 leyes, trunk test, reservoir of goodwill, testing barato)
- Diseño conductual / Psychology of UX — ver `references/books.md` §E. Fuentes: Fogg Behavior Model https://behaviormodel.org · Nir Eyal *Hooked* · Cialdini *Influence* · Thaler & Sunstein *Nudge* · deceptive/dark patterns https://www.deceptivepatterns.org · FTC click-to-cancel
