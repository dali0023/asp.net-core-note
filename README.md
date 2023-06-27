# asp.net-core-note

# Model
### Add a data model class
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
}
```
### Then you will need to create the `Context class` derived from the `DbContext`
```c#
public class AppContext : DbContext
{
    public AppContext(DbContextOptions<AppContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```

### Register the `Dependency Injection` in your application startup.
```c#
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<CookiePolicyOptions>(options =>
    {
        // This lambda determines whether user consent for non-essential cookies is needed for a given request.
        options.CheckConsentNeeded = context => true;
        options.MinimumSameSitePolicy = SameSiteMode.None;
    });

    var connection = "Server=localhost;Port=5432;Database=Test;User Id=postgres;Password=postgres;";
    services.AddDbContext<AppContext>(options => options.UseNpgsql(connection));

    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
}
```
**For PostgreSQL**
```c#
services.AddDbContext<AppContext>(options => options.UseNpgsql(connection));

// For MySQL
services.AddDbContext<AppContext>(options => options.UseMySql(connection));
// Or For SQL Server
services.AddDbContext<AppContext>(options => options.UseSqlServer(connection));
```



