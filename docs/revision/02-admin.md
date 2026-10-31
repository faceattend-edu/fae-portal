# Lista de verificación 2/4 — Administrador de sede

Cuenta principal: `fermin` (sede **SENA**). Cuenta de contraste para aislamiento: `admin.ict` (sede **ICT**) — ver `00-PROTOCOLO.md` §3.
Menú esperado: **Inicio, Historial, Justificaciones, Usuarios (Estudiantes · Profesores · Admins), Cursos, Ambientes, Académico, Alertas, Reportes, Configuración**.
No debe ver: *Sedes* ni *Roles*. Reglas y veredictos: `00-PROTOCOLO.md` §2 y §6. Registros creados: `QA-<ID>`.

Unidades:
`AD-AUT` · `AD-DSH` · `AD-USR` · `AD-IMP` · `AD-CUR` · `AD-AMB` · `AD-ACA` · `AD-HIS` · `AD-JUS` · `AD-ALE` · `AD-REP` · `AD-CFG` · `AD-KIO` · `AD-AIS` · `AD-RUT`

---

## AD-AUT · Sesión del administrador

- [ ] **AD-AUT-01** `[AUTO]` Iniciar sesión como `fermin` y llegar a `/app`.
  - Esperado: login 200, dashboard "Panel de Administración" con el nombre del usuario y la etiqueta de rol de sede.
- [ ] **AD-AUT-02** `[AUTO]` Revisar las entradas del menú lateral.
  - Esperado: exactamente las del encabezado; **no** aparecen *Sedes* ni *Roles*.
  - Falla si: aparece alguna de las dos.
- [ ] **AD-AUT-03** `[AUTO]` **Cerrar sesión** y volver a entrar.
  - Esperado: sin banner de error en el login; la segunda entrada funciona.

## AD-DSH · Dashboard de sede

- [ ] **AD-DSH-01** `[AUTO]` Abrir `/app` y esperar a que termine de cargar.
  - Esperado: tarjetas con números y listas (asistencia de instructores, mejores/peores cursos); sin `NaN/undefined`.
- [ ] **AD-DSH-02** `[API]` Comparar *total de estudiantes* con la sede SENA:
  `SELECT count(*) FROM academic.academic_actor a JOIN academic.academic_actor_type t ON t.actor_type_id=a.actor_type_id JOIN academic.school s ON s.school_id=a.school_id WHERE t.code='STUDENT' AND a.deleted_at IS NULL AND s.code=(SELECT code FROM academic.school WHERE school_id=<id de SENA>);`
  - Esperado: coincide con la tarjeta. Si no, `DUDA` con ambas cifras.
- [ ] **AD-DSH-03** `[AUTO]` Pulsar cada enlace/botón del panel (p. ej. **Tomar asistencia**).
  - Esperado: cada uno lleva a una ruta válida; sin pantalla en blanco.
- [ ] **AD-DSH-04** `[AUTO]` Revisar la consola.
  - Esperado: sin `Uncaught`.
- [ ] **AD-DSH-05** `[AUTO]` Ancho de 768 y 375 px.
  - Esperado: sin scroll horizontal; contenido legible.

## AD-USR · Usuarios de la sede

- [ ] **AD-USR-01** `[AUTO]` Abrir `/app/users`, `/students`, `/teachers`, `/admins`.
  - Esperado: solo personas de **SENA**; conteos de cada sección coinciden con la base.
  - Falla si: aparece alguien de ICT/SMP (**Bloqueante**).
- [ ] **AD-USR-02** `[AUTO]` Buscar por nombre existente / inexistente; ordenar; filtrar por estado.
  - Esperado: resultados y contador del encabezado coinciden con las filas.
- [ ] **AD-USR-03** `[AUTO]` Detalle de **un estudiante** (clic en la fila).
  - Esperado: modal con nombre, correo, documento, usuario, ficha, asistencia y estado; sin `Uncaught`.
  - Falla si: pantalla en blanco.
- [ ] **AD-USR-04** `[AUTO]` Del detalle, pulsar **Editar estudiante**.
  - Esperado: el formulario trae **nombre, apellido, documento, correo y usuario ya llenos**; *Vínculo académico* muestra código, sede y ficha.
  - Falla si: **Usuario** aparece vacío (bloquea el guardado).
