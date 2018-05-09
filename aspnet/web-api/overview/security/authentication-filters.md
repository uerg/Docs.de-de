---
uid: web-api/overview/security/authentication-filters
title: Authentifizierungsfilter in der ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Ein Authentifizierungsfilter ist eine Komponente, die eine HTTP-Anforderung authentifiziert. Web-API 2 und MVC 5 unterstützen sowohl Authentifizierungsfilter, aber sie unterscheiden sich geringfügig...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/25/2014
ms.topic: article
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: 16e451f52799625983368bc938091eff47019b52
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2018
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>Authentifizierungsfilter in der ASP.NET Web API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Ein Authentifizierungsfilter ist eine Komponente, die eine HTTP-Anforderung authentifiziert. Web-API 2 und MVC 5 unterstützen sowohl Authentifizierungsfilter, aber sie unterscheiden sich geringfügig, meist in den Benennungskonventionen für die Filter-Schnittstelle. Dieses Thema beschreibt die Web-API-Authentifizierungsfilter.


Authentifizierungsfilter können Sie ein anderes Authentifizierungsschema für einzelne Controllern oder Aktionen festlegen. Auf diese Weise kann Ihre app unterschiedliche Authentifizierungsmechanismen für verschiedene HTTP-Ressourcen unterstützen.

