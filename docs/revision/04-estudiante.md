# Lista de verificación 4/4 — Estudiante (Aprendiz)

Cuenta: `est.ict` (sede **ICT**). Cuenta de contraste: `est.smp` (sede **SMP**) — ver `00-PROTOCOLO.md` §3.
Menú esperado: **Inicio, Historial, Justificaciones, Mis cursos, Académico, Alertas, Configuración**.
No debe ver: *Usuarios, Ambientes, Reportes, Sedes, Roles*, ni el kiosco. Reglas y veredictos: `00-PROTOCOLO.md` §2 y §6.

Unidades:
`ES-AUT` · `ES-DSH` · `ES-HIS` · `ES-JUS` · `ES-CUR` · `ES-ACA` · `ES-ALE` · `ES-CFG` · `ES-BIO` · `ES-AIS` · `ES-RUT`

---

## ES-AUT · Sesión

- [ ] **ES-AUT-01** `[AUTO]` Iniciar sesión como `est.ict`.
  - Esperado: login 200; Dashboard "Mi Panel Personal"; nombre y etiqueta "Estudiante".
  - Falla si: pantalla en blanco tras entrar (antes pasaba por `thresholds is not defined`).
- [ ] **ES-AUT-02** `[AUTO]` Revisar el menú.
  - Esperado: exactamente las entradas del encabezado.
  - Falla si: aparece *Usuarios, Ambientes, Reportes, Sedes* o *Roles*.
- [ ] **ES-AUT-03** `[AUTO]` **Cerrar sesión** y entrar de nuevo.
  - Esperado: sin banner de error.
- [ ] **ES-AUT-04** `[AUTO]` Iniciar sesión con la cuenta de un estudiante **inactivo** (`QA-` desactivado).
  - Esperado: rechaza con mensaje claro; no entra.

## ES-DSH · Dashboard personal

- [ ] **ES-DSH-01** `[AUTO]` Abrir `/app` y esperar la carga.
  - Esperado: tarjetas (estado de hoy, mi asistencia, ambiente actual, instructor, clases de hoy, mis cursos) y gráficas semanal/diaria; sin `NaN/undefined`.
- [ ] **ES-DSH-02** `[API]` Comparar **Mi asistencia** con la base:
  `SELECT attendance_status, count(*) FROM attendance.attendance_record WHERE academic_actor_id=<actor de est.ict> AND deleted_at IS NULL GROUP BY 1;`
  - Esperado: el porcentaje corresponde a (Presente + Tarde + Justificado) / total. Si no, `DUDA`.
- [ ] **ES-DSH-03** `[AUTO]` Revisar las tarjetas **Instructor** y **Clases hoy**.
  - Esperado: el instructor mostrado es un docente real de la ficha, **no** el propio estudiante; "Clases hoy" no tiene formato raro (`49/0`). Si ocurre, `FAIL` Media con captura.
- [ ] **ES-DSH-04** `[AUTO]` Estudiante **sin matrícula o sin asistencia** (cuenta `QA-`).
  - Esperado: estado vacío claro; sin pantalla en blanco.
- [ ] **ES-DSH-05** `[AUTO]` Ancho de 768 y 375 px.
  - Esperado: sin scroll horizontal; tarjetas legibles.
- [ ] **ES-DSH-06** `[AUTO]` Consola tras la carga.
  - Esperado: sin `Uncaught`.

## ES-HIS · Historial (propio)

- [ ] **ES-HIS-01** `[AUTO]` Abrir **Historial**.
  - Esperado: solo **sus** registros (fecha, curso, estado, método).
  - Falla si: aparece asistencia de otra persona (**Bloqueante**).
- [ ] **ES-HIS-02** `[AUTO]` Filtrar por fechas → **Consultar**; **Limpiar**.
  - Esperado: resultados coherentes; **Limpiar** restaura.
- [ ] **ES-HIS-03** `[AUTO]` **Siguiente / Anterior**.
  - Esperado: sin repetidos.
