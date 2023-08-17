# Dosya Yapısı
```
Deneme
   │
   ├── Controllers
   │   └── ProductController.cs
   │
   ├── Models
   │   └── Product.cs
   │
   ├── Repositories
   │   └── RepositoryContext.cs
   │
   ├── appsettings.json
   │
   └── Program.cs
```
---
# Dosyalar

## Product.cs

```csharp
namespace Deneme.Models
{
    public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

}

```

## RepositoryContext.cs

```csharp
using Deneme.Models;
using Microsoft.EntityFrameworkCore;

namespace Deneme.Repositories
{
    public class RepositoryContext : DbContext
    {
        public RepositoryContext(DbContextOptions<RepositoryContext> options) : base(options) { }

        public DbSet<Product> Products { get; set; }
    }
}
```

## ProductController.cs

```csharp
using Deneme.Models;
using Deneme.Repositories;
using Microsoft.AspNetCore.Mvc;

namespace Deneme.Controllers
{
    [Route("api/products")]
    [ApiController]
    public class ProductController : ControllerBase
    {
        private readonly RepositoryContext _context;

        public ProductController(RepositoryContext context)
        {
            _context = context;
        }

        [HttpGet]
        public IActionResult GetProducts()
        {
            try
            {
                var products = _context.Products.ToList();

                return Ok(products);
            }
            catch (Exception e)
            {

                throw new Exception(e.Message);
            }
        }

        [HttpGet("{id:int}")]
        public IActionResult GetProduct([FromRoute(Name = "id")] int id)
        {
            try
            {
                var product = _context.Products.FirstOrDefault(p => p.Id == id);

                if (product == null) return NotFound();

                return Ok(product);
            }
            catch (Exception e)
            {

                throw new Exception(e.Message);
            }
        }

        [HttpPost]
        public IActionResult AddProduct([FromBody] Product product)
        {
            try
            {
                if (product == null) return BadRequest("Güncellenecek ürün verisi boş olamaz.");

                _context.Products.Add(product);
                _context.SaveChanges();

                return Created("/products/" + product.Id, $"Ürün oluşturuldu. Ürün id: {product.Id}");
            }
            catch (Exception e)
            {
                return StatusCode(500, $"Ürün eklenirken bir hata oluştu: {e.Message}");
            }
        }

        [HttpPut("{id:int}")]
        public IActionResult UpdateProduct([FromRoute(Name = "id")] int id, [FromBody] Product product)
        {
            try
            {
                if (product == null) return BadRequest("Güncellenecek ürün verisi boş olamaz.");

                var entity = _context.Products.FirstOrDefault(p => p.Id == id);

                if (entity == null) return NotFound("Belirtilen ID ile eşleşen ürün bulunamadı.");

                entity.Id = product.Id;
                entity.Name = product.Name;

                _context.SaveChanges();

                return Ok($"Ürün güncellendi. Ürün ${entity}");
            }
            catch (Exception e)
            {
                throw new Exception(e.Message);
            }
        }

        [HttpDelete("{id:int}")]
        public IActionResult DeleteProduct([FromRoute(Name = "id")] int id)
        {
            try
            {
                var entity = _context.Products.FirstOrDefault(p => p.Id == id);

                if (entity == null) return NotFound("Belirtilen ID ile eşleşen ürün bulunamadı.");

                _context.Products.Remove(entity);
                _context.SaveChanges();
                 
                return NoContent();
            }
            catch (Exception e)
            {
                throw new Exception(e.Message);
            }
        }
    }
}
```

## appsettings.json

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*",
  "ConnectionStrings": {
    "PgSqlConnection": "Host=localhost;Port=5432;User ID=postgres;Password=f.1234;Database=db_deneme;Pooling=true;"
  }
}
```

## Program.cs

```csharp
using Deneme.Repositories;
using Microsoft.EntityFrameworkCore;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddControllers();
builder.Services.AddEndpointsApiExplorer();

var connectionString = builder.Configuration.GetConnectionString("PgSqlConnection");

builder.Services.AddDbContext<RepositoryContext>(options =>
    options.UseNpgsql(connectionString,
    b => b.MigrationsAssembly("Deneme")));

var app = builder.Build();

app.MapControllers();

app.Run();
```

# Gerekli Paketler

## EntityFrameworkCore

```powershell
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet add package Microsoft.EntityFrameworkCore.Tools
```
## PostgreSQL

```powershell
dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL
```