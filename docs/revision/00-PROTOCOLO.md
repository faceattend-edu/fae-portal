# Protocolo de revisión web — FaceAttend EDU

Revisión **solo de la web**, de **todas las pantallas de los 4 roles**, con el foco en dos cosas:
que **cada botón funcione** y que **la respuesta del backend se muestre bien en pantalla**.

Está diseñada para que la ejecute **Haiku**: tareas pequeñas e independientes, veredictos binarios, reglas
explícitas y nada que dependa de criterio de diseño. Un modelo más capaz (o una persona) prepara los datos,
construye las herramientas de la sección 9 y **revisa todo lo que Haiku marque como FAIL o DUDA**.

| Archivo | Contenido |
|---|---|
| `00-PROTOCOLO.md` | Este documento: reglas, entorno, procedimiento, reporte |
| `01-super-admin.md` | Lista de verificación — Super Administrador (**prioridad 1**) |
| `02-admin.md` | Lista de verificación — Administrador de sede (**prioridad 2**) |
| `03-docente.md` | Lista de verificación — Docente / Instructor (**prioridad 3**) |
| `04-estudiante.md` | Lista de verificación — Estudiante / Aprendiz (**prioridad 4**) |
| `05-negativos.md` | Casos negativos: matriz de API (automatizable) y casos de interfaz |
| `99-REPORTE-plantilla.md` | Formato obligatorio del resultado |

---

## 1. Orden y tamaño de las unidades

Prioridad: **Super Admin → Admin → Docente → Estudiante**. Dentro de cada rol, en el orden del archivo.

- **Una unidad = un bloque de un rol** (por ejemplo `AD-USR`, "Admin · Usuarios"). Cada bloque tiene **como máximo 15 verificaciones**.
- **Una ejecución de Haiku = una unidad.** Contexto nuevo en cada unidad: no arrastrar lo anterior.
- Al terminar una unidad, escribir su resultado en `resultados/` (sección 7) antes de empezar otra.
- Las pantallas públicas (landing, login, registro, recuperación) se revisan **una sola vez**, en `SA-PUB` y `SA-AUT`.

Etiquetas de cada verificación:

| Etiqueta | Significa | Quién lo hace |
|---|---|---|
| `[API]` | Se comprueba con peticiones HTTP (curl / script), sin navegador | Haiku o script |
| `[AUTO]` | Navegador: abrir, hacer clic y leer pantalla, red y consola | Haiku |
| `[MANUAL]` | Requiere cámara, lector de huella, archivo descargado o juicio visual | Persona (Haiku solo lo marca `BLOQUEADO`) |

---

## 2. Reglas del ejecutor (obligatorias)

### Hacer
1. **Seguir el texto de la verificación al pie de la letra.** No inventar pasos ni "mejorar" la prueba.
2. **Un veredicto por verificación**, de entre: `PASS`, `FAIL`, `DUDA`, `BLOQUEADO` (sección 6).
3. **Toda verificación lleva evidencia**: línea de red (método, URL, status), texto exacto de pantalla y errores de consola.
4. **Comprobar con dos fuentes** cuando la verificación toque datos: lo que muestra la pantalla **y** la respuesta de red.
5. **Deshacer lo que se cambie**: si activó algo, vuelva a desactivarlo; todo registro creado se llama `QA-<ID>` y se desactiva al final.
6. **Si algo no cuadra con lo esperado y no está en la columna "Falla si", marcar `DUDA`**, nunca `PASS` ni `FAIL` por intuición.

### No hacer (límites duros)
- **Nunca borrar filas** ni ejecutar `DELETE`/`UPDATE`/`INSERT` por SQL. `psql` solo con `SELECT`.
- **Nunca cambiar contraseñas**, correos ni roles de cuentas que no empiecen por `QA-`.
- **No pulsar "Notificar todos"** ni ninguna acción que envíe correos a personas reales (marcado `[MANUAL]`).
- **No operar sobre datos reales de otra sede** salvo en los casos negativos que lo piden expresamente.
- **No repetir una acción fallida más de 2 veces.** Dos intentos y se marca `DUDA` con la evidencia.
- **No saltarse una verificación** por parecer repetida; si lo es, marcarla `PASS` solo si de verdad se ejecutó.
- **No modificar código ni configuración.** Esta revisión solo observa y reporta.

