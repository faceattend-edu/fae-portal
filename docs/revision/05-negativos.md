# Casos negativos

Dos partes:
- **A. Matriz de API** — 100 % automatizable (capa L1, `00-PROTOCOLO.md` §9). Un script (o Haiku con `curl`) la recorre fila por fila.
- **B. Casos de interfaz** — los hace Haiku con el navegador (`[AUTO]`) o una persona (`[MANUAL]`).

## Regla de veredicto para los negativos (la misma para todo el archivo)

| Resultado real | Veredicto |
|---|---|
| Una acción **prohibida** devuelve **2xx** (se ejecutó) | `FAIL` **Bloqueante** |
| Cualquier respuesta **5xx**, stack trace o mensaje con datos internos (SQL, rutas de archivo) | `FAIL` **Alta** |
| Devuelve **4xx distinto** del esperado pero dentro de *Aceptable* | `PASS` |
| Devuelve **4xx fuera** de lo esperado y lo aceptable | `DUDA` (anotar el código real) |
| Cumple *Esperado* | `PASS` |

> Los códigos "Esperado" son la **hipótesis de diseño**; el supervisor ajusta la tabla con lo que revele la primera pasada.
> Cuando una fila diga **"sin cambio"**, comprobar con `SELECT` que el dato **no** se modificó.

## Cuentas y datos de contraste

Sesión por rol: `POST /api/v1/auth/login` con `{"username","password"}` → usar `sessionId` como `Authorization: Bearer`.
Roles de la matriz: `ANON` (sin token) · `EST` (`est.ict`) · `DOC` (`maria.sanchez`) · `ADM` (`fermin`, SENA) · `ADM2` (`admin.ict`, ICT) · `SUP` (`admin.faceattend`).

Variables que el supervisor resuelve con SQL antes de correr:

| Variable | Qué es |
|---|---|
| `{U_ICT}`, `{P_ICT}` | `user_id` y `person_id` de un usuario de **ICT** |
| `{U_SENA}`, `{P_SENA}` | idem de **SENA** |
| `{U_SMP}`, `{P_SMP}` | idem de **SMP** |
| `{PROPIO_EST}` | `user_id`, `person_id`, `academic_actor_id` de `est.ict` |
| `{COMPA}` | `academic_actor_id` de un compañero de `est.ict` |
| `{C_ICT}`, `{COH_ICT}`, `{ENV_ICT}`, `{SES_ICT}` | curso, ficha, ambiente y sesión de ICT |
| `{ROL_ADMIN}`, `{ROL_SUPER}` | `role_id` de *Administrador* y *Super Administrador* |
| `{JUS_ICT}` | justificación de ICT |

---

## A. Matriz de API

### A1 · Autenticación

| ID | Rol | Método y ruta | Cuerpo / nota | Esperado | Aceptable | Falla si |
|---|---|---|---|---|---|---|
| NEG-A01 | ANON | GET `/persons` | — | 401 | — | 2xx |
| NEG-A02 | ANON | GET `/users` | — | 401 | — | 2xx |
| NEG-A03 | ANON | GET `/academic-actors` | — | 401 | — | 2xx |
| NEG-A04 | ANON | GET `/attendance-records` | — | 401 | — | 2xx |
| NEG-A05 | cualquiera | GET `/persons` | `Authorization: Bearer abc` (no UUID) | 401 | — | 2xx |
| NEG-A06 | cualquiera | GET `/persons` | `Bearer 00000000-0000-0000-0000-000000000000` (UUID inexistente) | 401 | — | 2xx |
| NEG-A07 | cualquiera | GET `/persons` | cabecera `Authorization: Basic xxx` | 401 | — | 2xx |
| NEG-A08 | EST | GET `/persons?token={sessionId}` | token **solo en la URL**, sin cabecera | 401 | — | 2xx |
| NEG-A09 | EST | tras `POST /auth/logout`, GET `/auth/me` con el token viejo | esperar 11 s antes (caché de Kong) | 401 | — | 2xx tras 11 s |
| NEG-A10 | ANON | POST `/auth/login` | `{}` | 400 | 401, 422 | 5xx |
| NEG-A11 | ANON | POST `/auth/login` | usuario existente + contraseña errónea | 401 | — | 200 / 5xx |
| NEG-A12 | ANON | POST `/auth/login` | `{"username":"' OR 1=1 --","password":"x"}` | 401 | 400 | 200 / 5xx |
| NEG-A13 | ANON | POST `/auth/login` | usuario **inexistente** | 401 (mismo mensaje que A11) | — | mensaje distinto que revele que no existe |
| NEG-A14 | ANON | POST `/auth/forgot-password` | correo inexistente | 202 | — | 404 / 5xx |
| NEG-A15 | ANON | POST `/auth/reset-password` | correo válido **sin** código verificado | 400/401/403 | 422 | 200 |
| NEG-A16 | EST | WS `/biometric/ws?token={sessionId}` | handshake con `Upgrade: websocket` | 101 | — | 401 con token válido |
| NEG-A17 | ANON | WS `/biometric/ws` | sin token | 401 | — | 101 |
| NEG-A18 | ANON | WS `/biometric/ws?token=00000000-0000-0000-0000-000000000000` | token inexistente | 401 | — | 101 |
| NEG-A19 | ANON | GET `/auth/me?username=<usuario>` | sin token: no debe devolver datos de la persona | 401 | — | 200 con `personId`, estado o último acceso |

