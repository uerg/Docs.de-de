---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
title: Verbindungsresilienz und Abfangen der Befehl mit dem Entity Framework in einer ASP.NET MVC-Anwendung | Microsoft Docs
author: tdykstra
description: Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/13/2015
ms.topic: article
ms.assetid: c89d809f-6c65-4425-a3fa-c9f6e8ac89f2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1a28284e203904cc943e5e46b369e8a58ea5c820
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="connection-resiliency-and-command-interception-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Verbindungsresilienz und Abfangen der Befehl mit dem Entity Framework in einer ASP.NET MVC-Anwendung
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Herunterladen des abgeschlossenen Projekts](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) oder [PDF herunterladen](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Die Contoso-University Beispielwebanwendung veranschaulicht, wie ASP.NET MVC 5-Anwendungen, die mit dem Entity Framework 6 Code First und Visual Studio 2013. Informationen über die Reihe von Lernprogrammen finden Sie unter [im ersten Lernprogramm, in der Reihe](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).


Bisher hat die Anwendung lokal in IIS Express auf dem Entwicklungscomputer ausgeführt wurde. Um einer realen Anwendung für andere Benutzer über das Internet verfügbar zu machen, müssen Sie es auf einen Webhostinganbieter bereitstellen, und müssen Sie die Datenbank mit einem Datenbankserver bereitstellen.

In diesem Lernprogramm erfahren Sie, wie zwei Eigenschaften von Entity Framework 6 zu verwenden, besonders nützlich sind, wenn Sie in der Cloud-Umgebung bereitstellen: verbindungsstabilität (automatische Wiederholungen für flüchtige Fehler) und dem Befehl abfangen (Catch alle SQL-Abfragen gesendet, um die Datenbank aus, um die melden oder geändert werden).

In diesem Lernprogramm Verbindung resilienz und Befehl abfangen ist optional. Wenn Sie dieses Lernprogramm überspringen, müssen einige kleinere Korrekturen in den nachfolgenden Lernprogrammen vorgenommen werden.

## <a name="enable-connection-resiliency"></a>Verbindungsresilienz aktivieren

Wenn Sie die Anwendung in Windows Azure bereitstellen, stellen Sie die Datenbank auf Windows Azure SQL-Datenbank einen Datenbank-Cloud-Dienst bereit. Vorübergehende Verbindungsfehler sind in der Regel häufiger, beim Herstellen der Verbindung mit einem Clouddienst für die Datenbank oder Ihrem Webserver und dem Datenbankserver direkt zusammen im selben Rechenzentrum angeschlossen sind. Auch wenn ein Cloud-Web-Server und einen Clouddienst für die Datenbank im selben Rechenzentrum gehostet werden, stehen weitere Netzwerkverbindungen zwischen ihnen, die Probleme, z. B. Lastenausgleichsmodule aufweisen können.

Auch ist ein Cloud-Dienst in der Regel gemeinsam mit anderen Benutzern, was bedeutet, dass die Reaktionsfähigkeit von ihnen betroffen sein kann. Und der Zugriff auf die Datenbank möglicherweise unterliegen die Einschränkung. Einschränkung bedeutet, dass löst der Datenbankdienst Ausnahmen aus, wenn Sie versuchen, darauf Weitere häufig zugreifen, als in Ihrer (Service Level Agreement, SLA) zulässig ist.

Viele oder die meisten Verbindungsprobleme, wenn Sie einen Cloud-Dienst zugreifen, sind vorübergehend, d. h. sie aufgelöst werden, selbst innerhalb kurzer Zeit. Also wenn Sie versuchen Sie es einer Datenbank und einen Typ des Fehlers, der in der Regel vorübergehend ist, können Sie den Vorgang versuchen, nach kurze Zeit warten, und der Vorgang erfolgreich ausgeführt werden kann. Sie können eine bessere navigationsmöglichkeiten für Ihre Benutzer bereitstellen, wenn vorübergehende Fehler zu behandeln, automatisch versucht erneut, die meisten von ihnen an den Kunden unsichtbar machen. Das verbindungsstabilitätsfeature in Entity Framework 6 automatisiert werden, dass der Vorgang einer wiederholten SQL-Abfragen ist fehlgeschlagen.

Das verbindungsstabilitätsfeature muss für eine bestimmte Datenbank-Dienst ordnungsgemäß konfiguriert werden:

- Er muss wissen, welche Ausnahmen nur vorübergehend aufzutreten wahrscheinlich sind. Fehler aufgrund einer vorübergehenden Dienstausfall Netzwerkkonnektivität, nicht vom Programmfehler verursacht hat, z. B. Fehler wiederholt werden sollen.
- Es muss eine entsprechende Menge an Zeit zwischen einer Wiederholung eines fehlgeschlagenen Vorgangs warten. Sie können mehr zwischen den Wiederholungsversuchen für einen Batchprozess warten, als Sie für eine online-Webseite können, in denen ein Benutzer auf eine Antwort wartet.
- Er verfügt über eine entsprechende Anzahl von Zyklen, bevor der Vorgang abgebrochen wird wiederholt. Sie möchten möglicherweise mehrere Male in einem Batchprozess wiederholen, die in einer online-Anwendung aus.

Sie können konfigurieren, dass diese Einstellungen manuell für jede datenbankumgebung, die von einem Anbieter für Entity Framework unterstützt, aber die Standardwerte aufgeführt, die in der Regel für eine Anwendung online arbeiten, die Windows Azure SQL-Datenbank verwendet wurden, bereits konfiguriert und Dies sind die Einstellungen, die Sie für die Contoso-University Anwendung implementieren.

Müssen Sie lediglich ausführen, um resilienz von Verbindungen zu aktivieren ist eine Klasse erstellen, in der Assembly, die von abgeleitet ist die [DbConfiguration](https://msdn.microsoft.com/data/jj680699.aspx) Klasse, und legen Sie die SQL-Datenbank in dieser Klasse *Ausführungsstrategie*, also in EF eine andere Bezeichnung für *wiederholungsrichtlinie*.

1. Fügen Sie eine Klassendatei mit dem Namen im Ordner "DAL" *SchoolConfiguration.cs*.
2. Ersetzen Sie den Vorlagencode, durch den folgenden Code:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

    Das Entity Framework automatisch ausgeführt wird, den Code in einer Klasse, die abgeleitet werden gefundenen `DbConfiguration`. Können Sie die `DbConfiguration` Klasse Konfigurationsaufgaben in Code ausführen, die Sie andernfalls würde in die *"Web.config"* Datei. Weitere Informationen finden Sie unter [EntityFramework codebasierte Konfiguration](https://msdn.microsoft.com/data/jj680699).
3. In *StudentController.cs*, Hinzufügen einer `using` -Anweisung für `System.Data.Entity.Infrastructure`.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]
4. Ändern Sie alle von der `catch` blockiert, Catch `DataException` Ausnahmen, damit sie catch `RetryLimitExceededException` Ausnahmen stattdessen. Zum Beispiel:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=1)]

    Verwenden Sie `DataException` zu dem Versuch, den Fehler zu identifizieren, die möglicherweise vorübergehender damit Geben Sie eine Meldung aus "Wiederholen". Aber jetzt, dass Sie eine wiederholungsrichtlinie aktiviert haben, wird die eigentliche Ausnahme zurückgegebene umschlossen werden nur vorübergehend aufzutreten wahrscheinlich nur Fehler werden bereits ausprobiert und wurden Fehler mehrere Male und die `RetryLimitExceededException` Ausnahme.

