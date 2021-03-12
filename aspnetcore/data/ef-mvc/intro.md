---
title: 'Samouczek: wprowadzenie do EF Core w aplikacji sieci Web ASP.NET MVC'
description: Ta strona jest pierwszą częścią serii samouczków, które wyjaśniają, jak skompilować przykładową aplikację Dr/MVC firmy Contoso.
author: rick-anderson
ms.author: riande
ms.custom: mvc
ms.date: 11/06/2020
ms.topic: tutorial
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
uid: data/ef-mvc/intro
ms.openlocfilehash: 5bb1a05f5e81730d0785c1d6a69a8cafb68f85ab
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102586686"
---
# <a name="tutorial-get-started-with-ef-core-in-an-aspnet-mvc-web-app"></a><span data-ttu-id="7e237-103">Samouczek: wprowadzenie do EF Core w aplikacji sieci Web ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="7e237-103">Tutorial: Get started with EF Core in an ASP.NET MVC web app</span></span>

<span data-ttu-id="7e237-104">Autorzy [Dykstra](https://github.com/tdykstra) i [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7e237-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-5.0"

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

<span data-ttu-id="7e237-105">Przykładowa aplikacja internetowa Contoso University pokazuje, jak utworzyć aplikację sieci Web ASP.NET Core MVC przy użyciu narzędzia Entity Framework (EF) Core i programu Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7e237-105">The Contoso University sample web app demonstrates how to create an ASP.NET Core MVC web app using Entity Framework (EF) Core and Visual Studio.</span></span>

<span data-ttu-id="7e237-106">Przykładowa aplikacja jest witryną internetową fikcyjnej firmy Contoso University.</span><span class="sxs-lookup"><span data-stu-id="7e237-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="7e237-107">Obejmuje to funkcje, takie jak przyjmowanie studentów, tworzenie kursu i przydziały instruktora.</span><span class="sxs-lookup"><span data-stu-id="7e237-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="7e237-108">Jest to pierwsza z szeregu samouczków, które opisują sposób tworzenia przykładowej aplikacji firmy Contoso University.</span><span class="sxs-lookup"><span data-stu-id="7e237-108">This is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7e237-109">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7e237-109">Prerequisites</span></span>

* <span data-ttu-id="7e237-110">Jeśli dopiero zaczynasz korzystać z ASP.NET Core MVC, przejdź przez serię samouczka [wprowadzenie do ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc) przed rozpoczęciem tego.</span><span class="sxs-lookup"><span data-stu-id="7e237-110">If you're new to ASP.NET Core MVC, go through the [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc) tutorial series before starting this one.</span></span>

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-5.0.md)]

## <a name="database-engines"></a><span data-ttu-id="7e237-111">Aparaty bazy danych</span><span class="sxs-lookup"><span data-stu-id="7e237-111">Database engines</span></span>

<span data-ttu-id="7e237-112">Instrukcje programu Visual Studio używają [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), wersji SQL Server Express, która działa tylko w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="7e237-112">The Visual Studio instructions use [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), a version of SQL Server Express that runs only on Windows.</span></span>

