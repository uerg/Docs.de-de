---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Web Development Best Practices (Building Real-World Cloud-Apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
ms.technology: ''
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: f31798032c3b690fcb6dfa8326580255f3412826
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371037"
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Bewährte Methoden der Webentwicklung (erstellen realer Cloud-Apps mit Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download korrigieren Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps mit Azure** e-Book basiert darauf, dass eine Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Weitere Informationen zu e-Book, finden Sie unter [im ersten Kapitel](introduction.md).


Die ersten drei Muster wurden über das Einrichten von ein Prozess beweglicher Entwicklung; die übrigen sind über Architektur und Code. Dies ist eine Auflistung von bewährte Methoden der Webentwicklung:

- [Statuslose Webserver](#stateless) hinter einem intelligenten Load Balancer.
- [Vermeiden Sie Sitzungszustand](#sessionstate) (oder wenn Sie es nicht vermeiden lassen, verwenden verteilte Caches anstelle einer Datenbank).
- [Verwenden Sie ein CDN](#cdn) auf Edge-Cache statischen Dateiressourcen (Bilder, Skripts).
- [Verwenden Sie .NET 4.5 Async-Unterstützung](#async) Aufrufe eine Blockierung zu vermeiden.

Diese Methoden gelten für alle Webentwicklung, nicht nur für Cloud-apps, aber sie sind besonders wichtig für Cloud-apps. Sie arbeiten zusammen, damit Sie optimal nutzen die äußerst flexible Skalierung von der Cloud-Umgebung angeboten werden können. Wenn Sie diese Methoden nicht einhalten, führen Sie dort auf Grenzen, wenn Sie versuchen, Ihre Anwendung skalieren.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Zustandsloser Web-Ebene hinter einem intelligenten Load balancer

*Zustandslose Web-Ebene* bedeutet, dass Sie keine Anwendungsdaten in der Web-Server-Arbeitsspeicher oder Datei-System speichern. Beibehalten der Webebene zustandslosen, können Sie sowohl ein besseres Benutzererlebnis zu bieten und Geld zu sparen:

- Wenn die Internetschicht zustandslos ist, und er sich hinter einem Load Balancer befindet, können Sie schnell auf Änderungen in Anwendungsdatenverkehr reagieren, indem dynamisch hinzufügen oder Entfernen von Servern. In der Cloudumgebung, in denen für Serverressourcen nur bezahlen so lange Sie tatsächlich nutzen, kann diese Fähigkeit, auf Änderungen der Nachfrage reagieren in enorme Einsparung an Zeit übersetzt.
- Eine zustandslose Web-Ebene ist architektonisch viel einfacher, um die Anwendung horizontal hochzuskalieren. Zu können, die Sie reagieren auf Anforderungen schneller zu skalieren und weniger Geld auf Entwicklungs- und Testvorgänge.
- Cloudservern, wie auf lokalen Servern, gepatcht und neu gestartet, gelegentlich werden müssen; und wenn die Internetschicht zustandslos ist, Umleiten von Datenverkehr bei Ausfall eines Servers vorübergehend führen nicht zu Fehlern oder unerwartetem Verhalten.

Die meisten realen Anwendungen müssen Zustand für eine websitzung zu speichern. Der wichtigste Punkt hierbei ist, es zu speichern, auf dem Webserver. Sie können Status auf andere Weise, wie z. B. auf dem Client, die Cookies für oder gegen serverseitigen Prozess im ASP.NET-Sitzungszustand verwenden einen Cache speichern. Sie können Dateien in speichern [Windows Azure-Blob-Speicher](unstructured-blob-storage.md) statt im lokalen Dateisystem.

Als ein Beispiel dafür, wie einfach es ist eine Anwendung in Windows Azure-Websites skalieren, wenn Ihre Webebene zustandslos ist, finden Sie unter den **Skalierung** Registerkarte für eine Windows Azure-Website im Verwaltungsportal:

![Registerkarte "Skalieren"](web-development-best-practices/_static/image1.png)

Wenn Sie die Webserver hinzufügen möchten, können Sie nur den Schieberegler für die Anzahl der auf der rechten Seite ziehen. Legen Sie ihn auf 5, und klicken Sie auf **speichern**, und innerhalb von Sekunden 5 Webserver in Windows Azure, die Ihre Website-Datenverkehr verarbeiten.

![Fünf Instanzen](web-development-best-practices/_static/image2.png)

Sie können auch einfach die Anzahl der Instanzen auf 3 oder wieder auf 1 festlegen. Wenn Sie Back skalieren, sparen Sie sofort, da Windows Azure pro Minute, nicht nach Stunden abgerechnet.

Sie können auch mitteilen, dass Windows Azure automatisch zu erhöhen oder verringern Sie die Anzahl von Webservern, die basierend auf CPU-Auslastung. Im folgenden Beispiel wenn CPU-Auslastung weniger als 60 % überschreitet, wird die Anzahl der Webserver auf ein Minimum von 2 verringern, und wenn CPU-Auslastung über 80 % überschreitet, wird die Anzahl von Webservern bis maximal 4 erhöht werden.

![Skalieren nach CPU-Auslastung](web-development-best-practices/_static/image3.png)

Oder was geschieht, wenn Sie wissen, dass Ihre Website während der Arbeitszeiten nur ausgelastet wird? Sie können feststellen, dass Windows Azure auf mehreren Servern ausführen, das tagsüber und einem einzelnen Server abends, Nächte und Wochenenden verringern. Die folgende Reihe von Screenshots veranschaulicht, wie Sie die Website festgelegt werden soll, ein Server außerhalb der Geschäftszeiten und 4 Server während der Arbeitszeiten aus 8: 00 Uhr, 17: 00 Uhr ausgeführt.

![Skalieren nach Zeitplan](web-development-best-practices/_static/image4.png)

![Zeiten für Zeitplan festlegen](web-development-best-practices/_static/image5.png)

![Unentgeltlich Zeitplan](web-development-best-practices/_static/image6.png)

![Weeknight Zeitplan](web-development-best-practices/_static/image7.png)

![Wochenende Zeitplan](web-development-best-practices/_static/image8.png)

Und natürlich können all dies in Skripts auch, wie im Portal ausgeführt werden.

Die Fähigkeit Ihrer Anwendung zu skalieren ist nahezu unbegrenzte in Windows Azure, solange Sie vermeiden, dass beeinträchtigungen des Diensts dynamisch hinzufügen oder Entfernen von Server-VMs die Webebene zustandslos zu bleiben.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Vermeiden des Sitzungszustands

Es ist häufig nicht sinnvoll, in einer echten Cloud-app, um zu vermeiden, das Speichern von Zustandsinformationen für eine benutzersitzung allerdings beeinträchtigen einige Herangehensweisen die Leistung und Skalierbarkeit stärker als andere. Wenn Sie Zustand speichern müssen, ist die beste Lösung, halten Sie die Menge an Statusinformationen klein und in Cookies zu speichern. Wenn dies nicht möglich ist, die nächstbeste Lösung ist die Verwendung von ASP.NET-Sitzungszustand mit einem Anbieter für [verteilter in-Memory-Cache](distributed-caching.md#sessionstate). Die schlechteste Lösung Hinblick auf Leistung und Skalierbarkeit Standpunkt aus betrachtet ist eine Datenbank-Sitzungszustandsanbieter gesichert.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Verwenden Sie ein CDN zum Zwischenspeichern von statischen Dateiressourcen

CDN ist ein Akronym für Content Delivery Network. Sie statischen Dateiressourcen wie Bilder und Skriptdateien zu einem CDN-Anbieter geben an, und der Anbieter speichert diese Dateien in Rechenzentren auf der ganzen Welt, damit immer Personen Ihre Anwendung zugreifen, sie relativ schnellen Reaktionen und niedriger Latenz für den zwischengespeicherten abrufen Ressourcen. Dies beschleunigt die Gesamtzeit der Auslastung des Standorts und reduziert die Last auf Ihrem Webserver. CDNs sind besonders wichtig, wenn Sie eine Zielgruppe erreichen, die geografisch weit verteilt sind.

Windows Azure verfügt über ein CDN, und können andere CDNs in einer Anwendung, die in Windows Azure oder bei allen Web-hostumgebung ausgeführt wird.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Verwenden Sie .NET 4.5 Async-Unterstützung, um zu vermeiden, Aufrufe zum Blockieren

.NET 4.5 erweitert die Programmiersprachen c# und VB, damit darauf viel einfacher, die Aufgaben asynchron zu verarbeiten. Der Vorteil der asynchronen Programmierung ist nicht nur für die parallelverarbeitung Situationen, z. B. Wenn Sie mehrere Aufrufe des Webdiensts gleichzeitig starten möchten. Er ermöglicht auch Ihrem Webserver das effizienter ausführen und zuverlässige unter hoher Last. Ein Webserver ist nur eine begrenzte Anzahl von Threads zur Verfügung stehen, und unter hoher Last, wenn alle Threads verwendet werden, werden eingehende Anforderungen müssen warten, bis Threads wieder freigegeben werden. Wenn Ihr Anwendungscode nicht Aufgaben wie Datenbank-Abfragen und Aufrufe des Webdiensts asynchron behandelt werden, sind viele Threads unnötigerweise um gebunden, während der Server für eine e/a-Antwort wartet. Dies schränkt die Menge des Datenverkehrs, die der Server unter hoher Last verarbeiten kann. Mit der asynchronen Programmierung, Threads, die einen Webdienst oder einer Datenbank zur Rückgabe von Daten warten, bis neue Anforderungen zu Daten freigegeben werden die empfangen wird. Auf einem ausgelasteten Webserver können Hunderte oder Tausende von Anfragen klicken Sie dann sofort verarbeitet, werden die Threads freigegeben werden, andernfalls warten würden.

Wie Sie gesehen haben, ist es einfach, verringern Sie die Anzahl von Webservern, die Ihre Website behandeln, wie sie erhöhen. Also, wenn ein Server mit höheren Durchsatz erreichen kann, müssen Sie nicht, wie viele und auch die Kosten verringert werden können, da Sie weniger Server für einen bestimmten Datenverkehrsvolumen benötigen, als würde Sie sonst.

Unterstützung für das asynchrone Programmiermodell von .NET 4.5 ist für Web Forms, MVC und Web-API in ASP.NET 4.5 enthalten; im Entitätsframework 6, und klicken Sie in der [Windows Azure Storage-API](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Async-Unterstützung in ASP.NET 4.5

ASP.NET 4.5 haben direkte Unterstützung für asynchroner Programmierung nicht nur für die Sprache, sondern auch für die Frameworks, MVC, Web Forms und Web-API hinzugefügt wurde. Beispielsweise wird eine ASP.NET MVC-Controller-Aktionsmethode empfängt Daten von einer webanforderung und übergibt die Daten an eine Ansicht, die wiederum von der HTML-Code an den Browser gesendet werden. Häufig muss die Action-Methode zum Abrufen von Daten aus einer Datenbank oder Web-Dienst um es auf einer Webseite anzuzeigen oder um auf einer Webseite eingegebenen Daten zu speichern. In diesen Fällen ist es einfach, die die Aktionsmethode asynchron auszuführen: anstatt ein *ActionResult* -Objekts können Sie zurückkehren *Aufgabe&lt;ActionResult&gt;*  und markieren Sie die Methode mit der *Async* Schlüsselwort. Innerhalb der Methode Wenn eine einzige Zeile Code aus einem Vorgang startet, bei der Wartezeit, kennzeichnen Sie ihn mit dem Schlüsselwort "await".

Hier ist eine einfache Aktion-Methode, die eine Repository-Methode für eine Datenbankabfrage aufruft:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

Und hier ist die gleiche Methode, die der Datenbankaufruf asynchron behandelt:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

Hinter den Kulissen generiert der Compiler den entsprechenden asynchronen Code. Wenn die Anwendung führt den Aufruf für `FindTaskByIdAsync`, stellt ASP.NET die `FindTask` anfordern und dann den Thread entladen und macht sie verfügbar ist, um eine weitere Anforderung zu verarbeiten. Wenn die `FindTask` Anforderung erfolgt, ein Thread neu gestartet wird, um die Verarbeitung des Codes, der nach diesem Aufruf steht fortzusetzen. In der Zwischenzeit zwischen der `FindTask` Anforderung initiiert, und wenn die Daten zurückgegeben werden, müssen Sie einen Thread sinnvolle Arbeit zu tun, die andernfalls die Antwort warten gebunden werden sollen.

Entsteht ein gewisser Mehraufwand für asynchronen Code allerdings unter Umständen mit geringer Auslastung dieser Aufwand sind zu vernachlässigen, während unter hohen auslastungen Bedingungen, die Sie zum Verarbeiten von Anforderungen, die andernfalls aufrechterhalten werden würde, um verfügbare Threads warten können.

Es war möglich, diese Art der asynchronen Programmierung, da ASP.NET 1.1, aber es war schwierig zu schreiben, fehleranfällig und schwierig zu debuggen. Nun, da wir den Code dafür in ASP.NET 4.5 vereinfacht haben, besteht kein Grund, nicht mehr auf.

### <a name="async-support-in-entity-framework-6"></a>Async-Unterstützung in Entity Framework 6

Lieferumfang als Teil der Async-Unterstützung in 4.5 Async-Unterstützung für Aufrufe des Webdiensts, Sockets und Dateisystem-e/a, aber die am häufigsten verwendete Muster für Webanwendungen ist eine Datenbank erreicht wird, und unsere Datenverbindungsbibliotheken nicht Async unterstützen. Nun fügt Entity Framework 6 Async-Unterstützung für den Datenbankzugriff.

In Entity Framework 6 haben alle Methoden, die dazu führen, dass eine Abfrage oder einen Befehl aus, um auf die Datenbank gesendet werden die asynchronen Versionen. In diesem Beispiel wird gezeigt, die asynchrone Version von der *finden* Methode.

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Und dieser Async-Unterstützung funktioniert nicht nur für einfügungen, löschungen, Updates und einfach gefunden, es funktioniert auch mit LINQ-Abfragen:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Es gibt ein `Async` Version von der `ToList` Methode da in diesem Code, der die Methode, die bewirkt, dass eine Abfrage an die Datenbank gesendet werden. Die `Where` und `OrderByDescending` Methoden nur konfigurieren, die Abfrage, während die `ToListAsync` Methode führt die Abfrage, und speichert die Antwort in die `result` Variable.

## <a name="summary"></a>Zusammenfassung

Sie können die bewährten Methoden der Webentwicklung alle hier beschriebenen allen Web-Programmierung, Framework und alle Cloud-Umgebung implementieren, aber wir haben die Tools in ASP.NET und Windows Azure zu erleichtern. Wenn Sie diese Muster folgen, können Sie problemlos horizontal hochskalieren der Webebene, und Sie müssen Ihre Ausgaben minimieren, da jeder Server mehr Datenverkehr verarbeiten kann.

Die [im nächsten Kapitel](single-sign-on.md) untersucht, wie die Cloud für Szenarien mit einmaligem Anmelden kann.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie in den folgenden Ressourcen.

Zustandsloser Web-Server:

- [Microsoft Patterns und Practices - Anleitungen zum automatischen Skalieren](https://msdn.microsoft.com/library/dn589774.aspx).
- [Deaktivieren des ARR-Affinität in Windows Azure-Websites Instanz](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Blogbeitrag von Erez Benari, erläutert die sitzungsaffinität in Windows Azure-Websites.

CDN:

- [FailSafe: Erstellen von skalierbaren, robusten Clouddiensten](https://channel9.msdn.com/Series/FailSafe). Videoreihe neun-Teil von Marc Mercuri, Ulrich Homann und Mark Simms. Finden Sie unter den CDN-Ausführungen im Episode 3 1:34:00 Uhr ab.
- [Muster Microsoft Patterns and Practices Hosten von statischen Inhalten](https://msdn.microsoft.com/library/dn589776.aspx)
- [CDN-Überprüfungen](http://www.cdnreviews.com/). Übersicht über viele CDNs.

Asynchrone Programmierung:

- [Verwenden asynchroner Methoden in ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). In diesem Tutorial von Rick Anderson.
- [Asynchrone Programmierung mit Async und Await (C#- und Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). MSDN-Whitepaper, aus der hervorgeht, Gründe für die asynchrone Programmierung, deren Funktionsweise in ASP.NET 4.5 und Schreiben von Code, um es zu implementieren.
- [Entity Framework-Async-Abfrage, und speichern](https://msdn.microsoft.com/data/jj819165)
- [Gewusst wie: Erstellen von ASP.NET Webanwendungen mit Async](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Eine Videopräsentation von Rowan Miller. Unter anderem Grafik veranschaulicht, wie asynchrone Programmierung deutliche erhöhen, Durchsatz des Webservers unter hoher Last erleichtern kann.
- [FailSafe: Erstellen von skalierbaren, robusten Clouddiensten](https://channel9.msdn.com/Series/FailSafe). Videoreihe neun-Teil von Marc Mercuri, Ulrich Homann und Mark Simms. Diskussionen über die Auswirkungen der asynchronen Programmierung auf Skalierbarkeit finden Sie in der Folge 4 "und" Episode 8.
- [Die Magie der Verwendung von asynchronen Methoden in ASP.NET 4.5 sowie ein wichtiger Aspekt](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Blogbeitrag von Scott Hanselman, in erster Linie zum Verwenden von Async in ASP.NET Web Forms-Anwendungen.

Zusätzliche Web Development best Practices für die finden Sie unter den folgenden Ressourcen:

- [Der Fix It-Beispielanwendung – bewährte Methoden](the-fix-it-sample-application.md#bestpractices). Im Anhang dieses e-Book führt eine Reihe von best Practices, die in der Fix It-Anwendung implementiert wurden.
- [Checkliste für die Web-Entwickler](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Zurück](continuous-integration-and-continuous-delivery.md)
> [Weiter](single-sign-on.md)
