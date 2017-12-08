---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: "Komponententests für SignalR-Anwendungen | Microsoft Docs"
author: pfletcher
description: Dieser Artikel beschreibt, wie die Komponententest-Features von SignalR 2.0.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: e55efd644dd4b6fb57061ffb89a5c041136c7b5e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="unit-testing-signalr-applications"></a>Einheit Testen von SignalR-Anwendungen
====================
durch [Patrick Fletcher](https://github.com/pfletcher)

> Dieser Artikel beschreibt die Verwendung der Funktionen UnitTests von SignalR-2. 
> 
> ## <a name="software-versions-used-in-this-topic"></a>In diesem Thema verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR Version 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können. Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET SignalR-Forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) oder [StackOverflow.com](http://stackoverflow.com/).


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>Komponententests für SignalR-Anwendungen

Die Einheit Test-Funktionen können in SignalR 2 Sie um Komponententests für die SignalR-Anwendung zu erstellen. SignalR 2 umfasst die [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) -Schnittstelle, die zum Erstellen einer simulierten-Objekt, um Ihre hubmethoden zu Testzwecken simulieren verwendet werden kann.

In diesem Abschnitt fügen Sie Komponententests für die Anwendung in der [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) mit [XUnit.net](https://github.com/xunit/xunit) und [Moq](https://github.com/Moq/moq4).

XUnit.net wird verwendet werden, um den Test zu steuern. Moq verwendet werden, erstellen Sie eine [modellieren](http://en.wikipedia.org/wiki/Mock_object) Objekt zum Testen. Andere Mockframeworks können verwendet werden, falls gewünscht. [NSubstitute](http://nsubstitute.github.io/) ist auch eine gute Wahl. Dieses Lernprogramm veranschaulicht, wie die simulierte Objekt auf zwei Arten eingerichtet: zunächst mithilfe einer `dynamic` Objekt (eingeführt in .NET Framework 4) und der Sekunde, mit einer Schnittstelle.

### <a name="contents"></a>Inhalt

Dieses Lernprogramm enthält die folgenden Abschnitte.

- [Komponententests mit dynamischen](#dynamic)
- [Komponententests, die nach Typ](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>Komponententests mit dynamischen

In diesem Abschnitt fügen Sie einen Komponententest für die Anwendung in der [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) ein dynamisches Objekt verwenden.

1. Installieren der [XUnit Runner Erweiterung](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099) für Visual Studio 2013.
2. Entweder ist zum Abschließen der [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md), oder Herunterladen die fertigen Anwendung von [MSDN Code Gallery](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9).
3. Öffnen Sie bei Verwendung der Downloadversion der Anwendung Einstieg **Package Manager Console** , und klicken Sie auf **wiederherstellen** des SignalR-Pakets zum Projekt hinzufügen.

    ![Stellen Sie die Pakete wieder her.](unit-testing-signalr-applications/_static/image1.png)
4. Fügen Sie ein Projekt der Projektmappe für den Komponententest aus. Mit der rechten Maustaste in der Projektmappe in **Projektmappen-Explorer** , und wählen Sie **hinzufügen**, **neues Projekt...** . Klicken Sie unter der **c#** Knoten, wählen die **Windows** Knoten. Wählen Sie **-Klassenbibliothek**. Nennen Sie das neue Projekt **TestLibrary** , und klicken Sie auf **OK**.

    ![Erstellen Sie die Bibliothek für das Testen](unit-testing-signalr-applications/_static/image2.png)
5. Fügen Sie einen Verweis auf das Projekt SignalRChat im Test-Klassenbibliotheksprojekt. Mit der rechten Maustaste die **TestLibrary** Projekt, und wählen Sie **hinzufügen**, **Verweis...** . Wählen Sie die **Projekte** unter Knoten die **Lösung** Knoten, und aktivieren Sie **SignalRChat**. Klicken Sie auf **OK**.

    ![Fügen Sie Projektverweis hinzu](unit-testing-signalr-applications/_static/image3.png)
6. Hinzufügen der Pakete SignalR Moq und XUnit, um die **TestLibrary** Projekt. In der **Package Manager Console**legen die **Projekt standardmäßig** Dropdownliste zu **TestLibrary**. Führen Sie die folgenden Befehle im Konsolenfenster angezeigt:

    - `Install-Package Microsoft.AspNet.SignalR`
    - `Install-Package Moq`
    - `Install-Package XUnit`

    ![Installieren von Paketen](unit-testing-signalr-applications/_static/image4.png)
7. Erstellen Sie die Testdatei. Mit der rechten Maustaste die **TestLibrary** Projekt, und klicken Sie auf **hinzufügen...** , **Klasse**. Benennen Sie die neue Klasse **Tests.cs**.
8. Ersetzen Sie den Inhalt der Tests.cs durch den folgenden Code ein.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    Im obigen Code wird ein Testclient erstellt, mit der `Mock` -Objekt aus der [Moq](https://github.com/Moq/moq4) Bibliothek des Typs [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (weisen Sie in SignalR-2.1 `dynamic` für den Typ Parameter.) Die `IHubCallerConnectionContext` Schnittstelle ist das Proxyobjekt, mit denen Sie die Methoden auf dem Client aufrufen. Die `broadcastMessage` Funktion klicken Sie dann für den simulierten Client definiert, dass sie aufgerufen werden kann, indem Sie die `ChatHub` Klasse. Das Modul ruft dann die `Send` Methode der `ChatHub` -Klasse, die ihrerseits die mocked `broadcastMessage` Funktion.
9. Erstellen Sie die Projektmappe durch Drücken von **F6**.
10. Führen Sie den Komponententest aus. Wählen Sie in Visual Studio **Test**, **Windows**, **Test-Explorer**. Im Test-Explorer-Fenster mit der Maustaste **HubsAreMockableViaDynamic** , und wählen Sie **ausgewählte Tests ausführen**.

    ![Test-Explorer](unit-testing-signalr-applications/_static/image5.png)
11. Stellen Sie sicher, dass der Test erfolgreich war, überprüfen Sie den unteren Bereich im Test-Explorer-Fenster. Das Fenster zeigt, dass der Test erfolgreich war.

    ![Bestandene Tests](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>Komponententests, die nach Typ

In diesem Abschnitt fügen Sie einen Test für die Anwendung erstellt, der [Getting Started Tutorial](../getting-started/tutorial-getting-started-with-signalr.md) über eine Schnittstelle, die die Methode, die zu testende enthält.

1. Führen Sie die Schritte 1 bis 7 in die [Komponententests mit dynamischen](#dynamic) Lernprogramm oben.
2. Ersetzen Sie den Inhalt der Tests.cs durch den folgenden Code ein.

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    Im obigen Code wird eine Schnittstelle erstellt, die Signatur der Definition der `broadcastMessage` Methode für die vom Testmodul ein simulierten Clients erstellen. Ein simulierten Client wird dann mit erstellt die `Mock` Objekt des Typs [IHubCallerConnectionContext](https://msdn.microsoft.com/en-us/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (weisen Sie in SignalR-2.1 `dynamic` für den Typparameter.) Die `IHubCallerConnectionContext` Schnittstelle ist das Proxyobjekt, mit denen Sie die Methoden auf dem Client aufrufen.

    Der Test erstellt dann eine Instanz des `ChatHub`, und erstellt dann auf eine Pseudoversion des der `broadcastMessage` -Methode, die wiederum, durch Aufrufen aufgerufen wird der `Send` Methode auf dem Hub.
3. Erstellen Sie die Projektmappe durch Drücken von **F6**.
4. Führen Sie den Komponententest aus. Wählen Sie in Visual Studio **Test**, **Windows**, **Test-Explorer**. Im Test-Explorer-Fenster mit der Maustaste **HubsAreMockableViaDynamic** , und wählen Sie **ausgewählte Tests ausführen**.

    ![Test-Explorer](unit-testing-signalr-applications/_static/image7.png)
5. Stellen Sie sicher, dass der Test erfolgreich war, überprüfen Sie den unteren Bereich im Test-Explorer-Fenster. Das Fenster zeigt, dass der Test erfolgreich war.

    ![Bestandene Tests](unit-testing-signalr-applications/_static/image8.png)
