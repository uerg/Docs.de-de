---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Hinzufügen von Geschäftslogikschicht auf ein Projekt, wurden die modellbindung und WebForms verwendet. | Microsoft Docs
author: tfitzmac
description: Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: 25e887bdc316abf65c780bb6c8d075e938e85064
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892750"
---
<a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a>Hinzufügen von Geschäftslogikschicht auf ein Projekt, wurden die modellbindung und WebForms verwendet
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion mehr die selbsterklärend als Umgang mit Data Source-Objekte (z. B. ObjectDataSource oder SqlDataSource). Diese Reihe beginnt mit einführendes Material und verschiebt die erweiterte Konzepte in späteren Lernprogrammen.
> 
> Dieses Lernprogramm zeigt, wie mit eine Geschäftslogikschicht wurden die modellbindung verwenden. Sie legen das OnCallingDataMethods-Element, um anzugeben, dass ein anderes Objekt als die aktuelle Seite verwendet wird, die Methoden aufrufen.
> 
> Dieses Lernprogramm baut auf das Projekt erstellt, der [früheren](retrieving-data.md) Teile der Reihe.
> 
> Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB dar. Der Code zum Herunterladen funktioniert mit Visual Studio 2012 oder Visual Studio 2013. Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Lernprogramm gezeigt wird.


## <a name="what-youll-build"></a>Was müssen Sie erstellen

Wurden die modellbindung können Sie Ihre Daten Interaktion-Code in der CodeBehind-Datei für eine Webseite oder in einer separaten Business Logic-Klasse platziert. In den vorherigen Lernprogrammen wurden zum Verwenden von Code-Behind-Dateien für die Interaktion Datencode gezeigt. Dieser Ansatz funktioniert bei kleinen Standorten kann es jedoch führen, um Wiederholung und erschwerte zu codieren, wenn eine große Website verwalten. Es kann auch sehr schwierig sein, Code programmgesteuert zu testen, die in CodeBehind-Dateien gespeichert ist, da keine Abstraktionsschicht vorhanden ist.

Um die Interaktion Datencode zu zentralisieren, können Sie eine Geschäftslogikschicht erstellen, die alle von der Logik für die Interaktion mit Daten enthält. Dann rufen Sie die Geschäftslogikschicht von Webseiten. Dieses Lernprogramm zeigt, wie verschieben Sie alle des Codes, den Sie in den vorherigen Lernprogrammen in eine Geschäftslogikschicht geschrieben haben, und verwenden Sie diesen Code auf den Seiten.

In diesem Lernprogramm lernen Sie folgende Aktionen ausführen:

1. Verschieben Sie den Code vom Code-Behind-Dateien, eine Geschäftslogikschicht
2. Ändern Sie die datengebundene Steuerelemente zum Aufrufen von Methoden in der Geschäftslogikebene

## <a name="create-business-logic-layer"></a>Erstellen Sie die Logik der Geschäftsebene

Nun erstellen Sie die Klasse, die die Webseiten aufgerufen wird. Die Methoden in dieser Klasse ähnelt die Methoden, die Sie in den vorherigen Lernprogrammen verwendet und der Anbieter Wertattribute enthalten.

Fügen Sie zunächst einen neuen Ordner namens **BLL**.

![Ordner hinzufügen](adding-business-logic-layer/_static/image1.png)

Erstellen Sie eine neue Klasse mit dem Namen im Ordner "BLL" **SchoolBL.cs**. Es wird alle Datenvorgänge enthalten, die im Code-Behind-Dateien ursprünglich gespeichert war. Die Methoden sind nahezu identisch mit den Methoden in der Code-Behind-Datei, aber einige Änderungen enthält.

Die wichtigste Änderung zu beachten ist, dass Sie den Code von innerhalb einer Instanz von nicht mehr ausgeführt werden **Seite** Klasse. Die Page-Klasse enthält die **TryUpdateModel** Methode und die **ModelState** Eigenschaft. Wenn dieser Code in eine Geschäftslogikschicht verschoben wird, müssen Sie nicht mehr eine Instanz der Page-Klasse, um diese Member aufzurufen. Um dieses Problem zu umgehen, fügen Sie eine **ModelMethodContext** Parameter für jede Methode, die TryUpdateModel oder ModelState zugreift. Zum Aufrufen von TryUpdateModel ModelState abzurufen, verwenden Sie diesen ModelMethodContext-Parameter. Sie müssen nicht ändert nichts in die Webseite für diesen neuen Parameter zu berücksichtigen.

Ersetzen Sie den Code in SchoolBL.cs durch den folgenden Code ein.

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a>Überarbeiten Sie die vorhandene Seiten zum Abrufen von Daten aus der Geschäftslogikebene

Zum Schluss konvertieren Sie Students.aspx, AddStudent.aspx und Courses.aspx Seiten aus der Verwendung von Abfragen in der Code-Behind-Datei mit der Geschäftslogikebene.

Klicken Sie in den Code-Behind-Dateien für Studenten, Bildungseinrichtungen AddStudent und Kurse löschen Sie, oder kommentieren Sie die folgenden Abfragemethoden:

- studentsGrid\_GetData
- studentsGrid\_UpdateItem
- studentsGrid\_DeleteItem
- addStudentForm\_InsertItem
- coursesGrid\_GetData

Sie verfügen jetzt über keinen Code in der CodeBehind-Datei, die zum Datenvorgänge gehört.

Die **OnCallingDataMethods** Ereignishandler ermöglicht es Ihnen, geben Sie ein Objekt, das für die Data-Methoden verwendet. Fügen Sie in Students.aspx einen Wert für diesen Ereignishandler hinzu, und ändern Sie die Namen der Datenmethoden den Namen der Methoden in der Business Logic-Klasse.

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

Definieren Sie in der CodeBehind-Datei für Students.aspx den Ereignishandler für das CallingDataMethods-Ereignis. In diesem Ereignishandler geben Sie die Business Logic-Klasse für Datenvorgänge.

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

Stellen Sie im AddStudent.aspx ähnliche Änderungen.

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

Stellen Sie im Courses.aspx ähnliche Änderungen.

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

Führen Sie die Anwendung, und beachten Sie, dass alle Seiten funktionieren wie zuvor mussten. Die Validierungslogik auch funktioniert ordnungsgemäß.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Lernprogramm strukturiert neu Sie die Anwendung eine Datenzugriffsschicht und Geschäftslogikschicht verwendet. Sie angegeben, dass die Data-Steuerelemente ein Objekts verwenden, das nicht die aktuelle Seite Datenvorgänge ist.

> [!div class="step-by-step"]
> [Vorherige](using-query-string-values-to-retrieve-data.md)
