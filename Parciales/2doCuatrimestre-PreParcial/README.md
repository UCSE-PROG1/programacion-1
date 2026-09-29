# Parcial 3 — Programación I | UCSE

## Instrucciones generales

- El examen es **individual y presencial**, y se resuelve en **2 horas**.

- Se debe entregar en un repositorio privado de GitHub llamado **`Prog1-Parcial3-ApellidoNombre`**.

- **Las entregas realizadas fuera del horario establecido tendrán una penalización automática de -10 puntos** sobre la nota final, sin excepción.

- La solución debe estar organizada en **4 proyectos** dentro de la misma solución:

  1. Un proyecto de Biblioteca de Clases (`.NET Core`) para la **Lógica de Negocio** (`SalaEstudio.Logica`).
  2. Un proyecto **Web API** (ASP.NET Core) que exponga esa lógica (`SalaEstudio.Api`), con **Swagger habilitado**.
  3. Un proyecto de pruebas unitarias con **NUnit** (`SalaEstudio.Tests`).
  4. Una carpeta `Frontend/` con HTML, CSS y JavaScript (sin frameworks) que consuma la API.

  El foco del examen está en la **API** (proyectos 1, 2 y 3). El frontend (proyecto 4) es intencionalmente acotado — ver Punto 5.

- **Commits obligatorios**: el repositorio de GitHub **se crea al arrancar el examen** (momento 0), con el **`.gitignore` correspondiente al proyecto ya agregado desde el primer commit** (no se crea recién al terminar, y no se sube sin `.gitignore`). A partir de ahí, el trabajo se hace con **un mínimo de 8 commits**, con mensajes que describan lo que se hizo en cada uno, y **cada commit se sube con su propio `git push` en el momento en que se hace** (no se van acumulando commits locales para subirlos todos juntos al final). Se revisa la cantidad de commits, que el repositorio exista desde el inicio del examen, y que los **pushes** — no solo los commits — estén repartidos a lo largo de las 2 horas. Un repo creado sobre el final, o una sola tanda de pushes que sube los 8 commits de una, se considera incumplimiento igual que si hubiera un solo commit. No cumplir esto resta puntos (ver tabla de corrección).

- **Personalización obligatoria por DNI (Punto 0)**: antes de empezar a programar, cada alumno hace un cálculo a partir de su propio DNI y **anota el resultado en el `README.md` de su propio repositorio** (no en un archivo aparte, no solo en el código). Esos valores anotados son los que después tiene que usar como constantes de negocio en su solución (ver Punto 0 y Punto 2). Resolver la consigna con los valores genéricos del enunciado, sin aplicar el propio cálculo, da resultados incorrectos aunque el código esté bien escrito.

---

## 0. Personalización — completar antes de empezar

Este es el primer paso del examen, antes de escribir una sola línea de código.

**Paso 1 — Calculá tus dos valores personales.** Tomá los **últimos 2 dígitos de tu DNI** como número `NN` (por ejemplo, si tu DNI es 41.234.**567**, `NN = 67`):

```
anticipacionMinimaHoras = (NN % 3) + 1        →  da 1, 2 o 3
duracionMaximaMinutos   = 60 + (NN % 4) * 30   →  da 60, 90, 120 o 150
```

**Paso 2 — Anotalos en el `README.md` de tu repositorio**, en una sección al principio que diga, por ejemplo:

```
## Personalización
DNI: 41234567 (NN = 67)
anticipacionMinimaHoras = 3
duracionMaximaMinutos = 150
```

**Paso 3 — Usá esos dos valores como constantes de negocio en tu código** (ver las reglas del Punto 2), de la forma que prefieras: como constantes en el Service, en un archivo de configuración, o como te resulte más cómodo — no hay una ubicación obligatoria dentro del código, la única obligación es que estén declarados en el README (Paso 2) y que el comportamiento de tu API realmente los respete.

**La corrección valida estos números contra tu DNI real**: si los valores del README no coinciden con tu DNI, o el código no se comporta de acuerdo a esos valores, se considera que la regla de negocio no está resuelta, aunque el código compile y funcione con otros valores.

---

## 1. Contexto

La biblioteca de la UCSE tiene salas de estudio grupal que los alumnos pueden reservar por franja horaria. Hoy la reserva se coordina por WhatsApp con el bibliotecario, y quieren reemplazarlo por un sistema propio: una API que gestione las reservas sobre un conjunto fijo de salas ya existentes, con una pantalla simple de solo consulta.

**Las salas ya vienen cargadas** (no se piden endpoints para crear/editar salas): al arrancar la API por primera vez, si `salas.json` no existe, se debe crear con estos 3 registros:

```json
[
  { "Id": 1, "Nombre": "Sala A", "Capacidad": 4 },
  { "Id": 2, "Nombre": "Sala B", "Capacidad": 6 },
  { "Id": 3, "Nombre": "Sala C", "Capacidad": 10 }
]
```

Todo el trabajo de este examen es sobre **Reservas**.

---

## 2. Entidad y reglas de negocio

