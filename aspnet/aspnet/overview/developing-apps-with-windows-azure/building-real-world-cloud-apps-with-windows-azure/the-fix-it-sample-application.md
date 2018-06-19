---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Anhang: Die Korrektur es Beispielanwendung (Real-World Cloud Apps with Azure erstellen) | Microsoft Docs'
author: MikeWasson
description: Die Building Real World Cloud Apps with Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 9a1fa36b34c4783b101bb27bc6931241e9251e10
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876474"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Anhang: Die Korrektur es Beispielanwendung (Real-World Cloud Apps with Azure erstellen)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Laden Sie das Update, das es Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> Die **Building Real World Cloud Apps with Azure** e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die Ihnen helfen können erfolgreich ausgeführt entwickeln Web-apps für die Cloud. Weitere Informationen zu e-Book herunterladen, finden Sie unter [im Kapitel über das erste](introduction.md).


Dieser Anhang, um die Building Real World Cloud Apps with Azure-e-Book enthält die folgenden Abschnitte, die zusätzliche Informationen über die korrigieren-beispielanwendung bereitstellen, die Sie herunterladen können:

- [Bekannte Probleme](#knownissues)
- [Bewährte Methoden](#bestpractices)
- [Gewusst wie: Ausführen der app aus Visual Studio auf dem lokalen computer](#run-in-vs)
- [Bereitstellen von der Basis-app auf Azure App Service-Web-Apps mithilfe von Windows PowerShell-Skripts](#deploybase)
- [Problembehandlung bei Windows PowerShell-Skripts](#troubleshooting)
- [Gewusst wie: Bereitstellen der app mit der Warteschlange verarbeiten, um Azure App Service-Web-Apps und ein Azure-Cloud-Dienst](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Bekannte Probleme

Die app zu beheben wurde ursprünglich entwickelt, um so weit wie möglich einiger der Muster in diesem e-Book dargestellt zu veranschaulichen. Allerdings seit dem e-Book zum Erstellen von realen apps ist, unterzogen werden wir den Code korrigieren, eine Überprüfung und Testprozess ähnelt, was wir für veröffentlichte Testsoftware machen würde. Stellten wir eine Reihe von Problemen und wie bei jeder Anwendung realen einige davon behoben, und einige davon wird verzögert, auf eine höhere Version.

Die folgende Liste enthält Probleme, die behoben werden sollten, in einer produktionsanwendung, jedoch für ein Grund oder anderer wir uns nicht an die Adresse in der ursprünglichen Version der beispielanwendung korrigieren entschieden.

### <a name="security"></a>Sicherheit

- Stellen Sie sicher, dass Sie eine Aufgabe zu einem nicht vorhandenen Besitzer zuweisen können.
- Stellen Sie sicher, dass nur Sie können anzeigen und Ändern von Aufgaben, die Sie erstellt oder Ihnen zugewiesen sind.
- Verwenden Sie HTTPS für Anmeldeseiten und Authentifizierungscookies.
- Geben Sie ein Zeitlimit dafür Authentifizierungscookies.

### <a name="input-validation"></a>Validierung von Benutzereingaben

Im Allgemeinen dazu Produktions-app eingegebenen Überprüfung als die app zu beheben. Z. B. die Bildgröße / bildupdates zulässige Dateigröße für Upload beschränkt werden soll.

### <a name="administrator-functionality"></a>Administrator-Funktion

Ein Administrator sollte den Besitz auf vorhandene Tasks ändern können. Der Ersteller eines Tasks verbleiben z. B. möglicherweise das Unternehmen verlassen niemand mit Autorität für die Aufgabe beibehalten, es sei denn, Sie administrativer Zugriff aktiviert ist.

### <a name="queue-message-processing"></a>Queue-Nachrichtenverarbeitung

Warteschlange der Nachrichtenverarbeitung in der app zu beheben wurde entwickelt, einfach gehalten, um veranschaulichen des Musters Warteschlange anwendungsorientierte Arbeit mit einem Minimum von Code verwendet werden. Diese einfachen Code würde nicht dessen Fähigkeitsgrad für eine tatsächliche produktionsanwendung.

- Der Code kann nicht garantiert werden, dass jede warteschlangennachricht höchstens einmal verarbeitet werden. Wenn Sie aus der Warteschlange eine Nachricht erhalten, wird ein Zeitlimit, während derer die Nachricht für andere warteschlangenlistenern nicht sichtbar ist. Wenn das Timeout abläuft, bevor die Nachricht gelöscht wird, wird die Nachricht erneut angezeigt. Aus diesem Grund ist eine workerrolleninstanz Verarbeiten einer Nachricht viel Zeit benötigt, es theoretisch möglich, dass dieselbe Nachricht zweimal verarbeitet werden führt eine doppelte Aufgabe in der Datenbank. Weitere Informationen zu diesem Problem finden Sie unter [mithilfe von Azure-Speicher-Warteschlangen](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- Die Warteschlange Abruflogik kostengünstiger, möglicherweise durch Batchverarbeitung beim Abruf der Nachricht. Jedes Mal, wenn Sie rufen [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), Transaktion Kosten. Rufen Sie stattdessen [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (Beachten Sie den Plural "), die mehrere Nachrichten in einer einzelnen Transaktion ruft. Die Transaktionskosten für Azure-Speicherwarteschlangen sehr gering sind, damit die Auswirkungen auf Kosten nicht erhebliche in den meisten Szenarien ist.
- Die enge Schleife in der Warteschlange Nachrichtenverarbeitung Code bewirkt, dass CPU-Affinität, die nicht mit mehreren Kernen VMs effizient verwenden wird. Aufgabenparallelität verwenden ein besserer Entwurf mehrere asynchrone Aufgaben parallel ausgeführt werden.
- Queue-Nachrichtenverarbeitung hat nur rudimentäre Ausnahmebehandlung. Der Code keine verarbeitet, z. B. [nicht verarbeitbare Nachrichten](https://msdn.microsoft.com/library/ms789028.aspx). (Bei der Verarbeitung von Nachrichten eine Ausnahme auslöst, müssen Sie den Fehler protokollieren und die Nachricht zu löschen oder die workerrolle nach Möglichkeit keine erneute Verarbeitung und unbegrenzt die Schleife fortgesetzt.)

### <a name="sql-queries-are-unbounded"></a>SQL-Abfragen sind nicht gebunden

Aktuelle korrigieren-Code wird keine Beschränkung auf die Abfragen für Indexseiten zurückgeben können, wie viele Zeilen platziert. Wenn eine große Menge von Aufgaben in die Datenbank eingegeben wird, kann die Größe der resultierenden Listen empfangen Leistungsprobleme verursachen. Die Lösung besteht darin, Auslagerung implementieren. Ein Beispiel finden Sie unter [sortieren, Filtern und Paging mit Entity Framework in einer ASP.NET MVC-Anwendung](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Ansichtsmodelle empfohlen

Die korrigieren-app verwendet die Entitätsklasse FixItTask um Informationen zwischen dem Controller und die Ansicht zu übergeben. Eine bewährte Methode ist die Verwendung der Modelle anzeigen. Das Domänenmodell (z. B. die FixItTask Entitätsklasse) basiert darauf, dass was für Datenpersistenz, benötigt wird, während ein Ansichtsmodell für die Darstellung von Daten entworfen werden kann. Weitere Informationen finden Sie unter [12 bewährte Methoden für ASP.NET MVC](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Sichere Image-Blob empfohlen

Die app-Stores für korrigieren hochgeladen Bilder als öffentlich ist, was bedeutet, dass jemand auf die URL findet die Bilder zugreifen kann. Die Bilder können anstelle von öffentlichen gesichert werden.

### <a name="no-powershell-automation-scripts-for-queues"></a>Keine PowerShell-Automatisierungsskripts für Warteschlangen

Beispiel-PowerShell-Automatisierungsskripts geschrieben wurden nur für die Basisversion der korrigieren, die vollständig in Azure App Service-Web-Apps ausgeführt wird. Wir noch nicht Skripts bereitgestellt, für das Einrichten und Bereitstellen von für die Web-app sowie Cloud-Service-Umgebung, die für die Verarbeitung der Warteschlange erforderlich.

### <a name="special-handling-for-html-codes-in-user-input"></a>Besondere Behandlung für HTML-Codes in Benutzereingaben

ASP.NET wird automatisch verhindert, dass viele Möglichkeiten, die in denen böswillige Benutzer Angriffe Cross-Site scripting versuchen möglicherweise, durch Eingabe von Skript in Benutzer Eingabetext Felder. Und MVC `DisplayFor` zur Anzeige von Task-Hilfsobjekt titles und Anmerkungen zu dieser automatisch HTML-codiert-Werte, die an den Browser gesendet. Aber in einer Produktions-app möchten Sie möglicherweise zusätzliche Maßnahmen ergreifen. Weitere Informationen finden Sie unter [anfordern Validierung in ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Bewährte Methoden

Es folgen einige Probleme, die nach einem behoben wurden, im codereviews erkannt und Testen der ursprünglichen Version der app zu beheben. Einige von dem ursprünglichen Programmierer nicht bewusst bestimmten wird empfohlen, einige einfach verursacht wurden, da der Code, schnell geschrieben wurde und wurde nicht für die veröffentlichte Software vorgesehen. Wir sind hier die Probleme aufgelistet, für den Fall, dass Sie etwas, die wir in dieser Überprüfung gelernt und testen, kann hilfreich sein, anderen Personen, die auch Web-apps entwickeln.

### <a name="dispose-the-database-repository"></a>Verwerfen von Datenbank-repository

Die `FixItTaskRepository` Klasse muss das Entity Framework dispose `DbContext` Instanz. Wir haben dies durch die Implementierung `IDisposable` in die `FixItTaskRepository` Klasse:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Beachten Sie, die automatisch AutoFac beseitigt wird, wird die `FixItTaskRepository` Instanz, daher brauchen wir sie explizit zu verwerfen.

Eine andere Möglichkeit ist, entfernen die `DbContext` Membervariable aus `FixItTaskRepository`, und erstellen Sie stattdessen einen lokalen `DbContext` Variable innerhalb jedes Repository-Methode innerhalb einer `using` Anweisung. Zum Beispiel:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registrieren Sie Singletons als solche mit DI

Da nur eine Instanz des der `PhotoService` Klasse und `Logger` Klasse erforderlich ist, werden diese Klassen werden sollte [als einzelne Instanzen für Abhängigkeitsinjektion registriert](https://code.google.com/p/autofac/wiki/InstanceScope) in *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Sicherheit: Benutzer Fehlerdetails anzeigen

Die ursprüngliche korrigieren-app haben haben eine generische Fehlerseite und an Ihre Seite alle Ausnahmen Blase bis zu der Benutzeroberfläche, damit einige Ausnahmen, z. B. Verbindungsfehlern Datenbank eine vollständige stapelüberwachung angezeigt wird, an den Browser führen können. Ausführliche Fehlerinformationen kann in einigen Fällen durch Angriffe böswilliger Benutzer ermöglichen. Die Lösung besteht darin, die Details der Ausnahme protokollieren und eine Fehlerseite für den Benutzer, der keine Fehlerdetails anzeigen. Die app zu beheben, wurde bereits protokollieren und um eine Fehlerseite anzeigen, wir hinzugefügt `<customErrors mode=On>` in der Datei "Web.config".

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Standardmäßig bewirkt dies, dass *Views\Shared\Error.cshtml* für Fehler angezeigt werden soll. Sie können anpassen, *Error.cshtml* oder erstellen Sie eine eigene Seitenansicht Fehler und Hinzufügen einer `defaultRedirect` Attribut. Sie können auch verschiedene Fehlerseiten für bestimmte Fehler angeben.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Sicherheit: kann nur bearbeitet werden, durch den Ersteller ein Tasks

Die Dashboard-Indexseite zeigt nur die Aufgaben erstellt, indem der Benutzer angemeldet, aber ein böswilliger Benutzer könnte eine URL erstellen, mit der ID eines anderen Benutzers-Vorgang. Wir haben in Code hinzugefügt *DashboardController.cs* in diesem Fall Fehler 404 zurückgegeben:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Behalten Sie nicht Ausnahmen

Die ursprüngliche korrigieren-app nur hat null zurückgegeben, nach der Anmeldung einer Ausnahme, die auf einer SQL-Abfrage zurückzuführen:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Dadurch würde, es dem Benutzer zu suchen, als ob die Abfrage erfolgreich war, aber keine Zeilen zurückgeben. Lösung besteht darin, die Ausnahme nach dem Abfangen und Protokollierung erneut ausgelöst:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Ausnahmen Sie alle in Workerrollen

Nicht behandelten Ausnahmen in einer workerrolle führt dazu, dass des virtuelle Computers neu gestartet werden, damit alles umschließen soll Ihnen in einem Try / Catch-Block, und alle Ausnahmen zu behandeln.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Geben Sie die Länge für Zeichenfolgeneigenschaften in Entitätsklassen

Um einfacher Code anzuzeigen, die ursprüngliche Version der app korrigieren Längen für die Felder der Entität FixItTask angegeben haben, und daher wurden sie als varchar(max) in der Datenbank definiert. Daher würde die Benutzeroberfläche nahezu jede Menge an Eingabe akzeptiert. Angabe Längen legt Grenzwerte, die sowohl für Benutzer gelten Eingaben in die Webseite und die Größe der Spalte in der Datenbank erforderlich:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Private Member als schreibgeschützt markiert wird, wenn diese zu erwarten sind nicht so ändern Sie

Beispielsweise ist in der `DashboardController` eine Instanz der Klasse `FixItTaskRepository` wird erstellt und ist nicht dazu gedacht, zu ändern, damit wir es als definiert [Readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Verwenden Sie die Liste. Any() anstelle der Liste. Count() &gt; 0

Verwenden, wenn Sie Sie von Interesse ist, gibt an, ob ein oder mehrere Elemente in einer Liste den angegebenen Kriterien entsprechen, die [alle](https://msdn.microsoft.com/library/bb534972.aspx) -Methode, weil sie zurückgibt, als ein Element, das die Kriterien einpassen gefunden wird, während die `Count` Methode hat immer durchlaufen über jedes Element. Das Dashboard *Index.cshtml* Datei hatte ursprünglich den folgenden Code:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Wir es hierbei geändert:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generieren von URLs in MVC-Ansichten mithilfe von MVC-Hilfsprogrammen

Für die **erstellen Sie ein Update er** Schaltfläche auf der Startseite, beheben Sie es-app, die schwer codiert ein Ankerelement:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Für Sicht/Aktionslinks sieht es ist besser, verwenden Sie die [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) HTML-Hilfsobjekt, beispielsweise:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Verwenden von Task.Delay anstelle Thread.Sleep Worker-Rolle

Legt die neue Projektvorlage `Thread.Sleep` im Beispiel für den Code für eine workerrolle, aber den Thread, in den Energiesparmodus verursacht kann dazu führen, dass den Threadpool weitere unnötige Threads zu starten. Sie können vermeiden, die mit [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) stattdessen.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Vermeiden Sie Async "void"

Wenn eine asynchrone Methode einen Wert zurückgeben, nicht zurückgeben eine `Task` Typ statt `void`.

Dieses Beispiel stammt aus dem `FixItQueueManager` Klasse: 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Verwenden Sie `async void` nur für den Ereignishandler der obersten Ebene. Wenn Sie eine Methode als definieren `async void`, kann der Aufrufer nicht **"await"** die Methode oder die Methode löst Ausnahmen abfangen. Weitere Informationen finden Sie unter [Best Practices bei der asynchronen Programmierung](https://msdn.microsoft.com/magazine/jj991977.aspx). 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Verwenden Sie ein Abbruchtoken, das von Worker-Rolle Schleife unterbrechen

In der Regel die **ausführen** Methode für eine workerrolle enthält eine Endlosschleife. Wenn die Worker-Rolle beendet wird, die [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) -Methode aufgerufen wird. Sollten Sie diese Methode verwenden, um die Arbeit abzubrechen, die in erfolgt die **ausführen** -Methode, und beenden ordnungsgemäß. Andernfalls kann der Prozess gerade ein Vorgang beendet werden.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Ablehnen der automatischen MIME-Sniffing-Prozedur

In einigen Fällen meldet Internet Explorer einen MIME-Typ mit dem Webserver unterscheidet. Z. B. findet Internet Explorer HTML Inhalt in einer Datei mit dem HTTP-Antwort-Header Content-Type: Text/Plain, Internet Explorer ermittelt, dass der Inhalt als HTML gerendert werden soll. Leider kann diese "MIME-Ermittlung" Sicherheitsprobleme für Server, auf denen nicht vertrauenswürdigen Inhalt auch führen. Um dieses Problem zu beheben, Internet Explorer 8 machte eine Reihe von Änderungen für die Bestimmung Code MIME-Typ und ermöglicht Entwicklern von [Ablehnen der MIME-Ermittlung](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). Der folgende Code wurde hinzugefügt, um die *"Web.config"* Datei.

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Aktivieren Sie Bündelung und Minimierung

Wenn Visual Studio ein neues Webprojekt erstellt, wird Bündelung und Minimierung von JavaScript-Dateien nicht standardmäßig aktiviert. Wir haben eine Codezeile in BundleConfig.cs hinzugefügt:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Legen Sie einen Timeout für Authentifizierungscookies

Standardmäßig werden Authentifizierungscookies in zwei Wochen ablaufen. Ein kürzeres ist sicherer. Sie können diese Einstellung in *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Gewusst wie: Ausführen der app aus Visual Studio auf dem lokalen computer

Es gibt zwei Möglichkeiten zum Ausführen der app zu beheben:

- Führen Sie die Basis-Anwendung, die neue Vorgänge direkt in der SQL-Datenbank geschrieben.
- Führen Sie die Anwendung, die mit einer Warteschlange sowie ein Back-End-Dienst um Aufgaben zu erstellen. Das Muster für die Warteschlange wird in diesem Kapitel beschrieben [Warteschlange anwendungsorientierte Arbeit Muster](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Führen Sie die Basis-Anwendung

1. Installieren Sie [Visual Studio 2013 oder Visual Studio 2013 Express für Web](https://www.visualstudio.com/downloads).
2. Installieren der [Azure SDK für .NET für Visual Studio 2013.](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. Laden Sie die ZIP-Datei aus der [MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. Mit der rechten Maustaste in der ZIP-Datei im Datei-Explorer klicken Sie auf Eigenschaften, und klicken Sie im Eigenschaftenfenster auf zulassen.
5. Entzippen Sie die Datei an.
6. Doppelklicken Sie auf die SLN-Datei, um Visual Studio zu starten.
7. Klicken Sie im Menü Extras auf Bibliothekspaket-Manager, und klicken Sie dann auf Paket-Manager-Konsole.
8. Klicken Sie in der Paket-Manager-Konsole (PMC) auf wiederherstellen.
9. Beenden Sie Visual Studio.
10. Starten Sie die [Azure-Speicheremulator](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx).
11. Visual Studio neu starten, öffnen die Projektmappendatei Sie im vorherigen Schritt geschlossen.
12. Stellen Sie sicher, dass das FixIt-Projekt als Startprojekt festgelegt ist, und drücken Sie dann STRG + F5, um das Projekt auszuführen.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Führen Sie die Anwendung mit der Verarbeitung der Warteschlange

1. Befolgen Sie die Anweisungen für [führen Sie die Basis Anwendung](#runbase), und schließen Sie den Browser, und schließen Sie Visual Studio.
2. Starten Sie Visual Studio mit Administratorrechten aus. (Sie müssen mithilfe der Azure-Serveremulator und, die sind Administratorrechte erforderlich.)
3. In der Anwendung *"Web.config"* in der Datei die *MyFixIt* Projekt (das Webprojekt), ändern Sie den Wert des `appSettings/UseQueues` auf "True": 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Wenn die [Azure-Speicheremulator](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) ist nicht immer noch ausgeführt, starten Sie ihn erneut.
5. Das Webprojekt FixIt und das Projekt MyFixItCloudService gleichzeitig ausgeführt.

    Verwenden von Visual Studio 2013:

   1. Drücken Sie F5, um das Projekt FixIt auszuführen.
   2. In **Projektmappen-Explorer**mit der rechten Maustaste auf das MyFixItCloudService-Projekt, und klicken Sie dann auf **Debuggen** -- **neue Instanz starten**.

      Verwenden von Visual Studio 2013 Express für Web:

   3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste der FixIt-Projektmappe, und wählen **Eigenschaften**.
   4. Wählen Sie **mehrere Startprojekte**...
   5. In der **Aktion** wählen Sie die Dropdownliste unter MyFixIt und MyFixItCloudService, **starten**.
   6. Klicken Sie auf **OK**.
   7. Drücken Sie F5, um beide Projekte auszuführen.

      Wenn Sie das Projekt MyFixItCloudService ausführen, startet Visual Studio Azure-Serveremulator. Abhängig von Ihrer Firewallkonfiguration müssen Sie den Emulator über die Firewall zulassen.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Bereitstellen von der Basis-app auf Azure App Service-Web-Apps mithilfe von Windows PowerShell-Skripts

Zur Veranschaulichung der [automatisieren alles](automate-everything.md) Muster, die app zu beheben, die mithilfe von Skripts, die einer Umgebung in Azure einrichten und Bereitstellen des Projekts in die neue Umgebung angegeben wird. Die folgenden Anweisungen wird erläutert, wie die Skripts verwenden.

Wenn Sie in Azure ausführen, ohne die Verwendung von Warteschlangen möchten und vorgenommenen Änderungen mit Warteschlangen lokal ausführen, stellen Sie sicher, dass Sie den UseQueues AppSetting-Wert wieder auf "false", bevor Sie mit den folgenden Anweisungen festlegen.

Diese Anweisungen wird angenommen, Sie haben bereits heruntergeladen und lokal ausgeführt wird die Projektmappe zu beheben, und dass Sie eine Azure-Konto oder ein Azure-Abonnement besitzen, die Sie sind berechtigt, zu verwalten.

1. Installieren der **Azure PowerShell** Konsole. Anweisungen hierzu finden Sie unter [zum Installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Diese benutzerdefinierte Konsole wird konfiguriert, für die Zusammenarbeit mit Ihrem Azure-Abonnement. Azure-Modul installiert ist, der *Programmdateien* Verzeichnis und wird automatisch bei jeder Verwendung der Azure-PowerShell-Konsole importiert.

    Wenn Sie lieber arbeiten in einem anderen Host-Programm, z. B. Windows PowerShell ISE, achten Sie darauf, verwenden die [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) -Cmdlet zum Importieren von Azure-Modul oder verwenden Sie einen Befehl im Azure-Modul zum Auslösen der automatische Import des Moduls.
2. Starten Sie Azure PowerShell mit der **als Administrator ausführen** Option.
3. Führen Sie die [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) Cmdlet, um die Azure PowerShell-Ausführungsrichtlinie auf `RemoteSigned`. Geben Sie **Y** (für Ja) um die Änderung der Richtlinie abzuschließen.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Mit dieser Einstellung können Sie lokale Skripts ausführen, die nicht digital signiert sind. (Sie können auch die Ausführungsrichtlinie festlegen, um `Unrestricted`, würde die später die Notwendigkeit für den Schritt zum Aufheben der Sperre, aber dies ist aus Sicherheitsgründen nicht empfohlen.)
4. Führen Sie die `Add-AzureAccount` Cmdlet, um PowerShell mit Anmeldeinformationen für Ihr Konto einzurichten.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Diese Anmeldeinformationen laufen nach einer Zeitspanne ab und muss erneut ausgeführt werden. die `Add-AzureAccount` Cmdlet. Wie diese e-Book geschrieben wird, ist das Zeitlimit überschreiten, bevor die Anmeldeinformationen ablaufen 12 Stunden an.
5. Wenn Sie mehrere Abonnements verfügen, verwenden Sie das Cmdlet "Select-AzureSubscription" an das Abonnement, in die testumgebung erstellt werden soll.
6. Importieren Sie ein Verwaltungszertifikat für die gleichen Azure-Abonnement mithilfe der `Get-AzurePublishSettingsFile` und `Import-AzurePublishSettingsFile` Cmdlets. Die erste dieser Cmdlets downloads einer Zertifikatsdatei, und in dem zweiten Ausdruck geben Sie den Speicherort dieser Datei, um es zu importieren. > [!IMPORTANT]
   > Halten Sie die heruntergeladene Datei an einem sicheren Ort oder löschen Sie, wenn Sie mithilfe dieser Option fertig sind, da sie ein Zertifikat enthält, die zum Verwalten Ihrer Azure-Dienste verwendet werden kann.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Das Zertifikat wird verwendet, für einen REST-API-Aufruf, der dem Entwicklungscomputer IP-Adresse erkennt, um eine Firewallregel auf dem SQL-Datenbankserver festzulegen.
7. Führen Sie die [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) Cmdlet (Aliase sind `cd`, `chdir`, und `sl`), navigieren zu dem Verzeichnis, das die Skripts enthält. (sie sind im Verzeichnis der *Automatisierung* im Ordner Lösung korrigieren.) Setzen Sie den Pfad in Anführungszeichen ein, wenn die Namen der Verzeichnisse Leerzeichen enthalten. Um beispielsweise zum Navigieren der `c:\Sample Apps\FixIt\Automation` Directory können Sie den folgenden Befehl eingeben:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Um Windows PowerShell zum Ausführen dieser Skripts zu ermöglichen, verwenden die [Unblock-File](https://go.microsoft.com/fwlink/p/?linkid=294021) Cmdlet. (Die Skripts werden blockiert, weil sie aus dem Internet heruntergeladen wurden.)

    > [!WARNING]
    > Sicherheit – vor der Ausführung `Unblock-File` auf ein Skript oder ausführbare Datei, öffnen Sie die Datei in Editor, überprüfen Sie die Befehle und stellen Sie sicher, dass sie keine bösartigen Code enthalten.

    Beispielsweise der folgende Befehl führt die `Unblock-File` Cmdlet für alle Skripts im aktuellen Verzeichnis.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Um die Web-app für die Basis (keine Warteschlangen verarbeiten) erstellen behoben sie app werden, führen Sie das Skript zum Erstellen der Umgebung.

    Die erforderliche `Name` Parameter gibt den Namen der Datenbank und wird auch verwendet werden, für das Speicherkonto an, die das Skript erstellt. Der Name muss innerhalb der Domäne azurewebsites.NET global eindeutig sein. Wenn Sie einen Namen angeben, die nicht eindeutig sind, z. B. Fixit oder Test (oder sogar wie im Beispiel Fixitdemo) ist die `New-AzureWebsite` tritt ein interner Fehler, die einen Konflikt meldet. Das Skript konvertiert den Namen in zur Einhaltung der Anforderungen an die für die Web-apps, Speicherkonten und Datenbanken vollständig aus Kleinbuchstaben.

    Die erforderliche `SqlDatabasePassword` Parameter gibt das Kennwort für das Administratorkonto ein, die für SQL-Datenbank erstellt werden soll. XML-Sonderzeichen im Kennwort enthalten nicht (&amp; &lt; &gt; ;). Dies ist eine Einschränkung der Art und Weise, in die Skripts geschrieben wurden, keine Einschränkung des Azure.

    Wenn Sie mit dem Namen "Fixitdemo" Web-app erstellen und eine SQL Server-Administratorkennwort des "Passw0rd1" verwenden möchten, können Sie z. B. den folgenden Befehl eingeben:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Der Name muss eindeutig sein, in der Domäne azurewebsites.NET und das Kennwort muss die SQL-Datenbank-Anforderungen für die Kennwortkomplexität entsprechen. (Im Beispiel Passw0rd1 erfüllt die Anforderungen.)

    Beachten Sie, die mit der Befehl beginnt ". \". Zum Vermeiden von böswilligen Ausführung von Skripts erfordert Windows PowerShell an, dass Sie den vollqualifizierten Pfad zur Skriptdatei angeben, wenn Sie ein Skript ausführen. Verwenden Sie einen Punkt an, dass das aktuelle Verzeichnis (".\") oder geben Sie den vollqualifizierten Pfad, z. B.:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Weitere Informationen zum Skript verwenden die `Get-Help` Cmdlet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Sie können die `Detailed`, `Full`, `Parameters`, und `Examples` Parameter des Get-Help-Cmdlet, um die Hilfe zu filtern, die zurückgegeben wird.

    Wenn das Skript fehlschlägt oder Fehler, z. B. "New-AzureWebsite: Rufen Sie Set-AzureSubscription und Select-AzureSubscription zuerst generiert" Sie möglicherweise nicht die Konfiguration von Azure PowerShell abgeschlossen haben.

    Nachdem das Skript beendet wurde, können im Azure-Verwaltungsportal finden in den Ressourcen, die erstellt wurden, entsprechend der [automatisieren alles](automate-everything.md) Kapitel.
10. Verwenden Sie zum Bereitstellen des Projekts FixIt in die neue Azure-Umgebung die *AzureWebsite.ps1* Skript. Zum Beispiel:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Bei der Bereitstellung ausgeführt wird, öffnet der Browser mit korrigieren Sie ihn in Azure ausgeführt wird.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Problembehandlung bei Windows PowerShell-Skripts

Die häufigsten Fehler gefunden wird, wenn diese Skripts ausgeführt werden, im Zusammenhang mit Berechtigungen. Stellen Sie sicher, dass `Add-AzureAccount` und `Import-AzurePublishSettingsFile` erfolgreich waren und, die Sie für die gleichen Azure-Abonnement verwendet. Auch wenn `Add-AzureAccount` wurde erfolgreich möglicherweise müssen Sie es erneut ausführen. Die Berechtigungen von hinzugefügten `Add-AzureAccount` laufen in 12 Stunden ab.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Der Objektverweis wurde nicht auf eine Objektinstanz festgelegt.

Wenn das Skript Fehler zurückgibt, z. B. "Objektverweis nicht auf eine Instanz eines Objekts festgelegt" Was bedeutet, dass Windows PowerShell ein Objekt zu verarbeiten (Dies ist ein null-Verweisausnahme auftreten) kann nicht gefunden werden, führen Sie die `Add-AzureAccount` Cmdlet, und wiederholen Sie das Skript.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: Der Server ist einen internen Fehler aufgetreten.

Die `New-AzureWebsite` Cmdlet gibt einen internen Fehler zurück, wenn der Zugriff auf der Namen in der Domäne azurewebsites.NET nicht eindeutig ist. Um den Fehler zu beheben, verwenden Sie einen anderen Wert für den Namen, die im Name-Parameter ist *neu AzureWebsiteEnv.ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Neustarten des Skripts

Wenn Sie neu starten müssen die *neu AzureWebsiteEnv.ps1* Skript da fehlgeschlagen ist, bevor er die Nachricht "Skript ist abgeschlossen" gedruckt, sollten Sie Ressourcen löschen, die das Skript erstellt, bevor er beendet. Angenommen, wenn das Skript bereits ContosoFixItDemo Web-app und führen Sie das Skript erneut mit dem gleichen Namen erstellt haben, schlägt das Skript fehl, da der Name verwendet wird.

Um zu bestimmen, welche Ressourcen das Skript erstellt werden, bevor er beendet wird, verwenden Sie die folgenden Cmdlets:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Um dieses Cmdlet ausführen, über die Pipeline übergeben den Namen der Datenbank-Server an `Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Um diese Ressourcen zu löschen, verwenden Sie die folgenden Befehle ein. Beachten Sie, wenn Sie den Datenbankserver löschen, automatisch mit dem Server zugeordneten Datenbanken löschen.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Gewusst wie: Bereitstellen der app mit der Warteschlange verarbeiten, um Azure App Service-Web-Apps und ein Azure-Cloud-Dienst

Um Warteschlangen zu aktivieren, stellen Sie die folgende Änderung in der Datei MyFixIt\Web.config. Klicken Sie unter `appSettings`, ändern Sie den Wert des `UseQueues` auf "True": 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Klicken Sie dann die MVC-Anwendung in eine Web-app in Azure App Service, bereitstellen, wie beschrieben [früheren](#deploybase).

Als Nächstes erstellen Sie einen neuen Azure-Cloud-Dienst. Die Skripts enthalten, mit der app zu beheben weder erstellt noch den Cloud-Dienst bereitstellen, damit Sie für dieses Azure-Portal verwenden müssen. Klicken Sie im Portal auf **neu** -- **berechnen** – **Cloud-Dienst** -- **Schnellerfassung**, und geben Sie dann eine URL und einen Speicherort im Rechenzentrum. Verwenden Sie die gleichen Rechenzentrum, wo Sie die Web-app bereitgestellt.

![](the-fix-it-sample-application/_static/image1.png)

Bevor Sie den Cloud-Dienst bereitstellen können, müssen Sie einige der Konfigurationsdateien zu aktualisieren.

In MyFixIt.WorkerRoler\app.config unter `connectionStrings`, ersetzen Sie den Wert, der die `appdb` Verbindungszeichenfolge mit der tatsächlichen Verbindungszeichenfolge für die SQL-Datenbank. Sie können die Verbindungszeichenfolge aus dem Portal abrufen. Klicken Sie im Portal auf **SQL-Datenbanken** - **Appdb** - **Ansicht SQL-Datenbank-Verbindungszeichenfolgen für ADO .NET, ODBC, PHP und JDBC**. Kopieren der ADO.NET-Verbindungszeichenfolge, und fügen Sie den Wert in der Datei "App.config". Ersetzen Sie "{Ihre\_Kennwort\_hier}" durch Ihr Datenbankkennwort. (Wenn Sie die Skripts zum Bereitstellen der MVC-app verwendet, das Datenbankkennwort am angegebenen der `SqlDatabasePassword` Skript Parameter.)

Das Ergebnis sollte wie folgt aussehen:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

In der gleichen Datei MyFixIt.WorkerRoler\app.config unter `appSettings`, ersetzen Sie die zwei Platzhalterwerte für Azure-Speicherkonto.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Sie können den Zugriffsschlüssel aus dem Portal abrufen. Finden Sie unter [zum Verwalten von Speicherkonten](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

Ersetzen Sie im MyFixItCloudService\ServiceConfiguration.Cloud.cscfg die gleichen zwei Platzhalter-Werte für die Azure-Speicherkonto aus.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Jetzt sind Sie bereit für die Bereitstellung von Cloud-Dienst. Klicken Sie im Projektmappen-Explorer, mit der rechten Maustaste des MyFixItCloudService-Projekts, und wählen **veröffentlichen**. Weitere Informationen finden Sie unter "[Bereitstellen der Anwendung in Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", also in Teil 2 von [dieses Lernprogramms](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Vorherige](more-patterns-and-guidance.md)