- [ ] **ES-HIS-04** `[AUTO]` En una **falta** pulsar **Justificar**.
  - Esperado: abre el formulario con **ese** registro ya seleccionado.
- [ ] **ES-HIS-05** `[AUTO]` En un registro **Presente** buscar **Justificar**.
  - Esperado: no se ofrece (solo faltas/tardanzas).
- [ ] **ES-HIS-06** `[AUTO]` Rango sin registros.
  - Esperado: estado vacío claro.

## ES-JUS · Justificaciones (envío)

- [ ] **ES-JUS-01** `[AUTO]` Abrir **Justificaciones**.
  - Esperado: sus justificaciones con estado (Pendiente/Aprobada/Rechazada); botón **Nueva justificación**; filtros por estado funcionan.
- [ ] **ES-JUS-02** `[AUTO]` **Nueva justificación** → **Enviar** con campos vacíos.
  - Esperado: validación; sin `POST`.
- [ ] **ES-JUS-03** `[AUTO]` Enviar una justificación válida para una falta `QA-` (motivo de 20+ caracteres).
  - Esperado: `POST` de justificación 201; queda **Pendiente**; aparece en la lista; persiste tras F5.
- [ ] **ES-JUS-04** `[AUTO]` Enviar **otra** justificación para **la misma** falta.
  - Esperado: error legible (solo una por registro); no se duplica.
- [ ] **ES-JUS-05** `[AUTO]` Elegir un tipo que **exige adjunto** y enviar sin adjunto.
  - Esperado: mensaje que pide el adjunto o queda pendiente de adjuntar; sin 5xx.
- [ ] **ES-JUS-06** `[MANUAL]` Adjuntar un archivo (PDF/imagen) y enviar.
  - Esperado: el adjunto se lista con nombre y tamaño; acepta solo tipos permitidos.
- [ ] **ES-JUS-07** `[AUTO]` **Retirar** una justificación Pendiente (confirmar).
  - Esperado: sale de pendientes; la falta queda disponible para justificar de nuevo.
- [ ] **ES-JUS-08** `[AUTO]` Buscar **Retirar** en una justificación **ya resuelta**.
  - Esperado: no aparece.
- [ ] **ES-JUS-09** `[AUTO]` Buscar botones **Aprobar / Rechazar / Evaluar** y *Tipos de justificación*.
  - Esperado: **ninguno** aparece para el estudiante.
- [ ] **ES-JUS-10** `[AUTO]` Cancelar el modal a medias.
  - Esperado: no se envía nada; al reabrir el formulario está limpio.

## ES-CUR · Mis cursos

- [ ] **ES-CUR-01** `[AUTO]` Abrir **Mis cursos**.
  - Esperado: solo los cursos de **su ficha**; sin botones de gestión (**Nuevo curso**, **Editar**, **Eliminar**, **Importar**).
- [ ] **ES-CUR-02** `[AUTO]` Detalle de un curso.
  - Esperado: datos legibles; solo lectura.
- [ ] **ES-CUR-03** `[AUTO]` Estudiante sin cursos.
  - Esperado: estado vacío claro.

## ES-ACA · Académico (solo lectura)

- [ ] **ES-ACA-01** `[AUTO]` Abrir **Académico**.
  - Esperado: información de su ficha/programa/período; sin `undefined`.
- [ ] **ES-ACA-02** `[AUTO]` Buscar botones **Nueva ficha, Nuevo programa, Nuevo período, Editar, Activar, Desactivar, Eliminar, Matrículas**.
  - Esperado: **ninguno** visible.
  - Falla si: alguno aparece (**Alta**) o permite una acción.

## ES-ALE · Alertas

- [ ] **ES-ALE-01** `[AUTO]` Abrir **Alertas**.
  - Esperado: solo **sus** alertas; contador coherente o "Sin alertas".
- [ ] **ES-ALE-02** `[AUTO]` **Ver resueltas / Ocultar**, **Actualizar**, **Ver**.
  - Esperado: funcionan sin errores.
