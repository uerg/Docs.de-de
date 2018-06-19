---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementieren der Vererbung mit dem Entity Framework 6 in einer ASP.NET MVC 5-Anwendung (11 12) | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 08834147-77ec-454a-bb7a-d931d2a40dab
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1826659626106993d4796641492c62fcbd22a1b3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873403"
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>Implementieren der Vererbung mit dem Entity Framework 6 in einer ASP.NET MVC 5-Anwendung (11 12)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio 2013. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Im vorherigen Lernprogramm behandelt Sie Parallelitätsausnahmen. In diesem Tutorial erfahren Sie, wie Sie die Vererbung in das Datenmodell implementieren können.

In einer objektorientierten Programmierung können [Vererbung](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) zur Erleichterung [Wiederverwendung von code](http://en.wikipedia.org/wiki/Code_reuse). In diesem Tutorial ändern Sie die Klassen `Instructor` und `Student` so, dass sie von einer `Person`-Basisklasse abgeleitet werden, die Eigenschaften wie `LastName` enthält. Diese Eigenschaften sind für Dozenten und Studenten gängig. Sie fügen keine Webseiten hinzu oder ändern diese, aber Sie werden Teile des Codes ändern. Diese Änderungen werden automatisch in der Datenbank widergespiegelt.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Optionen für die Zuordnung von Vererbung zu Datenbanktabellen

Die `Instructor` und `Student` Klassen in der `School` Datenmodell haben verschiedene Eigenschaften, die identisch sind:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Angenommen, Sie möchten den redundanten Code für die Eigenschaften löschen, die von den Entitäten `Instructor` und `Student` gemeinsam genutzt werden. Oder Sie möchten einen Dienst schreiben, mit dem Namen formatiert werden können, ohne dass es eine Rolle spielt, ob der Name von einem Dozenten oder von einem Studenten stammt. Sie erstellen eine `Person` Basisklasse enthält nur diese Eigenschaften gemeinsam genutzt, muss sich die `Instructor` und `Student` Entitäten aus dieser Basisklasse erben, wie in der folgenden Abbildung gezeigt:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Es gibt mehrere Möglichkeiten, wie diese Vererbungsstruktur in der Datenbank dargestellt werden kann. Sie möglicherweise eine `Person` Tabelle Informationen zu Studenten und Lehrkräfte in einer einzelnen Tabelle enthält. Einige Spalten könnte gelten nur für Lehrkräfte (`HireDate`), einige nur für Studenten (`EnrollmentDate`), einige sowohl (`LastName`, `FirstName`). In der Regel müssen eine *Unterscheidungseigenschaft* Spalte, um anzugeben, welche Typen von jeder Zeile darstellt. So kann die Unterscheidungsspalte beispielsweise „Instructor“ für Dozenten und „Student“ für Studenten enthalten.

![Tabelle pro hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Dieses Muster für eine Entität Vererbungsstruktur aus einer einzelnen Datenbanktabelle generiert heißt *Tabelle pro Hierarchie* Vererbung (TPH).

Alternativ kann die Datenbank so gestaltet werden, dass sie mehr wie die Vererbungsstruktur aussieht. Sie könnte z. B. nur die Namensfelder besitzen, der `Person` Tabelle und über separate `Instructor` und `Student` Tabellen mit den Datumsfeldern.

![Tabelle pro type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Dieses Muster versteht man eine Datenbanktabelle für jede Entitätsklasse aufgerufen *Tabelle pro Typ* Vererbung (TPT).

Eine weitere Möglichkeit besteht darin, individuellen Tabellen alle nicht abstrakten Typen zuzuordnen. Alle Eigenschaften einer Klasse, einschließlich der geerbten Eigenschaften, werden Spalten der entsprechenden Tabelle zugeordnet. Dieses Muster wird als TPC-Vererbung (TPC = Table per Concrete, Tabelle pro konkretem Typ) bezeichnet. TPC-Vererbung für implementiert, sollte die `Person`, `Student`, und `Instructor` Klassen wie oben gezeigt die `Student` und `Instructor` Tabellen sieht nach dem Implementieren der Vererbung als zum Zeitpunkt bevor unterscheidet.

TPC und TPH Muster der methodenvererbung aufgeführt übermittelt werden, im Allgemeinen eine bessere Leistung im Entity Framework als TPT Muster der methodenvererbung aufgeführt, weil TPT Muster komplexe Verknüpfungsabfragen führen können.

Dieses Tutorial veranschaulicht die Implementierung der TPH-Vererbung. TPH ist das Standardmuster für die Vererbung in Entity Framework, deshalb müssen Sie lediglich erstellen eine `Person` Klasse, ändern Sie die `Instructor` und `Student` Klassen ableiten `Person`, fügen Sie die neue Klasse, die `DbContext`, und erstellen Sie eine die Migration. (Informationen dazu, wie Sie die anderen Vererbung Muster zu implementieren, finden Sie unter [der Tabelle pro Typ (TPT) Vererbungsmapping](https://msdn.microsoft.com/data/jj591617#2.5) und [Vererbungsmapping der Tabelle pro konkrete Klasse (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) in der MSDN-Website Entity Framework-Dokumentation.)

## <a name="create-the-person-class"></a>Erstellen der Klasse „Person“

In der *Modelle* Ordner erstellen *Person.cs* , und Ersetzen Sie den Code durch den folgenden Code:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Veranlassen der Vererbung von der Klasse „Person“ an die Klassen „Student“ und „Instructor“

In *Instructor.cs*, leiten Sie die `Instructor` -Klasse aus den `Person` Klasse, und entfernen Sie die Schlüssel und den Namen der Felder. Der Code sieht aus wie im folgenden Beispiel:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Nehmen Sie ähnliche Änderungen an *Student.cs*. Die `Student` Klasse wird im folgenden Beispiel aussehen:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>Den Typ der Person-Entität dem Modell hinzufügen

In *SchoolContext.cs*, Hinzufügen einer `DbSet` -Eigenschaft für die `Person` Entitätstyp:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Das ist alles, was Entity Framework für die Konfiguration der „Tabelle pro Hierarchie“-Vererbung benötigt. Wie Sie sehen werden, wenn die Datenbank aktualisiert wird, muss ein `Person` anstelle der Tabelle die `Student` und `Instructor` Tabellen.

## <a name="create-and-update-a-migrations-file"></a>Erstellen Sie und aktualisieren Sie eine Datei Migrationen

Geben Sie in der Paket-Manager-Konsole (PMC) den folgenden Befehl ein:

`Add-Migration Inheritance`

Führen Sie die `Update-Database` Befehl in der Systemmonitor. An diesem Punkt schlägt der Befehl fehl, da wir die vorhandene Daten, die Migrationen weiß nicht, wie behandelt haben. Sie erhalten eine Fehlermeldung wie den folgenden Ausdruck:

> *Objekt konnte nicht gelöscht werden "Dbo. Instructor ", da es durch eine FOREIGN KEY-Einschränkung verwiesen wird.*


Open *Migrationen\&Lt; Zeitstempel&gt;\_Inheritance.cs* , und Ersetzen Sie die `Up` -Methode durch folgenden Code:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Dieser Code übernimmt folgende Tasks für Datenbankaktualisierungen:

- Er entfernt Fremdschlüsseleinschränkungen und -indizes, die auf die Tabelle „Student“ verweisen.
- Er benennt die Tabelle „Instructor“ in „Person“ um und nimmt die Änderungen vor, die für das Speichern von Studentendaten erforderlich sind:

    - Er fügt das EnrollmentDate für Studenten hinzu, bei dem NULL-Werte zugelassen sind.
    - Er fügt eine Unterscheidungsspalte hinzu, um anzugeben, ob eine Zeile für einen Studenten oder für einen Dozenten bestimmt ist.
    - Er legt fest, dass bei HireDate NULL-Werte zugelassen sind, da die Zeilen für Studenten keine Einstellungsdaten enthalten.
    - Er fügt ein temporäres Feld hinzu, über das Fremdschlüssel aktualisiert werden sollen, die auf Studenten verweisen. Beim Kopieren von Studenten in die Person-Tabelle erhalten sie neue primäre Schlüsselwerte.
- Kopiert Daten aus der Tabelle „Student“ in die Tabelle „Person“. Dadurch werden Studenten neue Primärschlüsselwerte zugewiesen.
- Er legt Fremdschlüsselwerte fest, die auf Studenten verweisen.
- Er erstellt Fremdschlüsseleinschränkungen und -indizes neu, die dann auf die Tabelle „Person“ verweisen.

(Hätten Sie als Primärschlüsseltyp statt einem Integer die grafische Benutzeroberfläche verwendet haben, hätten die Primärschlüsselwerte für Studenten nicht geändert werden müssen, und mehrere dieser Schritte hätten ausgelassen werden können.)

Führen Sie die `update-database` erneut aus.

(In einem Produktionssystem würden Sie entsprechende Änderungen an der Down-Methode vornehmen für den Fall, dass Sie jemals, zurückdatieren, um die frühere Datenbankversion verwenden. Für dieses Lernprogramm wird nicht die Down-Methode verwenden Sie.)

> [!NOTE]
> Es ist möglich, andere Fehlermeldungen erhalten, bei der Migration von Daten und festlegen, dass schemaänderungen. Wenn Sie Fehler bei der Migration erhalten Sie kann nicht aufgelöst, das Lernprogramm fort, indem Sie ändern die Verbindungszeichenfolge in der *"Web.config"* Datei oder durch Löschen der Datenbank. Der einfachste Ansatz besteht darin, benennen Sie die Datenbank in der *"Web.config"* Datei. Ändern Sie z. B. den Datenbanknamen in ContosoUniversity2, wie im folgenden Beispiel gezeigt:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> Mit einer neuen Datenbank, es sind keine Daten zu migrieren, und die `update-database` Befehl ist weitaus höheren Wahrscheinlichkeit in ohne Fehler abgeschlossen werden. Informationen zum Löschen der Datenbank finden Sie unter [Gewusst wie: Löschen einer Datenbank aus Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Wenn Sie diesen Ansatz verwenden, um dieses Lernprogramm fortsetzen, überspringen Sie den Bereitstellungsschritt am Ende dieses Lernprogramms oder zu einer neuen Website und die Datenbank bereitstellen. Wenn Sie ein Update auf derselben Website, die, denen Sie bereits bereitstellen die Bereitstellung wurde haben, erhalten EF derselben Fehler, wenn sie Migrationen automatisch ausgeführt wird. Wenn Sie einen Migrationen Fehler beheben möchten, ist die beste Ressource eine der Entity Framework-Foren oder StackOverflow.com.


## <a name="testing"></a>Test

Führen Sie den Standort aus, und wiederholen Sie den verschiedenen Seiten. Alles funktioniert genauso wie vorher.

In **Server-Explorer** erweitern **Daten Connections\SchoolContext** und dann **Tabellen**, und Sie sehen, dass die **Student** und **Dozenten** Tabellen wurden durch ersetzt eine **Person** Tabelle. Erweitern Sie die **Person** Tabelle, und wird angezeigt, dass sie alle Spalten, die verwendet besitzt werden die **Student** und **Dozenten** Tabellen.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Klicken Sie mit der rechten Maustaste auf die Tabelle „Person“, und klicken Sie anschließend auf **Tabellendaten anzeigen**, um die Unterscheidungsspalte anzuzeigen.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Das folgende Diagramm veranschaulicht die Struktur der neuen Datenbank "School":

![School_database_diagram](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

## <a name="deploy-to-azure"></a>In Azure bereitstellen

In diesem Abschnitt müssen Sie das optionale abgeschlossener **Bereitstellen der app in Azure** im Abschnitt [Teil 3, sortieren, Filtern und Paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md) dieser Reihe von Lernprogrammen. Wenn Sie Migrationen Fehler, die Sie aufgetreten durch Löschen der Datenbank in Ihrem lokalen Projekt behoben, überspringen Sie diesen Schritt; oder erstellen Sie eine neue Website und die Datenbank und in die neue Umgebung bereitstellen.

1. In Visual Studio mit der Maustaste des Projekts im **Projektmappen-Explorer** , und wählen Sie **veröffentlichen** aus dem Kontextmenü.  
  
    ![Im Projektkontextmenü veröffentlichen](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)
2. Klicken Sie auf **Veröffentlichen**.  
  
    ![publish](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)  
  
   Die Web-app wird in Ihrem Standardbrowser geöffnet.
3. Testen der Anwendung, überprüfen Sie, ob sie funktioniert.

    Beim ersten einer Seite ausführen, auf die Datenbank zugreift, wird das Entity Framework ausgeführt aller die Migrationen `Up` Methoden erforderlich, um die Datenbank mit dem aktuellen Datenmodell auf dem neuesten Stand zu bringen.

## <a name="summary"></a>Zusammenfassung

Sie haben die „Tabelle pro Hierarchie“-Vererbung für die Klassen `Person`, `Student` und `Instructor` implementiert. Weitere Informationen zu diesen und anderen Vererbung Strukturen finden Sie unter [TPT Vererbungsmuster](https://msdn.microsoft.com/data/jj618293) und [TPH Vererbungsmuster](https://msdn.microsoft.com/data/jj618292) auf MSDN. Im nächsten Tutorial erfahren Sie, wie Sie eine Vielzahl von Entity Framework-Szenarios auf fortgeschrittenem Niveau verarbeiten können.

Links zu anderen Entity Framework-Ressourcen finden Sie in der [ASP.NET Data Access - Ressourcen empfohlen](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Weiter](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
