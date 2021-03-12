---
title: Wprowadzenie do Swashbuckle i ASP.NET Core
author: zuckerthoben
description: Dowiedz się, jak dodać Swashbuckle do projektu interfejsu API sieci Web ASP.NET Core, aby zintegrować interfejs użytkownika struktury Swagger.
ms.author: scaddie
ms.custom: mvc
ms.date: 06/26/2020
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
uid: tutorials/get-started-with-swashbuckle
ms.openlocfilehash: fdec03422f4a4517950ff6de2a0400df5307b40f
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102588539"
---
# <a name="get-started-with-swashbuckle-and-aspnet-core"></a><span data-ttu-id="021fa-103">Wprowadzenie do Swashbuckle i ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="021fa-103">Get started with Swashbuckle and ASP.NET Core</span></span>

<span data-ttu-id="021fa-104">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="021fa-104">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/tutorials/web-api-help-pages-using-swagger/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="021fa-105">Pakiet Swashbuckle składa się z trzech głównych składników:</span><span class="sxs-lookup"><span data-stu-id="021fa-105">There are three main components to Swashbuckle:</span></span>

* <span data-ttu-id="021fa-106">[Swashbuckle. AspNetCore. Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): model obiektów Swagger i oprogramowanie pośredniczące umożliwiające Uwidacznianie `SwaggerDocument` obiektów jako punktów końcowych JSON.</span><span class="sxs-lookup"><span data-stu-id="021fa-106">[Swashbuckle.AspNetCore.Swagger](https://www.nuget.org/packages/Swashbuckle.AspNetCore.Swagger/): a Swagger object model and middleware to expose `SwaggerDocument` objects as JSON endpoints.</span></span>

* <span data-ttu-id="021fa-107">[Swashbuckle. AspNetCore. SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): Generator Swagger, który kompiluje `SwaggerDocument` obiekty bezpośrednio z tras, kontrolerów i modeli.</span><span class="sxs-lookup"><span data-stu-id="021fa-107">[Swashbuckle.AspNetCore.SwaggerGen](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerGen/): a Swagger generator that builds `SwaggerDocument` objects directly from your routes, controllers, and models.</span></span> <span data-ttu-id="021fa-108">Zazwyczaj jest ona łączona z oprogramowaniem pośredniczącym punktu końcowego programu Swagger, aby automatycznie uwidaczniać kod JSON programu Swagger.</span><span class="sxs-lookup"><span data-stu-id="021fa-108">It's typically combined with the Swagger endpoint middleware to automatically expose Swagger JSON.</span></span>

* <span data-ttu-id="021fa-109">[Swashbuckle. AspNetCore. SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): wbudowana wersja narzędzia interfejsu użytkownika struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="021fa-109">[Swashbuckle.AspNetCore.SwaggerUI](https://www.nuget.org/packages/Swashbuckle.AspNetCore.SwaggerUI/): an embedded version of the Swagger UI tool.</span></span> <span data-ttu-id="021fa-110">Interpretuje kod JSON programu Swagger w celu tworzenia rozbudowanych, możliwych do dostosowania środowisk na potrzeby opisywania funkcji internetowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="021fa-110">It interprets Swagger JSON to build a rich, customizable experience for describing the web API functionality.</span></span> <span data-ttu-id="021fa-111">Obejmuje wbudowane kontrolery testów dla metod publicznych.</span><span class="sxs-lookup"><span data-stu-id="021fa-111">It includes built-in test harnesses for the public methods.</span></span>

## <a name="package-installation"></a><span data-ttu-id="021fa-112">Instalacja pakietu</span><span class="sxs-lookup"><span data-stu-id="021fa-112">Package installation</span></span>

<span data-ttu-id="021fa-113">Swashbuckle można dodać przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="021fa-113">Swashbuckle can be added with the following approaches:</span></span>

### <a name="visual-studio"></a>[<span data-ttu-id="021fa-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="021fa-114">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="021fa-115">W oknie **konsola Menedżera pakietów** :</span><span class="sxs-lookup"><span data-stu-id="021fa-115">From the **Package Manager Console** window:</span></span>
  * <span data-ttu-id="021fa-116">Przejdź do **wyświetlania**  >  **innych**  >  **konsoli Menedżera pakietów** systemu Windows</span><span class="sxs-lookup"><span data-stu-id="021fa-116">Go to **View** > **Other Windows** > **Package Manager Console**</span></span>
  * <span data-ttu-id="021fa-117">Przejdź do katalogu, w którym znajduje się plik *TodoApi. csproj*</span><span class="sxs-lookup"><span data-stu-id="021fa-117">Navigate to the directory in which the *TodoApi.csproj* file exists</span></span>
  * <span data-ttu-id="021fa-118">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="021fa-118">Execute the following command:</span></span>

    ```powershell
    Install-Package Swashbuckle.AspNetCore -Version 5.6.3
    ```

* <span data-ttu-id="021fa-119">W oknie dialogowym **Zarządzanie pakietami NuGet** :</span><span class="sxs-lookup"><span data-stu-id="021fa-119">From the **Manage NuGet Packages** dialog:</span></span>
  * <span data-ttu-id="021fa-120">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań**  >  **Zarządzanie pakietami NuGet**</span><span class="sxs-lookup"><span data-stu-id="021fa-120">Right-click the project in **Solution Explorer** > **Manage NuGet Packages**</span></span>
  * <span data-ttu-id="021fa-121">Ustaw **Źródło pakietu** na "NuGet.org"</span><span class="sxs-lookup"><span data-stu-id="021fa-121">Set the **Package source** to "nuget.org"</span></span>
  * <span data-ttu-id="021fa-122">Upewnij się, że opcja "Uwzględnij wersję wstępną" jest włączona</span><span class="sxs-lookup"><span data-stu-id="021fa-122">Ensure the "Include prerelease" option is enabled</span></span>
  * <span data-ttu-id="021fa-123">Wprowadź ciąg "Swashbuckle. AspNetCore" w polu wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="021fa-123">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
  * <span data-ttu-id="021fa-124">Wybierz najnowszy pakiet "Swashbuckle. AspNetCore" z karty **Przeglądaj** , a następnie kliknij przycisk **Instaluj** .</span><span class="sxs-lookup"><span data-stu-id="021fa-124">Select the latest "Swashbuckle.AspNetCore" package from the **Browse** tab and click **Install**</span></span>

### <a name="visual-studio-for-mac"></a>[<span data-ttu-id="021fa-125">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="021fa-125">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="021fa-126">Kliknij prawym przyciskiem myszy folder *pakiety* w **okienko rozwiązania**  >  **Dodaj pakiety...**</span><span class="sxs-lookup"><span data-stu-id="021fa-126">Right-click the *Packages* folder in **Solution Pad** > **Add Packages...**</span></span>
* <span data-ttu-id="021fa-127">Ustaw listę rozwijaną **źródła** okna **Dodaj pakiety** na "NuGet.org"</span><span class="sxs-lookup"><span data-stu-id="021fa-127">Set the **Add Packages** window's **Source** drop-down to "nuget.org"</span></span>
* <span data-ttu-id="021fa-128">Upewnij się, że opcja "Pokaż pakiety wersji wstępnej" jest włączona</span><span class="sxs-lookup"><span data-stu-id="021fa-128">Ensure the "Show pre-release packages" option is enabled</span></span>
* <span data-ttu-id="021fa-129">Wprowadź ciąg "Swashbuckle. AspNetCore" w polu wyszukiwania</span><span class="sxs-lookup"><span data-stu-id="021fa-129">Enter "Swashbuckle.AspNetCore" in the search box</span></span>
* <span data-ttu-id="021fa-130">Wybierz najnowszy pakiet "Swashbuckle. AspNetCore" z okienka wyników, a następnie kliknij pozycję **Dodaj pakiet** .</span><span class="sxs-lookup"><span data-stu-id="021fa-130">Select the latest "Swashbuckle.AspNetCore" package from the results pane and click **Add Package**</span></span>

### <a name="visual-studio-code"></a>[<span data-ttu-id="021fa-131">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="021fa-131">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="021fa-132">Uruchom następujące polecenie w **zintegrowanym terminalu**:</span><span class="sxs-lookup"><span data-stu-id="021fa-132">Run the following command from the **Integrated Terminal**:</span></span>

```dotnetcli
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore -v 5.6.3
```

### <a name="net-core-cli"></a>[<span data-ttu-id="021fa-133">interfejs wiersza polecenia programu .NET Core</span><span class="sxs-lookup"><span data-stu-id="021fa-133">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="021fa-134">Uruchom następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="021fa-134">Run the following command:</span></span>

```dotnetcli
dotnet add TodoApi.csproj package Swashbuckle.AspNetCore -v 5.6.3
```

---

## <a name="add-and-configure-swagger-middleware"></a><span data-ttu-id="021fa-135">Dodawanie i Konfigurowanie oprogramowania pośredniczącego programu Swagger</span><span class="sxs-lookup"><span data-stu-id="021fa-135">Add and configure Swagger middleware</span></span>

<span data-ttu-id="021fa-136">Dodaj Generator Swagger do kolekcji usług w `Startup.ConfigureServices` metodzie:</span><span class="sxs-lookup"><span data-stu-id="021fa-136">Add the Swagger generator to the services collection in the `Startup.ConfigureServices` method:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=9)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/3.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_ConfigureServices&highlight=8)]

