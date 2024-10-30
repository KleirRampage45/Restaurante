Guía de Implementación del Proyecto de Gestión de Contactos
1. Configuración de la Base de Datos
1.1 Crear Base de Datos
Crea una base de datos llamada ContactDB.
1.2 Crear Tabla Contacto
Crea una tabla Contacto con las siguientes columnas:
IdContacto (int, PK)
Nombres (nvarchar(50))
Apellidos (nvarchar(50))
Telefono (nvarchar(15))
Correo (nvarchar(100))
1.3 Implementar Procedimientos Almacenados
Implementa los siguientes procedimientos almacenados:
sp_InsertarContacto
sp_ActualizarContacto
sp_EliminarContacto
sp_ObtenerContactos
sp_ObtenerContactoPorId
2. Desarrollo del Modelo
2.1 Crear Clase Contacto
En la carpeta Modelos, crea una clase Contacto:
csharp
Copiar código
using System.ComponentModel.DataAnnotations;

public class Contacto
{
    [Key]
    public int IdContacto { get; set; }

    [Required]
    [StringLength(50)]
    public string Nombres { get; set; } = string.Empty;

    [Required]
    [StringLength(50)]
    public string Apellidos { get; set; } = string.Empty;

    [Required]
    [StringLength(15)]
    public string Telefono { get; set; } = string.Empty;

    [Required]
    [EmailAddress]
    [StringLength(100)]
    public string Correo { get; set; } = string.Empty;
}
3. Desarrollo del Controlador
3.1 Crear ContactoController
Implementa un controlador ContactoController para manejar acciones CRUD:
csharp
Copiar código
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Configuration;
using Restaurante.API.Modelos;
using System.Collections.Generic;
using System.Data;
using System.Data.SqlClient;

namespace Restaurante.API.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class ContactoController : ControllerBase
    {
        private readonly string _connectionString;

        public ContactoController(IConfiguration configuration)
        {
            _connectionString = configuration.GetConnectionString("conexion");
        }

        // Métodos para Crear, Obtener, Actualizar, Eliminar
    }
}
4. Configuración de la Cadena de Conexión
4.1 Ajustar appsettings.json
Agrega la cadena de conexión en appsettings.json:
json
Copiar código
{
  "ConnectionStrings": {
    "conexion": "Data Source=LUMINA\\SQLEXPRESS01;Initial Catalog=ContactDB;Integrated Security=True"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
5. Implementación de la Vista
5.1 Crear Vistas
Crea vistas para permitir la interacción con la base de datos para:
Crear contactos
Listar contactos
Editar contactos
Eliminar contactos
6. Configuración de CORS
En Menu.cs o Program.cs, configura CORS para permitir solicitudes desde diferentes orígenes:
csharp
Copiar código
public void ConfigureServices(IServiceCollection services)
{
    services.AddCors(options =>
    {
        options.AddPolicy("AllowAllOrigins", builder =>
        {
            builder.AllowAnyOrigin()
                   .AllowAnyMethod()
                   .AllowAnyHeader();
        });
    });

    services.AddControllers();
}
7. Probar el API
Utiliza herramientas como Postman o Swagger para probar los endpoints del API:
Para crear un contacto, envía una solicitud POST con un cuerpo similar a:
json
Copiar código
{
  "nombres": "Juan",
  "apellidos": "Pérez",
  "telefono": "123456789",
  "correo": "juan.perez@example.com"
}
Nota
Asegúrate de que la base de datos y los procedimientos almacenados están correctamente configurados antes de ejecutar el API.
