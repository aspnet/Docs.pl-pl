---
title: Wykonywanie żądań HTTP przy użyciu IHttpClientFactory w ASP.NET Core
author: stevejgordon
description: Dowiedz się więcej o używaniu interfejsu IHttpClientFactory do zarządzania wystąpieniami logicznych HttpClient w ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 1/21/2021
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
uid: fundamentals/http-requests
ms.openlocfilehash: 2bc093af63f305dd9808e37011223043646852d5
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102588051"
---
# <a name="make-http-requests-using-ihttpclientfactory-in-aspnet-core"></a><span data-ttu-id="657a3-103">Wykonywanie żądań HTTP przy użyciu IHttpClientFactory w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="657a3-103">Make HTTP requests using IHttpClientFactory in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="657a3-104">[Kirka Larkin](https://github.com/serpent5), [Steve Gordon](https://github.com/stevejgordon), [Glenn Condron](https://github.com/glennc)i [Ryan Nowak](https://github.com/rynowak).</span><span class="sxs-lookup"><span data-stu-id="657a3-104">By [Kirk Larkin](https://github.com/serpent5), [Steve Gordon](https://github.com/stevejgordon), [Glenn Condron](https://github.com/glennc), and [Ryan Nowak](https://github.com/rynowak).</span></span>

<span data-ttu-id="657a3-105"><xref:System.Net.Http.IHttpClientFactory>Może być zarejestrowany i używany do konfigurowania i tworzenia <xref:System.Net.Http.HttpClient> wystąpień w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-105">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="657a3-106">`IHttpClientFactory` oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="657a3-106">`IHttpClientFactory` offers the following benefits:</span></span>