Weitere Informationen finden Sie unter [Entity Framework Connection Resiliency / Wiederholungslogik](https://msdn.microsoft.com/data/dn456835).

## <a name="enable-command-interception"></a>Befehl abfangen aktivieren

Nun, dass Sie eine wiederholungsrichtlinie aktiviert haben, führen Sie wie Sie testen, stellen Sie sicher, dass er wie erwartet funktioniert? Es ist nicht so einfach um einen vorübergehenden Fehler auftreten, insbesondere wenn Sie lokal ausführen und es wäre besonders schwierig zum Integrieren von tatsächlichen vorübergehende Fehler in eine automatisierte Komponententests zu erzwingen. Um das verbindungsstabilitätsfeature zu testen, benötigen Sie eine Möglichkeit zum Abfangen von Abfragen, die Entity Framework mit SQL Server sendet, und Ersetzen Sie die SQL Server-Antwort durch einen Ausnahmetyp aus, der in der Regel flüchtig ist.

Sie können auch Abfangen der Abfrage verwenden, um eine bewährte Methode für Cloudanwendungen zu implementieren: [melden Sie sich die Latenz und Erfolg oder Fehler aller Aufrufe externer Dienste](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) z. B. Datenbankdienste. EF6 bietet eine [dedizierte Protokollierung API](https://msdn.microsoft.com/data/dn469464) , können erleichtern soll, Protokollierung, aber in diesem Abschnitt des Lernprogramms erfahren Sie, wie das Entity Framework verwendet [abfangen Funktion](https://msdn.microsoft.com/data/dn469464) direkt, sowohl für Protokollierung und zum Simulieren von vorübergehender Fehlern.

### <a name="create-a-logging-interface-and-class"></a>Erstellen eines protokollierungsschnittstelle und -Klasse

Ein [bewährte Methode für die Protokollierung](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry.md#log) erfolgt mithilfe einer Schnittstelle anstatt eine feste Programmierung Aufrufe System.Diagnostics.Trace-oder eine Protokollierung-Klasse. Die erleichtert es Ihrer Protokollierungsmechanismus später zu ändern, wenn Sie dies möchten. Damit in diesem Abschnitt die protokollierungsschnittstelle und eine Klasse zum Implementieren von darauf/p erstellen Sie > 

1. Erstellen Sie einen Ordner im Projekt, und nennen Sie sie *Protokollierung*.
2. In der *Protokollierung* Ordner, erstellen Sie eine Klassendatei namens *ILogger.cs*, und Ersetzen Sie den Code durch den folgenden Code:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    Die Schnittstelle enthält drei Ablaufverfolgungsstufen an, dass die relative Wichtigkeit der Protokolle, und Sie eine latenzzeitinformationen für Aufrufe von externen Dienst z. B. Datenbankabfragen bereit. Die Protokollierungsmethoden verfügen über Überladungen, mit denen Sie die Ausnahme übergeben. Dies ist so, dass Ausnahmeinformationen einschließlich Stapel-Trace und die inneren Ausnahmen zuverlässig von der Klasse, die die Schnittstelle protokolliert wird anstatt auf, die in jedem Methodenaufruf Protokollierung in der gesamten Anwendung durchgeführter implementiert.

    Die TraceApi-Methoden können Sie verfolgen, die Latenz bei jedem Aufruf an einen externen Dienst z. B. SQL-Datenbank.
3. In der *Protokollierung* Ordner, erstellen Sie eine Klassendatei namens *Logger.cs*, und Ersetzen Sie den Code durch den folgenden Code:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    Die Implementierung verwendet System.Diagnostics für die Ablaufverfolgung. Dies ist eine integrierte Funktion von .NET zum Generieren und nutzen Sie diese Daten einfach. Es gibt viele "Listener" Sie ohne die System.Diagnostics-Ablaufverfolgung zum Schreiben von Protokollen in Dateien, z. B. oder Blob-Speicher in Azure Schreiben verwenden können. Einige der Optionen, und Links zu anderen Ressourcen für Weitere Informationen finden Sie in im [Problembehandlung bei Azure-Websites in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Für dieses Lernprogramm, das Sie nur betrachten in Visual Studio protokolliert **Ausgabe** Fenster.

    ILogger-Schnittstelle vereinfacht relativ zu einem anderen Ablaufverfolgungsmechanismus wechseln, wenn Sie sich dafür entscheiden, und in einer produktionsanwendung sollen Ablaufverfolgung von Paketen als System.Diagnostics berücksichtigt.

### <a name="create-interceptor-classes"></a>Erstellen der Interceptor-Klassen

Als Nächstes erstellen Sie die Klassen, denen das Entity Framework aus aufrufen wird jedes Mal, wenn sie also eine Abfrage an die Datenbank, um vorübergehende Fehler zu simulieren und zu Protokollierung gesendet. Diese Interceptor Klassen leiten sich aus der `DbCommandInterceptor` Klasse. In diesen schreiben Sie die Methode überschreibt, die automatisch aufgerufen werden, wenn die Abfrage ausgeführt werden muss. In diesen Methoden können Sie überprüfen oder melden Sie sich die Abfrage, die an die Datenbank gesendet wird, und ändern Sie die Abfrage aus, bevor sie an die Datenbank gesendet wird oder etwas zurückgeben zu Entity Framework selbst ohne auch die Abfrage an die Datenbank übergeben werden können.

1. Um den Interceptor-Klasse erstellen, die alle SQL-Abfrage protokolliert werden, die an die Datenbank gesendet wird, erstellen Sie eine Klassendatei namens *SchoolInterceptorLogging.cs* in der *DAL* Ordner, und Ersetzen Sie die Vorlage mit code Der folgende Code:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

    Dieser Code schreibt für erfolgreiche Abfragen oder Befehle ein Information-Protokoll mit abonnentenlatenzzeit-Informationen. Für Ausnahmen wird ein Fehlerprotokoll erstellt.
2. Zum Erstellen der Interceptor-Klasse, die bei der Eingabe "Throw" dummy vorübergehende Fehler generieren der **Suche** Feld, erstellen Sie eine Klassendatei namens *SchoolInterceptorTransientErrors.cs* in der *DAL* Ordner, und Ersetzen Sie den Code durch folgenden Code:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

    Dieser code nur überschreibt die `ReaderExecuting` Methode, die für Abfragen aufgerufen wird, die mehrere Datenzeilen zurückgeben können. Falls gewünscht, um resilienz von Verbindungen für andere Typen von Abfragen zu überprüfen, können Sie auch überschreiben die `NonQueryExecuting` und `ScalarExecuting` Methoden, wie die Protokollierung-Interceptor verfügt.

    Beim Ausführen der Seite "Student" und "Throw" als die zu suchende Zeichenfolge eingeben, erstellt der Code eine dummy-SQL-Datenbank-Ausnahme für Fehlernummer 20, einen Typ in der Regel nur vorübergehend aufzutreten bezeichnet. Andere Fehlernummern, die derzeit als vorübergehend erkannt werden 64, 233 10053, 10054, 10060, 10928, 10929, 40197, 40501 und 40613, aber dies sind implementierungsspezifisch in neuen Versionen von SQL-Datenbank.

    Der Code gibt die Ausnahme zurück, auf Entity Framework, anstatt die Abfrage ausführt, und übergeben Abfrageergebnisse zurück. Die vorübergehende Ausnahme viermal zurückgegeben wird, und der Code dann zurückgesetzt, um das normale Verfahren für die Abfrage an die Datenbank übergeben.

    Da alles, was protokolliert werden, müssen Sie möglicherweise feststellen, dass Entity Framework, zum Ausführen der Abfrage viermal versucht vor mehrmaligen und der einzige Unterschied in der Anwendung ist, dass es zum Rendern einer Seite mit Abfrageergebnissen länger dauert.

    Die Anzahl der Häufigkeit, mit die Entity Framework versucht ist konfigurierbar. der Code gibt vier Mal aus, da dies der Standardwert für die SQL-Datenbank-Ausführungsrichtlinie ist. Wenn Sie die Ausführungsrichtlinie ändern, hatte Sie ändern den Code hier, der angibt, wie oft vorübergehende Fehler generiert werden. Außerdem können Sie den Code, um mehrere Ausnahmen generiert werden, damit Entity Framework löst ändern die `RetryLimitExceededException` Ausnahme.

    Der Wert, die Sie in das Suchfeld eingeben, werden in `command.Parameters[0]` und `command.Parameters[1]` (einer für den ersten Namen und eine für den Nachnamen des verwendet wird). Wenn das Wert "% Throw %" gefunden wird, werden "Throw" in diese Parameter von "an" ersetzt, damit einige Studenten gefunden und zurückgegeben werden.

    Dies ist eine komfortable Methode zum Testen der resilienz von Verbindungen basierend auf Eingaben zur Benutzeroberfläche der Anwendung ändern. Sie können auch vorübergehende Fehler für alle Abfragen oder Updates, generierten Code schreiben, wie später in den Kommentaren über die *DbInterception.Add* Methode.
3. In *"Global.asax"*, fügen Sie die folgenden `using` Anweisungen:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]
4. Fügen Sie die markierten Zeilen, die `Application_Start` Methode:

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=7-8)]

    Diese Codezeilen befinden, was bewirkt, dass den Interceptor Code ausgeführt werden, wenn Entity Framework Abfragen an die Datenbank sendet. Beachten Sie, dass, da Sie separate Interceptor Klassen für die Simulation vorübergehender Fehler erstellt und protokollieren, können Sie unabhängig voneinander aktivieren und deaktivieren Sie sie.

    Sie können mithilfe von Interceptors Hinzufügen der `DbInterception.Add` Methode an einer beliebigen Stelle im Code; keinen in die `Application_Start` Methode. Eine weitere Option ist dieser Code in der DbConfiguration-Klasse platziert werden, die Sie zuvor erstellt haben, um die Ausführungsrichtlinie zu konfigurieren.

    [!code-csharp[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs?highlight=6-7)]

    Immer, wenn Sie diesen Code einfügen darauf achten, nicht zum Ausführen `DbInterception.Add` für den gleichen Interceptor mehr als einmal, oder Sie zusätzliche interceptorinstanzen erhalten. Wenn Sie die Protokollierung Interceptor zweimal hinzuzufügen, wird z. B. zwei Protokolle für jede SQL-Abfrage angezeigt.

    Interceptors werden in der Reihenfolge Ihrer Registrierung ausgeführt (die Reihenfolge, in der die `DbInterception.Add` -Methode aufgerufen wird). Die Reihenfolge kann relevant sein, je nachdem, was Sie in der Interceptor tun. Beispielsweise kann ein Interceptor ändern den SQL-Befehl, der es in ruft der `CommandText` Eigenschaft. Wenn sie den SQL-Befehl ändern, erhalten der nächste Interceptor den geänderten SQL-Befehl nicht den ursprünglichen SQL-Befehl.

    Sie haben auf eine Weise simulationscode vorübergehender Fehler geschrieben, die dazu führen, dass vorübergehende Fehler durch Eingabe eines anderen Werts in der Benutzeroberfläche ermöglicht. Als Alternative können konnte den Interceptor-Code, um die Sequenz von vorübergehenden Ausnahmen generieren immer ohne Überprüfung für einen bestimmten Parameterwert geschrieben werden. Sie können den Interceptor hinzufügen nur, wenn Sie, um vorübergehende Fehler zu generieren möchten. Wenn Sie dies tun, nicht jedoch Hinzufügen des Interceptors bis, nach Abschluss der Initialisierung der Datenbank. Führen Sie mindestens eine Datenbank-Vorgang wie z. B. eine Abfrage auf einem Ihrer Entitätenmengen also vor der Generierung von vorübergehenden Fehlern. Das Entity Framework werden mehrere Abfragen während der Initialisierung der Datenbank ausgeführt, und sie werden nicht in einer Transaktion ausgeführt, sodass Fehler während der Initialisierung den Kontext, in einem inkonsistenten Zustand zu versetzen verursachen könnte.

## <a name="test-logging-and-connection-resiliency"></a>Test-Protokollierung und Verbindung resilienz

1. Drücken Sie F5, um die Anwendung im Debugmodus ausgeführt, und klicken Sie dann auf die **Studenten** Registerkarte.
2. Betrachten Sie die Visual Studio **Ausgabe** Fenster aus, um die Ablaufverfolgungsausgabe finden Sie unter. Möglicherweise müssen Sie über einige JavaScript-Fehler, um die Protokolle geschrieben, die von Ihrer Protokollierung erhalten nach oben zu scrollen.

    Beachten Sie, dass Sie, die tatsächlichen SQL-Abfragen an die Datenbank gesendet sehen können. Einige anfängliche Abfragen und Befehle, die Entity Framework ermöglicht, um zu beginnen, überprüfen die Version der Datenbank und die Migration Verlaufstabelle (zur Migration in den nächsten Lernprogrammen erfahren Sie) angezeigt. Und Sie eine Abfrage für das Paging, um herauszufinden, wie viele Studenten vorhanden sind, finden Sie unter ab und schließlich die Abfrage, die die Student Daten abruft.

    ![Protokollierung für normale Abfrage](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)
3. In der **Studenten** Seite als der zu suchende Zeichenfolge eingeben "Throw", und klicken Sie auf **Suche**.

    ![Lösen Sie die zu suchende Zeichenfolge](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    Sie werden bemerken, dass der Browser ist scheinbar stillstehen für einige Sekunden, während das Entity Framework die Abfrage mehrere Male wiederholt. Der erste Wiederholungsversuch erfolgt sehr schnell, klicken Sie dann die Wartezeit vor vor jedem Neuversuch zusätzliche erhöht. Dieser Vorgang mehr vor jedem Neuversuch aufgerufen wird, warten auf *Exponentielles Backoff*.

    Wenn die Seite angezeigt wird, zeigt Studenten mit "ein" in ihren Namen, betrachten Sie das Fenster "Ausgabe", und Sie sehen, dass die gleiche Abfrage fünfmal zuerst viermal zurückgebenden vorübergehenden erfolgte Ausnahmen. Für jeden vorübergehender Fehler sehen Sie das Protokoll, den Sie schreiben, während der Generierung den vorübergehenden Fehler in der `SchoolInterceptorTransientErrors` -Klasse ("Returning vorübergehender Fehler für Befehl...") und Sie sehen das Protokoll geschrieben werden, wenn `SchoolInterceptorLogging` Ruft die Ausnahme ab.

    ![Anzeigen von Wiederholungen Protokollierungsausgabe](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    Da Sie eine Suchzeichenfolge eingegeben haben, parametrisiert die Abfrage, die Student-Daten zurückgibt:

    [!code-sql[Main](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.sql)]

    Protokolliert Sie nicht den Wert der Parameter, aber Sie könnten dazu. Wenn Sie die Parameterwerte anzeigen möchten, können Sie schreiben Protokollierungscode zum Abrufen von Parameterwerten aus der `Parameters` Eigenschaft von der `DbCommand` -Objekt, das Sie in den interceptormethoden zu erhalten.

    Beachten Sie, dass es sich bei diesem Test nicht wiederholt werden kann, es sei denn, Sie die Anwendung zu beenden und starten Sie ihn neu. Wunsch verbindungsstabilität mehrmals in einer einzelnen Ausführung der Anwendung testen können Sie konnte Code schreiben, um die Fehleranzahl im Zurücksetzen `SchoolInterceptorTransientErrors`.
4. Mit den Unterschied Ausführungsstrategie (wiederholungsrichtlinie) hergestellt hat, Kommentar der `SetExecutionStrategy` in Zeile *SchoolConfiguration.cs*, führen Sie die Seite "Students" im Debugmodus erneut aus und suchen Sie nach "Throw" erneut aus.

    Dieses Mal hält der Debugger bei der ersten generierte Ausnahme sofort, wenn er versucht, die Abfrage beim ersten ausführen.

    ![Dummy-Ausnahme](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)
5. Heben Sie die auskommentierung der *SetExecutionStrategy* in Zeile *SchoolConfiguration.cs*.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie gesehen, wie resilienz von Verbindungen zu aktivieren, und melden Sie sich SQL-Befehle, die Entity Framework aufstellt und sendet an die Datenbank. In den nächsten Lernprogrammen stellen Sie die Anwendung mit dem Internet mithilfe von Code First-Migrationen zum Bereitstellen der Datenbank bereit.

Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir weiter verbessern können. Sie können auch neue Themen am anfordern [Me wie mit Code anzeigen](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).

Links zu anderen Entity Framework-Ressourcen finden Sie im [ASP.NET Data Access - Ressourcen empfohlen](../../../../whitepapers/aspnet-data-access-content-map.md).

>[!div class="step-by-step"]
[Zurück](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md)
[Weiter](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md)
