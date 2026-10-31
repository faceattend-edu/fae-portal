# Lista de verificación 3/4 — Docente (Instructor)

Cuenta: `maria.sanchez` (sede **ICT**) — ver `00-PROTOCOLO.md` §3.
Menú esperado: **Inicio, Historial, Justificaciones, Alumnos, Mis cursos, Ambientes, Alertas, Reportes, Configuración**.
Además llega al kiosco por el botón **Tomar asistencia** del Dashboard (no tiene entrada de menú).
No debe ver: *Sedes, Roles, Académico, Usuarios con gestión*. Reglas y veredictos: `00-PROTOCOLO.md` §2 y §6.

**Datos previos (supervisor):** María necesita una clase **hoy** con ficha de ≥ 20 matriculados. Sin eso, `DO-KIO` queda `BLOQUEADO`.

Unidades:
`DO-AUT` · `DO-DSH` · `DO-USR` · `DO-CUR` · `DO-AMB` · `DO-HIS` · `DO-JUS` · `DO-ALE` · `DO-REP` · `DO-CFG` · `DO-KIO` · `DO-BIO` · `DO-AIS` · `DO-RUT`

---

## DO-AUT · Sesión

- [ ] **DO-AUT-01** `[AUTO]` Iniciar sesión como `maria.sanchez`.
  - Esperado: login 200; Dashboard del docente (sus fichas), nombre y etiqueta "Docente".
- [ ] **DO-AUT-02** `[AUTO]` Revisar el menú.
  - Esperado: exactamente las entradas del encabezado; la de usuarios se llama **Alumnos** y la de cursos **Mis cursos**.
  - Falla si: aparece *Académico*, *Sedes* o *Roles*.
- [ ] **DO-AUT-03** `[AUTO]` **Cerrar sesión**; volver a entrar.
  - Esperado: sin banner de error en el login.

## DO-DSH · Dashboard del docente

- [ ] **DO-DSH-01** `[AUTO]` Abrir `/app` y esperar la carga completa.
  - Esperado: tarjetas del docente (asistencia hoy, mis fichas, mis estudiantes, en riesgo, clases de hoy…); **sin pantalla en blanco**.
  - Falla si: `Uncaught` (antes fallaba con `thresholds is not defined` cuando el docente tenía fichas).
- [ ] **DO-DSH-02** `[API]` Comparar "Mis fichas" y "Mis estudiantes" con la base:
  `SELECT count(DISTINCT b.cohort_id) FROM scheduling.schedule_block b WHERE b.instructor_actor_id=<actor de María> AND b.deleted_at IS NULL;`
  - Esperado: coincide; si no, `DUDA` con ambas cifras.
- [ ] **DO-DSH-03** `[AUTO]` Comprobar un docente **sin fichas** (si existe otra cuenta) o vaciar la vista.
  - Esperado: estado vacío claro, sin `NaN`.
- [ ] **DO-DSH-04** `[AUTO]` Pulsar **Tomar asistencia**.
  - Esperado: abre `/app/kiosk`.
- [ ] **DO-DSH-05** `[AUTO]` Ancho de 768 y 375 px.
  - Esperado: tarjetas reacomodadas, sin scroll horizontal.

## DO-USR · Alumnos (solo lectura)

- [ ] **DO-USR-01** `[AUTO]` Abrir **Alumnos**.
  - Esperado: solo estudiantes de **sus fichas**; título "Alumnos"; sin botones **Importar** ni **Nuevo**.
  - Falla si: hay estudiantes de otras fichas o sedes (**Bloqueante**).
- [ ] **DO-USR-02** `[AUTO]` Revisar que **no existan casillas** de selección ni barra de acciones en lote.
  - Esperado: ausentes.
- [ ] **DO-USR-03** `[AUTO]` Buscar un alumno; usar el filtro de curso/ficha; paginar.
  - Esperado: resultados y contador coherentes; "Anterior/Siguiente" funcionan.
