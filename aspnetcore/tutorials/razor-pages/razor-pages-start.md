---
title: 'Samouczek: wprowadzenie do Razor stron w ASP.NET Core'
author: rick-anderson
description: Jest to pierwszy samouczek dotyczący serii, który uczy się podstaw tworzenia Razor aplikacji sieci web ASP.NET Core Pages.
ms.author: riande
ms.date: 09/15/2020
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
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 61ff4edfaedb22ea88d6bb0be2390ff734879942
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102588441"
---
# <a name="tutorial-get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="ab736-103">Samouczek: wprowadzenie do Razor stron w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab736-103">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="ab736-104">Autor: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ab736-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-5.0"
<span data-ttu-id="ab736-105">Jest to pierwszy samouczek dotyczący serii, który uczy się podstaw tworzenia Razor aplikacji sieci web ASP.NET Core Pages.</span><span class="sxs-lookup"><span data-stu-id="ab736-105">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

<span data-ttu-id="ab736-106">Aby uzyskać bardziej zaawansowane wprowadzenie do deweloperów, którzy znają kontrolery i widoki, zobacz [wprowadzenie do Razor stron](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="ab736-106">For a more advanced introduction aimed at developers who are familiar with controllers and views, see [Introduction to Razor Pages](xref:razor-pages/index).</span></span>

<span data-ttu-id="ab736-107">Na końcu serii będziesz mieć aplikację, która zarządza bazą danych filmów.</span><span class="sxs-lookup"><span data-stu-id="ab736-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

<span data-ttu-id="ab736-108">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie50) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="ab736-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie50) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="ab736-109">W tym samouczku zostały wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="ab736-109">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ab736-110">Tworzenie Razor aplikacji sieci Web dla stron.</span><span class="sxs-lookup"><span data-stu-id="ab736-110">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="ab736-111">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="ab736-111">Run the app.</span></span>
> * <span data-ttu-id="ab736-112">Przejrzyj pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="ab736-112">Examine the project files.</span></span>

<span data-ttu-id="ab736-113">Na końcu tego samouczka będziesz mieć Razor aplikację sieci Web strony roboczej, którą można ulepszyć w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="ab736-113">At the end of this tutorial, you'll have a working Razor Pages web app that you'll enhance in later tutorials.</span></span>

![Strona główna lub:: No-Loc (index)::: Page](razor-pages-start/_static/5/home5.png)

## <a name="prerequisites"></a><span data-ttu-id="ab736-115">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ab736-115">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ab736-116">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab736-116">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-5.0.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="ab736-117">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ab736-117">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-5.0.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="ab736-118">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="ab736-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-5.0.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="ab736-119">Tworzenie Razor aplikacji sieci Web dla stron</span><span class="sxs-lookup"><span data-stu-id="ab736-119">Create a Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ab736-120">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab736-120">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="ab736-121">Uruchom program Visual Studio i wybierz pozycję **Utwórz nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="ab736-121">Start Visual Studio and select **Create a new project**.</span></span> <span data-ttu-id="ab736-122">Aby uzyskać więcej informacji, zobacz [Tworzenie nowego projektu w programie Visual Studio](/visualstudio/ide/create-new-project).</span><span class="sxs-lookup"><span data-stu-id="ab736-122">For more information, see [Create a new project in Visual Studio](/visualstudio/ide/create-new-project).</span></span>

   ![Utwórz nowy projekt z okna uruchamiania](razor-pages-start/_static/5/start-window-create-new-project.png)

1. <span data-ttu-id="ab736-124">W oknie dialogowym **Tworzenie nowego projektu** wybierz pozycję **ASP.NET Core aplikacja sieci Web**, a następnie wybierz przycisk **dalej**.</span><span class="sxs-lookup"><span data-stu-id="ab736-124">In the **Create a new project** dialog, select **ASP.NET Core Web Application**, and then select **Next**.</span></span>

    ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/5/np.png)
    
1. <span data-ttu-id="ab736-126">W oknie dialogowym **Konfigurowanie nowego projektu** wprowadź wartość `RazorPagesMovie` w polu **Nazwa projektu**.</span><span class="sxs-lookup"><span data-stu-id="ab736-126">In the **Configure your new project** dialog, enter `RazorPagesMovie` for **Project name**.</span></span> <span data-ttu-id="ab736-127">Ważne jest, aby nazwa *Razor PagesMovie* projektu, w tym pasujące do wielkości liter, więc przestrzenie nazw są zgodne podczas kopiowania i wklejania przykładowego kodu.</span><span class="sxs-lookup"><span data-stu-id="ab736-127">It's important to name the project *RazorPagesMovie*, including matching the capitalization, so the namespaces will match when you copy and paste example code.</span></span>

