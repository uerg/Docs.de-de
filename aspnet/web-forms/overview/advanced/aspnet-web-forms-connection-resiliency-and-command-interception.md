---
uid: web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
title: ASP.NET Web Forms-Verbindungsresilienz und Abfangen von Befehlen | Microsoft-Dokumentation
author: Erikre
description: Dieses Tutorial beschreibt, wie Sie eine beispielanwendung zur Unterstützung von verbindungsresilienz und Abfangen von Befehlen zu ändern.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 6d497001-fa80-4765-b4cc-181fe90b894e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/advanced/aspnet-web-forms-connection-resiliency-and-command-interception
msc.type: authoredcontent
ms.openlocfilehash: 8b8f592c4e79cf9dbe1255c93fac2fe18f1d9409
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37368320"
---
<a name="aspnet-web-forms-connection-resiliency-and-command-interception"></a>ASP.NET Web Forms-Verbindungsresilienz und Abfangen von Befehlen
====================
durch [Erik Reitan](https://github.com/Erikre)

In diesem Tutorial ändern Sie die Wingtip Toys-beispielanwendung verbindungsresilienz und Abfangen von Befehlen zu unterstützen. Aktivieren Sie die resilienz von Verbindungen, wiederholt die Wingtip Toys-beispielanwendung automatisch Daten aufrufen, beim Auftreten von vorübergehender Fehlern, die typisch für eine Cloud-Umgebung. Darüber hinaus implementieren Sie Abfangen von Befehlen, die Wingtip Toys-beispielanwendung fängt alle SQL-Abfragen an die Datenbank aus, um zu melden oder geändert werden gesendet.

> [!NOTE] 
> 
> In diesem Tutorial Web Forms basiert auf folgenden Tom Dykstras MVC-Tutorial:  
> [Verbindungsresilienz und Abfangen von Befehlen mit Entitätsframework in einer ASP.NET MVC-Anwendung](../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)


## <a name="what-youll-learn"></a>Sie lernen Folgendes:

- So geben Sie die resilienz von Verbindungen.
- Informationen zum Abfangen von Befehlen zu implementieren.

## <a name="prerequisites"></a>Erforderliche Komponenten

Bevor Sie beginnen, stellen Sie sicher, dass Sie die folgende Software auf Ihrem Computer installiert haben:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) oder [Microsoft Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework wird automatisch installiert.
- Die Wingtip Toys Beispielprojekt, damit Sie die Funktionalität, die in diesem Lernprogramm, in das Wingtip Toys-Projekt genannten implementieren können. Den folgenden Link werden die Details herunterladen:

    - [Erste Schritte mit ASP.NET 4.5.1 Web Forms - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) (c#)
- Vor dem Abschluss dieses Lernprogramms sollten Sie die entsprechenden Reihe von Tutorials, [erste Schritte mit ASP.NET 4.5 Web Forms und Visual Studio 2013](../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md). Der Tutorial-Reihe hilft Ihnen, sich mit vertraut machen die **WingtipToys** Projekt- und Code.

## <a name="connection-resiliency"></a>Verbindungsresilienz

Ist die Bereitstellung eine Option in Betracht ziehen, wenn Sie in Betracht ziehen, eine Anwendung in Windows Azure bereitstellen, der Datenbank **Windows** **Azure SQL-Datenbank**, einen Clouddienst für die Datenbank. Vorübergehenden Verbindungsfehlern sind in der Regel häufiger, wenn Sie eine Verbindung herstellen, um einen Clouddienst für die Datenbank oder Ihr Webserver und dem Datenbankserver direkt zusammen im selben Rechenzentrum angeschlossen sind. Auch wenn ein Cloud-Webserver und einen Clouddienst für die Datenbank in demselben Rechenzentrum gehostet werden, stehen weitere Netzwerkverbindungen zwischen ihnen, die Probleme, z. B. Load balancer haben kann.

Außerdem ist ein Cloud-Dienst in der Regel gemeinsam mit anderen Benutzern, was bedeutet, dass ihre Reaktionsfähigkeit, die von ihnen beeinflusst werden kann. Und Ihr Zugriff auf die Datenbank gelten möglicherweise die Drosselung. Löst Ausnahmen Einschränkung bedeutet, dass der Datenbank-Dienst werden, wenn Sie versuchen, es häufiger als in zulässig ist der Zugriff auf Ihre *Vereinbarung zum Servicelevel* (SLA).

Viele oder die meisten Verbindungsprobleme, die auftreten, wenn Sie einen Cloud-Dienst zugreifen, sind vorübergehend, d. h. sie beheben sich selbst in kurzer Zeit. Also, wenn Sie versuchen Sie es einer Datenbank und einen Typ des Fehlers, der in der Regel vorübergehend ist, können Sie den Vorgang erneut versuchen, nach einer kurzen Wartezeit und der Vorgang erfolgreich sein kann. Sie können ein viel besseres Benutzererlebnis für Ihre Benutzer bereitstellen, wenn vorübergehende Fehler behandeln, indem versucht automatisch, die in diesem Fall die meisten von ihnen an den Kunden unsichtbar machen. Das verbindungsstabilitätsfeature in Entity Framework 6 automatisiert werden, dass der Vorgang einer wiederholten SQL-Abfragen ist fehlgeschlagen.

Das verbindungsstabilitätsfeature muss für eine bestimmte Datenbank-Dienst entsprechend konfiguriert werden:

1. Er muss wissen, welche Ausnahmen wahrscheinlich vorübergehend sind. Fehler von einem vorübergehenden Dienstausfall verursacht werden, an der Netzwerkkonnektivität nicht die Ursache von Programmfehlern, z. B. Fehler wiederholt werden sollen.
2. Er verfügt über eine entsprechende Zeitspanne zwischen zwei Wiederholungen einlegen, der einen fehlgeschlagenen Vorgang zu warten. Sie können mehr zwischen den Wiederholungsversuchen für einen Batchprozess warten, als Sie für eine Website können, in denen ein Benutzer auf eine Antwort wartet.
3. Er verfügt über eine entsprechende Anzahl von Malen, bevor er wird abgebrochen wiederholt. Sie möchten möglicherweise weitere Male in einem Batchprozess zu wiederholen, die Sie in einer online-Anwendung verwenden würden.

Sie können diese Einstellungen manuell für jede datenbankumgebung, die von einem Anbieter für Entity Framework unterstützt konfigurieren.

Alles, was Sie tun, um die resilienz von Verbindungen zu aktivieren ist eine Klasse in der Assembly zu erstellen, die von abgeleitet der `DbConfiguration` Klasse, und legen Sie die Ausführungsstrategie für SQL-Datenbank, die in Entity Framework ein weiterer Begriff für die wiederholungsrichtlinie ist in dieser Klasse.

### <a name="implementing-connection-resiliency"></a>Implementieren resilienz von Verbindungen

1. Herunterladen und öffnen Sie die [WingtipToys](https://go.microsoft.com/fwlink/?LinkID=389434&amp;clcid=0x409) Web Forms-beispielanwendung in Visual Studio.
2. In der *Logik* Ordner die **WingtipToys** Anwendung fügen Sie eine Klassendatei mit dem Namen *WingtipToysConfiguration.cs*.
3. Ersetzen Sie den vorhandenen Code durch folgenden Code:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample1.cs)]

Das Entity Framework führt den Code in eine abgeleitete Klasse gefundenen `DbConfiguration`. Können Sie die `DbConfiguration` Klasse, um Konfigurationsaufgaben im Code ausführen, die Sie andernfalls würde in die *"Web.config"* Datei. Weitere Informationen finden Sie unter [EntityFramework codebasierte Konfiguration](https://msdn.microsoft.com/data/jj680699).

1. In der *Logik* Ordner die *AddProducts.cs* Datei.
2. Hinzufügen einer `using` -Anweisung für `System.Data.Entity.Infrastructure` wie in gelb hervorgehoben angezeigt:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample2.cs?highlight=6)]
3. Hinzufügen einer `catch` -block, um die `AddProduct` Methode, damit die `RetryLimitExceededException` wird Gelb protokolliert:   

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample3.cs?highlight=14-15,17-22)]

