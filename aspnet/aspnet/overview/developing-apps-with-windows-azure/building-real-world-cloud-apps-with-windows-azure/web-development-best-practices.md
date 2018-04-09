---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
title: Web Development Best Practices (Building Real-World Cloud Apps with Azure) | Microsoft Docs
author: MikeWasson
description: Die Building Real World Cloud Apps with Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: 52d6c941-2cd9-442f-9872-2c798d6d90cd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices
msc.type: authoredcontent
ms.openlocfilehash: 4c43b256018d91e89b3427f90fc5c6cd018641f9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="web-development-best-practices-building-real-world-cloud-apps-with-azure"></a>Web Development Best Practices (Building Real-World Cloud Apps with Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download Behebungsskript Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps with Azure** e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die Ihnen helfen können erfolgreich ausgeführt entwickeln Web-apps für die Cloud. Weitere Informationen zu e-Book herunterladen, finden Sie unter [im Kapitel über das erste](introduction.md).


Die ersten drei Muster wurden Informationen zum Einrichten von dynamischen Entwicklungsprozessen; die übrigen sind Informationen zu Architektur und Code. Dieses Objekt ist eine Auflistung von Web Development best Practices:

- [Statuslose Webserver](#stateless) hinter einem intelligenten Lastenausgleich.
- [Vermeiden Sie Sitzungsstatus](#sessionstate) (oder wenn es nicht vermeiden können, verwenden verteilte Caches anstelle einer Datenbank).
- [Verwenden ein CDNS](#cdn) auf Edge-Cache statische Dateien Ressourcen (Bilder, Skripts).
- [Verwenden des .NET 4.5 asynchrone Unterstützung](#async) um blockierende Aufrufe zu vermeiden.

Diese Methoden gelten für alle Webentwicklung nicht nur für Cloud-apps, aber sie sind besonders wichtig für Cloud-apps. Sie arbeiten zusammen, um die optimale Nutzung der angebotenen Cloud-Umgebung mit sehr flexiblen Skalierung treffen zu können. Wenn Sie diese Methoden nicht folgen, führen Sie Einschränkungen auftreten, wenn Sie versuchen, Ihre Anwendung zu erweitern.

<a id="stateless"></a>
## <a name="stateless-web-tier-behind-a-smart-load-balancer"></a>Zustandslose Webebene hinter einem intelligenten Lastenausgleich

*Zustandslose Webebene* bedeutet, dass Sie keine Anwendungsdaten in der Web-Serversystem an Arbeitsspeicher oder Datei speichern. Halten die Webebene zustandslose, können Sie sowohl eine bessere benutzerfreundlichkeit bereitstellen und dabei Geld sparen:

- Wenn die Webebene zustandslos ist, und es befindet sich hinter einem Lastenausgleich, können Sie schnell auf Änderungen in der Anwendungsdatenverkehr reagieren, indem dynamisch hinzufügen oder Entfernen von Servern. In der Cloudumgebung, in denen Zahlen Sie nur für Server-Ressourcen für solange Sie tatsächlich verwenden, kann diese Funktion für die Reaktion auf Änderungen in der Anforderung in große Einsparung übersetzen.
- Eine zustandslose Webebene ist architektonisch viel einfacher zur horizontalen Skalierung der Anwendung. Zu können, die Sie reagieren auf Anforderungen schneller Skalierung und verbringen weniger Geld entwickeln und Testen des Prozesses.
- Cloud-Servern, z. B. auf lokalen Servern, gepatcht und gelegentlich gestartet werden müssen; und wenn die Webebene zustandslos ist, Datenverkehr umleiten, wenn der Ausfall eines Servers vorübergehend führen nicht zu Fehler oder unerwartetes Verhalten.

Die meisten echten Anwendungen müssen Zustand für eine Web-Sitzung zu speichern. im Mittelpunkt ist, nicht auf dem Webserver zu speichern. Sie können Status auf andere Weise, wie z. B. auf dem Client, die Cookies für oder gegen serverseitigen Prozess im ASP.NET-Sitzungszustand mithilfe eines Anbieters Cache speichern. Sie können Dateien in speichern [Windows Azure-Blob-Speicher](unstructured-blob-storage.md) statt im lokalen Dateisystem.

Als ein Beispiel dafür, wie einfach es zum Skalieren einer Anwendung in Windows Azure-Websites ist, wenn die Webebene zustandslos ist, finden Sie unter der **Skalierung** Registerkarte für eine Windows Azure-Website im Verwaltungsportal:

![Registerkarte "Skalieren"](web-development-best-practices/_static/image1.png)

Wenn Sie den Webserver hinzufügen möchten, können Sie nur die Instanz Anzahl Schieberegler rechts ziehen. Legen Sie es auf 5, und klicken Sie auf **speichern**, und innerhalb von Sekunden haben Sie 5 Webserver in Windows Azure, die Ihre Website-Datenverkehr behandeln.

![Fünf Instanzen](web-development-best-practices/_static/image2.png)

Sie können auch einfach die Anzahl der Instanzen auf 3 oder wieder auf 1 festlegen. Wenn Sie Back skaliert werden sollen, Sie beginnen damit Einsparung sofort verwendet werden, da Windows Azure durch die Minute, nicht pro Stunde abgerechnet.

Sie können auch Microsoft Azure können Sie automatisch zu erhöhen oder Verringern der Anzahl von Webservern, die basierend auf der CPU-Auslastung feststellen. Im folgenden Beispiel wenn CPU-Auslastung unter 60 % fällt, senken die Anzahl der Webserver auf einem Minimum von 2, und wenn 80 % CPU-Auslastung überschreitet, wird die Anzahl der Webserver erhöht werden, bis zu einem Maximum von 4.

![Skalierung nach CPU-Auslastung](web-development-best-practices/_static/image3.png)

Oder was geschieht, wenn Sie wissen, dass Ihr Standort während der Arbeitszeit nur ausgelastet ist? Sie können feststellen, dass Microsoft Azure können Sie mehrere Server tagsüber ausführen und auf einem einzelnen Server abends, Nächte und Wochenenden verringern. Die folgende Reihe von Screenshots veranschaulicht, wie die Website einzurichten, ein Server im außerhalb der normalen Arbeitszeiten und 4 Server während der Arbeitszeiten aus 8: 00 Uhr bis 17 Uhr ausgeführt.

![Skalieren, indem Sie Zeitplan](web-development-best-practices/_static/image4.png)

![Zeiten für Zeitplan festlegen](web-development-best-practices/_static/image5.png)

![Tagsüber Zeitplan](web-development-best-practices/_static/image6.png)

![Weeknight Zeitplan](web-development-best-practices/_static/image7.png)

![Wochenende Zeitplan](web-development-best-practices/_static/image8.png)

Und natürlich können all dies in Skripts auch wie im Portal ausgeführt werden.

Die Möglichkeit der Anwendung zu skalieren ist nahezu unbegrenzte in Windows Azure, solange Sie vermeiden, dass die Diagnose-impedimente dynamisch hinzufügen oder Entfernen von Server-VMs, die Webebene zustandslos zu bleiben.

<a id="sessionstate"></a>
## <a name="avoid-session-state"></a>Vermeiden Sie Sitzungsstatus

Es ist häufig nicht geeignet, in einer realen Cloud-app, um zu vermeiden, speichern eine Form des Zustands für eine benutzersitzung, aber einige Ansätze Auswirkungen auf Leistung und Skalierbarkeit mehr Informationen als andere. Wenn Sie Zustand speichern müssen, ist die beste Lösung halten die Menge des Zustands klein und in Cookies zu speichern. Wenn, die nicht möglich ist, wird die nächstbeste Lösung für die Verwendung von ASP.NET-Sitzungsstatus mit einem Anbieter für [verteilter in-Memory-Cache](distributed-caching.md#sessionstate). Die schlechteste Lösung unter eine Leistung und Skalierbarkeit Standpunkt ist für die Verwendung einer Datenbank sitzungstatusanbieter.

<a id="cdn"></a>
## <a name="use-a-cdn-to-cache-static-file-assets"></a>Verwenden von CDN zum Zwischenspeichern von statischen Datei Bestand

CDN ist ein Akronym für Content Delivery Network. Sie Bereitstellen statischer Dateien Ressourcen wie z. B. Bilder und Skriptdateien zu einem CDN-Anbieter und der Anbieter diese Dateien in Rechenzentren auf der ganzen Welt zwischenspeichert, sodass immer Personen Ihre Anwendung zugreifen, sind relativ schnelle Antworten und geringe Latenz für die zwischengespeicherten finden Bestand. Dies beschleunigt die gesamte Ladezeit des Standorts und verringert die Last auf Ihre Webserver. CDNs sind besonders wichtig, wenn Sie eine Zielgruppe erreichen, die geografisch weit verteilt werden.

Windows Azure verfügt über CDN und können Sie andere CDNs in einer Anwendung, die in Windows Azure oder allen Web-hosting-Umgebung ausgeführt wird.

<a id="async"></a>
## <a name="use-net-45s-async-support-to-avoid-blocking-calls"></a>Verwenden des .NET 4.5 asynchrone Unterstützung um blockierende Aufrufe zu vermeiden

.NET 4.5 verbessert die Programmiersprachen c# und VB, um es viel einfacher, die Aufgaben asynchron verarbeiten vornehmen zu können. Der Vorteil der asynchronen Programmierung ist nicht nur für die parallele Verarbeitungssituationen, z. B., wenn Sie mehrere Aufrufe des Webdiensts gleichzeitig starten möchten. Dadurch auch der Webserver effizienter ausführen und zuverlässige hoher Last. Ein Webserver ist nur eine begrenzte Anzahl von Threads zur Verfügung stehen, und hoher Last bei der alle Threads verwendet, werden eingehende Anforderungen haben, warten, bis die Threads freigegeben werden. Wenn der Anwendungscode nicht Aufgaben, wie Datenbank-Abfragen und Webdienstaufrufe asynchron behandelt werden, sind viele Threads unnötigerweise oben gebunden, während der Server für eine e/a-Antwort wartet. Dies schränkt den Umfang des Datenverkehrs, der Server unter hoher Last verarbeiten kann. Asynchrone Programmierung mit Threads, die auf einem Webdienst oder einer Datenbank zum Zurückgeben von Daten warten sind reserviert, bis zu neue Anforderungen bis die Daten der empfangen wird. In einem ausgelasteten Webserver können Hunderte oder Tausende Anforderungen klicken Sie dann umgehend verarbeitet werden die Threads freigegeben werden, andernfalls warten möchten.

Wie weiter oben gezeigt, ist es ebenso einfach zu verringern der Anzahl der Webserver Ihre Website behandeln, wie es sie erhöhen. Also wenn ein Server höheren Durchsatz erzielen kann, müssen Sie nicht, wie viele dieser Optionen und Sie Ihre Kosten verringern können, da Sie weniger Server für einen bestimmten Aufkommen, als Sie andernfalls würde benötigen.

Unterstützung für das asynchrone Programmiermodell in .NET 4.5 ist in ASP.NET 4.5 für Web Forms, MVC und Web-API enthalten; im Entity Framework 6, und klicken Sie in der [Windows Azure-Speicher-API](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/07/12/introducing-storage-client-library-2-1-rc-for-net-and-windows-phone-8.aspx).

### <a name="async-support-in-aspnet-45"></a>Asynchrone Unterstützung in ASP.NET 4.5

In ASP.NET 4.5 eine Unterstützung für asynchrone Programmierung nicht nur für die Sprache, sondern auch für die MVC, Web Forms und Web-API-Frameworks hinzugefügt wurde. Z. B. eine ASP.NET MVC-Controller-Aktionsmethode empfängt Daten von einer webanforderung und übergibt die Daten an eine Ansicht, die klicken Sie dann den HTML-Code an den Browser gesendet wird erstellt. Häufig muss die Aktionsmethode zum Abrufen von Daten aus einer Datenbank oder Web-Dienst um es auf einer Webseite anzuzeigen oder um auf einer Webseite eingegebenen Daten zu speichern. In jenen Szenarien ist es ganz einfach die Aktionsmethode asynchron: zurückgegeben, sondern ein *ActionResult* -Objekts können Sie zurückkehren *Aufgabe&lt;ActionResult&gt;*  und markieren Sie die Methode mit der *Async* Schlüsselwort. Innerhalb der Methode Wenn eine Codezeile deaktiviert einen Vorgang gestartet, bei der Wartezeit, markieren Sie Sie es mit dem Await-Schlüsselwort.

Hier ist eine einfache Aktion-Methode, die eine Repository-Methode für eine Datenbankabfrage aufruft:

[!code-csharp[Main](web-development-best-practices/samples/sample1.cs)]

Und hier ist die gleiche Methode, die asynchron Datenbankaufruf behandelt:

[!code-csharp[Main](web-development-best-practices/samples/sample2.cs?highlight=1,4)]

Im Hintergrund generiert der Compiler den entsprechenden asynchronen Code. Wenn die Anwendung führt den Aufruf für `FindTaskByIdAsync`, macht ASP.NET den `FindTask` anfordern und entlädt den Arbeitsthread, und sie für eine andere Anforderung verarbeiten verfügbar macht. Wenn die `FindTask` Anforderung erfolgt, ein Thread neu gestartet wird, um die Verarbeitung des Codes, der nach diesem Aufruf verwendete fortzusetzen. In der Zwischenzeit zwischen der `FindTask` Anforderung initiiert, und wenn die Daten zurückgegeben werden, müssen Sie einen Thread sinnvolle Arbeit zu tun, die andernfalls gebunden werden würde, die Antwort warten.

Ist etwas Aufwand für den asynchronen Code, aber unter Umständen mit geringer Auslastung Aufwand sind zu vernachlässigen, beim unter hoher Belastung, die zur Verarbeitung von Anforderungen, die andernfalls verfügbaren Threads warten gehalten werden würde, können Sie Ihre Kosten.

Es ist möglich, diese Art der asynchronen Programmierung seit ASP.NET 1.1 hat, aber es war schwierig zu schreiben, fehleranfällig und schwierig zu debuggen. Nun, da wir die Codierung für sie in ASP.NET 4.5 vereinfacht haben, besteht kein Grund ist es nicht mehr.

### <a name="async-support-in-entity-framework-6"></a>Asynchrone Unterstützung in Entity Framework 6

Im Rahmen der asynchrone Unterstützung in 4.5 ausgeliefert wir asynchrone Unterstützung für Aufrufe des Webdiensts, Sockets und Dateisystem-e/a, jedoch ist das am häufigsten verwendete Muster für Webanwendungen ist eine Datenbank erreicht und unsere Datenverbindungsbibliotheken hat unterstützt asynchrone. Entity Framework 6 fügt nun asynchrone Unterstützung für den Datenbankzugriff.

In Entity Framework 6 haben alle Methoden, die dazu führen, dass eine Abfrage oder einen Befehl aus, um an die Datenbank gesendet werden asynchrone Versionen. In diesem Beispiel wird gezeigt, die asynchrone Version der *suchen* Methode.

[!code-csharp[Main](web-development-best-practices/samples/sample3.cs?highlight=8)]

Und diese asynchrone Unterstützung funktioniert nicht nur für einfügungen, löschungen, Updates und einfache sucht, funktioniert auch mit LINQ-Abfragen:

[!code-csharp[Main](web-development-best-practices/samples/sample4.cs?highlight=7,10)]

Es ist ein `Async` Version von der `ToList` Methode in diesem Code, der die Methode besteht darin, die bewirkt, dass eine Abfrage an die Datenbank gesendet werden. Die `Where` und `OrderByDescending` Methoden nur konfigurieren, die Abfrage während der `ToListAsync` Methode führt die Abfrage aus und speichert die Antwort in den `result` Variable.

## <a name="summary"></a>Zusammenfassung

Sie können die bewährten Methoden der Webentwicklung in allen Web-Framework und Cloud-Umgebung Programmieren hier beschriebenen implementieren, aber wir haben die Tools in ASP.NET und Windows Azure zu erleichtern. Wenn Sie diese Muster folgen, können Sie problemlos Skalieren Ihrer Web-Ebene, und müssen Sie Ihre Ausgaben minimieren, da jeder Server größere Menge an Datenverkehr verarbeiten kann.

Die [nächsten Kapitels](single-sign-on.md) untersucht, wie die Cloud für Szenarien mit einmaligem Anmelden kann.

## <a name="resources"></a>Ressourcen

Weitere Informationen finden Sie unter den folgenden Ressourcen.

Statuslose Webserver:

- [Microsoft Patterns and Practices - Leitfaden für automatische Skalierung](https://msdn.microsoft.com/library/dn589774.aspx).
- [Deaktivieren von ARR-Instanz Affinität im Windows Azure-Websites](https://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/). Blogbeitrag von Erez Benari, erläutert die sitzungsaffinität in Windows Azure-Websites.

CDN:

- [FailSafe: Erstellen von skalierbaren, robusten Cloud-Dienste](https://channel9.msdn.com/Series/FailSafe). Videoreihe Marc Mercuri, Ulrich Homann und Mark Simms neun Teil. Finden Sie in der Folge 3 starten, um 1:34:00 Uhr CDN.
- [Microsoft Patterns und Practices statischen Inhalt hosten (Muster)](https://msdn.microsoft.com/library/dn589776.aspx)
- [CDN Reviews](http://www.cdnreviews.com/). Übersicht über viele CDNs.

Asynchrone Programmierung:

- [Verwenden von asynchronen Methoden in ASP.NET MVC 4](../../../../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md). Lernprogramm von Rick Anderson.
- [Asynchrone Programmierung mit Async und Await (C#- und Visual Basic)](https://msdn.microsoft.com/library/vstudio/hh191443.aspx). MSDN-Whitepaper, aus der hervorgeht, Begründung für die asynchrone Programmierung, Funktionsweise in ASP.NET 4.5 und das Schreiben von Code, um es zu implementieren.
- [Entity Framework Async Abfrage- und speichern](https://msdn.microsoft.com/data/jj819165)
- [Gewusst wie: Verwenden von Async ASP.NET-Webanwendungen erstellen](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2013/DEV-B337#fbid=tgkT4SR_DK7). Videopräsentation von Rowan Miller. Enthält eine grafische Demonstration der Funktionsweise der asynchronen Programmierung deutlichen Anstieg der Durchsatz des Webservers hoher Last zu vereinfachen können.
- [FailSafe: Erstellen von skalierbaren, robusten Cloud-Dienste](https://channel9.msdn.com/Series/FailSafe). Videoreihe Marc Mercuri, Ulrich Homann und Mark Simms neun Teil. Diskussionen zu den Auswirkungen der asynchronen Programmierung auf Skalierbarkeit finden Sie in der Folge 4 und Episode 8.
- [Das Geheimnis der Verwendung von asynchronen Methoden in ASP.NET 4.5 plus eine wichtige Problem](http://www.hanselman.com/blog/TheMagicOfUsingAsynchronousMethodsInASPNET45PlusAnImportantGotcha.aspx). Blogbeitrag von Scott Hanselman, in erster Linie zur Verwendung von asynchronen in ASP.NET Web Forms-Anwendungen.

Zusätzliche Web Development best Practices finden Sie unter den folgenden Ressourcen:

- [Die Lösung diese Beispielanwendung - Best Practices](the-fix-it-sample-application.md#bestpractices). Anhang, um diese e-Book führt eine Reihe von best Practices, die in der Anwendung beheben implementiert wurden.
- [Prüfliste für Web-Entwickler](http://webdevchecklist.com/asp.net)

> [!div class="step-by-step"]
> [Zurück](continuous-integration-and-continuous-delivery.md)
> [Weiter](single-sign-on.md)
