---
title: Co nowego w ASP.NET Core 5,0
author: rick-anderson
description: Dowiedz się więcej o nowych funkcjach w ASP.NET Core 5,0.
ms.author: riande
ms.custom: mvc
ms.date: 10/29/2020
no-loc:
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
- Kestrel
uid: aspnetcore-5.0
ms.openlocfilehash: 64e068194744d6c5495d8ecaf31076c7a1c1f349
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102585802"
---
# <a name="whats-new-in-aspnet-core-50"></a><span data-ttu-id="7561c-103">Co nowego w ASP.NET Core 5,0</span><span class="sxs-lookup"><span data-stu-id="7561c-103">What's new in ASP.NET Core 5.0</span></span>

<span data-ttu-id="7561c-104">W tym artykule przedstawiono najbardziej znaczące zmiany w ASP.NET Core 5,0 z linkami do odpowiedniej dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="7561c-104">This article highlights the most significant changes in ASP.NET Core 5.0 with links to relevant documentation.</span></span>

## <a name="aspnet-core-mvc-and-razor-improvements"></a><span data-ttu-id="7561c-105">ASP.NET Core MVC i Razor ulepszenia</span><span class="sxs-lookup"><span data-stu-id="7561c-105">ASP.NET Core MVC and Razor improvements</span></span>

### <a name="model-binding-datetime-as-utc"></a><span data-ttu-id="7561c-106">Element DateTime powiązania modelu jako czas UTC</span><span class="sxs-lookup"><span data-stu-id="7561c-106">Model binding DateTime as UTC</span></span>

<span data-ttu-id="7561c-107">Powiązanie modelu obsługuje teraz Powiązywanie ciągów czasu UTC z `DateTime` .</span><span class="sxs-lookup"><span data-stu-id="7561c-107">Model binding now supports binding UTC time strings to `DateTime`.</span></span> <span data-ttu-id="7561c-108">Jeśli żądanie zawiera ciąg czasu UTC, powiązanie modelu wiąże go z czasem UTC `DateTime` .</span><span class="sxs-lookup"><span data-stu-id="7561c-108">If the request contains a UTC time string, model binding binds it to a UTC `DateTime`.</span></span> <span data-ttu-id="7561c-109">Na przykład następujący ciąg czasu jest związany z czasem UTC `DateTime` : `https://example.com/mycontroller/myaction?time=2019-06-14T02%3A30%3A04.0576719Z`</span><span class="sxs-lookup"><span data-stu-id="7561c-109">For example, the following time string is bound the UTC `DateTime`: `https://example.com/mycontroller/myaction?time=2019-06-14T02%3A30%3A04.0576719Z`</span></span>

### <a name="model-binding-and-validation-with-c-9-record-types"></a><span data-ttu-id="7561c-110">Powiązanie modelu i walidacja przy użyciu typów rekordów języka C# 9</span><span class="sxs-lookup"><span data-stu-id="7561c-110">Model binding and validation with C# 9 record types</span></span>

