# Entity Framework Core 

## 1.	Connecting to Database:
   - Database Connection String
   - DBContext
   - DbSet

## 2. Configuring the Model: 
`Create Model>Add Migration>Apply Migration`

**Configure Using Conventions:**
   - Code First Conventions in Entity Framework Core

**Configure Using Data Annotations:**
  - Data Annotations in entity framework Core
  - Table Attribute
  - Column Attribute
  - Key Attribute
  - ComplexType Attribute
  - ConcurrencyCheck Attribute
  - Timestamp Attribute
  - Databasegenerated Attribute
  - ForeignKey Attribute
  - MaxLength / MinLength Attribute
  - StringLength Attribute
  - NotMapped Attribute
  - Required Attribute
  - InverseProperty Attribute

## 3. Configure Using Fluent API:
  - Fluent API Entity Framework Core
  - Ignore Method
  - HasAlternateKey
  - EntityType Configuration

## 4. Configuring the Relationships:
   - Relationships & Navigational Properties
   - One to One Relationships
   - One to Many Relationships
   - Many to Many Relationships

## 5. Querying in EF Core:
   - Querying in Entity Framework Core
   - First, FirstOrDefault, Single, SingleOrDefault in EF Core
   - EF Core Find Method
   - Projection Queries in EF Core
   - EF Core Join Query
   - Eager Loading using Include & ThenInclude in EF Core
   - SelectMany in EF Core
   - Explicit Loading in EF Core
   - Lazy Loading in EF Core

## 6	Persisting the Data:
   - Save Changes in Entity Framework Core
   - ChangeTracker, EntityEntry & Entity States
   - Add Records/Add Multiple Records
   - Update Record
   - Delete Record
   - Cascade Delete in Entity Framework Core

## 7	Migrations:
 - EF Core Migrations
 - Reverse Engineer Database (Database First)
 - EF Core Data Seeding to the Database
 - EF Core Script Migration

8	Others:
 - Logging in EF Core
 - Using MySQL & MariaDB in Entity Framework Core



## 1. Connecting to Database:
`Microsoft.EntityFrameworkCore`, `Microsoft.EntityFrameworkCore.SqlServer`,  `Microsoft.EntityFrameworkCore.Tools`, `Microsoft.EntityFrameworkCore.Design`

**Create Database Connection String to `appsettings.json`**
```json
"AllowedHosts": "*",
  "ConnectionStrings": {
    "TestDbContextConnection": "Server=(localdb)\\MSSQLLocalDB;Database=DatabaseName;Trusted_Connection=True;MultipleActiveResultSets=true"
  }
```

**DbContext & DbSet Add new file `Data/TestDbContext.cs`**
```c#
using Microsoft.EntityFrameworkCore;

namespace efcore2.Data
{
    public class TestDbContext : DbContext
    {
        public TestDbContext(DbContextOptions<TestDbContext> options) : base(options) { }
    }
}
```

**The database context is registered with the Dependency Injection container in the `Program.cs` file:**
```c#
// Add DbContext
builder.Services.AddDbContext<TestDbContext>(option => option.UseSqlServer(builder.Configuration.GetConnectionString("TestDbContextConnection")));
```

**Add a data model class:**
`Create model> add-migration>Update-Database`
```c#
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace efcore2.Models
{
    [Table("tbl_Movies")] // change Table Name
    public class Movie
    {
        public int Id { get; set; } // Primary key must be Id or ClassNameId(MovieId)
        
        [Required]
        [MaxLength(50), MinLength(10)]
        [Column("Title")] // change Column Name
        public string? Movie_Title { get; set; } //  ? means null value also allow
        
        [DataType(DataType.Date)]
        public DateTime ReleaseDate { get; set; }
        public string? Genre { get; set; }
        public decimal Price { get; set; }
        public bool IsTrendingProduct { get; set; }

        // Set Foreign Key
        [ForeignKey("Category")]
        public int Category_Id { get; set; }
        public Category Category { get; set; }

    }
}
```











