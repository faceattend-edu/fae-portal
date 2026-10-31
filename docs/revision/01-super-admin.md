# Lista de verificación 1/4 — Super Administrador

Cuenta: `admin.faceattend` (ver `00-PROTOCOLO.md` §3). Menú esperado: **Inicio, Sedes, Reportes, Usuarios, Roles, Historial, Alertas, Configuración**.
Leyenda: `[API]` HTTP · `[AUTO]` navegador · `[MANUAL]` persona. Veredictos y reglas: `00-PROTOCOLO.md` §2 y §6.
Los registros que se creen se llaman `QA-<ID>` y se desactivan al final.

Unidades (una ejecución por unidad):
`SA-PUB` · `SA-AUT` · `SA-DSH` · `SA-SED` · `SA-USR` · `SA-ROL` · `SA-REP` · `SA-HIS` · `SA-ALE` · `SA-CFG` · `SA-RUT`

---

## SA-PUB · Pantallas públicas (se revisan solo aquí, valen para todos los roles)

- [ ] **SA-PUB-01** `[AUTO]` Abrir `/` sin sesión.
  - Esperado: se ve la landing con los botones **Registrarse**, **Iniciar sesión**, **Comenzar gratis**, **Ver demo**.
  - Falla si: pantalla en blanco o error de consola.
- [ ] **SA-PUB-02** `[AUTO]` Pulsar **Iniciar sesión**.
  - Esperado: URL `/login` con campos *correo* y *contraseña* y botón **Ingresar**.
- [ ] **SA-PUB-03** `[AUTO]` Pulsar **Registrarse** (y luego **Comenzar gratis**).
  - Esperado: ambos llevan a `/register`. **Ver demo** no rompe la página (anotar a dónde lleva).
- [ ] **SA-PUB-04** `[AUTO]` En `/register`, pulsar **Registrarse** con todo vacío.
  - Esperado: mensajes de campo obligatorio; **no** hay petición `POST`.
  - Falla si: se envía la petición vacía o aparece un error crudo.
- [ ] **SA-PUB-05** `[AUTO]` En `/register`, registrar `QA-PUB-05` con datos válidos y un documento que no exista.
  - Esperado: `POST /persons` y `POST /users` con 201; mensaje de éxito; no queda sesión iniciada sin querer.
  - Falla si: 5xx, o crea la persona pero no el usuario sin avisar.
- [ ] **SA-PUB-06** `[AUTO]` Repetir SA-PUB-05 con el **mismo documento**.
  - Esperado: mensaje claro de duplicado (no "uniqueness constraint" ni JSON crudo); no se crea otra fila.
- [ ] **SA-PUB-07** `[AUTO]` En `/recovery`, escribir un correo **inexistente** y enviar.
  - Esperado: `POST /auth/forgot-password` → **202** y mensaje neutro (no revela si el correo existe).
  - Falla si: el mensaje dice "no existe" o devuelve 404/500.
- [ ] **SA-PUB-08** `[AUTO]` En `/recovery`, escribir un código **incorrecto**.
  - Esperado: `POST /auth/verify-code` rechaza; mensaje legible; se puede reintentar.
- [ ] **SA-PUB-09** `[AUTO]` En `/recovery`, probar **Reenviar código** dos veces seguidas.
  - Esperado: el botón se bloquea mientras envía; no se duplican peticiones.
- [ ] **SA-PUB-10** `[AUTO]` Abrir `/app` **sin sesión**.
  - Esperado: redirige a `/login`. Falla si muestra datos o pantalla en blanco.

## SA-AUT · Sesión

- [ ] **SA-AUT-01** `[AUTO]` Iniciar sesión con credenciales válidas.
  - Esperado: `POST /api/v1/auth/login` → **200** con `sessionId`; llega a `/app` con título del panel y el nombre del usuario arriba a la izquierda.
- [ ] **SA-AUT-02** `[AUTO]` Iniciar con contraseña **errónea**.
  - Esperado: mensaje de error visible, se queda en `/login`, login devuelve **401**; el campo contraseña se conserva vacío o enmascarado.
  - Falla si: entra, o muestra el error técnico del servidor.
- [ ] **SA-AUT-03** `[AUTO]` Pulsar **Ingresar** con ambos campos vacíos.
  - Esperado: validación local, **sin** petición de login.
- [ ] **SA-AUT-04** `[AUTO]` Pulsar **Ingresar** dos veces rápido con credenciales válidas.
  - Esperado: el botón muestra "Ingresando…" y se envía **una** sola petición de login.