- [ ] **AD-USR-05** `[AUTO]` Editar un `QA-` (teléfono → `3001234567`) y **Guardar cambios**.
  - Esperado: `PUT /persons/{id}` 200 y `PUT /users/{id}` 204; cierra el modal; el detalle muestra el teléfono; persiste tras F5.
- [ ] **AD-USR-06** `[AUTO]` Editar con el correo de **otra persona** existente.
  - Esperado: mensaje de duplicado legible; no cambia nada.
- [ ] **AD-USR-07** `[AUTO]` **Desactivar** un `QA-` (confirmar) y **Activar**.
  - Esperado: `PATCH …/status` 204 (usuario y persona); badge cambia y vuelve.
- [ ] **AD-USR-08** `[AUTO]` Detalle de un **administrador** y de un **docente**.
  - Esperado: el administrador no muestra bloque de asistencia/biometría; botones coherentes con su tipo (*Editar docente / administrador*).
- [ ] **AD-USR-09** `[AUTO]` Abrir **Editar** y buscar el selector de **rol**.
  - Esperado: **no** existe (solo lo tiene el Super Admin).
  - Falla si: aparece y permite cambiarlo.
- [ ] **AD-USR-10** `[AUTO]` **Nuevo estudiante** con datos `QA-` válidos.
  - Esperado: crea persona, actor, matrícula y usuario (peticiones 201); aparece en la lista con la ficha elegida. Mensaje "¡Guardado!".
- [ ] **AD-USR-11** `[AUTO]` **Nuevo estudiante** con el mismo documento.
  - Esperado: mensaje de duplicado legible; no se crea otra fila.
- [ ] **AD-USR-12** `[AUTO]` Marcar 2 `QA-` → **Desactivar** → Confirmar; luego **Activar**.
  - Esperado: "2 usuarios actualizados"; ambos cambian de estado; barra desaparece.
- [ ] **AD-USR-13** `[AUTO]` Marcar la **cuenta propia** + un `QA-` → **Desactivar**.
  - Esperado: se actualiza solo el `QA-`; aviso *No puedes aplicar esta acción a tu propia cuenta*; la cuenta propia sigue activa.
- [ ] **AD-USR-14** `[AUTO]` Casilla de cabecera "seleccionar todos" en una lista de > 10 filas.
  - Esperado: marca solo las filas **visibles de la página**; el contador de la barra coincide.
- [ ] **AD-USR-15** `[AUTO]` **Eliminar** un `QA-` desde el detalle.
  - Esperado: confirmación; queda inactivo (la fila **no** se borra de la base).

## AD-IMP · Importación de estudiantes

- [ ] **AD-IMP-01** `[AUTO]` **Importar** abre el modal con *Seleccionar archivo*, *Plantilla CSV*, *Curso destino* y *Ficha destino*.
  - Esperado: sin errores; *Ficha destino* aparece solo tras elegir curso.
- [ ] **AD-IMP-02** `[MANUAL]` Descargar **Plantilla CSV**.
  - Esperado: el archivo trae las columnas `name, code, email, course, grade, documentNumber, password, …`.
- [ ] **AD-IMP-03** `[MANUAL]` Subir un CSV sin `course` y **sin** elegir *Curso destino*.
  - Esperado: botón de importar no avanza o el resultado dice claramente que falta el curso (no solo "0 agregados").
- [ ] **AD-IMP-04** `[MANUAL]` Subir CSV de 3 filas `QA-` eligiendo curso y ficha.
  - Esperado: "3 agregados"; en **Usuarios** aparecen con la ficha elegida; la contraseña por defecto de importados es `Estudiante123!`.
- [ ] **AD-IMP-05** `[MANUAL]` Subir un CSV con **1 fila repetida** (mismo correo).
  - Esperado: la fila repetida sale como error con motivo legible; las demás se importan; **no** quedan registros a medias (persona sin usuario).
- [ ] **AD-IMP-06** `[MANUAL]` Subir un archivo **vacío** y otro con **extensión no soportada**.
  - Esperado: mensaje claro; sin pantalla en blanco.
