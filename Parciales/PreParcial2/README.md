\# Pre-Parcial 2 — Programación I | UCSE



\## Instrucciones generales

\- El examen debe resolverse en \*\*1 hora y media\*\*.

\- Se debe entregar en un repositorio privado de GitHub llamado \*\*`Prog1-2026-preparcial2-NombreApellido`\*\*.

\- La solución debe estar organizada en \*\*2 proyectos\*\* dentro de la misma solución:

&#x20; 1. Un proyecto de Biblioteca de Clases (`.NET Core` o `.NET Framework`) para la \*\*Lógica de Negocio\*\*.

&#x20; 2. Un proyecto de pruebas unitarias utilizando \*\*NUnit\*\* para la \*\*Calidad de Software\*\*.



\---



\## Readify — Sistema de Biblioteca Digital



\*Readify\* es una plataforma que permite a los usuarios acceder a libros digitales bajo dos modalidades: comprando el libro de forma permanente o alquilándolo por una cantidad de días. La empresa necesita un sistema básico que gestione su catálogo, registre las órdenes de los usuarios y permita realizar búsquedas y reportes financieros esenciales.



\---



\### 1. El Catálogo

Todos los recursos del catálogo tienen en común las siguientes propiedades: un \*\*Código ISBN\*\* (identificador único, texto), un \*\*Título\*\* y un \*\*Precio Base\*\* (valor decimal).



El catálogo se divide en dos tipos de accesos bien definidos:

\* \*\*Libros Comprados:\*\* Tienen como dato único el \*Formato\* de descarga (texto, ej: "PDF", "EPUB"). Su precio final se calcula sumando al \*Precio Base\* un \*\*10% de recargo fijo\*\* por gastos de mantenimiento de la plataforma.

\* \*\*Libros Alquilados:\*\* Tienen como dato único los \*Días de Alquiler\* (entero). Su precio final se calcula multiplicando el \*Precio Base\* por la cantidad de días solicitados, pero el sistema aplica un \*\*descuento fijo del 15%\*\* sobre ese total general como beneficio de lealtad.



\---



\### 2. Gestión de Órdenes

Cada vez que un usuario realiza una transacción, el sistema genera una orden. Cada objeto `Orden` debe contener de forma obligatoria:

\* Un \*\*ID único\*\* autogenerado en el constructor (puede ser un `Guid` o un entero autoincremental).

\* El \*\*Nombre del Usuario\*\* (texto) que realiza la operación.

\* El \*\*Ítem\*\* del catálogo que está adquiriendo (debe aceptar cualquier tipo de `Libro`).

\* Una propiedad de solo lectura o método que devuelva el \*\*Total a Pagar\*\* de la orden, delegando el cálculo matemático al objeto del libro correspondiente.



\---



\### 3. Servicio de Gestión (BibliotecaService)

Para centralizar las operaciones de la plataforma, se debe construir una clase llamada `BibliotecaService`. Esta clase administrará el historial de órdenes mediante una colección interna protegida (`List<Orden>`). Debe proveer los siguientes comportamientos:



\* \*\*`AgregarOrden(Orden orden)`\*\*: Añade una nueva orden al historial.

\* \*\*`BuscarOrdenesPorUsuario(string nombreUsuario)`\*\*: Busca y devuelve una lista con todas las órdenes que pertenezcan al usuario indicado (la búsqueda no debe discriminar mayúsculas de minúsculas).

\* \*\*`ObtenerTotalRecaudado()`\*\*: Devuelve la sumatoria total del dinero de todas las órdenes registradas en el sistema hasta el momento.



> \*\*Nota:\*\* No se permite el uso de sobrecarga de métodos (mismo nombre con distintos parámetros) en ninguna parte del servicio.



\---



\### 4. Reglas de Negocio (Validaciones)

Para asegurar la consistencia y la calidad de los datos, el sistema no debe permitir ingresos incoherentes. Se deben lanzar excepciones nativas (`ArgumentException` u otra adecuada) con mensajes claros y descriptivos si:

\* El \*Código ISBN\* se ingresa vacío o con espacios en blanco.

\* El \*Precio Base\* de cualquier libro es menor o igual a cero.

\* Los \*Días de Alquiler\* para un libro alquilado son menores o iguales a cero.



\---



\### 5. Calidad del Software (Pruebas Unitarias con NUnit)

El proyecto de pruebas debe utilizar el framework \*\*NUnit\*\* para garantizar que el núcleo del negocio funciona de manera aislada y robusta. Se deben programar, como mínimo, los siguientes casos de prueba independientes:



1\. \*\*`PrecioAlquiler\_ConDescuento`\*\*: Verificar que un libro de alquiler con un precio base de $100 por 5 días calcule y devuelva exactamente un precio final de $425 (aplicando el 15% de descuento).

2\. \*\*`BuscarOrdenesPorUsuario\_RetornaCorrectos`\*\*: Registrar al menos tres órdenes de usuarios distintos en el `BibliotecaService` y comprobar que al filtrar por un nombre, devuelva únicamente la cantidad de órdenes que le corresponden a ese usuario.

3\. \*\*`ObtenerTotalRecaudado\_SumaTodasLasOrdenes`\*\*: Cargar múltiples órdenes con diferentes tipos de libros en el servicio y verificar que el total recaudado sea la suma exacta de los montos de cada una.

