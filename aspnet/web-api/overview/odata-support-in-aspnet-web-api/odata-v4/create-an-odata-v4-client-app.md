---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Erstellen Sie eine OData v4-Client-App (c#) | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: 4d62a64006e2a500e0379419dbebe7ddff16fba5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827399"
---
<a name="create-an-odata-v4-client-app-c"></a>Erstellen Sie eine OData v4-Client-App (c#)
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Im vorherigen Tutorial haben Sie einen grundlegenden OData-Dienst, der CRUD-Vorgänge unterstützt erstellt. Jetzt erstellen wir einen Client für den Dienst.

Starten Sie eine neue Instanz von Visual Studio und erstellen Sie ein neues Konsolenanwendungsprojekt. In der **neues Projekt** wählen Sie im Dialogfeld **installiert** &gt; **Vorlagen** &gt; **Visual C#-** &gt; **Windows Desktop**, und wählen Sie die **Konsolenanwendung** Vorlage. Nennen Sie das Projekt &quot;ProductsApp&quot;.

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> Sie können auch die Konsolen-app hinzufügen, die der gleichen Visual Studio-Projektmappe, die den OData-Dienst enthält.


## <a name="install-the-odata-client-code-generator"></a>Installieren Sie den OData-Client-Code-Generator

Von der **Tools** , wählen Sie im Menü **Erweiterungen und Updates**. Wählen Sie **Online** &gt; **Visual Studio-Katalog**. Suchen Sie in das Suchfeld nach &quot;OData Client Code Generator&quot;. Klicken Sie auf **herunterladen** VSIX-Pakete zu installieren. Sie möglicherweise aufgefordert, Visual Studio neu starten.

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a>Der OData-Dienst lokal ausführen

Führen Sie das ProductService-Projekt in Visual Studio. Standardmäßig startet Visual Studio einen Browser zum Anwendungsstamm. Beachten Sie den URI an. Sie benötigen dies im nächsten Schritt. Unterbrechen Sie die Ausführung der Anwendung nicht.

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> Wenn Sie beide Projekte in der gleichen Projektmappe einfügen, stellen Sie sicher, dass die ProductService-Projekt ohne Debuggen auszuführen. Im nächsten Schritt müssen Sie zu der Dienst ausgeführt wird, während Sie das Projekt der Konsolenanwendung ändern.


## <a name="generate-the-service-proxy"></a>Generieren des Webdienstproxys

Der-Dienstproxy ist eine .NET-Klasse, die Methoden für den Zugriff auf den OData-Dienst definiert. Der Proxy übersetzt Methodenaufrufe in HTTP-Anforderungen. Erstellen Sie die Proxy-Klasse mit einem [T4-Vorlage](https://msdn.microsoft.com/library/bb126445.aspx).

Mit der rechten Maustaste in des Projekts. Wählen Sie **hinzufügen** &gt; **neues Element**.

![](create-an-odata-v4-client-app/_static/image5.png)

In der **neues Element hinzufügen** wählen Sie im Dialogfeld **Visual c#-Elemente** &gt; **Code** &gt; **OData-Client**. Nennen Sie die Vorlage &quot;ProductClient.tt&quot;. Klicken Sie auf **hinzufügen** , und klicken Sie auf, über die sicherheitswarnung.

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

An diesem Punkt erhalten Sie einen Fehler, die Sie ignorieren können. Visual Studio automatisch die Vorlage ausgeführt, aber die Vorlage benötigt einige Konfigurationseinstellungen erste.

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

Öffnen Sie die Datei ProductClient.odata.config. In der `Parameter` -Element, fügen Sie den URI aus dem Projekt ProductService (im vorherigen Schritt). Zum Beispiel:

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

Führen Sie die Vorlage erneut aus. Klicken Sie im Projektmappen-Explorer, klicken Sie mit der rechten Maustaste auf die ProductClient.tt-Datei, und wählen **benutzerdefiniertes Tool ausführen**.

Die Vorlage erstellt eine Codedatei namens ProductClient.cs, die den Proxy definiert. Wie Sie Ihre app entwickeln, wenn Sie den OData-Endpunkt ändern, führen Sie die Vorlage erneut aus, um den Proxy zu aktualisieren.

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a>Verwenden Sie den-Dienstproxy zum Aufrufen des OData-Diensts

Öffnen Sie die Datei "Program.cs", und Ersetzen Sie den Standardcode durch Folgendes.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

Ersetzen Sie den Wert der *ServiceUri* mit dem Dienst-URI aus.

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

Wenn Sie die app ausführen, sollten sie Folgendes ausgeben:

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
