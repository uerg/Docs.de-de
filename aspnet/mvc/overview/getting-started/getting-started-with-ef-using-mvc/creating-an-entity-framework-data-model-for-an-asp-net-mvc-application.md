---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Erste Schritte mit Entity Framework 6 Code First mit MVC 5 | Microsoft Docs
author: tdykstra
description: 'Eine neuere Version dieser Reihe von Lernprogrammen steht: Erste Schritte mit ASP.NET Core und Entity Framework Core mit Visual Studio 2015. Der Contoso-Universi...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/22/2015
ms.topic: article
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 46f53279e2e6daa4266c06feb4ba544e14b68a03
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>Erste Schritte mit Entity Framework 6 Code First unter Verwendung von MVC 5
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > Eine neuere Version dieser Reihe von Lernprogrammen zur Verfügung steht: [erste Schritte mit ASP.NET Core und Entity Framework Core mit Visual Studio 2015](https://docs.asp.net/en/latest/data/ef-mvc/intro.html).
> 
> 
> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 und Visual Studio 2013. Dieses Lernprogramm verwendet den Code First-Workflow. Weitere Informationen dazu, wie Sie zwischen Code First, Database First und Model First auswählen, finden Sie unter [Entity Framework-Entwicklungsworkflows](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> Die beispielanwendung ist eine Website für eine fiktive Contoso-Universität. Es umfasst Funktionen wie Student Zulassung, Kurs Erstellung und Instructor-Zuweisungen. Diese Reihe von Lernprogrammen wird erläutert, wie die Contoso University-beispielanwendung zu erstellen. Sie können [herunterladen die fertigen Anwendung](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8).
> 
> Steht eine Visual Basic-Version von Mike Brind übersetzt: [MVC 5 mit EF 6 in Visual Basic](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) auf der Website Mikesdotnetting.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Entity Framework 6 (EntityFramework 6.1.0 NuGet-Paket)
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) (optional)
>   
> 
> Das Lernprogramm sollte auch funktionieren mit [Visual Studio-2013 Express für Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) oder Visual Studio 2012. Die [VS 2012-Version von Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323511) für Windows Azure-Bereitstellung mit Visual Studio 2012 ist erforderlich.
> 
> 
> ## <a name="tutorial-versions"></a>Lernprogramm-Versionen
> 
> Für frühere Versionen dieses Lernprogramm finden Sie unter [der EF-4.1 / MVC 3-e-Book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC) und [erste Schritte mit EF 5 mit MVC 4](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).
> 
> ## <a name="questions-and-comments"></a>Fragen und Kommentare
> 
> Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir in den Kommentaren am unteren Rand der Seite verbessern können. Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx), [Entity Framework und LINQ to Entities-Forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), oder [ StackOverflow.com](http://stackoverflow.com/).
> 
> Wenn Sie ein Problem, die Sie nicht beheben können auftreten, im Allgemeinen finden Sie die Lösung für das Problem durch Vergleichen des Codes des abgeschlossenen Projekts, das Sie herunterladen können. Einige häufige Fehler und deren Lösung finden Sie unter [häufige Fehler und -Lösungen oder problemumgehungen für sie.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>Die Contoso-University-Webanwendung

Die Anwendung, die Sie in diesen Lernprogrammen erstellen werden müssen ist eine einfache University-Website.

Benutzer können anzeigen und Studenten, Kurs und Instructor-Informationen aktualisieren. Hier sind einige der Bildschirme, die Sie erstellen müssen.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![Student bearbeiten](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Den Stil der Benutzeroberfläche von diesem Standort wurde in der Nähe was von den integrierten Vorlagen generiert wird beibehalten, damit das Lernprogramm in erster Linie für die Verwendung von Entity Framework konzentrieren kann.

## <a name="prerequisites"></a>Erforderliche Komponenten

Finden Sie unter **Softwareversionen** am oberen Rand der Seite. Entity Framework 6 ist eine Voraussetzung nicht, da das EF NuGet-Paket als Teil des Lernprogramms zu installieren.

## <a name="create-an-mvc-web-application"></a>Erstellen einer MVC-Webanwendung

Öffnen Sie Visual Studio, und erstellen Sie ein neues c#-Projekt mit dem Namen "ContosoUniversity".

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

In der **neues ASP.NET-Projekt** aktivieren Sie im Dialogfeld die **MVC** Vorlage.

Wenn die **Host in der Cloud** Kontrollkästchen in der **Microsoft Azure** Abschnitt ausgewählt ist, deaktivieren sie das Kontrollkästchen.

Klicken Sie auf **ändern Authentifizierung**.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

In der **Authentifizierung ändern** wählen Sie im Dialogfeld **keine Authentifizierung**, und klicken Sie dann auf **OK**. Für dieses Lernprogramm wird nicht Sie Benutzer auffordern, melden Sie sich oder Einschränken des Zugriffs basierend auf den angemeldeten Benutzer sein.

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

Klicken Sie im Dialogfeld Neues ASP.NET-Projekt klicken Sie auf **OK** zum Erstellen des Projekts.

## <a name="set-up-the-site-style"></a>Richten Sie den Website-Stil

Einige einfache Änderungen werden das Menü "Vorschauwebsite", die Layout und die Startseite einrichten.

Open *Views\Shared\\_Layout.cshtml*, und nehmen Sie die folgenden Änderungen:

- Jedes Vorkommen von "Meine ASP.NET-Anwendung" und "Anwendungsname" in "Contoso University" ändern.
- Fügen Sie im Menüeinträge für Studenten, Bildungseinrichtungen Kurse, könne Dozenten und Abteilungen, und löschen Sie den Eintrag wenden Sie sich an.

Die Änderungen werden hervorgehoben.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

In *Views\Home\Index.cshtml*, ersetzen Sie den Inhalt der Datei mit den folgenden Code hinzu, ersetzt wechseln den Text zu ASP.NET und MVC mit Text zu dieser Anwendung:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

Drücken Sie STRG + F5, um die Website ausgeführt. Sie finden Sie auf der Startseite mit im Hauptmenü.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>Installieren von Entity Framework 6

Aus der **Tools** klicken Sie im Menü **NuGet Package Manager** , und klicken Sie dann auf **Package Manager Console**.

In der **Package Manager Console** Fenster geben Sie den folgenden Befehl aus:

`Install-Package EntityFramework`

![EF installiert](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

Die Abbildung zeigt 6.0.0 installiert wird, aber NuGet installiert die neueste Version von Entity Framework (ausgenommen Vorabversionen), die zum Zeitpunkt der letzten Aktualisierung, um das Lernprogramm 6.1.1 ist.

Dieser Schritt ist einer der wenigen Schritten, dass für dieses Lernprogramm sind Sie manuell durchführen, aber die wurden automatisch durch das ASP.NET MVC-Gerüstbau-Feature durchgeführt konnte. Sie tun sie manuell, damit Sie die erforderlichen Schritte zur Verwendung von Entity Framework sehen können. Sie können Gerüstbau später zum Erstellen von MVC-Controller und Ansichten verwenden. Eine Alternative besteht, lassen Gerüstbau automatisch das EF NuGet-Paket installieren, Erstellen der Datenbank Context-Klasse und die Verbindungszeichenfolge zu erstellen. Wenn Sie zu diesem Zweck auf diese Weise bereit sind, müssen Sie lediglich lediglich können Sie diese Schritte überspringen und zu Ihrer MVC-Controller zu erstellen, nachdem Sie die Entitätsklassen erstellt.

## <a name="create-the-data-model"></a>Erstellen des Datenmodells

Als Nächstes erstellen Sie für die Anwendung von Contoso University Entitätsklassen. Beginnen Sie mit den folgenden drei Entitäten:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Es wird eine 1: n-Beziehung zwischen `Student` und `Enrollment` Entitäten, und es wird eine 1: n-Beziehung zwischen `Course` und `Enrollment` Entitäten. Das heißt, eine Student kann in einer beliebigen Anzahl von Kursen registriert sein, und ein Kurs kann eine beliebige Anzahl der Schüler in es registriert haben.

In den folgenden Abschnitten erstellen Sie eine Klasse für jede dieser Entitäten.

> [!NOTE]
> Wenn Sie versuchen, das Projekt kompiliert werden, bevor Sie alle diese Entitätsklassen erstellt haben, erhalten Sie Compiler-Fehler.


### <a name="the-student-entity"></a>Die Student-Entität

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

In der *Modelle* Ordner, erstellen Sie eine Klassendatei namens *Student.cs* , und Ersetzen Sie den Code durch den folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

Die `ID` Eigenschaft werden die Primärschlüsselspalte der Datenbanktabelle, die diese Klasse entspricht. Wird standardmäßig das Entity Framework interpretiert eine Eigenschaft mit dem Namen `ID` oder *Classname* `ID` als Primärschlüssel.

Die `Enrollments` Eigenschaft ist ein *Navigationseigenschaft*. Navigationseigenschaften halten andere Entitäten, die mit dieser Entität verknüpft sind. In diesem Fall die `Enrollments` Eigenschaft eine `Student` Entität enthält alle der `Enrollment` , die verbundenen, Entitäten `Student` Entität. In anderen Worten: Wenn eine angegebene `Student` Zeile in der Datenbank verfügt über zwei im Zusammenhang `Enrollment` Zeilen (Zeilen mit Primärschlüssel, Student Wert in ihre `StudentID` foreign Key-Spalte), die `Student` Entität `Enrollments` Navigationseigenschaft enthält diese zwei `Enrollment` Entitäten.

Navigationseigenschaften werden in der Regel als definiert `virtual` , damit sie bestimmte Entity Framework-Funktionen wie z. B. nutzen können *verzögertes Laden*. (Lazy Loading werden erläutert werden weiter unten in der [das Lesen verknüpfter Daten](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) Tutorial weiter unten in dieser Serie.)

Wenn eine Navigationseigenschaft über mehrere Entitäten (wie in der m: n- oder 1: n-Beziehungen) enthalten kann, muss dessen Typ eine Liste, in dem Einträge können hinzugefügt, gelöscht, und aktualisiert, z. B. `ICollection`.

### <a name="the-enrollment-entity"></a>Die Registrierung-Entität

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

In der *Modelle* Ordner erstellen *Enrollment.cs* und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

Die `EnrollmentID` -Eigenschaft wird der Primärschlüssel sein; diese Entität verwendet die *Classname* `ID` Muster anstelle von `ID` von sich selbst als Sie gesehen haben, der `Student` Entität. Sie würden normalerweise wählen Sie ein Muster und verwenden sie während des gesamten Datenmodells. Hier veranschaulicht die Variation an, dass Sie entweder Muster verwenden können. Sehen Sie in einem späteren Lernprogramm Verwendung `ID` ohne `classname` erleichtert das Implementieren der Vererbung im Datenmodell.

Die `Grade` Eigenschaft ist ein [Enum](https://msdn.microsoft.com/data/hh859576.aspx). Das Fragezeichen nach der `Grade` Typdeklaration gibt an, dass die `Grade` Eigenschaft ist [NULL-Werte zulassen](https://msdn.microsoft.com/library/2cf62fcy.aspx). Eine Klasse, die null ist unterscheidet sich von einer NULL Grade – Null bedeutet, dass eine Klasse ist nicht bekannt oder noch nicht noch zugewiesen wurde.

Die `StudentID` Eigenschaft ist ein Fremdschlüssel, und die entsprechende Navigationseigenschaft ist `Student`. Ein `Enrollment` Entität bezieht sich auf eine `Student` Entität, damit die Eigenschaft nur ein einzelnes enthalten darf `Student` Entität (im Gegensatz zu den `Student.Enrollments` Navigationseigenschaft gelernt früher mehrere halten `Enrollment` Entitäten).

Die `CourseID` Eigenschaft ist ein Fremdschlüssel, und die entsprechende Navigationseigenschaft ist `Course`. Ein `Enrollment` Entität ist mit einem zugeordneten `Course` Entität.

Entity Framework interpretiert eine Eigenschaft als eine foreign Key-Eigenschaft, wenn es heißt  *&lt;navigationseigenschaftennamen&gt;&lt;Primärschlüsseleigenschaft Namen&gt;*  (z. B. `StudentID`für die `Student` Navigationseigenschaft seit der `Student` primären Schlüssel der Entität ist `ID`). Fremdschlüsseleigenschaften können auch den gleichen Namen einfach  *&lt;Primärschlüsseleigenschaft Namen&gt;*  (z. B. `CourseID` seit der `Course` primären Schlüssel der Entität ist `CourseID`).

### <a name="the-course-entity"></a>Die Kurs-Entität

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

In der *Modelle* Ordner erstellen *Course.cs*, ersetzen den Code durch den folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Die `Enrollments` Eigenschaft ist eine Navigationseigenschaft. Ein `Course` Entität kann sich auf eine beliebige Anzahl von beziehen `Enrollment` Entitäten.

Z. B. Weitere Informationen zu den [DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx) Attribut in einem späteren Lernprogramm dieser Reihe. Im Grunde, Sie können dieses Attribut geben Sie den primären Schlüssel für den Kurs, anstatt die Datenbank, die sie generieren.

## <a name="create-the-database-context"></a>Erstellen Sie den Datenbankkontext

Ist die Hauptklasse, das Entity Framework-Funktionen für einen angegebenen Datenmodell koordiniert die *Datenbankkontext* Klasse. Erstellen Sie diese Klasse durch Ableiten von der [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) Klasse. Im Code Geben Sie an, welche Entitäten im Datenmodell enthalten sind. Sie können auch bestimmte Entity Framework-Verhalten anpassen. In diesem Projekt die Klasse heißt `SchoolContext`.

Zum Erstellen eines Ordners im Projekt ContosoUniversity Maustaste das Projekt im **Projektmappen-Explorer** , und klicken Sie auf **hinzufügen**, und klicken Sie dann auf **neuer Ordner**. Nennen Sie diesen Ordner *DAL* (für die Datenzugriffsebene). In diesem Ordner erstellen Sie eine neue Klassendatei mit dem Namen *SchoolContext.cs*, und Ersetzen Sie den Code durch den folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>Entitätenmengen angibt.

Dieser Code erstellt ein [DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) Eigenschaft für jede Entitätenmenge. In der Terminologie von Entity Framework ein *Entitätenmenge* entspricht normalerweise einer Datenbanktabelle und ein *Entität* entspricht einer Zeile in der Tabelle.

> [!NOTE] 
> 
> Haben Sie ausgelassen der `DbSet<Enrollment>` und `DbSet<Course>` -Anweisungen, und es würde funktionieren identisch. Das Entity Framework zählen sie implizit da die `Student` Entitätsverweise der `Enrollment` Entität und die `Enrollment` Entitätsverweise der `Course` Entität.


### <a name="specifying-the-connection-string"></a>Die Verbindungszeichenfolge anzugeben

Der Name der Verbindungszeichenfolge (die Sie der Datei "Web.config" später hinzufügen) wird an den Konstruktor übergeben.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

Sie könnten auch in der Verbindungszeichenfolge selbst anstelle des Namens einer übergeben, die in der Datei "Web.config" gespeichert ist. Weitere Informationen zu Optionen für die Angabe der Datenbank zu verwenden, finden Sie unter [Entity Framework - Verbindungen und Modelle](https://msdn.microsoft.com/data/jj592674).

Wenn Sie eine Verbindungszeichenfolge oder den Namen einer explizit angeben, wird Entity Framework davon ausgegangen, dass der Name der Verbindungszeichenfolge mit dem Klassennamen identisch ist. Klicken Sie dann den Namen der Standardverbindungszeichenfolge in diesem Beispiel wäre `SchoolContext`, dem Sie explizit angeben können.

### <a name="specifying-singular-table-names"></a>Angeben der singular Tabellennamen

Die `modelBuilder.Conventions.Remove` -Anweisung in der [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) Methode verhindert, dass Tabellennamen wird pluralisiert. Wenn Sie dies nicht tun, würde die generierten Tabellen in der Datenbank benannt werden `Students`, `Courses`, und `Enrollments`. Stattdessen werden die Tabellennamen `Student`, `Course`, und `Enrollment`. Entwickler sind sich uneinig darüber, ob Tabellennamen im Plural stehen sollten oder nicht. In diesem Lernprogramm wird die Singularform verwendet, aber der wichtige Punkt ist, dass Sie unabhängig davon, welche Form Sie dann auswählen können durch ein- oder dieser Zeile des Codes auslassen.

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>Einrichten von EF zum Initialisieren der Datenbank mit Testdaten

Das Entity Framework können automatisch erstellt (oder drop Neuerstellen und) eine Datenbank für die Sie beim Ausführen der Anwendung. Sie können angeben, dass dies geschehen, sollte jedes Mal, wenn die Anwendung ausgeführt wird, oder nur, wenn das Modell nicht mit der vorhandenen Datenbank synchronisiert ist. Sie können auch Schreiben einer `Seed` Methode, die das Entity Framework ruft automatisch nach dem Erstellen der Datenbank, um sie mit Testdaten auszustatten.

Das Standardverhalten ist zum Erstellen einer Datenbank nur dann, wenn sie nicht vorhanden ist (und löst eine Ausnahme aus, wenn das Modell geändert wurde und die Datenbank bereits vorhanden ist). In diesem Abschnitt geben Sie an, dass die Datenbank gelöscht und neu erstellt werden, wenn das Modell ändert. Löschen der Datenbank bewirkt, dass alle Ihre Daten verloren gehen. Dies ist im Allgemeinen OK während der Entwicklung, da die `Seed` Methode wird ausgeführt, wenn die Datenbank neu erstellt wird und die Testdaten werden neu erstellt. In der Produktion in der Regel möchten jedoch nicht alle Ihre Daten zu verlieren, jedes Mal, wenn Sie das Schema der Datenbank ändern müssen. Weiter unten sehen Sie, wie modelländerungen behandeln, indem Sie Code First-Migrationen verwenden, um das Datenbankschema statt löschen und Neuerstellen der Datenbank zu ändern.

Erstellen Sie eine neue Klassendatei mit dem Namen im Ordner "DAL" *SchoolInitializer.cs* , und Ersetzen Sie den Code mit der  
folgenden Code, wodurch eine Datenbank erstellt werden benötigt, und lädt Testdaten in die neue Datenbank.

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

Die `Seed` Methode akzeptiert die Datenbank Context-Objekt als Eingabeparameter, und der Code in der Methode verwendet  
Dieses Objekt an der Datenbank neue Entitäten hinzugefügt werden soll. Für jeden Entitätstyp im Code erstellt eine Auflistung von neuen  
 Entitäten, fügt diese an die entsprechende `DbSet` -Eigenschaft, und klicken Sie dann speichert die Änderungen an der Datenbank. Es ist nicht  
zum Aufrufen der `SaveChanges` Methode nach jeder Gruppe von Entitäten, wie hier jedoch dazu verwendet werden, die der Identifikation  
Sie finden die Ursache eines Problems, wenn eine Ausnahme auftritt, während der Code in die Datenbank geschrieben werden.

Um Ihre Initialisiererklasse Verwenden von Entity Framework aufzufordern, hinzufügen ein Elements, dass die `entityFramework` Element in der Anwendung *"Web.config"* (die eine Datei im Stammordner des Projekts), wie im folgenden Beispiel gezeigt:

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

Die `context type` gibt den Kontext vollqualifizierten Klassennamen und die Assembly befindet sich im, und die `databaseinitializer type` gibt den vollqualifizierten Namen der Initialisiererklasse und die Assembly befindet sich im. (Wenn Sie keine EF Initialisierer verwenden möchten, können Sie ein Attribut festlegen, auf die `context` Element: `disableDatabaseInitialization="true"`.) Weitere Informationen finden Sie unter [Entity Framework - Config-Dateieinstellungen](https://msdn.microsoft.com/data/jj556606).

Als Alternative zum Festlegen des Initialisierers in der *"Web.config"* Datei wird im Code dafür, durch Hinzufügen einer `Database.SetInitializer` Anweisung, um die `Application_Start` Methode in der *Global.asax.cs* Datei. Weitere Informationen finden Sie unter [Grundlegendes zu Datenbank-Initialisierer in Entity Framework Code First](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm).

Die Anwendung ist nun so festgelegt wird, damit beim der Datenbank zum ersten Mal in einer bestimmten Ausführung Zugriff auf die  
Anwendung, das Entity Framework vergleicht die Datenbank für das Modell (Ihre `SchoolContext` und der Entitätsklassen). Wenn ein Unterschied vorhanden ist, wird die Anwendung gelöscht und neu erstellt die Datenbank.

> [!NOTE]
> Wenn Sie eine Anwendung auf einem Webserver für die Produktion bereitstellen, müssen Sie entfernen oder deaktivieren Code, der gelöscht und die Datenbank neu erstellt. Sie müssen dies tun, in einem späteren Lernprogramm dieser Reihe.


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>Einrichten von EF verwenden eine SQL Server Express LocalDB-Datenbank

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) ist eine vereinfachte Version von SQL Server Express-Datenbankmoduls. Es ist einfach zu installieren und konfigurieren, wird bei Bedarf gestartet und im Benutzermodus ausgeführt wird. LocalDB ausgeführt wird, in eine spezielle Ausführungsmodus von SQL Server Express, die Ihnen ermöglicht, die Arbeit mit Datenbanken als *mdf* Dateien. Sie können LocalDB-Datenbankdateien einfügen, der *App\_Daten* Ordner eines Webprojekts, wenn die Datenbank mit dem das Projekt kopiert werden sollen. Die Benutzerinstanzfunktion in SQL Server Express können Sie zur Bearbeitung auch *mdf* Dateien, aber der Benutzerinstanzfunktion wird als veraltet markiert; daher LocalDB wird empfohlen, für die Arbeit mit *mdf* Dateien. In Visual Studio 2012 und höheren Versionen ist LocalDB, die standardmäßig mit Visual Studio installiert.

SQL Server Express wird in der Regel für die Produktion Webanwendungen nicht verwendet. LocalDB ist mit einer Webanwendung für die Produktion insbesondere nicht empfohlen, da es nicht zum Arbeiten mit IIS ausgelegt ist.

In diesem Lernprogramm arbeiten Sie mit LocalDB. Öffnen Sie die Anwendung *"Web.config"* und fügen eine `connectionStrings` Element vor dem `appSettings` Element, wie im folgenden Beispiel dargestellt. (Stellen Sie sicher, dass Sie aktualisieren die *"Web.config"* -Datei im Stammordner des Projekts. Es gibt auch eine *"Web.config"* Datei befindet sich in der *Ansichten* Unterordner, die Sie nicht aktualisieren müssen.)

Bei Verwendung von Visual Studio 2015 ersetzen Sie "V11. 0" in der Verbindungszeichenfolge mit "MSSQLLocalDB", wie die standardmäßigen SQL Server-Instanznamen geändert hat.

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

Die Verbindungszeichenfolge, die Sie hinzugefügt haben gibt an, dass Entity Framework eine LocalDB-Datenbank mit dem Namen verwendet *ContosoUniversity1.mdf*. (Die Datenbank nicht noch vorhanden. EF wird erstellt.) Mussten Sie die Datenbank zu erstellende Ihrer *App\_Daten* Ordner hinzufügen `AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf` zur Verbindungszeichenfolge hinzufügen. Weitere Informationen zu Verbindungszeichenfolgen finden Sie unter [SQL Server-Verbindungszeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx).

Sie müssen nicht unbedingt, dass eine Verbindungszeichenfolge in der *"Web.config"* Datei. Wenn Sie eine Verbindungszeichenfolge angeben, wird Entity Framework den Standardwert verwenden eine auf Ihrer Context-Klasse basierend. Weitere Informationen finden Sie unter [Code First in eine neue Datenbank](https://msdn.microsoft.com/data/jj193542).

## <a name="creating-a-student-controller-and-views"></a>Ein Student-Controller und Ansichten werden erstellt

Nun erstellen Sie eine Webseite, um Daten anzuzeigen, und der Prozess für das Anfordern von Daten wird automatisch ausgelöst.  
die Erstellung der Datenbank. Sie beginnen, indem Sie einen neuen Domänencontroller erstellen. Aber bevor Sie dies tun, erstellen Sie das Projekt, um das Modell und dem angegebenen Kontext Klassen für Gerüstbau für MVC-Controller verfügbar zu machen.

1. Mit der rechten Maustaste die **Controller** Ordner **Projektmappen-Explorer**Option **hinzufügen**, und klicken Sie dann auf **neues Gerüstelement**.
- In der **Gerüst hinzufügen** wählen Sie im Dialogfeld **MVC 5-Controller mit Ansichten unter Verwendung von Entity Framework**.

    ![Gerüst hinzufügen](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
- Stellen Sie im Dialogfeld "Controller hinzufügen" die folgenden Optionen aus, und klicken Sie dann auf **hinzufügen**:

    - Modellschemas: **Student (ContosoUniversity.Models)**. (Wenn diese Option in der Dropdown-Liste nicht angezeigt wird, erstellen Sie das Projekt, und versuchen Sie es erneut.)
    - Datenkontextklasse: **SchoolContext (ContosoUniversity.DAL)**.
    - Controllername: **StudentController** (nicht StudentsController).
    - Lassen Sie die Standardwerte für die anderen Felder ein.

    ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    Beim Klicken auf **hinzufügen**, die Scaffolder erstellt eine StudentController.cs-Datei und einen Satz von Sichten (CSHTML-Dateien), die mit dem Controller. In der Zukunft, beim Erstellen von Projekten, die Verwendung von Entity Framework auch profitieren Sie von einigen zusätzlichen Funktionen von der Scaffolder: einfach erstellen Ihrer ersten Modellklasse, erstellen Sie eine Verbindungszeichenfolge keine und dann in der **"Controller hinzufügen"** Feld angeben, neue Context-Klasse. Erstellt die Scaffolder Ihrer `DbContext` Klasse und Ihre Verbindung eine Zeichenfolge sowie den Controller und Ansichten.
- Visual Studio öffnet die *Controllers\StudentController.cs* Datei. Sie sehen, dass eine Klassenvariable erstellt wurde, die eine Datenbank Context-Objekt instanziiert:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    Die `Index` Aktionsmethode ruft eine Liste der Schüler der *Studenten* Entitätenmenge durch Lesen der `Students` Eigenschaft des Context-Datenbankinstanz:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Die *Student\Index.cshtml* zeigt diese Liste in einer Tabelle:

    [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
- Drücken Sie STRG+F5, um das Projekt auszuführen. (Wenn Sie eine Fehlermeldung "Der Schattenkopie kann nicht erstellt werden" erhalten, schließen Sie den Browser, und versuchen Sie es erneut.)

    Klicken Sie auf die **Studenten** Tab, um die Testdaten finden Sie unter, die die `Seed` Methode eingefügt. Je nachdem, wie eng ist Ihres Browserfensters, sehen Sie den Link in der oberen Adressleiste Student-Registerkarte oder stehen Ihnen auf die obere rechte Ecke, um den Link klicken.

    ![Menüschaltfläche](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    ![Student-Indexseite](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>Zeigen Sie die Datenbank an.

Wenn Sie die Seite "Students" ausgeführt, und die Anwendung hat versucht, auf die Datenbank zugreifen, gesehen haben EF, es keine Datenbank wurde, und so erstellt, klicken Sie dann die Ausführung der Seed-Methode gedauert, um die Datenbank mit Daten aufzufüllen.

Verwenden Sie entweder **Server-Explorer** oder **Objekt-Explorer von SQL Server** (SSOX) zur Anzeige der Datenbank in Visual Studio. Verwenden Sie für dieses Lernprogramm **Server-Explorer**. (In Visual Studio Express-Editionen vor 2013 **Server-Explorer** heißt **Datenbank-Explorer**.)

1. Schließen Sie den Browser.
2. In **Server-Explorer**, erweitern Sie **Datenverbindungen**, erweitern Sie **School-Kontext (ContosoUniversity)**, und erweitern Sie dann **Tabellen** , finden Sie unter die Tabellen in der neuen Datenbank.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. Mit der rechten Maustaste die **Student** Tabelle, und klicken Sie auf **Tabellendaten anzeigen** , finden in den Spalten, die erstellt wurden und die Zeilen, die in die Tabelle eingefügt wurden.

    ![Studententabelle](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. Schließen der **Server-Explorer** Verbindung.

Die *ContosoUniversity1.mdf* und *ldf* -Datenbankdateien befinden sich in der `C:\Users\<yourusername>` Ordner.

Da Sie verwenden die `DropCreateDatabaseIfModelChanges` Initialisierer, konnte Sie jetzt eine Änderung vornehmen der `Student` Klasse, die Anwendung erneut auszuführen und die Datenbank automatisch neu erstellt, die Änderung entsprechend wäre. Angenommen, Sie fügen ein `EmailAddress` Eigenschaft, um die `Student` -Klasse, führen Sie die Seite "Students" erneut aus, und suchen Sie dann in die Tabelle erneut, sehen Sie ein neues `EmailAddress` Spalte.

## <a name="conventions"></a>Konventionen

Die Menge an Code in Reihenfolge für das Entity Framework voraussetzten können zum Erstellen einer vollständigen Datenbank ist aufgrund der Verwendung von minimal *Konventionen*, oder die Annahmen, die das Entity Framework stellt. Einige von ihnen bereits beobachtet wurden oder verwendet wurden, ohne dass bekannt:

- Die pluralized Formen von Namen für die Entität werden als Tabellennamen verwendet.
- Entitätsnamen-Eigenschaft werden für Spaltennamen verwendet.
- Eigenschaften der Entität mit dem Namen sind `ID` oder *Classname* `ID` werden als Eigenschaften des Primärschlüssels erkannt.
- Eine Eigenschaft wird als eine Fremdschlüsseleigenschaft interpretiert, wenn es heißt  *&lt;navigationseigenschaftennamen&gt;&lt;Primärschlüsseleigenschaft Namen&gt;*  (z. B. `StudentID` für die `Student` Navigationseigenschaft seit der `Student` primären Schlüssel der Entität ist `ID`). Fremdschlüsseleigenschaften können auch den gleichen Namen einfach &lt;Primärschlüsseleigenschaft Namen&gt; (z. B. `EnrollmentID` seit der `Enrollment` primären Schlüssel der Entität ist `EnrollmentID`).

Sie haben gesehen, dass die Konventionen überschrieben werden können. Angenommen, Sie angegeben haben, dass Tabellennamen darf keine pluralisiert sein, und Sie später sehen wie eine Eigenschaft als eine Fremdschlüsseleigenschaft explizit markiert. Erfahren Sie mehr über Konventionen und überschreiben Sie diese in die [erstellen eine weitere komplexe Datenmodell](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) Tutorial weiter unten in dieser Serie. Weitere Informationen zu Konventionen finden Sie unter [Code First-Konventionen](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Zusammenfassung

Sie haben nun eine einfache Anwendung erstellt, die das Entity Framework und SQL Server Express LocalDB zum Speichern und Anzeigen von Daten verwendet. Im folgenden Lernprogramm erfahren Sie, zum Ausführen von grundlegenden CRUD (erstellen, lesen, aktualisieren und löschen) Vorgänge.

Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir weiter verbessern können. Sie können auch neue Themen am anfordern [Me wie mit Code anzeigen](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links zu anderen Entity Framework-Ressourcen finden Sie im [ASP.NET Data Access - Ressourcen empfohlen](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Nächste](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
