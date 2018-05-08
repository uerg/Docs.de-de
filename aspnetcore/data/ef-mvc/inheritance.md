---
title: 'ASP.NET Core MVC mit EF Core: Vererbung (9 von 10)'
author: tdykstra
description: In diesem Tutorial erfahren Sie, wie Sie die Vererbung mithilfe von Entity Framework Core in einer ASP.NET Core-App implementieren.
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 25d4292e325e208ee08f4a7bb8d06580809f9e40
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-core-mvc-with-ef-core---inheritance---9-of-10"></a>ASP.NET Core MVC mit EF Core: Vererbung (9 von 10)

Von [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen mit Entity Framework Core und Visual Studio erstellt werden. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](intro.md).

Im vorherigen Tutorial haben Sie Parallelitätsausnahmen behandelt. In diesem Tutorial erfahren Sie, wie Sie die Vererbung in das Datenmodell implementieren können.

Bei objektorientierter Programmierung können Sie mithilfe von Vererbung die Wiederverwendung von Code vereinfachen. In diesem Tutorial ändern Sie die Klassen `Instructor` und `Student` so, dass sie von einer `Person`-Basisklasse abgeleitet werden, die Eigenschaften wie `LastName` enthält. Diese Eigenschaften sind für Dozenten und Studenten gängig. Sie fügen keine Webseiten hinzu oder ändern diese, aber Sie werden Teile des Codes ändern. Diese Änderungen werden automatisch in der Datenbank widergespiegelt.

## <a name="options-for-mapping-inheritance-to-database-tables"></a>Optionen für die Zuordnung von Vererbung zu Datenbanktabellen

Die Klassen `Instructor` und `Student` im Datenmodell „Schule“ weisen mehrere identische Eigenschaften auf:

![Die Klassen „Student“ und „Instructor“](inheritance/_static/no-inheritance.png)

Angenommen, Sie möchten den redundanten Code für die Eigenschaften löschen, die von den Entitäten `Instructor` und `Student` gemeinsam genutzt werden. Oder Sie möchten einen Dienst schreiben, mit dem Namen formatiert werden können, ohne dass es eine Rolle spielt, ob der Name von einem Dozenten oder von einem Studenten stammt. Dann können Sie eine `Person`-Basisklasse erstellen, die nur diese gemeinsam genutzten Eigenschaften enthält. Anschließend können Sie einstellen, dass die Klassen `Instructor` und `Student` von dieser Basisklasse erben sollen, wie in der folgenden Abbildung dargestellt wird:

![Die Klassen „Student“ und „Instructor“ abgeleitet von der Klasse „Person“](inheritance/_static/inheritance.png)

Es gibt mehrere Möglichkeiten, wie diese Vererbungsstruktur in der Datenbank dargestellt werden kann. Sie können z.B. über die Tabelle „Person“ verfügen, die Informationen zu Studenten und Dozenten in einer einzigen Tabelle enthält. Einige Spalten können dann nur für Dozenten gelten (HireDate), einige nur für Studenten (EnrollmentDate) und einige für beide (LastName, FirstName). Normalerweise sollten Sie über eine Unterscheidungsspalte verfügen, in der angegeben wird, welcher Typ in den jeweiligen Zeilen dargestellt wird. So kann die Unterscheidungsspalte beispielsweise „Instructor“ für Dozenten und „Student“ für Studenten enthalten.

![Beispiel: Tabelle pro Hierarchie](inheritance/_static/tph.png)

Dieses Muster, bei dem aus einer einzigen Datenbanktabelle eine Vererbungsstruktur für Entitäten generiert wird, wird als TPH-Vererbung (TPH = Table per Hierarchy, Tabelle pro Hierarchie) bezeichnet.

Alternativ kann die Datenbank so gestaltet werden, dass sie mehr wie die Vererbungsstruktur aussieht. Die Tabelle „Person“ könnte beispielsweise nur die Namensfelder aufweisen und über separate Tabellen mit den Namen „Instructor“ und „Student“ verfügen, in denen die Datumsfelder enthalten sind.

!["Tabelle pro Typ"-Vererbung](inheritance/_static/tpt.png)