::: moniker-end

<span data-ttu-id="021fa-137">W `Startup.Configure` metodzie Włącz oprogramowanie pośredniczące do obsługi wygenerowanego dokumentu JSON i interfejsu użytkownika programu Swagger:</span><span class="sxs-lookup"><span data-stu-id="021fa-137">In the `Startup.Configure` method, enable the middleware for serving the generated JSON document and the Swagger UI:</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/3.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_Configure&highlight=4,8-11)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="021fa-138">Swashbuckle opiera się na MVC <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> , aby odnaleźć trasy i punkty końcowe.</span><span class="sxs-lookup"><span data-stu-id="021fa-138">Swashbuckle relies on MVC's <xref:Microsoft.AspNetCore.Mvc.ApiExplorer> to discover the routes and endpoints.</span></span> <span data-ttu-id="021fa-139">Jeśli wywołania projektu <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc%2A> , trasy i punkty końcowe są wykrywane automatycznie.</span><span class="sxs-lookup"><span data-stu-id="021fa-139">If the project calls <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc%2A>, routes and endpoints are discovered automatically.</span></span> <span data-ttu-id="021fa-140">Podczas wywoływania <xref:Microsoft.Extensions.DependencyInjection.MvcCoreServiceCollectionExtensions.AddMvcCore%2A> <xref:Microsoft.Extensions.DependencyInjection.MvcApiExplorerMvcCoreBuilderExtensions.AddApiExplorer%2A> Metoda musi być jawnie wywołana.</span><span class="sxs-lookup"><span data-stu-id="021fa-140">When calling <xref:Microsoft.Extensions.DependencyInjection.MvcCoreServiceCollectionExtensions.AddMvcCore%2A>, the <xref:Microsoft.Extensions.DependencyInjection.MvcApiExplorerMvcCoreBuilderExtensions.AddApiExplorer%2A> method must be explicitly called.</span></span> <span data-ttu-id="021fa-141">Aby uzyskać więcej informacji, zobacz [Swashbuckle, ApiExplorer i Routing](https://github.com/domaindrivendev/Swashbuckle.AspNetCore#swashbuckle-apiexplorer-and-routing).</span><span class="sxs-lookup"><span data-stu-id="021fa-141">For more information, see [Swashbuckle, ApiExplorer, and Routing](https://github.com/domaindrivendev/Swashbuckle.AspNetCore#swashbuckle-apiexplorer-and-routing).</span></span>

