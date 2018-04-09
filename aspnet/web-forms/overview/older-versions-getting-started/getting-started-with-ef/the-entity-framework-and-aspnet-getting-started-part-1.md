---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
title: Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 Web Forms | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010 erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 5cb00916-8f46-491f-be25-4739a615d619
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1
msc.type: authoredcontent
ms.openlocfilehash: ad504b02d801f9513787f9fde1a4d00d7b0afff0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms"></a>Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 Web Forms-
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Die beispielanwendung ist eine Website für eine fiktive Contoso-Universität. Sie enthält Funktionen wie die Zulassung von Studenten, die Erstellung von Kursen und Aufgaben von Dozenten.
> 
> Im Lernprogramm werden Beispiele für in c#. Die [herunterladbaren Beispiel](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) enthält Code in c# und Visual Basic.
> 
> ## <a name="database-first"></a>Zuerst Datenbank
> 
> Es gibt drei Möglichkeiten, die Sie mit Daten im Entity Framework arbeiten können: *Database First*, *Model First*, und *Code First*. Dieses Lernprogramm ist für Database First. Informationen zu den Unterschieden zwischen den Workflows und einer Anleitung zum Auswählen der besten für Ihr Szenario finden Sie unter [Entity Framework-Entwicklungsworkflows](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="web-forms"></a>Web Forms
> 
> Diese Reihe von Lernprogrammen verwendet die ASP.NET Web Forms-Modell und setzt voraus, dass Sie wissen, dass zum Arbeiten mit ASP.NET Web Forms in Visual Studio. Wenn dies nicht tun, werden dort [erste Schritte mit ASP.NET 4.5 Web Forms](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Wenn Sie mit ASP.NET MVC-Framework arbeiten lieber, finden Sie unter [erste Schritte mit dem Entity Framework mit ASP.NET MVC](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
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
> Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx), [Entity Framework und LINQ to Entities-Forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), oder [ StackOverflow.com](http://stackoverflow.com/).


## <a name="overview"></a>Übersicht

Die Anwendung, die Sie in diesen Lernprogrammen erstellen werden müssen ist eine einfache University-Website.

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image1.png)

Benutzer können Informationen zu den Studenten, Kursen und Dozenten abrufen. Die Bildschirme erstellen Sie nun ein paar sind unten dargestellt.

[![Image30](the-entity-framework-and-aspnet-getting-started-part-1/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image3.png)

[![image37](the-entity-framework-and-aspnet-getting-started-part-1/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image5.png)

[![image31](the-entity-framework-and-aspnet-getting-started-part-1/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image7.png)

[![image32](the-entity-framework-and-aspnet-getting-started-part-1/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image9.png)

## <a name="creating-the-web-application"></a>Erstellen der Webanwendung

Um das Lernprogramm zu starten, öffnen Sie Visual Studio, und klicken Sie dann erstellen Sie ein neues ASP.NET-Webanwendungsprojekt mit der **ASP.NET-Webanwendung** Vorlage:

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-1/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image11.png)

Diese Vorlage erstellt ein Webanwendungsprojekt, die bereits ein Stylesheet und Gestaltungsvorlagen enthält:

[![Image02](the-entity-framework-and-aspnet-getting-started-part-1/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image13.png)

Öffnen der *Site.Master* Datei und Ändern von "Meine ASP.NET-Anwendung" auf "Contoso University".

[!code-html[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample1.html)]

Suchen der *Menü* Steuerelement namens `NavigationMenu` und Ersetzen sie durch das folgende Markup, das für die Seiten Menüelemente hinzufügt, Sie erstellen.

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample2.aspx)]

Öffnen der *"default.aspx"* Seite, und ändern Sie die `Content` Steuerelement namens `BodyContent` dieser:

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-1/samples/sample3.aspx)]

Sie verfügen jetzt über eine einfache Homepage mit Links zu den verschiedenen Seiten, die Sie erstellen müssen:

[![Image03](the-entity-framework-and-aspnet-getting-started-part-1/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image15.png)

## <a name="creating-the-database"></a>Erstellen der Datenbank

Für diese Lernprogramme verwenden Sie im Entity Framework Data Model-Designer automatisch das Datenmodell basierend auf einer vorhandenen Datenbank erstellen (oft als bezeichnet den *Database First* Ansatz). Eine Alternative, die in diesem Lernprogramm Reihe abgedeckt werden zum Erstellen des Datenmodells manuell, und lassen Sie dann den Designer generierten Skripts, die die Datenbank zu erstellen ist (die *Model First* Ansatz).

Im nächste Schritt werden für die Database First-Methode, die in diesem Lernprogramm verwendet um der Website eine Datenbank hinzuzufügen. Die einfachste Möglichkeit wird zuerst das Projekt herunterladen, das für dieses Lernprogramm geht. Klicken Sie dann mit der rechten Maustaste die *App\_Daten* Ordner wählen **vorhandenes Element hinzufügen**, und wählen Sie die *School.mdf* Datenbankdatei aus dem heruntergeladenen Projekt.

Eine Alternative besteht darin, folgen die Anweisungen unter [Erstellen der Beispieldatenbank "School"](https://msdn.microsoft.com/library/bb399731.aspx). Kopieren Sie, ob Sie laden Sie die Datenbank, oder erstellen Sie ihn, den *School.mdf* Datei aus dem folgenden Ordner für Ihre Anwendungsverzeichnis *App\_Daten* Ordner:

`%PROGRAMFILES%\Microsoft SQL Server\MSSQL10.SQLEXPRESS\MSSQL\DATA`

(Der Pfad der *mdf* Datei geht davon aus, verwenden Sie SQL Server 2008 Express.)

Wenn Sie die Datenbank aus einem Skript erstellen, führen Sie die folgenden Schritte aus, um ein Datenbankdiagramm erstellen:

1. In **Server-Explorer**, erweitern Sie **Datenverbindungen**, erweitern Sie *School.mdf*, mit der rechten Maustaste **Datenbankdiagrammen**, und wählen Sie **Neues Diagramm hinzufügen**.

    [![image35](the-entity-framework-and-aspnet-getting-started-part-1/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image17.png)
2. Wählen Sie alle Tabellen aus, und klicken Sie dann auf **hinzufügen**.

    [![image36](the-entity-framework-and-aspnet-getting-started-part-1/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image19.png)

    SQL Server erstellt ein Datenbankdiagramm, die Tabellen, Spalten in Tabellen und Beziehungen zwischen den Tabellen anzeigt. Sie können die Tabellen, etwa, um sie zu organisieren, beliebig verschieben.
3. Speichern Sie das Diagramm als "SchoolDiagram", und schließen Sie es.

Wenn Sie Herunterladen der *School.mdf* -Datei, die mit diesem Lernprogramm geht sehen Sie das Datenbankdiagramm durch Doppelklicken auf **SchoolDiagram** unter **Datenbankdiagrammen** in **Server-Explorer**.

[![image38](the-entity-framework-and-aspnet-getting-started-part-1/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image21.png)

Das Diagramm sieht etwa wie folgt (in den Tabellen möglicherweise an verschiedenen Standorten, von dem hier gezeigten):

[![Image04](the-entity-framework-and-aspnet-getting-started-part-1/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image23.png)

## <a name="creating-the-entity-framework-data-model"></a>Erstellen das Entity Framework-Datenmodells

Jetzt können Sie ein Entity Framework-Datenmodells aus dieser Datenbank erstellen. Sie können das Datenmodell im Stammordner der Anwendung erstellen, aber für dieses Lernprogramm müssen Sie ihn in einem Ordner namens platzieren *DAL* (für die Datenzugriffsebene).

In **Projektmappen-Explorer**, fügen Sie einen Projektordner mit dem Namen *DAL* (Stellen Sie sicher, dass er unter dem Projekt nicht unterhalb der Lösung).

Mit der rechten Maustaste die *DAL* Ordner, und wählen Sie dann **hinzufügen** und **neues Element**. Unter **installierte Vorlagen**Option **Daten**, wählen die **ADO.NET Entity Data Model** Vorlage, nennen Sie sie *SchoolModel.edmx*, und Klicken Sie dann auf **hinzufügen**.

[![Image05](the-entity-framework-and-aspnet-getting-started-part-1/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image25.png)

Assistent für Entity Data Model wird gestartet. In den ersten Schritt im Assistenten der **aus Datenbank generieren** Option ist standardmäßig aktiviert. Klicken Sie auf **Weiter**.

[![Image06](the-entity-framework-and-aspnet-getting-started-part-1/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image27.png)

In der **wählen Sie Ihre Datenverbindung** Schritt, übernehmen die Standardwerte, und klicken Sie auf **Weiter**. Die Datenbank "School" ist standardmäßig aktiviert und die verbindungseinstellung wird gespeichert, der *"Web.config"* als Datei **den Eintrag SchoolEntities**.

[![Image07](the-entity-framework-and-aspnet-getting-started-part-1/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image29.png)

In der **Datenbankobjekte auswählen** Schritt des Assistenten, wählen Sie alle Tabellen mit Ausnahme von `sysdiagrams` (die für das Diagramm, die zuvor von Ihnen generierte erstellt wurde), und klicken Sie dann auf **Fertig stellen**.

[![Image08](the-entity-framework-and-aspnet-getting-started-part-1/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image31.png)

Nachdem sie das Modell erstellt wurde, zeigt Visual Studio eine grafische Darstellung der Entity Framework-Objekte (Entitäten), die von Datenbanktabellen entsprechen. (Wie bei einem Datenbankdiagramm, der Speicherort der einzelnen Elemente weicht möglicherweise in der folgenden Abbildung angezeigt. Sie können die Elemente ziehen rund um die Abbildung entsprechen, sofern Sie möchten.)

[![Image09](the-entity-framework-and-aspnet-getting-started-part-1/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image33.png)

## <a name="exploring-the-entity-framework-data-model"></a>Untersuchen die Entity Framework-Datenmodells

Sie können sehen, dass die Entitätsdiagramm Datenbankdiagramm über ein paar Unterschiede sehr ähnelt. Ein Unterschied ist das Hinzufügen von Symbolen am Ende jeder Zuordnung, die den Typ der Zuordnung anzugeben (tabellenbeziehungen werden Zuordnungen Entität im Datenmodell genannt):

- Eine 1: 0 (null)-oder-1-Zuordnung wird durch "1" und "0.. 1" dargestellt.

    [![Image39](the-entity-framework-and-aspnet-getting-started-part-1/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image35.png)

    In diesem Fall eine `Person` Entität kann oder möglicherweise nicht zugeordnet werden ein `OfficeAssignment` Entität. Ein `OfficeAssignment` Entität zugeordnet werden eine `Person` Entität. Das heißt, ein Kursleiter kann oder kann nicht zugewiesen werden, um ein Office und alle Office kann nur einen Kursleiter zugewiesen werden.
- Eine 1: n-Zuordnung wird dargestellt, die durch "1" und "\*".

    [![image40](the-entity-framework-and-aspnet-getting-started-part-1/_static/image38.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image37.png)

    In diesem Fall eine `Person` Entität kann oder möglicherweise nicht über zugeordnete `StudentGrade` Entitäten. Ein `StudentGrade` Entität muss mit einem zugeordneten `Person` Entität. `StudentGrade` Entitäten darstellen tatsächlich registrierte Kurse in dieser Datenbank; Wenn eine Student einen Kurs registriert ist und noch keine Grade besteht die `Grade` -Eigenschaft null ist. Eine Student also möglicherweise nicht in jeder Kursen registriert sein noch, möglicherweise in einen Kurs registriert werden oder kann in mehreren Kursen registriert sein. Jeder Jahrgangsstufe einer registrierten Kurs gilt für nur eine Student.
- Eine m: n-Zuordnung dargestellte "\*"und"\*".

    [![image41](the-entity-framework-and-aspnet-getting-started-part-1/_static/image40.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image39.png)

    In diesem Fall eine `Person` Entität kann oder möglicherweise nicht über zugeordnete `Course` Entitäten und das Gegenteil ist ebenfalls "true": eine `Course` Entität kann oder möglicherweise nicht über zugeordnete `Person` Entitäten. Also ein Kursleiter möglicherweise mehrere Kurse werden folgende Themen behandelt, und ein Kurs kann durch mehrere Lehrkräfte behandelt werden. (In dieser Datenbank diese Beziehung gilt nur für Lehrkräfte; es jedoch nicht Studenten mit Kurse verknüpft. Studenten sind Kursen durch die StudentGrades-Tabelle verknüpft.)

Ein weiterer Unterschied zwischen dem Datenbankdiagramm und des Datenmodells ist die zusätzliche **Navigationseigenschaften** Abschnitt für jede Entität. Eine Navigationseigenschaft einer Entität verweist auf verknüpfte Entitäten. Z. B. die `Courses` Eigenschaft in einer `Person` Entität enthält eine Auflistung aller der `Course` , die verbundenen, Entitäten `Person` Entität.

[![image12](the-entity-framework-and-aspnet-getting-started-part-1/_static/image42.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image41.png)

Noch ein weiterer Unterschied zwischen der Datenbank und Datenmodell Abwesenheit ist die `CourseInstructor` Zuordnungstabelle, in der Datenbank, zum Verknüpfen verwendet wird, der `Person` und `Course` Tabellen in einer m: n-Beziehung. Navigationseigenschaften ermöglichen es Ihnen, die in Zusammenhang abrufen `Course` Entitäten aus der `Person` Entität und verknüpften `Person` Entitäten aus der `Course` Entität, daher keine Notwendigkeit besteht, die die Zuordnung im Datenmodell darstellen.

[![image11](the-entity-framework-and-aspnet-getting-started-part-1/_static/image44.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image43.png)

Für die Zwecke dieses Lernprogramms, nehmen Sie an der `FirstName` Spalte die `Person` Tabelle enthält einer Person Vornamen und Vornamen. So ändern Sie den Namen des Felds, wird dies berücksichtigt werden sollen, aber der Datenbankadministrator (DBA) möchten nicht, dass die Datenbank zu ändern. Sie können den Namen des ändern die `FirstName` unverändert Eigenschaft im Datenmodell, lassen Sie die Datenbank entspricht.

Klicken Sie im Designer mit der Maustaste **FirstName** in der `Person` Entität, und wählen Sie dann **umbenennen**.

[![image13](the-entity-framework-and-aspnet-getting-started-part-1/_static/image46.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image45.png)

Geben Sie den neuen Namen "FirstMidName". Dadurch wird die Möglichkeit, die Sie auf die Spalte im Code verweisen ohne Ändern der Datenbank geändert.

[![image29](the-entity-framework-and-aspnet-getting-started-part-1/_static/image48.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image47.png)

Die Model-Browser stellt eine andere Möglichkeit, die Datenbankstruktur, die Modellstruktur Daten und die Zuordnung zwischen ihnen anzuzeigen. Um diese anzuzeigen, mit der rechten Maustaste in eines leeren Bereichs im Entitätsdesigner, und klicken Sie dann auf **Modellbrowser**.

[![image18](the-entity-framework-and-aspnet-getting-started-part-1/_static/image50.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image49.png)

Die **Modellbrowser** Bereich wird eine Strukturansicht angezeigt. (Die **Modellbrowser** Bereich angedockt werden kann, mit der **Projektmappen-Explorer** Bereich.) Die **SchoolModel** Knoten darstellt, die Modellstruktur Daten und die **SchoolModel.Store** Knoten darstellt, die Struktur der Datenbank.

[![image26](the-entity-framework-and-aspnet-getting-started-part-1/_static/image52.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image51.png)

Erweitern Sie **SchoolModel.Store** um die Tabellen anzuzeigen, erweitern Sie **Tabellen / Sichten** finden in Tabellen, und erweitern dann **Kurs** Spalten innerhalb einer Tabelle angezeigt.

[![image19](the-entity-framework-and-aspnet-getting-started-part-1/_static/image54.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image53.png)

Erweitern Sie **SchoolModel**, erweitern Sie **Entitätstypen**, und erweitern Sie dann die **Kurs** Knoten, um die Entitäten und Eigenschaften innerhalb der Entitäten anzuzeigen.

[![image20](the-entity-framework-and-aspnet-getting-started-part-1/_static/image56.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image55.png)

In der-Designer oder der **Modellbrowser** Bereich können Sie sehen, wie das Entity Framework die Objekte der beiden Modelle, bezieht. Mit der rechten Maustaste die `Person` Entität, und wählen **Tabelle zuordnen**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-1/_static/image58.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image57.png)

Daraufhin wird die **Mappingdetails** Fenster. Beachten Sie, dass in diesem Fenster Sie sehen, die können die Datenbankspalte `FirstName` zugeordnet `FirstMidName`, was Sie es in das Datenmodell umbenannt ist.

[![image22](the-entity-framework-and-aspnet-getting-started-part-1/_static/image60.png)](the-entity-framework-and-aspnet-getting-started-part-1/_static/image59.png)

Das Entity Framework verwendet XML zum Speichern von Informationen zu der Datenbank, das Datenmodell und die Zuordnungen zwischen ihnen. Die *SchoolModel.edmx* Datei ist tatsächlich eine XML-Datei, die diese Informationen enthält. Rendert der Designer die Informationen in einem grafischen Format, aber Sie können auch die Datei im XML-Format anzeigen, indem Sie mit der rechten Maustaste die *EDMX* Datei **Projektmappen-Explorer**auf **Öffnen mit**, auswählen und **XML (Text)-Editor**. (Modell-Datendesigner und XML-Editor sind nur zwei unterschiedliche Arten der Start- und Arbeiten mit der gleichen Datei, damit Sie den Designer zu öffnen, und öffnen Sie die Datei in einem XML-Editor zur gleichen Zeit haben können.)

Sie haben jetzt eine Website, eine Datenbank und ein Datenmodell erstellt. In der nächsten exemplarischen Vorgehensweise beginnen Sie arbeiten mit Daten, die mithilfe des Datenmodells und das ASP.NET `EntityDataSource` Steuerelement.

> [!div class="step-by-step"]
> [Nächste](the-entity-framework-and-aspnet-getting-started-part-2.md)
