# Buenas Prácticas en .NET

## 1. Seguir las convenciones de nomenclatura

.NET tiene **estándares de nombres oficiales** que ayudan a mantener el código consistente.

### Clases y métodos
Usar **PascalCase**

```csharp
public class UserService
{
    public void GetUser()
    {
    }
}
```
Variables y parámetros

Usar camelCase
```csharp
int userAge;
string userName;
```
Interfaces

Siempre empezar con I
```csharp
public interface IUserRepository
{
}
```
2. Usar principios SOLID

Los principios SOLID ayudan a crear código mantenible y escalable.

S — Single Responsibility Principle

Cada clase debe tener una única responsabilidad.

❌ Incorrecto
```csharp
public class UserService
{
    public void SaveUser()
    {
    }

    public void SendEmail()
    {
    }
}
```
✔ Correcto
```csharp
public class UserService
{
    public void SaveUser()
    {
    }
}

public class EmailService
{
    public void SendEmail()
    {
    }
}
```
O — Open/Closed Principle

Las clases deben estar abiertas para extensión pero cerradas para modificación.

Usar interfaces o herencia.

L — Liskov Substitution Principle

Las clases derivadas deben poder sustituir a sus clases base sin romper el comportamiento.

I — Interface Segregation Principle

Evitar interfaces grandes.

❌
```csharp
public interface IUserService
{
    void Create();
    void Delete();
    void SendEmail();
}
```
✔
```csharp
public interface IUserService
{
    void Create();
    void Delete();
}

public interface IEmailService
{
    void SendEmail();
}
```
D — Dependency Inversion Principle

Depender de abstracciones, no de implementaciones.

❌
```csharp
public class UserService
{
    private EmailService emailService = new EmailService();
}
```

✔
```csharp
public class UserService
{
    private readonly IEmailService emailService;

    public UserService(IEmailService emailService)
    {
        this.emailService = emailService;
    }
}
```
3. Usar Dependency Injection

.NET tiene DI integrado.

Beneficios:

Código desacoplado
Facilita testing
Mejor mantenimiento

Ejemplo:
```csharp
builder.Services.AddScoped<IUserService, UserService>();
```
4. Evitar código duplicado (DRY)

DRY = Don't Repeat Yourself

❌
```csharp
int total = price + price * 0.21;
int total2 = price2 + price2 * 0.21;
```

✔
```csharp
public decimal CalculateTax(decimal price)
{
    return price * 1.21m;
}
```
5. Usar async / await correctamente

Evita bloquear threads.

✔ Correcto
```csharp
public async Task<User> GetUserAsync(int id)
{
    return await repository.GetUserAsync(id);
}
```
❌ Incorrecto
```csharp
repository.GetUserAsync(id).Result
```

Esto puede provocar deadlocks.

6. Manejar excepciones correctamente

No usar excepciones para flujo normal.

❌
```csharp
try
{
    var number = int.Parse(input);
}
catch
{
}
```
✔
```csharp
if(int.TryParse(input, out int number))
{
}
```
Capturar excepciones específicas.
```csharp
catch(IOException ex)
{
}
```
7. Usar logging en lugar de Console.WriteLine

Usar ILogger
```csharp
private readonly ILogger<UserService> logger;

logger.LogInformation("User created");
```
Niveles comunes:

Trace
Debug
Information
Warning
Error
Critical
8. Mantener controladores ligeros (ASP.NET)

Los controllers deben delegar lógica al servicio.

❌
```csharp
public IActionResult CreateUser(User user)
{
    // lógica compleja aquí
}
```
✔
```csharp
public IActionResult CreateUser(User user)
{
    userService.Create(user);
    return Ok();
}
```
9. Usar DTOs

No exponer entidades directamente.

❌
```csharp
public User GetUser()
```
✔
```csharp
public UserDto GetUser()
```
Esto evita:

fuga de datos
acoplamiento con la base de datos
10. Validar datos de entrada

Usar FluentValidation o DataAnnotations
```csharp
public class UserDto
{
    [Required]
    public string Name { get; set; }
}
```
11. Usar configuración tipada

Evitar strings mágicos.

❌
```csharp
var connection = Configuration["ConnectionString"];
```
✔
```csharp
public class DatabaseSettings
{
    public string ConnectionString { get; set; }
}
```
12. Usar LINQ de forma eficiente

✔
```csharp
var activeUsers = users.Where(u => u.IsActive).ToList();
```
Evitar múltiples enumeraciones.

❌
```csharp
users.Where(x => x.Active);
users.Where(x => x.Age > 18);
```
✔
```csharp
users.Where(x => x.Active && x.Age > 18);
```
13. Escribir código limpio

Reglas importantes:

métodos pequeños
nombres descriptivos
evitar comentarios innecesarios
evitar métodos largos

❌
```csharp
public void Process()
```
✔
```csharp
public void CalculateInvoiceTotal()
```
14. Usar pruebas unitarias

Frameworks comunes:

xUnit
NUnit
MSTest

Ejemplo con xUnit
```csharp
[Fact]
public void ShouldCalculateTotal()
{
    var service = new PriceService();

    var result = service.Calculate(100);

    Assert.Equal(121, result);
}
```
15. Usar arquitectura limpia

Arquitecturas comunes en .NET:

Clean Architecture
Onion Architecture
Hexagonal Architecture

Capas típicas:

API
Application
Domain
Infrastructure

Esto mejora:

mantenimiento
testabilidad
escalabilidad
16. Evitar clases gigantes

Una clase no debería tener demasiadas responsabilidades.

Si una clase supera:

300–400 líneas
demasiadas dependencias

Probablemente necesita dividirse.

17. Usar IEnumerable vs List correctamente

Usar IEnumerable cuando solo necesitas iterar.
```csharp
public IEnumerable<User> GetUsers()
```
Usar List cuando necesitas modificar.
```csharp
List<User> users = new();
```
18. Usar nameof en lugar de strings

❌
```csharp
throw new ArgumentNullException("user");
```
✔
```csharp
throw new ArgumentNullException(nameof(user))
```
19. Evitar magia de números

❌
```csharp
if(user.Age > 18)
```
✔
```csharp
const int AdultAge = 18;
if(user.Age > AdultAge)
```
20. Mantener el código simple (KISS)

KISS = Keep It Simple, Stupid

Evitar:

sobreingeniería
abstracciones innecesarias
patrones innecesarios

El código más simple suele ser el mejor.

❌ Demasiado complejo

```csharp
public interface IMessageFormatterStrategy
{
    string Format(string message);
}

public class UpperCaseMessageFormatterStrategy : IMessageFormatterStrategy
{
    public string Format(string message) => message.ToUpper();
}

public class MessageFormatterContext
{
    private readonly IMessageFormatterStrategy strategy;

    public MessageFormatterContext(IMessageFormatterStrategy strategy)
    {
        this.strategy = strategy;
    }

    public string Execute(string message)
    {
        return strategy.Format(message);
    }
}
```
Si solo necesitas transformar un texto una vez, esto puede ser excesivo.

✔ Más simple
```csharp
public string FormatMessage(string message)
{
    return message.ToUpper();
}
```
Aplicar KISS no significa escribir código pobre, sino evitar complejidad innecesaria.