* <span data-ttu-id="657a3-107">Zapewnia centralną lokalizację do nazywania i konfigurowania `HttpClient` wystąpień logicznych.</span><span class="sxs-lookup"><span data-stu-id="657a3-107">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="657a3-108">Na przykład klient o nazwie  *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="657a3-108">For example, a client named  *github* could be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="657a3-109">Domyślny klient można zarejestrować na potrzeby ogólnego dostępu.</span><span class="sxs-lookup"><span data-stu-id="657a3-109">A default client can be registered for general access.</span></span>
* <span data-ttu-id="657a3-110">Kodyfikacja koncepcji wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w programie `HttpClient` .</span><span class="sxs-lookup"><span data-stu-id="657a3-110">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient`.</span></span> <span data-ttu-id="657a3-111">Udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, aby skorzystać z delegowania programów obsługi w programie `HttpClient` .</span><span class="sxs-lookup"><span data-stu-id="657a3-111">Provides extensions for Polly-based middleware to take advantage of delegating handlers in `HttpClient`.</span></span>
* <span data-ttu-id="657a3-112">Zarządza buforowaniem i okresem istnienia `HttpClientMessageHandler` wystąpień podstawowych.</span><span class="sxs-lookup"><span data-stu-id="657a3-112">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances.</span></span> <span data-ttu-id="657a3-113">Automatyczne zarządzanie pozwala uniknąć typowych problemów z usługą DNS (Domain Name System) występujących podczas ręcznego zarządzania `HttpClient` okresami istnienia.</span><span class="sxs-lookup"><span data-stu-id="657a3-113">Automatic management avoids common DNS (Domain Name System) problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="657a3-114">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem programu `ILogger` ) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="657a3-114">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="657a3-115">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="657a3-115">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="657a3-116">Przykładowy kod w tym temacie używa <xref:System.Text.Json> do deserializacji zawartości JSON zwróconej w odpowiedziach http.</span><span class="sxs-lookup"><span data-stu-id="657a3-116">The sample code in this topic version uses <xref:System.Text.Json> to deserialize JSON content returned in HTTP responses.</span></span> <span data-ttu-id="657a3-117">Aby zapoznać się z przykładami używanymi przez `Json.NET` `ReadAsAsync<T>` program i, użyj selektora wersji, aby wybrać wersję 2. x tego tematu.</span><span class="sxs-lookup"><span data-stu-id="657a3-117">For samples that use `Json.NET` and `ReadAsAsync<T>`, use the version selector to select a 2.x version of this topic.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="657a3-118">Wzorce zużycia</span><span class="sxs-lookup"><span data-stu-id="657a3-118">Consumption patterns</span></span>

<span data-ttu-id="657a3-119">`IHttpClientFactory`W aplikacji można używać kilku sposobów:</span><span class="sxs-lookup"><span data-stu-id="657a3-119">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="657a3-120">Podstawowy sposób użycia</span><span class="sxs-lookup"><span data-stu-id="657a3-120">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="657a3-121">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="657a3-121">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="657a3-122">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="657a3-122">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="657a3-123">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="657a3-123">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="657a3-124">Najlepsze podejście zależy od wymagań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-124">The best approach depends upon the app's requirements.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="657a3-125">Podstawowy sposób użycia</span><span class="sxs-lookup"><span data-stu-id="657a3-125">Basic usage</span></span>

<span data-ttu-id="657a3-126">`IHttpClientFactory` można zarejestrować, wywołując `AddHttpClient` :</span><span class="sxs-lookup"><span data-stu-id="657a3-126">`IHttpClientFactory` can be registered by calling `AddHttpClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="657a3-127">`IHttpClientFactory`Można zażądać przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="657a3-127">An `IHttpClientFactory` can be requested using [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="657a3-128">Następujący kod używa `IHttpClientFactory` do tworzenia `HttpClient` wystąpienia:</span><span class="sxs-lookup"><span data-stu-id="657a3-128">The following code uses `IHttpClientFactory` to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="657a3-129">Użycie `IHttpClientFactory` podobnej do powyższego przykładu jest dobrym sposobem refaktoryzacji istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-129">Using `IHttpClientFactory` like in the preceding example is a good way to refactor an existing app.</span></span> <span data-ttu-id="657a3-130">Nie ma to wpływu na sposób `HttpClient` użycia.</span><span class="sxs-lookup"><span data-stu-id="657a3-130">It has no impact on how `HttpClient` is used.</span></span> <span data-ttu-id="657a3-131">W miejscach, w których `HttpClient` wystąpienia są tworzone w istniejącej aplikacji, Zastąp te wystąpienia wywołaniami do <xref:System.Net.Http.IHttpClientFactory.CreateClient*> .</span><span class="sxs-lookup"><span data-stu-id="657a3-131">In places where `HttpClient` instances are created in an existing app, replace those occurrences with calls to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="657a3-132">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="657a3-132">Named clients</span></span>

<span data-ttu-id="657a3-133">Nazwani klienci są dobrym wyborem w przypadku:</span><span class="sxs-lookup"><span data-stu-id="657a3-133">Named clients are a good choice when:</span></span>

* <span data-ttu-id="657a3-134">Aplikacja wymaga wielu odrębnych użycia `HttpClient` .</span><span class="sxs-lookup"><span data-stu-id="657a3-134">The app requires many distinct uses of `HttpClient`.</span></span>
* <span data-ttu-id="657a3-135">Wiele `HttpClient` s ma inną konfigurację.</span><span class="sxs-lookup"><span data-stu-id="657a3-135">Many `HttpClient`s have different configuration.</span></span>

<span data-ttu-id="657a3-136">Konfigurację dla nazwy `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="657a3-136">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="657a3-137">W powyższym kodzie klient ma skonfigurowany program:</span><span class="sxs-lookup"><span data-stu-id="657a3-137">In the preceding code the client is configured with:</span></span>

* <span data-ttu-id="657a3-138">Adres podstawowy `https://api.github.com/` .</span><span class="sxs-lookup"><span data-stu-id="657a3-138">The base address `https://api.github.com/`.</span></span>
* <span data-ttu-id="657a3-139">Dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="657a3-139">Two headers required to work with the GitHub API.</span></span>

#### <a name="createclient"></a><span data-ttu-id="657a3-140">Klient</span><span class="sxs-lookup"><span data-stu-id="657a3-140">CreateClient</span></span>

<span data-ttu-id="657a3-141">Za każdym razem <xref:System.Net.Http.IHttpClientFactory.CreateClient*> jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="657a3-141">Each time <xref:System.Net.Http.IHttpClientFactory.CreateClient*> is called:</span></span>

* <span data-ttu-id="657a3-142">Tworzone jest nowe wystąpienie `HttpClient` .</span><span class="sxs-lookup"><span data-stu-id="657a3-142">A new instance of `HttpClient` is created.</span></span>
* <span data-ttu-id="657a3-143">Akcja konfiguracji jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="657a3-143">The configuration action is called.</span></span>

<span data-ttu-id="657a3-144">Aby utworzyć nazwanego klienta, Przekaż jego nazwę do `CreateClient` :</span><span class="sxs-lookup"><span data-stu-id="657a3-144">To create a named client, pass its name into `CreateClient`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="657a3-145">W powyższym kodzie żądanie nie musi określać nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="657a3-145">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="657a3-146">Kod może przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-146">The code can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="657a3-147">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="657a3-147">Typed clients</span></span>

<span data-ttu-id="657a3-148">Wpisane komputery klienckie:</span><span class="sxs-lookup"><span data-stu-id="657a3-148">Typed clients:</span></span>

* <span data-ttu-id="657a3-149">Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.</span><span class="sxs-lookup"><span data-stu-id="657a3-149">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="657a3-150">Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.</span><span class="sxs-lookup"><span data-stu-id="657a3-150">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="657a3-151">Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient` .</span><span class="sxs-lookup"><span data-stu-id="657a3-151">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="657a3-152">Na przykład może zostać użyty pojedynczy klient z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="657a3-152">For example, a single typed client might be used:</span></span>
  * <span data-ttu-id="657a3-153">Dla pojedynczego punktu końcowego zaplecza.</span><span class="sxs-lookup"><span data-stu-id="657a3-153">For a single backend endpoint.</span></span>
  * <span data-ttu-id="657a3-154">Do hermetyzacji wszystkich logiki związanych z punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="657a3-154">To encapsulate all logic dealing with the endpoint.</span></span>
* <span data-ttu-id="657a3-155">Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-155">Work with DI and can be injected where required in the app.</span></span>

<span data-ttu-id="657a3-156">Klient z określonym typem akceptuje `HttpClient` parametr w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="657a3-156">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="657a3-157">Powyższy kod ma następujące działanie:</span><span class="sxs-lookup"><span data-stu-id="657a3-157">In the preceding code:</span></span>

* <span data-ttu-id="657a3-158">Konfiguracja zostanie przeniesiona do określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-158">The configuration is moved into the typed client.</span></span>
* <span data-ttu-id="657a3-159">`HttpClient`Obiekt jest ujawniany jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="657a3-159">The `HttpClient` object is exposed as a public property.</span></span>

<span data-ttu-id="657a3-160">Można tworzyć metody specyficzne dla interfejsu API, które uwidaczniają `HttpClient` funkcjonalność.</span><span class="sxs-lookup"><span data-stu-id="657a3-160">API-specific methods can be created that expose `HttpClient` functionality.</span></span> <span data-ttu-id="657a3-161">Na przykład `GetAspNetDocsIssues` Metoda hermetyzuje kod w celu pobrania otwartych problemów.</span><span class="sxs-lookup"><span data-stu-id="657a3-161">For example, the `GetAspNetDocsIssues` method encapsulates code to retrieve open issues.</span></span>

<span data-ttu-id="657a3-162">Następujący kod wywołuje <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> w `Startup.ConfigureServices` celu zarejestrowania klasy klienta typu:</span><span class="sxs-lookup"><span data-stu-id="657a3-162">The following code calls <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> in `Startup.ConfigureServices` to register a typed client class:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="657a3-163">Typ klienta jest rejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="657a3-163">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="657a3-164">W poprzednim kodzie program `AddHttpClient` rejestruje `GitHubService` jako usługę przejściową.</span><span class="sxs-lookup"><span data-stu-id="657a3-164">In the preceding code, `AddHttpClient` registers `GitHubService` as a transient service.</span></span> <span data-ttu-id="657a3-165">Ta rejestracja używa metody fabryki do:</span><span class="sxs-lookup"><span data-stu-id="657a3-165">This registration uses a factory method to:</span></span>

1. <span data-ttu-id="657a3-166">Utwórz wystąpienie elementu `HttpClient`.</span><span class="sxs-lookup"><span data-stu-id="657a3-166">Create an instance of `HttpClient`.</span></span>
1. <span data-ttu-id="657a3-167">Utwórz wystąpienie `GitHubService` , przekazując w wystąpieniu `HttpClient` do jego konstruktora.</span><span class="sxs-lookup"><span data-stu-id="657a3-167">Create an instance of `GitHubService`, passing in the instance of `HttpClient` to its constructor.</span></span>

<span data-ttu-id="657a3-168">Określony klient może zostać dodany i wykorzystany bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="657a3-168">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="657a3-169">Konfigurację dla określonego klienta można określić podczas rejestracji w `Startup.ConfigureServices` , a nie w konstruktorze określonego klienta:</span><span class="sxs-lookup"><span data-stu-id="657a3-169">The configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="657a3-170">`HttpClient`Mogą być hermetyzowane w ramach określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-170">The `HttpClient` can be encapsulated within a typed client.</span></span> <span data-ttu-id="657a3-171">Zamiast uwidaczniać ją jako właściwość, zdefiniuj metodę, która wywołuje `HttpClient` wystąpienie wewnętrznie:</span><span class="sxs-lookup"><span data-stu-id="657a3-171">Rather than exposing it as a property, define a method which calls the `HttpClient` instance internally:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="657a3-172">W poprzednim kodzie, `HttpClient` jest przechowywany w prywatnym polu.</span><span class="sxs-lookup"><span data-stu-id="657a3-172">In the preceding code, the `HttpClient` is stored in a private field.</span></span> <span data-ttu-id="657a3-173">Dostęp do programu `HttpClient` ma metoda publiczna `GetRepos` .</span><span class="sxs-lookup"><span data-stu-id="657a3-173">Access to the `HttpClient` is by the public `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="657a3-174">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="657a3-174">Generated clients</span></span>

<span data-ttu-id="657a3-175">`IHttpClientFactory` może być używany w połączeniu z bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="657a3-175">`IHttpClientFactory` can be used in combination with third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="657a3-176">REFIT to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="657a3-176">Refit is a REST library for .NET.</span></span> <span data-ttu-id="657a3-177">Konwertuje interfejsy API REST na interfejsy na żywo.</span><span class="sxs-lookup"><span data-stu-id="657a3-177">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="657a3-178">Implementacja interfejsu jest generowana dynamicznie przez `RestService` , przy użyciu polecenia, `HttpClient` Aby utworzyć zewnętrzne wywołania http.</span><span class="sxs-lookup"><span data-stu-id="657a3-178">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="657a3-179">Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="657a3-179">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="657a3-180">Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:</span><span class="sxs-lookup"><span data-stu-id="657a3-180">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddControllers();
}
```

<span data-ttu-id="657a3-181">Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:</span><span class="sxs-lookup"><span data-stu-id="657a3-181">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="make-post-put-and-delete-requests"></a><span data-ttu-id="657a3-182">Wykonywanie żądań POST, PUT i DELETE</span><span class="sxs-lookup"><span data-stu-id="657a3-182">Make POST, PUT, and DELETE requests</span></span>

<span data-ttu-id="657a3-183">W powyższych przykładach wszystkie żądania HTTP używają zlecenia GET HTTP.</span><span class="sxs-lookup"><span data-stu-id="657a3-183">In the preceding examples, all HTTP requests use the GET HTTP verb.</span></span> <span data-ttu-id="657a3-184">`HttpClient` Program obsługuje również inne zlecenia HTTP, w tym:</span><span class="sxs-lookup"><span data-stu-id="657a3-184">`HttpClient` also supports other HTTP verbs, including:</span></span>

* <span data-ttu-id="657a3-185">POST</span><span class="sxs-lookup"><span data-stu-id="657a3-185">POST</span></span>
* <span data-ttu-id="657a3-186">PUT</span><span class="sxs-lookup"><span data-stu-id="657a3-186">PUT</span></span>
* <span data-ttu-id="657a3-187">DELETE</span><span class="sxs-lookup"><span data-stu-id="657a3-187">DELETE</span></span>
* <span data-ttu-id="657a3-188">WYSŁANA</span><span class="sxs-lookup"><span data-stu-id="657a3-188">PATCH</span></span>

<span data-ttu-id="657a3-189">Aby uzyskać pełną listę obsługiwanych czasowników HTTP, zobacz <xref:System.Net.Http.HttpMethod> .</span><span class="sxs-lookup"><span data-stu-id="657a3-189">For a complete list of supported HTTP verbs, see <xref:System.Net.Http.HttpMethod>.</span></span>

<span data-ttu-id="657a3-190">Poniższy przykład pokazuje, jak utworzyć żądanie HTTP POST:</span><span class="sxs-lookup"><span data-stu-id="657a3-190">The following example shows how to make an HTTP POST request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpRequestsSample/Models/TodoClient.cs?name=snippet_POST)]

<span data-ttu-id="657a3-191">W poprzednim kodzie `CreateItemAsync` Metoda:</span><span class="sxs-lookup"><span data-stu-id="657a3-191">In the preceding code, the `CreateItemAsync` method:</span></span>

* <span data-ttu-id="657a3-192">Serializować `TodoItem` parametr do formatu JSON przy użyciu `System.Text.Json` .</span><span class="sxs-lookup"><span data-stu-id="657a3-192">Serializes the `TodoItem` parameter to JSON using `System.Text.Json`.</span></span> <span data-ttu-id="657a3-193">Powoduje to użycie wystąpienia programu <xref:System.Text.Json.JsonSerializerOptions> w celu skonfigurowania procesu serializacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-193">This uses an instance of <xref:System.Text.Json.JsonSerializerOptions> to configure the serialization process.</span></span>
* <span data-ttu-id="657a3-194">Tworzy wystąpienie <xref:System.Net.Http.StringContent> do pakowania serializowanego kodu JSON do wysłania w treści żądania HTTP.</span><span class="sxs-lookup"><span data-stu-id="657a3-194">Creates an instance of <xref:System.Net.Http.StringContent> to package the serialized JSON for sending in the HTTP request's body.</span></span>
* <span data-ttu-id="657a3-195">Wywołuje <xref:System.Net.Http.HttpClient.PostAsync%2A> wysyłanie zawartości JSON do określonego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="657a3-195">Calls <xref:System.Net.Http.HttpClient.PostAsync%2A> to send the JSON content to the specified URL.</span></span> <span data-ttu-id="657a3-196">Jest to względny adres URL, który zostanie dodany do [HttpClient. BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress).</span><span class="sxs-lookup"><span data-stu-id="657a3-196">This is a relative URL that gets added to the [HttpClient.BaseAddress](xref:System.Net.Http.HttpClient.BaseAddress).</span></span>
* <span data-ttu-id="657a3-197">Wywołuje, <xref:System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode%2A> Aby zgłosić wyjątek, jeśli kod stanu odpowiedzi nie wskazuje powodzenia.</span><span class="sxs-lookup"><span data-stu-id="657a3-197">Calls <xref:System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode%2A> to throw an exception if the response status code does not indicate success.</span></span>

<span data-ttu-id="657a3-198">`HttpClient` obsługuje również inne typy zawartości.</span><span class="sxs-lookup"><span data-stu-id="657a3-198">`HttpClient` also supports other types of content.</span></span> <span data-ttu-id="657a3-199">Na przykład <xref:System.Net.Http.MultipartContent> i <xref:System.Net.Http.StreamContent>.</span><span class="sxs-lookup"><span data-stu-id="657a3-199">For example, <xref:System.Net.Http.MultipartContent> and <xref:System.Net.Http.StreamContent>.</span></span> <span data-ttu-id="657a3-200">Aby zapoznać się z pełną listą obsługiwanych zawartości, zobacz <xref:System.Net.Http.HttpContent> .</span><span class="sxs-lookup"><span data-stu-id="657a3-200">For a complete list of supported content, see <xref:System.Net.Http.HttpContent>.</span></span>

<span data-ttu-id="657a3-201">Poniższy przykład przedstawia żądanie HTTP PUT:</span><span class="sxs-lookup"><span data-stu-id="657a3-201">The following example shows an HTTP PUT request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpRequestsSample/Models/TodoClient.cs?name=snippet_PUT)]

<span data-ttu-id="657a3-202">Poprzedni kod jest bardzo podobny do przykładu POST.</span><span class="sxs-lookup"><span data-stu-id="657a3-202">The preceding code is very similar to the POST example.</span></span> <span data-ttu-id="657a3-203">`SaveItemAsync`Metoda wywołuje <xref:System.Net.Http.HttpClient.PutAsync%2A> zamiast `PostAsync` .</span><span class="sxs-lookup"><span data-stu-id="657a3-203">The `SaveItemAsync` method calls <xref:System.Net.Http.HttpClient.PutAsync%2A> instead of `PostAsync`.</span></span>

<span data-ttu-id="657a3-204">Poniższy przykład przedstawia żądanie HTTP DELETE:</span><span class="sxs-lookup"><span data-stu-id="657a3-204">The following example shows an HTTP DELETE request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpRequestsSample/Models/TodoClient.cs?name=snippet_DELETE)]

<span data-ttu-id="657a3-205">W poprzednim kodzie `DeleteItemAsync` Metoda wywołuje metodę <xref:System.Net.Http.HttpClient.DeleteAsync%2A> .</span><span class="sxs-lookup"><span data-stu-id="657a3-205">In the preceding code, the `DeleteItemAsync` method calls <xref:System.Net.Http.HttpClient.DeleteAsync%2A>.</span></span> <span data-ttu-id="657a3-206">Ponieważ żądania HTTP DELETE zazwyczaj nie zawierają treści, `DeleteAsync` Metoda nie zapewnia przeciążenia, które akceptuje wystąpienie `HttpContent` .</span><span class="sxs-lookup"><span data-stu-id="657a3-206">Because HTTP DELETE requests typically contain no body, the `DeleteAsync` method doesn't provide an overload that accepts an instance of `HttpContent`.</span></span>

<span data-ttu-id="657a3-207">Aby dowiedzieć się więcej o używaniu różnych zleceń HTTP w programie `HttpClient` , zobacz <xref:System.Net.Http.HttpClient> .</span><span class="sxs-lookup"><span data-stu-id="657a3-207">To learn more about using different HTTP verbs with `HttpClient`, see <xref:System.Net.Http.HttpClient>.</span></span>

## <a name="outgoing-request-middleware"></a><span data-ttu-id="657a3-208">Oprogramowanie pośredniczące żądania wychodzące</span><span class="sxs-lookup"><span data-stu-id="657a3-208">Outgoing request middleware</span></span>

<span data-ttu-id="657a3-209">`HttpClient` ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="657a3-209">`HttpClient` has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="657a3-210">`IHttpClientFactory`:</span><span class="sxs-lookup"><span data-stu-id="657a3-210">`IHttpClientFactory`:</span></span>

  * <span data-ttu-id="657a3-211">Upraszcza definiowanie programów obsługi do zastosowania dla każdego nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-211">Simplifies defining the handlers to apply for each named client.</span></span>
  * <span data-ttu-id="657a3-212">Obsługuje rejestrację i łańcuch wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="657a3-212">Supports registration and chaining of multiple handlers to build an outgoing request   middleware pipeline.</span></span> <span data-ttu-id="657a3-213">Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym.</span><span class="sxs-lookup"><span data-stu-id="657a3-213">Each of these handlers is able to perform work before and after the   outgoing request.</span></span> <span data-ttu-id="657a3-214">Ten wzorzec:</span><span class="sxs-lookup"><span data-stu-id="657a3-214">This pattern:</span></span>
  
    * <span data-ttu-id="657a3-215">Jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="657a3-215">Is similar to the inbound middleware pipeline in ASP.NET Core.</span></span>
    * <span data-ttu-id="657a3-216">Program udostępnia mechanizm zarządzania problemami z obcinaniem między żądaniami HTTP, takimi jak:</span><span class="sxs-lookup"><span data-stu-id="657a3-216">Provides a mechanism to manage cross-cutting concerns around HTTP requests, such as:</span></span>
      * <span data-ttu-id="657a3-217">pamięć</span><span class="sxs-lookup"><span data-stu-id="657a3-217">caching</span></span>
      * <span data-ttu-id="657a3-218">Obsługa błędów</span><span class="sxs-lookup"><span data-stu-id="657a3-218">error handling</span></span>
      * <span data-ttu-id="657a3-219">serializacji</span><span class="sxs-lookup"><span data-stu-id="657a3-219">serialization</span></span>
      * <span data-ttu-id="657a3-220">rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="657a3-220">logging</span></span>

<span data-ttu-id="657a3-221">Aby utworzyć program obsługi delegowania:</span><span class="sxs-lookup"><span data-stu-id="657a3-221">To create a delegating handler:</span></span>

* <span data-ttu-id="657a3-222">Pochodny od <xref:System.Net.Http.DelegatingHandler> .</span><span class="sxs-lookup"><span data-stu-id="657a3-222">Derive from <xref:System.Net.Http.DelegatingHandler>.</span></span>
* <span data-ttu-id="657a3-223">Zastąpienie <xref:System.Net.Http.DelegatingHandler.SendAsync*> .</span><span class="sxs-lookup"><span data-stu-id="657a3-223">Override <xref:System.Net.Http.DelegatingHandler.SendAsync*>.</span></span> <span data-ttu-id="657a3-224">Wykonaj kod przed przekazaniem żądania do następnego programu obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="657a3-224">Execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="657a3-225">Poprzedni kod sprawdza, czy `X-API-KEY` nagłówek jest w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="657a3-225">The preceding code checks if the `X-API-KEY` header is in the request.</span></span> <span data-ttu-id="657a3-226">Jeśli `X-API-KEY` brakuje, <xref:System.Net.HttpStatusCode.BadRequest> jest zwracany.</span><span class="sxs-lookup"><span data-stu-id="657a3-226">If `X-API-KEY` is missing, <xref:System.Net.HttpStatusCode.BadRequest> is returned.</span></span>

<span data-ttu-id="657a3-227">Do konfiguracji dla elementu with można dodać więcej niż jeden program obsługi `HttpClient` <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName> :</span><span class="sxs-lookup"><span data-stu-id="657a3-227">More than one handler can be added to the configuration for an `HttpClient` with <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*?displayProperty=fullName>:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup2.cs?name=snippet1)]

<span data-ttu-id="657a3-228">W poprzednim kodzie, `ValidateHeaderHandler` jest zarejestrowany przy użyciu di.</span><span class="sxs-lookup"><span data-stu-id="657a3-228">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="657a3-229">Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując typ do procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="657a3-229">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="657a3-230">Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="657a3-230">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="657a3-231">Każdy program obsługi otacza następną procedurę obsługi do momentu zakończenia `HttpClientHandler` żądania:</span><span class="sxs-lookup"><span data-stu-id="657a3-231">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

### <a name="use-di-in-outgoing-request-middleware"></a><span data-ttu-id="657a3-232">Użyj programu "DI" dla oprogramowania pośredniczącego żądań wychodzących</span><span class="sxs-lookup"><span data-stu-id="657a3-232">Use DI in outgoing request middleware</span></span>

<span data-ttu-id="657a3-233">Gdy `IHttpClientFactory` program tworzy nową procedurę delegowania, używa di do spełnienia parametrów konstruktora procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="657a3-233">When `IHttpClientFactory` creates a new delegating handler, it uses DI to fulfill the handler's constructor parameters.</span></span> <span data-ttu-id="657a3-234">`IHttpClientFactory` tworzy **oddzielny** zakres di dla każdej procedury obsługi, co może prowadzić do zaskakujące zachowania, gdy program obsługi korzysta z usługi w *zakresie* .</span><span class="sxs-lookup"><span data-stu-id="657a3-234">`IHttpClientFactory` creates a **separate** DI scope for each handler, which can lead to surprising behavior when a handler consumes a *scoped* service.</span></span>

<span data-ttu-id="657a3-235">Rozważmy na przykład następujący interfejs i jego implementacja, która reprezentuje zadanie jako operację z identyfikatorem `OperationId` :</span><span class="sxs-lookup"><span data-stu-id="657a3-235">For example, consider the following interface and its implementation, which represents a task as an operation with an identifier, `OperationId`:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpRequestsSample/Models/OperationScoped.cs?name=snippet_Types)]

<span data-ttu-id="657a3-236">W miarę jak nazwa sugeruje, `IOperationScoped` jest zarejestrowana za pomocą di przy użyciu okresu istnienia w *zakresie* :</span><span class="sxs-lookup"><span data-stu-id="657a3-236">As its name suggests, `IOperationScoped` is registered with DI using a *scoped* lifetime:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpRequestsSample/Startup.cs?name=snippet_IOperationScoped&highlight=18,26)]

<span data-ttu-id="657a3-237">Następująca procedura delegowania wykorzystuje i używa `IOperationScoped` do ustawiania `X-OPERATION-ID` nagłówka dla żądania wychodzącego:</span><span class="sxs-lookup"><span data-stu-id="657a3-237">The following delegating handler consumes and uses `IOperationScoped` to set the `X-OPERATION-ID` header for the outgoing request:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpRequestsSample/Handlers/OperationHandler.cs?name=snippet_Class&highlight=13)]

<span data-ttu-id="657a3-238">W obszarze [ `HttpRequestsSample` pobieranie](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/http-requests/samples/3.x/HttpRequestsSample)] Przejdź do `/Operation` strony i Odśwież stronę.</span><span class="sxs-lookup"><span data-stu-id="657a3-238">In the [`HttpRequestsSample` download](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/http-requests/samples/3.x/HttpRequestsSample)], navigate to `/Operation` and refresh the page.</span></span> <span data-ttu-id="657a3-239">Wartość zakresu żądania zmienia się dla każdego żądania, ale wartość zakresu procedury obsługi zmienia się co 5 sekund.</span><span class="sxs-lookup"><span data-stu-id="657a3-239">The request scope value changes for each request, but the handler scope value only changes every 5 seconds.</span></span>

<span data-ttu-id="657a3-240">Programy obsługi mogą zależeć od usług dowolnego zakresu.</span><span class="sxs-lookup"><span data-stu-id="657a3-240">Handlers can depend upon services of any scope.</span></span> <span data-ttu-id="657a3-241">Usługi, które są zależne od programów obsługi, są usuwane, gdy program obsługi zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="657a3-241">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="657a3-242">Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="657a3-242">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="657a3-243">Przekaż dane do procedury obsługi przy użyciu [HttpRequestMessage. Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span><span class="sxs-lookup"><span data-stu-id="657a3-243">Pass data into the handler using [HttpRequestMessage.Properties](xref:System.Net.Http.HttpRequestMessage.Properties).</span></span>
* <span data-ttu-id="657a3-244">Użyj, <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> Aby uzyskać dostęp do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="657a3-244">Use <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> to access the current request.</span></span>
* <span data-ttu-id="657a3-245">Utwórz niestandardowy <xref:System.Threading.AsyncLocal`1> obiekt magazynu, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="657a3-245">Create a custom <xref:System.Threading.AsyncLocal`1> storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="657a3-246">Korzystanie z programów obsługi opartych na Polly</span><span class="sxs-lookup"><span data-stu-id="657a3-246">Use Polly-based handlers</span></span>

<span data-ttu-id="657a3-247">`IHttpClientFactory` integruje się z biblioteką innych firm [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="657a3-247">`IHttpClientFactory` integrates with the third-party library [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="657a3-248">Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="657a3-248">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="657a3-249">Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.</span><span class="sxs-lookup"><span data-stu-id="657a3-249">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="657a3-250">Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi `HttpClient` wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="657a3-250">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="657a3-251">Rozszerzenia Polly obsługują dodawanie programów obsługi opartych na Polly do klientów.</span><span class="sxs-lookup"><span data-stu-id="657a3-251">The Polly extensions support adding Polly-based handlers to clients.</span></span> <span data-ttu-id="657a3-252">Polly wymaga pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="657a3-252">Polly requires the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="657a3-253">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="657a3-253">Handle transient faults</span></span>

<span data-ttu-id="657a3-254">Błędy są zwykle wykonywane, gdy zewnętrzne wywołania HTTP są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="657a3-254">Faults typically occur when external HTTP calls are transient.</span></span> <span data-ttu-id="657a3-255"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="657a3-255"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="657a3-256">Skonfigurowane zasady z `AddTransientHttpErrorPolicy` obsługą następujących odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="657a3-256">Policies configured with `AddTransientHttpErrorPolicy` handle the following responses:</span></span>

* <xref:System.Net.Http.HttpRequestException>
* <span data-ttu-id="657a3-257">HTTP 5xx</span><span class="sxs-lookup"><span data-stu-id="657a3-257">HTTP 5xx</span></span>
* <span data-ttu-id="657a3-258">HTTP 408</span><span class="sxs-lookup"><span data-stu-id="657a3-258">HTTP 408</span></span>

<span data-ttu-id="657a3-259">`AddTransientHttpErrorPolicy` zapewnia dostęp do `PolicyBuilder` obiektu skonfigurowanego do obsługi błędów reprezentujących możliwy błąd przejściowy:</span><span class="sxs-lookup"><span data-stu-id="657a3-259">`AddTransientHttpErrorPolicy` provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup3.cs?name=snippet1)]

<span data-ttu-id="657a3-260">W poprzednim kodzie `WaitAndRetryAsync` zdefiniowane są zasady.</span><span class="sxs-lookup"><span data-stu-id="657a3-260">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="657a3-261">Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.</span><span class="sxs-lookup"><span data-stu-id="657a3-261">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="657a3-262">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="657a3-262">Dynamically select policies</span></span>

<span data-ttu-id="657a3-263">Metody rozszerzające umożliwiają dodawanie programów obsługi opartych na Polly, na przykład <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*> .</span><span class="sxs-lookup"><span data-stu-id="657a3-263">Extension methods are provided to add Polly-based handlers, for example, <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandler*>.</span></span> <span data-ttu-id="657a3-264">Następujące `AddPolicyHandler` Przeciążenie przeprowadziło inspekcję żądania, aby zdecydować, które zasady zastosować:</span><span class="sxs-lookup"><span data-stu-id="657a3-264">The following `AddPolicyHandler` overload inspects the request to decide which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="657a3-265">W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="657a3-265">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="657a3-266">Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="657a3-266">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="657a3-267">Dodaj wiele programów obsługi Polly</span><span class="sxs-lookup"><span data-stu-id="657a3-267">Add multiple Polly handlers</span></span>

<span data-ttu-id="657a3-268">Jest to typowy sposób zagnieżdżania zasad Pollyymi:</span><span class="sxs-lookup"><span data-stu-id="657a3-268">It's common to nest Polly policies:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="657a3-269">W powyższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="657a3-269">In the preceding example:</span></span>

* <span data-ttu-id="657a3-270">Dodawane są dwa procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="657a3-270">Two handlers are added.</span></span>
* <span data-ttu-id="657a3-271">Pierwsza procedura obsługi używa <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> do dodawania zasad ponawiania.</span><span class="sxs-lookup"><span data-stu-id="657a3-271">The first handler uses <xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddTransientHttpErrorPolicy*> to add a retry policy.</span></span> <span data-ttu-id="657a3-272">Nieudane żądania są ponawiane do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="657a3-272">Failed requests are retried up to three times.</span></span>
* <span data-ttu-id="657a3-273">Drugie `AddTransientHttpErrorPolicy` wywołanie dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="657a3-273">The second `AddTransientHttpErrorPolicy` call adds a circuit breaker policy.</span></span> <span data-ttu-id="657a3-274">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli 5 nieudanych prób wystąpi sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="657a3-274">Further external requests are blocked for 30 seconds if 5 failed attempts occur sequentially.</span></span> <span data-ttu-id="657a3-275">Zasady wyłącznika są stanowe.</span><span class="sxs-lookup"><span data-stu-id="657a3-275">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="657a3-276">Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="657a3-276">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="657a3-277">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="657a3-277">Add policies from the Polly registry</span></span>

<span data-ttu-id="657a3-278">Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry` .</span><span class="sxs-lookup"><span data-stu-id="657a3-278">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span>

