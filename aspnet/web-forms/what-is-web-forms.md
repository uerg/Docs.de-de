---
uid: web-forms/what-is-web-forms
title: Was ist, dass Web Forms | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: dcaba60a8640ce83f73460a5c8ee457257b9397e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528609"
---
<a name="what-is-web-forms"></a>Was ist Web Forms
====================
ASP.NET Web Forms ist ein Bestandteil der ASP.NET Web Application Framework und ist im Lieferumfang [Visual Studio](https://www.asp.net/downloads). Es sich um einen der vier Programmiermodelle, die Sie verwenden können, um ASP.NET-Webanwendungen zu erstellen, die anderen sind ASP.NET MVC, ASP.NET Web Pages und ASP.NET Single Page Applications.

WebForms sind die Seiten, die Ihre Benutzer anfordern, über einen Browser. Diese Seiten können mit einer Kombination aus HTML-Code geschrieben werden Clientskripts, Serversteuerelemente und Servercode. Wenn ein Benutzer eine Seite anfordert, kompiliert und auf dem Server ausgeführt wird, durch das Framework und das Framework generiert dann das HTML-Markup, das der Browser rendern kann. Eine ASP.NET Web Forms-Seite zeigt die Informationen in alle Browser oder Client-Gerät an den Benutzer.

ASP.NET Web Forms können Sie mithilfe von Visual Studio erstellen. Die Visual Studio Umgebung IDE (Integrated Development) können Sie die Drag & drop-Steuerelemente zum Erstellen von Web Forms-Seite. Sie können dann einfach Eigenschaften, Methoden und Ereignisse für Steuerelemente auf der Seite oder die Seite konsolenstart automatisch festlegen. Diese Eigenschaften, Methoden und Ereignisse werden verwendet, um die Webseite Verhalten, Aussehen und Verhalten usw. zu definieren. Zum Schreiben von Servercode, um die Logik für die Seite zu behandeln, können Sie eine .NET Sprache wie Visual Basic oder c# verwenden.

> [!NOTE] 
> 
> ASP.NET und Visual Studio-Dokumentation umfasst mehrere Versionen. Themen, in denen Funktionen aus früheren Versionen markieren möglicherweise für Ihre aktuelle Aufgaben und die neuesten Versionen mit Szenarien nützlich.


**ASP.NET Web Forms sind:** 

- Basierend auf Microsoft ASP.NET-Technologie, die in der Code, der auf dem Server ausgeführt dynamisch wird Webseitenausgabe an den Browser oder Client-Gerät erstellt wird.
- Alle Browser oder mobile Geräte kompatibel. Eine ASP.NET-Webseite rendert automatisch das korrekte Browser kompatiblen HTML für Funktionen wie Formatvorlagen, Layout und So weiter.
- Kompatibel mit einer beliebigen Sprache, die von der .NET common Language Runtime, wie z. B. Microsoft Visual Basic und Visual c# unterstützt.
- Basiert auf Microsoft .NET Framework. Dies bietet alle Vorteile von Framework, einschließlich einer verwalteten Umgebung, typsicherheit und Vererbung.
- Flexibel, da Sie benutzerdefinierte hinzufügen können und Steuerelemente von Drittanbietern werden.

**ASP.NET Web Forms bieten zu können:** 

- Trennung von HTML und anderen UI-Code von der Anwendungslogik.
- Eine umfangreiche Sammlung von Serversteuerelementen für allgemeine Aufgaben, einschließlich Datenzugriff.
- Leistungsstarke Daten binden, mit Unterstützung für großartiges Tool.
- Unterstützung für clientseitige Skripts, die im Browser ausgeführt wird.
- Unterstützung für eine Vielzahl von anderen Funktionen, einschließlich routing, Sicherheit, Leistung, Internationalisierung, testen, Debuggen, Fehlerbehandlung und Zustandsverwaltung.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>ASP.NET WebForms können Sie die Handhabung dieser

Webanwendungsprogrammierung führte die Herausforderungen, die beim Programmieren von herkömmlichen clientbasierten Anwendungen nicht in der Regel auftreten. Hierzu zählen sind:

- **Implementieren einer umfangreichen Webbenutzeroberfläche** – kann es schwierig sein und mühsam entwerfen und implementieren ein Benutzers-Schnittstelle mit grundlegenden HTML-Funktionen, insbesondere, wenn die Seite ein komplexes Layout, eine große Menge an dynamischen Inhalt verfügt und mit umfassenden interaktiven Objekte.
- **Trennung von Client und Server** – In einer Webanwendung, der Client (Browser) und Server können Sie verschiedene Programme, die häufig ausgeführt wird, auf unterschiedlichen Computern (und sogar auf anderen Betriebssystemen). Die beiden Hälften der Anwendung freigeben daher sehr wenig Informationen; Sie können die Kommunikation jedoch in der Regel nur kleine Blöcke von einfachen Informationen austauschen.
- **Zustandslose Ausführung** : Wenn ein Webserver eine Anforderung für eine Seite, erhält er sucht nach der Seite, verarbeitet, wird an den Browser gesendet und dann verwirft alle Informationen zur Seite. Wenn der Benutzer erneut die gleiche Seite anfordert, wiederholt der Server die gesamte Sequenz, die eine erneute Verarbeitung der Seite von Grund auf neu. Anders ausgedrückt, ein Server ist kein Arbeitsspeicher für Seiten, die verarbeitet wurde – Seite zustandslos sind. Wenn eine Anwendung Informationen zu einer Seite beibehalten muss, kann seine zustandsfrei sind daher ein Problem darstellen.
- **Unbekannte Clientfunktionen** -Webanwendungen In vielen Fällen sind für viele Benutzer mit verschiedenen Browsern zugegriffen werden kann. Browser verfügen über unterschiedliche Funktionen, sodass schwer voneinander zu eine Anwendung zu erstellen, die ausgeführt werden gleichermaßen für alle davon gut.
- **Komplikationen beim Datenzugriff** -lesen und Schreiben in einer Datenquelle im herkömmlichen Webanwendungen können kompliziert und ressourcenintensiv sein.
- **Komplikationen mit Skalierbarkeit** – In vielen Fällen Webanwendungen konzipiert, die mit vorhandenen Methoden nicht Skalierbarkeitszielen aufgrund eines fehlenden Kompatibilität zwischen den verschiedenen Komponenten der Anwendung zu erfüllen. Dies ist häufig eine allgemeine Fehlerpunkt für Anwendungen unter einem Zyklus stark anwachsen.

Diese Herausforderungen für Webanwendungen kann sehr viel Zeit und Aufwand erfordern. ASP.NET Web Forms und dem ASP.NET-Framework zu dieser Herausforderung auf folgende Weise beheben:

- **Intuitive, konsistenten Objektmodell** -das ASP.NET-Seitenframework bietet ein Objektmodell, das Ihnen ermöglicht, Ihren Formularen als eine Einheit und nicht als separate Teile von Client und Server vorstellen. In diesem Modell können Sie die Seite auf eine intuitivere Weise als in herkömmlichen Web-Anwendungen, einschließlich der Möglichkeit zum Festlegen von Eigenschaften für Elemente und reagieren auf Ereignisse programmieren. Darüber hinaus sind ASP.NET-Serversteuerelemente eine Abstraktion, die von den physischen Inhalt einer HTML-Seite und der direkten Interaktion zwischen Browser und Server. Im Allgemeinen können Sie Serversteuerelemente wie verwenden Sie möglicherweise verwenden von Steuerelementen in einer Clientanwendung und keine Gedanken machen, Informationen zum Erstellen des HTML-Code vorhanden und die Steuerelemente und ihren Inhalt zu verarbeiten.
- **Ereignisgesteuertes Programmiermodell** -ASP.NET Web Forms schalten Sie das vertraute Modell zum Schreiben von Ereignishandlern für Ereignisse, die auftreten, auf dem Client oder Server für Webanwendungen. Das ASP.NET-Seitenframework abstrahiert dieses Modell auf eine Weise, dass der zugrunde liegende Mechanismus für das Erfassen eines Ereignisses auf dem Client, sie an den Server übertragen und Aufrufen der entsprechenden Methode alle automatischen und für Sie nicht sichtbar ist. Das Ergebnis ist eine klaren und einfache Weise geschriebenen Codestruktur, die eine ereignisgesteuerte Entwicklung unterstützt.
- **Intuitive Zustandsverwaltung** – das ASP.NET-Seitenframework behandelt automatisch die Aufgabe der Verwaltung des Zustands von der Seite und seine Steuerelemente, und sie versorgt Sie mit expliziten weisen den Status der anwendungsspezifische Informationen beibehalten. Dies wird erreicht, ohne starke Nutzung von Serverressourcen und kann implementiert werden, mit oder ohne Cookies an den Browser senden.
- **Browser-unabhängigen Anwendungen** -das ASP.NET-Seitenframework können Sie die gesamte Anwendungslogik auf dem Server Dadurch entfällt die Notwendigkeit für Unterschiede in den Browser explizit code zu erstellen. Allerdings ermöglicht es weiterhin Browser-spezifischen Funktionen nutzen, durch das Schreiben von clientseitigem Code, um verbesserte Leistung und eine umfangreichere Clienterfahrung für den zu ermöglichen.
- **.NET Framework common Language Runtime-Unterstützung** -das ASP.NET-Seitenframework basiert auf .NET Framework, damit das gesamte Framework an eine ASP.NET-Anwendung verfügbar ist. Anwendungen können in einer beliebigen Sprache geschrieben werden, die kompatibel ist, die mit der Laufzeit ist. Darüber hinaus den Datenzugriff mithilfe von .NET Framework, einschließlich ADO.NET bereitgestellte Infrastruktur für den Datenzugriff vereinfacht.
- **.NET Framework skalierbare Leistung** -das ASP.NET-Seitenframework ermöglicht es Ihnen, Ihre Webanwendung von einem Computer mit einem einzelnen Prozessor einer Webfarm mit mehreren Computern ordnungsgemäß und ohne komplizierte Änderungen an der Anwendungsverzeichnis skalieren Logik.

## <a name="features-of-aspnet-web-forms"></a>Funktionen von ASP.NET Web Forms

- **Serversteuerelemente**-ASP.NET Web-Server-Steuerelemente sind Objekte in ASP.NET Web Pages, die ausgeführt werden, wenn die Seite angefordert wird und das Rendern Markup im Browser. Viele Webserversteuerelemente ähneln bekannten HTML-Elementen, z. B. Schaltflächen und Textfeldern. Andere Steuerelemente umfassen komplexeres Verhalten, z. B. ein Kalender-Steuerelemente und Steuerelemente, die Sie zum Herstellen einer Verbindung mit Datenquellen und Anzeigen von Daten verwenden können.
- **Masterseiten**-master ASP.NET-Seiten können Sie ein konsistentes Layout für die Seiten in der Anwendung zu erstellen. Eine einzelne Masterseite definiert die Aussehen und Verhalten und die gemäß dem Standardverhalten, die Sie für alle Seiten (oder eine Gruppe von Seiten) werden soll, in der Anwendung. Sie können dann einzelne Seiten erstellen, die den Inhalt enthalten, die, den Sie anzeigen möchten. Wenn Benutzer die Inhaltsseiten anfordern, führen sie mit der Masterseite Ausgaben erzeugen, die das Layout der Masterseite mit dem Inhalt der Seite Inhalt kombiniert.
- **Arbeiten mit Daten**-ASP.NET bietet viele Optionen zum Speichern, abrufen und Anzeigen von Daten. In einer ASP.NET Web Forms-Anwendung verwenden Sie datengebundene Steuerelemente zum Automatisieren der Präsentation oder Eingabe von Daten in der Webseite Benutzeroberflächenelemente, z. B. Tabellen und Textfelder und Dropdownlisten.
- **Mitgliedschaft**-ASP.NET Identity speichert Ihrer Benutzer-Anmeldeinformationen in einer Datenbank, die von der Anwendung erstellt. Wenn Ihre Benutzer anmelden, überprüft die Anwendung ihre Anmeldeinformationen durch Lesen der Datenbank. Des Projekts *Konto* Ordner enthält die Dateien, die die verschiedenen Teilen von Mitgliedschaft zu implementieren: registrieren, anmelden, das Ändern des Kennworts und Autorisieren des Zugriffs. Darüber hinaus unterstützt die ASP.NET Web Forms OAuth- und OpenID. Diese Verbesserungen Authentifizierung Benutzern gestatten, in Ihre vorhandenen Anmeldeinformationen, derartige Konten wie Facebook, Twitter, Windows Live und Google-Website anzumelden. Standardmäßig erstellt die Vorlage eine Mitgliedschaftsdatenbank mithilfe eines standarddatenbanknamens in einer Instanz von SQL Server Express LocalDB, dem Development-Datenbankserver, der mit Visual Studio Express 2013 für Web enthalten ist.
- **Clientskripts und Client-Frameworks**– Sie können die serverbasierten Funktionen von ASP.NET erhöhen, indem einschließlich Clientskripts Funktionen in ASP.NET Web Forms-Seiten. Sie können Clientskript verwenden, um eine umfangreichere, Reaktionsgeschwindigkeit Benutzeroberfläche für Benutzer bereitzustellen. Sie können auch Clientskripts verwenden, asynchrone Aufrufe an den Webserver vorzunehmen, während eine Seite im Browser ausgeführt wird.
- **Routing**-URL-routing können Sie so konfigurieren Sie eine Anwendung akzeptieren Anforderungs-URLs, die keinen physischen Dateien zugeordnet sind. Eine Anforderungs-URL ist einfach die URL, die ein Benutzer in ihrem Browser zum Suchen einer Seite auf Ihrer Website eingibt. Mithilfe von routing um URLs zu definieren, die für Benutzer semantisch sinnvoll sind und zur Entwicklung mit Search Engine Optimization (SEO) beitragen können.
- **Zustandsverwaltung**-ASP.NET Web Forms enthält mehrere Optionen, mit denen Sie Daten auf einer Seite-Basis und einer Basis anwendungsweite beibehalten.
- **Sicherheit**-ein wichtiger Teil beim Entwickeln einer sichereren Anwendung die Gefahren zu verstehen ist. Microsoft hat eine Möglichkeit zum Kategorisieren von Bedrohungen entwickelt: Spoofing, Manipulation, Nichtanerkennung, Veröffentlichung von Informationen, Denial of Service, Ausweitung von Berechtigungen (STRIDE). In ASP.NET Web Forms können Sie hinzufügen, Erweiterungspunkte und Konfigurationsoptionen, mit die Sie verschiedene Sicherheitsverhalten in ASP.NET Web Forms anpassen können.
- **Leistung**-Leistung kann ein entscheidender Faktor in einer erfolgreichen Website oder eines Projekts sein. ASP.NET Web Forms können Sie die Leistung im Zusammenhang mit der Seite und Steuerelement Verarbeitung, Zustandsverwaltung, Datenzugriff, Anwendungskonfiguration und laden und effiziente Codierungsmethoden ändern.
- **Internationalisierung**– ASP.NET Web Forms ermöglicht Ihnen die Erstellung von Webseiten, die erhalten Inhalt und andere Daten basierend auf der bevorzugten Sprache-Einstellung für den Browser oder basierend auf explizite Auswahl der Sprache des Benutzers. Inhalte und andere Daten werden als Ressourcen bezeichnet und können in Ressourcendateien oder anderen Datenquellen gespeichert werden. In einer ASP.NET Web Forms-Seite konfigurieren Sie Steuerelemente zum Abrufen von Eigenschaftswerten von Ressourcen. Zur Laufzeit werden die Ressourcenausdrücke durch Ressourcen aus der entsprechenden lokalisierten Ressourcendatei ersetzt.
- **Debuggen und Fehlerbehandlung**-ASP.NET umfasst Funktionen, mit denen Sie die diagnose von Problemen, die in der Web Forms-Anwendung auftreten können. Debuggen und Fehlerbehandlung sind auch in ASP.NET Web Forms unterstützt, damit Ihre Anwendungen zu kompilieren und effektiv ausführen.
- **Bereitstellung und Hosting**-Visual Studio, ASP.NET Azure und IIS bieten Tools, mit denen Sie diverse den Prozess der Bereitstellung und Ihre Web Forms-Anwendung hostet.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Gründe für eine Web Forms-Anwendung erstellen

Sie müssen sorgfältig abwägen, ob auf einer Webanwendung implementieren, indem Sie entweder die ASP.NET-Web Forms-Modell oder ein anderes Modell, z. B. das ASP.NET MVC-Framework. Das MVC-Framework ist kein Ersatz für das Web Forms-Modell. Sie können jedes der beiden Frameworks für Webanwendungen verwenden. Bevor Sie das Web Forms-Modell oder das MVC-Framework für eine bestimmte Website verwenden möchten, Wägen Sie die Vorteile beider Ansätze gegeneinander ab.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vorteile einer Web Forms-basierten Webanwendung

Das Web Forms-basierte Framework bietet die folgenden Vorteile:

- Es unterstützt ein Ereignismodell, das Zustand über HTTP beibehält, die Vorteile Line-of-Business-Web-Anwendungsentwicklung. Die Web Forms-basierte Anwendung stellt Dutzende von Ereignissen, die in Hunderten von Serversteuerelementen unterstützt werden.
- Es verwendet ein Seitencontroller-Schema, die Funktionalität für einzelne Seiten hinzugefügt. Weitere Informationen finden Sie unter [Seitencontroller](https://go.microsoft.com/fwlink/?LinkId=106359 "Seitencontroller") auf der MSDN-Website.
- Er verwendet Ansichtszustand oder und severbasierten Formulare, die Verwaltung von Zustandsinformationen vereinfachen können.
- Dies funktioniert gut für kleine Webentwicklerteams Webentwickler und Designer, die auf der großen Anzahl von Komponenten, die für eine schnelle Anwendungsentwicklung nutzen möchten.
- Im Allgemeinen ist es weniger komplex ist, für die Anwendungsentwicklung, da die Komponenten (die **Seite** -Klasse, Steuerelemente usw.) stark integriert sind und normalerweise weniger Code als das MVC-Modell erfordern.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vorteile einer MVC-basierten Webanwendung

ASP.NET MVC-Framework bietet die folgenden Vorteile:

- Dies erleichtert die Komplexität zu verwalten, indem eine Anwendung in das Modell, Ansicht und Controller unterteilen.
- Es verwendet keine Ansichtszustand oder severbasierten Formularen. Dadurch wird das MVC-Framework für Entwickler, die vollständige Kontrolle über das Verhalten einer Anwendung soll ideal.
- Er verwendet ein Frontcontroller-Schema, das webanwendungsanforderungen durch einen einzelnen Controller verarbeitet. Dadurch können Sie eine Anwendung entwerfen, die eine umfangreiche Routinginfrastruktur unterstützen. Weitere Informationen finden Sie unter [Frontcontroller](https://go.microsoft.com/fwlink/?LinkId=106357 "Frontcontroller") auf der MSDN-Website.
- Es bietet eine bessere Unterstützung für testgesteuerte Entwicklung (TDD).
- Dies funktioniert gut für Webanwendungen, die von großen Entwicklerteams von Entwicklern und Webdesignern, die benötigen ein hohes Maß an Kontrolle über das Verhalten der Anwendung, unterstützt werden.
