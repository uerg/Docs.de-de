---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Verwenden von Abfragezeichenfolgen-Werte zum Filtern von Daten mit modellbindung und web Forms | Microsoft-Dokumentation
author: tfitzmac
description: Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: fc4ec64cf583f25eca6795f7ff9215f025170054
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829569"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Verwenden von Abfragezeichenfolgen-Werte zum Filtern von Daten mit modellbindung und Web forms
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung, wird die dateninteraktion mehr geradlinigere als Umgang mit Data Source-Objekte (z. B. "ObjectDataSource" oder SqlDataSource-Steuerelement). Diese Serie beginnt mit einführendes Material und verschiebt in späteren Tutorials zu erweiterten Konzepten übergegangen.
> 
> In diesem Tutorial veranschaulicht einen Wert in der Abfragezeichenfolge übergeben, und verwenden diesen Wert zum Abrufen von Daten über die modellbindung an.
> 
> Dieses Tutorial baut auf das Projekt erstellt haben, der [früheren](retrieving-data.md) Teilen der Reihe.
> 
> Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB. Der herunterladbare Code funktioniert mit Visual Studio 2012 oder Visual Studio 2013. Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Tutorial gezeigt wird.


## <a name="what-youll-build"></a>Sie lernen Folgendes

In diesem Tutorial müssen Sie folgende Aktionen ausführen:

1. Hinzufügen einer neuen Seite, um die registrierten Kurse für Schüler/Student anzuzeigen.
2. Abrufen der registrierten Kurse für den ausgewählten Studenten, die basierend auf einem Wert in der Abfragezeichenfolge
3. Fügen Sie einen Link mit dem Wert einer Abfragezeichenfolge aus der Rasteransicht auf die neue Seite hinzu

Die Schritte in diesem Tutorial sind recht ähnlich wie für Sie in den früheren [Tutorial](sorting-paging-and-filtering-data.md) auf der Grundlage der Benutzerauswahl in einem Dropdown-Liste angezeigten Kursteilnehmer zu filtern. In diesem Tutorial haben Sie verwendet die **Steuerelement** -Attribut in der select-Methode an, dass der Wert des Parameters aus einem Steuerelement stammt. In diesem Tutorial verwenden Sie die **QueryString** -Attribut in der select-Methode an, dass der Wert des Parameters aus der Abfragezeichenfolge stammt.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Fügen Sie neue Seite für die Anzeige eines Studenten Kurse

Fügen Sie ein neues Webformular, das die Stammwebsite verwendet, und benennen Sie die Seite **Kurse**.

In der **Courses.aspx** hinzufügen eine Rasteransicht, um die Kurse für den ausgewählten Studenten angezeigt werden soll.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Definieren Sie die select-Methode

In **Courses.aspx.cs**, fügen Sie die select-Methode mit dem Namen, die Sie, in der Rasteransicht angegeben **SelectMethod** Eigenschaft. In dieser Methode müssen Sie definieren Sie eine Abfrage zum Abrufen eines Studenten, Kurse, und geben an, dass der Parameter den Wert einer Abfragezeichenfolge mit dem gleichen Namen wie der Parameter stammen.

Zunächst müssen Sie die folgende hinzufügen **mit** Anweisungen.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Anschließend fügen Sie den folgenden Code, um Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

Das QueryString-Attribut bedeutet, dass der Parameter in dieser Methode automatisch ein Abfragezeichenfolgenwert mit dem Namen "StudentID" zugewiesen ist.

## <a name="add-hyperlink-with-query-string-value"></a>Link mit dem Wert der Abfragezeichenfolge hinzufügen

In der Rasteransicht auf Students.aspx fügen Sie ein Hyperlink-Feld, das mit Ihrer neuen Kurse-Seite verknüpft. Der Link enthält einen Abfragezeichenfolgenwert mit die Id des Studierenden.

Fügen Sie in Students.aspx das folgende Feld für die gesamten Gutschriften auf die Spalten im Datenblatt Ansicht direkt unter dem Feld hinzu.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Führen Sie die Anwendung, und beachten Sie, dass die Rasteransicht jetzt den Link für die Kurse enthält.

![Link hinzufügen](using-query-string-values-to-retrieve-data/_static/image1.png)

Wenn Sie einen der Links klicken, sehen Sie die registrierten Kurse des Studenten.

![Kurse anzeigen](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Schlussbemerkung

In diesem Tutorial haben Sie eine Verbindung mit dem Wert einer Abfragezeichenfolge hinzugefügt. Sie verwendet, Abfragezeichenfolgen-Werts für den Parameterwert in der select-Methode.

In den nächsten [Tutorial](adding-business-logic-layer.md), Sie den Code aus dem Code-Behind-Dateien in einer Geschäftslogikebene und eine Datenzugriffsschicht verschoben.

> [!div class="step-by-step"]
> [Zurück](integrating-jquery-ui.md)
> [Weiter](adding-business-logic-layer.md)
