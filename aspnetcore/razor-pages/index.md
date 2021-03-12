---
title: Wprowadzenie do Razor stron w ASP.NET Core
author: Rick-Anderson
description: Wyjaśnia Razor , jak strony w ASP.NET Core sprawia, że kodowanie scenariuszy ukierunkowanych na strony jest łatwiejsze i wydajniejsze niż używanie MVC.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 02/12/2020
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
uid: razor-pages/index
ms.openlocfilehash: 78b192cb2240046d16b1b766954ed4ca5229d888
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102586712"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="7c52a-103">Wprowadzenie do Razor stron w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c52a-103">Introduction to Razor Pages in ASP.NET Core</span></span>


<span data-ttu-id="7c52a-104">[Rick Anderson](https://twitter.com/RickAndMSFT) i [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="7c52a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="7c52a-105">Razor Strony mogą sprawiać, że kodowanie scenariuszy ukierunkowanych na strony jest łatwiejsze i wydajniejsze niż używanie kontrolerów i widoków.</span><span class="sxs-lookup"><span data-stu-id="7c52a-105">Razor Pages can make coding page-focused scenarios easier and more productive than using controllers and views.</span></span>

<span data-ttu-id="7c52a-106">Jeśli szukasz samouczka korzystającego z podejścia Model-View-Controller, zobacz Wprowadzenie do [ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="7c52a-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="7c52a-107">Ten dokument zawiera wprowadzenie do Razor stron.</span><span class="sxs-lookup"><span data-stu-id="7c52a-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="7c52a-108">Nie jest to samouczek krok po kroku.</span><span class="sxs-lookup"><span data-stu-id="7c52a-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="7c52a-109">Jeśli okaże się, że niektóre sekcje są zbyt zaawansowane, zobacz [wprowadzenie do Razor stron](xref:tutorials/razor-pages/razor-pages-start).</span><span class="sxs-lookup"><span data-stu-id="7c52a-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="7c52a-110">Aby zapoznać się z omówieniem ASP.NET Core, zobacz [wprowadzenie do ASP.NET Core](xref:index).</span><span class="sxs-lookup"><span data-stu-id="7c52a-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c52a-111">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7c52a-111">Prerequisites</span></span>

::: moniker range=">= aspnetcore-3.0 < aspnetcore-5.0"

# <a name="visual-studio"></a>[<span data-ttu-id="7c52a-112">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c52a-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="7c52a-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c52a-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="7c52a-114">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="7c52a-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-5.0"

# <a name="visual-studio"></a>[<span data-ttu-id="7c52a-115">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c52a-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-5.0.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="7c52a-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c52a-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-5.0.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="7c52a-117">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="7c52a-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-5.0.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="7c52a-118">Tworzenie Razor projektu stron</span><span class="sxs-lookup"><span data-stu-id="7c52a-118">Create a Razor Pages project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="7c52a-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c52a-119">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7c52a-120">Aby uzyskać szczegółowe instrukcje dotyczące tworzenia projektu stron, zobacz Wprowadzenie do [ Razor stron](xref:tutorials/razor-pages/razor-pages-start) Razor .</span><span class="sxs-lookup"><span data-stu-id="7c52a-120">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="7c52a-121">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c52a-121">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7c52a-122">Uruchom `dotnet new webapp` polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="7c52a-122">Run `dotnet new webapp` from the command line.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="7c52a-123">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="7c52a-123">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7c52a-124">Aby uzyskać szczegółowe instrukcje dotyczące tworzenia projektu stron, zobacz Wprowadzenie do [ Razor stron](xref:tutorials/razor-pages/razor-pages-start) Razor .</span><span class="sxs-lookup"><span data-stu-id="7c52a-124">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="7c52a-125">Razor Page</span><span class="sxs-lookup"><span data-stu-id="7c52a-125">Razor Pages</span></span>

<span data-ttu-id="7c52a-126">Razor Strony są włączone w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7c52a-126">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-csharp[](index/3.0sample/RazorPagesIntro/Startup.cs?name=snippet_Startup&highlight=12,36)]

<span data-ttu-id="7c52a-127">Weź pod uwagę podstawową stronę: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="7c52a-127">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index.cshtml?highlight=1)]

