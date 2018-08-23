---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
title: Implementieren der Vererbung mit dem Entitätsframework in einer ASP.NET MVC-Anwendung (8 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: a5c3feff-5335-4cdd-a97d-f7a8785c2494
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 10ee0be62a1d601e323afc423e9022bed56f4f33
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832742"
---
<a name="implementing-inheritance-with-the-entity-framework-in-an-aspnet-mvc-application-8-of-10"></a>Implementieren der Vererbung mit dem Entitätsframework in einer ASP.NET MVC-Anwendung (8 von 10)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio 2012. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem Sie nicht lösen stoßen, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie es zum Reproduzieren des Problems an. Die Lösung des Problems finden in der Regel durch Ihren Code, den vollständigen Code vergleichen. Einige häufige Fehler und zu deren Lösung finden Sie [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


Im vorherigen Tutorial haben Sie Parallelitätsausnahmen behandelt. In diesem Tutorial erfahren Sie, wie Sie die Vererbung in das Datenmodell implementieren können.

Vererbung können Sie in der objektorientierten Programmierung, um redundanten Code zu vermeiden. In diesem Tutorial ändern Sie die Klassen `Instructor` und `Student` so, dass sie von einer `Person`-Basisklasse abgeleitet werden, die Eigenschaften wie `LastName` enthält. Diese Eigenschaften sind für Dozenten und Studenten gängig. Sie fügen keine Webseiten hinzu oder ändern diese, aber Sie werden Teile des Codes ändern. Diese Änderungen werden automatisch in der Datenbank widergespiegelt.

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>Tabelle pro Hierarchie im Vergleich zu Tabelle-pro-Typ '-Vererbung

In der objektorientierten Programmierung können Sie die Vererbung verwenden, zum Arbeiten mit verknüpften Klassen vereinfachen. Z. B. die `Instructor` und `Student` Klassen in der `School` Datenmodell freigeben mehrere Eigenschaften, was dazu führt, redundanter Code:

![Student_and_Instructor_classes](https://asp.net/media/2578113/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_e7a32f99-8bc4-48ce-aeaf-216a18071a8b.png)

Angenommen, Sie möchten den redundanten Code für die Eigenschaften löschen, die von den Entitäten `Instructor` und `Student` gemeinsam genutzt werden. Sie erstellen eine `Person` Basisklasse, die nur diese gemeinsam Eigenschaften genutzten enthält, muss sich die `Instructor` und `Student` Entitäten aus dieser Basisklasse erben, wie in der folgenden Abbildung gezeigt:

![Student_and_Instructor_classes_deriving_from_Person_class](https://asp.net/media/2578119/Windows-Live-Writer_58f5a93579b2_CC7B_Student_and_Instructor_classes_deriving_from_Person_class_671d708c-cbb8-454a-a8f8-c2d99439acd9.png)

Es gibt mehrere Möglichkeiten, wie diese Vererbungsstruktur in der Datenbank dargestellt werden kann. Sie verfügen über eine `Person` Tabelle Informationen über Schüler/Studenten und Dozenten in einer einzelnen Tabelle enthält. Einige Spalten können nur für Dozenten anwenden (`HireDate`), einige nur für Studenten (`EnrollmentDate`), werden einige auf beide (`LastName`, `FirstName`). In der Regel müssen eine *Diskriminator* Spalte, um anzugeben, welchen Typ von jeder Zeile darstellt. So kann die Unterscheidungsspalte beispielsweise „Instructor“ für Dozenten und „Student“ für Studenten enthalten.

![Tabelle-pro-hierarchy_example](https://asp.net/media/2578125/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-hierarchy_example_244067cd-b451-4e9b-9595-793b9afca505.png)

Dieses Muster zum Generieren von eine Vererbungsstruktur für Entitäten aus einer einzelnen Datenbanktabelle heißt *Tabelle pro Hierarchie* (TPH)-Vererbung.

Alternativ kann die Datenbank so gestaltet werden, dass sie mehr wie die Vererbungsstruktur aussieht. Angenommen, Sie verfügen über nur die Felder der `Person` Tabelle, und über separate `Instructor` und `Student` Tabellen mit den Feldern.

![Tabelle-pro-type_inheritance](https://asp.net/media/2578131/Windows-Live-Writer_58f5a93579b2_CC7B_Table-per-type_inheritance.png)

Dieses Muster eine Datenbanktabelle zu machen, für jede Entitätsklasse aufgerufen *Tabelle pro Typ* (TPT)-Vererbung.

TPH-vererbungsmustern in der Regel eine bessere Leistung im Entity Framework als TPT-vererbungsmustern, da TPT-Muster zu komplexen joinabfrage führen können. Dieses Tutorial veranschaulicht die Implementierung der TPH-Vererbung. Sie müssen dazu die folgenden Schritte ausführen:

- Erstellen Sie eine `Person` Klasse, und Ändern der `Instructor` und `Student` Klassen für die Ableitung `Person`.
- Die datenbankkontextklasse Zuordnungscode und Modell der Datenbank hinzugefügt haben.
- Änderung `InstructorID` und `StudentID` Verweise im Verlauf des Projekts `PersonID`.

## <a name="creating-the-person-class"></a>Erstellen die Person-Klasse

 Hinweis: Nicht möglich, kompilieren Sie das Projekt nach dem Erstellen der folgenden Klassen, bis Sie den Controller, die diese Klassen verwendet, aktualisieren. 

In der *Modelle* Ordner erstellen *Person.cs* , und Ersetzen Sie den Vorlagencode durch den folgenden Code:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

In *Instructor.cs*, leiten die `Instructor` -Klasse aus der `Person` Klasse, und entfernen Sie die Schlüssel- und Namensfelder. Der Code sieht aus wie im folgenden Beispiel:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Nehmen Sie ähnliche Änderungen an *Student.cs*. Die `Student` Klasse wird wie im folgenden Beispiel aussehen:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

## <a name="adding-the-person-entity-type-to-the-model"></a>Hinzufügen des Entitätstyps Person zum Modell

In *SchoolContext.cs*, Hinzufügen einer `DbSet` -Eigenschaft für die `Person` Entitätstyp:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Das ist alles, was Entity Framework für die Konfiguration der „Tabelle pro Hierarchie“-Vererbung benötigt. Wie Sie sehen werden, wenn die Datenbank neu erstellt ist, muss ein `Person` anstelle der Tabelle die `Student` und `Instructor` Tabellen.

## <a name="changing-instructorid-and-studentid-to-personid"></a>InstructorID und "StudentID" in PersonID ändern

In *SchoolContext.cs*, ändern Sie in der Anweisung "Instructor"-Kurs Zuordnung `MapRightKey("InstructorID")` zu `MapRightKey("PersonID")`:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=4)]

Diese Änderung ist nicht erforderlich; Es ändert nur den Namen der Spalte "InstructorID" in der m: n Jointabelle. Wenn Sie den Namen als InstructorID gelassen wird, würde die Anwendung weiterhin ordnungsgemäß. Hier ist die abgeschlossene *SchoolContext.cs*:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=15,24)]

Als Nächstes müssen Sie so ändern Sie `InstructorID` zu `PersonID` und `StudentID` zu `PersonID` im gesamten Projekt ***außer*** in den Migrationsdateien mit einem Zeitstempel in der *Migrationen* Ordner. Dazu müssen Sie finden und nur die Dateien, die geändert werden müssen, öffnen, und führen Sie eine globale Änderung auf die geöffneten Dateien. Die einzige Datei in die *Migrationen* Ordner Sie ändern *migrations\configuration. cs.*

1. > [!IMPORTANT]
   > Schließen Sie alle geöffneten Dateien in Visual Studio beginnen.
2. Klicken Sie auf **suchen und Ersetzen – Suchen nach allen Dateien** in die **bearbeiten** Menü, und suchen Sie für alle Dateien im Projekt, enthalten `InstructorID`.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. Öffnen Sie jede Datei in die **Suchergebnisse** Fenster ***außer*** der &lt;Zeitstempel&gt;*\_cs* Migrationsdateien in die *Migrationen* Ordner durch Doppelklicken auf eine Zeile für jede Datei.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)
4. Öffnen der **in Dateien ersetzen** Dialogfeld und ändern Sie **Suchen in** zu **alle geöffneten Dokumente**.
5. Verwenden der **in Dateien ersetzen** Dialogfeld so ändern Sie alle `InstructorID` auf `PersonID.`  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)
6. Suchen Sie alle Dateien in das Projekt, das enthalten `StudentID`.
7. Öffnen Sie jede Datei in die **Suchergebnisse** Fenster ***außer*** der &lt;Zeitstempel&gt;*\_\*cs* Migrationsdateien in der *Migrationen* Ordner durch Doppelklicken auf eine Zeile für jede Datei.  
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
8. Öffnen der **in Dateien ersetzen** Dialogfeld und ändern Sie **Suchen in** zu **alle geöffneten Dokumente**.
9. Verwenden der **in Dateien ersetzen** Dialogfeld so ändern Sie alle `StudentID` zu `PersonID`.   
  
    ![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
10. Erstellen Sie das Projekt.

(Beachten Sie, die dies veranschaulicht eine *Nachteil* von der `classnameID` Muster für die Benennung von Primärschlüsseln. Wenn Sie die Primärschlüssel-ID genannt hätten, ohne den Klassennamen voranstellen *keine* umbenennen erweist sich als notwendig jetzt.)

## <a name="create-and-update-a-migrations-file"></a>Erstellen Sie und aktualisieren Sie eine Migrationsdatei

Geben Sie in der Paket-Manager-Konsole (PMC) den folgenden Befehl ein:

`Add-Migration Inheritance`

Führen Sie die `Update-Database` Befehl in der PMC. An diesem Punkt schlägt der Befehl fehl, da haben wir die vorhandene Daten, die Migrationen nicht, wie behandeln. Sie erhalten die folgende Fehlermeldung:

*Die ALTER TABLE-Anweisung steht in Konflikt mit der FOREIGN KEY-Einschränkung "FK\_Dbo. Abteilung\_Dbo. Person\_PersonID ". Der Konflikt trat in der Datenbank "ContosoUniversity", Tabelle "Dbo. Person", Spalte"PersonID".*

Open *Migrationen\&Lt; Zeitstempel&gt;\_Inheritance.cs* , und Ersetzen Sie die `Up` Methode durch den folgenden Code:

[!code-csharp[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=25,29-40)]

Führen Sie die `update-database` -Befehl erneut aus.

> [!NOTE]
> Es ist möglich, dass andere Fehler auftreten, bei der Migration von Daten und zu Änderungen am Datenbankschema. Wenn Sie Migrationsfehler erhalten Sie kann nicht aufgelöst werden, Sie können mit dem Tutorial fortfahren, durch Ändern der Verbindungszeichenfolge in der *"Web.config"* Datei oder das Löschen der Datenbank. Der einfachste Ansatz ist zum Umbenennen der Datenbank in der *"Web.config"* Datei. Ändern Sie z. B. der Name der Datenbank in CU\_testen, wie im folgenden Beispiel gezeigt:
> 
> [!code-xml[Main](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.xml?highlight=1-2)]
> 
> Mit einer neuen Datenbank, sind keine Daten zu migrieren, und die `update-database` Befehl ist sehr wahrscheinlich ohne Fehler abgeschlossen. Anweisungen dazu, wie Sie die Datenbank zu löschen, finden Sie unter [Gewusst wie: Löschen einer Datenbank in Visual Studio 2012](http://romiller.com/2013/05/17/how-to-drop-a-database-from-visual-studio-2012/). Wenn Sie diesen Ansatz verwenden, um dieses Lernprogramm fortsetzen, überspringen Sie den Bereitstellungsschritt am Ende dieses Tutorials, da die bereitgestellte Website der gleiche Fehler angezeigt wird, wenn sie Migrationen automatisch ausgeführt wird. Wenn Sie einen Migrationen Fehler beheben möchten, ist die beste Ressource ein Entity Framework-Foren oder StackOverflow.com.


## <a name="testing"></a>Test

Führen Sie die Website, und Testen Sie verschiedene Seiten. Alles funktioniert genauso wie vorher.

In **Server-Explorer** erweitern **SchoolContext** und dann **Tabellen**, und Sie sehen, dass die **für Schüler und Studenten** und **"Instructor"**  Tabellen wurden durch ersetzt eine **Person** Tabelle. Erweitern Sie die **Person** Tabelle, und Sie finden Sie, dass sie alle Spalten, die verwendet werden hat, in der **für Schüler und Studenten** und **"Instructor"** Tabellen.

![Server_Explorer_showing_Person_table](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Klicken Sie mit der rechten Maustaste auf die Tabelle „Person“, und klicken Sie anschließend auf **Tabellendaten anzeigen**, um die Unterscheidungsspalte anzuzeigen.

![](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Das folgende Diagramm veranschaulicht die Struktur der neuen Datenbank "School":

![School_database_diagram](https://asp.net/media/2578143/Windows-Live-Writer_58f5a93579b2_CC7B_School_database_diagram_6350a801-7199-413f-bbac-4a2009ed19d7.png)

## <a name="summary"></a>Zusammenfassung

Tabelle pro Hierarchie-Vererbung ist jetzt für implementiert die `Person`, `Student`, und `Instructor` Klassen. Weitere Informationen zu diesen und anderen Strukturen Vererbung finden Sie unter [Zuordnen von Vererbung-Strategien](https://weblogs.asp.net/manavi/archive/2010/12/24/inheritance-mapping-strategies-with-entity-framework-code-first-ctp5-part-1-table-per-hierarchy-tph.aspx) Morteza Manavi Blog. Im nächsten Tutorial sehen Sie einige Möglichkeiten, die Repository- und arbeitseinheitsmuster Muster zu implementieren.

Links zu anderen Ressourcen des Entity Framework finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Weiter](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
