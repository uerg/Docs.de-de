---
uid: whitepapers/aspnet4/overview
title: ASP.NET 4 und Visual Studio 2010 Web Development Overview | Microsoft-Dokumentation
author: rick-anderson
description: Dieses Dokument enthält eine Übersicht über viele der neuen Features für ASP.NET, die in.NET Framework 4 und in Visual Studio 2010 enthalten sind.
ms.author: aspnetcontent
ms.date: 02/10/2010
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 5c7aa95b18bc0a97f42cc981476c110830286fa5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829042"
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>ASP.NET 4 und Visual Studio 2010 Web Development Overview
====================
> Dieses Dokument enthält eine Übersicht über viele der neuen Features für ASP.NET, die in.NET Framework 4 und in Visual Studio 2010 enthalten sind.
> 
> [In diesem Whitepaper herunterladen](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**Inhalt**

**[Kerndienste](#0.2__Toc253429238 "_Toc253429238")**  
[Datei "Web.config", die Umgestaltung](#0.2__Toc253429239 "_Toc253429239")  
[Erweiterbare Ausgabezwischenspeicherung](#0.2__Toc253429240 "_Toc253429240")  
[AutoStart-Webanwendungen](#0.2__Toc253429241 "_Toc253429241")  
[Umleiten dauerhaft auf einer Seite](#0.2__Toc253429242 "_Toc253429242")  
[Verkleinern der Sitzungszustand](#0.2__Toc253429243 "_Toc253429243")  
[Erweitern den Bereich der zulässigen URLs](#0.2__Toc253429244 "_Toc253429244")  
[Erweiterbare Anforderungsvalidierung](#0.2__Toc253429245 "_Toc253429245")  
[Objekt Zwischenspeichern und die Objekt-Zwischenspeicherung Erweiterbarkeit](#0.2__Toc253429246 "_Toc253429246")  
[Erweiterbare HTML, URL und HTTP-Header, die Codierung](#0.2__Toc253429247 "_Toc253429247")  
[Überwachung der Anwendungsleistung für einzelne Anwendungen in ein einzelner Workerprozess](#0.2__Toc253429248 "_Toc253429248")  
[Multi-Targeting](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery eingeschlossene mit Web Forms und MVC](#0.2__Toc253429251 "_Toc253429251")  
[Content Delivery Network-Unterstützung](#0.2__Toc253429252 "_Toc253429252")  
[Explizite Skripts ScriptManager](#0.2__Toc253429253 "_Toc253429253")

**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Festlegen von Meta-Tags mit dem Page.MetaKeywords-Eigenschaft und Page.MetaDescription-Eigenschaft](#0.2__Toc253429257 "_Toc253429257")  
[Aktivieren der Ansichtszustand für einzelne Steuerelemente](#0.2__Toc253429258 "_Toc253429258")  
[Änderungen an der Browserfunktionen](#0.2__Toc253429259 "_Toc253429259")  
[Routing in ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Festlegen von Client-IDs](#0.2__Toc253429261 "_Toc253429261")  
[Beibehalten von Zeilenauswahl in Datensteuerelementen](#0.2__Toc253429262 "_Toc253429262")  
[Diagrammsteuerelement für ASP.NET](#0.2__Toc253429263 "_Toc253429263")  
[Filtern von Daten mit das QueryExtender-Steuerelement](#0.2__Toc253429264 "_Toc253429264")  
[HTML-codierte Codeausdrücke](#0.2__Toc253429265 "_Toc253429265")  
[Projekt Vorlagenänderungen](#0.2__Toc253429266 "_Toc253429266")  
[CSS-Verbesserungen](#0.2__Toc253429267 "_Toc253429267")  
[Div-Elemente für ausgeblendete Felder ausblenden](#0.2__Toc253429268 "_Toc253429268")  
[Rendern von einem äußeren Tabelle für Steuerelemente mit Vorlagen](#0.2__Toc253429269 "_Toc253429269")  
[ListView-Steuerelement Verbesserungen](#0.2__Toc253429270 "_Toc253429270")  
[CheckBoxList und RadioButtonList-Steuerelement-Verbesserungen](#0.2__Toc253429271 "_Toc253429271")  
[Verbesserungen beim rollenkontextmenü-Steuerelement](#0.2__Toc253429272 "_Toc253429272")  
[Assistenten und CreateUserWizard Steuerelemente 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Unterstützung von Bereichen](#0.2__Toc253429275 "_Toc253429275")  
[Datenanmerkung Attribut Validierungsunterstützung](#0.2__Toc253429276 "_Toc253429276")  
[Vorlagenhelfer](#0.2__Toc253429277 "_Toc253429277")

**[Dynamische Daten](#0.2__Toc253429278 "_Toc253429278")**  
[Aktivieren dynamische Daten für vorhandene Projekte](#0.2__Toc253429279 "_Toc253429279")  
[Deklarative DynamicDataManager-Steuerelement-Syntax](#0.2__Toc253429280 "_Toc253429280")  
[Die Vorlagen für Entitäten](#0.2__Toc253429281 "_Toc253429281")  
[Neue Feldvorlagen für URLs und e-Mail-Adressen](#0.2__Toc253429282 "_Toc253429282")  
[Erstellen von Verknüpfungen mit dem Steuerelement DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Unterstützung von Vererbung im Datenmodell](#0.2__Toc253429284 "_Toc253429284")  
[Unterstützung für m: N Beziehungen (Entitätsframework nur)](#0.2__Toc253429285 "_Toc253429285")  
[Neue Attribute für die Steuerelementanzeige und Unterstützung von Enumerationen](#0.2__Toc253429286 "_Toc253429286")  
[Verbesserte Unterstützung für Filter](#0.2__Toc253429287 "_Toc253429287")

**[Visual Studio 2010 Verbesserungen bei der Webentwicklung](#0.2__Toc253429288 "_Toc253429288")**  
[Verbesserte CSS-Kompatibilität](#0.2__Toc253429289 "_Toc253429289")  
[HTML und JavaScript-Codeausschnitt](#0.2__Toc253429290 "_Toc253429290")  
[JavaScript-IntelliSense-Verbesserungen](#0.2__Toc253429291 "_Toc253429291")

**[Anwendungsbereitstellung mit Visual Studio 2010 Web](#0.2__Toc253429292 "_Toc253429292")**  
[Web-Paketerstellung](#0.2__Toc253429293 "_Toc253429293")  
[Web.config-Transformation](#0.2__Toc253429294 "_Toc253429294")  
[Datenbankbereitstellung](#0.2__Toc253429295 "_Toc253429295")  
[One-Click-Veröffentlichung für Webanwendungen](#0.2__Toc253429296 "_Toc253429296")  
[Ressourcen](#0.2__Toc253429297 "_Toc253429297")

**[Haftungsausschluss](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Kerndienste

ASP.NET 4 enthält eine Reihe von Funktionen, die ASP.NET-Kerndienste, z. B. Zwischenspeicherung der Ausgabe und sitzungszustandspeicherung zu verbessern.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Datei "Web.config", die Umgestaltung

Die `Web.config` -Datei mit der Konfiguration für eine Webanwendung über die letzten einige Versionen von .NET Framework deutlich geworden ist, wie neue Features, z. B. Ajax hinzugefügt wurden, routing und die Integration in IIS 7. Dies hat machte es schwieriger zu konfigurieren oder das Starten neuer Webanwendungen ohne ein Tool wie Visual Studio. In .NET Framework 4 ...die, wurden die wichtigen Konfigurationselemente in verschoben der `machine.config` -Datei und Anwendungen, die diese Einstellungen nun erbt. Dadurch wird die `Web.config` -Datei in ASP.NET 4-Anwendungen, die entweder leer sein oder nur die folgenden Zeilen enthält, die sich für Visual Studio welche Version von Framework Geben Sie die Anwendung vorgesehen ist:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Erweiterbare Ausgabezwischenspeicherung

Seit dem Zeitpunkt, die ASP.NET 1.0 veröffentlicht wurde, hat die Entwickler zum Speichern der generierten Ausgabe von Seiten, Steuerelementen und HTTP-Antworten im Arbeitsspeicher Zwischenspeichern der Ausgabe aktiviert werden. Bei nachfolgenden Anforderungen für Web dienen ASP.NET Inhalt schneller durch Abrufen der Ausgabe aus dem Speicher anstelle von Grund auf neu erstellt. Dieser Ansatz hat jedoch eine Einschränkung, generierter Inhalt immer im Arbeitsspeicher gespeichert werden, und auf Servern, bei denen hohen Datenverkehr auftritt, durch die Zwischenspeicherung der Ausgabe belegte Arbeitsspeicher konkurrieren kann, mit der Speicherbedarf von anderen Teilen einer Webanwendung.

ASP.NET 4 fügt einen Erweiterungspunkt zum Zwischenspeichern der Ausgabe, der Sie eine oder mehrere benutzerdefinierte Ausgabecacheanbieter konfigurieren kann. Ausgabecacheanbieter können alle Speichermechanismus verwenden, um HTML-Inhalt beizubehalten. Dies ermöglicht es zum Erstellen benutzerdefinierter Ausgabecacheanbieter für verschiedene Dauerhaftigkeitsmechanismen, die lokal oder remote Datenträger umfassen können,-Speicher Cloud und verteilter Cache-Engines.

Sie erstellen einen benutzerdefinierten Ausgabecacheanbieter als eine Klasse, die von der neuen abgeleitet *System.Web.Caching.OutputCacheProvider* Typ. Anschließend können Sie konfigurieren den Anbieter in der `Web.config` Datei mit dem neuen *Anbieter* Unterabschnitt der *OutputCache* Element, wie im folgenden Beispiel dargestellt:

[!code-xml[Main](overview/samples/sample2.xml)]

Standardmäßig in ASP.NET 4 alle HTTP-Antworten, gerenderten Seiten und Steuerelemente den in-Memory-Ausgabecache verwenden, wie im vorherigen Beispiel gezeigt, in denen die *DefaultProvider* -Attribut auf AspNetInternalProvider festgelegt ist. Sie können ändern, dass der Ausgabecache-Standardanbieter für eine Webanwendung verwendet wird, unter Angabe eines anderen Anbieter Namens für *DefaultProvider*.

Darüber hinaus können Sie verschiedene Ausgabecache-Anbieter pro Steuerelement "und" pro Anforderung auswählen. Die einfachste Möglichkeit, wählen Sie einen anderen Ausgabecache-Anbieter für andere Web-Benutzersteuerelemente ist, tun Sie dies deklarativ mit dem neuen *ProviderName* Attribut in einer Control-Direktive, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Angeben eines anderen Ausgabecacheanbieters für eine HTTP-Anforderung ist ein wenig mehr Aufwand erforderlich. Statt die deklarative Angabe des Anbieters, überschreiben Sie die neue *GetOuputCacheProviderName* -Methode in der die `Global.asax` Datei, um die für eine bestimmte Anforderung zu verwendenden Anbieter an programmgesteuert anzugeben. Das folgende Beispiel zeigt die dazu erforderliche Vorgehensweise.

[!code-csharp[Main](overview/samples/sample4.cs)]

Durch das Hinzufügen der Ausgabecacheanbieter Erweiterbarkeit auf ASP.NET 4 können Sie jetzt aggressivere und intelligentere ausgabezwischenspeicherung Strategien für Ihre Websites zu verfolgen. Beispielsweise ist es jetzt möglich, die "Top 10" Seiten einer Website im Arbeitsspeicher zwischenspeichern, beim Zwischenspeichern von Seiten, die zu geringerem Datenverkehr auf dem Datenträger zu erhalten. Alternativ können Sie jede variieren nach Kombination für eine gerenderte Seite zwischengespeichert, aber verwenden einen verteilten Cache, sodass die arbeitsspeichernutzung von Front-End-Webservern ausgelagert wird.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>AutoStart-Webanwendungen

Einige Webanwendungen müssen große Mengen von Daten zu laden oder kostenintensive Initialisierung verarbeiten müssen, bevor die erste Anforderung ausführen. In früheren Versionen von ASP.NET wird für diese Situationen musste benutzerdefinierte Ansätze zum "reaktiviert" einer ASP.NET-Anwendung aus, und führen Sie Code zum Initialisieren der während der Entwickeln der *Anwendung\_Load* -Methode in der die `Global.asax` die Datei.

Eine neue Skalierbarkeit-Funktion, die mit dem Namen *Autostart-* , direkt der Adressen dieses Szenario steht bei ASP.NET 4 auf IIS 7.5 auf Windows Server 2008 R2 ausgeführt wird. Die Funktion für automatischen Start bietet ein kontrollierten Ansatzes für einen Anwendungspool gestartet, eine ASP.NET-Anwendung initialisiert werden und dann die HTTP-Anforderungen akzeptiert.

> [!NOTE] 
> 
> IIS Application Warm-Up-Modul für IIS 7.5
> 
> Das IIS-Team hat die erste Betatest-Version des Moduls Application Warm-Up für IIS 7.5 veröffentlicht. Dadurch werden Ihre Anwendungen noch einfacher als zuvor beschrieben in der Aufwärmphase. Benutzerdefinierte statt Code zu schreiben, geben Sie die URLs der Ressourcen ausgeführt werden, bevor Sie die Webanwendung Anforderungen aus dem Netzwerk akzeptiert. Diese Aufwärmphase tritt auf, während des Starts des IIS-Diensts (Wenn Sie den IIS-Anwendungspool als konfiguriert *AlwaysRunning*) und wenn eine IIS-Arbeitsprozess wiederverwendet wird. Während der wiederverwendet weiterhin alte IIS-Arbeitsprozesses Anforderungen werden ausgeführt, bis der Arbeitsprozess für die neu erzeugten, vollständig vorbereitet ist, damit Anwendungen keine Unterbrechungen oder andere Probleme aufgrund von unprimed Caches auftreten. Beachten Sie, dass dieses Modul mit einer Version von ASP.NET, ab Version 2.0 funktioniert.
> 
> Weitere Informationen finden Sie unter [Application Warm-Up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) auf der Website IIS.net. Eine exemplarische Vorgehensweise, das veranschaulicht, wie Sie die Aufwärmphase-Funktion verwenden, finden Sie unter [Einstieg in das IIS 7.5 Anwendungsmodul Aufwärmphase](https://www.iis.net/learn/manage) auf der Website IIS.net.


Um die Funktion für automatischen Start zu verwenden, legt IIS-Administrator ein Anwendungspools in IIS 7.5 automatisch gestartet werden soll, mithilfe der folgenden Konfigurations in der `applicationHost.config` Datei:

[!code-xml[Main](overview/samples/sample5.xml)]

Da ein einzelnen Anwendungspool mehrere Anwendungen enthalten kann, geben Sie einzelne Anwendungen automatisch gestartet werden soll, mithilfe der folgenden Konfigurations in der `applicationHost.config` Datei:

[!code-xml[Main](overview/samples/sample6.xml)]

Wenn ein IIS 7.5-Server kaltgestartet ist oder ein einzelnen Anwendungspool wiederverwendet wird, verwendet IIS 7.5 die Informationen in den `applicationHost.config` Datei, um zu bestimmen, welche Web-Anwendungen müssen automatisch gestartet werden. Für jede Anwendung, die für Autostart markiert ist, sendet IIS 7.5 eine Anforderung an ASP.NET 4 zum Starten der Anwendung in einem Zustand, in dem die Anwendung vorübergehend nicht akzeptiert, HTTP-Anforderungen. Wenn sie in diesem Zustand ist, instanziiert ASP.NET den vom definierten Typ der *ServiceAutoStartProvider* -Attribut (wie im vorherigen Beispiel gezeigt) und Aufrufe an die öffentliche Einstiegspunkt.

Sie erstellen einen verwalteten Autostart-Typ mit der benötigte Einstiegspunkt aus, durch die Implementierung der *IProcessHostPreloadClient* Schnittstelle, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](overview/samples/sample7.cs)]

Nach der Initialisierung Code ausgeführt wird, der *Preload* Methode und die Methode zurückgibt, die ASP.NET-Anwendung ist zum Verarbeiten von Anforderungen bereit.

Durch das Hinzufügen von IIS.5 und ASP.NET 4 automatisch gestartet werden soll haben Sie jetzt einen klar definierten Ansatz für die Durchführung teurer anwendungsinitialisierung vor dem Verarbeiten der ersten HTTP-Anforderung. Beispielsweise können Sie die neue Funktion für automatischen Start eine Anwendung zu initialisieren und einem Load Balancer signalisiert werden, wurde von die Anwendung initialisiert und ist bereit, um HTTP-Datenverkehr zu akzeptieren.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Umleiten dauerhaft auf einer Seite

Es ist üblich, dass in Webanwendungen zum Verschieben von Seiten und anderen Inhalten, um im Laufe der Zeit, was zu einer Ansammlung von veralteten Links in Suchmaschinen führen kann. In ASP.NET können Entwickler normalerweise verarbeitet Anforderungen an die alten URLs mithilfe der *Response.Redirect* Methode, um eine Anforderung an der neuen URL weitergeleitet. Allerdings die *umleiten* -Methode gibt eine HTTP 302 gefunden (temporäre Umleitung)-Antwort, was in einem zusätzlichen HTTP Roundtrip führt, wenn Benutzer versuchen, auf die alten URLs zugreifen.

ASP.NET 4 fügt ein neues *RedirectPermanent* Hilfsmethode, die es einfach macht, Senden von HTTP 301 Permanent verschoben Antworten, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](overview/samples/sample8.cs)]

Suchmaschinen und andere Benutzer-Agents, die dauerhafte umleitungen erkennt speichert die neue URL, die mit dem Inhalt, verknüpft ist, der unnötige Roundtrip vorgenommen, die vom Browser zum temporären umleitungen mehr herstellen müssen.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Verkleinern der Sitzungszustand

ASP.NET bietet zwei Standardoptionen zum Speichern von Sitzungsdaten in einer ganzen Webfarm: Sitzungszustands-Anbieter, der ein Out-of-Process-Sitzungszustands-Server aufruft und eines Sitzungszustands-Anbieters, die Daten in einer Microsoft SQL Server-Datenbank speichert. Da beide Optionen beinhalten das Speichern von Zustandsinformationen, die außerhalb einer Webanwendung Arbeitsprozess muss Sitzungszustand serialisiert werden, bevor sie in den Remotespeicher gesendet wird. Je nachdem wie viele Informationen, die ein Entwickler im Sitzungszustand gespeichert wird, kann die Größe der serialisierten Daten sehr groß.

ASP.NET 4 enthält eine neue Komprimierungsoption für beide Arten der Out-of-Process-Sitzungszustands-Anbieter. Wenn die *CompressionEnabled* Festlegen der Konfigurationsoption, die im folgenden Beispiel gezeigt wird *"true"*, ASP.NET komprimiert (und dekomprimiert) serialisierten des Sitzungszustands mithilfe von .NET Framework  *: System.IO.Compression.GZipStream* Klasse.

[!code-xml[Main](overview/samples/sample9.xml)]

Durch das einfache Hinzufügen des neuen Attributs auf die `Web.config` Anwendungen mit zusätzlichen CPU-Zyklen auf Webservern-Datei kann erhebliche Reduzierung der Größe der serialisierten Sitzungszustandsdaten realisieren.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Erweitern den Bereich der zulässigen URLs

ASP.NET 4 enthält neue Optionen zum Erweitern der Größe des Anwendungs-URLs. Frühere Versionen von ASP.NET eingeschränkte URL Pfadlänge auf 260 Zeichen, die basierend auf dem NTFS-Dateipfad Grenzwert. In ASP.NET 4 haben Sie die Möglichkeit, diesen Grenzwert nach Bedarf für Ihre Anwendungen erhöhen (oder verringert) mit zwei neuen *HttpRuntime* Konfigurationsattribute. Das folgende Beispiel zeigt die neuen Attribute.

[!code-xml[Main](overview/samples/sample10.xml)]

Um längere oder kürzere Pfade (die Teil der URL, die nicht-Protokoll, Servernamen und Abfragezeichenfolge enthält) zu ermöglichen, ändern Sie die *[MaxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* Attribut. Um längere oder kürzere Abfragezeichenfolgen zu ermöglichen, ändern Sie den Wert des der *[MaxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* Attribut.

ASP.NET 4 können auch so konfigurieren Sie die Zeichen, die von der Überprüfung der URL-Zeichen verwendet werden. Wenn ASP.NET ein ungültiges Zeichen in der Pfadteil einer URL findet, lehnt die Anforderung ab und gibt einen HTTP 400-Fehler. In früheren Versionen von ASP.NET wurden die URL-Zeichen-Prüfungen auf einen festen Satz von Zeichen beschränkt. In ASP.NET 4 können Sie anpassen, den Satz gültiger Zeichen, die mit dem neuen *RequestPathInvalidChars* Attribut der *HttpRuntime* Konfigurationselement, wie im folgenden Beispiel dargestellt:

[!code-xml[Main](overview/samples/sample11.xml)]

In der Standardeinstellung die <em>RequestPathInvalidChars</em> Attribut definiert acht Zeichen als ungültig. (In der Zeichenfolge, die zugewiesen ist <em>RequestPathInvalidChars</em> standardmäßig<em>,</em>der kleiner als (&lt;), größer als (&gt;), und das kaufmännische und-Zeichen (&amp;) Zeichen sind codiert werden, da die `Web.config` Datei ist eine XML-Datei.) Sie können die Menge der ungültigen Zeichen anpassen, je nach Bedarf.

> [!NOTE]
> Beachten Sie ASP.NET 4 lehnt URL-Pfade, die Zeichen im ASCII-Bereich von 0 x 00 bis 0x1F, enthalten immer ab, da die ungültige URL-Zeichen sind, wie in RFC 2396 der IETF definiert ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). In Versionen von Windows Server mit IIS 6 oder höher wird der Gerätetreiber von http.sys Protokoll weist automatisch URLs mit diesen Zeichen.


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Erweiterbare Request-Überprüfung

ASP.NET-Anforderungsvalidierung durchsucht Cross-Site-scripting (XSS) Angriffe eingehende HTTP-Anforderungsdaten für Zeichenfolgen, die häufig verwendet werden. Wenn potenzielle XSS-Zeichenfolgen gefunden werden, werden Anforderungsvalidierung kennzeichnet die fehlerverdächtige Zeichenfolge und gibt einen Fehler zurück. Die integrierten Request-Überprüfung zurückgegeben ein Fehler, nur, wenn es feststellt, dass die am häufigsten verwendeten Zeichenfolgen, die in der XSS-Angriffe verwendet. Vorherige Versuche, die XSS-Validierung der Aggressivität hat zu viele falsch positive Ergebnisse. Kunden können jedoch, dass die Anforderung, die umfangreichere wiederherstellungsanforderungen erforderlich ist, oder umgekehrt sollten XSS absichtlich entspannen Sie sich für bestimmte Seiten oder für bestimmte Anforderungstypen wird überprüft.

In ASP.NET 4 wurde die Anforderungsvalidierungsfunktion extensible vorgenommen, damit Sie benutzerdefinierte Anforderungsvalidierung Logik verwenden können. Um die Anforderungsvalidierung zu erweitern, erstellen Sie eine Klasse, die von der neuen abgeleitet *System.Web.Util.RequestValidator* , und konfigurieren Sie die Anwendung (in der *HttpRuntime* Teil der `Web.config`Datei), der benutzerdefinierte Typ zu verwenden. Das folgende Beispiel zeigt, wie Sie eine benutzerdefinierte Anforderungsvalidierung-Klasse konfigurieren:

[!code-xml[Main](overview/samples/sample12.xml)]

Die neue *RequestValidationType* Attribut erfordert eine standard .NET Framework Typ-ID-Zeichenfolge, die die Klasse gibt an, die benutzerdefinierte Anforderungsvalidierung bereitstellt. Bei jeder Anforderung ruft ASP.NET den benutzerdefinierten Typ, um jedes Datenelement eingehende HTTP-Anforderung zu verarbeiten. Die eingehende URL, alle HTTP-Header (Cookies und benutzerdefinierte Header) und den Entitätstext sind alle verfügbaren für die Überprüfung durch eine benutzerdefinierte Anforderung Validation-Klasse, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](overview/samples/sample13.cs)]

In Fällen, in denen Sie nicht, einen Teil der eingehenden HTTP-Daten zu überprüfen möchten, die Anforderungsvalidierung Klasse auch möglich, damit können die Standard-ASP.NET-Anforderungsvalidierung auf, die ausgeführt, indem einfach *Basis. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Objekt Zwischenspeichern und die Objekt-Caching-Erweiterbarkeit

Seit der ersten Version ASP.NET ist eine leistungsstarke Objektcache mit in-Memory-enthalten (*System.Web.Caching.Cache*). Die Cacheimplementierung wurde so beliebt, dass es im nicht-Webanwendungen verwendet wurde. Allerdings ist es ganz einfach für eine Windows Forms oder WPF-Anwendung einen Verweis auf `System.Web.dll` nur auf ASP.NET Objektcache verwenden können.

Zwischenspeichern zur Verfügung stellen möchten für alle Anwendungen, wird .NET Framework 4 eingeführt, eine neue Assembly, einen neuen Namespace, einige Basistypen und eine konkrete Implementierung Zwischenspeichern. Die neue `System.Runtime.Caching.dll` Assembly enthält eine neue Cache-API in der *System.Runtime.Caching* Namespace. Der Namespace enthält zwei Core Sätzen von Klassen:

- Abstrakte Typen, die die Grundlage für die Erstellung aller Arten von benutzerdefinierten Cache-Implementierung bereitstellen.
- Eine Cacheimplementierung konkrete in-Memory-Objekt (das *System.Runtime.Caching.MemoryCache* Klasse).

Die neue *MemoryCache* Klasse ist eng auf dem ASP.NET-Cache modelliert werden soll, und es gibt ein Großteil der internen Cache-Engine-Logik für ASP.NET. Obwohl die Zwischenspeicherung öffentlichen APIs in *System.Runtime.Caching* aktualisiert wurden zur Unterstützung der Entwicklung von benutzerdefinierten Caches, wenn Sie die ASP.NET verwendet haben *Cache* Objekt finden Sie vertraute Konzepten in der neue APIs.

Eine ausführliche Erläuterung der neuen *MemoryCache* Klasse und die Basis-APIs unterstützen würde ein ganzes Dokument erfordern. Im folgende Beispiel erhalten Sie jedoch eine Vorstellung davon, wie der neue Cache-API funktioniert. Im Beispiel wurde für eine Windows Forms-Anwendung, ohne eine Abhängigkeit auf geschrieben `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Erweiterbare HTML, URL und HTTP-Header, die Codierung

In ASP.NET 4 können Sie benutzerdefinierte Codierungsroutinen für die folgenden allgemeinen textcodierung Aufgaben erstellen:

- HTML-Codierung.
- URL-Codierung.
- HTML-attributcodierung.
- Ausgehende HTTP-Header-Codierung.

Sie können einen benutzerdefinierten Encoder erstellen, durch Ableiten von neuen *System.Web.Util.HttpEncoder* Typ und klicken Sie dann ASP.NET wird konfiguriert, mit dem benutzerdefinierten Typ in der *HttpRuntime* Teil der `Web.config` -Datei als Im folgenden Beispiel gezeigt:

[!code-xml[Main](overview/samples/sample15.xml)]

Nachdem Sie ein benutzerdefinierter Encoder konfiguriert wurde, ruft ASP.NET automatisch die benutzerdefinierte codierungsimplementierung nach öffentlichen Methoden der Codierung der *System.Web.HttpUtility* oder *System.Web.HttpServerUtility* Klassen heißen. Dadurch wird ein Teil eines Web-Entwicklungsteams, die einen benutzerdefinierten Encoder erstellen, der implementiert aggressive-zeichencodierung, während die restlichen das Webentwicklungsteam weiterhin die öffentlichen APIs-Codierung verwenden. Durch die zentrale Konfiguration einen benutzerdefinierten Encoder in der *HttpRuntime* Element garantiert, dass alle textcodierung Aufrufe aus öffentlichen APIs Codierung ASP.NET über den benutzerdefinierten Encoder weitergeleitet werden.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Überwachung der Anwendungsleistung für einzelne Anwendungen in ein einzelner Workerprozess

Um die Anzahl von Websites zu erhöhen, die auf einem einzelnen Server gehostet werden können, führen Sie vielen Hostern mehrere ASP.NET-Anwendungen in ein einzelner Workerprozess. Wenn mehrere Anwendungen einen einzelner Workerprozess für freigegebene verwenden, ist es jedoch für Server-Administratoren, um eine einzelne Anwendung zu identifizieren, die Probleme aufgetreten sind schwierig.

ASP.NET 4 nutzt die neue Ressource-monitoring-Funktionen, von der CLR eingeführt. Um diese Funktionalität zu aktivieren, können Sie die folgenden XML-Konfiguration-Ausschnitt zum Hinzufügen der `aspnet.config` Konfigurationsdatei.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Beachten Sie die `aspnet.config` Datei befindet sich in dem Verzeichnis, in denen .NET Framework installiert ist. Es ist nicht die `Web.config` Datei.


Wenn die *AppDomainResourceMonitoring* Feature aktiviert wurde, die zwei neuen Leistungsindikatoren sind in der Leistungskategorie "ASP.NET-Anwendungen" verfügbar: *% Prozessorzeit verwaltet* und  *Verwalteter Speicher verwendet*. Beide dieser Leistungsindikatoren verwenden die neue CLR Anwendungsdomäne Resource Management-Funktion zum Nachverfolgen von Geschätzte CPU-Zeit und die verwaltete speicherauslastung der einzelnen ASP.NET-Anwendungen. Mit ASP.NET 4 haben Administratoren daher nun eine genauere Ansicht in den Ressourcenverbrauch der einzelnen Anwendungen, die in einem einzelnen Worker-Prozess ausgeführt.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Festlegung von Zielversionen

Sie können eine Anwendung erstellen, die eine bestimmte Version von .NET Framework ausgerichtet ist. In ASP.NET 4 ein neues Attribut in der *Kompilierung* Element der `Web.config` Datei können Sie mit der Zielplattform .NET Framework 4 und höher. Wenn Sie explizit die .NET Framework 4 abzielen, und wenn Sie optionale Elemente in einschließen der `Web.config` Datei, z. B. die Einträge für *system.codedom*, diese Elemente müssen korrekt sein, damit die .NET Framework 4. (Wenn Sie die .NET Framework 4 nicht explizit angeben, wird das Zielframework aus der Mangel an einen Eintrag im abgeleitet der `Web.config` Datei.)

Das folgende Beispiel zeigt die Verwendung von der *TargetFramework* -Attribut in der *Kompilierung* Element der `Web.config` Datei.

[!code-xml[Main](overview/samples/sample17.xml)]

Beachten Sie Folgendes für eine bestimmte Zielversion von .NET Framework:

- In einem .NET Framework 4-Anwendungspool das ASP.NET-Buildsystem übernimmt, .NET Framework 4 als Ziel Wenn die `Web.config` Datei enthält keine der *TargetFramework* Attribut oder, wenn die `Web.config` Datei ist nicht vorhanden. (Sie möglicherweise Schreiben von Code, um zu Ihrer Anwendung für die Ausführung unter .NET Framework 4 ändern.)
- Wenn Sie verwenden die *TargetFramework* -Attribut, und, wenn die *system.codeDom* -Element wird definiert, der `Web.config` -Datei, diese Datei muss die richtige Einträge enthalten, für die .NET Framework 4.
- Bei Verwendung der *Aspnet\_Compiler* -Befehls zum Vorkompilieren der Anwendung (z. B. in einer Buildumgebung), müssen Sie die richtige Version von der *Aspnet\_Compiler* der Befehl für das Zielframework. Verwenden Sie den Compiler an, die ausgeliefert wurden mit .NET Framework 2.0 (% WINDIR%\Microsoft.NET\Framework\v2.0.50727), um für die .NET Framework 3.5 und früheren Versionen zu kompilieren. Verwenden Sie den Compiler an, der bereitgestellt wird mit .NET Framework 4, zur Kompilierung von Anwendungen erstellt, mit dem Framework oder höher.
- Zur Laufzeit verwendet der Compiler den neuesten Frameworkassemblys, die auf dem Computer installiert sind (und somit auch im globalen Assemblycache). Wenn später ein Update für das Framework erfolgt (z. B. eine hypothetische Version 4.1 ist installiert), können Features in der neueren Version des Frameworks verwenden, obwohl die *TargetFramework* Attribut auf eine niedrigere Version abzielt. (z. B. 4.0). (Allerdings zur Entwurfszeit in Visual Studio 2010 oder bei Verwendung der *Aspnet\_Compiler* können mithilfe von neueren Features des Frameworks führt dazu, dass Compiler-Fehler).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery eingeschlossene mit Web Forms und MVC

Visual Studio-Vorlagen für Web Forms und MVC enthalten die Open-Source-jQuery-Bibliothek. Wenn Sie eine neue Website oder ein Projekt erstellen, wird ein Ordner "Scripts", enthält die folgenden 3-Dateien erstellt:

- jQuery-1.4.1.js – der lesbare, vollständige es sich um die Version der jQuery-Bibliothek.
- jQuery-14.1.min.js – die minimierte Version von jQuery-Bibliothek.
- jQuery-1.4.1-vsdoc.js – die Datei von Intellisense-Dokumentation für die jQuery-Bibliothek.

Schließen Sie die unminified Version von jQuery, beim Entwickeln einer Anwendung. Enthalten Sie die minimierte Version von jQuery für Produktionsanwendungen.

Folgende Web Forms-Seite wird beispielsweise veranschaulicht, wie Sie jQuery verwenden können, so ändern Sie die Hintergrundfarbe des ASP.NET TextBox-Steuerelemente auf Gelb, wenn sie den Fokus besitzen.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Unterstützung für Content Delivery Network

Das Microsoft Ajax Content Delivery Network (CDN) können Sie einfache Hinzufügen von ASP.NET Ajax und jQuery-Skripts zu Ihren Webanwendungen. Sie können z. B. starten, verwenden die jQuery-Bibliothek einfach durch Hinzufügen einer `<script>` Tag, um die Seite, die auf Ajax.microsoft.com, wie folgt zeigt:

[!code-html[Main](overview/samples/sample19.html)]

Durch das Microsoft Ajax CDN nutzen, können Sie die Leistung der Ajax-Anwendungen erheblich verbessern. Das Microsoft Ajax CDN den Inhalt werden auf Servern, die auf der ganzen Welt zwischengespeichert werden. Darüber hinaus ermöglicht das Microsoft Ajax CDN Browsern Wiederverwenden von zwischengespeicherten JavaScript-Dateien für Websites, die in verschiedenen Domänen befinden.

Das Microsoft Ajax Content Delivery Network unterstützt SSL (HTTPS) an, für den Fall, dass Sie einer Webseite mithilfe des Secure Sockets Layers bereitstellen müssen.

Implementieren Sie einen Fallback aus, wenn das CDN nicht verfügbar ist. Testen Sie das Fallback.

Besuchen Sie die folgende Website, um weitere Informationen über das Microsoft Ajax CDN:

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

ScriptManager ASP.NET unterstützt das Microsoft Ajax CDN. Einfach durch Festlegen einer Eigenschaft, die EnableCdn-Eigenschaft, können Sie alle ASP.NET Framework-JavaScript-Dateien aus dem CDN abrufen:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Nachdem Sie die EnableCdn-Eigenschaft auf den Wert "true" festgelegt, wird von ASP.NET-Framework alle ASP.NET Framework-JavaScript-Dateien aus dem CDN, einschließlich aller JavaScript-Dateien, die für die Überprüfung und UpdatePanel verwendet abgerufen werden. Durch Festlegen dieser Eigenschaft eine kann drastische Auswirkungen auf die Leistung Ihrer Webanwendung haben.

Sie können den CDN-Pfad für Ihre eigenen JavaScript-Dateien mithilfe des WebResource-Attributs festlegen. Die neue CdnPath-Eigenschaft gibt den Pfad für das CDN verwendet, wenn Sie die EnableCdn-Eigenschaft auf den Wert "true" festlegen:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Explizite ScriptManager-Skripts

In der Vergangenheit bei Verwendung der ASP.NET ScriptManger dann mussten Sie zum Laden der gesamten monolithische ASP.NET Ajax-Bibliothek. Durch Nutzen der neuen Eigenschaft ScriptManager.AjaxFrameworkMode, können Sie steuern, genau, welche Komponenten des ASP.NET Ajax-Bibliothek geladen werden, und Laden nur diejenigen Komponenten des ASP.NET Ajax-Bibliothek, die Sie benötigen.

Die ScriptManager.AjaxFrameworkMode-Eigenschaft kann auf die folgenden Werte festgelegt werden:

- Aktiviert – Gibt an, dass das ScriptManager-Steuerelement automatisch die Skriptdatei "MicrosoftAjax.js" enthält eine kombinierte Skriptdatei jedes Kernframeworkskripts (Legacyverhalten) handelt.
- Deaktiviert – Gibt an, dass alle Funktionen der Microsoft Ajax-Skript deaktiviert sind und das ScriptManager-Steuerelement von Skripts nicht automatisch verwiesen wird.
- Explizite – Gibt an, dass explizit Skriptverweise auf einzelne frameworkkernskriptdateien eingeschlossen werden, die für Ihre Seite benötigen, und Sie Verweise auf die Abhängigkeiten enthält, die jede Skriptdatei erforderlich sind.

Z. B. Wenn Sie die AjaxFrameworkMode-Eigenschaft auf den Wert explizit festlegen können Sie die bestimmten ASP.NET Ajax-Komponentenskripts angeben, die Sie benötigen:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web Forms

Web Forms ist seit der Veröffentlichung von ASP.NET 1.0 ein zentrales Feature in ASP.NET. Zahlreiche Verbesserungen wurden in diesem Bereich für ASP.NET 4, einschließlich der folgenden:

- Festlegen von *Meta* Tags.
- Mehr Kontrolle über den Ansichtszustand.
- Einfachere Möglichkeiten zum Arbeiten mit Browserfunktionen.
- Unterstützung für die Verwendung von ASP.NET-Routing mit Web Forms.
- Mehr Kontrolle über die generierten IDs.
- Die Möglichkeit, ausgewählte Zeilen in Datensteuerelementen persistent zu speichern.
- Mehr Kontrolle über gerendertes HTML in der *FormView* und *ListView* Steuerelemente.
- Unterstützung für Datenquellen-Steuerelemente für die Filterfunktion.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Festlegen von Meta-Tags mit dem Page.MetaKeywords-Eigenschaft und Page.MetaDescription-Eigenschaft

ASP.NET 4 fügt zwei Eigenschaften, die *Seite* -Klasse, *"MetaKeywords"* und *"MetaDescription"*. Diese beiden Eigenschaften darstellen, entsprechende *Meta* tags auf Ihrer Seite, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Diese beiden Eigenschaften funktionieren auf dieselbe Weise wie der Seite *Titel* Eigenschaft. Sie gelten folgende Regeln:

1. Treten keine *Meta* -Tags in der *Head* -Element, das den Eigenschaftennamen übereinstimmen (das heißt, benennen = "Keywords" für *Page.MetaKeywords-Eigenschaft* und den Namen = "Description" für  *Page.MetaDescription*, Bedeutung, die diese Eigenschaften nicht festgelegt wurden), wird die *Meta* Tags werden auf der Seite hinzugefügt, wenn er gerendert wird.
2. Wenn es bereits *Meta* Tags mit diesen Namen, diese Eigenschaften dienen, wie das Abrufen und Festlegen von Methoden für den Inhalt, der die vorhandenen Tags.

Sie können diese Eigenschaften festlegen, zur Laufzeit, sodass Sie den Inhalt aus einer Datenbank oder einer anderen Quelle abzurufen, und die Ihnen die legen Sie die Tags dynamisch beschrieben, wie eine bestimmte Seite ist.

Sie können auch Festlegen der *Schlüsselwörter* und *Beschreibung* Eigenschaften in der *@ Page* -Direktive ganz oben in der Web Forms im Markup Seite, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Dies überschreibt die *Meta* tag-Inhalt (sofern vorhanden), die bereits auf der Seite deklariert.

Der Inhalt der Beschreibung *Meta* Tag werden zum Verbessern der Suche nach vorschauberichte im Google auflisten. (Weitere Informationen finden Sie unter [verbessern von Ausschnitten mit einer Metadaten-Beschreibung zur Überarbeitung](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) Blog des Google Webmaster zentralen.) Google und Windows Live Search verwenden Sie nicht den Inhalt der Schlüsselwörter für alle Elemente, aber möglicherweise andere Suchmaschinen. Weitere Informationen finden Sie unter [Meta Schlüsselwörter Rat](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) auf der Search-Engine-Leitfaden-Website.

Diese neuen Eigenschaften sind eine einfache Funktion, aber sie speichern Sie diese manuell hinzufügen oder Schreiben Ihren eigenen Code zum Erstellen der *Meta* Tags.

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Aktivieren der Ansichtszustand für einzelne Steuerelemente

In der Standardeinstellung ist Ansichtszustand auf der Seite mit dem Ergebnis aktiviert, dass jedes Steuerelement auf der Seite möglicherweise Ansichtszustand speichert, auch wenn dies nicht für die Anwendung erforderlich ist. Ansichtszustandsdaten ist im Markup enthalten, dass eine Seite generiert und das Ausmaß der Zeitaufwand erhöht zu eine Seite an den Client senden sowie das zurücksenden. Speichern von weitere Ansichtszustand als erforderlich erledigt wird, kann signifikante Leistungseinbußen möglich als führen. In früheren Versionen von ASP.NET Entwickler konnte Ansichtszustand für einzelne Steuerelemente deaktivieren, um die Größe einer Seite zu reduzieren, aber musste also explizit für einzelne Steuerelemente. In ASP.NET 4-Webserversteuerelemente umfassen eine *ViewStateMode* Eigenschaft, mit dem Sie den Ansichtszustand standardmäßig zu deaktivieren und aktivieren Sie sie nur für die Steuerelemente, die sie auf der Seite zu erfordern.

Die *ViewStateMode* Eigenschaft akzeptiert eine Enumeration, die drei Werte annehmen: *aktiviert*, *deaktiviert*, und *erben*. *Aktiviert* aktiviert den Ansichtszustand für dieses Steuerelement und für alle untergeordneten Steuerelemente, die festgelegt werden, um *erben* oder "nothing" festgelegt haben. *Deaktivierte* deaktiviert Ansichtszustand und *erben* gibt an, dass das Steuerelement verwendet die *ViewStateMode* vom übergeordneten Steuerelement festlegen.

Das folgende Beispiel zeigt die *ViewStateMode* -Eigenschaft funktioniert. Markup und Code für die Steuerelemente in der folgenden Seite enthält die Werte für die *ViewStateMode* Eigenschaft:

[!code-aspx[Main](overview/samples/sample25.aspx)]

Wie Sie sehen können, wird der Code Ansichtszustand für das Steuerelement PlaceHolder1 deaktiviert. Das untergeordnete label1-Steuerelement erbt den Wert dieser Eigenschaft (*erben* ist der Standardwert für *ViewStateMode* für Steuerelemente fest.) und wird daher kein Ansichtszustand gespeichert. Im Steuerelement "Platzhalter2" *ViewStateMode* nastaven NA hodnotu *aktiviert*, sodass label2 diese Eigenschaft erbt, und speichert den Ansichtszustand. Wenn die Seite zuerst geladen wird, die *Text* -Eigenschaft beider *Bezeichnung* Steuerelemente auf die Zeichenfolge "[" dynamicvalue "]" festgelegt ist.

Die Auswirkung dieser Einstellungen ist, dass bei der erstmaligen der Seite laden im Browser die folgende Ausgabe angezeigt wird:

Deaktiviert `: [DynamicValue]`

Aktiviert:`[DynamicValue]`

Nach einem Postback ist jedoch die folgende Ausgabe angezeigt:

Deaktiviert `: [DeclaredValue]`

Aktiviert:`[DynamicValue]`

Das Steuerelement "Label1" (dessen *ViewStateMode* festgelegt ist *deaktiviert*) wurde den Wert, der er im Code, um festgelegt wurde nicht beibehalten. Jedoch steuern, die "Label2" (dessen *ViewStateMode* festgelegt ist *aktiviert*) hat seinen Zustand beibehalten.

Sie können auch festlegen *ViewStateMode* in die *@ Page* Direktive, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample26.aspx)]

Die *Seite* -Klasse ist nur ein anderes Steuerelement; es fungiert als das übergeordnete Steuerelement für alle anderen Steuerelemente auf der Seite. Der Standardwert von *ViewStateMode* ist *aktiviert* für Instanzen von *Seite*. Da die Steuerelemente standardmäßig *erben*, Steuerelemente erben die *aktiviert* Eigenschaftswert, es sei denn, Sie *ViewStateMode* auf der Seite oder das Steuerelement.

Der Wert des der *ViewStateMode* Eigenschaft bestimmt, ob der Ansichtszustand beibehalten wird, nur dann, wenn die *EnableViewState* -Eigenschaftensatz auf *"true"*. Wenn die *EnableViewState* -Eigenschaftensatz auf *"false"*, Ansichtszustand werden nicht beibehalten, wenn *ViewStateMode* nastaven NA hodnotu *aktiviert*.

Eine gute Verwendung für diese Funktion ist mit *ContentPlaceHolder* Steuerelemente in der Masterseiten geben, in dem Sie festlegen können *ViewStateMode* zu *deaktiviert* für den Master Seite, und klicken Sie dann aktivieren einzeln für *ContentPlaceHolder* Ansichtszustand für Steuerelemente, die wiederum Steuerelemente enthalten, die erfordern.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Änderungen an der Browserfunktionen

ASP.NET bestimmt die Funktionen des Browsers, die ein Benutzer verwendet, um Ihre Website zu durchsuchen, indem Sie mit einem Feature *Browserfunktionen*. Webbrowser-Funktionen werden durch dargestellt die *HttpBrowserCapabilities* Objekt (verfügbar gemacht werden, indem die *Request.Browser* Eigenschaft). Beispielsweise können Sie die *HttpBrowserCapabilities* bestimmt, ob der Typ und die Version des aktuellen Browsers unterstützt eine bestimmte Version von JavaScript. Sie können auch die *HttpBrowserCapabilities* bestimmt, ob die Anforderung von einem mobilen Gerät stammt.

Die *HttpBrowserCapabilities* Objekt wird durch eine Reihe von Browserdefinitionsdateien gesteuert. Diese Dateien enthalten Informationen zu den Funktionen der bestimmten Browser an. In ASP.NET 4 haben diese Browserdefinitionsdateien aktualisiert, um Informationen zu kürzlich eingeführten Browsern und Geräten wie z. B. Google Chrome, Forschung in Bewegung BlackBerry-Smartphones und Apple iPhone enthalten.

Die folgende Liste enthält die neuen Browser Definitionsdateien:

- *blackberry.browser*
- *chrome.browser*
- *Default.Browser*
- *firefox.browser*
- *gateway.browser*
- *generic.browser*
- *ie.browser*
- *iemobile.browser*
- *iphone.browser*
- *opera.browser*
- *safari.browser*

#### <a name="using-browser-capabilities-providers"></a>Mithilfe von Anbietern für Browser-Funktionen

In ASP.NET, Version 3.5 Service Pack 1, Sie können definieren, die Funktionen, die einen Browser auf folgende Weise verfügt:

- Auf der Computerebene, Sie erstellen oder Aktualisieren einer `.browser` XML-Datei in den folgenden Ordner:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Nachdem Sie die Browserfunktion definiert haben, führen Sie den folgenden Befehl aus der Visual Studio-Eingabeaufforderung, um die Assembly für die Browser neu zu erstellen, und fügen sie im globalen Assemblycache hinzu:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Für eine einzelne Anwendung, die Sie erstellen eine `.browser` -Datei in der Anwendung `App_Browsers` Ordner.

Diese Ansätze erfordern Sie XML-Dateien zu ändern, und für Computerebene geändert wird, Sie müssen die Anwendung neu starten nach dem Ausführen der Aspnet\_regbrowsers.exe Prozess.

ASP.NET 4 umfasst eine Funktion, um genannte *Browser Funktionen Anbieter*. Wie der Name schon sagt, mit diesem können Sie einen Anbieter zu entwickeln, der wiederum können Sie Ihren eigenen Code können um Browserfunktionen zu bestimmen.

In der Praxis ist Entwickler oft nicht benutzerdefinierte Browserfunktionen definiert. Browser-Dateien sind schwer zu aktualisieren, den Prozess für sie die Aktualisierung recht kompliziert ist und die XML-Syntax für `.browser` -Dateien können zu verwenden, und Definieren komplexer werden. Ist was diesen Prozess vereinfachen würde, gäbe es eine allgemeine Syntax des Browser-Definition oder eine Datenbank, die auf dem neuesten Stand Browserdefinitionen oder sogar ein Webdienst für eine Datenbank enthalten. Der neue Browser Funktionen Anbieter-Funktion ermöglicht diese Szenarien möglich und praktikabel für Drittanbieter-Entwickler.

Es gibt zwei Hauptansätze für die Verwendung des neuen ASP.NET 4 Browser Funktionen Anbieterfeatures: erweitern die ASP.NET Browserfunktionen Funktionalitäten oder vollständig zu ersetzen. In den folgenden Abschnitten werden zuerst beschrieben, wie auf die Funktionalität zu ersetzen, und klicken Sie dann erweitern.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Ersetzen die Funktionen von ASP.NET Browserfunktionalität

Um die ASP.NET Browser Funktionen Definition-Funktionalität vollständig zu ersetzen, gehen Sie folgendermaßen vor:

1. Erstellen Sie eine abgeleitete Anbieterklasse *HttpCapabilitiesProvider* und überschreibt, die die *GetBrowserCapabilities* -Methode, wie im folgenden Beispiel gezeigt: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    Der Code in diesem Beispiel erstellt ein neues *HttpBrowserCapabilities* Objekt, das nur die Funktion, die mit dem Namen Browser, und wenn diese Funktion auf MyCustomBrowser angeben.
2. Registrieren Sie den Anbieter mit der Anwendung. 

    Um einen Anbieter mit einer Anwendung verwenden zu können, müssen Sie Hinzufügen der *Anbieter* -Attribut auf die *BrowserCaps* im Abschnitt der `Web.config` oder `Machine.config` Dateien. (Sie können auch definieren, die Anbieterattribute in einer *Speicherort* -Element für bestimmte Verzeichnisse in der Anwendung, z. B. in einen Ordner für eine spezifische mobile Gerät.) Im folgende Beispiel veranschaulicht die legen Sie die *Anbieter* Attribut in einer Konfigurationsdatei:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Eine weitere Möglichkeit zum Registrieren der neuen Definition des Browser-Funktion ist die Verwendung von Code, wie im folgenden Beispiel gezeigt:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Dieser Code muss ausgeführt, der *Anwendung\_starten* Ereignis die `Global.asax` Datei. Änderungen an der *BrowserCapabilitiesProvider* Klasse auftreten muss, bevor der Code in der Anwendung ausgeführt wird, um sicherzustellen, dass der Cache in einem gültigen Zustand für das aufgelöste bleibt, *HttpCapabilitiesBase* Objekt.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>Zwischenspeichern der HttpBrowserCapabilities-Objekt

Im vorherige Beispiel wurde ein Problem, handelt es sich, dass der Code jedes Mal, die der benutzerdefinierte Anbieter aufgerufen ausgeführt würde, wird um das Abrufen der *HttpBrowserCapabilities* Objekt. Dies kann mehrere Male während der jede Anforderung geschehen. Im Beispiel tut der Code für den Anbieter nicht viel. Wenn der Code in den benutzerdefinierten Anbieter viel Arbeit in führt jedoch bestellen, zum Abrufen der *HttpBrowserCapabilities* Objekt, dies kann die Leistung beeinträchtigen. Um dies zu verhindern, können Sie Zwischenspeichern der *HttpBrowserCapabilities* Objekt. Führen Sie folgende Schritte aus:

1. Erstellen Sie eine abgeleitete Klasse *HttpCapabilitiesProvider*, wie im folgenden Beispiel: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    Im Beispiel im Code wird einen Cacheschlüssel generiert, durch Aufrufen einer benutzerdefinierten BuildCacheKey-Methode, und er ruft die Zeitdauer in Cache durch Aufrufen einer benutzerdefinierten GetCacheTime-Methode. Der Code fügt dann das aufgelöste *HttpBrowserCapabilities* Objekts in den Cache. Das Objekt kann aus dem Cache abgerufen und wiederverwendet werden für nachfolgende Anforderungen an den benutzerdefinierten Anbieter verwenden.
2. Registrieren Sie den Anbieter mit der Anwendung an, wie im vorherigen Verfahren beschrieben.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Erweitern der Funktionalität von ASP.NET-Browser-Funktionen

Im vorherigen Abschnitt beschrieben, wie zum Erstellen eines neuen *HttpBrowserCapabilities* Objekts in ASP.NET 4. Sie können auch Browserfunktionalität Funktionen der ASP.NET erweitern, indem neue Browserdefinitionen Funktionen hinzugefügt, die bereits in ASP.NET sind. Dies ist möglich, ohne die XML-Browser-Definitionen. Das folgende Verfahren zeigt wie.

1. Erstellen Sie eine abgeleitete Klasse *HttpCapabilitiesEvaluator* und, überschreibt die *GetBrowserCapabilities* Methode, wie im folgenden Beispiel gezeigt: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Dieser Code wird zunächst der ASP.NET Funktionen Browserfunktionalität versuchen, den Browser zu identifizieren. Jedoch, wenn kein Browser erkannt wird anhand der Informationen in der Anforderung definiert (d. h., wenn die *Browser* Eigenschaft der *HttpBrowserCapabilities* Object ist die Zeichenfolge "Unknown"), ruft der Code der benutzerdefinierte Anbieter (MyBrowserCapabilitiesEvaluator), um den Browser zu identifizieren.
2. Registrieren Sie den Anbieter mit der Anwendung an, wie im vorherigen Beispiel beschrieben.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Erweitern der Funktionalität der Browser-Funktionen durch Hinzufügen von neuen Funktionen zu vorhandenen Funktionen-Definitionen

Zusätzlich zum Erstellen eines benutzerdefinierten Browser-Definition-Anbieters und zum dynamischen Erstellen von neuen Browserdefinitionen können Sie vorhandene Browserdefinitionen mit zusätzlichen Funktionen erweitern. Dadurch können Sie eine Definition zu verwenden, die was Ihrer Vorstellung nahekommt ist jedoch nur ein paar Funktionen fehlen. Gehen Sie dazu folgendermaßen vor.

1. Erstellen Sie eine abgeleitete Klasse *HttpCapabilitiesEvaluator* und, überschreibt die *GetBrowserCapabilities* Methode, wie im folgenden Beispiel gezeigt: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    Der Beispielcode erweitert die vorhandenen ASP.NET-ANWENDUNGEN *HttpCapabilitiesEvaluator* -Klasse und ruft die *HttpBrowserCapabilities* -Objekt, das die Anforderungsdefinition der aktuellen übereinstimmt, mit dem folgenden Code :

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Der Code kann dann hinzufügen oder ändern eine Funktion für diesen Browser. Es gibt zwei Möglichkeiten, eine neue Browserfunktion anzugeben:

    - Hinzufügen von Schlüssel/Wert-Paar, die *IDictionary* -Objekt, das verfügbar gemacht werden die *Funktionen* Eigenschaft der *HttpCapabilitiesBase* Objekt. Im vorherigen Beispiel ist der Code Fügt eine Funktion, die mit dem Namen Multitouch-Funktionen mit einem Wert von *"true"*.
    - Legen Sie die vorhandene Eigenschaften von den *HttpCapabilitiesBase* Objekt. Im vorherigen Beispiel, mit dem Code wird die *Frames* Eigenschaft *"true"*. Diese Eigenschaft ist einfach einen Accessor für die *IDictionary* -Objekt, das verfügbar gemacht werden die *Funktionen* Eigenschaft. 

        > [!NOTE]
        > Beachten Sie, dieses Modell gilt für eine beliebige Eigenschaft des *HttpBrowserCapabilities*, einschließlich Steuerelementadapter.
2. Registrieren Sie den Anbieter mit der Anwendung an, wie im vorherigen Verfahren beschrieben.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Routing in ASP.NET 4

ASP.NET 4 erweitert die integrierten Unterstützung für die Verwendung von routing mit Webformularen. Routing können, die Sie konfigurieren, dass eine Anwendung zum akzeptieren, von Anforderungs-URLs, die nicht auf physischen Dateien zugeordnet sind. Stattdessen können Sie routing um und zu definieren URLs, die für Benutzer von Bedeutung sind, die mit der Such-Engine Optimization (SEO) für Ihre Anwendung unterstützen kann. Beispielsweise könnte die URL für eine Seite, die Produktkategorien in einer vorhandenen Anwendung angezeigt, wie im folgenden Beispiel aussehen:

[!code-console[Main](overview/samples/sample36.cmd)]

Routing verwenden, können Sie die Anwendung, akzeptieren die folgende URL ein, um die gleichen Informationen zu rendern konfigurieren:

[!code-console[Main](overview/samples/sample37.cmd)]

Routing wurde seit ASP.NET 3.5 SP1 verfügbar. (Ein Beispiel zum routing in ASP.NET 3.5 SP1 verwenden, finden Sie im Eintrag [Routing mit WebForms verwenden](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "Titel dieses Eintrags.") Haacks Blog.) ASP.NET 4 enthält jedoch einige Features, die einfacher zu verwenden, routing, einschließlich der folgenden:

- Die *PageRouteHandler* -Klasse, die eine einfache HTTP-Handler ist, mit denen Sie beim Definieren von Routen. Die Klasse übergibt Daten an die Seite, der die Anforderung weitergeleitet wird.
- Die neuen Eigenschaften *HttpRequest.RequestContext* und *Page.RouteData* (Dies ist ein Proxy für die *HttpRequest.RequestContext.RouteData* Objekt). Diese Eigenschaften erleichtern es, den Zugriff auf Informationen, die aus der Route übergeben wird.
- Die folgenden neuen Ausdrucks-Generatoren, die definiert sind *System.Web.Compilation.RouteUrlExpressionBuilder* und *System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*, die bietet einer einfachen Möglichkeit, eine URL zu erstellen, die ein Routen-URL in ein ASP.NET-Serversteuerelement entsprechen.
- *RouteValue*, die bietet einer einfachen Möglichkeit zum Extrahieren von Informationen aus der *RouteContext* Objekt.
- Die *RouteParameter* -Klasse, die übergeben von Daten, die in enthaltenen erleichtert eine *RouteContext* Objekt, das eine Abfrage für ein Datenquellen-Steuerelement (ähnlich wie [ *FormParameter* ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Routing für Web Forms-Seiten

Das folgende Beispiel zeigt, wie Sie eine Web Forms-Route definieren, mit dem neuen *MapPageRoute* Methode der *Route* Klasse:

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 enthält die *MapPageRoute* Methode. Im folgende Beispiel ist äquivalent zur SearchRoute Definition im vorherigen Beispiel gezeigt, verwendet jedoch die *PageRouteHandler* Klasse.

[!code-csharp[Main](overview/samples/sample39.cs)]

Der Code im Beispiel ordnet die Route auf einer physischen Seite (in der ersten Route zu `~/search.aspx`). Die erste Routendefinition gibt auch an, dass der Parameter, die mit dem Namen Searchterm aus der URL extrahierten und an die Seite übergeben werden soll.

Die *MapPageRoute* -Methode unterstützt die folgenden Überladungen:

- *MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess)*
- *MapPageRoute (Zeichenfolge RouteName, RouteUrl der Zeichenfolge, Zeichenfolge PhysicalFile, "bool" CheckPhysicalUrlAccess, RouteValueDictionary Standardwerte)*
- *MapPageRoute (Zeichenfolge RouteName, RouteUrl der Zeichenfolge, Zeichenfolge PhysicalFile, "bool" CheckPhysicalUrlAccess, RouteValueDictionary Standardwerte, RouteValueDictionary Einschränkungen)*

Die *CheckPhysicalUrlAccess* Parameter gibt an, ob die Route die Sicherheitsberechtigungen für die physische Seite, die weitergeleitet werden überprüft werden soll (in diesem Fall search.aspx) und die Berechtigungen für die eingehende URL (in diesem Fall suchen / {Searchterm}). Wenn der Wert des *CheckPhysicalUrlAccess* ist *"false"*, nur die Berechtigungen der eingehenden URL werden überprüft. Diese Berechtigungen werden definiert, der `Web.config` mithilfe von Einstellungen wie z. B. die folgende Datei:

[!code-xml[Main](overview/samples/sample40.xml)]

In der Beispielkonfiguration wird Zugriff auf die physische Seite `search.aspx` für alle Benutzer mit Ausnahme derjenigen, die in der Rolle "Administrator" sind. Wenn die *CheckPhysicalUrlAccess* Parametersatz zu *"true"* (Dies ist der Standardwert), nur Administratorbenutzer sind auf die URL-/search/ {Searchterm}, zulässig, da die physische Seite search.aspx ist Klicken Sie auf Benutzer in dieser Rolle beschränkt. Wenn *CheckPhysicalUrlAccess* nastaven NA hodnotu *"false"* und die Website konfiguriert ist, wie im vorherigen Beispiel gezeigt, sind alle authentifizierten Benutzer auf die URL-/search/ {Searchterm} zulässig.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Lesen, die Routinginformationen in einer Web Forms-Seite

Im Code der physischen Web Forms-Seite können Sie zugreifen, die Informationen, die Weiterleitung von der URL extrahiert wurde (oder andere Informationen, die ein anderes Objekt hinzugefügt die *RouteData* Objekt) mit zwei neue Eigenschaften:  *HttpRequest.RequestContext* und *Page.RouteData*. (*Page.RouteData* dient als Wrapper für *HttpRequest.RequestContext.RouteData*.) Das folgende Beispiel zeigt, wie Sie mit *Page.RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

Der Code extrahiert den Wert, der für den Suchbegriff-Parameter übergeben wurde, wie weiter oben in der Beispiel-Route definiert. Beachten Sie die folgenden Anforderungs-URL ein:

[!code-console[Main](overview/samples/sample42.cmd)]

Wenn diese Anforderung erstellt wird, wird das Wort "Scott" gerendert werden sollen, in der `search.aspx` Seite.

#### <a name="accessing-routing-information-in-markup"></a>Zugreifen auf die Routinginformationen im Markup

Die im vorherigen Abschnitt beschriebene Methode veranschaulicht das Weiterleiten von Daten im Code in Web Forms-Seite zu erhalten. Sie können auch Ausdrücke im Markup, mit denen Sie Zugriff auf die gleichen Informationen. Ausdrucks-Generatoren sind eine leistungsstarke und elegante Möglichkeit, die mit deklarativen Code zu arbeiten. (Weitere Informationen finden Sie im Eintrag [Express selbst mit benutzerdefinierten Ausdrucks-Generatoren](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) Haacks Blog.)

ASP.NET 4 umfasst zwei neuen Ausdrucks-Generatoren für die Weiterleitung von Web Forms. Das folgende Beispiel zeigt, wie sie verwendet wird.

[!code-aspx[Main](overview/samples/sample43.aspx)]

Im Beispiel die *RouteUrl* Ausdruck wird verwendet, um eine URL definieren, die für einen Routenparameter basiert. Dies erspart Ihnen, vordefinierten Code die vollständige URL in das Markup, und Sie können Sie die URL-Struktur später ändern, ohne dass Änderungen an diesen Link.

Dieses Markup basierend auf der Route, die zuvor definiert haben, wird die folgende URL generiert:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET funktioniert automatisch die richtige Route (d. h. er generiert die richtige URL) basierend auf den Eingabeparametern. Sie können auch ein Routenname einschließen, die im Ausdruck, die Sie eine zu verwendende Route angeben können.

Das folgende Beispiel zeigt, wie Sie mit der *RouteValue* Ausdruck.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Wenn die Seite, die dieses Steuerelement enthält, ausgeführt wird, wird der Wert "Scott" in der Bezeichnung angezeigt.

Die *RouteValue* Ausdruck können sie ganz einfach im Markup Weiterleiten von Daten zu verwenden, und es vermeidet arbeiten mit den komplexeren Page.RouteData["x"] Syntax im Markup.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Weiterleiten von Daten verwenden für Data Source Control Parameters

Die *RouteParameter* Klasse ermöglicht die Angabe Routendaten als Parameterwert für Abfragen in ein Datenquellen-Steuerelement. Es [funktioniert ähnlich wie die](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) Klasse, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample46.aspx)]

In diesem Fall wird der Wert des Searchterm der Route-Parameter verwendet werden, für die @companyname Parameter in der <em>wählen</em> Anweisung.

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Festlegen von Client-IDs

Die neue *"ClientIDMode"* Eigenschaft Adressen lange Zeit ein Problem in ASP.NET nämlich wie Steuerelemente erstellen die *Id* Attribut für Elemente, die sie zu rendern. Kenntnis der *Id* Attribut für gerenderte Elemente ist wichtig, wenn Ihre Anwendung Clientskript enthält, die auf diese Elemente verweist.

Die *Id* -Attribut in HTML, die für Webserver-Steuerelemente gerendert wird, wird basierend auf generiert die *"ClientID"* -Eigenschaft des Steuerelements. Bis ASP.NET 4 wird der Algorithmus zum Generieren der *Id* -Attribut aus dem *"ClientID"* -Eigenschaft wurde auf den Benennungscontainer (sofern vorhanden) mit der ID, und klicken Sie im Fall von wiederholte Steuerelemente (wie in verketten Datensteuerelemente), um ein Präfix und einer sequentiellen Nummer hinzuzufügen. Während dies immer garantiert ist, dass die IDs der Steuerelemente auf der Seite eindeutig sind, hat der Algorithmus im Steuerelement-IDs geführt, die nicht vorhersehbar waren, und wurden daher schwierig zu verweisen, die im Clientskript.

Die neue *"ClientIDMode"* -Eigenschaft können Sie angeben, genauer gesagt, wie die Client-ID für Steuerelemente generiert wird. Sie können festlegen, die *"ClientIDMode"* -Eigenschaft für jedes Steuerelement, einschließlich für die Seite. Mögliche Einstellungen lauten wie folgt:

- *"AutoID"* – Dies entspricht dem der Algorithmus zum Generieren von *"ClientID"* Eigenschaftswerte, die in früheren Versionen von ASP.NET verwendet wurde.
- *Statische* – Dies gibt an, dass die *"ClientID"* Wert wird die ID ohne verketten die IDs der übergeordneten Benennungscontainern identisch sein. Dies kann in Websteuerelementen für Benutzer nützlich sein. Da eine Web-Benutzersteuerelement sich auf verschiedenen Seiten und in anderen Containersteuerelementen sein kann, es kann schwierig sein, das Clientskript für Steuerelemente schreiben, verwenden die *"AutoID"* Algorithmus Da jedoch nicht vorhersehbar, welche die ID-Werte werden .
- *Vorhersagbare* – diese Option ist in erster Linie für die Verwendung in Datensteuerelementen, die sich wiederholenden Vorlagen verwenden. Er verkettet die ID-Eigenschaften von Benennungscontainern des Steuerelements, aber generierten *"ClientID"* Werte werden keine Zeichenfolgen wie "Ctlxxx". Diese Einstellung funktioniert in Verbindung mit der *ClientIDRowSuffix* -Eigenschaft des Steuerelements. Festlegen der *ClientIDRowSuffix* Eigenschaft, um den Namen eines Felds den Wert dieses Felds wird als Suffix verwendet, für die generierte *"ClientID"* Wert. Normalerweise verwenden Sie den primären Schlüssel für einen Datensatz als den *ClientIDRowSuffix* Wert.
- *Erben* – diese Einstellung ist das Standardverhalten für Steuerelemente, wird angegeben, dass das ID-Generierung eines Steuerelements mit seinem übergeordneten Element identisch ist.

Sie können festlegen, die *"ClientIDMode"* Eigenschaft auf Seitenebene. Dadurch werden die standardmäßige *"ClientIDMode"* Wert für alle Steuerelemente in der aktuellen Seite.

Der Standardwert *"ClientIDMode"* Wert auf Seitenebene ist *"AutoID"*, und die standardmäßige *"ClientIDMode"* ist der Wert an die Steuerungsebene *erben*. Daher ist Sie nicht diese Eigenschaft an eine beliebige Stelle im Code festlegen, alle Steuerelemente werden standardmäßig die *"AutoID"* Algorithmus.

Legen Sie den Wert auf Seitenebene im der *@ Page* Richtlinie, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample47.aspx)]

Sie können auch Festlegen der *"ClientIDMode"* Wert in die XML-Konfigurationsdatei auf Computerebene (Computer) oder auf Anwendungsebene. Dadurch werden die standardmäßige *"ClientIDMode"* für alle Steuerelemente auf allen Seiten in der Anwendung festlegen. Wenn Sie den Wert auf der Computerebene festlegen, werden Sie die standardmäßige *"ClientIDMode"* für alle Websites auf diesem Computer festlegen. Das folgende Beispiel zeigt die *"ClientIDMode"* in der Konfigurationsdatei festlegen:

[!code-xml[Main](overview/samples/sample48.xml)]

Wie bereits erwähnt von den Wert des der *"ClientID"* Eigenschaft ergibt sich aus den Namenscontainer für das übergeordnete Element des Steuerelements. In einigen Szenarien, z. B. Wenn Sie Masterseiten, können Steuerelemente mit den IDs wie in den folgenden Schluss gerenderten HTML-Code:

[!code-html[Main](overview/samples/sample49.html)]

Obwohl die *Eingabe* Element im Markup dargestellt (aus eine *Textfeld* Steuerelement) nur zwei Benennen von Containern ist tief auf der Seite (der geschachtelten *ContentPlaceholder* Steuerelemente), aufgrund der Art und Weise, in die master-Seiten verarbeitet werden, ist das Endergebnis eine Steuerelement-ID wie folgt:

[!code-console[Main](overview/samples/sample50.cmd)]

Diese ID ist garantiert eindeutig auf der Seite ist allerdings unnötig lange für die meisten Zwecke. Angenommen Sie, Sie möchten, um die Länge der gerenderten-ID zu verringern und damit Sie besser steuern, wie die ID generiert wird. (Zum Beispiel möchten Sie "Ctlxxx" Präfixe zu vermeiden.) Die einfachste Möglichkeit hierzu ist, durch Festlegen der *"ClientIDMode"* Eigenschaft wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample51.aspx)]

In diesem Beispiel wird die *"ClientIDMode"* -Eigenschaftensatz auf *statische* für das äußerste *NamingPanel* -Element, und legen auf *vorhersagbar* für das innere *NamingControl* Element. Diese Einstellungen führen das folgende Markup (der Rest der Seite und die Masterseite wird verwendet, wie im vorherigen Beispiel identisch sein):

[!code-html[Main](overview/samples/sample52.html)]

Die *statische* Einstellung wirkt sich das Zurücksetzen der Namenhierarchie für alle Steuerelemente in der äußersten *NamingPanel* -Element, und der Eliminierung der *ContentPlaceHolder* und *MasterPage* IDs aus der generierten ID (Die *Namen* -Attribut der gerenderten Elemente ist davon nicht betroffen, damit der normale Funktionalität von ASP.NET für die Ereignisse aufbewahrt werden, den Ansichtszustand und so weiter.) Ein Nebeneffekt der Namenhierarchie zurückgesetzt wird, die selbst wenn Sie das Markup für Verschieben der *NamingPanel* Elemente auf einen anderen *ContentPlaceholder* -Steuerelement, das gerenderte Clientkennungen bleiben unverändert.

> [!NOTE]
> Beachten Sie, dass sie entscheiden, ob Sie sicher, dass das gerenderte Steuerelement-IDs eindeutig sind. Ist dies nicht der Fall ist, können sie Funktionen, die die eindeutigen IDs für einzelne HTML-Elemente, z. B. den Client erfordert unterbrechen *"Document.getElementById"* Funktion.


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Vorhersagbare Client-IDs erstellen in datengebundenen Steuerelementen

Die *"ClientID"* Werte, die generiert werden, für die Steuerelemente in einem datengebundenen Listensteuerelement durch den alten Algorithmus werden kann lang sein und sind nicht wirklich vorhersagbar. Die *"ClientIDMode"* Funktionalität können Sie weitere steuern, über die Verwendung dieser IDs generiert werden.

Das Markup im folgenden Beispiel enthält eine *ListView* Steuerelement:

[!code-aspx[Main](overview/samples/sample53.aspx)]

Im vorherigen Beispiel die *"ClientIDMode"* und *RowClientIDRowSuffix* Eigenschaften werden im Markup festgelegt. Die *ClientIDRowSuffix* Eigenschaft kann nur in datengebundenen Steuerelementen verwendet werden, und ihr Verhalten unterscheidet sich je nachdem, welches Steuerelement Sie verwenden. Die Unterschiede sind diese:

- *GridView* Steuerelement – Sie können in der Datenquelle den Namen einer oder mehreren Spalten angeben, die zur Laufzeit zum Erstellen des Clients IDs kombiniert werden. Wenn Sie festlegen, z. B. *RowClientIDRowSuffix* zu "ProductName" ProductID"," Steuerelement-IDs für gerenderte Elemente, ein Format wie folgt aufweisen werden:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* Steuerelement – Sie können eine einzelne Spalte angeben, in der Datenquelle, die an die Client-ID angefügt wird Wenn Sie festlegen, z. B. *ClientIDRowSuffix* das gerenderte Steuerelement-IDs müssen auf "ProductName", ein ähnliches Format wie folgt:

- [!code-console[Main](overview/samples/sample55.cmd)]

- In diesem Fall wird die nachfolgende 1 von der Produkt-ID des aktuellen Datenelements abgeleitet.

- *Repeater* Steuerelement – dieses Steuerelement unterstützt nicht die *ClientIDRowSuffix* Eigenschaft. In einem *Repeater* -Steuerelement wird der Index der aktuellen Zeile verwendet. Bei Verwendung von "ClientIDMode" = "Predictable" mit einem *Repeater* zu steuern, Client-IDs werden generiert, die das folgende Format aufweisen:

- [!code-console[Main](overview/samples/sample56.cmd)]

- Die nachfolgende 0 ist der Index der aktuellen Zeile.

Die *FormView* und *DetailsView* Steuerelemente werden nicht mehrere Zeilen angezeigt, sodass sie nicht unterstützen die *ClientIDRowSuffix* Eigenschaft.

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Beibehalten von Zeilenauswahl in Datensteuerelementen

Die *GridView* und *ListView* Steuerelemente können Benutzer eine Zeile auswählen. In früheren Versionen von ASP.NET verfügt über den Index der Zeile auf der Seite Auswahl abhängig wurde. Wenn Sie wählen Sie das dritte Element auf der Seite "1", und klicken Sie dann auf Seite 2 wechseln, z. B. das dritte Element auf der Seite ausgewählt.

Persistente Auswahl wurde anfänglich nur in Dynamic Data-Projekte in .NET Framework 3.5 SP1 unterstützt. Wenn dieses Feature aktiviert ist, ist das aktuelle ausgewählte Element in den Datenschlüssel für das Element abhängig. Dies bedeutet, dass wenn Sie wählen Sie die dritte Zeile auf der Seite "1", und wechseln Sie zur Seite 2, "nothing" auf Seite 2. ausgewählt ist. Umstellung zurück auf Seite 1, ist die dritte Zeile immer noch ausgewählt. Persistente Auswahl wird jetzt unterstützt, für die *GridView* und *ListView* Steuerelemente in allen Projekten mit der *EnablePersistedSelection* -Eigenschaft, siehe die folgendermaßen:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Diagrammsteuerelement für ASP.NET

ASP.NET *Diagramm* -Steuerelement erweitert wird, die datenvisualisierung Angebote in .NET Framework. Mithilfe der *Diagramm* -Steuerelement, können Sie ASP.NET-Seiten mit intuitiven und grafisch anspruchsvollen Diagrammen für komplexe statistische oder finanzielle Analysen erstellen. ASP.NET *Diagramm* Steuerelement wurde als Add-on zu der Version von .NET Framework Version 3.5 SP1 eingeführt und ist Bestandteil der .NET Framework 4-Version.

Das Steuerelement enthält die folgenden Funktionen:

- 35 verschiedene Diagrammtypen
- Eine unbegrenzte Anzahl von Diagrammbereichen, Titeln, Legenden und Anmerkungen.
- Ein breites Spektrum darstellungseinstellungen für alle Diagrammelemente.
- 3-d-Unterstützung für die meisten Diagrammtypen.
- Intelligente Bezeichnungen, die automatisch auf Datenpunkte passen.
- Bereichsstreifen, skalierungsunterbrechungen und logarithmische Skalierung.
- Mehr als 50 Formeln für finanzielle und statistische Berechnungen zur Datenanalyse und -transformation
- Einfache Bindung und Bearbeitung von Diagrammdaten.
- Unterstützung für allgemeine Datenformaten, wie Datumsangaben, Uhrzeiten und Währungen.
- Unterstützung für Interaktivität und ereignisgesteuerte Anpassung, einschließlich Clients click-Ereignisse, die mithilfe von Ajax.
- Zustandsverwaltung
- Binäres Streaming

Die folgenden Abbildungen zeigen Beispiele für finanzielle Diagramme, die vom Diagrammsteuerelement für ASP.NET erstellt werden.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Abbildung 2: Beispiele für ASP.NET-Diagramm

Für weitere Beispiele zur Verwendung von ASP.NET Diagrammsteuerelement Beispielcode herunterladen, auf die [Beispielumgebung für Microsoft Chart Controls](https://go.microsoft.com/fwlink/?LinkId=128300) -Seite auf der MSDN-Website. Finden Sie weitere Beispiele für Communityinhalt im der [Chart Control Forum](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Hinzufügen von Chart-Steuerelement zu einer ASP.NET-Seite

Das folgende Beispiel veranschaulicht das Hinzufügen einer *Diagramm* Steuerelement zu einer ASP.NET-Seite mithilfe von Markup. Im Beispiel die *Diagramm* Steuerelement erstellt ein Säulendiagramm für statische Daten an.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Verwenden von 3D-Diagrammen

Die *Diagramm* Steuerelement enthält eine *ChartAreas* -Auflistung, die darf *Diagrammflächen (ChartArea)* Objekte, die Merkmale der Diagrammflächen zu definieren. Verwenden Sie z. B. um 3D für einen Diagrammbereich zu verwenden, die *Area3DStyle* Eigenschaft wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample59.aspx)]

Die folgende Abbildung zeigt ein 3D-Diagramm mit vier weitere Reihen mit dem *Leiste* Diagrammtyp.

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Abbildung 3: 3-d-Balkendiagramm

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Mithilfe von Skalierungsunterbrechungen, logarithmischen Skalierungen

Skalierungsunterbrechungen und logarithmischen Skalierungen sind zwei weitere Möglichkeiten, die Komplexität des Diagramms hinzuzufügen. Diese Features sind spezifisch für jede Achse in einer Diagrammfläche. Um diese Funktionen auf der primären y-Achse eines Diagrammbereichs verwenden zu können, verwenden Sie z. B. die *AxisY.IsLogarithmic* und *ScaleBreakStyle* Eigenschaften in eine *Diagrammflächen (ChartArea)* Objekt. Der folgende Codeausschnitt zeigt, wie Sie skalierungsunterbrechungen in der primären y-Achse zu verwenden.

[!code-aspx[Main](overview/samples/sample60.aspx)]

Die folgende Abbildung zeigt die y-Achse mit skalierungsunterbrechungen aktiviert.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Abbildung 4: Skalierungsunterbrechungen

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtern von Daten mit das QueryExtender-Steuerelement

Eine häufige Aufgabe für Entwickler, die Erstellen von datengesteuerten Webseiten ist zum Filtern von Daten. Diese normalerweise ausgeführt wurde durch das Erstellen von *, in denen* Klauseln in Data source, Steuerelemente. Dieser Ansatz kann kompliziert sein und in einigen Fällen die *, in denen* Syntax ist nicht möglich, die den vollen Funktionsumfang des zugrunde liegenden Datenbank zu nutzen.

Um die Filterung zu erleichtern, ein neues *QueryExtender* Steuerelement wurde in ASP.NET 4 hinzugefügt. Dieses Steuerelement kann hinzugefügt werden *EntityDataSource* oder *LinqDataSource* Steuerelemente, um die von diesen Steuerelementen zurückgegebenen Daten zu filtern. Da die *QueryExtender* Steuerelement LINQ verwendet, die der Filter wird auf dem Datenbankserver angewendet, bevor die Daten auf der Seite gesendet werden, was sehr effizienten Betrieb führt.

Die *QueryExtender* Steuerelement unterstützt eine Vielzahl von Optionen für Filter. In den folgenden Abschnitten werden diese Optionen beschrieben, und geben Sie Beispiele für deren Verwendung.

#### <a name="search"></a>Suchen

Für die Suchoption die *QueryExtender* Steuerelement führt eine Suche in den angegebenen Feldern. Im folgenden Beispiel verwendet das Steuerelement den Text, die in der TextBoxSearch-Steuerelement und sucht nach dessen Inhalt im eingegeben haben, wird die `ProductName` und `Supplier.CompanyName` Spalten in den Daten, die von zurückgegeben werden das *LinqDataSource* -Steuerelement.

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Bereich

Die Range-Option ist vergleichbar mit der Option, aber es gibt ein Paar von Werten, die den Bereich zu definieren. Im folgenden Beispiel die *QueryExtender* steuern, sucht die `UnitPrice` -Spalte in der vom zurückgegebenen Daten die *LinqDataSource* Steuerelement. Der Bereich wird aus der TextBoxFrom und TextBoxTo Steuerelemente auf der Seite gelesen.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

Die Property-Ausdruck-Option können Sie einen Vergleich mit einem Eigenschaftswert zu definieren. Wenn der Ausdruck ergibt *"true"*, die Daten, die untersucht wird zurückgegeben. Im folgenden Beispiel die *QueryExtender* Steuerelement filtert die Daten durch Vergleichen der Daten in die `Discontinued` -Spalte den Wert aus dem CheckBoxDiscontinued-Steuerelement auf der Seite.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Schließlich können Sie angeben, einen benutzerdefinierten Ausdruck für die Verwendung mit der *QueryExtender* Steuerelement. Diese Option können Sie eine Funktion auf der Seite aufrufen, die Logik des benutzerdefinierten Filter definiert. Das folgende Beispiel zeigt, wie Sie deklarativ angeben, einen benutzerdefinierten Ausdruck in der *QueryExtender* Steuerelement.

[!code-aspx[Main](overview/samples/sample64.aspx)]

Das folgende Beispiel zeigt die benutzerdefinierte Funktion, die aufgerufen wird, durch die *QueryExtender* Steuerelement. In diesem Fall anstelle der enthält eine Datenbankabfrage mit einem *, in denen* -Klausel der Code verwendet eine LINQ-Abfrage zum Filtern der Daten.

[!code-csharp[Main](overview/samples/sample65.cs)]

Diese Beispiele zeigen nur einen Ausdruck, der verwendet wird, der *QueryExtender* Steuerelement zu einem Zeitpunkt. Sie können jedoch mehrere Ausdrücke in einschließen der *QueryExtender* Steuerelement.

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>HTML-codierte Codeausdrücke

Einige ASP.NET-Websites (vor allem bei ASP.NET MVC) basieren häufig auf der Verwendung `<%` =  `expression %>` Syntax (oft als "Codeausdrücke" bezeichnet) zum Schreiben von Text in der Antwort. Bei Verwendung von Codeausdrücken ist es einfach vergessen, HTML-Codierung der Text, wenn der Text von Benutzer stammen Eingabe, die es Seiten öffnen XSS (Cross-Site Scripting) Angriffe lassen kann.

ASP.NET 4 führt die folgende neue Syntax für Codeausdrücke ein:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Diese Syntax wird das HTML-Codierung wird standardmäßig beim Schreiben in die Antwort verwendet. Diesen neuen Ausdruck übersetzt effektiv auf Folgendes:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Z. B. &lt;%: für die anforderungskorrelation ["UserInput"]&gt; ausführt, auf dem Wert des HTML-Codierung *anfordern ["UserInput"]*.

Das Ziel dieses Features ist es zu ermöglichen, um alle Instanzen von der alten Syntax mit der neuen Syntax zu ersetzen, damit Sie nicht gezwungen ist, werden bei jedem Schritt entscheiden, welcher Typ verwendet. Es gibt jedoch Fälle, in denen der Text, der die Ausgabe HTML werden soll oder bereits codiert ist, in diesem Fall kann dies zu doppelte Kodierung führen.

Für diese Fälle werden ASP.NET 4 eingeführt, eine neue Schnittstelle, *IHtmlString*, zusammen mit einer konkreten Implementierung *HtmlString*. Instanzen dieser Typen können Sie angeben, dass der zurückgegebene Wert ist bereits ordnungsgemäß codiert (oder anderweitig untersucht) für die Anzeige im HTML-Format und daher der Wert nicht HTML-codierte erneut werden soll. Z. B. Folgendes darf nicht sein (und nicht) HTML-codiert:

[!code-aspx[Main](overview/samples/sample68.aspx)]

ASP.NET MVC 2-Hilfsmethoden wurde aktualisiert und Arbeiten mit dieser neuen Syntax, damit sie nicht sind doppelte codiert, jedoch nur beim Ausführen von ASP.NET 4. Dieser neuen Syntax ist nicht möglich, wenn Sie eine Anwendung mithilfe von ASP.NET 3.5 SP1 ausführen.

Bedenken Sie, dass dies nicht, dass Schutz vor XSS-Angriffe garantiert. Beispielsweise kann HTML, die Attributwerte, die nicht in Anführungszeichen eingeschlossen sind, verwendet der Benutzereingabe enthalten, immer noch anfällig sind. Beachten Sie, dass die Ausgabe des ASP.NET- und ASP.NET MVC-Hilfsprogrammen Attributwerte immer in Anführungszeichen ein, enthält die ist die empfohlene Vorgehensweise.

Ebenso führt diese Syntax nicht aus JavaScript-Codierung, z. B. bei der Erstellung einer JavaScript-Zeichenfolge, die basierend auf Benutzereingaben.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Vorlage Projektänderungen

In früheren Versionen von ASP.NET bei Verwendung von Visual Studio zum Erstellen eines neues Websiteprojekt oder Webanwendungsprojekt, die sich ergebende Projekte enthalten nur eine Seite "default.aspx", den Standardwert `Web.config` -Datei, und die `App_Data` Ordner, wie im folgenden dargestellt. Abbildung:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio unterstützt auch einen leere Website-Projekttyp, der keine Dateien enthält, wie in der folgenden Abbildung gezeigt:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

Das Ergebnis ist, dass für die für Anfänger, nur sehr wenig Anleitung zum Erstellen einer Produktions-Web-Anwendung vorhanden ist. Aus diesem Grund wird ASP.NET 4 eingeführt, drei neue Vorlagen für ein leeres Webprojekt für die Anwendung und jeweils eine für eine Webanwendung und Website-Projekt.

#### <a name="empty-web-application-template"></a>Leere Web-Application-Vorlage

Wie der Name schon sagt, ist die Vorlage der leeren Webanwendung ein reduzierte Webanwendungsprojekt. Sie auswählen klicken Sie im Dialogfeld Neues Projekt in Visual Studio diese Projektvorlage, wie in der folgenden Abbildung gezeigt:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Klicken Sie, um das Bild in voller Größe anzeigen](overview/_static/image8.png))

Wenn Sie eine leere ASP.NET-Webanwendung erstellen, erstellt Visual Studio das ordnerlayout der folgenden:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Dieser Vorgang ähnelt das leere Website Layout aus früheren Versionen von ASP.NET mit einer Ausnahme. In Visual Studio 2010 leere Webanwendung und leere Website-Projekte enthalten die folgenden minimalen `Web.config` -Datei mit Informationen, die von Visual Studio verwendet werden, um das Framework zu identifizieren, die das Projekt abzielt:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Ohne diese *TargetFramework* -Eigenschaft, Visual Studio standardmäßig als Ziel .NET Framework 2.0 um Kompatibilität zu erhalten, wenn Sie ältere Anwendungen zu öffnen.

#### <a name="web-application-and-web-site-project-templates"></a>Webanwendung und Website-Projektvorlagen

Die anderen zwei neue Projektvorlagen, die mit Visual Studio 2010 geliefert werden, enthalten wesentliche Änderungen. Die folgende Abbildung zeigt das Projektlayout, das erstellt wird, wenn Sie ein neues Webanwendungsprojekt erstellen. (Das Layout für ein Websiteprojekt ist nahezu identisch.)

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

Das Projekt enthält eine Reihe von Dateien, die nicht in früheren Versionen erstellt wurden. Darüber hinaus wird das neue Webanwendungsprojekt Basis-Mitgliedschaft-Funktionalität, konfiguriert Sie schnell beim Sichern des Zugriffs auf die neue Anwendung beginnen können. Aufgrund dieser einschließen die `Web.config` Datei für das neue Projekt Einträge, die verwendet werden enthält, um die Mitgliedschaft, Rollen und Profile zu konfigurieren. Das folgende Beispiel zeigt die `Web.config` -Datei für ein neues Webanwendungsprojekt. (In diesem Fall *RoleManager* ist deaktiviert.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Klicken Sie, um das Bild in voller Größe anzeigen](overview/_static/image14.png))

Das Projekt enthält außerdem eine zweite `Web.config` Datei die `Account` Verzeichnis. Die zweite Konfigurationsdatei bietet eine Möglichkeit zum Schützen des Zugriffs auf der Seite ChangePassword.aspx für Benutzer nicht angemeldet ist. Das folgende Beispiel zeigt den Inhalt des zweiten `Web.config` Datei.

![](overview/_static/image15.png)

Die Seiten erstellt standardmäßig in die neuen Projektvorlagen enthalten auch weitere Inhalte als in früheren Versionen. Das Projekt enthält eine Standard-Masterseite und CSS-Datei, und die Standardseite ("default.aspx") ist die Masterseite verwenden standardmäßig konfiguriert. Das Ergebnis ist, dass wenn Sie die Webanwendung oder Website zum ersten Mal ausführen, der Standardwert (Homepage) noch funktionsfähig ist. In der Tat ist es ähnlich wie die Standardseite, die Sie sehen, wenn Sie eine neue MVC-Anwendung zu starten.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Klicken Sie, um das Bild in voller Größe anzeigen](overview/_static/image18.png))

Die Absicht dieser Änderungen auf die Projektvorlagen ist eine Anleitung zum Erstellen einer neuen Webanwendung. Nachdem semantisch richtig sind, strenge 1.0 kompatibel XHTML-Markup und Layout, das mit CSS angegeben wird, stellen die Seiten in den Vorlagen bewährte Methoden zum Erstellen von ASP.NET 4-Webanwendungen dar. Die Standardseiten haben auch eine zweispalten-Layout, die Sie problemlos anpassen können.

Angenommen Sie, dass für eine neue Webanwendung sollen einige der Farben ändern, und fügen Sie Ihr Firmenlogo anstelle der My ASP.NET Application-Logo. Zu diesem Zweck erstellen Sie ein neues Verzeichnis unter `Content` das Logo Image speichern möchten:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Um das Bild auf der Seite hinzuzufügen, öffnen Sie Sie dann die `Site.Master` Datei, suchen, in dem My ASP.NET Application Text definiert ist, und ersetzen es durch ein *Image* Element, dessen *Src* -Attribut auf das neue Logo festgelegt ist Bild, wie im folgenden Beispiel gezeigt:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Klicken Sie, um das Bild in voller Größe anzeigen](overview/_static/image22.png))

Klicken Sie dann können Sie die in der Datei "Site.CSS" wechseln und Ändern von CSS-Klassendefinitionen, um die Hintergrundfarbe der Seite sowie des Headers, zu ändern.

Das Ergebnis dieser Änderungen ist, dass Sie eine benutzerdefinierte Startseite mit sehr geringem Aufwand anzeigen können:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Klicken Sie, um das Bild in voller Größe anzeigen](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>CSS-Verbesserungen

Einer der wichtigen Bereiche der Arbeit in ASP.NET 4 wurde, können Sie das Rendering von HTML, die mit den neuesten HTML-Standards kompatibel ist. Dies schließt Änderungen, wie ASP.NET Webserver-Steuerelemente mit CSS-Formatvorlagen verwenden.

#### <a name="compatibility-setting-for-rendering"></a>Einstellung für Anwendungskompatibilität für das Rendering

Wenn eine Webanwendung oder Website der .NET Framework 4 ausgerichtet ist, wird standardmäßig die *ControlRenderingCompatibilityVersion* Attribut des der *Seiten* Element den Wert "4.0" festgelegt ist. Dieses Element wird in der Computerebene definiert `Web.config` Datei, und wendet Sie standardmäßig für alle ASP.NET 4-Anwendungen:

[!code-xml[Main](overview/samples/sample69.xml)]

Der Wert für *ControlRenderingCompatibility* ist eine Zeichenfolge, wodurch mögliche neue Version Definitionen in zukünftigen Versionen. In der aktuellen Version werden die folgenden Werte für diese Eigenschaft unterstützt:

- "3.5". Diese Einstellung gibt an, Legacyrendering und Markup. Markup gerendert, die von Steuerelementen ist 100-prozentige Abwärtskompatibilität und die Einstellung der *XhtmlConformance* Eigenschaft berücksichtigt.
- "4.0". Wenn die Eigenschaft diese Einstellung hat, gehen Sie ASP.NET Webserver-Steuerelemente:
- Die *XhtmlConformance* Eigenschaft wird immer als "Strict" behandelt. Steuerelemente wird daher XHTML 1.0 Strict-Markup gerendert.
- Nicht-Input-Steuerelemente nicht mehr deaktiviert werden ungültige Formate rendert.
- *Div* Elemente für verborgene Felder haben jetzt ein Format, damit sie von Benutzern erstellten CSS-Regeln nicht beeinträchtigen.
- Menu-Steuerelemente Rendern von Markup, das semantisch richtige und Richtlinien für Eingabehilfen kompatibel ist.
- Steuerelemente zur gültigkeitsprüfung rendern Inlinestile nicht.
- Steuert, die zuvor gerendert Border = "0" (Steuerelemente, die von ASP.NET abgeleitet *Tabelle* -Steuerelement und ASP.NET *Image* Steuerelement) rendern nicht mehr auf dieses Attribut.

#### <a name="disabling-controls"></a>Deaktivieren von Steuerelementen

In ASP.NET 3.5 SP1 und früheren Versionen rendert das Framework die *deaktiviert* -Attribut im HTML-Markup für, deren Steuerelemente *aktiviert* -Eigenschaftensatz auf *"false"*. Jedoch gemäß der HTML 4.01-Spezifikation nur *Eingabe* Elemente sollte dieses Attribut aufweisen.

In ASP.NET 4, legen Sie die *ControlRenderingCompatabilityVersion* Eigenschaft auf "3.5", wie im folgenden Beispiel gezeigt:

[!code-xml[Main](overview/samples/sample70.xml)]

Sie erstellen Markup für ein *Bezeichnung* Steuerelement wie folgt aus, das das Steuerelement deaktiviert:

[!code-aspx[Main](overview/samples/sample71.aspx)]

Die *Bezeichnung* Steuerelement würde folgenden HTML-Code gerendert:

[!code-html[Main](overview/samples/sample72.html)]

Sie können in ASP.NET 4 Festlegen der *ControlRenderingCompatabilityVersion* den Wert "4.0". In diesem Fall wird nur festgelegt, der Rendering *Eingabe* Elemente rendert eine *deaktiviert* -Attribut festlegen, wenn des Steuerelements *aktiviert* -Eigenschaftensatz auf *"false"* . Steuerelemente, die nicht HTML gerendert werden *Eingabe* Elemente gerendert, stattdessen eine *Klasse* Attribut, das eine CSS-Klasse, die Sie verwenden können verweist, um einiges "deaktiviert" für das Steuerelement zu definieren. Z. B. die *Bezeichnung* Steuerelement, dargestellt in das vorherige Beispiel erzeugt das folgende Markup:

[!code-html[Main](overview/samples/sample73.html)]

Der Standardwert für die Klasse, die für dieses Steuerelement angegeben ist "AspNetDisabled". Sie können diesen Standardwert jedoch ändern, durch Festlegen der statischen *DisabledCssClass* statische Eigenschaft von der *WebControl* Klasse. Für Entwickler von Steuerelementen, das Verhalten, verwenden Sie für ein bestimmtes Steuerelement kann auch mit definiert werden die *SupportsDisabledAttribute* Eigenschaft.

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Div-Elemente für ausgeblendete Felder ausblenden

Rendern von ASP.NET 2.0 und höheren Versionen systemspezifische verborgene Felder (z. B. der *ausgeblendeten* Element, das zum Speichern von Informationen zum Ansichtszustand verwendet) in *Div* Element, um mit dem XHTML-Standard entsprechen. Dies kann jedoch ein Problem führen, wenn Sie eine CSS-Regel betroffen sind *Div* Elemente auf einer Seite. Z. B. dadurch eine ein-Pixel-Zeile, die angezeigt werden, auf der Seite ausgeblendet, etwa *Div* Elemente. In ASP.NET 4 *Div* Elemente, die von ASP.NET generierten ausgeblendeten Felder einschließen hinzufügen Verweis auf eine CSS-Klasse wie im folgenden Beispiel gezeigt:

[!code-html[Main](overview/samples/sample74.html)]

Können Sie definieren eine CSS-Klasse, die gilt nur für die *ausgeblendeten* Elemente, die von ASP.NET, wie im folgenden Beispiel gezeigt generiert werden:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Rendern von einem äußeren Tabelle für Steuerelemente mit Vorlagen

Standardmäßig werden die folgenden ASP.NET Web-Server-Steuerelemente, die Vorlagen unterstützen automatisch in der äußeren Tabelle umschlossen, die verwendet wird, um Inlinestile zu übernehmen:

- *FormView*
- *Anmeldung*
- *PasswordRecovery*
- *ChangePassword*
- *Assistenten*
- *CreateUserWizard*

Eine neue Eigenschaft namens *RenderOuterTable* wurde hinzugefügt, um diesen Steuerelementen, mit der äußere Tabelle aus dem Markup entfernt werden können. Betrachten Sie beispielsweise das folgende Beispiel einer *FormView* Steuerelement:

[!code-aspx[Main](overview/samples/sample76.aspx)]

Dieses Markup wird gerendert, auf der Seite, die eine HTML-Tabelle enthält die folgende Ausgabe:

[!code-html[Main](overview/samples/sample77.html)]

Um zu verhindern, dass die Tabelle gerendert wird, Sie können festlegen, die *FormView* des Steuerelements *RenderOuterTable* -Eigenschaft, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample78.aspx)]

Im vorherige Beispiel gerendert wird, die folgende Ausgabe, ohne die *Tabelle*, *tr*, und *td* Elemente:

> Inhalt


Diese Erweiterung kann auf den Inhalt des Steuerelements im Hinblick auf CSS-Format erleichtern, da keine unerwarteten Tags vom Steuerelement gerendert werden.

> [!NOTE]
> Beachten Sie diese Änderung deaktiviert die Unterstützung für die AutoFormat-Funktion im Visual Studio 2010-Designer, da es nicht mehr ist eine *Tabelle* -Element, das Stilattribute hosten kann, die durch die automatische Formatierung-Option generiert werden.


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>ListView-Steuerelement-Verbesserungen

Die *ListView* Steuerelement wurde als einfacher in ASP.NET 4 verwendet. Die frühere Version des Steuerelements erforderlich, dass Sie eine Layoutvorlage aus angeben, die ein Serversteuerelement mit einer bekannten ID enthalten. Das folgende Markup zeigt ein typisches Beispiel zur Verwendung der *ListView* Steuerelement in ASP.NET 3.5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

In ASP.NET 4 die *ListView* Steuerelement erfordert keine Layoutvorlage aus. Im vorherigen Beispiel gezeigte Markup kann mit dem folgenden Markup ersetzt werden:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>CheckBoxList und RadioButtonList-Steuerelement-Verbesserungen

In ASP.NET 3.5 können Sie angeben, Layout für die *"CheckBoxList"* und *RadioButtonList* mithilfe der folgenden beiden Einstellungen:

- *Flow*. Das Steuerelement rendert *umfassen* Elementen, die den Inhalt enthalten.
- *Tabelle*. Das Steuerelement rendert eine *Tabelle* Element, dessen Inhalt enthalten.

Das folgende Beispiel zeigt Markup für jede dieser Steuerelemente.

[!code-aspx[Main](overview/samples/sample81.aspx)]

In der Standardeinstellung rendern die Steuerelemente HTML etwa wie folgt:

[!code-html[Main](overview/samples/sample82.html)]

Da diese Steuerelemente Listen von Elementen, die zum Rendern von HTML semantisch richtigen, enthalten sollten sie ihre Inhalte mit HTML-Liste rendern (*li*) Elemente. Dies erleichtert es für Benutzer, die von Webseiten mit hilfstechnologie lesen und erleichtert die Steuerelemente auf mithilfe von CSS-Format.

In ASP.NET 4 die *"CheckBoxList"* und *RadioButtonList* Steuerelemente unterstützen die folgenden neuen Werte für die *RepeatLayout* Eigenschaft:

- *OrderedList* : der Inhalt wird wiedergegeben, als *li* Elemente innerhalb einer *Ol* Element.
- *UnorderedList* : der Inhalt wird wiedergegeben, als *li* Elemente innerhalb einer *Ul* Element.

Das folgende Beispiel zeigt, wie Sie mit diesen neuen Werten.

[!code-aspx[Main](overview/samples/sample83.aspx)]

Das vorhergehende Markup generiert folgenden HTML-Code:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Beachten Sie, wenn Sie festlegen, *RepeatLayout* zu *OrderedList* oder *UnorderedList*, *RepeatDirection* -Eigenschaft kann nicht mehr verwendet werden und wird Lösen Sie zur Laufzeit eine Ausnahme aus, wenn die Eigenschaft in Ihrem Markup oder Code festgelegt wurde. Die Eigenschaft würde keinen Wert aufweisen, da das visuelle Layout dieser Steuerelemente mithilfe von CSS stattdessen definiert ist.


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Verbesserungen beim rollenkontextmenü-Steuerelement

Bevor Sie ASP.NET 4 die *Menü* Steuerelement gerendert, eine Reihe von HTML-Tabellen. Dies machte es schwieriger, Anwenden von CSS-Stilen außerhalb von Inline-Eigenschaften festlegen und auch mit nicht kompatibel war Barrierefreiheitsstandards einhält.

In ASP.NET 4 rendert das Steuerelement nun HTML mithilfe von semantischer Markupcode, der eine ungeordnete Liste und Listenelemente besteht. Das folgende Beispiel zeigt das Markup in einer ASP.NET-Seite für die *Menü* Steuerelement.

[!code-aspx[Main](overview/samples/sample85.aspx)]

Wenn die Seite gerendert wird, erzeugt das Steuerelement die folgenden HTML-Code (die *Onclick* Code wurde aus Gründen der Übersichtlichkeit weggelassen):

[!code-html[Main](overview/samples/sample86.html)]

Zusätzlich zu den Verbesserungen des Rendering wurde Tastaturnavigation im Menü auf der fokusverwaltung mit verbessert. Wenn die *Menü* Steuerelement erhält den Fokus, können Sie die Pfeiltasten verwenden, um Elemente zu navigieren. Die *Menü* Steuerelement jetzt auch fügt zugänglich rich Internet Applications (ARIA)-Rollen und Attribute der folgenden[Wing der](http://www.w3.org/TR/wai-aria-practices/#menu "Menü ARIA Richtlinien")für verbessert Barrierefreiheit.

Stile für das Menüsteuerelement werden in einen Stilblock am oberen Rand der Seite und nicht mit der gerenderte HTML-Elemente gerendert. Wenn Sie vollständige Kontrolle über den Stil für das Steuerelement nutzen möchten, legen Sie die neue *IncludeStyleBlock* Eigenschaft *"false"*, in diesem Fall der Style-Block nicht ausgegeben wird. Eine Möglichkeit, diese Eigenschaft verwenden, werden die AutoFormat-Funktion in Visual Studio-Designer zu verwenden, um die Darstellung des Menüs festlegen. Sie können dann führen Sie die Seite, den Quellcode der Seite öffnen, und kopieren Sie den gerenderten Style-Block in einer externen CSS-Datei. In Visual Studio rückgängig zu machen und die Formatierung und Set *IncludeStyleBlock* zu *"false"*. Das Ergebnis ist, dass die Darstellung des Menüs mit Formatvorlagen in einem externen Stylesheet definiert ist.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Assistenten und CreateUserWizard-Steuerelemente

ASP.NET *Assistenten* und *CreateUserWizard* Steuerelemente unterstützen Vorlagen, mit denen Sie den HTML-Code zu definieren, die sie zu rendern. (*CreateUserWizard* leitet sich von *Assistenten*.) Das folgende Beispiel zeigt das Markup für eine vollständig auf Vorlagen basierenden *CreateUserWizard* Steuerelement:

[!code-aspx[Main](overview/samples/sample87.aspx)]

Das Steuerelement rendert die HTML etwa wie folgt:

[!code-html[Main](overview/samples/sample88.html)]

In ASP.NET 3.5 SP1, obwohl Sie den Inhalt der Vorlage ändern, können Sie weiterhin verfügen über eingeschränkte Kontrolle über die Ausgabe von der *Assistenten* Steuerelement. Sie können in ASP.NET 4 erstellen eine *LayoutTemplate* Vorlage und fügen Sie *Platzhalter* steuert (unter Verwendung von reservierten Namen), um anzugeben, wie die *Assistenten-Steuerelement* zum Rendern. Das folgende Beispiel zeigt dies:

[!code-aspx[Main](overview/samples/sample89.aspx)]

Das Beispiel enthält die folgenden benannten Platzhalter in der *LayoutTemplate* Element:

- *HeaderPlaceholder* – zur Laufzeit wird dies durch den Inhalt des ersetzt die *HeaderTemplate* Element.
- *SideBarPlaceholder* – zur Laufzeit wird dies durch den Inhalt des ersetzt die *SideBarTemplate* Element.
- *WizardStepPlaceHolder* – zur Laufzeit wird dies durch den Inhalt des ersetzt die *WizardStepTemplate* Element.
- *NavigationPlaceholder* – zur Laufzeit ersetzt diese durch keine Navigation-Vorlagen, die Sie definiert haben.

Das Markup in dem Beispiel für das Verwendung von Platzhaltern wird folgenden HTML-Code (ohne den Inhalt in den Vorlagen definiert) gerendert:

[!code-html[Main](overview/samples/sample90.html)]

Ist der einzige HTML-Code, die jetzt keine benutzerdefinierten ist eine *umfassen* Element. (Wir erwarten, dass in zukünftigen Versionen noch die *umfassen* Element nicht gerendert werden.) Jetzt erhalten Sie vollständige Kontrolle über fast alle Inhalte, die vom generierten der *Assistenten* Steuerelement.

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC wurde im März 2009 als ein Add-on-Framework in ASP.NET 3.5 SP1 eingeführt. Visual Studio 2010 enthält die ASP.NET MVC 2, die neuen Features und Funktionen enthält.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Unterstützung von Bereichen

Bereiche ermöglichen das Group-Controllern und Ansichten in Abschnitte einer großen Anwendung relative isoliert von anderen Abschnitten. Jeder Bereich kann als ein separates ASP.NET MVC-Projekt implementiert werden, die dann von der hauptanwendung verwiesen werden kann. Dies hilft bei der Verwaltung von Komplexität beim Erstellen einer großen Anwendung und mehrere Teams zusammenarbeiten, auf eine einzelne Anwendung vereinfacht.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Validierungsunterstützung von Datenanmerkung Attribut

*"DataAnnotations"* von Attributen können Sie die Validierungslogik zum Anfügen an ein Modell mithilfe von Metadaten von Attributen. *"DataAnnotations"* Attribute in ASP.NET Dynamic Data in ASP.NET 3.5 SP1 eingeführt wurden. Diese Attribute wurden in den Standardmodellbinder integriert und bieten eine Möglichkeit metadatengesteuerte, um Benutzereingaben zu überprüfen.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Vorlagenhelfer

Hilfsvorlagen können Sie automatisch zuordnen bearbeiten und Anzeigen von Vorlagen mit Datentypen. Sie können z. B. eine Hilfsvorlage verwenden, um anzugeben, dass ein Datumsauswahl Element der Benutzeroberfläche automatisch zum gerendert wird eine *System.DateTime* Wert. Dies ist an Feldvorlagen in ASP.NET Dynamic Data vergleichbar.

Die *Html.EditorFor* und *Html.DisplayFor* Helper-Methoden verfügen über integrierte Unterstützung für Rendern von standard-Daten sowie die komplexen Objekten mit mehreren Eigenschaften Typen. Diese auch Rendering anpassen, indem Sie datenanmerkung Attribute wie anwenden lassen *"DisplayName"* und *ScaffoldColumn* auf die *"ViewModel"* Objekt.

Häufig möchten Sie die Ausgabe von UI-Hilfsprogramme weiter anpassen und haben uneingeschränkte Kontrolle über was generiert wird. Die *Html.EditorFor* und *Html.DisplayFor* Helper-Methoden unterstützen dies mit einem Vorlagen-Mechanismus, mit dem Sie die externe Vorlagen, die überschrieben werden können und Kontrolle der gerenderten Ausgabe zu definieren. Die Vorlagen können einzeln für eine Klasse gerendert werden.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dynamic Data

Dynamic Data eingeführt wurde in der .NET Framework 3.5 SP1-Version in der Mitte des Jahres 2008. Dieses Feature bietet zahlreiche Verbesserungen für das Erstellen datengesteuerter Anwendungen, einschließlich der folgenden:

- Ein RAD-Erfahrung für das schnelle Erstellen einer datengesteuerten Website.
- Automatische Validierung, die auf Einschränkungen definiert im Datenmodell basieren.
- Die Möglichkeit, das Markup problemlos zu ändern, der für die Felder im generiert die *GridView* und *DetailsView* Steuerelemente mithilfe von Feldvorlagen, die Teil des Dynamic Data-Projekts sind.

> [!NOTE]
> Weitere Informationen zu beachten, finden Sie unter den [Dynamic Data-Dokumentation](https://msdn.microsoft.com/library/cc488545.aspx) in der MSDN Library.


Für ASP.NET 4 hat Dynamic Data verbessert, um Entwickler noch mehr Optionen zum schnellen Erstellen von datengesteuerten Websites.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Aktivieren dynamische Daten für vorhandene Projekte

Dynamic Data-Features, die in .NET Framework 3.5 SP1 geliefert geschaltet neue Features wie die folgenden:

- Feld zur Verfügung – Geben Sie diese Daten-Typ-basierte Vorlagen für datengebundene Steuerelemente. Feldvorlagen bieten eine einfachere Möglichkeit zum Anpassen der Darstellung von Datensteuerelementen, als die Verwendung der Vorlagenfelder für jedes Feld.
- Validierung – dynamische Daten können Sie die Attribute auf Datenklassen zu verwenden, um die Überprüfung zu häufigen Szenarios wie die erforderlichen Felder, bereichsprüfung oder die Überprüfung, Überprüfung des Typs, Mustervergleich mit regulären Ausdrücken und die benutzerdefinierte Validierung angeben. Validierung wird von der Steuerelemente erzwungen.

Diese Funktionen haben jedoch die folgenden Anforderungen:

- Die Datenzugriffsebene musste auf Entity Framework oder LINQ to SQL basieren.
- Die einzige Datenquellen-Steuerelemente, die für diese Features wurden unterstützt die *EntityDataSource* oder *LinqDataSource* Steuerelemente.
- Die Funktionen erforderlich, ein Webprojekt, die mithilfe von dynamischen Daten oder Dynamic Data Entities-Vorlagen um alle Dateien, die erforderlich waren, zur Unterstützung der Funktion erstellt wurde.

Ein wesentliches Ziel der Dynamic Data-Unterstützung in ASP.NET 4 werden die neue Funktionen von ASP.NET Dynamic Data für ASP.NET-Anwendungen aktivieren. Im folgenden Beispiel wird das Markup für Steuerelemente, die von Dynamic Data-Funktionen in einer vorhandenen Seite nutzen können.

[!code-aspx[Main](overview/samples/sample91.aspx)]

Im Code für die Seite muss der folgende Code hinzugefügt werden, um Dynamic Data-Unterstützung für diese Steuerelemente zu aktivieren:

[!code-csharp[Main](overview/samples/sample92.cs)]

Wenn die *GridView* Steuerelement befindet sich im Bearbeitungsmodus befindet, dynamische Daten automatisch überprüft, ob die eingegebenen Daten das richtige Format ist. Wenn sie nicht der Fall ist, wird eine Fehlermeldung angezeigt.

Diese Funktion noch weitere Vorteile, z. B. Angeben der standardmäßigen Werte für die Einfügemodus. Ohne dynamische Daten, um einen Standardwert für ein Feld zu implementieren muss an Ereignis anfügen, suchen Sie das Steuerelement (mit *FindControl*), und legen Sie dessen Wert. In ASP.NET 4 die *EnableDynamicData* -Aufruf unterstützt einen zweiten Parameter, mit dem Sie die Standardwerte für jedes Feld für das Objekt zu übergeben, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Deklarative DynamicDataManager-Steuerelement-Syntax

Die *DynamicDataManager* Steuerelement wurde verbessert, damit Sie es deklarativ konfigurieren wie bei den meisten Steuerelementen in ASP.NET nicht nur im Code. Das Markup für die *DynamicDataManager* Steuerelement sieht wie im folgenden Beispiel aus:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Dieses Markup ermöglicht das Verhalten dynamischer Daten für das GridView1-Steuerelement, auf das verweist, die *Datensteuerelements* Teil der *DynamicDataManager* Steuerelement.

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Die Vorlagen für Entitäten

Vorlagen bieten eine neue Möglichkeit, um das Layout der Daten anzupassen, ohne dass Ihnen die Erstellung eine benutzerdefinierte Seite. Auf der Seite Vorlagen die *FormView* Steuerelement (statt der *DetailsView* zu steuern, wie in den Seitenvorlagen, die in früheren Versionen von ASP.NET Dynamic Data verwendet) und die *erzeugt wird* Steuerelement zum Rendern von Vorlagen für Entitäten. Dies bietet Ihnen mehr Kontrolle über das Markup, das von Dynamic Data gerendert wird.

Die folgende Liste enthält das neue Projekt Verzeichnisstruktur, das die Vorlagen für Entitäten enthält:

[!code-console[Main](overview/samples/sample95.cmd)]

Die `EntityTemplate` Verzeichnis enthält Vorlagen für Data Model-Objekte anzeigen. In der Standardeinstellung Objekte gerendert werden, indem Sie die `Default.ascx` Vorlage, die Markup enthält, der aussieht wie das Markup erstellt die *DetailsView* Steuerelement, das von Dynamic Data in ASP.NET 3.5 SP1 verwendet. Das folgende Beispiel zeigt das Markup für die `Default.ascx` Steuerelement:

[!code-aspx[Main](overview/samples/sample96.aspx)]

Die Standardvorlagen können bearbeitet werden, um das Aussehen und Verhalten für den gesamten Standort zu ändern. Es sind Vorlagen für die Anzeige, bearbeiten und insert-Vorgänge. Neue Vorlagen können basierend auf den Namen des Objekts hinzugefügt werden, um das Aussehen und Verhalten der nur eine Art von Objekt zu ändern. Beispielsweise können Sie die folgende Vorlage hinzufügen:

[!code-console[Main](overview/samples/sample97.cmd)]

Die Vorlage enthält möglicherweise das folgende Markup:

[!code-aspx[Main](overview/samples/sample98.aspx)]

Die neue Entitätsvorlagen werden auf einer Seite angezeigt, mit dem neuen *erzeugt wird* Steuerelement. Zur Laufzeit wird dieses Steuerelement mit dem Inhalt der Entitätsvorlage ersetzt. Das folgende Markup zeigt die *FormView* steuern, der `Detail.aspx` Seitenvorlage, die die Entitätsvorlage verwendet. Beachten Sie, dass die *erzeugt wird* Element im Markup.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Neue Feldvorlagen für URLs und e-Mail-Adressen

ASP.NET 4 enthält zwei neue integrierte Feldvorlagen, `EmailAddress.ascx` und `Url.ascx`. Diese Vorlagen werden als markierten Felder zum *EmailAddress* oder *Url* mit der *DataType* Attribut. Für *EmailAddress* Objekten, das Feld wird angezeigt, als einen Link, der mit der *Mailto:* Protokoll. Wenn Benutzer auf den Link klicken, öffnet der Benutzer-e-Mail-Client und erstellt eine Skelette-Nachricht. Als typisierte Objekte *Url* werden als gewöhnliche Links angezeigt.

Das folgende Beispiel zeigt, wie Felder markiert werden sollen.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Erstellen von Verknüpfungen mit dem DynamicHyperLink-Steuerelement

Dynamic Data verwendet die neue routing-Funktion, die in der .NET Framework 3.5 SP1, um die URLs zu steuern, die Endbenutzern angezeigt wird, beim Zugriff auf die Website hinzugefügt wurde. Die neue *DynamicHyperLink* Steuerelement erleichtert das Erstellen von Links zu Seiten in einer Dynamic Data-Website. Das folgende Beispiel zeigt, wie Sie mit der *DynamicHyperLink* Steuerelement:

[!code-aspx[Main](overview/samples/sample101.aspx)]

Dieses Markup erstellt einen Link, der auf der Listenseite für zeigt die `Products` Tabelle auf der Grundlage von Routen, die in definierten die `Global.asax` Datei. Das Steuerelement verwendet automatisch den Standardnamen für die Tabelle, dem auf die Seite dynamische Daten basieren.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Unterstützung von Vererbung im Datenmodell

Sowohl die Entity Framework und LINQ to SQL unterstützen die Vererbung in ihren Datenmodellen. Ein Beispiel hierfür ist möglicherweise eine Datenbank mit einer `InsurancePolicy` Tabelle. Er enthält möglicherweise auch `CarPolicy` und `HousePolicy` Tabellen, die die gleichen Felder wie `InsurancePolicy` und dann weitere Felder hinzufügen. Dynamische Daten wurde geändert, um geerbte Objekte in das Datenmodell zu verstehen und Gerüstbau für die geerbten Tabellen unterstützen.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Unterstützung von m: N Beziehungen (nur Entitätsframework)

Das Entity Framework bietet umfangreiche Unterstützung für die m: n Beziehungen zwischen Tabellen, die implementiert wird, indem das Verfügbarmachen der Beziehung wie eine Auflistung in ein *Entität* Objekt. Neue `ManyToMany.ascx` und `ManyToMany_Edit.ascx` Feldvorlagen wurden hinzugefügt, um bieten Unterstützung für die Anzeige und Bearbeitung von Daten, die m: n Beziehung beteiligt sind.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Neue Attribute für die Steuerelementanzeige und Unterstützung von Enumerationen

Die *Displayattribut* hinzugefügt wurde, geben Sie zusätzliche Kontrolle darüber, wie Felder angezeigt werden. Die *"DisplayName"* -Attribut in früheren Versionen von ASP.NET Dynamic Data konnten Sie zum Ändern des Namens, der als Beschriftung für ein Feld verwendet wird. Die neue *Displayattribut* Klasse ermöglicht die Angabe, die weitere Optionen zum Anzeigen eines Felds, wie z. B. die Reihenfolge, in der ein Feld angezeigt wird und ob ein Feld als Filter verwendet wird. Das Attribut bietet auch eine unabhängige Kontrolle über den Namen für die Bezeichnungen in einer *GridView* zu steuern, den Namen eine *DetailsView* steuern, den Hilfetext für das Feld, und das Wasserzeichen verwendet werden, für die Feld (wenn das Feld Texteingabe akzeptiert).

Die *EnumDataTypeAttribute* -Klasse wurde hinzugefügt, mit der Sie die Felder Enumerationen zugeordnet werden können. Wenn Sie dieses Attribut auf ein Feld anwenden, geben Sie einen Enumerationstyp. Dynamic Data verwendet die neue `Enumeration.ascx` Feldvorlage, die zum Erstellen der Benutzeroberfläche zum Anzeigen und Bearbeiten der Enumerationswerte. Die Vorlage ordnet die Werte aus der Datenbank den Namen in der Enumeration.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Verbesserte Unterstützung für Filter

Im Lieferumfang von Dynamic Data-1.0 sind integrierte Filter für boolesche Spalten und Fremdschlüsselspalten. Nicht die Filter können Sie angeben, ob sie angezeigt wurden, oder in welcher Reihenfolge sie angezeigt wurden. Die neue *Displayattribut* Attribut tragen beiden dieser Probleme durch bietet Ihnen die Kontrolle über die, ob eine Spalte als Filter angezeigt wird und in welcher Reihenfolge sie angezeigt wird.

Eine weitere Verbesserung ist, dass Unterstützung für die Filterfunktion re[geschrieben, um die Verwendung der neuen](#0.2__QueryExtender "_QueryExtender") Funktion von Web Forms. Dadurch können Sie Filter erstellen, ohne dass Kenntnisse über das Datenquellen-Steuerelement, dem mit die Filtern verwendet wird. Zusammen mit diesen Erweiterungen haben Filter auch in der Vorlagensteuerelemente aktiviert wurde, sodass Sie neue hinzuzufügen. Schließlich die *Displayattribut* Klasse, die zuvor erwähnten ermöglicht den standarddateilisten-Filter überschrieben werden, in der gleichen Weise *"UIHint"* können Sie die Standardvorlage-Feld für eine Spalte außer Kraft gesetzt werden.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Visual Studio 2010 Verbesserungen bei der Webentwicklung

Webentwicklung in Visual Studio 2010 wurde für mehr CSS-Kompatibilität, höhere Produktivität durch HTML- und Markup-Codeausschnitte und neue dynamischen IntelliSense JavaScript verbessert.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Verbesserte CSS-Kompatibilität

Visual Web Developer-Designer in Visual Studio 2010 wurde aktualisiert, um die Einhaltung von CSS 2.1-Standards zu verbessern. Der Designer besser behält die Integrität der HTML-Quelle und ist stabiler als in früheren Versionen von Visual Studio. Hinter den Kulissen wurde Verbesserungen an der Architektur auch vorgenommen, die besser auf zukünftige Erweiterungen Rendering, Layout und wartbarkeit aktivieren.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>HTML und JavaScript-Ausschnitte

In der HTML-Editor werden automatisch vervollständigt IntelliSense Tag-Namen. Das Feature für die IntelliSense-Codeausschnitte werden automatisch vervollständigt ganze Tags und vieles mehr. In Visual Studio 2010 werden IntelliSense-Codeausschnitte für JavaScript, zusammen mit c# und Visual Basic, die in früheren Versionen von Visual Studio unterstützt wurden.

Visual Studio 2010 umfasst mehr als 200 Codeausschnitte, mit denen Sie die automatische Vervollständigung allgemeine ASP.NET- und HTML-Tags, einschließlich der erforderlichen Attribute (z. B. Runat = "Server") und allgemeine Attribute, die spezifisch für ein Tag (z. B. *ID*,  *DataSourceID*, *ControlToValidate*, und *Text*).

Können Sie weitere Codeausschnitte herunterladen, oder Sie können Ihre eigenen Ausschnitte, die kapseln die Blöcke des Markups, mit denen Sie oder Ihr Team für allgemeine Aufgaben schreiben.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>JavaScript-IntelliSense-Verbesserungen

In Visual 2010 wurde JavaScript IntelliSense überarbeitet, um eine noch bessere Bearbeitung zu ermöglichen. IntelliSense erkennt nun die Objekte, die dynamisch durch Methoden wie z. B. generiert wurden *RegisterNamespace* und durch ähnliche Techniken, die von anderen JavaScript-Frameworks verwendet. Leistung wurde verbessert, um große Bibliotheken des Skripts zu analysieren und mit wenigen oder gar keinen Verzögerungen bei der Verarbeitung von IntelliSense angezeigt. Kompatibilität wurde zur Unterstützung der fast alle Drittanbieter-Bibliotheken und zur Unterstützung von Codierungsstile erheblich erhöht. Dokumentationskommentare werden jetzt analysiert, während der Eingabe und werden von IntelliSense sofort genutzt werden.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Bereitstellung von Webanwendungen mit Visual Studio 2010

Wenn Entwickler eine Webanwendung bereitstellen, finden sie häufig, dass sie z. B. folgende auftreten:

- Die Bereitstellung auf einem freigegebenen hosting-Site erfordert Technologien wie FTP, der langsam sein kann. Darüber hinaus müssen Sie Aufgaben wie das Ausführen von SQL-Skripts zum Konfigurieren einer Datenbank manuell ausführen, und müssen Sie IIS-Einstellungen, z. B. das Konfigurieren eines virtuellen Verzeichnisordner als Anwendung ändern.
- Administratoren müssen häufig in einer unternehmensumgebung, zusätzlich zur Bereitstellung der Web-Anwendungsdateien Konfigurationsdateien für ASP.NET und IIS-Einstellungen ändern. Datenbankadministratoren müssen eine Reihe von SQL-Skripts zum Abrufen der dienstanwendungs-Datenbank ausführen ausführen. Dieser Art von Installation Arbeitsaufwand, oft Stunden dauern, und muss sorgfältig dokumentiert werden.

Visual Studio 2010 enthält die Technologien, die diese Probleme beheben und, mit denen Sie die nahtlose Bereitstellung von Webanwendungen. Einer dieser Technologien ist das IIS-Webbereitstellungstool (MsDeploy.exe).

Web-Bereitstellungsfunktionen in Visual Studio 2010 umfassen die folgenden Hauptbereiche:

- Web-paketerstellung
- Web.config-transformation
- Die datenbankbereitstellung
- One-Click-Veröffentlichung für Webanwendungen

Die folgenden Abschnitte enthalten Details zu diesen Features.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Web-Paketerstellung

Visual Studio 2010 verwendet MSDeploy-Tool, um eine komprimierte Datei (.zip) für Ihre Anwendung zu erstellen als bezeichnet ein *Webpaket*. Die Paketdatei enthält Metadaten über Ihre Anwendung und dem folgenden Inhalt:

- IIS-Einstellungen, die Einstellungen des Anwendungspools, seiteneinstellungen für Fehler- und usw. enthält.
- Die eigentlichen Web-Inhalt, der Webseiten, Benutzersteuerelemente, statische Inhalte (Bilder und HTML-Dateien) und So weiter enthält.
- SQL Server-Datenbank-Schemas und Daten.
- Sicherheitszertifikate, Komponenten für die Installation im GAC, registrierungseinstellungen und So weiter.

Ein Webpaket auf einem beliebigen Server kopiert, und klicken Sie dann mit IIS-Manager manuell installiert werden kann. Alternativ kann zur automatischen Bereitstellung des Pakets mithilfe von Befehlen oder mithilfe des bereitstellungs-APIs installiert werden.

Visual Studio 2010 bietet integrierte MSBuild-Aufgaben und Ziele zum Erstellen von Web-Paketen. Weitere Informationen finden Sie unter [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) auf der MSDN-Website und [10 + 20 Gründe, warum sollten Sie ein Webpaket erstellen](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) Vishal Joshi Blog.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Web.config-Transformation

Für die Bereitstellung von Webanwendungen, Visual Studio 2010 bietet [transformieren XDT (XML Document)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), dies ist ein Feature, das Sie transformieren kann eine `Web.config` -Datei von entwicklungseinstellungen zu Produktionseinstellungen für die. Transformation angegeben sind in der Transformationsdateien, die mit dem Namen `web.debug.config`, `web.release.config`und so weiter. (Die Namen dieser Dateien mit MSBuild-Konfigurationen übereinstimmen.) Eine Transformationsdatei enthält nur die Änderungen, die Sie benötigen, um eine bereitgestellte `Web.config` Datei. Sie können die Änderungen mithilfe von einfachen Syntax angeben.

Das folgende Beispiel zeigt einen Teil einer `web.release.config` -Datei, die erstellt werden, kann für die Bereitstellung von der Releasekonfiguration. Das Schlüsselwort "Replace" im Beispiel gibt an, dass während der Bereitstellung der *"ConnectionString"* Knoten in der `Web.config` Datei mit den Werten, die im Beispiel aufgeführt sind, ersetzt werden.

[!code-xml[Main](overview/samples/sample102.xml)]

Weitere Informationen finden Sie unter [Syntax der Web.config-Transformation für die Bereitstellung von Webanwendungsprojekten](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) auf der MSDN <a id="0.2_a"> </a> Website und[Webbereitstellung: Web.Config-Transformation](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)Vishal Joshi Blog.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Die Datenbankbereitstellung

Ein Bereitstellungspaket für die Visual Studio 2010 kann Abhängigkeiten von SQL Server-Datenbanken enthalten. Im Rahmen der Paketdefinition geben Sie die Verbindungszeichenfolge für die Quelldatenbank. Bei der Erstellung des Pakets wird Visual Studio 2010 erstellt SQL-Skripts für das Datenbankschema und optional für die Daten, und fügt diese dem Paket anschließend hinzu. Sie können auch benutzerdefinierte SQL-Skripts bereitstellen und angeben die Reihenfolge, in der sie auf dem Server ausgeführt werden sollen. Zum Zeitpunkt der Bereitstellung Geben Sie eine Verbindungszeichenfolge, die für den Zielserver geeignet ist; Der Bereitstellungsprozess verwendet dann diese Verbindungszeichenfolge zum Ausführen der Skripts, die das Datenbankschema zu erstellen, und fügen Sie die Daten.

Darüber hinaus mit nur einem Klick veröffentlichen, Sie können Ihre Datenbank direkt zu veröffentlichen, wenn die Anwendung, an einem Remotestandort freigegebenen hosting veröffentlicht wird-Bereitstellung konfigurieren. Weitere Informationen finden Sie unter [Vorgehensweise: Bereitstellen einer Datenbank mit einem Webanwendungsprojekt](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) auf der MSDN-Website und [Datenbankbereitstellung in Visual Studio 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) Vishal Joshi Blog.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>One-Click-Veröffentlichung für Webanwendungen

Visual Studio 2010 können Sie den IIS-remote-Management-Dienst zu verwenden, um das Veröffentlichen einer Webanwendung auf einem Remoteserver. Sie können ein Veröffentlichungsprofil für Ihre hosting-Konto oder zum Testen der Server oder Stagingserver erstellen. Jedes Profil kann die entsprechende Anmeldeinformationen sicher speichern. Anschließend können Sie auf das Ziel bereitstellen Server mit nur einem Klick über das Internet einmalklick-Symbolleiste zu veröffentlichen. Mit Visual Studio 2010 können Sie auch veröffentlichen, mit der MSBuild-Befehlszeile. Dadurch können Sie das Konfigurieren der Team-Build-Umgebung zur Veröffentlichung in einem continuous Integration-Modell enthalten.

Weitere Informationen finden Sie unter [Vorgehensweise: Bereitstellen eines Webanwendungsprojekts-Anwendung mit One-Click-Veröffentlichung und das Web Deploy](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) auf der MSDN-Website und [1-Click-Veröffentlichung mit Visual Studio 2010 Web](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) Vishal Joshi Blog. Videopräsentationen zur Bereitstellung von Webanwendungen in Visual Studio 2010 finden Sie unter [VS 2010 für Web Developer Previews](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) Vishal Joshi Blog.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Ressourcen

Die folgenden Websites bieten weitere Informationen zu ASP.NET 4 und Visual Studio 2010.

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) – die offizielle Dokumentation für ASP.NET 4 auf der MSDN-Website.
- [https://www.asp.net/](https://www.asp.net/) – Die ASP.NET Website des Teams.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) und [ASP.NET Dynamic Data Content Map](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) – Online-Ressourcen auf der ASP.NET-Website-Team und in der offiziellen Dokumentation für ASP.NET Dynamic Data.
- [https://www.asp.net/ajax/](../../ajax/index.md) – Die wichtigsten Webressource für ASP.NET Ajax-Entwicklung.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) – Die Visual Web Developer-Team-Blog, die Informationen zu den Funktionen in Visual Studio 2010 enthält.
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) – die Haupt-Web-Ressource für die Preview-Versionen von ASP.NET.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Haftungsausschluss

Dies ist ein vorläufiges Dokument, das vor dem kommerziellen Release der beschriebenen Software ggf. erheblich geändert wird.

Die Informationen in diesem Dokument repräsentieren den aktuellen Standpunkt der Microsoft Corporation zu den erörterten Problemen zum Veröffentlichungstermin. Da Microsoft auf das Ändern von Marktlagen reagieren muss, ist das Dokument keinesfalls als Verpflichtung von Microsoft zu interpretieren, und Microsoft kann die Genauigkeit der Informationen nicht über den Zeitpunkt der Veröffentlichung hinaus garantieren.

Dieses Whitepaper dient ausschließlich Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHE, STILLSCHWEIGENDE ODER AUS GESETZ ERWACHSENDE GARANTIE IN BEZUG AUF DIE INFORMATIONEN IN DIESEM DOKUMENT.

Die Benutzer/innen sind verpflichtet, sich an alle anwendbaren Urheberrechtsgesetze zu halten. Unabhängig von der Anwendbarkeit der entsprechenden Urheberrechtsgesetze darf ohne ausdrückliche schriftliche Erlaubnis der Microsoft Corporation kein Teil dieses Dokuments für irgendwelche Zwecke vervielfältigt oder in einem Datenempfangssystem gespeichert oder darin eingelesen werden, unabhängig davon, auf welche Art und Weise oder mit welchen Mitteln (elektronisch, mechanisch, durch Fotokopieren, Aufzeichnen usw.) dies geschieht.

Microsoft Corporation kann Inhaber von Patenten oder Patentanträgen, Marken, Urheberrechten oder anderem geistigen Eigentum sein, die den Inhalt dieses Dokuments betreffen. Die Bereitstellung dieses Dokuments gewährt keinerlei Lizenzrechte an diesen Patenten, Marken, Urheberrechten oder anderem geistigen Eigentum, es sei denn, dies wurde ausdrücklich durch einen schriftlichen Lizenzvertrag mit der Microsoft Corporation vereinbart.

Sofern nicht anders angegeben, die Firmen, Organisationen, Produkte, Domänennamen, e-Mail-Adressen, Logos, Personen, Orte und Ereignisse in diesen Beispielen sind frei erfunden, und mit jeder Firmen, Unternehmen, Produkt, Domänennamen, e-Mail-Adresse Adresse ","-Logo "," Person "," Ort "oder" Ereignis richtet sich an, oder rein zufällig.

© 2009 Microsoft Corporation. Alle Rechte vorbehalten.

Microsoft und Windows sind eingetragene Marken oder Marken der Microsoft Corporation in den USA und/oder anderen Ländern.

Die in diesem Dokument erwähnten Namen von Unternehmen und Produkten können Marken der jeweiligen Eigentümer sein.
