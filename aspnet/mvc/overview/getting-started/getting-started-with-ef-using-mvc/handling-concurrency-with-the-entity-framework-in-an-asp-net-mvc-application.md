---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: "Behandeln von Parallelität mit dem Entity Framework 6 in einer ASP.NET MVC 5-Anwendung (10 12) | Microsoft Docs"
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
ms.openlocfilehash: e7b79503a1d297d40c6824f8b2b7bbc1f42b9fca
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="handling-concurrency-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-10-of-12"></a>Behandeln von Parallelität mit dem Entity Framework 6 in einer ASP.NET MVC 5-Anwendung (10 12)
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio 2013. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


In früheren Lernprogrammen haben Sie gelernt, Daten zu aktualisieren. Dieses Lernprogramm zeigt, wie Konflikte zu behandeln, wenn mehrere Benutzer gleichzeitig derselben Entität aktualisieren.

Ändern Sie die Webseiten, die mit arbeiten die `Department` Entität, damit sie Parallelitätsfehlern behandeln. Die folgenden Abbildungen zeigen die Seiten Index und löschen, einschließlich einige Nachrichten, die angezeigt werden, wenn einem Parallelitätskonflikt.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Parallelitätskonflikte

Parallelitätskonflikt tritt auf, wenn ein Benutzer eine Entität Daten angezeigt, um ihn zu bearbeiten und ein anderer Benutzer aktualisiert dann die gleiche Entität Daten, bevor die erste Änderung des Benutzers in die Datenbank geschrieben wird. Wenn Sie die Erkennung von Konflikte dieser Art nicht aktivieren, muss jede Person zur Aktualisierung der Datenbank zuletzt des anderen Benutzers werden Änderungen überschrieben. In vielen Anwendungen dieses Risiko ist akzeptabel: Wenn nur wenige Benutzer oder einige Updates vorhanden sind oder nicht wirklich wichtig, wenn einige Änderungen überschrieben werden, kann die Kosten für die Programmierung für die Parallelität die Vorteile überwiegen. In diesem Fall müssen Sie die Anwendung zur Handhabung von Parallelitätskonflikten zu konfigurieren.

### <a name="pessimistic-concurrency-locking"></a>Die eingeschränkte Parallelität (Sperren)

Wenn Ihre Anwendung muss in parallelitätsszenarios versehentliche Datenverluste zu verhindern, ist eine Möglichkeit dazu Datenbanksperren verwenden. Hierbei spricht *eingeschränkte Parallelität*. Bevor Sie eine Zeile aus einer Datenbank lesen, Sie anfordern, z. B. eine Sperre für nur-Lese oder Aktualisierungszugriff. Wenn Sie eine Zeile für Aktualisierungszugriff sperren, dürfen keine anderen Benutzer der Zeile, die entweder für Sperren nur-Lese oder Aktualisierungszugriff, da erhalten sie eine Kopie der Daten, die nicht gerade geändert wird. Wenn Sie eine Zeile für schreibgeschützten Zugriff gesperrt werden, können andere auch für schreibgeschützten Zugriff jedoch nicht für Update gesperrt.

Verwalten von Sperren hat Nachteile. Es kann zum Programm komplex sein. Erfordert erhebliche Datenbankressourcen für Verwaltung, und es kann zu Leistungsproblemen führen, als die Anzahl der Benutzer einer Anwendung erhöht. Aus diesen Gründen unterstützen nicht alle Datenbank-Managementsystemen eingeschränkte Parallelität. Das Entity Framework stellt keine integrierte Unterstützung dafür, und dieses Lernprogramms nicht zeigen, wie sie implementiert.

### <a name="optimistic-concurrency"></a>Vollständige Parallelität

