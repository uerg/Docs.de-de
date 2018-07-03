---
uid: web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
title: Abrufen und Anzeigen von Daten mit model-Bindung und Web Forms | Microsoft-Dokumentation
author: tfitzmac
description: Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung macht die dateninteraktion Weitere gerade-...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 9f24fb82-c7ac-48da-b8e2-51b3da17e365
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data
msc.type: authoredcontent
ms.openlocfilehash: 4a76fcfd2620eb0789b7a6a23990037a1eebaf4c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378494"
---
<a name="retrieving-and-displaying-data-with-model-binding-and-web-forms"></a>Abrufen und Anzeigen von Daten mit modellbindung und Web forms
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Diese tutorialreihe veranschaulicht die grundlegenden Aspekte der Verwendung von modellbindung mit einem ASP.NET Web Forms-Projekt. Modellbindung, wird die dateninteraktion mehr geradlinigere als Umgang mit Data Source-Objekte (z. B. "ObjectDataSource" oder SqlDataSource-Steuerelement). Diese Serie beginnt mit einführendes Material und verschiebt in späteren Tutorials zu erweiterten Konzepten übergegangen.
> 
> Der Modell-bindungsmuster funktioniert mit jedem datenzugriffstechnologie. In diesem Tutorial verwenden Sie Entity Framework, jedoch können Sie die neue datenzugriffstechnologie, die Sie bereits bekannt ist. Von einem datengebundenen Serversteuerelement, z. B. eine GridView, ListView, DetailsView oder FormView-Steuerelement geben Sie den Namen der Methoden zum auswählen, aktualisieren, löschen und Erstellen von Daten verwendet. In diesem Tutorial werden Sie einen Wert für die SelectMethod angeben.
> 
> In dieser Methode geben Sie die Logik zum Abrufen der Daten. Im nächsten Tutorial legen Sie Werte für UpdateMethod, DeleteMethod und InsertMethod fest.
> 
> Sie können [herunterladen](https://go.microsoft.com/fwlink/?LinkId=286116) das vollständige Projekt in c# oder VB. Der herunterladbare Code funktioniert mit Visual Studio 2012 oder Visual Studio 2013. Er verwendet die Visual Studio 2012-Vorlage, die unterscheidet sich etwas von der Visual Studio 2013-Vorlage, die in diesem Tutorial gezeigt wird.
> 
> In diesem Tutorial führen Sie die Anwendung in Visual Studio. Sie können auch die Anwendung verfügbar über das Internet, indem sie in einem Hostinganbieter bereitstellen. Microsoft bietet kostenloses Webhosting für bis zu 10 Websites in einem  
>  [Kostenloses Azure-Testkonto](https://azure.microsoft.com/free/?WT.mc_id=A443DD604). Informationen dazu, wie Sie ein Visual Studio-Webprojekt für Azure App Service-Web-Apps bereitstellen, finden Sie unter den [ASP.NET-webbereitstellung mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md) Reihe. In diesem Tutorial wird gezeigt, wie Entity Framework Code First-Migrationen zu verwenden, um Ihre SQL Server-Datenbank in Azure SQL-Datenbank bereitzustellen.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - Microsoft Visual Studio 2013 oder Microsoft Visual Studio Express 2013, für das Web
>   
> 
> In diesem Tutorial funktioniert auch mit Visual Studio 2012, aber es werden einige Unterschiede in der Vorlage "Benutzer"-Schnittstelle und Projekt vorhanden sein.


## <a name="what-youll-build"></a>Sie lernen Folgendes

In diesem Tutorial müssen Sie folgende Aktionen ausführen:

1. Erstellen Sie Datenobjekte, die eine Universität mit Schüler/Studenten, die Lehrveranstaltungen widerspiegeln.
2. Erstellen von Tabellen aus den Objekten
3. Auffüllen der Datenbank mit Testdaten
4. Anzeigen von Daten in einem Web form

## <a name="set-up-project"></a>Einrichten des Projekts

In Visual Studio 2013, erstellen Sie ein neues **ASP.NET-Webanwendung** namens **ContosoUniversityModelBinding**.

![Projekt erstellen](retrieving-data/_static/image2.png)

Wählen Sie die Web Forms-Vorlage, und lassen Sie die andere Standardoptionen. Klicken Sie auf OK, um das Projekt einrichten.

![Wählen Sie die Web forms](retrieving-data/_static/image3.png)

Zunächst wird ein paar kleinen Änderungen anpassen die Darstellung des Standorts erstellt. Öffnen der **Site.Master** Datei, und ändern Sie den Titel aus, um Contoso University My ASP.NET Application anstelle von aufzunehmen.

[!code-aspx[Main](retrieving-data/samples/sample1.aspx?highlight=1)]

Ändern Sie dann den Headertext von **Anwendungsname** zu **Contoso University**.

[!code-aspx[Main](retrieving-data/samples/sample2.aspx?highlight=7)]

Ändern Sie auch in Site.master-Seite, der Navigationslinks, die angezeigt, in der Kopfzeile werden auf die Seiten entsprechen, die diesem Standort relevant sind. Sie benötigen nicht entweder die **zu** Seite oder die **wenden Sie sich an** Seite, damit diese Links entfernt werden kann. Stattdessen benötigen Sie einen Link zu einer Seite mit dem Namen **Schüler/Studenten**. Diese Seite wurde noch nicht erstellt.

[!code-aspx[Main](retrieving-data/samples/sample3.aspx)]

Speichern Sie und schließen Sie Site.Master.

Nun erstellen Sie das Web Form für die Anzeige von Schüler-und Studentendaten. Mit der rechten Maustaste in Ihrem Projekts und **hinzufügen** eine **neues Element**. Wählen Sie die **Webformular mit Gestaltungsvorlage** Vorlage, und nennen Sie sie **Students.aspx**.

![Seite erstellen](retrieving-data/_static/image5.png)

Wählen Sie **Site.Master** als die Masterseite für das neue Webformular.

## <a name="create-the-data-models-and-database"></a>Erstellen Sie die Datenmodelle und Datenbank

Sie können Code First-Migrationen, um Objekte und den entsprechenden Datenbanktabellen erstellen. Diese Tabellen werden Informationen über die Schüler/Studenten und ihre Kurse gespeichert.

Fügen Sie im Ordner "Models", eine neue Klasse, die mit dem Namen **UniversityModels.cs**.

![Erstellen von Model-Klasse](retrieving-data/_static/image7.png)

Definieren Sie die Klassen SchoolContext, Studenten, Registrierung und Kurs in dieser Datei wie folgt:

[!code-csharp[Main](retrieving-data/samples/sample4.cs)]

Die SchoolContext-Klasse leitet sich von "DbContext", die die datenbankverbindung und Änderungen an den Daten verwaltet.

Beachten Sie die Attribute, die auf angewendet wurden, in der Klasse "Student", die **FirstName**, **"LastName"**, und **Jahr** Eigenschaften. Diese Attribute werden für die datenvalidierung in diesem Tutorial verwendet werden. Um den Code für dieses Projekt für die Demo zu vereinfachen, wurden nur diese Eigenschaften mit datenvalidierung Attributen markiert. In einem echten Projekt würde Sie Validierungsattribute, die allen Eigenschaften anwenden, die überprüften Daten, z. B. Eigenschaften in der Registrierung und Kurs Klassen benötigen.

UniversityModels.cs zu speichern.

Sie können die Tools für Code First-Migrationen zum Einrichten einer Datenbank, die basierend auf diesen Klassen.

In **-Paket-Manager-Konsole**, führen Sie den Befehl:  
`enable-migrations -ContextTypeName ContosoUniversityModelBinding.Models.SchoolContext`

Wenn der Befehl erfolgreich abgeschlossen wurde, erhalten Sie eine Nachricht, die darauf hinweist, dass Migrationen aktiviert wurden,

![Aktivieren von Migrationen](retrieving-data/_static/image8.png)

Beachten Sie, dass eine neue Datei mit dem Namen **Configuration.cs** erstellt wurde. In Visual Studio wird diese Datei automatisch geöffnet, nachdem es erstellt wurde. Die Configuration-Klasse enthält eine **Ausgangswert** Methode mit der Sie die Datenbanktabellen mit Testdaten vorab auffüllen.

Fügen Sie den folgenden Code, auf die Seed-Methode. Sie müssen Hinzufügen einer **mit** -Anweisung für die **ContosoUniversityModelBinding.Models** Namespace.

[!code-csharp[Main](retrieving-data/samples/sample5.cs)]

Configuration.cs zu speichern.

Führen Sie in der Paket-Manager-Konsole den Befehl `add-migration initial`.

Führen Sie dann den Befehl `update-database`.

Wenn Sie eine Ausnahme beim Ausführen dieses Befehls erhalten, ist es möglich, dass die Werte für "StudentID" und CourseID von den Werten in die Seed-Methode geändert haben. Öffnen Sie die Tabellen in der Datenbank aus, und suchen Sie vorhandene Werte für "StudentID" und CourseID. Fügen Sie diese Werte in den Code für das seeding der Registrierungen Tabelle hinzu.

Die Datenbankdatei wurde hinzugefügt, aber derzeit im Projekt ausgeblendet ist. Klicken Sie auf **alle Dateien anzeigen** zum Anzeigen der Datei.

![Alle Dateien anzeigen](retrieving-data/_static/image10.png)

Beachten Sie, dass die MDF-Datei wird jetzt in der App\_Datenordner.

![Datenbankdatei](retrieving-data/_static/image12.png)

Doppelklicken Sie auf die MDF-Datei, und öffnen Sie den Server-Explorer. Die Tabellen werden jetzt vorhanden und werden mit Daten gefüllt.

![Datenbanktabellen](retrieving-data/_static/image14.png)

## <a name="display-data-from-students-and-related-tables"></a>Anzeigen von Daten aus der Schüler/Studenten und verknüpften Tabellen

Mit Daten in der Datenbank können Sie nun die Daten abzurufen und auf einer Webseite anzuzeigen. Sie können eine **GridView** Steuerelement zum Anzeigen der Daten in Spalten und Zeilen.

Students.aspx öffnen, und suchen die **MainContent** Platzhalter. In dieser Platzhalter Hinzufügen einer **GridView** -Steuerelement, das den folgenden Code enthält.

[!code-aspx[Main](retrieving-data/samples/sample6.aspx)]

Es gibt eine Reihe wichtiger Konzepte in diesem Markupcode für Sie zu beachten. Erstens ist zu beachten, dass ein Wert, für festgelegt ist die **SelectMethod** Eigenschaft im GridView-Element. Dieser Wert gibt den Namen der Methode, die zum Abrufen von Daten für die GridView verwendet wird. Im nächsten Schritt erstellen Sie diese Methode. Zweitens: Beachten Sie, dass die **ItemType** -Eigenschaftensatz auf die Student-Klasse, die Sie zuvor erstellt haben. Durch Festlegen dieses Werts können Sie auf Eigenschaften dieser Klasse im Markupcode verweisen. Die Klasse "Student" enthält beispielsweise eine Sammlung namens Registrierungen. Sie können **Item.Enrollments** zu dieser Auflistung abzurufen, und klicken Sie dann LINQ-Syntax verwenden, um die Summe der registrierten Gutschriften für jeden Kursteilnehmer zu abzurufen.

In der Code-Behind-Datei müssen Sie die Methode, die für angegeben wird, Hinzufügen der **SelectMethod** Wert. Open **Students.aspx.cs**, und fügen **mit** -Anweisungen für die **ContosoUniversityModelBinding.Models** und **System.Data.Entity**Namespaces.

[!code-csharp[Main](retrieving-data/samples/sample7.cs)]

Fügen Sie dann die folgende Methode hinzu. Beachten Sie, dass der Name dieser Methode mit dem Namen übereinstimmt, die, den Sie für die SelectMethod bereitgestellt.

[!code-csharp[Main](retrieving-data/samples/sample8.cs)]

Die **Include** Klausel verbessert die Leistung der Abfrage jedoch nicht unbedingt erforderlich für die Abfrage funktioniert. Ohne die Include-Klausel würden die Daten mithilfe von lazy Loading und dazu senden, dass eine separate Abfrage an die Datenbank jedes Mal-bezogene Daten abgerufen werden abgerufen werden. Durch die Bereitstellung der Include-Klausel, werden die Daten abgerufen über eager Loading, d.h. alle zugehörigen Daten wird durch eine einzelne Abfrage der Datenbank abgerufen. In Fällen, in denen meisten der zugehörigen Daten nicht verwendet wird, eager Loading kann weniger effizient sein, da mehr Daten abgerufen werden. Allerdings bietet die beste Leistung in diesem Fall Eager Load daran, dass die verknüpften Daten für jeden Datensatz angezeigt werden.

Weitere Informationen zu Überlegungen zur Leistung beim Laden von Daten beziehen, finden Sie im Abschnitt **explizite Laden von verknüpften Daten, mittels Eager Load und Lazy** in die [verwandte Lesen von Daten mit dem Entity Framework in einer ASP .NET MVC-Anwendung](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md) Thema.

Standardmäßig ist die Daten durch die Werte der Eigenschaft als Schlüssel markiert sortiert. Sie können eine OrderBy-Klausel zum Angeben eines anderen Werts für die Sortierung hinzufügen. In diesem Beispiel wird die StudentID-Standardeigenschaft für die Sortierung verwendet. In der [sortieren, Paging und Filtern von Daten](sorting-paging-and-filtering-data.md) Thema aktivieren Sie die Benutzer die Auswahl eine Spalte zum Sortieren.

Führen Sie Ihre Webanwendung, und navigieren Sie zur studentenseite. Seite für Studenten zeigt die folgenden Informationen für Schüler und Studenten.

![Anzeigen von Daten](retrieving-data/_static/image16.png)

## <a name="automatic-generation-of-model-binding-methods"></a>Automatische Generierung von SMO-Methoden-Bindung

Wenn dieser tutorialreihe durcharbeiten, können Sie den Code einfach aus dem Lernprogramm zu Ihrem Projekt kopieren. Ein Nachteil dieses Ansatzes ist jedoch, dass Sie nicht über die Funktion, die von Visual Studio zum automatischen Generieren von Code für die Bindung SMO-Methoden bereitgestellt werden können. Bei der Arbeit an Ihre eigenen Projekte sparen automatische codegenerierung Sie Zeit und die Hilfe, die Sie wissen, wie Implementieren eines Vorgangs erhalten. Dieser Abschnitt beschreibt die automatischen Code Generation-Funktion. In diesem Abschnitt dient nur zu Informationszwecken und keinen Code, die, den Sie in Ihrem Projekt implementieren müssen.

Beim Festlegen eines Werts für die **SelectMethod**, **UpdateMethod**, **InsertMethod**, oder **DeleteMethod** Eigenschaften im Markupcode Sie können auswählen, die **neue Methode erstellen** Option.

![Erstellen Sie neue Methode](retrieving-data/_static/image18.png)

Visual Studio nicht nur eine Methode im Code-Behind mit der richtigen Signatur erstellt, sondern generiert auch Code zur Implementierung um Unterstützung bei der der Vorgang ausgeführt. Wenn Sie zuerst festlegen, die **ItemType** Eigenschaft, bevor Sie die automatische Code Generation-Funktion, generierten Codes mithilfe dieses Typs für die Vorgänge verwenden. Beispielsweise wird bei der die UpdateMethod-Eigenschaft festlegen, automatisch der folgende Code generiert:

[!code-csharp[Main](retrieving-data/samples/sample9.cs)]

In diesem Fall muss der obige Code nicht zu Ihrem Projekt hinzugefügt werden. Im nächsten Tutorial implementieren Sie Methoden zum Aktualisieren, löschen und neue Daten hinzufügen.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Tutorial erstellten Data Model-Klassen und eine Datenbank aus den Klassen generiert. Sie können Tabellen der Datenbank mit Testdaten gefüllt. Sie wurden die modellbindung zum Abrufen von Daten aus der Datenbank verwendet, und klicken Sie dann die Daten in einer GridView-Ansicht angezeigt.

In den nächsten [Tutorial](updating-deleting-and-creating-data.md) in dieser Reihe, aktivieren Sie aktualisieren, löschen und Erstellen von Daten.

> [!div class="step-by-step"]
> [Nächste](updating-deleting-and-creating-data.md)
