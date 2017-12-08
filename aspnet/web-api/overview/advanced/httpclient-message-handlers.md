---
uid: web-api/overview/advanced/httpclient-message-handlers
title: "Meldungshandler für \"HttpClient\" in ASP.NET Web-API | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a>Meldungshandler für "HttpClient" in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Ein *Nachrichtenhandler* ist eine Klasse, empfängt eine HTTP-Anforderung und gibt eine HTTP-Antwort zurück.

In der Regel eine Reihe von Meldungshandler verkettet werden. Der erste Handler empfängt eine HTTP-Anforderung, Verarbeitung und die Anforderung an dem nächsten Handler ermöglicht. Die Antwort wird erstellt und herabgesetzt aufwärts in der Kette, zu einem späteren Zeitpunkt. Dieses Muster wird aufgerufen, eine *Delegieren* Handler.

![](httpclient-message-handlers/_static/image1.png)

Auf der Clientseite der **"HttpClient"** Klasse einen Meldungshandler verwendet, um Anforderungen zu verarbeiten. Der Standardhandler ist **HttpClientHandler**, die die Anforderung über das Netzwerk sendet und ruft die Antwort vom Server ab. Sie können benutzerdefinierte Meldungshandler in die Client-Pipeline einfügen:

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> ASP.NET Web-API verwendet Meldungshandler auch auf der Serverseite. Weitere Informationen finden Sie unter [HTTP-Meldungshandler](http-message-handlers.md).


## <a name="custom-message-handlers"></a>Benutzerdefinierte Meldungshandler

Leiten Sie zum Schreiben eines benutzerdefinierten Message-Handlers aus **System.Net.Http.DelegatingHandler** und überschreiben die **"SendAsync"** Methode. Hier wird die Signatur der Methode:

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

Die Methode nimmt ein **HttpRequestMessage** als Eingabe und asynchron gibt eine **HttpResponseMessage**. Eine typische Implementierung führt Folgendes aus:

1. Verarbeitet die Anforderungsnachricht an.
2. Rufen Sie `base.SendAsync` zum Senden der Anforderung an den internen Handler.
3. Der innere Handler gibt eine Antwortnachricht zurück. (Dieser Schritt ist asynchron.)
4. Verarbeiten der Antwort, und geben sie an den Aufrufer zurück.

Das folgende Beispiel zeigt einen Message-Handler, der einen benutzerdefinierten Header der ausgehenden Anforderung hinzufügt:

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

Der Aufruf von `base.SendAsync` asynchron ist. Wenn der Handler nach diesem Aufruf Arbeit ausführt, verwenden Sie die **"await"** Schlüsselwort zum Fortsetzen der Ausführung nach Abschluss der Methode. Im folgenden Beispiel wird eines Handlers, der Fehlercodes protokolliert. Die Protokollierung selbst ist nicht besonders interessant, aber im Beispiel veranschaulicht, an die Antwort im Handler abzurufen.

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a>Hinzufügen von Meldungshandlern an die Client-Pipeline

Hinzufügen von benutzerdefinierten Handler **"HttpClient"**, verwenden Sie die **HttpClientFactory.Create** Methode:

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

Message-Ereignishandler werden aufgerufen, in der Reihenfolge ab, das Sie Sie sie in übergeben der **erstellen** Methode. Da der Handler geschachtelt sind, durchläuft die Antwortnachricht in die andere Richtung. Der letzte Handler ist, also zuerst die Antwortnachricht.
