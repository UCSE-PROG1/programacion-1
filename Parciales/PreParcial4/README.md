# Pre-Parcial 4 — Programación I | UCSE



## Instrucciones generales

- El examen debe resolverse en **1 hora y media**.

- Se debe entregar en un repositorio privado de GitHub llamado **`Prog1-2026-preparcial4-NombreApellido`**.

- La solución debe estar organizada en **2 proyectos** dentro de la misma solución:

  1. Un proyecto de Biblioteca de Clases (`.NET Core`) para la **Lógica de Negocio**.

  2. Un proyecto de pruebas unitarias utilizando **NUnit** para la **Calidad de Software**.



---



## FiguritasHub — Sistema de Gestión de Álbum del Mundial



*FiguritasHub* es una plataforma que permite a los coleccionistas registrar sus álbumes de figuritas del Mundial, identificar figuritas repetidas y gestionar canjes entre coleccionistas. Para ello, el sistema debe modelar distintos tipos de figuritas, administrar el contenido de cada álbum y centralizar las operaciones de intercambio.



---



### 1. Las Figuritas

Todas las figuritas del sistema tienen en común las siguientes propiedades: un **Número** (entero, identificador único de la figurita dentro del álbum), el **Nombre del Jugador** (texto) y el **País** (texto).

El sistema distingue dos tipos de figuritas, cada una con su propia descripción de categoría:

* **FiguritaComun:** Tiene como dato único su **Rareza** (entero del 1 al 5, donde 1 es la más común y 5 la más difícil de conseguir). Al consultar su categoría, devuelve el texto `"Común (Rareza: X)"`, donde *X* es su nivel de rareza.

* **FiguritaBrillante:** Tiene como dato único si **Es Edición Limitada** (booleano). Al consultar su categoría, devuelve el texto `"Brillante (Edición Limitada)"` si lo es, o simplemente `"Brillante"` en caso contrario.



---



### 2. El Álbum

Cada coleccionista tiene un álbum. La clase `Album` debe contener:

* El **Nombre del Coleccionista** (texto).

* Una colección interna protegida (`List<Figurita>`) que almacena todas las figuritas del álbum, incluyendo copias repetidas.

* Dos versiones sobrecargadas del método **`AgregarFigurita`** *(sobrecarga obligatoria)*:
  * `AgregarFigurita(Figurita figurita)` — agrega una única copia de la figurita al álbum.
  * `AgregarFigurita(Figurita figurita, int cantidad)` — agrega la cantidad indicada de copias de esa figurita al álbum.

* Dos versiones sobrecargadas del método **`TieneRepetida`** *(sobrecarga obligatoria)*:
  * `TieneRepetida(Figurita figurita)` — devuelve `true` si esa figurita aparece más de una vez en el álbum (la comparación se realiza por **Número** de figurita).
  * `TieneRepetida(int numeroDeFigurita)` — devuelve `true` si existe alguna figurita con ese número que aparezca más de una vez.

* **`ObtenerRepetidas()`** — devuelve una lista con las figuritas (sin duplicar) que tienen más de una copia en el álbum.



---



### 3. Servicio de Canje (GestorDeCanjeService)

Para centralizar las operaciones de intercambio, se debe construir una clase llamada `GestorDeCanjeService`. Esta clase administrará los álbumes registrados mediante una colección interna protegida (`List<Album>`). Debe proveer los siguientes comportamientos:

* Debe permitir **registrar un álbum** en el servicio.

* Dos versiones sobrecargadas del método **`Canjear`** *(sobrecarga obligatoria)*:
  * `Canjear(Album origen, Album destino, int numeroDeFigurita)` — busca en el álbum origen la figurita con ese número y transfiere una copia al álbum destino.
  * `Canjear(Album origen, Album destino, Figurita figurita)` — transfiere directamente esa figurita del álbum origen al álbum destino.

  En ambos casos, el canje consiste en **remover una copia** de la figurita del álbum origen y **agregar una copia** al álbum destino.

* **`ObtenerAlbumConMasRepetidas()`** — devuelve el álbum registrado que tenga la mayor cantidad total de copias repetidas (es decir, la suma de copias sobrantes por sobre la primera de cada figurita). Si no hay álbumes registrados, debe devolver un valor que indique la ausencia de resultado.



---



### 4. Reglas de Negocio (Validaciones)

Para asegurar la consistencia de los datos, el sistema debe lanzar excepciones nativas (`ArgumentException` u otra adecuada) con mensajes claros y descriptivos si:

* El **Número** de figurita es menor o igual a cero.

* El **Nombre del Jugador** se ingresa vacío o con espacios en blanco.

* La **Rareza** de una `FiguritaComun` es menor que 1 o mayor que 5.

* La **cantidad** en `AgregarFigurita(figurita, cantidad)` es menor o igual a cero.

* Se intenta realizar un canje donde el álbum **origen no tiene esa figurita como repetida** (solo posee una copia o ninguna).

* Se intenta realizar un canje donde el álbum **destino ya posee esa figurita** (sin importar si está repetida o no en el destino).



---



### 5. Calidad del Software (Pruebas Unitarias con NUnit)

El proyecto de pruebas debe utilizar el framework **NUnit** para garantizar que el núcleo del negocio funciona de manera aislada y robusta. Se deben programar, como mínimo, los siguientes **cinco** casos de prueba independientes:

1. Verificar que al usar `AgregarFigurita(figurita, 3)`, el álbum registra exactamente 3 copias de esa figurita y que `ObtenerRepetidas()` la incluye en su resultado.

2. Verificar que `TieneRepetida(int numeroDeFigurita)` devuelve `true` cuando el álbum tiene más de una copia de una figurita con ese número, y `false` cuando solo tiene una.

3. Verificar que al ejecutar un canje válido, el álbum origen pierde una copia de la figurita y el álbum destino la recibe correctamente.

4. Verificar que `Canjear` lanza una excepción cuando el álbum destino ya posee la figurita indicada.

5. Verificar que `Canjear` lanza una excepción cuando el álbum origen no tiene esa figurita como repetida.
