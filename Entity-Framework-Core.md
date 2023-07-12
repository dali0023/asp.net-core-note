# Entity Framework Core 

# 1. Create N-Tier Architecture:
•	Right Click `Project Soluation>Add>New Project>Search Class Library then Select> Class Library C#(.Net )`

•	Rename `ProjectName_DataAccess`, `Delete Class1.cs`, `Add New Folder` inside on it called `Data`

•	Create Another: `Right Click Project Soluation>Add>New Project>Search Class Library then Select> Class Library C# (.Net)`

•	`Rename ProjectName_Model`, `Delete Class1.cs` and Move `Models Folder` from `Main Project` to `ProjectName_Model`, 

Change NameSpace `ProjectName.ProjectName_Model` and chane name space in `HomeController, _ViewImport`, and `Model>ErrorViewModel.cs`.

•	`Delete Models Folder` from Main Project

•	Right click on `Main Project>Add> Project Reference> then select both`

•	and `ProjectName_DataAccess>Add> Add Project Reference> then select only projectName_Model.`

Install: 
- Microsoft.EntityFrameworkCore
Select only ProjectName And Project_Data_Access
- Microsoft.EntityFrameworkCore.SqlServer
- Microsoft.EntityFrameworkCore.Tools






## 1.	Connecting to Database:
   - Database Connection String
   - DBContext
   - DbSet

## 2. Configuring the Model: 
`Create Model>Add Migration>Apply Migration`

**Configure Using Conventions:**
   - Code First Conventions in Entity Framework Core

**Configure Using Data Annotations / Entity Properties:**
  - Table, Column, Key, ComplexType, ConcurrencyCheck, Timestamp, Databasegenerated, ForeignKey, MaxLength / MinLength, StringLength, NotMapped, Required, InverseProperty.

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

**Add Model and Configure Using Data Annotations / Entity Properties:**
`Create model> add-migration>Update-Database`
```c#
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;

namespace efcore2.Models
{
    [Table("tbl_Movies")] // change Table Name
    public class Movie
    {
        public int Id { get; set; } // Primary key must be Id or ClassNameId ( MovieId )
        
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

        [NotMapped] // NotMapped means this column will not saved on database table but can display on page
        public string PriceRange { get; set; }
    }
}
```
**Register Model Class to Database `Data/TestDbContext.cs`**
```c#
public class TestDbContext : DbContext
    {
        public TestDbContext(DbContextOptions<TestDbContext> options) : base(options) { }
        
        // Register All Model Class to Database
        public DbSet<Movie> Movies { get; set; }
    }
```
        
**Create your first migration**
```c#
Add-Migration InitialCreate
update-database
```


## Migration
**For N-tier Structure migration, Select on Package Manager Console> Default Project-  ProjectName_DataAccess**
```c#
add-migration AddCategoryTableToDB
```

```c#
// To unapply a specific migration(s):
dotnet ef database update LastGoodMigrationName
or
PM> Update-Database -Migration LastGoodMigrationName

// To unapply all migrations:

dotnet ef database update 0
or
PM> Update-Database -Migration 0

// To remove last migration:

dotnet ef migrations remove
or
PM> Remove-Migration

// To remove all migrations:

just remove Migrations folder.

To remove last few migrations (not all):

There is no a command to remove a bunch of migrations and we can't just remove these few migrations and their *.designer.cs files since we need to keep the snapshot file in the consistent state. We need to remove migrations one by one (see To remove last migration above).

// To unapply and remove last migration:

dotnet ef migrations remove --force
or
PM> Remove-Migration -Force
```

## 4. Configuring the Relationships    Using Data Anotation:
Relationships & Navigational Properties

**One to One Relationships:**
```c#
// Parent Class(Book.cs)
public class Book
    {
        public int Id { get; set; }
        [Required]

         public string? Title { get; set; }
        [Required]
        public double Price { get; set; }

        [NotMapped] // notmapped means this column wil not saved on table but can display
        public string? PriceRange { get; set; }

        // Set Foreign Key
        [ForeignKey("BookDetail")]
        public int BookDetailId { get; set; }
        public BookDetail? BookDetail { get; set; }
    }

// Child Class (BookDetail)

public class BookDetail
    {
        public int BookDetailId { get; set; }
        public int NumberOfChapters { get; set; }
        [Required]
        public int NumberOfPages { get; set; }
        public double Weight { get; set; }
        public Book? Book { get; set; }

    }
```



