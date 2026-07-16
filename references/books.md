# Canon de diseño — libros clave y sus principios accionables

El núcleo del conocimiento sobre diseño y **cómo la gente procesa información de forma
limpia y clara**. Cada libro: principios verbatim (nombre propio) + cómo aplicarlos al
auditar/arreglar UI. Ordenado por la pregunta del usuario: primero claridad/procesamiento
de información, luego usabilidad, cognición, tipografía/IA y patrones.

---

## A. Claridad y procesamiento de información

### The Visual Display of Quantitative Information — Edward Tufte
(+ *Envisioning Information*, *Beautiful Evidence*)
- **Data-ink ratio**: maximizar la tinta que representa datos; borrar todo lo demás.
- **Chartjunk**: eliminar decoración, degradados, 3D falso, rejillas pesadas.
- **Small multiples**: repetir un mismo gráfico chico para comparar series (no un gráfico saturado).
- **Sparklines**: mini-gráficos del tamaño de una palabra, inline con el texto/tabla.
- **Alta densidad de datos** + **integridad gráfica** (sin "lie factor": el tamaño visual = el dato).
- → *Aplicar*: quitar pixeles que no son datos de dashboards/tablas; ejes/leyendas legibles; una idea por gráfico.

### Show Me the Numbers / Information Dashboard Design — Stephen Few
- Un dashboard **cabe en una pantalla**, sin scroll, escaneable de un vistazo.
- **Reducir píxeles que no son datos**; declutter; color **semántico y escaso** (no arcoíris).
- Elegir el **gráfico correcto** para la relación (tendencia→línea, comparación→barra, parte-todo→con cuidado).
- Siempre dar **contexto** al número (vs meta, vs periodo previo).
- → *Aplicar*: KPIs con comparación, no números sueltos; paleta muted semántica; el gráfico correcto por dato.

### Information Architecture / Information Anxiety — Richard Saul Wurman
- Acuñó **"Information Architecture"**. La **ansiedad de información** = brecha entre dato y entendimiento.
- **LATCH**: sólo hay **5 formas de organizar** cualquier información — **L**ocation, **A**lphabet, **T**ime,
  **C**ategory, **H**ierarchy. Elegí la que matchea la tarea (reservas→Time/calendario; catálogo→Category).
- → *Aplicar*: antes de listar, preguntá "¿por cuál de LATCH lo busca el usuario?" y organizá por eso.

### Information Architecture (polar bear) — Rosenfeld, Morville & Arango
- Cuatro sistemas: **organización, etiquetado, navegación, búsqueda** → **findability**.
- Etiquetas en el idioma del usuario; navegación que revela dónde estoy y a dónde puedo ir.
- → *Aplicar*: labels reconocibles, nav con estado activo/breadcrumbs, búsqueda (⌘K) para findability.

### Information Visualization: Perception for Design — Colin Ware
- **Atributos preatentivos** (color, tamaño, orientación, posición) se procesan en <250 ms, sin esfuerzo.
- Codificá la variable **más importante** con un atributo preatentivo; no dependas de leer texto.
- → *Aplicar*: severidad/estado por color+forma (chip, franja), no solo por palabra; el dato clave "salta".

### The Laws of Simplicity — John Maeda
- 10 leyes; la 1ª: **Reduce** (la forma más simple = restar lo obvio, sumar lo significativo).
- **Organize** (agrupar hace que muchos parezcan pocos), **Time** (ahorrar tiempo se siente simple).
- → *Aplicar*: quitar campos/opciones que no aportan; agrupar; feedback rápido (<400 ms, Doherty).

---

## B. Usabilidad e interacción

### The Design of Everyday Things — Don Norman (la biblia)
- **Affordances** (qué acciones permite un objeto) y **signifiers** (pistas visibles de esa acción).
- **Mapping**: relación natural control↔efecto (que el layout espeje el mundo).
- **Feedback** inmediato de cada acción; **constraints** (físicos/lógicos/culturales) para prevenir error.
- **Modelo conceptual** claro; **discoverability**: se ve qué se puede hacer y cómo.
- **Golfo de ejecución** (¿cómo lo hago?) y **golfo de evaluación** (¿qué pasó?) — cerrarlos con signifiers y feedback.
- **Errores**: *slips* (acción correcta mal ejecutada) vs *mistakes* (plan equivocado) → **prevenir + permitir recuperar** (undo, forcing functions).
- → *Aplicar*: botones que parecen botones; feedback en toda acción; undo; validaciones que previenen; estado del sistema visible.

