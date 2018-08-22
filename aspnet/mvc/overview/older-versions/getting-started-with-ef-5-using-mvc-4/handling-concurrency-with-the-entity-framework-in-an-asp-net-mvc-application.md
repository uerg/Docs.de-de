---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: Verarbeiten von Parallelität bei Entitätsframework in einer ASP.NET MVC-Anwendung (7 von 10) | Microsoft-Dokumentation
author: tdykstra
description: Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 07d4d673b7bb6bad6e9d8cbacbc965a60608db2a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823889"
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>Verarbeiten von Parallelität bei Entitätsframework in einer ASP.NET MVC-Anwendung (7 von 10)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Abgeschlossenes Projekt herunterladen](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 4-Anwendungen, die mit dem Entity Framework 5 Code First "und" Visual Studio 2012. Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). Sie können die tutorialreihe vom Anfang starten oder [Herunterladen eines Startprojekts für dieses Kapitel](building-the-ef5-mvc4-chapter-downloads.md) und beginnen Sie hier.
> 
> > [!NOTE] 
> > 
> > Wenn Sie auf ein Problem Sie nicht lösen stoßen, [herunterladen im Kapitel über das abgeschlossene](building-the-ef5-mvc4-chapter-downloads.md) und versuchen Sie es zum Reproduzieren des Problems an. Die Lösung des Problems finden in der Regel durch Ihren Code, den vollständigen Code vergleichen. Einige häufige Fehler und zu deren Lösung finden Sie [Fehler und Problemumgehungen.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


In den vorherigen beiden Tutorials, die Sie mit verknüpften Daten gearbeitet haben. In diesem Tutorial wird gezeigt, wie Parallelität behandelt. Sie erstellen Webseiten, die Arbeit mit der `Department` Entität und die Seiten, die bearbeiten und Löschen von `Department` Entitäten behandeln Parallelitätsfehler werden. Die folgenden Abbildungen zeigen die Seiten Index und löschen, einschließlich einiger Meldungen, die angezeigt werden, wenn ein Parallelitätskonflikt auftritt.

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>Parallelitätskonflikte

Ein Parallelitätskonflikt tritt auf, wenn ein Benutzer die Daten einer Entität anzeigt, um diese zu bearbeiten, und ein anderer Benutzer eben diese Entitätsdaten aktualisiert, bevor die Änderungen des ersten Benutzers in die Datenbank geschrieben wurden. Wenn Sie die Erkennung solcher Konflikte nicht aktivieren, überschreibt das letzte Update der Datenbank die Änderungen des anderen Benutzers. In vielen Anwendungen ist dieses Risiko akzeptabel: Wenn es nur wenige Benutzer bzw. wenige Updates gibt, oder wenn es nicht schlimm ist, dass Änderungen überschrieben werden können, ist es den Aufwand, für die Parallelität zu programmieren, möglicherweise nicht wert. In diesem Fall müssen Sie für die Anwendung keine Behandlung von Nebenläufigkeitskonflikten konfigurieren.

### <a name="pessimistic-concurrency-locking"></a>Pessimistische Parallelität (Sperren)

Wenn Ihre Anwendung versehentliche Datenverluste in Parallelitätsszenarios verhindern muss, ist die Verwendung von Datenbanksperren eine Möglichkeit. Dies wird als bezeichnet *pessimistische Parallelität*. Bevor Sie zum Beispiel eine Zeile aus einer Datenbank lesen, fordern Sie eine Sperre für den schreibgeschützten Zugriff oder den Aktualisierungszugriff an. Wenn Sie eine Zeile für den Aktualisierungszugriff sperren, kann kein anderer Benutzer diese Zeile für den schreibgeschützten Zugriff oder den Aktualisierungszugriff sperren, da er eine Kopie der Daten erhalten würde, die gerade geändert werden. Wenn Sie eine Zeile für den schreibgeschützten Zugriff sperren, können andere diese Zeile ebenfalls für den schreibgeschützten Zugriff sperren, aber nicht für den Aktualisierungszugriff.

Das Verwalten von Sperren hat Nachteile. Es kann komplex sein, sie zu programmieren. Es erfordert erhebliche datenbankverwaltungsressourcen und kann es zu Leistungsproblemen führen, als die Anzahl der Benutzer einer Anwendung steigt (d. h. es nicht gut skalierbar). Aus diesen Gründen unterstützen nicht alle Datenbankverwaltungssysteme die pessimistische Parallelität. Bietet keine integrierte Unterstützung für das Entity Framework und in diesem Tutorial nicht angezeigt, wie er implementiert.

### <a name="optimistic-concurrency"></a>Optimistische Nebenläufigkeit

Die Alternative zur pessimistischen Parallelität ist *optimistische Parallelität*. Die Verwendung der optimistischen Parallelität bedeutet, Nebenläufigkeitskonflikte zu erlauben und entsprechend zu reagieren, wenn diese auftreten. Beispielsweise führt er die Seite Abteilungen zu bearbeiten, Änderungen der **Budget** Menge für die englische Abteilung von $350.000,00 in $0,00.

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Bevor John klickt **speichern**, Andrea führt den gleichen Seite und ändert die **Startdatum** Feld 9/1/2007, 8/8/2013.

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John klickt **speichern** zuerst und erkennt, die seine ändern, wenn der Browser die Indexseite, klicken Sie dann Jane gibt klickt **speichern**. Was daraufhin geschieht, ist abhängig davon, wie Sie Nebenläufigkeitskonflikte behandeln. Einige der Optionen schließen Folgendes ein:

- Sie können nachverfolgen, welche Eigenschaft ein Benutzer geändert hat und nur die entsprechenden Spalten in der Datenbank aktualisieren. Im Beispielszenario würden keine Daten verloren gehen, da verschiedene Eigenschaften von zwei Benutzern aktualisiert wurden. Das nächste Mal eine Person den englischen Fachbereich durchsucht, die sie erhalten sowohl Johns und Jane die Änderungen, ein Datum von vor 8/8/2013 sowie ein Budget von 0 Dollar.

    Diese Methode der Aktualisierung kann die Anzahl von Konflikten reduzieren, die zu Datenverlusten führen können. Sie kann Datenverluste jedoch nicht verhindern, wenn konkurrierende Änderungen an der gleichen Eigenschaft einer Entität vorgenommen werden. Ob Entity Framework auf diese Weise funktioniert, hängt davon ab, wie Sie Ihren Aktualisierungscode implementieren. Oft ist dies in Webanwendungen nicht praktikabel, da es erforderlich sein kann, viele Zustände zu verwalten, um alle ursprünglichen Eigenschaftswerte einer Entität und die neuen Werte im Auge zu behalten. Verwalten großer Mengen von Status kann die Anwendungsleistung beeinträchtigen, da es entweder Serverressourcen beansprucht oder auf der Webseite selbst (z.B. in ausgeblendeten Feldern) enthalten sein muss.
- Sie können die Janes Änderungen Benutzer2s die Änderungen zu überschreiben. Das nächste Mal eine Person den englischen Fachbereich durchsucht, sehen sie 8/8/2013 und der wiederhergestellte Wert $350.000,00. Das ist entweder ein *Client gewinnt*- oder ein *Letzter Schreiber gewinnt*-Szenario. (Der Client die Werte haben Vorrang vor im Datenspeicher neuerungen.) Wie in der Einführung dieses Abschnitts beschrieben, geschieht dies automatisch, wenn Sie für die Behandlung der Parallelität keinen Code schreiben.
- Sie können verhindern, dass Janes Änderung in der Datenbank aktualisiert wird. In der Regel würden Sie eine Fehlermeldung angezeigt, zeigen sie den aktuellen Zustand der Daten und können ihr Zugriff auf ihre Änderungen erneut anzuwenden, wenn er immer noch machen möchte. Dieses Szenario wird *Store Wins* (Speicher gewinnt) genannt. (Die Werte des Datenspeichers haben Vorrang vor den Werten, die vom Client gesendet werden.) In diesem Tutorial implementieren Sie das Szenario „Speicher gewinnt“. Diese Methode stellt sicher, dass keine Änderung überschrieben wird, ohne dass ein Benutzer darüber benachrichtigt wird.

### <a name="detecting-concurrency-conflicts"></a>Erkennen von Nebenläufigkeitskonflikten

Sie können Konflikte auflösen, durch behandeln [OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Ausnahmen, die vom Entity Framework ausgelöst. Entity Framework muss dazu in der Lage sein, Konflikte zu erkennen, damit es weiß, wann diese Ausnahmen ausgelöst werden sollen. Aus diesem Grund müssen Sie die Datenbank und das Datenmodell entsprechend konfigurieren. Einige der Optionen für das Aktivieren der Konflikterkennung schließen Folgendes ein:

- Fügen Sie eine Änderungsverfolgungsspalte in die Datenbanktabelle ein, die verwendet werden kann, um zu bestimmen, wenn eine Änderung an einer Zeile vorgenommen wurde. Anschließend können Sie konfigurieren die Entity Framework enthält diese Spalte in der `Where` -Klausel von SQL `Update` oder `Delete` Befehle.

    Der Datentyp der änderungsverfolgungsspalte ist in der Regel [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx). Die [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) Wert ist eine sequenzielle Zahl, die jeweils erhöht die Zeile aktualisiert. In einer `Update` oder `Delete` Befehl aus, die `Where` -Klausel enthält, den ursprünglichen Wert der änderungsverfolgungsspalte (die ursprüngliche Zeilenversion). Wenn von einem anderen Benutzer, der Wert in der zu aktualisierende Zeile geändert wurde der `rowversion` Spalte unterscheidet sich von den ursprünglichen Wert daher `Update` oder `Delete` Anweisung wurde nicht gefunden zu aufgrund von zu aktualisierenden Zeile die `Where` Klausel. Wenn Entity Framework feststellt, wurden keine Zeilen von aktualisiert, die `Update` oder `Delete` Befehl (d. h., wenn die Anzahl der betroffenen Zeilen 0 (null) ist), die als Parallelitätskonflikt interpretiert.
- Konfigurieren von Entity Framework für die ursprünglichen Werte jeder Spalte in der Tabelle sind die `Where` -Klausel der `Update` und `Delete` Befehle.

    Wie in der ersten Option, wenn etwas in der Zeile geändert hat, da die Zeile zuerst gelesen wurde die `Where` Klausel keine Zeile zum Aktualisieren, das Entity Framework als Parallelitätskonflikt interpretiert zurück. Für Datenbanktabellen, die viele Spalten aufweisen. dieser Ansatz kann dazu führen, sehr große `Where` -Klauseln, und Sie können festlegen, dass Sie mit große zustandsdatenmengen verwalten. Wie bereits erwähnt, kann Verwalten großer Mengen von Status Leistung der Anwendung beeinträchtigen, da es entweder Serverressourcen beansprucht oder auf der Webseite selbst enthalten sein muss. Aus diesem Grund dieser Ansatz nicht empfehlenswert, und sie nicht die in diesem Tutorial verwendete Methode ist.

    Wenn Sie diesen Ansatz für die Parallelität implementieren möchten, müssen Sie alle nicht-primären Schlüsseleigenschaften in der Entität markieren Sie Parallelität für durch Hinzufügen von nachverfolgen möchten die [ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx) -Attributs. Dass die Änderung der Entity Framework enthalten alle Spalten in der SQL ermöglicht `WHERE` -Klausel der `UPDATE` Anweisungen.

Im weiteren Verlauf dieses Tutorials fügen Sie eine [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) tracking-Eigenschaft, um die `Department` Entität, einen Controller und Ansichten zu erstellen und testen, um sicherzustellen, dass alles ordnungsgemäß funktioniert.

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Fügen Sie eine vollständige Parallelität-Eigenschaft auf die Entität "Department"

In *Models\Department.cs*, Hinzufügen einer Nachverfolgungseigenschaft namens `RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

Die [Zeitstempel](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) Attribut gibt an, dass in dieser Spalte berücksichtigt werden die `Where` -Klausel der `Update` und `Delete` Befehle, die an die Datenbank gesendet. Das Attribut heißt [Zeitstempel](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx) da frühere Versionen von SQL Server eine SQL verwendet [Zeitstempel](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx) -Datentyp, bevor SQL [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx) ersetzt. Der Typ .net für `rowversion` ist ein Bytearray. Wenn Sie die fluent-API verwenden möchten, können Sie mithilfe der [IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx) Methode, um die änderungsverfolgungseigenschaft anzugeben, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

Durch das Hinzufügen einer Eigenschaft ändern Sie das Datenbankmodell, daher müssen Sie eine weitere Migration durchführen. Geben Sie die folgenden Befehle in die Paket-Manager-Konsole ein:

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>Erstellen Sie einen Controller für die Abteilung

Erstellen Sie eine `Department` Controller und Ansichten, die genau wie die anderen Controller, mit den folgenden Einstellungen:

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

In *Controllers\DepartmentController.cs*, Hinzufügen einer `using` Anweisung:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

Ändern Sie "LastName", "FullName" überall in dieser Datei (vier Vorkommen) enthält, damit die Abteilung Administrator Dropdownlisten der vollständige Name des Dozenten und nicht nur der letzte Name.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

Ersetzen Sie den Code für die `HttpPost` `Edit` Methode durch den folgenden Code:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

Die Ansicht speichert die ursprüngliche `RowVersion` Wert in einem verborgenen Feld. Wenn die modellbindung erstellt die `department` Instanz dieses Objekt hat die ursprüngliche `RowVersion` Eigenschaftswert und die neuen Werte für die anderen Eigenschaften, wie der Benutzer auf der Seite "Bearbeiten" eingegeben haben. Klicken Sie dann, wenn Entity Framework SQL erstellt `UPDATE` Befehl ",", mit dem Befehl enthält eine `WHERE` -Klausel, die für eine Zeile sucht mit dem ursprünglichen `RowVersion` Wert.

Wenn keine Zeilen betroffen sind der `UPDATE` Befehl (keine Zeile enthält die ursprünglichen `RowVersion` Wert), löst Entity Framework eine `DbUpdateConcurrencyException` Ausnahme und der Code in die `catch` Block Ruft die betroffene `Department` Entität aus der Ausnahme -Objekt. Diese Entität hat sowohl die Werte aus der Datenbank gelesen und die neuen Werte, die vom Benutzer eingegeben werden:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

Als Nächstes fügt der Code eine benutzerdefinierte Fehlermeldung für jede Spalte, die Datenbankwerte sich von der Eingabe des Benutzers auf der Seite "Bearbeiten" hinzu:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Eine längere Fehlermeldung wird erläutert, was passiert ist und was Sie tun:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

Mit dem Code Abschließend wird die `RowVersion` Wert, der die `Department` Objekt, das den neuen Wert aus der Datenbank abgerufen. Dieser neue `RowVersion`-Wert wird in dem ausgeblendeten Feld gespeichert, wenn die Seite „Bearbeiten“ erneut angezeigt wird. Das nächste Mal, wenn der Benutzer auf **Speichern** klickt, werden nur Parallelitätsfehler abgefangen, die nach dem erneuten Anzeigen der Seite „Bearbeiten“ aufgetreten sind.

In *Views\Department\Edit.cshtml*, fügen Sie ein ausgeblendetes Feld zum Speichern der `RowVersion` Eigenschaftswert, sofort nach dem ausgeblendeten Feld für die `DepartmentID` Eigenschaft:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

In *Views\Department\Index.cshtml*, ersetzen Sie den vorhandenen Code durch den folgenden Code zum Verschieben von Links der Zeile auf der linken Seite und ändern die Seite Titel und die Spaltenüberschriften angezeigt `FullName` anstelle von `LastName` in die **Administrator** Spalte:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>Testen der Behandlung von Parallelität

Führen Sie die Website, und klicken Sie auf **Abteilungen**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

Klicken Sie mit der rechten Maustaste auf die **bearbeiten** Hyperlink für Kim Abercrombie, und wählen **in neuer Registerkarte öffnen** klicken Sie dann auf die **bearbeiten** Hyperlink für Kim Abercrombie. Die beiden Fenstern zeigen die gleichen Informationen.

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Ändern Sie ein Feld in der ersten Browserfenster, und klicken Sie auf **speichern**.

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

Der Browser zeigt die Indexseite mit dem geänderten Wert an.

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

Ändern Sie den jedes Feld in der zweiten Browserfenster, und klicken Sie auf **speichern**.

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

Klicken Sie auf **speichern** im zweiten Browserfenster. Folgende Fehlermeldung wird angezeigt:

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

Klicken Sie erneut auf **Speichern**. Der Wert in der zweiten Browser eingegebene wird zusammen mit den ursprünglichen Wert der Daten gespeichert, die Sie in der ersten Browser zu ändern. Die gespeicherten Werte werden Ihnen auf der Indexseite angezeigt.

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>Aktualisieren die Seite "löschen"

Bei der Seite „Löschen“ entdeckt Entity Framework Nebenläufigkeitskonflikte, die durch die Bearbeitung einer Abteilung ausgelöst wurden, auf ähnliche Weise. Wenn die `HttpGet` `Delete` Methode bestätigungsansicht anzeigt, die Ansicht enthält die ursprüngliche `RowVersion` Wert in einem verborgenen Feld. Dieser Wert dann zur Verfügung, um wird die `HttpPost` `Delete` -Methode, die aufgerufen wird, wenn der Benutzer das Löschen bestätigt. Wenn Entity Framework erstellt SQL `DELETE` Befehl, er enthält eine `WHERE` Klausel mit dem ursprünglichen `RowVersion` Wert. Wenn die Ergebnisse des Befehls in 0 (null) Zeilen betroffen (d. h. die Zeile wurde geändert, nachdem die Bestätigungsseite "löschen" angezeigt wurde), wird eine Parallelitätsausnahme ausgelöst, und die `HttpGet Delete` Methode wird aufgerufen, mit dem Fehler Kennzeichen festgelegt `true` um Kostenoptionen der Bestätigungsseite mit einer Fehlermeldung. Es ist auch möglich, dass keine Zeilen betroffen sind, da die Zeile von einem anderen Benutzer gelöscht wurde, sodass in diesem Fall eine andere Fehlermeldung angezeigt wird.

In *DepartmentController.cs*, ersetzen Sie die `HttpGet` `Delete` Methode durch den folgenden Code:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

Die Methode akzeptiert einen optionalen Parameter, der angibt, ob die Seite nach einem Parallelitätsfehler erneut angezeigt wird. Wenn dieses Flag ist `true`, eine Fehlermeldung wird gesendet, um die Ansicht mit einem `ViewBag` Eigenschaft.

Ersetzen Sie den Code in die `HttpPost` `Delete` Methode (mit dem Namen `DeleteConfirmed`) durch den folgenden Code:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

In dem eingerüsteten Code, den Sie soeben ersetzt haben, akzeptiert diese Methode nur eine Datensatz-ID:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

Du hast geändert, diesen Parameter, um eine `Department` Entitätsinstanz, die von der modellbindung erstellt wurde. So erhalten Sie Zugriff auf die `RowVersion` neben dem Datensatzschlüssel auch-Eigenschaftswert.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

Ebenfalls haben Sie den Namen der Aktionsmethode `DeleteConfirmed` auf `Delete` geändert. Der eingerüstete Code, der mit dem Namen der `HttpPost` `Delete` Methode `DeleteConfirmed` gerne die `HttpPost` Methode eine eindeutige Signatur. (Die CLR erfordert überladene Methoden, um verschiedene Methodenparameter zu enthalten.) Nun, dass die Signaturen eindeutig sind, können Sie mit der MVC-Konvention halten und den gleichen Namen für die `HttpPost` und `HttpGet` delete-Methoden.

Wenn ein Parallelitätsfehler abgefangen wird, zeigt der Code erneut die Bestätigungsseite „Löschen“ an, und stellt ein Flag bereit, das angibt, dass eine Fehlermeldung für die Parallelität angezeigt werden soll.

In *Views\Department\Delete.cshtml*, ersetzen Sie dies durch den folgenden Code, mit der Formatierung, der eingerüstete Code ändert, und fügt ein Nachrichtenfeld Fehler. Die Änderungen werden hervorgehoben.

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

Dieser Code Fügt eine Fehlermeldung zwischen der `h2` und `h3` Überschriften:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

Es ersetzt `LastName` mit `FullName` in die `Administrator` Feld:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

Schließlich fügt sie ausgeblendete Felder für die `DepartmentID` und `RowVersion` Eigenschaften nach der `Html.BeginForm` Anweisung:

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

Führen Sie die Indexseite "Abteilungen". Klicken Sie mit der rechten Maustaste auf die **löschen** Link für den englischen Fachbereich, und wählen **in neuem Fenster öffnen** im ersten Fenster klicken Sie dann auf die **bearbeiten** Link für die englische Abteilung.

Im ersten Fenster einen der Werte ändern, und klicken Sie auf **speichern** :

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

Die Indexseite wird die Änderung bestätigt.

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

Klicken Sie im zweiten Fenster auf **löschen**.

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

Ihnen wird eine Fehlermeldung zur Parallelität angezeigt, und die Abteilungswerte werden mit den aktuellen Werten der Datenbank aktualisiert.

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

Wenn Sie erneut auf **Löschen** klicken, werden Sie auf die Indexseite weitergeleitet, die anzeigt, dass die Abteilung gelöscht wurde.

## <a name="summary"></a>Zusammenfassung

Damit ist die Einführung in die Behandlung von Nebenläufigkeitskonflikten abgeschlossen. Weitere Informationen über andere Möglichkeiten, verschiedene Szenarien zu behandeln, finden Sie unter [optimistische Parallelitätsmuster](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx) und [arbeiten mit Eigenschaftswerten](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx) im Entity Framework-Team-Blog. Das nächste Tutorial zeigt, wie Sie Tabelle pro Hierarchie-Vererbung für implementieren die `Instructor` und `Student` Entitäten.

Links zu anderen Ressourcen des Entity Framework finden Sie in der [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).

> [!div class="step-by-step"]
> [Zurück](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [Weiter](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
