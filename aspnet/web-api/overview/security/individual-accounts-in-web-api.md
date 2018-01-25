---
uid: web-api/overview/security/individual-accounts-in-web-api
title: Sichern eine Web-API mit individuellen Konten und lokalen Anmeldenamen in der ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: In diesem Thema wird das Sichern einer Web-API, die mithilfe von "oauth2" zum Authentifizieren beim einer Mitgliedschaftsdatenbank veranschaulicht. Softwareversionen, in dem Lernprogramm Visual Studio 201 verwendet...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/15/2014
ms.topic: article
ms.assetid: 92c84846-f0ea-4b5e-94b6-5004874eb060
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/individual-accounts-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: e2056e769edf972cba830b31cf37f6418148ca73
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="secure-a-web-api-with-individual-accounts-and-local-login-in-aspnet-web-api-22"></a>Sichern einer Webs-API mit individuellen Konten und lokalen Anmeldenamen in der ASP.NET Web API 2.2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Beispiel-App herunterladen](https://github.com/MikeWasson/LocalAccountsApp)

> In diesem Thema wird das Sichern einer Web-API, die mithilfe von "oauth2" zum Authentifizieren beim einer Mitgliedschaftsdatenbank veranschaulicht.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013 Update 3](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - [Web API 2.2](../releases/whats-new-in-aspnet-web-api-22.md)
> - [ASP.NET Identity 2.1](../../../identity/index.md)


In Visual Studio 2013 bietet die Web-API-Projektvorlage drei Optionen für die Authentifizierung:

- **Einzelne Konten.** Die app verwendet eine Mitgliedschaftsdatenbank.
- **Organisationskonten.** Benutzer melden Sie sich mit ihren Azure Active Directory, Office 365 oder lokalen Active Directory-Anmeldeinformationen.
- **Windows-Authentifizierung.** Diese Option dient für Intranetanwendungen und verwendet das Modul für IIS für Windows-Authentifizierung.

Ausführliche Informationen zu diesen Optionen finden Sie unter [Erstellen von ASP.NET-Webprojekten in Visual Studio 2013](../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#auth).

Einzelkonten bieten zwei Möglichkeiten für einen Benutzer zum Anmelden an:

- **Lokale Anmeldung**. Der Benutzer registriert sich am Standort Eingabe eines Benutzernamens und Kennworts. Die app speichert den Hashwert des Kennworts in der Mitgliedschaftsdatenbank. Wenn der Benutzer anmeldet, überprüft das System ASP.NET Identity das Kennwort an.
- **Soziale Anmeldung**. Der Benutzer meldet sich mit einem externen Dienst wie Facebook, Microsoft oder Google. Die app noch erstellt einen Eintrag für den Benutzer in der Mitgliedschaftsdatenbank, aber keine Anmeldeinformationen nicht gespeichert. Der Benutzer authentifiziert sich der Anmeldung bei der externen Dienst.

Dieser Artikel beschäftigt sich mit dem Szenario für die lokale Anmeldung. Für lokale und sozialer Anmeldung verwendet Web-API "oauth2" zum Authentifizieren von Anforderungen an. Arbeitsabläufe Anmeldeinformationen unterscheiden sich jedoch für die Anmeldung bei lokalen und soziale.

In diesem Artikel zeige ich Ihnen eine einfache app, mit dem die Benutzer anmelden und zum Senden von authentifizierten AJAX-Aufrufe von einer Web-API. Sie können den Beispielcode herunterladen [hier](https://github.com/MikeWasson/LocalAccountsApp). Die Infodatei beschreibt, wie das Beispiel in Visual Studio erstellen.

[![](individual-accounts-in-web-api/_static/image2.png)](individual-accounts-in-web-api/_static/image1.png)

Die Beispiel-app verwendet Knockout.js für die Datenbindung und jQuery für das Senden von AJAX-Anforderungen. Ich werde auf AJAX-Aufrufe konzentrieren werden, daher brauchen Sie Knockout.js für diesen Artikel zu kennen.

Nebenbei werde ich beschreiben:

- Welche Vorgänge die app auf der Clientseite ausführt.
- Woran liegt das auf dem Server.
- Der HTTP-Datenverkehr in der Mitte aufweist.

Zunächst müssen wir einige Begriffe "oauth2" definieren.

- *Ressource*. Datenfragmente, die geschützt werden können.
- *Ressourcenserver*. Der Server, der die Ressource hostet.
- *Ressourcenbesitzer*. Die Entität, die über die Berechtigung zum Zugriff auf eine Ressource gewähren kann. (Üblicherweise der Benutzer.)
- *Client*: die app, die Zugriff auf die Ressource benötigt. In diesem Artikel wird der Client einen Webbrowser.
- *Zugriffstoken*. Ein Token, das Zugriff auf eine Ressource gewährt.
- *Trägertoken*. Einen bestimmten Typ von Zugriffstoken, mit der Eigenschaft an, dass jeder Benutzer auf das Token verwenden kann. Das heißt, erforderlich kein Client einen kryptografischen Schlüssel oder einem anderen geheimen Schlüssel verwenden ein trägertoken, das. Aus diesem Grund trägertoken sollte nur über eine HTTPS verwendet werden, und sollten relativ kurze Ablaufzeiten angegeben haben.
- *Autorisierungsserver*. Ein Server, der von Access-Token zu erhalten.

Eine Anwendung kann als autorisierungsserver und Ressourcenserver fungieren. Die Web-API-Projektvorlage folgt diesem Muster.

## <a name="local-login-credential-flow"></a>Lokale Anmeldeinformationen Anmeldeablauf

Für die lokale Anmeldung verwendet, Web-API verwendet die [Ressource Besitzer Kennwort Fluss](http://oauthlib.readthedocs.org/en/latest/oauth2/grants/password.html) in "oauth2" definiert.

1. Der Benutzer gibt einen Namen und ein Kennwort in den Client.
2. Der Client sendet diese Anmeldeinformationen an den autorisierungsserver.
3. Der autorisierungsserver authentifiziert die Anmeldeinformationen und gibt ein Zugriffstoken zurück.
4. Um eine geschützte Ressource zuzugreifen, fügt der Client das Zugriffstoken im Autorisierungsheader der HTTP-Anforderung.

![](individual-accounts-in-web-api/_static/image3.png)

Bei Auswahl **einzelkonten** in der Web-API-Projektvorlage enthält das Projekt einen autorisierungsserver, die Anmeldeinformationen des Benutzers überprüft und Token ausstellt. Das folgende Diagramm zeigt die gleichen Anmeldeinformationen Flusses im Hinblick auf Web-API-Komponenten.

![](individual-accounts-in-web-api/_static/image4.png)

In diesem Szenario werden Web-API-Controller als Ressourcenserver fungieren. Ein Authentifizierungsfilter überprüft Zugriffstoken, und die **[Authorize]** Attribut wird verwendet, um eine Ressource zu schützen. Wenn ein Controller bzw. die Aktionsmethode hat die **[Authorize]** -Attributs sind alle Anforderungen an diesem Controller oder die Aktion muss authentifiziert werden. Andernfalls die Autorisierung verweigert wird, und Web-API gibt Fehler 401 (nicht autorisiert).

Der autorisierungsserver und der Authentifizierungsfilter sowohl einen Aufruf an ein [OWIN-Middleware](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md) Komponente, die die Details von "oauth2" behandelt. Ich werde den Entwurf ausführlicher weiter unten in diesem Lernprogramm beschreiben.

## <a name="sending-an-unauthorized-request"></a>Eine nicht autorisierte Anforderung senden

Um zu beginnen, führen Sie die app, und klicken Sie auf die **API Aufrufen** Schaltfläche. Wenn die Anforderung abgeschlossen ist, sehen Sie eine Fehlermeldung in die **Ergebnis** Feld. Das ist, da die Anforderung ein Zugriffstokens nicht enthält, damit die Anforderung nicht autorisiert ist.

[![](individual-accounts-in-web-api/_static/image6.png)](individual-accounts-in-web-api/_static/image5.png)

Die **API Aufrufen** Schaltfläche sendet eine AJAX-Anforderung an ~/api/Werten, das eine Controlleraktion der Web-API aufruft. Hier ist der Abschnitt der JavaScript-Code, der die AJAX-Anforderung sendet. In der Beispiel-app alle von der JavaScript-app-Code befindet sich in der Datei Scripts\app.js.

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample1.js)]

Bis der Benutzer anmeldet, gibt es keine Bearer-Token und daher keine Authorization-Header in der Anforderung. Dies bewirkt, dass die Anforderung 401-Fehler zurückgegeben.

Hier ist die HTTP-Anforderung. (Verwendet [Fiddler](http://www.telerik.com/fiddler) zum Erfassen des HTTP-Datenverkehrs.)

[!code-console[Main](individual-accounts-in-web-api/samples/sample2.cmd)]

HTTP-Antwort:

[!code-console[Main](individual-accounts-in-web-api/samples/sample3.cmd?highlight=1,4)]

Beachten Sie, dass die Antwort enthält den Www-Authentifizierungsheader die Herausforderung, die auf Träger festgelegt. Wert, der angibt, dass der Server ein trägertoken, das erwartet.

## <a name="register-a-user"></a>Einen Benutzer registrieren

In der **registrieren** Abschnitt der Anwendung, ein e-Mail- und ein Kennwort einzugeben, und klicken Sie auf die **registrieren** Schaltfläche.

Sie müssen eine gültige e-Mail-Adresse für dieses Beispiel verwenden, aber eine wirkliche app wäre die Adresse bestätigen. (Siehe [erstellen Sie eine sichere ASP.NET MVC 5-Web-app mit anmelden, e-Mail-Bestätigung und das Kennwort zurücksetzen](../../../mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md).) Verwenden Sie für das Kennwort etwa "Password1!", mit einem Großbuchstaben, Kleinbuchstaben, Anzahl und nicht-alphanumerische Zeichen. Um die app einfach zu halten, ich die clientseitige Validierung ausgelassen, wenn ein Problem mit dem Kennwortformat vorliegt, Sie einen Fehler 400 (Ungültige Anforderung erhalten).

[![](individual-accounts-in-web-api/_static/image8.png)](individual-accounts-in-web-api/_static/image7.png)

Die **registrieren** Schaltfläche sendet eine POST-Anforderung an ~/api/Account/Register /. Der Anforderungstext ist ein JSON-Objekt, das den Namen und das Kennwort enthält. Hier ist die JavaScript-Code, der die Anforderung sendet:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample4.js)]

HTTP-Anforderung:

[!code-console[Main](individual-accounts-in-web-api/samples/sample5.cmd?highlight=5,10)]

HTTP-Antwort:

[!code-console[Main](individual-accounts-in-web-api/samples/sample6.cmd)]

Diese Anforderung erfolgt durch die `AccountController` Klasse. Intern `AccountController` ASP.NET Identity verwendet, um die Mitgliedschaftsdatenbank zu verwalten.

Wenn Sie die app lokal von Visual Studio ausführen, werden die Benutzerkonten in LocalDB, in der Tabelle AspNetUsers gespeichert. Um die Tabellen in Visual Studio anzuzeigen, klicken Sie auf die **Ansicht** klicken Sie im Menü **Server-Explorer**, erweitern Sie dann **Datenverbindungen**.

![](individual-accounts-in-web-api/_static/image9.png)

## <a name="get-an-access-token"></a>Abrufen eines Zugriffstokens

Bisher haben wir keine OAuth ausgeführt, aber nun können wir die OAuth-autorisierungsserver in Aktion, anzeigen, wenn wir eines Zugriffstokens anfordern. In der **anmelden** Clientbereich der Beispiel-app, geben Sie den e-Mail- und das Kennwort, und klicken Sie auf **anmelden**.

[![](individual-accounts-in-web-api/_static/image11.png)](individual-accounts-in-web-api/_static/image10.png)

Die **anmelden** Schaltfläche sendet eine Anforderung an den tokenendpunkt hinsichtlich. Der Anforderungstext enthält die folgenden Formular Url-codierte Daten:

- Erteilen Sie\_Typ: "Kennwort"
- Benutzername: &lt;die e-Mail des Benutzers&gt;
- Kennwort: &lt;Kennwort&gt;

Hier ist die JavaScript-Code, der die AJAX-Anforderung sendet:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample7.js?highlight=14)]