<span data-ttu-id="657a3-279">W poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="657a3-279">In the following code:</span></span>

* <span data-ttu-id="657a3-280">Są dodawane zasady "regularne" i "długie".</span><span class="sxs-lookup"><span data-stu-id="657a3-280">The "regular" and "long" policies are added.</span></span>
* <span data-ttu-id="657a3-281"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> dodaje zasady "regularne" i "długie" z rejestru.</span><span class="sxs-lookup"><span data-stu-id="657a3-281"><xref:Microsoft.Extensions.DependencyInjection.PollyHttpClientBuilderExtensions.AddPolicyHandlerFromRegistry*> adds the "regular" and "long" policies from the registry.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup4.cs?name=snippet1)]

<span data-ttu-id="657a3-282">Aby uzyskać więcej informacji na temat `IHttpClientFactory` integracji z integracją i Polly, zobacz [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="657a3-282">For more information on `IHttpClientFactory` and Polly integrations, see the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="657a3-283">HttpClient i zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="657a3-283">HttpClient and lifetime management</span></span>

<span data-ttu-id="657a3-284">Nowe `HttpClient` wystąpienie jest zwracane za każdym razem `CreateClient` , gdy jest wywoływana w `IHttpClientFactory` .</span><span class="sxs-lookup"><span data-stu-id="657a3-284">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="657a3-285"><xref:System.Net.Http.HttpMessageHandler>Tworzony jest na nazwany klient.</span><span class="sxs-lookup"><span data-stu-id="657a3-285">An <xref:System.Net.Http.HttpMessageHandler> is created per named client.</span></span> <span data-ttu-id="657a3-286">Fabryka zarządza okresami istnienia `HttpMessageHandler` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="657a3-286">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="657a3-287">`IHttpClientFactory` pule `HttpMessageHandler` wystąpień tworzonych przez fabrykę w celu zmniejszenia zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="657a3-287">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="657a3-288">`HttpMessageHandler`Wystąpienie może być ponownie używane z puli podczas tworzenia nowego `HttpClient` wystąpienia, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="657a3-288">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="657a3-289">Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="657a3-289">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="657a3-290">Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń.</span><span class="sxs-lookup"><span data-stu-id="657a3-290">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="657a3-291">Niektóre programy obsługi powodują również, że połączenia są otwierane w nieskończoność, co może uniemożliwić oddziałanie programu obsługi w systemie DNS (Domain Name System).</span><span class="sxs-lookup"><span data-stu-id="657a3-291">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS (Domain Name System) changes.</span></span>

<span data-ttu-id="657a3-292">Domyślny okres istnienia programu obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="657a3-292">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="657a3-293">Wartość domyślną można przesłonić na podstawie nazwy klienta:</span><span class="sxs-lookup"><span data-stu-id="657a3-293">The default value can be overridden on a per named client basis:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup5.cs?name=snippet1)]

<span data-ttu-id="657a3-294">`HttpClient` wystąpienia mogą być zwykle traktowane jako obiekty .NET, które **nie** wymagają usuwania.</span><span class="sxs-lookup"><span data-stu-id="657a3-294">`HttpClient` instances can generally be treated as .NET objects **not** requiring disposal.</span></span> <span data-ttu-id="657a3-295">Likwidacja anuluje żądania wychodzące i gwarantuje, że danego `HttpClient` wystąpienia nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*> .</span><span class="sxs-lookup"><span data-stu-id="657a3-295">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="657a3-296">`IHttpClientFactory` śledzi i usuwa zasoby używane przez `HttpClient` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="657a3-296">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span>

<span data-ttu-id="657a3-297">Utrzymywanie pojedynczego `HttpClient` wystąpienia aktywności przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory` .</span><span class="sxs-lookup"><span data-stu-id="657a3-297">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="657a3-298">Ten wzorzec będzie niepotrzebny po migracji do programu `IHttpClientFactory` .</span><span class="sxs-lookup"><span data-stu-id="657a3-298">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="657a3-299">Alternatywy do IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="657a3-299">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="657a3-300">Używanie `IHttpClientFactory` w aplikacji z obsługą podwójnego zapobiegania:</span><span class="sxs-lookup"><span data-stu-id="657a3-300">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="657a3-301">Problemy z wyczerpaniem zasobów przez `HttpMessageHandler` wystąpienia puli.</span><span class="sxs-lookup"><span data-stu-id="657a3-301">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="657a3-302">Nieodświeżone problemy z usługą DNS przez cykliczne `HttpMessageHandler` wystąpienia w regularnych odstępach czasu.</span><span class="sxs-lookup"><span data-stu-id="657a3-302">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="657a3-303">Istnieją alternatywne sposoby rozwiązywania powyższych problemów przy użyciu wystąpienia długotrwałego <xref:System.Net.Http.SocketsHttpHandler> .</span><span class="sxs-lookup"><span data-stu-id="657a3-303">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="657a3-304">Utwórz wystąpienie, `SocketsHttpHandler` gdy aplikacja zostanie uruchomiona i użyje jej na potrzeby życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-304">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="657a3-305">Skonfiguruj <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> odpowiednią wartość na podstawie czasu odświeżania DNS.</span><span class="sxs-lookup"><span data-stu-id="657a3-305">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="657a3-306">Utwórz `HttpClient` wystąpienia przy użyciu programu `new HttpClient(handler, disposeHandler: false)` zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="657a3-306">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="657a3-307">Powyższe podejścia rozwiązują problemy z zarządzaniem zasobami, które `IHttpClientFactory` rozwiązują się w podobny sposób.</span><span class="sxs-lookup"><span data-stu-id="657a3-307">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="657a3-308">`SocketsHttpHandler`Udostępnia połączenia między `HttpClient` wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="657a3-308">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="657a3-309">To udostępnianie uniemożliwia wyczerpanie gniazda.</span><span class="sxs-lookup"><span data-stu-id="657a3-309">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="657a3-310">`SocketsHttpHandler`Cykle połączeń zgodnie z, aby `PooledConnectionLifetime` uniknąć nieodświeżonych problemów z usługą DNS.</span><span class="sxs-lookup"><span data-stu-id="657a3-310">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="657a3-311">Cookiewolumin</span><span class="sxs-lookup"><span data-stu-id="657a3-311">Cookies</span></span>

<span data-ttu-id="657a3-312">`HttpMessageHandler`Wystąpienia w puli powoduje, że `CookieContainer` obiekty są udostępniane.</span><span class="sxs-lookup"><span data-stu-id="657a3-312">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="657a3-313">Nieoczekiwane `CookieContainer` udostępnianie obiektów często prowadzi do niepoprawnego kodu.</span><span class="sxs-lookup"><span data-stu-id="657a3-313">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="657a3-314">W przypadku aplikacji, które wymagają usługi cookie s, należy rozważyć następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="657a3-314">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="657a3-315">Wyłączanie cookie obsługi automatycznej</span><span class="sxs-lookup"><span data-stu-id="657a3-315">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="657a3-316">Unikanie `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="657a3-316">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="657a3-317">Wywołanie <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> wyłączania automatycznej cookie obsługi:</span><span class="sxs-lookup"><span data-stu-id="657a3-317">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="657a3-318">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="657a3-318">Logging</span></span>

<span data-ttu-id="657a3-319">Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="657a3-319">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="657a3-320">Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="657a3-320">Enable the appropriate information level in the logging configuration to see the default log messages.</span></span> <span data-ttu-id="657a3-321">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="657a3-321">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="657a3-322">Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu.</span><span class="sxs-lookup"><span data-stu-id="657a3-322">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="657a3-323">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią "System .NET. http. HttpClient. **MyNamedClient**. LogicalHandler".</span><span class="sxs-lookup"><span data-stu-id="657a3-323">A client named *MyNamedClient*, for example, logs messages with a category of "System.Net.Http.HttpClient.**MyNamedClient**.LogicalHandler".</span></span> <span data-ttu-id="657a3-324">Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="657a3-324">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="657a3-325">Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku.</span><span class="sxs-lookup"><span data-stu-id="657a3-325">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="657a3-326">W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.</span><span class="sxs-lookup"><span data-stu-id="657a3-326">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="657a3-327">Rejestrowanie odbywa się również w potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="657a3-327">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="657a3-328">W przykładzie *MyNamedClient* te komunikaty są rejestrowane z kategorią dziennika "System .NET. http. HttpClient. **MyNamedClient**. ClientHandler".</span><span class="sxs-lookup"><span data-stu-id="657a3-328">In the *MyNamedClient* example, those messages are logged with the log category "System.Net.Http.HttpClient.**MyNamedClient**.ClientHandler".</span></span> <span data-ttu-id="657a3-329">W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania.</span><span class="sxs-lookup"><span data-stu-id="657a3-329">For the request, this occurs after all other handlers have run and immediately before the request is sent.</span></span> <span data-ttu-id="657a3-330">W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="657a3-330">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="657a3-331">Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="657a3-331">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="657a3-332">Może to obejmować zmiany w nagłówkach żądania lub w kodzie stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="657a3-332">This may include changes to request headers or to the response status code.</span></span>

<span data-ttu-id="657a3-333">Uwzględnienie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów.</span><span class="sxs-lookup"><span data-stu-id="657a3-333">Including the name of the client in the log category enables log filtering for specific named clients.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="657a3-334">Konfigurowanie HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="657a3-334">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="657a3-335">Może być konieczne sterowanie konfiguracją wewnętrznej `HttpMessageHandler` używanej przez klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-335">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="657a3-336">`IHttpClientBuilder`Jest zwracany podczas dodawania nazwanych lub wpisanych klientów.</span><span class="sxs-lookup"><span data-stu-id="657a3-336">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="657a3-337"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>Metoda rozszerzająca może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="657a3-337">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="657a3-338">Delegat służy do tworzenia i konfigurowania głównej `HttpMessageHandler` używanej przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="657a3-338">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactorySample/Startup6.cs?name=snippet1)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="657a3-339">Korzystanie z IHttpClientFactory w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="657a3-339">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="657a3-340">W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="657a3-340">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="657a3-341">Microsoft. Extensions. hosting</span><span class="sxs-lookup"><span data-stu-id="657a3-341">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="657a3-342">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="657a3-342">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="657a3-343">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="657a3-343">In the following example:</span></span>

* <span data-ttu-id="657a3-344"><xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usługi [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="657a3-344"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="657a3-345">`MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient` .</span><span class="sxs-lookup"><span data-stu-id="657a3-345">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="657a3-346">`HttpClient` służy do pobierania strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="657a3-346">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="657a3-347">`Main` tworzy zakres do wykonania `GetPage` metody usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="657a3-347">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/3.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="657a3-348">Oprogramowanie pośredniczące propagacji nagłówka</span><span class="sxs-lookup"><span data-stu-id="657a3-348">Header propagation middleware</span></span>

<span data-ttu-id="657a3-349">Propagacja nagłówka to ASP.NET Core oprogramowanie pośredniczące do propagowania nagłówków HTTP z przychodzącego żądania do wychodzących żądań klienta HTTP.</span><span class="sxs-lookup"><span data-stu-id="657a3-349">Header propagation is an ASP.NET Core middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="657a3-350">Aby użyć propagacji nagłówka:</span><span class="sxs-lookup"><span data-stu-id="657a3-350">To use header propagation:</span></span>

* <span data-ttu-id="657a3-351">Odwołuje się do pakietu [Microsoft. AspNetCore. HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation) .</span><span class="sxs-lookup"><span data-stu-id="657a3-351">Reference the [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation) package.</span></span>
* <span data-ttu-id="657a3-352">Skonfiguruj oprogramowanie pośredniczące i `HttpClient` w programie `Startup` :</span><span class="sxs-lookup"><span data-stu-id="657a3-352">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/3.x/Startup.cs?highlight=5-9,21&name=snippet)]

* <span data-ttu-id="657a3-353">Klient zawiera skonfigurowane nagłówki w żądaniach wychodzących:</span><span class="sxs-lookup"><span data-stu-id="657a3-353">The client includes the configured headers on outbound requests:</span></span>

  ```csharp
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="657a3-354">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="657a3-354">Additional resources</span></span>

* [<span data-ttu-id="657a3-355">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="657a3-355">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="657a3-356">Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="657a3-356">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="657a3-357">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="657a3-357">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)
* [<span data-ttu-id="657a3-358">Jak serializować i deserializować kod JSON w programie .NET</span><span class="sxs-lookup"><span data-stu-id="657a3-358">How to serialize and deserialize JSON in .NET</span></span>](/dotnet/standard/serialization/system-text-json-how-to)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="657a3-359">[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak)i [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="657a3-359">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="657a3-360"><xref:System.Net.Http.IHttpClientFactory>Może być zarejestrowany i używany do konfigurowania i tworzenia <xref:System.Net.Http.HttpClient> wystąpień w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-360">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="657a3-361">Oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="657a3-361">It offers the following benefits:</span></span>

* <span data-ttu-id="657a3-362">Zapewnia centralną lokalizację do nazywania i konfigurowania `HttpClient` wystąpień logicznych.</span><span class="sxs-lookup"><span data-stu-id="657a3-362">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="657a3-363">Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="657a3-363">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="657a3-364">Domyślny klient można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="657a3-364">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="657a3-365">Skodyfikował koncepcję wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w programie `HttpClient` i udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, aby skorzystać z tego programu.</span><span class="sxs-lookup"><span data-stu-id="657a3-365">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="657a3-366">Zarządza buforowaniem i okresem istnienia podstawowych `HttpClientMessageHandler` wystąpień, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresami istnienia.</span><span class="sxs-lookup"><span data-stu-id="657a3-366">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="657a3-367">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem programu `ILogger` ) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="657a3-367">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="657a3-368">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="657a3-368">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="657a3-369">Wzorce zużycia</span><span class="sxs-lookup"><span data-stu-id="657a3-369">Consumption patterns</span></span>

<span data-ttu-id="657a3-370">`IHttpClientFactory`W aplikacji można używać kilku sposobów:</span><span class="sxs-lookup"><span data-stu-id="657a3-370">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="657a3-371">Podstawowy sposób użycia</span><span class="sxs-lookup"><span data-stu-id="657a3-371">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="657a3-372">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="657a3-372">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="657a3-373">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="657a3-373">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="657a3-374">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="657a3-374">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="657a3-375">Żadna z nich nie jest ściśle wyższa od siebie.</span><span class="sxs-lookup"><span data-stu-id="657a3-375">None of them are strictly superior to another.</span></span> <span data-ttu-id="657a3-376">Najlepsze podejście zależy od ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-376">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="657a3-377">Podstawowy sposób użycia</span><span class="sxs-lookup"><span data-stu-id="657a3-377">Basic usage</span></span>

<span data-ttu-id="657a3-378">`IHttpClientFactory`Można zarejestrować `AddHttpClient` , wywołując metodę rozszerzenia na `IServiceCollection` , wewnątrz `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="657a3-378">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="657a3-379">Po zarejestrowaniu kod może zaakceptować `IHttpClientFactory` usługi z dowolnego miejsca przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="657a3-379">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="657a3-380">`IHttpClientFactory`Można go użyć do utworzenia `HttpClient` wystąpienia:</span><span class="sxs-lookup"><span data-stu-id="657a3-380">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="657a3-381">Użycie `IHttpClientFactory` w ten sposób jest dobrym sposobem refaktoryzacji istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-381">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="657a3-382">Nie ma to wpływu na sposób `HttpClient` użycia.</span><span class="sxs-lookup"><span data-stu-id="657a3-382">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="657a3-383">W miejscach, w których `HttpClient` są obecnie tworzone wystąpienia, Zastąp te wystąpienia wywołaniem <xref:System.Net.Http.IHttpClientFactory.CreateClient*> .</span><span class="sxs-lookup"><span data-stu-id="657a3-383">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="657a3-384">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="657a3-384">Named clients</span></span>

