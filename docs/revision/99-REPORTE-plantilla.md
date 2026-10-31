# Plantilla de reporte (copiar a `resultados/<UNIDAD>.md` y rellenar)

Una fila por **cada** verificación del bloque, en el mismo orden. Evidencia **literal**, no resumida.
Veredictos: `PASS` · `FAIL` · `DUDA` · `BLOQUEADO` (ver `00-PROTOCOLO.md` §6).

```markdown
# Resultado — <UNIDAD> (<Rol> · <Pantalla>)

- Fecha y hora:
- Entorno: http://localhost:8090  (imagen reconstruida el: <fecha>)
- Cuenta usada:
- Ejecutor: Haiku
- Datos creados (todos `QA-…`) y si se dejaron **desactivados**: sí / no (lista)

## Verificaciones

| ID | Veredicto | Petición (método URL → status) | Texto en pantalla (literal) | Consola | Nota |
|---|---|---|---|---|---|
| AD-USR-01 | PASS | GET /api/v1/users → 200 | "Gestiona los usuarios… (23 estudiantes encontrados)" | limpia | |
| AD-USR-04 | FAIL | PUT /api/v1/users/… → (no se envió) | "El usuario es requerido" | limpia | ver H-01 |

## Hallazgos

### H-01 · <título corto>
- Verificación: AD-USR-04
- Rol / pantalla / botón: Admin · Usuarios · Guardar cambios
- Esperado:
- Obtenido:
- Petición y cuerpo relevante:
- Evidencia (captura / consola):
- Severidad: Bloqueante | Alta | Media | Baja
- Reproducción (3–5 pasos):

## Limpieza
- [ ] Todo lo cambiado quedó como estaba (activaciones revertidas, valores originales restaurados).
- [ ] Registros `QA-…` desactivados.

## Resumen
N PASS · N FAIL · N DUDA · N BLOQUEADO
```

## Reglas de calidad del reporte (el supervisor las usa para rechazar un reporte)

1. Faltan filas respecto del bloque → **se rechaza** (no se omite nada).
2. Un `PASS` sin petición ni texto de pantalla cuando la verificación los pedía → **se rechaza**.
3. Un `FAIL` sin reproducción de 3–5 pasos → **se devuelve**.
4. Una afirmación sin evidencia literal ("parece que funciona") → **se devuelve**.
5. Faltan los dos bloques *Limpieza* o *Resumen* → **se devuelve**.
