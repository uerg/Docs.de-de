---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Vorübergehende Fehlerbehandlung (Real-World Cloud Apps with Azure erstellen) | Microsoft Docs
author: MikeWasson
description: Die Building Real World Cloud Apps with Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/03/2015
ms.topic: article
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: 86bd67b04931ae2452f6e063e6475a434a0125bc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873078"
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Vorübergehende Fehlerbehandlung (Real-World Cloud Apps with Azure erstellen)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download Behebungsskript Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps with Azure** e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die Ihnen helfen können erfolgreich ausgeführt entwickeln Web-apps für die Cloud. Weitere Informationen zu e-Book herunterladen, finden Sie unter [im Kapitel über das erste](introduction.md).


Wenn Sie eine reale Welt Cloud-app entwerfen, ist eine Dinge, die im Wesentlichen stehen Ihnen temporäre dienstunterbrechungen zu behandeln. Dieses Problem ist eindeutig in der Cloud-apps wichtig, da Sie so Netzwerkverbindungen und externen Diensten abhängig sind. Erhalten Sie häufig nur wenig Problemen, die in der Regel Selbstkorrektur müssen entstehen, und wenn Sie nicht auf intelligente Weise handhaben vorbereitet sind, sie in eine schlechte Erfahrung für Ihre Kunden.

## <a name="causes-of-transient-failures"></a>Ursachen für vorübergehende Fehler

In der Cloud-Umgebung finden Sie, dass Fehler und Datenbank in regelmäßigen Abständen geschehen, Verbindungen getrennt. Das ist teilweise verwendet werden, da Sie zumindest die offensichtlichen über mehrere Lastenausgleichsmodule, die im Vergleich zu der lokalen Umgebung, in Ihrem Webserver und Datenbankserver eine direkte physische Verbindung haben. Darüber hinaus in einigen Fällen, wenn Sie ein mehrinstanzfähiger Dienst abhängig sind sehen Aufrufe des Diensts Get langsamer oder Timeout Sie, weil eine andere Person, die der Dienst verwendet stark aktiviert ist. In anderen Fällen möglicherweise der Benutzer, der den Dienst zu häufig aktiviert ist, und der Dienst schränkt Sie die – Verbindungen verweigert – absichtlich um zu verhindern, dass Sie negative Auswirkungen auf andere Mandanten des Diensts.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Verwenden Sie intelligenten Wiederholung/backofflogik bereitstellen, um die Auswirkung von vorübergehenden Ausfällen zu verringern

Statt eine Ausnahme auszulösen, und eine Seite nicht verfügbare oder Fehler an den Kunden anzeigen, können Sie Fehler, die in der Regel vorübergehend sind erkennen und automatisch wiederholen Sie den Vorgang mit der der Fehler erhofft sich, die vor dem lange Sie erfolgreich ausgeführt werden. In den meisten Fällen wird der Vorgang beim zweiten Versuch erfolgreich, und Sie müssen des Fehlers wiederherstellen, ohne dass der Kunde, dass jemals Beachten Sie, dass ein Problem aufgetreten.

Es gibt mehrere Möglichkeiten, die Sie intelligente Wiederholungslogik implementieren können.

