---
title: Verhindern von Angriffen von Open Umleitung in einer ASP.NET Core-app | Microsoft Docs
author: ardalis
description: Zeigt, wie open umleitungs-Angriffe gegen eine ASP.NET Core-app zu verhindern
keywords: ASP.NET Core, Sicherheit, Open Umleitung Angriff
ms.author: riande
manager: wpickett
ms.date: 07/07/2017
ms.topic: article
ms.assetid: 4604e563-e91a-4ecd-b7ed-00b3f1eee2b5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/preventing-open-redirects
ms.openlocfilehash: d5419aa149b3201ecbc93f4f17ae5928f1d39b1d
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a>Verhindern von Angriffen von Open Umleitung in einer ASP.NET Core-app

Eine Web-app, die an eine URL, die über die Anforderung angegeben wird umleitet, wie z. B. die Abfragezeichenfolge oder Formular Daten möglicherweise manipuliert werden können Benutzer an eine externe, böswillige URL umleiten. Diese Manipulation wird einen öffnen-Redirect-Angriff bezeichnet.

Wenn Sie die Anwendungslogik zu einer angegebenen URL umleitet, müssen Sie sicherstellen, dass die umleitungs-URL nicht manipuliert wurde. ASP.NET Core verfügt über integrierte Funktionalität zu apps öffnen Umleitung (auch bekannt als open-Umleitung) Angriffen zu schützen.

## <a name="what-is-an-open-redirect-attack"></a>Was ist ein Angriff öffnen Umleitung?

Webanwendungen Umleiten von Benutzern häufig zu einer Anmeldeseite beim Zugriff auf Ressourcen, die eine Authentifizierung erfordern. Die Umleitung Typlically umfasst eine `returnUrl` Querystring-Parameter, damit der Benutzer an die ursprünglich angeforderte URL zurückgegeben werden kann, nachdem sie erfolgreich angemeldet haben. Nachdem der Benutzer authentifiziert hat, wird er an die URL umgeleitet, die sie ursprünglich angefordert hatten.

Da der Ziel-URL in die Abfragezeichenfolge der Anforderung angegeben ist, kann ein böswilliger Benutzer Querystring manipulieren. Ein manipuliertes Querystring kann die Website zum Umleiten des Benutzers auf eine externe, schädlichen Website ermöglichen. Diese Technik ist einen open Umleitung (oder der Umleitung)-Angriff bezeichnet.

### <a name="an-example-attack"></a>Ein Beispiel-Angriff

Ein böswilliger Benutzer könnte einen Angriff den böswilliger Benutzerzugriff auf eines Benutzers Anmeldeinformationen oder vertrauliche Informationen in Ihrer app zu ermöglichen entwickeln. Um das Risiko eines Angriffs zu beginnen, ermöglichen sie den Benutzer einen Link zur Anmeldeseite für Ihre Website, und klicken Sie auf eine `returnUrl` Querystring-Wert zur URL hinzugefügt. Z. B. die [NerdDinner.com](http://nerddinner.com) beispielanwendung (für ASP.NET MVC geschrieben) schließt solche eine Anmeldeseite hier: ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``. Der Angriff führt dann folgende Schritte aus:

1. Benutzer klickt auf einen Link zur ``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn`` (Beachten Sie, dass zweiten URL ist Nerddi**n**mputerkonto, nicht Nerddi**nn**mputerkonto).
2. Der Benutzer anmeldet erfolgreich.
3. Der Benutzer umgeleitet wird (durch den Standort) ``http://nerddiner.com/Account/LogOn`` (bösartige Website, die echte Website aussieht).
4. Der Benutzer wieder anmeldet (Vergabe böswillige Standort ihrer Anmeldeinformationen) und wieder an die wirkliche Website umgeleitet werden.

Der Benutzer wird wahrscheinlich glauben, dass ihre ersten Versuch, melden Sie sich Fehler, und die zweiten Ausdruck erfolgreich war. Wahrscheinlich müssen Sie unabhängig von deren bleiben ihre Anmeldeinformationen beschädigt wurden.

![Open-Angriff-Weiterleitung](preventing-open-redirects/_static/open-redirection-attack-process.png)

Geben Sie zusätzlich zu den Anmeldungsseiten einige Websites Umleitung Seiten oder Endpunkte. Stellen Sie sich vor Ihrer app hat eine Seite mit einer geöffneten Umleitung ``/Home/Redirect``. Ein Angreifer könnte erstellen, z. B. einen Link in einer e-Mail, die auf geht ``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``. Ein normaler Benutzer wird an der URL suchen und feststellen, dass er mit dem Standortnamen Ihres beginnt. Sie werden als vertrauenswürdig festlegen, die, auf den Link klicken. Öffnen umleiten den Benutzer klicken Sie dann auf die Phishingwebsite identisch mit Ihren Instanzen-aussieht, senden und der Benutzer wahrscheinlich würden, was sie meinen Anmeldung ist Ihr Standort.

## <a name="protecting-against-open-redirect-attacks"></a>Schutz vor Angriffen öffnen umleiten

Wenn Sie Webanwendungen zu entwickeln, behandeln Sie alle Benutzer bereitgestellten Daten als nicht vertrauenswürdig. Wenn die Anwendung Funktionen, die den Benutzer basierend auf den Inhalt der URL umleitet verfügt, stellen Sie sicher, dass solche leitet nur lokal ausgeführt werden, innerhalb Ihrer app (oder eine bekannte URL, aber keine URL, die in der Abfragezeichenfolge angegeben werden kann).

### <a name="localredirect"></a>LocalRedirect

Verwenden der ``LocalRedirect`` Hilfsmethode, die von der Basisklasse `Controller` Klasse:

```
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

``LocalRedirect``Wenn eine nicht lokale-URL angegeben ist, wird eine Ausnahme ausgelöst. Andernfalls verhält sich ebenso wie die ``Redirect`` Methode.

### <a name="islocalurl"></a>IsLocalUrl

Verwenden der [IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_) Methode zum Testen von URLs vor der Umleitung:

Das folgende Beispiel zeigt, wie zu überprüfen, ob vor der Umleitung für eine URL lokal ist.

```
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

Die `IsLocalUrl` Methode verhindert, dass Benutzer versehentlich an eine bösartige Website umgeleitet wird. Sie können die Details der URL protokollieren, die bereitgestellt wurde, wird eine nicht lokale-URL in einer Situation bereitgestellt, in dem Sie eine lokale URL erwartet. Protokollierung Umleitung können URLs bei der Diagnose von Umleitung Angriffe.
