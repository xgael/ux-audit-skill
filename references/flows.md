# Flujos en el tiempo — onboarding, microinteracciones, journey mapping

El eje temporal de la UX: el **primer uso** (onboarding), el **momento a momento**
(microinteracciones) y el **viaje completo** (journey mapping). `heuristics.md` audita
pantallas; esto audita lo que pasa ENTRE ellas y a lo largo del tiempo.

---

## A. Onboarding — del signup al "aha"

**Conceptos clave**:
- **Aha moment** = la primera vez que el usuario SIENTE el valor del producto (emocional).
  **Activation** = la primera vez que lo obtiene de verdad (conductual). El onboarding existe
  para acortar el camino signup → aha → activation. Promedio SaaS: solo ~36% de los signups
  llega a activarse — el onboarding es donde se pierde a 2 de cada 3.
- Clásicos: Facebook "7 amigos en 10 días", Slack "2.000 mensajes del equipo", Dropbox "primer
  archivo en una carpeta". Encontrá el del producto: ¿qué acción temprana separa a los que se
  quedan de los que se van? Diseñá TODO el primer uso hacia esa acción.

**Reglas de diseño** (con datos):
1. **Learn by doing >> tour pasivo**: tours donde el usuario HACE la acción real (no solo
   "Siguiente") completan +123% y correlacionan con activación. El tour de tooltips pasivo se
   cierra sin leer (los usuarios "se las arreglan", Krug).
2. **Si hay tour: ≤4 pasos.** Con 4 pasos completa ~40%; con 5, ~21% — cada paso extra mata a la
   mitad. Un tour largo es señal de UI no auto-evidente: arreglá la UI, no alargues el tour.
3. **Time-to-value mínimo**: recortá TODO lo que se interponga entre signup y el aha (Fogg:
   ability). Pedí datos DESPUÉS de mostrar valor, no antes (goodwill). Defaults + datos de
   ejemplo para que la primera pantalla nunca esté vacía de valor.
4. **Checklist de setup con endowed progress**: 3-5 tareas, arrancando con 1-2 ya completadas
   ("Cuenta creada ✓") — goal-gradient + Zeigarnik empujan a terminar. Cada tarea = una acción
   real en el producto, no "mirá este video".
5. **Empty states como onboarding** (`writing.md` §4): el first-use empty ES el tour — qué es
   esta sección + CTA para crear lo primero.
6. **Progressive disclosure de features**: no enseñes las 20 features el día 1; revelá la
   feature avanzada cuando el contexto la vuelva relevante (tooltip contextual la PRIMERA vez
   que el usuario pisa esa zona, no un carrusel al inicio).
7. **Nunca bloquees al usuario que ya sabe**: tour siempre saltables ("Omitir"), re-accesible
   después (menú ayuda), y jamás modal de bienvenida de 6 pantallas sin escape.

**Al auditar**: ¿cuántos pasos/campos hay entre signup y la primera unidad de valor real?
¿el primer login aterriza en pantalla vacía muerta? ¿el tour es pasivo, largo, no saltable?
Cada uno = hallazgo [M/H] según cuánto usuario nuevo pierde.

---

## B. Microinteracciones — Dan Saffer (*Microinteractions: Designing with Details*)

Una microinteracción = un momento de producto que cumple UNA tarea (like, toggle, pull-to-refresh,
guardar, mute). "Los detalles no son detalles; SON el diseño." Estructura de 4 partes:

1. **Trigger** — lo que la inicia. De usuario (tap, hover, teclear) o de sistema (llegó un
   mensaje, terminó el export). El trigger manual debe ser **descubrible** (signifier) y
   consistente: el mismo trigger siempre dispara lo mismo.
2. **Rules** — qué pasa exactamente: qué se puede y qué no durante la interacción, en qué orden,
   qué estados toca. Las reglas absorben la complejidad (Tesler), no la delegan al usuario.
   **Usá los datos que ya tenés** para no preguntar (Saffer: "bring the data forward").
