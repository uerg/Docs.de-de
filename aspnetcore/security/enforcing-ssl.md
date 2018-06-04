---
title: Erzwingen von HTTPS in ASP.NET Core
author: rick-anderson
description: Veranschaulicht, wie HTTPS/TLS in einer ASP.NET Core erfordern Web-app.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 24ab83192ded381b46fab337a986f51fb22b2227
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729497"
---
# <a name="enforce-https-in-aspnet-core"></a>Erzwingen von HTTPS in ASP.NET Core

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieses Dokument zeigt, wie Sie:

* HTTPS für alle Anforderungen erforderlich.
* Alle HTTP-Anforderungen auf HTTPS umleiten.

> [!WARNING]
> Führen Sie **nicht** verwenden [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) auf Web-APIs, die vertraulichen Informationen zu erhalten. `RequireHttpsAttribute` leitet Browsers per HTTP-Statuscode von HTTP an HTTPS weiter. API-Clients verstehen diese Codes möglicherweise nicht, oder Sie führen keine Weiterleitung von HTTP an HTTPS durch. Dies kann dazu führen, dass solche Clients Daten unverschlüsselt mittels HTTP versenden. Web-APIs sollten daher entweder:
>
> * nicht auf HTTP lauschen oder
> * die Verbindung mit dem Statuscode 400 („Ungültige Anforderung“) schließen und die Anforderung nicht verarbeiten.

<a name="require"></a>
## <a name="require-https"></a>HTTPS erforderlich

::: moniker range=">= aspnetcore-2.1"

Es wird empfohlen, alle Web-apps mit ASP.NET Core aufrufen HTTPS-Umleitung Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) alle HTTP-Anforderungen an HTTPS umgeleitet.

Der folgende code ruft `UseHttpsRedirection` in die `Startup` Klasse:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

Der folgende code ruft [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) middlewareoptionen konfigurieren:

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

Die vorangehenden hervorgehobenen Code hinzu:

* Legt [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode).
* Legt den HTTPS-Port auf 5001 fest.

Die folgenden Mechanismen legen Sie den Port automatisch:

* Die Middleware erkennen, die Ports über [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) wenn Folgendes zutrifft:
  - Kestrel oder HTTP.sys direkt mit HTTPS-Endpunkte verwendet wird (gilt auch zum Ausführen der app mit Visual Studio Code-Debugger).
  - Nur **eine HTTPS-Port** wird von der app verwendet.
* Visual Studio wird verwendet:
  - IIS Express umfasst HTTPS aktiviert.
  - *launchSettings.json* legt die `sslPort` für IIS Express.

> [!NOTE]
> Beim Ausführen einer app hinter einem reverse-Proxy (z. B. IIS, IIS Express) `IServerAddressesFeature` ist nicht verfügbar. Der Port muss manuell konfiguriert werden. Wenn der Port nicht festgelegt ist, werden nicht Anforderungen umgeleitet.

Der Port kann konfiguriert werden, durch Festlegen der:

* Die Umgebungsvariable `ASPNETCORE_HTTPS_PORT`
* `http_port` Host-Konfigurationsschlüssel (z. B. über *hostsettings.json* oder ein Befehlszeilenargument).
* [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport). Finden Sie in vorhergehenden Beispiel, das zeigt, wie für den Port 5001 festgelegt.

> [!NOTE]
> Der Port kann indirekt konfiguriert werden, durch Festlegen der URL mit der `ASPNETCORE_URLS` -Umgebungsvariablen angegeben. Die Umgebungsvariable konfiguriert den Server, und dann die Middleware indirekt über den HTTPS-Port ermittelt `IServerAddressesFeature`.

Wenn kein Port festgelegt ist:

* Anforderungen werden nicht umgeleitet.
* Die Middleware protokolliert eine Warnung.

::: moniker-end

::: moniker range="< aspnetcore-2.1"

Die [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) wird verwendet, um HTTPS erforderlich ist. `[RequireHttpsAttribute]` ergänzen können, Controllern oder Methoden oder global angewendet werden können. Um das Attribut global anzuwenden, fügen Sie den folgenden Code zum `ConfigureServices` in `Startup`:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

