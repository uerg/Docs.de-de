---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Verwenden von Entitätsframework 4.0 und ObjectDataSource-Steuerelement, Teil 3: Sortieren und Filtern | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erstellt in der Contoso University-Webanwendung, die von den ersten Schritten mit der Entity Framework 4.0-Tutorial-Reihe erstellt wird. ICH...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 15d9a7ecd3119af02576cbcf728828353748a513
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806997"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Verwenden von Entitätsframework 4.0 und ObjectDataSource-Steuerelement, Teil 3: Sortieren und Filtern
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Dieser tutorialreihe erstellt, in der Contoso University-Webanwendung, die erstellt wird die [erste Schritte mit Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) Tutorial-Reihe. Wenn Sie den vorherigen Tutorials wurde nicht abgeschlossen haben, als Ausgangspunkt für dieses Tutorial können Sie [Laden Sie die Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , die Sie erstellt haben würden. Sie können auch [Laden Sie die Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , indem Sie die vollständige Reihe von Tutorials erstellt wird. Wenn Sie Fragen zu den Lernprogrammen haben, können Sie sie veröffentlichen das [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx).


Im vorherigen Tutorial, die Sie implementiert des Repository-Musters in einer n-schichtige Webanwendung, die das Entity Framework verwendet und die `ObjectDataSource` Steuerelement. Dieses Tutorial veranschaulicht das Sortieren und Filtern und Verarbeiten von Master / Detail-Szenarien. Fügen Sie die folgenden Erweiterungen der *Departments.aspx* Seite:

- Ein Textfeld, um den Benutzern ermöglichen, Abteilungen anhand des Namens auszuwählen.
- Eine Liste der Kurse jeder Abteilung, die im Raster angezeigt wird.
- Die Fähigkeit, indem Sie auf Spaltenüberschriften sortieren.

[![Image01 abgerufen wird](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>Hinzufügen der Funktion zum Sortieren von GridView-Spalten

Öffnen der *Departments.aspx* Seite, und fügen eine `SortParameterName="sortExpression"` -Attribut auf die `ObjectDataSource` Steuerelement mit dem Namen `DepartmentsObjectDataSource`. (Später erstellen Sie eine `GetDepartments` -Methode, die einen namens Parameter `sortExpression`.) Das Markup für das Anfangstag des Steuerelements ähnelt nun dem folgenden Beispiel.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Hinzufügen der `AllowSorting="true"` Attribut an das öffnende Tag das `GridView` Steuerelement. Das Markup für das Anfangstag des Steuerelements ähnelt nun dem folgenden Beispiel.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

In *Departments.aspx.cs*, legen Sie die Standardsortierreihenfolge durch Aufrufen der `GridView` des Steuerelements `Sort` Methode aus der `Page_Load` Methode:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Sie können den Code, der zum Sortieren oder Filtern in der Business Logic-Klasse oder die "Repository"-Klasse hinzufügen. Wenn der Business Logic-Klasse verwendet wird, Sortier- oder Filterkriterien Arbeit ausgeführt, nachdem die Daten aus der Datenbank abgerufen werden, da die Business Logic-Klasse mit arbeitet ein `IEnumerable` Repository zurückgegebenes Objekt. Wenn Sie sortieren und Filtern von Code in der repositoryklasse hinzufügen, und Sie dies, bevor Sie eine LINQ-Ausdruck tun oder Objektabfrage konvertiert wurde ein `IEnumerable` Objekts, die Befehle werden durch übergeben werden, in der Datenbank für die Verarbeitung, die in der Regel effizienter ist. In diesem Tutorial implementieren Sie sortieren und Filtern in einer Weise, die bewirkt, dass die Verarbeitung von der Datenbank ausgeführt werden – d. h. im Repository.

Um Sortierung Funktion hinzuzufügen, müssen Sie die Repository-Schnittstelle und Repositoryklassen auch hinsichtlich der Business Logic-Klasse eine neue Methode hinzufügen. In der *ISchoolRepository.cs* Datei, fügen Sie einen neuen `GetDepartments` Methode, eine `sortExpression` Parameter, der verwendet wird, um die Liste von Abteilungen zu sortieren, die zurückgegeben wird:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

Die `sortExpression` -Parameter wird die Spalte, nach denen sortiert und die Sortierreihenfolge angeben.

Fügen Sie Code für die neue Methode, um die *SchoolRepository.cs* Datei:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Ändern Sie die vorhandene parameterlosen `GetDepartments` Methode, die die neue Methode aufgerufen:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

Fügen Sie in das Testprojekt, und die folgende neue Methode zum *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Wenn Sie möchten alle Komponententests zu erstellen, die von dieser Methode zurückgeben einer sortierten Liste abhing, müssen Sie würde die Liste zu sortieren, bevor sie zurückgegeben wird. Tests wird nicht wie in diesem Tutorial erstellen Sie damit die Methode nur die unsortierte Liste von Abteilungen zurückgeben kann.

In der *SchoolBL.cs* Datei, die der Business Logic-Klasse die folgende neue Methode hinzufügen:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Dieser Code übergibt der Sortierparameter an die Repository-Methode.

Führen Sie die *Departments.aspx* Seite.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Sie können jetzt eine beliebige Spaltenüberschrift, um nach dieser Spalte sortieren klicken. Wenn die Spalte bereits sortiert ist, kehrt das Klicken auf die Überschrift die Sortierrichtung.

## <a name="adding-a-search-box"></a>Hinzufügen eines Suchfelds

In diesem Abschnitt werden Sie ein Textfeld Suche hinzufügen, verknüpfen Sie es mit der `ObjectDataSource` Steuerelement, mit einem Steuerelementparameter, und fügen Sie eine Methode zur Business Logic Klasse, um die Filterung von versionsbereichen.

Öffnen der *Departments.aspx* Seite, und fügen Sie das folgende Markup zwischen die Überschrift und die erste `ObjectDataSource` Steuerelement:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

In der `ObjectDataSource` Steuerelement mit dem Namen `DepartmentsObjectDataSource`, gehen Sie folgendermaßen vor:

- Hinzufügen einer `SelectParameters` -Element für einen Parameter namens `nameSearchString` abruft, die in eingegebenen Wert die `SearchTextBox` Steuerelement.
- Ändern der `SelectMethod` -Attributwert auf `GetDepartmentsByName`. (Sie müssen diese Methode später erstellen.)

Das Markup für die `ObjectDataSource` Steuerelement wird jetzt im folgende Beispiel ähnelt:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

In *ISchoolRepository.cs*, Hinzufügen einer `GetDepartmentsByName` Methode, die beide akzeptiert `sortExpression` und `nameSearchString` Parameter:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

In *SchoolRepository.cs*, fügen Sie die folgende neue Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Dieser Code verwendet ein `Where` Methode zum Auswählen von Elementen, die die Suchzeichenfolge enthalten. Wenn die Suchzeichenfolge leer ist, werden alle Datensätze ausgewählt werden. Beachten Sie, dass, wenn Sie die Methode ruft zusammen in einer Anweisung wie folgt angeben (`Include`, klicken Sie dann `OrderBy`, klicken Sie dann `Where`), wird die `Where` Methode muss immer in der letzten.

Ändern Sie die vorhandene `GetDepartments` Methode, eine `sortExpression` Parameter, um die neue Methode aufrufen:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

In *MockSchoolRepository.cs* in das Testprojekt, und fügen Sie die folgende neue Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

In *SchoolBL.cs*, fügen Sie die folgende neue Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Führen Sie die *Departments.aspx* Seite, und geben Sie eine Suchzeichenfolge, um sicherzustellen, dass die Auswahllogik funktioniert. Lassen Sie das Textfeld leer, und versuchen Sie es einer Suche, um sicherzustellen, dass alle Datensätze zurückgegeben werden.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Hinzufügen einer Detailspalte für jede Zeile des Datenblatts

Als Nächstes möchten Sie alle Kurse für jede Abteilung in der rechten Zelle des Rasters angezeigt. Zu diesem Zweck verwenden Sie eine geschachtelte `GridView` Kontrolle und Databind damit Daten aus der `Courses` Navigationseigenschaft der `Department` Entität.

Open *Departments.aspx* und im Markup für die `GridView` zu steuern, geben Sie einen Handler für die `RowDataBound` Ereignis. Das Markup für das Anfangstag des Steuerelements ähnelt nun dem folgenden Beispiel.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Fügen Sie einen neuen `TemplateField` Element an, nach der `Administrator` Vorlagenfeld:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Dieses Markup erstellt eine geschachtelte `GridView` Steuerelement, das der Kursnummer und Titel der Liste der Kurse anzeigt. Es wird eine Datenquelle nicht angegeben, da Databind wird es im Code in die `RowDataBound` Handler.

Open *Departments.aspx.cs* und fügen Sie den folgenden Ereignishandler für die `RowDataBound` Ereignis:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Dieser Code Ruft die `Department` Entität aus den Ereignisargumenten, konvertiert die `Courses` Navigationseigenschaft für eine `List` Sammlung und stellt die geschachtelte `GridView` der Auflistung.

Öffnen der *SchoolRepository.cs* Datei, und geben Sie eager Loading für die `Courses` Navigationseigenschaft durch Aufrufen der `Include` -Methode in der die Objektabfrage, die Sie, in Erstellen der `GetDepartmentsByName` Methode. Die `return` -Anweisung in der `GetDepartmentsByName` Methode ähnelt nun dem folgenden Beispiel.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Führen Sie die Seite. Neben den sortieren und Filtern die Funktion, die Sie zuvor hinzugefügt haben, zeigt das GridView-Steuerelement jetzt geschachtelte Kursdetails für jede Abteilung.

[![Image01 abgerufen wird](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Dies schließt die Einführung zu sortieren, Filtern und Master / Detail-Szenarien ab. Im nächsten Tutorial sehen Sie, wie Parallelität behandelt werden.

> [!div class="step-by-step"]
> [Zurück](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Weiter](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
