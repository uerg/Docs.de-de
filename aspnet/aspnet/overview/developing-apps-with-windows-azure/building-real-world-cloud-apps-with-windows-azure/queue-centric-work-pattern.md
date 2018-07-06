---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Warteschlangenorientierte-Muster (erstellen realer Cloud-Apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: f9916e4ecbe6234ee12bcb56519e7e2c0e490972
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840187"
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Warteschlangenorientierte-Muster (erstellen realer Cloud-Apps mit Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download korrigieren Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps mit Azure** e-Book basiert darauf, dass eine Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Weitere Informationen zu e-Book, finden Sie unter [im ersten Kapitel](introduction.md).


Schon kennen wir, dass mehrere Dienste mithilfe einer "zusammengesetzte" Vereinbarung zum SERVICELEVEL, ergeben kann, in dem die app effektive SLA ist die *Produkt* aus den einzelnen SLAs. Beispielsweise verwendet die app Fix It-Websites, Storage und SQL-Datenbank. Wenn einer dieser Dienste ein Fehler auftritt, wird die app einen Fehler an den Benutzer zurück.

Zwischenspeichern ist eine gute Möglichkeit zur Behandlung vorübergehender Fehler für nur-Lese Inhalt. Aber was geschieht, wenn Ihre Anwendung muss funktionieren? Wenn der Benutzer einen neuen Fix It-Task sendet, kann nicht z. B. die Aufgabe von der Anwendung nur in den Cache eingefügt. Die app muss sich in einem persistenten Datenspeicher, den Fix It-Task schreiben, damit er verarbeitet werden kann.

Hier kommt das warteschlangenorientierte-Muster. Mit diesem Muster können losen Kopplung zwischen eine Webebene und eine Back-End-Dienst.

So funktioniert das Muster. Wenn die Anwendung eine Anforderung erhält, fügt eine Arbeitsaufgabe in einer Warteschlange und gibt sofort die Antwort zurück. Klicken Sie dann ein separates Back-End-Prozess Arbeitselemente aus der Warteschlange abruft und führt die Arbeit.

Das warteschlangenorientierte-Muster ist hilfreich für:

- Arbeit, die Zeit in Anspruch nehmen (hohe Latenz).
- Aufgaben, die einen externen Dienst erfordert, der möglicherweise nicht immer verfügbar sind.
- D. h. arbeiten mit großem Ressourcenaufwand (hohe CPU-Auslastung).
- Aufgaben, die von der Rate (plötzliche Load anfrageanstieg) Lastenausgleich profitieren würden.

## <a name="reduced-latency"></a>Reduzierte Latenz

Warteschlangen sind nützlich, jedes Mal, wenn Sie zeitaufwendige Aufgaben ausführen. Wenn ein Vorgang ein paar Sekunden dauert oder länger betragen, anstatt zu blockieren, der Endbenutzer, die Arbeitsaufgabe in eine Warteschlange eingereiht. "Wir arbeiten daran" dem Benutzer mitgeteilt, und klicken Sie dann einen warteschlangenlistener verwenden, um die Aufgabe im Hintergrund zu verarbeiten.

Z. B. Wenn Sie etwas Onlinehändlers erwerben, bestätigt der Website Ihren Auftrag sofort. Aber, das bedeutet nicht, dass Ihre Daten bereits in einem LKW übermittelt werden. Dazu eine Aufgabe in einer Warteschlange, und sie im Hintergrund werden die kreditkartenüberprüfung Vorbereiten Ihrer Elemente für die Auslieferung ausführen und so weiter.

Für Szenarien mit kurzen Wartezeiten möglicherweise die gesamte End-to-End-Zeit länger über eine Warteschlange, die im Vergleich mit den Task synchron ausführen. Aber auch dann einen weiteren Vorteil, Nachteil überwiegen können.

## <a name="increased-reliability"></a>Erhöhte Zuverlässigkeit

In der Version der Fix It, die wir bisher betrachtet haben, ist die Web-Front-End-eng mit der SQL-Datenbank-Back-End verbunden. Wenn die SQL-Datenbank-Dienst nicht verfügbar ist, erhält der Benutzer eine Fehlermeldung an. Wenn Wiederholungen nicht funktionieren (d. h. der Fehler ist mehr als vorübergehende), ist das einzige, was Sie tun können, ein Fehler angezeigt, und den Benutzer bitten, versuchen es später noch mal.

![Diagramm mit Web-Front-End-Fehler bei der SQL-Datenbank-Back-End ein Fehler auftritt](queue-centric-work-pattern/_static/image1.png)

Verwendung von Warteschlangen, wenn ein Benutzer einen Fix It-Task sendet, schreibt die app eine Meldung an die Warteschlange an. Die Nutzlast der Nachricht ist eine [JSON](http://json.org/) Darstellung des Tasks. Sobald die Nachricht an die Warteschlange geschrieben wird, wird die app gibt zurück, und zeigt sofort dem Benutzer eine Erfolgsmeldung an.

Wenn Sie einen der Back-End-Dienste – z. B. die SQL-Datenbank "oder" der warteschlangenlistener--offline geschaltet werden, können Benutzer weiterhin neue Fix It-Aufgaben senden. Die Nachrichten werden nur in die Warteschlange, bis die Back-End-Dienste erneut verfügbar sind. An diesem Punkt werden die Back-End-Dienste im Backlog aufholen.

![Diagramm mit der Web-Front-End Sie weiterhin funktionieren, wenn eine Fehlermeldung für die SQL-Datenbank vorhanden ist](queue-centric-work-pattern/_static/image2.png)

Darüber hinaus können jetzt Sie mehrere Back-End-Logik hinzufügen, ohne Gedanken um die resilienz des Front-End. Beispielsweise empfiehlt es sich um das Senden einer e-Mail oder SMS-Nachricht an den Besitzer, wenn eine neue Fix It zugewiesen wird. Wenn die e-Mail oder SMS-Dienst nicht verfügbar ist, können Sie alles verarbeiten und dann eine Nachricht in einer separaten Warteschlange zum Senden von e-Mail/SMS-Nachrichten eingefügt.

Bisher war die effektive SLA-Web-Apps &times; Storage &times; SQL-Datenbank = 99,7 %. (Finden Sie unter [Entwurf Ausfälle überbrücken](design-to-survive-failures.md).)

Wenn wir die app für eine Warteschlange verwenden, ändern, hängt von der Web-Front-End nur Web-Apps und -Speicher für eine zusammengesetzte Vereinbarung zum SERVICELEVEL, um 99,8 %. (Beachten Sie, dass Warteschlangen sind Teil der Azure Storage-Dienst aus, und sie in die gleiche SLA als blobspeicher enthalten sind.)

Wenn Sie noch besser als 99,8 % benötigen, können Sie zwei Warteschlangen in zwei verschiedenen Regionen erstellen. Legen Sie eine als das primäre und das andere, als die sekundäre Datenbank. In der app ein Failover auf die sekundäre Warteschlange, wenn die primäre Warteschlange nicht verfügbar ist. Die Wahrscheinlichkeit von beiden nichtverfügbarkeit zur gleichen Zeit ist sehr klein.

## <a name="rate-leveling-and-independent-scaling"></a>Rate Lastenausgleich und das unabhängige skalieren

Warteschlangen sind auch nützlich, so genannte *bewerten Abgleich* oder *belastungsausgleich*.

Web-apps sind oft anfällig, plötzliche Spitzen im Datenverkehr. Während Sie für die automatische Skalierung, zum automatischen Hinzufügen von Webservern verwenden können, um höhere Webdatenverkehr zu bewältigen, möglicherweise für die automatische Skalierung nicht schnell genug reagieren, um plötzliche Spitzen bei der Auslastung zu behandeln. Wenn die Webserver einige der Aufgaben, die sie Auslagern können, indem Sie eine Meldung an eine Warteschlange geschrieben haben, können sie weitere Datenverkehr verarbeiten. Ein Back-End-Dienst Nachrichten aus der Warteschlange gelesen und verarbeitet werden. Die Tiefe der Warteschlange wird vergrößert oder verkleinert werden, da es sich bei der eingehenden Last.

Ein Großteil der zeitaufwändige Aufgabe, die an einen Back-End-Dienst verschoben kann die Webebene leichter auf plötzliche Spitzen im Datenverkehr reagieren. Und sparen Sie Geld, da alle angegebenen Umfang des Datenverkehrs durch weniger Webserver verarbeitet werden kann.

Sie können die Web-Ebene und Back-End-Dienst unabhängig voneinander skalieren. Beispielsweise können Sie drei Webserver aber nur ein Server Nachrichtenverarbeitung Warteschlange erforderlich. Oder wenn Sie eine rechenintensive Aufgabe im Hintergrund ausführen, benötigen Sie möglicherweise weitere Back-End-Server.

![](queue-centric-work-pattern/_static/image3.png)

Für die automatische Skalierung funktioniert mit Back-End-Dienste sowie der Webebene. Sie können zentral hoch- oder Herunterskalieren der Anzahl von VMs, die die Aufgaben in der Warteschlange basierend auf der CPU-Auslastung von Back-End-VMs verarbeitet werden. Alternativ können Sie für die automatische Skalierung basierend auf wie viele Elemente in einer Warteschlange befinden. Beispielsweise können Sie für die automatische Skalierung, um zu versuchen, die nicht mehr als 10 Elemente in der Warteschlange beibehalten feststellen. Wenn die Warteschlange mehr als 10 Elemente enthält, wird für die automatische Skalierung auf virtuelle Computer hinzufügen. Wenn sie sind soweit, wird für die automatische Skalierung nach unten die zusätzlichen virtuellen Computer entfernen.

## <a name="adding-queues-to-the-fix-it-application"></a>Hinzufügen von in eine Warteschlange für die Korrektur Anwendung

Um Muster der Warteschlange zu implementieren, müssen wir zwei Änderungen an der Fix It-app vornehmen.

- Wenn ein Benutzer eine neue Fix It-Aufgabe übermittelt, platzieren Sie den Task in der Warteschlange, statt sie auf die Datenbank geschrieben.
- Erstellen Sie einen Back-End-Dienst, der Nachrichten in der Warteschlange verarbeitet.

Für die Warteschlange, verwenden wir die [Azure Queue Storage-Dienst](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Eine weitere Möglichkeit ist die Verwendung [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

Um zu entscheiden, welche Queue-Dienst verwenden, beachten Sie, wie Ihre app muss zum Senden und Empfangen von Nachrichten in der Warteschlange:

- Wenn Sie miteinander kommunizierenden Producer und Consumer verfügen, sollten erwägen Sie, Azure Queue Storage-Dienst zu verwenden. "Das Darstellen der Producer" bedeutet, dass mehrere Prozesse in eine Warteschlange Nachrichten hinzufügen. "Konkurrierende Consumer" bedeutet, dass mehrere Prozesse rufen Nachrichten aus der Warteschlange für die Verarbeitung, aber jede Nachricht nur von einem "Consumer" verarbeitet werden kann Wenn Sie mehr Durchsatz benötigen als Sie mit einer einzigen Warteschlange abrufen können, sollten Sie zusätzliche Warteschlangen und/oder zusätzliche Speicherkonten verwendet.
- Wenn Sie benötigen eine [Veröffentlichen/Abonnieren-Modell](http://en.wikipedia.org/wiki/Publish/subscribe), erwägen Sie die Verwendung von Azure Service Bus-Warteschlangen.

Die Fix It-app passt kooperierender Herstellern und konkurrierenden Consumer-Modell.

Ein weiterer Aspekt ist die Verfügbarkeit der Anwendung. Die Warteschlangen-Speicherdienst ist Teil des gleichen Diensts, die wir für den BLOB-Speicher verwenden, daher verwenden sie keine Auswirkungen auf unsere SLA. Azure Service Bus ist ein separater Dienst mit eigenen SLA. Wenn wir über Service Bus-Warteschlangen verwendet, müssten wir, eine zusätzliche SLA-Prozentsatz zu berücksichtigen, und unsere zusammengesetzte Vereinbarung zum SERVICELEVEL niedriger sein würde. Wenn Sie ein Warteschlangendienst auswählen können, stellen Sie sicher, dass Sie die Auswirkungen Ihrer Wahl auf die Verfügbarkeit der Anwendung verstehen. Weitere Informationen finden Sie unter den [Ressourcen](#resources) Abschnitt.

## <a name="creating-queue-messages"></a>Erstellen von Nachrichten in Warteschlange

Um einen Fix It-Task in der Warteschlange zu versetzen, führt das Web-Front-End für die folgenden Schritte aus:

1. Erstellen Sie eine [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) Instanz. Die `CloudQueueClient` Instanz wird verwendet, um die Anforderungen für den Warteschlangendienst ausgeführt.
2. Erstellen der Warteschlange, falls er noch nicht vorhanden.
3. Serialisieren der Fix It-Aufgabe.
4. Rufen Sie [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) die Nachricht in die Warteschlange eingefügt.

Machen wir diese Arbeit im Konstruktor und `SendMessageAsync` Methode eines neuen `FixItQueueManager` Klasse.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Hier verwenden wir die [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Bibliothek, um die Fixit JSON-Format zu serialisieren. Sie können gleich, welcher Ansatz für die Serialisierung verwenden, die Sie bevorzugen. JSON hat den Vorteil, von Menschen lesbar, während Sie weniger ausführlich als XML.

Produktion einsetzbaren Code würde Fehlerbehandlungslogik hinzufügen, anhalten, wenn die Datenbank nicht mehr verfügbar war, Wiederherstellung besser bewältigen, die Warteschlange beim Start der Anwendung erstellen und verwalten "[unzustellbare" Nachrichten](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Eine nicht verarbeitbare Nachricht ist eine Nachricht, die aus irgendeinem Grund nicht verarbeitet werden kann. Sie sollten nicht für nicht verarbeitbare Nachrichten in der Warteschlange befindet, in denen die Worker-Rolle fortlaufend versucht, verarbeiten, fehlschlagen, versuchen Sie es erneut, fehl und so weiter.)

In der Front-End-MVC-Anwendung müssen Sie den Code aktualisieren, der eine neue Aufgabe erstellt. Rufen Sie anstelle des Tasks im Repository der `SendMessageAsync` oben gezeigten Methode.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Verarbeiten von Nachrichten in Warteschlange

Um Nachrichten in der Warteschlange zu verarbeiten, erstellen wir einen Back-End-Dienst. Der Back-End-Dienst ausgeführt wird eine unendliche Schleife, die die folgenden Schritte ausführt:

1. Die nächste Nachricht aus der Warteschlange abgerufen.
2. Deserialisieren Sie die Nachricht an eine Aufgabe zu beheben.
3. Schreiben Sie den Fix It-Task in der Datenbank.

Um den Back-End-Dienst zu hosten, erstellen wir eine Azure-Clouddienst, enthält eine *workerrolle*. Eine Worker-Rolle besteht aus einer oder mehreren VMs, die Back-End-Verarbeitung ausführen kann. Der Code, der in diesen virtuellen Computern ausgeführt wird, ruft Nachrichten aus der Warteschlange, sobald diese verfügbar werden. Für jede Nachricht wir Deserialisieren die JSON-Nutzlast und schreiben eine Instanz der Entität, diese Aufgabe zu beheben, in der Datenbank mithilfe des gleichen Repositorys, die wir weiter oben in der Webebene verwendet.

Die folgenden Schritte zeigen, wie einen Worker hinzufügen Webrollenprojekt hinzu, um eine Lösung, die ein standard-Web-Projekt enthält. Diese Schritte müssen bereits im Projekt Fix It ausgeführt wurde, die Sie herunterladen können.

Visual Studio-Projektmappe ein clouddienstprojekt erst hinzufügen. Mit der rechten Maustaste in der Projektmappe, und wählen Sie **hinzufügen**, klicken Sie dann **neues Projekt**. Erweitern Sie im linken Bereich **Visual C#-** , und wählen Sie **Cloud**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

In der **neuen Azure-Cloud-Dienst** Dialogfeld erweitern Sie die **Visual C#-** Knoten auf der linken Seite. Wählen Sie **Workerrolle** , und klicken Sie auf das Symbol "Pfeil nach rechts".

![](queue-centric-work-pattern/_static/image6.png)

(Beachten Sie, die Sie auch hinzufügen, können ein *Webrolle*. Wir konnten den Fix It-Front-End im gleichen Cloud-Dienst anstelle der Ausführung in einer Azure-Website ausführen. Das hat einige Vorteile bei der Erstellung von Verbindungen zwischen Front-End- und Back-End zu koordinieren. Allerdings um diese Demo einfach zu halten, wir halten das Front-End in einer Azure App Service-Web-App und nur das Back-End in einem Cloud-Dienst ausgeführt.)

Der Worker-Rolle wird ein Standardname zugewiesen. Ändern Sie den Namen, bewegen den Mauszeiger über die Worker-Rolle im rechten Bereich, und klicken Sie auf das Stiftsymbol.

![](queue-centric-work-pattern/_static/image7.png)

Klicken Sie auf **OK** um das Dialogfeld zu schließen. Dadurch wird der Visual Studio-Projektmappe zwei Projekte hinzugefügt.

- ein Azure-Projekt, das den Cloud-Dienst, einschließlich Informationen zur Konfiguration definiert.
- Eine Worker-rollenprojekt, das die Worker-Rolle definiert.

![](queue-centric-work-pattern/_static/image8.png)

Weitere Informationen finden Sie unter [Erstellen eines Azure-Projekts mit Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

In der Worker-Rolle, wir Fragen nach Nachrichten durch Aufrufen der `ProcessMessageAsync` Methode der `FixItQueueManager` -Klasse, die wir bereits gesehen haben.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

Die `ProcessMessagesAsync` Methode wird überprüft, ob eine Nachricht wartet. Sofern vorhanden, wird die Nachricht deserialisiert eine `FixItTask` Entität und die Entität in der Datenbank gespeichert. Diese Schleife, bis die Warteschlange leer ist.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Warteschlangennachrichten fällt eine kleine Transaktion abrufen anfallen, wenn keine Nachricht, die verarbeitet werden, also der workerrolle `RunAsync` Methode wartet eine Sekunde vor dem Abruf erneut durch Aufrufen von `Task.Delay(1000)`.

In einem Webprojekt verbessert Hinzufügen von asynchronem Code automatisch Leistung, da IIS einen begrenzten Threadpool verwaltet. Das ist nicht der Fall in einem workerrollenprojekt hinzu. Zur Verbesserung der Skalierbarkeit der Worker-Rolle können Sie Code mit mehreren Threads schreiben oder asynchronen Code implementieren [parallelprogrammierung](https://msdn.microsoft.com/library/ff963553.aspx). Das Beispiel implementiert nicht die parallele Programmierung, aber es zeigt, wie der Code asynchron auszuführen, damit Sie die parallele Programmierung implementieren können.

## <a name="summary"></a>Zusammenfassung

In diesem Kapitel haben gezeigt, wie Sie die Reaktionsfähigkeit der Anwendung, die Zuverlässigkeit und Skalierbarkeit zu verbessern, indem Sie das warteschlangenorientierte-Muster implementieren.

Dies ist die letzte 13 Muster behandelt, die in diesem e-Book, aber es gibt selbstverständlich viele weitere Muster und Methoden, die Ihnen helfen können erfolgreiche Cloud-apps erstellen. Die [letzte Kapitel](more-patterns-and-guidance.md) enthält Links zu Ressourcen für Themen, in diese 13 Muster behandelt wurde noch nicht.

<a id="resources"></a>
## <a name="resources"></a>Ressourcen

Weitere Informationen zu Warteschlangen finden Sie unter den folgenden Ressourcen.

Dokumentation:

- [Microsoft Azure Storage-Warteschlangen Teil 1: Erste Schritte](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/). Artikel von Roman Schacherl.
- [Ausführen von Hintergrundaufgaben](https://msdn.microsoft.com/library/ff803365.aspx), Kapitel 5 des [Verschieben von Anwendungen in der Cloud, 3. Auflage](https://msdn.microsoft.com/library/ff728592.aspx) aus Microsoft Patterns and Practices. (Insbesondere der Abschnitt ["Mithilfe der Azure Storage-Warteschlangen"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Bewährte Methoden zum Maximieren der Skalierbarkeit und Wirtschaftlichkeit von Warteschlangenbasierten Messaging-Lösungen in Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). Whitepaper von Valery Mizonov.
- [Vergleich von Azure-Warteschlangen und Service Bus-Warteschlangen](https://msdn.microsoft.com/magazine/jj159884.aspx). MSDN Magazin-Artikel enthält zusätzliche Informationen, die Ihnen bei der Auswahl der Queue-Dienst verwenden kann. Der Artikel wird erwähnt, dass Service Bus ACS für die Authentifizierung abhängig dies, dass Ihre Warteschlangen SB wäre nicht verfügbar ist bedeutet, wenn ACS nicht verfügbar ist. Aber da der Artikel geschrieben wurde, SB wurde geändert, um Sie verwenden können [SAS-Token](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) als Alternative zum ACS.
- [Microsoft Patterns and Practices - Leitfaden zur Azure](https://msdn.microsoft.com/library/dn568099.aspx). Einführung in asynchrone Nachrichten Muster für Pipes und Filter "," Muster "kompensierende Transaktion" "," Muster "konkurrierende Consumer" "," Muster "CQRS" angezeigt.
- [CQRS-Reise](https://msdn.microsoft.com/library/jj554200). E-Book zu CQRS von Microsoft Patterns and Practices.

Video:

- [FailSafe: Erstellen von skalierbaren, robusten Clouddiensten](https://channel9.msdn.com/Series/FailSafe). Videoreihe neun-Teil von Marc Mercuri, Ulrich Homann und Mark Simms. Bietet allgemeine Konzepte und architektonischen Prinzipien auf eine Weise sehr zugegriffen werden kann und interessante Geschichten, die von Microsoft Customer Advisory Teams (CAT) die Erfahrung für tatsächliche Kunden gezeichnet werden. Eine Einführung in den Azure Storage-Dienst und Warteschlangen finden Sie in der Folge 5 35:13 ab.

> [!div class="step-by-step"]
> [Zurück](distributed-caching.md)
> [Weiter](more-patterns-and-guidance.md)