<!--
The Visual Studio Code instructions use [SQLite](https://www.sqlite.org/), a cross-platform database engine.

If you choose to use SQLite, download and install a third-party tool for managing and viewing a SQLite database, such as [DB Browser for SQLite](https://sqlitebrowser.org/).
-->

## <a name="solve-problems-and-troubleshoot"></a><span data-ttu-id="7e237-113">Rozwiązywanie problemów i rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="7e237-113">Solve problems and troubleshoot</span></span>

<span data-ttu-id="7e237-114">Jeśli wystąpi problem, którego nie można rozwiązać, można ogólnie znaleźć rozwiązanie, porównując kod z [ukończonym projektem](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/data/ef-mvc/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="7e237-114">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/data/ef-mvc/intro/samples).</span></span> <span data-ttu-id="7e237-115">Aby zapoznać się z listą typowych błędów i sposobu ich rozwiązywania, zobacz [sekcję Rozwiązywanie problemów w ostatnim samouczku w serii](advanced.md#common-errors).</span><span class="sxs-lookup"><span data-stu-id="7e237-115">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="7e237-116">Jeśli nie możesz znaleźć tego, czego potrzebujesz, możesz ogłosić pytanie do StackOverflow.com dla [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="7e237-116">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="7e237-117">Jest to seria 10 samouczków, z których każdy jest oparty na tym, co zostało zrobione we wcześniejszych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="7e237-117">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="7e237-118">Rozważ zapisanie kopii projektu po każdym pomyślnym zakończeniu samouczka.</span><span class="sxs-lookup"><span data-stu-id="7e237-118">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="7e237-119">Jeśli wystąpią problemy, możesz zacząć od poprzedniego samouczka zamiast wrócić do początku całej serii.</span><span class="sxs-lookup"><span data-stu-id="7e237-119">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="contoso-university-web-app"></a><span data-ttu-id="7e237-120">Aplikacja sieci Web firmy Contoso University</span><span class="sxs-lookup"><span data-stu-id="7e237-120">Contoso University web app</span></span>

<span data-ttu-id="7e237-121">Aplikacja skompilowana w tych samouczkach jest podstawową szkołą w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7e237-121">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="7e237-122">Użytkownicy mogą wyświetlać i aktualizować informacje dotyczące uczniów, kursów i instruktorów.</span><span class="sxs-lookup"><span data-stu-id="7e237-122">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="7e237-123">Oto kilka ekranów w aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7e237-123">Here are a few of the screens in the app:</span></span>

![Strona indeksu uczniów](intro/_static/students-index.png)

![Strona edycji uczniów](intro/_static/student-edit.png)

## <a name="create-web-app"></a><span data-ttu-id="7e237-126">Tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="7e237-126">Create web app</span></span>

1. <span data-ttu-id="7e237-127">Uruchom program Visual Studio i wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="7e237-127">Start Visual Studio and select **Create a new project**.</span></span>
1. <span data-ttu-id="7e237-128">W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **ASP.NET Core aplikacja sieci Web** > **dalej**.</span><span class="sxs-lookup"><span data-stu-id="7e237-128">In the **Create a new project** dialog, select **ASP.NET Core Web Application** > **Next**.</span></span>
1. <span data-ttu-id="7e237-129">W oknie dialogowym **Konfigurowanie nowego projektu** wprowadź wartość `ContosoUniversity` w polu **Nazwa projektu**.</span><span class="sxs-lookup"><span data-stu-id="7e237-129">In the **Configure your new project** dialog, enter `ContosoUniversity` for **Project name**.</span></span> <span data-ttu-id="7e237-130">Ważne jest, aby użyć tej dokładnej nazwy, z uwzględnieniem wielkich liter, więc każdy pasuje do tego, `namespace` kiedy kod jest kopiowany.</span><span class="sxs-lookup"><span data-stu-id="7e237-130">It's important to use this exact name including capitalization, so each `namespace` matches when code is copied.</span></span>
1. <span data-ttu-id="7e237-131">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="7e237-131">Select **Create**.</span></span>
1. <span data-ttu-id="7e237-132">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** wybierz:</span><span class="sxs-lookup"><span data-stu-id="7e237-132">In the **Create a new ASP.NET Core web application** dialog, select:</span></span>
    1. <span data-ttu-id="7e237-133">**Platformy .NET Core** i **ASP.NET Core 5,0** na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="7e237-133">**.NET Core** and **ASP.NET Core 5.0** in the dropdowns.</span></span>
    1. <span data-ttu-id="7e237-134">**ASP.NET Core aplikacji sieci Web (Model-View-Controller)**.</span><span class="sxs-lookup"><span data-stu-id="7e237-134">**ASP.NET Core Web App (Model-View-Controller)**.</span></span>
    1. <span data-ttu-id="7e237-135"> 
       Utwórz ![ Nowe okno dialogowe projektu ASP.NET Core](~/data/ef-mvc/intro/_static/new-aspnet5.png)</span><span class="sxs-lookup"><span data-stu-id="7e237-135">**Create**
![New ASP.NET Core Project dialog](~/data/ef-mvc/intro/_static/new-aspnet5.png)</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="7e237-136">Konfigurowanie stylu witryny</span><span class="sxs-lookup"><span data-stu-id="7e237-136">Set up the site style</span></span>

<span data-ttu-id="7e237-137">Kilka podstawowych zmian powoduje skonfigurowanie menu witryny, układu i strony głównej.</span><span class="sxs-lookup"><span data-stu-id="7e237-137">A few basic changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="7e237-138">Otwórz *Widok widoki/Shared/_Layout. cshtml* i wprowadź następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="7e237-138">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="7e237-139">Zmień każde wystąpienie elementu `ContosoUniversity` na `Contoso University` .</span><span class="sxs-lookup"><span data-stu-id="7e237-139">Change each occurrence of `ContosoUniversity` to `Contoso University`.</span></span> <span data-ttu-id="7e237-140">Istnieją trzy wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="7e237-140">There are three occurrences.</span></span>
* <span data-ttu-id="7e237-141">Dodaj pozycje menu dla **informacji o** programie, **studentów**, **kursy**, **Instruktorzy** i **działy**, a następnie usuń wpis menu **prywatność** .</span><span class="sxs-lookup"><span data-stu-id="7e237-141">Add menu entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Privacy** menu entry.</span></span>

<span data-ttu-id="7e237-142">Powyższe zmiany są wyróżnione w następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="7e237-142">The preceding changes are highlighted in the following code:</span></span>

[!code-cshtml[](intro/samples/5cu/Views/Shared/_Layout.cshtml?highlight=6,24-38,52)]

<span data-ttu-id="7e237-143">W obszarze *widoki/Home/index. cshtml* Zastąp zawartość pliku następującym znacznikiem:</span><span class="sxs-lookup"><span data-stu-id="7e237-143">In *Views/Home/Index.cshtml*, replace the contents of the file with the following markup:</span></span>

[!code-cshtml[](intro/samples/5cu/Views/Home/Index.cshtml)]

<span data-ttu-id="7e237-144">Naciśnij klawisze CTRL + F5, aby uruchomić projekt, lub wybierz polecenie **debuguj > Uruchom bez debugowania** z menu.</span><span class="sxs-lookup"><span data-stu-id="7e237-144">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="7e237-145">Strona główna jest wyświetlana z kartami dla stron utworzonych w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="7e237-145">The home page is displayed with tabs for the pages created in this tutorial.</span></span>

![Strona główna firmy Contoso University](intro/_static/home-page5.png)

## <a name="ef-core-nuget-packages"></a><span data-ttu-id="7e237-147">EF Core pakietów NuGet</span><span class="sxs-lookup"><span data-stu-id="7e237-147">EF Core NuGet packages</span></span>

<span data-ttu-id="7e237-148">Ten samouczek używa SQL Server, a pakiet dostawcy to [Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="7e237-148">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

<span data-ttu-id="7e237-149">Pakiet EF SQL Server i jego zależności `Microsoft.EntityFrameworkCore` oraz `Microsoft.EntityFrameworkCore.Relational` zapewnia obsługę środowiska uruchomieniowego dla EF.</span><span class="sxs-lookup"><span data-stu-id="7e237-149">The EF SQL Server package and its dependencies, `Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`, provide runtime support for EF.</span></span>

<span data-ttu-id="7e237-150">Dodaj pakiet NuGet [Microsoft. AspNetCore. Diagnostics. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore) oraz pakiet NuGet [Microsoft. AspNetCore. Diagnostics. EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore) .</span><span class="sxs-lookup"><span data-stu-id="7e237-150">Add the [Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore) NuGet package and the [Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore) NuGet package.</span></span> <span data-ttu-id="7e237-151">W konsoli Menedżera pakietów (PMC) wprowadź następujące polecenia, aby dodać pakiety NuGet:</span><span class="sxs-lookup"><span data-stu-id="7e237-151">In the Package Manager Console (PMC), enter the following commands to add the NuGet packages:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="7e237-152">`Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore`Pakiet NuGet zawiera ASP.NET Core oprogramowanie pośredniczące dla EF Core stron błędów.</span><span class="sxs-lookup"><span data-stu-id="7e237-152">The `Microsoft.AspNetCore.Diagnostics.EntityFrameworkCore` NuGet package provides ASP.NET Core middleware for EF Core error pages.</span></span> <span data-ttu-id="7e237-153">To oprogramowanie pośredniczące pomaga wykrywać i diagnozować błędy za pomocą migracji EF Core.</span><span class="sxs-lookup"><span data-stu-id="7e237-153">This middleware helps to detect and diagnose errors with EF Core migrations.</span></span>

<span data-ttu-id="7e237-154">Aby uzyskać informacje o innych dostawcach baz danych, które są dostępne dla EF Core, zobacz [dostawcy bazy danych](/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="7e237-154">For information about other database providers that are available for EF Core, see [Database providers](/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="7e237-155">Tworzenie modelu danych</span><span class="sxs-lookup"><span data-stu-id="7e237-155">Create the data model</span></span>

<span data-ttu-id="7e237-156">Dla tej aplikacji są tworzone następujące klasy jednostek:</span><span class="sxs-lookup"><span data-stu-id="7e237-156">The following entity classes are created for this app:</span></span>

![Kurs — Diagram modelu danych ucznia](intro/_static/data-model-diagram.png)

<span data-ttu-id="7e237-158">Poprzednie jednostki mają następujące relacje:</span><span class="sxs-lookup"><span data-stu-id="7e237-158">The preceding entities have the following relationships:</span></span>

* <span data-ttu-id="7e237-159">Relacja jeden do wielu między elementami `Student` i `Enrollment` .</span><span class="sxs-lookup"><span data-stu-id="7e237-159">A one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="7e237-160">Student może być zarejestrowany w dowolnej liczbie kursów.</span><span class="sxs-lookup"><span data-stu-id="7e237-160">A student can be enrolled in any number of courses.</span></span>
* <span data-ttu-id="7e237-161">Relacja jeden do wielu między elementami `Course` i `Enrollment` .</span><span class="sxs-lookup"><span data-stu-id="7e237-161">A one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="7e237-162">Kurs może mieć dowolną liczbę uczniów zarejestrowanych w tym obszarze.</span><span class="sxs-lookup"><span data-stu-id="7e237-162">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="7e237-163">W poniższych sekcjach jest tworzona Klasa dla każdej z tych jednostek.</span><span class="sxs-lookup"><span data-stu-id="7e237-163">In the following sections, a class is created for each of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="7e237-164">Jednostka ucznia</span><span class="sxs-lookup"><span data-stu-id="7e237-164">The Student entity</span></span>

![Diagram jednostek uczniów](intro/_static/student-entity.png)

<span data-ttu-id="7e237-166">W folderze *modele* Utwórz `Student` klasę o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="7e237-166">In the *Models* folder, create the `Student` class with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="7e237-167">`ID`Właściwość jest kolumną klucza podstawowego (**PK**) tabeli bazy danych, która odnosi się do tej klasy.</span><span class="sxs-lookup"><span data-stu-id="7e237-167">The `ID` property is the primary key (**PK**) column of the database table that corresponds to this class.</span></span> <span data-ttu-id="7e237-168">Domyślnie EF interpretuje właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="7e237-168">By default, EF interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="7e237-169">Na przykład klucz podstawowy może być nazwany, `StudentID` a nie `ID` .</span><span class="sxs-lookup"><span data-stu-id="7e237-169">For example, the PK could be named `StudentID` rather than `ID`.</span></span>

<span data-ttu-id="7e237-170">`Enrollments`Właściwość jest [właściwością nawigacji](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="7e237-170">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="7e237-171">Właściwości nawigacji zawierają inne jednostki, które są powiązane z tą jednostką.</span><span class="sxs-lookup"><span data-stu-id="7e237-171">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="7e237-172">`Enrollments`Właściwość `Student` jednostki:</span><span class="sxs-lookup"><span data-stu-id="7e237-172">The `Enrollments` property of a `Student` entity:</span></span>

* <span data-ttu-id="7e237-173">Zawiera wszystkie `Enrollment` jednostki, które są powiązane z tą `Student` jednostką.</span><span class="sxs-lookup"><span data-stu-id="7e237-173">Contains all of the `Enrollment` entities that are related to that `Student` entity.</span></span>
* <span data-ttu-id="7e237-174">Jeśli konkretny `Student` wiersz w bazie danych ma dwa powiązane `Enrollment` wiersze:</span><span class="sxs-lookup"><span data-stu-id="7e237-174">If a specific `Student` row in the database has two related `Enrollment` rows:</span></span>
  * <span data-ttu-id="7e237-175">`Student` `Enrollments` Właściwość nawigacji tej jednostki zawiera te dwie `Enrollment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7e237-175">That `Student` entity's `Enrollments` navigation property contains those two `Enrollment` entities.</span></span>
  
<span data-ttu-id="7e237-176">`Enrollment` wiersze zawierają wartość klucza podstawowego ucznia w `StudentID` kolumnie klucz obcy (**FK**).</span><span class="sxs-lookup"><span data-stu-id="7e237-176">`Enrollment` rows contain a student's PK value in the `StudentID` foreign key (**FK**) column.</span></span>

<span data-ttu-id="7e237-177">Jeśli właściwość nawigacji może zawierać wiele jednostek:</span><span class="sxs-lookup"><span data-stu-id="7e237-177">If a navigation property can hold multiple entities:</span></span>

 * <span data-ttu-id="7e237-178">Typ musi być listą, taką jak `ICollection<T>` , `List<T>` , lub `HashSet<T>` .</span><span class="sxs-lookup"><span data-stu-id="7e237-178">The type must be a list, such as `ICollection<T>`, `List<T>`, or `HashSet<T>`.</span></span>
 * <span data-ttu-id="7e237-179">Jednostki można dodawać, usuwać i aktualizować.</span><span class="sxs-lookup"><span data-stu-id="7e237-179">Entities can be added, deleted, and updated.</span></span>

<span data-ttu-id="7e237-180">Relacje "wiele do wielu" i "jeden do wielu" mogą zawierać wiele jednostek.</span><span class="sxs-lookup"><span data-stu-id="7e237-180">Many-to-many and one-to-many navigation relationships can contain multiple entities.</span></span> <span data-ttu-id="7e237-181">Gdy `ICollection<T>` jest używany, EF domyślnie tworzy `HashSet<T>` kolekcję.</span><span class="sxs-lookup"><span data-stu-id="7e237-181">When `ICollection<T>` is used, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="7e237-182">Jednostka rejestracji</span><span class="sxs-lookup"><span data-stu-id="7e237-182">The Enrollment entity</span></span>

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

<span data-ttu-id="7e237-184">W folderze *modele* Utwórz `Enrollment` klasę o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="7e237-184">In the *Models* folder, create the `Enrollment` class with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="7e237-185">`EnrollmentID`Właściwość jest klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="7e237-185">The `EnrollmentID` property is the PK.</span></span> <span data-ttu-id="7e237-186">Ta jednostka używa `classnameID` wzorca zamiast `ID` samego siebie.</span><span class="sxs-lookup"><span data-stu-id="7e237-186">This entity uses the `classnameID` pattern instead of `ID` by itself.</span></span> <span data-ttu-id="7e237-187">`Student`Jednostka użyła `ID` wzorca.</span><span class="sxs-lookup"><span data-stu-id="7e237-187">The `Student` entity used the `ID` pattern.</span></span> <span data-ttu-id="7e237-188">Niektórzy deweloperzy wolą używać jednego wzorca w całym modelu danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-188">Some developers prefer to use one pattern throughout the data model.</span></span> <span data-ttu-id="7e237-189">W tym samouczku odmiana pokazuje, że można użyć wzorca.</span><span class="sxs-lookup"><span data-stu-id="7e237-189">In this tutorial, the variation illustrates that either pattern can be used.</span></span> <span data-ttu-id="7e237-190">W tym [samouczku](inheritance.md) pokazano, jak używać `ID` bez ClassName, ułatwia implementowanie dziedziczenia w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-190">A [later tutorial](inheritance.md) shows how using `ID` without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="7e237-191">`Grade`Właściwość jest `enum` .</span><span class="sxs-lookup"><span data-stu-id="7e237-191">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="7e237-192">`?`Po `Grade` deklaracji typu wskazuje, że `Grade` Właściwość [dopuszcza wartość null](/dotnet/csharp/language-reference/builtin-types/nullable-value-types).</span><span class="sxs-lookup"><span data-stu-id="7e237-192">The `?` after the `Grade` type declaration indicates that the `Grade` property is [nullable](/dotnet/csharp/language-reference/builtin-types/nullable-value-types).</span></span> <span data-ttu-id="7e237-193">Klasa, która `null` jest różna od zerowej klasy.</span><span class="sxs-lookup"><span data-stu-id="7e237-193">A grade that's `null` is different from a zero grade.</span></span> <span data-ttu-id="7e237-194">`null` oznacza, że Klasa nie jest znana lub nie została jeszcze przypisana.</span><span class="sxs-lookup"><span data-stu-id="7e237-194">`null` means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="7e237-195">`StudentID`Właściwość jest kluczem obcym (FK), a odpowiednia właściwość nawigacji to `Student` .</span><span class="sxs-lookup"><span data-stu-id="7e237-195">The `StudentID` property is a foreign key (FK), and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="7e237-196">`Enrollment`Jednostka jest skojarzona z jedną `Student` jednostką, więc właściwość może zawierać tylko jedną `Student` jednostkę.</span><span class="sxs-lookup"><span data-stu-id="7e237-196">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity.</span></span> <span data-ttu-id="7e237-197">Różni się to od `Student.Enrollments` właściwości nawigacji, która może zawierać wiele `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="7e237-197">This differs from the `Student.Enrollments` navigation property, which can hold multiple `Enrollment` entities.</span></span>

<span data-ttu-id="7e237-198">`CourseID`Właściwość jest FK, a odpowiednia właściwość nawigacji to `Course` .</span><span class="sxs-lookup"><span data-stu-id="7e237-198">The `CourseID` property is a FK, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="7e237-199">`Enrollment`Jednostka jest skojarzona z jedną `Course` jednostką.</span><span class="sxs-lookup"><span data-stu-id="7e237-199">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="7e237-200">Entity Framework interpretuje właściwość jako właściwość klucza obcego, jeśli ma nazwę właściwości `<` nawigacyjnej nazwa `><` właściwości klucza podstawowego `>` .</span><span class="sxs-lookup"><span data-stu-id="7e237-200">Entity Framework interprets a property as a FK property if it's named `<`navigation property name`><`primary key property name`>`.</span></span> <span data-ttu-id="7e237-201">Na przykład `StudentID` dla `Student` właściwości nawigacji, ponieważ `Student` klucz podstawowy jednostki to `ID` .</span><span class="sxs-lookup"><span data-stu-id="7e237-201">For example, `StudentID` for the `Student` navigation property since the `Student` entity's PK is `ID`.</span></span> <span data-ttu-id="7e237-202">Właściwości klucza obcego mogą również mieć  `<` nazwę właściwości klucz podstawowy `>` .</span><span class="sxs-lookup"><span data-stu-id="7e237-202">FK properties can also be named  `<`primary key property name`>`.</span></span> <span data-ttu-id="7e237-203">Na przykład, `CourseID` ponieważ `Course` klucz podstawowy jednostki to `CourseID` .</span><span class="sxs-lookup"><span data-stu-id="7e237-203">For example, `CourseID` because the `Course` entity's PK is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="7e237-204">Jednostka kursu</span><span class="sxs-lookup"><span data-stu-id="7e237-204">The Course entity</span></span>

![Diagram jednostek kursu](intro/_static/course-entity.png)

<span data-ttu-id="7e237-206">W folderze *modele* Utwórz `Course` klasę o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="7e237-206">In the *Models* folder, create the `Course` class with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="7e237-207">`Enrollments`Właściwość jest właściwością nawigacji.</span><span class="sxs-lookup"><span data-stu-id="7e237-207">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="7e237-208">`Course`Jednostka może być powiązana z dowolną liczbą `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="7e237-208">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="7e237-209">Atrybut [DatabaseGenerated](xref:System.ComponentModel.DataAnnotations.DatabaseGeneratedAttribute) został wyjaśniony w [późniejszym samouczku](complex-data-model.md).</span><span class="sxs-lookup"><span data-stu-id="7e237-209">The [DatabaseGenerated](xref:System.ComponentModel.DataAnnotations.DatabaseGeneratedAttribute) attribute is explained in a [later tutorial](complex-data-model.md).</span></span> <span data-ttu-id="7e237-210">Ten atrybut umożliwia wprowadzenie klucza podstawowego dla kursu, a nie jego wygenerowanie.</span><span class="sxs-lookup"><span data-stu-id="7e237-210">This attribute allows entering the PK for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="7e237-211">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="7e237-211">Create the database context</span></span>

<span data-ttu-id="7e237-212">Klasa główna, która koordynuje funkcje EF dla danego modelu danych, jest <xref:Microsoft.EntityFrameworkCore.DbContext> klasą kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-212">The main class that coordinates EF functionality for a given data model is the <xref:Microsoft.EntityFrameworkCore.DbContext> database context class.</span></span> <span data-ttu-id="7e237-213">Ta klasa jest tworzona przez wyprowadzanie z `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="7e237-213">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="7e237-214">`DbContext`Klasa pochodna określa, które jednostki są uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-214">The `DbContext` derived class specifies which entities are included in the data model.</span></span> <span data-ttu-id="7e237-215">Niektóre zachowania EF można dostosować.</span><span class="sxs-lookup"><span data-stu-id="7e237-215">Some EF behaviors can be customized.</span></span> <span data-ttu-id="7e237-216">W tym projekcie Klasa ma nazwę `SchoolContext` .</span><span class="sxs-lookup"><span data-stu-id="7e237-216">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="7e237-217">W folderze projektu Utwórz folder o nazwie `Data` .</span><span class="sxs-lookup"><span data-stu-id="7e237-217">In the project folder, create a folder named `Data`.</span></span>

<span data-ttu-id="7e237-218">W folderze *dane* Utwórz `SchoolContext` klasę o następującym kodzie:</span><span class="sxs-lookup"><span data-stu-id="7e237-218">In the *Data* folder create a `SchoolContext` class with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="7e237-219">Poprzedni kod tworzy `DbSet` Właściwość dla każdego zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="7e237-219">The preceding code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="7e237-220">W terminologii EF:</span><span class="sxs-lookup"><span data-stu-id="7e237-220">In EF terminology:</span></span>

* <span data-ttu-id="7e237-221">Zestaw jednostek zwykle odpowiada tabeli bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-221">An entity set typically corresponds to a database table.</span></span>
* <span data-ttu-id="7e237-222">Jednostka odnosi się do wiersza w tabeli.</span><span class="sxs-lookup"><span data-stu-id="7e237-222">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="7e237-223">`DbSet<Enrollment>`Instrukcje i `DbSet<Course>` mogą być pominięte i mogą działać tak samo.</span><span class="sxs-lookup"><span data-stu-id="7e237-223">The `DbSet<Enrollment>` and `DbSet<Course>` statements could be omitted and it would work the same.</span></span> <span data-ttu-id="7e237-224">EF uwzględni je niejawnie, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="7e237-224">EF would include them implicitly because:</span></span>

* <span data-ttu-id="7e237-225">`Student`Jednostka odwołuje się do `Enrollment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7e237-225">The `Student` entity references the `Enrollment` entity.</span></span>
* <span data-ttu-id="7e237-226">`Enrollment`Jednostka odwołuje się do `Course` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7e237-226">The `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="7e237-227">Po utworzeniu bazy danych EF tworzy tabele, które mają nazwy takie same jak `DbSet` nazwy właściwości.</span><span class="sxs-lookup"><span data-stu-id="7e237-227">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="7e237-228">Nazwy właściwości dla kolekcji są zwykle plural.</span><span class="sxs-lookup"><span data-stu-id="7e237-228">Property names for collections are typically plural.</span></span> <span data-ttu-id="7e237-229">Na przykład `Students` zamiast `Student` .</span><span class="sxs-lookup"><span data-stu-id="7e237-229">For example, `Students` rather than `Student`.</span></span> <span data-ttu-id="7e237-230">Deweloperzy nie zgadzają się, czy nazwy tabel powinny być w liczbie mnogiej.</span><span class="sxs-lookup"><span data-stu-id="7e237-230">Developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="7e237-231">W przypadku tych samouczków domyślne zachowanie jest przesłonięte przez określenie pojedynczej nazwy tabeli w `DbContext` .</span><span class="sxs-lookup"><span data-stu-id="7e237-231">For these tutorials, the default behavior is overridden by specifying singular table names in the `DbContext`.</span></span> <span data-ttu-id="7e237-232">Aby to zrobić, Dodaj następujący wyróżniony kod po ostatniej właściwości Nieogólnymi.</span><span class="sxs-lookup"><span data-stu-id="7e237-232">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-schoolcontext"></a><span data-ttu-id="7e237-233">Zarejestruj `SchoolContext`</span><span class="sxs-lookup"><span data-stu-id="7e237-233">Register the `SchoolContext`</span></span>

<span data-ttu-id="7e237-234">ASP.NET Core obejmuje [iniekcję zależności](../../fundamentals/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="7e237-234">ASP.NET Core includes [dependency injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="7e237-235">Usługi, takie jak kontekst bazy danych EF, są rejestrowane z iniekcją zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7e237-235">Services, such as the EF database context, are registered with dependency injection during app startup.</span></span> <span data-ttu-id="7e237-236">Składniki, które wymagają tych usług, takich jak kontrolery MVC, są dostarczane przez parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="7e237-236">Components that require these services, such as MVC controllers, are provided these services via constructor parameters.</span></span> <span data-ttu-id="7e237-237">Kod konstruktora kontrolera, który pobiera wystąpienie kontekstu, jest przedstawiony w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="7e237-237">The controller constructor code that gets a context instance is shown later in this tutorial.</span></span>

<span data-ttu-id="7e237-238">Aby zarejestrować się `SchoolContext` jako usługa, otwórz *Startup.cs* i Dodaj wyróżnione wiersze do `ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="7e237-238">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/5cu-snap/Startup.cs?name=snippet&highlight=1-2,22-23)]

<span data-ttu-id="7e237-239">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w `DbContextOptionsBuilder` obiekcie.</span><span class="sxs-lookup"><span data-stu-id="7e237-239">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="7e237-240">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="7e237-240">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="7e237-241">Otwórz *appsettings.json* plik i Dodaj parametry połączenia, jak pokazano w następującej postaci:</span><span class="sxs-lookup"><span data-stu-id="7e237-241">Open the *appsettings.json* file and add a connection string as shown in the following markup:</span></span>

[!code-json[](./intro/samples/5cu/appsettings1.json?highlight=2-4)]

### <a name="add-the-database-exception-filter"></a><span data-ttu-id="7e237-242">Dodaj filtr wyjątku bazy danych</span><span class="sxs-lookup"><span data-stu-id="7e237-242">Add the database exception filter</span></span>

<span data-ttu-id="7e237-243">Dodaj <xref:Microsoft.Extensions.DependencyInjection.DatabaseDeveloperPageExceptionFilterServiceExtensions.AddDatabaseDeveloperPageExceptionFilter%2A> do `ConfigureServices` , jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="7e237-243">Add <xref:Microsoft.Extensions.DependencyInjection.DatabaseDeveloperPageExceptionFilterServiceExtensions.AddDatabaseDeveloperPageExceptionFilter%2A> to `ConfigureServices` as shown in the following code:</span></span>

[!code-csharp[](intro/samples/5cu/Startup.cs?name=snippet&highlight=6)]

<span data-ttu-id="7e237-244">`AddDatabaseDeveloperPageExceptionFilter`Zapewnia przydatne informacje o błędzie w [środowisku programistycznym](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="7e237-244">The `AddDatabaseDeveloperPageExceptionFilter` provides helpful error information in the [development environment](xref:fundamentals/environments).</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="7e237-245">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="7e237-245">SQL Server Express LocalDB</span></span>

<span data-ttu-id="7e237-246">Parametry połączenia określają [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span><span class="sxs-lookup"><span data-stu-id="7e237-246">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="7e237-247">LocalDB to uproszczona wersja aparatu bazy danych SQL Server Express i jest przeznaczona do tworzenia aplikacji, a nie do użycia w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="7e237-247">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="7e237-248">LocalDB uruchamia się na żądanie i działa w trybie użytkownika, więc nie istnieje złożona konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="7e237-248">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="7e237-249">Domyślnie LocalDB tworzy pliki *. mdf* DB w `C:/Users/<user>` katalogu.</span><span class="sxs-lookup"><span data-stu-id="7e237-249">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="initialize-db-with-test-data"></a><span data-ttu-id="7e237-250">Zainicjuj bazę danych z danymi testowymi</span><span class="sxs-lookup"><span data-stu-id="7e237-250">Initialize DB with test data</span></span>

<span data-ttu-id="7e237-251">EF tworzy pustą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-251">EF creates an empty database.</span></span> <span data-ttu-id="7e237-252">W tej sekcji dodano metodę, która jest wywoływana po utworzeniu bazy danych w celu wypełnienia jej danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="7e237-252">In this section, a method is added that's called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="7e237-253">`EnsureCreated`Metoda jest używana do automatycznego tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-253">The `EnsureCreated` method is used to automatically create the database.</span></span> <span data-ttu-id="7e237-254">W [późniejszym samouczku](migrations.md)zobaczysz, jak obsługiwać zmiany modelu przy użyciu migracje Code First, aby zmienić schemat bazy danych zamiast upuszczania i ponownego tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-254">In a [later tutorial](migrations.md), you see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="7e237-255">W folderze *dane* Utwórz nową klasę o nazwie `DbInitializer` przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="7e237-255">In the *Data* folder, create a new class named `DbInitializer` with the following code:</span></span>

[!code-csharp[DbInitializer](intro/samples/5cu-snap/DbInitializer.cs)]

<span data-ttu-id="7e237-256">Poprzedni kod sprawdza, czy baza danych istnieje:</span><span class="sxs-lookup"><span data-stu-id="7e237-256">The preceding code checks if the database exists:</span></span>

* <span data-ttu-id="7e237-257">Jeśli baza danych nie zostanie znaleziona,</span><span class="sxs-lookup"><span data-stu-id="7e237-257">If the database is not found;</span></span>
  * <span data-ttu-id="7e237-258">Jest on tworzony i ładowany z danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="7e237-258">It is created and loaded with test data.</span></span> <span data-ttu-id="7e237-259">Ładuje dane testowe do tablic, a nie `List<T>` kolekcji w celu zoptymalizowania wydajności.</span><span class="sxs-lookup"><span data-stu-id="7e237-259">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>
* <span data-ttu-id="7e237-260">Jeśli baza danych zostanie znaleziona, nie podejmuje żadnych działań.</span><span class="sxs-lookup"><span data-stu-id="7e237-260">If the database if found, it takes no action.</span></span>

<span data-ttu-id="7e237-261">Zaktualizuj *program.cs* przy użyciu następującego kodu:</span><span class="sxs-lookup"><span data-stu-id="7e237-261">Update *Program.cs* with the following code:</span></span>

[!code-csharp[Program file](intro/samples/5cu-snap/Program.cs?highlight=1-2,14-18,21-37)]

<span data-ttu-id="7e237-262">*Program.cs* wykonuje następujące czynności podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7e237-262">*Program.cs* does the following on app startup:</span></span>

* <span data-ttu-id="7e237-263">Pobierz wystąpienie kontekstu bazy danych z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="7e237-263">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="7e237-264">Wywołaj `DbInitializer.Initialize` metodę.</span><span class="sxs-lookup"><span data-stu-id="7e237-264">Call the `DbInitializer.Initialize` method.</span></span>
* <span data-ttu-id="7e237-265">Usuń kontekst, gdy `Initialize` Metoda zostanie ukończona, jak pokazano w poniższym kodzie:</span><span class="sxs-lookup"><span data-stu-id="7e237-265">Dispose the context when the `Initialize` method completes as shown in the following code:</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=5-18)]

<span data-ttu-id="7e237-266">Gdy aplikacja jest uruchamiana po raz pierwszy, baza danych zostanie utworzona i załadowana z danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="7e237-266">The first time the app is run, the database is created and loaded with test data.</span></span> <span data-ttu-id="7e237-267">Za każdym razem, gdy zmienia się model danych:</span><span class="sxs-lookup"><span data-stu-id="7e237-267">Whenever the data model changes:</span></span>

* <span data-ttu-id="7e237-268">Usuń bazę danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-268">Delete the database.</span></span>
* <span data-ttu-id="7e237-269">Zaktualizuj metodę inicjatora i zacznij od nowa bazę danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-269">Update the seed method, and start afresh with a new database.</span></span>

 <span data-ttu-id="7e237-270">W kolejnych samouczkach baza danych jest modyfikowana, gdy model danych ulegnie zmianie, bez usuwania i ponownego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="7e237-270">In later tutorials, the database is modified when the data model changes, without deleting and re-creating it.</span></span> <span data-ttu-id="7e237-271">Po zmianie modelu danych nie są tracone żadne dane.</span><span class="sxs-lookup"><span data-stu-id="7e237-271">No data is lost when the data model changes.</span></span>

## <a name="create-controller-and-views"></a><span data-ttu-id="7e237-272">Tworzenie kontrolera i widoków</span><span class="sxs-lookup"><span data-stu-id="7e237-272">Create controller and views</span></span>

<span data-ttu-id="7e237-273">Użyj aparatu tworzenia szkieletów w programie Visual Studio, aby dodać kontroler MVC i widoki, które będą używać EF do wykonywania zapytań i zapisywania danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-273">Use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="7e237-274">Automatyczne tworzenie metod i widoków akcji [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) jest znane jako rusztowania.</span><span class="sxs-lookup"><span data-stu-id="7e237-274">The automatic creation of [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) action methods and views is known as scaffolding.</span></span>

* <span data-ttu-id="7e237-275">W **Eksplorator rozwiązań** kliknij prawym przyciskiem myszy `Controllers` folder, a następnie wybierz pozycję **Dodaj > nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="7e237-275">In **Solution Explorer**, right-click the `Controllers` folder  and select **Add > New Scaffolded Item**.</span></span>
* <span data-ttu-id="7e237-276">W oknie dialogowym **Dodawanie szkieletu** :</span><span class="sxs-lookup"><span data-stu-id="7e237-276">In the **Add Scaffold** dialog box:</span></span>
  * <span data-ttu-id="7e237-277">Wybierz **kontroler MVC z widokami, używając Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="7e237-277">Select **MVC controller with views, using Entity Framework**.</span></span>
  * <span data-ttu-id="7e237-278">Kliknij pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="7e237-278">Click **Add**.</span></span> <span data-ttu-id="7e237-279">Pojawia się okno dialogowe **Dodawanie kontrolera MVC z widokami z użyciem Entity Framework** : ![ studenta dla szkieletu](intro/_static/scaffold-student2.png)</span><span class="sxs-lookup"><span data-stu-id="7e237-279">The **Add MVC Controller with views, using Entity Framework** dialog box appears: ![Scaffold Student](intro/_static/scaffold-student2.png)</span></span>
  * <span data-ttu-id="7e237-280">W obszarze **Klasa modelu** wybierz pozycję **student**.</span><span class="sxs-lookup"><span data-stu-id="7e237-280">In **Model class**, select **Student**.</span></span>
  * <span data-ttu-id="7e237-281">W obszarze **Klasa kontekstu danych** wybierz pozycję **SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="7e237-281">In **Data context class**, select **SchoolContext**.</span></span>
  * <span data-ttu-id="7e237-282">Zaakceptuj domyślną **StudentsController** jako nazwę.</span><span class="sxs-lookup"><span data-stu-id="7e237-282">Accept the default **StudentsController** as the name.</span></span>
  * <span data-ttu-id="7e237-283">Kliknij pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="7e237-283">Click **Add**.</span></span>

<span data-ttu-id="7e237-284">Aparat szkieletu programu Visual Studio tworzy `StudentsController.cs` plik i zestaw widoków ( `*.cshtml` plików), które współpracują z kontrolerem.</span><span class="sxs-lookup"><span data-stu-id="7e237-284">The Visual Studio scaffolding engine creates a `StudentsController.cs` file and a set of views (`*.cshtml` files) that work with the controller.</span></span>

<span data-ttu-id="7e237-285">Zwróć uwagę, że kontroler przyjmuje `SchoolContext` jako parametr konstruktora.</span><span class="sxs-lookup"><span data-stu-id="7e237-285">Notice the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="7e237-286">ASP.NET Core iniekcja zależności ma zadbać o przekazanie wystąpienia `SchoolContext` do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7e237-286">ASP.NET Core dependency injection takes care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="7e237-287">Skonfigurowano tę wartość w `Startup` klasie.</span><span class="sxs-lookup"><span data-stu-id="7e237-287">You configured that in the `Startup` class.</span></span>

<span data-ttu-id="7e237-288">Kontroler zawiera `Index` metodę akcji, która wyświetla wszystkich uczniów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-288">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="7e237-289">Metoda pobiera listę studentów z zestawu jednostek studentów, odczytując `Students` Właściwość wystąpienia kontekstu bazy danych:</span><span class="sxs-lookup"><span data-stu-id="7e237-289">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="7e237-290">Asynchroniczne elementy programowania w tym kodzie są wyjaśnione w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="7e237-290">The asynchronous programming elements in this code are explained later in the tutorial.</span></span>

<span data-ttu-id="7e237-291">Widok *widoki/uczniowie/index. cshtml* wyświetla tę listę w tabeli:</span><span class="sxs-lookup"><span data-stu-id="7e237-291">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="7e237-292">Naciśnij klawisze CTRL + F5, aby uruchomić projekt, lub wybierz polecenie **debuguj > Uruchom bez debugowania** z menu.</span><span class="sxs-lookup"><span data-stu-id="7e237-292">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="7e237-293">Kliknij kartę studenci, aby zobaczyć dane testowe, które zostały `DbInitializer.Initialize` wstawione przez metodę.</span><span class="sxs-lookup"><span data-stu-id="7e237-293">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="7e237-294">W zależności od tego, jak Zawężasz okno przeglądarki, zobaczysz `Students` link do karty w górnej części strony lub kliknij ikonę nawigacji w prawym górnym rogu, aby zobaczyć link.</span><span class="sxs-lookup"><span data-stu-id="7e237-294">Depending on how narrow your browser window is, you'll see the `Students` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Strona główna firmy Contoso University Narrow](intro/_static/home-page-narrow.png)

![Strona indeksu uczniów](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="7e237-297">Wyświetlanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="7e237-297">View the database</span></span>

<span data-ttu-id="7e237-298">Gdy aplikacja zostanie uruchomiona, `DbInitializer.Initialize` Metoda wywołuje metodę `EnsureCreated` .</span><span class="sxs-lookup"><span data-stu-id="7e237-298">When the app is started, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="7e237-299">EF wykryto, że nie ma bazy danych:</span><span class="sxs-lookup"><span data-stu-id="7e237-299">EF saw that there was no database:</span></span>

* <span data-ttu-id="7e237-300">W związku z tym utworzono bazę danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-300">So it created a database.</span></span>
* <span data-ttu-id="7e237-301">`Initialize`Kod metody wypełniał bazę danych danymi.</span><span class="sxs-lookup"><span data-stu-id="7e237-301">The `Initialize` method code populated the database with data.</span></span>

<span data-ttu-id="7e237-302">Użyj **Eksplorator obiektów SQL Server** (SSOX), aby wyświetlić bazę danych w programie Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="7e237-302">Use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio:</span></span>

* <span data-ttu-id="7e237-303">Wybierz **Eksplorator obiektów SQL Server** z menu **Widok** w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7e237-303">Select **SQL Server Object Explorer** from the **View** menu in Visual Studio.</span></span>
* <span data-ttu-id="7e237-304">W SSOX wybierz pozycję **(LocalDB) \MSSQLLocalDB > bazy danych**.</span><span class="sxs-lookup"><span data-stu-id="7e237-304">In SSOX, select **(localdb)\MSSQLLocalDB > Databases**.</span></span>
* <span data-ttu-id="7e237-305">Wybierz pozycję `ContosoUniversity1` Nazwa bazy danych, która znajduje się w parametrach połączenia w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="7e237-305">Select `ContosoUniversity1`, the entry for the database name that's in the connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="7e237-306">Rozwiń węzeł **tabele** , aby wyświetlić tabele w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-306">Expand the **Tables** node to see the tables in the database.</span></span>

![Tabele w SSOX](intro/_static/ssox-tables.png)

<span data-ttu-id="7e237-308">Kliknij prawym przyciskiem myszy tabelę **uczniów** i kliknij polecenie **Wyświetl dane** , aby wyświetlić dane w tabeli.</span><span class="sxs-lookup"><span data-stu-id="7e237-308">Right-click the **Student** table and click **View Data** to see the data in the table.</span></span>

![Tabela uczniów w SSOX](intro/_static/ssox-student-table.png)

<span data-ttu-id="7e237-310">`*.mdf` `*.ldf` Pliki bazy danych i znajdują się w folderze *C:\Users \\ \<username>* .</span><span class="sxs-lookup"><span data-stu-id="7e237-310">The `*.mdf` and `*.ldf` database files are in the *C:\Users\\\<username>* folder.</span></span>

<span data-ttu-id="7e237-311">Ponieważ `EnsureCreated` jest wywoływana w metodzie inicjatora, która jest uruchamiana podczas uruchamiania aplikacji, można:</span><span class="sxs-lookup"><span data-stu-id="7e237-311">Because `EnsureCreated` is called in the initializer method that runs on app start, you could:</span></span>

* <span data-ttu-id="7e237-312">Wprowadź zmianę w `Student` klasie.</span><span class="sxs-lookup"><span data-stu-id="7e237-312">Make a change to the `Student` class.</span></span>
* <span data-ttu-id="7e237-313">Usuń bazę danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-313">Delete the database.</span></span>
* <span data-ttu-id="7e237-314">Zatrzymaj, a następnie uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="7e237-314">Stop, then start the app.</span></span> <span data-ttu-id="7e237-315">Baza danych zostanie automatycznie utworzona w celu dopasowania do zmiany.</span><span class="sxs-lookup"><span data-stu-id="7e237-315">The database is automatically re-created to match the change.</span></span>

<span data-ttu-id="7e237-316">Na przykład, jeśli `EmailAddress` do klasy zostanie dodana Właściwość `Student` , Nowa `EmailAddress` kolumna w nowo utworzonej tabeli.</span><span class="sxs-lookup"><span data-stu-id="7e237-316">For example, if an `EmailAddress` property is added to the `Student` class, a new `EmailAddress` column in the re-created table.</span></span> <span data-ttu-id="7e237-317">Nie zostanie wyświetlona nowa właściwość widoku z klasą `EmailAddress` .</span><span class="sxs-lookup"><span data-stu-id="7e237-317">The view classed won't display the new `EmailAddress` property.</span></span>

## <a name="conventions"></a><span data-ttu-id="7e237-318">Konwencje</span><span class="sxs-lookup"><span data-stu-id="7e237-318">Conventions</span></span>

<span data-ttu-id="7e237-319">Ilość kodu zapisywana w celu utworzenia kompletnej bazy danych przez EF, jest minimalna ze względu na użycie konwencji EF korzysta z:</span><span class="sxs-lookup"><span data-stu-id="7e237-319">The amount of code written in order for the EF to to create a complete database is minimal because of the use of the conventions EF uses:</span></span>

* <span data-ttu-id="7e237-320">Nazwy `DbSet` właściwości są używane jako nazwy tabel.</span><span class="sxs-lookup"><span data-stu-id="7e237-320">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="7e237-321">W przypadku jednostek, do których nie odwołuje się `DbSet` Właściwość, nazwy klas jednostek są używane jako nazwy tabel.</span><span class="sxs-lookup"><span data-stu-id="7e237-321">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>
* <span data-ttu-id="7e237-322">Nazwy właściwości jednostki są używane w nazwach kolumn.</span><span class="sxs-lookup"><span data-stu-id="7e237-322">Entity property names are used for column names.</span></span>
* <span data-ttu-id="7e237-323">Właściwości jednostki o nazwach `ID` lub `classnameID` są rozpoznawane jako właściwości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="7e237-323">Entity properties that are named `ID` or `classnameID` are recognized as PK properties.</span></span>
* <span data-ttu-id="7e237-324">Właściwość jest interpretowana jako właściwość klucza obcego, jeśli ma nazwę właściwość `<` nawigacji nazwa `><` klucza podstawowego `>` .</span><span class="sxs-lookup"><span data-stu-id="7e237-324">A property is interpreted as a FK property if it's named `<`navigation property name`><`PK property name`>`.</span></span> <span data-ttu-id="7e237-325">Na przykład `StudentID` dla `Student` właściwości nawigacji, ponieważ `Student` klucz podstawowy jednostki to `ID` .</span><span class="sxs-lookup"><span data-stu-id="7e237-325">For example, `StudentID` for the `Student` navigation property since the `Student` entity's PK is `ID`.</span></span> <span data-ttu-id="7e237-326">Właściwości klucza obcego mogą również mieć `<` nazwę właściwości klucz podstawowy `>` .</span><span class="sxs-lookup"><span data-stu-id="7e237-326">FK properties can also be named `<`primary key property name`>`.</span></span> <span data-ttu-id="7e237-327">Na przykład, `EnrollmentID` ponieważ `Enrollment` klucz podstawowy jednostki to `EnrollmentID` .</span><span class="sxs-lookup"><span data-stu-id="7e237-327">For example, `EnrollmentID` since the `Enrollment` entity's PK is `EnrollmentID`.</span></span>

<span data-ttu-id="7e237-328">Zachowanie konwencjonalne można zastąpić.</span><span class="sxs-lookup"><span data-stu-id="7e237-328">Conventional behavior can be overridden.</span></span> <span data-ttu-id="7e237-329">Na przykład nazwy tabel można jawnie określić, jak pokazano wcześniej w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="7e237-329">For example, table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="7e237-330">Nazwy kolumn i dowolnej właściwości można ustawić jako PK lub FK.</span><span class="sxs-lookup"><span data-stu-id="7e237-330">Column names and any property can be set as a PK or FK.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="7e237-331">Kod asynchroniczny</span><span class="sxs-lookup"><span data-stu-id="7e237-331">Asynchronous code</span></span>

<span data-ttu-id="7e237-332">Programowanie asynchroniczne jest trybem domyślnym dla ASP.NET Core i EF Core.</span><span class="sxs-lookup"><span data-stu-id="7e237-332">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="7e237-333">Serwer sieci Web ma ograniczoną liczbę dostępnych wątków, a w przypadku dużego obciążenia mogą być używane wszystkie dostępne wątki.</span><span class="sxs-lookup"><span data-stu-id="7e237-333">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="7e237-334">W takim przypadku serwer nie może przetwarzać nowych żądań, dopóki wątki nie zostaną zwolnione.</span><span class="sxs-lookup"><span data-stu-id="7e237-334">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="7e237-335">W przypadku kodu synchronicznego wiele wątków może zostać powiązanych, podczas gdy nie wykonuje żadnej pracy, ponieważ oczekuje na ukończenie operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="7e237-335">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="7e237-336">W przypadku kodu asynchronicznego, gdy proces oczekuje na ukończenie operacji we/wy, jego wątek zostanie zwolniony dla serwera do użycia na potrzeby przetwarzania innych żądań.</span><span class="sxs-lookup"><span data-stu-id="7e237-336">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="7e237-337">W efekcie kod asynchroniczny umożliwia efektywniejsze wykorzystanie zasobów serwera, a serwer jest włączony do obsługi większej ilości ruchu bez opóźnień.</span><span class="sxs-lookup"><span data-stu-id="7e237-337">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="7e237-338">Kod asynchroniczny wprowadza niewielką ilość narzutu w czasie wykonywania, ale w przypadku niskiego natężenia ruchu, gdy wydajność jest niewielka, w przypadku dużych sytuacji związanych z ruchem jest istotna poprawa wydajności.</span><span class="sxs-lookup"><span data-stu-id="7e237-338">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="7e237-339">W poniższym kodzie,, `async` , `Task<T>` `await` i sprawić, że `ToListAsync` kod jest wykonywany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7e237-339">In the following code, `async`, `Task<T>`, `await`, and `ToListAsync` make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="7e237-340">`async`Słowo kluczowe instruuje kompilator, aby generował wywołania zwrotne dla części treści metody i automatycznie utworzyć `Task<IActionResult>` zwracany obiekt.</span><span class="sxs-lookup"><span data-stu-id="7e237-340">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that's returned.</span></span>
* <span data-ttu-id="7e237-341">Typ zwracany `Task<IActionResult>` reprezentuje bieżącą współpracę z wynikiem typu `IActionResult` .</span><span class="sxs-lookup"><span data-stu-id="7e237-341">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>
* <span data-ttu-id="7e237-342">`await`Słowo kluczowe powoduje, że kompilator dzieli metodę na dwie części.</span><span class="sxs-lookup"><span data-stu-id="7e237-342">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="7e237-343">Pierwsza część jest zakończona operacją uruchomioną asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7e237-343">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="7e237-344">Druga część jest umieszczana w metodzie wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.</span><span class="sxs-lookup"><span data-stu-id="7e237-344">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="7e237-345">`ToListAsync` jest asynchroniczną wersją `ToList` metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="7e237-345">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="7e237-346">Niektóre kwestie, o których należy pamiętać podczas pisania asynchronicznego kodu, który używa EF:</span><span class="sxs-lookup"><span data-stu-id="7e237-346">Some things to be aware of when  writing asynchronous code that uses EF:</span></span>

* <span data-ttu-id="7e237-347">Tylko instrukcje, które powodują, że zapytania lub polecenia wysyłane do bazy danych są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7e237-347">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="7e237-348">Obejmuje to, na przykład, `ToListAsync` , `SingleOrDefaultAsync` i `SaveChangesAsync` .</span><span class="sxs-lookup"><span data-stu-id="7e237-348">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="7e237-349">Nie zawiera na przykład instrukcji, które po prostu zmieniają element `IQueryable` , taki jak `var students = context.Students.Where(s => s.LastName == "Davolio")` .</span><span class="sxs-lookup"><span data-stu-id="7e237-349">It doesn't include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="7e237-350">Kontekst EF nie jest bezpieczny wątkowo: nie próbuj wykonać równolegle wielu operacji.</span><span class="sxs-lookup"><span data-stu-id="7e237-350">An EF context isn't thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="7e237-351">Gdy wywoływana jest metoda async EF, zawsze używaj `await` słowa kluczowego.</span><span class="sxs-lookup"><span data-stu-id="7e237-351">When you call any async EF method, always use the `await` keyword.</span></span>
* <span data-ttu-id="7e237-352">Aby skorzystać z zalet wydajności w kodzie asynchronicznym, należy się upewnić, że wszystkie pakiety biblioteczne używają również metody asynchronicznej, jeśli wywołają one wszelkie metod EF, które powodują wysłanie zapytań do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-352">To take advantage of the performance benefits of async code, make sure that any library packages used also use async if they call any EF methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="7e237-353">Aby uzyskać więcej informacji na temat programowania asynchronicznego w programie .NET, zobacz [asynchroniczne Omówienie](/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="7e237-353">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

## <a name="limit-entities-fetched"></a><span data-ttu-id="7e237-354">Pobrano limit liczby jednostek</span><span class="sxs-lookup"><span data-stu-id="7e237-354">Limit entities fetched</span></span>

<span data-ttu-id="7e237-355">Zobacz [zagadnienia dotyczące wydajności](xref:data/ef-rp/intro) , aby uzyskać informacje na temat ograniczania liczby lub jednostek zwróconych z zapytania.</span><span class="sxs-lookup"><span data-stu-id="7e237-355">See [Performance considerations](xref:data/ef-rp/intro) for information on limiting the number or entities returned from a query.</span></span>

<span data-ttu-id="7e237-356">Przejdź do następnego samouczka, aby dowiedzieć się, jak wykonywać podstawowe operacje CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie).</span><span class="sxs-lookup"><span data-stu-id="7e237-356">Advance to the next tutorial to learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7e237-357">Zaimplementuj podstawowe funkcje CRUD</span><span class="sxs-lookup"><span data-stu-id="7e237-357">Implement basic CRUD functionality</span></span>](crud.md)

::: moniker-end

::: moniker range="<= aspnetcore-3.1"

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

<span data-ttu-id="7e237-358">Przykładowa aplikacja internetowa Contoso University demonstruje sposób tworzenia aplikacji sieci Web ASP.NET Core 2,2 MVC przy użyciu Entity Framework (EF) Core 2,2 i Visual Studio 2017 lub 2019.</span><span class="sxs-lookup"><span data-stu-id="7e237-358">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.2 MVC web applications using Entity Framework (EF) Core 2.2 and Visual Studio 2017 or 2019.</span></span>

<span data-ttu-id="7e237-359">Ten samouczek nie został zaktualizowany do ASP.NET Core 3,1.</span><span class="sxs-lookup"><span data-stu-id="7e237-359">This tutorial has not been updated for ASP.NET Core 3.1.</span></span> <span data-ttu-id="7e237-360">Została zaktualizowana [ASP.NET Core 5,0](xref:data/ef-mvc/intro?view=aspnetcore-5.0).</span><span class="sxs-lookup"><span data-stu-id="7e237-360">It has been updated for [ASP.NET Core 5.0](xref:data/ef-mvc/intro?view=aspnetcore-5.0).</span></span>

<span data-ttu-id="7e237-361">Przykładowa aplikacja jest witryną internetową fikcyjnej firmy Contoso University.</span><span class="sxs-lookup"><span data-stu-id="7e237-361">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="7e237-362">Obejmuje to funkcje, takie jak przyjmowanie studentów, tworzenie kursu i przydziały instruktora.</span><span class="sxs-lookup"><span data-stu-id="7e237-362">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="7e237-363">Jest to pierwsza z szeregu samouczków, które wyjaśniają, jak skompilować przykładową aplikację firmy Contoso University od podstaw.</span><span class="sxs-lookup"><span data-stu-id="7e237-363">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7e237-364">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="7e237-364">Prerequisites</span></span>

* [<span data-ttu-id="7e237-365">Zestaw .NET Core SDK 2,2</span><span class="sxs-lookup"><span data-stu-id="7e237-365">.NET Core SDK 2.2</span></span>](https://dotnet.microsoft.com/download)
* <span data-ttu-id="7e237-366">[Program Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) z następującymi obciążeniami:</span><span class="sxs-lookup"><span data-stu-id="7e237-366">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the following workloads:</span></span>
  * <span data-ttu-id="7e237-367">**ASP.NET i programowanie aplikacji sieci Web**</span><span class="sxs-lookup"><span data-stu-id="7e237-367">**ASP.NET and web development** workload</span></span>
  * <span data-ttu-id="7e237-368">**Tworzenie aplikacji dla wielu platform w środowisku .NET Core**</span><span class="sxs-lookup"><span data-stu-id="7e237-368">**.NET Core cross-platform development** workload</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7e237-369">Rozwiązywanie problemów</span><span class="sxs-lookup"><span data-stu-id="7e237-369">Troubleshooting</span></span>

<span data-ttu-id="7e237-370">Jeśli wystąpi problem, którego nie można rozwiązać, można ogólnie znaleźć rozwiązanie, porównując kod z [ukończonym projektem](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/data/ef-mvc/intro/samples).</span><span class="sxs-lookup"><span data-stu-id="7e237-370">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/data/ef-mvc/intro/samples).</span></span> <span data-ttu-id="7e237-371">Aby zapoznać się z listą typowych błędów i sposobu ich rozwiązywania, zobacz [sekcję Rozwiązywanie problemów w ostatnim samouczku w serii](advanced.md#common-errors).</span><span class="sxs-lookup"><span data-stu-id="7e237-371">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="7e237-372">Jeśli nie możesz znaleźć tego, czego potrzebujesz, możesz ogłosić pytanie do StackOverflow.com dla [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) lub [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span><span class="sxs-lookup"><span data-stu-id="7e237-372">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="7e237-373">Jest to seria 10 samouczków, z których każdy jest oparty na tym, co zostało zrobione we wcześniejszych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="7e237-373">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="7e237-374">Rozważ zapisanie kopii projektu po każdym pomyślnym zakończeniu samouczka.</span><span class="sxs-lookup"><span data-stu-id="7e237-374">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="7e237-375">Jeśli wystąpią problemy, możesz zacząć od poprzedniego samouczka zamiast wrócić do początku całej serii.</span><span class="sxs-lookup"><span data-stu-id="7e237-375">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="contoso-university-web-app"></a><span data-ttu-id="7e237-376">Aplikacja sieci Web firmy Contoso University</span><span class="sxs-lookup"><span data-stu-id="7e237-376">Contoso University web app</span></span>

<span data-ttu-id="7e237-377">Aplikacja, którą tworzysz w tych samouczkach, jest prostą witryną sieci Web.</span><span class="sxs-lookup"><span data-stu-id="7e237-377">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="7e237-378">Użytkownicy mogą wyświetlać i aktualizować informacje dotyczące uczniów, kursów i instruktorów.</span><span class="sxs-lookup"><span data-stu-id="7e237-378">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="7e237-379">Poniżej przedstawiono kilka ekranów, które zostaną utworzone.</span><span class="sxs-lookup"><span data-stu-id="7e237-379">Here are a few of the screens you'll create.</span></span>

![Strona indeksu uczniów](intro/_static/students-index.png)

![Strona edycji uczniów](intro/_static/student-edit.png)

## <a name="create-web-app"></a><span data-ttu-id="7e237-382">Tworzenie aplikacji internetowej</span><span class="sxs-lookup"><span data-stu-id="7e237-382">Create web app</span></span>

* <span data-ttu-id="7e237-383">Otwórz program Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7e237-383">Open Visual Studio.</span></span>

* <span data-ttu-id="7e237-384">Z menu **plik** wybierz pozycję **Nowy projekt >**.</span><span class="sxs-lookup"><span data-stu-id="7e237-384">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="7e237-385">W lewym okienku wybierz pozycję **zainstalowane > Visual C# > sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="7e237-385">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="7e237-386">Wybierz szablon projektu **aplikacji sieci Web ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="7e237-386">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="7e237-387">Wprowadź **ContosoUniversity** jako nazwę, a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7e237-387">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![Okno dialogowe Nowy projekt](intro/_static/new-project2.png)

* <span data-ttu-id="7e237-389">Zaczekaj, aż pojawi się okno dialogowe **Nowa aplikacja sieci Web ASP.NET Core** .</span><span class="sxs-lookup"><span data-stu-id="7e237-389">Wait for the **New ASP.NET Core Web Application** dialog to appear.</span></span>

* <span data-ttu-id="7e237-390">Wybierz pozycję **.NET Core**, **ASP.NET Core 2,2** i szablon **aplikacji sieci Web (Model-View-Controller)** .</span><span class="sxs-lookup"><span data-stu-id="7e237-390">Select **.NET Core**, **ASP.NET Core 2.2** and the **Web Application (Model-View-Controller)** template.</span></span>

* <span data-ttu-id="7e237-391">Upewnij się, że **uwierzytelnianie** jest ustawione na wartość **bez uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="7e237-391">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="7e237-392">Wybierz przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="7e237-392">Select **OK**</span></span>

  ![Nowe okno dialogowe projektu ASP.NET Core](intro/_static/new-aspnet2.png)

## <a name="set-up-the-site-style"></a><span data-ttu-id="7e237-394">Konfigurowanie stylu witryny</span><span class="sxs-lookup"><span data-stu-id="7e237-394">Set up the site style</span></span>

<span data-ttu-id="7e237-395">Kilka prostych zmian spowoduje skonfigurowanie menu witryny, układu i strony głównej.</span><span class="sxs-lookup"><span data-stu-id="7e237-395">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="7e237-396">Otwórz *Widok widoki/Shared/_Layout. cshtml* i wprowadź następujące zmiany:</span><span class="sxs-lookup"><span data-stu-id="7e237-396">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="7e237-397">Zmień każde wystąpienie "ContosoUniversity" na "Uniwersytet firmy Contoso".</span><span class="sxs-lookup"><span data-stu-id="7e237-397">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="7e237-398">Istnieją trzy wystąpienia.</span><span class="sxs-lookup"><span data-stu-id="7e237-398">There are three occurrences.</span></span>

* <span data-ttu-id="7e237-399">Dodaj pozycje menu dla **informacji o** programie, **studentów**, **kursy**, **Instruktorzy** i **działy**, a następnie usuń wpis menu **prywatność** .</span><span class="sxs-lookup"><span data-stu-id="7e237-399">Add menu entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Privacy** menu entry.</span></span>

<span data-ttu-id="7e237-400">Zmiany są wyróżnione.</span><span class="sxs-lookup"><span data-stu-id="7e237-400">The changes are highlighted.</span></span>

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,34-48,63)]

<span data-ttu-id="7e237-401">W obszarze *widoki/Home/index. cshtml* Zastąp zawartość pliku następującym kodem, aby zamienić tekst na ASP.NET i MVC z tekstem dotyczącym tej aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7e237-401">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="7e237-402">Naciśnij klawisze CTRL + F5, aby uruchomić projekt, lub wybierz polecenie **debuguj > Uruchom bez debugowania** z menu.</span><span class="sxs-lookup"><span data-stu-id="7e237-402">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="7e237-403">Zostanie wyświetlona strona główna z kartami dla stron, które zostaną utworzone w tych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="7e237-403">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Strona główna firmy Contoso University](intro/_static/home-page.png)

## <a name="about-ef-core-nuget-packages"></a><span data-ttu-id="7e237-405">Informacje o pakietach NuGet EF Core</span><span class="sxs-lookup"><span data-stu-id="7e237-405">About EF Core NuGet packages</span></span>

<span data-ttu-id="7e237-406">Aby dodać obsługę EF Core do projektu, zainstaluj dostawcę bazy danych, który ma być celem.</span><span class="sxs-lookup"><span data-stu-id="7e237-406">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="7e237-407">Ten samouczek używa SQL Server, a pakiet dostawcy to [Microsoft. EntityFrameworkCore. SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="7e237-407">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="7e237-408">Ten pakiet jest zawarty w [pakiecie Microsoft. AspNetCore. app](xref:fundamentals/metapackage-app), dlatego nie musisz się odwoływać do pakietu.</span><span class="sxs-lookup"><span data-stu-id="7e237-408">This package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), so you don't need to reference the package.</span></span>

<span data-ttu-id="7e237-409">Pakiet EF SQL Server i jego zależności ( `Microsoft.EntityFrameworkCore` oraz `Microsoft.EntityFrameworkCore.Relational` ) zapewniają obsługę środowiska uruchomieniowego dla EF.</span><span class="sxs-lookup"><span data-stu-id="7e237-409">The EF SQL Server package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="7e237-410">Pakiet narzędzi zostanie dodany później, w samouczku [migracji](migrations.md) .</span><span class="sxs-lookup"><span data-stu-id="7e237-410">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span>

<span data-ttu-id="7e237-411">Aby uzyskać informacje o innych dostawcach baz danych, które są dostępne dla Entity Framework Core, zobacz [dostawcy bazy danych](/ef/core/providers/).</span><span class="sxs-lookup"><span data-stu-id="7e237-411">For information about other database providers that are available for Entity Framework Core, see [Database providers](/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="7e237-412">Tworzenie modelu danych</span><span class="sxs-lookup"><span data-stu-id="7e237-412">Create the data model</span></span>

<span data-ttu-id="7e237-413">Następnie utworzysz klasy jednostek dla aplikacji firmy Contoso University.</span><span class="sxs-lookup"><span data-stu-id="7e237-413">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="7e237-414">Zaczniesz od następujących trzech jednostek.</span><span class="sxs-lookup"><span data-stu-id="7e237-414">You'll start with the following three entities.</span></span>

![Kurs — Diagram modelu danych ucznia](intro/_static/data-model-diagram.png)

<span data-ttu-id="7e237-416">Istnieje relacja jeden do wielu między `Student` jednostkami i i `Enrollment` istnieje relacja jeden do wielu między elementami `Course` i `Enrollment` .</span><span class="sxs-lookup"><span data-stu-id="7e237-416">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="7e237-417">Innymi słowy, student może być zarejestrowany w dowolnej liczbie kursów, a kurs może mieć dowolną liczbę uczniów zarejestrowanych w nim.</span><span class="sxs-lookup"><span data-stu-id="7e237-417">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="7e237-418">W poniższych sekcjach utworzysz klasę dla każdej z tych jednostek.</span><span class="sxs-lookup"><span data-stu-id="7e237-418">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="7e237-419">Jednostka ucznia</span><span class="sxs-lookup"><span data-stu-id="7e237-419">The Student entity</span></span>

![Diagram jednostek uczniów](intro/_static/student-entity.png)

<span data-ttu-id="7e237-421">W folderze *modele* Utwórz plik klasy o nazwie *student.cs* i Zastąp kod szablonu poniższym kodem.</span><span class="sxs-lookup"><span data-stu-id="7e237-421">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="7e237-422">`ID`Właściwość stanie się kolumną klucza podstawowego tabeli bazy danych, która odnosi się do tej klasy.</span><span class="sxs-lookup"><span data-stu-id="7e237-422">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="7e237-423">Domyślnie platforma Entity Framework interpretuje właściwość o nazwie `ID` lub `classnameID` jako klucz podstawowy.</span><span class="sxs-lookup"><span data-stu-id="7e237-423">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="7e237-424">`Enrollments`Właściwość jest [właściwością nawigacji](/ef/core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="7e237-424">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="7e237-425">Właściwości nawigacji zawierają inne jednostki, które są powiązane z tą jednostką.</span><span class="sxs-lookup"><span data-stu-id="7e237-425">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="7e237-426">W tym przypadku `Enrollments` Właściwość a `Student entity` będzie zawierać wszystkie `Enrollment` jednostki, które są powiązane z tą `Student` jednostką.</span><span class="sxs-lookup"><span data-stu-id="7e237-426">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="7e237-427">Innymi słowy, jeśli `Student` wiersz w bazie danych ma dwa powiązane `Enrollment` wiersze (wiersze zawierające wartość klucza podstawowego tego ucznia w kolumnie klucza obcego StudentID), `Student` `Enrollments` Właściwość nawigacji tej jednostki będzie zawierać te dwie `Enrollment` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7e237-427">In other words, if a `Student` row in the database has two related `Enrollment` rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="7e237-428">Jeśli właściwość nawigacji może zawierać wiele jednostek (tak jak w przypadku relacji "wiele do wielu" lub "jeden do wielu"), jej typem musi być lista, w której można dodawać, usuwać i aktualizować wpisy, na przykład `ICollection<T>` .</span><span class="sxs-lookup"><span data-stu-id="7e237-428">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span> <span data-ttu-id="7e237-429">Możesz określić `ICollection<T>` lub typ, taki jak `List<T>` lub `HashSet<T>` .</span><span class="sxs-lookup"><span data-stu-id="7e237-429">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="7e237-430">Jeśli określisz `ICollection<T>` , EF domyślnie tworzy `HashSet<T>` kolekcję.</span><span class="sxs-lookup"><span data-stu-id="7e237-430">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="7e237-431">Jednostka rejestracji</span><span class="sxs-lookup"><span data-stu-id="7e237-431">The Enrollment entity</span></span>

![Diagram jednostek rejestracji](intro/_static/enrollment-entity.png)

<span data-ttu-id="7e237-433">W folderze *modele* Utwórz *Enrollment.cs* i Zastąp istniejący kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7e237-433">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="7e237-434">`EnrollmentID`Właściwość będzie kluczem podstawowym; ta jednostka używa `classnameID` wzorca zamiast `ID` samego siebie, jak pokazano w `Student` jednostce.</span><span class="sxs-lookup"><span data-stu-id="7e237-434">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="7e237-435">Zwykle należy wybrać jeden wzorzec i używać go w całym modelu danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-435">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="7e237-436">Tutaj, odmiana ilustruje, że można użyć dowolnego wzorca.</span><span class="sxs-lookup"><span data-stu-id="7e237-436">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="7e237-437">W [późniejszym samouczku](inheritance.md)zobaczysz, jak używać identyfikatora bez ClassName, ułatwia implementowanie dziedziczenia w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-437">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="7e237-438">`Grade`Właściwość jest `enum` .</span><span class="sxs-lookup"><span data-stu-id="7e237-438">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="7e237-439">Znak zapytania po `Grade` deklaracji typu wskazuje, że `Grade` Właściwość dopuszcza wartość null.</span><span class="sxs-lookup"><span data-stu-id="7e237-439">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="7e237-440">Klasa o wartości null różni się od klasy zerowej — wartość null oznacza, że Klasa nie jest znana lub nie została jeszcze przypisana.</span><span class="sxs-lookup"><span data-stu-id="7e237-440">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="7e237-441">`StudentID`Właściwość jest kluczem obcym, a odpowiednia właściwość nawigacji to `Student` .</span><span class="sxs-lookup"><span data-stu-id="7e237-441">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="7e237-442">`Enrollment`Jednostka jest skojarzona z jedną `Student` jednostką, więc właściwość może zawierać tylko jedną `Student` jednostkę (w przeciwieństwie do `Student.Enrollments` wytoczonej wcześniej właściwości nawigacji, która może zawierać wiele `Enrollment` jednostek).</span><span class="sxs-lookup"><span data-stu-id="7e237-442">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="7e237-443">`CourseID`Właściwość jest kluczem obcym, a odpowiednia właściwość nawigacji to `Course` .</span><span class="sxs-lookup"><span data-stu-id="7e237-443">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="7e237-444">`Enrollment`Jednostka jest skojarzona z jedną `Course` jednostką.</span><span class="sxs-lookup"><span data-stu-id="7e237-444">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="7e237-445">Entity Framework interpretuje właściwość jako właściwość klucza obcego, jeśli jest nazwana `<navigation property name><primary key property name>` (na przykład `StudentID` dla `Student` właściwości nawigacji, ponieważ `Student` klucz podstawowy jednostki to `ID` ).</span><span class="sxs-lookup"><span data-stu-id="7e237-445">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="7e237-446">Właściwości klucza obcego mogą być również nazywane po prostu `<primary key property name>` (na przykład, `CourseID` ponieważ `Course` klucz podstawowy jednostki to `CourseID` ).</span><span class="sxs-lookup"><span data-stu-id="7e237-446">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="7e237-447">Jednostka kursu</span><span class="sxs-lookup"><span data-stu-id="7e237-447">The Course entity</span></span>

![Diagram jednostek kursu](intro/_static/course-entity.png)

<span data-ttu-id="7e237-449">W folderze *modele* Utwórz *Course.cs* i Zastąp istniejący kod następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7e237-449">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="7e237-450">`Enrollments`Właściwość jest właściwością nawigacji.</span><span class="sxs-lookup"><span data-stu-id="7e237-450">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="7e237-451">`Course`Jednostka może być powiązana z dowolną liczbą `Enrollment` jednostek.</span><span class="sxs-lookup"><span data-stu-id="7e237-451">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="7e237-452">Dowiesz się więcej na temat `DatabaseGenerated` atrybutu w [kolejnym samouczku](complex-data-model.md) w tej serii.</span><span class="sxs-lookup"><span data-stu-id="7e237-452">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="7e237-453">Zasadniczo ten atrybut umożliwia wprowadzenie klucza podstawowego dla kursu, a nie jego wygenerowanie.</span><span class="sxs-lookup"><span data-stu-id="7e237-453">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="7e237-454">Tworzenie kontekstu bazy danych</span><span class="sxs-lookup"><span data-stu-id="7e237-454">Create the database context</span></span>

<span data-ttu-id="7e237-455">Klasa główna, która koordynuje funkcje Entity Framework dla danego modelu danych, jest klasą kontekstu bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-455">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="7e237-456">Tę klasę można utworzyć, wyprowadzając ją z `Microsoft.EntityFrameworkCore.DbContext` klasy.</span><span class="sxs-lookup"><span data-stu-id="7e237-456">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="7e237-457">W kodzie możesz określić, które jednostki zostaną uwzględnione w modelu danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-457">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="7e237-458">Można również dostosować pewne zachowanie Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="7e237-458">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="7e237-459">W tym projekcie Klasa ma nazwę `SchoolContext` .</span><span class="sxs-lookup"><span data-stu-id="7e237-459">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="7e237-460">W folderze projektu Utwórz folder o nazwie *dane*.</span><span class="sxs-lookup"><span data-stu-id="7e237-460">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="7e237-461">W folderze *dane* Utwórz nowy plik klasy o nazwie *SchoolContext.cs* i Zastąp kod szablonu następującym kodem:</span><span class="sxs-lookup"><span data-stu-id="7e237-461">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="7e237-462">Ten kod tworzy `DbSet` Właściwość dla każdego zestawu jednostek.</span><span class="sxs-lookup"><span data-stu-id="7e237-462">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="7e237-463">W Entity Framework terminologii zestaw jednostek zwykle odpowiada tabeli bazy danych, a jednostka odpowiada wierszowi w tabeli.</span><span class="sxs-lookup"><span data-stu-id="7e237-463">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="7e237-464">Można pominąć `DbSet<Enrollment>` `DbSet<Course>` instrukcje i i będzie on działał tak samo.</span><span class="sxs-lookup"><span data-stu-id="7e237-464">You could've omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="7e237-465">Entity Framework będzie zawierać je niejawnie, ponieważ `Student` Jednostka odwołuje się do `Enrollment` jednostki, a `Enrollment` Jednostka odwołuje się do `Course` jednostki.</span><span class="sxs-lookup"><span data-stu-id="7e237-465">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="7e237-466">Po utworzeniu bazy danych EF tworzy tabele, które mają nazwy takie same jak `DbSet` nazwy właściwości.</span><span class="sxs-lookup"><span data-stu-id="7e237-466">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="7e237-467">Nazwy właściwości dla kolekcji są zwykle plural (studenci zamiast uczniów), ale deweloperzy zgadzają się na to, czy nazwy tabel powinny być wyrzucane.</span><span class="sxs-lookup"><span data-stu-id="7e237-467">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="7e237-468">Te samouczki zastąpią domyślne zachowanie, określając pojedyncze nazwy tabel w kontekście DbContext.</span><span class="sxs-lookup"><span data-stu-id="7e237-468">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="7e237-469">Aby to zrobić, Dodaj następujący wyróżniony kod po ostatniej właściwości Nieogólnymi.</span><span class="sxs-lookup"><span data-stu-id="7e237-469">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

<span data-ttu-id="7e237-470">Kompiluj projekt jako sprawdzenie błędów kompilatora.</span><span class="sxs-lookup"><span data-stu-id="7e237-470">Build the project as a check for compiler errors.</span></span>

## <a name="register-the-schoolcontext"></a><span data-ttu-id="7e237-471">Zarejestruj SchoolContext</span><span class="sxs-lookup"><span data-stu-id="7e237-471">Register the SchoolContext</span></span>

<span data-ttu-id="7e237-472">ASP.NET Core domyślnie implementuje [iniekcję zależności](../../fundamentals/dependency-injection.md) .</span><span class="sxs-lookup"><span data-stu-id="7e237-472">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="7e237-473">Usługi (takie jak kontekst bazy danych EF) są rejestrowane przy użyciu iniekcji zależności podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7e237-473">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="7e237-474">Składniki, które wymagają tych usług (takich jak kontrolery MVC), są dostarczane przez parametry konstruktora.</span><span class="sxs-lookup"><span data-stu-id="7e237-474">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="7e237-475">Zobaczysz kod konstruktora kontrolera, który pobiera wystąpienie kontekstu w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="7e237-475">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="7e237-476">Aby zarejestrować się `SchoolContext` jako usługa, otwórz *Startup.cs* i Dodaj wyróżnione wiersze do `ConfigureServices` metody.</span><span class="sxs-lookup"><span data-stu-id="7e237-476">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=9-10)]

<span data-ttu-id="7e237-477">Nazwa parametrów połączenia jest przenoszona do kontekstu przez wywołanie metody w `DbContextOptionsBuilder` obiekcie.</span><span class="sxs-lookup"><span data-stu-id="7e237-477">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="7e237-478">W przypadku lokalnego projektowania [system konfiguracji ASP.NET Core](xref:fundamentals/configuration/index) odczytuje parametry połączenia z *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="7e237-478">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="7e237-479">Dodaj `using` instrukcje dla `ContosoUniversity.Data` i `Microsoft.EntityFrameworkCore` przestrzeni nazw, a następnie Skompiluj projekt.</span><span class="sxs-lookup"><span data-stu-id="7e237-479">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="7e237-480">Otwórz *appsettings.json* plik i Dodaj parametry połączenia, jak pokazano w poniższym przykładzie.</span><span class="sxs-lookup"><span data-stu-id="7e237-480">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="7e237-481">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="7e237-481">SQL Server Express LocalDB</span></span>

<span data-ttu-id="7e237-482">Parametry połączenia określają SQL Server bazy danych LocalDB.</span><span class="sxs-lookup"><span data-stu-id="7e237-482">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="7e237-483">LocalDB to uproszczona wersja aparatu bazy danych SQL Server Express i jest przeznaczona do tworzenia aplikacji, a nie do użycia w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="7e237-483">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="7e237-484">LocalDB uruchamia się na żądanie i działa w trybie użytkownika, więc nie istnieje złożona konfiguracja.</span><span class="sxs-lookup"><span data-stu-id="7e237-484">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="7e237-485">Domyślnie LocalDB tworzy pliki bazy danych *MDF* w `C:/Users/<user>` katalogu.</span><span class="sxs-lookup"><span data-stu-id="7e237-485">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="initialize-db-with-test-data"></a><span data-ttu-id="7e237-486">Zainicjuj bazę danych z danymi testowymi</span><span class="sxs-lookup"><span data-stu-id="7e237-486">Initialize DB with test data</span></span>

<span data-ttu-id="7e237-487">Entity Framework utworzy pustą bazę danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-487">The Entity Framework will create an empty database for you.</span></span> <span data-ttu-id="7e237-488">W tej sekcji napiszesz metodę, która jest wywoływana po utworzeniu bazy danych w celu wypełnienia jej danymi testowymi.</span><span class="sxs-lookup"><span data-stu-id="7e237-488">In this section, you write a method that's called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="7e237-489">W tym miejscu będziesz używać `EnsureCreated` metody do automatycznego tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-489">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="7e237-490">W [późniejszym samouczku](migrations.md) zobaczysz, jak obsłużyć zmiany modelu przy użyciu migracje Code First, aby zmienić schemat bazy danych zamiast upuszczania i ponownego tworzenia bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-490">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="7e237-491">W folderze *dane* Utwórz nowy plik klasy o nazwie *DbInitializer.cs* i Zastąp kod szablonu następującym kodem, co spowoduje utworzenie bazy danych w razie potrzeby i załadowanie danych testowych do nowej bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-491">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="7e237-492">Kod sprawdza, czy w bazie danych znajdują się uczniowie i czy nie, zakłada, że baza danych jest nowa i należy ją umieścić w danych testowych.</span><span class="sxs-lookup"><span data-stu-id="7e237-492">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span> <span data-ttu-id="7e237-493">Ładuje dane testowe do tablic, a nie `List<T>` kolekcji w celu zoptymalizowania wydajności.</span><span class="sxs-lookup"><span data-stu-id="7e237-493">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="7e237-494">W *program.cs* Zmień metodę, `Main` Aby wykonać następujące czynności podczas uruchamiania aplikacji:</span><span class="sxs-lookup"><span data-stu-id="7e237-494">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="7e237-495">Pobierz wystąpienie kontekstu bazy danych z kontenera iniekcji zależności.</span><span class="sxs-lookup"><span data-stu-id="7e237-495">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="7e237-496">Wywołaj metodę inicjatora, przekazując ją do kontekstu.</span><span class="sxs-lookup"><span data-stu-id="7e237-496">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="7e237-497">Usuń kontekst, gdy metoda inicjatora jest gotowa.</span><span class="sxs-lookup"><span data-stu-id="7e237-497">Dispose the context when the seed method is done.</span></span>

[!code-csharp[](intro/samples/5cu-snap/Program.cs?highlight=1-2,14-18,21-38)]

<span data-ttu-id="7e237-498">Przy pierwszym uruchomieniu aplikacji baza danych zostanie utworzona i roztworzona przy użyciu danych testowych.</span><span class="sxs-lookup"><span data-stu-id="7e237-498">The first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="7e237-499">Za każdym razem, gdy zmienisz model danych:</span><span class="sxs-lookup"><span data-stu-id="7e237-499">Whenever you change the data model:</span></span>

 * <span data-ttu-id="7e237-500">Usuń bazę danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-500">Delete the database.</span></span>
 * <span data-ttu-id="7e237-501">Zaktualizuj metodę inicjatora i zacznij od nowa bazę danych w ten sam sposób.</span><span class="sxs-lookup"><span data-stu-id="7e237-501">Update the seed method, and start afresh with a new database the same way.</span></span>
 
<span data-ttu-id="7e237-502">W kolejnych samouczkach zobaczysz, jak zmodyfikować bazę danych, gdy zmieni się model danych, bez usuwania i ponownego tworzenia.</span><span class="sxs-lookup"><span data-stu-id="7e237-502">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-controller-and-views"></a><span data-ttu-id="7e237-503">Tworzenie kontrolera i widoków</span><span class="sxs-lookup"><span data-stu-id="7e237-503">Create controller and views</span></span>

<span data-ttu-id="7e237-504">W tej sekcji aparat tworzenia szkieletów w programie Visual Studio służy do dodawania kontrolera MVC i widoków, które będą używać EF do wykonywania zapytań i zapisywania danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-504">In this section, the scaffolding engine in Visual Studio is used to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="7e237-505">Automatyczne tworzenie metod i widoków akcji CRUD jest znane jako rusztowania.</span><span class="sxs-lookup"><span data-stu-id="7e237-505">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="7e237-506">Tworzenie szkieletu różni się od generowania kodu, ponieważ kod szkieletowy jest punktem wyjścia, który można zmodyfikować zgodnie z własnymi wymaganiami, ale zazwyczaj nie jest modyfikowany kod wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="7e237-506">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="7e237-507">Jeśli musisz dostosować wygenerowany kod, użyj klas częściowych lub ponownie Wygeneruj kod, gdy zmienią się.</span><span class="sxs-lookup"><span data-stu-id="7e237-507">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="7e237-508">Kliknij prawym przyciskiem myszy folder **controllers** w **Eksplorator rozwiązań** a następnie wybierz pozycję **Dodaj > nowy element szkieletowy**.</span><span class="sxs-lookup"><span data-stu-id="7e237-508">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>
* <span data-ttu-id="7e237-509">W oknie dialogowym **Dodawanie szkieletu** :</span><span class="sxs-lookup"><span data-stu-id="7e237-509">In the **Add Scaffold** dialog box:</span></span>
  * <span data-ttu-id="7e237-510">Wybierz **kontroler MVC z widokami, używając Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="7e237-510">Select **MVC controller with views, using Entity Framework**.</span></span>
  * <span data-ttu-id="7e237-511">Kliknij pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="7e237-511">Click **Add**.</span></span> <span data-ttu-id="7e237-512">Pojawia się okno dialogowe **Dodawanie kontrolera MVC z widokami z użyciem Entity Framework** : ![ studenta dla szkieletu](intro/_static/scaffold-student2.png)</span><span class="sxs-lookup"><span data-stu-id="7e237-512">The **Add MVC Controller with views, using Entity Framework** dialog box appears: ![Scaffold Student](intro/_static/scaffold-student2.png)</span></span>
  * <span data-ttu-id="7e237-513">W obszarze **Klasa modelu** wybierz **ucznia**.</span><span class="sxs-lookup"><span data-stu-id="7e237-513">In **Model class** select **Student**.</span></span>
  * <span data-ttu-id="7e237-514">W obszarze **Klasa kontekstu danych** wybierz pozycję **SchoolContext**.</span><span class="sxs-lookup"><span data-stu-id="7e237-514">In **Data context class** select **SchoolContext**.</span></span>
  * <span data-ttu-id="7e237-515">Zaakceptuj domyślną **StudentsController** jako nazwę.</span><span class="sxs-lookup"><span data-stu-id="7e237-515">Accept the default **StudentsController** as the name.</span></span>
  * <span data-ttu-id="7e237-516">Kliknij pozycję **Dodaj**.</span><span class="sxs-lookup"><span data-stu-id="7e237-516">Click **Add**.</span></span>

<span data-ttu-id="7e237-517">Aparat szkieletu programu Visual Studio tworzy plik *StudentsController.cs* i zestaw widoków (pliki *. cshtml* ), które współpracują z kontrolerem.</span><span class="sxs-lookup"><span data-stu-id="7e237-517">The Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="7e237-518">Zwróć uwagę, że kontroler przyjmuje `SchoolContext` jako parametr konstruktora.</span><span class="sxs-lookup"><span data-stu-id="7e237-518">Notice the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="7e237-519">ASP.NET Core iniekcja zależności ma zadbać o przekazanie wystąpienia `SchoolContext` do kontrolera.</span><span class="sxs-lookup"><span data-stu-id="7e237-519">ASP.NET Core dependency injection takes care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="7e237-520">Który został skonfigurowany w pliku *Startup.cs* .</span><span class="sxs-lookup"><span data-stu-id="7e237-520">That was configured  in the *Startup.cs* file.</span></span>

<span data-ttu-id="7e237-521">Kontroler zawiera `Index` metodę akcji, która wyświetla wszystkich uczniów w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-521">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="7e237-522">Metoda pobiera listę studentów z zestawu jednostek studentów, odczytując `Students` Właściwość wystąpienia kontekstu bazy danych:</span><span class="sxs-lookup"><span data-stu-id="7e237-522">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="7e237-523">Dowiesz się więcej na temat asynchronicznych elementów programowania w tym kodzie w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="7e237-523">You learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="7e237-524">Widok *widoki/uczniowie/index. cshtml* wyświetla tę listę w tabeli:</span><span class="sxs-lookup"><span data-stu-id="7e237-524">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="7e237-525">Naciśnij klawisze CTRL + F5, aby uruchomić projekt, lub wybierz polecenie **debuguj > Uruchom bez debugowania** z menu.</span><span class="sxs-lookup"><span data-stu-id="7e237-525">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="7e237-526">Kliknij kartę studenci, aby zobaczyć dane testowe, które zostały `DbInitializer.Initialize` wstawione przez metodę.</span><span class="sxs-lookup"><span data-stu-id="7e237-526">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="7e237-527">W zależności od tego, jak Zawężasz okno przeglądarki, zobaczysz `Students` link do karty w górnej części strony lub kliknij ikonę nawigacji w prawym górnym rogu, aby zobaczyć link.</span><span class="sxs-lookup"><span data-stu-id="7e237-527">Depending on how narrow your browser window is, you'll see the `Students` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Strona główna firmy Contoso University Narrow](intro/_static/home-page-narrow.png)

![Strona indeksu uczniów](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="7e237-530">Wyświetlanie bazy danych</span><span class="sxs-lookup"><span data-stu-id="7e237-530">View the database</span></span>

<span data-ttu-id="7e237-531">Po uruchomieniu aplikacji `DbInitializer.Initialize` Metoda wywołuje metodę `EnsureCreated` .</span><span class="sxs-lookup"><span data-stu-id="7e237-531">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="7e237-532">EF wykryto, że nie istniała baza danych i dlatego została utworzona, a następnie pozostała część `Initialize` kodu metody wypełnił bazę danych danymi.</span><span class="sxs-lookup"><span data-stu-id="7e237-532">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="7e237-533">Aby wyświetlić bazę danych w programie Visual Studio, można użyć **Eksplorator obiektów SQL Server** (SSOX).</span><span class="sxs-lookup"><span data-stu-id="7e237-533">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="7e237-534">Zamknij okno przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="7e237-534">Close the browser.</span></span>

<span data-ttu-id="7e237-535">Jeśli okno SSOX nie jest jeszcze otwarte, wybierz je z menu **Widok** w programie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7e237-535">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="7e237-536">W SSOX kliknij pozycję **(LocalDB) \MSSQLLocalDB > bazy danych**, a następnie kliknij pozycję Nazwa bazy danych, która znajduje się w parametrach połączenia w *appsettings.json* pliku.</span><span class="sxs-lookup"><span data-stu-id="7e237-536">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that's in the connection string in the *appsettings.json* file.</span></span>

<span data-ttu-id="7e237-537">Rozwiń węzeł **tabele** , aby wyświetlić tabele w bazie danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-537">Expand the **Tables** node to see the tables in the database.</span></span>

![Tabele w SSOX](intro/_static/ssox-tables.png)

<span data-ttu-id="7e237-539">Kliknij prawym przyciskiem myszy tabelę **uczniów** i kliknij polecenie **Wyświetl dane** , aby wyświetlić utworzone kolumny i wiersze, które zostały wstawione do tabeli.</span><span class="sxs-lookup"><span data-stu-id="7e237-539">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![Tabela uczniów w SSOX](intro/_static/ssox-student-table.png)

<span data-ttu-id="7e237-541">Pliki *. mdf* i *. ldf* znajdują się w folderze *C:\Users \\ \<username>* .</span><span class="sxs-lookup"><span data-stu-id="7e237-541">The *.mdf* and *.ldf* database files are in the *C:\Users\\\<username>* folder.</span></span>

<span data-ttu-id="7e237-542">Ponieważ wywołujesz `EnsureCreated` metodę inicjatora, która jest uruchamiana podczas uruchamiania aplikacji, możesz teraz wprowadzić zmianę `Student` klasy, usunąć bazę danych, ponownie uruchomić aplikację, a baza danych zostanie automatycznie utworzona ponownie w celu dopasowania do zmiany.</span><span class="sxs-lookup"><span data-stu-id="7e237-542">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="7e237-543">Na przykład, jeśli dodasz `EmailAddress` Właściwość do `Student` klasy, zobaczysz nową `EmailAddress` kolumnę w nowo utworzonej tabeli.</span><span class="sxs-lookup"><span data-stu-id="7e237-543">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="7e237-544">Konwencje</span><span class="sxs-lookup"><span data-stu-id="7e237-544">Conventions</span></span>

<span data-ttu-id="7e237-545">Ilość kodu, który miał zostać zapisany w celu Entity Framework być w stanie utworzyć kompletną bazę danych, jest minimalny ze względu na stosowanie Konwencji lub zaEntity Framework łożeń.</span><span class="sxs-lookup"><span data-stu-id="7e237-545">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="7e237-546">Nazwy `DbSet` właściwości są używane jako nazwy tabel.</span><span class="sxs-lookup"><span data-stu-id="7e237-546">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="7e237-547">W przypadku jednostek, do których nie odwołuje się `DbSet` Właściwość, nazwy klas jednostek są używane jako nazwy tabel.</span><span class="sxs-lookup"><span data-stu-id="7e237-547">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>
* <span data-ttu-id="7e237-548">Nazwy właściwości jednostki są używane w nazwach kolumn.</span><span class="sxs-lookup"><span data-stu-id="7e237-548">Entity property names are used for column names.</span></span>
* <span data-ttu-id="7e237-549">Właściwości jednostki o nazwach ID lub classnameID są rozpoznawane jako właściwości klucza podstawowego.</span><span class="sxs-lookup"><span data-stu-id="7e237-549">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>
* <span data-ttu-id="7e237-550">Właściwość jest interpretowana jako właściwość klucza obcego, jeśli jest nazwana *\<navigation property name>\<primary key property name>* (na przykład `StudentID` dla `Student` właściwości nawigacji, ponieważ `Student` klucz podstawowy jednostki to `ID` ).</span><span class="sxs-lookup"><span data-stu-id="7e237-550">A property is interpreted as a foreign key property if it's named *\<navigation property name>\<primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="7e237-551">Właściwości klucza obcego mogą być również nazywane po prostu *\<primary key property name>* (na przykład, `EnrollmentID` ponieważ `Enrollment` klucz podstawowy jednostki to `EnrollmentID` ).</span><span class="sxs-lookup"><span data-stu-id="7e237-551">Foreign key properties can also be named simply *\<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="7e237-552">Zachowanie konwencjonalne można zastąpić.</span><span class="sxs-lookup"><span data-stu-id="7e237-552">Conventional behavior can be overridden.</span></span> <span data-ttu-id="7e237-553">Na przykład można jawnie określić nazwy tabel, jak zostało to opisane wcześniej w tym samouczku.</span><span class="sxs-lookup"><span data-stu-id="7e237-553">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="7e237-554">I można ustawić nazwy kolumn i ustawić dowolną właściwość jako klucz podstawowy lub klucz obcy, jak widać w [późniejszym samouczku](complex-data-model.md) w tej serii.</span><span class="sxs-lookup"><span data-stu-id="7e237-554">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="7e237-555">Kod asynchroniczny</span><span class="sxs-lookup"><span data-stu-id="7e237-555">Asynchronous code</span></span>

<span data-ttu-id="7e237-556">Programowanie asynchroniczne jest trybem domyślnym dla ASP.NET Core i EF Core.</span><span class="sxs-lookup"><span data-stu-id="7e237-556">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="7e237-557">Serwer sieci Web ma ograniczoną liczbę dostępnych wątków, a w przypadku dużego obciążenia mogą być używane wszystkie dostępne wątki.</span><span class="sxs-lookup"><span data-stu-id="7e237-557">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="7e237-558">W takim przypadku serwer nie może przetwarzać nowych żądań, dopóki wątki nie zostaną zwolnione.</span><span class="sxs-lookup"><span data-stu-id="7e237-558">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="7e237-559">W przypadku kodu synchronicznego wiele wątków może zostać powiązanych, podczas gdy nie wykonuje żadnej pracy, ponieważ oczekuje na ukończenie operacji we/wy.</span><span class="sxs-lookup"><span data-stu-id="7e237-559">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="7e237-560">W przypadku kodu asynchronicznego, gdy proces oczekuje na ukończenie operacji we/wy, jego wątek zostanie zwolniony dla serwera do użycia na potrzeby przetwarzania innych żądań.</span><span class="sxs-lookup"><span data-stu-id="7e237-560">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="7e237-561">W efekcie kod asynchroniczny umożliwia efektywniejsze wykorzystanie zasobów serwera, a serwer jest włączony do obsługi większej ilości ruchu bez opóźnień.</span><span class="sxs-lookup"><span data-stu-id="7e237-561">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="7e237-562">Kod asynchroniczny wprowadza niewielką ilość narzutu w czasie wykonywania, ale w przypadku niskiego natężenia ruchu, gdy wydajność jest niewielka, w przypadku dużych sytuacji związanych z ruchem jest istotna poprawa wydajności.</span><span class="sxs-lookup"><span data-stu-id="7e237-562">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="7e237-563">W poniższym kodzie `async` słowo kluczowe, `Task<T>` wartość zwracana, `await` słowo kluczowe i `ToListAsync` Metoda sprawiają, że kod jest wykonywany asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7e237-563">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="7e237-564">`async`Słowo kluczowe instruuje kompilator, aby generował wywołania zwrotne dla części treści metody i automatycznie utworzyć `Task<IActionResult>` zwracany obiekt.</span><span class="sxs-lookup"><span data-stu-id="7e237-564">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that's returned.</span></span>
* <span data-ttu-id="7e237-565">Typ zwracany `Task<IActionResult>` reprezentuje bieżącą współpracę z wynikiem typu `IActionResult` .</span><span class="sxs-lookup"><span data-stu-id="7e237-565">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>
* <span data-ttu-id="7e237-566">`await`Słowo kluczowe powoduje, że kompilator dzieli metodę na dwie części.</span><span class="sxs-lookup"><span data-stu-id="7e237-566">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="7e237-567">Pierwsza część jest zakończona operacją uruchomioną asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7e237-567">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="7e237-568">Druga część jest umieszczana w metodzie wywołania zwrotnego, która jest wywoływana po zakończeniu operacji.</span><span class="sxs-lookup"><span data-stu-id="7e237-568">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="7e237-569">`ToListAsync` jest asynchroniczną wersją `ToList` metody rozszerzenia.</span><span class="sxs-lookup"><span data-stu-id="7e237-569">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="7e237-570">Niektóre kwestie, o których należy wiedzieć, gdy piszesz kod asynchroniczny, który używa Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="7e237-570">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="7e237-571">Tylko instrukcje, które powodują, że zapytania lub polecenia wysyłane do bazy danych są wykonywane asynchronicznie.</span><span class="sxs-lookup"><span data-stu-id="7e237-571">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="7e237-572">Obejmuje to, na przykład, `ToListAsync` , `SingleOrDefaultAsync` i `SaveChangesAsync` .</span><span class="sxs-lookup"><span data-stu-id="7e237-572">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="7e237-573">Nie zawiera na przykład instrukcji, które po prostu zmieniają element `IQueryable` , taki jak `var students = context.Students.Where(s => s.LastName == "Davolio")` .</span><span class="sxs-lookup"><span data-stu-id="7e237-573">It doesn't include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="7e237-574">Kontekst EF nie jest bezpieczny wątkowo: nie próbuj wykonać równolegle wielu operacji.</span><span class="sxs-lookup"><span data-stu-id="7e237-574">An EF context isn't thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="7e237-575">Gdy wywoływana jest metoda async EF, zawsze używaj `await` słowa kluczowego.</span><span class="sxs-lookup"><span data-stu-id="7e237-575">When you call any async EF method, always use the `await` keyword.</span></span>
* <span data-ttu-id="7e237-576">Jeśli chcesz wykorzystać zalety wydajności w kodzie asynchronicznym, upewnij się, że wszystkie używane pakiety biblioteki (na przykład na potrzeby stronicowania) używają również metody asynchronicznej, jeśli wywołują wszelkie Entity Framework metod, które powodują wysłanie zapytań do bazy danych.</span><span class="sxs-lookup"><span data-stu-id="7e237-576">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="7e237-577">Aby uzyskać więcej informacji na temat programowania asynchronicznego w programie .NET, zobacz [asynchroniczne Omówienie](/dotnet/articles/standard/async).</span><span class="sxs-lookup"><span data-stu-id="7e237-577">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e237-578">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="7e237-578">Next steps</span></span>

<span data-ttu-id="7e237-579">Przejdź do następnego samouczka, aby dowiedzieć się, jak wykonywać podstawowe operacje CRUD (tworzenie, odczytywanie, aktualizowanie i usuwanie).</span><span class="sxs-lookup"><span data-stu-id="7e237-579">Advance to the next tutorial to learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="7e237-580">Zaimplementuj podstawowe funkcje CRUD</span><span class="sxs-lookup"><span data-stu-id="7e237-580">Implement basic CRUD functionality</span></span>](crud.md)

::: moniker-end