<span data-ttu-id="7561c-111">[Typy rekordów języka C# 9](/dotnet/csharp/whats-new/csharp-9#record-types) mogą być używane z powiązaniem modelu w kontrolerze MVC lub Razor stronie.</span><span class="sxs-lookup"><span data-stu-id="7561c-111">[C# 9 record types](/dotnet/csharp/whats-new/csharp-9#record-types) can be used with model binding in an MVC controller or a Razor Page.</span></span> <span data-ttu-id="7561c-112">Typy rekordów to dobry sposób na Modelowanie danych przesyłanych przez sieć.</span><span class="sxs-lookup"><span data-stu-id="7561c-112">Record types are a good way to model data being transmitted over the network.</span></span>

<span data-ttu-id="7561c-113">Na przykład następujące polecenie `PersonController` używa `Person` typu rekordu z powiązaniem modelu i walidacją formularza:</span><span class="sxs-lookup"><span data-stu-id="7561c-113">For example, the following `PersonController` uses the `Person` record type with model binding and form validation:</span></span>

```csharp
public record Person([Required] string Name, [Range(0, 150)] int Age);

public class PersonController
{
   public IActionResult Index() => View();

   [HttpPost]
   public IActionResult Index(Person person)
   {
          // ...
   }
}
```

<span data-ttu-id="7561c-114">`Person/Index.cshtml`Plik:</span><span class="sxs-lookup"><span data-stu-id="7561c-114">The `Person/Index.cshtml` file:</span></span>

```cshtml
@model Person

Name: <input asp-for="Model.Name" />
<span asp-validation-for="Model.Name" />

Age: <input asp-for="Model.Age" />
<span asp-validation-for="Model.Age" />
```

### <a name="improvements-to-dynamicroutevaluetransformer"></a><span data-ttu-id="7561c-115">Ulepszenia DynamicRouteValueTransformer</span><span class="sxs-lookup"><span data-stu-id="7561c-115">Improvements to DynamicRouteValueTransformer</span></span>

<span data-ttu-id="7561c-116">ASP.NET Core 3,1 wprowadzony <xref:Microsoft.AspNetCore.Mvc.Routing.DynamicRouteValueTransformer> jako sposób używania niestandardowego punktu końcowego do dynamicznego wybierania akcji kontrolera MVC lub Razor strony.</span><span class="sxs-lookup"><span data-stu-id="7561c-116">ASP.NET Core 3.1 introduced <xref:Microsoft.AspNetCore.Mvc.Routing.DynamicRouteValueTransformer> as a way to use custom endpoint to dynamically select an MVC controller action or a Razor page.</span></span> <span data-ttu-id="7561c-117">Aplikacje ASP.NET Core 5,0 umożliwiają przekazanie stanu do `DynamicRouteValueTransformer` i filtrowanie zestawu punktów końcowych.</span><span class="sxs-lookup"><span data-stu-id="7561c-117">ASP.NET Core 5.0 apps can pass state to a `DynamicRouteValueTransformer` and filter the set of endpoints chosen.</span></span>

### <a name="miscellaneous"></a><span data-ttu-id="7561c-118">Różne</span><span class="sxs-lookup"><span data-stu-id="7561c-118">Miscellaneous</span></span>

* <span data-ttu-id="7561c-119">Atrybut [[Compare]](xref:System.ComponentModel.DataAnnotations.CompareAttribute) można zastosować do właściwości w Razor modelu strony.</span><span class="sxs-lookup"><span data-stu-id="7561c-119">The [[Compare]](xref:System.ComponentModel.DataAnnotations.CompareAttribute) attribute can be applied to properties on a Razor Page model.</span></span>
* <span data-ttu-id="7561c-120">Parametry i właściwości powiązane z treścią są uważane za wymagane domyślnie.</span><span class="sxs-lookup"><span data-stu-id="7561c-120">Parameters and properties bound from the body are considered required by default.</span></span> <!-- Review: How is this different from 3.1
The validation system in .NET Core 3.0 and later treats non-nullable parameters or bound properties as if they had a [Required] attribute.
see https://docs.microsoft.com/aspnet/core/mvc/models/validation?view=aspnetcore-3.1   
-->

## <a name="web-api"></a><span data-ttu-id="7561c-121">Interfejs API sieci Web</span><span class="sxs-lookup"><span data-stu-id="7561c-121">Web API</span></span>

### <a name="openapi-specification-on-by-default"></a><span data-ttu-id="7561c-122">Domyślnie Specyfikacja OpenAPI</span><span class="sxs-lookup"><span data-stu-id="7561c-122">OpenAPI Specification on by default</span></span>

<span data-ttu-id="7561c-123">[Specyfikacja openapi](http://spec.openapis.org/oas/v3.0.3) jest branżowym standardem do opisywania interfejsów API protokołu HTTP i integrowania ich z złożonymi procesami biznesowymi lub podmiotami trzecimi.</span><span class="sxs-lookup"><span data-stu-id="7561c-123">[OpenAPI Specification](http://spec.openapis.org/oas/v3.0.3) is an industry standard for describing HTTP APIs and integrating them into complex business processes or with third parties.</span></span> <span data-ttu-id="7561c-124">OpenAPI jest szeroko obsługiwana przez wszystkich dostawców chmury i wiele rejestrów interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="7561c-124">OpenAPI is widely supported by all cloud providers and many API registries.</span></span> <span data-ttu-id="7561c-125">Aplikacje, które emitują OpenAPI dokumenty z interfejsów API sieci Web, mają różne nowe możliwości, w których można używać tych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="7561c-125">Apps that emit OpenAPI documents from web APIs have a variety of new opportunities in which those APIs can be used.</span></span> <span data-ttu-id="7561c-126">W obszarze partnerstwa z obsłudze projektu typu open source [Swashbuckle. AspNetCore](https://www.nuget.org/packages/Swashbuckle.AspNetCore/)szablon interfejsu API ASP.NET Core zawiera zależność NuGet dla [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle.AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="7561c-126">In partnership with the maintainers of the open-source project [Swashbuckle.AspNetCore](https://www.nuget.org/packages/Swashbuckle.AspNetCore/), the ASP.NET Core API template contains a NuGet dependency on [Swashbuckle](https://github.com/domaindrivendev/Swashbuckle.AspNetCore).</span></span> <span data-ttu-id="7561c-127">Swashbuckle to popularny pakiet NuGet typu "open source", który dynamicznie emituje dokumenty OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="7561c-127">Swashbuckle is a popular open-source NuGet package that emits OpenAPI documents dynamically.</span></span> <span data-ttu-id="7561c-128">Swashbuckle to przez introspecting kontrolerami API i Generowanie dokumentu OpenAPI w czasie wykonywania lub w czasie kompilacji przy użyciu interfejsu wiersza polecenia Swashbuckle.</span><span class="sxs-lookup"><span data-stu-id="7561c-128">Swashbuckle does this by introspecting over the API controllers and generating the OpenAPI document at run-time, or at build time using the Swashbuckle CLI.</span></span>

<span data-ttu-id="7561c-129">W ASP.NET Core 5,0 szablony internetowego interfejsu API domyślnie włączają obsługę OpenAPI.</span><span class="sxs-lookup"><span data-stu-id="7561c-129">In ASP.NET Core 5.0, the web API templates enable the OpenAPI support by default.</span></span> <span data-ttu-id="7561c-130">Aby wyłączyć OpenAPI:</span><span class="sxs-lookup"><span data-stu-id="7561c-130">To disable OpenAPI:</span></span>

* <span data-ttu-id="7561c-131">Za pomocą wiersza polecenia:</span><span class="sxs-lookup"><span data-stu-id="7561c-131">From the command line:</span></span>

    ```dotnetcli
    dotnet new webapi --no-openapi true
    ```
* <span data-ttu-id="7561c-132">W programie Visual Studio: Usuń zaznaczenie opcji **Włącz obsługę openapi**.</span><span class="sxs-lookup"><span data-stu-id="7561c-132">From Visual Studio: Uncheck **Enable OpenAPI support**.</span></span>

<span data-ttu-id="7561c-133">Wszystkie pliki *csproj* utworzone dla projektów internetowego interfejsu API zawierają odwołanie do pakietu NuGet [Swashbuckle. AspNetCore](https://www.nuget.org/packages/Swashbuckle.AspNetCore/) .</span><span class="sxs-lookup"><span data-stu-id="7561c-133">All *.csproj* files created for web API projects contain the [Swashbuckle.AspNetCore](https://www.nuget.org/packages/Swashbuckle.AspNetCore/) NuGet package reference.</span></span>

```xml
<ItemGroup>
    <PackageReference Include="Swashbuckle.AspNetCore" Version="5.5.1" />
</ItemGroup>
```

<span data-ttu-id="7561c-134">Kod wygenerowany przez szablon zawiera kod `Startup.ConfigureServices` , który aktywuje Generowanie dokumentu openapi:</span><span class="sxs-lookup"><span data-stu-id="7561c-134">The template generated code contains code in `Startup.ConfigureServices` that activates OpenAPI document generation:</span></span>

[!code-csharp[](~/release-notes/sample/StartupSwagger.cs?name=snippet)]

<span data-ttu-id="7561c-135">`Startup.Configure`Metoda dodaje oprogramowanie pośredniczące Swashbuckle, które umożliwia:</span><span class="sxs-lookup"><span data-stu-id="7561c-135">The `Startup.Configure` method adds the Swashbuckle middleware, which enables the:</span></span>

* <span data-ttu-id="7561c-136">Proces generowania dokumentu.</span><span class="sxs-lookup"><span data-stu-id="7561c-136">Document generation process.</span></span>
* <span data-ttu-id="7561c-137">Strona interfejsu użytkownika struktury Swagger domyślnie w trybie projektowania.</span><span class="sxs-lookup"><span data-stu-id="7561c-137">Swagger UI page by default in development mode.</span></span>

<span data-ttu-id="7561c-138">Kod wygenerowany przez szablon nie spowoduje przypadkowego uwidocznienia opisu interfejsu API podczas publikowania w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="7561c-138">The template generated code won't accidentally expose the API's description when publishing to production.</span></span>

[!code-csharp[](~/release-notes/sample/StartupSwagger.cs?name=snippet2)]

#### <a name="azure-api-management-import"></a><span data-ttu-id="7561c-139">Importowanie API Management platformy Azure</span><span class="sxs-lookup"><span data-stu-id="7561c-139">Azure API Management Import</span></span>

<span data-ttu-id="7561c-140">Gdy ASP.NET Core projekty interfejsu API włączają OpenAPI, publikowanie programu Visual Studio 2019 w wersji 16,8 i nowszych automatycznie oferuje dodatkowy krok w przepływie publikowania.</span><span class="sxs-lookup"><span data-stu-id="7561c-140">When ASP.NET Core API projects enable OpenAPI, the Visual Studio 2019 version 16.8 and later publishing automatically offer an additional step in the publishing flow.</span></span> <span data-ttu-id="7561c-141">Deweloperzy korzystający z [platformy azure API Management](xref:tutorials/publish-to-azure-api-management-using-vs) mogą automatycznie zaimportować interfejsy API do usługi Azure API Management podczas przepływu publikowania:</span><span class="sxs-lookup"><span data-stu-id="7561c-141">Developers who use [Azure API Management](xref:tutorials/publish-to-azure-api-management-using-vs) have an opportunity to automatically import the APIs into Azure API Management during the publish flow:</span></span>

![Azure API Management Importowanie a publikowanie](~/release-notes/static/publish-to-apim.png)

### <a name="better-launch-experience-for-web-api-projects"></a><span data-ttu-id="7561c-143">Lepsze środowisko uruchamiania dla projektów interfejsu API sieci Web</span><span class="sxs-lookup"><span data-stu-id="7561c-143">Better launch experience for web API projects</span></span>

<span data-ttu-id="7561c-144">W przypadku OpenAPI domyślnie włączone środowisko uruchamiania aplikacji (F5) dla deweloperów interfejsu API sieci Web znacznie się zwiększa.</span><span class="sxs-lookup"><span data-stu-id="7561c-144">With OpenAPI enabled by default, the app launching experience (F5) for web API developers significantly improves.</span></span> <span data-ttu-id="7561c-145">W ASP.NET Core 5,0 szablon internetowego interfejsu API zawiera wstępnie skonfigurowane do załadowania strony interfejsu użytkownika struktury Swagger.</span><span class="sxs-lookup"><span data-stu-id="7561c-145">With ASP.NET Core 5.0, the web API template comes pre-configured to load up the Swagger UI page.</span></span> <span data-ttu-id="7561c-146">Strona interfejsu użytkownika struktury Swagger zawiera dokumentację dodaną dla opublikowanego interfejsu API i umożliwia testowanie interfejsów API za pomocą jednego kliknięcia.</span><span class="sxs-lookup"><span data-stu-id="7561c-146">The Swagger UI page provides both the documentation added for the published API, and enables testing the APIs with a single click.</span></span>

![Widok Swagger/index.html](~/release-notes/static/swagger-ui-page-rc1.png)

## Blazor

### <a name="performance-improvements"></a><span data-ttu-id="7561c-148">Usprawnienia wydajności</span><span class="sxs-lookup"><span data-stu-id="7561c-148">Performance improvements</span></span>

<span data-ttu-id="7561c-149">W przypadku platformy .NET 5 Wprowadziliśmy znaczące ulepszenia Blazor WebAssembly wydajności środowiska uruchomieniowego z określonym fokusem na potrzeby skomplikowanych renderingów interfejsu użytkownika i serializacji JSON.</span><span class="sxs-lookup"><span data-stu-id="7561c-149">For .NET 5, we made significant improvements to Blazor WebAssembly runtime performance with a specific focus on complex UI rendering and JSON serialization.</span></span> <span data-ttu-id="7561c-150">W naszych testach wydajności Blazor WebAssembly w przypadku większości scenariuszy środowisko .NET 5 jest szybsze od dwóch do trzech razy.</span><span class="sxs-lookup"><span data-stu-id="7561c-150">In our performance tests, Blazor WebAssembly in .NET 5 is two to three times faster for most scenarios.</span></span> <span data-ttu-id="7561c-151">Aby uzyskać więcej informacji, zobacz [Blog ASP.NET: aktualizacje ASP.NET Core w programie .NET 5 Release Candidate 1](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-1/#blazor-webassembly-performance-improvements).</span><span class="sxs-lookup"><span data-stu-id="7561c-151">For more information, see [ASP.NET Blog: ASP.NET Core updates in .NET 5 Release Candidate 1](https://devblogs.microsoft.com/aspnet/asp-net-core-updates-in-net-5-release-candidate-1/#blazor-webassembly-performance-improvements).</span></span>

### <a name="css-isolation"></a><span data-ttu-id="7561c-152">Izolacja CSS</span><span class="sxs-lookup"><span data-stu-id="7561c-152">CSS isolation</span></span>

<span data-ttu-id="7561c-153">Blazor Program obsługuje teraz Definiowanie stylów CSS, które są objęte zakresem danego składnika.</span><span class="sxs-lookup"><span data-stu-id="7561c-153">Blazor now supports defining CSS styles that are scoped to a given component.</span></span> <span data-ttu-id="7561c-154">Style CSS specyficzne dla składnika ułatwiają powodowanie stylów w aplikacji i unikanie niezamierzonych efektów ubocznych stylów globalnych.</span><span class="sxs-lookup"><span data-stu-id="7561c-154">Component-specific CSS styles make it easier to reason about the styles in an app and to avoid unintentional side effects of global styles.</span></span> <span data-ttu-id="7561c-155">Aby uzyskać więcej informacji, zobacz <xref:blazor/components/css-isolation>.</span><span class="sxs-lookup"><span data-stu-id="7561c-155">For more information, see <xref:blazor/components/css-isolation>.</span></span>

### <a name="new-inputfile-component"></a><span data-ttu-id="7561c-156">Nowy `InputFile` składnik</span><span class="sxs-lookup"><span data-stu-id="7561c-156">New `InputFile` component</span></span>

<span data-ttu-id="7561c-157">`InputFile`Składnik umożliwia odczytywanie co najmniej jednego pliku wybranego przez użytkownika do przekazania.</span><span class="sxs-lookup"><span data-stu-id="7561c-157">The `InputFile` component allows reading one or more files selected by a user for upload.</span></span> <span data-ttu-id="7561c-158">Aby uzyskać więcej informacji, zobacz <xref:blazor/file-uploads>.</span><span class="sxs-lookup"><span data-stu-id="7561c-158">For more information, see <xref:blazor/file-uploads>.</span></span>

### <a name="new-inputradio-and-inputradiogroup-components"></a><span data-ttu-id="7561c-159">Nowe `InputRadio` i `InputRadioGroup` składniki</span><span class="sxs-lookup"><span data-stu-id="7561c-159">New `InputRadio` and `InputRadioGroup` components</span></span>

<span data-ttu-id="7561c-160">Blazor Program zawiera wbudowane `InputRadio` i `InputRadioGroup` składniki upraszczające Powiązywanie danych z grupami przycisków radiowych ze zintegrowanym sprawdzaniem poprawności.</span><span class="sxs-lookup"><span data-stu-id="7561c-160">Blazor has built-in `InputRadio` and `InputRadioGroup` components that simplify data binding to radio button groups with integrated validation.</span></span> <span data-ttu-id="7561c-161">Aby uzyskać więcej informacji, zobacz <xref:blazor/forms-validation>.</span><span class="sxs-lookup"><span data-stu-id="7561c-161">For more information, see <xref:blazor/forms-validation>.</span></span>

### <a name="component-virtualization"></a><span data-ttu-id="7561c-162">Wirtualizacja składników</span><span class="sxs-lookup"><span data-stu-id="7561c-162">Component virtualization</span></span>

<span data-ttu-id="7561c-163">Popraw postrzeganą wydajność renderowania składników przy użyciu Blazor wbudowanej obsługi wirtualizacji platformy.</span><span class="sxs-lookup"><span data-stu-id="7561c-163">Improve the perceived performance of component rendering using the Blazor framework's built-in virtualization support.</span></span> <span data-ttu-id="7561c-164">Aby uzyskać więcej informacji, zobacz <xref:blazor/components/virtualization>.</span><span class="sxs-lookup"><span data-stu-id="7561c-164">For more information, see <xref:blazor/components/virtualization>.</span></span>

### <a name="ontoggle-event-support"></a><span data-ttu-id="7561c-165">`ontoggle` Obsługa zdarzeń</span><span class="sxs-lookup"><span data-stu-id="7561c-165">`ontoggle` event support</span></span>

<span data-ttu-id="7561c-166">Blazor zdarzenia obsługują teraz `ontoggle` zdarzenie dom.</span><span class="sxs-lookup"><span data-stu-id="7561c-166">Blazor events now support the `ontoggle` DOM event.</span></span> <span data-ttu-id="7561c-167">Aby uzyskać więcej informacji, zobacz <xref:blazor/components/event-handling#event-argument-types>.</span><span class="sxs-lookup"><span data-stu-id="7561c-167">For more information, see <xref:blazor/components/event-handling#event-argument-types>.</span></span>

### <a name="set-ui-focus-in-blazor-apps"></a><span data-ttu-id="7561c-168">Ustawianie fokusu interfejsu użytkownika w Blazor aplikacjach</span><span class="sxs-lookup"><span data-stu-id="7561c-168">Set UI focus in Blazor apps</span></span>

<span data-ttu-id="7561c-169">Użyj `FocusAsync` metody wygodnej dla odwołań do elementów, aby ustawić fokus interfejsu użytkownika dla tego elementu.</span><span class="sxs-lookup"><span data-stu-id="7561c-169">Use the `FocusAsync` convenience method on element references to set the UI focus to that element.</span></span> <span data-ttu-id="7561c-170">Aby uzyskać więcej informacji, zobacz <xref:blazor/components/event-handling#focus-an-element>.</span><span class="sxs-lookup"><span data-stu-id="7561c-170">For more information, see <xref:blazor/components/event-handling#focus-an-element>.</span></span>

### <a name="custom-validation-class-attributes"></a><span data-ttu-id="7561c-171">Niestandardowe atrybuty klasy walidacji</span><span class="sxs-lookup"><span data-stu-id="7561c-171">Custom validation class attributes</span></span>

<span data-ttu-id="7561c-172">Niestandardowe nazwy klas walidacji są przydatne podczas integrowania z strukturami CSS, takimi jak Bootstrap.</span><span class="sxs-lookup"><span data-stu-id="7561c-172">Custom validation class names are useful when integrating with CSS frameworks, such as Bootstrap.</span></span> <span data-ttu-id="7561c-173">Aby uzyskać więcej informacji, zobacz <xref:blazor/forms-validation#custom-validation-class-attributes>.</span><span class="sxs-lookup"><span data-stu-id="7561c-173">For more information, see <xref:blazor/forms-validation#custom-validation-class-attributes>.</span></span>

### <a name="iasyncdisposable-support"></a><span data-ttu-id="7561c-174">Obsługa IAsyncDisposable</span><span class="sxs-lookup"><span data-stu-id="7561c-174">IAsyncDisposable support</span></span>

<span data-ttu-id="7561c-175">Blazor składniki obsługują teraz <xref:System.IAsyncDisposable> interfejs do asynchronicznej wersji przyznanych zasobów.</span><span class="sxs-lookup"><span data-stu-id="7561c-175">Blazor components now support the <xref:System.IAsyncDisposable> interface for the asynchronous release of allocated resources.</span></span>

### <a name="javascript-isolation-and-object-references"></a><span data-ttu-id="7561c-176">Izolacja kodu JavaScript i odwołania do obiektów</span><span class="sxs-lookup"><span data-stu-id="7561c-176">JavaScript isolation and object references</span></span>

<span data-ttu-id="7561c-177">Blazor Włącza izolację JavaScript w standardowych [modułach języka JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Modules).</span><span class="sxs-lookup"><span data-stu-id="7561c-177">Blazor enables JavaScript isolation in standard [JavaScript modules](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Modules).</span></span> <span data-ttu-id="7561c-178">Aby uzyskać więcej informacji, zobacz <xref:blazor/call-javascript-from-dotnet#blazor-javascript-isolation-and-object-references>.</span><span class="sxs-lookup"><span data-stu-id="7561c-178">For more information, see <xref:blazor/call-javascript-from-dotnet#blazor-javascript-isolation-and-object-references>.</span></span>

### <a name="form-components-support-display-name"></a><span data-ttu-id="7561c-179">Obsługa nazw wyświetlanych przez składniki formularza</span><span class="sxs-lookup"><span data-stu-id="7561c-179">Form components support display name</span></span>

<span data-ttu-id="7561c-180">Następujące wbudowane składniki obsługują nazwy wyświetlane z `DisplayName` parametrem:</span><span class="sxs-lookup"><span data-stu-id="7561c-180">The following built-in components support display names with the `DisplayName` parameter:</span></span>

* `InputDate`
* `InputNumber`
* `InputSelect`

<span data-ttu-id="7561c-181">Aby uzyskać więcej informacji, zobacz <xref:blazor/forms-validation#display-name-support>.</span><span class="sxs-lookup"><span data-stu-id="7561c-181">For more information, see <xref:blazor/forms-validation#display-name-support>.</span></span>

### <a name="catch-all-route-parameters"></a><span data-ttu-id="7561c-182">Catch-wszystkie parametry tras</span><span class="sxs-lookup"><span data-stu-id="7561c-182">Catch-all route parameters</span></span>

<span data-ttu-id="7561c-183">Wszystkie parametry trasy, które przechwytują ścieżki między wieloma granicami folderów, są obsługiwane w składnikach.</span><span class="sxs-lookup"><span data-stu-id="7561c-183">Catch-all route parameters, which capture paths across multiple folder boundaries, are supported in components.</span></span> <span data-ttu-id="7561c-184">Aby uzyskać więcej informacji, zobacz <xref:blazor/fundamentals/routing#catch-all-route-parameters>.</span><span class="sxs-lookup"><span data-stu-id="7561c-184">For more information, see <xref:blazor/fundamentals/routing#catch-all-route-parameters>.</span></span>

### <a name="debugging-improvements"></a><span data-ttu-id="7561c-185">Ulepszenia debugowania</span><span class="sxs-lookup"><span data-stu-id="7561c-185">Debugging improvements</span></span>

<span data-ttu-id="7561c-186">Blazor WebAssemblyUlepszono debugowanie aplikacji w ASP.NET Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="7561c-186">Debugging Blazor WebAssembly apps is improved in ASP.NET Core 5.0.</span></span> <span data-ttu-id="7561c-187">Ponadto debugowanie jest teraz obsługiwane w Visual Studio dla komputerów Mac.</span><span class="sxs-lookup"><span data-stu-id="7561c-187">Additionally, debugging is now supported on Visual Studio for Mac.</span></span> <span data-ttu-id="7561c-188">Aby uzyskać więcej informacji, zobacz <xref:blazor/debug>.</span><span class="sxs-lookup"><span data-stu-id="7561c-188">For more information, see <xref:blazor/debug>.</span></span>

### <a name="microsoft-identity-v20-and-msal-v20"></a><span data-ttu-id="7561c-189">Microsoft Identity v 2.0 i MSAL v 2.0</span><span class="sxs-lookup"><span data-stu-id="7561c-189">Microsoft Identity v2.0 and MSAL v2.0</span></span>

<span data-ttu-id="7561c-190">Blazor zabezpieczenia wykorzystują teraz oprogramowanie Microsoft Identity v 2.0 ( [`Microsoft.Identity.Web`](https://www.nuget.org/packages/Microsoft.Identity.Web) i [`Microsoft.Identity.Web.UI`](https://www.nuget.org/packages/Microsoft.Identity.Web.UI) ) oraz MSAL v 2.0.</span><span class="sxs-lookup"><span data-stu-id="7561c-190">Blazor security now uses Microsoft Identity v2.0 ([`Microsoft.Identity.Web`](https://www.nuget.org/packages/Microsoft.Identity.Web) and [`Microsoft.Identity.Web.UI`](https://www.nuget.org/packages/Microsoft.Identity.Web.UI)) and MSAL v2.0.</span></span> <span data-ttu-id="7561c-191">Aby uzyskać więcej informacji, zobacz tematy w [ Blazor zabezpieczeniach i Identity węźle](xref:blazor/security/index).</span><span class="sxs-lookup"><span data-stu-id="7561c-191">For more information, see the topics in the [Blazor Security and Identity node](xref:blazor/security/index).</span></span>

### <a name="protected-browser-storage-for-blazor-server"></a><span data-ttu-id="7561c-192">Magazyn chronionej przeglądarki dla programu Blazor Server</span><span class="sxs-lookup"><span data-stu-id="7561c-192">Protected Browser Storage for Blazor Server</span></span>

<span data-ttu-id="7561c-193">Blazor Server aplikacje mogą teraz korzystać z wbudowanej obsługi przechowywania stanu aplikacji w przeglądarce chronionej przed naruszeniem przy użyciu ASP.NET Core ochrony danych.</span><span class="sxs-lookup"><span data-stu-id="7561c-193">Blazor Server apps can now use built-in support for storing app state in the browser that has been protected from tampering using ASP.NET Core data protection.</span></span> <span data-ttu-id="7561c-194">Dane mogą być przechowywane w lokalnym magazynie przeglądarki lub w magazynie sesji.</span><span class="sxs-lookup"><span data-stu-id="7561c-194">Data can be stored in either local browser storage or session storage.</span></span> <span data-ttu-id="7561c-195">Aby uzyskać więcej informacji, zobacz <xref:blazor/state-management>.</span><span class="sxs-lookup"><span data-stu-id="7561c-195">For more information, see <xref:blazor/state-management>.</span></span>

### <a name="blazor-webassembly-prerendering"></a><span data-ttu-id="7561c-196">Blazor WebAssembly Renderowanie prerenderingu</span><span class="sxs-lookup"><span data-stu-id="7561c-196">Blazor WebAssembly prerendering</span></span>

<span data-ttu-id="7561c-197">Ulepszono integrację składników między modelami hostingu, a Blazor WebAssembly aplikacje mogą teraz wygenerowania danych wyjściowych na serwerze.</span><span class="sxs-lookup"><span data-stu-id="7561c-197">Component integration is improved across hosting models, and Blazor WebAssembly apps can now prerender output on the server.</span></span> <!-- UNCOMMENT AFTER https://github.com/dotnet/AspNetCore.Docs/pull/19887 MERGES: For more information, see <xref:blazor/components/integrate-components-into-razor-pages-and-mvc-apps> and <xref:mvc/views/tag-helpers/builtin-th/component-tag-helper>. -->

### <a name="trimminglinking-improvements"></a><span data-ttu-id="7561c-198">Ulepszenia przycinania/łączenia</span><span class="sxs-lookup"><span data-stu-id="7561c-198">Trimming/linking improvements</span></span>

<span data-ttu-id="7561c-199">Blazor WebAssembly wykonuje przycinanie/łączenie języka pośredniego (IL) podczas kompilacji, aby przyciąć niepotrzebny kod IL z zestawów wyjściowych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7561c-199">Blazor WebAssembly performs Intermediate Language (IL) trimming/linking during a build to trim unnecessary IL from the app's output assemblies.</span></span> <span data-ttu-id="7561c-200">W wersji ASP.NET Core 5,0 program Blazor WebAssembly wykonuje ulepszone przycinanie z użyciem dodatkowych opcji konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="7561c-200">With the release of ASP.NET Core 5.0, Blazor WebAssembly performs improved trimming with additional configuration options.</span></span> <span data-ttu-id="7561c-201">Aby uzyskać więcej informacji, zobacz <xref:blazor/host-and-deploy/configure-trimmer> i [Opcje przycinania](/dotnet/core/deploying/trimming-options).</span><span class="sxs-lookup"><span data-stu-id="7561c-201">For more information, see <xref:blazor/host-and-deploy/configure-trimmer> and [Trimming options](/dotnet/core/deploying/trimming-options).</span></span>

### <a name="browser-compatibility-analyzer"></a><span data-ttu-id="7561c-202">Analizator zgodności przeglądarki</span><span class="sxs-lookup"><span data-stu-id="7561c-202">Browser compatibility analyzer</span></span>

<span data-ttu-id="7561c-203">Blazor WebAssembly aplikacje są przeznaczone dla całego obszaru powierzchni interfejsu API platformy .NET, ale nie wszystkie interfejsy API platformy .NET są obsługiwane w zestawie webassembly z powodu ograniczeń piaskownicy przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="7561c-203">Blazor WebAssembly apps target the full .NET API surface area, but not all .NET APIs are supported on WebAssembly due to browser sandbox constraints.</span></span> <span data-ttu-id="7561c-204">Nieobsługiwane interfejsy API są zgłaszane <xref:System.PlatformNotSupportedException> podczas uruchamiania w zestawie webassembly.</span><span class="sxs-lookup"><span data-stu-id="7561c-204">Unsupported APIs throw <xref:System.PlatformNotSupportedException> when running on WebAssembly.</span></span> <span data-ttu-id="7561c-205">Analizator zgodności platformy ostrzega dewelopera, gdy aplikacja korzysta z interfejsów API, które nie są obsługiwane przez Platformy docelowe aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7561c-205">A platform compatibility analyzer warns the developer when the app uses APIs that aren't supported by the app's target platforms.</span></span> <span data-ttu-id="7561c-206">Aby uzyskać więcej informacji, zobacz <xref:blazor/components/class-libraries#browser-compatibility-analyzer-for-blazor-webassembly>.</span><span class="sxs-lookup"><span data-stu-id="7561c-206">For more information, see <xref:blazor/components/class-libraries#browser-compatibility-analyzer-for-blazor-webassembly>.</span></span>

### <a name="lazy-load-assemblies"></a><span data-ttu-id="7561c-207">Zestawy ładowania z opóźnieniem</span><span class="sxs-lookup"><span data-stu-id="7561c-207">Lazy load assemblies</span></span>

<span data-ttu-id="7561c-208">Blazor WebAssembly wydajność uruchamiania aplikacji można ulepszyć przez odroczenie ładowania niektórych zestawów aplikacji, dopóki nie są wymagane.</span><span class="sxs-lookup"><span data-stu-id="7561c-208">Blazor WebAssembly app startup performance can be improved by deferring the loading of some application assemblies until they're required.</span></span> <span data-ttu-id="7561c-209">Aby uzyskać więcej informacji, zobacz <xref:blazor/webassembly-lazy-load-assemblies>.</span><span class="sxs-lookup"><span data-stu-id="7561c-209">For more information, see <xref:blazor/webassembly-lazy-load-assemblies>.</span></span>

### <a name="updated-globalization-support"></a><span data-ttu-id="7561c-210">Zaktualizowano obsługę globalizacji</span><span class="sxs-lookup"><span data-stu-id="7561c-210">Updated globalization support</span></span>

<span data-ttu-id="7561c-211">Obsługa globalizacji jest dostępna w Blazor WebAssembly oparciu o składniki międzynarodowe dla standardu Unicode (ICU).</span><span class="sxs-lookup"><span data-stu-id="7561c-211">Globalization support is available for Blazor WebAssembly based on International Components for Unicode (ICU).</span></span> <span data-ttu-id="7561c-212">Aby uzyskać więcej informacji, zobacz <xref:blazor/globalization-localization>.</span><span class="sxs-lookup"><span data-stu-id="7561c-212">For more information, see <xref:blazor/globalization-localization>.</span></span>

## <a name="grpc"></a><span data-ttu-id="7561c-213">gRPC</span><span class="sxs-lookup"><span data-stu-id="7561c-213">gRPC</span></span>

<span data-ttu-id="7561c-214">W programie [gRPC](https://grpc.io/)wprowadzono wiele ulepszeń preformowania.</span><span class="sxs-lookup"><span data-stu-id="7561c-214">Many preformance improvements have been made in [gRPC](https://grpc.io/).</span></span> <span data-ttu-id="7561c-215">Aby uzyskać więcej informacji, zobacz [udoskonalenia wydajności gRPC w programie .NET 5](https://devblogs.microsoft.com/aspnet/grpc-performance-improvements-in-net-5/).</span><span class="sxs-lookup"><span data-stu-id="7561c-215">For more information, see [gRPC performance improvements in .NET 5](https://devblogs.microsoft.com/aspnet/grpc-performance-improvements-in-net-5/).</span></span>

<span data-ttu-id="7561c-216">Więcej informacji gRPC można znaleźć w temacie <xref:grpc/index> .</span><span class="sxs-lookup"><span data-stu-id="7561c-216">For more gRPC information, see <xref:grpc/index>.</span></span>

## SignalR

### <a name="signalr-hub-filters"></a><span data-ttu-id="7561c-217">SignalR Filtry centrum</span><span class="sxs-lookup"><span data-stu-id="7561c-217">SignalR Hub filters</span></span>

<span data-ttu-id="7561c-218">SignalR Filtry centrów o nazwie potoki centrów w ASP.NET SignalR , to funkcja, która umożliwia uruchamianie kodu przed wywołaniem i po wywołaniu metod centrów.</span><span class="sxs-lookup"><span data-stu-id="7561c-218">SignalR Hub filters, called Hub pipelines in ASP.NET SignalR, is a feature that allows code to run before and after Hub methods are called.</span></span> <span data-ttu-id="7561c-219">Uruchamianie kodu przed i po wywołaniu metody Hub jest podobne do sposobu, w jaki oprogramowanie pośredniczące ma możliwość uruchomienia kodu przed i po żądaniu HTTP.</span><span class="sxs-lookup"><span data-stu-id="7561c-219">Running code before and after Hub methods are called is similar to how middleware has the ability to run code before and after an HTTP request.</span></span> <span data-ttu-id="7561c-220">Typowe zastosowania obejmują rejestrowanie, obsługę błędów i walidację argumentów.</span><span class="sxs-lookup"><span data-stu-id="7561c-220">Common uses include logging, error handling, and argument validation.</span></span>

<span data-ttu-id="7561c-221">Aby uzyskać więcej informacji, zobacz [Używanie filtrów centrum w SignalR ASP.NET Core ](xref:signalr/hub-filters).</span><span class="sxs-lookup"><span data-stu-id="7561c-221">For more information, see [Use hub filters in ASP.NET Core SignalR](xref:signalr/hub-filters).</span></span>

### <a name="signalr-parallel-hub-invocations"></a><span data-ttu-id="7561c-222">SignalR równoległe wywołania koncentratora</span><span class="sxs-lookup"><span data-stu-id="7561c-222">SignalR parallel hub invocations</span></span>

<span data-ttu-id="7561c-223">ASP.NET Core SignalR może teraz obsługiwać równoległe wywołania koncentratora.</span><span class="sxs-lookup"><span data-stu-id="7561c-223">ASP.NET Core SignalR is now capable of handling parallel hub invocations.</span></span> <span data-ttu-id="7561c-224">Zachowanie domyślne można zmienić, aby umożliwić klientom wywoływanie więcej niż jednej metody centrum w danej chwili:</span><span class="sxs-lookup"><span data-stu-id="7561c-224">The default behavior can be changed to allow clients to invoke more than one hub method at a time:</span></span>

[!code-csharp[](~/release-notes/sample/StartupSignalRhubs.cs?name=snippet)]

### <a name="added-messagepack-support-in-signalr-java-client"></a><span data-ttu-id="7561c-225">Dodano obsługę MessagePack w SignalR kliencie Java</span><span class="sxs-lookup"><span data-stu-id="7561c-225">Added Messagepack support in SignalR Java client</span></span>

<span data-ttu-id="7561c-226">Nowy pakiet, [com. Microsoft. signaler. MessagePack](https://mvnrepository.com/artifact/com.microsoft.signalr.messagepack), dodaje obsługę MessagePack do SignalR klienta Java.</span><span class="sxs-lookup"><span data-stu-id="7561c-226">A new package, [com.microsoft.signalr.messagepack](https://mvnrepository.com/artifact/com.microsoft.signalr.messagepack), adds MessagePack support to the SignalR Java client.</span></span> <span data-ttu-id="7561c-227">Aby użyć protokołu MessagePack Hub, Dodaj `.withHubProtocol(new MessagePackHubProtocol())` do konstruktora połączeń:</span><span class="sxs-lookup"><span data-stu-id="7561c-227">To use the MessagePack hub protocol, add `.withHubProtocol(new MessagePackHubProtocol())` to the connection builder:</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create(
                           "http://localhost:53353/MyHub")
               .withHubProtocol(new MessagePackHubProtocol())
               .build();
```

<!--
See [Update SignalR code](xref:migration/31-to-50#signalr) for migration instructions.
-->

## Kestrel

* <span data-ttu-id="7561c-228">Punkty końcowe do ponownego ładowania za pośrednictwem konfiguracji: Kestrel mogą wykryć zmiany w konfiguracji przekazane do [ KestrelServerOptions.Configuruj](xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Configure%2A) i usunąć powiązania z istniejących punktów końcowych i powiązać je z nowymi punktami końcowymi bez konieczności ponownego uruchamiania aplikacji, gdy `reloadOnChange` parametr ma wartość `true` .</span><span class="sxs-lookup"><span data-stu-id="7561c-228">Reloadable endpoints via configuration: Kestrel can detect changes to configuration passed to [KestrelServerOptions.Configure](xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Configure%2A) and unbind from existing endpoints and bind to new endpoints without requiring an application restart when the `reloadOnChange` parameter is `true`.</span></span> <span data-ttu-id="7561c-229">Domyślnie w przypadku korzystania z programu <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults%2A> lub <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> program Kestrel tworzy powiązanie z podsekcją konfiguracji [" Kestrel "](https://github.com/dotnet/aspnetcore/blob/7e9e03b70124784b1de5564c573bd65cdaccbfcc/src/DefaultBuilder/src/WebHost.cs#L226) z `reloadOnChange` włączonym.</span><span class="sxs-lookup"><span data-stu-id="7561c-229">By default when using <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults%2A> or <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A>, Kestrel binds to the ["Kestrel"](https://github.com/dotnet/aspnetcore/blob/7e9e03b70124784b1de5564c573bd65cdaccbfcc/src/DefaultBuilder/src/WebHost.cs#L226) configuration subsection with `reloadOnChange` enabled.</span></span> <span data-ttu-id="7561c-230">Aplikacje muszą `reloadOnChange: true` być przekazywane podczas `KestrelServerOptions.Configure` ręcznego wywoływania, aby uzyskać punkty końcowe do ponownego ładowania.</span><span class="sxs-lookup"><span data-stu-id="7561c-230">Apps must pass `reloadOnChange: true` when calling `KestrelServerOptions.Configure` manually to get reloadable endpoints.</span></span>
* <span data-ttu-id="7561c-231">Ulepszenia nagłówków odpowiedzi HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="7561c-231">HTTP/2 response headers improvements.</span></span> <span data-ttu-id="7561c-232">Aby uzyskać więcej informacji, zobacz [ulepszenia wydajności](#performance-improvements) w następnej sekcji.</span><span class="sxs-lookup"><span data-stu-id="7561c-232">For more information, see [Performance improvements](#performance-improvements) in the next section.</span></span>
* <span data-ttu-id="7561c-233">Obsługa dodatkowych typów punktów końcowych w usłudze Sockets transport: Dodawanie do nowego interfejsu API wprowadzonego w systemie, domyślnie przydziały usługi <xref:System.Net.Sockets?displayProperty=nameWithType> Sockets w [Kestrel](xref:fundamentals/servers/kestrel) systemie umożliwiają powiązanie zarówno z istniejącymi dojściami plików, jak i gniazdami domen systemu UNIX</span><span class="sxs-lookup"><span data-stu-id="7561c-233">Support for additional endpoints types in the sockets transport: Adding to the new API introduced in <xref:System.Net.Sockets?displayProperty=nameWithType>, the sockets default transport in [Kestrel](xref:fundamentals/servers/kestrel) allows binding to both existing file handles and Unix domain sockets.</span></span> <span data-ttu-id="7561c-234">Obsługa tworzenia powiązań z istniejącymi dojściami plików umożliwia korzystanie z istniejącej `Systemd` integracji bez konieczności `libuv` transportu.</span><span class="sxs-lookup"><span data-stu-id="7561c-234">Support for binding to existing file handles enables using the existing `Systemd` integration without requiring the `libuv` transport.</span></span>
* <span data-ttu-id="7561c-235">Dekodowanie nagłówków niestandardowych w programie Kestrel : aplikacje mogą określić, które elementy <xref:System.Text.Encoding> mają być używane do interpretacji nagłówków przychodzących na podstawie nazwy nagłówka zamiast domyślnego kodowania UTF-8.</span><span class="sxs-lookup"><span data-stu-id="7561c-235">Custom header decoding in Kestrel: Apps can specify which <xref:System.Text.Encoding> to use to interpret incoming headers based on the header name instead of defaulting to UTF-8.</span></span> <span data-ttu-id="7561c-236">Ustaw</span><span class="sxs-lookup"><span data-stu-id="7561c-236">Set the</span></span> <!--<xref:Microsoft.AspNetCore.Server.Kestrel.KestrelServerOptions.RequestHeaderEncodingSelector> --> <span data-ttu-id="7561c-237">`Microsoft.AspNetCore.Server.Kestrel.KestrelServerOptions.RequestHeaderEncodingSelector` właściwość określająca kodowanie, które ma być używane:</span><span class="sxs-lookup"><span data-stu-id="7561c-237">`Microsoft.AspNetCore.Server.Kestrel.KestrelServerOptions.RequestHeaderEncodingSelector` property to specify which encoding to use:</span></span>
 
  ```csharp
  public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                options.RequestHeaderEncodingSelector = encoding =>
                {
                      return encoding switch
                      {
                          "Host" => System.Text.Encoding.Latin1,
                          _ => System.Text.Encoding.UTF8,
                      };
                };
            });
            webBuilder.UseStartup<Startup>();
        });
  ```

### <a name="kestrel-endpoint-specific-options-via-configuration"></a><span data-ttu-id="7561c-238">Kestrel Opcje specyficzne dla punktu końcowego za pośrednictwem konfiguracji</span><span class="sxs-lookup"><span data-stu-id="7561c-238">Kestrel endpoint-specific options via configuration</span></span>

<span data-ttu-id="7561c-239">Dodano pomoc techniczną do konfigurowania Kestrel opcji specyficznych dla punktu końcowego za pośrednictwem [konfiguracji](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="7561c-239">Support has been added for configuring Kestrel’s endpoint-specific options via [configuration](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="7561c-240">Konfiguracje specyficzne dla punktu końcowego obejmują:</span><span class="sxs-lookup"><span data-stu-id="7561c-240">The endpoint-specific configurations includes the:</span></span>

* <span data-ttu-id="7561c-241">Używane protokoły HTTP</span><span class="sxs-lookup"><span data-stu-id="7561c-241">HTTP protocols used</span></span>
* <span data-ttu-id="7561c-242">Używane protokoły TLS</span><span class="sxs-lookup"><span data-stu-id="7561c-242">TLS protocols used</span></span>
* <span data-ttu-id="7561c-243">Wybrano certyfikat</span><span class="sxs-lookup"><span data-stu-id="7561c-243">Certificate selected</span></span>
* <span data-ttu-id="7561c-244">Tryb certyfikatu klienta</span><span class="sxs-lookup"><span data-stu-id="7561c-244">Client certificate mode</span></span>

<span data-ttu-id="7561c-245">Konfiguracja pozwala określić, który certyfikat jest wybierany w oparciu o określoną nazwę serwera.</span><span class="sxs-lookup"><span data-stu-id="7561c-245">Configuration allows specifying which certificate is selected based on the specified server name.</span></span> <span data-ttu-id="7561c-246">Nazwa serwera jest częścią rozszerzenia Oznaczanie nazwy serwera (SNI) do protokołu TLS wskazanego przez klienta.</span><span class="sxs-lookup"><span data-stu-id="7561c-246">The server name is part of the Server Name Indication (SNI) extension to the TLS protocol as indicated by the client.</span></span> <span data-ttu-id="7561c-247">KestrelKonfiguracja programu obsługuje również prefiks wieloznaczny w nazwie hosta.</span><span class="sxs-lookup"><span data-stu-id="7561c-247">Kestrel's configuration also supports a wildcard prefix in the host name.</span></span>

<span data-ttu-id="7561c-248">Poniższy przykład pokazuje, jak określić konkretny punkt końcowy przy użyciu pliku konfiguracji:</span><span class="sxs-lookup"><span data-stu-id="7561c-248">The following example shows how to specify endpoint-specific using a configuration file:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "EndpointName": {
        "Url": "https://*",
        "Sni": {
          "a.example.org": {
            "Protocols": "Http1AndHttp2",
            "SslProtocols": [ "Tls11", "Tls12"],
            "Certificate": {
              "Path": "testCert.pfx",
              "Password": "testPassword"
            },
            "ClientCertificateMode" : "NoCertificate"
          },
          "*.example.org": {
            "Certificate": {
              "Path": "testCert2.pfx",
              "Password": "testPassword"
            }
          },
          "*": {
            // At least one sub-property needs to exist per
            // SNI section or it cannot be discovered via
            // IConfiguration
            "Protocols": "Http1",
          }
        }
      }
    }
  }
}
```

<span data-ttu-id="7561c-249">Oznaczanie nazwy serwera (SNI) to rozszerzenie TLS służące do dołączania domeny wirtualnej w ramach negocjacji protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="7561c-249">Server Name Indication (SNI) is a TLS extension to include a virtual domain as a part of SSL negotiation.</span></span> <span data-ttu-id="7561c-250">W praktyce oznacza to, że nazwa domeny wirtualnej lub nazwa hosta może służyć do identyfikowania punktu końcowego sieci.</span><span class="sxs-lookup"><span data-stu-id="7561c-250">What this effectively means is that the virtual domain name, or a hostname, can be used to identify the network end point.</span></span>

## <a name="performance-improvements"></a><span data-ttu-id="7561c-251">Usprawnienia wydajności</span><span class="sxs-lookup"><span data-stu-id="7561c-251">Performance improvements</span></span>

### <a name="http2"></a><span data-ttu-id="7561c-252">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="7561c-252">HTTP/2</span></span>

* <span data-ttu-id="7561c-253">Znaczące redukcje alokacji w ścieżce kodu HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="7561c-253">Significant reductions in allocations in the HTTP/2 code path.</span></span>
* <span data-ttu-id="7561c-254">Obsługa [HPack dynamicznej kompresji](https://tools.ietf.org/html/rfc7541) nagłówków odpowiedzi HTTP/2 w programie [Kestrel](xref:fundamentals/servers/kestrel) .</span><span class="sxs-lookup"><span data-stu-id="7561c-254">Support for [HPack dynamic compression](https://tools.ietf.org/html/rfc7541) of HTTP/2 response headers in [Kestrel](xref:fundamentals/servers/kestrel).</span></span> <span data-ttu-id="7561c-255">Aby uzyskać więcej informacji, zobacz [rozmiar tabeli nagłówka](xref:fundamentals/servers/kestrel/options#header-table-size) i [HPACK: dyskretne Killer (funkcja) protokołu HTTP/2](https://blog.cloudflare.com/hpack-the-silent-killer-feature-of-http-2/).</span><span class="sxs-lookup"><span data-stu-id="7561c-255">For more information, see [Header table size](xref:fundamentals/servers/kestrel/options#header-table-size) and [HPACK: the silent killer (feature) of HTTP/2](https://blog.cloudflare.com/hpack-the-silent-killer-feature-of-http-2/).</span></span>
* <span data-ttu-id="7561c-256">Wysyłanie ramek PING protokołu HTTP/2: protokół HTTP/2 ma mechanizm wysyłania ramek PING, aby upewnić się, że połączenie bezczynne jest nadal funkcjonalne.</span><span class="sxs-lookup"><span data-stu-id="7561c-256">Sending HTTP/2 PING frames: HTTP/2 has a mechanism for sending PING frames to ensure an idle connection is still functional.</span></span> <span data-ttu-id="7561c-257">Zapewnienie działającego połączenia jest szczególnie przydatne podczas pracy z długimi strumieniami, które często są bezczynne, ale tylko sporadycznie widzą aktywność, na przykład gRPC strumienie.</span><span class="sxs-lookup"><span data-stu-id="7561c-257">Ensuring a viable connection is especially useful when working with long-lived streams that are often idle but only intermittently see activity, for example, gRPC streams.</span></span> <span data-ttu-id="7561c-258">Aplikacje mogą wysyłać okresowe ramki PING w programie [Kestrel](xref:fundamentals/servers/kestrel) , ustawiając limity dla <xref:Microsoft.AspNetCore.Server.Kestrel.KestrelServerOptions> :</span><span class="sxs-lookup"><span data-stu-id="7561c-258">Apps can send periodic PING frames in [Kestrel](xref:fundamentals/servers/kestrel) by setting limits on <xref:Microsoft.AspNetCore.Server.Kestrel.KestrelServerOptions>:</span></span>

   ```csharp
   public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                options.Limits.Http2.KeepAlivePingInterval =
                                               TimeSpan.FromSeconds(10);
                options.Limits.Http2.KeepAlivePingTimeout =
                                               TimeSpan.FromSeconds(1);
            });
            webBuilder.UseStartup<Startup>();
        });
   ```
   <!-- review: KeepAlivePingInterval not found in RC1. Try testing with RC1. See https://github.com/dotnet/aspnetcore/pull/22565/files see C:/Users/riande/source/repos/WebApplication128/WebApplication128 -->

### <a name="containers"></a><span data-ttu-id="7561c-259">Kontenery</span><span class="sxs-lookup"><span data-stu-id="7561c-259">Containers</span></span>

<span data-ttu-id="7561c-260">Przed rozpoczęciem programu .NET 5,0, kompilowanie i publikowanie *pliku dockerfile* dla aplikacji ASP.NET Core wymaga pobrania całego zestaw .NET Core SDK i ASP.NET Core obrazu.</span><span class="sxs-lookup"><span data-stu-id="7561c-260">Prior to .NET 5.0, building and publishing a *Dockerfile* for an ASP.NET Core app required pulling the entire .NET Core SDK and the ASP.NET Core image.</span></span> <span data-ttu-id="7561c-261">W tej wersji jest zmniejszana liczba bajtów obrazów zestawu SDK, a bajty pobierane dla ASP.NET Core obrazu są w znacznym stopniu eliminowane.</span><span class="sxs-lookup"><span data-stu-id="7561c-261">With this release, pulling the SDK images bytes is reduced and the bytes pulled for the ASP.NET Core image is largely eliminated.</span></span> <span data-ttu-id="7561c-262">Aby uzyskać więcej informacji, zobacz [ten komentarz dotyczący problemu](https://github.com/dotnet/dotnet-docker/issues/1814#issuecomment-625294750)w serwisie GitHub.</span><span class="sxs-lookup"><span data-stu-id="7561c-262">For more information, see [this GitHub issue comment](https://github.com/dotnet/dotnet-docker/issues/1814#issuecomment-625294750).</span></span>

## <a name="authentication-and-authorization"></a><span data-ttu-id="7561c-263">Uwierzytelnianie i autoryzacja</span><span class="sxs-lookup"><span data-stu-id="7561c-263">Authentication and authorization</span></span>

### <a name="azure-active-directory-authentication-with-microsoftidentityweb"></a><span data-ttu-id="7561c-264">Azure Active Directory uwierzytelnianie z firmą Microsoft. Identity . Witrynę</span><span class="sxs-lookup"><span data-stu-id="7561c-264">Azure Active Directory authentication with Microsoft.Identity.Web</span></span>

<span data-ttu-id="7561c-265">Szablony projektów ASP.NET Core teraz integrują się z usługą <xref:Microsoft.Identity.Web?displayProperty=fullName> w celu obsługi uwierzytelniania przy użyciu usługi [Azure Active Directory](/azure/active-directory/fundamentals/active-directory-whatis) (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7561c-265">The ASP.NET Core project templates now integrate with <xref:Microsoft.Identity.Web?displayProperty=fullName> to handle authentication with [Azure Active Directory](/azure/active-directory/fundamentals/active-directory-whatis) (Azure AD).</span></span> <span data-ttu-id="7561c-266">[Microsoft. Identity .. Pakiet internetowy](https://www.nuget.org/packages/Microsoft.Identity.Web/) oferuje następujące informacje:</span><span class="sxs-lookup"><span data-stu-id="7561c-266">The [Microsoft.Identity.Web package](https://www.nuget.org/packages/Microsoft.Identity.Web/) provides:</span></span>

* <span data-ttu-id="7561c-267">Lepsze środowisko uwierzytelniania za pomocą usługi Azure AD.</span><span class="sxs-lookup"><span data-stu-id="7561c-267">A better experience for authentication through Azure AD.</span></span>
* <span data-ttu-id="7561c-268">Łatwiejszy sposób uzyskiwania dostępu do zasobów platformy Azure w imieniu użytkowników, w tym [Microsoft Graph](/graph/overview).</span><span class="sxs-lookup"><span data-stu-id="7561c-268">An easier way to access Azure resources on behalf of your users, including [Microsoft Graph](/graph/overview).</span></span> <span data-ttu-id="7561c-269">Zobacz [Microsoft. Identity .. Przykład sieci Web](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2), który rozpoczyna się od podstawowej nazwy logowania i przechodzi przez wiele dzierżawców przy użyciu interfejsów API platformy Azure, przy użyciu Microsoft Graph i ochrony własnych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="7561c-269">See the [Microsoft.Identity.Web sample](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2), which starts with a basic login and advances through multi-tenancy, using Azure APIs, using Microsoft Graph, and protecting your own APIs.</span></span> <span data-ttu-id="7561c-270">`Microsoft.Identity.Web` jest dostępny razem z platformą .NET 5.</span><span class="sxs-lookup"><span data-stu-id="7561c-270">`Microsoft.Identity.Web` is available alongside .NET 5.</span></span>

### <a name="allow-anonymous-access-to-an-endpoint"></a><span data-ttu-id="7561c-271">Zezwalanie na dostęp anonimowy do punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="7561c-271">Allow anonymous access to an endpoint</span></span>

<span data-ttu-id="7561c-272">`AllowAnonymous`Metoda rozszerzenia umożliwia anonimowy dostęp do punktu końcowego:</span><span class="sxs-lookup"><span data-stu-id="7561c-272">The `AllowAnonymous` extension method allows anonymous access to an endpoint:</span></span>

[!code-csharp[](~/release-notes/sample/StartupAnonEndpoint.cs?name=snippet)]

### <a name="custom-handling-of-authorization-failures"></a><span data-ttu-id="7561c-273">Niestandardowa obsługa niepowodzeń autoryzacji</span><span class="sxs-lookup"><span data-stu-id="7561c-273">Custom handling of authorization failures</span></span>

<span data-ttu-id="7561c-274">Niestandardowa obsługa niepowodzeń autoryzacji jest teraz łatwiejsza przy użyciu nowego interfejsu [IAuthorizationMiddlewareResultHandler](https://github.com/dotnet/aspnetcore/blob/v5.0.0-rc.1.20451.17/src/Security/Authorization/Policy/src/IAuthorizationMiddlewareResultHandler.cs) , który jest wywoływany przez [oprogramowanie pośredniczące](xref:fundamentals/middleware/index) [autoryzacji](xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization%2A) .</span><span class="sxs-lookup"><span data-stu-id="7561c-274">Custom handling of authorization failures is now easier with the new [IAuthorizationMiddlewareResultHandler](https://github.com/dotnet/aspnetcore/blob/v5.0.0-rc.1.20451.17/src/Security/Authorization/Policy/src/IAuthorizationMiddlewareResultHandler.cs) interface that is invoked by the [authorization](xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization%2A) [Middleware](xref:fundamentals/middleware/index).</span></span> <span data-ttu-id="7561c-275">Domyślna implementacja pozostaje taka sama, ale obsługa niestandardowa może być zarejestrowana w [iniekcja zależności, która umożliwia niestandardowe odpowiedzi HTTP na podstawie przyczyny niepowodzenia autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="7561c-275">The default implementation remains the same, but a custom handler can be registered in [Dependency injection, which allows custom HTTP responses based on why authorization failed.</span></span> <span data-ttu-id="7561c-276">Zapoznaj się z [tym przykładem](https://github.com/dotnet/aspnetcore/blob/main/src/Security/samples/CustomAuthorizationFailureResponse/Authorization/SampleAuthorizationMiddlewareResultHandler.cs) , który pokazuje użycie `IAuthorizationMiddlewareResultHandler` .</span><span class="sxs-lookup"><span data-stu-id="7561c-276">See [this sample](https://github.com/dotnet/aspnetcore/blob/main/src/Security/samples/CustomAuthorizationFailureResponse/Authorization/SampleAuthorizationMiddlewareResultHandler.cs) that demonstrates usage of the `IAuthorizationMiddlewareResultHandler`.</span></span>

### <a name="authorization-when-using-endpoint-routing"></a><span data-ttu-id="7561c-277">Autoryzacja w przypadku korzystania z routingu punktu końcowego</span><span class="sxs-lookup"><span data-stu-id="7561c-277">Authorization when using endpoint routing</span></span>

<span data-ttu-id="7561c-278">Autoryzacja w przypadku korzystania z usługi Routing punktów końcowych otrzymuje teraz `HttpContext` zamiast wystąpienia punktu końcowego.</span><span class="sxs-lookup"><span data-stu-id="7561c-278">Authorization when using endpoint routing now receives the `HttpContext` rather than the endpoint instance.</span></span> <span data-ttu-id="7561c-279">Dzięki temu oprogramowanie pośredniczące autoryzacji uzyskuje dostęp do `RouteData` i innych właściwości `HttpContext` , które były niedostępne, chociaż Klasa jest niedostępna <xref:Microsoft.AspNetCore.Http.Endpoint> .</span><span class="sxs-lookup"><span data-stu-id="7561c-279">This allows the authorization middleware to access the `RouteData` and other properties of the `HttpContext` that were not accessible though the <xref:Microsoft.AspNetCore.Http.Endpoint> class.</span></span> <span data-ttu-id="7561c-280">Punkt końcowy można pobrać z kontekstu przy użyciu [kontekstu. GetEndPoint](xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint%2A).</span><span class="sxs-lookup"><span data-stu-id="7561c-280">The endpoint can be fetched from the context using [context.GetEndpoint](xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint%2A).</span></span>

### <a name="role-based-access-control-with-kerberos-authentication-and-ldap-on-linux"></a><span data-ttu-id="7561c-281">Kontrola dostępu oparta na rolach z uwierzytelnianiem Kerberos i LDAP w systemie Linux</span><span class="sxs-lookup"><span data-stu-id="7561c-281">Role-based access control with Kerberos authentication and LDAP on Linux</span></span>

<span data-ttu-id="7561c-282">Zobacz [uwierzytelnianie Kerberos i kontrola dostępu oparta na rolach (RBAC)](xref:security/authentication/windowsauth#rbac)</span><span class="sxs-lookup"><span data-stu-id="7561c-282">See [Kerberos authentication and role-based access control (RBAC)](xref:security/authentication/windowsauth#rbac)</span></span>

## <a name="api-improvements"></a><span data-ttu-id="7561c-283">Udoskonalenia interfejsu API</span><span class="sxs-lookup"><span data-stu-id="7561c-283">API improvements</span></span>

### <a name="json-extension-methods-for-httprequest-and-httpresponse"></a><span data-ttu-id="7561c-284">Metody rozszerzenia JSON dla elementu HttpRequest i HttpResponse</span><span class="sxs-lookup"><span data-stu-id="7561c-284">JSON extension methods for HttpRequest and HttpResponse</span></span>

<span data-ttu-id="7561c-285">Dane JSON mogą być odczytywane i zapisywane przy `HttpRequest` `HttpResponse` użyciu <xref:System.Net.Http.Json.HttpContentJsonExtensions.ReadFromJsonAsync%2A> metod New i `WriteAsJsonAsync` Extension.</span><span class="sxs-lookup"><span data-stu-id="7561c-285">JSON data can be read and written to from an `HttpRequest` and `HttpResponse` using the new <xref:System.Net.Http.Json.HttpContentJsonExtensions.ReadFromJsonAsync%2A> and `WriteAsJsonAsync` extension methods.</span></span> <span data-ttu-id="7561c-286">Te metody rozszerzające używają [System.Text.Jsna](xref:System.Text.Json) serializatorze do obsługi danych JSON.</span><span class="sxs-lookup"><span data-stu-id="7561c-286">These extension methods use the [System.Text.Json](xref:System.Text.Json) serializer to handle the JSON data.</span></span> <span data-ttu-id="7561c-287">Nowa `HasJsonContentType` Metoda rozszerzenia może również sprawdzić, czy żądanie ma typ zawartości JSON.</span><span class="sxs-lookup"><span data-stu-id="7561c-287">The new `HasJsonContentType` extension method can also check if a request has a JSON content type.</span></span>

<span data-ttu-id="7561c-288">Metody rozszerzenia JSON można łączyć z [routingiem punktów końcowych](xref:fundamentals/routing) w celu utworzenia interfejsów API JSON w stylu programowania wywołującego ***trasę do kodu***.</span><span class="sxs-lookup"><span data-stu-id="7561c-288">The JSON extension methods can be combined with [endpoint routing](xref:fundamentals/routing) to create JSON APIs in a style of programming we call ***route to code***.</span></span> <span data-ttu-id="7561c-289">Jest to nowa opcja dla deweloperów, którzy chcą w prosty sposób tworzyć podstawowe interfejsy API JSON.</span><span class="sxs-lookup"><span data-stu-id="7561c-289">It is a new option for developers who want to create basic JSON APIs in a lightweight way.</span></span> <span data-ttu-id="7561c-290">Na przykład aplikacja sieci Web, która ma tylko kilku punktów końcowych, może wybrać użycie trasy do kodu, a nie pełnej funkcjonalności ASP.NET Core MVC:</span><span class="sxs-lookup"><span data-stu-id="7561c-290">For example, a web app that has only a handful of endpoints might choose to use route to code rather than the full functionality of ASP.NET Core MVC:</span></span>

```csharp
endpoints.MapGet("/weather/{city:alpha}", async context =>
{
    var city = (string)context.Request.RouteValues["city"];
    var weather = GetFromDatabase(city);

    await context.Response.WriteAsJsonAsync(weather);
});
```

<span data-ttu-id="7561c-291">Aby uzyskać więcej informacji na temat nowych metod rozszerzenia JSON i trasy do kodu, zobacz [ten program .NET show](https://channel9.msdn.com/Shows/On-NET/ASPNET-Core-Series-Route-to-Code).</span><span class="sxs-lookup"><span data-stu-id="7561c-291">For more information on the new JSON extension methods and route to code, see [this .NET show](https://channel9.msdn.com/Shows/On-NET/ASPNET-Core-Series-Route-to-Code).</span></span>

### <a name="systemdiagnosticsactivity"></a><span data-ttu-id="7561c-292">System. Diagnostics. Activity</span><span class="sxs-lookup"><span data-stu-id="7561c-292">System.Diagnostics.Activity</span></span>

<span data-ttu-id="7561c-293">Domyślny format dla <xref:System.Diagnostics.Activity?displayProperty=fullName> teraz domyślnie jest FORMATEM W3C.</span><span class="sxs-lookup"><span data-stu-id="7561c-293">The default format for <xref:System.Diagnostics.Activity?displayProperty=fullName> now defaults to the W3C format.</span></span> <span data-ttu-id="7561c-294">Zapewnia to obsługę śledzenia rozproszonego w ASP.NET Core współdziałanie z większą liczbą platform.</span><span class="sxs-lookup"><span data-stu-id="7561c-294">This makes distributed tracing support in ASP.NET Core interoperable with more frameworks by default.</span></span>

### <a name="frombodyattribute"></a><span data-ttu-id="7561c-295">FromBodyAttribute</span><span class="sxs-lookup"><span data-stu-id="7561c-295">FromBodyAttribute</span></span>

<span data-ttu-id="7561c-296"><xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute> obsługuje teraz Konfigurowanie opcji, która umożliwia określenie parametrów lub właściwości jako opcjonalnych:</span><span class="sxs-lookup"><span data-stu-id="7561c-296"><xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute> now supports configuring an option that allows these parameters or properties to be considered optional:</span></span>

```csharp
public IActionResult Post([FromBody(EmptyBodyBehavior = EmptyBodyBehavior.Allow)]
                          MyModel model)
{
    ...
}
```

## <a name="miscellaneous-improvements"></a><span data-ttu-id="7561c-297">Różne ulepszenia</span><span class="sxs-lookup"><span data-stu-id="7561c-297">Miscellaneous improvements</span></span>

<span data-ttu-id="7561c-298">Rozpoczęto stosowanie [adnotacji dopuszczającej wartości null](/dotnet/csharp/nullable-references#attributes-describe-apis) do zestawów ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7561c-298">We’ve started applying [nullable annotations](/dotnet/csharp/nullable-references#attributes-describe-apis) to ASP.NET Core assemblies.</span></span> <span data-ttu-id="7561c-299">Firma Microsoft planuje Dodawanie adnotacji do najbardziej typowej publicznej powierzchni interfejsu API platformy .NET 5.</span><span class="sxs-lookup"><span data-stu-id="7561c-299">We plan to annotate most of the common public API surface of the .NET 5 framework.</span></span> <!-- Review: what's the impact of this? How does it work? Need more info.  Check the link I added -->

### <a name="control-startup-class-activation"></a><span data-ttu-id="7561c-300">Kontrola aktywacji klasy uruchamiania</span><span class="sxs-lookup"><span data-stu-id="7561c-300">Control Startup class activation</span></span>

<span data-ttu-id="7561c-301">Dodano dodatkowe <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseStartup%2A> Przeciążenie, które umożliwia aplikacji dostarczenie metody fabryki do kontrolowania `Startup` aktywacji klasy.</span><span class="sxs-lookup"><span data-stu-id="7561c-301">An additional <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseStartup%2A> overload has been added that lets an app provide a factory method for controlling `Startup` class activation.</span></span> <span data-ttu-id="7561c-302">Kontrolowanie `Startup` aktywacji klasy jest przydatne do przekazywania dodatkowych parametrów do `Startup` , które są inicjowane wraz z hostem:</span><span class="sxs-lookup"><span data-stu-id="7561c-302">Controlling `Startup` class activation is useful to pass additional parameters to `Startup` that are initialized along with the host:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var logger = CreateLogger();
        var host = Host.CreateDefaultBuilder()
            .ConfigureWebHost(builder =>
            {
                builder.UseStartup(context => new Startup(logger));
            })
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="auto-refresh-with-dotnet-watch"></a><span data-ttu-id="7561c-303">Autoodświeżanie za pomocą czujki dotnet</span><span class="sxs-lookup"><span data-stu-id="7561c-303">Auto refresh with dotnet watch</span></span>

<span data-ttu-id="7561c-304">W programie .NET 5 uruchomienie [czujka dotnet](xref:tutorials/dotnet-watch) w projekcie ASP.NET Core powoduje uruchomienie domyślnej przeglądarki i autoodowieżanie przeglądarki w miarę wprowadzania zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="7561c-304">In .NET 5, running [dotnet watch](xref:tutorials/dotnet-watch) on an ASP.NET Core project both launches the default browser and auto refreshes the browser as changes are made to the code.</span></span> <span data-ttu-id="7561c-305">Oznacza to, że możesz:</span><span class="sxs-lookup"><span data-stu-id="7561c-305">This means you can:</span></span>

* <span data-ttu-id="7561c-306">Otwórz projekt ASP.NET Core w edytorze tekstu.</span><span class="sxs-lookup"><span data-stu-id="7561c-306">Open an ASP.NET Core project in a text editor.</span></span>
* <span data-ttu-id="7561c-307">Uruchom polecenie `dotnet watch`.</span><span class="sxs-lookup"><span data-stu-id="7561c-307">Run `dotnet watch`.</span></span>
* <span data-ttu-id="7561c-308">Należy skoncentrować się na zmianach kodu, gdy narzędzia obsługują ponowne kompilowanie, ponowny Start i ładowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7561c-308">Focus on the code changes while the tooling handles rebuilding, restarting, and reloading the app.</span></span>

### <a name="console-logger-formatter"></a><span data-ttu-id="7561c-309">Program formatujący rejestratora konsoli</span><span class="sxs-lookup"><span data-stu-id="7561c-309">Console Logger Formatter</span></span>

<span data-ttu-id="7561c-310">Wprowadzono ulepszenia dostawcy dziennika konsoli w `Microsoft.Extensions.Logging` bibliotece programu.</span><span class="sxs-lookup"><span data-stu-id="7561c-310">Improvements have been made to the console log provider in the `Microsoft.Extensions.Logging` library.</span></span> <span data-ttu-id="7561c-311">Deweloperzy mogą teraz zaimplementować niestandardowe, `ConsoleFormatter` Aby uzyskać pełną kontrolę nad formatowaniem i kolorem danych wyjściowych konsoli.</span><span class="sxs-lookup"><span data-stu-id="7561c-311">Developers can now implement a custom `ConsoleFormatter` to exercise complete control over formatting and colorization of the console output.</span></span> <span data-ttu-id="7561c-312">Interfejsy API programu formatującego umożliwiają Zaawansowane formatowanie przez implementację podzestawu sekwencji unikowych VT-100.</span><span class="sxs-lookup"><span data-stu-id="7561c-312">The formatter APIs allow for rich formatting by implementing a subset of the VT-100 escape sequences.</span></span> <span data-ttu-id="7561c-313">Emulator VT-100 jest obsługiwany przez większość nowoczesnych terminali.</span><span class="sxs-lookup"><span data-stu-id="7561c-313">VT-100 is supported by most modern terminals.</span></span> <span data-ttu-id="7561c-314">Rejestrator konsoli może analizować sekwencje ucieczki dla nieobsługiwanych terminali, umożliwiając deweloperom tworzenie pojedynczego programu formatującego dla wszystkich terminali.</span><span class="sxs-lookup"><span data-stu-id="7561c-314">The console logger can parse out escape sequences on unsupported terminals allowing developers to author a single formatter for all terminals.</span></span>

### <a name="json-console-logger"></a><span data-ttu-id="7561c-315">Rejestrator konsoli JSON</span><span class="sxs-lookup"><span data-stu-id="7561c-315">JSON Console Logger</span></span>

<span data-ttu-id="7561c-316">Oprócz obsługi niestandardowych elementów formatujących dodano również wbudowany program formatujący JSON, który emituje uporządkowane dzienniki JSON do konsoli programu.</span><span class="sxs-lookup"><span data-stu-id="7561c-316">In addition to support for custom formatters, we’ve also added a built-in JSON formatter that emits structured JSON logs to the console.</span></span> <span data-ttu-id="7561c-317">Poniższy kod przedstawia sposób przełączania z domyślnego rejestratora do formatu JSON:</span><span class="sxs-lookup"><span data-stu-id="7561c-317">The following code shows how to switch from the default logger to JSON:</span></span>

[!code-csharp[](~/release-notes/sample/ProgramJsonLog.cs?name=snippet)]

<span data-ttu-id="7561c-318">Komunikaty dziennika emitowane do konsoli są sformatowane w formacie JSON:</span><span class="sxs-lookup"><span data-stu-id="7561c-318">Log messages emitted to the console are JSON formatted:</span></span>

```json
{
  "EventId": 0,
  "LogLevel": "Information",
  "Category": "Microsoft.Hosting.Lifetime",
  "Message": "Now listening on: https://localhost:5001",
  "State": {
    "Message": "Now listening on: https://localhost:5001",
    "address": "https://localhost:5001",
    "{OriginalFormat}": "Now listening on: {address}"
  }
}
```
