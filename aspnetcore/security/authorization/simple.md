---
title: Prosta autoryzacja w ASP.NET Core
author: rick-anderson
description: Dowiedz się, w jaki sposób używać atrybutu Autoryzuj, aby ograniczyć dostęp do kontrolerów ASP.NET Core i akcji.
ms.author: riande
ms.date: 10/14/2016
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
uid: security/authorization/simple
ms.openlocfilehash: eaf36822edc47f7238c356e167b05ac44f93175d
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102587570"
---
# <a name="simple-authorization-in-aspnet-core"></a>Prosta autoryzacja w ASP.NET Core

<a name="security-authorization-simple"></a>

Autoryzacja w ASP.NET Core jest kontrolowana przy użyciu <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> i jego różnych parametrów. W najprostszej postaci stosowanie `[Authorize]` atrybutu do kontrolera, akcji lub Razor strony ogranicza dostęp do tego składnika do dowolnego uwierzytelnionego użytkownika.

Na przykład poniższy kod ogranicza dostęp do `AccountController` dowolnego uwierzytelnionego użytkownika.

```csharp
[Authorize]
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

Jeśli chcesz zastosować autoryzację do akcji, a nie kontrolera, Zastosuj `AuthorizeAttribute` atrybut do samej akcji:

```csharp
public class AccountController : Controller
{
   public ActionResult Login()
   {
   }

   [Authorize]
   public ActionResult Logout()
   {
   }
}
```

Teraz tylko uwierzytelnieni użytkownicy mogą uzyskiwać dostęp do `Logout` funkcji.

Można również użyć atrybutu, `AllowAnonymous` Aby zezwolić na dostęp nieuwierzytelnionym użytkownikom do poszczególnych akcji. Na przykład:

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

Może to umożliwić tylko uwierzytelnionym użytkownikom `AccountController` , z wyjątkiem `Login` akcji, która jest dostępna dla wszystkich, niezależnie od ich uwierzytelnionego lub nieuwierzytelnionego/anonimowego stanu.

> [!WARNING]
> `[AllowAnonymous]` pomija wszystkie instrukcje autoryzacji. W przypadku łączenia `[AllowAnonymous]` i dowolnych atrybutów `[Authorize]` `[Authorize]` atrybuty zostaną zignorowane. Na przykład w przypadku zastosowania `[AllowAnonymous]` na poziomie kontrolera wszystkie `[Authorize]` atrybuty na tym samym kontrolerze (lub na dowolnej akcji w ramach tego elementu) zostaną zignorowane.

[!INCLUDE[](~/includes/requireAuth.md)]

<a name="aarp"></a>

## <a name="authorize-attribute-and-razor-pages"></a>Autoryzuj atrybut i Razor strony

<xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> ***Nie*** można zastosować do Razor obsługi stron. Na przykład `[Authorize]` nie można zastosować do `OnGet` `OnPost` programu obsługi stron ani żadnej z nich. Rozważ użycie kontrolera ASP.NET Core MVC dla stron z różnymi wymaganiami dotyczącymi autoryzacji dla różnych programów obsługi.

Poniższe dwa podejścia mogą służyć do stosowania autoryzacji do Razor metod obsługi stron:

* Użyj oddzielnych stron dla programów obsługi stron wymagających innej autoryzacji. Przenieś zawartość udostępnioną do co najmniej jednego [widoku częściowego](xref:mvc/views/partial). Jeśli to możliwe, jest to zalecane podejście.
* W przypadku zawartości, która musi udostępniać wspólną stronę, należy napisać filtr wykonujący autoryzację jako część [IAsyncPageFilter. OnPageHandlerSelectionAsync](xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncPageFilter.OnPageHandlerSelectionAsync%2A). Projekt [PageHandlerAuth](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth) GitHub ilustruje następujące podejście:
  * [AuthorizeIndexPageHandlerFilter](https://github.com/dotnet/AspNetCore.Docs/blob/main/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth/AuthorizeIndexPageHandlerFilter.cs) implementuje filtr autoryzacji:[!code-csharp[](~/security/authorization/simple/samples/3.1/PageHandlerAuth/Pages/Index.cshtml.cs?name=snippet)]

  * Atrybut [[AuthorizePageHandler]](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/security/authorization/simple/samples/3.1/PageHandlerAuth/Pages/Index.cshtml.cs#L16) jest stosowany do `OnGet` obsługi stron: [!code-csharp[](~/security/authorization/simple/samples/3.1/PageHandlerAuth/AuthorizeIndexPageHandlerFilter.cs?name=snippet)]

> [!WARNING]
> Podejście przykładowe [PageHandlerAuth](https://github.com/pranavkm/PageHandlerAuth) nie ***jest:***
> * Twórz przy użyciu atrybutów autoryzacji zastosowanych do strony, modelu strony lub globalnie. Tworzenie atrybutów autoryzacji powoduje, że uwierzytelnianie i autoryzacja są wykonywane wiele razy, gdy tylko jeden z nich jest `AuthorizeAttribute` `AuthorizeFilter` również stosowany do strony.
> * Pracuj w połączeniu z pozostałą częścią systemu uwierzytelniania i autoryzacji ASP.NET Core. Należy sprawdzić, czy użycie tego podejścia działa prawidłowo dla aplikacji.

Brak planów do obsługi `AuthorizeAttribute` Razor stron obsługi na stronie. 
