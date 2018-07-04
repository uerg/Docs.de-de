---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
title: Vorübergehende Fehlerbehandlung (erstellen realer Cloud-Apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/03/2015
ms.topic: article
ms.assetid: 7ead83bc-c08c-4b26-8617-00e07292e35c
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling
msc.type: authoredcontent
ms.openlocfilehash: 13ed8c2373c22070d21519bc495161e956b0ac4d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37398413"
---
<a name="transient-fault-handling-building-real-world-cloud-apps-with-azure"></a>Vorübergehende Fehlerbehandlung (erstellen realer Cloud-Apps mit Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download korrigieren Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps mit Azure** e-Book basiert darauf, dass eine Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Weitere Informationen zu e-Book, finden Sie unter [im ersten Kapitel](introduction.md).


Beim Entwerfen einer realen Cloud-Apps, ist eines der Dinge zu Bedenken haben wie temporäre dienstunterbrechungen behandelt. Dieses Problem ist in Cloud-apps eindeutig wichtig, weil Sie so von Verbindungen mit Netzwerken und externen Diensten abhängig sind. Erhalten Sie häufig wenig Störungen, die in der Regel selbst heilen sind, und wenn Sie nicht darauf vorbereitet, um sie auf intelligente Weise zu behandeln sind, werden führen dazu, dass eine schlechte Erfahrung für Ihre Kunden.

## <a name="causes-of-transient-failures"></a>Ursachen für vorübergehende Fehler

Finden Sie in der Cloud-Umgebung, die Fehler, und gelöschte Datenbank, die Verbindungen in regelmäßigen Abständen auftreten. Das ist teilweise daran, dass Sie also über mehrere Load balancer im Vergleich zu der lokalen Umgebung, in dem Ihr Webserver und Datenbankserver eine direkte physische Verbindung haben. Darüber hinaus manchmal, wenn Sie ein mehrinstanzenfähiger Dienst abhängig sind sehen Aufrufe des Diensts Get langsamer oder Timeout Sie, da eine andere Person, die der Dienst nutzt es stark erreicht. In anderen Fällen möglicherweise der Benutzer, der den Dienst zu häufig erreicht, und der Dienst drosselt Sie – Verbindungen verweigert – absichtlich um zu verhindern, dass Sie sich negativ auf die anderen Mandanten des Diensts.

## <a name="use-smart-retryback-off-logic-to-mitigate-the-effect-of-transient-failures"></a>Verwenden Sie intelligente wiederholen/Backoff-Logik, um die Auswirkungen der vorübergehende Fehler zu vermeiden

Anstatt eine Ausnahme auszulösen, und eine Seite nicht verfügbare oder Fehler an den Kunden anzeigen, können Sie erkennen, Fehler, die in der Regel vorübergehend sind, und automatisch wiederholen Sie den Vorgang mit der der Fehler erhofft sich, die vor dem lange Sie erfolgreich sein werden. In den meisten Fällen wird beim zweiten Mal wird der Vorgang erfolgreich, und Sie werden auf den Fehler wiederherstellen, ohne dass der Kunde, dass jemals bewusst, dass ein Problem aufgetreten.

Es gibt mehrere Möglichkeiten, die Sie intelligente Wiederholungslogik implementieren können.

- Der Microsoft Patterns &amp; Practices-Gruppe verfügt über eine [Transient Fault Handling Application Block](https://msdn.microsoft.com/library/dn440719(v=pandp.60).aspx) , die alles, was für Sie ist, wenn Sie ADO.NET für den Zugriff von SQL-Datenbank (nicht über Entity Framework) verwenden. Sie legen Sie einfach eine Richtlinie für Wiederholungsversuche – wie viele Male wiederholen eine Abfrage oder Befehl und warten Sie, wie lange zwischen versucht – und Wrap Ihrer SQL-code in einem *mit* Block.

    [!code-csharp[Main](transient-fault-handling/samples/sample1.cs)]

    TFH unterstützt auch [Azure In-Role Cache](https://msdn.microsoft.com/library/windowsazure/dn386103.aspx) und [Service Bus](https://azure.microsoft.com/services/service-bus/).
- Bei der Verwendung von Entity Framework werden nicht in der Regel Arbeit direkt mit SQL-Verbindungen können Sie also dieses Muster und Vorgehensweisen-Paket kann nicht verwendet werden, aber die Entity Framework 6 wird diese Art von Wiederholungslogik in das Framework erstellt. Auf ähnliche Weise Geben Sie die wiederholungsstrategie, und klicken Sie dann EF verwendet diese Strategie, wenn sie die Datenbank zugreift.

    Um dieses Feature in der Fix It-app verwenden zu können, müssen wir lediglich ist eine Klasse hinzugefügt werden, die von abgeleitet *"dbconfiguration"* und aktivieren Sie die Wiederholungslogik.

    [!code-csharp[Main](transient-fault-handling/samples/sample2.cs)]

    Für SQL-Datenbank-Ausnahmen, die das Framework als in der Regel vorübergehende Fehler zu identifizieren, weist der Code EF den Vorgang mit einer exponentiellen Backoff-Verzögerung zwischen Wiederholungen und einer maximalen Verzögerung von fünf Sekunden bis zu 3 Mal zu wiederholen. Exponentielles Backoff bedeutet, dass nach einzelnen fehlerhaften Wiederholungsversuchen für einen längeren Zeitraum vor dem erneuten Versuch gewartet wird. Wenn drei Versuche in einer Zeile ein Fehler auftritt, wird eine Ausnahme ausgelöst. Im folgenden Abschnitt über Trennschalter erläutert Exponentielles Backoff und eine begrenzte Anzahl von Wiederholungen Gründe für die.

    Sie können ähnliche Probleme auftreten, wenn Sie den Azure Storage-Dienst verwenden, wie die Fix It-app für Blobs ist, und die .NET Storage Client-API bereits die gleiche Art von Logik implementiert. Geben Sie einfach die wiederholungsrichtlinie, oder Sie müssen auch nicht, wenn Sie mit den Standardeinstellungen zufrieden sind.

<a id="circuitbreakers"></a>
## <a name="circuit-breakers"></a>Schutzschalter

Es gibt verschiedene Gründe, warum Sie nicht zu oft über zu langen Zeitraum wiederholen möchten:

- Zu viele Benutzer dauerhaft Wiederholung fehlerhafte Anforderungen möglicherweise andere Benutzer beeinträchtigt werden. Wenn Millionen von Menschen sind, sodass wiederholte Anforderungen erneut können Sie IIS Dispatch Warteschlangen beschäftigen und verhindert, dass Ihre app verarbeitet Anforderungen, die es andernfalls erfolgreich verarbeiten konnte.
- Wenn alle aufgrund eines Dienstfehlers gibt es wiederholt so viele Anforderungen Warteschlange eingereiht werden können, dass der Dienst überflutet beim Starten zum Wiederherstellen wird.
- Wenn der Fehler aufgrund einer Einschränkung wird, und ein Zeitfenster für die Einschränkung der Dienst verwendet, können weitere Wiederholungsversuche verschieben das Fenster und dazu führen, dass die Drosselung, um den Vorgang fortzusetzen.
- Sie müssen möglicherweise einen Benutzer warten auf eine Webseite zu rendern. Können Personen Wartezeit zu lang. möglicherweise mehr es ziemlich ärgerlich sein, relativ schnell ihm mitgeteilt wird, versuchen Sie es später noch mal.

Exponentielles Backoff behandelt einige dieser Probleme durch beschränken die Häufigkeit der Wiederholungen, die ein Dienst aus Ihrer Anwendung erhalten kann. Jedoch müssen Sie auch haben *Schutzschalter*: Dies bedeutet, dass auf einen bestimmten Schwellenwert wiederholen Sie dann Ihre app wird beendet, Wiederholung und verwendet eine andere Aktion, z. B. einen der folgenden:

- Benutzerdefiniertes fallback. Wenn Sie den Aktienkurs aus Reuters abrufen können, können Sie es vielleicht aus Bloomberg abrufen; oder wenn Sie Daten aus der Datenbank nicht, vielleicht können Sie es aus dem Cache.
- Ein Fehler auf automatische. Wenn nichts für Ihre app nicht was Sie von einem Dienst, nur gibt null zurück, wenn Sie die Daten zugreifen können. Wenn Sie einen Fix It-Task anzeigen sind und der Blob-Dienst antwortet nicht, können Sie die Taskdetails ohne das Bild darstellen.
- Fail-fast. Fehler, den Benutzer überflutet wird des Diensts mit, wiederholen Sie den Anforderungen, die dazu führen, dass die dienstunterbrechung für andere Benutzer oder eine einschränkungszeitfensters erweitern können. Sie können eine Meldung "versuchen Sie es später noch Mal" anzeigen.

Es gibt keine allgemeingültige wiederholungsrichtlinie. Sie können weitere Wiederholungsversuche und warten mehr in einem asynchronen Hintergrundprozess Worker, als Sie in einer synchronen Web-app tun würden, in denen ein Benutzer auf eine Antwort wartet. Sie können mehr zwischen den Wiederholungsversuchen für einen relationalen Datenbankdienst warten, als dies bei einem Cachedienst. Hier sind einige empfohlene Beispiel wiederholungsrichtlinien erhalten Sie eine Vorstellung davon, wie die Nummern variieren können. ("Schnelle erste" bedeutet, dass keine Verzögerung vor dem ersten Wiederholungsversuch.

![Beispiel-wiederholungsrichtlinien](transient-fault-handling/_static/image1.png)

SQL-Datenbank "Wiederholen" Richtlinie Anleitungen finden Sie [Problembehandlung bei vorübergehenden Fehlern und Verbindungsfehler mit SQL-Datenbank](https://azure.microsoft.com/documentation/articles/sql-database-connectivity-issues/).

## <a name="summary"></a>Zusammenfassung

Eine Wiederholung/Backoff-Strategie können temporäre Fehler unsichtbar für den Kunden in den meisten Fällen zu machen und Microsoft bietet Frameworks, mit denen Sie minimieren Ihre Arbeit, die Implementierung einer Strategie, ob Sie mit ADO.NET, Entity Framework oder die Azure nutzen Storage-Dienst.

In der [im nächsten Kapitel](distributed-caching.md), betrachten wir wie zur Verbesserung der Leistung und Zuverlässigkeit mit Verteiltes Zwischenspeichern.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in den folgenden Ressourcen:

Dokumentation

- [Bewährte Methoden für den Entwurf umfangreicher Dienste auf Azure Cloud Services](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). Whitepaper von Mark Simms und Michael Thomassy. Ähnlich wie die Failsafe-Serie, aber wird auf Weitere Gewusst-wie-Details. Finden Sie im Abschnitt Telemetrie und Diagnose.
- [Failsafe: Leitfaden zu Resilienten Cloudarchitekturen](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). Whitepaper von Marc Mercuri, Ulrich Homann und Andrew Townhill. Die FailSafe-Videoreihe Webseite-Version.
- [Microsoft Patterns and Practices - Leitfaden zur Azure](https://msdn.microsoft.com/library/dn568099.aspx). Finden Sie unter "Wiederholen"-Muster, die Scheduler-Agent-Supervisor-Muster.
- [Fehlertoleranz in Azure SQL-Datenbank](https://blogs.msdn.com/b/windowsazure/archive/2012/07/30/fault-tolerance-in-windows-azure-sql-database.aspx). Der Blogbeitrag von Tony Petrossian.
- [Entitätsframework – Verbindungsstabilität / Wiederholungslogik](https://msdn.microsoft.com/data/dn456835). So verwenden, und passen die Funktion von Entity Framework 6 behandeln vorübergehenden Fehler.
- [Verbindungsresilienz und Abfangen von Befehlen mit Entitätsframework in einer ASP.NET MVC-Anwendung](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md). Viertens veranschaulicht eines Tutorials neun Teilen, die EF 6 Verbindung Stabilität-Funktion für SQL-Datenbank eingerichtet.

Videos

- [FailSafe: Erstellen von skalierbaren, robusten Clouddiensten](https://channel9.msdn.com/Series/FailSafe). Teil 9-Reihe von Marc Mercuri, Ulrich Homann und Mark Simms. Bietet allgemeine Konzepte und architektonischen Prinzipien auf eine Weise sehr zugegriffen werden kann und interessante Geschichten, die von Microsoft Customer Advisory Teams (CAT) die Erfahrung für tatsächliche Kunden gezeichnet werden. Finden Sie unter der Erläuterung von schutzschaltern in der Folge 3 40:55 ab.
- [Erstellen von Big: Erfahrungen aus dem Azure-Kunden – Teil II](https://channel9.msdn.com/Events/Build/2012/3-030). Spricht Mark Simms, zum Entwerfen für vorübergehende Fehler, Fehler verarbeiten und alles instrumentieren.

Codebeispiel

- [Clouddienstgrundlagen in Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Die beispielanwendung erstellt haben, durch die Microsoft Azure Customer Advisory Teams, die zeigt, wie Sie mit der [Enterprise Library Transient Fault Handling Block](http://nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/) (TFH). Weitere Informationen finden Sie unter [Cloud-Dienst-Grundlagen Datenzugriffsebene-Handhabung vorübergehender Fehler](https://social.technet.microsoft.com/wiki/contents/articles/18665.cloud-service-fundamentals-data-access-layer-transient-fault-handling.aspx). TFH empfiehlt sich für den Datenbankzugriff mithilfe von ADO.NET direkt (ohne Verwendung von Entity Framework).

> [!div class="step-by-step"]
> [Zurück](monitoring-and-telemetry.md)
> [Weiter](distributed-caching.md)