<span data-ttu-id="021fa-142">Poprzednie `UseSwaggerUI` wywołanie metody włącza [oprogramowanie pośredniczące pliku statycznego](xref:fundamentals/static-files).</span><span class="sxs-lookup"><span data-stu-id="021fa-142">The preceding `UseSwaggerUI` method call enables the [Static File Middleware](xref:fundamentals/static-files).</span></span> <span data-ttu-id="021fa-143">Jeśli obiektem docelowym jest .NET Framework lub .NET Core 1. x, Dodaj pakiet NuGet [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) do projektu.</span><span class="sxs-lookup"><span data-stu-id="021fa-143">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) NuGet package to the project.</span></span>

<span data-ttu-id="021fa-144">Uruchom aplikację i przejdź do `http://localhost:<port>/swagger/v1/swagger.json` .</span><span class="sxs-lookup"><span data-stu-id="021fa-144">Launch the app, and navigate to `http://localhost:<port>/swagger/v1/swagger.json`.</span></span> <span data-ttu-id="021fa-145">Wygenerowany dokument opisujący punkty końcowe wygląda tak, jak pokazano w [specyfikacji openapi (openapi.json)](xref:tutorials/web-api-help-pages-using-swagger#openapi-specification-openapijson).</span><span class="sxs-lookup"><span data-stu-id="021fa-145">The generated document describing the endpoints appears as shown in [OpenAPI specification (openapi.json)](xref:tutorials/web-api-help-pages-using-swagger#openapi-specification-openapijson).</span></span>

<span data-ttu-id="021fa-146">Interfejs użytkownika struktury Swagger można znaleźć pod adresem `http://localhost:<port>/swagger` .</span><span class="sxs-lookup"><span data-stu-id="021fa-146">The Swagger UI can be found at `http://localhost:<port>/swagger`.</span></span> <span data-ttu-id="021fa-147">Eksploruj interfejs API za pośrednictwem interfejsu użytkownika struktury Swagger i Uwzględnij go w innych programach.</span><span class="sxs-lookup"><span data-stu-id="021fa-147">Explore the API via Swagger UI and incorporate it in other programs.</span></span>

> [!TIP]
> <span data-ttu-id="021fa-148">Aby obpracować interfejs użytkownika struktury Swagger w katalogu głównym aplikacji ( `http://localhost:<port>/` ), ustaw `RoutePrefix` Właściwość na pusty ciąg:</span><span class="sxs-lookup"><span data-stu-id="021fa-148">To serve the Swagger UI at the app's root (`http://localhost:<port>/`), set the `RoutePrefix` property to an empty string:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_UseSwaggerUI&highlight=4)]

<span data-ttu-id="021fa-149">W przypadku używania katalogów z usługami IIS lub zwrotnego serwera proxy Ustaw punkt końcowy struktury Swagger na ścieżkę względną przy użyciu `./` prefiksu.</span><span class="sxs-lookup"><span data-stu-id="021fa-149">If using directories with IIS or a reverse proxy, set the Swagger endpoint to a relative path using the `./` prefix.</span></span> <span data-ttu-id="021fa-150">Na przykład `./swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="021fa-150">For example, `./swagger/v1/swagger.json`.</span></span> <span data-ttu-id="021fa-151">Użycie `/swagger/v1/swagger.json` instruuje aplikację, aby wyszukać plik JSON w prawdziwym katalogu głównym adresu URL (plus prefiks trasy, jeśli jest używany).</span><span class="sxs-lookup"><span data-stu-id="021fa-151">Using `/swagger/v1/swagger.json` instructs the app to look for the JSON file at the true root of the URL (plus the route prefix, if used).</span></span> <span data-ttu-id="021fa-152">Na przykład: użyj opcji `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` zamiast `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span><span class="sxs-lookup"><span data-stu-id="021fa-152">For example, use `http://localhost:<port>/<route_prefix>/swagger/v1/swagger.json` instead of `http://localhost:<port>/<virtual_directory>/<route_prefix>/swagger/v1/swagger.json`.</span></span>

> [!NOTE]
> <span data-ttu-id="021fa-153">Domyślnie Swashbuckle generuje i uwidacznia kod JSON struktury Swagger w wersji 3,0 specyfikacji &mdash; oficjalnie zwanej specyfikacją openapi.</span><span class="sxs-lookup"><span data-stu-id="021fa-153">By default, Swashbuckle generates and exposes Swagger JSON in version 3.0 of the specification&mdash;officially called the OpenAPI Specification.</span></span> <span data-ttu-id="021fa-154">Aby zapewnić zgodność z poprzednimi wersjami, można w zamian wybrać kod JSON w formacie 2,0.</span><span class="sxs-lookup"><span data-stu-id="021fa-154">To support backwards compatibility, you can opt into exposing JSON in the 2.0 format instead.</span></span> <span data-ttu-id="021fa-155">Ten format 2,0 jest ważny dla integracji, takich jak Microsoft PowerShell Apps i Microsoft Flow, które obecnie obsługują OpenAPI w wersji 2,0.</span><span class="sxs-lookup"><span data-stu-id="021fa-155">This 2.0 format is important for integrations such as Microsoft Power Apps and Microsoft Flow that currently support OpenAPI version 2.0.</span></span> <span data-ttu-id="021fa-156">Aby wybrać format 2,0, ustaw `SerializeAsV2` Właściwość w `Startup.Configure` :</span><span class="sxs-lookup"><span data-stu-id="021fa-156">To opt into the 2.0 format, set the `SerializeAsV2` property in `Startup.Configure`:</span></span>
>
> [!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/3.0/TodoApi.Swashbuckle/Startup3.cs?name=snippet_Configure&highlight=4-7)]

