
# Introducción a SQL y Tipos de Datos en SQL Server

## ¿Qué es SQL?
SQL (Structured Query Language) es un lenguaje estándar para gestionar y manipular bases de datos relacionales. Con SQL, puedes realizar tareas como:
- Consultar datos
- Insertar nuevos registros
- Actualizar registros existentes
- Eliminar registros
- Crear y modificar estructuras de bases de datos

## Tipos de Datos en SQL Server
SQL Server soporta varios tipos de datos que se pueden usar para definir las columnas de una tabla. Aquí hay una descripción de algunos de los tipos de datos más comunes:

1. **uniqueidentifier**
   - Usado para almacenar valores de identificadores únicos globales (UUIDs o GUIDs).
   - Ejemplo: `uniqueidentifier`

2. **int**
   - Usado para almacenar enteros.
   - Rango: -2,147,483,648 a 2,147,483,647.
   - Ejemplo: `int`

3. **nvarchar**
   - Usado para almacenar cadenas de texto de longitud variable.
   - Puede almacenar caracteres Unicode, lo que permite almacenar texto en varios idiomas.
   - Ejemplo: `nvarchar(50)`, donde 50 es el número máximo de caracteres.

4. **decimal**
   - Usado para almacenar números decimales con una precisión fija.
   - Ejemplo: `decimal(10, 2)`, donde 10 es el total de dígitos y 2 es el número de dígitos a la derecha del punto decimal.

5. **bit**
   - Usado para almacenar valores booleanos (0 o 1).
   - Ejemplo: `bit`

6. **datetime**
   - Usado para almacenar tanto la fecha como la hora.
   - Rango: 1 de enero de 1753 a 31 de diciembre de 9999.
   - Ejemplo: `datetime`

## Ejemplo de Creación de una Tabla
Vamos a crear una tabla llamada `Users` que tiene las siguientes columnas:
- `UserID`: Un identificador único para cada usuario (uniqueidentifier).
- `FirstName`: El nombre del usuario (nvarchar).
- `LastName`: El apellido del usuario (nvarchar).
- `BirthDate`: La fecha y hora de nacimiento del usuario (datetime).
- `Email`: El correo electrónico del usuario (nvarchar).
- `IsActive`: El estado de actividad del usuario (bit).

```sql
CREATE TABLE Users (
    UserID uniqueidentifier DEFAULT NEWID() PRIMARY KEY,
    FirstName nvarchar(50) NOT NULL,
    LastName nvarchar(50) NOT NULL,
    BirthDate datetime NOT NULL,
    Email nvarchar(100) NOT NULL,
    IsActive bit NOT NULL
);
```

### Explicación del Ejemplo
- `UserID uniqueidentifier DEFAULT NEWID() PRIMARY KEY`: Crea una columna `UserID` de tipo `uniqueidentifier` y establece un valor por defecto usando la función `NEWID()` para generar un GUID único. Además, define esta columna como la clave primaria.
- `FirstName nvarchar(50) NOT NULL`: Crea una columna `FirstName` de tipo `nvarchar` con un máximo de 50 caracteres y no permite valores nulos.
- `LastName nvarchar(50) NOT NULL`: Similar a `FirstName`, pero para los apellidos.
- `BirthDate datetime NOT NULL`: Crea una columna `BirthDate` de tipo `datetime` y no permite valores nulos.
- `Email nvarchar(100) NOT NULL`: Crea una columna `Email` de tipo `nvarchar` con un máximo de 100 caracteres y no permite valores nulos.
- `IsActive bit NOT NULL`: Crea una columna `IsActive` de tipo `bit` para indicar si el usuario está activo y no permite valores nulos.

## Inserción de Datos en la Tabla
Aquí hay un ejemplo de cómo insertar datos en la tabla `Users`:

```sql
INSERT INTO Users (FirstName, LastName, BirthDate, Email, IsActive)
VALUES ('John', 'Doe', '2000-01-15 08:30:00', 'john.doe@example.com', 1);
```

En este ejemplo, estamos insertando un usuario con el nombre "John Doe", una fecha de nacimiento con fecha y hora `2000-01-15 08:30:00`, un correo electrónico `john.doe@example.com`, y un estado de actividad `IsActive` igual a 1 (activo).

## Peticiones HTTP en .NET 6 con C#
En aplicaciones web, las peticiones HTTP se utilizan para comunicar el cliente con el servidor. Las peticiones más comunes son:

### GET
Se usa para solicitar datos del servidor. No modifica el estado del servidor.

**Ejemplo en .NET 6 con C#:**

```csharp
[HttpGet]
public async Task<ActionResult<IEnumerable<User>>> GetUsers()
{
    return await _context.Users.ToListAsync();
}
```

### POST
Se usa para enviar datos al servidor y crear un nuevo recurso.

**Ejemplo en .NET 6 con C#:**

```csharp
[HttpPost]
public async Task<ActionResult<User>> PostUser(User user)
{
    _context.Users.Add(user);
    await _context.SaveChangesAsync();

    return CreatedAtAction("GetUser", new { id = user.UserID }, user);
}
```

### PUT
Se usa para actualizar un recurso existente en el servidor.

**Ejemplo en .NET 6 con C#:**

```csharp
[HttpPut("{id}")]
public async Task<IActionResult> PutUser(Guid id, User user)
{
    if (id != user.UserID)
    {
        return BadRequest();
    }

    _context.Entry(user).State = EntityState.Modified;

    try
    {
        await _context.SaveChangesAsync();
    }
    catch (DbUpdateConcurrencyException)
    {
        if (!UserExists(id))
        {
            return NotFound();
        }
        else
        {
            throw;
        }
    }

    return NoContent();
}
```

### DELETE
Se usa para eliminar un recurso del servidor.

**Ejemplo en .NET 6 con C#:**

```csharp
[HttpDelete("{id}")]
public async Task<IActionResult> DeleteUser(Guid id)
{
    var user = await _context.Users.FindAsync(id);
    if (user == null)
    {
        return NotFound();
    }

    _context.Users.Remove(user);
    await _context.SaveChangesAsync();

    return NoContent();
}
```

### Explicación de las Peticiones
- **GET**: Recupera datos del servidor. En el ejemplo, `GetUsers` devuelve una lista de todos los usuarios.
- **POST**: Envía datos al servidor para crear un nuevo recurso. En el ejemplo, `PostUser` añade un nuevo usuario a la base de datos.
- **PUT**: Actualiza un recurso existente. En el ejemplo, `PutUser` actualiza los datos de un usuario existente.
- **DELETE**: Elimina un recurso del servidor. En el ejemplo, `DeleteUser` elimina un usuario basado en su `UserID`.

