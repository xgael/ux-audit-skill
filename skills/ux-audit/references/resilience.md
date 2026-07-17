# Resiliencia — red, sesión, seguridad, permisos

UX cuando el mundo falla: red intermitente, sesión vencida, doble click, permisos. Lo que
separa un producto de demo de uno de producción. La regla madre: **el usuario nunca pierde
trabajo ni paga dos veces por un fallo que no causó**.

---

## A. Red intermitente y offline
- **Detectar y decir**: banner persistente no-modal al perder conexión ("Sin conexión — los
  cambios se guardarán al volver"), que desaparece SOLO al reconectar y confirma ("Conectado ✓",
  autodismiss). Nada de toasts que se pierden.
- **Cola de acciones**: lo que el usuario hizo offline/con red mala se ENCOLA y sincroniza al
  volver — no se tira con un error. Mínimo viable: preservar el estado del form y reintentar el
  submit; ideal: outbox con indicador ("2 cambios pendientes de sincronizar").
- **Retry con backoff automático** en lecturas (el usuario no debería clickear "Reintentar" 5
  veces — la app reintenta sola 2-3 veces con backoff y RECIÉN entonces muestra el error con
  botón).
- **Optimistic UI + reconciliación**: aplicar local, marcar "pendiente" sutil, resolver al
  confirmar el server. Conflicto (otro usuario editó) → mostrar ambos y elegir, nunca pisar
  silencioso.
- **Timeouts con presupuesto**: request colgado ≠ spinner infinito — corta a los ~10-15 s y
  pasa al estado de error con Reintentar (patterns §B).

## B. Idempotencia percibida (el doble click)
- **Doble submit = un solo efecto**: deshabilitar el botón DURANTE el vuelo (spinner+label en
  el botón) + idempotency key en el backend para pagos/creaciones. El clásico: doble click en
  "Pagar" → dos cargos = [H] crítico.
- Navegación repetida segura: refrescar la página de confirmación no re-ejecuta (POST-redirect-
  GET); el Back tras un submit no re-envía.
- Acciones de fila repetibles (aprobar, enviar): la segunda invocación dice "ya estaba
  aprobado", no falla ni duplica.

## C. Sesión y re-autenticación
- **Expirar sesión NUNCA tira trabajo**: el [H] más odiado en ERPs — 40 minutos llenando un
  form, submit, "sesión expirada", form vacío. Fixes en orden de preferencia:
  1. Refresh token silencioso mientras hay actividad (el usuario activo NO expira).
  2. Aviso previo con cuenta regresiva y botón "Seguir conectado" (accesible, `role="alert"`).
  3. Si expiró igual: **preservar el draft** (storage local) y re-auth en modal/overlay SIN
     navegar — al volver, el form sigue ahí y el submit se repite.
- Re-auth en contexto para acciones sensibles (cambiar password, ver dato enmascarado): modal
  puntual, no logout global.
- Multi-tab coherente: logout en una pestaña → las demás lo detectan y avisan (no fallan
  silenciosas en el próximo click).

## D. Security UX (la seguridad que no castiga)
- **Password managers primero**: `autocomplete="current-password"/"new-password"`, un solo
  campo de password (no "repetir email"), sin bloquear paste (bloquear paste = anti-password-
  manager = MENOS seguridad), sin reglas absurdas visibles solo al fallar (mostrar requisitos
  ANTES, checklist en vivo).
- **2FA/OTP**: `autocomplete="one-time-code"` (iOS/Android autocompletan del SMS), aceptar el
  código con o sin espacios (forgiving), reenvío con cooldown visible, y alternativas (backup
  codes) alcanzables.
- **Passkeys** donde se pueda: es EL upgrade de UX+seguridad (sin password que olvidar); botón
  junto al login tradicional, no reemplazo forzado.
- Mensajes de auth que no filtran ("credenciales incorrectas", no "el email no existe") PERO
  con recuperación clara al lado (¿olvidaste tu contraseña?).
- Bloqueos y rate limits con información: "demasiados intentos, esperá 5 min" con countdown —
  no un fallo mudo que parece bug.

## E. Permisos del sistema (browser/OS)
- **Pedir EN contexto, nunca al abrir**: el permission prompt del browser/OS aparece cuando el
  usuario inicia la acción que lo necesita (click en "Activar notificaciones", no al cargar la
  home). Prompt al aterrizar = rechazo casi seguro, y el rechazo del browser es CARO de revertir.
- **Primer propio antes del prompt nativo** para permisos costosos (cámara, ubicación, notifs):
  pantalla/modal propia que explica el beneficio concreto → si acepta, RECIÉN el prompt nativo.
  Doble opt-in barato: el "no" propio no quema el permiso del browser.
- **Rechazo con plan B**: denegó ubicación → input manual de dirección; denegó cámara → subir
  archivo. La feature se degrada, no se bloquea.
- Estado visible y reversible: dónde ver/cambiar los permisos otorgados (settings de la app con
  deep link a los del browser/OS si aplica).

## F. Checklist de resiliencia (auditable)
1. Matar la red (DevTools offline) a mitad de un form → ¿se pierde algo? ¿banner claro?
2. Doble click rápido en todo submit de dinero/creación → ¿un solo efecto?
3. Dejar la sesión expirar con un form a medias → ¿el trabajo sobrevive?
4. Login con password manager y OTP por SMS → ¿autocompletan? ¿paste funciona?
5. Denegar un permiso del browser → ¿hay plan B o pantalla muerta?
6. Request lento (throttling) → ¿timeout con Reintentar o spinner eterno?
7. Editar el mismo registro en dos pestañas → ¿conflicto visible o pisada silenciosa?

## Fuentes
- Offline UX — https://web.dev/articles/offline-ux-design-guidelines
- Stripe (idempotency keys, payment UX) — https://stripe.com/docs/api/idempotent_requests
- NN/g — timeouts y sesiones — https://www.nngroup.com/articles/website-response-times/
- web.dev — sign-in form best practices — https://web.dev/articles/sign-in-form-best-practices
- Permission UX (Chrome team) — https://developer.chrome.com/docs/web-platform/permissions-best-practices
