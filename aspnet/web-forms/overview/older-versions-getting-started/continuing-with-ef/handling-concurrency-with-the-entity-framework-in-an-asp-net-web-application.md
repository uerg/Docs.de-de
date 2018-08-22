---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: Verarbeiten von Parallelität bei Entitätsframework 4.0 in eine ASP.NET 4-Webanwendung | Microsoft-Dokumentation
author: tdykstra
description: Dieser tutorialreihe erstellt in der Contoso University-Webanwendung, die von den ersten Schritten mit der Entity Framework 4.0-Tutorial-Reihe erstellt wird. ICH...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 6b10aca944a322cae252666218aee4d5a2d6ed35
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41825981"
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>Verarbeiten von Parallelität bei Entitätsframework 4.0 in eine ASP.NET 4-Webanwendung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

> Dieser tutorialreihe erstellt, in der Contoso University-Webanwendung, die erstellt wird die [erste Schritte mit Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) Tutorial-Reihe. Wenn Sie den vorherigen Tutorials wurde nicht abgeschlossen haben, als Ausgangspunkt für dieses Tutorial können Sie [Laden Sie die Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) , die Sie erstellt haben würden. Sie können auch [Laden Sie die Anwendung](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) , indem Sie die vollständige Reihe von Tutorials erstellt wird. Wenn Sie Fragen zu den Lernprogrammen haben, können Sie sie veröffentlichen das [ASP.NET Entity Framework-Forum](https://forums.asp.net/1227.aspx).


Im vorherigen Tutorial haben Sie gelernt, wie Sortieren und Filtern Daten mit der `ObjectDataSource` -Steuerelement und das Entity Framework. Dieses Tutorial Zeigt Optionen für die Behandlung von Parallelität in einer ASP.NET-Webanwendung, die das Entity Framework verwendet. Erstellen Sie eine neue Webseite, die speziell für das Aktualisieren von Büroaufgaben "Instructor". Sie müssen behandeln, Parallelitätsprobleme in dieser Seite und in den Abteilungen-Seite, die Sie zuvor erstellt haben.

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01 abgerufen wird](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>Parallelitätskonflikte

Ein Parallelitätskonflikt tritt auf, wenn ein Benutzer, einen Datensatz bearbeitet, und ein anderer Benutzer den gleichen Datensatz bearbeitet, bevor die Änderung des ersten Benutzers in die Datenbank geschrieben wird. Wenn Sie die Entity Framework nicht auf solche Konflikte zu erkennen, überschreibt Person die Datenbank das letzte Änderungen des anderen Benutzers. In vielen Anwendungen ist dieses Risiko akzeptabel ist, und Sie müssen die Anwendung so behandeln Sie Parallelitätskonflikte möglich konfigurieren. (Wenn es gibt nur wenige Benutzer bzw. wenige Updates oder ist nicht schlimm, wenn einige Änderungen überschrieben werden, die Kosten für die Programmierung für die Parallelität möglicherweise nicht Wert.) Wenn Sie nicht über Parallelitätskonflikte machen müssen, können Sie dieses Tutorial überspringen; die verbleibenden beiden Tutorials der Reihe abhängig nicht sein, die Sie in diesem Artikel erstellen.

### <a name="pessimistic-concurrency-locking"></a>Pessimistische Parallelität (Sperren)

Wenn Ihre Anwendung versehentliche Datenverluste in Parallelitätsszenarios verhindern muss, ist die Verwendung von Datenbanksperren eine Möglichkeit. Dies wird als bezeichnet *pessimistische Parallelität*. Bevor Sie zum Beispiel eine Zeile aus einer Datenbank lesen, fordern Sie eine Sperre für den schreibgeschützten Zugriff oder den Aktualisierungszugriff an. Wenn Sie eine Zeile für den Aktualisierungszugriff sperren, kann kein anderer Benutzer diese Zeile für den schreibgeschützten Zugriff oder den Aktualisierungszugriff sperren, da er eine Kopie der Daten erhalten würde, die gerade geändert werden. Wenn Sie eine Zeile für den schreibgeschützten Zugriff sperren, können andere diese Zeile ebenfalls für den schreibgeschützten Zugriff sperren, aber nicht für den Aktualisierungszugriff.

Verwalten von Sperren hat Nachteile. Es kann komplex sein, sie zu programmieren. Es erfordert erhebliche datenbankverwaltungsressourcen und kann es zu Leistungsproblemen führen, als die Anzahl der Benutzer einer Anwendung steigt (d. h. es nicht gut skalierbar). Aus diesen Gründen unterstützen nicht alle Datenbankverwaltungssysteme die pessimistische Parallelität. Bietet keine integrierte Unterstützung für das Entity Framework und in diesem Tutorial nicht angezeigt, wie er implementiert.

### <a name="optimistic-concurrency"></a>Optimistische Nebenläufigkeit

Die Alternative zur pessimistischen Parallelität ist *optimistische Parallelität*. Die Verwendung der optimistischen Parallelität bedeutet, Nebenläufigkeitskonflikte zu erlauben und entsprechend zu reagieren, wenn diese auftreten. Beispielsweise führt er die *Department.aspx* Seite Klicks die **bearbeiten** für die Abteilung Verlauf zu verknüpfen, und verringert die **Budget** Betrag vom 1,000,000.00 $ $ 125,000.00. (John eine konkurrierende Abteilung verwaltet und freizugeben, Geld für seine eigene Abteilung möchte).

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

Bevor John klickt **Update**, Jane führt die gleiche Seite, klickt auf die **bearbeiten** Link für die Verlaufs-Abteilung, und klicken Sie dann Änderungen der **Startdatum** auf 1/1/1/10/2011 Feld 1999. (Jane die Verlaufs-Abteilung verwaltet und möchte ihm Weitere Betriebszugehörigkeit zu geben.)

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John klickt **Update** zunächst Jane klickt **Update**. Janes Browser jetzt Listen der **Budget** Betrag als $1,000,000.00, aber dies ist falsch, da die Menge von John in $125,000.00 geändert wurde.

Einige der Aktionen, die Sie, in diesem Szenario ergreifen können die folgenden:

- Sie können nachverfolgen, welche Eigenschaft ein Benutzer geändert hat und nur die entsprechenden Spalten in der Datenbank aktualisieren. Im Beispielszenario würden keine Daten verloren gehen, da verschiedene Eigenschaften von zwei Benutzern aktualisiert wurden. Das nächste Mal ein Benutzer die Verlaufs-Fachbereich durchsucht, wird Ihnen 1/1/1999 und $125,000.00. 

    Dies ist das Standardverhalten in Entity Framework, und sie können die Anzahl der Konflikte, die zu Datenverlust führen können erheblich reduzieren. Dieses Verhalten jedoch vermeiden nicht Datenverlust, wenn konkurrierende Änderungen an der gleichen Eigenschaft einer Entität vorgenommen werden. Dieses Verhalten nicht darüber hinaus immer möglich; Wenn Sie einen Entitätstyp gespeicherte Prozeduren zuordnen, werden alle Eigenschaften einer Entität aktualisiert, wenn Änderungen an der Entität in der Datenbank vorgenommen werden.
- Sie können die Janes Änderungen Benutzer2s die Änderungen zu überschreiben. Nach dem Jane klickt **Update**, **Budget** Menge wird auf $1,000,000.00 zurückgesetzt. Das ist entweder ein *Client gewinnt*- oder ein *Letzter Schreiber gewinnt*-Szenario. (Der Client die Werte haben Vorrang vor im Datenspeicher neuerungen.)
- Sie können verhindern, dass Janes Änderung in der Datenbank aktualisiert wird. In der Regel würden Sie eine Fehlermeldung angezeigt, zeigen sie den aktuellen Zustand der Daten und können ihr Zugriff auf ihre Änderungen erneut ein, wenn er immer noch machen möchte. Sie konnte den Prozess weiter automatisieren, indem Sie ihre Eingabe speichern und ihrer es wieder aktiviert, ohne dass er erneut eingeben. Dieses Szenario wird *Store Wins* (Speicher gewinnt) genannt. (Die Werte des Datenspeichers haben Vorrang gegenüber den Werten, die vom Client gesendet werden).

### <a name="detecting-concurrency-conflicts"></a>Erkennen von Nebenläufigkeitskonflikten

In Entity Framework können Sie Konflikte beheben, indem Behandlung `OptimisticConcurrencyException` Ausnahmen, die vom Entity Framework ausgelöst. Entity Framework muss dazu in der Lage sein, Konflikte zu erkennen, damit es weiß, wann diese Ausnahmen ausgelöst werden sollen. Aus diesem Grund müssen Sie die Datenbank und das Datenmodell entsprechend konfigurieren. Einige der Optionen für das Aktivieren der Konflikterkennung schließen Folgendes ein:

- Enthalten Sie in der Datenbank eine Tabellenspalte, die verwendet werden kann, um zu bestimmen, wenn eine Zeile geändert wurde. Anschließend können Sie konfigurieren die Entity Framework enthält diese Spalte in der `Where` -Klausel von SQL `Update` oder `Delete` Befehle.

    Dies ist der Zweck der `Timestamp` -Spalte in der `OfficeAssignment` Tabelle.

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    Der Datentyp der `Timestamp` Spalte ist die Abkürzung `Timestamp`. Allerdings nicht für die Spalte tatsächlich einen Datum oder Uhrzeit-Wert enthalten. Stattdessen ist der Wert eine sequenzielle Zahl, die jeweils erhöht, die die Zeile aktualisiert wird. In einer `Update` oder `Delete` Befehl aus, die `Where` -Klausel enthält, die ursprüngliche `Timestamp` Wert. Wenn die zu aktualisierende Zeile durch den Wert in einen anderen Benutzer geändert wurde `Timestamp` unterscheidet sich von den ursprünglichen Wert daher `Where` die Klausel gibt keine zu aktualisierenden Zeile zurück. Wenn Entity Framework feststellt, wurden keine Zeilen von der aktuellen aktualisiert, `Update` oder `Delete` Befehl (d. h., wenn die Anzahl der betroffenen Zeilen 0 (null) ist), die als Parallelitätskonflikt interpretiert.
- Konfigurieren von Entity Framework für die ursprünglichen Werte jeder Spalte in der Tabelle sind die `Where` -Klausel der `Update` und `Delete` Befehle.

    Wie in der ersten Option, wenn etwas in der Zeile geändert hat, da die Zeile zuerst gelesen wurde die `Where` Klausel keine Zeile zum Aktualisieren, das Entity Framework als Parallelitätskonflikt interpretiert zurück. Diese Methode ist so effektiv wie die Verwendung einer `Timestamp` Feld, jedoch kann ineffizient sein. Für Datenbanktabellen, die viele Spalten aufweisen, es kann dazu führen, sehr große `Where` -Klauseln, und in einer Webanwendung kann erfordern, dass Sie mit große zustandsdatenmengen verwalten. Verwalten großer Mengen von Status kann die Anwendungsleistung beeinträchtigen, da sie Serverressourcen (z. B. der Sitzungszustand erfordert) oder auf der Webseite selbst (z. B. "Ansichtszustand") enthalten sein muss.

In diesem Tutorial fügen Sie Konflikte bezüglich vollständiger Parallelität für eine Entität, das eine Nachverfolgungseigenschaft keine Fehlerbehandlung (der `Department` Entität) und für eine Entität, die eine Nachverfolgungseigenschaft verfügt (der `OfficeAssignment` Entität).

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>Behandeln von Parallelität ohne einer Nachverfolgungseigenschaft

Zum Implementieren einer optimistischen nebenläufigkeit für die `Department` -Entität, die eine nachverfolgung keine (`Timestamp`)-Eigenschaft, werden Sie die folgenden Aufgaben ausführen:

- Ändern Sie das Datenmodell zum Aktivieren der nachverfolgung für Parallelität `Department` Entitäten.
- In der `SchoolRepository` Klasse, Ausnahmen der Handle-Parallelität in der `SaveChanges` Methode.
- In der *Departments.aspx* Seite Handle Parallelitätsausnahmen durch Anzeigen einer Meldung, die dem Benutzer gewarnt, dass die versuchten Änderungen nicht erfolgreich ausgeführt wurden. Der Benutzer kann dann finden Sie unter den aktuellen Werten und wiederholen Sie die Änderungen, wenn sie noch immer benötigt werden.

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>Aktivieren die Parallelität nachverfolgen, die im Datenmodell

Öffnen Sie in Visual Studio die Contoso University-Webanwendung, die Sie im vorherigen Tutorial dieser Reihe verwendet wurden.

Open *SchoolModel.edmx*, im Modell-Designer mit der Maustaste der `Name` -Eigenschaft in der `Department` Entität, und klicken Sie dann auf **Eigenschaften**. In der **Eigenschaften** Ändern der `ConcurrencyMode` Eigenschaft `Fixed`.

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

Gehen Sie ebenso bei anderen nicht-Primärschlüssel skalaren Eigenschaften (`Budget`, `StartDate`, und `Administrator`.) (Dies für Navigationseigenschaften ist nicht möglich.) Gibt an, dass jedes Mal, wenn Entity Framework generiert eine `Update` oder `Delete` SQL-Befehl zum Aktualisieren der `Department` Entität in der Datenbank, diese Spalten (mit ursprünglichen Werten) enthalten sein müssen der `Where` Klausel. Wenn keine Zeile gefunden wird die `Update` oder `Delete` Befehl ausgeführt wird, Entity Framework löst eine Ausnahme für die vollständige Parallelität.

Speichern Sie und schließen Sie das Datenmodell.

### <a name="handling-concurrency-exceptions-in-the-dal"></a>Behandeln von Parallelitätsausnahmen in der Datenzugriffsschicht

Open *SchoolRepository.cs* und fügen Sie die folgenden `using` -Anweisung für die `System.Data` Namespace:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

Fügen Sie die folgenden neuen `SaveChanges` -Methode, die Ausnahmen bzgl. vollständiger Parallelität behandelt:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

Wenn ein Fehler tritt auf, wenn diese Methode aufgerufen wird, werden die Eigenschaftswerte der Entität im Speicher durch den aktuellen Werten in der Datenbank ersetzt. Die Concurrency-Ausnahme erneut ausgelöst, damit die Webseite verarbeitet werden können.

In der `DeleteDepartment` und `UpdateDepartment` Methoden ersetzen die vorhandenen Aufruf von `context.SaveChanges()` mit einem Aufruf von `SaveChanges()` um die neue Methode aufzurufen.

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>Behandeln von Parallelitätsausnahmen in der Darstellungsschicht

Open *Departments.aspx* und Hinzufügen einer `OnDeleted="DepartmentsObjectDataSource_Deleted"` -Attribut auf die `DepartmentsObjectDataSource` Steuerelement. Das öffnende Tag für das Steuerelement wird jetzt im folgende Beispiel ähneln.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

In der `DepartmentsGridView` geben alle Tabellenspalten in der `DataKeyNames` Attribut, wie im folgenden Beispiel gezeigt. Beachten Sie, dass dies sehr große Ansicht Felder "Bundesstaat", erstellt die ist ein guter Grund ist, warum ein Feld für die nachverfolgung verwenden im Allgemeinen die bevorzugte Methode zum Nachverfolgen von Parallelitätskonflikten.

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

Open *Departments.aspx.cs* und fügen Sie die folgenden `using` -Anweisung für die `System.Data` Namespace:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

Fügen Sie die folgende neue Methode, die Sie aus den Datenquellen-Steuerelement die aufrufen werden `Updated` und `Deleted` -Ereignishandler für die Behandlung von Parallelitätsausnahmen:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

Dieser Code überprüft, ob der Typ der Ausnahme, und wenn es sich um eine Parallelitätsausnahme ist, erstellt des Codes dynamisch eine `CustomValidator` -Steuerelement, das wiederum eine Meldung im angezeigt, die `ValidationSummary` Steuerelement.

Rufen Sie die neue Methode über die `Updated` -Ereignishandler, die Sie zuvor hinzugefügt haben. Darüber hinaus erstellen Sie ein neues `Deleted` -Ereignishandler, der die gleiche Methode aufruft (jedoch keine anderen Vorgänge ausgeführt):

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>Testen optimistischen Parallelität in der Seite "Abteilungen".

Führen Sie die *Departments.aspx* Seite.

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

Klicken Sie auf **bearbeiten** in einer Zeile, und ändern Sie den Wert in der **Budget** Spalte. (Beachten Sie, dass Sie nur bearbeiten, können Datensätze, die Sie für dieses Tutorial erstellt haben da die vorhandenen `School` Datenbank-Datensätzen enthalten ungültigen Daten. Der Datensatz für die Wirtschaftlichkeit Abteilung ist zum Experimentieren mit einer sicheren.)

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

Öffnen Sie ein neues Browserfenster, und führen Sie die Seite erneut aus (Kopieren Sie die URL aus der ersten Browserfenster Adressfeld zum zweiten Browserfenster).

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

Klicken Sie auf **bearbeiten** in der gleichen Sie die zuvor bearbeiteten Zeile, und ändern Sie die **Budget** Wert in einen anderen.

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

Klicken Sie in der zweiten Browserfenster auf **Update**. Die **Budget** Betrag erfolgreich auf diesen neuen Wert geändert wird.

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

Klicken Sie in der ersten Browserfenster auf **Update**. Das Update schlägt fehl. Die **Budget** Betrag wird erneut mit dem Wert, Sie in der zweiten Browserfenster legen, angezeigt, und Sie sehen eine Fehlermeldung angezeigt.

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>Behandeln von Parallelität mit einer Nachverfolgungseigenschaft

Um vollständige Parallelität für eine Entität zu behandeln, die eine Nachverfolgungseigenschaft verfügt, müssen Sie die folgenden Aufgaben ausführen:

- Hinzufügen von gespeicherten Prozeduren in das Datenmodell zum Verwalten von `OfficeAssignment` Entitäten. (Eigenschaften der nachrichtenüberwachung und gespeicherte Prozeduren müssen zusammen verwendet werden; sie sind nur gruppiert hier zur Veranschaulichung.)
- Hinzufügen von CRUD-Methoden zu der DAL und die BLL für `OfficeAssignment` Entitäten, einschließlich Code zum Behandeln von Ausnahmen bzgl. vollständiger Parallelität in der DAL.
- Erstellen Sie eine Office-Zuweisungen-Webseite.
- Vollständigen Parallelität in die neue Webseite zu testen.

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>Hinzufügen von "OfficeAssignment" gespeicherte Prozeduren, in das Datenmodell

Öffnen der *SchoolModel.edmx* im Modell-Designer-Datei mit der rechten Maustaste in der Entwurfsoberfläche, und klicken Sie auf **Modell aus der Datenbank aktualisieren**. In der **hinzufügen** Registerkarte die **Datenbankobjekte auswählen** Dialogfeld erweitern Sie **gespeicherte Prozeduren** , und wählen Sie die drei `OfficeAssignment` gespeicherte Prozeduren (finden Sie unter der Befolgen Sie Screenshot), und klicken Sie dann auf **Fertig stellen**. (Diese gespeicherten Prozeduren wurden bereits in der Datenbank, wenn Sie die heruntergeladenen oder erstellten es mit einem Skript.)

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

Mit der rechten Maustaste die `OfficeAssignment` Entität, und wählen **Zuordnung der gespeicherten Prozedur**.

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

Legen Sie die **einfügen**, **Update**, und **löschen** Funktionen für die Verwendung der entsprechenden gespeicherten Prozeduren. Für die `OrigTimestamp` Parameter der `Update` funktionieren, legen Sie die **Eigenschaft** zu `Timestamp` , und wählen Sie die **Originalwert verwenden** Option.

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

Wenn Entity Framework ruft die `UpdateOfficeAssignment` gespeicherte Prozedur übergeben sie den ursprünglichen Wert von der `Timestamp` -Spalte in der `OrigTimestamp` Parameter. Die gespeicherte Prozedur verwendet diesen Parameter in der `Where` Klausel:

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

Die gespeicherte Prozedur wird auch den neuen Wert des wählt die `Timestamp` Spalte nach dem Update, damit Entity Framework halten, können die `OfficeAssignment` Entität, die im Arbeitsspeicher mit der entsprechenden Datenbankzeile synchronisiert ist.

(Beachten Sie, dass die gespeicherte Prozedur für das Löschen einer bürozuweisung kein `OrigTimestamp` Parameter. Aus diesem Grund kann nicht das Entitätsframework stellen Sie sicher, dass eine Entität nicht geändert wird, bevor Sie ihn löschen.)

Speichern Sie und schließen Sie das Datenmodell.

### <a name="adding-officeassignment-methods-to-the-dal"></a>Hinzufügen von "OfficeAssignment"-Methoden an die DAL senden

Open *ISchoolRepository.cs* und fügen Sie die folgenden CRUD-Methoden für die `OfficeAssignment` -Entitätenmenge aufgezeigt:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

Fügen Sie die folgenden neuen Methoden zum *SchoolRepository.cs*. In der `UpdateOfficeAssignment` Methode Sie aufrufen, die lokale `SaveChanges` Methode anstelle von `context.SaveChanges`.

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

Öffnen Sie im Testprojekt *MockSchoolRepository.cs* und fügen Sie die folgenden `OfficeAssignment` Sammlung und CRUD-Methoden an diesen. (Das pseudorepository, muss die Repositoryschnittstelle implementieren oder die Lösung nicht kompiliert.)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>Hinzufügen von "OfficeAssignment"-Methoden an die BLL

Öffnen Sie in dem Hauptprojekt, *SchoolBL.cs* und fügen Sie die folgenden CRUD-Methoden für die `OfficeAssignment` Entität festgelegt:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>Erstellen eine Webseite OfficeAssignments

Erstellen Sie eine neue Webseite, verwendet der *Site.Master* Masterseite, und nennen Sie sie *OfficeAssignments.aspx*. Fügen Sie das folgende Markup, das `Content` Steuerelement mit dem Namen `Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

Beachten Sie, dass in der `DataKeyNames` Attribut gibt an, das Markup der `Timestamp` Eigenschaft als auch dem Datensatzschlüssel (`InstructorID`). Angeben von Eigenschaften in der `DataKeyNames` Attribut bewirkt, dass das Steuerelement, das in den Steuerelementzustand speichern (dem Ansichtszustand ähnelt), damit die ursprünglichen Werte, die während der postback Verarbeitung verfügbar sind.

Wenn Sie speichern nicht die `Timestamp` Wert, der Entity Framework keine für die `Where` -Klausel der SQL `Update` Befehl. Daher würde nichts zum Aktualisieren gefunden werden. Daher würde Entity Framework löst eine vollständige Parallelitätsausnahme jedes Mal ein `OfficeAssignment` Entität aktualisiert wird.

Open *OfficeAssignments.aspx.cs* und fügen Sie die folgenden `using` -Anweisung für die Datenzugriffsebene:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

Fügen Sie die folgenden `Page_Init` -Methode, die Dynamic Data-Funktionalität ermöglicht. Steigern Sie die folgenden Handler für die `ObjectDataSource` des Steuerelements `Updated` Ereignis, um die Prüfung von Parallelitätsfehlern:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>Vollständigen Parallelität Testen auf der Seite OfficeAssignments

Führen Sie die *OfficeAssignments.aspx* Seite.

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

Klicken Sie auf **bearbeiten** in einer Zeile, und ändern Sie den Wert in der **Speicherort** Spalte.

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

Öffnen Sie ein neues Browserfenster, und führen Sie die Seite erneut aus (Kopieren Sie die URL aus dem ersten Browserfenster zu dem zweiten Browserfenster).

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

Klicken Sie auf **bearbeiten** in der gleichen Sie die zuvor bearbeiteten Zeile, und ändern Sie die **Speicherort** Wert in einen anderen.

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

Klicken Sie in der zweiten Browserfenster auf **Update**.

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

Wechseln Sie zu dem ersten Browserfenster, und klicken Sie auf **Update**.

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

Sie sehen eine Fehlermeldung und der **Speicherort** Wert wurde aktualisiert, um dem Wert anzuzeigen, die Sie sie in der zweiten Browserfenster geändert.

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>Behandeln von Parallelität mit dem EntityDataSource-Steuerelement

Die `EntityDataSource` -Steuerelement enthält integrierten Logik, die die Concurrency-Einstellungen im Datenmodell erkennt und behandelt Update- und delete-Vorgänge entsprechend. Aber wie bei allen Ausnahmen, müssen Sie behandeln `OptimisticConcurrencyException` Ausnahmen selbst um eine benutzerfreundliche Fehlermeldung bereitzustellen.

Als Nächstes konfigurieren Sie die *Courses.aspx* Seite (verwendet ein `EntityDataSource` Steuerelement) ermöglichen Update und delete-Operationen und eine Fehlermeldung angezeigt wird, wenn ein Parallelitätskonflikt auftritt. Die `Course` Entität verfügt nicht über eine Parallelität-Nachverfolgungsspalte, daher Sie die gleiche Methode, die Sie verwenden mit der `Department` Entität: die Werte aller nicht schlüsselbezogene Eigenschaften nachverfolgen.

Öffnen der *SchoolModel.edmx* Datei. Für die nicht schlüsselbezogene Eigenschaften der `Course` Entität (`Title`, `Credits`, und `DepartmentID`), legen die **Parallelitätsmodus** Eigenschaft `Fixed`. Speichern Sie und schließen Sie das Datenmodell.

Öffnen der *Courses.aspx* Seite, und stellen Sie die folgenden Änderungen:

- In der `CoursesEntityDataSource` Steuerelements `EnableUpdate="true"` und `EnableDelete="true"` Attribute. Das öffnende Tag für das Steuerelement sieht nun wie im folgende Beispiel:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- In der `CoursesGridView` steuern, ändern Sie die `DataKeyNames` -Attributwert auf `"CourseID,Title,Credits,DepartmentID"`. Fügen Sie dann eine `CommandField` Element, das `Columns` -Element, das zeigt, **bearbeiten** und **löschen** Schaltflächen (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`). Die `GridView` Steuerelement wird jetzt im folgende Beispiel ähnelt:

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

