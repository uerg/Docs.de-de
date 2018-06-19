---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Erste Schritte mit OWIN und Katana | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: ac0302ef1a786f6b1eef8119b3134a965f01c533
ms.sourcegitcommit: 5ab5c5f4bfdb0150f42ba84c2770eadf540cae48
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/28/2018
ms.locfileid: "30257677"
---
<a name="getting-started-with-owin-and-katana"></a>Erste Schritte mit OWIN und Katana
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Öffnen Sie die Weboberfläche für .NET (OWIN)](http://owin.org/) definiert eine Abstraktion zwischen .NET Webservern und Webanwendungen. Durch die Trennung des Servers aus der Anwendung, erleichtert OWIN Middleware für die Webentwicklung .NET zu erstellen. Darüber hinaus OWIN erleichtert es, Port-Webanwendungen zu anderen Hosts&#8212;z. B. Selbsthosting in einer Windows-Dienst oder ein anderer Prozess.

OWIN ist eine Spezifikation Community gehören, nicht um eine Implementierung. Das Katana-Projekt ist eine Reihe von Open Source-OWIN-Komponenten von Microsoft entwickelt wurde. Eine allgemeine Übersicht über OWIN und Katana finden Sie unter [eine Übersicht über Project Katana](an-overview-of-project-katana.md). In diesem Artikel springt ich rechts in Code, um zu beginnen.

Dieses Lernprogramm verwendet [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), aber Sie können auch Visual Studio 2012 verwenden. Nur einige der Schritte unterscheiden sich in Visual Studio 2012, die ich Beachten Sie unten.

## <a name="host-owin-in-iis"></a>Host OWIN in IIS

In diesem Abschnitt werden OWIN in IIS gehostet werden. Diese Option bietet Ihnen die Flexibilität und Composability von einer OWIN-Pipeline zusammen mit den ausgereifte Funktionsumfang von IIS. Bei Verwendung dieser Option wird die OWIN-Anwendung in der ASP.NET-Pipeline für die Anforderung ausgeführt.

Erstellen Sie zunächst ein neues ASP.NET Webanwendungsprojekt. (In Visual Studio 2012, verwenden Sie den Projekttyp leere ASP.NET-Webanwendung.)

![](getting-started-with-owin-and-katana/_static/image1.png)

In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **leere** Vorlage.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>Hinzufügen von NuGet-Paketen

Fügen Sie anschließend die erforderlichen NuGet-Pakete. Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Fügen Sie eine Startklasse

Fügen Sie anschließend eine OWIN-Start-Klasse. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **hinzufügen**, und wählen Sie dann **neues Element**. In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Owin Startklasse**. Weitere Informationen zum Konfigurieren der Startklasse finden Sie unter [OWIN Start Klasse Erkennung](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Fügen Sie der `Startup1.Configuration`-Methode den folgenden Code hinzu:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Dieser Code Fügt einen einfachen Teil Middleware an die OWIN-Pipeline, die als eine Funktion, die empfängt implementiert eine **Microsoft.Owin.IOwinContext** Instanz. Wenn der Server über eine HTTP-Anforderung empfängt, ruft die OWIN-Pipeline die Middleware. Die Middleware legt den Inhaltstyp für die Antwort und schreibt den Antworttext.

> [!NOTE]
> Die OWIN-Start-Klassenvorlage ist in Visual Studio 2013 verfügbar. Wenn Sie Visual Studio 2012 verwenden, hinzufügen eine neue leere Klasse mit dem Namen `Startup1`, und fügen Sie in den folgenden Code:


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Ausführen der Anwendung

Drücken Sie F5, um das Debuggen zu starten. Visual Studio öffnet ein Browserfenster mit `http://localhost:*port*/`. Die Seite sollte wie folgt aussehen:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>Selbsthosting OWIN in einer Konsolenanwendung

Es ist einfach, diese Anwendung zu konvertieren, von IIS-hosting Selbsthosting in einem benutzerdefinierten Prozess. Mit IIS-hosting fungiert IIS als den HTTP-Server und der Prozess, der den Dienst hostet. Mit Selbsthosting, Ihre Anwendung den Prozess erstellt und verwendet die **HttpListener** Klasse wie der HTTP-Server.

Erstellen Sie in Visual Studio eine neue Konsolenanwendung. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Hinzufügen einer `Startup1` Klasse aus Teil 1 dieses Lernprogramms zum Projekt. Sie müssen diese Klasse zu ändern.

Implementieren Sie der Anwendungsverzeichnis `Main` Methode wie folgt.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Wenn Sie die Konsolenanwendung ausführen, Start des Servers überwachen `http://localhost:9000`. Wenn Sie an diese Adresse in einem Webbrowser navigieren, sehen Sie die Seite "Hello World".

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Hinzufügen von OWIN Diagnostics.

Das Paket "Microsoft.owin.Diagnostics" enthält die Middleware, die nicht behandelte Ausnahmen abfängt und eine HTML-Seite mit Fehlerdetails angezeigt. Diese Seite funktioniert ähnlich wie die ASP.NET-Seite für Fehler, die in einigen Fällen aufgerufen wird, wird der "[gelben Bildschirm eines Computers zum Tod](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Wie die YSOD Katana-Fehlerseite eignet sich während der Entwicklung, aber es wird empfohlen, ihn in den Produktionsmodus zu deaktivieren.

Geben Sie den folgenden Befehl zum Installieren des Pakets für die Diagnose in Ihrem Projekt in der Paket-Manager-Konsole:

`install-package Microsoft.Owin.Diagnostics –Pre`

Ändern Sie den Code in Ihrem `Startup1.Configuration` Methode wie folgt:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Jetzt verwenden Sie STRG + F5, um die Anwendung ohne Debuggen auszuführen, damit Visual Studio nicht auf die Ausnahme unterbrochen wird. Die Anwendung verhält sich wie zuvor, bis Sie zu navigieren, `http://localhost/fail`, an welchem Punkt die Anwendung die Ausnahme auslöst. Die fehlerseitenmiddleware wird die Ausnahme abfangen und eine HTML-Seite mit Informationen zum Fehler anzuzeigen. Sie können die Registerkarten, um den Stapel, Abfragezeichenfolge, Cookies, Anforderungsheader und OWIN-Umgebungsvariablen finden Sie unter klicken.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Nächste Schritte

- [Erkennung der OWIN-Startup-Klasse](owin-startup-class-detection.md)
- [Mit der ASP.NET Web API Self Host OWIN](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Mit der SignalR Selbsthosting OWIN](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
