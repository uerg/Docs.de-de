---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Sortieren, paging und Filtern von Daten mit modellbindung und WebForms | Microsoft Docs
author: tfitzmac
description: Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: 94fc84533be5fcbcf0612fcdcabea7dee738d89b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Sortieren, paging und Filtern von Daten mit modellbindung und WebForms
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion mehr die selbsterklärend als Umgang mit Data Source-Objekte (z. B. ObjectDataSource oder SqlDataSource). Diese Reihe beginnt mit einführendes Material und verschiebt die erweiterte Konzepte in späteren Lernprogrammen.
> 
> Dieses Lernprogramm zeigt, wie sortieren, paging und Filtern der Daten wurden die modellbindung hinzufügen.
> 
> Dieses Lernprogramm baut auf das Projekt erstellt, in der ersten [Teil](retrieving-data.md) der Reihe.
> 
> Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB dar. Der Code zum Herunterladen funktioniert mit Visual Studio 2012 oder Visual Studio 2013. Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Lernprogramm gezeigt wird.


## <a name="what-youll-build"></a>Was müssen Sie erstellen

In diesem Lernprogramm lernen Sie folgende Aktionen ausführen:

1. Aktivieren der Sortierung und paging von Daten
2. Aktivieren Sie Filtern der Daten basierend auf der Auswahl des Benutzers

## <a name="add-sorting"></a>Sortierung hinzufügen

Aktivieren der Sortierung in die GridView ist sehr einfach. Legen Sie einfach in die Datei Student.aspx **AllowSorting** auf **"true"** in die GridView. Sie müssen nicht festlegen einer **SortExpression** Wert für jede Spalte, wie die DataField automatisch verwendet wird. Die GridView ändert die Abfrage zum Anordnen der Daten nach dem ausgewählten Wert enthalten. Der hervorgehobene Code unten zeigt dem Hinzufügen, dass Sie zum Aktivieren der Sortierung vornehmen müssen.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Führen Sie die Webanwendung, und Testen Sie nach den Werten in verschiedenen Spalten sortieren Studentendatensätze.

![Studenten sortieren](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Hinzufügen von paging

Aktivieren von Paging ist sehr einfach. Legen Sie in der GridView der **AllowPaging** Eigenschaft **"true"** und legen Sie die **PageSize** Eigenschaft, um die Anzahl der Datensätze, die auf jeder Seite angezeigt werden sollen. In diesem Lernprogramm können Sie es auf 4 festgelegt.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Führen Sie die Webanwendung, und beachten Sie, dass nun die Datensätze mit mehr als 4 auf einer Seite angezeigten Datensätze über mehrere Seiten unterteilt sind.

![Hinzufügen von paging](sorting-paging-and-filtering-data/_static/image4.png)

Verzögerte abfrageausführung verbessert die anwendungseffizienz. Anstatt das gesamte Dataset abgerufen werden, ändert die GridView die Abfrage aus, um nur die Datensätze für die aktuelle Seite abrufen.

## <a name="filter-records-by-user-selection"></a>Filtern von Datensätzen durch Auswahl des Benutzers

Wurden die modellbindung fügt mehrere Attribute, die Sie festlegen, wie Sie den Wert für einen Parameter in einem Modell Bindungsmethode festlegen können. Diese Attribute sind der **System.Web.ModelBinding** Namespace. Dazu zählen:

- Steuerelement
- Cookie
- Formular
- Profile
- QueryString
- RouteData
- Sitzung
- UserProfile
- "ViewState" Speichern

In diesem Lernprogramm verwenden Sie den Wert eines Steuerelements um zu filtern, welche Datensätze in die GridView angezeigt werden. Fügen Sie der **Steuerelement** -Attribut auf die Abfragemethode, die Sie zuvor erstellt hatten. In einem [später](using-query-string-values-to-retrieve-data.md) Lernprogramm übernehmen Sie die **QueryString** -Attribut auf einen Parameter, um anzugeben, dass der Wert des Parameters einen Wert der Abfragezeichenfolge stammen.

Fügen Sie oberhalb der ValidationSummary zunächst eine Dropdown-Liste für das Filtern, welche Studenten angezeigt werden.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

In der Code-Behind-Datei ändern Sie die select-Methode Empfang ein Werts aus dem Steuerelement, und legen Sie den Namen des Parameters auf den Namen des Steuerelements, das den Wert bereitstellt.

Müssen Sie hinzufügen, eine **mit** -Anweisung für die **System.Web.ModelBinding** Namespace das Steuerelementattribut aufgelöst.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

Der folgende Code zeigt die select-Methode erneut gearbeitet, um die zurückgegebenen Daten basierend auf dem Wert von der Dropdown-Liste zu filtern. Hinzufügen eines Steuerelements-Attributs an, bevor ein Parameter gibt an, dass der Wert für diesen Parameter aus einem Steuerelement mit dem gleichen Namen.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Führen Sie die Webanwendung, und wählen Sie unterschiedliche Werte aus der Dropdown-Liste aus, um die Liste der Schüler zu filtern.

![Filter Studenten](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Schlussfolgerung

In diesem Lernprogramm ermöglichte Sortierung und Auslagerung der Daten. Sie wird aktiviert, Filtern von Daten durch den Wert eines Steuerelements.

In der nächsten [Lernprogramm](integrating-jquery-ui.md) Sie werden die Benutzeroberfläche erhöhen, indem das Integrieren von einem JQuery UI-Widget in der dynamic Data-Vorlage.

>[!div class="step-by-step"]
[Zurück](updating-deleting-and-creating-data.md)
[Weiter](integrating-jquery-ui.md)
