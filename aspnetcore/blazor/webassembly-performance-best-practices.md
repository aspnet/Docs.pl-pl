---
title: ASP.NET Core Blazor najlepszych rozwiązań dotyczących wydajności zestawu Webassembly
author: pranavkm
description: Wskazówki dotyczące zwiększania wydajności w ASP.NET Core Blazor aplikacjach webassembly i unikania typowych problemów z wydajnością.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/08/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/webassembly-performance-best-practices
ms.openlocfilehash: 2b6d4e706856cb28f26c2502feca4f959ca4abac
ms.sourcegitcommit: 066d66ea150f8aab63f9e0e0668b06c9426296fd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243034"
---
# <a name="aspnet-core-blazor-webassembly-performance-best-practices"></a>ASP.NET Core Blazor najlepszych rozwiązań dotyczących wydajności zestawu Webassembly

Autor [Pranav Krishnamoorthy](https://github.com/pranavkm)

Ten artykuł zawiera wskazówki dotyczące ASP.NET Core najlepszych rozwiązań w zakresie Blazor wydajności zestawu webassembly.

## <a name="avoid-unnecessary-component-renders"></a>Unikaj renderowania zbędnych składników

Blazoralgorytm różnicowania unika odrenderowania składnika, gdy algorytm postrzega, że składnik nie został zmieniony. Przesłoń dla szczegółowej <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A?displayProperty=nameWithType> kontroli nad renderowaniem składników.

W przypadku tworzenia składnika tylko interfejsu użytkownika, który nigdy nie zmienia się po początkowym renderowaniu, skonfiguruj <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> do zwrócenia `false` :

```razor
@code {
    protected override bool ShouldRender() => false;
}
```

Większość aplikacji nie wymaga precyzyjnej kontroli, ale <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> może być również używana do selektywnego renderowania składnika odpowiadającego zdarzeniu interfejsu użytkownika.

W poniższym przykładzie:

* <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A>jest zastępowany i ustawiany na wartość <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> pola, początkowo `false` podczas ładowania składnika.
* Gdy przycisk jest zaznaczony, <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> jest ustawiony na `true` , co wymusza ponowną renderowanie składnika przy użyciu zaktualizowanego `currentCount` .
* Natychmiast po <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> ponownym renderowaniu ustawia wartość z powrotem na, aby zapobiec dalszemu ponownemu przywracaniu do <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> `false` momentu, gdy przycisk zostanie wybrany.

```razor
<p>Current count: @currentCount</p>

<button @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;
    private bool shouldRender;

    protected override bool ShouldRender() => shouldRender;

    protected override void OnAfterRender(bool first)
    {
        shouldRender = false;
    }

    private void IncrementCount()
    {
        currentCount++;
        shouldRender = true;
    }
}
```

Aby uzyskać więcej informacji, zobacz <xref:blazor/components/lifecycle#after-component-render>.

## <a name="virtualize-re-usable-fragments"></a>Wirtualizacja fragmentów do ponownego użycia

Składniki oferują wygodną metodę tworzenia fragmentów kodu i znaczników do ponownego użycia. Ogólnie rzecz biorąc zalecamy tworzenie pojedynczych składników, które najlepiej dopasowują się do wymagań aplikacji. Jednym z zawartoocią jest to, że każdy dodatkowy składnik podrzędny wnosi do łącznego czasu potrzebnego na renderowanie składnika nadrzędnego. W przypadku większości aplikacji dodatkowe obciążenie jest znikome. W przypadku aplikacji, które tworzą dużą liczbę składników, należy rozważyć użycie strategii w celu zmniejszenia nakładu pracy, np. ograniczenia liczby renderowanych składników.

Na przykład siatka lub lista, która renderuje setki wierszy zawierających składniki, ma intensywną obsługę procesora. Rozważ wirtualizację układu siatki lub listy tak, że tylko podzestaw składników jest renderowany w danym momencie. Aby zapoznać się z przykładem renderowania podzestawu składników, zobacz następujące składniki w [ `Virtualization` aplikacji przykładowej (repozytorium w witrynie GitHub/przykłady)](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Virtualization):

* `Virtualize`składnik ( [`Shared/Virtualize.razor`](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Shared/Virtualize.cs) ): składnik zapisany w języku C#, który implementuje <xref:Microsoft.AspNetCore.Components.ComponentBase> zestaw wierszy danych pogody na podstawie przewijania przez użytkownika.
* `FetchData`składnik ( [`Pages/FetchData.razor`](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Pages/FetchData.razor) ): używa `Virtualize` składnika do wyświetlania 25 wierszy danych pogody jednocześnie.

## <a name="avoid-javascript-interop-to-marshal-data"></a>Unikaj międzyoperacyjności JavaScript do organizowania danych

W elemencie Blazor webassembly wywołanie międzyoperacyjne języka JavaScript (js) musi przejść do granicy webassembly-js. Serializacja i deserializacja zawartości w dwóch kontekstach powoduje utworzenie obciążenia dla aplikacji. Częste wywołania w programie JS Interop często mają negatywny wpływ na wydajność. Aby zmniejszyć kierowanie danych między granicami, ustal, czy aplikacja może skonsolidować wiele małych ładunków do pojedynczego dużego ładunku, aby uniknąć dużej liczby przełączeń kontekstu między zestawem webassembly i JS.

## <a name="use-systemtextjson"></a>Użyj System.Text.Jsna

Blazorimplementacja elementu webinterop w języku JS polega na tym <xref:System.Text.Json> , że jest to biblioteka serializacji JSON o wysokiej wydajności z alokacją małej ilości pamięci. Użycie nie powoduje, że <xref:System.Text.Json> rozmiar ładunku aplikacji jest większy niż dodanie co najmniej jednej alternatywnej biblioteki JSON.

Aby uzyskać wskazówki dotyczące migracji, zobacz [Jak przeprowadzić migrację z `Newtonsoft.Json` do programu `System.Text.Json` ](/dotnet/standard/serialization/system-text-json-migrate-from-newtonsoft-how-to).

## <a name="use-synchronous-and-unmarshalled-js-interop-apis-where-appropriate"></a>Używaj synchronicznych i nieskierowanych interfejsów API międzyoperacyjności w razie potrzeby

BlazorZestaw webassembly oferuje dwie dodatkowe wersje programu <xref:Microsoft.JSInterop.IJSRuntime> niż jedna wersja dostępna dla Blazor aplikacji serwerowych:

* <xref:Microsoft.JSInterop.IJSInProcessRuntime>umożliwia Asynchroniczne wywoływanie wywołań programu JS Interop, które ma mniej obciążenia niż wersje asynchroniczne:

  ```razor
  @inject IJSRuntime JS

  @code {
      protected override void OnInitialized()
      {
          var jsInProcess = (IJSInProcessRuntime)JS;

          var value = jsInProcess.Invoke<string>("jsInteropCall");
      }
  }
  ```

* <xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime>zezwala na nieskierowanie wywołań międzyoperacyjnych JS:

  ```javascript
  function jsInteropCall() {
    return BINDING.js_to_mono_obj("Hello world");
  }
  ```

  ```razor
  @inject IJSRuntime JS

  @code {
      protected override void OnInitialized()
      {
          var jsInProcess = (WebAssemblyJSRuntime)JS;

          var value = jsInProcess.InvokeUnmarshalled<string>("jsInteropCall");
      }
  }
  ```

  > [!WARNING]
  > Podczas używania <xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime> ma najmniejsze obciążenie związane z interfejsem js międzyoperacyjności, interfejsy API języka JavaScript wymagane do współdziałania z tymi interfejsy API są obecnie nieudokumentowane i mogą ulegać zmianom w przyszłych wersjach.

## <a name="reduce-app-size"></a>Zmniejsz rozmiar aplikacji

### <a name="intermediate-language-il-linking"></a>Tworzenie łączy języka pośredniego (IL)

[Łączenie z Blazor Aplikacja webassembly](xref:blazor/host-and-deploy/configure-linker) zmniejsza rozmiar aplikacji przez przycinanie nieużywanego kodu w plikach binarnych aplikacji. Domyślnie konsolidator jest włączony tylko w przypadku kompilowania w `Release` konfiguracji. Aby z tego skorzystać, Opublikuj aplikację do wdrożenia przy użyciu [`dotnet publish`](/dotnet/core/tools/dotnet-publish) polecenia z opcją [-c |--Configuration](/dotnet/core/tools/dotnet-publish#options) ustawioną na `Release` :

```dotnetcli
dotnet publish -c Release
```

### <a name="compression"></a>Kompresja

Po Blazor opublikowaniu aplikacji webassembly dane wyjściowe są kompresowane statycznie podczas publikowania, aby zmniejszyć rozmiar aplikacji i usunąć obciążenie dla kompresji w czasie wykonywania. Blazoropiera się na serwerze w celu przeprowadzenia negotation zawartości i obkompresji plików skompresowanych statycznie.

Po wdrożeniu aplikacji Sprawdź, czy aplikacja obsługuje skompresowane pliki. Zbadaj kartę Sieć w Narzędzia deweloperskie przeglądarki i sprawdź, czy pliki są obsługiwane przez program `Content-Encoding: br` lub `Content-Encoding: gz` . Jeśli host nie obsługuje skompresowanych plików, postępuj zgodnie z instrukcjami w temacie <xref:blazor/host-and-deploy/webassembly#compression> .

### <a name="disable-unused-features"></a>Wyłącz nieużywane funkcje

BlazorŚrodowisko uruchomieniowe webassembly obejmuje następujące funkcje platformy .NET, które można wyłączyć, jeśli aplikacja nie wymaga ich dla mniejszego rozmiaru ładunku:

* Plik danych jest uwzględniany w celu poprawnego wprowadzania informacji o strefie czasowej. Jeśli aplikacja nie wymaga tej funkcji, rozważ wyłączenie jej przez ustawienie właściwości programu `BlazorEnableTimeZoneSupport` MSBuild w pliku projektu aplikacji na `false` :

  ```xml
  <PropertyGroup>
    <BlazorEnableTimeZoneSupport>false</BlazorEnableTimeZoneSupport>
  </PropertyGroup>
  ```

* Informacje o sortowaniu są uwzględniane w celu <xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> poprawnego działania interfejsów API. Jeśli masz pewność, że aplikacja nie wymaga danych sortowania, rozważ wyłączenie jej przez ustawienie `BlazorWebAssemblyPreserveCollationData` Właściwości programu MSBuild w pliku projektu aplikacji na `false` :

  ```xml
  <PropertyGroup>
    <BlazorWebAssemblyPreserveCollationData>false</BlazorWebAssemblyPreserveCollationData>
  </PropertyGroup>
  ```