## <a name="customize-and-extend"></a><span data-ttu-id="021fa-157">Dostosuj i rozwiń</span><span class="sxs-lookup"><span data-stu-id="021fa-157">Customize and extend</span></span>

<span data-ttu-id="021fa-158">Struktura Swagger zawiera opcje dokumentowania modelu obiektów i dostosowywania interfejsu użytkownika w celu dopasowania go do motywu.</span><span class="sxs-lookup"><span data-stu-id="021fa-158">Swagger provides options for documenting the object model and customizing the UI to match your theme.</span></span>

<span data-ttu-id="021fa-159">W `Startup` klasie Dodaj następujące przestrzenie nazw:</span><span class="sxs-lookup"><span data-stu-id="021fa-159">In the `Startup` class, add the following namespaces:</span></span>

```csharp
using System;
using System.Reflection;
using System.IO;
```

### <a name="api-info-and-description"></a><span data-ttu-id="021fa-160">Informacje o interfejsie API i opis</span><span class="sxs-lookup"><span data-stu-id="021fa-160">API info and description</span></span>

<span data-ttu-id="021fa-161">Akcja konfiguracji przeniesiona do `AddSwaggerGen` metody powoduje dodanie informacji, takich jak autor, licencja i opis:</span><span class="sxs-lookup"><span data-stu-id="021fa-161">The configuration action passed to the `AddSwaggerGen` method adds information such as the author, license, and description:</span></span>

<span data-ttu-id="021fa-162">W `Startup` klasie zaimportuj następującą przestrzeń nazw, aby użyć `OpenApiInfo` klasy:</span><span class="sxs-lookup"><span data-stu-id="021fa-162">In the `Startup` class, import the following namespace to use the `OpenApiInfo` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup2.cs?name=snippet_InfoClassNamespace)]

<span data-ttu-id="021fa-163">Korzystając z `OpenApiInfo` klasy, należy zmodyfikować informacje wyświetlane w interfejsie użytkownika:</span><span class="sxs-lookup"><span data-stu-id="021fa-163">Using the `OpenApiInfo` class, modify the information displayed in the UI:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup4.cs?name=snippet_AddSwaggerGen)]

<span data-ttu-id="021fa-164">Interfejs użytkownika struktury Swagger wyświetla informacje o wersji:</span><span class="sxs-lookup"><span data-stu-id="021fa-164">The Swagger UI displays the version's information:</span></span>

![Interfejs użytkownika struktury Swagger z informacjami o wersji: Description, Author i linku więcej](web-api-help-pages-using-swagger/_static/custom-info.png)

### <a name="xml-comments"></a><span data-ttu-id="021fa-166">Komentarze XML</span><span class="sxs-lookup"><span data-stu-id="021fa-166">XML comments</span></span>

<span data-ttu-id="021fa-167">Komentarze XML można włączyć przy użyciu następujących metod:</span><span class="sxs-lookup"><span data-stu-id="021fa-167">XML comments can be enabled with the following approaches:</span></span>

#### <a name="visual-studio"></a>[<span data-ttu-id="021fa-168">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="021fa-168">Visual Studio</span></span>](#tab/visual-studio)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="021fa-169">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** a następnie wybierz polecenie **edytuj <Project_Name>. csproj**.</span><span class="sxs-lookup"><span data-stu-id="021fa-169">Right-click the project in **Solution Explorer** and select **Edit <project_name>.csproj**.</span></span>
* <span data-ttu-id="021fa-170">Ręcznie Dodaj wyróżnione wiersze do pliku *csproj* :</span><span class="sxs-lookup"><span data-stu-id="021fa-170">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="021fa-171">Kliknij prawym przyciskiem myszy projekt w **Eksplorator rozwiązań** i wybierz polecenie **Właściwości**.</span><span class="sxs-lookup"><span data-stu-id="021fa-171">Right-click the project in **Solution Explorer** and select **Properties**.</span></span>
* <span data-ttu-id="021fa-172">Sprawdź pole **plik dokumentacji XML** w sekcji **dane wyjściowe** na karcie **kompilacja** .</span><span class="sxs-lookup"><span data-stu-id="021fa-172">Check the **XML documentation file** box under the **Output** section of the **Build** tab.</span></span>

::: moniker-end

#### <a name="visual-studio-for-mac"></a>[<span data-ttu-id="021fa-173">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="021fa-173">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

::: moniker range=">= aspnetcore-2.0"

