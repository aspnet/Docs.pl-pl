---
title: Routing do akcji kontrolera w ASP.NET Core
author: rick-anderson
description: Dowiedz się, w jaki sposób ASP.NET Core MVC używa programów pośredniczących routingu, aby dopasować adresy URL żądań przychodzących i zmapować je na akcje.
ms.author: riande
ms.date: 3/25/2020
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
uid: mvc/controllers/routing
ms.openlocfilehash: c90ac92cf0866b78bcdb11663e76c116d714ca5d
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102589065"
---
# <a name="routing-to-controller-actions-in-aspnet-core"></a><span data-ttu-id="88f8a-103">Routing do akcji kontrolera w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="88f8a-103">Routing to controller actions in ASP.NET Core</span></span>

<span data-ttu-id="88f8a-104">[Ryan Nowak](https://github.com/rynowak), [Kirka Larkin](https://twitter.com/serpent5)i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="88f8a-104">By [Ryan Nowak](https://github.com/rynowak), [Kirk Larkin](https://twitter.com/serpent5), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="88f8a-105">Kontrolery ASP.NET Core używają [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) routingu do dopasowywania adresów URL żądań przychodzących i mapują je na [Akcje](#action).</span><span class="sxs-lookup"><span data-stu-id="88f8a-105">ASP.NET Core controllers use the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to [actions](#action).</span></span>  <span data-ttu-id="88f8a-106">Szablony tras:</span><span class="sxs-lookup"><span data-stu-id="88f8a-106">Route templates:</span></span>

* <span data-ttu-id="88f8a-107">Są zdefiniowane w kodzie startowym lub atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-107">Are defined in startup code or attributes.</span></span>
* <span data-ttu-id="88f8a-108">Opisz, w jaki sposób ścieżki URL są dopasowywane do [akcji](#action).</span><span class="sxs-lookup"><span data-stu-id="88f8a-108">Describe how URL paths are matched to [actions](#action).</span></span>
* <span data-ttu-id="88f8a-109">Służy do generowania adresów URL dla łączy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-109">Are used to generate URLs for links.</span></span> <span data-ttu-id="88f8a-110">Wygenerowane linki są zwykle zwracane w odpowiedziach.</span><span class="sxs-lookup"><span data-stu-id="88f8a-110">The generated links are typically returned in responses.</span></span>

<span data-ttu-id="88f8a-111">Akcje są albo [podkierowane do Konwencji](#cr) lub [kierowane przez atrybut](#ar).</span><span class="sxs-lookup"><span data-stu-id="88f8a-111">Actions are either [conventionally-routed](#cr) or [attribute-routed](#ar).</span></span> <span data-ttu-id="88f8a-112">Umieszczenie trasy na kontrolerze lub [akcji](#action) sprawia, że jest ona kierowana przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="88f8a-112">Placing a route on the controller or [action](#action) makes it attribute-routed.</span></span> <span data-ttu-id="88f8a-113">Aby uzyskać więcej informacji, zobacz [Routing mieszany](#routing-mixed-ref-label) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-113">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="88f8a-114">Ten dokument:</span><span class="sxs-lookup"><span data-stu-id="88f8a-114">This document:</span></span>

* <span data-ttu-id="88f8a-115">Wyjaśnia interakcje między MVC i Routing:</span><span class="sxs-lookup"><span data-stu-id="88f8a-115">Explains the interactions between MVC and routing:</span></span>
  * <span data-ttu-id="88f8a-116">Jak typowe aplikacje MVC używają funkcji routingu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-116">How typical MVC apps make use of routing features.</span></span>
  * <span data-ttu-id="88f8a-117">Obejmuje zarówno:</span><span class="sxs-lookup"><span data-stu-id="88f8a-117">Covers both:</span></span>
    * <span data-ttu-id="88f8a-118">[Konwencjonalne Routing](#cr) zazwyczaj jest używany z kontrolerami i widokami.</span><span class="sxs-lookup"><span data-stu-id="88f8a-118">[Conventional routing](#cr) typically used with controllers and views.</span></span>
    * <span data-ttu-id="88f8a-119">*Routing atrybutów* używany z interfejsami API REST.</span><span class="sxs-lookup"><span data-stu-id="88f8a-119">*Attribute routing* used with REST APIs.</span></span> <span data-ttu-id="88f8a-120">Jeśli jesteś głównie zainteresowani routingiem interfejsów API REST, przejdź do sekcji [Routing atrybutów dla interfejsów API REST](#ar) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-120">If you're primarily interested in routing for REST APIs, jump to the [Attribute routing for REST APIs](#ar) section.</span></span>
  * <span data-ttu-id="88f8a-121">Szczegóły dotyczące routingu zaawansowanego znajdują się w temacie [Routing](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-121">See [Routing](xref:fundamentals/routing) for advanced routing details.</span></span>
* <span data-ttu-id="88f8a-122">Odnosi się do domyślnego systemu routingu dodanego w ASP.NET Core 3,0, nazywanego routingiem punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="88f8a-122">Refers to the default routing system added in ASP.NET Core 3.0, called endpoint routing.</span></span> <span data-ttu-id="88f8a-123">Możliwe jest używanie kontrolerów z poprzednią wersją routingu w celu zapewnienia zgodności.</span><span class="sxs-lookup"><span data-stu-id="88f8a-123">It's possible to use controllers with the previous version of routing for compatibility purposes.</span></span> <span data-ttu-id="88f8a-124">Instrukcje można znaleźć w [przewodniku migracji 2.2-3.0](xref:migration/22-to-30) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-124">See the [2.2-3.0 migration guide](xref:migration/22-to-30) for instructions.</span></span> <span data-ttu-id="88f8a-125">Zapoznaj się z [wersją 2,2 tego dokumentu](xref:mvc/controllers/routing?view=aspnetcore-2.2) dotyczącą materiałów referencyjnych w starszym systemie routingu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-125">Refer to the [2.2 version of this document](xref:mvc/controllers/routing?view=aspnetcore-2.2) for reference material on the legacy routing system.</span></span>

<a name="cr"></a>

## <a name="set-up-conventional-route"></a><span data-ttu-id="88f8a-126">Konfigurowanie trasy konwencjonalnej</span><span class="sxs-lookup"><span data-stu-id="88f8a-126">Set up conventional route</span></span>

<span data-ttu-id="88f8a-127">`Startup.Configure` w przypadku korzystania z [konwencjonalnego routingu](#crd)zazwyczaj ma kod podobny do poniższego:</span><span class="sxs-lookup"><span data-stu-id="88f8a-127">`Startup.Configure` typically has code similar to the following when using [conventional routing](#crd):</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet)]

<span data-ttu-id="88f8a-128">Wewnątrz wywołania do <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints%2A> , służy <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute%2A> do tworzenia pojedynczej trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-128">Inside the call to <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints%2A>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute%2A> is used to create a single route.</span></span> <span data-ttu-id="88f8a-129">Pojedyncza trasa ma nazwę `default` Route.</span><span class="sxs-lookup"><span data-stu-id="88f8a-129">The single route is named `default` route.</span></span> <span data-ttu-id="88f8a-130">Większość aplikacji z kontrolerami i widokami używa szablonu trasy podobnego do `default` trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-130">Most apps with controllers and views use a route template similar to the `default` route.</span></span> <span data-ttu-id="88f8a-131">Interfejsy API REST powinny używać [routingu atrybutów](#ar).</span><span class="sxs-lookup"><span data-stu-id="88f8a-131">REST APIs should use [attribute routing](#ar).</span></span>

<span data-ttu-id="88f8a-132">Szablon trasy `"{controller=Home}/{action=Index}/{id?}"` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-132">The route template `"{controller=Home}/{action=Index}/{id?}"`:</span></span>

* <span data-ttu-id="88f8a-133">Dopasowuje ścieżkę URL, taką jak `/Products/Details/5`</span><span class="sxs-lookup"><span data-stu-id="88f8a-133">Matches a URL path like `/Products/Details/5`</span></span>
* <span data-ttu-id="88f8a-134">Wyodrębnia wartości trasy `{ controller = Products, action = Details, id = 5 }` przez tokenizowanie ścieżki.</span><span class="sxs-lookup"><span data-stu-id="88f8a-134">Extracts the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="88f8a-135">Wyodrębnienie wartości tras powoduje dopasowanie, jeśli aplikacja ma kontroler o nazwie `ProductsController` i `Details` akcję:</span><span class="sxs-lookup"><span data-stu-id="88f8a-135">The extraction of route values results in a match if the app has a controller named `ProductsController` and a `Details` action:</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippetA)]

  [!INCLUDE[](~/includes/MyDisplayRouteInfo.md)]

* <span data-ttu-id="88f8a-136">`/Products/Details/5` Model wiąże wartość, `id = 5` Aby ustawić parametr jako `id` `5` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-136">`/Products/Details/5` model binds the value of `id = 5` to set the `id` parameter to `5`.</span></span> <span data-ttu-id="88f8a-137">Aby uzyskać więcej informacji, zobacz [powiązanie modelu](xref:mvc/models/model-binding) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-137">See [Model Binding](xref:mvc/models/model-binding) for more details.</span></span>
* <span data-ttu-id="88f8a-138">`{controller=Home}` definiuje `Home` jako domyślny `controller` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-138">`{controller=Home}` defines `Home` as the default `controller`.</span></span>
* <span data-ttu-id="88f8a-139">`{action=Index}` definiuje `Index` jako domyślny `action` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-139">`{action=Index}` defines `Index` as the default `action`.</span></span>
*  <span data-ttu-id="88f8a-140">`?`Znak w `{id?}` definiuje `id` jako opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="88f8a-140">The `?` character in `{id?}` defines `id` as optional.</span></span>
  * <span data-ttu-id="88f8a-141">Domyślne i opcjonalne parametry trasy nie muszą być obecne w ścieżce URL dla dopasowania.</span><span class="sxs-lookup"><span data-stu-id="88f8a-141">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="88f8a-142">Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](xref:fundamentals/routing#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-142">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>
* <span data-ttu-id="88f8a-143">Dopasowuje ścieżkę URL `/` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-143">Matches the URL path `/`.</span></span>
* <span data-ttu-id="88f8a-144">Tworzy wartości trasy `{ controller = Home, action = Index }` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-144">Produces the route values `{ controller = Home, action = Index }`.</span></span>

<span data-ttu-id="88f8a-145">Wartości dla `controller` i `action` używają wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="88f8a-145">The values for `controller` and `action` make use of the default values.</span></span> <span data-ttu-id="88f8a-146">`id` nie produkuje wartości, ponieważ nie ma odpowiedniego segmentu w ścieżce adresu URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-146">`id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="88f8a-147">`/` dopasowuje się tylko wtedy, gdy istnieje `HomeController` `Index` Akcja i:</span><span class="sxs-lookup"><span data-stu-id="88f8a-147">`/` only matches if there exists a `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="88f8a-148">Przy użyciu definicji kontrolera i szablonu trasy, `HomeController.Index` Akcja jest uruchamiana dla następujących ścieżek URL:</span><span class="sxs-lookup"><span data-stu-id="88f8a-148">Using the preceding controller definition and route template, the `HomeController.Index` action is run for the following URL paths:</span></span>

* `/Home/Index/17`
* `/Home/Index`
* `/Home`
* `/`

<span data-ttu-id="88f8a-149">Ścieżka adresu URL `/` używa domyślnych `Home` kontrolerów i akcji szablonu trasy `Index` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-149">The URL path `/` uses the route template default `Home` controllers and `Index` action.</span></span> <span data-ttu-id="88f8a-150">Ścieżka adresu URL `/Home` używa domyślnej akcji szablonu trasy `Index` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-150">The URL path `/Home` uses the route template default `Index` action.</span></span>

<span data-ttu-id="88f8a-151">Wygodna metoda <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute%2A> :</span><span class="sxs-lookup"><span data-stu-id="88f8a-151">The convenience method <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute%2A>:</span></span>

```csharp
endpoints.MapDefaultControllerRoute();
```

<span data-ttu-id="88f8a-152">Zastępując</span><span class="sxs-lookup"><span data-stu-id="88f8a-152">Replaces:</span></span>

```csharp
endpoints.MapControllerRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

> [!IMPORTANT]
> <span data-ttu-id="88f8a-153">Routing jest konfigurowany przy użyciu programu <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting%2A> i <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints%2A> oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="88f8a-153">Routing is configured using the <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting%2A> and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints%2A> middleware.</span></span> <span data-ttu-id="88f8a-154">Aby użyć kontrolerów:</span><span class="sxs-lookup"><span data-stu-id="88f8a-154">To use controllers:</span></span>
>
> * <span data-ttu-id="88f8a-155">Wywołaj <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers%2A> wewnątrz `UseEndpoints` , aby zmapować kontrolery z [routingiem atrybutów](#ar) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-155">Call <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers%2A> inside `UseEndpoints` to map [attribute routed](#ar) controllers.</span></span>
> * <span data-ttu-id="88f8a-156">Wywołaj <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute%2A> lub <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute%2A> , aby zamapować zarówno kontrolery z [Konwencją routingu](#cr) , jak i kontrolery [przypisane do atrybutów](#ar) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-156">Call <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute%2A> or <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute%2A>, to map both [conventionally routed](#cr) controllers and [attribute routed](#ar) controllers.</span></span>

<a name="routing-conventional-ref-label"></a>
<a name="crd"></a>

## <a name="conventional-routing"></a><span data-ttu-id="88f8a-157">Routing konwencjonalny</span><span class="sxs-lookup"><span data-stu-id="88f8a-157">Conventional routing</span></span>

<span data-ttu-id="88f8a-158">Routing konwencjonalny jest używany z kontrolerami i widokami.</span><span class="sxs-lookup"><span data-stu-id="88f8a-158">Conventional routing is used with controllers and views.</span></span> <span data-ttu-id="88f8a-159">`default`Trasa:</span><span class="sxs-lookup"><span data-stu-id="88f8a-159">The `default` route:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupDefaultMVC.cs?name=snippet2)]

<span data-ttu-id="88f8a-160">Powyższym elementem jest przykład *trasy konwencjonalnej*.</span><span class="sxs-lookup"><span data-stu-id="88f8a-160">The preceding is an example of a *conventional route*.</span></span> <span data-ttu-id="88f8a-161">Jest on nazywany *konwencjonalnym routingiem* , ponieważ ustanawia *Konwencję* dla ścieżek adresów URL:</span><span class="sxs-lookup"><span data-stu-id="88f8a-161">It's called *conventional routing* because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="88f8a-162">Pierwszy segment ścieżki, `{controller=Home}` mapuje na nazwę kontrolera.</span><span class="sxs-lookup"><span data-stu-id="88f8a-162">The first path segment, `{controller=Home}`, maps to the controller name.</span></span>
* <span data-ttu-id="88f8a-163">Drugi segment, `{action=Index}` , mapuje na nazwę [akcji](#action) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-163">The second segment, `{action=Index}`, maps to the [action](#action) name.</span></span>
* <span data-ttu-id="88f8a-164">Trzeci segment `{id?}` jest używany jako opcjonalny `id` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-164">The third segment, `{id?}` is used for an optional `id`.</span></span> <span data-ttu-id="88f8a-165">`?`W programie w programie `{id?}` jest to opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="88f8a-165">The `?` in `{id?}` makes it optional.</span></span> <span data-ttu-id="88f8a-166">`id` służy do mapowania na jednostkę modelu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-166">`id` is used to map to a model entity.</span></span>

<span data-ttu-id="88f8a-167">Przy użyciu tej `default` trasy ścieżka URL:</span><span class="sxs-lookup"><span data-stu-id="88f8a-167">Using this `default` route, the URL path:</span></span>

* <span data-ttu-id="88f8a-168">`/Products/List` mapuje do `ProductsController.List` akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-168">`/Products/List` maps to the `ProductsController.List` action.</span></span>
* <span data-ttu-id="88f8a-169">`/Blog/Article/17` mapowanie na `BlogController.Article` model i zwykle wiąże się z `id` parametrem 17.</span><span class="sxs-lookup"><span data-stu-id="88f8a-169">`/Blog/Article/17` maps to `BlogController.Article` and typically model binds the `id` parameter to 17.</span></span>

<span data-ttu-id="88f8a-170">To mapowanie:</span><span class="sxs-lookup"><span data-stu-id="88f8a-170">This mapping:</span></span>

* <span data-ttu-id="88f8a-171">Jest oparty **tylko** na nazwach kontrolera i [akcji](#action) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-171">Is based on the controller and [action](#action) names **only**.</span></span>
* <span data-ttu-id="88f8a-172">Nie jest oparty na przestrzeniach nazw, lokalizacjach plików źródłowych ani parametrach metod.</span><span class="sxs-lookup"><span data-stu-id="88f8a-172">Isn't based on namespaces, source file locations, or method parameters.</span></span>

<span data-ttu-id="88f8a-173">Użycie konwencjonalnego routingu z domyślną trasą umożliwia tworzenie aplikacji bez konieczności korzystania z nowego wzorca adresu URL dla każdej akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-173">Using conventional routing with the default route allows creating the app without having to come up with a new URL pattern for each action.</span></span> <span data-ttu-id="88f8a-174">W przypadku aplikacji z akcjami w stylu [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) mających spójność dla adresów URL między kontrolerami:</span><span class="sxs-lookup"><span data-stu-id="88f8a-174">For an app with [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) style actions, having consistency for the URLs across controllers:</span></span>

* <span data-ttu-id="88f8a-175">Upraszcza kod.</span><span class="sxs-lookup"><span data-stu-id="88f8a-175">Helps simplify the code.</span></span>
* <span data-ttu-id="88f8a-176">Sprawia, że interfejs użytkownika jest bardziej przewidywalny.</span><span class="sxs-lookup"><span data-stu-id="88f8a-176">Makes the UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="88f8a-177">`id`W powyższym kodzie jest zdefiniowany jako opcjonalny przez szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-177">The `id` in the preceding code is defined as optional by the route template.</span></span> <span data-ttu-id="88f8a-178">Akcje można wykonać bez opcjonalnego identyfikatora podanego w ramach adresu URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-178">Actions can execute without the optional ID provided as part of the URL.</span></span> <span data-ttu-id="88f8a-179">Ogólnie mówiąc, gdy `id` pominięto w adresie URL:</span><span class="sxs-lookup"><span data-stu-id="88f8a-179">Generally, when`id` is omitted from the URL:</span></span>
>
> * <span data-ttu-id="88f8a-180">`id` jest ustawiony na `0` przez powiązanie modelu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-180">`id` is set to `0` by model binding.</span></span>
> * <span data-ttu-id="88f8a-181">Nie znaleziono jednostki w dopasowaniu do bazy danych `id == 0` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-181">No entity is found in the database matching `id == 0`.</span></span>
>
> <span data-ttu-id="88f8a-182">[Routing atrybutu](#ar) zawiera szczegółowy formant, który umożliwia określanie identyfikatora wymaganego dla niektórych akcji, a nie dla innych.</span><span class="sxs-lookup"><span data-stu-id="88f8a-182">[Attribute routing](#ar) provides fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="88f8a-183">Zgodnie z Konwencją, dokumentacja zawiera opcjonalne parametry, takie jak, `id` gdy prawdopodobnie pojawi się w prawidłowym użyciu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-183">By convention, the documentation includes optional parameters like `id` when they're likely to appear in correct usage.</span></span>

<span data-ttu-id="88f8a-184">Większość aplikacji powinna wybrać podstawowy i opisowy schemat routingu, aby adresy URL były czytelne i zrozumiałe.</span><span class="sxs-lookup"><span data-stu-id="88f8a-184">Most apps should choose a basic and descriptive routing scheme so that URLs are readable and meaningful.</span></span> <span data-ttu-id="88f8a-185">Domyślna trasa konwencjonalna `{controller=Home}/{action=Index}/{id?}` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-185">The default conventional route `{controller=Home}/{action=Index}/{id?}`:</span></span>

* <span data-ttu-id="88f8a-186">Obsługuje podstawowy i opisowy schemat routingu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-186">Supports a basic and descriptive routing scheme.</span></span>
* <span data-ttu-id="88f8a-187">Jest użytecznym punktem wyjścia dla aplikacji opartych na interfejsie użytkownika.</span><span class="sxs-lookup"><span data-stu-id="88f8a-187">Is a useful starting point for UI-based apps.</span></span>
* <span data-ttu-id="88f8a-188">Jest jedynym szablonem trasy wymaganym przez wiele aplikacji interfejsu użytkownika sieci Web.</span><span class="sxs-lookup"><span data-stu-id="88f8a-188">Is the only route template needed for many web UI apps.</span></span> <span data-ttu-id="88f8a-189">W przypadku większych aplikacji interfejsu użytkownika sieci Web inna trasa przy użyciu [obszarów](#areas) jest często wymagana.</span><span class="sxs-lookup"><span data-stu-id="88f8a-189">For larger web UI apps, another route using [Areas](#areas) is frequently all that's needed.</span></span>

<span data-ttu-id="88f8a-190"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute%2A> i <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute%2A> :</span><span class="sxs-lookup"><span data-stu-id="88f8a-190"><xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute%2A> and <xref:Microsoft.AspNetCore.Builder.MvcAreaRouteBuilderExtensions.MapAreaRoute%2A> :</span></span>

* <span data-ttu-id="88f8a-191">Automatycznie Przypisz wartość **zamówienia** do punktów końcowych na podstawie kolejności, w której są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="88f8a-191">Automatically assign an **order** value to their endpoints based on the order they are invoked.</span></span>

<span data-ttu-id="88f8a-192">Routing punktów końcowych w ASP.NET Core 3,0 i nowszych:</span><span class="sxs-lookup"><span data-stu-id="88f8a-192">Endpoint routing in ASP.NET Core 3.0 and later:</span></span>

* <span data-ttu-id="88f8a-193">Nie ma koncepcji tras.</span><span class="sxs-lookup"><span data-stu-id="88f8a-193">Doesn't have a concept of routes.</span></span>
* <span data-ttu-id="88f8a-194">Nie oferuje gwarancji porządkowania dla wykonywania rozszerzalności, wszystkie punkty końcowe są przetwarzane jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-194">Doesn't provide ordering guarantees for the execution of extensibility,  all endpoints are processed at once.</span></span>

<span data-ttu-id="88f8a-195">Włącz [Rejestrowanie](xref:fundamentals/logging/index) , aby zobaczyć, jak wbudowane implementacje routingu, takie jak <xref:Microsoft.AspNetCore.Routing.Route> , dopasowują żądania.</span><span class="sxs-lookup"><span data-stu-id="88f8a-195">Enable [Logging](xref:fundamentals/logging/index) to see how the built-in routing implementations, such as <xref:Microsoft.AspNetCore.Routing.Route>, match requests.</span></span>

<span data-ttu-id="88f8a-196">[Routing atrybutów](#ar) został wyjaśniony w dalszej części tego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-196">[Attribute routing](#ar) is explained later in this document.</span></span>

<a name="mr"></a>

### <a name="multiple-conventional-routes"></a><span data-ttu-id="88f8a-197">Wiele konwencjonalnych tras</span><span class="sxs-lookup"><span data-stu-id="88f8a-197">Multiple conventional routes</span></span>

<span data-ttu-id="88f8a-198">Wewnątrz można dodać wiele [konwencjonalnych tras](#cr) `UseEndpoints` , dodając więcej wywołań do <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute%2A> i <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute%2A> .</span><span class="sxs-lookup"><span data-stu-id="88f8a-198">Multiple [conventional routes](#cr) can be added inside `UseEndpoints` by adding more calls to <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute%2A> and <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute%2A>.</span></span> <span data-ttu-id="88f8a-199">Dzięki temu można definiować wiele Konwencji lub dodać do nich trasy konwencjonalne, które są przeznaczone dla konkretnej [akcji](#action), na przykład:</span><span class="sxs-lookup"><span data-stu-id="88f8a-199">Doing so allows defining multiple conventions, or to adding conventional routes that are dedicated to a specific [action](#action), such as:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<a name="dcr"></a>

<span data-ttu-id="88f8a-200">`blog`Trasa w powyższym kodzie jest **dedykowaną konwencjonalną trasą**.</span><span class="sxs-lookup"><span data-stu-id="88f8a-200">The `blog` route in the preceding code is a **dedicated conventional route**.</span></span> <span data-ttu-id="88f8a-201">Jest on nazywany dedykowaną trasą konwencjonalną, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="88f8a-201">It's called a dedicated conventional route because:</span></span>

* <span data-ttu-id="88f8a-202">Używa ona [konwencjonalnego routingu](#cr).</span><span class="sxs-lookup"><span data-stu-id="88f8a-202">It uses [conventional routing](#cr).</span></span>
* <span data-ttu-id="88f8a-203">Jest on przeznaczony dla konkretnej [akcji](#action).</span><span class="sxs-lookup"><span data-stu-id="88f8a-203">It's dedicated to a specific [action](#action).</span></span>

<span data-ttu-id="88f8a-204">Ponieważ `controller` i `action` nie pojawiają się w szablonie trasy `"blog/{*article}"` jako parametry:</span><span class="sxs-lookup"><span data-stu-id="88f8a-204">Because `controller` and `action` don't appear in the route template `"blog/{*article}"` as parameters:</span></span>

* <span data-ttu-id="88f8a-205">Mogą mieć tylko wartości domyślne `{ controller = "Blog", action = "Article" }` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-205">They can only have the default values `{ controller = "Blog", action = "Article" }`.</span></span>
* <span data-ttu-id="88f8a-206">Ta trasa zawsze jest mapowana na akcję `BlogController.Article` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-206">This route always maps to the action `BlogController.Article`.</span></span>

<span data-ttu-id="88f8a-207">`/Blog`, `/Blog/Article` i `/Blog/{any-string}` są jedynymi ŚCIEŻKAmi URL, które pasują do trasy blogu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-207">`/Blog`, `/Blog/Article`, and `/Blog/{any-string}` are the only URL paths that match the blog route.</span></span>

<span data-ttu-id="88f8a-208">Powyższy przykład:</span><span class="sxs-lookup"><span data-stu-id="88f8a-208">The preceding example:</span></span>

* <span data-ttu-id="88f8a-209">`blog` trasa ma wyższy priorytet dla dopasowania niż trasa, `default` ponieważ jest dodawana jako pierwsza.</span><span class="sxs-lookup"><span data-stu-id="88f8a-209">`blog` route has a higher priority for matches than the `default` route because it is added first.</span></span>
* <span data-ttu-id="88f8a-210">Jest przykładem routingu stylów [informacji](https://developer.mozilla.org/docs/Glossary/Slug) o miejscu, w którym typowym jest nazwa artykułu w ramach adresu URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-210">Is an example of [Slug](https://developer.mozilla.org/docs/Glossary/Slug) style routing where it's typical to have an article name as part of the URL.</span></span>

> [!WARNING]
> <span data-ttu-id="88f8a-211">W ASP.NET Core 3,0 i nowszych routingu nie są:</span><span class="sxs-lookup"><span data-stu-id="88f8a-211">In ASP.NET Core 3.0 and later, routing doesn't:</span></span>
> * <span data-ttu-id="88f8a-212">Zdefiniuj koncepcję o nazwie *trasa*.</span><span class="sxs-lookup"><span data-stu-id="88f8a-212">Define a concept called a *route*.</span></span> <span data-ttu-id="88f8a-213">`UseRouting` dodaje dopasowanie trasy do potoku programu pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="88f8a-213">`UseRouting` adds route matching to the middleware pipeline.</span></span> <span data-ttu-id="88f8a-214">`UseRouting`Oprogramowanie pośredniczące sprawdza zestaw punktów końcowych zdefiniowanych w aplikacji i wybiera najlepsze dopasowanie punktu końcowego na podstawie żądania.</span><span class="sxs-lookup"><span data-stu-id="88f8a-214">The `UseRouting` middleware looks at the set of endpoints defined in the app, and selects the best endpoint match based on the request.</span></span>
> * <span data-ttu-id="88f8a-215">Podaj gwarancje dotyczące kolejności wykonywania rozszerzalności, takich jak <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> lub <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> .</span><span class="sxs-lookup"><span data-stu-id="88f8a-215">Provide guarantees about the execution order of extensibility like <xref:Microsoft.AspNetCore.Routing.IRouteConstraint> or <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint>.</span></span>
>
><span data-ttu-id="88f8a-216">Zobacz [Routing](xref:fundamentals/routing) dla materiałów referencyjnych na trasie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-216">See [Routing](xref:fundamentals/routing) for reference material on routing.</span></span>

<a name="cro"></a>

### <a name="conventional-routing-order"></a><span data-ttu-id="88f8a-217">Tradycyjna kolejność routingu</span><span class="sxs-lookup"><span data-stu-id="88f8a-217">Conventional routing order</span></span>

<span data-ttu-id="88f8a-218">Routing konwencjonalny pasuje do kombinacji akcji i kontrolera, które są zdefiniowane przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="88f8a-218">Conventional routing only matches a combination of action and controller that are defined by the app.</span></span> <span data-ttu-id="88f8a-219">Jest to przeznaczone do uproszczenia sytuacji, w których trasy konwencjonalne nakładają się na siebie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-219">This is intended to simplify cases where conventional routes overlap.</span></span>
<span data-ttu-id="88f8a-220">Dodanie tras przy użyciu <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute%2A> , <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute%2A> i <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute%2A> automatycznie przypisanie wartości zamówienia do punktów końcowych w oparciu o kolejność, w której są wywoływane.</span><span class="sxs-lookup"><span data-stu-id="88f8a-220">Adding routes using <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute%2A>, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapDefaultControllerRoute%2A>, and <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute%2A> automatically assign an order value to their endpoints based on the order they are invoked.</span></span> <span data-ttu-id="88f8a-221">Dopasowania z trasy, która pojawiła się wcześniej, mają wyższy priorytet.</span><span class="sxs-lookup"><span data-stu-id="88f8a-221">Matches from a route that appears earlier have a higher priority.</span></span> <span data-ttu-id="88f8a-222">Routowanie konwencjonalne jest zależne od kolejności.</span><span class="sxs-lookup"><span data-stu-id="88f8a-222">Conventional routing is order-dependent.</span></span> <span data-ttu-id="88f8a-223">Ogólnie rzecz biorąc, trasy z obszarami powinny być umieszczone wcześniej, ponieważ są bardziej specyficzne niż trasy bez obszaru.</span><span class="sxs-lookup"><span data-stu-id="88f8a-223">In general, routes with areas should be placed earlier as they're more specific than routes without an area.</span></span> <span data-ttu-id="88f8a-224">[Dedykowane konwencjonalne trasy](#dcr) ze wszystkimi parametrami tras, takich jak, `{*article}` mogą spowodować zbyt [zachłanne](xref:fundamentals/routing#greedy)trasy, co oznacza, że pasuje do adresów URL, które mają być dopasowane przez inne trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-224">[Dedicated conventional routes](#dcr) with catch-all route parameters like `{*article}` can make a route too [greedy](xref:fundamentals/routing#greedy), meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="88f8a-225">Umieść trasy zachłanne w dalszej części tabeli tras, aby zapobiec dopasowania zachłanne.</span><span class="sxs-lookup"><span data-stu-id="88f8a-225">Put the greedy routes later in the route table to prevent greedy matches.</span></span>

[!INCLUDE[](~/includes/catchall.md)]

<a name="best"></a>

### <a name="resolving-ambiguous-actions"></a><span data-ttu-id="88f8a-226">Rozwiązywanie niejednoznacznych akcji</span><span class="sxs-lookup"><span data-stu-id="88f8a-226">Resolving ambiguous actions</span></span>

<span data-ttu-id="88f8a-227">Gdy dwa punkty końcowe pasują do routingu, routing musi wykonać jedną z następujących czynności:</span><span class="sxs-lookup"><span data-stu-id="88f8a-227">When two endpoints match through routing, routing must do one of the following:</span></span>

* <span data-ttu-id="88f8a-228">Wybierz najlepszego kandydata.</span><span class="sxs-lookup"><span data-stu-id="88f8a-228">Choose the best candidate.</span></span>
* <span data-ttu-id="88f8a-229">Zgłoś wyjątek.</span><span class="sxs-lookup"><span data-stu-id="88f8a-229">Throw an exception.</span></span>

<span data-ttu-id="88f8a-230">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="88f8a-230">For example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet9)]

<span data-ttu-id="88f8a-231">Poprzedni kontroler definiuje dwie akcje, które są zgodne:</span><span class="sxs-lookup"><span data-stu-id="88f8a-231">The preceding controller defines two actions that match:</span></span>

* <span data-ttu-id="88f8a-232">Ścieżka adresu URL `/Products33/Edit/17`</span><span class="sxs-lookup"><span data-stu-id="88f8a-232">The URL path `/Products33/Edit/17`</span></span>
* <span data-ttu-id="88f8a-233">Kierowanie danych `{ controller = Products33, action = Edit, id = 17 }` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-233">Route data `{ controller = Products33, action = Edit, id = 17 }`.</span></span>

<span data-ttu-id="88f8a-234">Jest to typowy wzorzec dla kontrolerów MVC:</span><span class="sxs-lookup"><span data-stu-id="88f8a-234">This is a typical pattern for MVC controllers:</span></span>

* <span data-ttu-id="88f8a-235">`Edit(int)` Wyświetla formularz, w którym można edytować produkt.</span><span class="sxs-lookup"><span data-stu-id="88f8a-235">`Edit(int)` displays a form to edit a product.</span></span>
* <span data-ttu-id="88f8a-236">`Edit(int, Product)` przetwarza opublikowany formularz.</span><span class="sxs-lookup"><span data-stu-id="88f8a-236">`Edit(int, Product)` processes  the posted form.</span></span>

<span data-ttu-id="88f8a-237">Aby rozwiązać poprawność trasy:</span><span class="sxs-lookup"><span data-stu-id="88f8a-237">To resolve the correct route:</span></span>

* <span data-ttu-id="88f8a-238">`Edit(int, Product)` jest wybierany, gdy żądanie jest HTTP `POST` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-238">`Edit(int, Product)` is selected when the request is an HTTP `POST`.</span></span>
* <span data-ttu-id="88f8a-239">`Edit(int)` jest wybierany, gdy [czasownik http](#verb) jest inny.</span><span class="sxs-lookup"><span data-stu-id="88f8a-239">`Edit(int)` is selected when the [HTTP verb](#verb) is anything else.</span></span> <span data-ttu-id="88f8a-240">`Edit(int)` jest zazwyczaj wywoływany za pośrednictwem `GET` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-240">`Edit(int)` is generally called via `GET`.</span></span>

<span data-ttu-id="88f8a-241"><xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, `[HttpPost]` ,, Jest dostarczany do routingu, aby można było wybrać oparty na metodzie HTTP żądania.</span><span class="sxs-lookup"><span data-stu-id="88f8a-241">The <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, `[HttpPost]`, is provided to routing so that it can choose based on the HTTP method of the request.</span></span> <span data-ttu-id="88f8a-242">`HttpPostAttribute`Zapewnia `Edit(int, Product)` lepszy odpowiednik niż `Edit(int)` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-242">The `HttpPostAttribute` makes `Edit(int, Product)` a better match than `Edit(int)`.</span></span>

<span data-ttu-id="88f8a-243">Ważne jest, aby zrozumieć rolę atrybutów, takich jak `HttpPostAttribute` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-243">It's important to understand the role of attributes like `HttpPostAttribute`.</span></span> <span data-ttu-id="88f8a-244">Podobne atrybuty są zdefiniowane dla innych [czasowników HTTP](#verb).</span><span class="sxs-lookup"><span data-stu-id="88f8a-244">Similar attributes are defined for other [HTTP verbs](#verb).</span></span> <span data-ttu-id="88f8a-245">W przypadku [routingu konwencjonalnego](#cr)typowe dla akcji używanie tej samej nazwy akcji, gdy są one częścią formularza Pokaż, przesyłania formularza.</span><span class="sxs-lookup"><span data-stu-id="88f8a-245">In [conventional routing](#cr), it's common for actions to use the same action name when they're part of a show form, submit form workflow.</span></span> <span data-ttu-id="88f8a-246">Na przykład zapoznaj [się z tematem badanie dwóch metod akcji edycji](xref:tutorials/first-mvc-app/controller-methods-views#get-post).</span><span class="sxs-lookup"><span data-stu-id="88f8a-246">For example, see [Examine the two Edit action methods](xref:tutorials/first-mvc-app/controller-methods-views#get-post).</span></span>

<span data-ttu-id="88f8a-247">Jeśli routingu nie można wybrać najlepszego kandydata, <xref:System.Reflection.AmbiguousMatchException> zostanie zgłoszony, zawierający wiele pasujących punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="88f8a-247">If routing can't choose a best candidate, an <xref:System.Reflection.AmbiguousMatchException> is thrown, listing the multiple matched endpoints.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="conventional-route-names"></a><span data-ttu-id="88f8a-248">Nazwy tras konwencjonalnych</span><span class="sxs-lookup"><span data-stu-id="88f8a-248">Conventional route names</span></span>

<span data-ttu-id="88f8a-249">Ciągi  `"blog"` i `"default"` w poniższych przykładach są nazwami konwencjonalnych tras:</span><span class="sxs-lookup"><span data-stu-id="88f8a-249">The strings  `"blog"` and `"default"` in the following examples are conventional route names:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<span data-ttu-id="88f8a-250">Nazwy tras nadają trasie nazwę logiczną.</span><span class="sxs-lookup"><span data-stu-id="88f8a-250">The route names give the route a logical name.</span></span> <span data-ttu-id="88f8a-251">Nazwana trasa może służyć do generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-251">The named route can be used for URL generation.</span></span> <span data-ttu-id="88f8a-252">Użycie nazwanej trasy upraszcza tworzenie adresów URL, gdy porządkowanie tras może spowodować skomplikowane generowanie adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-252">Using a named route simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="88f8a-253">Nazwy tras muszą być unikatowe w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-253">Route names must be unique application wide.</span></span>

<span data-ttu-id="88f8a-254">Nazwy tras:</span><span class="sxs-lookup"><span data-stu-id="88f8a-254">Route names:</span></span>

* <span data-ttu-id="88f8a-255">Nie mają wpływu na pasujące adresy URL ani obsługę żądań.</span><span class="sxs-lookup"><span data-stu-id="88f8a-255">Have no impact on URL matching or handling of requests.</span></span>
* <span data-ttu-id="88f8a-256">Są używane tylko do generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-256">Are used only for URL generation.</span></span>

<span data-ttu-id="88f8a-257">Koncepcja nazwy trasy jest reprezentowana w obszarze Routing jako [IEndpointNameMetadata](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata).</span><span class="sxs-lookup"><span data-stu-id="88f8a-257">The route name concept is represented in routing as [IEndpointNameMetadata](xref:Microsoft.AspNetCore.Routing.IEndpointNameMetadata).</span></span> <span data-ttu-id="88f8a-258">**Nazwa trasy** i nazwa **punktu końcowego**:</span><span class="sxs-lookup"><span data-stu-id="88f8a-258">The terms **route name** and **endpoint name**:</span></span>

* <span data-ttu-id="88f8a-259">Są zamienne.</span><span class="sxs-lookup"><span data-stu-id="88f8a-259">Are interchangeable.</span></span>
* <span data-ttu-id="88f8a-260">Który jest używany w dokumentacji i kodu zależy od opisanego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="88f8a-260">Which one is used in documentation and code depends on the API being described.</span></span>

<a name="attribute-routing-ref-label"></a>
<a name="ar"></a>

## <a name="attribute-routing-for-rest-apis"></a><span data-ttu-id="88f8a-261">Routing atrybutów dla interfejsów API REST</span><span class="sxs-lookup"><span data-stu-id="88f8a-261">Attribute routing for REST APIs</span></span>

<span data-ttu-id="88f8a-262">Interfejsy API REST powinny używać routingu atrybutów do modelowania funkcjonalności aplikacji jako zestawu zasobów, w których operacje są reprezentowane przez [zlecenia http](#verb).</span><span class="sxs-lookup"><span data-stu-id="88f8a-262">REST APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by [HTTP verbs](#verb).</span></span>

<span data-ttu-id="88f8a-263">Funkcja routingu atrybutów używa zestawu atrybutów do mapowania akcji bezpośrednio do szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="88f8a-263">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="88f8a-264">Poniższy `StartUp.Configure` kod jest typowy dla interfejsu API REST i jest używany w następnym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="88f8a-264">The following `StartUp.Configure` code is typical for a REST API and is used in the next sample:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupAPI.cs?name=snippet)]

<span data-ttu-id="88f8a-265">W powyższym kodzie <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers%2A> jest wywoływana w `UseEndpoints` celu mapowania kontrolerów z routingiem atrybutu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-265">In the preceding code, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllers%2A> is called inside `UseEndpoints` to map attribute routed controllers.</span></span>

<span data-ttu-id="88f8a-266">W poniższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="88f8a-266">In the following example:</span></span>

* <span data-ttu-id="88f8a-267">Poprzednia `Configure` Metoda jest używana.</span><span class="sxs-lookup"><span data-stu-id="88f8a-267">The preceding `Configure` method is used.</span></span>
* <span data-ttu-id="88f8a-268">`HomeController` dopasowuje zestaw adresów URL podobny do tego, co jest zgodne z domyślną trasą konwencjonalny `{controller=Home}/{action=Index}/{id?}` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-268">`HomeController` matches a set of URLs similar to what the default conventional route `{controller=Home}/{action=Index}/{id?}` matches.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

<span data-ttu-id="88f8a-269">`HomeController.Index`Akcja jest uruchamiana dla dowolnej ścieżki URL,, `/` `/Home` `/Home/Index` , lub `/Home/Index/3` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-269">The `HomeController.Index` action is run for any of the URL paths `/`, `/Home`, `/Home/Index`, or `/Home/Index/3`.</span></span>

<span data-ttu-id="88f8a-270">W tym przykładzie przedstawiono najważniejsze różnice programistyczne między routingiem atrybutów i [routingiem konwencjonalnym](#cr).</span><span class="sxs-lookup"><span data-stu-id="88f8a-270">This example highlights a key programming difference between attribute routing and [conventional routing](#cr).</span></span> <span data-ttu-id="88f8a-271">Routing atrybutów wymaga więcej danych wejściowych w celu określenia trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-271">Attribute routing requires more input to specify a route.</span></span> <span data-ttu-id="88f8a-272">Konwencjonalne trasy domyślne obsługuje trasy bardziej zwięzłie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-272">The conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="88f8a-273">Jednak Routing atrybutu zezwala na ścisłą kontrolę nad tym, które szablony tras mają zastosowanie do poszczególnych [akcji](#action).</span><span class="sxs-lookup"><span data-stu-id="88f8a-273">However, attribute routing allows and requires precise control of which route templates apply to each [action](#action).</span></span>

<span data-ttu-id="88f8a-274">W przypadku routingu atrybutów nazwy kontrolerów i akcji nie odgrywają części, w której akcja jest dopasowywana, chyba że zostanie użyta [Zastępowanie tokenu](#routing-token-replacement-templates-ref-label) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-274">With attribute routing, the controller and action names play no part in which action is matched, unless [token replacement](#routing-token-replacement-templates-ref-label) is used.</span></span> <span data-ttu-id="88f8a-275">Poniższy przykład dopasowuje te same adresy URL, co w poprzednim przykładzie:</span><span class="sxs-lookup"><span data-stu-id="88f8a-275">The following example matches the same URLs as the previous example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

<span data-ttu-id="88f8a-276">Następujący kod używa zastąpienia tokenu dla `action` i `controller` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-276">The following code uses token replacement for `action` and `controller`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet22)]

<span data-ttu-id="88f8a-277">Następujący kod ma zastosowanie `[Route("[controller]/[action]")]` do kontrolera:</span><span class="sxs-lookup"><span data-stu-id="88f8a-277">The following code applies `[Route("[controller]/[action]")]` to the controller:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet24)]

<span data-ttu-id="88f8a-278">W poprzednim kodzie, `Index` Szablony metod muszą być poprzedzone `/` lub `~/` do szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="88f8a-278">In the preceding code, the `Index` method templates must prepend `/` or `~/` to the route templates.</span></span> <span data-ttu-id="88f8a-279">Szablony tras zastosowane do akcji rozpoczynającej się od `/` lub `~/` nie są połączone z szablonami tras zastosowanymi do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="88f8a-279">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span>

<span data-ttu-id="88f8a-280">Zobacz [pierwszeństwo szablonu trasy](xref:fundamentals/routing#rtp) , aby uzyskać informacje o wyborze szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-280">See [Route template precedence](xref:fundamentals/routing#rtp) for information on route template selection.</span></span>

## <a name="reserved-routing-names"></a><span data-ttu-id="88f8a-281">Zastrzeżone nazwy routingu</span><span class="sxs-lookup"><span data-stu-id="88f8a-281">Reserved routing names</span></span>

<span data-ttu-id="88f8a-282">Następujące słowa kluczowe są zastrzeżonymi nazwami parametrów trasy podczas korzystania z kontrolerów lub Razor stron:</span><span class="sxs-lookup"><span data-stu-id="88f8a-282">The following keywords are reserved route parameter names when using Controllers or Razor Pages:</span></span>

* `action`
* `area`
* `controller`
* `handler`
* `page`

<span data-ttu-id="88f8a-283">Użycie `page` jako parametru trasy z routingiem atrybutu jest typowym błędem.</span><span class="sxs-lookup"><span data-stu-id="88f8a-283">Using `page` as a route parameter with attribute routing is a common error.</span></span> <span data-ttu-id="88f8a-284">Powoduje to niespójne i mylące zachowanie przy generowaniu adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-284">Doing that results in inconsistent and confusing behavior with URL generation.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo2Controller.cs?name=snippet)]

<span data-ttu-id="88f8a-285">Specjalne nazwy parametrów są używane przez generowanie adresów URL w celu ustalenia, czy operacja generowania adresu URL odwołuje się do Razor strony lub do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="88f8a-285">The special parameter names are used by the URL generation to determine if a URL generation operation refers to a Razor Page or to a Controller.</span></span>

<a name="verb"></a>

## <a name="http-verb-templates"></a><span data-ttu-id="88f8a-286">Szablony czasowników HTTP</span><span class="sxs-lookup"><span data-stu-id="88f8a-286">HTTP verb templates</span></span>

<span data-ttu-id="88f8a-287">ASP.NET Core ma następujące szablony czasowników HTTP:</span><span class="sxs-lookup"><span data-stu-id="88f8a-287">ASP.NET Core has the following HTTP verb templates:</span></span>

* <span data-ttu-id="88f8a-288">[Narzędzia HttpGet](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)</span><span class="sxs-lookup"><span data-stu-id="88f8a-288">[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)</span></span>
* <span data-ttu-id="88f8a-289">[HTTPPOST](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)</span><span class="sxs-lookup"><span data-stu-id="88f8a-289">[[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute)</span></span>
* <span data-ttu-id="88f8a-290">[[HttpPut]](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)</span><span class="sxs-lookup"><span data-stu-id="88f8a-290">[[HttpPut]](xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute)</span></span>
* <span data-ttu-id="88f8a-291">[[HttpDelete]](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)</span><span class="sxs-lookup"><span data-stu-id="88f8a-291">[[HttpDelete]](xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute)</span></span>
* <span data-ttu-id="88f8a-292">[[HttpHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)</span><span class="sxs-lookup"><span data-stu-id="88f8a-292">[[HttpHead]](xref:Microsoft.AspNetCore.Mvc.HttpHeadAttribute)</span></span>
* <span data-ttu-id="88f8a-293">[[HttpPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)</span><span class="sxs-lookup"><span data-stu-id="88f8a-293">[[HttpPatch]](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)</span></span>

<a name="rt"></a>

### <a name="route-templates"></a><span data-ttu-id="88f8a-294">Szablony tras</span><span class="sxs-lookup"><span data-stu-id="88f8a-294">Route templates</span></span>

<span data-ttu-id="88f8a-295">ASP.NET Core ma następujące szablony tras:</span><span class="sxs-lookup"><span data-stu-id="88f8a-295">ASP.NET Core has the following route templates:</span></span>

* <span data-ttu-id="88f8a-296">Wszystkie [Szablony czasowników HTTP](#verb) są szablonami tras.</span><span class="sxs-lookup"><span data-stu-id="88f8a-296">All the [HTTP verb templates](#verb) are route templates.</span></span>
* <span data-ttu-id="88f8a-297">[Szlak](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)</span><span class="sxs-lookup"><span data-stu-id="88f8a-297">[[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)</span></span>

<a name="arx"></a>

### <a name="attribute-routing-with-http-verb-attributes"></a><span data-ttu-id="88f8a-298">Routing atrybutów z atrybutami czasownik http</span><span class="sxs-lookup"><span data-stu-id="88f8a-298">Attribute routing with Http verb attributes</span></span>

<span data-ttu-id="88f8a-299">Rozważmy następujący kontroler:</span><span class="sxs-lookup"><span data-stu-id="88f8a-299">Consider the following controller:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet)]

<span data-ttu-id="88f8a-300">Powyższy kod ma następujące działanie:</span><span class="sxs-lookup"><span data-stu-id="88f8a-300">In the preceding code:</span></span>

* <span data-ttu-id="88f8a-301">Każda akcja zawiera `[HttpGet]` atrybut, który ogranicza dopasowywanie do żądań HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="88f8a-301">Each action contains the `[HttpGet]` attribute, which constrains matching to HTTP GET requests only.</span></span>
* <span data-ttu-id="88f8a-302">`GetProduct`Akcja obejmuje `"{id}"` szablon, dlatego `id` jest dołączany do `"api/[controller]"` szablonu na kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="88f8a-302">The `GetProduct` action includes the `"{id}"` template, therefore `id` is appended to the `"api/[controller]"` template on the controller.</span></span> <span data-ttu-id="88f8a-303">Szablon metod to `"api/[controller]/"{id}""` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-303">The methods template is `"api/[controller]/"{id}""`.</span></span> <span data-ttu-id="88f8a-304">W związku z tym ta akcja dopasowuje tylko żądania GET dla formularza `/api/test2/xyz` , itp `/api/test2/123` `/api/test2/{any string}` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-304">Therefore this action only matches GET requests for the form `/api/test2/xyz`,`/api/test2/123`,`/api/test2/{any string}`, etc.</span></span>
  [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet2)]
* <span data-ttu-id="88f8a-305">`GetIntProduct`Akcja zawiera `"int/{id:int}")` szablon.</span><span class="sxs-lookup"><span data-stu-id="88f8a-305">The `GetIntProduct` action contains the `"int/{id:int}")` template.</span></span> <span data-ttu-id="88f8a-306">`:int`Część szablonu ogranicza `id` wartości trasy do ciągów, które mogą być konwertowane na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="88f8a-306">The `:int` portion of the template constrains the `id` route values to strings that can be converted to an integer.</span></span> <span data-ttu-id="88f8a-307">Żądanie GET `/api/test2/int/abc` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-307">A GET request to `/api/test2/int/abc`:</span></span>
  * <span data-ttu-id="88f8a-308">Nie jest zgodna z tą akcją.</span><span class="sxs-lookup"><span data-stu-id="88f8a-308">Doesn't match this action.</span></span>
  * <span data-ttu-id="88f8a-309">Zwraca błąd [nie znaleziono 404](https://developer.mozilla.org/docs/Web/HTTP/Status/404) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-309">Returns a [404 Not Found](https://developer.mozilla.org/docs/Web/HTTP/Status/404) error.</span></span>
    [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet3)]
* <span data-ttu-id="88f8a-310">`GetInt2Product`Akcja zawiera `{id}` w szablonie, ale nie ogranicza `id` wartości, które można przekonwertować na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="88f8a-310">The `GetInt2Product` action contains `{id}` in the template, but doesn't constrain `id` to values that can be converted to an integer.</span></span> <span data-ttu-id="88f8a-311">Żądanie GET `/api/test2/int2/abc` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-311">A GET request to `/api/test2/int2/abc`:</span></span>
  * <span data-ttu-id="88f8a-312">Pasuje do tej trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-312">Matches this route.</span></span>
  * <span data-ttu-id="88f8a-313">Nie można przekonwertować powiązania modelu `abc` na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="88f8a-313">Model binding fails to convert `abc` to an integer.</span></span> <span data-ttu-id="88f8a-314">`id`Parametr metody jest liczbą całkowitą.</span><span class="sxs-lookup"><span data-stu-id="88f8a-314">The `id` parameter of the method is integer.</span></span>
  * <span data-ttu-id="88f8a-315">Zwraca [400 Nieprawidłowe żądanie](https://developer.mozilla.org/docs/Web/HTTP/Status/400) , ponieważ nie można skonwertować powiązania modelu `abc` na liczbę całkowitą.</span><span class="sxs-lookup"><span data-stu-id="88f8a-315">Returns a [400 Bad Request](https://developer.mozilla.org/docs/Web/HTTP/Status/400) because model binding failed to convert`abc` to an integer.</span></span>
      [!code-csharp[](routing/samples/3.x/main/Controllers/Test2Controller.cs?name=snippet4)]

<span data-ttu-id="88f8a-316">Funkcja routingu atrybutów może używać <xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute> atrybutów takich jak <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute> , <xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute> , i <xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute> .</span><span class="sxs-lookup"><span data-stu-id="88f8a-316">Attribute routing can use <xref:Microsoft.AspNetCore.Mvc.Routing.HttpMethodAttribute> attributes such as <xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute>, <xref:Microsoft.AspNetCore.Mvc.HttpPutAttribute>, and <xref:Microsoft.AspNetCore.Mvc.HttpDeleteAttribute>.</span></span> <span data-ttu-id="88f8a-317">Wszystkie atrybuty [czasowników HTTP](#verb) akceptują szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-317">All of the [HTTP verb](#verb) attributes accept a route template.</span></span> <span data-ttu-id="88f8a-318">Poniższy przykład przedstawia dwie akcje, które pasują do tego samego szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="88f8a-318">The following example shows two actions that match the same route template:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyProductsController.cs?name=snippet1)]

<span data-ttu-id="88f8a-319">Przy użyciu ścieżki URL `/products3` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-319">Using the URL path `/products3`:</span></span>

* <span data-ttu-id="88f8a-320">`MyProductsController.ListProducts`Akcja jest uruchamiana, gdy [czasownik http](#verb) to `GET` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-320">The `MyProductsController.ListProducts` action runs when the [HTTP verb](#verb) is `GET`.</span></span>
* <span data-ttu-id="88f8a-321">`MyProductsController.CreateProduct`Akcja jest uruchamiana, gdy [czasownik http](#verb) to `POST` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-321">The `MyProductsController.CreateProduct` action runs when the [HTTP verb](#verb) is `POST`.</span></span>

<span data-ttu-id="88f8a-322">Podczas kompilowania interfejsu API REST jest to rzadko konieczne, aby użyć `[Route(...)]` metody akcji, ponieważ akcja akceptuje wszystkie metody http.</span><span class="sxs-lookup"><span data-stu-id="88f8a-322">When building a REST API, it's rare that you'll need to use `[Route(...)]` on an action method because the action accepts all HTTP methods.</span></span> <span data-ttu-id="88f8a-323">Lepiej jest używać bardziej szczegółowego [atrybutu zlecenia http](#verb) , aby precyzyjnie dowiedzieć się, co obsługuje interfejs API.</span><span class="sxs-lookup"><span data-stu-id="88f8a-323">It's better to use the more specific [HTTP verb attribute](#verb) to be precise about what your API supports.</span></span> <span data-ttu-id="88f8a-324">Klienci interfejsów API REST powinni wiedzieć, jakie ścieżki i czasowniki HTTP mapują na określone operacje logiczne.</span><span class="sxs-lookup"><span data-stu-id="88f8a-324">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="88f8a-325">Interfejsy API REST powinny używać routingu atrybutów do modelowania funkcjonalności aplikacji jako zestawu zasobów, w których operacje są reprezentowane przez zlecenia HTTP.</span><span class="sxs-lookup"><span data-stu-id="88f8a-325">REST APIs should use attribute routing to model the app's functionality as a set of resources where operations are represented by HTTP verbs.</span></span> <span data-ttu-id="88f8a-326">Oznacza to, że wiele operacji, na przykład Pobierz i Opublikuj dla tego samego zasobu logicznego, użyj tego samego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-326">This means that many operations, for example, GET and POST on the same logical resource use the same URL.</span></span> <span data-ttu-id="88f8a-327">Routing atrybutu zapewnia poziom kontroli, który jest wymagany do dokładnego projektowania układu publicznego punktu końcowego interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="88f8a-327">Attribute routing provides a level of control that's needed to carefully design an API's public endpoint layout.</span></span>

<span data-ttu-id="88f8a-328">Ponieważ atrybut Route ma zastosowanie do określonej akcji, można łatwo wprowadzić parametry wymagane jako część definicji szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-328">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="88f8a-329">W poniższym przykładzie `id` jest wymagane jako część ścieżki URL:</span><span class="sxs-lookup"><span data-stu-id="88f8a-329">In the following example, `id` is required as part of the URL path:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

<span data-ttu-id="88f8a-330">`Products2ApiController.GetProduct(int)`Akcja:</span><span class="sxs-lookup"><span data-stu-id="88f8a-330">The `Products2ApiController.GetProduct(int)` action:</span></span>

* <span data-ttu-id="88f8a-331">Jest uruchamiany z ścieżką URL podobną do `/products2/3`</span><span class="sxs-lookup"><span data-stu-id="88f8a-331">Is run with URL path like `/products2/3`</span></span>
* <span data-ttu-id="88f8a-332">Nie jest uruchamiany z ścieżką URL `/products2` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-332">Isn't run with the URL path `/products2`.</span></span>

<span data-ttu-id="88f8a-333">Atrybut [[](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) Requests] umożliwia akcja ograniczenia obsługiwanych typów zawartości żądania.</span><span class="sxs-lookup"><span data-stu-id="88f8a-333">The [[Consumes]](<xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute>) attribute allows an action to limit the supported request content types.</span></span> <span data-ttu-id="88f8a-334">Aby uzyskać więcej informacji, zobacz [Definiowanie obsługiwanych typów zawartości żądania przy użyciu atrybutu użycia](xref:web-api/index#consumes).</span><span class="sxs-lookup"><span data-stu-id="88f8a-334">For more information, see [Define supported request content types with the Consumes attribute](xref:web-api/index#consumes).</span></span>

 <span data-ttu-id="88f8a-335">Aby uzyskać pełny opis szablonów tras i powiązanych opcji, zobacz [Routing](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-335">See [Routing](xref:fundamentals/routing) for a full description of route templates and related options.</span></span>

<span data-ttu-id="88f8a-336">Aby uzyskać więcej informacji na temat `[ApiController]` , zobacz [ApiController Attribute](xref:web-api/index##apicontroller-attribute).</span><span class="sxs-lookup"><span data-stu-id="88f8a-336">For more information on `[ApiController]`, see [ApiController attribute](xref:web-api/index##apicontroller-attribute).</span></span>

## <a name="route-name"></a><span data-ttu-id="88f8a-337">Nazwa trasy</span><span class="sxs-lookup"><span data-stu-id="88f8a-337">Route name</span></span>

<span data-ttu-id="88f8a-338">Następujący kod definiuje nazwę trasy `Products_List` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-338">The following code  defines a route name of `Products_List`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet2)]

<span data-ttu-id="88f8a-339">Nazwy tras mogą służyć do generowania adresów URL na podstawie określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-339">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="88f8a-340">Nazwy tras:</span><span class="sxs-lookup"><span data-stu-id="88f8a-340">Route names:</span></span>

* <span data-ttu-id="88f8a-341">Nie ma wpływu na zachowanie w zakresie routingu zgodnego z adresem URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-341">Have no impact on the URL matching behavior of routing.</span></span>
* <span data-ttu-id="88f8a-342">Są używane tylko na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-342">Are only used for URL generation.</span></span>

<span data-ttu-id="88f8a-343">Nazwy tras muszą być unikatowe w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-343">Route names must be unique application-wide.</span></span>

<span data-ttu-id="88f8a-344">Powyższy kod przy użyciu konwencjonalnej trasy domyślnej, która definiuje `id` parametr jako opcjonalny ( `{id?}` ).</span><span class="sxs-lookup"><span data-stu-id="88f8a-344">Contrast the preceding code with the conventional default route, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="88f8a-345">Możliwość precyzyjnego określania interfejsów API ma zalety, takie jak umożliwienie `/products` i `/products/5` wysyłanie ich do różnych akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-345">The ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

## <a name="combining-attribute-routes"></a><span data-ttu-id="88f8a-346">Łączenie tras atrybutów</span><span class="sxs-lookup"><span data-stu-id="88f8a-346">Combining attribute routes</span></span>

<span data-ttu-id="88f8a-347">Aby mniej powtarzać Routing atrybutów, atrybuty trasy na kontrolerze są łączone z atrybutami trasy dla poszczególnych akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-347">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="88f8a-348">Wszystkie szablony tras zdefiniowane na kontrolerze są poprzedzone w celu rozesłania szablonów w akcjach.</span><span class="sxs-lookup"><span data-stu-id="88f8a-348">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="88f8a-349">Umieszczenie atrybutu trasy na kontrolerze powoduje, że **wszystkie** akcje w kontrolerze używają routingu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-349">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsApiController.cs?name=snippet)]

<span data-ttu-id="88f8a-350">W powyższym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="88f8a-350">In the preceding example:</span></span>

* <span data-ttu-id="88f8a-351">Ścieżka adresu URL `/products` może być taka sama `ProductsApi.ListProducts`</span><span class="sxs-lookup"><span data-stu-id="88f8a-351">The URL path `/products` can match `ProductsApi.ListProducts`</span></span>
* <span data-ttu-id="88f8a-352">Ścieżka adresu URL `/products/5` może być taka sama `ProductsApi.GetProduct(int)` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-352">The URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span>

<span data-ttu-id="88f8a-353">Obie te akcje pasują tylko do protokołu HTTP, `GET` ponieważ są oznaczone `[HttpGet]` atrybutem.</span><span class="sxs-lookup"><span data-stu-id="88f8a-353">Both of these actions only match HTTP `GET` because they're marked with the `[HttpGet]` attribute.</span></span>

<span data-ttu-id="88f8a-354">Szablony tras zastosowane do akcji rozpoczynającej się od `/` lub `~/` nie są połączone z szablonami tras zastosowanymi do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="88f8a-354">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="88f8a-355">Poniższy przykład dopasowuje zestaw ścieżek adresów URL podobny do trasy domyślnej.</span><span class="sxs-lookup"><span data-stu-id="88f8a-355">The following example matches a set of URL paths similar to the default route.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet)]

<span data-ttu-id="88f8a-356">W poniższej tabeli opisano `[Route]` atrybuty w poprzednim kodzie:</span><span class="sxs-lookup"><span data-stu-id="88f8a-356">The following table explains the `[Route]` attributes in the preceding code:</span></span>

| <span data-ttu-id="88f8a-357">Atrybut</span><span class="sxs-lookup"><span data-stu-id="88f8a-357">Attribute</span></span>               | <span data-ttu-id="88f8a-358">Łączy z `[Route("Home")]`</span><span class="sxs-lookup"><span data-stu-id="88f8a-358">Combines with `[Route("Home")]`</span></span> | <span data-ttu-id="88f8a-359">Definiuje szablon trasy</span><span class="sxs-lookup"><span data-stu-id="88f8a-359">Defines route template</span></span> |
| ----------------- | ------------ | --------- |
| `[Route("")]` | <span data-ttu-id="88f8a-360">Tak</span><span class="sxs-lookup"><span data-stu-id="88f8a-360">Yes</span></span> | `"Home"` |
| `[Route("Index")]` | <span data-ttu-id="88f8a-361">Tak</span><span class="sxs-lookup"><span data-stu-id="88f8a-361">Yes</span></span> | `"Home/Index"` |
| `[Route("/")]` | <span data-ttu-id="88f8a-362">**Nie**</span><span class="sxs-lookup"><span data-stu-id="88f8a-362">**No**</span></span> | `""` |
| `[Route("About")]` | <span data-ttu-id="88f8a-363">Tak</span><span class="sxs-lookup"><span data-stu-id="88f8a-363">Yes</span></span> | `"Home/About"` |

<a name="routing-ordering-ref-label"></a>
<a name="oar"></a>

### <a name="attribute-route-order"></a><span data-ttu-id="88f8a-364">Porządek trasy dla atrybutu</span><span class="sxs-lookup"><span data-stu-id="88f8a-364">Attribute route order</span></span>

<span data-ttu-id="88f8a-365">Routing kompiluje drzewo i dopasowuje wszystkie punkty końcowe jednocześnie:</span><span class="sxs-lookup"><span data-stu-id="88f8a-365">Routing builds a tree and matches all endpoints simultaneously:</span></span>

* <span data-ttu-id="88f8a-366">Wpisy trasy zachowują się tak, jakby zostały umieszczone w idealnej kolejności.</span><span class="sxs-lookup"><span data-stu-id="88f8a-366">The route entries behave as if placed in an ideal ordering.</span></span>
* <span data-ttu-id="88f8a-367">Najbardziej konkretne trasy mają możliwość wykonania przed bardziej ogólnymi trasami.</span><span class="sxs-lookup"><span data-stu-id="88f8a-367">The most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="88f8a-368">Na przykład, podobnie jak w przypadku `blog/search/{topic}` bardziej szczegółowych informacji niż trasie atrybutu `blog/{*article}` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-368">For example, an attribute route like `blog/search/{topic}` is more specific than an attribute route like `blog/{*article}`.</span></span> <span data-ttu-id="88f8a-369">`blog/search/{topic}`Trasa ma wyższy priorytet, ponieważ jest domyślnie bardziej specyficzna.</span><span class="sxs-lookup"><span data-stu-id="88f8a-369">The `blog/search/{topic}` route has higher priority, by default, because it's more specific.</span></span> <span data-ttu-id="88f8a-370">Przy użyciu [konwencjonalnego routingu](#cr)deweloper jest odpowiedzialny za umieszczanie tras w odpowiedniej kolejności.</span><span class="sxs-lookup"><span data-stu-id="88f8a-370">Using [conventional routing](#cr), the developer is responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="88f8a-371">Trasy atrybutów mogą konfigurować zamówienie przy użyciu <xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order> właściwości.</span><span class="sxs-lookup"><span data-stu-id="88f8a-371">Attribute routes can configure an order using the <xref:Microsoft.AspNetCore.Mvc.RouteAttribute.Order> property.</span></span> <span data-ttu-id="88f8a-372">Wszystkie [atrybuty trasy](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) dostarczonej przez platformę obejmują `Order` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-372">All of the framework provided [route attributes](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) include `Order` .</span></span> <span data-ttu-id="88f8a-373">Trasy są przetwarzane zgodnie z rosnącą sortowaniem `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="88f8a-373">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="88f8a-374">Kolejność domyślna to `0` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-374">The default order is `0`.</span></span> <span data-ttu-id="88f8a-375">Ustawianie trasy przy użyciu `Order = -1` przebiegów przed trasami, które nie ustawiają kolejności.</span><span class="sxs-lookup"><span data-stu-id="88f8a-375">Setting a route using `Order = -1` runs before routes that don't set an order.</span></span> <span data-ttu-id="88f8a-376">Ustawianie trasy przy użyciu `Order = 1` przebiegu po domyślnej kolejności tras.</span><span class="sxs-lookup"><span data-stu-id="88f8a-376">Setting a route using `Order = 1` runs after default route ordering.</span></span>

<span data-ttu-id="88f8a-377">**Należy unikać** w zależności od `Order` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-377">**Avoid** depending on `Order`.</span></span> <span data-ttu-id="88f8a-378">Jeśli przestrzeń adresów URL aplikacji wymaga jawnych wartości kolejności, aby można było prawidłowo kierować trasy, prawdopodobnie również jest myląca dla klientów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-378">If an app's URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="88f8a-379">Ogólnie rzecz biorąc, routing atrybutów wybiera prawidłową trasę z dopasowywaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-379">In general, attribute routing selects the correct route with URL matching.</span></span> <span data-ttu-id="88f8a-380">Jeśli domyślna kolejność generowania adresów URL nie działa, użycie nazwy trasy jako przesłonięcia jest zwykle prostsze niż stosowanie `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="88f8a-380">If the default order used for URL generation isn't working, using a route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="88f8a-381">Należy wziąć pod uwagę następujące dwa kontrolery, które definiują trasę zgodną z `/home` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-381">Consider the following two controllers which both define the route matching `/home`:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/HomeController.cs?name=snippet2)]

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemoController.cs?name=snippet)]

<span data-ttu-id="88f8a-382">Żądanie `/home` z powyższym kodem zgłasza wyjątek podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="88f8a-382">Requesting `/home` with the preceding code throws an exception similar to the following:</span></span>

```text
AmbiguousMatchException: The request matched multiple endpoints. Matches:

 WebMvcRouting.Controllers.HomeController.Index
 WebMvcRouting.Controllers.MyDemoController.MyIndex
```

<span data-ttu-id="88f8a-383">Dodanie `Order` do jednego z atrybutów trasy rozwiązuje niejednoznaczność:</span><span class="sxs-lookup"><span data-stu-id="88f8a-383">Adding `Order` to one of the route attributes resolves the ambiguity:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyDemo3Controller.cs?name=snippet3& highlight=2)]