Durch Hinzufügen der `RetryLimitExceededException` Ausnahme, die Sie bieten eine bessere Protokollierung oder zeigt eine Fehlermeldung an, die dem Benutzer, in denen sie können auch den Prozess wiederholen, können. Indem Sie Sie Abfangen der `RetryLimitExceededException` Ausnahme, die nur Fehler wahrscheinlich vorübergehend ist bereits ausprobiert und wurden Fehler mehrere Male. Die eigentliche Ausnahme zurückgegeben wird eingebunden werden, der `RetryLimitExceededException` Ausnahme. Darüber hinaus, dass hinzugefügt Sie auch einen allgemeinen Catch-Block. Weitere Informationen zu den `RetryLimitExceededException` Ausnahme finden Sie unter [Entity Framework-Verbindungsstabilität / Wiederholungslogik](https://msdn.microsoft.com/data/dn456835).

## <a name="command-interception"></a>Abfangen von Befehlen

Nun, dass Sie eine wiederholungsrichtlinie aktiviert haben, testen wie Sie, stellen Sie sicher, dass es wie erwartet funktioniert? Es ist nicht so leicht zu erzwingen, dass einen vorübergehenden Fehler erfolgen kann, insbesondere wenn Sie lokal ausführen, und es wäre besonders schwierig, tatsächliche vorübergehende Fehler in eine automatisierte Komponententests zu integrieren. Um das verbindungsstabilitätsfeature testen zu können, benötigen Sie eine Möglichkeit zum Abfangen von Abfragen, die Entity Framework in SQL Server sendet, und Ersetzen Sie die SQL Server-Antwort mit einem Ausnahmetyp, der in der Regel vorübergehend ist.

Sie können auch Abfangen der Abfrage verwenden, um eine bewährte Methode für Cloud-Anwendungen zu implementieren: Protokoll, die die Latenz und Erfolg oder Fehler aller externer Aufrufe Dienste, wie z. B. Datenbankdienste.

In diesem Abschnitt des Tutorials verwenden Sie die Entity Framework [ *Abfangfunktion* ](https://msdn.microsoft.com/data/dn469464) sowohl für die Protokollierung und zum Simulieren von vorübergehender Fehlern.

### <a name="create-a-logging-interface-and-class"></a>Erstellen Sie eine protokollierungsschnittstelle und -Klasse

Eine bewährte Methode für die Protokollierung ist, verwenden sie dazu eine [ `interface` ](https://msdn.microsoft.com/library/ms173156.aspx) statt Aufrufe von fest zu programmieren `System.Diagnostics.Trace` oder eine Protokollierungsklasse. Die erleichtert es Ihrer Protokollierungsmechanismus später zu ändern, wenn Sie dies tun müssen. In diesem Abschnitt werden Sie also die protokollierungsschnittstelle und eine Klasse zur Implementierung erstellen.

Basierend auf dem oben genannten Verfahren, haben Sie heruntergeladen und geöffnet, die **WingtipToys** in Visual Studio beispielsanwendung.

1. Erstellen Sie einen Ordner in der **WingtipToys** Projekt, und nennen Sie sie *Protokollierung*.
2. In der *Protokollierung* Ordner, erstellen Sie eine Klassendatei mit dem Namen *ILogger.cs* , und Ersetzen Sie den Standardcode durch folgenden Code:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample4.cs)]

   Die Schnittstelle enthält drei Ablaufverfolgungsstufen an, dass die relative Wichtigkeit der Protokolle und entwickelt, um die latenzzeitinformationen für externe Dienst-Aufrufe wie z. B. Datenbankabfragen bieten ein. Die Protokollierungsmethoden haben Überladungen, mit denen Sie die Ausnahme übergeben. Dies ist so, dass Ausnahmeinformationen einschließlich Stack Trace und der inneren Ausnahmen zuverlässig von der Klasse protokolliert wird, die implementiert die Schnittstelle, statt die standardremotedatenbank, die in jedem Methodenaufruf der Protokollierung in der gesamten Anwendung ausgeführt wird.  
  
   Die `TraceApi` Methoden können Sie die Latenz von jedem Aufruf an einen externen Dienst z. B. SQL-Datenbank nachverfolgen.
3. In der *Protokollierung* Ordner, erstellen Sie eine Klassendatei mit dem Namen *Logger.cs* , und Ersetzen Sie den Standardcode durch folgenden Code:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample5.cs)]