* <span data-ttu-id="021fa-174">W *okienko rozwiązania* naciśnij klawisz **Control** i kliknij nazwę projektu.</span><span class="sxs-lookup"><span data-stu-id="021fa-174">From the *Solution Pad*, press **control** and click the project name.</span></span> <span data-ttu-id="021fa-175">Przejdź do **menu Narzędzia**  >  **Edytuj plik**.</span><span class="sxs-lookup"><span data-stu-id="021fa-175">Navigate to **Tools** > **Edit File**.</span></span>
* <span data-ttu-id="021fa-176">Ręcznie Dodaj wyróżnione wiersze do pliku *csproj* :</span><span class="sxs-lookup"><span data-stu-id="021fa-176">Manually add the highlighted lines to the *.csproj* file:</span></span>

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

* <span data-ttu-id="021fa-177">Otwórz okno dialogowe **Opcje projektu** >  > **kompilator** kompilacji</span><span class="sxs-lookup"><span data-stu-id="021fa-177">Open the **Project Options** dialog > **Build** > **Compiler**</span></span>
* <span data-ttu-id="021fa-178">Zaznacz pole **Generuj dokumentację XML** w sekcji **Opcje ogólne** .</span><span class="sxs-lookup"><span data-stu-id="021fa-178">Check the **Generate xml documentation** box under the **General Options** section</span></span>

::: moniker-end

#### <a name="visual-studio-code"></a>[<span data-ttu-id="021fa-179">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="021fa-179">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="021fa-180">Ręcznie Dodaj wyróżnione wiersze do pliku *csproj* :</span><span class="sxs-lookup"><span data-stu-id="021fa-180">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

#### <a name="net-core-cli"></a>[<span data-ttu-id="021fa-181">interfejs wiersza polecenia programu .NET Core</span><span class="sxs-lookup"><span data-stu-id="021fa-181">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="021fa-182">Ręcznie Dodaj wyróżnione wiersze do pliku *csproj* :</span><span class="sxs-lookup"><span data-stu-id="021fa-182">Manually add the highlighted lines to the *.csproj* file:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=1-2,4)]

::: moniker-end

---

<span data-ttu-id="021fa-183">Włączenie komentarzy XML zapewnia informacje debugowania dla nieudokumentowanych typów publicznych i członków.</span><span class="sxs-lookup"><span data-stu-id="021fa-183">Enabling XML comments provides debug information for undocumented public types and members.</span></span> <span data-ttu-id="021fa-184">Nieudokumentowane typy i elementy członkowskie są wskazywane przez komunikat ostrzegawczy.</span><span class="sxs-lookup"><span data-stu-id="021fa-184">Undocumented types and members are indicated by the warning message.</span></span> <span data-ttu-id="021fa-185">Na przykład następujący komunikat oznacza naruszenie kodu ostrzegawczego 1591:</span><span class="sxs-lookup"><span data-stu-id="021fa-185">For example, the following message indicates a violation of warning code 1591:</span></span>

```text
warning CS1591: Missing XML comment for publicly visible type or member 'TodoController.GetAll()'
```

