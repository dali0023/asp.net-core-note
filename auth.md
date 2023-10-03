Asp.Net Core Identity:
Install Package:
- `Microsoft.AspNetCore.Identity.EntityFrameworkCore`
- `Microsoft.AspNetCore.Identity.UI`

**Change `Data/CoffeeShopDbContext` files from `CoffeeShopDbContext : DbContext` To `CoffeeShopDbContext : IdentityDbContext`**
```C#
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
public class CoffeeShopDbContext : IdentityDbContext { }
```
**Also Add below code to `Program.cs`**
```C#
app.UseAuthentication();
app.UseAuthorization();
```

**Right Click on Project Folder and select `Build`**

**Migration: **
```C#
add-migration IdentityAdded
update-database
```

**Add Scaffolded Item**
- Right Click on Project Folder and Add > New Scaffolded Item> Identity > Add
- Select Project's DbContext and whatever I want, for basic `Login, logout, register`

**Edit `Program.cs` from `true` to `false`**
```C#
builder.Services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = false).AddEntityFrameworkStores<CoffeeShopDbContext>();

// Start Adding Session
builder.Services.AddSession();
builder.Services.AddHttpContextAccessor();
builder.Services.AddRazorPages();

var app = builder.Build();
app.UseSession();
// End added session
```
**Also add `MapRazorPages()` under UseStaticFiles()**
```c#
app.UseStaticFiles();
app.MapRazorPages();
```
**Add `'Scripts'` to `'/Views/Shared/_Layout.cshtml'`**
```cshtml
@await RenderSectionAsync("Scripts", false)
```
**edit `/Views/Shared/_LoginPartial.cshtml`**
```cshtml
@using Microsoft.AspNetCore.Identity

@inject SignInManager<IdentityUser> SignInManager
@inject UserManager<IdentityUser> UserManager

<ul class="navbar-nav">
@if (SignInManager.IsSignedIn(User))
{
        <a asp-area="Identity" asp-page="/Account/Logout">Logout</a>
}
else
{
       <a asp-area="Identity" asp-page="/Account/Register">Register</a>
       <a asp-area="Identity" asp-page="/Account/Login">Login</a>
}
</ul>
```
**Also Add `<partial name="_LoginPartial"></partial>` to `'/Views/Shared/_Layout.cshtml'`**

```cshtml
<ul>
  <li><a asp-area="" asp-controller="Home" asp-action="Index">Home</a></li>
  <partial name="_LoginPartial"></partial>
</ul>
```

## Authorization
Update `Program.cs`

```c#
// Change From
builder.Services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = false).AddEntityFrameworkStores<CoffeeShopDbContext>();

// To
builder.Services.AddIdentity<IdentityUser, IdentityRole>(options => options.SignIn.RequireConfirmedAccount = false)
                .AddEntityFrameworkStores<AuthTestDbContext>()
                .AddDefaultUI()
                .AddDefaultTokenProviders();
```
**Add `RolesController.cs`**

```C#
public class RolesController : Controller
    {
        private readonly RoleManager<IdentityRole> _manager;
        public RolesController(RoleManager<IdentityRole> roleManager)
        {
            this._manager = roleManager;
        }
        // GET: RolesController
        public ActionResult Index()
        {
            var roles = _manager.Roles.ToList();
            return View(roles);
        }

        // GET: RolesController/Create
        public ActionResult Create()
        {
            return View();
        }

        // POST: RolesController/Create
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create(IdentityRole role)
        {
            // Check if the role exists
            if (!_manager.RoleExistsAsync(role.Name).GetAwaiter().GetResult())
            {
                _manager.CreateAsync(new IdentityRole(role.Name)).GetAwaiter().GetResult();
            }
            return RedirectToAction("Index");
        }
    }
```
**Add Views/Roles/Index.cshtml**

```c#
@using Microsoft.AspNetCore.Identity
@model IEnumerable<IdentityRole>
@{
    ViewData["Title"]= "Roles";
}
<h2>Roles - </h2>
<a class="btn btn-link" asp-area="" asp-controller="Roles" asp-action="Create">Add New Role</a>
<table class="table table-hover">
    <thead>
        <tr>
            <th scope="col">Id</th>
            <th scope="col">Name</th>
            <th scope="col">NormalizeName</th>
        </tr>
    </thead>
    <tbody>
        @foreach (var item in Model)
        {
            <tr>
                <th scope="row">@item.Id</th>
                <td>@item.Name</td>
                <td>@item.NormalizedName</td>
            </tr>
        } 
    </tbody>
</table>
```

**Add Views/Roles/Create.cshtml**
```c#
@using Microsoft.AspNetCore.Identity
@model IdentityRole
@{
}
<h5>Add New Roles</h5>
<div class="row">
    <div class="col-md-6 offset-3">
        <form asp-action ="Create">
            <div asp-validation-summary="ModelOnly" class="text-danger"></div>
            <div class="mb-3">
                <label asp-for="Name" class="form-label">Name</label>
                <input type="text" class="form-control" asp-for="Name">
                <span asp-validation-for="Name" class="text-danger"></span>
            </div>
            <button type="submit" class="btn btn-primary">Save</button>
        </form>
    </div>
</div>
```











