---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
title: "Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 Web Forms - Teil 7 | Microsoft Docs"
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework. Die beispielanwendung ist...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: f8afb245-b705-419c-8790-0b295e90d5e2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-7
msc.type: authoredcontent
ms.openlocfilehash: 7697763b97e36304d686c77e8cedd060d630c530
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-7"></a>Erste Schritte mit Entity Framework 4.0-Datenbank zunächst und ASP.NET 4 WebForms - Teil 7
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET Web Forms-Anwendungen, die mit dem Entity Framework 4.0 und Visual Studio 2010. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-stored-procedures"></a>Verwenden von gespeicherten Prozeduren

Im vorherigen Lernprogramm implementieren Sie eine Tabelle pro Hierarchie Vererbungsmuster. In diesem Lernprogramm erfahren Sie, wie Sie gespeicherte Prozeduren verwenden, um mehr Kontrolle über Zugriff auf die Datenbank zu erhalten.

Das Entity Framework können Sie angeben, dass gespeicherte Prozeduren für den Datenbankzugriff verwendet werden soll. Für jeden Entitätstyp können Sie eine gespeicherte Prozedur für erstellen, aktualisieren oder Löschen von Entitäten des Typs verwenden angeben. Anschließend können Sie in das Datenmodell Verweise auf gespeicherte Prozeduren hinzufügen, die Sie verwenden können, um Aufgaben wie das Abrufen von Gruppen von Entitäten.

Verwenden von gespeicherten Prozeduren besteht eine häufige Anforderung für den Datenbankzugriff. In einigen Fällen kann ein Datenbankadministrator erforderlich gespeicherte Prozeduren aus Sicherheitsgründen jeglicher Datenbankzugriff durchlaufen. In anderen Fällen möchten Sie möglicherweise von Geschäftslogik in einige der Prozesse erstellen, die das Entity Framework verwendet, wenn die Datenbank aktualisiert. Wenn eine Entität gelöscht wird sollten Sie es auf eine Datenbank "Archive" kopieren. Oder wenn eine Zeile aktualisiert wird Sie möglicherweise einer Zeile in einer Protokollierungstabelle von aufgezeichneten schreiben möchten, die Änderung vorgenommen hat. Sie können diese Arten von Aufgaben in einer gespeicherten Prozedur ausführen, die aufgerufen wird, wenn das Entity Framework eine Entität löscht oder eine Entität aktualisiert.

Wie das vorherige Lernprogramm erstellen Sie keine neuen Seiten. Stattdessen ändern die Methode Sie, die das Entity Framework für einige der Seiten, die auf die Datenbank zugreift Sie bereits erstellt haben.

In diesem Lernprogramm erstellen Sie gespeicherte Prozeduren in der Datenbank für das Einfügen von `Student` und `Instructor` Entitäten. Fügen Sie sie in das Datenmodell, und Sie müssen angeben, dass für das Entity Framework sie zum Hinzufügen von `Student` und `Instructor` Entitäten in der Datenbank. Erstellen Sie eine gespeicherte Prozedur, die Sie, zum Abrufen verwenden können auch `Course` Entitäten.

## <a name="creating-stored-procedures-in-the-database"></a>Erstellen von gespeicherten Prozeduren in der Datenbank

(Bei Verwendung der *School.mdf* Datei aus dem Projekt zum Download für dieses Lernprogramm zur Verfügung, können Sie diesen Abschnitt überspringen, da die gespeicherten Prozeduren vorhanden.)

In **Server-Explorer**, erweitern Sie *School.mdf*, mit der rechten Maustaste **gespeicherte Prozeduren**, und wählen Sie **neue gespeicherte Prozedur hinzufügen**.

[![image15](the-entity-framework-and-aspnet-getting-started-part-7/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image1.png)

Kopieren Sie die folgenden SQL-Anweisungen, und fügen Sie sie in der gespeicherten Prozedur ein und Ersetzen Sie dabei das Gerüst eines gespeicherte Prozedur.

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample1.sql)]

[![image14](the-entity-framework-and-aspnet-getting-started-part-7/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image3.png)

`Student`Entitäten verfügen über vier Eigenschaften: `PersonID`, `LastName`, `FirstName`, und `EnrollmentDate`. Generiert die Datenbank automatisch den ID-Wert, und die gespeicherte Prozedur akzeptiert Parameter für die anderen drei. Die gespeicherte Prozedur gibt den Wert des Schlüssels für die neue Zeile Datensatz zurück, sodass Entity Framework nachverfolgen, die in der Version der Entität von können, die im Arbeitsspeicher behält.

Speichern Sie und schließen Sie das Fenster für die gespeicherte Prozedur.

Erstellen einer `InsertInstructor` gespeicherte Prozedur auf die gleiche Weise, die mithilfe der folgenden SQL-Anweisungen:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample2.sql)]

