# Node-RED Lab — Guía del desarrollador

**Repositorio:** `jmadasme/nodered-lab`  
**Versión:** `NodeRed`  
**Licencia:** MIT © 2026 jmadasme

---

## 1. Arquitectura del repositorio

```text
nodered-lab/
├── README.md
├── LICENSE
├── docs/
│   ├── datasheet.md
│   ├── developer.md
│   ├── REFERENCES.md
│   └── assets/
│       ├── architecture.svg
│       ├── arquitectura-nodered.svg
│       ├── mapa-flujos.svg
│       └── dashboard-mockup.svg
└── versions/
    └── NodeRed/
        ├── VERSION.md
        ├── Guía de Consulta Docker.pdf
        ├── Pro.rar
        ├── secuencia Ini/
        ├── secuencia Ed/
        └── secuencia eval/
```

No hay código de aplicación: el “código” son los flujos JSON y las convenciones de importación. La documentación separa **qué hace** el laboratorio (`datasheet.md`) de **cómo está construido** (`developer.md`).

## 2. Anatomía de un flujo Node-RED

Un archivo de flujo es un array de objetos. Cada objeto es un nodo o una configuración:

```json
[
  {
    "id": "abc123",
    "type": "inject",
    "z": "tab1",
    "name": "Cada 5 s",
    "props": [{ "p": "payload" }],
    "repeat": "5",
    "wires": [["n2"]]
  },
  {
    "id": "n2",
    "type": "debug",
    "z": "tab1",
    "name": "Salida",
    "active": true,
    "wires": []
  },
  {
    "id": "tab1",
    "type": "tab",
    "label": "Flujo 1"
  }
]
```

Campos relevantes:

| Campo | Significado |
|---|---|
| `id` | Identificador único dentro del flujo |
| `type` | Tipo de nodo (`inject`, `function`, `mqtt in`, etc.) |
| `z` | Pestaña a la que pertenece |
| `wires` | Conexiones de salida; array de arrays |
| `name` | Etiqueta visible |
| `repeat` | Intervalo de repetición en nodos `inject` |
| `payload` | Dato de entrada o salida |
| `active` | Nodo activo/inactivo |

Los nodos de configuración (broker MQTT, SMTP, PostgreSQL) usan `type` propio y son referenciados por `id` desde los nodos que los usan.

## 3. Estructura de las secuencias

### `secuencia Ini` — fundamentos

13 archivos. Progresión pensada para clase:

1. `02` primer dashboard.
2. `03` mensaje con hora.
3. `04` contador con `context`.
4. `05` condicional con `switch`.
5. `06` API de clima con `http request`.
6. `07` gráfico en dashboard.
7. `08` MQTT publicar/recibir.
8. `09` alerta por correo.
9. `10` mini proyecto IoT.
10. `a`, `b`, `c` dashboards de temperatura, barras y torta.

`03_mensaje_con_hora - copia.json` es una copia idéntica del original; se conserva por fidelidad histórica.

### `secuencia Ed` — catálogo de nodos

51 archivos. Cada archivo aísla un nodo o técnica. Los números no son consecutivos: faltan 24, 25, 34, 36, 38–40, 45, 48, 49 y 53–60.

Bloques:

- `01–10`: inject, debug, function, change, switch, template, delay, trigger, exec.
- `11–23`: RBE, HTTP, join/split, link, status, catch, JSON, CSV, XML, file.
- `26–28`: range, sort, batch.
- `29–37`: widgets de dashboard.
- `41–47`: WebSocket, UDP, variables, cron, POST.
- `50–52`: flujo completo y TCP.
- `61–62`: MQTT.
- `71–73`: PostgreSQL.
- `81–83`: GPIO.
- `lab`: cámara USB.

### `secuencia eval` — ejercicios

18 archivos. Ocho son JSON estricto:

- `1-10.json`
- `11-20.json`
- `1-30.json` (contiene 21–30)
- `1.-dashboard basico.json`
- `2.botonsimple.json`
- `3 slider.json`
- `4.grafico simple..json`
- `5 switch interruptor.json`

Diez son borradores JSONC:

- `6 al 20.json`
- `11 al 20.json`
- `11 al 20 version 2.json`
- `11 al 2 versin 2.json`
- `21 al 25.json`
- `21 al 35.json`
- `25 al 35.json`
- `25 al 5w vesion 2.json`
- `31-35.json`
- `36 al 50.json`

## 4. JSON estricto frente a JSONC

Node-RED espera un único array JSON. Los borradores JSONC pueden tener:

- Comentarios `//`.
- Varios arrays consecutivos.
- Comas finales.
- Texto fuera del array.

Validación:

```bash
python3 -m json.tool versions/NodeRed/secuencia\ Ini/02_hola_dashboard.json > /dev/null && echo OK
```

Para limpiar un JSONC:

1. Copiar el archivo a un nombre nuevo.
2. Eliminar comentarios y texto fuera del array.
3. Unir los arrays en uno solo.
4. Validar con `python3 -m json.tool`.
5. Importar en Node-RED.

