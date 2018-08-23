---
uid: web-api/overview/security/authentication-filters
title: Authentifizierungsfilter in ASP.NET-Web-API 2 | Microsoft-Dokumentation
author: MikeWasson
description: Ein Authentifizierungsfilter ist eine Komponente, die eine HTTP-Anforderung authentifiziert. Web-API 2 und MVC 5 unterstützen beide Authentifizierungsfilter, aber sie unterscheiden sich geringfügig...
ms.author: riande
ms.date: 09/25/2014
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: a14facad4cbd0f9be1ff7bde2667f61ec8cc2a14
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838412"
---
<a name="authentication-filters-in-aspnet-web-api-2"></a>Authentifizierungsfilter in ASP.NET-Web-API 2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> Ein Authentifizierungsfilter ist eine Komponente, die eine HTTP-Anforderung authentifiziert. Web-API 2 und MVC 5 unterstützen beide Authentifizierungsfilter, aber sie unterscheiden sich geringfügig, vor allem in den Benennungskonventionen für die filterschnittstelle. Dieses Thema beschreibt die Web-API-Authentifizierungsfilter.


Authentifizierungsfilter können Sie ein anderes Authentifizierungsschema für den individuellen Controllern oder Aktionen festlegen. Auf diese Weise kann Ihre app verschiedene Authentifizierungsmechanismen für unterschiedliche HTTP-Ressourcen unterstützen.