<span data-ttu-id="7c52a-128">Poprzedni kod wygląda podobnie jak [ Razor plik widoku](xref:tutorials/first-mvc-app/adding-view) używany w aplikacji ASP.NET Core z kontrolerami i widokami.</span><span class="sxs-lookup"><span data-stu-id="7c52a-128">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="7c52a-129">Co sprawia, że jest to [`@page`](xref:mvc/views/razor#page) dyrektywa.</span><span class="sxs-lookup"><span data-stu-id="7c52a-129">What makes it different is the [`@page`](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="7c52a-130">`@page` sprawia, że plik jest akcją MVC, co oznacza, że obsługuje żądania bezpośrednio, bez przechodzenia przez kontroler.</span><span class="sxs-lookup"><span data-stu-id="7c52a-130">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="7c52a-131">`@page` musi być pierwszą Razor dyrektywą na stronie.</span><span class="sxs-lookup"><span data-stu-id="7c52a-131">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="7c52a-132">`@page` wpływa na zachowanie innych [Razor](xref:mvc/views/razor) konstrukcji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-132">`@page` affects the behavior of other [Razor](xref:mvc/views/razor) constructs.</span></span> <span data-ttu-id="7c52a-133">Razor Nazwy plików stron mają sufiks *. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-133">Razor Pages file names have a *.cshtml* suffix.</span></span>

<span data-ttu-id="7c52a-134">Podobna Strona, za pomocą `PageModel` klasy, jest pokazana w następujących dwóch plikach.</span><span class="sxs-lookup"><span data-stu-id="7c52a-134">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="7c52a-135">Plik *Pages/index2. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-135">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="7c52a-136">Model strony *stron/index2. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-136">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-csharp[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="7c52a-137">Zgodnie z Konwencją `PageModel` plik klasy ma taką samą nazwę jak Razor plik stronicowania dołączony do *. cs* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-137">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="7c52a-138">Na przykład Poprzednia Razor strona to *Pages/index2. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7c52a-138">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="7c52a-139">Plik zawierający `PageModel` klasę ma nazwę *Pages/index2. cshtml. cs*.</span><span class="sxs-lookup"><span data-stu-id="7c52a-139">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="7c52a-140">Skojarzenia ścieżek adresów URL ze stronami są określane przez lokalizację strony w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="7c52a-140">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="7c52a-141">W poniższej tabeli przedstawiono Razor ścieżkę strony i pasujący adres URL:</span><span class="sxs-lookup"><span data-stu-id="7c52a-141">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="7c52a-142">Nazwa i ścieżka pliku</span><span class="sxs-lookup"><span data-stu-id="7c52a-142">File name and path</span></span>               | <span data-ttu-id="7c52a-143">pasujący adres URL</span><span class="sxs-lookup"><span data-stu-id="7c52a-143">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="7c52a-144">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-144">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="7c52a-145">`/` lub `/Index`</span><span class="sxs-lookup"><span data-stu-id="7c52a-145">`/` or `/Index`</span></span> |
| <span data-ttu-id="7c52a-146">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-146">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="7c52a-147">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-147">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="7c52a-148">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-148">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="7c52a-149">`/Store` lub `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="7c52a-149">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="7c52a-150">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="7c52a-150">Notes:</span></span>

* <span data-ttu-id="7c52a-151">Środowisko uruchomieniowe domyślnie wyszukuje Razor pliki stron w folderze *Pages* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-151">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="7c52a-152">`Index` jest stroną domyślną, gdy adres URL nie zawiera strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-152">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="7c52a-153">Napisz podstawowy formularz</span><span class="sxs-lookup"><span data-stu-id="7c52a-153">Write a basic form</span></span>

<span data-ttu-id="7c52a-154">Razor Strony są przeznaczone do tworzenia wspólnych wzorców używanych z przeglądarkami sieci Web, które są łatwe do wdrożenia podczas kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-154">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="7c52a-155">[Powiązania modelu](xref:mvc/models/model-binding), [pomocników tagów](xref:mvc/views/tag-helpers/intro)i pomocników HTML same *działają* z właściwościami zdefiniowanymi w Razor klasie Page.</span><span class="sxs-lookup"><span data-stu-id="7c52a-155">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="7c52a-156">Weź pod uwagę stronę implementującą podstawowy formularz "contact us" (kontakt z nami) dla `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="7c52a-156">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="7c52a-157">Przykłady w tym dokumencie `DbContext` są inicjowane w pliku [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/main/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) .</span><span class="sxs-lookup"><span data-stu-id="7c52a-157">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/main/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) file.</span></span>

<span data-ttu-id="7c52a-158">Baza danych w pamięci wymaga `Microsoft.EntityFrameworkCore.InMemory` pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="7c52a-158">The in memory database requires the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

[!code-csharp[](index/3.0sample/RazorPagesContacts/Startup.cs?name=snippet)]

<span data-ttu-id="7c52a-159">Model danych:</span><span class="sxs-lookup"><span data-stu-id="7c52a-159">The data model:</span></span>

[!code-csharp[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="7c52a-160">Kontekst bazy danych:</span><span class="sxs-lookup"><span data-stu-id="7c52a-160">The db context:</span></span>

[!code-csharp[](index/3.0sample/RazorPagesContacts/Data/CustomerDbContext.cs)]

<span data-ttu-id="7c52a-161">Plik widoku *Pages/Create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-161">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="7c52a-162">Model strony *Pages/Create. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-162">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="7c52a-163">Zgodnie z Konwencją `PageModel` Klasa jest wywoływana `<PageName>Model` i znajduje się w tej samej przestrzeni nazw co strona.</span><span class="sxs-lookup"><span data-stu-id="7c52a-163">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="7c52a-164">`PageModel`Klasa umożliwia rozdzielenie logiki strony od jej prezentacji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-164">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="7c52a-165">Definiuje procedury obsługi stron dla żądań wysyłanych do strony oraz dane używane do renderowania strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-165">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="7c52a-166">Ta separacja umożliwia:</span><span class="sxs-lookup"><span data-stu-id="7c52a-166">This separation allows:</span></span>

* <span data-ttu-id="7c52a-167">Zarządzanie zależnościami stron przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7c52a-167">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* [<span data-ttu-id="7c52a-168">Testowanie jednostek</span><span class="sxs-lookup"><span data-stu-id="7c52a-168">Unit testing</span></span>](xref:test/razor-pages-tests)

<span data-ttu-id="7c52a-169">Strona ma `OnPostAsync` *metodę obsługi*, która jest uruchamiana na `POST` żądaniach (gdy użytkownik księguje formularz).</span><span class="sxs-lookup"><span data-stu-id="7c52a-169">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="7c52a-170">Można dodać metody obsługi dla dowolnego zlecenia HTTP.</span><span class="sxs-lookup"><span data-stu-id="7c52a-170">Handler methods for any HTTP verb can be added.</span></span> <span data-ttu-id="7c52a-171">Najczęstsze procedury obsługi to:</span><span class="sxs-lookup"><span data-stu-id="7c52a-171">The most common handlers are:</span></span>

* <span data-ttu-id="7c52a-172">`OnGet` — na potrzeby inicjowania stanu wymaganego dla strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-172">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="7c52a-173">W poprzednim kodzie `OnGet` Metoda wyświetla stronę *onmode. cshtml* Razor .</span><span class="sxs-lookup"><span data-stu-id="7c52a-173">In the preceding code, the `OnGet` method displays the *CreateModel.cshtml* Razor Page.</span></span>
* <span data-ttu-id="7c52a-174">`OnPost` — na potrzeby obsługi przesłanych formularzy.</span><span class="sxs-lookup"><span data-stu-id="7c52a-174">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="7c52a-175">Sufiks nazewnictwa `Async` jest opcjonalny, ale jest często używany zgodnie z konwencją na potrzeby funkcji asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="7c52a-175">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="7c52a-176">Poprzedni kod jest typowy dla Razor stron.</span><span class="sxs-lookup"><span data-stu-id="7c52a-176">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="7c52a-177">Jeśli znasz już aplikacje ASP.NET przy użyciu kontrolerów i widoków:</span><span class="sxs-lookup"><span data-stu-id="7c52a-177">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="7c52a-178">`OnPostAsync`Kod w poprzednim przykładzie wygląda podobnie do typowego kodu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7c52a-178">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="7c52a-179">Większość elementów podstawowych MVC, takich jak [powiązania modelu](xref:mvc/models/model-binding), [Walidacja](xref:mvc/models/validation)i akcje, działa tak samo, jak w przypadku kontrolerów i Razor stron.</span><span class="sxs-lookup"><span data-stu-id="7c52a-179">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results work the same with Controllers and Razor Pages.</span></span> 

<span data-ttu-id="7c52a-180">Poprzednia `OnPostAsync` Metoda:</span><span class="sxs-lookup"><span data-stu-id="7c52a-180">The previous `OnPostAsync` method:</span></span>

[!code-csharp[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="7c52a-181">Podstawowy przepływ `OnPostAsync` :</span><span class="sxs-lookup"><span data-stu-id="7c52a-181">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="7c52a-182">Sprawdź, czy występują błędy walidacji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-182">Check for validation errors.</span></span>

* <span data-ttu-id="7c52a-183">Jeśli nie ma żadnych błędów, Zapisz dane i Przekieruj.</span><span class="sxs-lookup"><span data-stu-id="7c52a-183">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="7c52a-184">W przypadku wystąpienia błędów ponownie Wyświetl stronę z komunikatami walidacji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-184">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="7c52a-185">W wielu przypadkach błędy weryfikacji zostaną wykryte w kliencie i nigdy nie zostaną przesłane do serwera.</span><span class="sxs-lookup"><span data-stu-id="7c52a-185">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="7c52a-186">Plik widoku *Pages/Create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-186">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="7c52a-187">Renderowany kod HTML ze *stron/Create. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7c52a-187">The rendered HTML from *Pages/Create.cshtml*:</span></span>

[!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.html)]

<span data-ttu-id="7c52a-188">W poprzednim kodzie, ogłaszanie formularza:</span><span class="sxs-lookup"><span data-stu-id="7c52a-188">In the previous code, posting the form:</span></span>

* <span data-ttu-id="7c52a-189">Z prawidłowymi danymi:</span><span class="sxs-lookup"><span data-stu-id="7c52a-189">With valid data:</span></span>

  * <span data-ttu-id="7c52a-190">`OnPostAsync`Metoda obsługi wywołuje <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> metodę pomocnika.</span><span class="sxs-lookup"><span data-stu-id="7c52a-190">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper method.</span></span> <span data-ttu-id="7c52a-191">Metoda `RedirectToPage` zwraca wystąpienie klasy <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span><span class="sxs-lookup"><span data-stu-id="7c52a-191">`RedirectToPage` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span></span> <span data-ttu-id="7c52a-192">`RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="7c52a-192">`RedirectToPage`:</span></span>

    * <span data-ttu-id="7c52a-193">Jest wynikiem akcji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-193">Is an action result.</span></span>
    * <span data-ttu-id="7c52a-194">Jest podobny do `RedirectToAction` lub `RedirectToRoute` (używany w kontrolerach i widokach).</span><span class="sxs-lookup"><span data-stu-id="7c52a-194">Is similar to `RedirectToAction` or `RedirectToRoute` (used in controllers and views).</span></span>
    * <span data-ttu-id="7c52a-195">Jest dostosowywany dla stron.</span><span class="sxs-lookup"><span data-stu-id="7c52a-195">Is customized for pages.</span></span> <span data-ttu-id="7c52a-196">W powyższym przykładzie przekierowuje do strony indeksu głównego ( `/Index` ).</span><span class="sxs-lookup"><span data-stu-id="7c52a-196">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="7c52a-197">`RedirectToPage` szczegółowo znajduje się w sekcji [generowanie adresów URL dla stron](#url_gen) .</span><span class="sxs-lookup"><span data-stu-id="7c52a-197">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

* <span data-ttu-id="7c52a-198">Z błędami walidacji, które są przesyłane do serwera:</span><span class="sxs-lookup"><span data-stu-id="7c52a-198">With validation errors that are passed to the server:</span></span>

  * <span data-ttu-id="7c52a-199">`OnPostAsync`Metoda obsługi wywołuje <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> metodę pomocnika.</span><span class="sxs-lookup"><span data-stu-id="7c52a-199">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper method.</span></span> <span data-ttu-id="7c52a-200">Metoda `Page` zwraca wystąpienie klasy <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span><span class="sxs-lookup"><span data-stu-id="7c52a-200">`Page` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span></span> <span data-ttu-id="7c52a-201">Zwracanie `Page` jest podobne do sposobu, w jaki akcje w kontrolerach zwracają `View` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-201">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="7c52a-202">`PageResult` jest domyślnym typem zwracanym dla metody obsługi.</span><span class="sxs-lookup"><span data-stu-id="7c52a-202">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="7c52a-203">Metoda obsługi, która zwraca `void` renderowanie strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-203">A handler method that returns `void` renders the page.</span></span>
  * <span data-ttu-id="7c52a-204">W poprzednim przykładzie, księgowanie formularza bez wartości powoduje, że [ModelState. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) zwraca wartość false.</span><span class="sxs-lookup"><span data-stu-id="7c52a-204">In the preceding example, posting the form with no value results in [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) returning false.</span></span> <span data-ttu-id="7c52a-205">W tym przykładzie na kliencie nie są wyświetlane błędy sprawdzania poprawności.</span><span class="sxs-lookup"><span data-stu-id="7c52a-205">In this sample, no validation errors are displayed on the client.</span></span> <span data-ttu-id="7c52a-206">Przekazywanie błędów sprawdzania poprawności jest omówione w dalszej części tego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-206">Validation error handing is covered later in this document.</span></span>

  [!code-csharp[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

* <span data-ttu-id="7c52a-207">Z błędami walidacji wykrytymi przez weryfikację po stronie klienta:</span><span class="sxs-lookup"><span data-stu-id="7c52a-207">With validation errors detected by client side validation:</span></span>

  * <span data-ttu-id="7c52a-208">Dane **nie** są ogłaszane na serwerze.</span><span class="sxs-lookup"><span data-stu-id="7c52a-208">Data is **not** posted to the server.</span></span>
  * <span data-ttu-id="7c52a-209">Sprawdzanie poprawności po stronie klienta zostało wyjaśnione w dalszej części tego dokumentu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-209">Client-side validation is explained later in this document.</span></span>

<span data-ttu-id="7c52a-210">`Customer`Właściwość używa [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) atrybutu, aby zrezygnować z powiązania modelu:</span><span class="sxs-lookup"><span data-stu-id="7c52a-210">The `Customer` property uses [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute to opt in to model binding:</span></span>

[!code-csharp[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=15-16)]

<span data-ttu-id="7c52a-211">`[BindProperty]`**nie** powinien być używany dla modeli zawierających właściwości, które nie powinny być zmieniane przez klienta.</span><span class="sxs-lookup"><span data-stu-id="7c52a-211">`[BindProperty]` should **not** be used on models containing properties that should not be changed by the client.</span></span> <span data-ttu-id="7c52a-212">Aby uzyskać więcej informacji, zobacz temat [przefinalizowanie](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="7c52a-212">For more information, see [Overposting](xref:data/ef-rp/crud#overposting).</span></span>

<span data-ttu-id="7c52a-213">Razor Domyślnie strony powiążą właściwości tylko z `GET` niezleceniami.</span><span class="sxs-lookup"><span data-stu-id="7c52a-213">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="7c52a-214">Powiązanie z właściwościami eliminuje konieczność pisania kodu w celu przekonwertowania danych HTTP na typ modelu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-214">Binding to properties removes the need to writing code to convert HTTP data to the model type.</span></span> <span data-ttu-id="7c52a-215">Powiązanie zmniejsza kod, używając tej samej właściwości do renderowania pól formularza ( `<input asp-for="Customer.Name">` ) i akceptuję dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="7c52a-215">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="7c52a-216">Przeglądanie pliku widoku *stron/Create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-216">Reviewing the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml?highlight=3,9)]

* <span data-ttu-id="7c52a-217">W poprzednim kodzie [pomocnik tagów wejściowych](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` wiąże `<input>` element HTML z `Customer.Name` wyrażeniem modelu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-217">In the preceding code, the [input tag helper](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` binds the HTML `<input>` element to the `Customer.Name` model expression.</span></span>
* <span data-ttu-id="7c52a-218">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) udostępnia pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="7c52a-218">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) makes Tag Helpers available.</span></span>

### <a name="the-home-page"></a><span data-ttu-id="7c52a-219">Strona główna</span><span class="sxs-lookup"><span data-stu-id="7c52a-219">The home page</span></span>

<span data-ttu-id="7c52a-220">*Index. cshtml* to Strona główna:</span><span class="sxs-lookup"><span data-stu-id="7c52a-220">*Index.cshtml* is the home page:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml)]

<span data-ttu-id="7c52a-221">Skojarzona `PageModel` Klasa (*index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="7c52a-221">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-csharp[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="7c52a-222">Plik *index. cshtml* zawiera następujące znaczniki:</span><span class="sxs-lookup"><span data-stu-id="7c52a-222">The *Index.cshtml* file contains the following markup:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=21)]

<span data-ttu-id="7c52a-223">`<a /a>` [Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) użył `asp-route-{value}` atrybutu w celu wygenerowania linku do strony edycji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-223">The `<a /a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="7c52a-224">Link zawiera dane trasy z IDENTYFIKATORem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-224">The link contains route data with the contact ID.</span></span> <span data-ttu-id="7c52a-225">Na przykład `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="7c52a-225">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="7c52a-226">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) włączają kod po stronie serwera, aby uczestniczyć w tworzeniu i RENDEROWANIU elementów HTML w Razor plikach.</span><span class="sxs-lookup"><span data-stu-id="7c52a-226">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>

<span data-ttu-id="7c52a-227">Plik *index. cshtml* zawiera znaczniki umożliwiające utworzenie przycisku usuwania dla każdego kontaktu z klientem:</span><span class="sxs-lookup"><span data-stu-id="7c52a-227">The *Index.cshtml* file contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=22-23)]

<span data-ttu-id="7c52a-228">Renderowany kod HTML:</span><span class="sxs-lookup"><span data-stu-id="7c52a-228">The rendered HTML:</span></span>

```html
<button type="submit" formaction="/Customers?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="7c52a-229">Gdy przycisk Usuń jest renderowany w języku HTML, jego [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) zawiera parametry dla:</span><span class="sxs-lookup"><span data-stu-id="7c52a-229">When the delete button is rendered in HTML, its [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) includes parameters for:</span></span>

* <span data-ttu-id="7c52a-230">Identyfikator osoby kontaktowej klienta określony przez `asp-route-id` atrybut.</span><span class="sxs-lookup"><span data-stu-id="7c52a-230">The customer contact ID, specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="7c52a-231">`handler`, Określony przez `asp-page-handler` atrybut.</span><span class="sxs-lookup"><span data-stu-id="7c52a-231">The `handler`, specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="7c52a-232">Po wybraniu przycisku `POST` do serwera zostanie wysłane żądanie formularza.</span><span class="sxs-lookup"><span data-stu-id="7c52a-232">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="7c52a-233">Według Konwencji, nazwa metody obsługi jest wybierana na podstawie wartości `handler` parametru zgodnie z schematem `OnPost[handler]Async` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-233">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="7c52a-234">Ponieważ `handler` jest `delete` w tym przykładzie, `OnPostDeleteAsync` Metoda obsługi jest używana do przetwarzania `POST` żądania.</span><span class="sxs-lookup"><span data-stu-id="7c52a-234">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="7c52a-235">Jeśli `asp-page-handler` jest ustawiona na inną wartość, na przykład, jest `remove` wybierana metoda obsługi o nazwie `OnPostRemoveAsync` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-235">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-csharp[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="7c52a-236">`OnPostDeleteAsync`Metoda:</span><span class="sxs-lookup"><span data-stu-id="7c52a-236">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="7c52a-237">Pobiera `id` z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="7c52a-237">Gets the `id` from the query string.</span></span>
* <span data-ttu-id="7c52a-238">Wysyła zapytanie do bazy danych w celu skontaktowania się z klientem za pomocą programu `FindAsync` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-238">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="7c52a-239">Jeśli kontakt z klientem zostanie znaleziony, zostanie on usunięty, a baza danych zostanie zaktualizowana.</span><span class="sxs-lookup"><span data-stu-id="7c52a-239">If the customer contact is found, it's removed and the database is updated.</span></span>
* <span data-ttu-id="7c52a-240">Wywołania <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> do przekierowania na stronę indeksu głównego ( `/Index` ).</span><span class="sxs-lookup"><span data-stu-id="7c52a-240">Calls <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> to redirect to the root Index page (`/Index`).</span></span>

### <a name="the-editcshtml-file"></a><span data-ttu-id="7c52a-241">Plik Edit. cshtml</span><span class="sxs-lookup"><span data-stu-id="7c52a-241">The Edit.cshtml file</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml?highlight=1)]

<span data-ttu-id="7c52a-242">Pierwszy wiersz zawiera `@page "{id:int}"` dyrektywę.</span><span class="sxs-lookup"><span data-stu-id="7c52a-242">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="7c52a-243">Ograniczenie routingu `"{id:int}"` instruuje stronę, aby akceptowała żądania do strony zawierającej `int` dane trasy.</span><span class="sxs-lookup"><span data-stu-id="7c52a-243">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="7c52a-244">Jeśli żądanie do strony nie zawiera danych trasy, które można przekonwertować na obiekt `int` , środowisko uruchomieniowe zwróci błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="7c52a-244">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="7c52a-245">Aby identyfikator był opcjonalny, Dołącz `?` do ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="7c52a-245">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="7c52a-246">Plik *Edit.cshtml.cs* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-246">The *Edit.cshtml.cs* file:</span></span>

[!code-csharp[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml.cs?name=snippet)]

## <a name="validation"></a><span data-ttu-id="7c52a-247">Walidacja</span><span class="sxs-lookup"><span data-stu-id="7c52a-247">Validation</span></span>

<span data-ttu-id="7c52a-248">Reguły walidacji:</span><span class="sxs-lookup"><span data-stu-id="7c52a-248">Validation rules:</span></span>

* <span data-ttu-id="7c52a-249">Są deklaratywnie określone w klasie modelu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-249">Are declaratively specified in the model class.</span></span>
* <span data-ttu-id="7c52a-250">Są wymuszane wszędzie w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-250">Are enforced everywhere in the app.</span></span>

<span data-ttu-id="7c52a-251"><xref:System.ComponentModel.DataAnnotations>Przestrzeń nazw zawiera zestaw wbudowanych atrybutów walidacji, które są stosowane deklaratywnie do klasy lub właściwości.</span><span class="sxs-lookup"><span data-stu-id="7c52a-251">The <xref:System.ComponentModel.DataAnnotations> namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="7c52a-252">Adnotacje DataAnnotation zawierają również atrybuty formatowania, takie jak [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) Pomoc dotycząca formatowania i nie zapewniają weryfikacji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-252">DataAnnotations also contains formatting attributes like [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="7c52a-253">Rozważmy `Customer` model:</span><span class="sxs-lookup"><span data-stu-id="7c52a-253">Consider the `Customer` model:</span></span>

[!code-csharp[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="7c52a-254">Za pomocą następującego pliku widoku *Create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-254">Using the following *Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=3,8-9,15-99)]

<span data-ttu-id="7c52a-255">Powyższy kod ma następujące działanie:</span><span class="sxs-lookup"><span data-stu-id="7c52a-255">The preceding code:</span></span>

* <span data-ttu-id="7c52a-256">Obejmuje skrypty sprawdzania poprawności jQuery i jQuery.</span><span class="sxs-lookup"><span data-stu-id="7c52a-256">Includes jQuery and jQuery validation scripts.</span></span>
* <span data-ttu-id="7c52a-257">Używa `<div />` `<span />` [pomocników tagów](xref:mvc/views/tag-helpers/intro) i do włączania:</span><span class="sxs-lookup"><span data-stu-id="7c52a-257">Uses the `<div />` and `<span />` [Tag Helpers](xref:mvc/views/tag-helpers/intro) to enable:</span></span>

  * <span data-ttu-id="7c52a-258">Sprawdzanie poprawności po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="7c52a-258">Client-side validation.</span></span>
  * <span data-ttu-id="7c52a-259">Renderowanie błędów walidacji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-259">Validation error rendering.</span></span>

* <span data-ttu-id="7c52a-260">Generuje następujący kod HTML:</span><span class="sxs-lookup"><span data-stu-id="7c52a-260">Generates the following HTML:</span></span>

  [!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create5.html)]

<span data-ttu-id="7c52a-261">Opublikowanie formularza tworzenia bez wartości nazwa powoduje wyświetlenie komunikatu o błędzie "Nazwa pola jest wymagana."</span><span class="sxs-lookup"><span data-stu-id="7c52a-261">Posting the Create form without a name value displays the error message "The Name field is required."</span></span> <span data-ttu-id="7c52a-262">w formularzu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-262">on the form.</span></span> <span data-ttu-id="7c52a-263">Jeśli na kliencie jest włączona obsługa języka JavaScript, przeglądarka wyświetli komunikat o błędzie bez publikowania na serwerze.</span><span class="sxs-lookup"><span data-stu-id="7c52a-263">If JavaScript is enabled on the client, the browser displays the error without posting to the server.</span></span>

<span data-ttu-id="7c52a-264">Ten `[StringLength(10)]` atrybut jest generowany `data-val-length-max="10"` na RENDEROWANYM kodzie HTML.</span><span class="sxs-lookup"><span data-stu-id="7c52a-264">The `[StringLength(10)]` attribute generates `data-val-length-max="10"` on the rendered HTML.</span></span> <span data-ttu-id="7c52a-265">`data-val-length-max` zapobiega wprowadzaniu przez przeglądarki więcej niż określoną maksymalną długość.</span><span class="sxs-lookup"><span data-stu-id="7c52a-265">`data-val-length-max` prevents browsers from entering more than the maximum length specified.</span></span> <span data-ttu-id="7c52a-266">Jeśli jest używane narzędzie, takie jak [programu Fiddler](https://www.telerik.com/fiddler) , do edytowania i powtarzania wpisu:</span><span class="sxs-lookup"><span data-stu-id="7c52a-266">If a tool such as [Fiddler](https://www.telerik.com/fiddler) is used to edit and replay the post:</span></span>

* <span data-ttu-id="7c52a-267">O nazwie dłuższej niż 10.</span><span class="sxs-lookup"><span data-stu-id="7c52a-267">With the name longer than 10.</span></span>
* <span data-ttu-id="7c52a-268">Komunikat o błędzie "Nazwa pola musi być ciągiem o maksymalnej długości 10".</span><span class="sxs-lookup"><span data-stu-id="7c52a-268">The error message "The field Name must be a string with a maximum length of 10."</span></span> <span data-ttu-id="7c52a-269">.</span><span class="sxs-lookup"><span data-stu-id="7c52a-269">is returned.</span></span>

<span data-ttu-id="7c52a-270">Rozważmy następujący `Movie` model:</span><span class="sxs-lookup"><span data-stu-id="7c52a-270">Consider the following `Movie` model:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="7c52a-271">Atrybuty walidacji określają zachowanie do wymuszania na właściwościach modelu, do których są stosowane:</span><span class="sxs-lookup"><span data-stu-id="7c52a-271">The validation attributes specify behavior to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="7c52a-272">`Required`Atrybuty i `MinimumLength` wskazują, że właściwość musi mieć wartość, ale nic nie zapobiega wprowadzaniu przez użytkownika białych znaków w celu zaspokojenia tej walidacji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-272">The `Required` and `MinimumLength` attributes indicate that a property must have a value, but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="7c52a-273">Ten `RegularExpression` atrybut służy do ograniczania, jakie znaki mogą być wprowadzane.</span><span class="sxs-lookup"><span data-stu-id="7c52a-273">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="7c52a-274">W poprzednim kodzie "gatunek":</span><span class="sxs-lookup"><span data-stu-id="7c52a-274">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="7c52a-275">Należy używać tylko liter.</span><span class="sxs-lookup"><span data-stu-id="7c52a-275">Must only use letters.</span></span>
  * <span data-ttu-id="7c52a-276">Pierwsza litera musi być wielką literą.</span><span class="sxs-lookup"><span data-stu-id="7c52a-276">The first letter is required to be uppercase.</span></span> <span data-ttu-id="7c52a-277">Odstępy, cyfry i znaki specjalne są niedozwolone.</span><span class="sxs-lookup"><span data-stu-id="7c52a-277">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="7c52a-278">`RegularExpression`"Ocena":</span><span class="sxs-lookup"><span data-stu-id="7c52a-278">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="7c52a-279">Wymaga, aby pierwszy znak był wielką literą.</span><span class="sxs-lookup"><span data-stu-id="7c52a-279">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="7c52a-280">Zezwala na znaki specjalne i cyfry w kolejnych odstępach.</span><span class="sxs-lookup"><span data-stu-id="7c52a-280">Allows special characters and numbers in subsequent spaces.</span></span> <span data-ttu-id="7c52a-281">"PG-13" jest prawidłowy dla oceny, ale kończy się niepowodzeniem dla "gatunku".</span><span class="sxs-lookup"><span data-stu-id="7c52a-281">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="7c52a-282">Atrybut `Range` ogranicza wartość do określonego zakresu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-282">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="7c52a-283">`StringLength`Atrybut ustawia maksymalną długość właściwości ciągu i opcjonalnie jej długość minimalną.</span><span class="sxs-lookup"><span data-stu-id="7c52a-283">The `StringLength` attribute sets the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="7c52a-284">Typy wartości (takie jak `decimal` ,,, `int` `float` `DateTime` ) są z założenia wymagane i nie wymagają `[Required]` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-284">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="7c52a-285">Na stronie Tworzenie dla `Movie` modelu są wyświetlane błędy z nieprawidłowymi wartościami:</span><span class="sxs-lookup"><span data-stu-id="7c52a-285">The Create page for the `Movie` model shows displays errors with invalid values:</span></span>

![Formularz widoku filmu z wieloma błędami walidacji po stronie klienta jQuery](~/tutorials/razor-pages/validation/_static/val.png)

<span data-ttu-id="7c52a-287">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="7c52a-287">For more information, see:</span></span>

* [<span data-ttu-id="7c52a-288">Dodawanie walidacji do aplikacji filmowej</span><span class="sxs-lookup"><span data-stu-id="7c52a-288">Add validation to the Movie app</span></span>](xref:tutorials/razor-pages/validation)
* <span data-ttu-id="7c52a-289">[Walidacja modelu w ASP.NET Core](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="7c52a-289">[Model validation in ASP.NET Core](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="7c52a-290">Obsługa żądań głównych przy użyciu rezerwy procedury obsługi OnGet</span><span class="sxs-lookup"><span data-stu-id="7c52a-290">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="7c52a-291">`HEAD` żądania umożliwiają pobranie nagłówków dla określonego zasobu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-291">`HEAD` requests allow retrieving the headers for a specific resource.</span></span> <span data-ttu-id="7c52a-292">W przeciwieństwie do `GET` żądań, `HEAD` żądania nie zwracają treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="7c52a-292">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="7c52a-293">Zwykle `OnHead` program obsługi jest tworzony i wywoływany dla `HEAD` żądań:</span><span class="sxs-lookup"><span data-stu-id="7c52a-293">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span>

[!code-csharp[](index/3.0sample/RazorPagesContacts/Pages/Privacy.cshtml.cs?name=snippet)]

<span data-ttu-id="7c52a-294">Razor Strony powracają do wywoływania `OnGet` procedury obsługi, jeśli nie `OnHead` zdefiniowano procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="7c52a-294">Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="7c52a-295">XSRF/CSRF i Razor strony</span><span class="sxs-lookup"><span data-stu-id="7c52a-295">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="7c52a-296">Razor Strony są chronione przez [weryfikację przed fałszerstwem](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="7c52a-296">Razor Pages are protected by [Antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="7c52a-297">[FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) wprowadza do elementów formularza HTML tokeny zabezpieczające przed fałszerstwem.</span><span class="sxs-lookup"><span data-stu-id="7c52a-297">The [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="7c52a-298">Używanie układów, częściowych, szablonów i pomocników tagów ze Razor stronami</span><span class="sxs-lookup"><span data-stu-id="7c52a-298">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="7c52a-299">Strony współpracują ze wszystkimi możliwościami Razor aparatu widoku.</span><span class="sxs-lookup"><span data-stu-id="7c52a-299">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="7c52a-300">Układy, częściowe, szablony, pomocniki tagów, *_ViewStart. cshtml* i *_ViewImports. cshtml* działają w taki sam sposób, jak w przypadku widoków konwencjonalnych Razor .</span><span class="sxs-lookup"><span data-stu-id="7c52a-300">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, and *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="7c52a-301">Zanotujmy Tę stronę, korzystając z zalet niektórych z tych funkcji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-301">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="7c52a-302">Dodaj [stronę układu](xref:mvc/views/layout) do *stron/Shared/_Layout. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7c52a-302">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Shared/_Layout2.cshtml?hightlight=12)]

<span data-ttu-id="7c52a-303">[Układ](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="7c52a-303">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="7c52a-304">Steruje układem każdej strony (chyba że strona nie jest częścią układu).</span><span class="sxs-lookup"><span data-stu-id="7c52a-304">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="7c52a-305">Importuje struktury HTML, takie jak JavaScript i stylesheets.</span><span class="sxs-lookup"><span data-stu-id="7c52a-305">Imports HTML structures such as JavaScript and stylesheets.</span></span>
* <span data-ttu-id="7c52a-306">Zawartość Razor strony jest renderowana, gdzie `@RenderBody()` jest wywoływana.</span><span class="sxs-lookup"><span data-stu-id="7c52a-306">The contents of the Razor page are rendered where `@RenderBody()` is called.</span></span>

<span data-ttu-id="7c52a-307">Aby uzyskać więcej informacji, zobacz [stronę układu](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="7c52a-307">For more information, see [layout page](xref:mvc/views/layout).</span></span>

<span data-ttu-id="7c52a-308">Właściwość [układu](xref:mvc/views/layout#specifying-a-layout) jest ustawiana na *stronie/_ViewStart. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7c52a-308">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="7c52a-309">Układ znajduje się w *stronie/w folderze udostępnionym* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-309">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="7c52a-310">Strony szukają innych widoków (układów, szablonów, częściowych) hierarchicznie, rozpoczynając w tym samym folderze, w którym znajduje się bieżąca strona.</span><span class="sxs-lookup"><span data-stu-id="7c52a-310">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="7c52a-311">Układ na *stronach/w folderze udostępnionym* może być używany z dowolnej Razor strony w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-311">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="7c52a-312">Plik układu powinien przejść do *stron/folderu udostępnionego* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-312">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="7c52a-313">Zalecamy **umieszczenie pliku** układu w *widokach/folderze udostępnionym* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-313">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="7c52a-314">*Widoki/udostępnione* są wzorcem widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="7c52a-314">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="7c52a-315">Razor Strony są przeznaczone do korzystania z hierarchii folderów, a nie Konwencji ścieżek.</span><span class="sxs-lookup"><span data-stu-id="7c52a-315">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="7c52a-316">Widok wyszukiwania na Razor stronie zawiera folder *strony* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-316">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="7c52a-317">Układy, szablony i częściowe używane razem z kontrolerami MVC i konwencjonalnymi Razor widokami *działają tylko*.</span><span class="sxs-lookup"><span data-stu-id="7c52a-317">The layouts, templates, and partials used with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="7c52a-318">Dodaj plik *Pages/_ViewImports. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-318">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="7c52a-319">`@namespace` wyjaśniono w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="7c52a-319">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="7c52a-320">Dyrektywa znajduje się `@addTagHelper` w [wbudowanych pomocników tagów](xref:mvc/views/tag-helpers/builtin-th/Index) do wszystkich stron w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-320">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="7c52a-321">`@namespace`Dyrektywa ustawiona na stronie:</span><span class="sxs-lookup"><span data-stu-id="7c52a-321">The `@namespace` directive set on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="7c52a-322">`@namespace`Dyrektywa ustawia przestrzeń nazw dla strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-322">The `@namespace` directive sets the namespace for the page.</span></span> <span data-ttu-id="7c52a-323">`@model`Dyrektywa nie musi zawierać przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="7c52a-323">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="7c52a-324">Gdy `@namespace` dyrektywa jest zawarta w *_ViewImports. cshtml*, określona przestrzeń nazw udostępnia prefiks dla wygenerowanej przestrzeni nazw na stronie, która importuje `@namespace` dyrektywę.</span><span class="sxs-lookup"><span data-stu-id="7c52a-324">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="7c52a-325">Pozostała część wygenerowanej przestrzeni nazw (część sufiksu) jest ścieżką względną oddzieloną kropką między folderem zawierającym *_ViewImports. cshtml* i folderem zawierającym stronę.</span><span class="sxs-lookup"><span data-stu-id="7c52a-325">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="7c52a-326">Na przykład `PageModel` Klasa *Pages/Customers/Edit. cshtml. cs* jawnie ustawia przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="7c52a-326">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-csharp[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="7c52a-327">Plik *Pages/_ViewImports. cshtml* ustawia następującą przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="7c52a-327">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="7c52a-328">Wygenerowana przestrzeń nazw strony */Customers/Edit. cshtml* Razor jest taka sama jak `PageModel` Klasa.</span><span class="sxs-lookup"><span data-stu-id="7c52a-328">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="7c52a-329">`@namespace`*działa również z konwencjonalnymi Razor widokami.*</span><span class="sxs-lookup"><span data-stu-id="7c52a-329">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="7c52a-330">Rozważ użycie pliku widoku *Pages/Create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-330">Consider the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]

<span data-ttu-id="7c52a-331">Zaktualizowane *strony/Create. cshtml* plik widoku z *_ViewImports. cshtml* i poprzedni plik układu:</span><span class="sxs-lookup"><span data-stu-id="7c52a-331">The updated *Pages/Create.cshtml* view file with *_ViewImports.cshtml* and the preceding layout file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.cshtml?highlight=2)]

<span data-ttu-id="7c52a-332">W poprzednim kodzie, *_ViewImports. cshtml* zaimportował przestrzeń nazw i pomocników tagów.</span><span class="sxs-lookup"><span data-stu-id="7c52a-332">In the preceding code, the *_ViewImports.cshtml* imported the namespace and Tag Helpers.</span></span> <span data-ttu-id="7c52a-333">Plik układu zaimportował pliki JavaScript.</span><span class="sxs-lookup"><span data-stu-id="7c52a-333">The layout file imported the JavaScript files.</span></span>

<span data-ttu-id="7c52a-334">Na stronie [ Razor startowej projektu](#rpvs17) znajdują się *strony/_ValidationScriptsPartial. cshtml*, które przechwytuje walidację po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="7c52a-334">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="7c52a-335">Aby uzyskać więcej informacji o widokach częściowych, zobacz <xref:mvc/views/partial> .</span><span class="sxs-lookup"><span data-stu-id="7c52a-335">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="7c52a-336">Generowanie adresu URL dla stron</span><span class="sxs-lookup"><span data-stu-id="7c52a-336">URL generation for Pages</span></span>

<span data-ttu-id="7c52a-337">`Create`Strona, pokazana wcześniej, używa `RedirectToPage` :</span><span class="sxs-lookup"><span data-stu-id="7c52a-337">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-csharp[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=28)]

<span data-ttu-id="7c52a-338">Aplikacja ma następującą strukturę plików/folderów:</span><span class="sxs-lookup"><span data-stu-id="7c52a-338">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="7c52a-339">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="7c52a-339">*/Pages*</span></span>

  * <span data-ttu-id="7c52a-340">*Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-340">*Index.cshtml*</span></span>
  * <span data-ttu-id="7c52a-341">*Privacy. cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-341">*Privacy.cshtml*</span></span>
  * <span data-ttu-id="7c52a-342">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="7c52a-342">*/Customers*</span></span>

    * <span data-ttu-id="7c52a-343">*Create. cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-343">*Create.cshtml*</span></span>
    * <span data-ttu-id="7c52a-344">*Edytuj. cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-344">*Edit.cshtml*</span></span>
    * <span data-ttu-id="7c52a-345">*Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-345">*Index.cshtml*</span></span>

<span data-ttu-id="7c52a-346">Strony */Customers/Create. cshtml* i *Pages/Customers/Edit. cshtml* przekierowywać do *stron/Customers/index. cshtml* po powodzeniu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-346">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Customers/Index.cshtml* after success.</span></span> <span data-ttu-id="7c52a-347">Ciąg `./Index` jest względną nazwą strony używaną w celu uzyskania dostępu do poprzedniej strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-347">The string `./Index` is a relative page name used to access the preceding page.</span></span> <span data-ttu-id="7c52a-348">Służy do generowania adresów URL na stronie *strony/klienci/index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-348">It is used to generate URLs to the *Pages/Customers/Index.cshtml* page.</span></span> <span data-ttu-id="7c52a-349">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="7c52a-349">For example:</span></span>

* `Url.Page("./Index", ...)`
* `<a asp-page="./Index">Customers Index Page</a>`
* `RedirectToPage("./Index")`

<span data-ttu-id="7c52a-350">Bezwzględna nazwa strony `/Index` służy do generowania adresów URL na stronie *stron/index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-350">The absolute page name `/Index` is used to generate URLs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="7c52a-351">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="7c52a-351">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">Home Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="7c52a-352">Nazwa strony jest ścieżką do strony z folderu głównego */Pages* , włącznie z wiodącym `/` (na przykład `/Index` ).</span><span class="sxs-lookup"><span data-stu-id="7c52a-352">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="7c52a-353">Powyższe przykłady generowania adresów URL oferują udoskonalone Opcje i możliwości funkcjonalne w porównaniu z zakodowanym adresem URL.</span><span class="sxs-lookup"><span data-stu-id="7c52a-353">The preceding URL generation samples offer enhanced options and functional capabilities over hard-coding a URL.</span></span> <span data-ttu-id="7c52a-354">Generowanie adresów URL używa [routingu](xref:mvc/controllers/routing) i może generować i kodować parametry zgodnie ze sposobem zdefiniowania trasy w ścieżce docelowej.</span><span class="sxs-lookup"><span data-stu-id="7c52a-354">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="7c52a-355">Generowanie adresów URL dla stron obsługuje nazwy względne.</span><span class="sxs-lookup"><span data-stu-id="7c52a-355">URL generation for pages supports relative names.</span></span> <span data-ttu-id="7c52a-356">W poniższej tabeli przedstawiono, która strona indeksu została wybrana przy użyciu różnych `RedirectToPage` parametrów na stronie */Customers/Create. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7c52a-356">The following table shows which Index page is selected using different `RedirectToPage` parameters in *Pages/Customers/Create.cshtml*.</span></span>

| <span data-ttu-id="7c52a-357">RedirectToPage (x)</span><span class="sxs-lookup"><span data-stu-id="7c52a-357">RedirectToPage(x)</span></span>| <span data-ttu-id="7c52a-358">Strona</span><span class="sxs-lookup"><span data-stu-id="7c52a-358">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="7c52a-359">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="7c52a-359">RedirectToPage("/Index")</span></span> | <span data-ttu-id="7c52a-360">*Strony/indeks*</span><span class="sxs-lookup"><span data-stu-id="7c52a-360">*Pages/Index*</span></span> |
| <span data-ttu-id="7c52a-361">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="7c52a-361">RedirectToPage("./Index");</span></span> | <span data-ttu-id="7c52a-362">*Strony/klienci/indeks*</span><span class="sxs-lookup"><span data-stu-id="7c52a-362">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="7c52a-363">RedirectToPage(".. /Index")</span><span class="sxs-lookup"><span data-stu-id="7c52a-363">RedirectToPage("../Index")</span></span> | <span data-ttu-id="7c52a-364">*Strony/indeks*</span><span class="sxs-lookup"><span data-stu-id="7c52a-364">*Pages/Index*</span></span> |
| <span data-ttu-id="7c52a-365">RedirectToPage ("index")</span><span class="sxs-lookup"><span data-stu-id="7c52a-365">RedirectToPage("Index")</span></span>  | <span data-ttu-id="7c52a-366">*Strony/klienci/indeks*</span><span class="sxs-lookup"><span data-stu-id="7c52a-366">*Pages/Customers/Index*</span></span> |

<!-- Test via ~/razor-pages/index/3.0sample/RazorPagesContacts/Pages/Customers/Details.cshtml.cs -->

<span data-ttu-id="7c52a-367">`RedirectToPage("Index")`, `RedirectToPage("./Index")` , i `RedirectToPage("../Index")` są *nazwami względnymi*.</span><span class="sxs-lookup"><span data-stu-id="7c52a-367">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")` are *relative names*.</span></span> <span data-ttu-id="7c52a-368">`RedirectToPage`Parametr jest *połączony* ze ścieżką bieżącej strony, aby obliczyć nazwę strony docelowej.</span><span class="sxs-lookup"><span data-stu-id="7c52a-368">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>

<span data-ttu-id="7c52a-369">Łączenie nazw względnych jest przydatne podczas kompilowania lokacji ze złożoną strukturą.</span><span class="sxs-lookup"><span data-stu-id="7c52a-369">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="7c52a-370">Gdy nazwy względne są używane do łączenia między stronami w folderze:</span><span class="sxs-lookup"><span data-stu-id="7c52a-370">When relative names are used to link between pages in a folder:</span></span>

* <span data-ttu-id="7c52a-371">Zmiana nazwy folderu nie powoduje zerwania linków względnych.</span><span class="sxs-lookup"><span data-stu-id="7c52a-371">Renaming a folder doesn't break the relative links.</span></span>
* <span data-ttu-id="7c52a-372">Linki nie są przerwane, ponieważ nie zawierają nazwy folderu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-372">Links are not broken because they don't include the folder name.</span></span>

<span data-ttu-id="7c52a-373">Aby przekierować do strony w innym [obszarze](xref:mvc/controllers/areas), określ obszar:</span><span class="sxs-lookup"><span data-stu-id="7c52a-373">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="7c52a-374">Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/areas> i <xref:razor-pages/razor-pages-conventions>.</span><span class="sxs-lookup"><span data-stu-id="7c52a-374">For more information, see <xref:mvc/controllers/areas> and <xref:razor-pages/razor-pages-conventions>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="7c52a-375">ViewData — atrybut</span><span class="sxs-lookup"><span data-stu-id="7c52a-375">ViewData attribute</span></span>

<span data-ttu-id="7c52a-376">Dane można przekazywać do strony za pomocą <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute> .</span><span class="sxs-lookup"><span data-stu-id="7c52a-376">Data can be passed to a page with <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span></span> <span data-ttu-id="7c52a-377">Właściwości z `[ViewData]` atrybutem mają przechowywane i ładowane wartości z <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary> .</span><span class="sxs-lookup"><span data-stu-id="7c52a-377">Properties with the `[ViewData]` attribute have their values stored and loaded from the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span></span>

<span data-ttu-id="7c52a-378">W poniższym przykładzie `AboutModel` stosuje `[ViewData]` atrybut do `Title` Właściwości:</span><span class="sxs-lookup"><span data-stu-id="7c52a-378">In the following example, the `AboutModel` applies the `[ViewData]` attribute to the `Title` property:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="7c52a-379">Na stronie informacje uzyskaj dostęp do `Title` właściwości jako właściwości modelu:</span><span class="sxs-lookup"><span data-stu-id="7c52a-379">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="7c52a-380">W układzie tytuł jest odczytywany ze słownika ViewData:</span><span class="sxs-lookup"><span data-stu-id="7c52a-380">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="7c52a-381">TempData</span><span class="sxs-lookup"><span data-stu-id="7c52a-381">TempData</span></span>

<span data-ttu-id="7c52a-382">ASP.NET Core uwidacznia <xref:Microsoft.AspNetCore.Mvc.Controller.TempData> .</span><span class="sxs-lookup"><span data-stu-id="7c52a-382">ASP.NET Core exposes the <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span></span> <span data-ttu-id="7c52a-383">Ta właściwość przechowuje dane, dopóki nie zostanie odczytana.</span><span class="sxs-lookup"><span data-stu-id="7c52a-383">This property stores data until it's read.</span></span> <span data-ttu-id="7c52a-384"><xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*>Metody i <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> mogą służyć do badania danych bez usuwania.</span><span class="sxs-lookup"><span data-stu-id="7c52a-384">The <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> and <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="7c52a-385">`TempData` jest przydatne w przypadku przekierowania, gdy dane są potrzebne dla więcej niż jednego żądania.</span><span class="sxs-lookup"><span data-stu-id="7c52a-385">`TempData` is useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="7c52a-386">Poniższy kod ustawia wartość `Message` użycia `TempData` :</span><span class="sxs-lookup"><span data-stu-id="7c52a-386">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-csharp[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="7c52a-387">W poniższym znaczniku w pliku *Pages/Customers/index. cshtml* jest wyświetlana wartość `Message` using `TempData` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-387">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="7c52a-388">Model strony *Pages/Customers/index. cshtml. cs* stosuje `[TempData]` atrybut do `Message` właściwości.</span><span class="sxs-lookup"><span data-stu-id="7c52a-388">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```csharp
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="7c52a-389">Aby uzyskać więcej informacji, zobacz [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="7c52a-389">For more information, see [TempData](xref:fundamentals/app-state#tempdata).</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="7c52a-390">Wiele programów obsługi na stronie</span><span class="sxs-lookup"><span data-stu-id="7c52a-390">Multiple handlers per page</span></span>

<span data-ttu-id="7c52a-391">Poniższa Strona generuje znaczniki dla dwóch programów obsługi przy użyciu `asp-page-handler` pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="7c52a-391">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<span data-ttu-id="7c52a-392">Formularz w poprzednim przykładzie ma dwa przyciski przesyłania, z których każdy używa `FormActionTagHelper` do przesłania do innego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="7c52a-392">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="7c52a-393">Ten `asp-page-handler` atrybut jest towarzyszący do `asp-page` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-393">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="7c52a-394">`asp-page-handler` generuje adresy URL, które przesyłają do każdej metody obsługi zdefiniowanej przez stronę.</span><span class="sxs-lookup"><span data-stu-id="7c52a-394">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="7c52a-395">`asp-page` nie została określona, ponieważ próbka jest łączona z bieżącą stroną.</span><span class="sxs-lookup"><span data-stu-id="7c52a-395">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="7c52a-396">Model strony:</span><span class="sxs-lookup"><span data-stu-id="7c52a-396">The page model:</span></span>

[!code-csharp[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="7c52a-397">Poprzedni kod używa *nazwanych metod obsługi*.</span><span class="sxs-lookup"><span data-stu-id="7c52a-397">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="7c52a-398">Nazwane metody obsługi są tworzone przez pobranie tekstu w nazwie po `On<HTTP Verb>` i przed `Async` (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="7c52a-398">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="7c52a-399">W poprzednim przykładzie metody strony są onpost **JoinList** Async i Onpost **JoinListUC** Async.</span><span class="sxs-lookup"><span data-stu-id="7c52a-399">In the preceding example, the page methods are OnPost **JoinList** Async and OnPost **JoinListUC** Async.</span></span> <span data-ttu-id="7c52a-400">Po usunięciu funkcji *onpost* i *Async* nazwy programów obsługi są `JoinList` i `JoinListUC` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-400">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="7c52a-401">Korzystając z powyższego kodu, ścieżka URL, która jest przesyłana do usługi, `OnPostJoinListAsync` to `https://localhost:5001/Customers/CreateFATH?handler=JoinList` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-401">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="7c52a-402">Ścieżka URL, która jest przesyłana do programu, `OnPostJoinListUCAsync` to `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-402">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="7c52a-403">Trasy niestandardowe</span><span class="sxs-lookup"><span data-stu-id="7c52a-403">Custom routes</span></span>

<span data-ttu-id="7c52a-404">Użyj `@page` dyrektywy, aby:</span><span class="sxs-lookup"><span data-stu-id="7c52a-404">Use the `@page` directive to:</span></span>

* <span data-ttu-id="7c52a-405">Określ trasę niestandardową dla strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-405">Specify a custom route to a page.</span></span> <span data-ttu-id="7c52a-406">Na przykład trasy do strony informacje można ustawić na wartość `/Some/Other/Path` z `@page "/Some/Other/Path"` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-406">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="7c52a-407">Dołącz segmenty do domyślnej trasy strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-407">Append segments to a page's default route.</span></span> <span data-ttu-id="7c52a-408">Na przykład segment "Item" można dodać do domyślnej trasy strony przy użyciu `@page "item"` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-408">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="7c52a-409">Dołącz parametry do domyślnej trasy strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-409">Append parameters to a page's default route.</span></span> <span data-ttu-id="7c52a-410">Na przykład parametr ID, `id` , może być wymagany dla strony z `@page "{id}"` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-410">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="7c52a-411">Ścieżka względna elementu głównego wypisana przez tyldę ( `~` ) na początku ścieżki jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="7c52a-411">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="7c52a-412">Na przykład `@page "~/Some/Other/Path"` jest taka sama jak `@page "/Some/Other/Path"` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-412">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="7c52a-413">Jeśli nie podoba Ci się ciąg zapytania `?handler=JoinList` w adresie URL, Zmień trasę, aby umieścić nazwę programu obsługi w części adresu URL.</span><span class="sxs-lookup"><span data-stu-id="7c52a-413">If you don't like the query string `?handler=JoinList` in the URL, change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="7c52a-414">Trasę można dostosować, dodając szablon trasy ujęty w podwójne cudzysłowy po `@page` dyrektywie.</span><span class="sxs-lookup"><span data-stu-id="7c52a-414">The route can be customized by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="7c52a-415">Korzystając z powyższego kodu, ścieżka URL, która jest przesyłana do usługi, `OnPostJoinListAsync` to `https://localhost:5001/Customers/CreateFATH/JoinList` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-415">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="7c52a-416">Ścieżka URL, która jest przesyłana do programu, `OnPostJoinListUCAsync` to `https://localhost:5001/Customers/CreateFATH/JoinListUC` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-416">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="7c52a-417">`?`Poniżej przedstawiono `handler` , że parametr trasy jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="7c52a-417">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="advanced-configuration-and-settings"></a><span data-ttu-id="7c52a-418">Zaawansowana konfiguracja i ustawienia</span><span class="sxs-lookup"><span data-stu-id="7c52a-418">Advanced configuration and settings</span></span>

<span data-ttu-id="7c52a-419">Konfiguracja i ustawienia w poniższych sekcjach nie są wymagane przez większość aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-419">The configuration and settings in following sections is not required by most apps.</span></span>

<span data-ttu-id="7c52a-420">Aby skonfigurować opcje zaawansowane, użyj <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddRazorPages%2A> przeciążenia, które konfiguruje <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> :</span><span class="sxs-lookup"><span data-stu-id="7c52a-420">To configure advanced options, use the <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddRazorPages%2A> overload that configures <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions>:</span></span>

[!code-csharp[](index/3.0sample/RazorPagesContacts/StartupRPoptions.cs?name=snippet)]

<span data-ttu-id="7c52a-421">Użyj, <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> Aby ustawić katalog główny dla stron lub dodać konwencje modelu aplikacji dla stron.</span><span class="sxs-lookup"><span data-stu-id="7c52a-421">Use the <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="7c52a-422">Aby uzyskać więcej informacji na temat Konwencji, zobacz [ Razor strony konwencje autoryzacji](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="7c52a-422">For more information on conventions, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization).</span></span>

<span data-ttu-id="7c52a-423">Aby wstępnie skompilować widoki, zobacz [ Razor przeglądanie kompilacji](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="7c52a-423">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation).</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="7c52a-424">Określ, że Razor strony znajdują się w katalogu głównym zawartości</span><span class="sxs-lookup"><span data-stu-id="7c52a-424">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="7c52a-425">Domyślnie Razor strony są umieszczane w katalogu */Pages* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-425">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="7c52a-426">Dodaj <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> , aby określić, że Razor strony znajdują się w [katalogu głównym zawartości](xref:fundamentals/index#content-root) <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath> aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7c52a-426">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) of the app:</span></span>

[!code-csharp[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesAtContentRoot.cs?name=snippet)]

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="7c52a-427">Określ, że Razor strony znajdują się w niestandardowym katalogu głównym</span><span class="sxs-lookup"><span data-stu-id="7c52a-427">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="7c52a-428">Dodaj <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> , aby określić, że Razor strony znajdują się w niestandardowym katalogu głównym w aplikacji (podaj ścieżkę względną):</span><span class="sxs-lookup"><span data-stu-id="7c52a-428">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> to specify that Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

[!code-csharp[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesRoot.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="7c52a-429">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7c52a-429">Additional resources</span></span>

* <span data-ttu-id="7c52a-430">Zobacz Rozpoczynanie [pracy ze Razor stronami](xref:tutorials/razor-pages/razor-pages-start), które kompilują się w tym wprowadzeniu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-430">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>
* [<span data-ttu-id="7c52a-431">Autoryzuj atrybut i Razor strony</span><span class="sxs-lookup"><span data-stu-id="7c52a-431">Authorize attribute and Razor Pages</span></span>](xref:security/authorization/simple#aarp)
* [<span data-ttu-id="7c52a-432">Pobieranie lub wyświetlanie przykładowego kodu</span><span class="sxs-lookup"><span data-stu-id="7c52a-432">Download or view sample code</span></span>](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/razor-pages/index/3.0sample)
* <xref:index>
* [<span data-ttu-id="7c52a-433">Razor odwołanie do składni dla ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c52a-433">Razor syntax reference for ASP.NET Core</span></span>](xref:mvc/views/razor)
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>
* <xref:blazor/components/prerendering-and-integration>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

# <a name="visual-studio"></a>[<span data-ttu-id="7c52a-434">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c52a-434">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="7c52a-435">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c52a-435">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="7c52a-436">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="7c52a-436">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="7c52a-437">Tworzenie Razor projektu stron</span><span class="sxs-lookup"><span data-stu-id="7c52a-437">Create a Razor Pages project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="7c52a-438">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7c52a-438">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7c52a-439">Aby uzyskać szczegółowe instrukcje dotyczące tworzenia projektu stron, zobacz Wprowadzenie do [ Razor stron](xref:tutorials/razor-pages/razor-pages-start) Razor .</span><span class="sxs-lookup"><span data-stu-id="7c52a-439">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="7c52a-440">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="7c52a-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="7c52a-441">Uruchom `dotnet new webapp` polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="7c52a-441">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="7c52a-442">Otwórz wygenerowany plik *csproj* z Visual Studio dla komputerów Mac.</span><span class="sxs-lookup"><span data-stu-id="7c52a-442">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="7c52a-443">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="7c52a-443">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="7c52a-444">Uruchom `dotnet new webapp` polecenie w wierszu polecenia.</span><span class="sxs-lookup"><span data-stu-id="7c52a-444">Run `dotnet new webapp` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="7c52a-445">Razor Page</span><span class="sxs-lookup"><span data-stu-id="7c52a-445">Razor Pages</span></span>

<span data-ttu-id="7c52a-446">Razor Strony są włączone w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7c52a-446">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-csharp[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="7c52a-447">Weź pod uwagę podstawową stronę: <a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="7c52a-447">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="7c52a-448">Poprzedni kod wygląda podobnie jak [ Razor plik widoku](xref:tutorials/first-mvc-app/adding-view) używany w aplikacji ASP.NET Core z kontrolerami i widokami.</span><span class="sxs-lookup"><span data-stu-id="7c52a-448">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="7c52a-449">Co sprawia, że jest to `@page` dyrektywa.</span><span class="sxs-lookup"><span data-stu-id="7c52a-449">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="7c52a-450">`@page` sprawia, że plik jest akcją MVC, co oznacza, że obsługuje żądania bezpośrednio, bez przechodzenia przez kontroler.</span><span class="sxs-lookup"><span data-stu-id="7c52a-450">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="7c52a-451">`@page` musi być pierwszą Razor dyrektywą na stronie.</span><span class="sxs-lookup"><span data-stu-id="7c52a-451">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="7c52a-452">`@page` wpływa na zachowanie innych Razor konstrukcji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-452">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="7c52a-453">Podobna Strona, za pomocą `PageModel` klasy, jest pokazana w następujących dwóch plikach.</span><span class="sxs-lookup"><span data-stu-id="7c52a-453">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="7c52a-454">Plik *Pages/index2. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-454">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="7c52a-455">Model strony *stron/index2. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-455">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-csharp[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="7c52a-456">Zgodnie z Konwencją `PageModel` plik klasy ma taką samą nazwę jak Razor plik stronicowania dołączony do *. cs* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-456">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="7c52a-457">Na przykład Poprzednia Razor strona to *Pages/index2. cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7c52a-457">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="7c52a-458">Plik zawierający `PageModel` klasę ma nazwę *Pages/index2. cshtml. cs*.</span><span class="sxs-lookup"><span data-stu-id="7c52a-458">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="7c52a-459">Skojarzenia ścieżek adresów URL ze stronami są określane przez lokalizację strony w systemie plików.</span><span class="sxs-lookup"><span data-stu-id="7c52a-459">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="7c52a-460">W poniższej tabeli przedstawiono Razor ścieżkę strony i pasujący adres URL:</span><span class="sxs-lookup"><span data-stu-id="7c52a-460">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="7c52a-461">Nazwa i ścieżka pliku</span><span class="sxs-lookup"><span data-stu-id="7c52a-461">File name and path</span></span>               | <span data-ttu-id="7c52a-462">pasujący adres URL</span><span class="sxs-lookup"><span data-stu-id="7c52a-462">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="7c52a-463">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-463">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="7c52a-464">`/` lub `/Index`</span><span class="sxs-lookup"><span data-stu-id="7c52a-464">`/` or `/Index`</span></span> |
| <span data-ttu-id="7c52a-465">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-465">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="7c52a-466">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-466">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="7c52a-467">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-467">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="7c52a-468">`/Store` lub `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="7c52a-468">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="7c52a-469">Uwagi:</span><span class="sxs-lookup"><span data-stu-id="7c52a-469">Notes:</span></span>

* <span data-ttu-id="7c52a-470">Środowisko uruchomieniowe domyślnie wyszukuje Razor pliki stron w folderze *Pages* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-470">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="7c52a-471">`Index` jest stroną domyślną, gdy adres URL nie zawiera strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-471">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="7c52a-472">Napisz podstawowy formularz</span><span class="sxs-lookup"><span data-stu-id="7c52a-472">Write a basic form</span></span>

<span data-ttu-id="7c52a-473">Razor Strony są przeznaczone do tworzenia wspólnych wzorców używanych z przeglądarkami sieci Web, które są łatwe do wdrożenia podczas kompilowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-473">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="7c52a-474">[Powiązania modelu](xref:mvc/models/model-binding), [pomocników tagów](xref:mvc/views/tag-helpers/intro)i pomocników HTML same *działają* z właściwościami zdefiniowanymi w Razor klasie Page.</span><span class="sxs-lookup"><span data-stu-id="7c52a-474">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="7c52a-475">Weź pod uwagę stronę implementującą podstawowy formularz "contact us" (kontakt z nami) dla `Contact` modelu:</span><span class="sxs-lookup"><span data-stu-id="7c52a-475">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="7c52a-476">Przykłady w tym dokumencie `DbContext` są inicjowane w pliku [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/main/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) .</span><span class="sxs-lookup"><span data-stu-id="7c52a-476">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/main/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-csharp[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="7c52a-477">Model danych:</span><span class="sxs-lookup"><span data-stu-id="7c52a-477">The data model:</span></span>

[!code-csharp[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="7c52a-478">Kontekst bazy danych:</span><span class="sxs-lookup"><span data-stu-id="7c52a-478">The db context:</span></span>

[!code-csharp[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="7c52a-479">Plik widoku *Pages/Create. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-479">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="7c52a-480">Model strony *Pages/Create. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-480">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-csharp[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="7c52a-481">Zgodnie z Konwencją `PageModel` Klasa jest wywoływana `<PageName>Model` i znajduje się w tej samej przestrzeni nazw co strona.</span><span class="sxs-lookup"><span data-stu-id="7c52a-481">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="7c52a-482">`PageModel`Klasa umożliwia rozdzielenie logiki strony od jej prezentacji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-482">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="7c52a-483">Definiuje procedury obsługi stron dla żądań wysyłanych do strony oraz dane używane do renderowania strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-483">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="7c52a-484">Ta separacja umożliwia:</span><span class="sxs-lookup"><span data-stu-id="7c52a-484">This separation allows:</span></span>

* <span data-ttu-id="7c52a-485">Zarządzanie zależnościami stron przy użyciu [iniekcji zależności](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="7c52a-485">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="7c52a-486">[Testowanie jednostkowe](xref:test/razor-pages-tests) stron.</span><span class="sxs-lookup"><span data-stu-id="7c52a-486">[Unit testing](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="7c52a-487">Strona ma `OnPostAsync` *metodę obsługi*, która jest uruchamiana na `POST` żądaniach (gdy użytkownik księguje formularz).</span><span class="sxs-lookup"><span data-stu-id="7c52a-487">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="7c52a-488">Metody procedury obsługi można dodać dla dowolnego czasownika HTTP.</span><span class="sxs-lookup"><span data-stu-id="7c52a-488">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="7c52a-489">Najczęstsze procedury obsługi to:</span><span class="sxs-lookup"><span data-stu-id="7c52a-489">The most common handlers are:</span></span>

* <span data-ttu-id="7c52a-490">`OnGet` — na potrzeby inicjowania stanu wymaganego dla strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-490">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="7c52a-491">Przykład [OnGet](#OnGet) .</span><span class="sxs-lookup"><span data-stu-id="7c52a-491">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="7c52a-492">`OnPost` — na potrzeby obsługi przesłanych formularzy.</span><span class="sxs-lookup"><span data-stu-id="7c52a-492">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="7c52a-493">Sufiks nazewnictwa `Async` jest opcjonalny, ale jest często używany zgodnie z konwencją na potrzeby funkcji asynchronicznych.</span><span class="sxs-lookup"><span data-stu-id="7c52a-493">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="7c52a-494">Poprzedni kod jest typowy dla Razor stron.</span><span class="sxs-lookup"><span data-stu-id="7c52a-494">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="7c52a-495">Jeśli znasz już aplikacje ASP.NET przy użyciu kontrolerów i widoków:</span><span class="sxs-lookup"><span data-stu-id="7c52a-495">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="7c52a-496">`OnPostAsync`Kod w poprzednim przykładzie wygląda podobnie do typowego kodu kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7c52a-496">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="7c52a-497">Większość elementów podstawowych MVC, takich jak [powiązanie modelu](xref:mvc/models/model-binding), [Walidacja](xref:mvc/models/validation), [Walidacja](xref:mvc/models/validation)i wyniki akcji, jest udostępniana.</span><span class="sxs-lookup"><span data-stu-id="7c52a-497">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), [Validation](xref:mvc/models/validation),  and action results are shared.</span></span>

<span data-ttu-id="7c52a-498">Poprzednia `OnPostAsync` Metoda:</span><span class="sxs-lookup"><span data-stu-id="7c52a-498">The previous `OnPostAsync` method:</span></span>

[!code-csharp[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="7c52a-499">Podstawowy przepływ `OnPostAsync` :</span><span class="sxs-lookup"><span data-stu-id="7c52a-499">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="7c52a-500">Sprawdź, czy występują błędy walidacji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-500">Check for validation errors.</span></span>

* <span data-ttu-id="7c52a-501">Jeśli nie ma żadnych błędów, Zapisz dane i Przekieruj.</span><span class="sxs-lookup"><span data-stu-id="7c52a-501">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="7c52a-502">W przypadku wystąpienia błędów ponownie Wyświetl stronę z komunikatami walidacji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-502">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="7c52a-503">Walidacja po stronie klienta jest taka sama jak w przypadku tradycyjnych ASP.NET Core aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="7c52a-503">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="7c52a-504">W wielu przypadkach błędy weryfikacji zostaną wykryte w kliencie i nigdy nie zostaną przesłane do serwera.</span><span class="sxs-lookup"><span data-stu-id="7c52a-504">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="7c52a-505">Po pomyślnym wprowadzeniu danych `OnPostAsync` Metoda obsługi wywołuje `RedirectToPage` metodę pomocnika zwracającą wystąpienie elementu `RedirectToPageResult` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-505">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="7c52a-506">`RedirectToPage` to nowy wynik akcji, podobny do `RedirectToAction` lub `RedirectToRoute` , ale dostosowany do stron.</span><span class="sxs-lookup"><span data-stu-id="7c52a-506">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="7c52a-507">W powyższym przykładzie przekierowuje do strony indeksu głównego ( `/Index` ).</span><span class="sxs-lookup"><span data-stu-id="7c52a-507">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="7c52a-508">`RedirectToPage` szczegółowo znajduje się w sekcji [generowanie adresów URL dla stron](#url_gen) .</span><span class="sxs-lookup"><span data-stu-id="7c52a-508">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="7c52a-509">Gdy przesłany formularz ma błędy walidacji (które są przekazywane do serwera), `OnPostAsync` Metoda obsługi wywołuje `Page` metodę pomocnika.</span><span class="sxs-lookup"><span data-stu-id="7c52a-509">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="7c52a-510">Metoda `Page` zwraca wystąpienie klasy `PageResult`.</span><span class="sxs-lookup"><span data-stu-id="7c52a-510">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="7c52a-511">Zwracanie `Page` jest podobne do sposobu, w jaki akcje w kontrolerach zwracają `View` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-511">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="7c52a-512">`PageResult` jest domyślnym typem zwracanym dla metody obsługi.</span><span class="sxs-lookup"><span data-stu-id="7c52a-512">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="7c52a-513">Metoda obsługi, która zwraca `void` renderowanie strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-513">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="7c52a-514">`Customer`Właściwość używa `[BindProperty]` atrybutu, aby zrezygnować z powiązania modelu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-514">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-csharp[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="7c52a-515">Razor Domyślnie strony powiążą właściwości tylko z `GET` niezleceniami.</span><span class="sxs-lookup"><span data-stu-id="7c52a-515">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="7c52a-516">Utworzenie powiązania z właściwościami może zmniejszyć ilość kodu, który trzeba napisać.</span><span class="sxs-lookup"><span data-stu-id="7c52a-516">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="7c52a-517">Powiązanie zmniejsza kod, używając tej samej właściwości do renderowania pól formularza ( `<input asp-for="Customer.Name">` ) i akceptuję dane wejściowe.</span><span class="sxs-lookup"><span data-stu-id="7c52a-517">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="7c52a-518">Strona główna (*index. cshtml*):</span><span class="sxs-lookup"><span data-stu-id="7c52a-518">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="7c52a-519">Skojarzona `PageModel` Klasa (*index.cshtml.cs*):</span><span class="sxs-lookup"><span data-stu-id="7c52a-519">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-csharp[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="7c52a-520">Plik *index. cshtml* zawiera następujące znaczniki, aby utworzyć łącze do edycji dla każdego kontaktu:</span><span class="sxs-lookup"><span data-stu-id="7c52a-520">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="7c52a-521">`<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Pomocnik tagu kotwicy](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) użył `asp-route-{value}` atrybutu w celu wygenerowania linku do strony edycji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-521">The `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="7c52a-522">Link zawiera dane trasy z IDENTYFIKATORem kontaktu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-522">The link contains route data with the contact ID.</span></span> <span data-ttu-id="7c52a-523">Na przykład `https://localhost:5001/Edit/1`.</span><span class="sxs-lookup"><span data-stu-id="7c52a-523">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="7c52a-524">[Pomocnicy tagów](xref:mvc/views/tag-helpers/intro) włączają kod po stronie serwera, aby uczestniczyć w tworzeniu i RENDEROWANIU elementów HTML w Razor plikach.</span><span class="sxs-lookup"><span data-stu-id="7c52a-524">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="7c52a-525">Pomocnicy tagów są włączani przez `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="7c52a-525">Tag Helpers are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="7c52a-526">Plik *Pages/Edit. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-526">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="7c52a-527">Pierwszy wiersz zawiera `@page "{id:int}"` dyrektywę.</span><span class="sxs-lookup"><span data-stu-id="7c52a-527">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="7c52a-528">Ograniczenie routingu `"{id:int}"` instruuje stronę, aby akceptowała żądania do strony zawierającej `int` dane trasy.</span><span class="sxs-lookup"><span data-stu-id="7c52a-528">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="7c52a-529">Jeśli żądanie do strony nie zawiera danych trasy, które można przekonwertować na obiekt `int` , środowisko uruchomieniowe zwróci błąd HTTP 404 (nie znaleziono).</span><span class="sxs-lookup"><span data-stu-id="7c52a-529">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="7c52a-530">Aby identyfikator był opcjonalny, Dołącz `?` do ograniczenia trasy:</span><span class="sxs-lookup"><span data-stu-id="7c52a-530">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="7c52a-531">Plik *stron/Edytuj. cshtml. cs* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-531">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-csharp[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="7c52a-532">Plik *index. cshtml* zawiera również znaczniki umożliwiające utworzenie przycisku usuwania dla każdego kontaktu z klientem:</span><span class="sxs-lookup"><span data-stu-id="7c52a-532">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="7c52a-533">Gdy przycisk Usuń jest renderowany w języku HTML, jego `formaction` Parametry obejmują:</span><span class="sxs-lookup"><span data-stu-id="7c52a-533">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="7c52a-534">Identyfikator osoby kontaktowej klienta określony przez `asp-route-id` atrybut.</span><span class="sxs-lookup"><span data-stu-id="7c52a-534">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="7c52a-535">`handler`Określony przez `asp-page-handler` atrybut.</span><span class="sxs-lookup"><span data-stu-id="7c52a-535">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="7c52a-536">Oto przykład renderowanego przycisku usuwania z IDENTYFIKATORem kontaktu klienta `1` :</span><span class="sxs-lookup"><span data-stu-id="7c52a-536">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="7c52a-537">Po wybraniu przycisku `POST` do serwera zostanie wysłane żądanie formularza.</span><span class="sxs-lookup"><span data-stu-id="7c52a-537">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="7c52a-538">Według Konwencji, nazwa metody obsługi jest wybierana na podstawie wartości `handler` parametru zgodnie z schematem `OnPost[handler]Async` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-538">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="7c52a-539">Ponieważ `handler` jest `delete` w tym przykładzie, `OnPostDeleteAsync` Metoda obsługi jest używana do przetwarzania `POST` żądania.</span><span class="sxs-lookup"><span data-stu-id="7c52a-539">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="7c52a-540">Jeśli `asp-page-handler` jest ustawiona na inną wartość, na przykład, jest `remove` wybierana metoda obsługi o nazwie `OnPostRemoveAsync` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-540">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span> <span data-ttu-id="7c52a-541">Poniższy kod ilustruje `OnPostDeleteAsync` procedurę obsługi:</span><span class="sxs-lookup"><span data-stu-id="7c52a-541">The following code shows the `OnPostDeleteAsync` handler:</span></span>

[!code-csharp[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="7c52a-542">`OnPostDeleteAsync`Metoda:</span><span class="sxs-lookup"><span data-stu-id="7c52a-542">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="7c52a-543">Akceptuje `id` z ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="7c52a-543">Accepts the `id` from the query string.</span></span> <span data-ttu-id="7c52a-544">Jeśli na stronie *index. cshtml* znajduje się ograniczenie routingu `"{id:int?}"` , `id` będą pochodzić z danych tras.</span><span class="sxs-lookup"><span data-stu-id="7c52a-544">If the *Index.cshtml* page directive contained routing constraint `"{id:int?}"`, `id` would come from route data.</span></span> <span data-ttu-id="7c52a-545">Dane trasy dla programu `id` są określone w identyfikatorze URI, takich jak `https://localhost:5001/Customers/2` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-545">The route data for `id` is specified in the URI such as `https://localhost:5001/Customers/2`.</span></span>
* <span data-ttu-id="7c52a-546">Wysyła zapytanie do bazy danych w celu skontaktowania się z klientem za pomocą programu `FindAsync` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-546">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="7c52a-547">Jeśli kontakt z klientem zostanie znaleziony, zostanie on usunięty z listy kontaktów klientów.</span><span class="sxs-lookup"><span data-stu-id="7c52a-547">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="7c52a-548">Baza danych jest aktualizowana.</span><span class="sxs-lookup"><span data-stu-id="7c52a-548">The database is updated.</span></span>
* <span data-ttu-id="7c52a-549">Wywołania `RedirectToPage` do przekierowania na stronę indeksu głównego ( `/Index` ).</span><span class="sxs-lookup"><span data-stu-id="7c52a-549">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="7c52a-550">Oznacz właściwości strony jako wymagane</span><span class="sxs-lookup"><span data-stu-id="7c52a-550">Mark page properties as required</span></span>

<span data-ttu-id="7c52a-551">Właściwości na stronie `PageModel` można oznaczyć przy użyciu [wymaganego](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) atrybutu:</span><span class="sxs-lookup"><span data-stu-id="7c52a-551">Properties on a `PageModel` can be marked with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-csharp[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="7c52a-552">Aby uzyskać więcej informacji, zobacz [Walidacja modelu](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="7c52a-552">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="7c52a-553">Obsługa żądań głównych przy użyciu rezerwy procedury obsługi OnGet</span><span class="sxs-lookup"><span data-stu-id="7c52a-553">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="7c52a-554">`HEAD` żądania umożliwiają pobranie nagłówków dla określonego zasobu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-554">`HEAD` requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="7c52a-555">W przeciwieństwie do `GET` żądań, `HEAD` żądania nie zwracają treści odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="7c52a-555">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="7c52a-556">Zwykle `OnHead` program obsługi jest tworzony i wywoływany dla `HEAD` żądań:</span><span class="sxs-lookup"><span data-stu-id="7c52a-556">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="7c52a-557">W ASP.NET Core 2,1 lub nowszej strony wracają Razor do wywoływania `OnGet` procedury obsługi, jeśli nie `OnHead` zdefiniowano procedury obsługi.</span><span class="sxs-lookup"><span data-stu-id="7c52a-557">In ASP.NET Core 2.1 or later, Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span> <span data-ttu-id="7c52a-558">To zachowanie jest włączane przez wywołanie [SetCompatibilityVersion](xref:mvc/compatibility-version) w `Startup.ConfigureServices` :</span><span class="sxs-lookup"><span data-stu-id="7c52a-558">This behavior is enabled by the call to [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="7c52a-559">Szablony domyślne generują `SetCompatibilityVersion` wywołanie w ASP.NET Core 2,1 i 2,2.</span><span class="sxs-lookup"><span data-stu-id="7c52a-559">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span> <span data-ttu-id="7c52a-560">`SetCompatibilityVersion` efektywnie ustawia Razor opcję strony `AllowMappingHeadRequestsToGetHandler` na `true` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-560">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="7c52a-561">Zamiast korzystać z wszystkich zachowań w programie `SetCompatibilityVersion` , można jawnie zrezygnować z *określonych* zachowań.</span><span class="sxs-lookup"><span data-stu-id="7c52a-561">Rather than opting in to all behaviors with `SetCompatibilityVersion`, you can explicitly opt in to *specific* behaviors.</span></span> <span data-ttu-id="7c52a-562">Poniższy kod pozwala na umożliwienie `HEAD` mapowania żądań do `OnGet` programu obsługi:</span><span class="sxs-lookup"><span data-stu-id="7c52a-562">The following code opts in to allowing `HEAD` requests to be mapped to the `OnGet` handler:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="7c52a-563">XSRF/CSRF i Razor strony</span><span class="sxs-lookup"><span data-stu-id="7c52a-563">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="7c52a-564">Nie trzeba pisać kodu do [weryfikacji przed fałszerstwem](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="7c52a-564">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="7c52a-565">Generowanie i sprawdzanie poprawności tokenów antysfałszowanych są automatycznie dołączane do Razor stron.</span><span class="sxs-lookup"><span data-stu-id="7c52a-565">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="7c52a-566">Używanie układów, częściowych, szablonów i pomocników tagów ze Razor stronami</span><span class="sxs-lookup"><span data-stu-id="7c52a-566">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="7c52a-567">Strony współpracują ze wszystkimi możliwościami Razor aparatu widoku.</span><span class="sxs-lookup"><span data-stu-id="7c52a-567">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="7c52a-568">Układy, częściowe, szablony, pomocniki tagów, *_ViewStart. cshtml*, *_ViewImports. cshtml* działają w taki sam sposób, jak w przypadku Razor widoków konwencjonalnych.</span><span class="sxs-lookup"><span data-stu-id="7c52a-568">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="7c52a-569">Zanotujmy Tę stronę, korzystając z zalet niektórych z tych funkcji.</span><span class="sxs-lookup"><span data-stu-id="7c52a-569">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="7c52a-570">Dodaj [stronę układu](xref:mvc/views/layout) do *stron/Shared/_Layout. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7c52a-570">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="7c52a-571">[Układ](xref:mvc/views/layout):</span><span class="sxs-lookup"><span data-stu-id="7c52a-571">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="7c52a-572">Steruje układem każdej strony (chyba że strona nie jest częścią układu).</span><span class="sxs-lookup"><span data-stu-id="7c52a-572">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="7c52a-573">Importuje struktury HTML, takie jak JavaScript i stylesheets.</span><span class="sxs-lookup"><span data-stu-id="7c52a-573">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="7c52a-574">Aby uzyskać więcej informacji, zobacz [stronę układu](xref:mvc/views/layout) .</span><span class="sxs-lookup"><span data-stu-id="7c52a-574">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="7c52a-575">Właściwość [układu](xref:mvc/views/layout#specifying-a-layout) jest ustawiana na *stronie/_ViewStart. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7c52a-575">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="7c52a-576">Układ znajduje się w *stronie/w folderze udostępnionym* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-576">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="7c52a-577">Strony szukają innych widoków (układów, szablonów, częściowych) hierarchicznie, rozpoczynając w tym samym folderze, w którym znajduje się bieżąca strona.</span><span class="sxs-lookup"><span data-stu-id="7c52a-577">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="7c52a-578">Układ na *stronach/w folderze udostępnionym* może być używany z dowolnej Razor strony w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-578">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="7c52a-579">Plik układu powinien przejść do *stron/folderu udostępnionego* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-579">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="7c52a-580">Zalecamy **umieszczenie pliku** układu w *widokach/folderze udostępnionym* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-580">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="7c52a-581">*Widoki/udostępnione* są wzorcem widoków MVC.</span><span class="sxs-lookup"><span data-stu-id="7c52a-581">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="7c52a-582">Razor Strony są przeznaczone do korzystania z hierarchii folderów, a nie Konwencji ścieżek.</span><span class="sxs-lookup"><span data-stu-id="7c52a-582">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="7c52a-583">Widok wyszukiwania na Razor stronie zawiera folder *strony* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-583">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="7c52a-584">Układy, szablony i częściowe, które są używane z kontrolerami MVC i konwencjonalnymi Razor widokami *działają tylko*.</span><span class="sxs-lookup"><span data-stu-id="7c52a-584">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="7c52a-585">Dodaj plik *Pages/_ViewImports. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-585">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="7c52a-586">`@namespace` wyjaśniono w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="7c52a-586">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="7c52a-587">Dyrektywa znajduje się `@addTagHelper` w [wbudowanych pomocników tagów](xref:mvc/views/tag-helpers/builtin-th/Index) do wszystkich stron w folderze *strony* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-587">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="7c52a-588">Gdy `@namespace` dyrektywa jest używana jawnie na stronie:</span><span class="sxs-lookup"><span data-stu-id="7c52a-588">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="7c52a-589">Dyrektywa ustawia przestrzeń nazw dla strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-589">The directive sets the namespace for the page.</span></span> <span data-ttu-id="7c52a-590">`@model`Dyrektywa nie musi zawierać przestrzeni nazw.</span><span class="sxs-lookup"><span data-stu-id="7c52a-590">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="7c52a-591">Gdy `@namespace` dyrektywa jest zawarta w *_ViewImports. cshtml*, określona przestrzeń nazw udostępnia prefiks dla wygenerowanej przestrzeni nazw na stronie, która importuje `@namespace` dyrektywę.</span><span class="sxs-lookup"><span data-stu-id="7c52a-591">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="7c52a-592">Pozostała część wygenerowanej przestrzeni nazw (część sufiksu) jest ścieżką względną oddzieloną kropką między folderem zawierającym *_ViewImports. cshtml* i folderem zawierającym stronę.</span><span class="sxs-lookup"><span data-stu-id="7c52a-592">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="7c52a-593">Na przykład `PageModel` Klasa *Pages/Customers/Edit. cshtml. cs* jawnie ustawia przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="7c52a-593">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-csharp[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="7c52a-594">Plik *Pages/_ViewImports. cshtml* ustawia następującą przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="7c52a-594">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="7c52a-595">Wygenerowana przestrzeń nazw strony */Customers/Edit. cshtml* Razor jest taka sama jak `PageModel` Klasa.</span><span class="sxs-lookup"><span data-stu-id="7c52a-595">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="7c52a-596">`@namespace`*działa również z konwencjonalnymi Razor widokami.*</span><span class="sxs-lookup"><span data-stu-id="7c52a-596">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="7c52a-597">Oryginalne *strony/Utwórz plik widoku. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-597">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="7c52a-598">Zaktualizowane *strony/Utwórz plik widoku. cshtml* :</span><span class="sxs-lookup"><span data-stu-id="7c52a-598">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="7c52a-599">Na stronie [ Razor startowej projektu](#rpvs17) znajdują się *strony/_ValidationScriptsPartial. cshtml*, które przechwytuje walidację po stronie klienta.</span><span class="sxs-lookup"><span data-stu-id="7c52a-599">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="7c52a-600">Aby uzyskać więcej informacji o widokach częściowych, zobacz <xref:mvc/views/partial> .</span><span class="sxs-lookup"><span data-stu-id="7c52a-600">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="7c52a-601">Generowanie adresu URL dla stron</span><span class="sxs-lookup"><span data-stu-id="7c52a-601">URL generation for Pages</span></span>

<span data-ttu-id="7c52a-602">`Create`Strona, pokazana wcześniej, używa `RedirectToPage` :</span><span class="sxs-lookup"><span data-stu-id="7c52a-602">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-csharp[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="7c52a-603">Aplikacja ma następującą strukturę plików/folderów:</span><span class="sxs-lookup"><span data-stu-id="7c52a-603">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="7c52a-604">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="7c52a-604">*/Pages*</span></span>

  * <span data-ttu-id="7c52a-605">*Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-605">*Index.cshtml*</span></span>
  * <span data-ttu-id="7c52a-606">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="7c52a-606">*/Customers*</span></span>

    * <span data-ttu-id="7c52a-607">*Create. cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-607">*Create.cshtml*</span></span>
    * <span data-ttu-id="7c52a-608">*Edytuj. cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-608">*Edit.cshtml*</span></span>
    * <span data-ttu-id="7c52a-609">*Index. cshtml*</span><span class="sxs-lookup"><span data-stu-id="7c52a-609">*Index.cshtml*</span></span>

<span data-ttu-id="7c52a-610">Strony */Customers/Create. cshtml* i *Pages/Customers/Edit. cshtml* przekierują do *stron/index. cshtml* po powodzeniu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-610">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="7c52a-611">Ciąg `/Index` jest częścią identyfikatora URI, aby uzyskać dostęp do poprzedniej strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-611">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="7c52a-612">Ten ciąg `/Index` może służyć do generowania identyfikatorów URI na stronie *stron/index. cshtml* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-612">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="7c52a-613">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="7c52a-613">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="7c52a-614">Nazwa strony jest ścieżką do strony z folderu głównego */Pages* , włącznie z wiodącym `/` (na przykład `/Index` ).</span><span class="sxs-lookup"><span data-stu-id="7c52a-614">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="7c52a-615">Powyższe przykłady generowania adresów URL oferują ulepszone opcje i możliwości funkcjonalne w porównaniu z zakodowana adresem URL.</span><span class="sxs-lookup"><span data-stu-id="7c52a-615">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="7c52a-616">Generowanie adresów URL używa [routingu](xref:mvc/controllers/routing) i może generować i kodować parametry zgodnie ze sposobem zdefiniowania trasy w ścieżce docelowej.</span><span class="sxs-lookup"><span data-stu-id="7c52a-616">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="7c52a-617">Generowanie adresów URL dla stron obsługuje nazwy względne.</span><span class="sxs-lookup"><span data-stu-id="7c52a-617">URL generation for pages supports relative names.</span></span> <span data-ttu-id="7c52a-618">W poniższej tabeli przedstawiono, która strona indeksu została wybrana z różnymi `RedirectToPage` parametrami *stron/Customers/Create. cshtml*:</span><span class="sxs-lookup"><span data-stu-id="7c52a-618">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="7c52a-619">RedirectToPage (x)</span><span class="sxs-lookup"><span data-stu-id="7c52a-619">RedirectToPage(x)</span></span>| <span data-ttu-id="7c52a-620">Strona</span><span class="sxs-lookup"><span data-stu-id="7c52a-620">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="7c52a-621">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="7c52a-621">RedirectToPage("/Index")</span></span> | <span data-ttu-id="7c52a-622">*Strony/indeks*</span><span class="sxs-lookup"><span data-stu-id="7c52a-622">*Pages/Index*</span></span> |
| <span data-ttu-id="7c52a-623">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="7c52a-623">RedirectToPage("./Index");</span></span> | <span data-ttu-id="7c52a-624">*Strony/klienci/indeks*</span><span class="sxs-lookup"><span data-stu-id="7c52a-624">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="7c52a-625">RedirectToPage(".. /Index")</span><span class="sxs-lookup"><span data-stu-id="7c52a-625">RedirectToPage("../Index")</span></span> | <span data-ttu-id="7c52a-626">*Strony/indeks*</span><span class="sxs-lookup"><span data-stu-id="7c52a-626">*Pages/Index*</span></span> |
| <span data-ttu-id="7c52a-627">RedirectToPage ("index")</span><span class="sxs-lookup"><span data-stu-id="7c52a-627">RedirectToPage("Index")</span></span>  | <span data-ttu-id="7c52a-628">*Strony/klienci/indeks*</span><span class="sxs-lookup"><span data-stu-id="7c52a-628">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="7c52a-629">`RedirectToPage("Index")`, `RedirectToPage("./Index")` , i `RedirectToPage("../Index")`  są *nazwami względnymi*.</span><span class="sxs-lookup"><span data-stu-id="7c52a-629">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="7c52a-630">`RedirectToPage`Parametr jest *połączony* ze ścieżką bieżącej strony, aby obliczyć nazwę strony docelowej.</span><span class="sxs-lookup"><span data-stu-id="7c52a-630">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="7c52a-631">Łączenie nazw względnych jest przydatne podczas kompilowania lokacji ze złożoną strukturą.</span><span class="sxs-lookup"><span data-stu-id="7c52a-631">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="7c52a-632">Jeśli używasz nazw względnych do łączenia między stronami w folderze, możesz zmienić nazwę tego folderu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-632">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="7c52a-633">Wszystkie linki nadal działają (ponieważ nie zawierają nazwy folderu).</span><span class="sxs-lookup"><span data-stu-id="7c52a-633">All the links still work (because they didn't include the folder name).</span></span>

<span data-ttu-id="7c52a-634">Aby przekierować do strony w innym [obszarze](xref:mvc/controllers/areas), określ obszar:</span><span class="sxs-lookup"><span data-stu-id="7c52a-634">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="7c52a-635">Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/areas>.</span><span class="sxs-lookup"><span data-stu-id="7c52a-635">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="7c52a-636">ViewData — atrybut</span><span class="sxs-lookup"><span data-stu-id="7c52a-636">ViewData attribute</span></span>

<span data-ttu-id="7c52a-637">Dane można przekazywać do strony z [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span><span class="sxs-lookup"><span data-stu-id="7c52a-637">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="7c52a-638">Właściwości kontrolerów lub Razor modeli stron z `[ViewData]` atrybutem są przechowywane i ładowane z [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span><span class="sxs-lookup"><span data-stu-id="7c52a-638">Properties on controllers or Razor Page models with the `[ViewData]` attribute have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="7c52a-639">W poniższym przykładzie `AboutModel` zawiera właściwość, która jest `Title` oznaczona za pomocą `[ViewData]` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-639">In the following example, the `AboutModel` contains a `Title` property marked with `[ViewData]`.</span></span> <span data-ttu-id="7c52a-640">`Title`Właściwość jest ustawiona na tytuł strony informacje:</span><span class="sxs-lookup"><span data-stu-id="7c52a-640">The `Title` property is set to the title of the About page:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="7c52a-641">Na stronie informacje uzyskaj dostęp do `Title` właściwości jako właściwości modelu:</span><span class="sxs-lookup"><span data-stu-id="7c52a-641">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="7c52a-642">W układzie tytuł jest odczytywany ze słownika ViewData:</span><span class="sxs-lookup"><span data-stu-id="7c52a-642">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="7c52a-643">TempData</span><span class="sxs-lookup"><span data-stu-id="7c52a-643">TempData</span></span>

<span data-ttu-id="7c52a-644">ASP.NET Core uwidacznia Właściwość [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata#Microsoft_AspNetCore_Mvc_Controller_TempData) na [kontrolerze](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span><span class="sxs-lookup"><span data-stu-id="7c52a-644">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="7c52a-645">Ta właściwość przechowuje dane, dopóki nie zostanie odczytana.</span><span class="sxs-lookup"><span data-stu-id="7c52a-645">This property stores data until it's read.</span></span> <span data-ttu-id="7c52a-646">`Keep`Metody i `Peek` mogą służyć do badania danych bez usuwania.</span><span class="sxs-lookup"><span data-stu-id="7c52a-646">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="7c52a-647">`TempData` jest przydatne w przypadku przekierowania, gdy dane są potrzebne dla więcej niż jednego żądania.</span><span class="sxs-lookup"><span data-stu-id="7c52a-647">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="7c52a-648">Poniższy kod ustawia wartość `Message` użycia `TempData` :</span><span class="sxs-lookup"><span data-stu-id="7c52a-648">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-csharp[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="7c52a-649">W poniższym znaczniku w pliku *Pages/Customers/index. cshtml* jest wyświetlana wartość `Message` using `TempData` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-649">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="7c52a-650">Model strony *Pages/Customers/index. cshtml. cs* stosuje `[TempData]` atrybut do `Message` właściwości.</span><span class="sxs-lookup"><span data-stu-id="7c52a-650">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```csharp
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="7c52a-651">Aby uzyskać więcej informacji, zobacz [TempData](xref:fundamentals/app-state#tempdata) .</span><span class="sxs-lookup"><span data-stu-id="7c52a-651">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="7c52a-652">Wiele programów obsługi na stronie</span><span class="sxs-lookup"><span data-stu-id="7c52a-652">Multiple handlers per page</span></span>

<span data-ttu-id="7c52a-653">Poniższa Strona generuje znaczniki dla dwóch programów obsługi przy użyciu `asp-page-handler` pomocnika tagów:</span><span class="sxs-lookup"><span data-stu-id="7c52a-653">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="7c52a-654">Formularz w poprzednim przykładzie ma dwa przyciski przesyłania, z których każdy używa `FormActionTagHelper` do przesłania do innego adresu URL.</span><span class="sxs-lookup"><span data-stu-id="7c52a-654">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="7c52a-655">Ten `asp-page-handler` atrybut jest towarzyszący do `asp-page` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-655">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="7c52a-656">`asp-page-handler` generuje adresy URL, które przesyłają do każdej metody obsługi zdefiniowanej przez stronę.</span><span class="sxs-lookup"><span data-stu-id="7c52a-656">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="7c52a-657">`asp-page` nie została określona, ponieważ próbka jest łączona z bieżącą stroną.</span><span class="sxs-lookup"><span data-stu-id="7c52a-657">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="7c52a-658">Model strony:</span><span class="sxs-lookup"><span data-stu-id="7c52a-658">The page model:</span></span>

[!code-csharp[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="7c52a-659">Poprzedni kod używa *nazwanych metod obsługi*.</span><span class="sxs-lookup"><span data-stu-id="7c52a-659">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="7c52a-660">Nazwane metody obsługi są tworzone przez pobranie tekstu w nazwie po `On<HTTP Verb>` i przed `Async` (jeśli istnieje).</span><span class="sxs-lookup"><span data-stu-id="7c52a-660">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="7c52a-661">W poprzednim przykładzie metody strony są onpost **JoinList** Async i Onpost **JoinListUC** Async.</span><span class="sxs-lookup"><span data-stu-id="7c52a-661">In the preceding example, the page methods are OnPost **JoinList** Async and OnPost **JoinListUC** Async.</span></span> <span data-ttu-id="7c52a-662">Po usunięciu funkcji *onpost* i *Async* nazwy programów obsługi są `JoinList` i `JoinListUC` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-662">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="7c52a-663">Korzystając z powyższego kodu, ścieżka URL, która jest przesyłana do usługi, `OnPostJoinListAsync` to `https://localhost:5001/Customers/CreateFATH?handler=JoinList` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-663">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="7c52a-664">Ścieżka URL, która jest przesyłana do programu, `OnPostJoinListUCAsync` to `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-664">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="7c52a-665">Trasy niestandardowe</span><span class="sxs-lookup"><span data-stu-id="7c52a-665">Custom routes</span></span>

<span data-ttu-id="7c52a-666">Użyj `@page` dyrektywy, aby:</span><span class="sxs-lookup"><span data-stu-id="7c52a-666">Use the `@page` directive to:</span></span>

* <span data-ttu-id="7c52a-667">Określ trasę niestandardową dla strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-667">Specify a custom route to a page.</span></span> <span data-ttu-id="7c52a-668">Na przykład trasy do strony informacje można ustawić na wartość `/Some/Other/Path` z `@page "/Some/Other/Path"` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-668">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="7c52a-669">Dołącz segmenty do domyślnej trasy strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-669">Append segments to a page's default route.</span></span> <span data-ttu-id="7c52a-670">Na przykład segment "Item" można dodać do domyślnej trasy strony przy użyciu `@page "item"` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-670">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="7c52a-671">Dołącz parametry do domyślnej trasy strony.</span><span class="sxs-lookup"><span data-stu-id="7c52a-671">Append parameters to a page's default route.</span></span> <span data-ttu-id="7c52a-672">Na przykład parametr ID, `id` , może być wymagany dla strony z `@page "{id}"` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-672">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="7c52a-673">Ścieżka względna elementu głównego wypisana przez tyldę ( `~` ) na początku ścieżki jest obsługiwana.</span><span class="sxs-lookup"><span data-stu-id="7c52a-673">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="7c52a-674">Na przykład `@page "~/Some/Other/Path"` jest taka sama jak `@page "/Some/Other/Path"` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-674">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="7c52a-675">Jeśli nie podoba Ci się ciąg zapytania `?handler=JoinList` w adresie URL, Zmień trasę, aby umieścić nazwę programu obsługi w części adresu URL.</span><span class="sxs-lookup"><span data-stu-id="7c52a-675">If you don't like the query string `?handler=JoinList` in the URL, change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="7c52a-676">Trasę można dostosować, dodając szablon trasy ujęty w podwójne cudzysłowy po `@page` dyrektywie.</span><span class="sxs-lookup"><span data-stu-id="7c52a-676">The route can be customized by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="7c52a-677">Korzystając z powyższego kodu, ścieżka URL, która jest przesyłana do usługi, `OnPostJoinListAsync` to `https://localhost:5001/Customers/CreateFATH/JoinList` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-677">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="7c52a-678">Ścieżka URL, która jest przesyłana do programu, `OnPostJoinListUCAsync` to `https://localhost:5001/Customers/CreateFATH/JoinListUC` .</span><span class="sxs-lookup"><span data-stu-id="7c52a-678">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="7c52a-679">`?`Poniżej przedstawiono `handler` , że parametr trasy jest opcjonalny.</span><span class="sxs-lookup"><span data-stu-id="7c52a-679">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="7c52a-680">Konfiguracja i ustawienia</span><span class="sxs-lookup"><span data-stu-id="7c52a-680">Configuration and settings</span></span>

<span data-ttu-id="7c52a-681">Aby skonfigurować opcje zaawansowane, użyj metody rozszerzenia `AddRazorPagesOptions` w konstruktorze MVC:</span><span class="sxs-lookup"><span data-stu-id="7c52a-681">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-csharp[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="7c52a-682">Obecnie można użyć, `RazorPagesOptions` Aby ustawić katalog główny dla stron lub dodać konwencje modelu aplikacji dla stron.</span><span class="sxs-lookup"><span data-stu-id="7c52a-682">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="7c52a-683">W przyszłości włączysz więcej rozszerzeń w ten sposób.</span><span class="sxs-lookup"><span data-stu-id="7c52a-683">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="7c52a-684">Aby wstępnie skompilować widoki, zobacz [ Razor przeglądanie kompilacji](xref:mvc/views/view-compilation) .</span><span class="sxs-lookup"><span data-stu-id="7c52a-684">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="7c52a-685">[Pobierz lub Wyświetl przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/razor-pages/index/sample).</span><span class="sxs-lookup"><span data-stu-id="7c52a-685">[Download or view sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="7c52a-686">Zobacz Rozpoczynanie [pracy ze Razor stronami](xref:tutorials/razor-pages/razor-pages-start), które kompilują się w tym wprowadzeniu.</span><span class="sxs-lookup"><span data-stu-id="7c52a-686">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="7c52a-687">Określ, że Razor strony znajdują się w katalogu głównym zawartości</span><span class="sxs-lookup"><span data-stu-id="7c52a-687">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="7c52a-688">Domyślnie Razor strony są umieszczane w katalogu */Pages* .</span><span class="sxs-lookup"><span data-stu-id="7c52a-688">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="7c52a-689">Dodaj [program with Razor PagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) , aby określić, że Razor strony znajdują się w [katalogu głównym zawartości](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7c52a-689">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="7c52a-690">Określ, że Razor strony znajdują się w niestandardowym katalogu głównym</span><span class="sxs-lookup"><span data-stu-id="7c52a-690">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="7c52a-691">Dodaj element [with Razor PagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) do [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) , aby określić, że Razor strony znajdują się w niestandardowym katalogu głównym w aplikacji (podaj ścieżkę względną):</span><span class="sxs-lookup"><span data-stu-id="7c52a-691">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="7c52a-692">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="7c52a-692">Additional resources</span></span>

* [<span data-ttu-id="7c52a-693">Autoryzuj atrybut i Razor strony</span><span class="sxs-lookup"><span data-stu-id="7c52a-693">Authorize attribute and Razor Pages</span></span>](xref:security/authorization/simple#aarp)
* <xref:index>
* [<span data-ttu-id="7c52a-694">Razor odwołanie do składni dla ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7c52a-694">Razor syntax reference for ASP.NET Core</span></span>](xref:mvc/views/razor)
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end
