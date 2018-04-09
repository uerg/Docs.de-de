---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: ASP.NET Web Forms Connection Resiliency und Befehl abfangen | Microsoft Docs
author: Erikre
description: In diesem Lernprogramm wird beschrieben, wie so ändern Sie eine beispielanwendung zur Unterstützung von resilienz von Verbindungen und Befehl abgefangen wird.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: d5c4e46209e1b21a303fdf1fb16c6c868b3ca923
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>ASP.NET Web Forms Connection Resiliency und Befehl abfangen
====================
by [Erik Reitan](https://github.com/Erikre)

In diesem Lernprogramm ändern Sie die Wingtip Toys-beispielanwendung zur Unterstützung von resilienz von Verbindungen und Befehl abfangen. Durch die Aktivierung von resilienz von Verbindungen, wiederholt die beispielanwendung des Wingtip Toys automatisch Daten aufrufen, treten vorübergehende Fehler, die typisch für eine Cloud-Umgebung. Darüber hinaus durch die Implementierung Befehl abfangen können die beispielanwendung des Wingtip Toys fängt alle SQL-Abfragen gesendet, um die Datenbank aus, um die melden oder Sie zu ändern.

> [!NOTE] 
> 
> In diesem Lernprogramm Web Forms wurde basierend auf folgenden Tom Dykstras MVC-Lernprogramm:  
> [Verbindungsresilienz und Abfangen der Befehl mit dem Entity Framework in einer ASP.NET MVC-Anwendung](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>Lernen Sie:

- Wie verbindungsstabilität bereitgestellt.
- Zum Abfangen der Befehl zu implementieren.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, stellen Sie sicher, dass Sie die folgende Software auf Ihrem Computer installiert haben:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) oder [Microsoft Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework wird automatisch installiert.
- Die Wingtip Toys Beispielprojekt, damit Sie die Funktionalität, die in diesem Lernprogramm innerhalb des Wingtip Toys-Projekts genannten implementieren können. Der folgende Link enthält Details herunterladen:

    - [Erste Schritte mit ASP.NET 4.5.1 Web Forms - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (c#)
- Vor dem Abschluss dieses Lernprogramms sollten Sie das Lernprogrammen aufeinander bezogene Reihen, [erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Die Reihe von Lernprogrammen hilft Ihnen, sich mit vertraut der **WingtipToys** Projekt- und Code.

## <a name="connection-resiliency"></a>Verbindungsresilienz

Wenn Sie in Betracht ziehen, eine Anwendung in Windows Azure bereitstellen, wird eine Option zu berücksichtigenden Bereitstellen der Datenbank zu **Windows** **Azure SQL-Datenbank**, einen Cloud-Dienst-Datenbank. Vorübergehende Verbindungsfehler sind in der Regel häufiger, beim Herstellen der Verbindung mit einem Clouddienst für die Datenbank oder Ihrem Webserver und dem Datenbankserver direkt zusammen im selben Rechenzentrum angeschlossen sind. Auch wenn ein Cloud-Web-Server und einen Clouddienst für die Datenbank im selben Rechenzentrum gehostet werden, stehen weitere Netzwerkverbindungen zwischen ihnen, die Probleme, z. B. Lastenausgleichsmodule aufweisen können.

Auch ist ein Cloud-Dienst in der Regel gemeinsam mit anderen Benutzern, was bedeutet, dass die Reaktionsfähigkeit von ihnen betroffen sein kann. Und der Zugriff auf die Datenbank möglicherweise unterliegen die Einschränkung. Einschränkung bedeutet, dass der Datenbankdienst löst Ausnahmen aus, wenn Sie darauf zuzugreifen versuchen, häufiger als zulässig in Ihre *Vereinbarung zum Servicelevel* (SLA).

Viele oder die meisten Verbindungsprobleme, die auftreten, wenn Sie einen Cloud-Dienst zugreifen, sind vorübergehend, d. h. sie aufgelöst werden, selbst innerhalb kurzer Zeit. Also wenn Sie versuchen Sie es einer Datenbank und einen Typ des Fehlers, der in der Regel vorübergehend ist, können Sie den Vorgang versuchen, nach kurze Zeit warten, und der Vorgang erfolgreich ausgeführt werden kann. Sie können eine bessere navigationsmöglichkeiten für Ihre Benutzer bereitstellen, wenn vorübergehende Fehler zu behandeln, automatisch versucht erneut, die meisten von ihnen an den Kunden unsichtbar machen. Das verbindungsstabilitätsfeature in Entity Framework 6 automatisiert werden, dass der Vorgang einer wiederholten SQL-Abfragen ist fehlgeschlagen.

Das verbindungsstabilitätsfeature muss für eine bestimmte Datenbank-Dienst ordnungsgemäß konfiguriert werden:

1. Er muss wissen, welche Ausnahmen nur vorübergehend aufzutreten wahrscheinlich sind. Fehler aufgrund einer vorübergehenden Dienstausfall Netzwerkkonnektivität, nicht vom Programmfehler verursacht hat, z. B. Fehler wiederholt werden sollen.
2. Es muss eine entsprechende Menge an Zeit zwischen einer Wiederholung eines fehlgeschlagenen Vorgangs warten. Sie können mehr zwischen den Wiederholungsversuchen für einen Batchprozess warten, als Sie für eine online-Webseite können, in denen ein Benutzer auf eine Antwort wartet.
3. Er verfügt über eine entsprechende Anzahl von Zyklen, bevor der Vorgang abgebrochen wird wiederholt. Sie möchten möglicherweise mehrere Male in einem Batchprozess wiederholen, die in einer online-Anwendung aus.

Sie können diese Einstellungen manuell für jede datenbankumgebung, die von einem Anbieter für Entity Framework unterstützt konfigurieren.

Müssen Sie lediglich ausführen, um resilienz von Verbindungen aktivieren eine Klasse in der Assembly zu erstellen, die abgeleitet ist die `DbConfiguration` Klasse, und legen Sie in dieser Klasse die Ausführungsstrategie SQL-Datenbank, die in Entity Framework eine andere Bezeichnung für die wiederholungsrichtlinie ist.

### <a name="implementing-connection-resiliency"></a>Implementieren von resilienz von Verbindungen

1. Herunter, und Öffnen der [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) Web Forms-Anwendung in Visual Studio-Beispiel.
2. In der *Logik* Ordner, der die **WingtipToys** Anwendung, fügen Sie eine Klassendatei mit dem Namen *WingtipToysConfiguration.cs*.
3. Ersetzen Sie den vorhandenen Code durch folgenden Code:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Das Entity Framework automatisch ausgeführt wird, den Code in einer Klasse, die abgeleitet werden gefundenen `DbConfiguration`. Können Sie die `DbConfiguration` Klasse Konfigurationsaufgaben in Code ausführen, die Sie andernfalls würde in die *"Web.config"* Datei. Weitere Informationen finden Sie unter [EntityFramework codebasierte Konfiguration](https://msdn.microsoft.com/data/jj680699).

1. In der *Logik* Ordner die *AddProducts.cs* Datei.
2. Hinzufügen einer `using` -Anweisung für `System.Data.Entity.Infrastructure` wie in gelb hervorgehoben dargestellt:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Hinzufügen einer `catch` -block, um die `AddProduct` Methode, damit die `RetryLimitExceededException` ist gelb protokolliert:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Durch Hinzufügen der `RetryLimitExceededException` Ausnahme, die von Ihnen bereitgestellte besser Protokollierung oder zeigt eine Fehlermeldung an, die dem Benutzer, die den Prozess erneut auswählen können. Indem Sie Sie Abfangen der `RetryLimitExceededException` Ausnahme, die wahrscheinlich nur vorübergehend aufzutreten nur Fehler werden bereits ausprobiert und wurden Fehler mehrere Male. Die eigentliche Ausnahme zurückgegeben wird umschlossen werden, der `RetryLimitExceededException` Ausnahme. Darüber hinaus nun auch einen allgemeinen Catch-Block. Weitere Informationen zu den `RetryLimitExceededException` Ausnahme finden Sie unter [Entity Framework Connection Resiliency / Wiederholungslogik](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Befehl abfangen

Nun, dass Sie eine wiederholungsrichtlinie aktiviert haben, führen Sie wie Sie testen, stellen Sie sicher, dass er wie erwartet funktioniert? Es ist nicht so einfach um einen vorübergehenden Fehler auftreten, insbesondere wenn Sie lokal ausführen und es wäre besonders schwierig zum Integrieren von tatsächlichen vorübergehende Fehler in eine automatisierte Komponententests zu erzwingen. Um das verbindungsstabilitätsfeature zu testen, benötigen Sie eine Möglichkeit zum Abfangen von Abfragen, die Entity Framework mit SQL Server sendet, und Ersetzen Sie die SQL Server-Antwort durch einen Ausnahmetyp aus, der in der Regel flüchtig ist.

Sie können auch Abfangen der Abfrage verwenden, um eine bewährte Methode für Cloudanwendungen zu implementieren: Protokoll die Latenz und Erfolg oder Fehler aller zu externen Aufrufe Diensten, z. B. Datenbankdienste.

In diesem Abschnitt des Lernprogramms verwenden Sie die Entity Framework [ *abfangen Feature* ](https://msdn.microsoft.com/data/dn469464) sowohl für die Protokollierung und zum Simulieren von vorübergehender Fehlern.

### <a name="create-a-logging-interface-and-class"></a>Erstellen eines protokollierungsschnittstelle und -Klasse

Eine bewährte Methode für die Protokollierung ist dafür mithilfe einer [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx) anstatt eine feste Programmierung Aufrufe an `System.Diagnostics.Trace` oder eine Protokollierung-Klasse. Die erleichtert es Ihrer Protokollierungsmechanismus später zu ändern, wenn Sie dies möchten. In diesem Abschnitt müssen Sie so die protokollierungsschnittstelle und einer Klasse zu implementieren erstellen.

Wie oben beschrieben auf Grundlage, Sie heruntergeladen und geöffnet haben die **WingtipToys** beispielanwendung in Visual Studio.

1. Erstellen Sie einen Ordner in der **WingtipToys** Projekt, und nennen Sie sie *Protokollierung*.
2. In der *Protokollierung* Ordner, erstellen Sie eine Klassendatei namens *ILogger.cs* , und Ersetzen Sie den Standardcode durch folgenden Code:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   Die Schnittstelle enthält drei Ablaufverfolgungsstufen an, dass die relative Wichtigkeit der Protokolle, und Sie eine latenzzeitinformationen für Aufrufe von externen Dienst z. B. Datenbankabfragen bereit. Die Protokollierungsmethoden verfügen über Überladungen, mit denen Sie die Ausnahme übergeben. Dies ist so, dass Ausnahmeinformationen einschließlich Stapel-Trace und die inneren Ausnahmen zuverlässig von der Klasse, die die Schnittstelle protokolliert wird anstatt auf, die in jedem Methodenaufruf Protokollierung in der gesamten Anwendung durchgeführter implementiert.  
  
   Die `TraceApi` Methoden ermöglichen es Ihnen, die Latenz bei jedem Aufruf an einen externen Dienst z. B. SQL-Datenbank nachverfolgt.
3. In der *Protokollierung* Ordner, erstellen Sie eine Klassendatei namens *Logger.cs* , und Ersetzen Sie den Standardcode durch folgenden Code:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

Die Implementierung verwendet `System.Diagnostics` , führen Sie die Ablaufverfolgung. Dies ist eine integrierte Funktion von .NET zum Generieren und nutzen Sie diese Daten einfach. Es gibt viele &quot;Listener&quot; können Sie mit `System.Diagnostics` Ablaufverfolgung, in Dateien, z. B. Protokolle geschrieben oder um sie in Windows Azure Blob-Speicher zu schreiben. Einige der Optionen, und Links zu anderen Ressourcen für Weitere Informationen finden Sie in im [Problembehandlung bei Windows Azure-Websites in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). Für dieses Lernprogramm nur analysieren Sie die Protokolle in der Visual Studio **Ausgabe** Fenster.

In einer produktionsanwendung sollten in Betracht Tracing Frameworks außer `System.Diagnostics`, und die `ILogger` Schnittstelle vereinfacht relativ zu einem anderen Ablaufverfolgungsmechanismus wechseln, wenn Sie sich dazu entscheiden.

### <a name="create-interceptor-classes"></a>Erstellen der Interceptor-Klassen

Als Nächstes erstellen Sie die Klassen, denen das Entity Framework aus aufrufen wird jedes Mal, wenn sie also eine Abfrage an die Datenbank, um vorübergehende Fehler zu simulieren und zu Protokollierung gesendet. Diese Interceptor Klassen leiten sich aus der `DbCommandInterceptor` Klasse. In, Schreiben Sie die Methode überschreibt, die automatisch aufgerufen werden, wenn die Abfrage ausgeführt werden muss. In diesen Methoden können Sie überprüfen oder melden Sie sich die Abfrage, die an die Datenbank gesendet wird, und ändern Sie die Abfrage aus, bevor sie an die Datenbank gesendet wird oder etwas zurückgeben zu Entity Framework selbst ohne auch die Abfrage an die Datenbank übergeben werden können.

1. Um den Interceptor-Klasse erstellen, die alle SQL-Abfrage protokolliert werden, bevor sie an die Datenbank gesendet wird, erstellen Sie eine Klassendatei namens *InterceptorLogging.cs* in der *Logik* Ordner und Ersetzen Sie die Standardeinstellung von code mit der folgender Code:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Dieser Code schreibt für erfolgreiche Abfragen oder Befehle ein Information-Protokoll mit abonnentenlatenzzeit-Informationen. Für Ausnahmen wird ein Fehlerprotokoll erstellt.
2. So erstellen Sie den Interceptor-Klasse, die bei der Eingabe dummy vorübergehende Fehler generiert &quot;auslösen&quot; in der **Namen** Textfeld auf der Seite mit dem Namen *AdminPage.aspx*, erstellen Sie eine Klasse Datei mit dem Namen *InterceptorTransientErrors.cs* in der *Logik* Ordner und Ersetzen Sie die Standardeinstellung von code durch den folgenden Code:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Dieser code nur überschreibt die `ReaderExecuting` Methode, die für Abfragen aufgerufen wird, die mehrere Datenzeilen zurückgeben können. Falls gewünscht, um resilienz von Verbindungen für andere Typen von Abfragen zu überprüfen, können Sie auch überschreiben die `NonQueryExecuting` und `ScalarExecuting` Methoden, wie die Protokollierung-Interceptor verfügt.  
  
   Sie werden später erneut, melden Sie sich als "Administrator" und wählen die **Admin** Link auf der oberen Navigationsleiste. Klicken Sie dann auf die *AdminPage.aspx* Seite Sie ein Produkt mit dem Namen fügen &quot;auslösen&quot;. Der Code erstellt eine dummy-SQL-Datenbank-Ausnahme für Fehlernummer 20, einen Typ in der Regel nur vorübergehend aufzutreten bezeichnet. Andere Fehlernummern, die derzeit als vorübergehend erkannt werden 64, 233 10053, 10054, 10060, 10928, 10929, 40197, 40501 und 40613, aber dies sind implementierungsspezifisch in neuen Versionen von SQL-Datenbank. Das Produkt wird in "TransientErrorExample", die Sie im Code des folgen können umbenannt werden die *InterceptorTransientErrors.cs* Datei.  
  
   Der Code gibt die Ausnahme zurück, auf Entity Framework, anstatt die Abfrage ausgeführt, und übergeben die Ergebnisse zurück. Die vorübergehende Ausnahme zurückgegeben *vier* Zeiten und der Code dann wiederhergestellt wird, um das normale Verfahren für die Abfrage an die Datenbank übergeben.

    Da alles, was protokolliert werden, müssen Sie möglicherweise feststellen, dass Entity Framework, zum Ausführen der Abfrage viermal versucht vor mehrmaligen und der einzige Unterschied in der Anwendung ist, dass es zum Rendern einer Seite mit Abfrageergebnissen länger dauert.  
  
   Die Anzahl der Häufigkeit, mit die Entity Framework versucht ist konfigurierbar. der Code gibt vier Mal aus, da dies der Standardwert für die SQL-Datenbank-Ausführungsrichtlinie ist. Wenn Sie die Ausführungsrichtlinie ändern, hatte Sie ändern den Code hier, der angibt, wie oft vorübergehende Fehler generiert werden. Außerdem können Sie den Code, um mehrere Ausnahmen generiert werden, damit Entity Framework löst ändern die `RetryLimitExceededException` Ausnahme.
3. In *"Global.asax"*, fügen Sie die folgenden using-Anweisungen:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Fügen Sie dann auf die markierten Zeilen, die die `Application_Start` Methode:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Diese Codezeilen befinden, was bewirkt, dass den Interceptor Code ausgeführt werden, wenn Entity Framework Abfragen an die Datenbank sendet. Beachten Sie, dass, da Sie separate Interceptor Klassen für die Simulation vorübergehender Fehler erstellt und protokollieren, können Sie unabhängig voneinander aktivieren und deaktivieren Sie sie.   
  
 Sie können mithilfe von Interceptors Hinzufügen der `DbInterception.Add` Methode an einer beliebigen Stelle im Code; keinen in die `Application_Start` Methode. Eine weitere Option, wenn Sie Interceptors in hinzufügen, haben nicht die `Application_Start` Methode wäre zu aktualisieren, oder fügen Sie die Klasse mit dem Namen *WingtipToysConfiguration.cs* und platzieren Sie den oben aufgeführten Code am Ende des Konstruktors die `WingtipToysbConfiguration` Klasse.

Immer, wenn Sie diesen Code einfügen darauf achten, nicht zum Ausführen `DbInterception.Add` für den gleichen Interceptor mehr als einmal, oder Sie zusätzliche interceptorinstanzen erhalten. Wenn Sie die Protokollierung Interceptor zweimal hinzuzufügen, wird z. B. zwei Protokolle für jede SQL-Abfrage angezeigt.

Interceptors werden in der Reihenfolge Ihrer Registrierung ausgeführt (die Reihenfolge, in der die `DbInterception.Add` -Methode aufgerufen wird). Die Reihenfolge kann relevant sein, je nachdem, was Sie in der Interceptor tun. Beispielsweise kann ein Interceptor ändern den SQL-Befehl, der es in ruft der `CommandText` Eigenschaft. Wenn sie den SQL-Befehl ändern, erhalten der nächste Interceptor den geänderten SQL-Befehl nicht den ursprünglichen SQL-Befehl.

Sie haben auf eine Weise simulationscode vorübergehender Fehler geschrieben, die dazu führen, dass vorübergehende Fehler durch Eingabe eines anderen Werts in der Benutzeroberfläche ermöglicht. Als Alternative können konnte den Interceptor-Code, um die Sequenz von vorübergehenden Ausnahmen generieren immer ohne Überprüfung für einen bestimmten Parameterwert geschrieben werden. Sie können den Interceptor hinzufügen nur, wenn Sie, um vorübergehende Fehler zu generieren möchten. Wenn Sie dies tun, nicht jedoch Hinzufügen des Interceptors bis, nach Abschluss der Initialisierung der Datenbank. Führen Sie mindestens eine Datenbank-Vorgang wie z. B. eine Abfrage auf einem Ihrer Entitätenmengen also vor der Generierung von vorübergehenden Fehlern. Das Entity Framework werden mehrere Abfragen während der Initialisierung der Datenbank ausgeführt, und sie werden nicht in einer Transaktion ausgeführt, sodass Fehler während der Initialisierung den Kontext, in einem inkonsistenten Zustand zu versetzen verursachen könnte.

## <a name="test-logging-and-connection-resiliency"></a>Test-Protokollierung und Verbindung resilienz

1. Drücken Sie in Visual Studio **F5** zum Ausführen der Anwendung im Debugmodus befindet, und klicken Sie dann melden Sie sich als "Admin" mit "Pa$ $Word" als Kennwort ein.
2. Wählen Sie **Admin** in der Navigationsleiste oben.
3. Geben Sie ein neues Produkt mit dem Namen "Throw" mit der entsprechenden Beschreibung, Preis und Image-Datei ein.
4. Drücken Sie die **Produkt hinzufügen** Schaltfläche.  
   Sie werden bemerken, dass der Browser ist scheinbar stillstehen für einige Sekunden, während das Entity Framework die Abfrage mehrere Male wiederholt. Der erste Wiederholungsversuch erfolgt sehr schnell, und klicken Sie dann die Wartezeit vor jedem Neuversuch zusätzliche erhöht. Dieser Vorgang mehr vor jedem Neuversuch aufgerufen wird, warten auf *Exponentielles Backoff* .
5. Warten Sie, bis die Seite nicht mehr Atttempting geladen wird.
6. Beenden Sie das Projekt, und sehen Sie sich im Visual Studio **Ausgabe** Fenster aus, um die Ablaufverfolgungsausgabe finden Sie unter. Sie finden die **Ausgabe** Fenster dazu **Debuggen**  - &gt; **Windows**  - &gt;  **Ausgabe**. Sie müssen möglicherweise führen Sie einen Bildlauf nach verschiedene Protokolle, die von Ihrer Protokollierung geschrieben.  
  
   Beachten Sie, dass Sie, die tatsächlichen SQL-Abfragen an die Datenbank gesendet sehen können. Sie finden Sie einige anfängliche Abfragen und Befehle, die Entity Framework ermöglicht wird, um zu beginnen, überprüfen die Datenbank-Version und Migration die Verlaufstabelle.   
    ![Ausgabefenster](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Beachten Sie, dass es sich bei diesem Test nicht wiederholt werden kann, es sei denn, Sie die Anwendung zu beenden und starten Sie ihn neu. Wunsch verbindungsstabilität mehrmals in einer einzelnen Ausführung der Anwendung testen können Sie konnte Code schreiben, um die Fehleranzahl im Zurücksetzen `InterceptorTransientErrors` .
7. Mit den Unterschied Ausführungsstrategie (wiederholungsrichtlinie) hergestellt hat, Kommentar der `SetExecutionStrategy` in Zeile *WingtipToysConfiguration.cs* in der Datei die *Logik* Ordner, führen Sie die **Admin**  Seite im Debugmodus erneut, und fügen Sie das Produkt mit dem Namen &quot;auslösen&quot; erneut aus.  
  
   Dieses Mal hält der Debugger bei der ersten generierte Ausnahme sofort, wenn er versucht, die Abfrage beim ersten ausführen.  
    ![Debuggen - Details anzeigen](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Heben Sie die auskommentierung der `SetExecutionStrategy` Zeile in der *WingtipToysConfiguration.cs* Datei.

## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben gezeigt, wie Sie eine Web Forms-beispielanwendung zur Unterstützung von resilienz von Verbindungen und Abfangen der Befehl zu ändern.

## <a name="next-steps"></a>Nächste Schritte

Nachdem resilienz von Verbindungen und Abfangen des Befehls in ASP.NET Web Forms überprüft haben, lesen Sie die ASP.NET Web Forms-Thema [asynchroner Methoden in ASP.NET 4.5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). Das Thema erfahren Sie die Grundlagen der Erstellung einer asynchronen ASP.NET Web Forms-Anwendung mit Visual Studio.