3. **Feedback** — cómo el usuario SABE qué pasó: visual/sonoro/háptico. Mínimo necesario, máximo
   claro. El feedback ilumina las reglas: si el toggle tarda, muéstralo en tránsito; si falló,
   revertí visiblemente. Nada silencioso (NN#1) y nada que grite (una animación de 2 s para un
   like = fricción).
4. **Loops & Modes** — qué pasa con el tiempo y el contexto: ¿la 2ª vez es igual que la 1ª?
   (long loops: la microinteracción puede recordar — "la última vez elegiste X"); ¿hay un modo
   especial que cambia el comportamiento? Los **modos son fuente de error** (slip de Norman:
   mismo gesto, otro resultado) — minimizalos y hacelos visualmente inconfundibles.

**Reglas de calidad**:
- **Duración**: micro-animaciones 150-400 ms (más = estorba, menos = no se percibe); easing con
  propósito (Material motion); respetar `prefers-reduced-motion`.
- **La microinteracción repetida 50 veces/día** merece más pulido que la pantalla que se ve una
  vez (frecuencia × persistencia — misma fórmula de severidad).
- Empezá **desde cero visible**: estado presionado inmediato (<100 ms, `patterns.md` §B),
  optimistic UI, undo en vez de confirm.
- Señal de hallazgo: acción frecuente con 0 feedback, feedback genérico ("Operación exitosa"),
  animación decorativa que retrasa, modo invisible que cambia lo que hace un botón.

---

## C. Journey mapping — auditar el viaje, no la pantalla

Formato ligero (NN/g) para las 2-3 tareas críticas ya identificadas en Fase 1. Un journey map =
**actor + escenario** con expectativas + **fases** + por fase: **acciones / pensamientos /
emociones** + **oportunidades**.

**Versión de auditoría (tabla por tarea crítica)**:

| | Fase 1 | Fase 2 | … |
|---|---|---|---|
| **Acciones** (qué hace, pantalla por pantalla) | | | |
| **Preguntas/dudas** (¿qué se pregunta acá? — cada duda = fricción, Krug) | | | |
| **Emoción** (😀/😐/😖 — dónde se frustra) | | | |
| **Evidencia** (`file:line`, hallazgos del walkthrough) | | | |
| **Oportunidades** (fix por patrón) | | | |

**Cómo usarlo en la skill**:
1. El **cognitive walkthrough** (SKILL.md Fase 1.4) produce la fila de acciones y dudas; el
   journey map las ordena en el tiempo y les suma la capa emocional.
2. Buscá los **momentos que definen el juicio** (peak-end): el peor pico de fricción y el final
   de la tarea. Un checkout impecable con confirmación confusa se recuerda como malo. Cerrá
   fuerte: éxito concreto + siguiente paso + undo.
3. Marcá **cambios de contexto** (web→email→web, app→PDF→app): ahí se pierde estado, se rompe
   el Back, el usuario re-teclea (excise de Cooper). Cada handoff = punto de fuga.
4. Detectá **fricción acumulada**: hallazgos [L] individuales que en la MISMA tarea suman un
   [H] de viaje (3 formatos distintos + 2 confirms + 1 re-login = tarea agotadora aunque cada
   pantalla "apruebe").
5. Las **oportunidades** se agrupan por patrón sistémico, como todo en esta skill.

**Cuándo NO**: producto de 3 pantallas o auditoría de un solo módulo — el walkthrough basta;
el mapa formal es overhead. Usalo cuando la tarea cruza ≥3 pantallas o ≥2 contextos.

## Fuentes
- Dan Saffer, *Microinteractions* — https://www.oreilly.com/library/view/microinteractions/9781449342760/
- NN/g Journey Mapping 101 — https://www.nngroup.com/articles/journey-mapping-101/
- Datos de tours/activación — https://www.appcues.com/blog/aha-moment-guide ·
  https://www.chameleon.io/blog/successful-user-onboarding
- Samuel Hulick, UserOnboard (teardowns) — https://www.useronboard.com/
- ProductLed, aha moments — https://productled.com/blog/how-to-use-aha-moments-to-drive-onboarding-success