### Don't Make Me Think — Steve Krug (usabilidad web, sentido común)
El libro más práctico del canon. Tesis: la usabilidad = **quitar signos de interrogación** de la
cabeza del usuario. "Cada signo de interrogación suma a la carga cognitiva y distrae de la tarea."

**Las 3 leyes de Krug** (verbatim):
1. **"Don't make me think!"** — la pantalla debe ser **auto-evidente** (obvia sin pensar); si no se
   puede, al menos **auto-explicativa** (obvia con un vistazo). Es el criterio de desempate: si algo
   te hace pensar aunque sea un segundo, está mal.
2. **"No importa cuántas veces tenga que clickear, mientras cada click sea una decisión
   irreflexiva e inequívoca"** — el problema no es el número de clicks, es el **esfuerzo mental**
   por click. Un camino largo de decisiones obvias > un camino corto de decisiones ambiguas.
3. **"Deshazte de la mitad de las palabras de cada página; luego deshazte de la mitad de lo que
   queda"** — omití palabras innecesarias. Los dos grandes culpables: **happy talk** (texto
   auto-promocional de relleno) y **instrucciones** (nadie las lee; hacé la cosa auto-explicativa
   en su lugar). *Happy talk must die. Instructions must die.*

**Cómo la gente usa la web de verdad** (no como creemos):
- **Escanea, no lee** — mira la página, escanea algo de texto, clickea el primer link que le sirve.
  Uso web = "una valla publicitaria pasando a 90 km/h", no un libro.
- **Satisfices** (Herbert Simon: satisfy+suffice) — elige la **1ª opción razonable**, no la óptima.
- **Se las arregla** (*muddle through*) — no entiende cómo funciona; usa lo que sea que funcione y
  sigue. Por qué escanea: está apurada, sabe que no necesita todo, y es buena escaneando.

**Diseñar para el escaneo** (los 6 movimientos):
1. **Aprovechá las convenciones** — "las convenciones son tus amigas". Innová solo cuando tengas
   algo demostrablemente mejor; si no, usá el patrón que la gente ya conoce (Jakob).
2. **Jerarquía visual clara** — más importante = más prominente; lo relacionado se ve relacionado
   (agrupado); lo anidado muestra qué es parte de qué.
3. **Dividí la página en áreas** claramente definidas (el ojo sabe dónde mirar según su tarea).
4. **Hacé obvio qué es clickeable** — que los controles se vean como controles (affordance/Norman).
5. **Eliminá distracciones** — los 2 culpables: cosas que parpadean/se mueven, y el **desorden**
   (clutter). "El ruido visual compite con el contenido."
6. **Formateá para escanear** — muchos encabezados, párrafos cortos, listas con viñetas, resaltar
   términos clave.

**Navegación — el trunk test**: si te metieran en la cajuela de un auto, te movieran y te soltaran
en una página cualquiera del sitio, ¿podrías orientarte? En **cualquier** página deben verse:
**Site ID** (¿de quién es?), **nombre de la página** (¿en qué página estoy? y **el nombre coincide
con el link que cliqueé**), **secciones/nav primaria**, **nav local** (opciones de este nivel),
**"you are here"** (dónde estoy en el esquema — estado activo/breadcrumbs) y **búsqueda**.

**Home page / primer contacto** — debe responder de un vistazo 4 preguntas: **¿Qué es esto?
¿Qué puedo hacer acá? ¿Qué tienen acá? ¿Por qué debería estar acá y no en otro lado?** — más
**"¿por dónde empiezo?"**. Necesita un **tagline** claro que comunique la propuesta de valor
(las home mueren por las peleas de territorio → clutter).

**Reservoir of goodwill** (reserva de buena voluntad): cada usuario llega con una reserva; cada
fricción la **drena**, cada acierto la **rellena**. Si se vacía, se va (o piensa peor de vos).
- **Drenan**: esconder lo que quiero (precios, envío, teléfono de soporte); castigarme por no
  hacer las cosas a tu manera (formato rígido de teléfono/tarjeta); pedirme datos que no necesitás;
  hype/sizzle en mi camino; diseño amateur que erosiona confianza.
- **Rellenan**: sé qué quiere hacer la gente y hazlo obvio; decime lo que quiero saber; ahorrame
  pasos; que se note el esfuerzo; anticipá mis preguntas y respondelas; comodidades (ej. versión
  imprimible); facilitá recuperarse del error; **ante la duda, disculpate**.

