---
title: Część 6, Dodawanie wyszukiwania
author: rick-anderson
description: Część 6 serii samouczków na Razor stronach.
ms.author: riande
ms.date: 12/05/2019
ms.custom: contperf-fy21q2
no-loc:
- Index
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
uid: tutorials/razor-pages/search
ms.openlocfilehash: 0ffe4ed251eed551b2799c1a66424ba31137dc18
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102589520"
---
# <a name="part-6-add-search-to-aspnet-core-razor-pages"></a><span data-ttu-id="f55fb-103">Część 6, Dodawanie wyszukiwania do Razor stron ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f55fb-103">Part 6, add search to ASP.NET Core Razor Pages</span></span>

<span data-ttu-id="f55fb-104">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f55fb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="f55fb-105">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie50) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f55fb-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie50) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-5.0 >= aspnetcore-3.0"

<span data-ttu-id="f55fb-106">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f55fb-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f55fb-107">W poniższych sekcjach są dodawane przeszukiwania filmów według *gatunku* lub *nazwy* .</span><span class="sxs-lookup"><span data-stu-id="f55fb-107">In the following sections, searching movies by *genre* or *name* is added.</span></span>

<span data-ttu-id="f55fb-108">Dodaj następujące wyróżnione instrukcje using i właściwości do *stron/filmów/ Index . cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f55fb-108">Add the following highlighted using statement and properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=3,23,24,25,26,27)]

<span data-ttu-id="f55fb-109">W poprzednim kodzie:</span><span class="sxs-lookup"><span data-stu-id="f55fb-109">In the previous code:</span></span>