Wenn die Anforderung erfolgreich ist, gibt der autorisierungsserver Zugriffstoken im Antworttext zurück. Beachten Sie, dass wir das Token im Sitzungsspeicher, zur späteren Verwendung beim Senden von Anforderungen an die API zu speichern. Im Gegensatz zu einige Formen der Authentifizierung (z. B. Cookie-basierte Authentifizierung) wird der Browser nicht automatisch das Zugriffstoken in nachfolgenden Anforderungen hinzugefügt. Die Anwendung müssen manuell. Die gut ist, da sie beschränkt [CSRF Sicherheitsrisiken](preventing-cross-site-request-forgery-csrf-attacks.md).

HTTP-Anforderung:

[!code-console[Main](individual-accounts-in-web-api/samples/sample8.cmd?highlight=5,10)]

Sie sehen, dass die Anforderung die Anmeldeinformationen des Benutzers enthält. Sie *müssen* HTTPS verwenden, um TLS bereitzustellen.

HTTP-Antwort:

[!code-console[Main](individual-accounts-in-web-api/samples/sample9.cmd?highlight=8)]

Zur besseren Lesbarkeit ich eingezogen JSON und das Zugriffstoken, also ein sehr langes abgeschnitten.

Die `access_token`, `token_type`, und `expires_in` Eigenschaften werden durch die OAuth2-Spezifikation definiert. Die anderen Eigenschaften (`userName`, `.issued`, und `.expires`) sind nur zu Informationszwecken. Sie finden den Code, der diese zusätzlichen Eigenschaften in fügt die `TokenEndpoint` -Methode, in der Datei /Providers/ApplicationOAuthProvider.cs.

