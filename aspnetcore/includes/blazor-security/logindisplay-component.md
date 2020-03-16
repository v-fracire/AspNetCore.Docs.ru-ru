<span data-ttu-id="6fac8-101">Компонент `LoginDisplay` (*Shared/логиндисплай. Razor*) подготавливается к просмотру в компоненте `MainLayout` (*Shared/маинлайаут. Razor*) и управляет следующими поведениями:</span><span class="sxs-lookup"><span data-stu-id="6fac8-101">The `LoginDisplay` component (*Shared/LoginDisplay.razor*) is rendered in the `MainLayout` component (*Shared/MainLayout.razor*) and manages the following behaviors:</span></span>

* <span data-ttu-id="6fac8-102">Для пользователей, прошедших проверку подлинности:</span><span class="sxs-lookup"><span data-stu-id="6fac8-102">For authenticated users:</span></span>
  * <span data-ttu-id="6fac8-103">Отображает текущее имя пользователя.</span><span class="sxs-lookup"><span data-stu-id="6fac8-103">Displays the current username.</span></span>
  * <span data-ttu-id="6fac8-104">Предлагает кнопку для выхода из приложения.</span><span class="sxs-lookup"><span data-stu-id="6fac8-104">Offers a button to log out of the app.</span></span>
* <span data-ttu-id="6fac8-105">Для анонимных пользователей предлагает возможность входа в систему.</span><span class="sxs-lookup"><span data-stu-id="6fac8-105">For anonymous users, offers the option to log in.</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        Hello, @context.User.Identity.Name!
        <button class="nav-link btn btn-link" @onclick="BeginSignOut">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
        <a href="authentication/login">Log in</a>
    </NotAuthorized>
</AuthorizeView>

@code {
    private async Task BeginSignOut(MouseEventArgs args)
    {
        await SignOutManager.SetSignOutState();
        Navigation.NavigateTo("authentication/logout");
    }
}
```