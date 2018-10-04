---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
title: 'Anhang: Der Fix It-Beispielanwendung (erstellen realer Cloud-Apps mit Azure) | Microsoft-Dokumentation'
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 1bc333c5-f096-4ea7-b170-779accc21c1a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/the-fix-it-sample-application
msc.type: authoredcontent
ms.openlocfilehash: 6f4fa7cf3746da0a6cdd4bd037fea509d488a59d
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48578015"
---
<a name="appendix-the-fix-it-sample-application-building-real-world-cloud-apps-with-azure"></a>Anhang: Der Fix It-Beispielanwendung (erstellen realer Cloud-Apps mit Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Laden Sie das Update, das es Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4)

> Die **Building Real World Cloud Apps mit Azure** e-Book basiert darauf, dass eine Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Weitere Informationen zu e-Book, finden Sie unter [im ersten Kapitel](introduction.md).


In diesem Anhang zu den Building Real World Cloud Apps mit Azure-e-Book enthält folgende Abschnitte, die zusätzliche Informationen zu den Fix It-beispielanwendung bereitstellen, die Sie herunterladen können:

- [Bekannte Probleme](#knownissues)
- [Bewährte Methoden](#bestpractices)
- [Wie Sie die app in Visual Studio auf Ihrem lokalen Computer ausführen.](#run-in-vs)
- [Die Basis-app in Azure App Service Web Apps bereitstellen, mithilfe der Windows PowerShell-Skripts](#deploybase)
- [Problembehandlung bei der Windows PowerShell-Skripts](#troubleshooting)
- [Gewusst wie: Bereitstellen der app mit einer Warteschlange, die Verarbeitung von Azure App Service-Web-Apps und Azure-Clouddienst](#deployqueues)

<a id="knownissues"></a>
## <a name="known-issues"></a>Bekannte Probleme

Die Fix It-app wurde ursprünglich entwickelt, um zu veranschaulichen, so weit wie möglich einiger der Muster in diesem e-Book angezeigt. Aber da e-Book ist über das Erstellen von WPF-Anwendungen, unterzogen werden wir den Code Fix It, eine Überprüfung und Testprozess ähnelt, was wir, für die veröffentlichte Software tun würde. Wir haben festgestellt, dass eine Reihe von Problemen, und klicken Sie wie bei jeder Real-World-Anwendung, einige davon wurde behoben, und einige davon wir auf eine höhere Version verzögert.

Die folgende Liste enthält Probleme, die berücksichtigt werden sollten, in einer produktionsanwendung, aber aus irgendeinem Grund eine andere, die wir uns, nicht auf die Adresse in der ersten Version von der Fix It-beispielanwendung entschieden.

### <a name="security"></a>Sicherheit

- Stellen Sie sicher, dass Sie können eine Aufgabe zu einem nicht vorhandenen Besitzer zuweisen.
- Stellen Sie sicher, dass nur Sie können anzeigen und ändern Sie die Aufgaben, die Sie erstellt haben oder Ihnen zugewiesen sind.
- Verwenden Sie HTTPS für die Anmeldeseiten und Authentifizierungscookies an.
- Geben Sie ein Zeitlimit für den Authentifizierungs-Cookies.

### <a name="input-validation"></a>Eingabeüberprüfung

Im Allgemeinen eine Produktions-app dazu mehr eingabeüberprüfung wie bei der app zu beheben. Z. B. die Größe des Abbilds / image zulässige Dateigröße für Uploads beschränkt werden soll.

### <a name="administrator-functionality"></a>Administrator-Funktion

Ein Administrator sollte den Besitz an vorhandenen Aufgaben ändern können. Der Ersteller eines Tasks verbleiben z. B. möglicherweise das Unternehmen, sodass niemand Autorität für die Aufgabe darin, es sei denn, Sie administrativer Zugriff aktiviert ist.

### <a name="queue-message-processing"></a>Die Verarbeitung von Warteschlangennachrichten

Warteschlangen-Nachricht, die in der Fix It-app verarbeiten soll einfach gehalten, damit das veranschaulichen des Musters warteschlangenorientierte mit einem minimum an Code verwendet werden. Diesem einfache Code wäre nicht ausreichend für eine Anwendung für die eigentliche Produktion.

- Der Code garantiert nicht, dass jede warteschlangennachricht höchstens einmal verarbeitet wird. Wenn Sie aus der Warteschlange eine Nachricht erhalten, besteht ein Timeout, dem die Nachricht für andere warteschlangenlistener nicht mehr sichtbar ist. Wenn das Timeout abläuft, bevor die Nachricht gelöscht wird, wird die Nachricht erneut angezeigt. Aus diesem Grund, wenn eine workerrolleninstanz sehr lange, die Verarbeitung einer Nachricht verbringt, ist es theoretisch möglich, dass die Nachricht zweimal verarbeitet, was zu einer doppelten Aufgabe in der Datenbank. Weitere Informationen zu diesem Problem finden Sie unter [mithilfe von Azure Storage-Warteschlangen](https://msdn.microsoft.com/library/ff803365.aspx#sec7).
- Die Warteschlange Abruflogik kostengünstiger, möglicherweise durch die Batchverarbeitung beim Abruf der Nachricht. Jedes Mal, wenn Sie aufrufen [CloudQueue.GetMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessageasync.aspx), fallen Transaktionskosten. Stattdessen rufen Sie [CloudQueue.GetMessagesAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.getmessagesasync.aspx) (Beachten Sie die würfelvorgänge "), die mehrere Nachrichten in einer einzelnen Transaktion abruft. Die Transaktionskosten für Azure Storage-Warteschlangen sind sehr gering, daher die Auswirkungen auf die Kosten nicht wesentliche in den meisten Szenarien.
- Der enge Schleife in der Warteschlange Nachrichtenverarbeitung Code bewirkt, dass CPU-Affinität, die nicht mit mehreren Kernen virtuelle Computer effizient verwenden wird. Ein besseres Design würden Aufgabenparallelität verwenden, um mehrere asynchrone Aufgaben parallel auszuführen.
- Die Verarbeitung von Warteschlangennachrichten hat nur rudimentäre Ausnahmebehandlung. Z. B. der Code nicht behandeln, [nicht verarbeitbare Nachrichten](https://msdn.microsoft.com/library/ms789028.aspx). (Bei der Verarbeitung von Nachrichten eine Ausnahme auslöst, müssen Sie den Fehler protokollieren und die Nachricht, löschen oder versucht der Worker-Rolle, die erneute Verarbeitung und die Schleife wird auf unbestimmte Zeit fortgesetzt.)

### <a name="sql-queries-are-unbounded"></a>SQL-Abfragen sind nicht gebunden

Aktuelle Fix It-Code wird keine Beschränkung für wie viele Zeilen, die die Abfragen für Indexseiten zurückgeben können. Die Größe der resultierenden Listen empfangen kann eine große Anzahl von Aufgaben in der Datenbank eingegeben wird, Leistungsprobleme auftreten. Die Lösung besteht darin, Paging implementieren. Ein Beispiel finden Sie unter [sortieren, Filtern und Paging mit Entity Framework in einer ASP.NET MVC-Anwendung](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application.md).

### <a name="view-models-recommended"></a>Anzeigemodelle, die empfohlen

Die Fix It-app verwendet die Entitätsklasse FixItTask, um Informationen zwischen dem Controller und die Ansicht zu übergeben. Eine bewährte Methode ist die Verwendung der anzeigemodelle. Das Domänenmodell (z. B. die FixItTask Entity-Klasse) wurde entwickelt, um was für Dauerhaftigkeit von Daten benötigt wird, während ein Ansichtsmodell für die Darstellung von Daten entworfen werden kann. Weitere Informationen finden Sie unter [12 bewährte Methoden für ASP.NET MVC](http://codeclimber.net.nz/archive/2009/10/27/12-asp.net-mvc-best-practices.aspx).

### <a name="secure-image-blob-recommended"></a>Empfohlen, sicheres Image-blob

Den Fix It-app-Stores von hochgeladenen Bildern als öffentlich, was bedeutet, dass jemand auf die URL findet die Bilder zugreifen kann. Die Images können anstelle von öffentlichen gesichert werden.

### <a name="no-powershell-automation-scripts-for-queues"></a>Keine PowerShell-Automation-Skripts für Warteschlangen

Beispiel-PowerShell-Automation-Skripts wurden nur für die Basisversion der Fix It geschrieben, die vollständig in Azure App Service-Web-Apps ausgeführt wird. Wir dies nicht getan haben Skripts bereitgestellt, für das Einrichten und Bereitstellen von für die Web-app sowie Cloud-Service-Umgebung für die Verarbeitung der Warteschlange erforderlich sind.

### <a name="special-handling-for-html-codes-in-user-input"></a>Spezielle Behandlung für HTML-Codes in der Benutzereingabe

ASP.NET wird automatisch verhindert, dass viele Möglichkeiten, die in denen böswillige Benutzer Cross-Site scripting-Angriffe versuchen möglicherweise, durch das Skript in Eingabefelder Benutzer eingeben. Und die MVC `DisplayFor` -Hilfsprogramm zur Aufgabe anzuzeigen titles und Anmerkungen zu dieser automatisch HTML-codiert-Werte, die an den Browser gesendet. Aber in einer Produktions-app möchten Sie möglicherweise zusätzliche Maßnahmen ergreifen. Weitere Informationen finden Sie unter [Anforderungsüberprüfung in ASP.NET](https://msdn.microsoft.com/library/hh882339.aspx).

<a id="bestpractices"></a>
## <a name="best-practices"></a>Bewährte Methoden

Es folgen einige Probleme, die nach einem behoben wurden, Code Review ermittelt und die ursprüngliche Version der Fix It-app testen. Einige verursacht wurden, von dem ursprünglichen Programmierer, die nicht die Kenntnis der bestimmten wird empfohlen, einige einfach daran, dass der Code, schnell geschrieben wurde und wurde nicht für die veröffentlichte Software vorgesehen. Wir haben hier die Probleme aufgelistet, für den Fall, dass Sie etwas, die wir in diesem Review gelernt und Tests, die möglicherweise hilfreich für andere Personen, die auch Web-apps entwickeln.

### <a name="dispose-the-database-repository"></a>Löschen Sie die Datenbank-repository

Die `FixItTaskRepository` Klasse muss das Entity Framework freigeben `DbContext` Instanz. Wir haben dies durch die Implementierung `IDisposable` in die `FixItTaskRepository` Klasse:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample1.cs)]

Beachten Sie, die AutoFac automatisch verworfen werden die `FixItTaskRepository` -Instanz, daher wir nicht explizit müssen.

Entfernen Sie eine andere Möglichkeit ist die `DbContext` Membervariable vom `FixItTaskRepository`, und erstellen Sie stattdessen einen lokalen `DbContext` Variable in einzelnen Repository-Methode, in eine `using` Anweisung. Zum Beispiel:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample2.cs)]

### <a name="register-singletons-as-such-with-di"></a>Registrieren Sie Singletons als solche mit Dependency Injection

Da nur eine Instanz der `PhotoService` Klasse und `Logger` Klasse erforderlich ist, werden diese Klassen werden sollte [registriert als einzelne Instanzen für Dependency Injection](https://code.google.com/p/autofac/wiki/InstanceScope) in *DependenciesConfig.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample3.cs?highlight=1,3)]

### <a name="security-dont-show-error-details-to-users"></a>Sicherheit: Keine Fehlerdetails für Benutzer anzeigen

Die ursprüngliche Fix It-app nicht haben eine generische Fehlerseite und können nur alle Ausnahmen Blase bis zu der Benutzeroberfläche aus, damit einige Ausnahmen wie z. B. Datenbankverbindungsfehler eine vollständige stapelüberwachung, die angezeigt wird, an den Browser führen können. Ausführliche Fehlerinformationen kann manchmal durch Angriffe böswilliger Benutzer vereinfachen. Die Lösung besteht, melden Sie sich die Details der Ausnahme und eine Fehlerseite für den Benutzer, der keine Fehlerdetails angezeigt. Die Fix It-app wurde bereits Protokollierung und damit eine Fehlerseite angezeigt, auch `<customErrors mode=On>` in der Datei "Web.config".

[!code-xml[Main](the-fix-it-sample-application/samples/sample4.xml?highlight=2)]

Standardmäßig bewirkt dies, dass *Views\Shared\Error.cshtml* für Fehler angezeigt werden soll. Sie können anpassen, *Error.cshtml* oder erstellen Sie Ihre eigenen Fehler Seitenansicht und Hinzufügen einer `defaultRedirect` Attribut. Sie können auch andere Fehlerseiten nach bestimmten Fehlern angeben.

### <a name="security-only-allow-a-task-to-be-edited-by-its-creator"></a>Sicherheit: nur zulassen Sie eine Aufgabe bearbeitet werden, durch dessen Schöpfer

Die Indexseite des Dashboards zeigt nur die Aufgaben, die vom angemeldeten Benutzer erstellt, aber ein böswilliger Benutzer kann eine URL erstellen, mit der ID eines anderen Benutzers Aufgabe. Wir haben Code hinzugefügt *DashboardController.cs* einen 404-Fehler in diesem Fall zurückgegeben:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample5.cs?highlight=9-14,24-29)]

### <a name="dont-swallow-exceptions"></a>Behalten Sie nicht Ausnahmen

Die ursprüngliche Fix It-app zurückgegeben nur nach der Anmeldung einer Ausnahme, die das Ergebnis einer SQL-Abfrage ist null:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample6.cs?highlight=4)]

Dadurch würde, es dem Benutzer zu sehen, dass die Abfrage war erfolgreich, aber nicht nur Zeilen zurückgeben. Lösung besteht darin, die Ausnahme nach dem Abfangen und Protokollierung erneut ausgelöst:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample7.cs)]

### <a name="catch-all-exceptions-in-worker-roles"></a>Fangen Sie alle Ausnahmen in Workerrollen

Nicht behandelten Ausnahmen in eine Worker-Rolle führt dazu, dass des virtuelle Computers neu gestartet werden, daher sollten Sie alle Elemente umschließen Sie in einem Try / Catch-Block und alle Ausnahmen behandeln.

### <a name="specify-length-for-string-properties-in-entity-classes"></a>Geben Sie die Länge für Zeichenfolgeneigenschaften in Entitätsklassen

Um einfacher Code anzuzeigen, die ursprüngliche Version der app Fix It Länge für die Felder der Entität FixItTask angegeben haben, und daher wurden sie als varchar(max) in der Datenbank definiert. Die Benutzeroberfläche würde daher nahezu jede Menge der Eingabe akzeptieren. Angabe Längen legt sowohl für Benutzer geltende Grenzwerte Geben Sie in der Webseite und die Größe der Spalte in der Datenbank:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample8.cs?highlight=4,7,10,12,14)]

