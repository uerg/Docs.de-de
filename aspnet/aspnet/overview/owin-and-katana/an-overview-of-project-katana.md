---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: "Eine Übersicht über Project Katana | Microsoft Docs"
author: howarddierking
description: "Dem ASP.NET-Framework wurde um mehr als zehn Jahren, und die Plattform die Entwicklung von unzähligen Websites und Diensten aktiviert hat. Als Web-Anwendung..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/30/2013
ms.topic: article
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 8f28116f88f3cf5143d3d5c9821519d62c4e5452
ms.sourcegitcommit: 6541c8b11001dd617adf5eb04c814cda165070b9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2017
---
<a name="an-overview-of-project-katana"></a>Eine Übersicht über Project Katana
====================
durch [Howard Dierking](https://github.com/howarddierking)

> Dem ASP.NET-Framework wurde um mehr als zehn Jahren, und die Plattform die Entwicklung von unzähligen Websites und Diensten aktiviert hat. Wie Entwicklungsstrategien für Web-Anwendung entwickelt haben, wurde das Framework im Schritt mit Technologien wie ASP.NET MVC und ASP.NET Web-API ohne Unterbrechung weiterentwickeln können. Projekt wie Webanwendungsentwicklung der nächste Entwicklungsschritt in der ganzen Welt des Cloudcomputings benötigt, [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) enthält die zugrunde liegenden Komponenten für ASP.NET-Anwendungen aktivieren, flexible und portabel sein einfache, und eine bessere Leistung – anders ausgedrückt, Projekt [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) Cloud optimiert Ihrer ASP.NET-Anwendungen.


## <a name="why-katana--why-now"></a>Warum Katana – Warum jetzt?

 Unabhängig davon, ob eine ein Developer-Framework oder Endbenutzer Produkt erörtert wird, es ist wichtig zu verstehen, die zugrunde liegenden Motivation für die Erstellung enthält die Product-als auch Teil, zu wissen, wer für das Produkt erstellt wurde. ASP.NET wurde mit zwei Kunden Bedenken ursprünglich erstellt.   
  
**Die erste Gruppe von Kunden wurde klassische ASP-Entwickler.** Zum Zeitpunkt war ASP mit einer der primären Technologien für dynamische, datengesteuerte Websites und Anwendungen von interweaving Markup und serverseitige Skripts erstellen. Die ASP-Laufzeit bereitgestellt: serverseitige Skripts mit einem Satz von Objekten, die Hauptaspekte des zugrunde liegenden HTTP-Protokoll und Webserver abstrahiert und bereitgestellten Zugriff auf zusätzliche solche Zustandsverwaltung Sitzung und die Anwendung Dienste cache usw. an. Zwar leistungsstark, wurde um klassische ASP-Anwendungen schwierig zu verwalten, wie sie in der Größe und Komplexität vergrößert wurde. Dies war aufgrund der Mangel an der Struktur in skriptumgebungen gekoppelt mit der Duplizierung des Codes, die aufgrund der Überlappung der Code- und Markupdateien finden. Um die Vorteile von klassischem ASP profitieren beim Adressieren einige der Vorteile, nutzte ASP.NET der Code Organisation von der objektorientierten Sprachen von .NET Framework bereitgestellt wird, Beibehaltung auch das serverseitige-Programmiermodell auf welche klassischem ASP wuchs Entwicklern vertraut.

**Die zweite Gruppe von Zielkunden für ASP.NET wurde Windows Business-Anwendungsentwickler.** Im Gegensatz zum klassischen ASP-Entwickler, die beim Schreiben von HTML-Markup und den Code mehr HTML-Markup generieren würden, WinForms-Entwickler (z. B. die VB6 Entwickler vorgestellten) wurden mit einer entwurfszeitumgebung, das einen Zeichenbereich und einen umfangreichen Satz an Benutzer aufgenommen vertraut Benutzeroberflächen-Steuerelemente. Die erste Version von ASP.NET – bereitgestellt, auch bekannt als "Web Forms" eine ähnliche Erfahrung zur Entwurfszeit zusammen mit einem serverseitigen Ereignismodell für Benutzeroberflächenkomponenten und einen Satz von Infrastrukturfunktionen (z. B. "ViewState" Speichern) um eine nahtlose entwicklererfahrung erstellen zwischen Client und Server-Side-Programmierung. WebForms hid effektiv zustandsfrei sind im Web, unter einer statusbehafteten Ereignismodell, das WinForms-Entwicklern vertraut wurde.

### <a name="challenges-raised-by-the-historical-model"></a>Herausforderungen, die von der Verlaufsmodell ausgelöst

**Das Ergebnis war eine ausgereifte, funktionsreiche Common Language Runtime und Developer-Programmiermodell.** Mit dem stammt Feature Reichhaltigkeit jedoch einige wichtige Herausforderungen. Das Framework Erstens war **monolithischen**, logisch verschiedenartigen Zeiteinheiten Funktionalität wird in der gleichen System.Web.dll-Assembly (z. B. die Core-HTTP-Objekte mit dem Web Forms-Framework) eng gekoppelt. Zweitens, ASP.NET wurde als Teil einer größeren .NET Framework, die vorgesehen die **Abständen zwischen den Versionen wurde Größenordnung von Jahren.** Dies machte es schwierig für ASP.NET für alle Änderungen in der Webentwicklung ständig weiterentwickelt geschieht Tempo beibehalten. Schließlich wurde System.Web.dll selbst in mehrere Möglichkeiten, um eine bestimmte Webhosting-Option gekoppelt: Internetinformationsdienste (Internet Information Services, IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Entwicklungsschritte: ASP.NET MVC und ASP.NET Web-API

Und viele der Änderung in der Webentwicklung geschah! Webanwendungen wurden zunehmend entwickelt wird, wie eine Reihe von "klein", mit Fokus, Komponenten, sondern große Frameworks. Je schneller wurde die Anzahl von Komponenten als auch die Häufigkeit, mit der sie freigegeben wurden, erhöhen. Es wurde deutlich, dass durch die Beibehaltung Tempo mit dem Web Frameworks abzurufenden verkleinern, entkoppelte und genauer spezifizieren anstatt zu größeren und mehr funktionsreiche, daher erfordert die **ASP.NET-Team hat mehrere evolutionärem Schritte zum Aktivieren von ASP.NET als eine Familie von austauschbare Webkomponenten statt einem einzigen Framework**.

Eine der frühen Änderungen wurde der Anstieg der Beliebtheit der bekannten Model-View-Controller (MVC)-Entwurfsmusters Dank Web Entwicklungsframeworks wie Ruby Schienen. Diese Art der beim Erstellen von Webanwendungen hat den Entwickler größere Kontrolle über ihre Anwendungsmarkup gelernter die Trennung von Markup und Geschäftslogik, die einer der ersten verkaufen Aspekte für ASP.NET war. Um den Bedarf für diese Art der Webanwendungsentwicklung zu decken, Microsoft hat die Gelegenheit zum eigenen positionieren besser für die Zukunft von **Entwickeln von ASP.NET MVC Out of Band** (und nicht der Codekomponente in .NET Framework). ASP.NET MVC wurde als unabhängige Download freigegeben. Dies führte zu dem engineering-Team die Flexibilität zum Übermitteln von Updates zu viel häufiger als bisher möglich war.

Eine andere wichtige rückschaltungs Webanwendungsentwicklung wurde die Umstellung von dynamischen, vom Server generierte Webseiten statische anfänglichen Markup mit dynamischen Abschnitte der Skriptdatei auf Clientseite Kommunikation generiert Seite **mit Back-End-Web-APIs über AJAX-Anforderungen**. Diese architektonische geholfen gebautes der Aufschwung von Web-APIs und die Entwicklung von ASP.NET Web API-Framework. Wie im Fall von ASP.NET MVC bereitgestellt, die Version von ASP.NET Web-API eine weitere Gelegenheit ASP.NET weiter als modularer Framework entwickeln. Das engineering-Team hat den Vorteil der Verkaufschance und **ASP.NET Web API so erstellt, dass sie keine Abhängigkeiten auf den Core-Framework-Typen finden in "System.Web.dll" enthielt**. Diese Option aktiviert zwei Dinge: Erstens vorgesehen, die ASP.NET Web-API in einer vollständig unabhängigen Weise entwickeln konnte (und sie können weiterhin schnell durchlaufen werden, da es über NuGet geliefert wird). Da es keine externen Abhängigkeiten zu System.Web.dll und daher keine Abhängigkeiten in IIS waren, enthalten ASP.NET Web API Zweitens die Möglichkeit, führen Sie einen benutzerdefinierten Host (z. B. eine Konsolenanwendung, Windows-Dienst, usw.)

### <a name="the-future-a-nimble-framework"></a>Die Zukunft: Ein wendigen Framework

Durch clientframeworkkomponenten voneinander zu trennen, und veröffentlichen sie dann auf NuGet, Frameworks konnte jetzt **durchlaufen mehr unabhängig voneinander und schneller**. Darüber hinaus die Leistungsfähigkeit und Flexibilität von Web-API Selbsthosting-Funktion erwies sich als sehr attraktiv für Entwickler sollte eine **kleine, einfache Host** für ihre Dienste. So attraktiv nachgewiesen, in der Tat, die von anderen Frameworks zudem soll demonstriert werden diese Funktion und dadurch eine neue Herausforderung Diagnoseinformationen werden, dass jedes Framework in einem eigenen Hostprozess auf eine eigene Basisadresse ausgeführt und benötigt für die Verwaltung (gestartet, beendet, usw.) unabhängig voneinander. Eine moderne Webanwendung unterstützt grundsätzlich statische dateiserving, dynamische Seite Generation, Web-API und mehr vor kurzem real-time bzw. den Push-Benachrichtigungen. Erwartet, dass jeder dieser Dienste auszuführen und unabhängig verwaltet werden sollten war einfach nicht realistisch.

Erforderlich, wurde die Abstraktion eines einzelnen hosting, mit denen ein Entwickler eine Anwendung aus einer Vielzahl von verschiedenen Komponenten und Frameworks zu erstellen würde, und führen Sie die Anwendung auf einem Host unterstützenden.

## <a name="the-open-web-interface-for-net-owin"></a>Die offene Webschnittstelle für .NET (OWIN)

 Von den Vorteilen erreicht, indem Inspiriertes [Rack](http://rack.github.io/) in der Ruby-Community mehrere Mitgliedern der Community .NET dargelegt um eine Abstraktion zwischen Webserver und Framework-Komponenten zu erstellen. Zwei Ziele beim Entwurf für die OWIN-Abstraktion wurden, dass sie einfache war und die wenigsten möglichen Abhängigkeiten auf anderen Framework-Typen dauerte. Diese beiden Ziele sicherzustellen:

- Neue Komponenten konnte leichter entwickelt und genutzt werden.
- Anwendungen können einfacher zwischen Hosts und potenziell gesamte Plattformen/Betriebssysteme portiert werden.

Die resultierende Abstraktion besteht aus zwei wichtige Elemente. Das erste ist die Umgebungswörterbuch. Diese Datenstruktur ist verantwortlich für das Speichern von gesamte Zustand für die Verarbeitung einer HTTP-Anforderung und Antwort sowie alle relevanten Serverzustand erforderlich. Die Umgebungswörterbuch wird wie folgt definiert:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Ein Webserver mit OWIN kompatiblen ist verantwortlich für das Auffüllen mit Daten, z. B. den Textstreams und antwortheaderauflistungen für eine HTTP-Anforderung und Antwort-Umgebungswörterbuch. Es ist dann die Verantwortung der Anwendung oder das Framework Komponenten zum Auffüllen oder aktualisieren das Wörterbuch mit zusätzlichen Werten und Schreiben in den Antworttext-Datenstrom.

Zusätzlich zum Angeben des Typs für die Umgebungswörterbuch, definiert die OWIN-Spezifikation eine Liste von Core Wörterbuch Schlüssel-Wert-Paaren. Die folgende Tabelle zeigt beispielsweise die erforderlichen Wörterbuchschlüssel für eine HTTP-Anforderung:

| Schlüsselname | Wert Beschreibung |
| --- | --- |
| `"owin.RequestBody"` | Ein Datenstrom mit Anforderungstexts, falls vorhanden. Stream.Null kann als Platzhalter verwendet werden, wenn es keinen Anforderungstext gibt. Finden Sie unter [Anforderungstext](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | Ein `IDictionary<string, string[]>` von Anforderungsheadern. Finden Sie unter [Header](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | Ein `string` , enthält die HTTP-Anforderungsmethode der Anforderung (z. B. `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | Ein `string` , die den Pfad der Anforderung enthält. Der Pfad muss relativ zum "Root" des Delegaten Anwendung; sein. finden Sie unter [Pfade](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | Ein `string` , enthält das Teil der Anforderungspfad, "Root" des Delegaten Anwendung; entspricht finden Sie unter [Pfade](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | Ein `string` , enthält der Name des Protokolls und der Version (z. B. `"HTTP/1.0"` oder `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | Ein `string` , enthält die Zeichenfolge Abfragekomponente des HTTP-Anforderungs-URI, ohne das führende "?" (z. B. `"foo=bar&baz=quux"`). Der Wert kann eine leere Zeichenfolge sein. |
| `"owin.RequestScheme"` | Ein `string` , enthält das URI-Schema für die Anforderung verwendete (z. B. `"http"`, `"https"`); Siehe [URI-Schema](http://owin.org/html/owin.html#5-1-uri-scheme). |

Die zweite wesentliches Element der OWIN wird der Delegat für die Anwendung. Dies ist einer Funktionssignatur, die als die primäre Schnittstelle zwischen allen Komponenten in einer OWIN-Anwendung dient. Die Definition für den Delegaten Anwendung lautet wie folgt:

`Func<IDictionary<string, object>, Task>;`

Der Delegat für die Anwendung ist einfach eine Implementierung des Delegattyps Func, in dem die Funktion der Umgebungswörterbuch als Eingabe akzeptiert und gibt einen Task zurück. Dieser Entwurf hat mehrere Implikationen für Entwickler:

- Es gibt sehr wenige typabhängigkeiten erforderlich, um die owin-Komponenten zu schreiben. Dadurch erhöht sich dadurch enorm OWIN-Entwicklern den Zugriff.
- Der asynchronen Entwurf ermöglicht die Abstraktion, mit der Behandlung von Computerressourcen, insbesondere in mehrere e/a-intensive Vorgänge effizient sein.
- Da der Delegat für die Anwendung eine unteilbare Einheit der Ausführung ist und da die Umgebungswörterbuch als Parameter für den Delegaten übergeben werden, owin-Komponenten problemlos verkettet werden können zusammen, um komplexe Verarbeitungspipelines HTTP zu erstellen.

Aus Implementierungssicht, OWIN ist eine Spezifikation ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Das Ziel besteht darin nicht zu den nächsten Web-Framework, jedoch stattdessen eine Spezifikation für die Interaktion von webframeworks und Webservern einsetzbar.

Wenn Sie untersucht haben [OWIN](http://owin.org/) oder [Katana](https://github.com/aspnet/AspNetKatana/wiki), Sie möglicherweise haben Sie bemerkt der [Owin-NuGet-Paket](http://nuget.org/packages/Owin) und Owin.dll. Diese Bibliothek enthält eine einzelne Schnittstelle [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), formalisiert und wird in beschriebenen Startsequenz [Abschnitt 4](http://owin.org/html/owin.html#4-application-startup) der OWIN-Spezifikation. Obwohl nicht erforderlich, um OWIN-Servern erstellen die [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) Schnittstelle bietet einen konkrete Bezugspunkt, und er wird durch die Komponenten der Katana-Projekt verwendet.

## <a name="project-katana"></a>Projekt Katana

Während sowohl die [OWIN](http://owin.org/html/owin.html) Spezifikation und *Owin.dll* sind Community, die im Besitz und Community open-Source-Potenzial, führen die [Katana](https://github.com/aspnet/AspNetKatana/wiki) Projekt darstellt, den Satz von OWIN Komponenten, die beim weiterhin als open Source-erstellt und von Microsoft veröffentlicht werden. Zu diesen Komponenten gehören sowohl Infrastrukturkomponenten, wie z. B. Hosts und -Servern als auch funktionalen Komponenten, z. B. Authentifizierungskomponenten und Bindungen Frameworks wie z. B. [SignalR](../../../signalr/index.md) und [ASP.NET Web API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). Das Projekt enthält die folgenden drei allgemeine Ziele: 

- **Portable** – Komponenten sollten in der Lage, einfach für neue Komponenten ersetzt werden, sobald sie verfügbar sind. Dies schließt alle Typen von Komponenten, aus dem Framework auf dem Server und dem Host. Die Implikation dieses Ziel ist, dass Frameworks von Drittanbietern nahtlos auf Microsoft-Servern ausgeführt werden können, während-Frameworks von Microsoft potenziell auf Servern von Drittanbietern und Hosts ausgeführt werden können.
- **Modulare/flexible**– Katana Projektkomponenten müssen im Gegensatz zu vielen Frameworks die eine Vielzahl von Funktionen enthalten, die von standardmäßig eingeschaltet sein, klein und konzentriert sich, die Kontrolle über an den Anwendungsentwickler feststellen, welche Komponenten Verwenden Sie in ihrer Anwendung.
- **Lightweight/leistungsfähigen/skalierbare** – aufteilen den herkömmliche Begriff ein Framework in einen Satz von kleinen fokussierten Komponenten, die explizit vom Anwendungsentwickler, werden eine resultierende Katana-Anwendung beansprucht weniger berechnen Ressourcen, und daher mehr Auslastung als mit anderen Typen von Servern und Frameworks zu behandeln. Wie die Anforderungen der Anwendung weitere Funktionen aus der zugrunde liegenden Infrastruktur gefordert wird, kann die OWIN-Pipeline hinzugefügt werden, aber, die keine explizite Entscheidung seitens der Anwendungsentwickler muss. Darüber hinaus bedeutet die Substituierbarkeit von Komponenten auf niedriger Ebene, sobald sie verfügbar sind, neuen Server mit hoher Leistungsfähigkeit nahtlos eingeführt werden, können zur Verbesserung der Leistung der OWIN-Anwendungen, ohne dass diese Anwendungen.

## <a name="getting-started-with-katana-components"></a>Erste Schritte mit Katana-Komponenten

Wenn er erstmals eingeführt wurde, einen Aspekt der [Node.js](http://nodejs.org/) Framework, das sofort Aufmerksamkeit gezeichnet wurde, wurde der Einfachheit, mit denen eine erstellen und Ausführen von einem Webserver konnte. Wenn Katana-Ziele Finanzhilfe von eingebunden wurden [Node.js](http://nodejs.org/), eine möglicherweise diese besagt, dass Katana viele der Vorteile bringt zusammenfassen [Node.js](http://nodejs.org/) (und Frameworks, wie er) nur mit Erzwingen einer des Entwicklers ausgelöst werden soll Alles, was sie zum Entwickeln von ASP.NET-Webanwendungen weiß. Für diese Anweisung zum Speichern von "true", erste Schritte mit dem Katana-Projekt muss auf ebenso einfache [Node.js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Erstellen von "Hello World!"

Ein deutlicher Unterschied zwischen JavaScript und .NET Development ist das Vorhandensein (oder fehlen) eines Compilers. Daher ist der Ausgangspunkt für einen einfachen Katana-Server eine Visual Studio-Projekt. Allerdings können beginnen wir mit den am häufigsten minimal Projekttypen: leere ASP.NET-Webanwendung.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Als Nächstes wird es installiert die ["Microsoft.owin.Host.systemweb"](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) NuGet-Paket in das Projekt. Dieses Paket stellt einen OWIN-Server, der in der ASP.NET-Pipeline für die Anforderung ausgeführt wird. Es befinden sich auf die [NuGet Gallery](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) und können installiert werden, verwenden das Visual Studio-Dialogfeld "Paket-Manager" oder der Paket-Manager-Konsole mit den folgenden Befehl:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Installieren der `Microsoft.Owin.Host.SystemWeb` Paket als Abhängigkeiten einige zusätzliche Pakete installiert. Einer dieser Abhängigkeiten ist `Microsoft.Owin`, eine Bibliothek, die mehrere Hilfstypen und -Methoden für die Entwicklung von OWIN-Anwendungen bereitstellt. Wir können diese Typen verwenden, um schnell die folgenden "Hello World"-Server zu schreiben.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Diese sehr einfache Web-Server kann jetzt ausgeführt werden mithilfe von Visual Studio **F5** Befehls- und bietet vollständige Unterstützung für das Debuggen.

## <a name="switching-hosts"></a>Wechseln des hosts

Standardmäßig führt im vorherigen Beispiel für die "Hello World" in der ASP.NET-Pipeline für die Anforderung, die System.Web im Kontext von IIS verwendet. Dadurch kann allein enormen Wert hinzugefügt, wie sie uns die Flexibilität und Composability von einer OWIN-Pipeline mit den Managementfunktionen und allgemeinen Reife des IIS profitieren kann. Es gibt möglicherweise jedoch Fälle, in dem die Vorteile von IIS bereitgestellten sind nicht erforderlich, und die Absicht geäußert für einen Host kleinere, kompakter ist. Was ist, klicken Sie dann erforderlich, um unsere einfache Web-Server außerhalb von IIS und System.Web ausgeführt?

Um das Ziel Portabilität zu veranschaulichen, erfordert das Verschieben von einem Web-Server-Host auf einen Host über die Befehlszeile einfach die neuen Server und Host-Abhängigkeiten in den Projektausgabeordner hinzugefügt, und starten Sie dann den Host. In diesem Beispiel fügen wir unserer Webserver in einem Katana Host mit dem Namen hosten `OwinHost.exe` und den Katana HttpListener-basierten Server verwenden. Auf ähnliche Weise werden für die anderen Katana-Komponenten, diese von NuGet, die mithilfe des folgenden Befehls abgerufen werden:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Über die Befehlszeile können wir navigieren Sie zum Stammordner Projekts und führen Sie einfach die `OwinHost.exe` (die im Ordner "Tools" des ihre jeweiligen NuGet-Paket installiert wurde). Standardmäßig `OwinHost.exe` zum Suchen nach der HttpListener-basierten Server konfiguriert ist und daher keine zusätzliche Konfiguration erforderlich ist. Navigieren in einem Webbrowser zu `http://localhost:5000/` zeigt die Anwendung nun über die Konsole ausgeführt wird.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Katana-Architektur

 Katana-Komponentenarchitektur eine Anwendung in vier logische Schichten unterteilt, wie unten dargestellt: *Host, Server, Middleware,* und *Anwendung*. Komponentenarchitektur wird so angepasst, dass Implementierungen dieser Stufen einfach, in vielen Fällen ersetzen, ohne erneute Kompilierung der Anwendung.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Host

 Der Host ist verantwortlich für:

- Verwalten von den zugrunde liegenden Prozess aus.
- Orchestrierung des Workflows, die in der Auswahl eines Servers und der Erstellung einer OWIN-Pipeline, über welche Anforderungen verarbeitet werden.

 Zu diesem Zeitpunkt stehen 3 primäre Hostingoptionen für Katana-basierte Anwendungen zur Verfügung:  
  
**IIS/ASP.NET**: verwenden die Standardtypen HttpModule und HttpHandler, OWIN-Pipelines in IIS als Teil einer anforderungsflusses ASP.NET ausführen können. Unterstützung für das hosting ASP.NET wird durch installieren das Microsoft.AspNet.Host.SystemWeb NuGet-Paket in ein Webanwendungsprojekt aktiviert. Darüber hinaus, da IIS als einem Host und einem Server fungiert, ist die Unterscheidung der OWIN-Server-Host in enger Verbindung in NuGet-Paket, d. h., wenn der SystemWeb-Host verwendet, ein Entwickler eine Implementierung von einem alternativen Server ersetzen kann nicht.  
  
**Benutzerdefinierter Host**: das Katana-Komponentensuite ermöglicht Entwicklern die Möglichkeit zum Hosten von Anwendungen in eigenen benutzerdefinierten Prozess, ob eine Konsolenanwendung, Windows-Dienst usw. ist. Diese Funktion ähnelt der Selbsthosting-Funktion von Web-API bereitgestellt werden. Das folgende Beispiel zeigt einen benutzerdefinierten Host von Web-API-Code:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

Die Selbsthosting-Setup für eine Katana-Anwendung ähnelt:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Ein deutlicher Unterschied zwischen den Web-API und Katana Selbsthosting Beispielen ist, dass der Web-API-Konfigurationscode aus dem Katana Selbsthosting Beispiel fehlt. Um Portabilität und Composability zu aktivieren, trennt Katana den Code, der Start des Servers aus dem Code, über die Pipeline zur anforderungsverarbeitung konfiguriert. Der Code, der Web-API konfiguriert, dann befindet sich in der Klasse starten, der darüber wie der Typparameter in WebApplication.Start angegeben wird.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

Die Startklasse wird später in diesem Artikel ausführlicher besprochen. Allerdings müssen der Code eine Katana starten Selbsthosting Prozess abrupt Code ähnelt, die Sie heute in ASP.NET Web-API selbst gehostete Anwendungen möglicherweise verwenden.

**OwinHost.exe**: während einige einen benutzerdefinierten Prozess zum Ausführen von Katana-Web-Anwendungen schreiben möchten, müssen viele bevorzugen einfach eine vorgefertigte ausführbare Datei starten, die können, starten Sie einen Server, und führen Sie ihre Anwendung. In diesem Szenario die Katana-Komponente-Suite enthält die `OwinHost.exe`. Wenn im Stammverzeichnis des Projekts ausführen, wird diese ausführbare Datei Starten eines Servers (es wird der HttpListener-Server standardmäßig verwendet) und verwendet Konventionen zum Suchen und Ausführen des Benutzers Startklasse. Für eine detailliertere Kontrolle bietet die ausführbare Datei eine Anzahl zusätzlicher Befehlszeilenparameter an.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Server

 Während der Host verantwortlich ist für das Starten und Verwalten der Prozess, in dem die Anwendung ausgeführt wird, die Verantwortung des Servers, ist, öffnen einen Netzwerksocket, Lauschen auf Anforderungen und senden sie über die Pipeline der owin-Komponenten vom Benutzer (wie Sie angegeben Möglicherweise wurden bereits bemerkt, diese Pipeline in der Anwendungsentwickler Startklasse angegeben ist). Momentan zählen das Katana-Projekt zwei serverimplementierungen: 

- **"Microsoft.owin.Host.systemweb"**: wie bereits erwähnt, IIS zusammen mit der ASP.NET-Pipeline fungiert als ein Host und einem Server. Daher bei der diese Auswahl Hostingoption IIS sowohl verwaltet Hostebene bedenken, wie z. B. prozessaktivierung und Lauscht auf HTTP-Anforderungen. Für ASP.NET-Webanwendungen sendet er die Anforderungen dann in der ASP.NET-Pipeline. Der Katana SystemWeb-Host registriert eine ASP.NET HttpModule und HttpHandler, zum Abfangen von Anforderungen, wie sie über die HTTP-Pipeline übergeben und über die Benutzer angegebene OWIN-Pipeline zu senden.
- **Microsoft.Owin.Host.HttpListener**: wie der Name weist darauf hin, verwendet diese Katana-Server das .NET Framework HttpListener-Klasse, um ein Socket geöffnet und Senden von Anforderungen in einem Entwickler angegebene OWIN-Pipeline. Dies ist derzeit die Standardauswahl für die Server für das Selbsthosting Katana-API und die OwinHost.exe.

## <a name="middlewareframework"></a>Middleware/framework

 Wie bereits erwähnt ist es, wenn der Server eine Anforderung von einem Client akzeptiert verantwortlich für und übergeben sie über eine Pipeline OWIN-Komponenten, die von der Developer-Startcode angegeben werden. Diese Pipelinekomponenten werden als Middleware bezeichnet.  
 Auf einer sehr grundlegenden Ebene muss eine owin-middlewarekomponente einfach den Delegaten der OWIN-Anwendung zu implementieren, sodass es aufgerufen werden kann.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Um die Entwicklung und die Zusammensetzung der Middleware-Komponenten zu vereinfachen, unterstützt Katana jedoch eine Handvoll Konventionen und Hilfstypen für middlewarekomponenten gemeinsam sind. Die häufigsten davon ist der `OwinMiddleware` Klasse. Eine benutzerdefinierte middlewarekomponente, die mit dieser Klasse erstellten würde etwa wie folgt aussehen: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Diese Klasse leitet sich von `OwinMiddleware`, einen Konstruktor, der eine Instanz von die nächste Middleware in der Pipeline wird als eines ihrer Argumente akzeptiert, und übergibt diese an den Konstruktor der Basisklasse implementiert. Zusätzliche Argumente verwendet, um die Middleware konfigurieren, werden auch als Konstruktorparameter nach dem nächsten middlewareparameter deklariert.   
  
Zur Laufzeit die Middleware ausgeführt wird über die überschriebene `Invoke` Methode. Diese Methode akzeptiert ein einzelnes Argument vom Typ `OwinContext`. Diesem Kontextobjekt wird bereitgestellt, indem die `Microsoft.Owin` NuGet-Paket weiter oben beschriebenen und stark typisierten Zugriff auf die Anforderung, Antwort und Umgebung Wörterbuch vorhanden ist, zusammen mit einigen zusätzlichen Hilfstypen bereitstellt.   
  
Die Middleware-Klasse kann leicht wie folgt für die OWIN-Pipeline in den Anwendungsstartcode hinzugefügt werden:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Da die Katana-Infrastruktur einfach eine Pipeline der owin-Middleware Komponenten erstellt, und die Komponenten müssen einfach zur Unterstützung der Anwendung-Delegat, der in der Pipeline teilzunehmen, können middlewarekomponenten gemeinsam sind. in der Komplexität von simple liegen. Protokollierungen zum gesamten Frameworks wie ASP.NET Web-API oder [SignalR](../../../signalr/index.md). Hinzufügen den folgenden Startcode beispielsweise erfordert der vorherigen OWIN-Pipeline ASP.NET Web-API hinzugefügt:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

Katana-Infrastruktur erstellen Sie die Pipeline Middleware-Komponenten, die basierend auf der Reihenfolge, in der sie das IAppBuilder-Objekt in die Konfigurationsmethode hinzugefügt wurden. In unserem Beispiel kann dann LoggerMiddleware alle Anforderungen zu verarbeiten, die eingefügt werden, über die Pipeline, unabhängig davon, wie diese Anforderungen letztlich behandelt werden. Dies ermöglicht die leistungsstarke Szenarien, in denen eine middlewarekomponente (z. B. ein Authentifizierungskomponente) Anforderungen für eine Pipeline verarbeiten kann, die mehrere Komponenten und Frameworks (z. B. ASP.NET Web API SignalR und einen statischen Dateiserver) enthält.
 
## <a name="applications"></a>Anwendungen

Wie in den vorherigen Beispielen veranschaulicht wird, sollten OWIN und das Katana-Projekt nicht als eine neue Anwendung-Programmiermodell, sondern als eine Abstraktion, anwendungsprogrammiermodelle und Frameworks von Server- und-Hostinfrastruktur zu entkoppeln betrachtet werden. Beispielsweise wird beim Erstellen von Web-API-Anwendungen das Framework für Entwickler weiterhin mit ASP.NET Web API-Framework, unabhängig davon, ob die Anwendung in einer OWIN-Pipeline-Komponenten aus dem Katana-Projekt ausgeführt wird. Der Speicherort, in dem OWIN-bezogenen Code an den Anwendungsentwickler angezeigt werden, werden den Anwendungsstartcode, in denen der Entwickler die OWIN-Pipeline verfasst. In den Startcode registriert der Entwickler eine Reihe von UseXx-Anweisungen, in der Regel eine für jede middlewarekomponente, die eingehende Anforderungen verarbeitet. Diese Variante wird dieselbe Wirkung wie das Registrieren von HTTP-Module in der aktuellen System.Web-Welt haben. In der Regel eine größere Framework Middleware, z. B. ASP.NET Web-API oder [SignalR](../../../signalr/index.md) wird am Ende der Pipeline registriert werden. Querschnittliche middlewarekomponenten gemeinsam datenprofilen für die Authentifizierung oder zwischenspeichern, werden in der Regel bis zum Anfang der Pipeline registriert, damit Anforderungen für alle Frameworks und Komponenten registriert weiter unten in der Pipeline verarbeitet wird. Diese Trennung der Middleware Komponenten voneinander und aus den zugrunde liegenden Infrastrukturkomponenten kann die Komponenten auf verschiedenen Geschwindigkeiten weiterentwickelt werden, während Sie sicherstellen, dass das gesamte System stabil bleibt.

## <a name="components--nuget-packages"></a>Komponenten – NuGet-Pakete

Wie viele aktuelle Bibliotheken und Frameworks werden die Komponenten der Katana-Projekt als Satz von NuGet-Pakete übermittelt. Für das bevorstehende, Version 2.0 sieht das Abhängigkeitsdiagramm für Katana-Pakets wie folgt aus. (Klicken Sie auf das Bild für größere Ansicht.)

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Fast jedes Paket in das Katana-Projekt abhängig ist, weder direkt noch indirekt auf das Owin-Paket. Sie können denken Sie daran, dass dies das Paket, das die IAppBuilder-Schnittstelle eine konkrete Implementierung der Startsequenz Anwendung in der OWIN-Spezifikation, Abschnitt 4 beschriebenen bietet enthält. Darüber hinaus richten sich viele der Pakete nach "Microsoft.owin", die eine Reihe von Hilfstypen für das Arbeiten mit HTTP-Anforderungen und-Antworten bereitstellt. Der Rest des Pakets kann als Host Infrastruktur-Pakete (Server oder Hosts) oder Middleware klassifiziert werden. Pakete und Abhängigkeiten, die außerhalb der Katana-Projekt befinden, werden in orange dargestellt angezeigt.

Die-Hostinfrastruktur für Katana 2.0 umfasst die SystemWeb und HttpListener-basierte Server, das OwinHost-Paket zum Ausführen von OWIN-Anwendungen mit OwinHost.exe, und das Paket "Microsoft.owin.Hosting" für das Selbsthosting OWIN-Anwendungen in einem benutzerdefinierter Host (z. B. Konsolenanwendung, Windows-Dienst usw.)

Katana 2.0 sind die Middleware-Komponenten in erster Linie konzentriert sich auf unterschiedliche Weise der Authentifizierung. Eine zusätzliche middlewarekomponente für die Diagnose wird bereitgestellt, wodurch Unterstützung für eine Seite starten und den Fehler. Mit steigender OWIN in die de facto hosting Abstraktion vergrößert das Ökosystem der Middleware-Komponenten, beide entwickelt von Microsoft und Drittanbietern, die ebenfalls in Anzahl.

## <a name="conclusion"></a>Schlussfolgerung

 Von Anfang wurde das Katana-Projekt-Ziel nicht zum Erstellen und dadurch erzwingen Entwickler noch einem anderen Webframework erfahren Sie. Stattdessen wurde das Ziel eine Abstraktion so erteilen Sie .NET Web-Anwendungsentwickler mehr Auswahl wurde zuvor möglich erstellen. Durch die Aufteilung der logischen Ebenen eines typischen Web Application Stack in einen Satz von austauschbaren Komponenten, ermöglicht das Katana-Projekt Komponenten im ganzen Stapel an die Rate sinnvoll ist für diese Komponenten verbessern. Erstellen Sie zunächst alle Komponenten, um die einfache OWIN-Abstraktion, ermöglicht Katana-Frameworks und die Anwendung baut auf den sie über eine Vielzahl von unterschiedlichen Servern und Hosts portabel sein. Verlegen des Entwicklers Kontrolle über den Stapel Katana wird sichergestellt, dass die endgültige Auswahl zu wie einfache werden vom Entwickler durchgeführt oder wie funktionsreiche ihre Web-Stapel werden soll.  
  

## <a name="for-more-information-about-katana"></a>Weitere Informationen zu Katana

- Das Katana-Projekt auf GitHub: [https://github.com/aspnet/AspNetKatana/](https://github.com/aspnet/AspNetKatana/).
- Video: [Katana-Projekt - OWIN für ASP.NET](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), indem Howard Dierking.

## <a name="acknowledgements"></a>Bestätigungen

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick ist ein senior programming Writer zum Microsoft Azure und MVC konzentrieren.
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter [ @jongalloway ](https://twitter.com/jongalloway) )
