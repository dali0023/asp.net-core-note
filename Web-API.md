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
Operations
Add: `{ "op": "add", "path": "/biscuits/1", "value": { "name": "Ginger Nut" } }`
Remove: 
`{ "op": "remove", "path": "/biscuits" }` // Removes a value from an object or array.
`{ "op": "remove", "path": "/biscuits/0" }`
Removes the first element of the array at biscuits (or just removes the “0” key if biscuits is an object)

Replace
`{ "op": "replace", "path": "/biscuits/0/name", "value": "Chocolate Digestive" }`

Copy
`{ "op": "copy", "from": "/biscuits/0", "path": "/best_biscuit" }`
Copies a value from one location to another within the JSON document. Both from and path are JSON Pointers.

Move
`{ "op": "move", "from": "/biscuits", "path": "/cookies" }`
Moves a value from one location to the other. Both from and path are JSON Pointers.

Test
`{ "op": "test", "path": "/best_biscuit/name", "value": "Choco Leibniz" }`
Tests that the specified value is set in the document. If the test fails, then the patch as a whole should not apply.

Details: https://jsonpatch.com/









