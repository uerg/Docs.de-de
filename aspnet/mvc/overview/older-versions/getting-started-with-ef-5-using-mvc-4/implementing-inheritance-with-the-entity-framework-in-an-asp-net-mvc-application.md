---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementieren der Vererbung mit dem Entity Framework in einer ASP.NET MVC-Anwendung (8 10) | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: ee088f841bdb68f4806b0b62be7d379b9eab9f8c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Implementieren der Vererbung mit dem Entity Framework in einer ASP.NET MVC-Anwendung (8 10)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First und Visual Studio 2012. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die Reihe von Lernprogrammen vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn ein Problem Sie können nicht aufgelöst auftreten, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie, das Problem zu reproduzieren. Die Lösung des Problems finden in der Regel durch Vergleich des Codes den fertigen Code. Einige häufige Fehler und deren Lösung finden Sie unter [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Im vorherigen Lernprogramm behandelt Sie Parallelitätsausnahmen. In diesem Tutorial erfahren Sie, wie Sie die Vererbung in das Datenmodell implementieren können.

Vererbung können Sie in einer objektorientierten Programmierung um redundanten Code zu vermeiden. In diesem Tutorial ändern Sie die Klassen `Instructor` und `Student` so, dass sie von einer `Person`-Basisklasse abgeleitet werden, die Eigenschaften wie `LastName` enthält. Diese Eigenschaften sind für Dozenten und Studenten gängig. Sie fügen keine Webseiten hinzu oder ändern diese, aber Sie werden Teile des Codes ändern. Diese Änderungen werden automatisch in der Datenbank widergespiegelt.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabelle pro Hierarchie im Vergleich zu Tabelle pro Typ Vererbung

Vererbung können Sie in einer objektorientierten Programmierung mit verwandten Klassen arbeiten erleichtern. Z. B. die `Instructor` und `Student` Klassen in der `School` Datenmodell freigeben mehrere Eigenschaften führt zu redundanter Code:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Angenommen, Sie möchten den redundanten Code für die Eigenschaften löschen, die von den Entitäten `Instructor` und `Student` gemeinsam genutzt werden. Sie erstellen eine `Person` Basisklasse enthält nur diese Eigenschaften gemeinsam genutzt, muss sich die `Instructor` und `Student` Entitäten aus dieser Basisklasse erben, wie in der folgenden Abbildung gezeigt:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Es gibt mehrere Möglichkeiten, wie diese Vererbungsstruktur in der Datenbank dargestellt werden kann. Sie möglicherweise eine `Person` Tabelle Informationen zu Studenten und Lehrkräfte in einer einzelnen Tabelle enthält. Einige Spalten könnte gelten nur für Lehrkräfte (`HireDate`), einige nur für Studenten (`EnrollmentDate`), einige sowohl (`LastName`, `FirstName`). In der Regel müssen eine *Unterscheidungseigenschaft* Spalte, um anzugeben, welche Typen von jeder Zeile darstellt. So kann die Unterscheidungsspalte beispielsweise „Instructor“ für Dozenten und „Student“ für Studenten enthalten.

![Tabelle pro hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Dieses Muster für eine Entität Vererbungsstruktur aus einer einzelnen Datenbanktabelle generiert heißt *Tabelle pro Hierarchie* Vererbung (TPH).

Alternativ kann die Datenbank so gestaltet werden, dass sie mehr wie die Vererbungsstruktur aussieht. Sie könnte z. B. nur die Namensfelder besitzen, der `Person` Tabelle und über separate `Instructor` und `Student` Tabellen mit den Datumsfeldern.

![Tabelle pro type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Dieses Muster versteht man eine Datenbanktabelle für jede Entitätsklasse aufgerufen *Tabelle pro Typ* Vererbung (TPT).

TPH Muster der methodenvererbung aufgeführt übermitteln im Entity Framework als TPT Muster der methodenvererbung aufgeführt, in der Regel eine bessere Leistung, TPT Muster können komplexe Join-Abfragen ergeben. Dieses Tutorial veranschaulicht die Implementierung der TPH-Vererbung. Sie müssen zu diesem Zweck die folgenden Schritte ausführen:

- Erstellen einer `Person` Klasse, und Ändern der `Instructor` und `Student` Klassen ableiten `Person`.
- Der Context-Klasse für Datenbank-Modelldatenbank Zuordnungscode hinzugefügt.
- Änderung `InstructorID` und `StudentID` Verweise im Verlauf des Projekts `PersonID`.

## <a name="creating-the-person-class"></a>Erstellen der Personenklasse

 Hinweis: Kann nicht zum Kompilieren Sie das Projekt nach dem Erstellen der Klassen nachstehend bis zur Aktualisierung der Domänencontroller, auf denen diese Klassen verwendet. 

In der *Modelle* Ordner erstellen *Person.cs* , und Ersetzen Sie den Code durch den folgenden Code:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

In *Instructor.cs*, leiten Sie die `Instructor` -Klasse aus den `Person` Klasse, und entfernen Sie die Schlüssel und den Namen der Felder. Der Code sieht aus wie im folgenden Beispiel:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Nehmen Sie ähnliche Änderungen an *Student.cs*. Die `Student` Klasse wird im folgenden Beispiel aussehen:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Hinzufügen des Person-Entität vom Typs zum Modell

In *SchoolContext.cs*, Hinzufügen einer `DbSet` -Eigenschaft für die `Person` Entitätstyp:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Das ist alles, was Entity Framework für die Konfiguration der „Tabelle pro Hierarchie“-Vererbung benötigt. Wie Sie sehen werden, wenn die Datenbank neu erstellt wird, muss ein `Person` anstelle der Tabelle die `Student` und `Instructor` Tabellen.

## <a name="changing-instructorid-and-studentid-to-personid"></a>Veränderliche InstructorID und StudentID an PersonID

In *SchoolContext.cs*, ändern Sie in der Anweisung Instructor-Kurs Zuordnung `MapRightKey("InstructorID")` auf `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Diese Änderung ist nicht erforderlich; nur der Name der Spalte in der Jointabelle m: n-InstructorID geändert. Wenn Sie den Namen als InstructorID bleibt, würde die Anwendung weiterhin ordnungsgemäß. Hier ist die abgeschlossene *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Als Nächstes müssen Sie ändern `InstructorID` auf `PersonID` und `StudentID` auf `PersonID` während des gesamten Projekts ***außer*** in den Dateien Zeitstempel Migrationen in der *Migrationen* Ordner. Dazu suchen und öffnen Sie nur die Dateien, die geändert werden müssen, dann führen Sie eine globale Änderung auf die geöffneten Dateien. Die einzige Datei in die *Migrationen* ist der Ordner, die Sie ändern, sollten *Migrations\Configuration.cs.*

1. > [!IMPORTANT]
   > Schließen alle geöffneten Dateien in Visual Studio, um zu beginnen.
2. Klicken Sie auf **suchen und Ersetzen – Suchen nach allen Dateien** in der **bearbeiten** Menü, und suchen Sie anschließend für alle Dateien im Projekt mit `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Öffnen Sie jede Datei in die **Suchergebnisse** Fenster ***außer*** der &lt;Zeitstempel&gt;*\_cs* Migrationsdateien in die *Migrationen* Ordner durch Doppelklicken auf eine Zeile für jede Datei.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Öffnen der **in Dateien ersetzen** Dialogfeld und Änderung **Suchen in** auf **alle geöffneten Dokumente**.
5. Verwenden der **in Dateien ersetzen** Dialogfeld so ändern Sie alle `InstructorID` an `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Suchen Sie alle Dateien in das Projekt, das enthalten `StudentID`.
7. Öffnen Sie jede Datei in die **Suchergebnisse** Fenster ***außer*** der &lt;Zeitstempel&gt;*\_\*cs* Migrationsdateien in der *Migrationen* Ordner durch Doppelklicken auf eine Zeile für jede Datei.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Öffnen der **in Dateien ersetzen** Dialogfeld und Änderung **Suchen in** auf **alle geöffneten Dokumente**.
9. Verwenden der **in Dateien ersetzen** Dialogfeld so ändern Sie alle `StudentID` auf `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Erstellen Sie das Projekt.

(Beachten Sie, die dies veranschaulicht eine *Nachteil* von der `classnameID` Muster für die Benennung von Primärschlüsseln. Wenn Sie die Primärschlüssel-ID genannt hätten, ohne den Klassennamen voranstellen *keine* umbenennen erweist sich als notwendig jetzt.)

## <a name="create-and-update-a-migrations-file"></a>Erstellen Sie und aktualisieren Sie eine Datei Migrationen

Geben Sie in der Paket-Manager-Konsole (PMC) den folgenden Befehl ein:

`Add-Migration Inheritance`

Führen Sie die `Update-Database` Befehl in der Systemmonitor. An diesem Punkt schlägt der Befehl fehl, da wir die vorhandene Daten, die Migrationen weiß nicht, wie behandelt haben. Sie erhalten die folgende Fehlermeldung:

*Die ALTER TABLE-Anweisung steht in Konflikt mit der FOREIGN KEY-Einschränkung "FK\_Dbo. Abteilung\_Dbo. Person\_PersonID ". Der Konflikt trat in der Datenbank "ContosoUniversity", Tabelle "Dbo. Person", Spalte"PersonID".*

Open *Migrationen\&Lt; Zeitstempel&gt;\_Inheritance.cs* , und Ersetzen Sie die `Up` -Methode durch folgenden Code:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Führen Sie die `update-database` erneut aus.

> [!NOTE]
> Es ist möglich, andere Fehlermeldungen erhalten, bei der Migration von Daten und festlegen, dass schemaänderungen. Wenn Sie Fehler bei der Migration erhalten Sie kann nicht aufgelöst, das Lernprogramm fort, indem Sie ändern die Verbindungszeichenfolge in der *"Web.config"* Datei oder Datenbank wird gelöscht. Der einfachste Ansatz besteht darin, benennen Sie die Datenbank in der *"Web.config"* Datei. Ändern Sie z. B. den Datenbanknamen in CU\_testen, wie im folgenden Beispiel gezeigt:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> Mit einer neuen Datenbank, es sind keine Daten zu migrieren, und die `update-database` Befehl ist weitaus höheren Wahrscheinlichkeit in ohne Fehler abgeschlossen werden. Informationen zum Löschen der Datenbank finden Sie unter [Gewusst wie: Löschen einer Datenbank aus Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Wenn Sie diesen Ansatz verwenden, um dieses Lernprogramm fortsetzen, überspringen Sie den Bereitstellungsschritt am Ende dieses Lernprogramms, da die bereitgestellte Website den gleichen Fehler erhalten würden, wenn sie Migrationen automatisch ausgeführt wird. Wenn Sie einen Migrationen Fehler beheben möchten, ist die beste Ressource eine der Entity Framework-Foren oder StackOverflow.com.


## <a name="testing"></a>Test

Führen Sie den Standort aus, und wiederholen Sie den verschiedenen Seiten. Alles funktioniert genauso wie vorher.

In **Server-Explorer** erweitern **SchoolContext** und dann **Tabellen**, und Sie sehen, dass die **Student** und **Dozenten**  Tabellen wurden durch ersetzt eine **Person** Tabelle. Erweitern Sie die **Person** Tabelle, und wird angezeigt, dass sie alle Spalten, die verwendet besitzt werden die **Student** und **Dozenten** Tabellen.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Klicken Sie mit der rechten Maustaste auf die Tabelle „Person“, und klicken Sie anschließend auf **Tabellendaten anzeigen**, um die Unterscheidungsspalte anzuzeigen.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Das folgende Diagramm veranschaulicht die Struktur der neuen Datenbank "School":

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Zusammenfassung

Tabelle pro Hierarchie Vererbung wurde jetzt für implementiert die `Person`, `Student`, und `Instructor` Klassen. Weitere Informationen zu diesen und anderen Vererbung Strukturen finden Sie unter [Vererbung Zuordnung Strategien](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) Morteza Manavi Blog. In den nächsten Lernprogrammen sehen Sie einige Möglichkeiten, um das Repository und die Einheit der Arbeit Muster zu implementieren.

Links zu anderen Entity Framework-Ressourcen finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Weiter](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
