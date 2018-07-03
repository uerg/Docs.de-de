---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft-Dokumentation
author: rick-anderson
description: ASP.NET MVC 5 ASP.NET MVC 5 ist ein Framework zum Erstellen von skalierbaren, auf Standards basierende Webanwendungen, die mit bewährte Entwurfsmuster und die Leistungsfähigkeit von AS....
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: f3a81d249f186a2bdea9c25a3e178e13fd2bc830
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403031"
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>Neuerungen in ASP.NET MVC 5

### <a name="one-aspnet"></a>Eine ASP.NET

Die Web-MVC-Projektvorlagen nahtlose Integration in die neue Oberfläche für die One ASP.NET. Sie können Ihre MVC-Projekt anpassen und Konfigurieren von Authentifizierung unter Verwendung von One ASP.NET projekterstellungs-Assistenten. Ein einführendes Lernprogramm zu ASP.NET MVC 5 finden Sie unter [erste Schritte mit ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md).

Informationen zum Upgrade von MVC 4-Projekte auf MVC 5 finden Sie unter [das Upgrade von einer ASP.NET MVC 4 und Web-API-Projekt auf ASP.NET MVC 5 und Web-API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Die MVC-Projektvorlagen wurden aktualisiert, um ASP.NET Identity für Authentifizierung und identitätsverwaltung verwenden. Ein Tutorial mit Facebook und Google-Authentifizierung und der neuen Mitgliedschafts-API finden Sie unter [erstellen Sie eine ASP.NET MVC 5-App mit Facebook und Google OAuth2 und OpenID-Sign-on](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) und [Bereitstellen einer sicheren ASP.NET MVC-app mit Mitgliedschaft, OAuth und SQL-Datenbank in einer Windows Azure-Website](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

### <a name="bootstrap"></a>Bootstrap-Stil

MVC-Projektvorlage wurde aktualisiert, um verwenden [Bootstrap](http://getbootstrap.com/) , ein schlankes und reaktionsfähige Aussehen und Verhalten bereitzustellen, die Sie problemlos anpassen können. Weitere Informationen finden Sie unter [Bootstrap in den Visual Studio 2013-Web-Projektvorlagen](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Authentifizierungsfilter

[Authentifizierungsfilter](http://www.dotnetcurry.com/showarticle.aspx?ID=957) sind eine neue Art von Filter in ASP.NET MVC, die vor dem Autorisierungsfilter werden also in der ASP.NET MVC-Pipeline ausgeführt und ermöglichen es Ihnen, geben Sie die Authentifizierung Logik pro-Aktion, pro Controller oder global für alle Controller. Authentifizierungsfilter Anmeldeinformationen in der Anforderung zu verarbeiten, und geben einen entsprechenden Prinzipal. Authentifizierungsfilter können authentifizierungsanforderungen auch als Reaktion auf nicht autorisierte Anforderungen hinzufügen. Finden Sie unter [ASP.NET MVC 5-Authentifizierungsfilter](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [Authentifizierungsfilter in ASP.NET MVC 5](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/).

### <a name="filter-overrides"></a>Überschreibungsfilter

Sie können jetzt überschreiben, die Filter auf eine bestimmte Aktionsmethode bzw. einen Controller angewendet wird, durch Angabe einer [Filter überschreiben](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5). Überschreibungsfilter Geben Sie einen Satz von Filtertypen, die für einen bestimmten Bereich (Aktions- oder Controllerebene) nicht ausgeführt werden soll. Dadurch können Sie so konfigurieren Sie Filter, die global angewendet, aber dann bestimmte globale Filter ausschließen, nicht auf bestimmte Aktionen oder Controller angewendet. Finden Sie unter [neue Filterüberschreibungen-Feature in ASP.NET MVC 5 und ASP.NET Web API 2](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [wie mit der ASP.NET MVC 5 überschreibt Filterfunktion](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), und [Filterüberschreibungen in ASP.NET MVC 5](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>Attributrouting

Unterstützt nun das ASP.NET MVC [attributrouting](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), Dank der einen Beitrag von Tim McCall der Autor des [ http://attributerouting.net ](http://attributerouting.net). Das attributrouting können Sie Ihre Routen angeben, durch das Hinzufügen von Anmerkungen zu Ihren Aktionen und Controllern.

## <a name="new-web-project-experience"></a>Neue Project-Weboberfläche

Wir haben die Erfahrung mit der Erstellung neuer Webprojekte in Visual Studio 2013 verbessert. In der **neuen ASP.NET-Webprojekts** Dialogfeld können Sie den Projekttyp, Sie möchten, konfigurieren eine beliebige Kombination aus Technologien (Web Forms, MVC, Web-API), konfigurieren Sie Authentifizierungsoptionen und hinzufügen ein Komponententestprojekts, auswählen.

![Neues ASP.NET-Projekt](mvc5/_static/image1.png)

Der neue Dialog können Sie die standardmäßigen Authentifizierungsoptionen für viele der Vorlagen zu ändern. Z. B. beim Erstellen einer ASP.NET Web Forms-Projekts können Sie eine der folgenden Optionen auswählen:

- Keine Authentifizierung
- Einzelne Benutzerkonten (ASP.NET-Mitgliedschaft oder Anbieter sozialer-Anmeldung)
- Organisations-Konten (Active Directory in einer internetanwendung)
- Windows-Authentifizierung (Active Directory in einer Intranetanwendung)

![Authentifizierungsoptionen](mvc5/_static/image2.png)

Weitere Informationen zu den neuen Prozess für das Erstellen von Webprojekten, finden Sie unter [Erstellen von ASP.NET-Webprojekten in Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md). Weitere Informationen zu den neuen Authentifizierungsoptionen finden Sie unter [ASP.NET Identity](../identity/overview/index.md).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET-Gerüstbau

ASP.NET-Gerüstbau ist ein Code-Generierung-Framework für Webanwendungen mit ASP.NET. Es erleichtert die Codebausteine zu Ihrem Projekt hinzufügen, die mit einem Datenmodell interagiert.

In früheren Versionen von Visual Studio konnte Gerüstbau ASP.NET MVC-Projekte. Mit Visual Studio 2013 können Sie jetzt Gerüstbau für ASP.NET Projekte, einschließlich Web Forms. Visual Studio 2013 unterstützt derzeit keine Generieren von Seiten für eine Web Forms-Projekt, aber Sie können weiterhin Gerüstbau mit Web Forms verwenden, durch das Hinzufügen von MVC-Abhängigkeiten auf das Projekt. Unterstützung für das Generieren von Seiten für Web Forms wird in einem späteren Update hinzugefügt werden.

Wenn Gerüstbau verwenden zu können, stellen wir sicher, dass alle erforderlichen Abhängigkeiten im Projekt installiert sind. Z. B. Wenn Sie mit einem ASP.NET Web Forms-Projekt beginnen, und klicken Sie dann mithilfe von Gerüstbau einer Web-API-Controller hinzu, werden die erforderlichen NuGet-Pakete und Verweise dem Projekt automatisch hinzugefügt.

MVC-Gerüstbau um eine Web Forms-Projekt hinzuzufügen, fügen einen **neues Gerüstelement** , und wählen Sie **MVC 5-Abhängigkeiten** im Dialogfenster. Es gibt zwei Optionen für den Gerüstbau für MVC. Minimaler und vollständiger. Wenn Sie die minimale auswählen, werden nur die NuGet-Pakete und Verweise für ASP.NET MVC zu Ihrem Projekt hinzugefügt. Bei Auswahl der Option "vollständig", die mindestens erforderliche Abhängigkeiten hinzugefügt werden, sowie die erforderlichen Inhaltsdateien für ein MVC-Projekt.

Unterstützung für den Gerüstbau für Async-Controller verwendet, die neuen asynchronen Features von Entity Framework 6.

Weitere Informationen und Lernprogramme finden Sie unter [Gerüstbau-Übersicht über ASP.NET](../visual-studio/overview/2013/aspnet-scaffolding-overview.md).

### <a name="getting-help-and-reporting-issues"></a>Hilfe zu erhalten, und Melden von Problemen

- [Bekannte Probleme und wichtige Änderungsliste](../visual-studio/overview/2013/release-notes.md#knownissues)
- Hier erhalten Sie Hilfe und Erläutern Sie ASP.NET MVC 5 in der [Foren](https://forums.asp.net/1146.aspx)
- [Melden eines Problems in ASP.NET MVC 5](https://github.com/aspnet/AspNetWebStack/issues)
- [Feature anfordern](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>Aktualisieren von ASP.NET MVC 4

Finden Sie unter [das Upgrade von einer ASP.NET MVC 4 und Web-API-Projekts zum ASP.NET MVC 5 und Web-API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