Die vorherige hervorgehobene Code erfordert, verwenden alle Anforderungen `HTTPS`daher HTTP-Anforderungen werden ignoriert. Die folgende hervorgehobene Code leitet alle HTTP-Anforderungen an HTTPS:

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

Weitere Informationen finden Sie unter [URL umschreiben Middleware](xref:fundamentals/url-rewriting).

Das globale Erzwingen der Verwendung von HTTPS (`options.Filters.Add(new RequireHttpsAttribute());`) ist eine bewährte Sicherheitsmethode. Dieser Ansatz gilt im Vergleich zur Anwendung des `[RequireHttps]` -Attributs auf alle Controller und Razor Pages als sicherer. denn Sie können nicht gewährleisten, dass das `[RequireHttps]` -Attribut angewendet wird, wenn neue Controller oder Razor Pages hinzugefügt werden.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a>HTTP-strikte Sicherheit Transportprotokoll (HSTS)

Pro [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Sicherheit (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) ist eine opt-in-sicherheitserweiterung, die von einer Webanwendung durch die Verwendung eines speziellen Antwortheaders angegeben wird. Erstellt, sobald ein unterstützter Browser dieser Header empfangen wird dieses Browsers zu verhindern, dass die Kommunikation über HTTP gesendet werden, die der angegebenen Domäne und wird stattdessen die gesamte Kommunikation über HTTPS gesendet. Es wird verhindert, dass HTTPS auf über aufforderungen auf den Browsern.

ASP.NET Core 2.1 oder höher implementiert HSTS mit der `UseHsts` -Erweiterungsmethode. Der folgende code ruft `UseHsts` Wenn die app nicht im [-Entwicklungsmodus](xref:fundamentals/environments):

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

`UseHsts` wird nicht empfohlen, bei der Entwicklung, da der Header HSTS hoch von Browsern werden kann. Standardmäßig schließt UseHsts aus die lokalen Loopbackadresse.

Der folgende Code

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* Legt den Teiler Parameter des Strict-Transport-Security-Headers. Preload ist nicht Teil der [Spezifikation RFC HSTS](https://tools.ietf.org/html/rfc6797), aber von Webbrowsern zum Vorabladen HSTS Websites Neuinstallation unterstützt wird. Weitere Informationen finden Sie unter [https://hstspreload.org/](https://hstspreload.org/).
* Ermöglicht [IncludeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), denen Unterdomänen Host die HSTS-Richtlinie gilt. 
* Legt explizit den Max-Age-Parameter des Strict-Transport-Security-Headers, der auf 60 Tage. Wenn dies nicht festgelegt, der Standardwert ist 30 Tage. Finden Sie unter der [Max-Age-Direktive](https://tools.ietf.org/html/rfc6797#section-6.1.1) für Weitere Informationen.
* Fügt `example.com` zur Liste der Hosts ausschließen.

`UseHsts` Schließt die folgenden Loopback-Hosts an:

* `localhost` : Die IPv4-Loopback-Adresse.
* `127.0.0.1` : Die IPv4-Loopback-Adresse.
* `[::1]` : Die IPv6-Loopback-Adresse.

Das vorhergehende Beispiel zeigt, wie Sie weitere Hosts hinzufügen.
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a>Opt-Out-of HTTPS bei projekterstellung

Aktivieren Sie die ASP.NET Core 2.1 oder höher Anwendung Webvorlagen (in Visual Studio oder der Befehlszeile Dotnet) [HTTPS-Umleitung](#require) und [HSTS](#hsts). Für Bereitstellungen, die nicht HTTPS erforderlich ist, Sie können von HTTPS teilnehmen. Z. B. einige Back-End-Dienste, in denen HTTPS extern am Netzwerkrand behandelt wird über HTTPS an jedem Knoten, ist nicht erforderlich.

Um von HTTPS teilnehmen:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio) 

Deaktivieren Sie die **für HTTPS konfigurieren** Kontrollkästchen.

![Entitätsdiagramm](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[.NET Core-CLI](#tab/netcore-cli) 

Verwenden Sie die `--no-https`-Option. Beispiel:

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a>So richten Sie ein entwicklerzertifikat für Docker

Finden Sie unter [das GitHub-Problem](https://github.com/aspnet/Docs/issues/6199).

::: moniker-end