**One to Many Relationships:**
```c#
// One book has one Publisher
public class Book
    {
        public int Id { get; set; }
        [Required]
        public string? Title { get; set; }
        [Required]
        public double Price { get; set; }

        // Set Foreign Key
        [ForeignKey("Publisher")]
        public int PublisherId { get; set; }
        public Publisher? Publisher { get; set; }
    }

// One Publisher has many books
public class Publisher
    {
        public int Id { get; set; }
        [Required]
        public string? Name { get; set; }
        [Required]
        public string? Location { get; set; }

        public List<Book>? Books { get; set; } // Also cas use Icollection<>, IList<> as many data
    }

```

**Many to Many Relationships:**

- Many books have many Authors
- Create A Joint Table between Book And Author BookInAuthor
```c
    public class BookInAuthor
    {
        // Set Foreign Key
        [ForeignKey("Book")]
        public int BookId { get; set; }
        public Book? Book { get; set; }

        // Set Foreign Key
        [ForeignKey("Author")]
        public int AuthorId { get; set; }
        public Author? Author { get; set; }
    }
```
**Create a Composite Key on Data/TestDbContext.cs**
```c#
 public class TestDbContext : DbContext
    {
        public TestDbContext(DbContextOptions<TestDbContext> options) : base(options) { }
        public DbSet<BookInAuthor> BookInAuthor { get; set; }

        // Write below code inside ApplicationDBContext
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            // Create primary key by using composite key  
            modelBuilder.Entity<BookInAuthor>().HasKey(ba => new { ba.AuthorId, ba.BookId });
        }
    }
```

```c#
//    Book.cs
public class Book
    {
        public int Id { get; set; }
        [Required]
        public string? Title { get; set; }
        [Required]
        public double Price { get; set; }

        // Set Foreign Key
        [ForeignKey("Publisher")]
        public int PublisherId { get; set; }
        public Publisher? Publisher { get; set; }

        public ICollection<BookInAuthor> BookInAuthor { get; set; }
    }


// Author.cs
public class Author
    {
        public int Id { get; set; }
        [Required]
        public string? Name { get; set; }

        [Required]
        public DateTime BirthDate { get; set; }
        public string? Location { get; set; }

        [Required]
        public double Price { get; set; }

        // Add Reference for Many to Many Relationship
        public ICollection<BookInAuthor> BookInAuthor { get; set; }
    }
```

## Relationship Using Fluent API:

Fluent API provides a number of important methods to configure entities and its properties:
**Model-wide Configurations:**
  - **HasDefaultSchema()**
  - **ComplexType()**

**Entity Configurations:**
- **HasIndex()**:Configures the `index property` for the entity type.
- **HasKey()**:Configures the `primary key` property for the entity type.
- **HasMany()**:	Configures the `Many relationship` for one-to-many or many-to-many relationships.
- **HasOptional()**:Configures an optional relationship which will create a nullable foreign key in the database.
- **HasRequired()**:Configures the required relationship which will create a `non-nullable foreign key` column in the database.
- **Ignore()**:	Configures that the class or property should not be mapped to a table or column.
- **Map()**:Allows advanced configuration related to how the entity is mapped to the database schema.
- **MapToStoredProcedures()**:Configures the entity type to use INSERT, UPDATE and DELETE stored procedures.
- **ToTable()**:Configures the table name for the entity.

**Property Configurations:**

- **HasColumnAnnotation():**
- **IsRequired():**
- **IsConcurrencyToken():**
- **IsOptional():**
- **HasParameterName():**
- **HasDatabaseGeneratedOption():**
- **HasColumnOrder():**
- **HasColumnType():**
- **HasColumnName():**





```c#

```














