---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Gewusst wie: Hinzufügen mobiler Seiten zu Ihren ASP.NET Web Forms-/ MVC-Anwendung | Microsoft-Dokumentation'
author: rick-anderson
description: Diesem Artikel wird beschrieben, verschiedene Möglichkeiten, dienen der Seiten optimiert für mobile Geräte über Ihre ASP.NET Web Forms / MVC-Anwendung und vorschlägt, Architektur und...
ms.author: aspnetcontent
ms.date: 01/20/2011
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: 59b81184852a7fe0ad2dcad9718b572a8c756918
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823354"
---
<a name="how-to-add-mobile-pages-to-your-aspnet-web-forms--mvc-application"></a>Gewusst wie: Hinzufügen mobiler Seiten zu Ihren ASP.NET Web Forms-/ MVC-Anwendung
====================
> **Gilt für**
> 
> - ASP.NET Web Forms-Version 4.0
> - ASP.NET MVC, Version 3.0
> 
> **Zusammenfassung**
> 
> Diesem Artikel wird beschrieben, verschiedene Möglichkeiten, dienen der Seiten optimiert für mobile Geräte über Ihre ASP.NET Web Forms / MVC-Anwendung und schlägt vor Architektur und Probleme zu berücksichtigen, wenn eine Vielzahl von Geräten entwerfen. In diesem Dokument wird auch erläutert, warum ASP.NET Mobile-Steuerelemente von ASP.NET 2.0 bis 3.5 sind jetzt veraltet, und erläutert einige moderne alternativen.


## <a name="contents"></a>Inhalt

- Übersicht
- Architektur-Optionen
- Erkennung von Browser und Geräte
- ASP.NET Web Forms-Anwendungen können wie Mobile-spezifische Seiten präsentieren.
- Wie ASP.NET MVC-Anwendungen für Mobile-spezifische Seiten darstellen kann
- Zusätzliche Ressourcen

