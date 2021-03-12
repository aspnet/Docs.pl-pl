---
title: ASP.NET Core hosta SignalR w usługach w tle
author: bradygaster
description: Dowiedz się, jak wysyłać komunikaty do SignalR klientów z klas BackgroundService platformy .NET Core.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- appsettings.json
- ASP.NET Core Identity
- cookie
- Cookie
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/background-services
ms.openlocfilehash: 9d95d7c9a33bcf2f4a603d815269752124133ee6
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102589702"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a>ASP.NET Core hosta SignalR w usługach w tle

Autor [Brady gastera](https://twitter.com/bradygaster)

Ten artykuł zawiera wskazówki dotyczące:

* Hosting SignalR centrów przy użyciu procesu roboczego w tle hostowanego z ASP.NET Core.
* Wysyłanie komunikatów do podłączonych klientów z poziomu platformy .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).

::: moniker range=">= aspnetcore-3.0"

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/signalr/background-service/samples/3.x) [(jak pobrać)](xref:index#how-to-download-a-sample)

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/signalr/background-service/samples/2.2) [(jak pobrać)](xref:index#how-to-download-a-sample)

::: moniker-end

## <a name="enable-signalr-in-startup"></a>Włącz SignalR przy uruchamianiu

::: moniker range=">= aspnetcore-3.0"

Hostowanie ASP.NET Core SignalR Hub w kontekście procesu roboczego w tle jest takie samo, jak hostowanie centrum w aplikacji sieci web ASP.NET Core. W `Startup.ConfigureServices` metodzie wywoływanie `services.AddSignalR` powoduje dodanie wymaganych usług do warstwy iniekcja (ASP.NET Core zależność) do obsługi SignalR . W programie `Startup.Configure` `MapHub` Metoda jest wywoływana w `UseEndpoints` wywołaniu zwrotnym, aby połączyć punkty końcowe centrum w potoku żądania ASP.NET Core.

[!code-csharp[Startup](background-service/samples/3.x/Server/Startup.cs?name=Startup)]

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

Hostowanie ASP.NET Core SignalR Hub w kontekście procesu roboczego w tle jest takie samo, jak hostowanie centrum w aplikacji sieci web ASP.NET Core. W `Startup.ConfigureServices` metodzie wywoływanie `services.AddSignalR` powoduje dodanie wymaganych usług do warstwy iniekcja (ASP.NET Core zależność) do obsługi SignalR . W programie `Startup.Configure` `UseSignalR` Metoda jest wywoływana, aby połączyć punkty końcowe centrum w potoku żądania ASP.NET Core.

[!code-csharp[Startup](background-service/samples/2.2/Server/Startup.cs?name=Startup)]

::: moniker-end

W poprzednim przykładzie `ClockHub` Klasa implementuje `Hub<T>` klasę, aby utworzyć koncentrator o jednoznacznie określonym typie. Program `ClockHub` został skonfigurowany w klasie w `Startup` celu reagowania na żądania w punkcie końcowym `/hubs/clock` .

Aby uzyskać więcej informacji na temat silnych typach centrów, zobacz [Używanie centrów w SignalR dla ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).

> [!NOTE]
> Ta funkcja nie jest ograniczona do [klasy \<T> centrum](xref:Microsoft.AspNetCore.SignalR.Hub`1) . Każda klasa, która dziedziczy z [koncentratora](xref:Microsoft.AspNetCore.SignalR.Hub), na przykład [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), działa.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Startup](background-service/samples/3.x/Server/ClockHub.cs?name=ClockHub)]

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Startup](background-service/samples/2.2/Server/ClockHub.cs?name=ClockHub)]

::: moniker-end

Interfejs używany przez silnie typ `ClockHub` jest `IClock` interfejsem.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Startup](background-service/samples/3.x/HubServiceInterfaces/IClock.cs?name=IClock)]

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Startup](background-service/samples/2.2/HubServiceInterfaces/IClock.cs?name=IClock)]

::: moniker-end

## <a name="call-a-signalr-hub-from-a-background-service"></a>Wywoływanie SignalR centrum z poziomu usługi w tle

Podczas uruchamiania, `Worker` Klasa, a `BackgroundService` , jest włączona przy użyciu `AddHostedService` .

```csharp
services.AddHostedService<Worker>();
```

Ponieważ SignalR jest również włączona w `Startup` fazie, w którym każde centrum jest dołączone do pojedynczego punktu końcowego w potoku żądania HTTP ASP.NET Core, każde centrum jest reprezentowane przez `IHubContext<T>` serwer. Przy użyciu ASP.NET Core DI Features, inne klasy tworzone przez warstwę hostingu, takie jak `BackgroundService` klasy, klasy kontrolerów MVC lub Razor modele stron, mogą uzyskać odwołania do centrów po stronie serwera, akceptując wystąpienia `IHubContext<ClockHub, IClock>` w trakcie konstruowania.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Startup](background-service/samples/3.x/Server/Worker.cs?name=Worker)]

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Startup](background-service/samples/2.2/Server/Worker.cs?name=Worker)]

::: moniker-end

Ponieważ `ExecuteAsync` Metoda jest wywoływana iteracyjnie w usłudze w tle, bieżąca data i godzina serwera są wysyłane do podłączonych klientów przy użyciu `ClockHub` .

## <a name="react-to-signalr-events-with-background-services"></a>Reagowanie na SignalR zdarzenia za pomocą usług w tle

Podobnie jak w przypadku aplikacji jednostronicowej korzystającej z klienta JavaScript dla programu SignalR lub aplikacji klasycznej platformy .NET korzystającej z programu <xref:signalr/dotnet-client> , a lub do łączenia się z `BackgroundService` `IHostedService` SignalR centrami i reagowania na zdarzenia.

`ClockHubClient`Klasa implementuje `IClock` interfejs i `IHostedService` interfejs. W ten sposób można ją włączyć w sposób `Startup` ciągły i reagować na zdarzenia centrów z serwera.

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

Podczas inicjowania program `ClockHubClient` tworzy wystąpienie `HubConnection` a i włącza `IClock.ShowTime` metodę jako procedurę obsługi dla `ShowTime` zdarzenia centrum.

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[The ClockHubClient constructor](background-service/samples/3.x/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

W `IHostedService.StartAsync` implementacji `HubConnection` jest uruchamiany asynchronicznie.

[!code-csharp[StartAsync method](background-service/samples/3.x/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

W trakcie `IHostedService.StopAsync` metody `HubConnection` jest usuwany asynchronicznie.

[!code-csharp[StopAsync method](background-service/samples/3.x/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

::: moniker-end
::: moniker range="<= aspnetcore-2.2"

[!code-csharp[The ClockHubClient constructor](background-service/samples/2.2/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

W `IHostedService.StartAsync` implementacji `HubConnection` jest uruchamiany asynchronicznie.

[!code-csharp[StartAsync method](background-service/samples/2.2/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

W trakcie `IHostedService.StopAsync` metody `HubConnection` jest usuwany asynchronicznie.

[!code-csharp[StopAsync method](background-service/samples/2.2/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

::: moniker-end

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Wprowadzenie](xref:tutorials/signalr)
* [Centra](xref:signalr/hubs)
* [Publikowanie na platformie Azure](xref:signalr/publish-to-azure-web-app)
* [Centra o jednoznacznie określonym typie](xref:signalr/hubs#strongly-typed-hubs)