No modificar el original si se quiere conservar la fidelidad del material.

## 5. Patrones de nodos

| Patrón | Nodos | Uso |
|---|---|---|
| Entrada periódica | `inject` | Simular sensor o temporizador |
| Transformación | `function`, `change`, `range` | Normalizar unidades |
| Clasificación | `switch` | Umbrales y alarmas |
| Persistencia | `context`, `file`, PostgreSQL | Estado y histórico |
| Red | `mqtt in/out`, `http request`, `websocket` | Integración |
| Dashboard | `ui_gauge`, `ui_chart`, `ui_text`, `ui_table` | Visualización |
| Hardware | `rpi-gpio in/out` | Sensores y actuadores |
| Diagnóstico | `debug`, `catch`, `status` | Depuración |

## 6. MQTT: contrato de tópicos

Convención usada en los ejemplos:

```text
lab/<dispositivo>/<variable>
```

Ejemplos:

- `lab/sensor/temperatura`
- `lab/sensor/humedad`
- `lab/actuador/led`
- `lab/alerta/umbral`

Los flujos no imponen un broker; el nodo de configuración debe apuntar a uno accesible. Para pruebas locales:

```bash
mosquitto -v
mosquitto_sub -h 127.0.0.1 -t 'lab/#' -v
```

## 7. Dashboard

- Paquete: `node-red-dashboard@3.6.6`.
- Ruta: `/ui`.
- Los widgets se agrupan por `group` y `tab`.
- Los ejemplos usan gauge, chart, text, slider, switch, template, table y toast.
- El layout no está centralizado: cada flujo define su propia pestaña.

Para agregar un widget:

1. Arrastrar el nodo `ui_*` al canvas.
2. Crear o seleccionar un grupo.
3. Configurar etiqueta, unidad, rango y tamaño.
4. Conectar la salida del nodo de datos.
5. Deploy y abrir `/ui`.

## 8. GPIO

| Pin físico | GPIO | Uso |
|---|---|---|
| 11 | GPIO17 | LED 1 |
| 12 | GPIO18 | LED 2 / PWM |
| 13 | GPIO27 | Pulsador |

Buenas prácticas:

- Resistencia en serie con LEDs.
- `pull-up` o `pull-down` definido, no flotante.
- No alimentar cargas desde el GPIO.
- Verificar el modelo de Raspberry Pi antes de asignar pines.
- El flujo `83-rpi-gpio-pwm-output.json` tiene un error de formato; corregirlo antes de usarlo.

## 9. PostgreSQL

- Paquete: `node-red-contrib-postgresql`.
- Configurar un nodo `postgreSQLConfig` con host, puerto, base, usuario y contraseña.
- Usar consultas parametrizadas cuando se reciban datos externos.
- No versionar cadenas de conexión reales.
- Los ejemplos no incluyen esquema; crear las tablas antes de ejecutar.

## 10. Correo y HTTP

- `node-red-node-email` para SMTP.
- Usar contraseña de aplicación, no la del usuario principal.
- `http in` + `http response` para endpoints.
- Validar método, ruta, cuerpo y códigos de respuesta.
- No exponer endpoints sin autenticación.

## 11. Cómo extender la colección

1. Crear una carpeta nueva dentro de `versions/NodeRed/` o una secuencia con nombre claro.
2. Exportar el flujo desde Node-RED con **Export → Clipboard**.
3. Validar el JSON.
4. Añadir un `VERSION.md` o una entrada en el README.
5. Documentar requisitos, pines y credenciales de ejemplo.
6. Probar en una instancia limpia.
7. Actualizar `docs/datasheet.md` y `docs/developer.md`.

## 12. Pruebas recomendadas

- Importar cada JSON estricto en una instancia limpia.
- Verificar que no haya nodos desconocidos.
- Probar MQTT con `mosquitto_pub`/`mosquitto_sub`.
- Probar GPIO con un LED y un pulsador.
- Verificar PostgreSQL con una tabla temporal.
- Revisar el dashboard en desktop y móvil.
- Confirmar que no se publican credenciales.

## 13. Decisiones de diseño

- **Conservar los originales:** los JSONC y el archivo inválido se mantienen para documentar el proceso de aprendizaje.
- **Separar secuencias:** `Ini`, `Ed` y `eval` tienen objetivos distintos.
- **No incluir `flows.json`:** la colección es material, no una instancia.
- **Documentar límites:** se prefiere declarar lo que falta antes que simular una aplicación completa.
- **Paleta sobria:** la documentación usa azul petróleo, no verde neón.

## 14. Deuda técnica

- Limpiar los 10 JSONC.
- Corregir `83-rpi-gpio-pwm-output.json`.
- Añadir `package.json` si se quiere una instancia reproducible.
- Añadir pruebas automatizadas de validación JSON.
- Documentar el contenido de `Pro.rar` si se autoriza.
- Unificar nombres de archivos con errores de tipeo (`vesion`, `versin`).

## 15. Licencia

MIT © 2026 jmadasme. Ver [`LICENSE`](../LICENSE).
