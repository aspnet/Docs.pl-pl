---
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
ms.openlocfilehash: 1b045d437d1a16eabc0ab41573c8b66d9c4bb77e
ms.sourcegitcommit: a49c47d5a573379effee5c6b6e36f5c302aa756b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/16/2021
ms.locfileid: "100552380"
---
<span data-ttu-id="3520b-101">Domyślnie zostanie wyświetlona wartość domyślna tryb logowania w trybie przekierowania i powrót do trybu logowania przekierowywania, jeśli nie można otworzyć okna podręcznego.</span><span class="sxs-lookup"><span data-stu-id="3520b-101">The framework defaults to pop-up login mode and falls back to redirect login mode if a pop-up can't be opened.</span></span> <span data-ttu-id="3520b-102">Skonfiguruj MSAL do używania trybu logowania przekierowania, ustawiając `LoginMode` Właściwość <xref:Microsoft.Authentication.WebAssembly.Msal.Models.MsalProviderOptions> na `redirect` :</span><span class="sxs-lookup"><span data-stu-id="3520b-102">Configure MSAL to use redirect login mode by setting the `LoginMode` property of <xref:Microsoft.Authentication.WebAssembly.Msal.Models.MsalProviderOptions> to `redirect`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.LoginMode = "redirect";
});
```

<span data-ttu-id="3520b-103">Ustawieniem domyślnym jest `popup` , a w wartości ciągu nie jest rozróżniana wielkość liter.</span><span class="sxs-lookup"><span data-stu-id="3520b-103">The default setting is `popup`, and the string value isn't case sensitive.</span></span>