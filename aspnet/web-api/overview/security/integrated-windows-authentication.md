---
uid: web-api/overview/security/integrated-windows-authentication
title: Integrierte Windows-Authentifizierung | Microsoft Docs
author: MikeWasson
description: Beschreibt die Verwendung von integrierter Windows-Authentifizierung in ASP.NET Web-API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="integrated-windows-authentication"></a>Integrierte Windows-Authentifizierung
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Integrierte Windows-Authentifizierung kann Benutzer sich mit ihrer Windows-Anmeldeinformationen, die mit Kerberos oder NTLM anzumelden. Der Client sendet die Anmeldeinformationen in der Authorization-Header. Windows-Authentifizierung ist für Intranetumgebungen am besten geeignet. Weitere Informationen finden Sie unter [Windows-Authentifizierung](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).

| Vorteile | Nachteile |
| --- | --- |
| -In IIS erstellt. -Die Anmeldeinformationen des Benutzers in der Anforderung sendet keine. – Wenn der Clientcomputer mit der Domäne (z. B. Intranetanwendung) gehört, muss der Benutzer nicht um Anmeldeinformationen einzugeben. | -Für Internetanwendungen empfohlen nicht. -Unterstützung für Kerberos oder NTLM in der Client erfordert. -Client muss in Active Directory-Domäne sein. |

> [!NOTE]
> Wenn Ihre Anwendung in Azure gehostet wird, und einer lokalen Active Directory-Domäne haben, sollten Sie Verbund Ihrer lokalen AD mit Azure Active Directory. Auf diese Weise können Benutzer sich mit ihren lokalen Anmeldeinformationen anmelden, aber die Authentifizierung erfolgt durch Azure AD. Weitere Informationen finden Sie unter [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).


Wählen Sie zum Erstellen einer Anwendung, die integrierte Windows-Authentifizierung verwendet, die "Intranetanwendung"-Vorlage im Assistenten für MVC 4. Diese Projektvorlage fügt die folgende Einstellung in der Datei "Web.config":

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

Auf der Clientseite integrierte Windows-Authentifizierung funktioniert, mit jedem Browser, die unterstützt die [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) -Authentifizierungsschema, das meisten wichtigen Browser enthält. Für .NET-Clientanwendungen die **"HttpClient"** Klasse unterstützt die Windows-Authentifizierung:

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

Windows-Authentifizierung ist anfällig für standortübergreifenden Angriffen Request (Forgery). Finden Sie unter [verhindern von Angriffen Cross-Site Request (Forgery)](preventing-cross-site-request-forgery-csrf-attacks.md).
