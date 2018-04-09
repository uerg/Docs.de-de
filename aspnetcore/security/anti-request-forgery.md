---
title: -Angriffe zu verhindern, dass Cross-Site Request XSRF/Websiteübergreifender Anforderungsfälschung in ASP.NET Core
author: steve-smith
description: Erfahren Sie, wie ein, um Angriffe auf Web-apps zu verhindern, in denen eine bösartige Website die Interaktion zwischen einem Clientbrowser und die app auswirken kann.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/anti-request-forgery
ms.openlocfilehash: ad50f8b261447d40ccc24c0ee006239aa976bf20
ms.sourcegitcommit: 7d02ca5f5ddc2ca3eb0258fdd6996fbf538c129a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2018
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a>-Angriffe zu verhindern, dass Cross-Site Request XSRF/Websiteübergreifender Anforderungsfälschung in ASP.NET Core

Durch [Steve Smith](https://ardalis.com/), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), und [Rick Anderson](https://twitter.com/RickAndMSFT)

Websiteübergreifende anforderungsfälschung (auch bekannt als XSRF oder CSRF, Aussprache *Siehe Surfen*) ist ein Angriff gegen Web gehostete apps, die bei dem eine böswillige Web-app die Interaktion zwischen einem Clientbrowser und eine WebApp, die Vertrauensstellungen, die beeinflussen kann Browser. Diese Angriffe sind möglich, da Webbrowsern einige Arten von Authentifizierungstoken automatisch mit jeder Anforderung an eine Website gesendete. Diese Form von Angriff ist auch bekannt als ein *einmalklick-Angriff* oder *Sitzung riding* , da das Risiko eines Angriffs nutzt die der Benutzer zuvor Sitzung authentifiziert.

Ein Beispiel von CSRF-Angriffen:

1. Ein Benutzer meldet sich in `www.good-banking-site.com` mit Formularauthentifizierung. Der Server authentifiziert den Benutzer und gibt eine Antwort, die ein Authentifizierungscookie enthält. Der Standort ist anfällig für Angriffe, da sie alle Anforderungen vertraut, die sie mit einem gültigen Authentifizierungscookie erhält.
1. Der Benutzer besucht eine bösartige Website `www.bad-crook-site.com`.

   Die bösartige Website `www.bad-crook-site.com`, enthält ein HTML-Formular, das etwa wie folgt:

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   Beachten Sie, dass des Formulars `action` Beiträge, die an die anfällig für Website und nicht an die bösartige Website. Dies ist der "Cross-Site" Teil CSRF.

1. Der Benutzer wählt die Schaltfläche "Absenden". Der Browser sendet die Anforderung und schließt automatisch das Authentifizierungscookie für die angeforderte Domäne `www.good-banking-site.com`.
1. Die Anforderung ausgeführt wird, auf die `www.good-banking-site.com` Server mit dem Kontext des Benutzers Authentifizierung und alle Aktionen, die ein authentifizierter Benutzer ausführen darf ausführen können.

Wenn der Benutzer die Schaltfläche zum Senden des Formulars auswählt, kann die bösartige Website:

* Führen Sie ein Skript, das automatisch auf das Formular sendet.
* Sendet eine Formularübermittlung als eine AJAX-Anforderung. 
* Verwenden Sie ein ausgeblendetes Formular mit CSS ein. 

Verwendung von HTTPS nicht zu verhindern, dass eine CSRF-Angriffen. Kann die bösartige Website senden ein `https://www.good-banking-site.com/` anfordern genauso einfach wie eine unsichere Anforderung senden können.

Einige Angriffe als Ziel Endpunkte, auf die GET-Anforderungen reagiert. in diesem Fall einem Bildtag dienen zum Ausführen der Aktion. Diese Form von Angriff ist üblich, auf der Forum-Standorte, die Bilder ermöglichen jedoch JavaScript blockiert. Apps, die Status für GET-Anforderungen ändern, in denen Variablen oder Ressourcen geändert werden, sind anfällig für Angriffe. **GET-Anforderungen, die Status geändert sind unsicher. Eine bewährte Methode besteht darin nie Zustand für eine GET-Anforderung ändern.**

CSRF-Angriffen sind für Web-apps, die Cookies für die Authentifizierung zu verwenden, da möglich:

* Browser Speichern von Cookies, die von einer Web-app ausgegeben.
* Gespeicherte Cookies enthalten die Sitzungscookies für authentifizierte Benutzer.
* Browser senden alle Cookies einer Domäne in der Web-app zugeordnete jede Anforderung, unabhängig davon, wie die Anforderung an die app innerhalb des Browsers generiert wurde.

Allerdings CSRF-Angriffen sind nicht begrenzt auf Cookies ausnutzen. Beispielsweise Basis-und Digestauthentifizierung sind auch anfällig. Nachdem ein Benutzer mit Standard oder Digest-Authentifizierung anmeldet, sendet der Browser die Anmeldeinformationen automatisch bis die Sitzung&dagger; endet.

&dagger;In diesem Kontext *Sitzung* bezieht sich auf die clientseitige-Sitzung, die während der Authentifizierung des Benutzers. Es ist unabhängig vom stagingstatus serverseitige Sitzungen oder [ASP.NET Core Sitzung Middleware](xref:fundamentals/app-state).

Benutzer können CSRF Sicherheitsrisiken erraten, durch Vorsichtsmaßnahmen:

* Melden Sie sich außerhalb der Web-apps, die nach Abschluss ihrer Verwendung.
* Deaktivieren der Browsercookies in regelmäßigen Abständen.

Allerdings sind CSRF Sicherheitsrisiken im Grunde ein Problem mit der Web-app, nicht für den Endbenutzer.

## <a name="authentication-fundamentals"></a>Grundlagen der Authentifizierung

Cookie-basierte Authentifizierung ist eine gängige Form der Authentifizierung. Token-basierter Authentifizierungssysteme wachsen in Beliebtheit, insbesondere für Single Page Applications (SPAs).

### <a name="cookie-based-authentication"></a>Cookie-basierte Authentifizierung

Wenn ein Benutzer authentifiziert hat, mithilfe seines Benutzernamens und Kennworts, sind sie ein Token, das ein Authentifizierungsticket für die Authentifizierung und Autorisierung verwendeten enthält ausgestellt. Das Token wird als ein Cookie, die mit jeder Anforderung der Client stellt gespeichert. Generieren und überprüfen dieses Cookie wird von der Cookieauthentifizierungsmiddleware ausgeführt. Die [Middleware](xref:fundamentals/middleware/index) einen Benutzerprinzipal in einem verschlüsselten Cookie serialisiert. Bei nachfolgenden Anforderungen die Middleware überprüft das Cookie, den Prinzipal erstellt, und weist den Prinzipal, der [Benutzer](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) Eigenschaft [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).

### <a name="token-based-authentication"></a>Token-basierte Authentifizierung

Wenn ein Benutzer authentifiziert wurde, sind sie ein Token (nicht antiforgery Token) ausgegeben. Das Token enthält Benutzerinformationen in Form von [Ansprüche](/dotnet/framework/security/claims-based-identity-model) oder ein Verweistoken, die die app auf Benutzerzustand verwaltet in der app verweist. Wenn ein Benutzer versucht, Zugriff auf eine Ressource, die eine Authentifizierung erforderlich ist, wird das Token an die app mit einer zusätzlichen Authorization-Header in Form von Bearer-Token gesendet. Dadurch wird die app zustandslos. Jede nachfolgende Anforderung ist das Token für die serverseitige Validierung in der Anforderung übergeben. Dieses Token wird nicht *verschlüsselte*; es hat *codiert*. Auf dem Server wird das Token für den Zugriff auf seine Informationen decodiert. Um das Token bei nachfolgenden Anforderungen zu senden, speichern Sie das Token im lokalen Speicher des Browsers. Werden Sie nicht Sicherheitsrisiko FORGERY besorgt, wenn das Token im lokalen Speicher des Browsers gespeichert werden. CSRF ist relevant, wenn das Token in einem Cookie gespeichert wird.

### <a name="multiple-apps-hosted-at-one-domain"></a>Mehrere apps, die an einer Domäne gehostet

Freigegebene Hostingumgebungen sind anfällig für Sitzungsübernahme CSRF-Anmeldung und andere Angriffe.

Obwohl `example1.contoso.net` und `example2.contoso.net` sind verschiedene Hosts besteht ein implizites Vertrauensverhältnis zwischen Hosts unter der `*.contoso.net` Domäne. Diese implizite Vertrauensstellung kann potenziell nicht vertrauenswürdige Hosts Cookies gegenseitig beeinträchtigen (die gleichen-Origin-Richtlinien, die zum Steuern der AJAX-Anforderungen sind nicht unbedingt auf HTTP-Cookies anwendbar).

Angriffe, die vertrauenswürdigen Cookies zwischen apps, die auf der gleichen Domäne gehostet ausnutzen können verhindert werden, indem Sie Domänen nicht freigeben. Wenn jede app in einer eigenen Domäne gehostet wird, besteht keine Vertrauensstellung implizite Cookie, um diese auszunutzen.

## <a name="aspnet-core-antiforgery-configuration"></a>ASP.NET Core antiforgery-Konfiguration

> [!WARNING]
> ASP.NET Core implementiert antiforgery mit [ASP.NET Core Datenschutz](xref:security/data-protection/introduction). Data Protection Stapel muss konfiguriert werden, um in einer Serverfarm zu arbeiten. Finden Sie unter [Konfigurieren von Data Protection](xref:security/data-protection/configuration/overview) für Weitere Informationen.

In ASP.NET Core 2.0 oder höher der [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) antiforgery Token in HTML-Formularelemente injiziert. Das folgende Markup in einer Razor-Datei wird automatisch antiforgery Token generiert:

```cshtml
<form method="post">
    ...
</form>
```

Mithilfe von Pull, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) antiforgery Token standardmäßig generiert, wenn die-Methode des Formulars GET nicht ist.