### `Reserva`
- `Id` (autonumérico), `SalaId` (int), `Dni` (int), `NombreAlumno` (string), `FechaHoraInicio` (DateTime), `FechaHoraFin` (DateTime).

### Reglas de negocio (obligatorias, sin excepción)

1. `FechaHoraFin` debe ser posterior a `FechaHoraInicio`.
2. Una reserva no puede durar más de `duracionMaximaMinutos` (tu valor personalizado del Punto 0).
3. Una reserva debe hacerse con al menos `anticipacionMinimaHoras` (tu valor personalizado) de anticipación respecto del momento en que se registra.
4. **No se puede reservar una sala si ya existe otra reserva para esa misma sala que se superponga en el tiempo** — dos reservas se superponen si el inicio de una es anterior al fin de la otra y viceversa (caso borde: una reserva que termina justo cuando otra empieza **no** es superposición).
5. No se puede reservar una sala inexistente (`SalaId` fuera de las 3 precargadas).
6. No se puede cancelar una reserva que ya empezó (`FechaHoraInicio` en el pasado respecto del momento de la cancelación).

Estas reglas viven en el **Service**, no en el Controller ni en el Repositorio.

---

## 3. Persistencia (Unidad 5)

- Reservas se persisten en `reservas.json`, con ruta relativa. Salas en `salas.json` (ver Punto 1, solo lectura).
- Debe existir un `Repository` por entidad. El Service no debe usar `File.` ni `JsonConvert.` directamente en ningún punto.
- Si `reservas.json` no existe todavía, arrancar con lista vacía (no explotar). Si `salas.json` no existe, crearlo con el contenido fijo del Punto 1.
- Siempre leer el archivo completo antes de agregar/modificar y volver a guardar la lista completa.

---

## 4. Web API REST (Unidad 6)

| Método | Endpoint | Qué resuelve |
|---|---|---|
| GET | `/api/salas` | Lista las 3 salas precargadas. |
| GET | `/api/salas/{id}/disponibilidad?fecha=YYYY-MM-DD` | Lista las franjas ya reservadas de esa sala para el día indicado. |
| GET | `/api/reservas` | Lista todas las reservas. |
| POST | `/api/reservas` | Registra una reserva, aplicando **todas** las reglas del Punto 2. |
| DELETE | `/api/reservas/{id}` | Cancela una reserva (regla 6 del Punto 2). |

- Los Controllers **no contienen lógica de negocio**.
- Uso de **DTOs** (`Request`/`Response`) para `Reserva`, con mapeo Modelo ↔ DTO en el Controller.
- **Data Annotations** en el DTO de Request para validaciones de formato; las reglas de negocio del Punto 2 se resuelven en el Service (no con Data Annotations).
- Códigos de estado: `201` en alta exitosa, `400` con el **motivo en el body** cuando se viola una regla de negocio, `404` para sala/reserva inexistente.
- Swagger habilitado en `/swagger`.

---

## 5. Frontend (Unidad 7) — alcance reducido, solo lectura

No se pide formulario de alta ni de cancelación. Alcanza con:

1. `Frontend/index.html` + `estilos.css` + `app.js` que, al cargar, hagan `fetch` a `GET /api/salas` y `GET /api/reservas` y muestren ambas listas en pantalla (nombre de sala, y para cada reserva: sala, alumno, horario).
2. Si alguna de las dos peticiones falla, mostrar un mensaje de error en pantalla (no dejar la página en blanco ni solo un error en consola).

No se evalúa diseño visual. Se evalúa que los datos vengan realmente de la API vía `fetch`, no hardcodeados en el JS.

---

## 6. Tests NUnit

Mínimo **2 tests independientes** sobre la lógica de negocio:

1. Rechazar una reserva que se superpone con una ya existente para la misma sala.
2. Aceptar una reserva que termina exactamente cuando otra empieza en la misma sala (caso borde de la regla 4 del Punto 2).

---

## 7. Estructura de proyecto esperada

```
Prog1-Parcial3-ApellidoNombre/
├── SalaEstudio.sln
├── SalaEstudio.Logica/
│   ├── SalaEstudio.Logica.csproj
│   ├── Sala.cs
│   ├── Reserva.cs
│   ├── ReservaService.cs
│   └── Data/
│       ├── SalaRepository.cs
│       └── ReservaRepository.cs
├── SalaEstudio.Api/
│   ├── SalaEstudio.Api.csproj
│   ├── Controllers/
│   │   ├── SalasController.cs
│   │   └── ReservasController.cs
│   ├── DTOs/
│   └── Program.cs
├── SalaEstudio.Tests/
│   ├── SalaEstudio.Tests.csproj
│   └── ReservaServiceTests.cs
└── Frontend/
    ├── index.html
    ├── estilos.css
    └── app.js
```

---

## 8. Entregables

- Repositorio con los 4 proyectos, `.gitignore` desde el primer commit, y **al menos 8 commits** (cada uno con su propio push) distribuidos en las 2 horas.
- README con instrucciones para levantar la API y abrir el frontend, más la sección "Personalización" (Punto 0).
- Suite de tests NUnit en verde.

---

> Parcial 3 — Programación I | UCSE
