---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Verwenden von Abfragezeichenfolgen-Werte zum Filtern von Daten mit modellbindung und web Forms | Microsoft Docs
author: tfitzmac
description: Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 03d20decf0eeff6062fbc6f8dd66f644b405c7cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30886822"
---
<a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Verwenden von Abfragezeichenfolgen-Werte zum Filtern von Daten mit modellbindung und WebForms
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion mehr die selbsterklärend als Umgang mit Data Source-Objekte (z. B. ObjectDataSource oder SqlDataSource). Diese Reihe beginnt mit einführendes Material und verschiebt die erweiterte Konzepte in späteren Lernprogrammen.
> 
> Dieses Lernprogramm veranschaulicht, übergeben Sie den Wert in der Abfragezeichenfolge und diesen Wert zum Abrufen von Daten über die modellbindung verwenden.
> 
> Dieses Lernprogramm baut auf das Projekt erstellt, der [früheren](retrieving-data.md) Teile der Reihe.
> 
> Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB dar. Der Code zum Herunterladen funktioniert mit Visual Studio 2012 oder Visual Studio 2013. Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Lernprogramm gezeigt wird.


## <a name="what-youll-build"></a>Was müssen Sie erstellen

In diesem Lernprogramm lernen Sie folgende Aktionen ausführen:

1. Fügen Sie eine neue Seite um ein Student die registrierten Kurse anzeigen
2. Abrufen der registrierten Kurse für die ausgewählten Studenten, die anhand eines Werts in der Abfragezeichenfolge
3. Fügen Sie einen Link mit einem Zeichenfolgenwert für die Abfrage von der Rasteransicht, auf die neue Seite

Die Schritte in diesem Lernprogramm sind ähnlich, was Sie getan haben in den früheren [Lernprogramm](sorting-paging-and-filtering-data.md) zum Filtern der angezeigten Students, die basierend auf der Auswahl des Benutzers in einer Dropdownliste aus. In diesem Lernprogramm verwendet Sie die **Steuerelement** -Attribut in der select-Methode an, dass der Wert des Parameters aus einem Steuerelement stammt. In diesem Lernprogramm verwenden Sie die **QueryString** -Attribut in der select-Methode an, dass der Wert des Parameters aus der Abfragezeichenfolge stammt.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Fügen Sie neue Seite für ein Student Kurse anzeigen

Fügen Sie eine neue WebForm, die die Stammwebsite verwendet, und nennen Sie die Seite **Kurse**.

In der **Courses.aspx** Datei, fügen Sie eine Rasteransicht der Kurse für die ausgewählten Studenten anzeigen.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Definieren Sie die select-Methode

In **Courses.aspx.cs**, fügen Sie die select-Methode mit dem Namen in der Rasteransicht angegebenen **SelectMethod** Eigenschaft. In dieser Methode können Sie die Abfrage zum Abrufen einer Student Kurse zu definieren, und geben an, dass der Parameter einen Wert mit dem gleichen Namen wie der Parameter der Abfragezeichenfolge stammen.

Sie müssen zuerst, fügen Sie die folgenden **mit** Anweisungen.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Anschließend fügen Sie den folgenden Code, um Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

Das QueryString-Attribut bedeutet, dass ein mit dem Namen StudentID Wert der Abfragezeichenfolge an den Parameter in dieser Methode automatisch zugewiesen wird.

## <a name="add-hyperlink-with-query-string-value"></a>Mit dem Wert der Abfragezeichenfolge Link hinzufügen

In der Rasteransicht auf Students.aspx fügen Sie ein Hyperlinkfeld, das mit Ihrer neuen Kurse Seite verknüpft. Der Hyperlink wird einen Wert der Abfragezeichenfolge mit den Studenten-Id enthalten.

Fügen Sie in Students.aspx das folgende Feld für das Gesamtguthaben für das Raster Sichtspalten direkt unter dem Feld hinzu.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Führen Sie die Anwendung, und beachten Sie, dass die Rasteransicht jetzt den Link Kurse enthält.

![Link hinzufügen](using-query-string-values-to-retrieve-data/_static/image1.png)

Wenn Sie einen der Links klicken, sehen Sie diese Student registrierten Kurse.

![Kurse anzeigen](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Schlussbemerkung

In diesem Lernprogramm haben Sie eine Verknüpfung mit einem Abfragezeichenfolgenwert hinzugefügt. Dieser Wert der Abfragezeichenfolge wird verwendet, für den Wert des Parameters in der select-Methode.

In der nächsten [Lernprogramm](adding-business-logic-layer.md), Sie den Code aus dem Code-Behind-Dateien in eine Geschäftslogikschicht und eine Datenzugriffsschicht verschoben.

> [!div class="step-by-step"]
> [Zurück](integrating-jquery-ui.md)
> [Weiter](adding-business-logic-layer.md)