<span data-ttu-id="657a3-385">Jeśli aplikacja wymaga wielu odrębnych użycia `HttpClient` , z których każdy ma inną konfigurację, opcja służy do korzystania z **nazwanych klientów**.</span><span class="sxs-lookup"><span data-stu-id="657a3-385">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="657a3-386">Konfigurację dla nazwy `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices` .</span><span class="sxs-lookup"><span data-stu-id="657a3-386">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="657a3-387">W powyższym kodzie `AddHttpClient` jest wywoływana, dostarczając nazwę *GitHub*.</span><span class="sxs-lookup"><span data-stu-id="657a3-387">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="657a3-388">Ten klient ma zainstalowaną domyślną konfigurację &mdash; , a w tym adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="657a3-388">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="657a3-389">Przy każdym `CreateClient` wywołaniu jest tworzone nowe wystąpienie programu `HttpClient` i zostanie wywołana akcja konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="657a3-389">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="657a3-390">Aby użyć nazwanego klienta, parametr ciągu może być przekazaniem do `CreateClient` .</span><span class="sxs-lookup"><span data-stu-id="657a3-390">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="657a3-391">Określ nazwę klienta, który ma zostać utworzony:</span><span class="sxs-lookup"><span data-stu-id="657a3-391">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="657a3-392">W powyższym kodzie żądanie nie musi określać nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="657a3-392">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="657a3-393">Można przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-393">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="657a3-394">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="657a3-394">Typed clients</span></span>

<span data-ttu-id="657a3-395">Wpisane komputery klienckie:</span><span class="sxs-lookup"><span data-stu-id="657a3-395">Typed clients:</span></span>