1. <span data-ttu-id="ab736-128">Wybierz przycisk **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="ab736-128">Select **Create**.</span></span>

    ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/config.png)

1. <span data-ttu-id="ab736-130">W oknie dialogowym **Tworzenie nowej ASP.NET Core aplikacji sieci Web** wybierz:</span><span class="sxs-lookup"><span data-stu-id="ab736-130">In the **Create a new ASP.NET Core web application** dialog, select:</span></span>
    1. <span data-ttu-id="ab736-131">**Platformy .NET Core** i **ASP.NET Core 5,0** na liście rozwijanej.</span><span class="sxs-lookup"><span data-stu-id="ab736-131">**.NET Core** and **ASP.NET Core 5.0** in the dropdowns.</span></span>
    1. <span data-ttu-id="ab736-132">**Aplikacja sieci Web**.</span><span class="sxs-lookup"><span data-stu-id="ab736-132">**Web Application**.</span></span>
    1. <span data-ttu-id="ab736-133">**Create**.</span><span class="sxs-lookup"><span data-stu-id="ab736-133">**Create**.</span></span>

     ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/5/npx.png)

    <span data-ttu-id="ab736-135">Tworzony jest następujący projekt początkowy:</span><span class="sxs-lookup"><span data-stu-id="ab736-135">The following starter project is created:</span></span>

    ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="ab736-137">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ab736-137">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="ab736-138">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="ab736-138">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

1. <span data-ttu-id="ab736-139">Przejdź do katalogu ( `cd` ), który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="ab736-139">Change to the directory (`cd`) which will contain the project.</span></span>

