---
uid: web-api/overview/security/basic-authentication
title: Standardauthentifizierung in ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: Beschreibt die Verwendung der Standardauthentifizierung in ASP.NET Web-API.
ms.author: aspnetcontent
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 035baec7c56c0bf6eaacd26ea5192faf2ed6e932
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829586"
---
<a name="basic-authentication-in-aspnet-web-api"></a>Standardauthentifizierung in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Die Standardauthentifizierung ist in definiert [RFC 2617, HTTP-Authentifizierung: Standard- und Hashwertauthentifizierung für den Zugriff](http://www.ietf.org/rfc/rfc2617.txt).

Nachteile

- In der Anforderung werden die Anmeldeinformationen des Benutzers gesendet.
- Anmeldeinformationen werden als Klartext gesendet.
- Anmeldeinformationen werden bei jeder Anforderung gesendet.
- So melden Sie sich, mit Ausnahme der durch die Browsersitzung beenden.
- Anfällig für websiteübergreifende anforderungsfälschung (CSRF); erfordert, dass Anti-CSRF-Measures.

Vorteile

- Internet-Standard.
- Von allen wichtigen Browsern unterstützt.
- Relativ einfaches Protokoll.

Die Standardauthentifizierung funktioniert wie folgt aus:

1. Wenn eine Anforderung eine Authentifizierung erfordert, gibt der Server 401 (nicht autorisiert) zurück. Die Antwort enthält einen WWW-Authenticate-Header, der angibt, dass der Server die Standardauthentifizierung unterstützt.
2. Der Client sendet eine weitere Anforderung, mit die Anmeldeinformationen des Clients in der Authorization-Header. Die Anmeldeinformationen werden als "Name: Password" als base64-codierte Zeichenfolge formatiert. Die Anmeldeinformationen werden nicht verschlüsselt.

Standardauthentifizierung erfolgt im Rahmen einer "Realm". Der Server enthält den Namen des Bereichs im WWW-Authenticate-Header. Die Anmeldeinformationen des Benutzers sind innerhalb dieses Bereichs gültig. Der genauen Geltungsbereich ein Bereich wird durch den Server definiert. Beispielsweise können Sie mehrere Bereiche in der Reihenfolge nach Partition Ressourcen definieren.

![](basic-authentication/_static/image1.png)

Da die Anmeldeinformationen unverschlüsselt gesendet werden, ist die Standardauthentifizierung nur sichere über HTTPS. Finden Sie unter [arbeiten mit SSL in Web-API](working-with-ssl-in-web-api.md).

Standardauthentifizierung ist auch anfällig für CSRF-Angriffe. Nachdem der Benutzer Anmeldeinformationen eingegeben hat, sendet der Browser automatisch diese bei nachfolgenden Anforderungen der gleichen Domäne an, für die Dauer der Sitzung. Dies schließt die AJAX-Anforderungen. Finden Sie unter [verhindern von Angriffen Cross-Site Request Forgery (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Standardauthentifizierung mit IIS

IIS unterstützt die Standardauthentifizierung, es gibt jedoch ein Nachteil: der Benutzer anhand ihrer Windows-Anmeldeinformationen authentifiziert wird. Das bedeutet, dass der Benutzer ein Konto für die Domäne des Servers verfügen muss. Für eine öffentliche Website möchten Sie in der Regel ein ASP.NET-Mitgliedschaftsanbieter authentifiziert.

Um grundlegende Authentifizierung mit IIS zu aktivieren, legen Sie den Authentifizierungsmodus auf "Windows" in "Web.config" von Ihrem ASP.NET-Projekt aus:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

In diesem Modus verwendet IIS Windows-Anmeldeinformationen zum Authentifizieren. Darüber hinaus müssen Sie die grundlegenden Authentifizierung in IIS aktivieren. Im IIS-Manager, wechseln Sie zur Ansicht "Features", wählen Sie die Authentifizierung und aktivieren Sie der Standardauthentifizierung.

![](basic-authentication/_static/image2.png)

Fügen Sie in Ihrem Web-API-Projekt, das `[Authorize]` -Attribut für alle Controlleraktionen, die eine Authentifizierung benötigen.

Ein Client authentifiziert sich durch Festlegen des Authorization-Headers in der Anforderung. Browser-Clients führen Sie diesen Schritt automatisch. Nichtsuchdienst Clients müssen die Header festgelegt.

## <a name="basic-authentication-with-custom-membership"></a>Standardauthentifizierung mit benutzerdefinierten Mitgliedschaftsanbieter

Wie bereits erwähnt, verwendet die Standardauthentifizierung verwendet, die in IIS integrierte Windows-Anmeldeinformationen an. Das bedeutet, dass Sie Konten für die Benutzer auf dem Hostserver erstellen müssen. Aber für eine internetanwendung, Benutzerkonten in der Regel in einer externen Datenbank gespeichert sind.

Im folgenden code, wie ein HTTP-Modul, das Standardauthentifizierung ausführt. Sie können ganz einfach in ein ASP.NET-Mitgliedschaftsanbieter einbinden, durch Ersetzen der `CheckPassword` -Methode, die eine dummy-Methode in diesem Beispiel ist.

In Web-API 2, sollten Sie das Schreiben einer [Authentifizierungsfilter](authentication-filters.md) oder [OWIN-Middleware](../../../aspnet/overview/owin-and-katana/index.md), anstatt ein HTTP-Modul.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Um das HTTP-Modul zu aktivieren, fügen Sie Folgendes in die Datei "web.config" in der **"System.Webserver"** Abschnitt:

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Ersetzen Sie "YourAssemblyName", durch den Namen der Assembly (nicht einschließlich der Erweiterungs "Dll").

Sie sollten die anderen Authentifizierungsschemas, z. B. Forms oder Windows-Authentifizierung deaktivieren.
