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
ms.openlocfilehash: 118233338112a71216b909b1dabed2333bfa235e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="implementing-inheritance-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-11-of-12"></a>Implementieren der Vererbung mit dem Entity Framework 6 in einer ASP.NET MVC 5-Anwendung (11 12)
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio 2013. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Im vorherigen Lernprogramm behandelt Sie Parallelitätsausnahmen. In diesem Lernprogramm erfahren Sie, wie Vererbung im Datenmodell implementiert.

In einer objektorientierten Programmierung können [Vererbung](http://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)) zur Erleichterung [Wiederverwendung von code](http://en.wikipedia.org/wiki/Code_reuse). In diesem Lernprogramm ändern Sie die `Instructor` und `Student` Klassen, damit sie ableiten eine `Person` Basisklasse, die Eigenschaften, z. B. enthält `LastName` , könne Dozenten und Studenten gemeinsam sind. Sie wird nicht hinzufügen oder Ändern von Webseiten, aber ändern Sie Teil des Codes und diese Änderungen werden automatisch in der Datenbank übernommen.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Optionen für die Zuordnung von Vererbung zu Datenbanktabellen

Die `Instructor` und `Student` Klassen in der `School` Datenmodell haben verschiedene Eigenschaften, die identisch sind:

![Student_and_Instructor_classes](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

Angenommen, Sie möchten, für die Eigenschaften der redundanten Code vermeiden, die von gemeinsam genutzt werden die `Instructor` und `Student` Entitäten. Oder Sie möchten einen Dienst schreiben, der Formatnamen können ohne eine Rolle spielt, ob der Name einer Dozenten oder eines Teilnehmers stammt. Sie erstellen eine `Person` Basisklasse enthält nur diese Eigenschaften gemeinsam genutzt, muss sich die `Instructor` und `Student` Entitäten aus dieser Basisklasse erben, wie in der folgenden Abbildung gezeigt:

![Student_and_Instructor_classes_deriving_from_Person_class](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

Es gibt mehrere Möglichkeiten, die diese Vererbungsstruktur in der Datenbank dargestellt werden konnte. Sie möglicherweise eine `Person` Tabelle Informationen zu Studenten und Lehrkräfte in einer einzelnen Tabelle enthält. Einige Spalten könnte gelten nur für Lehrkräfte (`HireDate`), einige nur für Studenten (`EnrollmentDate`), einige sowohl (`LastName`, `FirstName`). In der Regel müssen eine *Unterscheidungseigenschaft* Spalte, um anzugeben, welche Typen von jeder Zeile darstellt. Z. B. möglicherweise Unterscheidungsspalte für Studenten "Instructor" für Dozenten und "Student" haben.

![Tabelle pro hierarchy_example](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Dieses Muster für eine Entität Vererbungsstruktur aus einer einzelnen Datenbanktabelle generiert heißt *Tabelle pro Hierarchie* Vererbung (TPH).

Eine Alternative besteht darin, von der Datenbank eher wie die Vererbungsstruktur. Sie könnte z. B. nur die Namensfelder besitzen, der `Person` Tabelle und über separate `Instructor` und `Student` Tabellen mit den Datumsfeldern.

![Tabelle pro type_inheritance](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Dieses Muster versteht man eine Datenbanktabelle für jede Entitätsklasse aufgerufen *Tabelle pro Typ* Vererbung (TPT).

Ist noch eine weitere Option, um einzelne Tabellen alle nicht abstrakten Typen zuzuordnen. Alle Eigenschaften einer Klasse, einschließlich der geerbte Eigenschaften sind Spalten der entsprechenden Tabelle zugeordnet. Dieses Muster wird Tabelle pro konkrete klassenvererbung (TPC) bezeichnet. TPC-Vererbung für implementiert, sollte die `Person`, `Student`, und `Instructor` Klassen wie oben gezeigt die `Student` und `Instructor` Tabellen sieht nach dem Implementieren der Vererbung als zum Zeitpunkt bevor unterscheidet.

TPC und TPH Muster der methodenvererbung aufgeführt übermittelt werden, im Allgemeinen eine bessere Leistung im Entity Framework als TPT Muster der methodenvererbung aufgeführt, weil TPT Muster komplexe Verknüpfungsabfragen führen können.

Dieses Lernprogramm veranschaulicht die TPH-Vererbung zu implementieren. TPH ist das Standardmuster für die Vererbung in Entity Framework, deshalb müssen Sie lediglich erstellen eine `Person` Klasse, ändern Sie die `Instructor` und `Student` Klassen ableiten `Person`, fügen Sie die neue Klasse, die `DbContext`, und erstellen Sie eine die Migration. (Informationen dazu, wie Sie die anderen Vererbung Muster zu implementieren, finden Sie unter [der Tabelle pro Typ (TPT) Vererbungsmapping](https://msdn.microsoft.com/data/jj591617#2.5) und [Vererbungsmapping der Tabelle pro konkrete Klasse (TPC)](https://msdn.microsoft.com/data/jj591617#2.6) in der MSDN-Website Entity Framework-Dokumentation.)

## <a name="create-the-person-class"></a>Erstellen Sie die Person-Klasse

In der *Modelle* Ordner erstellen *Person.cs* , und Ersetzen Sie den Code durch den folgenden Code:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Stellen Sie Student "und" Instructor-Klassen, die von der Person erben

In *Instructor.cs*, leiten Sie die `Instructor` -Klasse aus den `Person` Klasse, und entfernen Sie die Schlüssel und den Namen der Felder. Der Code wird wie im folgenden Beispiel aussehen:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Nehmen Sie ähnliche Änderungen an *Student.cs*. Die `Student` Klasse wird im folgenden Beispiel aussehen:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="add-the-person-entity-type-to-the-model"></a>Den Typ der Person-Entität dem Modell hinzufügen

In *SchoolContext.cs*, Hinzufügen einer `DbSet` -Eigenschaft für die `Person` Entitätstyp:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Dies ist das Entity Framework muss lediglich um eine Tabelle pro Hierarchie Vererbung konfigurieren. Wie Sie sehen werden, wenn die Datenbank aktualisiert wird, muss ein `Person` anstelle der Tabelle die `Student` und `Instructor` Tabellen.

## <a name="create-and-update-a-migrations-file"></a>Erstellen Sie und aktualisieren Sie eine Datei Migrationen

Geben Sie in der Paket-Manager-Konsole (PMC) den folgenden Befehl ein:

`Add-Migration Inheritance`

Führen Sie die `Update-Database` Befehl in der Systemmonitor. An diesem Punkt schlägt der Befehl fehl, da wir die vorhandene Daten, die Migrationen weiß nicht, wie behandelt haben. Sie erhalten eine Fehlermeldung wie den folgenden Ausdruck:

> *Objekt konnte nicht gelöscht werden "Dbo. Instructor ", da es durch eine FOREIGN KEY-Einschränkung verwiesen wird.*


Open *Migrationen\&Lt; Zeitstempel&gt;\_Inheritance.cs* , und Ersetzen Sie die `Up` -Methode durch folgenden Code:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

Dieser Code übernimmt die folgenden Aufgaben der Datenbank aktualisieren:

- Entfernt foreign Key-Einschränkungen und Indizes, die auf die Student-Tabelle verweisen.
- Benennt die Instructor-Tabelle als Person und nimmt Änderungen, die zum Speichern von Daten Student erforderlich:

    - Fügt NULL-Werte zulassen EnrollmentDate für Studenten hinzu.
    - Fügt Unterscheidungsspalte, um anzugeben, ob eine Zeile für ein Student oder einen Kursleiter bestimmt ist.
    - Macht HireDate seit Student Zeilen Einstellungsdaten keine NULL-Werte zulässt.
    - Fügt ein temporäres Feld, das zum Aktualisieren von Fremdschlüsseln, die auf Studenten verweisen verwendet werden. Beim Kopieren von Studenten in die Person-Tabelle erhalten sie neue primäre Schlüsselwerte.
- Kopiert Daten aus der Tabelle "Student" in der Person-Tabelle. Dies bewirkt, dass Studenten neue Primärschlüsselwerte zugewiesen.
- Behebt Fremdschlüsselwerte, die auf Studenten verweisen.
- Neu erstellt, foreign Key-Einschränkungen und Indizes, die Sie sie jetzt auf der Person-Tabelle verweisen.

(Wenn Sie GUID nicht ganze Zahl als den Typ des primären Schlüssels verwendet haben, Student Primärschlüsselwerte würde nicht ändern müssen und mehrere Schritte wurde konnte ausgelassen.)

Führen Sie die `update-database` erneut aus.

(In einem Produktionssystem würden Sie entsprechende Änderungen an der Down-Methode vornehmen für den Fall, dass Sie jemals, zurückdatieren, um die frühere Datenbankversion verwenden. Für dieses Lernprogramm wird nicht die Down-Methode verwenden Sie.)

> [!NOTE]
> Es ist möglich, andere Fehlermeldungen erhalten, bei der Migration von Daten und festlegen, dass schemaänderungen. Wenn Sie Fehler bei der Migration erhalten Sie kann nicht aufgelöst, das Lernprogramm fort, indem Sie ändern die Verbindungszeichenfolge in der *"Web.config"* Datei oder durch Löschen der Datenbank. Der einfachste Ansatz besteht darin, benennen Sie die Datenbank in der *"Web.config"* Datei. Ändern Sie z. B. den Datenbanknamen in ContosoUniversity2, wie im folgenden Beispiel gezeigt:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.xml?highlight=2)]
> 
> Mit einer neuen Datenbank, es sind keine Daten zu migrieren, und die `update-database` Befehl ist weitaus höheren Wahrscheinlichkeit in ohne Fehler abgeschlossen werden. Informationen zum Löschen der Datenbank finden Sie unter [Gewusst wie: Löschen einer Datenbank aus Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Wenn Sie diesen Ansatz verwenden, um dieses Lernprogramm fortsetzen, überspringen Sie den Bereitstellungsschritt am Ende dieses Lernprogramms oder zu einer neuen Website und die Datenbank bereitstellen. Wenn Sie ein Update auf derselben Website, die, denen Sie bereits bereitstellen die Bereitstellung wurde haben, erhalten EF derselben Fehler, wenn sie Migrationen automatisch ausgeführt wird. Wenn Sie einen Migrationen Fehler beheben möchten, ist die beste Ressource eine der Entity Framework-Foren oder StackOverflow.com.


## <a name="testing"></a>Test

Führen Sie den Standort aus, und wiederholen Sie den verschiedenen Seiten. Alles funktioniert genauso wie zuvor.

In **Server-Explorer** erweitern **Daten Connections\SchoolContext** und dann **Tabellen**, und Sie sehen, dass die **Student** und **Dozenten** Tabellen wurden durch ersetzt eine **Person** Tabelle. Erweitern Sie die **Person** Tabelle, und wird angezeigt, dass sie alle Spalten, die verwendet besitzt werden die **Student** und **Dozenten** Tabellen.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Mit der rechten Maustaste in den Person-Tabelle, und klicken Sie dann auf **Tabellendaten anzeigen** Unterscheidungsspalte angezeigt.

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

Sie haben die Tabelle pro Hierarchie Vererbung für implementiert die `Person`, `Student`, und `Instructor` Klassen. Weitere Informationen zu diesen und anderen Vererbung Strukturen finden Sie unter [TPT Vererbungsmuster](https://msdn.microsoft.com/data/jj618293) und [TPH Vererbungsmuster](https://msdn.microsoft.com/data/jj618292) auf MSDN. In den nächsten Lernprogrammen sehen Sie, wie eine Vielzahl von relativ erweiterte Entity Framework-Szenarien behandelt.

Links zu anderen Entity Framework-Ressourcen finden Sie in der [ASP.NET Data Access - Ressourcen empfohlen](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Zurück](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Weiter](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