Die Implementierung verwendet `System.Diagnostics` Sie die Ablaufverfolgung. Dies ist ein integriertes Feature von .NET und das Generieren und nutzen Sie diese Daten erleichtert. Es gibt viele &quot;Listener&quot; können Sie mit `System.Diagnostics` Ablaufverfolgung, Protokolle, Dateien, z. B. schreiben oder um sie in Windows Azure Blob Storage zu schreiben. Sehen Sie einige der Optionen und Links zu anderen Ressourcen für Weitere Informationen im [zur Problembehandlung von Windows Azure-Websites in Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio). In diesem Tutorial nur analysieren Sie die Protokolle im Visual Studio **Ausgabe** Fenster.

In einer produktionsanwendung sollten Sie für den Einsatz als von der Ablaufverfolgung Frameworks `System.Diagnostics`, und die `ILogger` Schnittstelle ist es relativ einfach, einen anderen Nachverfolgungsmechanismus wechseln, wenn Sie dies tun möchten.

### <a name="create-interceptor-classes"></a>Erstellen Sie Interceptor-Klassen

Als Nächstes erstellen Sie die Klassen, denen das Entity Framework aufrufen wird jedes Mal, wenn darauf eine Abfrage an die Datenbank, eine vorübergehende Fehler zu simulieren und zu Protokollierung durchführen gesendet. Diese Interceptor-Klassen werden müssen, aus der `DbCommandInterceptor` Klasse. Sie schreiben Sie Außerkraftsetzen von Methoden, die automatisch aufgerufen werden, wenn die Abfrage gerade ausgeführt werden. In diesen Methoden können Sie untersuchen oder melden Sie sich die Abfrage, die an die Datenbank gesendet werden, und Sie können die Abfrage ändern, bevor sie auf die Datenbank gesendet wird oder etwas Zurückgeben von Entity Framework selbst ohne die Abfrage auch in der Datenbank zu übergeben.

