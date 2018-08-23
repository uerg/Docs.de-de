---
uid: aspnet/overview/owin-and-katana/an-overview-of-project-katana
title: Eine Übersicht über das Katana-Projekt | Microsoft-Dokumentation
author: howarddierking
description: ASP.NET Framework schon seit über zehn Jahren, und die Plattform die Entwicklung von unzähligen Websites und Dienste aktiviert hat. Als Web-Anwendung...
ms.author: riande
ms.date: 08/30/2013
ms.assetid: 0ee21741-c1bf-4025-a9b0-24580cae24bc
msc.legacyurl: /aspnet/overview/owin-and-katana/an-overview-of-project-katana
msc.type: authoredcontent
ms.openlocfilehash: 52007eba109de28c6d178505b82b1d5ff2883b47
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833905"
---
<a name="an-overview-of-project-katana"></a>Eine Übersicht über das Katana-Projekt
====================
durch [Howard Dierking](https://github.com/howarddierking)

> ASP.NET Framework schon seit über zehn Jahren, und die Plattform die Entwicklung von unzähligen Websites und Dienste aktiviert hat. Wie webentwicklungsstrategien für die Anwendung entwickelt haben, wurde das Framework in Schritt mit Technologien wie ASP.NET MVC und ASP.NET Web API weiterentwickeln können. Während der Entwicklung von Webanwendungen der nächsten evolutionären Schritt in die Welt des Cloudcomputings dauert, Projekt [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) stellt die zugrunde liegende Reihe von Komponenten für ASP.NET-Anwendungen, sodass sie flexibel, portable, einfache, und geben Sie eine bessere Leistung – anders ausgedrückt, Projekt- [Katana](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET) Cloud optimiert Ihre ASP.NET-Anwendungen.


## <a name="why-katana--why-now"></a>Warum Katana – Warum jetzt?

 Unabhängig davon, ob eine ein Developer-Framework oder durch den Endbenutzer-Produkt bespricht, es ist wichtig zu verstehen, die zugrunde liegenden Gründe für das Erstellen das Produkt und Teil eines solchen enthält kennen, die für das Produkt erstellt wurde. ASP.NET wurde ursprünglich mit zwei Kunden Denken Sie daran erstellt.   
  
**Die erste Gruppe von Kunden wurde klassische ASP-Entwickler.** Damals war ASP mit einer der den zentralen Technologien zum Erstellen von dynamische, datengesteuerte Websites und Anwendungen durch die Verflechtung von Markup und serverseitigen Skript. Die ASP-Runtime angegeben serverseitigen Skript mit einem Satz von Objekten, die abstrahiert die Kernaspekte der vom zugrunde liegenden HTTP-Protokoll und den Webserver und die bereitgestellten services-Zugriff auf zusätzliche solche Sitzungs- und Zustandsverwaltung, cache usw. an. Zwar leistungsstark, wurde der klassische ASP-Anwendungen eine Herausforderung darstellen, wie sie in der Größe und Komplexität wächst verwalten. Dies war größtenteils aufgrund des Mangels an der Struktur, die in skriptumgebungen, die zusammen mit der Duplizierung von Code aus dem überlappen von Code und Markup gefunden. Um auf den Stärken von klassischem ASP nutzen zu können unter Berücksichtigung einiger auch einige Herausforderungen, nutzte ASP.NET der Code-Organisation, die von der objektorientierten Sprachen von .NET Framework bereitgestellt wird, und gleichzeitig auch das serverseitige-Programmiermodell mit der klassischen ASP wuchs Entwickler vertraut.

**Die zweite Gruppe von Zielkunden für ASP.NET war Windows Entwickler von Geschäftsanwendungen.** Im Gegensatz zum klassischen ASP-Entwickler, die zum Schreiben von HTML-Markup und der Code zum Generieren von HTML-Markup Weitere daran gewöhnt sind, Windows Forms-Entwickler (z. B. die VB6-Entwickler vor ihnen) vertraut, die eine Erfahrung zur Entwurfszeit, das eine Leinwand und einen umfangreichen Satz an Benutzer aufgenommen wurden Steuerelemente der Benutzeroberfläche. Die erste Version von ASP.NET – bereitgestellten auch bekannt als "Webformulare" eine ähnliche Erfahrung zur Entwurfszeit zusammen mit einer serverseitigen Ereignismodell für Komponenten der Benutzeroberfläche und eine Reihe von Infrastrukturfunktionen (wie z. B. ViewState) um eine nahtlose entwicklerumgebung zu erstellen. zwischen Client und Server-Side-Programmierung. Web Forms hid effektiv des Webs zustandslosigkeit unter einer zustandsbehafteten Ereignismodell, der WinForms-Entwicklern vertraut war.

### <a name="challenges-raised-by-the-historical-model"></a>Herausforderungen meistern, indem herkömmliche Modell

**Das Ergebnis war eine ausgereifte, funktionsreiche Common Language Runtime und Developer-Programmiermodell.** Mit dem kam Funktionsreichtum jedoch einige wichtige Herausforderungen. Zuerst das Framework wurde **monolithische**, logisch unterschiedlichen Einheiten von Funktionen, die in der gleichen Assembly "System.Web.dll" (z. B. die Core-HTTP-Objekte mit dem Web Forms-Framework) eng gekoppelt. Zweitens wurde ASP.NET enthalten ist, im Rahmen der größere .NET Framework, die das bedeutete das **Abständen zwischen den Releases Jahre war.** Dies machte es schwierig für ASP.NET mit allen Änderungen in die Webentwicklung, sich schnell entwickelnden Schritt zu halten. Schließlich wurde die "System.Web.dll" selbst in mehrere Möglichkeiten, um eine bestimmte Webhosting-Option gekoppelt: Internetinformationsdienste (Internet Information Services, IIS).

### <a name="evolutionary-steps-aspnet-mvc-and-aspnet-web-api"></a>Entwicklungsschritte: ASP.NET MVC und ASP.NET Web-API

Und viele Änderung wurde in der Webentwicklung passiert! Webanwendungen wurden immer mehr als eine Reihe von klein und konzentriert sich Komponenten anstelle von großen Frameworks entwickelt. Die Anzahl der Komponenten sowie die Häufigkeit, mit der sie veröffentlicht wurden, wurde eine je schneller erhöhen. Es wurde deutlich, dass bleiben Schritt mit dem Web Frameworks, die mehr und mehr funktionsreiche, kleinere und stärker fokussierte entkoppelte daher erhalten erfordert die **ASP.NET-Team hat mehrere Entwicklungsschritte ASP.NET als eine Familie von aktivieren austauschbare Webkomponenten statt einem einzigen Framework**.

Eine der frühen Änderungen war der zunehmenden Beliebtheit der bekannten Model-View-Controller (MVC)-Entwurfsmuster Dank Webentwicklungs-Frameworks wie Ruby on Rails. Diese Art von Webanwendungen, hat den Entwickler größere Kontrolle über Markup der Anwendung gleichzeitig die Trennung von Markup und Geschäftslogik, die eines der ersten Verkaufsargumente für ASP.NET war. Um die Anforderungen für diese Art der Entwicklung von Webanwendungen, Microsoft hat die Gelegenheit zum eigenen positionieren besser für die Zukunft von **Entwickeln von ASP.NET MVC-of-Band** (und nicht Integrieren der Codekomponente in .NET Framework). ASP.NET MVC wurde als unabhängige Download veröffentlicht. Dem engineering-Team erhielt die Flexibilität, sehr viel häufiger als zuvor möglich gewesen zur Verfügung stellen.

Eine andere wesentliche Veränderung in Entwicklung von Webanwendungen wurde der Wechsel von dynamischen, vom Server generierten Webseiten zu statischen ausgehenden Markups mit dynamischen Abschnitten der Seite generiert wird, kommuniziert der Client-seitige Skript **mit Back-End-Web-APIs über AJAX-Anforderungen**. Diese architektonische Änderung konnte der Aufstieg der Web-APIs und die Entwicklung von ASP.NET Web-API-Framework Vorteile verschaffen. Wie im Fall von ASP.NET MVC, bereitgestellt, die Version von ASP.NET Web-API eine weitere Verwendungsmöglichkeit für ASP.NET als ein modularer Framework zu entwickeln. Das engineering-Team hat die Gelegenheit nutzen und **ASP.NET Web-API so erstellt, dass es keine Abhängigkeiten auf die Core-Framework-Typen finden Sie in "System.Web.dll" hatte**. Diese Option aktiviert, zwei Dinge: Erstens vorgesehen, die ASP.NET Web-API in einer vollständig in sich geschlossenen Weise weiterentwickeln kann (und sie können weiterhin schnell durchlaufen werden, da sie über NuGet bereitgestellt wird). Da gab es keine externen Abhängigkeiten zu "System.Web.dll", und daher keine Abhängigkeiten mit IIS, enthalten ASP.NET Web-API zum anderen die Möglichkeit zur Ausführung in einem benutzerdefinierten Host (z. B. eine Konsolenanwendung, Windows-Dienst, usw.)

### <a name="the-future-a-nimble-framework"></a>Die Zukunft: Ein flexibler Framework

Framework-Komponenten voneinander entkoppelt, und klicken Sie dann auf NuGet freigegeben werden, könnten Frameworks jetzt **durchlaufen wesentlich unabhängiger und schneller**. Darüber hinaus die Leistungsfähigkeit und Flexibilität von Web-API Self-hosting-Funktion für mussten Entwickler sehr attraktiv erwies eine **kleiner, einfacher Host** für ihre Dienste. So attraktiv nachgewiesen, in der Tat, dass andere Frameworks zudem soll demonstriert werden diese Funktion und dadurch keine neue Herausforderung eingeblendet, jedes Framework in einem eigenen Hostprozess auf seiner eigenen Basisadresse ausgeführt und benötigt für die Verwaltung (gestartet, beendet, usw.) unabhängig voneinander. Eine moderne Webanwendung unterstützt in der Regel statische Dateiserverfunktionalität, dynamische seitengenerierung, Web-API und mehr vor kurzem real-time/erste Schritte mit Pushbenachrichtigungen. Wird erwartet, dass jeder dieser Dienste ausgeführt und unabhängig voneinander verwaltet werden sollten war einfach nicht realistisch.

Benötigt wurde, war einzelne hosting Abstraktion, mit denen ein Entwickler zum Verfassen einer Anwendung aus einer Vielzahl von verschiedenen Komponenten und Frameworks und führen Sie dann die Anwendung auf einem Host unterstützen würden.

## <a name="the-open-web-interface-for-net-owin"></a>Die offene Webschnittstelle für .NET (OWIN)

 Durch die Vorteile, die erreicht, indem Sie sich inspirieren [Rack](http://rack.github.io/) in der Ruby-Community verschiedene Mitglieder der Community .NET legen eine Abstraktion zwischen Webservern und Framework-Komponenten zu erstellen. Zwei Ziele beim Entwurf für die OWIN-Abstraktion waren, dass es einfach war und die wenigsten möglichen Abhängigkeiten auf anderen Frameworktypen dauerte. Diese beiden Ziele sichergestellt werden:

- Neue Komponenten können einfacher entwickelt und genutzt werden.
- Anwendungen können einfacher zwischen Hosts und potenziell gesamte Plattformen/Betriebssysteme portiert werden.

Die resultierende Abstraktion besteht aus zwei Elementen der Core. Die erste ist das Umgebungswörterbuch. Diese Datenstruktur ist verantwortlich für das Speichern des Status für die Verarbeitung einer HTTP-Anforderung und Antwort als auch alle relevanten Serverzustand erforderlich. Das Umgebungswörterbuch wird wie folgt definiert:

[!code-console[Main](an-overview-of-project-katana/samples/sample1.cmd)]

Ein OWIN-kompatiblen Webserver ist verantwortlich für das Auffüllen von des Umgebungswörterbuch mit Daten, z. B. die Textstreams und Header-Auflistungen für eine HTTP-Anforderung und Antwort. Es ist dann der Verantwortung der Anwendung oder ein Framework-Komponenten zum Auffüllen oder aktualisieren das Wörterbuch mit zusätzlichen Werten aus, und Schreiben in den Antworttext-Datenstrom.

Zusätzlich zum Angeben des Typs für das Umgebungswörterbuch, definiert die OWIN-Spezifikation eine Liste von Core Wörterbuch Schlüssel-Wert-Paaren. Die folgende Tabelle zeigt beispielsweise die erforderlichen Wörterbuchschlüssel für eine HTTP-Anforderung:

| Schlüsselname | Wertbeschreibung |
| --- | --- |
| `"owin.RequestBody"` | Ein Stream mit dem Anforderungstext, sofern vorhanden. "Stream.Null" kann als Platzhalter verwendet werden, wenn kein Anforderungstext vorhanden ist. Finden Sie unter [Anforderungstext](http://owin.org/html/owin.html#34-request-body-100-continue-and-completed-semantics). |
| `"owin.RequestHeaders"` | Ein `IDictionary<string, string[]>` von Anforderungsheadern. Finden Sie unter [Header](http://owin.org/html/owin.html#3-3-headers). |
| `"owin.RequestMethod"` | Ein `string` , die die HTTP-Anforderungsmethode der Anforderung enthält (z. B. `"GET"`, `"POST"`). |
| `"owin.RequestPath"` | Ein `string` , die den Pfad der Anforderung enthält. Der Pfad muss relativ zu den "Stamm" des anwendungsdelegaten; sein. finden Sie unter [Pfade](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestPathBase"` | Ein `string` angezeigt, die Teil der Anforderungspfad für den "Stamm" des anwendungsdelegaten; enthält [Pfade](http://owin.org/html/owin.html#5-3-paths). |
| `"owin.RequestProtocol"` | Ein `string` , enthält der Name des Protokolls und der Version (z. B. `"HTTP/1.0"` oder `"HTTP/1.1"`). |
| `"owin.RequestQueryString"` | Ein `string` , enthält die Komponente der Abfragezeichenfolge des HTTP-Anforderungs-URI, ohne das führende "?" (z. B. `"foo=bar&baz=quux"`). Der Wert kann es sich um eine leere Zeichenfolge sein. |
| `"owin.RequestScheme"` | Ein `string` , die für die Anforderung verwendete URI-Schema enthält (z. B. `"http"`, `"https"`); finden Sie unter [URI-Schema](http://owin.org/html/owin.html#5-1-uri-scheme). |

Das zweite Schlüsselelement von OWIN ist der anwendungsdelegat. Dies ist einer Funktionssignatur, die als die primäre Schnittstelle zwischen allen Komponenten in einer OWIN-Anwendung dient. Die Definition für den anwendungsdelegaten sieht folgendermaßen aus:

`Func<IDictionary<string, object>, Task>;`

Der anwendungsdelegat ist einfach eine Implementierung der Typ des Func-Delegaten, in dem die Funktion das Umgebungswörterbuch als Eingabe akzeptiert und gibt einen Task zurück. Dieser Entwurf hat mehrere Implikationen für Entwickler:

- Es gibt sehr wenige typabhängigkeiten erforderlich, um die OWIN-Komponenten zu schreiben. Dadurch erhöht sich erheblich auf die Barrierefreiheit von OWIN für Entwickler.
- Der asynchrone Entwurf ermöglicht die Abstraktion, mit der Behandlung von Computerressourcen, insbesondere in mehrere e/a-intensive Vorgänge effizient sein.
- Da der anwendungsdelegat eine unteilbare Einheit der Ausführung ist, und da das Umgebungswörterbuch als Parameter für den Delegaten übergeben werden, OWIN-Komponenten problemlos verkettet werden können zusammengeführt, um komplexe HTTP-Verarbeitung von Pipelines zu erstellen.

Aus Implementierungssicht OWIN ist eine Spezifikation ([http://owin.org/html/owin.html](http://owin.org/html/owin.html)). Das Ziel ist es nicht zu den nächsten Web-Framework, jedoch stattdessen eine Spezifikation für die Interaktion von Web-Frameworks und Webservern.

Wenn Sie untersucht haben [OWIN](http://owin.org/) oder [Katana](https://github.com/aspnet/AspNetKatana/wiki), Sie haben wahrscheinlich ebenfalls bemerkt der [Owin-NuGet-Paket](http://nuget.org/packages/Owin) und Owin.dll. Diese Bibliothek enthält eine einzige Benutzeroberfläche [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs), welche formalisiert und wird beschriebene, die der Startsequenz [Abschnitt 4](http://owin.org/html/owin.html#4-application-startup) der OWIN-Spezifikation. Zwar nicht erforderlich, zum Erstellen der OWIN-Servern, die [IAppBuilder](https://github.com/owin/owin/blob/master/src/Owin/IAppBuilder.cs) Schnittstelle bietet konkrete Bezugspunkt, und sie wird von den Komponenten des Katana-Projekt verwendet.

## <a name="project-katana"></a>Katana-Projekt

Während sowohl die [OWIN](http://owin.org/html/owin.html) Spezifikation und *Owin.dll* sind Community, die im Besitz und Community-open-Source-bemühungen, führen die [Katana](https://github.com/aspnet/AspNetKatana/wiki) Projekt stellt den Satz von OWIN dar. Komponenten, die beim weiterhin open-Source-erstellt und von Microsoft veröffentlicht werden. Zu diesen Komponenten gehören sowohl Infrastrukturkomponenten, wie z. B. Hosts und -Servern, als auch funktionalen Komponenten, z. B. Authentication-Komponenten und -Bindungen mit Frameworks wie z. B. [SignalR](../../../signalr/index.md) und [ASP.NET-Webanwendung API](../../../web-api/overview/getting-started-with-aspnet-web-api/index.md). Das Projekt enthält die folgenden drei allgemeine Ziele: 

- **Portable** – Komponenten sollten in der Lage, einfach für neue Komponenten ersetzt werden soll, sobald sie verfügbar sind. Dies schließt alle Typen von Komponenten, aus dem Framework auf dem Server und Host. Das hat zur Folge dieses Ziel ist, dass Drittanbieter-Frameworks nahtlos auf Microsoft-Servern ausgeführt werden können, während-Frameworks von Microsoft möglicherweise auf Drittanbieter-Servern und -Hosts ausgeführt werden können.
- **Modulare/flexible**– im Gegensatz zu vielen Frameworks, die eine Vielzahl von Features enthalten, die standardmäßig aktiviert sind, Komponenten von Katana-Projekt muss klein und konzentriert, die Kontrolle über an den Anwendungsentwickler bei der Bestimmung der Komponenten Verwenden Sie in ihre Anwendung.
- **Lightweight leistungsfähigen /, skalierbarer** – wichtige das herkömmliche Konzept eines Frameworks in einen Satz von kleinen, fokussierten Komponenten, die explizit vom Entwickler Anwendung hinzugefügt werden, eine resultierende Katana-Anwendung kann weniger computing nutzen Ressourcen, und daher höhere Auslastung als bei anderen Arten von Servern und Frameworks. Da die Anforderungen der Anwendung weitere Funktionen aus der zugrunde liegenden Infrastruktur erfordern, die können die OWIN-Pipeline hinzugefügt werden, aber dies sollte eine ausdrückliche Entscheidung seitens des Entwicklers der Anwendung sein. Darüber hinaus also die Ersetzbarkeit von Komponenten auf niedrigerer Ebene sobald sie verfügbar sind, neue Server mit hoher Leistungsfähigkeit nahtlos eingeführt werden, können um die Leistung der OWIN-Anwendungen zu verbessern, ohne diese Anwendungen.

## <a name="getting-started-with-katana-components"></a>Erste Schritte mit dem Katana-Komponenten

Wenn sie erstmals eingeführt wurde, ein Aspekt der [Node.js](http://nodejs.org/) Framework, das unmittelbar Aufmerksamkeit gezeichnet wurde, wurde die Einfachheit, mit dem eine erstellen und Ausführen einer Webserver konnte. Wenn Ziele von Katana Lichte der eingeschlossen wurden [Node.js](http://nodejs.org/), eine möglicherweise diese zusammenfassen, darauf hin, dass Katana, die viele der Vorteile bringt [Node.js](http://nodejs.org/) (und Frameworks wie es) ohne dass den Entwickler wegwerfen Alles, was sie zum Entwickeln von Webanwendungen mit ASP.NET weiß. Für diese Anweisung, die "true" enthalten soll, erste Schritte mit dem Katana-Projekt muss auf ebenso einfache [Node.js](http://nodejs.org/).

## <a name="creating-hello-world"></a>Erstellen von "Hello World!"

Ein deutlicher Unterschied zwischen JavaScript und .NET Entwicklung ist das Vorhandensein (oder fehlen) eines Compilers. Daher ist der Ausgangspunkt für einen einfachen Katana-Server eine Visual Studio-Projekt. Allerdings können wir mit der Projekttypen die minimalste beginnen: die leere ASP.NET-Webanwendung.

[![](an-overview-of-project-katana/_static/image1.png)](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb)

Als Nächstes installieren wir die ["Microsoft.owin.Host.systemweb"](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) NuGet-Paket in das Projekt. Dieses Paket stellt einen OWIN-Server, der in der ASP.NET-Pipeline für die Anforderung ausgeführt wird. Es befinden sich die [NuGet-Katalog](http://nuget.org/packages/Microsoft.Owin.Host.SystemWeb) und können installiert werden, verwenden entweder das Visual Studio-Dialogfeld "Paket-Manager" oder die Paket-Manager-Konsole mit dem folgenden Befehl:

[!code-console[Main](an-overview-of-project-katana/samples/sample2.cmd)]

Installieren der `Microsoft.Owin.Host.SystemWeb` Paket einige zusätzliche Pakete als Abhängigkeiten installiert. Einer dieser Abhängigkeiten ist `Microsoft.Owin`, sodass mehrere Hilfstypen und Methoden für die Entwicklung von OWIN-Anwendungen bereitstellt. Wir können diese Typen verwenden, um schnell die folgenden "Hello World"-Server zu schreiben.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample3.cs)]

Diese sehr einfache Web-Server kann jetzt ausgeführt werden mithilfe von Visual Studio **F5** Befehl und bietet volle Unterstützung für das Debuggen.

## <a name="switching-hosts"></a>Durch den Wechsel des hosts

Standardmäßig führt die vorherigen "Hello World"-Beispiel in der ASP.NET-Pipeline für die Anforderung, die "System.Web" im Kontext von IIS verwendet. Sich können als außerordentlich wertvoll hinzufügen, wie es uns, die von der Flexibilität und zusammensetzbarkeit von einer OWIN-Pipeline mit den Managementfunktionen und allgemeinen Fälligkeit von IIS profitieren kann. Allerdings gibt es möglicherweise Fälle, in denen die Vorteile von IIS sind nicht erforderlich, und der Wunsch, die für eine kleinere, einfachere Host besteht. Was ist, klicken Sie dann erforderlich, unsere einfache Web-Server außerhalb von IIS und "System.Web" ausgeführt werden?

Um das Ziel der Portabilität zu veranschaulichen, erfordert das Verschieben von einem Webserver-Host zu einem Host über die Befehlszeile einfach die neuen Server und Host-Abhängigkeiten in den Projektausgabeordner hinzufügen und starten Sie dann auf den Host. In diesem Beispiel hosten wir unsere Web-Server in der ein Katana-Host bezeichnet `OwinHost.exe` und den Katana-HttpListener-basierten Server verwenden. Auf ähnliche Weise werden für die anderen Katana-Komponenten, diese aus NuGet mithilfe des folgenden Befehls abgerufen werden:

[!code-console[Main](an-overview-of-project-katana/samples/sample4.cmd)]

Über die Befehlszeile verwenden, können wir navigieren Sie dann zum Stammordner Projekts und führen Sie einfach die `OwinHost.exe` (die in den Ordner "Tools" des jeweiligen NuGet-Paket installiert wurde). In der Standardeinstellung `OwinHost.exe` suchen Sie nach dem HttpListener-basierten Server konfiguriert ist und daher keine zusätzliche Konfiguration erforderlich ist. Navigieren in einem Webbrowser zu `http://localhost:5000/` zeigt die Anwendung nun über die Konsole ausgeführt wird.

![](an-overview-of-project-katana/_static/image2.png)

## <a name="katana-architecture"></a>Katana-Architektur

 Die Katana-Komponentenarchitektur unterteilt eine Anwendung in vier logische Schichten, aus, wie unten dargestellt: *Host "," Server ","-Middleware* und *Anwendung*. Komponentenarchitektur werden so berücksichtigt, dass Implementierungen dieser Ebenen einfach, in vielen Fällen ersetzt werden können, ohne erneute Kompilierung der Anwendung.   

![](an-overview-of-project-katana/_static/image3.png)

## <a name="host"></a>Host

 Der Host ist verantwortlich für:

- Verwalten von den zugrunde liegenden Prozess aus.
- Orchestrieren den Workflow, der in der Auswahl eines Servers und der Erstellung einer OWIN-Pipeline über die Anforderungen, die Ergebnisse verarbeitet werden.

  Derzeit stehen 3 primäre Hostingoptionen für Katana-basierte Anwendungen zur Verfügung:  
  
**IIS-/ASP.NET**: verwenden die Standardtypen HttpModule und meinem HttpHandler abzukoppeln, OWIN-Pipelines in IIS als Teil einer Request-ASP.NET-Ablauf ausführen können. Unterstützung für das hosting ASP.NET ist aktiviert, durch die Installation von Microsoft.AspNet.Host.SystemWeb NuGet-Paket in ein Webanwendungsprojekt. Darüber hinaus, da IIS als einen Host und einem Server fungiert, wird die Unterscheidung der OWIN-Server-Host in enger Verbindung im NuGet-Paket, was bedeutet, dass wenn den SystemWeb-Host zu verwenden, ein Entwickler eine Implementierung eines alternativen Servers ersetzen nicht möglich.  
  
**Benutzerdefinierten Host**: das Katana-Komponenten-Suite ermöglicht Entwicklern die Möglichkeit zum Hosten von Anwendungen in ihrem eigenen benutzerdefinierten Prozess, ob das ist eine Konsolenanwendung, Windows-Dienst usw. Diese Funktion ähnelt der Self-Hosting-Funktion von Web-API bereitgestellt. Das folgende Beispiel zeigt einen benutzerdefinierten Host von Web-API-Code:  

[!code-csharp[Main](an-overview-of-project-katana/samples/sample5.cs)]

Das Self-Hosting-Setup für eine Anwendung Katana ist ähnlich:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample6.cs)]

Ein deutlicher Unterschied zwischen der Web-API und Katana selbst gehostete Beispiele ist, dass der Web-API-Konfigurationscode aus dem Katana-Self-Hosting-Beispiel fehlt. Damit können sowohl auf Portabilität und zusammensetzbarkeit, trennt Katana den Code, der der Server aus dem Code gestartet wird, die die Pipeline zur anforderungsverarbeitung konfiguriert. Der Code, der Web-API konfiguriert, und klicken Sie dann befindet sich in der Klasse Start, das zusätzlich wie der Typparameter in WebApplication.Start angegeben wird.

[!code-csharp[Main](an-overview-of-project-katana/samples/sample7.cs)]

Die Startup-Klasse wird später in diesem Artikel ausführlicher besprochen. Jedoch erforderlich, der Code zu einem Katana, Self-Hosting-Prozess noch den Code ähnelt, die Sie noch heute in ASP.NET Web-API selbst gehostete Anwendungen verwenden können.

**OwinHost.exe**: während einige einen benutzerdefinierten Prozess zum Ausführen von Katana-Web-Anwendungen schreiben möchten wird, bevorzugen viele einfach eine vorab erstellte ausführbare Datei starten, das Starten eines Servers und ihre Anwendung ausführen können. In diesem Szenario die Katana-Komponenten-Suite enthält `OwinHost.exe`. Wenn im Stammverzeichnis des Projekts ausführen, wird diese ausführbare Datei Starten eines Servers (es wird der HttpListener-Server standardmäßig verwendet) und verwenden die Konventionen zum Suchen und Ausführen des Benutzers Startup-Klasse. Für eine präzisere Steuerung bietet die ausführbare Datei eine Anzahl von zusätzlichen Befehlszeilenparameter an.

![](an-overview-of-project-katana/_static/image4.png)

## <a name="server"></a>Server

 Während der Host verantwortlich ist für das Starten und Verwalten des Prozesses, in dem die Anwendung ausgeführt wird, die Verantwortung des Servers, werden durch den Benutzer (wie Sie an ein Netzwerksocket öffnen, für Anforderungen zu Lauschen und senden sie über die Pipeline von OWIN-Komponenten angegeben bereits bemerkt haben, diese Pipeline in der Startup-Klasse für den Entwickler angegeben ist). Das Katana-Projekt umfasst derzeit zwei serverimplementierungen: 

- **"Microsoft.owin.Host.systemweb"**: wie bereits erwähnt, IIS zusammen mit der ASP.NET-Pipeline fungiert sowohl als Host als auch einen Server. Aus diesem Grund bei der diese Auswahl Hostingoption IIS sowohl verwaltet auf Hostebene-Aspekte wie prozessaktivierung und Lauscht auf HTTP-Anforderungen. Für ASP.NET-Webanwendungen sendet er die Anforderungen dann in der ASP.NET-Pipeline. Der Katana-SystemWeb-Host wird ein ASP.NET HttpModule und meinem HttpHandler abzukoppeln, zum Abfangen von Anforderungen, wie sie die HTTP-Pipeline passieren, und senden sie durch die OWIN-Pipeline für den Benutzer angegebenen registriert.
- **Microsoft.Owin.Host.HttpListener**: wie der Name schon sagt, verwendet dieser Katana-Server die .NET Framework HttpListener-Klasse, um einen Socket öffnen, und Senden von Anforderungen in einer OWIN-Pipeline von Entwicklern. Dies ist derzeit die Standardauswahl für die Server für die Katana-Self-Hosting-API und OwinHost.exe.

## <a name="middlewareframework"></a>Middleware/Frameworks

 Wie bereits erwähnt ist es, wenn der Server eine Anforderung von einem Client akzeptiert verantwortlich für und übergeben sie über eine Pipeline mit OWIN-Komponenten, die durch den Startcode des Entwicklers angegeben werden. Diese Pipelinekomponenten werden als Middleware bezeichnet.  
 Auf einer sehr einfachen Ebene muss eine OWIN-Middleware-Komponente einfach der anwendungsdelegat OWIN zu implementieren, sodass sie aufgerufen werden kann.

[!code-console[Main](an-overview-of-project-katana/samples/sample8.cmd)]

Um die Entwicklung und die Zusammensetzung der Middleware-Komponenten zu vereinfachen, unterstützt Katana jedoch eine Reihe von Konventionen und Hilfstypen für Middleware-Komponenten. Die am häufigsten verwendeten davon ist die `OwinMiddleware` Klasse. Eine benutzerdefinierte Middleware-Komponente, die mit dieser Klasse erstellt, würde etwa wie folgt aussehen: 

[!code-csharp[Main](an-overview-of-project-katana/samples/sample9.cs)]

 Diese Klasse wird von `OwinMiddleware`, implementiert einen Konstruktor, der eine Instanz der die nächste Middleware in der Pipeline wird als eines ihrer Argumente akzeptiert, und leitet diese dann an den Basiskonstruktor. Zusätzliche Argumente verwendet, um die Middleware konfigurieren, werden auch als Konstruktorparameter nach dem nächsten middlewareparameter deklariert.   
  
Zur Laufzeit die Middleware ausgeführt wird über die überschriebene `Invoke` Methode. Diese Methode akzeptiert ein einzelnes Argument vom Typ `OwinContext`. Diesem Kontextobjekt erfolgt über die `Microsoft.Owin` NuGet-Paket zuvor beschriebenen und stellt stark typisierten Zugriff auf das Wörterbuch-Anforderung, Antwort und -Umgebung, zusammen mit einigen zusätzlichen Hilfstypen.   
  
Die Middleware-Klasse kann problemlos die OWIN-Pipeline im Anwendungsstartcode wie folgt hinzugefügt werden:   

[!code-csharp[Main](an-overview-of-project-katana/samples/sample10.cs)]

Da die Katana-Infrastruktur einfach Einrichten einer Pipeline von OWIN-Middleware-Komponenten erstellt, und die Komponenten einfach der anwendungsdelegat zur Teilnahme an der Pipelines unterstützen müssen, können Middleware-Komponenten Komplexität aufweisen – angefangen einfache liegen. Protokollierungen, die gesamte Frameworks wie ASP.NET Web-API oder [SignalR](../../../signalr/index.md). Hinzufügen von ASP.NET Web-API für die vorherigen OWIN-Pipeline erfordert beispielsweise den folgenden Startcode hinzufügen:

[!code-csharp[Main](an-overview-of-project-katana/samples/sample11.cs)]

Die Katana-Infrastruktur erstellt die Pipeline Middleware-Komponenten, die basierend auf der Reihenfolge, in der sie das IAppBuilder-Objekt in der "Configuration"-Methode hinzugefügt wurden. In unserem Beispiel können Sie dann LoggerMiddleware alle Anforderungen zu verarbeiten, die fließen über die Pipeline, unabhängig davon, wie diese Anforderungen letztendlich verarbeitet werden. Dies ermöglicht leistungsstarke Szenarien, in denen eine middlewarekomponente (z. B. eine Authentifizierungskomponente) Anforderungen für eine Pipeline verarbeitet werden können, die mehrere Komponenten und Frameworks (z. B. ASP.NET Web-API, SignalR und ein statischer Dateiserver) enthält.
 
## <a name="applications"></a>Anwendungen

Wie von den vorherigen Beispielen gezeigt wird, sollten OWIN und Katana-Projekt nicht als ein neues Programmiermodell der Anwendung, sondern als eine Abstraktion zum Entkoppeln von anwendungsprogrammiermodelle und Frameworks von Server und Hostinginfrastruktur betrachtet werden. Beispielsweise wird beim Erstellen von Web-API-Anwendungen, die Entwickler-Frameworks weiterhin zum Verwenden des ASP.NET Web-API-Frameworks, unabhängig davon, ob die Anwendung in einer OWIN-Pipeline mithilfe von Komponenten aus dem Katana-Projekt ausgeführt wird. Der Ort, in denen OWIN-bezogenem Code angezeigt, an den Anwendungsentwickler werden, werden den Anwendungsstartcode, in denen der Entwickler die OWIN-Pipeline erstellt. In den Startcode Registrieren der Entwickler eine Reihe von UseXx-Anweisungen, in der Regel eine für jede middlewarekomponente, die eingehende Anforderungen verarbeiten. Diese Erfahrung hat dieselbe Wirkung wie das Registrieren von HTTP-Module in der aktuellen System.Web-Welt. In der Regel eine größere Framework Middleware, wie z. B. ASP.NET Web-API oder [SignalR](../../../signalr/index.md) am Ende der Pipeline registriert wird. Übergreifende-Middleware-Komponenten, z. B. für die Authentifizierung oder die Zwischenspeicherung, werden in der Regel bis zum Anfang der Pipeline registriert, damit sie Anforderungen für alle Komponenten, die später in der Pipeline registriert und -Frameworks verarbeitet werden. Diese Trennung von der middlewarekomponenten voneinander und von den zugrunde liegenden Infrastrukturkomponenten kann die Komponenten auf verschiedenen Geschwindigkeiten weiterentwickelt, und gleichzeitig sicherstellen, dass das gesamte System stabil bleibt.

## <a name="components--nuget-packages"></a>Komponenten – NuGet-Pakete

Wie viele aktuelle Bibliotheken und Frameworks werden die Komponenten des Katana-Projekt als eine Reihe von NuGet-Pakete übermittelt. Für die bevorstehende Version 2.0 sieht das Abhängigkeitsdiagramm des Katana-Paket wie folgt aus. (Klicken Sie auf das Bild, um Sie zu vergrößern.)

[![](an-overview-of-project-katana/_static/image6.png)](an-overview-of-project-katana/_static/image5.png)

Fast jedes Paket in das Katana-Projekt abhängt, direkt oder indirekt auf das Owin-Paket. Sie möglicherweise Denken Sie daran, dass es sich um das Paket handelt, das die IAppBuilder-Schnittstelle enthält, bietet eine konkrete Implementierung der starten-Anwendung, die in Abschnitt 4 von der OWIN-Spezifikation beschrieben. Darüber hinaus hängen viele der Pakete Microsoft.Owin, die eine Gruppe von Hilfsprogrammtypen für die Arbeit mit HTTP-Anforderungen und-Antworten bereitstellt. Der Rest des Pakets kann entweder als Host infrastrukturpaketen (Server oder Hosts) oder Middleware klassifiziert werden. Pakete und Abhängigkeiten, die außerhalb des Katana-Projekts sind, werden in Orange angezeigt.

Die hosting-Infrastruktur für Katana 2.0 enthält sowohl der SystemWeb HttpListener-basierte Server, das OwinHost-Paket für die Ausführung von OWIN-Anwendungen, die mit OwinHost.exe, und der Microsoft.Owin.Hosting-Paket für OWIN-Anwendungen im Self-hosting ein benutzerdefinierten Host (z. B.-Konsolenanwendung, Windows-Dienst usw.)

Für Katana 2.0 sind middlewarekomponenten Schwerpunkt auf die Bereitstellung verschiedener Methoden der Authentifizierung. Eine Komponente von weiterer Middleware für die Diagnose wird bereitgestellt, die aktiviert die Unterstützung für eine Seite starten und den Fehler. Mit zunehmender OWIN in die de-facto-hosting-Abstraktion wird das Ökosystem der middlewarekomponenten, entwickelt von Microsoft und Drittanbietern, die auch in Anzahl vergrößert.

## <a name="conclusion"></a>Schlussbemerkung

 Von Anfang wurde das Katana-Projekt-Ziel nicht zu erstellen und erzwingen, dass Entwickler noch eine andere Web-Framework zu erfahren. Das Ziel wurde stattdessen erstellen Sie eine Abstraktion, sodass .NET Entwickler mehr Auswahl als zuvor möglich war. Durch die logische Schichten ein typischer webanwendungsstapel in einen Satz von austauschbaren Komponenten werden aufteilen, ermöglicht das Katana-Projekt Komponenten im ganzen Stapel zur Verbesserung der am jeweils Satz für diese Komponenten sinnvoll ist. Erstellen Sie alle Komponenten, die einfache OWIN-Abstraktion, können Katana-Frameworks und die Anwendungen baut auf den sie für eine Vielzahl von verschiedenen Servern und -Hosts portiert werden. Den Entwickler Kontrolle über den Stapel, einfügen, Katana wird sichergestellt, dass es sich bei der Entwickler die ultimate Wahl zu wie einfache macht oder wie funktionsreiche ihr Webstapel werden soll.  
  

## <a name="for-more-information-about-katana"></a>Weitere Informationen zu Katana

- Das Katana-Projekt auf GitHub: [ https://github.com/aspnet/AspNetKatana/ ](https://github.com/aspnet/AspNetKatana/).
- Video: [das Katana-Projekt - OWIN für ASP.NET](https://channel9.msdn.com/Shows/Web+Camps+TV/The-Katana-Project-OWIN-for-ASPNET), von Howard Dierking.

## <a name="acknowledgements"></a>Bestätigungen

- [Rick Anderson](https://blogs.msdn.com/b/rickandy/): (twitter- [ @RickAndMSFT ](http://twitter.com/RickAndMSFT) ) Rick ist leitender Redakteur bei Microsoft mit Schwerpunkt auf Azure und MVC.
- [Scott Hanselman](http://www.hanselman.com/blog/): (twitter- [ @shanselman ](https://twitter.com/shanselman) )
- [Jon Galloway](https://weblogs.asp.net/jgalloway/default.aspx): (twitter- [ @jongalloway ](https://twitter.com/jongalloway) )