### Cuándo detenerse y avisar al supervisor
- La app muestra **pantalla en blanco** o el login deja de funcionar → detener la unidad, reportar `FAIL` severidad **Bloqueante**.
- Una respuesta **5xx** o un **stack trace** visible → `FAIL` y continuar.
- Aparecen **datos de otra sede / de otra persona** que el rol no debería ver → `FAIL` **Bloqueante** y detener.
- Más de **5 `DUDA` seguidas** en una unidad → detener: probablemente falta un dato de prueba o el entorno cambió.

---

## 3. Entorno

| Dato | Valor |
|---|---|
| Web (contenedor, **usar este**) | `http://localhost:8090` |
| Web (desarrollo, solo si se pide) | `http://localhost:19006` |
| API (gateway) | `http://localhost:8000/api/v1` |
| Base de datos (solo lectura) | `docker exec fae-postgres psql -U postgres -d faceattend_db -c "SELECT ..."` |

**Antes de empezar (lo hace el supervisor, no Haiku):**
1. Reconstruir la imagen del front para que el contenedor tenga el código actual
   (`docker compose build frontend-web && docker compose up -d --no-deps frontend-web`). Un contenedor viejo ya causó
   pantallas en blanco que en desarrollo no existían.
2. Verificar que los servicios estén `healthy` (`docker ps`).
3. Cargar los datos de prueba de la sección 4.

### Cuentas
Los **usuarios** están aquí; las **contraseñas NO** (no se versionan). Van en `docs/revision/.cuentas.local`
(ignorado por git), una por línea: `usuario=contraseña`.

| Rol | Usuario | Correo de login | Sede |
|---|---|---|---|
| Super Administrador | `admin.faceattend` | `admin@faceattend.local` | — (global) |
| Super Administrador (2.ª) | `carolina.mendoza` | `carolina.mendoza@example.com` | — (global) |
| Administrador sede A | `fermin` | `fermin@sena.example` | SENA |
| Administrador sede B | `admin.ict` | `marcela.quintero@ict.edu.co` | ICT |
| Administrador sede C | `admin.smp` | `hernan.ocampo@smp.edu.co` | SMP |
| Docente | `maria.sanchez` | `maria.sanchez@ict.edu.co` | ICT |
| Estudiante | `est.ict` | `valentina.garcia1@estudiantes.ict.edu.co` | ICT |
| Estudiante (otra sede) | `est.smp` | `valentina.rendon801@estudiantes.smp.edu.co` | SMP |

> El formulario de login pide **correo** y contraseña; la API de login pide `username`.

---

## 4. Datos de prueba que deben existir (los prepara el supervisor)

- Una **clase hoy** para `maria.sanchez`: bloque horario del día actual, ficha de ICT con ≥ 20 matriculados.
- Estudiantes **con y sin** biometría; uno **sin horario** (columna *Programa* vacía).
- Justificaciones en cada estado (Pendiente, Aprobada, Rechazada); una con adjunto y otra sin.
- Alertas pendientes y resueltas.
- Una sesión de clase abierta, una cerrada y una cancelada.
- Un usuario, un curso y una sede **inactivos**.
- Un listado con **> 100 filas** (la API corta páginas en 100).
- Un nombre con tilde, ñ, apóstrofo y 80 caracteres.
- Por cada sede, al menos 1 persona que **no exista en las otras** (para los negativos de aislamiento).

---

## 5. Procedimiento estándar para una verificación `[AUTO]`

Aplíquese en este orden. Las herramientas se nombran por su función: use las equivalentes del navegador disponible
(panel Claude: `navigate`, `find`, `computer`, `read_network_requests`, `read_console_messages`, `get_page_text`;
Playwright: `browser_navigate`, `browser_click`, `browser_network_requests`, `browser_console_messages`, `browser_snapshot`).

