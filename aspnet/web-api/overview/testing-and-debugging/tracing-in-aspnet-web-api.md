---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: Ablaufverfolgung in ASP.NET-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Zeigt, wie die Ablaufverfolgung in ASP.NET Web-API aktiviert.
ms.author: riande
ms.date: 02/25/2014
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 02805eda4f8dceb467547fa4e00aef8ea956f228
ms.sourcegitcommit: c684eb6c0999d11d19e15e65939e5c7f99ba47df
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/19/2018
ms.locfileid: "46292283"
---
<a name="tracing-in-aspnet-web-api-2"></a>Ablaufverfolgung in ASP.NET Web-API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Beim Debuggen eine webbasierten Anwendung, ist es keinen Ersatz für einen guten Satz von Ablaufverfolgungsprotokollen an. Dieses Tutorial zeigt, wie die Ablaufverfolgung in ASP.NET Web-API aktiviert wird. Sie können diese Funktion verwenden, für die Ablaufverfolgung, was das Web-API-Framework ermöglicht, vor und nach dem sie Ihre Controller aufruft. Sie können es auch verwenden, um Ihren eigenen Code zu verfolgen.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - [Visual Studio 2017](https://www.visualstudio.com/downloads/) (funktioniert auch mit Visual Studio 2015)
> - Web-API 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Aktivieren Sie System.Diagnostics, die in der Web-API-Ablaufverfolgung

Zunächst erstellen wir ein neues ASP.NET-Webanwendungsprojekt. In Visual Studio aus der **Datei** , wählen Sie im Menü **neu**, klicken Sie dann **Projekt**. Klicken Sie unter **Vorlagen**, **Web**Option **ASP.NET-Webanwendung**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Wählen Sie die Web-API-Projektvorlage aus.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Von der **Tools** , wählen Sie im Menü **Bibliothekspaket-Manager**, klicken Sie dann **-Paket-Manager-Konsole**.

Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle ein.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

Mit dem erste Befehl wird das neueste Paket der Web-API-Ablaufverfolgung installiert. Außerdem aktualisiert es die Core-Web-API-Pakete. Der zweite Befehl aktualisiert das WebApi.WebHost-Paket auf die neueste Version.

> [!NOTE]
> Wenn Sie eine bestimmte Version von Web-API abzielen möchten, verwenden Sie das - Version-Flag, bei der Installation des Pakets für die Ablaufverfolgung.


Öffnen Sie in der App die Datei WebApiConfig.cs\_Startordner. Fügen Sie den folgenden Code der **registrieren** Methode.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Dieser Code fügt die [systemdiagnosticstracewriter-Objekt](https://msdn.microsoft.com/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) Klasse, um die Web-API-Pipeline. Die **systemdiagnosticstracewriter-Objekt** Klasse schreibt ablaufverfolgungen ["System.Diagnostics.Trace"](https://msdn.microsoft.com/library/system.diagnostics.trace).

Um die ablaufverfolgungen anzuzeigen, führen Sie die Anwendung im Debugger aus. Navigieren Sie im Browser zu `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Die Ablaufverfolgungs-Anweisungen werden im Ausgabefenster in Visual Studio geschrieben. (Aus der **Ansicht** , wählen Sie im Menü **Ausgabe**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Da **systemdiagnosticstracewriter-Objekt** schreibt ablaufverfolgungen **"System.Diagnostics.Trace"**, können Sie zusätzliche Ablaufverfolgungslistener registrieren; z. B. zum Schreiben von ablaufverfolgungen in einer Protokolldatei. Weitere Informationen zu Ablaufverfolgung Writern finden Sie unter den [Ablaufverfolgungslistener](https://msdn.microsoft.com/library/4y5y10s7.aspx) Thema auf MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Konfigurieren von systemdiagnosticstracewriter-Objekt

Der folgende Code zeigt, wie den ablaufverfolgungswriter konfiguriert wird.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Es gibt zwei Einstellungen, die Sie steuern können:

- IsVerbose: Wenn "false", enthält jede Ablaufverfolgung wenige Informationen. Bei "true", enthält die ablaufverfolgungen mehr Informationen.
- MinimumLevel: Legt die minimale Ablaufverfolgungsebene fest. Ablaufverfolgungsebenen, in der Reihenfolge, werden Debug-, Info, Warnung, Fehler und schwerwiegend.

## <a name="adding-traces-to-your-web-api-application"></a>Hinzufügen von Ablaufverfolgungen zu Ihrer Web-API-Anwendung

Hinzufügen eines Ablaufverfolgungs-Writers bietet Ihnen unmittelbaren Zugriff auf die ablaufverfolgungen, die von der Web-API-Pipeline erstellt. Sie können auch den ablaufverfolgungswriter verwenden, um Ihren eigenen Code zu verfolgen:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Rufen Sie zum Abrufen des ablaufverfolgungswriter **HttpConfiguration.Services.GetTraceWriter**. Diese Methode ist von einem Controller, über die **ApiController.Configuration** Eigenschaft.

Um eine Ablaufverfolgung zu schreiben, können Sie rufen die **ITraceWriter.Trace** -Methode direkt, aber die [ITraceWriterExtensions](https://msdn.microsoft.com/library/system.web.http.tracing.itracewriterextensions.aspx) -Klasse definiert einige Erweiterungsmethoden, die einfacher sind. Z. B. die **Informationen** oben gezeigten Methode erstellt eine Ablaufverfolgung mit Ablaufverfolgungsebene **Informationen**.

## <a name="web-api-tracing-infrastructure"></a>Web-API-Infrastruktur für Ereignisablaufverfolgung

Dieser Abschnitt beschreibt, wie Sie einen benutzerdefinierten Ablaufverfolgungs-Writer für Web-API schreiben.

Das Paket Microsoft.AspNet.WebApi.Tracing basiert auf einer allgemeineren Ablaufverfolgungsinfrastruktur in Web-API. Anstatt Microsoft.AspNet.WebApi.Tracing, Sie können auch einbinden in eine andere Ablaufverfolgung/Protokollierung-Bibliothek, z. B. [NLog](http://nlog-project.org/) oder [log4net](http://logging.apache.org/log4net/).

Um ablaufverfolgungen zu erfassen, implementieren die **ITraceWriter** Schnittstelle. Hier ist ein einfaches Beispiel:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

Die **ITraceWriter.Trace** Methode erstellt eine Ablaufverfolgung. Der Aufrufer gibt es sich um eine Kategorie und Trace-Ebene. Die Kategorie kann eine beliebige benutzerdefinierte Zeichenfolge sein. Die Implementierung von **Ablaufverfolgung** sollten gehen Sie folgendermaßen vor:

1. Erstellen Sie ein neues **TraceRecord**. Initialisieren Sie es mit der Anforderung, Kategorie und -Ablaufverfolgungsebene, wie gezeigt. Diese Werte werden vom Aufrufer bereitgestellt.
2. Rufen Sie die *TraceAction* delegieren. Innerhalb dieses Delegaten ein: der Aufrufer soll, geben Sie im weiteren Verlauf der **TraceRecord**.
3. Schreiben der **TraceRecord**, über jede Protokollierung-Technik, die Ihnen gefällt. Die hier gezeigte Beispiel ruft einfach in **"System.Diagnostics.Trace"**.

## <a name="setting-the-trace-writer"></a>Festlegen den Ablaufverfolgungswriter

Zum Aktivieren der Ablaufverfolgung müssen Sie die Web-API verwenden, konfigurieren Ihre **ITraceWriter** Implementierung. Sie ist dies über die **HttpConfiguration** Objekt, wie im folgenden Code gezeigt:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Nur eine Ablaufverfolgungs-Writer kann aktiv sein. Web-API legt standardmäßig eine &quot;ohne-Op-&quot; Überwachungstoken, die keine Aktionen ausführt. (Die &quot;ohne-Op-&quot; Überwachung vorhanden, sodass Rückverfolgen von Code nicht überprüfen unbedingt, ob der ablaufverfolgungswriter **null** vor dem Schreiben einer Ablaufverfolgungs.)

## <a name="how-web-api-tracing-works"></a>Wie Web-API-Ablaufverfolgung funktioniert

Die Ablaufverfolgung in Web-API verwendet einen *Fassade* Muster: Wenn die Ablaufverfolgung aktiviert ist, Web-API dient als Wrapper für verschiedene Teile der Pipeline mit Klassen, die Trace-Aufrufe ausführen.

Z. B. Wenn Sie einen Controller auswählen, die die Pipeline verwendet die **IHttpControllerSelector** Schnittstelle. Mit aktivierter Ablaufverfolgung, fügt die Pipleline eine Klasse, die implementiert **IHttpControllerSelector** aber Eigenschaftenaufrufe an die tatsächliche Implementierung:

![Web-API-Ablaufverfolgung verwendet die Fassade-Muster.](tracing-in-aspnet-web-api/_static/image8.png)

Die von dieser Entwurf bietet folgende Vorteile:

- Wenn Sie einen Ablaufverfolgungs-Writer nicht hinzufügen, werden die ablaufverfolgungskomponenten nicht instanziiert und haben keine Auswirkungen auf die Leistung.
- Wenn Sie z. B. Standarddienste ersetzen **IHttpControllerSelector** durch Ihre eigene benutzerdefinierte Implementierung, Ablaufverfolgung ist nicht betroffen, da die Ablaufverfolgung durch das Wrapperobjekt erfolgt.

Sie können auch das gesamte Web-API-Ablaufverfolgung-Framework mit Ihren eigenen benutzerdefinierten Framework verwenden, ersetzen, von den standardmäßigen ersetzen **ITraceManager** Dienst:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implementieren **ITraceManager.Initialize** zum Initialisieren von Ihrem Ablaufverfolgungssystem. Beachten Sie, dass dies ersetzt die *gesamte* Ablaufverfolgung Frameworks einschließlich sämtlicher den Ablaufverfolgungscode, die in Web-API integriert ist.