- [ ] **DO-USR-04** `[AUTO]` Abrir el **detalle** de un alumno.
  - Esperado: modal con datos y asistencia; **no** hay **Editar**, **Desactivar**, **Activar** ni **Eliminar**; sin `Uncaught`.
- [ ] **DO-USR-05** `[AUTO]` Estado vacío: buscar un nombre inexistente.
  - Esperado: "Sin estudiantes/resultados".
- [ ] **DO-USR-06** `[AUTO]` Abrir `/app/users/teachers` y `/app/users/admins` por URL.
  - Esperado: no muestra docentes ni administradores de la sede; `DUDA` con captura si lo hace.

## DO-CUR · Mis cursos

- [ ] **DO-CUR-01** `[AUTO]` Abrir **Mis cursos**.
  - Esperado: solo los cursos/fichas que imparte; sin **Nuevo curso** ni **Importar**.
- [ ] **DO-CUR-02** `[AUTO]` **Ver estudiantes** de un curso.
  - Esperado: lleva a la lista de alumnos filtrada por esa ficha; cantidad coincide con matriculados.
- [ ] **DO-CUR-03** `[AUTO]` **Detalle** de un curso.
  - Esperado: datos legibles; **sin** Editar/Desactivar/Eliminar.
- [ ] **DO-CUR-04** `[AUTO]` Docente sin cursos.
  - Esperado: estado vacío, sin spinner infinito.

## DO-AMB · Ambientes (solo lectura)

- [ ] **DO-AMB-01** `[AUTO]` Abrir **Ambientes**.
  - Esperado: tarjetas de **su sede**; sin **Nuevo ambiente**.
- [ ] **DO-AMB-02** `[AUTO]` **Ver detalle** de un ambiente.
  - Esperado: horarios visibles; **no** hay **Editar ambiente**, **Añadir horario** ni **Eliminar**.

## DO-HIS · Historial

- [ ] **DO-HIS-01** `[AUTO]` Abrir **Historial**.
  - Esperado: registros solo de **sus fichas**; sin `undefined`.
- [ ] **DO-HIS-02** `[AUTO]` Filtrar por ficha/fechas → **Consultar**; **Limpiar**.
  - Esperado: resultados coherentes; **Limpiar** restaura.
- [ ] **DO-HIS-03** `[AUTO]` **Siguiente / Anterior**.
  - Esperado: sin filas repetidas.
- [ ] **DO-HIS-04** `[AUTO]` Filtro sin resultados.
  - Esperado: estado vacío claro.

## DO-JUS · Justificaciones (revisión)

- [ ] **DO-JUS-01** `[AUTO]` Abrir **Justificaciones**.
  - Esperado: pestañas **Pendientes / Resueltas** de **sus fichas**; **sin** botón *Nueva justificación*.
- [ ] **DO-JUS-02** `[AUTO]` **Ver detalle** de una pendiente.
  - Esperado: motivo, tipo, estudiante, fecha y adjuntos legibles.
- [ ] **DO-JUS-03** `[AUTO]` **Aprobar** una pendiente `QA-` válida.
  - Esperado: `…/review` 200/204; pasa a Resueltas con revisor y fecha.
- [ ] **DO-JUS-04** `[AUTO]` **Rechazar** otra con nota.
  - Esperado: queda Rechazada con la nota visible.
- [ ] **DO-JUS-05** `[AUTO]` **Aprobar** una cuyo tipo exige adjunto y **no** lo tiene.
  - Esperado: mensaje claro de que falta el adjunto; sigue pendiente.
- [ ] **DO-JUS-06** `[AUTO]` Buscar la gestión de **Tipos de justificación**.
  - Esperado: **no existe** para el docente (solo admin).
- [ ] **DO-JUS-07** `[API]` Revisar una justificación de **otra sede**.
  - Esperado: 403/404; sin cambios.

## DO-ALE · Alertas

- [ ] **DO-ALE-01** `[AUTO]` Abrir **Alertas**.
  - Esperado: solo las de sus estudiantes; contador coherente.
