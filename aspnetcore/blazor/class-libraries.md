---
title: Biblioteki klas składników razor
author: guardrex
description: Dowiedz się, jak składniki mogły zostać uwzględnione w taki sposób, w aplikacji Blazor z biblioteki składników zewnętrznych.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: blazor/class-libraries
ms.openlocfilehash: f7c9ce20bf23bc532e664764d6e48d9163db727f
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982983"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="b649a-103">Biblioteki klas składników razor</span><span class="sxs-lookup"><span data-stu-id="b649a-103">Razor components class libraries</span></span>

<span data-ttu-id="b649a-104">Przez [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="b649a-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="b649a-105">Składniki mogą być udostępniane w bibliotekach klas Razor w projektach.</span><span class="sxs-lookup"><span data-stu-id="b649a-105">Components can be shared in Razor class libraries across projects.</span></span> <span data-ttu-id="b649a-106">Składniki można uwzględnić od:</span><span class="sxs-lookup"><span data-stu-id="b649a-106">Components can be included from:</span></span>

* <span data-ttu-id="b649a-107">Innego projektu w rozwiązaniu.</span><span class="sxs-lookup"><span data-stu-id="b649a-107">Another project in the solution.</span></span>
* <span data-ttu-id="b649a-108">Pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="b649a-108">A NuGet package.</span></span>
* <span data-ttu-id="b649a-109">Odwołania biblioteki .NET.</span><span class="sxs-lookup"><span data-stu-id="b649a-109">A referenced .NET library.</span></span>

<span data-ttu-id="b649a-110">Tak, jak składniki są regularnie typów .NET, składniki dostarczony przez biblioteki klas Razor są normalne zestawów platformy .NET.</span><span class="sxs-lookup"><span data-stu-id="b649a-110">Just as components are regular .NET types, components provided by Razor class libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="b649a-111">Użyj `razorclasslib` szablonu (biblioteki klas Razor) za pomocą [dotnet nowe](/dotnet/core/tools/dotnet-new) polecenia:</span><span class="sxs-lookup"><span data-stu-id="b649a-111">Use the `razorclasslib` (Razor class library) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command:</span></span>

```console
dotnet new razorclasslib -o MyComponentLib1
```

<span data-ttu-id="b649a-112">Dodaj pliki składników Razor (*.razor*) do biblioteki klas Razor.</span><span class="sxs-lookup"><span data-stu-id="b649a-112">Add Razor component files (*.razor*) to the Razor class library.</span></span>

<span data-ttu-id="b649a-113">Aby dodać bibliotekę do istniejącego projektu, należy użyć [dotnet sln](/dotnet/core/tools/dotnet-sln) polecenia:</span><span class="sxs-lookup"><span data-stu-id="b649a-113">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b649a-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b649a-114">Visual Studio</span></span>](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b649a-115">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="b649a-115">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> <span data-ttu-id="b649a-116">Biblioteki klas razor nie są zgodne z aplikacjami Blazor w programie ASP.NET Core w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b649a-116">Razor class libraries aren't compatible with Blazor apps in ASP.NET Core Preview 4.</span></span>
>
> <span data-ttu-id="b649a-117">Do tworzenia składników w bibliotece, które mogą być udostępniane innym Blazor po stronie klienta i Razor składniki po stronie serwera aplikacji, użyj utworzonych przez bibliotekę klas Blazor `blazorlib` szablonu.</span><span class="sxs-lookup"><span data-stu-id="b649a-117">To create components in a library that can be shared with Blazor client-side and Razor components server-side apps, use a Blazor class library created by the `blazorlib` template.</span></span>
>
> <span data-ttu-id="b649a-118">Biblioteki klas razor statycznych zasobów nie są obsługiwane w programie ASP.NET Core w wersji zapoznawczej 4.</span><span class="sxs-lookup"><span data-stu-id="b649a-118">Razor class libraries don't support static assets in ASP.NET Core Preview 4.</span></span> <span data-ttu-id="b649a-119">Biblioteki składników za pomocą `blazorlib` szablonu może zawierać pliki statyczne, takie jak obrazy, JavaScript i arkusze stylów.</span><span class="sxs-lookup"><span data-stu-id="b649a-119">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="b649a-120">W czasie kompilacji, pliki statyczne są osadzone w pliku zestawu wbudowanego (*.dll*), co umożliwia użycie składniki bez konieczności martwienia się o tym, jak do uwzględnienia ich zasobów.</span><span class="sxs-lookup"><span data-stu-id="b649a-120">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="b649a-121">Wszystkie pliki zawarte w `content` katalogu są oznaczone jako zasobu osadzonego.</span><span class="sxs-lookup"><span data-stu-id="b649a-121">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="b649a-122">Używanie składnik biblioteki</span><span class="sxs-lookup"><span data-stu-id="b649a-122">Consume a library component</span></span>

<span data-ttu-id="b649a-123">Aby można było używać składników zdefiniowane w bibliotece w innym projekcie, użyj jednej z następujących metod:</span><span class="sxs-lookup"><span data-stu-id="b649a-123">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="b649a-124">Pełna nazwa typu z przestrzenią nazw.</span><span class="sxs-lookup"><span data-stu-id="b649a-124">Full type name with the namespace.</span></span>
* <span data-ttu-id="b649a-125">Firmy razor [ \@przy użyciu](xref:mvc/views/razor#using) dyrektywy.</span><span class="sxs-lookup"><span data-stu-id="b649a-125">Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="b649a-126">Poszczególne składniki mogą być dodawane według nazwy.</span><span class="sxs-lookup"><span data-stu-id="b649a-126">Individual components may be added by name.</span></span>

<span data-ttu-id="b649a-127">W poniższych przykładach `MyComponentLibrary` to biblioteka składnika zawierająca raport sprzedaży (`SalesReport`) składnika.</span><span class="sxs-lookup"><span data-stu-id="b649a-127">In the following examples, `MyComponentLibrary` is a component library containing the Sales Report (`SalesReport`) component.</span></span>

<span data-ttu-id="b649a-128">Składnik raport sprzedaży mogą być przywoływane z przestrzenią nazw przy użyciu jego pełna nazwa typu:</span><span class="sxs-lookup"><span data-stu-id="b649a-128">The Sales Report component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLibrary.SalesReport />
```

<span data-ttu-id="b649a-129">Składnik również mogą być przywoływane, jeśli biblioteka zostanie przełączony w tryb do zakresu za pomocą `@using` dyrektywy:</span><span class="sxs-lookup"><span data-stu-id="b649a-129">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLibrary

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="b649a-130">`@using` Mogą być dołączane dyrektywą *_Import.razor* dokonać składników dostępne dla całego projektu lub zastosowane pojedynczej strony lub zbiór stron w folderze.</span><span class="sxs-lookup"><span data-stu-id="b649a-130">The `@using` directive can be included in *_Import.razor* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="b649a-131">Kompilacji, pakiet i dostarczanie na potrzeby narzędzia NuGet</span><span class="sxs-lookup"><span data-stu-id="b649a-131">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="b649a-132">Ponieważ biblioteki składnik to biblioteki .NET standard, pakowania i wysyłania ich do narzędzia NuGet nie różni się od pakowania i wysyłania każdą bibliotekę do narzędzia NuGet.</span><span class="sxs-lookup"><span data-stu-id="b649a-132">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="b649a-133">Pakowanie odbywa się przy użyciu [pakietu dotnet](/dotnet/core/tools/dotnet-pack) polecenia:</span><span class="sxs-lookup"><span data-stu-id="b649a-133">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="b649a-134">Przekazywanie pakietu NuGet za pomocą [dotnet nuget publikowania](/dotnet/core/tools/dotnet-nuget-push) polecenia:</span><span class="sxs-lookup"><span data-stu-id="b649a-134">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="b649a-135">Korzystając z `blazorlib` szablonu, zasoby statyczne są dołączone do pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="b649a-135">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="b649a-136">Konsumenci biblioteki automatycznie otrzymywać skrypty i arkusze stylów, dzięki czemu użytkownicy nie są wymagane do ręcznego zainstalowania zasobów.</span><span class="sxs-lookup"><span data-stu-id="b649a-136">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>