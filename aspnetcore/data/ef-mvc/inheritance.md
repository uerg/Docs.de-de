---
title: ASP.NET Core MVC mit EF-Kern - Vererbung - 9 von 10
author: tdykstra
description: In diesem Lernprogramm erfahren Sie, wie Vererbung in das Datenmodell mithilfe von Entity Framework Core in einer ASP.NET Core-Anwendung implementiert.
keywords: ASP.NET Core, Entity Framework Core-Vererbung
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 41dc0db7-6f17-453e-aba6-633430609c74
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 10bde121dac3bdbbf0e55f2d146d91dea0f0210f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a>Vererbung - EF-Core mit ASP.NET Core MVC-Lernprogramm (9 von 10)

Durch [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen, die mit Entity Framework Core und Visual Studio. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](intro.md).

Im vorherigen Lernprogramm behandelt Sie Parallelitätsausnahmen. In diesem Lernprogramm erfahren Sie, wie Vererbung im Datenmodell implementiert.

In einer objektorientierten Programmierung können Sie Vererbung verwenden, um die Wiederverwendung von Code zu vereinfachen. In diesem Lernprogramm ändern Sie die `Instructor` und `Student` Klassen, damit sie ableiten eine `Person` Basisklasse, die Eigenschaften, z. B. enthält `LastName` , könne Dozenten und Studenten gemeinsam sind. Sie wird nicht hinzufügen oder Ändern von Webseiten, aber ändern Sie Teil des Codes und diese Änderungen werden automatisch in der Datenbank übernommen.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Optionen für die Zuordnung von Vererbung zu Datenbanktabellen

Die `Instructor` und `Student` Klassen im Modell "School" Daten wurden mehrere Eigenschaften, die identisch sind:

![Student "und" Instructor-Klassen](inheritance/_static/no-inheritance.png)

Angenommen, Sie möchten, für die Eigenschaften der redundanten Code vermeiden, die von gemeinsam genutzt werden die `Instructor` und `Student` Entitäten. Oder Sie möchten einen Dienst schreiben, der Formatnamen können ohne eine Rolle spielt, ob der Name einer Dozenten oder eines Teilnehmers stammt. Sie erstellen eine `Person` Basisklasse, die nur diejenigen Eigenschaften gemeinsame enthält, muss sich die `Instructor` und `Student` Klassen erben von dieser Basisklasse auf, wie in der folgenden Abbildung gezeigt:

![Student "und" Instructor-Klassen ableiten von Person-Klasse](inheritance/_static/inheritance.png)

Es gibt mehrere Möglichkeiten, die diese Vererbungsstruktur in der Datenbank dargestellt werden konnte. Sie möglicherweise eine Person-Tabelle, die Informationen zu Studenten und Lehrkräfte in einer einzelnen Tabelle enthält. Einige Spalten könnte gelten nur für Lehrkräfte (Einstellungsdatum), einige nur für Studenten (EnrollmentDate), einige beide (Nachname, Vorname). Normalerweise müssten Sie, um anzugeben, welcher Typ jede Zeile stellt eine Unterscheidungsspalte. Z. B. möglicherweise Unterscheidungsspalte für Studenten "Instructor" für Dozenten und "Student" haben.

![Beispiel für eine Tabelle pro Hierarchie](inheritance/_static/tph.png)

Dieses Muster für eine Entität Vererbungsstruktur aus einer einzelnen Datenbanktabelle generiert wird Tabelle pro Hierarchie (TPH) Vererbung bezeichnet.

Eine Alternative besteht darin, von der Datenbank eher wie die Vererbungsstruktur. Sie konnten z. B. haben nur die Felder in den Person-Tabelle und separate Dozenten und Student-Tabellen mit den Datumsfeldern aufweisen.

!["Tabelle pro Typ"-Vererbung](inheritance/_static/tpt.png)