### <a name="mark-private-members-as-readonly-when-they-arent-expected-to-change"></a>Private Member als Readonly markiert wird, wenn sie sich erwartungsgemäß nicht ändern

Z. B. in der `DashboardController` eine Instanz der Klasse `FixItTaskRepository` wird erstellt und wird nicht erwartet, zu ändern, damit wir sie als definiert [Readonly](https://msdn.microsoft.com/library/acdd6hb7.aspx).

[!code-csharp[Main](the-fix-it-sample-application/samples/sample9.cs?highlight=3)]

### <a name="use-listany-instead-of-listcount-gt-0"></a>Verwenden Sie die Liste. Any() statt Liste. Count() &gt; 0

Verwenden, wenn Sie Sie interessiert ist, gibt an, ob ein oder mehrere Elemente in einer Liste mit die angegebenen Kriterien entsprechen, die [alle](https://msdn.microsoft.com/library/bb534972.aspx) -Methode, da wird als ein Element, das die Kriterien Anpassung gefunden wird, während die `Count` Methode immer durchlaufen muss jedes Element. Das Dashboard *"Index.cshtml"* Datei ursprünglich hatte den folgenden Code:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample10.cshtml)]

Wir es so geändert:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample11.cshtml?highlight=1)]

### <a name="generate-urls-in-mvc-views-using-mvc-helpers"></a>Generieren von URLs in MVC-Ansichten mithilfe von MVC-Hilfsprogrammen