<span data-ttu-id="88f8a-384">Za pomocą powyższego kodu program `/home` uruchamia `HomeController.Index` punkt końcowy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-384">With the preceding code, `/home` runs the `HomeController.Index` endpoint.</span></span> <span data-ttu-id="88f8a-385">, Aby przejść do `MyDemoController.MyIndex` żądania `/home/MyIndex` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-385">To get to the `MyDemoController.MyIndex`, request `/home/MyIndex`.</span></span> <span data-ttu-id="88f8a-386">**Uwaga**:</span><span class="sxs-lookup"><span data-stu-id="88f8a-386">**Note**:</span></span>

* <span data-ttu-id="88f8a-387">Poprzedni kod jest przykładem lub słabym projektem routingu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-387">The preceding code is an example or poor routing design.</span></span> <span data-ttu-id="88f8a-388">Została użyta do zilustrowania `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="88f8a-388">It was used to illustrate the `Order` property.</span></span>
* <span data-ttu-id="88f8a-389">`Order`Właściwość rozwiązuje tylko niejednoznaczność, ale nie można dopasować tego szablonu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-389">The `Order` property only resolves the ambiguity, that template cannot be matched.</span></span> <span data-ttu-id="88f8a-390">Lepszym rozwiązaniem jest usunięcie `[Route("Home")]` szablonu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-390">It would be better to remove the `[Route("Home")]` template.</span></span>

