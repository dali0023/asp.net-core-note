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
        <li class="nav-item dropdown">
            <a class="nav-link link text-black dropdown-toggle show display-4" data-toggle="dropdown-submenu" data-bs-toggle="dropdown" data-bs-auto-close="outside" aria-expanded="false"><span class="mobi-mbri mobi-mbri-user-2 mbr-iconfont mbr-iconfont-btn"></span></a>
            <div class="dropdown-menu show" aria-labelledby="dropdown-751" data-bs-popper="none">
                <a class="text-black show dropdown-item display-4" asp-area="Identity" asp-page="/Account/Logout">Logout</a>
            </div>
        </li>
}
else
{
        <li class="nav-item dropdown">
            <a class="nav-link link text-black dropdown-toggle show display-4" data-toggle="dropdown-submenu" data-bs-toggle="dropdown" data-bs-auto-close="outside" aria-expanded="false"><span class="mobi-mbri mobi-mbri-user-2 mbr-iconfont mbr-iconfont-btn"></span></a>
            <div class="dropdown-menu show" aria-labelledby="dropdown-751" data-bs-popper="none">
                <a class="text-black show dropdown-item display-4" asp-area="Identity" asp-page="/Account/Register">Register</a>
                <a class="text-black show dropdown-item display-4" asp-area="Identity" asp-page="/Account/Login">Login</a>
            </div>
        </li>
}
</ul>

**also Add `<partial name="_LoginPartial"></partial>` to `'/Views/Shared/_Layout.cshtml'`**
```cshtml
<div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav nav-dropdown nav-right" data-app-modern-menu="true">
          <li class="nav-item"><a class="nav-link link text-black show display-4" asp-area="" asp-controller="Home" asp-action="Index">Home</a></li>
          <partial name="_LoginPartial"></partial>
      </ul>
</div>
```