<span data-ttu-id="021fa-186">Aby pominąć ostrzeżenia dla całego projektu, należy zdefiniować rozdzielaną średnikami listę kodów ostrzeżeń do ignorowania w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="021fa-186">To suppress warnings project-wide, define a semicolon-delimited list of warning codes to ignore in the project file.</span></span> <span data-ttu-id="021fa-187">Dołączanie kodów ostrzeżeń w celu `$(NoWarn);` zastosowania [wartości domyślnych języka C#](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) .</span><span class="sxs-lookup"><span data-stu-id="021fa-187">Appending the warning codes to `$(NoWarn);` applies the [C# default values](https://github.com/dotnet/sdk/blob/2eb6c546931b5bcb92cd3128b93932a980553ea1/src/Tasks/Microsoft.NET.Build.Tasks/targets/Microsoft.NET.Sdk.CSharp.props#L16) too.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-xml[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/TodoApi.csproj?name=snippet_SuppressWarnings&highlight=3)]

::: moniker-end

<span data-ttu-id="021fa-188">Aby pominąć ostrzeżenia tylko dla określonych elementów członkowskich, należy ująć kod w [#pragma ostrzeżenia](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) preprocesora.</span><span class="sxs-lookup"><span data-stu-id="021fa-188">To suppress warnings only for specific members, enclose the code in [#pragma warning](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-pragma-warning) preprocessor directives.</span></span> <span data-ttu-id="021fa-189">Takie podejście jest przydatne w przypadku kodu, który nie powinien być ujawniony za pośrednictwem dokumentacji interfejsu API. W poniższym przykładzie kod ostrzegawczy CS1591 jest ignorowany dla całej `Program` klasy.</span><span class="sxs-lookup"><span data-stu-id="021fa-189">This approach is useful for code that shouldn't be exposed via the API docs. In the following example, warning code CS1591 is ignored for the entire `Program` class.</span></span> <span data-ttu-id="021fa-190">Wymuszanie kodu ostrzegawczego jest przywracane po zamknięciu definicji klasy.</span><span class="sxs-lookup"><span data-stu-id="021fa-190">Enforcement of the warning code is restored at the close of the class definition.</span></span> <span data-ttu-id="021fa-191">Określ wiele kodów ostrzeżeń z listą rozdzielaną przecinkami.</span><span class="sxs-lookup"><span data-stu-id="021fa-191">Specify multiple warning codes with a comma-delimited list.</span></span>

```csharp
namespace TodoApi
{
#pragma warning disable CS1591
    public class Program
    {
        public static void Main(string[] args) =>
            BuildWebHost(args).Run();

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
#pragma warning restore CS1591
}
```

<span data-ttu-id="021fa-192">Skonfiguruj strukturę Swagger, aby używała pliku XML, który jest generowany z poprzednimi instrukcjami.</span><span class="sxs-lookup"><span data-stu-id="021fa-192">Configure Swagger to use the XML file that's generated with the preceding instructions.</span></span> <span data-ttu-id="021fa-193">W przypadku systemów operacyjnych Linux lub innych niż Windows nazwy plików i ścieżki mogą być rozróżniane wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="021fa-193">For Linux or non-Windows operating systems, file names and paths can be case-sensitive.</span></span> <span data-ttu-id="021fa-194">Na przykład plik *TodoApi.XML* jest prawidłowy w systemie Windows, ale nie CentOS.</span><span class="sxs-lookup"><span data-stu-id="021fa-194">For example, a *TodoApi.XML* file is valid on Windows but not CentOS.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/3.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=31-33)]

::: moniker-end

::: moniker range="= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Startup1x.cs?name=snippet_ConfigureServices&highlight=30-32)]

::: moniker-end

<span data-ttu-id="021fa-195">W poprzednim kodzie [odbicie](/dotnet/csharp/programming-guide/concepts/reflection) jest używane do kompilowania nazwy pliku XML pasującego do projektu interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="021fa-195">In the preceding code, [Reflection](/dotnet/csharp/programming-guide/concepts/reflection) is used to build an XML file name matching that of the web API project.</span></span> <span data-ttu-id="021fa-196">Właściwość [AppContext. BaseDirectory](xref:System.AppContext.BaseDirectory*) służy do konstruowania ścieżki do pliku XML.</span><span class="sxs-lookup"><span data-stu-id="021fa-196">The [AppContext.BaseDirectory](xref:System.AppContext.BaseDirectory*) property is used to construct a path to the XML file.</span></span> <span data-ttu-id="021fa-197">Niektóre funkcje struktury Swagger (na przykład schematu parametrów wejściowych lub metod HTTP i kodów odpowiedzi z odpowiednich atrybutów) działają bez użycia pliku dokumentacji XML.</span><span class="sxs-lookup"><span data-stu-id="021fa-197">Some Swagger features (for example, schemata of input parameters or HTTP methods and response codes from the respective attributes) work without the use of an XML documentation file.</span></span> <span data-ttu-id="021fa-198">W przypadku większości funkcji, a mianowicie podsumowania metod i opisów parametrów i kodów odpowiedzi, użycie pliku XML jest obowiązkowe.</span><span class="sxs-lookup"><span data-stu-id="021fa-198">For most features, namely method summaries and the descriptions of parameters and response codes, the use of an XML file is mandatory.</span></span>

<span data-ttu-id="021fa-199">Dodawanie do akcji komentarzy z potrójnym ukośnikiem ulepsza wyniki narzędzia Swagger UI przez dodanie opisu do nagłówka sekcji.</span><span class="sxs-lookup"><span data-stu-id="021fa-199">Adding triple-slash comments to an action enhances the Swagger UI by adding the description to the section header.</span></span> <span data-ttu-id="021fa-200">Dodaj [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element powyżej `Delete` akcji:</span><span class="sxs-lookup"><span data-stu-id="021fa-200">Add a [\<summary>](/dotnet/csharp/programming-guide/xmldoc/summary) element above the `Delete` action:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Delete&highlight=1-3)]

<span data-ttu-id="021fa-201">Interfejs użytkownika struktury Swagger wyświetla tekst wewnętrzny elementu poprzedniego kodu `<summary>` :</span><span class="sxs-lookup"><span data-stu-id="021fa-201">The Swagger UI displays the inner text of the preceding code's `<summary>` element:</span></span>

![Interfejs użytkownika struktury Swagger pokazujący komentarz XML "Usuwa określony TodoItem".](web-api-help-pages-using-swagger/_static/triple-slash-comments.png)

<span data-ttu-id="021fa-204">Interfejs użytkownika jest oparty na wygenerowanym schemacie JSON:</span><span class="sxs-lookup"><span data-stu-id="021fa-204">The UI is driven by the generated JSON schema:</span></span>

```json
"delete": {
    "tags": [
        "Todo"
    ],
    "summary": "Deletes a specific TodoItem.",
    "operationId": "ApiTodoByIdDelete",
    "consumes": [],
    "produces": [],
    "parameters": [
        {
            "name": "id",
            "in": "path",
            "description": "",
            "required": true,
            "type": "integer",
            "format": "int64"
        }
    ],
    "responses": {
        "200": {
            "description": "Success"
        }
    }
}
```

<span data-ttu-id="021fa-205">Dodaj [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element do `Create` dokumentacji metody akcji.</span><span class="sxs-lookup"><span data-stu-id="021fa-205">Add a [\<remarks>](/dotnet/csharp/programming-guide/xmldoc/remarks) element to the `Create` action method documentation.</span></span> <span data-ttu-id="021fa-206">Uzupełnia on informacje określone w `<summary>` elemencie i zapewnia bardziej niezawodny interfejs użytkownika struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="021fa-206">It supplements information specified in the `<summary>` element and provides a more robust Swagger UI.</span></span> <span data-ttu-id="021fa-207">`<remarks>`Zawartość elementu może składać się z tekstu, JSON lub XML.</span><span class="sxs-lookup"><span data-stu-id="021fa-207">The `<remarks>` element content can consist of text, JSON, or XML.</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/3.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=4-14)]

