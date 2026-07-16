# Craft visual — lo que delata amateurismo a 3 metros

El aesthetic-usability effect es real: lo pulido se PERCIBE más usable. Esto NO es rediseñar
(eso es ui-ux-pro-max) — es detectar y corregir la falta de oficio dentro del design system
existente. Canon táctico: **Refactoring UI** (Wathan & Schoger) — visual para developers.

---

## A. Espaciado — la fuente #1 de "se ve raro y no sé por qué"
- **Escala fija de espaciado** (múltiplos de 4px: 4/8/12/16/24/32/48/64) — TODO margin/padding/
  gap sale de la escala. Valores sueltos (13px, 22px, 7px) = el olor a amateur más común.
- **Empezar con demasiado espacio y quitar** (Refactoring UI), no empezar apretado y sumar.
  Densidad es una decisión (admin-tables §A), no un accidente.
- **El espacio agrupa** (Gestalt proximidad): espacio DENTRO de un grupo < espacio ENTRE
  grupos. Label pegado a SU campo, separado del campo anterior. Si hace falta un borde para
  separar, probablemente falta espacio.
- Padding proporcional: botones/inputs con padding horizontal ≈ 2× el vertical; contenedores
  consistentes entre sí (todas las cards mismo padding).

## B. Jerarquía tipográfica — tamaño es el peor diferenciador
- **No confíes solo en el tamaño** (Refactoring UI): jerarquía = peso (400/500/600/700) +
  color (texto primario / secundario / terciario) + tamaño. Dos pesos y tres colores rinden
  más que seis tamaños.
- **Escala de tipo fija** (ej. 12/14/16/18/20/24/30/36) — no 15px, 17px, 19px sueltos.
- Texto secundario: **gris más claro, NO más chico** (bajar tamaño castiga legibilidad; bajar
  contraste jerarquiza sin castigar — dentro de AA).
- Line-height inverso al tamaño: cuerpo ~1.5; títulos grandes ~1.1-1.2 (título con line-height
  de cuerpo = flota raro).
- En UI, evitar centrado para texto >2 líneas; números alineados a la derecha con tabular
  figures (admin-tables §A); **no all-caps largo** (tracking + solo etiquetas cortas).

## C. Color — grises y semánticos con sistema
- **Los grises nunca son gray puro** (Refactoring UI): saturá levemente hacia el hue de marca
  (grises fríos/cálidos coherentes). Escala de 8-10 grises definida — no `#eee`, `#f5f5f5`,
  `#fafafa` inventados por pantalla.
- Color semántico ESCASO (Few): rojo/verde/ámbar solo significan estado — un dashboard arcoíris
  no jerarquiza nada. Si todo grita, nada grita (Von Restorff).
- **No confíes solo en el color** para significado (daltonismo, WCAG 1.4.1): estado = color +
  icono/texto.
- Texto sobre imagen/color: overlay o scrim, no esperanza.

## D. Profundidad y bordes
- **Sistema de elevación de 3-5 niveles** (flat / raised / overlay / modal) con sombras
  definidas por nivel — no una sombra distinta por componente. Sombra = jerarquía de foco, no
  decoración.
- **Sombras de dos capas** (Refactoring UI): una ambiental difusa + una de contacto corta =
  profundidad real; una sola sombra negra dura = sticker.
- Menos bordes (Refactoring UI): antes de agregar un borde probá espacio, sombra suave o fondo
  levemente distinto — UI con borde en todo = jaula.
- Radios consistentes por familia (inputs/cards/modales cada uno el suyo, de una escala de 2-3
  valores) — mezclar 4px/8px/16px al azar se nota.
- Z-index con escala nombrada (dropdown < sticky < overlay < modal < toast) — el bug del
  dropdown DEBAJO del modal es síntoma de z-index ad-hoc (999, 9999, 99999).

## E. Alineación y óptica
- **Todo se alinea con algo**: cada borde de texto/control comparte eje con otro elemento. Un
  elemento 3px fuera del eje se percibe aunque no se identifique ("algo se ve mal").
- Alineación óptica > matemática: iconos de play, chevrones y badges a veces necesitan 1-2px
  de compensación; el centrado vertical de texto con line-height engaña.
- Iconos de UNA familia y UN tamaño de grid (16/20/24) — mezclar outline con filled o pesos
  distintos delata collage.
- Imágenes/avatars con aspect-ratio fijo y object-fit — nada estirado ni saltando al cargar
  (CLS, patterns §B).

## F. Motion con física
- Duraciones por distancia/tamaño (Material): micro 100-200 ms, transiciones locales 200-300 ms,
  pantalla completa 300-400 ms. >400 ms estorba SIEMPRE en UI de trabajo.
- **Easing nunca lineal**: entrar = decelerar (ease-out), salir = acelerar (ease-in); spring
  sutil para lo táctil. Linear = mecánico.
- El motion ORIENTA (de dónde viene el panel, a dónde se fue el item) — animación que no
  explica nada es decoración que retrasa (flows §B, `prefers-reduced-motion` obligatorio).

## G. Checklist de craft (auditable por pantalla)
1. Todos los spacing/gaps salen de la escala 4/8 — cero valores sueltos.
2. ≤2 familias tipográficas; escala de tamaños fija; jerarquía por peso+color, no solo tamaño.
3. Secundario = más claro, no más chico; line-height correcto por tamaño.
4. Grises de UNA escala; semánticos escasos y con icono/texto de refuerzo.
5. Sombras/elevación de un sistema de niveles; z-index nombrado (nada de 9999).
6. Radios e iconos de una sola familia/escala.
7. Nada desalineado: cada elemento comparte eje con otro.
8. Motion 100-400 ms con easing correcto y propósito; reduced-motion respetado.

Cada ítem violado sistémicamente = hallazgo [L]-[M] con fix de TOKEN/escala (fix-once), no de
pantalla.

## Fuentes
- Wathan & Schoger — *Refactoring UI* — https://www.refactoringui.com/
- Butterick — *Practical Typography* — https://practicaltypography.com/
- Material — elevation/motion — https://m3.material.io/styles/elevation/overview ·
  https://m3.material.io/styles/motion/overview
- Apple HIG — layout/typography — https://developer.apple.com/design/human-interface-guidelines/layout