## <a name="send-an-authenticated-request"></a>Eine authentifizierte Anforderung senden

Nun, dass wir ein trägertoken verfügen, können wir eine authentifizierte Anforderung an die API vornehmen. Dies erfolgt durch Festlegen des Authorization-Headers in der Anforderung. Klicken Sie auf die **API Aufrufen** Schaltfläche erneut aus, um dies zu sehen.

[![](individual-accounts-in-web-api/_static/image13.png)](individual-accounts-in-web-api/_static/image12.png)

HTTP-Anforderung:

[!code-console[Main](individual-accounts-in-web-api/samples/sample10.cmd?highlight=5)]

HTTP-Antwort:

[!code-console[Main](individual-accounts-in-web-api/samples/sample11.cmd)]

## <a name="log-out"></a>Melden Sie sich ab

Da der Browser die Anmeldeinformationen oder das Zugriffstoken nicht zwischengespeichert, ist weiterhin Abmelden einfach "vergessen" das Token durch das Entfernen aus der Sitzungsspeicher:

[!code-javascript[Main](individual-accounts-in-web-api/samples/sample12.js)]

## <a name="understanding-the-individual-accounts-project-template"></a>Grundlegendes zu Einzelkonten-Projektvorlage

Bei Auswahl **Einzelkonten** in der Projektvorlage für ASP.NET Web-Anwendung, die das Projekt enthält:

- Ein autorisierungsserver für die "oauth2".
- Eine Web-API-Endpunkt für die Verwaltung von Benutzerkonten
- Ein EF-Modell zum Speichern von Benutzerkonten.

Hier sind die Hauptassembly der Anwendung-Klassen, die diese Funktionen zu implementieren:

- `AccountController` Bietet einen Web-API-Endpunkt für die Verwaltung von Benutzerkonten an. Die `Register` Aktion ist die einzige, die wir in diesem Lernprogramm verwendet. Andere Methoden für die Klasse unterstützt das Zurücksetzen des Kennworts, soziale Anmeldungen und andere Funktionen.
- `ApplicationUser`, definiert in /Models/IdentityModels.cs. Diese Klasse ist der EF-Modell für Benutzerkonten in der Mitgliedschaftsdatenbank.
- `ApplicationUserManager`, definiert in /App\_Start/IdentityConfig.cs, die diese Klasse leitet sich von [UserManager](https://msdn.microsoft.com/library/dn613290.aspx) und führt Vorgänge für Benutzerkonten, z. B. Erstellen eines neuen Benutzers, Kennwörter usw., überprüfen und automatisch weiterhin besteht Änderungen an der Datenbank.
- `ApplicationOAuthProvider` Dieses Objekt wird in der OWIN-Middleware integriert und verarbeitet Ereignisse, die von der Middleware ausgelöst. Er leitet sich von [OAuthAuthorizationServerProvider](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserverprovider.aspx).

![](individual-accounts-in-web-api/_static/image14.png)

### <a name="configuring-the-authorization-server"></a>Konfigurieren des Autorisierungsservers

Der folgende Code konfiguriert StartupAuth.cs den autorisierungsserver "oauth2".

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample13.cs)]

