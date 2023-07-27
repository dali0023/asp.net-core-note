## Web API

- API Fundamentals
- DTO's in API
- EF Core in API
- Dependency Injection
- Repository Pattern
- Authentication and Authorization
- API Versioning, Filtering, Sorting, Caching Etc
- Consuming API with Authentication endpoints
- Deploying API


**To Use JsonPatch install:**
```c#
Microsoft.AspNetCore.JsonPatch
Microsoft.AspNetCore.Mvc.NewtonsoftJson
```
Also Update Program.cs
```c#
// Change From
builder.Services.AddControllers();

// Change To
builder.Services.AddControllers().AddNewtonsoftJson();
```