Führen Sie die Seite, und erstellen Sie eine Situation Konflikt aus, wie zuvor in der Seite "Abteilungen". Führen Sie die Seite in zwei Browserfenstern, klicken Sie auf **bearbeiten** in der gleichen Zeile in jedem Fenster aus, und stellen Sie jeweils eine andere Änderung. Klicken Sie auf **Update** in einem Fenster, und klicken Sie dann auf **Update** in dem anderen Fenster. Beim Klicken auf **Update** bei der zweiten Fehlerseite wird angezeigt, die, die durch eine nicht behandelte Parallelitätsausnahme entsteht.

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

Sie behandeln dieses Fehlers im sehr ähnlich wie es für die Behandlung der `ObjectDataSource` Steuerelement. Öffnen der *Courses.aspx* Seite, und klicken Sie in der `CoursesEntityDataSource` -Steuerelement, geben Sie Handler für die `Deleted` und `Updated` Ereignisse. Das öffnende Tag des Steuerelements sieht nun wie im folgende Beispiel:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

Bevor Sie die `CoursesGridView` Steuerelement, fügen Sie die folgenden `ValidationSummary` Steuerelement:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

In *Courses.aspx.cs*, Hinzufügen einer `using` -Anweisung für die `System.Data` Namespace, fügen Sie eine Methode, die überprüft, wird für Parallelitätsausnahmen, die und Hinzufügen von Ereignishandlern für die `EntityDataSource` des Steuerelements `Updated` und `Deleted`Handler. Der Code wird wie folgt aussehen:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

Der einzige Unterschied zwischen diesem Code und was hat Ihnen für die `ObjectDataSource` Steuerelements besteht darin, dass in diesem Fall die Concurrency-Ausnahme in der `Exception` Eigenschaft des Ereignisobjekts Argumente nicht in der Ausnahme `InnerException` Eigenschaft.

Führen Sie die Seite, und erstellen Sie einen Parallelitätskonflikt erneut aus. Dieses Mal wird eine Fehlermeldung angezeigt:

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

Damit ist die Einführung in die Behandlung von Nebenläufigkeitskonflikten abgeschlossen. Im nächste Tutorial bietet Anleitungen dazu, wie zur Verbesserung der Leistung in einer Webanwendung, die das Entity Framework verwendet.

> [!div class="step-by-step"]
> [Zurück](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [Weiter](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
