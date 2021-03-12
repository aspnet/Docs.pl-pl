---
title: Powiązanie modelu w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak działa powiązanie modelu w ASP.NET Core i jak dostosować jego zachowanie.
ms.assetid: 0be164aa-1d72-4192-bd6b-192c9c301164
ms.author: riande
ms.date: 12/18/2019
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
uid: mvc/models/model-binding
ms.openlocfilehash: 5eaedf6dbe5df59848b9cf8a5bda67add48db2a6
ms.sourcegitcommit: 54fe1ae5e7d068e27376d562183ef9ddc7afc432
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/10/2021
ms.locfileid: "102586946"
---
# <a name="model-binding-in-aspnet-core"></a>Powiązanie modelu w ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

W tym artykule wyjaśniono, co to jest powiązanie modelu, jak to działa i jak dostosować jego zachowanie.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/mvc/models/model-binding/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).

## <a name="what-is-model-binding"></a>Co to jest powiązanie modelu

Kontrolery i Razor strony współpracują z danymi, które pochodzą z żądań HTTP. Na przykład dane trasy mogą dostarczyć klucz rekordu, a pola ogłoszone formularza mogą podawać wartości właściwości modelu. Pisanie kodu w celu pobrania każdej z tych wartości i przekonwertowania ich z ciągów na typy .NET byłoby żmudnym i podatne na błędy. Powiązanie modelu automatyzuje ten proces. System powiązań modelu:

* Pobiera dane z różnych źródeł, takich jak dane tras, pola formularzy i ciągi zapytań.
* Dostarcza dane do kontrolerów i Razor stron w parametrach metod i właściwościach publicznych.
* Konwertuje dane ciągu na typy .NET.
* Aktualizuje właściwości typów złożonych.

## <a name="example"></a>Przykład

Załóżmy, że masz następującą metodę działania:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

A aplikacja odbiera żądanie przy użyciu tego adresu URL:

```
http://contoso.com/api/pets/2?DogsOnly=true
```

Powiązanie modelu przechodzi przez następujące kroki, gdy system routingu wybierze metodę akcji:

* Znajduje pierwszy parametr z `GetByID` , liczba całkowita o nazwie `id` .
* Wyszukuje dostępne źródła w żądaniu HTTP i odnajduje `id` = "2" w danych trasy.
* Konwertuje ciąg "2" na liczbę całkowitą 2.
* Znajduje następny parametr elementu `GetByID` , wartość logiczna o nazwie `dogsOnly` .
* Wyszukuje źródła i wyszukuje ciąg "DogsOnly = true" w ciągu zapytania. W dopasowaniu nazw nie jest rozróżniana wielkość liter.
* Konwertuje ciąg "true" na wartość logiczną `true` .

Struktura następnie wywołuje `GetById` metodę, przekazując wartość 2 dla `id` parametru i `true` dla `dogsOnly` parametru.

W poprzednim przykładzie elementy docelowe powiązań modelu to parametry metody, które są typami prostymi. Elementy docelowe mogą być również właściwościami typu złożonego. Po pomyślnym powiązaniu każdej właściwości [Walidacja modelu](xref:mvc/models/validation) jest wykonywana dla tej właściwości. Rekord danych powiązanych z modelem oraz wszelkie błędy powiązań lub walidacji są przechowywane w [ControllerBase. ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) lub [PageModel. ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState). Aby dowiedzieć się, czy ten proces zakończył się pomyślnie, aplikacja sprawdza flagę [ModelState. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) .

## <a name="targets"></a>Targets (Obiekty docelowe)

Powiązanie modelu próbuje znaleźć wartości dla następujących rodzajów obiektów docelowych:

* Parametry metody akcji kontrolera, do której jest kierowane żądanie.
* Parametry Razor metody obsługi stron, do której jest kierowane żądanie. 
* Właściwości publiczne kontrolera lub `PageModel` klasy, jeśli są określone przez atrybuty.

### <a name="bindproperty-attribute"></a>[BindProperty] — atrybut

Można zastosować do właściwości publicznej kontrolera lub `PageModel` klasy, aby spowodować powiązanie modelu z celem tej właściwości:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindproperties-attribute"></a>[BindProperties] — atrybut

Dostępne w ASP.NET Core 2,1 i nowszych.  Można zastosować do kontrolera lub `PageModel` klasy, aby poinformować powiązanie modelu z elementem docelowym wszystkich właściwości publicznych klasy:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a>Powiązanie modelu dla żądań HTTP GET

Domyślnie właściwości nie są powiązane z żądaniami HTTP GET. Zazwyczaj wszystkie potrzebne do żądania GET są parametrem identyfikatora rekordu. Identyfikator rekordu służy do wyszukiwania elementu w bazie danych. W związku z tym nie ma potrzeby powiązania właściwości, która przechowuje wystąpienie modelu. W scenariuszach, w których właściwości są powiązane z żądaniami GET, należy ustawić `SupportsGet` Właściwość na `true` :

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a>Źródła

Domyślnie powiązanie modelu pobiera dane w postaci par klucz-wartość z następujących źródeł w żądaniu HTTP:

1. Pola formularza
1. Treść żądania (dla [kontrolerów, które mają atrybut [ApiController]](xref:web-api/index#binding-source-parameter-inference)).
1. Dane trasy
1. Parametry ciągu zapytania
1. Przekazane pliki

Dla każdego parametru lub właściwości docelowej źródła są skanowane w kolejności wskazanej na poprzedniej liście. Istnieje kilka wyjątków:

* Dane trasy i wartości ciągu zapytania są używane tylko dla typów prostych.
* Przekazane pliki są powiązane tylko z typami docelowymi implementującymi `IFormFile` lub `IEnumerable<IFormFile>` .

Jeśli źródło domyślne jest niepoprawne, użyj jednego z następujących atrybutów, aby określić Źródło:

* [`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) -Pobiera wartości z ciągu zapytania. 
* [`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) -Pobiera wartości z danych trasy.
* [`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) -Pobiera wartości ze opublikowanych pól formularza.
* [`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) -Pobiera wartości z treści żądania.
* [`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) -Pobiera wartości z nagłówków HTTP.

Następujące atrybuty:

* Są dodawane do właściwości modelu pojedynczo (nie do klasy modelu), jak w poniższym przykładzie:

  [!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* Opcjonalnie Zaakceptuj wartość nazwy modelu w konstruktorze. Ta opcja jest dostępna w przypadku, gdy nazwa właściwości nie jest zgodna z wartością w żądaniu. Na przykład wartość w żądaniu może być nagłówkiem z łącznikiem w nazwie, jak w poniższym przykładzie:

  [!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a>[FromBody] — atrybut

Zastosuj `[FromBody]` atrybut do parametru, aby wypełnić jego właściwości z treści żądania HTTP. Środowisko uruchomieniowe ASP.NET Core deleguje odpowiedzialność za odczytanie treści do wejściowego programu formatującego. Dane wejściowe są wyjaśnione [w dalszej części tego artykułu](#input-formatters).

Gdy `[FromBody]` jest stosowany do parametru typu złożonego, wszystkie atrybuty źródła powiązań zastosowane do jego właściwości są ignorowane. Na przykład Poniższa `Create` Akcja określa, że jego `pet` parametr jest wypełniany w treści:

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

`Pet`Klasa określa, że jej `Breed` Właściwość jest wypełniana przy użyciu parametru ciągu zapytania:

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

W powyższym przykładzie:

* `[FromQuery]`Atrybut jest ignorowany.
* `Breed`Właściwość nie jest wypełniona z parametru ciągu zapytania. 

W danych wejściowych programu formatującego są odczytywane tylko treści i nie są rozpoznawane atrybuty źródłowe powiązania. Jeśli w treści zostanie znaleziona odpowiednia wartość, ta wartość jest używana do wypełniania `Breed` właściwości.

Nie stosuj `[FromBody]` do więcej niż jednego parametru na metodę akcji. Gdy strumień żądania jest odczytywany przez dane wejściowe programu formatującego, nie jest już dostępny do ponownego odczytywania dla powiązań innych `[FromBody]` parametrów.

### <a name="additional-sources"></a>Dodatkowe źródła

Dane źródłowe są dostarczane do systemu powiązań modelu przez *dostawców wartości*. Można napisać i zarejestrować dostawców wartości niestandardowych, którzy pobierają dane dla powiązania modelu z innych źródeł. Na przykład możesz potrzebować danych z cookie lub stanu sesji. Aby pobrać dane z nowego źródła:

* Utwórz klasę implementującą `IValueProvider` .
* Utwórz klasę implementującą `IValueProviderFactory` .
* Zarejestruj klasę fabryki w `Startup.ConfigureServices` .

Aplikacja Przykładowa zawiera [dostawcę wartości](https://github.com/dotnet/AspNetCore.Docs/blob/main/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProvider.cs) i przykład [fabryki](https://github.com/dotnet/AspNetCore.Docs/blob/main/aspnetcore/mvc/models/model-binding/samples/3.x/ModelBindingSample/CookieValueProviderFactory.cs) , który pobiera wartości z cookie s. Oto kod rejestracji w programie `Startup.ConfigureServices` :

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4)]

Pokazany kod umieszcza niestandardowego dostawcę wartości po wszystkich wbudowanych dostawcach wartości.  Aby najpierw utworzyć go na liście, należy wywołać `Insert(0, new CookieValueProviderFactory())` zamiast `Add` .

## <a name="no-source-for-a-model-property"></a>Brak źródła dla właściwości modelu

Domyślnie błąd stanu modelu nie jest tworzony, jeśli dla właściwości modelu nie znaleziono żadnej wartości. Właściwość jest ustawiona na null lub wartość domyślną:

* Typy proste o wartości null są ustawione na wartość `null` .
* Typy wartości, które nie są dopuszczane do wartości null, są ustawione na `default(T)` . Na przykład parametr `int id` jest ustawiony na 0.
* W przypadku typów złożonych powiązanie modelu tworzy wystąpienie przy użyciu domyślnego konstruktora bez ustawiania właściwości.
* Tablice są ustawiane na `Array.Empty<T>()` , z tą różnicą, że `byte[]` tablice są ustawione na `null` .

Jeśli stan modelu ma być unieważniony, gdy nic nie zostanie znalezione w polach formularza dla właściwości modelu, użyj [`[BindRequired]`](#bindrequired-attribute) atrybutu.

Należy zauważyć, że to `[BindRequired]` zachowanie ma zastosowanie do powiązania modelu z ogłoszonych danych formularza, nie do danych JSON ani XML w treści żądania. Dane treści żądania są obsługiwane przez elementy [formatujące dane wejściowe](#input-formatters).

## <a name="type-conversion-errors"></a>Błędy konwersji typów

Jeśli źródło zostanie znalezione, ale nie można go przekonwertować na typ docelowy, stan modelu jest oflagowany jako nieprawidłowy. Parametr lub właściwość docelowa jest ustawiona na null lub wartość domyślną, jak wskazano w poprzedniej sekcji.

W kontrolerze interfejsu API, który ma `[ApiController]` atrybut, nieprawidłowy stan modelu powoduje automatyczne odpowiedź HTTP 400.

Na Razor stronie ponownie Wyświetl stronę z komunikatem o błędzie:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

Weryfikacja po stronie klienta umożliwia przechwycenie najbardziej nieprawidłowych danych, które w przeciwnym razie zostałyby przesłane do Razor formularza stron. Ta weryfikacja sprawia, że trudno jest wyzwolić poprzedni wyróżniony kod. Przykładowa aplikacja zawiera przycisk **Prześlij z nieprawidłowym dniem** , który umieszcza złe dane w polu **Data zatrudnienia** i przesyła formularz. Ten przycisk pokazuje, w jaki sposób kod na potrzeby wyświetlania strony działa po wystąpieniu błędów konwersji danych.

Gdy strona jest ponownie wyświetlana przez poprzedni kod, nieprawidłowe dane wejściowe nie są wyświetlane w polu formularza. Wynika to z faktu, że właściwość model ma wartość null lub domyślną. Nieprawidłowe dane wejściowe są wyświetlane w komunikacie o błędzie. Jeśli jednak chcesz ponownie wyświetlić złe dane w polu formularza, rozważ, że właściwość model jest ciągiem i ręcznie wykonuje konwersję danych.

Ta sama strategia jest zalecana, jeśli nie chcesz, aby Błędy konwersji typów powodowały błędy stanu modelu. W takim przypadku należy zmienić wartość właściwości model na ciąg.

## <a name="simple-types"></a>Typy proste

Typy proste, które tworzą spinacz modelu mogą konwertować ciągi źródłowe, w następujący sposób:

* [Wartość logiczna](xref:System.ComponentModel.BooleanConverter)
* [Byte, bajty](xref:System.ComponentModel.ByteConverter) [](xref:System.ComponentModel.SByteConverter)
* [Delikatn](xref:System.ComponentModel.CharConverter)
* [Data/godzina](xref:System.ComponentModel.DateTimeConverter)
* [DateTimeOffset](xref:System.ComponentModel.DateTimeOffsetConverter)
* [Dokładności](xref:System.ComponentModel.DecimalConverter)
* [Double](xref:System.ComponentModel.DoubleConverter)
* [Wyliczenie](xref:System.ComponentModel.EnumConverter)
* [Ident](xref:System.ComponentModel.GuidConverter)
* [Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)
* [Pojedynczy](xref:System.ComponentModel.SingleConverter)
* [Czasu](xref:System.ComponentModel.TimeSpanConverter)
* [UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)
* [Adresu](xref:System.UriTypeConverter)
* [Wersja](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a>Typy złożone

Typ złożony musi mieć publiczny Konstruktor domyślny i publiczne właściwości do zapisu do powiązania. W przypadku wystąpienia powiązania modelu Klasa jest tworzona przy użyciu publicznego konstruktora domyślnego. 

Dla każdej właściwości typu złożonego powiązanie modelu wyszukuje źródła nazwy wzorca *prefix.property_name*. Jeśli nic nie zostanie znalezione, szuka tylko *property_name* bez prefiksu.

W przypadku powiązania z parametrem prefiks jest nazwą parametru. W przypadku powiązania z `PageModel` właściwością publiczną prefiks jest publiczną nazwą właściwości. Niektóre atrybuty mają `Prefix` Właściwość, która pozwala zastąpić domyślne użycie parametru lub nazwy właściwości.

Na przykład, Załóżmy, że typ złożony jest następującą `Instructor` klasą:

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a>Prefix = Nazwa parametru

Jeśli modelem, który ma zostać powiązany, jest parametr o nazwie `instructorToUpdate` :

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

Powiązanie modelu zaczyna się od przejrzenia źródeł klucza `instructorToUpdate.ID` . Jeśli ta wartość nie zostanie znaleziona, szuka `ID` bez prefiksu.

### <a name="prefix--property-name"></a>Prefix = nazwa właściwości

Jeśli modelem do powiązania jest właściwość o nazwie `Instructor` kontrolera lub `PageModel` klasy:

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

Powiązanie modelu zaczyna się od przejrzenia źródeł klucza `Instructor.ID` . Jeśli ta wartość nie zostanie znaleziona, szuka `ID` bez prefiksu.

### <a name="custom-prefix"></a>Prefiks niestandardowy

Jeśli model do powiązania jest parametrem o nazwie `instructorToUpdate` i `Bind` atrybut określa `Instructor` jako prefiks:

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

Powiązanie modelu zaczyna się od przejrzenia źródeł klucza `Instructor.ID` . Jeśli ta wartość nie zostanie znaleziona, szuka `ID` bez prefiksu.

### <a name="attributes-for-complex-type-targets"></a>Atrybuty dla obiektów docelowych typu złożonego

Dostępne są kilka wbudowanych atrybutów do kontrolowania powiązania modelu typów złożonych:

* `[Bind]`
* `[BindRequired]`
* `[BindNever]`

> [!WARNING]
> Te atrybuty wpływają na powiązanie modelu, gdy dane formularza ogłoszonego są źródłem wartości. ***Nie*** wpływają one na wejściowe elementy formatujące, które przetwarzają ogłoszone treści kodu JSON i XML. Dane wejściowe są wyjaśnione [w dalszej części tego artykułu](#input-formatters).

### <a name="bind-attribute"></a>[Bind] — atrybut

Można zastosować do klasy lub parametru metody. Określa, które właściwości modelu powinny być dołączone do powiązania modelu. `[Bind]` nie ***ma wpływu na*** wejściowe elementy formatujące.

W poniższym przykładzie tylko określone właściwości `Instructor` modelu są powiązane, gdy wywoływana jest jakakolwiek procedura obsługi lub metoda działania:

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

W poniższym przykładzie tylko określone właściwości `Instructor` modelu są powiązane, gdy `OnPost` wywoływana jest metoda:

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

Ten `[Bind]` atrybut może służyć do ochrony przed nadużyciem w scenariuszach *tworzenia* scenariuszy. Nie działa prawidłowo w scenariuszach edycji, ponieważ wykluczone właściwości mają ustawioną wartość null lub wartość domyślną, a nie jako pozostawione bez zmian. W celu zapewnienia obrony przed przekroczeniem, zaleca się, aby zamiast `[Bind]` atrybutu. Aby uzyskać więcej informacji, zobacz [temat Security uwagi dotyczący przefinalizowania](xref:data/ef-mvc/crud#security-note-about-overposting).

### <a name="modelbinder-attribute"></a>[ModelBinder] — atrybut

<xref:Microsoft.AspNetCore.Mvc.ModelBinderAttribute> można zastosować do typów, właściwości lub parametrów. Umożliwia określenie typu spinacza modelu używanego do powiązania określonego wystąpienia lub typu. Na przykład:

```C#
[HttpPost]
public IActionResult OnPost([ModelBinder(typeof(MyInstructorModelBinder))] Instructor instructor)
```

Ten `[ModelBinder]` atrybut może być również używany do zmiany nazwy właściwości lub parametru, gdy jest on powiązany z modelem:

```C#
public class Instructor
{
    [ModelBinder(Name = "instructor_id")]
    public string Id { get; set; }
    
    public string Name { get; set; }
}
```

### <a name="bindrequired-attribute"></a>[BindRequired] — atrybut

Można stosować tylko do właściwości modelu, a nie do parametrów metody. Powoduje, że powiązanie modelu umożliwia dodanie błędu stanu modelu, Jeśli powiązanie nie może wystąpić dla właściwości modelu. Oto przykład:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

Zobacz również Omówienie `[Required]` atrybutu w [walidacji modelu](xref:mvc/models/validation#required-attribute).

### <a name="bindnever-attribute"></a>[BindNever] — atrybut

Można stosować tylko do właściwości modelu, a nie do parametrów metody. Uniemożliwia powiązanie modelu z ustawiania właściwości modelu. Oto przykład:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

## <a name="collections"></a>Kolekcje

Dla celów, które są kolekcjami typów prostych, powiązanie modelu wyszukuje dopasowania do *parameter_name* lub *property_name*. Jeśli dopasowanie nie zostanie znalezione, szuka jednego z obsługiwanych formatów bez prefiksu. Na przykład:

* Załóżmy, że parametr, który ma zostać powiązany, jest tablicą o nazwie `selectedCourses` :

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* Dane formularza lub ciągu zapytania mogą znajdować się w jednym z następujących formatów:
   
  ```
  selectedCourses=1050&selectedCourses=2000 
  ```

  ```
  selectedCourses[0]=1050&selectedCourses[1]=2000
  ```

  ```
  [0]=1050&[1]=2000
  ```

  ```
  selectedCourses[a]=1050&selectedCourses[b]=2000&selectedCourses.index=a&selectedCourses.index=b
  ```

  ```
  [a]=1050&[b]=2000&index=a&index=b
  ```

* Następujący format jest obsługiwany tylko w danych formularza:

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* We wszystkich powyższych formatach przykładowe powiązanie modelu przekazuje tablicę dwóch elementów do `selectedCourses` parametru:

  * selectedCourses [0] = 1050
  * selectedCourses [1] = 2000

  Formaty danych używające liczb indeksów dolnych (... [0]... [1]...) należy upewnić się, że są numerowane sekwencyjnie, zaczynając od zera. Jeśli występują luki w numerze indeksu dolnego, wszystkie elementy po przerwie zostaną zignorowane. Na przykład, jeśli indeksy dolne są równe 0 i 2 zamiast 0 i 1, drugi element jest ignorowany.

## <a name="dictionaries"></a>Słowniki

Dla `Dictionary` elementów docelowych powiązanie modelu wyszukuje dopasowania do *parameter_name* lub *property_name*. Jeśli dopasowanie nie zostanie znalezione, szuka jednego z obsługiwanych formatów bez prefiksu. Na przykład:

* Załóżmy, że parametr docelowy jest `Dictionary<int, string>` nazwany `selectedCourses` :

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* Ogłoszone dane formularza lub ciągu zapytania mogą wyglądać jak w jednym z następujących przykładów:

  ```
  selectedCourses[1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  [1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  selectedCourses[0].Key=1050&selectedCourses[0].Value=Chemistry&
  selectedCourses[1].Key=2000&selectedCourses[1].Value=Economics
  ```

  ```
  [0].Key=1050&[0].Value=Chemistry&[1].Key=2000&[1].Value=Economics
  ```

* We wszystkich powyższych formatach przykładowe powiązanie modelu przekazuje słownik dwóch elementów do `selectedCourses` parametru:

  * selectedCourses ["1050"] = "Chemia"
  * selectedCourses ["2000"] = "ekonomia"
  
::: moniker-end

::: moniker range=">= aspnetcore-5.0"

## <a name="constructor-binding-and-record-types"></a>Typy powiązań konstruktora i rekordów

Powiązanie modelu wymaga, aby typy złożone miały Konstruktor bez parametrów. Zarówno programowy `System.Text.Json` , jak i `Newtonsoft.Json` oparty na danych wejściowych elementy formatujące obsługują deserializacja klas, które nie mają konstruktora bez parametrów. 

W języku C# 9 wprowadzono typy rekordów, które są doskonałym sposobem na zwięzłe przedstawianie danych przez sieć. ASP.NET Core dodaje obsługę powiązania modelu i sprawdzanie poprawności typów rekordów przy użyciu pojedynczego konstruktora:

```csharp
public record Person([Required] string Name, [Range(0, 150)] int Age);

public class PersonController
{
   public IActionResult Index() => View();

   [HttpPost]
   public IActionResult Index(Person person)
   {
       ...
   }
}
```

`Person/Index.cshtml`:

```cshtml
@model Person

Name: <input asp-for="Name" />
...
Age: <input asp-for="Age" />
```

Podczas sprawdzania poprawności typów rekordów środowisko uruchomieniowe wyszukuje metadane walidacji w zależności od parametrów, a nie właściwości.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a>Zachowanie globalizacji danych tras powiązań modelu i ciągów zapytań

Dostawca wartości ASP.NET Core tras i dostawcy wartości ciągu zapytania:

* Traktuj wartości jako niezmienną kulturę.
* Należy oczekiwać, że adresy URL są kulturą niezmienną.

Natomiast wartości pochodzące z danych formularza są przekształcane z uwzględnieniem kultury. Jest to zaprojektowane w taki sposób, aby adresy URL były udostępniane w ustawieniach regionalnych.

Aby dostawca wartości ASP.NET Core trasy i dostawca wartości ciągu zapytania były poddawane konwersji zależnej od kultury:

* Dziedzicz po <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory>
* Skopiuj kod z [QueryStringValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/main/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) lub [RouteValueValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/main/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)
* Zastąp [wartość kultury](https://github.com/dotnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) przekazaną do konstruktora dostawcy wartości wartością [CultureInfo. CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)
* Zastąp domyślną fabrykę dostawcy wartości w opcjach MVC nowym:

[!code-csharp[](model-binding/samples_snapshot/3.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/3.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a>Specjalne typy danych

Istnieją specjalne typy danych, które może obsłużyć powiązanie modelu.

### <a name="iformfile-and-iformfilecollection"></a>IFormFile i IFormFileCollection

Przekazany plik uwzględniony w żądaniu HTTP.  Obsługiwane jest również `IEnumerable<IFormFile>` dla wielu plików.

### <a name="cancellationtoken"></a>CancellationToken

Akcje można opcjonalnie powiązać `CancellationToken` jako parametr. To wiąże <xref:Microsoft.AspNetCore.Http.HttpContext.RequestAborted> się z tym, że w przypadku przerwania połączenia z żądaniem http zostało przerwane. Akcje mogą używać tego parametru, aby anulować długotrwałe operacje asynchroniczne, które są wykonywane w ramach akcji kontrolera.

### <a name="formcollection"></a>Formularz

Służy do pobierania wszystkich wartości z ogłoszonych danych formularza.

## <a name="input-formatters"></a>Wejściowe elementy formatujące

Dane w treści żądania mogą być w formacie JSON, XML lub innym. Aby przeanalizować te dane, powiązanie modelu korzysta z *wejściowego programu formatującego* , który jest skonfigurowany do obsługi określonego typu zawartości. Domyślnie ASP.NET Core zawiera dane wejściowe w formacie JSON na potrzeby obsługi danych JSON. Można dodać inne elementy formatujące dla innych typów zawartości.

ASP.NET Core wybiera wejściowe elementy formatujące na podstawie atrybutu [użycia](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) . Jeśli atrybut nie jest obecny, używa [nagłówka Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).

Aby użyć wbudowanych elementów formatujących dane wejściowe XML:

* Zainstaluj `Microsoft.AspNetCore.Mvc.Formatters.Xml` pakiet NuGet.

* W `Startup.ConfigureServices` , wywołaj <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> lub <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*> .

  [!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=10)]

* Zastosuj `Consumes` atrybut do klas kontrolera lub metod akcji, które powinny oczekiwać XML w treści żądania.

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  Aby uzyskać więcej informacji, zobacz [wprowadzenie serializacji XML](/dotnet/standard/serialization/introducing-xml-serialization).

### <a name="customize-model-binding-with-input-formatters"></a>Dostosowywanie powiązania modelu z użyciem danych wejściowych

Wejściowy program formatujący pobiera pełną odpowiedzialność za odczytywanie danych z treści żądania. Aby dostosować ten proces, należy skonfigurować interfejsy API używane przez dane wejściowe programu formatującego. W tej sekcji opisano sposób dostosowywania programu `System.Text.Json` formatującego wejściowego opartego na danych wejściowych w celu zrozumienia niestandardowego typu o nazwie `ObjectId` . 

Rozważmy następujący model, który zawiera właściwość niestandardową `ObjectId` o nazwie `Id` :

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/ModelWithObjectId.cs?name=snippet_Class&highlight=3)]

Aby dostosować proces powiązania modelu przy użyciu `System.Text.Json` , Utwórz klasę pochodną <xref:System.Text.Json.Serialization.JsonConverter%601> :

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/JsonConverters/ObjectIdConverter.cs?name=snippet_Class)]

Aby użyć niestandardowego konwertera, Zastosuj <xref:System.Text.Json.Serialization.JsonConverterAttribute> atrybut do typu. W poniższym przykładzie `ObjectId` Typ jest skonfigurowany z użyciem `ObjectIdConverter` jako konwertera niestandardowego:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Models/ObjectId.cs?name=snippet_Class&highlight=1)]

Aby uzyskać więcej informacji, zobacz [jak pisać konwertery niestandardowe](/dotnet/standard/serialization/system-text-json-converters-how-to).

## <a name="exclude-specified-types-from-model-binding"></a>Wyklucz określone typy z powiązania modelu

Zachowanie modelu powiązań i systemów walidacji jest zależne od [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata). Można dostosować `ModelMetadata` , dodając dostawcę szczegółów do [MvcOptions. ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders). Wbudowane dostawcy szczegółów są dostępne do wyłączenia powiązania modelu lub walidacji dla określonych typów.

Aby wyłączyć powiązanie modelu dla wszystkich modeli określonego typu, Dodaj element <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> w `Startup.ConfigureServices` . Na przykład aby wyłączyć powiązanie modelu dla wszystkich modeli typu `System.Version` :

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=5-6)]

Aby wyłączyć walidację właściwości określonego typu, Dodaj element <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> w `Startup.ConfigureServices` . Na przykład aby wyłączyć walidację właściwości typu `System.Guid` :

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=7-8)]

## <a name="custom-model-binders"></a>Niestandardowe powiązania modelu

Można rozszerzać powiązania modelu, pisząc niestandardowy spinacz modelu i używając `[ModelBinder]` atrybutu, aby wybrać go dla danego elementu docelowego. Dowiedz się więcej na temat [niestandardowego powiązania modelu](xref:mvc/advanced/custom-model-binding).

## <a name="manual-model-binding"></a>Ręczne powiązanie modelu 

Powiązanie modelu można wywołać ręcznie przy użyciu <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> metody. Metoda jest zdefiniowana dla obu `ControllerBase` klas i `PageModel` . Przeciążenia metod umożliwiają określenie prefiksu i dostawcy wartości do użycia. Metoda zwraca `false` Jeśli powiązanie modelu nie powiedzie się. Oto przykład:

[!code-csharp[](model-binding/samples/3.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

<xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*>  używa dostawców wartości do pobierania danych z formularza, ciągu zapytania i danych tras. `TryUpdateModelAsync` zwykle: 

* Używany ze Razor stronami i aplikacjami MVC korzystającymi z kontrolerów i widoków, aby zapobiec nadmiernemu księgowaniu.
* Nieużywany z interfejsem API sieci Web, chyba że jest używane z danych formularza, ciągów zapytań i danych tras. Punkty końcowe interfejsu API sieci Web, które [używają formatu JSON, do](#input-formatters) deserializacji treści żądania do obiektu.

Aby uzyskać więcej informacji, zobacz [TryUpdateModelAsync](xref:data/ef-rp/crud#TryUpdateModelAsync).

## <a name="fromservices-attribute"></a>[FromServices] — atrybut

Nazwa tego atrybutu jest zgodna ze wzorcem atrybutów powiązania modelu, które określają źródło danych. Ale nie informacje o powiązaniu danych od dostawcy wartości. Pobiera wystąpienie typu z kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) . Jego celem jest zapewnienie alternatywy dla iniekcji konstruktorów, gdy potrzebna jest usługa tylko wtedy, gdy jest wywoływana konkretna metoda.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

W tym artykule wyjaśniono, co to jest powiązanie modelu, jak to działa i jak dostosować jego zachowanie.

[Wyświetlanie lub Pobieranie przykładowego kodu](https://github.com/dotnet/AspNetCore.Docs/tree/main/aspnetcore/mvc/models/model-binding/samples) ([jak pobrać](xref:index#how-to-download-a-sample)).

## <a name="what-is-model-binding"></a>Co to jest powiązanie modelu

Kontrolery i Razor strony współpracują z danymi, które pochodzą z żądań HTTP. Na przykład dane trasy mogą dostarczyć klucz rekordu, a pola ogłoszone formularza mogą podawać wartości właściwości modelu. Pisanie kodu w celu pobrania każdej z tych wartości i przekonwertowania ich z ciągów na typy .NET byłoby żmudnym i podatne na błędy. Powiązanie modelu automatyzuje ten proces. System powiązań modelu:

* Pobiera dane z różnych źródeł, takich jak dane tras, pola formularzy i ciągi zapytań.
* Dostarcza dane do kontrolerów i Razor stron w parametrach metod i właściwościach publicznych.
* Konwertuje dane ciągu na typy .NET.
* Aktualizuje właściwości typów złożonych.

## <a name="example"></a>Przykład

Załóżmy, że masz następującą metodę działania:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Controllers/PetsController.cs?name=snippet_DogsOnly)]

A aplikacja odbiera żądanie przy użyciu tego adresu URL:

```
http://contoso.com/api/pets/2?DogsOnly=true
```

Powiązanie modelu przechodzi przez następujące kroki, gdy system routingu wybierze metodę akcji:

* Znajduje pierwszy parametr z `GetByID` , liczba całkowita o nazwie `id` .
* Wyszukuje dostępne źródła w żądaniu HTTP i odnajduje `id` = "2" w danych trasy.
* Konwertuje ciąg "2" na liczbę całkowitą 2.
* Znajduje następny parametr elementu `GetByID` , wartość logiczna o nazwie `dogsOnly` .
* Wyszukuje źródła i wyszukuje ciąg "DogsOnly = true" w ciągu zapytania. W dopasowaniu nazw nie jest rozróżniana wielkość liter.
* Konwertuje ciąg "true" na wartość logiczną `true` .

Struktura następnie wywołuje `GetById` metodę, przekazując wartość 2 dla `id` parametru i `true` dla `dogsOnly` parametru.

W poprzednim przykładzie elementy docelowe powiązań modelu to parametry metody, które są typami prostymi. Elementy docelowe mogą być również właściwościami typu złożonego. Po pomyślnym powiązaniu każdej właściwości [Walidacja modelu](xref:mvc/models/validation) jest wykonywana dla tej właściwości. Rekord danych powiązanych z modelem oraz wszelkie błędy powiązań lub walidacji są przechowywane w [ControllerBase. ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState) lub [PageModel. ModelState](xref:Microsoft.AspNetCore.Mvc.ControllerBase.ModelState). Aby dowiedzieć się, czy ten proces zakończył się pomyślnie, aplikacja sprawdza flagę [ModelState. IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) .

## <a name="targets"></a>Targets (Obiekty docelowe)

Powiązanie modelu próbuje znaleźć wartości dla następujących rodzajów obiektów docelowych:

* Parametry metody akcji kontrolera, do której jest kierowane żądanie.
* Parametry Razor metody obsługi stron, do której jest kierowane żądanie. 
* Właściwości publiczne kontrolera lub `PageModel` klasy, jeśli są określone przez atrybuty.

### <a name="bindproperty-attribute"></a>[BindProperty] — atrybut

Można zastosować do właściwości publicznej kontrolera lub `PageModel` klasy, aby spowodować powiązanie modelu z celem tej właściwości:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Edit.cshtml.cs?name=snippet_BindProperty&highlight=3-4)]

### <a name="bindproperties-attribute"></a>[BindProperties] — atrybut

Dostępne w ASP.NET Core 2,1 i nowszych.  Można zastosować do kontrolera lub `PageModel` klasy, aby poinformować powiązanie modelu z elementem docelowym wszystkich właściwości publicznych klasy:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_BindProperties&highlight=1-2)]

### <a name="model-binding-for-http-get-requests"></a>Powiązanie modelu dla żądań HTTP GET

Domyślnie właściwości nie są powiązane z żądaniami HTTP GET. Zazwyczaj wszystkie potrzebne do żądania GET są parametrem identyfikatora rekordu. Identyfikator rekordu służy do wyszukiwania elementu w bazie danych. W związku z tym nie ma potrzeby powiązania właściwości, która przechowuje wystąpienie modelu. W scenariuszach, w których właściwości są powiązane z żądaniami GET, należy ustawić `SupportsGet` Właściwość na `true` :

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_SupportsGet)]

## <a name="sources"></a>Źródła

Domyślnie powiązanie modelu pobiera dane w postaci par klucz-wartość z następujących źródeł w żądaniu HTTP:

1. Pola formularza
1. Treść żądania (dla [kontrolerów, które mają atrybut [ApiController]](xref:web-api/index#binding-source-parameter-inference)).
1. Dane trasy
1. Parametry ciągu zapytania
1. Przekazane pliki

Dla każdego parametru lub właściwości docelowej źródła są skanowane w kolejności wskazanej na poprzedniej liście. Istnieje kilka wyjątków:

* Dane trasy i wartości ciągu zapytania są używane tylko dla typów prostych.
* Przekazane pliki są powiązane tylko z typami docelowymi implementującymi `IFormFile` lub `IEnumerable<IFormFile>` .

Jeśli źródło domyślne jest niepoprawne, użyj jednego z następujących atrybutów, aby określić Źródło:

* [`[FromQuery]`](xref:Microsoft.AspNetCore.Mvc.FromQueryAttribute) -Pobiera wartości z ciągu zapytania. 
* [`[FromRoute]`](xref:Microsoft.AspNetCore.Mvc.FromRouteAttribute) -Pobiera wartości z danych trasy.
* [`[FromForm]`](xref:Microsoft.AspNetCore.Mvc.FromFormAttribute) -Pobiera wartości ze opublikowanych pól formularza.
* [`[FromBody]`](xref:Microsoft.AspNetCore.Mvc.FromBodyAttribute) -Pobiera wartości z treści żądania.
* [`[FromHeader]`](xref:Microsoft.AspNetCore.Mvc.FromHeaderAttribute) -Pobiera wartości z nagłówków HTTP.

Następujące atrybuty:

* Są dodawane do właściwości modelu pojedynczo (nie do klasy modelu), jak w poniższym przykładzie:

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/Instructor.cs?name=snippet_FromQuery&highlight=5-6)]

* Opcjonalnie Zaakceptuj wartość nazwy modelu w konstruktorze. Ta opcja jest dostępna w przypadku, gdy nazwa właściwości nie jest zgodna z wartością w żądaniu. Na przykład wartość w żądaniu może być nagłówkiem z łącznikiem w nazwie, jak w poniższym przykładzie:

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Index.cshtml.cs?name=snippet_FromHeader)]

### <a name="frombody-attribute"></a>[FromBody] — atrybut

Zastosuj `[FromBody]` atrybut do parametru, aby wypełnić jego właściwości z treści żądania HTTP. Środowisko uruchomieniowe ASP.NET Core deleguje odpowiedzialność za odczytanie treści do wejściowego programu formatującego. Dane wejściowe są wyjaśnione [w dalszej części tego artykułu](#input-formatters).

Gdy `[FromBody]` jest stosowany do parametru typu złożonego, wszystkie atrybuty źródła powiązań zastosowane do jego właściwości są ignorowane. Na przykład Poniższa `Create` Akcja określa, że jego `pet` parametr jest wypełniany w treści:

```csharp
public ActionResult<Pet> Create([FromBody] Pet pet)
```

`Pet`Klasa określa, że jej `Breed` Właściwość jest wypełniana przy użyciu parametru ciągu zapytania:

```csharp
public class Pet
{
    public string Name { get; set; }

    [FromQuery] // Attribute is ignored.
    public string Breed { get; set; }
}
```

W powyższym przykładzie:

* `[FromQuery]`Atrybut jest ignorowany.
* `Breed`Właściwość nie jest wypełniona z parametru ciągu zapytania. 

W danych wejściowych programu formatującego są odczytywane tylko treści i nie są rozpoznawane atrybuty źródłowe powiązania. Jeśli w treści zostanie znaleziona odpowiednia wartość, ta wartość jest używana do wypełniania `Breed` właściwości.

Nie stosuj `[FromBody]` do więcej niż jednego parametru na metodę akcji. Gdy strumień żądania jest odczytywany przez dane wejściowe programu formatującego, nie jest już dostępny do ponownego odczytywania dla powiązań innych `[FromBody]` parametrów.

### <a name="additional-sources"></a>Dodatkowe źródła

Dane źródłowe są dostarczane do systemu powiązań modelu przez *dostawców wartości*. Można napisać i zarejestrować dostawców wartości niestandardowych, którzy pobierają dane dla powiązania modelu z innych źródeł. Na przykład możesz potrzebować danych z cookie lub stanu sesji. Aby pobrać dane z nowego źródła:

* Utwórz klasę implementującą `IValueProvider` .
* Utwórz klasę implementującą `IValueProviderFactory` .
* Zarejestruj klasę fabryki w `Startup.ConfigureServices` .

Aplikacja Przykładowa zawiera [dostawcę wartości](https://github.com/dotnet/AspNetCore.Docs/blob/main/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProvider.cs) i przykład [fabryki](https://github.com/dotnet/AspNetCore.Docs/blob/main/aspnetcore/mvc/models/model-binding/samples/2.x/ModelBindingSample/CookieValueProviderFactory.cs) , który pobiera wartości z cookie s. Oto kod rejestracji w programie `Startup.ConfigureServices` :

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=3)]

Pokazany kod umieszcza niestandardowego dostawcę wartości po wszystkich wbudowanych dostawcach wartości.  Aby najpierw utworzyć go na liście, należy wywołać `Insert(0, new CookieValueProviderFactory())` zamiast `Add` .

## <a name="no-source-for-a-model-property"></a>Brak źródła dla właściwości modelu

Domyślnie błąd stanu modelu nie jest tworzony, jeśli dla właściwości modelu nie znaleziono żadnej wartości. Właściwość jest ustawiona na null lub wartość domyślną:

* Typy proste o wartości null są ustawione na wartość `null` .
* Typy wartości, które nie są dopuszczane do wartości null, są ustawione na `default(T)` . Na przykład parametr `int id` jest ustawiony na 0.
* W przypadku typów złożonych powiązanie modelu tworzy wystąpienie przy użyciu domyślnego konstruktora bez ustawiania właściwości.
* Tablice są ustawiane na `Array.Empty<T>()` , z tą różnicą, że `byte[]` tablice są ustawione na `null` .

Jeśli stan modelu ma być unieważniony, gdy nic nie zostanie znalezione w polach formularza dla właściwości modelu, użyj [`[BindRequired]`](#bindrequired-attribute) atrybutu.

Należy zauważyć, że to `[BindRequired]` zachowanie ma zastosowanie do powiązania modelu z ogłoszonych danych formularza, nie do danych JSON ani XML w treści żądania. Dane treści żądania są obsługiwane przez elementy [formatujące dane wejściowe](#input-formatters).

## <a name="type-conversion-errors"></a>Błędy konwersji typów

Jeśli źródło zostanie znalezione, ale nie można go przekonwertować na typ docelowy, stan modelu jest oflagowany jako nieprawidłowy. Parametr lub właściwość docelowa jest ustawiona na null lub wartość domyślną, jak wskazano w poprzedniej sekcji.

W kontrolerze interfejsu API, który ma `[ApiController]` atrybut, nieprawidłowy stan modelu powoduje automatyczne odpowiedź HTTP 400.

Na Razor stronie ponownie Wyświetl stronę z komunikatem o błędzie:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/Instructors/Create.cshtml.cs?name=snippet_HandleMBError&highlight=3-6)]

Weryfikacja po stronie klienta umożliwia przechwycenie najbardziej nieprawidłowych danych, które w przeciwnym razie zostałyby przesłane do Razor formularza stron. Ta weryfikacja sprawia, że trudno jest wyzwolić poprzedni wyróżniony kod. Przykładowa aplikacja zawiera przycisk **Prześlij z nieprawidłowym dniem** , który umieszcza złe dane w polu **Data zatrudnienia** i przesyła formularz. Ten przycisk pokazuje, w jaki sposób kod na potrzeby wyświetlania strony działa po wystąpieniu błędów konwersji danych.

Gdy strona jest ponownie wyświetlana przez poprzedni kod, nieprawidłowe dane wejściowe nie są wyświetlane w polu formularza. Wynika to z faktu, że właściwość model ma wartość null lub domyślną. Nieprawidłowe dane wejściowe są wyświetlane w komunikacie o błędzie. Jeśli jednak chcesz ponownie wyświetlić złe dane w polu formularza, rozważ, że właściwość model jest ciągiem i ręcznie wykonuje konwersję danych.

Ta sama strategia jest zalecana, jeśli nie chcesz, aby Błędy konwersji typów powodowały błędy stanu modelu. W takim przypadku należy zmienić wartość właściwości model na ciąg.

## <a name="simple-types"></a>Typy proste

Typy proste, które tworzą spinacz modelu mogą konwertować ciągi źródłowe, w następujący sposób:

* [Wartość logiczna](xref:System.ComponentModel.BooleanConverter)
* [Byte, bajty](xref:System.ComponentModel.ByteConverter) [](xref:System.ComponentModel.SByteConverter)
* [Delikatn](xref:System.ComponentModel.CharConverter)
* [Data/godzina](xref:System.ComponentModel.DateTimeConverter)
* [DateTimeOffset](xref:System.ComponentModel.DateTimeOffsetConverter)
* [Dokładności](xref:System.ComponentModel.DecimalConverter)
* [Double](xref:System.ComponentModel.DoubleConverter)
* [Wyliczenie](xref:System.ComponentModel.EnumConverter)
* [Ident](xref:System.ComponentModel.GuidConverter)
* [Int16](xref:System.ComponentModel.Int16Converter), [Int32](xref:System.ComponentModel.Int32Converter), [Int64](xref:System.ComponentModel.Int64Converter)
* [Pojedynczy](xref:System.ComponentModel.SingleConverter)
* [Czasu](xref:System.ComponentModel.TimeSpanConverter)
* [UInt16](xref:System.ComponentModel.UInt16Converter), [UInt32](xref:System.ComponentModel.UInt32Converter), [UInt64](xref:System.ComponentModel.UInt64Converter)
* [Adresu](xref:System.UriTypeConverter)
* [Wersja](xref:System.ComponentModel.VersionConverter)

## <a name="complex-types"></a>Typy złożone

Typ złożony musi mieć publiczny Konstruktor domyślny i publiczne właściwości do zapisu do powiązania. W przypadku wystąpienia powiązania modelu Klasa jest tworzona przy użyciu publicznego konstruktora domyślnego. 

Dla każdej właściwości typu złożonego powiązanie modelu wyszukuje źródła nazwy wzorca *prefix.property_name*. Jeśli nic nie zostanie znalezione, szuka tylko *property_name* bez prefiksu.

W przypadku powiązania z parametrem prefiks jest nazwą parametru. W przypadku powiązania z `PageModel` właściwością publiczną prefiks jest publiczną nazwą właściwości. Niektóre atrybuty mają `Prefix` Właściwość, która pozwala zastąpić domyślne użycie parametru lub nazwy właściwości.

Na przykład, Załóżmy, że typ złożony jest następującą `Instructor` klasą:

  ```csharp
  public class Instructor
  {
      public int ID { get; set; }
      public string LastName { get; set; }
      public string FirstName { get; set; }
  }
  ```

### <a name="prefix--parameter-name"></a>Prefix = Nazwa parametru

Jeśli modelem, który ma zostać powiązany, jest parametr o nazwie `instructorToUpdate` :

```csharp
public IActionResult OnPost(int? id, Instructor instructorToUpdate)
```

Powiązanie modelu zaczyna się od przejrzenia źródeł klucza `instructorToUpdate.ID` . Jeśli ta wartość nie zostanie znaleziona, szuka `ID` bez prefiksu.

### <a name="prefix--property-name"></a>Prefix = nazwa właściwości

Jeśli modelem do powiązania jest właściwość o nazwie `Instructor` kontrolera lub `PageModel` klasy:

```csharp
[BindProperty]
public Instructor Instructor { get; set; }
```

Powiązanie modelu zaczyna się od przejrzenia źródeł klucza `Instructor.ID` . Jeśli ta wartość nie zostanie znaleziona, szuka `ID` bez prefiksu.

### <a name="custom-prefix"></a>Prefiks niestandardowy

Jeśli model do powiązania jest parametrem o nazwie `instructorToUpdate` i `Bind` atrybut określa `Instructor` jako prefiks:

```csharp
public IActionResult OnPost(
    int? id, [Bind(Prefix = "Instructor")] Instructor instructorToUpdate)
```

Powiązanie modelu zaczyna się od przejrzenia źródeł klucza `Instructor.ID` . Jeśli ta wartość nie zostanie znaleziona, szuka `ID` bez prefiksu.

### <a name="attributes-for-complex-type-targets"></a>Atrybuty dla obiektów docelowych typu złożonego

Dostępne są kilka wbudowanych atrybutów do kontrolowania powiązania modelu typów złożonych:

* `[BindRequired]`
* `[BindNever]`
* `[Bind]`

> [!NOTE]
> Te atrybuty wpływają na powiązanie modelu, gdy dane formularza ogłoszonego są źródłem wartości. Nie wpływają one na wejściowe elementy formatujące, które przetwarzają ogłoszone treści kodu JSON i XML. Dane wejściowe są wyjaśnione [w dalszej części tego artykułu](#input-formatters).
>
> Zobacz również Omówienie `[Required]` atrybutu w [walidacji modelu](xref:mvc/models/validation#required-attribute).

### <a name="bindrequired-attribute"></a>[BindRequired] — atrybut

Można stosować tylko do właściwości modelu, a nie do parametrów metody. Powoduje, że powiązanie modelu umożliwia dodanie błędu stanu modelu, Jeśli powiązanie nie może wystąpić dla właściwości modelu. Oto przykład:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithCollection.cs?name=snippet_BindRequired&highlight=8-9)]

### <a name="bindnever-attribute"></a>[BindNever] — atrybut

Można stosować tylko do właściwości modelu, a nie do parametrów metody. Uniemożliwia powiązanie modelu z ustawiania właściwości modelu. Oto przykład:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Models/InstructorWithDictionary.cs?name=snippet_BindNever&highlight=3-4)]

### <a name="bind-attribute"></a>[Bind] — atrybut

Można zastosować do klasy lub parametru metody. Określa, które właściwości modelu powinny być dołączone do powiązania modelu.

W poniższym przykładzie tylko określone właściwości `Instructor` modelu są powiązane, gdy wywoływana jest jakakolwiek procedura obsługi lub metoda działania:

```csharp
[Bind("LastName,FirstMidName,HireDate")]
public class Instructor
```

W poniższym przykładzie tylko określone właściwości `Instructor` modelu są powiązane, gdy `OnPost` wywoływana jest metoda:

```csharp
[HttpPost]
public IActionResult OnPost([Bind("LastName,FirstMidName,HireDate")] Instructor instructor)
```

Ten `[Bind]` atrybut może służyć do ochrony przed nadużyciem w scenariuszach *tworzenia* scenariuszy. Nie działa prawidłowo w scenariuszach edycji, ponieważ wykluczone właściwości mają ustawioną wartość null lub wartość domyślną, a nie jako pozostawione bez zmian. W celu zapewnienia obrony przed przekroczeniem, zaleca się, aby zamiast `[Bind]` atrybutu. Aby uzyskać więcej informacji, zobacz [temat Security uwagi dotyczący przefinalizowania](xref:data/ef-mvc/crud#security-note-about-overposting).

## <a name="collections"></a>Kolekcje

Dla celów, które są kolekcjami typów prostych, powiązanie modelu wyszukuje dopasowania do *parameter_name* lub *property_name*. Jeśli dopasowanie nie zostanie znalezione, szuka jednego z obsługiwanych formatów bez prefiksu. Na przykład:

* Załóżmy, że parametr, który ma zostać powiązany, jest tablicą o nazwie `selectedCourses` :

  ```csharp
  public IActionResult OnPost(int? id, int[] selectedCourses)
  ```

* Dane formularza lub ciągu zapytania mogą znajdować się w jednym z następujących formatów:
   
  ```
  selectedCourses=1050&selectedCourses=2000 
  ```

  ```
  selectedCourses[0]=1050&selectedCourses[1]=2000
  ```

  ```
  [0]=1050&[1]=2000
  ```

  ```
  selectedCourses[a]=1050&selectedCourses[b]=2000&selectedCourses.index=a&selectedCourses.index=b
  ```

  ```
  [a]=1050&[b]=2000&index=a&index=b
  ```

* Następujący format jest obsługiwany tylko w danych formularza:

  ```
  selectedCourses[]=1050&selectedCourses[]=2000
  ```

* We wszystkich powyższych formatach przykładowe powiązanie modelu przekazuje tablicę dwóch elementów do `selectedCourses` parametru:

  * selectedCourses [0] = 1050
  * selectedCourses [1] = 2000

  Formaty danych używające liczb indeksów dolnych (... [0]... [1]...) należy upewnić się, że są numerowane sekwencyjnie, zaczynając od zera. Jeśli występują luki w numerze indeksu dolnego, wszystkie elementy po przerwie zostaną zignorowane. Na przykład, jeśli indeksy dolne są równe 0 i 2 zamiast 0 i 1, drugi element jest ignorowany.

## <a name="dictionaries"></a>Słowniki

Dla `Dictionary` elementów docelowych powiązanie modelu wyszukuje dopasowania do *parameter_name* lub *property_name*. Jeśli dopasowanie nie zostanie znalezione, szuka jednego z obsługiwanych formatów bez prefiksu. Na przykład:

* Załóżmy, że parametr docelowy jest `Dictionary<int, string>` nazwany `selectedCourses` :

  ```csharp
  public IActionResult OnPost(int? id, Dictionary<int, string> selectedCourses)
  ```

* Ogłoszone dane formularza lub ciągu zapytania mogą wyglądać jak w jednym z następujących przykładów:

  ```
  selectedCourses[1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  [1050]=Chemistry&selectedCourses[2000]=Economics
  ```

  ```
  selectedCourses[0].Key=1050&selectedCourses[0].Value=Chemistry&
  selectedCourses[1].Key=2000&selectedCourses[1].Value=Economics
  ```

  ```
  [0].Key=1050&[0].Value=Chemistry&[1].Key=2000&[1].Value=Economics
  ```

* We wszystkich powyższych formatach przykładowe powiązanie modelu przekazuje słownik dwóch elementów do `selectedCourses` parametru:

  * selectedCourses ["1050"] = "Chemia"
  * selectedCourses ["2000"] = "ekonomia"

<a name="glob"></a>

## <a name="globalization-behavior-of-model-binding-route-data-and-query-strings"></a>Zachowanie globalizacji danych tras powiązań modelu i ciągów zapytań

Dostawca wartości ASP.NET Core tras i dostawcy wartości ciągu zapytania:

* Traktuj wartości jako niezmienną kulturę.
* Należy oczekiwać, że adresy URL są kulturą niezmienną.

Natomiast wartości pochodzące z danych formularza są przekształcane z uwzględnieniem kultury. Jest to zaprojektowane w taki sposób, aby adresy URL były udostępniane w ustawieniach regionalnych.

Aby dostawca wartości ASP.NET Core trasy i dostawca wartości ciągu zapytania były poddawane konwersji zależnej od kultury:

* Dziedzicz po <xref:Microsoft.AspNetCore.Mvc.ModelBinding.IValueProviderFactory>
* Skopiuj kod z [QueryStringValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/main/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs) lub [RouteValueValueProviderFactory](https://github.com/dotnet/AspNetCore/blob/main/src/Mvc/Mvc.Core/src/ModelBinding/RouteValueProviderFactory.cs)
* Zastąp [wartość kultury](https://github.com/dotnet/AspNetCore/blob/e625fe29b049c60242e8048b4ea743cca65aa7b5/src/Mvc/Mvc.Core/src/ModelBinding/QueryStringValueProviderFactory.cs#L30) przekazaną do konstruktora dostawcy wartości wartością [CultureInfo. CurrentCulture](xref:System.Globalization.CultureInfo.CurrentCulture)
* Zastąp domyślną fabrykę dostawcy wartości w opcjach MVC nowym:

[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet)]
[!code-csharp[](model-binding/samples_snapshot/2.x/Startup.cs?name=snippet1)]

## <a name="special-data-types"></a>Specjalne typy danych

Istnieją specjalne typy danych, które może obsłużyć powiązanie modelu.

### <a name="iformfile-and-iformfilecollection"></a>IFormFile i IFormFileCollection

Przekazany plik uwzględniony w żądaniu HTTP.  Obsługiwane jest również `IEnumerable<IFormFile>` dla wielu plików.

### <a name="cancellationtoken"></a>CancellationToken

Służy do anulowania działania w kontrolerach asynchronicznych.

### <a name="formcollection"></a>Formularz

Służy do pobierania wszystkich wartości z ogłoszonych danych formularza.

## <a name="input-formatters"></a>Wejściowe elementy formatujące

Dane w treści żądania mogą być w formacie JSON, XML lub innym. Aby przeanalizować te dane, powiązanie modelu korzysta z *wejściowego programu formatującego* , który jest skonfigurowany do obsługi określonego typu zawartości. Domyślnie ASP.NET Core zawiera dane wejściowe w formacie JSON na potrzeby obsługi danych JSON. Można dodać inne elementy formatujące dla innych typów zawartości.

ASP.NET Core wybiera wejściowe elementy formatujące na podstawie atrybutu [użycia](xref:Microsoft.AspNetCore.Mvc.ConsumesAttribute) . Jeśli atrybut nie jest obecny, używa [nagłówka Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html).

Aby użyć wbudowanych elementów formatujących dane wejściowe XML:

* Zainstaluj `Microsoft.AspNetCore.Mvc.Formatters.Xml` pakiet NuGet.

* W `Startup.ConfigureServices` , wywołaj <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlSerializerFormatters*> lub <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcCoreBuilderExtensions.AddXmlDataContractSerializerFormatters*> .

  [!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=9)]

* Zastosuj `Consumes` atrybut do klas kontrolera lub metod akcji, które powinny oczekiwać XML w treści żądania.

  ```csharp
  [HttpPost]
  [Consumes("application/xml")]
  public ActionResult<Pet> Create(Pet pet)
  ```

  Aby uzyskać więcej informacji, zobacz [wprowadzenie serializacji XML](/dotnet/standard/serialization/introducing-xml-serialization).

## <a name="exclude-specified-types-from-model-binding"></a>Wyklucz określone typy z powiązania modelu

Zachowanie modelu powiązań i systemów walidacji jest zależne od [ModelMetadata](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.modelmetadata). Można dostosować `ModelMetadata` , dodając dostawcę szczegółów do [MvcOptions. ModelMetadataDetailsProviders](xref:Microsoft.AspNetCore.Mvc.MvcOptions.ModelMetadataDetailsProviders). Wbudowane dostawcy szczegółów są dostępne do wyłączenia powiązania modelu lub walidacji dla określonych typów.

Aby wyłączyć powiązanie modelu dla wszystkich modeli określonego typu, Dodaj element <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.ExcludeBindingMetadataProvider> w `Startup.ConfigureServices` . Na przykład aby wyłączyć powiązanie modelu dla wszystkich modeli typu `System.Version` :

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=4-5)]

Aby wyłączyć walidację właściwości określonego typu, Dodaj element <xref:Microsoft.AspNetCore.Mvc.ModelBinding.SuppressChildValidationMetadataProvider> w `Startup.ConfigureServices` . Na przykład aby wyłączyć walidację właściwości typu `System.Guid` :

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Startup.cs?name=snippet_ValueProvider&highlight=6-7)]

## <a name="custom-model-binders"></a>Niestandardowe powiązania modelu

Można rozszerzać powiązania modelu, pisząc niestandardowy spinacz modelu i używając `[ModelBinder]` atrybutu, aby wybrać go dla danego elementu docelowego. Dowiedz się więcej na temat [niestandardowego powiązania modelu](xref:mvc/advanced/custom-model-binding).

## <a name="manual-model-binding"></a>Ręczne powiązanie modelu

Powiązanie modelu można wywołać ręcznie przy użyciu <xref:Microsoft.AspNetCore.Mvc.ControllerBase.TryUpdateModelAsync*> metody. Metoda jest zdefiniowana dla obu `ControllerBase` klas i `PageModel` . Przeciążenia metod umożliwiają określenie prefiksu i dostawcy wartości do użycia. Metoda zwraca `false` Jeśli powiązanie modelu nie powiedzie się. Oto przykład:

[!code-csharp[](model-binding/samples/2.x/ModelBindingSample/Pages/InstructorsWithCollection/Create.cshtml.cs?name=snippet_TryUpdate&highlight=1-4)]

## <a name="fromservices-attribute"></a>[FromServices] — atrybut

Nazwa tego atrybutu jest zgodna ze wzorcem atrybutów powiązania modelu, które określają źródło danych. Ale nie informacje o powiązaniu danych od dostawcy wartości. Pobiera wystąpienie typu z kontenera [iniekcji zależności](xref:fundamentals/dependency-injection) . Jego celem jest zapewnienie alternatywy dla iniekcji konstruktorów, gdy potrzebna jest usługa tylko wtedy, gdy jest wywoływana konkretna metoda.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:mvc/models/validation>
* <xref:mvc/advanced/custom-model-binding>

::: moniker-end
