---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Aktualisieren, löschen und Erstellen von Daten mit modellbindung und Web Forms | Microsoft-Dokumentation
author: Rick-Anderson
description: Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 127543b0696b01f136b340d07f6f806b6a6fb1eb
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021572"
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Aktualisieren, löschen und Erstellen von Daten mit modellbindung und Web forms
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung, wird die dateninteraktion mehr geradlinigere als Umgang mit Data Source-Objekte (z. B. "ObjectDataSource" oder SqlDataSource-Steuerelement). Diese Serie beginnt mit einführendes Material und verschiebt in späteren Tutorials zu erweiterten Konzepten übergegangen.
> 
> Dieses Tutorial veranschaulicht das Erstellen, aktualisieren und Löschen von Daten mit modellbindung. Sie werden die folgenden Eigenschaften festlegen:
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Diese Eigenschaften erhalten den Namen der Methode, die den entsprechenden Vorgang verarbeitet. In dieser Methode geben Sie die Logik für die Interaktion mit den Daten.
> 
> Dieses Tutorial baut auf das Projekt erstellt haben, in der ersten [Teil](retrieving-data.md) der Reihe.
> 
> Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB. Der herunterladbare Code funktioniert mit Visual Studio 2012 oder Visual Studio 2013. Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Tutorial gezeigt wird.


## <a name="what-youll-build"></a>Sie lernen Folgendes

In diesem Tutorial müssen Sie folgende Aktionen ausführen:

1. Hinzufügen von dynamic Data-Vorlagen
2. Aktivieren Sie aktualisieren und Löschen von Daten über Bindung Objektmodellmethoden
3. Anwenden von Regeln für die datenvalidierung – ermöglichen das Erstellen eines neuen Datensatzes in der Datenbank

## <a name="add-dynamic-data-templates"></a>Hinzufügen von dynamic Data-Vorlagen

Um die bestmögliche benutzererfahrung bieten und minimieren codewiederholungen, verwenden Sie dynamische Datenvorlagen. Sie können vorgefertigte dynamic Data-Vorlagen in Ihrer vorhandenen Website einfach integrieren, durch die Installation von NuGet-Paket.

Von der **NuGet-Pakete verwalten**, installieren Sie die **DynamicDataTemplatesCS**.

![Dynamic Data-Vorlagen](updating-deleting-and-creating-data/_static/image1.png)

Beachten Sie, dass das Projekt nun einen Ordner namens enthält **DynamicData**. In diesem Ordner finden Sie die Vorlagen, die automatisch auf dynamischen Steuerelementen in Ihren Webformularen angewendet werden.

