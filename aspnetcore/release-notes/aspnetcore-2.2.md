---
title: Новые возможности ASP.NET Core 2.2
author: tdykstra
description: Сведения о новых возможностях ASP.NET Core 2.2.
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/18/2018
uid: aspnetcore-2.2
ms.openlocfilehash: 13d7dec834a5661b445b4fc0c0be8be9b7b41b9e
ms.sourcegitcommit: 816f39e852a8f453e8682081871a31bc66db153a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/19/2018
ms.locfileid: "53637733"
---
# <a name="whats-new-in-aspnet-core-22"></a>Новые возможности ASP.NET Core 2.2

В этой статье описываются наиболее важные изменения в ASP.NET Core 2.2 со ссылками на соответствующую документацию.

## <a name="open-api-analyzers--conventions"></a>Соглашения и анализаторы Open API

Open API (или Swagger) — это не зависящая от языка спецификация для описания REST API. В экосистеме Open API представлены средства для обнаружения, тестирования и создания кода клиента в соответствии со спецификацией. Поддержка создания и визуализации документов Open API в ASP.NET Core MVC обеспечивается за счет управляемых сообществом проектов, таких как [NSwag](https://github.com/RSuter/NSwag) и [Swashbuckle.AspNetCore](https://github.com/domaindrivendev/Swashbuckle.AspNetCore). В ASP.NET Core 2.2 представлены улучшенные средства и возможности среды выполнения для создания документов Open API.

Дополнительные сведения см. в следующих ресурсах:

* <xref:web-api/advanced/analyzers>
* <xref:web-api/advanced/conventions>
* [ASP.NET Core 2.2.0, предварительная версия 1: соглашения и анализаторы Open API](https://blogs.msdn.microsoft.com/webdev/2018/08/23/asp-net-core-2-20-preview1-open-api-analyzers-conventions/)

## <a name="problem-details-support"></a>Поддержка сведений о проблеме

В ASP.NET Core 2.1 был представлен объект `ProblemDetails`, основанный на спецификации RFC 7807 для передачи сведений об ошибке в составе HTTP-ответа. В версии 2.2 `ProblemDetails` является стандартным ответом для кодов ошибок клиента в контроллерах с атрибутами `ApiControllerAttribute`. `IActionResult`, возвращающий код состояния ошибки клиента (4xx), теперь возвращает текст `ProblemDetails`. В результат также включается идентификатор корреляции, который может использоваться для сопоставления ошибки с помощью журналов запросов. В отношении ошибок клиентов `ProducesResponseType` по умолчанию использует тип ответа `ProblemDetails`. Это описывается в выходных данных Open API / Swagger, создаваемых с помощью NSwag или Swashbuckle.AspNetCore.

## <a name="endpoint-routing"></a>Маршрутизация конечных точек

В ASP.NET Core 2.2 используется новая система *маршрутизации конечных точек*, обеспечивающая оптимизированную диспетчеризацию запросов. Среди изменений представлены новые члены для создания ссылок API и преобразователи параметров маршрута.

Дополнительные сведения см. в следующих ресурсах:

* [Маршрутизация конечных точек в версии 2.2](https://blogs.msdn.microsoft.com/webdev/2018/08/27/asp-net-core-2-2-0-preview1-endpoint-routing/)
* [Преобразователи параметров маршрута](https://www.hanselman.com/blog/ASPNETCore22ParameterTransformersForCleanURLGenerationAndSlugsInRazorPagesOrMVC.aspx) (см. раздел **Маршрутизация**)
* [Различия между IRouter и маршрутизацией на основе конечных точек](xref:fundamentals/routing?view=aspnetcore-2.2#differences-from-earlier-versions-of-routing)

## <a name="health-checks"></a>Проверки работоспособности

Новая служба проверки работоспособности упрощает использование ASP.NET Core в средах с обязательной проверкой работоспособности, таких как Kubernetes. Служба проверки работоспособности включает ПО промежуточного слоя и набор библиотек, которые определяют абстракцию `IHealthCheck` и службу.

Проверки работоспособности используются подсистемой балансировки нагрузки или оркестратором контейнеров для быстрого определения того, насколько эффективно система отвечает на запросы. Оркестратор контейнеров может реагировать на неуспешную проверку работоспособности, остановив последовательное развертывание или перезапустив контейнер. Подсистема балансировки нагрузки может реагировать на результаты проверки работоспособности путем перенаправления трафика от неисправного экземпляра службы.

Проверки работоспособности предоставляются приложением в качестве конечной точки HTTP, используемой системами мониторинга. Проверки работоспособности можно настроить для различных сценариев мониторинга в реальном времени и систем мониторинга. Проверки работоспособности интегрируются с [проектом BeatPulse](https://github.com/Xabaril/BeatPulse). Это значительно упрощает добавление проверок в десятки распространенных систем и зависимых компонентов.

Дополнительные сведения см. в статье [Проверки работоспособности в ASP.NET Core](xref:host-and-deploy/health-checks).

## <a name="http2-in-kestrel"></a>HTTP/2 в Kestrel

В ASP.NET Core 2.2 добавлена поддержка HTTP/2. 

HTTP/2 является основной редакцией HTTP-протокола. Заметными нововведениями в HTTP/2 стали поддержка сжатия заголовков и полное мультиплексирование потоков в рамках одного соединения. Версия HTTP/2 сохраняет семантику HTTP (методы, заголовки HTTP и т. д.), однако она заметно отличается от HTTP/1.x в отношении механизмов кадрирования и передачи данных по сети.

В связи с этим изменением механизмов кадрирования серверам и клиентам необходимо согласовывать используемую версию протокола. Согласование протоколов на уровне приложений (Application-Layer Protocol Negotiation, ALPN) — это расширение TLS, с помощью которого сервер и клиент могут согласовать версию протокола в рамках процесса подтверждения TLS. Даже при наличии у сервера и клиента сведений о версии протокола все основные браузеры поддерживают ALPN как единственное средство для установления соединения по протоколу HTTP/2.

Дополнительные сведения см. в статье [Поддержка HTTP/2](xref:fundamentals/servers/index?view=aspnetcore-2.2#http2-support).

## <a name="kestrel-configuration"></a>Настройка Kestrel

В предыдущих версиях ASP.NET Core настройка параметров Kestrel осуществлялась путем вызова `UseKestrel`. В версии 2.2 для настройки параметров Kestrel вызывается метод `ConfigureKestrel` в построителе узла. Это изменение позволяет устранить проблему с порядком регистрации `IServer` для внутрипроцессного размещения. Дополнительные сведения см. в следующих ресурсах:

* [Устранение конфликта UseIIS](https://github.com/aspnet/KestrelHttpServer/issues/2760)
* [Настройка параметров сервера Kestrel с помощью ConfigureKestrel](xref:fundamentals/servers/kestrel?view=aspnetcore-2.2#how-to-use-kestrel-in-aspnet-core-apps)

## <a name="iis-in-process-hosting"></a>Внутрипроцессное размещение в службах IIS

В более ранних версиях ASP.NET Core службы IIS выступают в качестве обратного прокси-сервера. В версии 2.2 модуль ASP.NET Core может загружать CoreCLR и размещать приложение в рабочем процессе IIS (*w3wp.exe*). Внутрипроцессное размещение позволяет оптимизировать производительность и диагностику при работе со службами IIS.

Дополнительные сведения см. в статье [Внутрипроцессное размещение для служб IIS](xref:host-and-deploy/aspnet-core-module?view=aspnetcore-2.2#in-process-hosting-model).

## <a name="signalr-java-client"></a>Клиент Java для SignalR

В ASP.NET Core 2.2 представлен клиент Java для SignalR. Этот клиент поддерживает подключение к серверу SignalR ASP.NET Core из кода Java, в том числе из приложений Android.

Дополнительные сведения см. в статье [Клиент Java для SignalR ASP.NET Core](https://docs.microsoft.com/aspnet/core/signalr/java-client?view=aspnetcore-2.2).

## <a name="cors-improvements"></a>Усовершенствования CORS

В предыдущих версия ASP.NET Core ПО промежуточного слоя CORS обеспечивало отправку заголовков `Accept`, `Accept-Language`, `Content-Language` и `Origin` независимо от значений, настроенных в `CorsPolicy.Headers`. В версии 2.2 соблюдение политики ПО промежуточного слоя CORS возможно только в том случае, если отправляемые в `Access-Control-Request-Headers` заголовки точно соответствуют заголовкам, указанным в `WithHeaders`.

Дополнительные сведения см. в статье [ПО промежуточного слоя CORS](xref:security/cors?view=aspnetcore-2.2#set-the-allowed-request-headers).

## <a name="response-compression"></a>Сжатие ответов

ASP.NET Core 2.2 поддерживает сжатие ответов с использованием [формата сжатия Brotli](https://tools.ietf.org/html/rfc7932).

Дополнительные сведения см. в статье [Поддержка сжатия Brotli для сжатия ответов в ПО промежуточного слоя](xref:performance/response-compression?view=aspnetcore-2.2#brotli-compression-provider).

## <a name="project-templates"></a>Шаблоны проектов

Шаблоны веб-проектов ASP.NET Core обновлены до [Bootstrap 4](https://getbootstrap.com/docs/4.1/migration/) и [Angular 6](https://blog.angular.io/version-6-of-angular-now-available-cc56b0efa7a4). Новое оформление выглядит проще и позволяет более эффективно отображать важные структуры приложения.

![Домашняя или индексная страница](~/tutorials/razor-pages/razor-pages-start/_static/home2.2.png)

## <a name="validation-performance"></a>Производительность проверок

Модель MVC имеет расширяемую гибкую систему проверки, позволяющую на основе запросов определять, какие средства проверки применяются к конкретной модели. Эта возможность отлично подходит для разработки сложных поставщиков служб проверки. Тем не менее в большинстве распространенных случаев приложение использует только встроенные средства проверки и не нуждается в таких дополнительных возможностях. К встроенным средствам проверки относятся заметки к данным, такие как [Required] и [StringLength], а также `IValidatableObject`.

В ASP.NET Core 2.2 модель MVC может обойти проверку в случаях, когда определяется, что для указанного графа модели проверка не требуется. Пропуск проверки приводит к существенным улучшениям для моделей, в которых отсутствуют или не могут использоваться средства проверки. К ним относятся такие объекты, как коллекции примитивов (например, `byte[]`, `string[]`, `Dictionary<string, string>`) или сложные графы объектов с небольшим количеством средств проверки.

## <a name="http-client-performance"></a>Производительность клиента HTTP

В ASP.NET Core 2.2 была повышена производительность `SocketsHttpHandler` за счет уменьшения числа конфликтов, связанных с блокировкой пула подключений. Была увеличена пропускная способность приложений, которые выполняют множество исходящих запросов HTTP, таких как некоторые архитектуры микрослужб. Под нагрузкой пропускная способность `HttpClient` может повышаться до 60 % в Linux и до 20 % в Windows.

Дополнительные сведения см. в статье, посвященной [запросам на вытягивание, благодаря которым удалось добиться этого улучшения](https://github.com/dotnet/corefx/pull/32568).

## <a name="additional-information"></a>Дополнительные сведения

Полный список изменений см. в статье [Заметки о выпуске ASP.NET Core 2.2](https://github.com/aspnet/Home/releases/tag/2.2.0).