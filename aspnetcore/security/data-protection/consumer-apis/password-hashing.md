---
title: Hasła skrótu w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skrócić hasła przy użyciu interfejsów API ochrony danych ASP.NET Core.
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
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: b88abd076febae95f8990bcd730ce0d4517e92d2
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102587765"
---
# <a name="hash-passwords-in-aspnet-core"></a>Hasła skrótu w ASP.NET Core

Baza kodu ochrony danych zawiera pakiet *Microsoft. AspNetCore. Cryptography.* Key, który zawiera funkcje wyprowadzania klucza kryptograficznego. Ten pakiet jest składnikiem autonomicznym i nie ma żadnych zależności w pozostałej części systemu ochrony danych. Może być używany całkowicie niezależnie. Źródło istnieje wraz z bazą kodu ochrony danych jako wygoda.

Pakiet oferuje obecnie metodę `KeyDerivation.Pbkdf2` , która umożliwia mieszanie hasła przy użyciu [algorytmu PBKDF2](https://tools.ietf.org/html/rfc2898#section-5.2). Ten interfejs API jest bardzo podobny do istniejącego [typu Rfc2898DeriveBytes](/dotnet/api/system.security.cryptography.rfc2898derivebytes).NET Framework, ale istnieją trzy ważne rozróżnienia:

1. `KeyDerivation.Pbkdf2`Metoda obsługuje zużywanie wielu PRFs (obecnie `HMACSHA1` , `HMACSHA256` i `HMACSHA512` ), podczas gdy `Rfc2898DeriveBytes` Typ obsługuje tylko `HMACSHA1` .

2. `KeyDerivation.Pbkdf2`Metoda wykrywa bieżący system operacyjny i próbuje wybrać najbardziej zoptymalizowaną implementację procedury, zapewniając znacznie lepszą wydajność w niektórych przypadkach. (W systemie Windows 8 oferuje około 10X przepływności `Rfc2898DeriveBytes` .)

3. `KeyDerivation.Pbkdf2`Metoda wymaga, aby obiekt wywołujący określił wszystkie parametry (sól, PRF i liczba iteracji). `Rfc2898DeriveBytes`Typ zawiera wartości domyślne dla tych.

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

Zobacz [kod źródłowy](https://github.com/dotnet/AspNetCore/blob/main/src/Identity/Extensions.Core/src/PasswordHasher.cs) typu dla ASP.NET Core Identity `PasswordHasher` rzeczywistego przypadku użycia.
