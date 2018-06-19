---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: Übersicht über ASP.NET MVC | Microsoft Docs
author: microsoft
description: Weitere Informationen Sie zu den Unterschieden zwischen ASP.NET MVC-Anwendung und ASP.NET Web Forms-Anwendungen. Informationen Sie zum Erstellen einer ASP.NET MVC-Anwendung überlassen.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: f44e6fb1e19d3c2384ebaeeca0ddea8239dd5a3c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26500869"
---
<a name="aspnet-mvc-overview"></a>Übersicht über ASP.NET MVC
====================
durch [Microsoft](https://github.com/microsoft)

> Weitere Informationen Sie zu den Unterschieden zwischen ASP.NET MVC-Anwendung und ASP.NET Web Forms-Anwendungen. Informationen Sie zum Erstellen einer ASP.NET MVC-Anwendung überlassen.


Das Architekturschema Model-View-Controller (MVC) trennt eine Anwendung in drei Hauptkomponenten: das Modell, Ansicht und Controller. ASP.NET MVC-Framework bietet eine Alternative zu ASP.NET Web Forms-Muster für das MVC-basierten Webanwendungen erstellen. ASP.NET MVC-Framework ist eine kompakte, mitgliedschaftsbasierte zu testendes Präsentationsframework, (wie bei Web Forms-basierte Anwendungen) ist in vorhandene ASP.NET-Funktionen, z. B. Gestaltungsvorlagen und Mitgliedschaft basierende Authentifizierung integriert. Das MVC-Framework wird definiert, der **System.Web.Mvc** Namespace und ein grundlegender, unterstützter Teil der **System.Web** Namespace.   
  
MVC ist ein Standardentwurfsschema, die viele Entwickler mit vertraut sind. Einige Arten von Webanwendungen profitieren vom MVC-Framework. Andere werden weiterhin das herkömmliche ASP.NET-Anwendungsschema verwenden, das auf Web Forms und Postbacks basiert. Andere Arten von Webanwendungen werden beide Ansätze kombinieren. keiner dieser Ansätze, die den anderen ausschließt.   
  
Das MVC-Framework umfasst die folgenden Komponenten:


[![Eine Controlleraktion aufrufen, die einen Parameterwert erwartet](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Abbildung 01**: Aufrufen einer Controlleraktion, die einen Parameterwert erwartet ([klicken Sie hier, um das Bild in voller Größe angezeigt](asp-net-mvc-overview/_static/image2.png))


- **Modelle**. Modellobjekte sind die Teile der Anwendung, die Logik für die Anwendungsdomäne des s Daten implementieren. Häufig Model-Objekte abzurufen und Modellzustand in einer Datenbank speichern. Beispielsweise kann ein Produktobjekt Abrufen von Informationen aus einer Datenbank verarbeiten und anschließend aktualisierte Informationen zurück in eine Produkttabelle in SQL Server schreiben.

In kleinen Anwendungen ist das Modell häufig eine konzeptionelle Trennung statt physisch von den anderen. Z. B., wenn die Anwendung nur ein Dataset liest und sie an die Sicht sendet, besitzt die Anwendung nicht physische Modellebene und die zugehörigen Klassen. In diesem Fall übernimmt das Dataset die Rolle eines Modellobjekts.

- **Sichten**. Ansichten sind die Komponenten, die die Anwendung s-Benutzeroberfläche (UI) anzuzeigen. In der Regel wird diese Benutzeroberfläche aus den Modelldaten erstellt. Ein Beispiel wäre eine Bearbeitungsansicht einer Produkttabelle, in der Textfelder, Dropdownlisten und Kontrollkästchen, die basierend auf den aktuellen Status eines Objekts Produkte angezeigt.

- **Controller**. Controller sind die Komponenten behandeln Benutzerinteraktionen, mit dem Modell arbeiten und letztlich wählen Sie eine Ansicht zu rendern, in der Benutzeroberfläche angezeigt. In einer MVC-Anwendung zeigt die Ansicht nur Informationen; der Controller behandelt und antwortet auf Benutzereingaben und Interaktion. Beispielsweise wird der Controller behandelt Abfragezeichenfolgen-Werte und übergibt diese Werte an das Modell, das wiederum die Datenbank Abfragen unter Verwendung der Werte.

Das MVC-Muster können Sie Anwendungen erstellen, die die verschiedenen Aspekte der Anwendung (Eingabelogik, Geschäftslogik und Benutzeroberflächen-Logik), und gleichzeitig eine lose Kopplung zwischen diesen Elementen liegen. Das Muster angibt, in dem jede Art von Logik in der Anwendung gespeichert werden. Die Benutzeroberflächenlogik ist Teil der Ansicht (view). Die Eingabelogik gehört zum Controller. Die Geschäftslogik gehört zum Modell. Aufgrund dieser Trennung können Sie die Komplexität bei der Erstellung einer Anwendung verwalten, da Sie sich auf einen Aspekt der Implementierung zu einem Zeitpunkt konzentrieren können. Beispielsweise können Sie die Ansicht abhängig von der Geschäftslogik konzentrieren.   
  
Zusätzlich zur Verwaltung der Komplexität erleichtert das MVC-Schema zum Testen von Anwendungen als zu eine Web Forms-basierten ASP.NET-Webanwendung zu testen. In einer Web Forms-basierten ASP.NET-Webanwendung wird z. B. eine einzelne Klasse sowohl Ausgabe anzeigen und reagieren auf Benutzereingaben verwendet. Das Schreiben automatisierter Tests für Web Forms-basierte ASP.NET-Anwendungen kann komplex sein, da zum Testen einer einzelnen Seite die Seitenklasse, alle seine untergeordneten Steuerelemente und zusätzliche abhängige Klassen in der Anwendung instanziiert werden müssen. Da so viele Klassen instanziiert werden, um die Seite auszuführen, kann es schwierig werden Tests schreiben, die ausschließlich auf einzelne Teile der Anwendung zu konzentrieren. Tests für Web Forms-basierte ASP.NET-Anwendungen können daher schwieriger zu implementieren als Tests in einer MVC-Anwendung sein. Darüber hinaus ist die Tests in einer Web Forms-basierten ASP.NET-Anwendung ein Webserver erforderlich. Das MVC-Framework entkoppelt die Komponenten und nutzt intensiv Schnittstellen, wodurch es möglich, einzelne Komponenten isoliert vom Rest des Frameworks zu testen.   
  
Der lose Kopplung zwischen den drei Hauptkomponenten einer MVC-Anwendung begünstigt auch die parallele Entwicklung. Z. B. ein Entwickler kann für die Sicht arbeiten, ein zweiter Entwickler an der Controllerlogik arbeiten kann und ein dritter Entwickler auf die Geschäftslogik im Modell konzentrieren kann.

## <a name="deciding-when-to-create-an-mvc-application"></a>Gründe für das Erstellen einer MVC-Anwendung

Sie müssen sorgfältig überlegen, ob eine Webanwendung mithilfe von ASP.NET MVC-Framework oder der ASP.NET Web Forms-Modells implementiert. Das MVC-Framework ist kein Ersatz für das Web Forms-Modell. Sie können jedes der beiden Frameworks für Webanwendungen verwenden. (Wenn Sie vorhandene Webformulare basierende Anwendungen haben, diese funktionieren weiterhin genau wie entfallen.)   
  
Bevor Sie das MVC-Framework oder das Web Forms-Modell für eine bestimmte Website verwenden möchten, Wägen Sie die Vorteile beider Ansätze gegeneinander ab.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vorteile einer MVC-basierten Webanwendung

ASP.NET MVC-Framework bietet die folgenden Vorteile:

- Dies erleichtert die Komplexität zu verwalten, indem eine Anwendung in das Modell, Ansicht und Controller unterteilen.
- Es verwendet keine Ansichtszustand oder severbasierten Formularen. Dadurch wird das MVC-Framework für Entwickler, die vollständige Kontrolle über das Verhalten einer Anwendung soll ideal.
- Er verwendet ein Frontcontroller-Schema, das webanwendungsanforderungen durch einen einzelnen Controller verarbeitet. Dadurch können Sie eine Anwendung entwerfen, die eine umfangreiche Routinginfrastruktur unterstützen. Weitere Informationen finden Sie unter [Frontcontroller](https://go.microsoft.com/fwlink/?LinkId=106357 "Frontcontroller") auf der MSDN-Website.
- Es bietet eine bessere Unterstützung für testgesteuerte Entwicklung (TDD).
- Dies funktioniert gut für Webanwendungen, die von großen Entwicklerteams von Entwicklern und Webdesignern, die benötigen ein hohes Maß an Kontrolle über das Verhalten der Anwendung, unterstützt werden.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vorteile einer Web Forms-basierten Webanwendung

Das Web Forms-basierte Framework bietet die folgenden Vorteile:

- Es unterstützt ein Ereignismodell, das Zustand über HTTP beibehält, die Vorteile Line-of-Business-Web-Anwendungsentwicklung. Die Web Forms-basierte Anwendung stellt Dutzende von Ereignissen, die in Hunderten von Serversteuerelementen unterstützt werden.
- Es verwendet ein Seitencontroller-Schema, die Funktionalität für einzelne Seiten hinzugefügt. Weitere Informationen finden Sie unter [Seitencontroller](https://go.microsoft.com/fwlink/?LinkId=106359 "Seitencontroller") auf der MSDN-Website.
- Er verwendet Ansichtszustand oder und severbasierten Formulare, die Verwaltung von Zustandsinformationen vereinfachen können.
- Dies funktioniert gut für kleine Webentwicklerteams Webentwickler und Designer, die auf der großen Anzahl von Komponenten, die für eine schnelle Anwendungsentwicklung nutzen möchten.
- Im Allgemeinen ist es weniger komplex ist, für die Anwendungsentwicklung, da die Komponenten (die **Seite** -Klasse, Steuerelemente usw.) stark integriert sind und normalerweise weniger Code als das MVC-Modell erfordern.

## <a name="features-of-the-aspnet-mvc-framework"></a>Funktionen von ASP.NET MVC-Framework

ASP.NET MVC-Framework bietet die folgenden Funktionen:

- Trennung von Anwendungsaufgaben, (Eingabelogik, Geschäftslogik und Benutzeroberflächen-Logik) Prüfbarkeit und Test-driven Development (TDD) in der Standardeinstellung. Alle Kernverträge im MVC-Framework basieren auf Schnittstelle und können mit Pseudoobjekten simulierte Objekte, die das Verhalten der tatsächlichen Objekte in der Anwendung imitieren getestet werden. Sie können Komponententests die Anwendung ohne die Controller in einem ASP.NET-Prozess ausführen, wodurch Komponententests schnell und flexibel. Sie können alle Komponententestframework verwenden, die mit .NET Framework kompatibel ist.
- Ein erweiterbares und austauschbares Framework. Die Komponenten des ASP.NET MVC-Frameworks sind so konzipiert, dass sie leicht ausgetauscht oder angepasst werden können. Sie können Ihr eigenes Ansichtsmodul, URL-Routingrichtlinien Aktion Serialisierung für Aktionsmethodenparameter und andere Komponenten einbinden. ASP.NET MVC-Framework unterstützt auch die Verwendung von steuerungsumkehrung bei containermodellen (Dependency Injection, DI) und die Umkehrung des Steuerelements (IOC). DI können Sie Objekte in einer Klasse, anstatt auf die Klasse zum Erstellen des Objekts selbst einzufügen. IOC gibt an, dass das erste Objekt, wenn ein Objekt eines anderen Objekts erforderlich ist, das zweite Objekt aus einer externen Quelle, z. B. einer Konfigurationsdatei abrufen soll. Dadurch wird das Testen vereinfachen.
- Eine leistungsstarke URL-Zuordnung-Komponente, mit dem Sie Anwendungen erstellen, die verständlichen und durchsuchbaren URLs haben. URLs müssen keine Dateinamenerweiterungen einschließen und dienen zur Unterstützung von URL-Benennungsschemas, die funktionieren auch für die Suche engine Optimization (SEO) und representational State Transfer (REST) adressiert.
- Unterstützung für die Verwendung von Markup aus vorhandenen ASP.NET-Seite (ASPX-Dateien), das Benutzersteuerelement (ASCX-Dateien) und master Seite (Master-Dateien) als Ansichtsvorlagen. Sie können vorhandene ASP.NET-Funktionen mit ASP.NET MVC-Frameworks, z. B. verschachtelte Gestaltungsvorlagen, Inlineausdrücke (&lt;% = %&gt;), deklarative Serversteuerelemente, Vorlagen, Datenbindung, Lokalisierung und So weiter.
- Unterstützung für vorhandene ASP.NET-Funktionen. ASP.NET MVC können Sie Funktionen, z. B. Formularauthentifizierung und Windows-Authentifizierung, URL-Autorisierung, Mitgliedschaft und Rollen, Ausgabe und Zwischenspeichern von Daten, Session und profilzustandsverwaltung, Systemüberwachung, das Konfigurationssystem und den Anbieter zu verwenden Architektur.
