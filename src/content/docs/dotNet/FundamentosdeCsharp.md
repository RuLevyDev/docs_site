# Fundamentos de C#

## ¿Cuál es la diferencia entre value types y reference types?

En C# existen dos categorías principales de tipos:

### Value Types
- Almacenan **directamente el valor**.
- Se guardan normalmente en el **stack**.
- Cuando se asignan a otra variable, **se copia el valor**.

Ejemplos:
- `int`
- `double`
- `bool`
- `struct`
- `enum`

```csharp
int a = 5;
int b = a;
b = 10;
```

// a sigue siendo 5
Reference Types
Almacenan una referencia a la dirección de memoria donde está el objeto.
Los objetos suelen estar en el heap.
Al asignar a otra variable, se copia la referencia, no el objeto.

Ejemplos:

class
string
object
array
```csharp
class Persona { public string Nombre; }

Persona p1 = new Persona();
p1.Nombre = "Ana";

Persona p2 = p1;
p2.Nombre = "Luis";

// p1.Nombre también será "Luis"
```
¿Qué diferencia hay entre struct y class?
Característica	struct	class
Tipo	Value type	Reference type
Memoria	Stack	Heap
Herencia	No puede heredar de otra struct o clase	Sí puede heredar
Uso típico	Datos pequeños	Objetos complejos

Ejemplo:
```csharp
struct Punto
{
    public int X;
    public int Y;
}
```
```csharp
class Persona
{
    public string Nombre;
}
```
Se recomienda struct cuando:

El objeto es pequeño
Es inmutable
No requiere herencia
¿Qué es boxing y unboxing?

Es la conversión entre value types y reference types.

Boxing

Convierte un value type → object
```csharp
int numero = 10;
object obj = numero; // boxing
```
Unboxing


Convierte un object → value type
```csharp
object obj = 10;
int numero = (int)obj; // unboxing
```

El boxing genera una copia en el heap, lo que puede afectar al rendimiento.

¿Cuál es la diferencia entre var, dynamic y object?
var
El tipo se infiere en tiempo de compilación
Una vez definido, no cambia
var numero = 10; // int
dynamic
El tipo se resuelve en tiempo de ejecución
No hay verificación de tipos en compilación
```csharp
dynamic dato = 10;
dato = "hola";
```
object
Es la clase base de todos los tipos en .NET
Requiere casting
```
object dato = 10;
int numero = (int)dato;
```
¿Qué es un nullable type y cómo se usa?

Permite que un value type pueda ser null.

Sintaxis:
```csharp
int? edad = null;
```
Equivalente a:
```csharp
Nullable<int> edad = null;
```
Uso:
```csharp
int? numero = null;

if(numero.HasValue)
{
    Console.WriteLine(numero.Value);
}
```
¿Qué hace el operador ??

Es el operador de coalescencia nula.

Devuelve el valor de la izquierda si no es null, si lo es devuelve el de la derecha.
```csharp
string nombre = null;
string resultado = nombre ?? "Invitado";

Console.WriteLine(resultado); // Invitado
```
¿Qué diferencia hay entre == y .Equals()?
==
Compara valores o referencias
Su comportamiento puede variar según el tipo
Equals()
Método de la clase object
Compara contenido del objeto

Ejemplo:
```csharp
string a = "hola";
string b = "hola";

Console.WriteLine(a == b);        // true
Console.WriteLine(a.Equals(b));   // true
```
Con objetos personalizados puede cambiar.

¿Qué es readonly?

Indica que un campo solo puede asignarse una vez.

Se puede asignar:

En la declaración
En el constructor
```csharp
class Persona
{
    public readonly int Edad;

    public Persona(int edad)
    {
        Edad = edad;
    }
}
```
¿Qué es const y en qué se diferencia de readonly?
Característica	const	readonly
Valor	Constante	Solo lectura
Momento de asignación	Compilación	Ejecución
Modificable en constructor	No	Sí
Tipo	Primitivos	Cualquier tipo

Ejemplo:
```csharp
const double PI = 3.1416;

readonly int edad;
```
¿Qué hace using en C#?

Tiene tres usos principales.

1. Importar namespaces
```csharp
using System;
```
Permite usar clases sin escribir el namespace completo.

2. Liberar recursos automáticamente

Usado con objetos que implementan IDisposable.
```csharp
using (var archivo = new StreamReader("file.txt"))
{
    string contenido = archivo.ReadToEnd();
}
```
Esto llama automáticamente a Dispose().

3. Alias de tipos
```csharp
using Texto = System.String;

Texto mensaje = "Hola";
```
