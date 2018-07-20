---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: Erstellen ein Entity Framework-Datenmodells für eine ASP.NET MVC-Anwendung (1 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Eine neuere Version dieser tutorialreihe ist für Visual Studio 2013, Entity Framework 6 und MVC 5 verfügbar. Der Contoso University-Beispiel Web Application de...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: ae9a4f0f13b01d8e093030bb1def2f21580a9e48
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815158"
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Erstellen ein Entity Framework-Datenmodells für eine ASP.NET MVC-Anwendung (1 von 10)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > Ein [neuere Version dieser tutorialreihe](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) für Visual Studio 2013, Entity Framework 6 und MVC 5 verfügbar ist.
> 
> 
> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 und Visual Studio 2012. Bei der Beispiel-App handelt es sich um eine Website für die fiktive Contoso University. Sie enthält Funktionen wie die Zulassung von Studenten, die Erstellung von Kursen und Aufgaben von Dozenten. Dieser tutorialreihe wird erläutert, wie die beispielanwendung der Contoso University. Sie können [die fertige Anwendung herunterladen](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Code First
> 
> Es gibt drei Möglichkeiten, die Sie mit Daten im Entity Framework arbeiten können: *Database First*, *Model First*, und *Code First*. Dieses Tutorial ist für Code First. Weitere Informationen zu den Unterschieden zwischen den Workflows und Anleitungen dazu, wie die beste Option für Ihr Szenario auswählen, finden Sie unter [Entity Framework-Entwicklungsworkflows](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> Die beispielanwendung basiert auf [ASP.NET MVC](../../../index.md). Wenn Sie mit dem ASP.NET Web Forms-Modell arbeiten möchten, finden Sie unter den [Modellbindung und Web Forms](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) Reihe von Tutorials und [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Software-Versionen
> 
> | **In diesem Tutorial gezeigt** | **Funktioniert auch mit** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio 2012 Express für Web. Dies wird automatisch vom Windows Azure SDK installiert, wenn Visual Studio 2012 oder Visual Studio 2012 Express für Web noch nicht vorhanden ist. Visual Studio 2013 sollten funktionieren, aber das Tutorial wurde dabei nicht getestet, und einige Menüauswahl und Dialogfelder unterscheiden sich. Die [VS 2013-Version von Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510) für Windows Azure-Bereitstellung erforderlich ist. |
> | .NET 4.5 | Die meisten der dargestellten Funktionen in .NET 4 funktioniert, aber einige wird nicht. Beispielsweise erfordert die Unterstützung von Enumerationen in EF .NET 4.5. |
> | Entitätsframework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Wenn Sie die Schritte zur Bereitstellung von Windows Azure überspringen, benötigen Sie nicht das SDK. Wenn eine neue Version des SDK veröffentlicht wird, wird der Link auf die neuere Version installieren. In diesem Fall müssen Sie möglicherweise einige der Anweisungen, um die neue Benutzeroberfläche und Features anpassen. |
> 
> ## <a name="questions"></a>Fragen
> 
> Wenn Sie Fragen, die nicht direkt mit dem Tutorial verknüpft sind haben, können Sie sie veröffentlichen das [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx), [Entity Framework und LINQ to Entities-Forum](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/), oder [ StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Danksagungen
> 
> Finden Sie im letzten Tutorial der Reihe für [Bestätigungen und ein Hinweis zu VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Ursprüngliche Version des Lernprogramms
> 
> Die ursprüngliche Version des Lernprogramms finden Sie in der [EF 4.1 / MVC 3-e-Book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).


## <a name="the-contoso-university-web-application"></a>Der Contoso University-Web-Anwendung

Bei der Anwendung, die Sie mithilfe dieser Tutorials erstellen, handelt es sich um eine einfache Universitätswebsite.

Benutzer können Informationen zu den Studenten, Kursen und Dozenten abrufen. Nachfolgend werden einige Anzeigen dargestellt, die erstellt werden sollen.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Der Benutzeroberflächenstil dieser Website orientiert sich an den integrierten Vorlagen, sodass Sie mit dieses Tutorial hauptsächlich auf die Verwendung von Entity Framework konzentriert.

## <a name="prerequisites"></a>Erforderliche Komponenten

Die Anweisungen und Screenshots in diesem Tutorial wird davon ausgegangen, dass Sie nutzen [Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) oder [Visual Studio-2012 Express für Web](https://go.microsoft.com/fwlink/?LinkID=275131), mit den neuesten Updates und Azure SDK für .NET ab Juli, installiert 2013. Sie können all dies mit dem folgenden Link abrufen:

[Azure SDK für .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Wenn Sie Visual Studio installiert haben, installiert der oben stehenden Link die fehlenden Komponenten. Wenn Sie Visual Studio nicht haben, wird der Link, Visual Studio-2012 Express für Web installieren. Sie können Visual Studio 2013 verwenden, aber einige der erforderlichen Vorgehensweisen und Bildschirme unterscheiden sich.

## <a name="create-an-mvc-web-application"></a>Erstellen einer MVC-Webanwendung

Öffnen Sie Visual Studio, und erstellen Sie ein neues c#-Projekt, mit dem Namen "ContosoUniversity" mithilfe der **ASP.NET MVC 4-Webanwendung** Vorlage. Stellen Sie sicher, dass Sie als Ziel **.NET Framework 4.5** (verwenden Sie [ `enum` Eigenschaften](https://msdn.microsoft.com/data/hh859576.aspx), erfordert .NET 4.5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

In der **neues ASP.NET MVC 4-Projekt** aktivieren Sie im Dialogfeld die **Internetanwendung** Vorlage.

Lassen Sie die **Razor** anzeigen Engine ausgewählt, und lassen Sie die **erstellen ein Komponententestprojekts** das Kontrollkästchen deaktiviert.

Klicken Sie auf **OK**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Einrichten des Websitestils

Sie können das Websitemenü, das Layout und die Startseite über einige Änderungen einrichten.

Open *Views\Shared\\"_Layout.cshtml"*, und Ersetzen Sie den Inhalt der Datei mit dem folgenden Code. Die Änderungen werden hervorgehoben.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Durch diesen Code werden folgende Änderungen vorgenommen:

- Ersetzt die Vorlageninstanzen "Meine ASP.NET MVC-Anwendung" und "Ihr Logo hier einfügen" mit "Contoso University".
- Fügt mehrere Links für Aktionen, die später in diesem Tutorial verwendet werden.

In *Views\Home\Index.cshtml*, ersetzen Sie den Inhalt der Datei mit dem folgenden Code, um die Vorlage Absätze zu ASP.NET und MVC zu vermeiden:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

In *Controllers\HomeController. cs*, ändern Sie den Wert für `ViewBag.Message` in die `Index` Aktionsmethode "Willkommen in Contoso University!", wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Drücken Sie STRG + F5, um die Website auszuführen. Die Startseite mit im Hauptmenü angezeigt.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Erstellen des Datenmodells

Als nächstes erstellen Sie Entitätsklassen für die Contoso University-Anwendung. Beginnen Sie mit dem folgenden drei Entitäten:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Es besteht eine 1:n-Beziehung zwischen den Entitäten `Student` und `Enrollment`. Außerdem besteht eine 1:n-Beziehung zwischen den Entitäten `Course` und `Enrollment`. Das bedeutet, dass ein Student für beliebig viele Kurse angemeldet sein kann und sich für jeden Kurs eine beliebige Anzahl von Studenten anmelden kann.

In den folgenden Abschnitten erstellen Sie für jede dieser Entitäten eine Klasse.

> [!NOTE]
> Wenn Sie versuchen, das Projekt zu kompilieren, vor dem Erstellen aller dieser Entitätsklassen, erhalten Sie Compilerfehler zu beheben.


### <a name="the-student-entity"></a>Die Entität "Student"

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

In der *Modelle* Ordner erstellen *Student.cs* , und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

Die `StudentID`-Eigenschaft fungiert als Primärschlüsselspalte der Datenbanktabelle, die dieser Klasse entspricht. Standardmäßig interpretiert das Entity Framework eine Eigenschaft mit dem Namen `ID` oder *Classname* `ID` als Primärschlüssel.

Die `Enrollments` -Eigenschaft ist eine *Navigationseigenschaft*. Navigationseigenschaften enthalten andere Entitäten, die dieser Entität zugehörig sind. In diesem Fall die `Enrollments` Eigenschaft eine `Student` Entität enthält alle der `Enrollment` Entitäten, die im Zusammenhang mit, `Student` Entität. Das heißt, wenn eine angegebene `Student` Zeile in der Datenbank verfügt über zwei im Zusammenhang `Enrollment` Zeilen (Zeilen, die Primärschlüssel des Studenten enthalten Wert in ihre `StudentID` Fremdschlüsselspalte), die von `Student` Entität `Enrollments` Navigationseigenschaft Diese beiden `Enrollment` Entitäten.

Navigationseigenschaften werden in der Regel als definiert `virtual` , damit sie bestimmte Entity Framework-Funktionen wie z. B. nutzen können *Lazy Load*. (Lazy Loading werden erläutert werden weiter unten in der [Lesen von verwandten Daten](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) Tutorial weiter unten in dieser Serie.

Wenn eine Navigationseigenschaft mehrere Entitäten enthalten kann (wie bei m:n- oder 1:n-Beziehungen), muss dessen Typ aus einer Liste bestehen, in der Einträge hinzugefügt, gelöscht und aktualisiert werden können – z.B.: `ICollection`.

### <a name="the-enrollment-entity"></a>Die Entität "Enrollment"

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

Erstellen Sie im Ordner *Models* (Modelle) die Datei *Enrollment.cs*, und ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Die Grade-Eigenschaft ist ein [Enum](https://msdn.microsoft.com/data/hh859576.aspx). Das Fragezeichen nach der `Grade` Typdeklaration gibt an, dass die `Grade` -Eigenschaft ist [auf NULL festlegbare](https://msdn.microsoft.com/library/2cf62fcy.aspx). Eine Grade-Eigenschaft, die null unterscheidet sich von einer 0 (null) auf Unternehmensniveau – Null bedeutet, dass keine Grade-Eigenschaft bekannt ist oder noch nicht zugewiesen wurde.

Bei der `StudentID`-Eigenschaft handelt es sich um einen Fremdschlüssel, und `Student` ist die entsprechende Navigationseigenschaft. Eine `Enrollment`-Entität wird einer `Student`-Entität zugeordnet, damit die Eigenschaft nur eine `Student`-Entität enthalten kann. Dies steht im Gegensatz zu der bereits erläuterten `Student.Enrollments`-Navigationseigenschaft, die mehrere `Enrollment`-Entitäten enthalten kann.

Bei der `CourseID`-Eigenschaft handelt es sich um einen Fremdschlüssel, und die zugehörige Navigationseigenschaft lautet `Course`. Die `Enrollment`-Entität wird einer `Course`-Entität zugeordnet.

### <a name="the-course-entity"></a>Die Entität "Course"

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

In der *Modelle* Ordner erstellen *Course.cs*, und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

Die `Enrollments`-Eigenschaft ist eine Navigationseigenschaft. `Course`-Entitäten können sich auf jede beliebige Anzahl von `Enrollment`-Entitäten beziehen.

Wir definieren Weitere Informationen zu den [["databasegenerated"](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). Keine)]-Attribut im nächsten Tutorial. Im Grunde können Sie über dieses Attribut den Primärschlüssel für den Kurs angeben, anstatt ihn von der Datenbank generieren zu lassen.

## <a name="create-the-database-context"></a>Erstellen des Datenbankkontexts

Ist die Hauptklasse, die Entity Framework-Funktionen für ein angegebenes Datenmodell koordiniert die *Datenbankkontext* Klasse. Erstellen Sie diese Klasse durch Ableiten von der [System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx) Klasse. Sie geben in Ihrem Code an, welche Entitäten im Datenmodell enthalten sind. Außerdem können Sie bestimmte Entity Framework-Verhalten anpassen. In diesem Projekt heißt die Klasse `SchoolContext`.

Erstellen Sie einen Ordner mit dem Namen *DAL* (für Data Access Layer). In diesem Ordner erstellen Sie eine neue Klassendatei mit dem Namen *SchoolContext.cs*, und Ersetzen Sie den vorhandenen Code durch den folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Dieser Code erstellt eine ["DbSet"](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx) -Eigenschaft für jede Entitätenmenge. In der Terminologie von Entity Framework eine *Entitätenmenge* entspricht in der Regel einer Datenbanktabelle und ein *Entität* entspricht einer Zeile in der Tabelle.

Die `modelBuilder.Conventions.Remove` -Anweisung in der ["onmodelcreating"](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) Methode verhindert, dass Tabellennamen im Plural wird. Wenn Sie dies nicht gemacht hätte, die generierten Tabellen hieße `Students`, `Courses`, und `Enrollments`. Stattdessen werden die Tabellennamen `Student`, `Course`, und `Enrollment`. Entwickler sind sich uneinig darüber, ob Tabellennamen im Plural stehen sollten oder nicht. In diesem Tutorial wird die Form im singular verwendet, aber der wichtigste Punkt ist, dass Sie, welcher Form Sie bevorzugen auswählen können, durch Einfügen oder diese Zeile des Codes auslassen.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) ist eine einfache Version von SQL Server Express-Datenbankmoduls, die wird bedarfsgesteuert gestartet und im Benutzermodus ausgeführt wird. LocalDB ausgeführt wird, in einem speziellen Ausführungsmodus von SQL Server Express, die Ihnen ermöglicht, die Arbeit mit Datenbanken als *mdf* Dateien. LocalDB-Datenbankdateien bleiben in der Regel der *App\_Daten* Ordner eines Webprojekts. Das Feature "Instanz" in SQL Server Express ermöglicht Ihnen die Arbeit mit auch *mdf* Dateien, aber das Feature "Instanz" ist veraltet; LocalDB wird daher empfohlen, für die Arbeit mit *mdf* Dateien.

SQL Server Express wird in der Regel für Produktions-Web-Anwendungen nicht verwendet. LocalDB wird insbesondere nicht mit einer Webanwendung für die Produktion empfohlen, da es nicht ausgelegt ist, arbeiten Sie mit IIS.

In Visual Studio 2012 und höher wird die LocalDB wird standardmäßig mit Visual Studio installiert. In Visual Studio 2010 und früheren Versionen ist das SQL Server Express (ohne LocalDB), die standardmäßig mit Visual Studio installiert. Sie müssen diese manuell installieren, wenn Sie Visual Studio 2010 verwenden.

In diesem Tutorial Sie arbeiten mit LocalDB, damit Sie in die Datenbank gespeichert werden kann die *App\_Daten* Ordner wie eine *mdf* Datei. Öffnen Sie die Stammkonfigurationsdatei *"Web.config"* -Datei und fügen Sie eine neue Verbindungszeichenfolge für die `connectionStrings` -Auflistung, wie im folgenden Beispiel gezeigt. (Stellen Sie sicher, dass Sie aktualisieren die *"Web.config"* -Datei in den Stammordner des Projekts. Es gibt auch eine *"Web.config"* Datei befindet sich in der *Ansichten* Unterordner, die Sie nicht aktualisieren müssen.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

Standardmäßig sucht die Entity Framework eine Verbindungszeichenfolge, die den gleichen Namen wie die `DbContext` Klasse (`SchoolContext` für dieses Projekt). Gibt die Verbindungszeichenfolge, die Sie hinzugefügt haben, an einer LocalDB-Datenbank, die mit dem Namen *ContosoUniversity.mdf* befindet sich in der *App\_Daten* Ordner. Weitere Informationen finden Sie unter [SQL Server-Verbindungszeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/library/jj653752.aspx).

Tatsächlich müssen Sie die Verbindungszeichenfolge angeben. Wenn Sie eine Verbindungszeichenfolge angeben, erstellt Entity Framework für Sie; allerdings die Datenbank möglicherweise nicht der *App\_Daten* Ordner der app. Weitere Informationen dazu, in dem die Datenbank erstellt wird, finden Sie unter [Code First in eine neue Datenbank](https://msdn.microsoft.com/data/jj193542).

Die `connectionStrings` Sammlung verfügt auch über eine Verbindungszeichenfolge, die mit dem Namen `DefaultConnection` die für die Mitgliedschaftsdatenbank verwendet wird. Sie wird nicht die Mitgliedschaftsdatenbank in diesem Tutorial verwenden. Der einzige Unterschied zwischen die beiden Verbindungszeichenfolgen ist der Name der Datenbank und den Name-Attributwert.

## <a name="set-up-and-execute-a-code-first-migration"></a>Einrichten und Ausführen von Code First-Migration

Beim ersten zur Entwicklung einer Anwendung Start, Ihre Daten Änderungen am Datenmodell häufig, und jedes Mal das Modell ändert, die sie nicht mehr synchron mit der Datenbank abruft. Sie können konfigurieren, dass das Entity Framework für das automatische Löschen und Neuerstellen der Datenbank jedes Mal, die Sie das Datenmodell ändern. Dies ist kein Problem früh in der Entwicklung, da Testdaten einfach neu erstellt, aber nachdem Sie in der produktionsumgebung bereitgestellt haben in der Regel Sie das Datenbankschema zu aktualisieren, ohne Löschen der Datenbank möchten. Das Migrationsfeature ermöglicht Code First die Datenbank zu aktualisieren, ohne Sie zu löschen und neu zu erstellen. Früh im Entwicklungszyklus eines neuen Projekts möchten Sie möglicherweise verwenden Sie [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx) zu löschen, neu erstellen und erneutes Seeding für die Datenbank jedes Mal, wenn das Modell ändert. Eine, sondern erhalten das bereit für die Bereitstellung Ihrer Anwendung können Sie bei der Migration konvertieren. In diesem Tutorial verwenden Sie nur Migrationen. Weitere Informationen finden Sie unter [Code First-Migrationen](https://msdn.microsoft.com/data/jj591621) und [Migrationen Screencast-Reihe](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Aktivieren Sie Code First-Migrationen

1. Von der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager** und dann **-Paket-Manager-Konsole**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. Auf der `PM>` Eingabeaufforderung Geben Sie den folgenden Befehl aus:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![Enable-Migrations-Befehl](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Dieser Befehl erstellt eine *Migrationen* Ordner im Projekt ContosoUniversity, und es wird in diesem Ordner eine *Configuration.cs* -Datei, die Sie bearbeiten können, um die Migrationen zu konfigurieren.

    ![Ordner "Migrations"](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    Die `Configuration` Klasse enthält eine `Seed` Methode, die aufgerufen wird, wenn die Datenbank erstellt wird, und jedes Mal, wenn sie aktualisiert wird, nachdem eine Änderung des Datenmodells.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    Der Zweck dieses `Seed` Methode besteht darin, ermöglichen es Ihnen, nach dem Code First erstellt oder sie aktualisiert Testdaten in die Datenbank eingefügt.

### <a name="set-up-the-seed-method"></a>Richten Sie die Seed-Methode

Die [Ausgangswert](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) Methode ausgeführt wird, wenn Code First-Migrationen mit die Datenbank erstellt und jedes Mal, wenn sie die Datenbank zur aktuellen aktualisiert. Die Seed-Methode dient zum ersten Mal auf, um das Einfügen von Daten in Ihren Tabellen, bevor die Anwendung können die Datenbank zugreift.

In früheren Versionen von Code First, bevor Migrationen kam, war es üblich `Seed` Methoden zum Einfügen von Testdaten, weil bei jeder modelländerung während der Entwicklung verwendet die Datenbank vollständig löschen und von Grund auf neu erstellt haben. Code First-Migrationen, Test, der Daten, nachdem Änderungen in der Datenbank beibehalten werden, also mit den Testdaten in die [Ausgangswert](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) Methode ist in der Regel nicht erforderlich. In der Tat nicht sollen die `Seed` Methode, um Testdaten einzufügen, wenn Sie Migrationen zum Bereitstellen der Datenbank für die Produktion, verwenden da die `Seed` Methode in der Produktion ausgeführt. In diesem Fall möchten Sie die `Seed` Methode, um nur die Daten in die Datenbank eingefügt, der in der Produktion eingefügt werden soll. Sie können z. B. die Datenbank im tatsächlichen Abteilungsnamen enthalten die `Department` Tabelle, wenn die Anwendung in der Produktion verfügbar ist.

In diesem Tutorial verwenden Sie Migrationen für die Bereitstellung, aber das `Seed` Methode fügt Testdaten ohnehin um leichter sehen, wie die Funktionalität der Anwendung funktioniert, ohne eine große Datenmenge manuell einfügen.

1. Ersetzen Sie den Inhalt von der *Configuration.cs* -Datei mit den folgenden Code, der Testdaten in die neue Datenbank geladen werden. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    Die [Ausgangswert](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx) Methode nimmt das Datenbank-Context-Objekt als Eingabeparameter und der Code in der Methode verwendet das Objekt auf der Datenbank neue Entitäten hinzugefügt. Für jeden Entitätstyp, der Code erstellt eine Auflistung von neuen Entitäten, fügt diese an die entsprechende ["DbSet"](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx) Eigenschaft, und klicken Sie dann speichert die Änderungen an der Datenbank. Es ist nicht notwendig, die ["SaveChanges"](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) Methode nach jeder Gruppe von Entitäten, wie hier geschehen ist, aber dies hilft Ihnen, die Ursache eines Problems zu suchen, wenn eine Ausnahme auftritt, während der Code in die Datenbank geschrieben werden.

    Verwenden Sie einige der Anweisungen, die Daten einfügen der [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) Methode zum Ausführen eines Vorgangs "Upsert". Da die `Seed` Methode, die bei jeder Migration ausgeführt wird, Daten, können nicht nur eingefügt werden, da die Zeilen, die Sie hinzufügen möchten es nach der ersten Migration sein werden, der die Datenbank erstellt. Der Vorgang "Upsert" wird verhindert, dass Fehler, die passieren würde, wenn Sie versuchen, fügen Sie eine Zeile, die bereits vorhanden ist, aber es ***überschreibt*** Änderungen an Daten, die Sie möglicherweise beim Testen der Anwendung vorgenommen haben. Mit Testdaten in einigen Tabellen möchten Sie vielleicht nicht so: in einigen Fällen, wenn Sie Daten während des Tests ändern, sollen die Änderungen, um nach Datenbankupdates bleiben. In diesem Fall einen bedingte Insert-Vorgang ausgeführt werden soll: Fügen Sie eine Zeile nur dann, wenn er nicht bereits vorhanden. Die Seed-Methode verwendet beide Ansätze.

    Der erste Parameter zu übergeben, um die [AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) Methode gibt die Eigenschaft zu verwenden, um zu überprüfen, ob eine Zeile bereits vorhanden ist. Für die Testdaten für "Student", die Sie bereitstellen, die `LastName` Eigenschaft kann für diesen Zweck verwendet werden, da jedes Nachnamens in der Liste eindeutig ist:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Dieser Code wird davon ausgegangen, dass die Nachnamen eindeutig sind. Wenn Sie manuell ein Schüler/Student mit einem doppelten Nachnamen hinzufügen, erhalten Sie die folgende Ausnahme beim nächsten einer Migration ausführen.

    Die Sequenz enthält mehr als ein element

    Weitere Informationen zu den `AddOrUpdate` -Methode finden Sie unter [kümmern mit EF 4.3 AddOrUpdate Methode](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie lermans Blog.

    Der Code, `Enrollment` Entitäten verwendet nicht die `AddOrUpdate` Methode. Es überprüft, ob eine Entität bereits vorhanden ist und die Entität eingefügt, wenn er nicht vorhanden ist. Dieser Ansatz wird vorgenommene Änderungen beizubehalten, die Sie an einer Registrierung auf Unternehmensniveau beim Ausführen von Migrationen. Der Code durchläuft jedes Element der `Enrollment` [Liste](https://msdn.microsoft.com/library/6sh2ey19.aspx) und wenn die Registrierung in der Datenbank nicht gefunden wird, wird die Registrierung mit der Datenbank hinzugefügt. Beim ersten der Datenbank Aktualisierung, wird die Datenbank leer sein, sodass jede Registrierung hinzugefügt werden.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Informationen zum Debuggen der `Seed` -Methode und das Durchführen von redundanter Daten wie z. B. zwei Schüler/Studenten, die mit dem Namen "Alexander Carson", finden Sie unter [Seeding and Debugging Entity Framework (EF) DBs](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) andersons Blog.
2. Erstellen Sie das Projekt.

### <a name="create-and-execute-the-first-migration"></a>Erstellen und Ausführen der ersten Migrations

1. Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle aus: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    Die `add-migration` Befehl hinzugefügt, um den Ordner "Migrations" eine *[Datumsstempel]\_InitialCreate.cs* -Datei, die Code enthält, der die Datenbank erstellt. Der erste Parameter (`InitialCreate)` wird verwendet, für die Datei benennen und kann beliebig, die Sie in der Regel wählen Sie ein Wort oder Ausdruck, der zusammengefasst, was bei der Migration erfolgt. Sie können z. B. eine spätere Migration nennen &quot;AddDepartmentTable&quot;.

    ![Ordner "Migrations" bei der anfänglichen migration](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    Die `Up` Methode der `InitialCreate` Klasse erstellt, die Datenbanktabellen, die den datenmodellentitätenmengen entsprechen, und die `Down` Methode löscht sie. Die Migrationsfunktion ruft die Methode `Up` auf, um die Datenmodelländerungen für eine Migration zu implementieren. Wenn Sie einen Befehl eingeben, um ein Rollback für das Update auszuführen, ruft die Migrationsfunktion die Methode `Down` auf. Der folgende Code zeigt den Inhalt der `InitialCreate` Datei:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    Die `update-database` Befehl führt die `Up` Methode zum Erstellen der Datenbank aus, und klicken Sie dann es ausgeführt wird die `Seed` Methode, um die Datenbank zu füllen.

SQL Server-Datenbank ist jetzt für Ihr Datenmodell erstellt wurde. Der Name der Datenbank ist *ContosoUniversity*, und die *mdf* Datei befindet sich in Ihrem Projekts die *App\_Daten* Ordner denn das ist Ihre Angabe in Ihrer die Verbindungszeichenfolge.

Verwenden Sie entweder **Server-Explorer** oder **Objekt-Explorer von SQL Server** (SSOX), um die Datenbank in Visual Studio anzuzeigen. In diesem Tutorial verwenden Sie **Server-Explorer**. In Visual Studio Express 2012 für Web **Server-Explorer** heißt **Datenbank-Explorer**.

1. Von der **Ansicht** Menü klicken Sie auf **Server-Explorer**.
2. Klicken Sie auf die **Verbindung hinzufügen** Symbol.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. Wenn Sie mit aufgefordert werden die **Datenquelle auswählen** Dialogfeld klicken Sie auf **Microsoft SQL Server**, und klicken Sie dann auf **Weiter**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. In der **Verbindung hinzufügen** Dialogfeld Geben Sie **(Localdb) \v11.0** für die **Servernamen**. Klicken Sie unter **auswählen oder Eingeben eines Datenbanknamens**Option **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Sie konfiguriert die BizTalk-Orchestrierung, um den PeopleSoft-Adapter zu verwenden, um Daten aus dem PeopleSoft-System abzurufen.**
6. Erweitern Sie **SchoolContext** und erweitern Sie dann **Tabellen**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Mit der rechten Maustaste die **für Schüler und Studenten** Tabelle, und klicken Sie auf **Tabellendaten anzeigen** , finden Sie unter den Spalten, die erstellt wurden und die Zeilen, die in die Tabelle eingefügt wurden.

    ![Tabelle "Student"](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Erstellen einen "Student"-Controller und Ansichten

Der nächste Schritt ist auf einer ASP.NET MVC-Controller und Ansichten in Ihrer Anwendung erstellen, die eine dieser Tabellen verwenden können.

1. Zum Erstellen einer `Student` mit der rechten Maustaste der Controller die **Controller** Ordner in **Projektmappen-Explorer**, wählen **hinzufügen**, und klicken Sie dann auf **Controller** . In der **Controller hinzufügen** Dialogfeld Feld, die folgenden Optionen aus, und klicken Sie dann auf **hinzufügen**: 

   - Controllername: **StudentController**.
   - Vorlage: **MVC-Controller mit Lese-/schreibaktionen und Ansichten unter Verwendung von Entity Framework**.
   - Modellklasse: **für Schüler und Studenten (ContosoUniversity.Models)**. (Wenn Sie diese Option in der Dropdown-Liste nicht angezeigt wird, erstellen Sie das Projekt, und versuchen Sie es erneut.)
   - Datenkontextklasse: **SchoolContext (ContosoUniversity.Models)**.
   - Ansichten: **Razor (CSHTML)**. (Die Standardeinstellung.)

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Visual Studio öffnet die *Controllers\StudentController.cs* Datei. Sie sehen, dass eine Klassenvariable erstellt wurde, das ein Datenbank-Context-Objekt instanziiert:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     Die `Index` Action-Methode ruft eine Liste von Schülern und Studenten die *Schüler/Studenten* Entitätenmenge durch Lesen der `Students` Eigenschaft des Context-Instanz der Datenbank:

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     Die *Student\Index.cshtml* zeigt diese Liste in einer Tabelle:

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. Drücken Sie STRG+F5, um das Projekt auszuführen.

     Klicken Sie auf die **Schüler/Studenten** Tab, um die Testdaten abzurufen, die die `Seed` -Methode eingefügt wurden.

     ![Indexseite "Student"](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Konventionen

Die Menge des Codes, die Sie in der Reihenfolge für das Entity Framework, um eine vollständige Datenbank erstellen zu können schreiben musste ist minimal, durch die Verwendung von *Konventionen*, oder die Annahmen, die Entity Framework verwendet werden. Einige von ihnen haben bereits erwähnt, wurden:

- Die pluralisierte Formen der Entity-Klassennamen werden als Tabellennamen verwendet.
- Eigenschaftennamen von Entitäten werden als Spaltennamen verwendet.
- Eigenschaften der Entität mit dem Namen `ID` oder *Classname* `ID` werden als Primärschlüsseleigenschaften erkannt.

Sie haben gesehen, dass die Konventionen überschrieben werden können (z. B. Sie angegeben, dass Tabellennamen im Plural stehen sollten nicht), und erfahren Sie mehr über Konventionen und überschreiben sie in der [erstellen eine weitere komplexe Datenmodell](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) Tutorial später in dieser Serie. Weitere Informationen finden Sie unter [Code First-Konventionen](https://msdn.microsoft.com/data/jj679962).

## <a name="summary"></a>Zusammenfassung

Sie haben nun eine einfache Anwendung erstellt, die das Entity Framework und SQL Server Express zum Speichern und Anzeigen von Daten verwendet. Im folgenden Tutorial erfahren Sie, wie zum Ausführen von grundlegenden CRUD (erstellen, lesen, aktualisieren und löschen) Vorgänge. Sie können Feedback unten auf dieser Seite lassen. Informieren Sie uns, wie Sie diesen Teil des Tutorials gefallen hat und wie wir es verbessern können.

Links zu anderen Ressourcen des Entity Framework finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Nächste](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