- [ ] **AD-IMP-07** `[API]` Tras AD-IMP-04, verificar en la base que cada importado tiene persona, usuario, actor y matrícula:
  `SELECT p.email, u.username, a.actor_code, e.enrollment_status FROM identity.person p LEFT JOIN identity.app_user u ON u.person_id=p.person_id LEFT JOIN academic.academic_actor a ON a.person_id=p.person_id LEFT JOIN academic.enrollment e ON e.academic_actor_id=a.academic_actor_id WHERE p.email LIKE 'qa-%';`
  - Falla si: alguna columna viene `NULL`.

## AD-CUR · Cursos (`/app/courses`)

- [ ] **AD-CUR-01** `[AUTO]` Abrir **Cursos**.
  - Esperado: lista de cursos **de la sede** con nombre, código, programa y estado; sin `undefined`.
- [ ] **AD-CUR-02** `[AUTO]` **Nuevo curso** → **Registrar curso** con campos vacíos.
  - Esperado: validación; sin `POST`.
- [ ] **AD-CUR-03** `[AUTO]` Registrar `QA-CUR-03` en un programa existente.
  - Esperado: `POST /courses` 201; mensaje "¡Guardado!"; aparece en la lista.
- [ ] **AD-CUR-04** `[AUTO]` Registrar otro curso con el **mismo código en el mismo programa**.
  - Esperado: mensaje de duplicado legible. (El mismo código en **otro** programa sí es válido.)
- [ ] **AD-CUR-05** `[AUTO]` **Crear programa** desde el modal de curso con datos `QA-`.
  - Esperado: el programa nuevo queda seleccionable sin cerrar el modal.
- [ ] **AD-CUR-06** `[AUTO]` Abrir el **detalle** de `QA-CUR-03` → **Editar curso** → cambiar horas → **Guardar cambios**.
  - Esperado: `PUT /courses/{id}` 200/204; el detalle muestra el cambio; persiste tras F5.
- [ ] **AD-CUR-07** `[AUTO]` **Desactivar** y **Activar** `QA-CUR-03`.
  - Esperado: badge cambia; petición 200/204 en cada cambio.
- [ ] **AD-CUR-08** `[AUTO]` Buscar el botón **Eliminar** en el detalle del curso.
  - Esperado: **no aparece** (solo el Super Admin elimina cursos).
  - Falla si: está disponible para el administrador de sede.
- [ ] **AD-CUR-09** `[MANUAL]` **Importar cursos**: *Descargar plantilla*, subir 2 filas `QA-`, *Importar*; probar *Reintentar* con un error.
  - Esperado: resumen por fila; los errores traen motivo.

## AD-AMB · Ambientes (`/app/environments`)

- [ ] **AD-AMB-01** `[AUTO]` Abrir **Ambientes**.
  - Esperado: tarjetas con nombre, código, capacidad y estado; solo los de **SENA**.
- [ ] **AD-AMB-02** `[AUTO]` **Nuevo ambiente** → **Registrar ambiente** vacío.
  - Esperado: validación; sin `POST`.
- [ ] **AD-AMB-03** `[AUTO]` Registrar `QA-AMB-03` con capacidad 20.
  - Esperado: `POST /environments` 201; tarjeta nueva.
- [ ] **AD-AMB-04** `[AUTO]` Registrar otro con **el mismo código** en la sede.
  - Esperado: mensaje de duplicado legible.
- [ ] **AD-AMB-05** `[AUTO]` Capacidad **0** y **negativa**.
  - Esperado: no deja guardar (la base lo rechaza); mensaje claro, sin 5xx.
- [ ] **AD-AMB-06** `[AUTO]` **Ver detalle** → **Editar ambiente** → **Guardar cambios**.
  - Esperado: `PUT /environments/{id}` 200/204; persiste.
- [ ] **AD-AMB-07** `[AUTO]` **Añadir horario** → elegir ficha, curso, día y horas → **Guardar horario**.
  - Esperado: `POST /schedule-blocks` 201; el horario aparece en el detalle del ambiente.
