# Práctica: Panadería — Lógica y Persistencia

## Objetivo

Armar la base de una solución con arquitectura por capas y persistencia en archivos (Unidad 5) respaldada por tests (Unidad 4), diseñada para que **en la siguiente práctica de esta unidad se le sume un proyecto de Web API** sin reescribir nada de lo hecho acá.

---

## Contexto del ejercicio

Esta práctica se resuelve en etapas. Primero se resuelven las primeras dos (lógica de negocio con persistencia en archivos, y tests unitarios). Más adelante, sobre esta misma solución, se agrega un tercer proyecto con una **Web API** que expone esta lógica por HTTP — el contenido de esta misma unidad.

---

## Enunciado

Una panadería de barrio maneja los pedidos de sus clientes mediante un cuaderno en el mostrador. Necesitan reemplazarlo por un sistema propio.

Cada pedido tiene un identificador único autonumérico, un nombre de cliente (obligatorio, no puede estar vacío ni superar los 100 caracteres), un detalle (obligatorio, no puede estar vacío — por ejemplo "2 tortas de chocolate, 1 docena de facturas"), un tipo de entrega (retiro en el local o envío a domicilio) y una fecha de creación que se asigna automáticamente al crearlo.

Todo pedido nace en estado **Recibido**. A partir de ahí, solo puede avanzar en un orden fijo: de Recibido pasa a **En Preparación** cuando alguien empieza a armarlo, de En Preparación pasa a **Listo** cuando termina de hornearse/armarse, y de Listo pasa a **Entregado** cuando el cliente lo retira. No se puede saltear pasos (por ejemplo, no se puede entregar un pedido que nunca se empezó a preparar), ni retroceder, ni modificar un pedido que ya está Entregado.

El sistema debe permitir: crear pedidos, listar todos los pedidos, buscar un pedido por id, tomar un pedido, marcarlo listo, entregarlo, listar los pedidos filtrando por estado, y buscar pedidos por texto en el nombre del cliente. Todos los pedidos deben persistir en un archivo, de forma que la información sobreviva entre ejecuciones.

Cualquier intento de violar estas reglas (nombre de cliente vacío, transición de estado inválida, búsqueda de un pedido que no existe, etc.) debe manejarse de forma explícita, sin que el programa se rompa de forma inesperada.

---

## Notas

Conceptos de las unidades 1 a 5 que se ponen en juego (no se indica dónde va cada uno — eso es parte del análisis):

- Clases, propiedades y encapsulamiento
- Enumeradores (`enum`) para tipo de entrega y estado
- Excepciones (`ArgumentException`, `InvalidOperationException`)
- Listas (`List<T>`) y LINQ (filtrado, búsqueda)
- Serialización JSON con Newtonsoft.Json y persistencia en archivos (`File.ReadAllText` / `File.WriteAllText`)
- Arquitectura por capas: Entidades → Servicio → Datos (Repositorio)
- Testing unitario con NUnit (patrón AAA, nomenclatura `Metodo_Escenario_ResultadoEsperado`)

> No hace falta usar herencia, clases abstractas ni interfaces en este ejercicio — hay una sola entidad (`Pedido`), así que forzar una jerarquía de clases sería una complejidad artificial. El foco de esta práctica es persistencia + testing, no diseño orientado a objetos avanzado.

---

### Persistencia

- El archivo (por ejemplo `pedidos.json`) se guarda como una lista completa cada vez que cambia algo (siguiendo el patrón "leer todo → modificar en memoria → guardar todo" visto en la Unidad 5).
- Si el archivo no existe todavía (primera ejecución), el repositorio debe devolver una lista vacía, no romper.
- La ruta del archivo la decide `PedidoService`, no quien lo use — el repositorio solo sabe leer y escribir en la ruta que le pasan por constructor.

### Tests unitarios

Escribir tests con NUnit que cubran, como mínimo:
- Creación de un pedido válido (queda en estado Recibido).
- Creación con nombre de cliente vacío o demasiado largo (debe lanzar excepción).
- Creación con detalle vacío (debe lanzar excepción).
- Transición de estado válida (Recibido → En Preparación → Listo → Entregado).
- Al menos dos transiciones inválidas (por ejemplo, entregar un pedido Recibido; o intentar modificar un pedido ya Entregado).
- Búsqueda por id inexistente.
- Filtrado por estado y búsqueda por texto en el nombre del cliente.

Ejecutar `dotnet test` antes de dar por terminada la práctica y confirmar que todo pasa en verde.

---

## Preparación para sumar la Web API

Estas reglas no son "buenas prácticas" genéricas — son las que hacen que, en la próxima práctica de esta unidad, agregar un proyecto `webapi` sea directo:

1. **`PedidoService` no debe imprimir nada por consola ni leer input.** Sus métodos reciben parámetros y devuelven objetos (`Pedido`, `List<Pedido>`) o lanzan excepciones.
2. **Buscar por id que no existe devuelve `null`, no lanza excepción.**
3. **Las reglas de negocio violadas lanzan excepciones tipadas** (`ArgumentException` para datos inválidos, `InvalidOperationException` para transiciones de estado inválidas).
4. **Los métodos del Service se nombran pensando en el verbo HTTP que van a exponer**: `ObtenerTodos`, `ObtenerPorId`, `Crear`, `TomarPedido`, `MarcarListo`, `Entregar`, `ObtenerPorEstado`, `BuscarPorCliente`.
5. **No hardcodear la ruta del archivo JSON con una ruta absoluta.** Usar una ruta relativa simple (`"pedidos.json"`), tal como se hizo en la Unidad 5 — cuando se sume el proyecto de API, ese archivo va a vivir junto al nuevo proyecto sin cambios de código.

Si se respetan estos cinco puntos, sumar la API más adelante será sencillo

---

## Entrega

- Crear un repositorio privado en GitHub con el nombre `Prog1-Practica-Panaderia-TuApellido` (reemplazá `TuApellido` por tu apellido real).
- Agregar a los profesores como colaboradores.
- Incluir un `.gitignore` para proyectos .NET.
- Commits descriptivos que reflejen el avance (armado de la solución, entidad y enums, repositorio, service, tests).

---

*Práctica | Unidad 6: Web Services y REST APIs | Programación 1 | UCSE*
