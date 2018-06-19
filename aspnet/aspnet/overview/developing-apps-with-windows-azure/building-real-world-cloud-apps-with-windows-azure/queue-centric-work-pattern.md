---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
title: Warteschlange anwendungsorientierte Arbeit Muster (Building Real-World Cloud Apps with Azure) | Microsoft Docs
author: MikeWasson
description: Die Building Real World Cloud Apps with Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: cc1ad51b-40c3-4c68-8620-9aaa0fd1f6cf
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/queue-centric-work-pattern
msc.type: authoredcontent
ms.openlocfilehash: 124e673206ecea2eac5efb8c2802a32a690fa104
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875434"
---
<a name="queue-centric-work-pattern-building-real-world-cloud-apps-with-azure"></a>Warteschlange anwendungsorientierte Arbeit Muster (Building Real-World Cloud Apps with Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download Behebungsskript Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps with Azure** e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die Ihnen helfen können erfolgreich ausgeführt entwickeln Web-apps für die Cloud. Weitere Informationen zu e-Book herunterladen, finden Sie unter [im Kapitel über das erste](introduction.md).


Früher wurde erläutert, dass mehrere Dienste mithilfe eines "composite" SLA liefern kann, in dem die app effektive SLA ist die *Produkt* der einzelnen SLAs. Beispielsweise verwendet die app zu beheben, Websites, Speicher und SQL-Datenbank. Wenn eine dieser Dienste fehlschlägt, wird die app einen Fehler für den Benutzer zurück.

Caching ist eine gute Möglichkeit, vorübergehende Fehler für schreibgeschützten Inhalten zu behandeln. Aber was geschieht, wenn Ihre Anwendung benötigt, um arbeiten? Z. B. wenn der Benutzer eine neue korrigieren-Aufgabe sendet, kann nicht die app nur die Aufgabe in den Cache abgelegt werden soll. Die app muss die Aufgabe korrigieren in einen permanenten Datenspeicher schreiben verarbeitet werden können.

Hier kommt das Muster für die Warteschlange anwendungsorientierte Arbeit. Dieses Muster ermöglicht losen Kopplung zwischen eine Webebene und eine Back-End-Dienst.

Hier ist die Funktionsweise des Musters. Wenn die Anwendung eine Anforderung erhält, fügt eine Arbeitsaufgabe in einer Warteschlange und die Antwort wird sofort zurückgegeben. Klicken Sie dann ein separaten Back-End-Prozess von Arbeitselementen aus der Warteschlange abruft und die Arbeit ausführt.

Das Muster für die Warteschlange anwendungsorientierte Arbeit eignet sich für:

- Arbeit, die viel Zeit (hoher Latenz) ist.
- Arbeit, die einen externen Dienst erfordert, der möglicherweise nicht immer verfügbar.
- D. h. arbeiten ressourcenintensiven (hohe CPU-Auslastung).
- Arbeit, die von der Rate Abgleich (gemäß den Auslastungstest einen plötzlichen Anstieg) profitieren würden.

## <a name="reduced-latency"></a>Reduzierte Latenz

Warteschlangen sind hilfreich, jedes Mal, wenn Sie zeitintensive Arbeit verrichten. Wenn ein Vorgang ein paar Sekunden dauert oder länger betragen, anstatt zu blockieren, der Endbenutzer, die Arbeitsaufgabe in eine Warteschlange eingereiht. Dem Benutzer mitteilt, "Wir arbeiten daran", und klicken Sie dann einen warteschlangenlistener verwenden, um den Vorgang im Hintergrund zu verarbeiten.

Z. B. Wenn Sie etwas Onlinehändlers erwerben, bestätigt der Website Ihrer Bestellung sofort. Aber, die nicht bedeutet, dass Daten bereits in einem LKW übermittelt werden. Platzieren sie einen Task in einer Warteschlange, und im Hintergrund, sie sind auf diese Weise die kreditprüfung Vorbereiten Ihrer Elemente für die Auslieferung und usw.

Für Szenarien mit kurzen Latenz möglicherweise die End-to-End-Gesamtzeit mehr über eine Warteschlange, die im Vergleich mit den Task synchron ausführen. Aber selbst dann können die andere Vorteile dieser Nachteil überwiegen.

## <a name="increased-reliability"></a>Verbessert sich seine Zuverlässigkeit

In der Version des zu beheben, die wir bisher betrachtet haben, ist im Web-Front-End-eng mit dem Back-End-SQL-Datenbank verbunden. Wenn der SQL-Datenbankdienst nicht verfügbar ist, erhält der Benutzer einen Fehler auf. Wenn Wiederholungen nicht unterstützt (d. h. der Fehler ist mehr als vorübergehender), das einzige möglich ist, einen Fehler angezeigt wird, und ihn veranlassen, Sie es später erneut.

![Diagramm mit Web-Front-End-, bei denen Fehler aufgetreten, Fehler bei der SQL-Datenbank-Back-end](queue-centric-work-pattern/_static/image1.png)

Die app schreibt mithilfe von Warteschlangen, wenn ein Benutzer eine Aufgabe korrigieren übermittelt, eine Meldung, in die Warteschlange. Die Nachrichtennutzlast ist eine [JSON](http://json.org/) Darstellung des Tasks. Sobald die Nachricht an die Warteschlange geschrieben wurde, wird die app zurückgegeben und zeigt sofort eine Erfolgsmeldung an dem Benutzer.

Wenn der Back-End-Dienste – z. B. die SQL-Datenbank oder der warteschlangenlistener--offline geschaltet werden, können Benutzer weiterhin neue korrigieren-Aufgaben senden. Die Nachrichten werden nur in die Warteschlange, bis die Back-End-Dienste erneut zur Verfügung stehen. An diesem Punkt werden die Back-End-Dienste auf dem Backlog den aktuellen Stand.

![Diagramm mit der Web-Front-End zu fortfahren, funktionieren, wenn ein SQL-Datenbank beim Fehler](queue-centric-work-pattern/_static/image2.png)

Darüber hinaus können jetzt Sie weitere Back-End-Logik hinzufügen ohne Rücksicht auf die Stabilität von Front-End. Sie möchten z. B. eine e-Mail oder SMS-Nachricht an den Besitzer senden, wenn ein neues Update er zugewiesen wird. Die e-Mail-Adresse oder den SMS-Dienst nicht verfügbar, können Sie alles verarbeiten und dann eine Nachricht in eine separate Warteschlange zum Senden von e-Mail-/ SMS-Nachrichten eingefügt.

Bisher war unser effektiven SLA Web-Apps &times; Speicher &times; SQL-Datenbank = 99,7 %. (Siehe [Entwurf Fehlern überdauert](design-to-survive-failures.md).)

Wenn wir die app auf eine Warteschlange verwenden, ändern, hängt von der Web-front-End nur Web-Apps und Ihres Speichers, für eine zusammengesetzte SLA von 99,8 %. (Beachten Sie, dass Warteschlangen sind Teil des Diensts Azure-Speicher, damit sie in die gleiche SLA als Blob-Speicher enthalten sind.)

Wenn Sie auch eine bessere Leistung als 99,8 % benötigen, können Sie zwei Warteschlangen in zwei verschiedenen Regionen erstellen. Legen Sie eine als das primäre und der andere als sekundärer. In der app, und ein Failover auf die sekundäre Warteschlange, wenn die primäre Warteschlange nicht verfügbar ist. Die Wahrscheinlichkeit, dass beide nichtverfügbarkeit gleichzeitig ist sehr klein.

## <a name="rate-leveling-and-independent-scaling"></a>Rate Abgleich und unabhängige Skalierung

Warteschlangen sind auch nützlich sein, so genannte *rate Abgleich* oder *belastungsausgleich*.

Web-apps sind häufig anfällig für einen plötzlichen Anstieg Datenverkehr. Automatische Skalierung sind möglicherweise nicht schnell genug verarbeiten abrupte Spitzen in reagieren, während Sie automatische Skalierung verwenden können, automatisch hinzufügen von Webservern, um erhöhte Webdatenverkehr zu bewältigen. Wenn der Webserver Teil der Arbeit Auslagern können, indem eine Meldung an eine Warteschlange geschrieben haben, können sie mehr Datenverkehr zu bewältigen. Ein Back-End-Dienst kann dann Nachrichten aus der Warteschlange lesen und verarbeiten. Die Länge der Warteschlange wird vergrößert oder verkleinert werden, wie der eingehenden Last.

Mit großen Teil ihrer zeitaufwändig Arbeit, die an einen Back-End-Dienst verschoben kann die Webebene leichter auf einen plötzlichen Spitzen in der Datenverkehr reagieren. Und Sie dabei Geld sparen, da alle angegebenen Umfang des Datenverkehrs vom weniger Webserver verarbeitet werden kann.

Sie können die Web- und Back-End-Dienst unabhängig voneinander skalieren. Beispielsweise müssen Sie drei Webservern jedoch nur ein Server Verarbeitung Warteschlangennachrichten ggf. Oder wenn Sie einen rechenintensiven Vorgang im Hintergrund ausführen, könnten Sie mehrere Back-End-Server.

![](queue-centric-work-pattern/_static/image3.png)

Automatische Skalierung kann mit Back-End-Dienste sowie der Webebene verwendet werden. Sie können zu skalieren der Anzahl von virtuellen Computern, die den Aufgaben in der Warteschlange, basierend auf der CPU-Auslastung von Back-End-VMs verarbeitet werden. Oder, können Sie die automatische Skalierung basierend auf wie viele Elemente in der Warteschlange befinden. Beispielsweise können Sie automatische Skalierung, um zu versuchen, nicht mehr als 10 Elemente in der Warteschlange beibehalten erkannt. Wenn die Warteschlange mehr als 10 Elemente aufweist, wird zur automatischen Skalierung auf virtuelle Computer hinzufügen. Wenn sie sind soweit, wird zur automatischen Skalierung nach unten die zusätzlichen virtuellen Computer entfernen.

## <a name="adding-queues-to-the-fix-it-application"></a>Hinzufügen von die Korrektur wird Warteschlange Anwendung

Um die Warteschlange Muster zu implementieren, müssen wir zwei Änderungen an der app zu beheben.

- Wenn ein Benutzer eine neue korrigieren-Aufgabe sendet, platzieren Sie den Task in der Warteschlange, statt sie auf die Datenbank geschrieben.
- Erstellen Sie einen Back-End-Dienst, der Nachrichten in der Warteschlange verarbeitet.

Für die Warteschlange verwenden wir die [Azure Warteschlangenspeicherdiensts](https://www.windowsazure.com/develop/net/how-to-guides/queue-service/). Eine andere Möglichkeit ist die Verwendung [Azure Service Bus](https://docs.microsoft.com/azure/service-bus/).

Um zu entscheiden, welche Warteschlangendienst zu verwenden, beachten Sie, wie Ihre app benötigt zum Senden und Empfangen von Nachrichten in der Warteschlange:

- Wenn Sie kooperierender Producer und konkurrierende Consumer haben, sollten erwägen Sie, Azure-Warteschlangendienst-Speicher zu verwenden. "Producer kooperieren" bedeutet, dass mehrere Prozesse Nachrichten an eine Warteschlange hinzufügen. "Konkurrierenden Consumern" bedeutet, dass mehrere Prozesse werden Nachrichten aus der Warteschlange verarbeiten abzufangen, jedoch einer beliebigen vorhandenen Nachricht kann nur von einem "Consumer" verarbeitet werden Wenn Sie einen höheren Durchsatz, als Sie mit einer einzelnen Warteschlange erhalten möchten, verwenden Sie zusätzliche Warteschlangen und/oder zusätzliche Speicherkonten.
- Wenn Sie müssen eine [Veröffentlichen/Abonnieren-Modell](http://en.wikipedia.org/wiki/Publish/subscribe), erwägen Sie die Azure Service Bus-Warteschlangen.

Die app zu beheben passt die kooperierender Producer und konkurrierende Consumer-Modell.

Ein weiterer Aspekt ist die Verfügbarkeit der Anwendung. Warteschlangenspeicherdiensts ist Teil des gleichen Diensts, die wir für den blobspeicher verwenden, damit Verwendung keine Auswirkungen auf unserem SLA verfügt. Azure Service Bus ist einem separaten Dienst mit einem eigenen SLA. Wenn wir Service Bus-Warteschlangen verwendet, hätten wir, eine zusätzliche SLA-Prozentsatz zu berücksichtigen und unsere zusammengesetzten SLA würde geringer sein. Wenn Sie ein Warteschlangendienst auswählen möchten, stellen Sie sicher, dass Sie Ihrer Wahl auf die Verfügbarkeit der Anwendung auswirkt. Weitere Informationen finden Sie unter der [Ressourcen](#resources) Abschnitt.

## <a name="creating-queue-messages"></a>Erstellen von Nachrichten in Warteschlange

Um eine Aufgabe zu beheben in der Warteschlange zu versetzen, führt das Web-front-End die folgenden Schritte aus:

1. Erstellen einer [CloudQueueClient](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueueclient.aspx) Instanz. Die `CloudQueueClient` Instanz wird verwendet, um die Anforderungen für den Warteschlangendienst ausgeführt werden.
2. Erstellen Sie die Warteschlange aus, wenn er noch nicht vorhanden ist.
3. Serialisieren Sie die Aufgabe zu beheben.
4. Rufen Sie [CloudQueue.AddMessageAsync](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.queue.cloudqueue.addmessageasync.aspx) , die Nachricht die Warteschlange eingefügt werden soll.

Wir erledigen Sie diese dem Konstruktor arbeiten und `SendMessageAsync` Methode eines neuen `FixItQueueManager` Klasse.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample1.cs?highlight=11-12,16,18-25)]

Hier verwenden wir die [Json.NET](https://github.com/JamesNK/Newtonsoft.Json) Bibliothek die Fixit in JSON-Format serialisiert. Sie können unabhängig vom gewählten Ansatz Serialisierung gewünscht. JSON hat den Vorteil, dass lesbare, während weniger aufwändig als XML.

Professionell Code würde Fehlerbehandlungslogik hinzufügen, anhalten, wenn die Datenbank nicht verfügbar war Recovery mehr ordnungsgemäß behandeln, die Warteschlange beim Start der Anwendung erstellen und verwalten "[für potenziell schädliche Nachrichten" Nachrichten](https://msdn.microsoft.com/library/ms789028(v=vs.110).aspx). (Eine nicht verarbeitbare Nachricht ist eine Nachricht, die aus irgendeinem Grund nicht verarbeitet werden kann. Sie möchten schließlich nicht nicht verarbeitbare Nachrichten ungefähr in der Warteschlange, in denen die Worker-Rolle fortlaufend versucht, verarbeiten, schlägt fehl, versuchen Sie es erneut, fehlschlagen und usw.)

In der Front-End-MVC-Anwendung müssen Sie den Code aktualisieren, der eine neue Aufgabe erstellt. Rufen Sie probieren die Aufgabe, in das Repository, Sie anstelle der `SendMessageAsync` oben gezeigten Methode.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample2.cs?highlight=10)]

## <a name="processing-queue-messages"></a>Verarbeiten von Nachrichten in Warteschlange

Um Nachrichten in der Warteschlange zu verarbeiten, erstellen wir einen Back-End-Dienst. Der Back-End-Dienst wird eine unendliche Schleife ausgeführt werden, die die folgenden Schritte ausführt:

1. Die nächste Nachricht aus der Warteschlange abrufen.
2. Deserialisieren Sie die Nachricht an eine Aufgabe zu beheben.
3. Schreiben Sie den Task korrigieren in die Datenbank.

Um die Back-End-Dienst hosten, erstellen wir ein Azure-Cloud-Dienst, der enthält einem *workerrolle*. Eine workerrolle besteht aus einem oder mehreren virtuellen Computern, die Back-End-Verarbeitung ausführen können. Der Code, der in diesen virtuellen Computern ausgeführt wird ruft Nachrichten aus der Warteschlange, sobald diese verfügbar sind. Für jede Nachricht wir deserialisiert die JSON-Nutzlast und eine Instanz der Entität beheben Task in die Datenbank schreiben, die durch das verwenden dieselben Repositorys, das wir zuvor auf der Webebene verwendet.

Die folgenden Schritte zeigen, wie einen Worker hinzufügen rollenprojekt zu einer Projektmappe, die ein standard-Web-Projekt enthält. Diese Schritte wurden bereits im Projekt beheben sie ausgeführt wurde, die Sie herunterladen können.

Fügen Sie ein Cloud-Dienstprojekt hinzu, die Visual Studio-Projektmappe. Mit der rechten Maustaste in der Projektmappe, und wählen Sie **hinzufügen**, klicken Sie dann **neues Projekt**. Erweitern Sie im linken Bereich **Visual C#-** , und wählen Sie **Cloud**.

[![](queue-centric-work-pattern/_static/image5.png)](queue-centric-work-pattern/_static/image4.png)

In der **neuen Azure-Cloud-Dienst** Dialogfeld erweitern Sie die **Visual C#-** Knoten im linken Bereich. Wählen Sie **Workerrolle** , und klicken Sie auf das Symbol "Pfeil nach rechts".

![](queue-centric-work-pattern/_static/image6.png)

(Beachten Sie, die Sie auch hinzufügen, können ein *Webrolle*. Wir konnten die korrigieren sie Front-End-im selben Cloud-Dienst statt es in ein Azure-Website ausgeführt werden. Die weist einige Vorteile in herstellen von Verbindungen zwischen den Front-End- und Back-End einfacher zu koordinieren. Allerdings um diese Demo einfach zu halten, wir die Beibehaltung von Front-End in eine Azure App Service-Web-App und nur im Back-End in einem Cloud-Dienst ausgeführt.)

Der Worker-Rolle wird ein Standardname zugewiesen. Um den Namen ändern möchten, zeigen Sie mit der Maus über die Worker-Rolle im rechten Bereich, und klicken Sie auf das Stiftsymbol.

![](queue-centric-work-pattern/_static/image7.png)

Klicken Sie auf **OK** um das Dialogfeld "abzuschließen. Dadurch wird der Visual Studio-Projektmappe zwei Projekte hinzugefügt.

- ein Azure-Projekt, das den Cloud-Dienst, einschließlich Konfigurationsinformationen definiert.
- Ein Workerrollen-Projekt, das die workerrolle definiert.

![](queue-centric-work-pattern/_static/image8.png)

Weitere Informationen finden Sie unter [erstellen ein Azure-Projekts mit Visual Studio.](https://msdn.microsoft.com/library/windowsazure/ee405487.aspx)

Innerhalb der Worker-Rolle wir Nachrichten abrufen durch Aufrufen der `ProcessMessageAsync` Methode der `FixItQueueManager` -Klasse, die wir bereits kennen.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample3.cs?highlight=25)]

Die `ProcessMessagesAsync` Methode überprüft, ob es eine Nachricht wartet. Sofern vorhanden, deserialisiert sie die Nachricht in eine `FixItTask` Entität und speichert die Entität in der Datenbank. Diese Schleife, bis die Warteschlange leer ist.

[!code-csharp[Main](queue-centric-work-pattern/samples/sample4.cs)]

Abrufen der Warteschlangennachrichten verursacht eine kleine Transaktion in Rechnung zu stellen, wenn keine Nachricht, die noch verarbeitet werden müssen, es ist also der Worker-Rolle `RunAsync` -Methode wartet vor dem Abruf erneut durch Aufrufen von `Task.Delay(1000)`.

In einem Webprojekt verbessert Hinzufügen von asynchronen Code automatisch Leistung, da IIS einen begrenzten Threadpool verwaltet. Also nicht der Fall in einem workerrollenprojekt. Zum Verbessern der Skalierbarkeit der workerrolle können Sie mit mehreren Threads Code schreiben oder verwenden Sie asynchronen Code implementieren [parallele Programmierung](https://msdn.microsoft.com/library/ff963553.aspx). Das Beispiel implementiert nicht die parallele Programmierung, jedoch wird gezeigt, wie der Code asynchron auszuführen, damit Sie parallele Programmierung implementieren können.

## <a name="summary"></a>Zusammenfassung

In diesem Kapitel haben gezeigt, wie Sie die Reaktionsfähigkeit der Anwendung, Zuverlässigkeit und Skalierbarkeit zu verbessern, indem Sie die Implementierung des Musters Warteschlange anwendungsorientierte arbeiten.

Dies ist die letzte 13 Mustern in diesem e-Book behandelt, aber natürlich zahlreiche andere Muster vorhanden sind, und Methoden, die Ihnen helfen können erstellen erfolgreich Cloud-apps. Die [endgültigen Kapitel](more-patterns-and-guidance.md) enthält Links zu Ressourcen für Themen, die noch nicht in diese 13 Muster behandelt wurde.

<a id="resources"></a>
## <a name="resources"></a>Ressourcen

Weitere Informationen zu Warteschlangen finden Sie unter den folgenden Ressourcen.

Dokumentation:

- [Microsoft Azure-Speicher-Warteschlangen Teil 1: Erste Schritte](http://justazure.com/microsoft-azure-storage-queues-part-1-getting-started/). Artikel von Roman Schacherl.
- [Ausführen von Hintergrundaufgaben](https://msdn.microsoft.com/library/ff803365.aspx), Kapitel 5 [Verschieben von Anwendungen in der Cloud, 3. Edition](https://msdn.microsoft.com/library/ff728592.aspx) aus Microsoft Patterns and Practices. (Insbesondere den Abschnitt ["Mithilfe der Azure-Speicher-Warteschlangen"](https://msdn.microsoft.com/library/ff803365.aspx#sec7).)
- [Bewährte Methoden zum Maximieren der Skalierbarkeit und Wirtschaftlichkeit von Warteschlangenbasierten Messaging-Lösungen unter Azure](https://msdn.microsoft.com/library/windowsazure/hh697709.aspx). Whitepaper von Valery Mizonov.
- [Vergleich zwischen Azure-Warteschlangen und Servicebus-Warteschlangen](https://msdn.microsoft.com/magazine/jj159884.aspx). MSDN Magazine-Artikel enthält zusätzliche Informationen, die Ihnen bei der Auswahl der Warteschlangendienst verwenden kann. Der Artikel erwähnt, dass Service Bus ACS für die Authentifizierung abhängig dies, dass die Servicebus-Warteschlangen wäre nicht verfügbar ist bedeutet, wenn ACS nicht verfügbar ist. Jedoch, da der Artikel geschrieben wurde, SB wurde geändert, um Sie verwenden können [SAS-Token](https://msdn.microsoft.com/library/windowsazure/dn170477.aspx) als Alternative zum ACS.
- [Microsoft Patterns and Practices - Azure-Leitfaden](https://msdn.microsoft.com/library/dn568099.aspx). Finden Sie unter Primer asynchrones Messaging, Pipes und Filter (Muster), kompensierende Transaktion Muster, konkurrierende Consumer-Muster, CQRS-Muster.
- [CQRS-Reise](https://msdn.microsoft.com/library/jj554200). E-Book zu CQRS vom Microsoft Patterns and Practices.

Video:

- [FailSafe: Erstellen von skalierbaren, robusten Cloud-Dienste](https://channel9.msdn.com/Series/FailSafe). Videoreihe Marc Mercuri, Ulrich Homann und Mark Simms neun Teil. Bietet allgemeine Konzepte und Architekturprinzipien auf eine Weise zugegriffen werden kann, und interessante Storys, die von Microsoft Customer Advisory Team (CAT) anstelle von Erfahrungen mit Kunden gezeichnet. Eine Einführung in die Azure Storage-Dienst. und Warteschlangen finden Sie in der Folge 5 35:13 ab.

> [!div class="step-by-step"]
> [Zurück](distributed-caching.md)
> [Weiter](more-patterns-and-guidance.md)
