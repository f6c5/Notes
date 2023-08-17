# ProductsController

Bu C# kod örneği, ASP.NET Core ile oluşturulan bir Web API'da ürünlerle ilgili işlemleri gerçekleştirmek için bir Controller sınıfıdır. Controller, `Products` adında bir liste içerisinde bazı örnek ürünlerle çalışır.

## Endpoints

### GET /api/products

Bu endpoint, mevcut ürünlerin tamamını getirir.

### GET /api/products/{id}

Bu endpoint, belirtilen `id` ile eşleşen bir ürünü getirir.

### POST /api/products

Bu endpoint, yeni bir ürün ekler.

### PUT /api/products/{id}

Bu endpoint, belirtilen `id` ile eşleşen bir ürünü günceller.

### DELETE /api/products/{id}

Bu endpoint, belirtilen `id` ile eşleşen bir ürünü siler.

## Model: Product

Bir `Product` sınıfı, ürünlerin temel özelliklerini temsil eder. Bu özellikler `Id` ve `Name` olarak adlandırılmıştır.

### Örnek Ürünler

Controller sınıfı, aşağıdaki örnek ürünlerle başlatılmıştır:

1. Product{ Id=1, Name="product1" }
2. Product{ Id=2, Name="product2" }
3. Product{ Id=3, Name="product3" }

## Metodlar

### `GetProducts()`

Mevcut tüm ürünleri getiren HTTP GET isteğiyle tetiklenen bir metod.

### `GetProduct(int id)`

Belirtilen `id` ile eşleşen bir ürünü getiren HTTP GET isteğiyle tetiklenen bir metod.

### `AddProduct(Product product)`

Yeni bir ürün ekleyen HTTP POST isteğiyle tetiklenen bir metod.

### `UpdateProduct(int id, Product product)`

Belirtilen `id` ile eşleşen bir ürünü güncelleyen HTTP PUT isteğiyle tetiklenen bir metod.

### `DeleteProduct(int id)`

Belirtilen `id` ile eşleşen bir ürünü silen HTTP DELETE isteğiyle tetiklenen bir metod.

**Not:** Bu örnek, basit bir şekilde ürünleri bir bellek listesinde saklar. Gerçek uygulamalarda veritabanı gibi kalıcı bir veri kaynağı kullanmak daha uygun olacaktır.

```csharp
using Microsoft.AspNetCore.Mvc;
using deneme.Models;
namespace deneme.Controllers
{

    [Route("api/products")]
    [ApiController]
    public class ProductsController : ControllerBase
    {
        public static List<Product> products = new List<Product>{
                new Product(){ Id=1, name="product1" },
                new Product(){ Id=2, name="product2" },
                new Product(){ Id=3, name="product3" }
        };

        [HttpGet]
        public IActionResult GetProducts()
        {
            return Ok(products);
        }

        [HttpGet("{id:int}")]
        public IActionResult GetProduct([FromRoute(Name = "id")] int id)
        {
            var product = products.Where(p => p.Id == id).SingleOrDefault();

            if (product == null) return NotFound();

            return Ok(product);
        }

        [HttpPost]
        public IActionResult AddProduct([FromBody] Product product)
        {
            try
            {
                if (product == null) return BadRequest("Güncellenecek ürün verisi boş olamaz.");

                products.Add(product);

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
            if (product == null) return BadRequest("Güncellenecek ürün verisi boş olamaz.");

            var entity = products.Find(p => p.Id == id);

            if (entity == null) return NotFound("Belirtilen ID ile eşleşen ürün bulunamadı.");

            try
            {
                products[products.IndexOf(entity)] = product;

                return Ok($"Ürün güncellendi. Ürün ${product}");
            }
            catch (Exception e)
            {
                return StatusCode(500, $"Ürün güncelleme sırasında bir hata oluştu: {e.Message}");
            }
        }

        [HttpDelete("{id:int}")]
        public IActionResult DeleteProduct([FromRoute(Name = "id")] int id)
        {
            var entity = products.Find(p => p.Id == id);

            if (entity == null) return NotFound("Belirtilen ID ile eşleşen ürün bulunamadı.");

            try
            {
                products.Remove(entity);
                return NoContent();
            }
            catch (Exception e)
            {
                return StatusCode(500, $"Ürün silme sırasında bir hata oluştu: {e.Message}");
            }
        }
    }
}
```