In diesem Artikel zeige ich Code aus der [Standardauthentifizierung](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) -Beispiel unter [http://aspnet.codeplex.com](http://aspnet.codeplex.com). Das Beispiel zeigt einen Authentifizierungsfilter, der dem HTTP-Zugriff standardauthentifizierungsschema (RFC 2617) implementiert. Der Filter wird in einer Klasse mit dem Namen implementiert `IdentityBasicAuthenticationAttribute`. Ich zeigen nicht der gesamte Code aus dem Beispiel nur die Teile an, die veranschaulichen, einen Authentifizierungsfilter zu schreiben.

## <a name="setting-an-authentication-filter"></a>Das Festlegen eines Filters für die Authentifizierung

Wie andere Filter auch kann Authentifizierungsfilter pro Controller angewendet, die pro-Aktion oder die Global auf alle Web-API-Controller.

Um einen Authentifizierungsfilter mit einem Controller anzuwenden, ergänzen Sie die Controllerklasse mit dem Filterattribut. Im folgenden code wird die `[IdentityBasicAuthentication]` Filter für eine Controllerklasse, die Standardauthentifizierung für alle Aktionen für den Controller ermöglicht.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Um den Filter auf eine Aktion anwenden, ergänzen Sie die Aktion mit dem Filter aus. Im folgenden code wird die `[IdentityBasicAuthentication]` Filter auf des Controllers `Post` Methode.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Um den Filter auf alle Web-API-Controller anzuwenden, fügen sie **GlobalConfiguration.Filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementieren eine Web-API-Authentifizierungsfilter

In der Web-API, Authentifizierungsfilter implementieren die [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) Schnittstelle. Sie sollten auch erben von **System.Attribute**, um als Attribute angewendet werden.

Die **IAuthenticationFilter** Schnittstelle verfügt über zwei Methoden:

- **AuthenticateAsync** authentifiziert die Anforderung von Anmeldeinformationen in der Anforderung überprüft, falls vorhanden.
- **ChallengeAsync** der HTTP-Antwort, bei Bedarf eine authentifizierungsaufforderung hinzugefügt.

Diese Methoden entsprechen den Authentifizierungsablauf in definierten [RFC 2612](http://tools.ietf.org/html/rfc2616) und [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. Der Client sendet die Anmeldeinformationen in der Authorization-Header. Dies geschieht normalerweise, nachdem der Client eine 401 (nicht autorisiert)-Antwort vom Server empfangen. Allerdings kann ein Client Anmeldeinformationen mit keiner Anforderung senden, nicht nur nach 401 abrufen.
2. Wenn der Server die Anmeldeinformationen nicht akzeptiert, wird die Antwort ein 401 (nicht autorisiert) zurückgegeben. Die Antwort enthält einen Www-Authenticate-Header, der eine oder mehrere Herausforderungen enthält. Jede Abfrage gibt ein anderes Authentifizierungsschema vom Server erkannt.

Der Server kann auch eine anonyme Anforderung 401 zurückgeben. Tatsächlich ist, die in der Regel wie der Authentifizierungsprozess initiiert wird:

1. Der Client sendet eine anonyme Anforderung an.
2. Der Server gibt 401.
3. Die Clients sendet die Anforderung mit Anmeldeinformationen erneut.

Dieser Datenfluss enthält sowohl *Authentifizierung* und *Autorisierung* Schritte.

- Authentifizierung garantiert die Identität des Clients.
- Bestimmt die Autorisierung, ob der Client eine bestimmte Ressource zugreifen kann.

Behandeln Sie Authentifizierungsfilter in Web-API Authentifizierung, aber keine Autorisierung. Autorisierung sollte durch einen Autorisierungsfilter oder innerhalb der Controlleraktion erfolgen.

So sieht der Ablauf in der Web-API 2-Pipeline:

1. Vor dem Aufrufen einer Aktion, wird die Web-API eine Liste der Authentifizierungsfilter für diese Aktion erstellt. Dies schließt die Filter mit der Aktion Bereich, den Controller Bereich und globalen Bereich.
2. Web-API-Aufrufe **AuthenticateAsync** auf jedem Filter in der Liste. Jeder Filter kann die Anmeldeinformationen in der Anforderung überprüfen. Wenn ein beliebiger anzuwendender Filter Anmeldeinformationen erfolgreich überprüft hat, erstellt der Filter eine **IPrincipal** und fügt ihn der Anforderung. Ein Filter kann auch zu diesem Zeitpunkt einen Fehler ausgelöst werden. Wenn dies der Fall ist, wird der Rest der Pipeline nicht ausgeführt.
3. Vorausgesetzt, dass kein Fehler vorliegt, fließt die Anforderung durch den Rest der Pipeline.
4. Zum Schluss Web-API ruft alle Authentifizierungsfilter **ChallengeAsync** Methode. Filter mit dieser Methode können Sie die Antwort eine Aufforderung hinzuzufügen, bei Bedarf. In der Regel (aber nicht immer) würde, die als Antwort auf Fehler 401 geschehen.

Die folgenden Diagramme enthalten zwei möglichen Fälle. In der ersten der Authentifizierungsfilter erfolgreich die Anforderung authentifiziert, Autorisierungsfilter wird die Anforderung autorisiert und Controlleraktion gibt 200 (OK) zurück.

![](authentication-filters/_static/image1.png)

Im zweiten Beispiel wird der Authentifizierungsfilter authentifiziert die Anforderung, der Autorisierungsfilter gibt jedoch 401 (nicht autorisiert). In diesem Fall wird der Controlleraktion nicht aufgerufen. Der Authentifizierungsfilter hinzugefügt der Antwort einen Www-Authenticate-Header.

![](authentication-filters/_static/image2.png)

Andere Kombinationen sind mögliche&mdash;z. B. wenn Controlleraktion anonyme Anforderungen zulässt, müssen Sie möglicherweise einen Authentifizierungsfilter, aber es liegt keine Autorisierung.

## <a name="implementing-the-authenticateasync-method"></a>Implementieren der AuthenticateAsync-Methode

Die **AuthenticateAsync** Methode versucht, die Anforderung zu authentifizieren. Hier wird die Signatur der Methode:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

Die **AuthenticateAsync** Methode muss einen der folgenden Schritte ausführen:

1. Nothing (ohne-Op).
2. Erstellen einer **IPrincipal** und legen Sie es in der Anforderung.
3. Legen Sie ein Fehlerergebnis.

Option (1) bedeutet, dass die Anforderung, sind keine Anmeldeinformationen, die der Filter versteht. Option (2) bedeutet, dass der Filter wird die Anforderung erfolgreich authentifiziert. Option (3) bedeutet, dass die Anforderung wies ungültige Anmeldeinformationen (z. B. das falsche Kennwort), die eine Fehlerantwort auslöst.

Hier ist eine allgemeine Gliederung für die Implementierung **AuthenticateAsync**.

1. Suchen Sie nach Anmeldeinformationen in der Anforderung.
2. Wenn keine Anmeldeinformationen vorhanden sind, werden Sie keine Aktionen ausgeführt und zurückgegeben Sie (ohne-Op).
3. Wenn Anmeldeinformationen vorhanden sind, aber das Authentifizierungsschema der Filter nicht erkennt, wird nichts Unternehmen, und geben Sie (ohne-Op) zurück. Ein weiterer Filter in der Pipeline kann das Schema verstehen.
4. Wenn Anmeldeinformationen, die der Filter versteht vorhanden sind, versucht, diese zu authentifizieren.
5. Wenn die Anmeldeinformationen gefälscht sind, durch Festlegen 401 zurückgegeben `context.ErrorResult`.
6. Wenn die Anmeldeinformationen gültig sind, erstellen Sie eine **IPrincipal** und `context.Principal`.

Der folgende Code zeigt die **AuthenticateAsync** Methode aus der [Standardauthentifizierung](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) Beispiel. Jeder Schritt geben Sie die Kommentare an. Der Code zeigt mehrere Typen von Fehler: ein Autorisierungsheader mit keine Anmeldeinformationen, die ungültige Anmeldeinformationen und die ungültige Benutzername/Kennwort.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Ein Fehlerergebnis festlegen

Wenn die Anmeldeinformationen ungültig sind, muss der Filter festgelegt `context.ErrorResult` auf eine **IHttpActionResult** , erstellt eine Fehlerantwort. Weitere Informationen zu **IHttpActionResult**, finden Sie unter [Aktionsergebnisse in Web-API 2](../getting-started-with-aspnet-web-api/action-results.md).

Die Standardauthentifizierung-Beispiel enthält eine `AuthenticationFailureResult` -Klasse, die für diesen Zweck geeignet ist.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementieren von ChallengeAsync

Der Zweck der **ChallengeAsync** Methode ist die Antwort authentifizierungsaufforderungen hinzu, bei Bedarf. Hier wird die Signatur der Methode:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Die Methode ist für alle Authentifizierungsfilter in der Anforderungspipeline aufgerufen.

Es ist wichtig zu wissen, dass **ChallengeAsync** heißt *vor* die HTTP-Antwort wird erstellt, und möglicherweise sogar, bevor Sie die Controller-Aktion ausgeführt wird. Wenn **ChallengeAsync** aufgerufen wird, `context.Result` enthält ein **IHttpActionResult**, der später zum Erstellen der HTTP-Antwort verwendet wird. Daher **ChallengeAsync** wird aufgerufen, Sie wissen nicht, Informationen über die HTTP-Antwort noch. Die **ChallengeAsync** Methode sollte ersetzt den ursprünglichen Wert des `context.Result` mit einem neuen **IHttpActionResult**. Dies **IHttpActionResult** müssen die ursprüngliche umschließen `context.Result`.

![](authentication-filters/_static/image3.png)

Die ursprüngliche rufen **IHttpActionResult** der *innere Ergebnis*, und der neuen **IHttpActionResult** der *äußeren Ergebnis*. Das äußere Ergebnis muss folgende Anforderungen erfüllen:

1. Rufen Sie die innere Ergebnis um die HTTP-Antwort zu erstellen.
2. Überprüfen Sie die Antwort ein.
3. Fügen Sie bei Bedarf in die Antwort eine authentifizierungsaufforderung hinzu.

Das folgende Beispiel stammt aus dem Beispiel die Standardauthentifizierung. Definiert eine **IHttpActionResult** für das äußere Ergebnis.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

Die `InnerResult` Eigenschaft enthält die innere **IHttpActionResult**. Die `Challenge` Eigenschaft stellt einen Www-Authentifizierungsheader. Beachten Sie, dass **ExecuteAsync** ruft zuerst `InnerResult.ExecuteAsync` zum Erstellen der HTTP-Antwort und fügt dann die Abfrage bei Bedarf.

Überprüfen Sie den Antwortcode vor dem Hinzufügen der Aufforderung an. Die meisten Authentifizierungsschemen nur hinzufügen, eine neue Herausforderung dar, wenn die Antwort 401, ist, wie hier gezeigt. Allerdings einige Authentifizierungsschemas eine Aufforderung eine erfolgreiche Antwort hinzufügen. Beispielsweise finden Sie unter [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Erhält die `AddChallengeOnUnauthorizedResult` Klasse, den tatsächlichen Code in **ChallengeAsync** ist einfach. Nur das Ergebnis zu erstellen und fügen Sie es auf `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Hinweis: Das Beispiel die Standardauthentifizierung abstrahiert diese Logik etwas, indem Sie es auf eine Erweiterungsmethode.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Kombinieren von Authentifizierungsfilter mit Authentifizierung auf Hostebene

"Hostebene Authentifizierung" ist die Authentifizierung, die vom Host (z. B. IIS) ausgeführt wird vor der Anforderung erreicht der Web-API-Framework.

Möglicherweise möchten häufig Hostebene-Authentifizierung für den Rest der Anwendung aktivieren, aber für die Web-API-Controller zu deaktivieren. Beispielsweise ist ein typisches Szenario Formularauthentifizierung auf der Hostebene aktivieren, aber tokenbasierter Authentifizierung für die Web-API verwenden.

Rufen Sie zum Deaktivieren der Hostebene Authentifizierung innerhalb der Web-API-Pipeline `config.SuppressHostPrincipal()` in Ihrer Konfiguration. Dies bewirkt, dass Web-API zum Entfernen der **IPrincipal** über jede Anforderung, die die Web-API-Pipeline eingibt. Effektiv, es &quot;un-authentifiziert&quot; der Anforderung.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[ASP.NET Web API-Sicherheitsfilter](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)
