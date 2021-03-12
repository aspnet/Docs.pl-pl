---
title: Używanie zestawów startowych hostingu w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak ulepszyć aplikację ASP.NET Core z zestawu zewnętrznego przy użyciu implementacji IHostingStartup.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/26/2019
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
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: b39215a70f990afeb7d3fe0a62981113b154354e
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102588246"
---
# <a name="use-hosting-startup-assemblies-in-aspnet-core"></a><span data-ttu-id="8ae9f-103">Używanie zestawów startowych hostingu w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8ae9f-103">Use hosting startup assemblies in ASP.NET Core</span></span>

<span data-ttu-id="8ae9f-104">Autor [Pavel Krymets](https://github.com/pakrym)</span><span class="sxs-lookup"><span data-stu-id="8ae9f-104">By [Pavel Krymets](https://github.com/pakrym)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8ae9f-105"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup>Implementacja (uruchamianie hostingu) dodaje usprawnienia do aplikacji podczas uruchamiania z zestawu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-105">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="8ae9f-106">Na przykład biblioteka zewnętrzna może użyć implementacji uruchamiania hostingu, aby zapewnić dodatkowym dostawcom konfiguracji lub usługom aplikację.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="8ae9f-107">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8ae9f-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="8ae9f-108">HostingStartup — atrybut</span><span class="sxs-lookup"><span data-stu-id="8ae9f-108">HostingStartup attribute</span></span>

<span data-ttu-id="8ae9f-109">Atrybut [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) wskazuje obecność zestawu startowego hostingu do aktywowania w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-109">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="8ae9f-110">Zestaw wpisów lub zestaw zawierający `Startup` klasę jest automatycznie skanowany dla `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-110">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="8ae9f-111">Lista zestawów do wyszukiwania `HostingStartup` atrybutów jest ładowana w czasie wykonywania z konfiguracji w [WebHostDefaults. HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-111">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span></span> <span data-ttu-id="8ae9f-112">Lista zestawów do wykluczenia z odnajdywania jest ładowana z [WebHostDefaults. HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-112">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span></span>

<span data-ttu-id="8ae9f-113">W poniższym przykładzie przestrzeń nazw zestawu startowego hostingu to `StartupEnhancement` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-113">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="8ae9f-114">Klasa zawierająca kod uruchomienia hostingu `StartupEnhancementHostingStartup` :</span><span class="sxs-lookup"><span data-stu-id="8ae9f-114">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="8ae9f-115">Ten `HostingStartup` atrybut zazwyczaj znajduje się w pliku klasy implementacji zestawu startowego hostingu `IHostingStartup` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-115">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="8ae9f-116">Odkryj załadowane zestawy uruchamiania hostingu</span><span class="sxs-lookup"><span data-stu-id="8ae9f-116">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="8ae9f-117">Aby odnaleźć załadowane zestawy uruchamiania hostingu, Włącz rejestrowanie i Sprawdź dzienniki aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-117">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="8ae9f-118">Błędy występujące podczas rejestrowania zestawów.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-118">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="8ae9f-119">Załadowane zestawy startowe hostingu są rejestrowane na poziomie debugowania, a wszystkie błędy są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-119">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="8ae9f-120">Wyłącz automatyczne ładowanie zestawów startowych hostingu</span><span class="sxs-lookup"><span data-stu-id="8ae9f-120">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="8ae9f-121">Aby wyłączyć automatyczne ładowanie zestawów startowych hostingu, należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-121">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="8ae9f-122">Aby zapobiec ładowaniu wszystkich początkowych zestawów uruchamiania, należy ustawić jedną z następujących opcji na `true` lub `1` :</span><span class="sxs-lookup"><span data-stu-id="8ae9f-122">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>

  * <span data-ttu-id="8ae9f-123">Zapobiegaj ustawieniu konfiguracji hosta uruchamiania hostingu:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-123">Prevent Hosting Startup host configuration setting:</span></span>

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseSetting(
                        WebHostDefaults.PreventHostingStartupKey, "true")
                    .UseStartup<Startup>();
            });
    ```

  * <span data-ttu-id="8ae9f-124">`ASPNETCORE_PREVENTHOSTINGSTARTUP` Zmienna środowiskowa.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-124">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

* <span data-ttu-id="8ae9f-125">Aby zapobiec ładowaniu określonych początkowych zestawów uruchamiania, należy ustawić jedną z następujących wartości rozdzielanej średnikami ciąg początkowych zestawów startowych do wykluczenia podczas uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-125">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>

  * <span data-ttu-id="8ae9f-126">Uruchamianie hostingu Wyklucz ustawienia konfiguracja hosta:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-126">Hosting Startup Exclude Assemblies host configuration setting:</span></span>

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseSetting(
                        WebHostDefaults.HostingStartupExcludeAssembliesKey, 
                        "{ASSEMBLY1;ASSEMBLY2; ...}")
                    .UseStartup<Startup>();
            });
    ```

  * <span data-ttu-id="8ae9f-127">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` Zmienna środowiskowa.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-127">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="8ae9f-128">Jeśli ustawienia konfiguracji hosta i zmienna środowiskowa są ustawione, ustawienie hosta steruje zachowaniem.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-128">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="8ae9f-129">Wyłączenie hostingu zestawów startowych przy użyciu ustawienia hosta lub zmiennej środowiskowej wyłącza zestaw globalnie i może wyłączyć kilka cech aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-129">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="8ae9f-130">Project</span><span class="sxs-lookup"><span data-stu-id="8ae9f-130">Project</span></span>

<span data-ttu-id="8ae9f-131">Utwórz uruchamianie hostingu przy użyciu jednego z następujących typów projektów:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-131">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="8ae9f-132">Biblioteka klas</span><span class="sxs-lookup"><span data-stu-id="8ae9f-132">Class library</span></span>](#class-library)
* [<span data-ttu-id="8ae9f-133">Aplikacja konsolowa bez punktu wejścia</span><span class="sxs-lookup"><span data-stu-id="8ae9f-133">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="8ae9f-134">Biblioteka klas</span><span class="sxs-lookup"><span data-stu-id="8ae9f-134">Class library</span></span>

<span data-ttu-id="8ae9f-135">Rozszerzenie uruchamiania hostingu można podać w bibliotece klas.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-135">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="8ae9f-136">Biblioteka zawiera `HostingStartup` atrybut.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-136">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="8ae9f-137">[Przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) zawiera Razor aplikacje Pages, *HostingStartupApp* i Biblioteka klas *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-137">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="8ae9f-138">Biblioteka klas:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-138">The class library:</span></span>

* <span data-ttu-id="8ae9f-139">Zawiera klasę uruchomieniową hostingu, `ServiceKeyInjection` która implementuje `IHostingStartup` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-139">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="8ae9f-140">`ServiceKeyInjection` Dodaje parę ciągów usługi do konfiguracji aplikacji za pomocą dostawcy konfiguracji w pamięci ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-140">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span></span>
* <span data-ttu-id="8ae9f-141">Zawiera `HostingStartup` atrybut, który identyfikuje przestrzeń nazw i klasę uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-141">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="8ae9f-142">`ServiceKeyInjection` <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> Metoda klasy używa <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> do dodawania ulepszeń do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-142">The `ServiceKeyInjection` class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span>

<span data-ttu-id="8ae9f-143">*HostingStartupLibrary/ServiceKeyInjection. cs*:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-143">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="8ae9f-144">Strona indeksu aplikacji odczytuje i renderuje wartości konfiguracyjne dla dwóch kluczy ustawionych przez zestaw startowy obsługujący bibliotekę klas:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-144">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="8ae9f-145">*HostingStartupApp/Pages/index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-145">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="8ae9f-146">[Przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) zawiera również projekt pakietu NuGet, który zapewnia oddzielne uruchamianie hostingu, *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-146">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="8ae9f-147">Pakiet ma takie same charakterystyki biblioteki klas opisanej wcześniej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-147">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="8ae9f-148">Pakiet:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-148">The package:</span></span>

* <span data-ttu-id="8ae9f-149">Zawiera klasę uruchomieniową hostingu, `ServiceKeyInjection` która implementuje `IHostingStartup` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-149">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="8ae9f-150">`ServiceKeyInjection` Dodaje parę ciągów usługi do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-150">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="8ae9f-151">Zawiera `HostingStartup` atrybut.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-151">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="8ae9f-152">*HostingStartupPackage/ServiceKeyInjection. cs*:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-152">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="8ae9f-153">Strona indeksu aplikacji odczytuje i renderuje wartości konfiguracyjne dla dwóch kluczy ustawionych przez zestaw startowy obsługujący pakiet:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-153">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="8ae9f-154">*HostingStartupApp/Pages/index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-154">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="8ae9f-155">Aplikacja konsolowa bez punktu wejścia</span><span class="sxs-lookup"><span data-stu-id="8ae9f-155">Console app without an entry point</span></span>

<span data-ttu-id="8ae9f-156">*Ta metoda jest dostępna tylko dla aplikacji platformy .NET Core, a nie .NET Framework.*</span><span class="sxs-lookup"><span data-stu-id="8ae9f-156">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="8ae9f-157">Rozszerzenie dynamicznego uruchamiania hostingu, które nie wymaga odwołania do czasu kompilowania dla aktywacji, można dostarczyć w aplikacji konsolowej bez punktu wejścia, który zawiera `HostingStartup` atrybut.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-157">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="8ae9f-158">Opublikowanie aplikacji konsolowej tworzy zestaw startowy hostujący, który może być używany z magazynu środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-158">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="8ae9f-159">W tym procesie jest używana aplikacja konsolowa bez punktu wejścia, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-159">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="8ae9f-160">Plik zależności jest wymagany do korzystania z uruchamiania hostingu w zestawie uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-160">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="8ae9f-161">Plik zależności to zasób aplikacji możliwy do uruchomienia, który jest tworzony przez opublikowanie aplikacji, a nie biblioteki.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-161">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="8ae9f-162">Biblioteki nie można dodać bezpośrednio do [magazynu pakietów środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store), który wymaga projektu możliwy do uruchomienia, który jest przeznaczony dla udostępnionego środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-162">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="8ae9f-163">Podczas tworzenia dynamicznego uruchamiania hostingu:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-163">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="8ae9f-164">Zestaw startowy obsługujący hosting jest tworzony z poziomu aplikacji konsolowej bez punktu wejścia, który:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-164">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="8ae9f-165">Zawiera klasę, która zawiera `IHostingStartup` implementację.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-165">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="8ae9f-166">Zawiera atrybut [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) do identyfikacji `IHostingStartup` klasy implementacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-166">Includes a [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="8ae9f-167">Aplikacja konsolowa zostanie opublikowana w celu uzyskania zależności uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-167">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="8ae9f-168">Wynikiem publikowania aplikacji konsolowej jest to, że nieużywane zależności są przycinane z pliku zależności.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-168">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="8ae9f-169">Plik zależności został zmodyfikowany w celu ustawienia lokalizacji środowiska uruchomieniowego zestawu startowego hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-169">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="8ae9f-170">Zestaw startowy hostingu i jego plik zależności są umieszczane w magazynie pakietów środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-170">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="8ae9f-171">Aby odnaleźć zestaw startowy hostingu i jego plik zależności, są one wymienione w parze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-171">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="8ae9f-172">Aplikacja konsolowa odwołuje się do pakietu [Microsoft. AspNetCore. hosting. Abstracts](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) :</span><span class="sxs-lookup"><span data-stu-id="8ae9f-172">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.csproj)]

<span data-ttu-id="8ae9f-173">Atrybut [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) identyfikuje klasę jako implementację `IHostingStartup` do ładowania i wykonywania podczas kompilowania <xref:Microsoft.AspNetCore.Hosting.IWebHost> .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-173">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the <xref:Microsoft.AspNetCore.Hosting.IWebHost>.</span></span> <span data-ttu-id="8ae9f-174">W poniższym przykładzie przestrzeń nazw ma wartość `StartupEnhancement` , a Klasa to `StartupEnhancementHostingStartup` :</span><span class="sxs-lookup"><span data-stu-id="8ae9f-174">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="8ae9f-175">Implementuje klasę `IHostingStartup` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-175">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="8ae9f-176"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*>Metoda klasy używa <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> do dodawania ulepszeń do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-176">The class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span> <span data-ttu-id="8ae9f-177">`IHostingStartup.Configure` w zestawie startowym hostingu jest wywoływany przez środowisko uruchomieniowe przed `Startup.Configure` kodem użytkownika, co umożliwia kod użytkownika zastępowanie dowolnej konfiguracji podanej przez zestaw startowy hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-177">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="8ae9f-178">Podczas kompilowania `IHostingStartup` projektu plik zależności (*.deps.json*) ustawia `runtime` lokalizację zestawu do folderu *bin* :</span><span class="sxs-lookup"><span data-stu-id="8ae9f-178">When building an `IHostingStartup` project, the dependencies file (*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="8ae9f-179">Wyświetlana jest tylko część pliku.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-179">Only part of the file is shown.</span></span> <span data-ttu-id="8ae9f-180">Nazwa zestawu w przykładzie to `StartupEnhancement` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-180">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="8ae9f-181">Konfiguracja dostarczona przez uruchomienie hostingu</span><span class="sxs-lookup"><span data-stu-id="8ae9f-181">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="8ae9f-182">Istnieją dwa podejścia do obsługi konfiguracji w zależności od tego, czy konfiguracja uruchamiania hostingu ma mieć pierwszeństwo, czy konfiguracja aplikacji ma mieć pierwszeństwo:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-182">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="8ae9f-183">Podaj konfigurację aplikacji przy użyciu programu, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> Aby załadować konfigurację po <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> wykonaniu delegatów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-183">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="8ae9f-184">Hostowanie konfiguracji uruchamiania ma wyższy priorytet niż konfiguracja aplikacji przy użyciu tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-184">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="8ae9f-185">Podaj konfigurację aplikacji przy użyciu programu, <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> Aby załadować konfigurację przed <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> wykonaniem delegatów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-185">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="8ae9f-186">Wartości konfiguracyjne aplikacji mają pierwszeństwo przed tymi, które są udostępniane przez uruchamianie hostingu przy użyciu tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-186">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

```csharp
public class ConfigurationInjection : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        Dictionary<string, string> dict;

        builder.ConfigureAppConfiguration(config =>
        {
            dict = new Dictionary<string, string>
            {
                {"ConfigurationKey1", 
                    "From IHostingStartup: Higher priority " +
                    "than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority " +
                "than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="8ae9f-187">Określ zestaw startowy hostingu</span><span class="sxs-lookup"><span data-stu-id="8ae9f-187">Specify the hosting startup assembly</span></span>

<span data-ttu-id="8ae9f-188">W przypadku biblioteki klas lub uruchamiania hostingu obsługiwanej przez aplikację konsoli Określ nazwę zestawu startowego hostingu w `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-188">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="8ae9f-189">Zmienna środowiskowa to rozdzielana średnikami lista zestawów.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-189">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="8ae9f-190">Tylko hosting zestawów startowych jest skanowany dla `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-190">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="8ae9f-191">W przypadku przykładowej aplikacji, *HostingStartupApp*, aby odnaleźć opisane wcześniej uruchomienia hostingu, zmienna środowiskowa jest ustawiona na następującą wartość:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-191">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="8ae9f-192">Zestaw startowy obsługujący hosting można również ustawić przy użyciu ustawienia konfiguracji hosta zestawów uruchamiania hostingu:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-192">A hosting startup assembly can also be set using the Hosting Startup Assemblies host configuration setting:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseSetting(
                    WebHostDefaults.HostingStartupAssembliesKey, 
                    "{ASSEMBLY1;ASSEMBLY2; ...}")
                .UseStartup<Startup>();
        });
```

<span data-ttu-id="8ae9f-193">Gdy są obecne wiele asemblerów uruchamiania, ich <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> metody są wykonywane w kolejności, w jakiej są wymienione zestawy.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-193">When multiple hosting startup assembles are present, their <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="8ae9f-194">Uaktywnienie</span><span class="sxs-lookup"><span data-stu-id="8ae9f-194">Activation</span></span>

<span data-ttu-id="8ae9f-195">Opcje hostingu aktywacji uruchamiania są następujące:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-195">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="8ae9f-196">[Magazyn środowiska uruchomieniowego](#runtime-store): aktywacja nie wymaga odwołania do czasu kompilacji na potrzeby aktywacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-196">[Runtime store](#runtime-store): Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="8ae9f-197">Przykładowa aplikacja umieszcza zestaw startowy obsługujący i pliki zależności w folderze, *wdrożeniu*, aby ułatwić wdrożenie uruchamiania hostingu w środowisku wielomaszynowym.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-197">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="8ae9f-198">Folder *wdrożenia* zawiera także skrypt programu PowerShell, który tworzy lub modyfikuje zmienne środowiskowe w systemie wdrażania, aby umożliwić uruchamianie hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-198">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="8ae9f-199">Odwołanie do czasu kompilacji wymagane do aktywacji</span><span class="sxs-lookup"><span data-stu-id="8ae9f-199">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="8ae9f-200">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="8ae9f-200">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="8ae9f-201">Folder bin projektu</span><span class="sxs-lookup"><span data-stu-id="8ae9f-201">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="8ae9f-202">Magazyn środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="8ae9f-202">Runtime store</span></span>

<span data-ttu-id="8ae9f-203">Implementacja uruchamiania hostingu jest umieszczana w [magazynie w czasie wykonywania](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-203">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="8ae9f-204">Informacje o odwołaniu do zestawu nie są wymagane przez rozszerzoną aplikację.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-204">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="8ae9f-205">Po skompilowaniu uruchomienia hostingu magazyn środowiska uruchomieniowego jest generowany przy użyciu pliku projektu manifestu i polecenia [magazynu dotnet](/dotnet/core/tools/dotnet-store) .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-205">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```dotnetcli
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="8ae9f-206">W przykładowej aplikacji (projekt *RuntimeStore* ) używane jest następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-206">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

```dotnetcli
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="8ae9f-207">Aby środowisko uruchomieniowe wykrywało magazyn środowiska uruchomieniowego, lokalizacja magazynu środowiska uruchomieniowego jest dodawana do `DOTNET_SHARED_STORE` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-207">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="8ae9f-208">**Modyfikuj i umieść plik zależności uruchamiania hostingu**</span><span class="sxs-lookup"><span data-stu-id="8ae9f-208">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="8ae9f-209">Aby aktywować rozszerzanie bez odwołania do pakietu do rozszerzenia, określ dodatkowe zależności od środowiska uruchomieniowego za pomocą programu `additionalDeps` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-209">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="8ae9f-210">`additionalDeps` umożliwia:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-210">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="8ae9f-211">Rozwiń Graf biblioteki aplikacji, dostarczając zestaw dodatkowych *.deps.jsna* plikach do scalenia z własnym *.deps.jsaplikacji podczas* uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-211">Extend the app's library graph by providing a set of additional *.deps.json* files to merge with the app's own *.deps.json* file on startup.</span></span>
* <span data-ttu-id="8ae9f-212">Ustaw możliwość odnajdywania i ładowania zestawu uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-212">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="8ae9f-213">Zalecanym podejściem do generowania dodatkowego pliku zależności jest:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-213">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="8ae9f-214">Wykonaj w `dotnet publish` pliku manifestu magazynu środowiska uruchomieniowego, do którego odwołuje się w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-214">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="8ae9f-215">Usuń odwołanie do manifestu z bibliotek i `runtime` sekcji wyników *.deps.jsw* pliku.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-215">Remove the manifest reference from libraries and the `runtime` section of the resulting *.deps.json* file.</span></span>

<span data-ttu-id="8ae9f-216">W przykładowym projekcie `store.manifest/1.0.0` Właściwość jest usuwana z `targets` `libraries` sekcji i:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-216">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v3.0",
    "signature": ""
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v3.0": {
      "store.manifest/1.0.0": {
        "dependencies": {
          "StartupDiagnostics": "1.0.0"
        },
        "runtime": {
          "store.manifest.dll": {}
        }
      },
      "StartupDiagnostics/1.0.0": {
        "runtime": {
          "lib/netcoreapp3.0/StartupDiagnostics.dll": {
            "assemblyVersion": "1.0.0.0",
            "fileVersion": "1.0.0.0"
          }
        }
      }
    }
  },
  "libraries": {
    "store.manifest/1.0.0": {
      "type": "project",
      "serviceable": false,
      "sha512": ""
    },
    "StartupDiagnostics/1.0.0": {
      "type": "package",
      "serviceable": true,
      "sha512": "sha512-xrhzuNSyM5/f4ZswhooJ9dmIYLP64wMnqUJSyTKVDKDVj5T+qtzypl8JmM/aFJLLpYrf0FYpVWvGujd7/FfMEw==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

<span data-ttu-id="8ae9f-217">Umieść *.deps.jsw* pliku w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-217">Place the *.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="8ae9f-218">`{ADDITIONAL DEPENDENCIES PATH}`: Lokalizacja została dodana do `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-218">`{ADDITIONAL DEPENDENCIES PATH}`: Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="8ae9f-219">`{SHARED FRAMEWORK NAME}`: Udostępnione środowisko wymagane dla tego pliku zależności dodatkowych.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-219">`{SHARED FRAMEWORK NAME}`: Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="8ae9f-220">`{SHARED FRAMEWORK VERSION}`: Minimalna udostępniona wersja platformy.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-220">`{SHARED FRAMEWORK VERSION}`: Minimum shared framework version.</span></span>
* <span data-ttu-id="8ae9f-221">`{ENHANCEMENT ASSEMBLY NAME}`: Nazwa zestawu rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-221">`{ENHANCEMENT ASSEMBLY NAME}`: The enhancement's assembly name.</span></span>

<span data-ttu-id="8ae9f-222">W przykładowej aplikacji (projekt *RuntimeStore* ) plik dodatkowych zależności jest umieszczany w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-222">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
deployment/additionalDeps/shared/Microsoft.AspNetCore.App/3.0.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="8ae9f-223">Aby środowisko uruchomieniowe wykrywało lokalizację magazynu środowiska uruchomieniowego, do zmiennej środowiskowej zostanie dodana lokalizacja pliku z dodatkowymi zależnościami `DOTNET_ADDITIONAL_DEPS` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-223">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="8ae9f-224">W przykładowej aplikacji (projekt *RuntimeStore* ) Kompilowanie magazynu środowiska uruchomieniowego i generowanie pliku dodatkowych zależności odbywa się przy użyciu skryptu [programu PowerShell](/powershell/scripting/powershell-scripting) .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-224">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="8ae9f-225">Przykłady sposobu ustawiania zmiennych środowiskowych dla różnych systemów operacyjnych znajdują się w temacie [Używanie wielu środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-225">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="8ae9f-226">**Wdrożenie**</span><span class="sxs-lookup"><span data-stu-id="8ae9f-226">**Deployment**</span></span>

<span data-ttu-id="8ae9f-227">Aby ułatwić wdrożenie uruchamiania hostingu w środowisku wielokomputerowym, aplikacja Przykładowa tworzy folder *wdrożenia* w opublikowanych danych wyjściowych zawierający:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-227">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="8ae9f-228">Magazyn środowiska uruchomieniowego uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-228">The hosting startup runtime store.</span></span>
* <span data-ttu-id="8ae9f-229">Plik zależności uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-229">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="8ae9f-230">Skrypt programu PowerShell, który tworzy lub modyfikuje `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` , `DOTNET_SHARED_STORE` i `DOTNET_ADDITIONAL_DEPS` w celu obsługi aktywacji uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-230">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="8ae9f-231">Uruchom skrypt z wiersza polecenia administracyjnego programu PowerShell w systemie wdrażania.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-231">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="8ae9f-232">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="8ae9f-232">NuGet package</span></span>

<span data-ttu-id="8ae9f-233">Rozszerzenie uruchamiania hostingu można dostarczyć w pakiecie NuGet.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-233">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="8ae9f-234">Pakiet ma `HostingStartup` atrybut.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-234">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="8ae9f-235">Typy uruchamiania hostingu udostępniane przez pakiet są udostępniane aplikacji przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-235">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="8ae9f-236">Plik projektu aplikacji rozszerzonej udostępnia odwołanie do pakietu dla uruchomienia hostingu w pliku projektu aplikacji (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-236">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="8ae9f-237">Wraz z odwołaniem w czasie kompilacji, zestaw startowy hostingu i wszystkie jego zależności są zawarte w pliku zależności aplikacji (*.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-237">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="8ae9f-238">Takie podejście ma zastosowanie do pakietu zestawu startowego hostingu opublikowanego w [NuGet.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-238">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="8ae9f-239">Plik zależności uruchamiania hostingu jest udostępniany aplikacji rozszerzonej, zgodnie z opisem w sekcji [Magazyn środowiska uruchomieniowego](#runtime-store) (bez odwołania w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-239">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="8ae9f-240">Aby uzyskać więcej informacji na temat pakietów NuGet i magazynu środowiska uruchomieniowego, zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-240">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="8ae9f-241">Jak utworzyć pakiet NuGet przy użyciu narzędzi międzyplatformowych</span><span class="sxs-lookup"><span data-stu-id="8ae9f-241">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="8ae9f-242">Publikowanie pakietów</span><span class="sxs-lookup"><span data-stu-id="8ae9f-242">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="8ae9f-243">Magazyn pakietu środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="8ae9f-243">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="8ae9f-244">Folder bin projektu</span><span class="sxs-lookup"><span data-stu-id="8ae9f-244">Project bin folder</span></span>

<span data-ttu-id="8ae9f-245">Rozszerzanie uruchamiania hostingu może być dostarczone przez zestaw wdrożony przez *bin* w aplikacji rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-245">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="8ae9f-246">Typy uruchamiania hostingu udostępniane przez zestaw są udostępniane aplikacji przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-246">The hosting startup types provided by the assembly are made available to the app using one of the following approaches:</span></span>

* <span data-ttu-id="8ae9f-247">Plik projektu aplikacji rozszerzonej tworzy odwołanie do zestawu do uruchomienia hostingu (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-247">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="8ae9f-248">Wraz z odwołaniem w czasie kompilacji, zestaw startowy hostingu i wszystkie jego zależności są zawarte w pliku zależności aplikacji (*.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-248">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="8ae9f-249">Takie podejście ma zastosowanie, gdy scenariusz wdrażania wywoła odwołanie do zestawu uruchamiania hostingu (plik *. dll* ) i przenosząc zestaw do jednego z tych metod:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-249">This approach applies when the deployment scenario calls for making a compile-time reference to the hosting startup's assembly (*.dll* file) and moving the assembly to either:</span></span>
  * <span data-ttu-id="8ae9f-250">Projekt zużywający.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-250">The consuming project.</span></span>
  * <span data-ttu-id="8ae9f-251">Lokalizacja dostępna przez projekt zużywający.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-251">A location accessible by the consuming project.</span></span>
* <span data-ttu-id="8ae9f-252">Plik zależności uruchamiania hostingu jest udostępniany aplikacji rozszerzonej, zgodnie z opisem w sekcji [Magazyn środowiska uruchomieniowego](#runtime-store) (bez odwołania w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-252">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>
* <span data-ttu-id="8ae9f-253">Podczas określania .NET Framework, zestaw jest ładowany w domyślnym kontekście ładowania, który na .NET Framework oznacza, że zestaw znajduje się w jednej z następujących lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-253">When targeting the .NET Framework, the assembly is loadable in the default load context, which on .NET Framework means that the assembly is located at either of the following locations:</span></span>
  * <span data-ttu-id="8ae9f-254">Ścieżka bazowa aplikacji: folder *bin* , w którym znajduje się plik wykonywalny (*. exe*) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-254">Application base path: The *bin* folder where the app's executable (*.exe*) is located.</span></span>
  * <span data-ttu-id="8ae9f-255">Globalna pamięć podręczna zestawów (GAC): Magazyn GAC przechowuje zestawy, które są dostępne dla kilku .NET Framework aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-255">Global Assembly Cache (GAC): The GAC stores assemblies that several .NET Framework apps share.</span></span> <span data-ttu-id="8ae9f-256">Aby uzyskać więcej informacji, zobacz [jak: Instalowanie zestawu w globalnej pamięci podręcznej zestawów](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) w dokumentacji .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-256">For more information, see [How to: Install an assembly into the global assembly cache](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) in the .NET Framework documentation.</span></span>

## <a name="sample-code"></a><span data-ttu-id="8ae9f-257">Przykładowy kod</span><span class="sxs-lookup"><span data-stu-id="8ae9f-257">Sample code</span></span>

<span data-ttu-id="8ae9f-258">[Przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([jak pobrać](xref:index#how-to-download-a-sample)) pokazuje hosting scenariuszy implementacji uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-258">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="8ae9f-259">Dwa zestawy startowe hostingu (biblioteki klas) ustawiają parę par klucz-wartość konfiguracji w pamięci:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-259">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="8ae9f-260">Pakiet NuGet (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="8ae9f-260">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="8ae9f-261">Biblioteka klas (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="8ae9f-261">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="8ae9f-262">Uruchamianie hostingu jest aktywowane z zestawu wdrożonego w sklepie uruchomieniowym (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-262">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="8ae9f-263">Zestaw dodaje dwie middlewares do aplikacji podczas uruchamiania, które dostarczają informacji diagnostycznych na:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-263">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="8ae9f-264">Zarejestrowane usługi</span><span class="sxs-lookup"><span data-stu-id="8ae9f-264">Registered services</span></span>
  * <span data-ttu-id="8ae9f-265">Adres (schemat, host, baza ścieżki, ścieżka, ciąg zapytania)</span><span class="sxs-lookup"><span data-stu-id="8ae9f-265">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="8ae9f-266">Połączenie (zdalny adres IP, Port zdalny, lokalny adres IP, port lokalny, certyfikat klienta)</span><span class="sxs-lookup"><span data-stu-id="8ae9f-266">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="8ae9f-267">Nagłówki żądań</span><span class="sxs-lookup"><span data-stu-id="8ae9f-267">Request headers</span></span>
  * <span data-ttu-id="8ae9f-268">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="8ae9f-268">Environment variables</span></span>

<span data-ttu-id="8ae9f-269">Aby uruchomić przykład:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-269">To run the sample:</span></span>

<span data-ttu-id="8ae9f-270">**Aktywacja z pakietu NuGet**</span><span class="sxs-lookup"><span data-stu-id="8ae9f-270">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="8ae9f-271">Skompiluj pakiet *HostingStartupPackage* przy użyciu polecenia [pakietu dotnet Pack](/dotnet/core/tools/dotnet-pack) .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-271">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="8ae9f-272">Dodaj nazwę zestawu pakietu *HostingStartupPackage* do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-272">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="8ae9f-273">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-273">Compile and run the app.</span></span> <span data-ttu-id="8ae9f-274">Odwołanie do pakietu jest obecne w aplikacji rozszerzonej (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-274">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="8ae9f-275">`<PropertyGroup>`W pliku projektu aplikacji określa dane wyjściowe projektu pakietu (*.. /HostingStartupPackage/bin/Debug*) jako źródło pakietu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-275">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="8ae9f-276">Dzięki temu aplikacja może korzystać z pakietu bez przekazywania pakietu do [NuGet.org](https://www.nuget.org/). Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-276">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. <span data-ttu-id="8ae9f-277">Zwróć uwagę, że wartości klucza konfiguracji usługi renderowane przez stronę indeksu są zgodne z wartościami ustawionymi przez `ServiceKeyInjection.Configure` metodę pakietu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-277">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="8ae9f-278">Jeśli wprowadzisz zmiany w projekcie *HostingStartupPackage* i skompilujesz go ponownie, wyczyść pamięć podręczną pakietów NuGet, aby upewnić się, że *HostingStartupApp* odbierze zaktualizowany pakiet, a nie stary pakiet z lokalnej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-278">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="8ae9f-279">Aby wyczyścić lokalne pamięci podręczne NuGet, należy wykonać następujące polecenie [locale NuGet programu dotnet](/dotnet/core/tools/dotnet-nuget-locals) :</span><span class="sxs-lookup"><span data-stu-id="8ae9f-279">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```dotnetcli
dotnet nuget locals all --clear
```

<span data-ttu-id="8ae9f-280">**Aktywacja z biblioteki klas**</span><span class="sxs-lookup"><span data-stu-id="8ae9f-280">**Activation from a class library**</span></span>

1. <span data-ttu-id="8ae9f-281">Skompiluj bibliotekę klas *HostingStartupLibrary* za pomocą polecenia [kompilacji dotnet](/dotnet/core/tools/dotnet-build) .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-281">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="8ae9f-282">Dodaj nazwę zestawu *HostingStartupLibrary* biblioteki klas do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-282">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="8ae9f-283">*bin*— Wdróż zestaw biblioteki klas w aplikacji, kopiując plik *HostingStartupLibrary.dll* z skompilowanych danych wyjściowych biblioteki klas do folderu *bin/debug* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-283">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="8ae9f-284">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-284">Compile and run the app.</span></span> <span data-ttu-id="8ae9f-285">`<ItemGroup>`Plik projektu aplikacji odwołuje się do zestawu biblioteki klas (*.\bin\Debug\netcoreapp3.0\HostingStartupLibrary.dll*) (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-285">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp3.0\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="8ae9f-286">Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-286">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\\bin\\Debug\\netcoreapp3.0\\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp3.0\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion> 
     </Reference>
   </ItemGroup>
   ```

1. <span data-ttu-id="8ae9f-287">Zwróć uwagę, że wartości klucza konfiguracji usługi renderowane przez stronę indeksu są zgodne z wartościami ustawionymi przez metodę biblioteki klas `ServiceKeyInjection.Configure` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-287">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="8ae9f-288">**Aktywacja z zestawu wdrożonego w sklepie uruchomieniowym**</span><span class="sxs-lookup"><span data-stu-id="8ae9f-288">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="8ae9f-289">Projekt *StartupDiagnostics* używa [programu PowerShell](/powershell/scripting/powershell-scripting) do modyfikacji *StartupDiagnostics.deps.jsw* pliku.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-289">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="8ae9f-290">Program PowerShell jest instalowany domyślnie w systemie Windows, począwszy od systemu Windows 7 z dodatkiem SP1 i Windows Server 2008 R2 z dodatkiem SP1.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-290">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="8ae9f-291">Aby uzyskać program PowerShell na innych platformach, zobacz [Instalowanie różnych wersji programu PowerShell](/powershell/scripting/install/installing-powershell).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-291">To obtain PowerShell on other platforms, see [Installing various versions of PowerShell](/powershell/scripting/install/installing-powershell).</span></span>
1. <span data-ttu-id="8ae9f-292">Wykonaj skrypt *build.ps1* w folderze *RuntimeStore* .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-292">Execute the *build.ps1* script in the *RuntimeStore* folder.</span></span> <span data-ttu-id="8ae9f-293">Skrypt:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-293">The script:</span></span>
   * <span data-ttu-id="8ae9f-294">Generuje `StartupDiagnostics` pakiet w folderze *obj\packages* .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-294">Generates the `StartupDiagnostics` package in the *obj\packages* folder.</span></span>
   * <span data-ttu-id="8ae9f-295">Generuje magazyn środowiska uruchomieniowego dla `StartupDiagnostics` programu w folderze *Store* .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-295">Generates the runtime store for `StartupDiagnostics` in the *store* folder.</span></span> <span data-ttu-id="8ae9f-296">`dotnet store`Polecenie w skrypcie używa `win7-x64` [identyfikatora środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) do uruchomienia hostingu wdrożonego w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-296">The `dotnet store` command in the script uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog) for a hosting startup deployed to Windows.</span></span> <span data-ttu-id="8ae9f-297">Podczas zapewniania uruchamiania hostingu dla innego środowiska uruchomieniowego należy zastąpić prawidłowy identyfikator RID w wierszu 37 skryptu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-297">When providing the hosting startup for a different runtime, substitute the correct RID on line 37 of the script.</span></span> <span data-ttu-id="8ae9f-298">Magazyn środowiska uruchomieniowego programu `StartupDiagnostics` będzie później przenoszony do magazynu środowiska uruchomieniowego użytkownika lub systemu na komputerze, na którym zostanie użyty zestaw.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-298">The runtime store for `StartupDiagnostics` would later be moved to the user's or system's runtime store on the machine where the assembly will be consumed.</span></span> <span data-ttu-id="8ae9f-299">Lokalizacja instalacji magazynu środowiska uruchomieniowego użytkownika dla `StartupDiagnostics` zestawu to *. dotnet/Store/x64/netcoreapp 3.0/startupdiagnostics/1.0.0/lib/netcoreapp 3.0/StartupDiagnostics.dll*.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-299">The user runtime store install location for the `StartupDiagnostics` assembly is *.dotnet/store/x64/netcoreapp3.0/startupdiagnostics/1.0.0/lib/netcoreapp3.0/StartupDiagnostics.dll*.</span></span>
   * <span data-ttu-id="8ae9f-300">Generuje `additionalDeps` dla elementu `StartupDiagnostics` w folderze *additionalDeps* .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-300">Generates the `additionalDeps` for `StartupDiagnostics` in the *additionalDeps* folder.</span></span> <span data-ttu-id="8ae9f-301">Dodatkowe zależności byłyby później przenoszone do dodatkowych zależności użytkownika lub systemu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-301">The additional dependencies would later be moved to the user's or system's additional dependencies.</span></span> <span data-ttu-id="8ae9f-302">`StartupDiagnostics`Dodatkowa lokalizacja instalacji zależności użytkownika to *. dotnet/x64/AdditionalDeps/StartupDiagnostics/Shared/Microsoft. servicecore. app/3.0.0/StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-302">The user `StartupDiagnostics` additional dependencies install location is *.dotnet/x64/additionalDeps/StartupDiagnostics/shared/Microsoft.NETCore.App/3.0.0/StartupDiagnostics.deps.json*.</span></span>
   * <span data-ttu-id="8ae9f-303">Umieszcza plik *deploy.ps1* w folderze *wdrożenia* .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-303">Places the *deploy.ps1* file in the *deployment* folder.</span></span>
1. <span data-ttu-id="8ae9f-304">Uruchom skrypt *deploy.ps1* w folderze *Deployment* .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-304">Run the *deploy.ps1* script in the *deployment* folder.</span></span> <span data-ttu-id="8ae9f-305">Dołączenie skryptu:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-305">The script appends:</span></span>
   * <span data-ttu-id="8ae9f-306">`StartupDiagnostics` do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-306">`StartupDiagnostics` to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="8ae9f-307">Ścieżka zależności uruchamiania hostingu (w folderze *wdrażania* projektu RuntimeStore) do `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-307">The hosting startup dependencies path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
   * <span data-ttu-id="8ae9f-308">Ścieżka magazynu środowiska uruchomieniowego (w folderze *wdrażania* projektu RuntimeStore) do `DOTNET_SHARED_STORE` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-308">The runtime store path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_SHARED_STORE` environment variable.</span></span>
1. <span data-ttu-id="8ae9f-309">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-309">Run the sample app.</span></span>
1. <span data-ttu-id="8ae9f-310">Zażądaj `/services` punktu końcowego, aby zobaczyć zarejestrowane usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-310">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="8ae9f-311">Zażądaj `/diag` punktu końcowego, aby wyświetlić informacje diagnostyczne.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-311">Request the `/diag` endpoint to see the diagnostic information.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8ae9f-312"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup>Implementacja (uruchamianie hostingu) dodaje usprawnienia do aplikacji podczas uruchamiania z zestawu zewnętrznego.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-312">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="8ae9f-313">Na przykład biblioteka zewnętrzna może użyć implementacji uruchamiania hostingu, aby zapewnić dodatkowym dostawcom konfiguracji lub usługom aplikację.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-313">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="8ae9f-314">[Wyświetl lub pobierz przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([jak pobrać](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8ae9f-314">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="8ae9f-315">HostingStartup — atrybut</span><span class="sxs-lookup"><span data-stu-id="8ae9f-315">HostingStartup attribute</span></span>

<span data-ttu-id="8ae9f-316">Atrybut [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) wskazuje obecność zestawu startowego hostingu do aktywowania w czasie wykonywania.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-316">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="8ae9f-317">Zestaw wpisów lub zestaw zawierający `Startup` klasę jest automatycznie skanowany dla `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-317">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="8ae9f-318">Lista zestawów do wyszukiwania `HostingStartup` atrybutów jest ładowana w czasie wykonywania z konfiguracji w [WebHostDefaults. HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-318">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span></span> <span data-ttu-id="8ae9f-319">Lista zestawów do wykluczenia z odnajdywania jest ładowana z [WebHostDefaults. HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-319">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span></span> <span data-ttu-id="8ae9f-320">Aby uzyskać więcej informacji, zobacz [host sieci Web: hosting zestawów startowych](xref:fundamentals/host/web-host#hosting-startup-assemblies) i [hosta sieci Web: hosting z wykluczeniem uruchomienia](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-320">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="8ae9f-321">W poniższym przykładzie przestrzeń nazw zestawu startowego hostingu to `StartupEnhancement` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-321">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="8ae9f-322">Klasa zawierająca kod uruchomienia hostingu `StartupEnhancementHostingStartup` :</span><span class="sxs-lookup"><span data-stu-id="8ae9f-322">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="8ae9f-323">Ten `HostingStartup` atrybut zazwyczaj znajduje się w pliku klasy implementacji zestawu startowego hostingu `IHostingStartup` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-323">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="8ae9f-324">Odkryj załadowane zestawy uruchamiania hostingu</span><span class="sxs-lookup"><span data-stu-id="8ae9f-324">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="8ae9f-325">Aby odnaleźć załadowane zestawy uruchamiania hostingu, Włącz rejestrowanie i Sprawdź dzienniki aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-325">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="8ae9f-326">Błędy występujące podczas rejestrowania zestawów.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-326">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="8ae9f-327">Załadowane zestawy startowe hostingu są rejestrowane na poziomie debugowania, a wszystkie błędy są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-327">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="8ae9f-328">Wyłącz automatyczne ładowanie zestawów startowych hostingu</span><span class="sxs-lookup"><span data-stu-id="8ae9f-328">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="8ae9f-329">Aby wyłączyć automatyczne ładowanie zestawów startowych hostingu, należy użyć jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-329">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="8ae9f-330">Aby zapobiec ładowaniu wszystkich początkowych zestawów uruchamiania, należy ustawić jedną z następujących opcji na `true` lub `1` :</span><span class="sxs-lookup"><span data-stu-id="8ae9f-330">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="8ae9f-331">[Zapobiegaj](xref:fundamentals/host/web-host#prevent-hosting-startup) ustawieniu konfiguracji hosta uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-331">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="8ae9f-332">`ASPNETCORE_PREVENTHOSTINGSTARTUP` Zmienna środowiskowa.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-332">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="8ae9f-333">Aby zapobiec ładowaniu określonych początkowych zestawów uruchamiania, należy ustawić jedną z następujących wartości rozdzielanej średnikami ciąg początkowych zestawów startowych do wykluczenia podczas uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-333">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="8ae9f-334">[Uruchamianie hostingu Wyklucz](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) ustawienia konfiguracja hosta.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-334">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="8ae9f-335">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` Zmienna środowiskowa.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-335">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="8ae9f-336">Jeśli ustawienia konfiguracji hosta i zmienna środowiskowa są ustawione, ustawienie hosta steruje zachowaniem.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-336">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="8ae9f-337">Wyłączenie hostingu zestawów startowych przy użyciu ustawienia hosta lub zmiennej środowiskowej wyłącza zestaw globalnie i może wyłączyć kilka cech aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-337">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="8ae9f-338">Project</span><span class="sxs-lookup"><span data-stu-id="8ae9f-338">Project</span></span>

<span data-ttu-id="8ae9f-339">Utwórz uruchamianie hostingu przy użyciu jednego z następujących typów projektów:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-339">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="8ae9f-340">Biblioteka klas</span><span class="sxs-lookup"><span data-stu-id="8ae9f-340">Class library</span></span>](#class-library)
* [<span data-ttu-id="8ae9f-341">Aplikacja konsolowa bez punktu wejścia</span><span class="sxs-lookup"><span data-stu-id="8ae9f-341">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="8ae9f-342">Biblioteka klas</span><span class="sxs-lookup"><span data-stu-id="8ae9f-342">Class library</span></span>

<span data-ttu-id="8ae9f-343">Rozszerzenie uruchamiania hostingu można podać w bibliotece klas.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-343">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="8ae9f-344">Biblioteka zawiera `HostingStartup` atrybut.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-344">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="8ae9f-345">[Przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) zawiera Razor aplikacje Pages, *HostingStartupApp* i Biblioteka klas *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-345">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="8ae9f-346">Biblioteka klas:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-346">The class library:</span></span>

* <span data-ttu-id="8ae9f-347">Zawiera klasę uruchomieniową hostingu, `ServiceKeyInjection` która implementuje `IHostingStartup` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-347">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="8ae9f-348">`ServiceKeyInjection` Dodaje parę ciągów usługi do konfiguracji aplikacji za pomocą dostawcy konfiguracji w pamięci ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-348">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span></span>
* <span data-ttu-id="8ae9f-349">Zawiera `HostingStartup` atrybut, który identyfikuje przestrzeń nazw i klasę uruchomienia hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-349">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="8ae9f-350">`ServiceKeyInjection` <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> Metoda klasy używa <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> do dodawania ulepszeń do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-350">The `ServiceKeyInjection` class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span>

<span data-ttu-id="8ae9f-351">*HostingStartupLibrary/ServiceKeyInjection. cs*:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-351">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="8ae9f-352">Strona indeksu aplikacji odczytuje i renderuje wartości konfiguracyjne dla dwóch kluczy ustawionych przez zestaw startowy obsługujący bibliotekę klas:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-352">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="8ae9f-353">*HostingStartupApp/Pages/index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-353">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="8ae9f-354">[Przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) zawiera również projekt pakietu NuGet, który zapewnia oddzielne uruchamianie hostingu, *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-354">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="8ae9f-355">Pakiet ma takie same charakterystyki biblioteki klas opisanej wcześniej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-355">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="8ae9f-356">Pakiet:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-356">The package:</span></span>

* <span data-ttu-id="8ae9f-357">Zawiera klasę uruchomieniową hostingu, `ServiceKeyInjection` która implementuje `IHostingStartup` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-357">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="8ae9f-358">`ServiceKeyInjection` Dodaje parę ciągów usługi do konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-358">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="8ae9f-359">Zawiera `HostingStartup` atrybut.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-359">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="8ae9f-360">*HostingStartupPackage/ServiceKeyInjection. cs*:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-360">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="8ae9f-361">Strona indeksu aplikacji odczytuje i renderuje wartości konfiguracyjne dla dwóch kluczy ustawionych przez zestaw startowy obsługujący pakiet:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-361">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="8ae9f-362">*HostingStartupApp/Pages/index. cshtml. cs*:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-362">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="8ae9f-363">Aplikacja konsolowa bez punktu wejścia</span><span class="sxs-lookup"><span data-stu-id="8ae9f-363">Console app without an entry point</span></span>

<span data-ttu-id="8ae9f-364">*Ta metoda jest dostępna tylko dla aplikacji platformy .NET Core, a nie .NET Framework.*</span><span class="sxs-lookup"><span data-stu-id="8ae9f-364">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="8ae9f-365">Rozszerzenie dynamicznego uruchamiania hostingu, które nie wymaga odwołania do czasu kompilowania dla aktywacji, można dostarczyć w aplikacji konsolowej bez punktu wejścia, który zawiera `HostingStartup` atrybut.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-365">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="8ae9f-366">Opublikowanie aplikacji konsolowej tworzy zestaw startowy hostujący, który może być używany z magazynu środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-366">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="8ae9f-367">W tym procesie jest używana aplikacja konsolowa bez punktu wejścia, ponieważ:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-367">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="8ae9f-368">Plik zależności jest wymagany do korzystania z uruchamiania hostingu w zestawie uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-368">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="8ae9f-369">Plik zależności to zasób aplikacji możliwy do uruchomienia, który jest tworzony przez opublikowanie aplikacji, a nie biblioteki.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-369">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="8ae9f-370">Biblioteki nie można dodać bezpośrednio do [magazynu pakietów środowiska uruchomieniowego](/dotnet/core/deploying/runtime-store), który wymaga projektu możliwy do uruchomienia, który jest przeznaczony dla udostępnionego środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-370">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="8ae9f-371">Podczas tworzenia dynamicznego uruchamiania hostingu:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-371">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="8ae9f-372">Zestaw startowy obsługujący hosting jest tworzony z poziomu aplikacji konsolowej bez punktu wejścia, który:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-372">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="8ae9f-373">Zawiera klasę, która zawiera `IHostingStartup` implementację.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-373">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="8ae9f-374">Zawiera atrybut [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) do identyfikacji `IHostingStartup` klasy implementacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-374">Includes a [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="8ae9f-375">Aplikacja konsolowa zostanie opublikowana w celu uzyskania zależności uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-375">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="8ae9f-376">Wynikiem publikowania aplikacji konsolowej jest to, że nieużywane zależności są przycinane z pliku zależności.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-376">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="8ae9f-377">Plik zależności został zmodyfikowany w celu ustawienia lokalizacji środowiska uruchomieniowego zestawu startowego hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-377">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="8ae9f-378">Zestaw startowy hostingu i jego plik zależności są umieszczane w magazynie pakietów środowiska uruchomieniowego.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-378">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="8ae9f-379">Aby odnaleźć zestaw startowy hostingu i jego plik zależności, są one wymienione w parze zmiennych środowiskowych.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-379">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="8ae9f-380">Aplikacja konsolowa odwołuje się do pakietu [Microsoft. AspNetCore. hosting. Abstracts](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) :</span><span class="sxs-lookup"><span data-stu-id="8ae9f-380">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="8ae9f-381">Atrybut [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) identyfikuje klasę jako implementację `IHostingStartup` do ładowania i wykonywania podczas kompilowania <xref:Microsoft.AspNetCore.Hosting.IWebHost> .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-381">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the <xref:Microsoft.AspNetCore.Hosting.IWebHost>.</span></span> <span data-ttu-id="8ae9f-382">W poniższym przykładzie przestrzeń nazw ma wartość `StartupEnhancement` , a Klasa to `StartupEnhancementHostingStartup` :</span><span class="sxs-lookup"><span data-stu-id="8ae9f-382">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="8ae9f-383">Implementuje klasę `IHostingStartup` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-383">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="8ae9f-384"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*>Metoda klasy używa <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> do dodawania ulepszeń do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-384">The class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span> <span data-ttu-id="8ae9f-385">`IHostingStartup.Configure` w zestawie startowym hostingu jest wywoływany przez środowisko uruchomieniowe przed `Startup.Configure` kodem użytkownika, co umożliwia kod użytkownika zastępowanie dowolnej konfiguracji podanej przez zestaw startowy hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-385">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="8ae9f-386">Podczas kompilowania `IHostingStartup` projektu plik zależności (*.deps.json*) ustawia `runtime` lokalizację zestawu do folderu *bin* :</span><span class="sxs-lookup"><span data-stu-id="8ae9f-386">When building an `IHostingStartup` project, the dependencies file (*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="8ae9f-387">Wyświetlana jest tylko część pliku.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-387">Only part of the file is shown.</span></span> <span data-ttu-id="8ae9f-388">Nazwa zestawu w przykładzie to `StartupEnhancement` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-388">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="8ae9f-389">Konfiguracja dostarczona przez uruchomienie hostingu</span><span class="sxs-lookup"><span data-stu-id="8ae9f-389">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="8ae9f-390">Istnieją dwa podejścia do obsługi konfiguracji w zależności od tego, czy konfiguracja uruchamiania hostingu ma mieć pierwszeństwo, czy konfiguracja aplikacji ma mieć pierwszeństwo:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-390">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="8ae9f-391">Podaj konfigurację aplikacji przy użyciu programu, <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> Aby załadować konfigurację po <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> wykonaniu delegatów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-391">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="8ae9f-392">Hostowanie konfiguracji uruchamiania ma wyższy priorytet niż konfiguracja aplikacji przy użyciu tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-392">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="8ae9f-393">Podaj konfigurację aplikacji przy użyciu programu, <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> Aby załadować konfigurację przed <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> wykonaniem delegatów aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-393">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="8ae9f-394">Wartości konfiguracyjne aplikacji mają pierwszeństwo przed tymi, które są udostępniane przez uruchamianie hostingu przy użyciu tego podejścia.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-394">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

```csharp
public class ConfigurationInjection : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        Dictionary<string, string> dict;

        builder.ConfigureAppConfiguration(config =>
        {
            dict = new Dictionary<string, string>
            {
                {"ConfigurationKey1", 
                    "From IHostingStartup: Higher priority " +
                    "than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority " +
                "than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="8ae9f-395">Określ zestaw startowy hostingu</span><span class="sxs-lookup"><span data-stu-id="8ae9f-395">Specify the hosting startup assembly</span></span>

<span data-ttu-id="8ae9f-396">W przypadku biblioteki klas lub uruchamiania hostingu obsługiwanej przez aplikację konsoli Określ nazwę zestawu startowego hostingu w `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-396">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="8ae9f-397">Zmienna środowiskowa to rozdzielana średnikami lista zestawów.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-397">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="8ae9f-398">Tylko hosting zestawów startowych jest skanowany dla `HostingStartup` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-398">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="8ae9f-399">W przypadku przykładowej aplikacji, *HostingStartupApp*, aby odnaleźć opisane wcześniej uruchomienia hostingu, zmienna środowiskowa jest ustawiona na następującą wartość:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-399">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="8ae9f-400">Zestaw startowy obsługujący hosting można również ustawić przy użyciu ustawienia konfiguracji hosta [zestawów startowych](xref:fundamentals/host/web-host#hosting-startup-assemblies) .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-400">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="8ae9f-401">Gdy są obecne wiele asemblerów uruchamiania, ich <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> metody są wykonywane w kolejności, w jakiej są wymienione zestawy.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-401">When multiple hosting startup assembles are present, their <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="8ae9f-402">Uaktywnienie</span><span class="sxs-lookup"><span data-stu-id="8ae9f-402">Activation</span></span>

<span data-ttu-id="8ae9f-403">Opcje hostingu aktywacji uruchamiania są następujące:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-403">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="8ae9f-404">[Magazyn środowiska uruchomieniowego](#runtime-store): aktywacja nie wymaga odwołania do czasu kompilacji na potrzeby aktywacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-404">[Runtime store](#runtime-store): Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="8ae9f-405">Przykładowa aplikacja umieszcza zestaw startowy obsługujący i pliki zależności w folderze, *wdrożeniu*, aby ułatwić wdrożenie uruchamiania hostingu w środowisku wielomaszynowym.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-405">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="8ae9f-406">Folder *wdrożenia* zawiera także skrypt programu PowerShell, który tworzy lub modyfikuje zmienne środowiskowe w systemie wdrażania, aby umożliwić uruchamianie hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-406">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="8ae9f-407">Odwołanie do czasu kompilacji wymagane do aktywacji</span><span class="sxs-lookup"><span data-stu-id="8ae9f-407">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="8ae9f-408">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="8ae9f-408">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="8ae9f-409">Folder bin projektu</span><span class="sxs-lookup"><span data-stu-id="8ae9f-409">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="8ae9f-410">Magazyn środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="8ae9f-410">Runtime store</span></span>

<span data-ttu-id="8ae9f-411">Implementacja uruchamiania hostingu jest umieszczana w [magazynie w czasie wykonywania](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-411">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="8ae9f-412">Informacje o odwołaniu do zestawu nie są wymagane przez rozszerzoną aplikację.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-412">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="8ae9f-413">Po skompilowaniu uruchomienia hostingu magazyn środowiska uruchomieniowego jest generowany przy użyciu pliku projektu manifestu i polecenia [magazynu dotnet](/dotnet/core/tools/dotnet-store) .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-413">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```dotnetcli
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="8ae9f-414">W przykładowej aplikacji (projekt *RuntimeStore* ) używane jest następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-414">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

```dotnetcli
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="8ae9f-415">Aby środowisko uruchomieniowe wykrywało magazyn środowiska uruchomieniowego, lokalizacja magazynu środowiska uruchomieniowego jest dodawana do `DOTNET_SHARED_STORE` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-415">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="8ae9f-416">**Modyfikuj i umieść plik zależności uruchamiania hostingu**</span><span class="sxs-lookup"><span data-stu-id="8ae9f-416">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="8ae9f-417">Aby aktywować rozszerzanie bez odwołania do pakietu do rozszerzenia, określ dodatkowe zależności od środowiska uruchomieniowego za pomocą programu `additionalDeps` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-417">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="8ae9f-418">`additionalDeps` umożliwia:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-418">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="8ae9f-419">Rozwiń Graf biblioteki aplikacji, dostarczając zestaw dodatkowych *.deps.jsna* plikach do scalenia z własnym *.deps.jsaplikacji podczas* uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-419">Extend the app's library graph by providing a set of additional *.deps.json* files to merge with the app's own *.deps.json* file on startup.</span></span>
* <span data-ttu-id="8ae9f-420">Ustaw możliwość odnajdywania i ładowania zestawu uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-420">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="8ae9f-421">Zalecanym podejściem do generowania dodatkowego pliku zależności jest:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-421">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="8ae9f-422">Wykonaj w `dotnet publish` pliku manifestu magazynu środowiska uruchomieniowego, do którego odwołuje się w poprzedniej sekcji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-422">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="8ae9f-423">Usuń odwołanie do manifestu z bibliotek i `runtime` sekcji wyników *.deps.jsw* pliku.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-423">Remove the manifest reference from libraries and the `runtime` section of the resulting *.deps.json* file.</span></span>

<span data-ttu-id="8ae9f-424">W przykładowym projekcie `store.manifest/1.0.0` Właściwość jest usuwana z `targets` `libraries` sekcji i:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-424">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v2.1",
    "signature": "4ea77c7b75ad1895ae1ea65e6ba2399010514f99"
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v2.1": {
      "store.manifest/1.0.0": {
        "dependencies": {
          "StartupDiagnostics": "1.0.0"
        },
        "runtime": {
          "store.manifest.dll": {}
        }
      },
      "StartupDiagnostics/1.0.0": {
        "runtime": {
          "lib/netcoreapp2.1/StartupDiagnostics.dll": {
            "assemblyVersion": "1.0.0.0",
            "fileVersion": "1.0.0.0"
          }
        }
      }
    }
  },
  "libraries": {
    "store.manifest/1.0.0": {
      "type": "project",
      "serviceable": false,
      "sha512": ""
    },
    "StartupDiagnostics/1.0.0": {
      "type": "package",
      "serviceable": true,
      "sha512": "sha512-oiQr60vBQW7+nBTmgKLSldj06WNLRTdhOZpAdEbCuapoZ+M2DJH2uQbRLvFT8EGAAv4TAKzNtcztpx5YOgBXQQ==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

<span data-ttu-id="8ae9f-425">Umieść *.deps.jsw* pliku w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-425">Place the *.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="8ae9f-426">`{ADDITIONAL DEPENDENCIES PATH}`: Lokalizacja została dodana do `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-426">`{ADDITIONAL DEPENDENCIES PATH}`: Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="8ae9f-427">`{SHARED FRAMEWORK NAME}`: Udostępnione środowisko wymagane dla tego pliku zależności dodatkowych.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-427">`{SHARED FRAMEWORK NAME}`: Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="8ae9f-428">`{SHARED FRAMEWORK VERSION}`: Minimalna udostępniona wersja platformy.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-428">`{SHARED FRAMEWORK VERSION}`: Minimum shared framework version.</span></span>
* <span data-ttu-id="8ae9f-429">`{ENHANCEMENT ASSEMBLY NAME}`: Nazwa zestawu rozszerzeń.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-429">`{ENHANCEMENT ASSEMBLY NAME}`: The enhancement's assembly name.</span></span>

<span data-ttu-id="8ae9f-430">W przykładowej aplikacji (projekt *RuntimeStore* ) plik dodatkowych zależności jest umieszczany w następującej lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-430">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
deployment/additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="8ae9f-431">Aby środowisko uruchomieniowe wykrywało lokalizację magazynu środowiska uruchomieniowego, do zmiennej środowiskowej zostanie dodana lokalizacja pliku z dodatkowymi zależnościami `DOTNET_ADDITIONAL_DEPS` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-431">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="8ae9f-432">W przykładowej aplikacji (projekt *RuntimeStore* ) Kompilowanie magazynu środowiska uruchomieniowego i generowanie pliku dodatkowych zależności odbywa się przy użyciu skryptu [programu PowerShell](/powershell/scripting/powershell-scripting) .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-432">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="8ae9f-433">Przykłady sposobu ustawiania zmiennych środowiskowych dla różnych systemów operacyjnych znajdują się w temacie [Używanie wielu środowisk](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-433">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="8ae9f-434">**Wdrożenie**</span><span class="sxs-lookup"><span data-stu-id="8ae9f-434">**Deployment**</span></span>

<span data-ttu-id="8ae9f-435">Aby ułatwić wdrożenie uruchamiania hostingu w środowisku wielokomputerowym, aplikacja Przykładowa tworzy folder *wdrożenia* w opublikowanych danych wyjściowych zawierający:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-435">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="8ae9f-436">Magazyn środowiska uruchomieniowego uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-436">The hosting startup runtime store.</span></span>
* <span data-ttu-id="8ae9f-437">Plik zależności uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-437">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="8ae9f-438">Skrypt programu PowerShell, który tworzy lub modyfikuje `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` , `DOTNET_SHARED_STORE` i `DOTNET_ADDITIONAL_DEPS` w celu obsługi aktywacji uruchamiania hostingu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-438">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="8ae9f-439">Uruchom skrypt z wiersza polecenia administracyjnego programu PowerShell w systemie wdrażania.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-439">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="8ae9f-440">Pakiet NuGet</span><span class="sxs-lookup"><span data-stu-id="8ae9f-440">NuGet package</span></span>

<span data-ttu-id="8ae9f-441">Rozszerzenie uruchamiania hostingu można dostarczyć w pakiecie NuGet.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-441">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="8ae9f-442">Pakiet ma `HostingStartup` atrybut.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-442">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="8ae9f-443">Typy uruchamiania hostingu udostępniane przez pakiet są udostępniane aplikacji przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-443">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="8ae9f-444">Plik projektu aplikacji rozszerzonej udostępnia odwołanie do pakietu dla uruchomienia hostingu w pliku projektu aplikacji (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-444">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="8ae9f-445">Wraz z odwołaniem w czasie kompilacji, zestaw startowy hostingu i wszystkie jego zależności są zawarte w pliku zależności aplikacji (*.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-445">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="8ae9f-446">Takie podejście ma zastosowanie do pakietu zestawu startowego hostingu opublikowanego w [NuGet.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-446">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="8ae9f-447">Plik zależności uruchamiania hostingu jest udostępniany aplikacji rozszerzonej, zgodnie z opisem w sekcji [Magazyn środowiska uruchomieniowego](#runtime-store) (bez odwołania w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-447">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="8ae9f-448">Aby uzyskać więcej informacji na temat pakietów NuGet i magazynu środowiska uruchomieniowego, zobacz następujące tematy:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-448">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="8ae9f-449">Jak utworzyć pakiet NuGet przy użyciu narzędzi międzyplatformowych</span><span class="sxs-lookup"><span data-stu-id="8ae9f-449">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="8ae9f-450">Publikowanie pakietów</span><span class="sxs-lookup"><span data-stu-id="8ae9f-450">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="8ae9f-451">Magazyn pakietu środowiska uruchomieniowego</span><span class="sxs-lookup"><span data-stu-id="8ae9f-451">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="8ae9f-452">Folder bin projektu</span><span class="sxs-lookup"><span data-stu-id="8ae9f-452">Project bin folder</span></span>

<span data-ttu-id="8ae9f-453">Rozszerzanie uruchamiania hostingu może być dostarczone przez zestaw wdrożony przez *bin* w aplikacji rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-453">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="8ae9f-454">Typy uruchamiania hostingu udostępniane przez zestaw są udostępniane aplikacji przy użyciu jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-454">The hosting startup types provided by the assembly are made available to the app using one of the following approaches:</span></span>

* <span data-ttu-id="8ae9f-455">Plik projektu aplikacji rozszerzonej tworzy odwołanie do zestawu do uruchomienia hostingu (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-455">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="8ae9f-456">Wraz z odwołaniem w czasie kompilacji, zestaw startowy hostingu i wszystkie jego zależności są zawarte w pliku zależności aplikacji (*.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-456">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="8ae9f-457">Takie podejście ma zastosowanie, gdy scenariusz wdrażania wywoła odwołanie do zestawu uruchamiania hostingu (plik *. dll* ) i przenosząc zestaw do jednego z tych metod:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-457">This approach applies when the deployment scenario calls for making a compile-time reference to the hosting startup's assembly (*.dll* file) and moving the assembly to either:</span></span>
  * <span data-ttu-id="8ae9f-458">Projekt zużywający.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-458">The consuming project.</span></span>
  * <span data-ttu-id="8ae9f-459">Lokalizacja dostępna przez projekt zużywający.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-459">A location accessible by the consuming project.</span></span>
* <span data-ttu-id="8ae9f-460">Plik zależności uruchamiania hostingu jest udostępniany aplikacji rozszerzonej, zgodnie z opisem w sekcji [Magazyn środowiska uruchomieniowego](#runtime-store) (bez odwołania w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-460">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>
* <span data-ttu-id="8ae9f-461">Podczas określania .NET Framework, zestaw jest ładowany w domyślnym kontekście ładowania, który na .NET Framework oznacza, że zestaw znajduje się w jednej z następujących lokalizacji:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-461">When targeting the .NET Framework, the assembly is loadable in the default load context, which on .NET Framework means that the assembly is located at either of the following locations:</span></span>
  * <span data-ttu-id="8ae9f-462">Ścieżka bazowa aplikacji: folder *bin* , w którym znajduje się plik wykonywalny (*. exe*) aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-462">Application base path: The *bin* folder where the app's executable (*.exe*) is located.</span></span>
  * <span data-ttu-id="8ae9f-463">Globalna pamięć podręczna zestawów (GAC): Magazyn GAC przechowuje zestawy, które są dostępne dla kilku .NET Framework aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-463">Global Assembly Cache (GAC): The GAC stores assemblies that several .NET Framework apps share.</span></span> <span data-ttu-id="8ae9f-464">Aby uzyskać więcej informacji, zobacz [jak: Instalowanie zestawu w globalnej pamięci podręcznej zestawów](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) w dokumentacji .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-464">For more information, see [How to: Install an assembly into the global assembly cache](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) in the .NET Framework documentation.</span></span>

## <a name="sample-code"></a><span data-ttu-id="8ae9f-465">Przykładowy kod</span><span class="sxs-lookup"><span data-stu-id="8ae9f-465">Sample code</span></span>

<span data-ttu-id="8ae9f-466">[Przykładowy kod](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([jak pobrać](xref:index#how-to-download-a-sample)) pokazuje hosting scenariuszy implementacji uruchamiania:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-466">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="8ae9f-467">Dwa zestawy startowe hostingu (biblioteki klas) ustawiają parę par klucz-wartość konfiguracji w pamięci:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-467">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="8ae9f-468">Pakiet NuGet (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="8ae9f-468">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="8ae9f-469">Biblioteka klas (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="8ae9f-469">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="8ae9f-470">Uruchamianie hostingu jest aktywowane z zestawu wdrożonego w sklepie uruchomieniowym (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-470">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="8ae9f-471">Zestaw dodaje dwie middlewares do aplikacji podczas uruchamiania, które dostarczają informacji diagnostycznych na:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-471">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="8ae9f-472">Zarejestrowane usługi</span><span class="sxs-lookup"><span data-stu-id="8ae9f-472">Registered services</span></span>
  * <span data-ttu-id="8ae9f-473">Adres (schemat, host, baza ścieżki, ścieżka, ciąg zapytania)</span><span class="sxs-lookup"><span data-stu-id="8ae9f-473">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="8ae9f-474">Połączenie (zdalny adres IP, Port zdalny, lokalny adres IP, port lokalny, certyfikat klienta)</span><span class="sxs-lookup"><span data-stu-id="8ae9f-474">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="8ae9f-475">Nagłówki żądań</span><span class="sxs-lookup"><span data-stu-id="8ae9f-475">Request headers</span></span>
  * <span data-ttu-id="8ae9f-476">Zmienne środowiskowe</span><span class="sxs-lookup"><span data-stu-id="8ae9f-476">Environment variables</span></span>

<span data-ttu-id="8ae9f-477">Aby uruchomić przykład:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-477">To run the sample:</span></span>

<span data-ttu-id="8ae9f-478">**Aktywacja z pakietu NuGet**</span><span class="sxs-lookup"><span data-stu-id="8ae9f-478">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="8ae9f-479">Skompiluj pakiet *HostingStartupPackage* przy użyciu polecenia [pakietu dotnet Pack](/dotnet/core/tools/dotnet-pack) .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-479">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="8ae9f-480">Dodaj nazwę zestawu pakietu *HostingStartupPackage* do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-480">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="8ae9f-481">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-481">Compile and run the app.</span></span> <span data-ttu-id="8ae9f-482">Odwołanie do pakietu jest obecne w aplikacji rozszerzonej (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-482">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="8ae9f-483">`<PropertyGroup>`W pliku projektu aplikacji określa dane wyjściowe projektu pakietu (*.. /HostingStartupPackage/bin/Debug*) jako źródło pakietu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-483">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="8ae9f-484">Dzięki temu aplikacja może korzystać z pakietu bez przekazywania pakietu do [NuGet.org](https://www.nuget.org/). Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-484">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. <span data-ttu-id="8ae9f-485">Zwróć uwagę, że wartości klucza konfiguracji usługi renderowane przez stronę indeksu są zgodne z wartościami ustawionymi przez `ServiceKeyInjection.Configure` metodę pakietu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-485">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="8ae9f-486">Jeśli wprowadzisz zmiany w projekcie *HostingStartupPackage* i skompilujesz go ponownie, wyczyść pamięć podręczną pakietów NuGet, aby upewnić się, że *HostingStartupApp* odbierze zaktualizowany pakiet, a nie stary pakiet z lokalnej pamięci podręcznej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-486">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="8ae9f-487">Aby wyczyścić lokalne pamięci podręczne NuGet, należy wykonać następujące polecenie [locale NuGet programu dotnet](/dotnet/core/tools/dotnet-nuget-locals) :</span><span class="sxs-lookup"><span data-stu-id="8ae9f-487">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```dotnetcli
dotnet nuget locals all --clear
```

<span data-ttu-id="8ae9f-488">**Aktywacja z biblioteki klas**</span><span class="sxs-lookup"><span data-stu-id="8ae9f-488">**Activation from a class library**</span></span>

1. <span data-ttu-id="8ae9f-489">Skompiluj bibliotekę klas *HostingStartupLibrary* za pomocą polecenia [kompilacji dotnet](/dotnet/core/tools/dotnet-build) .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-489">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="8ae9f-490">Dodaj nazwę zestawu *HostingStartupLibrary* biblioteki klas do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-490">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="8ae9f-491">*bin*— Wdróż zestaw biblioteki klas w aplikacji, kopiując plik *HostingStartupLibrary.dll* z skompilowanych danych wyjściowych biblioteki klas do folderu *bin/debug* aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-491">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="8ae9f-492">Skompiluj i uruchom aplikację.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-492">Compile and run the app.</span></span> <span data-ttu-id="8ae9f-493">`<ItemGroup>`Plik projektu aplikacji odwołuje się do zestawu biblioteki klas (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (odwołanie w czasie kompilacji).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-493">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="8ae9f-494">Aby uzyskać więcej informacji, zobacz uwagi w pliku projektu HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-494">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\\bin\\Debug\\netcoreapp2.1\\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```

1. <span data-ttu-id="8ae9f-495">Zwróć uwagę, że wartości klucza konfiguracji usługi renderowane przez stronę indeksu są zgodne z wartościami ustawionymi przez metodę biblioteki klas `ServiceKeyInjection.Configure` .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-495">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="8ae9f-496">**Aktywacja z zestawu wdrożonego w sklepie uruchomieniowym**</span><span class="sxs-lookup"><span data-stu-id="8ae9f-496">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="8ae9f-497">Projekt *StartupDiagnostics* używa [programu PowerShell](/powershell/scripting/powershell-scripting) do modyfikacji *StartupDiagnostics.deps.jsw* pliku.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-497">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="8ae9f-498">Program PowerShell jest instalowany domyślnie w systemie Windows, począwszy od systemu Windows 7 z dodatkiem SP1 i Windows Server 2008 R2 z dodatkiem SP1.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-498">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="8ae9f-499">Aby uzyskać program PowerShell na innych platformach, zobacz [Instalowanie różnych wersji programu PowerShell](/powershell/scripting/install/installing-powershell).</span><span class="sxs-lookup"><span data-stu-id="8ae9f-499">To obtain PowerShell on other platforms, see [Installing various versions of PowerShell](/powershell/scripting/install/installing-powershell).</span></span>
1. <span data-ttu-id="8ae9f-500">Wykonaj skrypt *build.ps1* w folderze *RuntimeStore* .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-500">Execute the *build.ps1* script in the *RuntimeStore* folder.</span></span> <span data-ttu-id="8ae9f-501">Skrypt:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-501">The script:</span></span>
   * <span data-ttu-id="8ae9f-502">Generuje `StartupDiagnostics` pakiet w folderze *obj\packages* .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-502">Generates the `StartupDiagnostics` package in the *obj\packages* folder.</span></span>
   * <span data-ttu-id="8ae9f-503">Generuje magazyn środowiska uruchomieniowego dla `StartupDiagnostics` programu w folderze *Store* .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-503">Generates the runtime store for `StartupDiagnostics` in the *store* folder.</span></span> <span data-ttu-id="8ae9f-504">`dotnet store`Polecenie w skrypcie używa `win7-x64` [identyfikatora środowiska uruchomieniowego (RID)](/dotnet/core/rid-catalog) do uruchomienia hostingu wdrożonego w systemie Windows.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-504">The `dotnet store` command in the script uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog) for a hosting startup deployed to Windows.</span></span> <span data-ttu-id="8ae9f-505">Podczas zapewniania uruchamiania hostingu dla innego środowiska uruchomieniowego należy zastąpić prawidłowy identyfikator RID w wierszu 37 skryptu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-505">When providing the hosting startup for a different runtime, substitute the correct RID on line 37 of the script.</span></span> <span data-ttu-id="8ae9f-506">Magazyn środowiska uruchomieniowego programu `StartupDiagnostics` będzie później przenoszony do magazynu środowiska uruchomieniowego użytkownika lub systemu na komputerze, na którym zostanie użyty zestaw.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-506">The runtime store for `StartupDiagnostics` would later be moved to the user's or system's runtime store on the machine where the assembly will be consumed.</span></span> <span data-ttu-id="8ae9f-507">Lokalizacja instalacji magazynu środowiska uruchomieniowego użytkownika dla `StartupDiagnostics` zestawu to *. dotnet/Store/x64/netcoreapp 2.2/startupdiagnostics/1.0.0/lib/netcoreapp 2.2/StartupDiagnostics.dll*.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-507">The user runtime store install location for the `StartupDiagnostics` assembly is *.dotnet/store/x64/netcoreapp2.2/startupdiagnostics/1.0.0/lib/netcoreapp2.2/StartupDiagnostics.dll*.</span></span>
   * <span data-ttu-id="8ae9f-508">Generuje `additionalDeps` dla elementu `StartupDiagnostics` w folderze *additionalDeps* .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-508">Generates the `additionalDeps` for `StartupDiagnostics` in the *additionalDeps* folder.</span></span> <span data-ttu-id="8ae9f-509">Dodatkowe zależności byłyby później przenoszone do dodatkowych zależności użytkownika lub systemu.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-509">The additional dependencies would later be moved to the user's or system's additional dependencies.</span></span> <span data-ttu-id="8ae9f-510">`StartupDiagnostics`Dodatkowa lokalizacja instalacji zależności użytkownika to *. dotnet/x64/AdditionalDeps/StartupDiagnostics/Shared/Microsoft. servicecore. App/2.2.0/StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-510">The user `StartupDiagnostics` additional dependencies install location is *.dotnet/x64/additionalDeps/StartupDiagnostics/shared/Microsoft.NETCore.App/2.2.0/StartupDiagnostics.deps.json*.</span></span>
   * <span data-ttu-id="8ae9f-511">Umieszcza plik *deploy.ps1* w folderze *wdrożenia* .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-511">Places the *deploy.ps1* file in the *deployment* folder.</span></span>
1. <span data-ttu-id="8ae9f-512">Uruchom skrypt *deploy.ps1* w folderze *Deployment* .</span><span class="sxs-lookup"><span data-stu-id="8ae9f-512">Run the *deploy.ps1* script in the *deployment* folder.</span></span> <span data-ttu-id="8ae9f-513">Dołączenie skryptu:</span><span class="sxs-lookup"><span data-stu-id="8ae9f-513">The script appends:</span></span>
   * <span data-ttu-id="8ae9f-514">`StartupDiagnostics` do `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-514">`StartupDiagnostics` to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="8ae9f-515">Ścieżka zależności uruchamiania hostingu (w folderze *wdrażania* projektu RuntimeStore) do `DOTNET_ADDITIONAL_DEPS` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-515">The hosting startup dependencies path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
   * <span data-ttu-id="8ae9f-516">Ścieżka magazynu środowiska uruchomieniowego (w folderze *wdrażania* projektu RuntimeStore) do `DOTNET_SHARED_STORE` zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-516">The runtime store path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_SHARED_STORE` environment variable.</span></span>
1. <span data-ttu-id="8ae9f-517">Uruchom przykładową aplikację.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-517">Run the sample app.</span></span>
1. <span data-ttu-id="8ae9f-518">Zażądaj `/services` punktu końcowego, aby zobaczyć zarejestrowane usługi aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-518">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="8ae9f-519">Zażądaj `/diag` punktu końcowego, aby wyświetlić informacje diagnostyczne.</span><span class="sxs-lookup"><span data-stu-id="8ae9f-519">Request the `/diag` endpoint to see the diagnostic information.</span></span>

::: moniker-end
