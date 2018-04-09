---
uid: whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
title: 'Gewusst wie: Hinzufügen mobiler Seiten zu Ihren ASP.NET Web Forms-/ MVC-Anwendung | Microsoft Docs'
author: rick-anderson
description: Dieser Vorgehensweise beschreibt verschiedene Möglichkeiten, optimierte für mobile Geräte von Ihrer ASP.NET Web Forms Seiten dienen / MVC-Anwendung, und schlägt vor, Architektur und...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2011
ms.topic: article
ms.assetid: 3124f28e-cc32-418a-afe3-519fa56f4c36
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application
msc.type: content
ms.openlocfilehash: a8358b91ca424f4f3e576057ab43d850081dda60
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
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
> Dieser Vorgehensweise beschreibt verschiedene Möglichkeiten, optimierte für mobile Geräte von Ihrer ASP.NET Web Forms Seiten dienen / MVC-Anwendung, und schlägt vor, Architektur und Entwerfen Punkte zu berücksichtigen, wenn Sie eine Breite Palette von Geräten zu verwenden. Dieses Dokument wird auch erläutert, warum die ASP.NET Mobile-Steuerelemente von ASP.NET 2.0 auf 3.5 sind jetzt veraltet und werden einige moderne behandelt.


## <a name="contents"></a>Inhalt

- Übersicht
- Architektur-Optionen
- Browser und das Gerät
- ASP.NET Web Forms-Anwendungen können wie Mobile-spezifische Seiten darstellen.
- ASP.NET MVC-Anwendungen können wie Mobile-spezifische Seiten darstellen.
- Zusätzliche Ressourcen

