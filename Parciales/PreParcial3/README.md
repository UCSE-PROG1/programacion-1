# Pre-Parcial 3 — Programación I | UCSE



## Instrucciones generales

- El examen debe resolverse en **2 horas**.

- Se debe entregar en un repositorio privado de GitHub llamado **`Prog1-2026-preparcial3-NombreApellido`**.

- La solución debe estar organizada en **2 proyectos** dentro de la misma solución:

&#x20; 1. Un proyecto de Biblioteca de Clases (`.NET Core`) para la **Lógica de Negocio**.

&#x20; 2. Un proyecto de pruebas unitarias utilizando **NUnit** para la **Calidad de Software**.



---



## ParkHub — Sistema de Gestión de Estacionamiento



*ParkHub* es una empresa que administra playas de estacionamiento en distintos puntos de la ciudad. Para optimizar su operación, necesita un sistema que registre el ingreso de vehículos, calcule automáticamente el costo según el tipo de vehículo y genere reportes de facturación para el cierre de cada turno.



---



### 1. Los Vehículos

Todos los vehículos del sistema tienen en común las siguientes propiedades: una **Patente** (identificador único, texto), el **Nombre del Propietario** y un **Precio Base Por Hora** (valor decimal).



El sistema distingue dos tipos de vehículos con reglas de cobro propias:

* **Auto:** Tiene como dato único si **Es Eléctrico** (booleano). Su cobro se calcula multiplicando el *Precio Base Por Hora* por la cantidad de horas. Sin embargo, si el auto es eléctrico, se aplica un **descuento del 10%** sobre el total como beneficio ecológico.

* **Camioneta:** Tiene como dato único un **Número de Carga** (entero, identificador interno de la mercadería transportada). Su cobro se calcula multiplicando el *Precio Base Por Hora* por la cantidad de horas, y luego se aplica un **recargo fijo del 30%** sobre ese total por el espacio adicional que ocupa en la playa.



---



### 2. Registros de Ingreso

Cada vez que un vehículo ingresa a la playa, el sistema genera un Registro. Cada objeto `Registro` debe contener de forma obligatoria:

* Un **ID único** autogenerado en el constructor (puede ser un `Guid` o un entero autoincremental).

* El **Vehículo** que ingresó (debe aceptar cualquier tipo de `Vehiculo`).

* La **Cantidad de Horas** (entero) que el vehículo permanecerá en la playa.

* El sistema debe poder obtener el **Total a Cobrar** de cada registro, calculado en función del vehículo y las horas registradas.



---



### 3. Servicio de Gestión (EstacionamientoService)

Para centralizar las operaciones, se debe construir una clase llamada `EstacionamientoService`. Esta clase administrará el historial de registros mediante una colección interna protegida (`List<Registro>`). Debe proveer los siguientes comportamientos:

* Debe permitir **agregar un nuevo registro** al historial.

* Dado el **nombre de un propietario**, debe devolver una lista con todos los registros asociados a esa persona. La búsqueda no debe discriminar mayúsculas de minúsculas.

* Debe poder calcular y devolver la **sumatoria total del dinero** de todos los registros almacenados hasta el momento.

* Debe poder identificar y devolver el **registro con el mayor importe a cobrar** entre todos los almacenados. Si no hay registros, debe devolver un valor que indique la ausencia de resultado.



> **Nota:** No se permite el uso de sobrecarga de métodos (mismo nombre con distintos parámetros) en ninguna parte del servicio.



---



### 4. Reglas de Negocio (Validaciones)

Para asegurar la consistencia de los datos, el sistema debe lanzar excepciones nativas (`ArgumentException` u otra adecuada) con mensajes claros y descriptivos si:

* La **Patente** del vehículo se ingresa vacía o con espacios en blanco.

* El **Precio Base Por Hora** de cualquier vehículo es menor o igual a cero.

* La **Cantidad de Horas** del Registro es menor o igual a cero.



---



### 5. Calidad del Software (Pruebas Unitarias con NUnit)

El proyecto de pruebas debe utilizar el framework **NUnit** para garantizar que el núcleo del negocio funciona de manera aislada y robusta. Se deben programar, como mínimo, los siguientes **cuatro** casos de prueba independientes:



1. Verificar que un `Auto` eléctrico con un precio base de $200 por hora, registrado por 3 horas, calcule y devuelva exactamente un costo de **$540** (aplicando el 10% de descuento).

2. Verificar que una `Camioneta` con un precio base de $100 por hora, registrada por 4 horas, calcule y devuelva exactamente un costo de **$520** (aplicando el 30% de recargo).

3. Registrar al menos tres registros de propietarios distintos en el `EstacionamientoService` y comprobar que al filtrar por un nombre, se devuelvan únicamente los registros que le corresponden a ese propietario.

4. Cargar múltiples registros con distintos tipos de vehículos en el servicio y verificar que el total recaudado sea la **suma exacta** de los importes de cada uno.