1. Um den Interceptor-Klasse zu erstellen, die alle SQL-Abfrage protokollieren wird, bevor sie auf die Datenbank gesendet wird, erstellen Sie eine Klassendatei namens *InterceptorLogging.cs* in die *Logik* Ordner und Ersetzen Sie die Standardeinstellung von code mit der folgender Code:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample6.cs)]

   Für erfolgreiche Abfragen oder Befehle schreibt dieser Code ein Information-Protokoll mit Informationen zur Latenz. Für Ausnahmen wird ein Fehlerprotokoll erstellt.
2. Zum Erstellen der Interceptor-Klasse, die dummy vorübergehende Fehler bei der Eingabe generiert &quot;auslösen&quot; in die **Namen** Textfeld auf der Seite mit dem Namen *AdminPage.aspx*, erstellen Sie eine Klasse Datei mit dem Namen *InterceptorTransientErrors.cs* in die *Logik* Ordner und Ersetzen Sie die Standardeinstellung von code durch den folgenden Code:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample7.cs)]

    Dieser code nur überschreibt die `ReaderExecuting` -Methode, die für Abfragen aufgerufen wird, die mehrere Datenzeilen zurückgeben kann. Wenn Sie, überprüfen Sie die resilienz von Verbindungen für andere Typen von Abfragen möchten, können Sie auch überschreiben die `NonQueryExecuting` und `ScalarExecuting` Methoden, wie die Protokollierung-Interceptor ist.  
  
   Später können Sie melden Sie sich als der "Admin" ist, und wählen Sie die **Admin** Link auf der oberen Navigationsleiste. Klicken Sie auf die *AdminPage.aspx* Seite Sie ein Produkt fügen, mit dem Namen &quot;auslösen&quot;. Der Code erstellt eine dummy-SQL-Datenbank-Ausnahme für Fehlernummer 20, eines Typs in der Regel vorübergehend bezeichnet. Andere Fehlernummern, die derzeit als vorübergehend erkannt werden 64, 233, 10053, 10054, 10060, 10928, 10929, 40197, 40501 und 40613, aber diese unterliegen in neuen Versionen von SQL-Datenbank. Das Produkt wird umbenannt in "TransientErrorExample", die Sie, im Code befolgen können die *InterceptorTransientErrors.cs* Datei.  
  
   Der Code gibt die Ausnahme von Entity Framework, anstatt die Abfrage ausgeführt, und übergeben die Ergebnisse zurück. Die vorübergehende Ausnahme zurückgegeben *vier* Zeiten, und dann der Code mit dem normalen Verfahren übergeben Sie die Abfrage an die Datenbank wiederhergestellt wird.

    Da alles, was protokolliert wird, müssen Sie möglicherweise feststellen, dass Entity Framework versucht wird, um die Abfrage viermal auszuführen, bevor Sie erfolgreich, und des einzigen Unterschied in der Anwendung, dass es zum Rendern einer Seite mit Abfrageergebnissen länger dauert.  
  
   Die Anzahl der Häufigkeit, mit die dem Entity Framework erneut auszuführen ist konfigurierbar. der Code gibt viermal aus, da dies der Standardwert für die Ausführungsrichtlinie für die SQL-Datenbank ist. Wenn Sie die Ausführungsrichtlinie ändern, mussten Sie auch ändern hier den Code ein, der angibt, wie oft ein vorübergehender Fehler generiert wird. Sie können auch den Code, um weitere Ausnahmen zu generieren, sodass Entity Framework eine Ausnahme auslöst, Ändern der `RetryLimitExceededException` Ausnahme.