Herunterladbare Codebeispiele veranschaulichen dieses Whitepaper Techniken für ASP.NET Web Forms und MVC, finden Sie unter [Mobile Apps und Websites mit ASP.NET](https://docs.microsoft.com/aspnet/mobile/overview).

## <a name="overview"></a>Übersicht

Mobile Geräte – Smartphones, Feature-Telefone und Tablets – weiterhin als Mittel zum Zugriff auf das Web Popularität gewinnen. Für viele Entwickler von Websites und Unternehmen weborientierte bedeutet dies, dass sie immer wichtiger Besuchern, die diesen Geräten ein hervorragendes browsing-Erlebnis bereit ist.

### <a name="how-earlier-versions-of-aspnet-supported-mobile-browsers"></a>Wie frühere Versionen von ASP.NET unterstützten mobilen Browsern

ASP.NET Version 2.0, 3.5 enthalten *ASP.NET Mobile-Steuerelemente*: einen Satz von Serversteuerelementen für mobile Geräte in der *"System.Web.Mobile.dll"* Assembly und die  *System.Web.UI.MobileControls* Namespace. Die Assembly ist immer noch in ASP.NET 4 enthalten, aber es ist veraltet. Entwicklern werden empfohlen, zum Migrieren zu moderneren Ansätze, wie z. B. die in diesem Dokument beschrieben.

Der Grund, warum ASP.NET Mobile-Steuerelemente als veraltet markiert wurden, ist, dass am Entwurf für Mobiltelefone ausgerichtet ist, die allgemeine um 2005 und früher waren. Die Steuerelemente hauptsächlich dienen als Rendern WML oder cHTML Markup (anstelle von regelmäßigen HTML) für den WAP-Browsern diesem Zeitraum zurück. Jedoch WAP WML- und cHTML nicht mehr relevant für die meisten Projekte, da HTML jetzt die weit verbreitete Markupsprache für Desktops und mobile Browser gleichermaßen geworden ist.

### <a name="the-challenges-of-supporting-mobile-devices-today"></a>Die schwierigkeiten der Unterstützung von mobiler Geräts heute

Obwohl mobile Browsern nun fast universell HTML unterstützen, werden Sie dennoch viele Herausforderungen stoßen, wenn ins Leben gerufen, zum Erstellen von überzeugenden Anwendungen für mobile Browser:

- ***Displaygröße*** – Mobile Geräte variieren erheblich in Form, und ihre Bildschirme sind häufig wesentlich kleiner als Desktopmonitore. Daher müssen Sie möglicherweise völlig unterschiedlich Seitenlayouts dafür zu entwerfen.
- ***Geben Sie Methoden*** – einige Geräte sind identisch, einige Styluses haben, und andere Touch verwenden. Sie müssen möglicherweise mehrere Navigation Mechanismen und Daten Eingabemethoden berücksichtigen.
- ***Einhaltung von Standards*** – viele mobile Browser unterstützen nicht die aktuellsten Standards für HTML, CSS und JavaScript.
- ***Bandbreite*** – mobilfunkdaten netzwerkleistung äußerst variiert, und einige Endbenutzern befinden sich auf Tarifen, die von der Megabyte in Rechnung zu stellen.

Es gibt keine allgemeingültige Lösung; die Anwendung müssen Aussehen und Verhalten sich unterschiedlich, gemäß dem Gerät darauf zugreifen. Je nachdem welche Maß an Unterstützung mobilen Geräten werden soll kann dies eine größere Herausforderung für Webentwickler als jemals der Desktop "Browser konkurrierende wurde" sein.

Entwickler, die Unterstützung mobiler Browser zum ersten Mal häufig anfänglich annähert vorstellen, dass es nur ist wichtig, um die neuesten und hoch entwickelte Smartphones (z. B. Windows Phone 7, iPhone oder Android), vielleicht zu unterstützen, da Entwickler häufig persönlich etwa besitzen Geräte. Allerdings kostengünstiger Telefone sind immer noch äußerst beliebt und ihrer Inhaber verwenden, um das Web – insbesondere in Ländern durchsuchen, in dem Mobiltelefone leichter als ein Breitbandverbindung sind. Ihr Unternehmen müssen entscheiden, welcher Bereich von Geräten in Hinblick auf die wahrscheinlichsten Kunden zu unterstützen. Wenn Sie ein online Broschüren für eine Luxus Integrität Spa erstellen, können Sie als Business nur für die erweiterte Smartphones gelten Entscheidungshilfe während, wenn Sie ein Ticket Buchungssystem für eine im Kino erstellen, Sie wahrscheinlich Besucher mit weniger leistungsstarke Funktion berücksichtigen müssen Telefone.

## <a name="architectural-options"></a>Architektur-Optionen

Bevor wir auf die spezifische technische Details des ASP.NET Web Forms- oder MVC erhalten, beachten Sie, dass Entwickler von Websites in der Regel drei wichtigsten Optionen zur Unterstützung mobiler Browser:

1. ***Nichts Unternehmen –*** können Sie einfach eine standardmäßige, Desktop-orientierten Webanwendung erstellen und basieren auf mobilen Browsern um akzeptabler Geschwindigkeit zu rendern. 

    - **Vorteil**: Es ist die kostengünstigste Möglichkeit, implementieren und zu verwalten – ohne zusätzliche Arbeit
    - **Nachteil**: Gibt die schlechteste endbenutzererfahrung: 

        - Die neuesten Smartphones kann ebenso gut wie einen Desktopbrowser HTML-rendern, aber Benutzer werden weiterhin erzwungen werden, Zoomen und Bildlauf und vertikal um Ihren Inhalt auf einem kleinen Bildschirm zu nutzen. Dies ist alles andere als optimal.
        - Ältere Geräte und Feature-Smartphones können zum Rendern von Markup zufriedenstellend so fehlschlagen.
        - Selbst auf die neueste tabletgeräte (dessen Bildschirme können so groß wie der Laptop Bildschirme sein), andere Interaktion Regeln gelten. Touch-basierte Eingabe funktioniert am besten mit größeren Schaltflächen und Weiterverbreitung weiter auseinander verknüpft und es gibt keine Möglichkeit, ein Mauszeiger auf ein verkürztes Menü zeigen.
2. ***Löst das Problem auf dem Client* –** mit sorgfältige Verwendung von CSS und [schrittweise zunehmenden Verbesserungen](http://en.wikipedia.org/wiki/Progressive_enhancement) können Markup, Formate und Skripts, die Anpassung an den Browser, die sie ausgeführt wird. Z. B. mit [CSS 3-Medienabfragen](http://www.w3.org/TR/2010/CR-css3-mediaqueries-20100727/), Sie erstellen einen mehrspaltigen Layout, der in einer einzelnen Spaltenlayout auf Geräten umwandelt, deren Bildschirme enger gefasst als einen ausgewählten Schwellenwert sind. 

    - **Vorteil**: optimiert Rendering für das bestimmte Gerät verwendet, auch für unbekannte zukünftige Geräte gemäß dem Bildschirm und die Merkmale der Eingabe haben
    - **Vorteil**: einfach können Sie die serverseitige Logik für alle Gerätetypen – minimale Duplizierung des Codes oder der Aufwand freigeben
    - **Nachteil**: Mobilgeräte unterscheiden sich daher von desktop-Geräte, die Sie tatsächlich von mobilen-Seiten von Ihrem desktop Seiten vollständig unterscheiden ggf. unterschiedliche Informationen angezeigt. Solche Variationen können ineffizient sein oder gar nicht stabil zu erreichen über CSS allein, vor allem deshalb wie inkonsistent ältere Geräte interpretieren CSS-Regeln. Dies gilt insbesondere dann von CSS 3 Attributen.
    - **Nachteil**: bietet keine Unterstützung für unterschiedliche serverseitige Logik und Workflows für verschiedene Geräte. Sie implementieren nicht möglich ist, z. B. einen vereinfachte Warenkorb Einkaufswagen Checkout-Workflow für mobile Benutzer über CSS allein.
    - **Nachteil**: keine ineffizienten Auslastung der Bandbreite. Ihr Server möglicherweise zum Übertragen von Markup und Formate, die für alle möglichen-Geräte gelten, obwohl das Zielgerät nur eine Teilmenge dieser Daten verwendet wird.
3. ***Löst das Problem auf dem Server* –** , wenn der Server weiß, welches Gerät – zugreift oder zumindest die Merkmale des Geräts, z. B. Bildschirmgröße und Eingabemethode, und, ob es sich um ein mobiles Gerät – ist können sie eine unterschiedliche Logik ausgeführt und Ausgabe verschiedene HTML-Markups. 

    - **Vorteil:** maximale Flexibilität. Es gibt keine Beschränkung, wie viel können variieren die serverseitige Logik für mobile Geräte oder das Markup für das gewünschte, gerätespezifischen Layout optimieren.
    - **Vorteil:** effiziente Nutzung der Bandbreite. Sie müssen nur zum Übertragen von Markup und Stilinformationen, die das Zielgerät verwenden soll.
    - **Nachteile:** manchmal erzwingt Wiederholung Aufwand oder Code (z. B., wodurch Sie ähnliche aber leicht abweichende Kopien Ihrer Web Forms-Seiten oder MVC-Ansichten erstellen wird). In denen möglicherweise möglich Sie out gemeinsame Logik in eine darunter liegende Ebene oder Dienst, aber dennoch einige Teile des UI-Codes oder Markup objektiv werden dupliziert und anschließend parallel verwaltet werden.
    - **Nachteile:** Erkennung ist keine einfache Aufgabe. Er erfordert eine Liste oder eine Datenbank mit bekannten Gerätetypen und ihre Eigenschaften (die möglicherweise nicht immer perfekt auf dem neuesten Stand) und ist nicht unbedingt jede eingehende Anforderung genau übereinstimmen. Dieses Dokument beschreibt einige Optionen und deren Fehlerquellen später erneut.

Um die besten Ergebnisse zu erhalten, finden die meisten Entwickler, die sie benötigen, um Optionen (2) und (3) kombinieren. Kleine stilistischer Unterschiede sind am besten auf dem Client mithilfe der CSS- oder sogar JavaScript untergebracht, wohingegen wesentliche Unterschiede hinsichtlich der Daten, Workflow oder Markup möglichst effiziente Ausführung in serverseitigen Code implementiert sind.

### <a name="this-paper-focuses-on-server-side-techniques"></a>Dieses Dokument behandelt vorrangig serverseitige Techniken

Da ASP.NET Web Forms und MVC beide Technologien in erster Linie serverseitige sind, konzentriert dieses Whitepaper serverseitige Techniken sich auf, mit denen Sie verschiedene Markup und die Logik für mobile Browser erzeugen können. Natürlich können Sie dies auch mit beliebigen Client-Side-Verfahren (z. B. CSS 3 Medienabfragen, JavaScript Progressive-Erweiterung) kombinieren, aber mehr wenigen Webdesign als ASP.NET Development ist.

## <a name="browser-and-device-detection"></a>Browser und das Gerät

Die wichtigsten Voraussetzung für alle serverseitigen Techniken für die Unterstützung mobiler Geräte ist wissen, welches Gerät Besucher verwendet wird. Tatsächlich ist sogar noch besser kennen, die Anzahl Hersteller und Modell des Geräts ist das Wissen der *Merkmale* des Geräts. Eigenschaften können umfassen:

- Ist es ein mobiles Gerät?
- Eingabemethode (Maus und Tastatur, Fingereingabe, Zehnertastatur, Joystick,...)
- Displaygröße (physisch und in Pixel)
- Unterstützte Medien und Datenformate
- Usw.

Es ist besser, stellen Sie Ihre Entscheidungen basierend auf Eigenschaften als Modellnummer, da dann verlaufen besser ausgestattet, um zukünftige Geräte zu behandeln.

### <a name="using-aspnets-built-in-browser-detection-support"></a>Verwendung von ASP. NET integrierten Erkennung Browserunterstützung

ASP.NET Web Forms und MVC-Entwickler können sofort wichtige Merkmale eines Browsers besuchen durch Überprüfen der Eigenschaften eines Ermitteln der *Request.Browser* Objekt. Beispielsweise finden Sie unter

- Request.Browser.IsMobileDevice
- Request.Browser.MobileDeviceManufacturer, Request.Browser.MobileDeviceModel
- Request.Browser.ScreenPixelsWidth
- Request.Browser.SupportsXmlHttp
- ... und viele andere

Im Hintergrund die ASP.NET-Plattform entspricht den eingehenden *Benutzer-Agent-* (UA)-HTTP-Header für reguläre Ausdrücke in einem Satz von Browser-Definitions-XML-Dateien. Standardmäßig die Plattform enthält Definitionen für viele allgemeine mobile Geräte, und Sie können benutzerdefinierte Browser-Definitionsdateien hinzufügen, damit andere Benutzer, die Sie erkennen möchten. Weitere Einzelheiten finden Sie in der MSDN-Seite [ASP.NET Webserver- und Browserfunktionen](https://msdn.microsoft.com/library/x3k2ssx2.aspx).

### <a name="using-the-wurfl-device-database-via-51degreesmobi-foundation"></a>Mithilfe der Datenbank über 51Degrees.mobi Foundation WURFL-Geräte

Während des ASP. NET Browserunterstützung für Erkennung integrierte wird für viele Anwendungen ausreichend sein, es gibt zwei wichtigsten Fälle nicht gerecht werden kann:

- ***Die neuesten Geräte erkannt werden sollen***(ohne manuell erstellen Browser-Definitionsdateien für sie). Beachten Sie, dass .NET 4 Browser-Definitionsdateien nicht neu genug, um Windows Phone 7, Android-Telefone, Opera Mobile-Browser oder Apple iPads zu erkennen sind.
- ***Sie benötigen weitere Detailinformationen zu Gerätefunktionen***. Möglicherweise müssen Sie die Eingabemethode für ein Gerät (z. B. Touch Vs Zehnertastatur) erfahren oder formatiert, welche Audio den Browser unterstützt. Diese Informationen nicht in den standard-Browser-Definitionsdateien verfügbar.

Die [ *Wireless Universal Ressourcendatei* (WURFL) Projekt](http://wurfl.sourceforge.net/) viel mehr auf dem neuesten Stand und detaillierte Informationen zu mobilen Geräten verwendet heute verwaltet.

Die gute Nachricht für .NET-Entwickler ist, dass ASP. NET Erkennung Browserfunktion ist erweiterbar, daher ist es möglich, verbessern, um diese Probleme zu umgehen. Sie können z. B. die open Source hinzufügen [ *51Degrees.mobi Foundation* ](https://github.com/51Degrees/dotNET-Device-Detection) Bibliothek zum Projekt. Es ist ein ASP.NET IHttpModule oder Browser Funktionen Anbieter (verwendbar in Web Forms und MVC-Anwendungen), die direkt WURFL Daten liest und es in ASP-hooks. NET integrierten Browser Erkennungsmechanismus. Nach der Installation des Moduls *Request.Browser* plötzlich viel genauer und ausführliche Informationen enthält: es ordnungsgemäß viele Geräte bereits erwähnt erkennt und ihre Funktionen (einschließlich auflisten zusätzliche Funktionen wie z. B. Eingabemethoden). Finden Sie weitere Details des Projekts Dokumentation.

## <a name="how-web-forms-applications-can-present-mobile-specific-pages"></a>Web Forms-Anwendungen können wie Mobile-spezifische Seiten darstellen.

Hier wird standardmäßig eine brandneue Web Forms-Anwendung auf allgemeine mobilen Geräten wie angezeigt:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image1.png)

Es ist offensichtlich, wird weder Layout sieht sehr mobilgerätefreundliche – auf dieser Seite wurde für einen großen, Querformat-Monitor nicht für einen kleinen Hochformat Bildschirm entworfen. Was können Sie dies tun?

Wie weiter oben in diesem Dokument erläutert wird, sind viele Methoden zum Anpassen von Seiten für mobile Geräte. Einige Techniken basieren auf Server, auf dem Client von anderen Benutzern ausgeführt.

### <a name="creating-a-mobile-specific-master-page"></a>Erstellen eine spezifische Mobile Gestaltungsvorlage

Je nach Ihren Sie möglicherweise verwenden die gleichen Web Forms für alle Besucher konfigurieren, allerdings müssen zwei separate Masterseiten: eine für Desktops Besucher, ein weiteres für mobile Besucher. Dies bietet Ihnen die Flexibilität, die CSS-Stylesheet oder mobile Geräten, ohne dass Sie eine Seitenlogik duplizieren entsprechend der obersten Ebene HTML-Markup ändern.

Dies ist einfach. Beispielsweise können Sie einen Handler PreInit wie im folgenden ein Webformular hinzufügen:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample1.cs)]

Erstellen Sie jetzt eine Gestaltungsvorlage Mobile.Master in den Ordner der obersten Ebene Ihrer Anwendung aufgerufen, und es wird verwendet, wenn ein mobiles Gerät erkannt wird. Ihre mobile Gestaltungsvorlage kann eine Mobile-spezifische CSS-Stylesheet ggf. verweisen. Desktop Besucher werden Ihrer Standardmasterseite nicht auf den mobilen Faktor weiterhin angezeigt werden.

### <a name="creating-independent-mobile-specific-web-forms"></a>Erstellen unabhängige spezifische Mobile Web Forms

Maximale Flexibilität erhalten können Sie weitere als separate Masterseiten für verschiedene Gerätetypen wechseln. Sie können zwei implementieren *völlig separate Sätze von Web Forms-Seiten* – eine für Desktopbrowsern festgelegt, einen weiteren Satz für mobile Geräte. Dies funktioniert am besten, wenn Sie sehr unterschiedliche Informationen oder Workflows mobilen Besuchern bereitstellen möchten. Im restlichen Teil dieses Abschnitts wird dieser Ansatz im Detail beschrieben.

Vorausgesetzt, dass Sie bereits eine Web Forms-Anwendung für Desktopbrowsern entwickelt haben, ist die einfachste Möglichkeit, um den Vorgang fortzusetzen, erstellen einen Unterordner namens "Mobile" innerhalb des Projekts, und erstellen Ihre mobile Seiten vorhanden. Konstruieren einer gesamten Unterwebsite mit eigenen Masterseiten, Stylesheets und Seiten, über die gleichen Techniken, die Sie für eine andere Web Forms-Anwendung verwenden würden. Sie müssen nicht unbedingt eine mobile äquivalente erzeugen *jedes* Ihrer desktop-Website auf der Seite; Sie können auswählen, welche Teilmenge der Funktionen für mobile Besucher sinnvoll ist.

Mobilen Seiten können verwenden gemeinsam freigegebene statische Ressourcen (z. B. Bilder, JavaScript und CSS-Dateien) mit Ihren regulären Seiten gegebenenfalls. Da wird der Ordner "Mobile" *nicht* gekennzeichnet werden als separate Anwendung beim Hosten in IIS (es ist nur ein einfaches Unterordner des Web Forms-Seiten), es wird auch freigeben alle die gleiche Konfiguration, Sitzungsdaten und anderer Infrastruktur als Ihre Desktop Seiten.

> [!NOTE]
> Da dieser Ansatz i. d. r. einige Duplizierung des Codes beinhaltet (mobile Seiten sind wahrscheinlich einige ähnlichkeiten mit desktop Seiten freigeben), es ist wichtig, Factor alle allgemeinen Business Anwendungslogik oder der Zugriff Code in einer freigegebenen zugrunde liegenden Ebene oder eines Diensts. Andernfalls sind Sie doppelte den Endbenutzern das Erstellen und Verwalten Ihrer Anwendung.


#### <a name="redirecting-mobile-visitors-to-your-mobile-pages"></a>Umleiten von mobilen Besucher von mobilen-Seiten

Es ist häufig sinnvoll, nur auf mobile Besucher mobilen Seiten umleiten der *erste* in ihre Browsersitzung (und nicht bei jeder Anforderung in ihrer Sitzung) angefordert werden, da:

- Sie können dann problemlos erlauben Ihren desktop Seiten zugreifen, wenn sie möchten, einfach einen Link auf der Masterseite, die auf "Desktopversion" geht – mobile Besucher. Der Besucher wird nicht an einer mobilen Seite umgeleitet werden, weil es nicht mehr die erste Anforderung in ihre Sitzung ist.
- Er vermeidet das Risiko von stören Anforderungen für dynamische Ressourcen, die gemeinsam von desktop und mobile Teile Ihrer Website (z. B., wenn Sie eine allgemeine Web Form, in denen Desktop- und mobile Teile Ihrer Website in einem IFRAME oder bestimmte Ajax-Handler angezeigt haben)

Zu diesem Zweck können Sie Ihre Logik Umleitung in Platzieren einer **Sitzung\_starten** Methode. Fügen Sie z. B. die folgende Methode der Global.asax.cs-Datei:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample2.cs)]

#### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurieren der Formularauthentifizierung zur Einhaltung von mobilen-Seiten

Beachten Sie, dass die Formularauthentifizierung von bestimmten Annahmen zu, in dem sie Besucher während und nach der Authentifizierung umgeleitet werden kann:

- Wenn ein Benutzer authentifiziert werden muss, Formularauthentifizierung weitergeleitet wird ihnen, Ihre Desktops Anmeldeseite, unabhängig davon, ob sie einen Desktopbrowser oder einen mobilen Benutzer sind (weil er lediglich ein Konzept von verfügt *eine* -Anmelde-URL). Angenommen, Sie Ihre mobile Anmeldeseite unterschiedlich formatieren möchten, müssen Sie Ihrer desktop-Anmeldeseite zu verbessern, sodass mobile Benutzer auf einer separaten mobile-Anmeldeseite umgeleitet. Z. B. den folgenden Code zum Hinzufügen Ihrer **Desktop** Seite Code-Behind-Anmeldung: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample3.cs)]
- Nachdem ein Benutzer erfolgreich anmeldet, Formularauthentifizierung standardmäßig leitet sie an Ihre Desktops Startseite (weil er lediglich ein Konzept von verfügt *eine* Standardseite ""). Sie müssen Ihre mobile Anmeldeseite zu erweitern, sodass leitet an die Mobile-Startseite nach einer erfolgreichen Protokoll an. Z. B. den folgenden Code zum Hinzufügen Ihrer **mobile** Seite Code-Behind-Anmeldung: 

    [!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample4.cs)]
  
  Mit diesem Code wird davon ausgegangen, dass Ihre Seite Anmeldung Serversteuerelement LoginUser, wie der Standardprojektvorlage aufgerufen hat.