Die `TokenEndpointPath` Eigenschaft ist für den URL-Pfad, an den Server-autorisierungsendpunkt. Dies ist die URL dieser app verwendet, um die Bearer-Token abzurufen.

Die `Provider` Eigenschaft gibt einen Anbieter, wird in der OWIN-Middleware integriert, und Ereignisse, die ausgelöst wird, von der Middleware verarbeitet.

Hier wird der grundsätzliche Datenfluss auf, wenn möchte, dass die app ein Token abzurufen:

1. Um ein Zugriffstoken zu erhalten, die app sendet eine Anforderung zum ~ / Token.
2. Ruft die OAuth-Middleware `GrantResourceOwnerCredentials` beim Anbieter.
3. Der Anbieter Ruft die `ApplicationUserManager` So überprüfen die Anmeldeinformationen und erstellen eine anspruchsidentität.
4. Wenn dies erfolgreich ist, erstellt der Anbieter ein Authentifizierungsticket, die zum Generieren des Tokens verwendet wird.

[![](individual-accounts-in-web-api/_static/image16.png)](individual-accounts-in-web-api/_static/image15.png)

Die OAuth-Middleware weiß nicht, alles zu den Benutzerkonten. Der Anbieter die Kommunikation zwischen der Middleware und ASP.NET Identity. Weitere Informationen zum Implementieren des autorisierungsservers finden Sie unter [OWIN OAuth 2.0-Autorisierungsserver](../../../aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server.md).

### <a name="configuring-web-api-to-use-bearer-tokens"></a>Konfigurieren von Web-API zur Verwendung von Trägertoken

In der `WebApiConfig.Register` -Methode, der dem folgenden Code wird eine Authentifizierung für die Web-API-Pipeline:

[!code-csharp[Main](individual-accounts-in-web-api/samples/sample14.cs)]

Die **HostAuthenticationFilter** Klasse ermöglicht die Authentifizierung mithilfe von Bearer-Token.

