# **SimplePutApi Instructions and Code**

## **Step 1: Create a New .NET Core Web API Project**

Open a terminal or command prompt and run the following command to create a new Web API project:

```bash
dotnet new webapi -n SimplePutApi
```

Navigate to the project directory:

```bash
cd SimplePutApi
```

## **Step 2: Create a Model**

Create a simple model class. Let's create a **`Product`** model.

Create a new file named **`Product.cs`** in the **`Models`** folder (create the **`Models`** folder if it doesn't exist):

```csharp
namespace SimplePutApi.Models
{
    public class Product
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public decimal Price { get; set; }
    }
}
```

## **Step 3: Create a Controller**

Create a new controller named **`ProductsController`**.

Create a new file named **`ProductsController.cs`** in the **`Controllers`** folder:

```csharp
Using Microsoft.AspNetCore.Mvc;
using SimplePutApi.Models
using System.Collections.Generic;
using System.Linq;

namespace SimplePutApi.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class ProductsController : ControllerBase
    {
        private static List<Product> products = new List<Product>
        {
            new Product { Id = 1, Name = "Product1", Price = 10.0m },
            new Product { Id = 2, Name = "Product2", Price = 20.0m }
        };

        [HttpGet]
        public ActionResult<IEnumerable<Product>> Get()
        {
            return products;
        }

        [HttpGet("{id}")]
        public ActionResult<Product> Get(int id)
        {
            var product = products.FirstOrDefault(p => p.Id == id);
            if (product == null)
            {
                return NotFound();
            }
            return product;
        }

        [HttpPut("{id}")]
        public IActionResult Put(int id, [FromBody] Product updatedProduct)
        {
            var product = products.FirstOrDefault(p => p.Id == id);
            if (product == null)
            {
                return NotFound();
            }

            product.Name = updatedProduct.Name;
            product.Price = updatedProduct.Price;

            return NoContent();
        }
    }
}

```

## **Step 4: Run the Application**

Run the application by executing the following command in the terminal:

```bash
dotnet run
```

## **Step 5: Test the PUT Endpoint**

You can use a tool like [Postman](https://www.postman.com/) or **`curl`** to test the PUT endpoint.

For example, using **`curl`**:

1. **Check the existing products:**

```bash
curl -X GET http://localhost:5006/api/products
```

1. **Update a product:**

```bash
curl -X PUT http://localhost:5006/api/products/1 -H "Content-Type: application/json" -d "{\"id\":1, \"name\":\"UpdatedProduct\", \"price\":15.0}"
```

1. **Verify the update:**

```bash
curl -X GET http://localhost:5006/api/products/1
```

This will show the updated product details.
