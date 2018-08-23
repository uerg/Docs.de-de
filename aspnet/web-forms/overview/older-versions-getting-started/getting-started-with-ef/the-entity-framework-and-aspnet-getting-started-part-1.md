---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010 erstellen...
ms.author: riande
ms.date: 12/03/2010
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: f9f100ccaf5e9cfdaf0633f9bfebbad273212a0e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837407"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Die beispielanwendung ist eine Website für die fiktive Contoso University. Sie enthält Funktionen wie die Zulassung von Studenten, die Erstellung von Kursen und Aufgaben von Dozenten.
> 
> Das Tutorial zeigt Beispiele in C# geschrieben. Die [herunterladbare Beispielversion](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) in c# und Visual Basic-Code enthält.
> 
> ## <a name="database-first"></a>Zunächst-Datenbank
> 
> Es gibt drei Möglichkeiten, die Sie mit Daten im Entity Framework arbeiten können: *Database First*, *Model First*, und *Code First*. Dieses Tutorial ist für Database First. Weitere Informationen zu den Unterschieden zwischen den Workflows und Anleitungen dazu, wie die beste Option für Ihr Szenario auswählen, finden Sie unter [Entity Framework-Entwicklungsworkflows](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> Dieser tutorialreihe wird der ASP.NET Web Forms-Modell verwendet und setzt voraus, dass Sie wissen, wie mit ASP.NET Web Forms in Visual Studio arbeiten. Wenn dies nicht tun, werden dort [erste Schritte mit ASP.NET 4.5 Web Forms](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Wenn Sie mit ASP.NET MVC-Frameworks arbeiten lieber, finden Sie unter [erste Schritte mit Entity Framework mit ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
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


## <a name="overview"></a>Übersicht

Die Anwendung, die Sie in diesen Tutorials erstellen, ist eine einfache University-Website.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Benutzer können Informationen zu den Studenten, Kursen und Dozenten abrufen. Nur einige anzeigen dargestellt, die Sie erstellen, sind unten aufgeführt.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![Image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![Image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![Image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Erstellen der Webanwendung

Um das Lernprogramm zu starten, öffnen Sie Visual Studio, und klicken Sie dann erstellen Sie ein neues ASP.NET-Webanwendungsprojekt mit der **ASP.NET-Webanwendung** Vorlage:

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Mit dieser Vorlage erstellt ein Webanwendungsprojekt, die bereits eines Stylesheets und Masterseiten enthält:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Öffnen der *Site.Master* Datei, und Ändern von "My ASP.NET Application" in "Contoso University".

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Suchen der *Menü* Steuerelement mit dem Namen `NavigationMenu` und Ersetzen Sie ihn durch Folgendes Markup, das Menüelemente für die Seiten hinzufügt, Sie erstellen.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Öffnen der *"default.aspx"* Seite, und ändern Sie die `Content` Steuerelement mit dem Namen `BodyContent` folgt:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Sie verfügen nun über eine einfache Startseite mit Links zu den verschiedenen Seiten, die Sie erstellen:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Erstellen der Datenbank

Für diese Tutorials verwenden Sie den Entity Framework Data Model-Designer automatisch das Datenmodell, das basierend auf einer vorhandenen Datenbank erstellen (wird häufig aufgerufen, die *Database First* Ansatz). Eine Alternative, die in diesem Tutorial nicht behandelt wird das Datenmodell manuell erstellen und dann den Designer generierten-Skripts, die die Datenbank zu erstellen ist (die *Model First* Ansatz).

Im nächste Schritt werden für die Database First-Methode, die in diesem Tutorial verwendeten eine Datenbank des Standorts hinzufügen. Die einfachste Möglichkeit ist, um zuerst das Projekt herunterzuladen, das in diesem Tutorial geht. Klicken Sie dann mit der rechten Maustaste die *App\_Daten* Ordner **vorhandenes Element hinzufügen**, und wählen Sie die *School.mdf* Datenbankdatei aus dem heruntergeladenen Projekt.

Eine Alternative ist, folgen Sie die Anweisungen unter [Erstellen der Beispieldatenbank "School"](https://msdn.microsoft.com/library/bb399731.aspx). Kopieren Sie, ob Sie laden Sie die Datenbank, oder erstellen Sie ihn, die *School.mdf* -Datei aus den folgenden Ordner der Anwendung das *App\_Daten* Ordner:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Der Pfad der *mdf* Datei geht davon aus, verwenden Sie SQL Server 2008 Express.)

Wenn Sie die Datenbank aus einem Skript erstellen, führen Sie die folgenden Schritte zum Erstellen eines neuen Datenbankdiagramms:

1. In **Server-Explorer**, erweitern Sie **Datenverbindungen**, erweitern Sie *School.mdf*, mit der rechten Maustaste **Datenbankdiagramme**, und wählen Sie **Neues Diagramm hinzufügen**.

    [![Image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Wählen Sie alle Tabellen aus, und klicken Sie dann auf **hinzufügen**.

    [![Image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server erstellt, ein Datenbankdiagramm, die Tabellen, Spalten in Tabellen und Beziehungen zwischen den Tabellen anzeigt. Sie können die Tabellen, etwa, um sie zu organisieren, beliebig verschieben.
3. Speichern Sie das Diagramm als "SchoolDiagram", und schließen Sie sie.

Wenn Sie Herunterladen der *School.mdf* -Datei, die in diesem Tutorial geht sehen Sie das Datenbankdiagramm durch Doppelklicken auf **SchoolDiagram** unter **Datenbankdiagramme** in **Server-Explorer**.

[![Image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Das Diagramm sieht etwa folgendermaßen aus (in den Tabellen möglicherweise an verschiedenen Standorten aus, was hier gezeigt wird):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Erstellen die Entity Framework-Datenmodells

Jetzt können Sie ein Entity Framework-Datenmodell aus dieser Datenbank erstellen. Sie können das Datenmodell im Stammordner der Anwendung erstellen, aber für dieses Tutorial müssen Sie es in einem Ordner namens platzieren *DAL* (für Data Access Layer).

In **Projektmappen-Explorer**, fügen Sie einen Projektordner mit dem Namen *DAL* (Stellen Sie sicher, dass es unter dem Projekt nicht unterhalb der Lösung).

Mit der rechten Maustaste die *DAL* Ordner, und wählen Sie dann **hinzufügen** und **neues Element**. Unter **installierte Vorlagen**Option **Daten**, wählen die **ADO.NET Entity Data Model** Vorlage, nennen Sie sie *SchoolModel.edmx*, und Klicken Sie dann auf **hinzufügen**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Assistent für Entity Data Model wird gestartet. In den ersten Schritt im Assistenten der **aus Datenbank generieren** Option ist standardmäßig aktiviert. Klicken Sie auf **Weiter**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

In der **wählen Sie Ihre Datenverbindung** Schritt, übernehmen Sie die Standardwerte, und klicken Sie auf **Weiter**. Die Datenbank "School" wird standardmäßig ausgewählt, und die verbindungseinstellung wird gespeichert, der *"Web.config"* als Datei **SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

In der **Datenbankobjekte auswählen** Assistentenschritt, Option alle Tabellen außer `sysdiagrams` (die für das Diagramm, das Sie zuvor generierten erstellt wurde), und klicken Sie dann auf **Fertig stellen**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Nachdem sie mit dem Erstellen des Modells abgeschlossen ist, wird von Visual Studio eine grafische Darstellung der Entity Framework-Objekte (Entitäten), die von Datenbanktabellen entsprechen. (Wie bei Datenbankdiagramm, der Speicherort der einzelnen Elemente möglicherweise unterscheiden, was Sie in dieser Abbildung angezeigt. Sie können die Elemente ziehen etwa, um in der Abbildung zu entsprechen, wenn Sie möchten.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Untersuchen die Entity Framework-Datenmodells

Sie können sehen, dass das Entitätsdiagramm sehr ähnlich wie das Datenbankdiagramm ein, mit wenigen unterschieden. Ein Unterschied ist das Hinzufügen von Symbolen am Ende jeder Zuordnung, die den Typ der Zuordnung angeben (tabellenbeziehungen werden Zuordnungen der Entität im Datenmodell genannt):

- Eine 1: 0 (null)-oder-1-Zuordnung wird durch "1" und "0.. 1" dargestellt.

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    In diesem Fall eine `Person` Entität möglicherweise oder kann nicht zugeordnet werden ein `OfficeAssignment` Entität. Ein `OfficeAssignment` Entität zugeordnet werden eine `Person` Entität. Das heißt, einem Dozenten möglicherweise oder möglicherweise ein Office nicht zugewiesen werden, und eine kann nur ein "Instructor" zugewiesen werden.
- Eine 1: n Zuordnung wird dargestellt, von "1" und "\*".

    [![Image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    In diesem Fall eine `Person` Entität kann oder möglicherweise nicht `StudentGrade` Entitäten. Ein `StudentGrade` Entität muss mit einem zugeordneten `Person` Entität. `StudentGrade` Entitäten darstellen tatsächlich registrierte Kurse in dieser Datenbank; Wenn ein Schüler/Student einen Kurs registriert ist und es keine noch ist die `Grade` -Eigenschaft ist null. Das heißt, Schüler/Student kann nicht in Kurse registriert werden jedoch noch, kann auf einen Kurs registriert werden oder kann in mehreren Kursen registriert sein. Jeder Jahrgangsstufe einer registrierten Kurs gilt für nur eine für Schüler und Studenten.
- Eine m: n Zuordnung wird dargestellt, indem Sie "\*"und"\*".

    [![Image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    In diesem Fall eine `Person` Entität kann oder möglicherweise nicht `Course` Entitäten und umgekehrt gilt auch: eine `Course` Entität kann oder möglicherweise nicht `Person` Entitäten. Das heißt, ein Dozenten möglicherweise mehrere Kurse unterrichten, und ein Kurs kann von mehreren Dozenten unterrichtet werden. (In dieser Datenbank, diese Beziehung gilt nur für Dozenten; er enthält keine Schüler/Studenten links, um Kurse. Schüler/Studenten sind, Kurse in der Tabelle StudentGrades verknüpft.)

Ein weiterer Unterschied zwischen der neuen Datenbankdiagramms und das Datenmodell wird die zusätzliche **Navigationseigenschaften** Abschnitt für jede Entität. Eine Navigationseigenschaft einer Entität verweist auf verknüpfte Entitäten. Z. B. die `Courses` -Eigenschaft in einem `Person` Entität enthält eine Auflistung aller der `Course` Entitäten, die im Zusammenhang mit, `Person` Entität.

[![Image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Noch ein weiterer Unterschied zwischen der Datenbank und zum Datenmodell das Fehlen von ist die `CourseInstructor` Zuordnungstabelle, in der Datenbank, zum Verknüpfen verwendet wird, der `Person` und `Course` Tabellen in einer m: n Beziehung. Die Navigationseigenschaften können Sie für eine Beziehung abrufen `Course` Entitäten aus der `Person` Entität und die zugehörigen `Person` Entitäten aus der `Course` Entität, daher keine Notwendigkeit besteht, die die Zuordnung im Datenmodell darstellen.

[![Image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Für dieses Lernprogramm nehmen Sie an der `FirstName` Spalte die `Person` Tabelle einer Person Vornamen und zweite Vorname enthält. Der Name des Felds, um dies widerzuspiegeln ändern möchten, aber der Datenbankadministrator (DBA) sollten nicht die Datenbank zu ändern. Sie können den Namen des Ändern der `FirstName` unverändert Eigenschaft im Datenmodell, ohne die Datenbank entspricht.

Klicken Sie im Designer mit der Maustaste **FirstName** in die `Person` Entität, und wählen Sie dann **umbenennen**.

[![Image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Geben Sie den neuen Namen "FirstMidName" an. Dies ändert sich die Möglichkeit, die Sie auf die Spalte im Code ohne Ändern der Datenbank verweisen.

[![Image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

Modellbrowser bietet es sich um eine weitere Möglichkeit, um die Struktur der Datenbank, die Datenstruktur für das Modell und die Zuordnung zwischen ihnen anzuzeigen. Um zu überprüfen, mit der rechten Maustaste in eines leeren Bereichs im Entitätsdesigner, und klicken Sie dann auf **Modellbrowser**.

[![Image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

Die **Modellbrowser** Bereich wird eine Strukturansicht angezeigt. (Die **Modellbrowser** Bereich angedockt werden kann, mit der **Projektmappen-Explorer** Bereich.) Die **"SchoolModel"** Knoten darstellt, der Struktur der Daten, und die **SchoolModel.Store** Knoten darstellt, die Struktur der Datenbank.

[![Image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Erweitern Sie **SchoolModel.Store** erweitern Sie zum Anzeigen der Tabellen **Tabellen / Sichten** finden in Tabellen, und erweitern dann **Kurs** der Spalten in einer Tabelle anzuzeigen.

[![Image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Erweitern Sie **"SchoolModel"**, erweitern Sie **Entitätstypen**, und erweitern Sie dann die **Kurs** Knoten aus, um die Entitäten und die Eigenschaften in den Entitäten anzeigen.

[![Image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

In einem Designer, der oder die **Modellbrowser** Bereich sehen Sie, wie das Entity Framework für die Objekte der beiden Modelle bezieht. Mit der rechten Maustaste die `Person` Entität, und wählen **Tabellenzuordnung**.

[![Image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Daraufhin wird die **Mappingdetails** Fenster. Beachten Sie, dass in diesem Fenster angezeigt, die können die Datenbankspalte `FirstName` zugeordnet ist `FirstMidName`, d.h., wie Sie es in das Datenmodell umbenannt.

[![Image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Das Entity Framework verwendet XML zum Speichern von Informationen über die Datenbank, das Datenmodell und die Zuordnungen zwischen ihnen. Die *SchoolModel.edmx* Datei ist tatsächlich eine XML-Datei, die diese Informationen enthält. Rendert der Designer die Informationen in einem grafischen Format, aber Sie können auch die Datei im XML-Format anzeigen, indem Sie mit der rechten Maustaste die *EDMX* Datei **Projektmappen-Explorer**, klicken Sie auf **Öffnen mit**, und wählen **XML (Text)-Editor**. (Data Model-Designer und einem XML-Editor sind nur zwei unterschiedliche Methoden zum Öffnen von und Arbeiten mit der gleichen Datei, daher nicht den Designer zu öffnen, und öffnen die Datei in einem XML-Editor, zur gleichen Zeit.)

Sie haben nun eine Website, eine Datenbank und einem Datenmodell erstellt. In der nächsten exemplarischen Vorgehensweise beginnen Sie mit Daten, die das Datenmodell mit ASP.NET `EntityDataSource` Steuerelement.

> [!div class="step-by-step"]
> [Nächste](the-entity-framework-and-aspnet-getting-started-part-2.md)
