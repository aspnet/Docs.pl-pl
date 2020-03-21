<span data-ttu-id="48364-101">Składnik `RedirectToLogin` (*Shared/RedirectToLogin. Razor*):</span><span class="sxs-lookup"><span data-stu-id="48364-101">The `RedirectToLogin` component (*Shared/RedirectToLogin.razor*):</span></span>

* <span data-ttu-id="48364-102">Zarządza przekierowywaniem nieautoryzowanych użytkowników do strony logowania.</span><span class="sxs-lookup"><span data-stu-id="48364-102">Manages redirecting unauthorized users to the login page.</span></span>
* <span data-ttu-id="48364-103">Zachowuje bieżący adres URL, do którego użytkownik próbuje uzyskać dostęp, aby można go było zwrócić do tej strony w przypadku pomyślnego uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="48364-103">Preserves the current URL that the user is attempting to access so that they can be returned to that page if authentication is successful.</span></span>

```razor
@inject NavigationManager Navigation
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@code {
    protected override void OnInitialized()
    {
        Navigation.NavigateTo($"authentication/login?returnUrl={Navigation.Uri}");
    }
}
```