1. **Abrir** la ruta con la URL completa (`BASE_URL` + ruta).
2. **Esperar a que cargue**: la app hace cientos de peticiones al arrancar. Esperar hasta que aparezca el **título de la pantalla**
   (máximo **40 s**). Si no aparece: `FAIL` "no carga" y adjuntar consola.
3. **Leer la consola**: cualquier línea `Uncaught`, `TypeError`, `ReferenceError` o `is not a function` ⇒ `FAIL`.
4. **Ejecutar el paso** de la verificación (clic, escribir, elegir).
   - Localizar elementos **por su texto** (`find "Guardar cambios"`), **nunca por coordenadas** de una captura.
   - Los **desplegables personalizados** no responden bien al clic sobre sus opciones: hacer clic en el campo y elegir con
     **teclado** (↓ y Enter) o por referencia. Si tras 2 intentos no selecciona: `DUDA` "control no automatizable".
   - Los modales tienen animación: esperar 1–2 s antes de leer.
5. **Leer la red**: buscar la petición que dispara el botón. Anotar `MÉTODO URL → status`. Si el esperado es 200/201/204 y llega
   ≥ 400 ⇒ `FAIL`. Un `PATCH`/`PUT` que devuelve 204 **no** trae cuerpo: es normal.
6. **Leer la pantalla** (`get_page_text` o `find`): el texto/valor esperado debe estar y **no** deben aparecer
   `undefined`, `null`, `NaN`, `[object Object]` ni claves de traducción crudas.
7. **Recargar** (cuando la verificación diga "persiste") y repetir 6: el dato debe seguir igual.
8. **Deshacer** lo cambiado.

### Cómo ver el cuerpo de una respuesta
- Con el panel: `read_network_requests` con el `requestId` de la petición.
- Con `curl`: iniciar sesión por API y repetir la misma llamada:
  ```bash
  S=$(curl -s -X POST http://localhost:8000/api/v1/auth/login -H 'Content-Type: application/json' \
      -d '{"username":"<usuario>","password":"<clave>"}' | sed -E 's/.*"sessionId":"([^"]+)".*/\1/')
  curl -s -H "Authorization: Bearer $S" http://localhost:8000/api/v1/<ruta>
  ```
- Un **204** sin cuerpo se verifica por su efecto (releer el recurso con `GET`).

---

## 6. Veredictos

| Veredicto | Cuándo |
|---|---|
| `PASS` | Se ejecutó el paso y **todo** lo de "Esperado" se cumplió |
| `FAIL` | Se cumple algo de la columna **"Falla si"**, o hay `Uncaught`/5xx/pantalla en blanco |
| `DUDA` | El resultado difiere de lo esperado pero **no** está en "Falla si"; o el control no se pudo manejar tras 2 intentos |
| `BLOQUEADO` | Falta un dato de prueba, una herramienta (cámara, lector) o un permiso para ejecutarlo. Indicar **qué** falta |

Reglas fijas de severidad (para `FAIL`):

| Severidad | Criterio |
|---|---|
| **Bloqueante** | Pantalla en blanco · no se puede iniciar sesión · fuga de datos entre sedes/personas · acción prohibida que sí se ejecuta |
| **Alta** | El botón no hace lo que dice · error 5xx · dato guardado que no persiste |
| **Media** | Dato mal mostrado (`undefined`, `NaN`, formato) · mensaje de error poco claro · estado vacío roto |
| **Baja** | Texto, traducción, alineación, contraste |

---

## 7. Resultado de cada unidad

Cada unidad produce **un archivo** `docs/revision/resultados/<UNIDAD>.md` (por ejemplo `AD-USR.md`) con la plantilla de
`99-REPORTE-plantilla.md`. Reglas:
- Una fila por **cada** verificación del bloque, sin omitir ninguna.
- Evidencia **copiada literal** (no resumida): línea de red, texto de pantalla, mensaje de consola.
- Al final, un bloque **"Hallazgos"** con un ticket por cada `FAIL`.
- El archivo termina con el **resumen**: `N PASS · N FAIL · N DUDA · N BLOQUEADO`.

---

## 8. Instrucción de arranque para Haiku (copiar y pegar, cambiando la unidad)

