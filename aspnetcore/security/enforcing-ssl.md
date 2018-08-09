---
title: Erzwingen von HTTPS in ASP.NET Core
author: rick-anderson
description: Zeigt die Vorgehensweise zum Erzwingen einer HTTPS/TLS, in einer ASP.NET Core-Web-app.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 3bea8661e17fec5128e822d98741d1f8ed7434e5
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655497"
---
# <a name="enforce-https-in-aspnet-core"></a>Erzwingen von HTTPS in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieses Dokument zeigt, wie Sie:

* HTTPS für alle Anforderungen erforderlich.
* Alle HTTP-Anforderungen auf HTTPS umleiten.

> [!WARNING]
> Führen Sie **nicht** verwenden [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) für Web-APIs, die vertraulichen Informationen zu erhalten. `RequireHttpsAttribute` leitet Browsers per HTTP-Statuscode von HTTP an HTTPS weiter. API-Clients verstehen diese Codes möglicherweise nicht, oder Sie führen keine Weiterleitung von HTTP an HTTPS durch. Dies kann dazu führen, dass solche Clients Daten unverschlüsselt mittels HTTP versenden. Web-APIs sollten daher entweder:
>
> * nicht auf HTTP lauschen oder
> * die Verbindung mit dem Statuscode 400 („Ungültige Anforderung“) schließen und die Anforderung nicht verarbeiten.

<a name="require"></a>
## <a name="require-https"></a>Anforderung von HTTPS

::: moniker range=">= aspnetcore-2.1"

Es wird empfohlen, alle ASP.NET Core-Web-apps rufen die HTTPS-Umleitung-Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) auf alle HTTP-Anfragen an HTTPS umzuleiten.

Der folgende code ruft `UseHttpsRedirection` in die `Startup` Klasse:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Der oben markierte Code:

* Verwendet die standardmäßige [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`). Produktions-apps sollten Aufrufen [UseHsts](#hsts).
* Verwendet die standardmäßige [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).

Der folgende code ruft [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) middlewareoptionen konfigurieren:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Der oben markierte Code:

* Legt [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) zu `Status307TemporaryRedirect`, dies ist der Standardwert. Produktions-apps sollten Aufrufen [UseHsts](#hsts).
* Legt den HTTPS-Port in 5001 fest. Der Standardwert ist 443.

Die folgenden Mechanismen legen Sie den Port automatisch:

* Die Middleware kann ermitteln, die Ports über [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) wenn Folgendes zutrifft:
  - Kestrel oder HTTP.sys direkt mit HTTPS-Endpunkte verwendet wird (gilt auch für die app mit Visual Studio Code-Debugger ausgeführt wird).
  - Nur **eine HTTPS-Port** wird von der app verwendet.
* Visual Studio wird verwendet:
  - IIS Express ist HTTPS-tauglich.
  - *"launchsettings.JSON"* legt die `sslPort` für IIS Express.

> [!NOTE]
> Wenn eine app ausgeführt wird, hinter einem Reverseproxy (z. B. IIS, IIS Express) `IServerAddressesFeature` ist nicht verfügbar. Der Port muss manuell konfiguriert werden. Wenn der Port nicht festgelegt ist, werden nicht die Anforderungen umgeleitet.

Der Port kann konfiguriert werden, durch Festlegen der [Https_port Webhost-Konfigurationseinstellung](xref:fundamentals/host/web-host#https-port):

**Schlüssel**: https_port **Typ**: *string*
**Standard**: Es ist kein Standardwert festgelegt.
**Legen Sie mithilfe von**: `UseSetting` 
 **Umgebungsvariable**: `<PREFIX_>HTTPS_PORT` (das Präfix ist `ASPNETCORE_` bei Verwendung der Web-Host.)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> Der Port kann indirekt konfiguriert werden, durch Festlegen der URL durch die `ASPNETCORE_URLS` -Umgebungsvariablen angegeben. Die Umgebungsvariable konfiguriert den Server, und klicken Sie dann die Middleware indirekt über den HTTPS-Port ermittelt `IServerAddressesFeature`.

Wenn kein Port festgelegt ist:

* Anforderungen werden nicht umgeleitet.
* Die Middleware protokolliert eine Warnung.

> [!NOTE]
> Eine Alternative zur Verwendung von HTTPS-Umleitung-Middleware (`UseHttpsRedirection`) ist die Verwendung von URL-Umschreibenden Middleware (`AddRedirectToHttps`). `AddRedirectToHttps` können den Statuscode und den Port auch festlegen, wenn die Umleitung ausgeführt wird. Weitere Informationen finden Sie unter [URL-Umschreibenden Middleware](xref:fundamentals/url-rewriting).
>
> Ohne die Notwendigkeit von zusätzlichen umleitungs-Regeln an HTTPS umleiten möchten, empfehlen wir Ihnen unter Verwendung von HTTPS-Umleitung-Middleware (`UseHttpsRedirection`) in diesem Thema beschrieben.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

Die [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) wird verwendet, um die HTTPS erforderlich ist. `[RequireHttpsAttribute]` ergänzen können, Controllern oder Methoden oder global angewendet werden können. Um das Attribut global anzuwenden, fügen Sie den folgenden Code zur `ConfigureServices` in `Startup`:

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Der oben markierte Code ist erforderlich, alle Anforderungen verwenden `HTTPS`daher HTTP-Anforderungen werden ignoriert. Der folgende hervorgehobene Code leitet alle HTTP-Anfragen an HTTPS um:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Weitere Informationen finden Sie unter [URL-Umschreibenden Middleware](xref:fundamentals/url-rewriting). Die Middleware kann außerdem die app, die den Statuscode oder den Statuscode sowie den Port festgelegt, wenn die Umleitung ausgeführt wird.

Das globale Erzwingen der Verwendung von HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode. Dieser Ansatz gilt im Vergleich zur Anwendung des `[RequireHttps]` -Attributs auf alle Controller und Razor Pages als sicherer. denn Sie können nicht gewährleisten, dass das `[RequireHttps]` -Attribut angewendet wird, wenn neue Controller oder Razor Pages hinzugefügt werden.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>HTTP Strict Transport Security-Protokoll (HSTS)

Pro [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) ist eine optionale sicherheitserweiterung, die von einer Web-app durch die Verwendung eines Antwortheaders angegeben wird. Wenn ein Browser, der mit Unterstützung von HSTS diesen Header empfängt:

* Der Browser speichert die Konfiguration für die Domäne, die verhindert, dass eine Kommunikation über HTTP senden. Der Browser wird die gesamte Kommunikation über HTTPS erzwungen. 
* Der Browser wird verhindert, dass der Benutzer mithilfe von Zertifikaten für nicht vertrauenswürdig oder ungültig. Der Browser deaktiviert eingabeaufforderungen, die einen solches Zertifikat vorübergehend vertrauen Benutzer zu ermöglichen.

ASP.NET Core 2.1 oder höher implementiert HSTS mit der `UseHsts` -Erweiterungsmethode. Der folgende code ruft `UseHsts` bei der app nicht im [Entwicklungsmodus](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` ist nicht in der Entwicklung empfohlen, da der Header des HSTS hohem Maße zwischenspeicherbar ist von Browsern. In der Standardeinstellung `UseHsts` schließt die lokalen Loopback-Adresse.

Für produktionsumgebungen HTTPS zum ersten Mal implementieren wird den Anfangswert von HSTS auf einen kleinen Wert fest. Legen Sie den Wert von Stunden keine mehr als ein Tag für den Fall, dass Sie die HTTP-HTTPS-Infrastruktur wiederherstellen müssen. Nachdem Sie sich die Nachhaltigkeit der HTTPS-Konfiguration sicher sind, erhöhen Sie den HSTS-Max-Age-Wert; häufig verwendeter Wert ist ein Jahr. 

Der folgende Code

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Legt den preload Parameter des Strict-Transport-Security-Headers. Preload ist nicht Teil der [RFC HSTS Spezifikation](https://tools.ietf.org/html/rfc6797), aber vom Webbrowser zum Vorabladen von HSTS Websites Neuinstallation unterstützt wird. Weitere Informationen finden Sie unter [https://hstspreload.org/](https://hstspreload.org/).
* Ermöglicht [IncludeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), denen Unterdomänen der Host die HSTS-Richtlinie gilt. 
* Explizit festlegt den Max-Age-Parameter des Strict-Transport-Security-Headers auf 60 Tage. Wenn nicht festgelegt, der Standardwert ist 30 Tage. Finden Sie unter den [Max-Age-Direktive](https://tools.ietf.org/html/rfc6797#section-6.1.1) für Weitere Informationen.
* Fügt `example.com` zur Liste der Hosts zu schließen.

`UseHsts` Schließt die folgenden Loopback-Hosts an:

* `localhost` : Die IPv4-Loopback-Adresse.
* `127.0.0.1` : Die IPv4-Loopback-Adresse.
* `[::1]` : Die IPv6-Loopback-Adresse.

Im vorherige Beispiel veranschaulicht das weitere Hosts hinzufügen möchten.
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Deaktivieren von HTTPS bei projekterstellung

Aktivieren Sie die ASP.NET Core 2.1 oder höher Webanwendungsvorlagen (von Visual Studio oder der Dotnet-Befehlszeile) [HTTPS-Umleitung](#require) und [HSTS](#hsts). Für Bereitstellungen, die keine HTTPS erforderlich ist, Sie können HTTPS deaktivieren. Z. B. einige Back-End-Dienste, in denen HTTPS extern am Netzwerkrand behandelt wird über HTTPS an jedem Knoten, ist nicht erforderlich.

Zum Deaktivieren von HTTPS:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Deaktivieren Sie die **für HTTPS konfigurieren** Kontrollkästchen.

![Entitätsdiagramm](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli) 

Verwenden Sie die `--no-https`-Option. Beispiel:

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a>Zum Einrichten von einem entwicklerzertifikat für Docker

Finden Sie unter [GitHub-Problem](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
