---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Verwenden von Entitätsframework 4.0 und ObjectDataSource-Steuerelement, Teil 1: Erste Schritte | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erstellt in der Contoso University-Webanwendung, die von den ersten Schritten mit der Entity Framework-Tutorial-Reihe erstellt wird. Wenn "yo"...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: ba3b65dededeca3534b9273bfd3ae48429711fce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369750"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Verwenden von Entitätsframework 4.0 und ObjectDataSource-Steuerelement, Teil 1: Erste Schritte
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Dieser tutorialreihe erstellt, in der Contoso University-Webanwendung, die erstellt wird die [erste Schritte mit Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) Tutorial-Reihe. Wenn Sie den vorherigen Tutorials wurde nicht abgeschlossen haben, als Ausgangspunkt für dieses Tutorial können Sie [Laden Sie die Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , die Sie erstellt haben würden. Sie können auch [Laden Sie die Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , indem Sie die vollständige Reihe von Tutorials erstellt wird.
> 
> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Die beispielanwendung ist eine Website für die fiktive Contoso University. Sie enthält Funktionen wie die Zulassung von Studenten, die Erstellung von Kursen und Aufgaben von Dozenten.
> 
> Das Tutorial zeigt Beispiele in C# geschrieben. Die [herunterladbare Beispielversion](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) in c# und Visual Basic-Code enthält.
> 
> ## <a name="database-first"></a>Zunächst-Datenbank
> 
> Es gibt drei Möglichkeiten, die Sie mit Daten im Entity Framework arbeiten können: *Database First*, *Model First*, und *Code First*. Dieses Tutorial ist für Database First. Weitere Informationen zu den Unterschieden zwischen den Workflows und Anleitungen dazu, wie die beste Option für Ihr Szenario auswählen, finden Sie unter [Entity Framework-Entwicklungsworkflows](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> Wie der erste Schritte-Serie dieser tutorialreihe verwendet das ASP.NET Web Forms-Modell, und setzt voraus, dass Sie wissen, wie mit ASP.NET Web Forms in Visual Studio arbeiten. Wenn dies nicht tun, werden dort [erste Schritte mit ASP.NET 4.5 Web Forms](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Wenn Sie mit ASP.NET MVC-Frameworks arbeiten lieber, finden Sie unter [erste Schritte mit Entity Framework mit ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Software-Versionen
> 
> | **In diesem Tutorial gezeigt** | **Funktioniert auch mit** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express für Web. Das Tutorial wurde nicht mit späteren Versionen von Visual Studio getestet. Es gibt viele Unterschiede in den Menüoptionen, Dialogfelder und Vorlagen. |
> | .NET 4 | .NET 4.5 ist abwärtskompatibel mit .NET 4, aber das Tutorial wurde mit .NET 4.5 nicht getestet. |
> | Entitätsframework 4 | Das Tutorial wurde mit höheren Versionen von Entity Framework noch nicht getestet. Ab Entity Framework 5 EF verwendet standardmäßig die `DbContext API` , die mit EF 4.1 eingeführt wurde. Das EntityDataSource-Steuerelement wurde entwickelt, mit der `ObjectContext` API. Informationen zur Verwendung von EntityDataSource steuern, mit der `DbContext` -API finden Sie unter [in diesem Blogbeitrag](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Fragen
> 
> Wenn Sie Fragen, die nicht direkt mit dem Tutorial verknüpft sind haben, können Sie sie veröffentlichen das [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx), [Entity Framework und LINQ to Entities-Forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), oder [ StackOverflow.com](http://stackoverflow.com/).


Die `EntityDataSource` -Steuerelement können Sie sehr schnell eine Anwendung zu erstellen, aber es in der Regel müssen Sie eine beträchtliche Menge an Geschäftslogik und Datenzugriff Logik zu Ihrem *aspx* Seiten. Wenn Sie Ihre Anwendung immer komplexer und fortlaufende Wartung erfordern erwarten, können Sie weitere Entwicklungszeit voraus investiert, um die erstellen eine *n-schichtige* oder *layered* Anwendungsstruktur Das ist besser verwaltbar. Zum Implementieren dieser Architektur, trennen Sie die Darstellungsschicht, von der Geschäftslogikschicht (BLL) und der Datenzugriffsebene (DAL). Eine Möglichkeit zum Implementieren dieser Struktur ist die Verwendung der `ObjectDataSource` -Steuerelement statt der `EntityDataSource` Steuerelement. Bei Verwendung der `ObjectDataSource` Steuerelement Sie Ihren eigenen Datenzugriffscode zu implementieren, und rufen Sie dann in *aspx* Seiten, die mithilfe eines Steuerelements, die die gleichen Funktionen wie bei anderen Datenquellen-Steuerelementen. Dadurch können Sie die Vorteile einer n-schichtverfahren mit den Vorteilen der Verwendung eines Web Forms-Steuerelements für den Datenzugriff zu kombinieren.

Die `ObjectDataSource` Steuerelement können Sie flexibler auf andere Weise auch. Da Sie Ihren eigenen Datenzugriffscode schreiben, ist es einfacher, mehr als nur lesen, einfügen, aktualisieren oder Löschen eines bestimmten Entitätstyps, die sind die Aufgaben, die die `EntityDataSource` Steuerelement ausführen soll. Sie können z. B. Führen Sie die Protokollierung wird jedes Mal, wenn eine Entität aktualisiert wird, Daten zu archivieren, wenn eine Entität gelöscht wird, oder automatisch überprüfen und aktualisieren Daten verwandter nach Bedarf, wenn Sie eine Zeile mit einem Fremdschlüsselwert einfügen.

## <a name="business-logic-and-repository-classes"></a>Geschäftslogik und Repositoryklassen

Ein `ObjectDataSource` Funktionsweise der Zugriffssteuerung durch den Aufruf einer Klasse, die Sie erstellen. Die Klasse enthält Methoden, die Daten abgerufen und aktualisiert, und Sie die Namen der diese Methoden, um die `ObjectDataSource` Steuerelement im Markup. Beim Rendern oder postback-Verarbeitung die `ObjectDataSource` Ruft die Methoden, die Sie angegeben haben.

Neben der grundlegende CRUD-Vorgänge, die Klasse, die Sie erstellen, um die Verwendung mit der `ObjectDataSource` Steuerelement zum Ausführen von Geschäftslogik müssen möglicherweise bei der `ObjectDataSource` gelesen oder aktualisiert Daten. Wenn Sie eine Abteilung aktualisieren, müssen Sie z. B. überprüfen, dass keine anderen Abteilungen derselben Administrator verfügen, da eine Person Administrator der mehr als eine Abteilung sein darf.

In einigen `ObjectDataSource` Dokumentation, wie z. B. die ["ObjectDataSource"-Klassenübersicht](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx), ruft das Steuerelement eine Klasse, die als eine *Geschäftsobjekt, das* , die sowohl von Geschäftslogik und Datenzugriff Logik enthält . In diesem Tutorial erstellen Sie separate Klassen für Geschäftslogik und Datenzugriffslogik. Wird aufgerufen, die Klasse, die Datenzugriffslogik kapselt einen *Repository*. Die Business Logic-Klasse enthält sowohl die Geschäftslogik Methoden als auch die Datenzugriffsmethoden, aber die Datenzugriff-Methoden aufrufen, das Repository, um Datenzugriffsaufgaben auszuführen.

Außerdem erstellen Sie eine Abstraktionsebene zwischen Ihrem BLL- und DAL, die automatisierte Komponententests erleichtert die BLL testen. Diese Abstraktionsschicht wird durch Erstellen einer Schnittstelle, und die Benutzeroberfläche verwenden, wenn Sie das Repository in der Geschäftslogik-Klasse instanziieren implementiert. Dadurch können Sie die Geschäftslogik-Klasse mit einem Verweis auf ein Objekt bereit, die die Repositoryschnittstelle implementiert. Für den normalen Betrieb Geben Sie ein Repositoryobjekt, das mit dem Entity Framework arbeitet. Zum Testen, geben Sie ein Repository-Objekt, das zusammen mit Daten in einer Weise, die Sie problemlos ändern können, z. B. Klassenvariablen als Sammlungen definiert.

Die folgende Abbildung zeigt den Unterschied zwischen einer Geschäftslogik-Klasse, die Datenzugriffslogik ohne ein Repository enthält und ein Repository verwendet.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Sie beginnen, Erstellen von Webseiten in der die `ObjectDataSource` -Steuerelement direkt in einem Repository gebunden ist, da sie nur die grundlegenden Datenzugriffsaufgaben ausführt. Im nächsten Tutorial erstellen Sie eine Business Logic-Klasse mit der Validierungslogik und Binden der `ObjectDataSource` Steuerelement auf diese Klasse anstelle der Repository-Klasse. Außerdem erstellen Sie Komponententests für die Validierungslogik. Fügen Sie in das dritte Tutorial dieser Reihe Sortier- und Filterfunktionen zur Anwendung.

Arbeiten Sie die Seiten, die in diesem Tutorial erstellen Sie mit der `Departments` Entitätenmenge des Datenmodells, die Sie in erstellt die [erste Schritte-Tutorial-Reihe](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01 abgerufen wird](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Aktualisieren der Datenbank und des Datenmodells

Sie werden in diesem Tutorial beginnen, indem Sie zwei Änderungen an der Datenbank, beide müssen die entsprechende Änderungen in das Datenmodell, das Sie in erstellt die [erste Schritte mit Entity Framework und Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) Tutorials. In einem dieser Tutorials Änderungen Sie im Designer, manuell, um das Datenmodell mit der Datenbank nach der Änderung an einer Datenbank zu synchronisieren. In diesem Tutorial verwenden Sie den Designer des **Modell aus Datenbank aktualisieren** Tool, um das Datenmodell automatisch zu aktualisieren.

### <a name="adding-a-relationship-to-the-database"></a>Hinzufügen einer Beziehung mit der Datenbank

In Visual Studio, öffnen Sie die Contoso University-Webanwendung, die Sie erstellt, in haben der [erste Schritte mit Entity Framework und Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) tutorialreihe hinzu, und öffnen Sie die `SchoolDiagram` Datenbankdiagramm.

Bei Betrachtung der `Department` Tabelle im Datenbankdiagramm, sehen Sie, dass sie verfügt über eine `Administrator` Spalte. Diese Spalte ist ein Fremdschlüssel für die `Person` Tabelle, aber keine Fremdschlüssel-Beziehung in der Datenbank definiert ist. Sie müssen die Beziehung zu erstellen und Aktualisieren des Datenmodells, damit diese Beziehung von Entity Framework automatisch behandeln kann.

Das Datenbankdiagramm mit der Maustaste der `Department` Tabelle, und wählen Sie **Beziehungen**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

In der **Foreign Key Relationships** auf **hinzufügen**, klicken Sie dann auf die Auslassungspunkte für **Tabellen- und Spaltenspezifikation**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

In der **Tabellen und Spalten** Dialogfeld ein, legen Sie die Primärschlüsseltabelle und Feld `Person` und `PersonID`, und legen Sie die Fremdschlüsseltabelle, und das Feld auf `Department` und `Administrator`. (Wenn Sie dies tun, ändert sich der Name der Beziehung aus `FK_Department_Department` zu `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Klicken Sie auf **OK** in die **Tabellen und Spalten** auf **schließen** in die **Foreign Key Relationships** ein, und speichern Sie die Änderungen. Wenn Sie aufgefordert werden, wenn Sie speichern möchten die `Person` und `Department` Tabellen, klicken Sie auf **Ja**.

> [!NOTE]
> Wenn Sie gelöscht haben `Person` Zeilen, die auf Daten entsprechen, die bereits in der `Administrator` Spalte nicht werden können, um diese Änderung zu speichern. In diesem Fall verwenden Sie die Tabellen-Editor in **Server-Explorer** sicherstellen, dass die `Administrator` Wert in jeder `Department` Zeile enthält die ID eines Datensatzes, die tatsächlich vorhanden ist die `Person` Tabelle.
> 
> Nachdem Sie die Änderung zu speichern, Sie ist nicht möglich, löschen Sie eine Zeile aus der `Person` Tabelle, wenn diese Person Administrator der Abteilung ist. In einer produktionsanwendung würden Sie eine spezifische Fehlermeldung bereitstellen, wenn eine Datenbank-Einschränkung wird verhindert, einen Löschvorgang dass aus, oder Sie einen überlappenden Löschvorgang geben. Ein Beispiel für einen überlappenden Löschvorgang angeben, finden Sie unter [Entity Framework und ASP.NET: Erste Schritte-Teil 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Hinzufügen einer Ansicht in der Datenbank

In der neuen *Departments.aspx* Seite, die Sie erstellen eine Dropdown-Liste der Dozenten, deren Namen im Format "Nachname, Vorname" bieten, sodass Benutzer abteilungsadministratoren auswählen können sollen. Um dies zu vereinfachen, erstellen Sie eine Ansicht in der Datenbank. Die Ansicht besteht aus nur die Daten, die von der Dropdown Liste benötigt: der vollständige Name (ordnungsgemäß formatiert) und dem Datensatzschlüssel.

In **Server-Explorer**, erweitern Sie *School.mdf*, mit der rechten Maustaste die **Ansichten** Ordner, und wählen **neue Sicht hinzufügen**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Klicken Sie auf **schließen** bei der **Tabelle hinzufügen** Dialogfeld wird angezeigt, und fügen Sie die folgende SQL-Anweisung in SQL-Bereich:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Speichern Sie die Ansicht als `vInstructorName`.

### <a name="updating-the-data-model"></a>Aktualisieren des Datenmodells

In der *DAL* Ordner die *SchoolModel.edmx* , mit der rechten Maustaste in der Entwurfsoberfläche, und wählen **Modell aus der Datenbank aktualisieren**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

In der **Datenbankobjekte auswählen** wählen Sie im Dialogfeld die **hinzufügen** Registerkarte, und wählen Sie die Ansicht, die Sie gerade erstellt haben.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Klicken Sie auf **Fertig stellen**.

Im Designer angezeigt, dass das Tool erstellt eine `vInstructorName` Entität und eine neue Zuordnung zwischen der `Department` und `Person` Entitäten.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> In der **Ausgabe** und **Fehlerliste** Windows, die Sie möglicherweise eine Warnmeldung angezeigt, die Sie darüber informiert, dass das Tool automatisch, einen primären erstellt Schlüssel für das neue `vInstructorName` anzeigen. Dabei handelt es sich um ein erwartetes Verhalten.


Bei Verweisen auf die neue `vInstructorName` Entität im Code nicht als Präfix voranstellen lower-case "V", die Datenbank-Konvention verwenden möchten. Aus diesem Grund, benennen Sie die Entität und einer Entität im Modell festzulegen.

Öffnen der **Model Browser**. Sie finden Sie unter `vInstructorName` als Entitätstyp und einer Ansicht aufgeführt.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

Klicken Sie unter **"SchoolModel"** (nicht **SchoolModel.Store**), mit der rechten Maustaste **vInstructorName** , und wählen Sie **Eigenschaften**. In der **Eigenschaften** Ändern der **Namen** Eigenschaft auf "InstructorName", und ändern Sie die **Entitätenmengenname** Eigenschaft auf "InstructorNames".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Speichern Sie und schließen Sie das Datenmodell, und klicken Sie dann erstellen Sie das Projekt neu.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Unter Verwendung einer Repository-Klasse und ein ObjectDataSource-Steuerelement

Erstellen Sie eine neue Klassendatei in der *DAL* Ordner, nennen Sie sie *SchoolRepository.cs*, und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Dieser Code stellt ein einzelnes `GetDepartments` Methode, die alle Entitäten in Objekte der `Departments` Entitätenmenge. Da Sie wissen, dass der Zugriff auf die wird die `Person` Navigationseigenschaft für jede Zeile zurückgegeben, Sie geben mit eager für diese Eigenschaft mithilfe der `Include` Methode. Die Klasse implementiert auch die `IDisposable` Schnittstelle, um sicherzustellen, dass die Verbindung mit der Datenbank veröffentlicht wird, wenn das Objekt verworfen wird.

> [!NOTE]
> Üblicherweise wird zum Erstellen einer repositoryklasse für jeden Entitätstyp. In diesem Tutorial wird eine repositoryklasse für mehrere Entitätstypen verwendet werden. Weitere Informationen über das Repositorymuster finden Sie unter die Beiträge in [Blogbeitrag des Entity Framework-Teams](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) und [Julie lermans Blog](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


Die `GetDepartments` Methode gibt ein `IEnumerable` Objekt anstelle einer `IQueryable` Objekt, um sicherzustellen, dass die zurückgegebene Auflistung verwendet werden kann, auch wenn das Repositoryobjekt selbst verworfen wird. Ein `IQueryable` Objekt kann dazu führen, dass Zugriff auf die Datenbank immer darauf zugegriffen wird, aber das Repositoryobjekt mit der Zeit, die einem datengebundenen Steuerelement zum Rendern der Daten versucht verworfen werden kann. Kann eine andere Auflistungstyp zurückgegeben, wie z. B. eine `IList` Objekt, sondern mit einem `IEnumerable` Objekt. Zurückgeben von jedoch eine `IEnumerable` Objekt wird sichergestellt, dass Sie typische Liste für schreibgeschützten Zugriff Verarbeitungsaufgaben wie z. B. ausführen können `foreach` Schleifen und LINQ-Abfragen, aber Sie können nicht zum Hinzufügen oder Entfernen von Elementen in der Auflistung, die impliziert, dass solche Änderungen ist in der Datenbank gespeichert.

Erstellen Sie eine *Departments.aspx* Seite, die verwendet die *Site.Master* Masterseite, und fügen Sie das folgende Markup in der `Content` Steuerelement mit dem Namen `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Dieses Markup erstellt eine `ObjectDataSource` -Steuerelement, das die "Repository"-Klasse wird verwendet, Sie gerade erstellt haben, und ein `GridView` Steuerelement zum Anzeigen der Daten. Die `GridView` Steuerelement angegeben **bearbeiten** und **löschen** Code zur Unterstützung dieser noch nicht über Befehle, aber Sie hinzugefügt.

Verwenden Sie mehrere Spalten `DynamicField` Steuerelemente, sodass Sie automatische Formatierung und Validierung Funktionen nutzen können. Für diese funktioniert, müssen zum Aufrufen der `EnableDynamicData` -Methode in der die `Page_Init` -Ereignishandler. (`DynamicControl` Steuerelemente werden nicht verwendet, der `Administrator` Feld, da sie mit Navigationseigenschaften funktionieren nicht.)

Die `Vertical-Align="Top"` Attribute werden wichtige später noch Mal, wenn Sie eine Spalte hinzufügen, die eine geschachtelte `GridView` Steuerelement zum Raster.

Öffnen der *Departments.aspx.cs* Datei, und fügen Sie die folgenden `using` Anweisung:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Fügen Sie dann den folgenden Ereignishandler für der Seite des `Init` Ereignis:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

In der *DAL* Ordner, erstellen Sie eine neue Klassendatei mit dem Namen *Department.cs* , und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Dieser Code fügt die Metadaten des Datenmodells. Es gibt an, dass die `Budget` Eigenschaft der `Department` Entität tatsächlich Währung darstellt, auch wenn der Datentyp ist `Decimal`, und gibt an, dass der Wert muss zwischen 0 und $1,000,000.00 liegen. Außerdem wird angegeben, die die `StartDate` Eigenschaft als Datum im Format mm/tt/jjjj formatiert werden sollen.

Führen Sie die *Departments.aspx* Seite.

[![Image01 abgerufen wird](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Beachten Sie, dass, obwohl Sie keine Formatzeichenfolge in angegeben haben die *Departments.aspx* Markup der Seite für die **Budget** oder **Startdatum** Spalten, Standard, währungs- und Formatierung auf angewendet wurde, indem sie die `DynamicField` gesteuert wird, mithilfe der Metadaten, die Sie, in angegeben der *Department.cs* Datei.

## <a name="adding-insert-and-delete-functionality"></a>Hinzufügen von INSERT- und Delete-Funktion

Open *SchoolRepository.cs*, fügen Sie den folgenden Code zum Erstellen einer `Insert` Methode und eine `Delete` Methode. Der Code enthält auch eine Methode namens `GenerateDepartmentID` , berechnet den nächsten verfügbaren Datensatz Schlüsselwert für die Verwendung durch die `Insert` Methode. Dies ist erforderlich, da die Datenbank nicht, zum Berechnen der dies automatisch für konfiguriert ist die `Department` Tabelle.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Die Attach-Methode

Die `DeleteDepartment` Methodenaufrufe der `Attach` Methode, um den Link, der beibehalten wird in den Objektkontext Objektstatus-Manager zwischen der Entität im Speicher und die Datenbank wiederherzustellen. Zeile darstellt. Dies vorkommen muss, bevor die Methode ruft die `SaveChanges` Methode.

Der Begriff *Objektkontext* bezieht sich auf die Entity Framework-Klasse, die von abgeleitet ist die `ObjectContext` -Klasse, die Sie verwenden, um Ihre Entitätenmengen und Entitäten zugreifen. Im Code für dieses Projekt heißt die Klasse `SchoolEntities`, und eine Instanz davon wird stets der Name `context`. Des Objektkontexts *Objektstatus-Manager* ist eine abgeleitete Klasse die `ObjectStateManager` Klasse. Das Objekt wenden Sie sich an verwendet den Objekt zum Speichern von Entitätsobjekten und um zu verfolgen, ob jeweils mit einer entsprechenden Zeile der Tabelle oder die Zeilen in der Datenbank synchronisiert ist.

Wenn Sie eine Entität lesen, der Objektkontext speichert sie in der Objekt-Status-Manager und verfolgt, ob dieser Darstellung des Objekts mit der Datenbank synchronisiert ist. Wenn Sie einen Eigenschaftswert ändern, wird beispielsweise ein Flag festgelegt, um anzugeben, dass die Eigenschaft, die Sie geändert haben, nicht mehr mit der Datenbank synchronisiert ist. Klicken Sie dann beim Aufrufen der `SaveChanges` -Methode, den Objektkontext weiß, wie Sie in der Datenbank, da der Objekt-Status-Manager weiß, was sich zwischen den aktuellen Zustand der Entität und den Status der Datenbank wird.

Dieser Prozess in der Regel funktioniert nicht in einer Webanwendung, jedoch, da die Kontext-Objektinstanz, die eine Entität zusammen mit der alle Elemente in einen Objekt-Zustands-Manager liest verworfen wird, nachdem eine Seite gerendert wird. Die Kontext-Objektinstanz, die Änderungen zu übernehmen, muss, ist eine neue Ressourcengruppe, die für die Verarbeitung der postback instanziiert wird. Im Fall von der `DeleteDepartment` -Methode, die `ObjectDataSource` Steuerelement erstellt erneut die ursprüngliche Version der Entität für Sie aus der Werte im Ansichtszustand, aber dies neu erstellt `Department` Entität ist nicht vorhanden, in der Objekt-Status-Manager. Bei einem Aufruf der `DeleteObject` Methode für diese Entität neu erstellt, der Aufruf würde fehlschlagen, da der Objektkontext nicht weiß, ob die Entität mit der Datenbank synchronisiert ist. Aufruf der `Attach` -Methode richtet die gleiche nachverfolgung zwischen der neu erstellten Entität und die Werte erneut in der Datenbank, die ursprünglich automatisch durchgeführt wurde, wenn die Entität in einer früheren Instanz des Objektkontexts gelesen wurde.

Es gibt Situationen, wenn Sie möchten nicht den Objektkontext zum Nachverfolgen von Entitäten in der Objekt-Status-Manager, und Sie können die Flags, die verhindern, dass sie dies festlegen. Beispiele hierfür sind in späteren Tutorials dieser Reihe angezeigt.

### <a name="the-savechanges-method"></a>Die SaveChanges-Methode

Diese einfache repositoryklasse veranschaulicht grundlegende Prinzipien der Durchführung von CRUD-Vorgänge. In diesem Beispiel die `SaveChanges` Methode wird aufgerufen, unmittelbar nach jedem Update. In einer produktionsanwendung sollten Sie zum Aufrufen der `SaveChanges` Methode aus einer separaten Methode gerne Sie besser steuern zu können, wenn die Datenbank aktualisiert wird. (Am Ende der im nächsten Tutorial werden Sie einen Link zu einem Whitepaper, die das arbeitseinheitsmuster erläutert wird, wird ein Ansatz zum Koordinieren von ähnliche Updates finden.) Beachten Sie auch, dass im Beispiel die `DeleteDepartment` Methode enthält keinen Code für die Behandlung von nebenläufigkeitskonflikten führen; Code hierfür wird in einem späteren Tutorial dieser Reihe hinzugefügt werden.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Abrufen von "Instructor" auszuwählenden beim Einfügen

Benutzer müssen einen Administrator eine Liste der Dozenten in einer Dropdown-Liste Wählen Sie beim Erstellen von neuer Abteilungen können. Aus diesem Grund fügen Sie den folgenden Code *SchoolRepository.cs* erstellen Sie eine Methode zum Abrufen der Liste der Dozenten mithilfe der Ansicht, die Sie zuvor erstellt haben:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Erstellen einer Seite für das Einfügen von Abteilungen

Erstellen Sie eine *DepartmentsAdd.aspx* Seite, die verwendet die *Site.Master* Seite, und fügen Sie das folgende Markup in der `Content` Steuerelement mit dem Namen `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Dieses Markup erstellt zwei `ObjectDataSource` Steuerelemente, eine für das Einfügen von neuen `Department` Entitäten und eine zum Abrufen von "Instructor" Namen für die `DropDownList` -Steuerelement, das für die Auswahl von abteilungsadministratoren verwendet wird. Das Markup erstellt eine `DetailsView` Steuern für einen Handler für des Steuerelements die Eingabe neuer Abteilungen und es gibt an, `ItemInserting` Ereignis, damit Sie festlegen können, die `Administrator` Fremdschlüsselwert. Am Ende einer `ValidationSummary` -Steuerelement zum Anzeigen von Fehlermeldungen.

Open *DepartmentsAdd.aspx.cs* und fügen Sie die folgenden `using` Anweisung:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Fügen Sie die folgende Klassenvariable und die Methoden hinzu:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

Die `Page_Init` Methode ermöglicht das Dynamic Data-Funktionen. Der Handler für die `DropDownList` des Steuerelements `Init` Ereignis speichert einen Verweis auf das Steuerelement, und der Handler für die `DetailsView` des Steuerelements `Inserting` Ereignis verwendet diesen Verweis zum Abrufen der `PersonID` Wert des ausgewählten Dozenten und aktualisieren die `Administrator` Fremdschlüsseleigenschaft die `Department` Entität.

Führen Sie die Seite, fügen Sie für eine neue Abteilung hinzu, und klicken Sie dann auf die **einfügen** Link.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Geben Sie Werte für eine andere neue ein. Geben Sie eine Zahl größer als 1,000,000.00 in die **Budget** Feld und Tab zum nächsten Feld. Ein Sternchen im Feld angezeigt wird, und wenn Sie den Mauszeiger darüber bewegen, sehen Sie die Fehlermeldung, die Sie in den Metadaten für dieses Feld eingegeben haben.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Klicken Sie auf **einfügen**, und Sie sehen, dass die Fehlermeldung wird angezeigt, die von der `ValidationSummary` Steuerelement am unteren Rand der Seite.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Als Nächstes den Browser schließen und öffnen Sie die *Departments.aspx* Seite. Delete-Funktion zum Hinzufügen der *Departments.aspx* Seite durch Hinzufügen einer `DeleteMethod` -Attribut auf die `ObjectDataSource` -Steuerelement, und ein `DataKeyNames` -Attribut auf die `GridView` Steuerelement. Die öffnende Tags für diese Steuerelemente werden nun im folgende Beispiel aussehen:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Führen Sie die Seite.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Löschen Sie die Abteilung, die Sie hinzugefügt haben, bei der Ausführung der *DepartmentsAdd.aspx* Seite.

## <a name="adding-update-functionality"></a>Update-Funktionalität hinzufügen

Open *SchoolRepository.cs* und fügen Sie die folgenden `Update` Methode:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Beim Klicken auf **Update** in die *Departments.aspx* Seite die `ObjectDataSource` Steuerelement erstellt zwei `Department` Entitäten Übergabe an die `UpdateDepartment` Methode. Eine enthält die ursprünglichen Werte, die im Ansichtszustand gespeichert wurden, und die andere die neuen Werte, die eingegeben wurden die `GridView` Steuerelement. Der Code in die `UpdateDepartment` -Methode übergibt die `Department` Entität mit den ursprünglichen Werten, die die `Attach` Methode, um die nachverfolgung zwischen der Entität und die neuerungen in der Datenbank herzustellen. Dann der Code übergibt die `Department` Entität, die den neuen Werten, die `ApplyCurrentValues` Methode. Der Objektkontext vergleicht die alten und neuen Werte. Wenn Sie ein neuer Wert aus einem alten Wert unterscheidet, ändert den Objektkontext, den Wert der Eigenschaft an. Die `SaveChanges` Methode wird dann nur die geänderten Spalten in der Datenbank aktualisiert. (Jedoch, wenn die Update-Funktion für diese Entität einer gespeicherten Prozedur zugeordnet wurden, würde die gesamte Zeile aktualisiert werden unabhängig davon, welche Spalten geändert wurden.)

Öffnen der *Departments.aspx* Datei, und fügen Sie die folgenden Attribute, die `DepartmentsObjectDataSource` Steuerelement:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Dieser bewirkt, dass alte Werte im Ansichtszustand gespeichert, damit sie mit den neuen Werten im verglichen werden, können die `Update` Methode.
- `OldValuesParameterFormatString="orig{0}"`   
 Benachrichtigt das Steuerelement, das der Namen des ursprünglichen Werteparameter ist, dies `origDepartment` .

Das Markup für das Starttag der `ObjectDataSource` Steuerelement wird jetzt im folgende Beispiel ähnelt:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Hinzufügen einer `OnRowUpdating="DepartmentsGridView_RowUpdating"` -Attribut auf die `GridView` Steuerelement. Verwenden Sie diese zum Festlegen der `Administrator` Eigenschaftswert basierend auf der Zeile, die der Benutzer, die in einer Dropdownliste auswählt. Die `GridView` öffnungstag jetzt etwa wie folgt:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Hinzufügen einer `EditItemTemplate` Steuerelement, für die `Administrator` Spalte der `GridView` zu steuern, unmittelbar nach dem die `ItemTemplate` Steuerelement für diese Spalte:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Dies `EditItemTemplate` -Steuerelement ähnelt der `InsertItemTemplate` steuern, der *DepartmentsAdd.aspx* Seite. Der Unterschied besteht darin, dass der Anfangswert des Steuerelements festgelegt ist, mit der `SelectedValue` Attribut.

Vor der `GridView` Steuerelement, fügen einen `ValidationSummary` steuern, wie in der *DepartmentsAdd.aspx* Seite.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Open *Departments.aspx.cs* und fügen Sie unmittelbar nach der Deklaration der partiellen Klasse den folgenden Code zum Erstellen eines privaten Feldes zur verweisen die `DropDownList` Steuerelement:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Hinzufügen von Ereignishandlern für die `DropDownList` des Steuerelements `Init` Ereignis und die `GridView` des Steuerelements `RowUpdating` Ereignis:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

Der Handler für die `Init` Ereignis speichert einen Verweis auf die `DropDownList` -Steuerelement in das Klassenfeld. Der Handler für die `RowUpdating` Ereignis des Verweises verwendet, rufen Sie den Wert des eingegebene Benutzers aus, und sie zum Anwenden der `Administrator` Eigenschaft der `Department` Entität.

Verwenden der *DepartmentsAdd.aspx* Seite, um eine neue Abteilung hinzuzufügen, und führen Sie die *Departments.aspx* Seite, und klicken Sie auf **bearbeiten** in der Zeile, die Sie hinzugefügt haben.

> [!NOTE]
> Sie werden nicht in der Lage sind, Bearbeiten von Zeilen, die Sie nicht hinzugefügt haben (d. h., die bereits in der Datenbank wurden), aufgrund ungültiger Daten in der Datenbank die Administratoren für die Zeilen, die mit der Datenbank erstellt wurden, sind Schüler/Studenten. Wenn Sie versuchen, eine davon bearbeiten, erhalten eine Fehlerseite Sie, die meldet einen Fehler wie `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Wenn Sie eine ungültige eingeben **Budget** Betrag ein, und klicken Sie dann auf **Update**, Sie sehen die gleichen Sternchen und eine Fehlermeldung angezeigt, die Sie haben, in gesehen der *Departments.aspx* Seite.

Wert eines Felds ändern, oder wählen Sie einen anderen Administrator, und klicken Sie auf **Update**. Die Änderung wird angezeigt.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Dies schließt die Einführung in die Verwendung der `ObjectDataSource` -Steuerelement für grundlegende CRUD (erstellen, lesen, aktualisieren und löschen) Vorgänge mit dem Entity Framework. Sie haben eine einfache Anwendung mit n-Ebenen erstellt, aber die Geschäftslogik-Ebene ist immer noch eng gekoppelt, auf die Datenzugriffsebene, die automatisierte Komponententests erschwert. Im folgenden Tutorial sehen Sie, wie implementiert das Repository-Muster, um Komponententests zu ermöglichen.

> [!div class="step-by-step"]
> [Nächste](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
