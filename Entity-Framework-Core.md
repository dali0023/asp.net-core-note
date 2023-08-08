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


## Seed Data to a Table:

`ApplicationDbContext.cs`

```c#
// Seed Data
public class ApplicationDbContext : DbContext
{
   public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options) : base(options) { }
   public DbSet<Villa> Villas { get; set; }
   protected override void OnModelCreating(ModelBuilder modelBuilder)
   {
      base.OnModelCreating(modelBuilder);
      modelBuilder.Entity<Villa>().HasData(
        new Villa
          {
              Id = 1,
              Name = "Royal Villa",
              ImageUrl = "https://dotnetmastery.com/bluevillaimages/villa3.jpg",
              Amenity = "",
              CreatedDate = DateTime.Now
          },
        new Villa
        {
            Id = 2,
            Name = "Premium Pool Villa",
            ImageUrl = "https://dotnetmastery.com/bluevillaimages/villa1.jpg",
            Amenity = "",
            CreatedDate = DateTime.Now
        });
   }
}
```
**Add Migration And Update Database**

`add-migration SeedDataToVillaTable`
`update-database`

## Relationship:

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


## One To One Relationship using Data Annotation and Fluent Api
```c#
// using data annotation
    public class Book
    {
        public int Id { get; set; }
        [Required]
        public string? Title { get; set; }
        [NotMapped] // notmapped means this column wil not saved on table but can display
        public string? PriceRange { get; set; }

        // Set Foreign Key
        [ForeignKey("BookDetail")]
        public int BookDetailId { get; set; }
        public BookDetail? BookDetail { get; set; }
    }

public class BookDetail
    {
        public int BookDetailId { get; set; }
        public int NumberOfChapters { get; set; }
        [Required]
        public int NumberOfPages { get; set; }
        public double Weight { get; set; }
        public Book? Book { get; set; } // Navigational Properties

    }
```
**Same Table using Fluent Api**
```c#
public class FluentBook
    {
        public int Id { get; set; }
        public string? Title { get; set; }
        public string? PriceRange { get; set; }

        // Set Foreign Key
        public int BookDetailId { get; set; }
        public FluentBookDetail? FluentBookDetail { get; set; }
    }

// FluentBookDetail
public class FluentBookDetail
    {
        public int BookDetailId { get; set; }
        public int NumberOfChapters { get; set; }
        public int NumberOfPages { get; set; }
        public double Weight { get; set; }
        
        // Set Reference
        public FluentBook? FluentBook { get; set; }
    }
```
**Also write in TestDbContext**
```c#
public DbSet<FluentBook> FluentBook { get; set; }
public DbSet<FluentBookDetail> FluentBookDetails { get; set; }
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
   // Book
   modelBuilder.Entity<FluentBook>().HasKey(b => b.Id);
   modelBuilder.Entity<FluentBook>().Property(b => b.Title).IsRequired();
   modelBuilder.Entity<FluentBook>().Property(b => b.ISBN).IsRequired().HasMaxLength(30);
   modelBuilder.Entity<FluentBook>().Property(b => b.Price).IsRequired();
   
   //BookDetails
   modelBuilder.Entity<FluentBookDetail>().HasKey(b => b.BookDetailId); // create primary key
   modelBuilder.Entity<FluentBookDetail>().Property(b => b.NumberOfChapters).IsRequired(); // create required     
    
   //One to One Relationship Between FluentBook and FluentBookDetail
   modelBuilder.Entity<FluentBook>()
               .HasOne(b => b.FluentBookDetail)
               .WithOne(b => b.FluentBook)
               .HasForeignKey<FluentBook>(fk => fk.BookDetailId);
}
```

### One To Many Relationship
**One To Many Relationship Using Data Annotaion**
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

// One Publisher can have many books
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

