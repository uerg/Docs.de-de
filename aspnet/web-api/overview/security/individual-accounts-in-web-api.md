---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Schützen eine Web-API mit einzelnen Konten und lokale Anmeldung in ASP.NET-Web-API 2.2 | Microsoft-Dokumentation
author: MikeWasson
description: In diesem Thema wird das Sichern einer Web-API, authentifizieren eine Mitgliedschaftsdatenbank mithilfe von OAuth2 veranschaulicht. Die Softwareversionen, die in den Tutorials Visual Studio 201 verwendet...
ms.author: aspnetcontent
ms.date: 10/15/2014
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 0a520333492a60014f7e9f9182a16f0ce514ba1d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842084"
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Schützen einer Webs-API mit einzelnen Konten und lokale Anmeldung in ASP.NET-Web-API 2.2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Beispiel-App herunter](https://github.com/MikeWasson/LocalAccountsApp)

> In diesem Thema wird das Sichern einer Web-API, authentifizieren eine Mitgliedschaftsdatenbank mithilfe von OAuth2 veranschaulicht.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Web-API 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)


In Visual Studio 2013 bietet die Web-API-Projektvorlage aus drei Optionen für die Authentifizierung:

- **Individuelle Konten.** Die app verwendet eine Mitgliedschaftsdatenbank.
- **Organisationskonten.** Benutzer melden Sie sich mit ihren Azure Active Directory, Office 365 oder einer lokalen Active Directory-Anmeldeinformationen.
- **Windows-Authentifizierung.** Diese Option dient für Intranetanwendungen und verwendet das Windows-Authentifizierung-IIS-Modul.