- [ ] **ES-ALE-03** `[AUTO]` Buscar **Marcar resuelta** y **Eliminar**.
  - Esperado: no deberían aparecer (gestionar alertas es de admin); si aparecen, `DUDA`.

## ES-CFG · Configuración (limitada)

- [ ] **ES-CFG-01** `[AUTO]` Abrir **Configuración**.
  - Esperado: solo **Notificaciones** y **Apariencia** (y la sede con instructores en lectura).
  - Falla si: aparecen controles editables de *General, Reconocimiento* o *Seguridad*.
- [ ] **ES-CFG-02** `[AUTO]` **Notificaciones**: cambiar un interruptor y **Guardar cambios**.
  - Esperado: "¡Guardado!"; persiste tras F5; revertir.
- [ ] **ES-CFG-03** `[AUTO]` **Apariencia**: claro/oscuro y acento.
  - Esperado: ningún texto invisible.
- [ ] **ES-CFG-04** `[AUTO]` Información de la **sede**.
  - Esperado: se ve en lectura, con instructores; **sin** botón **Editar**.

## ES-BIO · Biometría propia

- [ ] **ES-BIO-01** `[AUTO]` Abrir `/app/biometrics` por URL.
  - Esperado: registrar comportamiento (no tiene menú); solo puede operar sobre **sí mismo**. `FAIL` si queda en blanco.
- [ ] **ES-BIO-02** `[MANUAL]` Con cámara: **Registrar rostro (3 gestos)** sobre sí mismo.
  - Esperado: pasos y mensajes claros; **Apagar cámara** la libera; **no** hay selector de otra persona.
- [ ] **ES-BIO-03** `[API]` `POST /biometric/update-request` con su propia persona.
  - Esperado: 201 con estado `Pending`; con la persona de **otro** estudiante → 403.

## ES-AIS · Permisos y aislamiento

Datos de contraste: un compañero de la misma ficha y `est.smp` (otra sede).

- [ ] **ES-AIS-01** `[API]` `GET /users` → **403**.
- [ ] **ES-AIS-02** `[API]` `GET /persons/{compañero}` → 403/404.
- [ ] **ES-AIS-03** `[API]` `GET /academic-actors/{compañero}/attendance` → 403/404.
- [ ] **ES-AIS-04** `[API]` `GET /academic-actors/{propio}/attendance` → **200** con **solo** sus registros.
- [ ] **ES-AIS-05** `[API]` `POST /users`, `POST /persons`, `PATCH /users/{id}/status`, `PUT /persons/{otro}` → todos **403**.
- [ ] **ES-AIS-06** `[API]` `POST /users/{propio}/roles` con rol *Administrador* → **403** (escalada de privilegios).
- [ ] **ES-AIS-07** `[API]` `POST /attendance-records` (marcarse asistencia) → **403**.
- [ ] **ES-AIS-08** `[API]` `POST /justifications/{id}/review` (aprobar la propia) → **403**.
- [ ] **ES-AIS-09** `[API]` `GET /academic-actors/{alguien de SMP}/attendance` → 403/404.

## ES-RUT · Rutas que no le corresponden

Abrir por URL. Esperado en todas: **"Acceso denegado"** o redirección; **nunca** datos de gestión ni pantalla en blanco.

- [ ] **ES-RUT-01** `[AUTO]` `/app/users` y `/app/users/students`
- [ ] **ES-RUT-02** `[AUTO]` `/app/environments`
- [ ] **ES-RUT-03** `[AUTO]` `/app/reports`
- [ ] **ES-RUT-04** `[AUTO]` `/app/kiosk`
- [ ] **ES-RUT-05** `[AUTO]` `/app/schools` y `/app/roles`
- [ ] **ES-RUT-06** `[AUTO]` `/app/settings/general` (sección de administración) — no debe permitir editar.
- [ ] **ES-RUT-07** `[AUTO]` `/app/contacts`, `/app/school-info` — registrar comportamiento.