* <span data-ttu-id="657a3-396">Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.</span><span class="sxs-lookup"><span data-stu-id="657a3-396">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="657a3-397">Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.</span><span class="sxs-lookup"><span data-stu-id="657a3-397">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="657a3-398">Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient` .</span><span class="sxs-lookup"><span data-stu-id="657a3-398">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="657a3-399">Na przykład pojedynczy klient z określonym typem może być używany w przypadku pojedynczego punktu końcowego zaplecza i hermetyzował wszystkie logike związane z tym punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="657a3-399">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="657a3-400">Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-400">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="657a3-401">Klient z określonym typem akceptuje `HttpClient` parametr w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="657a3-401">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="657a3-402">W poprzednim kodzie konfiguracja jest przenoszona do określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-402">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="657a3-403">`HttpClient`Obiekt jest ujawniany jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="657a3-403">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="657a3-404">Istnieje możliwość zdefiniowania metod specyficznych dla interfejsu API, które uwidaczniają `HttpClient` funkcjonalność.</span><span class="sxs-lookup"><span data-stu-id="657a3-404">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="657a3-405">`GetAspNetDocsIssues`Metoda hermetyzuje kod potrzebny do zbadania i wyanalizowania najnowszych otwartych problemów z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="657a3-405">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="657a3-406">Aby zarejestrować klienta z określonym typem, <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> można użyć generycznej metody rozszerzenia w programie `Startup.ConfigureServices` , określając klasę klienta z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="657a3-406">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="657a3-407">Typ klienta jest rejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="657a3-407">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="657a3-408">Określony klient może zostać dodany i wykorzystany bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="657a3-408">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="657a3-409">Jeśli jest preferowana, konfiguracja dla klienta z określonym typem może być określona podczas rejestracji w `Startup.ConfigureServices` , a nie w konstruktorze określonego klienta:</span><span class="sxs-lookup"><span data-stu-id="657a3-409">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="657a3-410">Można całkowicie hermetyzować `HttpClient` w ramach określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-410">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="657a3-411">Zamiast uwidaczniać je jako właściwość, można dostarczyć metody publiczne, które wywołują `HttpClient` wystąpienie wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="657a3-411">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="657a3-412">W poprzednim kodzie, `HttpClient` jest przechowywany jako pole prywatne.</span><span class="sxs-lookup"><span data-stu-id="657a3-412">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="657a3-413">Cały dostęp do wykonywania wywołań zewnętrznych odbywa się za pomocą `GetRepos` metody.</span><span class="sxs-lookup"><span data-stu-id="657a3-413">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="657a3-414">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="657a3-414">Generated clients</span></span>

<span data-ttu-id="657a3-415">`IHttpClientFactory` może być używany w połączeniu z innymi bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="657a3-415">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="657a3-416">REFIT to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="657a3-416">Refit is a REST library for .NET.</span></span> <span data-ttu-id="657a3-417">Konwertuje interfejsy API REST na interfejsy na żywo.</span><span class="sxs-lookup"><span data-stu-id="657a3-417">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="657a3-418">Implementacja interfejsu jest generowana dynamicznie przez `RestService` , przy użyciu polecenia, `HttpClient` Aby utworzyć zewnętrzne wywołania http.</span><span class="sxs-lookup"><span data-stu-id="657a3-418">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="657a3-419">Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="657a3-419">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="657a3-420">Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:</span><span class="sxs-lookup"><span data-stu-id="657a3-420">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("https://localhost:5001");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="657a3-421">Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:</span><span class="sxs-lookup"><span data-stu-id="657a3-421">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="657a3-422">Oprogramowanie pośredniczące żądania wychodzące</span><span class="sxs-lookup"><span data-stu-id="657a3-422">Outgoing request middleware</span></span>

<span data-ttu-id="657a3-423">`HttpClient` ma już koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="657a3-423">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="657a3-424">`IHttpClientFactory`Ułatwia definiowanie programów obsługi do zastosowania dla każdego nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-424">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="657a3-425">Obsługuje rejestrację i łańcuchowanie wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="657a3-425">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="657a3-426">Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym.</span><span class="sxs-lookup"><span data-stu-id="657a3-426">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="657a3-427">Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="657a3-427">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="657a3-428">Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="657a3-428">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="657a3-429">Aby utworzyć procedurę obsługi, zdefiniuj klasę pochodną z elementu <xref:System.Net.Http.DelegatingHandler> .</span><span class="sxs-lookup"><span data-stu-id="657a3-429">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="657a3-430">Zastąp `SendAsync` metodę w celu wykonania kodu przed przekazaniem żądania do następnego programu obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="657a3-430">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="657a3-431">Poprzedni kod definiuje procedurę obsługi podstawowej.</span><span class="sxs-lookup"><span data-stu-id="657a3-431">The preceding code defines a basic handler.</span></span> <span data-ttu-id="657a3-432">Sprawdza, czy `X-API-KEY` nagłówek został uwzględniony w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="657a3-432">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="657a3-433">Jeśli brakuje nagłówka, można uniknąć wywołania HTTP i zwrócić odpowiednią odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="657a3-433">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="657a3-434">Podczas rejestracji do konfiguracji można dodać co najmniej jeden program obsługi `HttpClient` .</span><span class="sxs-lookup"><span data-stu-id="657a3-434">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="657a3-435">To zadanie jest realizowane za pośrednictwem metod rozszerzających na <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> .</span><span class="sxs-lookup"><span data-stu-id="657a3-435">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="657a3-436">W poprzednim kodzie, `ValidateHeaderHandler` jest zarejestrowany przy użyciu di.</span><span class="sxs-lookup"><span data-stu-id="657a3-436">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="657a3-437">`IHttpClientFactory`Tworzy oddzielny zakres di dla każdej procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="657a3-437">The `IHttpClientFactory` creates a separate DI scope for each handler.</span></span> <span data-ttu-id="657a3-438">Programy obsługi są bezpłatne, aby zależeć od usług dowolnego zakresu.</span><span class="sxs-lookup"><span data-stu-id="657a3-438">Handlers are free to depend upon services of any scope.</span></span> <span data-ttu-id="657a3-439">Usługi, które są zależne od programów obsługi, są usuwane, gdy program obsługi zostanie usunięty.</span><span class="sxs-lookup"><span data-stu-id="657a3-439">Services that handlers depend upon are disposed when the handler is disposed.</span></span>

<span data-ttu-id="657a3-440">Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując typ do procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="657a3-440">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the type for the handler.</span></span>

<span data-ttu-id="657a3-441">Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="657a3-441">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="657a3-442">Każdy program obsługi otacza następną procedurę obsługi do momentu zakończenia `HttpClientHandler` żądania:</span><span class="sxs-lookup"><span data-stu-id="657a3-442">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="657a3-443">Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="657a3-443">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="657a3-444">Przekaż dane do procedury obsługi przy użyciu polecenia `HttpRequestMessage.Properties` .</span><span class="sxs-lookup"><span data-stu-id="657a3-444">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="657a3-445">Użyj, `IHttpContextAccessor` Aby uzyskać dostęp do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="657a3-445">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="657a3-446">Utwórz niestandardowy `AsyncLocal` obiekt magazynu, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="657a3-446">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="657a3-447">Korzystanie z programów obsługi opartych na Polly</span><span class="sxs-lookup"><span data-stu-id="657a3-447">Use Polly-based handlers</span></span>

<span data-ttu-id="657a3-448">`IHttpClientFactory` integruje się z popularną biblioteką innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="657a3-448">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="657a3-449">Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="657a3-449">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="657a3-450">Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.</span><span class="sxs-lookup"><span data-stu-id="657a3-450">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="657a3-451">Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi `HttpClient` wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="657a3-451">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="657a3-452">Rozszerzenia Polly:</span><span class="sxs-lookup"><span data-stu-id="657a3-452">The Polly extensions:</span></span>

* <span data-ttu-id="657a3-453">Obsługa dodawania programów obsługi opartych na Pollych do klientów.</span><span class="sxs-lookup"><span data-stu-id="657a3-453">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="657a3-454">Można go użyć po zainstalowaniu pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="657a3-454">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="657a3-455">Pakiet nie znajduje się w ASP.NET Core udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="657a3-455">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="657a3-456">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="657a3-456">Handle transient faults</span></span>

<span data-ttu-id="657a3-457">Najczęstsze błędy występują, gdy zewnętrzne wywołania HTTP są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="657a3-457">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="657a3-458">Dołączono wygodną metodę rozszerzenia `AddTransientHttpErrorPolicy` , która umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="657a3-458">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="657a3-459">Zasady skonfigurowane przy użyciu tego uchwytu metody rozszerzenia `HttpRequestException` , odpowiedzi HTTP 5xx i odpowiedzi http 408.</span><span class="sxs-lookup"><span data-stu-id="657a3-459">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="657a3-460">`AddTransientHttpErrorPolicy`Rozszerzenie może być używane w `Startup.ConfigureServices` .</span><span class="sxs-lookup"><span data-stu-id="657a3-460">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="657a3-461">Rozszerzenie zapewnia dostęp do `PolicyBuilder` obiektu skonfigurowanego do obsługi błędów reprezentujących możliwy błąd przejściowy:</span><span class="sxs-lookup"><span data-stu-id="657a3-461">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="657a3-462">W poprzednim kodzie `WaitAndRetryAsync` zdefiniowane są zasady.</span><span class="sxs-lookup"><span data-stu-id="657a3-462">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="657a3-463">Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.</span><span class="sxs-lookup"><span data-stu-id="657a3-463">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="657a3-464">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="657a3-464">Dynamically select policies</span></span>

<span data-ttu-id="657a3-465">Istnieją dodatkowe metody rozszerzające, których można użyć do dodawania programów obsługi opartych na Polly.</span><span class="sxs-lookup"><span data-stu-id="657a3-465">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="657a3-466">Jedno takie rozszerzenie ma `AddPolicyHandler` wiele przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="657a3-466">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="657a3-467">Jedno Przeciążenie umożliwia sprawdzenie żądania podczas definiowania zasad, które mają zostać zastosowane:</span><span class="sxs-lookup"><span data-stu-id="657a3-467">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="657a3-468">W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="657a3-468">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="657a3-469">Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="657a3-469">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="657a3-470">Dodaj wiele programów obsługi Polly</span><span class="sxs-lookup"><span data-stu-id="657a3-470">Add multiple Polly handlers</span></span>

<span data-ttu-id="657a3-471">Jest to typowy sposób zagnieżdżania zasad Pollyymi w celu zapewnienia zwiększonej funkcjonalności:</span><span class="sxs-lookup"><span data-stu-id="657a3-471">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="657a3-472">W poprzednim przykładzie dodawane są dwa programy obsługi.</span><span class="sxs-lookup"><span data-stu-id="657a3-472">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="657a3-473">Pierwsze używa rozszerzenia, `AddTransientHttpErrorPolicy` Aby dodać zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="657a3-473">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="657a3-474">Nieudane żądania są ponawiane do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="657a3-474">Failed requests are retried up to three times.</span></span> <span data-ttu-id="657a3-475">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="657a3-475">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="657a3-476">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli pięć nieudanych prób wystąpi sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="657a3-476">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="657a3-477">Zasady wyłącznika są stanowe.</span><span class="sxs-lookup"><span data-stu-id="657a3-477">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="657a3-478">Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="657a3-478">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="657a3-479">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="657a3-479">Add policies from the Polly registry</span></span>

<span data-ttu-id="657a3-480">Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry` .</span><span class="sxs-lookup"><span data-stu-id="657a3-480">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="657a3-481">Zapewniana jest metoda rozszerzająca, która umożliwia dodanie programu obsługi przy użyciu zasad z rejestru:</span><span class="sxs-lookup"><span data-stu-id="657a3-481">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="657a3-482">W poprzednim kodzie dwie zasady są rejestrowane po `PolicyRegistry` dodaniu do `ServiceCollection` .</span><span class="sxs-lookup"><span data-stu-id="657a3-482">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="657a3-483">Aby użyć zasad z rejestru, `AddPolicyHandlerFromRegistry` Metoda jest używana, przekazując nazwę zasad do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="657a3-483">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="657a3-484">Więcej informacji na temat integracji z programem `IHttpClientFactory` i Polly można znaleźć w witrynie [typu wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="657a3-484">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="657a3-485">HttpClient i zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="657a3-485">HttpClient and lifetime management</span></span>

<span data-ttu-id="657a3-486">Nowe `HttpClient` wystąpienie jest zwracane za każdym razem `CreateClient` , gdy jest wywoływana w `IHttpClientFactory` .</span><span class="sxs-lookup"><span data-stu-id="657a3-486">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="657a3-487">Istnieje <xref:System.Net.Http.HttpMessageHandler> za nazwany klient.</span><span class="sxs-lookup"><span data-stu-id="657a3-487">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="657a3-488">Fabryka zarządza okresami istnienia `HttpMessageHandler` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="657a3-488">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="657a3-489">`IHttpClientFactory` pule `HttpMessageHandler` wystąpień tworzonych przez fabrykę w celu zmniejszenia zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="657a3-489">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="657a3-490">`HttpMessageHandler`Wystąpienie może być ponownie używane z puli podczas tworzenia nowego `HttpClient` wystąpienia, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="657a3-490">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="657a3-491">Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="657a3-491">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="657a3-492">Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń.</span><span class="sxs-lookup"><span data-stu-id="657a3-492">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="657a3-493">Niektóre programy obsługi powodują również, że połączenia są otwarte w nieskończoność, co może uniemożliwić obsłużenie zmian DNS przez program obsługi.</span><span class="sxs-lookup"><span data-stu-id="657a3-493">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="657a3-494">Domyślny okres istnienia programu obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="657a3-494">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="657a3-495">Wartość domyślną można przesłonić na podstawie nazwy klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-495">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="657a3-496">Aby zastąpić ten element, wywołaj metodę <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> `IHttpClientBuilder` zwracaną podczas tworzenia klienta:</span><span class="sxs-lookup"><span data-stu-id="657a3-496">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="657a3-497">Usuwanie klienta nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="657a3-497">Disposal of the client isn't required.</span></span> <span data-ttu-id="657a3-498">Likwidacja anuluje żądania wychodzące i gwarantuje, że danego `HttpClient` wystąpienia nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*> .</span><span class="sxs-lookup"><span data-stu-id="657a3-498">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="657a3-499">`IHttpClientFactory` śledzi i usuwa zasoby używane przez `HttpClient` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="657a3-499">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="657a3-500">`HttpClient`Wystąpienia mogą być zwykle traktowane jako obiekty .NET, które nie wymagają usuwania.</span><span class="sxs-lookup"><span data-stu-id="657a3-500">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="657a3-501">Utrzymywanie pojedynczego `HttpClient` wystąpienia aktywności przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory` .</span><span class="sxs-lookup"><span data-stu-id="657a3-501">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="657a3-502">Ten wzorzec będzie niepotrzebny po migracji do programu `IHttpClientFactory` .</span><span class="sxs-lookup"><span data-stu-id="657a3-502">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="657a3-503">Alternatywy do IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="657a3-503">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="657a3-504">Używanie `IHttpClientFactory` w aplikacji z obsługą podwójnego zapobiegania:</span><span class="sxs-lookup"><span data-stu-id="657a3-504">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="657a3-505">Problemy z wyczerpaniem zasobów przez `HttpMessageHandler` wystąpienia puli.</span><span class="sxs-lookup"><span data-stu-id="657a3-505">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="657a3-506">Nieodświeżone problemy z usługą DNS przez cykliczne `HttpMessageHandler` wystąpienia w regularnych odstępach czasu.</span><span class="sxs-lookup"><span data-stu-id="657a3-506">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="657a3-507">Istnieją alternatywne sposoby rozwiązywania powyższych problemów przy użyciu wystąpienia długotrwałego <xref:System.Net.Http.SocketsHttpHandler> .</span><span class="sxs-lookup"><span data-stu-id="657a3-507">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="657a3-508">Utwórz wystąpienie, `SocketsHttpHandler` gdy aplikacja zostanie uruchomiona i użyje jej na potrzeby życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-508">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="657a3-509">Skonfiguruj <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> odpowiednią wartość na podstawie czasu odświeżania DNS.</span><span class="sxs-lookup"><span data-stu-id="657a3-509">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="657a3-510">Utwórz `HttpClient` wystąpienia przy użyciu programu `new HttpClient(handler, disposeHandler: false)` zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="657a3-510">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="657a3-511">Powyższe podejścia rozwiązują problemy z zarządzaniem zasobami, które `IHttpClientFactory` rozwiązują się w podobny sposób.</span><span class="sxs-lookup"><span data-stu-id="657a3-511">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="657a3-512">`SocketsHttpHandler`Udostępnia połączenia między `HttpClient` wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="657a3-512">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="657a3-513">To udostępnianie uniemożliwia wyczerpanie gniazda.</span><span class="sxs-lookup"><span data-stu-id="657a3-513">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="657a3-514">`SocketsHttpHandler`Cykle połączeń zgodnie z, aby `PooledConnectionLifetime` uniknąć nieodświeżonych problemów z usługą DNS.</span><span class="sxs-lookup"><span data-stu-id="657a3-514">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="657a3-515">Cookiewolumin</span><span class="sxs-lookup"><span data-stu-id="657a3-515">Cookies</span></span>

<span data-ttu-id="657a3-516">`HttpMessageHandler`Wystąpienia w puli powoduje, że `CookieContainer` obiekty są udostępniane.</span><span class="sxs-lookup"><span data-stu-id="657a3-516">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="657a3-517">Nieoczekiwane `CookieContainer` udostępnianie obiektów często prowadzi do niepoprawnego kodu.</span><span class="sxs-lookup"><span data-stu-id="657a3-517">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="657a3-518">W przypadku aplikacji, które wymagają usługi cookie s, należy rozważyć następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="657a3-518">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="657a3-519">Wyłączanie cookie obsługi automatycznej</span><span class="sxs-lookup"><span data-stu-id="657a3-519">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="657a3-520">Unikanie `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="657a3-520">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="657a3-521">Wywołanie <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> wyłączania automatycznej cookie obsługi:</span><span class="sxs-lookup"><span data-stu-id="657a3-521">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="657a3-522">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="657a3-522">Logging</span></span>

<span data-ttu-id="657a3-523">Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="657a3-523">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="657a3-524">Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="657a3-524">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="657a3-525">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="657a3-525">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="657a3-526">Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu.</span><span class="sxs-lookup"><span data-stu-id="657a3-526">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="657a3-527">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` .</span><span class="sxs-lookup"><span data-stu-id="657a3-527">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="657a3-528">Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="657a3-528">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="657a3-529">Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku.</span><span class="sxs-lookup"><span data-stu-id="657a3-529">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="657a3-530">W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.</span><span class="sxs-lookup"><span data-stu-id="657a3-530">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="657a3-531">Rejestrowanie odbywa się również w potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="657a3-531">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="657a3-532">W przykładzie *MyNamedClient* te komunikaty są rejestrowane w kategorii dzienników `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` .</span><span class="sxs-lookup"><span data-stu-id="657a3-532">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="657a3-533">W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania w sieci.</span><span class="sxs-lookup"><span data-stu-id="657a3-533">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="657a3-534">W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="657a3-534">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="657a3-535">Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="657a3-535">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="657a3-536">Może to obejmować zmiany nagłówków żądań, na przykład lub do kodu stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="657a3-536">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="657a3-537">Dołączenie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="657a3-537">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="657a3-538">Konfigurowanie HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="657a3-538">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="657a3-539">Może być konieczne sterowanie konfiguracją wewnętrznej `HttpMessageHandler` używanej przez klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-539">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="657a3-540">`IHttpClientBuilder`Jest zwracany podczas dodawania nazwanych lub wpisanych klientów.</span><span class="sxs-lookup"><span data-stu-id="657a3-540">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="657a3-541"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>Metoda rozszerzająca może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="657a3-541">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="657a3-542">Delegat służy do tworzenia i konfigurowania głównej `HttpMessageHandler` używanej przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="657a3-542">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="657a3-543">Korzystanie z IHttpClientFactory w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="657a3-543">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="657a3-544">W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="657a3-544">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="657a3-545">Microsoft. Extensions. hosting</span><span class="sxs-lookup"><span data-stu-id="657a3-545">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="657a3-546">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="657a3-546">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="657a3-547">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="657a3-547">In the following example:</span></span>

* <span data-ttu-id="657a3-548"><xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usługi [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="657a3-548"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="657a3-549">`MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient` .</span><span class="sxs-lookup"><span data-stu-id="657a3-549">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="657a3-550">`HttpClient` służy do pobierania strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="657a3-550">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="657a3-551">`Main` tworzy zakres do wykonania `GetPage` metody usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="657a3-551">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="additional-resources"></a><span data-ttu-id="657a3-552">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="657a3-552">Additional resources</span></span>

* [<span data-ttu-id="657a3-553">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="657a3-553">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="657a3-554">Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="657a3-554">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="657a3-555">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="657a3-555">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="657a3-556">[Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak)i [Steve Gordon](https://github.com/stevejgordon)</span><span class="sxs-lookup"><span data-stu-id="657a3-556">By [Glenn Condron](https://github.com/glennc), [Ryan Nowak](https://github.com/rynowak), and [Steve Gordon](https://github.com/stevejgordon)</span></span>

<span data-ttu-id="657a3-557"><xref:System.Net.Http.IHttpClientFactory>Może być zarejestrowany i używany do konfigurowania i tworzenia <xref:System.Net.Http.HttpClient> wystąpień w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-557">An <xref:System.Net.Http.IHttpClientFactory> can be registered and used to configure and create <xref:System.Net.Http.HttpClient> instances in an app.</span></span> <span data-ttu-id="657a3-558">Oferuje następujące korzyści:</span><span class="sxs-lookup"><span data-stu-id="657a3-558">It offers the following benefits:</span></span>

* <span data-ttu-id="657a3-559">Zapewnia centralną lokalizację do nazywania i konfigurowania `HttpClient` wystąpień logicznych.</span><span class="sxs-lookup"><span data-stu-id="657a3-559">Provides a central location for naming and configuring logical `HttpClient` instances.</span></span> <span data-ttu-id="657a3-560">Na przykład klient usługi *GitHub* można zarejestrować i skonfigurować do uzyskiwania dostępu do usługi [GitHub](https://github.com/).</span><span class="sxs-lookup"><span data-stu-id="657a3-560">For example, a *github* client can be registered and configured to access [GitHub](https://github.com/).</span></span> <span data-ttu-id="657a3-561">Domyślny klient można zarejestrować do innych celów.</span><span class="sxs-lookup"><span data-stu-id="657a3-561">A default client can be registered for other purposes.</span></span>
* <span data-ttu-id="657a3-562">Skodyfikował koncepcję wychodzącego oprogramowania pośredniczącego przez delegowanie programów obsługi w programie `HttpClient` i udostępnia rozszerzenia dla oprogramowania pośredniczącego opartego na Polly, aby skorzystać z tego programu.</span><span class="sxs-lookup"><span data-stu-id="657a3-562">Codifies the concept of outgoing middleware via delegating handlers in `HttpClient` and provides extensions for Polly-based middleware to take advantage of that.</span></span>
* <span data-ttu-id="657a3-563">Zarządza buforowaniem i okresem istnienia podstawowych `HttpClientMessageHandler` wystąpień, aby uniknąć typowych problemów z usługą DNS występujących podczas ręcznego zarządzania `HttpClient` okresami istnienia.</span><span class="sxs-lookup"><span data-stu-id="657a3-563">Manages the pooling and lifetime of underlying `HttpClientMessageHandler` instances to avoid common DNS problems that occur when manually managing `HttpClient` lifetimes.</span></span>
* <span data-ttu-id="657a3-564">Dodaje konfigurowalne środowisko rejestrowania (za pośrednictwem programu `ILogger` ) dla wszystkich żądań wysyłanych przez klientów utworzonych przez fabrykę.</span><span class="sxs-lookup"><span data-stu-id="657a3-564">Adds a configurable logging experience (via `ILogger`) for all requests sent through clients created by the factory.</span></span>

<span data-ttu-id="657a3-565">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/http-requests/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="657a3-565">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/http-requests/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="657a3-566">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="657a3-566">Prerequisites</span></span>

<span data-ttu-id="657a3-567">Projekty docelowe .NET Framework wymagają instalacji pakietu NuGet [Microsoft. Extensions. http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) .</span><span class="sxs-lookup"><span data-stu-id="657a3-567">Projects targeting .NET Framework require installation of the [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) NuGet package.</span></span> <span data-ttu-id="657a3-568">Projekty przeznaczone dla platformy .NET Core i odwołujące się do [pakietu Microsoft. AspNetCore. appbinding](xref:fundamentals/metapackage-app) zawierają już `Microsoft.Extensions.Http` pakiet.</span><span class="sxs-lookup"><span data-stu-id="657a3-568">Projects that target .NET Core and reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) already include the `Microsoft.Extensions.Http` package.</span></span>

## <a name="consumption-patterns"></a><span data-ttu-id="657a3-569">Wzorce zużycia</span><span class="sxs-lookup"><span data-stu-id="657a3-569">Consumption patterns</span></span>

<span data-ttu-id="657a3-570">`IHttpClientFactory`W aplikacji można używać kilku sposobów:</span><span class="sxs-lookup"><span data-stu-id="657a3-570">There are several ways `IHttpClientFactory` can be used in an app:</span></span>

* [<span data-ttu-id="657a3-571">Podstawowy sposób użycia</span><span class="sxs-lookup"><span data-stu-id="657a3-571">Basic usage</span></span>](#basic-usage)
* [<span data-ttu-id="657a3-572">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="657a3-572">Named clients</span></span>](#named-clients)
* [<span data-ttu-id="657a3-573">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="657a3-573">Typed clients</span></span>](#typed-clients)
* [<span data-ttu-id="657a3-574">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="657a3-574">Generated clients</span></span>](#generated-clients)

<span data-ttu-id="657a3-575">Żadna z nich nie jest ściśle wyższa od siebie.</span><span class="sxs-lookup"><span data-stu-id="657a3-575">None of them are strictly superior to another.</span></span> <span data-ttu-id="657a3-576">Najlepsze podejście zależy od ograniczeń aplikacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-576">The best approach depends upon the app's constraints.</span></span>

### <a name="basic-usage"></a><span data-ttu-id="657a3-577">Podstawowy sposób użycia</span><span class="sxs-lookup"><span data-stu-id="657a3-577">Basic usage</span></span>

<span data-ttu-id="657a3-578">`IHttpClientFactory`Można zarejestrować `AddHttpClient` , wywołując metodę rozszerzenia na `IServiceCollection` , wewnątrz `Startup.ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="657a3-578">The `IHttpClientFactory` can be registered by calling the `AddHttpClient` extension method on the `IServiceCollection`, inside the `Startup.ConfigureServices` method.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet1)]

<span data-ttu-id="657a3-579">Po zarejestrowaniu kod może zaakceptować `IHttpClientFactory` usługi z dowolnego miejsca przy użyciu [iniekcji zależności (di)](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="657a3-579">Once registered, code can accept an `IHttpClientFactory` anywhere services can be injected with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="657a3-580">`IHttpClientFactory`Można go użyć do utworzenia `HttpClient` wystąpienia:</span><span class="sxs-lookup"><span data-stu-id="657a3-580">The `IHttpClientFactory` can be used to create an `HttpClient` instance:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/BasicUsage.cshtml.cs?name=snippet1&highlight=9-12,21)]

<span data-ttu-id="657a3-581">Użycie `IHttpClientFactory` w ten sposób jest dobrym sposobem refaktoryzacji istniejącej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-581">Using `IHttpClientFactory` in this fashion is a good way to refactor an existing app.</span></span> <span data-ttu-id="657a3-582">Nie ma to wpływu na sposób `HttpClient` użycia.</span><span class="sxs-lookup"><span data-stu-id="657a3-582">It has no impact on the way `HttpClient` is used.</span></span> <span data-ttu-id="657a3-583">W miejscach, w których `HttpClient` są obecnie tworzone wystąpienia, Zastąp te wystąpienia wywołaniem <xref:System.Net.Http.IHttpClientFactory.CreateClient*> .</span><span class="sxs-lookup"><span data-stu-id="657a3-583">In places where `HttpClient` instances are currently created, replace those occurrences with a call to <xref:System.Net.Http.IHttpClientFactory.CreateClient*>.</span></span>

### <a name="named-clients"></a><span data-ttu-id="657a3-584">Nazwani klienci</span><span class="sxs-lookup"><span data-stu-id="657a3-584">Named clients</span></span>

<span data-ttu-id="657a3-585">Jeśli aplikacja wymaga wielu odrębnych użycia `HttpClient` , z których każdy ma inną konfigurację, opcja służy do korzystania z **nazwanych klientów**.</span><span class="sxs-lookup"><span data-stu-id="657a3-585">If an app requires many distinct uses of `HttpClient`, each with a different configuration, an option is to use **named clients**.</span></span> <span data-ttu-id="657a3-586">Konfigurację dla nazwy `HttpClient` można określić podczas rejestracji w `Startup.ConfigureServices` .</span><span class="sxs-lookup"><span data-stu-id="657a3-586">Configuration for a named `HttpClient` can be specified during registration in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet2)]

<span data-ttu-id="657a3-587">W powyższym kodzie `AddHttpClient` jest wywoływana, dostarczając nazwę *GitHub*.</span><span class="sxs-lookup"><span data-stu-id="657a3-587">In the preceding code, `AddHttpClient` is called, providing the name *github*.</span></span> <span data-ttu-id="657a3-588">Ten klient ma zainstalowaną domyślną konfigurację &mdash; , a w tym adres podstawowy i dwa nagłówki wymagane do pracy z interfejsem API usługi GitHub.</span><span class="sxs-lookup"><span data-stu-id="657a3-588">This client has some default configuration applied&mdash;namely the base address and two headers required to work with the GitHub API.</span></span>

<span data-ttu-id="657a3-589">Przy każdym `CreateClient` wywołaniu jest tworzone nowe wystąpienie programu `HttpClient` i zostanie wywołana akcja konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="657a3-589">Each time `CreateClient` is called, a new instance of `HttpClient` is created and the configuration action is called.</span></span>

<span data-ttu-id="657a3-590">Aby użyć nazwanego klienta, parametr ciągu może być przekazaniem do `CreateClient` .</span><span class="sxs-lookup"><span data-stu-id="657a3-590">To consume a named client, a string parameter can be passed to `CreateClient`.</span></span> <span data-ttu-id="657a3-591">Określ nazwę klienta, który ma zostać utworzony:</span><span class="sxs-lookup"><span data-stu-id="657a3-591">Specify the name of the client to be created:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/NamedClient.cshtml.cs?name=snippet1&highlight=21)]

<span data-ttu-id="657a3-592">W powyższym kodzie żądanie nie musi określać nazwy hosta.</span><span class="sxs-lookup"><span data-stu-id="657a3-592">In the preceding code, the request doesn't need to specify a hostname.</span></span> <span data-ttu-id="657a3-593">Można przekazać tylko ścieżkę, ponieważ jest używany adres podstawowy skonfigurowany dla klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-593">It can pass just the path, since the base address configured for the client is used.</span></span>

### <a name="typed-clients"></a><span data-ttu-id="657a3-594">Wpisane komputery klienckie</span><span class="sxs-lookup"><span data-stu-id="657a3-594">Typed clients</span></span>

<span data-ttu-id="657a3-595">Wpisane komputery klienckie:</span><span class="sxs-lookup"><span data-stu-id="657a3-595">Typed clients:</span></span>

* <span data-ttu-id="657a3-596">Podaj te same możliwości co nazwanych klientów bez konieczności używania ciągów jako kluczy.</span><span class="sxs-lookup"><span data-stu-id="657a3-596">Provide the same capabilities as named clients without the need to use strings as keys.</span></span>
* <span data-ttu-id="657a3-597">Zapewnia obsługę funkcji IntelliSense i kompilatora podczas konsumowania klientów.</span><span class="sxs-lookup"><span data-stu-id="657a3-597">Provides IntelliSense and compiler help when consuming clients.</span></span>
* <span data-ttu-id="657a3-598">Podaj jedną lokalizację do konfiguracji i współpracy z konkretną `HttpClient` .</span><span class="sxs-lookup"><span data-stu-id="657a3-598">Provide a single location to configure and interact with a particular `HttpClient`.</span></span> <span data-ttu-id="657a3-599">Na przykład pojedynczy klient z określonym typem może być używany w przypadku pojedynczego punktu końcowego zaplecza i hermetyzował wszystkie logike związane z tym punktem końcowym.</span><span class="sxs-lookup"><span data-stu-id="657a3-599">For example, a single typed client might be used for a single backend endpoint and encapsulate all logic dealing with that endpoint.</span></span>
* <span data-ttu-id="657a3-600">Pracuj z opcją DI i można ją wstrzyknąć, gdy jest to wymagane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-600">Work with DI and can be injected where required in your app.</span></span>

<span data-ttu-id="657a3-601">Klient z określonym typem akceptuje `HttpClient` parametr w jego konstruktorze:</span><span class="sxs-lookup"><span data-stu-id="657a3-601">A typed client accepts an `HttpClient` parameter in its constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/GitHubService.cs?name=snippet1&highlight=5)]

<span data-ttu-id="657a3-602">W poprzednim kodzie konfiguracja jest przenoszona do określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-602">In the preceding code, the configuration is moved into the typed client.</span></span> <span data-ttu-id="657a3-603">`HttpClient`Obiekt jest ujawniany jako właściwość publiczna.</span><span class="sxs-lookup"><span data-stu-id="657a3-603">The `HttpClient` object is exposed as a public property.</span></span> <span data-ttu-id="657a3-604">Istnieje możliwość zdefiniowania metod specyficznych dla interfejsu API, które uwidaczniają `HttpClient` funkcjonalność.</span><span class="sxs-lookup"><span data-stu-id="657a3-604">It's possible to define API-specific methods that expose `HttpClient` functionality.</span></span> <span data-ttu-id="657a3-605">`GetAspNetDocsIssues`Metoda hermetyzuje kod potrzebny do zbadania i wyanalizowania najnowszych otwartych problemów z repozytorium GitHub.</span><span class="sxs-lookup"><span data-stu-id="657a3-605">The `GetAspNetDocsIssues` method encapsulates the code needed to query for and parse out the latest open issues from a GitHub repository.</span></span>

<span data-ttu-id="657a3-606">Aby zarejestrować klienta z określonym typem, <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> można użyć generycznej metody rozszerzenia w programie `Startup.ConfigureServices` , określając klasę klienta z określonym typem:</span><span class="sxs-lookup"><span data-stu-id="657a3-606">To register a typed client, the generic <xref:Microsoft.Extensions.DependencyInjection.HttpClientFactoryServiceCollectionExtensions.AddHttpClient*> extension method can be used within `Startup.ConfigureServices`, specifying the typed client class:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet3)]

<span data-ttu-id="657a3-607">Typ klienta jest rejestrowany jako przejściowy z DI.</span><span class="sxs-lookup"><span data-stu-id="657a3-607">The typed client is registered as transient with DI.</span></span> <span data-ttu-id="657a3-608">Określony klient może zostać dodany i wykorzystany bezpośrednio:</span><span class="sxs-lookup"><span data-stu-id="657a3-608">The typed client can be injected and consumed directly:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Pages/TypedClient.cshtml.cs?name=snippet1&highlight=11-14,20)]

<span data-ttu-id="657a3-609">Jeśli jest preferowana, konfiguracja dla klienta z określonym typem może być określona podczas rejestracji w `Startup.ConfigureServices` , a nie w konstruktorze określonego klienta:</span><span class="sxs-lookup"><span data-stu-id="657a3-609">If preferred, the configuration for a typed client can be specified during registration in `Startup.ConfigureServices`, rather than in the typed client's constructor:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet4)]

<span data-ttu-id="657a3-610">Można całkowicie hermetyzować `HttpClient` w ramach określonego klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-610">It's possible to entirely encapsulate the `HttpClient` within a typed client.</span></span> <span data-ttu-id="657a3-611">Zamiast uwidaczniać je jako właściwość, można dostarczyć metody publiczne, które wywołują `HttpClient` wystąpienie wewnętrznie.</span><span class="sxs-lookup"><span data-stu-id="657a3-611">Rather than exposing it as a property, public methods can be provided which call the `HttpClient` instance internally.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/GitHub/RepoService.cs?name=snippet1&highlight=4)]

<span data-ttu-id="657a3-612">W poprzednim kodzie, `HttpClient` jest przechowywany jako pole prywatne.</span><span class="sxs-lookup"><span data-stu-id="657a3-612">In the preceding code, the `HttpClient` is stored as a private field.</span></span> <span data-ttu-id="657a3-613">Cały dostęp do wykonywania wywołań zewnętrznych odbywa się za pomocą `GetRepos` metody.</span><span class="sxs-lookup"><span data-stu-id="657a3-613">All access to make external calls goes through the `GetRepos` method.</span></span>

### <a name="generated-clients"></a><span data-ttu-id="657a3-614">Wygenerowane klienci</span><span class="sxs-lookup"><span data-stu-id="657a3-614">Generated clients</span></span>

<span data-ttu-id="657a3-615">`IHttpClientFactory` może być używany w połączeniu z innymi bibliotekami innych firm, takimi jak [REFIT](https://github.com/paulcbetts/refit).</span><span class="sxs-lookup"><span data-stu-id="657a3-615">`IHttpClientFactory` can be used in combination with other third-party libraries such as [Refit](https://github.com/paulcbetts/refit).</span></span> <span data-ttu-id="657a3-616">REFIT to biblioteka REST dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="657a3-616">Refit is a REST library for .NET.</span></span> <span data-ttu-id="657a3-617">Konwertuje interfejsy API REST na interfejsy na żywo.</span><span class="sxs-lookup"><span data-stu-id="657a3-617">It converts REST APIs into live interfaces.</span></span> <span data-ttu-id="657a3-618">Implementacja interfejsu jest generowana dynamicznie przez `RestService` , przy użyciu polecenia, `HttpClient` Aby utworzyć zewnętrzne wywołania http.</span><span class="sxs-lookup"><span data-stu-id="657a3-618">An implementation of the interface is generated dynamically by the `RestService`, using `HttpClient` to make the external HTTP calls.</span></span>

<span data-ttu-id="657a3-619">Interfejs i odpowiedź są zdefiniowane do reprezentowania zewnętrznego interfejsu API i jego odpowiedzi:</span><span class="sxs-lookup"><span data-stu-id="657a3-619">An interface and a reply are defined to represent the external API and its response:</span></span>

```csharp
public interface IHelloClient
{
    [Get("/helloworld")]
    Task<Reply> GetMessageAsync();
}

public class Reply
{
    public string Message { get; set; }
}
```

<span data-ttu-id="657a3-620">Można dodać klienta z określonym typem, używając REFIT do wygenerowania implementacji:</span><span class="sxs-lookup"><span data-stu-id="657a3-620">A typed client can be added, using Refit to generate the implementation:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHttpClient("hello", c =>
    {
        c.BaseAddress = new Uri("http://localhost:5000");
    })
    .AddTypedClient(c => Refit.RestService.For<IHelloClient>(c));

    services.AddMvc();
}
```

<span data-ttu-id="657a3-621">Zdefiniowany interfejs może być używany, w razie potrzeby, z implementacją podaną przez DI i REFIT:</span><span class="sxs-lookup"><span data-stu-id="657a3-621">The defined interface can be consumed where necessary, with the implementation provided by DI and Refit:</span></span>

```csharp
[ApiController]
public class ValuesController : ControllerBase
{
    private readonly IHelloClient _client;

    public ValuesController(IHelloClient client)
    {
        _client = client;
    }

    [HttpGet("/")]
    public async Task<ActionResult<Reply>> Index()
    {
        return await _client.GetMessageAsync();
    }
}
```

## <a name="outgoing-request-middleware"></a><span data-ttu-id="657a3-622">Oprogramowanie pośredniczące żądania wychodzące</span><span class="sxs-lookup"><span data-stu-id="657a3-622">Outgoing request middleware</span></span>

<span data-ttu-id="657a3-623">`HttpClient` ma już koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="657a3-623">`HttpClient` already has the concept of delegating handlers that can be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="657a3-624">`IHttpClientFactory`Ułatwia definiowanie programów obsługi do zastosowania dla każdego nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-624">The `IHttpClientFactory` makes it easy to define the handlers to apply for each named client.</span></span> <span data-ttu-id="657a3-625">Obsługuje rejestrację i łańcuchowanie wielu programów obsługi w celu utworzenia potoku pośredniczącego żądania wychodzącego.</span><span class="sxs-lookup"><span data-stu-id="657a3-625">It supports registration and chaining of multiple handlers to build an outgoing request middleware pipeline.</span></span> <span data-ttu-id="657a3-626">Każdy z tych programów obsługi jest w stanie wykonać zadania przed i po żądaniu wychodzącym.</span><span class="sxs-lookup"><span data-stu-id="657a3-626">Each of these handlers is able to perform work before and after the outgoing request.</span></span> <span data-ttu-id="657a3-627">Ten wzorzec jest podobny do przychodzącego potoku oprogramowania pośredniczącego w ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="657a3-627">This pattern is similar to the inbound middleware pipeline in ASP.NET Core.</span></span> <span data-ttu-id="657a3-628">Wzorzec zapewnia mechanizm zarządzania problemami z wycinaniem między żądaniami HTTP, takimi jak buforowanie, obsługa błędów, serializacja i rejestrowanie.</span><span class="sxs-lookup"><span data-stu-id="657a3-628">The pattern provides a mechanism to manage cross-cutting concerns around HTTP requests, including caching, error handling, serialization, and logging.</span></span>

<span data-ttu-id="657a3-629">Aby utworzyć procedurę obsługi, zdefiniuj klasę pochodną z elementu <xref:System.Net.Http.DelegatingHandler> .</span><span class="sxs-lookup"><span data-stu-id="657a3-629">To create a handler, define a class deriving from <xref:System.Net.Http.DelegatingHandler>.</span></span> <span data-ttu-id="657a3-630">Zastąp `SendAsync` metodę w celu wykonania kodu przed przekazaniem żądania do następnego programu obsługi w potoku:</span><span class="sxs-lookup"><span data-stu-id="657a3-630">Override the `SendAsync` method to execute code before passing the request to the next handler in the pipeline:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Handlers/ValidateHeaderHandler.cs?name=snippet1)]

<span data-ttu-id="657a3-631">Poprzedni kod definiuje procedurę obsługi podstawowej.</span><span class="sxs-lookup"><span data-stu-id="657a3-631">The preceding code defines a basic handler.</span></span> <span data-ttu-id="657a3-632">Sprawdza, czy `X-API-KEY` nagłówek został uwzględniony w żądaniu.</span><span class="sxs-lookup"><span data-stu-id="657a3-632">It checks to see if an `X-API-KEY` header has been included on the request.</span></span> <span data-ttu-id="657a3-633">Jeśli brakuje nagłówka, można uniknąć wywołania HTTP i zwrócić odpowiednią odpowiedź.</span><span class="sxs-lookup"><span data-stu-id="657a3-633">If the header is missing, it can avoid the HTTP call and return a suitable response.</span></span>

<span data-ttu-id="657a3-634">Podczas rejestracji do konfiguracji można dodać co najmniej jeden program obsługi `HttpClient` .</span><span class="sxs-lookup"><span data-stu-id="657a3-634">During registration, one or more handlers can be added to the configuration for an `HttpClient`.</span></span> <span data-ttu-id="657a3-635">To zadanie jest realizowane za pośrednictwem metod rozszerzających na <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder> .</span><span class="sxs-lookup"><span data-stu-id="657a3-635">This task is accomplished via extension methods on the <xref:Microsoft.Extensions.DependencyInjection.IHttpClientBuilder>.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet5)]

<span data-ttu-id="657a3-636">W poprzednim kodzie, `ValidateHeaderHandler` jest zarejestrowany przy użyciu di.</span><span class="sxs-lookup"><span data-stu-id="657a3-636">In the preceding code, the `ValidateHeaderHandler` is registered with DI.</span></span> <span data-ttu-id="657a3-637">Procedura obsługi **musi** być zarejestrowana w programie di jako przejściowa usługa, nigdy nie jest objęta zakresem.</span><span class="sxs-lookup"><span data-stu-id="657a3-637">The handler **must** be registered in DI as a transient service, never scoped.</span></span> <span data-ttu-id="657a3-638">Jeśli program obsługi jest zarejestrowany jako usługa objęta zakresem, a wszystkie usługi, od których zależy program obsługi, są jednorazowe:</span><span class="sxs-lookup"><span data-stu-id="657a3-638">If the handler is registered as a scoped service and any services that the handler depends upon are disposable:</span></span>

* <span data-ttu-id="657a3-639">Usługi programu obsługi mogły zostać zlikwidowane, zanim program obsługi znajdzie się poza zakresem.</span><span class="sxs-lookup"><span data-stu-id="657a3-639">The handler's services could be disposed before the handler goes out of scope.</span></span>
* <span data-ttu-id="657a3-640">Usunięte usługi obsługi powodują niepowodzenie procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="657a3-640">The disposed handler services causes the handler to fail.</span></span>

<span data-ttu-id="657a3-641">Po zarejestrowaniu <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> można wywołać, przekazując w typie procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="657a3-641">Once registered, <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.AddHttpMessageHandler*> can be called, passing in the handler type.</span></span>

<span data-ttu-id="657a3-642">Wiele programów obsługi można zarejestrować w kolejności, w jakiej powinny być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="657a3-642">Multiple handlers can be registered in the order that they should execute.</span></span> <span data-ttu-id="657a3-643">Każdy program obsługi otacza następną procedurę obsługi do momentu zakończenia `HttpClientHandler` żądania:</span><span class="sxs-lookup"><span data-stu-id="657a3-643">Each handler wraps the next handler until the final `HttpClientHandler` executes the request:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet6)]

<span data-ttu-id="657a3-644">Użyj jednego z następujących metod, aby udostępnić stan dla żądania za pomocą programów obsługi komunikatów:</span><span class="sxs-lookup"><span data-stu-id="657a3-644">Use one of the following approaches to share per-request state with message handlers:</span></span>

* <span data-ttu-id="657a3-645">Przekaż dane do procedury obsługi przy użyciu polecenia `HttpRequestMessage.Properties` .</span><span class="sxs-lookup"><span data-stu-id="657a3-645">Pass data into the handler using `HttpRequestMessage.Properties`.</span></span>
* <span data-ttu-id="657a3-646">Użyj, `IHttpContextAccessor` Aby uzyskać dostęp do bieżącego żądania.</span><span class="sxs-lookup"><span data-stu-id="657a3-646">Use `IHttpContextAccessor` to access the current request.</span></span>
* <span data-ttu-id="657a3-647">Utwórz niestandardowy `AsyncLocal` obiekt magazynu, aby przekazać dane.</span><span class="sxs-lookup"><span data-stu-id="657a3-647">Create a custom `AsyncLocal` storage object to pass the data.</span></span>

## <a name="use-polly-based-handlers"></a><span data-ttu-id="657a3-648">Korzystanie z programów obsługi opartych na Polly</span><span class="sxs-lookup"><span data-stu-id="657a3-648">Use Polly-based handlers</span></span>

<span data-ttu-id="657a3-649">`IHttpClientFactory` integruje się z popularną biblioteką innej firmy o nazwie [Polly](https://github.com/App-vNext/Polly).</span><span class="sxs-lookup"><span data-stu-id="657a3-649">`IHttpClientFactory` integrates with a popular third-party library called [Polly](https://github.com/App-vNext/Polly).</span></span> <span data-ttu-id="657a3-650">Polly to kompleksowa i przejściowa Biblioteka obsługi błędów dla platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="657a3-650">Polly is a comprehensive resilience and transient fault-handling library for .NET.</span></span> <span data-ttu-id="657a3-651">Pozwala to deweloperom na tworzenie zasad, takich jak ponawianie próby, wyłączniki, przekroczenie limitu czasu, izolacja grodziowa i rezerwa w sposób bezpieczny dla bezpieczeństwa i bezpiecznego wątkowo.</span><span class="sxs-lookup"><span data-stu-id="657a3-651">It allows developers to express policies such as Retry, Circuit Breaker, Timeout, Bulkhead Isolation, and Fallback in a fluent and thread-safe manner.</span></span>

<span data-ttu-id="657a3-652">Dostarczone metody rozszerzające umożliwiają korzystanie z zasad Pollyymi ze skonfigurowanymi `HttpClient` wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="657a3-652">Extension methods are provided to enable the use of Polly policies with configured `HttpClient` instances.</span></span> <span data-ttu-id="657a3-653">Rozszerzenia Polly:</span><span class="sxs-lookup"><span data-stu-id="657a3-653">The Polly extensions:</span></span>

* <span data-ttu-id="657a3-654">Obsługa dodawania programów obsługi opartych na Pollych do klientów.</span><span class="sxs-lookup"><span data-stu-id="657a3-654">Support adding Polly-based handlers to clients.</span></span>
* <span data-ttu-id="657a3-655">Można go użyć po zainstalowaniu pakietu NuGet [Microsoft. Extensions. http. Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) .</span><span class="sxs-lookup"><span data-stu-id="657a3-655">Can be used after installing the [Microsoft.Extensions.Http.Polly](https://www.nuget.org/packages/Microsoft.Extensions.Http.Polly/) NuGet package.</span></span> <span data-ttu-id="657a3-656">Pakiet nie znajduje się w ASP.NET Core udostępnionej platformy.</span><span class="sxs-lookup"><span data-stu-id="657a3-656">The package isn't included in the ASP.NET Core shared framework.</span></span>

### <a name="handle-transient-faults"></a><span data-ttu-id="657a3-657">Obsługa błędów przejściowych</span><span class="sxs-lookup"><span data-stu-id="657a3-657">Handle transient faults</span></span>

<span data-ttu-id="657a3-658">Najczęstsze błędy występują, gdy zewnętrzne wywołania HTTP są przejściowe.</span><span class="sxs-lookup"><span data-stu-id="657a3-658">Most common faults occur when external HTTP calls are transient.</span></span> <span data-ttu-id="657a3-659">Dołączono wygodną metodę rozszerzenia `AddTransientHttpErrorPolicy` , która umożliwia zdefiniowanie zasad w celu obsługi błędów przejściowych.</span><span class="sxs-lookup"><span data-stu-id="657a3-659">A convenient extension method called `AddTransientHttpErrorPolicy` is included which allows a policy to be defined to handle transient errors.</span></span> <span data-ttu-id="657a3-660">Zasady skonfigurowane przy użyciu tego uchwytu metody rozszerzenia `HttpRequestException` , odpowiedzi HTTP 5xx i odpowiedzi http 408.</span><span class="sxs-lookup"><span data-stu-id="657a3-660">Policies configured with this extension method handle `HttpRequestException`, HTTP 5xx responses, and HTTP 408 responses.</span></span>

<span data-ttu-id="657a3-661">`AddTransientHttpErrorPolicy`Rozszerzenie może być używane w `Startup.ConfigureServices` .</span><span class="sxs-lookup"><span data-stu-id="657a3-661">The `AddTransientHttpErrorPolicy` extension can be used within `Startup.ConfigureServices`.</span></span> <span data-ttu-id="657a3-662">Rozszerzenie zapewnia dostęp do `PolicyBuilder` obiektu skonfigurowanego do obsługi błędów reprezentujących możliwy błąd przejściowy:</span><span class="sxs-lookup"><span data-stu-id="657a3-662">The extension provides access to a `PolicyBuilder` object configured to handle errors representing a possible transient fault:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet7)]

<span data-ttu-id="657a3-663">W poprzednim kodzie `WaitAndRetryAsync` zdefiniowane są zasady.</span><span class="sxs-lookup"><span data-stu-id="657a3-663">In the preceding code, a `WaitAndRetryAsync` policy is defined.</span></span> <span data-ttu-id="657a3-664">Żądania zakończone niepowodzeniem są ponawiane do trzech razy z opóźnieniem 600 MS między próbami.</span><span class="sxs-lookup"><span data-stu-id="657a3-664">Failed requests are retried up to three times with a delay of 600 ms between attempts.</span></span>

### <a name="dynamically-select-policies"></a><span data-ttu-id="657a3-665">Dynamiczne Wybieranie zasad</span><span class="sxs-lookup"><span data-stu-id="657a3-665">Dynamically select policies</span></span>

<span data-ttu-id="657a3-666">Istnieją dodatkowe metody rozszerzające, których można użyć do dodawania programów obsługi opartych na Polly.</span><span class="sxs-lookup"><span data-stu-id="657a3-666">Additional extension methods exist which can be used to add Polly-based handlers.</span></span> <span data-ttu-id="657a3-667">Jedno takie rozszerzenie ma `AddPolicyHandler` wiele przeciążeń.</span><span class="sxs-lookup"><span data-stu-id="657a3-667">One such extension is `AddPolicyHandler`, which has multiple overloads.</span></span> <span data-ttu-id="657a3-668">Jedno Przeciążenie umożliwia sprawdzenie żądania podczas definiowania zasad, które mają zostać zastosowane:</span><span class="sxs-lookup"><span data-stu-id="657a3-668">One overload allows the request to be inspected when defining which policy to apply:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet8)]

