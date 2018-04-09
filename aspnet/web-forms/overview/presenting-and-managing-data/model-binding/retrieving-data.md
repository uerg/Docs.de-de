---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Abrufen und Anzeigen von Daten mit Modell Bindung und Web Forms | Microsoft Docs
author: tfitzmac
description: Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 26873efa5dbfdbdab39a52cfb9cfd5a65c8231a3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Abrufen und Anzeigen von Daten mit modellbindung und WebForms
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diese Reihe von Lernprogrammen veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung bei einem ASP.NET Web Forms-Projekt. Wurden die modellbindung macht die dateninteraktion mehr die selbsterklärend als Umgang mit Data Source-Objekte (z. B. ObjectDataSource oder SqlDataSource). Diese Reihe beginnt mit einführendes Material und verschiebt die erweiterte Konzepte in späteren Lernprogrammen.
> 
> Das Modell bindungsmuster funktioniert mit jeder datenzugriffstechnologie. In diesem Lernprogramm verwenden Sie Entity Framework, jedoch können Sie die datenzugriffstechnologie, die für Sie am besten vertraut ist. Von einem datengebundenen Serversteuerelement, z. B. eine GridView, ListView, DetailsView oder FormView-Steuerelement geben Sie die Namen der Methoden zum auswählen, aktualisieren, löschen und Erstellen von Daten verwendet werden soll. In diesem Lernprogramm werden Sie einen Wert für die SelectMethod angeben.
> 
> Innerhalb dieser Methode können bereitstellen Sie die Logik zum Abrufen der Daten. In den nächsten Lernprogrammen werden Werte für UpdateMethod, DeleteMethod und InsertMethod festgelegt werden.
> 
> Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB dar. Der Code zum Herunterladen funktioniert mit Visual Studio 2012 oder Visual Studio 2013. Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Lernprogramm gezeigt wird.
> 
> In diesem Lernprogramm führen Sie die Anwendung in Visual Studio. Sie können auch die Anwendung verfügbar über das Internet, indem sie in einem Hostinganbieter bereitstellen. Microsoft bietet kostenlose Webhosting für bis zu 10 Websites in einem  
>  [Freigeben von Azure-Testkonto](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Informationen zum Bereitstellen einer Visual Studio-Webprojekt zu Azure App Service-Web-Apps finden Sie unter der [ASP.NET Web-Bereitstellung mit Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) Reihe. Dieses Lernprogramm wird gezeigt, wie Entity Framework Code First-Migrationen zu verwenden, um die SQL Server-Datenbank auf Azure SQL-Datenbank bereitstellen.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - Microsoft Visual Studio 2013 oder Microsoft Visual Studio Express 2013 für Web
>   
> 
> Dieses Lernprogramm funktioniert auch mit Visual Studio 2012, jedoch werden einige Unterschiede in der Vorlage "Benutzer" Schnittstelle "und"-Projekt.


## <a name="what-youll-build"></a>Was müssen Sie erstellen

In diesem Lernprogramm lernen Sie folgende Aktionen ausführen:

1. Data-Objekte, die eine Universität mit Studenten in Kursen registriert wiedergeben
2. Erstellen von Tabellen aus den Objekten
3. Füllen Sie die Datenbank mit Testdaten
4. Anzeigen von Daten in einem Web form

## <a name="set-up-project"></a>Einrichten des Projekts

In Visual Studio 2013, erstellen Sie ein neues **ASP.NET-Webanwendung** aufgerufen **ContosoUniversityModelBinding**.

![Projekt erstellen](retrieving-data/_static/image2.png)

Wählen Sie die Web Forms-Vorlage, und lassen Sie die andere Standardoptionen. Klicken Sie auf OK, um das Projekt einrichten.

![Wählen Sie WebForms](retrieving-data/_static/image3.png)

Zunächst wird eine Reihe von kleinen Änderungen zum Anpassen der Darstellung des Standorts erstellt. Öffnen der **Site.Master** Datei, und ändern Sie den Titel Einbeziehung von Contoso University statt meine ASP.NET-Anwendung.

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

Ändern Sie dann den Headertext aus **Anwendungsname** auf **Contoso University**.

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

Ändern Sie auch in Site.Master, die Navigationslinks, die angezeigt, in der Kopfzeile werden entsprechend der Seiten, die diesem Standort relevant sind. Sie benötigen nicht entweder die **zu** Seite oder die **wenden Sie sich an** Seite, damit diese Links entfernt werden kann. Stattdessen benötigen Sie einen Link zu einer Seite aufgerufen **Studenten**. Diese Seite wurde noch nicht erstellt.

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

Speichern und Schließen von Site.Master.

Nun erstellen Sie das Web Form zur Anzeige von Student-Daten. Mit der rechten Maustaste des Projekts und **hinzufügen** eine **neues Element**. Wählen Sie die **Webformular mit Gestaltungsvorlage** Vorlage, und nennen Sie sie **Students.aspx**.

![Seite "erstellen"](retrieving-data/_static/image5.png)

Wählen Sie **Site.Master** als die Gestaltungsvorlage für das neue WebForm.

## <a name="create-the-data-models-and-database"></a>Erstellen Sie die Datenmodelle und Datenbank

Sie werden Code First-Migrationen verwenden, um Objekte und den entsprechenden Datenbanktabellen erstellen. Diese Tabellen werden Informationen zu den Studenten und ihre Kurse gespeichert.

Fügen Sie eine neue Klasse mit dem Namen im Ordner Models **UniversityModels.cs**.

![Erstellen der Skriptobjektmodell-Klasse](retrieving-data/_static/image7.png)

Definieren Sie die Klassen SchoolContext, Studenten, Registrierung und Kurs in dieser Datei wie folgt:

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

Die SchoolContext-Klasse abgeleitet von ' DbContext ' abgeleitet, die die datenbankverbindung und die Änderungen in den Daten verwaltet wird.

Beachten Sie die Attribute, die auf angewendet wurden, in der Klasse Student die **FirstName**, **LastName**, und **Jahr** Eigenschaften. Diese Attribute werden für die datenüberprüfung in diesem Lernprogramm verwendet werden. Um den Code für diese Demo-Projekt zu vereinfachen, wurden nur diese Eigenschaften mit Data-Validierungsattributen gekennzeichnet. In einem echten Projekt würde Sie Validierungsattribute für alle Eigenschaften gelten, die überprüfte Daten, z. B. Eigenschaften in der Registrierung und Kurs Klassen benötigen.

UniversityModels.cs zu speichern.

Die Tools für die Code First-Migrationen verwendet zum Einrichten einer Datenbank, basierend auf diesen Klassen.

In **Package Manager Console**, führen Sie den Befehl:  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

Wenn der Befehl erfolgreich ausgeführt wird, erhalten Sie eine Meldung, die besagt, dass die Migrationen aktiviert wurden,

![Aktivieren von Migrationen](retrieving-data/_static/image8.png)

Beachten Sie, dass eine neue Datei mit dem Namen **"Configuration.cs"** erstellt wurde. In Visual Studio wird diese Datei automatisch geöffnet, nachdem er erstellt wurde. Enthält die Konfigurationsklasse eine **Ausgangswert** Methode, die Ihnen ermöglicht, die die Datenbanktabellen mit Testdaten vorab aufzufüllen.

Fügen Sie den folgenden Code, um die Seed-Methode. Sie müssen Hinzufügen einer **mit** -Anweisung für die **ContosoUniversityModelBinding.Models** Namespace.

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

"Configuration.cs" zu speichern.

In der Paket-Manager-Konsole führen Sie den Befehl `add-migration initial`.

Führen Sie dann den Befehl `update-database`.

Wenn Sie eine Ausnahme beim Ausführen dieses Befehls erhalten, ist es möglich, dass die Werte für StudentID und CourseID aus den Werten der Seed-Methode geändert haben. Öffnen Sie die Tabellen in der Datenbank, und suchen Sie vorhandene Werte für StudentID und CourseID. Fügen Sie diese Werte in den Code für das seeding der Registrierung für die Tabelle hinzu.

Die Datenbankdatei wurde hinzugefügt, aber derzeit im Projekt ausgeblendet ist. Klicken Sie auf **alle Dateien anzeigen** die Datei anzuzeigen.

![Alle Dateien anzeigen](retrieving-data/_static/image10.png)

Beachten Sie die MDF-Datei wird jetzt in der App\_Datenordner.

![Datenbankdatei](retrieving-data/_static/image12.png)

Doppelklicken Sie auf die MDF-Datei, und öffnen Sie den Server-Explorer. Die Tabellen jetzt vorhanden und werden mit Daten aufgefüllt.

![Datenbanktabellen](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>Anzeigen von Daten aus verknüpften Tabellen und Studenten

Die Daten in der Datenbank sind Sie nun bereit zum Abrufen der Daten und zeigt sie auf einer Webseite. Verwenden Sie eine **GridView** Steuerelement zum Anzeigen der Daten in Spalten und Zeilen.

Students.aspx öffnen, und suchen Sie die **MainContent** Platzhalter. In dieser Platzhalter Hinzufügen einer **GridView** Steuerelement, das den folgenden Code enthält.

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

Es gibt eine Reihe von wichtiger Konzepte, die in diesem Markupcode für Sie zu beachten. Zuerst, beachten Sie, dass ein Wert, für festgelegt wird die **SelectMethod** Eigenschaft im GridView-Element. Dieser Wert gibt den Namen der Methode, die zum Abrufen von Daten für die GridView verwendet wird. Diese Methode wird im nächsten Schritt erstellt werden. Zweitens, beachten Sie, dass die **ItemType** Eigenschaftensatz Student-Klasse, die Sie zuvor erstellt haben. Durch Festlegen dieses Werts finden Sie in den Eigenschaften dieser Klasse im Markupcode. Beispielsweise enthält die Student-Klasse eine Auflistung mit dem Namen Bereitstellungen. Sie können **Item.Enrollments** auf diese Auflistung abzurufen und dann LINQ-Syntax verwenden, um die Summe der registrierten Gutschriften für Studenten zu abzurufen.

In der Code-Behind-Datei müssen Sie die Methode hinzufügen, die für angegeben wird die **SelectMethod** Wert. Open **Students.aspx.cs**, und fügen **mit** -Anweisungen für die **ContosoUniversityModelBinding.Models** und **System.Data.Entity**Namespaces.

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

Anschließend fügen Sie die folgende Methode hinzu. Beachten Sie, dass die Namen dieser Methode mit dem Namen übereinstimmt, den Sie für die SelectMethod bereitgestellt.

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

Die **Include** Klausel verbessert die Leistung dieser Abfrage jedoch nicht unbedingt erforderlich für die Abfrage funktioniert. Ohne die Include-Klausel würde die Daten abgerufen werden mithilfe des verzögerten Ladens umfasst das Senden, dass eine separate Abfrage an die Datenbank jedes Mal-bezogene Daten abgerufen werden. Durch die Bereitstellung der Include-Klausel, werden die Daten abgerufen, mit unverzüglichem Laden, d. h., alle verwandten Daten über eine einzelne Abfrage der Datenbank abgerufen wird. In Fällen, in denen meisten der zugehörigen Daten werden nicht verwendet wird, sind unverzüglichem Laden kann weniger effizient sein, da mehr Daten abgerufen werden. Allerdings bietet die beste Leistung in diesem Fall unverzüglichem Laden, da die verknüpften Daten für jeden Datensatz angezeigt werden.

Weitere Informationen zu Überlegungen zur Leistung beim Laden von Daten beziehen, finden Sie im Abschnitt **Lazy Eager und explizite Laden von verknüpften Daten** in die [verwandte Lesen von Daten mit dem Entity Framework in einer ASP .NET MVC-Anwendung](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) Thema.

Standardmäßig ist die Daten durch die Werte der Eigenschaft markiert als Schlüssel sortiert. Sie können eine OrderBy-Klausel geben Sie einen anderen Wert für die Sortierung hinzufügen. In diesem Beispiel wird die StudentID-Standardeigenschaft für die Sortierung verwendet. In der [sortieren, Paging und Filtern von Daten](sorting-paging-and-filtering-data.md) Thema, damit Sie der Benutzer für eine Spalte für die Sortierung auswählen.

Führen Sie die Webanwendung, und navigieren Sie zu der Seite "Students". Die Seite "Students" zeigt die folgenden Student-Informationen.

![Anzeigen von Daten](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Automatische Generierung von Modell Bindung Methoden

Wenn Sie über diese Reihe von Lernprogrammen zu arbeiten, können Sie einfach den Code aus dem Lernprogramm zu Ihrem Projekt kopieren. Ein Nachteil dieses Ansatzes ist jedoch, dass Sie nicht über die Funktion von Visual Studio zum automatischen Generieren von Code für die Bindung Modellmethoden bereitgestellten informiert werden können. Wenn Sie auf Ihren eigenen Projekten arbeiten, können Sie automatische codegenerierung speichern, Zeit und Hilfe erhalten Sie einen Eindruck davon, wie einen Vorgang implementiert. Dieser Abschnitt beschreibt die Funktion "Automatische Code generieren". Dieser Abschnitt dient nur zu Informationszwecken und enthält keiner Code, den Sie in Ihrem Projekt implementieren müssen.

Beim Festlegen eines Werts für die **SelectMethod**, **UpdateMethod**, **InsertMethod**, oder **DeleteMethod** Eigenschaften im Markupcode Sie können auswählen, die **erstellen neue Methode** Option.

![Erstellen Sie neue Methode](retrieving-data/_static/image18.png)

Visual Studio nicht nur eine Methode im Code-Behind mit der richtigen Signatur erstellt, sondern generiert auch Implementierungscode um Unterstützung bei der Ausführung des Vorgangs. Wenn Sie zuerst festlegen, dass die **ItemType** Eigenschaft vor der Verwendung der automatischen Code Generation-Funktion den generierten Code verwendet dieses Typs für die Vorgänge. Beispielsweise wird beim Festlegen der Eigenschaft UpdateMethod automatisch der folgende Code generiert:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

Der obige Code müssen erneut, nicht dem Projekt hinzugefügt werden. In den nächsten Lernprogrammen implementieren Sie Methoden zum Aktualisieren, löschen und neue Daten hinzufügen.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Lernprogramm erstellte Modell Datenklassen und eine Datenbank aus dieser Klassen generiert. Sie können die Datenbanktabellen mit Testdaten gefüllt. Sie wurden die modellbindung zum Abrufen von Daten aus der Datenbank verwendet, und klicken Sie dann die Daten in einem GridView angezeigt.

In der nächsten [Lernprogramm](updating-deleting-and-creating-data.md) in dieser Serie, aktivieren Sie aktualisieren, löschen und Erstellen von Daten.

> [!div class="step-by-step"]
> [Nächste](updating-deleting-and-creating-data.md)
