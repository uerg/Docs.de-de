---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Erstellen Sie eine Client-App OData v4 (c#) | Microsoft Docs
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: daa39fbbb4ff17d61f71bf2a642a9c2260b353e4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="create-an-odata-v4-client-app-c"></a>Erstellen Sie eine Client-App OData v4 (c#)
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Im vorherigen Lernprogramm erstellt Sie einen grundlegenden OData-Dienst, der CRUD-Vorgänge unterstützt. Jetzt erstellen wir einen Client für den Dienst.

Starten Sie eine neue Instanz von Visual Studio und erstellen Sie ein neues Konsolenanwendungsprojekt. In der **neues Projekt** wählen Sie im Dialogfeld **installiert** &gt; **Vorlagen** &gt; **Visual C#-** &gt; **Windows Desktop**, und wählen Sie die **Konsolenanwendung** Vorlage. Nennen Sie das Projekt &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> Sie können auch die Konsolen-app auf der gleichen Visual Studio-Projektmappe hinzufügen, die den OData-Dienst enthält.


## <a name="install-the-odata-client-code-generator"></a>Installieren Sie den OData-Client-Code-Generator

Aus der **Tools** klicken Sie im Menü **Erweiterungen und Updates**. Wählen Sie **Online** &gt; **Visual Studio Gallery**. Suchen Sie in das Suchfeld &quot;OData-Client-Codegenerator&quot;. Klicken Sie auf **herunterladen** VSIX-Pakete installieren. Sie möglicherweise aufgefordert, die Visual Studio neu starten.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Die OData-Dienst lokal ausführen

Führen Sie das ProductService-Projekt in Visual Studio. Standardmäßig wird in Visual Studio das Stammverzeichnis der Anwendung in einen Browser gestartet. Beachten Sie den URI an. Dies ist im nächsten Schritt benötigen. Unterbrechen Sie die Ausführung der Anwendung nicht.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Wenn Sie beide Projekte in der gleichen Projektmappe festlegen, stellen Sie sicher, um das ProductService Projekt ohne Debuggen auszuführen. Im nächsten Schritt müssen Sie zu der Dienst ausgeführt wird, während Sie das Konsolenanwendungsprojekt ändern.


## <a name="generate-the-service-proxy"></a>Generieren des Webdienstproxys

Der Dienstproxy ist eine .NET-Klasse, die Methoden für den Zugriff auf den OData-Dienst definiert. Der Proxy übersetzt Methodenaufrufe in HTTP-Anforderungen. Erstellen Sie die Proxyklasse durch Ausführen einer [T4-Vorlage](https://msdn.microsoft.com/en-us/library/bb126445.aspx).

Mit der rechten Maustaste in des Projekts. Wählen Sie **hinzufügen** &gt; **neues Element**.

![](create-an-odata-v4-client-app/_static/image5.png)

In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Visual C#-Elemente** &gt; **Code** &gt; **OData-Client**. Nennen Sie die Vorlage &quot;ProductClient.tt&quot;. Klicken Sie auf **hinzufügen** und über die sicherheitswarnung auf.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

An diesem Punkt erhalten Sie einen Fehler, die Sie ignorieren können. Visual Studio automatisch die Vorlage ausgeführt, aber die Vorlage benötigt einige Konfigurationseinstellungen erste.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Öffnen Sie die Datei ProductClient.odata.config. In der `Parameter` Element einfügen, die im URI aus dem Projekt ProductService (vorigen Schritt). Zum Beispiel:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Führen Sie die Vorlage erneut aus. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste, klicken Sie auf die ProductClient.tt-Datei, und wählen **benutzerdefiniertes Tool ausführen**.

Die Projektvorlage erstellt eine Codedatei mit dem Namen ProductClient.cs, die den Proxy definiert. Wie Sie Ihre app entwickeln, wenn Sie den OData-Endpunkt ändern, führen Sie die Vorlage erneut aus, um den Proxy zu aktualisieren.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Verwenden Sie den Dienstproxy zum Aufrufen des OData-Diensts

Öffnen Sie die Datei "Program.cs", und Ersetzen Sie den Standardcode durch Folgendes.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Ersetzen Sie den Wert der *ServiceUri* mit dem Dienst-URI aus zuvor.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Beim Ausführen der app sollte die folgende Ausgabe:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