- [ ] **SA-AUT-05** `[AUTO]` Recargar (F5) estando en `/app`.
  - Esperado: sigue dentro de la sesión, mismo usuario, sin volver a `/login`.
- [ ] **SA-AUT-06** `[AUTO]` Pulsar **Cerrar sesión**.
  - Esperado: vuelve a `/login`; **no** aparece banner de error de datos en la pantalla de login.
- [ ] **SA-AUT-07** `[API]` Con el `sessionId` anterior al cierre: `GET /api/v1/auth/me`.
  - Esperado: **401** (la caché de Kong tolera hasta ~10 s: reintentar tras 11 s antes de marcar FAIL).
- [ ] **SA-AUT-08** `[AUTO]` Tras cerrar sesión pulsar **Atrás** del navegador.
  - Esperado: no muestra pantallas autenticadas con datos.

## SA-DSH · Dashboard global (`/app`)

- [ ] **SA-DSH-01** `[AUTO]` Abrir `/app` y esperar a que desaparezca el spinner.
  - Esperado: título del panel y tarjetas con números; **ninguna** tarjeta con `NaN`, `undefined` o vacío.
- [ ] **SA-DSH-02** `[API]` Comparar **total de estudiantes** de la pantalla con la base:
  `SELECT count(*) FROM academic.academic_actor a JOIN academic.academic_actor_type t ON t.actor_type_id=a.actor_type_id WHERE t.code='STUDENT' AND a.deleted_at IS NULL;`
  - Esperado: coincide. Si difiere por filtro de estado, anotar ambas cifras como `DUDA`.
- [ ] **SA-DSH-03** `[AUTO]` Recorrer cada tarjeta, gráfica y lista del panel.
  - Esperado: cada una muestra datos o su estado vacío explícito ("Sin datos"); ninguna queda cargando para siempre (> 30 s).
- [ ] **SA-DSH-04** `[AUTO]` Pulsar cada enlace o botón del panel (anotar sus nombres).
  - Esperado: cada uno lleva a una ruta existente, sin pantalla en blanco.
- [ ] **SA-DSH-05** `[AUTO]` Revisar la consola tras la carga completa.
  - Esperado: sin `Uncaught`. Falla si hay uno.
- [ ] **SA-DSH-06** `[AUTO]` Cambiar el ancho de ventana a 768 px y a 375 px.
  - Esperado: sin scroll horizontal de página; las tarjetas se reacomodan; el menú sigue accesible.

## SA-SED · Sedes (`/app/schools`)

- [ ] **SA-SED-01** `[AUTO]` Abrir **Sedes**.
  - Esperado: lista con cada sede (SENA, ICT, SMP…) con nombre y estado; coincide con `SELECT count(*) FROM academic.school WHERE deleted_at IS NULL;`.
- [ ] **SA-SED-02** `[AUTO]` **Nueva sede** → **Crear sede** con todo vacío.
  - Esperado: validación de campos obligatorios; sin `POST`.
- [ ] **SA-SED-03** `[AUTO]` Crear sede `QA-SED-03` con código `QA03`.
  - Esperado: `POST /schools` → **201**; aparece en la lista sin recargar; tras F5 sigue ahí.
- [ ] **SA-SED-04** `[AUTO]` Crear otra sede con el **mismo código** `QA03`.
  - Esperado: mensaje claro de duplicado; no se crea.
  - Falla si: crea la segunda o muestra el error crudo del servidor.
- [ ] **SA-SED-05** `[AUTO]` **Editar** `QA-SED-03`, cambiar el nombre a `QA-SED-03b`, **Guardar**.
  - Esperado: `PUT /schools/{id}` 200/204; nombre nuevo visible; persiste tras F5.
- [ ] **SA-SED-06** `[AUTO]` **Cancelar** una edición con cambios sin guardar.
  - Esperado: el nombre original se conserva; no hay `PUT`.
- [ ] **SA-SED-07** `[AUTO]` **Desactivar** `QA-SED-03b`.
  - Esperado: `PATCH /schools/{id}/status` 200/204; badge "Inactiva"; **Activar** la restaura.
- [ ] **SA-SED-08** `[AUTO]` Con la sede QA inactiva, abrir **Usuarios**.
  - Esperado: no rompe la pantalla.
