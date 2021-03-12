---
title: Niestandardowe elementy formatujące w ASP.NET Core Web API
author: rick-anderson
description: Dowiedz się, jak tworzyć i używać niestandardowych elementów formatujących dla interfejsów API sieci Web w programie ASP.NET Core.
ms.author: riande
ms.date: 06/25/2020
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
uid: web-api/advanced/custom-formatters
ms.openlocfilehash: 91c9c6513d7c8df671e283508ecc276768d79539
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102587830"
---
# <a name="custom-formatters-in-aspnet-core-web-api"></a><span data-ttu-id="e89d2-103">Niestandardowe elementy formatujące w ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="e89d2-103">Custom formatters in ASP.NET Core Web API</span></span>

<span data-ttu-id="e89d2-104">[Kirka Larkin](https://twitter.com/serpent5) i [Tomasz Dykstra](https://github.com/tdykstra).</span><span class="sxs-lookup"><span data-stu-id="e89d2-104">By [Kirk Larkin](https://twitter.com/serpent5) and [Tom Dykstra](https://github.com/tdykstra).</span></span>

<span data-ttu-id="e89d2-105">ASP.NET Core MVC obsługuje wymianę danych w interfejsach API sieci Web przy użyciu danych wejściowych i wyjściowych.</span><span class="sxs-lookup"><span data-stu-id="e89d2-105">ASP.NET Core MVC supports data exchange in Web APIs using input and output formatters.</span></span> <span data-ttu-id="e89d2-106">Wejściowe elementy formatującego są używane przez [powiązanie modelu](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="e89d2-106">Input formatters are used by [Model Binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="e89d2-107">Wyjściowe elementy formatujące są używane do [formatowania odpowiedzi](xref:web-api/advanced/formatting).</span><span class="sxs-lookup"><span data-stu-id="e89d2-107">Output formatters are used to [format responses](xref:web-api/advanced/formatting).</span></span>

<span data-ttu-id="e89d2-108">Struktura zawiera wbudowane, wejściowe i wyjściowe elementy formatujące dla JSON i XML.</span><span class="sxs-lookup"><span data-stu-id="e89d2-108">The framework provides built-in input and output formatters for JSON and XML.</span></span> <span data-ttu-id="e89d2-109">Udostępnia wbudowany program formatujący dane wyjściowe dla zwykłego tekstu, ale nie udostępnia wejściowego programu formatującego dla zwykłego tekstu.</span><span class="sxs-lookup"><span data-stu-id="e89d2-109">It provides a built-in output formatter for plain text, but doesn't provide an input formatter for plain text.</span></span>

<span data-ttu-id="e89d2-110">W tym artykule pokazano, jak dodać obsługę dodatkowych formatów, tworząc niestandardowe elementy formatujące.</span><span class="sxs-lookup"><span data-stu-id="e89d2-110">This article shows how to add support for additional formats by creating custom formatters.</span></span> <span data-ttu-id="e89d2-111">Aby zapoznać się z przykładem niestandardowego programu formatującego wprowadzanie tekstu, zobacz [TextPlainInputFormatter](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.Formatters/TextPlainInputFormatter.cs) w witrynie GitHub.</span><span class="sxs-lookup"><span data-stu-id="e89d2-111">For an example of a custom plain text input formatter, see [TextPlainInputFormatter](https://github.com/aspnet/Entropy/blob/master/samples/Mvc.Formatters/TextPlainInputFormatter.cs) on GitHub.</span></span>

<span data-ttu-id="e89d2-112">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/web-api/advanced/custom-formatters/samples) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e89d2-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/web-api/advanced/custom-formatters/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-custom-formatters"></a><span data-ttu-id="e89d2-113">Kiedy używać niestandardowych elementów formatujących</span><span class="sxs-lookup"><span data-stu-id="e89d2-113">When to use custom formatters</span></span>

<span data-ttu-id="e89d2-114">Użyj niestandardowego programu formatującego, aby dodać obsługę typu zawartości, który nie jest obsługiwany przez wbudowane elementy formatujące.</span><span class="sxs-lookup"><span data-stu-id="e89d2-114">Use a custom formatter to add support for a content type that isn't handled by the built-in formatters.</span></span>

## <a name="overview-of-how-to-use-a-custom-formatter"></a><span data-ttu-id="e89d2-115">Omówienie korzystania z niestandardowego programu formatującego</span><span class="sxs-lookup"><span data-stu-id="e89d2-115">Overview of how to use a custom formatter</span></span>

<span data-ttu-id="e89d2-116">Aby utworzyć niestandardowy program formatujący:</span><span class="sxs-lookup"><span data-stu-id="e89d2-116">To create a custom formatter:</span></span>

* <span data-ttu-id="e89d2-117">W przypadku serializowania danych wysyłanych do klienta Utwórz klasę wyjściowego programu formatującego.</span><span class="sxs-lookup"><span data-stu-id="e89d2-117">For serializing data sent to the client, create an output formatter class.</span></span>
* <span data-ttu-id="e89d2-118">W przypadku deserializacji danych otrzymanych od klienta Utwórz klasę danych wejściowych programu formatującego.</span><span class="sxs-lookup"><span data-stu-id="e89d2-118">For deserializing data received from the client, create an input formatter class.</span></span>
* <span data-ttu-id="e89d2-119">Dodaj wystąpienia klas programu formatującego do `InputFormatters` kolekcji i `OutputFormatters` w <xref:Microsoft.AspNetCore.Mvc.MvcOptions> .</span><span class="sxs-lookup"><span data-stu-id="e89d2-119">Add instances of formatter classes to the `InputFormatters` and `OutputFormatters` collections in <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span></span>

## <a name="how-to-create-a-custom-formatter-class"></a><span data-ttu-id="e89d2-120">Jak utworzyć niestandardową klasę programu formatującego</span><span class="sxs-lookup"><span data-stu-id="e89d2-120">How to create a custom formatter class</span></span>

<span data-ttu-id="e89d2-121">Aby utworzyć program formatujący:</span><span class="sxs-lookup"><span data-stu-id="e89d2-121">To create a formatter:</span></span>

* <span data-ttu-id="e89d2-122">Utwórz klasę z odpowiedniej klasy bazowej.</span><span class="sxs-lookup"><span data-stu-id="e89d2-122">Derive the class from the appropriate base class.</span></span> <span data-ttu-id="e89d2-123">Przykładowa aplikacja pochodzi z <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter> i <xref:Microsoft.AspNetCore.Mvc.Formatters.TextInputFormatter> .</span><span class="sxs-lookup"><span data-stu-id="e89d2-123">The sample app derives from <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter> and <xref:Microsoft.AspNetCore.Mvc.Formatters.TextInputFormatter>.</span></span>
* <span data-ttu-id="e89d2-124">Określ prawidłowe typy nośników i kodowania w konstruktorze.</span><span class="sxs-lookup"><span data-stu-id="e89d2-124">Specify valid media types and encodings in the constructor.</span></span>
* <span data-ttu-id="e89d2-125">Zastąp <xref:Microsoft.AspNetCore.Mvc.Formatters.InputFormatter.CanReadType%2A> <xref:Microsoft.AspNetCore.Mvc.Formatters.OutputFormatter.CanWriteType%2A> metody i.</span><span class="sxs-lookup"><span data-stu-id="e89d2-125">Override the <xref:Microsoft.AspNetCore.Mvc.Formatters.InputFormatter.CanReadType%2A> and <xref:Microsoft.AspNetCore.Mvc.Formatters.OutputFormatter.CanWriteType%2A> methods.</span></span>
* <span data-ttu-id="e89d2-126">Zastąp <xref:Microsoft.AspNetCore.Mvc.Formatters.InputFormatter.ReadRequestBodyAsync%2A> `WriteResponseBodyAsync` metody i.</span><span class="sxs-lookup"><span data-stu-id="e89d2-126">Override the <xref:Microsoft.AspNetCore.Mvc.Formatters.InputFormatter.ReadRequestBodyAsync%2A> and `WriteResponseBodyAsync` methods.</span></span>

<span data-ttu-id="e89d2-127">Poniższy kod przedstawia `VcardOutputFormatter` klasę z [przykładu](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/web-api/advanced/custom-formatters/samples):</span><span class="sxs-lookup"><span data-stu-id="e89d2-127">The following code shows the `VcardOutputFormatter` class from the [sample](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/web-api/advanced/custom-formatters/samples):</span></span>

[!code-csharp[](custom-formatters/samples/3.x/CustomFormattersSample/Formatters/VcardOutputFormatter.cs?name=snippet_Class)]
  
### <a name="derive-from-the-appropriate-base-class"></a><span data-ttu-id="e89d2-128">Pochodny od odpowiedniej klasy bazowej</span><span class="sxs-lookup"><span data-stu-id="e89d2-128">Derive from the appropriate base class</span></span>

<span data-ttu-id="e89d2-129">W przypadku typów multimediów tekstowych (na przykład vCard) pochodzi z <xref:Microsoft.AspNetCore.Mvc.Formatters.TextInputFormatter> <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter> klasy bazowej lub.</span><span class="sxs-lookup"><span data-stu-id="e89d2-129">For text media types (for example, vCard), derive from the <xref:Microsoft.AspNetCore.Mvc.Formatters.TextInputFormatter> or <xref:Microsoft.AspNetCore.Mvc.Formatters.TextOutputFormatter> base class.</span></span>

[!code-csharp[](custom-formatters/samples/3.x/CustomFormattersSample/Formatters/VcardOutputFormatter.cs?name=snippet_ClassDeclaration)]

<span data-ttu-id="e89d2-130">W przypadku typów binarnych pochodzi od <xref:Microsoft.AspNetCore.Mvc.Formatters.InputFormatter> <xref:Microsoft.AspNetCore.Mvc.Formatters.OutputFormatter> klasy bazowej lub.</span><span class="sxs-lookup"><span data-stu-id="e89d2-130">For binary types, derive from the <xref:Microsoft.AspNetCore.Mvc.Formatters.InputFormatter> or <xref:Microsoft.AspNetCore.Mvc.Formatters.OutputFormatter> base class.</span></span>

### <a name="specify-valid-media-types-and-encodings"></a><span data-ttu-id="e89d2-131">Określ prawidłowe typy multimediów i kodowania</span><span class="sxs-lookup"><span data-stu-id="e89d2-131">Specify valid media types and encodings</span></span>

<span data-ttu-id="e89d2-132">W konstruktorze Określ prawidłowe typy nośników i kodowania przez dodanie do `SupportedMediaTypes` `SupportedEncodings` kolekcji i.</span><span class="sxs-lookup"><span data-stu-id="e89d2-132">In the constructor, specify valid media types and encodings by adding to the `SupportedMediaTypes` and `SupportedEncodings` collections.</span></span>

[!code-csharp[](custom-formatters/samples/3.x/CustomFormattersSample/Formatters/VcardOutputFormatter.cs?name=snippet_ctor)]

<span data-ttu-id="e89d2-133">Klasa programu formatującego **nie** może użyć iniekcji konstruktora dla jego zależności.</span><span class="sxs-lookup"><span data-stu-id="e89d2-133">A formatter class can **not** use constructor injection for its dependencies.</span></span> <span data-ttu-id="e89d2-134">Na przykład `ILogger<VcardOutputFormatter>` nie można dodać jako parametru do konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e89d2-134">For example, `ILogger<VcardOutputFormatter>` cannot be added as a parameter to the constructor.</span></span> <span data-ttu-id="e89d2-135">Aby uzyskać dostęp do usług, należy użyć obiektu kontekstu, który jest przesyłany do metod.</span><span class="sxs-lookup"><span data-stu-id="e89d2-135">To access services, use the context object that gets passed in to the methods.</span></span> <span data-ttu-id="e89d2-136">Przykład kodu w tym artykule i [przykład](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/web-api/advanced/custom-formatters/samples) pokazują, jak to zrobić.</span><span class="sxs-lookup"><span data-stu-id="e89d2-136">A code example in this article and the [sample](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/web-api/advanced/custom-formatters/samples) show how to do this.</span></span>

### <a name="override-canreadtype-and-canwritetype"></a><span data-ttu-id="e89d2-137">Przesłoń element overridetype i element unwritetype</span><span class="sxs-lookup"><span data-stu-id="e89d2-137">Override CanReadType and CanWriteType</span></span>

<span data-ttu-id="e89d2-138">Określ typ do deserializacji lub serializacji z, zastępując `CanReadType` `CanWriteType` metody lub.</span><span class="sxs-lookup"><span data-stu-id="e89d2-138">Specify the type to deserialize into or serialize from by overriding the `CanReadType` or `CanWriteType` methods.</span></span> <span data-ttu-id="e89d2-139">Na przykład tworzenie tekstu vCard z `Contact` typu i na odwrót.</span><span class="sxs-lookup"><span data-stu-id="e89d2-139">For example, creating vCard text from a `Contact` type and vice versa.</span></span>

[!code-csharp[](custom-formatters/samples/3.x/CustomFormattersSample/Formatters/VcardOutputFormatter.cs?name=snippet_CanWriteType)]

#### <a name="the-canwriteresult-method"></a><span data-ttu-id="e89d2-140">Metoda CanWriteResult</span><span class="sxs-lookup"><span data-stu-id="e89d2-140">The CanWriteResult method</span></span>

<span data-ttu-id="e89d2-141">W niektórych scenariuszach `CanWriteResult` musi być zastąpiona, a nie `CanWriteType` .</span><span class="sxs-lookup"><span data-stu-id="e89d2-141">In some scenarios, `CanWriteResult` must be overridden rather than `CanWriteType`.</span></span> <span data-ttu-id="e89d2-142">Użyj `CanWriteResult` , jeśli spełnione są następujące warunki:</span><span class="sxs-lookup"><span data-stu-id="e89d2-142">Use `CanWriteResult` if the following conditions are true:</span></span>

* <span data-ttu-id="e89d2-143">Metoda Action zwraca klasę modelu.</span><span class="sxs-lookup"><span data-stu-id="e89d2-143">The action method returns a model class.</span></span>
* <span data-ttu-id="e89d2-144">Istnieją klasy pochodne, które mogą być zwracane w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="e89d2-144">There are derived classes which might be returned at runtime.</span></span>
* <span data-ttu-id="e89d2-145">Klasa pochodna zwrócona przez akcję musi być znana w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="e89d2-145">The derived class returned by the action must be known at runtime.</span></span>

<span data-ttu-id="e89d2-146">Załóżmy na przykład, że metoda działania:</span><span class="sxs-lookup"><span data-stu-id="e89d2-146">For example, suppose the action method:</span></span>

* <span data-ttu-id="e89d2-147">Sygnatura zwraca `Person` Typ.</span><span class="sxs-lookup"><span data-stu-id="e89d2-147">Signature returns a `Person` type.</span></span>
* <span data-ttu-id="e89d2-148">Może zwracać `Student` lub `Instructor` Typ, który pochodzi od `Person` .</span><span class="sxs-lookup"><span data-stu-id="e89d2-148">Can return a `Student` or `Instructor` type that derives from `Person`.</span></span> 

<span data-ttu-id="e89d2-149">Aby program formatujący obsługiwał tylko `Student` obiekty, Sprawdź typ elementu <xref:Microsoft.AspNetCore.Mvc.Formatters.OutputFormatterCanWriteContext.Object> w obiekcie kontekstu udostępnionym `CanWriteResult` metody.</span><span class="sxs-lookup"><span data-stu-id="e89d2-149">For the formatter to handle only `Student` objects, check the type of <xref:Microsoft.AspNetCore.Mvc.Formatters.OutputFormatterCanWriteContext.Object> in the context object provided to the `CanWriteResult` method.</span></span> <span data-ttu-id="e89d2-150">Gdy metoda akcji zwraca `IActionResult` :</span><span class="sxs-lookup"><span data-stu-id="e89d2-150">When the action method returns `IActionResult`:</span></span>

* <span data-ttu-id="e89d2-151">Nie jest konieczne korzystanie z programu `CanWriteResult` .</span><span class="sxs-lookup"><span data-stu-id="e89d2-151">It's not necessary to use `CanWriteResult`.</span></span>
* <span data-ttu-id="e89d2-152">`CanWriteType`Metoda odbiera typ środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="e89d2-152">The `CanWriteType` method receives the runtime type.</span></span>

<a id="read-write"></a>

### <a name="override-readrequestbodyasync-and-writeresponsebodyasync"></a><span data-ttu-id="e89d2-153">Zastąp ReadRequestBodyAsync i WriteResponseBodyAsync</span><span class="sxs-lookup"><span data-stu-id="e89d2-153">Override ReadRequestBodyAsync and WriteResponseBodyAsync</span></span>

<span data-ttu-id="e89d2-154">Deserializacja lub Serializacja jest wykonywana w `ReadRequestBodyAsync` lub `WriteResponseBodyAsync` .</span><span class="sxs-lookup"><span data-stu-id="e89d2-154">Deserialization or serialization is performed in `ReadRequestBodyAsync` or `WriteResponseBodyAsync`.</span></span> <span data-ttu-id="e89d2-155">Poniższy przykład pokazuje, jak uzyskać usługi z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="e89d2-155">The following example shows how to get services from the dependency injection container.</span></span> <span data-ttu-id="e89d2-156">Nie można uzyskać usług z parametrów konstruktora.</span><span class="sxs-lookup"><span data-stu-id="e89d2-156">Services can't be obtained from constructor parameters.</span></span>

[!code-csharp[](custom-formatters/samples/3.x/CustomFormattersSample/Formatters/VcardOutputFormatter.cs?name=snippet_WriteResponseBodyAsync)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a><span data-ttu-id="e89d2-157">Jak skonfigurować MVC do używania niestandardowego programu formatującego</span><span class="sxs-lookup"><span data-stu-id="e89d2-157">How to configure MVC to use a custom formatter</span></span>

<span data-ttu-id="e89d2-158">Aby użyć niestandardowego programu formatującego, Dodaj wystąpienie klasy programu formatującego do `InputFormatters` `OutputFormatters` kolekcji lub.</span><span class="sxs-lookup"><span data-stu-id="e89d2-158">To use a custom formatter, add an instance of the formatter class to the `InputFormatters` or `OutputFormatters` collection.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](custom-formatters/samples/3.x/CustomFormattersSample/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](custom-formatters/samples/2.x/CustomFormattersSample/Startup.cs?name=mvcoptions&highlight=3-4)]

::: moniker-end

<span data-ttu-id="e89d2-159">Elementy formatujące są oceniane w kolejności, w jakiej je wstawiasz.</span><span class="sxs-lookup"><span data-stu-id="e89d2-159">Formatters are evaluated in the order you insert them.</span></span> <span data-ttu-id="e89d2-160">Pierwszeństwo ma pierwszy.</span><span class="sxs-lookup"><span data-stu-id="e89d2-160">The first one takes precedence.</span></span>

## <a name="the-complete-vcardinputformatter-class"></a><span data-ttu-id="e89d2-161">Kompletna `VcardInputFormatter` Klasa</span><span class="sxs-lookup"><span data-stu-id="e89d2-161">The complete `VcardInputFormatter` class</span></span>

<span data-ttu-id="e89d2-162">Poniższy kod przedstawia `VcardInputFormatter` klasę z [przykładu](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/web-api/advanced/custom-formatters/samples):</span><span class="sxs-lookup"><span data-stu-id="e89d2-162">The following code shows the `VcardInputFormatter` class from the [sample](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/web-api/advanced/custom-formatters/samples):</span></span>

[!code-csharp[](custom-formatters/samples/3.x/CustomFormattersSample/Formatters/VcardInputFormatter.cs?name=snippet_Class)]

## <a name="test-the-app"></a><span data-ttu-id="e89d2-163">Testowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="e89d2-163">Test the app</span></span>

<span data-ttu-id="e89d2-164">[Uruchom przykładową aplikację dla tego artykułu](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/web-api/advanced/custom-formatters/samples), która implementuje podstawowe elementy formatujące dane wejściowe i wyjściowe w formacie vCard.</span><span class="sxs-lookup"><span data-stu-id="e89d2-164">[Run the sample app for this article](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/web-api/advanced/custom-formatters/samples), which implements basic vCard input and output formatters.</span></span> <span data-ttu-id="e89d2-165">Aplikacja odczytuje i zapisuje wizytówki vCard podobne do następujących:</span><span class="sxs-lookup"><span data-stu-id="e89d2-165">The app reads and writes vCards similar to the following:</span></span>

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
END:VCARD
```

<span data-ttu-id="e89d2-166">Aby wyświetlić dane wyjściowe w formacie vCard, uruchom aplikację i Wyślij żądanie Get z nagłówkiem Akceptuj `text/vcard` do `https://localhost:5001/api/contacts` .</span><span class="sxs-lookup"><span data-stu-id="e89d2-166">To see vCard output, run the app and send a Get request with Accept header `text/vcard` to `https://localhost:5001/api/contacts`.</span></span>

<span data-ttu-id="e89d2-167">Aby dodać wizytówkę vCard do kolekcji kontaktów w pamięci:</span><span class="sxs-lookup"><span data-stu-id="e89d2-167">To add a vCard to the in-memory collection of contacts:</span></span>

* <span data-ttu-id="e89d2-168">Wyślij `Post` żądanie do programu `/api/contacts` przy użyciu narzędzia, takiego jak Poster.</span><span class="sxs-lookup"><span data-stu-id="e89d2-168">Send a `Post` request to `/api/contacts` with a tool like Postman.</span></span>
* <span data-ttu-id="e89d2-169">Ustaw `Content-Type` nagłówek na `text/vcard` .</span><span class="sxs-lookup"><span data-stu-id="e89d2-169">Set the `Content-Type` header to `text/vcard`.</span></span>
* <span data-ttu-id="e89d2-170">Ustaw `vCard` tekst w treści, tak jak w poprzednim przykładzie.</span><span class="sxs-lookup"><span data-stu-id="e89d2-170">Set `vCard` text in the body, formatted like the preceding example.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e89d2-171">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="e89d2-171">Additional resources</span></span>

* <xref:web-api/advanced/formatting>
* <xref:grpc/dotnet-grpc>