Dieses Muster, bei dem für jede Entitätsklasse eine Datenbanktabelle erstellt wird, wird als TPT-Vererbung (TPT = Table per Type, Tabelle pro Typ) bezeichnet.

Eine weitere Möglichkeit besteht darin, individuellen Tabellen alle nicht abstrakten Typen zuzuordnen. Alle Eigenschaften einer Klasse, einschließlich der geerbten Eigenschaften, werden Spalten der entsprechenden Tabelle zugeordnet. Dieses Muster wird als TPC-Vererbung (TPC = Table per Concrete, Tabelle pro konkretem Typ) bezeichnet. Wenn Sie die TPC-Vererbung für die Klassen „Person“, „Student“ und „Instructor“ wie oben beschrieben implementieren, würden die Tabellen „Student“ und „Instructor“ nach der Implementierung unverändert aussehen.

Bei den TPC- und TPH-Vererbungsmustern wird in der Regel eine bessere Leistung erzielt als bei den TPT-Vererbungsmustern, da TPT-Muster zu komplexen Joinabfrage führen können.

Dieses Tutorial veranschaulicht die Implementierung der TPH-Vererbung. TPH ist das einzige Vererbungsmuster, das von Entity Framework Core unterstützt wird.  Dabei erstellen Sie eine `Person`-Klasse, ändern die Klassen `Instructor` und `Student`, die von `Person` abgeleitet werden sollen, fügen die neue Klasse zum `DbContext` hinzu und erstellen eine Migration.

> [!TIP] 
> Sie sollten eine Kopie des Projekts speichern, bevor Sie folgende Änderungen vornehmen.  Wenn Probleme auftreten und Sie von vorne beginnen müssen, ist es leichter, vom gespeicherten Projekt aus zu starten, statt für dieses Tutorial ausgeführte Schritte rückgängig zu machen oder zum Anfang der Reihe zurückkehren zu müssen.

## <a name="create-the-person-class"></a>Erstellen der Klasse „Person“

Erstellen Sie im Ordner „Models“ (Modelle) die Datei „Person.cs“ und ersetzen Sie den Vorlagencode durch folgenden Code:

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Veranlassen der Vererbung von der Klasse „Person“ an die Klassen „Student“ und „Instructor“

Leiten Sie in der Datei *Instructor.cs* die Klasse „Instructor“ von der Klasse „Person“ ab, und entfernen Sie Schlüssel- und Namensfelder. Der Code sieht aus wie im folgenden Beispiel:

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

Nehmen Sie an der Datei *Student.cs* die gleichen Änderungen vor.

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a>Hinzufügen des Entitätstyps „Person“ zum Datenmodell

Fügen Sie den Entitätstyp „Person“ zur Datei *SchoolContext.cs* hinzu. Die neuen Zeilen werden hervorgehoben.

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

Das ist alles, was Entity Framework für die Konfiguration der „Tabelle pro Hierarchie“-Vererbung benötigt. Sie werden feststellen, dass die Datenbank nach ihrer Aktualisierung statt der Tabellen „Student“ und „Instructor“ eine Person-Tabelle enthält.

## <a name="create-and-customize-migration-code"></a>Erstellen und Anpassen des Migrationscodes

Speichern Sie Ihre Änderungen, und erstellen Sie das Projekt. Öffnen Sie anschließend das Befehlsfenster im Projektordner, und geben Sie folgenden Befehl ein:

```console
dotnet ef migrations add Inheritance
```

Führen Sie noch nicht den Befehl `database update` aus. Die Ausführung dieses Befehls führt zu einem Datenverlust, da er die Tabelle „Instructor“ löscht und die Tabelle „Student“ in „Person“ umbenennt. Sie müssen benutzerdefinierten Code angeben, damit vorhandene Daten erhalten bleiben.

Öffnen Sie *Migrations/\<timestamp>_Inheritance.cs*, und ersetzen Sie die Methode `Up` durch den folgenden Code:

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

Dieser Code übernimmt folgende Tasks für Datenbankaktualisierungen:

* Er entfernt Fremdschlüsseleinschränkungen und -indizes, die auf die Tabelle „Student“ verweisen.