<span data-ttu-id="657a3-669">W poprzednim kodzie, jeśli żądanie wychodzące jest GET HTTP, zostanie zastosowany 10-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="657a3-669">In the preceding code, if the outgoing request is an HTTP GET, a 10-second timeout is applied.</span></span> <span data-ttu-id="657a3-670">Dla każdej innej metody HTTP jest używany 30-sekundowy limit czasu.</span><span class="sxs-lookup"><span data-stu-id="657a3-670">For any other HTTP method, a 30-second timeout is used.</span></span>

### <a name="add-multiple-polly-handlers"></a><span data-ttu-id="657a3-671">Dodaj wiele programów obsługi Polly</span><span class="sxs-lookup"><span data-stu-id="657a3-671">Add multiple Polly handlers</span></span>

<span data-ttu-id="657a3-672">Jest to typowy sposób zagnieżdżania zasad Pollyymi w celu zapewnienia zwiększonej funkcjonalności:</span><span class="sxs-lookup"><span data-stu-id="657a3-672">It's common to nest Polly policies to provide enhanced functionality:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet9)]

<span data-ttu-id="657a3-673">W poprzednim przykładzie dodawane są dwa programy obsługi.</span><span class="sxs-lookup"><span data-stu-id="657a3-673">In the preceding example, two handlers are added.</span></span> <span data-ttu-id="657a3-674">Pierwsze używa rozszerzenia, `AddTransientHttpErrorPolicy` Aby dodać zasady ponawiania.</span><span class="sxs-lookup"><span data-stu-id="657a3-674">The first uses the `AddTransientHttpErrorPolicy` extension to add a retry policy.</span></span> <span data-ttu-id="657a3-675">Nieudane żądania są ponawiane do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="657a3-675">Failed requests are retried up to three times.</span></span> <span data-ttu-id="657a3-676">Drugie wywołanie `AddTransientHttpErrorPolicy` dodaje zasady wyłącznika.</span><span class="sxs-lookup"><span data-stu-id="657a3-676">The second call to `AddTransientHttpErrorPolicy` adds a circuit breaker policy.</span></span> <span data-ttu-id="657a3-677">Dalsze żądania zewnętrzne są blokowane przez 30 sekund, jeśli pięć nieudanych prób wystąpi sekwencyjnie.</span><span class="sxs-lookup"><span data-stu-id="657a3-677">Further external requests are blocked for 30 seconds if five failed attempts occur sequentially.</span></span> <span data-ttu-id="657a3-678">Zasady wyłącznika są stanowe.</span><span class="sxs-lookup"><span data-stu-id="657a3-678">Circuit breaker policies are stateful.</span></span> <span data-ttu-id="657a3-679">Wszystkie wywołania przez tego klienta współdzielą ten sam stan obwodu.</span><span class="sxs-lookup"><span data-stu-id="657a3-679">All calls through this client share the same circuit state.</span></span>

