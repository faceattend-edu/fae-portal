# Resumen de la revisión automatizada

- Fecha: 2026-10-19T16:08:54.922Z
- Web: http://localhost:8090 · API: http://localhost:8000/api/v1
- Total: 177 PASS · 0 FAIL · 20 DUDA · 25 BLOQUEADO

## L1 · API
114 PASS · 0 FAIL · 4 DUDA · 25 BLOQUEADO

### FAIL
Ninguno.

### DUDA (revisa el supervisor)
- **NEG-B04** EST — PUT /persons/e91fa255-b786-45c7-b9fb-58a6eae008d9 → 200 · el estudiante pudo editar SU PROPIA persona: confirmar si la autoedición es intencional
- **NEG-C13** ADM — GET /class-sessions/1/attendance → 200 · 200 con lista vacía: el servicio no valida el alcance, pero no hay datos que filtrar en esa sesión
- **NEG-C17-13** ADM2 — GET /class-sessions/1213/attendance → 200 · 200 con lista vacía: el servicio no valida el alcance, pero no hay datos que filtrar en esa sesión
- **NEG-E15** ADM — PATCH /users/ffef1d25-6161-49f9-988f-ce1780ff255d/status → 403 · status real 403; esperado 400

## L2 · UI
63 PASS · 0 FAIL · 16 DUDA · 0 BLOQUEADO

### FAIL
Ninguno.

### DUDA (revisa el supervisor)
- **L2-SUP-/app/kiosk** Super Admin · /app/kiosk — 0 peticiones · 2416 ms · con contenido · carga con contenido (sin expectativa definida)
- **L2-SUP-/app/biometrics** Super Admin · /app/biometrics — 1 peticiones · 2416 ms · con contenido · carga con contenido (sin expectativa definida)
- **L2-SUP-/app/contacts** Super Admin · /app/contacts — 0 peticiones · 2664 ms · con contenido · carga con contenido (sin expectativa definida)
- **L2-SUP-/app/school-info** Super Admin · /app/school-info — 1 peticiones · 2662 ms · con contenido · carga con contenido (sin expectativa definida)
- **L2-SUP-/app/ruta-que-no-existe** Super Admin · /app/ruta-que-no-existe — 0 peticiones · 2651 ms · con contenido · carga con contenido (sin expectativa definida)
- **L2-ADM-/app/biometrics** Admin · /app/biometrics — 1 peticiones · 2657 ms · con contenido · carga con contenido (sin expectativa definida)
- **L2-ADM-/app/contacts** Admin · /app/contacts — 1 peticiones · 2919 ms · con contenido · carga con contenido (sin expectativa definida)
- **L2-ADM-/app/school-info** Admin · /app/school-info — 1 peticiones · 2413 ms · con contenido · carga con contenido (sin expectativa definida)
- **L2-DOC-/app/users/teachers** Docente · /app/users/teachers — 0 peticiones · 2420 ms · con contenido · carga con contenido (sin expectativa definida)
- **L2-DOC-/app/users/admins** Docente · /app/users/admins — 0 peticiones · 2665 ms · con contenido · carga con contenido (sin expectativa definida)
- **L2-DOC-/app/biometrics** Docente · /app/biometrics — 1 peticiones · 2648 ms · con contenido · carga con contenido (sin expectativa definida)
- **L2-DOC-/app/contacts** Docente · /app/contacts — 1 peticiones · 2431 ms · con contenido · carga con contenido (sin expectativa definida)
- **L2-DOC-/app/school-info** Docente · /app/school-info — 1 peticiones · 2417 ms · con contenido · carga con contenido (sin expectativa definida)
- **L2-EST-/app/settings/general** Estudiante · /app/settings/general — 0 peticiones · 2660 ms · con contenido · carga con contenido (sin expectativa definida)
- **L2-EST-/app/contacts** Estudiante · /app/contacts — 1 peticiones · 2419 ms · con contenido · carga con contenido (sin expectativa definida)
- **L2-EST-/app/school-info** Estudiante · /app/school-info — 1 peticiones · 2654 ms · con contenido · carga con contenido (sin expectativa definida)

Detalle completo: `L1-api.md`, `L2-ui.md`. Capturas de rutas no PASS: `ui/`.
