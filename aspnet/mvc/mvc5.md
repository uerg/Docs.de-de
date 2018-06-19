---
uid: mvc/mvc5
title: ASP.NET MVC 5 | Microsoft Docs
author: rick-anderson
description: ASP.NET MVC 5 ASP.NET MVC 5 ist ein Framework zum Erstellen von skalierbaren, standardbasierte Windows-Webanwendungen, die unter Verwendung von bewährte Entwurfsmuster geeinigt und die Leistungsfähigkeit von AS....
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: f79fbf7f-59e5-4279-a832-c1a0294630f4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc5
msc.type: content
ms.openlocfilehash: 1b3f920b51a70757ec0e20e36fa8e7dc329e663d
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
ms.locfileid: "30077919"
---
<a name="aspnet-mvc-5"></a>ASP.NET MVC 5
====================
## <a name="whats-new-in-aspnet-mvc-5"></a>Was ist neu in ASP.NET MVC 5

### <a name="one-aspnet"></a>One ASP.NET

Die Web-MVC-Projektvorlagen nahtlose Integration in die neue Umgebung für eine ASP.NET. Sie können das MVC-Projekt anpassen und konfigurieren Sie die Authentifizierung mit dem Assistenten für eine ASP.NET erstellen. Ein einführendes Lernprogramm zu ASP.NET MVC 5 finden Sie unter [erste Schritte mit ASP.NET MVC 5](overview/getting-started/introduction/getting-started.md).

Informationen zum Upgrade von MVC 4-Projekte auf MVC 5 finden Sie unter [das Upgrade von einer ASP.NET MVC 4 und Web-API-Projekt auf ASP.NET MVC 5 und Web-API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md).

### <a name="aspnet-identity"></a>ASP.NET Identity

