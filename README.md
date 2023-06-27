# asp.net-core-note

# Model
- Add a data model class
- Install Packages:
   > `Microsoft.EntityFrameworkCore`
   
   > `Microsoft.EntityFrameworkCore.SqlServer`
   
   > `Microsoft.EntityFrameworkCore.Tools`
- Add Repository Pattern
- DbContext(Database) and DbSet(Table)
- Connection String to `appsettings.json`
- Register DbContext in Program.cs file:**

**1. Add a data model class:**
   
```c#
using System.ComponentModel.DataAnnotations;

namespace RazorPagesMovie.Models;

public class Movie
{
    public int Id { get; set; }
    public string? Title { get; set; }
    [DataType(DataType.Date)]
    public DateTime ReleaseDate { get; set; }
    public string? Genre { get; set; }  // ? means null
    public decimal Price { get; set; }
    public bool IsTrendingProduct { get; set;}
}
```

**2.1:  Repository Pattern Logic:**
**Create an `Interface` for the repository inside the `Models/Interfaces`**
`Models/Interfaces/IProductRepository.cs`
```c#
namespace CoffeeShop.Models.Interfaces
{
    public interface IProductRepository
    {
        IEnumerable<Product> GetAllProducts();
        IEnumerable<Product> GetTrendingProducts();
        Product? GetProductDetail(int id);
    }
}
```

**2.2:  Create an `Class` for the repository inside the `Models/Repositories`**
`Models/Repositories/ProductRepository.cs`
```c#
using CoffeeShop.Data;
using CoffeeShop.Models.Interfaces;

namespace CoffeeShop.Models.Repositories
{
    public class ProductRepository : IProductRepository
    {
        private CoffeeShopDbContext _dbContext;

        public ProductRepository(CoffeeShopDbContext _dbContext)
        {
            this._dbContext = _dbContext;
        }

        public IEnumerable<Product> GetAllProducts()
        {
            return _dbContext.Products;
        }

        public Product? GetProductDetail(int id)
        {
            return _dbContext.Products.FirstOrDefault(p => p.Id == id);
        }

        public IEnumerable<Product> GetTrendingProducts()
        {
            return _dbContext.Products.Where(p => p.IsTrendingProduct == true);
        }
    }
}


```
**2.3  Register Services/Repository in IOC Container in `program.cs`**
```c#
// Add services to the container.
builder.Services.AddControllersWithViews();
builder.Services.AddScoped<IProductRepository, ProductRepository>();
```

**3. DbContext & DbSet**
Add new file `Data/CoffeeShopDbContext.cs`
```c#
using CoffeeShop.Models;
using Microsoft.EntityFrameworkCore;

namespace CoffeeShop.Data
{
    public class CoffeeShopDbContext : DbContext
    {
        public CoffeeShopDbContext(DbContextOptions<CoffeeShopDbContext> options) : base(options) {}
        public DbSet<Product> Products { get; set; }
    }
}
```
**4. Create Database Connection String to `appsettings.json`**
```json
  "AllowedHosts": "*",
  "ConnectionStrings": {
    "CoffeeShopDbContextConnection": "Server=(localdb)\\MSSQLLocalDB;Database=CoffeeShopDb;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
```
**5. The database context is registered with the Dependency Injection container in the Program.cs file:**
```c#
// Add DbContext
builder.Services.AddDbContext<CoffeeShopDbContext>(option => option.UseSqlServer(builder.Configuration.GetConnectionString("CoffeeShopDbContextConnection")));
```
**6. Create your first migration**
```c#
Add-Migration InitialCreate
update-database
```





  

