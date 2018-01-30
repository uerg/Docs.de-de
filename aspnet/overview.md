---
uid: overview
title: "Übersicht über ASP.NET | Microsoft Docs"
author: rick-anderson
description: "Einführung in ASP.NET ist ein kostenloses Framework zum Erstellen von Websites, Webanwendungen und Web-APIs."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2010
ms.topic: article
ms.assetid: 3a309468-f1ca-4e51-b9c3-536af79d7a8b
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: 
msc.type: content
ms.openlocfilehash: 3d4c34a35e2e34ed78f481c759eda3718edb4da6
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="aspnet-overview"></a>Übersicht über ASP.NET

ASP.NET ist ein kostenloses Webframework zum Erstellen von großen Websites und Webanwendungen mit HTML, CSS und JavaScript. Sie können auch Web-APIs erstellen und echtzeittechnologien wie z. B. WebSockets.

[ASP.NET Core](https://docs.microsoft.com/aspnet/core/) ist eine Alternative zum ASP.NET.  Finden Sie unter der [Anleitungen zur Wahl zwischen ASP.NET und ASP.NET Core](https://docs.microsoft.com/aspnet/core/choose-aspnet-framework).

## <a name="get-started"></a>Erste Schritte

[Herunterladen von Visual Studio 2015](https://go.microsoft.com/fwlink/?LinkId=826064), eine IDE für ASP.NET unter Windows frei.

## <a name="websites-and-web-applications"></a>Websites und Webanwendungen

 ASP.NET bietet drei Frameworks zum Erstellen von Webanwendungen: Web Forms, ASP.NET MVC und ASP.NET Web Pages. Alle drei Frameworks sind stabil und ausgereifte und können Sie mit einer von ihnen hervorragende Webanwendungen erstellen. Unabhängig davon, welche Framework, die Sie auswählen, erhalten Sie die Vorteile und Funktionen von ASP.NET überall.

Jedes Framework als Ziel verwendet einen anderen Entwicklungsstil. Eine gewählten hängt eine Kombination der Programmierung Medienobjekte (wissen, Qualifikationen und entwicklungserfahrung), den Typ der Anwendung, den Sie erstellen und die Entwicklungsansatz, denen Sie vertraut sind.

Es folgt eine Übersicht der einzelnen Frameworks und einige Ideen zur Vorgehensweise für die Auswahl zwischen diesen. Wenn Sie ein Einführungsvideo bevorzugen, finden Sie unter [ausführenden Websites mit ASP.NET](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/Making-Websites-with-ASPNET) und [was Webtools ist?](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/what-is-web-tools)

|   | Wenn Sie Erfahrung haben | Entwicklungsstil | Kenntnisse | 
|-----------|----------------------|-----------------------------------------------------|----------------|
| Web Forms | Win Forms, WPF und .NET | Schnelle Entwicklung über eine umfangreiche Bibliothek von Steuerelementen, die HTML-Markup zu kapseln. | Mittleren Ebene, erweiterte RAD |
| MVC       | Ruby Schienen .NET  | Vollständige Kontrolle über die HTML-Markup, Code und Markup getrennt und leicht zu Tests schreiben. Die beste Wahl für mobile und Einzelseiten-Anwendungen (SPA). | Mittleren Ebene, erweitert |
| Webseiten  | Klassische ASP PHP     | HTML-Markup und Code in der gleichen Datei zusammen | Neue, mittleren Ebene |

### <a name="web-forms"></a>Web Forms

Mit ASP.NET Web Forms können Sie dynamische Websites mithilfe eines vertrauten Drag & Drop, ereignisgesteuerte-Modells erstellen. Eine Entwurfsoberfläche und Hunderte von Steuerelementen und Komponenten ermöglichen Ihnen das schnelle einfach komplexe und umfangreiche GUI-gesteuerte Websites mit Datenzugriff erstellen. 

[Weitere Informationen zu Web Forms](web-forms/index.md)

### <a name="mvc"></a>MVC

ASP.NET MVC bietet Ihnen eine leistungsfähige, auf Mustern basierende Möglichkeit zum Erstellen dynamischer Websites, mit der eine klare Trennung von Anliegen und die vollständige Kontrolle über Markup für die angenehme, agile-Entwicklung. ASP.NET MVC umfasst zahlreiche Funktionen, die ermöglichen die schnelle, TDD-freundliche Entwicklung ermöglichen, um anspruchsvolle Anwendungen, die die aktuellsten Webstandards verwenden. 

[Weitere Informationen zu MVC](mvc/index.md)

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

ASP.NET Web Pages und Razor-Syntax bieten eine schnelle, bequeme und einfache Möglichkeit zum Kombinieren von Servercode HTML dynamische Webinhalte zu erstellen. Herstellen einer Verbindung mit Datenbanken, Hinzufügen von Videos, Verknüpfen mit social Network-Sites und gehören viele weitere Funktionen, mit denen Sie erstellen ansprechender Websites, die die aktuellsten Webstandards entsprechen.

[Weitere Informationen zu Web Pages](web-pages/index.md)

### <a name="notes-about-web-forms-mvc-and-web-pages"></a>Hinweise zu WebForms, MVC und Web Pages

Alle drei ASP.NET Frameworks basieren auf .NET Framework und Freigeben der Kernfunktionalität von .NET und von ASP.NET. Zum Beispiel alle drei Frameworks bieten eine Anmeldung Sicherheitsmodell Mitgliedschaft basieren, und alle drei gemeinsam die gleichen Funktionen zum Verwalten der Anforderungen, Behandlung von Sitzungen und So weiter, die Teil der Kernfunktionalität von ASP.NET sind.

Darüber hinaus die drei Frameworks sind kein Punkt an vollkommen unabhängig, und eine schließt das nicht mit einem anderen. Da die Frameworks in derselben Webanwendung gleichzeitig ausgeführt werden können, ist es nicht ungewöhnlich, dass einzelne Komponenten der Anwendungen, die unter Verwendung verschiedener Frameworks geschrieben. Kundenorientierte Teile einer App können beispielsweise entwickelt werden, in MVC das Markup optimiert werden, während die Datenzugriffs- und administrative Teile in Web Forms-Steuerelemente und einfachen Datenzugriff nutzen entwickelt wurden.

## <a name="web-apis"></a>Web-APIs

ASP.NET Web-API ist ein Framework, das erleichtert das Erstellen von HTTP-Diensten, die eine Breite Palette von Clients, einschließlich Browsern und mobilen Geräten erreichen. Die ASP.NET-Web-API ist eine ideale Plattform zum Erstellen von RESTful-Anwendungen in .NET Framework.

[Weitere Informationen zu Web-API](web-api/index.md)

<!-- Put first under Web API TOC:  Watch video (9 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/services-and-aspnet -->

## <a name="real-time-technologies"></a>Echtzeit-Technologien

ASP.NET SignalR ist eine neue Bibliothek für ASP.NET-Entwickler,, die beim Entwickeln von Echtzeit-Webfunktionen erleichtert wird. SignalR ermöglicht die bidirektionale Kommunikation zwischen Server und Client. Server können Inhalte mithilfe von push sofort an verbundene Clients sobald sie verfügbar sind. SignalR WebSockets unterstützt und auf andere kompatibel Techniken für älteren Browsern zurückgegriffen. SignalR enthält APIs für die verbindungsverwaltung (z. B. eine Verbindung herstellen und trennungsereignisse) Gruppieren von Verbindungen und Autorisierung.

[Weitere Informationen zu SignalR](signalr/index.md)

<!-- Put first under SignalR TOC:  Watch video (6 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/signalr-and-the-real-time-web -->

## <a name="mobile-apps-and-sites"></a>Mobile apps und Websites 

ASP.NET können systemeigene mobile apps mit einem Web-API-Back-End als auch mobile Websites, die mithilfe des reaktionsfähiges Design-Frameworks wie Twitter Bootstrap einschalten. Wenn Sie eine systemeigene mobile app erstellen, ist es einfach, eine JSON-basierte-Web-API mit dem Handle Daten, Authentifizierung, und Pushbenachrichtigungen für Ihre app zu erstellen. Wenn Sie eine reaktionsfähige mobile Website erstellen, können Sie CSS-Framework oder offene Rastersystem Sie es vorziehen, oder wählen Sie ein leistungsfähiges mobiles System wie jQuery Mobile oder Sencha und hervorragende mobilen Anwendungen mit PhoneGap.

[Erfahren Sie mehr über die Entwicklung für mobile app und Standort](mobile/index.md)

<!-- Put first under mobile TOC:  Watch video (11 minutes) https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/aspnet-and-mobile -->

## <a name="single-page-applications"></a>Single-Page-Anwendungen 

ASP.NET Single-Page-Anwendung (SPA) können Sie Anwendungen erstellen, die erhebliche clientseitige Interaktion mit HTML 5, CSS 3 und JavaScript enthalten. Visual Studio enthält eine Vorlage zum Erstellen von einseitige Anwendungen mit knockout.js und ASP.NET Web-API. Zusätzlich zu den integrierten SPA-Vorlage sind auch Community erstellte SPA Vorlagen zum Download zur Verfügung.

[Erfahren Sie mehr über die Entwicklung für Einzelseiten-app](single-page-application/index.md)

## <a name="webhooks"></a>WebHooks

WebHooks ist eine einfache HTTP-Muster, die eine einfache und Abonnementmodell für Verkabelung zusammen Web-APIs und SaaS-Dienste bereitstellen. Wenn ein Ereignis in einem Dienst auftritt, wird eine Benachrichtigung an die registrierten Abonnenten in Form einer HTTP POST-Anforderung gesendet. Die POST-Anforderung enthält Informationen zum Ereignis, wodurch es möglich, dass der Empfänger entsprechend reagieren.

WebHooks sind eine große Anzahl von Diensten, einschließlich Dropbox, GitHub, Instagram, MailChimp, PayPal, Slack, Trello und vieles mehr zur Verfügung gestellt. Beispielsweise kann ein WebHook anzugeben, dass in Dropbox, eine Datei geändert wurde oder eine Änderung des Codes in GitHub ein Commit ausgeführt wurde hat oder eine Zahlung wurde in PayPal initiiert, oder eine Karte in Trello erstellt wurde.

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
