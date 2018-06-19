---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Owin-OAuth 2.0-Autorisierungsserver | Microsoft Docs
author: hongyes
description: Dieses Lernprogramm führt Sie zum Implementieren von einem OAuth 2.0-Autorisierungsserver mithilfe von OAuth OWIN-Middleware. Dies ist ein erweitertes Lernprogramm, nur Outlin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/20/2014
ms.topic: article
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: e5968f8d19191c3f44e9bd58f8e22a39d8d8faff
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876552"
---
<a name="owin-oauth-20-authorization-server"></a>OWIN OAuth 2.0 Authorization Server
====================
durch [Hongye Sun](https://github.com/hongyes), [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)

> Dieses Lernprogramm führt Sie zum Implementieren von einem OAuth 2.0-Autorisierungsserver mithilfe von OAuth OWIN-Middleware. Dies ist eine erweiterte Lernprogramm, in dem nur die Schritte zum Erstellen einer owin-OAuth 2.0 Authorization-Server werden erläutert. Dies ist kein Schritt-für-Schritt-Tutorial. [Herunterladen den Beispielcode](https://code.msdn.microsoft.com/OWIN-OAuth-20-Authorization-ba2b8783/file/114932/1/AuthorizationServer.zip).
> 
> > [!NOTE]
> > Dieser Umriss sollte nicht vorgesehen, zum Erstellen einer sicheren Produktions-app verwendet werden soll. Dieses Lernprogramm richtet sich nur einen Umriss zum Implementieren von einem OAuth 2.0-Autorisierungsserver mithilfe von OAuth OWIN-Middleware bereitgestellt.
> 
> 
> ## <a name="software-versions"></a>Versionen der Software
> 
> | **In diesem Lernprogramm gezeigt** | **Funktioniert auch mit** |
> | --- | --- |
> | Windows 8.1 | Windows 8, Windows 7 |
> | [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) | [Visual Studio 2013 Express für Desktop](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express). Visual Studio 2012 mit dem aktuellen Update sollte funktionieren, aber im Lernprogramm wurde nicht mit getestet, und einige Menüs und Dialogfelder unterscheiden sich. |
> | .NET 4.5 |  |
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, können Sie sie unter post [Katana-Projekt auf GitHub](https://github.com/aspnet/AspNetKatana/). Fragen und Kommentare in Bezug auf das Lernprogramm selbst finden Sie Abschnitt "Kommentare" am unteren Rand der Seite.


Die [OAuth 2.0-Framework](http://tools.ietf.org/html/rfc6749) ermöglicht eine Drittanbieter-app zum eingeschränkten Zugriff auf einen HTTP-Dienst abrufen. Anstatt Anmeldeinformationen des ressourcenbesitzers, auf eine geschützte Ressource zuzugreifen, erhält der Client ein Zugriffstoken (Dies ist eine Zeichenfolge, die einen bestimmten Bereich, Lebensdauer und anderen Attributen Zugriff angibt). Zugriffstoken werden für Drittanbieter-Clients von einem autorisierungsserver mit Genehmigung des ressourcenbesitzers ausgegeben.

Dieses Lernprogramm umfasst folgende Themen:

- Vorgehensweise: Erstellen von einem autorisierungsserver zur Unterstützung von vier Autorisierung erteilungstyp, und Aktualisieren von Token:
    - Authorization Code grant
    - Implizite Gewährung
    - Das Kennwort des Ressourcenbesitzers Clientanmeldeinformationen
    - Erteilen von Clientanmeldeinformationen
- Erstellt einen Ressourcenserver durch ein Zugriffstoken geschützt ist.
- Erstellen von OAuth 2.0-Clients.

<a id="prerequisites"></a>
## <a name="prerequisites"></a>Erforderliche Komponenten

- [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-editions) oder das kostenlose [Visual Studio Express 2013](https://www.microsoft.com/visualstudio/eng/downloads#d-2013-express), gemäß **Softwareversionen** am oberen Rand der Seite.
- Vertrautheit mit OWIN. Finden Sie unter [Einstieg in das Projekt Katana](https://msdn.microsoft.com/magazine/dn451439.aspx) und [Neuheiten bei OWIN und Katana](index.md).
- Vertrautheit mit [OAuth](http://tools.ietf.org/html/rfc6749) Terminologie, einschließlich [Rollen](http://tools.ietf.org/html/rfc6749#section-1.1), [Protokoll Flow](http://tools.ietf.org/html/rfc6749#section-1.2), und [Autorisierungserteilung](http://tools.ietf.org/html/rfc6749#section-1.3). [OAuth 2.0-Einführung](http://tools.ietf.org/html/rfc6749#section-1) bietet eine gute Einführung.

## <a name="create-an-authorization-server"></a>Einen Autorisierungsserver erstellen

In diesem Lernprogramm werden wir grob, wie Sie skizzieren [OWIN](https://msdn.microsoft.com/magazine/dn451439.aspx) und ASP.NET MVC einen autorisierungsserver erstellen. Wir hoffen, Sie bald einen Download für das vollständige Beispiel bereitstellen, wie in diesem Lernprogramm nicht jeder Schritt enthalten ist. Erstellen Sie zunächst eine leeres Web-app mit dem Namen *AuthorizationServer* und installieren Sie die folgenden Pakete:

- Microsoft.AspNet.Mvc
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth
- Microsoft.Owin.Security.Cookies
- "Microsoft.owin.Security.Google" (oder einer beliebigen anderen sozialen Anmeldung zu verpacken, z. B. "Microsoft.owin.Security.Facebook")

Hinzufügen einer [OWIN Startklasse](owin-startup-class-detection.md) unter dem Stammordner des Projekts mit dem Namen *Start*.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample1.cs?highlight=8)]

Erstellen einer *App\_starten* Ordner. Wählen Sie die *App\_starten* Ordner und verwenden Sie Umschalt + Alt + A, Hinzufügen der heruntergeladene Version der *AuthorizationServer\App\_Start\Startup.Auth.cs* Datei.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample2.cs)]

Der obige Code ermöglicht die Anwendung/externe Anmeldung Cookies und Google-Authentifizierung, die vom autorisierungsserver selbst verwendet werden, um Konten zu verwalten.

Die `UseOAuthAuthorizationServer` Erweiterungsmethode ist beim Einrichten des autorisierungsservers. Die Setupoptionen sind:

- `AuthorizeEndpointPath`: Der Pfad der Anforderung, in denen Clientanwendungen User-Agent umgeleitet wird, um die Benutzer erhalten, stimmen ein Token oder die Code ausgeben. Er muss beginnen mit einem führenden Schrägstrich, z. B. "`/Authorize`".
- `TokenEndpointPath`: Die Anforderung Pfad Clientanwendungen kommunizieren direkt um das Zugriffstoken zu erhalten. Er muss mit einem führenden Schrägstrich, z. B. "/ Token" beginnen. Wenn der Client ausgestellt wird eine [Client\_geheimen](http://tools.ietf.org/html/rfc6749#appendix-A.2), muss Sie an diesen Endpunkt bereitgestellt werden.
- `ApplicationCanDisplayErrors`: Legen Sie auf `true` möchte, dass die Webanwendung auf eine benutzerdefinierte Fehlerseite für den Client-Überprüfungsfehler generieren `/Authorize` Endpunkt. Dies ist nur erforderlich, für Fälle, in dem der Browser wird nicht umgeleitet, an die Clientanwendung, z. B. sichern, wenn die `client_id` oder `redirect_uri` sind falsch. Die `/Authorize` Endpunkt rechnen, finden in der "Oauth. Fehler","Oauth. ErrorDescription"und"Oauth. Der OWIN-Umgebung werden Eigenschaften ErrorUri"hinzugefügt. 

    > [!NOTE]
    > Wenn nicht "true", der autorisierungsserver eine Standardfehlerseite mit den Details der Fehlermeldung zurückgegeben wird.
- `AllowInsecureHttp`: True zu ermöglichen, Autorisierungs- und tokenanforderungen für HTTP-URI-Adressen eingehen und eingehende `redirect_uri` autorisieren Anforderungsparameter HTTP-URI-Adressen haben. 

    > [!WARNING]
    > Sicherheit – ist dies für die Entwicklung nur.
- `Provider`: Das Objekt von der Anwendung zum Verarbeiten von Ereignissen, die von der autorisierungsservermiddleware ausgelöst bereitgestellt. Die Anwendung kann die Schnittstelle vollständig implementieren, oder es möglicherweise erstellen Sie eine Instanz des `OAuthAuthorizationServerProvider` , und weisen Sie Delegaten erforderlich für die OAuth-Arbeitsabläufe, die dieser Server unterstützt.
- `AuthorizationCodeProvider`: Erstellt einen Single-Use-Autorisierungscode an die Clientanwendung zurückgegeben. Sichern Sie die Anwendung für die OAuth-Server **müssen** Geben Sie eine Instanz für `AuthorizationCodeProvider` , in dem das Token erzeugt, indem die `OnCreate/OnCreateAsync` Ereignis als gültig interpretiert wird für nur einen Aufruf von `OnReceive/OnReceiveAsync`.
- `RefreshTokenProvider`: Erstellt ein Aktualisierungstoken, das verwendet werden kann, um ein neues Zugriffstoken bei Bedarf zu erzeugen. Wenn nicht angegeben der autorisierungsserver keine Aktualisierungstoken aus zurückliefert der `/Token` Endpunkt.

## <a name="account-management"></a>Kontenverwaltung

OAuth darauf nicht, wo und wie Sie Ihre Benutzerinformationen für das Konto verwalten. Es wurde [ASP.NET Identity](../../../identity/index.md) die dafür verantwortlich ist. In diesem Lernprogramm wird das Konto Management Code vereinfachen und achten Sie darauf, dass sich der Benutzer anmelden kann mithilfe der OWIN-Cookie-Middleware. Hier ist die vereinfachte Beispielcode für die `AccountController`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample3.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample4.cs?highlight=1)]

`ValidateClientRedirectUri` wird verwendet, um den Client mit der registrierte umleitungs-URL zu überprüfen. `ValidateClientAuthentication` überprüft die grundlegende Schema Header und Formulartext, um die Anmeldeinformationen des Clients abzurufen.

Die Anmeldeseite wird unten gezeigt:

![](owin-oauth-20-authorization-server/_static/image1.png)

Überprüfen Sie der IETF-OAuth-2 [Authorization Code Grant](http://tools.ietf.org/html/rfc6749#section-4.1) jetzt Abschnitt. 

**Anbieter** ist (in der folgenden Tabelle) [OAuthAuthorizationServerOptions](https://msdn.microsoft.com/library/microsoft.owin.security.oauth.oauthauthorizationserveroptions(v=vs.111).aspx). Anbieter, der vom Typ `OAuthAuthorizationServerProvider`, die alle OAuth-Server-Ereignisse enthält. 

| Die einzelnen Schritte im Abschnitt "Authorization Code Grant" | Beispiel zum Herunterladen führt diese Schritte mit: |
| --- | --- |
|  |  |
| (A) der Client initiiert den Fluss durch Umleiten des ressourcenbesitzers Benutzer-Agent an den autorisierungsendpunkt. Der Client enthält die Client-ID, angeforderten Bereich lokalen Zustand und eine umleitungs-URI, an die der autorisierungsserver den Benutzer-Agent senden soll, zurückgesendet, sobald der Zugriff gewährt (oder verweigert). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) der autorisierungsserver authentifiziert den Besitzer der Ressource (über den Benutzer-Agent) und legt fest, ob der Besitzer der Ressource gewährt oder die Anforderung des Clients den Zugriff verweigert. | **&lt;Wenn Benutzer den Zugriff gewährt&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) vorausgesetzt, der Besitzer der Ressource wird Zugriff gewährt, der autorisierungsserver leitet den Benutzer-Agent zurück an den Client mithilfe der umleitungs-URI bereitgestellt früheren (in der Anforderung oder während der Clientregistrierung). ... |  |
|  |  |
| (D) der Client fordert ein Zugriffstoken aus der autorisierungsserver-token-Endpunkt, dazu den Autorisierungscode, der im vorherigen Schritt empfangen. Wenn die Anforderung ausführen, wird der Client mit der autorisierungsserver authentifiziert. Der Client schließt die Umleitung URI verwendet wird, um den Autorisierungscode für die Überprüfung zu erhalten. | Provider.MatchEndpoint Provider.ValidateClientAuthentication AuthorizationCodeProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantAuthorizationCode Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |

Eine beispielimplementierung für `AuthorizationCodeProvider.CreateAsync` und `ReceiveAsync` zum Steuern der Erstellung und Überprüfung der Autorisierungscode werden unten gezeigt.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample5.cs)]

Der obige Code verwendet ein in-Memory-Gleichzeitiges Wörterbuch zum Speichern von Code und Identität Ticket und Wiederherstellen der Identität nach dem Empfang des Codes. In einer realen Anwendung würde es von einem persistenten Datenspeicher ersetzt. Der autorisierungsendpunkt ist für den Besitzer der Ressource gewähren von Zugriff an den Client. In der Regel benötigt eine Benutzeroberfläche, damit der Benutzer auf eine Schaltfläche klicken, und bestätigen die Gewährung. OAuth OWIN-Middleware kann Anwendungscode autorisierungsendpunkt behandelt. In unserem Beispiel-app verwenden wir einen MVC-Controller aufgerufen `OAuthController` verarbeiten. So sieht die beispielimplementierung aus:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample6.cs?highlight=15)]

