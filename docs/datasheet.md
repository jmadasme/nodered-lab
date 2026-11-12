# Node-RED Lab — Hoja de datos y manual de operación

**Repositorio:** `jmadasme/nodered-lab`  
**Versión documentada:** `NodeRed` (contenido original)  
**Licencia:** MIT © 2026 jmadasme  
**Ámbito:** laboratorio docente y colección de flujos Node-RED. No es una aplicación desplegable llave en mano.

---

## 1. Qué es

`nodered-lab` reúne **82 archivos JSON** de flujos Node-RED, un PDF de consulta sobre Docker y tres secuencias didácticas:

- `secuencia Ini` — fundamentos: dashboard, hora, contador, condicionales, HTTP, MQTT, correo y un mini proyecto IoT.
- `secuencia Ed` — catálogo de nodos: inject, function, switch, HTTP, JSON, CSV, XML, dashboard, WebSocket, PostgreSQL, GPIO y cámara USB.
- `secuencia eval` — ejercicios y dashboards de evaluación: textos, tablas, alarmas, motor, sensores, iluminación, clima, RFID, inventario y videovigilancia.

La copia publicada conserva el contenido original y excluye credenciales, bases de datos y artefactos de ejecución.

## 2. Qué no es

- No contiene `package.json`, `node_modules`, `flows.json`, `flows_cred.json` ni `settings.js`.
- No es una instancia Node-RED ya configurada.
- No incluye credenciales reales ni un servidor listo para producción.
- El archivo `Pro.rar` está cifrado y su contenido no se documenta.

## 3. Cifras de la colección

| Dato | Valor |
|---|---:|
| Archivos JSON | 82 |
| JSON estrictamente válidos | 71 |
| JSONC que requieren limpieza | 10 |
| Archivo con error de formato | 1 (`83-rpi-gpio-pwm-output.json`) |
| Nodos en los 71 válidos | 322 |
| Tipos de nodo | 62 |
| Pestañas de dashboard | 18 |
| Nodos de dashboard | 98 |
| Archivos con saltos CRLF | 81 de 82 |
| PDF de consulta | 1 |

## 4. Requisitos

### Software

- Node.js 18 o 20 LTS.
- Node-RED 3.x (4.x debería funcionar).
- `node-red-dashboard` 3.6.6 para los widgets.
- `node-red-node-pi-gpio` para GPIO de Raspberry Pi.
- `node-red-node-email` para alertas por correo.
- `node-red-contrib-postgresql` para PostgreSQL.
- Opcionales: Mosquitto 2.x, PostgreSQL 13+ y Docker 20+.
- Navegador reciente para el editor y el dashboard.

### Hardware

- PC con Linux, macOS o Windows y 2 GB de RAM recomendados.
- Para GPIO: Raspberry Pi 3/4/5 con header de 40 pines, dos LEDs, resistencias de 220–330 Ω, un pulsador, resistencia de 10 kΩ opcional, protoboard y cables.
- Para cámara: cámara USB compatible con UVC servida por MJPEG-streamer.

### Puertos

| Puerto | Uso |
|---|---|
| 1880/TCP | Editor y dashboard |
| 1883/TCP | MQTT sin TLS |
| 8883/TCP | MQTT con TLS |
| 5432/TCP | PostgreSQL |
| 465/587/TCP | SMTP |
| 8080/TCP | Cámara MJPEG |
| 18800/TCP | Flujo TCP de ejemplo |
| 4000/UDP | Flujo UDP de ejemplo |

## 5. Instalación

### Opción A — Node-RED nativo

```bash
node --version
npm install -g --unsafe-perm node-red
node-red
```

Abrir `http://127.0.0.1:1880`.

### Opción B — Docker

```bash
docker run -it --rm \
  -p 1880:1880 \
  -p 1883:1883 \
  -v node_red_data:/data \
  --name nodered \
  nodered/node-red:latest
```

### Paquetes de la paleta

Desde el editor: **Menú → Manage palette → Install**, o por terminal dentro de `~/.node-red`:

```bash
npm install node-red-dashboard@3.6.6
npm install node-red-node-pi-gpio
npm install node-red-node-email
npm install node-red-contrib-postgresql
```

## 6. Cómo importar un flujo

1. Abrir el editor en `http://127.0.0.1:1880`.
2. **Menú → Import**.
3. Seleccionar **Clipboard** o **Import from file**.
4. Pegar/abrir un `.json` de `versions/NodeRed/secuencia Ini`, `secuencia Ed` o `secuencia eval`.
5. Revisar los nodos de configuración: broker MQTT, SMTP, PostgreSQL y pines GPIO.
6. Pulsar **Import** y luego **Deploy**.

> Importar de a un flujo por vez. La colección es una progresión didáctica, no un proyecto único.

### JSON estrictos y JSONC

- Los **71 JSON estrictos** se importan directamente.
- Los **10 JSONC** contienen comentarios `//` o varios arrays consecutivos; hay que limpiarlos antes de importar.
- `83-rpi-gpio-pwm-output.json` tiene un error de formato y no debe importarse sin corregirlo.

## 7. Mapa de flujos

### `secuencia Ini` — fundamentos (13 archivos)

| Archivo(s) | Qué enseña |
|---|---|
| `02`–`05` | Primer dashboard, hora, contador con `context` y condicional |
| `06`–`08` | Open-Meteo, gráfico y MQTT local publicar/recibir |
| `09`–`10` | Alerta por correo y mini proyecto IoT con gauge, historial y umbral |
| `a.json`, `b.json`, `c.json` | Dashboards de temperatura, barras y torta |

Ruta sugerida: `02 → 03 → 04 → 05 → 06 → 07 → 08 → 09 → 10 → a/b/c`.