### <a name="working-with-output-caching"></a>Arbeiten mit Zwischenspeichern der Ausgabe

Wenn Sie die Ausgabe-caching verwenden, beachten Sie, dass standardmäßig ist es möglich, dass ein Desktopbenutzer eine bestimmte URL (verursacht die Ausgabe zwischengespeichert werden soll) besuchen, von einem mobilen Benutzer ausgeführt wird, klicken Sie dann die zwischengespeicherte Ausgabe desktop erhält. Diese Warnung gilt, ob Sie nur variieren die Masterseite nach Gerätetyp, oder implementieren völlig separater Web Forms pro Gerätetyp.

Um das Problem zu vermeiden, können Sie ASP.NET anzuweisen, variieren den Cacheeintrag nach, ob der Besucher ein mobiles Gerät verwendet wird. Fügen Sie einen Parameter VaryByCustom auf der Seite OutputCache Deklaration wie folgt:

[!code-aspx[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample5.aspx)]

Als Nächstes definieren *IsMobileDevice* als Cache, benutzerdefinierte Parameter durch Hinzufügen der folgenden Methode zu überschreiben, mit Ihrer Datei Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample6.cs)]

Dadurch wird sichergestellt, dass mobile Besucher der Seite keine Ausgabe, die zuvor im Cache abgelegt, durch einen desktop Besucher erhalten.

