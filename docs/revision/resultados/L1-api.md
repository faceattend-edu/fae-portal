# L1 · Matriz de API (casos negativos)

- Fecha: 2026-10-19T16:03:23.126Z
- Web: http://localhost:8090 · API: http://localhost:8000/api/v1
- Duración: 76 s
- API: http://localhost:8000/api/v1

| ID | Veredicto | Severidad | Rol / ruta | Petición | Evidencia | Nota |
|---|---|---|---|---|---|---|
| NEG-A01 | PASS |  | ANON | GET /persons → 401 | {"status":401,"error":"Unauthorized","message":"Missing or invalid bearer token","path":"/api/v1/persons"} |  |
| NEG-A02 | PASS |  | ANON | GET /users → 401 | {"status":401,"error":"Unauthorized","message":"Missing or invalid bearer token","path":"/api/v1/users"} |  |
| NEG-A03 | PASS |  | ANON | GET /academic-actors → 401 | {"status":401,"error":"Unauthorized","message":"Missing or invalid bearer token","path":"/api/v1/academic-actors"} |  |
| NEG-A04 | PASS |  | ANON | GET /attendance-records → 401 | {"status":401,"error":"Unauthorized","message":"Missing or invalid bearer token","path":"/api/v1/attendance-records"} |  |
| NEG-A05 | PASS |  | EST | GET /persons → 401 | {"status":401,"error":"Unauthorized","message":"Missing or invalid bearer token","path":"/api/v1/persons"} |  |
| NEG-A06 | PASS |  | EST | GET /persons → 401 | {"status":401,"error":"Unauthorized","message":"Invalid or expired session","path":"/api/v1/persons"} |  |
| NEG-A07 | PASS |  | EST | GET /persons → 401 | {"status":401,"error":"Unauthorized","message":"Missing or invalid bearer token","path":"/api/v1/persons"} |  |
| NEG-A08 | PASS |  | EST | GET /persons → 401 | {"status":401,"error":"Unauthorized","message":"Missing or invalid bearer token","path":"/api/v1/persons"} |  |
| NEG-A09 | PASS |  | EST | GET /persons (token cerrado, +11 s) → 401 | {"status":401,"error":"Unauthorized","message":"Invalid or expired session","path":"/api/v1/persons"} |  |
| NEG-A10 | PASS |  | ANON | POST /auth/login → 400 | {"error":"VALIDATION_ERROR","message":"identifier: identifier is required, password: password is required","status":400, |  |
| NEG-A11 | PASS |  | ANON | POST /auth/login → 401 | {"error":"UNAUTHORIZED","message":"Invalid email or password","status":401,"path":"/api/v1/auth/login","timestamp":"2026 |  |
| NEG-A12 | PASS |  | ANON | POST /auth/login → 401 | {"error":"UNAUTHORIZED","message":"Invalid email or password","status":401,"path":"/api/v1/auth/login","timestamp":"2026 |  |
| NEG-A13 | PASS |  | ANON | POST /auth/login → 401 | {"error":"UNAUTHORIZED","message":"Invalid email or password","status":401,"path":"/api/v1/auth/login","timestamp":"2026 |  |
| NEG-A14 | PASS |  | ANON | POST /auth/forgot-password → 202 | {"status":"accepted","message":"Request accepted"} |  |
| NEG-A15 | PASS |  | ANON | POST /auth/reset-password → 401 | {"error":"UNAUTHORIZED","message":"Verification code required","status":401,"path":"/api/v1/auth/reset-password","timest |  |
| NEG-A16 | PASS |  | EST | WS /biometric/ws?token → 101 |  |  |
| NEG-A17 | PASS |  | ANON | WS /biometric/ws → 401 |  |  |
| NEG-A18 | PASS |  | ANON | WS token inexistente → 401 |  |  |
| NEG-A19 | PASS |  | ANON | GET /auth/me → 401 | {"status":401,"error":"Unauthorized","message":"Missing or invalid bearer token","path":"/api/v1/auth/me"} | GET /auth/me es público en Kong y devuelve personId, estado y último acceso de cualquier usuario |
| NEG-B01 | PASS |  | EST | GET /users → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires identity.person:write","path":"/api/v1/users"} |  |
| NEG-B02 | PASS |  | EST | POST /users → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires identity.person:write","path":"/api/v1/users"} |  |
| NEG-B03 | PASS |  | EST | POST /persons → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires identity.person:write","path":"/api/v1/persons"} |  |
| NEG-B04 | DUDA |  | EST | PUT /persons/e91fa255-b786-45c7-b9fb-58a6eae008d9 → 200 | {"address":null,"birthDate":null,"bloodType":null,"documentNumber":"10981000000","documentType":null,"email":"valentina. | el estudiante pudo editar SU PROPIA persona: confirmar si la autoedición es intencional |
| NEG-B04b | PASS |  | EST | PUT /persons/d5cae1b8-7a9a-493d-8b84-5168f947a43c → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires identity.person:write","path":"/api/v1/persons/d5cae1b8 |  |
| NEG-B05 | PASS |  | EST | PATCH /users/bb02db16-83e2-4891-bda0-fcc9c9c528d2/status → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires identity.person:write","path":"/api/v1/users/bb02db16-8 |  |
| NEG-B06 | PASS |  | EST | POST /users/bb02db16-83e2-4891-bda0-fcc9c9c528d2/roles → 403 |  |  |
| NEG-B07 | PASS |  | EST | POST /roles → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires Super Administrador","path":"/api/v1/roles"} |  |
| NEG-B08 | PASS |  | EST | POST /schools → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires academic.school:manage","path":"/api/v1/schools"} |  |
| NEG-B09 | PASS |  | EST | POST /courses → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires academic.school:manage","path":"/api/v1/courses"} |  |
| NEG-B10 | PASS |  | EST | POST /attendance-records → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires attendance.record:write","path":"/api/v1/attendance-rec |  |
| NEG-B11 | PASS |  | EST | POST /class-sessions → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires scheduling.session:manage","path":"/api/v1/class-sessio |  |
| NEG-B12 | PASS |  | EST | PATCH /justifications/999999999/review → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires attendance.justification:approve","path":"/api/v1/justi |  |
| NEG-B13 | PASS |  | EST | DELETE /justification-types/999999999 → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires configuration:manage","path":"/api/v1/justification-typ |  |
| NEG-B14 | PASS |  | EST | PUT /configurations/security/999999999 → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires configuration:manage","path":"/api/v1/configurations/se |  |
| NEG-B15 | PASS |  | DOC | POST /users → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires identity.person:write","path":"/api/v1/users"} |  |
| NEG-B16 | PASS |  | DOC | PATCH /users/bb02db16-83e2-4891-bda0-fcc9c9c528d2/status → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires identity.person:write","path":"/api/v1/users/bb02db16-8 |  |
| NEG-B17 | PASS |  | DOC | POST /users/ffef1d25-6161-49f9-988f-ce1780ff255d/roles → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires configuration:manage","path":"/api/v1/users/ffef1d25-61 |  |
| NEG-B18 | PASS |  | DOC | POST /schools → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires academic.school:manage","path":"/api/v1/schools"} |  |
| NEG-B18b | PASS |  | DOC | POST /roles → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires Super Administrador","path":"/api/v1/roles"} |  |
| NEG-B18c | PASS |  | DOC | POST /courses → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires academic.school:manage","path":"/api/v1/courses"} |  |
| NEG-B19 | PASS |  | DOC | POST /environments → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires academic.school:manage","path":"/api/v1/environments"} |  |
| NEG-B20 | PASS |  | DOC | PUT /configurations/academic/999999999 → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires configuration:manage","path":"/api/v1/configurations/ac |  |
| NEG-B21 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-B22 | PASS |  | ADM | POST /users/ffef1d25-6161-49f9-988f-ce1780ff255d/roles → 403 |  |  |
| NEG-B23 | PASS |  | ADM | POST /users/ffef1d25-6161-49f9-988f-ce1780ff255d/roles → 403 |  |  |
| NEG-B24 | PASS |  | ADM | DELETE /users/ffef1d25-6161-49f9-988f-ce1780ff255d/roles/1 → 403 |  |  |
| NEG-B25 | PASS |  | ADM | POST /schools → 403 | {"error":"Forbidden","message":"Only Super Administrador manages schools","timestamp":"2026-10-19T16:02:57.710Z"} |  |
| NEG-B26 | PASS |  | ADM | POST /roles → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: requires Super Administrador","path":"/api/v1/roles"} |  |
| NEG-B27 | PASS |  | ADM | DELETE /courses/112 → 403 | {"error":"Forbidden","message":"Only Super Administrador deletes courses","timestamp":"2026-10-19T16:02:57.799Z"} |  |
| NEG-B28 | PASS |  | ADM | POST /users/ffef1d25-6161-49f9-988f-ce1780ff255d/roles → 201 |  |  |
| NEG-C01 | PASS |  | ADM | GET /users/bb02db16-83e2-4891-bda0-fcc9c9c528d2 → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: user outside your scope","path":"/api/v1/users/bb02db16-83e2-489 |  |
| NEG-C02 | PASS |  | ADM | GET /persons/e91fa255-b786-45c7-b9fb-58a6eae008d9 → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: person outside your scope","path":"/api/v1/persons/e91fa255-b786 |  |
| NEG-C03 | PASS |  | ADM | PUT /persons/e91fa255-b786-45c7-b9fb-58a6eae008d9 → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: person outside your scope","path":"/api/v1/persons/e91fa255-b786 |  |
| NEG-C04 | PASS |  | ADM | PATCH /users/bb02db16-83e2-4891-bda0-fcc9c9c528d2/status → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: user outside your scope","path":"/api/v1/users/bb02db16-83e2-489 |  |
| NEG-C05 | PASS |  | ADM | GET /users (26 filas) → 200 | todas de su sede |  |
| NEG-C06 | PASS |  | ADM | GET /persons (26 filas) → 200 | todas de su sede |  |
| NEG-C07 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-C08 | PASS |  | ADM | GET /cohorts/1/enrollments → 403 | {"error":"Forbidden","message":"You can only manage your own school","timestamp":"2026-10-19T16:02:58.970Z"} |  |
| NEG-C09 | PASS |  | ADM | POST /enrollments → 403 | {"error":"Forbidden","message":"You can only manage your own school","timestamp":"2026-10-19T16:02:59.002Z"} |  |
| NEG-C10 | PASS |  | ADM | GET /environments (3 filas) → 200 | todas de su sede |  |
| NEG-C11 | PASS |  | ADM | GET /schedule-blocks (3 filas) → 200 | todas de su sede |  |
| NEG-C12 | PASS |  | ADM | GET /schools/1/actors → 403 | {"error":"Forbidden","message":"You can only manage your own school","timestamp":"2026-10-19T16:03:00.504Z"} |  |
| NEG-C13 | DUDA |  | ADM | GET /class-sessions/1/attendance → 200 | [] | 200 con lista vacía: el servicio no valida el alcance, pero no hay datos que filtrar en esa sesión |
| NEG-C14 | PASS |  | ADM | GET /biometric/facial/e91fa255-b786-45c7-b9fb-58a6eae008d9 → 404 | {"error":"NotFound","message":"no active facial template","path":"/api/v1/biometric/facial/e91fa255-b786-45c7-b9fb-58a6e | aceptable 404 |
| NEG-C15 | PASS |  | ADM | GET /alerts (0 filas) → 200 | todas de su sede |  |
| NEG-C16 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-C17-01 | PASS |  | ADM2 | GET /users/c5c08567-bc84-4349-9184-c12a13dd521b → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: user outside your scope","path":"/api/v1/users/c5c08567-bc84-434 |  |
| NEG-C17-02 | PASS |  | ADM2 | GET /persons/9c2e72e6-7757-45c3-9e18-752eb5ff7d92 → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: person outside your scope","path":"/api/v1/persons/9c2e72e6-7757 |  |
| NEG-C17-03 | PASS |  | ADM2 | PUT /persons/9c2e72e6-7757-45c3-9e18-752eb5ff7d92 → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: person outside your scope","path":"/api/v1/persons/9c2e72e6-7757 |  |
| NEG-C17-04 | PASS |  | ADM2 | PATCH /users/c5c08567-bc84-4349-9184-c12a13dd521b/status → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: user outside your scope","path":"/api/v1/users/c5c08567-bc84-434 |  |
| NEG-C17-05 | PASS |  | ADM2 | GET /users (42 filas) → 200 | todas de su sede |  |
| NEG-C17-06 | PASS |  | ADM2 | GET /persons (841 filas) → 200 | todas de su sede |  |
| NEG-C17-08 | PASS |  | ADM2 | GET /cohorts/25/enrollments → 403 | {"error":"Forbidden","message":"You can only manage your own school","timestamp":"2026-10-19T16:03:14.560Z"} |  |
| NEG-C17-09 | PASS |  | ADM2 | POST /enrollments → 403 | {"error":"Forbidden","message":"You can only manage your own school","timestamp":"2026-10-19T16:03:14.611Z"} |  |
| NEG-C17-10 | PASS |  | ADM2 | GET /environments (12 filas) → 200 | todas de su sede |  |
| NEG-C17-11 | PASS |  | ADM2 | GET /schedule-blocks (146 filas) → 200 | todas de su sede |  |
| NEG-C17-12 | PASS |  | ADM2 | GET /schools/3/actors → 403 | {"error":"Forbidden","message":"You can only manage your own school","timestamp":"2026-10-19T16:03:15.758Z"} |  |
| NEG-C17-13 | DUDA |  | ADM2 | GET /class-sessions/1213/attendance → 200 | [] | 200 con lista vacía: el servicio no valida el alcance, pero no hay datos que filtrar en esa sesión |
| NEG-C17-14 | PASS |  | ADM2 | GET /biometric/facial/9c2e72e6-7757-45c3-9e18-752eb5ff7d92 → 404 | {"error":"NotFound","message":"no active facial template","path":"/api/v1/biometric/facial/9c2e72e6-7757-45c3-9e18-752eb | aceptable 404 |
| NEG-C17-15 | PASS |  | ADM2 | GET /alerts (0 filas) → 200 | todas de su sede |  |
| NEG-C18 | PASS |  | DOC | GET /persons/9c2e72e6-7757-45c3-9e18-752eb5ff7d92 → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: person outside your scope","path":"/api/v1/persons/9c2e72e6-7757 |  |
| NEG-C19 | PASS |  | DOC | GET /academic-actors/1829/attendance → 403 | {"error":{"code":"ISO-8.2-AUTH-001","message":"Forbidden: outside your scope","correlationId":"d0f70101-34f7-4a28-9dc5-8 |  |
| NEG-C20 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-C21 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-C22 | PASS |  | EST | GET /academic-actors/8/attendance → 403 | {"error":{"code":"ISO-8.2-AUTH-001","message":"Forbidden: outside your scope","correlationId":"513bfdce-22af-44b1-a5b7-3 |  |
| NEG-C23 | PASS |  | EST | GET /academic-actors/1/attendance → 200 | [{"academicActorId":1,"attendanceRecordId":1,"attendanceStatus":"Present","captureMethod":"FACIAL","capturedAt":"2026-10 | 54 registros propios |
| NEG-C24 | PASS |  | EST | GET /persons/d5cae1b8-7a9a-493d-8b84-5168f947a43c → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: person outside your scope","path":"/api/v1/persons/d5cae1b8-7a9a |  |
| NEG-C25 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-C26 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-C27 | PASS |  | SUP | GET /users (137) → 200 | 3 sedes visibles |  |
| NEG-D01 | PASS |  | ADM | POST /persons → 400 | {"error":"VALIDATION_ERROR","message":"name: must not be blank","status":400,"path":"/api/v1/persons","timestamp":"2026- |  |
| NEG-D02 | PASS |  | ADM | POST /persons → 400 | {"error":"VALIDATION_ERROR","message":"documentType: must be one of CC, TI, CE, PP","status":400,"path":"/api/v1/persons |  |
| NEG-D03 | PASS |  | ADM | POST /persons → 400 | {"error":"VALIDATION_ERROR","message":"Unknown blood type: Z+","status":400,"path":"/api/v1/persons","timestamp":"2026-1 |  |
| NEG-D04 | PASS |  | ADM | POST /persons → 400 | {"error":"VALIDATION_ERROR","message":"email: must be a well-formed email address","status":400,"path":"/api/v1/persons" |  |
| NEG-D05 | PASS |  | ADM | POST /persons → 409 | {"error":"CONFLICT","message":"Resource violates a uniqueness constraint","status":409,"path":"/api/v1/persons","timesta |  |
| NEG-D06 | PASS |  | ADM | POST /persons → 409 | {"error":"CONFLICT","message":"Resource violates a uniqueness constraint","status":409,"path":"/api/v1/persons","timesta |  |
| NEG-D07 | PASS |  | ADM | POST /persons → 400 | {"error":"VALIDATION_ERROR","message":"name: size must be between 0 and 255","status":400,"path":"/api/v1/persons","time |  |
| NEG-D08 | PASS |  | ADM | POST /persons → 201 | {"address":null,"birthDate":null,"bloodType":null,"documentNumber":"QArk4uhld0898156","documentType":"CC","email":"qa-rk | guardado como texto: verificar B/NEG-F04 en pantalla |
| NEG-D09 | PASS |  | ADM | POST /persons → 400 | {"error":"VALIDATION_ERROR","message":"Malformed request body","status":400,"path":"/api/v1/persons","timestamp":"2026-1 |  |
| NEG-D10 | PASS |  | ADM | POST /persons → 415 | {"error":"UNSUPPORTED_MEDIA_TYPE","message":"Unsupported Media Type","status":415,"path":"/api/v1/persons","timestamp":" |  |
| NEG-D11 | PASS |  | ADM | POST /users → 400 | {"error":"VALIDATION_ERROR","message":"password: password must be between 8 and 100 characters","status":400,"path":"/ap |  |
| NEG-D12 | PASS |  | ADM | POST /users → 409 | {"error":"CONFLICT","message":"User already exists with value=qa.rk4uhl","status":409,"path":"/api/v1/users","timestamp" |  |
| NEG-D13 | PASS |  | ADM | POST /users → 404 | {"error":"NOT_FOUND","message":"Person not found with identifier=00000000-0000-4000-8000-0000000000aa","status":404,"pat |  |
| NEG-D14 | PASS |  | ADM | POST /users → 409 | {"error":"CONFLICT","message":"Resource violates a uniqueness constraint","status":409,"path":"/api/v1/users","timestamp |  |
| NEG-D15 | PASS |  | ADM | GET /persons/not-a-uuid → 400 | {"error":"VALIDATION_ERROR","message":"Invalid value for parameter 'id'","status":400,"path":"/api/v1/persons/not-a-uuid |  |
| NEG-D16 | PASS |  | ADM | GET /persons/00000000-0000-4000-8000-0000000000bb → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: person outside your scope","path":"/api/v1/persons/00000000-0000 | aceptable 403 |
| NEG-D17 | PASS |  | ADM | GET /courses/99999999 → 404 | {"error":"NotFound"} |  |
| NEG-D18 | PASS |  | ADM | GET /academic-actors → 200 | [{"academicActorId":1683,"personId":"27309d88-cdea-4fcc-90c1-407cc420fa3c","actorTypeId":3,"schoolId":3,"actorCode":"ADM |  |
| NEG-D19 | PASS |  | ADM | GET /academic-actors → 200 | [{"academicActorId":1683,"personId":"27309d88-cdea-4fcc-90c1-407cc420fa3c","actorTypeId":3,"schoolId":3,"actorCode":"ADM |  |
| NEG-D20 | PASS |  | ADM | GET /academic-actors → 200 | [{"academicActorId":1683,"personId":"27309d88-cdea-4fcc-90c1-407cc420fa3c","actorTypeId":3,"schoolId":3,"actorCode":"ADM |  |
| NEG-D21 | PASS |  | ADM | GET /persons → 200 | {"data":[{"address":null,"birthDate":null,"bloodType":null,"documentNumber":"1069402330","documentType":null,"email":"lu |  |
| NEG-D22 | PASS |  | ADM | POST /academic-periods → 400 | {"error":"BadRequest","message":"Value rejected by check constraint 'chk_period_dates' on academic.academic_period","det |  |
| NEG-D23 | PASS |  | ADM | POST /courses → 409 | {"error":"Conflict","message":"academic.course already exists (uq_course_code_program)","details":{"entity":"academic.co |  |
| NEG-D24 | PASS |  | ADM | POST /environments → 400 | {"error":"capacity: must be greater than 0"} |  |
| NEG-D25 | PASS |  | ADM | POST /environments → 400 | {"error":"capacity: must be greater than 0"} |  |
| NEG-D26 | PASS |  | ADM | POST /schedule-blocks → 400 | {"error":"dayOfWeek: must be less than or equal to 7"} |  |
| NEG-D27 | PASS |  | ADM | POST /schedule-blocks → 400 | {"error":"startsAt must be before endsAt"} |  |
| NEG-D28 | PASS |  | ADM | POST /schedule-blocks → 409 | {"error":"Environment 26 is already booked between 06:00 and 12:00 on day 1"} |  |
| NEG-D29 | PASS |  | ADM | POST /schedule-blocks → 404 | {"error":"Environment not found with identifier=100026"} | aceptable 404 |
| NEG-D30 | PASS |  | ADM | POST /enrollments → 409 | {"error":"Conflict","message":"academic.enrollment already exists (uq_enrollment_actor_cohort)","details":{"entity":"aca |  |
| NEG-D31 | BLOQUEADO |  | ADM |  |  | requiere captura biométrica real (L3) |
| NEG-D32 | PASS |  | SUP | POST /schools → 409 | {"error":"Conflict","message":"academic.school already exists (school_code_key)","details":{"entity":"academic.school"," |  |
| NEG-D33 | PASS |  | SUP | POST /justification-types → 409 | {"error":{"code":"ISO-8.5-DUP-001","message":"Global JustificationType already exists with name=Academic commitment","co |  |
| NEG-E01 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E02 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E03 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E04 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E05 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E06 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E07 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E08 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E09 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E10 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E11 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E12 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E13 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E14 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E15 | DUDA |  | ADM | PATCH /users/ffef1d25-6161-49f9-988f-ce1780ff255d/status → 403 | {"status":403,"error":"Forbidden","message":"Forbidden: user outside your scope","path":"/api/v1/users/ffef1d25-6161-49f | status real 403; esperado 400 |
| NEG-E16 | PASS |  | ADM | PUT /persons/3f4d2a89-87ff-4501-931d-b6733713e5e7 → 200 | {"address":null,"birthDate":null,"bloodType":null,"documentNumber":"QArk4uhlp29053","documentType":null,"email":"qa-rk4u | idempotente |
| NEG-E17 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E18 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E19 | BLOQUEADO |  | — |  |  | no cubierto por L1 (requiere datos o acción manual): se revisa en L3 |
| NEG-E20 | PASS |  | SUP | DELETE /courses/114 → 204 | deleted_at lleno: t |  |

## Resumen
114 PASS · 0 FAIL · 4 DUDA · 25 BLOQUEADO