- [ ] **AD-AMB-08** `[AUTO]` Añadir **otro horario que se cruce** en la misma sala/día/hora.
  - Esperado: mensaje de conflicto legible; no se crea.
- [ ] **AD-AMB-09** `[AUTO]` Añadir un horario con **hora fin ≤ hora inicio**.
  - Esperado: validación; sin 5xx.
- [ ] **AD-AMB-10** `[AUTO]` **Eliminar** `QA-AMB-03` y sus horarios `QA-`.
  - Esperado: confirmación; desaparece; en la base queda con `deleted_at`.

## AD-ACA · Académico (`/app/academic`)

- [ ] **AD-ACA-01** `[AUTO]` Abrir **Académico**.
  - Esperado: fichas, programas y períodos de la sede con estado; sin `undefined`.
- [ ] **AD-ACA-02** `[AUTO]` **Nuevo período** con inicio **posterior** al fin.
  - Esperado: validación; no se envía o el backend responde 400 con mensaje legible.
- [ ] **AD-ACA-03** `[AUTO]` Crear período `QA-ACA-03` válido.
  - Esperado: `POST /academic-periods` 201; aparece.
- [ ] **AD-ACA-04** `[AUTO]` **Nuevo programa** `QA-ACA-04`; repetir con el mismo código.
  - Esperado: el primero se crea; el segundo da duplicado legible.
- [ ] **AD-ACA-05** `[AUTO]` **Nueva ficha** `QA-ACA-05` con programa y período `QA-`; probar **Nuevo programa** / **Nuevo período** dentro del modal.
  - Esperado: `POST /cohorts` 201; "¡Guardado!"; la ficha aparece con su programa.
- [ ] **AD-ACA-06** `[AUTO]` **Editar** programa y período `QA-` y guardar.
  - Esperado: `PUT` 200/204; cambios visibles tras F5.
- [ ] **AD-ACA-07** `[AUTO]` **Matrículas** de una ficha: ver la lista.
  - Esperado: cantidad = `SELECT count(*) FROM academic.enrollment WHERE cohort_id=<id> AND deleted_at IS NULL;`.
- [ ] **AD-ACA-08** `[AUTO]` **Matricular** a un estudiante `QA-` en la ficha `QA-` y luego **Desmatricular**.
  - Esperado: `POST /enrollments` 201 y cambio de estado 200/204; el contador de la ficha sube y baja.
- [ ] **AD-ACA-09** `[AUTO]` **Matricular** al mismo estudiante dos veces en la misma ficha.
  - Esperado: mensaje de duplicado legible; no se duplica.
- [ ] **AD-ACA-10** `[AUTO]` **Desactivar / Activar / Eliminar** la ficha `QA-` (confirmar).
  - Esperado: cada cambio persiste; eliminar es lógico.

## AD-HIS · Historial

- [ ] **AD-HIS-01** `[AUTO]` Abrir **Historial**.
  - Esperado: registros **solo de SENA**; fecha, estudiante, estado, método sin `undefined`.
- [ ] **AD-HIS-02** `[AUTO]` Filtrar por ficha y fechas → **Consultar**; **Limpiar**.
  - Esperado: resultados coherentes; **Limpiar** restaura.
- [ ] **AD-HIS-03** `[AUTO]` **Siguiente / Anterior** en un historial largo.
  - Esperado: sin filas repetidas ni saltos.
- [ ] **AD-HIS-04** `[AUTO]` Filtro sin resultados.
  - Esperado: estado vacío claro.
- [ ] **AD-HIS-05** `[AUTO]` Abrir el historial de un estudiante desde Reportes/Usuarios (si hay enlace).
  - Esperado: modal con su detalle, botón **Cerrar** funciona.

## AD-JUS · Justificaciones

- [ ] **AD-JUS-01** `[AUTO]` Abrir **Justificaciones**.
  - Esperado: pestañas **Pendientes** y **Resueltas** con contadores coherentes con la base (`SELECT review_status, count(*) FROM attendance.justification WHERE deleted_at IS NULL GROUP BY 1;`, acotado a la sede).
- [ ] **AD-JUS-02** `[AUTO]` **Ver detalle** de una pendiente.
  - Esperado: motivo, tipo, fecha, estudiante y adjuntos legibles; **Cerrar** cierra.
