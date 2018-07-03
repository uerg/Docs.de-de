---
uid: web-api/overview/security/forms-authentication
title: Formularauthentifizierung in ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: Beschreibt die Verwendung der Formularauthentifizierung in ASP.NET Web-API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 7642a30816a04a88a25ef8bf4f01e766fda362ce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365070"
---
<a name="forms-authentication-in-aspnet-web-api"></a>Formularauthentifizierung in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Die Formularauthentifizierung verwendet ein HTML-Formular, um die Anmeldeinformationen des Benutzers an den Server gesendet. Es ist kein Internetstandard. Die Formularauthentifizierung eignet sich nur für Web-APIs, die aus einer Webanwendung aufgerufen werden, damit der Benutzer mit dem HTML-Formular interagieren kann.

| Vorteile | Nachteile |
| --- | --- |
| – Leicht zu implementieren: in ASP.NET integriert. – Verwendet ASP.NET-Mitgliedschaftsanbieter ab, der Verwaltung von Benutzerkonten vereinfacht. | – Keiner standard-HTTP-Authentifizierungsmechanismus; verwendet HTTP-Cookies anstelle der standardmäßigen Authorization-Header. – Einen Browserclient erfordert. -Anmeldeinformationen werden als Klartext gesendet. -Anfällig für websiteübergreifende anforderungsfälschung (CSRF); erfordert, dass Anti-CSRF-Measures. -Schwierig, von nichtsuchdienst Clients zu verwenden. Anmeldung erfordert einen Browser. -Benutzer Anmeldeinformationen werden in der Anforderung gesendet. – Einige Benutzer deaktivieren Cookies. |

Kurz gesagt, Formularauthentifizierung in ASP.NET funktioniert wie folgt:

1. Der Client fordert eine Ressource, die eine Authentifizierung erforderlich ist.
2. Wenn der Benutzer nicht authentifiziert ist, wird der Server Gibt HTTP 302 (Found) und an eine Anmeldeseite umgeleitet.
3. Der Benutzer Anmeldeinformationen eingibt und das Formular übermittelt.
4. Der Server gibt eine andere HTTP-302, der an den ursprünglichen URI umleitet. Diese Antwort enthält ein Authentifizierungscookie.
5. Der Client fordert die Ressource erneut aus. Die Anforderung enthält das Authentifizierungscookie und damit der Server die Anforderung gewährt.

![](forms-authentication/_static/image1.png)

Weitere Informationen finden Sie unter [eine Übersicht der Formularauthentifizierung.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Verwenden der Formularauthentifizierung mit Web-API

Wählen Sie zum Erstellen einer Anwendung, die Formularauthentifizierung verwendet, die "Internetanwendung"-Vorlage im Assistenten für MVC 4. Diese Vorlage erstellt die MVC-Controller für die kontoverwaltung. Sie können auch die Vorlage "Single Page Application", die in der ASP.NET Herbst 2012-Update verfügbar.

In Ihren Web-API-Controllern, können Sie den Zugriff mithilfe der `[Authorize]` Attribut, wie in beschrieben [mit dem [Authorize]-Attribut](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Formularauthentifizierung verwendet ein Sitzungscookie zum Authentifizieren von Anforderungen an. Browser senden automatisch alle relevanten Cookies an die Ziel-Website. Dieses Feature macht Formularauthentifizierung potenziell anfällige Cross-Site Request Forgery (CSRF) attacks finden Sie unter [verhindern von Cross-Site Request Forgery (CSRF) Angriffe](preventing-cross-site-request-forgery-csrf-attacks.md).

Formular-Authentifizierung werden die Anmeldeinformationen des Benutzers nicht verschlüsselt werden. Aus diesem Grund ist die Formularauthentifizierung nicht sicher, es sei denn, der mit SSL verwendet. Finden Sie unter [arbeiten mit SSL in Web-API](working-with-ssl-in-web-api.md).