### <a name="a-working-example"></a>Ein funktionsfähiges Beispiel

Um dieser Techniken in Aktion zu sehen, herunterladen [Codebeispiele in diesem Whitepaper](https://docs.microsoft.com/aspnet/mobile/overview). Die Web Forms-beispielanwendung leitet automatisch mobile Benutzer auf einen Satz von Mobile-spezifische Seiten im Unterordner Mobile. Das Markup und formatieren diese Seiten ist besser optimiert für mobile Browser, wie Sie in den folgenden Screenshots sehen können:

![](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/_static/image2.png)

Weitere Tipps zum Optimieren der Markup und CSS für mobile Browser finden Sie im Abschnitt "Mobile Webseiten für mobile Browser formatieren" weiter unten in diesem Dokument.

## <a name="how-aspnet-mvc-applications-can-present-mobile-specific-pages"></a>ASP.NET MVC-Anwendungen können wie Mobile-spezifische Seiten darstellen.

Da das Model View Controller-Muster Anwendungslogik (in Controllern) von Präsentationslogik (in Sichten) entkoppelt, können Sie für die Behandlung der Unterstützung von mobilen Geräten in serverseitigen Code aus einem der folgenden Ansätze:

1. ***Verwenden der gleichen Controller und Ansichten für Desktop- und mobile Browser Rendern von Ansichten mit anderen Razor-Layouts, je nach Gerät Typ *.** Diese Option funktioniert am besten, wenn Sie sind identische Daten auf allen Geräten anzeigen, jedoch einfach verschiedene CSS-Stylesheets angeben oder einige Elemente der obersten Ebene HTML-Webseiten ändern möchten.
2. ***Dieselben Controller für Desktop- und mobile Browser verwenden, aber unterschiedliche Sichten je nach Gerätetyp Rendern***. Diese Option funktioniert am besten, wenn Sie etwa die gleichen Daten anzeigen und entspricht den Workflows für Endbenutzer bereitstellen, aber sehr unterschiedliche HTML-Markup des verwendeten Geräts entsprechend rendern möchten.
3. ***Erstellen Sie separate Bereiche für Desktop- und mobilen Browsern, implementieren unabhängige Controller und Ansichten für jeden *.** Diese Option funktioniert am besten, wenn Sie sehr unterschiedliche Bildschirme anzeigen, enthält verschiedene Informationen und wodurch der Benutzer über unterschiedliche Workflows, die für ihren Gerätetyp optimiert. Einige Wiederholung des Codes kann bedeuten, aber Sie können durch gemeinsame Logik in einer zugrunde liegenden Ebene oder einem Dienst Ausklammern verringern.

Wenn Sie nutzen möchten die **erste** aus, und nur die Razor-Layout variieren pro Gerätetyp, es ist sehr einfach. Ändern Sie einfach Ihre \_ViewStart.cshtml Datei wie folgt:

[!code-cshtml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample7.cshtml)]

