---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Aktualisieren, löschen und Erstellen von Daten mit modellbindung und WebForms | Microsoft Docs
author: tfitzmac
description: Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: e6536f7858afde5faf3aedd34f3cbe95c5ed0d53
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Aktualisieren, löschen und Erstellen von Daten mit modellbindung und WebForms
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion mehr die selbsterklärend als Umgang mit Data Source-Objekte (z. B. ObjectDataSource oder SqlDataSource). Diese Reihe beginnt mit einführendes Material und verschiebt die erweiterte Konzepte in späteren Lernprogrammen.
> 
> Dieses Lernprogramm veranschaulicht das Erstellen, aktualisieren und Löschen von Daten mit modellbindung. Sie können die folgenden Eigenschaften festlegen:
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Diese Eigenschaften erhalten den Namen der Methode, die den entsprechenden Vorgang verarbeitet. Innerhalb dieser Methode können bereitstellen Sie die Logik für die Interaktion mit den Daten.
> 
> Dieses Lernprogramm baut auf das Projekt erstellt, in der ersten [Teil](retrieving-data.md) der Reihe.
> 
> Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB dar. Der Code zum Herunterladen funktioniert mit Visual Studio 2012 oder Visual Studio 2013. Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Lernprogramm gezeigt wird.


## <a name="what-youll-build"></a>Was müssen Sie erstellen

In diesem Lernprogramm lernen Sie folgende Aktionen ausführen:

1. Dynamic Data-Vorlagen hinzufügen
2. Aktivieren Sie aktualisieren und Löschen von Daten über die Bindung Modellmethoden
3. Anwenden von Validierungsregeln Daten – ermöglichen das Erstellen eines neuen Datensatzes in der Datenbank

## <a name="add-dynamic-data-templates"></a>Dynamic Data-Vorlagen hinzufügen

Um bestmögliche benutzererfahrung bieten und codewiederholungen minimieren, verwenden Sie dynamische Datenvorlagen. Sie können problemlos in Ihre vorhandene Website vorgefertigten dynamic Data-Vorlagen integrieren, durch die Installation von NuGet-Paket.

Aus der **NuGet-Pakete verwalten**, installieren Sie die **DynamicDataTemplatesCS**.

![Dynamic Data-Vorlagen](updating-deleting-and-creating-data/_static/image1.png)

Beachten Sie, dass das Projekt jetzt einen Ordner namens enthält **DynamicData**. In diesem Ordner finden Sie die Vorlagen, die von dynamischen Steuerelementen in WebForms automatisch angewendet werden.

