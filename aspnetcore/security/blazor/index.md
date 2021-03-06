---
title: Проверка подлинности и авторизация в ASP.NET Core Blazor
author: guardrex
description: Сведения о проверке подлинности и авторизации в Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2020
no-loc:
- Blazor
- SignalR
uid: security/blazor/index
ms.openlocfilehash: ba00c0370094df4e0ae0479aeb23eceea6ef04f0
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083347"
---
# <a name="aspnet-core-blazor-authentication-and-authorization"></a>Аутентификация и авторизация в ASP.NET Core Blazor

Автор: [Стив Сандерсон](https://github.com/SteveSandersonMS) (Steve Sanderson)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

ASP.NET Core поддерживает настройку и администрирование средств безопасности в приложениях Blazor.

Сценарии безопасности для серверных приложений Blazor и приложений Blazor WebAssembly отличаются. Так как серверные приложения Blazor выполняются на стороне сервера, проверки авторизации могут определить следующее:

* параметры пользовательского интерфейса, предоставленные пользователю (например, какие пункты меню доступны пользователю);
* правила доступа для приложений и компонентов.

Приложения Blazor WebAssembly выполняются на стороне клиента. В этом случае авторизация используется *только* для определения отображаемых вариантов пользовательского интерфейса. Так как пользователь может изменить или обойти проверки на стороне клиента, приложение Blazor WebAssembly не может применять правила авторизации доступа.

## <a name="authentication"></a>Проверка подлинности

Blazor использует существующие механизмы аутентификации ASP.NET Core для установления личности пользователя. Конкретный механизм зависит от того, как размещается приложение Blazor (серверное приложение Blazor или приложение Blazor WebAssembly).

### <a name="blazor-server-authentication"></a>Аутентификация в приложении Blazor Server

Серверные приложения Blazor работают через подключение в реальном времени, созданное с помощью SignalR. [Аутентификация в приложениях на основе SignalR](xref:signalr/authn-and-authz) выполняется при установлении подключения. Аутентификация может выполняться на основе файлов cookie или других маркеров носителя.

Шаблон серверного проекта Blazor позволяет автоматически настроить аутентификацию при создании проекта.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Следуйте указаниям по работе с Visual Studio (<xref:blazor/get-started>), чтобы создать проект Blazor на стороне сервера с механизмом аутентификации.

Выбрав шаблон **Серверное приложение Blazor** в диалоговом окне **Создать веб-приложение ASP.NET Core**, щелкните **Изменить** в разделе **Проверка подлинности**.

Откроется диалоговое окно с тем же набором механизмов аутентификации, которые доступны для других проектов ASP.NET Core.

* **Без аутентификации**.
* **Учетные записи отдельных пользователей**, которые можно хранить следующим образом:
  * в приложении с помощью системы [удостоверений](xref:security/authentication/identity) ASP.NET Core;
  * в [Azure AD B2C](xref:security/authentication/azure-ad-b2c);
* **рабочие или учебные учетные записи**.
* **Проверка подлинности Windows**

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Следуйте указаниям по работе с Visual Studio Code (<xref:blazor/get-started>), чтобы создать проект Blazor на стороне сервера с механизмом аутентификации.

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

Допустимые значения аутентификации (`{AUTHENTICATION}`) перечислены в следующей таблице.

| Механизм аутентификации                                                                 | Значение`{AUTHENTICATION}` |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| Без аутентификации                                                                        | `None`                   |
| Индивидуальное лицо<br>Пользователи, сохраненные в приложении с помощью ASP.NET Core Identity                        | `Individual`             |
| Индивидуальное лицо<br>Пользователи, сохраненные в [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Рабочие или учебные учетные записи<br>Корпоративная аутентификация для отдельного клиента.            | `SingleOrg`              |
| Рабочие или учебные учетные записи<br>Корпоративная аутентификация для нескольких клиентов.           | `MultiOrg`               |
| Проверка подлинности Windows                                                                   | `Windows`                |

Эта команда создает папку, в качестве имени для которой берется значение, предоставленное вместо заполнителя `{APP NAME}`, а затем использует имя папки в качестве имени приложения. Подробнее см. статью о команде [dotnet new](/dotnet/core/tools/dotnet-new) в руководстве по .NET Core.

<!--

# [Visual Studio for Mac](#tab/visual-studio-mac)

1. Follow the Visual Studio for Mac guidance in the <xref:blazor/get-started> article.

1.

1.

-->

<!--
# [.NET Core CLI](#tab/netcore-cli/)

Follow the .NET Core CLI guidance in the <xref:blazor/get-started> article to create a new Blazor Server project with an authentication mechanism:

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

Permissible authentication values (`{AUTHENTICATION}`) are shown in the following table.

| Authentication mechanism                                                                 | `{AUTHENTICATION}` value |
| ---------------------------------------------------------------------------------------- | :----------------------: |
| No Authentication                                                                        | `None`                   |
| Individual<br>Users stored in the app with ASP.NET Core Identity.                        | `Individual`             |
| Individual<br>Users stored in [Azure AD B2C](xref:security/authentication/azure-ad-b2c). | `IndividualB2C`          |
| Work or School Accounts<br>Organizational authentication for a single tenant.            | `SingleOrg`              |
| Work or School Accounts<br>Organizational authentication for multiple tenants.           | `MultiOrg`               |
| Windows Authentication                                                                   | `Windows`                |

The command creates a folder named with the value provided for the `{APP NAME}` placeholder and uses the folder name as the app's name. For more information, see the [dotnet new](/dotnet/core/tools/dotnet-new) command in the .NET Core Guide.

-->

---

### <a name="opno-locblazor-webassembly-authentication"></a>Проверка подлинности в Blazor WebAssembly

В приложениях Blazor WebAssembly проверку подлинности можно обойти, так как пользователь может изменять весь код на стороне клиента. Это же справедливо для всех технологий на стороне клиента, включая платформы одностраничного приложения JavaScript или собственных приложений для любой операционной системы.

Добавьте ссылку пакета для [Microsoft.AspNetCore.Components.Authorization](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Authorization/) в файл проекта приложения.

Реализация пользовательской службы `AuthenticationStateProvider` для приложений Blazor WebAssembly рассматривается в следующих разделах.

## <a name="authenticationstateprovider-service"></a>Служба AuthenticationStateProvider

Серверные приложения Blazor включают встроенную службу `AuthenticationStateProvider`, которая получает данные о состоянии проверки подлинности из `HttpContext.User` в ASP.NET Core. Так состояние аутентификации интегрируется с существующими соответствующими механизмами ASP.NET Core на стороне сервера.

`AuthenticationStateProvider` является базовой службой, которую компоненты `AuthorizeView` и `CascadingAuthenticationState` используют для получения состояния аутентификации.

Обычно вы не используете `AuthenticationStateProvider` напрямую. Выберите подход на основе [компонента AuthorizeView](#authorizeview-component) или [задачи<AuthenticationState>](#expose-the-authentication-state-as-a-cascading-parameter), которые описаны далее в этой статье. Основной недостаток при использовании `AuthenticationStateProvider` напрямую заключается в том, что компонент не получает автоматического уведомления при изменении базовых данных о состоянии аутентификации.

Служба `AuthenticationStateProvider` может предоставить данные <xref:System.Security.Claims.ClaimsPrincipal> о текущем пользователе, как показано в следующем примере:

```razor
@page "/"
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthenticationStateProvider

<button @onclick="LogUsername">Write user info to console</button>

@code {
    private async Task LogUsername()
    {
        var authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

Если `user.Identity.IsAuthenticated` имеет значение `true`, а пользователь является <xref:System.Security.Claims.ClaimsPrincipal>, можно перечислить утверждения и оценить членство в ролях.

Дополнительные сведения о внедрении зависимостей и службах см. в статьях <xref:blazor/dependency-injection> и <xref:fundamentals/dependency-injection>.

## <a name="implement-a-custom-authenticationstateprovider"></a>Реализация пользовательского AuthenticationStateProvider

Если вы создаете приложение Blazor WebAssembly или спецификация приложения предусматривает строгое требование включить пользовательский поставщик, создайте реализацию этого поставщика и переопределите `GetAuthenticationStateAsync` следующим образом:

```csharp
using System.Security.Claims;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Authorization;

namespace BlazorSample.Services
{
    public class CustomAuthStateProvider : AuthenticationStateProvider
    {
        public override Task<AuthenticationState> GetAuthenticationStateAsync()
        {
            var identity = new ClaimsIdentity(new[]
            {
                new Claim(ClaimTypes.Name, "mrfibuli"),
            }, "Fake authentication type");

            var user = new ClaimsPrincipal(identity);

            return Task.FromResult(new AuthenticationState(user));
        }
    }
}
```

В приложении WebAssembly Blazor служба `CustomAuthStateProvider` регистрируется в `Main` в файле *Program.cs*:

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Microsoft.AspNetCore.Components.Authorization;
using Microsoft.Extensions.DependencyInjection;
using BlazorSample.Services;

public class Program
{
    public static async Task Main(string[] args)
    {
        var builder = WebAssemblyHostBuilder.CreateDefault(args);
        builder.Services.AddScoped<AuthenticationStateProvider, 
            CustomAuthStateProvider>();
        builder.RootComponents.Add<App>("app");

        await builder.Build().RunAsync();
    }
}
```

С помощью `CustomAuthStateProvider`, все пользователи проходят аутентификацию с использованием имени пользователя `mrfibuli`.

## <a name="expose-the-authentication-state-as-a-cascading-parameter"></a>Предоставление состояния аутентификации в качестве каскадного параметра

Если процедурная логика требует данных о состоянии аутентификации, например при выполнении активируемых пользователем действий, для получения таких данных определите каскадный параметр типа `Task<AuthenticationState>`:

```razor
@page "/"

<button @onclick="LogUsername">Log username</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task LogUsername()
    {
        var authState = await authenticationStateTask;
        var user = authState.User;

        if (user.Identity.IsAuthenticated)
        {
            Console.WriteLine($"{user.Identity.Name} is authenticated.");
        }
        else
        {
            Console.WriteLine("The user is NOT authenticated.");
        }
    }
}
```

> [!NOTE]
> В компоненте приложения Blazor WebAssembly добавьте пространство имен `Microsoft.AspNetCore.Components.Authorization` (`@using Microsoft.AspNetCore.Components.Authorization`).

Если `user.Identity.IsAuthenticated` имеет значение `true`, можно перечислить утверждения и оценить членство в ролях.

Настройте каскадный параметр `Task<AuthenticationState>` с помощью компонентов `AuthorizeRouteView` и `CascadingAuthenticationState` в файле *App.razor*:

```razor
@using Microsoft.AspNetCore.Components.Authorization

<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

Добавьте службы для параметров и авторизации в `Program.Main`:

```csharp
builder.Services.AddOptions();
builder.Services.AddAuthorizationCore();
```

## <a name="authorization"></a>Авторизация

После аутентификации пользователя применяются правила *авторизации*, которые определяют доступные этому пользователю действия.

Доступ обычно предоставляется или запрещается в зависимости от следующих аспектов:

* выполнена ли аутентификация (выполнен ли вход);
* есть ли у пользователя определенная *роль*;
* есть ли у пользователя определенное *утверждение*;
* выполняются ли требования *политики*.

Каждый из этих аспектов применяется здесь так же, как в приложениях ASP.NET Core MVC или Razor Pages. Дополнительные сведения о безопасности в ASP.NET Core вы найдете в статьях [о безопасности и идентификации в ASP.NET Core](xref:security/index).

## <a name="authorizeview-component"></a>Компонент AuthorizeView

Компонент `AuthorizeView` избирательно демонстрирует элементы пользовательского интерфейса в зависимости от того, имеет ли пользователь права на доступ к этим элементам. Этот подход полезен, если вам нужно просто *отображать* пользователю данные, и не использовать удостоверение пользователя в процедурной логике.

Этот компонент представляет переменную `context` типа `AuthenticationState`, которую можно использовать для доступа к сведениям о пользователе, выполнившем вход:

```razor
<AuthorizeView>
    <h1>Hello, @context.User.Identity.Name!</h1>
    <p>You can only see this content if you're authenticated.</p>
</AuthorizeView>
```

Также вы можете указать другое содержимое, которое будет отображаться, если пользователь не выполнил аутентификацию:

```razor
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <NotAuthorized>
        <h1>Authentication Failure!</h1>
        <p>You're not signed in.</p>
    </NotAuthorized>
</AuthorizeView>
```

Содержимое тегов `<Authorized>` и `<NotAuthorized>` может включать произвольные элементы, например другие интерактивные компоненты.

Условия авторизации, такие как роли или правила для выбора вариантов пользовательского интерфейса и доступа к ним, описаны в разделе [об авторизации](#authorization).

Если условия авторизации не указаны, `AuthorizeView` использует политику по умолчанию со следующими правилами:

* выполнившие аутентификацию (выполнившие вход) пользователи считаются авторизованными;
* не выполнившие аутентификацию (не выполнившие вход) пользователи считаются не авторизованными.

### <a name="role-based-and-policy-based-authorization"></a>Авторизация на основе ролей и политик

Компонент `AuthorizeView` поддерживает авторизацию на основе *ролей* или *политик*.

Для авторизации на основе ролей примените параметр `Roles`:

```razor
<AuthorizeView Roles="admin, superuser">
    <p>You can only see this if you're an admin or superuser.</p>
</AuthorizeView>
```

Для получения дополнительной информации см. <xref:security/authorization/roles>.

Для авторизации на основе политик примените параметр `Policy`:

```razor
<AuthorizeView Policy="content-editor">
    <p>You can only see this if you satisfy the "content-editor" policy.</p>
</AuthorizeView>
```

Авторизация на основе утверждений считается особым случаем авторизации на основе политик. Например, вы можете определить политику, которая требует наличия определенного утверждения у пользователя. Для получения дополнительной информации см. <xref:security/authorization/policies>.

Эти API могут использоваться в серверных приложениях Blazor и приложениях Blazor WebAssembly.

Если не указано ни `Roles`, ни `Policy`, `AuthorizeView` использует политику по умолчанию.

### <a name="content-displayed-during-asynchronous-authentication"></a>Содержимое, отображаемое при асинхронной аутентификации

Blazor позволяет *асинхронно* определять состояние проверки подлинности. Основной сценарий для такого подхода — приложение Blazor WebAssembly, которое направляет запрос на проверку подлинности во внешнюю конечную точку.

Пока аутентификация выполняется, `AuthorizeView` по умолчанию не отображает содержимое. Чтобы содержимое отображалось, используйте элемент `<Authorizing>`:

```razor
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you're authenticated.</p>
    </Authorized>
    <Authorizing>
        <h1>Authentication in progress</h1>
        <p>You can only see this content while authentication is in progress.</p>
    </Authorizing>
</AuthorizeView>
```

Такой подход обычно не применим к серверным приложениям Blazor. Серверные приложения Blazor узнают состояние проверки подлинности, как только оно устанавливается. Содержимое `Authorizing` можно указать в компоненте `AuthorizeView` для серверного приложения Blazor, но это содержимое никогда не отображается.

## <a name="authorize-attribute"></a>Атрибут [Authorize]

Атрибут `[Authorize]` можно использовать в компонентах Razor:

```razor
@page "/"
@attribute [Authorize]

You can only see this if you're signed in.
```

> [!NOTE]
> В компоненте приложения Blazor WebAssembly добавьте пространство имен `Microsoft.AspNetCore.Authorization` (`@using Microsoft.AspNetCore.Authorization`) к примерам в этом разделе.

> [!IMPORTANT]
> Используйте `[Authorize]` только для компонентов `@page`, полученных через маршрутизатор Blazor. Авторизация выполняется только как аспект маршрутизации и *не* для дочерних компонентов, которые отображаются на странице. Чтобы разрешить отображение конкретных частей на странице, используйте вместо этого `AuthorizeView`.

Атрибут `[Authorize]` также поддерживает авторизацию на основе ролей или политик. Для авторизации на основе ролей примените параметр `Roles`:

```razor
@page "/"
@attribute [Authorize(Roles = "admin, superuser")]

<p>You can only see this if you're in the 'admin' or 'superuser' role.</p>
```

Для авторизации на основе политик примените параметр `Policy`:

```razor
@page "/"
@attribute [Authorize(Policy = "content-editor")]

<p>You can only see this if you satisfy the 'content-editor' policy.</p>
```

Если не указано ни `Roles`, ни `Policy`, `[Authorize]` использует политику по умолчанию со следующими правилами:

* выполнившие аутентификацию (выполнившие вход) пользователи считаются авторизованными;
* не выполнившие аутентификацию (не выполнившие вход) пользователи считаются не авторизованными.

## <a name="customize-unauthorized-content-with-the-router-component"></a>Настройка несанкционированного содержимого для компонента маршрутизатора

Компонент `Router` вместе с компонентом `AuthorizeRouteView` позволяет приложению указать пользовательское содержимое для следующих ситуаций:

* содержимое не найдено;
* пользователь не удовлетворяет условию `[Authorize]`, которое применено к компоненту (атрибут `[Authorize]` описан в разделе [Атрибут `[Authorize]`](#authorize-attribute));
* выполняется асинхронная аутентификация.

В стандартном шаблоне проекта Blazor Server есть файл *App.razor* с примером пользовательского содержимого:

```razor
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <AuthorizeRouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)">
            <NotAuthorized>
                <h1>Sorry</h1>
                <p>You're not authorized to reach this page.</p>
                <p>You may need to log in as a different user.</p>
            </NotAuthorized>
            <Authorizing>
                <h1>Authentication in progress</h1>
                <p>Only visible while authentication is in progress.</p>
            </Authorizing>
        </AuthorizeRouteView>
    </Found>
    <NotFound>
        <CascadingAuthenticationState>
            <LayoutView Layout="@typeof(MainLayout)">
                <h1>Sorry</h1>
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </CascadingAuthenticationState>
    </NotFound>
</Router>
```

Содержимое тегов `<NotFound>`, `<NotAuthorized>` и `<Authorizing>` может включать произвольные элементы, например другие интерактивные компоненты.

Если элемент `<NotAuthorized>` не указан, `AuthorizeRouteView` использует следующее резервное сообщение:

```html
Not authorized.
```

## <a name="notification-about-authentication-state-changes"></a>Уведомление об изменении состояния аутентификации

Если приложение определяет, что изменились базовые данные о состоянии аутентификации (например, пользователь вышел из системы или другой пользователь изменил роль), `AuthenticationStateProvider` пользователя может вызвать необязательный метод `NotifyAuthenticationStateChanged` из базового класса `AuthenticationStateProvider`. Это действие информирует потребителей данных состояния аутентификации (таких, как `AuthorizeView`) о необходимости повторно отображения с учетом новых данных.

## <a name="procedural-logic"></a>Процедурная логика

Если приложению нужно проверять правила авторизации в составе процедурной логики, используйте каскадный параметр с типом `Task<AuthenticationState>`, чтобы получить <xref:System.Security.Claims.ClaimsPrincipal> пользователя. `Task<AuthenticationState>` можно комбинировать для оценки политик с другими службами, например `IAuthorizationService`.

```razor
@inject IAuthorizationService AuthorizationService

<button @onclick="@DoSomething">Do something important</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task DoSomething()
    {
        var user = (await authenticationStateTask).User;

        if (user.Identity.IsAuthenticated)
        {
            // Perform an action only available to authenticated (signed-in) users.
        }

        if (user.IsInRole("admin"))
        {
            // Perform an action only available to users in the 'admin' role.
        }

        if ((await AuthorizationService.AuthorizeAsync(user, "content-editor"))
            .Succeeded)
        {
            // Perform an action only available to users satisfying the 
            // 'content-editor' policy.
        }
    }
}
```

> [!NOTE]
> В компоненте приложения Blazor WebAssembly добавьте пространства имен `Microsoft.AspNetCore.Authorization` и `Microsoft.AspNetCore.Components.Authorization`:
>
> ```razor
> @using Microsoft.AspNetCore.Authorization
> @using Microsoft.AspNetCore.Components.Authorization
> ```

## <a name="authorization-in-opno-locblazor-webassembly-apps"></a>Авторизация в приложениях Blazor WebAssembly

В приложениях Blazor WebAssembly авторизацию можно обойти, так как пользователь может изменять весь код на стороне клиента. Это же справедливо для всех технологий на стороне клиента, включая платформы одностраничного приложения JavaScript или собственных приложений для любой операционной системы.

**Всегда выполняйте проверки авторизации на стороне сервера в конечных точках API, к которым обращается клиентское приложение.**

Дополнительные сведения см. в статьях в документе <xref:security/blazor/webassembly/index>.

## <a name="troubleshoot-errors"></a>Устранение неполадок

Распространенные ошибки

* **Для авторизации требуется каскадный параметр с типом \<AuthenticationState>. Попробуйте предоставить его с помощью CascadingAuthenticationState.**

* **Для `authenticationStateTask` возвращается значение `null`**

Вполне вероятно, что проект не был создан на основе шаблона серверного приложения Blazor с включенной проверкой подлинности. Примените `<CascadingAuthenticationState>` в качестве оболочки некоторой части дерева пользовательского интерфейса, например в *App.razor*, следующим образом:

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="typeof(Startup).Assembly">
        ...
    </Router>
</CascadingAuthenticationState>
```

`CascadingAuthenticationState` предоставляет каскадный параметр `Task<AuthenticationState>`, который он в свою очередь получает из базовой службы внедрения зависимостей `AuthenticationStateProvider`.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:security/index>
* <xref:security/blazor/server>
* <xref:security/authentication/windowsauth>
* [Awesome Blazor: проверка подлинности](https://github.com/AdrienTorris/awesome-blazor#authentication) — ссылки на примеры от сообщества