### <a name="add-policies-from-the-polly-registry"></a><span data-ttu-id="657a3-680">Dodawanie zasad z rejestru Polly</span><span class="sxs-lookup"><span data-stu-id="657a3-680">Add policies from the Polly registry</span></span>

<span data-ttu-id="657a3-681">Podejście do zarządzania regularnie używanymi zasadami polega na ich definiowaniu i zarejestrowaniu za pomocą `PolicyRegistry` .</span><span class="sxs-lookup"><span data-stu-id="657a3-681">An approach to managing regularly used policies is to define them once and register them with a `PolicyRegistry`.</span></span> <span data-ttu-id="657a3-682">Zapewniana jest metoda rozszerzająca, która umożliwia dodanie programu obsługi przy użyciu zasad z rejestru:</span><span class="sxs-lookup"><span data-stu-id="657a3-682">An extension method is provided which allows a handler to be added using a policy from the registry:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet10)]

<span data-ttu-id="657a3-683">W poprzednim kodzie dwie zasady są rejestrowane po `PolicyRegistry` dodaniu do `ServiceCollection` .</span><span class="sxs-lookup"><span data-stu-id="657a3-683">In the preceding code, two policies are registered when the `PolicyRegistry` is added to the `ServiceCollection`.</span></span> <span data-ttu-id="657a3-684">Aby użyć zasad z rejestru, `AddPolicyHandlerFromRegistry` Metoda jest używana, przekazując nazwę zasad do zastosowania.</span><span class="sxs-lookup"><span data-stu-id="657a3-684">To use a policy from the registry, the `AddPolicyHandlerFromRegistry` method is used, passing the name of the policy to apply.</span></span>

<span data-ttu-id="657a3-685">Więcej informacji na temat integracji z programem `IHttpClientFactory` i Polly można znaleźć w witrynie [typu wiki Polly](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span><span class="sxs-lookup"><span data-stu-id="657a3-685">Further information about `IHttpClientFactory` and Polly integrations can be found on the [Polly wiki](https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory).</span></span>

## <a name="httpclient-and-lifetime-management"></a><span data-ttu-id="657a3-686">HttpClient i zarządzanie okresem istnienia</span><span class="sxs-lookup"><span data-stu-id="657a3-686">HttpClient and lifetime management</span></span>

<span data-ttu-id="657a3-687">Nowe `HttpClient` wystąpienie jest zwracane za każdym razem `CreateClient` , gdy jest wywoływana w `IHttpClientFactory` .</span><span class="sxs-lookup"><span data-stu-id="657a3-687">A new `HttpClient` instance is returned each time `CreateClient` is called on the `IHttpClientFactory`.</span></span> <span data-ttu-id="657a3-688">Istnieje <xref:System.Net.Http.HttpMessageHandler> za nazwany klient.</span><span class="sxs-lookup"><span data-stu-id="657a3-688">There's an <xref:System.Net.Http.HttpMessageHandler> per named client.</span></span> <span data-ttu-id="657a3-689">Fabryka zarządza okresami istnienia `HttpMessageHandler` wystąpień.</span><span class="sxs-lookup"><span data-stu-id="657a3-689">The factory manages the lifetimes of the `HttpMessageHandler` instances.</span></span>

<span data-ttu-id="657a3-690">`IHttpClientFactory` pule `HttpMessageHandler` wystąpień tworzonych przez fabrykę w celu zmniejszenia zużycia zasobów.</span><span class="sxs-lookup"><span data-stu-id="657a3-690">`IHttpClientFactory` pools the `HttpMessageHandler` instances created by the factory to reduce resource consumption.</span></span> <span data-ttu-id="657a3-691">`HttpMessageHandler`Wystąpienie może być ponownie używane z puli podczas tworzenia nowego `HttpClient` wystąpienia, jeśli jego okres istnienia nie wygasł.</span><span class="sxs-lookup"><span data-stu-id="657a3-691">An `HttpMessageHandler` instance may be reused from the pool when creating a new `HttpClient` instance if its lifetime hasn't expired.</span></span>

<span data-ttu-id="657a3-692">Buforowanie programów obsługi jest pożądane, ponieważ każdy program obsługi zazwyczaj zarządza własnymi połączeniami HTTP.</span><span class="sxs-lookup"><span data-stu-id="657a3-692">Pooling of handlers is desirable as each handler typically manages its own underlying HTTP connections.</span></span> <span data-ttu-id="657a3-693">Utworzenie większej liczby programów obsługi niż to konieczne może skutkować opóźnieniami połączeń.</span><span class="sxs-lookup"><span data-stu-id="657a3-693">Creating more handlers than necessary can result in connection delays.</span></span> <span data-ttu-id="657a3-694">Niektóre programy obsługi powodują również, że połączenia są otwarte w nieskończoność, co może uniemożliwić obsłużenie zmian DNS przez program obsługi.</span><span class="sxs-lookup"><span data-stu-id="657a3-694">Some handlers also keep connections open indefinitely, which can prevent the handler from reacting to DNS changes.</span></span>

<span data-ttu-id="657a3-695">Domyślny okres istnienia programu obsługi wynosi dwie minuty.</span><span class="sxs-lookup"><span data-stu-id="657a3-695">The default handler lifetime is two minutes.</span></span> <span data-ttu-id="657a3-696">Wartość domyślną można przesłonić na podstawie nazwy klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-696">The default value can be overridden on a per named client basis.</span></span> <span data-ttu-id="657a3-697">Aby zastąpić ten element, wywołaj metodę <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> `IHttpClientBuilder` zwracaną podczas tworzenia klienta:</span><span class="sxs-lookup"><span data-stu-id="657a3-697">To override it, call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.SetHandlerLifetime*> on the `IHttpClientBuilder` that is returned when creating the client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet11)]

<span data-ttu-id="657a3-698">Usuwanie klienta nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="657a3-698">Disposal of the client isn't required.</span></span> <span data-ttu-id="657a3-699">Likwidacja anuluje żądania wychodzące i gwarantuje, że danego `HttpClient` wystąpienia nie można użyć po wywołaniu <xref:System.IDisposable.Dispose*> .</span><span class="sxs-lookup"><span data-stu-id="657a3-699">Disposal cancels outgoing requests and guarantees the given `HttpClient` instance can't be used after calling <xref:System.IDisposable.Dispose*>.</span></span> <span data-ttu-id="657a3-700">`IHttpClientFactory` śledzi i usuwa zasoby używane przez `HttpClient` wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="657a3-700">`IHttpClientFactory` tracks and disposes resources used by `HttpClient` instances.</span></span> <span data-ttu-id="657a3-701">`HttpClient`Wystąpienia mogą być zwykle traktowane jako obiekty .NET, które nie wymagają usuwania.</span><span class="sxs-lookup"><span data-stu-id="657a3-701">The `HttpClient` instances can generally be treated as .NET objects not requiring disposal.</span></span>

<span data-ttu-id="657a3-702">Utrzymywanie pojedynczego `HttpClient` wystąpienia aktywności przez długi czas jest typowym wzorcem używanym przed rozpoczęciem `IHttpClientFactory` .</span><span class="sxs-lookup"><span data-stu-id="657a3-702">Keeping a single `HttpClient` instance alive for a long duration is a common pattern used before the inception of `IHttpClientFactory`.</span></span> <span data-ttu-id="657a3-703">Ten wzorzec będzie niepotrzebny po migracji do programu `IHttpClientFactory` .</span><span class="sxs-lookup"><span data-stu-id="657a3-703">This pattern becomes unnecessary after migrating to `IHttpClientFactory`.</span></span>

### <a name="alternatives-to-ihttpclientfactory"></a><span data-ttu-id="657a3-704">Alternatywy do IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="657a3-704">Alternatives to IHttpClientFactory</span></span>

<span data-ttu-id="657a3-705">Używanie `IHttpClientFactory` w aplikacji z obsługą podwójnego zapobiegania:</span><span class="sxs-lookup"><span data-stu-id="657a3-705">Using `IHttpClientFactory` in a DI-enabled app avoids:</span></span>

* <span data-ttu-id="657a3-706">Problemy z wyczerpaniem zasobów przez `HttpMessageHandler` wystąpienia puli.</span><span class="sxs-lookup"><span data-stu-id="657a3-706">Resource exhaustion problems by pooling `HttpMessageHandler` instances.</span></span>
* <span data-ttu-id="657a3-707">Nieodświeżone problemy z usługą DNS przez cykliczne `HttpMessageHandler` wystąpienia w regularnych odstępach czasu.</span><span class="sxs-lookup"><span data-stu-id="657a3-707">Stale DNS problems by cycling `HttpMessageHandler` instances at regular intervals.</span></span>

<span data-ttu-id="657a3-708">Istnieją alternatywne sposoby rozwiązywania powyższych problemów przy użyciu wystąpienia długotrwałego <xref:System.Net.Http.SocketsHttpHandler> .</span><span class="sxs-lookup"><span data-stu-id="657a3-708">There are alternative ways to solve the preceding problems using a long-lived <xref:System.Net.Http.SocketsHttpHandler> instance.</span></span>

- <span data-ttu-id="657a3-709">Utwórz wystąpienie, `SocketsHttpHandler` gdy aplikacja zostanie uruchomiona i użyje jej na potrzeby życia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="657a3-709">Create an instance of `SocketsHttpHandler` when the app starts and use it for the life of the app.</span></span>
- <span data-ttu-id="657a3-710">Skonfiguruj <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> odpowiednią wartość na podstawie czasu odświeżania DNS.</span><span class="sxs-lookup"><span data-stu-id="657a3-710">Configure <xref:System.Net.Http.SocketsHttpHandler.PooledConnectionLifetime> to an appropriate value based on DNS refresh times.</span></span>
- <span data-ttu-id="657a3-711">Utwórz `HttpClient` wystąpienia przy użyciu programu `new HttpClient(handler, disposeHandler: false)` zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="657a3-711">Create `HttpClient` instances using `new HttpClient(handler, disposeHandler: false)` as needed.</span></span>

<span data-ttu-id="657a3-712">Powyższe podejścia rozwiązują problemy z zarządzaniem zasobami, które `IHttpClientFactory` rozwiązują się w podobny sposób.</span><span class="sxs-lookup"><span data-stu-id="657a3-712">The preceding approaches solve the resource management problems that `IHttpClientFactory` solves in a similar way.</span></span>

- <span data-ttu-id="657a3-713">`SocketsHttpHandler`Udostępnia połączenia między `HttpClient` wystąpieniami.</span><span class="sxs-lookup"><span data-stu-id="657a3-713">The `SocketsHttpHandler` shares connections across `HttpClient` instances.</span></span> <span data-ttu-id="657a3-714">To udostępnianie uniemożliwia wyczerpanie gniazda.</span><span class="sxs-lookup"><span data-stu-id="657a3-714">This sharing prevents socket exhaustion.</span></span>
- <span data-ttu-id="657a3-715">`SocketsHttpHandler`Cykle połączeń zgodnie z, aby `PooledConnectionLifetime` uniknąć nieodświeżonych problemów z usługą DNS.</span><span class="sxs-lookup"><span data-stu-id="657a3-715">The `SocketsHttpHandler` cycles connections according to `PooledConnectionLifetime` to avoid stale DNS problems.</span></span>

### <a name="cookies"></a><span data-ttu-id="657a3-716">Cookiewolumin</span><span class="sxs-lookup"><span data-stu-id="657a3-716">Cookies</span></span>

<span data-ttu-id="657a3-717">`HttpMessageHandler`Wystąpienia w puli powoduje, że `CookieContainer` obiekty są udostępniane.</span><span class="sxs-lookup"><span data-stu-id="657a3-717">The pooled `HttpMessageHandler` instances results in `CookieContainer` objects being shared.</span></span> <span data-ttu-id="657a3-718">Nieoczekiwane `CookieContainer` udostępnianie obiektów często prowadzi do niepoprawnego kodu.</span><span class="sxs-lookup"><span data-stu-id="657a3-718">Unanticipated `CookieContainer` object sharing often results in incorrect code.</span></span> <span data-ttu-id="657a3-719">W przypadku aplikacji, które wymagają usługi cookie s, należy rozważyć następujące kwestie:</span><span class="sxs-lookup"><span data-stu-id="657a3-719">For apps that require cookies, consider either:</span></span>

 - <span data-ttu-id="657a3-720">Wyłączanie cookie obsługi automatycznej</span><span class="sxs-lookup"><span data-stu-id="657a3-720">Disabling automatic cookie handling</span></span>
 - <span data-ttu-id="657a3-721">Unikanie `IHttpClientFactory`</span><span class="sxs-lookup"><span data-stu-id="657a3-721">Avoiding `IHttpClientFactory`</span></span>

<span data-ttu-id="657a3-722">Wywołanie <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> wyłączania automatycznej cookie obsługi:</span><span class="sxs-lookup"><span data-stu-id="657a3-722">Call <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> to disable automatic cookie handling:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet13)]