### A2 · Permisos por rol (acciones prohibidas)

| ID | Rol | Método y ruta | Cuerpo / nota | Esperado | Aceptable | Falla si |
|---|---|---|---|---|---|---|
| NEG-B01 | EST | GET `/users` | — | 403 | — | 200 |
| NEG-B02 | EST | POST `/users` | usuario válido | 403 | — | 2xx |
| NEG-B03 | EST | POST `/persons` | persona válida | 403 | — | 2xx |
| NEG-B04 | EST | PUT `/persons/{PROPIO_EST.person}` | cambiar su teléfono (**autoedición**: si el producto la permite, un 200 es `DUDA`, no `FAIL`) | 403 | — | 2xx → `DUDA` |
| NEG-B04b | EST | PUT `/persons/{COMPA.person}` | editar a **otra** persona | 403 | 404 | 2xx |
| NEG-B05 | EST | PATCH `/users/{PROPIO_EST.user}/status?status=false` | desactivarse | 403 | — | 2xx |
| NEG-B06 | EST | POST `/users/{PROPIO_EST.user}/roles` | `{"roleId":{ROL_ADMIN}}` | 403 | — | 2xx |
| NEG-B07 | EST | POST `/roles` | `{"roleName":"QA-X"}` | 403 | — | 2xx |
| NEG-B08 | EST | POST `/schools` | sede válida | 403 | — | 2xx |
| NEG-B09 | EST | POST `/courses` | curso válido | 403 | — | 2xx |
| NEG-B10 | EST | POST `/attendance-records` | marcarse asistencia | 403 | — | 2xx |
| NEG-B11 | EST | POST `/class-sessions` | abrir una sesión | 403 | — | 2xx |
| NEG-B12 | EST | PATCH `/justifications/{JUS_propia}/review` | aprobar la propia | 403 | — | 2xx |
| NEG-B13 | EST | DELETE `/justification-types/{id}` | — | 403 | — | 2xx |
| NEG-B14 | EST | PUT `/configurations/security/{id}` | cambiar seguridad | 403 | — | 2xx |
| NEG-B15 | DOC | POST `/users` | usuario válido | 403 | — | 2xx |
| NEG-B16 | DOC | PATCH `/users/{estudiante propio}/status?status=false` | — | 403 | — | 2xx |
| NEG-B17 | DOC | POST `/users/{id}/roles` | `{"roleId":{ROL_ADMIN}}` | 403 | — | 2xx |
| NEG-B18 | DOC | POST `/schools`, POST `/roles`, POST `/courses` | cuerpos válidos | 403 | — | 2xx |
| NEG-B19 | DOC | POST `/environments` | ambiente válido | 403 | — | 2xx |
| NEG-B20 | DOC | PUT `/configurations/academic/{id}` | — | 403 | — | 2xx |
| NEG-B21 | DOC | PATCH `/justifications/{JUS_de su ficha}/review` | aprobar | 200/204 | — | 403 (debería poder) |
| NEG-B22 | ADM | POST `/users/{id}/roles` | `{"roleId":{ROL_ADMIN}}` | 403 | — | 2xx |
| NEG-B23 | ADM | POST `/users/{id}/roles` | `{"roleId":{ROL_SUPER}}` | 403 | — | 2xx |
| NEG-B24 | ADM | DELETE `/users/{id}/roles/{ROL_ADMIN}` | quitar rol privilegiado | 403 | — | 2xx |
| NEG-B25 | ADM | POST `/schools` | sede válida | 403 | — | 2xx |
| NEG-B26 | ADM | POST `/roles`, DELETE `/roles/{id}`, POST `/roles/{id}/permissions` | — | 403 | — | 2xx |
| NEG-B27 | ADM | DELETE `/courses/{curso de su sede}` | solo Super Admin | 403 | — | 2xx |
| NEG-B28 | ADM | POST `/users/{id estudiante}/roles` | rol *Aprendiz* a un estudiante nuevo | 201 | — | 403 (debería poder) |