![Dynamic Data-Ordner](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Enable aktualisieren und löschen

Aktivieren von Benutzern zum Aktualisieren und Löschen von Datensätzen in der Datenbank ist sehr ähnlich ist, an den Prozess zum Abrufen von Daten. In der **UpdateMethod** und **DeleteMethod** Eigenschaften, Sie geben Sie die Namen der Methoden, die diese Vorgänge durchzuführen. Sie können auch Geben Sie die automatische Generierung von bearbeiten und Löschen von Schaltflächen, mit einem GridView-Steuerelement. Der folgende hervorgehobene Code zeigt die der GridView-Code-Erweiterungen.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

In der CodeBehind-Datei, fügen Sie eine using-Anweisung für **System.Data.Entity.Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Anschließend fügen Sie das folgende Update und delete-Methoden.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

Die **TryUpdateModel** Methode gilt die entsprechende datengebundene Werte aus dem Webformular, für das Datenelement. Das Datenelement wird basierend auf den Wert des Id-Parameters abgerufen.

## <a name="enforce-validation-requirements"></a>Erzwingen Sie die überprüfungsanforderungen zu erfüllen

Die Validierungsattribute, die auf die Eigenschaften "FirstName, LastName und Year" in der Klasse "Student" angewendet werden automatisch erzwungen werden, wenn die Daten aktualisieren. Die DynamicField--Steuerelemente hinzufügen, Client und Server-Validierungssteuerelemente, die basierend auf den Validierungsattributen. Die FirstName und LastName-Eigenschaften sind erforderlich. FirstName kann nicht mehr, als 20 Zeichen umfassen und darf in "LastName" 40 Zeichen nicht überschreiten. Jahr, muss ein gültiger Wert für die AcademicYear-Enumeration sein.

Wenn der Benutzer eine der Voraussetzungen für Validierung verletzt, wird das Update nicht fortgesetzt werden. Um die Fehlermeldung anzuzeigen, fügen Sie ein ValidationSummary-Steuerelement oben GridView hinzu. Um Fehler bei der Validierung von der modellbindung anzuzeigen, legen die **ShowModelStateErrors** -Eigenschaftensatz auf **"true"**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Führen Sie die Webanwendung aktualisieren und Löschen von Datensätzen.

![Aktualisieren von Daten](updating-deleting-and-creating-data/_static/image3.png)

Beachten Sie, dass, wenn in den Bearbeitungsmodus automatisch der Wert für die Eigenschaft "Year" als eine Dropdownliste gerendert wird. Die Eigenschaft "Year" ist ein Enumerationswert, und die dynamic Data-Vorlage für einen Enumerationswert gibt eine Dropdown-Liste für die Bearbeitung. Sie finden diese Vorlage, indem Sie öffnen die **Enumeration\_Edit.ascx zeigt** Datei die **DynamicData**/**"FieldTemplates"** Ordner.

Wenn Sie gültige Werte angeben, wird das Update erfolgreich abgeschlossen. Wenn Sie eine der Voraussetzungen für Validierung verletzen würden, das Update wird nicht fortgesetzt, und eine Fehlermeldung wird oberhalb des Rasters angezeigt.

![Fehlermeldung](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Neue Datensätze hinzufügen

Das GridView-Steuerelement enthält keinen der **InsertMethod** Eigenschaft und kann daher nicht für das Hinzufügen eines neuen Datensatzes mit modellbindung verwendet werden. Sie finden die InsertMethod-Eigenschaft in der **FormView**, **DetailsView**, oder **ListView** Steuerelemente. In diesem Tutorial verwenden Sie einen FormView-Steuerelement, um einen neuen Datensatz hinzuzufügen.

Fügen Sie zunächst einen Link auf die neue Seite, die Sie zum Hinzufügen eines neuen Eintrags erstellen. Fügen Sie oberhalb der ValidationSummary:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

Der neue Link wird am oberen Rand der Inhalt für die Schüler/Studenten-Seite angezeigt.

![Neuer link](updating-deleting-and-creating-data/_static/image5.png)

Anschließend fügen Sie ein neues Webformular mit einer Masterseite hinzu, und nennen Sie sie **AddStudent**. Wählen Sie als Masterseite Site.Master.

Rendert Sie die Felder zum Hinzufügen eines neuen Studenten mit einem **erzeugt wird** Steuerelement. Das Steuerelement erzeugt wird, rendert, bearbeitbaren Eigenschaften in der Klasse, die in der ItemType-Eigenschaft angegeben. Die Eigenschaft "StudentID" wurde mit markiert die **[ScaffoldColumn(false)]** Attribut, damit es nicht gerendert wird. Fügen Sie im Platzhalter MainContent der AddStudent Seite den folgenden Code ein.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

Fügen Sie in der CodeBehind-Datei (AddStudent.aspx.cs) eine **mit** -Anweisung für die **ContosoUniversityModelBinding.Models** Namespace.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Anschließend fügen Sie die folgenden Methoden zum angeben, wie zum Einfügen eines neuen Datensatzes und einen Ereignishandler für die Schaltfläche "Abbrechen".

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Speichern Sie alle Änderungen an.

Führen Sie die Webanwendung, und erstellen Sie einen neuen Studenten.

![Hinzufügen von neuen Studenten](updating-deleting-and-creating-data/_static/image6.png)

Klicken Sie auf **einfügen** und beachten Sie, dass der neue Student erstellt wurde.

![Anzeigen von neuen Studenten](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Schlussbemerkung

In diesem Tutorial haben aktiviert Sie aktualisieren, löschen und Erstellen von Daten. Sie wird sichergestellt, dass Validierungsregeln angewendet werden, bei der Interaktion mit den Daten.

In den nächsten [Tutorial](sorting-paging-and-filtering-data.md) in dieser Reihe, aktivieren Sie sortieren, paging und Filtern von Daten.

> [!div class="step-by-step"]
> [Zurück](retrieving-data.md)
> [Weiter](sorting-paging-and-filtering-data.md)
