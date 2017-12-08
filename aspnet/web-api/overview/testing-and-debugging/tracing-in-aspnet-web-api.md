---
uid: web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
title: In der ASP.NET Web API 2 Tracing | Microsoft Docs
author: MikeWasson
description: Zeigt, wie in ASP.NET Web-API-Ablaufverfolgung aktivieren.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 66a837e9-600b-4b72-97a9-19804231c64a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/testing-and-debugging/tracing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f35c8a10018ce796e2d905d6ee839ff09bb380a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="tracing-in-aspnet-web-api-2"></a>In der ASP.NET Web API 2 Tracing
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Wenn Sie eine webbasierte Anwendung debuggen möchten, wird kein Ersatz für einen geeigneten Satz von Ablaufverfolgungsprotokollen. Dieses Lernprogramm zeigt, wie in ASP.NET Web-API-Ablaufverfolgung aktivieren. Sie können diese Funktion verwenden, für die Ablaufverfolgung der Wirkungsweise der Web-API-Framework, bevor und nachdem sie Ihre Controller aufruft. Sie können es auch verwenden, um Ihren eigenen Code zu verfolgen.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Visual Studio-2017](https://www.visualstudio.com/downloads/) (funktioniert auch mit Visual Studio 2015)
> - Web-API 2
> - [Microsoft.AspNet.WebApi.Tracing](http://www.nuget.org/packages/Microsoft.AspNet.WebApi.Tracing)


## <a name="enable-systemdiagnostics-tracing-in-web-api"></a>Aktivieren Sie im Web API Tracing System.Diagnostics

Erstellen Sie zunächst ein neues ASP.NET Webanwendungsprojekt. In Visual Studio aus der **Datei** klicken Sie im Menü **neu**, klicken Sie dann **Projekt**. Klicken Sie unter **Vorlagen**, **Web**Option **ASP.NET-Webanwendung**.

[![](tracing-in-aspnet-web-api/_static/image2.png)](tracing-in-aspnet-web-api/_static/image1.png)

Wählen Sie die Web-API-Projektvorlage.

[![](tracing-in-aspnet-web-api/_static/image4.png)](tracing-in-aspnet-web-api/_static/image3.png)

Aus der **Tools** klicken Sie im Menü **Bibliothekspaket-Manager**, klicken Sie dann **Paket Verwaltungskonsole**.

Geben Sie die folgenden Befehle aus, klicken Sie im Paket-Manager-Konsole.

[!code-console[Main](tracing-in-aspnet-web-api/samples/sample1.cmd)]

Mit dem erste Befehl wird das aktuellste Web API Tracing-Paket installiert. Außerdem wird die Core-Web-API-Pakete aktualisiert. Mit dem zweite Befehl werden das WebApi.WebHost-Paket auf die neueste Version aktualisiert.

> [!NOTE]
> Wenn eine bestimmte Version von Web-API ausgerichtet werden sollen, verwenden Sie Kennzeichen "- Version" bei der Installation des Pakets für die Ablaufverfolgung.


Öffnen Sie die Datei WebApiConfig.cs in der App\_Startordner. Fügen Sie folgenden Code, der **registrieren** Methode.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample2.cs?highlight=6)]

Dieser Code fügt der [systemdiagnosticstracewriter-Objekt](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.systemdiagnosticstracewriter.aspx) Klasse für die Web-API-Pipeline. Die **systemdiagnosticstracewriter-Objekt** Klasse schreibt ablaufverfolgungen [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace).

Um die ablaufverfolgungen anzuzeigen, führen Sie die Anwendung im Debugger. Wechseln Sie in den Browser zu `/api/values`.

![](tracing-in-aspnet-web-api/_static/image5.png)

Die ablaufverfolgungsanweisungen werden in das Ausgabefenster in Visual Studio geschrieben. (Aus der **Ansicht** klicken Sie im Menü **Ausgabe**).

[![](tracing-in-aspnet-web-api/_static/image7.png)](tracing-in-aspnet-web-api/_static/image6.png)

Da **systemdiagnosticstracewriter-Objekt** schreibt ablaufverfolgungen **System.Diagnostics.Trace**, können Sie zusätzliche Ablaufverfolgungslistener registrieren; z. B. zum Schreiben von ablaufverfolgungen in einer Protokolldatei. Weitere Informationen zur Ablaufverfolgung Writer finden Sie unter der [Ablaufverfolgungslistener](https://msdn.microsoft.com/en-us/library/4y5y10s7.aspx) Thema auf MSDN.

### <a name="configuring-systemdiagnosticstracewriter"></a>Konfigurieren von systemdiagnosticstracewriter-Objekt

Der folgende Code zeigt, wie den ablaufverfolgungswriter konfigurieren.

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample3.cs)]

Es gibt zwei Einstellungen, die Sie steuern können:

- IsVerbose: Wenn "false", enthält jede Ablaufverfolgung wenige Informationen. Bei "true", enthalten die ablaufverfolgungen mehr Informationen.
- MinimumLevel: Legt die minimale Ablaufverfolgungsebene fest. Ablaufverfolgungsebenen nacheinander, werden Debug-, Info, Warn, Fehler und schwerwiegend.

## <a name="adding-traces-to-your-web-api-application"></a>Hinzufügen von Ablaufverfolgungen für Ihre Web-API-Anwendung

Hinzufügen einer ablaufverfolgungswriter ermöglicht Ihnen unmittelbaren Zugriff auf die ablaufverfolgungen, die von der Web-API-Pipeline erstellt. Sie können auch den ablaufverfolgungswriter verwenden, um Ihren eigenen Code zu verfolgen:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample4.cs)]

