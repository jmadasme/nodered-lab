# Referencias — Node-RED Lab

**Documentación oficial, paquetes, protocolos y material de estudio**

Esta página reúne las fuentes que se usaron para documentar la colección y las que conviene consultar para extenderla. Los enlaces externos pertenecen a sus autores y pueden cambiar; la fecha de consulta es **2026-11-12**.

---

## 1. Documentación interna

| Documento | Contenido |
|---|---|
| [`README.md`](../README.md) | Visión general, instalación, importación, MQTT, dashboard, GPIO, ejemplos y límites. |
| [`docs/datasheet.md`](datasheet.md) | Manual de usuario, especificaciones, catálogo de flujos, calibración y troubleshooting. |
| [`docs/developer.md`](developer.md) | Arquitectura, formato JSON, API de funciones, limpiador JSONC y extensión. |
| [`docs/BITACORA.md`](BITACORA.md) | Problemas encontrados, decisiones y lecciones aprendidas. |
| [`LICENSE`](../LICENSE) | Licencia MIT del repositorio. |
| [`versions/NodeRed/VERSION.md`](../versions/NodeRed/VERSION.md) | Procedencia de la colección. |
| [`versions/NodeRed/secuencia Ed/create.txt`](../versions/NodeRed/secuencia%20Ed/create.txt) | Esquema SQL de la tabla `sensores`. |
| [`versions/NodeRed/Guía de Consulta Docker.pdf`](../versions/NodeRed/Guía%20de%20Consulta%20Docker.pdf) | Guía local de `docker run` interactivo. |

---

## 2. Node-RED