Erstellen Sie `Update` gespeicherten Systemprozeduren für die `Student` und `Instructor` Entitäten auch. (Die Datenbank bereits eine `DeletePerson` gespeicherte Prozedur, die für beide funktionieren `Instructor` und `Student` Entitäten.)

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample3.sql)]

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample4.sql)]

In diesem Lernprogramm ordnen Sie alle drei Funktionen – INSERT-, Update- und Delete – für jeden Entitätstyp. Entity Framework, Version 4 können Sie nur einen zuordnen oder zwei dieser Funktionen zu gespeicherten Prozeduren ohne Zuordnung, die anderen, mit einer Ausnahme: Wenn Sie die Update-Funktion, aber nicht die Delete-Funktion zugeordnet sind, das Entity Framework löst eine Ausnahme bei der Sie der Versuch, eine Entität zu löschen. In Entity Framework, Version 3.5, Sie keine viel Flexibilität bei der Zuordnung gespeicherter Prozeduren: Wenn Sie eine bestimmte Funktion zugeordnet waren Sie ordnen Sie alle drei erforderlich.

Um eine gespeicherte Prozedur zu erstellen, die Daten aktualisiert, sondern liest, erstellen Sie eine, die alle auswählt `Course` Entitäten, die mithilfe der folgenden SQL-Anweisungen:

[!code-sql[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample5.sql)]

## <a name="adding-the-stored-procedures-to-the-data-model"></a>Die gespeicherten Prozeduren hinzufügen in das Datenmodell

Die gespeicherten Prozeduren werden jetzt in der Datenbank definiert, aber sie müssen hinzugefügt werden, in das Datenmodell, um sie zu Entity Framework verfügbar zu machen. Open *SchoolModel.edmx*mit der rechten Maustaste auf die Entwurfsoberfläche, und wählen Sie **Modell aus der Datenbank aktualisieren**. In der **hinzufügen** auf der Registerkarte die **Datenbankobjekte auswählen** Dialogfeld erweitern Sie **gespeicherte Prozeduren**, wählen Sie die neu erstellten gespeicherten Prozeduren und die `DeletePerson` gespeicherte Prozedur, und klicken Sie dann auf **Fertig stellen**.