- [ ] **DO-ALE-02** `[AUTO]` **Ver resueltas / Ocultar**, **Actualizar**, **Ver**.
  - Esperado: cada uno funciona sin errores.
- [ ] **DO-ALE-03** `[AUTO]` Buscar **Marcar resuelta** y **Eliminar**.
  - Esperado: si el docente **no** gestiona alertas, no deben aparecer; si aparecen, `DUDA` (el permiso `canManageAlerts` es solo de admin).

## DO-REP · Reportes

- [ ] **DO-REP-01** `[AUTO]` Abrir **Reportes**.
  - Esperado: cifras de **sus fichas**; secciones disponibles cargan sin errores.
- [ ] **DO-REP-02** `[AUTO]` **Aplicar** y **Limpiar** filtros; rango invertido.
  - Esperado: sin 5xx; validación o vacío explícito.
- [ ] **DO-REP-03** `[AUTO]` Buscar **Notificar todos**.
  - Esperado: **no aparece** (solo admin).
- [ ] **DO-REP-04** `[MANUAL]` **Exportar** y comprobar el archivo.
  - Esperado: totales = pantalla.

## DO-CFG · Configuración (limitada)

- [ ] **DO-CFG-01** `[AUTO]` Abrir **Configuración**.
  - Esperado: solo **Notificaciones** y **Apariencia** (y datos de la sede en lectura).
  - Falla si: aparecen *General*, *Reconocimiento* o *Seguridad* con controles editables.
- [ ] **DO-CFG-02** `[AUTO]` **Notificaciones**: cambiar un interruptor y **Guardar cambios**.
  - Esperado: "¡Guardado!"; persiste tras F5; **revertir**.
- [ ] **DO-CFG-03** `[AUTO]` **Apariencia**: claro/oscuro y acento.
  - Esperado: ningún texto invisible.
- [ ] **DO-CFG-04** `[AUTO]` Forzar la URL de una sección de administración (`/app/settings/general`).
  - Esperado: no permite editar; `FAIL` Alta si guarda cambios.

## DO-KIO · Kiosco de asistencia (requiere clase hoy)

- [ ] **DO-KIO-01** `[AUTO]` Abrir `/app/kiosk`.
  - Esperado: tarjeta(s) de la clase de hoy con **curso, ficha, horario, ambiente y matriculados**; badge "Sin iniciar"; botón **Iniciar clase**.
  - Falla si: la tarjeta solo dice "Clase de hoy" sin datos pasados 40 s.
- [ ] **DO-KIO-02** `[AUTO]` Mientras cargan los detalles.
  - Esperado: muestra "Cargando…" y **no** ofrece botón ni estado falso.
- [ ] **DO-KIO-03** `[AUTO]` **Iniciar clase**.
  - Esperado: `POST /class-sessions` (si no existe) y `…/open` 200/201; pasa a "Kiosco de asistencia" con lista de estudiantes "(0/N)".
- [ ] **DO-KIO-04** `[AUTO]` Comprobar el canal biométrico.
  - Esperado: handshake `/api/v1/biometric/ws` con **101**; **no** queda el banner "Reconectando…".
- [ ] **DO-KIO-05** `[AUTO]` Con la cámara bloqueada o denegada.
  - Esperado: mensaje rojo visible ("permiso de la cámara denegado…") y botón **Reintentar cámara**. Falla si solo hay un recuadro negro mudo.
- [ ] **DO-KIO-06** `[AUTO]` Recargar la página con la clase abierta y volver a `/app/kiosk`.
  - Esperado: la tarjeta dice **En curso** (borde verde), "X de N" y botón **Continuar clase**; no ofrece "Iniciar clase".
- [ ] **DO-KIO-07** `[MANUAL]` Con cámara real: acercar un estudiante enrolado.
  - Esperado: banner "Asistencia registrada"; sube "Registrados"; la lista marca al estudiante con *Rostro*; el mismo estudiante otra vez → "Ya registrado".