Die Alternative für die eingeschränkte Parallelität wird *vollständige Parallelität*. Vollständige Parallelität bedeutet ermöglicht Parallelitätskonflikte durchgeführt werden soll, und klicken Sie dann entsprechend reagieren, wenn dies der Fall. John führt z. B. die Seite Abteilungen bearbeiten, ändert sich die **Budget** Betrag für die englischen Abteilung von $350,000.00 0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Bevor John klickt **speichern**, Andrea führt die gleiche Seite und Änderungen der **Startdatum** Feld von 9/1/2007, 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John klickt **speichern** First und seine ändern, wenn der Browser an die Indexseite, dann stellt Andrea zurückgibt klickt sieht **speichern**. Was daraufhin geschieht richtet sich nach der Behandlung von Parallelitätskonflikten. Einige der Optionen umfassen Folgendes:

- Sie können Nachverfolgen von ein Benutzer geändert hat, dessen Eigenschaft und nur die entsprechenden Spalten in der Datenbank zu aktualisieren. In diesem Beispielszenario würde keine Daten verloren, weil Sie unterschiedliche Eigenschaften von zwei Benutzer aktualisiert wurden. Das nächste Mal eine Person durchsucht die englische Abteilung, sehen sie Peters und Janes Änderungen – ein Startdatum des 8/8/2013 und das Budget für 0 (null) Dollar.

    Diese Methode zur Aktualisierung reduzieren die Anzahl der Konflikte, die zu Datenverlusten führen können, aber es kann nicht Datenverluste vermeiden, wenn die gleiche Eigenschaft einer Entität konkurrierende geändert werden. Ob das Entity Framework auf diese Weise funktioniert, hängt davon ab, wie Update Code implementieren. Es ist häufig nicht praktikabel ist, in einer Web-Anwendung, da erforderlich, große Mengen des Zustands zu verwalten, um nachzuverfolgen, um alle ursprünglichen Eigenschaftswerte für eine Entität sowie die neuen Werte. Verwalten von großen Datenmengen Zustand kann Leistung der Anwendung beeinträchtigen, da es Serverressourcen erfordert oder auf der Webseite selbst (z. B. in ausgeblendeten Feldern) enthalten sein muss oder in einem Cookie.
- Sie können Janes Änderung Peters Änderung überschreiben lassen. Das nächste Mal eine Person durchsucht die englische Abteilung, sehen sie 8/8/2013 und die wiederhergestellten $350,000.00-Wert. Hierbei spricht einen *Client gewinnt* oder *Last in Wins* Szenario. (Alle Werte aus dem Client haben Vorrang vor was im Datenspeicher ist.) Wie in der Einführung zu diesem Abschnitt erwähnt, wenn Sie die Codierung für Parallelitätsbehandlung nicht tun, erfolgt dies automatisch.
- Sie können verhindern, dass Janes Änderung in der Datenbank aktualisiert wird. In der Regel würden Sie zeigt eine Fehlermeldung an, zeigen sie den aktuellen Zustand der Daten und ermöglicht es ihr Änderungen erneut angewendet, wenn sie noch machen möchte. Hierbei spricht einen *Store Wins* Szenario. (Der Datenspeicher Werte haben Vorrang vor den Werten, die vom Client gesendet.) In diesem Lernprogramm implementieren Sie die Wins-Store-Szenario. Diese Methode wird sichergestellt, dass keine Änderungen, ohne dass ein Benutzer wird gewarnt überschrieben werden, was geschieht.

### <a name="detecting-concurrency-conflicts"></a>Erkennen von Konflikten bei der Parallelität