**Testing de usabilidad barato** (el capítulo más citado):
- **1 usuario testeado = 100% mejor que ninguno.** Testear pocos, temprano, seguido > muchos tarde.
- **3 usuarios, una mañana al mes** basta para encontrar los problemas más graves.
- Es **cualitativo**: encontrá los peores problemas, arreglalos, repetí. Reclutar usuarios
  perfectamente representativos está **sobrevalorado** (sirven "más o menos" representativos).
- **Focus group ≠ usability test.** No existe el "usuario promedio" → la respuesta a casi todo
  "¿cuál es mejor?" es **"depende"**; se sale del debate religioso **testeando**, no discutiendo.

**Mobile** (ed. *Revisited*): las mismas leyes aplican. La restricción de pantalla **obliga a
priorizar** (no metas todo con calzador; "responsive" no es excusa para amontonar). Cuidado con la
**tiranía de los targets diminutos** (áreas táctiles muy chicas). Mantené los signifiers aunque
recortes cromo.

**Más principios de alto valor** (ed. *Revisited*):
- **"Clarity trumps consistency"** (verbatim) — si un pequeño quiebre de consistencia hace algo
  **mucho más claro**, elegí la claridad. Es el matiz de Krug sobre NN#4: consistencia al servicio
  del entendimiento, no como dogma.
- **No rompas el botón Back** — "es la función más usada de la web; mientras el Back funcione, los
  errores no importan mucho". No abras ventanas/pestañas nuevas gratis (rompen el Back), y el link
  **Home es la salida de emergencia definitiva** → siempre visible y obvio.
- **Hacé obvio qué es clickeable** — links parecen links, botones parecen botones. El **flat design
  que borra las pistas** de clickabilidad hace pensar. **Preservá el estado visitado/no-visitado**
  de los links (le dice al usuario dónde ya estuvo).
- **Information scent / trigger words** (Pirolli & Card, "information foraging"; los usuarios son
  *informavores*) — un link que identifica claro su destino da **olor fuerte** y asegura que
  clickear acerca a la "presa"; labels ambiguos → **pogo-sticking** (entrar-salir a tientas). Nombrá
  con las **palabras del objetivo del usuario**, no con jerga interna.
- **Búsqueda siempre disponible** — hay usuarios **search-dominant** (van directo al buscador) y
  **link-dominant**; serví a ambos. Mantené el buscador simple (no lo satures de scopes/dropdowns).
- **Persistent navigation = 5 elementos** en toda página: **Site ID, secciones (nav primaria),
  utilities, búsqueda y Home**. En páginas de formulario, usá una **versión mínima** (Site ID +
  Home + utilities que ayuden) para no distraer de la tarea.
- **Primera impresión en milisegundos** — el juicio-relámpago de la gente predice su evaluación
  posterior; el *above the fold* importa para ese primer vistazo aunque después sí se scrollee.
- **Definición de usabilidad (Krug)**: "una persona de habilidad y experiencia **promedio (o
  incluso por debajo)** puede darse cuenta de cómo usar la cosa para su propósito, sin que sea más
  problema de lo que vale".
- **Testing — "recruit loosely, grade on a curve"**: no te obsesiones con reclutar usuarios
  perfectos; lo que buscás primero son los problemas que **cualquiera** encuentra (interfaz
  confusa). Testeá igual y pesá los hallazgos en consecuencia.

- → *Aplicar*: CTAs auto-evidentes y jerarquía visual fuerte; copy mínimo (matar happy talk e
  instrucciones); seguir convenciones **salvo cuando romperlas da más claridad** (clarity trumps
  consistency); correr el **trunk test** en páginas profundas (¿me oriento?); nombre de página =
  link clickeado; nunca romper el **Back** ni abrir ventanas gratis, Home siempre visible; links y
  botones **obviamente clickeables** (flat no borra pistas) con estado visitado/no-visitado; labels
  con **trigger words**; búsqueda siempre; landing que responde las 4 preguntas; auditar cada
  fricción como fuga del *reservoir of goodwill*; y validar dudas con **testing barato**, no debate.

### About Face — Alan Cooper
- **Diseño dirigido por objetivos** (goal-directed) + **personas**; diseñar para el objetivo, no la tarea.
- Eliminar **excise** (trabajo innecesario que el sistema impone al usuario).
- "La computadora hace el trabajo": defaults inteligentes, recordar, no re-preguntar.
- → *Aplicar*: prellenar/heredar datos (handoff), defaults, no exigir re-teclear.

### Web Form Design / Mobile First — Luke Wroblewski
- Minimizar campos; **labels top-aligned** (más rápidas de completar); **validación inline**.
- **Smart defaults** y **forgiving formats** (aceptar teléfono/fecha con o sin formato).
- **Mobile First**: empezar por la restricción chica obliga a priorizar contenido.
- → *Aplicar*: formularios cortos, validación en vivo, defaults, formatos tolerantes.