3. In *"Global.asax"*, fügen Sie die folgenden using-Anweisungen:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample8.cs)]
4. Dann fügen die hervorgehobenen Zeilen, die `Application_Start` Methode:  

    [!code-csharp[Main](aspnet-web-forms-connection-resiliency-and-command-interception/samples/sample9.cs?highlight=17-20)]

Diese Codezeilen sind, was bewirkt, dass den Interceptor Code ausgeführt werden, wenn Entity Framework Abfragen an die Datenbank sendet. Beachten Sie, dass, da Sie separate Interceptor-Klassen für vorübergehende Fehler Simulation erstellt und Protokollierung können Sie unabhängig voneinander aktivieren und deaktivieren Sie diese.   
  
 Können Sie mithilfe von Interceptors Hinzufügen der `DbInterception.Add` Methode eine beliebige Stelle im Code; es muss keine in der `Application_Start` Methode. Eine weitere Option, wenn Sie abfangfunktionen in hinzugefügt haben die `Application_Start` Methode wäre zu aktualisieren, oder fügen Sie die Klasse, die mit dem Namen *WingtipToysConfiguration.cs* , und fügen Sie im oben stehenden Code am Ende des Konstruktors für den `WingtipToysbConfiguration` Klasse.

Ganz egal, wo Sie diesen Code, einfügen, werden Sie darauf achten, nicht zum Ausführen `DbInterception.Add` für den gleichen Interceptor mehr als einmal, oder Sie zusätzliche interceptorinstanzen erhalten. Z. B. Wenn Sie den Interceptor Protokollierung zweimal hinzufügen, sehen Sie zwei Protokolle für alle SQL-Abfrage.

Interceptors werden in der Reihenfolge Ihrer Registrierung ausgeführt (die Reihenfolge, in der `DbInterception.Add` Methode wird aufgerufen). Die Reihenfolge kann relevant sein, je nachdem, was Sie in der Interceptor tun. Beispielsweise kann ein Interceptor ändern den SQL-Befehl, die es, in abruft der `CommandText` Eigenschaft. Wenn sie den SQL-Befehl geändert wird, erhalten der nächsten Interceptor den geänderten SQL-Befehl, nicht den ursprünglichen SQL-Befehl.

Sie haben die simulationscode vorübergehender Fehler in einer Weise geschrieben, die dazu führen, dass vorübergehende Fehler durch einen anderen Wert eingeben, in der Benutzeroberfläche ermöglicht. Als Alternative könnten Sie den Interceptor-Code, um die Reihenfolge der vorübergehende Ausnahmen immer zu generieren, ohne eine Überprüfung auf einen bestimmten Parameterwert schreiben. Anschließend können Sie den Interceptor hinzufügen, nur, wenn Sie vorübergehende Fehler generieren möchten. Wenn Sie dies tun, nicht jedoch Hinzufügen des Interceptors bis, nach Abschluss der Initialisierung der Datenbank. Führen Sie das heißt, mindestens eine Datenbank-Vorgang wie z. B. eine Abfrage auf einem von Ihrem Entitätenmengen auf, bevor Sie beginnen, vorübergehende Fehler generieren. Das Entity Framework führt mehrere Abfragen während der Initialisierung der Datenbank, und sie sind nicht in einer Transaktion ausgeführt, sodass Fehler während der Initialisierung den Kontext, der in einem inkonsistenten Zustand versetzen verursachen könnte.