- [ ] **AD-JUS-03** `[AUTO]` **Evaluar** → **Aprobar** una pendiente `QA-` **sin** adjunto cuando su tipo exige adjunto.
  - Esperado: se rechaza con mensaje claro (regla de negocio); sigue pendiente.
- [ ] **AD-JUS-04** `[AUTO]` **Aprobar** una pendiente válida.
  - Esperado: `PATCH/POST …/justifications/{id}/review` 200/204; pasa a *Resueltas* como Aprobada con revisor y fecha.
- [ ] **AD-JUS-05** `[AUTO]` **Rechazar** otra pendiente con una nota.
  - Esperado: pasa a Rechazada con la nota visible.
- [ ] **AD-JUS-06** `[AUTO]` Intentar **Aprobar** una ya resuelta.
  - Esperado: no se puede (botones ausentes o error legible).
- [ ] **AD-JUS-07** `[AUTO]` **Tipos de justificación**: **Nuevo tipo** `QA-JUS-07` → **Guardar**.
  - Esperado: `POST /justification-types` 201; aparece; el nombre duplicado da error legible.
- [ ] **AD-JUS-08** `[AUTO]` **Editar** el tipo `QA-` y **Guardar**; luego **Eliminar**.
  - Esperado: `PUT` 200/204; eliminar pide confirmación y deja el tipo sin uso inactivo.
- [ ] **AD-JUS-09** `[AUTO]` Revisar que el administrador **no** vea el botón **Nueva justificación** (solo aprendices).
  - Esperado: ausente.

## AD-ALE · Alertas

- [ ] **AD-ALE-01** `[AUTO]` Abrir **Alertas**.
  - Esperado: contador "N sin resolver" coherente; filas con tipo, severidad y fecha legibles.
- [ ] **AD-ALE-02** `[AUTO]` **Ver resueltas / Ocultar resueltas**, **Actualizar**.
  - Esperado: la lista cambia y refresca sin errores.
- [ ] **AD-ALE-03** `[AUTO]` **Marcar resuelta** una alerta `QA-`.
  - Esperado: sale de pendientes; el contador baja en 1; persiste.
- [ ] **AD-ALE-04** `[AUTO]` **Eliminar** una alerta `QA-` resuelta.
  - Esperado: confirmación; desaparece; en la base tiene `deleted_at`.
- [ ] **AD-ALE-05** `[AUTO]` Alertas de **otra sede**.
  - Esperado: **no** aparecen. Falla si aparece alguna (**Bloqueante**).

## AD-REP · Reportes

- [ ] **AD-REP-01** `[AUTO]` Abrir **Reportes** y recorrer todas sus secciones (*Históricos*, *Sanciones*, …).
  - Esperado: cada una carga sin errores; cifras solo de la sede.
- [ ] **AD-REP-02** `[AUTO]` **Aplicar** y **Limpiar** filtros.
  - Esperado: la lista y el contador cambian y se restauran.
- [ ] **AD-REP-03** `[AUTO]` Rango de fechas invertido.
  - Esperado: validación o resultado vacío; **sin 5xx**.
- [ ] **AD-REP-04** `[AUTO]` Abrir el modal **Exportar** y **Cancelar**.
  - Esperado: abre y cierra sin errores; no descarga nada.
- [ ] **AD-REP-05** `[MANUAL]` **Exportar** con un formato y comprobar el archivo.
  - Esperado: totales del archivo = pantalla.
- [ ] **AD-REP-06** `[MANUAL]` **Notificar todos**: **NO pulsar** (envía mensajes reales). Solo confirmar que existe y pide confirmación en entorno de prueba.

## AD-CFG · Configuración

- [ ] **AD-CFG-01** `[AUTO]` Abrir **General, Reconocimiento, Notificaciones, Seguridad, Apariencia** y **Sede**.
  - Esperado: todas cargan; sin errores de consola.
- [ ] **AD-CFG-02** `[AUTO]` **General**: cambiar un valor, **Guardar cambios**; luego **Descartar** otro cambio.
  - Esperado: "¡Guardado!" y persistencia tras F5 en el primero; **Descartar** revierte el segundo sin petición.
