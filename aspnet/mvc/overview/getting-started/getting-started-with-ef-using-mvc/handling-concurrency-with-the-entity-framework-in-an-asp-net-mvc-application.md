---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Behandeln von Parallelität mit dem Entity Framework 6 in einer ASP.NET MVC 5-Anwendung (10 12) | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b92aded80ad6b435a2409a137bb96fe4d0a726f4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875654"
---
<a name="handling-concurrency-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-10-of-12"></a>Behandeln von Parallelität mit dem Entity Framework 6 in einer ASP.NET MVC 5-Anwendung (10 12)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio 2013. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


In früheren Lernprogrammen haben Sie gelernt, Daten zu aktualisieren. In diesem Tutorial wird gezeigt, wie Sie Konflikte behandeln, wenn mehrere Benutzer gleichzeitig dieselbe Entität aktualisieren.

Ändern Sie die Webseiten, die mit arbeiten die `Department` Entität, damit sie Parallelitätsfehlern behandeln. Die folgenden Abbildungen zeigen die Seiten Index und löschen, einschließlich einige Nachrichten, die angezeigt werden, wenn einem Parallelitätskonflikt.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Parallelitätskonflikte

Ein Parallelitätskonflikt tritt auf, wenn ein Benutzer die Daten einer Entität anzeigt, um diese zu bearbeiten, und ein anderer Benutzer eben diese Entitätsdaten aktualisiert, bevor die Änderungen des ersten Benutzers in die Datenbank geschrieben wurden. Wenn Sie die Erkennung solcher Konflikte nicht aktivieren, überschreibt das letzte Update der Datenbank die Änderungen des anderen Benutzers. In vielen Anwendungen ist dieses Risiko akzeptabel: Wenn es nur wenige Benutzer bzw. wenige Updates gibt, oder wenn es nicht schlimm ist, dass Änderungen überschrieben werden können, ist es den Aufwand, für die Parallelität zu programmieren, möglicherweise nicht wert. In diesem Fall müssen Sie für die Anwendung keine Behandlung von Nebenläufigkeitskonflikten konfigurieren.

### <a name="pessimistic-concurrency-locking"></a>Die eingeschränkte Parallelität (Sperren)

Wenn Ihre Anwendung versehentliche Datenverluste in Parallelitätsszenarios verhindern muss, ist die Verwendung von Datenbanksperren eine Möglichkeit. Hierbei spricht *eingeschränkte Parallelität*. Bevor Sie zum Beispiel eine Zeile aus einer Datenbank lesen, fordern Sie eine Sperre für den schreibgeschützten Zugriff oder den Aktualisierungszugriff an. Wenn Sie eine Zeile für den Aktualisierungszugriff sperren, kann kein anderer Benutzer diese Zeile für den schreibgeschützten Zugriff oder den Aktualisierungszugriff sperren, da er eine Kopie der Daten erhalten würde, die gerade geändert werden. Wenn Sie eine Zeile für den schreibgeschützten Zugriff sperren, können andere diese Zeile ebenfalls für den schreibgeschützten Zugriff sperren, aber nicht für den Aktualisierungszugriff.

Das Verwalten von Sperren hat Nachteile. Es kann komplex sein, sie zu programmieren. Dies erfordert erhebliche Datenbankverwaltungsressourcen und kann mit steigender Anzahl der Benutzer einer Anwendung zu Leistungsproblemen führen. Aus diesen Gründen unterstützen nicht alle Datenbankverwaltungssysteme die pessimistische Parallelität. Das Entity Framework stellt keine integrierte Unterstützung dafür, und dieses Lernprogramms nicht zeigen, wie sie implementiert.

### <a name="optimistic-concurrency"></a>Optimistische Nebenläufigkeit

Die Alternative für die eingeschränkte Parallelität wird *vollständige Parallelität*. Die Verwendung der optimistischen Parallelität bedeutet, Nebenläufigkeitskonflikte zu erlauben und entsprechend zu reagieren, wenn diese auftreten. John führt z. B. die Seite Abteilungen bearbeiten, ändert sich die **Budget** Betrag für die englischen Abteilung von $350,000.00 0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Bevor John klickt **speichern**, Andrea führt die gleiche Seite und Änderungen der **Startdatum** Feld von 9/1/2007, 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John klickt **speichern** First und seine ändern, wenn der Browser an die Indexseite, dann stellt Andrea zurückgibt klickt sieht **speichern**. Was daraufhin geschieht, ist abhängig davon, wie Sie Nebenläufigkeitskonflikte behandeln. Einige der Optionen schließen Folgendes ein:

- Sie können nachverfolgen, welche Eigenschaft ein Benutzer geändert hat und nur die entsprechenden Spalten in der Datenbank aktualisieren. Im Beispielszenario würden keine Daten verloren gehen, da verschiedene Eigenschaften von zwei Benutzern aktualisiert wurden. Das nächste Mal eine Person durchsucht die englische Abteilung, sehen sie Peters und Janes Änderungen – ein Startdatum des 8/8/2013 und das Budget für 0 (null) Dollar.

    Diese Methode der Aktualisierung kann die Anzahl von Konflikten reduzieren, die zu Datenverlusten führen können. Sie kann Datenverluste jedoch nicht verhindern, wenn konkurrierende Änderungen an der gleichen Eigenschaft einer Entität vorgenommen werden. Ob Entity Framework auf diese Weise funktioniert, hängt davon ab, wie Sie Ihren Aktualisierungscode implementieren. Oft ist dies in Webanwendungen nicht praktikabel, da es erforderlich sein kann, viele Zustände zu verwalten, um alle ursprünglichen Eigenschaftswerte einer Entität und die neuen Werte im Auge zu behalten. Das Verwalten vieler Zuständen kann sich auf die Leistung der Anwendung auswirken, da es entweder Serverressourcen beansprucht oder in der Webseite selbst (z.B. in ausgeblendeten Feldern) oder in einem Cookie enthalten sein muss.
- Sie können Janes Änderung Peters Änderung überschreiben lassen. Das nächste Mal eine Person durchsucht die englische Abteilung, sehen sie 8/8/2013 und die wiederhergestellten $350,000.00-Wert. Das ist entweder ein *Client gewinnt*- oder ein *Letzter Schreiber gewinnt*-Szenario. (Alle Werte des Clients haben Vorrang vor dem Datenspeicher.) Wie in der Einführung dieses Abschnitts beschrieben, geschieht dies automatisch, wenn Sie für die Behandlung der Parallelität keinen Code schreiben.
- Sie können verhindern, dass Janes Änderung in der Datenbank aktualisiert wird. In der Regel würden Sie zeigt eine Fehlermeldung an, zeigen sie den aktuellen Zustand der Daten und ermöglicht es ihr Änderungen erneut angewendet, wenn sie noch machen möchte. Dieses Szenario wird *Store Wins* (Speicher gewinnt) genannt. (Die Werte des Datenspeichers haben Vorrang vor den Werten, die vom Client gesendet werden.) In diesem Tutorial implementieren Sie das Szenario „Speicher gewinnt“. Diese Methode stellt sicher, dass keine Änderung überschrieben wird, ohne dass ein Benutzer darüber benachrichtigt wird.

### <a name="detecting-concurrency-conflicts"></a>Erkennen von Konflikten bei der Parallelität