| Recurso | Para qué |
|---|---|
| [Sitio oficial](https://nodered.org/) | Descripción, descargas y novedades. |
| [Documentación](https://nodered.org/docs/) | Punto de entrada a la documentación. |
| [Guía de usuario](https://nodered.org/docs/user-guide/) | Editor, flujos, mensajes, contexto y paleta. |
| [Primeros pasos](https://nodered.org/docs/getting-started/) | Instalación y primer flujo. |
| [Instalación](https://nodered.org/docs/getting-started/installation) | Requisitos y métodos de instalación. |
| [Docker](https://nodered.org/docs/getting-started/docker) | Ejecutar Node-RED en contenedor. |
| [Configuración del runtime](https://nodered.org/docs/user-guide/runtime/configuration) | `settings.js`, puertos, auth y contexto. |
| [Variables de entorno](https://nodered.org/docs/user-guide/environment-variables) | `env.get` y configuración por entorno. |
| [Contexto](https://nodered.org/docs/user-guide/context) | `context`, `flow`, `global` y almacenes. |
| [Mensajes](https://nodered.org/docs/user-guide/messages) | Estructura de `msg` y `msg.parts`. |
| [Crear nodos](https://nodered.org/docs/creating-nodes/) | Desarrollo de nodos propios. |
| [API de runtime](https://nodered.org/docs/api/) | API para nodos y plugins. |
| [Seguridad](https://nodered.org/docs/user-guide/runtime/securing-node-red) | `adminAuth`, HTTPS, firewall y buenas prácticas. |
| [Repositorio GitHub](https://github.com/node-red/node-red) | Código fuente, issues y releases. |
| [Foro](https://discourse.nodered.org/) | Comunidad y preguntas. |
| [Flows Library](https://flows.nodered.org/) | Biblioteca de flujos y nodos. |

---

## 3. Dashboard

| Recurso | Para qué |
|---|---|
| [node-red-dashboard en Flows](https://flows.nodered.org/node/node-red-dashboard) | Paquete, instalación y documentación. |
| [Repositorio GitHub](https://github.com/node-red/node-red-dashboard) | Código, issues y ejemplos. |
| [Wiki del dashboard](https://github.com/node-red/node-red-dashboard/wiki) | Widgets, layout y plantillas. |
| [Dashboard 2.0 (FlowFuse)](https://flows.nodered.org/node/@flowfuse/node-red-dashboard) | Alternativa moderna; **no** es la usada por estos flujos. |
| [Documentación de FlowFuse](https://dashboard.flowfuse.com/) | Guía de Dashboard 2.0. |
| [AngularJS](https://angularjs.org/) | Base de las plantillas `ui_template` de dashboard 3.x. |

> **Importante:** este repositorio usa `node-red-dashboard` **3.6.6**, la generación Angular. Los ejemplos con `ng-src` no son portables a Dashboard 2.0 sin reescribir la plantilla.

---

## 4. Nodos y paquetes

| Paquete | Documentación | Uso en el repo |
|---|---|---|
| `node-red-node-pi-gpio` | [Flows](https://flows.nodered.org/node/node-red-node-pi-gpio) · [GitHub](https://github.com/node-red/node-red-nodes/tree/master/hardware/PiGpio) | `rpi-gpio in/out` (flujos 81–83). |
| `node-red-node-email` | [Flows](https://flows.nodered.org/node/node-red-node-email) · [GitHub](https://github.com/node-red/node-red-nodes/tree/master/io/email) | Nodo `e-mail` (flujo 09). |
| `node-red-contrib-postgresql` | [Flows](https://flows.nodered.org/node/node-red-contrib-postgresql) | `postgres` / `postgresdb` (flujos 71–73). |
| `node-red-node-serialport` | [Flows](https://flows.nodered.org/node/node-red-node-serialport) | Alternativa para sensores seriales (no usada). |
| `node-red-node-smooth` | [Flows](https://flows.nodered.org/node/node-red-node-smooth) | Filtros y suavizado (no usado). |
| `node-red-contrib-modbus` | [Flows](https://flows.nodered.org/node/node-red-contrib-modbus) | Modbus (no usado). |
| `node-red-contrib-opcua` | [Flows](https://flows.nodered.org/node/node-red-contrib-opcua) | OPC UA (no usado). |

### Gestión de la paleta

- [Palette Manager](https://nodered.org/docs/user-guide/editor/palette/manager): instalar, actualizar y eliminar nodos.
- [package.json en Node-RED](https://nodered.org/docs/user-guide/runtime/configuration#packagejson): declarar dependencias de una instancia.

---

## 5. MQTT

| Recurso | Para qué |
|---|---|
| [MQTT.org](https://mqtt.org/) | Especificación y ecosistema. |
| [MQTT 3.1.1 (OASIS)](https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html) | Protocolo usado por la mayoría de los brokers. |
| [MQTT 5.0 (OASIS)](https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html) | Versión moderna con propiedades y reason codes. |
| [Mosquitto](https://mosquitto.org/) | Broker ligero y cliente CLI. |
| [Mosquitto: documentación](https://mosquitto.org/documentation/) | Configuración, TLS y ACL. |
| [mosquitto.conf](https://mosquitto.org/man/mosquitto-conf-5.html) | Referencia de configuración. |
| [mosquitto_pub](https://mosquitto.org/man/mosquitto_pub-1.html) | Publicar desde CLI. |
| [mosquitto_sub](https://mosquitto.org/man/mosquitto_sub-1.html) | Suscribirse desde CLI. |
| [test.mosquitto.org](https://test.mosquitto.org/) | Broker público de pruebas. **No usar con datos reales.** |
| [Eclipse Paho Python](https://www.eclipse.org/paho/index.php?page=clients/python/docs/index.php) | Cliente MQTT para Python. |
| [HiveMQ: fundamentos de MQTT](https://www.hivemq.com/blog/mqtt-essentials/) | Serie introductoria. |

### Tópicos y QoS

- [MQTT Topics and Wildcards](https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html#_Toc398718106): `+`, `#`, niveles.
- [QoS](https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html#_Toc398718099): 0, 1, 2.
- [Retained messages](https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/os/mqtt-v3.1.1-os.html#_Toc398718103): último valor persistente.

---

## 6. HTTP, WebSocket, TCP y UDP

| Recurso | Para qué |
|---|---|
| [MDN HTTP](https://developer.mozilla.org/es/docs/Web/HTTP) | Métodos, cabeceras, códigos. |
| [RFC 9110 — HTTP Semantics](https://www.rfc-editor.org/rfc/rfc9110) | Especificación actual. |
| [RFC 6455 — WebSocket](https://www.rfc-editor.org/rfc/rfc6455) | Protocolo WebSocket. |
| [RFC 768 — UDP](https://www.rfc-editor.org/rfc/rfc768) | Datagramas. |
| [RFC 9293 — TCP](https://www.rfc-editor.org/rfc/rfc9293) | Transporte confiable. |
| [MDN WebSocket API](https://developer.mozilla.org/es/docs/Web/API/WebSocket) | Cliente WebSocket en navegador. |
| [Node.js `net`](https://nodejs.org/api/net.html) | TCP en Node. |
| [Node.js `dgram`](https://nodejs.org/api/dgram.html) | UDP en Node. |
| [Postman](https://www.postman.com/) | Probar APIs. |
| [curl](https://curl.se/docs/) | Cliente HTTP de línea de comandos. |

---

## 7. Raspberry Pi y GPIO

| Recurso | Para qué |
|---|---|
| [Raspberry Pi Documentation](https://www.raspberrypi.com/documentation/) | Documentación oficial. |
| [GPIO](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#gpio) | Pines, niveles y funciones. |
| [pinout.xyz](https://pinout.xyz/) | Mapa interactivo de los 40 pines. |
| [Raspberry Pi OS](https://www.raspberrypi.com/software/) | Sistema operativo. |
| [libgpiod](https://git.kernel.org/pub/scm/libs/libgpiod/libgpiod.git/) | GPIO moderno en Linux. |
| [gpiozero](https://gpiozero.readthedocs.io/) | Biblioteca Python para GPIO. |
| [Raspberry Pi 5 y GPIO](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html) | Cambios de la Pi 5. |
| [Node-RED en Raspberry Pi](https://nodered.org/docs/getting-started/raspberrypi) | Instalador y servicio. |

### Seguridad eléctrica

- [GPIO: corrientes y tensiones](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#gpio-pads): no aplicar 5 V.
- [LED: resistor y corriente](https://www.allaboutcircuits.com/textbook/semiconductors/chpt-3/light-emitting-diodes/): cálculo de resistencia.
- [Relés y cargas inductivas](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html): usar driver y diodo.

---

## 8. PostgreSQL

| Recurso | Para qué |
|---|---|
| [PostgreSQL Documentation](https://www.postgresql.org/docs/) | Manual oficial. |
| [SQL INSERT](https://www.postgresql.org/docs/current/sql-insert.html) | Inserción. |
| [SQL SELECT](https://www.postgresql.org/docs/current/sql-select.html) | Consulta. |
| [SQL DELETE](https://www.postgresql.org/docs/current/sql-delete.html) | Borrado. |
| [Tipos de datos](https://www.postgresql.org/docs/current/datatype.html) | `FLOAT`, `TIMESTAMPTZ`, etc. |
| [`pg_hba.conf`](https://www.postgresql.org/docs/current/auth-pg-hba-conf.html) | Autenticación de clientes. |
| [psql](https://www.postgresql.org/docs/current/app-psql.html) | Cliente de línea de comandos. |
| [node-postgres](https://node-postgres.com/) | Driver usado por muchos nodos. |

### Prevención de inyección SQL

- [OWASP: SQL Injection](https://owasp.org/www-community/attacks/SQL_Injection)
- [OWASP: SQL Injection Prevention](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [PostgreSQL: consultas parametrizadas](https://www.postgresql.org/docs/current/sql-prepare.html)

---

## 9. Correo electrónico

| Recurso | Para qué |
|---|---|
| [RFC 5321 — SMTP](https://www.rfc-editor.org/rfc/rfc5321) | Protocolo de envío. |
| [RFC 6409 — Message Submission](https://www.rfc-editor.org/rfc/rfc6409) | Puerto 587. |
| [Nodemailer](https://nodemailer.com/) | Librería usada por el nodo de correo. |
| [Gmail: contraseñas de aplicación](https://support.google.com/accounts/answer/185833) | Autenticación sin password principal. |
| [Microsoft: contraseñas de aplicación](https://support.microsoft.com/account-billing/using-app-passwords-with-apps-that-don-t-support-two-step-verification-5896ed9b-4263-e681-128a-a6f2979a7944) | Alternativa a OAuth. |

---

## 10. APIs de ejemplo

| Recurso | Para qué |
|---|---|
| [Open-Meteo](https://open-meteo.com/) | API meteorológica sin key. |
| [Open-Meteo: documentación](https://open-meteo.com/en/docs) | Parámetros y respuesta. |
| [JSONPlaceholder](https://jsonplaceholder.typicode.com/) | API falsa para GET/POST. |
| [httpbin](https://httpbin.org/) | Espejo de peticiones HTTP. |
| [World Time API](https://worldtimeapi.org/) | Hora por zona (alternativa). |

---

## 11. Docker

| Recurso | Para qué |
|---|---|
| [Docker Documentation](https://docs.docker.com/) | Manual oficial. |
| [`docker run`](https://docs.docker.com/reference/cli/docker/container/run/) | Referencia del comando. |
| [`docker container`](https://docs.docker.com/reference/cli/docker/container/) | Gestión de contenedores. |
| [Imagen `nodered/node-red`](https://hub.docker.com/r/nodered/node-red) | Imagen oficial. |
| [Docker Compose](https://docs.docker.com/compose/) | Orquestación local. |
| [Volúmenes](https://docs.docker.com/storage/volumes/) | Persistencia de `/data`. |

---

## 12. Node.js y JavaScript

| Recurso | Para qué |
|---|---|
| [Node.js](https://nodejs.org/) | Runtime. |
| [Descargas](https://nodejs.org/en/download) | Instaladores. |
| [Releases LTS](https://nodejs.org/en/about/previous-releases) | Calendario de soporte. |
| [MDN JavaScript](https://developer.mozilla.org/es/docs/Web/JavaScript) | Referencia del lenguaje. |
| [MDN `Date`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Date) | Fechas y zonas horarias. |
| [MDN `Intl.DateTimeFormat`](https://developer.mozilla.org/es/docs/Web/JavaScript/Reference/Global_Objects/Intl/DateTimeFormat) | Formato con `timeZone`. |
| [npm](https://docs.npmjs.com/) | Gestión de paquetes. |

---

## 13. Seguridad y buenas prácticas

| Recurso | Para qué |
|---|---|
| [OWASP Top 10](https://owasp.org/www-project-top-ten/) | Riesgos web. |
| [OWASP Cheat Sheets](https://cheatsheetseries.owasp.org/) | Guías concretas. |
| [Node-RED: securing](https://nodered.org/docs/user-guide/runtime/securing-node-red) | Auth, HTTPS, firewall. |
| [Caddy](https://caddyserver.com/docs/) | Reverse proxy con TLS automático. |
| [Nginx](https://nginx.org/en/docs/) | Reverse proxy. |
| [Let's Encrypt](https://letsencrypt.org/docs/) | Certificados TLS. |
| [Mozilla SSL Configuration Generator](https://ssl-config.mozilla.org/) | Configuración TLS. |

---

## 14. Herramientas de validación

| Recurso | Para qué |
|---|---|
| [Python `json`](https://docs.python.org/3/library/json.html) | Parseo y validación. |
| [`python3 -m json.tool`](https://docs.python.org/3/library/json.html#module-json.tool) | Validar y formatear JSON. |
| [jq](https://jqlang.github.io/jq/) | Procesar JSON en shell. |
| [JSONLint](https://jsonlint.com/) | Validador online. |
| [7-Zip](https://www.7-zip.org/) | Abrir RAR/ZIP (el RAR del repo está cifrado). |
| [Wireshark](https://www.wireshark.org/) | Analizar MQTT/TCP/UDP. |
| [MQTT Explorer](https://mqtt-explorer.com/) | Inspeccionar brokers MQTT. |

---

## 15. Lecturas recomendadas

- **Node-RED: del prototipo a la producción.** Conceptos de contexto, errores y seguridad.
- **MQTT Essentials (HiveMQ).** Serie de artículos para entender publish/subscribe.
- **Raspberry Pi GPIO: guía de seguridad.** Tensiones, corrientes y protecciones.
- **OWASP SQL Injection Prevention.** Obligatorio antes de tocar el flujo 71.
- **PostgreSQL: transacciones y aislamiento.** Para persistencia de telemetría.
- **Docker: volúmenes y redes.** Para desplegar Node-RED sin perder datos.

---

## 16. Cómo citar este repositorio

```text
jmadasme. (2026). Node-RED Lab: flujos, dashboards, MQTT y GPIO
[Repositorio de laboratorio]. Licencia MIT.
```

Si reutilizás un flujo, conservá la atribución de la colección original `jmadasme/NodeRed` y revisá la licencia de cada paquete de terceros.

---

## 17. Estado de los enlaces

Los enlaces se verificaron conceptualmente al preparar esta documentación, pero los sitios externos cambian. Si encontrás uno roto:

1. Buscá el título exacto en el sitio oficial.
2. Revisá la versión archivada en [Wayback Machine](https://web.archive.org/).
3. Abrí un issue en el repositorio con la URL y el reemplazo propuesto.

**Última revisión:** 2026-11-12.