In diesem Artikel zeige ich, Code aus der [Standardauthentifizierung](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) auf [ http://aspnet.codeplex.com ](http://aspnet.codeplex.com). Das Beispiel zeigt einen Authentifizierungsfilter, der das HTTP-Standardauthentifizierung Access-Schema (RFC 2617) implementiert. Der Filter wird in einer Klasse, die mit dem Namen implementiert `IdentityBasicAuthenticationAttribute`. Ich nicht der gesamte Code aus dem Beispiel nur die Bestandteile angezeigt, die veranschaulichen, wie Sie einen Authentifizierungsfilter zu schreiben.

## <a name="setting-an-authentication-filter"></a>Einen Authentifizierungsfilter festlegen

Wie andere Filter auch können die Authentifizierungsfilter angewendeten pro Controller, Aktion oder global auf alle Web-API-Controller sein.

Um einen Authentifizierungsfilter mit einem Controller anzuwenden, ergänzen Sie die Controller-Klasse mit dem Filterattribut. Im folgenden code wird die `[IdentityBasicAuthentication]` Filter für eine Controller-Klasse, die Standardauthentifizierung für alle Aktionen des Controllers ermöglicht.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Um den Filter auf eine Aktion anwenden, ergänzen Sie die Aktion, mit dem Filter. Im folgenden code wird die `[IdentityBasicAuthentication]` Filter für des Controllers `Post` Methode.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Um den Filter auf alle Web-API-Controller anzuwenden, hinzufügen zu **GlobalConfiguration.Filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementieren eine Web-API-Authentifizierungsfilter

In Web-API, Authentifizierungsfilter implementieren die [System.Web.Http.Filters.IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) Schnittstelle. Sie sollten auch eine Vererbung von **System.Attribute**, um als Attribute angewendet werden.

Die **IAuthenticationFilter** Schnittstelle verfügt über zwei Methoden:

- **AuthenticateAsync** authentifiziert die Anforderung von Anmeldeinformationen in der Anforderung überprüft, falls vorhanden.
- **"Challengeasync"** der HTTP-Antwort bei Bedarf eine authentifizierungsaufforderung hinzugefügt.

Diese Methoden entsprechen den Authentifizierungsablauf in definierten [RFC 2612](http://tools.ietf.org/html/rfc2616) und [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. Der Client sendet die Anmeldeinformationen in der Authorization-Header. Dies geschieht normalerweise, nachdem der Client eine 401-Antwort (nicht autorisiert) vom Server empfangen. Allerdings kann ein Client Anmeldeinformationen mit jeder Anforderung senden, nicht nur nach dem Abrufen von 401.
2. Wenn der Server die Anmeldeinformationen nicht akzeptiert, wird eine 401-Antwort (nicht autorisiert) zurückgegeben. Die Antwort enthält einen Www-Authenticate-Header, der eine oder mehrere Herausforderungen enthält. Jede Abfrage gibt ein Authentifizierungsschema, das vom Server erkannt.

Der Server kann auch eine anonyme Anforderung 401 zurückgeben. In der Tat ist, die in der Regel wie die Authentifizierung initiiert wird:

1. Der Client sendet eine anonyme Anforderung an.
2. Der Server gibt 401 zurück.
3. Die Clients sendet die Anforderung mit den Anmeldeinformationen an.

Dieser Flow enthält die beiden *Authentifizierung* und *Autorisierung* Schritte.

- Authentifizierung erweist es sich um die Identität des Clients.
- Autorisierung wird bestimmt, ob der Client eine bestimmte Ressource zugreifen kann.

Behandeln Sie Authentifizierungsfilter in Web-API Authentifizierung, aber keine Autorisierung. Autorisierung sollte durch einen Autorisierungsfilter oder innerhalb der Controlleraktion durchgeführt werden.

Dies ist der Flow in der Web-API 2-Pipeline:

1. Vor dem Aufrufen einer Aktion, erstellt die Web-API eine Liste der Authentifizierungsfilter für diese Aktion. Dies schließt die Filter mit den Aktionsbereich, Bereich von Controller und globalen Bereich.
2. Web-API-Aufrufe **AuthenticateAsync** auf jedem Filter in der Liste. Jeder Filter kann die Anmeldeinformationen in der Anforderung überprüfen. Wenn Filter Anmeldeinformationen erfolgreich überprüft wurde, erstellt der Filter ein **IPrincipal** und an die Anforderung angefügt wurde. Filter kann auch an diesem Punkt einen Fehler auslösen. Wenn dies der Fall ist, wird der Rest der Pipeline nicht ausgeführt.
3. Vorausgesetzt, dass kein Fehler vorliegt, wird die Anforderung den Rest der Pipeline durchläuft.
4. Schließlich Web-API ruft alle Authentifizierungsfilter **"challengeasync"** Methode. Filter mit dieser Methode können Sie eine Herausforderung für die Antwort, hinzufügen, bei Bedarf. In der Regel (aber nicht immer) würde, die als Reaktion auf ein 401-Fehler erfolgen.

Die folgenden Diagramme zeigen die beiden möglichen Fälle. Im ersten der Authentifizierungsfilter erfolgreiche Authentifizierung der Anforderung, ein Autorisierungsfilter autorisiert die Anforderung und die Controlleraktion gibt 200 (OK) zurück.

![](authentication-filters/_static/image1.png)

Im zweiten Beispiel die Authentifizierungsfilter authentifiziert die Anforderung, aber der Autorisierungsfilter gibt 401 (nicht autorisiert). In diesem Fall wird die Controlleraktion nicht aufgerufen werden. Der Authentifizierungsfilter Fügt einen Www-Authenticate-Header der Antwort hinzu.

![](authentication-filters/_static/image2.png)

Andere Kombinationen sind möglich&mdash;z. B. wenn die Controlleraktion über anonyme Anforderungen zulässt, müssen Sie möglicherweise einen Authentifizierungsfilter, aber ohne Autorisierung.

## <a name="implementing-the-authenticateasync-method"></a>Implementierung der AuthenticateAsync-Methode

Die **AuthenticateAsync** Methode versucht, die die Anforderung zu authentifizieren. So sieht die Signatur der Methode aus:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

Die **AuthenticateAsync** Methode müssen eine der folgenden:

1. "Nothing" (ohne-Op).
2. Erstellen Sie eine **IPrincipal** und legen Sie ihn in der Anforderung.
3. Legen Sie ein fehlerhaftes Ergebnis.

(1) bedeutet, dass die Anforderung nicht über Anmeldeinformationen verfügt, die der Filter versteht-Option. Option (2) bedeutet, dass der Filter wird die Anforderung erfolgreich authentifiziert. Option (3) bedeutet, dass die Anforderung hatte ungültige Anmeldeinformationen (z. B. das falsche Kennwort), die eine Fehlerantwort auslöst.

Hier ist eine allgemeine Gliederung für die Implementierung **AuthenticateAsync**.

1. Suchen Sie nach Anmeldeinformationen in der Anforderung.
2. Wenn keine Anmeldeinformationen vorhanden sind, wird keine Aktion durchführen, und geben Sie (keine Aktion) zurück.
3. Wenn Anmeldeinformationen vorhanden sind, aber das Authentifizierungsschema der Filter nicht erkennt, wird keine Aktion durchführen, und geben Sie (keine Aktion) zurück. Ein weiterer Filter in der Pipeline kann das Schema erhalten.
4. Wenn die Anmeldeinformationen, die der Filter zu verstehen sind, versuchen Sie, die Authentifizierung.
5. Wenn die Anmeldeinformationen gefälscht sind, durch Festlegen 401 zurückgegeben `context.ErrorResult`.
6. Wenn die Anmeldeinformationen gültig sind, erstellen Sie eine **IPrincipal** und `context.Principal`.

Im folgenden Code veranschaulicht die **AuthenticateAsync** Methode aus der [Standardauthentifizierung](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/BasicAuthentication/ReadMe.txt) Beispiel. Jeder Schritt geben Sie die Kommentare an. Der Code zeigt verschiedene Beispiele für Fehler: ein Autorisierungsheader mit keine Anmeldeinformationen, fehlerhafte Anmeldeinformationen und ungültigen Benutzernamens/Kennworts.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Ein Fehlerergebnis festlegt

Der Filter muss festgelegt, wenn die Anmeldeinformationen ungültig sind, `context.ErrorResult` auf eine **IHttpActionResult** erstellt eine Fehlerantwort. Weitere Informationen zu **IHttpActionResult**, finden Sie unter [Aktionsergebnisse in Web-API 2](../getting-started-with-aspnet-web-api/action-results.md).

Das Beispiel für die Standardauthentifizierung enthält ein `AuthenticationFailureResult` -Klasse, die für diesen Zweck geeignet ist.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementieren von "challengeasync"

Der Zweck der **"challengeasync"** Methode wird die Antwort, authentifizierungsanforderungen hinzugefügt werden kann, bei Bedarf. So sieht die Signatur der Methode aus:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Die Methode ist für alle Authentifizierungsfilter in der Pipeline aufgerufen.

Es ist wichtig zu verstehen, dass **"challengeasync"** heißt *vor* die HTTP-Antwort wird erstellt, und möglicherweise sogar, bevor Sie die Controller-Aktion ausgeführt wird. Wenn **"challengeasync"** aufgerufen wird, `context.Result` enthält ein **IHttpActionResult**, der später zum Erstellen der HTTP-Antwort verwendet wird. Dies der Fall bei **"challengeasync"** wird aufgerufen, Sie kennen nichts über die HTTP-Antwort noch nicht. Die **"challengeasync"** sollten ersetzen Sie die Methode den ursprünglichen Wert von `context.Result` mit einem neuen **IHttpActionResult**. Dies **IHttpActionResult** müssen die ursprüngliche umschließen `context.Result`.

![](authentication-filters/_static/image3.png)

Ich nenne die ursprüngliche **IHttpActionResult** der *innere Ergebnis*, und die neue **IHttpActionResult** der *äußeren Ergebnis*. Das äußere Ergebnis muss die folgenden Schritte ausführen:

1. Rufen Sie das innere Ergebnis um die HTTP-Antwort zu erstellen.
2. Überprüfen Sie die Antwort.
3. Fügen Sie gegebenenfalls an die Antwort, eine authentifizierungsaufforderung hinzu.

Das folgende Beispiel stammt aus dem Beispiel für die Standardauthentifizierung. Definiert eine **IHttpActionResult** für das äußere Ergebnis.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

Die `InnerResult` Eigenschaft enthält die innere **IHttpActionResult**. Die `Challenge` Eigenschaft darstellt, ein Www-Authentifizierungsheader. Beachten Sie, dass **ExecuteAsync** ruft zuerst `InnerResult.ExecuteAsync` zum Erstellen der HTTP-Antwort und fügt dann die Herausforderung bei Bedarf.

Überprüfen Sie den Antwortcode vor dem Hinzufügen der Aufforderung an. Die meisten Authentifizierungsschemen nur hinzufügen, eine Herausforderung darstellen, wenn die Antwort "401" ist, wie hier gezeigt. Einige Authentifizierungsschemas jedoch eine Herausforderung eine erfolgreiche Antwort hinzufügen. Beispielsweise finden Sie unter [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Erhält die `AddChallengeOnUnauthorizedResult` Klasse, den tatsächlichen Code **"challengeasync"** ist einfach. Sie gerade das Ergebnis zu erstellen, und fügen Sie ihn auf `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Hinweis: Das Beispiel für die Standardauthentifizierung abstrahiert diese Logik etwas, indem Sie diesen in eine Erweiterungsmethode.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Kombinieren von Authentifizierungsfilter mit Authentifizierung auf Aufrufebene Host

"Authentifizierung auf Hostebene" ist die Authentifizierung, die vom Host (z. B. IIS) ausgeführt wird vor der Anforderung erreicht die Web-API-Framework.

Häufig empfiehlt es sich zum Aktivieren der Authentifizierung auf Hostebene, für den Rest der Anwendung aber deaktivieren, für Ihre Web-API-Controller. Beispielsweise ist ein typisches Szenario zum Aktivieren der Formularauthentifizierung auf der Hostebene, jedoch tokenbasierte Authentifizierung für Web-API ein.

Rufen Sie zum Deaktivieren der Authentifizierung auf Hostebene, in der Web-API-Pipeline `config.SuppressHostPrincipal()` in Ihrer Konfiguration. Dies bewirkt, dass Web-API So entfernen Sie die **IPrincipal** in jede Anforderung, die die Web-API-Pipeline eingibt. Effektiv es &quot;un-authentifiziert&quot; der Anforderung.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Sicherheitsfilter von ASP.NET Web API](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazin)