### One To Many Relationship Using Fluent Api
```c#
public class FluentBook
    {
        public int FluentBookId { get; set; }
        public string? Title { get; set; }
        public string? ISBN { get; set; }
        public double Price { get; set; }
        public string? PriceRange { get; set; }

        // Set Foreign Key
        public int PublisherId { get; set; }
        public FluentPublisher? FluentPublisher { get; set; }
    }

public class FluentPublisher
    {
        public int FluentPublisherId { get; set; }
        public string? Name { get; set; }
        public string? Location { get; set; }
        public List<FluentBook>? FluentBooks { get; set; }
    }
```
**Also write in TestDbContext**
```c#
public DbSet<FluentBook> FluentBooks { get; set; }
public DbSet<FluentPublisher> FluentPublishers { get; set; }

protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            // FluentBook
            modelBuilder.Entity<FluentBook>().HasKey(b => b.FluentBookId);
            modelBuilder.Entity<FluentBook>().Property(b => b.Title).IsRequired();
            modelBuilder.Entity<FluentBook>().Property(b => b.ISBN).IsRequired().HasMaxLength(30);
            modelBuilder.Entity<FluentBook>().Property(b => b.Price).IsRequired();

            // FluentPublisher
            modelBuilder.Entity<FluentPublisher>().HasKey(b => b.FluentPublisherId);
            modelBuilder.Entity<FluentPublisher>().Property(b => b.Name).IsRequired();
            modelBuilder.Entity<FluentPublisher>().Property(b => b.Location).IsRequired();

            // One to Many relationship betwen Book and Publisher
            modelBuilder.Entity<FluentBook>()
                        .HasOne(b => b.FluentPublisher)
                        .WithMany(c => c.FluentBooks)
                        .HasForeignKey(fk => fk.PublisherId);
        }
```

### Many To Many Relationship
**Many to Many Relationship using Data Annotation**
```c#
//    Book.cs
public class Book
    {
        public int Id { get; set; }
        [Required]
        public string? Title { get; set; }
        [Required]
        public double Price { get; set; }

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

        // Add Reference for Many to Many Relationship
        public ICollection<BookInAuthor> BookInAuthor { get; set; }
    }

// BookInAuthor
public class BookInAuthor
    {
        [ForeignKey("Book")]
        public int BookId { get; set; }
        public Book? Book { get; set; }

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

**Many to Many Relationship using Fluent Api**
```c#
// FluentBook
public class FluentBook
    {
        public int FluentBookId { get; set; }
        public string? Title { get; set; }
        public string? ISBN { get; set; }
        public double Price { get; set; }
        public string? PriceRange { get; set; }

        // Many to many Relationship
        public ICollection<FluentBookAuthor>? FluentBookAuthors { get; set; }
    }

// 
public class FluentAuthor
    {
        public int FluentAuthorId { get; set; }
        public string? FirstName { get; set; }
        public string? LastName { get; set; }
        public DateTime BirthDate { get; set; }
        public string FullName
        {
            get
            {
                return $"{FirstName} {LastName}";
            }
        }

        public ICollection<FluentBookAuthor>? FluentBookAuthors { get; set; }
    }

// FluentBookAuthor
public class FluentBookAuthor
    {
        public int FluentBookId { get; set; }
        public FluentBook? FluentBook { get; set; }

        public int FluentAuthorId { get; set; }
        public FluentAuthor? FluentAuthor { get; set; }
    }

```
**Also write in Data/TestDbContext**
```c#
public DbSet<FluentBook> FluentBooks { get; set; }
public DbSet<FluentAuthor>? FluentAuthors{ get; set; }
public DbSet<FluentBookAuthor> FluentBookAuthors { get; set; }

protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            // FluentBook
            modelBuilder.Entity<FluentBook>().HasKey(b => b.FluentBookId);
            modelBuilder.Entity<FluentBook>().Property(b => b.Title).IsRequired();
            modelBuilder.Entity<FluentBook>().Property(b => b.ISBN).IsRequired().HasMaxLength(30);
            modelBuilder.Entity<FluentBook>().Property(b => b.Price).IsRequired();

            // FluentAuthor
            modelBuilder.Entity<FluentAuthor>().HasKey(b => b.FluentAuthorId);
            modelBuilder.Entity<FluentAuthor>().Property(b => b.FirstName).IsRequired();
            modelBuilder.Entity<FluentAuthor>().Ignore(b => b.FullName);

            // Many to Many Relationship between Book and Author
            
            // Create primary key by using composite key   
            modelBuilder.Entity<FluentBookAuthor>().HasKey(ba => new { ba.FluentAuthorId, ba.FluentBookId });
            
            // Double One To Many Relationship = many to many relationship
            modelBuilder.Entity<FluentBookAuthor>()
                        .HasOne(b => b.FluentBook)
                        .WithMany(b => b.FluentBookAuthors)
                        .HasForeignKey(fk => fk.FluentBookId);

            modelBuilder.Entity<FluentBookAuthor>()
                        .HasOne(b => b.FluentAuthor)
                        .WithMany(b => b.FluentBookAuthors)
                        .HasForeignKey(fk => fk.FluentAuthorId);

        }
