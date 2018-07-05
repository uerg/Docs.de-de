---
uid: mvc/overview/older-versions-1/overview/asp-net-mvc-overview
title: ASP.NET MVC – Übersicht | Microsoft-Dokumentation
author: microsoft
description: Informationen Sie zu den Unterschieden zwischen ASP.NET MVC-Anwendung und ASP.NET Web Forms-Anwendungen. Erfahren Sie, wie, wann zum Erstellen einer ASP.NET MVC-Anwendung.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2dcb44a4-5cbf-4d62-b363-718104082d86
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/overview/asp-net-mvc-overview
msc.type: authoredcontent
ms.openlocfilehash: 2a83f77815c4e55ac2b9bac30dc5019b329baeac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399817"
---
<a name="aspnet-mvc-overview"></a>ASP.NET MVC – Übersicht
====================
durch [Microsoft](https://github.com/microsoft)

> Informationen Sie zu den Unterschieden zwischen ASP.NET MVC-Anwendung und ASP.NET Web Forms-Anwendungen. Erfahren Sie, wie, wann zum Erstellen einer ASP.NET MVC-Anwendung.


Das Architekturmuster Model-View-Controller (MVC) trennt eine Anwendung in drei Hauptkomponenten: das Modell, Ansicht und Controller. ASP.NET MVC-Framework bietet eine Alternative für das ASP.NET Web Forms-Muster zum Erstellen von MVC-basierte Web-Anwendungen. ASP.NET MVC-Framework ist eine einfache und leicht zu testendes Präsentationsframework, (wie bei Web Forms-basierte Anwendungen) ist in vorhandene ASP.NET-Funktionen, z. B. Gestaltungsvorlagen und mitgliedschaftsbasierte Authentifizierung integriert. Das MVC-Framework wird definiert, der **System.Web.Mvc** Namespace und ein grundlegender, unterstützter Teil der **"System.Web"** Namespace.   
  
MVC ist ein Standardentwurfsschema, die viele Entwickler kennen. Einige Arten von Webanwendungen profitieren vom MVC-Framework. Andere werden weiterhin das herkömmliche ASP.NET-Anwendungsschema verwenden, das auf Web Forms und Postbacks basiert. Andere Arten von Webanwendungen werden beide Ansätze kombinieren; keiner dieser Ansätze, die den anderen ausschließt.   
  
Das MVC-Framework umfasst die folgenden Komponenten:


[![Eine Controlleraktion aufgerufen wird, die einen Parameterwert erwartet](asp-net-mvc-overview/_static/image1.jpg)](asp-net-mvc-overview/_static/image1.png)

**Abbildung 01**: eine Controlleraktion, die einen Parameterwert erwartet aufrufen ([klicken Sie, um das Bild in voller Größe anzeigen](asp-net-mvc-overview/_static/image2.png))


- **Modelle**. Modellobjekte sind die Teile der Anwendung, die die Logik für die Anwendungsdomäne des s Daten implementieren. Häufig Modellobjekte abgerufen und in einer Datenbank gespeichert. Beispielsweise kann ein Product-Objekt abrufen von Informationen aus einer Datenbank, verarbeiten und anschließend aktualisierten Informationen zurück in eine Produkttabelle in SQL Server schreiben.

Bei kleineren Anwendungen ist das Modell häufig eine konzeptionelle Trennung statt physisch von den anderen. Z. B. wenn die Anwendung wird nur ein Dataset liest und sie an die Ansicht sendet, verfügt die Anwendung keine physische Modellebene und verknüpfter Klassen über. In diesem Fall übernimmt das Dataset die Rolle eines Modellobjekts.

- **Ansichten**. Ansichten sind die Komponenten, die die Anwendung-s-Benutzeroberfläche (UI) anzuzeigen. In der Regel wird diese Benutzeroberfläche aus den Modelldaten erstellt. Ein Beispiel wäre eine Bearbeitungsansicht einer Produkttabelle, in der Textfelder, Dropdownlisten und Kontrollkästchen, die basierend auf den aktuellen Status eines Objekts Produkte angezeigt.

- **Controller**. Controller sind Komponenten, die Benutzerinteraktionen verarbeiten, mit dem Modell arbeiten und letztlich eine Ansicht zu rendern, die Benutzeroberfläche zeigt auswählen. In einer MVC-Anwendung zeigt die Ansicht nur Informationen an. Benutzereingaben und -interaktionen werden vom Controller verarbeitet und beantwortet. Beispielsweise wird der Controller behandelt die Abfragezeichenfolgen-Werte und übergibt diese Werte an das Modell, das wiederum die Datenbank abgefragt wird mithilfe der Werte.

Das MVC-Muster können Sie Anwendungen erstellen, die die verschiedenen Aspekte der Anwendung (Eingabelogik, Geschäftslogik und UI-Logik), und gleichzeitig eine lose Kopplung zwischen diesen Elementen zu trennen. Das Muster gibt an, in dem jede Art von Logik in der Anwendung befinden soll. Die Benutzeroberflächenlogik ist Teil der Ansicht (view). Die Eingabelogik gehört zum Controller. Die Geschäftslogik gehört zum Modell. Diese Trennung ermöglicht Ihnen das bewältigen von Komplexität beim Erstellen einer Anwendung, da Sie sich auf einen Aspekt der Implementierung zu einem Zeitpunkt konzentrieren können. Beispielsweise können Sie in der Ansicht ohne von der Geschäftslogik konzentrieren.   
  
Zusätzlich zur Verwaltung der Komplexität erleichtert das MVC-Muster zum Testen von Anwendungen, als zu eine Web Forms-basierten ASP.NET-Webanwendung zu testen. In einer Web Forms-basierten ASP.NET-Webanwendung wird z. B. eine einzelne Klasse sowohl zur Anzeige der Ausgabe als auch auf Benutzereingaben reagieren verwendet. Das Schreiben automatisierter Tests für ASP.NET Web Forms-basierten Anwendungen kann komplex sein, da zum Testen einer einzelnen Seite die Seitenklasse, alle seine untergeordneten Steuerelemente und zusätzliche abhängige Klassen in der Anwendung instanziiert werden müssen. Da so viele Klassen instanziiert werden, um die Seite ausführen, kann es schwierig sein, Tests zu schreiben, die sich ausschließlich auf einzelne Teile der Anwendung konzentrieren. Tests für ASP.NET Web Forms-basierte Anwendungen können daher schwieriger zu implementieren als Tests in einer MVC-Anwendung sein. Darüber hinaus ist für Tests in einer Web Forms-basierten ASP.NET-Anwendung ein Webserver erforderlich. Das MVC-Framework entkoppelt die Komponenten und nutzt intensiv Schnittstellen, die es ermöglichen, einzelne Komponenten isoliert vom Rest des Frameworks zu testen.   
  
Die lose Kopplung zwischen den drei Hauptkomponenten einer MVC-Anwendung begünstigt auch parallele Entwicklung. Z. B. ein Entwickler kann die auf die Ansicht, ein zweiter Entwickler an der Controllerlogik arbeiten kann und ein dritter Entwickler kann sich auf die Geschäftslogik im Modell konzentrieren.

## <a name="deciding-when-to-create-an-mvc-application"></a>Entscheidungshilfen für die Erstellung eine MVC-Anwendung

Sie müssen sorgfältig, ob eine Webanwendung mit ASP.NET MVC-Framework oder das ASP.NET Web Forms-Modell implementiert. Das MVC-Framework ist kein Ersatz für das Web Forms-Modell. Sie können jedes der beiden Frameworks für Webanwendungen verwenden. (Wenn Sie eine vorhandene Web Forms-basierten Anwendungen verfügen, weiterhin diese genau wie gewohnt funktioniert.)   
  
Bevor Sie das MVC-Framework oder das Web Forms-Modell für eine bestimmte Website verwenden möchten, wiegen Sie die Vorteile beider Ansätze.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vorteile einer MVC-basierten Webanwendung

ASP.NET MVC-Framework bietet die folgenden Vorteile:

- Es erleichtert die Komplexität zu verwalten, indem Sie eine Anwendung in das Modell, Ansicht und den Controller zu unterteilen.
- Es verwendet keine Ansichtszustand oder severbasierten Formularen. Dadurch wird das MVC-Framework für Entwickler, die vollständige Kontrolle über das Verhalten einer Anwendung ideal.
- Er verwendet ein Frontcontroller-Schema, das webanwendungsanforderungen durch einen einzelnen Controller verarbeitet. Dadurch können Sie eine Anwendung entwerfen, die eine umfangreiche Routinginfrastruktur unterstützen. Weitere Informationen finden Sie unter [Frontcontroller](https://go.microsoft.com/fwlink/?LinkId=106357 "Frontcontroller") auf der MSDN-Website.
- Es bietet bessere Unterstützung für testgesteuerte Entwicklung (TDD).
- Es eignet sich gut für Webanwendungen, die unterstützt werden, indem große Teams, die von Entwicklern und Web-Designer, die ein hohes Maß an Kontrolle über das Verhalten der Anwendung benötigen.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vorteile einer Web Forms-basierten Webanwendung

Das Web Forms-basierte Framework bietet die folgenden Vorteile:

- Es unterstützt ein Ereignismodell, der Zustand über HTTP beibehält, die Entwicklung von LOB-Webanwendungen profitieren. Die Web Forms-basierte Anwendung stellt Dutzende von Ereignissen, die in Hunderten von Serversteuerelementen unterstützt werden.
- Er verwendet ein Seitencontroller-Muster, das Funktionen für einzelne Seiten hinzugefügt. Weitere Informationen finden Sie unter [Seitencontroller](https://go.microsoft.com/fwlink/?LinkId=106359 "Seitencontroller") auf der MSDN-Website.
- Er verwendet Ansichtszustand oder severbasierten Formularen, die Verwaltung von Zustandsinformationen vereinfachen können.
- Es eignet sich gut für kleine Teams von Web-Entwicklern und Designern, die die große Anzahl von Komponenten, die für eine schnelle Anwendungsentwicklung nutzen möchten.
- Ist im Allgemeinen weniger komplex ist, für die Anwendungsentwicklung, da die Komponenten (die **Seite** -Klasse, Steuerelemente usw.) stark integriert sind und normalerweise weniger Code als das MVC-Modell erfordern.

## <a name="features-of-the-aspnet-mvc-framework"></a>Funktionen von ASP.NET MVC-Framework

ASP.NET MVC-Framework bietet die folgenden Features:

- Trennung von Anwendungsaufgaben, (Eingabelogik, Geschäftslogik und UI-Logik) Prüfbarkeit und testgesteuerter Entwicklung (TDD) in der Standardeinstellung. Alle Kernverträge im MVC-Framework sind, und können getestet werden, mithilfe von mock-Objekte, die simulierte Objekte sind, die das Verhalten tatsächlicher Objekte in der Anwendung imitieren. Sie können Komponententests die Anwendung ohne die Controller in einem ASP.NET-Prozess auszuführen, wodurch Komponententests schnell und flexibel. Sie können alle Komponententestframework verwenden, die mit .NET Framework kompatibel ist.
- Ein erweiterbares und austauschbares Framework. Die Komponenten von ASP.NET MVC-Framework sind so konzipiert, dass sie leicht ausgetauscht oder angepasst werden können. Sie können eigene Ansichts-Engine, URL-Routingrichtlinien, Aktion Serialisierung für Aktionsmethodenparameter und andere Komponenten einbinden. ASP.NET MVC-Framework unterstützt auch die Verwendung von Dependency Injection (DI) und die Umkehrung von Control (IOC) die steuerungsumkehrung bei containermodellen. DI ermöglicht Ihnen, Objekte in einer Klasse, anstatt für die Klasse zum Erstellen des Objekts selbst einzufügen. IOC gibt an, dass die ersten Objekte Wenn ein Objekt eines anderen Objekts erforderlich ist, das zweite Objekt aus einer externen Quelle, z. B. einer Konfigurationsdatei abrufen soll. Dadurch wird das Testen vereinfacht.
- Eine leistungsstarke URL-Zuordnungskomponente, mit dem Sie Anwendungen erstellen, die verständlichen und suchbaren URLs haben. URLs müssen keine Dateinamenerweiterungen enthalten und dienen zur Unterstützung von URL-Benennungsschemas, die funktionieren gut für Search engine Optimization (SEO) und die representational State Transfer (REST), adressiert.
- Unterstützung für das Verwenden von Markup aus vorhandenen ASP.NET-Seite (ASPX-Dateien), das Benutzersteuerelement (ASCX-Dateien) und master Seite (Master-Dateien) als Ansichtsvorlagen. Sie können vorhandene ASP.NET-Funktionen mit dem ASP.NET MVC-Framework, z. B. verschachtelte Gestaltungsvorlagen, Inlineausdrücke (&lt;% = %&gt;), deklarative Serversteuerelemente, Vorlagen, Datenbindung, Lokalisierung und So weiter.
- Unterstützung für vorhandene ASP.NET-Funktionen. ASP.NET MVC können Sie die Features wie z. B. Formularauthentifizierung und Windows-Authentifizierung, URL-Autorisierung, Mitgliedschaft und Rollen, Ausgabe und das Zwischenspeichern von Daten, Sitzung und profilzustandsverwaltung, Systemüberwachung, das Konfigurationssystem und der Anbieter verwenden. Architektur.
