---
uid: web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
title: Verhindern von Cross-Site Request Forgery (CSRF)-Angriffen in ASP.NET MVC
author: MikeWasson
description: Beschreibt die websiteübergreifende anforderungsfälschung (CSRF)-Angriffe und Anti-CSRF-Measures in ASP.NET Web MVC implementiert.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 81d46f14-8f48-4d8c-830d-cc8d594dc11b
msc.legacyurl: /web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks
msc.type: authoredcontent
ms.openlocfilehash: c88975d1c205e9d0733bfb4c710b92bc8fdaaa7a
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48911496"
---
<a name="preventing-cross-site-request-forgery-csrf-attacks-in-aspnet-mvc-application"></a>Verhindern von Cross-Site Request Forgery (CSRF) Angriffen in ASP.NET MVC-Anwendung
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Cross-Site Request Forgery (CSRF) ist ein Angriff, in dem eine schädliche Website sendet eine Anforderung an einer anfälligen Website, in denen der Benutzer zurzeit angemeldet ist

Hier ist ein Beispiel eines CSRF-Angriffs aus:

1. Ein Benutzer meldet sich bei `www.example.com` mit Formularauthentifizierung.
2. Der Server authentifiziert den Benutzer. Die Antwort vom Server enthält ein Authentifizierungscookie.
3. Der Benutzer, ohne Abmeldung, eine schädliche Website besucht. Diese schädliche Websites enthält das folgende HTML-Format an: 

    [!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample1.html)]

    Beachten Sie, dass die Formularaktion an den Standort anfällig für nicht auf die bösartige Website sendet. Dies ist der Teil von "Cross-Site" für CSRF.
4. Der Benutzer klickt auf die Schaltfläche "Senden". Der Browser enthält das Cookie für die Authentifizierung mit der Anforderung.
5. Die Anforderung ausgeführt wird, auf dem Server mit dem Kontext des Benutzers Authentifizierung und Aktionen möglich, die ein authentifizierter Benutzer ausführen darf.

Obwohl in diesem Beispiel wird den Benutzer auf die Schaltfläche "Format" erforderlich sind, könnten auch einfach ein Skript ausführen, die das Formular automatisch übermittelt die schädliche Seite. Darüber hinaus verhindert mithilfe von SSL ein CSRF-Angriffs nicht, da die bösartige Website eine "https://"-Anforderung senden kann.

In der Regel sind die CSRF-Angriffe möglich vor Websites, die Cookies für die Authentifizierung zu verwenden, da Browser alle relevanten Cookies an die Ziel-Website senden. CSRF-Angriffe sind jedoch nicht beschränkt auf Cookies ausnutzen. Z. B. Basis-und Digestauthentifizierung sind auch anfällig. Nachdem ein Benutzer meldet sich mit Standard-oder Digestauthentifizierung. der Browser sendet automatisch die Anmeldeinformationen an, bis die Sitzung beendet.

## <a name="anti-forgery-tokens"></a>Fälschungssicherheitstoken

Um CSRF-Angriffe zu vermeiden, verwendet ASP.NET MVC fälschungssicherheitstoken, so genannte *Überprüfung Token anfordern*.

1. Der Client fordert eine HTML-Seite, die ein Formular enthält.
2. Der Server umfasst zwei Token in der Antwort. Ein Token wird als Cookie gesendet. Die andere befindet sich in einem ausgeblendeten Formularfeld. Die Token werden nach dem Zufallsprinzip generiert, so, dass ein Angreifer die Werte nicht wissen.
3. Wenn der Client das Formular sendet, müssen sie beide Token an den Server senden. Der Client sendet das cookietoken als ein Cookie, und sendet das formulartoken innerhalb von Daten aus dem Formular. (Ein Browserclient Fall automatisch, wenn der Benutzer das Formular übermittelt.)
4. Wenn eine Anforderung nicht beide Token enthalten ist, lässt der Server die Anforderung an.

Hier ist ein Beispiel für ein HTML-Formular mit einem ausgeblendeten Feld-Token:

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample2.html)]

Fälschungssicherheitstoken funktionieren, da die Token des Benutzers, aufgrund von Richtlinien für denselben Ursprung der schädliche Seite nicht gelesen werden kann. ([Richtlinien für denselben Ursprung](http://www.w3.org/Security/wiki/Same_Origin_Policy) verhindern, dass Dokumente auf zwei verschiedenen Standorten aus den Zugriff auf die Inhalte des jeweils anderen gehostet. Damit die oben aufgeführten Beispiel kann die schädliche Seite Anforderungen an "example.com" senden, aber er die Antwort kann nicht gelesen werden.)

Um CSRF-Angriffe zu verhindern, verwenden Sie fälschungssicherheitstoken Beliebiges Authentifizierungsprotokoll sendet, in dem der Browser automatisch Anmeldeinformationen, nachdem der Benutzer anmeldet. Dies umfasst cookiebasierte Authentifizierung-Protokolle, z. B. Formularauthentifizierung, als auch Protokolle wie Basis-und Digestauthentifizierung.

Sie sollten fälschungssicherheitstoken für nonsafe Methoden (POST, PUT, DELETE) erfordern. Stellen Sie außerdem sicher, dass sichere Methoden (GET, HEAD) nicht keine Nebeneffekte haben. Darüber hinaus, wenn Sie Unterstützung für domänenübergreifende, z. B. CORS oder JSONP, aktivieren sind sogar sichere Methoden wie GET potenziell anfällig für CSRF-Angriffe, denen der Angreifer möglicherweise sensible Daten zu lesen.

## <a name="anti-forgery-tokens-in-aspnet-mvc"></a>Fälschungssicherheitstoken in ASP.NET MVC

Verwenden Sie zum Hinzufügen der fälschungssicherheitstoken zu einer Razor Page der **HtmlHelper.AntiForgeryToken** Hilfsmethode:

[!code-cshtml[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample3.cshtml)]

Diese Methode fügt das verborgene Feld hinzu und legt auch fest, das cookietoken.

## <a name="anti-csrf-and-ajax"></a>Anti-CSRF und AJAX

Das formulartoken kann für AJAX-Anforderungen ein Problem sein, da eine AJAX-Anforderung, JSON-Daten nicht HTML-Formulardaten senden kann. Eine Lösung ist, um die Token in einem benutzerdefinierten HTTP-Header zu senden. Der folgende Code verwendet Razor-Syntax zum Generieren der Token und fügt dann die Token auf ein AJAX-Anforderung. Die Token werden durch den Aufruf auf dem Server generierte **AntiForgery.GetTokens**.

[!code-html[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample4.html)]

Wenn Sie die Anforderung verarbeiten, extrahieren Sie die Token aus dem Anforderungsheader. Rufen Sie dann die **AntiForgery.Validate** Methode zum Überprüfen der Token. Die **überprüfen** Methode löst eine Ausnahme aus, wenn die Token nicht gültig sind.

[!code-csharp[Main](preventing-cross-site-request-forgery-csrf-attacks/samples/sample5.cs)]
