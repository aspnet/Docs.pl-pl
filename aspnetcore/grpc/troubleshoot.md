---
title: Rozwiązywanie problemów z gRPC na platformie .NET Core
author: jamesnk
description: Rozwiązywanie problemów dotyczących błędów podczas korzystania z gRPC na platformie .NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 07/09/2020
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
uid: grpc/troubleshoot
ms.openlocfilehash: 1fd89059183300993c7fa78aa8dab1bda247a530
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102586985"
---
# <a name="troubleshoot-grpc-on-net-core"></a><span data-ttu-id="69150-103">Rozwiązywanie problemów z gRPC na platformie .NET Core</span><span class="sxs-lookup"><span data-stu-id="69150-103">Troubleshoot gRPC on .NET Core</span></span>

<span data-ttu-id="69150-104">Przez [Kuba Kowalski-króla](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="69150-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="69150-105">W tym dokumencie omówiono często występujące problemy podczas tworzenia aplikacji gRPC na platformie .NET.</span><span class="sxs-lookup"><span data-stu-id="69150-105">This document discusses commonly encountered problems when developing gRPC apps on .NET.</span></span>

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a><span data-ttu-id="69150-106">Niezgodność między konfiguracją protokołu SSL klienta i usługi TLS</span><span class="sxs-lookup"><span data-stu-id="69150-106">Mismatch between client and service SSL/TLS configuration</span></span>

<span data-ttu-id="69150-107">Szablon gRPC i przykłady używają usługi [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) do domyślnego zabezpieczania usług gRPC.</span><span class="sxs-lookup"><span data-stu-id="69150-107">The gRPC template and samples use [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) to secure gRPC services by default.</span></span> <span data-ttu-id="69150-108">Klienci gRPC muszą używać bezpiecznego połączenia w celu pomyślnego wywołania zabezpieczonych usług gRPC.</span><span class="sxs-lookup"><span data-stu-id="69150-108">gRPC clients need to use a secure connection to call secured gRPC services successfully.</span></span>

<span data-ttu-id="69150-109">Możesz sprawdzić, ASP.NET Core usługa gRPC korzysta z protokołu TLS w dziennikach, które zostały zapisane podczas uruchamiania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="69150-109">You can verify the ASP.NET Core gRPC service is using TLS in the logs written on app start.</span></span> <span data-ttu-id="69150-110">Usługa nasłuchuje na punkcie końcowym HTTPS:</span><span class="sxs-lookup"><span data-stu-id="69150-110">The service will be listening on an HTTPS endpoint:</span></span>

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

<span data-ttu-id="69150-111">Aby umożliwić nawiązywanie połączeń z bezpiecznym połączeniem, klient .NET Core musi użyć `https` adresu serwera:</span><span class="sxs-lookup"><span data-stu-id="69150-111">The .NET Core client must use `https` in the server address to make calls with a secured connection:</span></span>

```csharp
static async Task Main(string[] args)
{
    // The port number(5001) must match the port of the gRPC server.
    var channel = GrpcChannel.ForAddress("https://localhost:5001");
    var client = new Greet.GreeterClient(channel);
}
```

<span data-ttu-id="69150-112">Wszystkie implementacje klienta gRPC obsługują protokół TLS.</span><span class="sxs-lookup"><span data-stu-id="69150-112">All gRPC client implementations support TLS.</span></span> <span data-ttu-id="69150-113">Klienci gRPC z innych języków zwykle wymagają kanału skonfigurowanego z `SslCredentials` .</span><span class="sxs-lookup"><span data-stu-id="69150-113">gRPC clients from other languages typically require the channel configured with `SslCredentials`.</span></span> <span data-ttu-id="69150-114">`SslCredentials` Określa certyfikat, który będzie używany przez klienta i musi być używany zamiast niezabezpieczonych poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="69150-114">`SslCredentials` specifies the certificate that the client will use, and it must be used instead of insecure credentials.</span></span> <span data-ttu-id="69150-115">Przykłady konfigurowania różnych implementacji klienta gRPC do korzystania z protokołu TLS można znaleźć w temacie [GRPC Authentication (uwierzytelnianie](https://www.grpc.io/docs/guides/auth/)).</span><span class="sxs-lookup"><span data-stu-id="69150-115">For examples of configuring the different gRPC client implementations to use TLS, see [gRPC Authentication](https://www.grpc.io/docs/guides/auth/).</span></span>

## <a name="call-a-grpc-service-with-an-untrustedinvalid-certificate"></a><span data-ttu-id="69150-116">Wywołaj usługę gRPC z niezaufanym/nieprawidłowym certyfikatem</span><span class="sxs-lookup"><span data-stu-id="69150-116">Call a gRPC service with an untrusted/invalid certificate</span></span>

<span data-ttu-id="69150-117">Klient .NET gRPC wymaga, aby usługa miała zaufany certyfikat.</span><span class="sxs-lookup"><span data-stu-id="69150-117">The .NET gRPC client requires the service to have a trusted certificate.</span></span> <span data-ttu-id="69150-118">Podczas wywoływania usługi gRPC bez zaufanego certyfikatu zwracany jest następujący komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="69150-118">The following error message is returned when calling a gRPC service without a trusted certificate:</span></span>

> <span data-ttu-id="69150-119">Nieobsługiwany wyjątek.</span><span class="sxs-lookup"><span data-stu-id="69150-119">Unhandled exception.</span></span> <span data-ttu-id="69150-120">System .NET. http. HttpRequestexception: nie można nawiązać połączenia SSL, zobacz wyjątek wewnętrzny.</span><span class="sxs-lookup"><span data-stu-id="69150-120">System.Net.Http.HttpRequestException: The SSL connection could not be established, see inner exception.</span></span>
> <span data-ttu-id="69150-121">---> system. Security. Authentication. AuthenticationException: certyfikat zdalny jest nieprawidłowy zgodnie z procedurą walidacji.</span><span class="sxs-lookup"><span data-stu-id="69150-121">---> System.Security.Authentication.AuthenticationException: The remote certificate is invalid according to the validation procedure.</span></span>

<span data-ttu-id="69150-122">Ten błąd może pojawić się, jeśli testujesz aplikację lokalnie, a ASP.NET Core certyfikat programistyczny HTTPS nie jest zaufany.</span><span class="sxs-lookup"><span data-stu-id="69150-122">You may see this error if you are testing your app locally and the ASP.NET Core HTTPS development certificate is not trusted.</span></span> <span data-ttu-id="69150-123">Aby uzyskać instrukcje dotyczące rozwiązania tego problemu, zobacz temat [ASP.NET Core ufanie certyfikatowi Deweloperskiemu protokołu HTTPS w systemie Windows i macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span><span class="sxs-lookup"><span data-stu-id="69150-123">For instructions to fix this issue, see [Trust the ASP.NET Core HTTPS development certificate on Windows and macOS](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos).</span></span>

<span data-ttu-id="69150-124">Jeśli wywołujesz usługę gRPC na innym komputerze i nie można ufać certyfikatowi, można skonfigurować klienta gRPC do ignorowania nieprawidłowego certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="69150-124">If you are calling a gRPC service on another machine and are unable to trust the certificate then the gRPC client can be configured to ignore the invalid certificate.</span></span> <span data-ttu-id="69150-125">Poniższy kod używa metody [HttpClientHandler. ServerCertificateCustomValidationCallback](/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback) , aby zezwalać na wywołania bez zaufanego certyfikatu:</span><span class="sxs-lookup"><span data-stu-id="69150-125">The following code uses [HttpClientHandler.ServerCertificateCustomValidationCallback](/dotnet/api/system.net.http.httpclienthandler.servercertificatecustomvalidationcallback) to allow calls without a trusted certificate:</span></span>

```csharp
var httpHandler = new HttpClientHandler();
// Return `true` to allow certificates that are untrusted/invalid
httpHandler.ServerCertificateCustomValidationCallback = 
    HttpClientHandler.DangerousAcceptAnyServerCertificateValidator;

var channel = GrpcChannel.ForAddress("https://localhost:5001",
    new GrpcChannelOptions { HttpHandler = httpHandler });
var client = new Greet.GreeterClient(channel);
```

> [!WARNING]
> <span data-ttu-id="69150-126">Niezaufanych certyfikatów należy używać tylko podczas opracowywania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="69150-126">Untrusted certificates should only be used during app development.</span></span> <span data-ttu-id="69150-127">Aplikacje produkcyjne powinny zawsze używać prawidłowych certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="69150-127">Production apps should always use valid certificates.</span></span>

## <a name="call-insecure-grpc-services-with-net-core-client"></a><span data-ttu-id="69150-128">Wywoływanie niezabezpieczonych usług gRPC z klientem .NET Core</span><span class="sxs-lookup"><span data-stu-id="69150-128">Call insecure gRPC services with .NET Core client</span></span>

<span data-ttu-id="69150-129">Klient .NET gRPC może wywoływać niezabezpieczone usługi gRPC przez specifing `http` w adresie serwera.</span><span class="sxs-lookup"><span data-stu-id="69150-129">The .NET gRPC client can call insecure gRPC services by specifing `http` in the server address.</span></span> <span data-ttu-id="69150-130">Na przykład `GrpcChannel.ForAddress("http://localhost:5000")`.</span><span class="sxs-lookup"><span data-stu-id="69150-130">For example, `GrpcChannel.ForAddress("http://localhost:5000")`.</span></span>

<span data-ttu-id="69150-131">Istnieją pewne dodatkowe wymagania dotyczące wywoływania niezabezpieczonych usług gRPC w zależności od wersji platformy .NET używanej przez aplikację:</span><span class="sxs-lookup"><span data-stu-id="69150-131">There are some additional requirements to call insecure gRPC services depending on the .NET version an app is using:</span></span>

* <span data-ttu-id="69150-132">Program .NET 5 lub nowszy wymaga [GRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client) w wersji 2.32.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="69150-132">.NET 5 or later requires [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) version 2.32.0 or later.</span></span>
* <span data-ttu-id="69150-133">Program .NET Core 3. x wymaga dodatkowej konfiguracji.</span><span class="sxs-lookup"><span data-stu-id="69150-133">.NET Core 3.x requires additional configuration.</span></span> <span data-ttu-id="69150-134">Aplikacja musi ustawić `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` przełącznik na `true` :</span><span class="sxs-lookup"><span data-stu-id="69150-134">The app must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true`:</span></span>
    
    ```csharp
    // This switch must be set before creating the GrpcChannel/HttpClient.
    AppContext.SetSwitch(
        "System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);
    
    // The port number(5000) must match the port of the gRPC server.
    var channel = GrpcChannel.ForAddress("http://localhost:5000");
    var client = new Greet.GreeterClient(channel);
    ```

<span data-ttu-id="69150-135">`System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport`Przełącznik jest wymagany tylko w przypadku programu .NET Core 3. x.</span><span class="sxs-lookup"><span data-stu-id="69150-135">The `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch is only required for .NET Core 3.x.</span></span> <span data-ttu-id="69150-136">Nic nie działa w programie .NET 5 i nie jest wymagane.</span><span class="sxs-lookup"><span data-stu-id="69150-136">It does nothing in .NET 5 and isn't required.</span></span>

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a><span data-ttu-id="69150-137">Nie można uruchomić aplikacji ASP.NET Core gRPC na macOS</span><span class="sxs-lookup"><span data-stu-id="69150-137">Unable to start ASP.NET Core gRPC app on macOS</span></span>

<span data-ttu-id="69150-138">Kestrel nie obsługuje protokołu HTTP/2 z protokołem TLS w macOS i starszych wersjach systemu Windows, takich jak Windows 7.</span><span class="sxs-lookup"><span data-stu-id="69150-138">Kestrel doesn't support HTTP/2 with TLS on macOS and older Windows versions such as Windows 7.</span></span> <span data-ttu-id="69150-139">ASP.NET Core szablon i przykłady gRPC domyślnie korzystają z protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="69150-139">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="69150-140">Podczas próby uruchomienia serwera gRPC zobaczysz następujący komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="69150-140">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="69150-141">Nie można utworzyć powiązania z https://localhost:5001 interfejsem sprzężenia zwrotnego IPv4: "http/2 za pośrednictwem protokołu TLS nie jest obsługiwane w macOS ze względu na brak pomocy technicznej ClientHello alpn".</span><span class="sxs-lookup"><span data-stu-id="69150-141">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="69150-142">Aby obejść ten problem, należy skonfigurować Kestrel i klienta gRPC do używania protokołu HTTP/2 *bez* szyfrowania TLS.</span><span class="sxs-lookup"><span data-stu-id="69150-142">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 *without* TLS.</span></span> <span data-ttu-id="69150-143">Należy to zrobić tylko podczas projektowania.</span><span class="sxs-lookup"><span data-stu-id="69150-143">You should only do this during development.</span></span> <span data-ttu-id="69150-144">Użycie protokołu TLS spowoduje, że komunikaty gRPC są wysyłane bez szyfrowania.</span><span class="sxs-lookup"><span data-stu-id="69150-144">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="69150-145">Kestrel musi skonfigurować punkt końcowy HTTP/2 bez protokołu TLS w *program.cs*:</span><span class="sxs-lookup"><span data-stu-id="69150-145">Kestrel must configure an HTTP/2 endpoint without TLS in *Program.cs*:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = 
                    HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

::: moniker range=">= aspnetcore-5.0"
<span data-ttu-id="69150-146">Gdy punkt końcowy protokołu HTTP/2 jest skonfigurowany bez protokołu TLS, [ListenOptions. protokoły](xref:fundamentals/servers/kestrel/endpoints#listenoptionsprotocols) punktu końcowego muszą mieć ustawioną wartość `HttpProtocols.Http2` .</span><span class="sxs-lookup"><span data-stu-id="69150-146">When an HTTP/2 endpoint is configured without TLS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel/endpoints#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="69150-147">`HttpProtocols.Http1AndHttp2` nie można użyć, ponieważ protokół TLS jest wymagany do negocjowania protokołu HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="69150-147">`HttpProtocols.Http1AndHttp2` can't be used because TLS is required to negotiate HTTP/2.</span></span> <span data-ttu-id="69150-148">Bez protokołu TLS wszystkie połączenia z punktem końcowym domyślne do protokołu HTTP/1.1 i wywołania gRPC kończą się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="69150-148">Without TLS, all connections to the endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>
::: moniker-end

::: moniker range="< aspnetcore-5.0"
<span data-ttu-id="69150-149">Gdy punkt końcowy protokołu HTTP/2 jest skonfigurowany bez protokołu TLS, [ListenOptions. protokoły](xref:fundamentals/servers/kestrel#listenoptionsprotocols) punktu końcowego muszą mieć ustawioną wartość `HttpProtocols.Http2` .</span><span class="sxs-lookup"><span data-stu-id="69150-149">When an HTTP/2 endpoint is configured without TLS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="69150-150">`HttpProtocols.Http1AndHttp2` nie można użyć, ponieważ protokół TLS jest wymagany do negocjowania protokołu HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="69150-150">`HttpProtocols.Http1AndHttp2` can't be used because TLS is required to negotiate HTTP/2.</span></span> <span data-ttu-id="69150-151">Bez protokołu TLS wszystkie połączenia z punktem końcowym domyślne do protokołu HTTP/1.1 i wywołania gRPC kończą się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="69150-151">Without TLS, all connections to the endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>
::: moniker-end

<span data-ttu-id="69150-152">Klienta gRPC należy również skonfigurować tak, aby nie korzystał z protokołu TLS.</span><span class="sxs-lookup"><span data-stu-id="69150-152">The gRPC client must also be configured to not use TLS.</span></span> <span data-ttu-id="69150-153">Aby uzyskać więcej informacji, zobacz [wywoływanie niezabezpieczonych usług gRPC przy użyciu programu .NET Core Client](#call-insecure-grpc-services-with-net-core-client).</span><span class="sxs-lookup"><span data-stu-id="69150-153">For more information, see [Call insecure gRPC services with .NET Core client](#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!WARNING]
> <span data-ttu-id="69150-154">Protokołu HTTP/2 bez protokołu TLS należy używać tylko podczas opracowywania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="69150-154">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="69150-155">Aplikacje produkcyjne powinny zawsze korzystać z zabezpieczeń transportu.</span><span class="sxs-lookup"><span data-stu-id="69150-155">Production apps should always use transport security.</span></span> <span data-ttu-id="69150-156">Aby uzyskać więcej informacji, zobacz [zagadnienia dotyczące zabezpieczeń w programie gRPC ASP.NET Core](xref:grpc/security#transport-security).</span><span class="sxs-lookup"><span data-stu-id="69150-156">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a><span data-ttu-id="69150-157">elementy gRPC języka C# nie są generowane w kodzie z plików. proto</span><span class="sxs-lookup"><span data-stu-id="69150-157">gRPC C# assets are not code generated from .proto files</span></span>

<span data-ttu-id="69150-158">gRPC generowanie kodu dla konkretnych klientów i klas podstawowych usług wymaga przywoływania plików protobuf i narzędzi z projektu.</span><span class="sxs-lookup"><span data-stu-id="69150-158">gRPC code generation of concrete clients and service base classes requires protobuf files and tooling to be referenced from a project.</span></span> <span data-ttu-id="69150-159">Należy uwzględnić:</span><span class="sxs-lookup"><span data-stu-id="69150-159">You must include:</span></span>

* <span data-ttu-id="69150-160">pliki *. proto* , które mają być używane w `<Protobuf>` grupie elementów.</span><span class="sxs-lookup"><span data-stu-id="69150-160">*.proto* files you want to use in the `<Protobuf>` item group.</span></span> <span data-ttu-id="69150-161">[Zaimportowane pliki *proto*](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) muszą odwoływać się do projektu.</span><span class="sxs-lookup"><span data-stu-id="69150-161">[Imported *.proto* files](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) must be referenced by the project.</span></span>
* <span data-ttu-id="69150-162">Odwołanie do pakietu dla pakietu narzędzi gRPC [gRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).</span><span class="sxs-lookup"><span data-stu-id="69150-162">Package reference to the gRPC tooling package [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/).</span></span>

<span data-ttu-id="69150-163">Aby uzyskać więcej informacji na temat generowania zasobów gRPC C#, zobacz <xref:grpc/basics> .</span><span class="sxs-lookup"><span data-stu-id="69150-163">For more information on generating gRPC C# assets, see <xref:grpc/basics>.</span></span>

<span data-ttu-id="69150-164">ASP.NET Core usługi gRPC Web App hosting Services wymagają tylko wygenerowania klasy bazowej usługi:</span><span class="sxs-lookup"><span data-stu-id="69150-164">An ASP.NET Core web app hosting gRPC services only needs the service base class generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

<span data-ttu-id="69150-165">Aplikacja kliencka gRPC wykonująca wywołania gRPC potrzebuje tylko konkretnego wygenerowanego klienta:</span><span class="sxs-lookup"><span data-stu-id="69150-165">A gRPC client app making gRPC calls only needs the concrete client generated:</span></span>

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```

## <a name="wpf-projects-unable-to-generate-grpc-c-assets-from-proto-files"></a><span data-ttu-id="69150-166">Projekty WPF nie mogą generować zasobów gRPC C# z plików. proto</span><span class="sxs-lookup"><span data-stu-id="69150-166">WPF projects unable to generate gRPC C# assets from .proto files</span></span>

<span data-ttu-id="69150-167">Projekty WPF mają [znany problem](https://github.com/dotnet/wpf/issues/810) , który uniemożliwia poprawne działanie generowania kodu gRPC.</span><span class="sxs-lookup"><span data-stu-id="69150-167">WPF projects have a [known issue](https://github.com/dotnet/wpf/issues/810) that prevents gRPC code generation from working correctly.</span></span> <span data-ttu-id="69150-168">Wszystkie typy gRPC wygenerowane w projekcie WPF przez odwołujące się do nich `Grpc.Tools` pliki i *. proto* spowodują błędy kompilacji, gdy są używane:</span><span class="sxs-lookup"><span data-stu-id="69150-168">Any gRPC types generated in a WPF project by referencing `Grpc.Tools` and *.proto* files will create compilation errors when used:</span></span>

> <span data-ttu-id="69150-169">błąd CS0246: nie można znaleźć nazwy typu lub przestrzeni nazw "MyGrpcServices" (czy nie brakuje dyrektywy using lub odwołania do zestawu?)</span><span class="sxs-lookup"><span data-stu-id="69150-169">error CS0246: The type or namespace name 'MyGrpcServices' could not be found (are you missing a using directive or an assembly reference?)</span></span>

<span data-ttu-id="69150-170">Ten problem można obejść, wykonując następujące:</span><span class="sxs-lookup"><span data-stu-id="69150-170">You can workaround this issue by:</span></span>

1. <span data-ttu-id="69150-171">Utwórz nowy projekt biblioteki klas .NET Core.</span><span class="sxs-lookup"><span data-stu-id="69150-171">Create a new .NET Core class library project.</span></span>
2. <span data-ttu-id="69150-172">W nowym projekcie Dodaj odwołania, aby włączyć [generowanie kodu w języku C# z plików *\* . proto*](xref:grpc/basics#generated-c-assets):</span><span class="sxs-lookup"><span data-stu-id="69150-172">In the new project, add references to enable [C# code generation from *\*.proto* files](xref:grpc/basics#generated-c-assets):</span></span>
    * <span data-ttu-id="69150-173">Dodaj odwołanie do pakietu do pakietu [GRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/) .</span><span class="sxs-lookup"><span data-stu-id="69150-173">Add a package reference to [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/) package.</span></span>
    * <span data-ttu-id="69150-174">Dodaj pliki *\* . proto* do `<Protobuf>` grupy elementów.</span><span class="sxs-lookup"><span data-stu-id="69150-174">Add *\*.proto* files to the `<Protobuf>` item group.</span></span>
3. <span data-ttu-id="69150-175">W aplikacji WPF Dodaj odwołanie do nowego projektu.</span><span class="sxs-lookup"><span data-stu-id="69150-175">In the WPF application, add a reference to the new project.</span></span>

<span data-ttu-id="69150-176">Aplikacja WPF może używać typów wygenerowanych przez gRPC z nowego projektu biblioteki klas.</span><span class="sxs-lookup"><span data-stu-id="69150-176">The WPF application can use the gRPC generated types from the new class library project.</span></span>

[!INCLUDE[](~/includes/gRPCazure.md)]