```

## Organize Data/TestDbContext File:
**Original Data/TestDbContext**
```c#
using Microsoft.EntityFrameworkCore;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using TestNtier_Model.Models;

namespace TestNtier_DataAccess.Data
{
    public class TestDbContext : DbContext
    {
        public TestDbContext(DbContextOptions<TestDbContext> options) : base(options) { }

        public DbSet<Book> Books { get; set; }
        public DbSet<Publisher> Publishers { get; set; }
        public DbSet<FluentBook> FluentBooks { get; set; }
        public DbSet<FluentPublisher> FluentPublishers { get; set; }
        public DbSet<FluentAuthor>? FluentAuthors{ get; set; }
        public DbSet<FluentBookAuthor> FluentBookAuthors { get; set; }
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {

            // FluentBook
            modelBuilder.Entity<FluentBook>().HasKey(b => b.FluentBookId);
            modelBuilder.Entity<FluentBook>().Property(b => b.Title).IsRequired();
            modelBuilder.Entity<FluentBook>().Property(b => b.ISBN).IsRequired().HasMaxLength(30);
            modelBuilder.Entity<FluentBook>().Property(b => b.Price).IsRequired();


            //BookDetails
            modelBuilder.Entity<FluentBookDetail>().HasKey(b => b.BookDetailId); // create primary key
            modelBuilder.Entity<FluentBookDetail>().Property(b => b.NumberOfChapters).IsRequired(); // create required     

            //One to One Relationship Between FluentBook and FluentBookDetail
            modelBuilder.Entity<FluentBook>()
                        .HasOne(b => b.FluentBookDetail)
                        .WithOne(b => b.FluentBook)
                        .HasForeignKey<FluentBook>(fk => fk.BookDetailId);



            // FluentPublisher
            modelBuilder.Entity<FluentPublisher>().HasKey(b => b.FluentPublisherId);
            modelBuilder.Entity<FluentPublisher>().Property(b => b.Name).IsRequired();
            modelBuilder.Entity<FluentPublisher>().Property(b => b.Location).IsRequired();


            // One to Many relationship betwen Book and Publisher
            modelBuilder.Entity<FluentBook>().HasOne(b => b.FluentPublisher)
                        .WithMany(c => c.FluentBooks)
                        .HasForeignKey(fk => fk.PublisherId);


            // Fluent Author
            modelBuilder.Entity<FluentAuthor>().HasKey(b => b.FluentAuthorId);
            modelBuilder.Entity<FluentAuthor>().Property(b => b.FirstName).IsRequired();
            modelBuilder.Entity<FluentAuthor>().Ignore(b => b.FullName);

            // Many to Many Relationship between Book and Author
            
            // Create primary key by using composite key   
            modelBuilder.Entity<FluentBookAuthor>().HasKey(ba => new { ba.FluentAuthorId, ba.FluentBookId });
            
            // Double One To Many Relationship = many to many relationship
            modelBuilder.Entity<FluentBookAuthor>()
                        .HasOne(b => b.FluentBook)
                        .WithMany(b => b.FluentBookAuthors)
                        .HasForeignKey(fk => fk.FluentBookId);

            modelBuilder.Entity<FluentBookAuthor>()
                        .HasOne(b => b.FluentAuthor)
                        .WithMany(b => b.FluentBookAuthors)
                        .HasForeignKey(fk => fk.FluentAuthorId);

        }
    }
}