Dieses Muster versteht man eine Datenbanktabelle für jede Entitätsklasse heißt Tabelle pro Typ (TPT) Vererbung.

Ist noch eine weitere Option, um einzelne Tabellen alle nicht abstrakten Typen zuzuordnen. Alle Eigenschaften einer Klasse, einschließlich der geerbte Eigenschaften sind Spalten der entsprechenden Tabelle zugeordnet. Dieses Muster wird Tabelle pro konkrete klassenvererbung (TPC) bezeichnet. Wenn Sie vorher gezeigten TPC-Vererbung für die Person, Studenten und Dozenten Klassen implementiert, sieht Student "und" Instructor-Tabellen unterscheidet sich nach dem Implementieren der Vererbung als vor der funktionsfähig.

TPC und TPH Muster der methodenvererbung aufgeführt übermitteln im Allgemeinen eine bessere Leistung als TPT Muster der methodenvererbung aufgeführt, da TPT Muster komplexer Join-Abfragen führen können.

Dieses Lernprogramm veranschaulicht die TPH-Vererbung zu implementieren. TPH ist die einzige Vererbungsmuster, die den Kern des Entity Framework unterstützt.  Was Sie tun müssen ist, erstellen Sie eine `Person` Klasse, Ändern der `Instructor` und `Student` Klassen ableiten `Person`, fügen Sie die neue Klasse, die `DbContext`, und erstellen Sie eine Migration.

> [!TIP] 
> Sollten Sie eine Kopie des Projekts speichern, bevor Sie die folgenden Änderungen vornehmen.  Klicken Sie dann werden Probleme und müssen über starten auftreten, einfacher aus dem gespeicherten Projekt anstelle von "Fertig" für dieses Lernprogramm Schritte umkehren oder wechseln zurück zum Anfang die gesamte Reihe gestartet.

## <a name="create-the-person-class"></a>Erstellen Sie die Person-Klasse

Klicken Sie im Ordner Models erstellen Sie Person.cs, und Ersetzen Sie den Vorlagencode durch den folgenden Code:

[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Stellen Sie Student "und" Instructor-Klassen, die von der Person erben

In *Instructor.cs*, leiten Sie die Instructor-Klasse, von der Person-Klasse, und entfernen Sie die Schlüssel und den Namen der Felder. Der Code wird wie im folgenden Beispiel aussehen:

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

Nehmen Sie die gleichen Änderungen in *Student.cs*.

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a>Fügen Sie des Person-Entität vom Typs des Datenmodells

Fügen Sie den Person-Entität auf *SchoolContext.cs*. Die neuen Zeilen werden hervorgehoben.

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

Dies ist das Entity Framework muss lediglich um eine Tabelle pro Hierarchie Vererbung konfigurieren. Wie Sie sehen werden, wenn die Datenbank aktualisiert wird, müssen sie eine Personentabelle anstelle der Student "und" Instructor-Tabellen.

## <a name="create-and-customize-migration-code"></a>Erstellen und Anpassen von Code der migration

Speichern Sie die Änderungen zu, und erstellen Sie das Projekt. Klicken Sie dann öffnen Sie das Befehlsfenster zu, in den Projektordner, und geben Sie den folgenden Befehl aus:

```console
dotnet ef migrations add Inheritance
```

Führen Sie nicht die `database update` noch Befehl. Dieser Befehl führt zu Datenverlust, da die Instructor-Tabelle löschen und benennen Sie die Student-Tabelle, die Person. Sie müssen zum Bereitstellen von benutzerdefiniertem Code, um vorhandene Daten nicht beibehalten.

Open *Migrationen /\<Zeitstempel > _Inheritance.cs* , und Ersetzen Sie die `Up` -Methode durch folgenden Code:

[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

Dieser Code übernimmt die folgenden Aufgaben der Datenbank aktualisieren:

* Entfernt foreign Key-Einschränkungen und Indizes, die auf die Student-Tabelle verweisen.

* Benennt die Instructor-Tabelle als Person und nimmt Änderungen, die zum Speichern von Daten Student erforderlich:

* Fügt NULL-Werte zulassen EnrollmentDate für Studenten hinzu.

* Fügt Unterscheidungsspalte, um anzugeben, ob eine Zeile für ein Student oder einen Kursleiter bestimmt ist.

* Macht HireDate seit Student Zeilen Einstellungsdaten keine NULL-Werte zulässt.

* Fügt ein temporäres Feld, das zum Aktualisieren von Fremdschlüsseln, die auf Studenten verweisen verwendet werden. Beim Kopieren von Studenten in die Person-Tabelle erhalten sie neue primäre Schlüsselwerte.

* Kopiert Daten aus der Tabelle "Student" in der Person-Tabelle. Dies bewirkt, dass Studenten neue Primärschlüsselwerte zugewiesen.

* Behebt Fremdschlüsselwerte, die auf Studenten verweisen.

* Neu erstellt, foreign Key-Einschränkungen und Indizes, die Sie sie jetzt auf der Person-Tabelle verweisen.

(Wenn Sie GUID nicht ganze Zahl als den Typ des primären Schlüssels verwendet haben, Student Primärschlüsselwerte würde nicht ändern müssen und mehrere Schritte wurde konnte ausgelassen.)

Führen Sie die `database update` Befehl:

```console
dotnet ef database update
```

(In einem Produktionssystem Sie entsprechende Änderungen vornehmen würden die `Down` Methode im Fall schon zurückdatieren, um die frühere Datenbankversion verwendet. In diesem Lernprogramm nicht benötigte die `Down` Methode.)

> [!NOTE] 
> Es ist möglich, andere Fehler auftreten, wenn schemaänderungen in einer Datenbank zu bestimmen, die vorhandene Daten enthält. Wenn Sie Fehler bei der Migration, die Sie nicht beheben können erhalten, können Sie den Datenbanknamen in der Verbindungszeichenfolge ändern oder löschen Sie die Datenbank. Mit einer neuen Datenbank es sind keine Daten zu migrieren, und der Update-Database-Befehl ist wahrscheinlicher ohne Fehler abgeschlossen. Klicken Sie zum Löschen der Datenbank SSOX verwenden, oder führen Sie die `database drop` CLI-Befehl.

## <a name="test-with-inheritance-implemented"></a>Testen mit Vererbung implementiert

Führen Sie die app, und wiederholen Sie den verschiedenen Seiten. Alles funktioniert genauso wie zuvor.

In **Objekt-Explorer von SQL Server**, erweitern Sie **Daten Verbindungen/SchoolContext** und dann **Tabellen**, und Sie sehen, dass die Tabellen Student "und" Dozenten durch ersetzt wurden eine Person-Tabelle. Die Person-Tabellen-Designer öffnen und sehen Sie, dass sie alle Spalten hat, die in den Tabellen Student "und" Dozenten werden verwendet.

![Person-Tabelle in SSOX](inheritance/_static/ssox-person-table.png)

Mit der rechten Maustaste in den Person-Tabelle, und klicken Sie dann auf **Tabellendaten anzeigen** Unterscheidungsspalte angezeigt.

![Person-Tabelle in SSOX - Tabellendaten](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a>Zusammenfassung

Sie haben die Tabelle pro Hierarchie Vererbung für implementiert die `Person`, `Student`, und `Instructor` Klassen. Weitere Informationen zu Vererbung in Entity Framework Core, finden Sie unter [Vererbung](https://docs.microsoft.com/ef/core/modeling/inheritance). In den nächsten Lernprogrammen sehen Sie, wie eine Vielzahl von relativ erweiterte Entity Framework-Szenarien behandelt.

>[!div class="step-by-step"]
[Zurück](concurrency.md)
[Weiter](advanced.md)  