![Dynamic Data-Ordner](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Enable aktualisieren und löschen

Aktivieren von Benutzern zu aktualisieren und Löschen von Datensätzen in der Datenbank ist sehr ähnlich, an den Prozess zum Abrufen von Daten. In der **UpdateMethod** und **DeleteMethod** Eigenschaften, geben Sie die Namen der Methoden, die diese Vorgänge ausführen. Sie können auch Geben Sie die automatische Generierung von bearbeiten und Löschen von Schaltflächen, mit einem GridView-Steuerelement. Die folgende hervorgehobene Code werden die Ergänzungen an die GridView-Code dargestellt.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

In der CodeBehind-Datei fügen Sie eine using-Anweisung für **System.Data.Entity.Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Anschließend fügen Sie das folgende Update und delete-Methode.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

Die **TryUpdateModel** Methode gilt die übereinstimmenden datengebundene Werte aus dem Webformular an, für das Datenelement. Das Datenelement wird basierend auf den Wert des Id-Parameters abgerufen.

## <a name="enforce-validation-requirements"></a>Erzwingen Sie die überprüfungsanforderungen zu erfüllen

Die überprüfungsattribute aus, denen auf die Eigenschaften FirstName, LastName und Jahr in der Student-Klasse angewendet werden automatisch erzwungen werden, wenn die Daten zu aktualisieren. Die DynamicField--Steuerelemente hinzufügen, Client und Server Validierungssteuerelemente, die basierend auf der Validierungsattribute. Die Eigenschaften FirstName und LastName sind erforderlich. FirstName nicht überschreiten 20 Zeichen lang sein und Nachname darf 40 Zeichen nicht überschreiten. Jahr muss einen gültigen Wert für die AcademicYear-Enumeration.

Wenn der Benutzer eine die Prüfungsanforderungen verletzt, wird das Update nicht fortgesetzt werden. Fügen Sie ValidationSummary Steuerelement über die GridView hinzu, um die Fehlermeldung anzuzeigen. Wenn die Validierungsfehler von der modellbindung anzeigen zu können, legen Sie die **ShowModelStateErrors** -Eigenschaftensatz auf **"true"**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Führen Sie die Webanwendung, und aktualisieren und Löschen von Datensätzen.

![Aktualisieren von Daten](updating-deleting-and-creating-data/_static/image3.png)

Beachten Sie, dass, wenn in den Bearbeitungsmodus automatisch der Wert für die Eigenschaft "Year" als eine Dropdown-Liste gerendert wird. Die Year-Eigenschaft ist ein Enumerationswert, und die dynamische Datenvorlage für einen Enumerationswert gibt eine Dropdownliste für die Bearbeitung. Sie finden diese Vorlage durch Öffnen der **Enumeration\_Edit.ascx zeigt** in der Datei die **DynamicData**/**FieldTemplates** Ordner.

Wenn Sie gültige Werte angeben, wird das Update erfolgreich abgeschlossen. Wenn Sie eine der Anforderungen Überprüfung verletzen, das Update wird nicht fortgesetzt, und folgende Fehlermeldung wird oberhalb des Rasters angezeigt.

![Fehlermeldung](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Neue Datensätze hinzufügen

GridView-Steuerelement enthält keinen der **InsertMethod** Eigenschaft und kann daher nicht für das Hinzufügen eines neuen Datensatzes mit modellbindung verwendet werden. Sie finden die InsertMethod-Eigenschaft in der **FormView**, **DetailsView**, oder **ListView** Steuerelemente. In diesem Lernprogramm verwenden Sie ein FormView-Steuerelement, um einen neuen Datensatz hinzuzufügen.

Fügen Sie zunächst einen Link auf die neue Seite, die Sie erstellen einen neuen Datensatz hinzuzufügen. Fügen Sie oberhalb der ValidationSummary:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

Der neue Link wird am oberen Rand der Inhalt für die Seite "Students" angezeigt.

![Link "Neues"](updating-deleting-and-creating-data/_static/image5.png)

Anschließend fügen Sie eine neue WebForm mithilfe einer Masterseite hinzu, und nennen Sie sie **AddStudent**. Wählen Sie Site.Master, als die Gestaltungsvorlage.

Sie werden die Felder für das Hinzufügen eines neuen Studenten mit Rendern einer **erzeugt wird** Steuerelement. Das Steuerelement erzeugt wird rendert, bearbeitbaren Eigenschaften in der Klasse, die in der ItemType-Eigenschaft angegeben. Die StudentID-Eigenschaft wurde gekennzeichnet, mit der **[ScaffoldColumn(false)]** Attribut, damit es nicht gerendert wird. Fügen Sie in den von der Seite "AddStudent" MainContent-Platzhalter den folgenden Code ein.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

Fügen Sie in der Code-Behind-Datei (AddStudent.aspx.cs) eine **mit** -Anweisung für die **ContosoUniversityModelBinding.Models** Namespace.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Anschließend fügen Sie die folgenden Methoden zum Einfügen eines neuen Datensatzes und einen Ereignishandler für die Schaltfläche "Abbrechen" angeben.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Alle Änderungen zu speichern.

Führen Sie die Webanwendung, und erstellen Sie einen neuen Studenten.

![Hinzufügen von neuen Studenten](updating-deleting-and-creating-data/_static/image6.png)

Klicken Sie auf **einfügen** , und beachten Sie die neue Studenten erstellt wurde.

![Anzeigen von neuen Studenten](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Schlussbemerkung

In diesem Lernprogramm ermöglichte aktualisieren, löschen und Erstellen von Daten. Sie wird sichergestellt, dass es sich um eine Überprüfungsregeln angewendet werden, bei der Interaktion mit den Daten.

In der nächsten [Lernprogramm](sorting-paging-and-filtering-data.md) in dieser Serie, aktivieren Sie sortieren, paging und Filtern von Daten.

> [!div class="step-by-step"]
> [Zurück](retrieving-data.md)
> [Weiter](sorting-paging-and-filtering-data.md)