```
#### Separate Configuration Classes
**Create Folder on `TestNtier_DataAccess/FluentConfig/FluentPublisherConfig`**
```c#
public class TestDbContext : DbContext
    {
        public TestDbContext(DbContextOptions<TestDbContext> options) : base(options) { }

        public DbSet<FluentBook> FluentBooks { get; set; }
        public DbSet<FluentPublisher> FluentPublishers { get; set; }
        public DbSet<FluentAuthor>? FluentAuthors{ get; set; }
        public DbSet<FluentBookAuthor> FluentBookAuthors { get; set; }
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {         
            modelBuilder.ApplyConfiguration(new FluentBookConfig()); // FluentBook
            modelBuilder.ApplyConfiguration(new FluentBookDetailConfig()); //BookDetails
            modelBuilder.ApplyConfiguration(new FluentPublisherConfig()); // FluentPublisher
            modelBuilder.ApplyConfiguration(new FluentAuthorConfig()); // FluentAuthor
            modelBuilder.ApplyConfiguration(new FluentBookAuthorConfig()); // FluentBookAuthor
        }
    }
```

**Move Code from `TestNtier_DataAccess/Data/TestDbContext` to `TestNtier_DataAccess/FluentConfig/FluentBookConfig.cs`**
```c#
// TestNtier_DataAccess/FluentConfig/FluentBookConfig.cs
public class FluentBookConfig : IEntityTypeConfiguration<FluentBook>
    {
        public void Configure(EntityTypeBuilder<FluentBook> modelBuilder)
        {
            modelBuilder.HasKey(b => b.FluentBookId);
            modelBuilder.Property(b => b.Title).IsRequired();
            modelBuilder.Property(b => b.ISBN).IsRequired().HasMaxLength(30);
            modelBuilder.Property(b => b.Price).IsRequired();

            // One to One Relationship Between FluentBook and FluentBookDetail
            modelBuilder.HasOne(b => b.FluentBookDetail)
                        .WithOne(b => b.FluentBook)
                        .HasForeignKey<FluentBook>(fk => fk.BookDetailId);

            // One to Many relationship betwen Book and Publisher
            modelBuilder.HasOne(b => b.FluentPublisher)
                        .WithMany(c => c.FluentBooks)
                        .HasForeignKey(fk => fk.PublisherId);
        }
    }

// TestNtier_DataAccess/FluentConfig/FluentBookDetailConfig.cs
public class FluentBookDetailConfig : IEntityTypeConfiguration<FluentBookDetail>
    {
        public void Configure(EntityTypeBuilder<FluentBookDetail> modelBuilder)
        {
            modelBuilder.HasKey(b => b.BookDetailId); // create primary key
            modelBuilder.Property(b => b.NumberOfChapters).IsRequired(); // create required     
        }
    }

// TestNtier_DataAccess/FluentConfig/FluentPublisherConfig.cs
public class FluentPublisherConfig : IEntityTypeConfiguration<FluentPublisher>
    {
        public void Configure(EntityTypeBuilder<FluentPublisher> modelBuilder)
        {
            modelBuilder.HasKey(b => b.FluentPublisherId);
            modelBuilder.Property(b => b.Name).IsRequired();
            modelBuilder.Property(b => b.Location).IsRequired();
        }
    }

// TestNtier_DataAccess/FluentConfig/FluentAuthorConfig.cs
public class FluentAuthorConfig : IEntityTypeConfiguration<FluentAuthor>
    {
        public void Configure(EntityTypeBuilder<FluentAuthor> modelBuilder)
        {
            modelBuilder.HasKey(b => b.FluentAuthorId);
            modelBuilder.Property(b => b.FirstName).IsRequired();
            modelBuilder.Ignore(b => b.FullName);
        }
    }

// TestNtier_DataAccess/FluentConfig/FluentBookAuthorConfig.cs
public class FluentBookAuthorConfig : IEntityTypeConfiguration<FluentBookAuthor>
    {
        public void Configure(EntityTypeBuilder<FluentBookAuthor> modelBuilder)
        {
            // Create primary key by using composite key   
            modelBuilder.HasKey(ba => new { ba.FluentAuthorId, ba.FluentBookId });

            // Double One To Many Relationship = many to many relationship
            modelBuilder.HasOne(b => b.FluentBook)
                        .WithMany(b => b.FluentBookAuthors)
                        .HasForeignKey(fk => fk.FluentBookId);

            modelBuilder.HasOne(b => b.FluentAuthor)
                        .WithMany(b => b.FluentBookAuthors)
                        .HasForeignKey(fk => fk.FluentAuthorId);
        }
    }
```


