---
uid: web-api/overview/advanced/http-message-handlers
title: HTTP-Meldungshandler in der ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 02/13/2012
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 0b0d7b4c543dc4e597c6c472083898f3a8095a83
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835842"
---
<a name="http-message-handlers-in-aspnet-web-api"></a>HTTP-Meldungshandler in der ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Ein *Meldungshandler* ist eine Klasse, die eine HTTP-Anforderung empfängt, und gibt eine HTTP-Antwort zurück. Meldungshandler abgeleitet werden, von der abstrakten **HttpMessageHandler** Klasse.

In der Regel eine Reihe von Meldungshandler verkettet werden. Der erste Handler eine HTTP-Anforderung empfängt, erfolgt die Verarbeitung und gibt die Anforderung an dem nächsten Handler. An einem bestimmten Punkt wird die Antwort wird erstellt und wird wieder die Kette. Dieses Muster wird aufgerufen, eine *Delegieren* Handler.

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a>Serverseitige-Meldungshandler

Auf der Serverseite verwendet die Web-API-Pipeline einige integrierte-Meldungshandler:

- **HttpServer** vom Host der Anforderung ab.
- **HttpRoutingDispatcher** sendet die Anforderung auf Grundlage der Route.
- **HttpControllerDispatcher** sendet die Anforderung an einen Web-API-Controller.

Sie können benutzerdefinierte Handler zur Pipeline hinzufügen. Nachrichtenhandler sind gut für die übergreifende Probleme, die auf der Ebene der HTTP-Nachrichten (statt über Controlleraktionen) ausgeführt werden. Beispielsweise können ein Nachrichtenhandler:

- Lesen oder Ändern von Anforderungsheadern.
- Hinzufügen eines Antwortheaders zu antworten.
- Überprüfen Sie Anforderungen, bevor sie den Controller erreichen.

Dieses Diagramm zeigt zwei benutzerdefinierte Handler, die in der Pipeline eingefügt:

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> Klicken Sie auf der Clientseite verwendet "HttpClient" auch Meldungshandler. Weitere Informationen finden Sie unter [HttpClient-Meldungshandler](httpclient-message-handlers.md).


## <a name="custom-message-handlers"></a>Benutzerdefinierte Meldungshandler

Zum Schreiben von eines benutzerdefinierten Nachrichtenhandler abgeleitet **System.Net.Http.DelegatingHandler** und überschreiben die **SendAsync** Methode. Diese Methode hat die folgende Signatur:

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

Die Methode akzeptiert eine **HttpRequestMessage** als Eingabe und asynchron gibt ein **HttpResponseMessage**. Eine typische Implementierung führt Folgendes aus:

1. Verarbeitet die Anforderungsnachricht an.
2. Rufen Sie `base.SendAsync` zum Senden der Anforderung an den inneren Handler.
3. Der innere Handler gibt eine Antwortnachricht zurück. (Dieser Schritt ist asynchron.)
4. Die Antwort zu verarbeiten und an den Aufrufer zurückgeben.

Hier ist ein sehr einfaches Beispiel:

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> Der Aufruf von `base.SendAsync` ist asynchron. Wenn der Handler für alle Vorgänge nach dem Aufruf ausführt, verwenden Sie die **"await"** -Schlüsselwort, wie gezeigt.


Ein delegierender Handler kann den inneren Handler auch überspringen und direkt die Antwort zu erstellen:

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

Wenn eine Delegierung Ereignishandler erstellt die Antwort ohne `base.SendAsync`, die Anforderung wird übersprungen, den Rest der Pipeline. Dies kann nach einem Handler nützlich sein, der die Anforderung (erstellen eine Fehlerantwort) überprüft.

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a>Hinzufügen eines Handlers für die Pipeline

Um einen Meldungshandler auf dem Server hinzuzufügen, fügen Sie den Handler, der die **HttpConfiguration.MessageHandlers** Auflistung. Wenn Sie die Vorlage "ASP.NET MVC 4-Webanwendung" verwendet, um das Projekt zu erstellen, erreichen Sie dies in der **WebApiConfig** Klasse:

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

Message-Ereignishandler werden aufgerufen, in der gleichen Reihenfolge, die sie in angezeigt werden **MessageHandlers** Auflistung. Da sie geschachtelt sind, wird die Response-Nachricht in die andere Richtung übertragen. Der letzte Handler ist, also den ersten, die Response-Nachricht.

Beachten Sie, dass Sie nicht der internen Handler festlegen müssen. die Web-API-Framework wird automatisch der Meldungshandler hergestellt.

Möchten [Selbsthosting](../older-versions/self-host-a-web-api.md), erstellen Sie eine Instanz von der **HttpSelfHostConfiguration** -Klasse und die Handler zum Hinzufügen der **MessageHandlers** Auflistung.

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

Jetzt sehen wir uns einige Beispiele für benutzerdefinierte Meldungshandler.