```
Eres un revisor de calidad. Ejecuta SOLO la unidad {UNIDAD} (por ejemplo AD-USR).

1. Lee docs/revision/00-PROTOCOLO.md completo.
2. Lee en {ARCHIVO_DEL_ROL} únicamente el bloque {UNIDAD}.
3. Inicia sesión con la cuenta del rol indicada en el protocolo (contraseña en docs/revision/.cuentas.local).
4. Ejecuta las verificaciones en orden, una por una, siguiendo la sección 5 del protocolo.
5. Da un veredicto por verificación (PASS/FAIL/DUDA/BLOQUEADO) con su evidencia literal.
6. Escribe el resultado en docs/revision/resultados/{UNIDAD}.md con la plantilla 99-REPORTE-plantilla.md.

No modifiques código. No borres datos. No cambies contraseñas. No repitas una acción fallida más de 2 veces.
Si dudas, marca DUDA. Al terminar responde solo con el resumen: N PASS · N FAIL · N DUDA · N BLOQUEADO.
```

---

## 9. Automatización (ya construida)

Cuanto más se automatice, menos juicio necesita el ejecutor. Tres capas, de más barata a más cara:

| Capa | Qué cubre | Herramienta | Estado |
|---|---|---|---|
| **L1 · API** | Permisos por rol, aislamiento entre sedes, validaciones y reglas (`05-negativos.md` §A) | `tools/qa/api-matrix.mjs` (Node, sin navegador) | ✔ construida |
| **L2 · Humo UI** | Cada ruta × cada rol: carga, sin pantalla en blanco ni `Uncaught`, sin `undefined/NaN`, sin 5xx, rutas prohibidas bloqueadas | `tools/qa/ui-smoke.mjs` (`playwright-core` + Chrome/Edge instalado) | ✔ construida |
| **L3 · Acciones UI** | Cada botón de los checklists (`[AUTO]`) | Haiku con el protocolo de la sección 5 | Este documento |

### Comando único

```bash
npm run qa                  # L1 + L2 y resumen en docs/revision/resultados/RESUMEN.md
npm run qa:rebuild          # igual, pero primero reconstruye el front en Docker
npm run qa -- --only=api    # solo L1   (--only=ui → solo L2)
npm run qa -- --roles=SUP,ADM --mobile --skip-slow
```

Código de salida: `0` sin FAIL · `1` hay FAIL · `2` falló el preflight (Docker, web o cuentas).
Las cuentas salen de `docs/revision/.cuentas.local` (no versionado). Detalle de uso: `tools/qa/README.md`.

### Reparto de trabajo
1. El **supervisor** (o el CI) corre `npm run qa` antes de lanzar a Haiku y revisa `RESUMEN.md`.
2. Haiku **no repite** lo que L1 y L2 ya cubren: para cada ID del checklist que L1/L2 marcó PASS, puede citar ese resultado como evidencia.
3. Haiku ejecuta lo que solo L3 cubre: botones y flujos (`[AUTO]`) y deja `[MANUAL]` como `BLOQUEADO`.
4. Tras cada corrección: `npm run qa:rebuild` y volver a la unidad afectada.

### Lo que L1 y L2 **no** cubren (queda para L3 o para una persona)
- Todo lo marcado `[MANUAL]`: cámara, lector de huella, archivos descargados, correos reales.
- Los casos de `05-negativos.md` §A5 que necesitan datos específicos (sesión cerrada, justificación con adjunto…): L1 los lista como `BLOQUEADO — no cubierto`.
- Los casos de interfaz de `05-negativos.md` §B (formularios, doble clic, sin red…).

## 10. Supervisión y criterio de salida

- El supervisor revisa **el 100 % de los `FAIL` y `DUDA`** y una muestra del 10 % de los `PASS` (re-ejecutar y comparar).
- Cada `FAIL` se confirma una vez antes de abrir ticket (descarta falsos positivos por carga lenta o datos).
- **Salida**: 0 hallazgos Bloqueantes · 0 Altos abiertos · Medios y Bajos con responsable · todas las unidades con resultado archivado.
- Tras cada corrección: reconstruir el front, volver a ejecutar **la unidad afectada** y **L1 + L2**.
