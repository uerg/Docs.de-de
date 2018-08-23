---
uid: web-forms/what-is-web-forms
title: Was ist, dass Web Forms | Microsoft-Dokumentation
author: rick-anderson
description: ''
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: ccb0e6096b0281e5ef8af63bfd84b172e7f209b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839011"
---
<a name="what-is-web-forms"></a>Was ist Web Forms
====================
ASP.NET Web Forms ist ein Bestandteil von ASP.NET Web Application Framework und ist im Lieferumfang [Visual Studio](https://www.asp.net/downloads). Es ist eines der vier Programmiermodellen, die Sie verwenden können, um ASP.NET-Webanwendungen zu erstellen, die anderen sind ASP.NET MVC, ASP.NET Web Pages und einseitigen ASP.NET-Anwendungen.

Web Forms sind Seiten, die Ihre Benutzer anfordern, über einen Browser. Diese Seiten können mit einer Kombination aus HTML-Code geschrieben werden Clientskript-Steuerelemente und Servercode. Wenn Benutzer eine Seite anfordern, wird sie kompiliert, die durch das Framework auf dem Server ausgeführt, und klicken Sie dann das Framework generiert das HTML-Markup, das im Browser rendern kann. Eine ASP.NET Web Forms-Seite enthält Informationen für den Benutzer in einem beliebigen Browser oder Client-Gerät.

ASP.NET Web Forms können Sie mithilfe von Visual Studio erstellen. Die Visual Studio integrierte Entwicklungsumgebung (IDE) können Sie die Drag & drop-Steuerelemente zum Anordnen der Web Forms-Seite. Sie können dann problemlos Eigenschaften, Methoden und Ereignisse für Steuerelemente auf der Seite oder für die Seite Dateiprotokoll festlegen. Diese Eigenschaften, Methoden und Ereignisse werden verwendet, um die Webseite Verhalten, Aussehen und Verhalten usw. zu definieren. Zum Schreiben von Servercode, um die Logik für die Seite zu verarbeiten, können Sie eine .NET-Sprache verwendet wie Visual Basic oder c# verwenden.

> [!NOTE] 
> 
> Dokumentation zu ASP.NET und Visual Studio umfasst mehrere Versionen. Themen, in denen Funktionen aus früheren Versionen zu markieren, können für Ihre aktuelle Vorgänge und Szenarien, die die neuesten Versionen nützlich sein.


**ASP.NET Web Forms sind:** 

- Basierend auf Microsoft ASP.NET-Technologie, Code, auf dem Server dynamisch ausgeführt wird, in der Ausgabe einer Webseite an den Browser oder Client-Gerät erstellt wird.
- Mit jedem Browser oder mobile Geräte kompatibel. Eine ASP.NET-Webseite rendert automatisch die richtige Browser kompatibel HTML für Features wie z. B. Stile, Layout und So weiter.
- Kompatibel mit einer beliebigen Sprache, die von der .NET common Language Runtime, z. B. Microsoft Visual Basic und Microsoft Visual c# unterstützt.
- Basiert auf Microsoft .NET Framework. Dies bietet alle Vorteile von Framework, einschließlich einer verwalteten Umgebung, typsicherheit und Vererbung.
- Flexibel, da Sie die Benutzer erstellte hinzufügen können und von Drittanbietern werden Steuerelemente.

**ASP.NET Web Forms bieten zu können:** 

- Trennung von HTML und anderen UI-Code von der Anwendungslogik.
- Eine umfassende Suite von Serversteuerelementen für allgemeine Aufgaben, einschließlich der Datenzugriff.
- Leistungsstarke Daten, die Bindung, mit der hervorragenden toolunterstützung.
- Unterstützung für clientseitige Skripts, die im Browser ausgeführt wird.
- Unterstützung für eine Vielzahl von anderen Funktionen, einschließlich routing, Sicherheit, Leistung, Internationalisierung, testen, Debuggen, Fehlerbehandlung und Zustandsverwaltung.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>ASP.NET Web Forms können Sie die Herausforderungen meistern

Programmierung von Webanwendungen Herausforderungen, die bei der Programmierung von herkömmlichen clientbasierten Anwendungen nicht in der Regel auftreten. Sind zwei der Herausforderungen:

- **Implementieren eine umfangreiche Webbenutzeroberfläche** : Es kann schwierig sein und mühsam, Entwerfen und implementieren ein Benutzers Schnittstelle. dabei wird grundlegende HTML-Funktionen, insbesondere dann, wenn die Seite ein komplexes Layout, eine große Menge an dynamische Inhalte verfügt, voll funktionsfähige interaktiven Objekte.
- **Trennung von Client und Server** – In einer Webanwendung, den Client (Browser) und Server werden verschiedene Programme, die häufig ausgeführt wird, auf verschiedenen Computern (und sogar unter verschiedenen Betriebssystemen). Die beiden Hälften der Anwendung wird daher nur sehr wenig Informationen austauschen; Sie können kommunizieren, aber Sie können exchange in der Regel nur kleine Blöcke einfacher Informationen.
- **Zustandslose Ausführung** : Wenn Sie ein Webserver eine Anforderung für eine Seite, empfängt die Seite gefunden wird, verarbeitet, sendet es an den Browser und dann verwirft alle Informationen zur Seite. Wenn der Benutzer erneut die gleiche Seite anfordert, wird der Server die gesamte Sequenz, die eine erneute Verarbeitung der Seite von Grund auf neu wiederholt. Anders ausgedrückt, ein Server keinen Speicher von Seiten, die es verarbeitet hat hat – Seite sind zustandslos. Wenn eine Anwendung Informationen zu einer Seite beibehalten muss, kann die zustandslosigkeit aus diesem Grund problematisch werden.
- **Unbekannter Clientfunktionen** -Webanwendungen In vielen Fällen sind für viele Benutzer mit unterschiedlichen Browsern zugegriffen werden kann. Browser haben verschiedene Funktionen, sodass es schwierig wird zum Erstellen einer Anwendung, die ausgeführt werden für alle gleichermaßen gut.
- **Komplikationen bei Datenzugriff** -lesen und Schreiben in eine Datenquelle in herkömmlichen Webanwendungen können kompliziert und ressourcenintensiv sein.
- **Komplikationen bei der Skalierbarkeit** – In vielen Fällen nicht Webanwendungen, die speziell für die vorhandenen Methoden skalierungsziele aufgrund des Mangels an Kompatibilität zwischen den verschiedenen Komponenten der Anwendung zu erfüllen. Dies ist oft einen gemeinsamen Fehlerpunkt für Anwendungen unter einem Zyklus hoher Zuwachs.

Erfüllt diese Herausforderungen für Webanwendungen kann sehr viel Zeit und Aufwand erfordern. ASP.NET Web Forms und ASP.NET-Framework Herausforderungen diese auf folgende Weise:

- **Intuitive und konsistentes Objektmodell** -das ASP.NET-Seitenframework stellt ein Objektmodell, das Ihnen ermöglicht, Ihre Formulare als eine Einheit und nicht als separate Teile von Client und Server vorstellen. In diesem Modell können Sie die Seite in einer intuitiver Weise als in herkömmlichen Webanwendungen, einschließlich der Möglichkeit, legen Sie Eigenschaften für Elemente und reagieren auf Ereignisse programmieren. Darüber hinaus sind die ASP.NET-Serversteuerelemente eine Abstraktion von physischen Inhalt einer HTML-Seite und der direkten Interaktion zwischen Browser und Server. Im Allgemeinen können Steuerelemente die Möglichkeit Sie, die Sie möglicherweise verwenden von Steuerelementen in einer Clientanwendung und nicht kümmern, wie Sie den HTML-Code vorhanden und verarbeiten die Steuerelemente und deren Inhalt erstellen.
- **Ereignisgesteuertes Programmiermodell** -ASP.NET Web Forms nutzen Sie das vertraute Modell das Schreiben von Ereignishandlern für Ereignisse, die auftreten, auf dem Client oder Server für Web-Anwendungen. Das ASP.NET-Seitenframework abstrahiert dieses Modell so, dass der zugrunde liegende Mechanismus für das Erfassen eines Ereignisses auf dem Client, übertragen es an den Server und Aufrufen der entsprechenden Methode alle automatisch und unsichtbar bleibt, ist. Das Ergebnis ist eine klare und einfache Weise geschriebenen Codestruktur, die das ereignisgesteuerte-Entwicklung unterstützt.
- **Intuitive Zustandsverwaltung** – das ASP.NET-Seitenframework behandelt automatisch die Aufgabe der Verwaltung des Ihrer Seite und ihre Steuerelemente und bietet Ihnen eine explizite Möglichkeiten, den Status einer anwendungsspezifischen Informationen beibehalten. Dies wird erreicht, ohne die intensiven Gebrauch von Serverressourcen und kann implementiert werden, mit oder ohne Cookies an den Browser senden.
- **Browser-unabhängigen Anwendungen** -Framework für die ASP.NET-Seiten können Sie die gesamte Anwendungslogik auf dem Server, und Sie müssen explizit für die Unterschiede in Browsern code zu erstellen. Allerdings können immer noch Sie nutzen Browser-spezifischen Funktionen durch clientseitigen Code zum verbesserten Leistung und ein umfangreicheres Clientbenutzerszenario bereitstellen.
- **.NET Framework common Language Runtime-Unterstützung** -das ASP.NET-Seitenframework basiert auf .NET Framework, damit das gesamte Framework für ASP.NET-Anwendungen verfügbar ist. Ihre Anwendungen können in einer beliebigen Sprache geschrieben werden, die kompatibel ist, die mit der Laufzeit ist. Darüber hinaus wird der Datenzugriff vereinfacht mithilfe von .NET Framework einschließlich ADO.NET bereitgestellte Infrastruktur für den Datenzugriff.
- **.NET Framework skalierbare Leistung** -das ASP.NET-Seitenframework ermöglicht Ihnen, Ihre Webanwendung von einem Computer mit einem einzigen Prozessor einer Webfarm mit mehreren Computern ordnungsgemäß und ohne komplizierte Änderungen an der Anwendung zu skalieren die Logik.

## <a name="features-of-aspnet-web-forms"></a>Funktionen von ASP.NET Web Forms

- **Serversteuerelemente**-ASP.NET Webserver-Steuerelemente sind Objekte in ASP.NET Web Pages, die ausgeführt werden, wenn die Seite angefordert wird und der, Rendern Markup im Browser. Viele Webserver-Steuerelemente ähneln vertraut HTML-Elementen, z. B. Schaltflächen und Textfelder. Andere Steuerelemente umfassen komplexes Verhalten, z. B. Kalender-Steuerelemente und Steuerelemente, die Sie verwenden können, um eine Verbindung mit Datenquellen herstellen und Daten anzuzeigen.
- **Masterseiten**-ASP.NET-Masterseiten können Sie ein konsistentes Layout für die Seiten in Ihrer Anwendung zu erstellen. Eine einzige masterdseite definiert das Aussehen und Verhalten und die gemäß dem Standardverhalten, das Sie für alle Seiten (oder eine Gruppe von Seiten) verwenden möchten, in Ihrer Anwendung. Sie können dann einzelne Seiten erstellen, die den Inhalt enthalten, die, den Sie anzeigen möchten. Wenn Benutzer Inhaltsseiten anfordern, führen sie mit der Masterseite Ausgabe erzeugt, die das Layout der Masterseite mit dem Inhalt der Inhaltsseite kombiniert.
- **Arbeiten mit Daten**-ASP.NET bietet viele Optionen zum Speichern, abrufen und Anzeigen von Daten. In einer ASP.NET Web Forms-Anwendung verwenden Sie datengebundene Steuerelemente, um die Präsentation oder Eingabe von Daten in die Webseite UI-Elemente wie z. B. Tabellen, Textfelder und Dropdownlisten zu automatisieren.
- **Mitgliedschaft**-ASP.NET Identity speichert die Benutzeranmeldeinformationen in einer Datenbank, die von der Anwendung erstellt. Wenn Ihre Benutzer anmelden, überprüft die Anwendung ihrer Anmeldeinformationen durch Lesen aus der Datenbank an. Ihres Projekts *Konto* Ordner enthält die Dateien, die die verschiedenen Teile der Mitgliedschaft zu implementieren: Registrierung, Anmeldung, das Ändern des Kennworts und Autorisieren des Zugriffs. Darüber hinaus unterstützt ASP.NET Web Forms, OAuth und OpenID. Diese Verbesserungen der Authentifizierung können Benutzer bei einer Website mithilfe von vorhandenen Anmeldeinformationen aus solchen Facebook, Twitter, Windows Live und Google-Konten anmelden. Standardmäßig erstellt die Vorlage eine Mitgliedschaftsdatenbank mithilfe eines standarddatenbanknamens auf einer Instanz von SQL Server Express LocalDB, den Datenbankserver für Entwicklung, die in Visual Studio Express 2013 für Web enthalten ist.
- **Clientskripts und Clientframeworks**– Sie können die serverbasierten Funktionen von ASP.NET durch Clientskript-Funktionen in ASP.NET Web Form-Seiten verbessern. Sie können Clientskript verwenden, eine vielfältiger, reaktionsfähiger Benutzeroberfläche für Benutzer bereitstellen. Sie können auch Clientskript verwenden, um asynchrone Aufrufe an den Webserver während eine Seite im Browser ausgeführt wird.
- **Routing**-URL-routing, können Sie zum Konfigurieren einer Anwendung zum Akzeptieren von Anforderungs-URLs, die nicht auf physischen Dateien zugeordnet sind. Eine Anforderungs-URL ist einfach die URL, die ein Benutzer in ihrem Browser eine Seite auf Ihrer Website sucht eingibt. Sie verwenden routing, um die URLs zu definieren, die semantisch für Benutzer von Bedeutung sind, und suchen-suchmaschinenoptimierung (SEO) unterstützen kann.
- **Zustandsverwaltung**-ASP.NET Web Forms enthält mehrere Optionen, mit denen Sie Daten auf einer Basis pro Seite und einer gesamten Anwendung-Basis zu erhalten.
- **Sicherheit**– ein wichtiger Bestandteil der Entwicklung einer Anwendung sichereren wird, die Bedrohungen, zu verstehen. Microsoft hat eine Möglichkeit zum Kategorisieren von Bedrohungen entwickelt: Spoofing, Verfälschungen, Nichtanerkennung, Offenlegung von Informationen, Denial of Service, Rechteerweiterungen (STRIDE). In ASP.NET Web Forms können Sie hinzufügen, Erweiterungspunkte und Konfigurationsoptionen, mit die Sie verschiedene Sicherheitsverhalten in ASP.NET Web Forms anpassen können.
- **Leistung**-Leistung kann ein entscheidender Faktor sein, in einem erfolgreichen-Website oder das Projekt. ASP.NET Web Forms können Sie Leistung, bezieht sich auf der Seite und serververarbeitung-Steuerelement, Zustandsverwaltung, Datenzugriff, Konfiguration und Anwendung laden und effizienten Methoden Schreiben von Code zu ändern.
- **Internationalisierung**: ASP.NET Web Forms ermöglicht Ihnen die Erstellung von Webseiten, die abrufen können, Inhalt und andere Daten basierend auf der bevorzugten Sprache-Einstellung für den Browser oder explizite Auswahl der Sprache des Benutzers. Inhalte und andere Daten werden als Ressourcen bezeichnet, und diese Daten können in Ressourcendateien oder anderen Quellen gespeichert werden. In einer ASP.NET Web Forms-Seite konfigurieren Sie die Steuerelemente zum Abrufen von Eigenschaftswerten von Ressourcen. Zur Laufzeit werden die Ressourcenausdrücke von Ressourcen aus der entsprechenden lokalisierten Ressourcendatei ersetzt.
- **Debuggen und Fehlerbehandlung**-ASP.NET enthält Funktionen, mit denen Sie die diagnose von Problemen, die in der Web Forms-Anwendung auftreten können. Debuggen und zur Fehlerbehandlung werden auch in ASP.NET Web Forms unterstützt, so, dass Ihre Anwendungen zu kompilieren und effektiv ausgeführt.
- **Bereitstellung und Hosting**– Visual Studio, Azure, ASP.NET und IIS bieten Tools, mit denen Sie mit dem Prozess der Bereitstellung und zum Hosten der Web Forms-Anwendung.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Entscheidungshilfen für die Erstellung eine Web Forms-Anwendung

Sie müssen sorgfältig überlegen, ob auf einer Web-Anwendung implementieren, indem Sie entweder die ASP.NET-Web Forms-Modell oder ein anderes Modell, z. B. ASP.NET MVC-Framework. Das MVC-Framework ist kein Ersatz für das Web Forms-Modell. Sie können jedes der beiden Frameworks für Webanwendungen verwenden. Bevor Sie die Web Forms-Modell oder das MVC-Framework für eine bestimmte Website verwenden möchten, wiegen Sie die Vorteile beider Ansätze.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vorteile einer Web Forms-basierten Webanwendung

Das Web Forms-basierte Framework bietet die folgenden Vorteile:

- Es unterstützt ein Ereignismodell, der Zustand über HTTP beibehält, die Entwicklung von LOB-Webanwendungen profitieren. Die Web Forms-basierte Anwendung stellt Dutzende von Ereignissen, die in Hunderten von Serversteuerelementen unterstützt werden.
- Er verwendet ein Seitencontroller-Muster, das Funktionen für einzelne Seiten hinzugefügt. Weitere Informationen finden Sie unter [Seitencontroller](https://go.microsoft.com/fwlink/?LinkId=106359 "Seitencontroller") auf der MSDN-Website.
- Er verwendet Ansichtszustand oder severbasierten Formularen, die Verwaltung von Zustandsinformationen vereinfachen können.
- Es eignet sich gut für kleine Teams von Web-Entwicklern und Designern, die die große Anzahl von Komponenten, die für eine schnelle Anwendungsentwicklung nutzen möchten.
- Ist im Allgemeinen weniger komplex ist, für die Anwendungsentwicklung, da die Komponenten (die **Seite** -Klasse, Steuerelemente usw.) stark integriert sind und normalerweise weniger Code als das MVC-Modell erfordern.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vorteile einer MVC-basierten Webanwendung

ASP.NET MVC-Framework bietet die folgenden Vorteile:

- Es erleichtert die Komplexität zu verwalten, indem Sie eine Anwendung in das Modell, Ansicht und den Controller zu unterteilen.
- Es verwendet keine Ansichtszustand oder severbasierten Formularen. Dadurch wird das MVC-Framework für Entwickler, die vollständige Kontrolle über das Verhalten einer Anwendung ideal.
- Er verwendet ein Frontcontroller-Schema, das webanwendungsanforderungen durch einen einzelnen Controller verarbeitet. Dadurch können Sie eine Anwendung entwerfen, die eine umfangreiche Routinginfrastruktur unterstützen. Weitere Informationen finden Sie unter [Frontcontroller](https://go.microsoft.com/fwlink/?LinkId=106357 "Frontcontroller") auf der MSDN-Website.
- Es bietet bessere Unterstützung für testgesteuerte Entwicklung (TDD).
- Es eignet sich gut für Webanwendungen, die unterstützt werden, indem große Teams, die von Entwicklern und Web-Designer, die ein hohes Maß an Kontrolle über das Verhalten der Anwendung benötigen.