Nun können Sie eine Mobile-spezifische Layout mit der Bezeichnung erstellen \_LayoutMobile.cshtml mit einer Seitenstruktur und CSS-Regeln für mobile Geräte optimiert.

Wenn Sie nutzen möchten die **zweite** option Render völlig unterschiedliche Ansichten gemäß den Besucher Gerätetyp, finden Sie unter [Blogbeitrag des Scott Hanselman](http://www.hanselman.com/blog/ABetterASPNETMVCMobileDeviceCapabilitiesViewEngine.aspx).

Der Rest dieses Dokuments behandelt schwerpunktmäßig die **dritte** Option – erstellen separate Controller *und* Ansichten für mobile Geräte – damit können Sie steuern, genau die Teilmenge der Funktionen für mobile Besucher angeboten wird.

### <a name="setting-up-a-mobile-area-within-your-aspnet-mvc-application"></a>Einrichten von einem mobilen Bereich innerhalb der ASP.NET MVC-Anwendung

Sie können einen Bereich namens "Mobile", um eine vorhandene ASP.NET MVC-Anwendung auf die übliche Weise hinzufügen: mit der rechten Maustaste auf den Projektnamen im Projektmappen-Explorer, wählen Sie dann hinzufügen À Bereich. Sie können dann Controller und Ansichten hinzufügen, wie bei jedem anderen Bereich innerhalb einer ASP.NET MVC-Anwendung. Z. B. Fügen Sie an Ihren mobilen Bereich einen neuen Domänencontroller HomeController als eine Startseite für mobile Besucher fungieren aufgerufen hinzu.

### <a name="ensuring-the-url-mobile-reaches-the-mobile-homepage"></a>Sicherstellen der URL-/Mobile erreicht die mobile-Startseite

Wenn Sie die URL-/Mobile die Index-Aktion auf HomeController in Ihrer mobilen Bereich erreichen möchten, müssen Sie zwei kleinere Änderungen an der Routingkonfiguration vornehmen. Zunächst aktualisiert, Ihre Klasse MobileAreaRegistration HomeController die Standard-Controller in Ihrem mobilen Bereich ist wie im folgenden Code gezeigt:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample8.cs)]

Dies bedeutet, dass mobile Homepage wird jetzt gefunden /Mobile statt/Mobile/Home, da nun "Home" ist die implizit Controller Standardname für mobile-Seiten.

Beachten Sie, dass von der Anwendung (d. h. mobile derjenige, zusätzlich zu den vorhandenen Desktop einer) eine zweite HomeController hinzufügen, die reguläre desktop Homepage beeinträchtigt haben müssen. Sie schlägt fehl mit Fehler "*wurden mehrere Typen gefunden, die den Controller, mit dem Namen"Home"entsprechen*". Zum Beheben dieses Problems aktualisieren Sie Ihre der obersten Ebene Routingkonfiguration (in Global.asax.cs) um anzugeben, dass Ihre Desktops HomeController Priorität beim Entwickeln einer Mehrdeutigkeit berücksichtigen sollten:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample9.cs)]

Nun der Fehler beseitigen und die URL http:// geleitet werden<em>Standortname</em>/ gelangen, der desktop-Startseite, und http://<em>Standortname</em>/mobile/ mobile Homepage erreicht wird.

### <a name="redirecting-mobile-visitors-to-your-mobile-area"></a>Umleiten von mobilen Besuchern an Ihren mobilen Bereich

Es gibt viele verschiedene Erweiterungspunkte in ASP.NET MVC, daher gibt es viele Möglichkeiten der Umleitung Logik einfügen. Eine praktische Möglichkeit besteht darin ein Filterattribut [RedirectMobileDevicesToMobileArea], erstellen, die eine Umleitung ausführt, wenn die folgenden Bedingungen erfüllt sind:

1. Es ist die erste Anforderung in der Sitzung des Benutzers (d. h. Session.IsNewSession gleich "true")
2. Die Anforderung stammt von einem mobilen Browser (d. h. Request.Browser.IsMobileDevice gleich "true")
3. Der Benutzer nicht bereits eine Ressource im Bereich mobile anfordert (d. h. die *Pfad* Teil der URL beginnt nicht mit /Mobile)

Im herunterladbare Beispiel dieses Whitepaper enthält eine Implementierung dieser Logik. Es wird als ein Autorisierungsfilter, abgeleitet von AuthorizeAttribute, was, dass er ordnungsgemäß arbeiten kann bedeutet, selbst wenn Sie Ausgabecache verwenden (, andernfalls, wenn auf einen desktop Besucher erste greift auf eine bestimmte URL, den desktop Ausgabe zwischengespeichert und dann bereitgestellt werden möglicherweise implementiert nachfolgende mobile Besucher).

Da es sich um einen Filter handelt, können Sie entweder für die anzuwendende bestimmten Controller und Aktionen, z. B. auswählen,

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample10.cs)]

… oder Sie können Sie als eine MVC 3 für alle Controller und Aktionen anwenden *globalen Filter* in der Datei Global.asax.cs:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample11.cs)]

Im herunterladbare Beispiel veranschaulicht auch Unterklassen dieses Attributs erstellen können, die an bestimmten Positionen im Ihrer mobilen Region umleiten. Dies bedeutet z. B. können Sie:

- Registrieren Sie einen globalen Filter wie oben, der mobile Besucher für die mobile-Startseite standardmäßig sendet.
- Gelten Sie auch einen speziellen [RedirectMobileDevicesToMobileProductPage] Filter für eine "View Produkt"-Aktion, die mobile Besucher der mobilen Version von der Seite "Product" akzeptiert, die sie angefordert hatten.
- Gelten Sie auch andere speziellen Unterklassen des Filters für bestimmte Aktionen, die mobile Besucher der entsprechende mobilen Seite umleiten