Die `Authorize` Aktion wird zuerst überprüft, ob der Benutzer mit dem autorisierungsserver angemeldet hat. Wenn dies nicht der Fall, die authentifizierungsmiddleware Herausforderungen den Aufrufer die Authentifizierung mit das Cookie "Application" und zur Anmeldeseite umgeleitet. (Siehe obigen hervorgehobene Code). Wenn Benutzer angemeldet hat, wird die Sicht autorisieren gerendert, wie unten dargestellt:

![](owin-oauth-20-authorization-server/_static/image2.png)

Wenn die **Grant** Schaltfläche aktiviert ist, die `Authorize` Aktion wird ein neues "Träger" Identität und melden Sie sich mit der sie erstellt. Der autorisierungsserver, um ein trägertoken, das Generieren und senden es an den Client mit JSON-Nutzlast löst. 

### <a name="implicit-grant"></a>Implizite Gewährung

Finden Sie in der IETF-OAuth-2 [implizite Gewährung](http://tools.ietf.org/html/rfc6749#section-4.2) jetzt Abschnitt.

 Die [implizite Gewährung](http://tools.ietf.org/html/rfc6749#section-4.2) Fluss in Abbildung 4 dargestellt, ist der Fluss und Zuordnen von denen die OWIN-OAuth Middleware folgt.  

| Die einzelnen Schritte im Abschnitt "implizite Gewährung" | Beispiel zum Herunterladen führt diese Schritte mit: |
| --- | --- |
|  |  |
| (A) der Client initiiert den Fluss durch Umleiten des ressourcenbesitzers Benutzer-Agent an den autorisierungsendpunkt. Der Client enthält die Client-ID, angeforderten Bereich lokalen Zustand und eine umleitungs-URI, an die der autorisierungsserver den Benutzer-Agent senden soll, zurückgesendet, sobald der Zugriff gewährt (oder verweigert). | Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint |
|  |  |
| (B) der autorisierungsserver authentifiziert den Besitzer der Ressource (über den Benutzer-Agent) und legt fest, ob der Besitzer der Ressource gewährt oder die Anforderung des Clients den Zugriff verweigert. | **&lt;Wenn Benutzer den Zugriff gewährt&gt;**  Provider.MatchEndpoint Provider.ValidateClientRedirectUri Provider.ValidateAuthorizeRequest Provider.AuthorizeEndpoint AuthorizationCodeProvider.CreateAsync |
|  |  |
| (C) vorausgesetzt, der Besitzer der Ressource wird Zugriff gewährt, der autorisierungsserver leitet den Benutzer-Agent zurück an den Client mithilfe der umleitungs-URI bereitgestellt früheren (in der Anforderung oder während der Clientregistrierung). ... |  |
|  |  |
| (D) der Client fordert ein Zugriffstoken aus der autorisierungsserver-token-Endpunkt, dazu den Autorisierungscode, der im vorherigen Schritt empfangen. Wenn die Anforderung ausführen, wird der Client mit der autorisierungsserver authentifiziert. Der Client schließt die Umleitung URI verwendet wird, um den Autorisierungscode für die Überprüfung zu erhalten. |  |

Da wir bereits autorisierungsendpunkt implementiert (`OAuthController.Authorize` Aktion) nach Authorization Code GRANT-Anweisung, es impliziten Datenfluss ebenfalls automatisch aktiviert. Hinweis: `Provider.ValidateClientRedirectUri` wird verwendet, um die Client-ID mit der umleitungs-URL, zu überprüfen, bei denen die implizite Grant-Datenfluss schützt vor dem Senden des Zugriffs-Token genutzt, um böswillige Clients ([Man-in-the-Middle-Angriff](https://www.owasp.org/index.php/Man-in-the-middle_attack)).

### <a name="resource-owner-password-credentials-grant"></a>Das Kennwort des Ressourcenbesitzers Clientanmeldeinformationen

Finden Sie in der IETF-OAuth-2 [Kennwort Anmeldeinformationen Ressourcenbesitzererteilung](http://tools.ietf.org/html/rfc6749#section-4.3) jetzt Abschnitt.

 Die [Kennwort Anmeldeinformationen Ressourcenbesitzererteilung](http://tools.ietf.org/html/rfc6749#section-4.3) Fluss in Abbildung 5 dargestellten ist der Fluss und Zuordnen von denen die OWIN-OAuth Middleware folgt.  

| Die einzelnen Schritte im Abschnitt "Kennwort Anmeldeinformationen Ressourcenbesitzererteilung" | Beispiel zum Herunterladen führt diese Schritte mit: |
| --- | --- |
|  |  |
| (A) der Besitzer der Ressource stellt dem Client mit dem Benutzernamen und Kennwort an. |  |
|  |  |
| (B) der Client fordert ein Zugriffstoken aus der autorisierungsserver-token-Endpunkt, indem Sie die Anmeldeinformationen des ressourcenbesitzers erhaltene einschließen. Wenn die Anforderung ausführen, wird der Client mit der autorisierungsserver authentifiziert. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantResourceOwnerCredentials Provider.TokenEndpoint AccessToken Provider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (C) der autorisierungsserver authentifiziert den Client und die Ressource Besitzer Anmeldeinformationen überprüft und wenn gültig ist, gibt ein Zugriffstoken. |  |

Hier wird die beispielimplementierung für `Provider.GrantResourceOwnerCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample7.cs)]

> [!NOTE]
> Der obige Code soll in diesem Abschnitt des Lernprogramms erläutert und sollte nicht verwendet werden, in der sicheren oder Produktion apps. Er überprüft nicht die Besitzer ressourcenanmeldeinformationen. Es wird vorausgesetzt, alle Anmeldeinformationen gültig ist und eine neue Identität für sie erstellt. Die neue Identität wird zum Generieren von Zugriffstoken und Aktualisierungstoken verwendet werden. Ersetzen Sie den Code, Ihren eigenen sicheres Konto Management-Code.


### <a name="client-credentials-grant"></a>Erteilen von Clientanmeldeinformationen

Finden Sie in der IETF-OAuth-2 [Erteilungsfluss](http://tools.ietf.org/html/rfc6749#section-4.4) jetzt Abschnitt.

 Die [Erteilungsfluss](http://tools.ietf.org/html/rfc6749#section-4.4) in Abbildung 6 dargestellten Ablauf ist der Fluss und Zuordnen von denen die OWIN-OAuth Middleware folgt.  

| Die einzelnen Schritte im Abschnitt "Erteilungsfluss" | Beispiel zum Herunterladen führt diese Schritte mit: |
| --- | --- |
|  |  |
| (A) der Client mit der autorisierungsserver authentifiziert und fordert ein Zugriffstoken vom token-Endpunkt. | Provider.MatchEndpoint Provider.ValidateClientAuthentication Provider.ValidateTokenRequest Provider.GrantClientCredentials Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (B) der autorisierungsserver authentifiziert den Client, und wenn gültig ist, gibt ein Zugriffstoken. |  |

Hier wird die beispielimplementierung für `Provider.GrantClientCredentials`:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample8.cs)]

> [!NOTE]
> Der obige Code soll in diesem Abschnitt des Lernprogramms erläutert und sollte nicht verwendet werden, in der sicheren oder Produktion apps. Ersetzen Sie den Code, Ihren eigenen sichere Client Management-Code.


### <a name="refresh-token"></a>Aktualisierungstoken

Finden Sie in der IETF-OAuth-2 [Aktualisierungstoken](http://tools.ietf.org/html/rfc6749#section-1.5) jetzt Abschnitt.

 Die [Aktualisierungstoken](http://tools.ietf.org/html/rfc6749#section-1.5) in Abbildung 2 veranschaulichte Datenfluss finden Sie den Ablauf und Zuordnen von denen die OWIN-OAuth Middleware folgt.  

| Die einzelnen Schritte im Abschnitt "Erteilungsfluss" | Beispiel zum Herunterladen führt diese Schritte mit: |
| --- | --- |
|  |  |
| (G) der Client fordert ein neues Zugriffstoken von Authentifizierung mit dem autorisierungsserver und das Aktualisierungstoken darstellen. Die clientauthentifizierungsanforderungen basieren auf dem Client und auf die Autorisierungsrichtlinien für den Server. | Provider.MatchEndpoint Provider.ValidateClientAuthentication RefreshTokenProvider.ReceiveAsync Provider.ValidateTokenRequest Provider.GrantRefreshToken Provider.TokenEndpoint AccessTokenProvider.CreateAsync RefreshTokenProvider.CreateAsync |
|  |  |
| (H) der autorisierungsserver authentifiziert den Client und das Aktualisierungstoken, das überprüft, und wenn gültig ist, gibt ein neues Zugriffstoken (und optional einen neuen Aktualisierungstoken). |  |

Hier wird die beispielimplementierung für `Provider.GrantRefreshToken`: 

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample9.cs)]

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample10.cs)]

## <a name="create-a-resource-server-which-is-protected-by-access-token"></a>Erstellen Sie einen Ressourcenserver das mit Zugriffstoken geschützt wird

Erstellen Sie ein leeres Web-app-Projekt, und folgende Pakete im Projekt installieren:

- Microsoft.AspNet.WebApi.Owin
- Microsoft.Owin.Host.SystemWeb
- Microsoft.Owin.Security.OAuth

Erstellen Sie eine Startklasse, und Konfigurieren von Authentifizierung und Web-API. Finden Sie unter *AuthorizationServer\ResourceServer\Startup.cs* im Downloadbeispiel.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample11.cs)]

Finden Sie unter *AuthorizationServer\ResourceServer\App\_Start\Startup.Auth.cs* im Downloadbeispiel.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample12.cs)]

Finden Sie unter *AuthorizationServer\ResourceServer\App\_Start\Startup.WebApi.cs* im Downloadbeispiel.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample13.cs)]

- `UseCors` Methode können CORS für alle Domänen.
- `UseOAuthBearerAuthentication` Methode ermöglicht OAuth token trägerauthentifizierungsmiddleware empfangen und trägertoken, das aus dem Autorisierungsheader in der Anforderung überprüft wird.
- `Config.SuppressDefaultHostAuthenticaiton` Unterdrückt die standardmäßige authentifizierten Prinzipal aus der app zu hosten, daher alle Anforderungen anonyme nach dem Aufruf.
- `HostAuthenticationFilter` aktiviert die Authentifizierung nur für den angegebenen Authentifizierungstyp. In diesem Fall ist es Träger-Authentifizierungstyp.

Um die authentifizierte Identität zu veranschaulichen, erstellen wir eine ApiController zum Ausgeben von Ansprüchen des aktuellen Benutzers.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample14.cs)]

Wenn dem autorisierungsserver und der Ressourcenserver nicht auf demselben Computer sind, verwendet die OAuth-Middleware die anderen Computerschlüssel zum Verschlüsseln und Entschlüsseln von Bearer-Zugriffstoken. Damit den gleichen privaten Schlüssel zwischen beide Projekte gemeinsam verwendet werden, fügen dem `machinekey` festlegen, die in beiden *"Web.config"* Dateien.

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample15.xml?highlight=8-10)]

## <a name="create-oauth-20-clients"></a>Erstellen von OAuth 2.0-Clients

 Wir verwenden die [DotNetOpenAuth.OAuth2.Client](http://www.nuget.org/packages/DotNetOpenAuth.OAuth2.Client) NuGet-Paket an den Clientcode zu vereinfachen.

### <a name="authorization-code-grant-client"></a>Authorization Code Grant-Client

 Dieser Client ist eine MVC-Anwendung. Löst eine Authorization Code Grant-Datenfluss, um den Zugriff von Back-End-token zu erhalten. Es wurde eine einzelne Seite, wie unten dargestellt:

![](owin-oauth-20-authorization-server/_static/image3.png)

- Die **autorisieren** Schaltfläche Browser weitergeleitet wird, der autorisierungsserver, um den Besitzer der Ressource gewähren von Zugriff auf diesen Client zu benachrichtigen.
- Die **aktualisieren** Schaltfläche erhalten ein neues Zugriffstoken und Aktualisierungstoken, das die derzeitige Aktualisierung mit token.
- Die **Zugriff geschützte Ressource API** Schaltfläche der Ressourcenserver zum Abrufen von Daten von Ansprüchen des aktuellen Benutzers, und zeigen sie auf der Seite aufgerufen wird.

Hier wird der Beispielcode die `HomeController` des Clients.

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample16.cs)]

`DotNetOpenAuth` erfordert SSL standardmäßig an. Da der Demo HTTP verwendet wird, müssen Sie fügen folgende Einstellung in der Datei "App.config":

[!code-xml[Main](owin-oauth-20-authorization-server/samples/sample17.xml?highlight=4-6)]

> [!WARNING]
> Sicherheit – nie deaktivieren SSL in einer Produktions-app. Ihre Anmeldeinformationen werden jetzt in Klartext über das Netz gesendet wird. Der obige Code ist nur für lokale Beispiel zu debuggen und das Durchsuchen.


### <a name="implicit-grant-client"></a>Implizite Grant-Client

Dieser Client wird JavaScript zu verwenden:

1. Öffnen Sie ein neues Fenster, und leiten Sie an den autorisierungsendpunkt des Autorisierungsservers.
2. Abrufen des Zugriffstokens aus URL-Fragmente, wenn er wieder leitet.

Die folgende Abbildung veranschaulicht diesen Prozess:

![](owin-oauth-20-authorization-server/_static/image4.png)

Der Client sollte haben zwei Seiten: eine für die Startseite und das andere für den Rückruf. Hier ist die JavaScript-Code finden Sie im Beispiel die *Index.cshtml* Datei:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample18.cshtml)]

Hier wird die Verarbeitung von Code im Rückruf *SignIn.cshtml* Datei:

[!code-cshtml[Main](owin-oauth-20-authorization-server/samples/sample19.cshtml)]

> [!NOTE]
> Eine bewährte Methode ist der JavaScript-Code in einer externen Datei verschoben, und nicht mit dem Razor-Markup einbetten. Um dieses Beispiel einfach zu halten, haben sie kombiniert.


### <a name="resource-owner-password-credentials-grant-client"></a>Das Kennwort des Ressourcenbesitzers Anmeldeinformationen Grant-Client

Es verwendet eine Konsolen-app, um diesen Client demo. Hier ist der Code ein:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample20.cs)]

### <a name="client-credentials-grant-client"></a>Clientanmeldeinformationen Grant-Client

Die Kennwort-Anmeldeinformationen-Ressourcenbesitzererteilung, hier ähnelt Konsole app-Code:

[!code-csharp[Main](owin-oauth-20-authorization-server/samples/sample21.cs)]
