---
layout: post
title:  "Blazor webassebmly set UI focus"
date:   2020-12-31 23:08:00 +0700
categories: Blazor Csharp
---
Blazor webassebmly set UI focus

File `Uifocus.razor` (must start with Upper case `U`) put in the same folder with file `Counter.razor`

```
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

```
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