## <a name="example-x-http-method-override"></a>Beispiel: X-HTTP-Method-Override-

X-HTTP-Method-Override ist einem nicht standardmäßigen HTTP-Header. Es dient für Clients, die bestimmte HTTP-Anforderungstypen, z. B. PUT oder DELETE senden können. Stattdessen wird der Client sendet eine POST-Anforderung, und legt den X-HTTP-Method-Override-Header auf die gewünschte Methode. Zum Beispiel:

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

Hier ist ein Meldungshandler, das Unterstützung für X-HTTP-Method-Override aus:

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

In der **SendAsync** -Methode, der Handler überprüft wird, gibt an, ob die Anforderungsnachricht für eine POST-Anforderung ist, und ob es sich um den X-HTTP-Method-Override-Header enthält. Wenn dies der Fall ist, überprüft den Wert von Header und ändert anschließend die Anforderungsmethode. Der Handler zum Schluss ruft `base.SendAsync` die Nachricht an den nächsten Handler übergeben.

Wenn die Anforderung erreicht den **HttpControllerDispatcher** -Klasse, **HttpControllerDispatcher** leitet die Anforderung, die basierend auf dem die aktualisierte Anforderungsmethode.

## <a name="example-adding-a-custom-response-header"></a>Beispiel: Hinzufügen eines benutzerdefinierten Antwortheaders

Hier ist ein Message-Handler, der einen benutzerdefinierten Header zu jeder Response-Nachricht hinzufügt:

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

Zunächst ruft der Handler `base.SendAsync` die Anforderung an den inneren Meldungshandler übergeben. Der innere Handler gibt eine Antwortnachricht zurück, sondern nur asynchron mit einem **Aufgabe&lt;T&gt;**  Objekt. Die Response-Nachricht ist nicht verfügbar, bis `base.SendAsync` asynchron abgeschlossen.

Dieses Beispiel verwendet die **"await"** Schlüsselwort, um die Aufgaben asynchron nach `SendAsync` abgeschlossen ist. Wenn Sie .NET Framework 4.0 ausgerichtet sind, verwenden Sie die **Aufgabe**&lt;T&gt;**. ContinueWith** Methode:

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a>Beispiel: Überprüfen eines API-Schlüssels

Einige Webdienste werden Clients für einen API-Schlüssel in der Anforderung angeben müssen. Das folgende Beispiel zeigt, wie ein Meldungshandler Anforderungen für einen gültigen API-Schlüssel überprüfen kann:

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

Dieser Handler sucht nach den API-Schlüssel in der URI-Abfragezeichenfolge. (In diesem Beispiel wird davon ausgegangen, dass der Schlüssel eine statische Zeichenfolge. "Eine realen Implementierung würden wahrscheinlich eine komplexere Validierung verwenden.) Wenn die Abfragezeichenfolge der Schlüssel enthält, übergibt der Handler für die Anforderung an den inneren Handler.

Wenn die Anforderung nicht über einen gültigen Schlüssel verfügt, erstellt der Handler für eine Antwortnachricht mit dem Status 403, verboten. In diesem Fall ruft der Handler für nicht auf `base.SendAsync`, also der innere Handler empfängt die Anforderung, noch wird des Controllers. Der Controller kann daher davon ausgehen, dass alle eingehende Anforderungen über einen gültigen API-Schlüssel verfügen.

> [!NOTE]
> Wenn der API-Schlüssel nur für bestimmte Controlleraktionen gilt, erwägen Sie, die einen Aktionsfilter anstelle eines meldungshandlers. Aktionsfilter werden nach dem URI routing ausgeführt wird.


## <a name="per-route-message-handlers"></a>Pro-Route-Meldungshandler

-Handler in der **HttpConfiguration.MessageHandlers** Auflistung global angewendet werden.

Alternativ können Sie einen Meldungshandler für eine bestimmte Route hinzufügen, wenn Sie die Route definieren:

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

In diesem Beispiel, wenn der Anforderungs-URI "Route2", entspricht die Anforderung wird gesendet `MessageHandler2`. Das folgende Diagramm zeigt die Pipeline für diese zwei Routen:

![](http-message-handlers/_static/image4.png)

Beachten Sie, dass `MessageHandler2` ersetzt das standardmäßige **HttpControllerDispatcher**. In diesem Beispiel `MessageHandler2` erstellt die Antwort und -Anforderungen, die mit "Route2" Gehen Sie niemals auf einen Controller übereinstimmen. Dadurch können Sie den gesamten Web-API-Controller-Mechanismus durch Ihren eigenen benutzerdefinierten Endpunkt zu ersetzen.

Alternativ kann ein Message-Handler pro Route an Delegieren **HttpControllerDispatcher**, die dann an einen Controller sendet.

![](http-message-handlers/_static/image5.png)

Der folgende Code zeigt, wie Sie diese Route zu konfigurieren:

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