- [ ] **SA-SED-09** `[AUTO]` Dejar `QA-SED-03b` **inactiva** al terminar. (Cierre de la unidad.)

## SA-USR · Usuarios (todas las sedes)

- [ ] **SA-USR-01** `[AUTO]` Abrir `/app/users`.
  - Esperado: encabezado "N usuarios encontrados"; N = `SELECT count(*) FROM identity.app_user WHERE deleted_at IS NULL;` (si difiere, `DUDA` con ambas cifras).
- [ ] **SA-USR-02** `[AUTO]` Abrir `/app/users/students`, `/app/users/teachers`, `/app/users/admins`.
  - Esperado: cada una lista solo ese tipo; la columna *Programa* solo en estudiantes/docentes; sin pantalla en blanco.
- [ ] **SA-USR-03** `[AUTO]` Buscar un nombre que exista y luego uno inexistente.
  - Esperado: el primero filtra; el segundo muestra estado vacío "Sin resultados"; al borrar la búsqueda vuelve la lista.
- [ ] **SA-USR-04** `[AUTO]` Abrir el **detalle de un estudiante** (clic en la fila).
  - Esperado: modal con nombre, correo, documento, usuario, ficha y asistencia; **sin** `Uncaught`. Falla si hay pantalla en blanco.
- [ ] **SA-USR-05** `[AUTO]` Abrir el detalle de un **docente** y de un **administrador**.
  - Esperado: el administrador **no** muestra bloque de asistencia ni de biometría.
- [ ] **SA-USR-06** `[AUTO]` Crear un usuario `QA-SA-USR-06` (docente) con **Nuevo**.
  - Esperado: `POST /persons`, `POST /users`, `POST /users/{id}/roles` → 201; aparece en la lista.
- [ ] **SA-USR-07** `[AUTO]` **Editar** `QA-SA-USR-06`: cambiar teléfono a `3001234567` y **Guardar cambios**.
  - Esperado: `PUT /persons/{id}` 200 y `PUT /users/{id}` 204; al reabrir el detalle se ve el teléfono; persiste tras F5.
  - Falla si: pide "El usuario es requerido" aunque el usuario existe.
- [ ] **SA-USR-08** `[AUTO]` **Desactivar** `QA-SA-USR-06` (confirmar) y luego **Activar**.
  - Esperado: dos `PATCH …/status` 204 por cambio; badge cambia a *Inactivo* y vuelve a *Activo*.
- [ ] **SA-USR-09** `[AUTO]` **Cambiar el rol** de `QA-SA-USR-06` a *Estudiante* desde Editar (solo el Super Admin ve este selector).
  - Esperado: se asigna el rol nuevo y se retira el anterior; el badge de rol cambia; se puede volver a revertir.
- [ ] **SA-USR-10** `[AUTO]` Marcar 2 usuarios `QA-` con las casillas.
  - Esperado: aparece la barra "2 seleccionados" con **Activar**, **Desactivar**, **Eliminar**, **Limpiar selección**.
- [ ] **SA-USR-11** `[AUTO]` Con 2 seleccionados pulsar **Desactivar** → Confirmar.
  - Esperado: mensaje "2 usuarios actualizados"; ambos *Inactivo*; la selección se limpia. Luego **Activar** los restaura.
- [ ] **SA-USR-12** `[AUTO]` Marcar **a sí mismo** y a un `QA-` y pulsar **Desactivar**.
  - Esperado: "1 usuarios actualizados" y un aviso con *No puedes aplicar esta acción a tu propia cuenta*; la cuenta propia **sigue activa**.
  - Falla si: se desactiva la cuenta propia.
- [ ] **SA-USR-13** `[AUTO]` **Eliminar** (lógico) `QA-SA-USR-06` desde el detalle.
  - Esperado: pide confirmación; queda inactivo (fila **no** borrada: `SELECT deleted_at, status FROM identity.app_user WHERE username LIKE 'qa-%';`).
- [ ] **SA-USR-14** `[AUTO]` Con una lista > 1 página, usar **Siguiente** y **Anterior**.
  - Esperado: cambian las filas; texto "Página X de Y"; "Mostrando N de M" coherente; en la primera página **Anterior** está inactivo.
- [ ] **SA-USR-15** `[MANUAL]` **Importar**: abrir el modal, descargar **Plantilla CSV**, subir un CSV de 2 filas `QA-` y confirmar.
  - Esperado: resumen "2 agregados"; las filas con error muestran su motivo; no quedan huérfanos.

