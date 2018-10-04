---
uid: overview
title: Übersicht über ASP.NET | Microsoft-Dokumentation
author: rick-anderson
description: Einführung in ASP.NET ist ein kostenloses Framework für das Erstellen von Websites, Webanwendungen und Web-APIs.
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.author: riande
ms.date: 03/12/2010
ms.technology: aspnet
msc.legacyurl: ''
msc.type: content
ms.openlocfilehash: 2dc48e1262b1807a77a9889f7e0e62c9b9ea463e
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48794810"
---
# <a name="aspnet-overview"></a>Übersicht über ASP.NET

ASP.NET ist eine kostenlose webumgebung zum Erstellen ansprechender Websites und Webanwendungen mit HTML, CSS und JavaScript. Sie können auch Web-APIs zu erstellen und echtzeittechnologien wie Web Sockets.

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) ist eine Alternative zum ASP.NET.  Finden Sie unter den [Anleitungen zur Wahl zwischen ASP.NET und ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Erste Schritte

Installieren Sie [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community-Edition, eine kostenlose IDE für ASP.NET unter Windows.

## <a name="websites-and-web-applications"></a>Websites und Webanwendungen

 ASP.NET bietet drei Frameworks zum Erstellen von Webanwendungen: Web Forms, ASP.NET MVC und ASP.NET Web Pages. Alle drei Frameworks sind stabil und ausgereifte, und Sie tolle Webanwendungen mit einer von ihnen erstellen. Unabhängig davon, welches Framework Sie auswählen, erhalten Sie alle Vorteile und Funktionen von ASP.NET überall.

Jedes Framework zielt auf einen anderen Entwicklungsstil. Die gewählte abhängig ist auf eine Kombination aus Programmiersprachen Medienobjekte (Kenntnisse, Fähigkeiten und Erfahrung in der Entwicklung), die den Typ der Anwendung aus, die Sie erstellen, und der Entwicklungsansatz, dem Sie mit vertraut.

Es folgt eine Übersicht über jedes der Frameworks und einige Ideen zur Auswahl zwischen diesen. Ein Einführungsvideo finden Sie unter [vornehmen von Websites mit ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) und [was Webtools ist?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Wenn Sie Erfahrung, in haben | Entwicklungsstil | Fachwissen |
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web Forms | Win Forms, WPF, .NET | Eine schnelle Entwicklung mithilfe einer umfangreichen Bibliothek von Steuerelementen, die HTML-Markup zu kapseln | Mittlere, erweiterte RAD |
| MVC       | Ruby on Rails .NET  | Vollständige Kontrolle über die HTML-Markup, Code und Markup getrennt und leicht zu Tests zu schreiben. Die beste Wahl für mobile und Single-Page-Anwendungen (SPA). | Mittlere, erweiterte |
| Webseiten  | Klassische ASP, PHP     | HTML-Markup und Code in der gleichen Datei zusammen | Neue, mittlere |

### <a name="web-forms"></a>Web Forms

Mit ASP.NET Web Forms können Sie dynamische Websites mithilfe eines vertrauten Drag & Drop, ereignisgesteuerte-Modells erstellen. Eine Entwurfsoberfläche und Hunderte von Steuerelementen und Komponenten können Sie schnell komplexe und umfangreiche GUI-gesteuerte Websites mit Datenzugriff erstellen.

[Erfahren Sie mehr über Web Forms](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC bietet Ihnen leistungsfähige, auf Mustern basierende Funktionen zum Erstellen dynamischer Websites, ermöglicht eine klare Trennung von Anliegen und die vollständige Kontrolle über Markup für angenehm, agile-Entwicklung. ASP.NET MVC umfasst zahlreiche Funktionen, mit denen schnelle, TDD-freundliche Entwicklung ermöglichen, um anspruchsvolle Anwendungen, die die aktuellsten Webstandards verwenden.

[Erfahren Sie mehr über MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

ASP.NET Web Pages und Razor-Syntax bieten eine schnelle, bedienungsfreundliche und einfache Möglichkeit zum Kombinieren von Servercode mit HTML dynamische Webinhalte zu erstellen. Verbinden mit Datenbanken, Hinzufügen von Videos, Verknüpfen mit sozialen Netzwerken und enthalten viele weitere Features, mit denen Sie erstellen Sie ansprechende Sites, die die aktuellsten Webstandards entsprechen.

[Erfahren Sie mehr über die Webseiten](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Hinweise zu Web Forms, MVC und Webseiten

Alle drei ASP.NET Frameworks basieren auf .NET Framework und Freigeben von .NET und ASP.NET Core-Funktionalität. Z. B. alle drei Frameworks bieten ein Login-Sicherheitsmodell, um die Mitgliedschaft, und alle drei Teilen die gleichen Funktionen für die Verwaltung von Anforderungen, Behandlung von Sitzungen und So weiter, die Teil der Funktionalität von ASP.NET Core sind.

Darüber hinaus die drei Frameworks sind nicht vollständig voneinander unabhängig, und eine schließt das nicht mit einem anderen. Da die Frameworks in der gleichen Webanwendung gleichzeitig verwendet werden können, ist es nicht ungewöhnlich, die einzelnen Komponenten von Anwendungen unter Verwendung verschiedener Frameworks zu finden. Kundenorientierte Teile einer Anwendung können z. B. entwickelt werden, in MVC, um das Markup zu optimieren, während der Datenzugriff und administrative Teile in Web Forms-Datensteuerelemente und einfache Datenzugriff nutzen entwickelt werden.

## <a name="web-apis"></a>Web-APIs

ASP.NET Web-API ist ein Framework, das erleichtert die Erstellung von HTTP-Diensten, die eine Breite Palette von Clients, einschließlich Browsern und mobilen Geräten zu erreichen. Die ASP.NET-Web-API ist eine ideale Plattform zum Erstellen von RESTful-Anwendungen in .NET Framework.

[Erfahren Sie mehr über Web-API](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Real-Time-Technologien

ASP.NET SignalR ist eine neue Bibliothek für ASP.NET-Entwickler,, die Entwicklung von Echtzeit-Webfunktionen einfacher macht. SignalR ermöglicht die bidirektionale Kommunikation zwischen Server und Client. Server können Inhalte mithilfe von push sofort an verbundene Clients sobald diese verfügbar werden. SignalR-WebSockets unterstützt und greift auf andere kompatiblen Techniken für ältere Browser zurück. SignalR enthält APIs für die verbindungsverwaltung (z. B. eine Verbindung herstellen und trennungsereignisse), Gruppieren von Verbindungen und -Autorisierung.

[Weitere Informationen zu SignalR](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Mobile apps und Websites

ASP.NET kann es sich um native mobile apps mit einem Web-API-Back-End als auch mobile Websites mit reaktionsfähiges Design-Frameworks wie Twitter Bootstrap bereitstellen. Wenn Sie eine native mobile app erstellen, ist es einfach, eine JSON-basierte Web-API Zugriff auf die Handle-Daten, Authentifizierung und Pushbenachrichtigungen für Ihre app zu erstellen. Wenn Sie eine mobile reaktionsfähige Website erstellen, können Sie CSS-Framework oder Sie möchten, oder wählen Sie eine leistungsstarke mobile System wie jQuery Mobile oder Sencha und leistungsstarker Anwendungen für mobile mit PhoneGap-Rastersystem öffnen.

[Erfahren Sie mehr über die mobile Entwicklung von Apps und -Website](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Single-Page-Anwendungen

ASP.NET Single Page Application (SPA) können Sie die Anwendungen zu erstellen, die signifikante clientseitigen Interaktionen mit HTML 5, CSS 3 und JavaScript enthalten. Visual Studio enthält eine Vorlage zum Erstellen von einseitigen Anwendungen mithilfe von knockout.js und ASP.NET Web-API. Zusätzlich zu der integrierten SPA-Vorlage sind auch Community erstellte SPA-Vorlagen zum Download zur Verfügung.

[Erfahren Sie mehr über die Single-Page-app-Entwicklung](single-page-application/index.md)

## <a name="webhooks"></a>WebHooks

WebHooks ist ein einfacher HTTP-Muster, die eine einfache Veröffentlichungs-/Abonnementmodells verbinden zusammen Web-APIs und SaaS-Dienste bereitstellen. Wenn ein Ereignis in einem Dienst auftritt, wird eine Benachrichtigung an die registrierten Abonnenten in Form von HTTP POST-Anforderung gesendet. Die POST-Anforderung enthält Informationen über das Ereignis dadurch ist es möglich, dass der Empfänger entsprechend reagieren.

WebHooks werden durch eine große Anzahl von Diensten, einschließlich, Dropbox, GitHub, Instagram, MailChimp, PayPal, Slack, Trello und vieles mehr verfügbar gemacht. Ein WebHook kann beispielsweise angeben, in Dropbox eine Datei geändert wurde oder hat eine Änderung des Codes in GitHub ein Commit ausgeführt wurde oder eine Zahlung wurde in PayPal initiiert, oder eine Karte in Trello erstellt wurde.

[Erfahren Sie mehr über WebHooks](webhooks/index.md)





<!--
Create Deployment TOC based on https://www.asp.net/aspnet/overview/deployment
Copy deployment content map to MVC, WebForms, Web Pages, Web API sections.
Copy Web Deployment in Enterprise from WebForms to MVC
Move under ASP.NET Best practices
    What not to do in ASP.NET, and what to do instead https://review.docs.microsoft.cus/aspnet/aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
    Async and await https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/async-and-await
    Building Real World Cloud Apps with Azure https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
    Hands on Lab: Maintainable Azure Websites: Managing Change and Scale https://review.docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale

-->
