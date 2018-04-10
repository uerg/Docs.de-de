---
uid: whitepapers/aspnet4/overview
title: ASP.NET 4 und Visual Studio 2010 Web Development Overview | Microsoft Docs
author: rick-anderson
description: Dieses Dokument bietet einen Überblick über viele neue Features für ASP.NET, die in.NET Framework 4 und Visual Studio 2010 enthalten sind.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 6ce52c387ff835eda46bc1882b8b974889e2d4af
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/10/2018
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>ASP.NET 4 und Visual Studio 2010 Web Development (Übersicht)
====================
> Dieses Dokument bietet einen Überblick über viele neue Features für ASP.NET, die in.NET Framework 4 und Visual Studio 2010 enthalten sind.
> 
> [In diesem Whitepaper herunterladen](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**Inhalt**

**[Core Services](#0.2__Toc253429238 "_Toc253429238")**  
[Datei "Web.config", die Umgestaltung](#0.2__Toc253429239 "_Toc253429239")  
[Erweiterbare Ausgabezwischenspeicherung](#0.2__Toc253429240 "_Toc253429240")  
[AutoStart-Webanwendungen](#0.2__Toc253429241 "_Toc253429241")  
[Umleiten von einer Seite dauerhaft](#0.2__Toc253429242 "_Toc253429242")  
[Verkleinern der Sitzungsstatus](#0.2__Toc253429243 "_Toc253429243")  
[Erweitern den Bereich der zulässigen URLs](#0.2__Toc253429244 "_Toc253429244")  
[Erweiterbare Anforderungsvalidierung](#0.2__Toc253429245 "_Toc253429245")  
[Caching-Objekt und die Objekt-Caching Erweiterbarkeit](#0.2__Toc253429246 "_Toc253429246")  
[Erweiterbare HTML, URL und HTTP-Header, die Codierung](#0.2__Toc253429247 "_Toc253429247")  
[Überwachung der Anwendungsleistung für einzelne Anwendungen in ein einzelner Workerprozess](#0.2__Toc253429248 "_Toc253429248")  
[Multi-Targeting](#0.2__Toc253429249 "_Toc253429249")

**[Ajax](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery eingeschlossene mit Web Forms und MVC](#0.2__Toc253429251 "_Toc253429251")  
[Content Delivery Network-Support](#0.2__Toc253429252 "_Toc253429252")  
[Explizite Skripts in ScriptManager](#0.2__Toc253429253 "_Toc253429253")

**[Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Festlegen von Meta-Tags mit dem Page.MetaKeywords-Eigenschaft und die Page.MetaDescription Eigenschaften](#0.2__Toc253429257 "_Toc253429257")  
[Aktivieren des Ansichtszustands für einzelne Steuerelemente](#0.2__Toc253429258 "_Toc253429258")  
[Änderungen an Browserfunktionen](#0.2__Toc253429259 "_Toc253429259")  
[Routing in ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Festlegen von Client-IDs](#0.2__Toc253429261 "_Toc253429261")  
[Beibehalten von Zeilenauswahl in Datensteuerelementen](#0.2__Toc253429262 "_Toc253429262")  
[ASP.NET Diagrammsteuerelement](#0.2__Toc253429263 "_Toc253429263")  
[Filtern von Daten mit dem Steuerelement QueryExtender](#0.2__Toc253429264 "_Toc253429264")  
[HTML-codierte Codeausdrücke](#0.2__Toc253429265 "_Toc253429265")  
[Projekt Vorlagenänderungen](#0.2__Toc253429266 "_Toc253429266")  
[CSS-Verbesserungen](#0.2__Toc253429267 "_Toc253429267")  
[Ausblenden von Div-Elemente um ausgeblendete Felder](#0.2__Toc253429268 "_Toc253429268")  
[Rendern einer äußeren Tabelle für Vorlagenbasierte Steuerelemente](#0.2__Toc253429269 "_Toc253429269")  
[ListView-Steuerelement Verbesserungen](#0.2__Toc253429270 "_Toc253429270")  
[CheckBoxList und RadioButtonList-Steuerelement Erweiterungen](#0.2__Toc253429271 "_Toc253429271")  
[Menü-Steuerelement Verbesserungen](#0.2__Toc253429272 "_Toc253429272")  
[Assistenten und CreateUserWizard Steuerelemente 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Unterstützung von Bereichen](#0.2__Toc253429275 "_Toc253429275")  
[Datenanmerkung Attribut Validierungsunterstützung](#0.2__Toc253429276 "_Toc253429276")  
[Mit Hilfsvorlagen](#0.2__Toc253429277 "_Toc253429277")

**[Dynamic Data](#0.2__Toc253429278 "_Toc253429278")**  
[Aktivieren dynamische Daten für vorhandene Projekte](#0.2__Toc253429279 "_Toc253429279")  
[Deklarative DynamicDataManager Steuerelement Syntax](#0.2__Toc253429280 "_Toc253429280")  
[Entitätsvorlagen](#0.2__Toc253429281 "_Toc253429281")  
[Neues Feldvorlagen für URLs und e-Mail-Adressen](#0.2__Toc253429282 "_Toc253429282")  
[Zum Erstellen von Verknüpfungen mit dem Steuerelement DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Unterstützung von Vererbung im Datenmodell](#0.2__Toc253429284 "_Toc253429284")  
[Unterstützung für viele-zu-viele-Beziehungen (nur Entity Framework)](#0.2__Toc253429285 "_Toc253429285")  
[Neue Attribute zu Anzeige- und Unterstützung von Enumerationen](#0.2__Toc253429286 "_Toc253429286")  
[Erweiterte Unterstützung für Filter](#0.2__Toc253429287 "_Toc253429287")

**[Visual Studio 2010 Web Development Verbesserungen](#0.2__Toc253429288 "_Toc253429288")**  
[Verbesserte CSS-Kompatibilität](#0.2__Toc253429289 "_Toc253429289")  
[HTML und JavaScript-Ausschnitte](#0.2__Toc253429290 "_Toc253429290")  
[JavaScript-IntelliSense-Erweiterungen](#0.2__Toc253429291 "_Toc253429291")

**[Bereitstellung der Anwendung mit Visual Studio 2010 Web](#0.2__Toc253429292 "_Toc253429292")**  
[Web packen](#0.2__Toc253429293 "_Toc253429293")  
[Web.config-Transformation](#0.2__Toc253429294 "_Toc253429294")  
[Datenbankbereitstellung](#0.2__Toc253429295 "_Toc253429295")  
[One-Click-Veröffentlichung für Webanwendungen](#0.2__Toc253429296 "_Toc253429296")  
[Resources](#0.2__Toc253429297 "_Toc253429297")

**[Haftungsausschluss](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Basisdienste

ASP.NET 4 enthält eine Reihe von Funktionen, die ASP.NET-Kerndienste, z. B. Zwischenspeichern der Ausgabe und sitzungszustandspeicherung zu verbessern.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Datei "Web.config", die Umgestaltung

Die `Web.config` Datei, die die Konfiguration enthält, für eine Webanwendung über die letzten einige Versionen von .NET Framework deutlich geworden ist, wie neue Features, z. B. Ajax hinzugefügt wurden routing und Integration in IIS 7. Dies machte es schwieriger zu konfigurieren oder neue Web-Anwendungen ohne ein Tool wie Visual Studio starten. In hergestellt haben NET Framework 4, die wichtigsten Konfigurationselemente an verschoben wurden die `machine.config` Datei- und Anwendungen, die diese Einstellungen nun erben. Dies ermöglicht die `Web.config` Datei in ASP.NET 4-Anwendungen entweder leer sein oder nur die folgenden Zeilen enthalten, die für Visual Studio angeben, welche Version des Frameworks die Anwendung vorgesehen ist:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Erweiterbare Ausgabezwischenspeicherung

Seit dem Zeitpunkt, die ASP.NET 1.0 veröffentlicht wurde, hat die Entwickler zum Speichern der generierten Ausgabe der Seiten, Steuerelemente und HTTP-Antworten im Arbeitsspeicher Zwischenspeichern der Ausgabe aktiviert werden. Bei nachfolgenden Anforderungen für Web dienen ASP.NET Inhalt schneller durch Abrufen der generierten Ausgabe aus dem Arbeitsspeicher anstelle von Grund auf neu erstellt. Dieser Ansatz hat jedoch eine Einschränkung – generierter Inhalt immer im Arbeitsspeicher gespeichert werden, und auf Servern, auf denen hohem Datenverkehr auftritt, kann die arbeitsspeichernutzung durch das Zwischenspeichern der Ausgabe mit der Speicherbedarf von anderen Teilen einer Webanwendung konkurrieren.

ASP.NET 4 fügt einen Erweiterungspunkt Ausgabecaches, der Ihnen ermöglicht, eine oder mehrere benutzerdefinierte Ausgabecacheanbieter konfigurieren. Ausgabecacheanbieter können alle Speichermechanismus verwenden, um HTML-Inhalt beizubehalten. Dadurch wird es möglich, benutzerdefinierte Ausgabecacheanbieter für verschiedene Persistenz Mechanismen, die lokal oder remote-Datenträger enthalten kann,-Speicher Cloud zu erstellen und verteilten Cache-Engines.

Sie erstellen eine benutzerdefinierte Ausgabecacheanbieter als eine Klasse, die aus dem neuen abgeleitet *System.Web.Caching.OutputCacheProvider* Typ. Anschließend konfigurieren Sie den Anbieter in der `Web.config` Datei mithilfe des neuen *Anbieter* -Unterabschnitt des der *OutputCache* Element, wie im folgenden Beispiel dargestellt:

[!code-xml[Main](overview/samples/sample2.xml)]

In ASP.NET 4 alle HTTP-Antworten standardmäßig gerenderten Seiten und Steuerelemente den in-Memory-Ausgabecache verwenden, wie im vorherigen Beispiel gezeigt, in dem die *DefaultProvider* -Attribut auf AspNetInternalProvider festgelegt ist. Sie können den Ausgabecache-Standardanbieter verwendet für eine Webanwendung unter Angabe eines anderen Anbieter Namens für ändern *DefaultProvider*.

Darüber hinaus können Sie verschiedene Ausgabecacheanbieter pro Steuerelement und pro Anforderung auswählen. Die einfachste Möglichkeit, einen anderen Ausgabecache-Anbieter für andere Web-Benutzersteuerelemente auswählen möchten dies deklarativ mit dem neuen ist *ProviderName* Attribut in einer Direktive Steuerung, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Angeben eines anderen Ausgabecacheanbieters für eine HTTP-Anforderung ist ein wenig mehr Arbeit erforderlich. Anstelle von deklarativ angeben des Anbieters, überschreiben Sie die neue *GetOuputCacheProviderName* Methode in der `Global.asax` Datei an, dass programmgesteuert die für eine bestimmte Anforderung zu verwendenden Anbieter an. Das folgende Beispiel zeigt die dazu erforderliche Vorgehensweise.

[!code-csharp[Main](overview/samples/sample4.cs)]

Durch das Hinzufügen von Ausgabecacheanbieter Erweiterbarkeit für ASP.NET 4 können Sie jetzt aggressivere und intelligentere Zwischenspeichern der Ausgabe von Strategien für die Websites kann jedoch auch. Beispielsweise ist es jetzt möglich, die "Top 10" Seiten einer Website im Arbeitsspeicher zwischenspeichern, beim Zwischenspeichern von Seiten, die niedrigere Datenverkehr auf dem Datenträger erhalten. Alternativ können Sie jede Kombination variieren von für eine gerenderte Seite zwischengespeichert, aber einen verteilten Cache verwenden, so, dass die arbeitsspeichernutzung von Front-End-Webservern ausgelagert wird.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>AutoStart-Webanwendungen

Einige Webanwendungen müssen große Mengen an Daten geladen oder kostenintensive Initialisierung Verarbeitung vor der Bereitstellung der ersten Anforderung ausführen. In früheren Versionen von ASP.NET für diese Situationen mussten benutzerdefinierte Ansätze zur "reaktiviert" einer ASP.NET-Anwendung und führen dann Initialisierungscode beim Entwickeln der *Anwendung\_laden* Methode in der `Global.asax` die Datei.

Eine neue Skalierbarkeit-Funktion mit dem Namen *Autostart-* , direkt Adressen dieses Szenario verfügbare ist bei ASP.NET 4 auf IIS 7.5 auf Windows Server 2008 R2 ausgeführt wird. Die Autostart-Funktion bietet ein kontrollierten Ansatzes für einen Anwendungspool gestartet, eine ASP.NET-Anwendung initialisieren und dann HTTP-Anforderungen akzeptiert.

> [!NOTE] 
> 
> IIS Application Warm-Up-Modul für IIS 7.5
> 
> Das IIS-Team hat die erste Beta-Testversion des Moduls Aufwärmphase Anwendung für IIS 7.5 veröffentlicht. Dadurch wird der Aufwärmphase, Ihre Anwendungen noch einfacher als zuvor beschrieben. Anstelle von benutzerdefinierten Code schreiben, geben Sie die URLs der Ressourcen ausgeführt werden, bevor die Webanwendung aus dem Netzwerk-Anforderungen akzeptiert. Diese Aufwärmphase tritt auf, während des Starts des IIS-Diensts (Wenn Sie den IIS-Anwendungspool als konfiguriert *AlwaysRunning*) und wenn eine IIS-Arbeitsprozess wiederverwendet wird. Während der Wiederverwendung weiterhin die alte IIS-Arbeitsprozess Anforderungen ausgeführt werden, bis die neu gestartete Arbeitsprozess gestartet wurde, vollständig warm ist, sodass Anwendungen keine Unterbrechungen oder andere Probleme aufgrund von unprimed Caches auftreten. Beachten Sie, dass dieses Modul mit einer Version von ASP.NET ab Version 2.0 funktioniert.
> 
> Weitere Informationen finden Sie unter [Application Warm-Up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) auf der Website IIS.NET an. Eine exemplarische Vorgehensweise, die veranschaulicht, wie Sie die Aufwärmphase-Funktion verwenden, finden Sie unter [erste Schritte mit der IIS 7.5 Anwendungsmodul Aufwärmphase](https://www.iis.net/learn/manage) auf der Website IIS.NET an.


Zum Verwenden der Autostart-Funktion legt ein IIS-Administrator ein Anwendungspools in IIS 7.5 automatisch gestartet werden soll, mithilfe der folgenden Konfigurations in der `applicationHost.config` Datei:

[!code-xml[Main](overview/samples/sample5.xml)]

Da ein einzelnen Anwendungspool mehrere Anwendungen enthalten kann, geben Sie einzelne Anwendungen automatisch gestartet werden soll, mithilfe der folgenden Konfigurations in der `applicationHost.config` Datei:

[!code-xml[Main](overview/samples/sample6.xml)]

Wenn ein IIS 7.5-Server kalt gestartet wird oder ein einzelnen Anwendungspool wiederverwendet wird, verwendet IIS 7.5 die Informationen in den `applicationHost.config` Datei, um zu bestimmen, welche Web Applications müssen automatisch gestartet werden. Für jede Anwendung, die für Autostart-markiert ist, sendet IIS 7.5 eine Anforderung an ASP.NET 4, um die Anwendung in einem Zustand zu starten, während dessen die Anwendung vorübergehend nicht akzeptiert, HTTP-Anforderungen. Wird in diesem Status, instanziiert ASP.NET den Typ definiert, indem Sie die *ServiceAutoStartProvider* Attribut (wie im vorherigen Beispiel gezeigt) und Aufrufe an die öffentliche Einstiegspunkt.

Sie erstellen einen verwalteten Autostart-Typ mit der benötigte Einstiegspunkt durch Implementieren der *IProcessHostPreloadClient* -Schnittstelle ein, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](overview/samples/sample7.cs)]

Nach der Initialisierung Code ausführt, der *Preload* Methode und die Methode zurückgibt, die ASP.NET-Anwendung zur Verarbeitung von Anforderungen bereit ist.

Durch das Hinzufügen des automatischen Starts IIS.5 und ASP.NET 4 haben Sie jetzt einen klar definierten Ansatz zum Ausführen von teure anwendungsinitialisierung vor dem Verarbeiten der ersten HTTP-Anforderung. Beispielsweise können Sie die neue Funktion für automatischen Start initialisieren Sie eine Anwendung, und klicken Sie dann einem Lastenausgleich also, dass die Anwendung initialisiert wird und bereit zur Annahme von HTTP-Datenverkehr wurde zu signalisieren.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Dauerhaftes Umleiten einer Seite

Es ist üblich in Webanwendungen zu Seiten und anderen Inhalten, um im Laufe der Zeit zu verschieben, d. h., eine Ansammlung von veralteten Verknüpfungen in Suchmaschinen führen kann. In ASP.NET Entwickler haben bisher behandelt Anforderungen an die alten URLs mithilfe der *Response.Redirect* Methode, um eine Anforderung an der neuen URL weiterleiten. Allerdings die *umleiten* -Methode gibt eine Antwort HTTP 302 gefunden (temporäre Umleitung), was in einen zusätzlichen HTTP-Roundtrip führt, wenn Benutzer versuchen, die alten URLs zugreifen.

ASP.NET 4 fügt ein neues *RedirectPermanent* Hilfsmethode, die Problem HTTP 301 erleichtert permanent verschoben Antworten, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](overview/samples/sample8.cs)]

Suchmaschinen und andere Benutzer-Agents, die permanente leitet erkennen speichert die neue URL, die der Inhalt zugeordnet ist, unnötige Roundtrips vorgenommen, die vom Browser für temporäre Umleitung mehr herstellen müssen.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>Verkleinern des Sitzungsstatus

ASP.NET bietet zwei Standardoptionen für das Speichern des Sitzungszustands in einer Webfarm: einen Sitzungsstatusanbieter, der eine Out-of-Process-Sitzungszustands Server aufruft und einen Sitzungsstatusanbieter, die Daten in einer Microsoft SQL Server-Datenbank speichert. Da beide Optionen beinhalten das Speichern von Zustandsinformationen, die außerhalb einer Webanwendung Arbeitsprozess muss Sitzungsstatus serialisiert werden, bevor sie in den Remotespeicher gesendet wird. Je nach Umfang der Informationen ein Entwickler im Sitzungszustand gespeichert, kann die Größe der serialisierten Daten sehr groß.

ASP.NET 4 stellt eine neue Komprimierungsoption für beide Arten von Out-of-Process-Sitzungszustand Anbietern bereit. Wenn die *CompressionEnabled* Konfigurationsoption, die im folgenden Beispiel gezeigt wird festgelegt, um *"true"*, ASP.NET komprimiert (und dekomprimiert) serialisierten Sitzungszustand mithilfe des .NET Frameworks  *System.IO.Compression.GZipStream* Klasse.

[!code-xml[Main](overview/samples/sample9.xml)]

Durch das einfache Hinzufügen des neuen Attributs auf die `Web.config` Anwendungen mit zusätzlichen CPU-Zyklen auf Webservern-Datei kann erhebliche Reduzierung der Größe der serialisierten Sitzungszustandsdaten mit sich bringen.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Erweitern den Bereich der zulässigen URLs

ASP.NET 4 führt neue Möglichkeiten beim Erweitern der Größe des Anwendungs-URLs. Frühere Versionen von ASP.NET eingeschränkte URL Pfadlänge auf 260 Zeichen, die basierend auf dem NTFS-Dateipfad Grenzwert. In ASP.NET 4, haben Sie die Möglichkeit zum Vergrößern (oder verkleinern) dieses Limit nach Bedarf für Ihre Anwendungen mit zwei neuen *HttpRuntime* Konfigurationsattribute. Das folgende Beispiel zeigt diese neue Attribute.

[!code-xml[Main](overview/samples/sample10.xml)]

Um längere oder kürzere Pfade (der Teil der URL, die keine Protokoll, Servernamen und Abfragezeichenfolge enthalten) zu ermöglichen, Ändern der *[MaxUrlLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxurllength.aspx)* Attribut. Um längere oder kürzere Abfragezeichenfolgen zu ermöglichen, ändern Sie den Wert des der *[MaxQueryStringLength](https://msdn.microsoft.com/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)* Attribut.

ASP.NET 4 können Sie so konfigurieren Sie die Zeichen, die durch die Überprüfung der URL-Zeichen verwendet werden. Wenn ASP.NET ein ungültiges Zeichen in den Pfadteil einer URL findet, lehnt die Anforderung ab und gibt einen HTTP 400-Fehler aus. In früheren Versionen von ASP.NET waren die URL-Zeichen-Überprüfungen auf einen festen Satz von Zeichen beschränkt. In ASP.NET 4 können Sie den Satz von Zeichen, die mit dem neuen anpassen *RequestPathInvalidChars* Attribut von der *HttpRuntime* Configuration-Element, wie im folgenden Beispiel gezeigt:

[!code-xml[Main](overview/samples/sample11.xml)]

Wird standardmäßig die <em>RequestPathInvalidChars</em> Attribut definiert acht Zeichen als ungültig. (In der Zeichenfolge, die zugewiesen <em>RequestPathInvalidChars</em> standardmäßig<em>,</em>die kleiner als (&lt;), größer als (&gt;), und das kaufmännische und-Zeichen (&amp;) Zeichen sind codiert werden, da die `Web.config` Datei ist eine XML-Datei.) Sie können die Menge der ungültigen Zeichen bei Bedarf anpassen.

> [!NOTE]
> Beachten Sie ASP.NET 4 immer URL-Pfade, die Zeichen im ASCII-Bereich von 0 x 00 bis 0x1F, abgelehnt, da diese ungültige URL-Zeichen sind, gemäß Definition in RFC 2396 der IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). In Versionen von Windows Server ausführen, die IIS 6 oder höher der http.sys-Protokoll-Gerätetreiber automatisch abgelehnt URLs mit diesen Zeichen.


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Erweiterbare Anforderungsüberprüfung

ASP.NET-Anforderungsvalidierung sucht siteübergreifendem Skripting (XSS)-Angriffe eingehende HTTP-Anforderungsdaten für Zeichenfolgen, die häufig verwendet werden. Wenn potenzielle XSS-Zeichenfolgen gefunden werden, Anforderungsvalidierung die Zeichenfolge als fehlerverdächtig kennzeichnet, und gibt einen Fehler zurück. Die integrierten Anforderungsvalidierung gibt einen Fehler zurück, nur, wenn es feststellt, dass die am häufigsten verwendeten Zeichenfolgen im XSS-Angriffen verwendet. Vorherige Wiederholungsversuchen an die XSS-Validierung aggressiver führte zu viele falsch positive Ergebnisse. Kunden können jedoch, dass die anforderungsüberprüfung, das aggressiver oder umgekehrt, absichtlich XSS lockern möchten möglicherweise für bestimmte Seiten oder für bestimmte Anforderungstypen überprüft.

In ASP.NET 4 hat die Anforderung Überprüfungsfunktion erweiterbare wurde, damit Sie benutzerdefinierte Anforderungsvalidierung Logik verwenden können. Um Anforderungsvalidierung zu erweitern, erstellen Sie eine Klasse, die aus dem neuen abgeleitet *System.Web.Util.RequestValidator* Typ, und konfigurieren Sie die Anwendung (in der *HttpRuntime* Teil der `Web.config`Datei), der benutzerdefinierte Typ zu verwenden. Das folgende Beispiel zeigt, wie zum Konfigurieren einer benutzerdefinierten Anforderungsvalidierung-Klasse:

[!code-xml[Main](overview/samples/sample12.xml)]

Die neue *RequestValidationType* Attribut erfordert eine standardmäßige .NET Framework Typ-Bezeichnerzeichenfolge folgt, der die Klasse angibt, der benutzerdefinierte Anforderungsvalidierung bereitstellt. Für jede Anforderung ruft ASP.NET den benutzerdefinierten Typ zum Verarbeiten der einzelnen Bestandteile des eingehenden HTTP-Anforderungsdaten verwendet werden. Die eingehende URL, alle HTTP-Header (Cookies und benutzerdefinierte Header) und den Entitätstext sind alle verfügbaren für die Überprüfung durch eine benutzerdefinierte Anforderung Validierungsklasse wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](overview/samples/sample13.cs)]

Für Fälle, in denen Sie nicht, einen Teil der eingehenden HTTP-Daten zu überprüfen möchten, kann die Anforderungsvalidierung Klasse können Sie die ASP.NET standardüberprüfung Anforderung ausführen, indem Sie einfach aufrufen ausweichen *Basis. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Caching-Objekt und die Objekt-Caching-Erweiterbarkeit

Seit der ersten Veröffentlichung ASP.NET ist einen leistungsstarke im Speicher enthaltenes Objektcache enthalten (*System.Web.Caching.Cache*). Der Cacheimplementierung wurde so beliebt, dass er in nicht-Webanwendungen verwendet wurde. Es ist jedoch für eine Windows Forms- oder WPF-Anwendung einen Verweis auf umständliche `System.Web.dll` nur auf den ASP.NET-Cache-Objekt verwenden können.

Zum Zwischenspeichern verfügbar machen für alle Anwendungen, werden .NET Framework 4 eingeführt, eine neue Assembly, einen neuen Namespace, einige Basistypen und eine konkrete Implementierung Zwischenspeichern. Die neue `System.Runtime.Caching.dll` Assembly enthält eine neue Cache-API in der *System.Runtime.Caching* Namespace. Der Namespace enthält zwei Core Sätze von Klassen:

- Abstrakte Typen, die die Grundlage für das Erstellen von jede Art von benutzerdefinierten Cacheimplementierung bereitstellen.
- Eine konkrete im Speicher enthaltenes Objekt Cacheimplementierung (die *System.Runtime.Caching.MemoryCache* Klasse).

Die neue *MemoryCache* Klasse ist eng in der ASP.NET-Cache modelliert und, die ein Großteil der Logik der internen Cache-Modul mit ASP.NET geteilt. Obwohl der öffentlichen caching-APIs in *System.Runtime.Caching* wurde aktualisiert, damit die Entwicklung von benutzerdefinierten Caches unterstützen bei Verwendung von ASP.NET *Cache* Objekt finden Sie im vertraute Konzepten der neue APIs.

Eine ausführliche Diskussion der neuen *MemoryCache* Klasse und Basis-APIs unterstützen, müsste ein ganzes Dokument. Das folgende Beispiel bietet Ihnen jedoch einen Überblick über die Funktionsweise der neue Cache-API. Im Beispiel Schreibvorgangs für eine Windows Forms-Anwendung, ohne eine Abhängigkeit auf `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>Erweiterbare HTML, URL und HTTP-Header, die Codierung

In ASP.NET 4 können Sie benutzerdefinierte Codierung Routinen für die folgenden allgemeinen textcodierung Aufgaben erstellen:

- HTML-Codierung.
- URL-Codierung.
- HTML-attributcodierung.
- Ausgehende HTTP-Header-Codierung.

Sie können einen benutzerdefinierten Encoder erstellen, durch Ableiten von neuen *System.Web.Util.HttpEncoder* Typ und ASP.NET konfigurieren, verwenden Sie den benutzerdefinierten Typ in der *HttpRuntime* Teil der `Web.config` -Datei als Im folgenden Beispiel gezeigt:

[!code-xml[Main](overview/samples/sample15.xml)]

Nachdem Sie ein benutzerdefinierter Encoder konfiguriert wurde, ruft ASP.NET automatisch die benutzerdefinierte codierungsimplementierung wenn öffentliche Methoden der Codierung der *System.Web.HttpUtility* oder *System.Web.HttpServerUtility* Klassen heißen. Auf diese Weise wird ein Teil eines Entwicklungsteams Web einen benutzerdefinierten Encoder erstellen, der implementiert aggressiven zeichencodierung, während der Rest des Entwicklungsteams Web weiterhin öffentlichen APIs Codierung ASP.NET verwenden können. Durch die zentrale Konfiguration eines benutzerdefinierten Encoders in die *HttpRuntime* -Element, wird sichergestellt, dass alle textcodierung Aufrufe aus der öffentlichen ASP.NET Codierung APIs über den benutzerdefinierten Encoder weitergeleitet werden.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Überwachung der Anwendungsleistung für einzelne Anwendungen in ein einzelner Workerprozess

Erhöhen Sie die Anzahl der Websites, die auf einem einzelnen Server gehostet werden können, führen zahlreiche Hoster mehrere ASP.NET-Anwendungen in einem einzelnen Arbeitsprozess an. Wenn mehrere Anwendungen einen einzelner Workerprozess für freigegebene verwenden, ist es jedoch für Server-Administratoren, um eine einzelne Anwendung zu identifizieren, bei denen Probleme aufgetreten ist, schwierig.

ASP.NET 4 nutzt neue ressourcenüberwachung Funktionen, die von der CLR eingeführt. Um diese Funktionalität zu aktivieren, können Sie den folgenden Ausschnitt der XML-Konfiguration zum Hinzufügen der `aspnet.config` Konfigurationsdatei.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Beachten Sie die `aspnet.config` Datei befindet sich im Verzeichnis, auf dem .NET Framework installiert ist. Es ist nicht die `Web.config` Datei.


Wenn die *AppDomainResourceMonitoring* Feature aktiviert wurde, sind zwei neue Leistungsindikatoren verfügbar, in der Leistungskategorie "ASP.NET Applications": *verwaltet Prozessorzeit (%)* und  *Verwalteter Speicher verwendet*. Beide dieser Leistungsindikatoren verwenden die neue CLR-Anwendungsdomäne Ressource Management-Funktion zum Nachverfolgen von Geschätzte CPU-Zeit und verwalteten arbeitsspeichernutzung der einzelnen ASP.NET-Anwendungen. Daher mit ASP.NET 4 Administratoren verfügen jetzt über eine differenziertere Ansicht in den Ressourcenverbrauch der einzelnen Anwendungen, die ein einzelner Workerprozess ausgeführt.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Festlegung von Zielversionen

Sie können eine Anwendung erstellen, die eine bestimmte Version von .NET Framework vorgesehen ist. In ASP.NET 4 hat ein neues Attribut in der *Kompilierung* Element von der `Web.config` der Datei können Sie die Zielversion von .NET Framework 4 und höher. Wenn Sie explizit die .NET Framework 4 abzielen und Einschließen von optionalen Elementen in der `Web.config` Datei, z. B. die Einträge für *system.codedom*, diese Elemente müssen korrekt sein, damit die .NET Framework 4. (Wenn Sie .NET Framework 4 nicht explizit abzielen, wird das Zielframework abgeleitet, aus der Mangel an einen Eintrag in der `Web.config` Datei.)

Das folgende Beispiel zeigt die Verwendung von der *TargetFramework* Attribut in der *Kompilierung* Element von der `Web.config` Datei.

[!code-xml[Main](overview/samples/sample17.xml)]

Beachten Sie Folgendes im Zusammenhang mit eine bestimmte Version von .NET Framework abzielt:

- In einer .NET Framework 4-Anwendungspool das ASP.NET-Buildsystem .NET Framework 4 als Ziel wenn nimmt die `Web.config` Datei enthält keinen der *TargetFramework* Attribut oder, wenn die `Web.config` Datei ist nicht vorhanden. (Sie möglicherweise Codierung Änderungen auf Ihre Anwendung für die Ausführung unter .NET Framework 4 zu vereinfachen.)
- Wenn Sie aufnehmen der *TargetFramework* -Attribut, und wenn die *system.codeDom* -Element wird definiert, der `Web.config` Datei, diese Datei muss die richtige Einträge für .NET Framework 4 enthalten.
- Bei Verwendung der *Aspnet\_Compiler* -Befehl, um das Vorkompilieren von Ihrer Anwendung (z. B. in einer Buildumgebung), müssen Sie die richtige Version von den *Aspnet\_Compiler* für das Zielframework-Befehl. Verwenden Sie den Compiler an, die gelieferten mit .NET Framework 2.0 (% WINDIR%\Microsoft.NET\Framework\v2.0.50727), um für die .NET Framework 3.5 und frühere Versionen kompilieren. Verwenden Sie den Compiler, der geliefert wird mit .NET Framework 4 zum Kompilieren von Anwendungen erstellt wurden, mithilfe dieses Framework oder höhere Versionen.
- Zur Laufzeit verwendet der Compiler die neuesten Framework-Assemblys, die auf dem Computer installiert sind (und daher im GAC). Wenn ein Update für das Framework später vorgenommen wird (z. B. eine hypothetische Version 4.1 installiert ist), Sie werden in der Lage, Funktionen, obwohl in der neueren Version des Frameworks verwenden die *TargetFramework* Attribut zielt auf eine niedrigere Version (z. B. 4.0). (Allerdings zur Entwurfszeit in Visual Studio 2010 oder bei Verwendung der *Aspnet\_Compiler* Befehl neuere Funktionen des Frameworks Benutzung des werden Compilerfehler).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery eingeschlossene mit Web Forms und MVC

Die Visual Studio-Vorlagen für Web Forms und MVC enthalten die Open-Source-jQuery-Bibliothek. Wenn Sie eine neue Website oder ein Projekt erstellen, wird ein Ordner Scripts, enthält die folgenden 3 Dateien erstellt:

- jQuery-1.4.1.js – der lesbare vollständige Version von jQuery-Bibliothek.
- jQuery-14.1.min.js – die verkleinerte Version der jQuery-Bibliothek.
- jQuery-1.4.1-vsdoc.js – die Intellisense-Dokumentationsdatei für die jQuery-Bibliothek.

Schließen Sie die unminified Version von jQuery, beim Entwickeln einer Anwendung. Enthalten Sie die verkleinerte Version von jQuery für Produktionsanwendungen.

Die folgenden Web Forms-Seite wird z. B. veranschaulicht, wie Sie jQuery verwenden können, so ändern Sie die Hintergrundfarbe des ASP.NET TextBox-Steuerelemente Gelb, wenn sie den Fokus besitzen.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Content Delivery Network-Support

Das Microsoft Ajax Content Delivery Network (CDN) können Sie problemlos ASP.NET Ajax und jQuery-Skripts auf die Webanwendungen hinzufügen. Sie können z. B. starten, mithilfe der jQuery-Bibliothek einfach durch Hinzufügen einer `<script>` -Tag, um die Seite, die auf Ajax.microsoft.com wie folgt verweist:

[!code-html[Main](overview/samples/sample19.html)]

Durch das Microsoft Ajax-CDN nutzen, können Sie die Leistung der Ajax-Anwendungen erheblich verbessern. Der Inhalt des Microsoft Ajax-CDNS werden auf Servern, die auf der ganzen Welt zwischengespeichert. Darüber hinaus ermöglicht das Microsoft Ajax-CDN Browsern zum Wiederverwenden von zwischengespeicherter JavaScript-Dateien für Websites, die in unterschiedlichen Domänen befinden.

Microsoft Ajax Content Delivery Network unterstützt SSL (HTTPS) an, für den Fall, dass Sie einer Webseite mithilfe des Secure Sockets Layers bereitstellen müssen.

Implementieren Sie ein Fallback, wenn das CDN nicht verfügbar ist. Das Fallback zu testen.

Um weitere Informationen über das Microsoft Ajax-CDN finden Sie auf der folgenden Website:

[https://www.asp.net/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

Die ScriptManager ASP.NET unterstützt das Microsoft Ajax-CDN. Durch Festlegen der Eigenschaft ein, die Eigenschaft EnableCdn können Sie alle ASP.NET Framework JavaScript-Dateien aus dem CDN abrufen:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Nachdem Sie die EnableCdn-Eigenschaft auf den Wert "true" festgelegt, wird von das ASP.NET-Framework alle ASP.NET Framework JavaScript-Dateien aus dem CDN, einschließlich aller JavaScript-Dateien, die für die Validierung und UpdatePanel verwendet abgerufen werden. Durch Festlegen dieser Eigenschaft eine haben eine drastische Auswirkungen auf die Leistung Ihrer Webanwendung.

Sie können den CDN-Pfad für Ihre eigenen JavaScript-Dateien festlegen, mit dem WebResource-Attribut. Die neue CdnPath-Eigenschaft gibt den Pfad zu dem CDN verwendet, wenn Sie die EnableCdn-Eigenschaft auf den Wert "true" festlegen:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Explizite Skripts in ScriptManager

In der Vergangenheit bei Verwendung der ASP.NET ScriptManger dann mussten Sie die gesamte monolithischen ASP.NET Ajax-Bibliothek laden. Durch die Nutzung der neuen ScriptManager.AjaxFrameworkMode-Eigenschaft, können Sie steuern, genau die Komponenten des ASP.NET Ajax-Bibliothek geladen werden und Laden nur die Komponenten des ASP.NET Ajax-Bibliothek, die Sie benötigen.

Die ScriptManager.AjaxFrameworkMode-Eigenschaft kann auf die folgenden Werte festgelegt werden:

- Aktiviert – Gibt an, dass das ScriptManager-Steuerelement automatisch die Skriptdatei MicrosoftAjax.js beinhaltet. dabei eine kombinierte Skriptdatei jedes Core-Framework-Skripts (Legacyverhalten).
- Deaktiviert – Gibt an, dass alle Funktionen von Microsoft Ajax-Skript deaktiviert sind und das ScriptManager-Steuerelement automatisch keine Skripts verwiesen wird.
- Explizite – Gibt an, dass Sie explizit Skriptverweise in einzelne Framework Core-Skriptdatei enthält, die Ihre Seite erforderlich ist, und Sie Verweise auf die Abhängigkeiten enthalten, die jede Skriptdatei aus erfordert.

Z. B. Wenn Sie die AjaxFrameworkMode-Eigenschaft auf den Wert explizit festlegen können Sie die bestimmten ASP.NET Ajax-Komponente-Skripts angeben, die Sie benötigen:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>Web Forms

WebForms ist seit der Veröffentlichung von ASP.NET 1.0 ein Hauptmerkmal in ASP.NET. Zahlreiche Verbesserungen wurden in diesem Bereich für ASP.NET 4, u. a. folgende:

- Festlegen von *Meta* Tags.
- Mehr Kontrolle über den Ansichtszustand.
- Einfacher Verfahren zum Arbeiten mit Browserfunktionen.
- Unterstützung für die Verwendung von ASP.NET-Routing mit Web Forms.
- Mehr Kontrolle über die generierten IDs.
- Die Fähigkeit zum ausgewählten Zeilen in Datensteuerelementen persistent zu speichern.
- Mehr Kontrolle über gerendertes HTML in der *FormView* und *ListView* Steuerelemente.
- Filtern die Unterstützung für Datenquellensteuerelemente.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Festlegen von Meta-Tags mit dem Page.MetaKeywords-Eigenschaft und die Page.MetaDescription-Eigenschaft

ASP.NET 4 fügt zwei Eigenschaften, die *Seite* -Klasse, *MetaKeywords* und *MetaDescription*. Diese beiden Eigenschaften darstellen, entspricht *Meta* tags auf der Seite, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Diese beiden Eigenschaften funktionieren genauso wie der Seite *Titel* Eigenschaft verfügt. Sie können diesen Regeln entsprechen:

1. Treten keine *Meta* -Tags in der *Head* Element, das die Eigenschaftennamen übereinstimmen (das heißt, = "Schlüsselwörter" für *Page.MetaKeywords-Eigenschaft* und Name = "Description" für  *Page.MetaDescription*, Bedeutung, die diese Eigenschaften nicht festgelegt wurden), wird die *Meta* Tags werden auf der Seite hinzugefügt, wenn er gerendert wird.
2. Wenn es bereits *Meta* Tags mit diesen Namen, diese Eigenschaften dienen als get- und-Methoden für den Inhalt der vorhandenen Tags Set.

Sie können diese Eigenschaften festlegen, zur Laufzeit, sodass Sie den Inhalt aus einer Datenbank oder einer anderen Quelle erhalten können, und die können Sie die Tags dynamisch beschrieben festlegen eine bestimmte Seite ist.

Sie können auch Festlegen der *Schlüsselwörter* und *Beschreibung* Eigenschaften in der *@ Page* -Direktive am Anfang der Web Forms-Seitenmarkup, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Dies überschreibt die *Meta* tag-Inhalt (sofern vorhanden) auf der Seite bereits deklariert.

Der Inhalt der Beschreibung *Meta* Tag dienen zum Verbessern der Suche nach vorschauberichte im Google auflisten. (Weitere Informationen finden Sie unter [verbessern Codeausschnitte mit Webseminar ein Meta-Beschreibung](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) Google Webmaster zentralen-Blog.) Google und Windows Live Search nicht den Inhalt der Schlüsselwörter für Elemente verwendet, aber möglicherweise andere Suchmaschinen. Weitere Informationen finden Sie unter [Meta Schlüsselwörter Ratschläge](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) auf der Website von Search Engine Guide.

Diese neuen Eigenschaften sind eine einfache Funktion, aber sie speichern Sie aus der Anforderung, diese manuell hinzufügen oder Schreiben von Code zum Erstellen der *Meta* Tags.

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Aktivieren des Ansichtszustands für einzelne Steuerelemente

Standardmäßig ist Ansichtszustand für die Seite, mit dem Ergebnis aktiviert, dass jedes Steuerelement auf der Seite potenziell Ansichtszustand speichert, auch wenn es nicht für die Anwendung erforderlich ist. Ansichtszustandsdaten ist im Markup enthalten, dass eine Seite generiert und erhöht das Ausmaß der Zeitaufwand zum Senden einer Seite an den Client und zurücksenden. Speichern von weitere Ansichtszustand als notwendig kann bedeutenden Leistungseinbußen führen. In früheren Versionen von ASP.NET Entwickler Ansichtszustand für einzelne Steuerelemente konnte deaktiviert werden, um die Seitengröße zu reduzieren, enthielt jedoch explizit für einzelne Steuerelemente ausführen. In ASP.NET 4-Webserversteuerelemente enthalten eine *ViewStateMode* Eigenschaft, mit dem Sie die Ansichtszustand standardmäßig zu deaktivieren und aktivieren Sie ihn nur für die Steuerelemente, die sie auf der Seite erfordern.

Die *ViewStateMode* Eigenschaft nimmt eine Enumeration, die drei Werte aufweisen: *aktiviert*, *deaktiviert*, und *erben*. *Aktiviert* ermöglicht den Ansichtszustand für das entsprechende Steuerelement und alle untergeordneten Steuerelemente, die festgelegt werden, um *erben* oder nichts eingerichtet haben. *Deaktivierte* deaktiviert den Ansichtszustand, und *erben* gibt an, dass das Steuerelement verwendet die *ViewStateMode* aus dem übergeordneten Steuerelement festlegen.

Das folgende Beispiel zeigt wie die *ViewStateMode* -Eigenschaft funktioniert. Markup und Code für die Steuerelemente in der folgenden Seite enthält Werte für die *ViewStateMode* Eigenschaft:

[!code-aspx[Main](overview/samples/sample25.aspx)]

Wie Sie sehen können, wird der Code Ansichtszustand für das Steuerelement PlaceHolder1 deaktiviert. Das untergeordnete label1 Steuerelement erbt dieses Eigenschaftswerts (*erben* ist der Standardwert für *ViewStateMode* für Steuerelemente fest.) und daher kein Ansichtszustand speichert. In das Steuerelement "Platzhalter2" *ViewStateMode* festgelegt ist, um *aktiviert*, sodass label2 diese Eigenschaft erbt und speichert den Ansichtszustand. Wenn die Seite erstmals geladen wird, die *Text* -Eigenschaft beider *Bezeichnung* Steuerelemente auf die Zeichenfolge "[" dynamicvalue "]" festgelegt ist.

Die Auswirkung dieser Einstellungen ist, dass beim ersten der Seite laden im Browser die folgende Ausgabe angezeigt wird:

Deaktiviert `: [DynamicValue]`

Aktiviert:`[DynamicValue]`

Nach einem Postback wird die folgende Ausgabe angezeigt:

Deaktiviert `: [DeclaredValue]`

Aktiviert:`[DynamicValue]`

Das Steuerelement label1 (, deren *ViewStateMode* Wert wird festgelegt, um *deaktiviert*) hat nicht den Wert an, die sie im Code festgelegt wurde beibehalten. Allerdings steuern, die label2 (, deren *ViewStateMode* Wert wird festgelegt, um *aktiviert*) hat den Zustand beibehalten.

Sie können auch festlegen *ViewStateMode* in der *@ Page* Richtlinie, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample26.aspx)]

Die *Seite* Klasse ist nur ein anderes Steuerelement; es dient als das übergeordnete Steuerelement für alle anderen Steuerelemente auf der Seite. Der Standardwert von *ViewStateMode* ist *aktiviert* für Instanzen von *Seite*. Da Steuerelemente standardmäßig *erben*, Steuerelemente, erbt die *aktiviert* Eigenschaftswert, es sei denn, Sie legen *ViewStateMode* auf der Seite oder eines Steuerelements.

Der Wert von der *ViewStateMode* Eigenschaft bestimmt, wenn der Ansichtszustand beibehalten wird, nur dann, wenn die *EnableViewState* -Eigenschaftensatz auf *"true"*. Wenn der *EnableViewState* -Eigenschaftensatz auf *"false"*, Ansichtszustand werden nicht beibehalten, wenn *ViewStateMode* auf festgelegt ist *aktiviert*.

Eine gute Verwendung für diese Funktion ist mit *ContentPlaceHolder* Steuerelemente in Masterseiten, in dem Sie festlegen können *ViewStateMode* auf *deaktiviert* für den Master Seite, und aktivieren Sie ihn einzeln für *ContentPlaceHolder* Steuerelemente, die wiederum Steuerelemente enthalten, müssen den Ansichtszustand.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Änderungen an der Webbrowser-Funktionen

ASP.NET bestimmt die Funktionen des Browsers, die ein Benutzer verwendet, um Ihre Website zu durchsuchen, indem Sie mit einem Feature *Browserfunktionen*. Webbrowser-Funktionen werden durch dargestellt die *HttpBrowserCapabilities* Objekt (verfügbar gemacht werden, indem Sie die *Request.Browser* Eigenschaft). Beispielsweise können Sie die *HttpBrowserCapabilities* bestimmt, ob der Typ und der Version des aktuellen Browsers unterstützt eine bestimmte Version von JavaScript. Oder Sie können die *HttpBrowserCapabilities* bestimmt, ob die Anforderung von einem mobilen Gerät stammt.

Die *HttpBrowserCapabilities* Objekt wird durch eine Reihe von Browserdefinitionsdateien gesteuert. Diese Dateien enthalten Informationen zu den Funktionen von bestimmten Browsern. In ASP.NET 4 haben diese Browserdefinitionsdateien aktualisiert, um Informationen zu kürzlich eingeführten Browsern und Geräten wie z. B. Google Chrome, Research in Motion BlackBerry-Smartphones und Apple iPhone enthalten.

Die folgende Liste enthält neue Browser Definitionsdateien:

- *blackberry.browser*
- *chrome.browser*
- *Default.browser*
- *firefox.browser*
- *gateway.browser*
- *generic.browser*
- *ie.browser*
- *iemobile.browser*
- *iphone.browser*
- *opera.browser*
- *safari.browser*

#### <a name="using-browser-capabilities-providers"></a>Browser Funktionen Anbieter verwenden

In ASP.NET Version 3.5 Service Pack 1, definieren Sie die Funktionen, die über ein Browser auf folgende Weise verfügt:

- Auf der Computerebene erstellen oder Aktualisieren einer `.browser` XML-Datei in den folgenden Ordner:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Nachdem Sie die Browserfunktion definiert haben, führen Sie den folgenden Befehl aus Visual Studio-Eingabeaufforderung, um das Neuerstellen der Assembly des Browsers, und fügen sie dem GAC hinzu:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Für eine einzelne Anwendung Sie erstellen eine `.browser` Datei in der Anwendungsverzeichnis `App_Browsers` Ordner.

Diese Ansätze erfordern, dass Sie XML-Dateien ändern und für Computerebene ändern, Sie müssen neu starten, die Anwendung nach dem Ausführen der Aspnet\_regbrowsers.exe Prozess.

ASP.NET 4 umfasst eine Funktion, die so genannte *Browser Funktionen Anbieter*. Wie der Name bereits vermuten lässt, mithilfe dieser können Sie einen Anbieter erstellen, der wiederum können Sie Ihren eigenen Code um Browserfunktionen zu bestimmen.

In der Praxis definieren Entwickler häufig benutzerdefinierte Browserfunktionen. Browser-Dateien sind schwer zu aktualisieren, den Prozess für recht kompliziert wird aktualisiert und die XML-Syntax für `.browser` Dateien können schwierig sein, zu verwenden und definieren. Was diese Prozess wesentlich einfacher machen würden ist, wenn gab es eine allgemeine definitionssyntax für Browser oder eine Datenbank, die auf dem neuesten Stand Browserdefinitionen oder sogar einen Webdienst für eine Datenbank enthalten. Anbieter der neuen Browserfunktion-Funktionen ermöglicht diese Szenarien möglich und praktikabel für Entwickler von Drittanbietern.

Es gibt zwei Hauptansätze für die Verwendung der neuen ASP.NET 4-Funktionen Anbieter Browserfunktion: erweitern die ASP.NET Browserfunktionen Funktionalitäten oder vollständig zu ersetzen. In den folgenden Abschnitten werden zuerst beschrieben, wie die Funktionalität zu ersetzen und das anschließende erweitern.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Ersetzen die Funktionen von ASP.NET Browserfunktionen

Um die ASP.NET Browser Funktionen Definition Funktionalität vollständig zu ersetzen, gehen Sie folgendermaßen vor:

1. Erstellen eine Klasse, die abgeleitet *HttpCapabilitiesProvider* und überschreibt die *GetBrowserCapabilities* Methode, wie im folgenden Beispiel gezeigt: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    Der Code in diesem Beispiel erstellt ein neues *HttpBrowserCapabilities* Objekt, das nur die Möglichkeit, die mit dem Namen Browser und diese Funktion auf MyCustomBrowser angeben.
2. Registrieren Sie den Anbieter mit der Anwendung. 

    Um einen Anbieter mit einer Anwendung verwenden zu können, müssen Sie Hinzufügen der *Anbieter* -Attribut auf die *BrowserCaps* im Abschnitt der `Web.config` oder `Machine.config` Dateien. (Sie können auch definieren, die Anbieterattribute in einer *Speicherort* -Element für bestimmte Verzeichnisse, in der Anwendung, z. B. in einen Ordner für eine spezifische mobile Gerät.) Im folgende Beispiel wird gezeigt, wie zum Festlegen der *Anbieter* Attribut in einer Konfigurationsdatei:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Eine weitere Möglichkeit zum Registrieren der neuen Definition des Browser-Funktion ist die Verwendung von Code, wie im folgenden Beispiel gezeigt:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Dieser Code muss ausgeführt, der *Anwendung\_starten* -Ereignis der `Global.asax` Datei. Jede Änderung der *BrowserCapabilitiesProvider* Klasse auftreten muss, bevor der Code in der Anwendung ausgeführt wird, um sicherzustellen, dass der Cache in einem gültigen Zustand für das aufgelöste bleibt, *HttpCapabilitiesBase* Objekt.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>Das Objekt HttpBrowserCapabilities Zwischenspeichern

Im vorherige Beispiel wurde ein Problem ist, dass der Code jedes Mal, den benutzerdefinierte Anbieter aufgerufen ausgeführt würde, wird um das Abrufen der *HttpBrowserCapabilities* Objekt. Dies kann mehrere Male während jeder Anforderung geschehen. Im Beispiel wird der Code für den Anbieter viel nicht. Wenn der Code in Ihrem benutzerdefinierten Anbieter beträchtlichen Arbeitsaufwand in führt jedoch bestellen, zum Abrufen der *HttpBrowserCapabilities* -Objekt, kann dies die Leistung beeinträchtigen. Um dies zu vermeiden, können Sie Zwischenspeichern der *HttpBrowserCapabilities* Objekt. Führen Sie folgende Schritte aus:

1. Erstellen Sie eine Klasse, die abgeleitet *HttpCapabilitiesProvider*, wie das im folgenden Beispiel: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    Im Beispiel der Code generiert einen Cacheschlüssel durch Aufrufen einer benutzerdefinierten BuildCacheKey-Methode, und es Ruft die Zeitdauer zum Cache durch Aufrufen einer benutzerdefinierten GetCacheTime-Methode. Der Code fügt dann das aufgelöste *HttpBrowserCapabilities* Objekt im Cache. Das Objekt kann aus dem Cache abgerufen und auf wiederverwendet werden nachfolgende Anforderungen, die Stellen des benutzerdefinierten Anbieters verwenden.
2. Registrieren Sie den Anbieter mit der Anwendung an, wie im vorherigen Verfahren beschrieben.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Erweitern von Funktionen für ASP.NET Browserfunktionen

Im vorherigen Abschnitt beschrieben, wie zum Erstellen eines neuen *HttpBrowserCapabilities* Objekt in ASP.NET 4. Sie können auch die ASP.NET Browser Funktionen Funktionalität erweitern, indem Sie neue Definitionen von Browser-Funktionen an, die bereits in ASP.NET sind. Dies ist möglich, ohne die XML-Browser-Definitionen. Die folgende Prozedur zeigt wie.

1. Erstellen Sie eine Klasse, die abgeleitet *HttpCapabilitiesEvaluator* und überschreibt die *GetBrowserCapabilities* Methode, wie im folgenden Beispiel gezeigt: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    Dieser Code verwendet die ASP.NET Funktionen Browserfunktionen zunächst zu dem Versuch, den Browser zu identifizieren. Allerdings wird keine Browser identifiziert anhand der Informationen in der Anforderung definiert (d. h., wenn der *Browser* Eigenschaft von der *HttpBrowserCapabilities* Object ist die Zeichenfolge "Unknown"), ruft der Code der benutzerdefinierte Anbieter (MyBrowserCapabilitiesEvaluator), um den Browser zu identifizieren.
2. Registrieren Sie den Anbieter mit der Anwendung an, wie im vorherigen Beispiel beschrieben.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Browserfunktionen-Funktionen zu erweitern, indem Sie neue Funktionen hinzufügen, um vorhandene Funktionen Definitionen

Zusätzlich zum Erstellen eines benutzerdefinierten Browser Definition Anbieters und neue Browserdefinitionen dynamisch erstellen können Sie vorhandene Browserdefinitionen mit einem zusätzlichen Funktionsumfang erweitern. Dadurch können Sie eine Definition verwenden, die in der Nähe erwünscht ist, jedoch nur einige Funktionen fehlen. Gehen Sie dazu folgendermaßen vor.

1. Erstellen Sie eine Klasse, die abgeleitet *HttpCapabilitiesEvaluator* und überschreibt die *GetBrowserCapabilities* Methode, wie im folgenden Beispiel gezeigt: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    Der Beispielcode erweitert den vorhandenen ASP.NET *HttpCapabilitiesEvaluator* Klasse und ruft die *HttpBrowserCapabilities* -Objekt, das die Anforderungsdefinition der aktuellen übereinstimmt, mithilfe des folgenden Codes :

    [!code-csharp[Main](overview/samples/sample35.cs)]

    Der Code kann dann hinzufügen oder ändern eine Funktion für diesen Browser. Es gibt zwei Möglichkeiten, eine neue Browserfunktion anzugeben:

    - Ein Schlüssel/Wert-Paar hinzufügen der *IDictionary* -Objekt, das vom verfügbar gemacht wird die *Funktionen* Eigenschaft von der *HttpCapabilitiesBase* Objekt. Im vorherigen Beispiel ist der Code Fügt eine Funktion mit dem Namen Mehrfingereingabe mit einem Wert von *"true"*.
    - Legen Sie die vorhandene Eigenschaften von der *HttpCapabilitiesBase* Objekt. Im vorherigen Beispiel, mit dem Code wird die *Frames* Eigenschaft *"true"*. Diese Eigenschaft ist einfach einen Accessor für die *IDictionary* -Objekt, das vom verfügbar gemacht wird die *Funktionen* Eigenschaft. 

        > [!NOTE]
        > Beachten Sie, dieses Modell gilt, für jede Eigenschaft eines *HttpBrowserCapabilities*, einschließlich Steuerelementadapter.
2. Registrieren Sie den Anbieter mit der Anwendung an, wie im vorherigen Verfahren beschrieben.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Routing in ASP.NET 4

ASP.NET 4 fügt integrierte Unterstützung für die Verwendung mit Web Forms routing. Routing können, die Sie konfigurieren, dass eine Anwendung akzeptieren, Anforderungs-URLs, die keinen physischen Dateien zugeordnet sind. Stattdessen können Sie routing um URLs zu definieren, die für Benutzer aussagekräftig sind und zur Entwicklung mit Search Engine Optimization (SEO) für Ihre Anwendung beitragen können. Beispielsweise könnte die URL für eine Seite, die Produktkategorien in einer vorhandenen Anwendung zeigt, wie im folgenden Beispiel aussehen:

[!code-console[Main](overview/samples/sample36.cmd)]

Mithilfe von routing können Sie die Anwendung, akzeptieren die folgende URL für das Rendern von der gleichen Informationen konfigurieren:

[!code-console[Main](overview/samples/sample37.cmd)]

Routing wurde seit ASP.NET 3.5 SP1 verfügbar. (Ein Beispiel zum Verwenden von routing in ASP.NET 3.5 SP1, finden Sie im Eintrag [Routing mit WebForms verwenden](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "Titel dieses Eintrags.") Haacks Blog.) ASP.NET 4 enthält jedoch einige Funktionen, die Weiterleitung, verwenden Sie erleichtern u. a. folgende:

- Die *PageRouteHandler* Klasse, die eine einfache HTTP-Handler ist, die Sie beim Definieren von Routen verwenden. Die Klasse übergibt Daten an die Seite, der die Anforderung weitergeleitet wird.
- Die neuen Eigenschaften *HttpRequest.RequestContext* und *Page.RouteData* (also ein Proxy für die *HttpRequest.RequestContext.RouteData* Objekt). Diese Eigenschaften erleichtern es, den Zugriff auf Informationen, die aus der Route übergeben wird.
- Die folgenden neuen Ausdrucks-Generatoren, die in definiert sind *System.Web.Compilation.RouteUrlExpressionBuilder* und *System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*, stellt eine einfache Möglichkeit zum Erstellen einer URL, die Routen-URL in ein ASP.NET-Serversteuerelement entspricht.
- *RouteValue*, stellt eine einfache Möglichkeit zum Extrahieren von Informationen aus der *RouteContext* Objekt.
- Die *RouteParameter* -Klasse, die übergeben von Daten in vereinfacht eine *RouteContext* Objekt auf eine Abfrage für ein Datenquellensteuerelement (ähnlich wie [ *FormParameter* ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Routing für Web Forms-Seiten

Im folgende Beispiel wird gezeigt, wie eine Web Forms-Route definieren, mit dem neuen *MapPageRoute* Methode der *Route* Klasse:

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 stellt die *MapPageRoute* Methode. Im folgende Beispiel entspricht der SearchRoute-Definition, die im vorherigen Beispiel dargestellt ist, verwendet jedoch die *PageRouteHandler* Klasse.

[!code-csharp[Main](overview/samples/sample39.cs)]

Der Code im Beispiel ordnet die Route zu einer physischen Seite (in der ersten Route zum `~/search.aspx`). Die erste Route-Definition gibt auch an, dass Parameternamen Searchterm aus der URL extrahiert und an die Seite übergeben werden soll.

Die *MapPageRoute* Methode unterstützt die folgenden methodenüberladungen:

- *MapPageRoute(string routeName, string routeUrl, string physicalFile, bool checkPhysicalUrlAccess)*
- *MapPageRoute (Zeichenfolge RouteName, Zeichenfolge RouteUrl, PhysicalFile String, Bool CheckPhysicalUrlAccess, RouteValueDictionary Standardwerte)*
- *MapPageRoute (Zeichenfolge RouteName, Zeichenfolge RouteUrl, PhysicalFile String, Bool CheckPhysicalUrlAccess, RouteValueDictionary Standardwerte, RouteValueDictionary Einschränkungen)*

Die *CheckPhysicalUrlAccess* Parameter gibt an, ob die Route die Sicherheitsberechtigungen für die physische Seite weitergeleitet überprüft werden soll (in diesem Fall search.aspx) und die Berechtigungen für die eingehende URL (in diesem Fall suchen / {Searchterm}). Wenn der Wert der *CheckPhysicalUrlAccess* ist *"false"*, werden nur die Berechtigungen der eingehenden URL überprüft werden. Diese Berechtigungen werden definiert, der `Web.config` mithilfe von Einstellungen wie z. B. die folgende Datei:

[!code-xml[Main](overview/samples/sample40.xml)]

In der Beispielkonfiguration wird Zugriff auf die physische Seite `search.aspx` für alle Benutzer mit Ausnahme derjenigen, die in der Rolle "Admin" sind. Wenn die *CheckPhysicalUrlAccess* Parametersatz auf *"true"* (Dies ist der Standardwert), nur Administratorbenutzern sind URL-/search/ {Searchterm}, den Zugriff auf zulässig, da die physische Seite search.aspx ist auf Benutzer in dieser Rolle beschränkt. Wenn *CheckPhysicalUrlAccess* festgelegt ist, um *"false"* und die Website konfiguriert ist, wie im vorherigen Beispiel gezeigt, alle authentifizierten Benutzer können die URL-/search/ {Searchterm} zugreifen.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Lesen, die Routinginformationen in einer Web Forms-Seite

Im Code von der physischen Web Forms-Seite können Sie die Informationen, die Weiterleitung von der URL extrahiert wurde zugreifen (oder andere Informationen, die ein anderes Objekt hinzugefügt hat die *RouteData* Objekt) mithilfe von zwei neue Eigenschaften:  *HttpRequest.RequestContext* und *Page.RouteData*. (*Page.RouteData* umschließt *HttpRequest.RequestContext.RouteData*.) Das folgende Beispiel zeigt, wie Sie *Page.RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

Der Code extrahiert den Wert, der für den Parameter Searchterm übergeben wurde, wie weiter oben in der Beispiel-Route definiert. Betrachten Sie die folgenden Anforderungs-URL ein:

[!code-console[Main](overview/samples/sample42.cmd)]

Wenn diese Anforderung gesendet wird, wird das Wort "Scott" würde gerendert werden, in der `search.aspx` Seite.

#### <a name="accessing-routing-information-in-markup"></a>Zugreifen auf die Routinginformationen im Markup

Im vorherigen Abschnitt beschriebene Methode veranschaulicht das Abrufen von Routendaten im Code in einer Web Forms-Seite. Sie können auch Ausdrücke in Markup, mit die Sie Zugriff auf die gleichen Informationen zu erhalten. Ausdrucks-Generatoren sind eine elegante Anzeige von deklarativem Code arbeiten. (Weitere Informationen finden Sie im Eintrag [Express selbst mit benutzerdefinierten Ausdrucks-Generatoren](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) Haacks Blog.)

ASP.NET 4 umfasst zwei neuen Ausdrucks-Generatoren für Web Forms-routing. Im folgende Beispiel wird gezeigt, wie sie verwendet wird.

[!code-aspx[Main](overview/samples/sample43.aspx)]

Im Beispiel die *RouteUrl* Ausdruck wird verwendet, um eine URL definieren, die einen Routenparameter basiert. Dies erspart Ihnen von zu programmieren die vollständige URL in das Markup und können Sie die URL-Struktur später ändern, ohne dass alle Änderungen an diesen Link.

Dieses Markup basierend auf der Route, die zuvor definiert wurde, wird die folgende URL generiert:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET funktioniert automatisch, die richtige Route (d. h. er generiert die richtige URL) basierend auf den Eingabeparametern. Sie können auch einen Routennamen einschließen, die im Ausdruck, die Sie eine zu verwendende Route angeben können.

Das folgende Beispiel zeigt, wie Sie die *RouteValue* Ausdruck.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Wenn die Seite, die dieses Steuerelement enthält ausgeführt wird, wird der Wert "Scott" in der Bezeichnung angezeigt.

Die *RouteValue* Ausdruck vereinfacht die Routendaten im Markup verwenden und es vermeidet arbeiten mit komplexen Page.RouteData["x"] Syntax im Markup.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Verwenden für Data Source Control Parameters Routendaten

Die *RouteParameter* Klasse ermöglicht die Angabe Routendaten als Parameterwert für Abfragen in ein Datenquellen-Steuerelement. Es [funktioniert ähnlich wie die](https://msdn.microsoft.com/library/system.web.ui.webcontrols.formparameter.aspx) Klasse, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample46.aspx)]

In diesem Fall wird der Wert der Route Parameter Searchterm verwendet werden, für die @companyname Parameter in der <em>wählen</em> Anweisung.

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Festlegen von Client-IDs

Die neue *ClientIDMode* Eigenschaft löst ein langjähriges Problem in ASP.NET nämlich wie Steuerelemente erstellen die *Id* Attribut für Elemente, die diese rendern. Kenntnis der *Id* Attribut für gerenderte Elemente ist wichtig, wenn Ihre Anwendung Clientskript enthält, die diese Elemente verweist.

Die *Id* -Attribut in HTML, die für Webserversteuerelemente gerendert wird generiert, basierend auf den *ClientID* Eigenschaft des Steuerelements. Bis ASP.NET 4, der Algorithmus zum Generieren der *Id* -Attribut aus der *ClientID* Eigenschaft wurde auf den Benennungscontainer (sofern vorhanden) mit der ID, und bei wiederholten Steuerelemente (als in verketten Data-Steuerelemente), einem Präfix und eine laufende Nummer hinzu. Während dies immer sichergestellt wurde, dass die IDs der Steuerelemente auf der Seite eindeutig sind, hat der Algorithmus im Steuerelement-IDs geführt, die wurden nicht vorhersagbare und daher schwierig zu verweisen, die im Clientskript.

Die neue *ClientIDMode* -Eigenschaft können Sie angeben, genauer gesagt, wie die Client-ID für Steuerelemente generiert wird. Sie können festlegen, die *ClientIDMode* -Eigenschaft für jedes Steuerelement, einschließlich für die Seite. Mögliche Einstellungen lauten wie folgt:

- *AutoID* – Dies ist gleichbedeutend mit der Algorithmus zum Generieren von *ClientID* Eigenschaftswerte, die in früheren Versionen von ASP.NET verwendet wurde.
- *Statische* – gibt an, dass die *ClientID* Wert wird mit der ID ohne verketten die IDs der übergeordneten Benennen von Containern identisch sein. Dies kann in Web-Benutzersteuerelemente nützlich sein. Da ein Web-Benutzersteuerelement sich auf unterschiedlichen Seiten und in anderen Containersteuerelementen sein kann, es kann schwierig sein, Clientskripts für Steuerelemente zu schreiben, verwenden die *AutoID* Algorithmus, da Sie im Voraus können nicht die ID-Werte .
- *Vorhersagbare* – diese Option ist in erster Linie für die Verwendung in Datensteuerelementen, die sich wiederholenden Vorlagen verwenden. Er das "ID"-Eigenschaften naming Container des Steuerelements, aber generierten verkettet *ClientID* Werte werden keine Zeichenfolgen wie "Ctlxxx". Diese Einstellung funktioniert in Verbindung mit der *ClientIDRowSuffix* Eigenschaft des Steuerelements. Sie legen die *ClientIDRowSuffix* Eigenschaft auf den Namen eines Datenfelds und der Wert dieses Felds wird als Suffix verwendet, für die generierte *ClientID* Wert. Normalerweise verwenden Sie den primären Schlüssel für einen Datensatz als den *ClientIDRowSuffix* Wert.
- *Erben* – diese Einstellung ist das Standardverhalten für Steuerelemente; er gibt an, dass ein Steuerelement-ID Generation identisch mit seinem übergeordneten Element.

Sie können festlegen, die *ClientIDMode* Eigenschaft auf Seitenebene. Dadurch wird die Standardeinstellung definiert *ClientIDMode* Wert für alle Steuerelemente in der aktuellen Seite.

Die Standardeinstellung *ClientIDMode* ist der Wert auf Seitenebene *AutoID*, und der standardmäßige *ClientIDMode* ist der Wert an die Steuerungsebene *erben*. Daher, wenn Sie diese Eigenschaft nicht an einer beliebigen Stelle im Code festlegen, alle Steuerelemente verwendet standardmäßig die *AutoID* Algorithmus.

Sie legen Sie den Wert auf Seitenebene die *@ Page* Richtlinie, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample47.aspx)]

Sie können auch Festlegen der *ClientIDMode* Wert in der Konfigurationsdatei, die auf der Computerebene (Computer) oder auf Anwendungsebene. Dadurch wird die Standardeinstellung definiert *ClientIDMode* für alle Steuerelemente auf allen Seiten in der Anwendung festlegen. Wenn Sie den Wert auf der Computerebene festlegen, definiert Sie die Standardeinstellung *ClientIDMode* Einstellung für alle Websites auf diesem Computer. Das folgende Beispiel zeigt die *ClientIDMode* in der Konfigurationsdatei festlegen:

[!code-xml[Main](overview/samples/sample48.xml)]

Wie den Wert der erwähnt, die *ClientID* Eigenschaft wird von den Benennungscontainer für übergeordneten Elements eines Steuerelements abgeleitet. In einigen Szenarien, z. B. Wenn Sie Masterseiten, verwenden können Steuerelemente IDs wie in der folgenden Schluss HTML gerendert:

[!code-html[Main](overview/samples/sample49.html)]

Obwohl die *Eingabe* Element im Markup dargestellt (aus eine *Textfeld* Steuerelement) ist nur zwei Benennen von Containern deep auf der Seite (der geschachtelten *ContentPlaceholder* Steuerelemente), aufgrund der Art Masterseiten verarbeitet werden, ist das Endergebnis eine Steuerelement-ID wie folgt:

[!code-console[Main](overview/samples/sample50.cmd)]

Diese ID ist auf der Seite eindeutig ist gewährleistet, allerdings ist unnötig lange für die Mehrzahl der Fälle. Stellen Sie sich vor, dass die Länge der gerenderten-ID zu reduzieren und haben mehr Kontrolle über die zum Generieren der ID werden soll. (Beispielsweise möchten Sie "Ctlxxx" Präfixe zu vermeiden.) Die einfachste Möglichkeit, dies zu erreichen ist, durch Festlegen der *ClientIDMode* Eigenschaft wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample51.aspx)]

In diesem Beispiel wird die *ClientIDMode* -Eigenschaftensatz auf *statische* für die äußerste *NamingPanel* -Element, und legen auf *vorhersagbar* für die innere *NamingControl* Element. Diese Einstellungen führen dazu das folgende Markup (der Rest der Seite und die Masterseite wird davon ausgegangen, dass wie im vorherigen Beispiel identisch sein):

[!code-html[Main](overview/samples/sample52.html)]

Die *statische* Einstellung wirkt sich das Zurücksetzen der Namenhierarchie für alle Steuerelemente in der äußersten *NamingPanel* -Elements und des Entfernens der *ContentPlaceHolder* und *MasterPage* IDs aus der generierten-ID. (Die *Namen* -Attribut des gerenderten Elemente ist davon nicht betroffen, damit die normale ASP.NET-Funktionalität für Ereignisse beibehalten wird, den Ansichtszustand und so weiter.) Ein Nebeneffekt des Zurücksetzens der Namenhierarchie ist, selbst wenn Sie das Markup für Verschieben der *NamingPanel* Elemente zu einer anderen *ContentPlaceholder* -Steuerelement, das gerenderte Clientkennungen bleiben unverändert.

> [!NOTE]
> Beachten Sie, dass es liegt bei Ihnen um sicherzustellen, dass das gerenderte Steuerelement-IDs eindeutig sind. Wenn sie nicht sind, können solche Funktionen verwendet werden, die eindeutige IDs für einzelne HTML-Elemente, z. B. den Client erfordert unterbrochen *document.getElementById* Funktion.


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Erstellen von vorhersehbaren Client-IDs in datengebundene Steuerelemente

Die *ClientID* Werte, die generiert werden, für die Steuerelemente in einem datengebundenen Listensteuerelement durch die legacy-Algorithmus können lang sein und sind nicht wirklich vorhersagbar. Die *ClientIDMode* Funktionalität ermöglicht Ihnen mehr steuern, über die Verwendung dieser IDs generiert werden.

Das Markup im folgenden Beispiel enthält eine *ListView* Steuerelement:

[!code-aspx[Main](overview/samples/sample53.aspx)]

Im vorherigen Beispiel der *ClientIDMode* und *RowClientIDRowSuffix* in Markup festgelegt sind. Die *ClientIDRowSuffix* Eigenschaft kann nur in datengebundene Steuerelemente verwendet werden, und ihr Verhalten unterscheidet sich je nachdem welches Steuerelement verwenden. Die Unterschiede sind dies:

- *GridView* Steuerelement – Sie können in der Datenquelle den Namen von einer oder mehreren Spalten angeben, die zur Laufzeit zum Erstellen des Clients IDs kombiniert werden. Wenn Sie festlegen, z. B. *RowClientIDRowSuffix* auf "ProductName" ProductID"," Steuerelement-IDs für gerenderte Elemente ein Format wie die folgende hat:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* Steuerelement – Sie können eine einzelne Spalte angeben, in der Datenquelle, die an die Client-ID angefügt ist Wenn Sie festlegen, z. B. *ClientIDRowSuffix* auf "ProductName", wird das gerenderte Steuerelement-IDs haben das Format wie folgt:

- [!code-console[Main](overview/samples/sample55.cmd)]

- In diesem Fall wird der nachfolgende 1 von der Produkt-ID des aktuellen Datenelements abgeleitet.

- *Repeater* Steuerelement – dieses Steuerelement unterstützt nicht die *ClientIDRowSuffix* Eigenschaft. In einem *Repeater* -Steuerelement wird der Index der aktuellen Zeile verwendet. Bei Verwendung von ClientIDMode = "Vorhersagbar" mit einem *Repeater* zu steuern, Client IDs werden generiert, die das folgende Format aufweisen:

- [!code-console[Main](overview/samples/sample56.cmd)]

- Die nachfolgende 0 ist der Index der aktuellen Zeile.

Die *FormView* und *DetailsView* Steuerelemente werden nicht mehrere Zeilen angezeigt, damit sie nicht unterstützen die *ClientIDRowSuffix* Eigenschaft.

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Beibehalten von Zeilenauswahl in Datensteuerelementen

Die *GridView* und *ListView* Steuerelemente können Benutzer eine Zeile auswählen können. In früheren Versionen von ASP.NET Auswahl auf den Index der Zeile auf der Seite basiert. Wenn Sie wählen Sie das dritte Element auf der Seite "1", und klicken Sie dann auf Seite 2 verschieben, z. B. das dritte Element auf der Seite ausgewählt.

Persistente Auswahl wurde anfänglich nur in Dynamic Data-Projekten in .NET Framework 3.5 SP1 unterstützt. Wenn diese Funktion aktiviert ist, basiert das aktuelle ausgewählte Element auf dem Data-Schlüssel für das Element. Dies bedeutet, dass wenn Sie wählen Sie die dritte Zeile auf der Seite "1", und wechseln zur Seite 2, ' Nothing ' auf der Seite "2" ausgewählt ist. Wenn Sie zurück auf Seite 1 verschoben haben, wird die dritte Zeile immer noch ausgewählt. Persistente Auswahl wird jetzt unterstützt, für die *GridView* und *ListView* Steuerelemente in allen Projekten mithilfe der *EnablePersistedSelection* Eigenschaft entsprechend den folgende:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>ASP.NET Chart-Steuerelement

Die ASP.NET *Diagramm* -Steuerelement nach unten erweitert die datenvisualisierung Angebote in .NET Framework. Mithilfe der *Diagramm* -Steuerelement können Sie ASP.NET-Seiten denen intuitive und grafisch anspruchsvollen Diagrammen für komplexe statistische oder finanzielle Analysen erstellen. Die ASP.NET *Diagramm* Steuerelement wurde als Add-on auf der Version von .NET Framework Version 3.5 SP1 eingeführt und ist Bestandteil der .NET Framework 4-Version.

Das Steuerelement schließt die folgenden Funktionen:

- 35 verschiedene Diagrammtypen
- Eine unbegrenzte Anzahl von Diagrammbereichen, Titeln, Legenden und Anmerkungen.
- Eine Vielzahl von darstellungseinstellungen für alle Diagrammelemente.
- 3D Unterstützung für die meisten Diagrammtypen.
- Intelligente datenbeschriftungen, die automatisch auf Datenpunkte anpassen können.
- Bereichsstreifen skalierungsunterbrechungen und logarithmische Skalierung.
- Mehr als 50 Formeln für finanzielle und statistische Berechnungen zur Datenanalyse und -transformation
- Einfache Bindung und Bearbeitung von Diagrammdaten.
- Unterstützung für allgemeine Datenformate wie z. B. Datumsangaben, Uhrzeiten und Währungen.
- Unterstützung für Interaktivität und ereignisgesteuerte Anpassung, einschließlich Client click-Ereignisse, die mithilfe von Ajax.
- Zustandsverwaltung
- Binäres Streaming

Die folgenden Abbildungen zeigen Beispiele für finanzielle Diagramme, die vom Diagrammsteuerelement für ASP.NET erstellt werden.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Abbildung 2: Beispiele für ASP.NET-Diagramm

Weitere Beispiele zur Verwendung von ASP.NET Diagrammsteuerelement-Download im Beispielcode für die [Beispielumgebung für Microsoft Chart Controls](https://go.microsoft.com/fwlink/?LinkId=128300) Seite auf der MSDN-Website. Finden Sie weitere Beispiele für Community am Inhalt der [Chart Control Forum](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Hinzufügen von Chart-Steuerelement zu einer ASP.NET-Seite

Das folgende Beispiel veranschaulicht das Hinzufügen einer *Diagramm* Steuerelement zu einer ASP.NET-Seite mithilfe von Markup. Im Beispiel die *Diagramm* Steuerelement erzeugt ein Säulendiagramm für statische Datenpunkte.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Verwenden von 3D-Diagrammen

Die *Diagramm* Steuerelement enthält eine *ChartAreas* -Auflistung, die enthalten kann *ChartArea* Objekten, die Merkmale von Diagrammflächen zu definieren. Um 3D für einen Diagrammbereich zu verwenden, verwenden Sie z. B. die *Area3DStyle* Eigenschaft wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample59.aspx)]

Die folgende Abbildung zeigt ein 3D-Diagramm mit vier weitere Reihen mit dem *Leiste* Diagrammtyp.

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Abbildung 3: Balkendiagramm (3D)

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Mithilfe von Skalierungsunterbrechungen, logarithmischen Skalierungen

Skalierungsunterbrechungen und logarithmischen Skalierungen sind zwei weitere Verfahren zum Hinzufügen von Wissensstand für das Diagramm. Diese Funktionen sind spezifisch für jede Achse in einer Diagrammfläche. Verwenden Sie z. B. zur Verwendung dieser Funktionen auf der primären y-Achse einer Diagrammfläche die *AxisY.IsLogarithmic* und *ScaleBreakStyle* Eigenschaften in einem *ChartArea* Objekt. Der folgende Codeausschnitt zeigt, wie skalierungsunterbrechungen in der primären y-Achse verwendet wird.

[!code-aspx[Main](overview/samples/sample60.aspx)]

Die folgende Abbildung zeigt die y-Achse mit skalierungsunterbrechungen aktiviert.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Abbildung 4: Skalierungsunterbrechungen

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtern von Daten mit dem QueryExtender-Steuerelement

Eine häufige Aufgabe für Entwickler, die Datengesteuerte Webseiten zu erstellen, wird zum Filtern von Daten. Dies bisher ausgeführt wurde durch das Erstellen von *, in denen* Klauseln in Datenquellensteuerelemente. Dieser Ansatz kann kompliziert sein und in einigen Fällen die *, in denen* Syntax können Sie weder die vollständige Funktionalität der zugrunde liegenden Datenbank nutzen.

Um die Filterung zu erleichtern, ein neues *QueryExtender* Steuerelement in ASP.NET 4 hinzugefügt wurde. Dieses Steuerelement hinzugefügt werden kann *EntityDataSource* oder *LinqDataSource* Steuerelemente, um die von diesen Steuerelementen zurückgegebenen Daten zu filtern. Da die *QueryExtender* Steuerelement LINQ abhängig, auf dem Datenbankserver wird der Filter angewendet, bevor die Daten auf der Seite gesendet werden, die sehr effizient ausgeführt werden.

Die *QueryExtender* Steuerelement unterstützt eine Vielzahl von Optionen für Filter. In den folgenden Abschnitten werden diese Optionen beschrieben und enthalten Beispiele für deren Verwendung.

#### <a name="search"></a>Suchen

Für die Suchoption der *QueryExtender* -Steuerelement führt eine Suche in der angegebenen Felder. Im folgenden Beispiel verwendet das Steuerelement den Text, der in die TextBoxSearch-Steuerelement und sucht nach dessen Inhalt im eingegeben wurde die `ProductName` und `Supplier.CompanyName` Spalten in den Daten, die von zurückgegeben werden das *LinqDataSource* -Steuerelement.

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Bereich

Die Range (Option) ähnelt die Suchoption, aber es gibt ein Paar von Werten, die den Bereich zu definieren. Im folgenden Beispiel die *QueryExtender* steuern, sucht der `UnitPrice` Spalte in den Daten zurückgegeben, die von der *LinqDataSource* Steuerelement. Der Bereich ist die TextBoxFrom und TextBoxTo Steuerelemente auf der Seite gelesen werden.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

Die Property-Ausdruck-Option können Sie einen Vergleich mit einem Eigenschaftswert zu definieren. Wenn der ausgewertete Ausdruck *"true"*, die Daten, die untersucht wird zurückgegeben. Im folgenden Beispiel die *QueryExtender* Steuerelement filtert die Daten durch Vergleichen der Daten in der `Discontinued` -Spalte den Wert aus dem CheckBoxDiscontinued-Steuerelement auf der Seite.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>CustomExpression

Schließlich können Sie angeben, einen benutzerdefinierten Ausdruck für die Verwendung mit der *QueryExtender* Steuerelement. Diese Option können Sie eine Funktion auf der Seite aufrufen, die benutzerdefinierte Filterlogik definiert. Das folgende Beispiel zeigt, wie Sie deklarativ angeben, einen benutzerdefinierten Ausdruck in der *QueryExtender* Steuerelement.

[!code-aspx[Main](overview/samples/sample64.aspx)]

Das folgende Beispiel zeigt die benutzerdefinierte Funktion, die aufgerufen wird, indem Sie die *QueryExtender* Steuerelement. In diesem Fall anstelle der enthält eine Datenbankabfrage mit einem *, in dem* -Klausel, der Code verwendet eine LINQ-Abfrage zum Filtern der Daten.

[!code-csharp[Main](overview/samples/sample65.cs)]

Diese Beispiele zeigen nur ein Ausdruck verwendet wird, der *QueryExtender* Steuerelement zu einem Zeitpunkt. Sie können jedoch mehrere Ausdrücke in einschließen der *QueryExtender* Steuerelement.

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>HTML-codierte Codeausdrücke

Einige ASP.NET-Websites (insbesondere mit ASP.NET MVC) basieren auf der Verwendung stark `<%` =  `expression %>` Syntax (häufig als "Codeausdrücke" bezeichnet) zum Schreiben von Text in der Antwort. Bei Verwendung von Codeausdrücke ist es einfach, HTML-codieren Eingabe der Text, wenn der Text von Benutzer stammen, es Seiten Angriff (Cross-Site Scripting) XSS bestehen kann vergessen.

ASP.NET 4 werden die folgenden neue Syntax für Codeausdrücke eingeführt:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Diese Syntax wird das HTML-Codierung wird standardmäßig beim Schreiben in die Antwort verwendet. Diesen neuen Ausdruck ergibt effektiv Folgendes:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Beispielsweise &lt;%: Anforderung ["UserInput"] %&gt; ausführt, auf dem Wert des HTML-Codierung *anfordern ["UserInput"]*.

Das Ziel dieser Funktion ist es zu ermöglichen, um alle Instanzen der alten Syntax mit der neuen Syntax zu ersetzen, damit Sie nicht bei jedem Schritt entscheiden, welcher Typ verwendet erzwungen werden. Es gibt jedoch Fälle, in denen der Text, der die Ausgabe HTML vorgesehen ist oder bereits codiert ist, in diesem Fall kann dies zu einer doppelte Kodierung führen.

Für diese Fälle wird ASP.NET 4 eingeführt, eine neue Schnittstelle *IHtmlString*, zusammen mit der eine konkrete Implementierung *HtmlString*. Instanzen dieser Typen können Sie angeben, dass der Rückgabewert ist bereits ordnungsgemäß codiert (oder andernfalls untersucht) für die Anzeige als HTML und daher der Wert nicht HTML-codierte erneut liegen. Z. B. Folgendes nicht sein (und nicht) HTML-codiert:

[!code-aspx[Main](overview/samples/sample68.aspx)]

ASP.NET MVC 2-Hilfsmethoden wurden aktualisierte mit dieser neuen Syntax funktioniert, damit sie nicht sind doppelte codiert, jedoch nur, beim Ausführen von ASP.NET 4. Dieser neuen Syntax funktioniert nicht, wenn Sie eine Anwendung mithilfe von ASP.NET 3.5 SP1 ausgeführt werden.

Bedenken Sie, dass dies nicht, dass der Schutz vor XSS-Angriffen garantiert. Beispielsweise kann HTML, die Attributwerte verwendet werden, die nicht in Anführungszeichen eingeschlossen sind Benutzereingaben enthalten, immer noch anfällig sind. Beachten Sie, dass die Ausgabe der ASP.NET-Steuerelemente und ASP.NET MVC-Hilfsprogrammen Attributwerte immer in Anführungszeichen ein, enthält die ist die empfohlene Vorgehensweise.

Dagegen führt diese Syntax nicht aus JavaScript-Codierung, z. B. beim Erstellen einer JavaScript-Zeichenfolge, die basierend auf Benutzereingaben.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Projekt-Vorlagenänderungen

In früheren Versionen von ASP.NET bei der Verwendung von Visual Studio, erstellen Sie ein neues Websiteprojekt oder Webanwendungsprojekt, die sich ergebende Projekte enthalten nur eine Seite "default.aspx", den Standardwert `Web.config` Datei, und die `App_Data` Ordner, wie im folgenden gezeigt. Abbildung:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio unterstützt auch eine leere Website-Projekttyp, der keine Dateien enthält, wie in der folgenden Abbildung gezeigt:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

Das Ergebnis ist, dass für die Anfänger kaum Leitfaden zum Erstellen einer Produktions-Web-Anwendung vorhanden ist. Aus diesem Grund wird ASP.NET 4 eingeführt, drei neue Vorlagen, die für ein leeres Projekt für die Web-Anwendung und die jeweils eine für eine Webanwendung und die Website-Projekt.

#### <a name="empty-web-application-template"></a>Leere Web Application-Vorlage

Wie der Name schon sagt, ist die leere Web Application-Vorlage ein abgespeckte Webanwendungsprojekt. Sie wählen diese Projektvorlage klicken Sie im Dialogfeld Neues Projekt in Visual Studio, wie in der folgenden Abbildung gezeigt:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](overview/_static/image8.png))

Wenn Sie eine leere ASP.NET-Webanwendung erstellen, erstellt Visual Studio die folgende Ordnerstruktur:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Dieser Vorgang ähnelt dem Layout leere Website aus früheren Versionen von ASP.NET verwenden, mit einer Ausnahme. In Visual Studio 2010 leere Web-Anwendung und leere Website-Projekte enthalten die folgenden minimalem `Web.config` Datei, die Informationen, die von Visual Studio verwendet, um das Framework zu identifizieren, die das Projekt abzielt enthält:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Ohne diese *TargetFramework* -Eigenschaft, Visual Studio standardmäßig die .NET Framework 2.0 abzielen, um Kompatibilität zu gewährleisten, wenn Sie ältere Anwendungen zu öffnen.

#### <a name="web-application-and-web-site-project-templates"></a>Webanwendung und Website-Projektvorlagen

Die anderen zwei neue Projektvorlagen, die mit Visual Studio 2010 geliefert werden enthalten wichtige Änderungen an. Die folgende Abbildung zeigt die Projekt-Layouts, die erstellt wird, wenn Sie ein neues Webanwendungsprojekt zu erstellen. (Das Layout für das Websiteprojekt ist nahezu identisch.)

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

Das Projekt enthält eine Reihe von Dateien, die nicht in früheren Versionen erstellt wurden. Darüber hinaus wird das neue Webanwendungsprojekt mit Basis-Mitgliedschaft-Funktionalität konfiguriert letztere Schnelleinstieg beim Sichern des Zugriffs auf die neue Anwendung. Aufgrund dieser Aufnahme der `Web.config` -Konfigurationsdatei für das neue Projekt Einträge enthält, die zum Konfigurieren von Mitgliedschaft, Rollen und Profile verwendet werden. Das folgende Beispiel zeigt die `Web.config` -Datei für eine neue Webanwendungsprojekt. (In diesem Fall *RoleManager* ist deaktiviert.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](overview/_static/image14.png))

Das Projekt enthält außerdem ein zweites `Web.config` in der Datei die `Account` Verzeichnis. Die zweite Konfigurationsdatei bietet eine Möglichkeit zum Schützen des Zugriffs auf die Seite ChangePassword.aspx für Benutzer nicht angemeldet ist. Das folgende Beispiel zeigt den Inhalt des zweiten `Web.config` Datei.

![](overview/_static/image15.png)

Die Seiten erstellt standardmäßig in die neue Projektvorlagen enthalten auch mehr Inhalt als in früheren Versionen. Das Projekt enthält eine Standardseite "master" und der CSS-Datei, und die Standardseite ("default.aspx") ist die Gestaltungsvorlage verwenden standardmäßig konfiguriert. Das Ergebnis ist, dass wenn Sie die Webanwendung oder die Website zum ersten Mal ausführen, die Standardseite (home) noch funktionsfähig ist. Tatsächlich ähnelt die Standardseite, die angezeigt werden, wenn Sie eine neue MVC-Anwendung zu starten.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](overview/_static/image18.png))

Die Absicht dieser Änderungen zu den Projektvorlagen werden Anleitungen zum Erstellen einer neuen Webanwendung starten bereit. Mit semantisch richtig sind, strenge XHTML 1.0 kompatiblen Markup und Layout, die Verwendung von CSS angegeben ist, stellen die Seiten in den Vorlagen bewährte Methoden zum Erstellen von ASP.NET 4 Web-Anwendungen dar. Der Standardseiten verfügen auch einem zweispalten Layout, die Sie leicht anpassen können.

Angenommen Sie, dass für eine neue Web-Anwendung einige der Farben ändern, und fügen Sie Ihr Firmenlogo anstelle des Logos meine ASP.NET-Anwendung werden sollen. Zu diesem Zweck erstellen Sie ein neues Verzeichnis unter `Content` Ihr Firmenlogo speichern:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Um das Bild zur Seite hinzuzufügen, öffnen Sie dann die `Site.Master` Datei, suchen, in denen Text meine ASP.NET-Anwendung definiert ist, und ersetzen es durch ein *Image* Element, dessen *Src* -Attribut auf das neue Logo festgelegt ist Bild, wie im folgenden Beispiel gezeigt:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](overview/_static/image22.png))

Sie können ein Wechsel in die Datei "Site.CSS" ändern und Ändern von CSS-Klassendefinitionen, um die Hintergrundfarbe der Seite sowie, des Headers zu ändern.

Das Ergebnis dieser Änderungen ist, dass Sie eine benutzerdefinierte Startseite mit relativ geringem Aufwand anzeigen können:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Klicken Sie hier, um das Bild in voller Größe angezeigt](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>CSS-Verbesserungen

Einer der wesentlichen Bereiche der Arbeit in ASP.NET 4 wurde die Hilfe HTML zu rendern, die mit den neuesten HTML-Standards kompatibel ist. Dies umfasst Änderungen an der Verwendung von ASP.NET Web-Serversteuerelemente CSS-Formatvorlagen.

#### <a name="compatibility-setting-for-rendering"></a>Kompatibilitätsgrad für das Rendering

In der Standardeinstellung, wenn eine Webanwendung oder die Website der .NET Framework 4 abzielt der *ControlRenderingCompatibilityVersion* Attribut der *Seiten* -Element den Wert "4.0" festgelegt ist. Dieses Element wird in der Computerebene definiert `Web.config` Datei und in der Standardeinstellung gilt für alle ASP.NET 4-Anwendungen:

[!code-xml[Main](overview/samples/sample69.xml)]

Der Wert für *ControlRenderingCompatibility* ist eine Zeichenfolge, die in zukünftigen Versionen von potenziellen neuen Version Definitionen ermöglicht. In der aktuellen Version werden die folgenden Werte für diese Eigenschaft unterstützt:

- "3.5". Diese Einstellung gibt an, ältere Rendering- und Markup. Markup Rendern von Steuerelementen ist abwärtskompatibel ist 100 %, und die Einstellung von der *XhtmlConformance* Eigenschaft berücksichtigt wird.
- "4.0". Wenn die Eigenschaft dieser Einstellung hat, gehen Sie Serversteuerelemente von ASP.NET Web:
- Die *XhtmlConformance* Eigenschaft wird immer als "Strict" behandelt. Steuerelemente werden daher XHTML 1.0 strikte Markup rendern.
- Deaktivieren nicht zur Eingabe Steuerelemente nicht mehr rendert ungültige Formate.
- *Div* Elemente um ausgeblendete Felder sind jetzt formatiert, sodass sie nicht mit benutzerdefinierten CSS-Regeln in Konflikt treten.
- Menüsteuerelemente Rendern Markup, das semantisch richtig und mit Richtlinien für Eingabehilfen kompatibel ist.
- Validierungssteuerelemente werden Inlineformatvorlagen nicht gerendert.
- Steuert, die zuvor gerendert Border = "0" (Steuerelemente, die von ASP.NET abgeleitet werden *Tabelle* -Steuerelement und das ASP.NET *Image* Steuerelement) dieses Attribut nicht mehr zu rendern.

#### <a name="disabling-controls"></a>Deaktivieren von Steuerelementen

In ASP.NET 3.5 SP1 und früheren Versionen wird das Framework rendert die *deaktiviert* -Attribut im HTML-Markup für jedes, dessen Steuerelement *aktiviert* -Eigenschaftensatz auf *"false"*. Jedoch gemäß der HTML 4.01-Spezifikation nur *input* Elemente sollte dieses Attribut aufweisen.

Sie können in ASP.NET 4 hat das Festlegen der *ControlRenderingCompatabilityVersion* Eigenschaft auf "3.5", wie im folgenden Beispiel:

[!code-xml[Main](overview/samples/sample70.xml)]

Möglicherweise erstellen Sie Markup für eine *Bezeichnung* Steuerelement wie folgt aus, die das Steuerelement deaktiviert:

[!code-aspx[Main](overview/samples/sample71.aspx)]

Die *Bezeichnung* Rendern Steuerelements folgenden HTML-Code:

[!code-html[Main](overview/samples/sample72.html)]

Sie können in ASP.NET 4 hat das Festlegen der *ControlRenderingCompatabilityVersion* den Wert "4.0". In diesem Fall nur Steuerelemente gerendert *input* Elemente gerendert werden eine *deaktiviert* -Attribut festlegen, wenn des Steuerelements *aktiviert* -Eigenschaftensatz auf *"false"* . Steuerelemente, die nicht HTML gerendert werden *input* stattdessen Rendern der Elemente einer *Klasse* -Attribut, das eine CSS-Klasse, die Sie verwenden können verweist, um eine deaktivierte suchen für das Steuerelement zu definieren. Z. B. die *Bezeichnung* Steuerelement angezeigt, in dem oben aufgeführten Beispiel würde das folgende Markup generieren:

[!code-html[Main](overview/samples/sample73.html)]

Der Standardwert für die Klasse, die für dieses Steuerelement angegeben wird "AspNetDisabled". Sie können diesen Standardwert jedoch ändern, durch Festlegen der statischen *DisabledCssClass* statische Eigenschaft von der *WebControl* Klasse. Für Entwickler von Steuerelementen, das Verhalten für ein bestimmtes Steuerelement verwenden kann auch definiert werden mithilfe der *SupportsDisabledAttribute* Eigenschaft.

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Div-Elemente um ausgeblendete Felder ausblenden

ASP.NET 2.0 und höheren Versionen rendern systemspezifische verborgene Felder (z. B. die *ausgeblendete* Element zum Speichern der Ansichtszustandsinformationen) innerhalb *Div* Element, um die XHTML-Standard entsprechen. Allerdings kann dies ein Problem verursachen, wenn Sie eine CSS-Regel wirkt sich auf *Div* Elemente auf einer Seite. Z. B. in einer einem Pixel Zeile angezeigt, die auf der Seite werden dadurch ca. ausgeblendet *Div* Elemente. In ASP.NET 4 *Div* Elemente, die die ausgeblendeten Felder, die von ASP.NET generiert einschließen hinzufügen Verweis auf eine CSS-Klasse wie im folgenden Beispiel gezeigt:

[!code-html[Main](overview/samples/sample74.html)]

Sie können eine CSS-Klasse, die gilt nur dann definieren die *ausgeblendete* Elemente, die von ASP.NET, wie im folgenden Beispiel gezeigt generiert werden:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Rendern einer äußeren Tabelle für Vorlagenbasierte Steuerelemente

Standardmäßig werden die folgenden ASP.NET Web-Server-Steuerelemente, die Vorlagen unterstützen automatisch in einer äußeren Tabelle umschlossen, die verwendet wird, um Inlineformatvorlagen anzuwenden:

- *FormView*
- *Anmeldung*
- *PasswordRecovery*
- *ChangePassword*
- *Assistenten*
- *CreateUserWizard*

Eine neue Eigenschaft mit dem Namen *RenderOuterTable* wurde hinzugefügt, um diese Steuerelemente, die von der äußeren Tabelle aus dem Markup entfernt werden können. Betrachten Sie beispielsweise das folgende Beispiel für eine *FormView* Steuerelement:

[!code-aspx[Main](overview/samples/sample76.aspx)]

Dieses Markup gerendert wird, auf der Seite, die eine HTML-Tabelle enthält die folgende Ausgabe:

[!code-html[Main](overview/samples/sample77.html)]

Um zu verhindern, dass die Tabelle gerendert wird, legen Sie die *FormView* des Steuerelements *RenderOuterTable* -Eigenschaft, wie im folgenden Beispiel gezeigt:

[!code-aspx[Main](overview/samples/sample78.aspx)]

Im vorherige Beispiel rendert die folgende Ausgabe, ohne die *Tabelle*, *tr*, und *td* Elemente:

> Inhalt


Dank dieser Erweiterung kann der Inhalt des Steuerelements mit CSS-Stil vereinfachen, da keine unerwarteten Tags vom Steuerelement gerendert werden.

> [!NOTE]
> Beachten Sie diese Änderung wird Unterstützung für die AutoFormat Funktion in der Visual Studio 2010-Designer deaktiviert, da es nicht mehr ist eine *Tabelle* -Element, das Stilattribute hosten kann, die durch die Option Auto-Format generiert werden.


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>ListView-Steuerelement-Erweiterungen

Die *ListView* Steuerelement einfacher zu verwenden in ASP.NET 4 vorgenommen wurden. Die frühere Version des Steuerelements erforderlich, dass Sie eine Layoutvorlage aus angeben, die ein Serversteuerelement mit einer bekannten ID enthalten Das folgende Markup zeigt ein typisches Beispiel zum Verwenden der *ListView* -Steuerelement in ASP.NET 3.5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

In ASP.NET 4 hat das *ListView* Steuerelement erfordert keine Layoutvorlage aus. Das Markup, das im vorherigen Beispiel gezeigt, kann durch Folgendes Markup ersetzt werden:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>CheckBoxList und RadioButtonList-Steuerelement-Erweiterungen

In ASP.NET 3.5 verwendet wird, können Sie angeben, Layout für die *CheckBoxList* und *RadioButtonList* mithilfe der folgenden beiden Einstellungen:

- *Flow*. Rendert das Steuerelement *umfassen* Elemente, die den Inhalt enthalten.
- *Tabelle*. Rendert das Steuerelement eine *Tabelle* Element, dessen Inhalt enthalten.

Das folgende Beispiel enthält Markup für jedes dieser Steuerelemente.

[!code-aspx[Main](overview/samples/sample81.aspx)]

Wird standardmäßig Rendern die Steuerelemente HTML folgt oder ähnlich:

[!code-html[Main](overview/samples/sample82.html)]

Da diese Steuerelemente Listen von Elementen, die zum Rendern des semantisch richtige HTML enthalten sollten sie ihre Inhalte mit HTML-Liste rendern (*li*) Elemente. Dies erleichtert es für Benutzer, die Webseiten mit hilfstechnologie lesen und erleichtert die Steuerelemente auf CSS-Format.

In ASP.NET 4 hat das *CheckBoxList* und *RadioButtonList* -Steuerelemente unterstützen die folgenden neuen Werte für die *unterstützt* Eigenschaft:

- *OrderedList* – Inhalt wird als gerendert *li* Elemente innerhalb einer *Ol* Element.
- *UnorderedList* – Inhalt wird als gerendert *li* Elemente innerhalb einer *Ul* Element.

Im folgende Beispiel wird gezeigt, wie diese neuen Werte verwendet wird.

[!code-aspx[Main](overview/samples/sample83.aspx)]

Die vorangehende Markup generiert den folgenden HTML-Code:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Beachten Sie, wenn Sie festlegen, *unterstützt* auf *OrderedList* oder *UnorderedList*, *RepeatDirection* -Eigenschaft kann nicht mehr verwendet werden und wird Lösen Sie zur Laufzeit eine Ausnahme aus, wenn die Eigenschaft innerhalb Ihrer Markup oder-Code festgelegt wurde. Die Eigenschaft würde kein Wert vorhanden, da das visuelle Layout dieser Steuerelemente verwenden Sie stattdessen CSS definiert ist.


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Menü-Steuerelement-Verbesserungen

Bevor Sie ASP.NET 4 die *Menü* Steuerelement gerendert, eine Reihe von HTML-Tabellen. Dies macht es, schwieriger, Anwenden von CSS-Stilen außerhalb von Inline-Eigenschaften festlegen und auch mit nicht kompatibel war Barrierefreiheitsstandards.

In ASP.NET 4 rendert das Steuerelement nun HTML mithilfe von semantic Markup, das eine ungeordnete Liste und Listenelemente besteht. Das folgende Beispiel enthält Markup auf einer ASP.NET-Seite für die *Menü* Steuerelement.

[!code-aspx[Main](overview/samples/sample85.aspx)]

Wenn die Seite gerendert wird, erzeugt das Steuerelement die folgenden HTML-Code (die *Onclick* Code wurde aus Gründen der Übersichtlichkeit weggelassen):

[!code-html[Main](overview/samples/sample86.html)]

Zusätzlich zum Rendering-Verbesserungen wurde Tastaturnavigation des Menüs mit fokusverwaltung verbessert wurden. Wenn die *Menü* Steuerelement den Fokus erhält, können Sie die Pfeiltasten verwenden, um Elemente zu navigieren. Die *Menü* Steuerelement jetzt auch fügt zugänglich rich Internet Applications (ARIA) Rollen und Attribute ausge[Häufigkeit der](http://www.w3.org/TR/wai-aria-practices/#menu "Menü ARIA Richtlinien")für verbessert für die Barrierefreiheit.

Stile für das Menüsteuerelement werden in einem Stilblock am oberen Rand der Seite und nicht im Einklang mit den gerenderten HTML-Elementen gerendert werden. Wenn Sie vollständige Kontrolle über das Format für das Steuerelement nutzen möchten, legen Sie die neue *IncludeStyleBlock* Eigenschaft *"false"*, in diesem Fall die Style-Block nicht ausgegeben wird. Eine Möglichkeit, diese Eigenschaft verwenden, werden die AutoFormat Funktion in Visual Studio-Designer zu verwenden, um die Darstellung des Menüs festlegen. Sie können dann führen Sie die Seite, die Seitenquelle öffnen, und kopieren Sie den gerenderten Style-Block in einer externen CSS-Datei. In Visual Studio rückgängig machen, das Erstellen von Formaten und Set *IncludeStyleBlock* auf *"false"*. Das Ergebnis ist, dass die Darstellung des Menüs mit Formatvorlagen in einem externen Stylesheet definiert ist.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Assistenten und CreateUserWizard-Steuerelemente

Die ASP.NET *Assistenten* und *CreateUserWizard* Steuerelemente unterstützen Vorlagen, mit denen Sie den HTML-Code zu definieren, die sie rendern. (*CreateUserWizard* leitet sich von *Assistenten*.) Das folgende Beispiel zeigt das Markup für eine vollständig aus einer Vorlage gebildete *CreateUserWizard* Steuerelement:

[!code-aspx[Main](overview/samples/sample87.aspx)]

Das Steuerelement rendert HTML folgt oder ähnlich:

[!code-html[Main](overview/samples/sample88.html)]

In ASP.NET 3.5 SP1, obwohl Sie den Vorlageninhalt ändern, können Sie weiterhin nur über begrenzte Kontrolle über die Ausgabe von der *Assistenten* Steuerelement. In ASP.NET 4, erstellen Sie eine *LayoutTemplate* Vorlage und Insert *Platzhalter* steuert (unter Verwendung reservierte Namen), um anzugeben, wie Sie möchten die *Assistenten Steuerelement* zum Rendern. Das folgende Beispiel zeigt dies:

[!code-aspx[Main](overview/samples/sample89.aspx)]

Das Beispiel enthält die folgenden benannten Platzhalter in der *LayoutTemplate* Element:

- *HeaderPlaceholder* – zur Laufzeit wird dieser durch den Inhalt der ersetzt die *HeaderTemplate* Element.
- *SideBarPlaceholder* – zur Laufzeit wird dieser durch den Inhalt der ersetzt die *SideBarTemplate* Element.
- *WizardStepPlaceHolder* – zur Laufzeit wird dieser durch den Inhalt der ersetzt die *WizardStepTemplate* Element.
- *NavigationPlaceholder* – zur Laufzeit ersetzt diese durch Navigation Vorlagen, die Sie definiert haben.

Das Markup in dem Beispiel für das Verwendung von Platzhaltern rendert den folgenden HTML-Code (ohne den Inhalt, der tatsächlich in den Vorlagen definiert wird):

[!code-html[Main](overview/samples/sample90.html)]

Wird nur der HTML-Code jetzt keine benutzerdefinierten ist ein *umfassen* Element. (Wir erwarten, dass in zukünftigen Versionen noch die *umfassen* Element nicht gerendert werden.) Nun können Sie die vollständige Kontrolle über nahezu alle Inhalte, die von generiert wird die *Assistenten* Steuerelement.

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC wurde im März 2009 ein Add-on-Framework auf ASP.NET 3.5 SP1 eingeführt. Visual Studio 2010 umfasst ASP.NET MVC 2, enthält neue Features und Funktionen.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Unterstützung von Bereichen

Bereiche können Sie Gruppe Controller und Ansichten in Abschnitte einer großen Anwendung relative isoliert von anderen Abschnitten. Jeder Bereich kann als ein separates Projekt für ASP.NET-MVC implementiert werden, die dann von der hauptanwendung verwiesen werden kann. Dies hilft, Komplexität bei der Erstellung einer großen Anwendung zu verwalten und für mehrere Teams zusammenarbeiten auf eine einzelne Anwendung vereinfacht.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Validierungsunterstützung für Datenanmerkung Attribut

*DataAnnotations* Attributen können Sie ein Modell mithilfe von Metadatenattributen Validierungslogik zuordnen. *DataAnnotations* Attribute in ASP.NET Dynamic Data in ASP.NET 3.5 SP1 eingeführt wurden. Diese Attribute in den Standardmodellbinder integriert und bieten eine Möglichkeit metadatengesteuerte, um Benutzereingaben zu überprüfen.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Mit Hilfsvorlagen

Mit Hilfsvorlagen können Sie automatisch zuordnen bearbeiten und Vorlagen mit Datentypen anzuzeigen. Beispielsweise können Sie eine Hilfsvorlage anzeigt um anzugeben, dass ein Element der Datumsauswahl Benutzeroberfläche automatisch für gerendert wird eine *System.DateTime* Wert. Dieser Vorgang ähnelt Feldvorlagen in ASP.NET Dynamic Data.

Die *Html.EditorFor* und *Html.DisplayFor* Hilfsmethoden bieten eine integrierte Unterstützung für Standarddaten Rendern sowie die komplexen Objekten mit mehreren Eigenschaften Typen. Sie auch Rendering anpassen, indem Sie datenanmerkung Attribute wie anwenden lassen *DisplayName* und *ScaffoldColumn* auf die *ViewModel* Objekt.

Häufig möchten Sie die Ausgabe von UI-Hilfsprogramme noch weiter anpassen und haben vollständige Kontrolle über was generiert wird. Die *Html.EditorFor* und *Html.DisplayFor* Hilfsmethoden unterstützt diese mit einem Datenvorlagen-Mechanismus, mit dem Sie die externe Vorlagen, die überschrieben werden können, und die Steuerung der gerenderten Ausgabe zu definieren. Die Vorlagen können einzeln für eine Klasse gerendert werden.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Dynamic Data

Dynamische Daten eingeführt wurde in der .NET Framework 3.5 SP1-Version in der Mitte 2008. Diese Funktion bietet zahlreiche Verbesserungen für das Erstellen eines datengesteuerten Anwendungen, einschließlich der folgenden:

- Ein RAD praktisches Beispiel für das schnelle Erstellen einer Website eines datengesteuerten.
- Automatische Überprüfung, die auf Einschränkungen definiert im Datenmodell basiert.
- Die Fähigkeit, einfach das Markup zu ändern, die generiert wird, für Felder in der *GridView* und *DetailsView* Steuerelemente mithilfe von Feldvorlagen, die Teil des Dynamic Data-Projekts sind.

> [!NOTE]
> Notieren Sie sich für Weitere Informationen, finden Sie unter der [Dynamic Data-Dokumentation](https://msdn.microsoft.com/library/cc488545.aspx) in der MSDN Library.


Für ASP.NET 4 wurde Dynamic Data verbessert, um Entwickler noch mehr Leistung für das schnelle Erstellen von datengesteuerten Websites.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Aktivieren dynamische Daten für vorhandene Projekte

Dynamische Data-Funktionen, die in .NET Framework 3.5 SP1 geliefert geschaltet neue Features wie die folgenden:

- Feldvorlagen – bieten diese Data-Type-basierte Vorlagen für datengebundene Steuerelemente. Feld-Vorlagen stellen eine einfachere Möglichkeit zum Anpassen der Darstellung von Data-Steuerelemente als die Verwendung von Vorlagenfelder für jedes Feld.
- Überprüfung – dynamische Daten können Sie die Attribute auf Datenklassen zu verwenden, um die Validierung für häufig vorkommende Szenarien wie erforderliche Felder, bereichsprüfung oder die Überprüfung, typüberprüfung und Mustervergleich mit regulären Ausdrücken und benutzerdefinierten Validierungstyp angeben. Überprüfung wird durch die Datensteuerelemente erzwungen.

Diese Funktionen waren jedoch die folgenden Anforderungen:

- Die Datenzugriffsschicht musste basiert auf Entity Framework oder LINQ to SQL.
- Die einzige Datenquelle Steuerelemente, die für diese Funktionen wurden unterstützt die *EntityDataSource* oder *LinqDataSource* Steuerelemente.
- Die Features erforderlich, ein Webprojekt, die die Dynamic Data oder Dynamic Data Entities-Vorlagen verwenden, um alle Dateien haben, die erforderlich waren, zur Unterstützung der Funktion erstellt wurde.

Ein Hauptziel Dynamic Data-Unterstützung in ASP.NET 4 werden die neue Funktionen von ASP.NET Dynamic Data für eine ASP.NET-Anwendung aktivieren. Im folgenden Beispiel wird das Markup für Steuerelemente, die Dynamic Data-Funktionen in einer vorhandenen Seite nutzen können.

[!code-aspx[Main](overview/samples/sample91.aspx)]

Im Code für die Seite muss der folgende Code hinzugefügt werden, um Dynamic Data-Unterstützung für diese Steuerelemente zu aktivieren:

[!code-csharp[Main](overview/samples/sample92.cs)]

Wenn die *GridView* -Steuerelement befindet sich im Bearbeitungsmodus wechseln, Dynamic Data automatisch überprüft, ob die eingegebenen Daten das richtige Format. Wenn sie nicht der Fall ist, wird eine Fehlermeldung angezeigt.

Diese Funktionalität bietet darüber hinaus weitere Vorteile, z. B. befindet er an Standardeinstellung können Werte für die Einfügemodus. Ohne Dynamic Data implementieren Sie einen Standardwert für ein Feld muss an ein Ereignis anfügen, suchen Sie das Steuerelement (mit *FindControl*), und legen Sie dessen Wert. In ASP.NET 4 hat das *EnableDynamicData* -Aufruf unterstützt einen zweiten Parameter, mit dem Sie die Standardwerte für jedes Feld für das Objekt zu übergeben, wie im folgenden Beispiel gezeigt:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Deklarative DynamicDataManager-Steuerelement-Syntax

Die *DynamicDataManager* Steuerelement wurde verbessert, damit Sie deklarativ konfigurieren können wie bei den meisten Steuerelementen in ASP.NET und nicht nur im Code. Das Markup für die *DynamicDataManager* -Steuerelement sieht wie im folgenden Beispiel:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Dieses Markup aktiviert Dynamic Data-Verhalten für das GridView1-Steuerelement, auf das verweist, die *Datensteuerelements* Teil der *DynamicDataManager* Steuerelement.

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Entitätsvorlagen

Entitätsvorlagen bieten eine neue Methode zum Anpassen des Layouts von Daten, ohne dass Sie eine benutzerdefinierte Seite erstellen. Seite-Vorlagen verwenden den *FormView* Steuerelement (anstelle von der *DetailsView* zu steuern, wie Sie in Vorlagen in früheren Versionen von ASP.NET Dynamic Data verwendet) und die *erzeugt wird* Steuerelement Entitätsvorlagen gerendert werden soll. Dies bietet Ihnen mehr Kontrolle über das Markup, das von Dynamic Data gerendert wird.

Die folgende Liste enthält das neue Projekt Verzeichnisstruktur, das Entitätsvorlagen enthält:

[!code-console[Main](overview/samples/sample95.cmd)]

Die `EntityTemplate` Verzeichnis enthält Vorlagen für Data Model-Objekte anzeigen. Standardmäßig werden Objekte gerendert, indem Sie die `Default.ascx` Vorlage, die Markup, das aussieht wie das Markup erstellt enthält, indem die *DetailsView* Steuerelement durch dynamische Daten in ASP.NET 3.5 SP1 verwendet. Das folgende Beispiel zeigt das Markup für die `Default.ascx` Steuerelement:

[!code-aspx[Main](overview/samples/sample96.aspx)]

Die Standardvorlagen können bearbeitet werden, um das Aussehen und Verhalten für den gesamten Standort zu ändern. Es sind Vorlagen für das anzeigen, bearbeiten und Insert-Vorgänge. Neue Vorlagen können basierend auf den Namen des Datenobjekts hinzugefügt werden, um das Aussehen und Verhalten der nur eine Art von Objekt zu ändern. Beispielsweise können Sie die folgende Vorlage hinzufügen:

[!code-console[Main](overview/samples/sample97.cmd)]

Die Vorlage enthält möglicherweise das folgende Markup:

[!code-aspx[Main](overview/samples/sample98.aspx)]

Die neue Entitätsvorlagen werden auf einer Seite angezeigt, mit dem neuen *erzeugt wird* Steuerelement. Dieses Steuerelement wird zur Laufzeit mit dem Inhalt der Entitätsvorlage ersetzt. Das folgende Markup zeigt die *FormView* steuern, der `Detail.aspx` Seitenvorlage aus, die Entitätsvorlage verwendet. Beachten Sie, dass die *erzeugt wird* Element im Markup.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-email-addresses"></a>Neues Feldvorlagen für URLs und e-Mail-Adressen

ASP.NET 4 enthält zwei neue integrierte Feldvorlagen `EmailAddress.ascx` und `Url.ascx`. Diese Vorlagen werden als markierten Felder zum *EmailAddress* oder *Url* mit der *DataType* Attribut. Für *EmailAddress* -Objekten, das Feld wird angezeigt, wie ein Link, der erstellt wird die *Mailto:* Protokoll. Wenn Benutzer auf den Link klicken, öffnet der Benutzer-e-Mail-Client und erstellt eine Skelette-Nachricht. Als typisierte Objekte *Url* werden als gewöhnliche Links angezeigt.

Das folgende Beispiel zeigt, wie Felder gekennzeichnet werden sollen.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Zum Erstellen von Verknüpfungen mit dem DynamicHyperLink-Steuerelement

Dynamic Data verwendet die neue routing-Funktion, die in der .NET Framework 3.5 SP1, um die URLs zu steuern, die Endbenutzern angezeigt, wenn sie Zugriff auf die Website hinzugefügt wurde. Die neue *DynamicHyperLink* Steuerelement erleichtert das Erstellen von Links zu Seiten in einer Dynamic Data-Website. Das folgende Beispiel zeigt, wie Sie die *DynamicHyperLink* Steuerelement:

[!code-aspx[Main](overview/samples/sample101.aspx)]

Dieses Markup erstellt einen Link, der auf der Listenseite für zeigt die `Products` Tabelle auf der Grundlage von Routen, die in definierten der `Global.asax` Datei. Das Steuerelement verwendet automatisch den Standardnamen für die Tabelle, dem auf die Seite dynamische Daten basieren.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Unterstützung von Vererbung im Datenmodell

Das Entity Framework und die LINQ to SQL unterstützen die Vererbung in ihren Datenmodellen. Ein Beispiel hierfür ist möglicherweise eine Datenbank mit einer `InsurancePolicy` Tabelle. Er enthält möglicherweise auch `CarPolicy` und `HousePolicy` Tabellen, die die gleichen als Felder `InsurancePolicy` und fügen Sie weitere Felder hinzu. Dynamic Data wurde geändert, um geerbten Objekte im Datenmodell zu verstehen und Gerüstbau für die geerbten Tabellen unterstützen.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Unterstützung für viele-zu-viele-Beziehungen (nur Entity Framework)

Das Entity Framework bietet umfangreiche Unterstützung für die m: n-Beziehungen zwischen Tabellen, die implementiert wird, verfügbar machen, die Beziehung als Auflistung auf eine *Entität* Objekt. Neue `ManyToMany.ascx` und `ManyToMany_Edit.ascx` Feldvorlagen bieten Unterstützung für die Anzeige und Bearbeitung von Daten, die viele-zu-viele-Beziehungen zusammenhängt hinzugefügt wurden.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Neue Attribute zu Anzeige- und Unterstützung von Enumerationen

Die *DisplayAttribute* wurde hinzugefügt, um Sie bieten zusätzliche Kontrolle darüber, wie Felder angezeigt werden. Die *DisplayName* Attribut in früheren Versionen von ASP.NET Dynamic Data erlaubt Ihnen, den Namen zu ändern, die als Beschriftung für ein Feld verwendet wird. Die neue *DisplayAttribute* Klasse ermöglicht die Angabe weitere Anzeigeoptionen für ein Feld, z. B. die Reihenfolge, in der ein Feld angezeigt wird und ob ein Feld als Filter verwendet wird. Das Attribut bietet auch eine unabhängige Kontrolle über den Namen für die Bezeichnungen in einer *GridView* zu steuern, den Namen in verwendet eine *DetailsView* zu steuern, den Hilfetext für das Feld und das Wasserzeichen für verwendet die Feld (wenn das Feld Texteingabe akzeptiert).

Die *EnumDataTypeAttribute* Klasse wurde hinzugefügt, mit der Sie die Felder Enumerationen zuordnen können. Wenn Sie dieses Attribut auf ein Feld anwenden, geben Sie einen Enumerationstyp. Dynamic Data verwendet die neue `Enumeration.ascx` Feldvorlage Benutzeroberfläche zum Anzeigen und Bearbeiten von Enumerationswerten erstellt. Die Vorlage ordnet die Werte aus der Datenbank den Namen in der Enumeration.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Verbesserte Unterstützung für Filter

Dynamische Daten 1.0 Lieferumfang integrierte Filter für boolesche Spalten und Fremdschlüsselspalten ab. Die Filter hat nicht zugelassen, dass Sie angeben, ob sie angezeigt wurden, oder in welcher Reihenfolge sie angezeigt wurden. Die neue *DisplayAttribute* Attribut Adressen beide dieser Probleme ermöglicht die Kontrolle über gibt an, ob eine Spalte als Filter angezeigt wird und in welcher Reihenfolge sie angezeigt wird.

Eine weitere Verbesserung ist, dass Filtern Unterstützung re[geschrieben werden, um den neuen](#0.2__QueryExtender "_QueryExtender") Funktion von Web Forms. Dadurch können Sie Filter erstellen, ohne Wissen des Datenquellen-Steuerelements, das mit die Filtern verwendet wird. Zusammen mit diesen Erweiterungen haben Filter auch in Vorlagensteuerelemente aktiviert wurde, damit Sie neue Spalten hinzufügen können. Schließlich die *DisplayAttribute* Klasse, die zuvor erwähnten ermöglicht den Standardfilter überschrieben werden, auf die gleiche Weise *UIHint* Feld Standardvorlage für eine Spalte aus, außer Kraft gesetzt werden können.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Visual Studio 2010 Web Development-Verbesserungen

Webentwicklung in Visual Studio 2010 wurde auf größer CSS-Kompatibilität und steigert die Produktivität durch HTML- und ASP.NET Markup Ausschnitte und neue dynamische IntelliSense JavaScript verbessert.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Verbesserte CSS-Kompatibilität

Visual Web Developer-Designer in Visual Studio 2010 wurde aktualisiert, um die Einhaltung von Standards CSS 2.1 zu verbessern. Der Designer behält die Integrität der HTML-Quelle eine bessere Leistung und ist robuster als in früheren Versionen von Visual Studio. Hinter den Kulissen verbesserten auch zukünftige Erweiterungen Rendering, Layout und Wartungsfreundlichkeit besser aktivieren wird wurden.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>HTML und JavaScript-Ausschnitte

In der HTML-Editor werden automatisch vervollständigt IntelliSense Tagnamen. Die IntelliSense-Codeausschnitte-Funktion werden automatisch vervollständigt ganze Tags und vieles mehr. In Visual Studio 2010 werden IntelliSense-Codeausschnitten unterstützt, für JavaScript, zusammen mit c# und Visual Basic, die in früheren Versionen von Visual Studio unterstützt wurden.

Visual Studio 2010 umfasst mehr als 200 Ausschnitte, mit denen Sie AutoVervollständigen common ASP.NET und HTML-Tags, einschließlich erforderlichen Attribute (z. B. Runat = "Server") und allgemeiner Attribute, die spezifisch für einen Tag (z. B. *ID*,  *DataSourceID*, *ControlToValidate*, und *Text*).

Sie können zusätzliche Ausschnitte herunterladen, oder Sie können eigene Ausschnitte, die die Blöcke mit Markup, mit denen Sie oder Ihr Team für häufige Aufgaben zu kapseln schreiben.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>JavaScript-IntelliSense-Erweiterungen

In Visual 2010 wurde JavaScript IntelliSense überarbeitet, um eine noch umfassendere Bearbeitungsoberfläche bereitzustellen. IntelliSense erkennt jetzt Objekte, die dynamisch durch Methoden wie z. B. generiert wurden *RegisterNamespace* und ähnliche Verfahren, die von anderen JavaScript-Frameworks verwendet. Leistung wurde verbessert, um große Bibliotheken des Skripts zu analysieren und mit wenigen oder gar keinen verarbeitungsverzögerung von IntelliSense angezeigt. Kompatibilität wurde zur Unterstützung von nahezu alle Drittanbieter-Bibliotheken und zur Unterstützung von verschiedenartige erheblich erhöht. Dokumentationskommentare werden jetzt analysiert, während der Eingabe und sofort von IntelliSense genutzt werden.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Bereitstellung von Webanwendungen mit Visual Studio 2010

Wenn Sie ASP.NET-Entwickler eine Anwendung bereitstellen, finden sie häufig, dass sie z. B. folgende Probleme auftreten:

- Bereitstellung auf einer freigegebenen hosting-Site benötigen Technologien wie FTP, die langsam sein kann. Darüber hinaus müssen Sie manuell Aufgaben wie das Ausführen von SQL-Skripts zum Konfigurieren einer Datenbank ausführen und müssen Sie IIS-Einstellungen, z. B. ein virtuelles Verzeichnis konfigurieren, wie eine Anwendung ändern.
- Administratoren müssen häufig in einer unternehmensumgebung, zusätzlich zur Bereitstellung der Webanwendungsdateien von Konfigurationsdateien für ASP.NET und IIS-Einstellungen ändern. Datenbankadministratoren müssen eine Reihe von SQL-Skripts zum Abrufen der Anwendungsdatenbank ausführen ausführen. Solche Installationen sind arbeitsintensiv, häufig Stunden dauern, und muss sorgfältig dokumentiert werden.

Visual Studio 2010 umfasst Technologien, die diese Probleme beheben und, mit deren Hilfe Sie nahtlos deploy Web Applications. Keines dieser Technologien handelt es sich um das IIS-Webbereitstellungstool (MsDeploy.exe).

Web-Bereitstellungsfunktionen in Visual Studio 2010 umfassen die folgenden Hauptbereichen:

- Web packen
- Web.config-transformation
- Datenbankbereitstellung
- One-Click-Veröffentlichung für Webanwendungen

Die folgenden Abschnitte enthalten Details zu diesen Features.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Web packen

Visual Studio 2010 verwendet der MSDeploy-Tool, um eine komprimierte Datei (.zip) für Ihre Anwendung erstellen, die so genannte eine *Webpaket*. Die Paketdatei enthält Metadaten über die Anwendung sowie den folgenden Inhalt:

- IIS-Einstellungen, mit denen Einstellungen des Anwendungspools, seiteneinstellungen für Fehler- und usw. enthält.
- Die tatsächliche Webinhalte, einschließlich Webseiten, Benutzersteuerelemente, statische Inhalte (Bilder und HTML-Dateien) und So weiter.
- SQL Server-Datenbank-Schemas und Daten.
- Sicherheitszertifikate, Komponenten für die Installation im GAC, registrierungseinstellungen und So weiter.

Ein Webpaket kann auf einem beliebigen Server kopiert und dann manuell mithilfe des IIS-Managers installiert werden. Alternativ kann für die automatisierte Bereitstellung des Pakets mithilfe von Befehlen oder mithilfe des bereitstellungs-APIs installiert werden.

Visual Studio 2010 bietet integrierte MSBuild-Aufgaben und Ziele zum Erstellen von Web-Paketen. Weitere Informationen finden Sie unter [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698%28VS.100%29.aspx) auf der MSDN-Website und [10 + 20 Gründe, warum sollten Sie eine Web-Paket erstellen](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) Vishal Joshi Blog.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Web.config-Transformation

Für die Bereitstellung von Webanwendungen, führt Visual Studio 2010 [XML-Dokuments transformieren (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), dies ist ein Feature, das Sie transformieren kann eine `Web.config` -Datei von entwicklungseinstellungen auf Produktionseinstellungen. Transformation angegeben sind in der Transformationsdateien, die mit dem Namen `web.debug.config`, `web.release.config`und so weiter. (Die Namen dieser Dateien mit MSBuild Konfigurationen übereinstimmen.) Eine Transformationsdatei enthält nur die Änderungen, die Sie benötigen, um eine bereitgestellte `Web.config` Datei. Geben Sie die Änderungen mithilfe der einfachen Syntax.

Das folgende Beispiel zeigt einen Teil einer `web.release.config` Datei, die erzeugt werden möglicherweise für die Bereitstellung von der Releasekonfiguration. Im Beispiel die ersetzen-Schlüsselwort Gibt an, dass während der Bereitstellung der *"ConnectionString"* Knoten in der `Web.config` Datei ersetzt werden mit den Werten, die im Beispiel aufgeführt sind.

[!code-xml[Main](overview/samples/sample102.xml)]

Weitere Informationen finden Sie unter ["Web.config" Transformation Syntax für die Bereitstellung von Webanwendungsprojekten](https://msdn.microsoft.com/library/dd465326%28VS.100%29.aspx) im MSDN <a id="0.2_a"> </a> Website und[Webbereitstellung: Web.Config-Transformation](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)Vishal Joshi Blog.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Datenbankbereitstellung

Ein Visual Studio 2010-Bereitstellungspaket kann Abhängigkeiten von SQL Server-Datenbanken enthalten. Als Teil der Paketdefinition geben Sie die Verbindungszeichenfolge für die Quelldatenbank an. Wenn Sie das Web-Paket erstellen, wird von Visual Studio 2010 erstellt SQL-Skripts für das Datenbankschema und optional für die Daten und fügt Sie diese dem Paket hinzu. Sie können auch benutzerdefinierte SQL-Skripts bereitstellen und angeben die Reihenfolge, in der sie auf dem Server ausgeführt werden sollen. Zum Zeitpunkt der Bereitstellung Geben Sie eine Verbindungszeichenfolge, die für den Zielserver geeignet ist; Der Bereitstellungsprozess verwendet dann diese Verbindungszeichenfolge, um die Skripts auszuführen, die das Datenbankschema zu erstellen, und fügen Sie die Daten.

Darüber hinaus mit nur einem Klick veröffentlichen, können Sie konfigurieren, Bereitstellung, um Ihre Datenbank direkt zu veröffentlichen, wenn die Anwendung an einem Remotestandort freigegebenen hosting veröffentlicht wird. Weitere Informationen finden Sie unter [Vorgehensweise: Bereitstellen einer Datenbank mit einem Webanwendungsprojekt](https://msdn.microsoft.com/library/dd465343%28VS.100%29.aspx) auf der MSDN-Website und [Datenbankbereitstellung mit VS 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) Vishal Joshi Blog.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>One-Click-Veröffentlichung für Webanwendungen

Visual Studio 2010 können auch den IIS-remote-Management-Dienst zum Veröffentlichen einer Webanwendung auf einem Remoteserver verwenden. Sie können ein Veröffentlichungsprofil für Ihr Konto hosting oder zum Testen von Servern oder Stagingserver erstellen. Jedes Profil kann die entsprechende Anmeldeinformationen sicher speichern. Anschließend können Sie einem beliebigen des Ziels bereitstellen veröffentlichen Server mit nur einem Klick mit der Web-einmalklick-Symbolleiste. Mit Visual Studio 2010 können Sie auch mit der MSBuild-Befehlszeile veröffentlichen. Dadurch können Sie das Konfigurieren der Team Build-Umgebung zur Veröffentlichung in einem continuous Integration-Modell enthalten.

Weitere Informationen finden Sie unter [Vorgehensweise: Bereitstellen von Web Application Project One-Click veröffentlichen und Web Deploy](https://msdn.microsoft.com/library/dd465337%28VS.100%29.aspx) auf der MSDN-Website und [1-Klick-Veröffentlichen mit Visual Studio 2010 Web](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) Vishal Joshi Blog. Videopräsentationen zur Bereitstellung von Webanwendungen in Visual Studio 2010 finden Sie unter [VS 2010 für die Web Developer Vorschau](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) Vishal Joshi Blog.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Ressourcen

Die folgenden Websites bieten zusätzliche Informationen zu ASP.NET 4 und Visual Studio 2010.

- [ASP.NET 4](https://msdn.microsoft.com/library/ee532866%28VS.100%29.aspx) – der offiziellen Dokumentation für ASP.NET 4 auf Daten in der MSDN-Website.
- [https://www.asp.net/](https://www.asp.net/) – Die ASP.NET Website des Teams.
- [https://www.asp.net/dynamicdata/](https://msdn.microsoft.com/library/cc488545.aspx) und [ASP.NET Dynamic Data Content Map](https://msdn.microsoft.com/library/cc488545%28VS.100%29.aspx) – Online-Ressourcen auf der ASP.NET-Website-Team und in der offiziellen Dokumentation für ASP.NET Dynamic Data.
- [https://www.asp.net/ajax/](../../ajax/index.md) – Die wichtigsten Webressource für ASP.NET Ajax-Entwicklung.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) – Die Visual Web Developer-Team-Blog, die Informationen zu den Funktionen in Visual Studio 2010 enthalten.
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) – die wichtigsten Webressource für Preview-Versionen von ASP.NET.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Haftungsausschluss

Dies ist ein vorläufiges Dokument, das vor dem kommerziellen Release der beschriebenen Software ggf. erheblich geändert wird.

Die Informationen in diesem Dokument repräsentieren den aktuellen Standpunkt der Microsoft Corporation zu den erörterten Problemen zum Veröffentlichungstermin. Da Microsoft auf das Ändern von Marktlagen reagieren muss, ist das Dokument keinesfalls als Verpflichtung von Microsoft zu interpretieren, und Microsoft kann die Genauigkeit der Informationen nicht über den Zeitpunkt der Veröffentlichung hinaus garantieren.

Dieses Whitepaper dient ausschließlich Informationszwecken. MICROSOFT ÜBERNIMMT KEINE AUSDRÜCKLICHE, STILLSCHWEIGENDE ODER AUS GESETZ ERWACHSENDE GARANTIE IN BEZUG AUF DIE INFORMATIONEN IN DIESEM DOKUMENT.

Die Benutzer/innen sind verpflichtet, sich an alle anwendbaren Urheberrechtsgesetze zu halten. Unabhängig von der Anwendbarkeit der entsprechenden Urheberrechtsgesetze darf ohne ausdrückliche schriftliche Erlaubnis der Microsoft Corporation kein Teil dieses Dokuments für irgendwelche Zwecke vervielfältigt oder in einem Datenempfangssystem gespeichert oder darin eingelesen werden, unabhängig davon, auf welche Art und Weise oder mit welchen Mitteln (elektronisch, mechanisch, durch Fotokopieren, Aufzeichnen usw.) dies geschieht.

Microsoft Corporation kann Inhaber von Patenten oder Patentanträgen, Marken, Urheberrechten oder anderem geistigen Eigentum sein, die den Inhalt dieses Dokuments betreffen. Die Bereitstellung dieses Dokuments gewährt keinerlei Lizenzrechte an diesen Patenten, Marken, Urheberrechten oder anderem geistigen Eigentum, es sei denn, dies wurde ausdrücklich durch einen schriftlichen Lizenzvertrag mit der Microsoft Corporation vereinbart.

Sofern nicht anders angegeben, einer der Firmen, Organisationen, Produkte, Domänennamen, e-Mail-Adressen, Logos, Personen, Orte und Ereignisse in diesem Dokument dargestellten sind frei erfunden und keine Zuordnung zu echten Unternehmen, Organisation, Produkt, Domänennamen, e-Mail Adresse Logo "," Person "," Ort "oder" Ereignis dient, noch ableitbar.

© 2009 Microsoft Corporation. Alle Rechte vorbehalten.

Microsoft und Windows sind eingetragene Marken oder Marken der Microsoft Corporation in den USA und/oder anderen Ländern.

Die in diesem Dokument erwähnten Namen von Unternehmen und Produkten können Marken der jeweiligen Eigentümer sein.