- Die Microsoft Patterns &amp; Practices-Gruppe verfügt über eine [vorübergehenden Anwendungsblock zur Behandlung](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) , die erledigt alles für Sie bei Verwendung von ADO.NET für den SQL-Datenbankzugriff (nicht über Entity Framework). Sie legen nur eine Richtlinie für Wiederholungen – wie viele Male auf, um eine Abfrage zu wiederholen oder Befehl und warten Sie, wie lange zwischen versucht – und Wrap Ihrer SQL code in eine *mit* Block.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    TFH unterstützt auch [Azure In-Role Cache](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) und [Service Bus](https://azure.microsoft.com/services/service-bus/).
- Bei Verwendung von Entity Framework werden nicht in der Regel arbeiten Sie direkt mit der SQL-benutzerverbindungen Entity Framework 6 dieser Art von Logik für Wiederholungsversuche direkt in das Framework erstellt, sodass dieses Patterns and Practices-Paket kann nicht verwendet. Auf ähnliche Weise Geben Sie die wiederholungsstrategie und EF verwendet dann diese Strategie, wenn er auf die Datenbank zugreift.

    Zum Verwenden dieser Funktion in der app zu beheben, müssen Sie tun, lediglich eine Klasse hinzuzufügen, die abgeleitet *DbConfiguration* und aktivieren Sie die Wiederholungslogik erneut.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Für SQL-Datenbank-Ausnahmen, die das Framework als in der Regel vorübergehende Fehler identifiziert wird, weist der Pseudocode EF Wiederholung des Vorgangs bis zu 3 Mal mit einem exponentiellen Backoff-Verzögerung zwischen Wiederholungen und einer maximalen Verzögerung von 5 Sekunden. Exponential Backoff bedeutet, dass nach jeder fehlerhaften Wiederholung für einen längeren Zeitraum vor dem Wiederholen des Vorgangs gewartet wird. Wenn drei Versuche in einer Zeile ein Fehler auftritt, wird eine Ausnahme ausgelöst. Im folgenden Abschnitt zu Trennschalter wird erläutert, warum exponentiellen Backoff aus, und eine begrenzte Anzahl von Wiederholungen werden soll.

    Sie können ähnliche Probleme haben, wenn Sie den Azure-Speicherdienst verwenden, wie die app zu beheben für Blobs ist und die .NET Storage Client API bereits die gleiche Art von Logik implementiert. Geben Sie einfach die wiederholungsrichtlinie, oder Sie müssen auch nicht, wenn Sie mit den Standardeinstellungen zufrieden sind.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Trennschalter

Es gibt verschiedene Gründe, warum Sie oft über zu langen Zeitraum wiederholen möchten:

- Zu viele Benutzer nach mehreren versuchen weiterhin Wiederholung Anforderungsfehler möglicherweise andere benutzererfahrung beeinträchtigt werden. Wenn Millionen von Menschen sind, dass alle ausführenden wiederholte Wiederholen von Anforderungen konnten Sie IIS Dispatch Warteschlangen binden und verhindert, dass Ihre app Wartung Anforderungen, die es andernfalls erfolgreich verarbeiten konnte.
- Wenn "Jeder" aufgrund eines Dienstfehlers verwendet wird, gibt es wiederholt so viele Anforderungen in die Warteschlange werden konnte, dass der Dienst beim Starten zum Wiederherstellen geleitete abruft.
- Wenn der Fehler aufgrund einer Einschränkung anwendet wird, und es ein Zeitfenster ist für die Einschränkung der Dienst verwendet, könnte fortgesetzte Wiederholungen dieses Fensters verschieben und dazu führen, dass die Einschränkung, um den Vorgang fortzusetzen.
- Sie müssen möglicherweise einen Benutzer warten auf eine Webseite gerendert. Festlegen, dass Personen Wartezeit zu lang möglicherweise mehr störende werden diese relativ schnell ihm mitgeteilt wird, versuchen Sie es später noch einmal.

Exponential Backoff Adressen einige dieser Probleme durch die Begrenzung der Häufigkeit der Wiederholungen, die ein Dienst von der Anwendung erhalten kann. Aber Sie benötigen auch *Trennschalter*: Dies bedeutet, dass an einem bestimmten Schwellenwert für Ihre app wird beendet, und wiederholen Sie dann und akzeptiert eine andere Aktion, z. B. einen der folgenden Wiederholen:

- Benutzerdefiniertes fallback. Wenn Sie einen Aktienkurs von Reuters abrufen können, können Sie es vielleicht von Bloomberg abrufen; oder wenn Sie Daten aus der Datenbank abrufen können, ist möglicherweise erhalten Sie es aus dem Cache.
- Fehler im Hintergrund. Wenn von einem Dienst, benötigen Sie nichts für Ihre app nicht, nur gibt null zurück, wenn Sie die Daten abrufen können. Sie können anzeigen, eine Aufgabe zu beheben, und der Blob-Dienst antwortet nicht, konnte die Taskdetails auf ohne das Bild angezeigt werden.
- Fehlschlagen Sie schnell. Fehler ausgegeben der Benutzer wird für den Dienst mit wiederholen Sie den Anforderungen für die Unterbrechung des Diensts für andere Benutzer oder eine einschränkungszeitfensters erweitern konnte. Sie können eine Meldung aus "versuchen Sie es später" anzeigen.

Es gibt keine allgemeingültigen wiederholungsrichtlinie. Sie können mehrere Male wiederholen und mehr in Warten eines asynchronen Hintergrundprozesses Worker als in einer synchronen Web-app verwenden, in denen ein Benutzer auf eine Antwort wartet. Sie können mehr zwischen den Wiederholungsversuchen für einen relationalen Datenbankdienst warten, als Sie für einen Cachedienst würden. Hier sind einige Beispiele für empfohlene wiederholungsrichtlinien bieten einen Überblick darüber, wie die Zahlen variieren können. ("Schnelle erste" bedeutet keine Verzögerung vor dem ersten Wiederholungsversuch.

![Beispiel-wiederholungsrichtlinien](transient-fault-handling/_static/image1.png)

SQL-Datenbank Wiederholung Richtlinie Anleitungen finden Sie unter [Problembehandlung bei vorübergehenden Fehlern und Verbindungsfehlern mit SQL-Datenbank](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Zusammenfassung

Eine Wiederholung/backoffstrategie können temporäre Fehler an den Kunden in den meisten Fällen unsichtbar, und Microsoft stellt Frameworks, die Sie verwenden können, minimiert die Arbeit, die Implementierung einer Strategie, ob Sie ADO.NET, Entity Framework oder Azure verwenden Storage-Dienst.

In der [nächsten Kapitels](distributed-caching.md), betrachten wir zum Verbessern der Leistung und Zuverlässigkeit mit Verteiltes Zwischenspeichern.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in den folgenden Ressourcen:

Dokumentation

- [Bewährte Methoden für den Entwurf umfangreicher Dienste auf Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Whitepaper von Mark Simms und Michael Thomassy. Vergleichbar mit dem Failsafe-Serie, aber wechselt in den Gewusst-wie-Informationen. Finden Sie im Abschnitt Telemetrie und Diagnose.
- [Failsafe: Leitfaden zu Resilienten Cloudarchitekturen](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Whitepaper von Marc Mercuri, Ulrich Homann und Andrew Townhill. Version der FailSafe-Videoreihe Webseite.
- [Microsoft Patterns and Practices - Azure-Leitfaden](https://msdn.microsoft.com/library/dn568099.aspx). Finden Sie unter "Wiederholen" Muster, Scheduler Agent Supervisor-Muster.
- [Fehlertoleranz in Azure SQL-Datenbank](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Der Blogbeitrag von Tony Petrossian.
- [Entity Framework - Verbindungsstabilität / Wiederholungslogik](https://msdn.microsoft.com/data/dn456835). Informationen zum verwenden und Anpassen der Funktion von Entity Framework 6 behandeln vorübergehenden Fehlers.
- [Verbindungsresilienz und Abfangen der Befehl mit dem Entity Framework in einer ASP.NET MVC-Anwendung](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Vierte veranschaulicht in einer Reihe Tutorial neun-Teil der EF-6 Verbindung ausfallsicherung durch die Funktion für SQL-Datenbank einrichten.

Videos

- [FailSafe: Erstellen von skalierbaren, robusten Cloud-Dienste](https://channel9.msdn.com/Series/FailSafe). Neun zweiteilige Reihe Marc Mercuri, Ulrich Homann und Mark Simms. Bietet allgemeine Konzepte und Architekturprinzipien auf eine Weise zugegriffen werden kann, und interessante Storys, die von Microsoft Customer Advisory Team (CAT) anstelle von Erfahrungen mit Kunden gezeichnet. Finden Sie unter der Beschreibung der Trennschalter in Folge 3 beginnenden 40:55.
- [Erstellen von Big: Erkenntnisse aus Azure-Kunden – Teil II](https://channel9.msdn.com/Events/Build/2012/3-030). Mark Simms Gespräche zum Entwerfen für vorübergehende Fehler Fehler behandeln, und alles instrumentieren.

Codebeispiel

- [Grundlagen von Clouddiensten in Azure Cloud](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Beispielanwendung erstellt, indem die Microsoft Azure-Kundenberatungsteam, die zeigt, wie die [Enterprise Library vorübergehende Behandlung von Fehlerblock](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Weitere Informationen finden Sie unter [Cloud-Dienst-Grundlagen Datenzugriffsebene – vorübergehender Fehler behandeln](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH wird empfohlen, für den Datenbankzugriff mithilfe von ADO.NET direkt (ohne Verwendung von Entity Framework).

> [!div class="step-by-step"]
> [Zurück](monitoring-and-telemetry.md)
> [Weiter](distributed-caching.md)
