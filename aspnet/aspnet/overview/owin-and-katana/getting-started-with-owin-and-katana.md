---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: Erste Schritte mit OWIN und Katana | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 9920861da0e67d9304a944cacfb8ff8685267cd6
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913176"
---
<a name="getting-started-with-owin-and-katana"></a>Erste Schritte mit OWIN und Katana
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Öffnen Sie Web Interface for .NET (OWIN)](http://owin.org/) definiert eine Abstraktion zwischen Webservern für .NET und Webanwendungen. Durch die Entkopplung des Webserver, aus der Anwendung, erleichtert OWIN-Middleware für .NET Web-Entwicklung zu erstellen. Darüber hinaus OWIN erleichtert es, Port-Webanwendungen zu anderen Hosts&#8212;z. B. Self-hosting in einem Windows-Dienst oder einen anderen Prozess.

OWIN ist eine Spezifikation Community gehören, nicht auf eine Implementierung. Das Katana-Projekt ist eine Reihe von Open Source-OWIN-Komponenten, die von Microsoft entwickelt wurde. Eine allgemeine Übersicht über OWIN und Katana, finden Sie unter [eine Übersicht des Katana-Projekt](an-overview-of-project-katana.md). In diesem Artikel werden ich direkt in Code für den Einstieg springen.

Dieses Tutorial verwendet [Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566), aber Sie können auch Visual Studio 2012 verwenden. Ein paar Schritte unterscheiden sich in Visual Studio 2012, die ich Beachten Sie unten.

## <a name="host-owin-in-iis"></a>Hosten von OWIN in IIS

In diesem Abschnitt werden wir OWIN in IIS hosten. Diese Option bietet Ihnen die Flexibilität und zusammensetzbarkeit von einer OWIN-Pipeline zusammen mit dem ausgereifte Features von IIS. Mit dieser Option wird die OWIN-Anwendung in der ASP.NET-Pipeline für die Anforderung ausgeführt.

Erstellen Sie zunächst ein neues ASP.NET-Webanwendungsprojekt. (In Visual Studio 2012, verwenden Sie den Projekttyp leere ASP.NET-Webanwendung.)

![](getting-started-with-owin-and-katana/_static/image1.png)

In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **leere** Vorlage.

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>NuGet-Pakete hinzufügen

Als Nächstes fügen Sie die erforderlichen NuGet-Pakete hinzu. Von der **Tools** , wählen Sie im Menü **NuGet Package Manager**, und wählen Sie dann **-Paket-Manager-Konsole**. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Fügen Sie eine Startklasse

Fügen Sie anschließend eine OWIN-Startup-Klasse. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **hinzufügen**, und wählen Sie dann **neues Element**. In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Owin-Startklasse**. Weitere Informationen zum Konfigurieren der Startup-Klasse finden Sie unter [OWIN-Startup-Klasse Erkennung](owin-startup-class-detection.md).

![](getting-started-with-owin-and-katana/_static/image4.png)

Fügen Sie der `Startup1.Configuration`-Methode den folgenden Code hinzu:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

Dieser Code Fügt eine einfache Aufgabe der Middleware OWIN-Pipeline, die als eine Funktion, die empfängt implementiert eine **Microsoft.Owin.IOwinContext** Instanz. Wenn der Server eine HTTP-Anforderung empfängt, ruft die OWIN-Pipeline die Middleware. Die Middleware legt den Inhaltstyp für die Antwort fest und schreibt den Antworttext.

> [!NOTE]
> Die OWIN-Startup-Klassenvorlage ist in Visual Studio 2013 verfügbar. Wenn Sie Visual Studio 2012 verwenden, fügen Sie einfach eine neue leere Klasse, die mit dem Namen `Startup1`, und fügen Sie in den folgenden Code:


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>Ausführen der Anwendung

Drücken Sie F5, um das Debuggen zu beginnen. Visual Studio öffnet ein Browserfenster mit `http://localhost:*port*/`. Die Seite sollte wie folgt aussehen:

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>Self-Hosting von OWIN in einer Konsolenanwendung

Es ist einfach, diese Anwendung zu konvertieren, von IIS hosten, selbst in einem benutzerdefinierten Prozess hosten. Mit IIS zu hosten fungiert IIS, als der HTTP-Server und den Prozess, der den Dienst hostet. Dem Selbsthosting, Ihre Anwendung wird der Prozess erstellt und verwendet die **HttpListener** Klasse wie der HTTP-Server.

Erstellen Sie eine neue Konsolenanwendung in Visual Studio. Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:

`Install-Package Microsoft.Owin.SelfHost -Pre`

Hinzufügen einer `Startup1` Klasse aus Teil 1 dieses Lernprogramms zum Projekt. Sie müssen nicht diese Klasse zu ändern.

Implementieren der Anwendung `Main` -Methode wie folgt.

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

Wenn Sie die Konsolenanwendung ausführen, wird der Server startet, Lauschen auf `http://localhost:9000`. Wenn Sie in einem Webbrowser zu dieser Adresse navigieren, sehen Sie die Seite "Hello World".

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>Hinzufügen von OWIN-Diagnose

Das Microsoft.Owin.Diagnostics-Paket enthält Middleware, die nicht behandelte Ausnahmen abfängt und eine HTML-Seite mit Fehlerdetails angezeigt. Diese Seite funktioniert ähnlich wie die ASP.NET-Seite für Fehler, die manchmal aufgerufen wird, wird die "[gelben Bildschirm fehl](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)" (YSOD). Wie die YSOD die Katana-Fehlerseite wird während der Entwicklung nützlich, aber es wird empfohlen, ihn in den Produktionsmodus deaktivieren.

Geben Sie den folgenden Befehl im Fenster Paket-Manager-Konsole, um die Diagnose-Paket in Ihrem Projekt zu installieren:

`install-package Microsoft.Owin.Diagnostics –Pre`

Ändern Sie den Code in Ihre `Startup1.Configuration` -Methode wie folgt:

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

Jetzt verwenden Sie STRG + F5, um die Anwendung ohne Debuggen auszuführen, damit Visual Studio nicht auf die Ausnahme unterbrochen wird. Die Anwendung verhält sich wie zuvor, bis Sie zum Navigieren `http://localhost/fail`, wodurch die Anwendung die Ausnahme auslöst. Die fehlerseitenmiddleware wird die Ausnahme abzufangen und eine HTML-Seite mit Informationen zum Fehler anzuzeigen. Sie können die Registerkarten, um die Stapel, Abfragezeichenfolgen, Cookies, Anforderungsheader und OWIN-Umgebungsvariablen finden Sie unter klicken.

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>Nächste Schritte

- [Erkennung der OWIN-Startup-Klasse](owin-startup-class-detection.md)
- [Verwenden von OWIN zum Selfhosten von ASP.NET-Web-API](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [Verwenden von OWIN zum Selfhosten von SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