* Er benennt die Tabelle „Instructor“ in „Person“ um und nimmt die Änderungen vor, die für das Speichern von Studentendaten erforderlich sind:

* Er fügt das EnrollmentDate für Studenten hinzu, bei dem NULL-Werte zugelassen sind.

* Er fügt eine Unterscheidungsspalte hinzu, um anzugeben, ob eine Zeile für einen Studenten oder für einen Dozenten bestimmt ist.

* Er legt fest, dass bei HireDate NULL-Werte zugelassen sind, da die Zeilen für Studenten keine Einstellungsdaten enthalten.

* Er fügt ein temporäres Feld hinzu, über das Fremdschlüssel aktualisiert werden sollen, die auf Studenten verweisen. Wenn Sie Studenten in die Tabelle „Person“ kopieren, erhalten diese neue Primärschlüsselwerte.

* Kopiert Daten aus der Tabelle „Student“ in die Tabelle „Person“. Dadurch werden Studenten neue Primärschlüsselwerte zugewiesen.

* Er legt Fremdschlüsselwerte fest, die auf Studenten verweisen.

* Er erstellt Fremdschlüsseleinschränkungen und -indizes neu, die dann auf die Tabelle „Person“ verweisen.

(Hätten Sie als Primärschlüsseltyp statt einem Integer die grafische Benutzeroberfläche verwendet haben, hätten die Primärschlüsselwerte für Studenten nicht geändert werden müssen, und mehrere dieser Schritte hätten ausgelassen werden können.)

Führen Sie den Befehl `database update` aus:

```console
dotnet ef database update
```

(In einem Produktionssystem würden Sie entsprechende Änderungen an der Methode `Down` vornehmen, falls Sie diese jemals verwenden müssten, um zur vorherigen Datenbankversion zurückzukehren. In diesem Tutorial wird die Methode `Down` nicht verwendet.)

> [!NOTE] 
> Es ist möglich, dass andere Fehler auftreten, wenn Schemaänderungen in einer Datenbank durchgeführt werden, die vorhandene Daten enthält. Wenn Migrationsfehler auftreten, die Sie nicht beheben können, können Sie entweder den Datenbanknamen in der Verbindungszeichenfolge ändern oder die Datenbank löschen. In einer neuen Datenbank gibt es keine zu migrierenden Daten, und der Befehl „update-database“ wird wahrscheinlich ohne Fehler ausgeführt. Verwenden Sie zum Löschen der Datenbank SSOX, oder führen Sie den CLI-Befehl `database drop` aus.

## <a name="test-with-inheritance-implemented"></a>Testen mit implementierter Vererbung

Führen Sie die App aus, und testen Sie verschiedene Seiten. Alles funktioniert genauso wie vorher.

Wenn Sie im **SQL Server-Objekt-Explorer** **Data Connections/SchoolContext** und anschließend **Tabellen** erweitern, können Sie sehen, dass die Tabellen „Student“ und „Instructor“ durch eine Person-Tabelle ersetzt wurden. Wenn Sie den Person-Tabellen-Designer öffnen, sehen Sie, dass er alle Spalten aus den Tabellen „Student“ und „Instructor“ enthält.

![Person-Tabelle im SSOX](inheritance/_static/ssox-person-table.png)

Klicken Sie mit der rechten Maustaste auf die Tabelle „Person“, und klicken Sie anschließend auf **Tabellendaten anzeigen**, um die Unterscheidungsspalte anzuzeigen.

![Person-Tabelle im SSOX: Tabellendaten](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a>Zusammenfassung

Sie haben die „Tabelle pro Hierarchie“-Vererbung für die Klassen `Person`, `Student` und `Instructor` implementiert. Weitere Informationen zur Vererbung in Entity Framework Core finden Sie unter [Vererbung](https://docs.microsoft.com/ef/core/modeling/inheritance). Im nächsten Tutorial erfahren Sie, wie Sie eine Vielzahl von Entity Framework-Szenarios auf fortgeschrittenem Niveau verarbeiten können.

> [!div class="step-by-step"]
> [Zurück](concurrency.md)
> [Weiter](advanced.md)  
