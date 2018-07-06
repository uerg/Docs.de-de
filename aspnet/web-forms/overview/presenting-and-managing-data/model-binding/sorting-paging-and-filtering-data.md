---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Sortieren, paging und Filtern von Daten mit modellbindung und Web Forms | Microsoft-Dokumentation
author: tfitzmac
description: Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: ded398bcbbb8d7d9e5c882a598bf9d6faa6ea81e
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833875"
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Sortieren, paging und Filtern von Daten mit modellbindung und Web forms
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung, wird die dateninteraktion mehr geradlinigere als Umgang mit Data Source-Objekte (z. B. "ObjectDataSource" oder SqlDataSource-Steuerelement). Diese Serie beginnt mit einführendes Material und verschiebt in späteren Tutorials zu erweiterten Konzepten übergegangen.
> 
> In diesem Tutorial veranschaulicht das Hinzufügen, sortieren, paging und Filterung der Daten über die modellbindung.
> 
> Dieses Tutorial baut auf das Projekt erstellt haben, in der ersten [Teil](retrieving-data.md) der Reihe.
> 
> Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB. Der herunterladbare Code funktioniert mit Visual Studio 2012 oder Visual Studio 2013. Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Tutorial gezeigt wird.


## <a name="what-youll-build"></a>Sie lernen Folgendes

In diesem Tutorial müssen Sie folgende Aktionen ausführen:

1. Aktivieren Sie sortieren und paging von Daten
2. Aktivieren Sie die Filterung der Daten basierend auf der Auswahl des Benutzers

## <a name="add-sorting"></a>Hinzufügen einer Sortierung

Aktivieren der Sortierung in der GridView ist sehr einfach. Legen Sie einfach in der Datei Student.aspx **AllowSorting** zu **"true"** in den GridView-Ansicht. Sie müssen nicht festlegen einer **SortExpression** Wert für jede Spalte, wie die DataField automatisch verwendet wird. Das GridView ändert es sich um die Abfrage enthält Anordnen der Daten durch die ausgewählten Werte. Der folgende hervorgehobene Code zeigt dem Hinzufügen, die Sie benötigen zu sortieren.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Führen Sie die Webanwendung, und Testen von sortieren Studentendatensätze von den Werten in verschiedenen Spalten.

![Sortierung Schüler/Studenten](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Hinzufügen von paging

Aktivieren von Paging ist ebenfalls ganz einfach. Legen Sie in den GridView-Ansicht, die **AllowPaging** Eigenschaft **"true"** und legen Sie die **PageSize** Eigenschaft, um die Anzahl der Datensätze, die Sie auf jeder Seite anzeigen möchten. In diesem Tutorial können Sie es auf 4 festlegen.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Führen Sie die Webanwendung, und beachten Sie, dass die Datensätze mit mehr als 4 auf einer einzelnen Seite angezeigten Datensätze über mehrere Seiten unterteilt sind.

![Hinzufügen von paging](sorting-paging-and-filtering-data/_static/image4.png)

Verzögerte abfrageausführung verbessert die ANWENDUGNSEFFIZIENZ. Anstatt durch Abrufen des gesamten Datasets, ändert die GridView für die Abfrage, um nur die Datensätze für die aktuelle Seite abrufen.

## <a name="filter-records-by-user-selection"></a>Filtern von Datensätzen durch Auswahl des Benutzers

Modellbindung fügt mehrere Attribute, die Sie festlegen, wie Sie den Wert für einen Parameter in einem Modell Bindungsmethode festlegen können. Diese Attribute sind der **System.Web.ModelBinding** Namespace. Dazu zählen:

- Steuerelement
- Cookie
- Formular
- Profile
- QueryString
- RouteData
- Sitzung
- UserProfile
- ViewState

In diesem Tutorial verwenden Sie den Wert eines Steuerelements um zu filtern, welche Datensätze in den GridView-Ansicht angezeigt werden. Fügen Sie der **Steuerelement** -Attribut auf die Query-Methode, die Sie zuvor erstellt haben. In einem [später](using-query-string-values-to-retrieve-data.md) Tutorial übernehmen Sie die **QueryString** -Attribut auf einen Parameter, um anzugeben, dass der Wert des Parameters den Wert einer Abfragezeichenfolge stammen.

Fügen Sie zunächst über die ValidationSummary eine Dropdownliste zum Filtern der Schüler/Studenten angezeigt werden.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

Klicken Sie in der CodeBehind-Datei ändern Sie die select-Methode Empfang ein Werts aus dem Steuerelement, und legen Sie den Namen des Parameters auf den Namen des Steuerelements, das den Wert bereitstellt.

Müssen Sie hinzufügen, eine **mit** -Anweisung für die **System.Web.ModelBinding** das Steuerelementattribut aufzulösende Namespace.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

Der folgende Code zeigt die select-Methode erneut gearbeitet, um die zurückgegebenen Daten basierend auf dem Wert des Dropdown-Liste zu filtern. Ein Control-Attribut hinzufügen, bevor ein Parameter gibt an, dass der Wert für diesen Parameter aus einem Steuerelement mit dem gleichen Namen stammt.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Führen Sie die Webanwendung aus, und wählen Sie unterschiedliche Werte aus der Dropdown-Liste aus, um die Liste der Studenten zu filtern.

![Filter Schüler/Studenten](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Schlussbemerkung

In diesem Tutorial haben aktiviert Sie sortieren und paging von Daten. Sie wird aktiviert, die Daten durch den Wert eines Steuerelements zu filtern.

In den nächsten [Tutorial](integrating-jquery-ui.md) werden Sie die Benutzeroberfläche durch die Integration eines JQuery UI-Widgets in der Vorlage für die dynamische Daten verbessern.

> [!div class="step-by-step"]
> [Zurück](updating-deleting-and-creating-data.md)
> [Weiter](integrating-jquery-ui.md)