### A3 · Aislamiento entre sedes y entre personas

| ID | Rol | Método y ruta | Cuerpo / nota | Esperado | Aceptable | Falla si |
|---|---|---|---|---|---|---|
| NEG-C01 | ADM (SENA) | GET `/users/{U_ICT}` | — | 403 | 404 | 200 |
| NEG-C02 | ADM | GET `/persons/{P_ICT}` | — | 403 | 404 | 200 |
| NEG-C03 | ADM | PUT `/persons/{P_ICT}` | cambiar teléfono | 403 | 404 | 2xx; **sin cambio** |
| NEG-C04 | ADM | PATCH `/users/{U_ICT}/status?status=false` | — | 403 | 404 | 2xx; **sin cambio** |
| NEG-C05 | ADM | GET `/users` | listado | solo SENA | — | aparece alguien de ICT/SMP |
| NEG-C06 | ADM | GET `/persons` | listado | solo SENA | — | aparece alguien de ICT/SMP |
| NEG-C07 | ADM | PUT `/courses/{C_ICT}` | — | 403 | 404 | 2xx; **sin cambio** |
| NEG-C08 | ADM | GET `/cohorts/{COH_ICT}/enrollments` | — | 403 | 404, lista vacía | filas de ICT |
| NEG-C09 | ADM | POST `/enrollments` | ficha `{COH_ICT}` | 403 | 404, 400 | 201 |
| NEG-C10 | ADM | GET `/environments` | listado | solo SENA | — | ambientes de ICT |
| NEG-C11 | ADM | GET `/schedule-blocks` | listado | solo SENA | — | bloques de ICT |
| NEG-C12 | ADM | GET `/schools/{ICT}/actors` | — | 403 | 404 | 200 con filas |
| NEG-C13 | ADM | GET `/attendance-records?classSessionId={SES_ICT}` | — | 403 | 404, lista vacía | filas de ICT |
| NEG-C14 | ADM | GET `/biometric/facial/{P_ICT}` | — | 403 | 404 | 200 |
| NEG-C15 | ADM | GET `/alerts` | listado | solo SENA | — | alertas de ICT |
| NEG-C16 | ADM | PATCH `/justifications/{JUS_ICT}/review` | aprobar | 403 | 404 | 2xx |
| NEG-C17 | ADM2 (ICT) | repetir C01–C16 hacia **SENA** | simetría | igual que arriba | — | igual que arriba |
| NEG-C18 | DOC (ICT) | GET `/persons/{estudiante de SMP}` | — | 403 | 404 | 200 |
| NEG-C19 | DOC | GET `/academic-actors/{actor SMP}/attendance` | — | 403 | 404 | 200 |
| NEG-C20 | DOC | POST `/class-sessions` | bloque de **otro** instructor | 403 | 400 | 201 |
| NEG-C21 | DOC | POST `/attendance-records` | sesión de otra sede | 403 | 404 | 201 |
| NEG-C22 | EST | GET `/academic-actors/{COMPA}/attendance` | — | 403 | 404 | 200 |
| NEG-C23 | EST | GET `/academic-actors/{PROPIO}/attendance` | — | 200 solo propios | — | 403 / filas ajenas |
| NEG-C24 | EST | GET `/persons/{COMPA.person}` | — | 403 | 404 | 200 |
| NEG-C25 | EST | POST `/biometric/update-request` | persona de **otro** estudiante | 403 | 404 | 201 |
| NEG-C26 | EST (SMP) | GET `/justifications` | listado | solo propias | — | ajenas |
| NEG-C27 | SUP | GET `/users` | listado | todas las sedes | — | filtra a una sola sede |

