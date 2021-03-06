# <a name="add-validation-to-an-aspnet-core-mvc-app"></a>Dodawanie walidacji do aplikacji ASP.NET Core MVC

Przez [Rick Anderson](https://twitter.com/RickAndMSFT)

W tej sekcji dodasz logikę walidacji do `Movie` modelu i będziesz mieć pewność, że reguły walidacji są wymuszane za każdym razem, gdy użytkownik tworzy lub edytuje film.

## <a name="keeping-things-dry"></a>Przechowywanie SUCHEj zawartości

Jeden z założenia projektu MVC jest suchy [](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("nie powtarzaj się"). ASP.NET Core MVC zachęca do określania funkcjonalności lub zachowania tylko raz, a następnie znajdować się w dowolnym miejscu w aplikacji. Pozwala to zmniejszyć ilość kodu, który trzeba napisać, i sprawia, że kod jest mniej podatny na błędy, łatwiejszy do testowania i łatwiejszy w obsłudze.

Obsługa walidacji świadczona przez MVC i Entity Framework Core Code First jest dobrym przykładem zasady SUCHa w działaniu. Można deklaratywnie określić reguły walidacji w jednym miejscu (w klasie modelu), a reguły są wymuszane wszędzie w aplikacji.

## <a name="adding-validation-rules-to-the-movie-model"></a>Dodawanie reguł walidacji do modelu filmu

Otwórz plik *Movie.cs* . DataAnnotations zawiera wbudowany zestaw atrybutów sprawdzania poprawności, które są stosowane deklaratywnie do dowolnej klasy lub właściwości. (Zawiera również atrybuty formatowania, takie `DataType` jak pomoc z formatowaniem i nie podawanie żadnych walidacji).

`Range` `RegularExpression` `StringLength` `Required`Zaktualizuj klasę, aby skorzystać z wbudowanych atrybutów,, i walidacji. `Movie`

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie21/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

::: moniker-end

Atrybuty walidacji określają zachowanie, które chcesz wymusić na właściwościach modelu, do których są stosowane. Atrybuty `Required` i`MinimumLength` wskazują, że właściwość musi mieć wartość, ale nic uniemożliwia użytkownikowi wprowadzanie białych znaków w celu zaspokojenia tej walidacji. Ten `RegularExpression` atrybut służy do ograniczania, jakie znaki mogą być wprowadzane. W powyższym `Genre` kodzie i `Rating` musi używać tylko liter (pierwsza litera, wielkie litery, cyfry i znaki specjalne są niedozwolone). `Range` Atrybut ogranicza wartość do określonego zakresu. Ten `StringLength` atrybut pozwala ustawić maksymalną długość właściwości ciągu i opcjonalnie jej długość minimalną. Typy wartości (takie jak `decimal` `float`, `int` `DateTime`,,) są z założenia wymagane i nie wymagają `[Required]` atrybutu.

Automatyczne Wymuszanie reguł sprawdzania poprawności przez ASP.NET Core pomaga zwiększyć niezawodność aplikacji. Gwarantuje to również, że nie można zapomnieć, aby zweryfikować coś i przypadkowo umożliwić niewłaściwe dane w bazie danych.

## <a name="validation-error-ui-in-mvc"></a>Interfejs użytkownika błędu walidacji w MVC

Uruchom aplikację i przejdź do kontrolera filmów.

Naciśnij link **Utwórz nowy** , aby dodać nowy film. Wypełnij formularz nieprawidłowymi wartościami. Gdy tylko Walidacja po stronie klienta jQuery wykryje błąd, zostanie wyświetlony komunikat o błędzie.

![Formularz wyświetlania filmu z wieloma błędami walidacji po stronie klienta jQuery](~/tutorials/first-mvc-app/validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

Zwróć uwagę, jak formularz automatycznie renderuje odpowiedni komunikat o błędzie walidacji w każdym polu zawierającym nieprawidłową wartość. Błędy są wymuszane po stronie klienta (przy użyciu języków JavaScript i jQuery) i po stronie serwera (w przypadku, gdy użytkownik ma wyłączony kod JavaScript).

Znacząca korzyść polega na tym, że nie trzeba zmieniać pojedynczego wiersza kodu w `MoviesController` klasie lub w widoku *Create. cshtml* , aby włączyć ten interfejs użytkownika weryfikacji. Kontroler i widoki utworzone wcześniej w tym samouczku automatycznie pobierają reguły sprawdzania poprawności określone przy użyciu atrybutów walidacji we właściwościach `Movie` klasy modelu. Sprawdzanie poprawności testu przy `Edit` użyciu metody akcji i zastosowanie tej samej walidacji.

Dane formularza nie są wysyłane do serwera, dopóki nie zostaną wykryte błędy weryfikacji po stronie klienta. Można to sprawdzić, umieszczając punkt przerwania w `HTTP Post` metodzie przy użyciu [Narzędzia programu Fiddler](https://www.telerik.com/fiddler) lub [narzędzi programistycznych F12](/microsoft-edge/devtools-guide).

## <a name="how-validation-works"></a>Jak działa Walidacja

Możesz zastanawiać się, jak został wygenerowany interfejs użytkownika weryfikacji bez aktualizacji kodu w kontrolerze lub widokach. Poniższy kod przedstawia dwie `Create` metody.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Controllers/MoviesController.cs?name=snippetCreate)]

Pierwsza metoda akcji (http get `Create` ) zawiera początkowy formularz tworzenia. Druga wersja (`[HttpPost]`) obsługuje wpis w formularzu. Druga `Create` Metoda `ModelState.IsValid` (wersja) wywołania, aby sprawdzić, czy film ma jakiekolwiek błędy walidacji. `[HttpPost]` Wywołanie tej metody szacuje wszystkie atrybuty walidacji, które zostały zastosowane do obiektu. Jeśli obiekt ma błędy walidacji, `Create` Metoda ponowna wyświetli formularz. Jeśli nie ma żadnych błędów, Metoda zapisuje nowy film w bazie danych. W naszym przykładzie filmu nie jest on ogłaszany na serwerze, gdy na stronie klienta wykryto błędy walidacji. Druga `Create` Metoda nie jest nigdy wywoływana, gdy występują błędy walidacji po stronie klienta. W przypadku wyłączenia języka JavaScript w przeglądarce sprawdzanie poprawności klienta jest wyłączone i można przetestować metodę `Create` `ModelState.IsValid` post protokołu HTTP w celu wykrycia błędów walidacji.

Można ustawić punkt przerwania w `[HttpPost] Create` metodzie i sprawdzić, czy metoda nie jest nigdy wywoływana, podczas walidacji po stronie klienta nie będą przesyłane dane formularza po wykryciu błędów walidacji. Jeśli wyłączysz JavaScript w przeglądarce, a następnie prześlesz formularz z błędami, zostanie osiągnięty punkt przerwania. Nadal będziesz mieć pełną weryfikację bez języka JavaScript. 

Na poniższej ilustracji przedstawiono sposób wyłączania języka JavaScript w przeglądarce FireFox.

![Firefox Na karcie zawartość w obszarze Opcje usuń zaznaczenie pola wyboru Włącz język JavaScript.](~/tutorials/first-mvc-app/validation/_static/ff.png)

Na poniższej ilustracji przedstawiono sposób wyłączania języka JavaScript w przeglądarce Chrome.

![Google Chrome: W sekcji JavaScript ustawień zawartości wybierz opcję nie Zezwalaj na uruchamianie skryptów JavaScript przez żadną lokację.](~/tutorials/first-mvc-app/validation/_static/chrome.png)

Po wyłączeniu języka JavaScript Opublikuj nieprawidłowe dane i przechodzenie przez debuger.

![Podczas debugowania wpisu nieprawidłowych danych funkcja IntelliSense w ModelState. IsValid pokazuje wartość false.](~/tutorials/first-mvc-app/validation/_static/ms.png)

Poniżej znajduje się część szablonu widoku *Create. cshtml* , który został utworzony wcześniej w samouczku. Jest on używany przez metody akcji pokazane powyżej obydwu, aby wyświetlić początkowy formularz i ponownie wyświetlić go w przypadku błędu.

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Views/Movies/CreateRatingBrevity.cshtml)]

