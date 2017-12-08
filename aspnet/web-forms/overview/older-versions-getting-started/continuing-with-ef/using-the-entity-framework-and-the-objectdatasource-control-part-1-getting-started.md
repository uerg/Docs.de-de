---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 'Verwenden das Entity Framework 4.0 und das ObjectDataSource-Steuerelement, Teil 1: Erste Schritte | Microsoft Docs'
author: tdykstra
description: Diese Reihe von Lernprogrammen baut auf der Contoso-University-Webanwendung, die von den ersten Schritten mit dem Lernprogramm Entity Framework-Reihe erstellt wird. Wenn Handelsversion...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 6f93d6033b68773507d624125936f0a69777e2b7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>Verwenden das Entity Framework 4.0 und das ObjectDataSource-Steuerelement, Teil 1: Erste Schritte
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

> Diese Reihe von Lernprogrammen in der Contoso-University Webanwendung durch die erstellte builds der [erste Schritte mit dem Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) Reihe von Lernprogrammen. Wenn Sie die frühere Lernprogramme nicht abgeschlossen wurde, als Ausgangspunkt für dieses Lernprogramm können Sie [Herunterladen der Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , die Sie erstellt haben würden. Sie können auch [Herunterladen der Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , die durch das vollständige Lernprogramm Reihe erstellt wird.
> 
> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Die beispielanwendung ist eine Website für eine fiktive Contoso-Universität. Es umfasst Funktionen wie Student Zulassung, Kurs Erstellung und Instructor-Zuweisungen.
> 
> Im Lernprogramm werden Beispiele für in c#. Die [herunterladbaren Beispiel](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) enthält Code in c# und Visual Basic.
> 
> ## <a name="database-first"></a>Zuerst Datenbank
> 
> Es gibt drei Möglichkeiten, die Sie mit Daten im Entity Framework arbeiten können: *Database First*, *Model First*, und *Code First*. Dieses Lernprogramm ist für Database First. Informationen zu den Unterschieden zwischen den Workflows und einer Anleitung zum Auswählen der besten für Ihr Szenario finden Sie unter [Entity Framework-Entwicklungsworkflows](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> Wie die Reihe Einstieg diese Reihe von Lernprogrammen verwendet die ASP.NET Web Forms-Modell und setzt voraus, dass Sie wissen, dass zum Arbeiten mit ASP.NET Web Forms in Visual Studio. Wenn dies nicht tun, werden dort [erste Schritte mit ASP.NET 4.5 Web Forms](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Wenn Sie mit ASP.NET MVC-Framework arbeiten lieber, finden Sie unter [erste Schritte mit dem Entity Framework mit ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="software-versions"></a>Versionen der Software
> 
> | **In diesem Lernprogramm gezeigt** | **Funktioniert auch mit** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express für Web. Das Lernprogramm wurde nicht mit späteren Versionen von Visual Studio getestet. Es gibt viele Unterschiede in Menüs, Dialogfelder und Vorlagen aus. |
> | .NET 4 | .NET 4.5 ist abwärtskompatibel mit .NET 4, aber im Lernprogramm wurde mit .NET 4.5 nicht getestet. |
> | Entity Framework 4 | Das Lernprogramm wurde mit höheren Versionen von Entity Framework nicht getestet. Ab Entity Framework 5, EF verwendet standardmäßig die `DbContext API` , EF 4.1 eingeführt wurde. EntityDataSource-Steuerelements wurde entworfen, um mithilfe der `ObjectContext` API. Informationen zur Verwendung von EntityDataSource steuern, mit der `DbContext` -API finden Sie unter [diesem Blogbeitrag](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx). |
> 
> ## <a name="questions"></a>Fragen
> 
> Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx), [Entity Framework und LINQ to Entities-Forum](https://social.msdn.microsoft.com/forums/en-US/adodotnetentityframework/threads/), oder [ StackOverflow.com](http://stackoverflow.com/).


Die `EntityDataSource` Steuerelement können Sie eine Anwendung sehr schnell zu erstellen, aber in der Regel müssen Sie eine beträchtliche Menge an von Geschäftslogik und Datenzugriffslogik in behalten Ihre *aspx* Seiten. Wenn Sie erwarten, die Anwendung dass an Komplexität zunehmen und laufende Wartung erforderlich ist, können Sie weitere Entwicklungszeit vorab investieren, um das Erstellen einer *n-Tier-* oder *überlappende* Anwendungsstruktur Das ist sind besser verwaltbar. Um diese Architektur zu implementieren, trennen Sie die Darstellungsschicht von Logik der Geschäftsebene (BLL) und die Datenzugriffsebene (DAL). Eine Möglichkeit, implementieren diese Struktur ist die Verwendung der `ObjectDataSource` anstelle von steuern die `EntityDataSource` Steuerelement. Bei Verwendung der `ObjectDataSource` -Steuerelement, implementieren Sie einen eigene Datenzugriffs-Code und anschließend in Aufrufen *aspx* Seiten, die ein Steuerelement, das viele der gleichen wurde mit Funktionen wie bei anderen Datenquellen-Steuerelementen. Dadurch können Sie die Vorteile einer n-Tier-Ansatz mit den Vorteilen der Verwendung einer Web Forms-Steuerelement für den Datenzugriff zu kombinieren.

Die `ObjectDataSource` Steuerelement können Sie flexibler auf andere Weise als auch. Da Sie Datenzugriff Code schreiben, es ist einfacher, mehr als nur lesen, einfügen, aktualisieren oder Löschen einen bestimmten Typ, dem sind die Aufgaben, die die `EntityDataSource` Steuerelement ausführen soll. Sie können z. B. Führen Sie die Protokollierung wird jedes Mal, wenn eine Entität aktualisiert wird, Daten archivieren, wenn eine Entität gelöscht wird oder automatisch Check- und Update-Daten bezogene nach Bedarf, wenn Sie eine Zeile mit einem Fremdschlüsselwert einfügen.

## <a name="business-logic-and-repository-classes"></a>Geschäftslogik und Repositoryklassen

Ein `ObjectDataSource` -Steuerelement funktioniert durch den Aufruf einer Klasse, die Sie erstellen. Die Klasse enthält Methoden, die Daten abgerufen und aktualisiert, und Sie die Namen dieser Methoden zum Bereitstellen der `ObjectDataSource` Steuerelement im Markup. Beim Rendern oder postback Verarbeitung der `ObjectDataSource` Ruft die Methoden, die Sie angegeben haben.

Neben grundlegende CRUD-Vorgänge, die Klasse, die Sie erstellen, für die Verwendung mit der `ObjectDataSource` Steuerelement zum Ausführen der Geschäftslogik müssen möglicherweise bei der `ObjectDataSource` liest Daten oder darin aktualisiert. Wenn Sie eine Abteilung aktualisieren, müssen Sie überprüfen, dass keine anderen Abteilungen derselben Administrator verfügen, da eine Person Administrator der mehr als eine Abteilung sein darf.

In einigen `ObjectDataSource` Dokumentation, z. B. die [ObjectDataSource-Klassenübersicht](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.objectdatasource.aspx), ruft das Steuerelement eine Klasse, die als bezeichnet eine *Geschäftsobjekt* , Geschäftslogik und Datenzugriffslogik enthält . In diesem Lernprogramm erstellen Sie separate Klassen für Geschäftslogik und Datenzugriffslogik. Die Klasse, die Datenzugriffslogik kapselt heißt ein *Repository*. Die Business Logic-Klasse enthält Methoden Geschäftslogik und Datenzugriff Methoden, aber die Datenzugriffs-Methoden aufrufen, das Repository, sodass die Datenzugriffs-Aufgaben ausführen.

Erstellen Sie auch eine Abstraktionsebene zwischen Ihrem BLL und der DAL, die die automatisierte Komponententests erleichtert die BLL testen. Diese Abstraktionsebene wird durch Erstellen einer Schnittstelle und die Benutzeroberfläche verwenden, wenn Sie das Repository in der Geschäftslogik Klasse instanziieren implementiert. Dies ermöglicht es Ihnen, die Geschäftslogik Klasse mit einem Verweis auf ein Objekt zu senden, der Repository-Schnittstelle implementiert. Für den normalen Betrieb Geben Sie ein Repository-Objekt, das mit dem Entity Framework zusammenarbeitet. Zum Testen, geben Sie ein Repository-Objekt, das zusammen mit den Daten gespeichert, mit denen Sie problemlos ändern können, z. B. Klassenvariablen als Sammlungen definiert.

Die folgende Abbildung zeigt den Unterschied zwischen einer Geschäftslogik-Klasse, die Datenzugriffslogik ohne ein Repository enthält und die andere ein Repository verwendet.

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

Sie können jetzt durch das Erstellen von Webseiten in der die `ObjectDataSource` -Steuerelement direkt in einem Repository gebunden ist, da es nur grundlegende Datenzugriffs-Aufgaben ausgeführt werden. Im nächsten Lernprogramm erstellen Sie eine Business Logic-Klasse mit der Validierungslogik und Binden der `ObjectDataSource` Steuerelement für diese Klasse anstelle von der Repository-Klasse. Außerdem erstellen Sie Komponententests für die Validierungslogik. Fügen Sie in das dritte Lernprogramm dieser Reihe Sortier- und Filterfunktionen zur Anwendung.

Die Seiten, die Sie in diesem Lernprogramm erstellen, arbeiten mit der `Departments` Entitätenmenge des Datenmodells, die Sie in erstellt die [Getting Started Tutorial Reihe](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md).

[![Image01 abgerufen wird](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>Aktualisieren der Datenbank und des Datenmodells

Sie werden in diesem Lernprogramm beginnen, durch zwei Änderungen an der Datenbank, die beides erforderlich, dass entsprechende Änderungen an das Datenmodell, das Sie in erstellt die [erste Schritte mit dem Entity Framework und Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) Lernprogramme. In einem diese Lernprogramme wurden Sie Änderungen an, in den Designer manuell in das Datenmodell mit der Datenbank nach dem Ändern einer Datenbank zu synchronisieren. In diesem Lernprogramm verwenden Sie des Designers **Aktualisieren von Modelldatenbank** Tool, um das Datenmodell automatisch zu aktualisieren.

### <a name="adding-a-relationship-to-the-database"></a>Hinzufügen einer Beziehung mit der Datenbank

Öffnen Sie in Visual Studio die Contoso University-Webanwendung, die Sie erstellt, in haben der [erste Schritte mit dem Entity Framework und Web Forms](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) Reihe von Lernprogrammen, und öffnen Sie dann die `SchoolDiagram` Datenbankdiagramm.

Bei Betrachtung der `Department` Tabelle im Datenbankdiagramm, sehen Sie, dass es wurde eine `Administrator` Spalte. Diese Spalte ist ein Fremdschlüssel für die `Person` Tabelle, aber keine Fremdschlüssel-Beziehung in der Datenbank definiert ist. Sie müssen die Beziehung zu erstellen und Aktualisieren des Datenmodells, damit Entity Framework automatisch diese Beziehung behandeln können.

Das Datenbankdiagramm mit der Maustaste die `Department` Tabelle, und wählen Sie **Beziehungen**.

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

In der **Foreign Key Relationships** auf **hinzufügen**, klicken Sie dann auf die Schaltfläche für **Tabellen- und Spaltenspezifikation**.

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

In der **Tabellen und Spalten** Dialogfeld Feld, legen Sie die Primärschlüsseltabelle und Datumsfeld in `Person` und `PersonID`, und legen Sie die Fremdschlüsseltabelle, und das Feld auf `Department` und `Administrator`. (Wenn Sie dies tun, ändert sich der Name der Beziehung aus `FK_Department_Department` auf `FK_Department_Person`.)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

Klicken Sie auf **OK** in der **Tabellen und Spalten** auf **schließen** in der **Foreign Key Relationships** ein, und speichern Sie die Änderungen. Wenn Sie gefragt werden, ob Sie speichern möchten die `Person` und `Department` Tabellen, klicken Sie auf **Ja**.

> [!NOTE]
> Wenn Sie gelöscht haben `Person` Zeilen, die Daten entsprechen, die bereits in der `Administrator` Spalte, Sie ist nicht möglich, diese Änderung zu speichern. In diesem Fall verwenden Sie die Tabellen-Editor in **Server-Explorer** sicherstellen, dass die `Administrator` Wert in jeder `Department` Zeile enthält die ID eines Datensatzes, der im tatsächlich vorhanden ist die `Person` Tabelle.
> 
> Nachdem Sie die Änderung zu speichern, Sie ist nicht möglich, löschen Sie eine Zeile aus der `Person` Tabelle, wenn diese Person ein Abteilung-Administrator ist. In einer produktionsanwendung würde Sie eine entsprechende Fehlermeldung bereitstellen, wenn eine Datenbank-Einschränkung wird verhindert, einen Löschvorgang dass, oder Sie eine Löschweitergabe geben. Ein Beispiel zum Angeben einer Löschweitergabe, finden Sie unter [das Entity Framework und ASP.NET – erste Schritte Teil 2](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md).


### <a name="adding-a-view-to-the-database"></a>Hinzufügen einer Ansicht mit der Datenbank

In der neuen *Departments.aspx* Seite, die Sie erstellen, Dropdownlisten Trainern mit Namen im Format "zuerst letzten" bieten, sodass Benutzer, Abteilung Administratoren auswählen können werden sollen. Um dies zu vereinfachen, erstellen Sie eine Ansicht in der Datenbank. Die Sicht wird nur die Daten, die von der Dropdown Liste benötigt bestehen: der vollständige Name (ordnungsgemäß formatiert) und Datensatzschlüssel.

In **Server-Explorer**, erweitern Sie *School.mdf*, mit der rechten Maustaste die **Ansichten** Ordner, und wählen **neue Sicht hinzufügen**.

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

Klicken Sie auf **schließen** bei der **Tabelle hinzufügen** Dialogfeld wird angezeigt, und fügen Sie die folgende SQL-Anweisung in der SQL-Bereich:

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

Speichern Sie die Ansicht als `vInstructorName`.

### <a name="updating-the-data-model"></a>Aktualisieren das Datenmodell

In der *DAL* Ordner die *SchoolModel.edmx* Datei, mit der rechten Maustaste in der Entwurfsoberfläche, und wählen **Modell aus der Datenbank aktualisieren**.

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

In der **Datenbankobjekte auswählen** wählen Sie im Dialogfeld die **hinzufügen** Registerkarte, und wählen Sie die Ansicht, die Sie gerade erstellt haben.

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

Klicken Sie auf **Fertig stellen**.

Im Designer angezeigt, dass das Tool erstellt eine `vInstructorName` Entität und eine neue Zuordnung zwischen der `Department` und `Person` Entitäten.

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> In der **Ausgabe** und **Fehlerliste** Windows möglicherweise eine Warnung darüber informiert, dass das Tool automatisch, einen primären erstellt Schlüssel für die neue `vInstructorName` anzeigen. Dabei handelt es sich um ein erwartetes Verhalten.


Bei Verweisen auf die neue `vInstructorName` Entität im Code, nicht die Datenbank-Konvention vom voranstellen von eines Kleinbuchstaben "V" darauf verwenden möchten. Aus diesem Grund werden Sie die Entität und zur Entitätssammlung im Modell umbenannt.

Öffnen der **Model-Browser**. Sie finden Sie unter `vInstructorName` als Entitätstyp und einer Ansicht aufgeführt.

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

Klicken Sie unter **SchoolModel** (nicht **SchoolModel.Store**), mit der rechten Maustaste **vInstructorName** , und wählen Sie **Eigenschaften**. In der **Eigenschaften** Ändern der **Namen** -Eigenschaft auf "InstructorName", und Ändern der **Entitätsname festgelegt** -Eigenschaft auf "InstructorNames".

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

Speichern und schließen Sie das Datenmodell, und klicken Sie dann das Projekt neu.

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>Verwenden eine Repository-Klasse und einem ObjectDataSource-Steuerelement

Erstellen Sie eine neue Klassendatei in der *DAL* Ordner, nennen Sie sie *SchoolRepository.cs*, und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

Dieser Code stellt eine einzelne `GetDepartments` Methode, die alle Entitäten in Objekte die `Departments` Entitätenmenge. Da Sie wissen, dass der Zugriff auf die wird die `Person` Navigationseigenschaft für jede Zeile zurückgegeben, Sie geben unverzüglichem Laden von für diese Eigenschaft mit dem `Include` Methode. Die Klasse implementiert auch die `IDisposable` Schnittstelle, um sicherzustellen, dass die Verbindung mit der Datenbank freigegeben wird, wenn das Objekt freigegeben wurde.

> [!NOTE]
> Üblicherweise wird zum Erstellen einer Klasse Repository für jeden Entitätstyp. In diesem Lernprogramm wird eine Repository-Klasse für mehrere Entitätstypen verwendet. Weitere Informationen über das Repositorymuster finden Sie unter den Beiträgen in [das Entity Framework-Team-Blog](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx) und [Julie Lermans Blog](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/).


Die `GetDepartments` Methode gibt ein `IEnumerable` Objekt anstelle einer `IQueryable` Objekt, um sicherzustellen, dass die zurückgegebene Auflistung verwendet werden, selbst wenn das Repositoryobjekt selbst freigegeben wurde. Ein `IQueryable` Objekt kann dazu führen, dass Zugriff auf die Datenbank bei jedem Zugriff darauf ist möglich, aber das Repositoryobjekt möglicherweise nach der Zeit, die einem datengebundenen Steuerelement zum Rendern der Daten versucht verworfen werden. Sie können einen anderen Sammlungstyp wie zurückgeben ein `IList` -Objekt anstelle von ein `IEnumerable` Objekt. Allerdings Zurückgeben einer `IEnumerable` Objekt wird sichergestellt, wie z. B. typischen schreibgeschützten Liste Verarbeitungsaufgaben ausführen können `foreach` Schleifen und LINQ-Abfragen, aber Sie können nicht zum Hinzufügen oder Entfernen von Elementen in der Auflistung, die impliziert, dass solche Änderungen ist in der Datenbank gespeichert.

Erstellen einer *Departments.aspx* Seite, verwendet der *Site.Master* Masterseite, und fügen Sie das folgende Markup in der `Content` -Steuerelement namens `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

Dieses Markup erstellt einen `ObjectDataSource` Steuerelement, das die Repository-Klasse Sie gerade erstellt haben verwendet, und ein `GridView` Steuerelement zum Anzeigen der Daten. Die `GridView` Steuerelement gibt **bearbeiten** und **löschen** Befehle, aber Sie noch nicht Code hinzugefügt, um diese noch zu unterstützen.

Verwenden Sie mehrere Spalten `DynamicField` Steuerelemente, sodass Sie automatische Formatierung und Validierung Funktionen nutzen können. Für diese arbeiten, müssen Sie anrufen der `EnableDynamicData` Methode in der `Page_Init` -Ereignishandler. (`DynamicControl` -Steuerelementen wird nicht der `Administrator` -Feld verwendet wird, weil sie nicht mit Navigationseigenschaften unterstützt.)

Die `Vertical-Align="Top"` Attribute werden wichtige später, wenn Sie eine Spalte hinzufügen, die eine geschachtelte hat `GridView` Steuerelement in den Entwurfsbereich.

Öffnen der *Departments.aspx.cs* Datei, und fügen Sie die folgenden `using` Anweisung:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

Fügen Sie dann den folgenden Ereignishandler für der Seite `Init` Ereignis:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

In der *DAL* Ordner, erstellen Sie eine neue Klassendatei mit dem Namen *Department.cs* und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

Dieser Code fügt die Metadaten des Datenmodells. Gibt an, dass die `Budget` Eigenschaft von der `Department` Entität tatsächlich Währung darstellt, obwohl die Angabe des Datentyps ist `Decimal`, und gibt an, dass der Wert muss zwischen 0 und $1,000,000.00 liegen. Es gibt auch an, die die `StartDate` Eigenschaft als Datum im Format mm/tt/jjjj formatiert werden sollen.

Führen Sie die *Departments.aspx* Seite.

[![Image01 abgerufen wird](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

Beachten Sie, dass, obwohl Sie nicht in eine Formatzeichenfolge angegeben haben die *Departments.aspx* Seitenmarkup für die **Budget** oder **Startdatum** Spalten, Standard, Datum und Währung Formatierung verwendet wurde, durch die `DynamicField` Steuerelemente, die mithilfe der Metadaten, die Sie, in angegeben der *Department.cs* Datei.

## <a name="adding-insert-and-delete-functionality"></a>Hinzufügen von INSERT- und Delete-Funktionen

Open *SchoolRepository.cs*, fügen Sie den folgenden Code zum Erstellen einer `Insert` Methode und eine `Delete` Methode. Der Code umfasst auch eine Methode namens `GenerateDepartmentID` , berechnet den nächsten verfügbaren Datensatz Schlüsselwert für die Verwendung durch die `Insert` Methode. Dies ist erforderlich, da die Datenbank nicht, zum Berechnen der dies automatisch für konfiguriert ist die `Department` Tabelle.

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Die Attach-Methode

Die `DeleteDepartment` Methodenaufrufe der `Attach` Methode, um den Link, der beibehalten wird in den Objektkontext Objektstatus-Manager zwischen der Entität im Arbeitsspeicher und die Datenbank wiederherstellen Zeile darstellt. Dies vorkommen muss, bevor die Methode ruft die `SaveChanges` Methode.

Der Begriff *Objektkontext* bezieht sich auf die Entity Framework-Klasse, abgeleitet wird, die `ObjectContext` -Klasse, die Sie verwenden, um Ihre Entitätenmengen und Entitäten zugreifen. Im Code für dieses Projekt ist die Klasse heißt `SchoolEntities`, und eine Instanz wird immer mit dem Namen `context`. Der Objektkontext *Objektstatus-Manager* ist eine Klasse, abgeleitet wird, die `ObjectStateManager` Klasse. Der Objekt-Kontakt verwendet den Objektstatus-Manager zum Speichern von Entitätsobjekten und zum Nachverfolgen, ob jeweils mit der entsprechenden Zeile der Tabelle oder Zeilen in der Datenbank synchronisiert ist.

Wenn Sie eine Entität lesen, Objektkontext speichert diesen in den Objektstatus-Manager und verfolgt, ob diese Darstellung des Objekts mit dem die Datenbank synchronisiert wurde. Wenn Sie einen Eigenschaftswert ändern, wird z. B. ein Flag festgelegt, um anzugeben, dass die Eigenschaft, die Sie geändert haben, nicht mehr mit der Datenbank synchronisiert ist. Klicken Sie dann beim Aufrufen der `SaveChanges` -Methode, den Objektkontext weiß, was in der Datenbank zu tun, denn der Objektstatus-Manager weiß, was unterschieden zwischen den aktuellen Zustand der Entität und den Status der Datenbank wird.

Dieser Prozess in der Regel funktioniert nicht in einer Web-Anwendung jedoch, da die Kontext-Objektinstanz, die eine Entität gehören, sowie alle Elemente in der Objekt-Status-Manager liest verworfen wird, nachdem eine Seite gerendert wird. Die Kontext-Objektinstanz, die Änderungen zu übernehmen, muss ist ein neues Konto, das für Postbacks Verarbeitung instanziiert wird. Im Fall von der `DeleteDepartment` -Methode, die `ObjectDataSource` Steuerelement erstellt erneut die ursprüngliche Version der Entität für Sie aus Werten im Ansichtszustand Angaben, aber dies neu erstellt `Department` Entität in den Objektstatus-Manager nicht vorhanden. Bei einem Aufruf der `DeleteObject` Methode für diese Entität neu erstellt, der Aufruf fehl, weil der Objektkontext nicht weiß, ob die Entität mit der Datenbank synchronisiert ist. Bei Aufruf der `Attach` Methode stellt die gleiche Überwachung zwischen der neu erstellte Entität und die Werte in der Datenbank, die ursprünglich automatisch durchgeführt wurde, wenn die Entität in einer früheren Instanz, von dem Objektkontext gelesen wurde.

Es gibt Situationen, wenn Sie möchten schließlich nicht den Objektkontext zum Nachverfolgen von Entitäten in den Objektstatus-Manager, und Sie können Flags, um zu verhindern, dass auf diese Weise festlegen. Beispiele hierfür sind in späteren Lernprogrammen dieser Reihe angezeigt.

### <a name="the-savechanges-method"></a>Die SaveChanges-Methode

Diese Klasse einfaches Repository veranschaulicht grundlegende Prinzipien der zum Ausführen von CRUD-Vorgänge. In diesem Beispiel wird die `SaveChanges` Methode wird aufgerufen, unmittelbar nach jeder Aktualisierung. In einer produktionsanwendung kann aufgerufen werden soll die `SaveChanges` Methode über eine separate Methode bieten mehr Kontrolle über, wenn die Datenbank aktualisiert wird. (Am Ende des nächsten Lernprogramms wird einen Link zur ein Whitepaper, die die Einheit der Arbeit Muster erläutert wird, also ein Ansatz zum Koordinieren der entsprechenden Updates suchen.) Beachten Sie auch, dass im Beispiel die `DeleteDepartment` Methode keinen Code Parallelitätskonflikte; Code dazu wird in einem späteren Lernprogramm dieser Reihe hinzugefügt werden.

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>Abrufen von Instructor-Namen auswählen, wenn einfügen

Benutzer muss einen Administrator aus einer Liste von Kursleiter eine Dropdown-Liste auswählen, für die Erstellung neue Abteilungen. Deshalb fügen Sie folgenden Code zum *SchoolRepository.cs* zum Erstellen einer Methode zum Abrufen der Liste der Lehrkräfte mithilfe der Ansicht, die Sie zuvor erstellt haben:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>Erstellen eine Seite zum Einfügen von Abteilungen

Erstellen einer *DepartmentsAdd.aspx* Seite, verwendet der *Site.Master* Seite, und fügen Sie das folgende Markup in der `Content` -Steuerelement namens `Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

Dieses Markup erstellt zwei `ObjectDataSource` Steuerelemente, die für das Einfügen von neuen `Department` Entitäten und eine zum Abrufen von Instructor-Namen für die `DropDownList` für die Auswahl der Abteilung Administratoren verwendeten Steuerelements ab. Das Markup erstellt einen `DetailsView` Steuern für einen Handler für des Steuerelements eingeben neuer Abteilungen, und es gibt an, `ItemInserting` Ereignis, damit Sie festlegen können, die `Administrator` Fremdschlüsselwert. Wird am Ende einer `ValidationSummary` -Steuerelement zum Anzeigen von Fehlermeldungen.

Open *DepartmentsAdd.aspx.cs* und fügen Sie die folgenden `using` Anweisung:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

Fügen Sie die folgenden Klassenvariable und Methoden:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

Die `Page_Init` Methode ermöglicht die Dynamic Data-Funktionen. Der Handler für die `DropDownList` des Steuerelements `Init` Ereignis speichert einen Verweis auf das Steuerelement und den Handler für die `DetailsView` des Steuerelements `Inserting` Ereignis verwendet diesen Verweis zum Abrufen der `PersonID` Wert des ausgewählten Kursleiter und aktualisieren die `Administrator` Fremdschlüsseleigenschaft von der `Department` Entität.

Führen Sie die Seite, Hinzufügen von Informationen für eine neue Abteilung, und klicken Sie dann auf die **einfügen** Link.

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

Geben Sie Werte für eine andere neue Abteilung ein. Geben Sie eine Zahl größer als 1,000,000.00 in der **Budget** Feld und Tab, um das nächste Feld. Ein Sternchen im Feld angezeigt wird, und wenn Sie den Mauszeiger darüber halten, sehen Sie die Fehlermeldung, die Sie in den Metadaten für das Feld eingegeben haben.

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

Klicken Sie auf **einfügen**, und Sie sehen die Fehlermeldung angezeigt, indem die `ValidationSummary` Steuerelement am unteren Rand der Seite.

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

Als Nächstes den Browser schließen und öffnen Sie die *Departments.aspx* Seite. Delete-Funktion zum Hinzufügen der *Departments.aspx* Seite durch Hinzufügen einer `DeleteMethod` -Attribut auf die `ObjectDataSource` -Steuerelement, und ein `DataKeyNames` -Attribut auf die `GridView` Steuerelement. Die öffnenden Tags für diese Steuerelemente werden jetzt im folgende Beispiel ähneln:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

Führen Sie die Seite.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

Löschen Sie die Abteilung, die Sie hinzugefügt haben, bei der Ausführung der *DepartmentsAdd.aspx* Seite.

## <a name="adding-update-functionality"></a>Hinzufügen einer Updatefunktion

Open *SchoolRepository.cs* und fügen Sie die folgenden `Update` Methode:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

Beim Klicken auf **Update** in der *Departments.aspx* Seite der `ObjectDataSource` Steuerelement erstellt zwei `Department` Entitäten Übergabe an die `UpdateDepartment` Methode. Eine enthält die ursprünglichen Werte, die im Ansichtszustand gespeichert wurden, und die andere die neuen Werte, die eingegeben wurden die `GridView` Steuerelement. Der Code in der `UpdateDepartment` -Methode übergibt die `Department` Entität mit den ursprünglichen Werten auf der `Attach` Methode gewahrt die Verfolgung zwischen der Entität und was in der Datenbank ist. Dann der Code übergibt die `Department` Entität mit den neuen Werten zu den `ApplyCurrentValues` Methode. Der Objektkontext vergleicht die alten und neuen Werte. Wenn Sie ein neuer Wert aus einer alten Wert unterscheidet, ändert der Objektkontext den Wert der Eigenschaft. Die `SaveChanges` Methode wird dann nur die geänderten Spalten in der Datenbank aktualisiert. (Jedoch, wenn die Update-Funktion für diese Entität an eine gespeicherte Prozedur zugeordnet wurden, würde die gesamte Zeile aktualisiert werden unabhängig davon, welche Spalten geändert wurden.)

Öffnen der *Departments.aspx* Datei, und fügen Sie die folgenden Attribute, die `DepartmentsObjectDataSource` Steuerelement:

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 Diese Ursachen für die alten Werte werden im Ansichtszustand gespeichert, damit sie mit den neuen Werten verglichen werden, können die `Update` Methode.
- `OldValuesParameterFormatString="orig{0}"`   
 Damit wird das Steuerelement, das der Namen des ursprünglichen Werte Parameters wird mitgeteilt `origDepartment` .

Das Markup für das Anfangstag des der `ObjectDataSource` -Steuerelement jetzt im folgende Beispiel ähnelt:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

Hinzufügen einer `OnRowUpdating="DepartmentsGridView_RowUpdating"` -Attribut auf die `GridView` Steuerelement. Verwenden Sie dies zum Festlegen der `Administrator` Eigenschaftswert auf Grundlage der Zeile, die der Benutzer, die in einer Dropdownliste auswählt. Die `GridView` öffnungstag jetzt etwa wie folgt:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

Hinzufügen einer `EditItemTemplate` für steuern die `Administrator` Spalte die `GridView` steuern, unmittelbar nach dem die `ItemTemplate` Steuerelement für diese Spalte:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

Dies `EditItemTemplate` Steuerelement ähnelt dem `InsertItemTemplate` steuern, der *DepartmentsAdd.aspx* Seite. Der Unterschied besteht darin, dass der Anfangswert des Steuerelements festgelegt ist, mit der `SelectedValue` Attribut.

Vor der `GridView` steuern, fügen Sie eine `ValidationSummary` steuern, wie in der *DepartmentsAdd.aspx* Seite.

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

Open *Departments.aspx.cs* und unmittelbar nach der Deklaration der partiellen Klasse fügen Sie den folgenden Code zum Erstellen eines privaten Felds zum Verweisen auf die `DropDownList` Steuerelement:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

Fügen Sie dann den Handler für das `DropDownList` des Steuerelements `Init` Ereignis und die `GridView` des Steuerelements `RowUpdating` Ereignis:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

Der Handler für das `Init` Ereignis speichert einen Verweis auf die `DropDownList` Steuerelement im Feld "Klasse". Der Handler für die `RowUpdating` Ereignis verwendet die Referenz, rufen Sie den Wert der Benutzer eingegeben hat, und wenden Sie es auf die `Administrator` Eigenschaft von der `Department` Entität.

Verwenden der *DepartmentsAdd.aspx* Seite, um eine neue Abteilung hinzuzufügen, und führen Sie die *Departments.aspx* Seite, und klicken Sie auf **bearbeiten** auf die Zeile, die Sie hinzugefügt haben.

> [!NOTE]
> Sie werden nicht in der Lage, Bearbeiten von Zeilen, die Sie nicht hinzugefügt haben (d. h., die bereits in der Datenbank wurden), aufgrund ungültiger Daten in der Datenbank; die Administratoren für die Zeilen, die mit der Datenbank erstellt wurden, sind Studenten. Wenn Sie versuchen, eine davon zu bearbeiten, erhalten eine Fehlerseite Sie, die einen Fehler wie meldet`'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

Bei der Eingabe eines ungültiges **Budget** Betrag, und klicken Sie dann auf **Update**, sehen die gleichen Sternchen und eine Fehlermeldung angezeigt, die Sie in gesehen haben die *Departments.aspx* Seite.

Ändern Sie einen Feldwert angibt, oder wählen Sie einen anderen Administrator, und klicken Sie auf **Update**. Die Änderung wird angezeigt.

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

Dies schließt die Einführung in die Verwendung der `ObjectDataSource` -Steuerelement für grundlegende CRUD (erstellen, lesen, aktualisieren und löschen) Vorgänge mit dem Entity Framework. Sie haben eine einfache n-Tier-Anwendung erstellt, aber die Geschäftslogik Ebene ist immer noch eng verbunden, der Datenzugriffs--Ebene, die automatisierte Komponententests etwas komplizierter macht. Im folgenden Lernprogramm sehen Sie zum Implementieren des Repositorymusters, um Komponententests zu ermöglichen.

>[!div class="step-by-step"]
[Nächste](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