- [ ] **AD-CFG-03** `[AUTO]` **Reconocimiento**: cambiar el mínimo de asistencia y guardar.
  - Esperado: el cambio se refleja en estados/colores de Dashboard y Usuarios. **Revertir.**
- [ ] **AD-CFG-04** `[AUTO]` **Seguridad**: valor inválido (0, negativo, texto) en el tiempo de sesión.
  - Esperado: validación; no guarda.
- [ ] **AD-CFG-05** `[AUTO]` **Apariencia**: claro/oscuro y acento.
  - Esperado: ningún texto invisible en ningún modo.
- [ ] **AD-CFG-06** `[AUTO]` **Sede**: **Editar** → cambiar el teléfono → **Guardar**; luego **Cancelar** otro cambio.
  - Esperado: `PUT /schools/{id}` 200/204; persiste; **Cancelar** descarta. **Revertir.**
- [ ] **AD-CFG-07** `[AUTO]` Cambios hechos aquí **no** afectan a la sede ICT.
  - Esperado: la sede ICT conserva sus valores (verificar con `admin.ict` o con SQL).

## AD-KIO · Kiosco (el administrador no tiene clases propias)

- [ ] **AD-KIO-01** `[AUTO]` Abrir `/app/kiosk`.
  - Esperado: título "Tomar asistencia" y el estado vacío "No tienes clases programadas para hoy"; sin pantalla en blanco.
- [ ] **AD-KIO-02** `[AUTO]` Revisar consola y red.
  - Esperado: sin `Uncaught`; sin peticiones ≥ 400.

## AD-AIS · Aislamiento entre sedes (el administrador SENA no ve ICT/SMP)

Datos de contraste: identificar con SQL un `person_id`, `user_id`, `course_id`, `environment_id`, `cohort_id` y `class_session_id` que **pertenezcan a ICT**.

- [ ] **AD-AIS-01** `[API]` Con la sesión de `fermin`: `GET /users/{user_id de ICT}`.
  - Esperado: **403** (aceptable 404). Falla si 200.
- [ ] **AD-AIS-02** `[API]` `GET /persons/{person_id de ICT}`. Esperado: 403/404.
- [ ] **AD-AIS-03** `[API]` `PATCH /users/{user_id de ICT}/status?status=false`. Esperado: 403/404 y el usuario ICT **sigue activo** (verificar con SQL).
- [ ] **AD-AIS-04** `[API]` `PUT /courses/{course_id de ICT}`. Esperado: 403/404.
- [ ] **AD-AIS-05** `[API]` `GET /cohorts/{cohort_id de ICT}/enrollments`. Esperado: 403/404 o lista vacía.
- [ ] **AD-AIS-06** `[API]` `GET /environments` y `GET /schedule-blocks`. Esperado: **solo** elementos de SENA.
- [ ] **AD-AIS-07** `[API]` `POST /enrollments` con una ficha de ICT. Esperado: 403/404.
- [ ] **AD-AIS-08** `[AUTO]` Abrir por URL la pantalla de **ficha/curso/usuario** de ICT (si hay ruta con id). Esperado: sin datos de ICT.
- [ ] **AD-AIS-09** `[API]` `POST /users/{id}/roles` con el rol *Administrador* o *Super Administrador*.
  - Esperado: **403** (solo el Super Admin asigna roles privilegiados). Falla si 201.

## AD-RUT · Rutas que el administrador no debe usar

Abrir por URL. Esperado en las dos primeras: **"Acceso denegado"** (no datos, no pantalla en blanco). `FAIL` si carga datos de gestión.

- [ ] **AD-RUT-01** `[AUTO]` `/app/schools`
- [ ] **AD-RUT-02** `[AUTO]` `/app/roles`
- [ ] **AD-RUT-03** `[AUTO]` `/app/biometrics` — registrar comportamiento (no tiene menú); `DUDA` si carga.
- [ ] **AD-RUT-04** `[AUTO]` `/app/contacts` — registrar comportamiento.
- [ ] **AD-RUT-05** `[AUTO]` `/app/school-info` — registrar comportamiento.
