---
title: Moduł ASP.NET Core
author: rick-anderson
description: Dowiedz się więcej na temat modułu ASP.NET Core na potrzeby hostowania aplikacji ASP.NET Core za pomocą usług IIS.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/13/2020
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
uid: host-and-deploy/aspnet-core-module
ms.openlocfilehash: ffa2a7c73e00ccfb5dd726a084352c0b80cc41b0
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102586972"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="49e70-103">Moduł ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49e70-103">ASP.NET Core Module</span></span>

<span data-ttu-id="49e70-104">Autorzy [Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Krzysztof Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [sourabh Shirhatti](https://twitter.com/sshirhatti)i [Justin Kotalik](https://github.com/jkotalik)</span><span class="sxs-lookup"><span data-stu-id="49e70-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), [Chris Ross](https://github.com/Tratcher), [Rick Anderson](https://twitter.com/RickAndMSFT), [Sourabh Shirhatti](https://twitter.com/sshirhatti), and [Justin Kotalik](https://github.com/jkotalik)</span></span>

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="49e70-105">Moduł ASP.NET Core jest natywnym modułem usług IIS, który jest podłączany do potoku usług IIS, dzięki czemu aplikacje ASP.NET Core mogą współdziałać z usługami IIS.</span><span class="sxs-lookup"><span data-stu-id="49e70-105">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline, allowing ASP.NET Core applications to work with IIS.</span></span> <span data-ttu-id="49e70-106">Uruchom ASP.NET Core aplikacje za pomocą usług IIS:</span><span class="sxs-lookup"><span data-stu-id="49e70-106">Run ASP.NET Core apps with IIS by either:</span></span> 

* <span data-ttu-id="49e70-107">Hostowanie aplikacji ASP.NET Core w procesie roboczym usług IIS ( `w3wp.exe` ), nazywanym [modelem hostingu w procesie](xref:host-and-deploy/iis/in-process-hosting).</span><span class="sxs-lookup"><span data-stu-id="49e70-107">Hosting an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](xref:host-and-deploy/iis/in-process-hosting).</span></span>
* <span data-ttu-id="49e70-108">Przekazywanie żądań sieci Web do zaplecza ASP.NET Core aplikacji, na których uruchomiono serwer Kestrel, nazywany [modelem hostingu poza procesem](xref:host-and-deploy/iis/out-of-process-hosting).</span><span class="sxs-lookup"><span data-stu-id="49e70-108">Forwarding web requests to a backend ASP.NET Core app running the Kestrel server, called the [out-of-process hosting model](xref:host-and-deploy/iis/out-of-process-hosting).</span></span>

<span data-ttu-id="49e70-109">Istnieją kompromisy między poszczególnymi modelami hostingu.</span><span class="sxs-lookup"><span data-stu-id="49e70-109">There are trade-offs between each of the hosting models.</span></span> <span data-ttu-id="49e70-110">Domyślnie model hostingu w procesie jest używany z powodu lepszej wydajności i diagnostyki.</span><span class="sxs-lookup"><span data-stu-id="49e70-110">By default, the in-process hosting model is used due to better performance and diagnostics.</span></span>

## <a name="install-aspnet-core-module"></a><span data-ttu-id="49e70-111">Zainstaluj moduł ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49e70-111">Install ASP.NET Core Module</span></span>

<span data-ttu-id="49e70-112">Pobierz instalatora przy użyciu następującego linku:</span><span class="sxs-lookup"><span data-stu-id="49e70-112">Download the installer using the following link:</span></span>

[<span data-ttu-id="49e70-113">Bieżący Instalator pakietu hostingu platformy .NET Core (Pobieranie bezpośrednie)</span><span class="sxs-lookup"><span data-stu-id="49e70-113">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

<span data-ttu-id="49e70-114">Aby uzyskać szczegółowe informacje na temat sposobu instalowania modułu ASP.NET Core lub instalowania różnych wersji modułu, zobacz [Instalowanie zestawu hostingu platformy .NET Core](xref:host-and-deploy/iis/hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="49e70-114">For more details instructions on how to install the ASP.NET Core Module, or installing different versions of the module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/hosting-bundle).</span></span>

<span data-ttu-id="49e70-115">Aby zapoznać się z samouczkiem dotyczącym publikowania aplikacji ASP.NET Core na serwerze usług IIS, zobacz <xref:tutorials/publish-to-iis> .</span><span class="sxs-lookup"><span data-stu-id="49e70-115">For a tutorial experience on publishing an ASP.NET Core app to an IIS server, see <xref:tutorials/publish-to-iis>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0 < aspnetcore-5.0"

<span data-ttu-id="49e70-116">Moduł ASP.NET Core jest natywnym modułem usług IIS, który jest podłączany do potoku usług IIS:</span><span class="sxs-lookup"><span data-stu-id="49e70-116">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="49e70-117">Hostowanie aplikacji ASP.NET Core w procesie roboczym usług IIS ( `w3wp.exe` ), nazywanym [modelem hostingu w procesie](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="49e70-117">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="49e70-118">Przekazuj żądania sieci Web do zaplecza ASP.NET Core aplikacji, na której uruchomiono [serwer Kestrel](xref:fundamentals/servers/kestrel), nazywany [modelem hostingu poza procesem](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="49e70-118">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="49e70-119">Obsługiwane wersje systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="49e70-119">Supported Windows versions:</span></span>

* <span data-ttu-id="49e70-120">System Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="49e70-120">Windows 7 or later</span></span>
* <span data-ttu-id="49e70-121">System Windows Server 2012 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="49e70-121">Windows Server 2012 R2 or later</span></span>

<span data-ttu-id="49e70-122">Podczas hostingu w procesie moduł używa implementacji serwera w procesie dla usług IIS, nazywanego serwerem HTTP IIS ( `IISHttpServer` ).</span><span class="sxs-lookup"><span data-stu-id="49e70-122">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="49e70-123">Podczas hostingu poza procesem moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="49e70-123">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="49e70-124">Moduł nie działa z [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="49e70-124">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="49e70-125">Modele hostingu</span><span class="sxs-lookup"><span data-stu-id="49e70-125">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="49e70-126">Model hostingu w procesie</span><span class="sxs-lookup"><span data-stu-id="49e70-126">In-process hosting model</span></span>

<span data-ttu-id="49e70-127">Domyślnie ASP.NET Core aplikacje do modelu hostingu w procesie.</span><span class="sxs-lookup"><span data-stu-id="49e70-127">ASP.NET Core apps default to the in-process hosting model.</span></span>

<span data-ttu-id="49e70-128">Następujące cechy są stosowane podczas hostingu w procesie:</span><span class="sxs-lookup"><span data-stu-id="49e70-128">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="49e70-129">Serwer HTTP IIS ( `IISHttpServer` ) jest używany zamiast serwera [Kestrel](xref:fundamentals/servers/kestrel) .</span><span class="sxs-lookup"><span data-stu-id="49e70-129">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="49e70-130">W przypadku [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) wywołań <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> do:</span><span class="sxs-lookup"><span data-stu-id="49e70-130">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="49e70-131">Zarejestruj `IISHttpServer` .</span><span class="sxs-lookup"><span data-stu-id="49e70-131">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="49e70-132">Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49e70-132">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="49e70-133">Skonfiguruj hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="49e70-133">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="49e70-134">[Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostingu w procesie.</span><span class="sxs-lookup"><span data-stu-id="49e70-134">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="49e70-135">Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="49e70-135">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="49e70-136">Użyj jednej puli aplikacji na aplikację.</span><span class="sxs-lookup"><span data-stu-id="49e70-136">Use one app pool per app.</span></span>

* <span data-ttu-id="49e70-137">W przypadku korzystania z [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręcznego umieszczania [ `app_offline.htm` pliku we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files)aplikacja może nie zostać wyłączona natychmiast po otwarciu połączenia.</span><span class="sxs-lookup"><span data-stu-id="49e70-137">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [`app_offline.htm` file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="49e70-138">Na przykład połączenie protokołu WebSocket może opóźnić zamykanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-138">For example, a WebSocket connection may delay app shut down.</span></span>

* <span data-ttu-id="49e70-139">Architektura (bitową) aplikacji i zainstalowane środowisko uruchomieniowe (x64 lub x86) musi być zgodna z architekturą puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-139">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="49e70-140">Wykryto rozłączenia klientów.</span><span class="sxs-lookup"><span data-stu-id="49e70-140">Client disconnects are detected.</span></span> <span data-ttu-id="49e70-141">[`HttpContext.RequestAborted`](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*)Token anulowania jest anulowany, gdy klient odłączy się.</span><span class="sxs-lookup"><span data-stu-id="49e70-141">The [`HttpContext.RequestAborted`](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="49e70-142">W ASP.NET Core 2.2.1 lub wcześniejszym <xref:System.IO.Directory.GetCurrentDirectory*> zwraca katalog procesów roboczych procesu uruchomionego przez usługi IIS, a nie katalog aplikacji (na przykład `C:\Windows\System32\inetsrv` w przypadku `w3wp.exe` ).</span><span class="sxs-lookup"><span data-stu-id="49e70-142">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, `C:\Windows\System32\inetsrv` for `w3wp.exe`).</span></span>

  <span data-ttu-id="49e70-143">Przykładowy kod, który ustawia bieżący katalog aplikacji, znajduje się w [ `CurrentDirectoryHelpers` klasie](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="49e70-143">For sample code that sets the app's current directory, see the [`CurrentDirectoryHelpers` class](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/3.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="49e70-144">Wywołaj `SetCurrentDirectory` metodę.</span><span class="sxs-lookup"><span data-stu-id="49e70-144">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="49e70-145">Kolejne wywołania w celu <xref:System.IO.Directory.GetCurrentDirectory*> udostępnienia katalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-145">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="49e70-146">Podczas hostingu w procesie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wywoływana wewnętrznie w celu zainicjowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="49e70-146">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="49e70-147">W związku z tym, <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementacja użyta do przekształcenia oświadczeń po każdym uwierzytelnieniu nie jest domyślnie aktywowana.</span><span class="sxs-lookup"><span data-stu-id="49e70-147">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="49e70-148">Podczas przekształcania oświadczeń z <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementacją, wywołaj polecenie <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> Dodaj usługi uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="49e70-148">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

  ```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
      services.AddAuthentication(IISServerDefaults.AuthenticationScheme);
  }

  public void Configure(IApplicationBuilder app)
  {
      app.UseAuthentication();
  }
  ```
  
  * <span data-ttu-id="49e70-149">[Wdrożenia pakietów sieci Web (jednego pliku)](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) nie są obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="49e70-149">[Web Package (single-file) deployments](/aspnet/web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-web-packages) aren't supported.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="49e70-150">Model hostingu poza procesem</span><span class="sxs-lookup"><span data-stu-id="49e70-150">Out-of-process hosting model</span></span>

<span data-ttu-id="49e70-151">Aby skonfigurować aplikację do hostingu poza procesem, ustaw wartość `<AspNetCoreHostingModel>` właściwości na `OutOfProcess` w pliku projektu ( `.csproj` ):</span><span class="sxs-lookup"><span data-stu-id="49e70-151">To configure an app for out-of-process hosting, set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` in the project file (`.csproj`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="49e70-152">Hosting w procesie jest ustawiony przy użyciu `InProcess` wartości domyślnej.</span><span class="sxs-lookup"><span data-stu-id="49e70-152">In-process hosting is set with `InProcess`, which is the default value.</span></span>

<span data-ttu-id="49e70-153">Wartość `<AspNetCoreHostingModel>` jest uwzględniana wielkość liter, dlatego `inprocess` i `outofprocess` są prawidłowymi wartościami.</span><span class="sxs-lookup"><span data-stu-id="49e70-153">The value of `<AspNetCoreHostingModel>` is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="49e70-154">Serwer [Kestrel](xref:fundamentals/servers/kestrel) jest używany zamiast serwera http usług IIS ( `IISHttpServer` ).</span><span class="sxs-lookup"><span data-stu-id="49e70-154">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="49e70-155">W przypadku nieprzetwarzanych [`CreateDefaultBuilder`](xref:fundamentals/host/generic-host#default-builder-settings) wywołań <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> :</span><span class="sxs-lookup"><span data-stu-id="49e70-155">For out-of-process, [`CreateDefaultBuilder`](xref:fundamentals/host/generic-host#default-builder-settings) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="49e70-156">Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49e70-156">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="49e70-157">Skonfiguruj hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="49e70-157">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="49e70-158">Zmiany modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="49e70-158">Hosting model changes</span></span>

<span data-ttu-id="49e70-159">Jeśli to `hostingModel` ustawienie zostanie zmienione w `web.config` pliku (wyjaśniono w sekcji [Konfiguracja z `web.config` ](#configuration-with-webconfig) ), moduł odtwarza proces roboczy dla usług IIS.</span><span class="sxs-lookup"><span data-stu-id="49e70-159">If the `hostingModel` setting is changed in the `web.config` file (explained in the [Configuration with `web.config`](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="49e70-160">W przypadku IIS Express moduł nie odtwarza procesu roboczego, ale zamiast tego wyzwala bezpieczne zamknięcie bieżącego procesu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="49e70-160">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="49e70-161">Następne żądanie do aplikacji powoduje zduplikowanie nowego procesu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="49e70-161">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="49e70-162">Nazwa procesu</span><span class="sxs-lookup"><span data-stu-id="49e70-162">Process name</span></span>

<span data-ttu-id="49e70-163">`Process.GetCurrentProcess().ProcessName`Raporty `w3wp` / `iisexpress` (w procesie) lub `dotnet` (pozaprocesowe).</span><span class="sxs-lookup"><span data-stu-id="49e70-163">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="49e70-164">Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostaje aktywnych.</span><span class="sxs-lookup"><span data-stu-id="49e70-164">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="49e70-165">Aby dowiedzieć się więcej na temat modułów usług IIS aktywnych przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules> .</span><span class="sxs-lookup"><span data-stu-id="49e70-165">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="49e70-166">Moduł ASP.NET Core może również:</span><span class="sxs-lookup"><span data-stu-id="49e70-166">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="49e70-167">Ustaw zmienne środowiskowe dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="49e70-167">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="49e70-168">Rejestruj dane wyjściowe stdout do magazynu plików w celu rozwiązywania problemów z uruchamianiem.</span><span class="sxs-lookup"><span data-stu-id="49e70-168">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="49e70-169">Przekazuj tokeny uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="49e70-169">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="49e70-170">Jak zainstalować moduł ASP.NET Core i korzystać z niego</span><span class="sxs-lookup"><span data-stu-id="49e70-170">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="49e70-171">Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [installing the .NET Core hosting](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="49e70-171">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="49e70-172">Konfiguracja z web.config</span><span class="sxs-lookup"><span data-stu-id="49e70-172">Configuration with web.config</span></span>

<span data-ttu-id="49e70-173">Moduł ASP.NET Core jest skonfigurowany przy użyciu `aspNetCore` sekcji `system.webServer` węzła w pliku *web.config* witryny.</span><span class="sxs-lookup"><span data-stu-id="49e70-173">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="49e70-174">Następujący `web.config` plik jest publikowany dla [wdrożenia zależnego od platformy](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje moduł ASP.NET Core do obsługi żądań lokacji:</span><span class="sxs-lookup"><span data-stu-id="49e70-174">The following `web.config` file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet"
                  arguments=".\MyApp.dll"
                  stdoutLogEnabled="false"
                  stdoutLogFile=".\logs\stdout"
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="49e70-175">Następujące *web.config* są publikowane dla [wdrożenia samodzielnego](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="49e70-175">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe"
                  stdoutLogEnabled="false"
                  stdoutLogFile=".\logs\stdout"
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="49e70-176"><xref:System.Configuration.SectionInformation.InheritInChildApplications*>Właściwość jest ustawiona na `false` , aby wskazać, że ustawienia określone w [`<location>`](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) elemencie nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-176">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [`<location>`](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="49e70-177">Gdy aplikacja zostanie wdrożona do [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ścieżka jest ustawiona na `\\?\%home%\LogFiles\stdout` .</span><span class="sxs-lookup"><span data-stu-id="49e70-177">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="49e70-178">Ścieżka zapisuje dzienniki stdout do `LogFiles` folderu, który jest lokalizacją automatycznie utworzoną przez usługę.</span><span class="sxs-lookup"><span data-stu-id="49e70-178">The path saves stdout logs to the `LogFiles` folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="49e70-179">Aby uzyskać informacje na temat konfiguracji aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications> .</span><span class="sxs-lookup"><span data-stu-id="49e70-179">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="49e70-180">Atrybuty elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="49e70-180">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="49e70-181">Atrybut</span><span class="sxs-lookup"><span data-stu-id="49e70-181">Attribute</span></span> | <span data-ttu-id="49e70-182">Opis</span><span class="sxs-lookup"><span data-stu-id="49e70-182">Description</span></span> | <span data-ttu-id="49e70-183">Domyślne</span><span class="sxs-lookup"><span data-stu-id="49e70-183">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="49e70-184">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="49e70-184">Optional string attribute.</span></span></p><p><span data-ttu-id="49e70-185">Argumenty do pliku wykonywalnego określonego w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="49e70-185">Arguments to the executable specified in **processPath**.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="49e70-186">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="49e70-186">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="49e70-187">W przypadku wartości true strona **błędu 502,5 procesu** jest pomijana, a strona kodu stanu 502 skonfigurowana w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="49e70-187">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="49e70-188">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="49e70-188">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="49e70-189">Jeśli wartość jest równa true, token jest przekazywany do procesu podrzędnego, który nasłuchuje w `%ASPNETCORE_PORT%` formie nagłówka `'MS-ASPNETCORE-WINAUTHTOKEN'` na żądanie.</span><span class="sxs-lookup"><span data-stu-id="49e70-189">If true, the token is forwarded to the child process listening on `%ASPNETCORE_PORT%` as a header `'MS-ASPNETCORE-WINAUTHTOKEN'` per request.</span></span> <span data-ttu-id="49e70-190">Jest odpowiedzialny za ten proces, aby wywołać metodę CloseHandle na tym tokenie na żądanie.</span><span class="sxs-lookup"><span data-stu-id="49e70-190">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="49e70-191">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="49e70-191">Optional string attribute.</span></span></p><p><span data-ttu-id="49e70-192">Określa model hostingu jako proces ( `InProcess` / `inprocess` ) lub out-of-Process ( `OutOfProcess` / `outofprocess` ).</span><span class="sxs-lookup"><span data-stu-id="49e70-192">Specifies the hosting model as in-process (`InProcess`/`inprocess`) or out-of-process (`OutOfProcess`/`outofprocess`).</span></span></p> | `InProcess`<br>`inprocess` |
| `processesPerApplication` | <p><span data-ttu-id="49e70-193">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="49e70-193">Optional integer attribute.</span></span></p><p><span data-ttu-id="49e70-194">Określa liczbę wystąpień procesu określonego w ustawieniu **processPath** , które może być przypadające na aplikację.</span><span class="sxs-lookup"><span data-stu-id="49e70-194">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="49e70-195">&dagger;W przypadku hostingu w procesie wartość jest ograniczona do `1` .</span><span class="sxs-lookup"><span data-stu-id="49e70-195">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="49e70-196">Ustawienie `processesPerApplication` jest niezalecane.</span><span class="sxs-lookup"><span data-stu-id="49e70-196">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="49e70-197">Ten atrybut zostanie usunięty w przyszłych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="49e70-197">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="49e70-198">Wartooć `1`</span><span class="sxs-lookup"><span data-stu-id="49e70-198">Default: `1`</span></span><br><span data-ttu-id="49e70-199">Długości `1`</span><span class="sxs-lookup"><span data-stu-id="49e70-199">Min: `1`</span></span><br><span data-ttu-id="49e70-200">Maksymalny `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="49e70-200">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="49e70-201">Wymagany atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="49e70-201">Required string attribute.</span></span></p><p><span data-ttu-id="49e70-202">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="49e70-202">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="49e70-203">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="49e70-203">Relative paths are supported.</span></span> <span data-ttu-id="49e70-204">Jeśli ścieżka zaczyna się od `.` , ścieżka jest uznawana za względną względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="49e70-204">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="49e70-205">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="49e70-205">Optional integer attribute.</span></span></p><p><span data-ttu-id="49e70-206">Określa, ile razy proces określony w **processPath** może ulec awarii na minutę.</span><span class="sxs-lookup"><span data-stu-id="49e70-206">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="49e70-207">W przypadku przekroczenia tego limitu moduł przestaje uruchomić proces przez pozostałą część minuty.</span><span class="sxs-lookup"><span data-stu-id="49e70-207">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="49e70-208">Nieobsługiwane w przypadku hostingu w procesie.</span><span class="sxs-lookup"><span data-stu-id="49e70-208">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="49e70-209">Wartooć `10`</span><span class="sxs-lookup"><span data-stu-id="49e70-209">Default: `10`</span></span><br><span data-ttu-id="49e70-210">Długości `0`</span><span class="sxs-lookup"><span data-stu-id="49e70-210">Min: `0`</span></span><br><span data-ttu-id="49e70-211">Maksymalny `100`</span><span class="sxs-lookup"><span data-stu-id="49e70-211">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="49e70-212">Opcjonalny atrybut TimeSpan.</span><span class="sxs-lookup"><span data-stu-id="49e70-212">Optional timespan attribute.</span></span></p><p><span data-ttu-id="49e70-213">Określa czas, przez który moduł ASP.NET Core czeka na odpowiedź z procesu nasłuchiwania na% ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="49e70-213">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="49e70-214">W wersjach modułu ASP.NET Core, który został dostarczony z wersją ASP.NET Core 2,1 lub nowszą, wartość `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="49e70-214">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="49e70-215">Nie dotyczy hostingu w procesie.</span><span class="sxs-lookup"><span data-stu-id="49e70-215">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="49e70-216">W przypadku hostingu w procesie moduł czeka na aplikację w celu przetworzenia żądania.</span><span class="sxs-lookup"><span data-stu-id="49e70-216">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="49e70-217">Prawidłowe wartości segmentów minut i sekund ciągu mieszczą się w zakresie 0-59.</span><span class="sxs-lookup"><span data-stu-id="49e70-217">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="49e70-218">Użycie **60** w wartości minut lub sekund skutkuje *błędem wewnętrznego serwera 500*.</span><span class="sxs-lookup"><span data-stu-id="49e70-218">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="49e70-219">Wartooć `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="49e70-219">Default: `00:02:00`</span></span><br><span data-ttu-id="49e70-220">Długości `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="49e70-220">Min: `00:00:00`</span></span><br><span data-ttu-id="49e70-221">Maksymalny `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="49e70-221">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="49e70-222">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="49e70-222">Optional integer attribute.</span></span></p><p><span data-ttu-id="49e70-223">Czas w sekundach, przez który moduł czeka, aż plik wykonywalny zostanie bezpiecznie zamknięty po wykryciu pliku *app_offline.htm* .</span><span class="sxs-lookup"><span data-stu-id="49e70-223">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="49e70-224">Wartooć `10`</span><span class="sxs-lookup"><span data-stu-id="49e70-224">Default: `10`</span></span><br><span data-ttu-id="49e70-225">Długości `0`</span><span class="sxs-lookup"><span data-stu-id="49e70-225">Min: `0`</span></span><br><span data-ttu-id="49e70-226">Maksymalny `600`</span><span class="sxs-lookup"><span data-stu-id="49e70-226">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="49e70-227">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="49e70-227">Optional integer attribute.</span></span></p><p><span data-ttu-id="49e70-228">Czas w sekundach, przez który moduł czeka, aż plik wykonywalny uruchomi proces nasłuchujący na porcie.</span><span class="sxs-lookup"><span data-stu-id="49e70-228">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="49e70-229">Jeśli ten limit czasu zostanie przekroczony, moduł zakasuje proces.</span><span class="sxs-lookup"><span data-stu-id="49e70-229">If this time limit is exceeded, the module kills the process.</span></span></p><p><span data-ttu-id="49e70-230">Podczas hostingu *w procesie*: proces **nie** jest ponownie uruchamiany i **nie używa ustawienia** **rapidFailsPerMinute** .</span><span class="sxs-lookup"><span data-stu-id="49e70-230">When hosting *in-process*: The process is **not** restarted and does **not** use the **rapidFailsPerMinute** setting.</span></span></p><p><span data-ttu-id="49e70-231">Podczas hostingu *poza procesem*: moduł próbuje ponownie uruchomić proces, gdy odbierze nowe żądanie i kontynuuje próbę ponownego uruchomienia procesu na kolejnych żądaniach przychodzących, chyba że aplikacja nie uruchomi **rapidFailsPerMinute** liczby razy w ostatniej minucie.</span><span class="sxs-lookup"><span data-stu-id="49e70-231">When hosting *out-of-process*: The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="49e70-232">Wartość 0 (zero) **nie** jest uważana za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="49e70-232">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="49e70-233">Wartooć `120`</span><span class="sxs-lookup"><span data-stu-id="49e70-233">Default: `120`</span></span><br><span data-ttu-id="49e70-234">Długości `0`</span><span class="sxs-lookup"><span data-stu-id="49e70-234">Min: `0`</span></span><br><span data-ttu-id="49e70-235">Maksymalny `3600`</span><span class="sxs-lookup"><span data-stu-id="49e70-235">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="49e70-236">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="49e70-236">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="49e70-237">Jeśli wartość jest równa true, **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="49e70-237">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="49e70-238">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="49e70-238">Optional string attribute.</span></span></p><p><span data-ttu-id="49e70-239">Określa względną lub bezwzględną ścieżkę do pliku, dla którego jest rejestrowany **stdout** i **stderr** z procesu określonego w **processPath** .</span><span class="sxs-lookup"><span data-stu-id="49e70-239">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="49e70-240">Ścieżki względne są względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="49e70-240">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="49e70-241">Każda ścieżka rozpoczynająca `.` się od jest określana względem katalogu głównego witryny, a wszystkie inne ścieżki są traktowane jako ścieżki bezwzględne.</span><span class="sxs-lookup"><span data-stu-id="49e70-241">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="49e70-242">Wszystkie foldery podane w ścieżce są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="49e70-242">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="49e70-243">Przy użyciu ograniczników podkreślenia, sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku ( `.log` ) są dodawane do ostatniego segmentu ścieżki **stdoutLogFile** .</span><span class="sxs-lookup"><span data-stu-id="49e70-243">Using underscore delimiters, a timestamp, process ID, and file extension (`.log`) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="49e70-244">Jeśli `.\logs\stdout` jest podana jako wartość, przykładowy dziennik stdout jest zapisywany jako `stdout_20180205194132_1934.log` w folderze, `logs` gdy zapisywany w dniu 2/5/2018 o godzinie 19:41:32 z identyfikatorem procesu 1934.</span><span class="sxs-lookup"><span data-stu-id="49e70-244">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as `stdout_20180205194132_1934.log` in the `logs` folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="set-environment-variables"></a><span data-ttu-id="49e70-245">Ustawianie zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="49e70-245">Set environment variables</span></span>

<span data-ttu-id="49e70-246">Zmienne środowiskowe można określić dla procesu w `processPath` atrybucie.</span><span class="sxs-lookup"><span data-stu-id="49e70-246">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="49e70-247">Określ zmienną środowiskową z `<environmentVariable>` elementem podrzędnym `<environmentVariables>` elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="49e70-247">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="49e70-248">Zmienne środowiskowe ustawione w tej sekcji mają pierwszeństwo przed zmiennymi środowiskowymi systemowymi.</span><span class="sxs-lookup"><span data-stu-id="49e70-248">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="49e70-249">Poniższy przykład ustawia dwie zmienne środowiskowe w `web.config` .</span><span class="sxs-lookup"><span data-stu-id="49e70-249">The following example sets two environment variables in `web.config`.</span></span> <span data-ttu-id="49e70-250">`ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji do programu `Development` .</span><span class="sxs-lookup"><span data-stu-id="49e70-250">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="49e70-251">Deweloper może tymczasowo ustawić tę wartość w pliku, `web.config` Aby wymusić ładowanie [strony wyjątku dewelopera](xref:fundamentals/error-handling) podczas debugowania wyjątku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-251">A developer may temporarily set this value in the `web.config` file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="49e70-252">`CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, w której deweloper ma napisać kod, który odczytuje wartość przy uruchamianiu, aby utworzyć ścieżkę do ładowania pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-252">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!NOTE]
> <span data-ttu-id="49e70-253">Alternatywą dla ustawienia środowiska bezpośrednio w programie `web.config` jest uwzględnienie `<EnvironmentName>` właściwości w [profilu publikacji ( `.pubxml` )](xref:host-and-deploy/visual-studio-publish-profiles) lub w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="49e70-253">An alternative to setting the environment directly in `web.config` is to include the `<EnvironmentName>` property in the [publish profile (`.pubxml`)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="49e70-254">To podejście ustawia środowisko w `web.config` momencie opublikowania projektu:</span><span class="sxs-lookup"><span data-stu-id="49e70-254">This approach sets the environment in `web.config` when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="49e70-255">Dla `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej należy ustawić tylko `Development` serwery przejściowe i testowe, które nie są dostępne dla niezaufanych sieci, takich jak Internet.</span><span class="sxs-lookup"><span data-stu-id="49e70-255">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## `app_offline.htm`

<span data-ttu-id="49e70-256">Jeśli plik o nazwie `app_offline.htm` zostanie wykryty w katalogu głównym aplikacji, moduł ASP.NET Core próbuje bezpiecznie zamknąć aplikację i zatrzymać przetwarzanie żądań przychodzących.</span><span class="sxs-lookup"><span data-stu-id="49e70-256">If a file with the name `app_offline.htm` is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="49e70-257">Jeśli aplikacja nadal działa po upływie liczby sekund zdefiniowanej w programie `shutdownTimeLimit` , moduł ASP.NET Core kasuje uruchomiony proces.</span><span class="sxs-lookup"><span data-stu-id="49e70-257">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="49e70-258">Gdy `app_offline.htm` plik jest obecny, moduł ASP.NET Core odpowiada na żądania, wysyłając z powrotem zawartość `app_offline.htm` pliku.</span><span class="sxs-lookup"><span data-stu-id="49e70-258">While the `app_offline.htm` file is present, the ASP.NET Core Module responds to requests by sending back the contents of the `app_offline.htm` file.</span></span> <span data-ttu-id="49e70-259">Po `app_offline.htm` usunięciu pliku, następne żądanie uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="49e70-259">When the `app_offline.htm` file is removed, the next request starts the app.</span></span>

<span data-ttu-id="49e70-260">W przypadku korzystania z modelu hostingu poza procesem aplikacja może nie zostać wyłączona natychmiast, jeśli istnieje otwarte połączenie.</span><span class="sxs-lookup"><span data-stu-id="49e70-260">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="49e70-261">Na przykład połączenie protokołu WebSocket może opóźnić zamykanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-261">For example, a WebSocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="49e70-262">Strona błędu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="49e70-262">Start-up error page</span></span>

<span data-ttu-id="49e70-263">Hosting zarówno w procesie, jak i poza procesem tworzy niestandardowe strony błędów, gdy nie można uruchomić aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-263">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="49e70-264">Jeśli moduł ASP.NET Core nie może odnaleźć programu obsługi żądania w procesie lub out-of-process, zostanie wyświetlona strona kodowa stanu *niepowodzenia ładowania 500,0-procesowego/wyjściowego* .</span><span class="sxs-lookup"><span data-stu-id="49e70-264">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="49e70-265">W przypadku hostingu w procesie, Jeśli uruchomienie aplikacji przez moduł ASP.NET Core nie powiedzie się, zostanie wyświetlona strona kod stanu *niepowodzenia 500,30* .</span><span class="sxs-lookup"><span data-stu-id="49e70-265">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="49e70-266">W przypadku hostingu poza procesem, jeśli moduł ASP.NET Core nie może uruchomić procesu zaplecza lub proces zaplecza zostanie uruchomiony, ale nie nasłuchuje na skonfigurowanym porcie, zostanie wyświetlona strona kod stanu *niepowodzenia procesu 502,5* .</span><span class="sxs-lookup"><span data-stu-id="49e70-266">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="49e70-267">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stanu 5xx usługi IIS, użyj `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="49e70-267">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="49e70-268">Aby uzyskać więcej informacji o konfigurowaniu niestandardowych komunikatów o błędach, zobacz [Błędy `<httpErrors>` http ](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="49e70-268">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="49e70-269">Tworzenie i przekierowywanie dzienników</span><span class="sxs-lookup"><span data-stu-id="49e70-269">Log creation and redirection</span></span>

<span data-ttu-id="49e70-270">Moduł ASP.NET Core przekierowuje dane wyjściowe z konsoli stdout i stderr do dysku, jeśli `stdoutLogEnabled` `stdoutLogFile` atrybuty i `aspNetCore` elementu są ustawione.</span><span class="sxs-lookup"><span data-stu-id="49e70-270">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="49e70-271">Wszystkie foldery w `stdoutLogFile` ścieżce są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="49e70-271">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="49e70-272">Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\<app_pool_name>` do zapewnienia uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="49e70-272">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="49e70-273">Dzienniki nie są obracane, chyba że zostanie wykonane odtwarzanie procesów/ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="49e70-273">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="49e70-274">Ponosisz odpowiedzialność dostawcy usług hostingowych, aby ograniczyć ilość miejsca na dysku zużywanej przez dzienniki.</span><span class="sxs-lookup"><span data-stu-id="49e70-274">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="49e70-275">Użycie dziennika stdout jest zalecane tylko w przypadku rozwiązywania problemów z uruchamianiem aplikacji w usługach IIS lub w przypadku korzystania z [obsługi usług IIS w czasie projektowania w programie Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), a nie podczas debugowania lokalnego i uruchamiania aplikacji przy użyciu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="49e70-275">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="49e70-276">Nie używaj dziennika stdout w celu uzyskania ogólnych celów rejestrowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-276">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="49e70-277">Aby rejestrować procedury w aplikacji ASP.NET Core, użyj biblioteki rejestrowania, która ogranicza rozmiar pliku dziennika i obraca dzienniki.</span><span class="sxs-lookup"><span data-stu-id="49e70-277">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="49e70-278">Aby uzyskać więcej informacji, zobacz [dostawców rejestrowania innych](xref:fundamentals/logging/index#third-party-logging-providers)firm.</span><span class="sxs-lookup"><span data-stu-id="49e70-278">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="49e70-279">Sygnatura czasowa i rozszerzenie pliku są dodawane automatycznie podczas tworzenia pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="49e70-279">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="49e70-280">Nazwa pliku dziennika jest złożona przez dołączenie sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku ( `.log` ) do ostatniego segmentu `stdoutLogFile` ścieżki (zwykle `stdout` ) rozdzielanej znakami podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="49e70-280">The log file name is composed by appending the timestamp, process ID, and file extension (`.log`) to the last segment of the `stdoutLogFile` path (typically `stdout`) delimited by underscores.</span></span> <span data-ttu-id="49e70-281">Jeśli `stdoutLogFile` ścieżka kończy się na `stdout` , dziennik aplikacji o identyfikatorze PID 1934 utworzony w dniu 2/5/2018 o 19:42:32 ma nazwę pliku `stdout_20180205194132_1934.log` .</span><span class="sxs-lookup"><span data-stu-id="49e70-281">If the `stdoutLogFile` path ends with `stdout`, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name `stdout_20180205194132_1934.log`.</span></span>

<span data-ttu-id="49e70-282">Jeśli `stdoutLogEnabled` ma wartość false, błędy występujące podczas uruchamiania aplikacji są przechwytywane i emitowane do dziennika zdarzeń do 30 KB.</span><span class="sxs-lookup"><span data-stu-id="49e70-282">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="49e70-283">Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="49e70-283">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="49e70-284">Poniższy przykładowy `aspNetCore` element konfiguruje rejestrowanie stdout w ścieżce względnej `.\log\` .</span><span class="sxs-lookup"><span data-stu-id="49e70-284">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="49e70-285">Upewnij się, że tożsamość użytkownika puli aplikacji ma uprawnienia do zapisu w podanej ścieżce.</span><span class="sxs-lookup"><span data-stu-id="49e70-285">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

<span data-ttu-id="49e70-286">W przypadku publikowania aplikacji dla Azure App Service wdrożenia zestaw SDK sieci Web ustawia `stdoutLogFile` wartość na `\\?\%home%\LogFiles\stdout` .</span><span class="sxs-lookup"><span data-stu-id="49e70-286">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="49e70-287">`%home`Zmienna środowiskowa jest wstępnie zdefiniowana dla aplikacji hostowanych przez Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="49e70-287">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="49e70-288">Aby utworzyć reguły filtru rejestrowania, zobacz sekcje [Konfiguracja](xref:fundamentals/logging/index#log-filtering) i [filtrowanie dzienników](xref:fundamentals/logging/index#log-filtering) w dokumentacji rejestrowania ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49e70-288">To create logging filter rules, see the [Configuration](xref:fundamentals/logging/index#log-filtering) and [Log filtering](xref:fundamentals/logging/index#log-filtering) sections of the ASP.NET Core logging documentation.</span></span>

<span data-ttu-id="49e70-289">Aby uzyskać więcej informacji na temat formatów ścieżki, zobacz [formaty ścieżki plików w systemach Windows](/dotnet/standard/io/file-path-formats).</span><span class="sxs-lookup"><span data-stu-id="49e70-289">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="49e70-290">Udoskonalone dzienniki diagnostyczne</span><span class="sxs-lookup"><span data-stu-id="49e70-290">Enhanced diagnostic logs</span></span>

<span data-ttu-id="49e70-291">Moduł ASP.NET Core można skonfigurować w celu udostępnienia dzienników diagnostyki rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="49e70-291">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="49e70-292">Dodaj `<handlerSettings>` element do `<aspNetCore>` elementu w `web.config` .</span><span class="sxs-lookup"><span data-stu-id="49e70-292">Add the `<handlerSettings>` element to the `<aspNetCore>` element in `web.config`.</span></span> <span data-ttu-id="49e70-293">Ustawienie w `debugLevel` celu `TRACE` ujawniania większej wierności informacji diagnostycznych:</span><span class="sxs-lookup"><span data-stu-id="49e70-293">Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="49e70-294">Wszystkie foldery w ścieżce ( `logs` w poprzednim przykładzie) są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="49e70-294">Any folders in the path (`logs` in the preceding example) are created by the module when the log file is created.</span></span> <span data-ttu-id="49e70-295">Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\{APP POOL NAME}` , gdzie symbol zastępczy `{APP POOL NAME}` jest nazwą puli aplikacji, aby zapewnić uprawnienie do zapisu).</span><span class="sxs-lookup"><span data-stu-id="49e70-295">The app pool must have write access to the location where the logs are written (use `IIS AppPool\{APP POOL NAME}`, where the placeholder `{APP POOL NAME}` is the app pool name, to provide write permission).</span></span>

<span data-ttu-id="49e70-296">Wartości poziomu debugowania ( `debugLevel` ) mogą zawierać zarówno poziom, jak i lokalizację.</span><span class="sxs-lookup"><span data-stu-id="49e70-296">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="49e70-297">Poziomy (w kolejności od najmniejszej do największej szczegółowości):</span><span class="sxs-lookup"><span data-stu-id="49e70-297">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="49e70-298">BŁĄD</span><span class="sxs-lookup"><span data-stu-id="49e70-298">ERROR</span></span>
* <span data-ttu-id="49e70-299">OSTRZEŻENIE</span><span class="sxs-lookup"><span data-stu-id="49e70-299">WARNING</span></span>
* <span data-ttu-id="49e70-300">INFORMACJE</span><span class="sxs-lookup"><span data-stu-id="49e70-300">INFO</span></span>
* <span data-ttu-id="49e70-301">TRACE</span><span class="sxs-lookup"><span data-stu-id="49e70-301">TRACE</span></span>

<span data-ttu-id="49e70-302">Lokalizacje (wiele lokalizacji jest dozwolonych):</span><span class="sxs-lookup"><span data-stu-id="49e70-302">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="49e70-303">KONSOLI</span><span class="sxs-lookup"><span data-stu-id="49e70-303">CONSOLE</span></span>
* <span data-ttu-id="49e70-304">ELEMENCIE</span><span class="sxs-lookup"><span data-stu-id="49e70-304">EVENTLOG</span></span>
* <span data-ttu-id="49e70-305">PLIK</span><span class="sxs-lookup"><span data-stu-id="49e70-305">FILE</span></span>

<span data-ttu-id="49e70-306">Ustawienia programu obsługi można także zapewnić za pomocą zmiennych środowiskowych:</span><span class="sxs-lookup"><span data-stu-id="49e70-306">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="49e70-307">`ASPNETCORE_MODULE_DEBUG_FILE`: Ścieżka do pliku dziennika debugowania.</span><span class="sxs-lookup"><span data-stu-id="49e70-307">`ASPNETCORE_MODULE_DEBUG_FILE`: Path to the debug log file.</span></span> <span data-ttu-id="49e70-308">(Domyślnie: `aspnetcore-debug.log` )</span><span class="sxs-lookup"><span data-stu-id="49e70-308">(Default: `aspnetcore-debug.log`)</span></span>
* <span data-ttu-id="49e70-309">`ASPNETCORE_MODULE_DEBUG`: Debugowanie ustawienia poziomu.</span><span class="sxs-lookup"><span data-stu-id="49e70-309">`ASPNETCORE_MODULE_DEBUG`: Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="49e70-310">**Nie** pozostawiaj włączonej rejestracji debugowania w ramach wdrożenia dłużej niż jest to wymagane, aby rozwiązać problem.</span><span class="sxs-lookup"><span data-stu-id="49e70-310">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="49e70-311">Rozmiar dziennika nie jest ograniczony.</span><span class="sxs-lookup"><span data-stu-id="49e70-311">The size of the log isn't limited.</span></span> <span data-ttu-id="49e70-312">Pozostawienie włączonego dziennika debugowania może spowodować wyczerpanie dostępnego miejsca na dysku i awarię serwera lub usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="49e70-312">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="49e70-313">Zapoznaj się z tematem [Konfiguracja z web.config](#configuration-with-webconfig) , aby zapoznać się z przykładem `aspNetCore` elementu w `web.config` pliku.</span><span class="sxs-lookup"><span data-stu-id="49e70-313">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the `web.config` file.</span></span>

## <a name="modify-the-stack-size"></a><span data-ttu-id="49e70-314">Modyfikowanie rozmiaru stosu</span><span class="sxs-lookup"><span data-stu-id="49e70-314">Modify the stack size</span></span>

<span data-ttu-id="49e70-315">*Stosuje się tylko w przypadku korzystania z modelu hostingu w procesie.*</span><span class="sxs-lookup"><span data-stu-id="49e70-315">*Only applies when using the in-process hosting model.*</span></span>

<span data-ttu-id="49e70-316">Skonfiguruj zarządzany rozmiar stosu przy użyciu `stackSize` Ustawienia w bajtach w `web.config` .</span><span class="sxs-lookup"><span data-stu-id="49e70-316">Configure the managed stack size using the `stackSize` setting in bytes in `web.config`.</span></span> <span data-ttu-id="49e70-317">Domyślny rozmiar to 1 048 576 bajtów (1 MB).</span><span class="sxs-lookup"><span data-stu-id="49e70-317">The default size is 1,048,576 bytes (1 MB).</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="stackSize" value="2097152" />
  </handlerSettings>
</aspNetCore>
```

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="49e70-318">Konfiguracja serwera proxy używa protokołu HTTP i tokenu parowania</span><span class="sxs-lookup"><span data-stu-id="49e70-318">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="49e70-319">*Dotyczy tylko hostingu poza procesem.*</span><span class="sxs-lookup"><span data-stu-id="49e70-319">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="49e70-320">Serwer proxy utworzony między modułem ASP.NET Core a Kestrel używa protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="49e70-320">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="49e70-321">Nie ma ryzyka podsłuchiwanie ruchu między modułem i Kestrel z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="49e70-321">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="49e70-322">Token parowania jest używany w celu zagwarantowania, że żądania odbierane przez Kestrel zostały przekazane przez usługi IIS i nie pochodzą z innego źródła.</span><span class="sxs-lookup"><span data-stu-id="49e70-322">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="49e70-323">Token parowania jest tworzony i ustawiany jako zmienna środowiskowa ( `ASPNETCORE_TOKEN` ) przez moduł.</span><span class="sxs-lookup"><span data-stu-id="49e70-323">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="49e70-324">Token parowania jest również ustawiany w nagłówku ( `MS-ASPNETCORE-TOKEN` ) na każdym żądaniu z serwerem proxy.</span><span class="sxs-lookup"><span data-stu-id="49e70-324">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="49e70-325">Oprogramowanie pośredniczące usług IIS sprawdza każde odebrane żądanie, aby potwierdzić, że wartość nagłówka tokenu parowania jest zgodna z wartością zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="49e70-325">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="49e70-326">Jeśli wartości tokenu są niezgodne, żądanie zostanie zarejestrowane i odrzucone.</span><span class="sxs-lookup"><span data-stu-id="49e70-326">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="49e70-327">Zmienna środowiskowa tokena parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="49e70-327">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="49e70-328">Bez znajomości wartości tokenu parowania osoba atakująca nie może przesłać żądań, które pomijają Ewidencjonowanie oprogramowania pośredniczącego usług IIS.</span><span class="sxs-lookup"><span data-stu-id="49e70-328">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="49e70-329">Moduł ASP.NET Core z konfiguracją udostępnioną usług IIS</span><span class="sxs-lookup"><span data-stu-id="49e70-329">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="49e70-330">Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami konta **TrustedInstaller** .</span><span class="sxs-lookup"><span data-stu-id="49e70-330">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="49e70-331">Ponieważ konto systemu lokalnego nie ma uprawnień do modyfikowania dla ścieżki udziału używanej przez udostępnioną konfigurację usług IIS, Instalator zgłasza błąd odmowy dostępu podczas próby skonfigurowania ustawień modułu w `applicationHost.config` pliku w udziale.</span><span class="sxs-lookup"><span data-stu-id="49e70-331">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the `applicationHost.config` file on the share.</span></span>

<span data-ttu-id="49e70-332">W przypadku korzystania z konfiguracji udostępnionej przez usługi IIS na tym samym komputerze, na którym znajduje się instalacja usług IIS, uruchom Instalatora pakietu ASP.NET Core hostowania z `OPT_NO_SHARED_CONFIG_CHECK` parametrem ustawionym na `1` :</span><span class="sxs-lookup"><span data-stu-id="49e70-332">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="49e70-333">Jeśli ścieżka do konfiguracji udostępnionej nie znajduje się na tym samym komputerze, na którym znajduje się instalacja usług IIS, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="49e70-333">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="49e70-334">Wyłącz konfigurację udostępnioną usług IIS.</span><span class="sxs-lookup"><span data-stu-id="49e70-334">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="49e70-335">Uruchom instalatora.</span><span class="sxs-lookup"><span data-stu-id="49e70-335">Run the installer.</span></span>
1. <span data-ttu-id="49e70-336">Wyeksportuj zaktualizowany `applicationHost.config` plik do udziału.</span><span class="sxs-lookup"><span data-stu-id="49e70-336">Export the updated `applicationHost.config` file to the share.</span></span>
1. <span data-ttu-id="49e70-337">Włącz ponownie konfigurację udostępnioną usług IIS.</span><span class="sxs-lookup"><span data-stu-id="49e70-337">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="49e70-338">Dzienniki instalacji pakietu i pakietów hostingu</span><span class="sxs-lookup"><span data-stu-id="49e70-338">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="49e70-339">Aby określić wersję zainstalowanego modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="49e70-339">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="49e70-340">W systemie hostingu przejdź do `%windir%\System32\inetsrv` .</span><span class="sxs-lookup"><span data-stu-id="49e70-340">On the hosting system, navigate to `%windir%\System32\inetsrv`.</span></span>
1. <span data-ttu-id="49e70-341">Znajdź `aspnetcore.dll` plik.</span><span class="sxs-lookup"><span data-stu-id="49e70-341">Locate the `aspnetcore.dll` file.</span></span>
1. <span data-ttu-id="49e70-342">Kliknij prawym przyciskiem myszy plik i wybierz polecenie **Właściwości** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="49e70-342">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="49e70-343">Wybierz kartę **szczegóły** . **Wersja pliku** i **Wersja produktu** reprezentują zainstalowaną wersję modułu.</span><span class="sxs-lookup"><span data-stu-id="49e70-343">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="49e70-344">Dzienniki Instalatora pakietu hostingu dla modułu znajdują się pod adresem `C:\Users\%UserName%\AppData\Local\Temp` .</span><span class="sxs-lookup"><span data-stu-id="49e70-344">The Hosting Bundle installer logs for the module are found at `C:\Users\%UserName%\AppData\Local\Temp`.</span></span> <span data-ttu-id="49e70-345">Plik ma nazwę `dd_DotNetCoreWinSvrHosting__{TIMESTAMP}_000_AspNetCoreModule_x64.log` .</span><span class="sxs-lookup"><span data-stu-id="49e70-345">The file is named `dd_DotNetCoreWinSvrHosting__{TIMESTAMP}_000_AspNetCoreModule_x64.log`.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="49e70-346">Lokalizacje pliku modułu, schematu i konfiguracji</span><span class="sxs-lookup"><span data-stu-id="49e70-346">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="49e70-347">Moduł</span><span class="sxs-lookup"><span data-stu-id="49e70-347">Module</span></span>

<span data-ttu-id="49e70-348">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="49e70-348">**IIS (x86/amd64):**</span></span>

* `%windir%\System32\inetsrv\aspnetcore.dll`

* `%windir%\SysWOW64\inetsrv\aspnetcore.dll`

* `%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll`

* `%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll`

<span data-ttu-id="49e70-349">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="49e70-349">**IIS Express (x86/amd64):**</span></span>

* `%ProgramFiles%\IIS Express\aspnetcore.dll`

* `%ProgramFiles(x86)%\IIS Express\aspnetcore.dll`

* `%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll`

* `%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll`

### <a name="schema"></a><span data-ttu-id="49e70-350">Schemat</span><span class="sxs-lookup"><span data-stu-id="49e70-350">Schema</span></span>

<span data-ttu-id="49e70-351">**IIS**</span><span class="sxs-lookup"><span data-stu-id="49e70-351">**IIS**</span></span>

* `%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml`

* `%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml`

<span data-ttu-id="49e70-352">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="49e70-352">**IIS Express**</span></span>

* `%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml`

* `%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml`

### <a name="configuration"></a><span data-ttu-id="49e70-353">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="49e70-353">Configuration</span></span>

<span data-ttu-id="49e70-354">**IIS**</span><span class="sxs-lookup"><span data-stu-id="49e70-354">**IIS**</span></span>

* `%windir%\System32\inetsrv\config\applicationHost.config`

<span data-ttu-id="49e70-355">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="49e70-355">**IIS Express**</span></span>

* <span data-ttu-id="49e70-356">Program Visual Studio: `{APPLICATION ROOT}\.vs\config\applicationHost.config`</span><span class="sxs-lookup"><span data-stu-id="49e70-356">Visual Studio: `{APPLICATION ROOT}\.vs\config\applicationHost.config`</span></span>

* <span data-ttu-id="49e70-357">*iisexpress.exe* Interfejs `%USERPROFILE%\Documents\IISExpress\config\applicationhost.config`</span><span class="sxs-lookup"><span data-stu-id="49e70-357">*iisexpress.exe* CLI: `%USERPROFILE%\Documents\IISExpress\config\applicationhost.config`</span></span>

<span data-ttu-id="49e70-358">Pliki można znaleźć, wyszukując je `aspnetcore` w `applicationHost.config` pliku.</span><span class="sxs-lookup"><span data-stu-id="49e70-358">The files can be found by searching for `aspnetcore` in the `applicationHost.config` file.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="49e70-359">Moduł ASP.NET Core jest natywnym modułem usług IIS, który jest podłączany do potoku usług IIS:</span><span class="sxs-lookup"><span data-stu-id="49e70-359">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="49e70-360">Hostowanie aplikacji ASP.NET Core w procesie roboczym usług IIS ( `w3wp.exe` ), nazywanym [modelem hostingu w procesie](#in-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="49e70-360">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>
* <span data-ttu-id="49e70-361">Przekazuj żądania sieci Web do zaplecza ASP.NET Core aplikacji, na której uruchomiono [serwer Kestrel](xref:fundamentals/servers/kestrel), nazywany [modelem hostingu poza procesem](#out-of-process-hosting-model).</span><span class="sxs-lookup"><span data-stu-id="49e70-361">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

<span data-ttu-id="49e70-362">Obsługiwane wersje systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="49e70-362">Supported Windows versions:</span></span>

* <span data-ttu-id="49e70-363">System Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="49e70-363">Windows 7 or later</span></span>
* <span data-ttu-id="49e70-364">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="49e70-364">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="49e70-365">Podczas hostingu w procesie moduł używa implementacji serwera w procesie dla usług IIS, nazywanego serwerem HTTP IIS ( `IISHttpServer` ).</span><span class="sxs-lookup"><span data-stu-id="49e70-365">When hosting in-process, the module uses an in-process server implementation for IIS, called IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="49e70-366">Podczas hostingu poza procesem moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="49e70-366">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="49e70-367">Moduł nie działa z [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="49e70-367">The module doesn't function with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

## <a name="hosting-models"></a><span data-ttu-id="49e70-368">Modele hostingu</span><span class="sxs-lookup"><span data-stu-id="49e70-368">Hosting models</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="49e70-369">Model hostingu w procesie</span><span class="sxs-lookup"><span data-stu-id="49e70-369">In-process hosting model</span></span>

<span data-ttu-id="49e70-370">Aby skonfigurować aplikację do hostingu w procesie, należy dodać `<AspNetCoreHostingModel>` Właściwość do pliku projektu aplikacji z wartością `InProcess` (hosting poza procesem jest ustawiony z `OutOfProcess` ):</span><span class="sxs-lookup"><span data-stu-id="49e70-370">To configure an app for in-process hosting, add the `<AspNetCoreHostingModel>` property to the app's project file with a value of `InProcess` (out-of-process hosting is set with `OutOfProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="49e70-371">Model hostingu w procesie nie jest obsługiwany w przypadku aplikacji ASP.NET Core przeznaczonych dla .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="49e70-371">The in-process hosting model isn't supported for ASP.NET Core apps that target the .NET Framework.</span></span>

<span data-ttu-id="49e70-372">Wartość `<AspNetCoreHostingModel>` jest uwzględniana wielkość liter, dlatego `inprocess` i `outofprocess` są prawidłowymi wartościami.</span><span class="sxs-lookup"><span data-stu-id="49e70-372">The value of `<AspNetCoreHostingModel>` is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="49e70-373">Jeśli `<AspNetCoreHostingModel>` Właściwość nie jest obecna w pliku, wartość domyślna to `OutOfProcess` .</span><span class="sxs-lookup"><span data-stu-id="49e70-373">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>

<span data-ttu-id="49e70-374">Następujące cechy są stosowane podczas hostingu w procesie:</span><span class="sxs-lookup"><span data-stu-id="49e70-374">The following characteristics apply when hosting in-process:</span></span>

* <span data-ttu-id="49e70-375">Serwer HTTP IIS ( `IISHttpServer` ) jest używany zamiast serwera [Kestrel](xref:fundamentals/servers/kestrel) .</span><span class="sxs-lookup"><span data-stu-id="49e70-375">IIS HTTP Server (`IISHttpServer`) is used instead of [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span> <span data-ttu-id="49e70-376">W przypadku [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) wywołań <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> do:</span><span class="sxs-lookup"><span data-stu-id="49e70-376">For in-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIIS*> to:</span></span>

  * <span data-ttu-id="49e70-377">Zarejestruj `IISHttpServer` .</span><span class="sxs-lookup"><span data-stu-id="49e70-377">Register the `IISHttpServer`.</span></span>
  * <span data-ttu-id="49e70-378">Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49e70-378">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
  * <span data-ttu-id="49e70-379">Skonfiguruj hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="49e70-379">Configure the host to capture startup errors.</span></span>

* <span data-ttu-id="49e70-380">[Atrybut requestTimeout](#attributes-of-the-aspnetcore-element) nie ma zastosowania do hostingu w procesie.</span><span class="sxs-lookup"><span data-stu-id="49e70-380">The [requestTimeout attribute](#attributes-of-the-aspnetcore-element) doesn't apply to in-process hosting.</span></span>

* <span data-ttu-id="49e70-381">Udostępnianie puli aplikacji między aplikacjami nie jest obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="49e70-381">Sharing an app pool among apps isn't supported.</span></span> <span data-ttu-id="49e70-382">Użyj jednej puli aplikacji na aplikację.</span><span class="sxs-lookup"><span data-stu-id="49e70-382">Use one app pool per app.</span></span>

* <span data-ttu-id="49e70-383">W przypadku korzystania z [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) lub ręcznego umieszczania [ plikuapp_offline.htm we wdrożeniu](xref:host-and-deploy/iis/index#locked-deployment-files)aplikacja może nie zostać ZAMKNIĘTA natychmiast po otwarciu połączenia.</span><span class="sxs-lookup"><span data-stu-id="49e70-383">When using [Web Deploy](/iis/publish/using-web-deploy/introduction-to-web-deploy) or manually placing an [app_offline.htm file in the deployment](xref:host-and-deploy/iis/index#locked-deployment-files), the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="49e70-384">Na przykład połączenie protokołu WebSocket może opóźnić zamykanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-384">For example, a websocket connection may delay app shut down.</span></span>

* <span data-ttu-id="49e70-385">Architektura (bitową) aplikacji i zainstalowane środowisko uruchomieniowe (x64 lub x86) musi być zgodna z architekturą puli aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-385">The architecture (bitness) of the app and installed runtime (x64 or x86) must match the architecture of the app pool.</span></span>

* <span data-ttu-id="49e70-386">Wykryto rozłączenia klientów.</span><span class="sxs-lookup"><span data-stu-id="49e70-386">Client disconnects are detected.</span></span> <span data-ttu-id="49e70-387">Token anulowania [HttpContext. RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) został anulowany po rozłączeniu klienta.</span><span class="sxs-lookup"><span data-stu-id="49e70-387">The [HttpContext.RequestAborted](xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted*) cancellation token is cancelled when the client disconnects.</span></span>

* <span data-ttu-id="49e70-388">W ASP.NET Core 2.2.1 lub wcześniejszym <xref:System.IO.Directory.GetCurrentDirectory*> zwraca katalog procesów roboczych procesu uruchomionego przez usługi IIS, a nie katalog aplikacji (na przykład *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span><span class="sxs-lookup"><span data-stu-id="49e70-388">In ASP.NET Core 2.2.1 or earlier, <xref:System.IO.Directory.GetCurrentDirectory*> returns the worker directory of the process started by IIS rather than the app's directory (for example, *C:\Windows\System32\inetsrv* for *w3wp.exe*).</span></span>

  <span data-ttu-id="49e70-389">Przykładowy kod, który ustawia bieżący katalog aplikacji, znajduje się w [klasie CurrentDirectoryHelpers](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span><span class="sxs-lookup"><span data-stu-id="49e70-389">For sample code that sets the app's current directory, see the [CurrentDirectoryHelpers class](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/host-and-deploy/aspnet-core-module/samples_snapshot/2.x/CurrentDirectoryHelpers.cs).</span></span> <span data-ttu-id="49e70-390">Wywołaj `SetCurrentDirectory` metodę.</span><span class="sxs-lookup"><span data-stu-id="49e70-390">Call the `SetCurrentDirectory` method.</span></span> <span data-ttu-id="49e70-391">Kolejne wywołania w celu <xref:System.IO.Directory.GetCurrentDirectory*> udostępnienia katalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-391">Subsequent calls to <xref:System.IO.Directory.GetCurrentDirectory*> provide the app's directory.</span></span>

* <span data-ttu-id="49e70-392">Podczas hostingu w procesie <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> nie jest wywoływana wewnętrznie w celu zainicjowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="49e70-392">When hosting in-process, <xref:Microsoft.AspNetCore.Authentication.AuthenticationService.AuthenticateAsync*> isn't called internally to initialize a user.</span></span> <span data-ttu-id="49e70-393">W związku z tym, <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementacja użyta do przekształcenia oświadczeń po każdym uwierzytelnieniu nie jest domyślnie aktywowana.</span><span class="sxs-lookup"><span data-stu-id="49e70-393">Therefore, an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation used to transform claims after every authentication isn't activated by default.</span></span> <span data-ttu-id="49e70-394">Podczas przekształcania oświadczeń z <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementacją, wywołaj polecenie <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> Dodaj usługi uwierzytelniania:</span><span class="sxs-lookup"><span data-stu-id="49e70-394">When transforming claims with an <xref:Microsoft.AspNetCore.Authentication.IClaimsTransformation> implementation, call <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> to add authentication services:</span></span>

  ```csharp
  public void ConfigureServices(IServiceCollection services)
  {
      services.AddTransient<IClaimsTransformation, ClaimsTransformer>();
      services.AddAuthentication(IISServerDefaults.AuthenticationScheme);
  }

  public void Configure(IApplicationBuilder app)
  {
      app.UseAuthentication();
  }
  ```

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="49e70-395">Model hostingu poza procesem</span><span class="sxs-lookup"><span data-stu-id="49e70-395">Out-of-process hosting model</span></span>

<span data-ttu-id="49e70-396">Aby skonfigurować aplikację do hostingu poza procesem, użyj jednego z następujących metod w pliku projektu:</span><span class="sxs-lookup"><span data-stu-id="49e70-396">To configure an app for out-of-process hosting, use either of the following approaches in the project file:</span></span>

* <span data-ttu-id="49e70-397">Nie określaj `<AspNetCoreHostingModel>` właściwości.</span><span class="sxs-lookup"><span data-stu-id="49e70-397">Don't specify the `<AspNetCoreHostingModel>` property.</span></span> <span data-ttu-id="49e70-398">Jeśli `<AspNetCoreHostingModel>` Właściwość nie jest obecna w pliku, wartość domyślna to `OutOfProcess` .</span><span class="sxs-lookup"><span data-stu-id="49e70-398">If the `<AspNetCoreHostingModel>` property isn't present in the file, the default value is `OutOfProcess`.</span></span>
* <span data-ttu-id="49e70-399">Ustaw wartość `<AspNetCoreHostingModel>` właściwości na `OutOfProcess` (hosting w procesie jest ustawiany z `InProcess` ):</span><span class="sxs-lookup"><span data-stu-id="49e70-399">Set the value of the `<AspNetCoreHostingModel>` property to `OutOfProcess` (in-process hosting is set with `InProcess`):</span></span>

```xml
<PropertyGroup>
  <AspNetCoreHostingModel>OutOfProcess</AspNetCoreHostingModel>
</PropertyGroup>
```

<span data-ttu-id="49e70-400">W tej wartości wielkość liter nie jest rozróżniana `inprocess` i `outofprocess` są prawidłowe wartości.</span><span class="sxs-lookup"><span data-stu-id="49e70-400">The value is case insensitive, so `inprocess` and `outofprocess` are valid values.</span></span>

<span data-ttu-id="49e70-401">Serwer [Kestrel](xref:fundamentals/servers/kestrel) jest używany zamiast serwera http usług IIS ( `IISHttpServer` ).</span><span class="sxs-lookup"><span data-stu-id="49e70-401">[Kestrel](xref:fundamentals/servers/kestrel) server is used instead of IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="49e70-402">W przypadku [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) połączeń z <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> :</span><span class="sxs-lookup"><span data-stu-id="49e70-402">For out-of-process, [CreateDefaultBuilder](xref:fundamentals/host/web-host#set-up-a-host) calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderIISExtensions.UseIISIntegration*> to:</span></span>

* <span data-ttu-id="49e70-403">Skonfiguruj port i ścieżkę bazową, na której serwer powinien nasłuchiwać przy uruchomionym za modułem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49e70-403">Configure the port and base path the server should listen on when running behind the ASP.NET Core Module.</span></span>
* <span data-ttu-id="49e70-404">Skonfiguruj hosta do przechwytywania błędów uruchamiania.</span><span class="sxs-lookup"><span data-stu-id="49e70-404">Configure the host to capture startup errors.</span></span>

### <a name="hosting-model-changes"></a><span data-ttu-id="49e70-405">Zmiany modelu hostingu</span><span class="sxs-lookup"><span data-stu-id="49e70-405">Hosting model changes</span></span>

<span data-ttu-id="49e70-406">Jeśli to `hostingModel` ustawienie zostanie zmienione w pliku *web.config* (wyjaśniono w sekcji [Konfiguracja z web.config](#configuration-with-webconfig) ), moduł odtwarza proces roboczy dla usług IIS.</span><span class="sxs-lookup"><span data-stu-id="49e70-406">If the `hostingModel` setting is changed in the *web.config* file (explained in the [Configuration with web.config](#configuration-with-webconfig) section), the module recycles the worker process for IIS.</span></span>

<span data-ttu-id="49e70-407">W przypadku IIS Express moduł nie odtwarza procesu roboczego, ale zamiast tego wyzwala bezpieczne zamknięcie bieżącego procesu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="49e70-407">For IIS Express, the module doesn't recycle the worker process but instead triggers a graceful shutdown of the current IIS Express process.</span></span> <span data-ttu-id="49e70-408">Następne żądanie do aplikacji powoduje zduplikowanie nowego procesu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="49e70-408">The next request to the app spawns a new IIS Express process.</span></span>

### <a name="process-name"></a><span data-ttu-id="49e70-409">Nazwa procesu</span><span class="sxs-lookup"><span data-stu-id="49e70-409">Process name</span></span>

<span data-ttu-id="49e70-410">`Process.GetCurrentProcess().ProcessName`Raporty `w3wp` / `iisexpress` (w procesie) lub `dotnet` (pozaprocesowe).</span><span class="sxs-lookup"><span data-stu-id="49e70-410">`Process.GetCurrentProcess().ProcessName` reports `w3wp`/`iisexpress` (in-process) or `dotnet` (out-of-process).</span></span>

<span data-ttu-id="49e70-411">Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostaje aktywnych.</span><span class="sxs-lookup"><span data-stu-id="49e70-411">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="49e70-412">Aby dowiedzieć się więcej na temat modułów usług IIS aktywnych przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules> .</span><span class="sxs-lookup"><span data-stu-id="49e70-412">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="49e70-413">Moduł ASP.NET Core może również:</span><span class="sxs-lookup"><span data-stu-id="49e70-413">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="49e70-414">Ustaw zmienne środowiskowe dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="49e70-414">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="49e70-415">Rejestruj dane wyjściowe stdout do magazynu plików w celu rozwiązywania problemów z uruchamianiem.</span><span class="sxs-lookup"><span data-stu-id="49e70-415">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="49e70-416">Przekazuj tokeny uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="49e70-416">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="49e70-417">Jak zainstalować moduł ASP.NET Core i korzystać z niego</span><span class="sxs-lookup"><span data-stu-id="49e70-417">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="49e70-418">Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [installing the .NET Core hosting](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="49e70-418">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="49e70-419">Konfiguracja z web.config</span><span class="sxs-lookup"><span data-stu-id="49e70-419">Configuration with web.config</span></span>

<span data-ttu-id="49e70-420">Moduł ASP.NET Core jest skonfigurowany przy użyciu `aspNetCore` sekcji `system.webServer` węzła w pliku *web.config* witryny.</span><span class="sxs-lookup"><span data-stu-id="49e70-420">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="49e70-421">Następujący *web.config* plik jest publikowany dla [wdrożenia zależnego od platformy](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje moduł ASP.NET Core do obsługi żądań lokacji:</span><span class="sxs-lookup"><span data-stu-id="49e70-421">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath="dotnet"
                  arguments=".\MyApp.dll"
                  stdoutLogEnabled="false"
                  stdoutLogFile=".\logs\stdout"
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="49e70-422">Następujące *web.config* są publikowane dla [wdrożenia samodzielnego](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="49e70-422">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <location path="." inheritInChildApplications="false">
    <system.webServer>
      <handlers>
        <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModuleV2" resourceType="Unspecified" />
      </handlers>
      <aspNetCore processPath=".\MyApp.exe"
                  stdoutLogEnabled="false"
                  stdoutLogFile=".\logs\stdout"
                  hostingModel="inprocess" />
    </system.webServer>
  </location>
</configuration>
```

<span data-ttu-id="49e70-423"><xref:System.Configuration.SectionInformation.InheritInChildApplications*>Właściwość jest ustawiona na `false` , aby wskazać, że ustawienia określone w [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) elemencie nie są dziedziczone przez aplikacje, które znajdują się w podkatalogu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-423">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the app.</span></span>

<span data-ttu-id="49e70-424">Gdy aplikacja zostanie wdrożona do [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ścieżka jest ustawiona na `\\?\%home%\LogFiles\stdout` .</span><span class="sxs-lookup"><span data-stu-id="49e70-424">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="49e70-425">Ścieżka zapisuje dzienniki stdout do folderu *LogFiles* , który jest lokalizacją automatycznie utworzoną przez usługę.</span><span class="sxs-lookup"><span data-stu-id="49e70-425">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="49e70-426">Aby uzyskać informacje na temat konfiguracji aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications> .</span><span class="sxs-lookup"><span data-stu-id="49e70-426">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="49e70-427">Atrybuty elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="49e70-427">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="49e70-428">Atrybut</span><span class="sxs-lookup"><span data-stu-id="49e70-428">Attribute</span></span> | <span data-ttu-id="49e70-429">Opis</span><span class="sxs-lookup"><span data-stu-id="49e70-429">Description</span></span> | <span data-ttu-id="49e70-430">Domyślne</span><span class="sxs-lookup"><span data-stu-id="49e70-430">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="49e70-431">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="49e70-431">Optional string attribute.</span></span></p><p><span data-ttu-id="49e70-432">Argumenty do pliku wykonywalnego określonego w `processPath` .</span><span class="sxs-lookup"><span data-stu-id="49e70-432">Arguments to the executable specified in `processPath`.</span></span></p> | |
| `disableStartUpErrorPage` | <p><span data-ttu-id="49e70-433">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="49e70-433">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="49e70-434">W przypadku wartości true strona **błędu 502,5 procesu** jest pomijana, a strona kodu stanu 502 skonfigurowana w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="49e70-434">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="49e70-435">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="49e70-435">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="49e70-436">Jeśli wartość jest równa true, token jest przekazywany do procesu podrzędnego, który nasłuchuje na% ASPNETCORE_PORT% jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="49e70-436">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="49e70-437">Jest odpowiedzialny za ten proces, aby wywołać metodę CloseHandle na tym tokenie na żądanie.</span><span class="sxs-lookup"><span data-stu-id="49e70-437">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `hostingModel` | <p><span data-ttu-id="49e70-438">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="49e70-438">Optional string attribute.</span></span></p><p><span data-ttu-id="49e70-439">Określa model hostingu jako proces ( `InProcess` / `inprocess` ) lub out-of-Process ( `OutOfProcess` / `outofprocess` ).</span><span class="sxs-lookup"><span data-stu-id="49e70-439">Specifies the hosting model as in-process (`InProcess`/`inprocess`) or out-of-process (`OutOfProcess`/`outofprocess`).</span></span></p> | `OutOfProcess`<br>`outofprocess` |
| `processesPerApplication` | <p><span data-ttu-id="49e70-440">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="49e70-440">Optional integer attribute.</span></span></p><p><span data-ttu-id="49e70-441">Określa liczbę wystąpień procesu określonego w `processPath` ustawieniu, które może być przypadające na aplikację.</span><span class="sxs-lookup"><span data-stu-id="49e70-441">Specifies the number of instances of the process specified in the `processPath` setting that can be spun up per app.</span></span></p><p><span data-ttu-id="49e70-442">&dagger;W przypadku hostingu w procesie wartość jest ograniczona do `1` .</span><span class="sxs-lookup"><span data-stu-id="49e70-442">&dagger;For in-process hosting, the value is limited to `1`.</span></span></p><p><span data-ttu-id="49e70-443">Ustawienie `processesPerApplication` jest niezalecane.</span><span class="sxs-lookup"><span data-stu-id="49e70-443">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="49e70-444">Ten atrybut zostanie usunięty w przyszłych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="49e70-444">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="49e70-445">Wartooć `1`</span><span class="sxs-lookup"><span data-stu-id="49e70-445">Default: `1`</span></span><br><span data-ttu-id="49e70-446">Długości `1`</span><span class="sxs-lookup"><span data-stu-id="49e70-446">Min: `1`</span></span><br><span data-ttu-id="49e70-447">Maksymalny `100`&dagger;</span><span class="sxs-lookup"><span data-stu-id="49e70-447">Max: `100`&dagger;</span></span> |
| `processPath` | <p><span data-ttu-id="49e70-448">Wymagany atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="49e70-448">Required string attribute.</span></span></p><p><span data-ttu-id="49e70-449">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="49e70-449">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="49e70-450">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="49e70-450">Relative paths are supported.</span></span> <span data-ttu-id="49e70-451">Jeśli ścieżka zaczyna się od `.` , ścieżka jest uznawana za względną względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="49e70-451">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="49e70-452">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="49e70-452">Optional integer attribute.</span></span></p><p><span data-ttu-id="49e70-453">Określa, ile razy proces określony w programie `processPath` może ulec awarii na minutę.</span><span class="sxs-lookup"><span data-stu-id="49e70-453">Specifies the number of times the process specified in `processPath` is allowed to crash per minute.</span></span> <span data-ttu-id="49e70-454">W przypadku przekroczenia tego limitu moduł przestaje uruchomić proces przez pozostałą część minuty.</span><span class="sxs-lookup"><span data-stu-id="49e70-454">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="49e70-455">Nieobsługiwane w przypadku hostingu w procesie.</span><span class="sxs-lookup"><span data-stu-id="49e70-455">Not supported with in-process hosting.</span></span></p> | <span data-ttu-id="49e70-456">Wartooć `10`</span><span class="sxs-lookup"><span data-stu-id="49e70-456">Default: `10`</span></span><br><span data-ttu-id="49e70-457">Długości `0`</span><span class="sxs-lookup"><span data-stu-id="49e70-457">Min: `0`</span></span><br><span data-ttu-id="49e70-458">Maksymalny `100`</span><span class="sxs-lookup"><span data-stu-id="49e70-458">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="49e70-459">Opcjonalny atrybut TimeSpan.</span><span class="sxs-lookup"><span data-stu-id="49e70-459">Optional timespan attribute.</span></span></p><p><span data-ttu-id="49e70-460">Określa czas, przez który moduł ASP.NET Core czeka na odpowiedź z procesu nasłuchiwania na% ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="49e70-460">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="49e70-461">W wersjach modułu ASP.NET Core, który został dostarczony z wersją ASP.NET Core 2,1 lub nowszą, wartość `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="49e70-461">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p><p><span data-ttu-id="49e70-462">Nie dotyczy hostingu w procesie.</span><span class="sxs-lookup"><span data-stu-id="49e70-462">Doesn't apply to in-process hosting.</span></span> <span data-ttu-id="49e70-463">W przypadku hostingu w procesie moduł czeka na aplikację w celu przetworzenia żądania.</span><span class="sxs-lookup"><span data-stu-id="49e70-463">For in-process hosting, the module waits for the app to process the request.</span></span></p><p><span data-ttu-id="49e70-464">Prawidłowe wartości segmentów minut i sekund ciągu mieszczą się w zakresie 0-59.</span><span class="sxs-lookup"><span data-stu-id="49e70-464">Valid values for minutes and seconds segments of the string are in the range 0-59.</span></span> <span data-ttu-id="49e70-465">Użycie **60** w wartości minut lub sekund skutkuje *błędem wewnętrznego serwera 500*.</span><span class="sxs-lookup"><span data-stu-id="49e70-465">Use of **60** in the value for minutes or seconds results in a *500 - Internal Server Error*.</span></span></p> | <span data-ttu-id="49e70-466">Wartooć `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="49e70-466">Default: `00:02:00`</span></span><br><span data-ttu-id="49e70-467">Długości `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="49e70-467">Min: `00:00:00`</span></span><br><span data-ttu-id="49e70-468">Maksymalny `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="49e70-468">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="49e70-469">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="49e70-469">Optional integer attribute.</span></span></p><p><span data-ttu-id="49e70-470">Czas w sekundach, przez który moduł czeka na łagodne zamknięcie pliku wykonywalnego, gdy `app_offline.htm` zostanie wykryty.</span><span class="sxs-lookup"><span data-stu-id="49e70-470">Duration in seconds that the module waits for the executable to gracefully shutdown when the `app_offline.htm` file is detected.</span></span></p> | <span data-ttu-id="49e70-471">Wartooć `10`</span><span class="sxs-lookup"><span data-stu-id="49e70-471">Default: `10`</span></span><br><span data-ttu-id="49e70-472">Długości `0`</span><span class="sxs-lookup"><span data-stu-id="49e70-472">Min: `0`</span></span><br><span data-ttu-id="49e70-473">Maksymalny `600`</span><span class="sxs-lookup"><span data-stu-id="49e70-473">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="49e70-474">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="49e70-474">Optional integer attribute.</span></span></p><p><span data-ttu-id="49e70-475">Czas w sekundach, przez który moduł czeka, aż plik wykonywalny uruchomi proces nasłuchujący na porcie.</span><span class="sxs-lookup"><span data-stu-id="49e70-475">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="49e70-476">Jeśli ten limit czasu zostanie przekroczony, moduł zakasuje proces.</span><span class="sxs-lookup"><span data-stu-id="49e70-476">If this time limit is exceeded, the module kills the process.</span></span></p><p><span data-ttu-id="49e70-477">Podczas hostingu *w procesie*: proces **nie** jest ponownie uruchamiany i **nie używa tego** `rapidFailsPerMinute` Ustawienia.</span><span class="sxs-lookup"><span data-stu-id="49e70-477">When hosting *in-process*: The process is **not** restarted and does **not** use the `rapidFailsPerMinute` setting.</span></span></p><p><span data-ttu-id="49e70-478">Podczas hostingu *poza procesem*: moduł próbuje ponownie uruchomić proces, gdy odbierze nowe żądanie, i kontynuuje próbę ponownego uruchomienia procesu na kolejnych żądaniach przychodzących, chyba że aplikacja nie powiedzie się `rapidFailsPerMinute` w ciągu ostatniej minuty.</span><span class="sxs-lookup"><span data-stu-id="49e70-478">When hosting *out-of-process*: The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start `rapidFailsPerMinute` number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="49e70-479">Wartość 0 (zero) **nie** jest uważana za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="49e70-479">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="49e70-480">Wartooć `120`</span><span class="sxs-lookup"><span data-stu-id="49e70-480">Default: `120`</span></span><br><span data-ttu-id="49e70-481">Długości `0`</span><span class="sxs-lookup"><span data-stu-id="49e70-481">Min: `0`</span></span><br><span data-ttu-id="49e70-482">Maksymalny `3600`</span><span class="sxs-lookup"><span data-stu-id="49e70-482">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="49e70-483">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="49e70-483">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="49e70-484">Jeśli wartość jest równa true, **stdout** i **stderr** dla procesu określonego w programie `processPath` są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="49e70-484">If true, **stdout** and **stderr** for the process specified in `processPath` are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="49e70-485">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="49e70-485">Optional string attribute.</span></span></p><p><span data-ttu-id="49e70-486">Określa względną lub bezwzględną ścieżkę do pliku, dla którego `stdout` `stderr` proces określony w `processPath` jest rejestrowany.</span><span class="sxs-lookup"><span data-stu-id="49e70-486">Specifies the relative or absolute file path for which `stdout` and `stderr` from the process specified in `processPath` are logged.</span></span> <span data-ttu-id="49e70-487">Ścieżki względne są względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="49e70-487">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="49e70-488">Każda ścieżka rozpoczynająca `.` się od jest określana względem katalogu głównego witryny, a wszystkie inne ścieżki są traktowane jako ścieżki bezwzględne.</span><span class="sxs-lookup"><span data-stu-id="49e70-488">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="49e70-489">Wszystkie foldery podane w ścieżce są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="49e70-489">Any folders provided in the path are created by the module when the log file is created.</span></span> <span data-ttu-id="49e70-490">Przy użyciu ograniczników podkreślenia, sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku ( `.log` ) są dodawane do ostatniego segmentu `stdoutLogFile` ścieżki.</span><span class="sxs-lookup"><span data-stu-id="49e70-490">Using underscore delimiters, a timestamp, process ID, and file extension (`.log`) are added to the last segment of the `stdoutLogFile` path.</span></span> <span data-ttu-id="49e70-491">Jeśli `.\logs\stdout` jest podana jako wartość, przykładowy dziennik stdout jest zapisywany jako `stdout_20180205194132_1934.log` w folderze, `logs` gdy zapisywany w dniu 2/5/2018 o godzinie 19:41:32 z identyfikatorem procesu 1934.</span><span class="sxs-lookup"><span data-stu-id="49e70-491">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as `stdout_20180205194132_1934.log` in the `logs` folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="49e70-492">Ustawianie zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="49e70-492">Setting environment variables</span></span>

<span data-ttu-id="49e70-493">Zmienne środowiskowe można określić dla procesu w `processPath` atrybucie.</span><span class="sxs-lookup"><span data-stu-id="49e70-493">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="49e70-494">Określ zmienną środowiskową z `<environmentVariable>` elementem podrzędnym `<environmentVariables>` elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="49e70-494">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span> <span data-ttu-id="49e70-495">Zmienne środowiskowe ustawione w tej sekcji mają pierwszeństwo przed zmiennymi środowiskowymi systemowymi.</span><span class="sxs-lookup"><span data-stu-id="49e70-495">Environment variables set in this section take precedence over system environment variables.</span></span>

<span data-ttu-id="49e70-496">W poniższym przykładzie są ustawiane dwie zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="49e70-496">The following example sets two environment variables.</span></span> <span data-ttu-id="49e70-497">`ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji do programu `Development` .</span><span class="sxs-lookup"><span data-stu-id="49e70-497">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="49e70-498">Deweloper może tymczasowo ustawić tę wartość w pliku, `web.config` Aby wymusić ładowanie [strony wyjątku dewelopera](xref:fundamentals/error-handling) podczas debugowania wyjątku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-498">A developer may temporarily set this value in the `web.config` file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="49e70-499">`CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, w której deweloper ma napisać kod, który odczytuje wartość przy uruchamianiu, aby utworzyć ścieżkę do ładowania pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-499">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile=".\logs\stdout"
      hostingModel="inprocess">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!NOTE]
> <span data-ttu-id="49e70-500">Alternatywą dla ustawienia środowiska bezpośrednio w programie `web.config` jest uwzględnienie `<EnvironmentName>` właściwości w [profilu publikacji (pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) lub w pliku projektu.</span><span class="sxs-lookup"><span data-stu-id="49e70-500">An alternative to setting the environment directly in `web.config` is to include the `<EnvironmentName>` property in the [publish profile (.pubxml)](xref:host-and-deploy/visual-studio-publish-profiles) or project file.</span></span> <span data-ttu-id="49e70-501">To podejście ustawia środowisko w `web.config` momencie opublikowania projektu:</span><span class="sxs-lookup"><span data-stu-id="49e70-501">This approach sets the environment in `web.config` when the project is published:</span></span>
>
> ```xml
> <PropertyGroup>
>   <EnvironmentName>Development</EnvironmentName>
> </PropertyGroup>
> ```

> [!WARNING]
> <span data-ttu-id="49e70-502">Dla `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej należy ustawić tylko `Development` serwery przejściowe i testowe, które nie są dostępne dla niezaufanych sieci, takich jak Internet.</span><span class="sxs-lookup"><span data-stu-id="49e70-502">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="49e70-503">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="49e70-503">app_offline.htm</span></span>

<span data-ttu-id="49e70-504">Jeśli plik o nazwie `app_offline.htm` zostanie wykryty w katalogu głównym aplikacji, moduł ASP.NET Core próbuje bezpiecznie zamknąć aplikację i zatrzymać przetwarzanie żądań przychodzących.</span><span class="sxs-lookup"><span data-stu-id="49e70-504">If a file with the name `app_offline.htm` is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="49e70-505">Jeśli aplikacja nadal działa po upływie liczby sekund zdefiniowanej w programie `shutdownTimeLimit` , moduł ASP.NET Core kasuje uruchomiony proces.</span><span class="sxs-lookup"><span data-stu-id="49e70-505">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="49e70-506">Gdy `app_offline.htm` plik jest obecny, moduł ASP.NET Core odpowiada na żądania, wysyłając z powrotem zawartość `app_offline.htm` pliku.</span><span class="sxs-lookup"><span data-stu-id="49e70-506">While the `app_offline.htm` file is present, the ASP.NET Core Module responds to requests by sending back the contents of the `app_offline.htm` file.</span></span> <span data-ttu-id="49e70-507">Po `app_offline.htm` usunięciu pliku, następne żądanie uruchamia aplikację.</span><span class="sxs-lookup"><span data-stu-id="49e70-507">When the `app_offline.htm` file is removed, the next request starts the app.</span></span>

<span data-ttu-id="49e70-508">W przypadku korzystania z modelu hostingu poza procesem aplikacja może nie zostać wyłączona natychmiast, jeśli istnieje otwarte połączenie.</span><span class="sxs-lookup"><span data-stu-id="49e70-508">When using the out-of-process hosting model, the app might not shut down immediately if there's an open connection.</span></span> <span data-ttu-id="49e70-509">Na przykład połączenie protokołu WebSocket może opóźnić zamykanie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-509">For example, a websocket connection may delay app shut down.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="49e70-510">Strona błędu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="49e70-510">Start-up error page</span></span>

<span data-ttu-id="49e70-511">Hosting zarówno w procesie, jak i poza procesem tworzy niestandardowe strony błędów, gdy nie można uruchomić aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-511">Both in-process and out-of-process hosting produce custom error pages when they fail to start the app.</span></span>

<span data-ttu-id="49e70-512">Jeśli moduł ASP.NET Core nie może odnaleźć programu obsługi żądania w procesie lub out-of-process, zostanie wyświetlona strona kodowa stanu *niepowodzenia ładowania 500,0-procesowego/wyjściowego* .</span><span class="sxs-lookup"><span data-stu-id="49e70-512">If the ASP.NET Core Module fails to find either the in-process or out-of-process request handler, a *500.0 - In-Process/Out-Of-Process Handler Load Failure* status code page appears.</span></span>

<span data-ttu-id="49e70-513">W przypadku hostingu w procesie, Jeśli uruchomienie aplikacji przez moduł ASP.NET Core nie powiedzie się, zostanie wyświetlona strona kod stanu *niepowodzenia 500,30* .</span><span class="sxs-lookup"><span data-stu-id="49e70-513">For in-process hosting if the ASP.NET Core Module fails to start the app, a *500.30 - Start Failure* status code page appears.</span></span>

<span data-ttu-id="49e70-514">W przypadku hostingu poza procesem, jeśli moduł ASP.NET Core nie może uruchomić procesu zaplecza lub proces zaplecza zostanie uruchomiony, ale nie nasłuchuje na skonfigurowanym porcie, zostanie wyświetlona strona kod stanu *niepowodzenia procesu 502,5* .</span><span class="sxs-lookup"><span data-stu-id="49e70-514">For out-of-process hosting if the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span>

<span data-ttu-id="49e70-515">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stanu 5xx usługi IIS, użyj `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="49e70-515">To suppress this page and revert to the default IIS 5xx status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="49e70-516">Aby uzyskać więcej informacji o konfigurowaniu niestandardowych komunikatów o błędach, zobacz [Błędy \<httpErrors> http ](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="49e70-516">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="49e70-517">Tworzenie i przekierowywanie dzienników</span><span class="sxs-lookup"><span data-stu-id="49e70-517">Log creation and redirection</span></span>

<span data-ttu-id="49e70-518">Moduł ASP.NET Core przekierowuje dane wyjściowe z konsoli stdout i stderr do dysku, jeśli `stdoutLogEnabled` `stdoutLogFile` atrybuty i `aspNetCore` elementu są ustawione.</span><span class="sxs-lookup"><span data-stu-id="49e70-518">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="49e70-519">Wszystkie foldery w `stdoutLogFile` ścieżce są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="49e70-519">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="49e70-520">Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\{APP POOL NAME}` do udostępnienia uprawnienia do zapisu, gdzie symbol zastępczy `{APP POOL NAME}` to nazwa puli aplikacji).</span><span class="sxs-lookup"><span data-stu-id="49e70-520">The app pool must have write access to the location where the logs are written (use `IIS AppPool\{APP POOL NAME}` to provide write permission, where the placeholder `{APP POOL NAME}` is the app pool name).</span></span>

<span data-ttu-id="49e70-521">Dzienniki nie są obracane, chyba że zostanie wykonane odtwarzanie procesów/ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="49e70-521">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="49e70-522">Ponosisz odpowiedzialność dostawcy usług hostingowych, aby ograniczyć ilość miejsca na dysku zużywanej przez dzienniki.</span><span class="sxs-lookup"><span data-stu-id="49e70-522">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="49e70-523">Użycie dziennika stdout jest zalecane tylko w przypadku rozwiązywania problemów z uruchamianiem aplikacji w usługach IIS lub w przypadku korzystania z [obsługi usług IIS w czasie projektowania w programie Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), a nie podczas debugowania lokalnego i uruchamiania aplikacji przy użyciu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="49e70-523">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="49e70-524">Nie używaj dziennika stdout w celu uzyskania ogólnych celów rejestrowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-524">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="49e70-525">Aby rejestrować procedury w aplikacji ASP.NET Core, użyj biblioteki rejestrowania, która ogranicza rozmiar pliku dziennika i obraca dzienniki.</span><span class="sxs-lookup"><span data-stu-id="49e70-525">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="49e70-526">Aby uzyskać więcej informacji, zobacz [dostawców rejestrowania innych](xref:fundamentals/logging/index#third-party-logging-providers)firm.</span><span class="sxs-lookup"><span data-stu-id="49e70-526">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="49e70-527">Sygnatura czasowa i rozszerzenie pliku są dodawane automatycznie podczas tworzenia pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="49e70-527">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="49e70-528">Nazwa pliku dziennika jest złożona przez dołączenie sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku ( `.log` ) do ostatniego segmentu `stdoutLogFile` ścieżki (zwykle `stdout` ) rozdzielanej znakami podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="49e70-528">The log file name is composed by appending the timestamp, process ID, and file extension (`.log`) to the last segment of the `stdoutLogFile` path (typically `stdout`) delimited by underscores.</span></span> <span data-ttu-id="49e70-529">Jeśli `stdoutLogFile` ścieżka kończy się na `stdout` , dziennik aplikacji o identyfikatorze PID 1934 utworzony w dniu 2/5/2018 o 19:42:32 ma nazwę pliku `stdout_20180205194132_1934.log` .</span><span class="sxs-lookup"><span data-stu-id="49e70-529">If the `stdoutLogFile` path ends with `stdout`, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name `stdout_20180205194132_1934.log`.</span></span>

<span data-ttu-id="49e70-530">Jeśli `stdoutLogEnabled` ma wartość false, błędy występujące podczas uruchamiania aplikacji są przechwytywane i emitowane do dziennika zdarzeń do 30 KB.</span><span class="sxs-lookup"><span data-stu-id="49e70-530">If `stdoutLogEnabled` is false, errors that occur on app startup are captured and emitted to the event log up to 30 KB.</span></span> <span data-ttu-id="49e70-531">Po uruchomieniu wszystkie dodatkowe dzienniki są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="49e70-531">After startup, all additional logs are discarded.</span></span>

<span data-ttu-id="49e70-532">Poniższy przykładowy `aspNetCore` element konfiguruje rejestrowanie stdout w ścieżce względnej `.\log\` .</span><span class="sxs-lookup"><span data-stu-id="49e70-532">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="49e70-533">Upewnij się, że tożsamość użytkownika puli aplikacji ma uprawnienia do zapisu w podanej ścieżce.</span><span class="sxs-lookup"><span data-stu-id="49e70-533">Confirm that the app pool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout"
    hostingModel="inprocess">
</aspNetCore>
```

<span data-ttu-id="49e70-534">W przypadku publikowania aplikacji dla Azure App Service wdrożenia zestaw SDK sieci Web ustawia `stdoutLogFile` wartość na `\\?\%home%\LogFiles\stdout` .</span><span class="sxs-lookup"><span data-stu-id="49e70-534">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="49e70-535">`%home`Zmienna środowiskowa jest wstępnie zdefiniowana dla aplikacji hostowanych przez Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="49e70-535">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="49e70-536">Aby uzyskać więcej informacji na temat formatów ścieżki, zobacz [formaty ścieżki plików w systemach Windows](/dotnet/standard/io/file-path-formats).</span><span class="sxs-lookup"><span data-stu-id="49e70-536">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="enhanced-diagnostic-logs"></a><span data-ttu-id="49e70-537">Udoskonalone dzienniki diagnostyczne</span><span class="sxs-lookup"><span data-stu-id="49e70-537">Enhanced diagnostic logs</span></span>

<span data-ttu-id="49e70-538">Moduł ASP.NET Core można skonfigurować w celu udostępnienia dzienników diagnostyki rozszerzonej.</span><span class="sxs-lookup"><span data-stu-id="49e70-538">The ASP.NET Core Module is configurable to provide enhanced diagnostics logs.</span></span> <span data-ttu-id="49e70-539">Dodaj `<handlerSettings>` element do `<aspNetCore>` elementu w `web.config` .</span><span class="sxs-lookup"><span data-stu-id="49e70-539">Add the `<handlerSettings>` element to the `<aspNetCore>` element in `web.config`.</span></span> <span data-ttu-id="49e70-540">Ustawienie w `debugLevel` celu `TRACE` ujawniania większej wierności informacji diagnostycznych:</span><span class="sxs-lookup"><span data-stu-id="49e70-540">Setting the `debugLevel` to `TRACE` exposes a higher fidelity of diagnostic information:</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="false"
    stdoutLogFile="\\?\%home%\LogFiles\stdout"
    hostingModel="inprocess">
  <handlerSettings>
    <handlerSetting name="debugFile" value=".\logs\aspnetcore-debug.log" />
    <handlerSetting name="debugLevel" value="FILE,TRACE" />
  </handlerSettings>
</aspNetCore>
```

<span data-ttu-id="49e70-541">Foldery w ścieżce przekazanej do `<handlerSetting>` wartości ( `logs` w poprzednim przykładzie) nie są automatycznie tworzone przez moduł i powinny być wcześniej dostępne we wdrożeniu.</span><span class="sxs-lookup"><span data-stu-id="49e70-541">Folders in the path provided to the `<handlerSetting>` value (`logs` in the preceding example) aren't created by the module automatically and should pre-exist in the deployment.</span></span> <span data-ttu-id="49e70-542">Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\{APP POOL NAME}` do udostępnienia uprawnienia do zapisu, gdzie symbol zastępczy `{APP POOL NAME}` to nazwa puli aplikacji).</span><span class="sxs-lookup"><span data-stu-id="49e70-542">The app pool must have write access to the location where the logs are written (use `IIS AppPool\{APP POOL NAME}` to provide write permission, where the placeholder `{APP POOL NAME}` is the app pool name).</span></span>

<span data-ttu-id="49e70-543">Wartości poziomu debugowania ( `debugLevel` ) mogą zawierać zarówno poziom, jak i lokalizację.</span><span class="sxs-lookup"><span data-stu-id="49e70-543">Debug level (`debugLevel`) values can include both the level and the location.</span></span>

<span data-ttu-id="49e70-544">Poziomy (w kolejności od najmniejszej do największej szczegółowości):</span><span class="sxs-lookup"><span data-stu-id="49e70-544">Levels (in order from least to most verbose):</span></span>

* <span data-ttu-id="49e70-545">BŁĄD</span><span class="sxs-lookup"><span data-stu-id="49e70-545">ERROR</span></span>
* <span data-ttu-id="49e70-546">OSTRZEŻENIE</span><span class="sxs-lookup"><span data-stu-id="49e70-546">WARNING</span></span>
* <span data-ttu-id="49e70-547">INFORMACJE</span><span class="sxs-lookup"><span data-stu-id="49e70-547">INFO</span></span>
* <span data-ttu-id="49e70-548">TRACE</span><span class="sxs-lookup"><span data-stu-id="49e70-548">TRACE</span></span>

<span data-ttu-id="49e70-549">Lokalizacje (wiele lokalizacji jest dozwolonych):</span><span class="sxs-lookup"><span data-stu-id="49e70-549">Locations (multiple locations are permitted):</span></span>

* <span data-ttu-id="49e70-550">KONSOLI</span><span class="sxs-lookup"><span data-stu-id="49e70-550">CONSOLE</span></span>
* <span data-ttu-id="49e70-551">ELEMENCIE</span><span class="sxs-lookup"><span data-stu-id="49e70-551">EVENTLOG</span></span>
* <span data-ttu-id="49e70-552">PLIK</span><span class="sxs-lookup"><span data-stu-id="49e70-552">FILE</span></span>

<span data-ttu-id="49e70-553">Ustawienia programu obsługi można także zapewnić za pomocą zmiennych środowiskowych:</span><span class="sxs-lookup"><span data-stu-id="49e70-553">The handler settings can also be provided via environment variables:</span></span>

* <span data-ttu-id="49e70-554">`ASPNETCORE_MODULE_DEBUG_FILE`: Ścieżka do pliku dziennika debugowania.</span><span class="sxs-lookup"><span data-stu-id="49e70-554">`ASPNETCORE_MODULE_DEBUG_FILE`: Path to the debug log file.</span></span> <span data-ttu-id="49e70-555">(Domyślnie: `aspnetcore-debug.log` )</span><span class="sxs-lookup"><span data-stu-id="49e70-555">(Default: `aspnetcore-debug.log`)</span></span>
* <span data-ttu-id="49e70-556">`ASPNETCORE_MODULE_DEBUG`: Debugowanie ustawienia poziomu.</span><span class="sxs-lookup"><span data-stu-id="49e70-556">`ASPNETCORE_MODULE_DEBUG`: Debug level setting.</span></span>

> [!WARNING]
> <span data-ttu-id="49e70-557">**Nie** pozostawiaj włączonej rejestracji debugowania w ramach wdrożenia dłużej niż jest to wymagane, aby rozwiązać problem.</span><span class="sxs-lookup"><span data-stu-id="49e70-557">Do **not** leave debug logging enabled in the deployment for longer than required to troubleshoot an issue.</span></span> <span data-ttu-id="49e70-558">Rozmiar dziennika nie jest ograniczony.</span><span class="sxs-lookup"><span data-stu-id="49e70-558">The size of the log isn't limited.</span></span> <span data-ttu-id="49e70-559">Pozostawienie włączonego dziennika debugowania może spowodować wyczerpanie dostępnego miejsca na dysku i awarię serwera lub usługi App Service.</span><span class="sxs-lookup"><span data-stu-id="49e70-559">Leaving the debug log enabled can exhaust the available disk space and crash the server or app service.</span></span>

<span data-ttu-id="49e70-560">Zapoznaj się z tematem [Konfiguracja z web.config](#configuration-with-webconfig) , aby zapoznać się z przykładem `aspNetCore` elementu w `web.config` pliku.</span><span class="sxs-lookup"><span data-stu-id="49e70-560">See [Configuration with web.config](#configuration-with-webconfig) for an example of the `aspNetCore` element in the `web.config` file.</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="49e70-561">Konfiguracja serwera proxy używa protokołu HTTP i tokenu parowania</span><span class="sxs-lookup"><span data-stu-id="49e70-561">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="49e70-562">*Dotyczy tylko hostingu poza procesem.*</span><span class="sxs-lookup"><span data-stu-id="49e70-562">*Only applies to out-of-process hosting.*</span></span>

<span data-ttu-id="49e70-563">Serwer proxy utworzony między modułem ASP.NET Core a Kestrel używa protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="49e70-563">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="49e70-564">Nie ma ryzyka podsłuchiwanie ruchu między modułem i Kestrel z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="49e70-564">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="49e70-565">Token parowania jest używany w celu zagwarantowania, że żądania odbierane przez Kestrel zostały przekazane przez usługi IIS i nie pochodzą z innego źródła.</span><span class="sxs-lookup"><span data-stu-id="49e70-565">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="49e70-566">Token parowania jest tworzony i ustawiany jako zmienna środowiskowa ( `ASPNETCORE_TOKEN` ) przez moduł.</span><span class="sxs-lookup"><span data-stu-id="49e70-566">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="49e70-567">Token parowania jest również ustawiany w nagłówku ( `MS-ASPNETCORE-TOKEN` ) na każdym żądaniu z serwerem proxy.</span><span class="sxs-lookup"><span data-stu-id="49e70-567">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="49e70-568">Oprogramowanie pośredniczące usług IIS sprawdza każde odebrane żądanie, aby potwierdzić, że wartość nagłówka tokenu parowania jest zgodna z wartością zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="49e70-568">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="49e70-569">Jeśli wartości tokenu są niezgodne, żądanie zostanie zarejestrowane i odrzucone.</span><span class="sxs-lookup"><span data-stu-id="49e70-569">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="49e70-570">Zmienna środowiskowa tokena parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="49e70-570">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="49e70-571">Bez znajomości wartości tokenu parowania osoba atakująca nie może przesłać żądań, które pomijają Ewidencjonowanie oprogramowania pośredniczącego usług IIS.</span><span class="sxs-lookup"><span data-stu-id="49e70-571">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="49e70-572">Moduł ASP.NET Core z konfiguracją udostępnioną usług IIS</span><span class="sxs-lookup"><span data-stu-id="49e70-572">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="49e70-573">Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami `TrustedInstaller` konta.</span><span class="sxs-lookup"><span data-stu-id="49e70-573">The ASP.NET Core Module installer runs with the privileges of the `TrustedInstaller` account.</span></span> <span data-ttu-id="49e70-574">Ponieważ konto systemu lokalnego nie ma uprawnień do modyfikowania dla ścieżki udziału używanej przez udostępnioną konfigurację usług IIS, Instalator zgłasza błąd odmowy dostępu podczas próby skonfigurowania ustawień modułu w `applicationHost.config` pliku w udziale.</span><span class="sxs-lookup"><span data-stu-id="49e70-574">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the `applicationHost.config` file on the share.</span></span>

<span data-ttu-id="49e70-575">W przypadku korzystania z konfiguracji udostępnionej przez usługi IIS na tym samym komputerze, na którym znajduje się instalacja usług IIS, uruchom Instalatora pakietu ASP.NET Core hostowania z `OPT_NO_SHARED_CONFIG_CHECK` parametrem ustawionym na `1` :</span><span class="sxs-lookup"><span data-stu-id="49e70-575">When using an IIS Shared Configuration on the same machine as the IIS installation, run the ASP.NET Core Hosting Bundle installer with the `OPT_NO_SHARED_CONFIG_CHECK` parameter set to `1`:</span></span>

```console
dotnet-hosting-{VERSION}.exe OPT_NO_SHARED_CONFIG_CHECK=1
```

<span data-ttu-id="49e70-576">Jeśli ścieżka do konfiguracji udostępnionej nie znajduje się na tym samym komputerze, na którym znajduje się instalacja usług IIS, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="49e70-576">When the path to the shared configuration isn't on the same machine as the IIS installation, follow these steps:</span></span>

1. <span data-ttu-id="49e70-577">Wyłącz konfigurację udostępnioną usług IIS.</span><span class="sxs-lookup"><span data-stu-id="49e70-577">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="49e70-578">Uruchom instalatora.</span><span class="sxs-lookup"><span data-stu-id="49e70-578">Run the installer.</span></span>
1. <span data-ttu-id="49e70-579">Wyeksportuj zaktualizowany `applicationHost.config` plik do udziału.</span><span class="sxs-lookup"><span data-stu-id="49e70-579">Export the updated `applicationHost.config` file to the share.</span></span>
1. <span data-ttu-id="49e70-580">Włącz ponownie konfigurację udostępnioną usług IIS.</span><span class="sxs-lookup"><span data-stu-id="49e70-580">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="49e70-581">Dzienniki instalacji pakietu i pakietów hostingu</span><span class="sxs-lookup"><span data-stu-id="49e70-581">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="49e70-582">Aby określić wersję zainstalowanego modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="49e70-582">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="49e70-583">W systemie hostingu przejdź do `%windir%\System32\inetsrv` .</span><span class="sxs-lookup"><span data-stu-id="49e70-583">On the hosting system, navigate to `%windir%\System32\inetsrv`.</span></span>
1. <span data-ttu-id="49e70-584">Znajdź `aspnetcore.dll` plik.</span><span class="sxs-lookup"><span data-stu-id="49e70-584">Locate the `aspnetcore.dll` file.</span></span>
1. <span data-ttu-id="49e70-585">Kliknij prawym przyciskiem myszy plik i wybierz polecenie **Właściwości** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="49e70-585">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="49e70-586">Wybierz kartę **szczegóły** . **Wersja pliku** i **Wersja produktu** reprezentują zainstalowaną wersję modułu.</span><span class="sxs-lookup"><span data-stu-id="49e70-586">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="49e70-587">Dzienniki Instalatora pakietu hostingu dla modułu znajdują się pod adresem `C:\\Users\\%UserName%\\AppData\\Local\\Temp` .</span><span class="sxs-lookup"><span data-stu-id="49e70-587">The Hosting Bundle installer logs for the module are found at `C:\\Users\\%UserName%\\AppData\\Local\\Temp`.</span></span> <span data-ttu-id="49e70-588">Plik ma nazwę `dd_DotNetCoreWinSvrHosting__\{TIMESTAMP}_000_AspNetCoreModule_x64.log` , gdzie symbol zastępczy `{TIMESTAMP}` jest sygnaturą czasową.</span><span class="sxs-lookup"><span data-stu-id="49e70-588">The file is named `dd_DotNetCoreWinSvrHosting__\{TIMESTAMP}_000_AspNetCoreModule_x64.log`, where the placeholder `{TIMESTAMP}` is the timestamp.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="49e70-589">Lokalizacje pliku modułu, schematu i konfiguracji</span><span class="sxs-lookup"><span data-stu-id="49e70-589">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="49e70-590">Moduł</span><span class="sxs-lookup"><span data-stu-id="49e70-590">Module</span></span>

<span data-ttu-id="49e70-591">**IIS (x86/amd64)**:</span><span class="sxs-lookup"><span data-stu-id="49e70-591">**IIS (x86/amd64)**:</span></span>

* `%windir%\System32\inetsrv\aspnetcore.dll`

* `%windir%\SysWOW64\inetsrv\aspnetcore.dll`

* `%ProgramFiles%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll`

* `%ProgramFiles(x86)%\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll`

<span data-ttu-id="49e70-592">**IIS Express (x86/amd64)**:</span><span class="sxs-lookup"><span data-stu-id="49e70-592">**IIS Express (x86/amd64)**:</span></span>

* `%ProgramFiles%\IIS Express\aspnetcore.dll`

* `%ProgramFiles(x86)%\IIS Express\aspnetcore.dll`

* `%ProgramFiles%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll`

* `%ProgramFiles(x86)%\IIS Express\Asp.Net Core Module\V2\aspnetcorev2.dll`

### <a name="schema"></a><span data-ttu-id="49e70-593">Schemat</span><span class="sxs-lookup"><span data-stu-id="49e70-593">Schema</span></span>

<span data-ttu-id="49e70-594">**IIS**</span><span class="sxs-lookup"><span data-stu-id="49e70-594">**IIS**</span></span>

* `%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml`

* `%windir%\System32\inetsrv\config\schema\aspnetcore_schema_v2.xml`

<span data-ttu-id="49e70-595">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="49e70-595">**IIS Express**</span></span>

* `%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml`

* `%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema_v2.xml`

### <a name="configuration"></a><span data-ttu-id="49e70-596">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="49e70-596">Configuration</span></span>

<span data-ttu-id="49e70-597">**IIS**</span><span class="sxs-lookup"><span data-stu-id="49e70-597">**IIS**</span></span>

* `%windir%\System32\inetsrv\config\applicationHost.config`

<span data-ttu-id="49e70-598">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="49e70-598">**IIS Express**</span></span>

* <span data-ttu-id="49e70-599">Program Visual Studio: `{APPLICATION ROOT}\.vs\config\applicationHost.config`</span><span class="sxs-lookup"><span data-stu-id="49e70-599">Visual Studio: `{APPLICATION ROOT}\.vs\config\applicationHost.config`</span></span>

* <span data-ttu-id="49e70-600">*iisexpress.exe* Interfejs `%USERPROFILE%\Documents\IISExpress\config\applicationhost.config`</span><span class="sxs-lookup"><span data-stu-id="49e70-600">*iisexpress.exe* CLI: `%USERPROFILE%\Documents\IISExpress\config\applicationhost.config`</span></span>

<span data-ttu-id="49e70-601">Pliki można znaleźć, wyszukując je `aspnetcore` w `applicationHost.config` pliku.</span><span class="sxs-lookup"><span data-stu-id="49e70-601">The files can be found by searching for `aspnetcore` in the `applicationHost.config` file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="49e70-602">Moduł ASP.NET Core jest natywnym modułem usług IIS, który jest podłączany do potoku usług IIS do przesyłania dalej żądań sieci Web do zaplecza ASP.NET Core aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-602">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="49e70-603">Obsługiwane wersje systemu Windows:</span><span class="sxs-lookup"><span data-stu-id="49e70-603">Supported Windows versions:</span></span>

* <span data-ttu-id="49e70-604">System Windows 7 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="49e70-604">Windows 7 or later</span></span>
* <span data-ttu-id="49e70-605">Windows Server 2008 R2 lub nowszy</span><span class="sxs-lookup"><span data-stu-id="49e70-605">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="49e70-606">Moduł działa tylko z Kestrel.</span><span class="sxs-lookup"><span data-stu-id="49e70-606">The module only works with Kestrel.</span></span> <span data-ttu-id="49e70-607">Moduł jest niezgodny z [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="49e70-607">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

<span data-ttu-id="49e70-608">Ponieważ ASP.NET Core aplikacje działają w procesie innym niż proces roboczy usług IIS, moduł obsługuje również zarządzanie procesami.</span><span class="sxs-lookup"><span data-stu-id="49e70-608">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="49e70-609">Moduł uruchamia proces dla aplikacji ASP.NET Core, gdy pierwsze żądanie zostanie odebrane i ponownie uruchomiony, jeśli wystąpi awaria.</span><span class="sxs-lookup"><span data-stu-id="49e70-609">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="49e70-610">Jest to zasadniczo takie samo zachowanie jak w przypadku aplikacji ASP.NET 4. x, które działają w procesie w usługach IIS, które są zarządzane przez [usługę aktywacji procesów systemu Windows (was)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="49e70-610">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="49e70-611">Na poniższym diagramie przedstawiono relację między usługami IIS, modułem ASP.NET Core i aplikacją:</span><span class="sxs-lookup"><span data-stu-id="49e70-611">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![Moduł ASP.NET Core](iis/index/_static/ancm-outofprocess.png)

<span data-ttu-id="49e70-613">Żądania docierają z sieci Web do sterownika HTTP.sys trybu jądra.</span><span class="sxs-lookup"><span data-stu-id="49e70-613">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="49e70-614">Sterownik kieruje żądania do usług IIS na skonfigurowanym porcie witryny sieci Web, zwykle 80 (HTTP) lub 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="49e70-614">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="49e70-615">Moduł przekazuje żądania do Kestrel na losowo wybranym porcie dla aplikacji, która nie jest portem 80 lub 443.</span><span class="sxs-lookup"><span data-stu-id="49e70-615">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80 or 443.</span></span>

<span data-ttu-id="49e70-616">Moduł określa port za pośrednictwem zmiennej środowiskowej podczas uruchamiania, a [oprogramowanie pośredniczące integracji usług IIS](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) konfiguruje serwer do nasłuchiwania `http://localhost:{port}` .</span><span class="sxs-lookup"><span data-stu-id="49e70-616">The module specifies the port via an environment variable at startup, and the [IIS Integration Middleware](xref:host-and-deploy/iis/index#enable-the-iisintegration-components) configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="49e70-617">Dodatkowe sprawdzenia są wykonywane, a żądania, które nie pochodzą z modułu, są odrzucane.</span><span class="sxs-lookup"><span data-stu-id="49e70-617">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="49e70-618">Moduł nie obsługuje przekazywania HTTPS, dlatego żądania są przekazywane przez protokół HTTP nawet wtedy, gdy są odbierane przez usługę IIS przez protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="49e70-618">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="49e70-619">Po podaniu przez Kestrel żądania z modułu żądanie jest wypychane do potoku ASP.NET Core pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="49e70-619">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="49e70-620">Potok oprogramowania pośredniczącego obsługuje żądanie i przekazuje go jako `HttpContext` wystąpienie do logiki aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-620">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="49e70-621">Oprogramowanie pośredniczące dodane przez integrację usług IIS aktualizuje schemat, zdalny adres IP i pathbase, aby można było przesłać żądanie do Kestrel.</span><span class="sxs-lookup"><span data-stu-id="49e70-621">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="49e70-622">Odpowiedź aplikacji jest przesyłana z powrotem do usług IIS, która wypycha ją z powrotem do klienta HTTP, który zainicjował żądanie.</span><span class="sxs-lookup"><span data-stu-id="49e70-622">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="49e70-623">Wiele modułów macierzystych, takich jak uwierzytelnianie systemu Windows, pozostaje aktywnych.</span><span class="sxs-lookup"><span data-stu-id="49e70-623">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="49e70-624">Aby dowiedzieć się więcej na temat modułów usług IIS aktywnych przy użyciu modułu ASP.NET Core, zobacz <xref:host-and-deploy/iis/modules> .</span><span class="sxs-lookup"><span data-stu-id="49e70-624">To learn more about IIS modules active with the ASP.NET Core Module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="49e70-625">Moduł ASP.NET Core może również:</span><span class="sxs-lookup"><span data-stu-id="49e70-625">The ASP.NET Core Module can also:</span></span>

* <span data-ttu-id="49e70-626">Ustaw zmienne środowiskowe dla procesu roboczego.</span><span class="sxs-lookup"><span data-stu-id="49e70-626">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="49e70-627">Rejestruj dane wyjściowe stdout do magazynu plików w celu rozwiązywania problemów z uruchamianiem.</span><span class="sxs-lookup"><span data-stu-id="49e70-627">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="49e70-628">Przekazuj tokeny uwierzytelniania systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="49e70-628">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="49e70-629">Jak zainstalować moduł ASP.NET Core i korzystać z niego</span><span class="sxs-lookup"><span data-stu-id="49e70-629">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="49e70-630">Aby uzyskać instrukcje dotyczące sposobu instalowania modułu ASP.NET Core, zobacz [installing the .NET Core hosting](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span><span class="sxs-lookup"><span data-stu-id="49e70-630">For instructions on how to install the ASP.NET Core Module, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

## <a name="configuration-with-webconfig"></a><span data-ttu-id="49e70-631">Konfiguracja z web.config</span><span class="sxs-lookup"><span data-stu-id="49e70-631">Configuration with web.config</span></span>

<span data-ttu-id="49e70-632">Moduł ASP.NET Core jest skonfigurowany przy użyciu `aspNetCore` sekcji `system.webServer` węzła w pliku *web.config* witryny.</span><span class="sxs-lookup"><span data-stu-id="49e70-632">The ASP.NET Core Module is configured with the `aspNetCore` section of the `system.webServer` node in the site's *web.config* file.</span></span>

<span data-ttu-id="49e70-633">Następujący *web.config* plik jest publikowany dla [wdrożenia zależnego od platformy](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) i konfiguruje moduł ASP.NET Core do obsługi żądań lokacji:</span><span class="sxs-lookup"><span data-stu-id="49e70-633">The following *web.config* file is published for a [framework-dependent deployment](/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) and configures the ASP.NET Core Module to handle site requests:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="dotnet"
                arguments=".\MyApp.dll"
                stdoutLogEnabled="false"
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="49e70-634">Następujące *web.config* są publikowane dla [wdrożenia samodzielnego](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span><span class="sxs-lookup"><span data-stu-id="49e70-634">The following *web.config* is published for a [self-contained deployment](/dotnet/articles/core/deploying/#self-contained-deployments-scd):</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe"
                stdoutLogEnabled="false"
                stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="49e70-635">Gdy aplikacja zostanie wdrożona do [Azure App Service](https://azure.microsoft.com/services/app-service/), `stdoutLogFile` ścieżka jest ustawiona na `\\?\%home%\LogFiles\stdout` .</span><span class="sxs-lookup"><span data-stu-id="49e70-635">When an app is deployed to [Azure App Service](https://azure.microsoft.com/services/app-service/), the `stdoutLogFile` path is set to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="49e70-636">Ścieżka zapisuje dzienniki stdout do folderu *LogFiles* , który jest lokalizacją automatycznie utworzoną przez usługę.</span><span class="sxs-lookup"><span data-stu-id="49e70-636">The path saves stdout logs to the *LogFiles* folder, which is a location automatically created by the service.</span></span>

<span data-ttu-id="49e70-637">Aby uzyskać informacje na temat konfiguracji aplikacji podrzędnych usług IIS, zobacz <xref:host-and-deploy/iis/index#sub-applications> .</span><span class="sxs-lookup"><span data-stu-id="49e70-637">For information on IIS sub-application configuration, see <xref:host-and-deploy/iis/index#sub-applications>.</span></span>

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="49e70-638">Atrybuty elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="49e70-638">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="49e70-639">Atrybut</span><span class="sxs-lookup"><span data-stu-id="49e70-639">Attribute</span></span> | <span data-ttu-id="49e70-640">Opis</span><span class="sxs-lookup"><span data-stu-id="49e70-640">Description</span></span> | <span data-ttu-id="49e70-641">Domyślne</span><span class="sxs-lookup"><span data-stu-id="49e70-641">Default</span></span> |
| --------- | ----------- | :-----: |
| `arguments` | <p><span data-ttu-id="49e70-642">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="49e70-642">Optional string attribute.</span></span></p><p><span data-ttu-id="49e70-643">Argumenty do pliku wykonywalnego określonego w **processPath**.</span><span class="sxs-lookup"><span data-stu-id="49e70-643">Arguments to the executable specified in **processPath**.</span></span></p>| |
| `disableStartUpErrorPage` | <p><span data-ttu-id="49e70-644">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="49e70-644">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="49e70-645">W przypadku wartości true strona **błędu 502,5 procesu** jest pomijana, a strona kodu stanu 502 skonfigurowana w *web.config* ma pierwszeństwo.</span><span class="sxs-lookup"><span data-stu-id="49e70-645">If true, the **502.5 - Process Failure** page is suppressed, and the 502 status code page configured in the *web.config* takes precedence.</span></span></p> | `false` |
| `forwardWindowsAuthToken` | <p><span data-ttu-id="49e70-646">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="49e70-646">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="49e70-647">Jeśli wartość jest równa true, token jest przekazywany do procesu podrzędnego, który nasłuchuje na% ASPNETCORE_PORT% jako nagłówek "MS-ASPNETCORE-WINAUTHTOKEN" na żądanie.</span><span class="sxs-lookup"><span data-stu-id="49e70-647">If true, the token is forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="49e70-648">Jest odpowiedzialny za ten proces, aby wywołać metodę CloseHandle na tym tokenie na żądanie.</span><span class="sxs-lookup"><span data-stu-id="49e70-648">It's the responsibility of that process to call CloseHandle on this token per request.</span></span></p> | `true` |
| `processesPerApplication` | <p><span data-ttu-id="49e70-649">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="49e70-649">Optional integer attribute.</span></span></p><p><span data-ttu-id="49e70-650">Określa liczbę wystąpień procesu określonego w ustawieniu **processPath** , które może być przypadające na aplikację.</span><span class="sxs-lookup"><span data-stu-id="49e70-650">Specifies the number of instances of the process specified in the **processPath** setting that can be spun up per app.</span></span></p><p><span data-ttu-id="49e70-651">Ustawienie `processesPerApplication` jest niezalecane.</span><span class="sxs-lookup"><span data-stu-id="49e70-651">Setting `processesPerApplication` is discouraged.</span></span> <span data-ttu-id="49e70-652">Ten atrybut zostanie usunięty w przyszłych wydaniach.</span><span class="sxs-lookup"><span data-stu-id="49e70-652">This attribute will be removed in a future release.</span></span></p> | <span data-ttu-id="49e70-653">Wartooć `1`</span><span class="sxs-lookup"><span data-stu-id="49e70-653">Default: `1`</span></span><br><span data-ttu-id="49e70-654">Długości `1`</span><span class="sxs-lookup"><span data-stu-id="49e70-654">Min: `1`</span></span><br><span data-ttu-id="49e70-655">Maksymalny `100`</span><span class="sxs-lookup"><span data-stu-id="49e70-655">Max: `100`</span></span> |
| `processPath` | <p><span data-ttu-id="49e70-656">Wymagany atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="49e70-656">Required string attribute.</span></span></p><p><span data-ttu-id="49e70-657">Ścieżka do pliku wykonywalnego, który uruchamia proces nasłuchiwanie żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="49e70-657">Path to the executable that launches a process listening for HTTP requests.</span></span> <span data-ttu-id="49e70-658">Obsługiwane są ścieżki względne.</span><span class="sxs-lookup"><span data-stu-id="49e70-658">Relative paths are supported.</span></span> <span data-ttu-id="49e70-659">Jeśli ścieżka zaczyna się od `.` , ścieżka jest uznawana za względną względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="49e70-659">If the path begins with `.`, the path is considered to be relative to the site root.</span></span></p> | |
| `rapidFailsPerMinute` | <p><span data-ttu-id="49e70-660">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="49e70-660">Optional integer attribute.</span></span></p><p><span data-ttu-id="49e70-661">Określa, ile razy proces określony w **processPath** może ulec awarii na minutę.</span><span class="sxs-lookup"><span data-stu-id="49e70-661">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="49e70-662">W przypadku przekroczenia tego limitu moduł przestaje uruchomić proces przez pozostałą część minuty.</span><span class="sxs-lookup"><span data-stu-id="49e70-662">If this limit is exceeded, the module stops launching the process for the remainder of the minute.</span></span></p> | <span data-ttu-id="49e70-663">Wartooć `10`</span><span class="sxs-lookup"><span data-stu-id="49e70-663">Default: `10`</span></span><br><span data-ttu-id="49e70-664">Długości `0`</span><span class="sxs-lookup"><span data-stu-id="49e70-664">Min: `0`</span></span><br><span data-ttu-id="49e70-665">Maksymalny `100`</span><span class="sxs-lookup"><span data-stu-id="49e70-665">Max: `100`</span></span> |
| `requestTimeout` | <p><span data-ttu-id="49e70-666">Opcjonalny atrybut TimeSpan.</span><span class="sxs-lookup"><span data-stu-id="49e70-666">Optional timespan attribute.</span></span></p><p><span data-ttu-id="49e70-667">Określa czas, przez który moduł ASP.NET Core czeka na odpowiedź z procesu nasłuchiwania na% ASPNETCORE_PORT%.</span><span class="sxs-lookup"><span data-stu-id="49e70-667">Specifies the duration for which the ASP.NET Core Module waits for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="49e70-668">W wersjach modułu ASP.NET Core, który został dostarczony z wersją ASP.NET Core 2,1 lub nowszą, wartość `requestTimeout` jest określona w godzinach, minutach i sekundach.</span><span class="sxs-lookup"><span data-stu-id="49e70-668">In versions of the ASP.NET Core Module that shipped with the release of ASP.NET Core 2.1 or later, the `requestTimeout` is specified in hours, minutes, and seconds.</span></span></p> | <span data-ttu-id="49e70-669">Wartooć `00:02:00`</span><span class="sxs-lookup"><span data-stu-id="49e70-669">Default: `00:02:00`</span></span><br><span data-ttu-id="49e70-670">Długości `00:00:00`</span><span class="sxs-lookup"><span data-stu-id="49e70-670">Min: `00:00:00`</span></span><br><span data-ttu-id="49e70-671">Maksymalny `360:00:00`</span><span class="sxs-lookup"><span data-stu-id="49e70-671">Max: `360:00:00`</span></span> |
| `shutdownTimeLimit` | <p><span data-ttu-id="49e70-672">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="49e70-672">Optional integer attribute.</span></span></p><p><span data-ttu-id="49e70-673">Czas w sekundach, przez który moduł czeka, aż plik wykonywalny zostanie bezpiecznie zamknięty po wykryciu pliku *app_offline.htm* .</span><span class="sxs-lookup"><span data-stu-id="49e70-673">Duration in seconds that the module waits for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p> | <span data-ttu-id="49e70-674">Wartooć `10`</span><span class="sxs-lookup"><span data-stu-id="49e70-674">Default: `10`</span></span><br><span data-ttu-id="49e70-675">Długości `0`</span><span class="sxs-lookup"><span data-stu-id="49e70-675">Min: `0`</span></span><br><span data-ttu-id="49e70-676">Maksymalny `600`</span><span class="sxs-lookup"><span data-stu-id="49e70-676">Max: `600`</span></span> |
| `startupTimeLimit` | <p><span data-ttu-id="49e70-677">Opcjonalny atrybut Integer.</span><span class="sxs-lookup"><span data-stu-id="49e70-677">Optional integer attribute.</span></span></p><p><span data-ttu-id="49e70-678">Czas w sekundach, przez który moduł czeka, aż plik wykonywalny uruchomi proces nasłuchujący na porcie.</span><span class="sxs-lookup"><span data-stu-id="49e70-678">Duration in seconds that the module waits for the executable to start a process listening on the port.</span></span> <span data-ttu-id="49e70-679">Jeśli ten limit czasu zostanie przekroczony, moduł zakasuje proces.</span><span class="sxs-lookup"><span data-stu-id="49e70-679">If this time limit is exceeded, the module kills the process.</span></span> <span data-ttu-id="49e70-680">Moduł podejmuje próbę ponownego uruchomienia procesu, gdy odbierze nowe żądanie i kontynuuje ponowne uruchomienie procesu na kolejnych żądaniach przychodzących, chyba że aplikacja nie będzie mogła uruchomić **rapidFailsPerMinute** liczbę razy w ostatniej minucie.</span><span class="sxs-lookup"><span data-stu-id="49e70-680">The module attempts to relaunch the process when it receives a new request and continues to attempt to restart the process on subsequent incoming requests unless the app fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="49e70-681">Wartość 0 (zero) **nie** jest uważana za nieskończony limit czasu.</span><span class="sxs-lookup"><span data-stu-id="49e70-681">A value of 0 (zero) is **not** considered an infinite timeout.</span></span></p> | <span data-ttu-id="49e70-682">Wartooć `120`</span><span class="sxs-lookup"><span data-stu-id="49e70-682">Default: `120`</span></span><br><span data-ttu-id="49e70-683">Długości `0`</span><span class="sxs-lookup"><span data-stu-id="49e70-683">Min: `0`</span></span><br><span data-ttu-id="49e70-684">Maksymalny `3600`</span><span class="sxs-lookup"><span data-stu-id="49e70-684">Max: `3600`</span></span> |
| `stdoutLogEnabled` | <p><span data-ttu-id="49e70-685">Opcjonalny atrybut Boolean.</span><span class="sxs-lookup"><span data-stu-id="49e70-685">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="49e70-686">Jeśli wartość jest równa true, **stdout** i **stderr** dla procesu określonego w **processPath** są przekierowywane do pliku określonego w **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="49e70-686">If true, **stdout** and **stderr** for the process specified in **processPath** are redirected to the file specified in **stdoutLogFile**.</span></span></p> | `false` |
| `stdoutLogFile` | <p><span data-ttu-id="49e70-687">Opcjonalny atrybut ciągu.</span><span class="sxs-lookup"><span data-stu-id="49e70-687">Optional string attribute.</span></span></p><p><span data-ttu-id="49e70-688">Określa względną lub bezwzględną ścieżkę do pliku, dla którego jest rejestrowany **stdout** i **stderr** z procesu określonego w **processPath** .</span><span class="sxs-lookup"><span data-stu-id="49e70-688">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** are logged.</span></span> <span data-ttu-id="49e70-689">Ścieżki względne są względne względem katalogu głównego witryny.</span><span class="sxs-lookup"><span data-stu-id="49e70-689">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="49e70-690">Każda ścieżka rozpoczynająca `.` się od jest określana względem katalogu głównego witryny, a wszystkie inne ścieżki są traktowane jako ścieżki bezwzględne.</span><span class="sxs-lookup"><span data-stu-id="49e70-690">Any path starting with `.` are relative to the site root and all other paths are treated as absolute paths.</span></span> <span data-ttu-id="49e70-691">Wszystkie foldery podane w ścieżce muszą istnieć, aby moduł mógł utworzyć plik dziennika.</span><span class="sxs-lookup"><span data-stu-id="49e70-691">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="49e70-692">Przy użyciu ograniczników podkreślenia, sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku (*log*) są dodawane do ostatniego segmentu ścieżki **stdoutLogFile** .</span><span class="sxs-lookup"><span data-stu-id="49e70-692">Using underscore delimiters, a timestamp, process ID, and file extension (*.log*) are added to the last segment of the **stdoutLogFile** path.</span></span> <span data-ttu-id="49e70-693">Jeśli `.\logs\stdout` jest podana jako wartość, przykładowy dziennik stdout jest zapisywany jako *stdout_20180205194132_1934. log* w folderze *Logs* , gdy jest zapisywany na 2/5/2018 o godzinie 19:41:32 przy użyciu identyfikatora procesu 1934.</span><span class="sxs-lookup"><span data-stu-id="49e70-693">If `.\logs\stdout` is supplied as a value, an example stdout log is saved as *stdout_20180205194132_1934.log* in the *logs* folder when saved on 2/5/2018 at 19:41:32 with a process ID of 1934.</span></span></p> | `aspnetcore-stdout` |

### <a name="setting-environment-variables"></a><span data-ttu-id="49e70-694">Ustawianie zmiennych środowiskowych</span><span class="sxs-lookup"><span data-stu-id="49e70-694">Setting environment variables</span></span>

<span data-ttu-id="49e70-695">Zmienne środowiskowe można określić dla procesu w `processPath` atrybucie.</span><span class="sxs-lookup"><span data-stu-id="49e70-695">Environment variables can be specified for the process in the `processPath` attribute.</span></span> <span data-ttu-id="49e70-696">Określ zmienną środowiskową z `<environmentVariable>` elementem podrzędnym `<environmentVariables>` elementu kolekcji.</span><span class="sxs-lookup"><span data-stu-id="49e70-696">Specify an environment variable with the `<environmentVariable>` child element of an `<environmentVariables>` collection element.</span></span>

> [!WARNING]
> <span data-ttu-id="49e70-697">Zmienne środowiskowe ustawione w tej sekcji powodują konflikt z systemowymi zmiennymi środowiskowymi ustawionymi z tą samą nazwą.</span><span class="sxs-lookup"><span data-stu-id="49e70-697">Environment variables set in this section conflict with system environment variables set with the same name.</span></span> <span data-ttu-id="49e70-698">Jeśli zmienna środowiskowa jest ustawiona zarówno w pliku *web.config* , jak i na poziomie systemu w systemie Windows, wartość z pliku *web.config* zostanie dołączona do systemowej wartości zmiennej środowiskowej (na przykład `ASPNETCORE_ENVIRONMENT: Development;Development` ), co uniemożliwia uruchomienie aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-698">If an environment variable is set in both the *web.config* file and at the system level in Windows, the value from the *web.config* file becomes appended to the system environment variable value (for example, `ASPNETCORE_ENVIRONMENT: Development;Development`), which prevents the app from starting.</span></span>

<span data-ttu-id="49e70-699">W poniższym przykładzie są ustawiane dwie zmienne środowiskowe.</span><span class="sxs-lookup"><span data-stu-id="49e70-699">The following example sets two environment variables.</span></span> <span data-ttu-id="49e70-700">`ASPNETCORE_ENVIRONMENT` konfiguruje środowisko aplikacji do programu `Development` .</span><span class="sxs-lookup"><span data-stu-id="49e70-700">`ASPNETCORE_ENVIRONMENT` configures the app's environment to `Development`.</span></span> <span data-ttu-id="49e70-701">Deweloper może tymczasowo ustawić tę wartość w pliku *web.config* w celu wymuszenia załadowania [strony wyjątku dewelopera](xref:fundamentals/error-handling) podczas debugowania wyjątku aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-701">A developer may temporarily set this value in the *web.config* file in order to force the [Developer Exception Page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="49e70-702">`CONFIG_DIR` to przykład zmiennej środowiskowej zdefiniowanej przez użytkownika, w której deweloper ma napisać kod, który odczytuje wartość przy uruchamianiu, aby utworzyć ścieżkę do ładowania pliku konfiguracji aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-702">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that reads the value on startup to form a path for loading the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

> [!WARNING]
> <span data-ttu-id="49e70-703">Dla `ASPNETCORE_ENVIRONMENT` zmiennej środowiskowej należy ustawić tylko `Development` serwery przejściowe i testowe, które nie są dostępne dla niezaufanych sieci, takich jak Internet.</span><span class="sxs-lookup"><span data-stu-id="49e70-703">Only set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development` on staging and testing servers that aren't accessible to untrusted networks, such as the Internet.</span></span>

## <a name="app_offlinehtm"></a><span data-ttu-id="49e70-704">app_offline.htm</span><span class="sxs-lookup"><span data-stu-id="49e70-704">app_offline.htm</span></span>

<span data-ttu-id="49e70-705">Jeśli plik o nazwie *app_offline.htm* zostanie wykryty w katalogu głównym aplikacji, moduł ASP.NET Core próbuje bezpiecznie zamknąć aplikację i zatrzymać przetwarzanie żądań przychodzących.</span><span class="sxs-lookup"><span data-stu-id="49e70-705">If a file with the name *app_offline.htm* is detected in the root directory of an app, the ASP.NET Core Module attempts to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="49e70-706">Jeśli aplikacja nadal działa po upływie liczby sekund zdefiniowanej w programie `shutdownTimeLimit` , moduł ASP.NET Core kasuje uruchomiony proces.</span><span class="sxs-lookup"><span data-stu-id="49e70-706">If the app is still running after the number of seconds defined in `shutdownTimeLimit`, the ASP.NET Core Module kills the running process.</span></span>

<span data-ttu-id="49e70-707">Gdy plik *app_offline.htm* jest obecny, moduł ASP.NET Core reaguje na żądania, wysyłając z powrotem zawartość pliku *app_offline.htm* .</span><span class="sxs-lookup"><span data-stu-id="49e70-707">While the *app_offline.htm* file is present, the ASP.NET Core Module responds to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="49e70-708">Po usunięciu pliku *app_offline.htm* następnym żądaniu zostanie uruchomiona aplikacja.</span><span class="sxs-lookup"><span data-stu-id="49e70-708">When the *app_offline.htm* file is removed, the next request starts the app.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="49e70-709">Strona błędu uruchamiania</span><span class="sxs-lookup"><span data-stu-id="49e70-709">Start-up error page</span></span>

<span data-ttu-id="49e70-710">Jeśli moduł ASP.NET Core nie może uruchomić procesu zaplecza lub proces zaplecza zostanie uruchomiony, ale nie nasłuchuje na skonfigurowanym porcie, zostanie wyświetlona strona kod stanu *niepowodzenia procesu 502,5* .</span><span class="sxs-lookup"><span data-stu-id="49e70-710">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, a *502.5 - Process Failure* status code page appears.</span></span> <span data-ttu-id="49e70-711">Aby pominąć tę stronę i przywrócić domyślną stronę kodową stanu 502 usług IIS, użyj `disableStartUpErrorPage` atrybutu.</span><span class="sxs-lookup"><span data-stu-id="49e70-711">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="49e70-712">Aby uzyskać więcej informacji o konfigurowaniu niestandardowych komunikatów o błędach, zobacz [Błędy \<httpErrors> http ](/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="49e70-712">For more information on configuring custom error messages, see [HTTP Errors \<httpErrors>](/iis/configuration/system.webServer/httpErrors/).</span></span>

## <a name="log-creation-and-redirection"></a><span data-ttu-id="49e70-713">Tworzenie i przekierowywanie dzienników</span><span class="sxs-lookup"><span data-stu-id="49e70-713">Log creation and redirection</span></span>

<span data-ttu-id="49e70-714">Moduł ASP.NET Core przekierowuje dane wyjściowe z konsoli stdout i stderr do dysku, jeśli `stdoutLogEnabled` `stdoutLogFile` atrybuty i `aspNetCore` elementu są ustawione.</span><span class="sxs-lookup"><span data-stu-id="49e70-714">The ASP.NET Core Module redirects stdout and stderr console output to disk if the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element are set.</span></span> <span data-ttu-id="49e70-715">Wszystkie foldery w `stdoutLogFile` ścieżce są tworzone przez moduł po utworzeniu pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="49e70-715">Any folders in the `stdoutLogFile` path are created by the module when the log file is created.</span></span> <span data-ttu-id="49e70-716">Pula aplikacji musi mieć dostęp do zapisu w lokalizacji, w której zapisano dzienniki (Użyj `IIS AppPool\<app_pool_name>` do zapewnienia uprawnienia do zapisu).</span><span class="sxs-lookup"><span data-stu-id="49e70-716">The app pool must have write access to the location where the logs are written (use `IIS AppPool\<app_pool_name>` to provide write permission).</span></span>

<span data-ttu-id="49e70-717">Dzienniki nie są obracane, chyba że zostanie wykonane odtwarzanie procesów/ponowne uruchomienie.</span><span class="sxs-lookup"><span data-stu-id="49e70-717">Logs aren't rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="49e70-718">Ponosisz odpowiedzialność dostawcy usług hostingowych, aby ograniczyć ilość miejsca na dysku zużywanej przez dzienniki.</span><span class="sxs-lookup"><span data-stu-id="49e70-718">It's the responsibility of the hoster to limit the disk space the logs consume.</span></span>

<span data-ttu-id="49e70-719">Użycie dziennika stdout jest zalecane tylko w przypadku rozwiązywania problemów z uruchamianiem aplikacji w usługach IIS lub w przypadku korzystania z [obsługi usług IIS w czasie projektowania w programie Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), a nie podczas debugowania lokalnego i uruchamiania aplikacji przy użyciu IIS Express.</span><span class="sxs-lookup"><span data-stu-id="49e70-719">Using the stdout log is only recommended for troubleshooting app startup issues when hosting on IIS or when using [development-time support for IIS with Visual Studio](xref:host-and-deploy/iis/development-time-iis-support), not while debugging locally and running the app with IIS Express.</span></span>

<span data-ttu-id="49e70-720">Nie używaj dziennika stdout w celu uzyskania ogólnych celów rejestrowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="49e70-720">Don't use the stdout log for general app logging purposes.</span></span> <span data-ttu-id="49e70-721">Aby rejestrować procedury w aplikacji ASP.NET Core, użyj biblioteki rejestrowania, która ogranicza rozmiar pliku dziennika i obraca dzienniki.</span><span class="sxs-lookup"><span data-stu-id="49e70-721">For routine logging in an ASP.NET Core app, use a logging library that limits log file size and rotates logs.</span></span> <span data-ttu-id="49e70-722">Aby uzyskać więcej informacji, zobacz [dostawców rejestrowania innych](xref:fundamentals/logging/index#third-party-logging-providers)firm.</span><span class="sxs-lookup"><span data-stu-id="49e70-722">For more information, see [third-party logging providers](xref:fundamentals/logging/index#third-party-logging-providers).</span></span>

<span data-ttu-id="49e70-723">Sygnatura czasowa i rozszerzenie pliku są dodawane automatycznie podczas tworzenia pliku dziennika.</span><span class="sxs-lookup"><span data-stu-id="49e70-723">A timestamp and file extension are added automatically when the log file is created.</span></span> <span data-ttu-id="49e70-724">Nazwa pliku dziennika składa się przez dołączenie sygnatury czasowej, identyfikatora procesu i rozszerzenia pliku (*log*) do ostatniego segmentu `stdoutLogFile` ścieżki (zazwyczaj *stdout*), które są rozdzielane znakami podkreślenia.</span><span class="sxs-lookup"><span data-stu-id="49e70-724">The log file name is composed by appending the timestamp, process ID, and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="49e70-725">Jeśli `stdoutLogFile` ścieżka kończy się od *stdout*, dziennik aplikacji o identyfikatorze PID 1934 utworzony w 2/5/2018 o 19:42:32 ma nazwę pliku *stdout_20180205194132_1934. log*.</span><span class="sxs-lookup"><span data-stu-id="49e70-725">If the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 1934 created on 2/5/2018 at 19:42:32 has the file name *stdout_20180205194132_1934.log*.</span></span>

<span data-ttu-id="49e70-726">Poniższy przykładowy `aspNetCore` element konfiguruje rejestrowanie stdout w ścieżce względnej `.\log\` .</span><span class="sxs-lookup"><span data-stu-id="49e70-726">The following sample `aspNetCore` element configures stdout logging at the relative path `.\log\`.</span></span> <span data-ttu-id="49e70-727">Upewnij się, że tożsamość użytkownika puli aplikacji ma uprawnienia do zapisu w podanej ścieżce.</span><span class="sxs-lookup"><span data-stu-id="49e70-727">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile=".\logs\stdout">
</aspNetCore>
```

<span data-ttu-id="49e70-728">W przypadku publikowania aplikacji dla Azure App Service wdrożenia zestaw SDK sieci Web ustawia `stdoutLogFile` wartość na `\\?\%home%\LogFiles\stdout` .</span><span class="sxs-lookup"><span data-stu-id="49e70-728">When publishing an app for Azure App Service deployment, the Web SDK sets the `stdoutLogFile` value to `\\?\%home%\LogFiles\stdout`.</span></span> <span data-ttu-id="49e70-729">`%home`Zmienna środowiskowa jest wstępnie zdefiniowana dla aplikacji hostowanych przez Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="49e70-729">The `%home` environment variable is predefined for apps hosted by Azure App Service.</span></span>

<span data-ttu-id="49e70-730">Aby utworzyć reguły filtru rejestrowania, zobacz sekcje [Konfiguracja](xref:fundamentals/logging/index#log-filtering) i [filtrowanie dzienników](xref:fundamentals/logging/index#log-filtering) w dokumentacji rejestrowania ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49e70-730">To create logging filter rules, see the [Configuration](xref:fundamentals/logging/index#log-filtering) and [Log filtering](xref:fundamentals/logging/index#log-filtering) sections of the ASP.NET Core logging documentation.</span></span>

<span data-ttu-id="49e70-731">Aby uzyskać więcej informacji na temat formatów ścieżki, zobacz [formaty ścieżki plików w systemach Windows](/dotnet/standard/io/file-path-formats).</span><span class="sxs-lookup"><span data-stu-id="49e70-731">For more information on path formats, see [File path formats on Windows systems](/dotnet/standard/io/file-path-formats).</span></span>

## <a name="proxy-configuration-uses-http-protocol-and-a-pairing-token"></a><span data-ttu-id="49e70-732">Konfiguracja serwera proxy używa protokołu HTTP i tokenu parowania</span><span class="sxs-lookup"><span data-stu-id="49e70-732">Proxy configuration uses HTTP protocol and a pairing token</span></span>

<span data-ttu-id="49e70-733">Serwer proxy utworzony między modułem ASP.NET Core a Kestrel używa protokołu HTTP.</span><span class="sxs-lookup"><span data-stu-id="49e70-733">The proxy created between the ASP.NET Core Module and Kestrel uses the HTTP protocol.</span></span> <span data-ttu-id="49e70-734">Nie ma ryzyka podsłuchiwanie ruchu między modułem i Kestrel z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="49e70-734">There's no risk of eavesdropping the traffic between the module and Kestrel from a location off of the server.</span></span>

<span data-ttu-id="49e70-735">Token parowania jest używany w celu zagwarantowania, że żądania odbierane przez Kestrel zostały przekazane przez usługi IIS i nie pochodzą z innego źródła.</span><span class="sxs-lookup"><span data-stu-id="49e70-735">A pairing token is used to guarantee that the requests received by Kestrel were proxied by IIS and didn't come from some other source.</span></span> <span data-ttu-id="49e70-736">Token parowania jest tworzony i ustawiany jako zmienna środowiskowa ( `ASPNETCORE_TOKEN` ) przez moduł.</span><span class="sxs-lookup"><span data-stu-id="49e70-736">The pairing token is created and set into an environment variable (`ASPNETCORE_TOKEN`) by the module.</span></span> <span data-ttu-id="49e70-737">Token parowania jest również ustawiany w nagłówku ( `MS-ASPNETCORE-TOKEN` ) na każdym żądaniu z serwerem proxy.</span><span class="sxs-lookup"><span data-stu-id="49e70-737">The pairing token is also set into a header (`MS-ASPNETCORE-TOKEN`) on every proxied request.</span></span> <span data-ttu-id="49e70-738">Oprogramowanie pośredniczące usług IIS sprawdza każde odebrane żądanie, aby potwierdzić, że wartość nagłówka tokenu parowania jest zgodna z wartością zmiennej środowiskowej.</span><span class="sxs-lookup"><span data-stu-id="49e70-738">IIS Middleware checks each request it receives to confirm that the pairing token header value matches the environment variable value.</span></span> <span data-ttu-id="49e70-739">Jeśli wartości tokenu są niezgodne, żądanie zostanie zarejestrowane i odrzucone.</span><span class="sxs-lookup"><span data-stu-id="49e70-739">If the token values are mismatched, the request is logged and rejected.</span></span> <span data-ttu-id="49e70-740">Zmienna środowiskowa tokena parowania i ruch między modułem i Kestrel nie są dostępne z lokalizacji poza serwerem.</span><span class="sxs-lookup"><span data-stu-id="49e70-740">The pairing token environment variable and the traffic between the module and Kestrel aren't accessible from a location off of the server.</span></span> <span data-ttu-id="49e70-741">Bez znajomości wartości tokenu parowania osoba atakująca nie może przesłać żądań, które pomijają Ewidencjonowanie oprogramowania pośredniczącego usług IIS.</span><span class="sxs-lookup"><span data-stu-id="49e70-741">Without knowing the pairing token value, an attacker can't submit requests that bypass the check in the IIS Middleware.</span></span>

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="49e70-742">Moduł ASP.NET Core z konfiguracją udostępnioną usług IIS</span><span class="sxs-lookup"><span data-stu-id="49e70-742">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="49e70-743">Instalator modułu ASP.NET Core jest uruchamiany z uprawnieniami konta **TrustedInstaller** .</span><span class="sxs-lookup"><span data-stu-id="49e70-743">The ASP.NET Core Module installer runs with the privileges of the **TrustedInstaller** account.</span></span> <span data-ttu-id="49e70-744">Ponieważ konto systemu lokalnego nie ma uprawnień do modyfikowania dla ścieżki udziału używanej przez udostępnioną konfigurację usług IIS, Instalator zgłasza błąd odmowy dostępu podczas próby skonfigurowania ustawień modułu w pliku *applicationHost.config* w udziale.</span><span class="sxs-lookup"><span data-stu-id="49e70-744">Because the local system account doesn't have modify permission for the share path used by the IIS Shared Configuration, the installer throws an access denied error when attempting to configure the module settings in the *applicationHost.config* file on the share.</span></span>

<span data-ttu-id="49e70-745">W przypadku korzystania z konfiguracji udostępnionej usług IIS wykonaj następujące kroki:</span><span class="sxs-lookup"><span data-stu-id="49e70-745">When using an IIS Shared Configuration, follow these steps:</span></span>

1. <span data-ttu-id="49e70-746">Wyłącz konfigurację udostępnioną usług IIS.</span><span class="sxs-lookup"><span data-stu-id="49e70-746">Disable the IIS Shared Configuration.</span></span>
1. <span data-ttu-id="49e70-747">Uruchom instalatora.</span><span class="sxs-lookup"><span data-stu-id="49e70-747">Run the installer.</span></span>
1. <span data-ttu-id="49e70-748">Wyeksportuj zaktualizowany plik *applicationHost.config* do udziału.</span><span class="sxs-lookup"><span data-stu-id="49e70-748">Export the updated *applicationHost.config* file to the share.</span></span>
1. <span data-ttu-id="49e70-749">Włącz ponownie konfigurację udostępnioną usług IIS.</span><span class="sxs-lookup"><span data-stu-id="49e70-749">Re-enable the IIS Shared Configuration.</span></span>

## <a name="module-version-and-hosting-bundle-installer-logs"></a><span data-ttu-id="49e70-750">Dzienniki instalacji pakietu i pakietów hostingu</span><span class="sxs-lookup"><span data-stu-id="49e70-750">Module version and Hosting Bundle installer logs</span></span>

<span data-ttu-id="49e70-751">Aby określić wersję zainstalowanego modułu ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="49e70-751">To determine the version of the installed ASP.NET Core Module:</span></span>

1. <span data-ttu-id="49e70-752">W systemie hostingu przejdź do *%windir%\System32\inetsrv*.</span><span class="sxs-lookup"><span data-stu-id="49e70-752">On the hosting system, navigate to *%windir%\System32\inetsrv*.</span></span>
1. <span data-ttu-id="49e70-753">Znajdź plik *aspnetcore.dll* .</span><span class="sxs-lookup"><span data-stu-id="49e70-753">Locate the *aspnetcore.dll* file.</span></span>
1. <span data-ttu-id="49e70-754">Kliknij prawym przyciskiem myszy plik i wybierz polecenie **Właściwości** z menu kontekstowego.</span><span class="sxs-lookup"><span data-stu-id="49e70-754">Right-click the file and select **Properties** from the contextual menu.</span></span>
1. <span data-ttu-id="49e70-755">Wybierz kartę **szczegóły** . **Wersja pliku** i **Wersja produktu** reprezentują zainstalowaną wersję modułu.</span><span class="sxs-lookup"><span data-stu-id="49e70-755">Select the **Details** tab. The **File version** and **Product version** represent the installed version of the module.</span></span>

<span data-ttu-id="49e70-756">Dzienniki Instalatora pakietu hostingu dla modułu znajdują się pod adresem *C: \\ Użytkownicy \\ % username% \\ AppData \\ Local \\ temp*. Plik ma nazwę *dd_DotNetCoreWinSvrHosting__ \<timestamp> _000_AspNetCoreModule_x64. log*.</span><span class="sxs-lookup"><span data-stu-id="49e70-756">The Hosting Bundle installer logs for the module are found at *C:\\Users\\%UserName%\\AppData\\Local\\Temp*. The file is named *dd_DotNetCoreWinSvrHosting__\<timestamp>_000_AspNetCoreModule_x64.log*.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="49e70-757">Lokalizacje pliku modułu, schematu i konfiguracji</span><span class="sxs-lookup"><span data-stu-id="49e70-757">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="49e70-758">Moduł</span><span class="sxs-lookup"><span data-stu-id="49e70-758">Module</span></span>

<span data-ttu-id="49e70-759">**IIS (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="49e70-759">**IIS (x86/amd64):**</span></span>

* <span data-ttu-id="49e70-760">% windir% \System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="49e70-760">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

* <span data-ttu-id="49e70-761">% windir% \SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="49e70-761">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="49e70-762">**IIS Express (x86/amd64):**</span><span class="sxs-lookup"><span data-stu-id="49e70-762">**IIS Express (x86/amd64):**</span></span>

* <span data-ttu-id="49e70-763">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="49e70-763">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

* <span data-ttu-id="49e70-764">% ProgramFiles (x86)% \ IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="49e70-764">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="49e70-765">Schemat</span><span class="sxs-lookup"><span data-stu-id="49e70-765">Schema</span></span>

<span data-ttu-id="49e70-766">**IIS**</span><span class="sxs-lookup"><span data-stu-id="49e70-766">**IIS**</span></span>

* <span data-ttu-id="49e70-767">% windir% \System32\inetsrv\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="49e70-767">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="49e70-768">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="49e70-768">**IIS Express**</span></span>

* <span data-ttu-id="49e70-769">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="49e70-769">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="49e70-770">Konfigurowanie</span><span class="sxs-lookup"><span data-stu-id="49e70-770">Configuration</span></span>

<span data-ttu-id="49e70-771">**IIS**</span><span class="sxs-lookup"><span data-stu-id="49e70-771">**IIS**</span></span>

* <span data-ttu-id="49e70-772">% windir% \System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="49e70-772">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="49e70-773">**IIS Express**</span><span class="sxs-lookup"><span data-stu-id="49e70-773">**IIS Express**</span></span>

* <span data-ttu-id="49e70-774">Visual Studio: {Aplikacja główna} \\.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="49e70-774">Visual Studio: {APPLICATION ROOT}\\.vs\config\applicationHost.config</span></span>

* <span data-ttu-id="49e70-775">*iisexpress.exe* INTERFEJS WIERSZA POLECENIA:% USERPROFILE% \Documents\IISExpress\config\applicationhost.config</span><span class="sxs-lookup"><span data-stu-id="49e70-775">*iisexpress.exe* CLI: %USERPROFILE%\Documents\IISExpress\config\applicationhost.config</span></span>

<span data-ttu-id="49e70-776">Pliki można znaleźć, wyszukując *aspnetcore* w pliku *applicationHost.config* .</span><span class="sxs-lookup"><span data-stu-id="49e70-776">The files can be found by searching for *aspnetcore* in the *applicationHost.config* file.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="49e70-777">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="49e70-777">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <span data-ttu-id="49e70-778">[Źródło odwołania do modułu ASP.NET Core [gałąź domyślna (główna)]](https://github.com/dotnet/aspnetcore/tree/main/src/Servers/IIS/AspNetCoreModuleV2): Użyj listy rozwijanej **rozgałęzienie** , aby wybrać konkretną wersję (na przykład `release/3.1` ).</span><span class="sxs-lookup"><span data-stu-id="49e70-778">[ASP.NET Core Module reference source [default branch (master)]](https://github.com/dotnet/aspnetcore/tree/main/src/Servers/IIS/AspNetCoreModuleV2): Use the **Branch** drop down list to select a specific release (for example, `release/3.1`).</span></span>
* <xref:host-and-deploy/iis/modules>