* <span data-ttu-id="f55fb-110">`SearchString`: Zawiera tekst wprowadzany przez użytkowników w polu tekstowym Wyszukaj.</span><span class="sxs-lookup"><span data-stu-id="f55fb-110">`SearchString`: Contains the text users enter in the search text box.</span></span> <span data-ttu-id="f55fb-111">`SearchString` ma [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) atrybut.</span><span class="sxs-lookup"><span data-stu-id="f55fb-111">`SearchString` has the [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute.</span></span> <span data-ttu-id="f55fb-112">`[BindProperty]` tworzy powiązanie wartości formularzy i ciągów zapytań o takiej samej nazwie jak właściwość.</span><span class="sxs-lookup"><span data-stu-id="f55fb-112">`[BindProperty]` binds form values and query strings with the same name as the property.</span></span> <span data-ttu-id="f55fb-113">`[BindProperty(SupportsGet = true)]` jest wymagany do powiązania żądań HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="f55fb-113">`[BindProperty(SupportsGet = true)]` is required for binding on HTTP GET requests.</span></span>
* <span data-ttu-id="f55fb-114">`Genres`: Zawiera listę gatunku.</span><span class="sxs-lookup"><span data-stu-id="f55fb-114">`Genres`: Contains the list of genres.</span></span> <span data-ttu-id="f55fb-115">`Genres` umożliwia użytkownikowi wybranie gatunku z listy.</span><span class="sxs-lookup"><span data-stu-id="f55fb-115">`Genres` allows the user to select a genre from the list.</span></span> <span data-ttu-id="f55fb-116">`SelectList` KONIECZN `using Microsoft.AspNetCore.Mvc.Rendering;`</span><span class="sxs-lookup"><span data-stu-id="f55fb-116">`SelectList` requires `using Microsoft.AspNetCore.Mvc.Rendering;`</span></span>
* <span data-ttu-id="f55fb-117">`MovieGenre`: Zawiera konkretny gatunek wybierany przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f55fb-117">`MovieGenre`: Contains the specific genre the user selects.</span></span> <span data-ttu-id="f55fb-118">Na przykład "zachodnie".</span><span class="sxs-lookup"><span data-stu-id="f55fb-118">For example, "Western".</span></span>
* <span data-ttu-id="f55fb-119">`Genres` i `MovieGenre` są używane w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="f55fb-119">`Genres` and `MovieGenre` are used later in this tutorial.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="f55fb-120">Zaktualizuj Index `OnGetAsync` metodę strony przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="f55fb-120">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="f55fb-121">Pierwszy wiersz `OnGetAsync` metody tworzy zapytanie [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) do wybierania filmów:</span><span class="sxs-lookup"><span data-stu-id="f55fb-121">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="f55fb-122">Zapytanie jest tylko ***zdefiniowane** _ w tym momencie, a *_nie_** zostało uruchomione względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f55fb-122">The query is only ***defined** _ at this point, it has _ *_not_** been run against the database.</span></span>

<span data-ttu-id="f55fb-123">Jeśli `SearchString` Właściwość nie ma wartości null lub jest pusta, zapytanie o filmy jest modyfikowane w celu odfiltrowania ciągu wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="f55fb-123">If the `SearchString` property is not null or empty, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="f55fb-124">`s => s.Title.Contains()`Kod jest [wyrażeniem lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="f55fb-124">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="f55fb-125">Wyrażenia lambda są używane w kwerendach [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) opartych na metodach jako argumenty dla standardowych metod operatorów zapytań, takich jak Metoda [WHERE](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) lub `Contains` .</span><span class="sxs-lookup"><span data-stu-id="f55fb-125">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains`.</span></span> <span data-ttu-id="f55fb-126">Zapytania LINQ nie są wykonywane, gdy są zdefiniowane lub są modyfikowane przez wywołanie metody, takiej jak `Where` , `Contains` lub `OrderBy` .</span><span class="sxs-lookup"><span data-stu-id="f55fb-126">LINQ queries are not executed when they're defined or when they're modified by calling a method, such as `Where`, `Contains`, or `OrderBy`.</span></span> <span data-ttu-id="f55fb-127">Zamiast tego wykonywanie zapytania jest odroczone.</span><span class="sxs-lookup"><span data-stu-id="f55fb-127">Rather, query execution is deferred.</span></span> <span data-ttu-id="f55fb-128">Obliczanie wyrażenia jest opóźnione do momentu przekroczenia jego prawdziwej wartości lub `ToListAsync` wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="f55fb-128">The evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="f55fb-129">Aby uzyskać więcej informacji, zobacz [wykonywanie zapytań](/dotnet/framework/data/adonet/ef/language-reference/query-execution) .</span><span class="sxs-lookup"><span data-stu-id="f55fb-129">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="f55fb-130">Metoda [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) jest uruchamiana w bazie danych, a nie w kodzie C#.</span><span class="sxs-lookup"><span data-stu-id="f55fb-130">The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="f55fb-131">Uwzględnianie wielkości liter w zapytaniu zależy od bazy danych i sortowania.</span><span class="sxs-lookup"><span data-stu-id="f55fb-131">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="f55fb-132">Na SQL Server `Contains` mapuje do [programu SQL Server, np](/sql/t-sql/language-elements/like-transact-sql). bez uwzględniania wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="f55fb-132">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="f55fb-133">W ramach programu SQLite domyślne sortowanie uwzględnia wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="f55fb-133">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="f55fb-134">Przejdź do strony filmy i dołącz ciąg zapytania, taki jak `?searchString=Ghost` adres URL.</span><span class="sxs-lookup"><span data-stu-id="f55fb-134">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL.</span></span> <span data-ttu-id="f55fb-135">Na przykład `https://localhost:5001/Movies?searchString=Ghost`.</span><span class="sxs-lookup"><span data-stu-id="f55fb-135">For example, `https://localhost:5001/Movies?searchString=Ghost`.</span></span> <span data-ttu-id="f55fb-136">Wyświetlane są filtrowane filmy.</span><span class="sxs-lookup"><span data-stu-id="f55fb-136">The filtered movies are displayed.</span></span>

![::: No-Loc (indeks)::: View](search/_static/ghost.png)

<span data-ttu-id="f55fb-138">Jeśli do strony zostanie dodany następujący szablon trasy Index , ciąg wyszukiwania może zostać przekierowany jako segment adresu URL.</span><span class="sxs-lookup"><span data-stu-id="f55fb-138">If the following route template is added to the Index page, the search string can be passed as a URL segment.</span></span> <span data-ttu-id="f55fb-139">Na przykład `https://localhost:5001/Movies/Ghost`.</span><span class="sxs-lookup"><span data-stu-id="f55fb-139">For example, `https://localhost:5001/Movies/Ghost`.</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="f55fb-140">Powyższe ograniczenie trasy umożliwia przeszukiwanie tytułu jako dane trasy (segment adresu URL), a nie jako wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="f55fb-140">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="f55fb-141">`?`W `"{searchString?}"` tym przypadku jest to opcjonalny parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="f55fb-141">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![::: No-Loc (index)::: View z wyrazem "Ghost" dodanym do adresu URL i zwrotną listą filmów dwóch filmów, Ghostbusters i Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="f55fb-143">Środowisko uruchomieniowe ASP.NET Core używa [powiązania modelu](xref:mvc/models/model-binding) , aby ustawić wartość `SearchString` właściwości z ciągu zapytania ( `?searchString=Ghost` ) lub danych trasy ( `https://localhost:5001/Movies/Ghost` ).</span><span class="sxs-lookup"><span data-stu-id="f55fb-143">The ASP.NET Core runtime uses [model binding](xref:mvc/models/model-binding) to set the value of the `SearchString` property from the query string (`?searchString=Ghost`) or route data (`https://localhost:5001/Movies/Ghost`).</span></span> <span data-ttu-id="f55fb-144">W powiązaniu modelu ***nie*** jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="f55fb-144">Model binding is ***not*** case sensitive.</span></span>

<span data-ttu-id="f55fb-145">Jednak użytkownicy nie mogą zmieniać adresu URL w celu wyszukania filmu.</span><span class="sxs-lookup"><span data-stu-id="f55fb-145">However, users cannot be expected to modify the URL to search for a movie.</span></span> <span data-ttu-id="f55fb-146">W tym kroku zostanie dodany interfejs użytkownika do filtrowania filmów.</span><span class="sxs-lookup"><span data-stu-id="f55fb-146">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="f55fb-147">Jeśli dodano ograniczenie trasy `"{searchString?}"` , usuń je.</span><span class="sxs-lookup"><span data-stu-id="f55fb-147">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="f55fb-148">Otwórz plik *Pages/Films/ Index . cshtml* i Dodaj znaczniki wyróżnione w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="f55fb-148">Open the *Pages/Movies/Index.cshtml* file, and add the markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="f55fb-149">Tag HTML `<form>` używa następujących [pomocników tagów](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="f55fb-149">The HTML `<form>` tag uses the following [Tag Helpers](xref:mvc/views/tag-helpers/intro):</span></span>

* <span data-ttu-id="f55fb-150">[Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="f55fb-150">[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="f55fb-151">Gdy formularz zostanie przesłany, ciąg filtru jest wysyłany do *stron/filmów/ Index* stron za pośrednictwem ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="f55fb-151">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page via query string.</span></span>
* [<span data-ttu-id="f55fb-152">Pomocnik tagu wejściowego</span><span class="sxs-lookup"><span data-stu-id="f55fb-152">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms#the-input-tag-helper)

<span data-ttu-id="f55fb-153">Zapisz zmiany i przetestuj filtr.</span><span class="sxs-lookup"><span data-stu-id="f55fb-153">Save the changes and test the filter.</span></span>

![::: No-Loc (index)::: View z wyrazem Ghost wpisanych do pola tekstowego filtru tytułu](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="f55fb-155">Wyszukaj według gatunku</span><span class="sxs-lookup"><span data-stu-id="f55fb-155">Search by genre</span></span>

<span data-ttu-id="f55fb-156">Zaktualizuj Index `OnGetAsync` metodę strony przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="f55fb-156">Update the Index page's `OnGetAsync` method with the following code:</span></span>

   [!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="f55fb-157">Poniższy kod jest zapytanie LINQ, które pobiera wszystkie gatunki z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f55fb-157">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="f55fb-158">`SelectList`Gatunek jest tworzony przez projekcję odrębnych gatuneków.</span><span class="sxs-lookup"><span data-stu-id="f55fb-158">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a><span data-ttu-id="f55fb-159">Dodaj wyszukiwanie według gatunku na Razor stronie</span><span class="sxs-lookup"><span data-stu-id="f55fb-159">Add search by genre to the Razor Page</span></span>

1. <span data-ttu-id="f55fb-160">Aktualizowanie *Index . cshtml* [ `<form>` element] ( https://developer.mozilla.org/docs/Web/HTML/Element/form) jak wyróżniono w następującej adjustacji):</span><span class="sxs-lookup"><span data-stu-id="f55fb-160">Update the *Index.cshtml* [`<form>` element] (https://developer.mozilla.org/docs/Web/HTML/Element/form) as highlighted in the following markup:</span></span>

   [!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

1. <span data-ttu-id="f55fb-161">Przetestuj aplikację, wyszukując według gatunku, tytułu filmu i obu tych elementów.</span><span class="sxs-lookup"><span data-stu-id="f55fb-161">Test the app by searching by genre, by movie title, and by both.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f55fb-162">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f55fb-162">Additional resources</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f55fb-163">[Poprzedni: aktualizowanie stron](xref:tutorials/razor-pages/da1) 
>  [Dalej: Dodaj nowe pole](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="f55fb-163">[Previous: Update the pages](xref:tutorials/razor-pages/da1)
[Next: Add a new field](xref:tutorials/razor-pages/new-field)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f55fb-164">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/tutorials/razor-pages/razor-pages-start) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f55fb-164">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/tutorials/razor-pages/razor-pages-start) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="f55fb-165">W poniższych sekcjach są dodawane przeszukiwania filmów według *gatunku* lub *nazwy* .</span><span class="sxs-lookup"><span data-stu-id="f55fb-165">In the following sections, searching movies by *genre* or *name* is added.</span></span>

<span data-ttu-id="f55fb-166">Dodaj następujące wyróżnione właściwości do *stron/filmów/ Index . cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f55fb-166">Add the following highlighted properties to *Pages/Movies/Index.cshtml.cs*:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_newProps&highlight=11-999)]

* <span data-ttu-id="f55fb-167">`SearchString`: Zawiera tekst wprowadzany przez użytkowników w polu tekstowym Wyszukaj.</span><span class="sxs-lookup"><span data-stu-id="f55fb-167">`SearchString`: Contains the text users enter in the search text box.</span></span> <span data-ttu-id="f55fb-168">`SearchString` ma [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) atrybut.</span><span class="sxs-lookup"><span data-stu-id="f55fb-168">`SearchString` has the [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute.</span></span> <span data-ttu-id="f55fb-169">`[BindProperty]` tworzy powiązanie wartości formularzy i ciągów zapytań o takiej samej nazwie jak właściwość.</span><span class="sxs-lookup"><span data-stu-id="f55fb-169">`[BindProperty]` binds form values and query strings with the same name as the property.</span></span> <span data-ttu-id="f55fb-170">`[BindProperty(SupportsGet = true)]` jest wymagany do powiązania żądań HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="f55fb-170">`[BindProperty(SupportsGet = true)]` is required for binding on HTTP GET requests.</span></span>
* <span data-ttu-id="f55fb-171">`Genres`: Zawiera listę gatunku.</span><span class="sxs-lookup"><span data-stu-id="f55fb-171">`Genres`: Contains the list of genres.</span></span> <span data-ttu-id="f55fb-172">`Genres` umożliwia użytkownikowi wybranie gatunku z listy.</span><span class="sxs-lookup"><span data-stu-id="f55fb-172">`Genres` allows the user to select a genre from the list.</span></span> <span data-ttu-id="f55fb-173">`SelectList` KONIECZN `using Microsoft.AspNetCore.Mvc.Rendering;`</span><span class="sxs-lookup"><span data-stu-id="f55fb-173">`SelectList` requires `using Microsoft.AspNetCore.Mvc.Rendering;`</span></span>
* <span data-ttu-id="f55fb-174">`MovieGenre`: Zawiera konkretny gatunek wybierany przez użytkownika.</span><span class="sxs-lookup"><span data-stu-id="f55fb-174">`MovieGenre`: Contains the specific genre the user selects.</span></span> <span data-ttu-id="f55fb-175">Na przykład "zachodnie".</span><span class="sxs-lookup"><span data-stu-id="f55fb-175">For example, "Western".</span></span>
* <span data-ttu-id="f55fb-176">`Genres` i `MovieGenre` są używane w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="f55fb-176">`Genres` and `MovieGenre` are used later in this tutorial.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="f55fb-177">Zaktualizuj Index `OnGetAsync` metodę strony przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="f55fb-177">Update the Index page's `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_1stSearch)]

<span data-ttu-id="f55fb-178">Pierwszy wiersz `OnGetAsync` metody tworzy zapytanie [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) do wybierania filmów:</span><span class="sxs-lookup"><span data-stu-id="f55fb-178">The first line of the `OnGetAsync` method creates a [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) query to select the movies:</span></span>

```csharp
// using System.Linq;
var movies = from m in _context.Movie
             select m;
```

<span data-ttu-id="f55fb-179">Zapytanie jest zdefiniowane *tylko* w tym momencie, **nie** zostało uruchomione względem bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f55fb-179">The query is *only* defined at this point, it has **not** been run against the database.</span></span>

<span data-ttu-id="f55fb-180">Jeśli `SearchString` Właściwość nie ma wartości null lub jest pusta, zapytanie o filmy jest modyfikowane w celu odfiltrowania ciągu wyszukiwania:</span><span class="sxs-lookup"><span data-stu-id="f55fb-180">If the `SearchString` property is not null or empty, the movies query is modified to filter on the search string:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchNull)]

<span data-ttu-id="f55fb-181">`s => s.Title.Contains()`Kod jest [wyrażeniem lambda](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span><span class="sxs-lookup"><span data-stu-id="f55fb-181">The `s => s.Title.Contains()` code is a [Lambda Expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="f55fb-182">Wyrażenia lambda są używane w kwerendach [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) opartych na metodach jako argumenty dla standardowych metod operatorów zapytań, takich jak Metoda [WHERE](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) lub `Contains` .</span><span class="sxs-lookup"><span data-stu-id="f55fb-182">Lambdas are used in method-based [LINQ](/dotnet/csharp/programming-guide/concepts/linq/) queries as arguments to standard query operator methods such as the [Where](/dotnet/csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq) method or `Contains`.</span></span> <span data-ttu-id="f55fb-183">Zapytania LINQ nie są wykonywane, gdy są zdefiniowane lub są modyfikowane przez wywołanie metody, takiej jak `Where` , `Contains`  lub `OrderBy` .</span><span class="sxs-lookup"><span data-stu-id="f55fb-183">LINQ queries are not executed when they're defined or when they're modified by calling a method, such as `Where`, `Contains`  or `OrderBy`.</span></span> <span data-ttu-id="f55fb-184">Zamiast tego wykonywanie zapytania jest odroczone.</span><span class="sxs-lookup"><span data-stu-id="f55fb-184">Rather, query execution is deferred.</span></span> <span data-ttu-id="f55fb-185">Obliczanie wyrażenia jest opóźnione do momentu przekroczenia jego prawdziwej wartości lub `ToListAsync` wywołania metody.</span><span class="sxs-lookup"><span data-stu-id="f55fb-185">The evaluation of an expression is delayed until its realized value is iterated over or the `ToListAsync` method is called.</span></span> <span data-ttu-id="f55fb-186">Aby uzyskać więcej informacji, zobacz [wykonywanie zapytań](/dotnet/framework/data/adonet/ef/language-reference/query-execution) .</span><span class="sxs-lookup"><span data-stu-id="f55fb-186">See [Query Execution](/dotnet/framework/data/adonet/ef/language-reference/query-execution) for more information.</span></span>

<span data-ttu-id="f55fb-187">**Uwaga:** Metoda [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) jest uruchamiana w bazie danych, a nie w kodzie C#.</span><span class="sxs-lookup"><span data-stu-id="f55fb-187">**Note:** The [Contains](/dotnet/api/system.data.objects.dataclasses.entitycollection-1.contains) method is run on the database, not in the C# code.</span></span> <span data-ttu-id="f55fb-188">Uwzględnianie wielkości liter w zapytaniu zależy od bazy danych i sortowania.</span><span class="sxs-lookup"><span data-stu-id="f55fb-188">The case sensitivity on the query depends on the database and the collation.</span></span> <span data-ttu-id="f55fb-189">Na SQL Server `Contains` mapuje do [programu SQL Server, np](/sql/t-sql/language-elements/like-transact-sql). bez uwzględniania wielkości liter.</span><span class="sxs-lookup"><span data-stu-id="f55fb-189">On SQL Server, `Contains` maps to [SQL LIKE](/sql/t-sql/language-elements/like-transact-sql), which is case insensitive.</span></span> <span data-ttu-id="f55fb-190">W ramach programu SQLite domyślne sortowanie uwzględnia wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="f55fb-190">In SQLite, with the default collation, it's case sensitive.</span></span>

<span data-ttu-id="f55fb-191">Przejdź do strony filmy i dołącz ciąg zapytania, taki jak `?searchString=Ghost` adres URL.</span><span class="sxs-lookup"><span data-stu-id="f55fb-191">Navigate to the Movies page and append a query string such as `?searchString=Ghost` to the URL.</span></span> <span data-ttu-id="f55fb-192">Na przykład `https://localhost:5001/Movies?searchString=Ghost`.</span><span class="sxs-lookup"><span data-stu-id="f55fb-192">For example, `https://localhost:5001/Movies?searchString=Ghost`.</span></span> <span data-ttu-id="f55fb-193">Wyświetlane są filtrowane filmy.</span><span class="sxs-lookup"><span data-stu-id="f55fb-193">The filtered movies are displayed.</span></span>

![::: No-Loc (indeks)::: View](search/_static/ghost.png)

<span data-ttu-id="f55fb-195">Jeśli do strony zostanie dodany następujący szablon trasy Index , ciąg wyszukiwania może zostać przekierowany jako segment adresu URL.</span><span class="sxs-lookup"><span data-stu-id="f55fb-195">If the following route template is added to the Index page, the search string can be passed as a URL segment.</span></span> <span data-ttu-id="f55fb-196">Na przykład `https://localhost:5001/Movies/Ghost`.</span><span class="sxs-lookup"><span data-stu-id="f55fb-196">For example, `https://localhost:5001/Movies/Ghost`.</span></span>

```cshtml
@page "{searchString?}"
```

<span data-ttu-id="f55fb-197">Powyższe ograniczenie trasy umożliwia przeszukiwanie tytułu jako dane trasy (segment adresu URL), a nie jako wartość ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="f55fb-197">The preceding route constraint allows searching the title as route data (a URL segment) instead of as a query string value.</span></span>  <span data-ttu-id="f55fb-198">`?`W `"{searchString?}"` tym przypadku jest to opcjonalny parametr trasy.</span><span class="sxs-lookup"><span data-stu-id="f55fb-198">The `?` in `"{searchString?}"` means this is an optional route parameter.</span></span>

![::: No-Loc (index)::: View z wyrazem "Ghost" dodanym do adresu URL i zwrotną listą filmów dwóch filmów, Ghostbusters i Ghostbusters 2](search/_static/g2.png)

<span data-ttu-id="f55fb-200">Środowisko uruchomieniowe ASP.NET Core używa [powiązania modelu](xref:mvc/models/model-binding) , aby ustawić wartość `SearchString` właściwości z ciągu zapytania ( `?searchString=Ghost` ) lub danych trasy ( `https://localhost:5001/Movies/Ghost` ).</span><span class="sxs-lookup"><span data-stu-id="f55fb-200">The ASP.NET Core runtime uses [model binding](xref:mvc/models/model-binding) to set the value of the `SearchString` property from the query string (`?searchString=Ghost`) or route data (`https://localhost:5001/Movies/Ghost`).</span></span> <span data-ttu-id="f55fb-201">W powiązaniu modelu ***nie*** jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="f55fb-201">Model binding is ***not*** case sensitive.</span></span>

<span data-ttu-id="f55fb-202">Jednak użytkownicy nie mogą zmieniać adresu URL w celu wyszukania filmu.</span><span class="sxs-lookup"><span data-stu-id="f55fb-202">However, users can't be expected to modify the URL to search for a movie.</span></span> <span data-ttu-id="f55fb-203">W tym kroku zostanie dodany interfejs użytkownika do filtrowania filmów.</span><span class="sxs-lookup"><span data-stu-id="f55fb-203">In this step, UI is added to filter movies.</span></span> <span data-ttu-id="f55fb-204">Jeśli dodano ograniczenie trasy `"{searchString?}"` , usuń je.</span><span class="sxs-lookup"><span data-stu-id="f55fb-204">If you added the route constraint `"{searchString?}"`, remove it.</span></span>

<span data-ttu-id="f55fb-205">Otwórz plik *Pages/Films/ Index . cshtml* i Dodaj `<form>` znaczniki wyróżnione w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="f55fb-205">Open the *Pages/Movies/Index.cshtml* file, and add the `<form>` markup highlighted in the following code:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index2.cshtml?highlight=14-19&range=1-22)]

<span data-ttu-id="f55fb-206">Tag HTML `<form>` używa następujących [pomocników tagów](xref:mvc/views/tag-helpers/intro):</span><span class="sxs-lookup"><span data-stu-id="f55fb-206">The HTML `<form>` tag uses the following [Tag Helpers](xref:mvc/views/tag-helpers/intro):</span></span>

* <span data-ttu-id="f55fb-207">[Pomocnik tagu formularza](xref:mvc/views/working-with-forms#the-form-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="f55fb-207">[Form Tag Helper](xref:mvc/views/working-with-forms#the-form-tag-helper).</span></span> <span data-ttu-id="f55fb-208">Gdy formularz zostanie przesłany, ciąg filtru jest wysyłany do *stron/filmów/ Index* stron za pośrednictwem ciągu zapytania.</span><span class="sxs-lookup"><span data-stu-id="f55fb-208">When the form is submitted, the filter string is sent to the *Pages/Movies/Index* page via query string.</span></span>
* [<span data-ttu-id="f55fb-209">Pomocnik tagu wejściowego</span><span class="sxs-lookup"><span data-stu-id="f55fb-209">Input Tag Helper</span></span>](xref:mvc/views/working-with-forms#the-input-tag-helper)

<span data-ttu-id="f55fb-210">Zapisz zmiany i przetestuj filtr.</span><span class="sxs-lookup"><span data-stu-id="f55fb-210">Save the changes and test the filter.</span></span>

![::: No-Loc (index)::: View z wyrazem Ghost wpisanych do pola tekstowego filtru tytułu](search/_static/filter.png)

## <a name="search-by-genre"></a><span data-ttu-id="f55fb-212">Wyszukaj według gatunku</span><span class="sxs-lookup"><span data-stu-id="f55fb-212">Search by genre</span></span>

<span data-ttu-id="f55fb-213">Zaktualizuj `OnGetAsync` metodę przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="f55fb-213">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SearchGenre)]

<span data-ttu-id="f55fb-214">Poniższy kod jest zapytanie LINQ, które pobiera wszystkie gatunki z bazy danych.</span><span class="sxs-lookup"><span data-stu-id="f55fb-214">The following code is a LINQ query that retrieves all the genres from the database.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_LINQ)]

<span data-ttu-id="f55fb-215">`SelectList`Gatunek jest tworzony przez projekcję odrębnych gatuneków.</span><span class="sxs-lookup"><span data-stu-id="f55fb-215">The `SelectList` of genres is created by projecting the distinct genres.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Index.cshtml.cs?name=snippet_SelectList)]

### <a name="add-search-by-genre-to-the-razor-page"></a><span data-ttu-id="f55fb-216">Dodaj wyszukiwanie według gatunku na Razor stronie</span><span class="sxs-lookup"><span data-stu-id="f55fb-216">Add search by genre to the Razor Page</span></span>

<span data-ttu-id="f55fb-217">Aktualizowanie *Index . cshtml* [ `<form>` element] ( https://developer.mozilla.org/docs/Web/HTML/Element/form) jak wyróżniono w następującej adjustacji):</span><span class="sxs-lookup"><span data-stu-id="f55fb-217">Update the *Index.cshtml* [`<form>` element] (https://developer.mozilla.org/docs/Web/HTML/Element/form) as highlighted in the following markup:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexFormGenreNoRating.cshtml?highlight=16-18&range=1-26)]

<span data-ttu-id="f55fb-218">Przetestuj aplikację, wyszukując według gatunku, tytułu filmu i obu tych elementów.</span><span class="sxs-lookup"><span data-stu-id="f55fb-218">Test the app by searching by genre, by movie title, and by both.</span></span>
<span data-ttu-id="f55fb-219">Powyższy kod używa pomocnika [SELECT tag](xref:mvc/views/working-with-forms#the-select-tag-helper) i znacznika opcji.</span><span class="sxs-lookup"><span data-stu-id="f55fb-219">The preceding code uses the [Select Tag Helper](xref:mvc/views/working-with-forms#the-select-tag-helper) and Option Tag Helper.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f55fb-220">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="f55fb-220">Additional resources</span></span>

* [<span data-ttu-id="f55fb-221">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="f55fb-221">YouTube version of this tutorial</span></span>](https://youtu.be/4B6pHtdyo08)

> [!div class="step-by-step"]
> <span data-ttu-id="f55fb-222">[Poprzedni: aktualizowanie stron](xref:tutorials/razor-pages/da1) 
>  [Dalej: Dodaj nowe pole](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="f55fb-222">[Previous: Update the pages](xref:tutorials/razor-pages/da1)
[Next: Add a new field](xref:tutorials/razor-pages/new-field)</span></span>

::: moniker-end
