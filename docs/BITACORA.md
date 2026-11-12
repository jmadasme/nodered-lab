# Node-RED Lab — Bitácora

Registro de hallazgos, decisiones y deuda técnica al documentar la colección de flujos.

---

## 1. Qué se encontró

- **82 archivos JSON** en tres secuencias: `Ini`, `Ed` y `eval`.
- **71 JSON estrictamente válidos**.
- **10 JSONC**: comentarios `//` y, en varios casos, varios arrays consecutivos.
- **1 archivo con error de formato**: `83-rpi-gpio-pwm-output.json` empieza con `n[` (byte espurio antes del array).
- **322 nodos** y **62 tipos** en los 71 válidos.
- **18 pestañas de dashboard** y **98 nodos `ui_*`**.
- **81 de 82 archivos** usan saltos CRLF.
- **Un duplicado exacto**: `03_mensaje_con_hora - copia.json` tiene el mismo MD5 que el original.
- **Una etiqueta engañosa**: `1-30.json` contiene en realidad los flujos 21–30.

## 2. Decisiones

### Documentar sin tocar los originales

No se renombraron, reformatearon ni corrigieron los JSON. La colección se conserva tal como se usó en clase.

**Consecuencia:** el visitante debe limpiar los JSONC antes de importarlos. Se documenta el procedimiento.

### Importación individual

Se recomienda importar un flujo por vez, no la colección completa. Los flujos comparten IDs de dashboard, brokers y configuraciones; una importación masiva puede generar conflictos.

### Limpiador JSONC documentado, no ejecutado

`docs/developer.md` incluye un script de referencia para limpiar comentarios y unir arrays. No se ejecutó sobre los originales para no alterarlos.

### Versión de referencia del dashboard

Se fijó `node-red-dashboard@3.6.6` como versión probada. Node-RED 4.x debería funcionar, pero no se validó.

## 3. Problemas y soluciones

| Problema | Impacto | Tratamiento |
|---|---|---|
| 10 JSONC | No importan directamente | Documentar limpieza manual |
| `83-...json` con `n[` | Error de parseo | Marcar como inválido; no importar |
| IDs de dashboard compartidos | Conflictos al importar todo | Importar de a un flujo |
| `1-30.json` contiene 21–30 | Nombre engañoso | Documentar el contenido real |
| Duplicado de `03` | Ruido | Conservar y señalar el MD5 |
| CRLF en 81 archivos | Diferencias de diff | No normalizar; conservar |
| Sin `package.json` | No hay instancia reproducible | Documentar instalación manual |

## 4. Enfoques descartados

- **Importación masiva con `jq`:** no resuelve los IDs compartidos ni los nodos desconocidos.
- **Parseo tolerante de JSONC:** oculta errores; se prefiere limpieza explícita.
- **Convertir todos los flujos a JSON estricto:** alteraría los originales.
- **Añadir `package.json`:** convertiría el material en una aplicación, que no es el objetivo.
- **Extraer `Pro.rar`:** está cifrado y no se dispone de la contraseña.

## 5. Lecciones

- Un archivo con extensión `.json` no garantiza JSON válido.
- Los IDs de dashboard son globales; importar varios flujos puede pisar pestañas.
- Documentar los defectos es más útil que “arreglarlos” sin contexto.
- El material didáctico vale por su progresión, no por ser un producto terminado.
- La paleta sobria también aplica a los diagramas: azul petróleo, sin verde neón.

## 6. Deuda técnica

- Crear un validador que reporte JSONC, errores y nodos desconocidos.
- Limpiar los 10 JSONC en una rama separada.
- Corregir el byte espurio de `83-rpi-gpio-pwm-output.json`.
- Añadir un `package.json` opcional para una instancia reproducible.
- Documentar el contenido de `Pro.rar` si se autoriza.
- Unificar nombres con tipeos (`vesion`, `versin`).

## 7. Licencia

MIT © 2026 jmadasme. Ver [`LICENSE`](../LICENSE).