Die MVC-Projektvorlagen wurden aktualisiert, um ASP.NET Identity für die Authentifizierung und identitätsverwaltung verwenden. Ein Lernprogramm mit Facebook und Google-Authentifizierung und die neue Mitgliedschafts-API finden Sie unter [erstellen Sie eine ASP.NET MVC 5-App mit Facebook und Google OAuth2 und OpenID-Sign-on](overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) und [bereitstellen eine Secure ASP.NET MVC-Anwendung mit Mitgliedschaft, OAuth und SQL-Datenbank auf einer Windows Azure-Website](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

### <a name="bootstrap"></a>Bootstrap

Die MVC-Projektvorlage wurde aktualisiert, um verwenden [Bootstrap](http://getbootstrap.com/) , ein schlankes und reaktionsschnelle Aussehen und Verhalten bereitzustellen, die Sie leicht anpassen können. Weitere Informationen finden Sie unter [Bootstrap in Visual Studio 2013 Webprojektvorlagen](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap).

### <a name="authentication-filters"></a>Authentifizierungsfilter

[Authentifizierungsfilter](http://www.dotnetcurry.com/showarticle.aspx?ID=957) sind eine neue Art von Filter in ASP.NET MVC, die vor dem Autorisierungsfilter in der ASP.NET MVC-Pipeline ausgeführt werden und ermöglichen es Ihnen, geben Sie die Authentifizierung Logik pro-Aktion, pro Controller oder global für alle Controller. Authentifizierungsfilter Anmeldeinformationen in der Anforderung zu verarbeiten, und geben Sie einen entsprechenden Prinzipal. Authentifizierungsfilter können auch authentifizierungsaufforderungen als Antwort auf nicht autorisierte Anfragen hinzufügen. Finden Sie unter [ASP.NET MVC 5-Authentifizierungsfilter](http://www.dotnetcurry.com/showarticle.aspx?ID=957), [Authentifizierungsfilter in ASP.NET MVC 5](http://theshravan.net/blog/authentication-filters-in-asp-net-mvc-5/).

### <a name="filter-overrides"></a>Filtern von Außerkraftsetzungen

Sie können jetzt überschreiben, Anwenden der Filter auf eine bestimmte Aktionsmethode oder Controller durch Angabe einer [Filter überschreiben](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5). Außerkraftsetzung Filter geben Sie einen Satz von Typen, die für einen bestimmten Bereich (Aktions- oder Controllerebene) nicht ausgeführt werden soll. Dadurch können Sie so konfigurieren Sie Filter, die global angewendet, aber dann Ausschließen bestimmter globale Filter nicht auf bestimmte Aktionen oder Controllern angewendet. Finden Sie unter [Feature in ASP.NET MVC 5 und ASP.NET Web API 2 neue Filter überschreibt](https://weblogs.asp.net/imranbaloch/archive/2013/09/25/new-filter-overrides-in-asp-net-mvc-5-and-asp-net-web-api-2.aspx), [zum Verwenden der ASP.NET MVC 5 überschreibt Filterfunktion](http://hackwebwith.net/how-to-use-the-asp-net-mvc-5-filter-overrides-feature/), und [Filter in ASP.NET MVC 5 überschreibt](http://www.davidhayden.me/blog/filter-overrides-in-asp-net-mvc-5)

### <a name="attribute-routing"></a>Attributrouting

ASP.NET MVC unterstützt jetzt auch [routing-Attribut](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx), Dank einen Beitrag von Tim McCall der Autor [ http://attributerouting.net ](http://attributerouting.net). Mit routing-Attribut können Sie Ihre Routen angeben, durch Hinzufügen einer Anmerkung zu Ihrer Aktionen und Controller.

## <a name="new-web-project-experience"></a>Neue Web Project-Benutzeroberfläche

Wir haben die Funktion zum Erstellen neuer Webprojekte in Visual Studio 2013 erweitert. In der **neuen ASP.NET-Webprojekts** Dialogfeld können Sie den Projekttyp werden soll, konfigurieren eine beliebige Kombination aus Technologien (Web Forms, MVC, Web-API), konfigurieren Sie Authentifizierungsoptionen und fügen Sie ein Komponententestprojekt hinzu auswählen.

![Neues ASP.NET-Projekt](mvc5/_static/image1.png)

Das Dialogfeld "neue" können Sie die standardmäßigen Authentifizierungsoptionen für viele der Vorlagen ändern. Z. B. beim Erstellen einer ASP.NET Web Forms-Projekt können Sie eine der folgenden Optionen auswählen:

- Keine Authentifizierung
- Einzelne Benutzerkonten (ASP.NET-Mitgliedschaft oder soziale Anbieter anzumelden)
- Organisations-Konten (Active Directory in einer internetanwendung)
- Windows-Authentifizierung (Active Directory in einem Intranetanwendung)

![-Authentifizierungsoptionen.](mvc5/_static/image2.png)

Weitere Informationen zu den neuen Prozess zum Erstellen von Webprojekten, finden Sie unter [Erstellen von ASP.NET-Webprojekten in Visual Studio 2013](../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md). Weitere Informationen zu den neuen Authentifizierungsoptionen finden Sie unter [ASP.NET Identity](../identity/overview/index.md).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>ASP.NET Gerüstbau

ASP.NET Gerüstbau ist ein Code-Generierung-Framework für ASP.NET-Webanwendungen. Es vereinfacht den Standardcode zu Ihrem Projekt hinzufügen, die mit einem Datenmodell interagiert.

In früheren Versionen von Visual Studio konnte Gerüstbau ASP.NET MVC-Projekte. Mit Visual Studio 2013 können Sie jetzt Gerüst für eine beliebige ASP.NET-Projekt, einschließlich Web Forms verwenden. Visual Studio 2013 unterstützt zurzeit nicht Generieren von Seiten für eine Web Forms-Projekt, aber Sie können weiterhin Gerüstbau mit Web Forms verwenden, indem Sie das Projekt MVC-Abhängigkeiten hinzufügen. Unterstützung für das Generieren von Seiten für Web Forms wird in einem zukünftigen Update hinzugefügt werden.

Mithilfe von Gerüstbau stellen wir sicher, dass alle erforderlichen Abhängigkeiten im Projekt installiert sind. Z. B. Wenn Sie mit einer ASP.NET Web Forms-Projekt beginnen und dann mithilfe von Gerüstbau einer Web-API-Controller hinzufügen, werden die erforderlichen NuGet-Pakete und Verweise dem Projekt automatisch hinzugefügt.

Um Gerüstbau für MVC eine Web Forms-Projekt hinzuzufügen, fügen einen **neues Gerüstelement** , und wählen Sie **MVC 5-Abhängigkeiten** im Dialogfenster. Es gibt zwei Optionen für MVC Gerüstbau; Minimal "und" vollständig ". Wenn Sie die minimale auswählen, werden nur die NuGet-Pakete und Verweise für ASP.NET MVC zu Ihrem Projekt hinzugefügt. Wenn Sie die Option "vollständig", wählen Sie die minimale Abhängigkeiten hinzugefügt werden, sowie die erforderlichen Inhaltsdateien für ein MVC-Projekt.

Unterstützung für asynchrone Controller Gerüstbau verwendet die neue asynchrone Funktionen von Entity Framework 6.

Weitere Informationen und Lernprogramme finden Sie unter [Gerüstbau-Übersicht über ASP.NET](../visual-studio/overview/2013/aspnet-scaffolding-overview.md).

### <a name="getting-help-and-reporting-issues"></a>Abrufen von Hilfe und Melden von Problemen

- [Bekannte Probleme und aktuelle Änderungen Liste](../visual-studio/overview/2013/release-notes.md#knownissues)
- Abrufen von Hilfe und Erläutern Sie ASP.NET MVC 5 in der [Foren](https://forums.asp.net/1146.aspx)
- [Melden eines Fehlers in ASP.NET MVC 5](https://github.com/aspnet/AspNetWebStack/issues)
- [Stellen Sie eine Anforderung zur](http://aspnet.uservoice.com/forums/41201-asp-net-mvc)

### <a name="upgrading-from-aspnet-mvc-4"></a>Aktualisieren von ASP.NET MVC 4

Finden Sie unter [zum Aktualisieren eines ASP.NET MVC 4 und Web-API-Projekt für ASP.NET MVC 5 und Web-API 2](overview/releases/how-to-upgrade-an-aspnet-mvc-4-and-web-api-project-to-aspnet-mvc-5-and-web-api-2.md)
