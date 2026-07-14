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

### Don't Make Me Think — Steve Krug
- **1ª ley: "No me hagas pensar"** — cada pantalla auto-evidente/auto-explicativa.
- La gente **escanea, no lee**; **satisface** (elige la 1ª opción razonable), **se las arregla** sin entender todo.
- **Las convenciones son tus amigas**; jerarquía visual clara; **"omití palabras innecesarias"**.
- Testeo de usabilidad barato ("una mañana al mes") vale más que debates.
- → *Aplicar*: CTAs obvios, jerarquía visual fuerte, copy mínimo, seguir convenciones (Jakob).

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