## SA-ROL · Roles y permisos (`/app/roles`)

- [ ] **SA-ROL-01** `[AUTO]` Abrir **Roles**.
  - Esperado: se ven los 4 roles base (*Super Administrador, Administrador, Instructor, Aprendiz*) y la matriz de permisos con nombres legibles.
- [ ] **SA-ROL-02** `[AUTO]` **Nuevo rol** → **Crear rol** con nombre vacío.
  - Esperado: validación; sin `POST`.
- [ ] **SA-ROL-03** `[AUTO]` Crear el rol `QA-ROL-03`.
  - Esperado: `POST /roles` 201; aparece en la lista; persiste tras F5.
- [ ] **SA-ROL-04** `[AUTO]` Crear otro rol con el **mismo nombre**.
  - Esperado: mensaje de duplicado legible; no se crea.
- [ ] **SA-ROL-05** `[AUTO]` Asignar un permiso a `QA-ROL-03`.
  - Esperado: `POST /roles/{id}/permissions` 201/204; la casilla queda marcada; persiste tras F5.
- [ ] **SA-ROL-06** `[AUTO]` Quitar ese permiso.
  - Esperado: `DELETE /roles/{id}/permissions/{permId}` 204; casilla desmarcada.
- [ ] **SA-ROL-07** `[AUTO]` **Eliminar** `QA-ROL-03` y confirmar.
  - Esperado: `DELETE /roles/{id}` 204; desaparece de la lista.
- [ ] **SA-ROL-08** `[AUTO]` Mirar (**sin confirmar**) qué ofrece la pantalla para un rol base.
  - Esperado: **Eliminar** no está disponible o pide confirmación fuerte. **No pulsar confirmar.** Si permite borrar un rol con usuarios sin aviso: `FAIL` Alta.

## SA-REP · Reportes (`/app/reports`)

- [ ] **SA-REP-01** `[AUTO]` Abrir **Reportes**.
  - Esperado: carga con cifras globales; sin `NaN/undefined`.
- [ ] **SA-REP-02** `[AUTO]` Recorrer las pestañas/secciones (anotar sus nombres, p. ej. *Históricos*, *Sanciones*).
  - Esperado: cada una carga sin errores; los umbrales usan el mínimo de asistencia configurado.
- [ ] **SA-REP-03** `[AUTO]` Aplicar un filtro (ficha o fechas) con **Aplicar**, luego **Limpiar**.
  - Esperado: la lista cambia y vuelve; el contador coincide con las filas.
- [ ] **SA-REP-04** `[AUTO]` Aplicar un filtro con un rango de fechas **invertido** (fin antes de inicio).
  - Esperado: mensaje de validación o resultado vacío explícito; **no** error 5xx.
- [ ] **SA-REP-05** `[MANUAL]` **Exportar**: elegir formato y rango, abrir el archivo.
  - Esperado: el archivo abre y sus totales coinciden con la pantalla.
- [ ] **SA-REP-06** `[MANUAL]` **Notificar todos**: **NO pulsar** (envía mensajes a personas reales). Solo comprobar que el botón existe.
  - Esperado: botón visible; al pulsarlo (lo hace una persona en entorno de prueba) pide confirmación.

## SA-HIS · Historial (`/app/history`)

- [ ] **SA-HIS-01** `[AUTO]` Abrir **Historial**.
  - Esperado: filas con fecha, estudiante, estado y método; sin `undefined`.
- [ ] **SA-HIS-02** `[AUTO]` Filtrar por una ficha y pulsar **Consultar**; luego **Limpiar**.
  - Esperado: solo registros de esa ficha; **Limpiar** restaura.
- [ ] **SA-HIS-03** `[AUTO]` Con varias páginas, **Siguiente** y **Anterior**.
  - Esperado: cambian las filas; sin duplicados entre páginas.
- [ ] **SA-HIS-04** `[AUTO]` Consultar una ficha **sin registros**.
  - Esperado: estado vacío claro, no spinner infinito.

## SA-ALE · Alertas (`/app/alerts`)

- [ ] **SA-ALE-01** `[AUTO]` Abrir **Alertas**.
  - Esperado: encabezado "N sin resolver" coherente con las filas pendientes; o estado "Sin alertas".
- [ ] **SA-ALE-02** `[AUTO]` **Ver resueltas** y **Ocultar resueltas**.
  - Esperado: la lista alterna; el botón cambia de nombre.