::: moniker-end

<span data-ttu-id="021fa-208">Zwróć uwagę na ulepszenia interfejsu użytkownika z następującymi dodatkowymi komentarzami:</span><span class="sxs-lookup"><span data-stu-id="021fa-208">Notice the UI enhancements with these additional comments:</span></span>

![Interfejs użytkownika struktury Swagger z pokazanymi dodatkowymi komentarzami](web-api-help-pages-using-swagger/_static/xml-comments-extended.png)

### <a name="data-annotations"></a><span data-ttu-id="021fa-210">Adnotacje danych</span><span class="sxs-lookup"><span data-stu-id="021fa-210">Data annotations</span></span>

<span data-ttu-id="021fa-211">Oznacz model atrybutami, które znajdują się w przestrzeni nazw [System. ComponentModel. DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) , aby ułatwić DYSKOM interfejsu użytkownika struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="021fa-211">Mark the model with attributes, found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace, to help drive the Swagger UI components.</span></span>

<span data-ttu-id="021fa-212">Dodaj `[Required]` atrybut do `Name` właściwości `TodoItem` klasy:</span><span class="sxs-lookup"><span data-stu-id="021fa-212">Add the `[Required]` attribute to the `Name` property of the `TodoItem` class:</span></span>

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Models/TodoItem.cs?highlight=10)]

<span data-ttu-id="021fa-213">Obecność tego atrybutu zmienia zachowanie interfejsu użytkownika i zmienia źródłowy schemat JSON:</span><span class="sxs-lookup"><span data-stu-id="021fa-213">The presence of this attribute changes the UI behavior and alters the underlying JSON schema:</span></span>

```json
"definitions": {
    "TodoItem": {
        "required": [
            "name"
        ],
        "type": "object",
        "properties": {
            "id": {
                "format": "int64",
                "type": "integer"
            },
            "name": {
                "type": "string"
            },
            "isComplete": {
                "default": false,
                "type": "boolean"
            }
        }
    }
},
```

<span data-ttu-id="021fa-214">Dodaj `[Produces("application/json")]` atrybut do kontrolera interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="021fa-214">Add the `[Produces("application/json")]` attribute to the API controller.</span></span> <span data-ttu-id="021fa-215">Celem jest zadeklarowanie, że działania kontrolera obsługują typ zawartości odpowiedzi dla *aplikacji/JSON*:</span><span class="sxs-lookup"><span data-stu-id="021fa-215">Its purpose is to declare that the controller's actions support a response content type of *application/json*:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/3.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_TodoController&highlight=1)]

::: moniker-end

<span data-ttu-id="021fa-216">Lista rozwijana **Typ zawartości odpowiedzi** wybiera ten typ zawartości jako domyślny dla akcji Get kontrolera:</span><span class="sxs-lookup"><span data-stu-id="021fa-216">The **Response Content Type** drop-down selects this content type as the default for the controller's GET actions:</span></span>

![Interfejs użytkownika struktury Swagger z domyślnym typem zawartości odpowiedzi](web-api-help-pages-using-swagger/_static/json-response-content-type.png)

<span data-ttu-id="021fa-218">W miarę wzrostu użycia adnotacji danych w interfejsie API sieci Web strony interfejsu użytkownika i interfejsu API stają się bardziej opisowe i przydatne.</span><span class="sxs-lookup"><span data-stu-id="021fa-218">As the usage of data annotations in the web API increases, the UI and API help pages become more descriptive and useful.</span></span>

### <a name="describe-response-types"></a><span data-ttu-id="021fa-219">Opisz typy odpowiedzi</span><span class="sxs-lookup"><span data-stu-id="021fa-219">Describe response types</span></span>

<span data-ttu-id="021fa-220">Deweloperzy korzystający z internetowego interfejsu API są najbardziej zainteresowani, które są zwracane w &mdash; szczególności typy odpowiedzi i kody błędów (jeśli nie są standardem).</span><span class="sxs-lookup"><span data-stu-id="021fa-220">Developers consuming a web API are most concerned with what's returned&mdash;specifically response types and error codes (if not standard).</span></span> <span data-ttu-id="021fa-221">Typy odpowiedzi i kody błędów są oznaczane w komentarzach XML i adnotacjach danych.</span><span class="sxs-lookup"><span data-stu-id="021fa-221">The response types and error codes are denoted in the XML comments and data annotations.</span></span>

<span data-ttu-id="021fa-222">`Create`Akcja zwraca kod stanu HTTP 201 na sukcesie.</span><span class="sxs-lookup"><span data-stu-id="021fa-222">The `Create` action returns an HTTP 201 status code on success.</span></span> <span data-ttu-id="021fa-223">Kod stanu HTTP 400 jest zwracany, gdy treść ogłoszonego żądania ma wartość null.</span><span class="sxs-lookup"><span data-stu-id="021fa-223">An HTTP 400 status code is returned when the posted request body is null.</span></span> <span data-ttu-id="021fa-224">Bez odpowiedniej dokumentacji w interfejsie użytkownika programu Swagger klient nie ma wiedzy na temat oczekiwanych wyników.</span><span class="sxs-lookup"><span data-stu-id="021fa-224">Without proper documentation in the Swagger UI, the consumer lacks knowledge of these expected outcomes.</span></span> <span data-ttu-id="021fa-225">Rozwiąż ten problem, dodając wyróżnione wiersze w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="021fa-225">Fix that problem by adding the highlighted lines in the following example:</span></span>

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/3.0/TodoApi.Swashbuckle/Controllers/TodoController.cs?name=snippet_Create&highlight=17,18,20,21)]