### <a name="configuring-forms-authentication-to-respect-your-mobile-pages"></a>Konfigurieren der Formularauthentifizierung zur Einhaltung von mobilen-Seiten

Wenn Sie die Formularauthentifizierung verwenden, beachten Sie, dass wenn ein Benutzer benötigt, um sich anzumelden, automatisch den Benutzer mit einer einzelnen bestimmten "Anmelden" URL, leitet der standardmäßig **/Account/LogOn**. Dies bedeutet, dass mobile Benutzer an Ihre Aktion desktop "Anmelden" umgeleitet werden können.

Um dieses Problem zu vermeiden, fügen Sie Logik auf Ihrem desktop "Anmelden"-Aktion, sodass mobile Benutzer erneut an eine mobile "Anmelden" umgeleitet. Wenn Sie die Standard-Vorlage für ASP.NET MVC-Anwendung verwenden, aktualisieren Sie AccountController Anmeldung Aktion wie folgt:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample12.cs)]

… und implementieren Sie eine geeignete Mobile-spezifische "Anmelden" Aktion auf einem Domänencontroller AccountController in Ihrem mobilen aufgerufen.

### <a name="working-with-output-caching"></a>Arbeiten mit Zwischenspeichern der Ausgabe

Wenn Sie den Filter [OutputCache] verwenden, müssen Sie den Cacheeintrag, je nach Gerätetyp variieren erzwingen. Z. B. Folgendes schreiben:

[!code-javascript[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample13.js)]

Anschließend fügen Sie die folgende Methode der Global.asax.cs-Datei:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample14.cs)]

Dadurch wird sichergestellt, dass mobile Besucher der Seite keine Ausgabe, die zuvor im Cache abgelegt, durch einen desktop Besucher erhalten.

### <a name="a-working-example"></a>Ein funktionsfähiges Beispiel

Um dieser Techniken in Aktion zu sehen, herunterladen [dieses Whitepaper Code verknüpften Beispiele](https://docs.microsoft.com/aspnet/mobile/overview). Das Beispiel enthält eine ASP.NET MVC 3 (Release Candidate)-Anwendung, die zur Unterstützung mobiler Geräte, die mit den oben beschriebenen Methoden verbessert.

## <a name="further-guidance-and-suggestions"></a>Weitere Anleitungen und Vorschläge

Die folgende Darstellung bietet wendet auf Web Forms und MVC-Entwickler, die in diesem Dokument behandelten Verfahren verwenden.

### <a name="enhancing-your-redirection-logic-using-51degreesmobi-foundation"></a>Verbessern Ihre Logik kann mithilfe von 51Degrees.mobi Foundation Umleitung

Die Umleitung-Logik, die in diesem Dokument dargestellten perfekt reicht für Ihre Anwendung möglicherweise, aber es funktioniert nicht, wenn Sie benötigen, um Sitzungen zu deaktivieren, oder mit mobilen Browsern, die Cookies (diese Sitzungen sind keine), zurückgewiesen, da es nicht, ob eine bestimmte Anforderung wissen das erste Schema von diesem Besucher.

Bereits haben Sie gelernt, wie die Genauigkeit von ASP mit open-Source-51Degrees.mobi Foundation verbessert werden kann. NET Browser-Erkennung. Außerdem wird eine integrierte Funktionalität zum Umleiten von mobiler Besuchern an einen bestimmten Standort in der Datei "Web.config" konfiguriert wurde. Dies ist möglich, ohne je nach ASP.NET Sitzungen ausgeführt (und daher Cookies) synchronisierungsleistung, da ein temporäres Protokolls Hashes des Besuchers HTTP-Headern und IP-Adressen, damit es weiß, und zwar unabhängig davon, ob jede Anforderung von einem bestimmten Vistor der erste ist.

Das folgende Element dem FiftyOne-Abschnitt der Datei "Web.config" hinzugefügt, leitet die erste Anforderung von einem erkannte mobilen Gerät auf der Seite "~ / Mobile/Default.aspx. Alle Anforderungen an die Seiten im Ordner Mobile werden *nicht* umgeleitet werden, unabhängig vom Gerätetyp. Wenn das mobile Gerät für einen Zeitraum inaktiv war von 20 Minuten oder weitere das Gerät wird vergessen, und nachfolgende Anforderungen als neue für die Zwecke der Umleitung behandelt werden.

[!code-xml[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample15.xml)]

Weitere Informationen finden Sie unter [51degrees.mobi Foundation-Dokumentation](https://github.com/51Degrees/dotNET-Device-Detection).

> [!NOTE]
> Sie *können* verwenden 51Degrees.mobi Foundation Funktion zur automatischen bindungsumleitung in ASP.NET MVC-Anwendungen, aber Sie müssen die Konfiguration der Umleitung im Hinblick auf einfache URLs, die nicht im Hinblick auf routing-Parameter oder indem Sie die MVC-Filter definieren für Aktionen. Grund hierfür ist, (zum Zeitpunkt der Verfassung) 51Degrees.mobi Foundation Filter oder routing nicht erkennt.


### <a name="disabling-transcoders-and-proxy-servers"></a>Das Deaktivieren der Transcoder und Proxy-Servern

Mobiler Netzwerkbetreiber bestehen zwei allgemeine Ziele in ihre Herangehensweise an das mobile Internet:

1. Geben Sie als viel relevanten Inhalt wie möglich
2. Maximieren Sie die Anzahl der Kunden, die die Netzwerkbandbreite beschränkt Radio freigeben können

Da die meisten Webseiten für große Desktop große Bildschirme und schnell behoben-Line-breitbandverbindungen entwickelt wurden, verwenden Sie viele Operatoren *Transcoder* oder *Proxyserver* , die dynamisch Webinhalte zu ändern. Sie können Ihre HTML-Markup oder CSS entsprechend kleinere Bildschirmen (insbesondere für "Funktion Telefone", die die verarbeitungsgeschwindigkeit behandeln Sie komplexe Layouts fehlender) ändern und sie möglicherweise Ihre Images (erheblich reduziert wird ihre Qualität) komprimieren, um mindestgeschwindigkeiten für die Übermittlung zu verbessern.

Aber wenn Sie den Aufwand beim Erzeugen einer Mobilgeräte optimierte Version Ihres Standorts vorgenommen haben, Sie möchten wahrscheinlich nicht die Netzwerk-Operator, um alle weiteren stören. Sie können die folgende Zeile hinzufügen, auf der Seite\_Load-Ereignis in irgendeiner Form der ASP.NET Web:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample16.cs)]