<span data-ttu-id="88f8a-391">Zobacz [ Razor strony trasy i konwencje aplikacji: kolejność tras](xref:razor-pages/razor-pages-conventions#route-order) na potrzeby informacji o kolejności tras na Razor stronach.</span><span class="sxs-lookup"><span data-stu-id="88f8a-391">See [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order) for information on route order with Razor Pages.</span></span>

<span data-ttu-id="88f8a-392">W niektórych przypadkach błąd HTTP 500 jest zwracany z niejednoznacznych tras.</span><span class="sxs-lookup"><span data-stu-id="88f8a-392">In some cases, an HTTP 500 error is returned with ambiguous routes.</span></span> <span data-ttu-id="88f8a-393">Użyj [rejestrowania](xref:fundamentals/logging/index) , aby zobaczyć, które punkty końcowe spowodowały `AmbiguousMatchException` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-393">Use [logging](xref:fundamentals/logging/index) to see which endpoints caused the `AmbiguousMatchException`.</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="88f8a-394">Zastępowanie tokenu w szablonach tras [Controller], [Action], [obszar]</span><span class="sxs-lookup"><span data-stu-id="88f8a-394">Token replacement in route templates [controller], [action], [area]</span></span>

<span data-ttu-id="88f8a-395">Dla wygody, trasy atrybutu obsługują *Zastępowanie tokenu* przez ujęcie tokenu w nawiasy kwadratowe ( `[` , `]` ).</span><span class="sxs-lookup"><span data-stu-id="88f8a-395">For convenience, attribute routes support *token replacement* by enclosing a token in square-brackets (`[`, `]`).</span></span> <span data-ttu-id="88f8a-396">Tokeny `[action]` , `[area]` i `[controller]` są zastępowane wartościami nazwy akcji, obszaru i nazwy kontrolera z akcji, w której zdefiniowano trasę:</span><span class="sxs-lookup"><span data-stu-id="88f8a-396">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="88f8a-397">Powyższy kod ma następujące działanie:</span><span class="sxs-lookup"><span data-stu-id="88f8a-397">In the preceding code:</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet10)]

  * <span data-ttu-id="88f8a-398">Prawdopodobny `/Products0/List`</span><span class="sxs-lookup"><span data-stu-id="88f8a-398">Matches `/Products0/List`</span></span>

  [!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet11)]

  * <span data-ttu-id="88f8a-399">Prawdopodobny `/Products0/Edit/{id}`</span><span class="sxs-lookup"><span data-stu-id="88f8a-399">Matches `/Products0/Edit/{id}`</span></span>

<span data-ttu-id="88f8a-400">Zastępowanie tokenu występuje jako ostatni krok tworzenia tras atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-400">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="88f8a-401">Poprzedni przykład zachowuje się tak samo jak w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="88f8a-401">The preceding example behaves the same as the following code:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet20)]

[!INCLUDE[](~/includes/MTcomments.md)]

<span data-ttu-id="88f8a-402">Trasy atrybutu można także łączyć z dziedziczeniem.</span><span class="sxs-lookup"><span data-stu-id="88f8a-402">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="88f8a-403">Jest to zaawansowane połączenie z zastępowaniem tokenu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-403">This is powerful combined with token replacement.</span></span> <span data-ttu-id="88f8a-404">Zastępowanie tokenu dotyczy również nazw tras zdefiniowanych przez trasy atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-404">Token replacement also applies to route names defined by attribute routes.</span></span>
<span data-ttu-id="88f8a-405">`[Route("[controller]/[action]", Name="[controller]_[action]")]`generuje unikatową nazwę trasy dla każdej akcji:</span><span class="sxs-lookup"><span data-stu-id="88f8a-405">`[Route("[controller]/[action]", Name="[controller]_[action]")]`generates a unique route name for each action:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet5)]

<span data-ttu-id="88f8a-406">Zastępowanie tokenu dotyczy również nazw tras zdefiniowanych przez trasy atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-406">Token replacement also applies to route names defined by attribute routes.</span></span>
`[Route("[controller]/[action]", Name="[controller]_[action]")]`
<span data-ttu-id="88f8a-407">generuje unikatową nazwę trasy dla każdej akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-407">generates a unique route name for each action.</span></span>

<span data-ttu-id="88f8a-408">Aby dopasować ogranicznik zamiany tokenu literału `[` lub  `]` , należy to zrobić, powtarzając znak ( `[[` lub `]]` ).</span><span class="sxs-lookup"><span data-stu-id="88f8a-408">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="88f8a-409">Używanie transformatora parametrów do dostosowywania zastępowania tokenu</span><span class="sxs-lookup"><span data-stu-id="88f8a-409">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="88f8a-410">Zastępowanie tokenu można dostosować za pomocą transformatora parametrów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-410">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="88f8a-411">Transformator parametrów implementuje <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> i przekształca wartość parametrów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-411">A parameter transformer implements <xref:Microsoft.AspNetCore.Routing.IOutboundParameterTransformer> and transforms the value of parameters.</span></span> <span data-ttu-id="88f8a-412">Na przykład niestandardowy `SlugifyParameterTransformer` transformator parametrów zmienia `SubscriptionManagement` wartość trasy na `subscription-management` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-412">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`:</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet2)]

<span data-ttu-id="88f8a-413"><xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention>Jest konwencją modelu aplikacji, która:</span><span class="sxs-lookup"><span data-stu-id="88f8a-413">The <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.RouteTokenTransformerConvention> is an application model convention that:</span></span>

* <span data-ttu-id="88f8a-414">Stosuje transformator parametrów do wszystkich tras atrybutów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-414">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="88f8a-415">Dostosowuje wartości tokenów tras w miarę ich wymiany.</span><span class="sxs-lookup"><span data-stu-id="88f8a-415">Customizes the attribute route token values as they are replaced.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/SubscriptionManagementController.cs?name=snippet)]

<span data-ttu-id="88f8a-416">Poprzednia `ListAll` Metoda pasuje `/subscription-management/list-all` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-416">The preceding `ListAll` method matches `/subscription-management/list-all`.</span></span>

<span data-ttu-id="88f8a-417">`RouteTokenTransformerConvention`Jest zarejestrowany jako opcja w `ConfigureServices` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-417">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

[!code-csharp[](routing/samples/3.x/main/StartupSlugifyParamTransformer.cs?name=snippet)]

<span data-ttu-id="88f8a-418">Zobacz [powiadomienia MDN Web docs](https://developer.mozilla.org/docs/Glossary/Slug) , aby zapoznać się z definicją informacji o sobie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-418">See [MDN web docs on Slug](https://developer.mozilla.org/docs/Glossary/Slug) for the definition of Slug.</span></span>

[!INCLUDE[](~/includes/regex.md)]
<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-attribute-routes"></a><span data-ttu-id="88f8a-419">Wiele tras atrybutów</span><span class="sxs-lookup"><span data-stu-id="88f8a-419">Multiple attribute routes</span></span>

<span data-ttu-id="88f8a-420">Routing atrybutów obsługuje definiowanie wielu tras, które docierają do tej samej akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-420">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="88f8a-421">Najczęstszym sposobem użycia tej metody jest naśladowanie zachowania domyślnej trasy konwencjonalnej, jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="88f8a-421">The most common usage of this is to mimic the behavior of the default conventional route as shown in the following example:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6x)]

<span data-ttu-id="88f8a-422">Umieszczenie wielu atrybutów trasy na kontrolerze oznacza, że każda z nich łączy się z każdym z atrybutów tras w metodach akcji:</span><span class="sxs-lookup"><span data-stu-id="88f8a-422">Putting multiple route attributes on the controller means that each one combines with each of the route attributes on the action methods:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet6)]

<span data-ttu-id="88f8a-423">Wszystkie ograniczenia trasy [http zlecenia](#verb) są implementowane `IActionConstraint` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-423">All the [HTTP verb](#verb) route constraints implement `IActionConstraint`.</span></span>

<span data-ttu-id="88f8a-424">Gdy wiele atrybutów trasy, które implementują <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> są umieszczane w akcji:</span><span class="sxs-lookup"><span data-stu-id="88f8a-424">When multiple route attributes that implement <xref:Microsoft.AspNetCore.Mvc.ActionConstraints.IActionConstraint> are placed on an action:</span></span>

* <span data-ttu-id="88f8a-425">Każde ograniczenie akcji łączy się z szablonem trasy zastosowanym do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="88f8a-425">Each action constraint combines with the route template applied to the controller.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet7)]

<span data-ttu-id="88f8a-426">Korzystanie z wielu tras w akcjach może wydawać się użyteczne i zaawansowane, dlatego lepiej jest zachować podstawową i dobrze zdefiniowaną przestrzeń adresów URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-426">Using multiple routes on actions might seem useful and powerful, it's better to keep your app's URL space basic and well defined.</span></span> <span data-ttu-id="88f8a-427">Użyj wielu tras w przypadku akcji **tylko wtedy** , gdy jest to konieczne, na przykład w celu obsługi istniejących klientów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-427">Use multiple routes on actions **only** where needed, for example, to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="88f8a-428">Określanie opcjonalnych parametrów trasy, wartości domyślnych i ograniczeń</span><span class="sxs-lookup"><span data-stu-id="88f8a-428">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="88f8a-429">Trasy atrybutów obsługują tę samą składnię wbudowaną co konwencjonalne trasy, aby określić parametry opcjonalne, wartości domyślne i ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="88f8a-429">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/ProductsController.cs?name=snippet8&highlight=3)]

<span data-ttu-id="88f8a-430">W poprzednim kodzie, `[HttpPost("product14/{id:int}")]` stosuje ograniczenie trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-430">In the preceding code, `[HttpPost("product14/{id:int}")]` applies a route constraint.</span></span> <span data-ttu-id="88f8a-431">`Products14Controller.ShowProduct`Akcja jest dopasowywana tylko przez ścieżki URL, takie jak `/product14/3` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-431">The `Products14Controller.ShowProduct` action is matched only by URL paths like `/product14/3`.</span></span> <span data-ttu-id="88f8a-432">Część szablonu trasy `{id:int}` ogranicza ten segment tylko do liczb całkowitych.</span><span class="sxs-lookup"><span data-stu-id="88f8a-432">The route template portion `{id:int}` constrains that segment to only integers.</span></span>

<span data-ttu-id="88f8a-433">Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](xref:fundamentals/routing#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-433">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="88f8a-434">Niestandardowe atrybuty trasy przy użyciu IRouteTemplateProvider</span><span class="sxs-lookup"><span data-stu-id="88f8a-434">Custom route attributes using IRouteTemplateProvider</span></span>

<span data-ttu-id="88f8a-435">Wszystkie [atrybuty trasy](#rt) implementują <xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider> .</span><span class="sxs-lookup"><span data-stu-id="88f8a-435">All of the [route attributes](#rt) implement <xref:Microsoft.AspNetCore.Mvc.Routing.IRouteTemplateProvider>.</span></span> <span data-ttu-id="88f8a-436">Środowisko uruchomieniowe ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="88f8a-436">The ASP.NET Core runtime:</span></span>

* <span data-ttu-id="88f8a-437">Wyszukuje atrybuty klas kontrolera i metod akcji podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-437">Looks for attributes on controller classes and action methods when the app starts.</span></span>
* <span data-ttu-id="88f8a-438">Używa atrybutów, które implementują `IRouteTemplateProvider` w celu utworzenia początkowego zestawu tras.</span><span class="sxs-lookup"><span data-stu-id="88f8a-438">Uses the attributes that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="88f8a-439">Zaimplementuj `IRouteTemplateProvider` , aby zdefiniować niestandardowe atrybuty trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-439">Implement `IRouteTemplateProvider` to define custom route attributes.</span></span> <span data-ttu-id="88f8a-440">Każdy `IRouteTemplateProvider` z nich umożliwia definiowanie pojedynczej trasy z niestandardowym szablonem trasy, kolejnością i nazwą:</span><span class="sxs-lookup"><span data-stu-id="88f8a-440">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/MyTestApiController.cs?name=snippet&highlight=1-10)]

<span data-ttu-id="88f8a-441">Poprzednia `Get` Metoda zwraca `Order = 2, Template = api/MyTestApi` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-441">The preceding `Get` method returns `Order = 2, Template = api/MyTestApi`.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="use-application-model-to-customize-attribute-routes"></a><span data-ttu-id="88f8a-442">Dostosowywanie tras atrybutów przy użyciu modelu aplikacji</span><span class="sxs-lookup"><span data-stu-id="88f8a-442">Use application model to customize attribute routes</span></span>

<span data-ttu-id="88f8a-443">Model aplikacji:</span><span class="sxs-lookup"><span data-stu-id="88f8a-443">The application model:</span></span>

* <span data-ttu-id="88f8a-444">Jest modelem obiektu utworzonym podczas uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="88f8a-444">Is an object model created at startup.</span></span>
* <span data-ttu-id="88f8a-445">Zawiera wszystkie metadane używane przez ASP.NET Core do kierowania i wykonywania akcji w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-445">Contains all of the metadata used by ASP.NET Core to route and execute the actions in an app.</span></span>

<span data-ttu-id="88f8a-446">Model aplikacji zawiera wszystkie dane zebrane z atrybutów tras.</span><span class="sxs-lookup"><span data-stu-id="88f8a-446">The application model includes all of the data gathered from route attributes.</span></span> <span data-ttu-id="88f8a-447">Dane z atrybutów trasy są udostępniane przez `IRouteTemplateProvider` implementację.</span><span class="sxs-lookup"><span data-stu-id="88f8a-447">The data from route attributes is provided by the `IRouteTemplateProvider` implementation.</span></span> <span data-ttu-id="88f8a-448">Obowiązujący</span><span class="sxs-lookup"><span data-stu-id="88f8a-448">Conventions:</span></span>

* <span data-ttu-id="88f8a-449">Można napisać, aby zmodyfikować model aplikacji, aby dostosować sposób zachowania routingu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-449">Can be written to modify the application model to customize how routing behaves.</span></span>
* <span data-ttu-id="88f8a-450">Są odczytywane podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-450">Are read at app startup.</span></span>

<span data-ttu-id="88f8a-451">W tej sekcji przedstawiono podstawowy przykład dostosowywania routingu przy użyciu modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-451">This section shows a basic example of customizing routing using application model.</span></span> <span data-ttu-id="88f8a-452">Poniższy kod sprawia, że trasy są w przybliżeniu zgodne ze strukturą folderów projektu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-452">The following code makes routes roughly line up with the folder structure of the project.</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet)]

