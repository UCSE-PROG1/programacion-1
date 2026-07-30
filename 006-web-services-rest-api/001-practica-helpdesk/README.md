# Práctica: HelpDesk — Lógica y Persistencia

## Objetivo

Armar la base de una solución con arquitectura por capas y persistencia en archivos (Unidad 5) respaldada por tests (Unidad 4), diseñada para que **en la siguiente práctica de esta unidad se le sume un proyecto de Web API** sin reescribir nada de lo hecho acá.

---

## Contexto del ejercicio

Esta práctica se resuelve en etapas. Primero se resuelven las primeras dos (lógica de negocio con persistencia en archivos, y tests unitarios). Más adelante, sobre esta misma solución, se agrega un tercer proyecto con una **Web API** que expone esta lógica por HTTP — el contenido de esta misma unidad.

---

## Enunciado

Una empresa de soporte técnico maneja los reclamos de sus clientes mediante **tickets**. Necesitan reemplazar la planilla de Excel que usan hoy por un sistema propio.

Cada ticket tiene un identificador único autonumérico, un título (obligatorio, no puede estar vacío ni superar los 100 caracteres), una descripción (obligatoria, no puede estar vacía), una prioridad (baja, media, alta o crítica) y una fecha de creación que se asigna automáticamente al crearlo.

Todo ticket nace en estado **Abierto**. A partir de ahí, solo puede avanzar en un orden fijo: de Abierto pasa a **En Proceso** cuando alguien lo toma, de En Proceso pasa a **Resuelto** cuando se soluciona, y de Resuelto pasa a **Cerrado** cuando el cliente confirma. No se puede saltear pasos (por ejemplo, no se puede cerrar un ticket que nunca fue tomado), ni retroceder, ni modificar un ticket que ya está Cerrado.

El sistema debe permitir: crear tickets, listar todos los tickets, buscar un ticket por id, tomar un ticket, resolverlo, cerrarlo, listar los tickets filtrando por estado, y buscar tickets por texto en el título. Todos los tickets deben persistir en un archivo, de forma que la información sobreviva entre ejecuciones.

Cualquier intento de violar estas reglas (título vacío, transición de estado inválida, búsqueda de un ticket que no existe, etc.) debe manejarse de forma explícita, sin que el programa se rompa de forma inesperada.

---

## Notas

Conceptos de las unidades 1 a 5 que se ponen en juego (no se indica dónde va cada uno — eso es parte del análisis):

- Clases, propiedades y encapsulamiento
- Enumeradores (`enum`) para prioridad y estado
- Excepciones (`ArgumentException`, `InvalidOperationException`)
- Listas (`List<T>`) y LINQ (filtrado, búsqueda)
- Serialización JSON con Newtonsoft.Json y persistencia en archivos (`File.ReadAllText` / `File.WriteAllText`)
- Arquitectura por capas: Entidades → Servicio → Datos (Repositorio)
- Testing unitario con NUnit (patrón AAA, nomenclatura `Metodo_Escenario_ResultadoEsperado`)

> No hace falta usar herencia, clases abstractas ni interfaces en este ejercicio — hay una sola entidad (`Ticket`), así que forzar una jerarquía de clases sería una complejidad artificial. El foco de esta práctica es persistencia + testing, no diseño orientado a objetos avanzado.

---

### Persistencia

- El archivo (por ejemplo `tickets.json`) se guarda como una lista completa cada vez que cambia algo (siguiendo el patrón "leer todo → modificar en memoria → guardar todo" visto en la Unidad 5).
- Si el archivo no existe todavía (primera ejecución), el repositorio debe devolver una lista vacía, no romper.
- La ruta del archivo la decide `TicketService`, no quien lo use — el repositorio solo sabe leer y escribir en la ruta que le pasan por constructor.

### Tests unitarios

Escribir tests con NUnit que cubran, como mínimo:
- Creación de un ticket válido (queda en estado Abierto).
- Creación con título vacío o demasiado largo (debe lanzar excepción).
- Creación con descripción vacía (debe lanzar excepción).
- Transición de estado válida (Abierto → En Proceso → Resuelto → Cerrado).
- Al menos dos transiciones inválidas (por ejemplo, cerrar un ticket Abierto; o intentar modificar un ticket ya Cerrado).
- Búsqueda por id inexistente.
- Filtrado por estado y búsqueda por texto en el título.

Ejecutar `dotnet test` antes de dar por terminada la práctica y confirmar que todo pasa en verde.

---

## Preparación para sumar la Web API

Estas reglas no son "buenas prácticas" genéricas — son las que hacen que, en la próxima práctica de esta unidad, agregar un proyecto `webapi` sea directo:

1. **`TicketService` no debe imprimir nada por consola ni leer input.** Sus métodos reciben parámetros y devuelven objetos (`Ticket`, `List<Ticket>`) o lanzan excepciones. 
2. **Buscar por id que no existe devuelve `null`, no lanza excepción.** 
3. **Las reglas de negocio violadas lanzan excepciones tipadas** (`ArgumentException` para datos inválidos, `InvalidOperationException` para transiciones de estado inválidas). 
4. **Los métodos del Service se nombran pensando en el verbo HTTP que van a exponer**: `ObtenerTodos`, `ObtenerPorId`, `Crear`, `TomarTicket`, `ResolverTicket`, `CerrarTicket`, `ObtenerPorEstado`, `BuscarPorTitulo`.
5. **No hardcodear la ruta del archivo JSON con una ruta absoluta.** Usar una ruta relativa simple (`"tickets.json"`), tal como se hizo en la Unidad 5 — cuando se sume el proyecto de API, ese archivo va a vivir junto al nuevo proyecto sin cambios de código.

Si se respetan estos cinco puntos, sumar la API más adelante será sencillo

---

## Entrega

- Crear un repositorio privado en GitHub con el nombre `Prog1-Practica-HelpDesk-TuApellido` (reemplazá `TuApellido` por tu apellido real).
- Agregar a los profesores como colaboradores.
- Incluir un `.gitignore` para proyectos .NET.
- Commits descriptivos que reflejen el avance (armado de la solución, entidad y enums, repositorio, service, tests).

---

*Práctica | Unidad 6: Web Services y REST APIs | Programación 1 | UCSE*
