---
title: Hostowanie i wdrażanie ASP.NET Core Blazor WebAssembly
author: guardrex
description: Dowiedz się, jak hostować i wdrażać Blazor aplikację przy użyciu ASP.NET Core, sieci dostarczania zawartości (CDN), serwerów plików i stron usługi GitHub.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/12/2021
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
uid: blazor/host-and-deploy/webassembly
ms.openlocfilehash: bb45b763fb24b5270c92b3ffd18f3fbc3ba1093b
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102589429"
---
# <a name="host-and-deploy-aspnet-core-blazor-webassembly"></a><span data-ttu-id="40e57-103">Hostowanie i wdrażanie ASP.NET Core Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="40e57-103">Host and deploy ASP.NET Core Blazor WebAssembly</span></span>

<span data-ttu-id="40e57-104">Z [ Blazor WebAssembly modelem hostingu](xref:blazor/hosting-models#blazor-webassembly):</span><span class="sxs-lookup"><span data-stu-id="40e57-104">With the [Blazor WebAssembly hosting model](xref:blazor/hosting-models#blazor-webassembly):</span></span>

* <span data-ttu-id="40e57-105">BlazorAplikacja, jej zależności i środowisko uruchomieniowe platformy .NET są pobierane równolegle do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="40e57-105">The Blazor app, its dependencies, and the .NET runtime are downloaded to the browser in parallel.</span></span>
* <span data-ttu-id="40e57-106">Aplikacja jest wykonywana bezpośrednio w wątku interfejsu użytkownika przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="40e57-106">The app is executed directly on the browser UI thread.</span></span>

<span data-ttu-id="40e57-107">Obsługiwane są następujące strategie wdrażania:</span><span class="sxs-lookup"><span data-stu-id="40e57-107">The following deployment strategies are supported:</span></span>

* <span data-ttu-id="40e57-108">BlazorAplikacja jest obsługiwana przez aplikację ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="40e57-108">The Blazor app is served by an ASP.NET Core app.</span></span> <span data-ttu-id="40e57-109">Ta strategia jest objęta [wdrożeniem hostowanym za pomocą ASP.NET Core](#hosted-deployment-with-aspnet-core) sekcji.</span><span class="sxs-lookup"><span data-stu-id="40e57-109">This strategy is covered in the [Hosted deployment with ASP.NET Core](#hosted-deployment-with-aspnet-core) section.</span></span>
* <span data-ttu-id="40e57-110">BlazorAplikacja jest umieszczana na statycznym, hostingowym serwerze sieci Web lub usłudze, w której program .NET nie jest używany do obsługi Blazor aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-110">The Blazor app is placed on a static hosting web server or service, where .NET isn't used to serve the Blazor app.</span></span> <span data-ttu-id="40e57-111">Ta strategia została omówiona w sekcji [wdrażanie autonomiczne](#standalone-deployment) , która obejmuje informacje dotyczące hostingu Blazor WebAssembly aplikacji jako PODRZĘDNEJ aplikacji usług IIS.</span><span class="sxs-lookup"><span data-stu-id="40e57-111">This strategy is covered in the [Standalone deployment](#standalone-deployment) section, which includes information on hosting a Blazor WebAssembly app as an IIS sub-app.</span></span>

## <a name="compression"></a><span data-ttu-id="40e57-112">Kompresja</span><span class="sxs-lookup"><span data-stu-id="40e57-112">Compression</span></span>

<span data-ttu-id="40e57-113">Po Blazor WebAssembly opublikowaniu aplikacji dane wyjściowe są kompresowane statycznie podczas publikowania, aby zmniejszyć rozmiar aplikacji i usunąć obciążenie dla kompresji w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="40e57-113">When a Blazor WebAssembly app is published, the output is statically compressed during publish to reduce the app's size and remove the overhead for runtime compression.</span></span> <span data-ttu-id="40e57-114">Używane są następujące algorytmy kompresji:</span><span class="sxs-lookup"><span data-stu-id="40e57-114">The following compression algorithms are used:</span></span>

* <span data-ttu-id="40e57-115">[Brotli](https://tools.ietf.org/html/rfc7932) (najwyższy poziom)</span><span class="sxs-lookup"><span data-stu-id="40e57-115">[Brotli](https://tools.ietf.org/html/rfc7932) (highest level)</span></span>
* [<span data-ttu-id="40e57-116">Gzip</span><span class="sxs-lookup"><span data-stu-id="40e57-116">Gzip</span></span>](https://tools.ietf.org/html/rfc1952)

<span data-ttu-id="40e57-117">Blazor korzysta z hosta, aby zapewnić odpowiednie skompresowane pliki.</span><span class="sxs-lookup"><span data-stu-id="40e57-117">Blazor relies on the host to the serve the appropriate compressed files.</span></span> <span data-ttu-id="40e57-118">W przypadku korzystania z ASP.NET Core hostowanego projektu host jest w stanie wykonywać negocjacje zawartości i obsługiwać statycznie skompresowane pliki.</span><span class="sxs-lookup"><span data-stu-id="40e57-118">When using an ASP.NET Core hosted project, the host project is capable of performing content negotiation and serving the statically-compressed files.</span></span> <span data-ttu-id="40e57-119">W przypadku udostępniania Blazor WebAssembly aplikacji autonomicznej może być wymagane dodatkowe działanie, aby zapewnić obsługę plików skompresowanych statycznie:</span><span class="sxs-lookup"><span data-stu-id="40e57-119">When hosting a Blazor WebAssembly standalone app, additional work might be required to ensure that statically-compressed files are served:</span></span>

* <span data-ttu-id="40e57-120">Aby uzyskać `web.config` konfigurację kompresji usług IIS, zobacz sekcję [rekompresji usług IIS: Brotli i gzip](#brotli-and-gzip-compression) .</span><span class="sxs-lookup"><span data-stu-id="40e57-120">For IIS `web.config` compression configuration, see the [IIS: Brotli and Gzip compression](#brotli-and-gzip-compression) section.</span></span> 
* <span data-ttu-id="40e57-121">Podczas hostingu w rozwiązaniach hostingu statycznego, które nie obsługują negocjowanej statycznie negocjacji zawartości plików, na przykład stron usługi GitHub, należy rozważyć skonfigurowanie aplikacji do pobierania i dekodowania skompresowanych plików Brotli:</span><span class="sxs-lookup"><span data-stu-id="40e57-121">When hosting on static hosting solutions that don't support statically-compressed file content negotiation, such as GitHub Pages, consider configuring the app to fetch and decode Brotli compressed files:</span></span>

  * <span data-ttu-id="40e57-122">Uzyskaj dekoder JavaScript Brotli z repozytorium usługi [GitHub firmy Google/Brotli](https://github.com/google/brotli).</span><span class="sxs-lookup"><span data-stu-id="40e57-122">Obtain the JavaScript Brotli decoder from the [google/brotli GitHub repository](https://github.com/google/brotli).</span></span> <span data-ttu-id="40e57-123">Plik dekodera ma nazwę `decode.js` i znajduje się w [ `js` folderze](https://github.com/google/brotli/tree/master/js)repozytorium.</span><span class="sxs-lookup"><span data-stu-id="40e57-123">The decoder file is named `decode.js` and found in the repository's [`js` folder](https://github.com/google/brotli/tree/master/js).</span></span>
  
    > [!NOTE]
    > <span data-ttu-id="40e57-124">Regresja jest obecna w wersji zminimalizowanego `decode.js` skryptu ( `decode.min.js` ) w [repozytorium usługi GitHub Google/brotli](https://github.com/google/brotli).</span><span class="sxs-lookup"><span data-stu-id="40e57-124">A regression is present in the minified version of the `decode.js` script (`decode.min.js`) in the [google/brotli GitHub repository](https://github.com/google/brotli).</span></span> <span data-ttu-id="40e57-125">Do momentu rozwiązania problemu [TypeError w decode.min.js (Google/brotli #881)](https://github.com/google/brotli/issues/881) należy zastosować jedną z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="40e57-125">Until the issue [TypeError in decode.min.js (google/brotli #881)](https://github.com/google/brotli/issues/881) is resolved, take one of the following approaches:</span></span>
    >
    > * <span data-ttu-id="40e57-126">Tymczasowo Użyj unminified wersji skryptu.</span><span class="sxs-lookup"><span data-stu-id="40e57-126">Temporarily use the unminified version of the script.</span></span>
    > * <span data-ttu-id="40e57-127">Automatycznie zminifikować skrypt w czasie kompilacji za pomocą narzędzia minifikacja innej firmy, zgodnego z ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="40e57-127">Automatically minify the script at build-time with a third-party minification tool compatible with ASP.NET Core.</span></span>
    > * <span data-ttu-id="40e57-128">Użyj [pakietu npm](https://www.npmjs.com/package/brotli).</span><span class="sxs-lookup"><span data-stu-id="40e57-128">Use the [npm package](https://www.npmjs.com/package/brotli).</span></span>
    >
    > <span data-ttu-id="40e57-129">Przykładowy kod w tej sekcji używa wersji **unminified** skryptu ( `decode.js` ).</span><span class="sxs-lookup"><span data-stu-id="40e57-129">The example code in this section uses the **unminified** version of the script (`decode.js`).</span></span>

  * <span data-ttu-id="40e57-130">Zaktualizuj aplikację, aby użyć dekodera.</span><span class="sxs-lookup"><span data-stu-id="40e57-130">Update the app to use the decoder.</span></span> <span data-ttu-id="40e57-131">Zmień adiustację wewnątrz tagu zamykającego `<body>` w `wwwroot/index.html` następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="40e57-131">Change the markup inside the closing `<body>` tag in `wwwroot/index.html` to the following:</span></span>
  
    ```html
    <script src="decode.js"></script>
    <script src="_framework/blazor.webassembly.js" autostart="false"></script>
    <script>
      Blazor.start({
        loadBootResource: function (type, name, defaultUri, integrity) {
          if (type !== 'dotnetjs' && location.hostname !== 'localhost') {
            return (async function () {
              const response = await fetch(defaultUri + '.br', { cache: 'no-cache' });
              if (!response.ok) {
                throw new Error(response.statusText);
              }
              const originalResponseBuffer = await response.arrayBuffer();
              const originalResponseArray = new Int8Array(originalResponseBuffer);
              const decompressedResponseArray = BrotliDecode(originalResponseArray);
              const contentType = type === 
                'dotnetwasm' ? 'application/wasm' : 'application/octet-stream';
              return new Response(decompressedResponseArray, 
                { headers: { 'content-type': contentType } });
            })();
          }
        }
      });
    </script>
    ```
 
<span data-ttu-id="40e57-132">Aby wyłączyć kompresję, należy dodać `BlazorEnableCompression` Właściwość programu MSBuild do pliku projektu aplikacji i ustawić wartość na `false` :</span><span class="sxs-lookup"><span data-stu-id="40e57-132">To disable compression, add the `BlazorEnableCompression` MSBuild property to the app's project file and set the value to `false`:</span></span>

```xml
<PropertyGroup>
  <BlazorEnableCompression>false</BlazorEnableCompression>
</PropertyGroup>
```

<span data-ttu-id="40e57-133">`BlazorEnableCompression`Właściwość można przesłać do [`dotnet publish`](/dotnet/core/tools/dotnet-publish) polecenia z następującą składnią w powłoce poleceń:</span><span class="sxs-lookup"><span data-stu-id="40e57-133">The `BlazorEnableCompression` property can be passed to the [`dotnet publish`](/dotnet/core/tools/dotnet-publish) command with the following syntax in a command shell:</span></span>

```dotnetcli
dotnet publish -p:BlazorEnableCompression=false
```

## <a name="rewrite-urls-for-correct-routing"></a><span data-ttu-id="40e57-134">Ponownie Napisz adresy URL pod kątem prawidłowego routingu</span><span class="sxs-lookup"><span data-stu-id="40e57-134">Rewrite URLs for correct routing</span></span>

<span data-ttu-id="40e57-135">Żądania routingu dla składników strony w Blazor WebAssembly aplikacji nie są tak proste jak żądania routingu w Blazor Server hostowanej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-135">Routing requests for page components in a Blazor WebAssembly app isn't as straightforward as routing requests in a Blazor Server, hosted app.</span></span> <span data-ttu-id="40e57-136">Weź pod uwagę Blazor WebAssembly aplikację z dwoma składnikami:</span><span class="sxs-lookup"><span data-stu-id="40e57-136">Consider a Blazor WebAssembly app with two components:</span></span>

* <span data-ttu-id="40e57-137">`Main.razor`: Ładuje się w katalogu głównym aplikacji i zawiera link do `About` składnika ( `href="About"` ).</span><span class="sxs-lookup"><span data-stu-id="40e57-137">`Main.razor`: Loads at the root of the app and contains a link to the `About` component (`href="About"`).</span></span>
* <span data-ttu-id="40e57-138">`About.razor`: `About` składnik.</span><span class="sxs-lookup"><span data-stu-id="40e57-138">`About.razor`: `About` component.</span></span>

<span data-ttu-id="40e57-139">Gdy zażądano dokumentu domyślnego aplikacji przy użyciu paska adresu przeglądarki (na przykład `https://www.contoso.com/` ):</span><span class="sxs-lookup"><span data-stu-id="40e57-139">When the app's default document is requested using the browser's address bar (for example, `https://www.contoso.com/`):</span></span>

1. <span data-ttu-id="40e57-140">Przeglądarka wykonuje żądanie.</span><span class="sxs-lookup"><span data-stu-id="40e57-140">The browser makes a request.</span></span>
1. <span data-ttu-id="40e57-141">Zostanie zwrócona domyślna strona, która zwykle `index.html` .</span><span class="sxs-lookup"><span data-stu-id="40e57-141">The default page is returned, which is usually `index.html`.</span></span>
1. <span data-ttu-id="40e57-142">`index.html` wyładowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-142">`index.html` bootstraps the app.</span></span>
1. <span data-ttu-id="40e57-143">Blazorładuje router, a Razor `Main` składnik jest renderowany.</span><span class="sxs-lookup"><span data-stu-id="40e57-143">Blazor's router loads, and the Razor `Main` component is rendered.</span></span>

<span data-ttu-id="40e57-144">Na stronie głównej wybranie linku do `About` składnika działa na kliencie, ponieważ Blazor router uniemożliwia przeglądarce wykonywanie żądania w Internecie do `www.contoso.com` programu `About` i obsługuje wyrenderowany `About` składnik.</span><span class="sxs-lookup"><span data-stu-id="40e57-144">In the Main page, selecting the link to the `About` component works on the client because the Blazor router stops the browser from making a request on the Internet to `www.contoso.com` for `About` and serves the rendered `About` component itself.</span></span> <span data-ttu-id="40e57-145">Wszystkie żądania dotyczące wewnętrznych punktów końcowych *w Blazor WebAssembly aplikacji* działają w taki sam sposób: żądania nie wyzwalają żądań przeglądarki do zasobów hostowanych przez serwer w Internecie.</span><span class="sxs-lookup"><span data-stu-id="40e57-145">All of the requests for internal endpoints *within the Blazor WebAssembly app* work the same way: Requests don't trigger browser-based requests to server-hosted resources on the Internet.</span></span> <span data-ttu-id="40e57-146">Router obsługuje wewnętrznie żądania.</span><span class="sxs-lookup"><span data-stu-id="40e57-146">The router handles the requests internally.</span></span>

<span data-ttu-id="40e57-147">Żądanie kończy się niepowodzeniem, jeśli żądanie zostanie wykonane przy użyciu paska adresu przeglądarki `www.contoso.com/About` .</span><span class="sxs-lookup"><span data-stu-id="40e57-147">If a request is made using the browser's address bar for `www.contoso.com/About`, the request fails.</span></span> <span data-ttu-id="40e57-148">Ten zasób nie istnieje na hoście internetowym aplikacji, więc zwracana jest odpowiedź *404 — nie znaleziono* .</span><span class="sxs-lookup"><span data-stu-id="40e57-148">No such resource exists on the app's Internet host, so a *404 - Not Found* response is returned.</span></span>

<span data-ttu-id="40e57-149">Ponieważ przeglądarki wysyłają żądania do hostów internetowych dla stron po stronie klienta, serwery sieci Web i usługi hostingu muszą ponownie zapisywać wszystkie żądania dotyczące zasobów, które nie znajdują się fizycznie na serwerze na `index.html` stronie.</span><span class="sxs-lookup"><span data-stu-id="40e57-149">Because browsers make requests to Internet-based hosts for client-side pages, web servers and hosting services must rewrite all requests for resources not physically on the server to the `index.html` page.</span></span> <span data-ttu-id="40e57-150">Gdy `index.html` jest zwracany, Blazor router aplikacji przejmuje i reaguje na prawidłowy zasób.</span><span class="sxs-lookup"><span data-stu-id="40e57-150">When `index.html` is returned, the app's Blazor router takes over and responds with the correct resource.</span></span>

<span data-ttu-id="40e57-151">Podczas wdrażania na serwerze usług IIS można użyć modułu ponownego zapisywania adresu URL z opublikowanym `web.config` plikiem aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-151">When deploying to an IIS server, you can use the URL Rewrite Module with the app's published `web.config` file.</span></span> <span data-ttu-id="40e57-152">Aby uzyskać więcej informacji, zobacz sekcję [usług IIS](#iis) .</span><span class="sxs-lookup"><span data-stu-id="40e57-152">For more information, see the [IIS](#iis) section.</span></span>

## <a name="hosted-deployment-with-aspnet-core"></a><span data-ttu-id="40e57-153">Hostowane wdrożenie z ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="40e57-153">Hosted deployment with ASP.NET Core</span></span>

<span data-ttu-id="40e57-154">*Wdrożenie hostowane* Blazor WebAssembly umożliwia aplikacji przeglądarki z poziomu [aplikacji ASP.NET Core](xref:index) działającej na serwerze sieci Web.</span><span class="sxs-lookup"><span data-stu-id="40e57-154">A *hosted deployment* serves the Blazor WebAssembly app to browsers from an [ASP.NET Core app](xref:index) that runs on a web server.</span></span>

<span data-ttu-id="40e57-155">Aplikacja kliencka Blazor WebAssembly jest publikowana w `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` folderze aplikacji serwerowej wraz z wszelkimi innymi statycznymi zasobami sieci Web aplikacji serwera.</span><span class="sxs-lookup"><span data-stu-id="40e57-155">The client Blazor WebAssembly app is published into the `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` folder of the server app, along with any other static web assets of the server app.</span></span> <span data-ttu-id="40e57-156">Te dwie aplikacje są wdrażane razem.</span><span class="sxs-lookup"><span data-stu-id="40e57-156">The two apps are deployed together.</span></span> <span data-ttu-id="40e57-157">Wymagany jest serwer sieci Web, który umożliwia hostowanie aplikacji ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="40e57-157">A web server that is capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="40e57-158">W przypadku wdrożenia hostowanego program Visual Studio zawiera szablon projektu **Blazor WebAssembly aplikacji** ( `blazorwasm` szablon przy użyciu [`dotnet new`](/dotnet/core/tools/dotnet-new) polecenia) z **`Hosted`** wybraną opcją ( `-ho|--hosted` przy użyciu `dotnet new` polecenia).</span><span class="sxs-lookup"><span data-stu-id="40e57-158">For a hosted deployment, Visual Studio includes the **Blazor WebAssembly App** project template (`blazorwasm` template when using the [`dotnet new`](/dotnet/core/tools/dotnet-new) command) with the **`Hosted`** option selected (`-ho|--hosted` when using the `dotnet new` command).</span></span>

<span data-ttu-id="40e57-159">Aby uzyskać więcej informacji, zobacz następujące artykuły:</span><span class="sxs-lookup"><span data-stu-id="40e57-159">For more information, see the following articles:</span></span>

* <span data-ttu-id="40e57-160">ASP.NET Core hosting i wdrażanie aplikacji: <xref:host-and-deploy/index></span><span class="sxs-lookup"><span data-stu-id="40e57-160">ASP.NET Core app hosting and deployment: <xref:host-and-deploy/index></span></span>
* <span data-ttu-id="40e57-161">Wdrożenie do Azure App Service: <xref:tutorials/publish-to-azure-webapp-using-vs></span><span class="sxs-lookup"><span data-stu-id="40e57-161">Deployment to Azure App Service: <xref:tutorials/publish-to-azure-webapp-using-vs></span></span>
* <span data-ttu-id="40e57-162">Blazor Szablony projektu: <xref:blazor/project-structure></span><span class="sxs-lookup"><span data-stu-id="40e57-162">Blazor project templates: <xref:blazor/project-structure></span></span>

## <a name="hosted-deployment-with-multiple-blazor-webassembly-apps"></a><span data-ttu-id="40e57-163">Hostowane wdrożenie z wieloma Blazor WebAssembly aplikacjami</span><span class="sxs-lookup"><span data-stu-id="40e57-163">Hosted deployment with multiple Blazor WebAssembly apps</span></span>

### <a name="app-configuration"></a><span data-ttu-id="40e57-164">Konfiguracja aplikacji</span><span class="sxs-lookup"><span data-stu-id="40e57-164">App configuration</span></span>

<span data-ttu-id="40e57-165">Rozwiązania hostowane Blazor mogą obsługiwać wiele Blazor WebAssembly aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-165">Hosted Blazor solutions can serve multiple Blazor WebAssembly apps.</span></span>

> [!NOTE]
> <span data-ttu-id="40e57-166">W przykładzie w tej sekcji odwołuje się do korzystania z *rozwiązania* Visual Studio, ale użycie programu Visual Studio i rozwiązania Visual Studio nie jest wymagane, aby wiele aplikacji klienckich działało w Blazor WebAssembly scenariuszu hostowanych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-166">The example in this section references the use of a Visual Studio *solution*, but the use of Visual Studio and a Visual Studio solution isn't required for multiple client apps to work in a hosted Blazor WebAssembly apps scenario.</span></span> <span data-ttu-id="40e57-167">Jeśli nie używasz programu Visual Studio, zignoruj `{SOLUTION NAME}.sln` plik i wszystkie inne pliki utworzone dla programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="40e57-167">If you aren't using Visual Studio, ignore the `{SOLUTION NAME}.sln` file and any other files created for Visual Studio.</span></span>

<span data-ttu-id="40e57-168">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="40e57-168">In the following example:</span></span>

* <span data-ttu-id="40e57-169">Początkowa (pierwsza) aplikacja kliencka jest domyślnym projektem klienta rozwiązania utworzonego na podstawie Blazor WebAssembly szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="40e57-169">The initial (first) client app is the default client project of a solution created from the Blazor WebAssembly project template.</span></span> <span data-ttu-id="40e57-170">Pierwsza aplikacja kliencka jest dostępna w przeglądarce z adresu URL `/FirstApp` na porcie 5001 lub na hoście `firstapp.com` .</span><span class="sxs-lookup"><span data-stu-id="40e57-170">The first client app is accessible in a browser from the URL `/FirstApp` on either port 5001 or with a host of `firstapp.com`.</span></span>
* <span data-ttu-id="40e57-171">Druga aplikacja kliencka jest dodawana do rozwiązania `SecondBlazorApp.Client` .</span><span class="sxs-lookup"><span data-stu-id="40e57-171">A second client app is added to the solution, `SecondBlazorApp.Client`.</span></span> <span data-ttu-id="40e57-172">Druga aplikacja kliencka jest dostępna w przeglądarce z adresu URL `/SecondApp` na porcie 5002 lub na hoście `secondapp.com` .</span><span class="sxs-lookup"><span data-stu-id="40e57-172">The second client app is accessible in a browser from the the URL `/SecondApp` on either port 5002 or with a host of `secondapp.com`.</span></span>

<span data-ttu-id="40e57-173">Użyj istniejącego rozwiązania hostowanego Blazor lub Utwórz nowe rozwiązanie z Blazor szablonu hostowanego projektu:</span><span class="sxs-lookup"><span data-stu-id="40e57-173">Use an existing hosted Blazor solution or create a new solution from the Blazor Hosted project template:</span></span>

* <span data-ttu-id="40e57-174">W pliku projektu aplikacji klienckiej Dodaj `<StaticWebAssetBasePath>` Właściwość do `<PropertyGroup>` wartości, `FirstApp` Aby ustawić ścieżkę bazową dla statycznych zasobów projektu:</span><span class="sxs-lookup"><span data-stu-id="40e57-174">In the client app's project file, add a `<StaticWebAssetBasePath>` property to the `<PropertyGroup>` with a value of `FirstApp` to set the base path for the project's static assets:</span></span>

  ```xml
  <PropertyGroup>
    ...
    <StaticWebAssetBasePath>FirstApp</StaticWebAssetBasePath>
  </PropertyGroup>
  ```

* <span data-ttu-id="40e57-175">Dodaj drugą aplikację kliencką do rozwiązania:</span><span class="sxs-lookup"><span data-stu-id="40e57-175">Add a second client app to the solution:</span></span>

  * <span data-ttu-id="40e57-176">Dodaj folder o nazwie `SecondClient` do folderu rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="40e57-176">Add a folder named `SecondClient` to the solution's folder.</span></span> <span data-ttu-id="40e57-177">Folder rozwiązania utworzony na podstawie szablonu projektu zawiera następujący plik rozwiązania i foldery po `SecondClient` dodaniu folderu:</span><span class="sxs-lookup"><span data-stu-id="40e57-177">The solution folder created from the project template contains the following solution file and folders after the `SecondClient` folder is added:</span></span>
  
    * <span data-ttu-id="40e57-178">`Client` system32\drivers\etc</span><span class="sxs-lookup"><span data-stu-id="40e57-178">`Client` (folder)</span></span>
    * <span data-ttu-id="40e57-179">`SecondClient` system32\drivers\etc</span><span class="sxs-lookup"><span data-stu-id="40e57-179">`SecondClient` (folder)</span></span>
    * <span data-ttu-id="40e57-180">`Server` system32\drivers\etc</span><span class="sxs-lookup"><span data-stu-id="40e57-180">`Server` (folder)</span></span>
    * <span data-ttu-id="40e57-181">`Shared` system32\drivers\etc</span><span class="sxs-lookup"><span data-stu-id="40e57-181">`Shared` (folder)</span></span>
    * <span data-ttu-id="40e57-182">`{SOLUTION NAME}.sln` rozszerzeniem</span><span class="sxs-lookup"><span data-stu-id="40e57-182">`{SOLUTION NAME}.sln` (file)</span></span>

    <span data-ttu-id="40e57-183">Symbol zastępczy `{SOLUTION NAME}` jest nazwą rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="40e57-183">The placeholder `{SOLUTION NAME}` is the solution's name.</span></span>

  * <span data-ttu-id="40e57-184">Utwórz Blazor WebAssembly aplikację o nazwie `SecondBlazorApp.Client` w `SecondClient` folderze z Blazor WebAssembly szablonu projektu.</span><span class="sxs-lookup"><span data-stu-id="40e57-184">Create a Blazor WebAssembly app named `SecondBlazorApp.Client` in the `SecondClient` folder from the Blazor WebAssembly project template.</span></span>

  * <span data-ttu-id="40e57-185">W `SecondBlazorApp.Client` pliku projektu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="40e57-185">In the `SecondBlazorApp.Client` app's project file:</span></span>

    * <span data-ttu-id="40e57-186">Dodaj `<StaticWebAssetBasePath>` Właściwość do `<PropertyGroup>` wartości z `SecondApp` :</span><span class="sxs-lookup"><span data-stu-id="40e57-186">Add a `<StaticWebAssetBasePath>` property to the `<PropertyGroup>` with a value of `SecondApp`:</span></span>

      ```xml
      <PropertyGroup>
        ...
        <StaticWebAssetBasePath>SecondApp</StaticWebAssetBasePath>
      </PropertyGroup>
      ```

    * <span data-ttu-id="40e57-187">Dodaj odwołanie do projektu do `Shared` projektu:</span><span class="sxs-lookup"><span data-stu-id="40e57-187">Add a project reference to the `Shared` project:</span></span>

      ```xml
      <ItemGroup>
        <ProjectReference Include="..\Shared\{SOLUTION NAME}.Shared.csproj" />
      </ItemGroup>
      ```

      <span data-ttu-id="40e57-188">Symbol zastępczy `{SOLUTION NAME}` jest nazwą rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="40e57-188">The placeholder `{SOLUTION NAME}` is the solution's name.</span></span>

* <span data-ttu-id="40e57-189">W pliku projektu aplikacji serwera Utwórz odwołanie do projektu dla dodanej `SecondBlazorApp.Client` aplikacji klienckiej:</span><span class="sxs-lookup"><span data-stu-id="40e57-189">In the server app's project file, create a project reference for the added `SecondBlazorApp.Client` client app:</span></span>

  ```xml
  <ItemGroup>
    <ProjectReference Include="..\Client\{SOLUTION NAME}.Client.csproj" />
    <ProjectReference Include="..\SecondClient\SecondBlazorApp.Client.csproj" />
    <ProjectReference Include="..\Shared\{SOLUTION NAME}.Shared.csproj" />
  </ItemGroup>
  ```
  
  <span data-ttu-id="40e57-190">Symbol zastępczy `{SOLUTION NAME}` jest nazwą rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="40e57-190">The placeholder `{SOLUTION NAME}` is the solution's name.</span></span>

* <span data-ttu-id="40e57-191">W pliku aplikacji serwera `Properties/launchSettings.json` Skonfiguruj `applicationUrl` profil Kestrel ( `{SOLUTION NAME}.Server` ), aby uzyskać dostęp do aplikacji klienckich na portach 5001 i 5002:</span><span class="sxs-lookup"><span data-stu-id="40e57-191">In the server app's `Properties/launchSettings.json` file, configure the `applicationUrl` of the Kestrel profile (`{SOLUTION NAME}.Server`) to access the client apps at ports 5001 and 5002:</span></span>

  ```json
  "applicationUrl": "https://localhost:5001;https://localhost:5002",
  ```

* <span data-ttu-id="40e57-192">W metodzie aplikacji serwera `Startup.Configure` ( `Startup.cs` ) Usuń następujące wiersze, które pojawią się po wywołaniu <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection%2A> :</span><span class="sxs-lookup"><span data-stu-id="40e57-192">In the server app's `Startup.Configure` method (`Startup.cs`), remove the following lines, which appear after the call to <xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection%2A>:</span></span>

  ```csharp
  app.UseBlazorFrameworkFiles();
  app.UseStaticFiles();

  app.UseRouting();

  app.UseEndpoints(endpoints =>
  {
      endpoints.MapRazorPages();
      endpoints.MapControllers();
      endpoints.MapFallbackToFile("index.html");
  });
  ```

  <span data-ttu-id="40e57-193">Dodaj oprogramowanie pośredniczące, które mapuje żądania do aplikacji klienckich.</span><span class="sxs-lookup"><span data-stu-id="40e57-193">Add middleware that maps requests to the client apps.</span></span> <span data-ttu-id="40e57-194">Poniższy przykład konfiguruje oprogramowanie pośredniczące do uruchomienia, gdy:</span><span class="sxs-lookup"><span data-stu-id="40e57-194">The following example configures the middleware to run when:</span></span>

  * <span data-ttu-id="40e57-195">Port żądania ma wartość 5001 dla oryginalnej aplikacji klienckiej lub 5002 dla dodanej aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="40e57-195">The request port is either 5001 for the original client app or 5002 for the added client app.</span></span>
  * <span data-ttu-id="40e57-196">Host żądania jest `firstapp.com` przeznaczony dla oryginalnej aplikacji klienckiej lub `secondapp.com` dla dodanej aplikacji klienckiej.</span><span class="sxs-lookup"><span data-stu-id="40e57-196">The request host is either `firstapp.com` for the original client app or `secondapp.com` for the added client app.</span></span>

    > [!NOTE]
    > <span data-ttu-id="40e57-197">Przykład przedstawiony w tej sekcji wymaga dodatkowej konfiguracji dla:</span><span class="sxs-lookup"><span data-stu-id="40e57-197">The example shown in this section requires additional configuration for:</span></span>
    >
    > * <span data-ttu-id="40e57-198">Uzyskiwanie dostępu do aplikacji w przykładowych domenach hostów `firstapp.com` i `secondapp.com` .</span><span class="sxs-lookup"><span data-stu-id="40e57-198">Accessing the apps at the example host domains, `firstapp.com` and `secondapp.com`.</span></span>
    > * <span data-ttu-id="40e57-199">Certyfikaty dla aplikacji klienckich, aby włączyć zabezpieczenia protokołu TLS (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="40e57-199">Certificates for the client apps to enable TLS security (HTTPS).</span></span>
    >
    > <span data-ttu-id="40e57-200">Wymagana konfiguracja wykracza poza zakres tego artykułu i zależy od tego, w jaki sposób jest hostowane rozwiązanie.</span><span class="sxs-lookup"><span data-stu-id="40e57-200">The required configuration is beyond the scope of this article and depends on how the solution is hosted.</span></span> <span data-ttu-id="40e57-201">Aby uzyskać więcej informacji, zobacz artykuł dotyczący [hosta i wdrażania](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="40e57-201">For more information see the [Host and deploy articles](xref:host-and-deploy/index).</span></span>

  <span data-ttu-id="40e57-202">Umieść następujący kod, w którym wiersze zostały wcześniej usunięte:</span><span class="sxs-lookup"><span data-stu-id="40e57-202">Place the following code where the lines were removed earlier:</span></span>

  ```csharp
  app.MapWhen(ctx => ctx.Request.Host.Port == 5001 || 
      ctx.Request.Host.Equals("firstapp.com"), first =>
  {
      first.Use((ctx, nxt) =>
      {
          ctx.Request.Path = "/FirstApp" + ctx.Request.Path;
          return nxt();
      });

      first.UseBlazorFrameworkFiles("/FirstApp");
      first.UseStaticFiles();
      first.UseStaticFiles("/FirstApp");
      first.UseRouting();

      first.UseEndpoints(endpoints =>
      {
          endpoints.MapControllers();
          endpoints.MapFallbackToFile("/FirstApp/{*path:nonfile}", 
              "FirstApp/index.html");
      });
  });
  
  app.MapWhen(ctx => ctx.Request.Host.Port == 5002 || 
      ctx.Request.Host.Equals("secondapp.com"), second =>
  {
      second.Use((ctx, nxt) =>
      {
          ctx.Request.Path = "/SecondApp" + ctx.Request.Path;
          return nxt();
      });

      second.UseBlazorFrameworkFiles("/SecondApp");
      second.UseStaticFiles();
      second.UseStaticFiles("/SecondApp");
      second.UseRouting();

      second.UseEndpoints(endpoints =>
      {
          endpoints.MapControllers();
          endpoints.MapFallbackToFile("/SecondApp/{*path:nonfile}", 
              "SecondApp/index.html");
      });
  });
  ```

* <span data-ttu-id="40e57-203">W kontrolerze prognozowania pogody aplikacji serwera `Controllers/WeatherForecastController.cs` Zastąp istniejącą trasę ( `[Route("[controller]")]` ) do `WeatherForecastController` następującej trasy:</span><span class="sxs-lookup"><span data-stu-id="40e57-203">In the server app's weather forecast controller (`Controllers/WeatherForecastController.cs`), replace the existing route (`[Route("[controller]")]`) to `WeatherForecastController` with the following routes:</span></span>

  ```csharp
  [Route("FirstApp/[controller]")]
  [Route("SecondApp/[controller]")]
  ```

  <span data-ttu-id="40e57-204">Oprogramowanie pośredniczące dodane do metody aplikacji serwerowej `Startup.Configure` wcześniej modyfikuje przychodzące żądania do `/WeatherForecast` albo `/FirstApp/WeatherForecast` `/SecondApp/WeatherForecast` w zależności od portu (5001/5002) lub domeny ( `firstapp.com` / `secondapp.com` ).</span><span class="sxs-lookup"><span data-stu-id="40e57-204">The middleware added to the server app's `Startup.Configure` method earlier modifies incoming requests to `/WeatherForecast` to either `/FirstApp/WeatherForecast` or `/SecondApp/WeatherForecast` depending on the port (5001/5002) or domain (`firstapp.com`/`secondapp.com`).</span></span> <span data-ttu-id="40e57-205">Powyższe trasy kontrolera są wymagane w celu zwrócenia danych pogody z aplikacji serwerowej do aplikacji klienckich.</span><span class="sxs-lookup"><span data-stu-id="40e57-205">The preceding controller routes are required in order to return weather data from the server app to the client apps.</span></span>

### <a name="static-assets-and-class-libraries"></a><span data-ttu-id="40e57-206">Statyczne zasoby i biblioteki klas</span><span class="sxs-lookup"><span data-stu-id="40e57-206">Static assets and class libraries</span></span>

<span data-ttu-id="40e57-207">W przypadku zasobów statycznych należy stosować następujące podejścia:</span><span class="sxs-lookup"><span data-stu-id="40e57-207">Use the following approaches for static assets:</span></span>

* <span data-ttu-id="40e57-208">Gdy element zawartości znajduje się w folderze aplikacji klienta `wwwroot` , podaj ścieżki zwykle:</span><span class="sxs-lookup"><span data-stu-id="40e57-208">When the asset is in the client app's `wwwroot` folder, provide their paths normally:</span></span>

  ```razor
  <img alt="..." src="/{ASSET FILE NAME}" />
  ```

* <span data-ttu-id="40e57-209">Gdy element zawartości znajduje się w `wwwroot` folderze [ Razor biblioteki klas (RCL)](xref:blazor/components/class-libraries), odwołuje się do zasobu statycznego w aplikacji klienckiej zgodnie ze wskazówkami w [artykule RCL](xref:razor-pages/ui-class#consume-content-from-a-referenced-rcl):</span><span class="sxs-lookup"><span data-stu-id="40e57-209">When the asset is in the `wwwroot` folder of a [Razor Class Library (RCL)](xref:blazor/components/class-libraries), reference the static asset in the client app per the guidance in the [RCL article](xref:razor-pages/ui-class#consume-content-from-a-referenced-rcl):</span></span>

  ```razor
  <img alt="..." src="_content/{LIBRARY NAME}/{ASSET FILE NAME}" />
  ```

<!-- HOLD for reactivation at 5.x

::: moniker range=">= aspnetcore-5.0"

Components provided to a client app by a class library are referenced normally. If any components require stylesheets or JavaScript files, use either of the following approaches to obtain the static assets:

* The client app's `wwwroot/index.html` file can link (`<link>`) to the static assets.
* The component can use the framework's [`Link` component](xref:blazor/fundamentals/signalr#influence-html-head-tag-elements) to obtain the static assets.

The preceding approaches are demonstrated in the following examples.

::: moniker-end

::: moniker range="< aspnetcore-5.0"

-->

<span data-ttu-id="40e57-210">Składniki udostępniane aplikacji klienckiej przez bibliotekę klas są zwykle przywoływane.</span><span class="sxs-lookup"><span data-stu-id="40e57-210">Components provided to a client app by a class library are referenced normally.</span></span> <span data-ttu-id="40e57-211">Jeśli wszystkie składniki wymagają plików stylów lub JavaScript, plik aplikacji klienta `wwwroot/index.html` musi zawierać poprawne linki do zasobów statycznych.</span><span class="sxs-lookup"><span data-stu-id="40e57-211">If any components require stylesheets or JavaScript files, the client app's `wwwroot/index.html` file must include the correct static asset links.</span></span> <span data-ttu-id="40e57-212">Te podejścia przedstawiono w poniższych przykładach.</span><span class="sxs-lookup"><span data-stu-id="40e57-212">These approaches are demonstrated in the following examples.</span></span>

<!-- HOLD for reactivation at 5.x

::: moniker-end

-->

<span data-ttu-id="40e57-213">Dodaj następujący `Jeep` składnik do jednej z aplikacji klienckich.</span><span class="sxs-lookup"><span data-stu-id="40e57-213">Add the following `Jeep` component to one of the client apps.</span></span> <span data-ttu-id="40e57-214">`Jeep`Składnik używa:</span><span class="sxs-lookup"><span data-stu-id="40e57-214">The `Jeep` component uses:</span></span>

* <span data-ttu-id="40e57-215">Obraz z folderu aplikacji klienta `wwwroot` ( `jeep-cj.png` ).</span><span class="sxs-lookup"><span data-stu-id="40e57-215">An image from the client app's `wwwroot` folder (`jeep-cj.png`).</span></span>
* <span data-ttu-id="40e57-216">Obraz z [dodanego folderu Razor biblioteki składników](xref:blazor/components/class-libraries) () `JeepImage` `wwwroot` `jeep-yj.png` .</span><span class="sxs-lookup"><span data-stu-id="40e57-216">An image from an [added Razor component library](xref:blazor/components/class-libraries) (`JeepImage`) `wwwroot` folder (`jeep-yj.png`).</span></span>
* <span data-ttu-id="40e57-217">Przykładowy składnik ( `Component1` ) jest tworzony automatycznie przez szablon projektu RCL, gdy `JeepImage` biblioteka zostanie dodana do rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="40e57-217">The example component (`Component1`) is created automatically by the RCL project template when the `JeepImage` library is added to the solution.</span></span>

```razor
@page "/Jeep"

<h1>1979 Jeep CJ-5&trade;</h1>

<p>
    <img alt="1979 Jeep CJ-5&trade;" src="/jeep-cj.png" />
</p>

<h1>1991 Jeep YJ&trade;</h1>

<p>
    <img alt="1991 Jeep YJ&trade;" src="_content/JeepImage/jeep-yj.png" />
</p>

<p>
    <em>Jeep CJ-5</em> and <em>Jeep YJ</em> are a trademarks of 
    <a href="https://www.fcagroup.com">Fiat Chrysler Automobiles</a>.
</p>

<JeepImage.Component1 />
```

> [!WARNING]
> <span data-ttu-id="40e57-218">Nie Publikuj obrazów pojazdów publicznie, chyba że są **one** właścicielami.</span><span class="sxs-lookup"><span data-stu-id="40e57-218">Do **not** publish images of vehicles publicly unless you own the images.</span></span> <span data-ttu-id="40e57-219">W przeciwnym razie ryzyko naruszenia praw autorskich.</span><span class="sxs-lookup"><span data-stu-id="40e57-219">Otherwise, you risk copyright infringement.</span></span>

<!-- HOLD for reactivation at 5.x

::: moniker range=">= aspnetcore-5.0"

The library's `jeep-yj.png` image can also be added to the library's `Component1` component (`Component1.razor`). To provide the `my-component` CSS class to the client app's page, link to the library's stylesheet using the framework's [`Link` component](xref:blazor/fundamentals/signalr#influence-html-head-tag-elements):

```razor
<div class="my-component">
    <Link href="_content/JeepImage/styles.css" rel="stylesheet" />

    <h1>JeepImage.Component1</h1>

    <p>
        This Blazor component is defined in the <strong>JeepImage</strong> package.
    </p>

    <p>
        <img alt="1991 Jeep YJ&trade;" src="_content/JeepImage/jeep-yj.png" />
    </p>
</div>
```

An alternative to using the [`Link` component](xref:blazor/fundamentals/signalr#influence-html-head-tag-elements) is to load the stylesheet from the client app's `wwwroot/index.html` file. This approach makes the stylesheet available to all of the components in the client app:

```html
<head>
    ...
    <link href="_content/JeepImage/styles.css" rel="stylesheet" />
</head>
```

::: moniker-end

::: moniker range="< aspnetcore-5.0"

-->

<span data-ttu-id="40e57-220">`jeep-yj.png`Obraz biblioteki można również dodać do `Component1` składnika biblioteki ( `Component1.razor` ):</span><span class="sxs-lookup"><span data-stu-id="40e57-220">The library's `jeep-yj.png` image can also be added to the library's `Component1` component (`Component1.razor`):</span></span>

```razor
<div class="my-component">
    <h1>JeepImage.Component1</h1>

    <p>
        This Blazor component is defined in the <strong>JeepImage</strong> package.
    </p>

    <p>
        <img alt="1991 Jeep YJ&trade;" src="_content/JeepImage/jeep-yj.png" />
    </p>
</div>
```

<span data-ttu-id="40e57-221">Plik aplikacji klienta `wwwroot/index.html` żąda arkusza stylów biblioteki z następującym dodany `<link>` tag:</span><span class="sxs-lookup"><span data-stu-id="40e57-221">The client app's `wwwroot/index.html` file requests the library's stylesheet with the following added `<link>` tag:</span></span>

```html
<head>
    ...
    <link href="_content/JeepImage/styles.css" rel="stylesheet" />
</head>
```

<!-- HOLD for reactivation at 5.x

::: moniker-end

-->

<span data-ttu-id="40e57-222">Dodaj nawigację do `Jeep` składnika w składniku aplikacji klienckiej `NavMenu` ( `Shared/NavMenu.razor` ):</span><span class="sxs-lookup"><span data-stu-id="40e57-222">Add navigation to the `Jeep` component in the client app's `NavMenu` component (`Shared/NavMenu.razor`):</span></span>

```razor
<li class="nav-item px-3">
    <NavLink class="nav-link" href="Jeep">
        <span class="oi oi-list-rich" aria-hidden="true"></span> Jeep
    </NavLink>
</li>
```

<span data-ttu-id="40e57-223">Aby uzyskać więcej informacji na temat RCLs, zobacz:</span><span class="sxs-lookup"><span data-stu-id="40e57-223">For more information on RCLs, see:</span></span>

* <xref:blazor/components/class-libraries>
* <xref:razor-pages/ui-class>

## <a name="standalone-deployment"></a><span data-ttu-id="40e57-224">Wdrożenie autonomiczne</span><span class="sxs-lookup"><span data-stu-id="40e57-224">Standalone deployment</span></span>

<span data-ttu-id="40e57-225">*Wdrożenie samodzielne* służy Blazor WebAssembly jako zestaw plików statycznych, które są żądane bezpośrednio przez klientów.</span><span class="sxs-lookup"><span data-stu-id="40e57-225">A *standalone deployment* serves the Blazor WebAssembly app as a set of static files that are requested directly by clients.</span></span> <span data-ttu-id="40e57-226">Każdy statyczny serwer plików jest w stanie obsłużyć Blazor aplikację.</span><span class="sxs-lookup"><span data-stu-id="40e57-226">Any static file server is able to serve the Blazor app.</span></span>

<span data-ttu-id="40e57-227">Zasoby wdrażania autonomicznego są publikowane w `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` folderze.</span><span class="sxs-lookup"><span data-stu-id="40e57-227">Standalone deployment assets are published into the `/bin/Release/{TARGET FRAMEWORK}/publish/wwwroot` folder.</span></span>

### <a name="azure-app-service"></a><span data-ttu-id="40e57-228">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="40e57-228">Azure App Service</span></span>

<span data-ttu-id="40e57-229">Blazor WebAssembly Aplikacje można wdrażać w usłudze Azure App Services w systemie Windows, który jest hostem aplikacji w [usługach IIS](#iis).</span><span class="sxs-lookup"><span data-stu-id="40e57-229">Blazor WebAssembly apps can be deployed to Azure App Services on Windows, which hosts the app on [IIS](#iis).</span></span>

<span data-ttu-id="40e57-230">Wdrażanie autonomicznej Blazor WebAssembly aplikacji do Azure App Service dla systemu Linux nie jest obecnie obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="40e57-230">Deploying a standalone Blazor WebAssembly app to Azure App Service for Linux isn't currently supported.</span></span> <span data-ttu-id="40e57-231">Obraz serwera z systemem Linux do hostowania aplikacji nie jest obecnie dostępny.</span><span class="sxs-lookup"><span data-stu-id="40e57-231">A Linux server image to host the app isn't available at this time.</span></span> <span data-ttu-id="40e57-232">Trwa wykonywanie pracy, aby włączyć ten scenariusz.</span><span class="sxs-lookup"><span data-stu-id="40e57-232">Work is in progress to enable this scenario.</span></span>

### <a name="iis"></a><span data-ttu-id="40e57-233">IIS</span><span class="sxs-lookup"><span data-stu-id="40e57-233">IIS</span></span>

<span data-ttu-id="40e57-234">Program IIS jest obsługującym statycznego serwera plików dla Blazor aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-234">IIS is a capable static file server for Blazor apps.</span></span> <span data-ttu-id="40e57-235">Aby skonfigurować usługi IIS do hostowania Blazor , zobacz [Tworzenie statycznej witryny sieci Web w usługach IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span><span class="sxs-lookup"><span data-stu-id="40e57-235">To configure IIS to host Blazor, see [Build a Static Website on IIS](/iis/manage/creating-websites/scenario-build-a-static-website-on-iis).</span></span>

<span data-ttu-id="40e57-236">Opublikowane zasoby są tworzone w `/bin/Release/{TARGET FRAMEWORK}/publish` folderze.</span><span class="sxs-lookup"><span data-stu-id="40e57-236">Published assets are created in the `/bin/Release/{TARGET FRAMEWORK}/publish` folder.</span></span> <span data-ttu-id="40e57-237">Hostowanie zawartości `publish` folderu na serwerze sieci Web lub w usłudze hostingu.</span><span class="sxs-lookup"><span data-stu-id="40e57-237">Host the contents of the `publish` folder on the web server or hosting service.</span></span>

#### <a name="webconfig"></a><span data-ttu-id="40e57-238">web.config</span><span class="sxs-lookup"><span data-stu-id="40e57-238">web.config</span></span>

<span data-ttu-id="40e57-239">Po Blazor opublikowaniu projektu `web.config` tworzony jest plik z następującą konfiguracją usług IIS:</span><span class="sxs-lookup"><span data-stu-id="40e57-239">When a Blazor project is published, a `web.config` file is created with the following IIS configuration:</span></span>

* <span data-ttu-id="40e57-240">Typy MIME są ustawiane dla następujących rozszerzeń plików:</span><span class="sxs-lookup"><span data-stu-id="40e57-240">MIME types are set for the following file extensions:</span></span>
  * <span data-ttu-id="40e57-241">`.dll`: `application/octet-stream`</span><span class="sxs-lookup"><span data-stu-id="40e57-241">`.dll`: `application/octet-stream`</span></span>
  * <span data-ttu-id="40e57-242">`.json`: `application/json`</span><span class="sxs-lookup"><span data-stu-id="40e57-242">`.json`: `application/json`</span></span>
  * <span data-ttu-id="40e57-243">`.wasm`: `application/wasm`</span><span class="sxs-lookup"><span data-stu-id="40e57-243">`.wasm`: `application/wasm`</span></span>
  * <span data-ttu-id="40e57-244">`.woff`: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="40e57-244">`.woff`: `application/font-woff`</span></span>
  * <span data-ttu-id="40e57-245">`.woff2`: `application/font-woff`</span><span class="sxs-lookup"><span data-stu-id="40e57-245">`.woff2`: `application/font-woff`</span></span>
* <span data-ttu-id="40e57-246">Kompresja HTTP jest włączona dla następujących typów MIME:</span><span class="sxs-lookup"><span data-stu-id="40e57-246">HTTP compression is enabled for the following MIME types:</span></span>
  * `application/octet-stream`
  * `application/wasm`
* <span data-ttu-id="40e57-247">Reguły modułu ponownego zapisywania adresu URL zostały ustanowione:</span><span class="sxs-lookup"><span data-stu-id="40e57-247">URL Rewrite Module rules are established:</span></span>
  * <span data-ttu-id="40e57-248">Obsługuj podkatalog, w którym znajdują się zasoby statyczne aplikacji ( `wwwroot/{PATH REQUESTED}` ).</span><span class="sxs-lookup"><span data-stu-id="40e57-248">Serve the sub-directory where the app's static assets reside (`wwwroot/{PATH REQUESTED}`).</span></span>
  * <span data-ttu-id="40e57-249">Utwórz Routing awaryjny SPA, aby żądania dotyczące zasobów nienależących do pliku zostały przekierowane do domyślnego dokumentu aplikacji w folderze zasobów statycznych ( `wwwroot/index.html` ).</span><span class="sxs-lookup"><span data-stu-id="40e57-249">Create SPA fallback routing so that requests for non-file assets are redirected to the app's default document in its static assets folder (`wwwroot/index.html`).</span></span>
  
#### <a name="use-a-custom-webconfig"></a><span data-ttu-id="40e57-250">Użyj niestandardowego web.config</span><span class="sxs-lookup"><span data-stu-id="40e57-250">Use a custom web.config</span></span>

<span data-ttu-id="40e57-251">Aby użyć pliku niestandardowego `web.config` , umieść `web.config` plik niestandardowy w folderze głównym folderu projektu.</span><span class="sxs-lookup"><span data-stu-id="40e57-251">To use a custom `web.config` file, place the custom `web.config` file at the root of the project folder.</span></span> <span data-ttu-id="40e57-252">Skonfiguruj projekt do publikowania zasobów specyficznych dla usług IIS przy użyciu `PublishIISAssets` w pliku projektu aplikacji i Opublikuj projekt:</span><span class="sxs-lookup"><span data-stu-id="40e57-252">Configure the project to publish IIS-specific assets using `PublishIISAssets` in the app's project file and publish the project:</span></span>

```xml
<PropertyGroup>
  <PublishIISAssets>true</PublishIISAssets>
</PropertyGroup>
```

#### <a name="install-the-url-rewrite-module"></a><span data-ttu-id="40e57-253">Zainstaluj moduł ponownego zapisywania adresów URL</span><span class="sxs-lookup"><span data-stu-id="40e57-253">Install the URL Rewrite Module</span></span>

<span data-ttu-id="40e57-254">[Moduł ponownego zapisywania adresu URL](https://www.iis.net/downloads/microsoft/url-rewrite) jest wymagany do ponownego zapisywania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="40e57-254">The [URL Rewrite Module](https://www.iis.net/downloads/microsoft/url-rewrite) is required to rewrite URLs.</span></span> <span data-ttu-id="40e57-255">Moduł nie jest instalowany domyślnie i nie jest dostępny do zainstalowania jako funkcja usługi roli Serwer sieci Web (IIS).</span><span class="sxs-lookup"><span data-stu-id="40e57-255">The module isn't installed by default, and it isn't available for install as a Web Server (IIS) role service feature.</span></span> <span data-ttu-id="40e57-256">Moduł musi zostać pobrany z witryny sieci Web usług IIS.</span><span class="sxs-lookup"><span data-stu-id="40e57-256">The module must be downloaded from the IIS website.</span></span> <span data-ttu-id="40e57-257">Zainstaluj moduł przy użyciu Instalatora platformy sieci Web:</span><span class="sxs-lookup"><span data-stu-id="40e57-257">Use the Web Platform Installer to install the module:</span></span>

1. <span data-ttu-id="40e57-258">Lokalnie przejdź do [strony pobierania modułu ponowne zapisywanie adresów URL](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span><span class="sxs-lookup"><span data-stu-id="40e57-258">Locally, navigate to the [URL Rewrite Module downloads page](https://www.iis.net/downloads/microsoft/url-rewrite#additionalDownloads).</span></span> <span data-ttu-id="40e57-259">W przypadku wersji angielskiej wybierz pozycję **Instalatora WebPI** , aby pobrać Instalatora Instalatora WebPI.</span><span class="sxs-lookup"><span data-stu-id="40e57-259">For the English version, select **WebPI** to download the WebPI installer.</span></span> <span data-ttu-id="40e57-260">W przypadku innych języków wybierz odpowiednią architekturę dla serwera (x86/x64), aby pobrać Instalatora.</span><span class="sxs-lookup"><span data-stu-id="40e57-260">For other languages, select the appropriate architecture for the server (x86/x64) to download the installer.</span></span>
1. <span data-ttu-id="40e57-261">Skopiuj Instalatora na serwer.</span><span class="sxs-lookup"><span data-stu-id="40e57-261">Copy the installer to the server.</span></span> <span data-ttu-id="40e57-262">Uruchom instalatora.</span><span class="sxs-lookup"><span data-stu-id="40e57-262">Run the installer.</span></span> <span data-ttu-id="40e57-263">Wybierz przycisk **Zainstaluj** i zaakceptuj postanowienia licencyjne.</span><span class="sxs-lookup"><span data-stu-id="40e57-263">Select the **Install** button and accept the license terms.</span></span> <span data-ttu-id="40e57-264">Po zakończeniu instalacji nie jest wymagane ponowne uruchomienie serwera.</span><span class="sxs-lookup"><span data-stu-id="40e57-264">A server restart isn't required after the install completes.</span></span>

#### <a name="configure-the-website"></a><span data-ttu-id="40e57-265">Skonfiguruj witrynę sieci Web</span><span class="sxs-lookup"><span data-stu-id="40e57-265">Configure the website</span></span>

<span data-ttu-id="40e57-266">Ustaw **ścieżkę fizyczną** witryny sieci Web do folderu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-266">Set the website's **Physical path** to the app's folder.</span></span> <span data-ttu-id="40e57-267">Folder zawiera:</span><span class="sxs-lookup"><span data-stu-id="40e57-267">The folder contains:</span></span>

* <span data-ttu-id="40e57-268">Plik, za `web.config` pomocą którego usługi IIS konfiguruje witrynę sieci Web, w tym wymagane reguły przekierowań i typy zawartości plików.</span><span class="sxs-lookup"><span data-stu-id="40e57-268">The `web.config` file that IIS uses to configure the website, including the required redirect rules and file content types.</span></span>
* <span data-ttu-id="40e57-269">Folder elementu zawartości statycznej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-269">The app's static asset folder.</span></span>

#### <a name="host-as-an-iis-sub-app"></a><span data-ttu-id="40e57-270">Host jako podrzędną aplikację usług IIS</span><span class="sxs-lookup"><span data-stu-id="40e57-270">Host as an IIS sub-app</span></span>

<span data-ttu-id="40e57-271">Jeśli aplikacja autonomiczna jest hostowana jako podaplikacja usług IIS, wykonaj jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="40e57-271">If a standalone app is hosted as an IIS sub-app, perform either of the following:</span></span>

* <span data-ttu-id="40e57-272">Wyłącz procedurę obsługi ASP.NET Core dziedziczonego modułu.</span><span class="sxs-lookup"><span data-stu-id="40e57-272">Disable the inherited ASP.NET Core Module handler.</span></span>

  <span data-ttu-id="40e57-273">Usuń program obsługi w Blazor opublikowanym pliku aplikacji `web.config` , dodając `<handlers>` sekcję do pliku:</span><span class="sxs-lookup"><span data-stu-id="40e57-273">Remove the handler in the Blazor app's published `web.config` file by adding a `<handlers>` section to the file:</span></span>

  ```xml
  <handlers>
    <remove name="aspNetCore" />
  </handlers>
  ```

* <span data-ttu-id="40e57-274">Wyłącz dziedziczenie sekcji głównej (nadrzędnej) aplikacji `<system.webServer>` przy użyciu `<location>` elementu z `inheritInChildApplications` ustawioną opcją `false` :</span><span class="sxs-lookup"><span data-stu-id="40e57-274">Disable inheritance of the root (parent) app's `<system.webServer>` section using a `<location>` element with `inheritInChildApplications` set to `false`:</span></span>

  ```xml
  <?xml version="1.0" encoding="utf-8"?>
  <configuration>
    <location path="." inheritInChildApplications="false">
      <system.webServer>
        <handlers>
          <add name="aspNetCore" ... />
        </handlers>
        <aspNetCore ... />
      </system.webServer>
    </location>
  </configuration>
  ```

<span data-ttu-id="40e57-275">Usuwanie procedury obsługi lub wyłączanie dziedziczenia jest wykonywane poza [konfiguracją ścieżki podstawowej aplikacji](xref:blazor/host-and-deploy/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="40e57-275">Removing the handler or disabling inheritance is performed in addition to [configuring the app's base path](xref:blazor/host-and-deploy/index#app-base-path).</span></span> <span data-ttu-id="40e57-276">Ustaw ścieżkę bazową aplikacji w `index.html` pliku aplikacji na alias IIS używany podczas konfigurowania aplikacji podrzędnej w usługach IIS.</span><span class="sxs-lookup"><span data-stu-id="40e57-276">Set the app base path in the app's `index.html` file to the IIS alias used when configuring the sub-app in IIS.</span></span>

#### <a name="brotli-and-gzip-compression"></a><span data-ttu-id="40e57-277">Brotli i Kompresja gzip</span><span class="sxs-lookup"><span data-stu-id="40e57-277">Brotli and Gzip compression</span></span>

<span data-ttu-id="40e57-278">*Ta sekcja dotyczy tylko aplikacji autonomicznych Blazor WebAssembly . aplikacje hostowane Blazor używają domyślnego pliku aplikacji ASP.NET Core `web.config` , a nie pliku połączonego z tą sekcją.*</span><span class="sxs-lookup"><span data-stu-id="40e57-278">*This section only applies to standalone Blazor WebAssembly apps. Hosted Blazor apps use a default ASP.NET Core app `web.config` file, not the file linked in this section.*</span></span>

<span data-ttu-id="40e57-279">Program IIS można skonfigurować `web.config` w taki sposób, aby obsługiwał skompresowane zasoby Brotli lub gzip Blazor dla aplikacji autonomicznych Blazor WebAssembly .</span><span class="sxs-lookup"><span data-stu-id="40e57-279">IIS can be configured via `web.config` to serve Brotli or Gzip compressed Blazor assets for standalone Blazor WebAssembly apps.</span></span> <span data-ttu-id="40e57-280">Przykładowy plik konfiguracji można znaleźć w temacie [`web.config`](https://github.com/dotnet/AspNetCore.Docs/blob/main/aspnetcore/blazor/host-and-deploy/webassembly/_samples/web.config?raw=true) .</span><span class="sxs-lookup"><span data-stu-id="40e57-280">For an example configuration file, see [`web.config`](https://github.com/dotnet/AspNetCore.Docs/blob/main/aspnetcore/blazor/host-and-deploy/webassembly/_samples/web.config?raw=true).</span></span>

<span data-ttu-id="40e57-281">Dodatkowa konfiguracja przykładowego `web.config` pliku może być wymagana w następujących scenariuszach:</span><span class="sxs-lookup"><span data-stu-id="40e57-281">Additional configuration of the example `web.config` file might be required in the following scenarios:</span></span>

* <span data-ttu-id="40e57-282">Specyfikacja aplikacji wywołuje jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="40e57-282">The app's specification calls for either of the following:</span></span>
  * <span data-ttu-id="40e57-283">Obsługa skompresowanych plików, które nie są konfigurowane przez przykładowy `web.config` plik.</span><span class="sxs-lookup"><span data-stu-id="40e57-283">Serving compressed files that aren't configured by the example `web.config` file.</span></span>
  * <span data-ttu-id="40e57-284">Obsługa skompresowanych plików skonfigurowanych przez przykładowy `web.config` plik w nieskompresowanym formacie.</span><span class="sxs-lookup"><span data-stu-id="40e57-284">Serving compressed files configured by the example `web.config` file in an uncompressed format.</span></span>
* <span data-ttu-id="40e57-285">Konfiguracja usług IIS na serwerze (na przykład `applicationHost.config` ) zapewnia ustawienia domyślne usług IIS na poziomie serwera.</span><span class="sxs-lookup"><span data-stu-id="40e57-285">The server's IIS configuration (for example, `applicationHost.config`) provides server-level IIS defaults.</span></span> <span data-ttu-id="40e57-286">W zależności od konfiguracji na poziomie serwera aplikacja może wymagać innej konfiguracji usług IIS niż zawartość pliku przykładowego `web.config` .</span><span class="sxs-lookup"><span data-stu-id="40e57-286">Depending on the server-level configuration, the app might require a different IIS configuration than what the example `web.config` file contains.</span></span>

#### <a name="troubleshooting"></a><span data-ttu-id="40e57-287">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="40e57-287">Troubleshooting</span></span>

<span data-ttu-id="40e57-288">W przypadku odebrania *500 — wewnętrzny błąd serwera* , a Menedżer usług IIS zgłasza błędy przy próbie uzyskania dostępu do konfiguracji witryny sieci Web, upewnij się, że zainstalowano moduł ponownego zapisywania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="40e57-288">If a *500 - Internal Server Error* is received and IIS Manager throws errors when attempting to access the website's configuration, confirm that the URL Rewrite Module is installed.</span></span> <span data-ttu-id="40e57-289">Gdy moduł nie jest zainstalowany, `web.config` nie można przeanalizować pliku przez usługi IIS.</span><span class="sxs-lookup"><span data-stu-id="40e57-289">When the module isn't installed, the `web.config` file can't be parsed by IIS.</span></span> <span data-ttu-id="40e57-290">Zapobiega to załadowaniu przez Menedżera usług IIS konfiguracji witryny sieci Web i witryny sieci Web na podstawie Blazor plików statycznych.</span><span class="sxs-lookup"><span data-stu-id="40e57-290">This prevents the IIS Manager from loading the website's configuration and the website from serving Blazor's static files.</span></span>

<span data-ttu-id="40e57-291">Aby uzyskać więcej informacji na temat rozwiązywania problemów z wdrożeniami w usługach IIS, zobacz <xref:test/troubleshoot-azure-iis> .</span><span class="sxs-lookup"><span data-stu-id="40e57-291">For more information on troubleshooting deployments to IIS, see <xref:test/troubleshoot-azure-iis>.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="40e57-292">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="40e57-292">Azure Storage</span></span>

<span data-ttu-id="40e57-293">Hosting pliku statycznego [usługi Azure Storage](/azure/storage/) umożliwia Blazor hosting aplikacji bezserwerowych.</span><span class="sxs-lookup"><span data-stu-id="40e57-293">[Azure Storage](/azure/storage/) static file hosting allows serverless Blazor app hosting.</span></span> <span data-ttu-id="40e57-294">Obsługiwane są niestandardowe nazwy domen, usługa Azure Content Delivery Network (CDN) i protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="40e57-294">Custom domain names, the Azure Content Delivery Network (CDN), and HTTPS are supported.</span></span>

<span data-ttu-id="40e57-295">Gdy usługa BLOB jest włączona dla hostingu statycznej witryny sieci Web na koncie magazynu:</span><span class="sxs-lookup"><span data-stu-id="40e57-295">When the blob service is enabled for static website hosting on a storage account:</span></span>

* <span data-ttu-id="40e57-296">Ustaw **nazwę dokumentu indeksu** na `index.html` .</span><span class="sxs-lookup"><span data-stu-id="40e57-296">Set the **Index document name** to `index.html`.</span></span>
* <span data-ttu-id="40e57-297">Ustaw **ścieżkę do dokumentu błędu** `index.html` .</span><span class="sxs-lookup"><span data-stu-id="40e57-297">Set the **Error document path** to `index.html`.</span></span> <span data-ttu-id="40e57-298">Razor składniki i inne punkty końcowe inne niż pliki nie znajdują się w ścieżkach fizycznych w zawartości statycznej przechowywanej przez usługę BLOB.</span><span class="sxs-lookup"><span data-stu-id="40e57-298">Razor components and other non-file endpoints don't reside at physical paths in the static content stored by the blob service.</span></span> <span data-ttu-id="40e57-299">Po otrzymaniu żądania dla jednego z tych zasobów, który Blazor powinien zostać obsłużony przez router, błąd *404-nie znaleziono* przez usługę BLOB Service kieruje żądanie do **ścieżki dokumentu błędu**.</span><span class="sxs-lookup"><span data-stu-id="40e57-299">When a request for one of these resources is received that the Blazor router should handle, the *404 - Not Found* error generated by the blob service routes the request to the **Error document path**.</span></span> <span data-ttu-id="40e57-300">`index.html`Obiekt BLOB jest zwracany, a Blazor router ładuje i przetwarza ścieżkę.</span><span class="sxs-lookup"><span data-stu-id="40e57-300">The `index.html` blob is returned, and the Blazor router loads and processes the path.</span></span>

<span data-ttu-id="40e57-301">Jeśli pliki nie są ładowane w czasie wykonywania ze względu na nieodpowiednie typy MIME w `Content-Type` nagłówkach plików, wykonaj jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="40e57-301">If files aren't loaded at runtime due to inappropriate MIME types in the files' `Content-Type` headers, take either of the following actions:</span></span>

* <span data-ttu-id="40e57-302">Skonfiguruj narzędzia do ustawiania prawidłowych typów MIME ( `Content-Type` nagłówkami) podczas wdrażania plików.</span><span class="sxs-lookup"><span data-stu-id="40e57-302">Configure your tooling to set the correct MIME types (`Content-Type` headers) when the files are deployed.</span></span>
* <span data-ttu-id="40e57-303">Zmień typy MIME ( `Content-Type` nagłówki) dla plików po wdrożeniu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-303">Change the MIME types (`Content-Type` headers) for the files after the app is deployed.</span></span>

  <span data-ttu-id="40e57-304">W Eksplorator usługi Storage (Azure Portal) dla każdego pliku:</span><span class="sxs-lookup"><span data-stu-id="40e57-304">In Storage Explorer (Azure portal) for each file:</span></span>
  
  1. <span data-ttu-id="40e57-305">Kliknij prawym przyciskiem myszy plik i wybierz polecenie **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="40e57-305">Right-click the file and select **Properties**.</span></span>
  1. <span data-ttu-id="40e57-306">Ustaw wartość **ContentType** i wybierz przycisk **Zapisz** .</span><span class="sxs-lookup"><span data-stu-id="40e57-306">Set the **ContentType** and select the **Save** button.</span></span>

<span data-ttu-id="40e57-307">Aby uzyskać więcej informacji, zobacz [Obsługa statycznej witryny sieci Web w usłudze Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span><span class="sxs-lookup"><span data-stu-id="40e57-307">For more information, see [Static website hosting in Azure Storage](/azure/storage/blobs/storage-blob-static-website).</span></span>

### <a name="nginx"></a><span data-ttu-id="40e57-308">Nginx</span><span class="sxs-lookup"><span data-stu-id="40e57-308">Nginx</span></span>

<span data-ttu-id="40e57-309">Następujący `nginx.conf` plik jest uproszczony, aby pokazać, jak skonfigurować Nginx do wysyłania `index.html` pliku za każdym razem, gdy nie można znaleźć odpowiedniego pliku na dysku.</span><span class="sxs-lookup"><span data-stu-id="40e57-309">The following `nginx.conf` file is simplified to show how to configure Nginx to send the `index.html` file whenever it can't find a corresponding file on disk.</span></span>

```
events { }
http {
    server {
        listen 80;

        location / {
            root      /usr/share/nginx/html;
            try_files $uri $uri/ /index.html =404;
        }
    }
}
```

<span data-ttu-id="40e57-310">W przypadku ustawienia [limitu szybkości serii Nginx](https://www.nginx.com/blog/rate-limiting-nginx/#bursts) w [`limit_req`](https://nginx.org/docs/http/ngx_http_limit_req_module.html#limit_req) programie Blazor WebAssembly aplikacje mogą wymagać dużej `burst` wartości parametru, aby pomieścić stosunkowo dużą liczbę żądań wykonywanych przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="40e57-310">When setting the [NGINX burst rate limit](https://www.nginx.com/blog/rate-limiting-nginx/#bursts) with [`limit_req`](https://nginx.org/docs/http/ngx_http_limit_req_module.html#limit_req), Blazor WebAssembly apps may require a large `burst` parameter value to accommodate the relatively large number of requests made by an app.</span></span> <span data-ttu-id="40e57-311">Początkowo ustaw wartość na co najmniej 60:</span><span class="sxs-lookup"><span data-stu-id="40e57-311">Initially, set the value to at least 60:</span></span>

```
http {
    server {
        ...

        location / {
            ...

            limit_req zone=one burst=60 nodelay;
        }
    }
}
```

<span data-ttu-id="40e57-312">Zwiększ wartość, jeśli narzędzia deweloperskie przeglądarki lub ruch sieciowy wskazuje, że żądania odbierają kod stanu *niedostępny dla usługi 503* .</span><span class="sxs-lookup"><span data-stu-id="40e57-312">Increase the value if browser developer tools or a network traffic tool indicates that requests are receiving a *503 - Service Unavailable* status code.</span></span>

<span data-ttu-id="40e57-313">Aby uzyskać więcej informacji na temat konfiguracji serwera sieci Web w środowisku produkcyjnym, zobacz [Tworzenie plików konfiguracji Nginx Plus i Nginx](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span><span class="sxs-lookup"><span data-stu-id="40e57-313">For more information on production Nginx web server configuration, see [Creating NGINX Plus and NGINX Configuration Files](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/).</span></span>

### <a name="apache"></a><span data-ttu-id="40e57-314">Apache</span><span class="sxs-lookup"><span data-stu-id="40e57-314">Apache</span></span>

<span data-ttu-id="40e57-315">Aby wdrożyć Blazor WebAssembly aplikację w programie CentOS 7 lub nowszym:</span><span class="sxs-lookup"><span data-stu-id="40e57-315">To deploy a Blazor WebAssembly app to CentOS 7 or later:</span></span>

1. <span data-ttu-id="40e57-316">Utwórz plik konfiguracji Apache.</span><span class="sxs-lookup"><span data-stu-id="40e57-316">Create the Apache configuration file.</span></span> <span data-ttu-id="40e57-317">Poniższy przykład to uproszczony plik konfiguracji ( `blazorapp.config` ):</span><span class="sxs-lookup"><span data-stu-id="40e57-317">The following example is a simplified configuration file (`blazorapp.config`):</span></span>

   ```
   <VirtualHost *:80>
       ServerName www.example.com
       ServerAlias *.example.com

       DocumentRoot "/var/www/blazorapp"
       ErrorDocument 404 /index.html

       AddType application/wasm .wasm
       AddType application/octet-stream .dll
   
       <Directory "/var/www/blazorapp">
           Options -Indexes
           AllowOverride None
       </Directory>

       <IfModule mod_deflate.c>
           AddOutputFilterByType DEFLATE text/css
           AddOutputFilterByType DEFLATE application/javascript
           AddOutputFilterByType DEFLATE text/html
           AddOutputFilterByType DEFLATE application/octet-stream
           AddOutputFilterByType DEFLATE application/wasm
           <IfModule mod_setenvif.c>
           BrowserMatch ^Mozilla/4 gzip-only-text/html
           BrowserMatch ^Mozilla/4.0[678] no-gzip
           BrowserMatch bMSIE !no-gzip !gzip-only-text/html
       </IfModule>
       </IfModule>

       ErrorLog /var/log/httpd/blazorapp-error.log
       CustomLog /var/log/httpd/blazorapp-access.log common
   </VirtualHost>
   ```

1. <span data-ttu-id="40e57-318">Umieść plik konfiguracji Apache w `/etc/httpd/conf.d/` katalogu, który jest domyślnym katalogiem konfiguracji Apache w CentOS 7.</span><span class="sxs-lookup"><span data-stu-id="40e57-318">Place the Apache configuration file into the `/etc/httpd/conf.d/` directory, which is the default Apache configuration directory in CentOS 7.</span></span>

1. <span data-ttu-id="40e57-319">Umieść pliki aplikacji w `/var/www/blazorapp` katalogu (lokalizacja określona `DocumentRoot` w pliku konfiguracyjnym).</span><span class="sxs-lookup"><span data-stu-id="40e57-319">Place the app's files into the `/var/www/blazorapp` directory (the location specified to `DocumentRoot` in the configuration file).</span></span>

1. <span data-ttu-id="40e57-320">Uruchom ponownie usługę Apache.</span><span class="sxs-lookup"><span data-stu-id="40e57-320">Restart the Apache service.</span></span>

<span data-ttu-id="40e57-321">Aby uzyskać więcej informacji, zobacz [`mod_mime`](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) i [`mod_deflate`](https://httpd.apache.org/docs/current/mod/mod_deflate.html) .</span><span class="sxs-lookup"><span data-stu-id="40e57-321">For more information, see [`mod_mime`](https://httpd.apache.org/docs/2.4/mod/mod_mime.html) and [`mod_deflate`](https://httpd.apache.org/docs/current/mod/mod_deflate.html).</span></span>

### <a name="github-pages"></a><span data-ttu-id="40e57-322">Usługa GitHub Pages</span><span class="sxs-lookup"><span data-stu-id="40e57-322">GitHub Pages</span></span>

<span data-ttu-id="40e57-323">Aby obsłużyć ponowne zapisywanie adresów URL, Dodaj `wwwroot/404.html` plik ze skryptem, który obsługuje przekierowywanie żądania do `index.html` strony.</span><span class="sxs-lookup"><span data-stu-id="40e57-323">To handle URL rewrites, add a `wwwroot/404.html` file with a script that handles redirecting the request to the `index.html` page.</span></span> <span data-ttu-id="40e57-324">Aby zapoznać się z przykładem, zobacz [ Blazor repozytorium GitHub SteveSandersonMS/OnGitHubPages](https://github.com/SteveSandersonMS/BlazorOnGitHubPages):</span><span class="sxs-lookup"><span data-stu-id="40e57-324">For an example, see the [SteveSandersonMS/BlazorOnGitHubPages GitHub repository](https://github.com/SteveSandersonMS/BlazorOnGitHubPages):</span></span>

* [`wwwroot/404.html`](https://github.com/SteveSandersonMS/BlazorOnGitHubPages/blob/master/wwwroot/404.html)
* <span data-ttu-id="40e57-325">[Aktywna witryna](https://stevesandersonms.github.io/BlazorOnGitHubPages/))</span><span class="sxs-lookup"><span data-stu-id="40e57-325">[Live site](https://stevesandersonms.github.io/BlazorOnGitHubPages/))</span></span>

<span data-ttu-id="40e57-326">W przypadku korzystania z witryny projektu zamiast witryny organizacji zaktualizuj `<base>` tag w temacie `wwwroot/index.html` .</span><span class="sxs-lookup"><span data-stu-id="40e57-326">When using a project site instead of an organization site, update the `<base>` tag in `wwwroot/index.html`.</span></span> <span data-ttu-id="40e57-327">Ustaw `href` wartość atrybutu na nazwę repozytorium GitHub z końcowym ukośnikiem (na przykład `/my-repository/` ).</span><span class="sxs-lookup"><span data-stu-id="40e57-327">Set the `href` attribute value to the GitHub repository name with a trailing slash (for example, `/my-repository/`).</span></span> <span data-ttu-id="40e57-328">W [ Blazor repozytorium GitHub SteveSandersonMS/OnGitHubPages](https://github.com/SteveSandersonMS/BlazorOnGitHubPages)baza `href` jest aktualizowana przy publikowaniu przez [ `.github/workflows/main.yml` plik konfiguracji](https://github.com/SteveSandersonMS/BlazorOnGitHubPages/blob/master/.github/workflows/main.yml).</span><span class="sxs-lookup"><span data-stu-id="40e57-328">In the [SteveSandersonMS/BlazorOnGitHubPages GitHub repository](https://github.com/SteveSandersonMS/BlazorOnGitHubPages), the base `href` is updated at publish by the [`.github/workflows/main.yml` configuration file](https://github.com/SteveSandersonMS/BlazorOnGitHubPages/blob/master/.github/workflows/main.yml).</span></span>

> [!NOTE]
> <span data-ttu-id="40e57-329">[ Blazor Repozytorium GitHub SteveSandersonMS/OnGitHubPages](https://github.com/SteveSandersonMS/BlazorOnGitHubPages) nie należy do ani nie jest obsługiwane przez platformę .NET Foundation ani firmę Microsoft.</span><span class="sxs-lookup"><span data-stu-id="40e57-329">The [SteveSandersonMS/BlazorOnGitHubPages GitHub repository](https://github.com/SteveSandersonMS/BlazorOnGitHubPages) isn't owned, maintained, or supported by the .NET Foundation or Microsoft.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="40e57-330">Wartości konfiguracji hosta</span><span class="sxs-lookup"><span data-stu-id="40e57-330">Host configuration values</span></span>

<span data-ttu-id="40e57-331">[ Blazor WebAssembly aplikacje](xref:blazor/hosting-models#blazor-webassembly) mogą akceptować następujące wartości konfiguracji hosta jako argumenty wiersza polecenia w czasie wykonywania w środowisku programistycznym.</span><span class="sxs-lookup"><span data-stu-id="40e57-331">[Blazor WebAssembly apps](xref:blazor/hosting-models#blazor-webassembly) can accept the following host configuration values as command-line arguments at runtime in the development environment.</span></span>

### <a name="content-root"></a><span data-ttu-id="40e57-332">Katalog główny zawartości</span><span class="sxs-lookup"><span data-stu-id="40e57-332">Content root</span></span>

<span data-ttu-id="40e57-333">`--contentroot`Argument ustawia ścieżkę bezwzględną do katalogu, który zawiera pliki zawartości aplikacji ([katalog główny zawartości](xref:fundamentals/index#content-root)).</span><span class="sxs-lookup"><span data-stu-id="40e57-333">The `--contentroot` argument sets the absolute path to the directory that contains the app's content files ([content root](xref:fundamentals/index#content-root)).</span></span> <span data-ttu-id="40e57-334">W poniższych przykładach `/content-root-path` jest ścieżką katalogu głównego zawartości aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-334">In the following examples, `/content-root-path` is the app's content root path.</span></span>

* <span data-ttu-id="40e57-335">Przekaż argument podczas lokalnego uruchamiania aplikacji w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="40e57-335">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="40e57-336">W katalogu aplikacji wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="40e57-336">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --contentroot=/content-root-path
  ```

* <span data-ttu-id="40e57-337">Dodaj wpis do `launchSettings.json` pliku aplikacji w profilu **IIS Express** .</span><span class="sxs-lookup"><span data-stu-id="40e57-337">Add an entry to the app's `launchSettings.json` file in the **IIS Express** profile.</span></span> <span data-ttu-id="40e57-338">To ustawienie jest używane, gdy aplikacja jest uruchamiana z debugerem programu Visual Studio i z wiersza polecenia z `dotnet run` .</span><span class="sxs-lookup"><span data-stu-id="40e57-338">This setting is used when the app is run with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--contentroot=/content-root-path"
  ```

* <span data-ttu-id="40e57-339">W programie Visual Studio Określ argument w **właściwościach**  >  **Debuguj**  >  **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="40e57-339">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="40e57-340">Ustawienie argumentu na stronie właściwości programu Visual Studio powoduje dodanie argumentu do `launchSettings.json` pliku.</span><span class="sxs-lookup"><span data-stu-id="40e57-340">Setting the argument in the Visual Studio property page adds the argument to the `launchSettings.json` file.</span></span>

  ```console
  --contentroot=/content-root-path
  ```

### <a name="path-base"></a><span data-ttu-id="40e57-341">Baza ścieżki</span><span class="sxs-lookup"><span data-stu-id="40e57-341">Path base</span></span>

<span data-ttu-id="40e57-342">`--pathbase`Argument ustawia ścieżkę bazową aplikacji dla aplikacji uruchamianej lokalnie z niegłówną względną ścieżką URL ( `<base>` tag `href` jest ustawiony na ścieżkę inną niż `/` w przypadku przemieszczania i produkcji).</span><span class="sxs-lookup"><span data-stu-id="40e57-342">The `--pathbase` argument sets the app base path for an app run locally with a non-root relative URL path (the `<base>` tag `href` is set to a path other than `/` for staging and production).</span></span> <span data-ttu-id="40e57-343">W poniższych przykładach `/relative-URL-path` jest podstawą ścieżki aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-343">In the following examples, `/relative-URL-path` is the app's path base.</span></span> <span data-ttu-id="40e57-344">Aby uzyskać więcej informacji, zobacz [Ścieżka podstawowa aplikacji](xref:blazor/host-and-deploy/index#app-base-path).</span><span class="sxs-lookup"><span data-stu-id="40e57-344">For more information, see [App base path](xref:blazor/host-and-deploy/index#app-base-path).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="40e57-345">W przeciwieństwie do ścieżki przekazanej do `href` `<base>` tagu, nie dodawaj końcowego ukośnika ( `/` ) podczas przekazywania `--pathbase` wartości argumentu.</span><span class="sxs-lookup"><span data-stu-id="40e57-345">Unlike the path provided to `href` of the `<base>` tag, don't include a trailing slash (`/`) when passing the `--pathbase` argument value.</span></span> <span data-ttu-id="40e57-346">Jeśli ścieżka podstawowa aplikacji jest podana w `<base>` tagu jako `<base href="/CoolApp/">` (zawiera końcowy ukośnik), należy przekazać wartość argumentu wiersza polecenia jako `--pathbase=/CoolApp` (bez ukośnika na końcu).</span><span class="sxs-lookup"><span data-stu-id="40e57-346">If the app base path is provided in the `<base>` tag as `<base href="/CoolApp/">` (includes a trailing slash), pass the command-line argument value as `--pathbase=/CoolApp` (no trailing slash).</span></span>

* <span data-ttu-id="40e57-347">Przekaż argument podczas lokalnego uruchamiania aplikacji w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="40e57-347">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="40e57-348">W katalogu aplikacji wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="40e57-348">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --pathbase=/relative-URL-path
  ```

* <span data-ttu-id="40e57-349">Dodaj wpis do `launchSettings.json` pliku aplikacji w profilu **IIS Express** .</span><span class="sxs-lookup"><span data-stu-id="40e57-349">Add an entry to the app's `launchSettings.json` file in the **IIS Express** profile.</span></span> <span data-ttu-id="40e57-350">To ustawienie jest używane podczas uruchamiania aplikacji za pomocą debugera programu Visual Studio i z wiersza polecenia w programie `dotnet run` .</span><span class="sxs-lookup"><span data-stu-id="40e57-350">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--pathbase=/relative-URL-path"
  ```

* <span data-ttu-id="40e57-351">W programie Visual Studio Określ argument w **właściwościach**  >  **Debuguj**  >  **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="40e57-351">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="40e57-352">Ustawienie argumentu na stronie właściwości programu Visual Studio powoduje dodanie argumentu do `launchSettings.json` pliku.</span><span class="sxs-lookup"><span data-stu-id="40e57-352">Setting the argument in the Visual Studio property page adds the argument to the `launchSettings.json` file.</span></span>

  ```console
  --pathbase=/relative-URL-path
  ```

### <a name="urls"></a><span data-ttu-id="40e57-353">Adresy URL</span><span class="sxs-lookup"><span data-stu-id="40e57-353">URLs</span></span>

<span data-ttu-id="40e57-354">`--urls`Argument ustawia adresy IP lub adresy hosta z portami i protokołami, aby nasłuchiwać żądań.</span><span class="sxs-lookup"><span data-stu-id="40e57-354">The `--urls` argument sets the IP addresses or host addresses with ports and protocols to listen on for requests.</span></span>

* <span data-ttu-id="40e57-355">Przekaż argument podczas lokalnego uruchamiania aplikacji w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="40e57-355">Pass the argument when running the app locally at a command prompt.</span></span> <span data-ttu-id="40e57-356">W katalogu aplikacji wykonaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="40e57-356">From the app's directory, execute:</span></span>

  ```dotnetcli
  dotnet run --urls=http://127.0.0.1:0
  ```

* <span data-ttu-id="40e57-357">Dodaj wpis do `launchSettings.json` pliku aplikacji w profilu **IIS Express** .</span><span class="sxs-lookup"><span data-stu-id="40e57-357">Add an entry to the app's `launchSettings.json` file in the **IIS Express** profile.</span></span> <span data-ttu-id="40e57-358">To ustawienie jest używane podczas uruchamiania aplikacji za pomocą debugera programu Visual Studio i z wiersza polecenia w programie `dotnet run` .</span><span class="sxs-lookup"><span data-stu-id="40e57-358">This setting is used when running the app with the Visual Studio Debugger and from a command prompt with `dotnet run`.</span></span>

  ```json
  "commandLineArgs": "--urls=http://127.0.0.1:0"
  ```

* <span data-ttu-id="40e57-359">W programie Visual Studio Określ argument w **właściwościach**  >  **Debuguj**  >  **argumenty aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="40e57-359">In Visual Studio, specify the argument in **Properties** > **Debug** > **Application arguments**.</span></span> <span data-ttu-id="40e57-360">Ustawienie argumentu na stronie właściwości programu Visual Studio powoduje dodanie argumentu do `launchSettings.json` pliku.</span><span class="sxs-lookup"><span data-stu-id="40e57-360">Setting the argument in the Visual Studio property page adds the argument to the `launchSettings.json` file.</span></span>

  ```console
  --urls=http://127.0.0.1:0
  ```

::: moniker range=">= aspnetcore-5.0"

## <a name="configure-the-trimmer"></a><span data-ttu-id="40e57-361">Konfigurowanie elementu przycinającego</span><span class="sxs-lookup"><span data-stu-id="40e57-361">Configure the Trimmer</span></span>

<span data-ttu-id="40e57-362">Blazor wykonuje przycinanie języka pośredniego (IL) dla każdej kompilacji wydania, aby usunąć niepotrzebny kod IL z zestawów wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="40e57-362">Blazor performs Intermediate Language (IL) trimming on each Release build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="40e57-363">Aby uzyskać więcej informacji, zobacz <xref:blazor/host-and-deploy/configure-trimmer>.</span><span class="sxs-lookup"><span data-stu-id="40e57-363">For more information, see <xref:blazor/host-and-deploy/configure-trimmer>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-5.0"

## <a name="configure-the-linker"></a><span data-ttu-id="40e57-364">Konfigurowanie konsolidatora</span><span class="sxs-lookup"><span data-stu-id="40e57-364">Configure the Linker</span></span>

<span data-ttu-id="40e57-365">Blazor wykonuje konsolidację języka pośredniego (IL) dla każdej kompilacji wydania, aby usunąć niepotrzebny kod IL z zestawów wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="40e57-365">Blazor performs Intermediate Language (IL) linking on each Release build to remove unnecessary IL from the output assemblies.</span></span> <span data-ttu-id="40e57-366">Aby uzyskać więcej informacji, zobacz <xref:blazor/host-and-deploy/configure-linker>.</span><span class="sxs-lookup"><span data-stu-id="40e57-366">For more information, see <xref:blazor/host-and-deploy/configure-linker>.</span></span>

::: moniker-end

## <a name="custom-boot-resource-loading"></a><span data-ttu-id="40e57-367">Ładowanie niestandardowego zasobu rozruchowego</span><span class="sxs-lookup"><span data-stu-id="40e57-367">Custom boot resource loading</span></span>

<span data-ttu-id="40e57-368">Blazor WebAssemblyAplikacja może zostać zainicjowana przy użyciu `loadBootResource` funkcji w celu zastąpienia wbudowanego mechanizmu ładowania zasobów rozruchowego.</span><span class="sxs-lookup"><span data-stu-id="40e57-368">A Blazor WebAssembly app can be initialized with the `loadBootResource` function to override the built-in boot resource loading mechanism.</span></span> <span data-ttu-id="40e57-369">Należy używać `loadBootResource` w następujących scenariuszach:</span><span class="sxs-lookup"><span data-stu-id="40e57-369">Use `loadBootResource` for the following scenarios:</span></span>

* <span data-ttu-id="40e57-370">Zezwalaj użytkownikom na ładowanie zasobów statycznych, takich jak dane dotyczące strefy czasowej lub `dotnet.wasm` z sieci CDN.</span><span class="sxs-lookup"><span data-stu-id="40e57-370">Allow users to load static resources, such as timezone data or `dotnet.wasm` from a CDN.</span></span>
* <span data-ttu-id="40e57-371">Załaduj skompresowane zestawy za pomocą żądania HTTP i zdekompresuj je na kliencie dla hostów, które nie obsługują pobierania skompresowanej zawartości z serwera.</span><span class="sxs-lookup"><span data-stu-id="40e57-371">Load compressed assemblies using an HTTP request and decompress them on the client for hosts that don't support fetching compressed contents from the server.</span></span>
* <span data-ttu-id="40e57-372">Aliasowanie zasobów do innej nazwy przez przekierowanie każdego `fetch` żądania do nowej nazwy.</span><span class="sxs-lookup"><span data-stu-id="40e57-372">Alias resources to a different name by redirecting each `fetch` request to a new name.</span></span>

<span data-ttu-id="40e57-373">`loadBootResource` Parametry znajdują się w poniższej tabeli.</span><span class="sxs-lookup"><span data-stu-id="40e57-373">`loadBootResource` parameters appear in the following table.</span></span>

| <span data-ttu-id="40e57-374">Parametr</span><span class="sxs-lookup"><span data-stu-id="40e57-374">Parameter</span></span>    | <span data-ttu-id="40e57-375">Opis</span><span class="sxs-lookup"><span data-stu-id="40e57-375">Description</span></span> |
| ------------ | ----------- |
| `type`       | <span data-ttu-id="40e57-376">Typ zasobu.</span><span class="sxs-lookup"><span data-stu-id="40e57-376">The type of the resource.</span></span> <span data-ttu-id="40e57-377">Typy Permissable: `assembly` , `pdb` , `dotnetjs` , `dotnetwasm` , `timezonedata`</span><span class="sxs-lookup"><span data-stu-id="40e57-377">Permissable types: `assembly`, `pdb`, `dotnetjs`, `dotnetwasm`, `timezonedata`</span></span> |
| `name`       | <span data-ttu-id="40e57-378">Nazwa zasobu.</span><span class="sxs-lookup"><span data-stu-id="40e57-378">The name of the resource.</span></span> |
| `defaultUri` | <span data-ttu-id="40e57-379">Względny lub bezwzględny identyfikator URI zasobu.</span><span class="sxs-lookup"><span data-stu-id="40e57-379">The relative or absolute URI of the resource.</span></span> |
| `integrity`  | <span data-ttu-id="40e57-380">Ciąg integralności reprezentujący oczekiwaną zawartość w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="40e57-380">The integrity string representing the expected content in the response.</span></span> |

<span data-ttu-id="40e57-381">`loadBootResource` zwraca jedną z następujących wartości, aby zastąpić proces ładowania:</span><span class="sxs-lookup"><span data-stu-id="40e57-381">`loadBootResource` returns any of the following to override the loading process:</span></span>

* <span data-ttu-id="40e57-382">Ciąg identyfikatora URI.</span><span class="sxs-lookup"><span data-stu-id="40e57-382">URI string.</span></span> <span data-ttu-id="40e57-383">W poniższym przykładzie ( `wwwroot/index.html` ) następujące pliki są obsługiwane z sieci CDN w `https://my-awesome-cdn.com/` :</span><span class="sxs-lookup"><span data-stu-id="40e57-383">In the following example (`wwwroot/index.html`), the following files are served from a CDN at `https://my-awesome-cdn.com/`:</span></span>

  * `dotnet.*.js`
  * `dotnet.wasm`
  * <span data-ttu-id="40e57-384">Dane strefy czasowej</span><span class="sxs-lookup"><span data-stu-id="40e57-384">Timezone data</span></span>

  ```html
  ...

  <script src="_framework/blazor.webassembly.js" autostart="false"></script>
  <script>
    Blazor.start({
      loadBootResource: function (type, name, defaultUri, integrity) {
        console.log(`Loading: '${type}', '${name}', '${defaultUri}', '${integrity}'`);
        switch (type) {
          case 'dotnetjs':
          case 'dotnetwasm':
          case 'timezonedata':
            return `https://my-awesome-cdn.com/blazorwebassembly/3.2.0/${name}`;
        }
      }
    });
  </script>
  ```

* <span data-ttu-id="40e57-385">`Promise<Response>`.</span><span class="sxs-lookup"><span data-stu-id="40e57-385">`Promise<Response>`.</span></span> <span data-ttu-id="40e57-386">Przekaż `integrity` parametr w nagłówku, aby zachować domyślne zachowanie sprawdzania integralności.</span><span class="sxs-lookup"><span data-stu-id="40e57-386">Pass the `integrity` parameter in a header to retain the default integrity-checking behavior.</span></span>

  <span data-ttu-id="40e57-387">Poniższy przykład ( `wwwroot/index.html` ) dodaje niestandardowy nagłówek HTTP do żądań wychodzących i przekazuje `integrity` parametr do `fetch` wywołania:</span><span class="sxs-lookup"><span data-stu-id="40e57-387">The following example (`wwwroot/index.html`) adds a custom HTTP header to the outbound requests and passes the `integrity` parameter through to the `fetch` call:</span></span>
  
  ```html
  <script src="_framework/blazor.webassembly.js" autostart="false"></script>
  <script>
    Blazor.start({
      loadBootResource: function (type, name, defaultUri, integrity) {
        return fetch(defaultUri, { 
          cache: 'no-cache',
          integrity: integrity,
          headers: { 'MyCustomHeader': 'My custom value' }
        });
      }
    });
  </script>
  ```

* <span data-ttu-id="40e57-388">`null`/`undefined`, które powoduje domyślne zachowanie podczas ładowania.</span><span class="sxs-lookup"><span data-stu-id="40e57-388">`null`/`undefined`, which results in the default loading behavior.</span></span>

<span data-ttu-id="40e57-389">Źródła zewnętrzne muszą zwracać wymagane nagłówki CORS dla przeglądarek, aby umożliwić ładowanie zasobów między źródłami.</span><span class="sxs-lookup"><span data-stu-id="40e57-389">External sources must return the required CORS headers for browsers to allow the cross-origin resource loading.</span></span> <span data-ttu-id="40e57-390">Sieci CDN zwykle domyślnie udostępnia wymagane nagłówki.</span><span class="sxs-lookup"><span data-stu-id="40e57-390">CDNs usually provide the required headers by default.</span></span>

<span data-ttu-id="40e57-391">Wystarczy określić typy zachowań niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="40e57-391">You only need to specify types for custom behaviors.</span></span> <span data-ttu-id="40e57-392">Typy, które nie zostały określone do, `loadBootResource` są ładowane przez platformę na ich domyślne zachowania ładowania.</span><span class="sxs-lookup"><span data-stu-id="40e57-392">Types not specified to `loadBootResource` are loaded by the framework per their default loading behaviors.</span></span>

## <a name="change-the-filename-extension-of-dll-files"></a><span data-ttu-id="40e57-393">Zmień rozszerzenie nazwy pliku DLL</span><span class="sxs-lookup"><span data-stu-id="40e57-393">Change the filename extension of DLL files</span></span>

<span data-ttu-id="40e57-394">Jeśli potrzebujesz zmienić rozszerzenia nazw plików opublikowanych w aplikacji `.dll` , postępuj zgodnie ze wskazówkami w tej sekcji.</span><span class="sxs-lookup"><span data-stu-id="40e57-394">In case you have a need to change the filename extensions of the app's published `.dll` files, follow the guidance in this section.</span></span>

<span data-ttu-id="40e57-395">Po opublikowaniu aplikacji Użyj skryptu powłoki lub potoku kompilacji DevOps, aby zmienić nazwy `.dll` plików, aby użyć innego rozszerzenia pliku.</span><span class="sxs-lookup"><span data-stu-id="40e57-395">After publishing the app, use a shell script or DevOps build pipeline to rename `.dll` files to use a different file extension.</span></span> <span data-ttu-id="40e57-396">Docelowo `.dll` pliki w `wwwroot` katalogu opublikowanych danych wyjściowych aplikacji (na przykład `{CONTENT ROOT}/bin/Release/netstandard2.1/publish/wwwroot` ).</span><span class="sxs-lookup"><span data-stu-id="40e57-396">Target the `.dll` files in the `wwwroot` directory of the app's published output (for example, `{CONTENT ROOT}/bin/Release/netstandard2.1/publish/wwwroot`).</span></span>

<span data-ttu-id="40e57-397">W poniższych przykładach `.dll` nazwy plików są zmieniane, aby użyć `.bin` rozszerzenia pliku.</span><span class="sxs-lookup"><span data-stu-id="40e57-397">In the following examples, `.dll` files are renamed to use the `.bin` file extension.</span></span>

<span data-ttu-id="40e57-398">W systemie Windows:</span><span class="sxs-lookup"><span data-stu-id="40e57-398">On Windows:</span></span>

```powershell
dir .\_framework\_bin | rename-item -NewName { $_.name -replace ".dll\b",".bin" }
((Get-Content .\_framework\blazor.boot.json -Raw) -replace '.dll"','.bin"') | Set-Content .\_framework\blazor.boot.json
```

<span data-ttu-id="40e57-399">Jeśli zasoby procesu roboczego usługi są również używane, Dodaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="40e57-399">If service worker assets are also in use, add the following command:</span></span>

```powershell
((Get-Content .\service-worker-assets.js -Raw) -replace '.dll"','.bin"') | Set-Content .\service-worker-assets.js
```

<span data-ttu-id="40e57-400">W systemie Linux lub macOS:</span><span class="sxs-lookup"><span data-stu-id="40e57-400">On Linux or macOS:</span></span>

```console
for f in _framework/_bin/*; do mv "$f" "`echo $f | sed -e 's/\.dll/.bin/g'`"; done
sed -i 's/\.dll"/.bin"/g' _framework/blazor.boot.json
```

<span data-ttu-id="40e57-401">Jeśli zasoby procesu roboczego usługi są również używane, Dodaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="40e57-401">If service worker assets are also in use, add the following command:</span></span>

```console
sed -i 's/\.dll"/.bin"/g' service-worker-assets.js
```
   
<span data-ttu-id="40e57-402">Aby użyć innego rozszerzenia pliku niż `.bin` , Zastąp `.bin` w poprzednich poleceniach.</span><span class="sxs-lookup"><span data-stu-id="40e57-402">To use a different file extension than `.bin`, replace `.bin` in the preceding commands.</span></span>

<span data-ttu-id="40e57-403">Aby rozwiązać skompresowane `blazor.boot.json.gz` i `blazor.boot.json.br` pliki, należy zastosować jedną z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="40e57-403">To address the compressed `blazor.boot.json.gz` and `blazor.boot.json.br` files, adopt either of the following approaches:</span></span>

* <span data-ttu-id="40e57-404">Usuń skompresowane `blazor.boot.json.gz` i `blazor.boot.json.br` pliki.</span><span class="sxs-lookup"><span data-stu-id="40e57-404">Remove the compressed `blazor.boot.json.gz` and `blazor.boot.json.br` files.</span></span> <span data-ttu-id="40e57-405">Kompresja jest wyłączona w tym podejściu.</span><span class="sxs-lookup"><span data-stu-id="40e57-405">Compression is disabled with this approach.</span></span>
* <span data-ttu-id="40e57-406">Rekompresuj zaktualizowany `blazor.boot.json` plik.</span><span class="sxs-lookup"><span data-stu-id="40e57-406">Recompress the updated `blazor.boot.json` file.</span></span>

<span data-ttu-id="40e57-407">Powyższe wskazówki stosuje się również w przypadku używania zasobów roboczych usługi.</span><span class="sxs-lookup"><span data-stu-id="40e57-407">The preceding guidance also applies when service worker assets are in use.</span></span> <span data-ttu-id="40e57-408">Usuń lub przekompresuj `wwwroot/service-worker-assets.js.br` i `wwwroot/service-worker-assets.js.gz` .</span><span class="sxs-lookup"><span data-stu-id="40e57-408">Remove or recompress `wwwroot/service-worker-assets.js.br` and `wwwroot/service-worker-assets.js.gz`.</span></span> <span data-ttu-id="40e57-409">W przeciwnym razie sprawdzanie integralności plików nie powiedzie się w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="40e57-409">Otherwise, file integrity checks fail in the browser.</span></span>

<span data-ttu-id="40e57-410">Poniższy przykład systemu Windows używa skryptu programu PowerShell znajdującego się w katalogu głównym projektu.</span><span class="sxs-lookup"><span data-stu-id="40e57-410">The following Windows example uses a PowerShell script placed at the root of the project.</span></span>

<span data-ttu-id="40e57-411">`ChangeDLLExtensions.ps1:`:</span><span class="sxs-lookup"><span data-stu-id="40e57-411">`ChangeDLLExtensions.ps1:`:</span></span>

```powershell
param([string]$filepath,[string]$tfm)
dir $filepath\bin\Release\$tfm\wwwroot\_framework\_bin | rename-item -NewName { $_.name -replace ".dll\b",".bin" }
((Get-Content $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json -Raw) -replace '.dll"','.bin"') | Set-Content $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json
Remove-Item $filepath\bin\Release\$tfm\wwwroot\_framework\blazor.boot.json.gz
```

<span data-ttu-id="40e57-412">Jeśli zasoby procesu roboczego usługi są również używane, Dodaj następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="40e57-412">If service worker assets are also in use, add the following command:</span></span>

```powershell
((Get-Content $filepath\bin\Release\$tfm\wwwroot\service-worker-assets.js -Raw) -replace '.dll"','.bin"') | Set-Content $filepath\bin\Release\$tfm\wwwroot\service-worker-assets.js
```

<span data-ttu-id="40e57-413">W pliku projektu skrypt jest uruchamiany po opublikowaniu aplikacji:</span><span class="sxs-lookup"><span data-stu-id="40e57-413">In the project file, the script is run after publishing the app:</span></span>

```xml
<Target Name="ChangeDLLFileExtensions" AfterTargets="Publish" Condition="'$(Configuration)'=='Release'">
  <Exec Command="powershell.exe -command &quot;&amp; { .\ChangeDLLExtensions.ps1 '$(SolutionDir)' '$(TargetFramework)'}&quot;" />
</Target>
```

> [!NOTE]
> <span data-ttu-id="40e57-414">Podczas zmiany nazwy i opóźnionego ładowania tych samych zestawów zobacz wskazówki w temacie <xref:blazor/webassembly-lazy-load-assemblies#onnavigateasync-events-and-renamed-assembly-files> .</span><span class="sxs-lookup"><span data-stu-id="40e57-414">When renaming and lazy loading the same assemblies, see the guidance in <xref:blazor/webassembly-lazy-load-assemblies#onnavigateasync-events-and-renamed-assembly-files>.</span></span>

## <a name="resolve-integrity-check-failures"></a><span data-ttu-id="40e57-415">Rozwiązywanie błędów sprawdzania integralności</span><span class="sxs-lookup"><span data-stu-id="40e57-415">Resolve integrity check failures</span></span>

<span data-ttu-id="40e57-416">Po Blazor WebAssembly pobraniu plików startowych aplikacji nakazuje ona przeprowadzenie kontroli integralności odpowiedzi w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="40e57-416">When Blazor WebAssembly downloads an app's startup files, it instructs the browser to perform integrity checks on the responses.</span></span> <span data-ttu-id="40e57-417">Używa informacji w pliku, `blazor.boot.json` Aby określić oczekiwane wartości skrótu SHA-256 dla `.dll` , `.wasm` i innych plików.</span><span class="sxs-lookup"><span data-stu-id="40e57-417">It uses information in the `blazor.boot.json` file to specify the expected SHA-256 hash values for `.dll`, `.wasm`, and other files.</span></span> <span data-ttu-id="40e57-418">Jest to korzystne z następujących powodów:</span><span class="sxs-lookup"><span data-stu-id="40e57-418">This is beneficial for the following reasons:</span></span>

* <span data-ttu-id="40e57-419">Gwarantuje to, że nie ma ryzyka ładowania niespójnego zestawu plików, na przykład jeśli nowe wdrożenie jest stosowane do serwera sieci Web, podczas gdy użytkownik jest w trakcie procesu pobierania plików aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-419">It ensures you don't risk loading an inconsistent set of files, for example if a new deployment is applied to your web server while the user is in the process of downloading the application files.</span></span> <span data-ttu-id="40e57-420">Niespójne pliki mogą prowadzić do niezdefiniowanego zachowania.</span><span class="sxs-lookup"><span data-stu-id="40e57-420">Inconsistent files could lead to undefined behavior.</span></span>
* <span data-ttu-id="40e57-421">Gwarantuje to, że przeglądarka użytkownika nigdy nie buforuje niespójnych lub nieprawidłowych odpowiedzi, co może uniemożliwić ich uruchomienie aplikacji nawet w przypadku ręcznego odświeżenia strony.</span><span class="sxs-lookup"><span data-stu-id="40e57-421">It ensures the user's browser never caches inconsistent or invalid responses, which could prevent them from starting the app even if they manually refresh the page.</span></span>
* <span data-ttu-id="40e57-422">Pozwala ona bezpiecznie na buforowanie odpowiedzi i nie sprawdza, czy zmiany po stronie serwera są zmieniane, dopóki nie zmienią się oczekiwanych skrótów SHA-256, więc kolejne obciążenia strony obejmują mniejszą liczbę żądań i są znacznie szybsze.</span><span class="sxs-lookup"><span data-stu-id="40e57-422">It makes it safe to cache the responses and not even check for server-side changes until the expected SHA-256 hashes themselves change, so subsequent page loads involve fewer requests and complete much faster.</span></span>

<span data-ttu-id="40e57-423">Jeśli serwer sieci Web zwróci odpowiedzi, które nie pasują do oczekiwanych skrótów SHA-256, zobaczysz błąd podobny do poniższego, pojawia się w konsoli dewelopera przeglądarki:</span><span class="sxs-lookup"><span data-stu-id="40e57-423">If your web server returns responses that don't match the expected SHA-256 hashes, you will see an error similar to the following appear in the browser's developer console:</span></span>

> <span data-ttu-id="40e57-424">Nie można znaleźć prawidłowego skrótu w atrybucie "Integrity" dla zasobu " https://myapp.example.com/\_framework/My BlazorApp.dll" z obliczoną integralnością SHA-256 "IIa70iwvmEg5WiDV17OpQ5eCztNYqL186J56852RpJY =".</span><span class="sxs-lookup"><span data-stu-id="40e57-424">Failed to find a valid digest in the 'integrity' attribute for resource 'https://myapp.example.com/\_framework/MyBlazorApp.dll' with computed SHA-256 integrity 'IIa70iwvmEg5WiDV17OpQ5eCztNYqL186J56852RpJY='.</span></span> <span data-ttu-id="40e57-425">Zasób został zablokowany.</span><span class="sxs-lookup"><span data-stu-id="40e57-425">The resource has been blocked.</span></span>

<span data-ttu-id="40e57-426">W większości przypadków *nie* jest to problem ze sprawdzaniem integralności.</span><span class="sxs-lookup"><span data-stu-id="40e57-426">In most cases, this is *not* a problem with integrity checking itself.</span></span> <span data-ttu-id="40e57-427">Zamiast tego oznacza to, że występuje jakiś inny problem, a Kontrola integralności ostrzega o tym, że ten problem wystąpi.</span><span class="sxs-lookup"><span data-stu-id="40e57-427">Instead, it means there is some other problem, and the integrity check is warning you about that other problem.</span></span>

### <a name="diagnosing-integrity-problems"></a><span data-ttu-id="40e57-428">Diagnozowanie problemów z integralnością</span><span class="sxs-lookup"><span data-stu-id="40e57-428">Diagnosing integrity problems</span></span>

<span data-ttu-id="40e57-429">Po skompilowaniu aplikacji wygenerowany `blazor.boot.json` manifest opisuje skróty SHA-256 zasobów rozruchowych (na przykład, `.dll` `.wasm` i innych plików) w momencie wygenerowania danych wyjściowych kompilacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-429">When an app is built, the generated `blazor.boot.json` manifest describes the SHA-256 hashes of your boot resources (for example, `.dll`, `.wasm`, and other files) at the time that the build output is produced.</span></span> <span data-ttu-id="40e57-430">Sprawdzanie integralności przebiega tak długo, jak skróty SHA-256 `blazor.boot.json` są zgodne z plikami dostarczonymi do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="40e57-430">The integrity check passes as long as the SHA-256 hashes in `blazor.boot.json` match the files delivered to the browser.</span></span>

<span data-ttu-id="40e57-431">Najczęstsze przyczyny tego niepowodzenia to:</span><span class="sxs-lookup"><span data-stu-id="40e57-431">Common reasons why this fails are:</span></span>

 * <span data-ttu-id="40e57-432">Odpowiedź serwera sieci Web jest błędem (na przykład *404 — nie można znaleźć* lub *500 — wewnętrzny błąd serwera*) zamiast pliku, którego żądała przeglądarka.</span><span class="sxs-lookup"><span data-stu-id="40e57-432">The web server's response is an error (for example, a *404 - Not Found* or a *500 - Internal Server Error*) instead of the file the browser requested.</span></span> <span data-ttu-id="40e57-433">Jest on raportowany przez przeglądarkę jako błąd sprawdzania integralności, a nie jako błąd odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="40e57-433">This is reported by the browser as an integrity check failure and not as a response failure.</span></span>
 * <span data-ttu-id="40e57-434">Coś zmieniło zawartość plików między kompilacją a dostarczeniem plików do przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="40e57-434">Something has changed the contents of the files between the build and delivery of the files to the browser.</span></span> <span data-ttu-id="40e57-435">Może się tak zdarzyć:</span><span class="sxs-lookup"><span data-stu-id="40e57-435">This might happen:</span></span>
   * <span data-ttu-id="40e57-436">Jeśli narzędzia kompilacji lub kompilacje ręcznie modyfikują dane wyjściowe kompilacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-436">If you or build tools manually modify the build output.</span></span>
   * <span data-ttu-id="40e57-437">Jeśli jakiś aspekt procesu wdrażania zmodyfikował pliki.</span><span class="sxs-lookup"><span data-stu-id="40e57-437">If some aspect of the deployment process modified the files.</span></span> <span data-ttu-id="40e57-438">Na przykład jeśli korzystasz z mechanizmu wdrożenia opartego na usłudze git, weź pod uwagę, że git w sposób przezroczysty konwertuje końce wierszy w stylu systemu Windows do końca wiersza w stylu Unix, jeśli zatwierdzisz pliki w systemie Windows i wyewidencjonujesz je w systemie Linux.</span><span class="sxs-lookup"><span data-stu-id="40e57-438">For example if you use a Git-based deployment mechanism, bear in mind that Git transparently converts Windows-style line endings to Unix-style line endings if you commit files on Windows and check them out on Linux.</span></span> <span data-ttu-id="40e57-439">Zmiana końców wierszy plików zmienia wartości skrótów SHA-256.</span><span class="sxs-lookup"><span data-stu-id="40e57-439">Changing file line endings change the SHA-256 hashes.</span></span> <span data-ttu-id="40e57-440">Aby uniknąć tego problemu, rozważ [użycie `.gitattributes` programu do traktowania artefaktów kompilacji jako `binary` plików](https://git-scm.com/book/en/v2/Customizing-Git-Git-Attributes).</span><span class="sxs-lookup"><span data-stu-id="40e57-440">To avoid this problem, consider [using `.gitattributes` to treat build artifacts as `binary` files](https://git-scm.com/book/en/v2/Customizing-Git-Git-Attributes).</span></span>
   * <span data-ttu-id="40e57-441">Serwer sieci Web modyfikuje zawartość pliku w ramach ich obsługi.</span><span class="sxs-lookup"><span data-stu-id="40e57-441">The web server modifies the file contents as part of serving them.</span></span> <span data-ttu-id="40e57-442">Na przykład niektóre sieci dystrybucji zawartości (sieci CDN) automatycznie próbują [zminifikować](xref:client-side/bundling-and-minification#minification) kod HTML, modyfikując go.</span><span class="sxs-lookup"><span data-stu-id="40e57-442">For example, some content distribution networks (CDNs) automatically attempt to [minify](xref:client-side/bundling-and-minification#minification) HTML, thereby modifying it.</span></span> <span data-ttu-id="40e57-443">Może być konieczne wyłączenie takich funkcji.</span><span class="sxs-lookup"><span data-stu-id="40e57-443">You may need to disable such features.</span></span>

<span data-ttu-id="40e57-444">Aby zdiagnozować, które z nich mają zastosowanie w Twoim przypadku:</span><span class="sxs-lookup"><span data-stu-id="40e57-444">To diagnose which of these applies in your case:</span></span>

 1. <span data-ttu-id="40e57-445">Zwróć uwagę, który plik wyzwala błąd, odczytując komunikat o błędzie.</span><span class="sxs-lookup"><span data-stu-id="40e57-445">Note which file is triggering the error by reading the error message.</span></span>
 1. <span data-ttu-id="40e57-446">Otwórz narzędzia deweloperskie w przeglądarce i sprawdź kartę *Sieć* . W razie potrzeby Załaduj ponownie stronę, aby zobaczyć listę żądań i odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="40e57-446">Open your browser's developer tools and look in the *Network* tab. If necessary, reload the page to see the list of requests and responses.</span></span> <span data-ttu-id="40e57-447">Znajdź plik wyzwalający błąd na tej liście.</span><span class="sxs-lookup"><span data-stu-id="40e57-447">Find the file that is triggering the error in that list.</span></span>
 1. <span data-ttu-id="40e57-448">Sprawdź kod stanu HTTP w odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="40e57-448">Check the HTTP status code in the response.</span></span> <span data-ttu-id="40e57-449">Jeśli serwer zwróci coś innego niż *200-OK* (lub inny kod stanu 2xx), wystąpił problem po stronie serwera do zdiagnozowania.</span><span class="sxs-lookup"><span data-stu-id="40e57-449">If the server returns anything other than *200 - OK* (or another 2xx status code), then you have a server-side problem to diagnose.</span></span> <span data-ttu-id="40e57-450">Na przykład kod stanu 403 oznacza problem z autoryzacją, natomiast kod stanu 500 oznacza, że serwer kończy się nieokreślonym sposobem.</span><span class="sxs-lookup"><span data-stu-id="40e57-450">For example, status code 403 means there's an authorization problem, whereas status code 500 means the server is failing in an unspecified manner.</span></span> <span data-ttu-id="40e57-451">Zapoznaj się z dziennikami po stronie serwera, aby zdiagnozować i naprawić aplikację.</span><span class="sxs-lookup"><span data-stu-id="40e57-451">Consult server-side logs to diagnose and fix the app.</span></span>
 1. <span data-ttu-id="40e57-452">Jeśli kod stanu to *200-OK* dla zasobu, zapoznaj się z zawartością odpowiedzi w narzędziach deweloperskich przeglądarki i sprawdź, czy zawartość jest zgodna z oczekiwanymi danymi.</span><span class="sxs-lookup"><span data-stu-id="40e57-452">If the status code is *200 - OK* for the resource, look at the response content in browser's developer tools and check that the content matches up with the data expected.</span></span> <span data-ttu-id="40e57-453">Typowym problemem jest na przykład Nieskonfigurowanie routingu w taki sposób, aby żądania zwracały `index.html` dane nawet dla innych plików.</span><span class="sxs-lookup"><span data-stu-id="40e57-453">For example, a common problem is to misconfigure routing so that requests return your `index.html` data even for other files.</span></span> <span data-ttu-id="40e57-454">Upewnij się, że odpowiedzi na `.wasm` żądania to pliki binarne webassembly, a odpowiedzi na `.dll` żądania to pliki binarne zestawu platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="40e57-454">Make sure that responses to `.wasm` requests are WebAssembly binaries and that responses to `.dll` requests are .NET assembly binaries.</span></span> <span data-ttu-id="40e57-455">W przeciwnym razie masz problem z Routing po stronie serwera do zdiagnozowania.</span><span class="sxs-lookup"><span data-stu-id="40e57-455">If not, you have a server-side routing problem to diagnose.</span></span>
 1. <span data-ttu-id="40e57-456">Sprawdź poprawność opublikowanych i wdrożonych danych wyjściowych aplikacji za pomocą [skryptu Rozwiązywanie problemów ze integralnością programu PowerShell](#troubleshoot-integrity-powershell-script).</span><span class="sxs-lookup"><span data-stu-id="40e57-456">Seek to validate the app's published and deployed output with the [Troubleshoot integrity PowerShell script](#troubleshoot-integrity-powershell-script).</span></span>

<span data-ttu-id="40e57-457">Jeśli potwierdzasz, że serwer zwraca poprawne dane plausibly, konieczne może być zmodyfikowanie zawartości między kompilacją i dostarczeniem pliku.</span><span class="sxs-lookup"><span data-stu-id="40e57-457">If you confirm that the server is returning plausibly correct data, there must be something else modifying the contents in between build and delivery of the file.</span></span> <span data-ttu-id="40e57-458">Aby zbadać to:</span><span class="sxs-lookup"><span data-stu-id="40e57-458">To investigate this:</span></span>

 * <span data-ttu-id="40e57-459">Zapoznaj się z mechanizmem kompilowania łańcucha narzędzi i wdrażania na wypadek modyfikacji plików po skompilowaniu plików.</span><span class="sxs-lookup"><span data-stu-id="40e57-459">Examine the build toolchain and deployment mechanism in case they're modifying files after the files are built.</span></span> <span data-ttu-id="40e57-460">Przykładem takiej sytuacji jest to, że program git przekształca końce wierszy plików zgodnie z wcześniejszym opisem.</span><span class="sxs-lookup"><span data-stu-id="40e57-460">An example of this is when Git transforms file line endings, as described earlier.</span></span>
 * <span data-ttu-id="40e57-461">Zapoznaj się z konfiguracją serwer sieci Web lub sieć CDN w przypadku, gdy są skonfigurowane do dynamicznego modyfikowania odpowiedzi (na przykład próba zminifikować HTML).</span><span class="sxs-lookup"><span data-stu-id="40e57-461">Examine the web server or CDN configuration in case they're set up to modify responses dynamically (for example, trying to minify HTML).</span></span> <span data-ttu-id="40e57-462">Serwer sieci Web może zaimplementować kompresję HTTP (na przykład zwracając `content-encoding: br` lub `content-encoding: gzip` ), ponieważ nie ma to wpływu na wynik po dekompresji.</span><span class="sxs-lookup"><span data-stu-id="40e57-462">It's fine for the web server to implement HTTP compression (for example, returning `content-encoding: br` or `content-encoding: gzip`), since this doesn't affect the result after decompression.</span></span> <span data-ttu-id="40e57-463">*Nie* jest jednak konieczne, aby serwer sieci Web modyfikował nieskompresowane dane.</span><span class="sxs-lookup"><span data-stu-id="40e57-463">However, it's *not* fine for the web server to modify the uncompressed data.</span></span>

### <a name="troubleshoot-integrity-powershell-script"></a><span data-ttu-id="40e57-464">Rozwiązywanie problemów z integralnością skryptu programu PowerShell</span><span class="sxs-lookup"><span data-stu-id="40e57-464">Troubleshoot integrity PowerShell script</span></span>

<span data-ttu-id="40e57-465">Użyj [`integrity.ps1`](https://github.com/dotnet/AspNetCore.Docs/blob/main/aspnetcore/blazor/host-and-deploy/webassembly/_samples/integrity.ps1?raw=true) skryptu programu PowerShell, aby zweryfikować opublikowaną i wdrożoną Blazor aplikację.</span><span class="sxs-lookup"><span data-stu-id="40e57-465">Use the [`integrity.ps1`](https://github.com/dotnet/AspNetCore.Docs/blob/main/aspnetcore/blazor/host-and-deploy/webassembly/_samples/integrity.ps1?raw=true) PowerShell script to validate a published and deployed Blazor app.</span></span> <span data-ttu-id="40e57-466">Skrypt jest dostarczany jako punkt wyjścia, gdy aplikacja ma problemy ze integralnością, których Blazor nie można zidentyfikować w strukturze.</span><span class="sxs-lookup"><span data-stu-id="40e57-466">The script is provided as a starting point when the app has integrity issues that the Blazor framework can't identify.</span></span> <span data-ttu-id="40e57-467">Dla Twoich aplikacji może być wymagane dostosowanie skryptu.</span><span class="sxs-lookup"><span data-stu-id="40e57-467">Customization of the script might be required for your apps.</span></span>

<span data-ttu-id="40e57-468">Skrypt sprawdza pliki w `publish` folderze i pobrane ze wdrożonej aplikacji w celu wykrywania problemów w różnych manifestach zawierających skróty integralności.</span><span class="sxs-lookup"><span data-stu-id="40e57-468">The script checks the files in the `publish` folder and downloaded from the deployed app to detect issues in the different manifests that contain integrity hashes.</span></span> <span data-ttu-id="40e57-469">Te testy powinny wykrywać najczęstsze problemy:</span><span class="sxs-lookup"><span data-stu-id="40e57-469">These checks should detect the most common problems:</span></span>

* <span data-ttu-id="40e57-470">Plik został zmodyfikowany w opublikowanym danych wyjściowych bez jego realizacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-470">You modified a file in the published output without realizing it.</span></span>
* <span data-ttu-id="40e57-471">Aplikacja nie została poprawnie wdrożona w miejscu docelowym wdrożenia lub coś uległo zmianie w środowisku docelowym wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="40e57-471">The app wasn't correctly deployed to the deployment target, or something changed within the deployment target's environment.</span></span>
* <span data-ttu-id="40e57-472">Istnieją różnice między wdrożoną aplikacją a danymi wyjściowymi publikowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-472">There are differences between the deployed app and the output from publishing the app.</span></span>

<span data-ttu-id="40e57-473">Wywołaj skrypt za pomocą następującego polecenia w powłoce poleceń programu PowerShell:</span><span class="sxs-lookup"><span data-stu-id="40e57-473">Invoke the script with the following command in a PowerShell command shell:</span></span>

```powershell
.\integrity.ps1 {BASE URL} {PUBLISH OUTPUT FOLDER}
```

<span data-ttu-id="40e57-474">Symbole zastępcze</span><span class="sxs-lookup"><span data-stu-id="40e57-474">Placeholders:</span></span>

* <span data-ttu-id="40e57-475">`{BASE URL}`: Adres URL wdrożonej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="40e57-475">`{BASE URL}`: The URL of the deployed app.</span></span>
* <span data-ttu-id="40e57-476">`{PUBLISH OUTPUT FOLDER}`: Ścieżka do `publish` folderu lub lokalizacji aplikacji, w której aplikacja została opublikowana do wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="40e57-476">`{PUBLISH OUTPUT FOLDER}`: The path to the app's `publish` folder or location where the app is published for deployment.</span></span>

> [!NOTE]
> <span data-ttu-id="40e57-477">Aby sklonować `dotnet/AspNetCore.Docs` repozytorium GitHub do systemu, który używa skanera antywirusowego [BitDefender](https://www.bitdefender.com) , Dodaj wyjątek do BitDefender dla `integrity.ps1` skryptu.</span><span class="sxs-lookup"><span data-stu-id="40e57-477">To clone the `dotnet/AspNetCore.Docs` GitHub repository to a system that uses the [Bitdefender](https://www.bitdefender.com) virus scanner, add an exception to Bitdefender for the `integrity.ps1` script.</span></span> <span data-ttu-id="40e57-478">Dodaj wyjątek do BitDefender przed klonem repozytorium, aby uniknąć posiadania skryptu poddanego kwarantannie przez skaner wirusów.</span><span class="sxs-lookup"><span data-stu-id="40e57-478">Add the exception to Bitdefender before cloning the repo to avoid having the script quarantined by the virus scanner.</span></span> <span data-ttu-id="40e57-479">Poniższy przykład jest typową ścieżką do skryptu dla sklonowanego repozytorium w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="40e57-479">The following example is a typical path to the script for the cloned repo on a Windows system.</span></span> <span data-ttu-id="40e57-480">Dostosuj ścieżkę w razie konieczności.</span><span class="sxs-lookup"><span data-stu-id="40e57-480">Adjust the path as needed.</span></span> <span data-ttu-id="40e57-481">Symbol zastępczy `{USER}` jest segmentem ścieżki użytkownika.</span><span class="sxs-lookup"><span data-stu-id="40e57-481">The placeholder `{USER}` is the user's path segment.</span></span>
>
> ```
> C:\Users\{USER}\Documents\GitHub\AspNetCore.Docs\aspnetcore\blazor\host-and-deploy\webassembly\_samples\integrity.ps1
> ```

### <a name="disable-integrity-checking-for-non-pwa-apps"></a><span data-ttu-id="40e57-482">Wyłącz sprawdzanie integralności dla aplikacji innych niż PWA</span><span class="sxs-lookup"><span data-stu-id="40e57-482">Disable integrity checking for non-PWA apps</span></span>

<span data-ttu-id="40e57-483">W większości przypadków nie należy wyłączać sprawdzania integralności.</span><span class="sxs-lookup"><span data-stu-id="40e57-483">In most cases, don't disable integrity checking.</span></span> <span data-ttu-id="40e57-484">Wyłączenie sprawdzania integralności nie rozwiązuje podstawowego problemu, który spowodował nieoczekiwane odpowiedzi i spowoduje utratę wymienionych wcześniej korzyści.</span><span class="sxs-lookup"><span data-stu-id="40e57-484">Disabling integrity checking doesn't solve the underlying problem that has caused the unexpected responses and results in losing the benefits listed earlier.</span></span>

<span data-ttu-id="40e57-485">Mogą jednak wystąpić sytuacje, w których serwer sieci Web nie może być oparty na zwracaniu spójnych odpowiedzi i nie ma możliwości wyboru, ale w celu wyłączenia kontroli integralności.</span><span class="sxs-lookup"><span data-stu-id="40e57-485">There may be cases where the web server can't be relied upon to return consistent responses, and you have no choice but to disable integrity checks.</span></span> <span data-ttu-id="40e57-486">Aby wyłączyć sprawdzanie integralności, Dodaj następujący element do grupy właściwości w Blazor WebAssembly `.csproj` pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="40e57-486">To disable integrity checks, add the following to a property group in the Blazor WebAssembly project's `.csproj` file:</span></span>

```xml
<BlazorCacheBootResources>false</BlazorCacheBootResources>
```

<span data-ttu-id="40e57-487">`BlazorCacheBootResources` wyłącza również Blazor domyślne zachowanie buforowania `.dll` , `.wasm` i innych plików na podstawie skrótów SHA-256, ponieważ właściwość wskazuje, że nie można opierać się na wartości skrótów SHA-256 w celu poprawnego działania.</span><span class="sxs-lookup"><span data-stu-id="40e57-487">`BlazorCacheBootResources` also disables Blazor's default behavior of caching the `.dll`, `.wasm`, and other files based on their SHA-256 hashes because the property indicates that the SHA-256 hashes can't be relied upon for correctness.</span></span> <span data-ttu-id="40e57-488">Nawet w przypadku tego ustawienia Normalna pamięć podręczna w przeglądarce może nadal buforować te pliki, ale bez względu na to, czy ma to miejsce, zależy od konfiguracji serwera sieci Web i `cache-control` nagłówków, które obsługuje.</span><span class="sxs-lookup"><span data-stu-id="40e57-488">Even with this setting, the browser's normal HTTP cache may still cache those files, but whether or not this happens depends on your web server configuration and the `cache-control` headers that it serves.</span></span>

> [!NOTE]
> <span data-ttu-id="40e57-489">`BlazorCacheBootResources`Właściwość nie wyłącza sprawdzania integralności w przypadku [progresywnych aplikacji sieci Web (PWAs)](xref:blazor/progressive-web-app).</span><span class="sxs-lookup"><span data-stu-id="40e57-489">The `BlazorCacheBootResources` property doesn't disable integrity checks for [Progressive Web Applications (PWAs)](xref:blazor/progressive-web-app).</span></span> <span data-ttu-id="40e57-490">Wskazówki dotyczące PWAs można znaleźć w sekcji [Wyłączanie sprawdzania integralności dla PWAs](#disable-integrity-checking-for-pwas) .</span><span class="sxs-lookup"><span data-stu-id="40e57-490">For guidance pertaining to PWAs, see the [Disable integrity checking for PWAs](#disable-integrity-checking-for-pwas) section.</span></span>

### <a name="disable-integrity-checking-for-pwas"></a><span data-ttu-id="40e57-491">Wyłącz sprawdzanie integralności dla PWAs</span><span class="sxs-lookup"><span data-stu-id="40e57-491">Disable integrity checking for PWAs</span></span>

<span data-ttu-id="40e57-492">Blazorszablon aplikacji sieci Web progresywnej (PWA) zawiera sugerowany `service-worker.published.js` plik, który jest odpowiedzialny za pobieranie i przechowywanie plików aplikacji do użytku w trybie offline.</span><span class="sxs-lookup"><span data-stu-id="40e57-492">Blazor's Progressive Web Application (PWA) template contains a suggested `service-worker.published.js` file that's responsible for fetching and storing application files for offline use.</span></span> <span data-ttu-id="40e57-493">Jest to oddzielny proces od normalnego mechanizmu uruchamiania aplikacji i ma własną logikę sprawdzania integralności.</span><span class="sxs-lookup"><span data-stu-id="40e57-493">This is a separate process from the normal app startup mechanism and has its own separate integrity checking logic.</span></span>

<span data-ttu-id="40e57-494">W `service-worker.published.js` pliku znajduje się następujący wiersz:</span><span class="sxs-lookup"><span data-stu-id="40e57-494">Inside the `service-worker.published.js` file, following line is present:</span></span>

```javascript
.map(asset => new Request(asset.url, { integrity: asset.hash }));
```

<span data-ttu-id="40e57-495">Aby wyłączyć sprawdzanie integralności, Usuń `integrity` parametr, zmieniając wiersz na następujący:</span><span class="sxs-lookup"><span data-stu-id="40e57-495">To disable integrity checking, remove the `integrity` parameter by changing the line to the following:</span></span>

```javascript
.map(asset => new Request(asset.url));
```

<span data-ttu-id="40e57-496">Po ponownym uruchomieniu kontroli integralności oznacza to, że utracisz gwarancje bezpieczeństwa oferowane przez sprawdzanie integralności.</span><span class="sxs-lookup"><span data-stu-id="40e57-496">Again, disabling integrity checking means that you lose the safety guarantees offered by integrity checking.</span></span> <span data-ttu-id="40e57-497">Na przykład istnieje ryzyko, że jeśli przeglądarka użytkownika będzie buforować aplikację w tym samym czasie, gdy zostanie wdrożona nowa wersja, może to spowodować przełączenie niektórych plików ze starego wdrożenia, a niektóre z nowego wdrożenia.</span><span class="sxs-lookup"><span data-stu-id="40e57-497">For example, there is a risk that if the user's browser is caching the app at the exact moment that you deploy a new version, it could cache some files from the old deployment and some from the new deployment.</span></span> <span data-ttu-id="40e57-498">W takim przypadku aplikacja zostanie zablokowana w stanie przerwania, dopóki nie zostanie wdrożona dodatkowa aktualizacja.</span><span class="sxs-lookup"><span data-stu-id="40e57-498">If that happens, the app becomes stuck in a broken state until you deploy a further update.</span></span>