Herunterladbare Codebeispiele in diesem Whitepaper Techniken für ASP.NET Web Forms und MVC veranschaulichen, finden Sie unter [Mobile Apps und Websites mit ASP.NET](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Übersicht

Mobile Geräte – Smartphones, Mobiltelefonen und Tablets – weiterhin an Popularität als Mittel zum Zugriff auf das Web gewinnen. Für viele Webentwickler und Web-orientierten Unternehmen bedeutet dies, dass es immer wichtiger, Besucher, die mit diesen Geräten eine hervorragende Browsingumgebung bereit ist.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Wie frühere Versionen von ASP.NET unterstützt mobile Browser

ASP.NET-Versionen 2.0 bis 3.5 enthalten *ASP.NET Mobile Controls*: einen Satz von Serversteuerelementen für mobile Geräte in der *"System.Web.Mobile.dll"* Assembly und die  *System.Web.UI.MobileControls* Namespace. Die Assembly wird immer noch in ASP.NET 4 enthalten, aber es ist als veraltet markiert. Entwicklern wird empfohlen, zum Migrieren zu moderneren Ansätze, z. B. die in diesem Dokument beschrieben.

Der Grund, warum ASP.NET Mobile-Steuerelemente als veraltet markiert wurden, ist, dass es sich bei ihren Entwurf für die Mobiltelefone ausgerichtet wird, die häufig für 2005 und früher wurden. Die Steuerelemente hauptsächlich dienen zum Rendern von WML oder cHTML-Markup (statt reguläre HTML) für die WAP-Browser dieser Ära. Aber WAP, cHTML- und WML sind nicht mehr für die meisten Projekte relevant, da HTML jetzt die ubiquitäre Markupsprache für mobilen und Desktopbrowsern gleichermaßen geworden ist.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Die Herausforderungen von heute Unterstützung von Mobilgeräten

Obwohl mobile Browser jetzt fast durchgehend HTML unterstützen, werden Sie weiterhin viele Herausforderungen konfrontiert, bei denen versucht wird, erstellen Sie großartige mobile browserumgebungen kennen:

- ***Bildschirmgröße*** – Mobile Geräte unterscheiden sich erheblich, im Formular aus, und ihren Bildschirmen sind häufig viel kleiner als Desktopmonitore. Daher müssen Sie möglicherweise für sie völlig anderen Seitenlayouts zu entwerfen.
- ***Geben Sie Methoden*** : Einige Geräte Eingaben per Tastatur, einige haben Styluses, andere Touch verwenden. Sie müssen möglicherweise mehrere Navigation Eingabemethoden von Mechanismen und Daten berücksichtigen.
- ***Einhaltung von Standards*** – viele mobile Browser unterstützen nicht die neuesten Standards für HTML, CSS oder JavaScript.
- ***Bandbreite*** – datenverbindungen netzwerkleistung ist völlig unterschiedlich, und einige Benutzer über Tarife, die von der Megabyte berechnet werden.

Es gibt keine allgemeingültige Lösung. Ihre Anwendung müssen Aussehen und Verhalten sich anders, entsprechend dem Gerät darauf zugreifen. Dies kann je nachdem, welche Ebene der mobile-Unterstützung soll eine größere Herausforderung für Webentwickler konzipierter als je zuvor war der Desktop "browserkriege" sein.

Entwickler häufig anfänglich Annäherung an Unterstützung für mobile Browser zum ersten Mal vorstellen, dass es ist nur wichtig, um die neuesten und entwickelten Smartphones (z. B. Windows Phone 7, iPhone oder Android), z. B. zu unterstützen, da Entwickler oft persönlich z. B. besitzen Geräte. Kostengünstiger Smartphones sind allerdings weiterhin äußerst beliebte, und die Besitzern verwenden sie das Web – insbesondere in Ländern durchsuchen, in denen einfacher, als einer Breitbandverbindung sind Mobiltelefone. Ihr Unternehmen müssen entscheiden, welche Zahl von Geräten zu unterstützen, indem Sie die wahrscheinlichsten Kunden berücksichtigt. Wenn Sie ein online-Broschüre für einen Luxus Integrität Spa erstellen, können Sie eine geschäftliche Entscheidung nur für die erweiterte Smartphones als Ziel vornehmen, wenn Sie ein Ticket-Buchungssystem für ein Kino erstellen, wahrscheinlich muss allerdings Besucher mit weniger leistungsfähigen Feature berücksichtigen Smartphones.

## <a name="architectural-options"></a>Architektur-Optionen

Bevor wir zu den technischen Einzelheiten der ASP.NET Web Forms oder MVC eingehen, beachten Sie, dass Webentwickler in der Regel drei mögliche Hauptoptionen für die Unterstützung mobiler Browser:

1. ***Nichts Unternehmen –*** können Sie einfach eine standardmäßige, Desktop-orientierten Webanwendung erstellen und in mobilen Browsern Auswirkung in akzeptabler Weise Rendern basieren. 

    - **Vorteil**: Es ist die kostengünstigste Möglichkeit, implementieren und verwalten – ohne zusätzliche arbeiten.
    - **Nachteil**: die schlimmste benutzerfreundlichkeit erhalten: 

        - Die neuesten Smartphones möglicherweise genauso gut wie eine desktop-Browser den HTML-Code gerendert, aber Benutzer werden immer noch gezwungen, zoom und Bildlauf und vertikal um Ihre Inhalte auf einem kleinen Bildschirm zu verwenden. Dies ist alles andere als optimal.
        - Ältere Geräte und Mobiltelefonen können zum Rendern von Markup auf zufriedenstellender Weise fehlschlagen.
        - Auch auf den neuesten Tablet-Geräten (dessen Bildschirme können so groß wie Laptopbildschirmen sein), Interaktion mit anderen Regeln angewendet werden. Touchbasierte Eingabe funktioniert am besten mit größeren Schaltflächen und links Spread weiter auseinander liegen und es gibt keine Möglichkeit, ein Flyout-Menü ein Mauszeiger darauf zeigen.
2. ***Das Problem auf dem Client* –** mit sorgfältige Verwendung von CSS und [progressive Verbesserung](http://en.wikipedia.org/wiki/Progressive_enhancement) können Erstellung Markup, Formate und Skripts, die Anpassung an den Browser, die sie ausgeführt wird. Z. B. mit [3 der CSS-Medienabfragen](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), Sie erstellen ein mehrspaltiges Layout, die in einer einspaltigen Layouts auf Geräten verwandelt, deren Bildschirme enger als einen ausgewählten Schwellenwert sind. 

    - **Vorteil**: Rendering für das jeweilige Gerät verwendet werden, auch für unbekannte zukünftige Geräte entsprechend den Bildschirm und die eingegebenen Merkmale sie haben optimiert
    - **Vorteil**: einfach können Sie die serverseitige Logik für alle Gerätetypen – minimale Duplizierung von Code oder jeglichen Aufwand freigeben
    - **Nachteil**: Mobile Geräte unterscheiden sich daher von desktop-Geräte, dass Sie tatsächlich Ihre mobilen Seiten grundlegend von Ihrem desktop Seiten werden möglicherweise unterschiedliche Informationen angezeigt. Diese Varianten können ineffizient sein oder überhaupt nicht realisiert robust durch CSS eigenständig verwendet, insbesondere angesichts der Tatsache wie inkonsistent ältere Geräte interpretieren CSS-Regeln. Dies gilt insbesondere für CSS 3-Attribute.
    - **Nachteil**: bietet keine Unterstützung für unterschiedliche serverseitige Logik und Workflows für verschiedene Geräte. Sie nicht möglich ist, z. B. eine vereinfachte shopping Cart Arbeitsablauf beim Auschecken für Benutzer mobiler Geräte mithilfe von CSS allein implementieren.
    - **Nachteil**: ineffizienten Nutzung der Bandbreite. Ihr Server möglicherweise zum Übertragen von Markup und Stile, die für alle Geräte möglich, gelten auch, wenn das Gerät nur eine Teilmenge der Informationen verwendet.
3. ***Das Problem auf dem Server* –** , wenn Ihre Server weiß, welches Gerät – zugreift oder mindestens die Merkmale des Geräts, z. B. die Bildschirmgröße und -Methode, und, ob es sich um ein mobiles Gerät – ist es kann eine unterschiedliche Logik ausgeführt und Ausgabe bei der anderen HTML-Markup. 

    - **-Vorteil:** maximale Flexibilität. Es gibt keine Beschränkung, wie viel Sie variieren die serverseitige Logik für mobile Geräte oder das Markup für das gewünschte Layout mit gerätespezifischen optimieren können.
    - **-Vorteil:** effiziente bandbreitennutzung. Sie müssen nur zum Übertragen von Markup und Formatierung-Informationen, die das Zielgerät verwenden soll.
    - **Der Nachteil:** manchmal erzwingt die Wiederholung von Aufwand oder Code (z. B. sorgen, dass Sie ähnliche, aber leicht unterschiedliche Kopien Ihrer Webformularen oder MVC-Ansichten zu erstellen). Möglich, die Sie in eine darunter liegende Ebene oder Dienst, aber dennoch einige Teile des UI-Code oder Markup, gemeinsame Logik zu berücksichtigen gilt, in denen möglicherweise dupliziert und anschließend parallel verwaltet werden.
    - **Der Nachteil:** geräteerkennung ist keine leichte Aufgabe. Erfordert eine Liste oder eine Datenbank mit bekannten Gerät und Typen und deren Merkmale (die möglicherweise nicht immer perfekt auf dem neuesten Stand) und ist nicht zwingend jede eingehende Anforderung genau übereinstimmen. In diesem Dokument einige Optionen und ihre fallen an späterer Stelle beschrieben.

Um die besten Ergebnisse zu erhalten, finden die meisten Entwickler, die sie benötigen, um Optionen (2) und (3) zu kombinieren. Im geringfügig sind am besten auf dem Client mithilfe von CSS- oder sogar JavaScript untergebracht, während die wichtigsten Unterschiede in den Daten, Workflows oder Markup am effektivsten in serverseitigem Code implementiert werden.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Dieses Dokument konzentriert sich auf Serverseite Techniken

Da ASP.NET Web Forms und MVC beide in erster Linie eine serverseitige Technologien sind, konzentriert in diesem Whitepaper serverseitige Techniken sich auf, mit denen Sie die anderen Markup und Logik für alle mobilen Browser zu erzeugen. Natürlich, Sie können diese auch kombinieren, mit beliebigen clientseitigen-Verfahren (z. B. 3 der CSS-Medienabfragen progressiven Verbesserung des JavaScript), aber mehr darum Webdesign als ASP.NET-Entwicklung ist.

## <a name="browser-and-device-detection"></a>Erkennung von Browser und Geräte

Die wichtige Voraussetzung für alle serverseitigen Techniken für die Unterstützung mobiler Geräte ist wissen, welches Gerät Ihre Besucher verwendet wird. Tatsächlich ist sogar noch besser kennen, die Anzahl Hersteller und Modell des Geräts ist das Wissen der *Merkmale* des Geräts. Eigenschaften können sein:

- Handelt es sich um ein mobiles Gerät?
- Die Eingabemethode (Tastatur oder Maus, Touch, Zehnertastatur, Joystick,...)
- Bildschirmgröße (physisch und in Pixel)
- Unterstützte Formate für Medien und Daten
- Usw.

Ist es besser, Entscheidungen auf Grundlage der Eigenschaften als Modellnummer, da vornehmen, und Sie können besser auf zukünftigen Geräten behandeln.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Verwenden ASP. NET integrierte Erkennung Browserunterstützung

ASP.NET Web Forms und MVC-Entwickler können sofort wichtige Merkmale von einem Browser aufgerufen. durch Überprüfen der Eigenschaften der *Request.Browser* Objekt. Beispielsweise finden Sie unter

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ... und viele andere

Hinter den Kulissen entspricht die ASP.NET-Plattform eingehenden *User-Agent* (UA) HTTP-Header für reguläre Ausdrücke in einen Satz von Browser-Definitions-XML-Dateien. Wird standardmäßig die Plattform enthält Definitionen für viele allgemeine mobile Geräte, und Sie können benutzerdefinierte Browserdefinition Dateien hinzufügen, für andere Benutzer, die Sie erkennen möchten. Weitere Informationen finden Sie unter der MSDN-Seite [ASP.NET Webserver- und Browserfunktionen](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Mithilfe der Datenbank des WURFL Geräte über 51Degrees.mobi Foundation

Während Sie ASP. NET integrierten Browser Erkennung-Unterstützung wird für viele Anwendungen ausreichend sein. Es gibt zwei wichtigsten Fälle nicht ausreichend sein könnten:

- ***Sie möchten die neuesten Geräten zu erkennen***(ohne das manuelle Erstellen von Browser-Definitionsdateien für diese). Beachten Sie, dass .NET 4 Browser-Definitionsdateien nicht aktuell genug, um Windows Phone 7, Android-Telefone, Opera Mobile Browser oder Apple iPads zu erkennen sind.
- ***Sie benötigen Weitere ausführliche Informationen zu den Gerätefunktionen***. Müssen Sie möglicherweise Wissen zur Eingabe eines Geräts-Methode (z. B. Touch und Tastatur) oder welche Audio über den Browser Formate unterstützt. Diese Informationen sind nicht in den standard-Browser-Definitionsdateien zur Verfügung.

Die [ *Wireless Universal Resource File* (WURFL)-Projekt](http://wurfl.sourceforge.net/) verwaltet viel mehr auf dem neuesten Stand und detaillierte Informationen zu mobilen Geräten verwendet noch heute.

Die gute Nachricht für .NET-Entwickler ist, dass ASP. NET Erkennung Browserfunktion ist erweiterbar und, daher ist es möglich, zu verbessern, um diese Probleme zu umgehen. Sie können z. B. hinzufügen, die open Source [ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection) Bibliothek zu Ihrem Projekt. Es ist ein ASP.NET "IHttpModule" oder Browser-Funktionen-Anbieter (für Web Forms und MVC-Anwendungen verwendet werden), die direkt WURFL Daten liest und verknüpft diesen in ASP. NET integrierten Browser der Erkennungsmechanismus. Nachdem Sie das Modul installiert haben *Request.Browser* plötzlich viel genauer und ausführliche Informationen enthält: es ordnungsgemäß erkennt viele der zuvor erwähnten Geräte und ihre Funktionen (einschließlich der Liste zusätzliche Funktionen wie z. B. Eingabemethode). Finden Sie weitere Details des Projekts Dokumentation.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Web Forms-Anwendungen können wie Mobile-spezifische Seiten präsentieren.

Standardmäßig sieht aus wie eine neue Web Forms-Anwendung auf allgemeine mobilen Geräten angezeigt:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Natürlich wird weder Layout sieht sehr mobilgerätefreundliche – auf dieser Seite einen großen, Querformat Monitor nicht für einen kleinen Hochformat-Bildschirm konzipiert wurde. Daher können Sie dazu vorgehen?

Wie weiter oben in diesem Artikel erläutert wird, gibt es viele Möglichkeiten, Ihre Seiten für mobile Geräte anzupassen. Stehen einige Techniken, Server-basierten, andere Benutzer auf dem Client ausgeführt.

### <a name="creating-a-mobile-specific-master-page"></a>Erstellen eine spezifische Mobile Gestaltungsvorlage

Je nach Ihren Anforderungen können Sie möglicherweise verwenden den gleichen Webformularen für alle Besucher, allerdings müssen zwei separate Masterseiten: einer für den desktop Besucher, einen anderen mobilen Besucher. Dies bietet Ihnen die Flexibilität der CSS-Stylesheet oder HTML-Markup an mobile Geräte anpassen, ohne dass Sie jede Seitenlogik Duplizieren der obersten Ebene ändern.

Dies ist ganz einfach. Beispielsweise können Sie einen PreInit-Ereignishandler wie den folgenden zu einem Web Form hinzufügen:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Erstellen Sie jetzt eine Masterseite Mobile.Master in den Ordner der obersten Ebene Ihrer Anwendung aufgerufen, und es wird verwendet, wenn ein mobiles Gerät erkannt wird. Ihre mobile Masterseite kann ein Mobile-spezifischen CSS-Stylesheet ggf. verweisen. Desktop Besucher werden Ihrer Standardmasterseite, nicht auf den mobilen Faktor weiterhin angezeigt werden.

### <a name="creating-independent-mobile-specific-web-forms"></a>Erstellen von unabhängigen speziell für Mobile Web Forms

Maximale Flexibilität erhalten können Sie viel mehr, als separate Masterseiten für verschiedene Gerätetypen wechseln. Sie können zwei implementieren *völlig separate Sätze von Web Forms-Seiten* – eine für Desktopbrowser festgelegt, einen zweiten Satz für mobile Geräte. Dies funktioniert am besten, wenn es sich bei mobilen Besuchern sehr unterschiedliche Informationen oder Workflows angezeigt werden soll. Im restlichen Teil dieses Abschnitts wird dieser Ansatz im Detail beschrieben.

Vorausgesetzt, dass Sie bereits über Web Forms-Anwendungen für desktop-Browser entwickelt haben, ist die einfachste Möglichkeit, um den Vorgang fortzusetzen, einen Unterordner namens "Mobil" innerhalb des Projekts, und Erstellen Ihrer mobilen Seiten vorhanden ist. Sie können erstellen gesamten untergeordneten Standort, mit eigenen Masterseiten, Stylesheets und Seiten mit den gleichen Techniken, die Sie für eine andere Web Forms-Anwendung verwenden würden. Sie müssen nicht unbedingt ein mobile Äquivalent für erzeugen *jeder* Ihrer desktop-Website auf der Seite; Sie können auswählen, welche Teilmenge der Funktionen für mobile Besucher sinnvoll ist.

Ihre mobilen Seiten können verwenden gemeinsam freigegebene statische Ressourcen (z. B. Bilder, JavaScript oder CSS-Dateien) mit Ihren regulären Seiten, wenn Sie möchten. Da es sich bei Ihrem Ordner "Mobil" wird *nicht* markiert werden als separate Anwendung beim Hosten in IIS (es ist nur ein einfaches Unterordner des Web Forms-Seiten), es außerdem stellen die gleichen Konfiguration, Sitzungsdaten und andere Infrastructure-as-Ihre Desktop-Seiten.

> [!NOTE]
> Da dieser Ansatz in der Regel eine Duplizierung von Code umfasst (mobile Seiten sind wahrscheinlich einige ähnlichkeiten mit desktop Seiten freizugeben), es ist wichtig, die einen Faktor Sie common Business Anwendungslogik oder der Zugriff Code in eine gemeinsame zugrunde liegenden Schicht oder einen Dienst. Andernfalls sind Sie in double der Aufwand für das Erstellen und Verwalten Ihrer Anwendung.


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Umleiten von mobilen Besucher von mobilen-Seiten

Häufig ist es sinnvoll, zum Umleiten von mobilen Besucher der mobilen-Seiten nur auf die *erste* in ihrer Browsersitzung (und nicht bei jeder Anforderung in ihrer Sitzung) angefordert werden, weil:

- Sie können dann problemlos zulassen mobile Besucher auf Ihrem desktop Seiten zuzugreifen, bei Bedarf platziert einen Link auf der Masterseite, die auf "Desktop-Version" geht –. Der Besucher wird nicht an einer mobilen Seite – umgeleitet werden, da es die erste Anforderung nicht mehr in ihre Sitzung ist.
- Es vermeidet das Risiko von behindert Anforderungen für dynamische Ressourcen gemeinsam von Desktop- und mobile Teile Ihrer Website (z. B., wenn Sie eine allgemeine Web Form, in denen Desktop- und mobile Teile Ihrer Website in einem IFRAME oder bestimmte Ajax-Handler angezeigt haben)

Zu diesem Zweck können Sie die umleitungslogik in Platzieren einer **Sitzung\_starten** Methode. Fügen Sie z. B. die folgende Methode Ihrer Datei "Global.asax.cs":

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurieren der Formularauthentifizierung zur Einhaltung von mobilen-Seiten

Beachten Sie, dass die Formularauthentifizierung von bestimmten Annahmen zu, in dem sie Besucher während und nach der Authentifizierung umgeleitet werden kann:

- Wenn ein Benutzer authentifiziert werden muss, die Formularauthentifizierung wird eine Umleitung an Ihre desktop-Anmeldeseite, unabhängig davon, ob sie eine Desktop- oder mobile Benutzer (weil er nur ein Konzept von *eine* -Anmelde-URL). Vorausgesetzt, dass Sie Ihre mobile Anmeldeseite unterschiedlich formatieren möchten, müssen Sie Ihre desktop-Anmeldeseite zu erweitern, sodass Benutzer mobiler Geräte zu einer separaten mobilen-Anmeldeseite umgeleitet. Z. B. den folgenden Code zum Hinzufügen Ihrer **Desktop** Seite Code-Behind-Anmeldung: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Nachdem ein Benutzer erfolgreich anmeldet, Formularauthentifizierung wird standardmäßig eine Umleitung an die Startseite Ihres desktop (weil er nur ein Konzept von *eine* Standardseite). Sie müssen Ihre mobile Anmeldeseite zu erweitern, sodass es nach einer erfolgreichen Anmeldung zur Mobile-Homepage leitet. Z. B. den folgenden Code zum Hinzufügen Ihrer **mobile** Seite Code-Behind-Anmeldung: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Dieser Code wird davon ausgegangen, dass Ihre Seite ein Anmeldesteuerelement-Server LoginUser, wie die standardmäßige Projektvorlage aufgerufen hat.

### <a name="working-with-output-caching"></a>Arbeiten mit Zwischenspeicherung der Ausgabe

Wenn Sie die Zwischenspeicherung der Ausgabe verwenden, beachten Sie, dass in der Standardeinstellung ist es möglich, dass desktop-Benutzer auf eine bestimmte URL (verursacht die Ausgabe zwischengespeichert werden) finden von einem mobilen Benutzer ausgeführt haben, klicken Sie dann die zwischengespeicherte Ausgabe in desktop erhält. Diese Warnung gilt, ob Sie nur die Masterseite nach Gerätetyp variieren oder völlig separate Web Forms pro Gerätetyp implementieren.

Um das Problem zu vermeiden, können Sie anweisen, ASP.NET zum variieren des Cacheeintrags entsprechend gibt an, ob der Besucher auf ein mobiles Gerät verwendet wird. Fügen Sie einen VaryByCustom-Parameter auf Ihrer Seite OutputCache Deklaration wie folgt:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Definieren Sie als Nächstes *IsMobileDevice* als benutzerdefinierte Cache Parameter durch Hinzufügen der folgenden Methode zu überschreiben, in Ihrer Datei "Global.asax.cs":

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Dadurch wird sichergestellt, dass mobile Besucher der Seite keine Ausgabe, die bereits im Cache abgelegt, von einem desktop Besucher erhalten.

### <a name="a-working-example"></a>Ein praktisches Beispiel

Um diese Techniken in Aktion zu sehen, laden [Codebeispiele in diesem Whitepaper](https://docs.microsoft.com/aspnet/mobile/overview). Die Web Forms-beispielanwendung leitet automatisch mobile Benutzer auf eine Gruppe spezieller mobiler Seiten in einem Unterordner namens Mobile. Das Markup und die Gestaltung der Seiten ist besser optimiert für mobile Browser, wie Sie von den folgenden Screenshots sehen können:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Weitere Tipps zum Optimieren der Markup und CSS für mobile Browser finden Sie im Abschnitt "Mobile Seiten für alle mobilen Browser formatieren" weiter unten in diesem Dokument.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>Wie ASP.NET MVC-Anwendungen für Mobile-spezifische Seiten darstellen kann

Da das Model-View-Controller-Muster Anwendungslogik (in-Controller) von Präsentationslogik (in Ansichten) entkoppelt, können Sie zur Behandlung von mobile-Unterstützung in serverseitigem Code aus einer der folgenden Methoden auswählen:

1. ***Verwenden Sie denselben Controller und Ansichten für Desktop- und mobile Browser, aber Rendern Sie die Ansichten mit anderen Razor-Layouts abhängig vom Typ * Gerät zu.** Diese Option funktioniert am besten, wenn Sie die werden identische Daten auf allen Geräten anzeigen, aber einfach möchten, geben Sie verschiedene CSS-Stylesheets oder einige obersten Ebene HTML-Elementen für mobile Geräte zu ändern.
2. ***Verwenden Sie denselben Controllern für Desktop- und mobile Browser, aber unterschiedliche Ansichten je nach Gerätetyp Rendern***. Diese Option funktioniert am besten, wenn Sie anzeigen, etwa die gleichen Daten und die gleichen Workflows für Benutzer bereitstellen, aber entsprechend das verwendeten Gerät sehr unterschiedliche HTML-Markup gerendert werden soll.
3. ***Erstellen Sie separate Bereiche für Desktop- und mobile Browser, unabhängig von Controllern und Ansichten für jede Implementierung *.** Diese Option funktioniert am besten, wenn Sie sehr unterschiedliche Bildschirme anzeigen, enthält verschiedene Informationen und führt den Benutzer mithilfe von anderen Workflows, die für ihren Gerätetyp optimiert. Eine Wiederholung von Code das kann bedeuten, Sie können jedoch minimiert werden, die durch das Ausklammern von gemeinsame Logik in einer zugrunde liegenden Ebene oder eines Diensts.

Wenn Sie nutzen möchten die **erste** aus, und nur die Razor-Layout variieren pro Gerätetyp, es ist sehr einfach. Ändern Sie lediglich Ihre \_ViewStart.cshtml Datei wie folgt:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Nun können Sie eine spezifische Mobile Layout mit dem Namen erstellen \_LayoutMobile.cshtml mit einer Seitenstruktur und CSS-Regeln für mobile Geräte optimiert.

Wenn Sie nutzen möchten die **zweite** option völlig unterschiedliche Ansichten rendern, nach Typ des Besuchers des Geräts, finden Sie unter [Blogbeitrag des Scott Hanselman](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

Die übrigen Teil dieses Artikels im Mittelpunkt der **dritte** Option – erstellen separate Controller *und* Ansichten für mobile Geräte – damit Sie steuern können, genau die gewünschte Teilmenge der Funktionen für mobile Besucher angeboten wird.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Das Einrichten eines mobilen Bereichs innerhalb der ASP.NET MVC-Anwendung

Sie können einen Bereich namens "Mobil" zu einer vorhandenen ASP.NET MVC-Anwendung auf die übliche Weise hinzufügen: mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, und wählen Sie Add Kenntnis Bereich. Sie können dann Controller und Ansichten hinzufügen, wie bei jedem anderen Bereich innerhalb einer ASP.NET MVC-Anwendung. Z. B. auf Ihren mobilen Bereich Hinzufügen eines neuen Controllers HomeController, fungiert als Startseite für mobile Besucher aufgerufen.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Sicherstellen der URL-/Mobile erreicht die mobile-Startseite

Wenn Sie die URL-/Mobile die Index-Aktion auf HomeController in Ihrer mobilen Bereich erreichen möchten, müssen Sie zwei kleine Änderungen an der routing-Konfiguration vornehmen. Aktualisieren Sie zunächst Ihre MobileAreaRegistration-Klasse, damit HomeController Standardcontroller in Ihrem mobilen Bereich ist wie im folgenden Code gezeigt:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Dies bedeutet, dass die mobile-Startseite wird jetzt befindet am /Mobile, anstatt/Mobile/Home, da "Home" jetzt ist die implizit Controller Standardname für mobile Seiten.

Beachten Sie als Nächstes, dass durch das Hinzufügen eines zweiten HomeController für Ihre Anwendung (d. h. mobile derjenige, zusätzlich zu den vorhandenen Desktop-1), Sie der regulären desktop Startseite unterbrochen wurde, werden. Sie schlägt mit Fehler "*wurden mehrere Typen gefunden, die den Controller, mit dem Namen"Home"entsprechen*". Zum Beheben dieses Problems aktualisieren Sie die Routingkonfiguration auf oberster Ebene. (in "Global.asax.cs"), um anzugeben, dass Ihr desktop HomeController Priorität ausführen sollten, wenn Mehrdeutigkeit vorliegt:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Jetzt der Fehler wird entfernt, und die URL http://<em>Yoursite</em>/ erreichen, die desktop-Startseite, und http://<em>Yoursite</em>/mobile/ erreichen die mobile-Homepage.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Umleiten von mobilen Besucher auf Ihren mobilen Bereich

Es gibt viele verschiedene Erweiterungspunkte in ASP.NET MVC, daher gibt es viele Möglichkeiten, umleitungslogik einzufügen. Eine praktische Option ist ein Filterattribut, [RedirectMobileDevicesToMobileArea], erstellen, die eine Umleitung ausführt, wenn die folgenden Bedingungen erfüllt sind:

1. Es ist die erste Anforderung in die Sitzung des Benutzers (d. h. Session.IsNewSession gleich "true")
2. Die Anforderung stammt von einem mobilen Browser (d. h. Request.Browser.IsMobileDevice gleich "true")
3. Der Benutzer nicht bereits eine Ressource in den mobilen Bereich anfordert (d. h. die *Pfad* Teil der URL beginnt nicht mit /Mobile)

Im herunterladbare Beispiel in diesem Whitepaper enthaltenen beinhaltet die Implementierung dieser Logik. Es wird implementiert, als ein Autorisierungsfilter, abgeleitet von AuthorizeAttribute, was, dass sie ordnungsgemäß funktionieren kann bedeutet, auch wenn Sie Zwischenspeichern der Ausgabe verwenden (Wenn hingegen eine desktop-Besucher erste greift auf eine bestimmte URL, die desktop-Ausgabe zwischengespeichert und dann bereitgestellt werden kann nachfolgende mobile Besucher).

Da es sich um einen Filter handelt, können Sie entweder für die anzuwendende für bestimmte Controller und Aktionen, z. B. auswählen,

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… oder Sie können es auf alle Controller und Aktionen anwenden, wie eine MVC 3 *globalen Filter* in der Datei "Global.asax.cs":

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

Im herunterladbare Beispiel wird auch veranschaulicht, wie Sie Unterklassen dieses Attributs erstellen können, die an einen bestimmten Standort in Ihrer mobilen Region umleiten. Dies bedeutet, z. B. können Sie:

- Registrieren Sie einen globalen Filter wie oben, der mobile Besucher der mobile-Startseite standardmäßig gesendet.
- Gelten Sie auch einen speziellen [RedirectMobileDevicesToMobileProductPage] Filter für eine "Produkt anzeigen"-Aktion, die mobile Besucher der mobilen Version des alle-Produktseite nutzt, die sie angefordert haben ein.
- Auch wenden Sie andere speziellen Unterklassen des Filters auf bestimmte Aktionen, die mobile Besucher auf die entsprechende mobile Seite umleiten an

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurieren der Formularauthentifizierung zur Einhaltung von mobilen-Seiten

Wenn Sie die Formularauthentifizierung verwenden, beachten Sie, dass wenn ein Benutzer für die Anmeldung muss, es den Benutzer auf eine einzelne bestimmte "Anmelden" URL, leitet automatisch standardmäßig **/Account/LogOn**. Dies bedeutet, dass mobile Benutzer auf Ihrem desktop "Anmelden" Aktion umgeleitet werden können.

Um dieses Problem zu vermeiden, fügen Sie Logik Ihrer desktop "Anmelden" Aktion hinzu, damit mobile Benutzer wieder an eine mobile "Anmelden" umgeleitet. Wenn Sie die standardmäßige ASP.NET-MVC-Anwendungsvorlage verwenden, aktualisieren Sie die Aktion der AccountController Komponente-Anmeldung wie folgt:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… und implementieren Sie eine geeignete mobiltaugliche "Anmelden"-Aktion für einen Controller namens "AccountController" in Ihrer mobilen Region.

### <a name="working-with-output-caching"></a>Arbeiten mit Zwischenspeicherung der Ausgabe

Wenn Sie den Filter [OutputCache] verwenden, müssen Sie den Cacheeintrag, je nach Gerätetyp variieren erzwingen. Beispielsweise schreiben:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Fügen Sie dann die folgende Methode zu Ihrer Datei "Global.asax.cs":

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Dadurch wird sichergestellt, dass mobile Besucher der Seite keine Ausgabe, die bereits im Cache abgelegt, von einem desktop Besucher erhalten.

### <a name="a-working-example"></a>Ein praktisches Beispiel

Um diese Techniken in Aktion zu sehen, laden [verknüpft dieses Whitepaper Beispiele](https://docs.microsoft.com/aspnet/mobile/overview). Das Beispiel umfasst eine ASP.NET MVC 3 (Release Candidate)-Anwendung erweitert, um die Unterstützung mobiler Geräte mithilfe der oben beschriebenen Methoden.

## <a name="further-guidance-and-suggestions"></a>Weitere Anleitungen und Empfehlungen

Die folgende Diskussion gilt sowohl Web Forms und MVC-Entwickler, die in diesem Dokument behandelten Verfahren verwenden.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Verbessern Ihre umleitungslogik 51Degrees.mobi Foundation verwenden

Die umleitungslogik, die in diesem Dokument dargestellten ist möglicherweise für Ihre Anwendung vollkommen ausreichend, aber es funktioniert nicht, wenn Sie Sitzungen deaktivieren müssen, bzw. in mobilen Browsern, die Cookies (diese Sitzungen sind keine), zurückgewiesen, da es nicht, ob eine bestimmte Anforderung bemerken das erste Element von diesem Besucher.

Sie haben bereits gelernt, wie die open-Source-51Degrees.mobi Foundation die Genauigkeit der ASP verbessern können. NET Erkennung des Webbrowsers. Es kann auch eine integrierte umleitungs-mobile Besuchern an einen bestimmten Standort in der Datei "Web.config" konfiguriert. Dies ist möglich, ohne je nach ASP.NET-Sitzungen ausgeführt (und somit Cookies) speichern Sie ein temporäres Protokolls Hashes des Besuchers HTTP-Header und IP-Adressen, damit es weiß, und zwar unabhängig davon, ob jede Anforderung über einen angegebenen Besucher der erste ist.

Das folgende Element, das dem FiftyOne-Abschnitt der Datei "Web.config" hinzugefügt, leitet die erste Anforderung von einem erkannten mobilen Gerät auf der Seite "~ / Mobile/Default.aspx. Werden alle Anforderungen an den Seiten unter dem Ordner "Mobile" *nicht* umgeleitet werden, unabhängig vom Gerätetyp. Wenn das mobile Gerät für einen bestimmten Zeitraum inaktiv war von 20 Minuten oder mehr das Gerät wird vergessen werden, und nachfolgende Anforderungen werden als neue für die Zwecke der Umleitung behandelt.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Weitere Informationen finden Sie unter [51degrees.mobi Foundation-Dokumentation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Sie *können* Umleitungsfunktion verwenden 51Degrees.mobi Foundation, ASP.NET MVC-Anwendungen, aber Sie müssen Ihre umleitungskonfiguration im Hinblick auf einfache URLs, die nicht im Hinblick auf die Routingparameter oder platzieren die MVC-Filter definieren für Aktionen. Grund hierfür ist, (zum Zeitpunkt der Verfassung) 51Degrees.mobi Foundation erkennt keine Filter oder routing.


### <a name="disabling-transcoders-and-proxy-servers"></a>Deaktivieren der Transcoder und Webanwendungsproxy-Server

Mobiles Netzwerk-Operatoren haben zwei Hauptziele Ansätze, um das mobile Internet:

1. Geben Sie als viel relevante Inhalte wie möglich
2. Maximieren Sie die Anzahl der Kunden, die die Netzwerkbandbreite beschränkt Radio freigeben können

Da die meisten Webseiten für große Desktop große Bildschirme und schnell behoben-Line-breitbandverbindungen entwickelt wurden, verwenden Sie viele Operatoren *Transcoder* oder *Proxyserver* , die Web-Inhalte dynamisch ändern. Sie können Ihre HTML-Markup oder CSS-kleinere Bildschirmen (insbesondere für "Mobiltelefonen", die die verarbeitungsleistung, um komplexe Layouts zu verarbeiten hat) entsprechend ändern, und sie möglicherweise erneut Ihre Images (erheblich reduzieren ihre Qualität) komprimieren, zur Verbesserung der Geschwindigkeit der Datenübermittlung Seite.

Aber wenn Sie den Aufwand zum Erzeugen einer Mobilgeräte optimierte Version Ihrer Website erstellt haben, Sie möchten wahrscheinlich nicht die Netzwerk-Operator, um alle weiteren stören. Sie können die folgende Zeile hinzufügen, um die Seite\_Load-Ereignis in jeder ASP.NET Web Form:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

Oder für ASP.NET MVC-Controller, konnte Sie die folgenden methodenüberschreibung hinzugefügt werden, damit sie auf alle Aktionen für diesen Controller gilt:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

Die resultierende HTTP-Nachricht informiert kompatibel W3C-Transcoder und Proxys nicht für den Inhalt ändern. Es ist natürlich keine Garantie, dass mobile Netzwerkbetreiber diese Meldung berücksichtigt.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Formatieren von mobilen Seiten für alle mobilen Browser

Es würde den Rahmen dieses Dokuments, um ausführlich zu beschreiben. welche Arten von HTML-Markup Arbeit ordnungsgemäß oder Techniken der Web-Design maximieren, benutzerfreundlichkeit, die auf bestimmten Geräten. Er optimiert sich, ein ausreichend einfaches Layout, finden Sie für einen Bildschirm Größe ohne Verwendung von unzuverlässigen HTML- oder CSS-Tricks zu positionieren. Eine wichtige Verfahren sollte erwähnt werden, ist jedoch die *Viewportmetatag*.

Bestimmte modernen mobilen Browsern, in ein Aufwand anzeigen Webseiten, die für Desktopmonitore, vorgesehen Rendern die Seite auf einer virtuellen Canvas, so genannte "Ansicht" (z. B. virtuelle Viewport ist dem 980 Pixel breit ist, auf dem iPhone und 850 Pixel breit Opera Mobile standardmäßig), und klicken Sie dann Skalieren Sie das Ergebnis auf physischen Pixel des Bildschirms anpassen abwärts. Der Benutzer kann dann vergrößern und Schwenken des Viewports. Dies hat den Vorteil, dass es ermöglicht, dass des Browsers die Seite in das vorgesehene Layout anzuzeigen, aber es auch ist hat dies den Nachteil darin, dass Zoomen und Schwenken, die für den Benutzer mergereplikationsabonnenten nicht geeignet ist. Wenn Sie für mobile Geräte entwerfen können, empfiehlt sich Entwurf für einen engen Bildschirm, damit keine zoomen oder horizontalen Bildlauf erforderlich ist.

Eine Möglichkeit, um den mobilen Browser wie breit Viewport sein soll wird über die nicht dem Standard entsprechende *Viewport* Meta-Tag. Z. B. Wenn Sie die folgenden Ihrer Seite HEAD-Abschnitt hinzufügen,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… Klicken Sie dann Unterstützung Smartphone-Browser wird die Seite eine computerweite virtuelle 480 Pixel-Zeichenbereich anordnen. Dies bedeutet, dass wenn Ihr HTML-Elemente die Breite in Prozent definieren, die Prozentsätze in Bezug auf diese 480 Pixeln Breite, nicht die standardmäßigen Breite des Viewports interpretiert werden. Daher ist der Benutzer weniger wahrscheinlich, Zoomen und Schwenken, horizontal – erheblich verbessern die benutzererfahrung beim mobilen Browsen zu müssen.

Wenn Sie die Breite des Viewports physischen Pixel des Geräts entsprechend möchten, können Sie Folgendes angeben:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Damit dies ordnungsgemäß funktioniert, Sie müssen nicht explizit zu erzwingen Elementen, die die Breite überschreiten (z. B. mithilfe einer *Breite* Attribut- oder CSS-Eigenschaft), andernfalls wird der Browser erzwungen werden, einen größeren Viewport unabhängig davon verwenden. Siehe auch: [Weitere Details zu den nicht dem Standard entsprechende Viewporttag](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Unterstützt die meisten modernen Smartphones *duale Ausrichtung*: sie können in entweder hoch-oder Querformat gehalten werden. Daher ist es wichtig, nicht auf Annahmen über die Bildschirmbreite in Pixel. Ausgegangen Sie nicht selbst wird davon, dass die Bildschirmbreite festgelegt ist, da der Benutzer ihre Geräte neu ausrichten kann, während sie auf der Seite sind.

Ältere Windows Mobile und Blackberry-Geräte akzeptiert möglicherweise auch die folgenden Meta-Tags im Seitenkopf Inhalt informiert für mobile Geräte optimiert wurde und aus diesem Grund sollten nicht transformiert.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Eine Liste der mobilen Geräteemulatoren und Simulatoren, die zum Testen Ihrer mobilen ASP.NET-Webanwendung können, finden Sie auf der Seite [simulieren von beliebten Mobilgeräten zu Testzwecken](../mobile/device-simulators.md).

## <a name="credits"></a>Mitwirkende

- Hauptautor: Steven Sanderson
- Prüfer / zusätzliche Autoren content: James Rosewell, Mikael Söderström, Scott Hanselman, Scott Hunter