Rufen Sie zum Abrufen des ablaufverfolgungswriter **HttpConfiguration.Services.GetTraceWriter**. Diese Methode ist von einem Controller über die **ApiController.Configuration** Eigenschaft.

Um eine Ablaufverfolgung zu schreiben, rufen Sie die **ITraceWriter.Trace** -Methode direkt, aber die [ITraceWriterExtensions](https://msdn.microsoft.com/en-us/library/system.web.http.tracing.itracewriterextensions.aspx) Klasse definiert einige Erweiterungsmethoden, die benutzerfreundlichere sind. Z. B. die **Info** oben gezeigten Methode erstellt eine Ablaufverfolgung mit Ablaufverfolgungsebene **Info**.

## <a name="web-api-tracing-infrastructure"></a>Web-API-Infrastruktur für Ereignisablaufverfolgung

Dieser Abschnitt beschreibt, wie eine benutzerdefinierte ablaufverfolgungswriter für die Web-API schreiben.

Eine allgemeine Infrastruktur für ereignisablaufverfolgung in Web-API baut auf das Microsoft.AspNet.WebApi.Tracing-Paket. Anstatt Microsoft.AspNet.WebApi.Tracing zu verwenden, können Sie auch anschließen in eine andere Bibliothek Ablaufverfolgung/Protokollierung, z. B. [NLog](http://nlog-project.org/) oder [log4net](http://logging.apache.org/log4net/).

Zum Sammeln von ablaufverfolgungen implementieren die **ITraceWriter** Schnittstelle. Hier ist ein einfaches Beispiel:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample5.cs)]

Die **ITraceWriter.Trace** Methode erstellt eine Ablaufverfolgung. Der Aufrufer gibt eine Kategorie und Trace-Ebene. Die Kategorie kann es sich um eine beliebige benutzerdefinierte Zeichenfolge sein. Ihre Implementierung von **Trace** Folgendes tun:

1. Erstellen Sie ein neues **TraceRecord**. Initialisieren Sie sie mit der Anforderung, Kategorie und der Ablaufverfolgungsebene, wie dargestellt. Diese Werte werden vom Aufrufer bereitgestellt.
2. Aufrufen der *TraceAction* delegieren. Der Aufrufer muss innerhalb dieser Delegat füllen Sie das restliche der **TraceRecord**.
3. Schreiben der **TraceRecord**, verwenden Protokollierung-Technik, die Ihnen gefällt. Die hier gezeigte Beispiel einfach Aufrufe **System.Diagnostics.Trace**.

## <a name="setting-the-trace-writer"></a>Festlegen den Ablaufverfolgungswriter

Zum Aktivieren der Ablaufverfolgung müssen Sie konfigurieren, Web-API zur Verwendung Ihrer **ITraceWriter** Implementierung. Hierzu Sie über die **HttpConfiguration** -Objekts, wie im folgenden Code gezeigt:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample6.cs)]

Nur ein ablaufverfolgungswriter kann aktiv sein. Standardmäßig legt Web-API eine &quot;ohne-Op&quot; Überwachungstoken, die keine Aktionen ausführt. (Die &quot;ohne-Op-&quot; Überwachungstoken vorhanden ist, sodass Ablaufverfolgungscode nicht überprüfen, ob der ablaufverfolgungswriter **null** vor dem Schreiben einer Ablaufverfolgungs.)

## <a name="how-web-api-tracing-works"></a>Wie Web API Tracing Works

Ablaufverfolgung in Web-API verwendet, die eine im Web-API verwendet einen *Fassade* Muster: Wenn Ablaufverfolgung aktiviert ist, bricht Web-API verschiedenen Bestandteile der Anforderungspipeline mit Klassen, die Trace-Aufrufe ausführen.

Zum Beispiel wenn Sie einen Controller auswählen, die Pipeline verwendet die **IHttpControllerSelector** Schnittstelle. Ohne die Ablaufverfolgung aktiviert, fügt der Pipleline eine Klasse, die implementiert **IHttpControllerSelector** jedoch Eigenschaftenaufrufe an die tatsächliche Implementierung:

![Web-API-Ablaufverfolgung verwendet das Fassade-Muster.](tracing-in-aspnet-web-api/_static/image8.png)

Dieser Entwurf bietet folgende Vorteile:

- Wenn Sie keine ablaufverfolgungswriter hinzufügen, wird der ablaufverfolgungskomponenten nicht instanziiert und haben keine Auswirkungen auf die Leistung.
- Wenn Sie z. B. Standarddienste ersetzen **IHttpControllerSelector** durch eine eigene benutzerdefinierte Implementierung Ablaufverfolgung ist nicht betroffen, da die Ablaufverfolgung durch die Wrapperobjekt erfolgt.

Sie können auch das gesamte Framework des Web-API-Ablaufverfolgung mit Ihren eigenen benutzerdefinierten Framework ersetzen, indem Sie durch das Ersetzen der standardmäßigen **ITraceManager** Dienst:

[!code-csharp[Main](tracing-in-aspnet-web-api/samples/sample7.cs)]

Implementieren **ITraceManager.Initialize** Ihre Ablaufverfolgungssystem initialisiert werden. Beachten Sie, dass dies ersetzt die *gesamte* Trace-Framework, einschließlich aller dem Rückverfolgen von Code, der in der Web-API integriert ist.
