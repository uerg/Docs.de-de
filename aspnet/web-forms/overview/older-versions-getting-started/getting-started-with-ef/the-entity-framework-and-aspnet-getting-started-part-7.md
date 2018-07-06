---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms – Teil 7 | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mithilfe von Entity Framework. Die beispielanwendung ist...
ms.author: aspnetcontent
ms.date: 12/03/2010
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: bb1abe52b913785f47b7f8ec9822ed9db5ee8a05
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823406"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Erste Schritte mit Entity Framework 4.0 Database First und ASP.NET 4 Web Forms – Teil 7
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Weitere Informationen zu dieser tutorialreihe finden Sie unter [im ersten Tutorial der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>Verwenden von gespeicherten Prozeduren

Im vorherigen Tutorial implementiert Sie eine Tabelle pro Hierarchie Vererbungsmuster. Dieses Tutorial zeigt Ihnen, wie Sie gespeicherte Prozeduren zu verwenden, um mehr Kontrolle über Zugriff auf die Datenbank zu erhalten.

Das Entity Framework können Sie angeben, dass gespeicherte Prozeduren für den Datenbankzugriff verwendet werden soll. Für jeden Entitätstyp können Sie eine gespeicherte Prozedur für die Verwendung für erstellen, aktualisieren oder Löschen von Entitäten des Typs angeben. Im Datenmodell können Sie dann Verweise auf gespeicherte Prozeduren, mit denen Sie Aufgaben wie das Abrufen von Gruppen von Entitäten hinzufügen.

Verwenden von gespeicherten Prozeduren ist eine häufige Anforderung für den Datenbankzugriff. In einigen Fällen kann ein Datenbankadministrator erfordert, dass gespeicherte Prozeduren aus Sicherheitsgründen jeglicher Datenbankzugriff durchlaufen. In anderen Fällen empfiehlt es sich um Geschäftslogik in einige der Prozesse zu erstellen, die das Entity Framework verwendet, wenn die Datenbank aktualisiert. Z. B. wenn eine Entität gelöscht wird empfiehlt in einer Datenbank "Archive" kopieren. Oder wenn eine Zeile aktualisiert wird möglicherweise einer Zeile in einer Protokollierungstabelle, in dem aufgezeichnet schreiben möchten, die Änderung vorgenommen hat. Sie können diese Arten von Aufgaben in einer gespeicherten Prozedur ausführen, die aufgerufen wird, wenn Entity Framework eine Entität löscht oder eine Entität aktualisiert.

Wie im vorherigen Tutorial erstellen Sie keine neuen Seiten. Stattdessen werden Sie ändern, wie das Entity Framework die Datenbank für einige Seiten greift auf Sie bereits erstellt.

In diesem Tutorial erstellen Sie gespeicherte Prozeduren in der Datenbank für das Einfügen von `Student` und `Instructor` Entitäten. Fügen Sie sie in das Datenmodell, und geben Sie, dass sie Entity Framework zum Hinzufügen von verwenden soll `Student` und `Instructor` Entitäten in der Datenbank. Sie erstellen auch eine gespeicherte Prozedur, die Sie, zum Abrufen verwenden können `Course` Entitäten.

## <a name="creating-stored-procedures-in-the-database"></a>Erstellen von gespeicherten Prozeduren in der Datenbank

(Bei Verwendung der *School.mdf* Datei aus dem Projekt heruntergeladen, die in diesem Tutorial können Sie diesen Abschnitt überspringen, da die gespeicherten Prozeduren bereits vorhanden sind.)

In **Server-Explorer**, erweitern Sie *School.mdf*, mit der rechten Maustaste **gespeicherte Prozeduren**, und wählen Sie **neue gespeicherte Prozedur hinzufügen**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Kopieren Sie die folgenden SQL-Anweisungen, und fügen Sie sie in der gespeicherten Prozedur-Fenster, und Ersetzen Sie dabei die Skelette gespeicherte Prozedur.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student` Entitäten verfügen über vier Eigenschaften: `PersonID`, `LastName`, `FirstName`, und `EnrollmentDate`. Generiert die Datenbank automatisch den ID-Wert, und die gespeicherte Prozedur akzeptiert Parameter für die anderen drei. Die gespeicherte Prozedur gibt den Wert des Schlüssels für die neue-Zeile-Datensatz, damit Entity Framework, die in der Version der Entität mitverfolgen können, die sie im Arbeitsspeicher beibehält.

Speichern Sie und schließen Sie das Fenster für die gespeicherte Prozedur.

Erstellen Sie eine `InsertInstructor` gespeicherte Prozedur auf die gleiche Weise, die mit den folgenden SQL-Anweisungen:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Erstellen Sie `Update` gespeicherte Prozeduren für `Student` und `Instructor` Entitäten auch. (Die Datenbank verfügt bereits über eine `DeletePerson` gespeicherte Prozedur das funktioniert für beide `Instructor` und `Student` Entitäten.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

In diesem Tutorial ordnen Sie alle drei Funktionen: Insert, Update und Delete – für jeden Entitätstyp. Entity Framework, Version 4 können Sie nur eine zuordnen oder zwei dieser Funktionen zu gespeicherten Prozeduren ohne Zuordnung, die anderen, mit einer Ausnahme: Wenn Sie die Update-Funktion, aber nicht die Löschfunktion zugeordnet, die Entity Framework löst eine Ausnahme bei der Sie Es wurde versucht, eine Entität zu löschen. In Entity Framework, Version 3.5, Sie keine viel Flexibilität beim Zuordnen von gespeicherten Prozeduren: Wenn Sie eine Funktion zugeordnet war es erforderlich, alle drei zuordnen.

Um eine gespeicherte Prozedur zu erstellen, die gelesen, anstatt Daten aktualisiert, erstellen, der alle ausgewählt `Course` Entitäten, die mit den folgenden SQL-Anweisungen:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Die gespeicherten Prozeduren hinzufügen in das Datenmodell

Die gespeicherten Prozeduren werden nun in der Datenbank definiert, aber sie müssen hinzugefügt werden, um das Datenmodell auf Entity Framework zur Verfügung stellen. Open *SchoolModel.edmx*mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen Sie **Modell aus der Datenbank aktualisieren**. In der **hinzufügen** Registerkarte die **Datenbankobjekte auswählen** Dialogfeld erweitern Sie **gespeicherte Prozeduren**, wählen Sie die neu erstellten gespeicherten Prozeduren und die `DeletePerson` gespeicherte Prozedur, und klicken Sie dann auf **Fertig stellen**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Die gespeicherten Prozeduren zuordnen

In der Data Model-Designer, mit der Maustaste der `Student` Entität, und wählen **Zuordnung der gespeicherten Prozedur**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

Die **Mappingdetails** Fenster angezeigt wird, in dem Sie die gespeicherte Prozeduren angeben können, die das Entity Framework verwenden soll, für das Einfügen, aktualisieren und Löschen von Entitäten, die dieses Typs.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Legen Sie die **einfügen** -Funktion **InsertStudent**. Das Fenster zeigt eine Liste der Parameter der gespeicherten Prozedur, von die jede eine Entitätseigenschaft zugeordnet werden muss. Zwei davon werden automatisch zugeordnet, da die Namen identisch sind. Es gibt keine Entitätseigenschaft, die mit dem Namen `FirstName`, daher müssen Sie manuell auswählen `FirstMidName` aus einer Dropdown-Liste, die verfügbaren Entitätseigenschaften anzeigt. (Dies ist, da Sie geändert, dass der Name des der `FirstName` Eigenschaft `FirstMidName` im ersten Tutorial.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

In der gleichen **Mappingdetails** Fenster, Zuordnung der `Update` -Funktion der `UpdateStudent` gespeicherte Prozedur (Geben Sie unbedingt `FirstMidName` als Parameterwert für `FirstName`, ebenso wie für die `Insert` gespeicherte Prozedur) und die `Delete` Funktion, um die `DeletePerson` gespeicherte Prozedur.

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Führen Sie dasselbe Verfahren zum Zuordnen von den INSERT-, Update- und Delete gespeicherten Prozeduren für Dozenten zu der `Instructor` Entität.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Für gespeicherte Prozeduren, die statt der Update-Daten lesen, verwenden Sie die **Modellbrowser** Fenster aus, um die gespeicherte Prozedur für die Entität zugeordnet. Geben Sie ihn gibt. In der Data Model-Designer, mit der Maustaste Entwurfsoberfläche, und wählen **Modellbrowser**. Öffnen Sie die **SchoolModel.Store** Knoten, und öffnen Sie dann die **gespeicherte Prozeduren** Knoten. Klicken Sie dann mit der rechten Maustaste die `GetCourses` gespeicherte Prozedur, und wählen Sie **Funktionsimport hinzufügen**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

In der **Funktionsimport hinzufügen** Dialogfeld **gibt eine Auflistung von** wählen **Entitäten**, und wählen Sie dann `Course` als den Entitätstyp zurückgegeben. Wenn Sie fertig sind, klicken Sie auf **OK**. Speichern und schließen Sie die *EDMX* Datei.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Verwenden von Insert, aktualisieren und Löschen von gespeicherten Prozeduren

Gespeicherte Prozeduren zum Einfügen, aktualisieren und Löschen von Daten wird vom Entity Framework automatisch nach dem Datenmodell hinzugefügt wurden und sie die entsprechenden Entitäten zugeordnet. Sie können jetzt ausführen der *StudentsAdd.aspx* Seite und jedes Mal, wenn Sie einen neuen Studenten erstellt haben, wird das Entity Framework verwenden die `InsertStudent` gespeicherte Prozedur, um die neue Zeile hinzuzufügen der `Student` Tabelle.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Führen Sie die *Students.aspx* Seite und den neuen Studenten in der Liste angezeigt.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Ändern Sie den Namen, um sicherzustellen, dass die Update-Funktion funktioniert, und löschen Sie dann die "Student", um sicherzustellen, dass die Delete-Funktion arbeitet.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Verwenden von auf gespeicherten Prozeduren

Entity Framework ist nicht automatisch gespeicherten Prozeduren ausführen, wie z. B. `GetCourses`, und Sie nicht verwenden, mit der `EntityDataSource` Steuerelement. Um diese zu verwenden, rufen Sie sie aus Code.

Öffnen der *InstructorsCourses.aspx.cs* Datei. Die `PopulateDropDownLists` Methode verwendet eine LINQ to Entities-Abfrage, um alle Course-Entitäten abgerufen werden, damit sie die Liste durchlaufen und bestimmen, welche ein Dozent zugewiesen ist und welche nicht zugewiesen werden kann:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Ersetzen Sie dies durch den folgenden Code:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

Die Seite nun verwendet der `GetCourses` gespeicherte Prozedur zum Abrufen der Liste aller Kurse. Führen Sie die Seite, um sicherzustellen, dass es funktioniert wie vorher.

(Eigenschaften von Entitäten abgerufen, indem eine gespeicherte Prozedur können mit den Daten, die im Zusammenhang mit diesen Entitäten, die je nach nicht automatisch aufgefüllt `ObjectContext` Standardeinstellungen. Weitere Informationen finden Sie unter [laden verbundener Objekte](https://msdn.microsoft.com/library/bb896272.aspx) in der MSDN Library.)

Im nächsten Tutorial erfahren Sie, wie Sie Dynamic Data-Funktionalität verwenden, um die Anwendung und Testen von Formatierung und Validierung Regeln zu vereinfachen. Anstatt auf jeder Webseite-Regeln, wie z. B. Daten Formatzeichenfolgen und davon, ob ein Feld erforderlich ist, können Sie diese Regeln in Datenmodellmetadaten angeben, und sie werden automatisch auf jeder Seite angewendet.

> [!div class="step-by-step"]
> [Zurück](the-entity-framework-and-aspnet-getting-started-part-6.md)
> [Weiter](the-entity-framework-and-aspnet-getting-started-part-8.md)
