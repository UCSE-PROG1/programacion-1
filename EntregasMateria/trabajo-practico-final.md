# TP Integrador — Sistema de Venta de Entradas para Eventos

- **Materia:** Programación 1 — UCSE
- **Modalidad:** Grupal (equipos de 2 o 3 integrantes, ver sección 1)
- **Dedicación estimada:** 4 hs/semana por integrante durante 6 semanas (~72 hs de equipo)
- **Unidades evaluadas:** 1 a 7 (C#/.NET, POO, Git, Testing, Archivos/JSON, Web APIs REST, Frontend)
- **Nombre del repositorio:** `2026-prog1-tpfinal-<nombre-del-equipo>`

---

## 1. Equipos y armado

- El curso se organiza en **9 equipos de 3 personas** y **2 equipos de 2 personas**. Los equipos de 2 personas no tienen que resolver las dos últimas pantallas de la sección 8 (control de acceso y consulta de una compra).
- Cada equipo elige un **nombre**, relacionado con el trabajo, que no sea de temática religiosa, política ni racista.
- Cada equipo crea un **canal de Slack** con el nombre del equipo, y suma al **Profesor Gonzalo** a ese canal.
- En el hilo de slack del TP, un integrante por equipo debera indicar por quienes estan conformados los equipos, solamente los primeros 2 de 2 personas quedarán asi: https://programacion1-2026.slack.com/archives/C0AKJPB83KM/p1787261674846039
- El nombre del repositorio se arma reemplazando `<nombre-del-equipo>` por el nombre elegido, por ejemplo: `2026-prog1-tpfinal-ticketflow`.

---

## 2. Fechas de entrega

- **Viernes 02/10 — Web APIs completas y funcionando.** Si la entrega no compila o no está completa, se pierde la posibilidad de promoción (si el equipo la tenía). Se va a corregir lo entregado hasta esta fecha, pero el equipo tiene que seguir trabajando para que la entrega esté funcional el día del coloquio.
- **Viernes 16/10 — Páginas HTML que completan el trabajo** (el frontend).
- **Coloquios: 27/10 y 29/10**, comenzando a las 18 hs.

---

## 3. Contexto

Una organizadora de eventos maneja hoy la venta de entradas con planillas de Excel y grupos de WhatsApp, y quiere reemplazar eso por un sistema propio. El problema no termina cuando se vende una entrada: el día del evento, en la puerta, alguien tiene que poder confirmar que esa entrada es legítima y que no la está usando dos veces la misma persona (o dos personas distintas con una foto de la misma entrada).

Por eso pide **dos sistemas separados, que se ejecutan en local y comparten la misma información**:

- Uno para **gestionar** todo lo que pasa antes del evento: qué eventos existen, qué modalidades de entrada tiene cada uno, y la venta de esas entradas.
- Otro, pensado para usarse **el día del evento, en la puerta**, cuyo único propósito es **validar entradas**: dado el código de una entrada, decir si se puede dejar pasar a esa persona o no.

Los dos sistemas trabajan sobre los mismos datos: una entrada vendida desde el primero tiene que poder validarse desde el segundo. No hay una base de datos central ni un tercer servicio que sincronice — ambos leen y escriben directamente sobre la misma información persistida en disco.

Sobre estos dos servicios se construye una aplicación web con varias pantallas, para que tanto la organizadora como el personal de la puerta puedan usar el sistema sin tocar una API directamente. No hay pantalla de login ni contraseñas — los usuarios ya existen, precargados de antemano —, pero eso no significa que cualquiera pueda hacer cualquier cosa: cada acción sigue estando permitida solo para determinado tipo de usuario, así que el sistema necesita saber, en cada pedido, quién lo está haciendo.

El análisis de qué información hace falta guardar, qué entidades existen y cómo se relacionan entre sí es parte del trabajo del equipo — no viene dado. Lean con atención las secciones siguientes: ahí está toda la información necesaria para diseñar el modelo.

---

## 4. Qué resuelve cada sistema

**Gestión de eventos y ventas.** La organizadora publica los eventos que van a existir: de qué se trata cada uno, cuándo y dónde es. Un evento puede tener más de una modalidad de entrada — por ejemplo, acceso general y otra que dé algo más (un sector especial, una consumición, lo que sea) — y esas modalidades no necesariamente cuestan lo mismo ni tienen la misma cantidad disponible. La organizadora necesita poder cancelar un evento si algo se cae, y saber en cualquier momento cuánto se recaudó y cuántas entradas se vendieron por evento.

Cualquiera puede recorrer el catálogo de eventos, elegir uno y ver las modalidades disponibles. Pero gestionar eventos (crearlos, editarlos, cancelarlos) y ver el reporte de recaudación son acciones exclusivas de un usuario con rol **organizador** — un usuario con rol **comprador** no debería poder hacerlas, ni verlas disponibles. Comprar entradas, en cambio, lo hace un usuario con rol comprador, y la compra queda asociada a su DNI.

Comprar una entrada no es solo "restar del cupo": el día del evento, alguien en la puerta tiene que poder confirmar, entrada por entrada, si esa entrada puntual ya se usó o no. Si alguien compra 3 entradas en una sola operación, esas 3 personas van a entrar en momentos distintos — el sistema tiene que poder validar cada una por separado, no la compra como un todo.

**Validación de entradas.** Es la aplicación que se usa en la puerta el día del evento. Recibe el código de una entrada y tiene que responder si esa entrada es válida para ingresar en ese momento, o si hay que rechazarla — y por qué (no existe, es de otro evento, ya fue usada). Cuando una entrada se valida correctamente, queda registrado que esa entrada ya ingresó, para que no se pueda volver a usar. Esto tiene que reflejarse contra los mismos datos que carga la aplicación de gestión: si alguien compró la entrada cinco minutos antes por la otra aplicación, tiene que poder validarla ya.

---

## 5. Reglas del negocio

Estas son las reglas que el sistema tiene que hacer cumplir siempre, sin excepción:

- No se puede vender una entrada si ya no queda cupo disponible de esa modalidad para ese evento.
- No se puede comprar una entrada para un evento que ya fue cancelado, que ya terminó, o cuya fecha ya pasó.
- Si alguien compra 5 entradas o más de la misma modalidad en una sola compra, el precio total tiene un descuento respecto de comprarlas de a una.
- Cada modalidad de entrada de un evento puede tener sus propias condiciones (precio, beneficios, cupo) independientes de las demás modalidades del mismo evento.
- Cada entrada vendida se valida de forma individual el día del evento, no la compra completa — si alguien compró 3 entradas, cada una se usa (y se valida) por separado.
- Una entrada solo puede usarse para ingresar **una vez**. Si se intenta validar de nuevo una entrada que ya ingresó, el sistema tiene que rechazarla.
- Una entrada solo es válida para el evento con el que se compró — no sirve para otro evento aunque sea de la misma organizadora.
- Un usuario con rol comprador no puede gestionar eventos ni ver el reporte de recaudación, aunque conozca la dirección del endpoint correspondiente.

Estas reglas — y no una lista de clases — son el punto de partida para decidir qué entidades necesita el sistema, qué datos tiene que guardar cada una, y dónde conviene que viva cada comportamiento (por ejemplo: ¿el descuento es algo que calcula la modalidad de entrada, o algo que decide quien procesa la compra? ¿Qué necesita guardar una entrada individual para poder validarse sola, distinta de las demás de la misma compra? Esas decisiones son del equipo, y deberían poder defenderse).

---

## 6. Requisitos técnicos

- **Dos APIs REST** (ASP.NET Core) ejecutándose en local: una de **gestión** (eventos, modalidades, ventas, reportes) y otra de **validación de entradas** (consultada el día del evento). La API de gestión tiene que tener **Swagger habilitado**, para poder probar sus endpoints sin necesidad del frontend.
- **Ambas APIs comparten los mismos datos.** No hay base de datos ni un tercer servicio en el medio: las dos leen y escriben sobre los mismos archivos en disco. Una entrada vendida por la API de gestión tiene que poder validarse inmediatamente desde la API de validación.
- **Usuarios precargados y validación por rol.** Los usuarios del sistema (organizadores y compradores) vienen precargados en un archivo, con su DNI, nombre, username y rol (ver anexo al final de este documento). No hay login ni contraseña, pero cada endpoint de la API de gestión que corresponda a una acción restringida (gestionar eventos, ver el reporte) tiene que recibir el DNI de quien hace el pedido y validar, contra ese archivo de usuarios, si el rol de esa persona tiene permiso para esa acción — rechazando el pedido si no lo tiene.
- **El frontend también respeta el rol.** No alcanza con que el backend rechace lo que no corresponde: la interfaz tampoco debe mostrarle a un usuario las opciones de una acción para la que no tiene permiso (por ejemplo, un comprador no debería ver el botón para crear un evento).
- **Persistencia en archivos**, no en memoria: la información tiene que sobrevivir a un reinicio de cualquiera de las dos APIs. No se usa base de datos ni ORM — se serializa el estado a disco.
- **Pruebas unitarias (NUnit)** sobre la lógica de negocio (las reglas de la sección 5), no sobre getters/setters. Cada una de las dos APIs tiene que tener su propio proyecto de lógica separado del proyecto de la API, y su propio proyecto de tests sobre esa lógica — no alcanza con testear una sola de las dos. El equipo decide qué casos de borde probar; se van a evaluar tanto los casos que tienen que funcionar como los que tienen que ser rechazados (entrada inexistente, ya usada, de otro evento, cupo agotado, evento cancelado, etc.).
- **Frontend web** en HTML, CSS y JavaScript (sin frameworks) que consuma ambas APIs. No requiere login.

---

## 7. Métodos REST a resolver

Esta es la lista mínima de operaciones que cada API tiene que exponer. Los nombres de ruta son una sugerencia para que las dos APIs sean fáciles de integrar entre sí — el equipo puede ajustarlos, pero tiene que cubrir estas operaciones.

Los endpoints marcados con un rol requerido tienen que recibir también el DNI de quien hace el pedido (como parte de la URL, query string, header o body — es decisión del equipo) para poder validar el rol contra el archivo de usuarios.

### API de gestión

| Método | Endpoint sugerido | Rol requerido | Qué resuelve |
|---|---|---|---|
| GET | `/api/usuarios` | — | Lista los usuarios cargados (para elegir con cuál se está operando). |
| GET | `/api/eventos` | — | Lista los eventos disponibles. |
| GET | `/api/eventos/{id}` | — | Detalle de un evento con sus modalidades de entrada. |
| POST | `/api/eventos` | Organizador | Crea un evento. |
| PUT | `/api/eventos/{id}` | Organizador | Edita un evento. |
| PUT | `/api/eventos/{id}/cancelar` | Organizador | Cancela un evento. |
| POST | `/api/eventos/{id}/modalidades` | Organizador | Agrega una modalidad de entrada a un evento. |
| POST | `/api/compras` | Comprador | Registra una compra y genera las entradas individuales correspondientes. |
| GET | `/api/compras/{id}` | — | Consulta el detalle de una compra y el estado de cada entrada que generó. |
| GET | `/api/reportes/recaudacion` | Organizador | Recaudación y entradas vendidas por evento. |

Solo para equipos con algún integrante en condición de promoción (ver sección 12):

| Método | Endpoint sugerido | Rol requerido | Qué resuelve |
|---|---|---|---|
| DELETE | `/api/entradas/{codigo}` | Comprador | Cancela una entrada ya comprada y la vuelve a dejar disponible. |

### API de validación de entradas

| Método | Endpoint sugerido | Qué resuelve |
|---|---|---|
| POST | `/api/validaciones` | Recibe el código de una entrada. Si es válida, la marca como usada y confirma el ingreso; si no, rechaza e indica el motivo (no existe, ya fue usada, es de otro evento). |

---

## 8. Pantallas esperadas

La aplicación tiene que resolver, con pantallas concretas, al menos:

1. Una forma simple de indicar qué usuario está operando el sistema en ese momento (no es un login: no se pide contraseña, alcanza con elegir o ingresar su DNI de la lista de usuarios cargados). A partir de ahí, el resto de las pantallas se ajusta a lo que ese usuario tiene permitido.
2. Un catálogo de eventos disponibles.
3. El detalle de un evento puntual, donde se pueda elegir una modalidad de entrada, una cantidad, y confirmar la compra (solo visible para un comprador).
4. Una pantalla de **control de acceso**, pensada para usarse el día del evento: se ingresa el código de una entrada y el sistema muestra si se puede dejar pasar o no (y por qué, si se rechaza).
5. Una vista para consultar el detalle de una compra ya hecha y ver el estado de cada entrada que generó — usada o no.

Las pantallas 4 y 5 no son obligatorias para los dos equipos de 2 personas (ver sección 1).

La gestión de eventos (crear/editar/cancelar eventos y sus modalidades de entrada) no requiere una pantalla propia: alcanza con que esos endpoints existan y funcionen (se pueden probar desde Swagger).

Cómo se organizan esas pantallas — un único sitio con secciones, o dos frontends separados (uno pensado para venta/gestión y otro para la puerta el día del evento) — es una decisión del equipo.

---

## 9. Trabajo en equipo (Unidad 3 — Git)

- Un repositorio llamado `2026-prog1-tpfinal-<nombre-del-equipo>`, con las dos soluciones .NET y el frontend, organizado en carpetas en la raíz:
  - `CarpetaApi1` — solución de una de las dos APIs.
  - `CarpetaApi2` — solución de la otra API.
  - `CarpetaFrontend` — el frontend (o `CarpetaFrontend1` y `CarpetaFrontend2` si el equipo decide separarlo en dos).
  
  Los nombres de carpeta son orientativos: cada equipo los puede reemplazar por algo más descriptivo (por ejemplo, `ApiGestion`, `ApiValidacion`), siempre que la estructura de tres (o cuatro) carpetas en la raíz se mantenga.
- Los integrantes deben tener commits distribuidos a lo largo de las 6 semanas en más de una parte del sistema — no vale que una sola persona haga toda una API de punta a punta y nunca toque el resto. Recomendado: rotar de área cada 1-2 semanas o trabajar en pares cruzados.

---

## 10. Cronograma sugerido (6 semanas)

| Semana | Foco | Unidad |
|---|---|---|
| 1 | Lectura y análisis del enunciado, diseño del modelo (qué entidades, qué datos, qué relaciones), setup del repo y de las 2 soluciones .NET | 1, 3 |
| 2 | Lógica de negocio en memoria (sin persistencia todavía): eventos, modalidades, compra, generación de entradas individuales + primeros tests | 2, 4 |
| 3 | Persistencia compartida en archivos + tests de los casos de borde de la sección 5, en particular la validación de entradas | 4, 5 |
| 4 | Exposición como servicios REST (Swagger) de ambas APIs, verificando que las dos leen y escriben correctamente sobre los mismos datos | 6 |
| 5 | Frontend de gestión y venta: catálogo, detalle, compra | 7 |
| 6 | Frontend de control de acceso, integración final, README, demo | 7 |

Las fechas concretas de entrega están en la sección 2 — este cronograma es una guía de ritmo, no reemplaza esas fechas.

---

## 11. Entregables

- Repositorio con las 2 soluciones .NET + frontend, con historial de commits/PRs de los integrantes.
- README con instrucciones para levantar ambas APIs y abrir el frontend en local.
- Suite de tests NUnit en verde en los dos proyectos de lógica (gestión y validación).
- Video de la demo funcional, de **no más de 1 minuto**: alta de un evento → venta de una compra con varias entradas → validación exitosa de una de esas entradas en la puerta → intento de volver a validar la misma entrada (tiene que rechazarse).

---

## 12. Sección para quienes estén en condición de promoción

Este punto es adicional, y aplica para los equipos que tengan al menos una persona en condición de promoción.

La organizadora pide algo más: quiere que, al ver el detalle de un evento, se pueda ubicar en un mapa dónde es. Queda a cargo de esos integrantes **investigar e implementar** el uso de **Google Maps** en el frontend (HTML/JS) para mostrar la ubicación de un evento.

Esto no es solo agregar un mapa en la pantalla: probablemente implique repensar qué datos guarda un evento sobre su ubicación — hoy alcanza con un texto libre tipo "Lugar", pero un mapa necesita algo más preciso para poder ubicar un punto. Qué cambia exactamente en el modelo, y en qué capa (dominio, persistencia, API, frontend) se resuelve cada parte, queda para que lo investiguen y decidan.

Además, para estos equipos, un comprador tiene que poder **cancelar una entrada ya comprada** y que esa entrada vuelva a estar disponible para que otro la compre (ver el endpoint correspondiente en la sección 7). Pensar qué pasa si la entrada que se quiere cancelar ya fue validada en la puerta el día del evento es parte del análisis — no toda entrada comprada debería poder cancelarse en cualquier momento.

---

## Anexo: usuarios precargados

Archivo `usuarios.json` con los 10 usuarios de ejemplo (2 organizadores, 8 compradores) que debe cargar la API de gestión al iniciar:

```json
[
  { "dni": 30111222, "nombre": "Lucía Fernández", "username": "lfernandez", "rol": "Organizador" },
  { "dni": 29888777, "nombre": "Martín Aguirre", "username": "maguirre", "rol": "Organizador" },
  { "dni": 40123456, "nombre": "Sofía Gómez", "username": "sgomez", "rol": "Comprador" },
  { "dni": 38456789, "nombre": "Nicolás Pereyra", "username": "npereyra", "rol": "Comprador" },
  { "dni": 41234567, "nombre": "Valentina Ríos", "username": "vrios", "rol": "Comprador" },
  { "dni": 37654321, "nombre": "Tomás Ibáñez", "username": "tibanez", "rol": "Comprador" },
  { "dni": 42345678, "nombre": "Camila Suárez", "username": "csuarez", "rol": "Comprador" },
  { "dni": 39876543, "nombre": "Agustín Molina", "username": "amolina", "rol": "Comprador" },
  { "dni": 43456789, "nombre": "Julieta Torres", "username": "jtorres", "rol": "Comprador" },
  { "dni": 36765432, "nombre": "Bruno Acosta", "username": "bacosta", "rol": "Comprador" }
]
```
