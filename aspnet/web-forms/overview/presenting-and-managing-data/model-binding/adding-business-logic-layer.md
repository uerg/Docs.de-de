---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Hinzufügen der Geschäftslogikebene auf ein Projekt, modellbindung und Web Forms verwendet. | Microsoft-Dokumentation
author: tfitzmac
description: Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 2c54c0a635c662c8740aa7f9b6bd02cd71913e24
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827504"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Hinzufügen der Geschäftslogikebene auf ein Projekt, modellbindung und Web Forms verwendet
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung, wird die dateninteraktion mehr geradlinigere als Umgang mit Data Source-Objekte (z. B. "ObjectDataSource" oder SqlDataSource-Steuerelement). Diese Serie beginnt mit einführendes Material und verschiebt in späteren Tutorials zu erweiterten Konzepten übergegangen.
> 
> In diesem Tutorial wird gezeigt, wie Sie mit einer Geschäftslogikebene mit modellbindung. Sie legen das OnCallingDataMethods-Element, um anzugeben, dass ein anderes Objekt als die aktuelle Seite mit der die Datenmethoden aufgerufen.
> 
> Dieses Tutorial baut auf das Projekt erstellt haben, der [früheren](retrieving-data.md) Teilen der Reihe.
> 
> Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB. Der herunterladbare Code funktioniert mit Visual Studio 2012 oder Visual Studio 2013. Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Tutorial gezeigt wird.


## <a name="what-youll-build"></a>Sie lernen Folgendes

Modellbindung ermöglicht Ihnen, Ihre Interaktion Datencode in der CodeBehind-Datei für eine Webseite oder in einer separaten Business Logic-Klasse zu platzieren. Den vorherigen Tutorials haben gezeigt, wie der Code-Behind-Dateien für die Interaktion Datencode zu verwenden. Dieser Ansatz funktioniert bei kleinen Standorten, jedoch kann dies zu Wiederholung und schwierigere code, wenn eine große Site verwaltet. Sie können auch sehr schwierig sein, Code programmgesteuert zu testen, die in der CodeBehind-Dateien befindet, da keine Abstraktionsschicht vorhanden ist.

Um die Interaktion Datenzugriffscode zu zentralisieren, können Sie eine Geschäftslogikebene erstellen, die die gesamte Logik für die Interaktion mit Daten enthält. Anschließend rufen Sie die Geschäftslogikebene von Ihren Webseiten. Dieses Tutorial veranschaulicht, wie verschieben sämtlichen Code, die, den Sie in den vorherigen Tutorials in einer Geschäftslogikebene geschrieben haben, und klicken Sie dann diesen Code auf den Seiten verwenden.

In diesem Tutorial müssen Sie folgende Aktionen ausführen:

1. Verschieben Sie den Code vom Code-Behind-Dateien auf einer Geschäftslogikebene
2. Ändern Sie die datengebundene Steuerelemente zum Aufrufen von Methoden in der Geschäftslogikebene

## <a name="create-business-logic-layer"></a>Erstellen der Geschäftslogikebene

Erstellen Sie nun die Klasse, die von der Webseiten aufgerufen wird. Die Methoden in dieser Klasse ähnelt die Methoden, die Sie in den vorherigen Tutorials verwendet und enthalten die Attribute des Wert-Anbieter.

Fügen Sie zunächst einen neuen Ordner namens **BLL**.

![Ordner hinzufügen](adding-business-logic-layer/_static/image1.png)

In den BLL-Ordner, erstellen Sie eine neue Klasse namens **SchoolBL.cs**. Alle Datenvorgänge, enthält, die im Code-Behind-Dateien ursprünglich gespeichert war. Die Methoden sind nahezu identisch mit den Methoden in der CodeBehind-Datei, aber einige Änderungen enthält.

Die wichtigste Änderung zu beachten ist, dass Sie den Code aus innerhalb einer Instanz von nicht mehr ausgeführt werden **Seite** Klasse. Die Page-Klasse enthält die **TryUpdateModel** Methode und die **ModelState** Eigenschaft. Wenn dieser Code in einer Geschäftslogikebene verschoben wird, müssen Sie nicht mehr eine Instanz der Page-Klasse diese Member aufgerufen. Um dieses Problem zu umgehen, fügen Sie eine **ModelMethodContext** Parameter, um eine Methode, die TryUpdateModel oder ModelState zugreift. Sie können diesen ModelMethodContext-Parameter verwenden, rufen TryUpdateModel oder ModelState abzurufen. Sie müssen keine Änderungen vornehmen, auf der Webseite für diesen neuen Parameter zu berücksichtigen.

Ersetzen Sie den Code in SchoolBL.cs durch den folgenden Code ein.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Überarbeiten Sie die vorhandene Seiten zum Abrufen von Daten aus der Geschäftslogikebene

Zum Schluss konvertieren Sie die Seiten Students.aspx AddStudent.aspx und Courses.aspx aus der Verwendung von Abfragen in der CodeBehind-Datei mit der Geschäftslogikebene.

Klicken Sie in den Code-Behind-Dateien für Schüler/Studenten, Kurse und AddStudent löschen Sie, oder kommentieren Sie die folgenden Abfragemethoden:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- CoursesGrid\_GetData

Sie verfügen nun über keinen Code im Code-Behind-Datei, die Datenvorgänge betrifft.

Die **OnCallingDataMethods** Ereignishandler können Sie zur Angabe eines Objekts für die Methoden verwenden. Students.aspx fügen Sie einen Wert für diesen Ereignishandler hinzu und ändern Sie den Namen der Datenmethoden in den Namen der Methoden in der Business Logic-Klasse.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

Definieren Sie in der CodeBehind-Datei für Students.aspx den Ereignishandler für das CallingDataMethods-Ereignis. In diesem Ereignishandler geben Sie die Business Logic-Klasse für Datenvorgänge an.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

Stellen Sie in AddStudent.aspx ähnliche Änderungen.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

Stellen Sie in Courses.aspx ähnliche Änderungen.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Führen Sie die Anwendung, und beachten Sie, dass alle Seiten funktionieren wie zuvor hatten. Die Validierungslogik auch funktioniert ordnungsgemäß.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Tutorial strukturiert neu Sie Ihrer Anwendung zur Verwendung einer Datenzugriffsebene und den Geschäftslogikebene. Sie angegeben haben, dass die Datensteuerelemente ein Objekt verwenden, die nicht die aktuelle Seite für Datenvorgänge ist.

> [!div class="step-by-step"]
> [Vorherige](using-query-string-values-to-retrieve-data.md)
