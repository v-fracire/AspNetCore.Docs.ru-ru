---
title: Многоразовый интерфейс Razor в библиотеках классов в ASP.NET Core
author: Rick-Anderson
description: Описание способов создания многоразового пользовательского интерфейса Razor в библиотеке классов.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/21/2018
uid: razor-pages/ui-class
ms.openlocfilehash: 1f0ef59ce3f3294d6a3bde015ca34800770b1be4
ms.sourcegitcommit: e955a722c05ce2e5e21b4219f7d94fb878e255a6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/31/2018
ms.locfileid: "42910121"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Создание многоразового пользовательского интерфейса с помощью проекта библиотеки классов Razor в ASP.NET Core.

Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)

Представления Razor, страницы, контроллеры, модели страниц, [Просмотр компонентов](xref:mvc/views/view-components) и модели данных можно создавать в библиотеке классов Razor (RCL). RCL можно упаковать и использовать повторно. Приложения могут включать RCL и переопределять содержащиеся в нем представления и страницы. При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в RCL приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении.

Эта функция требует [!INCLUDE[](~/includes/2.1-SDK.md)]

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Создание библиотеки классов с пользовательским интерфейсом Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.
* Выберите **Новое веб-приложение ASP.NET Core**.
* Назовите библиотеку (например, "RazorClassLib") > **OK**. Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.
* Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.
* Выберите **Библиотека классов Razor** > **OK**.

Библиотеки классов Razor имеет следующий файл проекта:

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Выполните из командной строки команду `dotnet new razorclasslib`. Пример:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
```

Дополнительные сведения см. в разделе [dotnet new](/dotnet/core/tools/dotnet-new). Чтобы избежать конфликта имени файла с созданной библиотекой представлений, проверьте, что имя библиотеки не заканчивается на `.Views`.

------
Добавление файлов Razor в RCL.

Шаблоны ASP.NET Core считает содержимое RCL *областей* папки. См. в разделе [макет страниц RCL](#afs) для создания содержимого в RCL, который предоставляет `~/Pages` вместо `~/Areas/Pages`.

## <a name="referencing-razor-class-library-content"></a>Создание ссылок на содержимое библиотеки классов Razor

На RCL могут ссылаться:

* Пакет NuGet. См. [Создание пакетов NuGet](/nuget/create-packages/creating-a-package), [dotnet add package](/dotnet/core/tools/dotnet-add-package) и [Создание и публикация пакета NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName}.csproj*. См. [dotnet-add reference](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-a-razor-class-library-project-and-use-from-a-razor-pages-project"></a>Пошаговое руководство. Создание проекта библиотеки классов Razor и использование в проекте Razor Pages

Вы можете не создавать, а загрузить [целый проект](https://github.com/aspnet/Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) и протестировать его. Образец загрузки содержит дополнительный код и ссылки, что упрощает тестирование проекта. Оставьте свой комментарий о сравнении образцов загрузки с пошаговыми инструкциями в [этой проблеме GitHub](https://github.com/aspnet/Docs/issues/6098).

### <a name="test-the-download-app"></a>Тестирование приложения загрузки

Если вы еще не загрузили завершенное приложение и вместо этого хотите создать проект пошагового руководства, перейдите к [следующему разделу](#create-a-razor-class-library).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Откройте *SLN*-файл в Visual Studio. Запустите приложение.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

В командной строке в каталоге *cli* создайте RCL и веб-приложение.

``` CLI
dotnet build
```

Перейдите в каталог *WebApp1* и запустите приложение:

``` CLI
dotnet run
```
------

Следуйте инструкциям в разделе [Тестирование WebApp1](#test)

## <a name="create-a-razor-class-library"></a>Создание библиотеки классов Razor

В этом разделе создается библиотека классов Razor (RCL). Файлы Razor будут добавлены в RCL.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Создание проекта RCL:

* В меню **Файл** Visual Studio откройте меню **Создать** > **Проект**.
* Выберите **Новое веб-приложение ASP.NET Core**.
* Присвойте приложению имя **RazorUIClassLib** > **ОК**.
* Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.
* Выберите **Библиотека классов Razor** > **OK**.
* Добавьте файл частичного представления Razor с именем *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Выполните следующую команду в командной строке:

``` CLI
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Предыдущие команды:

* Создает библиотеку классов Razor (RCL) `RazorUIClassLib`.
* Создает страницу Razor _Message и добавляет ее в RCL. Параметр `-np` создает страницу без `PageModel`.
* Создает файл [viewstart](xref:mvc/views/layout#running-code-before-each-view) и добавляет его в RCL.

Файл viewstart требуется для использования макета проекта Razor Pages (который добавляется в следующем разделе).

------

### <a name="add-razor-files-and-folders-to-the-project"></a>Добавление файлов и папок Razor в проект.

* Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* следующим кодом:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Замените разметку в *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* следующим кодом:

[!code-html[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` требуется для использования частичного представления (`<partial name="_Message" />`). Вместо включения директивы `@addTagHelper` можно добавить файл *_ViewImports.cshtml*. Пример:

``` CLI
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

Дополнительные сведения о файле viewimports см. в разделе [Импорт общих директив](xref:mvc/views/layout#importing-shared-directives)

* Создайте библиотеку классов, чтобы убедиться в отсутствии ошибок компилятора:

``` CLI
dotnet build RazorUIClassLib
```

Выходные данные сборки содержат библиотеки *RazorUIClassLib.dll* и *RazorUIClassLib.Views.dll*. В библиотеке *RazorUIClassLib.Views.dll* находится скомпилированное содержимое Razor.

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Использование библиотеки пользовательского интерфейса Razor в проекте Razor Pages

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Создание веб-приложения Razor Pages:

* В **обозревателе решений** щелкните решение правой кнопкой мыши > **Добавить** >  **Новый проект**.
* Выберите **Новое веб-приложение ASP.NET Core**.
* Назовите приложение **WebApp1**.
* Убедитесь, что выбрано **ASP.NET Core 2.1** или более поздней версии.
* Выберите **Веб-приложение** > **OK**.

* В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Назначить запускаемым проектом**.
* В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Зависимости сборки** > **Зависимости проекта**.
* Отметьте **RazorUIClassLib** как зависимость от **WebApp1**.
* В **обозревателе решений** щелкните правой кнопкой мыши **WebApp1** и выберите **Добавить** > **Ссылка**.
* В диалоговом окне **Диспетчер ссылок** нажмите **RazorUIClassLib** > **ОК**.

Запустите приложение.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Создайте веб-приложение Razor Pages и файл решения, содержащий приложение Razor Pages и библиотеку классов Razor:

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

Выполните сборку и запустите приложения:

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a>Тестирование WebApp1

Убедитесь, что используется библиотека классов пользовательского интерфейса Razor.

* Перейдите по адресу `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Переопределение представлений, частичных представлений и страниц

При обнаружении представления, частичного представления или страницы Razor и в веб-приложении, и в библиотеке классов Razor приоритет имеет разметка Razor (файл *.cshtml*) в веб-приложении. Например, если вы добавите *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* в WebApp1, Page1 в WebApp1 будет иметь приоритет над Page1 в библиотеке классов Razor.

В примере загрузки переименуйте *WebApp1/Areas/MyFeature2* в *WebApp1/Areas/MyFeature*, чтобы протестировать приоритет.

Скопируйте частичное представление *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* в *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Обновите разметку, чтобы указать новое расположение. Скомпилируйте и запустите приложение, чтобы убедиться, что используется версия частичного представления из приложения.

<a name="afs"></a>

### <a name="rcl-pages-layout"></a>Макет страниц RCL

Для ссылки на содержимое RCL, как будто он является частью папки страниц веб приложения, создайте проект RCL со следующей структурой: файл:

* *RazorUIClassLib/страниц*
* *RazorUIClassLib/страниц/Shared*

Предположим, что *RazorUIClassLib/страниц/Shared* содержит два неполных файлов *_Header.cshtml* и *_Footer.cshtml*. <partial> Удалось добавить теги *_Layout.cshtml* файла: 
  
```
  <body>
    <partial name="_Header">
    @RenderBody()
    <partial name="_Footer">
  </body>
```