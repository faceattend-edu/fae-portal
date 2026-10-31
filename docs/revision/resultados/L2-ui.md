# L2 · Humo de interfaz por rol

- Fecha: 2026-10-19T16:08:54.905Z
- Web: http://localhost:8090 · API: http://localhost:8000/api/v1
- Duración: 331 s
- Capturas: resultados/ui/ (solo rutas no PASS)
- Modo: navegación interna

| ID | Veredicto | Severidad | Rol / ruta | Petición | Evidencia | Nota |
|---|---|---|---|---|---|---|
| L2-SUP-login | PASS |  | Super Admin | 118 peticiones |  |  |
| L2-SUP-/app | PASS |  | Super Admin · /app | 0 peticiones · 2427 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido |
| L2-SUP-/app/schools | PASS |  | Super Admin · /app/schools | 1 peticiones · 2416 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido |
| L2-SUP-/app/users | PASS |  | Super Admin · /app/users | 1 peticiones · 2429 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido |
| L2-SUP-/app/users/students | PASS |  | Super Admin · /app/users/students | 1 peticiones · 2663 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido |
| L2-SUP-/app/users/teachers | PASS |  | Super Admin · /app/users/teachers | 1 peticiones · 2428 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido |
| L2-SUP-/app/users/admins | PASS |  | Super Admin · /app/users/admins | 1 peticiones · 2662 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido |
| L2-SUP-/app/roles | PASS |  | Super Admin · /app/roles | 6 peticiones · 2414 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido |
| L2-SUP-/app/reports | PASS |  | Super Admin · /app/reports | 0 peticiones · 2668 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido |
| L2-SUP-/app/history | PASS |  | Super Admin · /app/history | 1 peticiones · 2664 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido |
| L2-SUP-/app/alerts | PASS |  | Super Admin · /app/alerts | 2 peticiones · 2425 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido |
| L2-SUP-/app/settings | PASS |  | Super Admin · /app/settings | 0 peticiones · 2416 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido |
| L2-SUP-/app/courses | PASS |  | Super Admin · /app/courses | 1 peticiones · 2402 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | Acceso denegado |
| L2-SUP-/app/environments | PASS |  | Super Admin · /app/environments | 0 peticiones · 2417 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | Acceso denegado |
| L2-SUP-/app/academic | PASS |  | Super Admin · /app/academic | 0 peticiones · 2412 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | Acceso denegado |
| L2-SUP-/app/justifications | PASS |  | Super Admin · /app/justifications | 0 peticiones · 2418 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | Acceso denegado |
| L2-SUP-/app/kiosk | DUDA |  | Super Admin · /app/kiosk | 0 peticiones · 2416 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido · carga con contenido (sin expectativa definida) |
| L2-SUP-/app/biometrics | DUDA |  | Super Admin · /app/biometrics | 1 peticiones · 2416 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido · carga con contenido (sin expectativa definida) |
| L2-SUP-/app/contacts | DUDA |  | Super Admin · /app/contacts | 0 peticiones · 2664 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido · carga con contenido (sin expectativa definida) |
| L2-SUP-/app/school-info | DUDA |  | Super Admin · /app/school-info | 1 peticiones · 2662 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido · carga con contenido (sin expectativa definida) |
| L2-SUP-/app/ruta-que-no-existe | DUDA |  | Super Admin · /app/ruta-que-no-existe | 0 peticiones · 2651 ms | Admin FaceAttend Super Admin  Inicio   Escuelas   Historial   Usuarios   Estudiantes  Prof | con contenido · carga con contenido (sin expectativa definida) |
| L2-ADM-login | PASS |  | Admin | 25 peticiones |  |  |
| L2-ADM-/app | PASS |  | Admin · /app | 0 peticiones · 2424 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido |
| L2-ADM-/app/users | PASS |  | Admin · /app/users | 0 peticiones · 2661 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido |
| L2-ADM-/app/users/students | PASS |  | Admin · /app/users/students | 0 peticiones · 2415 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido |
| L2-ADM-/app/users/teachers | PASS |  | Admin · /app/users/teachers | 0 peticiones · 2412 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido |
| L2-ADM-/app/users/admins | PASS |  | Admin · /app/users/admins | 0 peticiones · 2432 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido |
| L2-ADM-/app/courses | PASS |  | Admin · /app/courses | 0 peticiones · 2406 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido |
| L2-ADM-/app/environments | PASS |  | Admin · /app/environments | 0 peticiones · 2431 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido |
| L2-ADM-/app/academic | PASS |  | Admin · /app/academic | 1 peticiones · 2414 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido |
| L2-ADM-/app/history | PASS |  | Admin · /app/history | 1 peticiones · 2927 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido |
| L2-ADM-/app/justifications | PASS |  | Admin · /app/justifications | 5 peticiones · 3723 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido |
| L2-ADM-/app/alerts | PASS |  | Admin · /app/alerts | 2 peticiones · 2418 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido |
| L2-ADM-/app/reports | PASS |  | Admin · /app/reports | 0 peticiones · 2652 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido |
| L2-ADM-/app/settings | PASS |  | Admin · /app/settings | 2 peticiones · 2657 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido |
| L2-ADM-/app/kiosk | PASS |  | Admin · /app/kiosk | 0 peticiones · 2675 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido |
| L2-ADM-/app/schools | PASS |  | Admin · /app/schools | 1 peticiones · 2406 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | Acceso denegado |
| L2-ADM-/app/roles | PASS |  | Admin · /app/roles | 0 peticiones · 2433 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | Acceso denegado |
| L2-ADM-/app/biometrics | DUDA |  | Admin · /app/biometrics | 1 peticiones · 2657 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido · carga con contenido (sin expectativa definida) |
| L2-ADM-/app/contacts | DUDA |  | Admin · /app/contacts | 1 peticiones · 2919 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido · carga con contenido (sin expectativa definida) |
| L2-ADM-/app/school-info | DUDA |  | Admin · /app/school-info | 1 peticiones · 2413 ms | Fermín Beltrán Barragán Admin de sede  Inicio   Historial   Justificaciones   Usuarios   Es | con contenido · carga con contenido (sin expectativa definida) |
| L2-DOC-login | PASS |  | Docente | 59 peticiones |  |  |
| L2-DOC-/app | PASS |  | Docente · /app | 0 peticiones · 2411 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | con contenido |
| L2-DOC-/app/users | PASS |  | Docente · /app/users | 0 peticiones · 2655 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | con contenido |
| L2-DOC-/app/courses | PASS |  | Docente · /app/courses | 0 peticiones · 2406 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | con contenido |
| L2-DOC-/app/environments | PASS |  | Docente · /app/environments | 0 peticiones · 2431 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | con contenido |
| L2-DOC-/app/history | PASS |  | Docente · /app/history | 1 peticiones · 2672 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | con contenido |
| L2-DOC-/app/justifications | PASS |  | Docente · /app/justifications | 6 peticiones · 3715 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | con contenido |
| L2-DOC-/app/alerts | PASS |  | Docente · /app/alerts | 2 peticiones · 2427 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | con contenido |
| L2-DOC-/app/reports | PASS |  | Docente · /app/reports | 0 peticiones · 2425 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | con contenido |
| L2-DOC-/app/settings | PASS |  | Docente · /app/settings | 0 peticiones · 2416 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | con contenido |
| L2-DOC-/app/kiosk | PASS |  | Docente · /app/kiosk | 0 peticiones · 2424 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | con contenido |
| L2-DOC-/app/schools | PASS |  | Docente · /app/schools | 1 peticiones · 2655 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | Acceso denegado |
| L2-DOC-/app/roles | PASS |  | Docente · /app/roles | 0 peticiones · 2656 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | Acceso denegado |
| L2-DOC-/app/academic | PASS |  | Docente · /app/academic | 0 peticiones · 2422 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | Acceso denegado |
| L2-DOC-/app/users/teachers | DUDA |  | Docente · /app/users/teachers | 0 peticiones · 2420 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | con contenido · carga con contenido (sin expectativa definida) |
| L2-DOC-/app/users/admins | DUDA |  | Docente · /app/users/admins | 0 peticiones · 2665 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | con contenido · carga con contenido (sin expectativa definida) |
| L2-DOC-/app/biometrics | DUDA |  | Docente · /app/biometrics | 1 peticiones · 2648 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | con contenido · carga con contenido (sin expectativa definida) |
| L2-DOC-/app/contacts | DUDA |  | Docente · /app/contacts | 1 peticiones · 2431 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | con contenido · carga con contenido (sin expectativa definida) |
| L2-DOC-/app/school-info | DUDA |  | Docente · /app/school-info | 1 peticiones · 2417 ms | María Sánchez Urbina Docente  Inicio   Historial   Justificaciones   Alumnos   Estudiantes  | con contenido · carga con contenido (sin expectativa definida) |
| L2-EST-login | PASS |  | Estudiante | 70 peticiones |  |  |
| L2-EST-/app | PASS |  | Estudiante · /app | 0 peticiones · 2415 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | con contenido |
| L2-EST-/app/history | PASS |  | Estudiante · /app/history | 1 peticiones · 2678 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | con contenido |
| L2-EST-/app/justifications | PASS |  | Estudiante · /app/justifications | 5 peticiones · 3447 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | con contenido |
| L2-EST-/app/courses | PASS |  | Estudiante · /app/courses | 0 peticiones · 2417 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | con contenido |
| L2-EST-/app/academic | PASS |  | Estudiante · /app/academic | 1 peticiones · 2652 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | con contenido |
| L2-EST-/app/alerts | PASS |  | Estudiante · /app/alerts | 2 peticiones · 2420 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | con contenido |
| L2-EST-/app/settings | PASS |  | Estudiante · /app/settings | 0 peticiones · 2414 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | con contenido |
| L2-EST-/app/users | PASS |  | Estudiante · /app/users | 0 peticiones · 2656 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | Acceso denegado |
| L2-EST-/app/users/students | PASS |  | Estudiante · /app/users/students | 0 peticiones · 2406 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | Acceso denegado |
| L2-EST-/app/environments | PASS |  | Estudiante · /app/environments | 0 peticiones · 2417 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | Acceso denegado |
| L2-EST-/app/reports | PASS |  | Estudiante · /app/reports | 0 peticiones · 2405 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | Acceso denegado |
| L2-EST-/app/kiosk | PASS |  | Estudiante · /app/kiosk | 0 peticiones · 2434 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | Acceso denegado |
| L2-EST-/app/schools | PASS |  | Estudiante · /app/schools | 0 peticiones · 2423 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | Acceso denegado |
| L2-EST-/app/roles | PASS |  | Estudiante · /app/roles | 0 peticiones · 2418 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | Acceso denegado |
| L2-EST-/app/settings/general | DUDA |  | Estudiante · /app/settings/general | 0 peticiones · 2660 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | con contenido · carga con contenido (sin expectativa definida) |
| L2-EST-/app/biometrics | PASS |  | Estudiante · /app/biometrics | 0 peticiones · 2651 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | Acceso denegado |
| L2-EST-/app/contacts | DUDA |  | Estudiante · /app/contacts | 1 peticiones · 2419 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | con contenido · carga con contenido (sin expectativa definida) |
| L2-EST-/app/school-info | DUDA |  | Estudiante · /app/school-info | 1 peticiones · 2654 ms | Valentina García Ortiz Estudiante  Inicio   Historial   Justificaciones   Mis cursos   Acad | con contenido · carga con contenido (sin expectativa definida) |

## Resumen
63 PASS · 0 FAIL · 16 DUDA · 0 BLOQUEADO
