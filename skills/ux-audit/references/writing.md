# UX Writing — microcopy, errores, empty states

El texto ES la interfaz. La mayoría de los fallos de UX "de diseño" son fallos de copy:
un botón que no dice qué hace, un error que no dice cómo salir, un vacío que no dice cómo
empezar. Consultá esto al auditar/arreglar cualquier texto visible.

## 1. Principios (NN/g + GOV.UK content + Polaris)
- **Claro > listo**: palabras del usuario, no jerga interna (information scent, Krug).
- **Conciso**: la 3ª ley de Krug aplica al microcopy — si el label funciona sin la palabra, sobra.
- **Frontload**: la palabra clave PRIMERO ("Facturas vencidas", no "Listado de todas las facturas
  que están vencidas"). La gente escanea las primeras 1-2 palabras.
- **Voz activa + verbo específico**: "Guardá los cambios" / "Guardar cambios", no "Los cambios
  pueden ser guardados" ni "Aceptar".
- **Escaneable**: NN/g midió +58% de usabilidad reescribiendo conciso y formateado para escaneo.
- **Consistencia terminológica**: una cosa = un nombre en TODA la app (no "cliente" acá y
  "cuenta" allá). Diccionario de términos del producto.

## 2. Botones y CTAs
- **Verbo + objeto**: "Eliminar proyecto", "Enviar factura" — nunca "OK", "Sí", "Continuar" solo,
  ni "Click aquí". El label debe sobrevivir fuera de contexto (lectores de pantalla saltan de
  botón en botón).
- **El botón dice exactamente qué pasa** (Polaris). En confirmaciones: título = pregunta con el
  verbo ("¿Eliminar el proyecto?"), botones = **verbos específicos** ("Eliminar" / "Cancelar"),
  jamás "Sí"/"No" (obliga a releer la pregunta = hace pensar).
- **Acción destructiva dice su alcance**: "Eliminar 3 archivos", no "Eliminar".
- Un solo **CTA primario** por vista (Von Restorff); el resto secundarios/terciarios.

## 3. Mensajes de error — el canon
Anatomía de un error bien escrito (NN/g guidelines):
1. **Qué pasó**, en lenguaje humano ("No pudimos guardar el borrador").
2. **Por qué**, si se sabe ("Se cortó la conexión").
3. **Cómo resolverlo** — acción concreta y/o botón ("Reintentar", "Verificá el número e intentá
   de nuevo").
4. **Tono sin culpa ni alarma**: nada de "¡ERROR FATAL!", "Entrada ilegal", "Usted no…". El
   sistema falló o el dato no matchea; el usuario nunca es el acusado. Ante la duda,
   **disculpate** (Krug, goodwill).
5. **Preservá el input**: un error NUNCA borra lo que el usuario tecleó (re-teclear = castigo).
6. Código técnico (si hace falta para soporte) al final, chico o colapsado — no como titular.

Anti-patrones: "Something went wrong" sin más (no dice qué ni cómo seguir); "Error 500" pelado;
error genérico para causas distintas; validación que grita en rojo mientras el usuario todavía
está tecleando (ver `references/patterns.md` §C: reward early, punish late).

## 4. Empty states — cada vacío es distinto (y es una oportunidad)
| Estado | Qué decir | CTA |
|---|---|---|
| **First-use** (nunca hubo datos) | Qué es esta sección + valor ("Acá van a vivir tus facturas") | Crear el primero / importar / ejemplo |
| **Vaciado por el usuario** (todo procesado/archivado) | Reconocer el logro ("Bandeja limpia 🎉") | Siguiente acción útil |
| **Sin resultados** (búsqueda/filtros) | Qué se buscó + por qué puede no haber match | Limpiar filtros / corregir / crear "X" |
| **Error de carga** | Canon de §3 — **JAMÁS se ve igual que un vacío** | Reintentar |
| **Parcial** (algunos items fallaron) | Qué cargó y qué no | Reintentar lo fallido |
- Empty state muerto ("No hay datos.") = hallazgo [M]. Sin CTA = texto muerto.

## 5. Formularios (el copy; la mecánica en `patterns.md` §C)
- **Placeholder NO es label** — desaparece al teclear y la memoria de trabajo lo pierde. Label
  visible siempre; hint persistente debajo si hace falta formato.
- **Hint ANTES del error**: si el campo tiene formato esperado, decilo de entrada ("8 dígitos,
  sin espacios"), no lo reveles recién en el mensaje de error.
- Preguntá como persona, no como base de datos ("¿A dónde enviamos la factura?" > "Email address
  field required").
- Marcá lo **opcional**, no lo requerido (si la mayoría es requerida).

## 6. Feedback de éxito y estados vivos
- Éxito concreto: "Factura #1043 enviada a Berel" > "Operación exitosa".
- Toast con **Undo** cuando aplica (ver SKILL.md primitivas).
- Estados de proceso con progreso real ("Procesando 3 de 12…"), no "Espere…".

## 7. Checklist rápido de writing (auditable por pantalla)
1. Todo botón dice **verbo + objeto**; ningún "OK/Sí/No/Click aquí".
2. Todo error dice **qué + por qué + cómo seguir**, sin culpa, sin borrar input.
3. Todo empty state tiene **explicación + CTA** (y error ≠ vacío).
4. Ningún placeholder actúa de label; hints visibles antes del error.
5. Un término = un concepto en toda la app.
6. Keyword primero en labels/títulos (frontload).
7. Cero happy talk; cero instrucciones que reemplazan a un diseño auto-evidente (Krug).

Marco de fondo (Redish, books §B): **cada página es una conversación que el usuario inició** —
títulos/errores/empty states son RESPUESTAS a su pregunta, no anuncios. Y **bite-snack-meal**:
la misma info en 3 profundidades (titular → resumen → detalle), el usuario elige cuánto comer.

## Fuentes
- NN/g Error-Message Guidelines — https://www.nngroup.com/articles/error-message-guidelines/
- NN/g UX Writing study guide — https://www.nngroup.com/articles/writing-study-guide/
- GOV.UK content design — https://www.gov.uk/guidance/content-design/writing-for-gov-uk
- Shopify Polaris content — https://polaris.shopify.com/content
- Microcopy: The Complete Guide — Kinneret Yifrah
- Ginny Redish — *Letting Go of the Words* (books §B)