Oder für eine ASP.NET MVC-Controller, konnte Sie Überschreibung der folgenden hinzugefügt werden, damit es für alle Aktionen auf diesem Controller gilt:

[!code-csharp[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample17.cs)]

Die resultierende HTTP-Nachricht informiert kompatibel W3C-Transcoder und Proxys nicht für den Inhalt ändern. Natürlich ist keine Garantie, dass ein mobiler Netzwerkbetreiber. diese Meldung berücksichtigt werden.

### <a name="styling-mobile-pages-for-mobile-browsers"></a>Formatieren von mobile-Seiten für mobile Browser

Es ist nicht Gegenstand dieses Dokuments, um ausführlich zu beschreiben welche Arten von HTML-Markup Arbeit ordnungsgemäß, oder die Web-Design-Techniken maximieren Verwendbarkeit auf bestimmten Geräten. Es hat sich zum Auffinden von einem ausreichend einfachen Layout für einen Bildschirm Mobile Größe optimiert, ohne mit unzuverlässigen HTML- oder CSS-Positionierung Tricks. Allerdings ist ein wichtiges Verfahren des Objektzuständen, die *Viewport Meta-Tag*.

Bestimmte modernen mobilen Browsern, in eine Aufwand Anzeige von Webseiten für Desktopmonitore, vorgesehen Rendern die Seite auf einem virtuellen Zeichenbereich, so genannte "Viewport" (z. B. der virtuellen Sichtbereich ist ein 980 Pixel auf dem iPhone und 850 Pixel breit auf Opera Mobile standardmäßig) und dann Skalieren Sie das Ergebnis nach unten, um auf dem Bildschirm physischen Pixel passt. Der Benutzer kann dann vergrößern und Schwenken, Viewports. Dies hat den Vorteil, dass sie im Browser die Seite in seiner vorgesehenen Layout anzuzeigen können, aber es auch ist hat dies den Nachteil, dass er erzwingt, Zoomen und Schwenken, die für den Benutzer ist. Wenn Sie für mobile Geräte entwerfen, ist es besser Entwurf für einen engen Bildschirm, so, dass keine vergrößern/verkleinern oder ein horizontaler Bildlauf erforderlich ist.

Eine Möglichkeit, um mobilen Browser wie breit Viewport liegen ist über die nicht dem Standard entsprechende *Viewport* Meta-Tag. Z. B. Wenn Sie die folgenden HEAD-Abschnitt der Seite hinzufügen,

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample18.html)]

… Klicken Sie dann Unterstützung Smartphone-Browser wird die Seite eine computerweite virtuelle 480 Pixel-Zeichenbereich anordnen. Dies bedeutet, dass die HTML-Elementen die Breite in Prozent definieren, die Prozentsätze in Bezug auf diese 480 Pixel Breite, nicht die Standardbreite Viewport interpretiert werden. Daher ist der Benutzer weniger wahrscheinlich, dass Sie Zoomen und Schwenken horizontal – erheblich verbessern die mobile Browserumgebung haben.

Gegebenenfalls die Breite des Anzeigebereichs entsprechend der physischen Pixel des Geräts können Sie Folgendes angeben:

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample19.html)]

Damit dies ordnungsgemäß funktioniert, Sie müssen nicht explizit zu erzwingen Elemente dieser Breite überschreitet (z. B. mithilfe einer *Breite* Attribut- oder CSS-Eigenschaft), andernfalls wird der Browser erzwungen werden, einen größeren Viewport unabhängig verwenden. Siehe auch: [Weitere Details für das Viewporttag nicht dem Standard entsprechende,](https://developer.apple.com/library/safari/#documentation/AppleApplications/Reference/SafariHTMLRef/Articles/MetaTags.html).

Unterstützt die meisten modernen Smartphones *duale Ausrichtung*: sie können im Hochformat oder Querformat aufrechterhalten werden. Daher ist es wichtig keine Annahmen über die Bildschirmbreite in Pixel. Ausgegangen Sie nicht selbst wird davon, dass die Bildschirmbreite behoben ist, da der Benutzer ihr Gerät neu ausrichten kann, während sie auf der Seite sind.

Ältere Windows Mobile- und Blackberry-Geräte akzeptiert möglicherweise auch die folgenden Meta-Tags in den Seitenkopf, um Inhalt zu informieren für mobile Geräte optimiert wurde und daher nicht transformiert werden sollen.

[!code-html[Main](add-mobile-pages-to-your-aspnet-web-forms-mvc-application/samples/sample20.html)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

Eine Liste der mobilen Geräteemulatoren und Simulatoren, Sie Ihre mobile ASP.NET Web-Anwendung zu testen können, finden Sie auf der Seite [verbreitete mobile Geräte zu Testzwecken simulieren](../mobile/device-simulators.md).

## <a name="credits"></a>Mitwirkende

- Primäre Autor: Steven Sanderson
- Bearbeiter / zusätzliche Inhalte Writer: James Rosewell, Mikael Söderström, Scott Hanselman, Scott Hunter
