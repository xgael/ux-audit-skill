# AI/LLM UX — patrones para productos con IA

UX de features con LLMs/agentes: chat, copilots, generación, análisis automático. La regla
madre: **la IA es asistente, no oráculo** — el usuario siempre puede ver, editar, rechazar y
deshacer lo que la IA hizo. NN/g: **explicación + recuperabilidad** son los 2 predictores más
fuertes de uso sostenido.

---

## A. Esperas y streaming (la performance percibida de la IA)
Extiende `patterns.md` §B para latencias de LLM (0.5-2 s hasta el primer token, 5-60 s total):
- **Streaming = baseline**: mostrar tokens en vivo; respuesta que espera al final completo se
  siente rota. Pre-primer-token: skeleton de 3-5 líneas de ancho variable o indicador "pensando"
  — NUNCA pantalla congelada.
- **Estados del proceso con verbos reales** ("Leyendo el PDF… Extrayendo partidas…"), no spinner
  mudo de 30 s — la espera con narrativa se tolera; la incierta no (patterns §B).
- **Cancelar/Detener SIEMPRE visible** durante la generación (user control, NN#3). Regenerar
  disponible después.
- Operación > ~30 s → **background + notificación** al terminar; no rehén de la pestaña.
- Streaming que no rompe: el texto en vivo no debe empujar el viewport (auto-scroll con opt-out
  si el usuario scrolleó), ni re-layout brusco al renderizar markdown/tablas.

## B. Incertidumbre y confianza (probabilistic UX)
- **La IA se equivoca → el diseño lo debe decir**: disclaimer contextual corto donde el error
  cuesta (montos, legal, salud), no un banner genérico permanente que se vuelve invisible.
- **Confidence visible cuando es accionable**: separá lo auto-aprobable (alta confianza) de lo
  que requiere juicio humano (baja) — cola de revisión, no un % decorativo. Un "87%" pelado no
  informa; "revisá estos 3 campos dudosos" sí.
- **Citas/fuentes navegables** cuando la IA afirma sobre datos del usuario (RAG): click → el
  fragmento del documento original. Sin fuente, el usuario no puede verificar = no puede confiar.
- **Mostrar el input entendido**: qué entendió/extrajo la IA (campos parseados, filtros
  aplicados) ANTES de actuar sobre ello — el usuario corrige la comprensión, no el desastre.

## C. Control humano (human-in-the-loop)
- **Toda salida de IA es editable** — el output aterriza en un editor/campos, no en piedra.
  Editar > regenerar ruleta.
- **Acciones de agente reversibles**: log cronológico de qué hizo (qué archivos/registros tocó)
  + **Undo por acción** (misma primitiva soft-delete/restaurar de SKILL.md) o snapshot/rollback.
  Irreversible → confirm con alcance explícito ("El agente enviará 12 emails").
- **Autonomía gradual**: primero sugerir (humano aprueba) → luego auto con revisión → auto
  silencioso solo lo trivial y de alta confianza. El usuario configura el nivel, y el sistema
  muestra SUS límites ("puedo leer, no puedo borrar").
- **Feedback loop**: pulgar/corregir en outputs — señal de mejora Y sensación de control (los
  usuarios que saben que pueden corregir usan más el sistema).

## D. Affordances de prompt (el input)
- **Empty state del chat = onboarding** (flows §A): 3-4 ejemplos clickeables de prompts REALES
  del dominio ("Analiza esta licitación…"), no "Pregúntame lo que sea" (parálisis de página en
  blanco, Hick).
- **Acciones estructuradas > prompt libre** para tareas conocidas: botones/comandos ("Resumir",
  "Extraer partidas", `/comando`) — el prompt libre es el fallback, no la única puerta.
  Sugerencias de follow-up tras cada respuesta.
- Indicar capacidades y límites honestos en el punto de uso ("Analizo PDFs de hasta 100
  págs") — expectativas correctas = menos goodwill drenado. Nada de vender magia (heurística
  #20: sin claims falsos).
- Historial/contexto visible: qué documentos/datos está usando la IA ahora (chips removibles) —
  el usuario controla el contexto, no lo adivina.

## E. Checklist AI-UX (auditable)
1. ¿Streaming o narrativa de progreso? (nada congelado > 2 s)
2. ¿Cancelar visible durante generación? ¿Regenerar después?
3. ¿Output editable en el lugar? ¿Feedback (👍/corregir)?
4. ¿Acciones del agente logueadas y deshacibles? ¿Confirm con alcance para lo irreversible?
5. ¿Fuentes/citas navegables en afirmaciones sobre datos? ¿Se muestra qué entendió antes de actuar?
6. ¿Disclaimer contextual (no banner muerto) donde el error cuesta?
7. ¿Empty state con prompts de ejemplo del dominio? ¿Acciones estructuradas para lo repetitivo?
8. ¿Límites/capacidades honestos en el punto de uso?

## Fuentes
- NN/g — AI & trust / generative UI — https://www.nngroup.com/articles/generative-ui/
- Agentic Design — UI/UX patterns for agents — https://agentic-design.ai/patterns/ui-ux-patterns
- Probabilistic UX (uncertainty/confidence) — https://cocreate.consulting/field-notes/probabilistic-ux-design-patterns
- Streaming UI patterns — https://thepromptbench.com/ai-product-ux/streaming-ui-patterns-that-dont-break/
- People + AI Guidebook (Google PAIR) — https://pair.withgoogle.com/guidebook/