[Pomocnik tagu wejściowego](xref:mvc/views/working-with-forms) używa atrybutów [](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) DataAnnotations i tworzy atrybuty HTML, które są zbędne do walidacji jQuery po stronie klienta. [Pomocnik tagów walidacji](xref:mvc/views/working-with-forms#the-validation-tag-helpers) wyświetla błędy walidacji. Aby [](xref:mvc/models/validation) uzyskać więcej informacji, zobacz Walidacja.

W rzeczywistości to podejście jest takie, że ani kontroler, ani `Create` szablon widoku nie wie o faktycznych regułach walidacji lub o określonych komunikatach o błędach. Reguły walidacji i ciągi błędów są określone tylko w `Movie` klasie. Te same reguły sprawdzania poprawności są automatycznie stosowane do `Edit` widoku i wszystkich innych szablonów widoków, które można utworzyć, edytując model.

W przypadku konieczności zmiany logiki walidacji można to zrobić w dokładnie jednym miejscu przez dodanie atrybutów sprawdzania poprawności do modelu (w tym przykładzie `Movie` Klasa). Nie trzeba martwić się o różne części aplikacji, które nie są zgodne z zasadami, w których są wymuszane — Cała logika walidacji zostanie zdefiniowana w jednym miejscu i użyta wszędzie. Dzięki temu kod jest bardzo czysty i ułatwia utrzymanie i rozwój. Oznacza to, że będziesz w pełni przestrzegać SUCHEj zasady.

## <a name="using-datatype-attributes"></a>Używanie atrybutów DataType

Otwórz plik *Movie.cs* i zapoznaj się `Movie` z klasą. `System.ComponentModel.DataAnnotations` Przestrzeń nazw zawiera atrybuty formatowania oprócz wbudowanego zestawu atrybutów walidacji. Wartość `DataType` wyliczenia została już zastosowana do daty wydania i do pól cen. Poniższy kod pokazuje `ReleaseDate` właściwości i `Price` z odpowiednim `DataType` atrybutem.

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

Atrybuty zawierają tylko wskazówki dla aparatu widoku do formatowania danych (i udostępniają elementy/atrybuty, takie jak `<a>` adresy URL i `<a href="mailto:EmailAddress.com">` wiadomości e-mail. `DataType` Możesz użyć atrybutu, `RegularExpression` aby sprawdzić poprawność formatu danych. Ten `DataType` atrybut służy do określania typu danych, który jest bardziej szczegółowy niż typ wewnętrzny bazy danych, nie są atrybutami walidacji. W tym przypadku chcemy tylko śledzić datę, a nie godzinę. `DataType` Wyliczenie zawiera wiele typów danych, takich jak data, godzina, numer telefonu, waluta, EmailAddress i inne. Ten `DataType` atrybut może również umożliwić aplikacji automatyczne udostępnianie funkcji specyficznych dla typu. Na przykład `mailto:` można utworzyć łącze dla `DataType.EmailAddress`i dla programu `DataType.Date` w przeglądarkach, które obsługują HTML5, można podać selektor daty. Atrybuty emitują HTML 5 `data-` (wymawiane kreski danych), które mogą zrozumieć przeglądarki HTML 5. `DataType` Atrybuty nie zapewniają żadnej weryfikacji.  `DataType`

`DataType.Date`nie określa formatu wyświetlanej daty. Domyślnie pole dane jest wyświetlane zgodnie z domyślnymi formatami opartymi na serwerze `CultureInfo`.

Ten `DisplayFormat` atrybut służy do jawnego określenia formatu daty:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode` Ustawienie określa, że formatowanie powinno być również stosowane, gdy wartość jest wyświetlana w polu tekstowym do edycji. (Możesz nie chcieć, aby w przypadku niektórych pól — na przykład w przypadku wartości walutowych, prawdopodobnie nie chcesz, aby symbol waluty był widoczny w polu tekstowym do edycji).

Możesz użyć `DisplayFormat` atrybutu przez siebie, ale zazwyczaj dobrym pomysłem jest `DataType` użycie atrybutu. Ten `DataType` atrybut przekazuje semantykę danych w przeciwieństwie do sposobu renderowania na ekranie i zapewnia następujące korzyści, których nie można uzyskać za pomocą DisplayFormat:

* Przeglądarka może włączać funkcje HTML5 (na przykład w celu wyświetlania kontrolki kalendarza, symbolu waluty właściwej dla ustawień regionalnych, linków e-mail itp.).

* Domyślnie przeglądarka będzie renderować dane przy użyciu poprawnego formatu na podstawie ustawień regionalnych.

* Ten `DataType` atrybut umożliwia wybranie odpowiedniego szablonu pola do renderowania danych przez MVC. `DisplayFormat` (Jeśli używane przez siebie korzysta z szablonu ciągu).

> [!NOTE]
> Walidacja jQuery nie działa z `Range` atrybutem `DateTime`i. Na przykład poniższy kod zawsze będzie wyświetlał błąd walidacji po stronie klienta, nawet wtedy, gdy data jest w określonym zakresie:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

Należy wyłączyć sprawdzanie poprawności daty jQuery, aby użyć `Range` atrybutu z. `DateTime` Ogólnie rzecz biorąc, nie jest dobrym sposobem kompilowania dat stałych w modelach, dlatego przy użyciu `Range` atrybutu i `DateTime` nie jest to odradzane.

Poniższy kod ilustruje łączenie atrybutów w jednym wierszu:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie21/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

::: moniker-end

W następnej części serii zapoznajemy się z aplikacją i wprowadzisz ulepszenia do automatycznie generowanych `Details` i `Delete` metod.

## <a name="additional-resources"></a>Dodatkowe zasoby

* [Praca z formularzami](xref:mvc/views/working-with-forms)
* [Globalizacja i lokalizacja](xref:fundamentals/localization)
* [Wprowadzenie do pomocy tagów](xref:mvc/views/tag-helpers/intro)
* [Autorzy tagów](xref:mvc/views/tag-helpers/authoring)