### A4 · Validación de entrada (cualquier rol con permiso, normalmente `ADM`)

| ID | Método y ruta | Cuerpo | Esperado | Aceptable | Falla si |
|---|---|---|---|---|---|
| NEG-D01 | POST `/persons` | falta `name` | 400 | 422 | 5xx / 201 |
| NEG-D02 | POST `/persons` | `documentType` = `"XX"` (fuera de CC/TI/CE/PP) | 400 | 422 | 5xx / 201 |
| NEG-D03 | POST `/persons` | `bloodType` = `"Z+"` | 400 | 422 | 5xx / 201 |
| NEG-D04 | POST `/persons` | correo sin `@` | 400 | 422 | 5xx / 201 |
| NEG-D05 | POST `/persons` | mismo `documentType`+`documentNumber` ya existente | 409 | 400 | 5xx / 201 |
| NEG-D06 | POST `/persons` | mismo correo de una persona viva | 409 | 400 | 5xx / 201 |
| NEG-D07 | POST `/persons` | `name` de 10 000 caracteres | 400 | 413, 422 | 5xx / 201 |
| NEG-D08 | POST `/persons` | `name` = `"<script>alert(1)</script>"` | 201 (se guarda como texto) | 400 | 5xx; luego **se ejecuta** en pantalla (ver B) |
| NEG-D09 | POST `/persons` | JSON malformado (`{"name":`) | 400 | 422 | 5xx |
| NEG-D10 | POST `/persons` | `Content-Type: text/plain` | 400/415 | 422 | 5xx |
| NEG-D11 | POST `/users` | `password` de 3 caracteres | 400 | 422 | 5xx / 201 |
| NEG-D12 | POST `/users` | `username` repetido | 409 | 400 | 5xx / 201 |
| NEG-D13 | POST `/users` | `personId` inexistente | 404 | 400 | 5xx / 201 |
| NEG-D14 | POST `/users` | segundo usuario para la **misma persona** | 409 | 400 | 5xx / 201 |
| NEG-D15 | GET `/persons/not-a-uuid` | — | 400 | 404 | 5xx |
| NEG-D16 | GET `/persons/{UUID inexistente}` | — | 404 | — | 5xx / 200 |
| NEG-D17 | GET `/courses/99999999` | — | 404 | — | 5xx |
| NEG-D18 | GET `/academic-actors?limit=-1` | — | 400 o corrige a un valor válido | — | 5xx |
| NEG-D19 | GET `/academic-actors?limit=100000` | — | recorta a **100** | 400 | 5xx / devuelve > 100 |
| NEG-D20 | GET `/academic-actors?offset=-5` | — | 400 o 0 | — | 5xx |
| NEG-D21 | GET `/persons?q=' OR 1=1 --` | — | resultados normales o vacío | 400 | 5xx / vuelca todo |
| NEG-D22 | POST `/academic-periods` | `startsOn` ≥ `endsOn` | 400 | 422 | 5xx / 201 |
| NEG-D23 | POST `/courses` | mismo `code` en el mismo programa | 409 | 400 | 5xx / 201 |
| NEG-D24 | POST `/environments` | `capacity` = 0 | 400 | 422 | 5xx / 201 |
| NEG-D25 | POST `/environments` | `capacity` = -5 | 400 | 422 | 5xx / 201 |
| NEG-D26 | POST `/schedule-blocks` | `dayOfWeek` = 0 y = 8 | 400 | 422 | 5xx / 201 |
| NEG-D27 | POST `/schedule-blocks` | `startsAt` ≥ `endsAt` | 400 | 422 | 5xx / 201 |
| NEG-D28 | POST `/schedule-blocks` | misma sala/día/hora que otro | 409 | 400 | 5xx / 201 |
| NEG-D29 | POST `/schedule-blocks` | mismo instructor/día/hora que otro | 409 | 400 | 5xx / 201 |
| NEG-D30 | POST `/enrollments` | mismo actor + misma ficha | 409 | 400 | 5xx / 201 |
| NEG-D31 | POST `/biometric/fingerprint/enroll-sample` | `finger_number` = 11 | 400 | 422 | 5xx / 201 |
| NEG-D32 | POST `/schools` | mismo `code` | 409 | 400 | 5xx / 201 |
| NEG-D33 | POST `/justification-types` | mismo nombre en la misma sede **y** nombre igual a un tipo global | 409 | 400 | 5xx / 201 |

