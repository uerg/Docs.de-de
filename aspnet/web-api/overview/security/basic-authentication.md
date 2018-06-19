---
uid: web-api/overview/security/basic-authentication
title: Basic-Authentifizierung in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: Beschreibt die Verwendung der Standardauthentifizierung in ASP.NET Web-API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4b8e6410668b2db289488bb4b6cd26d881e70f4c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508129"
---
<a name="basic-authentication-in-aspnet-web-api"></a>Basic-Authentifizierung in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Die Standardauthentifizierung ist definiert [RFC 2617, HTTP-Authentifizierung: Standard- und Digestauthentifizierung für den Zugriff](http://www.ietf.org/rfc/rfc2617.txt).

Nachteile

- Benutzeranmeldeinformationen werden in der Anforderung gesendet.
- Anmeldeinformationen werden unverschlüsselt gesendet.
- Anmeldeinformationen werden mit jeder Anforderung gesendet.
- Keine Möglichkeit zum Abmelden außer durch die Browsersitzung beenden.
- Anfällig für websiteübergreifende anforderungsfälschung (FORGERY); erfordert die Anti-CSRF-Measures.

Vorteile

- Internet-Standard.
- Von allen wichtigen Browsern unterstützt.
- Relativ einfache Protokoll.

Die Standardauthentifizierung funktioniert wie folgt:

1. Wenn eine Anforderung eine Authentifizierung erfordert, gibt der Server 401 (nicht autorisiert) zurück. Die Antwort enthält einen WWW-Authenticate-Header, der angibt, dass der Server die Standardauthentifizierung unterstützt.
2. Der Client sendet eine andere Anforderung, mit den Anmeldeinformationen des Clients in der Authorization-Header. Die Anmeldeinformationen werden als "Name: Password" als base64-codierte Zeichenfolge formatiert. Die Anmeldeinformationen werden nicht verschlüsselt.

Standardauthentifizierung erfolgt im Kontext von einem "Bereich". Der Server enthält den Namen des Bereichs im WWW-Authenticate-Header. Die Anmeldeinformationen des Benutzers, die innerhalb dieses Bereichs gültig sind. Der genauen Rahmen eines Bereichs wird durch den Server definiert. Beispielsweise können Sie mehrere Bereiche in der Reihenfolge nach Partition Ressourcen definieren.

![](basic-authentication/_static/image1.png)

Da die Anmeldeinformationen unverschlüsselt gesendet werden, ist die Standardauthentifizierung nur über HTTPS sicher. Finden Sie unter [arbeiten mit SSL in Web-API](working-with-ssl-in-web-api.md).

Die Standardauthentifizierung ist auch CSRF-Angriffe anfällig. Nachdem der Benutzer Anmeldeinformationen eingegeben hat, sendet der Browser automatisch diese bei nachfolgenden Anforderungen der gleichen Domäne für die Dauer der Sitzung. Dies schließt die AJAX-Anforderungen. Finden Sie unter [verhindern von Angriffen Cross-Site Request (Forgery)](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Standardauthentifizierung mit IIS

IIS unterstützt die Standardauthentifizierung, es gibt jedoch ein Nachteil: der Benutzer anhand ihrer Windows-Anmeldeinformationen authentifiziert wird. Das bedeutet, dass der Benutzer ein Konto für die Domäne des Servers verfügen muss. Für eine öffentliche Website sollten Sie in der Regel eine ASP.NET-Mitgliedschaftsanbieter authentifiziert.

Um Standardauthentifizierung mit IIS zu aktivieren, legen Sie den Authentifizierungsmodus auf "Windows" in der Datei "Web.config" des ASP.NET-Projekts, aus:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

In diesem Modus verwendet IIS Windows-Anmeldeinformationen zum Authentifizieren. Darüber hinaus müssen Sie die Standardauthentifizierung in IIS aktivieren. Im IIS-Manager wechseln Sie zur Ansicht "Features", wählen Sie die Authentifizierung und aktivieren Sie der Standardauthentifizierung.

![](basic-authentication/_static/image2.png)

Im Web-API-Projekt hinzufügen der `[Authorize]` Attribut für jede Controlleraktionen, die Authentifizierung benötigen.

Ein Client authentifiziert sich durch Festlegen des Authorization-Headers in der Anforderung. Browser-Clients führen diesen Schritt automatisch. Nonbrowser-Clients müssen auf die Header festgelegt werden.

## <a name="basic-authentication-with-custom-membership"></a>Standardauthentifizierung mit benutzerdefinierten Mitgliedschaft

Wie bereits erwähnt, verwendet die Standardauthentifizierung in IIS integrierte Windows-Anmeldeinformationen an. Das bedeutet, dass Sie Konten für die Benutzer auf dem Hostserver erstellen müssen. Aber für eine internetanwendung Benutzerkonten in der Regel in einer externen Datenbank gespeichert sind.

Der folgende code, wie ein HTTP-Modul, das Standardauthentifizierung ausführt. Sie können problemlos in eine ASP.NET-Mitgliedschaftsanbieter anschließen, durch Ersetzen der `CheckPassword` Methode, die eine dummy-Methode in diesem Beispiel ist.

In der Web-API 2, sollten Sie das Schreiben einer [Authentifizierungsfilter](authentication-filters.md) oder [OWIN-Middleware](../../../aspnet/overview/owin-and-katana/index.md), statt ein HTTP-Modul.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Um das HTTP-Modul zu aktivieren, fügen Sie die folgenden auf die Datei "Web.config" in der **"System.Webserver"** Abschnitt:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Ersetzen Sie "YourAssemblyName" durch den Namen der Assembly (nicht einschließlich der Erweiterungs "Dll").

Deaktivieren Sie alle anderen Authentifizierungsschemas, z. B. Forms oder Windows-auth.
