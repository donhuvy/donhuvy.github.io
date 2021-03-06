---
layout: post
title:  "Blazor webassembly set UI focus"
date:   2020-12-31 23:08:00 +0700
categories: Blazor Csharp
---
Blazor webassembly set UI focus

Development environment: ASP.NET Core Blazor WebAssembly version 5 with Individual security, Microsoft Visual Studio Community 2019 Version 16.8.3, .NET 5 version 5.0.101, Windows x64 version 2004, DevExpress Blazor web-assembly version 20.0.4 .

File `Uifocus.razor` (must start with Upper case `U`) put in the same folder with file `Counter.razor`

```html
@page "/ui-focus"

<h1>Set UI focus</h1>

<button class="btn btn-primary" @onclick="() => input.FocusAsync()">Focus the input!</button>

<input @ref="input" />

@Action

@code{
    [Parameter] public string Action { get; set; }
    ElementReference input;
}
```


File `NavMenu.razor`

```html
<div class="top-row pl-4 navbar navbar-dark">
    <a class="navbar-brand" href="">a2021</a>
    <button class="navbar-toggler" @onclick="ToggleNavMenu">
        <span class="navbar-toggler-icon"></span>
    </button>
</div>

<div class="@NavMenuCssClass" @onclick="ToggleNavMenu">
    <ul class="nav flex-column">
        <li class="nav-item px-3">
            <NavLink class="nav-link" href="" Match="NavLinkMatch.All">
                <span class="oi oi-home" aria-hidden="true"></span> Home
            </NavLink>
        </li>
        <li class="nav-item px-3">
            <NavLink class="nav-link" href="counter">
                <span class="oi oi-plus" aria-hidden="true"></span> Counter
            </NavLink>
        </li>
        <li class="nav-item px-3">
            <NavLink class="nav-link" href="fetchdata">
                <span class="oi oi-list-rich" aria-hidden="true"></span> Fetch data
            </NavLink>
        </li>

        <li class="nav-item px-3">
            <NavLink class="nav-link" href="ui-focus">
                <span class="oi oi-list-rich" aria-hidden="true"></span> Set focus
            </NavLink>
        </li>
    </ul>
</div>

@code {
    private bool collapseNavMenu = true;

    private string NavMenuCssClass => collapseNavMenu ? "collapse" : null;

    private void ToggleNavMenu()
    {
        collapseNavMenu = !collapseNavMenu;
    }
}
```