<span data-ttu-id="88f8a-453">Poniższy kod uniemożliwia `namespace` stosowanie Konwencji do kontrolerów, które są kierowane przez atrybut:</span><span class="sxs-lookup"><span data-stu-id="88f8a-453">The following code prevents the `namespace` convention from being applied to controllers that are attribute routed:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/NamespaceRoutingConvention.cs?name=snippet2)]

<span data-ttu-id="88f8a-454">Na przykład następujący kontroler nie używa `NamespaceRoutingConvention` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-454">For example, the following controller doesn't use `NamespaceRoutingConvention`:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/ManagersController.cs?name=snippet&highlight=1)]

<span data-ttu-id="88f8a-455">`NamespaceRoutingConvention.Apply`Metoda:</span><span class="sxs-lookup"><span data-stu-id="88f8a-455">The `NamespaceRoutingConvention.Apply` method:</span></span>

* <span data-ttu-id="88f8a-456">Nie robi nic, Jeśli kontroler ma przypisany atrybut.</span><span class="sxs-lookup"><span data-stu-id="88f8a-456">Does nothing if the controller is attribute routed.</span></span>
* <span data-ttu-id="88f8a-457">Ustawia szablon szablonu oparty na `namespace` , z `namespace` usuniętym podstawowym.</span><span class="sxs-lookup"><span data-stu-id="88f8a-457">Sets the controllers template based on the `namespace`, with the base `namespace` removed.</span></span>