1. <span data-ttu-id="ab736-140">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="ab736-140">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapp -o RazorPagesMovie
   code -r RazorPagesMovie
   ```

   * <span data-ttu-id="ab736-141">`dotnet new`Polecenie tworzy nowy Razor Projekt strony w folderze *Razor PagesMovie* .</span><span class="sxs-lookup"><span data-stu-id="ab736-141">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
   * <span data-ttu-id="ab736-142">`code`Polecenie otwiera folder *Razor PagesMovie* w bieżącym wystąpieniu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ab736-142">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="ab736-143">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="ab736-143">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="ab736-144">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="ab736-144">Select **File** > **New Solution**.</span></span>

    ![macOS nowe rozwiązanie](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

1. <span data-ttu-id="ab736-146">W Visual Studio dla komputerów Mac starszej niż wersja 8,6 Wybierz pozycję  >    >  **aplikacja internetowa aplikacji sieci Web** platformy .NET Core  >  .</span><span class="sxs-lookup"><span data-stu-id="ab736-146">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **Web Application** > **Next**.</span></span> <span data-ttu-id="ab736-147">W wersji 8,6 lub nowszej wybierz kolejno pozycje **Sieć Web i aplikacja konsoli** aplikacji  >    >  **sieci Web**  >  .</span><span class="sxs-lookup"><span data-stu-id="ab736-147">In version 8.6 or later, select **Web and Console** > **App** > **Web Application** > **Next**.</span></span>

    ![Wybieranie szablonu aplikacji sieci Web macOS](razor-pages-start/_static/web_app_template_vsmac.png)

1. <span data-ttu-id="ab736-149">W oknie dialogowym **Konfigurowanie nowej aplikacji sieci Web** :</span><span class="sxs-lookup"><span data-stu-id="ab736-149">In the **Configure the new Web Application** dialog:</span></span>

    1. <span data-ttu-id="ab736-150">Upewnij się, że **uwierzytelnianie** jest ustawione na wartość **bez uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="ab736-150">Confirm that **Authentication** is set to **No Authentication**.</span></span>
    1. <span data-ttu-id="ab736-151">Jeśli jest prezentowana opcja wyboru **platformy docelowej**, wybierz najnowszą wersję programu .NET 5. x.</span><span class="sxs-lookup"><span data-stu-id="ab736-151">If presented an option to select a **Target Framework**, select the latest .NET 5.x version.</span></span>
    1. <span data-ttu-id="ab736-152">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="ab736-152">Select **Next**.</span></span>

1. <span data-ttu-id="ab736-153">Nadaj projektowi nazwę *Razor PagesMovie* i wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="ab736-153">Name the project *RazorPagesMovie* and select **Create**.</span></span>

    ![macOS Nazwij projekt](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="ab736-155">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ab736-155">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="ab736-156">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="ab736-156">Examine the project files</span></span>

<span data-ttu-id="ab736-157">Poniżej przedstawiono Omówienie folderów i plików projektu głównego, z których będziesz korzystać w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="ab736-157">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="ab736-158">Folder stron</span><span class="sxs-lookup"><span data-stu-id="ab736-158">Pages folder</span></span>

<span data-ttu-id="ab736-159">Zawiera Razor strony i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="ab736-159">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="ab736-160">Każda Razor strona to para plików:</span><span class="sxs-lookup"><span data-stu-id="ab736-160">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="ab736-161">Plik *. cshtml* , który zawiera znaczniki HTML z kodem C# używając Razor składni.</span><span class="sxs-lookup"><span data-stu-id="ab736-161">A *.cshtml* file that has HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="ab736-162">Plik *. cshtml.cs* , który ma kod C#, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="ab736-162">A *.cshtml.cs* file that has C# code that handles page events.</span></span>

<span data-ttu-id="ab736-163">Pliki pomocnicze mają nazwy zaczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="ab736-163">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="ab736-164">Na przykład plik *_Layout. cshtml* służy do konfigurowania elementów interfejsu użytkownika wspólnych dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="ab736-164">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="ab736-165">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="ab736-165">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="ab736-166">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="ab736-166">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="ab736-167">folder wwwroot</span><span class="sxs-lookup"><span data-stu-id="ab736-167">wwwroot folder</span></span>

<span data-ttu-id="ab736-168">Zawiera statyczne zasoby, takie jak pliki HTML, pliki JavaScript i pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="ab736-168">Contains static assets, like HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="ab736-169">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="ab736-169">For more information, see <xref:fundamentals/static-files>.</span></span>

### appsettings.json

<span data-ttu-id="ab736-170">Zawiera dane konfiguracji, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="ab736-170">Contains configuration data, like connection strings.</span></span> <span data-ttu-id="ab736-171">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ab736-171">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="ab736-172">Program.cs</span><span class="sxs-lookup"><span data-stu-id="ab736-172">Program.cs</span></span>

<span data-ttu-id="ab736-173">Zawiera punkt wejścia dla aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ab736-173">Contains the entry point for the app.</span></span> <span data-ttu-id="ab736-174">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="ab736-174">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="ab736-175">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="ab736-175">Startup.cs</span></span>

<span data-ttu-id="ab736-176">Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ab736-176">Contains code that configures app behavior.</span></span> <span data-ttu-id="ab736-177">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="ab736-177">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab736-178">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="ab736-178">Next steps</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ab736-179">Dalej: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="ab736-179">Next: Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-5.0" -->

::: moniker range=">= aspnetcore-3.0 < aspnetcore-5.0"

<span data-ttu-id="ab736-180">Jest to pierwszy samouczek dotyczący serii, który uczy się podstaw tworzenia Razor aplikacji sieci web ASP.NET Core Pages.</span><span class="sxs-lookup"><span data-stu-id="ab736-180">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

<span data-ttu-id="ab736-181">Aby uzyskać bardziej zaawansowane wprowadzenie do deweloperów, którzy znają kontrolery i widoki, zobacz [wprowadzenie do Razor stron](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="ab736-181">For a more advanced introduction aimed at developers who are familiar with controllers and views, see [Introduction to Razor Pages](xref:razor-pages/index).</span></span>

<span data-ttu-id="ab736-182">Na końcu serii będziesz mieć aplikację, która zarządza bazą danych filmów.</span><span class="sxs-lookup"><span data-stu-id="ab736-182">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

<span data-ttu-id="ab736-183">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="ab736-183">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="ab736-184">W tym samouczku zostały wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="ab736-184">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ab736-185">Tworzenie Razor aplikacji sieci Web dla stron.</span><span class="sxs-lookup"><span data-stu-id="ab736-185">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="ab736-186">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="ab736-186">Run the app.</span></span>
> * <span data-ttu-id="ab736-187">Przejrzyj pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="ab736-187">Examine the project files.</span></span>

<span data-ttu-id="ab736-188">Na końcu tego samouczka będziesz mieć Razor aplikację sieci Web strony roboczej, która zostanie utworzona w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="ab736-188">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Strona główna lub:: No-Loc (index)::: Page](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="ab736-190">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ab736-190">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ab736-191">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab736-191">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="ab736-192">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ab736-192">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="ab736-193">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="ab736-193">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="ab736-194">Tworzenie Razor aplikacji sieci Web dla stron</span><span class="sxs-lookup"><span data-stu-id="ab736-194">Create a Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ab736-195">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab736-195">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ab736-196">Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="ab736-196">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="ab736-197">Utwórz nową aplikację sieci Web ASP.NET Core a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="ab736-197">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="ab736-198">![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="ab736-198">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="ab736-199">Nazwij projekt **Razor PagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="ab736-199">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="ab736-200">Ważne jest, aby nazwa projektu *Razor PagesMovie* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="ab736-200">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="ab736-201">![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="ab736-201">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="ab736-202">Wybierz pozycję **ASP.NET Core 3,1** na liście rozwijanej, **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="ab736-202">Select **ASP.NET Core 3.1** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="ab736-204">Tworzony jest następujący projekt początkowy:</span><span class="sxs-lookup"><span data-stu-id="ab736-204">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="ab736-206">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ab736-206">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="ab736-207">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="ab736-207">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="ab736-208">Przejdź do katalogu ( `cd` ), który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="ab736-208">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="ab736-209">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="ab736-209">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="ab736-210">`dotnet new`Polecenie tworzy nowy Razor Projekt strony w folderze *Razor PagesMovie* .</span><span class="sxs-lookup"><span data-stu-id="ab736-210">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="ab736-211">`code`Polecenie otwiera folder *Razor PagesMovie* w bieżącym wystąpieniu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ab736-211">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="ab736-212">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w oknie dialogowym zostanie wyświetlony monit **o podanie wymaganych zasobów do skompilowania i debugowania z elementu " Razor PagesMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="ab736-212">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="ab736-213">Wybierz pozycję **Tak**.</span><span class="sxs-lookup"><span data-stu-id="ab736-213">Select **Yes**.</span></span>

  <span data-ttu-id="ab736-214">Katalog *. programu vscode* , zawierający *launch.json* i *tasks.jsdla* plików, jest dodawany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="ab736-214">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="ab736-215">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="ab736-215">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ab736-216">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="ab736-216">Select **File** > **New Solution**.</span></span>

  ![macOS nowe rozwiązanie](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="ab736-218">W Visual Studio dla komputerów Mac starszej niż wersja 8,6 Wybierz pozycję  >    >  **aplikacja internetowa aplikacji sieci Web** platformy .NET Core  >  .</span><span class="sxs-lookup"><span data-stu-id="ab736-218">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **Web Application** > **Next**.</span></span> <span data-ttu-id="ab736-219">W wersji 8,6 lub nowszej wybierz kolejno pozycje **Sieć Web i aplikacja konsoli** aplikacji  >    >  **sieci Web**  >  .</span><span class="sxs-lookup"><span data-stu-id="ab736-219">In version 8.6 or later, select **Web and Console** > **App** > **Web Application** > **Next**.</span></span>

  ![Wybieranie szablonu aplikacji sieci Web macOS](razor-pages-start/_static/web_app_template_vsmac.png)

* <span data-ttu-id="ab736-221">W oknie dialogowym **Konfigurowanie nowej aplikacji sieci Web** :</span><span class="sxs-lookup"><span data-stu-id="ab736-221">In the **Configure the new Web Application** dialog:</span></span>

  * <span data-ttu-id="ab736-222">Upewnij się, że **uwierzytelnianie** jest ustawione na wartość **bez uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="ab736-222">Confirm that **Authentication** is set to **No Authentication**.</span></span>
  * <span data-ttu-id="ab736-223">Jeśli jest prezentowana opcja wyboru **platformy docelowej**, wybierz najnowszą wersję 3. x.</span><span class="sxs-lookup"><span data-stu-id="ab736-223">If presented an option to select a **Target Framework**, select the latest 3.x version.</span></span>

  <span data-ttu-id="ab736-224">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="ab736-224">Select **Next**.</span></span>

* <span data-ttu-id="ab736-225">Nazwij projekt **Razor PagesMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="ab736-225">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![macOS Nazwij projekt](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="ab736-227">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ab736-227">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="ab736-228">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="ab736-228">Examine the project files</span></span>

<span data-ttu-id="ab736-229">Poniżej przedstawiono Omówienie folderów i plików projektu głównego, z których będziesz korzystać w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="ab736-229">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="ab736-230">Folder stron</span><span class="sxs-lookup"><span data-stu-id="ab736-230">Pages folder</span></span>

<span data-ttu-id="ab736-231">Zawiera Razor strony i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="ab736-231">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="ab736-232">Każda Razor strona to para plików:</span><span class="sxs-lookup"><span data-stu-id="ab736-232">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="ab736-233">Plik *. cshtml* , który zawiera znaczniki HTML z kodem C# używając Razor składni.</span><span class="sxs-lookup"><span data-stu-id="ab736-233">A *.cshtml* file that has HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="ab736-234">Plik *. cshtml.cs* , który ma kod C#, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="ab736-234">A *.cshtml.cs* file that has C# code that handles page events.</span></span>

<span data-ttu-id="ab736-235">Pliki pomocnicze mają nazwy zaczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="ab736-235">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="ab736-236">Na przykład plik *_Layout. cshtml* służy do konfigurowania elementów interfejsu użytkownika wspólnych dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="ab736-236">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="ab736-237">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="ab736-237">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="ab736-238">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="ab736-238">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="ab736-239">folder wwwroot</span><span class="sxs-lookup"><span data-stu-id="ab736-239">wwwroot folder</span></span>

<span data-ttu-id="ab736-240">Zawiera pliki statyczne, takie jak pliki HTML, pliki JavaScript i pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="ab736-240">Contains static files, like HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="ab736-241">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="ab736-241">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="ab736-242">appSettings.jsna</span><span class="sxs-lookup"><span data-stu-id="ab736-242">appSettings.json</span></span>

<span data-ttu-id="ab736-243">Zawiera dane konfiguracji, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="ab736-243">Contains configuration data, like connection strings.</span></span> <span data-ttu-id="ab736-244">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ab736-244">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="ab736-245">Program.cs</span><span class="sxs-lookup"><span data-stu-id="ab736-245">Program.cs</span></span>

<span data-ttu-id="ab736-246">Zawiera punkt wejścia dla programu.</span><span class="sxs-lookup"><span data-stu-id="ab736-246">Contains the entry point for the program.</span></span> <span data-ttu-id="ab736-247">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="ab736-247">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="ab736-248">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="ab736-248">Startup.cs</span></span>

<span data-ttu-id="ab736-249">Zawiera kod, który konfiguruje zachowanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ab736-249">Contains code that configures app behavior.</span></span> <span data-ttu-id="ab736-250">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="ab736-250">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab736-251">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="ab736-251">Next steps</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ab736-252">Dalej: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="ab736-252">Next: Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ab736-253">Jest to pierwszy samouczek dotyczący serii.</span><span class="sxs-lookup"><span data-stu-id="ab736-253">This is the first tutorial of a series.</span></span> <span data-ttu-id="ab736-254">[Seria](xref:tutorials/razor-pages/index) uczy się podstaw tworzenia Razor aplikacji sieci Web ASP.NET Core Pages.</span><span class="sxs-lookup"><span data-stu-id="ab736-254">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

<span data-ttu-id="ab736-255">Aby uzyskać bardziej zaawansowane wprowadzenie do deweloperów, którzy znają kontrolery i widoki, zobacz [wprowadzenie do Razor stron](xref:razor-pages/index).</span><span class="sxs-lookup"><span data-stu-id="ab736-255">For a more advanced introduction aimed at developers who are familiar with controllers and views, see [Introduction to Razor Pages](xref:razor-pages/index).</span></span>

<span data-ttu-id="ab736-256">Na końcu serii będziesz mieć aplikację, która zarządza bazą danych filmów.</span><span class="sxs-lookup"><span data-stu-id="ab736-256">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

<span data-ttu-id="ab736-257">[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/tutorials/razor-pages/razor-pages-start) ([jak pobrać](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="ab736-257">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/tutorials/razor-pages/razor-pages-start) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="ab736-258">W tym samouczku zostały wykonane następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="ab736-258">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="ab736-259">Tworzenie Razor aplikacji sieci Web dla stron.</span><span class="sxs-lookup"><span data-stu-id="ab736-259">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="ab736-260">Uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="ab736-260">Run the app.</span></span>
> * <span data-ttu-id="ab736-261">Przejrzyj pliki projektu.</span><span class="sxs-lookup"><span data-stu-id="ab736-261">Examine the project files.</span></span>

<span data-ttu-id="ab736-262">Na końcu tego samouczka będziesz mieć Razor aplikację sieci Web strony roboczej, która zostanie utworzona w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="ab736-262">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Strona główna lub:: No-Loc (index)::: Page](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="ab736-264">Wymagania wstępne</span><span class="sxs-lookup"><span data-stu-id="ab736-264">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ab736-265">Program Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab736-265">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="ab736-266">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ab736-266">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="ab736-267">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="ab736-267">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="ab736-268">Tworzenie Razor aplikacji sieci Web dla stron</span><span class="sxs-lookup"><span data-stu-id="ab736-268">Create a Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ab736-269">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab736-269">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ab736-270">Z menu **plik** programu Visual Studio wybierz pozycję **Nowy** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="ab736-270">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="ab736-271">Utwórz nową aplikację sieci Web ASP.NET Core a następnie wybierz pozycję **dalej**.</span><span class="sxs-lookup"><span data-stu-id="ab736-271">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="ab736-273">Nazwij projekt **Razor PagesMovie**.</span><span class="sxs-lookup"><span data-stu-id="ab736-273">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="ab736-274">Ważne jest, aby nazwa projektu *Razor PagesMovie* , tak aby przestrzenie nazw były zgodne podczas kopiowania i wklejania kodu.</span><span class="sxs-lookup"><span data-stu-id="ab736-274">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/config.png)

* <span data-ttu-id="ab736-276">Wybierz pozycję **ASP.NET Core 2,2** na liście rozwijanej, **aplikacji sieci Web**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="ab736-276">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![Nowa aplikacja sieci Web ASP.NET Core](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="ab736-278">Tworzony jest następujący projekt początkowy:</span><span class="sxs-lookup"><span data-stu-id="ab736-278">The following starter project is created:</span></span>

  ![Eksplorator rozwiązań](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="ab736-280">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ab736-280">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="ab736-281">Otwórz [zintegrowany terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span><span class="sxs-lookup"><span data-stu-id="ab736-281">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="ab736-282">Przejdź do katalogu ( `cd` ), który będzie zawierać projekt.</span><span class="sxs-lookup"><span data-stu-id="ab736-282">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="ab736-283">Uruchom następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="ab736-283">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="ab736-284">`dotnet new`Polecenie tworzy nowy Razor Projekt strony w folderze *Razor PagesMovie* .</span><span class="sxs-lookup"><span data-stu-id="ab736-284">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="ab736-285">`code`Polecenie otwiera folder *Razor PagesMovie* w bieżącym wystąpieniu Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="ab736-285">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="ab736-286">Gdy ikona płomienia OmniSharp na pasku stanu zmieni kolor na zielony, w oknie dialogowym zostanie wyświetlony monit **o podanie wymaganych zasobów do skompilowania i debugowania z elementu " Razor PagesMovie". Dodać je?**</span><span class="sxs-lookup"><span data-stu-id="ab736-286">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="ab736-287">Wybierz pozycję **Tak**.</span><span class="sxs-lookup"><span data-stu-id="ab736-287">Select **Yes**.</span></span>

  <span data-ttu-id="ab736-288">Katalog *. programu vscode* , zawierający *launch.json* i *tasks.jsdla* plików, jest dodawany do katalogu głównego projektu.</span><span class="sxs-lookup"><span data-stu-id="ab736-288">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="ab736-289">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="ab736-289">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="ab736-290">Wybierz pozycję **plik** > **nowe rozwiązanie**.</span><span class="sxs-lookup"><span data-stu-id="ab736-290">Select **File** > **New Solution**.</span></span>

![macOS nowe rozwiązanie](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="ab736-292">W Visual Studio dla komputerów Mac starszej niż wersja 8,6 Wybierz pozycję  >    >  **aplikacja internetowa aplikacji sieci Web** platformy .NET Core  >  .</span><span class="sxs-lookup"><span data-stu-id="ab736-292">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **Web Application** > **Next**.</span></span> <span data-ttu-id="ab736-293">W wersji 8,6 lub nowszej wybierz kolejno pozycje **Sieć Web i aplikacja konsoli** aplikacji  >    >  **sieci Web**  >  .</span><span class="sxs-lookup"><span data-stu-id="ab736-293">In version 8.6 or later, select **Web and Console** > **App** > **Web Application** > **Next**.</span></span>

* <span data-ttu-id="ab736-294">W oknie dialogowym **Konfigurowanie nowej aplikacji sieci Web** :</span><span class="sxs-lookup"><span data-stu-id="ab736-294">In the **Configure the new Web Application** dialog:</span></span>

  * <span data-ttu-id="ab736-295">Upewnij się, że **uwierzytelnianie** jest ustawione na wartość **bez uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="ab736-295">Confirm that **Authentication** is set to **No Authentication**.</span></span>
  * <span data-ttu-id="ab736-296">Jeśli jest prezentowana opcja wyboru **platformy docelowej**, wybierz najnowszą wersję 2. x.</span><span class="sxs-lookup"><span data-stu-id="ab736-296">If presented an option to select a **Target Framework**, select the latest 2.x version.</span></span>

  <span data-ttu-id="ab736-297">Wybierz opcję **Dalej**.</span><span class="sxs-lookup"><span data-stu-id="ab736-297">Select **Next**.</span></span>

* <span data-ttu-id="ab736-298">Nazwij projekt **Razor PagesMovie**, a następnie wybierz pozycję **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="ab736-298">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="ab736-300">Uruchamianie aplikacji</span><span class="sxs-lookup"><span data-stu-id="ab736-300">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="ab736-301">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab736-301">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="ab736-302">Naciśnij klawisze CTRL + F5, aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="ab736-302">Press Ctrl+F5 to run without the debugger.</span></span>

  <span data-ttu-id="ab736-303">Uruchamianie aplikacji za pomocą <kbd>klawiszy CTRL + F5</kbd> (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="ab736-303">Launching the app with <kbd>Ctrl+F5</kbd> (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ab736-304">Wielu deweloperów woli używać trybu bez debugowania, aby móc szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="ab736-304">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="ab736-305">Program Visual Studio jest uruchamiany [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) i uruchomi aplikację.</span><span class="sxs-lookup"><span data-stu-id="ab736-305">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="ab736-306">Na pasku adresu są wyświetlane inne elementy `localhost:port#` , takie jak `example.com` .</span><span class="sxs-lookup"><span data-stu-id="ab736-306">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ab736-307">Wynika to z tego, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="ab736-307">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="ab736-308">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="ab736-308">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="ab736-309">Podczas tworzenia projektu internetowego w programie Visual Studio dla serwera internetowego jest używany losowy port.</span><span class="sxs-lookup"><span data-stu-id="ab736-309">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="ab736-310">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="ab736-310">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="ab736-311">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="ab736-311">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub:: No-Loc (index)::: Page](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="ab736-313">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="ab736-313">The following image shows the app after consent to tracking is provided:</span></span>

  ![Strona główna lub:: No-Loc (index)::: Page](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-code"></a>[<span data-ttu-id="ab736-315">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="ab736-315">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="ab736-316">Naciśnij <kbd>klawisze CTRL + F5</kbd> , aby uruchomić bez debugera.</span><span class="sxs-lookup"><span data-stu-id="ab736-316">Press <kbd>Ctrl+F5</kbd> to run without the debugger.</span></span>

  <span data-ttu-id="ab736-317">Uruchamianie aplikacji za pomocą <kbd>klawiszy CTRL + F5</kbd> (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="ab736-317">Launching the app with <kbd>Ctrl+F5</kbd> (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ab736-318">Wielu deweloperów woli używać trybu bez debugowania, aby móc szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="ab736-318">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

  <span data-ttu-id="ab736-319">Visual Studio Code uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001` .</span><span class="sxs-lookup"><span data-stu-id="ab736-319">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and goes to `http://localhost:5001`.</span></span> <span data-ttu-id="ab736-320">Na pasku adresu są wyświetlane inne elementy `localhost:port#` , takie jak `example.com` .</span><span class="sxs-lookup"><span data-stu-id="ab736-320">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="ab736-321">Wynika to z tego, że `localhost` jest standardową nazwą hosta dla komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="ab736-321">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="ab736-322">Host lokalny obsługuje tylko żądania sieci Web z komputera lokalnego.</span><span class="sxs-lookup"><span data-stu-id="ab736-322">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="ab736-323">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="ab736-323">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="ab736-324">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="ab736-324">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub:: No-Loc (index)::: Page](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="ab736-326">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="ab736-326">The following image shows the app after you give consent to tracking:</span></span>

  ![Strona główna lub:: No-Loc (index)::: Page](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="ab736-328">Visual Studio dla komputerów Mac</span><span class="sxs-lookup"><span data-stu-id="ab736-328">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="ab736-329">Naciśnij **polecenie cmd-opt-F5** , aby uruchomić program bez debugera.</span><span class="sxs-lookup"><span data-stu-id="ab736-329">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="ab736-330">Uruchamianie aplikacji za pomocą <kbd>polecenia CMD + OPT + F5</kbd> (tryb bez debugowania) umożliwia wprowadzanie zmian w kodzie, zapisywanie pliku, odświeżanie przeglądarki i wyświetlanie zmian w kodzie.</span><span class="sxs-lookup"><span data-stu-id="ab736-330">Launching the app with <kbd>Cmd+Opt+F5</kbd> (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="ab736-331">Wielu deweloperów woli używać trybu bez debugowania, aby móc szybko uruchomić aplikację i wyświetlić zmiany.</span><span class="sxs-lookup"><span data-stu-id="ab736-331">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

  <span data-ttu-id="ab736-332">Program Visual Studio uruchamia [Kestrel](xref:fundamentals/servers/kestrel), uruchamia przeglądarkę i przechodzi do `http://localhost:5001` .</span><span class="sxs-lookup"><span data-stu-id="ab736-332">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and goes to `http://localhost:5001`.</span></span>

* <span data-ttu-id="ab736-333">Na stronie głównej aplikacji wybierz pozycję **Akceptuj** , aby wyrazić zgodę na śledzenie.</span><span class="sxs-lookup"><span data-stu-id="ab736-333">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="ab736-334">Ta aplikacja nie śledzi informacji osobistych, ale szablon projektu zawiera funkcję zgody na wypadek, gdyby była niezbędna do przestrzegania Ogólne rozporządzenie o ochronie danych Unii Europejskiej [(Rodo)](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="ab736-334">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Strona główna lub:: No-Loc (index)::: Page](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="ab736-336">Na poniższej ilustracji przedstawiono aplikację po udzieleniu zgody na śledzenie:</span><span class="sxs-lookup"><span data-stu-id="ab736-336">The following image shows the app after consent to tracking is provided:</span></span>

  ![Strona główna lub:: No-Loc (index)::: Page](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="ab736-338">Sprawdzanie plików projektu</span><span class="sxs-lookup"><span data-stu-id="ab736-338">Examine the project files</span></span>

<span data-ttu-id="ab736-339">Poniżej przedstawiono Omówienie folderów i plików projektu głównego, z których będziesz korzystać w kolejnych samouczkach.</span><span class="sxs-lookup"><span data-stu-id="ab736-339">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="ab736-340">Folder stron</span><span class="sxs-lookup"><span data-stu-id="ab736-340">Pages folder</span></span>

<span data-ttu-id="ab736-341">Zawiera Razor strony i pliki pomocnicze.</span><span class="sxs-lookup"><span data-stu-id="ab736-341">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="ab736-342">Każda Razor strona to para plików:</span><span class="sxs-lookup"><span data-stu-id="ab736-342">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="ab736-343">Plik *. cshtml* , który zawiera znaczniki HTML z kodem C# używając Razor składni.</span><span class="sxs-lookup"><span data-stu-id="ab736-343">A *.cshtml* file that has HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="ab736-344">Plik *. cshtml.cs* , który ma kod C#, który obsługuje zdarzenia strony.</span><span class="sxs-lookup"><span data-stu-id="ab736-344">A *.cshtml.cs* file that has C# code that handles page events.</span></span>

<span data-ttu-id="ab736-345">Pliki pomocnicze mają nazwy zaczynające się od znaku podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="ab736-345">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="ab736-346">Na przykład plik *_Layout. cshtml* służy do konfigurowania elementów interfejsu użytkownika wspólnych dla wszystkich stron.</span><span class="sxs-lookup"><span data-stu-id="ab736-346">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="ab736-347">Ten plik konfiguruje menu nawigacji w górnej części strony i informacje o prawach autorskich w dolnej części strony.</span><span class="sxs-lookup"><span data-stu-id="ab736-347">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="ab736-348">Aby uzyskać więcej informacji, zobacz <xref:mvc/views/layout>.</span><span class="sxs-lookup"><span data-stu-id="ab736-348">For more information, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="ab736-349">Razor Strony pochodzą od `PageModel` .</span><span class="sxs-lookup"><span data-stu-id="ab736-349">Razor Pages are derived from `PageModel`.</span></span> <span data-ttu-id="ab736-350">Zgodnie z Konwencją `PageModel` Klasa pochodna ma nazwę `<PageName>Model` .</span><span class="sxs-lookup"><span data-stu-id="ab736-350">By convention, the `PageModel`-derived class is named `<PageName>Model`.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="ab736-351">folder wwwroot</span><span class="sxs-lookup"><span data-stu-id="ab736-351">wwwroot folder</span></span>

<span data-ttu-id="ab736-352">Zawiera pliki statyczne, takie jak pliki HTML, pliki JavaScript i pliki CSS.</span><span class="sxs-lookup"><span data-stu-id="ab736-352">Contains static files, like HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="ab736-353">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/static-files>.</span><span class="sxs-lookup"><span data-stu-id="ab736-353">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="ab736-354">appSettings.jsna</span><span class="sxs-lookup"><span data-stu-id="ab736-354">appSettings.json</span></span>

<span data-ttu-id="ab736-355">Zawiera dane konfiguracji, takie jak parametry połączenia.</span><span class="sxs-lookup"><span data-stu-id="ab736-355">Contains configuration data, like connection strings.</span></span> <span data-ttu-id="ab736-356">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="ab736-356">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="ab736-357">Program.cs</span><span class="sxs-lookup"><span data-stu-id="ab736-357">Program.cs</span></span>

<span data-ttu-id="ab736-358">Zawiera punkt wejścia dla programu.</span><span class="sxs-lookup"><span data-stu-id="ab736-358">Contains the entry point for the program.</span></span> <span data-ttu-id="ab736-359">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="ab736-359">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="ab736-360">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="ab736-360">Startup.cs</span></span>

<span data-ttu-id="ab736-361">Zawiera kod, który konfiguruje zachowanie aplikacji, na przykład czy wymaga zgody na polecenie cookie s.</span><span class="sxs-lookup"><span data-stu-id="ab736-361">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="ab736-362">Aby uzyskać więcej informacji, zobacz <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="ab736-362">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ab736-363">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="ab736-363">Additional resources</span></span>

* [<span data-ttu-id="ab736-364">Wersja tego samouczka usługi YouTube</span><span class="sxs-lookup"><span data-stu-id="ab736-364">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="ab736-365">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="ab736-365">Next steps</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ab736-366">Dalej: Dodawanie modelu</span><span class="sxs-lookup"><span data-stu-id="ab736-366">Next: Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