Lösen von Konflikten durch behandeln [OptimisticConcurrencyException](https://msdn.microsoft.com/en-us/library/system.data.optimisticconcurrencyexception.aspx) Ausnahmen, die das Entity Framework löst. Damit Sie wissen, wann diese Ausnahmen ausgelöst werden soll, muss das Entity Framework zum Erkennen von Konflikten können. Aus diesem Grund müssen Sie die Datenbank und des Datenmodells entsprechend konfigurieren. Einige Optionen zum Aktivieren der konflikterkennung umfassen Folgendes:

- Enthalten Sie in der Datenbanktabelle eine Überwachung-Spalte, die verwendet werden kann, um zu bestimmen, wenn eine Zeile geändert wurde. Anschließend konfigurieren Sie das Entity Framework Einbeziehung dieser Spalte in der `Where` -Klausel der SQL `Update` oder `Delete` Befehle.

    Der Datentyp der Spalte Überwachung ist in der Regel [Rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx). Die [Rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) Wert ist eine sequenzielle Zahl, die jedes Mal erhöht ist die Zeile aktualisiert wird. In einer `Update` oder `Delete` Befehl, der `Where` -Klausel enthält den ursprünglichen Wert des Nachverfolgungsspalte (die ursprüngliche Zeilenversion). Wenn Sie die aktualisierte Zeile bereits durch den Wert in einen anderen Benutzer geändert wurde die `rowversion` Spalte unterscheidet sich von den ursprünglichen Wert also die `Update` oder `Delete` Anweisung zu aufgrund der zu aktualisierenden Zeile kann nicht gefunden werden die `Where` Klausel. Wenn Entity Framework findet, dass keine Zeilen von aktualisiert wurden die `Update` oder `Delete` Befehl (d. h., wenn die Anzahl der betroffenen Zeilen auf 0 (null) ist), die als Parallelitätskonflikt interpretiert.
- Konfigurieren Sie das Entity Framework, um die ursprünglichen Werte der jede Spalte in der Tabelle enthalten den `Where` -Klausel der `Update` und `Delete` Befehle.

    Wie in der ersten Option, wenn alle Elemente in der Zeile geändert wurde, seit die Zeile zuerst gelesen wurde die `Where` Klausel keine Zeile zu aktualisieren, der das Entity Framework als Parallelitätskonflikt interpretiert zurückgegeben. Für Datenbanktabellen, die viele Spalten aufweisen, dieser Ansatz kann dazu führen, sehr große `Where` -Klausel und kann festlegen, dass Sie große Mengen an Zustand beibehalten. Wie bereits erwähnt, kann das Verwalten von großen Datenmengen Zustand Leistung der Anwendung beeinträchtigen. Aus diesem Grund diese Vorgehensweise wird im Allgemeinen nicht empfohlen, und dies nicht der Fall in diesem Lernprogramm verwendete Methode.

    Wenn Sie diesen Ansatz zur Parallelität implementieren möchten, müssen Sie alle nicht primären Schlüssel-Eigenschaften in der Entität kennzeichnen Sie durch Hinzufügen von Parallelität für nachverfolgen möchten die [ConcurrencyCheck](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) -Attribut auf diese. Dass die Änderung der Entity Framework enthalten alle Spalten in der SQL ermöglicht `WHERE` -Klausel der `UPDATE` Anweisungen.

Im weiteren Verlauf dieses Lernprogramms fügen Sie eine [Rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) tracking-Eigenschaft, um die `Department` Entität, einen Controller und Ansichten zu erstellen und testen, um sicherzustellen, dass alles ordnungsgemäß funktioniert.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Eine vollständige Parallelität-Eigenschaft der Department-Entität hinzufügen

In *Models\Department.cs*, Hinzufügen einer Überwachung-Eigenschaft, die mit dem Namen `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

Die [Zeitstempel](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) Attribut gibt an, dass diese Spalte Bestandteil der `Where` -Klausel der `Update` und `Delete` Befehle an die Datenbank gesendet. Das Attribut heißt [Zeitstempel](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.timestampattribute.aspx) da frühere Versionen von SQL Server eine SQL verwendet [Zeitstempel](https://msdn.microsoft.com/en-us/library/ms182776(v=SQL.90).aspx) -Datentyp, bevor der SQL- [Rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) ersetzt. Der Typ .net für [Rowversion](https://msdn.microsoft.com/en-us/library/ms182776(v=sql.110).aspx) ist ein Bytearray.

Wenn Sie die fluent-API verwenden möchten, können Sie mithilfe der [IsConcurrencyToken](https://msdn.microsoft.com/en-us/library/gg679501(v=VS.103).aspx) Methode, um die Tracking-Eigenschaft anzugeben, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Durch Hinzufügen einer Eigenschaft wird das Datenbankmodell geändert, daher Sie einen anderen Migration ausführen müssen. Geben Sie in der Paket-Manager-Konsole (PMC) die folgenden Befehle ein:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-the-department-controller"></a>Ändern von Controller-Abteilung

In *Controllers\DepartmentController.cs*, Hinzufügen einer `using` Anweisung:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

In der *DepartmentController.cs* Datei, ändern Sie alle vier Vorkommen von "LastName" in "FullName", sodass die Abteilung Administrator Dropdown-Listen der vollständige Name der Kursleiter anstatt nur der letzte Name enthält.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Ersetzen Sie den vorhandenen Code für die `HttpPost` `Edit` -Methode durch folgenden Code:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Wenn die `FindAsync` Methode gibt null zurück, die Abteilung wurde von einem anderen Benutzer gelöscht. Dargestellten Code verwendet die übermittelte Formularwerte, um eine Entität Department erstellen, sodass die bearbeiten (Seite) mit einer Fehlermeldung angezeigt werden kann. Als Alternative wäre nicht stehen Ihnen die Entität Department neu zu erstellen, wenn Sie nur eine Fehlermeldung angezeigt, ohne erneute Anzeigen der Abteilung Felder.

Die Sicht speichert die ursprüngliche `RowVersion` Wert in ein verborgenes Feld und die Methode erhält sie in der `rowVersion` Parameter. Vor dem Aufruf `SaveChanges`, müssen Sie, die ursprüngliche put `RowVersion` Eigenschaftswert in der `OriginalValues` Auflistung für die Entität. Klicken Sie dann, wenn das Entity Framework eine SQL erstellt `UPDATE` Befehl, mit dem Befehl einschließen, wird eine `WHERE` -Klausel, die für eine Zeile sucht, die die ursprüngliche `RowVersion` Wert.

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

Schließlich der Code legt die `RowVersion` Wert, der die `Department` Objekt, das den neuen Wert aus der Datenbank abgerufen. Diese neue `RowVersion` Wert wird in das ausgeblendete Feld gespeichert werden, wenn die Seite wird erneut bearbeiten und das nächste die benutzeraufrufen Mal **speichern**, nur Parallelitätsfehlern, die auftreten, da es sich bei der erneuten Anzeige Bearbeitungsseite abgefangen wird.

In *Views\Department\Edit.cshtml*, fügen Sie ein ausgeblendetes Feld zum Speichern der `RowVersion` Eigenschaftswert, der unmittelbar hinter das ausgeblendete Feld für die `DepartmentID` Eigenschaft:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="testing-optimistic-concurrency-handling"></a>Testen die vollständige Parallelitätsbehandlung

Führen Sie den Standort aus, und klicken Sie auf **Abteilungen**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Klicken Sie mit der rechten Maustaste auf die **bearbeiten** Link für die englischen Abteilung, und wählen **in neuer Registerkarte öffnen** klicken Sie dann auf die **bearbeiten** Link für die englischen Abteilung. Die beiden Registerkarten werden dieselben Informationen angezeigt.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Ändern eines Felds in der ersten Registerkarte ' Browser ', und klicken Sie auf **speichern**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Der Browser zeigt die Indexseite mit den geänderten Wert an.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Ändern eines Felds in der zweiten Registerkarte ' Browser ', und klicken Sie auf **speichern**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Klicken Sie auf **speichern** in der zweiten Registerkarte. Sie sehen eine Fehlermeldung angezeigt:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Klicken Sie auf **speichern** erneut aus. Der in der zweiten Registerkarte eingegebene Wert wird zusammen mit den ursprünglichen Wert der Daten gespeichert, die Sie in der ersten Browser geändert haben. Die gespeicherten Werte wird angezeigt, wenn die Indexseite angezeigt wird.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualisieren die Seite "löschen"

Für die Seite "löschen" erkennt das Entity Framework Parallelitätskonflikte zurückzuführen, dass jemand die ansonsten die Abteilung auf ähnliche Weise bearbeiten. Wenn die `HttpGet` `Delete` Methode zeigt, die Bestätigung an, die Ansicht enthält die ursprüngliche `RowVersion` Wert in ein ausgeblendetes Feld. Dieser Wert dann zur Verfügung, um wird die `HttpPost` `Delete` -Methode, die aufgerufen wird, wenn der Benutzer den Löschvorgang bestätigt. Wenn Entity Framework erstellt SQL `DELETE` Befehl, er enthält eine `WHERE` -Klausel mit der ursprünglichen `RowVersion` Wert. Wenn die Ergebnisse von Befehlen in 0 Zeilen betroffen (d. h. die Zeile geändert wurde, nach die Bestätigungsseite löschen angezeigt wurde), wird eine Parallelitätsausnahme ausgelöst, und die `HttpGet Delete` -Methode aufgerufen wird und ein Kennzeichen "Fehler" auf `true` um Kostenoptionen der Bestätigungsseite mit einer Fehlermeldung. Es ist auch möglich, dass 0 Zeilen betroffen sind, da die Zeile von einem anderen Benutzer gelöscht wurde, damit in diesem Fall eine andere Fehlermeldung angezeigt wird.

In *DepartmentController.cs*, ersetzen Sie die `HttpGet` `Delete` -Methode durch folgenden Code:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

Die Methode akzeptiert einen optionalen Parameter, der angibt, ob die Seite nach einem Parallelitätsfehler wird erneut angezeigt wird. Wenn dieses Flag ist `true`, eine Fehlermeldung wird gesendet, um die Ansicht mit einem `ViewBag` Eigenschaft.

Ersetzen Sie den Code in der `HttpPost` `Delete` Methode (mit dem Namen `DeleteConfirmed`) durch den folgenden Code:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

In den scaffolded Code, den Sie soeben ersetzt, akzeptiert diese Methode nur eine Datensatz-ID:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Sie geändert haben, diesen Parameter, um eine `Department` Entitätsinstanz erstellt, indem der Modellbinder. Dadurch erhalten Sie Zugriff auf die `RowVersion` Eigenschaftswert zusätzlich zu dem Datensatzschlüssel.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

Sie haben auch den Methodennamen Aktion von geändert `DeleteConfirmed` auf `Delete`. Der scaffolded Code mit dem Namen der `HttpPost` `Delete` Methode `DeleteConfirmed` so erteilen Sie die `HttpPost` Methode eine eindeutige Signatur. (Die CLR erfordert überladene Methoden zum anderen Methodenparameter angegeben haben.) Nun, dass die Signaturen eindeutig sind, können Sie mit der MVC-Konvention einhalten und den gleichen Namen für die `HttpPost` und `HttpGet` Löschmethoden.

Wenn ein Parallelitätsfehler abgefangen wird, wird der Code erneut an die Bestätigungsseite löschen und bietet ein Flag, das darauf eine Fehlermeldung Parallelität angezeigt werden soll.

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

Klicken Sie in der zweiten Registerkarte auf **löschen**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Die Parallelität Fehlermeldung angezeigt, und die Department-Werte werden aktualisiert, was derzeit in der Datenbank ist.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Wenn Sie auf **löschen** erneut, sind Sie auf die Indexseite, der anzeigt, dass die Abteilung gelöscht wurde umgeleitet.

## <a name="summary"></a>Zusammenfassung

Dies schließt die Einführung in die Behandlung von Parallelitätskonflikten. Informationen zu anderen Methoden zum Behandeln von verschiedenen Parallelitätsszenarien finden Sie unter [optimistische Parallelität Muster](https://msdn.microsoft.com/en-us/data/jj592904) und [arbeiten mit Eigenschaftswerten](https://msdn.microsoft.com/en-us/data/jj592677) auf MSDN. Im nächste Lernprogramm veranschaulicht das Implementieren von Tabelle pro Hierarchie Vererbung für den `Instructor` und `Student` Entitäten.

Links zu anderen Entity Framework-Ressourcen finden Sie in der [ASP.NET Data Access - Ressourcen empfohlen](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Zurück](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Weiter](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