Die automatische Generierung von antiforgery Token für HTML-Formularelemente geschieht bei der `<form>` -Tag enthält die `method="post"` Attribut und eine der folgenden sind "true":

  * Das Action-Attribut ist leer (`action=""`).
  * Das Action-Attribut nicht angegeben (`<form method="post">`).

Automatische Generierung von antiforgery Token für HTML-Formularelemente kann deaktiviert werden:

* Explizit deaktivieren antiforgery-Token mit den `asp-antiforgery` Attribut:

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* Form-Elements ist entschieden-Out-of-Tag-Hilfsprogramme von unter Verwendung des Hilfsprogramms Tag [! Ausschlussverfahren Symbol](xref:mvc/views/tag-helpers/intro#opt-out):

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* Entfernen Sie die `FormTagHelper` aus der Sicht. Die `FormTagHelper` kann aus einer Sicht entfernt werden, indem Sie die folgende Anweisung an die Razor-Ansicht:

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> [Razor-Seiten](xref:mvc/razor-pages/index) vor XSRF/CSRF automatisch geschützt sind. Weitere Informationen finden Sie unter [XSRF/CSRF und Razor-Seiten](xref:mvc/razor-pages/index#xsrf).

Die am häufigsten verwendete Ansatz zum Schutz vor CSRF-Angriffen ist die Verwendung der *für die Domänensynchronisierung Tokenmuster* (STP). STP wird verwendet, wenn der Benutzer eine Seite mit den Formulardaten anfordert:

1. Der Server sendet ein Token, das die Identität des aktuellen Benutzers an den Client zugeordnet.
1. Der Client sendet das Token wieder an den Server für die Überprüfung.
1. Wenn der Server ein Token, die Identität des authentifizierten Benutzers übereinstimmt empfängt, wird die Anforderung abgelehnt.

Das Token ist eindeutig und unvorhersehbar. Das Token kann auch verwendet werden, um sicherzustellen, dass ordnungsgemäße Sequenzierung einer Reihe von Anforderungen (z. B. Sicherstellen der anforderungssequenz des: Seite 1 &ndash; Seite 2 &ndash; Seite 3). Alle Formulare in den Razor-Seiten und ASP.NET Core MVC-Vorlagen generiert antiforgery-Token. Die folgenden zwei Beispiele anzeigen generieren antiforgery Token:

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

Explizit ein antiforgery Token zum Hinzufügen einer `<form>` Element ohne das HTML-Hilfsobjekt Tag Hilfsprogramme mit [ @Html.AntiForgeryToken ](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

In jedem der vorangegangenen Fälle fügt ASP.NET Core ein ausgeblendetes Formularfeld folgt oder ähnlich:

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

ASP.NET Core umfasst drei [Filter](xref:mvc/controllers/filters) für die Arbeit mit antiforgery Token:

* [ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a>Antiforgery Optionen

Anpassen [antiforgery Optionen](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| Option | Beschreibung |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | Hängen die Einstellungen, die zum Erstellen der antiforgery Cookies verwendet. |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain) | Die Domäne des Cookies. Wird standardmäßig auf `null` festgelegt. Diese Eigenschaft ist veraltet und wird in einer zukünftigen Version entfernt. Die empfohlene Alternative ist Cookie.Domain. |
| [CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename) | Der Name des Cookies. Wenn nicht festgelegt, der vom System eine eindeutige Namen, die mit beginnt generiert die [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (". AspNetCore.Antiforgery."). Diese Eigenschaft ist veraltet und wird in einer zukünftigen Version entfernt. Die empfohlene Alternative ist Cookie.Name. |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath) | Der Pfad für das Cookie festgelegt. Diese Eigenschaft ist veraltet und wird in einer zukünftigen Version entfernt. Die empfohlene Alternative ist Cookie.Path. |
| [FormFieldName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | Der Name des Felds verborgene vom antiforgery System zum Rendern von antiforgery Token in Ansichten verwendet werden soll. |
| [HeaderName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | Der Name des Headers vom antiforgery System verwendet werden soll. Wenn `null`, das System berücksichtigt nur Formulardaten. |
| [RequireSsl](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | Gibt an, ob SSL vom antiforgery System erforderlich ist. Wenn `true`, nicht-SSL-Anforderungen fehl. Wird standardmäßig auf `false` festgelegt. Diese Eigenschaft ist veraltet und wird in einer zukünftigen Version entfernt. Die empfohlene Alternative ist die Cookie.SecurePolicy festgelegt. |
| [SuppressXFrameOptionsHeader](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | Gibt an, ob die Generierung von Unterdrücken der `X-Frame-Options` Header. Standardmäßig wird der Header mit einem Wert von "SAMEORIGIN" generiert. Wird standardmäßig auf `false` festgelegt. |

Weitere Informationen finden Sie unter [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).

## <a name="configure-antiforgery-features-with-iantiforgery"></a>Konfigurieren von antiforgery Funktionen mit IAntiforgery

[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) stellt die API zum Konfigurieren von antiforgery Funktionen bereit. `IAntiforgery` angefordert werden können, der `Configure` Methode der `Startup` Klasse. Im folgenden Beispiel wird die Middleware auf der Startseite der app antiforgery-Token generieren und senden es in der Antwort als Cookie (mithilfe der Angular Dateinamenskonvention weiter unten in diesem Thema beschrieben):

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a>Antiforgery-Überprüfung

[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) ist ein Aktionsfilter, die auf einer einzelnen Aktion, die einen Controller angewendet werden kann oder Global. Anforderungen für Aktionen, die dieser Filter angewendet haben werden blockiert, es sei denn, die Anforderung ein gültiges antiforgery Token enthält.

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

Die `ValidateAntiForgeryToken` Attribut erfordert ein Token für Anforderungen an die Aktionsmethoden, die es ausstattet, einschließlich HTTP GET-Anforderungen. Wenn die `ValidateAntiForgeryToken` Attribut über die app Controller angewendet wird, können sie mit außer Kraft gesetzt werden die `IgnoreAntiforgeryToken` Attribut.

> [!NOTE]
> ASP.NET Core unterstützt keine GET-Anforderungen automatisch antiforgery Token hinzugefügt werden.

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a>Überprüfen Sie automatisch antiforgery Token für unsichere nur HTTP-Methoden

ASP.NET Core apps erstellen keine antiforgery Token für die sichere HTTP-Methoden (GET, HEAD, Optionen und TRACE). Anstelle von Allgemein Anwenden der `ValidateAntiForgeryToken` Attribut und überschreiben diese mit `IgnoreAntiforgeryToken` Attribute, die [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) Attribut kann verwendet werden. Dieses Attribut funktioniert genauso wie die `ValidateAntiForgeryToken` Attribut, außer dass es keine Token für Anforderungen, die mit den folgenden HTTP-Methoden erforderlich:

* GET
* HEAD-,
* OPTIONEN
* TRACE

Wir empfehlen die Verwendung von `AutoValidateAntiforgeryToken` weitgefasst gilt dies für nicht-API-Szenarien. Dadurch wird sichergestellt, dass Aktionen nach standardmäßig geschützt werden. Die Alternative besteht darin, antiforgery Token standardmäßig ignoriert, es sei denn, `ValidateAntiForgeryToken` auf einzelner Aktionsmethoden angewendet wird. Es wahrscheinlicher ist in diesem Szenario für eine POST-Aktionsmethode, verbleiben soll, versehentlich ungeschützten Ort verlassen der app CSRF-Angriffe anfällig. Alle Einträge sollten das antiforgery Token gesendet werden.

APIs haben keinen automatischen Mechanismus zum Senden des nicht-Cookie-Teils des Tokens. Die Implementierung hängt wahrscheinlich die Client-Code-Implementierung. Einige Beispiele werden unten gezeigt:

Auf Klassenebene-Beispiel:

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

Globale Beispiel:

```csharp
services.AddMvc(options => 
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

### <a name="override-global-or-controller-antiforgery-attributes"></a>Überschreiben globaler oder antiforgery Attributen für controller

Die [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) Filter wird verwendet, um ein antiforgery Token für eine bestimmte Aktion (oder Domänencontroller) erforderlich. Beim Anwenden dieses Filters überschreibt `ValidateAntiForgeryToken` und `AutoValidateAntiforgeryToken` Filter, die auf einer höheren Ebene angegeben ist (Global oder auf einem Domänencontroller).

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a>Aktualisierungstoken nach der Authentifizierung

Token sollte aktualisiert werden, nachdem der Benutzer authentifiziert ist, durch Weiterleiten des Benutzers auf eine Sicht oder eine Seite mit Razor-Seiten.

## <a name="javascript-ajax-and-spas"></a>JavaScript, AJAX- und SPAs

In herkömmlichen HTML-basierte apps werden antiforgery Token an den Server mithilfe von ausgeblendeten Formularfeldern übergeben. Viele Anforderungen werden in modernen JavaScript-basierten apps und SPAs programmgesteuert vorgenommen. Diese AJAX-Anforderungen möglicherweise andere Techniken (z. B. Anforderungsheader oder Cookies) verwenden, um das Token zu senden.

Wenn Cookies zum Speichern von Authentifizierungstoken und zum Authentifizieren von API-Anforderungen auf dem Server verwendet werden, ist CSRF ein potenzielles Problem hin. Lokaler Speicher zum Speichern von Token verwendet wird, möglicherweise Sicherheitsrisiko FORGERY verringert werden, da die Werte aus dem lokalen Speicher automatisch mit jeder Anforderung an den Server gesendet werden nicht. Daher mit dem lokalen Speicher zum Speichern der antiforgery-Token auf dem Client und Senden des Tokens als ein Anforderungsheader und eine empfohlene Vorgehensweise ist.

### <a name="javascript"></a>JavaScript

Verwenden von JavaScript mit Sichten, kann das Token erstellt werden mithilfe eines Diensts aus innerhalb der Ansicht. Einfügen der [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) -Dienst in der Ansicht, und rufen [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):

[!code-csharp[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

Dadurch entfällt die Notwendigkeit so behandeln Sie direkt mit dem Festlegen von Cookies auf dem Server, oder sie vom Client gelesen werden.

Das obige Beispiel nutzt JavaScript zur Festlegung um den Wert des ausgeblendeten Felds für die AJAX-POST-Header zu lesen.

JavaScript kann auch Zugriffstoken in Cookies und das Cookie Inhalt zum Erstellen eines Headers mit dem Wert des Tokens verwenden.

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

Vorausgesetzt, das Skript zum Senden von Token in einer Kopfzeile namens fordert `X-CSRF-TOKEN`, konfigurieren Sie den antiforgery Dienst gesucht werden soll, für die `X-CSRF-TOKEN` Header:

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

Im folgende Beispiel nutzt JavaScript zur Festlegung eine AJAX-Anforderung mit den entsprechenden Header vornehmen:

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a>AngularJS

AngularJS verwendet stattdessen eine Konvention CSRF-Adresse. Wenn der Server ein Cookie mit dem Namen sendet `XSRF-TOKEN`, die AngularJS `$http` Dienst einen Header den Cookiewert hinzugefügt, wenn er eine Anforderung an den Server sendet. Dieser Prozess erfolgt automatisch. Der Header muss nicht explizit festgelegt werden. Der Headername wird `X-XSRF-TOKEN`. Der Server sollte dieser Header zu erkennen und seinen Inhalt zu überprüfen.

Arbeiten Sie mit dieser Konvention für ASP.NET Haupt-API:

* Ihre app so konfigurieren, geben Sie ein Token in einem Cookie aufgerufen `XSRF-TOKEN`.
* Konfigurieren Sie den antiforgery Dienst zum Suchen nach einem Header mit dem Namen `X-XSRF-TOKEN`.

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
```

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="extend-antiforgery"></a>Erweitern Sie antiforgery

Die [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) Typ ermöglicht Entwicklern das Verhalten des Anti-CSRF-Systems durch zusätzliche Round-Tripping-Daten in einzelnen Token zu erweitern. Die [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) Methode wird jedes Mal aufgerufen, wenn ein Token wird generiert, und der Rückgabewert in das generierte Token eingebettet ist. Eine Implementierung konnte einen Zeitstempel, eine Nonce oder ein anderer Wert zurückgeben, und rufen dann [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) dieser Daten validieren, wenn das Token überprüft wird. Der Client Benutzernamen ist bereits in die generierten Token eingebettet, daher keine Notwendigkeit besteht, diese Informationen umfassen. Wenn ein Token ergänzenden Daten, aber keine enthält `IAntiForgeryAdditionalDataProvider` wird konfiguriert, die zusätzlichen Daten wird nicht überprüft.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) auf [öffnen Webanwendungsprojekt Sicherheit](https://www.owasp.org/index.php/Main_Page) (OWASP).
