---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Aktivieren von ursprungsübergreifenden Anforderungen in ASP.NET-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Zeigt, wie zur Unterstützung von Cross-Origin Resource Sharing (CORS) in ASP.NET Web-API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 603eb55f8fa0b629d0287b66086b9495ef55faf7
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381088"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a>Aktivieren von ursprungsübergreifenden Anforderungen in ASP.NET-Web-API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Browsersicherheit verhindert, dass eine Webseite AJAX-Anforderungen in eine andere Domäne. Diese Einschränkung wird aufgerufen, die *Richtlinie desselben Ursprungs*, und verhindert, dass eine schädliche Website sensible Daten von einer anderen Website liest. Jedoch sollten Sie manchmal auf andere Standorte Ihrer Web-API aufrufen können.
> 
> [Cross-Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) ist ein W3C-Standard, der einem Server zu lockern die Richtlinie des gleichen Ursprungs ermöglicht. Mit CORS kann ein Server explizit einige ursprungsübergreifende Anforderungen zulassen und andere ablehnen. CORS ist sicherer und flexibler als frühere Techniken wie z. B. [JSONP](http://en.wikipedia.org/wiki/JSONP). In diesem Tutorial veranschaulicht das Aktivieren von CORS in Ihrer Web-API-Anwendung.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - [Visual Studio 2013 Update 2](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web-API 2.2


<a id="intro"></a>
## <a name="introduction"></a>Einführung

In diesem Tutorial wird veranschaulicht, dass CORS-Unterstützung in ASP.NET Web-API. Wir beginnen, erstellen Sie zwei ASP.NET-Projekte – einen namens "WebService", hostet einen Web-API-Controller, und die anderen namens "" Webclient "" der Webdienst aufruft. Da die beiden Anwendungen auf verschiedenen Domänen gehostet werden, ist eine AJAX-Anforderung von "Webclient" auf den Webdienst eine cors-Anforderung.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>Was ist "Denselben Ursprung"?

Zwei URLs haben denselben Ursprung ggf. identische Schemas, Hosts und -Ports. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Diese zwei URLs haben denselben Ursprung an:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Diese URLs haben verschiedene Ursprünge als die vorherige zwei:

- `http://example.net` -Andere Domäne
- `http://example.com:9000/foo.html` -Portnummer
- `https://example.com/foo.html` -Andere Partitionsschema
- `http://www.example.com/foo.html` -Andere Unterdomäne

> [!NOTE]
> Internet Explorer wird den Port nicht berücksichtigt, für den Vergleich Ursprünge.


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a>Erstellen Sie das WebService-Projekt

> [!NOTE]
> In diesem Abschnitt wird davon ausgegangen, dass Sie bereits wissen, wie Web-API-Projekte erstellen. Falls nicht, siehe [erste Schritte mit ASP.NET Web-API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).


Starten Sie Visual Studio, und erstellen Sie ein neues **ASP.NET-Webanwendung** Projekt. Wählen Sie die **leere** Projektvorlage. Unter "Hinzufügen von Ordnern und kernreferenzen für" Wählen Sie die **Web-API-** Kontrollkästchen. Wählen Sie optional die Option "Host in der Cloud" zum Bereitstellen der app in Microsoft Azure. Microsoft bietet kostenloses Webhosting für bis zu 10 Websites in einem [kostenloses Azure-Testkonto](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

Fügen Sie einen Web-API-Controller mit dem Namen `TestController` durch den folgenden Code:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

Sie können die Anwendung lokal ausführen oder in Azure bereitstellen. (Die Screenshots in diesem Tutorial bereitgestellt ich zu Azure App Service-Web-Apps.) Navigieren Sie zu, um sicherzustellen, dass die Web-API arbeitet, `http://hostname/api/test/`, wobei *Hostname* ist die Domäne, in dem Sie die Anwendung bereitgestellt. Daraufhin sollte der Antworttext, &quot;abrufen: Testnachricht&quot;.

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a>Erstellen Sie das Projekt "Webclient"

Erstellen Sie ein ASP.NET Web-Anwendung, und wählen Sie die **MVC** Projektvorlage. Wählen Sie optional **Authentifizierung ändern** > **keine Authentifizierung**. Für dieses Tutorial benötigen Sie keine Authentifizierung.

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

Öffnen Sie im Projektmappen-Explorer die Datei Views/Home/Index.cshtml aus. Ersetzen Sie den Code in dieser Datei durch Folgendes ein:

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

Für die *ServiceUrl* Variable, verwenden Sie den URI der WebService-app. Nun führen Sie den WebClient-app lokal oder in einer anderen Website veröffentlichen.

Klicken auf die Schaltfläche "Ausprobieren" sendet eine AJAX-Anforderung an die WebService-app, die mit der HTTP-Methode aufgeführt, die der Dropdownliste (GET, POST oder PUT). Auf diese Weise unterschiedliche Cross-Origin-Anforderungen zu überprüfen. Moment, die Webdienst-app unterstützt keine CORS, sodass, wenn Sie die Schaltfläche klicken, Sie einen Fehler erhalten.

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Wenn Sie sehen Sie sich den HTTP-Datenverkehr in einem Tool wie [Fiddler](http://www.telerik.com/fiddler), sehen Sie, dass der Browser die GET-Anforderung sendet, und die Anforderung erfolgreich ist, aber der AJAX-Aufruf gibt einen Fehler zurück. Es ist wichtig zu verstehen, dass die Richtlinie desselben Ursprungs nicht durch den Browser verhindert *senden* der Anforderung. Stattdessen es verhindert, dass die Anwendung sehen die *Antwort*.


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a>Aktivieren von CORS

Jetzt aktivieren wir CORS in der WebService-app. Fügen Sie zunächst das CORS-NuGet-Paket. In Visual Studio aus der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **-Paket-Manager-Konsole**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Dieser Befehl installiert das neueste Paket und alle Abhängigkeiten, einschließlich der Core-Web-API-Bibliotheken aktualisiert. Benutzer-versionsflag, das eine bestimmte Version abzielen. Das CORS-Paket ist die Web-API 2.0 oder höher erforderlich.

Öffnen Sie die Datei App\_Start/WebApiConfig.cs. Fügen Sie den folgenden Code der **WebApiConfig.Register** Methode.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Fügen Sie als Nächstes die **[EnableCors]** -Attribut auf die `TestController` Klasse:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Für die *Ursprünge* -Parameter verwenden Sie den URI, wo Sie die Anwendung "Webclient" bereitgestellt. Dies erlaubt Cross-Origin-Anfragen von "Webclient" untersagen weiterhin auf alle anderen domänenübergreifende Anforderungen. Ich beschreibe später die Parameter für **[EnableCors]** im Detail.

Schließen Sie keinen Schrägstrich am Ende der *Ursprünge* URL.

Bereitstellen Sie die aktualisierte WebService-Anwendung erneut. Sie müssen nicht "Webclient" zu aktualisieren. Die AJAX-Anforderung von "Webclient" sollte jetzt erfolgreich sein. Die GET, PUT und POST-Methoden sind alle zulässig.

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a>Funktionsweise von CORS

In diesem Abschnitt wird beschrieben, was geschieht, in einer CORS-Anforderung auf der Ebene der HTTP-Nachrichten. Es ist wichtig zum Verständnis der Funktionsweise von CORS, sodass Sie konfigurieren können die **[EnableCors]** ordnungsgemäß Attribut aus, und beheben, wenn etwas nicht wie erwartet funktioniert.

CORS-Spezifikation führt mehrere neue HTTP-Header, die Cross-Origin-Anforderungen zu ermöglichen. Wenn ein Browser CORS unterstützt, wird diese Header automatisch für Cross-Origin-Anforderungen; Sie müssen gar nichts Besonderes im JavaScript-Code.

Hier ist ein Beispiel für eine cors-Anforderung. Der "Origin"-Header gibt die Domäne der Website, die die Anforderung stammt.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Wenn der Server die Anforderung zulässt, wird der Access-Control-Allow-Origin-Header. Der Wert dieses Headers entspricht den Origin-Header oder ist Sie den Platzhalterwert "\*", Bedeutung, die einen beliebigen Ursprung zulässig ist.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Wenn es sich bei den Access-Control-Allow-Origin-Header in die Antwort nicht enthalten ist, schlägt die AJAX-Anforderung. Der Browser lässt, die die Anforderung. Selbst wenn der Server eine erfolgreiche Antwort zurückgibt, ist der Browser nicht die Antwort an die Clientanwendung zur Verfügung.

**Preflight-Anforderungen**

Für einige CORS-Anforderungen sendet der Browser eine zusätzliche Anforderung, die eine "preflight-Anforderung", aufgerufen, bevor die tatsächliche Anforderung für die Ressource gesendet.

Der Browser kann die preflight-Anforderung überspringen, wenn die folgenden Bedingungen erfüllt sind:

- Die Anforderungsmethode ist GET, HEAD oder POST, *und*
- Die Anwendung ist nicht festgelegt Anforderungsheader als Accept, Accept-Language, Inhaltssprache, Content-Type oder letzten-Ereignis-ID, *und*
- Der Content-Type-Header (falls festgelegt) ist eine der folgenden: 

    - application/x-www-form-urlencoded
    - Multipart/Form-data
    - Text/plain

Die Regel zu Anforderungsheader gilt für Header, die die Anwendung durch Aufrufen von festlegt **SetRequestHeader** auf die **XMLHttpRequest** Objekt. (Die CORS-Spezifikation als diese "Author-Anforderungsheader" bezeichnet). Die Regel gilt nicht für Header der *Browser* festlegen können, z. B. Benutzer-Agent, Host und Content-Length.

Hier ist ein Beispiel für eine preflight-Anforderung:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

Die Pre-Flight-Anforderung verwendet die HTTP OPTIONS-Methode. Es enthält zwei spezielle Header:

- Access-Control-Request-Method: Die HTTP-Methode, die für die tatsächliche Anforderung verwendet werden.
- Access-Control-Request-Headers: Eine Liste der Anforderungsheader, die die *Anwendung* legen Sie für die tatsächliche Anforderung. (In diesem Fall ist dies nicht-Header, die der Browser legt diese fest enthalten.)

Hier ist eine Beispielantwort, vorausgesetzt, dass der Server die Anforderung zulässt:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

Die Antwort enthält eine Access-Control-Allow-Methods-Header, der die zulässigen Methoden aufgeführt, und optional eine Access-Control-Allow-Headers-Header, der die erlaubten Header aufgeführt sind. Wenn die preflight-Anforderung erfolgreich ist, sendet der Browser die tatsächliche Anforderung an, wie oben beschrieben.

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a>Bereichsregeln für [EnableCors]

Sie können CORS pro Aktion, pro Controller oder global für alle Web-API-Controller in Ihrer Anwendung ermöglichen.

**Pro Aktion**

Legen Sie zum Aktivieren von CORS für eine einzelne Aktion die **[EnableCors]** Attribut für die Aktionsmethode. Im folgenden Beispiel wird CORS für den `GetItem` nur Methode.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Pro Controller**

Setzen Sie **[EnableCors]** auf die Controllerklasse, gilt für alle Aktionen auf dem Controller. Wenn Sie CORS für eine Aktion deaktivieren möchten, fügen die **[DisableCors]** -Attribut auf die Aktion. Im folgenden Beispiel wird CORS für jede Methode, mit Ausnahme von `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Global**

Übergeben Sie zum Aktivieren von CORS für alle Web-API-Controller in Ihrer Anwendung eine **EnableCorsAttribute** -Instanz, auf die **EnableCors** Methode:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Wenn Sie das Attribut auf mehr als einen Bereich festlegen, ist die Reihenfolge auf:

1. Aktion
2. Controller
3. Global

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a>Legen Sie die zulässigen Ursprünge

Die *Ursprünge* Parameter, der die **[EnableCors]** Attribut gibt an, welche Ursprünge zulässig sind, auf die Ressource zuzugreifen. Der Wert ist eine durch Trennzeichen getrennte Liste der zulässigen Ursprünge.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

Sie können auch den Platzhalterwert "\*" um Anforderungen von der alle Ursprünge zuzulassen.

Wägen Sie sorgfältig, bevor Sie die Anforderungen von einem beliebigen Ursprung zulassen. Es bedeutet, dass praktisch jeder Website AJAX-Aufrufe Ihrer Web-API durchführen kann.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a>Legen Sie die zulässigen HTTP-Methoden

Die *Methoden* Parameter, der die **[EnableCors]** Attribut gibt an, welche HTTP-Methoden zulässig sind, auf die Ressource zuzugreifen. Um alle Methoden zu ermöglichen, verwenden Sie den Platzhalterwert "\*". Im folgende Beispiel kann nur Get- und POST-Anforderungen.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a>Legt die zulässigen Anforderungsheader fest.

Weiter oben beschriebenen wie eine preflight-Anforderung einen Header Access-Control-Request-Headers, enthalten möglicherweise die HTTP-Header, die von der Anwendung festgelegte auflisten (den so genannten "author-Anforderungsheader"). Die *Header* Parameter, der die **[EnableCors]** Attribut gibt an, welche Autor Anforderungsheader zulässig sind. Um alle Header zuzulassen, legen *Header* auf "\*". Legen Sie auf eine Whitelist bestimmte Header, *Header* auf eine durch Trennzeichen getrennte Liste der zulässigen Header:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Browser sind jedoch nicht vollständig konsistent wie sie Access-Control-Request-Headers festgelegt. Derzeit enthält "Origin"; z. B. Chrome während Sie FireFox nicht beinhaltet Standardheader wie z. B. "Accept", selbst wenn die Anwendung im Skript festlegt.

Setzen Sie *Header* auf irgendetwas außer "\*", aufzunehmen mindestens "accept", "Content-Type" und "Origin" sowie alle benutzerdefinierten Header, die Sie unterstützen möchten.

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a>Legen Sie die zulässigen Antwortheader

Standardmäßig ist der Browser nicht alle Antwortheader für die Anwendung verfügbar machen. Die Antwortheader, die standardmäßig verfügbar sind:

- Cache-Control
- Content-Language
- Inhaltstyp
- Läuft ab
- Zuletzt geändert
- Pragma

Die CORS-Spezifikation ruft diese [Header für die einfache Antwort](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Um weitere Header für die Anwendung verfügbar machen, legen die *ExposedHeaders* Parameter **[EnableCors]**.

Im folgenden Beispiel ist der Controller die `Get` Methode legt einen benutzerdefinierten Header mit dem Namen "X-Custom-Header" fest. Standardmäßig wird der Browser diesen Header in einer cors-Anforderung nicht verfügbar gemacht. Um den Header verfügbar zu machen, schließen Sie "X-Custom-Header" in *ExposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a>Übergeben von Anmeldeinformationen in Cross-Origin-Anforderungen

Anmeldeinformationen erfordern besondere Behandlung in eine CORS-Anforderung. Standardmäßig sendet der Browser keine Anmeldeinformationen mit einer cors-Anforderung. Anmeldeinformationen enthalten, Cookies sowie HTTP-Authentifizierungsschemas. Um die Anmeldeinformationen mit einer cors-Anforderung zu senden, muss der Client festgelegt **XMLHttpRequest.withCredentials** auf "true".

Mithilfe von **XMLHttpRequest** direkt:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

In jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Darüber hinaus muss der Server die Anmeldeinformationen zulassen. Um ursprungsübergreifende Anmeldeinformationen in Web-API zu ermöglichen, legen die **"supportscredentials"** Eigenschaft auf "true", auf die **[EnableCors]** Attribut:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Wenn diese Eigenschaft auf "true" festgelegt ist, wird die HTTP-Antwort einen Access-Control-Allow-Credentials-Header einfügen. Dieser Header informiert den Browser, dass der Server die Anmeldeinformationen für eine Anforderung zwischen verschiedenen Ursprüngen zulässt.

Wenn der Browser sendet die Anmeldeinformationen, aber die Antwort enthält keines gültigen Access-Control-Allow-Credentials-Headers, der Browser wird die Antwort an die Anwendung nicht verfügbar, und die AJAX-Anforderung ein Fehler auftritt.

Zu den Einstellungen sehr vorsichtig sein **"supportscredentials"** auf "true", da es bedeutet, dass eine Website in einer anderen Domäne kann Ihrer Web-API im Auftrag des Benutzers, eines angemeldeten Benutzers Anmeldeinformationen senden, ohne dass der Benutzer an, dass Sie sich,. Die CORS-Spezifikation gibt auch an dieser Einstellung *Ursprünge* zu &quot; \* &quot; ist ungültig Wenn **"supportscredentials"** ist "true".

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a>Benutzerdefiniertes CORS-Richtlinie-Anbieter

Die **[EnableCors]** Attribut implementiert die **ICorsPolicyProvider** Schnittstelle. Sie können Ihre eigene Implementierung bereitstellen, indem Sie eine abgeleitete Klasse erstellen **Attribut** und implementiert **ICorsProlicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Nachdem Sie das Attribut anwenden können, beliebiger Stelle, platzieren Sie würde **[EnableCors]**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Beispielsweise kann ein benutzerdefiniertes CORS-Richtlinienanbieter die Einstellungen aus einer Konfigurationsdatei lesen.

Als Alternative zur Verwendung von Attributen, können Sie registrieren einen **ICorsPolicyProviderFactory** Objekt, das erstellt **ICorsPolicyProvider** Objekte.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Festlegen der **ICorsPolicyProviderFactory**, rufen Sie die **SetCorsPolicyProviderFactory** Erweiterungsmethode beim Start wie folgt:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a>Browserunterstützung

Die Web-API-CORS-Paket ist eine serverseitige Technologie. Außerdem muss der Browser des Benutzers CORS-Unterstützung. Die aktuellen Versionen von allen wichtigen Browsern zum Glück enthalten [Unterstützung für CORS](http://caniuse.com/cors).

Internet Explorer 8 und Internet Explorer 9 haben teilweise Unterstützung für CORS XMLHttpRequest anstelle der älteren XDomainRequest-Objekts. Weitere Informationen finden Sie unter [XDomainRequest - Einschränkungen und Einschränkungen sowie Problemumgehungen](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).
