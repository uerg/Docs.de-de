---
uid: web-api/overview/advanced/httpclient-message-handlers
title: HttpClient-Meldungshandler in der ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/01/2012
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 764244d1299d8cfcb59c3f15d63b42ebff4f6ac0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837749"
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>HttpClient-Meldungshandler in der ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Ein *Meldungshandler* ist eine Klasse, die eine HTTP-Anforderung empfängt, und gibt eine HTTP-Antwort zurück.

In der Regel eine Reihe von Meldungshandler verkettet werden. Der erste Handler eine HTTP-Anforderung empfängt, erfolgt die Verarbeitung und gibt die Anforderung an dem nächsten Handler. An einem bestimmten Punkt wird die Antwort wird erstellt und wird wieder die Kette. Dieses Muster wird aufgerufen, eine *Delegieren* Handler.

![](httpclient-message-handlers/_static/image1.png)

Klicken Sie auf der Clientseite der **"HttpClient"** Klasse einen Meldungshandler verwendet, um Anforderungen zu verarbeiten. Der Standardhandler **HttpClientHandler**, die die Anforderung sendet, über das Netzwerk und die Antwort vom Server abgerufen. Sie können benutzerdefinierte Meldungshandler in der Client-Pipeline einfügen:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> ASP.NET Web-API verwendet Meldungshandler auch auf dem Server. Weitere Informationen finden Sie unter [HTTP-Meldungshandler](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Benutzerdefinierte Meldungshandler

Zum Schreiben von eines benutzerdefinierten Nachrichtenhandler abgeleitet **System.Net.Http.DelegatingHandler** und überschreiben die **SendAsync** Methode. So sieht die Signatur der Methode aus:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

Die Methode akzeptiert eine **HttpRequestMessage** als Eingabe und asynchron gibt ein **HttpResponseMessage**. Eine typische Implementierung führt Folgendes aus:

1. Verarbeitet die Anforderungsnachricht an.
2. Rufen Sie `base.SendAsync` zum Senden der Anforderung an den inneren Handler.
3. Der innere Handler gibt eine Antwortnachricht zurück. (Dieser Schritt ist asynchron.)
4. Die Antwort zu verarbeiten und an den Aufrufer zurückgeben.

Das folgende Beispiel zeigt einen Meldungshandler, der einen benutzerdefinierten Header zu einer ausgehenden Anforderung hinzufügt:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

Der Aufruf von `base.SendAsync` ist asynchron. Wenn der Handler für alle Vorgänge nach dem Aufruf ausführt, verwenden Sie die **"await"** Schlüsselwort zum Fortsetzen der Ausführung nach Abschluss der Methode. Das folgende Beispiel zeigt einen Handler, der Fehlercode protokolliert. Die Protokollierung selbst ist nicht besonders interessant, aber das Beispiel veranschaulicht, an die Antwort innerhalb des Handlers abzurufen.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Hinzufügen von Meldungshandlern für die Client-Pipeline

Hinzufügen von benutzerdefinierten Handler zum **"HttpClient"**, verwenden Sie die **HttpClientFactory.Create** Methode:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Message-Ereignishandler werden aufgerufen, in der Reihenfolge, die übergeben werden sie in der **erstellen** Methode. Da der Handler geschachtelt sind, wird die Response-Nachricht in die andere Richtung übertragen. Der letzte Handler ist, also den ersten, die Response-Nachricht.