[![image20](the-entity-framework-and-aspnet-getting-started-part-7/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image5.png)

## <a name="mapping-the-stored-procedures"></a>Zuordnen von gespeicherten Prozeduren

Im Data Model-Designer mit der Maustaste die `Student` Entität, und wählen **Zuordnung der gespeicherten Prozedur**.

[![image21](the-entity-framework-and-aspnet-getting-started-part-7/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image7.png)

Die **Mappingdetails** Fenster angezeigt wird, in dem Sie gespeicherte Prozeduren angeben können, die das Entity Framework zum Einfügen, aktualisieren und Löschen von Entitäten dieses Typs verwenden soll.

[![image22](the-entity-framework-and-aspnet-getting-started-part-7/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image9.png)

Legen Sie die **einfügen** -Funktion **InsertStudent**. Das Fenster zeigt eine Liste der Parameter der gespeicherten Prozedur, von die jeder eine Entitätseigenschaft zugeordnet werden muss. Zwei Methoden werden automatisch zugeordnet, da die Namen identisch sind. Es ist keine Eigenschaft der Entität mit dem Namen `FirstName`, daher müssen Sie manuell auswählen `FirstMidName` aus einer Dropdown-Liste, die verfügbare Entitätsklasse Eigenschaften anzeigt. (Dies ist, da Sie den Namen des geändert die `FirstName` Eigenschaft `FirstMidName` im ersten Lernprogramm.)

[![image23](the-entity-framework-and-aspnet-getting-started-part-7/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image11.png)

In der gleichen **Mappingdetails** Fenster, die Zuordnung der `Update` -Funktion die `UpdateStudent` gespeicherte Prozedur (Stellen Sie sicher, dass Sie angeben `FirstMidName` als Parameterwert für `FirstName`, wie Sie für die `Insert` gespeicherte Systemprozedur) und die `Delete` -Funktion die `DeletePerson` gespeicherte Prozedur.

[![Image01 abgerufen wird](the-entity-framework-and-aspnet-getting-started-part-7/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image13.png)

Führen Sie dasselbe Verfahren zum Zuordnen der INSERT-, Update- und Delete-Prozeduren für Lehrkräfte auf die `Instructor` Entität.

[![image02](the-entity-framework-and-aspnet-getting-started-part-7/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image15.png)

Für gespeicherte Prozeduren, die statt der Updatedaten gelesen werden, verwenden Sie die **Modellbrowser** Fenster aus, um die gespeicherte Prozedur auf die Entität zuordnen Geben Sie ihn zurück. Im Data Model-Designer mit der Maustaste des Entwurfs, die Entwurfsoberfläche, und wählen **Modellbrowser**. Öffnen Sie die **SchoolModel.Store** Knoten, und öffnen Sie dann die **gespeicherte Prozeduren** Knoten. Klicken Sie dann mit der rechten Maustaste die `GetCourses` gespeicherte Prozedur, und wählen Sie **Funktionsimport hinzufügen**.

[![image24](the-entity-framework-and-aspnet-getting-started-part-7/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image17.png)

In der **Funktionsimport hinzufügen** Dialogfeld unter **gibt eine Auflistung von** wählen **Entitäten**, und wählen Sie dann `Course` als den Entitätstyp zurückgegeben. Wenn Sie fertig sind, klicken Sie auf **OK**. Speichern und schließen Sie die *EDMX* Datei.

[![image25](the-entity-framework-and-aspnet-getting-started-part-7/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image19.png)

## <a name="using-insert-update-and-delete-stored-procedures"></a>Verwenden von Insert, aktualisieren und Löschen von gespeicherten Prozeduren

Gespeicherte Prozeduren zum Einfügen, aktualisieren und Löschen von Daten werden vom Entity Framework automatisch verwendet, nachdem Sie das Datenmodell hinzugefügt und diese zu den entsprechenden Entitäten zugeordnet haben. Sie können jetzt ausführen der *StudentsAdd.aspx* Seite und jedes Mal, wenn Sie einen neuen Studenten erstellt haben, wird das Entity Framework verwendet die `InsertStudent` gespeicherte Prozedur, um die neue Zeile hinzuzufügen der `Student` Tabelle.

[![image03](the-entity-framework-and-aspnet-getting-started-part-7/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image21.png)

Führen Sie die *Students.aspx* Seite und den neuen Studenten in der Liste angezeigt.

[![image04](the-entity-framework-and-aspnet-getting-started-part-7/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image23.png)

Ändern Sie den Namen, um sicherzustellen, dass die Update-Funktion, und löschen Sie dann die Studenten, um sicherzustellen, dass der Delete-Funktion funktioniert.

[![image05](the-entity-framework-and-aspnet-getting-started-part-7/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-7/_static/image25.png)

## <a name="using-select-stored-procedures"></a>Verwenden von Select gespeicherten Prozeduren

Das Entity Framework nicht automatisch aus gespeicherten Prozeduren z. B. `GetCourses`, und Sie können nicht mit der `EntityDataSource` Steuerelement. Um sie zu verwenden, rufen Sie sie aus Code ein.

Öffnen der *InstructorsCourses.aspx.cs* Datei. Die `PopulateDropDownLists` Methode verwendet eine LINQ-to-Entities-Abfrage, um alle Kurs Entitäten abgerufen werden, damit sie die Liste durchlaufen und bestimmen, welche ein Kursleiter zugewiesen wird und welche nicht zugewiesen werden kann:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample6.cs)]

Ersetzen Sie dies durch den folgenden Code:

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-7/samples/sample7.cs)]

Die Seite nun verwendet der `GetCourses` gespeicherte Prozedur beim Abrufen der Liste aller Kurse. Führen Sie die Seite, um sicherzustellen, dass er funktioniert es hat sich nichts geändert.

(Navigationseigenschaften von Entitäten, die abgerufen, indem eine gespeicherte Prozedur können mit den Daten im Zusammenhang mit diesen Entitäten, die je nach nicht automatisch aufgefüllt `ObjectContext` Standardeinstellungen. Weitere Informationen finden Sie unter [Laden von verknüpften Objekten](https://msdn.microsoft.com/en-us/library/bb896272.aspx) in der MSDN Library.)

In den nächsten Lernprogrammen erfahren Sie, wie Dynamic Data-Funktionen zu verwenden, um das Programm und Testen von Formatierung und Validierung Regeln erleichtern. Statt anzugeben, auf jeder Webseite Regeln wie z. B. Formatzeichenfolgen Daten und davon, ob ein Feld erforderlich ist, können Sie diese Regeln in Daten darin enthaltenen Modellmetadaten angeben und sie automatisch auf jeder Seite angewendet werden.

>[!div class="step-by-step"]
[Zurück](the-entity-framework-and-aspnet-getting-started-part-6.md)
[Weiter](the-entity-framework-and-aspnet-getting-started-part-8.md)