### A5 · Estado y reglas de negocio

> **Nota de vigencia:** hasta el 2026-10-23 este caso decía lo contrario (`FINGERPRINT` inválido, `IOT` válido). El método de captura `IOT` se sustituyó por `FINGERPRINT`; los resultados anteriores en `resultados/` son evidencia histórica y no se modifican.

| ID | Rol | Método y ruta | Cuerpo / nota | Esperado | Aceptable | Falla si |
|---|---|---|---|---|---|---|
| NEG-E01 | DOC | POST `/class-sessions/{id}/open` | sesión ya **Cerrada** | 400 | 409 | 2xx |
| NEG-E02 | DOC | POST `/class-sessions/{id}/close` | sesión ya **Cerrada** | 400 | 409 | 2xx |
| NEG-E03 | DOC | POST `/class-sessions/{id}/cancel` | sesión ya **Cerrada** | 400 | 409 | 2xx |
| NEG-E04 | DOC | POST `/class-sessions` | mismo bloque + misma fecha dos veces | 409 | 400 | 5xx / segunda 201 |
| NEG-E05 | DOC | POST `/attendance-records` | mismo actor + misma sesión dos veces | 409 | 400 | 5xx / segunda 201 |
| NEG-E06 | DOC | POST `/attendance-records` | sesión **Cerrada** o **Cancelada** | 400 | 409 | 201 |
| NEG-E07 | DOC | POST `/attendance-records` | `captureMethod` = `IOT` (ya no existe; fue renombrado a `FINGERPRINT`) | 400 | 422 | 5xx / 201 |
| NEG-E08 | DOC | POST `/attendance-records` | `matchScore` = 5 (fuera de rango) | 400 | 422 | 5xx / 201 |
| NEG-E09 | EST | POST `/justifications` | misma falta dos veces | 409 | 400 | 5xx / 201 |
| NEG-E10 | EST | POST `/justifications` | falta de **otra persona** | 403 | 404 | 201 |
| NEG-E11 | DOC | PATCH `/justifications/{id}/review` | aprobar tipo que exige adjunto sin documento | 400 | 422 | 2xx |
| NEG-E12 | DOC | PATCH `/justifications/{id}/review` | ya **Aprobada** → Rechazar | 400 | 409 | 2xx |
| NEG-E13 | DOC | PATCH `/justifications/{id}/review` | estado inventado `"Maybe"` | 400 | 422 | 5xx / 2xx |
| NEG-E14 | ADM | PATCH `/enrollments/{id}/status` | estado inventado | 400 | 422 | 5xx / 2xx |
| NEG-E15 | ADM | PATCH `/users/{id}/status?status=abc` | valor no booleano | 400 | — | 5xx / 2xx |
| NEG-E16 | ADM | PUT `/persons/{id}` | dos veces seguidas con el mismo cuerpo | 200/204 ambas | — | 5xx |
| NEG-E17 | ADM | POST `/enrollments` | actor **inactivo** | 400 | 409 | 201 |
| NEG-E18 | ADM | POST `/users` | persona **inactiva** | 400 | 409 | 201 |
| NEG-E19 | ADM | PUT con `row_version` desfasado (si el API lo expone) | bloqueo optimista | 409 | 412 | 2xx (sobrescribe) |
| NEG-E20 | cualquiera | cualquier `DELETE` | tras ejecutarlo, `SELECT` de la fila | la fila existe con `deleted_at` lleno | — | la fila **desapareció** (borrado físico) |

---

## B. Casos de interfaz

Se ejecutan sobre la pantalla indicada, con el rol que la usa (prioridad Super Admin → Admin → Docente → Estudiante).
Veredictos según §6 de `00-PROTOCOLO.md`.

### B1 · Entradas inválidas en formularios (cualquier formulario de alta/edición)