Die **SuppressDefaultHostAuthentication** Methode teilt Web-API, keinerlei Authentifizierung ignoriert werden sollen, das ausgeführt wird, bevor die Anforderung die Web-API-Pipeline von IIS oder OWIN-Middleware erreicht hat. Auf diese Weise können wir Web-API, die Authentifizierung nur mit trägertoken einschränken.

> [!NOTE]
> Insbesondere kann der MVC-Teil der app Formularauthentifizierung verwendet die Anmeldeinformationen in einem Cookie gespeichert. Cookie-basierte Authentifizierung erfordert die Verwendung von fälschungssicherheitstoken zum Verhindern von CSRF-Angriffen. Das ist ein Problem für Web-APIs, da es gibt keine praktische Möglichkeit für die Web-API das fälschungssicherheitstoken an den Client zu senden. (Weitere Hintergrundinformationen zu diesem Problem finden Sie unter [verhindern von CSRF-Angriffen in Web-API](preventing-cross-site-request-forgery-csrf-attacks.md).) Aufrufen von **SuppressDefaultHostAuthentication** wird sichergestellt, dass Web-API nicht anfällig für CSRF-Angriffen von Anmeldeinformationen in Cookies gespeichert ist.


Wenn der Client eine geschützte Ressource anfordert, geschieht in der Web-API-Pipeline:

1. Die **HostAuthentication** Filter Ruft die OAuth-Middleware, um das Token zu überprüfen.
2. Die Middleware konvertiert das Token in eine anspruchsidentität.
3. Die Anforderung an diesem Punkt ist *authentifiziert* , aber nicht *autorisiert*.
4. Der Autorisierungsfilter untersucht die anspruchsidentität. Wenn die Ansprüche den Benutzer für diese Ressource zu autorisieren, wird die Anforderung autorisiert. Wird standardmäßig die **[Authorize]** Attribut wird jede Anforderung, die authentifiziert wird autorisieren. Allerdings können Sie die Rolle oder von anderen Ansprüche autorisieren. Weitere Informationen finden Sie unter [Authentifizierung und Autorisierung in Web-API](authentication-and-authorization-in-aspnet-web-api.md).
5. Wenn die vorherigen Schritte erfolgreich sind, gibt der Controller die geschützte Ressource zurück. Andernfalls empfängt der Client Fehler 401 (nicht autorisiert).

[![](individual-accounts-in-web-api/_static/image18.png)](individual-accounts-in-web-api/_static/image17.png)

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [ASP.NET Identity](../../../identity/index.md)
- [Grundlegendes zu Sicherheitsfunktionen in der SPA-Vorlage für VS2013 RC](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx). MSDN Blogbeitrag von Hongye Sun.
- [Unterzogen, wodurch der Web-API einzelnen Konten Vorlage – Teil 2: lokalen Konten](http://leastprivilege.com/2013/11/26/dissecting-the-web-api-individual-accounts-templatepart-2-local-accounts/). Der Blogbeitrag von Dominick Baier.
- [Hosten von Authentifizierung und Web-API mit OWIN](http://brockallen.com/2013/10/27/host-authentication-and-web-api-with-owin-and-active-vs-passive-authentication-middleware/). Einer guten Erklärung `SuppressDefaultHostAuthentication` und `HostAuthenticationFilter` von Brock Abläufe.
- [Anpassen von Profilinformationen in ASP.NET Identity in Visual Studio 2013 Vorlagen](https://blogs.msdn.com/b/webdev/archive/2013/10/16/customizing-profile-information-in-asp-net-identity-in-vs-2013-templates.aspx). MSDN-Blogbeitrag von Pranav Rastogi.
- [Pro Anforderung Prozesslebensdauer-Verwaltung für UserManager-Klasse in ASP.NET Identity](https://blogs.msdn.com/b/webdev/archive/2014/02/12/per-request-lifetime-management-for-usermanager-class-in-asp-net-identity.aspx). MSDN-Blogbeitrag von Suhas Joshi, mit einer guten Erklärung der `UserManager` Klasse.
