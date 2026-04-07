# Práctica: Control de Versiones con Git y GitHub

## Objetivo

Practicar la creación y gestión de un repositorio en GitHub aplicando el flujo de trabajo visto en clase, mientras se resuelve un ejercicio de Programación Orientada a Objetos.

---

## Contexto del ejercicio

Vas a modelar el sistema de gestión de una **veterinaria**. El sistema debe permitir registrar mascotas, asignarles un dueño y consultar información sobre ellas.

---

## Paso 1 — Crear y configurar el repositorio

1. Crear un repositorio privado en GitHub con el nombre `Prog1-Practica-Git-TuApellido` (reemplazá `TuApellido` por tu apellido real).
2. Tildar **"Add a README file"** al crearlo.
3. Agregar a los profesores como colaboradores (**Settings → Collaborators → Add people**):
   - `gonzaperez2312@gmail.com`
   - `maximilianolovera@gmail.com`
4. Clonar el repositorio en tu máquina.
5. Dentro de la carpeta clonada, crear un proyecto de consola en C# con Visual Studio.
6. Agregar un archivo `.gitignore` para proyectos .NET (podés usar el contenido del apartado `.gitignore` de la unidad).

Una vez que tengas el proyecto creado y el `.gitignore` en su lugar, realizá el primer commit y push:

```bash
git add .
git commit -m "Inicializar proyecto con .gitignore"
git push
```

---

## Paso 2 — Clase `Dueño`

Crear la clase `Dueño` con las siguientes propiedades:

- `Nombre` (string)
- `Telefono` (string)

La clase debe tener:
- Un constructor que reciba y asigne ambas propiedades.
- Un método `ObtenerInfo()` que retorne un string con el formato: `"Nombre: [nombre] | Teléfono: [telefono]"`.

Una vez implementada la clase, realizá el segundo commit y push:

```bash
git add .
git commit -m "Agregar clase Dueño con propiedades y método ObtenerInfo"
git push
```

---

## Paso 3 — Clase `Mascota`

Crear la clase `Mascota` con las siguientes propiedades:

- `Nombre` (string)
- `Especie` (string) — por ejemplo: "Perro", "Gato", "Conejo"
- `Edad` (int) — en años
- `Dueño` (de tipo `Dueño`)

La clase debe tener:
- Un constructor que reciba y asigne todas las propiedades.
- Un método `ObtenerInfo()` que retorne un string con el formato:
  `"[nombre] | [especie] | [edad] años | Dueño: [nombre del dueño]"`.

Una vez implementada la clase, realizá el tercer commit y push:

```bash
git add .
git commit -m "Agregar clase Mascota con propiedades y método ObtenerInfo"
git push
```

---

## Paso 4 — Clase `Veterinaria`

Crear la clase `Veterinaria` que administre una lista de mascotas registradas. Debe contener:

- Una lista privada de `Mascota`.
- Un constructor que inicialice la lista vacía.
- Método `RegistrarMascota(Mascota mascota)`: agrega una mascota a la lista.
- Método `MostrarTodas()`: imprime por consola la información de todas las mascotas registradas usando su método `ObtenerInfo()`. Si no hay mascotas, debe mostrar el mensaje `"No hay mascotas registradas."`.
- Método `BuscarPorEspecie(string especie)`: imprime por consola solo las mascotas cuya especie coincida con la indicada (sin distinguir mayúsculas/minúsculas). Si no hay coincidencias, debe mostrar `"No se encontraron mascotas de esa especie."`.

Una vez implementada la clase, realizá el cuarto commit y push:

```bash
git add .
git commit -m "Agregar clase Veterinaria con registro y búsqueda de mascotas"
git push
```

---

## Paso 5 — Programa principal

En `Program.cs`, escribir un programa que demuestre el funcionamiento del sistema:

1. Crear al menos **3 dueños** distintos.
2. Crear al menos **4 mascotas** de distintas especies y asignarlas a sus dueños.
3. Registrar todas las mascotas en una instancia de `Veterinaria`.
4. Llamar a `MostrarTodas()` para listar todas las mascotas.
5. Llamar a `BuscarPorEspecie()` al menos una vez para filtrar por una especie concreta.

Una vez implementado el programa principal, realizá el quinto commit y push:

```bash
git add .
git commit -m "Agregar programa principal con ejemplo de uso del sistema"
git push
```

---

## Criterios de evaluación

| Criterio | Detalle |
|---|---|
| Repositorio correctamente configurado | Privado, con colaboradores agregados |
| Commits | Mínimo 5 commits con mensajes descriptivos |
| Clase `Dueño` | Propiedades, constructor y método `ObtenerInfo` |
| Clase `Mascota` | Propiedades, constructor y método `ObtenerInfo` |
| Clase `Veterinaria` | Lista, registro, listado y búsqueda por especie |
| Programa principal | Uso correcto de todas las clases |

---

> Unidad 3 - Programación 1 | UCSE
