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


### For JsonPatch Notes:
```Json
Add
{ "op": "add", "path": "/biscuits/1", "value": { "name": "Ginger Nut" } }

Remove: 
{ "op": "remove", "path": "/biscuits" }
{ "op": "remove", "path": "/biscuits/0" }

Replace
{ "op": "replace", "path": "/biscuits/0/name", "value": "Chocolate Digestive" }

Copy
{ "op": "copy", "from": "/biscuits/0", "path": "/best_biscuit" }

Move
{ "op": "move", "from": "/biscuits", "path": "/cookies" }

Test
{ "op": "test", "path": "/best_biscuit/name", "value": "Choco Leibniz" }
```
Operations


Details: https://jsonpatch.com/


**To Stop other Type except JSON, and allow manually XML Type in Program.cs**
```c#
// Change From
builder.Services.AddControllers().AddNewtonsoftJson();

// Change To
builder.Services.AddControllers(option =>
{
  option.ReturnHttpNotAcceptable=true;
}).AddNewtonsoftJson().AddXmlDataContractSerializerFormatters();
```





