---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: Async und gespeicherten Prozeduren mit dem Entity Framework in einer ASP.NET MVC-Anwendung | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 7412b32ac29179dfa319544781d4c7165c58196b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="async-and-stored-procedures-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Async und gespeicherten Prozeduren mit dem Entity Framework in einer ASP.NET MVC-Anwendung
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio 2013. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


In früheren Lernprogrammen haben Sie gelernt, lesen und Aktualisieren von Daten mit dem synchronen Programmiermodell. In diesem Lernprogramm erfahren Sie, wie das asynchrone Programmiermodell implementieren. Asynchronem Code können auf eine Anwendung, die eine bessere Leistung ausgeführt werden, da es eine bessere Verwendung von Server-Ressourcen ermöglicht.

In diesem Lernprogramm wird auch zum Verwenden von gespeicherter Prozeduren für INSERT-, Update- und Delete-Operationen für eine Entität angezeigt werden.

Schließlich müssen Sie die Anwendung in Azure, zusammen mit allen Änderungen an der Datenbank erneut bereitstellen, die Sie seit der ersten Ausführung implementiert haben, die Sie bereitgestellt haben.

Die folgenden Abbildungen zeigen einige Seiten, denen Sie verwenden müssen.

![Seite "Abteilung"](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Abteilung erstellen](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="why-bother-with-asynchronous-code"></a>Warum kümmern Sie asynchronen code

Ein Webserver verfügt über eine begrenzte Anzahl von Threads zur Verfügung stehen, und in Situationen mit hoher Last möglicherweise alle verfügbaren Threads verwendet. In diesem Fall kann der Server neue Anforderungen nicht verarbeiten, bis die Threads freigegeben werden. Mit synchroner Code möglicherweise viele Threads einrichten gebunden werden, während sie tatsächlich alle Aufgaben durchführt werden nicht, da für e/a auf Abschluss warten, können sie. Wenn ein Prozess für e/a auf den Abschluss wartet, wird der Thread mit asynchronen Code für den Server für die Verarbeitung anderer Anforderungen verwenden freigegeben. Folglich kann asynchronen Code Serverressourcen effizienter und der Server ist aktiviert, um weitere ohne Verzögerungen-Datenverkehr zu bewältigen.

In früheren Versionen von .NET schreiben und Testen asynchronen Code war komplex, fehleranfällig und schwierig zu debuggen. In .NET 4.5 ist schreiben, testen und Debuggen asynchronen Code sehr viel einfacher, dass Sie in der Regel asynchronen Code schreiben soll, es sei denn, Sie einen Grund nicht für haben. Asynchronen Code eine kleine Menge an Mehraufwand einschleusen, aber die Leistungseinbußen bei unerheblich, während für Situationen mit hohem Datenverkehr ist mit niedrigem Datenverkehr Situationen möglicher Leistungssteigerungen erhebliche ist.

Weitere Informationen zur asynchronen Programmierung finden Sie unter [verwenden .NET 4.5 asynchrone Unterstützung, um zu vermeiden, Aufrufe blockiert](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-the-department-controller"></a>Erstellen Sie die Department-controller

Erstellen Sie einen Abteilung-Controller, wählen Sie die gleiche Weise wie die früheren Controller, mit Ausnahme von dieser Zeit haben, die **verwenden asynchrone Controller** Aktionen Kontrollkästchen.

![Abteilung Controller Gerüst](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

Die folgenden Merkmale anzeigen können, die synchronem Code für hinzugefügt wurde die `Index` Methode, um asynchrone erleichtern:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Vier Änderungen wurden angewendet, um die Entity Framework-Datenbankabfrage asynchron auszuführende zu aktivieren:

- Die Methode wird gekennzeichnet, mit der `async` Schlüsselwort, das der Compiler, um Rückrufe für Teile des Methodentextes zu generieren und zum automatischen Erstellen von gibt die `Task<ActionResult>` -Objekt, das zurückgegeben wird.
- Der Rückgabetyp geändert wurde `ActionResult` auf `Task<ActionResult>`. Die `Task<T>` Typ stellt derzeit ausgeführte Arbeit mit einem Ergebnis vom Typ `T`.
- Die `await` -Schlüsselwort auf den Aufruf des Webdiensts angewendet wurde. Wenn der Compiler dieses Schlüsselwort sieht, teilt hinter den Kulissen er die Methode in zwei Teile. Der erste Teil endet mit dem Vorgang, der asynchron gestartet wird. Der zweite Teil ist eine Rückrufmethode abgelegt, die aufgerufen wird, wenn der Vorgang abgeschlossen ist.
- Die asynchrone Version von den `ToList` Erweiterungsmethode aufgerufen wurde.

Warum ist die `departments.ToList` -Anweisung geändert, aber nicht die `departments = db.Departments` Anweisung? Der Grund ist, dass nur die Anweisungen, die dazu führen, dass Abfragen oder Befehle an die Datenbank gesendet werden asynchron ausgeführt werden. Die `departments = db.Departments` Anweisung richtet eine Abfrage, aber die Abfrage wird nicht ausgeführt, bis die `ToList` -Methode aufgerufen wird. Daher nur die `ToList` Methode asynchron ausgeführt wird.

In der `Details` Methode und die `HttpGet` `Edit` und `Delete` Methoden, die `Find` Methode ist dasjenige, das bewirkt, dass eine Abfrage auf die Datenbank gesendet werden, damit die Methode ist, die asynchron ausgeführt wird:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

In der `Create`, `HttpPost Edit`, und `DeleteConfirmed` Methoden, ist die `SaveChanges` -Methodenaufruf, der bewirkt, dass einen Befehl ausgeführt wird, nicht-Anweisungen wie z. B. `db.Departments.Add(department)` nur führen die Entitäten im Arbeitsspeicher geändert werden.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Open *Views\Department\Index.cshtml*, und Ersetzen Sie den Code durch den folgenden Code:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Dieser Code ändert den Titel aus dem Index, Abteilungen, verschiebt der Administratorname nach rechts, und stellt den vollständigen Namen des Administrators.

Erstellen, löschen, Details, und Bearbeiten von Ansichten, ändern Sie die Beschriftung für das `InstructorID` Feld "Administrator" auf die gleiche Weise wie Sie das Namensfeld Abteilung "Abteilung" in den Kurs Ansichten geändert.

Verwenden Sie Sichten in das Erstellen und Bearbeiten von den folgenden Code:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

Verwenden Sie in den Ansichten löschen und die Details des folgenden Codes ein:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Führen Sie die Anwendung, und klicken Sie auf die **Abteilungen** Registerkarte.

![Seite "Abteilung"](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

Alles funktioniert genauso wie bei anderen Controller, aber in diesem Controller alle SQL-Abfragen asynchron ausgeführt werden.

Einige Punkte zu beachten, wenn Sie asynchrone Programmierung mit dem Entity Framework verwenden:

- Die Async-Code ist nicht threadsicher. In anderen Worten heißt, versuchen Sie nicht für mehrere Vorgänge parallel verwenden dieselbe Kontextinstanz.
- Wenn Sie die Leistungsvorteile von asynchronem Code nutzen, stellen Sie sicher, dass jede Bibliothek von Paketen möchten Sie verwenden (z. B. für die Auslagerungsdatei), asynchrone auch verwenden, wenn sie keine Entity Framework-Methoden aufrufen, die dazu führen, dass Abfragen an die Datenbank gesendet werden.

## <a name="use-stored-procedures-for-inserting-updating-and-deleting"></a>Verwenden Sie gespeicherte Prozeduren zum Einfügen, aktualisieren und löschen

Einige Entwickler und Datenbankadministratoren möchten gespeicherte Prozeduren für den Datenbankzugriff verwenden. In früheren Versionen von Entity Framework können Sie Daten mithilfe einer gespeicherten Prozedur durch Abrufen [Ausführen einer unformatierten SQL-Abfrage](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), aber Sie können nicht zum Verwenden gespeicherter Prozeduren für Updatevorgänge EF anweisen. In EF 6 ist es einfach zu Code First zur Verwendung gespeicherter Prozeduren konfigurieren.

1. In *DAL\SchoolContext.cs*, den hervorgehobenen Code zum Hinzufügen der `OnModelCreating` Methode.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Dieser Code weist Entity Framework verwenden Sie gespeicherte Prozeduren für Einfüge-, update und delete-Operationen auf der `Department` Entität.
2. Geben Sie im Paket Konsole verwalten den folgenden Befehl aus:

    `add-migration DepartmentSP`

    Open *Migrationen\&Lt; Zeitstempel&gt;\_DepartmentSP.cs* sehen Sie den Code in die `Up` Methode zum Einfügen, aktualisieren und Löschen gespeicherter Prozeduren:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
- Geben Sie im Paket Konsole verwalten den folgenden Befehl aus:

    `update-database`
- Führen Sie die Anwendung im Debugmodus befindet, klicken Sie auf die **Abteilungen** Registerkarte, und klicken Sie dann auf **neu erstellen**.
- Geben Sie Daten für eine Abteilung ein, und klicken Sie dann auf **erstellen**.

    ![Abteilung erstellen](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)
- In Visual Studio sehen Sie sich die Protokolle in der **Ausgabe** Fenster zu sehen, dass eine gespeicherte Prozedur zum Einfügen der neuen Zeile der Abteilung verwendet wurde.

    ![Abteilung Insert SP](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Code wird zuerst Standardnamen gespeicherte Prozedur erstellt. Wenn Sie eine vorhandene Datenbank verwenden, müssen Sie die Namen der gespeicherten Prozedur anpassen, um gespeicherte Prozeduren, die bereits in der Datenbank definierten verwenden. Informationen hierzu finden Sie unter [Entity Framework Code erste Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).

Wenn Sie anpassen, welche gespeicherten Prozeduren werden generiert werden sollen, können Sie die scaffolded Code für die Migrationen bearbeiten `Up` -Methode, die gespeicherte Prozedur erstellt. Auf diese Weise werden die Änderungen bei jedem wiedergegeben, dass die Migration wird ausgeführt, und gelten für die Produktionsdatenbank bei Migrationen wird automatisch in der Produktion nach der Bereitstellung ausgeführt werden.

Wenn Sie eine vorhandene gespeicherte Prozedur, die in eine vorherige Migration erstellt wurde, ändern möchten, können Sie mithilfe des Befehls Add-Migration um eine leere Migration zu generieren und dann manuell Code schreiben, aufgerufen der [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) Methode .

## <a name="deploy-to-azure"></a>In Azure bereitstellen

In diesem Abschnitt müssen Sie das optionale abgeschlossener **Bereitstellen der app in Azure** im Abschnitt der [Migration und Bereitstellung](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) Lernprogramm dieser Reihe. Wenn Sie Migrationen Fehler, die Sie aufgetreten durch Löschen der Datenbank in Ihrem lokalen Projekt behoben, überspringen Sie diesen Abschnitt.

1. In Visual Studio mit der Maustaste des Projekts im **Projektmappen-Explorer** , und wählen Sie **veröffentlichen** aus dem Kontextmenü.
2. Klicken Sie auf **Veröffentlichen**.

    Visual Studio wird die Anwendung in Azure bereitgestellt und die Anwendung geöffnet, in Ihrem Standardbrowser in Azure ausgeführt wird.
3. Testen der Anwendung, überprüfen Sie, ob sie funktioniert.

    Beim ersten einer Seite ausführen, auf die Datenbank zugreift, wird das Entity Framework ausgeführt aller die Migrationen `Up` Methoden erforderlich, um die Datenbank mit dem aktuellen Datenmodell auf dem neuesten Stand zu bringen. Sie können jetzt alle Web-Seiten verwenden, die Sie seit der letzten Ausführung, die Sie bereitgestellt haben hinzugefügt, einschließlich der Abteilung-Seiten, die Sie in diesem Lernprogramm hinzugefügt.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie gesehen zum Server-Effizienz verbessern, indem Sie Code schreiben, der asynchron ausgeführt wird und wie mit gespeicherten Prozeduren zum Einfügen, aktualisieren und delete-Operationen. In den nächsten Lernprogrammen sehen Sie, wie Sie Datenverluste zu vermeiden, wenn mehrere Benutzer versuchen, denselben Datensatz zur gleichen Zeit zu bearbeiten.

Links zu anderen Entity Framework-Ressourcen finden Sie in der [ASP.NET Data Access - Ressourcen empfohlen](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Zurück](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Weiter](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
