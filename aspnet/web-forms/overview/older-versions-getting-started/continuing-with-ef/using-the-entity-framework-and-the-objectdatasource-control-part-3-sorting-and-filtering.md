---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Verwenden das Entity Framework 4.0 und das ObjectDataSource-Steuerelement, Teil 3: Sortieren und Filtern von | Microsoft Docs'
author: tdykstra
description: Diese Reihe von Lernprogrammen baut auf der Contoso-University-Webanwendung, die von den ersten Schritten mit der Entity Framework 4.0 Tutorial Reihe erstellt wird. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: e412d3ad98a37931e7190a4909cb09fa2abfb3d0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887654"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a>Verwenden das Entity Framework 4.0 und das ObjectDataSource-Steuerelement, Teil 3: Sortieren und Filtern
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Diese Reihe von Lernprogrammen in der Contoso-University Webanwendung durch die erstellte builds der [erste Schritte mit dem Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) Reihe von Lernprogrammen. Wenn Sie die frühere Lernprogramme nicht abgeschlossen wurde, als Ausgangspunkt für dieses Lernprogramm können Sie [Herunterladen der Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , die Sie erstellt haben würden. Sie können auch [Herunterladen der Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , die durch das vollständige Lernprogramm Reihe erstellt wird. Wenn Sie Fragen zu den Lernprogrammen haben, können Sie stellen Sie diese auf die [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx).


Im vorherigen Lernprogramm implementieren Sie das Repositorymuster in einer n-Tier-Webanwendung, die das Entity Framework verwendet und die `ObjectDataSource` Steuerelement. Dieses Lernprogramm zeigt, wie zum Sortieren und Filtern von und Behandeln von Master / Detail-Szenarien. Fügen Sie die folgenden Erweiterungen der *Departments.aspx* Seite:

- Ein Textfeld, um Benutzern ermöglichen, Abteilungen namentlich auszuwählen.
- Eine Liste der Kurse für jede Abteilung, die im Raster angezeigt wird.
- Die Fähigkeit zum Sortieren, indem Sie auf die Spaltenüberschriften.

[![Image01 abgerufen wird](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)

## <a name="adding-the-ability-to-sort-gridview-columns"></a>GridView-Sortierspalten hinzugefügt die Möglichkeit

Öffnen der *Departments.aspx* Seite, und fügen eine `SortParameterName="sortExpression"` -Attribut auf die `ObjectDataSource` Steuerelement namens `DepartmentsObjectDataSource`. (Später erstellen Sie eine `GetDepartments` -Methode, die einen Parameter mit dem Namen akzeptiert `sortExpression`.) Das Markup für das Anfangstag des Steuerelements sieht nun wie im folgende Beispiel.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

Hinzufügen der `AllowSorting="true"` -Attribut des öffnenden Tags von der `GridView` Steuerelement. Das Markup für das Anfangstag des Steuerelements sieht nun wie im folgende Beispiel.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

In *Departments.aspx.cs*, legen Sie die Standard-Sortierreihenfolge durch Aufrufen der `GridView` des Steuerelements `Sort` Methode aus der `Page_Load` Methode:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

Sie können den Code, die sortiert oder Filtern hinzufügen, die Business Logic-Klasse oder der Repository-Klasse. Wenn der Business Logic-Klasse verwendet wird, den Sortier- oder Filterinformationen Arbeit erfolgt, nachdem die Daten aus der Datenbank abgerufen werden, weil die Business Logic-Klasse mit arbeitet ein `IEnumerable` Repository zurückgegebenes Objekt. Wenn Sie sortieren und Filtern von Code in der Klasse Repository hinzufügen und Sie dies, bevor Sie eine LINQ-Ausdruck tun oder Objektabfrage, um konvertiert wurde ein `IEnumerable` -Objekt Befehle zur Verarbeitung, die in der Regel effizienter ist an die Datenbank übermittelt. In diesem Lernprogramm implementieren Sie sortieren und Filtern in einer Weise, die bewirkt, dass die Verarbeitung von der Datenbank ausgeführt werden – d. h. im Repository.

Um Sortierung Funktion hinzuzufügen, müssen Sie eine neue Methode zur Repository-Schnittstelle, und Repositoryklassen sowie hinsichtlich der Geschäftsklasse Logik hinzufügen. In der *ISchoolRepository.cs* Datei, fügen Sie einen neuen `GetDepartments` Methode, eine `sortExpression` Parameter, der verwendet wird, um die Liste der Abteilungen zu sortieren, die zurückgegeben wird:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

Die `sortExpression` Parameter geben die Spalte, nach denen sortiert und die Sortierreihenfolge an.

Hinzufügen von Code für die neue Methode, die die *SchoolRepository.cs* Datei:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

Ändern Sie den vorhandenen parameterlosen `GetDepartments` Methode, die die neue Methode aufgerufen:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

Im Testprojekt befindet, fügen Sie die folgende neue Methode zum *MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

Wenn Sie vorhaben, wurden alle Komponententests zu erstellen, die diese Methode zurückgeben einer sortierten Liste abhängen, müssen Sie die Liste zu sortieren, bevor Sie Sie zurückgeben. Tests wird nicht wie in diesem Lernprogramm erstellen Sie damit die Methode nur die ungeordnete Liste der Abteilungen zurückgeben kann.

In der *SchoolBL.cs* Datei, fügen Sie die folgende neue Methode für die Business Logic-Klasse:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

Dieser Code übergibt der Sortierparameter an die Repository-Methode.

Führen Sie die *Departments.aspx* Seite.

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)

Sie können nun eine beliebige Spaltenüberschrift, um nach dieser Spalte sortieren klicken. Wenn die Spalte bereits sortiert sind, kehrt die auf die Überschrift der Sortierreihenfolge an.

## <a name="adding-a-search-box"></a>Ein Suchfeld hinzufügen

In diesem Abschnitt fügen Sie fügen ein Textfeld für die Suche hinzu, verknüpfen Sie es mit der `ObjectDataSource` Steuerelement, mit einem Steuerelementparameter, und fügen Sie eine Methode zur Business Logik Klasse, um die Filterung unterstützt.

Öffnen der *Departments.aspx* Seite, und fügen Sie das folgende Markup zwischen den Spaltenüberschrift und dem ersten `ObjectDataSource` Steuerelement:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

In der `ObjectDataSource` Steuerelement namens `DepartmentsObjectDataSource`, gehen Sie folgendermaßen vor:

- Hinzufügen einer `SelectParameters` -Element für einen Parameter namens `nameSearchString` abruft, die in eingegebenen Wert die `SearchTextBox` Steuerelement.
- Ändern der `SelectMethod` -Attributwert `GetDepartmentsByName`. (Sie müssen diese Methode später erstellen.)

Das Markup für die `ObjectDataSource` -Steuerelement jetzt im folgende Beispiel ähnelt:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

In *ISchoolRepository.cs*, Hinzufügen einer `GetDepartmentsByName` Methode, die beide annimmt `sortExpression` und `nameSearchString` Parameter:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

In *SchoolRepository.cs*, fügen Sie die folgende neue Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

Dieser Code verwendet eine `Where` Methode, um die Elemente auszuwählen, die die Suchzeichenfolge enthalten. Wenn die zu suchende Zeichenfolge leer ist, werden alle Datensätze ausgewählt werden. Beachten Sie, dass bei der Angabe Methodenaufrufe in einer einzelnen Anweisung wie folgt zusammen (`Include`, klicken Sie dann `OrderBy`, klicken Sie dann `Where`), wird die `Where` Methode muss immer in der letzten.

Ändern Sie den vorhandenen `GetDepartments` Methode, eine `sortExpression` Parameter aufrufen, die neue Methode:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

In *MockSchoolRepository.cs* im Testprojekt befindet, fügen Sie die folgende neue Methode:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

In *SchoolBL.cs*, fügen Sie die folgende neue Methode hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

Führen Sie die *Departments.aspx* Seite, und geben Sie eine Suchzeichenfolge ein, um sicherzustellen, dass die Auswahllogik funktioniert. Lassen Sie das Textfeld leer, und wiederholen Sie eine Suche aus, um sicherzustellen, dass alle Datensätze zurückgegeben werden.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)

## <a name="adding-a-details-column-for-each-grid-row"></a>Hinzufügen einer Spalteninhalts "Details" für jede Zeile des Datenblatts

Als Nächstes möchten Sie alle von der Kurse für jede Abteilung in der rechten Zelle des Rasters angezeigt anzeigen. Zu diesem Zweck verwenden Sie eine geschachtelte `GridView` Steuerelement und Databind er Daten aus der `Courses` Navigationseigenschaft der `Department` Entität.

Open *Departments.aspx* und in das Markup für die `GridView` zu steuern, geben Sie einen Handler für das `RowDataBound` Ereignis. Das Markup für das Anfangstag des Steuerelements sieht nun wie im folgende Beispiel.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

Fügen Sie einen neuen `TemplateField` Elements nach dem `Administrator` Vorlagenfeld:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

Dieses Markup erstellt einen geschachtelten `GridView` Steuerelement, das die Kurs-Anzahl und den Titel der Liste der Kurse anzeigt. Es wird eine Datenquelle nicht angegeben, da Sie Databind müssen sie im Code in der `RowDataBound` Handler.

Open *Departments.aspx.cs* und fügen Sie den folgenden Ereignishandler für das `RowDataBound` Ereignis:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

Dieser Code Ruft die `Department` Entität, auf die Ereignisargumente, konvertiert der `Courses` -Navigationseigenschaft auf eine `List` Auflistung und stellt die geschachtelte `GridView` auf die Auflistung.

Öffnen der *SchoolRepository.cs* Datei, und geben Sie unverzüglichem Laden für die `Courses` Navigationseigenschaft durch Aufrufen der `Include` Methode in der Objektabfrage, die Sie, in Erstellen der `GetDepartmentsByName` Methode. Die `return` -Anweisung in die `GetDepartmentsByName` Methode sieht wie im folgende Beispiel.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

Führen Sie die Seite. Zusätzlich zu sortieren und Filtern von Funktion, die Sie zuvor hinzugefügt haben, zeigt des GridView-Steuerelements jetzt geschachtelte Kursdetails für jede Abteilung.

[![Image01 abgerufen wird](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)

Dies schließt die Einführung zu sortieren, Filtern und Master / Detail-Szenarien. In den nächsten Lernprogrammen sehen Sie, wie Concurrency behandelt.

> [!div class="step-by-step"]
> [Zurück](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Weiter](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)