<span data-ttu-id="88f8a-458">`NamespaceRoutingConvention`Można zastosować w `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-458">The `NamespaceRoutingConvention` can be applied in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Startup.cs?name=snippet&highlight=1,14-18)]

<span data-ttu-id="88f8a-459">Rozważmy na przykład następujący kontroler:</span><span class="sxs-lookup"><span data-stu-id="88f8a-459">For example, consider the following controller:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/UsersController.cs)]

<span data-ttu-id="88f8a-460">Powyższy kod ma następujące działanie:</span><span class="sxs-lookup"><span data-stu-id="88f8a-460">In the preceding code:</span></span>

* <span data-ttu-id="88f8a-461">Podstawą `namespace` jest `My.Application` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-461">The base `namespace` is `My.Application`.</span></span>
* <span data-ttu-id="88f8a-462">Pełna nazwa poprzedniego kontrolera to `My.Application.Admin.Controllers.UsersController` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-462">The full name of the preceding controller is `My.Application.Admin.Controllers.UsersController`.</span></span>
* <span data-ttu-id="88f8a-463">`NamespaceRoutingConvention`Ustawia szablon controllers na `Admin/Controllers/Users/[action]/{id?` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-463">The `NamespaceRoutingConvention` sets the controllers template to `Admin/Controllers/Users/[action]/{id?`.</span></span>

<span data-ttu-id="88f8a-464">`NamespaceRoutingConvention`Można również zastosować jako atrybut na kontrolerze:</span><span class="sxs-lookup"><span data-stu-id="88f8a-464">The `NamespaceRoutingConvention` can also be applied as an attribute on a controller:</span></span>

[!code-csharp[](routing/samples/3.x/nsrc/Controllers/TestController.cs?name=snippet&highlight=1)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="88f8a-465">Routing mieszany: Routing atrybutów a konwencjonalny Routing</span><span class="sxs-lookup"><span data-stu-id="88f8a-465">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="88f8a-466">Aplikacje ASP.NET Core mogą łączyć użycie konwencjonalnych routingu i routingu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-466">ASP.NET Core apps can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="88f8a-467">Typowym zastosowaniem są trasy konwencjonalne dla kontrolerów obsługujących strony HTML dla przeglądarek i routingu atrybutów dla kontrolerów obsługujących interfejsy API REST.</span><span class="sxs-lookup"><span data-stu-id="88f8a-467">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="88f8a-468">Akcje są albo podkierowane do Konwencji lub kierowane przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="88f8a-468">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="88f8a-469">Umieszczenie trasy na kontrolerze lub akcja powoduje, że atrybut jest kierowany.</span><span class="sxs-lookup"><span data-stu-id="88f8a-469">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="88f8a-470">Akcje definiujące trasy atrybutów nie mogą być osiągane za pomocą konwencjonalnych tras i vice versa.</span><span class="sxs-lookup"><span data-stu-id="88f8a-470">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="88f8a-471">**Każdy** atrybut trasy na kontrolerze powoduje kierowanie **wszystkich** akcji w atrybucie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="88f8a-471">**Any** route attribute on the controller makes **all** actions in the controller attribute routed.</span></span>

<span data-ttu-id="88f8a-472">Routing atrybutów i Routing konwencjonalny używają tego samego aparatu routingu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-472">Attribute routing and conventional routing use the same routing engine.</span></span>

<a name="routing-url-gen-ref-label"></a>
<a name="ambient"></a>

## <a name="url-generation-and-ambient-values"></a><span data-ttu-id="88f8a-473">Generowanie adresów URL i wartości otoczenia</span><span class="sxs-lookup"><span data-stu-id="88f8a-473">URL Generation and ambient values</span></span>

<span data-ttu-id="88f8a-474">Aplikacje mogą używać funkcji generowania adresów URL routingu do generowania linków URL do akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-474">Apps can use routing URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="88f8a-475">Generowanie adresów URL eliminuje adresy URL zakodowana, co sprawia, że kod jest bardziej niezawodny i konserwowany.</span><span class="sxs-lookup"><span data-stu-id="88f8a-475">Generating URLs eliminates hardcoding URLs, making code more robust and maintainable.</span></span> <span data-ttu-id="88f8a-476">Ta sekcja koncentruje się na funkcjach generowania adresów URL udostępnianych przez MVC i obejmuje jedynie podstawowe informacje na temat sposobu działania generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-476">This section focuses on the URL generation features provided by MVC and only cover basics of how URL generation works.</span></span> <span data-ttu-id="88f8a-477">Aby uzyskać szczegółowy opis generowania adresów URL, zobacz temat [Routing](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-477">See [Routing](xref:fundamentals/routing) for a detailed description of URL generation.</span></span>

<span data-ttu-id="88f8a-478"><xref:Microsoft.AspNetCore.Mvc.IUrlHelper>Interfejs jest podstawowym elementem infrastruktury między MVC i Routing na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-478">The <xref:Microsoft.AspNetCore.Mvc.IUrlHelper> interface is the underlying element of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="88f8a-479">Wystąpienie `IUrlHelper` jest dostępne za pomocą `Url` właściwości w kontrolerach, widokach i składnikach widoku.</span><span class="sxs-lookup"><span data-stu-id="88f8a-479">An instance of `IUrlHelper` is available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="88f8a-480">W poniższym przykładzie `IUrlHelper` interfejs jest używany przez `Controller.Url` Właściwość w celu WYGENEROWANIA adresu URL dla innej akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-480">In the following example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="88f8a-481">Jeśli aplikacja używa domyślnej trasy konwencjonalnej, wartość `url` zmiennej jest ciągiem ścieżki URL `/UrlGeneration/Destination` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-481">If the app is using the default conventional route, the value of the `url` variable is the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="88f8a-482">Ta ścieżka URL jest tworzona przez połączenie za pomocą routingu:</span><span class="sxs-lookup"><span data-stu-id="88f8a-482">This URL path is created by routing by combining:</span></span>

* <span data-ttu-id="88f8a-483">Wartości trasy z bieżącego żądania, które są nazywane **wartościami otoczenia**.</span><span class="sxs-lookup"><span data-stu-id="88f8a-483">The route values from the current request, which are called **ambient values**.</span></span>
* <span data-ttu-id="88f8a-484">Wartości przesyłane do `Url.Action` i podstawiające te wartości do szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="88f8a-484">The values passed to `Url.Action` and substituting those values into the route template:</span></span>

``` text
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="88f8a-485">Każdy parametr trasy w szablonie trasy ma swoją wartość zastępowaną przez pasujące nazwy wartościami i wartościami otoczenia.</span><span class="sxs-lookup"><span data-stu-id="88f8a-485">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="88f8a-486">Parametr trasy, który nie ma wartości, może:</span><span class="sxs-lookup"><span data-stu-id="88f8a-486">A route parameter that doesn't have a value can:</span></span>

* <span data-ttu-id="88f8a-487">Użyj wartości domyślnej, jeśli ma ją.</span><span class="sxs-lookup"><span data-stu-id="88f8a-487">Use a default value if it has one.</span></span>
* <span data-ttu-id="88f8a-488">Można pominąć, jeśli jest to opcjonalne.</span><span class="sxs-lookup"><span data-stu-id="88f8a-488">Be skipped if it's optional.</span></span> <span data-ttu-id="88f8a-489">Na przykład, `id` z szablonu trasy `{controller}/{action}/{id?}` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-489">For example, the `id` from the  route template `{controller}/{action}/{id?}`.</span></span>

<span data-ttu-id="88f8a-490">Generowanie adresu URL kończy się niepowodzeniem, jeśli którykolwiek z wymaganych parametrów trasy nie ma odpowiadającej mu wartości.</span><span class="sxs-lookup"><span data-stu-id="88f8a-490">URL generation fails if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="88f8a-491">Jeśli generowanie adresów URL kończy się niepowodzeniem dla trasy, kolejna trasa zostanie ponowiona do momentu przetworzenia wszystkich tras lub znalezienia dopasowania.</span><span class="sxs-lookup"><span data-stu-id="88f8a-491">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="88f8a-492">W poprzednim przykładzie `Url.Action` przyjęto założenie [konwencjonalnego routingu](#cr).</span><span class="sxs-lookup"><span data-stu-id="88f8a-492">The preceding example of `Url.Action` assumes [conventional routing](#cr).</span></span> <span data-ttu-id="88f8a-493">Generowanie adresów URL działa podobnie jak w przypadku [routingu atrybutów](#ar), chociaż koncepcje różnią się.</span><span class="sxs-lookup"><span data-stu-id="88f8a-493">URL generation works similarly with [attribute routing](#ar), though the concepts are different.</span></span> <span data-ttu-id="88f8a-494">Z routingiem konwencjonalnym:</span><span class="sxs-lookup"><span data-stu-id="88f8a-494">With conventional routing:</span></span>

* <span data-ttu-id="88f8a-495">Wartości trasy służą do rozszerzania szablonu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-495">The route values are used to expand a template.</span></span>
* <span data-ttu-id="88f8a-496">Wartości trasy dla `controller` i `action` zwykle pojawiają się w tym szablonie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-496">The route values for `controller` and `action` usually appear in that template.</span></span> <span data-ttu-id="88f8a-497">Dzieje się tak, ponieważ adresy URL dopasowane przez Routing są zgodne z Konwencją.</span><span class="sxs-lookup"><span data-stu-id="88f8a-497">This works because the URLs matched by routing adhere to a convention.</span></span>

<span data-ttu-id="88f8a-498">W poniższym przykładzie zastosowano Routing atrybutów:</span><span class="sxs-lookup"><span data-stu-id="88f8a-498">The following example uses attribute routing:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGenerationAttrController.cs?name=snippet_1)]

<span data-ttu-id="88f8a-499">`Source`Akcja w poprzednim kodzie generuje `custom/url/to/destination` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-499">The `Source` action in the preceding code generates `custom/url/to/destination`.</span></span>

<span data-ttu-id="88f8a-500"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> został dodany w ASP.NET Core 3,0 jako alternatywa dla `IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-500"><xref:Microsoft.AspNetCore.Routing.LinkGenerator> was added in ASP.NET Core 3.0 as an alternative to `IUrlHelper`.</span></span> <span data-ttu-id="88f8a-501">`LinkGenerator` oferuje podobne, ale bardziej elastyczne funkcje.</span><span class="sxs-lookup"><span data-stu-id="88f8a-501">`LinkGenerator` offers similar but more flexible functionality.</span></span> <span data-ttu-id="88f8a-502">Każda metoda w systemie `IUrlHelper` ma również odpowiednią rodzinę metod `LinkGenerator` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-502">Each method on `IUrlHelper` has a corresponding family of methods on `LinkGenerator` as well.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="88f8a-503">Generowanie adresów URL według nazwy akcji</span><span class="sxs-lookup"><span data-stu-id="88f8a-503">Generating URLs by action name</span></span>

<span data-ttu-id="88f8a-504">Wartość [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), [LinkGenerator. GetPathByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*)i wszystkie powiązane przeciążenia są przeznaczone do generowania docelowego punktu końcowego przez określenie nazwy kontrolera i nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-504">[Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), [LinkGenerator.GetPathByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetPathByAction*), and all related overloads all are designed to generate the target endpoint by specifying a controller name and action name.</span></span>

<span data-ttu-id="88f8a-505">W przypadku używania `Url.Action` , bieżąca wartość trasy dla `controller` i `action` jest udostępniana przez środowisko uruchomieniowe:</span><span class="sxs-lookup"><span data-stu-id="88f8a-505">When using `Url.Action`, the current route values for `controller` and `action` are provided by the runtime:</span></span>

* <span data-ttu-id="88f8a-506">Wartość `controller` i `action` jest częścią zarówno [wartości otoczenia](#ambient) , jak i wartości.</span><span class="sxs-lookup"><span data-stu-id="88f8a-506">The value of `controller` and `action` are part of both [ambient values](#ambient) and values.</span></span> <span data-ttu-id="88f8a-507">Metoda `Url.Action` zawsze używa bieżących wartości `action` i `controller` i generuje ścieżkę URL, która jest przesyłana do bieżącej akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-507">The method `Url.Action` always uses the current values of `action` and `controller` and generates a URL path that routes to the current action.</span></span>

<span data-ttu-id="88f8a-508">Funkcja routingu próbuje użyć wartości w otoczeniu wartości, aby podać informacje, które nie zostały dostarczone podczas generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-508">Routing attempts to use the values in ambient values to fill in information that wasn't provided when generating a URL.</span></span> <span data-ttu-id="88f8a-509">Rozważ użycie trasy podobnej do `{a}/{b}/{c}/{d}` wartości otoczenia `{ a = Alice, b = Bob, c = Carol, d = David }` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-509">Consider a route like `{a}/{b}/{c}/{d}` with ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`:</span></span>

* <span data-ttu-id="88f8a-510">Routing ma wystarczającą ilość informacji do wygenerowania adresu URL bez dodatkowych wartości.</span><span class="sxs-lookup"><span data-stu-id="88f8a-510">Routing has enough information to generate a URL without any additional values.</span></span>
* <span data-ttu-id="88f8a-511">Routing ma wystarczającą ilość informacji, ponieważ wszystkie parametry tras mają wartość.</span><span class="sxs-lookup"><span data-stu-id="88f8a-511">Routing has enough information because all route parameters have a value.</span></span>

<span data-ttu-id="88f8a-512">Jeśli wartość `{ d = Donovan }` zostanie dodana:</span><span class="sxs-lookup"><span data-stu-id="88f8a-512">If the value `{ d = Donovan }` is added:</span></span>

* <span data-ttu-id="88f8a-513">Wartość `{ d = David }` jest ignorowana.</span><span class="sxs-lookup"><span data-stu-id="88f8a-513">The value `{ d = David }` is ignored.</span></span>
* <span data-ttu-id="88f8a-514">Wygenerowana Ścieżka adresu URL to `Alice/Bob/Carol/Donovan` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-514">The generated URL path is `Alice/Bob/Carol/Donovan`.</span></span>

<span data-ttu-id="88f8a-515">**Ostrzeżenie**: ścieżki URL są hierarchiczne.</span><span class="sxs-lookup"><span data-stu-id="88f8a-515">**Warning**: URL paths are hierarchical.</span></span> <span data-ttu-id="88f8a-516">W poprzednim przykładzie, jeśli wartość `{ c = Cheryl }` jest dodawana:</span><span class="sxs-lookup"><span data-stu-id="88f8a-516">In the preceding example, if the value `{ c = Cheryl }` is added:</span></span>

* <span data-ttu-id="88f8a-517">Obie wartości `{ c = Carol, d = David }` są ignorowane.</span><span class="sxs-lookup"><span data-stu-id="88f8a-517">Both of the values `{ c = Carol, d = David }` are ignored.</span></span>
* <span data-ttu-id="88f8a-518">Nie jest już dostępna wartość `d` i generowanie adresu URL nie powiodło się.</span><span class="sxs-lookup"><span data-stu-id="88f8a-518">There is no longer a value for `d` and URL generation fails.</span></span>
* <span data-ttu-id="88f8a-519">Wymagane wartości `c` i `d` muszą być określone w celu WYGENEROWANIA adresu URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-519">The desired values of `c` and `d` must be specified to generate a URL.</span></span>  

<span data-ttu-id="88f8a-520">Prawdopodobnie wystąpił problem z domyślną trasą `{controller}/{action}/{id?}` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-520">You might expect to hit this problem with the default route `{controller}/{action}/{id?}`.</span></span> <span data-ttu-id="88f8a-521">Ten problem występuje rzadko `Url.Action` , ponieważ zawsze jawnie określa `controller` `action` wartość i.</span><span class="sxs-lookup"><span data-stu-id="88f8a-521">This problem is rare in practice because `Url.Action` always explicitly specifies a `controller` and `action` value.</span></span>

<span data-ttu-id="88f8a-522">Kilka przeciążeń [adresu URL. akcja](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) przyjmuje obiekt wartości trasy, aby podać wartości parametrów trasy innych niż `controller` i `action` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-522">Several overloads of [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) take a route values object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="88f8a-523">Obiekt wartości trasy jest często używany z `id` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-523">The route values object is frequently used with `id`.</span></span> <span data-ttu-id="88f8a-524">Na przykład `Url.Action("Buy", "Products", new { id = 17 })`.</span><span class="sxs-lookup"><span data-stu-id="88f8a-524">For example, `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="88f8a-525">Obiekt wartości trasy:</span><span class="sxs-lookup"><span data-stu-id="88f8a-525">The route values object:</span></span>

* <span data-ttu-id="88f8a-526">Według Konwencji jest zwykle obiektem typu anonimowego.</span><span class="sxs-lookup"><span data-stu-id="88f8a-526">By convention is usually an object of anonymous type.</span></span>
* <span data-ttu-id="88f8a-527">Może to być `IDictionary<>` lub [poco](https://wikipedia.org/wiki/Plain_old_CLR_object)).</span><span class="sxs-lookup"><span data-stu-id="88f8a-527">Can be an `IDictionary<>` or a [POCO](https://wikipedia.org/wiki/Plain_old_CLR_object)).</span></span>

<span data-ttu-id="88f8a-528">Wszystkie dodatkowe wartości trasy, które nie pasują do parametrów trasy, są umieszczane w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="88f8a-528">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet)]

<span data-ttu-id="88f8a-529">Poprzedni kod generuje `/Products/Buy/17?color=red` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-529">The preceding code generates `/Products/Buy/17?color=red`.</span></span>

<span data-ttu-id="88f8a-530">Poniższy kod generuje bezwzględny adres URL:</span><span class="sxs-lookup"><span data-stu-id="88f8a-530">The following code generates an absolute URL:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/TestController.cs?name=snippet2)]

<span data-ttu-id="88f8a-531">Aby utworzyć bezwzględny adres URL, użyj jednego z następujących elementów:</span><span class="sxs-lookup"><span data-stu-id="88f8a-531">To create an absolute URL, use one of the following:</span></span>

* <span data-ttu-id="88f8a-532">Przeciążenie, które akceptuje `protocol` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-532">An overload that accepts a `protocol`.</span></span> <span data-ttu-id="88f8a-533">Na przykład poprzedzający kod.</span><span class="sxs-lookup"><span data-stu-id="88f8a-533">For example, the preceding code.</span></span>
* <span data-ttu-id="88f8a-534">[LinkGenerator. GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*), który domyślnie generuje bezwzględne identyfikatory URI.</span><span class="sxs-lookup"><span data-stu-id="88f8a-534">[LinkGenerator.GetUriByAction](xref:Microsoft.AspNetCore.Routing.ControllerLinkGeneratorExtensions.GetUriByAction*), which generates absolute URIs by default.</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generate-urls-by-route"></a><span data-ttu-id="88f8a-535">Generuj adresy URL według trasy</span><span class="sxs-lookup"><span data-stu-id="88f8a-535">Generate URLs by route</span></span>

<span data-ttu-id="88f8a-536">Poprzedni kod wykazał wygenerowanie adresu URL przez przekazanie go do kontrolera i nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-536">The preceding code demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="88f8a-537">`IUrlHelper` zapewnia także rodzinę [adresów URL. RouteUrl](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-537">`IUrlHelper` also provides the [Url.RouteUrl](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.RouteUrl*) family of methods.</span></span> <span data-ttu-id="88f8a-538">Te metody są podobne do [adresu URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), ale nie kopiują bieżących wartości `action` i `controller` do wartości trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-538">These methods are similar to [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*), but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="88f8a-539">Najczęstsze użycie `Url.RouteUrl` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-539">The most common usage of `Url.RouteUrl`:</span></span>

* <span data-ttu-id="88f8a-540">Określa nazwę trasy do wygenerowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-540">Specifies a route name to generate the URL.</span></span>
* <span data-ttu-id="88f8a-541">Zwykle nie określa kontrolera ani nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-541">Generally doesn't specify a controller or action name.</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/UrlGeneration2Controller.cs?name=snippet_1)]

<span data-ttu-id="88f8a-542">Następujący Razor plik generuje link HTML do `Destination_Route` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-542">The following Razor file generates an HTML link to the `Destination_Route`:</span></span>

[!code-cshtml[](routing/samples/3.x/main/Views/Shared/MyLink.cshtml)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generate-urls-in-html-and-razor"></a><span data-ttu-id="88f8a-543">Generuj adresy URL w kodzie HTML i Razor</span><span class="sxs-lookup"><span data-stu-id="88f8a-543">Generate URLs in HTML and Razor</span></span>

<span data-ttu-id="88f8a-544"><xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper> udostępnia <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper> metody [HTML. BeginForm](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*) oraz [HTML. ActionLink](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*) do wygenerowania `<form>` i `<a>` elementów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-544"><xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper> provides the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.HtmlHelper> methods [Html.BeginForm](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.BeginForm*) and [Html.ActionLink](xref:Microsoft.AspNetCore.Mvc.Rendering.IHtmlHelper.ActionLink*) to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="88f8a-545">Metody te używają metody [URL. Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) do generowania adresu URL i akceptują podobne argumenty.</span><span class="sxs-lookup"><span data-stu-id="88f8a-545">These methods use the [Url.Action](xref:Microsoft.AspNetCore.Mvc.IUrlHelper.Action*) method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="88f8a-546">`Url.RouteUrl`Pomocników dla programu `HtmlHelper` mają `Html.BeginRouteForm` `Html.RouteLink` podobną funkcjonalność.</span><span class="sxs-lookup"><span data-stu-id="88f8a-546">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="88f8a-547">TagHelpers Generuj adresy URL za pomocą `form` TagHelper i `<a>` TagHelper.</span><span class="sxs-lookup"><span data-stu-id="88f8a-547">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="88f8a-548">Oba te zastosowania `IUrlHelper` dla ich implementacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-548">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="88f8a-549">Aby uzyskać więcej informacji, zobacz [pomocników tagów w formularzach](xref:mvc/views/working-with-forms) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-549">See [Tag Helpers in forms](xref:mvc/views/working-with-forms) for more information.</span></span>

<span data-ttu-id="88f8a-550">W widokach `IUrlHelper` można korzystać z `Url` właściwości dla dowolnej generacji adresów URL ad hoc, która nie jest objęta powyższym.</span><span class="sxs-lookup"><span data-stu-id="88f8a-550">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="url-generation-in-action-results"></a><span data-ttu-id="88f8a-551">Generowanie adresów URL w wynikach akcji</span><span class="sxs-lookup"><span data-stu-id="88f8a-551">URL generation in Action Results</span></span>

<span data-ttu-id="88f8a-552">Poprzednie przykłady pokazano przy użyciu `IUrlHelper` w kontrolerze.</span><span class="sxs-lookup"><span data-stu-id="88f8a-552">The preceding examples showed using `IUrlHelper` in a controller.</span></span> <span data-ttu-id="88f8a-553">Najczęstszym sposobem użycia na kontrolerze jest wygenerowanie adresu URL jako części wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-553">The most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="88f8a-554"><xref:Microsoft.AspNetCore.Mvc.ControllerBase> <xref:Microsoft.AspNetCore.Mvc.Controller> Klasy bazowe i zapewniają wygodne metody dla wyników akcji, które odwołują się do innej akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-554">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase> and <xref:Microsoft.AspNetCore.Mvc.Controller> base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="88f8a-555">Jednym z typowych zastosowań jest przekierowanie po zaakceptowaniu danych wejściowych użytkownika:</span><span class="sxs-lookup"><span data-stu-id="88f8a-555">One typical usage is to redirect after accepting user input:</span></span>

[!code-csharp[](routing/samples/3.x/main/Controllers/CustomerController.cs?name=snippet)]

<span data-ttu-id="88f8a-556">Wynikiem akcji są metody fabryki, takie jak <xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction%2A> i, <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction%2A> postępuj zgodnie z podobnym wzorcem do metod w `IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-556">The action results factory methods such as <xref:Microsoft.AspNetCore.Mvc.ControllerBase.RedirectToAction%2A> and <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction%2A> follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="88f8a-557">Specjalny przypadek dla dedykowanych tras konwencjonalnych</span><span class="sxs-lookup"><span data-stu-id="88f8a-557">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="88f8a-558">Funkcja [routingu konwencjonalnego](#cr) może używać specjalnego rodzaju definicji trasy o nazwie [dedykowanej, konwencjonalnej trasy](#dcr).</span><span class="sxs-lookup"><span data-stu-id="88f8a-558">[Conventional routing](#cr) can use a special kind of route definition called a [dedicated conventional route](#dcr).</span></span> <span data-ttu-id="88f8a-559">W poniższym przykładzie trasa o nazwie `blog` jest dedykowaną konwencjonalną trasą:</span><span class="sxs-lookup"><span data-stu-id="88f8a-559">In the following example, the route named `blog` is a dedicated conventional route:</span></span>

[!code-csharp[](routing/samples/3.x/main/Startup.cs?name=snippet_1)]

<span data-ttu-id="88f8a-560">Przy użyciu powyższych definicji trasy program `Url.Action("Index", "Home")` generuje ścieżkę URL `/` przy użyciu `default` trasy, ale dlaczego?</span><span class="sxs-lookup"><span data-stu-id="88f8a-560">Using the preceding route definitions, `Url.Action("Index", "Home")` generates the URL path `/` using the `default` route, but why?</span></span> <span data-ttu-id="88f8a-561">Możliwe jest odpuszczenie, że wartości trasy `{ controller = Home, action = Index }` byłyby wystarczające do wygenerowania adresu URL przy użyciu `blog` , a wynikiem będzie `/blog?action=Index&controller=Home` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-561">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="88f8a-562">[Dedykowane konwencjonalne trasy](#dcr) polegają na specjalnym zachowaniu wartości domyślnych, które nie mają odpowiadającego parametru trasy, który uniemożliwia zbyt [zachłanne](xref:fundamentals/routing#greedy) trasy z generowaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-562">[Dedicated conventional routes](#dcr) rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being too [greedy](xref:fundamentals/routing#greedy) with URL generation.</span></span> <span data-ttu-id="88f8a-563">W takim przypadku wartości domyślne to `{ controller = Blog, action = Article }` , a ani nie `controller` `action` pojawiają się jako parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-563">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="88f8a-564">Gdy Routing wykonuje generowanie adresów URL, podane wartości muszą być zgodne z wartościami domyślnymi.</span><span class="sxs-lookup"><span data-stu-id="88f8a-564">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="88f8a-565">Generowanie adresu URL `blog` nie powiodło się, ponieważ wartości `{ controller = Home, action = Index }` nie są zgodne `{ controller = Blog, action = Article }` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-565">URL generation using `blog` fails because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="88f8a-566">Następnie usługa routingu wraca do wypróbowania `default` , która się powiedzie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-566">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="88f8a-567">Obszary</span><span class="sxs-lookup"><span data-stu-id="88f8a-567">Areas</span></span>

<span data-ttu-id="88f8a-568">[Obszary](xref:mvc/controllers/areas) są funkcją MVC służącą do organizowania powiązanych funkcji w grupie jako oddzielnej:</span><span class="sxs-lookup"><span data-stu-id="88f8a-568">[Areas](xref:mvc/controllers/areas) are an MVC feature used to organize related functionality into a group as a separate:</span></span>

* <span data-ttu-id="88f8a-569">Przestrzeń nazw routingu dla akcji kontrolera.</span><span class="sxs-lookup"><span data-stu-id="88f8a-569">Routing namespace for controller actions.</span></span>
* <span data-ttu-id="88f8a-570">Struktura folderów dla widoków.</span><span class="sxs-lookup"><span data-stu-id="88f8a-570">Folder structure for views.</span></span>

<span data-ttu-id="88f8a-571">Użycie obszarów umożliwia aplikacji posiadanie wielu kontrolerów o tej samej nazwie, o ile mają one różne obszary.</span><span class="sxs-lookup"><span data-stu-id="88f8a-571">Using areas allows an app to have multiple controllers with the same name, as long as they have different areas.</span></span> <span data-ttu-id="88f8a-572">Za pomocą obszarów tworzy hierarchię na potrzeby routingu przez dodanie kolejnego parametru trasy `area` do `controller` i `action` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-572">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="88f8a-573">W tej sekcji omówiono sposób, w jaki Routing współdziała z obszarami.</span><span class="sxs-lookup"><span data-stu-id="88f8a-573">This section discusses how routing interacts with areas.</span></span> <span data-ttu-id="88f8a-574">Zobacz [obszary](xref:mvc/controllers/areas) , aby uzyskać szczegółowe informacje na temat sposobu używania obszarów w widokach.</span><span class="sxs-lookup"><span data-stu-id="88f8a-574">See [Areas](xref:mvc/controllers/areas) for details about how areas are used with views.</span></span>

<span data-ttu-id="88f8a-575">Poniższy przykład konfiguruje MVC do używania domyślnej trasy konwencjonalnej i `area` trasy dla `area` nazwanego `Blog` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-575">The following example configures MVC to use the default conventional route and an `area` route for an `area` named `Blog`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="88f8a-576">W poprzednim kodzie <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute%2A> jest wywoływana w celu utworzenia `"blog_route"` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-576">In the preceding code, <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapAreaControllerRoute%2A> is called to create the `"blog_route"`.</span></span> <span data-ttu-id="88f8a-577">Drugi parametr, `"Blog"` , jest nazwą obszaru.</span><span class="sxs-lookup"><span data-stu-id="88f8a-577">The second parameter, `"Blog"`, is the area name.</span></span>

<span data-ttu-id="88f8a-578">W przypadku dopasowania ścieżki URL podobnej `/Manage/Users/AddUser` do `"blog_route"` trasy generuje wartości trasy `{ area = Blog, controller = Users, action = AddUser }` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-578">When matching a URL path like `/Manage/Users/AddUser`, the `"blog_route"` route generates the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="88f8a-579">`area`Wartość trasy jest generowana przez wartość domyślną dla `area` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-579">The `area` route value is produced by a default value for `area`.</span></span> <span data-ttu-id="88f8a-580">Trasa utworzona przez `MapAreaControllerRoute` jest równoważna następującym:</span><span class="sxs-lookup"><span data-stu-id="88f8a-580">The route created by `MapAreaControllerRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup2.cs?name=snippet2)]

<span data-ttu-id="88f8a-581">`MapAreaControllerRoute` tworzy trasę przy użyciu wartości domyślnej i ograniczenia dotyczącej `area` użycia podanej nazwy obszaru, w tym przypadku `Blog` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-581">`MapAreaControllerRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="88f8a-582">Wartość domyślna zapewnia, że trasa zawsze produkuje `{ area = Blog, ... }` , ograniczenie wymaga wartości `{ area = Blog, ... }` dla generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-582">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

<span data-ttu-id="88f8a-583">Routowanie konwencjonalne jest zależne od kolejności.</span><span class="sxs-lookup"><span data-stu-id="88f8a-583">Conventional routing is order-dependent.</span></span> <span data-ttu-id="88f8a-584">Ogólnie rzecz biorąc, trasy z obszarami powinny być umieszczone wcześniej, ponieważ są bardziej specyficzne niż trasy bez obszaru.</span><span class="sxs-lookup"><span data-stu-id="88f8a-584">In general, routes with areas should be placed earlier as they're more specific than routes without an area.</span></span>

<span data-ttu-id="88f8a-585">Korzystając z powyższego przykładu, wartości trasy `{ area = Blog, controller = Users, action = AddUser }` są zgodne z następującą akcją:</span><span class="sxs-lookup"><span data-stu-id="88f8a-585">Using the preceding example, the route values `{ area = Blog, controller = Users, action = AddUser }` match the following action:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="88f8a-586">Atrybut [[Area]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) wskazuje, co oznacza kontroler w ramach obszaru.</span><span class="sxs-lookup"><span data-stu-id="88f8a-586">The [[Area]](xref:Microsoft.AspNetCore.Mvc.AreaAttribute) attribute is what denotes a controller as part of an area.</span></span> <span data-ttu-id="88f8a-587">Ten kontroler znajduje się w `Blog` obszarze.</span><span class="sxs-lookup"><span data-stu-id="88f8a-587">This controller is in the `Blog` area.</span></span> <span data-ttu-id="88f8a-588">Kontrolery bez `[Area]` atrybutu nie są członkami żadnego obszaru i **nie** są zgodne, gdy `area` wartość trasy jest dostarczana przez Routing.</span><span class="sxs-lookup"><span data-stu-id="88f8a-588">Controllers without an `[Area]` attribute are not members of any area, and do **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="88f8a-589">W poniższym przykładzie tylko pierwszy kontroler może być zgodny z wartościami trasy `{ area = Blog, controller = Users, action = AddUser }` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-589">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

<span data-ttu-id="88f8a-590">Przestrzeń nazw każdego kontrolera jest pokazana w tym miejscu na potrzeby kompletności.</span><span class="sxs-lookup"><span data-stu-id="88f8a-590">The namespace of each controller is shown here for completeness.</span></span> <span data-ttu-id="88f8a-591">Jeśli poprzedni kontroler użył tego samego obszaru nazw, wygenerowany zostanie błąd kompilatora.</span><span class="sxs-lookup"><span data-stu-id="88f8a-591">If the preceding controllers used the same namespace, a compiler error would be generated.</span></span> <span data-ttu-id="88f8a-592">Przestrzenie nazw klas nie mają wpływu na Routing MVC.</span><span class="sxs-lookup"><span data-stu-id="88f8a-592">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="88f8a-593">Pierwsze dwa kontrolery są członkami obszarów i są zgodne tylko wtedy, gdy ich nazwa obszaru jest podana przez `area` wartość trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-593">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="88f8a-594">Trzeci kontroler nie jest członkiem żadnego obszaru i można go dopasować tylko wtedy, gdy nie ma żadnej wartości parametru `area` Routing.</span><span class="sxs-lookup"><span data-stu-id="88f8a-594">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

<a name="aa"></a>

<span data-ttu-id="88f8a-595">W warunkach pasujących do *nie ma żadnej* wartości, brak `area` wartości jest taka sama jak wartość `area` null lub pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="88f8a-595">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="88f8a-596">Podczas wykonywania akcji wewnątrz obszaru wartość trasy dla `area` jest dostępna jako [wartość otoczenia](#ambient) dla routingu do użycia na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-596">When executing an action inside an area, the route value for `area` is available as an [ambient value](#ambient) for routing to use for URL generation.</span></span> <span data-ttu-id="88f8a-597">Oznacza to, że domyślnie obszary programu *Sticky Notes* mają być używane do generowania adresów URL, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-597">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup3.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<span data-ttu-id="88f8a-598">Poniższy kod generuje adres URL `/Zebra/Users/AddUser` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-598">The following code generates a URL to `/Zebra/Users/AddUser`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/HomeController.cs?name=snippet)]

<a name="action"></a>

## <a name="action-definition"></a><span data-ttu-id="88f8a-599">Definicja akcji</span><span class="sxs-lookup"><span data-stu-id="88f8a-599">Action definition</span></span>

<span data-ttu-id="88f8a-600">Metody publiczne na kontrolerze, z wyjątkiem tych z atrybutem nie będącym [akcją](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute) , są akcjami.</span><span class="sxs-lookup"><span data-stu-id="88f8a-600">Public methods on a controller, except those with the [NonAction](xref:Microsoft.AspNetCore.Mvc.NonActionAttribute) attribute, are actions.</span></span>

## <a name="sample-code"></a><span data-ttu-id="88f8a-601">Przykładowy kod</span><span class="sxs-lookup"><span data-stu-id="88f8a-601">Sample code</span></span>

* [!INCLUDE[](~/includes/MyDisplayRouteInfo.md)]
* <span data-ttu-id="88f8a-602">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/mvc/controllers/routing/samples/3.x) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="88f8a-602">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/mvc/controllers/routing/samples/3.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

[!INCLUDE[](~/includes/dbg-route.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="88f8a-603">ASP.NET Core MVC używa [oprogramowania pośredniczącego](xref:fundamentals/middleware/index) routingu, aby dopasować adresy URL żądań przychodzących i zmapować je na akcje.</span><span class="sxs-lookup"><span data-stu-id="88f8a-603">ASP.NET Core MVC uses the Routing [middleware](xref:fundamentals/middleware/index) to match the URLs of incoming requests and map them to actions.</span></span> <span data-ttu-id="88f8a-604">Trasy są zdefiniowane w kodzie startowym lub atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-604">Routes are defined in startup code or attributes.</span></span> <span data-ttu-id="88f8a-605">Trasy opisują sposób dopasowywania ścieżek adresów URL do akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-605">Routes describe how URL paths should be matched to actions.</span></span> <span data-ttu-id="88f8a-606">Trasy są również używane do generowania adresów URL (dla linków) wysyłanych w odpowiedziach.</span><span class="sxs-lookup"><span data-stu-id="88f8a-606">Routes are also used to generate URLs (for links) sent out in responses.</span></span>

<span data-ttu-id="88f8a-607">Akcje są albo podkierowane do Konwencji lub kierowane przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="88f8a-607">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="88f8a-608">Umieszczenie trasy na kontrolerze lub akcja powoduje, że atrybut jest kierowany.</span><span class="sxs-lookup"><span data-stu-id="88f8a-608">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="88f8a-609">Aby uzyskać więcej informacji, zobacz [Routing mieszany](#routing-mixed-ref-label) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-609">See [Mixed routing](#routing-mixed-ref-label) for more information.</span></span>

<span data-ttu-id="88f8a-610">Ten dokument wyjaśnia interakcje między MVC i routingiem oraz sposób, w jaki typowe aplikacje MVC używają funkcji routingu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-610">This document will explain the interactions between MVC and routing, and how typical MVC apps make use of routing features.</span></span> <span data-ttu-id="88f8a-611">Zobacz [Routing](xref:fundamentals/routing) , aby uzyskać szczegółowe informacje na temat routingu zaawansowanego.</span><span class="sxs-lookup"><span data-stu-id="88f8a-611">See [Routing](xref:fundamentals/routing) for details on advanced routing.</span></span>

## <a name="setting-up-routing-middleware"></a><span data-ttu-id="88f8a-612">Konfigurowanie oprogramowania pośredniczącego routingu</span><span class="sxs-lookup"><span data-stu-id="88f8a-612">Setting up Routing Middleware</span></span>

<span data-ttu-id="88f8a-613">W metodzie *konfigurowania* może zostać wyświetlony kod podobny do:</span><span class="sxs-lookup"><span data-stu-id="88f8a-613">In your *Configure* method you may see code similar to:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="88f8a-614">Wewnątrz wywołania do `UseMvc` , służy `MapRoute` do tworzenia pojedynczej trasy, którą będziemy odnosić jako `default` trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-614">Inside the call to `UseMvc`, `MapRoute` is used to create a single route, which we'll refer to as the `default` route.</span></span> <span data-ttu-id="88f8a-615">Większość aplikacji MVC będzie używać trasy z szablonem podobnym do `default` trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-615">Most MVC apps will use a route with a template similar to the `default` route.</span></span>

<span data-ttu-id="88f8a-616">Szablon trasy `"{controller=Home}/{action=Index}/{id?}"` może być zgodny z ścieżką URL `/Products/Details/5` , tak jak i będzie wyodrębniał wartości trasy `{ controller = Products, action = Details, id = 5 }` przez tokenizowanie ścieżki.</span><span class="sxs-lookup"><span data-stu-id="88f8a-616">The route template `"{controller=Home}/{action=Index}/{id?}"` can match a URL path like `/Products/Details/5` and will extract the route values `{ controller = Products, action = Details, id = 5 }` by tokenizing the path.</span></span> <span data-ttu-id="88f8a-617">MVC spróbuje zlokalizować kontroler o nazwie `ProductsController` i uruchomić akcję `Details` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-617">MVC will attempt to locate a controller named `ProductsController` and run the action `Details`:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Details(int id) { ... }
}
```

<span data-ttu-id="88f8a-618">Należy zauważyć, że w tym przykładzie powiązanie modelu będzie używać wartości, `id = 5` Aby ustawić `id` parametr `5` podczas wywoływania tej akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-618">Note that in this example, model binding would use the value of `id = 5` to set the `id` parameter to `5` when invoking this action.</span></span> <span data-ttu-id="88f8a-619">Aby uzyskać więcej informacji, zobacz [powiązanie modelu](../models/model-binding.md) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-619">See the [Model Binding](../models/model-binding.md) for more details.</span></span>

<span data-ttu-id="88f8a-620">Przy użyciu `default` trasy:</span><span class="sxs-lookup"><span data-stu-id="88f8a-620">Using the `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="88f8a-621">Szablon trasy:</span><span class="sxs-lookup"><span data-stu-id="88f8a-621">The route template:</span></span>

* <span data-ttu-id="88f8a-622">`{controller=Home}` definiuje `Home` jako domyślne `controller`</span><span class="sxs-lookup"><span data-stu-id="88f8a-622">`{controller=Home}` defines `Home` as the default `controller`</span></span>

* <span data-ttu-id="88f8a-623">`{action=Index}` definiuje `Index` jako domyślne `action`</span><span class="sxs-lookup"><span data-stu-id="88f8a-623">`{action=Index}` defines `Index` as the default `action`</span></span>

* <span data-ttu-id="88f8a-624">`{id?}` definiuje `id` jako opcjonalne</span><span class="sxs-lookup"><span data-stu-id="88f8a-624">`{id?}` defines `id` as optional</span></span>

<span data-ttu-id="88f8a-625">Domyślne i opcjonalne parametry trasy nie muszą być obecne w ścieżce URL dla dopasowania.</span><span class="sxs-lookup"><span data-stu-id="88f8a-625">Default and optional route parameters don't need to be present in the URL path for a match.</span></span> <span data-ttu-id="88f8a-626">Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](xref:fundamentals/routing#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-626">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<span data-ttu-id="88f8a-627">`"{controller=Home}/{action=Index}/{id?}"` może być zgodna ze ścieżką URL `/` i będzie generować wartości trasy `{ controller = Home, action = Index }` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-627">`"{controller=Home}/{action=Index}/{id?}"` can match the URL path `/` and will produce the route values `{ controller = Home, action = Index }`.</span></span> <span data-ttu-id="88f8a-628">Wartości dla `controller` i `action` używają wartości domyślnych, `id` nie tworzy wartości, ponieważ w ścieżce URL nie ma odpowiedniego segmentu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-628">The values for `controller` and `action` make use of the default values, `id` doesn't produce a value since there's no corresponding segment in the URL path.</span></span> <span data-ttu-id="88f8a-629">Aby można było wybrać akcję i, MVC będzie używać tych wartości tras `HomeController` `Index` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-629">MVC would use these route values to select the `HomeController` and `Index` action:</span></span>

```csharp
public class HomeController : Controller
{
  public IActionResult Index() { ... }
}
```

<span data-ttu-id="88f8a-630">Przy użyciu tej definicji kontrolera i szablonu trasy `HomeController.Index` Akcja będzie wykonywana dla dowolnej z następujących ścieżek URL:</span><span class="sxs-lookup"><span data-stu-id="88f8a-630">Using this controller definition and route template, the `HomeController.Index` action would be executed for any of the following URL paths:</span></span>

* `/Home/Index/17`

* `/Home/Index`

* `/Home`

* `/`

<span data-ttu-id="88f8a-631">Wygodna metoda `UseMvcWithDefaultRoute` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-631">The convenience method `UseMvcWithDefaultRoute`:</span></span>

```csharp
app.UseMvcWithDefaultRoute();
```

<span data-ttu-id="88f8a-632">Może służyć do zastępowania:</span><span class="sxs-lookup"><span data-stu-id="88f8a-632">Can be used to replace:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="88f8a-633">`UseMvc` i `UseMvcWithDefaultRoute` Dodaj wystąpienie `RouterMiddleware` do potoku programu pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="88f8a-633">`UseMvc` and `UseMvcWithDefaultRoute` add an instance of `RouterMiddleware` to the middleware pipeline.</span></span> <span data-ttu-id="88f8a-634">MVC nie działa bezpośrednio w oprogramowaniu pośredniczącym i używa routingu do obsługi żądań.</span><span class="sxs-lookup"><span data-stu-id="88f8a-634">MVC doesn't interact directly with middleware, and uses routing to handle requests.</span></span> <span data-ttu-id="88f8a-635">MVC jest połączony z trasami za pomocą wystąpienia `MvcRouteHandler` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-635">MVC is connected to the routes through an instance of `MvcRouteHandler`.</span></span> <span data-ttu-id="88f8a-636">Kod w programie `UseMvc` jest podobny do następującego:</span><span class="sxs-lookup"><span data-stu-id="88f8a-636">The code inside of `UseMvc` is similar to the following:</span></span>

```csharp
var routes = new RouteBuilder(app);

// Add connection to MVC, will be hooked up by calls to MapRoute.
routes.DefaultHandler = new MvcRouteHandler(...);

// Execute callback to register routes.
// routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");

// Create route collection and add the middleware.
app.UseRouter(routes.Build());
```

<span data-ttu-id="88f8a-637">`UseMvc` nie definiuje bezpośrednio żadnych tras, dodaje symbol zastępczy do kolekcji tras dla `attribute` trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-637">`UseMvc` doesn't directly define any routes, it adds a placeholder to the route collection for the `attribute` route.</span></span> <span data-ttu-id="88f8a-638">Przeciążenie `UseMvc(Action<IRouteBuilder>)` umożliwia dodanie własnych tras, a także obsługuje routing atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-638">The overload `UseMvc(Action<IRouteBuilder>)` lets you add your own routes and also supports attribute routing.</span></span>  <span data-ttu-id="88f8a-639">`UseMvc` i wszystkie jego odmiany dodają symbol zastępczy dla atrybutu trasy — atrybut jest zawsze dostępny niezależnie od sposobu konfigurowania `UseMvc` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-639">`UseMvc` and all of its variations add a placeholder for the attribute route - attribute routing is always available regardless of how you configure `UseMvc`.</span></span> <span data-ttu-id="88f8a-640">`UseMvcWithDefaultRoute` Definiuje domyślną trasę i obsługuje routing atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-640">`UseMvcWithDefaultRoute` defines a default route and supports attribute routing.</span></span> <span data-ttu-id="88f8a-641">Sekcja [Routing atrybutów](#attribute-routing-ref-label) zawiera więcej szczegółów dotyczących routingu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-641">The [Attribute Routing](#attribute-routing-ref-label) section includes more details on attribute routing.</span></span>

<a name="routing-conventional-ref-label"></a>

## <a name="conventional-routing"></a><span data-ttu-id="88f8a-642">Routing konwencjonalny</span><span class="sxs-lookup"><span data-stu-id="88f8a-642">Conventional routing</span></span>

<span data-ttu-id="88f8a-643">`default`Trasa:</span><span class="sxs-lookup"><span data-stu-id="88f8a-643">The `default` route:</span></span>

```csharp
routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
```

<span data-ttu-id="88f8a-644">Poprzedni kod jest przykładem konwencjonalnego routingu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-644">The preceding code is an example of a conventional routing.</span></span> <span data-ttu-id="88f8a-645">Ten styl jest nazywany routingiem konwencjonalnym, ponieważ ustanawia *Konwencję* dla ścieżek adresów URL:</span><span class="sxs-lookup"><span data-stu-id="88f8a-645">This style is called conventional routing because it establishes a *convention* for URL paths:</span></span>

* <span data-ttu-id="88f8a-646">Pierwszy segment ścieżki jest mapowany na nazwę kontrolera.</span><span class="sxs-lookup"><span data-stu-id="88f8a-646">The first path segment maps to the controller name.</span></span>
* <span data-ttu-id="88f8a-647">Druga mapowanie na nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-647">The second maps to the action name.</span></span>
* <span data-ttu-id="88f8a-648">Trzeci segment jest używany opcjonalnie `id` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-648">The third segment is used for an optional `id`.</span></span> <span data-ttu-id="88f8a-649">`id` mapuje do jednostki modelu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-649">`id` maps to a model entity.</span></span>

<span data-ttu-id="88f8a-650">Przy użyciu tej `default` trasy ścieżka URL jest `/Products/List` mapowana na `ProductsController.List` akcję i jest `/Blog/Article/17` mapowana na `BlogController.Article` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-650">Using this `default` route, the URL path `/Products/List` maps to the `ProductsController.List` action, and `/Blog/Article/17` maps to `BlogController.Article`.</span></span> <span data-ttu-id="88f8a-651">To mapowanie jest oparte **tylko** na nazwach kontrolera i akcji, a nie na podstawie przestrzeni nazw, lokalizacji plików źródłowych ani parametrów metody.</span><span class="sxs-lookup"><span data-stu-id="88f8a-651">This mapping is based on the controller and action names **only** and isn't based on namespaces, source file locations, or method parameters.</span></span>

> [!TIP]
> <span data-ttu-id="88f8a-652">Użycie konwencjonalnego routingu z domyślną trasą umożliwia szybkie Kompilowanie aplikacji bez konieczności podawania nowego wzorca adresu URL dla każdej zdefiniowanej akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-652">Using conventional routing with the default route allows you to build the application quickly without having to come up with a new URL pattern for each action you define.</span></span> <span data-ttu-id="88f8a-653">W przypadku aplikacji z akcjami w stylu CRUD zachowanie spójności adresów URL na kontrolerach może pomóc uprościć kod i uczynić interfejs użytkownika bardziej przewidywalny.</span><span class="sxs-lookup"><span data-stu-id="88f8a-653">For an application with CRUD style actions, having consistency for the URLs across your controllers can help simplify your code and make your UI more predictable.</span></span>

> [!WARNING]
> <span data-ttu-id="88f8a-654">`id`Jest zdefiniowany jako opcjonalny przez szablon trasy, co oznacza, że akcje mogą być wykonywane bez identyfikatora dostarczonego jako część adresu URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-654">The `id` is defined as optional by the route template, meaning that your actions can execute without the ID provided as part of the URL.</span></span> <span data-ttu-id="88f8a-655">Typowo, co się stanie, jeśli zostanie `id` pominięty z adresu URL, to zostanie ono ustawione na `0` podstawie powiązania modelu, a w rezultacie nie zostanie znaleziona żadna jednostka w dopasowaniu do bazy danych `id == 0` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-655">Usually what will happen if `id` is omitted from the URL is that it will be set to `0` by model binding, and as a result no entity will be found in the database matching `id == 0`.</span></span> <span data-ttu-id="88f8a-656">Routing atrybutu może dać szczegółowy formant, który ma być wymagany dla niektórych akcji, a nie dla innych.</span><span class="sxs-lookup"><span data-stu-id="88f8a-656">Attribute routing can give you fine-grained control to make the ID required for some actions and not for others.</span></span> <span data-ttu-id="88f8a-657">Zgodnie z Konwencją dokumentacja będzie zawierać opcjonalne parametry, takie jak, `id` gdy prawdopodobnie będą widoczne w prawidłowym użyciu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-657">By convention the documentation will include optional parameters like `id` when they're likely to appear in correct usage.</span></span>

## <a name="multiple-routes"></a><span data-ttu-id="88f8a-658">Wiele tras</span><span class="sxs-lookup"><span data-stu-id="88f8a-658">Multiple routes</span></span>

<span data-ttu-id="88f8a-659">Można dodać wiele tras wewnątrz `UseMvc` , dodając więcej wywołań do `MapRoute` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-659">You can add multiple routes inside `UseMvc` by adding more calls to `MapRoute`.</span></span> <span data-ttu-id="88f8a-660">Dzięki temu można zdefiniować wiele Konwencji lub dodać trasy konwencjonalne, które są przeznaczone dla konkretnej akcji, na przykład:</span><span class="sxs-lookup"><span data-stu-id="88f8a-660">Doing so allows you to define multiple conventions, or to add conventional routes that are dedicated to a specific action, such as:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
            defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="88f8a-661">`blog`Trasa w tym miejscu jest *dedykowaną tradycyjną trasą*, co oznacza, że używa systemu routingu konwencjonalnego, ale jest przeznaczona dla konkretnej akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-661">The `blog` route here is a *dedicated conventional route*, meaning that it uses the conventional routing system, but is dedicated to a specific action.</span></span> <span data-ttu-id="88f8a-662">Ponieważ `controller` i `action` nie pojawiają się w szablonie trasy jako parametry, mogą mieć tylko wartości domyślne i w związku z tym trasa będzie zawsze mapowana na akcję `BlogController.Article` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-662">Since `controller` and `action` don't appear in the route template as parameters, they can only have the default values, and thus this route will always map to the action `BlogController.Article`.</span></span>

<span data-ttu-id="88f8a-663">Trasy w kolekcji tras są uporządkowane i będą przetwarzane w kolejności, w jakiej zostały dodane.</span><span class="sxs-lookup"><span data-stu-id="88f8a-663">Routes in the route collection are ordered, and will be processed in the order they're added.</span></span> <span data-ttu-id="88f8a-664">W tym przykładzie `blog` zostanie podjęta próba trasy przed `default` trasą.</span><span class="sxs-lookup"><span data-stu-id="88f8a-664">So in this example, the `blog` route will be tried before the `default` route.</span></span>

> [!NOTE]
> <span data-ttu-id="88f8a-665">*Dedykowane konwencjonalne trasy* często korzystają z parametrów trasy **catch-all** , takich jak `{*article}` przechwytywanie pozostałej części ścieżki URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-665">*Dedicated conventional routes* often use **catch-all** route parameters like `{*article}` to capture the remaining portion of the URL path.</span></span> <span data-ttu-id="88f8a-666">Może to spowodować, że trasa "zbyt zachłanne" oznacza, że pasuje do adresów URL, które mają być dopasowane przez inne trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-666">This can make a route 'too greedy' meaning that it matches URLs that you intended to be matched by other routes.</span></span> <span data-ttu-id="88f8a-667">Umieść trasy "zachłanne" później w tabeli tras, aby rozwiązać ten problem.</span><span class="sxs-lookup"><span data-stu-id="88f8a-667">Put the 'greedy' routes later in the route table to solve this.</span></span>

### <a name="fallback"></a><span data-ttu-id="88f8a-668">Opcja rezerwowa</span><span class="sxs-lookup"><span data-stu-id="88f8a-668">Fallback</span></span>

<span data-ttu-id="88f8a-669">W ramach przetwarzania żądań MVC sprawdzi, czy wartości trasy mogą być używane do znajdowania kontrolera i akcji w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-669">As part of request processing, MVC will verify that the route values can be used to find a controller and action in your application.</span></span> <span data-ttu-id="88f8a-670">Jeśli wartości trasy nie są zgodne z akcją, trasa nie jest uważana za dopasowanie i zostanie podjęta kolejna trasa.</span><span class="sxs-lookup"><span data-stu-id="88f8a-670">If the route values don't match an action then the route isn't considered a match, and the next route will be tried.</span></span> <span data-ttu-id="88f8a-671">Jest to nazywane *Fallback* i ma na celu uproszczenie przypadków, w których trasy konwencjonalne nakładają się na siebie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-671">This is called *fallback*, and it's intended to simplify cases where conventional routes overlap.</span></span>

### <a name="disambiguating-actions"></a><span data-ttu-id="88f8a-672">Niejednoznaczne akcje</span><span class="sxs-lookup"><span data-stu-id="88f8a-672">Disambiguating actions</span></span>

<span data-ttu-id="88f8a-673">Gdy dwie akcje są zgodne z routingiem, MVC musi odróżnić się, aby wybrać najlepszy kandydat lub w przeciwnym razie zgłosić wyjątek.</span><span class="sxs-lookup"><span data-stu-id="88f8a-673">When two actions match through routing, MVC must disambiguate to choose the 'best' candidate or else throw an exception.</span></span> <span data-ttu-id="88f8a-674">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="88f8a-674">For example:</span></span>

```csharp
public class ProductsController : Controller
{
   public IActionResult Edit(int id) { ... }

   [HttpPost]
   public IActionResult Edit(int id, Product product) { ... }
}
```

<span data-ttu-id="88f8a-675">Ten kontroler definiuje dwie akcje, które byłyby zgodne ze ścieżką URL `/Products/Edit/17` i danymi tras `{ controller = Products, action = Edit, id = 17 }` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-675">This controller defines two actions that would match the URL path `/Products/Edit/17` and route data `{ controller = Products, action = Edit, id = 17 }`.</span></span> <span data-ttu-id="88f8a-676">Jest to typowy wzorzec dla kontrolerów MVC, gdzie `Edit(int)` pokazuje formularz służący do edytowania produktu i `Edit(int, Product)` przetwarza opublikowany formularz.</span><span class="sxs-lookup"><span data-stu-id="88f8a-676">This is a typical pattern for MVC controllers where `Edit(int)` shows a form to edit a product, and `Edit(int, Product)` processes  the posted form.</span></span> <span data-ttu-id="88f8a-677">Aby ten możliwy składnik MVC musiał wybrać, `Edit(int, Product)` gdy żądanie jest http `POST` i `Edit(int)` gdy zlecenie http jest inne.</span><span class="sxs-lookup"><span data-stu-id="88f8a-677">To make this possible MVC would need to choose `Edit(int, Product)` when the request is an HTTP `POST` and `Edit(int)` when the HTTP verb is anything else.</span></span>

<span data-ttu-id="88f8a-678">`HttpPostAttribute`( `[HttpPost]` ) Jest implementacją programu `IActionConstraint` , która będzie zezwalać na wybór akcji tylko wtedy, gdy czasownik http to `POST` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-678">The `HttpPostAttribute` ( `[HttpPost]` ) is an implementation of `IActionConstraint` that will only allow the action to be selected when the HTTP verb is `POST`.</span></span> <span data-ttu-id="88f8a-679">Obecność elementu sprawia, że `IActionConstraint` `Edit(int, Product)` lepszym rozwiązaniem jest dopasowanie `Edit(int)` , więc `Edit(int, Product)` zostanie ono najpierw ponowione.</span><span class="sxs-lookup"><span data-stu-id="88f8a-679">The presence of an `IActionConstraint` makes the `Edit(int, Product)` a 'better' match than `Edit(int)`, so `Edit(int, Product)` will be tried first.</span></span>

<span data-ttu-id="88f8a-680">Musisz tylko pisać `IActionConstraint` implementacje niestandardowe w wyspecjalizowanych scenariuszach, ale ważne jest, aby zrozumieć rolę atrybutów, takich jak `HttpPostAttribute`  atrybuty podobne do innych czasowników HTTP.</span><span class="sxs-lookup"><span data-stu-id="88f8a-680">You will only need to write custom `IActionConstraint` implementations in specialized scenarios, but it's important to understand the role of attributes like `HttpPostAttribute`  - similar attributes are defined for other HTTP verbs.</span></span> <span data-ttu-id="88f8a-681">W przypadku routingu konwencjonalnego typowe dla akcji używanie tej samej nazwy akcji, gdy są one częścią `show form -> submit form` przepływu pracy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-681">In conventional routing it's common for actions to use the same action name when they're part of a `show form -> submit form` workflow.</span></span> <span data-ttu-id="88f8a-682">Wygoda tego wzorca stanie się bardziej oczywista po przejrzeniu sekcji [zrozumienie IActionConstraint](#understanding-iactionconstraint) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-682">The convenience of this pattern will become more apparent after reviewing the [Understanding IActionConstraint](#understanding-iactionconstraint) section.</span></span>

<span data-ttu-id="88f8a-683">Jeśli wiele pasujących tras i MVC nie mogą znaleźć "najlepszej" trasy, spowoduje to zgłoszenie `AmbiguousActionException` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-683">If multiple routes match, and MVC can't find a 'best' route, it will throw an `AmbiguousActionException`.</span></span>

<a name="routing-route-name-ref-label"></a>

### <a name="route-names"></a><span data-ttu-id="88f8a-684">Nazwy tras</span><span class="sxs-lookup"><span data-stu-id="88f8a-684">Route names</span></span>

<span data-ttu-id="88f8a-685">Ciągi  `"blog"` i `"default"` w poniższych przykładach są nazwami tras:</span><span class="sxs-lookup"><span data-stu-id="88f8a-685">The strings  `"blog"` and `"default"` in the following examples are route names:</span></span>

```csharp
app.UseMvc(routes =>
{
   routes.MapRoute("blog", "blog/{*article}",
               defaults: new { controller = "Blog", action = "Article" });
   routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="88f8a-686">Nazwy tras nadaj logicznej nazwie trasy, aby nazwana trasa mogła być używana do generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-686">The route names give the route a logical name so that the named route can be used for URL generation.</span></span> <span data-ttu-id="88f8a-687">Znacznie upraszcza to tworzenie adresów URL, gdy porządkowanie tras może spowodować skomplikowane generowanie adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-687">This greatly simplifies URL creation when the ordering of routes could make URL generation complicated.</span></span> <span data-ttu-id="88f8a-688">Nazwy tras muszą być unikatowe w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-688">Route names must be unique application-wide.</span></span>

<span data-ttu-id="88f8a-689">Nazwy tras nie mają wpływu na Dopasowywanie adresów URL ani obsługę żądań; są one używane tylko do generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-689">Route names have no impact on URL matching or handling of requests; they're used only for URL generation.</span></span> <span data-ttu-id="88f8a-690">[Routing](xref:fundamentals/routing) zawiera bardziej szczegółowe informacje na temat generowania adresów URL, w tym generowanie adresów URL w pomocnikach specyficznych dla MVC.</span><span class="sxs-lookup"><span data-stu-id="88f8a-690">[Routing](xref:fundamentals/routing) has more detailed information on URL generation including URL generation in MVC-specific helpers.</span></span>

<a name="attribute-routing-ref-label"></a>

## <a name="attribute-routing"></a><span data-ttu-id="88f8a-691">Routing atrybutów</span><span class="sxs-lookup"><span data-stu-id="88f8a-691">Attribute routing</span></span>

<span data-ttu-id="88f8a-692">Funkcja routingu atrybutów używa zestawu atrybutów do mapowania akcji bezpośrednio do szablonów tras.</span><span class="sxs-lookup"><span data-stu-id="88f8a-692">Attribute routing uses a set of attributes to map actions directly to route templates.</span></span> <span data-ttu-id="88f8a-693">W poniższym przykładzie `app.UseMvc();` jest używany w `Configure` metodzie i nie jest przesyłana żadna trasa.</span><span class="sxs-lookup"><span data-stu-id="88f8a-693">In the following example, `app.UseMvc();` is used in the `Configure` method and no route is passed.</span></span> <span data-ttu-id="88f8a-694">`HomeController`Będzie zgodna z zestawem adresów URL podobnym do trasy domyślnej, która `{controller=Home}/{action=Index}/{id?}` byłaby zgodna:</span><span class="sxs-lookup"><span data-stu-id="88f8a-694">The `HomeController` will match a set of URLs similar to what the default route `{controller=Home}/{action=Index}/{id?}` would match:</span></span>

```csharp
public class HomeController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult Index()
   {
      return View();
   }
   [Route("Home/About")]
   public IActionResult About()
   {
      return View();
   }
   [Route("Home/Contact")]
   public IActionResult Contact()
   {
      return View();
   }
}
```

<span data-ttu-id="88f8a-695">`HomeController.Index()`Akcja będzie wykonywana dla dowolnej ścieżki URL `/` , `/Home` lub `/Home/Index` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-695">The `HomeController.Index()` action will be executed for any of the URL paths `/`, `/Home`, or `/Home/Index`.</span></span>

> [!NOTE]
> <span data-ttu-id="88f8a-696">W tym przykładzie przedstawiono najważniejsze różnice programistyczne między routingiem atrybutów i routingiem konwencjonalnym.</span><span class="sxs-lookup"><span data-stu-id="88f8a-696">This example highlights a key programming difference between attribute routing and conventional routing.</span></span> <span data-ttu-id="88f8a-697">Routing atrybutów wymaga więcej danych wejściowych w celu określenia trasy; konwencjonalne trasy domyślne obsługuje trasy bardziej zwięzłie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-697">Attribute routing requires more input to specify a route; the conventional default route handles routes more succinctly.</span></span> <span data-ttu-id="88f8a-698">Jednak Routing atrybutu zezwala na (i wymaga) precyzyjnej kontroli, które szablony tras mają zastosowanie do poszczególnych akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-698">However, attribute routing allows (and requires) precise control of which route templates apply to each action.</span></span>

<span data-ttu-id="88f8a-699">W przypadku routingu atrybutów nazwa kontrolera i nazwy akcji nie odgrywają **żadnej** roli, w której akcja jest zaznaczona.</span><span class="sxs-lookup"><span data-stu-id="88f8a-699">With attribute routing the controller name and action names play **no** role in which action is selected.</span></span> <span data-ttu-id="88f8a-700">Ten przykład będzie pasował do tych samych adresów URL, co w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-700">This example will match the same URLs as the previous example.</span></span>

```csharp
public class MyDemoController : Controller
{
   [Route("")]
   [Route("Home")]
   [Route("Home/Index")]
   public IActionResult MyIndex()
   {
      return View("Index");
   }
   [Route("Home/About")]
   public IActionResult MyAbout()
   {
      return View("About");
   }
   [Route("Home/Contact")]
   public IActionResult MyContact()
   {
      return View("Contact");
   }
}
```

> [!NOTE]
> <span data-ttu-id="88f8a-701">Powyższe szablony tras nie definiują parametrów trasy dla `action` , `area` i `controller` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-701">The route templates above don't define route parameters for `action`, `area`, and `controller`.</span></span> <span data-ttu-id="88f8a-702">W rzeczywistości te parametry tras są niedozwolone w trasach atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-702">In fact, these route parameters are not allowed in attribute routes.</span></span> <span data-ttu-id="88f8a-703">Ponieważ szablon trasy jest już skojarzony z akcją, nie ma sensu analizy nazwy akcji na podstawie adresu URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-703">Since the route template is already associated with an action, it wouldn't make sense to parse the action name from the URL.</span></span>

## <a name="attribute-routing-with-httpverb-attributes"></a><span data-ttu-id="88f8a-704">Routing atrybutów z atrybutami http [Verb]</span><span class="sxs-lookup"><span data-stu-id="88f8a-704">Attribute routing with Http[Verb] attributes</span></span>

<span data-ttu-id="88f8a-705">Routing atrybutu może również używać `Http[Verb]` atrybutów, takich jak `HttpPostAttribute` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-705">Attribute routing can also make use of the `Http[Verb]` attributes such as `HttpPostAttribute`.</span></span> <span data-ttu-id="88f8a-706">Wszystkie te atrybuty mogą akceptować szablon trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-706">All of these attributes can accept a route template.</span></span> <span data-ttu-id="88f8a-707">Ten przykład przedstawia dwie akcje, które pasują do tego samego szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="88f8a-707">This example shows two actions that match the same route template:</span></span>

```csharp
[HttpGet("/products")]
public IActionResult ListProducts()
{
   // ...
}

[HttpPost("/products")]
public IActionResult CreateProduct(...)
{
   // ...
}
```

<span data-ttu-id="88f8a-708">W przypadku ścieżki URL podobnej do `/products` `ProductsApi.ListProducts` akcji zostanie wykonane, gdy zlecenie http jest `GET` i `ProductsApi.CreateProduct` zostanie wykonane, gdy zlecenie http ma wartość `POST` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-708">For a URL path like `/products` the `ProductsApi.ListProducts` action will be executed when the HTTP verb is `GET` and `ProductsApi.CreateProduct` will be executed when the HTTP verb is `POST`.</span></span> <span data-ttu-id="88f8a-709">Routing atrybutu najpierw jest zgodny z adresem URL względem zestawu szablonów tras zdefiniowanych przez atrybuty trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-709">Attribute routing first matches the URL against the set of route templates defined by route attributes.</span></span> <span data-ttu-id="88f8a-710">Po dopasowaniu szablonu trasy `IActionConstraint` ograniczenia są stosowane w celu określenia, które akcje mogą być wykonywane.</span><span class="sxs-lookup"><span data-stu-id="88f8a-710">Once a route template matches, `IActionConstraint` constraints are applied to determine which actions can be executed.</span></span>

> [!TIP]
> <span data-ttu-id="88f8a-711">Podczas kompilowania interfejsu API REST jest to rzadki, że będzie on używany `[Route(...)]` na potrzeby metody akcji, ponieważ akcja akceptuje wszystkie metody http.</span><span class="sxs-lookup"><span data-stu-id="88f8a-711">When building a REST API, it's rare that you will want to use `[Route(...)]` on an action method as the action will accept all HTTP methods.</span></span> <span data-ttu-id="88f8a-712">Lepiej jest użyć bardziej szczegółowych `Http*Verb*Attributes` informacji na temat tego, co obsługuje interfejs API.</span><span class="sxs-lookup"><span data-stu-id="88f8a-712">It's better to use the more specific `Http*Verb*Attributes` to be precise about what your API supports.</span></span> <span data-ttu-id="88f8a-713">Klienci interfejsów API REST powinni wiedzieć, jakie ścieżki i czasowniki HTTP mapują na określone operacje logiczne.</span><span class="sxs-lookup"><span data-stu-id="88f8a-713">Clients of REST APIs are expected to know what paths and HTTP verbs map to specific logical operations.</span></span>

<span data-ttu-id="88f8a-714">Ponieważ atrybut Route ma zastosowanie do określonej akcji, można łatwo wprowadzić parametry wymagane jako część definicji szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-714">Since an attribute route applies to a specific action, it's easy to make parameters required as part of the route template definition.</span></span> <span data-ttu-id="88f8a-715">W tym przykładzie `id` jest wymagane jako część ścieżki URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-715">In this example, `id` is required as part of the URL path.</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="88f8a-716">`ProductsApi.GetProduct(int)`Akcja zostanie wykonana dla ścieżki URL, na przykład, `/products/3` ale nie dla ścieżki adresu URL `/products` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-716">The `ProductsApi.GetProduct(int)` action will be executed for a URL path like `/products/3` but not for a URL path like `/products`.</span></span> <span data-ttu-id="88f8a-717">Aby uzyskać pełny opis szablonów tras i powiązanych opcji, zobacz [Routing](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-717">See [Routing](xref:fundamentals/routing) for a full description of route templates and related options.</span></span>

## <a name="route-name"></a><span data-ttu-id="88f8a-718">Nazwa trasy</span><span class="sxs-lookup"><span data-stu-id="88f8a-718">Route Name</span></span>

<span data-ttu-id="88f8a-719">Następujący kod definiuje *nazwę trasy* `Products_List` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-719">The following code  defines a *route name* of `Products_List`:</span></span>

```csharp
public class ProductsApiController : Controller
{
   [HttpGet("/products/{id}", Name = "Products_List")]
   public IActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="88f8a-720">Nazwy tras mogą służyć do generowania adresów URL na podstawie określonej trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-720">Route names can be used to generate a URL based on a specific route.</span></span> <span data-ttu-id="88f8a-721">Nazwy tras nie mają wpływu na zachowanie routingu w adresie URL i są używane tylko na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-721">Route names have no impact on the URL matching behavior of routing and are only used for URL generation.</span></span> <span data-ttu-id="88f8a-722">Nazwy tras muszą być unikatowe w całej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-722">Route names must be unique application-wide.</span></span>

> [!NOTE]
> <span data-ttu-id="88f8a-723">Jest to zróżnicowane dla konwencjonalnej *trasy domyślnej*, która definiuje `id` parametr jako opcjonalny ( `{id?}` ).</span><span class="sxs-lookup"><span data-stu-id="88f8a-723">Contrast this with the conventional *default route*, which defines the `id` parameter as optional (`{id?}`).</span></span> <span data-ttu-id="88f8a-724">Ta możliwość precyzyjnego określania interfejsów API ma zalety, takich jak umożliwienie `/products` i `/products/5` wysyłanie ich do różnych akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-724">This ability to precisely specify APIs has advantages, such as  allowing `/products` and `/products/5` to be dispatched to different actions.</span></span>

<a name="routing-combining-ref-label"></a>

### <a name="combining-routes"></a><span data-ttu-id="88f8a-725">Łączenie tras</span><span class="sxs-lookup"><span data-stu-id="88f8a-725">Combining routes</span></span>

<span data-ttu-id="88f8a-726">Aby mniej powtarzać Routing atrybutów, atrybuty trasy na kontrolerze są łączone z atrybutami trasy dla poszczególnych akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-726">To make attribute routing less repetitive, route attributes on the controller are combined with route attributes on the individual actions.</span></span> <span data-ttu-id="88f8a-727">Wszystkie szablony tras zdefiniowane na kontrolerze są poprzedzone w celu rozesłania szablonów w akcjach.</span><span class="sxs-lookup"><span data-stu-id="88f8a-727">Any route templates defined on the controller are prepended to route templates on the actions.</span></span> <span data-ttu-id="88f8a-728">Umieszczenie atrybutu trasy na kontrolerze powoduje, że **wszystkie** akcje w kontrolerze używają routingu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-728">Placing a route attribute on the controller makes **all** actions in the controller use attribute routing.</span></span>

```csharp
[Route("products")]
public class ProductsApiController : Controller
{
   [HttpGet]
   public IActionResult ListProducts() { ... }

   [HttpGet("{id}")]
   public ActionResult GetProduct(int id) { ... }
}
```

<span data-ttu-id="88f8a-729">W tym przykładzie ścieżka adresu URL `/products` może być taka sama `ProductsApi.ListProducts` , a Ścieżka adresu URL `/products/5` może być taka sama `ProductsApi.GetProduct(int)` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-729">In this example the URL path `/products` can match `ProductsApi.ListProducts`, and the URL path `/products/5` can match `ProductsApi.GetProduct(int)`.</span></span> <span data-ttu-id="88f8a-730">Obie te akcje pasują tylko do protokołu HTTP `GET` , ponieważ są oznaczone za pomocą `HttpGetAttribute` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-730">Both of these actions only match HTTP `GET` because they're marked with the `HttpGetAttribute`.</span></span>

<span data-ttu-id="88f8a-731">Szablony tras zastosowane do akcji rozpoczynającej się od `/` lub `~/` nie są połączone z szablonami tras zastosowanymi do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="88f8a-731">Route templates applied to an action that begin with `/` or `~/` don't get combined with route templates applied to the controller.</span></span> <span data-ttu-id="88f8a-732">Ten przykład dopasowuje zestaw ścieżek URL podobny do *trasy domyślnej*.</span><span class="sxs-lookup"><span data-stu-id="88f8a-732">This example matches a set of URL paths similar to the *default route*.</span></span>

```csharp
[Route("Home")]
public class HomeController : Controller
{
    [Route("")]      // Combines to define the route template "Home"
    [Route("Index")] // Combines to define the route template "Home/Index"
    [Route("/")]     // Doesn't combine, defines the route template ""
    public IActionResult Index()
    {
        ViewData["Message"] = "Home index";
        var url = Url.Action("Index", "Home");
        ViewData["Message"] = "Home index" + "var url = Url.Action; =  " + url;
        return View();
    }

    [Route("About")] // Combines to define the route template "Home/About"
    public IActionResult About()
    {
        return View();
    }   
}
```

<a name="routing-ordering-ref-label"></a>

### <a name="ordering-attribute-routes"></a><span data-ttu-id="88f8a-733">Określanie kolejności tras atrybutów</span><span class="sxs-lookup"><span data-stu-id="88f8a-733">Ordering attribute routes</span></span>

<span data-ttu-id="88f8a-734">W przeciwieństwie do konwencjonalnych tras, które są wykonywane w określonej kolejności, routing atrybutu kompiluje drzewo i dopasowuje wszystkie trasy jednocześnie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-734">In contrast to conventional routes, which execute in a defined order, attribute routing builds a tree and matches all routes simultaneously.</span></span> <span data-ttu-id="88f8a-735">Zachowuje się tak, jakby wpisy trasy zostały umieszczone w idealnym porządku; najbardziej konkretne trasy mają możliwość wykonania przed bardziej ogólnymi trasami.</span><span class="sxs-lookup"><span data-stu-id="88f8a-735">This behaves as-if the route entries were placed in an ideal ordering; the most specific routes have a chance to execute before the more general routes.</span></span>

<span data-ttu-id="88f8a-736">Na przykład taka trasa `blog/search/{topic}` jest bardziej szczegółowa niż trasa, taka jak `blog/{*article}` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-736">For example, a route like `blog/search/{topic}` is more specific than a route like `blog/{*article}`.</span></span> <span data-ttu-id="88f8a-737">Logicznie mówiąc, `blog/search/{topic}` że trasy "uruchomienia" są najpierw domyślnie, ponieważ to jest jedyną rozsądną kolejnością.</span><span class="sxs-lookup"><span data-stu-id="88f8a-737">Logically speaking the `blog/search/{topic}` route 'runs' first, by default, because that's the only sensible ordering.</span></span> <span data-ttu-id="88f8a-738">Przy użyciu konwencjonalnego routingu deweloper jest odpowiedzialny za umieszczanie tras w odpowiedniej kolejności.</span><span class="sxs-lookup"><span data-stu-id="88f8a-738">Using conventional routing, the developer is  responsible for placing routes in the desired order.</span></span>

<span data-ttu-id="88f8a-739">Trasy atrybutów mogą konfigurować kolejność przy użyciu `Order` właściwości wszystkich atrybutów tras dostarczonych przez platformę.</span><span class="sxs-lookup"><span data-stu-id="88f8a-739">Attribute routes can configure an order, using the `Order` property of all of the framework provided route attributes.</span></span> <span data-ttu-id="88f8a-740">Trasy są przetwarzane zgodnie z rosnącą sortowaniem `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="88f8a-740">Routes are processed according to an ascending sort of the `Order` property.</span></span> <span data-ttu-id="88f8a-741">Kolejność domyślna to `0` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-741">The default order is `0`.</span></span> <span data-ttu-id="88f8a-742">Ustawienie trasy przy użyciu `Order = -1` zostanie uruchomione przed trasami, które nie ustawiają kolejności.</span><span class="sxs-lookup"><span data-stu-id="88f8a-742">Setting a route using `Order = -1` will run before routes that don't set an order.</span></span> <span data-ttu-id="88f8a-743">Ustawienie trasy przy użyciu `Order = 1` zostanie uruchomione po określeniu domyślnej kolejności tras.</span><span class="sxs-lookup"><span data-stu-id="88f8a-743">Setting a route using `Order = 1` will run after default route ordering.</span></span>

> [!TIP]
> <span data-ttu-id="88f8a-744">Należy unikać w zależności od `Order` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-744">Avoid depending on `Order`.</span></span> <span data-ttu-id="88f8a-745">Jeśli przestrzeń adresów URL wymaga jawnych wartości kolejności, aby można było prawidłowo kierować trasy, to prawdopodobnie również jest myląca dla klientów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-745">If your URL-space requires explicit order values to route correctly, then it's likely confusing to clients as well.</span></span> <span data-ttu-id="88f8a-746">W ogólnym routingu atrybutów wybierz prawidłową trasę z dopasowywaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-746">In general attribute routing will select the correct route with URL matching.</span></span> <span data-ttu-id="88f8a-747">Jeśli domyślna kolejność generowania adresów URL nie działa, użycie nazwy trasy jako przesłonięcia jest zwykle prostsze niż stosowanie `Order` właściwości.</span><span class="sxs-lookup"><span data-stu-id="88f8a-747">If the default order used for URL generation isn't working, using route name as an override is usually simpler than applying the `Order` property.</span></span>

<span data-ttu-id="88f8a-748">Razor Strony routingu i routingu kontrolera MVC współdzielą implementację.</span><span class="sxs-lookup"><span data-stu-id="88f8a-748">Razor Pages routing and MVC controller routing share an implementation.</span></span> <span data-ttu-id="88f8a-749">Informacje o zamówieniu trasy w Razor tematach stron są dostępne na [ Razor stronach trasy i konwencje aplikacji: kolejność trasy](xref:razor-pages/razor-pages-conventions#route-order).</span><span class="sxs-lookup"><span data-stu-id="88f8a-749">Information on route order in the Razor Pages topics is available at [Razor Pages route and app conventions: Route order](xref:razor-pages/razor-pages-conventions#route-order).</span></span>

<a name="routing-token-replacement-templates-ref-label"></a>

## <a name="token-replacement-in-route-templates-controller-action-area"></a><span data-ttu-id="88f8a-750">Zastępowanie tokenu w szablonach tras ([Controller], [Action], [obszar])</span><span class="sxs-lookup"><span data-stu-id="88f8a-750">Token replacement in route templates ([controller], [action], [area])</span></span>

<span data-ttu-id="88f8a-751">Dla wygody, trasy atrybutu obsługują *Zastępowanie tokenu* przez ujęcie tokenu w nawiasy kwadratowe ( `[` , `]` ).</span><span class="sxs-lookup"><span data-stu-id="88f8a-751">For convenience, attribute routes support *token replacement* by enclosing a token in square-brackets (`[`, `]`).</span></span> <span data-ttu-id="88f8a-752">Tokeny `[action]` , `[area]` i `[controller]` są zastępowane wartościami nazwy akcji, obszaru i nazwy kontrolera z akcji, w której jest zdefiniowana trasa.</span><span class="sxs-lookup"><span data-stu-id="88f8a-752">The tokens `[action]`, `[area]`, and `[controller]` are replaced with the values of the action name, area name, and controller name from the action where the route is defined.</span></span> <span data-ttu-id="88f8a-753">W poniższym przykładzie akcje są zgodne ze ścieżkami URL zgodnie z opisem w komentarzach:</span><span class="sxs-lookup"><span data-stu-id="88f8a-753">In the following example, the actions match URL paths as described in the comments:</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="88f8a-754">Zastępowanie tokenu występuje jako ostatni krok tworzenia tras atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-754">Token replacement occurs as the last step of building the attribute routes.</span></span> <span data-ttu-id="88f8a-755">Powyższy przykład będzie zachowywać się tak samo jak poniższy kod:</span><span class="sxs-lookup"><span data-stu-id="88f8a-755">The above example will behave the same as the following code:</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/ProductsController2.cs?range=7-11,13-17,20-22)]

<span data-ttu-id="88f8a-756">Trasy atrybutu można także łączyć z dziedziczeniem.</span><span class="sxs-lookup"><span data-stu-id="88f8a-756">Attribute routes can also be combined with inheritance.</span></span> <span data-ttu-id="88f8a-757">Jest to szczególnie zaawansowane w połączeniu z zastępowaniem tokenu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-757">This is particularly powerful combined with token replacement.</span></span>

```csharp
[Route("api/[controller]")]
public abstract class MyBaseController : Controller { ... }

public class ProductsController : MyBaseController
{
   [HttpGet] // Matches '/api/Products'
   public IActionResult List() { ... }

   [HttpPut("{id}")] // Matches '/api/Products/{id}'
   public IActionResult Edit(int id) { ... }
}
```

<span data-ttu-id="88f8a-758">Zastępowanie tokenu dotyczy również nazw tras zdefiniowanych przez trasy atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-758">Token replacement also applies to route names defined by attribute routes.</span></span> <span data-ttu-id="88f8a-759">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generuje unikatową nazwę trasy dla każdej akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-759">`[Route("[controller]/[action]", Name="[controller]_[action]")]` generates a unique route name for each action.</span></span>

<span data-ttu-id="88f8a-760">Aby dopasować ogranicznik zamiany tokenu literału `[` lub  `]` , należy to zrobić, powtarzając znak ( `[[` lub `]]` ).</span><span class="sxs-lookup"><span data-stu-id="88f8a-760">To match the literal token replacement delimiter `[` or  `]`, escape it by repeating the character (`[[` or `]]`).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<a name="routing-token-replacement-transformers-ref-label"></a>

### <a name="use-a-parameter-transformer-to-customize-token-replacement"></a><span data-ttu-id="88f8a-761">Używanie transformatora parametrów do dostosowywania zastępowania tokenu</span><span class="sxs-lookup"><span data-stu-id="88f8a-761">Use a parameter transformer to customize token replacement</span></span>

<span data-ttu-id="88f8a-762">Zastępowanie tokenu można dostosować za pomocą transformatora parametrów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-762">Token replacement can be customized using a parameter transformer.</span></span> <span data-ttu-id="88f8a-763">Transformator parametrów implementuje `IOutboundParameterTransformer` i przekształca wartość parametrów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-763">A parameter transformer implements `IOutboundParameterTransformer` and transforms the value of parameters.</span></span> <span data-ttu-id="88f8a-764">Na przykład niestandardowy `SlugifyParameterTransformer` transformator parametrów zmienia `SubscriptionManagement` wartość trasy na `subscription-management` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-764">For example, a custom `SlugifyParameterTransformer` parameter transformer changes the `SubscriptionManagement` route value to `subscription-management`.</span></span>

<span data-ttu-id="88f8a-765">`RouteTokenTransformerConvention`Jest konwencją modelu aplikacji, która:</span><span class="sxs-lookup"><span data-stu-id="88f8a-765">The `RouteTokenTransformerConvention` is an application model convention that:</span></span>

* <span data-ttu-id="88f8a-766">Stosuje transformator parametrów do wszystkich tras atrybutów w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-766">Applies a parameter transformer to all attribute routes in an application.</span></span>
* <span data-ttu-id="88f8a-767">Dostosowuje wartości tokenów tras w miarę ich wymiany.</span><span class="sxs-lookup"><span data-stu-id="88f8a-767">Customizes the attribute route token values as they are replaced.</span></span>

```csharp
public class SubscriptionManagementController : Controller
{
    [HttpGet("[controller]/[action]")] // Matches '/subscription-management/list-all'
    public IActionResult ListAll() { ... }
}
```

<span data-ttu-id="88f8a-768">`RouteTokenTransformerConvention`Jest zarejestrowany jako opcja w `ConfigureServices` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-768">The `RouteTokenTransformerConvention` is registered as an option in `ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc(options =>
    {
        options.Conventions.Add(new RouteTokenTransformerConvention(
                                     new SlugifyParameterTransformer()));
    });
}

public class SlugifyParameterTransformer : IOutboundParameterTransformer
{
    public string TransformOutbound(object value)
    {
        if (value == null) { return null; }

        // Slugify value
        return Regex.Replace(value.ToString(), "([a-z])([A-Z])", "$1-$2").ToLower();
    }
}
```

::: moniker-end


::: moniker range="< aspnetcore-3.0"
<a name="routing-multiple-routes-ref-label"></a>

### <a name="multiple-routes"></a><span data-ttu-id="88f8a-769">Wiele tras</span><span class="sxs-lookup"><span data-stu-id="88f8a-769">Multiple Routes</span></span>

<span data-ttu-id="88f8a-770">Routing atrybutów obsługuje definiowanie wielu tras, które docierają do tej samej akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-770">Attribute routing supports defining multiple routes that reach the same action.</span></span> <span data-ttu-id="88f8a-771">Najczęstszym sposobem użycia tej metody jest naśladowanie zachowania *domyślnej trasy konwencjonalnej* , jak pokazano w następującym przykładzie:</span><span class="sxs-lookup"><span data-stu-id="88f8a-771">The most common usage of this is to mimic the behavior of the *default conventional route* as shown in the following example:</span></span>

```csharp
[Route("[controller]")]
public class ProductsController : Controller
{
   [Route("")]     // Matches 'Products'
   [Route("Index")] // Matches 'Products/Index'
   public IActionResult Index()
}
```

<span data-ttu-id="88f8a-772">Umieszczenie wielu atrybutów trasy na kontrolerze oznacza, że każda z nich będzie łączyć się z poszczególnymi atrybutami trasy w metodach akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-772">Putting multiple route attributes on the controller means that each one will combine with each of the route attributes on the action methods.</span></span>

```csharp
[Route("Store")]
[Route("[controller]")]
public class ProductsController : Controller
{
   [HttpPost("Buy")]     // Matches 'Products/Buy' and 'Store/Buy'
   [HttpPost("Checkout")] // Matches 'Products/Checkout' and 'Store/Checkout'
   public IActionResult Buy()
}
```

<span data-ttu-id="88f8a-773">Gdy wiele atrybutów trasy (które Implementuj `IActionConstraint` ) jest umieszczanych w akcji, każde ograniczenie akcji łączy się z szablonem trasy z atrybutu, który go zdefiniował.</span><span class="sxs-lookup"><span data-stu-id="88f8a-773">When multiple route attributes (that implement `IActionConstraint`) are placed on an action, then each action constraint combines with the route template from the attribute that defined it.</span></span>

```csharp
[Route("api/[controller]")]
public class ProductsController : Controller
{
   [HttpPut("Buy")]      // Matches PUT 'api/Products/Buy'
   [HttpPost("Checkout")] // Matches POST 'api/Products/Checkout'
   public IActionResult Buy()
}
```

> [!TIP]
> <span data-ttu-id="88f8a-774">Chociaż używanie wielu tras w akcjach może wydawać się zaawansowane, lepiej jest zachować prosty i dobrze zdefiniowany obszar adresów URL aplikacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-774">While using multiple routes on actions can seem powerful, it's better to keep your application's URL space simple and well-defined.</span></span> <span data-ttu-id="88f8a-775">Użyj wielu tras w przypadku akcji tylko wtedy, gdy jest to konieczne, na przykład do obsługi istniejących klientów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-775">Use multiple routes on actions only where needed, for example to support existing clients.</span></span>

<a name="routing-attr-options"></a>

### <a name="specifying-attribute-route-optional-parameters-default-values-and-constraints"></a><span data-ttu-id="88f8a-776">Określanie opcjonalnych parametrów trasy, wartości domyślnych i ograniczeń</span><span class="sxs-lookup"><span data-stu-id="88f8a-776">Specifying attribute route optional parameters, default values, and constraints</span></span>

<span data-ttu-id="88f8a-777">Trasy atrybutów obsługują tę samą składnię wbudowaną co konwencjonalne trasy, aby określić parametry opcjonalne, wartości domyślne i ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="88f8a-777">Attribute routes support the same inline syntax as conventional routes to specify optional parameters, default values, and constraints.</span></span>

```csharp
[HttpPost("product/{id:int}")]
public IActionResult ShowProduct(int id)
{
   // ...
}
```

<span data-ttu-id="88f8a-778">Aby uzyskać szczegółowy opis składni szablonu trasy, zobacz [odwołanie do szablonu trasy](xref:fundamentals/routing#route-template-reference) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-778">See [Route Template Reference](xref:fundamentals/routing#route-template-reference) for a detailed description of route template syntax.</span></span>

<a name="routing-cust-rt-attr-irt-ref-label"></a>

### <a name="custom-route-attributes-using-iroutetemplateprovider"></a><span data-ttu-id="88f8a-779">Niestandardowe atrybuty trasy przy użyciu `IRouteTemplateProvider`</span><span class="sxs-lookup"><span data-stu-id="88f8a-779">Custom route attributes using `IRouteTemplateProvider`</span></span>

<span data-ttu-id="88f8a-780">Wszystkie atrybuty trasy podane w strukturze ( `[Route(...)]` , `[HttpGet(...)]` itp.) implementują `IRouteTemplateProvider` interfejs.</span><span class="sxs-lookup"><span data-stu-id="88f8a-780">All of the route attributes provided in the framework ( `[Route(...)]`, `[HttpGet(...)]` , etc.) implement the `IRouteTemplateProvider` interface.</span></span> <span data-ttu-id="88f8a-781">MVC wyszukuje atrybuty klas kontrolera i metod akcji, gdy aplikacja jest uruchamiana i używa tych, które implementują w `IRouteTemplateProvider` celu utworzenia początkowego zestawu tras.</span><span class="sxs-lookup"><span data-stu-id="88f8a-781">MVC looks for attributes on controller classes and action methods when the app starts and uses the ones that implement `IRouteTemplateProvider` to build the initial set of routes.</span></span>

<span data-ttu-id="88f8a-782">Można zaimplementować, `IRouteTemplateProvider` Aby zdefiniować własne atrybuty trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-782">You can implement `IRouteTemplateProvider` to define your own route attributes.</span></span> <span data-ttu-id="88f8a-783">Każdy `IRouteTemplateProvider` z nich umożliwia definiowanie pojedynczej trasy z niestandardowym szablonem trasy, kolejnością i nazwą:</span><span class="sxs-lookup"><span data-stu-id="88f8a-783">Each `IRouteTemplateProvider` allows you to define a single route with a custom route template, order, and name:</span></span>

```csharp
public class MyApiControllerAttribute : Attribute, IRouteTemplateProvider
{
   public string Template => "api/[controller]";

   public int? Order { get; set; }

   public string Name { get; set; }
}
```

<span data-ttu-id="88f8a-784">Atrybut z powyższego przykładu automatycznie ustawia wartość `Template` na `"api/[controller]"` gdy `[MyApiController]` jest stosowany.</span><span class="sxs-lookup"><span data-stu-id="88f8a-784">The attribute from the above example automatically sets the `Template` to `"api/[controller]"` when `[MyApiController]` is applied.</span></span>

<a name="routing-app-model-ref-label"></a>

### <a name="using-application-model-to-customize-attribute-routes"></a><span data-ttu-id="88f8a-785">Dostosowywanie tras atrybutów przy użyciu modelu aplikacji</span><span class="sxs-lookup"><span data-stu-id="88f8a-785">Using Application Model to customize attribute routes</span></span>

<span data-ttu-id="88f8a-786">*Model aplikacji* jest modelem obiektów tworzonym podczas uruchamiania ze wszystkimi metadanymi używanymi przez MVC do kierowania i wykonywania akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-786">The *application model* is an object model created at startup with all of the metadata used by MVC to route and execute your actions.</span></span> <span data-ttu-id="88f8a-787">*Model aplikacji* zawiera wszystkie dane zebrane z atrybutów trasy (do `IRouteTemplateProvider` ).</span><span class="sxs-lookup"><span data-stu-id="88f8a-787">The *application model* includes all of the data gathered from route attributes (through `IRouteTemplateProvider`).</span></span> <span data-ttu-id="88f8a-788">Można napisać *konwencje* , aby zmodyfikować model aplikacji w czasie uruchamiania, aby dostosować sposób zachowania routingu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-788">You can write *conventions* to modify the application model at startup time to customize how routing behaves.</span></span> <span data-ttu-id="88f8a-789">W tej sekcji przedstawiono prosty przykład dostosowywania routingu przy użyciu modelu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-789">This section shows a simple example of customizing routing using application model.</span></span>

[!code-csharp[](routing/samples/2.x/main/NamespaceRoutingConvention.cs)]

<a name="routing-mixed-ref-label"></a>

## <a name="mixed-routing-attribute-routing-vs-conventional-routing"></a><span data-ttu-id="88f8a-790">Routing mieszany: Routing atrybutów a konwencjonalny Routing</span><span class="sxs-lookup"><span data-stu-id="88f8a-790">Mixed routing: Attribute routing vs conventional routing</span></span>

<span data-ttu-id="88f8a-791">Aplikacje MVC mogą łączyć użycie konwencjonalnego routingu i routingu atrybutów.</span><span class="sxs-lookup"><span data-stu-id="88f8a-791">MVC applications can mix the use of conventional routing and attribute routing.</span></span> <span data-ttu-id="88f8a-792">Typowym zastosowaniem są trasy konwencjonalne dla kontrolerów obsługujących strony HTML dla przeglądarek i routingu atrybutów dla kontrolerów obsługujących interfejsy API REST.</span><span class="sxs-lookup"><span data-stu-id="88f8a-792">It's typical to use conventional routes for controllers serving HTML pages for browsers, and attribute routing for controllers serving REST APIs.</span></span>

<span data-ttu-id="88f8a-793">Akcje są albo podkierowane do Konwencji lub kierowane przez atrybut.</span><span class="sxs-lookup"><span data-stu-id="88f8a-793">Actions are either conventionally routed or attribute routed.</span></span> <span data-ttu-id="88f8a-794">Umieszczenie trasy na kontrolerze lub akcja powoduje, że atrybut jest kierowany.</span><span class="sxs-lookup"><span data-stu-id="88f8a-794">Placing a route on the controller or the action makes it attribute routed.</span></span> <span data-ttu-id="88f8a-795">Akcje definiujące trasy atrybutów nie mogą być osiągane za pomocą konwencjonalnych tras i vice versa.</span><span class="sxs-lookup"><span data-stu-id="88f8a-795">Actions that define attribute routes cannot be reached through the conventional routes and vice-versa.</span></span> <span data-ttu-id="88f8a-796">**Każdy** atrybut trasy na kontrolerze powoduje kierowanie wszystkich akcji w atrybucie kontrolera.</span><span class="sxs-lookup"><span data-stu-id="88f8a-796">**Any** route attribute on the controller makes all actions in the controller attribute routed.</span></span>

> [!NOTE]
> <span data-ttu-id="88f8a-797">Co odróżnia dwa typy systemów routingu, proces stosowany po adresie URL pasuje do szablonu trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-797">What distinguishes the two types of routing systems is the process applied after a URL matches a route template.</span></span> <span data-ttu-id="88f8a-798">W przypadku routingu konwencjonalnego wartości trasy z dopasowania są używane do wybierania akcji i kontrolera z tabeli odnośników wszystkich konwencjonalnych akcji kierowanych.</span><span class="sxs-lookup"><span data-stu-id="88f8a-798">In conventional routing, the route values from the match are used to choose the action and controller from a lookup table of all conventional routed actions.</span></span> <span data-ttu-id="88f8a-799">W obszarze Routing atrybutów każdy szablon jest już skojarzony z akcją i nie jest wymagany dalsze wyszukiwanie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-799">In attribute routing, each template is already associated with an action, and no further lookup is needed.</span></span>

## <a name="complex-segments"></a><span data-ttu-id="88f8a-800">Złożone segmenty</span><span class="sxs-lookup"><span data-stu-id="88f8a-800">Complex segments</span></span>

<span data-ttu-id="88f8a-801">Złożone segmenty (na przykład `[Route("/dog{token}cat")]` ) są przetwarzane przez dopasowanie literałów z prawej strony do lewej w sposób niezachłanney.</span><span class="sxs-lookup"><span data-stu-id="88f8a-801">Complex segments (for example, `[Route("/dog{token}cat")]`), are processed by matching up literals from right to left in a non-greedy way.</span></span> <span data-ttu-id="88f8a-802">Sprawdź [kod źródłowy](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) opisu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-802">See [the source code](https://github.com/aspnet/Routing/blob/9cea167cfac36cf034dbb780e3f783114ef94780/src/Microsoft.AspNetCore.Routing/Patterns/RoutePatternMatcher.cs#L296) for a description.</span></span> <span data-ttu-id="88f8a-803">Aby uzyskać więcej informacji, zobacz [ten problem](https://github.com/dotnet/AspNetCore.Docs/issues/8197).</span><span class="sxs-lookup"><span data-stu-id="88f8a-803">For more information, see [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/8197).</span></span>

<a name="routing-url-gen-ref-label"></a>

## <a name="url-generation"></a><span data-ttu-id="88f8a-804">Generowanie adresu URL</span><span class="sxs-lookup"><span data-stu-id="88f8a-804">URL Generation</span></span>

<span data-ttu-id="88f8a-805">Aplikacje MVC mogą używać funkcji generowania adresów URL routingu do generowania linków URL do akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-805">MVC applications can use routing's URL generation features to generate URL links to actions.</span></span> <span data-ttu-id="88f8a-806">Generowanie adresów URL eliminuje adresy URL zakodowana, co sprawia, że kod jest bardziej niezawodny i konserwowany.</span><span class="sxs-lookup"><span data-stu-id="88f8a-806">Generating URLs eliminates hardcoding URLs, making your code more robust and maintainable.</span></span> <span data-ttu-id="88f8a-807">Ta sekcja koncentruje się na funkcjach generowania adresów URL dostarczonych przez MVC i obejmuje tylko podstawowe informacje na temat sposobu działania generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-807">This section focuses on the URL generation features provided by MVC and will only cover basics of how URL generation works.</span></span> <span data-ttu-id="88f8a-808">Aby uzyskać szczegółowy opis generowania adresów URL, zobacz temat [Routing](xref:fundamentals/routing) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-808">See [Routing](xref:fundamentals/routing) for a detailed description of URL generation.</span></span>

<span data-ttu-id="88f8a-809">`IUrlHelper`Interfejs jest podstawową częścią infrastruktury między MVC i routingiem na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-809">The `IUrlHelper` interface is the underlying piece of infrastructure between MVC and routing for URL generation.</span></span> <span data-ttu-id="88f8a-810">Wystąpienie `IUrlHelper` dostępne za pomocą właściwości można znaleźć `Url` w obszarze Kontrolery, widoki i składniki widoku.</span><span class="sxs-lookup"><span data-stu-id="88f8a-810">You'll find an instance of `IUrlHelper` available through the `Url` property in controllers, views, and view components.</span></span>

<span data-ttu-id="88f8a-811">W tym przykładzie `IUrlHelper` interfejs jest używany przez `Controller.Url` Właściwość w celu WYGENEROWANIA adresu URL dla innej akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-811">In this example, the `IUrlHelper` interface is used through the `Controller.Url` property to generate a URL to another action.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationController.cs?name=snippet_1)]

<span data-ttu-id="88f8a-812">Jeśli aplikacja używa domyślnej trasy konwencjonalnej, wartość `url` zmiennej będzie ciąg ścieżki URL `/UrlGeneration/Destination` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-812">If the application is using the default conventional route, the value of the `url` variable will be the URL path string `/UrlGeneration/Destination`.</span></span> <span data-ttu-id="88f8a-813">Ta ścieżka URL jest tworzona przez Routing przez połączenie wartości trasy z bieżącego żądania (wartości otoczenia), z wartościami przekazanymi do `Url.Action` i podstawianie tych wartości do szablonu trasy:</span><span class="sxs-lookup"><span data-stu-id="88f8a-813">This URL path is created by routing by combining the route values from the current request (ambient values), with the values passed to `Url.Action` and substituting those values into the route template:</span></span>

```
ambient values: { controller = "UrlGeneration", action = "Source" }
values passed to Url.Action: { controller = "UrlGeneration", action = "Destination" }
route template: {controller}/{action}/{id?}

result: /UrlGeneration/Destination
```

<span data-ttu-id="88f8a-814">Każdy parametr trasy w szablonie trasy ma swoją wartość zastępowaną przez pasujące nazwy wartościami i wartościami otoczenia.</span><span class="sxs-lookup"><span data-stu-id="88f8a-814">Each route parameter in the route template has its value substituted by matching names with the values and ambient values.</span></span> <span data-ttu-id="88f8a-815">Parametr trasy, który nie ma wartości, może korzystać z wartości domyślnej, jeśli ma taką wartość, lub być pominięty, jeśli jest opcjonalny (tak jak w przypadku `id` w przypadku w tym przykładzie).</span><span class="sxs-lookup"><span data-stu-id="88f8a-815">A route parameter that doesn't have a value can use a default value if it has one, or be skipped if it's optional (as in the case of `id` in this example).</span></span> <span data-ttu-id="88f8a-816">Generowanie adresu URL zakończy się niepowodzeniem, jeśli którykolwiek z wymaganych parametrów trasy nie ma odpowiadającej wartości.</span><span class="sxs-lookup"><span data-stu-id="88f8a-816">URL generation will fail if any required route parameter doesn't have a corresponding value.</span></span> <span data-ttu-id="88f8a-817">Jeśli generowanie adresów URL kończy się niepowodzeniem dla trasy, kolejna trasa zostanie ponowiona do momentu przetworzenia wszystkich tras lub znalezienia dopasowania.</span><span class="sxs-lookup"><span data-stu-id="88f8a-817">If URL generation fails for a route, the next route is tried until all routes have been tried or a match is found.</span></span>

<span data-ttu-id="88f8a-818">W `Url.Action` powyższym przykładzie przyjęto założenie konwencjonalnego routingu, ale generowanie adresów URL działa podobnie jak w przypadku routingu atrybutów, chociaż koncepcje różnią się.</span><span class="sxs-lookup"><span data-stu-id="88f8a-818">The example of `Url.Action` above assumes conventional routing, but URL generation works similarly with attribute routing, though the concepts are different.</span></span> <span data-ttu-id="88f8a-819">W przypadku routingu konwencjonalnego wartości trasy służą do rozszerzania szablonu, a wartości trasy dla `controller` i `action` zwykle pojawiają się w tym szablonie — działa to, ponieważ adresy URL dopasowane przez Routing są zgodne z *Konwencją*.</span><span class="sxs-lookup"><span data-stu-id="88f8a-819">With conventional routing, the route values are used to expand a template, and the route values for `controller` and `action` usually appear in that template - this works because the URLs matched by routing adhere to a *convention*.</span></span> <span data-ttu-id="88f8a-820">W obszarze Routing atrybutów wartości trasy dla `controller` i `action` nie mogą występować w szablonie — zamiast tego są używane do wyszukiwania szablonu do użycia.</span><span class="sxs-lookup"><span data-stu-id="88f8a-820">In attribute routing, the route values for `controller` and `action` are not allowed to appear in the template - they're instead used to look up which template to use.</span></span>

<span data-ttu-id="88f8a-821">W tym przykładzie zastosowano Routing atrybutów:</span><span class="sxs-lookup"><span data-stu-id="88f8a-821">This example uses attribute routing:</span></span>

[!code-csharp[](routing/samples/2.x/main/StartupUseMvc.cs?name=snippet_1)]

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerAttr.cs?name=snippet_1)]

<span data-ttu-id="88f8a-822">MVC kompiluje tabelę odnośników wszystkich akcji przypisanych do atrybutu i odpowiada `controller` wartości i, `action` Aby wybrać szablon trasy do użycia na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-822">MVC builds a lookup table of all attribute routed actions and will match the `controller` and `action` values to select the route template to use for URL generation.</span></span> <span data-ttu-id="88f8a-823">W powyższym przykładzie   `custom/url/to/destination` jest generowany.</span><span class="sxs-lookup"><span data-stu-id="88f8a-823">In the sample above,   `custom/url/to/destination` is generated.</span></span>

### <a name="generating-urls-by-action-name"></a><span data-ttu-id="88f8a-824">Generowanie adresów URL według nazwy akcji</span><span class="sxs-lookup"><span data-stu-id="88f8a-824">Generating URLs by action name</span></span>

<span data-ttu-id="88f8a-825">`Url.Action` (`IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-825">`Url.Action` (`IUrlHelper` .</span></span> <span data-ttu-id="88f8a-826">`Action`) i wszystkie powiązane przeciążenia są oparte na tym pomysłie, że chcesz określić, do czego łączysz, określając nazwę kontrolera i nazwę akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-826">`Action`) and all related overloads all are based on that idea that you want to specify what you're linking to by specifying a controller name and action name.</span></span>

> [!NOTE]
> <span data-ttu-id="88f8a-827">W przypadku używania `Url.Action` , bieżąca wartość trasy dla `controller` i `action` jest określona dla Ciebie — wartości `controller` i `action` są częścią obu *wartości otoczenia* **i** *wartości*.</span><span class="sxs-lookup"><span data-stu-id="88f8a-827">When using `Url.Action`, the current route values for `controller` and `action` are specified for you - the value of `controller` and `action` are part of both *ambient values* **and** *values*.</span></span> <span data-ttu-id="88f8a-828">Metoda `Url.Action` zawsze używa bieżących wartości `action` i `controller` i wygeneruje ścieżkę URL, która jest przesyłana do bieżącej akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-828">The method `Url.Action`, always uses the current values of `action` and `controller` and will generate a URL path that routes to the current action.</span></span>

<span data-ttu-id="88f8a-829">Funkcja routingu próbuje użyć wartości w otoczeniu wartości, aby podać informacje, które nie zostały wprowadzone podczas generowania adresu URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-829">Routing attempts to use the values in ambient values to fill in information that you didn't provide when generating a URL.</span></span> <span data-ttu-id="88f8a-830">Przy użyciu trasy podobnej do `{a}/{b}/{c}/{d}` i otoczenia `{ a = Alice, b = Bob, c = Carol, d = David }` Routing ma wystarczającą ilość informacji do WYGENEROWANIA adresu URL bez żadnych dodatkowych wartości — ponieważ wszystkie parametry trasy mają wartość.</span><span class="sxs-lookup"><span data-stu-id="88f8a-830">Using a route like `{a}/{b}/{c}/{d}` and ambient values `{ a = Alice, b = Bob, c = Carol, d = David }`, routing has enough information to generate a URL without any additional values - since all route parameters have a value.</span></span> <span data-ttu-id="88f8a-831">Po dodaniu wartości wartość zostanie `{ d = Donovan }` `{ d = David }` zignorowana, a wygenerowana Ścieżka adresu URL `Alice/Bob/Carol/Donovan` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-831">If you added the value `{ d = Donovan }`, the value `{ d = David }` would be ignored, and the generated URL path would be `Alice/Bob/Carol/Donovan`.</span></span>

> [!WARNING]
> <span data-ttu-id="88f8a-832">Ścieżki URL są hierarchiczne.</span><span class="sxs-lookup"><span data-stu-id="88f8a-832">URL paths are hierarchical.</span></span> <span data-ttu-id="88f8a-833">W powyższym przykładzie, jeśli dodano wartość `{ c = Cheryl }` , obie wartości `{ c = Carol, d = David }` byłyby ignorowane.</span><span class="sxs-lookup"><span data-stu-id="88f8a-833">In the example above, if you added the value `{ c = Cheryl }`, both of the values `{ c = Carol, d = David }` would be ignored.</span></span> <span data-ttu-id="88f8a-834">W takim przypadku nie jest już dostępna wartość `d` i generowanie adresu URL zakończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="88f8a-834">In this case we no longer have a value for `d` and URL generation will fail.</span></span> <span data-ttu-id="88f8a-835">Należy określić pożądaną wartość `c` i `d` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-835">You would need to specify the desired value of `c` and `d`.</span></span>  <span data-ttu-id="88f8a-836">Można oczekiwać, że ten problem zostanie osiągnięty przy użyciu trasy domyślnej ( `{controller}/{action}/{id?}` ) — ale w takiej sytuacji rzadko napotkasz to zachowanie, ponieważ `Url.Action` zawsze jawnie określimy `controller` `action` wartość i.</span><span class="sxs-lookup"><span data-stu-id="88f8a-836">You might expect to hit this problem with the default route (`{controller}/{action}/{id?}`) - but you will rarely encounter this behavior in practice as `Url.Action` will always explicitly specify a `controller` and `action` value.</span></span>

<span data-ttu-id="88f8a-837">Dłuższe przeciążenia `Url.Action` również pobierają dodatkowy obiekt *wartości trasy* , aby zapewnić wartości parametrów trasy innych niż `controller` i `action` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-837">Longer overloads of `Url.Action` also take an additional *route values* object to provide values for route parameters other than `controller` and `action`.</span></span> <span data-ttu-id="88f8a-838">Najczęściej zobaczysz, że będzie on używany z usługą `id` `Url.Action("Buy", "Products", new { id = 17 })` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-838">You will most commonly see this used with `id` like `Url.Action("Buy", "Products", new { id = 17 })`.</span></span> <span data-ttu-id="88f8a-839">Według Konwencji obiekt *wartości trasy* jest zwykle obiektem typu anonimowego, ale może również być `IDictionary<>` lub *zwykłym starym obiektem platformy .NET*.</span><span class="sxs-lookup"><span data-stu-id="88f8a-839">By convention the *route values* object is usually an object of anonymous type, but it can also be an `IDictionary<>` or a *plain old .NET object*.</span></span> <span data-ttu-id="88f8a-840">Wszystkie dodatkowe wartości trasy, które nie pasują do parametrów trasy, są umieszczane w ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="88f8a-840">Any additional route values that don't match route parameters are put in the query string.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/TestController.cs)]

> [!TIP]
> <span data-ttu-id="88f8a-841">Aby utworzyć bezwzględny adres URL, Użyj przeciążenia, które akceptuje `protocol` : `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span><span class="sxs-lookup"><span data-stu-id="88f8a-841">To create an absolute URL, use an overload that accepts a `protocol`: `Url.Action("Buy", "Products", new { id = 17 }, protocol: Request.Scheme)`</span></span>

<a name="routing-gen-urls-route-ref-label"></a>

### <a name="generating-urls-by-route"></a><span data-ttu-id="88f8a-842">Generowanie adresów URL według trasy</span><span class="sxs-lookup"><span data-stu-id="88f8a-842">Generating URLs by route</span></span>

<span data-ttu-id="88f8a-843">Powyższy kod wygeneruje adres URL przez przekazanie go do kontrolera i nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-843">The code above demonstrated generating a URL by passing in the controller and action name.</span></span> <span data-ttu-id="88f8a-844">`IUrlHelper` zapewnia także `Url.RouteUrl` rodzinę metod.</span><span class="sxs-lookup"><span data-stu-id="88f8a-844">`IUrlHelper` also provides the `Url.RouteUrl` family of methods.</span></span> <span data-ttu-id="88f8a-845">Te metody są podobne do `Url.Action` , ale nie kopiują bieżących wartości `action` i `controller` do wartości trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-845">These methods are similar to `Url.Action`, but they don't copy the current values of `action` and `controller` to the route values.</span></span> <span data-ttu-id="88f8a-846">Najbardziej typowym zastosowaniem jest określenie nazwy trasy do użycia określonej trasy do wygenerowania adresu URL, na ogół *bez* określania kontrolera lub nazwy akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-846">The most common usage is to specify a route name to use a specific route to generate the URL, generally *without* specifying a controller or action name.</span></span>

[!code-csharp[](routing/samples/2.x/main/Controllers/UrlGenerationControllerRouting.cs?name=snippet_1)]

<a name="routing-gen-urls-html-ref-label"></a>

### <a name="generating-urls-in-html"></a><span data-ttu-id="88f8a-847">Generowanie adresów URL w kodzie HTML</span><span class="sxs-lookup"><span data-stu-id="88f8a-847">Generating URLs in HTML</span></span>

<span data-ttu-id="88f8a-848">`IHtmlHelper` zapewnia `HtmlHelper` metody `Html.BeginForm` i `Html.ActionLink` wygenerowanie `<form>` `<a>` odpowiednio elementów i.</span><span class="sxs-lookup"><span data-stu-id="88f8a-848">`IHtmlHelper` provides the `HtmlHelper` methods `Html.BeginForm` and `Html.ActionLink` to generate `<form>` and `<a>` elements respectively.</span></span> <span data-ttu-id="88f8a-849">Metody te używają `Url.Action` metody do generowania adresu URL i akceptują podobne argumenty.</span><span class="sxs-lookup"><span data-stu-id="88f8a-849">These methods use the `Url.Action` method to generate a URL and they accept similar arguments.</span></span> <span data-ttu-id="88f8a-850">`Url.RouteUrl`Pomocników dla programu `HtmlHelper` mają `Html.BeginRouteForm` `Html.RouteLink` podobną funkcjonalność.</span><span class="sxs-lookup"><span data-stu-id="88f8a-850">The `Url.RouteUrl` companions for `HtmlHelper` are `Html.BeginRouteForm` and `Html.RouteLink` which have similar functionality.</span></span>

<span data-ttu-id="88f8a-851">TagHelpers Generuj adresy URL za pomocą `form` TagHelper i `<a>` TagHelper.</span><span class="sxs-lookup"><span data-stu-id="88f8a-851">TagHelpers generate URLs through the `form` TagHelper and the `<a>` TagHelper.</span></span> <span data-ttu-id="88f8a-852">Oba te zastosowania `IUrlHelper` dla ich implementacji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-852">Both of these use `IUrlHelper` for their implementation.</span></span> <span data-ttu-id="88f8a-853">Aby uzyskać więcej informacji, zobacz [Praca z formularzami](../views/working-with-forms.md) .</span><span class="sxs-lookup"><span data-stu-id="88f8a-853">See [Working with Forms](../views/working-with-forms.md) for more information.</span></span>

<span data-ttu-id="88f8a-854">W widokach `IUrlHelper` można korzystać z `Url` właściwości dla dowolnej generacji adresów URL ad hoc, która nie jest objęta powyższym.</span><span class="sxs-lookup"><span data-stu-id="88f8a-854">Inside views, the `IUrlHelper` is available through the `Url` property for any ad-hoc URL generation not covered by the above.</span></span>

<a name="routing-gen-urls-action-ref-label"></a>

### <a name="generating-urls-in-action-results"></a><span data-ttu-id="88f8a-855">Generowanie adresów URL w wynikach akcji</span><span class="sxs-lookup"><span data-stu-id="88f8a-855">Generating URLS in Action Results</span></span>

<span data-ttu-id="88f8a-856">Powyższe przykłady przedstawiono przy użyciu `IUrlHelper` w kontrolerze, podczas gdy najbardziej typowym użyciem na kontrolerze jest wygenerowanie adresu URL jako części wyniku akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-856">The examples above have shown using `IUrlHelper` in a controller, while the most common usage in a controller is to generate a URL as part of an action result.</span></span>

<span data-ttu-id="88f8a-857">`ControllerBase` `Controller` Klasy bazowe i zapewniają wygodne metody dla wyników akcji, które odwołują się do innej akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-857">The `ControllerBase` and `Controller` base classes provide convenience methods for action results that reference another action.</span></span> <span data-ttu-id="88f8a-858">Jednym z typowych zastosowań jest przekierowanie po zaakceptowaniu danych wejściowych użytkownika.</span><span class="sxs-lookup"><span data-stu-id="88f8a-858">One typical usage is to redirect after accepting user input.</span></span>

```csharp
public IActionResult Edit(int id, Customer customer)
{
    if (ModelState.IsValid)
    {
        // Update DB with new details.
        return RedirectToAction("Index");
    }
    return View(customer);
}
```

<span data-ttu-id="88f8a-859">Metody fabryki wyników akcji postępują zgodnie z podobnym wzorcem do metod w `IUrlHelper` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-859">The action results factory methods follow a similar pattern to the methods on `IUrlHelper`.</span></span>

<a name="routing-dedicated-ref-label"></a>

### <a name="special-case-for-dedicated-conventional-routes"></a><span data-ttu-id="88f8a-860">Specjalny przypadek dla dedykowanych tras konwencjonalnych</span><span class="sxs-lookup"><span data-stu-id="88f8a-860">Special case for dedicated conventional routes</span></span>

<span data-ttu-id="88f8a-861">Funkcja routingu konwencjonalnego może używać specjalnego rodzaju definicji trasy o nazwie *dedykowanej, konwencjonalnej trasy*.</span><span class="sxs-lookup"><span data-stu-id="88f8a-861">Conventional routing can use a special kind of route definition called a *dedicated conventional route*.</span></span> <span data-ttu-id="88f8a-862">W poniższym przykładzie trasa o nazwie `blog` jest dedykowaną umowną trasą.</span><span class="sxs-lookup"><span data-stu-id="88f8a-862">In the example below, the route named `blog` is a dedicated conventional route.</span></span>

```csharp
app.UseMvc(routes =>
{
    routes.MapRoute("blog", "blog/{*article}",
        defaults: new { controller = "Blog", action = "Article" });
    routes.MapRoute("default", "{controller=Home}/{action=Index}/{id?}");
});
```

<span data-ttu-id="88f8a-863">Użycie tych definicji tras `Url.Action("Index", "Home")` spowoduje wygenerowanie ścieżki URL `/` z `default` trasą, ale dlaczego?</span><span class="sxs-lookup"><span data-stu-id="88f8a-863">Using these route definitions, `Url.Action("Index", "Home")` will generate the URL path `/` with the `default` route, but why?</span></span> <span data-ttu-id="88f8a-864">Możliwe jest odpuszczenie, że wartości trasy `{ controller = Home, action = Index }` byłyby wystarczające do wygenerowania adresu URL przy użyciu `blog` , a wynikiem będzie `/blog?action=Index&controller=Home` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-864">You might guess the route values `{ controller = Home, action = Index }` would be enough to generate a URL using `blog`, and the result would be `/blog?action=Index&controller=Home`.</span></span>

<span data-ttu-id="88f8a-865">Dedykowane konwencjonalne trasy polegają na specjalnym zachowaniu wartości domyślnych, które nie mają odpowiadającego parametru trasy, który uniemożliwia trasie "zbyt zachłanne" z generowaniem adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-865">Dedicated conventional routes rely on a special behavior of default values that don't have a corresponding route parameter that prevents the route from being "too greedy" with URL generation.</span></span> <span data-ttu-id="88f8a-866">W takim przypadku wartości domyślne to `{ controller = Blog, action = Article }` , a ani nie `controller` `action` pojawiają się jako parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-866">In this case the default values are `{ controller = Blog, action = Article }`, and neither `controller` nor `action` appears as a route parameter.</span></span> <span data-ttu-id="88f8a-867">Gdy Routing wykonuje generowanie adresów URL, podane wartości muszą być zgodne z wartościami domyślnymi.</span><span class="sxs-lookup"><span data-stu-id="88f8a-867">When routing performs URL generation, the values provided must match the default values.</span></span> <span data-ttu-id="88f8a-868">Generowanie adresu URL `blog` nie powiedzie się, ponieważ wartości `{ controller = Home, action = Index }` nie są zgodne `{ controller = Blog, action = Article }` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-868">URL generation using `blog` will fail because the values `{ controller = Home, action = Index }` don't match `{ controller = Blog, action = Article }`.</span></span> <span data-ttu-id="88f8a-869">Następnie usługa routingu wraca do wypróbowania `default` , która się powiedzie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-869">Routing then falls back to try `default`, which succeeds.</span></span>

<a name="routing-areas-ref-label"></a>

## <a name="areas"></a><span data-ttu-id="88f8a-870">Obszary</span><span class="sxs-lookup"><span data-stu-id="88f8a-870">Areas</span></span>

<span data-ttu-id="88f8a-871">[Obszary](areas.md) są funkcją MVC służącą do organizowania powiązanych funkcji w grupie jako oddzielnej przestrzeni nazw routingu (dla akcji kontrolera) i struktury folderów (dla widoków).</span><span class="sxs-lookup"><span data-stu-id="88f8a-871">[Areas](areas.md) are an MVC feature used to organize related functionality into a group as a separate routing-namespace (for controller actions) and folder structure (for views).</span></span> <span data-ttu-id="88f8a-872">Użycie obszarów umożliwia aplikacji posiadanie wielu kontrolerów o tej samej nazwie, o ile mają one różne *obszary*.</span><span class="sxs-lookup"><span data-stu-id="88f8a-872">Using areas allows an application to have multiple controllers with the same name - as long as they have different *areas*.</span></span> <span data-ttu-id="88f8a-873">Za pomocą obszarów tworzy hierarchię na potrzeby routingu przez dodanie kolejnego parametru trasy `area` do `controller` i `action` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-873">Using areas creates a hierarchy for the purpose of routing by adding another route parameter, `area` to `controller` and `action`.</span></span> <span data-ttu-id="88f8a-874">W tej sekcji omówiono sposób, w jaki Routing współdziała z obszarami — zobacz [obszary](areas.md) , aby uzyskać szczegółowe informacje o tym, jak obszary są używane w widokach.</span><span class="sxs-lookup"><span data-stu-id="88f8a-874">This section will discuss how routing interacts with areas - see [Areas](areas.md) for details about how areas are used with views.</span></span>

<span data-ttu-id="88f8a-875">Poniższy przykład konfiguruje MVC do używania domyślnej trasy konwencjonalnej i *trasy obszaru* dla obszaru o nazwie `Blog` :</span><span class="sxs-lookup"><span data-stu-id="88f8a-875">The following example configures MVC to use the default conventional route and an *area route* for an area named `Blog`:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet1)]

<span data-ttu-id="88f8a-876">W przypadku dopasowania ścieżki adresu URL `/Manage/Users/AddUser` , na przykład, Pierwsza trasa spowoduje wygenerowanie wartości trasy `{ area = Blog, controller = Users, action = AddUser }` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-876">When matching a URL path like `/Manage/Users/AddUser`, the first route will produce the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span> <span data-ttu-id="88f8a-877">`area`Wartość trasy jest generowana przez wartość domyślną dla `area` , w rzeczywistości trasa utworzona przez `MapAreaRoute` jest równoważna z następującymi:</span><span class="sxs-lookup"><span data-stu-id="88f8a-877">The `area` route value is produced by a default value for `area`, in fact the route created by `MapAreaRoute` is equivalent to the following:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet2)]

<span data-ttu-id="88f8a-878">`MapAreaRoute` tworzy trasę przy użyciu wartości domyślnej i ograniczenia dotyczącej `area` użycia podanej nazwy obszaru, w tym przypadku `Blog` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-878">`MapAreaRoute` creates a route using both a default value and constraint for `area` using the provided area name, in this case `Blog`.</span></span> <span data-ttu-id="88f8a-879">Wartość domyślna zapewnia, że trasa zawsze produkuje `{ area = Blog, ... }` , ograniczenie wymaga wartości `{ area = Blog, ... }` dla generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-879">The default value ensures that the route always produces `{ area = Blog, ... }`, the constraint requires the value `{ area = Blog, ... }` for URL generation.</span></span>

> [!TIP]
> <span data-ttu-id="88f8a-880">Routowanie konwencjonalne jest zależne od kolejności.</span><span class="sxs-lookup"><span data-stu-id="88f8a-880">Conventional routing is order-dependent.</span></span> <span data-ttu-id="88f8a-881">Ogólnie rzecz biorąc, trasy z obszarami należy umieścić wcześniej w tabeli tras, ponieważ są one bardziej specyficzne niż trasy bez obszaru.</span><span class="sxs-lookup"><span data-stu-id="88f8a-881">In general, routes with areas should be placed earlier in the route table as they're more specific than routes without an area.</span></span>

<span data-ttu-id="88f8a-882">Korzystając z powyższego przykładu, wartości trasy będą zgodne z następującą akcją:</span><span class="sxs-lookup"><span data-stu-id="88f8a-882">Using the above example, the route values would match the following action:</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

<span data-ttu-id="88f8a-883">`AreaAttribute`Wskazuje to, co oznacza kontroler w ramach obszaru. Załóżmy, że ten kontroler znajduje się w `Blog` obszarze.</span><span class="sxs-lookup"><span data-stu-id="88f8a-883">The `AreaAttribute` is what denotes a controller as part of an area, we say that this controller is in the `Blog` area.</span></span> <span data-ttu-id="88f8a-884">Kontrolery bez `[Area]` atrybutu nie są członkami żadnego obszaru i **nie** będą zgodne, gdy `area` wartość trasy zostanie podana przez Routing.</span><span class="sxs-lookup"><span data-stu-id="88f8a-884">Controllers without an `[Area]` attribute are not members of any area, and will **not** match when the `area` route value is provided by routing.</span></span> <span data-ttu-id="88f8a-885">W poniższym przykładzie tylko pierwszy kontroler może być zgodny z wartościami trasy `{ area = Blog, controller = Users, action = AddUser }` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-885">In the following example, only the first controller listed can match the route values `{ area = Blog, controller = Users, action = AddUser }`.</span></span>

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Blog/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Zebra/Controllers/UsersController.cs)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Controllers/UsersController.cs)]

> [!NOTE]
> <span data-ttu-id="88f8a-886">Przestrzeń nazw każdego kontrolera jest pokazana w tym miejscu w celu zapewnienia kompletności — w przeciwnym razie kontrolery będą mieć konflikt nazw i generują błąd kompilatora.</span><span class="sxs-lookup"><span data-stu-id="88f8a-886">The namespace of each controller is shown here for completeness - otherwise the controllers would have a naming conflict and generate a compiler error.</span></span> <span data-ttu-id="88f8a-887">Przestrzenie nazw klas nie mają wpływu na Routing MVC.</span><span class="sxs-lookup"><span data-stu-id="88f8a-887">Class namespaces have no effect on MVC's routing.</span></span>

<span data-ttu-id="88f8a-888">Pierwsze dwa kontrolery są członkami obszarów i są zgodne tylko wtedy, gdy ich nazwa obszaru jest podana przez `area` wartość trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-888">The first two controllers are members of areas, and only match when their respective area name is provided by the `area` route value.</span></span> <span data-ttu-id="88f8a-889">Trzeci kontroler nie jest członkiem żadnego obszaru i można go dopasować tylko wtedy, gdy nie ma żadnej wartości parametru `area` Routing.</span><span class="sxs-lookup"><span data-stu-id="88f8a-889">The third controller isn't a member of any area, and can only match when no value for `area` is provided by routing.</span></span>

> [!NOTE]
> <span data-ttu-id="88f8a-890">W warunkach pasujących do *nie ma żadnej* wartości, brak `area` wartości jest taka sama jak wartość `area` null lub pusty ciąg.</span><span class="sxs-lookup"><span data-stu-id="88f8a-890">In terms of matching *no value*, the absence of the `area` value is the same as if the value for `area` were null or the empty string.</span></span>

<span data-ttu-id="88f8a-891">Podczas wykonywania akcji wewnątrz obszaru wartość trasy dla `area` będzie dostępna jako *wartość otoczenia* dla routingu do użycia na potrzeby generowania adresów URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-891">When executing an action inside an area, the route value for `area` will be available as an *ambient value* for routing to use for URL generation.</span></span> <span data-ttu-id="88f8a-892">Oznacza to, że domyślnie obszary programu *Sticky Notes* mają być używane do generowania adresów URL, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-892">This means that by default areas act *sticky* for URL generation as demonstrated by the following sample.</span></span>
[!code-csharp[](routing/samples/3.x/AreasRouting/Startup.cs?name=snippet3)]

[!code-csharp[](routing/samples/3.x/AreasRouting/Areas/Duck/Controllers/UsersController.cs)]

<a name="iactionconstraint-ref-label"></a>

## <a name="understanding-iactionconstraint"></a><span data-ttu-id="88f8a-893">Zrozumienie IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="88f8a-893">Understanding IActionConstraint</span></span>

> [!NOTE]
> <span data-ttu-id="88f8a-894">Ta sekcja jest głęboką szczegółoweą wewnętrznych struktur oraz jak MVC wybiera akcję do wykonania.</span><span class="sxs-lookup"><span data-stu-id="88f8a-894">This section is a deep-dive on framework internals and how MVC chooses an action to execute.</span></span> <span data-ttu-id="88f8a-895">Typowa aplikacja nie będzie potrzebować niestandardowego `IActionConstraint`</span><span class="sxs-lookup"><span data-stu-id="88f8a-895">A typical application won't need a custom `IActionConstraint`</span></span>

<span data-ttu-id="88f8a-896">Prawdopodobnie już było używane, `IActionConstraint` nawet jeśli nie masz doświadczenia z interfejsem.</span><span class="sxs-lookup"><span data-stu-id="88f8a-896">You have likely already used `IActionConstraint` even if you're not familiar with the interface.</span></span> <span data-ttu-id="88f8a-897">`[HttpGet]`Atrybut i podobne `[Http-VERB]` atrybuty implementują `IActionConstraint` w celu ograniczenia wykonywania metody akcji.</span><span class="sxs-lookup"><span data-stu-id="88f8a-897">The `[HttpGet]` Attribute and similar `[Http-VERB]` attributes implement `IActionConstraint` in order to limit the execution of an action method.</span></span>

```csharp
public class ProductsController : Controller
{
    [HttpGet]
    public IActionResult Edit() { }

    public IActionResult Edit(...) { }
}
```

<span data-ttu-id="88f8a-898">Przy założeniu domyślnej trasy konwencjonalnej, ścieżka URL `/Products/Edit` będzie generować wartości `{ controller = Products, action = Edit }` , które byłyby zgodne z **obiema** akcjami przedstawionymi w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-898">Assuming the default conventional route, the URL path `/Products/Edit` would produce the values `{ controller = Products, action = Edit }`, which would match **both** of the actions shown here.</span></span> <span data-ttu-id="88f8a-899">W `IActionConstraint` terminologii mamy, że obie te akcje są uważane za kandydatów, ponieważ oba te są zgodne z danymi trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-899">In `IActionConstraint` terminology we would say that both of these actions are considered candidates - as they both match the route data.</span></span>

<span data-ttu-id="88f8a-900">Gdy `HttpGetAttribute` zostanie wykonane, zobaczy, że *Edit ()* jest dopasowaniem do *Get* i nie jest dopasowaniem dla żadnego innego zlecenia http.</span><span class="sxs-lookup"><span data-stu-id="88f8a-900">When the `HttpGetAttribute` executes, it will say that *Edit()* is a match for *GET* and isn't a match for any other HTTP verb.</span></span> <span data-ttu-id="88f8a-901">`Edit(...)`Akcja nie ma zdefiniowanych żadnych ograniczeń i dlatego będzie pasować do dowolnego czasownika http.</span><span class="sxs-lookup"><span data-stu-id="88f8a-901">The `Edit(...)` action doesn't have any constraints defined, and so will match any HTTP verb.</span></span> <span data-ttu-id="88f8a-902">Dlatego przy założeniu, że `POST` `Edit(...)` dopasowań są tylko.</span><span class="sxs-lookup"><span data-stu-id="88f8a-902">So assuming a `POST` - only `Edit(...)` matches.</span></span> <span data-ttu-id="88f8a-903">Jednak w przypadku `GET` obu akcji można nadal dopasować się, jednak akcja z funkcją `IActionConstraint` jest zawsze uznawana za *lepszą* od akcji bez.</span><span class="sxs-lookup"><span data-stu-id="88f8a-903">But, for a `GET` both actions can still match - however, an action with an `IActionConstraint` is always considered *better* than an action without.</span></span> <span data-ttu-id="88f8a-904">Dlatego, że jest `Edit()` `[HttpGet]` traktowany jako bardziej szczegółowy i zostanie wybrany, jeśli obie akcje mogą być zgodne.</span><span class="sxs-lookup"><span data-stu-id="88f8a-904">So because `Edit()` has `[HttpGet]` it's considered more specific, and will be selected if both actions can match.</span></span>

<span data-ttu-id="88f8a-905">Koncepcyjnie, `IActionConstraint` jest formą *przeciążenia*, ale zamiast przeciążać metody o tej samej nazwie, przeciążanie między akcjami, które pasują do tego samego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="88f8a-905">Conceptually, `IActionConstraint` is a form of *overloading*, but instead of overloading methods with the same name, it's overloading between actions that match the same URL.</span></span> <span data-ttu-id="88f8a-906">Funkcja routingu atrybutów używa także `IActionConstraint` i może powodować, że działania z różnych kontrolerów są traktowane jako kandydaci.</span><span class="sxs-lookup"><span data-stu-id="88f8a-906">Attribute routing also uses `IActionConstraint` and can result in actions from different controllers both being considered candidates.</span></span>

<a name="iactionconstraint-impl-ref-label"></a>

### <a name="implementing-iactionconstraint"></a><span data-ttu-id="88f8a-907">Implementowanie IActionConstraint</span><span class="sxs-lookup"><span data-stu-id="88f8a-907">Implementing IActionConstraint</span></span>

<span data-ttu-id="88f8a-908">Najprostszym sposobem implementacji `IActionConstraint` jest utworzenie klasy pochodnej od `System.Attribute` i umieszczenie jej w akcjach i kontrolerach.</span><span class="sxs-lookup"><span data-stu-id="88f8a-908">The simplest way to implement an `IActionConstraint` is to create a class derived from `System.Attribute` and place it on your actions and controllers.</span></span> <span data-ttu-id="88f8a-909">MVC automatycznie odnajdzie wszystkie `IActionConstraint` , które są stosowane jako atrybuty.</span><span class="sxs-lookup"><span data-stu-id="88f8a-909">MVC will automatically discover any `IActionConstraint` that are applied as attributes.</span></span> <span data-ttu-id="88f8a-910">Możesz użyć modelu aplikacji, aby zastosować ograniczenia i prawdopodobnie jest to najbardziej elastyczne podejście, ponieważ pozwala to na to, w jaki sposób są stosowane.</span><span class="sxs-lookup"><span data-stu-id="88f8a-910">You can use the application model to apply constraints, and this is probably the most flexible approach as it allows you to metaprogram how they're applied.</span></span>

<span data-ttu-id="88f8a-911">W poniższym przykładzie ograniczenie wybiera akcję na podstawie *kodu kraju* z danych trasy.</span><span class="sxs-lookup"><span data-stu-id="88f8a-911">In the following example, a constraint chooses an action based on a *country code* from the route data.</span></span> <span data-ttu-id="88f8a-912">[Pełny przykład w witrynie GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span><span class="sxs-lookup"><span data-stu-id="88f8a-912">The [full sample on GitHub](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.ActionConstraintSample.Web/CountrySpecificAttribute.cs).</span></span>

```csharp
public class CountrySpecificAttribute : Attribute, IActionConstraint
{
    private readonly string _countryCode;

    public CountrySpecificAttribute(string countryCode)
    {
        _countryCode = countryCode;
    }

    public int Order
    {
        get
        {
            return 0;
        }
    }

    public bool Accept(ActionConstraintContext context)
    {
        return string.Equals(
            context.RouteContext.RouteData.Values["country"].ToString(),
            _countryCode,
            StringComparison.OrdinalIgnoreCase);
    }
}
```

<span data-ttu-id="88f8a-913">Użytkownik jest odpowiedzialny za implementację `Accept` metody i wybranie "Order" dla ograniczenia, które ma zostać wykonane.</span><span class="sxs-lookup"><span data-stu-id="88f8a-913">You are responsible for implementing the `Accept` method and choosing an 'Order' for the constraint to execute.</span></span> <span data-ttu-id="88f8a-914">W tym przypadku `Accept` Metoda zwraca wartość, `true` aby zauważyć, że akcja jest dopasowanie, gdy `country` wartość trasy jest zgodna.</span><span class="sxs-lookup"><span data-stu-id="88f8a-914">In this case, the `Accept` method returns `true` to denote the action is a match when the `country` route value matches.</span></span> <span data-ttu-id="88f8a-915">Różni się to od elementu `RouteValueAttribute` , który umożliwia powrót do akcji, która nie ma atrybutu.</span><span class="sxs-lookup"><span data-stu-id="88f8a-915">This is different from a `RouteValueAttribute` in that it allows fallback to a non-attributed action.</span></span> <span data-ttu-id="88f8a-916">Przykład pokazuje, że w przypadku zdefiniowania `en-US` akcji, kod kraju, taki jak, powróci `fr-FR` do bardziej ogólnego kontrolera, który nie został `[CountrySpecific(...)]` zastosowany.</span><span class="sxs-lookup"><span data-stu-id="88f8a-916">The sample shows that if you define an `en-US` action then a country code like `fr-FR` will fall back to a more generic controller that doesn't have `[CountrySpecific(...)]` applied.</span></span>

<span data-ttu-id="88f8a-917">`Order`Właściwość decyduje, *który etap* jest częścią tego ograniczenia.</span><span class="sxs-lookup"><span data-stu-id="88f8a-917">The `Order` property decides which *stage* the constraint is part of.</span></span> <span data-ttu-id="88f8a-918">Ograniczenia akcji są uruchamiane w grupach na podstawie `Order` .</span><span class="sxs-lookup"><span data-stu-id="88f8a-918">Action constraints run in groups based on the `Order`.</span></span> <span data-ttu-id="88f8a-919">Na przykład wszystkie atrybuty metody HTTP podane przez platformę używają tej samej `Order` wartości, aby były uruchamiane na tym samym etapie.</span><span class="sxs-lookup"><span data-stu-id="88f8a-919">For example, all of the framework provided HTTP method attributes use the same `Order` value so that they run in the same stage.</span></span> <span data-ttu-id="88f8a-920">Możesz mieć tyle etapów, ile potrzebujesz, aby zaimplementować odpowiednie zasady.</span><span class="sxs-lookup"><span data-stu-id="88f8a-920">You can have as many stages as you need to implement your desired policies.</span></span>

> [!TIP]
> <span data-ttu-id="88f8a-921">Aby zdecydować, `Order` czy należy zastosować ograniczenie, przed metodami http.</span><span class="sxs-lookup"><span data-stu-id="88f8a-921">To decide on a value for `Order` think about whether or not your constraint should be applied before HTTP methods.</span></span> <span data-ttu-id="88f8a-922">Najpierw należy uruchomić mniejsze numery.</span><span class="sxs-lookup"><span data-stu-id="88f8a-922">Lower numbers run first.</span></span>

::: moniker-end
