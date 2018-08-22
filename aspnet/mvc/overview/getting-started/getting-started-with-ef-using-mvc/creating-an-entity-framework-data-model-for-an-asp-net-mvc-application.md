---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Erste Schritte mit Entity Framework 6 Code First anhand von MVC 5 | Microsoft-Dokumentation
author: tdykstra
description: 'Eine neuere Version dieser tutorialreihe ist verfügbar: Erste Schritte mit ASP.NET Core und Entity Framework Core mithilfe von Visual Studio 2015. Der Contoso-Universi...'
ms.author: riande
ms.date: 10/22/2015
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 29004ec2271dbf77395f07e030533e23662b67c1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826542"
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>Erste Schritte mit Entity Framework 6 Code First unter Verwendung von MVC 5
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > Eine neuere Version dieser tutorialreihe ist verfügbar: [erste Schritte mit ASP.NET Core und Entity Framework Core mithilfe von Visual Studio 2015](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).
> 
> 
> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 und Visual Studio 2013. Dieses Tutorial verwendet die Code First-Workflow. Informationen zur Wahl zwischen dem Code First, Database First und Model First, finden Sie unter [Entity Framework-Entwicklungsworkflows](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> Bei der Beispiel-App handelt es sich um eine Website für die fiktive Contoso University. Sie enthält Funktionen wie die Zulassung von Studenten, die Erstellung von Kursen und Aufgaben von Dozenten. Dieser tutorialreihe wird erläutert, wie die beispielanwendung der Contoso University. Sie können [die fertige Anwendung herunterladen](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
> 
> Steht eine Visual Basic-Version von Mike Brind übersetzt: [MVC 5 mit EF 6 in Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) auf der Website Mikesdotnetting.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Entitätsframework 6 (EntityFramework 6.1.0-NuGet-Paket)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (optional)
>   
> 
> Das Tutorial sollten ebenfalls funktionieren mit [Visual Studio-2013 Express für Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) oder Visual Studio 2012. Die [VS 2012-Version von Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323511) für Windows Azure-Bereitstellung mit Visual Studio 2012 ist erforderlich.
> 
> 
> ## <a name="tutorial-versions"></a>Lernprogramm-Versionen
> 
> Bei früheren Versionen dieses Tutorials finden Sie unter [der EF 4.1 / MVC 3-e-Book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) und [erste Schritte mit EF 5 anhand von MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Lassen Sie Feedback, auf wie Ihnen in diesem Tutorial gefallen hat und was wir in den Kommentaren am unteren Rand der Seite verbessern können. Wenn Sie Fragen, die nicht direkt mit dem Tutorial verknüpft sind haben, können Sie sie veröffentlichen das [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx), [Entity Framework und LINQ to Entities-Forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), oder [ StackOverflow.com](http://stackoverflow.com/).
> 
> Wenn Sie auf ein Problem, die Sie nicht beheben können stoßen, im Allgemeinen finden Sie die Lösung des Problems durch vergleichen Ihren Code mit dem abgeschlossenen Projekt, das Sie herunterladen können. Einige häufige Fehler und zu deren Lösung finden Sie [allgemeine Fehler und Lösungen oder problemumgehungen für sie.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>Der Contoso University-Web-Anwendung

Bei der Anwendung, die Sie mithilfe dieser Tutorials erstellen, handelt es sich um eine einfache Universitätswebsite.

Benutzer können Informationen zu den Studenten, Kursen und Dozenten abrufen. Nachfolgend werden einige Anzeigen dargestellt, die erstellt werden sollen.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Kursteilnehmer bearbeiten](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Der Benutzeroberflächenstil dieser Website orientiert sich an den integrierten Vorlagen, sodass Sie mit dieses Tutorial hauptsächlich auf die Verwendung von Entity Framework konzentriert.

## <a name="prerequisites"></a>Erforderliche Komponenten

Finden Sie unter **Softwareversionen** am oberen Rand der Seite. Entitätsframework 6 ist eine Voraussetzung nicht, da Sie als Teil des Tutorials das EF-NuGet-Paket installieren.

## <a name="create-an-mvc-web-application"></a>Erstellen einer MVC-Webanwendung

Öffnen Sie Visual Studio, und erstellen Sie ein neues c#-Projekt mit dem Namen "ContosoUniversity".

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

In der **neues ASP.NET-Projekt** aktivieren Sie im Dialogfeld die **MVC** Vorlage.

Wenn die **in der Cloud hosten** Kontrollkästchen in der **Microsoft Azure** Abschnitt ausgewählt ist, müssen Sie diese löschen.

Klicken Sie auf **Authentifizierung ändern**.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

In der **Authentifizierung ändern** wählen Sie im Dialogfeld **keine Authentifizierung**, und klicken Sie dann auf **OK**. Für dieses Tutorial Sie nicht, die Benutzer zur Anmeldung erfordern oder Einschränken des Zugriffs basierend auf den angemeldeten Benutzer.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Klicken Sie in das Dialogfeld Neues ASP.NET-Projekt auf **OK** zum Erstellen des Projekts.

## <a name="set-up-the-site-style"></a>Einrichten des Websitestils

Sie können das Websitemenü, das Layout und die Startseite über einige Änderungen einrichten.

Open *Views\Shared\\"_Layout.cshtml"*, und nehmen Sie die folgenden Änderungen:

- Ändern Sie jedes Vorkommen von "My ASP.NET Application" und "Application Name" in "Contoso University".
- Fügen Sie Menüeinträge für Schüler/Studenten, Kurse, Dozenten und Abteilungen, und löschen Sie den Eintrag wenden Sie sich an.

Die Änderungen werden hervorgehoben.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

In *Views\Home\Index.cshtml*, ersetzen Sie den Inhalt der Datei mit den folgenden Code hinzu, den Text zu ASP.NET und MVC durch Text zu dieser Anwendung zu ersetzen:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Drücken Sie STRG + F5, um die Website auszuführen. Die Startseite mit im Hauptmenü angezeigt.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Installieren von Entitätsframework 6

Von der **Tools** klicken Sie im Menü **NuGet Package Manager** , und klicken Sie dann auf **-Paket-Manager-Konsole**.

In der **-Paket-Manager-Konsole** Geben Sie im Fenster den folgenden Befehl aus:

`Install-Package EntityFramework`

![EF installiert](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Die Abbildung zeigt die 6.0.0 installiert wird, aber NuGet installiert die neueste Version von Entity Framework (mit Ausnahme des Vorabversionen), die seit der letzten Aktualisierung mit dem Tutorial 6.1.1 ist.

Dieser Schritt ist eine der wenigen Schritten an, dass für dieses Lernprogramm sind Sie manuell ausführen, aber das hätte automatisch von der ASP.NET MVC-Gerüstbau-Funktion. Sie tun sie manuell, damit Sie die erforderlichen Schritte für die Verwendung von Entity Framework sehen können. Sie müssen Gerüstbau später verwenden, um die MVC-Controller und Ansichten zu erstellen. Alternativ kann können Gerüstbau automatisch das EF-NuGet-Paket installieren, erstellen den Datenbankkontext-Klasse und die Verbindungszeichenfolge zu erstellen. Wenn Sie auf diese Weise dafür bereit sind, müssen Sie, lediglich diese Schritte überspringen und das Gerüst für Ihre MVC-Controller nach der Erstellung Ihrer Entitätsklassen.

## <a name="create-the-data-model"></a>Erstellen des Datenmodells

Als nächstes erstellen Sie Entitätsklassen für die Contoso University-Anwendung. Beginnen Sie mit dem folgenden drei Entitäten:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Es besteht eine 1:n-Beziehung zwischen den Entitäten `Student` und `Enrollment`. Außerdem besteht eine 1:n-Beziehung zwischen den Entitäten `Course` und `Enrollment`. Das bedeutet, dass ein Student für beliebig viele Kurse angemeldet sein kann und sich für jeden Kurs eine beliebige Anzahl von Studenten anmelden kann.

In den folgenden Abschnitten erstellen Sie für jede dieser Entitäten eine Klasse.

> [!NOTE]
> Wenn Sie versuchen, das Projekt zu kompilieren, vor dem Erstellen aller dieser Entitätsklassen, erhalten Sie Compilerfehler zu beheben.


### <a name="the-student-entity"></a>Die Entität "Student"

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

In der *Modelle* Ordner, erstellen Sie eine Klassendatei mit dem Namen *Student.cs* , und Ersetzen Sie den Vorlagencode durch den folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

Die `ID`-Eigenschaft fungiert als Primärschlüsselspalte der Datenbanktabelle, die dieser Klasse entspricht. Standardmäßig interpretiert das Entity Framework eine Eigenschaft mit dem Namen `ID` oder *Classname* `ID` als Primärschlüssel.

Die `Enrollments` -Eigenschaft ist eine *Navigationseigenschaft*. Navigationseigenschaften enthalten andere Entitäten, die dieser Entität zugehörig sind. In diesem Fall die `Enrollments` Eigenschaft eine `Student` Entität enthält alle der `Enrollment` Entitäten, die im Zusammenhang mit, `Student` Entität. Das heißt, wenn eine angegebene `Student` Zeile in der Datenbank verfügt über zwei im Zusammenhang `Enrollment` Zeilen (Zeilen, die Primärschlüssel des Studenten enthalten Wert in ihre `StudentID` Fremdschlüsselspalte), die von `Student` Entität `Enrollments` Navigationseigenschaft Diese beiden `Enrollment` Entitäten.

Navigationseigenschaften werden in der Regel als definiert `virtual` , damit sie bestimmte Entity Framework-Funktionen wie z. B. nutzen können *Lazy Load*. (Lazy Loading werden erläutert werden weiter unten in der [Lesen von verwandten Daten](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) Tutorial weiter unten in dieser Reihe.)

Wenn eine Navigationseigenschaft mehrere Entitäten enthalten kann (wie bei m:n- oder 1:n-Beziehungen), muss dessen Typ aus einer Liste bestehen, in der Einträge hinzugefügt, gelöscht und aktualisiert werden können – z.B.: `ICollection`.

### <a name="the-enrollment-entity"></a>Die Entität "Enrollment"

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

Erstellen Sie im Ordner *Models* (Modelle) die Datei *Enrollment.cs*, und ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

Die `EnrollmentID` Eigenschaft wird der Primärschlüssel sein; diese Entität verwendet die *Classname* `ID` -Muster anstelle von `ID` selbst, wie Sie gesehen haben, der `Student` Entität. Normalerweise würden Sie nur ein Muster auswählen und dieses für das gesamte Datenmodell verwenden. Diese Variation soll verdeutlichen, dass Sie ein beliebiges Muster erstellen können. Sehen Sie in einem späteren Tutorial, wie sich durch Verwendung `ID` ohne `classname` erleichtert es, die Vererbung in das Datenmodell zu implementieren.

Die `Grade` -Eigenschaft ist ein [Enum](https://msdn.microsoft.com/data/hh859576.aspx). Das Fragezeichen nach der `Grade` Typdeklaration gibt an, dass die `Grade` -Eigenschaft ist [auf NULL festlegbare](https://msdn.microsoft.com/library/2cf62fcy.aspx). Eine Grade-Eigenschaft, die null unterscheidet sich von einer 0 (null) auf Unternehmensniveau – Null bedeutet, dass keine Grade-Eigenschaft bekannt ist oder noch nicht zugewiesen wurde.

Bei der `StudentID`-Eigenschaft handelt es sich um einen Fremdschlüssel, und `Student` ist die entsprechende Navigationseigenschaft. Eine `Enrollment`-Entität wird einer `Student`-Entität zugeordnet, damit die Eigenschaft nur eine `Student`-Entität enthalten kann. Dies steht im Gegensatz zu der bereits erläuterten `Student.Enrollments`-Navigationseigenschaft, die mehrere `Enrollment`-Entitäten enthalten kann.

Bei der `CourseID`-Eigenschaft handelt es sich um einen Fremdschlüssel, und die zugehörige Navigationseigenschaft lautet `Course`. Die `Enrollment`-Entität wird einer `Course`-Entität zugeordnet.

Entitätsframework interpretiert Eigenschaften als Fremdschlüsseleigenschaften, wenn sie den Namen *&lt;navigationseigenschaftennamen&gt;&lt;Primärschlüsseleigenschaft Namen&gt;* (z. B. `StudentID`für die `Student` Navigationseigenschaft, da die `Student` Primärschlüssel der Entität ist `ID`). Fremdschlüsseleigenschaften können auch einfach den Namen der gleiche *&lt;Primärschlüsseleigenschaft Namen&gt;* (z. B. `CourseID` seit der `Course` Primärschlüssel der Entität ist `CourseID`).

### <a name="the-course-entity"></a>Die Entität "Course"

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

In der *Modelle* Ordner erstellen *Course.cs*, und Ersetzen Sie den Vorlagencode durch den folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Die `Enrollments`-Eigenschaft ist eine Navigationseigenschaft. `Course`-Entitäten können sich auf jede beliebige Anzahl von `Enrollment`-Entitäten beziehen.

Wir definieren Weitere Informationen zu den ["databasegenerated"](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) -Attribut in einem späteren Tutorial dieser Reihe. Im Grunde können Sie über dieses Attribut den Primärschlüssel für den Kurs angeben, anstatt ihn von der Datenbank generieren zu lassen.

## <a name="create-the-database-context"></a>Erstellen des Datenbankkontexts

Ist die Hauptklasse, die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert die *Datenbankkontext* Klasse. Erstellen Sie diese Klasse durch Ableiten von der [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) Klasse. Sie geben in Ihrem Code an, welche Entitäten im Datenmodell enthalten sind. Außerdem können Sie bestimmte Entity Framework-Verhalten anpassen. In diesem Projekt heißt die Klasse `SchoolContext`.

Zum Erstellen eines Ordners im Projekt ContosoUniversity, Rechtsklick im Projekt im **Projektmappen-Explorer** , und klicken Sie auf **hinzufügen**, und klicken Sie dann auf **neuer Ordner**. Nennen Sie diesen Ordner *DAL* (für Data Access Layer). In diesem Ordner erstellen Sie eine neue Klassendatei mit dem Namen *SchoolContext.cs*, und Ersetzen Sie den Vorlagencode durch den folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>Angeben von Entitätenmengen

Dieser Code erstellt eine ["DbSet"](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) -Eigenschaft für jede Entitätenmenge. In der Terminologie von Entity Framework eine *Entitätenmenge* entspricht in der Regel einer Datenbanktabelle und ein *Entität* entspricht einer Zeile in der Tabelle.

> [!NOTE] 
> 
> Sie können ausgelassen haben die `DbSet<Enrollment>` und `DbSet<Course>` -Anweisungen, und es würde funktionieren auf dieselbe Weise. Diese sind implizit in Entity Framework enthalten, da die `Student`-Entität auf die `Enrollment`-Entität und die `Enrollment`-Entität auf die `Course`-Entität verweist.


### <a name="specifying-the-connection-string"></a>Angeben der Verbindungszeichenfolge

Der Name der Verbindungszeichenfolge (die Sie der Datei "Web.config" später hinzufügen) wird in an den Konstruktor übergeben.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Sie könnten auch die Verbindungszeichenfolge selbst anstelle des Namens eines übergeben, die in der Datei "Web.config" gespeichert ist. Weitere Informationen zu Optionen zum Angeben der Datenbank verwenden, finden Sie unter [Entity Framework - Verbindungen und Modelle](https://msdn.microsoft.com/data/jj592674).

Wenn Sie eine Verbindungszeichenfolge oder den Namen eines nicht explizit angeben, wird das Entity Framework davon ausgegangen, dass der Name der Verbindungszeichenfolge den Namen der Klasse identisch ist. Die Namen der Standardverbindungszeichenfolge in diesem Beispiel werden dann `SchoolContext`, identisch mit, was Sie explizit angeben.

### <a name="specifying-singular-table-names"></a>Tabellennamen im singular angegeben

Die `modelBuilder.Conventions.Remove` -Anweisung in der ["onmodelcreating"](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) Methode verhindert, dass Tabellennamen im Plural wird. Wenn Sie dies nicht gemacht hätte, die generierten Tabellen in der Datenbank hieße `Students`, `Courses`, und `Enrollments`. Stattdessen werden die Tabellennamen `Student`, `Course`, und `Enrollment`. Entwickler sind sich uneinig darüber, ob Tabellennamen im Plural stehen sollten oder nicht. In diesem Tutorial wird die Form im singular verwendet, aber der wichtigste Punkt ist, dass Sie, welcher Form Sie bevorzugen auswählen können, durch Einfügen oder diese Zeile des Codes auslassen.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Einrichten von EF zum Initialisieren der Datenbank mit Testdaten

Entity Framework können automatisch erstellen (oder drop neu zu erstellen und) eine Datenbank auf, wenn die Anwendung ausgeführt wird. Sie können angeben, dass dies erfolgen sollte jedes Mal, wenn Ihre Anwendung ausgeführt wird, oder nur, wenn das Modell mit der vorhandenen Datenbank synchron sind. Sie können auch Schreiben einer `Seed` Methode, die das Entity Framework ruft automatisch nach dem Erstellen der Datenbank, um diese mit Testdaten aufzufüllen.

Das Standardverhalten ist zum Erstellen einer Datenbank nur dann, wenn sie nicht vorhanden ist (und löst eine Ausnahme aus, wenn das Modell geändert wurde, und die Datenbank bereits vorhanden ist). In diesem Abschnitt müssen Sie angeben, dass die Datenbank gelöscht und neu erstellt werden, wenn das Modell ändert. Löschen der Datenbank bewirkt, dass den Verlust aller Daten. Dies ist im Allgemeinen OK während der Entwicklung, da die `Seed` Methode ausgeführt, wenn die Datenbank neu erstellt wird und die Testdaten wird erneut erstellt. In der Produktion in der Regel möchten jedoch nicht alle Ihre Daten verloren gehen, jedes Mal, wenn Sie das Schema der Datenbank ändern müssen. Weiter unten sehen Sie, wie Sie die Änderungen des Datenmodells zu behandeln, indem Sie Code First-Migrationen zum Ändern des Datenbankschemas löschen und Neuerstellen der Datenbank.

Erstellen Sie eine neue Klassendatei mit dem Namen im Ordner DAL *SchoolInitializer.cs* , und Ersetzen Sie den Vorlagencode durch den  
folgenden Code, der bewirkt, dass eine Datenbank erstellt, wenn werden benötigt, und lädt Testdaten in die neue Datenbank.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

Die `Seed` Methode nimmt das Datenbank-Context-Objekt als Eingabeparameter und der Code in der Methode verwendet das Objekt auf der Datenbank neue Entitäten hinzugefügt. Für jeden Entitätstyp, der Code erstellt eine Auflistung von neuen Entitäten, fügt diese an die entsprechende `DbSet` Eigenschaft, und klicken Sie dann speichert die Änderungen an der Datenbank. Es ist nicht notwendig, die `SaveChanges` Methode nach jeder Gruppe von Entitäten, wie hier geschehen ist, aber dies hilft Ihnen, die Ursache eines Problems zu suchen, wenn eine Ausnahme auftritt, während der Code in die Datenbank geschrieben werden.

Um sagen Entity Framework, die Ihre Initialisiererklasse verwenden, fügen Sie ein Element der `entityFramework` Element in der Anwendung *"Web.config"* (die eine Datei in den Stammordner des Projekts), wie im folgenden Beispiel gezeigt:

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

Die `context type` gibt an, der Name der vollqualifizierte Kontext und die Assembly, die er sich befindet, können und die `databaseinitializer type` gibt an, den vollqualifizierten Namen der Initialisiererklasse und die Assembly ist. (Wenn Sie nicht, dass EF die Initialisierer verwenden möchten, können Sie ein Attribut festlegen, auf die `context` Element: `disableDatabaseInitialization="true"`.) Weitere Informationen finden Sie unter [Entity Framework - Konfigurationseinstellungen für die Datei](https://msdn.microsoft.com/data/jj556606).

Als Alternative zum Festlegen des Initialisierers in der *"Web.config"* Datei ist, dass der Code durch Hinzufügen einer `Database.SetInitializer` Anweisung, um die `Application_Start` -Methode in der die *"Global.asax.cs"* Datei. Weitere Informationen finden Sie unter [Grundlegendes zu Datenbank-Initialisierer in Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

Die Anwendung ist nun so festgelegt einrichten, damit beim der Datenbank zum ersten Mal in einer bestimmten Ausführung der Zugriff auf die  
Anwendung Entity Framework vergleicht die Datenbank für das Modell (Ihre `SchoolContext` und Entitätsklassen). Wenn es ein Unterschied besteht, wird von die Anwendung gelöscht und neu erstellt die Datenbank.

> [!NOTE]
> Wenn Sie eine Anwendung auf einem Webserver für die Produktion bereitstellen, müssen Sie entfernen oder Deaktivieren von Code, der gelöscht und die Datenbank neu erstellt. Haben Sie Gelegenheit, die in einem späteren Tutorial dieser Reihe.


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Einrichten von EF verwenden eine SQL Server Express LocalDB-Datenbank

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) ist eine Basisversion von SQL Server Express-Datenbankmoduls. Es ist einfach zu installieren und konfigurieren, wird bedarfsgesteuert gestartet und im Benutzermodus ausgeführt wird. LocalDB ausgeführt wird, in einem speziellen Ausführungsmodus von SQL Server Express, die Ihnen ermöglicht, die Arbeit mit Datenbanken als *mdf* Dateien. Sie können LocalDB-Datenbankdateien abzulegen, der *App\_Daten* Ordner eines Webprojekts, wenn die Datenbank mit dem Projekt kopiert werden sollen. Das Feature "Instanz" in SQL Server Express ermöglicht Ihnen die Arbeit mit auch *mdf* Dateien, aber das Feature "Instanz" ist veraltet; LocalDB wird daher empfohlen, für die Arbeit mit *mdf* Dateien. In Visual Studio 2012 und höher wird die LocalDB wird standardmäßig mit Visual Studio installiert.

SQL Server Express wird in der Regel für Produktions-Web-Anwendungen nicht verwendet. LocalDB wird insbesondere nicht mit einer Webanwendung für die Produktion empfohlen, da es nicht ausgelegt ist, arbeiten Sie mit IIS.

In diesem Tutorial arbeiten Sie mit LocalDB. Öffnen Sie die Anwendung *"Web.config"* -Datei und fügen eine `connectionStrings` Element vor dem `appSettings` Element, wie im folgenden Beispiel dargestellt. (Stellen Sie sicher, dass Sie aktualisieren die *"Web.config"* -Datei in den Stammordner des Projekts. Es gibt auch eine *"Web.config"* Datei befindet sich in der *Ansichten* Unterordner, die Sie nicht aktualisieren müssen.)

Wenn Sie Visual Studio 2015 verwenden, ersetzen Sie "V11. 0" in der Verbindungszeichenfolge durch "MSSQLLocalDB", wie der Standardinstanzname von SQL Server geändert wurde.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Die Verbindungszeichenfolge, die Sie hinzugefügt haben gibt an, dass Entity Framework eine LocalDB-Datenbank, die mit dem Namen verwendet *ContosoUniversity1.mdf*. (Die Datenbank ist noch nicht vorhanden; EF wird erstellt.) Wenn Sie beispielsweise, dass die Datenbank erstellt werden Ihre *App\_Daten* Ordner hinzufügen `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` an der Verbindungszeichenfolge. Weitere Informationen zu Verbindungszeichenfolgen finden Sie unter [SQL Server-Verbindungszeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx).

Sie müssen nicht unbedingt eine Verbindungszeichenfolge verfügen, der *"Web.config"* Datei. Wenn Sie eine Verbindungszeichenfolge angeben, verwendet Entity Framework den Standardwert 1-auf Ihr Context-Klasse basiert. Weitere Informationen finden Sie unter [Code First in eine neue Datenbank](https://msdn.microsoft.com/data/jj193542).

## <a name="creating-a-student-controller-and-views"></a>Erstellen einen "Student"-Controller und Ansichten

Nun erstellen Sie eine Webseite, um Daten anzuzeigen, und der Prozess des Anforderns von Daten wird automatisch ausgelöst.  
die Erstellung der Datenbank. Sie beginnen, indem Sie einen neuen Domänencontroller erstellen. Aber bevor Sie dies tun, erstellen Sie das Projekt aus, um die Klassen Modell und Kontext auf MVC-Controller-Gerüstbau verfügbar zu machen.

1. Mit der rechten Maustaste die **Controller** Ordner **Projektmappen-Explorer**Option **hinzufügen**, und klicken Sie dann auf **neues Gerüstelement**.
2. In der **Gerüst hinzufügen** wählen Sie im Dialogfeld **MVC 5-Controller mit Ansichten unter Verwendung von Entity Framework**.

     ![Gerüst hinzufügen](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
3. Stellen Sie im Dialogfeld "Controller hinzufügen" die folgenden Optionen aus, und klicken Sie dann auf **hinzufügen**:

   - Modellklasse: **für Schüler und Studenten (ContosoUniversity.Models)**. (Wenn Sie diese Option in der Dropdown-Liste nicht angezeigt wird, erstellen Sie das Projekt, und versuchen Sie es erneut.)
   - Datenkontextklasse: **SchoolContext (ContosoUniversity.DAL)**.
   - Controllername: **StudentController** (nicht StudentsController).
   - Übernehmen Sie die Standardwerte für die anderen Felder aus.

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

     Beim Klicken auf **hinzufügen**, der gerüstbauer erstellt eine StudentController.cs-Datei und einen Satz von Ansichten (CSHTML-Dateien), die mit dem Controller verwendet. In der Zukunft verwendet werden, wenn Sie Projekte erstellen, die Verwendung von Entity Framework Außerdem profitieren Sie von einigen zusätzlichen Funktionen von der gerüstbauer: einfach erstellen Ihrer ersten Modellklasse, erstellen Sie eine Verbindungszeichenfolge keine, und klicken Sie dann in der **-Controllerhinzufügen** Feld angeben, neue Context-Klasse. Der gerüstbauer erstellt Ihre `DbContext` string-Klasse und Ihre Verbindung sowie die Controller und Ansichten.
4. Visual Studio öffnet die *Controllers\StudentController.cs* Datei. Sie sehen, dass eine Klassenvariable erstellt wurde, das ein Datenbank-Context-Objekt instanziiert:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     Die `Index` Action-Methode ruft eine Liste von Schülern und Studenten die *Schüler/Studenten* Entitätenmenge durch Lesen der `Students` Eigenschaft des Context-Instanz der Datenbank:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     Die *Student\Index.cshtml* zeigt diese Liste in einer Tabelle:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. Drücken Sie STRG+F5, um das Projekt auszuführen. (Wenn Sie eine Fehlermeldung "Der Schattenkopie kann nicht erstellt werden" erhalten, schließen Sie den Browser, und versuchen Sie es erneut.)

     Klicken Sie auf die **Schüler/Studenten** Tab, um die Testdaten abzurufen, die die `Seed` -Methode eingefügt wurden. Je nachdem, wie eng ist das Browserfenster, sehen Sie den Link "Student"-Registerkarte in der Top-Adressleiste ein, oder müssen Sie auf der oberen rechten Ecke, um den Link finden Sie unter.

     ![Menütaste](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![Indexseite "Student"](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Abrufen der Datenbank

Wenn Sie die Seite für Studenten ausgeführt, und die Anwendung hat versucht, auf die Datenbank zugreifen, sah EF, dass keine Datenbank vorhanden war und damit sie eine erstellt haben, klicken Sie dann die Seed-Methode Ausführung, um die Datenbank mit Daten aufzufüllen.

Verwenden Sie entweder **Server-Explorer** oder **Objekt-Explorer von SQL Server** (SSOX), um die Datenbank in Visual Studio anzuzeigen. In diesem Tutorial verwenden Sie **Server-Explorer**. (In Visual Studio Express-Editionen älter als 2013 **Server-Explorer** heißt **Datenbank-Explorer**.)

1. Schließen Sie den Browser.
2. In **Server-Explorer**, erweitern Sie **Datenverbindungen**, erweitern Sie **"School"-Kontext (ContosoUniversity)**, und erweitern Sie dann **Tabellen** , finden Sie unter die Tabellen in der neuen Datenbank.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. Mit der rechten Maustaste die **für Schüler und Studenten** Tabelle, und klicken Sie auf **Tabellendaten anzeigen** , finden Sie unter den Spalten, die erstellt wurden und die Zeilen, die in die Tabelle eingefügt wurden.

    ![Tabelle "Student"](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. Schließen der **Server-Explorer** Verbindung.

Die *ContosoUniversity1.mdf* und *ldf* -Datenbankdateien befinden sich in der `C:\Users\<yourusername>` Ordner.

Da Sie verwenden die `DropCreateDatabaseIfModelChanges` Initialisierer, können Sie nun eine Änderung vornehmen der `Student` Klasse, die Anwendung erneut auszuführen und die Datenbank automatisch Ihren Änderungen entsprechend neu erstellt werden würde. Angenommen, Sie fügen eine `EmailAddress` Eigenschaft, um die `Student` Klasse, führen Sie die Seite für Studenten erneut aus, und suchen Sie dann in der Tabelle erneut, sehen Sie ein neues `EmailAddress` Spalte.

## <a name="conventions"></a>Konventionen

Die Menge des Codes, die Sie in der Reihenfolge für das Entity Framework, um eine vollständige Datenbank erstellen zu können schreiben musste ist minimal, durch die Verwendung von *Konventionen*, oder die Annahmen, die Entity Framework verwendet werden. Einige von ihnen haben bereits erwähnt wurde, oder verwendet wurden, ohne Ihre kennen:

- Die pluralisierte Formen der Entity-Klassennamen werden als Tabellennamen verwendet.
- Eigenschaftennamen von Entitäten werden als Spaltennamen verwendet.
- Eigenschaften der Entität mit dem Namen `ID` oder *Classname* `ID` werden als Primärschlüsseleigenschaften erkannt.
- Eine Eigenschaft wird als Fremdschlüsseleigenschaften interpretiert, wenn sie den Namen *&lt;navigationseigenschaftennamen&gt;&lt;Primärschlüsseleigenschaft Namen&gt;* (z. B. `StudentID` für die `Student` Navigationseigenschaft, da die `Student` Primärschlüssel der Entität ist `ID`). Fremdschlüsseleigenschaften können auch einfach den Namen der gleiche &lt;Primärschlüsseleigenschaft Namen&gt; (z. B. `EnrollmentID` seit der `Enrollment` Primärschlüssel der Entität ist `EnrollmentID`).

Sie haben gesehen, dass die Konventionen überschrieben werden können. Beispielsweise angegeben werden kann, dass Tabellennamen im Plural werden darf nicht aus, und Sie später feststellen, wie Sie eine Eigenschaft als eine Fremdschlüsseleigenschaft explizit zu kennzeichnen. Erfahren Sie mehr über Konventionen und überschreiben sie in der [erstellen eine weitere komplexe Datenmodell](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) Tutorial weiter unten in dieser Serie. Weitere Informationen zu Konventionen finden Sie unter [Code First-Konventionen](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Zusammenfassung

Sie haben nun eine einfache Anwendung erstellt, die das Entity Framework und SQL Server Express LocalDB zum Speichern und Anzeigen von Daten verwendet. Im folgenden Tutorial erfahren Sie, wie zum Ausführen von grundlegenden CRUD (erstellen, lesen, aktualisieren und löschen) Vorgänge.

Lassen Sie Feedback auf, wie Sie in diesem Tutorial gefallen hat und was wir verbessern können. Sie können auch neue Themen anfordern [anzeigen Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links zu anderen Ressourcen des Entity Framework finden Sie im [ASP.NET-Datenzugriff – empfohlene Ressourcen](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Nächste](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
