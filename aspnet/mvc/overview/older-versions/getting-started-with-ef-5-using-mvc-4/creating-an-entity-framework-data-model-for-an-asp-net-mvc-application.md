---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: "Erstellen eines Entity Framework-Datenmodells für eine ASP.NET MVC-Anwendung (1 von 10) | Microsoft Docs"
author: tdykstra
description: "Eine neuere Version dieser Reihe von Lernprogrammen ist verfügbar, für das Visual Studio 2013 und Entity Framework 6 mit MVC 5. Contoso University Beispiel Web Application de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c25ebf472df5dcbc664257cdf8678bfac535d846
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>Erstellen eines Entity Framework-Datenmodells für eine ASP.NET MVC-Anwendung (1 von 10)
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > Ein [neuere Version dieser Reihe von Lernprogrammen](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) für Visual Studio 2013, Entity Framework 6 und MVC 5 verfügbar ist.
> 
> 
> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 und Visual Studio 2012. Die beispielanwendung ist eine Website für eine fiktive Contoso-Universität. Es umfasst Funktionen wie Student Zulassung, Kurs Erstellung und Instructor-Zuweisungen. Diese Reihe von Lernprogrammen wird erläutert, wie die Contoso University-beispielanwendung zu erstellen. Sie können [herunterladen die fertigen Anwendung](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8).
> 
> ## <a name="code-first"></a>Code First
> 
> Es gibt drei Möglichkeiten, die Sie mit Daten im Entity Framework arbeiten können: *Database First*, *Model First*, und *Code First*. Dieses Lernprogramm ist für Code First. Informationen zu den Unterschieden zwischen den Workflows und einer Anleitung zum Auswählen der besten für Ihr Szenario finden Sie unter [Entity Framework-Entwicklungsworkflows](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf).
> 
> ## <a name="mvc"></a>MVC
> 
> Die beispielanwendung basiert auf [ASP.NET-MVC](../../../index.md). Wenn Sie mit dem ASP.NET Web Forms-Modell arbeiten möchten, finden Sie unter der [Modell binden und Web Forms](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md) Reihe von Lernprogrammen und [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).
> 
> ## <a name="software-versions"></a>Versionen der Software
> 
> | **In diesem Lernprogramm gezeigt** | **Funktioniert auch mit** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio 2012 Express für Web. Dies wird automatisch vom Windows Azure SDK installiert, wenn Sie Visual Studio 2012 oder Visual Studio 2012 Express für Web noch nicht haben. Visual Studio 2013 sollten arbeiten, aber im Lernprogramm wurde nicht mit getestet, und einige Menüs und Dialogfelder unterscheiden sich. Die [VS 2013-Version des Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510) für Windows Azure-Bereitstellung erforderlich ist. |
> | .NET 4.5 | Die meisten Funktionen gezeigt funktioniert in .NET 4, aber einige wird nicht. Beispielsweise erfordert die Enum-Unterstützung in EF .NET 4.5. |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | Wenn Sie die Windows Azure-Bereitstellungsschritte überspringen, benötigen Sie nicht das SDK. Wenn eine neue Version des SDK veröffentlicht wird, wird der Link die neuere Version installiert. In diesem Fall müssen Sie möglicherweise einige der Features für die neue Benutzeroberfläche und Anweisungen anpassen. |
> 
> ## <a name="questions"></a>Fragen
> 
> Wenn Sie Fragen, die nicht direkt mit dem Lernprogramm verknüpft sind haben, bereitstellen können, die [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx), [Entity Framework und LINQ to Entities-Forum](https://social.msdn.microsoft.com/forums/en-US/adodotnetentityframework/threads/), oder [ StackOverflow.com](http://stackoverflow.com/).
> 
> ## <a name="acknowledgments"></a>Bestätigungen
> 
> Finden Sie im letzten Lernprogramm in der Reihe für [Bestätigungen und einen Hinweis zu VB](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments).
> 
> ## <a name="original-version-of-the-tutorial"></a>Ursprüngliche Version des Lernprogramms
> 
> Die ursprüngliche Version des Lernprogramms finden Sie in der [der EF-4.1 / MVC 3-e-Book](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC).


## <a name="the-contoso-university-web-application"></a>Die Contoso-University-Webanwendung

Die Anwendung, die Sie in diesen Lernprogrammen erstellen werden müssen ist eine einfache University-Website.

Benutzer können anzeigen und Studenten, Kurs und Instructor-Informationen aktualisieren. Hier sind einige der Bildschirme, die Sie erstellen müssen.

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

Den Stil der Benutzeroberfläche von diesem Standort wurde in der Nähe was von den integrierten Vorlagen generiert wird beibehalten, damit das Lernprogramm in erster Linie für die Verwendung von Entity Framework konzentrieren kann.

## <a name="prerequisites"></a>Erforderliche Komponenten

Die Richtungen und Screenshots in diesem Lernprogramm wird davon ausgegangen, dass die Verwendung von [Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads) oder [zuerst Visual Studio 2012 Express für Web](https://go.microsoft.com/fwlink/?LinkID=275131), mit der neuesten Updates und Azure SDK für .NET ab Juli, installiert 2013. Sie können all dies mit dem folgenden Link abrufen:

[Azure SDK für .NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

Wenn Sie Visual Studio installiert haben, wird die oben genannten Link fehlenden Komponenten installiert. Wenn Sie Visual Studio nicht haben, wird der Link zuerst Visual Studio 2012 Express für Web installieren. Sie können Visual Studio 2013 verwenden, die einige der erforderlichen Vorgehensweisen und Bildschirme wird unterscheiden sich jedoch.

## <a name="create-an-mvc-web-application"></a>Erstellen einer MVC-Webanwendung

Öffnen Sie Visual Studio und erstellen Sie ein neues c#-Projekt, mit dem Namen "ContosoUniversity" mit der **ASP.NET MVC 4-Webanwendung** Vorlage. Stellen Sie sicher, dass Sie als Ziel **.NET Framework 4.5** (Verwendung müssen [ `enum` Eigenschaften](https://msdn.microsoft.com/en-us/data/hh859576.aspx), und dies erfordert .NET 4.5).

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

In der **neues ASP.NET MVC 4-Projekt** aktivieren Sie im Dialogfeld die **Internetanwendung** Vorlage.

Lassen Sie die **Razor** anzeigen Modul ausgewählt, und lassen Sie die **ein Komponententestprojekt erstellen** Kontrollkästchen deaktiviert.

Klicken Sie auf **OK**.

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>Richten Sie den Website-Stil

Einige einfache Änderungen werden das Menü "Vorschauwebsite", die Layout und die Startseite einrichten.

Open *Views\Shared\\_Layout.cshtml*, und Ersetzen Sie den Inhalt der Datei durch den folgenden Code. Die Änderungen werden hervorgehoben.

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

Dieser Code macht die folgenden Änderungen:

- Die für Vorlageninstanzen von "Meine ASP.NET MVC-Anwendung" und "Ihr Logo hier einfügen" ersetzt durch "Contoso University".
- Fügt mehrere Aktionslinks, die später in diesem Lernprogramm verwendet werden.

In *Views\Home\Index.cshtml*, ersetzen Sie den Inhalt der Datei mit den folgenden Code hinzu, die Vorlage Absätze zu ASP.NET und MVC zu vermeiden:

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

In *Controllers\HomeController. cs*, ändern Sie den Wert für `ViewBag.Message` in die `Index` Aktionsmethode auf "Welcome to Contoso University!", wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

Drücken Sie STRG + F5, um die Website ausgeführt. Sie finden Sie auf der Startseite mit im Hauptmenü.

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>Erstellen des Datenmodells

Als Nächstes erstellen Sie für die Anwendung von Contoso University Entitätsklassen. Beginnen Sie mit den folgenden drei Entitäten:

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

Es wird eine 1: n-Beziehung zwischen `Student` und `Enrollment` Entitäten, und es wird eine 1: n-Beziehung zwischen `Course` und `Enrollment` Entitäten. Das heißt, eine Student kann in einer beliebigen Anzahl von Kursen registriert sein, und ein Kurs kann eine beliebige Anzahl der Schüler in es registriert haben.

In den folgenden Abschnitten erstellen Sie eine Klasse für jede dieser Entitäten.

> [!NOTE]
> Wenn Sie versuchen, das Projekt kompiliert werden, bevor Sie alle diese Entitätsklassen erstellt haben, erhalten Sie Compiler-Fehler.


### <a name="the-student-entity"></a>Die Student-Entität

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

In der *Modelle* Ordner erstellen *Student.cs* und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

Die `StudentID` Eigenschaft werden die Primärschlüsselspalte der Datenbanktabelle, die diese Klasse entspricht. Wird standardmäßig das Entity Framework interpretiert eine Eigenschaft mit dem Namen `ID` oder *Classname* `ID` als Primärschlüssel.

Die `Enrollments` Eigenschaft ist ein *Navigationseigenschaft*. Navigationseigenschaften halten andere Entitäten, die mit dieser Entität verknüpft sind. In diesem Fall die `Enrollments` Eigenschaft eine `Student` Entität enthält alle der `Enrollment` , die verbundenen, Entitäten `Student` Entität. In anderen Worten: Wenn eine angegebene `Student` Zeile in der Datenbank verfügt über zwei im Zusammenhang `Enrollment` Zeilen (Zeilen mit Primärschlüssel, Student Wert in ihre `StudentID` foreign Key-Spalte), die `Student` Entität `Enrollments` Navigationseigenschaft enthält diese zwei `Enrollment` Entitäten.

Navigationseigenschaften werden in der Regel als definiert `virtual` , damit sie bestimmte Entity Framework-Funktionen wie z. B. nutzen können *verzögertes Laden*. (Lazy Loading werden erläutert werden weiter unten in der [das Lesen verknüpfter Daten](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) Tutorial weiter unten in dieser Serie.

Wenn eine Navigationseigenschaft über mehrere Entitäten (wie in der m: n- oder 1: n-Beziehungen) enthalten kann, muss dessen Typ eine Liste, in dem Einträge können hinzugefügt, gelöscht, und aktualisiert, z. B. `ICollection`.

### <a name="the-enrollment-entity"></a>Die Registrierung-Entität

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

In der *Modelle* Ordner erstellen *Enrollment.cs* und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Die Dienstqualität-Eigenschaft ist ein [Enum](https://msdn.microsoft.com/en-us/data/hh859576.aspx). Das Fragezeichen nach der `Grade` Typdeklaration gibt an, dass die `Grade` Eigenschaft ist [NULL-Werte zulassen](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx). Eine Klasse, die null ist unterscheidet sich von einer NULL Grade – Null bedeutet, dass eine Klasse ist nicht bekannt oder noch nicht noch zugewiesen wurde.

Die `StudentID` Eigenschaft ist ein Fremdschlüssel, und die entsprechende Navigationseigenschaft ist `Student`. Ein `Enrollment` Entität bezieht sich auf eine `Student` Entität, damit die Eigenschaft nur ein einzelnes enthalten darf `Student` Entität (im Gegensatz zu den `Student.Enrollments` Navigationseigenschaft gelernt früher mehrere halten `Enrollment` Entitäten).

Die `CourseID` Eigenschaft ist ein Fremdschlüssel, und die entsprechende Navigationseigenschaft ist `Course`. Ein `Enrollment` Entität ist mit einem zugeordneten `Course` Entität.

### <a name="the-course-entity"></a>Die Kurs-Entität

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

In der *Modelle* Ordner erstellen *Course.cs*, ersetzen den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

Die `Enrollments` Eigenschaft ist eine Navigationseigenschaft. Ein `Course` Entität kann sich auf eine beliebige Anzahl von beziehen `Enrollment` Entitäten.

Z. B. Weitere Informationen zu den [[DatabaseGenerated](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx). Keine)]-Attribut in den nächsten Lernprogrammen. Im Grunde, Sie können dieses Attribut geben Sie den primären Schlüssel für den Kurs, anstatt die Datenbank, die sie generieren.

## <a name="create-the-database-context"></a>Erstellen Sie den Datenbankkontext

Ist die Hauptklasse, das Entity Framework-Funktionen für einen angegebenen Datenmodell koordiniert die *Datenbankkontext* Klasse. Erstellen Sie diese Klasse durch Ableiten von der [System.Data.Entity.DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx) Klasse. Im Code Geben Sie an, welche Entitäten im Datenmodell enthalten sind. Sie können auch bestimmte Entity Framework-Verhalten anpassen. In diesem Projekt die Klasse heißt `SchoolContext`.

Erstellen Sie einen Ordner mit dem Namen *DAL* (für die Datenzugriffsebene). In diesem Ordner erstellen Sie eine neue Klassendatei mit dem Namen *SchoolContext.cs*, und Ersetzen Sie den vorhandenen Code durch folgenden Code:

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

Dieser Code erstellt ein [DbSet](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset(v=VS.103).aspx) Eigenschaft für jede Entitätenmenge. In der Terminologie von Entity Framework ein *Entitätenmenge* entspricht normalerweise einer Datenbanktabelle und ein *Entität* entspricht einer Zeile in der Tabelle.

Die `modelBuilder.Conventions.Remove` -Anweisung in der [OnModelCreating](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx) Methode verhindert, dass Tabellennamen wird pluralisiert. Wenn Sie dies nicht tun, würde die generierten Tabellen benannt werden `Students`, `Courses`, und `Enrollments`. Stattdessen werden die Tabellennamen `Student`, `Course`, und `Enrollment`. Entwickler sind sich uneinig darüber, ob Tabellennamen im Plural stehen sollten oder nicht. In diesem Lernprogramm wird die Singularform verwendet, aber der wichtige Punkt ist, dass Sie unabhängig davon, welche Form Sie dann auswählen können durch ein- oder dieser Zeile des Codes auslassen.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx) ist eine vereinfachte Version von SQL Server Express-Datenbankmoduls, die bedarfsgesteuert gestartet und im Benutzermodus ausgeführt wird. LocalDB ausgeführt wird, in eine spezielle Ausführungsmodus von SQL Server Express, die Ihnen ermöglicht, die Arbeit mit Datenbanken als *mdf* Dateien. LocalDB-Datenbankdateien bleiben in der Regel der *App\_Daten* Ordner eines Webprojekts. Die Benutzerinstanzfunktion in SQL Server Express können Sie zur Bearbeitung auch *mdf* Dateien, aber der Benutzerinstanzfunktion wird als veraltet markiert; daher LocalDB wird empfohlen, für die Arbeit mit *mdf* Dateien.

SQL Server Express wird in der Regel für die Produktion Webanwendungen nicht verwendet. LocalDB ist mit einer Webanwendung für die Produktion insbesondere nicht empfohlen, da es nicht zum Arbeiten mit IIS ausgelegt ist.

In Visual Studio 2012 und höheren Versionen ist LocalDB, die standardmäßig mit Visual Studio installiert. In Visual Studio 2010 und frühere Versionen ist das SQL Server Express (ohne LocalDB) standardmäßig mit Visual Studio installiert. Sie müssen diese manuell installieren, wenn Sie Visual Studio 2010 verwenden.

In diesem Lernprogramm vorerst arbeiten Sie mit LocalDB, damit die Datenbank gespeichert werden kann, in der *App\_Daten* Ordner wie eine *mdf* Datei. Öffnen Sie die Stammkonfigurationsdatei *"Web.config"* Datei, und fügen Sie eine neue Verbindungszeichenfolge für die `connectionStrings` -Auflistung, wie im folgenden Beispiel gezeigt. (Stellen Sie sicher, dass Sie aktualisieren die *"Web.config"* -Datei im Stammordner des Projekts. Es gibt auch eine *"Web.config"* Datei befindet sich in der *Ansichten* Unterordner, die Sie nicht aktualisieren müssen.)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

Standardmäßig sucht die Entity Framework für eine Verbindungszeichenfolge, die den gleichen Namen wie die `DbContext` Klasse (`SchoolContext` für dieses Projekt). Die Verbindungszeichenfolge, die Sie hinzugefügt haben, gibt eine LocalDB-Datenbank mit dem Namen *ContosoUniversity.mdf* befindet sich in der *App\_Daten* Ordner. Weitere Informationen finden Sie unter [SQL Server-Verbindungszeichenfolgen für ASP.NET-Webanwendungen](https://msdn.microsoft.com/en-us/library/jj653752.aspx).

Sie müssen nicht tatsächlich die Verbindungszeichenfolge angeben. Wenn Sie eine Verbindungszeichenfolge angeben, wird Entity Framework für Sie erstellen. allerdings die Datenbank möglicherweise nicht der *App\_Daten* Ordner Ihrer App. Informationen, auf dem die Datenbank erstellt wird, finden Sie unter [Code First in eine neue Datenbank](https://msdn.microsoft.com/en-us/data/jj193542).

Die `connectionStrings` Auflistung verfügt auch über eine Verbindungszeichenfolge mit dem Namen `DefaultConnection` die für die Mitgliedschaftsdatenbank verwendet wird. Sie wird nicht die Mitgliedschaftsdatenbank in diesem Lernprogramm verwenden. Der einzige Unterschied zwischen den zwei Verbindungszeichenfolgen ist der Datenbankname und der Wert des Name-Attributs.

## <a name="set-up-and-execute-a-code-first-migration"></a>Richten Sie ein und führen Sie einer ersten Codemigration aus

Bei der ersten zur Entwicklung einer Anwendung, Modellieren Ihrer Daten Änderungen häufig auf, und jedes Mal das Modell ändert, die es nicht synchron mit der Datenbank abruft. Sie können konfigurieren, dass das Entity Framework automatisch löschen und Neuerstellen der Datenbank bei jeder Änderung des Datenmodells. Dies ist kein Problem früh in der Entwicklung, da aus Testdaten einfach neu erstellt besteht, aber nachdem Sie in der produktionsumgebung bereitgestellt haben in der Regel Sie das Datenbankschema zu aktualisieren, ohne Löschen der Datenbank möchten. Migrationen dieser Funktion können Code First zum Aktualisieren der Datenbank ohne gelöscht und neu erstellt wird. Früh im Entwicklungszyklus eines neuen Projekts sollen verwenden [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/en-us/library/gg679604(v=vs.103).aspx) löschen, neu zu erstellen und die Datenbank ein erneutes Seeding ausgeführt jedes Mal, wenn das Modell ändert. Eine Sie bereit sind, Ihre Anwendung bereitzustellen, können Sie bei der Migration konvertieren. Für dieses Lernprogramm verwenden Sie nur Migrationen. Weitere Informationen finden Sie unter [Code First-Migrationen](https://msdn.microsoft.com/en-us/data/jj591621) und [Migrationen Screencast Reihe](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx).

### <a name="enable-code-first-migrations"></a>Aktivieren Sie die Code First-Migrationen

1. Aus der **Tools** Menü klicken Sie auf **Bibliothekspaket-Manager** und dann **Package Manager Console**.

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. Bei der `PM>` Eingabeaufforderung Geben Sie den folgenden Befehl aus:

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![Enable-Migrations-Befehl](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    Dieser Befehl erstellt eine *Migrationen* Ordner im Projekt ContosoUniversity, und es wird in diesem Ordner eine *"Configuration.cs"* -Datei, die Sie bearbeiten können, um Migrationen zu konfigurieren.

    ![Ordner](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    Die `Configuration` Klasse enthält eine `Seed` Methode, die aufgerufen wird, wenn die Datenbank erstellt wird, und jedes Mal, wenn sie aktualisiert wird, nachdem die Änderung eines Datenmodells.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    Der Zweck dieses `Seed` Methode besteht darin, ermöglichen es Ihnen, Testdaten in die Datenbank eingefügt werden, nachdem der Code First erstellt oder aktualisiert wird.

### <a name="set-up-the-seed-method"></a>Einrichten der Seed-Methode

Die [Ausgangswert](https://msdn.microsoft.com/en-us/library/hh829453(v=vs.103).aspx) Methode ausgeführt wird, wenn der Code First-Migrationen die Datenbank erstellt und jedes Mal, wenn es zur Aktualisierung der Datenbank auf den neuesten Migration. Die Seed-Methode dient ermöglicht das Einfügen von Daten in Ihren Tabellen, bevor die Anwendung zum ersten Mal auf die Datenbank zugreift.

In früheren Versionen von Code First, bevor Migrationen freigegeben wurde, war es üblich `Seed` Methoden, um Testdaten, eingefügt werden, da bei jeder Änderung Modell während der Entwicklung die Datenbank mussten vollständig gelöscht und von Grund auf neu erstellt werden. Mit Code First-Migrationen Test Daten, nachdem Änderungen an der Datenbank beibehalten werden, einschließlich also Testdaten in die [Ausgangswert](https://msdn.microsoft.com/en-us/library/hh829453(v=vs.103).aspx) Methode ist in der Regel nicht erforderlich. Tatsächlich sollen die `Seed` Methode, um Testdaten einzufügen, wenn Sie Migrationen zum Bereitstellen der Datenbank bis hin zur Produktion, verwenden da die `Seed` Methode wird ausgeführt, in der Produktion. In diesem Fall sollen die `Seed` Methode, um nur die Daten einfügen in die Datenbank, die in der Produktion eingefügt werden soll. Beispielsweise sollten Sie die Datenbank im tatsächlichen Abteilungsnamen enthalten die `Department` Tabelle, wenn die Anwendung in der Produktion zur Verfügung steht.

Für dieses Lernprogramm müssen verwenden Sie Migrationen für die Bereitstellung, aber das `Seed` einfügen Methode Testdaten trotzdem um erleichtern es zu sehen, wie die Funktionalität der Anwendung funktioniert, ohne dass manuell eine große Datenmenge eingefügt.

1. Ersetzen Sie den Inhalt von der *"Configuration.cs"* -Datei mit den folgenden Code, der Testdaten in die neue Datenbank geladen werden. 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    Die [Ausgangswert](https://msdn.microsoft.com/en-us/library/hh829453(v=vs.103).aspx) Methode nimmt das Datenbankkontextobjekt als Eingabeparameter und der Code in der Methode verwendet das Objekt, das Hinzufügen neuer Entitäten auf die Datenbank. Für jeden Entitätstyp im Code erstellt eine Auflistung von neuen Entitäten, fügt diese an die entsprechende [DbSet](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset(v=vs.103).aspx) -Eigenschaft, und klicken Sie dann speichert die Änderungen an der Datenbank. Es ist nicht erforderlich, rufen Sie die [SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx) Methode nach jeder Gruppe von Entitäten, wie hier erfolgt, aber auf diese Weise können Sie die Ursache eines Problems zu suchen, wenn eine Ausnahme auftritt, während der Code in die Datenbank geschrieben werden.

    Einige der Anweisungen, das Einfügen von Daten verwendet werden, die [AddOrUpdate](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) Methode zum Ausführen eines Vorgangs "Upsert". Da die `Seed` Methode, die mit jeder Migration ausgeführt wird, die Daten können nicht nur eingefügt werden, da die Zeilen, die Sie hinzufügen möchten bereits es nach der ersten Migration entsprechen, die die Datenbank erstellt. Der Vorgang "Upsert" wird verhindert, dass Fehler, die eintreten würden, wenn Sie versuchen, fügen Sie eine Zeile, die bereits vorhanden ist, aber es ***überschreibt*** Änderungen an Daten, die Sie möglicherweise beim Testen der Anwendung vorgenommen haben. Mit Testdaten in einigen Tabellen nicht empfiehlt, die durchgeführt werden soll: in einigen Fällen beim Ändern von Daten beim Testen soll Ihre Änderungen bleiben nach einer Aktualisierung der Datenbank. In diesem Fall einen bedingten Einfügevorgang erstellt werden sollen: Fügen Sie eine Zeile nur dann, wenn er nicht bereits vorhanden. Die Seed-Methode verwendet beide Ansätze.

    Der erste Parameter übergeben wird, um die [AddOrUpdate](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx) Methode gibt die Eigenschaft zu verwenden, um zu überprüfen, ob bereits eine Zeile vorhanden. Für die Testdaten für Studenten, die Sie bereitstellen, die `LastName` Eigenschaft kann für diesen Zweck verwendet werden, da jeder letzte Name in der Liste eindeutig ist:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    Mit diesem Code wird davon ausgegangen, dass die Nachnamen eindeutig sind. Wenn Sie einen Studenten mit einem doppelten Nachnamen manuell hinzufügen, erhalten Sie die folgende Ausnahme beim nächsten einer Migration durchführen.

    Die Sequenz enthält mehr als ein element

    Weitere Informationen zu den `AddOrUpdate` -Methode finden Sie unter [Achten Sie darauf mit EF 4.3 AddOrUpdate-Methode](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/) Julie Lerman Blog.

    Der Code, hinzufügt `Enrollment` Entitäten verwendet nicht die `AddOrUpdate` Methode. Er überprüft, ob eine Entität bereits vorhanden ist, und fügt die Entität aus, wenn er nicht vorhanden ist. Dieser Ansatz wird Änderungen beizubehalten, die Sie an eine Registrierung Dienstqualität bei Migrationen ausgeführt. Der Code durchläuft jedes Mitglied der `Enrollment` [Liste](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx) und wenn die Registrierung in der Datenbank nicht gefunden wird, wird die Registrierung mit der Datenbank hinzugefügt. Beim ersten der Datenbank Aktualisierung, wird die Datenbank leer sein, sodass jede Registrierung hinzugefügt wird.

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    Informationen zum Debuggen der `Seed` -Methode und Behandeln von redundanten Daten wie z. B. zwei Studenten, die mit dem Namen "Alexander Carson", finden Sie unter [Seeding und Debuggen von Entity Framework (EF) Datenbanken](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) in Rick andersons Blog.
2. Erstellen Sie das Projekt.

### <a name="create-and-execute-the-first-migration"></a>Erstellen und Ausführen der ersten Migrations

1. Geben Sie im Fenster Paket-Manager-Konsole die folgenden Befehle aus: 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    Die `add-migration` Befehl hinzugefügt, um den Ordner ein *[Datumsstempel]\_InitialCreate.cs* -Datei, die Code enthält, der die Datenbank erstellt. Der erste Parameter (`InitialCreate)` wird verwendet, für die Datei benennen und kann beliebig sein; Sie wird in der Regel wählen Sie ein Wort oder Ausdruck, die zusammengefasst, was bei der Migration durchgeführt wird. Sie können z. B. eine spätere Migrationen Namen &quot;AddDepartmentTable&quot;.

    ![Ordner mit der anfänglichen migration](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    Die `Up` Methode der `InitialCreate` Klasse erstellt, die Datenbanktabellen, die die Daten Modell Entitätenmengen, entsprechen und die `Down` Methode löscht sie. Migrationen Aufrufe der `Up` Methode für die Implementierung des Datenmodells für eine Migration. Bei der Eingabe eines Befehls ein Rollback der Updates, die Migrationen Aufrufe der `Down` Methode. Der folgende Code zeigt den Inhalt der `InitialCreate` Datei:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    Die `update-database` -Befehl ausgeführt wird die `Up` Methode zum Erstellen der Datenbank aus, und klicken Sie dann es führt die `Seed` Methode zum Auffüllen der Datenbank.

SQL Server-Datenbank wurde für das Datenmodell jetzt erstellt. Der Name der Datenbank ist *ContosoUniversity*, und die *mdf* Datei befindet sich in Ihrem Projekts *App\_Daten* Ordner ist, die Ihrer Angabe in Ihre Verbindungszeichenfolge.

Verwenden Sie entweder **Server-Explorer** oder **Objekt-Explorer von SQL Server** (SSOX) zur Anzeige der Datenbank in Visual Studio. Verwenden Sie für dieses Lernprogramm **Server-Explorer**. In Visual Studio Express 2012 für das Web **Server-Explorer** heißt **Datenbank-Explorer**.

1. Aus der **Ansicht** Menü klicken Sie auf **Server-Explorer**.
2. Klicken Sie auf die **Verbindung hinzufügen** Symbol.

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. Wenn Sie aufgefordert werden, mit der **Datenquelle auswählen** Dialogfeld klicken Sie auf **Microsoft SQL Server**, und klicken Sie dann auf **Fortfahren**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. In der **Verbindung hinzufügen** Dialogfeld Geben Sie **(Localdb) \v11.0** für die **Servernamen**. Klicken Sie unter **einen Datenbanknamen eingeben oder auswählen**Option **ContosoUniversity.**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. Klicken Sie auf **OK.**
6. Erweitern Sie **SchoolContext** und schließlich **Tabellen**.  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. Mit der rechten Maustaste die **Student** Tabelle, und klicken Sie auf **Tabellendaten anzeigen** , finden in den Spalten, die erstellt wurden und die Zeilen, die in die Tabelle eingefügt wurden.

    ![Studententabelle](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>Ein Student-Controller und Ansichten werden erstellt

Der nächste Schritt besteht auf einer ASP.NET MVC-Controller und Ansichten in Ihrer Anwendung erstellen, die eine dieser Tabellen verwenden können.

1. Zum Erstellen einer `Student` Controller Maustaste die **Controller** Ordner in **Projektmappen-Explorer**, wählen **hinzufügen**, und klicken Sie dann auf **Controller** . In der **Controller hinzufügen** Dialogfeld Feld, die folgenden Optionen aus, und klicken Sie dann auf **hinzufügen**: 

    - Controllername: **StudentController**.
    - Vorlage: **MVC-Controller mit Lese-/schreibaktionen und Ansichten unter Verwendung von Entity Framework**.
    - Modellschemas: **Student (ContosoUniversity.Models)**. (Wenn diese Option in der Dropdown-Liste nicht angezeigt wird, erstellen Sie das Projekt, und versuchen Sie es erneut.)
    - Datenkontextklasse: **SchoolContext (ContosoUniversity.Models)**.
    - Ansichten: **Razor (CSHTML)**. (Die Standardeinstellung.)

    ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
- Visual Studio öffnet die *Controllers\StudentController.cs* Datei. Sie sehen, dass eine Klassenvariable erstellt wurde, die eine Datenbank Context-Objekt instanziiert:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

    Die `Index` Aktionsmethode ruft eine Liste der Schüler der *Studenten* Entitätenmenge durch Lesen der `Students` Eigenschaft des Context-Datenbankinstanz:

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

    Die *Student\Index.cshtml* zeigt diese Liste in einer Tabelle:

    [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
- Drücken Sie STRG+F5, um das Projekt auszuführen.

    Klicken Sie auf die **Studenten** Tab, um die Testdaten finden Sie unter, die die `Seed` Methode eingefügt.

    ![Student-Indexseite](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>Konventionen

Die Menge an Code in Reihenfolge für das Entity Framework voraussetzten können zum Erstellen einer vollständigen Datenbank ist aufgrund der Verwendung von minimal *Konventionen*, oder die Annahmen, die das Entity Framework stellt. Einige von ihnen haben bereits angemerkt wurde:

- Die pluralized Formen von Namen für die Entität werden als Tabellennamen verwendet.
- Entitätsnamen-Eigenschaft werden für Spaltennamen verwendet.
- Eigenschaften der Entität mit dem Namen sind `ID` oder *Classname* `ID` werden als Eigenschaften des Primärschlüssels erkannt.

Sie gesehen haben, dass die Konventionen überschrieben werden können (z. B. Sie angegeben, dass Tabellennamen darf keine pluralisiert werden), und erfahren Sie mehr über Konventionen und überschreiben Sie diese in die [erstellen eine weitere komplexe Datenmodell](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md) Lernprogramm weiter unten in dieser Serie. Weitere Informationen finden Sie unter [Code First-Konventionen](https://msdn.microsoft.com/en-us/data/jj679962).

## <a name="summary"></a>Zusammenfassung

Sie haben nun eine einfache Anwendung erstellt, die das Entity Framework und SQL Server Express zum Speichern und Anzeigen von Daten verwendet. Im folgenden Lernprogramm erfahren Sie, zum Ausführen von grundlegenden CRUD (erstellen, lesen, aktualisieren und löschen) Vorgänge. Sie können Feedback am unteren Rand dieser Seite lassen. Bitte lassen Sie uns wissen Sie, wie Sie diesen Teil des Lernprogramms gefallen hat und wie wir es verbessern können.

Links zu anderen Entity Framework-Ressourcen finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Nächste](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