| ID | Qué hacer | Esperado | Falla si |
|---|---|---|---|
| NEG-F01 | Enviar con **todos** los campos vacíos | Validación por campo, sin petición | Se envía o error crudo |
| NEG-F02 | Solo **espacios** en un campo obligatorio | Se trata como vacío | Se guarda en blanco |
| NEG-F03 | Texto de **200+ caracteres** en un campo corto | Se limita o muestra error claro | 5xx / el diseño se rompe |
| NEG-F04 | Nombre con `<script>alert(1)</script>` | Se muestra **como texto** en lista, detalle y modales | Se ejecuta / aparece una alerta |
| NEG-F05 | Nombre con tildes, ñ, apóstrofo (`O'Brien`), emoji | Se guarda y muestra igual | Se corrompe (`Ã±`) |
| NEG-F06 | Correo sin `@`, con espacios, en mayúsculas | Validación o se normaliza a minúsculas | Se guarda inválido |
| NEG-F07 | Documento con letras donde se espera número | Validación | 5xx |
| NEG-F08 | Fecha inexistente (31/02) o invertida | Validación | 5xx / se guarda |
| NEG-F09 | Número negativo o `0` donde no cabe (capacidad, horas) | Validación | Se guarda |
| NEG-F10 | Pegar un texto enorme (50 KB) | No congela la pestaña | Se congela |

### B2 · Comportamiento del usuario

| ID | Qué hacer | Esperado | Falla si |
|---|---|---|---|
| NEG-G01 | **Doble clic** rápido en *Guardar / Importar / Iniciar clase / Enviar* | Una sola petición; botón bloqueado | Dos peticiones / registro duplicado |
| NEG-G02 | **F5** a mitad de un envío | Sin duplicados al volver; estado consistente | Dato a medias |
| NEG-G03 | Botón **Atrás** del navegador desde un modal o formulario | Cierra o vuelve sin perder la sesión | Pantalla en blanco |
| NEG-G04 | Abrir la misma pantalla en **dos pestañas**, cerrar sesión en una y pulsar un botón en la otra | La otra pide iniciar sesión | Sigue operando con el token viejo > 15 s |
| NEG-G05 | Borrar el token del almacenamiento del navegador y pulsar un botón | Vuelve a `/login` con mensaje | Pantalla en blanco / spinner infinito |
| NEG-G06 | Simular **sin red** (DevTools → Offline) y pulsar **Guardar** | Mensaje de error; el formulario conserva lo escrito | Dice "¡Guardado!" |
| NEG-G07 | **Detener un microservicio** (lo hace el supervisor) y recargar su pantalla | Aviso de error legible | Spinner infinito / pantalla en blanco |
| NEG-G08 | Respuesta **lenta** (limitar red a "Slow 3G") | Indicador de carga; sin doble envío | Acciones duplicadas |
| NEG-G09 | Abrir una **URL con id inexistente** (`/app/users/99999999`) | Mensaje o redirección | Pantalla en blanco |
| NEG-G10 | **Zoom 200 %** y ancho 375 px | Nada tapado; botones accesibles | Botón fuera de alcance |
| NEG-G11 | Navegar **solo con teclado** (Tab, Enter, Esc) por un modal | Foco visible; Esc cierra; Enter envía | Atrapa el foco / no cierra |
| NEG-G12 | Cambiar **idioma** a mitad de un formulario | Textos traducidos; no pierde lo escrito | Claves crudas |
| NEG-G13 | Cambiar a **modo oscuro** con un modal abierto | Texto legible | Texto invisible |

### B3 · Datos extremos en pantalla

| ID | Qué hacer | Esperado | Falla si |
|---|---|---|---|
| NEG-H01 | Lista con **0 filas** | Estado vacío con mensaje | Cabecera sola / spinner |
| NEG-H02 | Lista con **> 100 filas** (la API corta en 100) | Se recorren todas las páginas; el total es exacto | Falta gente o el total = 100 |
| NEG-H03 | Persona con **campos nulos** (sin teléfono, dirección, correo) | Muestra "—" | `null`, `undefined`, `NaN` |
| NEG-H04 | Estudiante **sin horario** (sin curso asignado) | *Programa* muestra "—" sin romper | Error / fila rota |
| NEG-H05 | Nombre de **80 caracteres** | Se recorta con "…" sin romper la tabla | Desborda la pantalla |
| NEG-H06 | Asistencia **0 %** y **100 %** | Barra y color correctos | `NaN %` / color incorrecto |
| NEG-H07 | Estudiante con **0 registros** de asistencia | "0 %" o "Sin datos" | `NaN` |
| NEG-H08 | Fechas de **otro huso** (cerca de medianoche) | La fecha mostrada coincide con la local | Un día de diferencia |
