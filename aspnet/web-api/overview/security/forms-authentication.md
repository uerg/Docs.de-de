---
uid: web-api/overview/security/forms-authentication
title: Formularauthentifizierung in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: Beschreibt die Verwendung der Formularauthentifizierung in ASP.NET Web-API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9027d76bcf8854fc85f11906d3651511f350cd32
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="forms-authentication-in-aspnet-web-api"></a>Formularauthentifizierung in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Formularauthentifizierung verwendet ein HTML-Formular, um die Anmeldeinformationen des Benutzers an den Server gesendet. Es ist kein Internetstandard. Formularauthentifizierung eignet sich nur für Web-APIs, die aus einer Webanwendung aufgerufen werden, damit der Benutzer mit der HTML-Formular interagieren kann.

| Vorteile | Nachteile |
| --- | --- |
| – Einfach zu implementieren: in ASP.NET integriert. -ASP.NET-Mitgliedschaftsanbieter, wodurch das erleichtern die Verwaltung von Benutzerkonten verwendet. | -Keinen standard-HTTP-Authentifizierungsmechanismus; verwendet HTTP-Cookies anstelle der standardmäßigen Authorization-Header. – Einen Browserclient erfordert. -Anmeldeinformationen werden als Klartext gesendet. – Die anfällig für websiteübergreifende anforderungsfälschung (FORGERY); erfordert die Anti-CSRF-Measures. -Schwierig zu von Nonbrowser-Clients verwenden. Anmeldung erfordert einen Browser. -Benutzer Anmeldeinformationen werden in der Anforderung gesendet. -Einige Benutzer deaktivieren Cookies. |

Kurz gesagt Formularauthentifizierung in ASP.NET funktioniert wie folgt:

1. Der Client fordert eine Ressource, die eine Authentifizierung erforderlich ist.
2. Wenn der Benutzer nicht authentifiziert ist, wird der Server Gibt HTTP 302 (Found) und leitet zu einer Anmeldeseite um.
3. Der Benutzer Anmeldeinformationen eingibt und das Formular übermittelt.
4. Der Server gibt eine andere HTTP-302, die zurück an den ursprünglichen URI umgeleitet. Diese Antwort enthält ein Authentifizierungscookie.
5. Der Client fordert die Ressource erneut aus. Die Anforderung enthält das Authentifizierungscookie, damit der Server die Anforderung gewährt.

![](forms-authentication/_static/image1.png)

Weitere Informationen finden Sie unter [eine Übersicht über die Formularauthentifizierung verwenden.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Verwenden der Formularauthentifizierung mit Web-API

Wählen Sie zum Erstellen einer Anwendung, die Formularauthentifizierung verwendet, die "Internetanwendung"-Vorlage im Assistenten für MVC 4. Diese Vorlage erstellt MVC-Controller für die Kontenverwaltung. Sie können auch die Vorlage "Einseitigen-Anwendung" in der ASP.NET Herbst 2012-Update.

In die Web-API-Controller, können Sie den Zugriff mithilfe der `[Authorize]` Attribut, wie in beschrieben [mit dem Attribut [Authorize]](authentication-and-authorization-in-aspnet-web-api.md#auth3).

Formularauthentifizierung verwendet ein Sitzungscookie zum Authentifizieren von Anforderungen. Browser senden alle relevanten Cookies automatisch an den Zielstandort für das Web. Diese Funktion nutzt die Formularauthentifizierung potenziell anfällig für standortübergreifenden Anforderung Websiteübergreifender anforderungsfälschung-Angriffen finden Sie unter [verhindern Cross-Site Request (Angriffen FORGERY)](preventing-cross-site-request-forgery-csrf-attacks.md).

Formular-Authentifizierung werden keine Anmeldeinformationen des Benutzers verschlüsselt. Formularauthentifizierung ist daher nicht sicher ist, es sei denn, mit SSL verwendet. Finden Sie unter [arbeiten mit SSL in Web-API](working-with-ssl-in-web-api.md).