## <a name="test-logging-and-connection-resiliency"></a>Protokollierung und Test-verbindungsresilienz

1. Drücken Sie in Visual Studio **F5** zum Ausführen der Anwendung im Debugmodus befindet, und klicken Sie dann melden Sie sich als "Admin", "Pa$ $Wort" als Kennwort verwenden.
2. Wählen Sie **Admin** in der Navigationsleiste oben.
3. Geben Sie ein neues Produkt mit dem Namen "Throw" mit der entsprechenden Beschreibung "," Price "und" Image-Datei ein.
4. Drücken Sie die **Produkt hinzufügen** Schaltfläche.  
   Sie werden feststellen, dass der Browser ist nicht mehr reagiert für einige Sekunden, während der Entity Framework die Abfrage mehrere Male wiederholt. Der erste Wiederholungsversuch erfolgt sehr schnell, und klicken Sie dann die Wartezeit vor jedem Neuversuch zusätzliche erhöht. Dieser Prozess vor jedem erneuten Versuch aufgerufen wird, beim Warten auf mehr *exponentiell ansteigende Wartezeiten* .
5. Warten Sie, bis die Seite nicht mehr Atttempting geladen ist.
6. Beenden Sie das Projekt, und sehen Sie sich Visual Studio **Ausgabe** Fenster aus, um die Ausgabe der Ablaufverfolgung finden Sie unter. Sie finden die **Ausgabe** Fenster durch Auswahl **Debuggen**  - &gt; **Windows**  - &gt;  **Ausgabe**. Sie müssen möglicherweise über verschiedene Protokolle, die geschrieben werden, indem Sie den Logger zu scrollen.  
  
   Beachten Sie, dass Sie, die tatsächlichen SQL-Abfragen an die Datenbank gesendet sehen können. Sie sehen einige anfängliche Abfragen und Befehle, die Entity Framework ermöglicht wird, um den ersten Schritten überprüfen die Datenbank-Version und die Migration der Verlaufstabelle.   
    ![Ausgabefenster](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image1.png)   
   Beachten Sie, dass Sie diesen Test nicht wiederholt werden können, es sei denn, Sie die Anwendung zu beenden und starten Sie ihn neu. Wenn Sie die resilienz von Verbindungen mehrere Male in einer einzelnen Ausführung der Anwendung testen möchten, könnten Sie Code zum Zurücksetzen des Zählers Fehler im Schreiben `InterceptorTransientErrors` .
7. Um den Unterschied die Ausführungsstrategie (wiederholungsrichtlinie) sendet, Kommentar der `SetExecutionStrategy` in Zeile *WingtipToysConfiguration.cs* Datei die *Logik* Ordner die **Admin**  Seite im Debugmodus erneut, und fügen Sie das Produkt mit dem Namen &quot;auslösen&quot; erneut aus.  
  
   Dieses Mal hält der Debugger zur ersten generierte Ausnahme, sofort verwendet werden, wenn er versucht, die Abfrage beim ersten ausführen.  
    ![Debuggen - Details anzeigen](aspnet-web-forms-connection-resiliency-and-command-interception/_static/image2.png)
8. Heben Sie die auskommentierung der `SetExecutionStrategy` Zeile in der *WingtipToysConfiguration.cs* Datei.

## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie gesehen, wie eine Web Forms-beispielanwendung zur Unterstützung von verbindungsresilienz und Abfangen von Befehlen ändern wird.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie verbindungsresilienz und Abfangen von Befehlen in ASP.NET Web Forms überprüft haben, lesen Sie das ASP.NET Web Forms-Thema [asynchroner Methoden in ASP.NET 4.5](../performance-and-caching/using-asynchronous-methods-in-aspnet-45.md). Das Thema vermittelt Ihnen die Grundlagen der Erstellung einer asynchronen ASP.NET Web Forms-Anwendung mithilfe von Visual Studio.