- [ ] **DO-KIO-08** `[AUTO]` En un estudiante registrado pulsar **Tarde**.
  - Esperado: `PATCH /attendance-records/{id}` 200/204 y la lista muestra **Tarde**; el botón **Tarde** desaparece para él.
- [ ] **DO-KIO-09** `[AUTO]` Pulsar **Quitar** en un registro (confirmar).
  - Esperado: `DELETE` 200/204; sale de "registrados"; el contador baja.
- [ ] **DO-KIO-10** `[MANUAL]` Con cámara real, **cambiar de sección** (p. ej. Historial) y mirar el indicador de cámara.
  - Esperado: la luz/indicador de la cámara **se apaga**; al volver al kiosco se reactiva.
- [ ] **DO-KIO-11** `[AUTO]` **Finalizar clase**.
  - Esperado: `POST …/close` 200/204; vuelve a la lista con la tarjeta **Cerrada**, asistencia "X de N", "% presentes", **sin** botón.
- [ ] **DO-KIO-12** `[AUTO]` **Cancelar clase** en una segunda clase `QA-` (confirmar).
  - Esperado: `POST …/cancel` 200/204; tarjeta **Cancelada**; el canal biométrico se cierra.
- [ ] **DO-KIO-13** `[AUTO]` Intentar **Cancelar clase** con el backend de scheduling caído (**lo hace el supervisor**).
  - Esperado: aparece un mensaje de error; la clase **sigue abierta**.

## DO-BIO · Biometría por URL (`/app/biometrics`)

- [ ] **DO-BIO-01** `[AUTO]` Abrir `/app/biometrics`.
  - Esperado: registrar el comportamiento (no tiene menú). `FAIL` solo si queda en blanco.
- [ ] **DO-BIO-02** `[MANUAL]` Con cámara: **Dar acceso a la cámara**, **Registrar rostro (3 gestos)**, **Reconocer rostro (2 gestos)**, **Apagar cámara**.
  - Esperado: cada paso muestra su mensaje; **Apagar cámara** libera la cámara.
- [ ] **DO-BIO-03** `[MANUAL]` Con lector: **Detectar lector**, **Iniciar captura**, **Guardar muestra**, **Reconocer huella**, **Detener lector**.
  - Esperado: estados coherentes; sin errores de consola.

## DO-AIS · Aislamiento y permisos (el docente solo ve lo suyo)

Datos de contraste: un estudiante de **otra ficha** de ICT y uno de **otra sede**.

- [ ] **DO-AIS-01** `[API]` `GET /persons/{estudiante de otra sede}` → 403/404.
- [ ] **DO-AIS-02** `[API]` `GET /academic-actors/{actor de otra sede}/attendance` → 403/404.
- [ ] **DO-AIS-03** `[API]` `PATCH /users/{id}/status?status=false` sobre un estudiante propio → **403** (no es admin). El usuario sigue activo.
- [ ] **DO-AIS-04** `[API]` `POST /users` → 403. `POST /schools` → 403. `POST /roles` → 403.
- [ ] **DO-AIS-05** `[API]` `POST /users/{id}/roles` → 403.
- [ ] **DO-AIS-06** `[API]` `POST /class-sessions` con un `scheduleBlockId` de **otro instructor**.
  - Esperado: 403/400; no abre sesiones ajenas.
- [ ] **DO-AIS-07** `[API]` `POST /attendance-records` en una sesión de otra sede → 403/404.

## DO-RUT · Rutas fuera de su alcance

- [ ] **DO-RUT-01** `[AUTO]` `/app/schools` → Esperado "Acceso denegado".
- [ ] **DO-RUT-02** `[AUTO]` `/app/roles` → Esperado "Acceso denegado".
- [ ] **DO-RUT-03** `[AUTO]` `/app/academic` → registrar (no está en su menú); `DUDA` si carga gestión.
- [ ] **DO-RUT-04** `[AUTO]` `/app/school-info`, `/app/contacts` → registrar comportamiento.
