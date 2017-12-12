---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: "Behandeln von Parallelität mit dem Entity Framework 4.0 in eine ASP.NET 4-Webanwendung | Microsoft Docs"
author: tdykstra
description: Diese Reihe von Lernprogrammen baut auf der Contoso-University-Webanwendung, die von den ersten Schritten mit der Entity Framework 4.0 Tutorial Reihe erstellt wird. ICH...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 7bdcf610458631749531ed1279d27e90572f0371
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Behandeln von Parallelität mit dem Entity Framework 4.0 in eine ASP.NET 4-Webanwendung
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

> Diese Reihe von Lernprogrammen in der Contoso-University Webanwendung durch die erstellte builds der [erste Schritte mit dem Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) Reihe von Lernprogrammen. Wenn Sie die frühere Lernprogramme nicht abgeschlossen wurde, als Ausgangspunkt für dieses Lernprogramm können Sie [Herunterladen der Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , die Sie erstellt haben würden. Sie können auch [Herunterladen der Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , die durch das vollständige Lernprogramm Reihe erstellt wird. Wenn Sie Fragen zu den Lernprogrammen haben, können Sie stellen Sie diese auf die [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx).


Im vorherigen Lernprogramm haben Sie gelernt, wie Sortieren und Filtern Daten mithilfe der `ObjectDataSource` -Steuerelement und das Entity Framework. Dieses Lernprogramm zeigt die Optionen für das Behandeln von Parallelität in einer ASP.NET Web-Anwendung, die das Entity Framework verwendet. Erstellen Sie eine neue Webseite, die für die Aktualisierung Dozenten Office Zuweisungen vorgesehen ist. Sie müssen behandeln Parallelitätsproblemen in dieser Seite und die Abteilungen-Seite, die Sie zuvor erstellt haben.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01 abgerufen wird](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Parallelitätskonflikte

Parallelitätskonflikt tritt auf, wenn einem Benutzer, einen Datensatz bearbeitet, und ein anderer Benutzer denselben Datensatz bearbeitet, bevor die erste Änderung des Benutzers in die Datenbank geschrieben wird. Wenn Sie das Entity Framework zum Erkennen von solche Konflikten einrichten nicht, wem auch immer zur Aktualisierung der Datenbank zuletzt des anderen Benutzers werden Änderungen überschrieben. In vielen Anwendungen müssen dieses Risiko akzeptabel ist, und Sie zum Konfigurieren der Anwendung möglich Parallelitätskonflikte zu behandeln. (Wenn nur wenige Benutzer oder einige Updates vorhanden sind oder nicht wirklich wichtig, wenn einige Änderungen überschrieben werden, kann die Kosten für die Programmierung für die Parallelität die Vorteile überwiegen.) Wenn Sie benötigen, um Parallelitätskonflikte zu kümmern, können Sie dieses Lernprogramm überspringen; die verbleibenden zwei Lernprogramme in der Reihe abhängig nicht, die Sie in dieses Objekt zu erstellen.

### <a name="pessimistic-concurrency-locking"></a>Die eingeschränkte Parallelität (Sperren)

Wenn Ihre Anwendung muss in parallelitätsszenarios versehentliche Datenverluste zu verhindern, ist eine Möglichkeit dazu Datenbanksperren verwenden. Hierbei spricht *eingeschränkte Parallelität*. Bevor Sie eine Zeile aus einer Datenbank lesen, Sie anfordern, z. B. eine Sperre für nur-Lese oder Aktualisierungszugriff. Wenn Sie eine Zeile für Aktualisierungszugriff sperren, dürfen keine anderen Benutzer der Zeile, die entweder für Sperren nur-Lese oder Aktualisierungszugriff, da erhalten sie eine Kopie der Daten, die nicht gerade geändert wird. Wenn Sie eine Zeile für schreibgeschützten Zugriff gesperrt werden, können andere auch für schreibgeschützten Zugriff jedoch nicht für Update gesperrt.

Verwalten von Sperren hat einige Nachteile. Es kann zum Programm komplex sein. Erfordert erhebliche Datenbankressourcen für Verwaltung, und es kann zu Leistungsproblemen führen, als die Anzahl der Benutzer einer Anwendung erhöht (d. h. es Skalierung nicht optimal). Aus diesen Gründen unterstützen nicht alle Datenbank-Managementsystemen eingeschränkte Parallelität. Das Entity Framework stellt keine integrierte Unterstützung dafür, und dieses Lernprogramms nicht zeigen, wie sie implementiert.

### <a name="optimistic-concurrency"></a>Vollständige Parallelität

Die Alternative für die eingeschränkte Parallelität wird *vollständige Parallelität*. Vollständige Parallelität bedeutet ermöglicht Parallelitätskonflikte durchgeführt werden soll, und klicken Sie dann entsprechend reagieren, wenn dies der Fall. John führt z. B. die *Department.aspx* Seite klickt der **bearbeiten** Verknüpfen der Abteilung, Verlauf und verringert die **Budget** Betrag vom 1,000,000.00 $ $ 125,000.00. (John eine konkurrierende Abteilung verwaltet und Geld für sein eigenes Abteilung freigeben möchte.)

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Bevor John klickt **Update**, Jane führt die gleiche Seite, klickt auf die **bearbeiten** Link, um den Verlauf Abteilung und Änderungen der **Startdatum** auf 1/1/1/10/2011 Feld 1999. (Jane Verlauf-Abteilung verwaltet und ihm Weitere Betriebszugehörigkeit zu erteilen möchte.)

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John klickt **Update** zuerst, dann stellt Andrea klickt **Update**. Janes Browser jetzt Listen der **Budget** Betrag als $1,000,000.00, aber dies ist falsch, da die Menge von John in $125,000.00 geändert wurde.

Einige Aktionen, die Sie, in diesem Szenario ergreifen können umfassen Folgendes:

- Sie können Nachverfolgen von ein Benutzer geändert hat, dessen Eigenschaft und nur die entsprechenden Spalten in der Datenbank zu aktualisieren. In diesem Beispielszenario würde keine Daten verloren, weil Sie unterschiedliche Eigenschaften von zwei Benutzer aktualisiert wurden. Das nächste Mal eine Person durchsucht die Abteilung Verlauf, sehen sie, 1/1/1999 und $125,000.00. 

    Dies ist das Standardverhalten in Entity Framework, und es kann erheblich verringern Sie die Anzahl der Konflikte, die zu Datenverlusten führen kann. Allerdings nicht dieses Verhalten Datenverluste vermeiden, wenn die gleiche Eigenschaft einer Entität konkurrierende geändert werden. Dieses Verhalten nicht darüber hinaus immer möglich. Wenn Sie gespeicherte Prozeduren auf einen Entitätstyp zuordnen, werden alle Eigenschaften einer Entität aktualisiert, wenn Änderungen auf die Entität in der Datenbank vorgenommen werden.
- Sie können Janes Änderung Peters Änderung überschreiben lassen. Nachdem Andrea klickt **Update**, die **Budget** Betrag wechselt zum $1,000,000.00. Hierbei spricht einen *Client gewinnt* oder *Last in Wins* Szenario. (Der Client-Werte haben Vorrang vor was im Datenspeicher ist.)
- Sie können verhindern, dass Janes Änderung in der Datenbank aktualisiert wird. In der Regel würden Sie zeigt eine Fehlermeldung an, zeigen sie den aktuellen Zustand der Daten und ermöglicht es ihr Änderungen erneut eingeben, wenn sie noch machen möchte. Weitere konnte Sie diesen Prozess automatisieren, indem Sie ihre Eingabe speichern und ihr Gelegenheit erneut anwenden, ohne ihn erneut ein. Hierbei spricht einen *Store Wins* Szenario. (Der Datenspeicher Werte haben Vorrang vor den Werten, die vom Client gesendet.)

### <a name="detecting-concurrency-conflicts"></a>Erkennen von Konflikten bei der Parallelität

Im Entity Framework können Sie lösen von Konflikten durch behandeln `OptimisticConcurrencyException` Ausnahmen, die das Entity Framework löst. Damit Sie wissen, wann diese Ausnahmen ausgelöst werden soll, muss das Entity Framework zum Erkennen von Konflikten können. Aus diesem Grund müssen Sie die Datenbank und des Datenmodells entsprechend konfigurieren. Einige Optionen zum Aktivieren der konflikterkennung umfassen Folgendes:

- Enthalten Sie in der Datenbank eine Tabellenspalte, die verwendet werden kann, um zu bestimmen, wenn eine Zeile geändert wurde. Anschließend konfigurieren Sie das Entity Framework Einbeziehung dieser Spalte in der `Where` -Klausel der SQL `Update` oder `Delete` Befehle.

    Das ist der Zweck der `Timestamp` Spalte in der `OfficeAssignment` Tabelle.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Der Datentyp der `Timestamp` Spalte wird auch bezeichnet `Timestamp`. Die Spalte enthalten nicht jedoch tatsächlich einen Datum oder Uhrzeit-Wert. Stattdessen ist der Wert eine laufende Nummer, die jedes Mal inkrementiert wurde, die die Zeile aktualisiert wird. In einer `Update` oder `Delete` Befehl, der `Where` -Klausel enthält, die ursprüngliche `Timestamp` Wert. Wenn Sie die aktualisierte Zeile bereits durch den Wert in einen anderen Benutzer geändert wurde `Timestamp` unterscheidet sich von den ursprünglichen Wert also die `Where` -Klausel gibt keine zu aktualisierenden Zeile zurück. Wenn Entity Framework findet, dass keine Zeilen von der aktuellen aktualisiert wurden `Update` oder `Delete` Befehl (d. h., wenn die Anzahl der betroffenen Zeilen auf 0 (null) ist), die als Parallelitätskonflikt interpretiert.
- Konfigurieren Sie das Entity Framework, um die ursprünglichen Werte der jede Spalte in der Tabelle enthalten den `Where` -Klausel der `Update` und `Delete` Befehle.

    Wie in der ersten Option, wenn alle Elemente in der Zeile geändert wurde, seit die Zeile zuerst gelesen wurde die `Where` Klausel keine Zeile zu aktualisieren, der das Entity Framework als Parallelitätskonflikt interpretiert zurückgegeben. Diese Methode ist so effektiv wie mit einem `Timestamp` Feld, kann jedoch ineffizient. Für Datenbanktabellen, die viele Spalten aufweisen, es kann dazu führen, sehr große `Where` -Klausel in einer Webanwendung können ist erforderlich, große Mengen des Zustands zu verwalten. Verwalten von großen Datenmengen Zustand kann die Anwendungsleistung beeinträchtigen, da es Serverressourcen (z. B. Sitzungsstatus erfordert) oder auf der Webseite selbst (z. B. Ansichtszustand) enthalten sein muss.

In diesem Lernprogramm fügen Sie eine Fehlerbehandlung für vollständige Parallelitätskonflikte für eine Entität, die eine Nachverfolgungseigenschaft nicht (die `Department` Entität) und für eine Entität, die eine Nachverfolgungseigenschaft aufweist (der `OfficeAssignment` Entität).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Behandeln von Parallelität ohne eine Überwachung-Eigenschaft

Implementieren Sie vollständigen Parallelität für die `Department` Entität, die eine nachverfolgung besitzt (`Timestamp`)-Eigenschaft, schließen Sie die folgenden Aufgaben:

- Ändern Sie das Datenmodell zum Aktivieren der nachverfolgung für Parallelität `Department` Entitäten.
- In der `SchoolRepository` -Klasse Handle Parallelitätsausnahmen in die `SaveChanges` Methode.
- In der *Departments.aspx* Seite Handle Parallelitätsausnahmen durch Anzeigen einer Meldung für den Benutzer, die Warnung, dass die versuchte Änderung nicht erfolgreich waren. Der Benutzer kann dann finden Sie unter den aktuellen Werten und wiederholen Sie dann die Änderungen, wenn sie noch immer benötigt werden.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Aktivieren der Parallelität, die im Datenmodell nachverfolgen

Öffnen Sie in Visual Studio die Contoso University-Webanwendung, die Sie im vorherigen Lernprogramm dieser Reihe gearbeitet haben.

Open *SchoolModel.edmx*, und im Daten-Modell-Designer mit der Maustaste die `Name` Eigenschaft in der `Department` Entität, und klicken Sie dann auf **Eigenschaften**. In der **Eigenschaften** Ändern der `ConcurrencyMode` Eigenschaft `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Gehen Sie genauso für die anderen skalaren nicht primären Schlüssel-Eigenschaften (`Budget`, `StartDate`, und `Administrator`.) (Dies für Navigationseigenschaften ist nicht möglich.) Dies gibt an, dass bei jedem Entity Framework generiert einen `Update` oder `Delete` SQL-Befehl zum Aktualisieren der `Department` Entität in der Datenbank, diese Spalten (mit ursprünglichen Werten) enthalten sein müssen der `Where` Klausel. Wenn keine Zeile gefunden wird die `Update` oder `Delete` -Befehl ausführt, die das Entity Framework löst eine Ausnahme durch vollständige Parallelität.

Speichern Sie und schließen Sie das Datenmodell.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Behandeln von Parallelitätsausnahmen in der DAL

Open *SchoolRepository.cs* und fügen Sie die folgenden `using` -Anweisung für die `System.Data` Namespace:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Fügen Sie die folgenden neuen `SaveChanges` -Methode, die vollständige Parallelitätsausnahmen behandelt:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Wenn ein Parallelitätsfehler tritt auf, wenn diese Methode aufgerufen wird, werden die Eigenschaftenwerte der Entität im Speicher mit den aktuellen Werten in der Datenbank ersetzt. Parallelitätsausnahmen wird erneut ausgelöst, damit die Webseite verarbeitet werden können.

In der `DeleteDepartment` und `UpdateDepartment` Methoden, ersetzen Sie die vorhandenen Aufruf von `context.SaveChanges()` mit einem Aufruf von `SaveChanges()` um die neue Methode aufzurufen.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Behandeln von Parallelitätsausnahmen in der Darstellungsschicht

Open *Departments.aspx* und Hinzufügen einer `OnDeleted="DepartmentsObjectDataSource_Deleted"` -Attribut auf die `DepartmentsObjectDataSource` Steuerelement. Das öffnende Tag für das Steuerelement wird nun im folgende Beispiel entsprechen.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

In der `DepartmentsGridView` steuern, die alle Tabellenspalten im Angeben der `DataKeyNames` Attribut, wie im folgenden Beispiel gezeigt. Beachten Sie, dass dies sehr große Felder Zustand, erstellt die ist ein Grund ist, warum ein Feld für die nachverfolgung verwenden im Allgemeinen die bevorzugte Methode zum Nachverfolgen von Parallelitätskonflikten.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Open *Departments.aspx.cs* und fügen Sie die folgenden `using` -Anweisung für die `System.Data` Namespace:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Fügen Sie die folgende neue Methode, die Sie aus den Datenquellen-Steuerelements aufrufen, werden `Updated` und `Deleted` -Ereignishandler für das Behandeln von Parallelitätsausnahmen:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Dieser Code überprüft den Ausnahmetyp, und wenn es sich um eine Parallelitätsausnahme handelt, erstellt des Codes dynamisch eine `CustomValidator` Steuerelement, das wiederum eine Nachricht in zeigt den `ValidationSummary` Steuerelement.

Rufen Sie die neue Methode aus der `Updated` -Ereignishandler, die Sie zuvor hinzugefügt haben. Darüber hinaus erstellen Sie ein neues `Deleted` Ereignishandler, die die gleiche Methode aufruft (jedoch nicht weiter Sonstiges):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Vollständigen Parallelität Testen auf der Seite Abteilungen

Führen Sie die *Departments.aspx* Seite.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Klicken Sie auf **bearbeiten** in einer Zeile, und ändern Sie den Wert in der **Budget** Spalte. (Beachten Sie, dass Sie nur bearbeiten, können Datensätze, die Sie für dieses Lernprogramm erstellt haben da die vorhandenen `School` Datenbankdatensätze enthält einige ungültige Daten. Der Datensatz für die Wirtschaftlichkeit Abteilung ist eine sichere experimentieren.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Öffnen Sie ein neues Browserfenster, und führen Sie erneut die Seite (Kopieren Sie die URL aus der ersten Browserfenster Adressfeld auf der zweiten Browserfenster).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Klicken Sie auf **bearbeiten** in derselben Zeile Sie zuvor bearbeitet, und ändern Sie die **Budget** Wert einem anderen Element.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Klicken Sie in der zweiten Browserfenster auf **Update**. Die **Budget** Betrag erfolgreich auf diesen neuen Wert geändert wird.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Klicken Sie in der ersten Browserfenster auf **Update**. Das Update schlägt fehl. Die **Budget** Betrag wird erneut mit dem Wert, die Sie, in der zweiten Browserfenster festlegen angezeigt, und Sie sehen eine Fehlermeldung angezeigt.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Behandeln von Parallelität mithilfe einer Überwachung-Eigenschaft

Um vollständige Parallelität für eine Entität zu behandeln, die eine Nachverfolgungseigenschaft verfügt, werden Sie die folgenden Aufgaben ausführen:

- Hinzufügen von gespeicherten Prozeduren in das Datenmodell verwalten `OfficeAssignment` Entitäten. (Eigenschaften der nachrichtenüberwachung und gespeicherte Prozeduren müssen zusammen verwendet werden; sie sind nur gruppiert hier zur Veranschaulichung.)
- Hinzufügen von CRUD-Methoden der DAL und die BLL für `OfficeAssignment` Entitäten, einschließlich Code zum Behandeln von optimistischen Parallelitätsausnahmen in der DAL.
- Erstellen Sie eine Office-Zuweisungen-Webseite.
- Vollständigen Parallelität in der neuen Webseite zu testen.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Hinzufügen von OfficeAssignment gespeicherte Prozeduren in das Datenmodell

Öffnen der *SchoolModel.edmx* im Modell-Designer-Datei, mit der rechten Maustaste in der Entwurfsoberfläche, und klicken Sie auf **Modell aus der Datenbank aktualisieren**. In der **hinzufügen** auf der Registerkarte die **Datenbankobjekte auswählen** Dialogfeld erweitern Sie **gespeicherte Prozeduren** , und wählen Sie die drei `OfficeAssignment` gespeicherte Prozeduren (finden Sie unter der Befolgen Sie Screenshot), und klicken Sie dann auf **Fertig stellen**. (Diese gespeicherten Prozeduren wurden bereits in der Datenbank beim Herunterladen oder mithilfe eines Skripts erstellt.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Mit der rechten Maustaste die `OfficeAssignment` Entität, und wählen **Zuordnung der gespeicherten Prozedur**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Legen Sie die **einfügen**, **Update**, und **löschen** Funktionen für die entsprechenden gespeicherten Prozeduren. Für die `OrigTimestamp` Parameter von der `Update` funktionieren, legen Sie die **Eigenschaft** auf `Timestamp` , und wählen Sie die **ursprünglichen Wert verwenden** Option.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Wenn Entity Framework ruft die `UpdateOfficeAssignment` gespeicherte Prozedur übergeben sie den ursprünglichen Wert von der `Timestamp` Spalte in der `OrigTimestamp` Parameter. Die gespeicherte Prozedur verwendet diese Parameter in seiner `Where` Klausel:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

Die gespeicherte Prozedur wird auch den neuen Wert des wählt der `Timestamp` Spalte nach dem Update, damit Entity Framework halten, können die `OfficeAssignment` Entität, die im Arbeitsspeicher mit der entsprechenden Datenbankzeile synchronisiert ist.

(Beachten Sie, dass die gespeicherte Prozedur zum Löschen einer Office-Zuweisung kein `OrigTimestamp` Parameter. Aus diesem Grund kann das Entity Framework überprüfen, ob eine Entität nicht geändert wird, bevor Sie ihn löschen.)

Speichern Sie und schließen Sie das Datenmodell.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Hinzufügen von OfficeAssignment Methoden DAL

Open *ISchoolRepository.cs* und fügen Sie die folgenden CRUD-Methoden für die `OfficeAssignment` Entitätenmenge:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Fügen Sie die folgenden neuen Methoden zum *SchoolRepository.cs*. In der `UpdateOfficeAssignment` -Methode, rufen Sie die lokale `SaveChanges` Methode anstelle von `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Öffnen Sie im Testprojekt befindet, *MockSchoolRepository.cs* und fügen Sie die folgenden `OfficeAssignment` Auflistung und CRUD-Methoden, um es. (Das pseudorepository die Repository-Schnittstelle implementieren muss, oder die Projektmappe nicht kompiliert.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Hinzufügen von OfficeAssignment Methoden der BLL

Öffnen Sie in das Hauptprojekt *SchoolBL.cs* und fügen Sie die folgenden CRUD-Methoden für die `OfficeAssignment` Entität festgelegt:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Erstellen eine Webseite OfficeAssignments

Erstellen Sie eine neue Webseite, verwendet der *Site.Master* Masterseite, und nennen Sie sie *OfficeAssignments.aspx*. Fügen Sie das folgende Markup zum Rendern der `Content` Steuerelement namens `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Beachten Sie, dass in der `DataKeyNames` -Attribut gibt an, das Markup der `Timestamp` Eigenschaft sowie Datensatzschlüssel (`InstructorID`). Angeben von Eigenschaften in der `DataKeyNames` Attribut bewirkt, dass das Steuerelement im Steuerelementzustand speichern (die Statusansicht ähnelt), damit die ursprünglichen Werte, die während der Verarbeitung postback verfügbar sind.

Wenn Sie nicht speichern die `Timestamp` Wert, der Entity Framework keine für die `Where` -Klausel der SQL `Update` Befehl. Daher würde nichts zum Aktualisieren gefunden werden. Daher würde das Entity Framework löst eine vollständige Parallelitätsausnahme jedes Mal ein `OfficeAssignment` Entität wird aktualisiert.

Open *OfficeAssignments.aspx.cs* und fügen Sie die folgenden `using` -Anweisung für die Datenzugriffsebene:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Fügen Sie die folgenden `Page_Init` -Methode, die Dynamic Data-Funktionen kann. Auch fügen Sie den folgenden Ereignishandler für das `ObjectDataSource` des Steuerelements `Updated` Ereignis um Parallelitätsfehlern zu überprüfen:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Vollständigen Parallelität Testen auf der Seite OfficeAssignments

Führen Sie die *OfficeAssignments.aspx* Seite.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Klicken Sie auf **bearbeiten** in einer Zeile, und ändern Sie den Wert in der **Speicherort** Spalte.

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Öffnen Sie ein neues Browserfenster, und führen Sie erneut die Seite (Kopieren Sie die URL aus dem ersten Browserfenster auf den zweiten Browserfenster).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Klicken Sie auf **bearbeiten** in derselben Zeile Sie zuvor bearbeitet, und ändern Sie die **Speicherort** Wert einem anderen Element.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

Klicken Sie in der zweiten Browserfenster auf **Update**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Wechseln Sie zu den ersten Browserfenster, und klicken Sie auf **Update**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Sie sehen eine Fehlermeldung und der **Speicherort** Wert wurde aktualisiert, um den Wert anzuzeigen, es in der zweiten Browserfenster geändert.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Behandeln von Parallelität mit EntityDataSource-Steuerelements

Die `EntityDataSource` Steuerelement umfasst integrierte Logik, die die Parallelität Einstellungen im Datenmodell erkennt und behandelt Update- und delete-Operationen entsprechend. Jedoch wie bei allen Ausnahmen, müssen Sie behandeln `OptimisticConcurrencyException` Ausnahmen selbst um eine benutzerfreundliche Fehlermeldung bereitzustellen.

Als Nächstes konfigurieren Sie die *Courses.aspx* Seite (verwendet eine `EntityDataSource` Steuerelement) ermöglichen Update und delete-Operationen und eine Fehlermeldung angezeigt, wenn einem Parallelitätskonflikt. Die `Course` Entität verfügt nicht über eine Parallelität, die nachverfolgung der Spalten, daher Sie die gleiche Methode, die Sie verwenden mit der `Department` Entität: die Werte aller nicht schlüsselbezogene Eigenschaften nachverfolgen.

Öffnen der *SchoolModel.edmx* Datei. Für die nicht schlüsselbezogene Eigenschaften der der `Course` Entität (`Title`, `Credits`, und `DepartmentID`), legen die **Parallelitätsmodus** Eigenschaft `Fixed`. Speichern Sie und schließen Sie das Datenmodell.

Öffnen der *Courses.aspx* Seite und die folgenden Änderungen vornehmen:

- In der `CoursesEntityDataSource` Steuerelement, fügen `EnableUpdate="true"` und `EnableDelete="true"` Attribute. Das öffnende Tag für das entsprechende Steuerelement sieht nun wie im folgende Beispiel:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- In der `CoursesGridView` steuern, ändern Sie die `DataKeyNames` -Attributwert `"CourseID,Title,Credits,DepartmentID"`. Fügen Sie dann eine `CommandField` Element an der `Columns` Element, das zeigt **bearbeiten** und **löschen** Schaltflächen (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). Die `GridView` -Steuerelement jetzt im folgende Beispiel ähnelt:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Führen Sie die Seite, und erstellen Sie eine Konflikt Situation ein, wie zuvor in der Seite "Abteilungen". Führen Sie die Seite in zwei Browserfenstern, klicken Sie auf **bearbeiten** in der gleichen Zeile in jedem Fenster aus, und stellen Sie eine andere Änderung in jeder Kategorie. Klicken Sie auf **Update** in einem Fenster, und klicken Sie dann auf **Update** in die anderen Fenster. Beim Klicken auf **Update** bei der zweiten Fehlerseite wird angezeigt, die, die durch eine nicht behandelte Parallelitätsausnahme entsteht.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Sie behandeln dieser Fehler in einer Weise ähnelt, wie Sie es für behandelt die `ObjectDataSource` Steuerelement. Öffnen der *Courses.aspx* Seite, und klicken Sie in der `CoursesEntityDataSource` -Steuerelement, geben Sie die Handler für die `Deleted` und `Updated` Ereignisse. Das Anfangstag des Steuerelements sieht nun wie im folgende Beispiel:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Bevor Sie die `CoursesGridView` steuern, fügen Sie die folgenden `ValidationSummary` Steuerelement:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

In *Courses.aspx.cs*, Hinzufügen einer `using` -Anweisung für die `System.Data` Namespace, fügen Sie eine Methode, die überprüft für Parallelitätsausnahmen, und fügen Sie Ereignishandler für die `EntityDataSource` des Steuerelements `Updated` und `Deleted`Handler. Der Code sieht wie folgt:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

Der einzige Unterschied zwischen diesem Code und was Sie getan haben, für die `ObjectDataSource` Steuerelement ist, dass in diesem Fall Parallelitätsausnahmen in der `Exception` Eigenschaft des Ereignisobjekts Argumente nicht in diese Ausnahme `InnerException` Eigenschaft.

Führen Sie die Seite, und erstellen Sie einen Parallelitätskonflikt neu. Dieses Mal wird eine Fehlermeldung angezeigt:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Dies schließt die Einführung in die Behandlung von Parallelitätskonflikten. Die nächste Lernprogramm bieten Anweisungen zum Verbessern der Leistung in einer Web-Anwendung, die das Entity Framework verwendet.

>[!div class="step-by-step"]
[Zurück](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
[Weiter](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