Weitere Informationen zu diesen Optionen finden Sie unter [Erstellen von ASP.NET-Webprojekten in Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Einzelkonten bieten zwei Möglichkeiten, die für einen Benutzer für die Anmeldung:

- **Lokale Anmeldung**. Der Benutzer registriert sich am Standort Eingabe eines Benutzernamens und Kennworts. Die app speichert den Hashwert des Kennworts in der Mitgliedschaftsdatenbank. Wenn der Benutzer anmeldet, überprüft das ASP.NET Identity-System das Kennwort an.
- **Anmeldung für soziale Netzwerke**. Der Benutzer meldet sich mit einem externen Dienst, z. B. Facebook, Microsoft oder Google. Die app weiterhin erstellt einen Eintrag für den Benutzer in der Mitgliedschaftsdatenbank, aber keine Anmeldeinformationen nicht gespeichert. Der Benutzer authentifiziert, indem Sie den externen Dienst anmelden.

Dieser Artikel befasst sich das Szenario für die lokale Anmeldung. Für lokale und soziale Anmeldung verwendet Web-API "oauth2" zum Authentifizieren von Anforderungen an. Die Datenflüsse Anmeldeinformationen unterscheiden sich jedoch für lokale und soziale Anmeldung.

In diesem Artikel zeige ich eine einfache app, mit dem den Benutzer anmelden und zum Senden von authentifizierten AJAX-Aufrufe an eine Web-API. Sie können den Beispielcode herunterladen [hier](https://github.com/MikeWasson/LocalAccountsApp). Die Infodatei beschreibt, wie Sie das Beispiel von Grund auf neu in Visual Studio zu erstellen.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

Die Beispiel-app verwendet "Knockout.js" für die Datenbindung und jQuery für AJAX-Anforderungen senden. Ich werde auf die AJAX-Aufrufe, konzentrieren, sodass Sie nicht "Knockout.js" für diesen Artikel kennen müssen.

Dabei werde ich Folgendes beschreiben:

- Was die app auf der Clientseite ausführt.
- Woran liegt das auf dem Server.
- Der HTTP-Datenverkehr in der Mitte.

Zunächst müssen wir einige OAuth2-Terminologie zu definieren.

- *Ressource*. Teil der Daten, die geschützt werden können.
- *Ressourcenserver*. Der Server, der die Ressource hostet.
- *Ressourcenbesitzer*. Die Entität, die über die Berechtigung zum Zugriff auf eine Ressource erteilen kann. (Dies ist üblicherweise der Benutzer.)
- *Client*: die app, die auf die Ressource zugreifen möchte. In diesem Artikel ist der Client ein Webbrowser.
- *Zugriffstoken*. Ein Token, die Zugriff auf eine Ressource.
- *Bearertoken*. Einen bestimmten Typ von Zugriffstoken, mit der Eigenschaft an, dass jemand das Token verwenden kann. Ein Client benötigen nicht in anderen Worten: ein kryptografischer Schlüssel oder anderen geheimen Schlüssel ein trägertoken, das mit. Aus diesem Grund Bearer-Tokens sollte nur über eine HTTPS verwendet werden, und benötigen relativ kurze Ablaufzeiten.
- *Autorisierungsserver*. Ein Server, der von Access-Token zu erhalten.

Eine Anwendung kann als autorisierungsserver und Ressourcenserver fungieren. Die Web-API-Projektvorlage folgt diesem Muster.

## <a name="local-login-credential-flow"></a>Lokale Anmeldung mit Clientanmeldeinformationen

Für die lokale Anmeldung verwendet, Web-API verwendet die [ressourcenbesitzerflow Kennwort](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) in "oauth2" definiert.

1. Der Benutzer gibt eines Namens und Kennworts in den Client.
2. Der Client sendet diese Anmeldeinformationen an den autorisierungsserver.
3. Der autorisierungsserver authentifiziert die Anmeldeinformationen und gibt ein Zugriffstoken zurück.
4. Um eine geschützte Ressource zuzugreifen, fügt der Client das Zugriffstoken im Autorisierungsheader der HTTP-Anforderung.

![](individual-accounts-in-web-api/_static/image3.png)

Bei der Auswahl **einzelkonten** in der Web-API-Projektvorlage enthält das Projekt einen autorisierungsserver, die Anmeldeinformationen des Benutzers überprüft und Token ausstellt. Das folgende Diagramm zeigt den gleichen Anmeldeinformationen Flow in Form von Web-API.

![](individual-accounts-in-web-api/_static/image4.png)

In diesem Szenario werden Web-API-Controller als Ressourcenserver fungieren. Ein Authentifizierungsfilter überprüft das Zugriffstoken, und die **[Authorize]** Attribut wird verwendet, um eine Ressource zu schützen. Wenn ein Controller bzw. die Aktionsmethode hat die **[Authorize]** Attribut, alle Anforderungen an diesen Controller oder die Aktion muss authentifiziert werden. Andernfalls wird die Autorisierung verweigert, und Web-API-Fehler 401 (nicht autorisiert) zurückgibt.

Der autorisierungsserver und der Authentifizierungsfilter, die beide einen Aufruf an ein [OWIN-Middleware](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) Komponente, die Details der "oauth2" verarbeitet. Ich beschreibe später in diesem Tutorial das Design im Detail.

## <a name="sending-an-unauthorized-request"></a>Senden eine nicht autorisierte Anforderung

Klicken Sie zum Einstieg, führen Sie die app, und klicken Sie auf die **-API Aufrufen** Schaltfläche. Wenn die Anforderung abgeschlossen ist, sollte eine Fehlermeldung in die **Ergebnis** Feld. Das ist da die Anforderung ein Zugriffstoken nicht enthält, damit die Anforderung nicht autorisiert ist.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

Die **-API Aufrufen** Schaltfläche sendet eine AJAX-Anforderung an ~/api/Werten, die eine Controlleraktion der Web-API aufruft. Hier ist der Teil der JavaScript-Code, der die AJAX-Anforderung sendet. In der Beispiel-app alle von der JavaScript-app-Code befindet sich in der Datei Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Bis der Benutzer anmeldet, ist es kein bearertoken und daher keine Authorization-Header in der Anforderung. Dies bewirkt, dass die Anforderung ein 401-Fehler zurückgegeben.

Hier ist die HTTP-Anforderung. (Ich verwendet habe [Fiddler](http://www.telerik.com/fiddler) den HTTP-Datenverkehr zu erfassen.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

HTTP-Antwort:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Beachten Sie, dass die Antwort enthält den Www-Authentifizierungsheader der Herausforderung, die auf den Bearer festgelegt. Der angibt, dass der Server ein bearertoken erwartet.

## <a name="register-a-user"></a>Registrieren Sie einen Benutzer

In der **registrieren** Abschnitt der app, geben Sie einen e-Mail-Adresse und ein Kennwort ein, und klicken Sie auf die **registrieren** Schaltfläche.

Sie müssen eine gültige e-Mail-Adresse für dieses Beispiel verwenden, aber eine echte app würde die Adresse zu bestätigen. (Finden Sie unter [erstellen eine sichere ASP.NET MVC 5-Web-app mit Anmeldung, e-Mail-Bestätigung und kennwortzurücksetzung](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) Verwenden Sie für das Kennwort etwa "Kennwort1!", mit einem Großbuchstaben, Kleinbuchstaben, Anzahl und nicht alphanumerische Zeichen. Um die app einfach zu halten, ausgelassen ich die clientseitige Validierung, daher liegt ein Problem mit dem Kennwortformat vor, Sie Fehlercode 400 (Ungültige Anforderung) erhalten.

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

Die **registrieren** Schaltfläche sendet eine POST-Anforderung an ~/api/Account/Register /. Der Anforderungstext ist eine jsonobjekt, das den Namen und das Kennwort enthält. Hier ist der JavaScript-Code, der die Anforderung sendet:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

HTTP-Anforderung:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

HTTP-Antwort:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Diese Anforderung erfolgt durch die `AccountController` Klasse. Intern `AccountController` verwendet ASP.NET Identity zum Verwalten der Mitgliedschaftsdatenbank.

Wenn Sie die app lokal aus Visual Studio ausführen, werden die Benutzerkonten in LocalDB, in der Tabelle "aspnetusers" gespeichert. Um die Tabellen in Visual Studio anzuzeigen, klicken Sie auf die **Ansicht** , wählen Sie im Menü **Server-Explorer**, erweitern Sie dann **Datenverbindungen**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Abrufen eines Zugriffstokens

Bisher haben wir keine OAuth fertig, aber nun sehen, dass der OAuth-autorisierungsserver in Aktion, wenn es sich um ein Zugriffstoken anfordern. In der **anmelden** Bereich der Beispiel-app, geben Sie den e-Mail-Adresse und das Kennwort, und klicken Sie auf **anmelden**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

Die **anmelden** Schaltfläche sendet eine Anforderung an den token-Endpunkt. Der Hauptteil der Anforderung enthält die folgenden Formular-Url-codierte Daten:

- Erteilen Sie\_Typ: "Password"
- Benutzername: &lt;e-Mail des Benutzers&gt;
- Kennwort: &lt;Kennwort&gt;

Hier ist der JavaScript-Code, der die AJAX-Anforderung sendet:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Wenn die Anforderung erfolgreich ist, gibt der autorisierungsserver ein Zugriffstoken im Antworttext zurück. Beachten Sie, dass wir das Token in Sitzung Storage zur späteren Verwendung beim Senden von Anforderungen an die API gespeichert werden. Im Gegensatz zu einige Formen der Authentifizierung (z. B. Cookie-basierte Authentifizierung) wird der Browser nicht automatisch das Zugriffstoken in nachfolgenden Anforderungen hinzugefügt. Die Anwendung müssen manuell. Das ist eine gute Sache, da er begrenzt [CSRF-Sicherheitsrisiken](preventing-cross-site-request-forgery-csrf-attacks.md).

HTTP-Anforderung:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

Sie können sehen, dass die Anforderung die Anmeldeinformationen des Benutzers enthält. Sie *müssen* HTTPS TLS zu verwenden.

HTTP-Antwort:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Zur besseren Lesbarkeit, wenn ich den JSON-Code mit Einzug dargestellt und das Zugriffstoken, das eine ziemlich lang ist abgeschnitten.

Die `access_token`, `token_type`, und `expires_in` Eigenschaften durch OAuth2-Spezifikation definiert sind. Die anderen Eigenschaften (`userName`, `.issued`, und `.expires`) sind nur zu Informationszwecken. Sie finden den Code, der diese zusätzlichen Eigenschaften in fügt die `TokenEndpoint` -Methode, in der Datei /Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Eine authentifizierte Anforderung senden

Nun, da wir ein bearertoken verfügen, können wir eine authentifizierte Anforderung an die API vornehmen. Dies erfolgt durch Festlegen des Authorization-Headers in der Anforderung. Klicken Sie auf die **-API Aufrufen** Schaltfläche erneut aus, um dies zu sehen.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

HTTP-Anforderung:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

HTTP-Antwort:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Melden Sie sich ab

Da der Browser die Anmeldeinformationen oder das Zugriffstoken nicht zwischengespeichert ist, ist das Abmelden lediglich "vergessen" das Token, indem Sie ihn aus dem Sitzungsspeicher entfernen:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Verstehen der Einzelkonten-Projektvorlage

Bei der Auswahl **Einzelkonten** in der ASP.NET Web Application-Projektvorlage, die das Projekt enthält:

- Ein Server für den OAuth2-Autorisierung.
- Eine Web-API-Endpunkt für die Verwaltung von Benutzerkonten
- Ein EF-Modell zum Speichern von Benutzerkonten.

Hier sind die hauptanwendung-Klassen, die diese Funktionen zu implementieren:

- `AccountController` Stellt einen Web-API-Endpunkt für die Verwaltung von Benutzerkonten bereit. Die `Register` Aktion ist die einzige, die wir in diesem Tutorial verwendet. Andere Methoden für die Klasse unterstützt das Zurücksetzen von Kennwörtern, Anmeldungen per sozialem Netzwerk und andere Funktionen.
- `ApplicationUser`, in /Models/IdentityModels.cs definiert. Diese Klasse ist das EF-Modell für Benutzerkonten in der Mitgliedschaftsdatenbank.
- `ApplicationUserManager`, definiert in/app\_Start/IdentityConfig.cs, die diese Klasse von wird [UserManager](https://msdn.microsoft.com/library/dn613290.aspx) führt Vorgänge für Benutzerkonten, z. B. Erstellen eines neuen Benutzers, und Überprüfen von Kennwörtern und So weiter, und automatisch beibehalten. Änderungen an der Datenbank.
- `ApplicationOAuthProvider` Dieses Objekt wird in die OWIN-Middleware integriert und von der Middleware ausgelösten Ereignisse verarbeitet. Es leitet sich von [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Konfigurieren des Autorisierungsservers

In StartupAuth.cs konfiguriert der folgende Code den autorisierungsserver "oauth2".

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

Die `TokenEndpointPath` -Eigenschaft ist die URL-Pfad, an den autorisierungsendpunkt-Server. Dies ist die URL die app verwendet, um das bearertoken zu erhalten.

Die `Provider` Eigenschaft gibt an, einen Anbieter, der integriert wird, in die OWIN-Middleware und verarbeitet Ereignisse, die von der Middleware ausgelöst.

Hier ist der grundlegende Ablauf bei möchte, dass die app ein Token zu erhalten:

1. Um ein Zugriffstoken zu erhalten, die app sendet eine Anforderung zum ~ / Token.
2. Ruft die OAuth-Middleware `GrantResourceOwnerCredentials` für den Anbieter.
3. Der Anbieter Ruft die `ApplicationUserManager` die Anmeldeinformationen zu überprüfen, und erstellen eine anspruchsidentität.
4. Wenn dies erfolgreich ist, erstellt der Anbieter ein Authentifizierungsticket, die zum Generieren des Tokens verwendet wird.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

Die OAuth-Middleware weiß nicht, alles zu den Benutzerkonten. Der Anbieter kommuniziert zwischen dem Middleware und ASP.NET Identity. Weitere Informationen über die autorisierungsserver Implementierung finden Sie unter [OWIN OAuth 2.0-Autorisierungsserver](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Konfigurieren von Web-API mit Bearer-Tokens

In der `WebApiConfig.Register` -Methode, der folgende Code legt fest, um die Authentifizierung für die Web-API-Pipeline:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

Die **auch namensbasiert** Klasse ermöglicht die Authentifizierung mithilfe von Bearer-Tokens.

Die **SuppressDefaultHostAuthentication** Methode weist die Web-API-Authentifizierung zu ignorieren, die erfolgt, bevor die Anforderung die Web-API-Pipeline, die von IIS oder von OWIN-Middleware erreicht hat. Auf diese Weise können wir die Web-API für die Authentifizierung nur mit Bearer-Tokens einschränken.

> [!NOTE]
> Insbesondere kann der MVC-Teil der app Formularauthentifizierung verwendet die Anmeldeinformationen in einem Cookie gespeichert. Cookiebasierte Authentifizierung erfordert die Verwendung von fälschungssicherheitstoken, um CSRF-Angriffe zu verhindern. Das ist ein Problem für Web-APIs, da es gibt keine praktische Möglichkeit für die Web-API das fälschungssicherheitstoken an den Client zu senden. (Weitere Informationen zu diesem Problem finden Sie unter [verhindern von CSRF-Angriffen in Web-API-](preventing-cross-site-request-forgery-csrf-attacks.md).) Aufrufen von **SuppressDefaultHostAuthentication** wird sichergestellt, dass die Web-API nicht von den Anmeldeinformationen in Cookies gespeicherten CSRF-Angriffe anfällig ist.


Wenn der Client eine geschützte Ressource anfordert, hier geschieht in der Web-API-Pipeline:

1. Die **HostAuthentication** Filter Ruft die OAuth-Methode, um das Token zu überprüfen.
2. Die Middleware konvertiert das Token in eine anspruchsidentität.
3. Die Anforderung an diesem Punkt ist *authentifiziert* , nicht jedoch *autorisiert*.
4. Der Autorisierungsfilter untersucht die anspruchsidentität. Wenn die Ansprüche den Benutzer für diese Ressource zu autorisieren, wird die Anforderung autorisiert. In der Standardeinstellung die **[Authorize]** Attribut wird jede Anforderung, die authentifiziert werden autorisiert. Allerdings können Sie die Rolle oder von anderen Ansprüchen autorisieren. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung in Web-API-](authentication-and-authorization-in-aspnet-web-api.md).
5. Wenn die vorherigen Schritte erfolgreich sind, gibt der Controller die geschützte Ressource zurück. Andernfalls erhält der Client Fehler 401 (nicht autorisiert).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [ASP.NET Identity](../../../identity/index.md)
- [Grundlegendes zu Sicherheitsfunktionen in der SPA-Vorlage für VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). MSDN-Blog-Beitrag von Hongye Sun.
- [Analyse der einzelnen Web-API-Konten Vorlage – Teil 2: lokale Konten](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Der Blogbeitrag von Dominick Baier.
- [Hosten von Authentifizierungs- und Web-API mit OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Eine gute Erklärung dafür `SuppressDefaultHostAuthentication` und `HostAuthenticationFilter` von Brock Allen.
- [Anpassen von Profilinformationen in ASP.NET Identity in Visual Studio 2013-Vorlagen](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). MSDN-Blogbeitrag von Pranav Rastogi.
- [Pro Anforderung Prozesslebensdauer-Verwaltung für UserManager-Klasse in ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). MSDN-Blogbeitrag von Suhas Joshi, mit einer guten Erklärung der `UserManager` Klasse.