### Designing for Emotion — Aarron Walter
- **Jerarquía de necesidades de diseño**: funcional → confiable → usable → **placentero**.
- Personalidad y micro-momentos de deleite (sin sacrificar claridad).
- → *Aplicar*: una vez cubierto lo usable, sumar micro-interacciones/tono con mesura.

---

## C. Cognición y percepción

### Thinking, Fast and Slow — Daniel Kahneman
- **Sistema 1** (rápido, automático, intuitivo) vs **Sistema 2** (lento, esforzado).
- Diseñar para el Sistema 1: **cognitive ease**, reconocimiento, defaults, baja fricción.
- **Anclaje** y **aversión a la pérdida** afectan decisiones (precios, defaults, framing).
- → *Aplicar*: reducir carga cognitiva; reconocer en vez de recordar; framing honesto.

### 100 Things Every Designer Needs to Know About People — Susan Weinschenk
- La gente **escanea**; la **visión periférica** guía el foco; la memoria de trabajo aguanta ~**4 chunks**.
- **Progressive disclosure** reduce la carga; la atención es selectiva.
- → *Aplicar*: chunking, revelar detalle bajo demanda, jerarquía que dirige el ojo.

### Universal Principles of Design — Lidwell, Holden & Butler
- **Signal-to-noise ratio** (maximizar señal, minimizar ruido); **progressive disclosure**;
  **80/20** (el 20% de features = 80% del uso → priorizar); **chunking**; **consistency**; **mapping**;
  **performance load** (reducir carga cognitiva/física); **confirmation** para acciones destructivas.
- → *Aplicar*: subir la relación señal/ruido de cada pantalla; priorizar por 80/20.

### Gestalt (principios de agrupación perceptual)
- **Proximidad, Similitud, Región común, Cierre, Continuidad** → cómo el ojo agrupa.
- → *Aplicar*: agrupar campos/acciones relacionados con espacio/bordes/color; la estructura visual = la lógica.

---

## D. Tipografía y patrones

### The Elements of Typographic Style — Robert Bringhurst
- **Measure**: 45–75 caracteres por línea (66 ideal) para lectura cómoda.
- **Escala modular** de tamaños; **ritmo vertical** (leading consistente); jerarquía por tamaño/peso/espacio.
- "La tipografía existe para **honrar el contenido**."
- → *Aplicar*: ancho de texto ~65ch; escala de tipo fija; jerarquía tipográfica real, no todo del mismo peso.

### Designing Interfaces — Jenifer Tidwell
- Catálogo de **patrones de interacción** probados (navegación, listas, formularios, feedback).
- → *Aplicar*: reusar el patrón conocido para el problema (no inventar); alimenta la librería de componentes.

### A Pattern Language — Christopher Alexander
- Origen de los **patrones de diseño**: soluciones reutilizables a problemas recurrentes **en su contexto**.
- → *Aplicar*: justifica el método de esta skill — **arreglar el patrón, no la pantalla**; primitivas compartidas.

---

## E. Playbook de claridad (síntesis accionable de todo lo anterior)
Los 10 movimientos de mayor palanca para que el usuario **procese información limpia y clara**:
1. **Subir la relación señal/ruido**: borrar tinta/píxeles que no son datos (Tufte, Few, Lidwell).
2. **Organizar por LATCH** el contenido según cómo lo busca el usuario (Wurman) — temporal→calendario, etc.
3. **Chunking**: agrupar en ≤~4–7 bloques con jerarquía visual (Miller, Weinschenk, Gestalt).
4. **Progressive disclosure**: mostrar lo esencial; el detalle bajo demanda (Krug, Lidwell, Weinschenk).
5. **Codificar lo importante con atributos preatentivos** (color/posición/tamaño), no solo texto (Ware).
6. **Un dato = su contexto** (vs meta/periodo); el gráfico correcto para la relación (Few, Tufte).
7. **No me hagas pensar**: pantalla auto-evidente, CTA obvio, convenciones (Krug, Jakob).
8. **Cerrar los golfos**: signifiers para saber qué hacer + feedback para saber qué pasó (Norman).
9. **Diseñar para el Sistema 1**: defaults, reconocimiento, baja fricción, respuesta <400 ms (Kahneman, Doherty).
10. **Tipografía que honra el contenido**: ~65ch de medida, escala y ritmo, jerarquía real (Bringhurst).