Für die **erstellen Sie einen Fix It** Schaltfläche auf der Startseite, Fix It-app, die schwer codiert ein Anchor-Element:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample12.cshtml)]

Aktion/Links wie folgt ist es besser, verwenden Sie die [Url.Action](https://msdn.microsoft.com/library/system.web.mvc.urlhelper.action.aspx) HTML-Hilfsobjekt, z.B.:

[!code-cshtml[Main](the-fix-it-sample-application/samples/sample13.cshtml)]

### <a name="use-taskdelay-instead-of-threadsleep-in-worker-role"></a>Verwenden von Task.Delay anstelle von Thread.Sleep im Worker-Rolle

Die neue Webprojektvorlage legt `Thread.Sleep` im Beispiel Code für eine Worker-Rolle, aber den Thread in den Standbymodus verursachen kann dazu führen, dass den Threadpool weitere unnötige Threads zu starten. Sie können verhindern, indem [Task.Delay](https://msdn.microsoft.com/library/hh139096.aspx) stattdessen.

[!code-csharp[Main](the-fix-it-sample-application/samples/sample14.cs?highlight=11)]

### <a name="avoid-async-void"></a>Vermeiden von Async-void

Wenn eine asynchrone Methode einen Wert zurückgeben, nicht zurückgeben einer `Task` Typ statt `void`.

Dieses Beispiel stammt aus dem `FixItQueueManager` Klasse: 

[!code-csharp[Main](the-fix-it-sample-application/samples/sample15.cs)]

Verwenden Sie `async void` nur für den Ereignishandler der obersten Ebene. Wenn Sie eine Methode als definieren `async void`, kann der Aufrufer nicht **"await"** die Methode oder die Methode löst Ausnahmen abgefangen. Weitere Informationen finden Sie unter [Best Practices bei der asynchronen Programmierung](https://msdn.microsoft.com/magazine/jj991977.aspx). 

### <a name="use-a-cancellation-token-to-break-from-worker-role-loop"></a>Verwenden Sie ein Abbruchtoken, um Worker-Rolle-Schleife zu unterbrechen

In der Regel die **ausführen** -Methode für eine workerrolle enthält eine Endlosschleife. Wenn die Worker-Rolle beendet wird, weist der [RoleEntryPoint.OnStop](https://msdn.microsoft.com/library/windowsazure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstop.aspx) Methode wird aufgerufen. Sie sollten diese Methode verwenden, um die Arbeit abzubrechen, die in erfolgt die **ausführen** -Methode, und beenden ordnungsgemäß. Andernfalls kann der Prozess während eines Vorgangs beendet werden.

### <a name="opt-out-of-automatic-mime-sniffing-procedure"></a>Deaktivieren der automatischen MIME-Ermittlung-Prozedur

In einigen Fällen meldet Internet Explorer einen MIME-Typ, die vom Webserver angegebenen Typ unterscheiden. Beispielsweise, wenn Internet Explorer HTML in einer Datei mit dem HTTP-Antwort-Header Content-Type übermittelt Inhalt findet: Text/Plain über Internet Explorer ermittelt, dass der Inhalt als HTML gerendert werden soll. Leider kann diese "MIME-Ermittlung" auch zu Sicherheitsproblemen für Server, die nicht vertrauenswürdige Inhalte hosten führen. Um dieses Problem zu beheben, Internet Explorer 8, MIME-Typ Entscheidung Code eine Reihe von Änderungen vorgenommen hat, und bietet Anwendungsentwicklern [abwählen, MIME-Ermittlung](https://blogs.msdn.com/b/ie/archive/2008/07/02/ie8-security-part-v-comprehensive-protection.aspx). Der folgende Code wurde hinzugefügt, um die *"Web.config"* Datei.

[!code-xml[Main](the-fix-it-sample-application/samples/sample16.xml?highlight=2-7)]

### <a name="enable-bundling-and-minification"></a>Aktivieren der Bündelung und Minimierung

Wenn Visual Studio ein neues Webprojekt erstellt, ist Bündelung und Minimierung von JavaScript-Dateien nicht standardmäßig aktiviert. Wir haben eine einzige Zeile Code in BundleConfig.cs hinzugefügt:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample17.cs?highlight=9)]

### <a name="set-an-expiration-time-out-for-authentication-cookies"></a>Legen Sie ein Ablaufzeitlimit für Authentifizierungscookies

Standardmäßig werden Authentifizierungscookies sind zwei Wochen gültig. Ein kürzeres Intervall ist sicherer. Sie können diese Einstellung in *StartupAuth.cs*:

[!code-csharp[Main](the-fix-it-sample-application/samples/sample18.cs?highlight=4-5)]

<a id="run-in-vs"></a>
## <a name="how-to-run-the-app-from-visual-studio-on-your-local-computer"></a>Wie Sie die app in Visual Studio auf Ihrem lokalen Computer ausführen.

Es gibt zwei Möglichkeiten zum Ausführen der app zu beheben:

- Führen Sie die Basis-Anwendung, die neue Aufgaben direkt in der SQL-Datenbank schreibt.
- Führen Sie die Anwendung mithilfe einer Warteschlange und einem Back-End-Dienst Tasks erstellen. Muster der Warteschlange finden Sie im Kapitel [Warteschlange-orientierte Arbeit Muster](queue-centric-work-pattern.md).

<a id="runbase"></a>
### <a name="run-the-base-application"></a>Führen Sie die grundlegende Anwendung

1. Installieren Sie [Visual Studio 2013 oder Visual Studio 2013 Express für Web](https://www.visualstudio.com/downloads).
2. Installieren Sie die [Azure SDK für .NET für Visual Studio 2013.](https://go.microsoft.com/fwlink/p/?linkid=323510&amp;clcid=0x409)
3. Laden Sie die ZIP-Datei aus dem [MSDN Code Gallery](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4).
4. Mit der rechten Maustaste in der ZIP-Datei im Datei-Explorer klicken Sie auf Eigenschaften, und klicken Sie im Fenster Eigenschaften auf zulassen.
5. Entzippen Sie die Datei aus.
6. Doppelklicken Sie auf der SLN-Datei, um Visual Studio zu starten.
7. Klicken Sie im Menü Extras auf Bibliothekspaket-Manager, und klicken Sie dann auf Paket-Manager-Konsole.
8. Klicken Sie in der Paket-Manager-Konsole (PMC) auf wiederherstellen.
9. Beenden Sie Visual Studio.
10. Starten Sie den [Azure-Speicheremulator](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx).
11. Visual Studio neu starten, die Projektmappe öffnen Sie im vorherigen Schritt geschlossen.
12. Stellen Sie sicher, dass das FixIt-Projekt als Startprojekt festgelegt ist, und drücken Sie STRG + F5, um das Projekt auszuführen.

<a id="queueslocal"></a>
### <a name="run-the-application-with-queue-processing"></a>Führen Sie die Anwendung mit der Verarbeitung der Warteschlange

1. Befolgen Sie die Anweisungen für [führen Sie die grundlegende Anwendung](#runbase), und klicken Sie dann den Browser schließen, und schließen Sie Visual Studio.
2. Starten Sie Visual Studio mit Administratorrechten aus. (Verwenden Sie Azure-Serveremulator und, die Administratorrechte erforderlich.)
3. In der Anwendung *"Web.config"* Datei die *MyFixIt* Projekt (das Webprojekt), ändern Sie den Wert der `appSettings/UseQueues` auf "True": 

    [!code-console[Main](the-fix-it-sample-application/samples/sample19.cmd?highlight=3)]
4. Wenn die [Azure-Speicheremulator](https://msdn.microsoft.com/library/windowsazure/hh403989.aspx) ist nicht immer noch ausgeführt, starten Sie ihn erneut.
5. Das Webprojekt für die FixIt und das Projekt MyFixItCloudService werden gleichzeitig ausgeführt.

    Mithilfe von Visual Studio 2013:

   1. Drücken Sie F5, um das FixIt-Projekt auszuführen.
   2. In **Projektmappen-Explorer**mit der rechten Maustaste auf das MyFixItCloudService-Projekt, und klicken Sie dann auf **Debuggen** -- **neue Instanz starten**.

      Verwenden von Visual Studio 2013 Express für Web:

   3. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste der FixIt-Projektmappe, und wählen **Eigenschaften**.
   4. Wählen Sie **mehrere Startprojekte**...
   5. In der **Aktion** wählen Sie die Dropdownliste unter MyFixIt und MyFixItCloudService, **starten**.
   6. Klicken Sie auf **OK**.
   7. Drücken Sie F5, um beide Projekte auszuführen.

      Wenn Sie das Projekt MyFixItCloudService ausführen, startet Visual Studio Azure-Serveremulator. Abhängig von Ihrer Firewallkonfiguration müssen Sie den Emulator über die Firewall zulassen.

<a id="deploybase"></a>
## <a name="how-to-deploy-the-base-app-to-azure-app-service-web-apps-by-using-the-windows-powershell-scripts"></a>Die Basis-app in Azure App Service Web Apps bereitstellen, mithilfe der Windows PowerShell-Skripts

Zur Veranschaulichung der [automatisieren alles](automate-everything.md) der Fix It-app-Muster mit Skripts, die eine Umgebung in Azure einrichten und Bereitstellen des Projekts in der neuen Umgebung angegeben wird. Die folgenden Anweisungen wird erläutert, wie die Skripts verwenden.

Wenn Sie in Azure ausgeführt werden, ohne die Verwendung von Warteschlangen, und Sie die Änderungen lokal mit Warteschlangen, stellen Sie sicher, dass Sie wieder auf "false", bevor Sie mit den folgenden Anweisungen fortfahren den UseQueues AppSetting-Wert festgelegt.

Diesen Anweisungen wird vorausgesetzt, Sie haben bereits heruntergeladen und lokal ausgeführt wird die Fix It-Lösung, und dass Sie eine Azure-Konto oder Azure-Abonnement besitzen, die Sie zum Verwalten von autorisiert sind.

1. Installieren Sie die **Azure PowerShell** Konsole. Anweisungen hierzu finden Sie unter [installieren und Konfigurieren von Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1).

    Diese benutzerdefinierte Konsole ist für die Arbeit mit Ihrem Azure-Abonnement konfiguriert. Das Azure-Modul installiert ist, der *Programmdateien* Directory und wird automatisch bei jeder Verwendung der Azure PowerShell-Konsole importiert.

    Wenn Sie bevorzugt arbeiten in einem anderen Host-Programm, z. B. Windows PowerShell ISE, müssen Sie verwenden die [Import-Module](https://go.microsoft.com/fwlink/?LinkID=141553) Cmdlet, um das Azure-Modul zu importieren, oder verwenden Sie einen Befehl im Azure-Modul zum Auslösen der automatische Import des Moduls.
2. Starten Sie Azure PowerShell mit der **als Administrator ausführen** Option.
3. Führen Sie die [Set-ExecutionPolicy](https://go.microsoft.com/fwlink/p/?linkid=293941) Cmdlet, um die Azure PowerShell-Ausführungsrichtlinie auf `RemoteSigned`. Geben Sie **Y** (für Ja) um die Änderung der Richtlinie abzuschließen.

    [!code-console[Main](the-fix-it-sample-application/samples/sample20.cmd)]

    Mit dieser Einstellung können Sie lokale Skripts ausführen, die nicht digital signiert. (Sie können auch die Ausführungsrichtlinie festlegen, um `Unrestricted`, das würde entfällt die Notwendigkeit der zulassen-Schritt später noch Mal, aber dies wird nicht empfohlen aus Sicherheitsgründen.)
4. Führen Sie die `Add-AzureAccount` -Cmdlet zum Einrichten von PowerShell mit den Anmeldeinformationen für Ihr Konto.

    [!code-console[Main](the-fix-it-sample-application/samples/sample21.cmd)]

    Diese Anmeldeinformationen laufen nach einiger Zeit ab und muss erneut ausgeführt werden. die `Add-AzureAccount` Cmdlet. Während dieses e-Book geschrieben wird, beträgt das Zeitlimit, bevor die Anmeldeinformationen ablaufen 12 Stunden.
5. Wenn Sie mehrere Abonnements verfügen, verwenden Sie das Select-AzureSubscription-Cmdlet, das Abonnement angeben, in die Test-Umgebung erstellt werden soll.
6. Importieren Sie ein Verwaltungszertifikat für das gleiche Azure-Abonnement mithilfe der `Get-AzurePublishSettingsFile` und `Import-AzurePublishSettingsFile` Cmdlets. Die erste dieser Cmdlets eine Zertifikatsdatei heruntergeladen und in der zweiten Geben Sie den Speicherort der Datei, um es zu importieren. > [!IMPORTANT]
   > Behalten Sie die heruntergeladene Datei an einem sicheren Ort oder löschen Sie, wenn Sie damit fertig sind, da es sich um ein Zertifikat enthält, die zum Verwalten Ihrer Azure-Dienste verwendet werden kann.

    [!code-console[Main](the-fix-it-sample-application/samples/sample22.cmd)]

    Das Zertifikat wird verwendet, für einen REST-API-Aufruf, der dem Entwicklungscomputer IP-Adresse erkennt, um eine Firewallregel auf dem SQL-Datenbank-Server festlegen.
7. Führen Sie die [Set-Location](https://go.microsoft.com/fwlink/p/?linkid=293912) Cmdlet (Aliase werden `cd`, `chdir`, und `sl`), navigieren zu dem Verzeichnis, die die Skripts enthält. (sie sind im Verzeichnis der *Automation* im Ordner "der Fix It-Lösung".) Setzen Sie den Pfad in Anführungszeichen ein, wenn die Namen der Verzeichnisse Leerzeichen enthalten. Um beispielsweise zu navigieren die `c:\Sample Apps\FixIt\Automation` Directory könnten Sie den folgenden Befehl eingeben:

    [!code-console[Main](the-fix-it-sample-application/samples/sample23.cmd)]
8. Um Windows PowerShell zum Ausführen dieser Skripts zu ermöglichen, verwenden die [Unblock-File](https://go.microsoft.com/fwlink/p/?linkid=294021) Cmdlet. (Die Skripts werden blockiert, da sie aus dem Internet heruntergeladen wurden.)

    > [!WARNING]
    > Sicherheit – vor der Ausführung `Unblock-File` auf Skripts oder ausführbare Datei, öffnen Sie die Datei in Editor, überprüfen Sie die Befehle und stellen Sie sicher, dass sie keine bösartigen Code enthalten.

    Beispielsweise der folgende Befehl führt die `Unblock-File` -Cmdlet für alle Skripts im aktuellen Verzeichnis.

    [!code-console[Main](the-fix-it-sample-application/samples/sample24.cmd)]
9. Klicken Sie zum Erstellen der Web-app für die Basis (keine Warteschlangen verarbeiten) beheben Sie app, führen Sie das Skript zum Erstellen der Umgebung.

    Die erforderlichen `Name` Parameter gibt den Namen der Datenbank und wird auch verwendet werden, für das Speicherkonto, das das Skript erstellt. Der Name muss innerhalb der Domäne azurewebsites.NET global eindeutig sein. Wenn Sie einen Namen angeben, die nicht eindeutig ist, wie z. B. Fixit oder Test (oder auch wie im Beispiel Fixitdemo) ist die `New-AzureWebsite` tritt ein interner Fehler, die einen Konflikt meldet. Das Skript konvertiert den Namen in Kleinbuchstaben zur Einhaltung der Anforderungen an die für die Web-apps, Speicherkonten und Datenbanken.

    Die erforderlichen `SqlDatabasePassword` Parameter gibt das Kennwort für das Administratorkonto ein, die für SQL-Datenbank erstellt werden. Keine XML-Sonderzeichen im Kennwort enthalten (&amp; &lt; &gt; ;). Dies ist eine Einschränkung der Art und Weise, in die Skripts geschrieben wurden, nicht um eine Einschränkung von Azure.

    Wenn Sie mit dem Namen "Fixitdemo" Web-app erstellen und eine SQL Server-Administratorkennwort der "Passw0rd1" verwenden möchten, können Sie z. B. den folgenden Befehl eingeben:

    [!code-console[Main](the-fix-it-sample-application/samples/sample25.cmd)]

    Der Name muss eindeutig sein, in der Domäne azurewebsites.net, und das Kennwort muss die SQL-Datenbank-Anforderungen für die Kennwortrichtlinie zur Komplexität erfüllen. (Im Beispiel Passw0rd1 erfüllt die Anforderungen.)

    Beachten Sie, der mit der Befehl ". \". Um zu verhindern, dass böswillige Ausführung von Skripts, müssen Windows PowerShell, dass Sie den vollqualifizierten Pfad zur Skriptdatei angeben, wenn Sie ein Skript ausführen. Sie können einen Punkt verwenden, um das aktuelle Verzeichnis anzugeben (".\") oder geben Sie den vollqualifizierten Pfad, z.B.:

    [!code-console[Main](the-fix-it-sample-application/samples/sample26.cmd)]

    Weitere Informationen zum Skript zur Verwendung der `Get-Help` Cmdlet.

    [!code-console[Main](the-fix-it-sample-application/samples/sample27.cmd)]

    Sie können die `Detailed`, `Full`, `Parameters`, und `Examples` Parameter des Cmdlets Get-Help die Hilfe zu filtern, die zurückgegeben wird.

    Wenn das Skript ein Fehler auftritt oder Fehler, z. B. "New-AzureWebsite: Rufen Sie Set-AzureSubscription und Select-AzureSubscription zuerst" generiert Sie möglicherweise nicht die Konfiguration von Azure PowerShell abgeschlossen haben.

    Nachdem das Skript abgeschlossen ist, können Sie im Azure-Verwaltungsportal auf die Ressourcen, die erstellt wurden, finden Sie unter, siehe die [automatisieren alles](automate-everything.md) Kapitel.
10. Verwenden Sie zum Bereitstellen des FixIt-Projekts in die neue Azure-Umgebung die *AzureWebsite.ps1* Skript. Zum Beispiel:

    [!code-console[Main](the-fix-it-sample-application/samples/sample28.cmd)]

    Wenn die Bereitstellung abgeschlossen ist, öffnet sich der Browser mit Fix It in Azure ausgeführt wird.

<a id="troubleshooting"></a>
## <a name="troubleshooting-the-windows-powershell-scripts"></a>Problembehandlung bei der Windows PowerShell-Skripts

Die häufigsten Fehler bei der Ausführung dieser Skripts auftreten werden im Zusammenhang mit Berechtigungen. Stellen Sie sicher, dass `Add-AzureAccount` und `Import-AzurePublishSettingsFile` war erfolgreich, und, die Sie für das gleiche Azure-Abonnement verwendet. Auch wenn `Add-AzureAccount` wurde erfolgreich Sie möglicherweise erneut ausführen. Die Berechtigungen hinzugefügt, indem `Add-AzureAccount` sind 12 Stunden gültig.

### <a name="object-reference-not-set-to-an-instance-of-an-object"></a>Der Objektverweis wurde nicht auf eine Objektinstanz festgelegt.

Wenn das Skript Fehler zurückgibt, wie z. B. "Objektverweis nicht auf eine Instanz eines Objekts festgelegt" das bedeutet, dass Windows PowerShell ein Objekt an (Dies ist ein null-Verweisausnahme auftreten) Prozess kann nicht gefunden werden, führen Sie die `Add-AzureAccount` Cmdlet, und wiederholen Sie das Skript.

[!code-console[Main](the-fix-it-sample-application/samples/sample29.cmd)]

### <a name="internalerror-the-server-encountered-an-internal-error"></a>InternalError: Mit dem Server ist einen internen Fehler aufgetreten.

Die `New-AzureWebsite` Cmdlet gibt einen internen Fehler zurück, wenn der Zugriff auf der Namen in der Domäne azurewebsites.net nicht eindeutig ist. Um den Fehler zu beheben, verwenden Sie einen anderen Wert für den Namen in der die Name-Parameter von *New-AzureWebsiteEnv.ps1*.

[!code-console[Main](the-fix-it-sample-application/samples/sample30.cmd)]

### <a name="restarting-the-script"></a>Neustarten des Skripts

Wenn Sie neu starten müssen die *New-AzureWebsiteEnv.ps1* Skript, da sie Fehler, bevor er die Nachricht "Skript ist abgeschlossen" gedruckt, möchten Sie möglicherweise Ressourcen zu löschen, die vom Skript erstellt wurden, bevor sie beendet. Wenn das Skript bereits ContosoFixItDemo Web-app, und Sie das Skript erneut ausführen, mit dem gleichen Namen erstellt, wird z. B. das Skript fehl, da der Name verwendet wird.

Um zu bestimmen, welche Ressourcen vom Skript erstellt wurden, bevor er angehalten wird, verwenden Sie die folgenden Cmdlets:

- `Get-AzureWebsite`
- `Get-AzureSqlDatabaseServer`
- `Get-AzureSqlDatabase`: Führen Sie dieses Cmdlet aus, übergeben Sie den Namen der Datenbank-Server an `Get-AzureSqlDatabase`:  
    `Get-AzureSqlDatabaseServer | Get-AzureSqlDatabase.`

Um diese Ressourcen zu löschen, verwenden Sie die folgenden Befehle aus. Beachten Sie, wenn Sie den Datenbankserver löschen, werden automatisch mit dem Server zugeordneten Datenbanken gelöscht.

- `Get-AzureWebsite -Name <WebsiteName> | Remove-AzureWebsite`
- `Get-AzureSqlDatabase -Name <DatabaseName> -ServerName <DatabaseServerName> | Remove-SqlAzureDatabase`
- `Get-AzureSqlDatabaseServer | Remove-AzureSqlDatabaseServer`

<a id="deployqueues"></a>
## <a name="how-to-deploy-the-app-with-queue-processing-to-azure-app-service-web-apps-and-an-azure-cloud-service"></a>Gewusst wie: Bereitstellen der app mit einer Warteschlange, die Verarbeitung von Azure App Service-Web-Apps und Azure-Clouddienst

Um Warteschlangen zu aktivieren, stellen Sie die folgende Änderung in der Datei MyFixIt\Web.config aus. Klicken Sie unter `appSettings`, ändern Sie den Wert der `UseQueues` auf "True": 

[!code-xml[Main](the-fix-it-sample-application/samples/sample31.xml)]

Klicken Sie dann die MVC-Anwendung in eine Web-app in Azure App Service bereitstellen, wie beschrieben [früheren](#deploybase).

Als Nächstes erstellen Sie einen neuen Azure-Cloud-Dienst. Die Skripts enthalten, mit der Fix It-app erstellen und Sie keine Cloud-Diensts bereitstellen, damit Sie für dieses Azure-Portal verwenden müssen. Klicken Sie im Portal auf **neu** -- **Compute** – **Clouddienst** -- **Schnellerfassung**, und geben Sie dann eine URL und einen Speicherort im Rechenzentrum. Verwenden Sie das Rechenzentrum, in dem Sie die Web-app bereitgestellt.

![](the-fix-it-sample-application/_static/image1.png)

Bevor Sie den Cloud-Dienst bereitstellen können, müssen Sie einige der Konfigurationsdateien zu aktualisieren.

In MyFixIt.WorkerRoler\app.config unter `connectionStrings`, ersetzen Sie den Wert der `appdb` Verbindungszeichenfolge durch die tatsächliche Verbindungszeichenfolge für die SQL-Datenbank. Sie können die Verbindungszeichenfolge aus dem Portal abrufen. Klicken Sie im Portal auf **SQL-Datenbanken** - **Appdb** - **Ansicht SQL-Datenbank-Verbindungszeichenfolgen für ADO .NET, ODBC, PHP und JDBC**. Kopieren Sie die ADO.NET-Verbindungszeichenfolge aus, und fügen Sie den Wert in der Datei "App.config". Ersetzen Sie "{Ihr\_Kennwort\_hier}" mit dem Datenbankkennwort. (Wenn Sie die Skripts verwendet, um die MVC-app bereitzustellen, das Datenbankkennwort angegebene die `SqlDatabasePassword` Skript Parameter.)

Das Ergebnis sollte wie folgt aussehen:

[!code-xml[Main](the-fix-it-sample-application/samples/sample32.xml)]

In der gleichen Datei MyFixIt.WorkerRoler\app.config unter `appSettings`, ersetzen Sie die beiden Platzhalterwerte für die Azure Storage-Konto.

[!code-xml[Main](the-fix-it-sample-application/samples/sample33.xml?highlight=2-3)]

Sie können den Zugriffsschlüssel aus dem Portal abrufen. Finden Sie unter [Gewusst wie: Verwalten von Speicherkonten](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account).

Ersetzen Sie in MyFixItCloudService\ServiceConfiguration.Cloud.cscfg die gleichen zwei Platzhalter-Werte für die Azure Storage-Konto aus.

[!code-xml[Main](the-fix-it-sample-application/samples/sample34.xml?highlight=3)]

Jetzt können Sie den Cloud-Dienst bereitstellen. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des MyFixItCloudService-Projekts, und wählen **veröffentlichen**. Weitere Informationen finden Sie unter "[Bereitstellen der Anwendung in Azure](https://www.windowsazure.com/develop/net/tutorials/multi-tier-web-site/2-download-and-run/#deployAz)", d.h. in Teil 2 des [in diesem Tutorial](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36).

> [!div class="step-by-step"]
> [Vorherige](more-patterns-and-guidance.md)
