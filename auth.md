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

**Also Add `<partial name="_LoginPartial"></partial>` to `'/Views/Shared/_Layout.cshtml'`**
```cshtml
<ul>
  <li><a asp-area="" asp-controller="Home" asp-action="Index">Home</a></li>
  <partial name="_LoginPartial"></partial>
</ul>
```

