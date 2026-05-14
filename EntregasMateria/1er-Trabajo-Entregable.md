# 1er Trabajo Entregable

**Materia:** Programación 1 | UCSE
**Modalidad:** Individual

---

## Enunciado

Una cafetería local quiere reemplazar su sistema de anotaciones en papel por un sistema digital. El negocio vende dos tipos de productos: bebidas y comidas. Todos los productos tienen nombre y precio base, pero el precio final varía según el tipo: las bebidas tienen un volumen en mililitros y aplican un IVA reducido del 10,5%, mientras que las comidas indican si son veganas o no, aplicando IVA reducido si lo son y completo (21%) si no lo son. Las bebidas no pueden tener un volumen menor a 100ml ni mayor a 1000ml, y ningún producto puede tener nombre vacío ni precio negativo o cero. Cuando un cliente hace un pedido, se van agregando productos con una cantidad determinada; un pedido puede pasar por distintos estados (pendiente, en preparación, listo, entregado o cancelado) y no se pueden agregar productos si el pedido ya no está pendiente. El sistema debe poder listar el menú completo, filtrar solo bebidas o solo comidas, buscar productos por nombre, calcular el total de un pedido, registrar pedidos y consultar la recaudación acumulada considerando únicamente los pedidos entregados. Si se intenta agregar un producto con un nombre ya existente en el menú, se debe informar el error. Lo mismo si se busca un producto que no existe.

---

## Notas

Para resolver este trabajo se deben aplicar los siguientes conceptos vistos en las unidades 2, 3 y 4. No se indica en qué parte del sistema corresponde usar cada uno — eso forma parte del análisis y diseño que debe realizar el alumno.

- Clase abstracta
- Herencia
- Interfaz
- Polimorfismo
- Encapsulamiento (propiedades con acceso controlado)
- Enumeradores (`enum`)
- Excepciones (`throw`, `ArgumentException`, `InvalidOperationException`, `ArgumentNullException`)
- Listas (`List<T>`)
- LINQ (filtrado, proyección, agregación)
- Arquitectura por capas (separación entre lógica de negocio y el resto)
- Testing unitario con NUnit (patrón AAA, nomenclatura, cobertura de casos)
- Control de versiones con Git y GitHub (commits descriptivos, `.gitignore`, repositorio privado)

---

## Requisitos técnicos

### Proyectos

La solución debe tener exactamente dos proyectos: uno de lógica (`classlib`) y uno de tests (`nunit`). No se debe agregar un proyecto de consola.

```bash
dotnet new sln -n GestionCafeteria
dotnet new classlib -n GestionCafeteria
dotnet new nunit -n GestionCafeteriaTest
dotnet sln GestionCafeteria.sln add GestionCafeteria/GestionCafeteria.csproj
dotnet sln GestionCafeteria.sln add GestionCafeteriaTest/GestionCafeteriaTest.csproj
dotnet add GestionCafeteriaTest/GestionCafeteriaTest.csproj reference GestionCafeteria/GestionCafeteria.csproj
```

### Tests unitarios

Se deben escribir tests unitarios con NUnit que verifiquen el comportamiento de las clases implementadas. Los tests deben aplicar el patrón Arrange-Act-Assert, seguir la nomenclatura `Metodo_Escenario_ResultadoEsperado`, y cubrir casos de uso normales, casos borde y situaciones de error (excepciones). Ejecutar `dotnet test` antes de entregar y confirmar que todos los tests pasan.

### Control de versiones

- Crear el repositorio en GitHub como **privado**, con el nombre `Prog1-TP-Integrador-[Apellido]`.
- Agregar como colaboradores: `gonzaperez2312@gmail.com`, `mateolerda03@gmail.com`.
- Incluir un `.gitignore` adecuado para proyectos .NET.
- Realizar **al menos 5 commits** con mensajes descriptivos en modo imperativo que reflejen la evolución del trabajo.

---

## Entrega

Enviar el link del repositorio de GitHub al docente por el medio indicado en clase, antes de la fecha límite.

---

*Trabajo Práctico Integrador | Programación 1 | UCSE*