::: moniker-end

<span data-ttu-id="021fa-226">Interfejs użytkownika struktury Swagger teraz jasno dokumentuje oczekiwane kody odpowiedzi HTTP:</span><span class="sxs-lookup"><span data-stu-id="021fa-226">The Swagger UI now clearly documents the expected HTTP response codes:</span></span>

![Interfejs użytkownika struktury Swagger pokazujący opis klasy odpowiedzi "zwraca nowo utworzony element zadania" i "400-Jeśli element ma wartość null" dla kodu stanu i przyczyny w komunikatach odpowiedzi](web-api-help-pages-using-swagger/_static/data-annotations-response-types.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="021fa-228">W ASP.NET Core 2,2 lub nowszych Konwencji mogą służyć jako alternatywa dla jawnego dekorowania nazwy poszczególnych akcji z `[ProducesResponseType]` .</span><span class="sxs-lookup"><span data-stu-id="021fa-228">In ASP.NET Core 2.2 or later, conventions can be used as an alternative to explicitly decorating individual actions with `[ProducesResponseType]`.</span></span> <span data-ttu-id="021fa-229">Aby uzyskać więcej informacji, zobacz <xref:web-api/advanced/conventions>.</span><span class="sxs-lookup"><span data-stu-id="021fa-229">For more information, see <xref:web-api/advanced/conventions>.</span></span>

<span data-ttu-id="021fa-230">Aby można było obsłużyć `[ProducesResponseType]` dekorację, pakiet [Swashbuckle. AspNetCore. Annotations](https://github.com/domaindrivendev/Swashbuckle.AspNetCore/blob/master/README.md#swashbuckleaspnetcoreannotations) oferuje rozszerzenia umożliwiające włączenie i wzbogacenie metadanych odpowiedzi, schematu i parametru.</span><span class="sxs-lookup"><span data-stu-id="021fa-230">To support the `[ProducesResponseType]` decoration, the [Swashbuckle.AspNetCore.Annotations](https://github.com/domaindrivendev/Swashbuckle.AspNetCore/blob/master/README.md#swashbuckleaspnetcoreannotations) package offers extensions to enable and enrich the response, schema, and parameter metadata.</span></span>

::: moniker-end

### <a name="customize-the-ui"></a><span data-ttu-id="021fa-231">Dostosowywanie interfejsu użytkownika</span><span class="sxs-lookup"><span data-stu-id="021fa-231">Customize the UI</span></span>

<span data-ttu-id="021fa-232">Domyślny interfejs użytkownika jest zarówno funkcjonalny, jak i najbardziej do wysłania.</span><span class="sxs-lookup"><span data-stu-id="021fa-232">The default UI is both functional and presentable.</span></span> <span data-ttu-id="021fa-233">Jednak strony dokumentacji interfejsu API powinny reprezentować swoją markę lub motyw.</span><span class="sxs-lookup"><span data-stu-id="021fa-233">However, API documentation pages should represent your brand or theme.</span></span> <span data-ttu-id="021fa-234">Znakowanie składników Swashbuckle wymaga dodania zasobów do obsługi plików statycznych i skompilowania struktury folderów do hostowania tych plików.</span><span class="sxs-lookup"><span data-stu-id="021fa-234">Branding the Swashbuckle components requires adding the resources to serve static files and building the folder structure to host those files.</span></span>

<span data-ttu-id="021fa-235">Jeśli obiektem docelowym jest .NET Framework lub .NET Core 1. x, Dodaj pakiet NuGet [Microsoft. AspNetCore. StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) do projektu:</span><span class="sxs-lookup"><span data-stu-id="021fa-235">If targeting .NET Framework or .NET Core 1.x, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles) NuGet package to the project:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="2.0.0" />
```

<span data-ttu-id="021fa-236">Poprzedni pakiet NuGet jest już zainstalowany, jeśli celem jest .NET Core 2. x i używanie [pakietu](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="021fa-236">The preceding NuGet package is already installed if targeting .NET Core 2.x and using the [metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="021fa-237">Włącz oprogramowanie pośredniczące plików statycznych:</span><span class="sxs-lookup"><span data-stu-id="021fa-237">Enable Static File Middleware:</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/2.1/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](../tutorials/web-api-help-pages-using-swagger/samples/3.0/TodoApi.Swashbuckle/Startup.cs?name=snippet_Configure&highlight=3)]

::: moniker-end

<span data-ttu-id="021fa-238">Aby wstrzyknąć dodatkowe arkusze stylów CSS, Dodaj je do folderu *wwwroot* projektu i określ ścieżkę względną w opcjach oprogramowania pośredniczącego:</span><span class="sxs-lookup"><span data-stu-id="021fa-238">To inject additional CSS stylesheets, add them to the project's *wwwroot* folder and specify the relative path in the middleware options:</span></span>

```csharp
app.UseSwaggerUI(c =>
{
     c.InjectStylesheet("/swagger-ui/custom.css");
}
```

## <a name="additional-resources"></a><span data-ttu-id="021fa-239">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="021fa-239">Additional resources</span></span>

* [<span data-ttu-id="021fa-240">Microsoft Learn: ulepszanie środowiska deweloperskiego interfejsu API za pomocą dokumentacji programu Swagger</span><span class="sxs-lookup"><span data-stu-id="021fa-240">Microsoft Learn: Improve the developer experience of an API with Swagger documentation</span></span>](/learn/modules/improve-api-developer-experience-with-swagger/)
