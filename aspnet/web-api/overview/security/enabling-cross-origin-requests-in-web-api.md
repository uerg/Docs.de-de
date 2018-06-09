---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Aktivieren von Cross-Origin-Anforderungen in der ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Zeigt, wie in ASP.NET Web-API-Cross-Origin Resource Sharing (CORS) unterstützen.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/15/2014
ms.topic: article
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 453ad29ff4f10f9660f3aa8bab358519b4cfd48b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508379"
---
<a name="enabling-cross-origin-requests-in-aspnet-web-api-2"></a>Cross-Origin-Anforderungen in der ASP.NET Web API 2 aktivieren
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Browsersicherheit wird verhindert, dass eine Webseite, die AJAX-Anforderungen in eine andere Domäne. Diese Einschränkung wird aufgerufen, die *gleichen Origin-Richtlinie*, und verhindert, dass eine bösartige Website vertrauliche Daten von einem anderen Standort lesen. Möglicherweise in einigen Fällen möchten Sie jedoch auf andere Standorte Ihrer Web-API aufrufen können.
> 
> [Cross Origin Resource Sharing](http://www.w3.org/TR/cors/) (CORS) ist ein W3C-Standard, der einem Server ermöglicht, die gleichen-Origin-Richtlinie weniger streng gehandhabt. Verwenden CORS, können ein Servers explizit einige Cross-Origin-Anforderungen beim Ablehnen von anderen Benutzern. CORS ist eine sicherere und flexibler als die früheren Methoden wie z. B. [JSONP](http://en.wikipedia.org/wiki/JSONP). Dieses Lernprogramm zeigt, wie CORS in der Web-API-Anwendung zu aktivieren.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013 Update 2](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web-API 2.2


<a id="intro"></a>
## <a name="introduction"></a>Einführung

Dieses Lernprogramm veranschaulicht, dass die in ASP.NET Web API CORS-Unterstützung. Wir beginnen mit zwei ASP.NET-Projekte – eine aufgerufene "WebService", die einen Web-API-Controller hostet, und den anderen aufgerufenen "WebClient", der Webdienst aufgerufen wird, erstellen. Da die beiden Anwendungen auf verschiedenen Domänen gehostet werden, ist eine AJAX-Anforderung vom WebClient zum Webdienst eine Cross-Origin-Anforderung.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>Was ist "Gleichen Origin"?

Zwei URLs müssen denselben Ursprung aus, wenn sie identische Schemas, Hosts und Ports verfügen. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Diese beiden URLs haben die gleichen Ursprungs:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Diese URLs haben unterschiedliche Ursprünge als den vorherigen zwei:

- `http://example.net` -Der anderen Domäne
- `http://example.com:9000/foo.html` -Anschluss
- `https://example.com/foo.html` -Anderes Schema
- `http://www.example.com/foo.html` -Andere Unterdomäne

> [!NOTE]
> Den Port wird von Internet Explorer nicht berücksichtigt werden, für den Vergleich von Ursprüngen.


<a id="create-webapi-project"></a>
## <a name="create-the-webservice-project"></a>Erstellen des Projekts WebService

> [!NOTE]
> In diesem Abschnitt wird davon ausgegangen, dass Sie bereits wissen, wie Web-API-Projekte erstellen. Falls nicht, siehe [erste Schritte mit ASP.NET Web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).


Starten Sie Visual Studio und erstellen Sie ein neues **ASP.NET-Webanwendung** Projekt. Wählen Sie die **leere** -Projektvorlage. Klicken Sie unter "Hinzufügen von Ordnern und Verweise für core" Wählen Sie die **Web-API** Kontrollkästchen. Wählen Sie optional, die Option "Host in der Cloud" die app in Microsoft Azure bereitgestellt. Microsoft bietet kostenlose Webhosting für bis zu 10 Websites in einer [frei von Azure-Testkonto](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

[![](enabling-cross-origin-requests-in-web-api/_static/image3.png)](enabling-cross-origin-requests-in-web-api/_static/image2.png)

Fügen Sie einen Web-API-Controller mit dem Namen `TestController` durch den folgenden Code:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

Sie können die Anwendung lokal ausführen oder in Azure bereitstellen. (Die Screenshots in diesem Lernprogramm bereitgestellt ich auf Azure App Service-Web-Apps.) Navigieren Sie zu, um sicherzustellen, dass die Web-API arbeitet, `http://hostname/api/test/`, wobei *Hostname* die Domäne, wo Sie die Anwendung bereitgestellt, wird. Sehen Sie den Antworttext &quot;abrufen: Testnachricht&quot;.

![](enabling-cross-origin-requests-in-web-api/_static/image4.png)

<a id="create-client"></a>
## <a name="create-the-webclient-project"></a>Erstellen des Projekts WebClient

Erstellen Sie ein anderes ASP.NET-Webanwendung-Projekt, und wählen Sie die **MVC** -Projektvorlage. Wählen Sie optional **Authentifizierung ändern** > **keine Authentifizierung**. Für dieses Lernprogramm benötigen Sie keine Authentifizierung.

[![](enabling-cross-origin-requests-in-web-api/_static/image6.png)](enabling-cross-origin-requests-in-web-api/_static/image5.png)

Öffnen Sie im Projektmappen-Explorer die Datei Views/Home/Index.cshtml. Ersetzen Sie den Code in dieser Datei durch Folgendes:

[!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

Für die *ServiceUrl* Variable, verwenden Sie den URI der WebService-app. Jetzt führen Sie den WebClient-app lokal oder in einer anderen Website veröffentlichen.

Klicken auf die Schaltfläche "Versuchen Sie es" sendet eine AJAX-Anforderung an den WebService-app, die mithilfe der HTTP-Methode aufgeführt, die der Dropdown-Feld (GET, POST oder PUT). Auf diese Weise verschiedene Cross-Origin-Anforderungen zu überprüfen. Jetzt, die WebService-app unterstützt keine CORS, sodass, wenn Sie die Schaltfläche klicken, Sie einen Fehler erhalten.

![](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Wenn Sie sehen Sie sich den HTTP-Datenverkehr in einem Tool wie [Fiddler](http://www.telerik.com/fiddler), sehen Sie, dass der Browser die GET-Anforderung sendet und die Anforderung erfolgreich ist, aber die AJAX-Aufruf gibt einen Fehler zurück. Es ist wichtig zu verstehen, dass durch den Browser nicht vom gleichen Origin-Richtlinie verhindert wird *senden* der Anforderung. Stattdessen es verhindert, dass die Anwendung wird angezeigt, die *Antwort*.


![](enabling-cross-origin-requests-in-web-api/_static/image8.png)

<a id="enable-cors"></a>
## <a name="enable-cors"></a>Aktivieren Sie CORS

Jetzt aktivieren wir in der app WebService CORS. Fügen Sie zuerst die CORS-NuGet-Paket hinzu. In Visual Studio aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Mit diesem Befehl wird das aktuellste Paket installiert und aktualisiert alle Abhängigkeiten, einschließlich der Core-Web-API-Bibliotheken. Benutzer-versionsflag, das eine bestimmte Version abzielen. Das CORS-Paket ist die Web-API 2.0 oder höher erforderlich.

Öffnen Sie die Datei App\_Start/WebApiConfig.cs. Fügen Sie folgenden Code, der **WebApiConfig.Register** Methode.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

Als Nächstes fügen Sie der **[EnableCors]** -Attribut auf die `TestController` Klasse:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

Für die *Ursprünge* Parameter, verwenden Sie den URI, wo Sie den WebClient-Anwendung bereitgestellt. Dadurch Cross-Origin-Anfragen von WebClient, während alle anderen domänenübergreifende Anforderungen weiterhin untersagen. Ich werde später wird beschrieben, die Parameter für **[EnableCors]** im Detail.

Schließen Sie keinen Schrägstrich am Ende der *Ursprünge* URL.

Erneute Bereitstellung der aktualisierten WebService-Anwendung aus. Sie müssen nicht den WebClient zu aktualisieren. Die AJAX-Anforderung vom WebClient sollte jetzt erfolgreich sein. Die GET, PUT und POST-Methoden sind alle zulässig.

![](enabling-cross-origin-requests-in-web-api/_static/image9.png)

<a id="how-it-works"></a>
## <a name="how-cors-works"></a>Funktionsweise von CORS

In diesem Abschnitt wird beschrieben, was geschieht, in einer CORS-Anforderung auf der Ebene der HTTP-Nachrichten. Es ist wichtig zum Verständnis der Funktionsweise von CORS, sodass Sie konfigurieren können die **[EnableCors]** ordnungsgemäß Attribut, und beheben, wenn Elemente nicht wie erwartet funktionieren.

CORS-Spezifikation führt mehrere neue HTTP-Header, die Cross-Origin-Anforderungen zu ermöglichen. Wenn ein Browser CORS unterstützt, wird diese Header automatisch für Cross-Origin-Anfragen; Sie müssen besondere im JavaScript-Code keine Wirkung.

Hier ist ein Beispiel einer Cross-Origin-Anforderung. Der Header "Origin" kann die Domäne des Standorts, der die Anforderung stammt.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Wenn der Server die Anforderung zulässt, wird die Access-Control-Allow-Origin-Header festgelegt. Der Wert dieses Headers entspricht den Origin-Header oder Wert für die Platzhalter "\*", Bedeutung, die einen beliebigen Ursprung zulässig ist.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Wenn die Antwort den Access-Control-Allow-Origin-Header nicht enthalten ist, schlägt fehl, die AJAX-Anforderung. Der Browser lässt, die die Anforderung. Selbst wenn der Server eine erfolgreiche Antwort zurückgibt, wird der Browser nicht die Antwort an die Clientanwendung zur Verfügung.

**Preflight-Anforderungen**

Für einige CORS-Anforderungen sendet der Browser eine zusätzliche Anforderung, eine "preflight-Anforderung", aufgerufen, bevor die tatsächliche Anforderung für die Ressource gesendet.

Der Browser kann die preflight-Anforderung überspringen, wenn Folgendes zutrifft:

- Die Anforderungsmethode ist GET, HEAD oder POST, *und*
- Die Anwendung wird nicht festgelegt Anforderungsheader als Accept, Accept-Language-Content-Language, Content-Type oder letzten-Ereignis-ID *und*
- Der Content-Type-Header (falls festgelegt) ist eines der folgenden: 

    - application/x-www-form-urlencoded
    - Multipart/Form-data
    - Text/plain

Die Regel zu Anforderungsheader gilt für Header, die durch Aufrufen der Anwendung festlegt **SetRequestHeader** auf die **XMLHttpRequest** Objekt. (Die CORS-Spezifikation ruft diese "Autor Anforderungsheader".) Die Regel gilt nicht für Header der *Browser* können "Benutzer-Agent-Host" oder "Content-Length festlegen.

Hier ist ein Beispiel für eine preflight-Anforderung:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

Die Preflight-Anforderung mithilfe der HTTP OPTIONS-Methode. Sie schließt zwei spezielle Header:

- Access-Control-Request-Method: Die HTTP-Methode, die für die tatsächliche Anforderung verwendet werden.
- Access-Control-Request-Headers: Eine Liste der Anforderungsheader, die die *Anwendung* legen Sie für die Anforderung tatsächlich bearbeitet. (Erneut, schließt dies keine Header, die der Browser legt ein.)

Hier ist eine Beispielantwort, vorausgesetzt, dass der Server die Anforderung zulässt:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

Die Antwort enthält eine Access-Control-Allow-Methods-Header, der die zulässigen Methoden aufgelistet, und optional eine Access-Control-Allow-Headers-Header, der Listet die erlaubten Header. Wenn die preflight-Anforderung erfolgreich ist, sendet der Browser die tatsächliche Anforderung an, wie zuvor beschrieben.

<a id="scope"></a>
## <a name="scope-rules-for-enablecors"></a>Bereichsregeln für [EnableCors]

Sie können CORS pro Aktion, die pro Controller oder global für alle Web-API-Controller in der Anwendung aktivieren.

**Pro Aktion**

Legen Sie zum Aktivieren von CORS für eine einzelne Aktion die **[EnableCors]** Attribut auf die Aktionsmethode. Im folgenden Beispiel wird CORS für den `GetItem` nur Methode.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Pro Controller**

Wenn Sie festlegen, **[EnableCors]** auf der Controller-Klasse, die sie für alle Aktionen auf dem Controller gilt. Um CORS für eine Aktion zu deaktivieren, Hinzufügen der **[DisableCors]** -Attribut auf die Aktion. Im folgenden Beispiel wird CORS für jede Methode, außer `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Global**

Übergeben Sie zum Aktivieren von CORS für alle Web-API-Controller in der Anwendung ein **EnableCorsAttribute** -Instanz, auf die **EnableCors** Methode:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Wenn Sie das Attribut auf mehr als ein Bereich festlegen, wird die Reihenfolge der Rangfolge:

1. Aktion
2. Controller
3. Global

<a id="allowed-origins"></a>
## <a name="set-the-allowed-origins"></a>Legen Sie die zulässigen Ursprünge

Die *Ursprünge* Parameter von der **[EnableCors]** Attribut gibt an, welche Ursprungsdomänen zulässig sind, auf die Ressource zuzugreifen. Der Wert ist eine durch Trennzeichen getrennte Liste zulässiger Ursprünge.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

Sie können auch den Platzhalterwert "\*" um Anforderungen über alle Ursprünge zuzulassen.

Sollten Sie sorgfältig überlegen, bevor Anforderungen über einen beliebigen Ursprung zugelassen. Dies bedeutet, dass es sich bei wörtlich in eine beliebige Website AJAX-Aufrufe, um Ihre Web-API vornehmen kann.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

<a id="allowed-methods"></a>
## <a name="set-the-allowed-http-methods"></a>Legen Sie die zulässigen HTTP-Methoden

Die *Methoden* Parameter von der **[EnableCors]** Attribut gibt an, welche HTTP-Methoden zulässig sind, auf die Ressource zuzugreifen. Um alle Methoden zu ermöglichen, verwenden Sie den Platzhalterwert "\*". Im folgende Beispiel kann nur Get- und POST-Anforderungen.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

<a id="allowed-request-headers"></a>
## <a name="set-the-allowed-request-headers"></a>Legt die zulässige Anforderungsheader fest.

Ich beschriebenen wie eine preflight-Anforderung für einen Access-Control-Request-Headers-Header enthalten, kann die HTTP-Header, die von der Anwendung festgelegtes auflisten (den so genannten "author Anforderungsheader"). Die *Header* Parameter von der **[EnableCors]** Attribut gibt an, welche Autor Anforderungsheader zulässig sind. Um alle Header zu ermöglichen, legen *Header* zu "\*". Legen Sie auf bestimmte Positivlisten-Header, *Header* auf eine durch Trennzeichen getrennte Liste der zulässigen Header:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Browser sind jedoch nicht vollständig in diese Festlegung von Access-Control-Request-Headers konsistent. Beispielsweise enthält Chrome derzeit "Origin"; Wenn FireFox keine Standardheader z. B. "Accept", enthält selbst wenn die Anwendung im Skript festlegt.

Wenn Sie festlegen, *Header* zu beliebiegen Dokumentbestandteilen außer "\*", Sie sollten berücksichtigen mindestens "Annehmen", "Content-Type" und "Origin" sowie alle benutzerdefinierten Header, die Sie unterstützen möchten.

<a id="allowed-response-headers"></a>
## <a name="set-the-allowed-response-headers"></a>Die zulässigen Antwortheader festlegen

Standardmäßig macht der Browser nicht alle der Antwortheader für die Anwendung verfügbar. Die Antwortheader, die standardmäßig verfügbar sind:

- Cache-Control
- Content-Language
- Inhaltstyp
- Läuft ab
- Zuletzt geändert
- Pragma

Die CORS-Spezifikation ruft diese [einfache Antwortheader](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Um weitere Header für die Anwendung verfügbar zu machen, legen Sie die *ExposedHeaders* Parameter **[EnableCors]**.

Im folgenden Beispiel die Controller des `Get` Methode legt einen benutzerdefinierten Header mit dem Namen "X-Custom-Header". Standardmäßig wird der Browser nicht diesen Header in einer Anforderung Cross-Origin verfügbar. Damit den Header verfügbar ist, schließen Sie "X-Custom-Header" in *ExposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

<a id="credentials"></a>
## <a name="passing-credentials-in-cross-origin-requests"></a>Übergabe von Anmeldeinformationen in Cross-Origin-Anforderungen

Anmeldeinformationen erfordern eine besondere Behandlung in einer CORS-Anforderung. Standardmäßig sendet der Browser keine Anmeldeinformationen mit einem Cross-Origin-Anforderung. Anmeldeinformationen werden Cookies sowie HTTP-Authentifizierungsschemas einschließen. Um Anmeldeinformationen mit einem Cross-Origin-Anforderung zu senden, muss der Client festgelegt **XMLHttpRequest.withCredentials** auf "true".

Mit **XMLHttpRequest** direkt:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

In jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Darüber hinaus muss der Server die Anmeldeinformationen zulassen. Um Cross-Origin-Anmeldeinformationen im Web-API zu ermöglichen, legen die **SupportsCredentials** Eigenschaft auf "true", auf die **[EnableCors]** Attribut:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Wenn diese Eigenschaft auf "true" festgelegt ist, wird die HTTP-Antwort einen Access-Control-Allow-Credentials-Header enthalten. Dieser Header wird der Browser angewiesen, dass der Server die Anmeldeinformationen für eine Anforderung Cross-Origin zulässt.

Wenn der Browser Anmeldeinformationen sendet, aber die Antwort enthält keinen gültigen Access-Control-Allow-Credentials-Header, der Browser wird die Antwort an die Anwendung nicht verfügbar, und die AJAX-Anforderung schlägt fehl.

Werden nur mit großer Vorsicht Einstellung **SupportsCredentials** auf "true", da es bedeutet, dass eine Website finden Sie unter einer anderen Domäne kann auf Ihre Web-API im Auftrag des Benutzers, eines angemeldeten Benutzers Anmeldeinformationen senden, ohne dass der Benutzer die Erkennung,. Die CORS-Spezifikation besagt ebenfalls diese Einstellung *Ursprünge* auf &quot; \* &quot; ist ungültig Wenn **SupportsCredentials** ist "true".

<a id="cors-policy-providers"></a>
## <a name="custom-cors-policy-providers"></a>Anbieter für benutzerdefinierte CORS-Richtlinie

Die **[EnableCors]** -Attribut implementiert die **ICorsPolicyProvider** Schnittstelle. Sie können eine eigene Implementierung bereitzustellen, durch Erstellen einer Klasse, die abgeleitet **Attribut** und implementiert **ICorsProlicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Nachdem Sie das Attribut anwenden können, die jeden Ort, dass Sie eingefügt würde **[EnableCors]**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Eine benutzerdefinierte CORS-Richtlinienanbieter konnte z. B. die Einstellungen aus einer Konfigurationsdatei lesen.

Als Alternative zum Verwenden von Attributen, registrieren Sie eine **ICorsPolicyProviderFactory** Objekt, das erstellt **ICorsPolicyProvider** Objekte.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Festlegen der **ICorsPolicyProviderFactory**, rufen Sie die **SetCorsPolicyProviderFactory** Erweiterungsmethode beim Start wie folgt:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

<a id="browser-support"></a>
## <a name="browser-support"></a>Browserunterstützung

Das Web-API-CORS-Paket ist eine serverseitige-Technologie. Der Browser des Benutzers muss auch die CORS-Unterstützung. Die aktuellen Versionen von allen wichtigen Browsern Glücklicherweise enthalten [Unterstützung für CORS](http://caniuse.com/cors).

Internet Explorer 8 und Internet Explorer 9 wurden teilweise Unterstützung für CORS XMLHttpRequest XDomainRequest legacyobjekte anstelle. Weitere Informationen finden Sie unter [XDomainRequest - Einschränkungen, Einschränkungen und Abhilfen](https://blogs.msdn.com/b/ieinternals/archive/2010/05/13/xdomainrequest-restrictions-limitations-and-workarounds.aspx).
