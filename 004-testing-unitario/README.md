# Unidad 4: Testing Unitario con NUnit

## Tabla de Contenidos

1. [¿Qué es Testing de Software?](#1-qué-es-testing-de-software)
2. [Testing Unitario](#2-testing-unitario)
3. [Herramientas de Testing en C#](#3-herramientas-de-testing-en-c)
4. [Estructura de un Proyecto de Testing](#4-estructura-de-un-proyecto-de-testing)
5. [Patrón Arrange-Act-Assert (AAA)](#5-patrón-arrange-act-assert-aaa)
6. [Primer Test Unitario](#6-primer-test-unitario)
7. [Métodos de Assert en NUnit](#7-métodos-de-assert-en-nunit)
8. [Pruebas con Excepciones](#8-pruebas-con-excepciones)
9. [Nomenclatura de Tests](#9-nomenclatura-de-tests)
10. [Testing con POO y Herencia](#10-testing-con-poo-y-herencia)
11. [Cobertura de Tests](#11-cobertura-de-tests)
12. [Ejecutar los Tests](#12-ejecutar-los-tests)
13. [Buenas Prácticas en Testing](#13-buenas-prácticas-en-testing)

---

## 1. ¿Qué es Testing de Software?

### Teoría

El **testing de software** es el proceso de evaluar un sistema o sus componentes con el objetivo de verificar que funciona correctamente y detectar posibles defectos antes de que lleguen a los usuarios finales.

Encontrar y corregir un bug tiene un costo que crece exponencialmente cuanto más tarde se detecta:

```
Costo de corrección

  Alto  │                                         ████
        │                                    ████
        │                               ████
        │                          ████
  Bajo  │  ████  ████  ████
        └────────────────────────────────────────────>
          Diseño  Código  Testing  Producción  Post-lanzamiento
```

Un bug detectado durante el desarrollo cuesta unas pocas horas de trabajo. El mismo bug detectado en producción puede costar días de trabajo, pérdida de datos, clientes insatisfechos y daño reputacional.

#### Tipos de testing

| Tipo | Descripción | Alcance |
|---|---|---|
| **Unitario** | Prueba componentes individuales en aislamiento | Una clase o método |
| **Integración** | Prueba cómo interactúan varios componentes entre sí | Varios módulos |
| **Sistema** | Prueba el sistema completo como un todo | Aplicación entera |
| **Aceptación** | Verifica que cumple los requisitos del cliente | Flujos completos |
| **Performance** | Evalúa velocidad, carga y escalabilidad | Sistema bajo estrés |

En esta materia nos enfocamos en el **testing unitario**, que es la base de todo lo demás.

---

## 2. Testing Unitario

### Teoría

Un **test unitario** es una prueba automatizada que verifica el comportamiento de una unidad pequeña y aislada de código, generalmente un método de una clase.

La palabra clave es **aislada**: el test debe probar un único comportamiento sin depender de factores externos como bases de datos, archivos, la red o el reloj del sistema.

#### ¿Por qué automatizar los tests?

Sin tests automatizados, la única forma de verificar que el código funciona es ejecutar la aplicación manualmente y probar a mano. Esto es:
- **Lento**: cada cambio requiere repetir toda la prueba manual.
- **Poco confiable**: es fácil olvidarse de probar algún caso.
- **No reproducible**: otra persona no puede repetir exactamente el mismo proceso.

Con tests automatizados podés ejecutar cientos de verificaciones en segundos, cada vez que realizás un cambio.

#### Beneficios principales

- **Detección temprana de errores**: los bugs se detectan inmediatamente cuando se introduce un cambio que rompe algo.
- **Facilita el mantenimiento**: podés refactorizar el código con confianza, sabiendo que los tests te avisarán si algo se rompe.
- **Aumenta la confianza**: permite hacer cambios sin el miedo de "¿habré roto algo sin darme cuenta?".
- **Documenta el comportamiento**: los tests describen exactamente qué debe hacer cada método, funcionando como documentación viva y ejecutable.

---

## 3. Herramientas de Testing en C#

### Teoría

En el ecosistema .NET existen tres frameworks de testing principales:

| Framework | Descripción |
|---|---|
| **NUnit** | Gratuito, de código abierto y multiplataforma. Inspirado en JUnit (Java). El más utilizado en la industria con .NET. |
| **MSTest** | El framework de Microsoft, integrado directamente en Visual Studio. |
| **xUnit** | Gratuito, de código abierto. Soporta ejecución de tests en paralelo. Muy popular en proyectos modernos. |

**En esta materia utilizamos NUnit.**

### Ejemplo: Crear un proyecto de tests con NUnit desde la terminal

```bash
# Crear un proyecto de tipo "Class Library" para la lógica de negocio
dotnet new classlib -n MiServicio

# Crear un proyecto de tests con NUnit
dotnet new nunit -n MiServicioTest

# Crear una solución que agrupe ambos proyectos
dotnet new sln -n MiSolucion
dotnet sln MiSolucion.sln add MiServicio/MiServicio.csproj
dotnet sln MiSolucion.sln add MiServicioTest/MiServicioTest.csproj

# Agregar la referencia del proyecto de tests al proyecto de lógica
dotnet add MiServicioTest/MiServicioTest.csproj reference MiServicio/MiServicio.csproj

# Ejecutar los tests
dotnet test
```

---

## 4. Estructura de un Proyecto de Testing

### Teoría

La estructura recomendada es una solución con dos proyectos separados:

- **Proyecto de servicio** (`ClassLibrary`): contiene toda la lógica de negocio — las clases que queremos probar. No tiene interfaz de usuario.
- **Proyecto de tests** (`NUnit Test Project`): contiene las clases de test. Referencia al proyecto de servicio para poder usar sus clases.

Esta separación es importante porque el código de producción no debe depender del código de tests. Los tests son los que dependen del código de producción, no al revés.

### Ejemplo: Estructura de archivos de una solución

```
MiSolucion/
│
├── MiSolucion.sln                          ← Archivo de solución (agrupa proyectos)
│
├── MiServicio/                             ← Proyecto Class Library (lógica de negocio)
│   ├── MiServicio.csproj
│   ├── Calculadora.cs
│   └── Persona.cs
│
└── MiServicioTest/                         ← Proyecto NUnit Test (tests)
    ├── MiServicioTest.csproj               ← Tiene referencia a MiServicio
    ├── CalculadoraTest.cs
    └── PersonaTest.cs
```

El archivo `.csproj` del proyecto de tests debe tener la referencia al proyecto de lógica:

```xml
<!-- MiServicioTest.csproj -->
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net8.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="NUnit" Version="4.x.x" />
    <PackageReference Include="NUnit3TestAdapter" Version="4.x.x" />
    <PackageReference Include="Microsoft.NET.Test.Sdk" Version="17.x.x" />
  </ItemGroup>

  <!-- Referencia al proyecto de lógica de negocio -->
  <ItemGroup>
    <ProjectReference Include="..\MiServicio\MiServicio.csproj" />
  </ItemGroup>
</Project>
```

---

## 5. Patrón Arrange-Act-Assert (AAA)

### Teoría

El patrón **AAA** es la estructura estándar para escribir tests unitarios. Divide cada test en tres secciones claramente diferenciadas:

- **Arrange (Preparar)**: se configuran todas las condiciones iniciales necesarias para la prueba. Se crean los objetos, se definen los valores de entrada y se prepara todo lo que el método necesita para ejecutarse.
- **Act (Actuar)**: se ejecuta exactamente el método que se quiere probar. Esta sección idealmente tiene una sola línea de código.
- **Assert (Verificar)**: se comprueba que el resultado obtenido coincide con el resultado esperado. Si la verificación falla, el test falla.

Respetar esta estructura hace que los tests sean fáciles de leer y mantener, ya que cualquier desarrollador puede identificar rápidamente qué se está preparando, qué se está probando y qué se está verificando.

### Ejemplo: Test con las tres secciones claramente comentadas

```csharp
[Test]
public void Sumar_DosNumerosPositivos_RetornaLaSuma()
{
    // Arrange: preparar las condiciones iniciales
    var calculadora = new Calculadora();
    int primerNumero = 10;
    int segundoNumero = 5;
    int resultadoEsperado = 15;

    // Act: ejecutar el método que se está probando
    int resultadoObtenido = calculadora.Sumar(primerNumero, segundoNumero);

    // Assert: verificar que el resultado es el esperado
    Assert.That(resultadoObtenido, Is.EqualTo(resultadoEsperado));
}
```

---

## 6. Primer Test Unitario

### Teoría

Para que NUnit reconozca las clases y métodos de test, se utilizan **atributos** especiales:

- **`[TestFixture]`**: marca una clase como contenedora de tests. NUnit buscará tests dentro de ella.
- **`[Test]`**: marca un método como un test individual que debe ser ejecutado.

El método de verificación más común es `Assert.That(valorReal, Is.EqualTo(valorEsperado))`. El primer argumento es el valor que obtuvo el código, y el segundo es el valor que esperábamos obtener.

### Ejemplo: Clase Calculadora y sus tests

```csharp
// Archivo: MiServicio/Calculadora.cs
public class Calculadora
{
    public int Sumar(int a, int b)
    {
        return a + b;
    }

    public int Restar(int a, int b)
    {
        return a - b;
    }

    public double Dividir(double a, double b)
    {
        if (b == 0)
            throw new DivideByZeroException("No se puede dividir por cero.");

        return a / b;
    }

    public int Multiplicar(int a, int b)
    {
        return a * b;
    }
}
```

```csharp
// Archivo: MiServicioTest/CalculadoraTest.cs
using NUnit.Framework;

[TestFixture]
public class CalculadoraTest
{
    [Test]
    public void Sumar_DosNumerosPositivos_RetornaLaSuma()
    {
        // Arrange
        var calculadora = new Calculadora();

        // Act
        int resultado = calculadora.Sumar(10, 5);

        // Assert
        Assert.That(resultado, Is.EqualTo(15));
    }

    [Test]
    public void Restar_NumeroPequenioDeNumeroGrande_RetornaDiferencia()
    {
        // Arrange
        var calculadora = new Calculadora();

        // Act
        int resultado = calculadora.Restar(10, 3);

        // Assert
        Assert.That(resultado, Is.EqualTo(7));
    }

    [Test]
    public void Multiplicar_DosNumeros_RetornaProducto()
    {
        // Arrange
        var calculadora = new Calculadora();

        // Act
        int resultado = calculadora.Multiplicar(4, 5);

        // Assert
        Assert.That(resultado, Is.EqualTo(20));
    }
}
```

---

## 7. Métodos de Assert en NUnit

### Teoría

NUnit ofrece una amplia variedad de métodos de verificación a través de la sintaxis `Assert.That(valor, restriccion)`. Las **restricciones** (constraints) se construyen con la clase `Is` y describen la condición que debe cumplir el valor real para que el test pase.

| Restricción | Descripción |
|---|---|
| `Is.EqualTo(x)` | El valor debe ser igual a `x` |
| `Is.Not.EqualTo(x)` | El valor no debe ser igual a `x` |
| `Is.True` | El valor booleano debe ser `true` |
| `Is.False` | El valor booleano debe ser `false` |
| `Is.Null` | El valor debe ser `null` |
| `Is.Not.Null` | El valor no debe ser `null` |
| `Is.GreaterThan(x)` | El valor debe ser mayor que `x` |
| `Is.LessThan(x)` | El valor debe ser menor que `x` |
| `Is.GreaterThanOrEqualTo(x)` | El valor debe ser mayor o igual que `x` |
| `Is.InstanceOf<T>()` | El objeto debe ser una instancia del tipo `T` |
| `Is.Empty` | La colección o string debe estar vacío |
| `Is.Not.Empty` | La colección o string no debe estar vacío |

### Ejemplo: Tests que demuestran cada tipo de Assert

```csharp
[TestFixture]
public class EjemplosAssertTest
{
    // Assert.That con Is.EqualTo
    [Test]
    public void EqualTo_SumaCorrecta_Pasa()
    {
        var calc = new Calculadora();
        Assert.That(calc.Sumar(3, 4), Is.EqualTo(7));
    }

    // Assert.That con Is.Not.EqualTo
    [Test]
    public void NotEqualTo_SumaIncorrecta_NoEsIgual()
    {
        var calc = new Calculadora();
        Assert.That(calc.Sumar(3, 4), Is.Not.EqualTo(10));
    }

    // Assert.That con Is.True
    [Test]
    public void IsTrue_MayorDeEdad_RetornaTrue()
    {
        var persona = new Persona("Laura", 25);
        Assert.That(persona.EsMayorDeEdad(), Is.True);
    }

    // Assert.That con Is.False
    [Test]
    public void IsFalse_Menor_RetornaFalse()
    {
        var persona = new Persona("Juan", 15);
        Assert.That(persona.EsMayorDeEdad(), Is.False);
    }

    // Assert.That con Is.Null
    [Test]
    public void IsNull_PersonaSinNombre_NombreEsNull()
    {
        var persona = new Persona(null, 20);
        Assert.That(persona.Nombre, Is.Null);
    }

    // Assert.That con Is.Not.Null
    [Test]
    public void IsNotNull_PersonaCreada_ObjetoNoEsNull()
    {
        var persona = new Persona("Carlos", 30);
        Assert.That(persona, Is.Not.Null);
    }

    // Assert.That con Is.GreaterThan
    [Test]
    public void GreaterThan_SueldoConAumento_EsMayorAlOriginal()
    {
        var empleado = new Empleado("Ana", 50000);
        empleado.AplicarAumento(10);
        Assert.That(empleado.Sueldo, Is.GreaterThan(50000));
    }

    // Assert.That con Is.InstanceOf
    [Test]
    public void InstanceOf_EmpleadoEsPersona_EsInstanciaDeTipo()
    {
        var empleado = new Empleado("Luis", 40000);
        Assert.That(empleado, Is.InstanceOf<Persona>());
    }

    // Assert.That con Is.Empty y Is.Not.Empty
    [Test]
    public void NotEmpty_ListaConElementos_NoEstaVacia()
    {
        var lista = new List<string> { "elemento1", "elemento2" };
        Assert.That(lista, Is.Not.Empty);
    }

    [Test]
    public void Empty_ListaNueva_EstaVacia()
    {
        var lista = new List<string>();
        Assert.That(lista, Is.Empty);
    }
}
```

---

## 8. Pruebas con Excepciones

### Teoría

No solo hay que probar que un método retorna el valor correcto en condiciones normales, sino también que **falla correctamente** cuando recibe entradas inválidas. Un método bien diseñado lanza excepciones con mensajes claros en lugar de retornar valores incorrectos silenciosamente.

Para verificar que un método lanza una excepción determinada se usa `Assert.Throws<TipoDeExcepcion>(() => { ... })`.

La expresión lambda `() => { ... }` encapsula la llamada al método que se espera que falle. Si el método lanza la excepción del tipo indicado, el test pasa. Si no la lanza (o lanza una excepción de otro tipo), el test falla.

### Ejemplo: Verificar que Dividir lanza excepción al dividir por cero

```csharp
// Recordar: Calculadora.Dividir lanza DivideByZeroException cuando b == 0

[TestFixture]
public class CalculadoraExcepcionesTest
{
    [Test]
    public void Dividir_DivisorCero_LanzaDivideByZeroException()
    {
        // Arrange
        var calculadora = new Calculadora();

        // Act y Assert juntos: verificamos que se lanza la excepción
        Assert.Throws<DivideByZeroException>(() =>
        {
            calculadora.Dividir(10, 0);
        });
    }

    [Test]
    public void Dividir_DivisorNoEsCero_NoLanzaExcepcion()
    {
        // Arrange
        var calculadora = new Calculadora();

        // Act
        double resultado = calculadora.Dividir(10, 2);

        // Assert: verificamos que el resultado es correcto (no se lanzó excepción)
        Assert.That(resultado, Is.EqualTo(5.0));
    }
}
```

También es posible capturar la excepción para verificar su mensaje:

```csharp
[Test]
public void Dividir_DivisorCero_ExcepcionConMensajeCorrecto()
{
    // Arrange
    var calculadora = new Calculadora();

    // Act
    var excepcion = Assert.Throws<DivideByZeroException>(() =>
    {
        calculadora.Dividir(10, 0);
    });

    // Assert: verificar también el mensaje de la excepción
    Assert.That(excepcion.Message, Is.EqualTo("No se puede dividir por cero."));
}
```

---

## 9. Nomenclatura de Tests

### Teoría

El nombre de un test es fundamental: debe describir exactamente qué se está probando sin necesidad de leer el cuerpo del método. Un test bien nombrado actúa como documentación.

La convención recomendada es:

```
MetodoQuePruebo_Escenario_ResultadoEsperado
```

- **MetodoQuePruebo**: el nombre del método o funcionalidad bajo prueba.
- **Escenario**: las condiciones específicas o el contexto del test (qué entrada, qué estado).
- **ResultadoEsperado**: qué se espera que suceda en ese escenario.

Esta convención tiene ventajas concretas:
- Al ver un test fallido en el reporte, inmediatamente sabés qué caso falló.
- Fuerza al desarrollador a pensar claramente qué está probando antes de escribir el test.
- Facilita la comunicación en el equipo: "el test `Dividir_DivisorCero_LanzaExcepcion` está fallando".

### Ejemplo: Comparación de nombres buenos y malos

```csharp
// MALOS nombres: no dicen nada sobre qué se prueba ni qué se espera
[Test] public void Test1() { ... }
[Test] public void TestSumar() { ... }
[Test] public void PruebaCalculadora() { ... }
[Test] public void MetodoFunciona() { ... }

// BUENOS nombres: claros y autodescriptivos
[Test] public void Sumar_DosNumerosPositivos_RetornaLaSuma() { ... }
[Test] public void Sumar_NumeroPositivoYNegativo_RetornaDiferencia() { ... }
[Test] public void Dividir_DivisorCero_LanzaDivideByZeroException() { ... }
[Test] public void Dividir_DosNumerosPositivos_RetornaCociente() { ... }
[Test] public void Persona_EdadNegativa_LanzaArgumentException() { ... }
[Test] public void Persona_MayorDeEdad_EsMayorDeEdadRetornaTrue() { ... }
[Test] public void ListaProductos_AgregarProducto_ContieneLaLista() { ... }
[Test] public void ListaProductos_ListaVacia_LanzaInvalidOperationException() { ... }
```

---

## 10. Testing con POO y Herencia

### Teoría

Cuando trabajamos con herencia y polimorfismo, los tests deben verificar el comportamiento específico de cada clase concreta. Si tenemos una jerarquía de clases, normalmente testeamos los métodos de cada clase derivada, especialmente aquellos que sobreescriben comportamiento de la clase base.

Un punto importante: testeamos el **comportamiento** (qué retorna el método, qué efecto produce), no la **implementación** (cómo lo hace internamente).

### Ejemplo: Jerarquía Figura y tests para cada subclase

```csharp
// Archivo: MiServicio/Figura.cs
public abstract class Figura
{
    public abstract double CalcularArea();
    public abstract double CalcularPerimetro();
}

// Archivo: MiServicio/Circulo.cs
public class Circulo : Figura
{
    public double Radio { get; }

    public Circulo(double radio)
    {
        if (radio <= 0)
            throw new ArgumentException("El radio debe ser mayor a cero.");
        Radio = radio;
    }

    public override double CalcularArea()
    {
        return Math.PI * Radio * Radio;
    }

    public override double CalcularPerimetro()
    {
        return 2 * Math.PI * Radio;
    }
}

// Archivo: MiServicio/Rectangulo.cs
public class Rectangulo : Figura
{
    public double Ancho { get; }
    public double Alto { get; }

    public Rectangulo(double ancho, double alto)
    {
        Ancho = ancho;
        Alto = alto;
    }

    public override double CalcularArea() => Ancho * Alto;
    public override double CalcularPerimetro() => 2 * (Ancho + Alto);
}
```

```csharp
// Archivo: MiServicioTest/FiguraTest.cs
[TestFixture]
public class CirculoTest
{
    [Test]
    public void CalcularArea_RadioValido_RetornaAreaCorrecta()
    {
        // Arrange
        var circulo = new Circulo(5);
        double areaEsperada = Math.PI * 5 * 5; // ≈ 78.54

        // Act
        double areaObtenida = circulo.CalcularArea();

        // Assert
        Assert.That(areaObtenida, Is.EqualTo(areaEsperada).Within(0.001));
    }

    [Test]
    public void CalcularPerimetro_RadioValido_RetornaPerimetroCorrect()
    {
        // Arrange
        var circulo = new Circulo(3);
        double perimetroEsperado = 2 * Math.PI * 3; // ≈ 18.85

        // Act
        double perimetroObtenido = circulo.CalcularPerimetro();

        // Assert
        Assert.That(perimetroObtenido, Is.EqualTo(perimetroEsperado).Within(0.001));
    }

    [Test]
    public void Constructor_RadioNegativo_LanzaArgumentException()
    {
        Assert.Throws<ArgumentException>(() => new Circulo(-1));
    }

    [Test]
    public void EsInstanciaCorrect_Circulo_EsInstanciaDeFigura()
    {
        var circulo = new Circulo(5);
        Assert.That(circulo, Is.InstanceOf<Figura>());
    }
}

[TestFixture]
public class RectanguloTest
{
    [Test]
    public void CalcularArea_DimensionesValidas_RetornaArea()
    {
        // Arrange
        var rectangulo = new Rectangulo(4, 6);

        // Act
        double area = rectangulo.CalcularArea();

        // Assert
        Assert.That(area, Is.EqualTo(24));
    }

    [Test]
    public void CalcularPerimetro_DimensionesValidas_RetornaPerimetro()
    {
        // Arrange
        var rectangulo = new Rectangulo(4, 6);

        // Act
        double perimetro = rectangulo.CalcularPerimetro();

        // Assert
        Assert.That(perimetro, Is.EqualTo(20));
    }
}
```

Nota: para comparar números de punto flotante (`double`) se usa `.Within(tolerancia)` para evitar fallos por errores de precisión numérica.

---

## 11. Cobertura de Tests

### Teoría

La **cobertura de tests** mide qué porcentaje del código de producción es ejecutado por los tests. Un método puede tener múltiples caminos de ejecución (por ejemplo, un `if`/`else`) y cada uno debe ser probado.

Una buena cobertura implica tener tests para:

- **Camino feliz (happy path)**: la entrada válida y normal que produce el resultado esperado.
- **Casos borde (edge cases)**: valores límite como cero, negativo, cadena vacía, null.
- **Casos de error**: entradas que deben producir una excepción o un fallo controlado.

Si un método retorna `true` o `false` dependiendo de una condición, hay que tener al menos un test que verifique el caso `true` y otro que verifique el caso `false`.

### Ejemplo: Cobertura completa para un método con múltiples caminos

```csharp
// Método a probar
public class EdadValidator
{
    public bool EsMayorDeEdad(int edad)
    {
        if (edad < 0)
            throw new ArgumentException("La edad no puede ser negativa.");

        return edad >= 18;
    }
}
```

```csharp
// Tests con cobertura completa del método
[TestFixture]
public class EdadValidatorTest
{
    // Caso 1: happy path — persona mayor de edad
    [Test]
    public void EsMayorDeEdad_Edad18_RetornaTrue()
    {
        var validator = new EdadValidator();
        Assert.That(validator.EsMayorDeEdad(18), Is.True);
    }

    // Caso 2: persona claramente mayor de edad
    [Test]
    public void EsMayorDeEdad_Edad30_RetornaTrue()
    {
        var validator = new EdadValidator();
        Assert.That(validator.EsMayorDeEdad(30), Is.True);
    }

    // Caso 3: menor de edad
    [Test]
    public void EsMayorDeEdad_Edad17_RetornaFalse()
    {
        var validator = new EdadValidator();
        Assert.That(validator.EsMayorDeEdad(17), Is.False);
    }

    // Caso 4: caso borde — recién nacido
    [Test]
    public void EsMayorDeEdad_EdadCero_RetornaFalse()
    {
        var validator = new EdadValidator();
        Assert.That(validator.EsMayorDeEdad(0), Is.False);
    }

    // Caso 5: caso de error — edad negativa debe lanzar excepción
    [Test]
    public void EsMayorDeEdad_EdadNegativa_LanzaArgumentException()
    {
        var validator = new EdadValidator();
        Assert.Throws<ArgumentException>(() => validator.EsMayorDeEdad(-5));
    }
}
```

Con estos 5 tests, todos los caminos posibles del método están cubiertos.

---

## 12. Ejecutar los Tests

### Teoría

Hay dos formas principales de ejecutar los tests:

**Desde la terminal** con `dotnet test`:
- Ejecuta todos los tests de la solución.
- Muestra un resumen con cuántos pasaron y cuántos fallaron.
- Útil para integración continua o cuando se trabaja sin IDE.

**Desde Visual Studio**:
- Menú `Test` → `Run All Tests`.
- O bien desde el panel "Test Explorer" (Ver → Test Explorer).
- Los tests verdes pasaron, los rojos fallaron.

Cuando un test falla, la salida muestra:
- Qué test falló (nombre del método).
- Qué valor se esperaba y qué valor se obtuvo realmente.
- En qué línea del código falló.

### Ejemplo: Ejecutar tests y leer la salida

```bash
# Ejecutar todos los tests de la solución
dotnet test

# Salida cuando todos los tests pasan:
# Build succeeded.
# Test run for MiServicioTest.dll (.NETCoreApp,Version=v8.0)
# Microsoft (R) Test Execution Command Line Tool Version 17.x.x
# Starting test execution, please wait...
#
# Passed!  - Failed:     0, Passed:    8, Skipped:   0, Total:    8, Duration: 45 ms

# Salida cuando hay un test fallido:
# Failed   CalculadoraTest.Sumar_DosNumerosPositivos_RetornaLaSuma
#   Expected: 15
#   But was:  14
#
# Failed!  - Failed:     1, Passed:    7, Skipped:   0, Total:    8, Duration: 52 ms
```

La línea más importante en caso de fallo es:
- `Expected: 15` — lo que el test esperaba obtener.
- `But was: 14` — lo que el método retornó realmente.

Esto te indica exactamente qué está mal en el código de producción.

---

## 13. Buenas Prácticas en Testing

### Teoría

#### Una sola assertion por test (idealmente)

Cada test debe verificar una única cosa. Si un test tiene diez `Assert.That`, es difícil saber exactamente qué falló cuando el test no pasa.

#### Tests independientes entre sí

El orden en que se ejecutan los tests no debe importar. Un test no debe depender del estado que dejó otro test anterior. Cada test debe preparar su propio estado en el Arrange.

#### Tests deterministas

Un test debe producir siempre el mismo resultado, sin importar cuántas veces se ejecute, en qué máquina, a qué hora o en qué orden. Los tests que a veces pasan y a veces fallan (llamados "flaky tests") son peores que no tener tests, porque generan falsa confianza.

#### Probar comportamiento, no implementación

Los tests deben verificar qué hace un método (su contrato con el mundo exterior), no cómo lo hace internamente. Si refactorizás la implementación sin cambiar el comportamiento, los tests no deben romperse.

### Ejemplo: Comparación de tests malos vs buenos

```csharp
// MAL: test con múltiples assertions que prueban cosas distintas
[Test]
public void PruebaCalculadoraTodo()
{
    var calc = new Calculadora();
    Assert.That(calc.Sumar(2, 3), Is.EqualTo(5));       // prueba suma
    Assert.That(calc.Restar(10, 4), Is.EqualTo(6));     // prueba resta
    Assert.That(calc.Multiplicar(3, 4), Is.EqualTo(12)); // prueba multiplicación
    // Si falla el primero, no sabemos si los otros funcionan
}

// BIEN: un test por comportamiento
[Test]
public void Sumar_DosPositivos_RetornaSuma()
{
    var calc = new Calculadora();
    Assert.That(calc.Sumar(2, 3), Is.EqualTo(5));
}

[Test]
public void Restar_NumeroPequenioDeGrande_RetornaDiferencia()
{
    var calc = new Calculadora();
    Assert.That(calc.Restar(10, 4), Is.EqualTo(6));
}

[Test]
public void Multiplicar_DosNumeros_RetornaProducto()
{
    var calc = new Calculadora();
    Assert.That(calc.Multiplicar(3, 4), Is.EqualTo(12));
}

// MAL: test que depende de estado compartido entre tests (frágil)
private static Calculadora _calculadoraCompartida = new Calculadora();

[Test]
public void Test_A_ModificaEstado() { /* modifica _calculadoraCompartida */ }

[Test]
public void Test_B_DependeDelTestAnterior() { /* usa el estado que dejó Test_A */ }

// BIEN: cada test crea su propio objeto en el Arrange
[Test]
public void Cualquier_Test_IndependienteConSuPropioArrange()
{
    var calc = new Calculadora(); // siempre fresco, sin dependencias
    Assert.That(calc.Sumar(1, 1), Is.EqualTo(2));
}
```

---

*Unidad 4 - Programación I | UCSE*