### `secuencia Ed` — catálogo de nodos (51 archivos)

| Bloque | Contenido |
|---|---|
| `01–10` | inject, debug, function, change, switch, template, delay, trigger, exec |
| `11–23` | RBE, HTTP, join/split, link, status, catch, JSON, CSV, XML, file |
| `26–28` | range, sort, batch |
| `29–37` | Widgets de dashboard |
| `41–47` | WebSocket, UDP, variables, cron, POST |
| `50–52` | Flujo completo y TCP |
| `61–62` | MQTT |
| `71–73` | PostgreSQL |
| `81–83` | GPIO |
| `lab` | Cámara USB |

### `secuencia eval` — ejercicios y dashboards (18 archivos)

Cubren textos, dropdowns, toasts, formularios, tablas, imágenes, alarmas, motor, sensores, iluminación, clima, RFID, inventario y videovigilancia. Ocho son JSON estricto; diez son borradores JSONC.

## 8. MQTT

Los flujos usan un broker local por defecto:

```
mqtt://127.0.0.1:1883
```

Pasos:

1. Instalar Mosquitto o usar un broker existente.
2. En Node-RED, editar el nodo **MQTT broker**.
3. Cambiar host, puerto y credenciales.
4. Publicar en un tópico de prueba, por ejemplo `lab/sensor/temperatura`.
5. Verificar con `mosquitto_sub -h 127.0.0.1 -t 'lab/#' -v`.

Los flujos de ejemplo no incluyen credenciales reales; los valores son marcadores que deben reemplazarse.

## 9. Dashboard

- Requiere `node-red-dashboard` 3.6.6.
- La UI se sirve en `http://127.0.0.1:1880/ui`.
- Los ejemplos usan gauge, chart, text, slider, switch, template, table y toast.
- El layout se define por grupos y pestañas; no hay un dashboard único.

## 10. GPIO de Raspberry Pi

| Función | Pin físico | GPIO |
|---|---|---|
| LED 1 | 11 | GPIO17 |
| LED 2 | 12 | GPIO18 |
| Pulsador | 13 | GPIO27 |
| PWM | 12 | GPIO18 |

- Usar resistencias de 220–330 Ω en serie con los LEDs.
- El pulsador va entre GPIO27 y GND; la resistencia de 10 kΩ es opcional si se usa `pull-up` interno.
- No conectar cargas de potencia directas al GPIO. Usar relé, MOSFET u optoacoplador.
- El flujo `83-rpi-gpio-pwm-output.json` requiere corrección antes de usarse.

## 11. PostgreSQL y correo

### PostgreSQL

- Instalar `node-red-contrib-postgresql`.
- Configurar host, puerto, base, usuario y contraseña en el nodo de configuración.
- Los flujos de ejemplo usan consultas `SELECT` y tablas de laboratorio; no incluyen esquema ni datos.

### Correo

- Instalar `node-red-node-email`.
- Configurar SMTP con usuario y contraseña de aplicación.
- No usar la contraseña principal del correo.
- Probar primero con un destinatario propio.

## 12. HTTP, APIs y cámara

- `http in` crea endpoints; `http response` devuelve la respuesta.
- Los ejemplos consultan Open-Meteo y APIs de clima sin clave.
- La cámara USB se sirve por MJPEG (por ejemplo, `http://127.0.0.1:8080/video`).
- El flujo toma un snapshot JPEG y lo convierte a `data:` URI para el dashboard.
- No exponer el editor Node-RED a Internet sin autenticación y TLS.

## 13. Operación y diagnóstico

| Síntoma | Causa probable | Solución |
|---|---|---|
| El flujo no importa | JSONC o error de formato | Validar con `python3 -m json.tool` y limpiar comentarios |
| El dashboard no aparece | Falta `node-red-dashboard` | Instalar el paquete y reiniciar |
| MQTT no conecta | Broker, puerto o credenciales | Revisar nodo de configuración y logs |
| GPIO no responde | Pin ocupado, permiso o placa | Verificar `gpio readall` y usuario en grupo `gpio` |
| PostgreSQL falla | Servicio, base o credenciales | Probar con `psql` antes de Node-RED |
| Correo no sale | SMTP o contraseña de aplicación | Revisar TLS, puerto y credenciales |
| Cámara no carga | Stream MJPEG no disponible | Abrir la URL de la cámara en el navegador |

## 14. Límites conocidos

- No hay `package.json` ni una instancia reproducible.
- 10 archivos son JSONC y requieren limpieza manual.
- Un archivo tiene error de formato.
- Los flujos de GPIO se probaron en una Raspberry Pi concreta; los pines pueden variar.
- No hay pruebas automatizadas.
- No hay autenticación ni TLS en los ejemplos.
- `Pro.rar` está cifrado y no se documenta.
- El PDF de Docker es una guía breve, no un manual completo.

## 15. Seguridad

- No publicar `flows_cred.json`, `settings.js` con credenciales ni claves.
- Cambiar todas las credenciales de ejemplo antes de desplegar.
- No exponer el editor en `0.0.0.0` sin autenticación.
- Usar TLS para MQTT y SMTP cuando salga de la red local.
- Revisar los nodos `exec` antes de importar flujos de terceros.
- Mantener Node-RED y los paquetes actualizados.

## 16. Mantenimiento

- Versionar los flujos exportados, no el estado de `~/.node-red`.
- Validar cada JSON con `python3 -m json.tool`.
- Documentar los cambios de pines y credenciales.
- Probar cada flujo en una instancia limpia antes de publicarlo.
- Conservar el PDF y `VERSION.md` como referencia histórica.

## 17. Licencia

MIT © 2026 jmadasme. Ver [`LICENSE`](../LICENSE).