- [ ] **SA-ALE-03** `[AUTO]` **Actualizar**.
  - Esperado: nueva petición `GET …/alerts`; sin parpadeo roto.
- [ ] **SA-ALE-04** `[AUTO]` **Ver** una alerta.
  - Esperado: muestra su detalle con tipo, severidad y fecha legibles.
- [ ] **SA-ALE-05** `[AUTO]` **Marcar resuelta** una alerta `QA-`.
  - Esperado: petición `PATCH/PUT` 200/204; sale de pendientes y el contador baja en 1.
- [ ] **SA-ALE-06** `[AUTO]` **Eliminar** una alerta `QA-` resuelta (confirmar).
  - Esperado: desaparece; en la base queda con `deleted_at` (no se borra la fila).

## SA-CFG · Configuración (`/app/settings`)

Secciones en el menú lateral: **General, Reconocimiento, Notificaciones, Seguridad, Apariencia** (y *Sede* si aparece).

- [ ] **SA-CFG-01** `[AUTO]` Abrir cada sección una por una.
  - Esperado: todas cargan con sus controles; sin errores de consola.
- [ ] **SA-CFG-02** `[AUTO]` **General**: cambiar el formato de fecha a otro, **Guardar cambios**.
  - Esperado: aparece "¡Guardado!"; la petición de guardado devuelve 200/204; las fechas de otras pantallas usan el formato nuevo; persiste tras F5. **Revertir.**
- [ ] **SA-CFG-03** `[AUTO]` **General**: cambiar el idioma y volver.
  - Esperado: los textos cambian sin claves crudas (`some.key`).
- [ ] **SA-CFG-04** `[AUTO]` **General · Período**: probar **Automático**, **Manual** y **Sugerir fechas**.
  - Esperado: *Sugerir fechas* rellena inicio < fin; con fechas invertidas no deja guardar.
- [ ] **SA-CFG-05** `[AUTO]` **Reconocimiento**: mover el umbral de asistencia mínimo y **Guardar**.
  - Esperado: se guarda; en el Dashboard/Usuarios cambian los colores/estados según el nuevo umbral. **Revertir.**
- [ ] **SA-CFG-06** `[AUTO]` Cambiar un valor y pulsar **Descartar**.
  - Esperado: el valor vuelve al guardado; no hay petición de guardado.
- [ ] **SA-CFG-07** `[AUTO]` **Notificaciones**: activar/desactivar un interruptor y **Guardar**.
  - Esperado: persiste tras F5.
- [ ] **SA-CFG-08** `[AUTO]` **Seguridad**: poner un tiempo de sesión **inválido** (0, negativo, texto).
  - Esperado: validación; no se guarda. Con valor válido sí.
- [ ] **SA-CFG-09** `[AUTO]` **Apariencia**: alternar modo claro/oscuro y cambiar el color de acento.
  - Esperado: toda la pantalla cambia; ningún texto queda invisible (mismo color que el fondo).
- [ ] **SA-CFG-10** `[AUTO]` Guardar con el **backend caído** (apagar `fae-ms-configuration` un momento — **lo hace el supervisor**).
  - Esperado: mensaje de error visible; el formulario conserva los valores; no dice "¡Guardado!".

## SA-RUT · Rutas fuera de su menú (no hay expectativa definida: solo registrar)

Para cada ruta, abrir por URL y anotar el resultado literal. `FAIL` solo si queda **pantalla en blanco** o hay `Uncaught`.
Si carga con datos o muestra "Acceso denegado", **no es FAIL**: registrar como `DUDA` con la captura para que el supervisor decida si es correcto.

- [ ] **SA-RUT-01** `[AUTO]` `/app/courses`
- [ ] **SA-RUT-02** `[AUTO]` `/app/environments`
- [ ] **SA-RUT-03** `[AUTO]` `/app/academic`
- [ ] **SA-RUT-04** `[AUTO]` `/app/justifications`
- [ ] **SA-RUT-05** `[AUTO]` `/app/kiosk`
- [ ] **SA-RUT-06** `[AUTO]` `/app/biometrics` (no tiene entrada en ningún menú)
- [ ] **SA-RUT-07** `[AUTO]` `/app/contacts`
- [ ] **SA-RUT-08** `[AUTO]` `/app/school-info`
- [ ] **SA-RUT-09** `[AUTO]` `/app/ruta-que-no-existe` — Esperado: página de "no encontrada" o redirección, no pantalla en blanco.