## <a name="logging"></a><span data-ttu-id="657a3-723">Rejestrowanie</span><span class="sxs-lookup"><span data-stu-id="657a3-723">Logging</span></span>

<span data-ttu-id="657a3-724">Klienci utworzeni za pomocą `IHttpClientFactory` rejestrowania komunikatów dzienników dla wszystkich żądań.</span><span class="sxs-lookup"><span data-stu-id="657a3-724">Clients created via `IHttpClientFactory` record log messages for all requests.</span></span> <span data-ttu-id="657a3-725">Włącz odpowiedni poziom informacji w konfiguracji rejestrowania, aby wyświetlić domyślne komunikaty dziennika.</span><span class="sxs-lookup"><span data-stu-id="657a3-725">Enable the appropriate information level in your logging configuration to see the default log messages.</span></span> <span data-ttu-id="657a3-726">Dodatkowe rejestrowanie, takie jak rejestrowanie nagłówków żądań, jest uwzględniane tylko na poziomie śledzenia.</span><span class="sxs-lookup"><span data-stu-id="657a3-726">Additional logging, such as the logging of request headers, is only included at trace level.</span></span>

<span data-ttu-id="657a3-727">Kategoria dziennika używana dla każdego klienta zawiera nazwę klienta programu.</span><span class="sxs-lookup"><span data-stu-id="657a3-727">The log category used for each client includes the name of the client.</span></span> <span data-ttu-id="657a3-728">Klient o nazwie *MyNamedClient*, na przykład rejestruje komunikaty z kategorią `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler` .</span><span class="sxs-lookup"><span data-stu-id="657a3-728">A client named *MyNamedClient*, for example, logs messages with a category of `System.Net.Http.HttpClient.MyNamedClient.LogicalHandler`.</span></span> <span data-ttu-id="657a3-729">Komunikaty z sufiksem *LogicalHandler* występują poza potokiem obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="657a3-729">Messages suffixed with *LogicalHandler* occur outside the request handler pipeline.</span></span> <span data-ttu-id="657a3-730">Na żądanie komunikaty są rejestrowane przed przetworzeniem przez inne procedury obsługi w potoku.</span><span class="sxs-lookup"><span data-stu-id="657a3-730">On the request, messages are logged before any other handlers in the pipeline have processed it.</span></span> <span data-ttu-id="657a3-731">W odpowiedzi komunikaty są rejestrowane po odebraniu odpowiedzi przez inne programy obsługi potoków.</span><span class="sxs-lookup"><span data-stu-id="657a3-731">On the response, messages are logged after any other pipeline handlers have received the response.</span></span>

<span data-ttu-id="657a3-732">Rejestrowanie odbywa się również w potoku obsługi żądania.</span><span class="sxs-lookup"><span data-stu-id="657a3-732">Logging also occurs inside the request handler pipeline.</span></span> <span data-ttu-id="657a3-733">W przykładzie *MyNamedClient* te komunikaty są rejestrowane w kategorii dzienników `System.Net.Http.HttpClient.MyNamedClient.ClientHandler` .</span><span class="sxs-lookup"><span data-stu-id="657a3-733">In the *MyNamedClient* example, those messages are logged against the log category `System.Net.Http.HttpClient.MyNamedClient.ClientHandler`.</span></span> <span data-ttu-id="657a3-734">W przypadku żądania dzieje się tak po uruchomieniu wszystkich innych programów obsługi i natychmiast przed wysłaniem żądania w sieci.</span><span class="sxs-lookup"><span data-stu-id="657a3-734">For the request, this occurs after all other handlers have run and immediately before the request is sent out on the network.</span></span> <span data-ttu-id="657a3-735">W odpowiedzi to rejestrowanie obejmuje stan odpowiedzi, zanim przejdzie do powrotem za pomocą potoku programu obsługi.</span><span class="sxs-lookup"><span data-stu-id="657a3-735">On the response, this logging includes the state of the response before it passes back through the handler pipeline.</span></span>

<span data-ttu-id="657a3-736">Włączenie rejestrowania poza i wewnątrz potoku umożliwia inspekcję zmian wprowadzonych przez inne programy obsługi potoku.</span><span class="sxs-lookup"><span data-stu-id="657a3-736">Enabling logging outside and inside the pipeline enables inspection of the changes made by the other pipeline handlers.</span></span> <span data-ttu-id="657a3-737">Może to obejmować zmiany nagłówków żądań, na przykład lub do kodu stanu odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="657a3-737">This may include changes to request headers, for example, or to the response status code.</span></span>

<span data-ttu-id="657a3-738">Dołączenie nazwy klienta w kategorii dziennika umożliwia filtrowanie dzienników dla określonych nazwanych klientów, jeśli jest to konieczne.</span><span class="sxs-lookup"><span data-stu-id="657a3-738">Including the name of the client in the log category enables log filtering for specific named clients where necessary.</span></span>

## <a name="configure-the-httpmessagehandler"></a><span data-ttu-id="657a3-739">Konfigurowanie HttpMessageHandler</span><span class="sxs-lookup"><span data-stu-id="657a3-739">Configure the HttpMessageHandler</span></span>

<span data-ttu-id="657a3-740">Może być konieczne sterowanie konfiguracją wewnętrznej `HttpMessageHandler` używanej przez klienta.</span><span class="sxs-lookup"><span data-stu-id="657a3-740">It may be necessary to control the configuration of the inner `HttpMessageHandler` used by a client.</span></span>

<span data-ttu-id="657a3-741">`IHttpClientBuilder`Jest zwracany podczas dodawania nazwanych lub wpisanych klientów.</span><span class="sxs-lookup"><span data-stu-id="657a3-741">An `IHttpClientBuilder` is returned when adding named or typed clients.</span></span> <span data-ttu-id="657a3-742"><xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*>Metoda rozszerzająca może służyć do definiowania delegata.</span><span class="sxs-lookup"><span data-stu-id="657a3-742">The <xref:Microsoft.Extensions.DependencyInjection.HttpClientBuilderExtensions.ConfigurePrimaryHttpMessageHandler*> extension method can be used to define a delegate.</span></span> <span data-ttu-id="657a3-743">Delegat służy do tworzenia i konfigurowania głównej `HttpMessageHandler` używanej przez tego klienta:</span><span class="sxs-lookup"><span data-stu-id="657a3-743">The delegate is used to create and configure the primary `HttpMessageHandler` used by that client:</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactorySample/Startup.cs?name=snippet12)]

## <a name="use-ihttpclientfactory-in-a-console-app"></a><span data-ttu-id="657a3-744">Korzystanie z IHttpClientFactory w aplikacji konsolowej</span><span class="sxs-lookup"><span data-stu-id="657a3-744">Use IHttpClientFactory in a console app</span></span>

<span data-ttu-id="657a3-745">W aplikacji konsoli Dodaj następujące odwołania do pakietu do projektu:</span><span class="sxs-lookup"><span data-stu-id="657a3-745">In a console app, add the following package references to the project:</span></span>

* [<span data-ttu-id="657a3-746">Microsoft. Extensions. hosting</span><span class="sxs-lookup"><span data-stu-id="657a3-746">Microsoft.Extensions.Hosting</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Hosting)
* [<span data-ttu-id="657a3-747">Microsoft. Extensions. http</span><span class="sxs-lookup"><span data-stu-id="657a3-747">Microsoft.Extensions.Http</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Http)

<span data-ttu-id="657a3-748">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="657a3-748">In the following example:</span></span>

* <span data-ttu-id="657a3-749"><xref:System.Net.Http.IHttpClientFactory> jest zarejestrowany w kontenerze usługi [hosta ogólnego](xref:fundamentals/host/generic-host) .</span><span class="sxs-lookup"><span data-stu-id="657a3-749"><xref:System.Net.Http.IHttpClientFactory> is registered in the [Generic Host's](xref:fundamentals/host/generic-host) service container.</span></span>
* <span data-ttu-id="657a3-750">`MyService` tworzy wystąpienie fabryki klienta na podstawie usługi, która jest używana do tworzenia `HttpClient` .</span><span class="sxs-lookup"><span data-stu-id="657a3-750">`MyService` creates a client factory instance from the service, which is used to create an `HttpClient`.</span></span> <span data-ttu-id="657a3-751">`HttpClient` służy do pobierania strony sieci Web.</span><span class="sxs-lookup"><span data-stu-id="657a3-751">`HttpClient` is used to retrieve a webpage.</span></span>
* <span data-ttu-id="657a3-752">`Main` tworzy zakres do wykonania `GetPage` metody usługi i zapisuje pierwsze 500 znaków zawartości strony sieci Web w konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="657a3-752">`Main` creates a scope to execute the service's `GetPage` method and write the first 500 characters of the webpage content to the console.</span></span>

[!code-csharp[](http-requests/samples/2.x/HttpClientFactoryConsoleSample/Program.cs?highlight=14-15,20,26-27,59-62)]

## <a name="header-propagation-middleware"></a><span data-ttu-id="657a3-753">Oprogramowanie pośredniczące propagacji nagłówka</span><span class="sxs-lookup"><span data-stu-id="657a3-753">Header propagation middleware</span></span>

<span data-ttu-id="657a3-754">Propagacja nagłówka to społeczność obsługiwana przez oprogramowanie pośredniczące do propagowania nagłówków HTTP z przychodzącego żądania do wychodzących żądań klienta HTTP.</span><span class="sxs-lookup"><span data-stu-id="657a3-754">Header propagation is a community supported middleware to propagate HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> <span data-ttu-id="657a3-755">Aby użyć propagacji nagłówka:</span><span class="sxs-lookup"><span data-stu-id="657a3-755">To use header propagation:</span></span>

* <span data-ttu-id="657a3-756">Odwołuje się do obsługiwanego przez społeczność portu [HeaderPropagation](https://www.nuget.org/packages/HeaderPropagation)pakietu.</span><span class="sxs-lookup"><span data-stu-id="657a3-756">Reference the community supported port of the package [HeaderPropagation](https://www.nuget.org/packages/HeaderPropagation).</span></span> <span data-ttu-id="657a3-757">ASP.NET Core 3,1 i nowsze obsługuje [Microsoft. AspNetCore. HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation).</span><span class="sxs-lookup"><span data-stu-id="657a3-757">ASP.NET Core 3.1 and later supports [Microsoft.AspNetCore.HeaderPropagation](https://www.nuget.org/packages/Microsoft.AspNetCore.HeaderPropagation).</span></span>

* <span data-ttu-id="657a3-758">Skonfiguruj oprogramowanie pośredniczące i `HttpClient` w programie `Startup` :</span><span class="sxs-lookup"><span data-stu-id="657a3-758">Configure the middleware and `HttpClient` in `Startup`:</span></span>

  [!code-csharp[](http-requests/samples/2.x/Startup21.cs?highlight=5-9,25&name=snippet)]

* <span data-ttu-id="657a3-759">Klient zawiera skonfigurowane nagłówki w żądaniach wychodzących:</span><span class="sxs-lookup"><span data-stu-id="657a3-759">The client includes the configured headers on outbound requests:</span></span>

  ```csharp
  var client = clientFactory.CreateClient("MyForwardingClient");
  var response = client.GetAsync(...);
  ```

## <a name="additional-resources"></a><span data-ttu-id="657a3-760">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="657a3-760">Additional resources</span></span>

* [<span data-ttu-id="657a3-761">Używanie elementu HttpClientFactory do implementowania odpornych na błędy żądań HTTP</span><span class="sxs-lookup"><span data-stu-id="657a3-761">Use HttpClientFactory to implement resilient HTTP requests</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/use-httpclientfactory-to-implement-resilient-http-requests)
* [<span data-ttu-id="657a3-762">Zaimplementuj ponowne próby wywołania HTTP przy użyciu wykładniczej wycofywania z zasadami HttpClientFactory i Polly</span><span class="sxs-lookup"><span data-stu-id="657a3-762">Implement HTTP call retries with exponential backoff with HttpClientFactory and Polly policies</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-http-call-retries-exponential-backoff-polly)
* [<span data-ttu-id="657a3-763">Implementowanie wzorca wyłącznika</span><span class="sxs-lookup"><span data-stu-id="657a3-763">Implement the Circuit Breaker pattern</span></span>](/dotnet/standard/microservices-architecture/implement-resilient-applications/implement-circuit-breaker-pattern)

::: moniker-end