Lösen von Konflikten durch behandeln [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Ausnahmen, die das Entity Framework löst. Entity Framework muss dazu in der Lage sein, Konflikte zu erkennen, damit es weiß, wann diese Ausnahmen ausgelöst werden sollen. Aus diesem Grund müssen Sie die Datenbank und das Datenmodell entsprechend konfigurieren. Einige der Optionen für das Aktivieren der Konflikterkennung schließen Folgendes ein:

- Fügen Sie eine Änderungsverfolgungsspalte in die Datenbanktabelle ein, die verwendet werden kann, um zu bestimmen, wenn eine Änderung an einer Zeile vorgenommen wurde. Anschließend konfigurieren Sie das Entity Framework Einbeziehung dieser Spalte in der `Where` -Klausel der SQL `Update` oder `Delete` Befehle.

    Der Datentyp der Spalte Überwachung ist in der Regel [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). Die [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) Wert ist eine sequenzielle Zahl, die jedes Mal erhöht ist die Zeile aktualisiert wird. In einer `Update` oder `Delete` Befehl, der `Where` -Klausel enthält den ursprünglichen Wert des Nachverfolgungsspalte (die ursprüngliche Zeilenversion). Wenn Sie die aktualisierte Zeile bereits durch den Wert in einen anderen Benutzer geändert wurde die `rowversion` Spalte unterscheidet sich von den ursprünglichen Wert also die `Update` oder `Delete` Anweisung zu aufgrund der zu aktualisierenden Zeile kann nicht gefunden werden die `Where` Klausel. Wenn Entity Framework findet, dass keine Zeilen von aktualisiert wurden die `Update` oder `Delete` Befehl (d. h., wenn die Anzahl der betroffenen Zeilen auf 0 (null) ist), die als Parallelitätskonflikt interpretiert.
- Konfigurieren Sie das Entity Framework, um die ursprünglichen Werte der jede Spalte in der Tabelle enthalten den `Where` -Klausel der `Update` und `Delete` Befehle.

    Wie in der ersten Option, wenn alle Elemente in der Zeile geändert wurde, seit die Zeile zuerst gelesen wurde die `Where` Klausel keine Zeile zu aktualisieren, der das Entity Framework als Parallelitätskonflikt interpretiert zurückgegeben. Für Datenbanktabellen, die viele Spalten aufweisen, dieser Ansatz kann dazu führen, sehr große `Where` -Klausel und kann festlegen, dass Sie große Mengen an Zustand beibehalten. Wie bereits erwähnt, kann das Verwalten großer Mengen von Zuständen die Anwendungsleistung beeinträchtigen. Deshalb wird dieser Ansatz in der Regel nicht empfohlen, und ist nicht die Methode, die in diesem Tutorial verwendet wird.

    Wenn Sie diesen Ansatz zur Parallelität implementieren möchten, müssen Sie alle nicht primären Schlüssel-Eigenschaften in der Entität kennzeichnen Sie durch Hinzufügen von Parallelität für nachverfolgen möchten die [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) -Attribut auf diese. Dass die Änderung der Entity Framework enthalten alle Spalten in der SQL ermöglicht `WHERE` -Klausel der `UPDATE` Anweisungen.

Im weiteren Verlauf dieses Lernprogramms fügen Sie eine [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) tracking-Eigenschaft, um die `Department` Entität, einen Controller und Ansichten zu erstellen und testen, um sicherzustellen, dass alles ordnungsgemäß funktioniert.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Eine vollständige Parallelität-Eigenschaft der Department-Entität hinzufügen

In *Models\Department.cs*, Hinzufügen einer Überwachung-Eigenschaft, die mit dem Namen `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

Die [Zeitstempel](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) Attribut gibt an, dass diese Spalte Bestandteil der `Where` -Klausel der `Update` und `Delete` Befehle an die Datenbank gesendet. Das Attribut heißt [Zeitstempel](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) da frühere Versionen von SQL Server eine SQL verwendet [Zeitstempel](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) -Datentyp, bevor der SQL- [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) ersetzt. Der Typ .net für [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) ist ein Bytearray.

Wenn Sie die fluent-API verwenden möchten, können Sie mithilfe der [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) Methode, um die Tracking-Eigenschaft anzugeben, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Durch das Hinzufügen einer Eigenschaft ändern Sie das Datenbankmodell, daher müssen Sie eine weitere Migration durchführen. Geben Sie die folgenden Befehle in die Paket-Manager-Konsole ein:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-the-department-controller"></a>Ändern von Controller-Abteilung

In *Controllers\DepartmentController.cs*, Hinzufügen einer `using` Anweisung:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

In der *DepartmentController.cs* Datei, ändern Sie alle vier Vorkommen von "LastName" in "FullName", sodass die Abteilung Administrator Dropdown-Listen der vollständige Name der Kursleiter anstatt nur der letzte Name enthält.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Ersetzen Sie den vorhandenen Code für die `HttpPost` `Edit` -Methode durch folgenden Code:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Wenn die Methode `FindAsync` NULL zurückgibt, wurde die Abteilung von einem anderen Benutzer gelöscht. Dargestellten Code verwendet die übermittelte Formularwerte, um eine Entität Department erstellen, sodass die bearbeiten (Seite) mit einer Fehlermeldung angezeigt werden kann. Alternativ müssen Sie die Abteilungsentität nicht erneut erstellen, wenn Sie nur eine Fehlermeldung anzeigen, ohne die Abteilungsfelder erneut anzuzeigen.

Die Sicht speichert die ursprüngliche `RowVersion` Wert in ein verborgenes Feld und die Methode erhält sie in der `rowVersion` Parameter. Bevor Sie `SaveChanges` aufrufen, müssen Sie diesen ursprünglichen Eigenschaftswert von `RowVersion` in die Auflistung `OriginalValues` für die Entität einfügen. Klicken Sie dann, wenn das Entity Framework eine SQL erstellt `UPDATE` Befehl, mit dem Befehl einschließen, wird eine `WHERE` -Klausel, die für eine Zeile sucht, die die ursprüngliche `RowVersion` Wert.

Wenn keine Zeilen betroffen sind der `UPDATE` Befehl (keine Zeilen vorhanden sind, die ursprüngliche `RowVersion` Wert), das Entity Framework löst eine `DbUpdateConcurrencyException` Ausnahme und dem Code in der `catch` Block Ruft die betroffenen `Department` Entität aus der Ausnahme -Objekt.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Dieses Objekt verfügt über die neuen Werte eingegeben haben, vom Benutzer in seiner `Entity` -Eigenschaft, und Sie erhalten die Werte aus der Datenbank gelesen wird, durch Aufrufen der `GetDatabaseValues` Methode.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Die `GetDatabaseValues` Methodenrückgabe null, wenn ein Benutzer die Zeile aus der Datenbank gelöscht wurde; Andernfalls müssen Sie das zurückgegebene Objekt umwandeln der `Department` Klasse Zugriff auf die `Department` Eigenschaften. (Da Sie bereits zum Löschen überprüft `databaseEntry` wäre nur, wenn die Abteilung nach gelöscht wurde null `FindAsync` ausgeführt wird und vor dem `SaveChanges` ausgeführt wird.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Der Code fügt anschließend eine benutzerdefinierte Fehlermeldung für jede Spalte, die Datenbankwerte unterscheiden, was der Benutzer auf der Seite "Bearbeiten" eingegeben wurde:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

Eine längere Fehlermeldung wird erklärt, was geschehen ist, und was wird:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

Schließlich der Code legt die `RowVersion` Wert, der die `Department` Objekt, das den neuen Wert aus der Datenbank abgerufen. Dieser neue `RowVersion`-Wert wird in dem ausgeblendeten Feld gespeichert, wenn die Seite „Bearbeiten“ erneut angezeigt wird. Das nächste Mal, wenn der Benutzer auf **Speichern** klickt, werden nur Parallelitätsfehler abgefangen, die nach dem erneuten Anzeigen der Seite „Bearbeiten“ aufgetreten sind.

In *Views\Department\Edit.cshtml*, fügen Sie ein ausgeblendetes Feld zum Speichern der `RowVersion` Eigenschaftswert, der unmittelbar hinter das ausgeblendete Feld für die `DepartmentID` Eigenschaft:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="testing-optimistic-concurrency-handling"></a>Testen die vollständige Parallelitätsbehandlung

Führen Sie den Standort aus, und klicken Sie auf **Abteilungen**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Klicken Sie mit der rechten Maustaste auf die **bearbeiten** Link für die englischen Abteilung, und wählen **in neuer Registerkarte öffnen** klicken Sie dann auf die **bearbeiten** Link für die englischen Abteilung. Die beiden Registerkarten werden dieselben Informationen angezeigt.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Ändern Sie ein Feld in der ersten Registerkarte, und klicken Sie auf **Speichern**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Der Browser zeigt die Indexseite mit dem geänderten Wert an.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Ändern eines Felds in der zweiten Registerkarte ' Browser ', und klicken Sie auf **speichern**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Klicken Sie auf **speichern** in der zweiten Registerkarte. Folgende Fehlermeldung wird angezeigt:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Klicken Sie erneut auf **Speichern**. Der in der zweiten Registerkarte eingegebene Wert wird zusammen mit den ursprünglichen Wert der Daten gespeichert, die Sie in der ersten Browser geändert haben. Die gespeicherten Werte werden Ihnen auf der Indexseite angezeigt.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualisieren die Seite "löschen"

Bei der Seite „Löschen“ entdeckt Entity Framework Nebenläufigkeitskonflikte, die durch die Bearbeitung einer Abteilung ausgelöst wurden, auf ähnliche Weise. Wenn die `HttpGet` `Delete` Methode zeigt, die Bestätigung an, die Ansicht enthält die ursprüngliche `RowVersion` Wert in ein ausgeblendetes Feld. Dieser Wert dann zur Verfügung, um wird die `HttpPost` `Delete` -Methode, die aufgerufen wird, wenn der Benutzer den Löschvorgang bestätigt. Wenn Entity Framework erstellt SQL `DELETE` Befehl, er enthält eine `WHERE` -Klausel mit der ursprünglichen `RowVersion` Wert. Wenn die Ergebnisse von Befehlen in 0 Zeilen betroffen (d. h. die Zeile geändert wurde, nach die Bestätigungsseite löschen angezeigt wurde), wird eine Parallelitätsausnahme ausgelöst, und die `HttpGet Delete` -Methode aufgerufen wird und ein Kennzeichen "Fehler" auf `true` um Kostenoptionen der Bestätigungsseite mit einer Fehlermeldung. Es ist auch möglich, dass 0 Zeilen betroffen sind, da die Zeile von einem anderen Benutzer gelöscht wurde, damit in diesem Fall eine andere Fehlermeldung angezeigt wird.

In *DepartmentController.cs*, ersetzen Sie die `HttpGet` `Delete` -Methode durch folgenden Code:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Die Methode akzeptiert einen optionalen Parameter, der angibt, ob die Seite nach einem Parallelitätsfehler erneut angezeigt wird. Wenn dieses Flag ist `true`, eine Fehlermeldung wird gesendet, um die Ansicht mit einem `ViewBag` Eigenschaft.

Ersetzen Sie den Code in der `HttpPost` `Delete` Methode (mit dem Namen `DeleteConfirmed`) durch den folgenden Code:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

In dem eingerüsteten Code, den Sie soeben ersetzt haben, akzeptiert diese Methode nur eine Datensatz-ID:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Sie geändert haben, diesen Parameter, um eine `Department` Entitätsinstanz erstellt, indem der Modellbinder. Dadurch erhalten Sie Zugriff auf die `RowVersion` Eigenschaftswert zusätzlich zu dem Datensatzschlüssel.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Ebenfalls haben Sie den Namen der Aktionsmethode `DeleteConfirmed` auf `Delete` geändert. Der scaffolded Code mit dem Namen der `HttpPost` `Delete` Methode `DeleteConfirmed` so erteilen Sie die `HttpPost` Methode eine eindeutige Signatur. (Die CLR erfordert überladene Methoden zum anderen Methodenparameter angegeben haben.) Nun, dass die Signaturen eindeutig sind, können Sie mit der MVC-Konvention einhalten und den gleichen Namen für die `HttpPost` und `HttpGet` Löschmethoden.

Wenn ein Parallelitätsfehler abgefangen wird, zeigt der Code erneut die Bestätigungsseite „Löschen“ an, und stellt ein Flag bereit, das angibt, dass eine Fehlermeldung für die Parallelität angezeigt werden soll.

In *Views\Department\Delete.cshtml*, ersetzen Sie den scaffolded Code durch den folgenden Code, der ein Fehler Nachrichtenfeld und ausgeblendete Felder für die Eigenschaften "DepartmentID" und RowVersion hinzufügt. Die Änderungen werden hervorgehoben.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

Dieser Code Fügt eine Fehlermeldung zwischen den `h2` und `h3` Überschriften:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Er ersetzt `LastName` mit `FullName` in die `Administrator` Feld:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Schließlich fügt sie ausgeblendete Felder für die `DepartmentID` und `RowVersion` Eigenschaften nach der `Html.BeginForm` Anweisung:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

Führen Sie die Indexseite Abteilungen. Klicken Sie mit der rechten Maustaste auf die **löschen** Link für die englischen Abteilung, und wählen **in neuer Registerkarte öffnen** klicken Sie in der ersten Registerkarte der **bearbeiten** Link für die englischen Abteilung.

Im ersten Fenster ändern Sie einen der Werte, und klicken Sie auf **speichern** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

Die Indexseite bestätigt die Änderung.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Klicken Sie in der zweiten Registerkarte auf **Löschen**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Ihnen wird eine Fehlermeldung zur Parallelität angezeigt, und die Abteilungswerte werden mit den aktuellen Werten der Datenbank aktualisiert.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Wenn Sie erneut auf **Löschen** klicken, werden Sie auf die Indexseite weitergeleitet, die anzeigt, dass die Abteilung gelöscht wurde.

## <a name="summary"></a>Zusammenfassung

Damit ist die Einführung in die Behandlung von Nebenläufigkeitskonflikten abgeschlossen. Informationen zu anderen Methoden zum Behandeln von verschiedenen Parallelitätsszenarien finden Sie unter [optimistische Parallelität Muster](https://msdn.microsoft.com/data/jj592904) und [arbeiten mit Eigenschaftswerten](https://msdn.microsoft.com/data/jj592677) auf MSDN. Im nächste Lernprogramm veranschaulicht das Implementieren von Tabelle pro Hierarchie Vererbung für den `Instructor` und `Student` Entitäten.

Links zu anderen Entity Framework-Ressourcen finden Sie in der [ASP.NET Data Access - Ressourcen empfohlen](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Weiter](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
