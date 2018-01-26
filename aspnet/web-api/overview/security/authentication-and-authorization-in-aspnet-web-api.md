---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Authentifizierung und Autorisierung in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: "Bietet eine allgemeine Übersicht über die Authentifizierung und Autorisierung in ASP.NET Web-API."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/27/2012
ms.topic: article
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 2a4b5ed8a712b061b4afdf5a3adc9378dd72b37f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>Authentifizierung und Autorisierung in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Sie erstellt haben, eine Web-API, aber Sie möchten nun den Zugriff darauf steuern. In dieser Reihe von Artikeln sehen wir uns einige Optionen für das Sichern einer Webs-API von nicht autorisierten Benutzern. Diese Reihe befasst sich mit Authentifizierung und Autorisierung.

- *Authentifizierung* ist die Identität des Benutzers kennen. Beispielsweise Alice meldet sich mit ihrem Benutzernamen und Kennwort, und der Server das Kennwort zum Authentifizieren von Alice verwendet.
- *Autorisierung* ist entscheiden, ob ein Benutzer zum Ausführen einer Aktion zulässig ist. Alice verfügt beispielsweise über die Berechtigung zum Abrufen einer Ressource, aber nicht erstellen, eine Ressource.

Der erste Artikel in der Reihe bietet einen allgemeinen Überblick über die Authentifizierung und Autorisierung in ASP.NET Web-API. Andere Themen beschreiben den allgemeinen Authentifizierungsszenarien für Web-API.

> [!NOTE]
> Dank der Personen, die diese Reihe überprüft und wertvolles Feedback bereitgestellt: Rick Anderson Levi Broderick Barry Dorrans Tom Dykstra Hongmei Ge David Matson Daniel Roth, Tim Teebken.


## <a name="authentication"></a>Authentifizierung

Web-API wird davon ausgegangen, dass die Authentifizierung auf dem Host erfolgt. Für das Webhosting ist der Host IIS, die HTTP-Module für die Authentifizierung verwendet. Sie können Ihr Projekt verwendet die Authentifizierungsmodule, die in IIS oder ASP.NET integriert konfigurieren, oder Sie können eigene HTTP-Modul für die benutzerdefinierte Authentifizierung schreiben.

Wenn der Host den Benutzer authentifiziert hat, erstellt er eine *principal*, also ein [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) -Objekt, das den Sicherheitskontext darstellt, in dem Code ausgeführt wird. Der Host den Prinzipal für den aktuellen Thread festlegen anfügt **Thread.CurrentPrincipal**. Der Prinzipal enthält eine zugeordnete **Identität** Objekt, das Informationen über den Benutzer enthält. Wenn der Benutzer authentifiziert ist, die **Identity.IsAuthenticated** -Eigenschaft gibt **"true"**. Für anonyme Anforderungen **IsAuthenticated** gibt **"false"**. Weitere Informationen zu Prinzipalen finden Sie unter [rollenbasierte Sicherheit](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>HTTP-Meldungshandler für die Authentifizierung

Anstatt den Host für die Authentifizierung zu verwenden, können Sie die Authentifizierungslogik Einfügen einer [HTTP-Nachrichtenhandler](../advanced/http-message-handlers.md). In diesem Fall wird der Message-Handler überprüft die HTTP-Anforderung und legt den Prinzipal fest.

Wann sollte Sie Meldungshandler für die Authentifizierung verwenden? Hier sind einige Nachteile:

- Ein HTTP-Modul sieht alle Anforderungen, die der ASP.NET-Pipeline durchlaufen. Message-Handler sieht nur Anforderungen, die Web-API weitergeleitet werden.
- Sie können pro Route-Meldungshandler festlegen, dadurch können Sie ein anderes Authentifizierungsschema zu einem bestimmten Arbeitsplan anwenden.
- HTTP-Module gelten speziell für IIS. Meldungshandler sind Unabhängigkeit von Host, damit diese mit Web-hosting und Selbsthosting verwendet werden können.
- HTTP-Module, die in IIS-Protokollierung, Überwachung und So weiter einbezogen werden.
- HTTP-Module, die weiter oben in der Pipeline ausgeführt werden. Wenn Sie Authentifizierung in einer Message-Handler behandelt, der Prinzipal ist nicht abrufen oder Festlegen der bis der Handler ausgeführt wird. Darüber hinaus wiederhergestellt der Prinzipal, der vorherige Prinzipal, wenn die Antwort den Nachrichtenhandler verlässt.

Wenn Sie Selbsthosting Unterstützung benötigen, ist ein HTTP-Modul im Allgemeinen eine bessere Option. Wenn Sie Selbsthosting unterstützen müssen, sollten Sie einen Meldungshandler.

### <a name="setting-the-principal"></a>Den Prinzipal festlegen

Wenn Ihre Anwendung eine benutzerdefinierte Authentifizierungslogik ausführt, müssen Sie den Prinzipal auf zwei Stellen festlegen:

- **Thread.CurrentPrincipal**. Diese Eigenschaft wird standardmäßig in .NET Prinzipal des Threads festlegen.
- **HttpContext.Current.User**. Diese Eigenschaft ist spezifisch für ASP.NET.

Der folgende Code zeigt, wie Sie zum Festlegen des Prinzipals:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Für das Webhosting, müssen Sie den Prinzipal an beiden Orten festlegen. Andernfalls kann der Sicherheitskontext inkonsistent werden. Für das Selbsthosting jedoch **HttpContext.Current** ist null. Um sicherzustellen, dass der Code ist die Unabhängigkeit von der Host, daher überprüfen, für Null-Zeichen vor dem Zuweisen **HttpContext.Current**, wie dargestellt.

## <a name="authorization"></a>Autorisierung

Autorisierung weiter unten in der Pipeline näher an den Controller. Mit der Sie genauere Entscheidungen zu treffen, wenn Sie den Zugriff auf Ressourcen gewähren.

- *Autorisierungsfilter* ausführen, bevor der Controlleraktion. Wenn die Anforderung nicht autorisiert ist, der Filter gibt eine Fehlerantwort zurück und die Aktion wird nicht aufgerufen.
- Innerhalb einer Controlleraktion erhalten Sie den aktuellen Prinzipal aus der **ApiController.User** Eigenschaft. Beispielsweise können Sie filtern eine Liste der Ressourcen basierend auf den Benutzernamen, nur die Ressourcen, die für diesen Benutzer gehören zurückgeben.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Mit dem [autorisieren]-Attribut

Web-API bietet eine integrierte Autorisierungsfilter [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Dieser Filter überprüft, ob der Benutzer authentifiziert ist. Wenn dies nicht der Fall ist, wird die HTTP-Statuscode 401 (nicht autorisiert), ohne Aufrufen der Aktion zurückgegeben.

Sie können den Filter, die Global auf Controllerebene, oder klicken Sie auf der Ebene der Inidivual Aktionen anwenden.

**Global**: zum Einschränken des Zugriffs für alle Web-API-Controller Hinzufügen der **AuthorizeAttribute** Filter zur globalen Filterliste:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Controller**: zum Einschränken des Zugriffs für einen bestimmten Controller hinzufügen den Filter als ein Attribut mit dem Controller:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Aktion**: um den Zugriff auf bestimmte Aktionen einzuschränken, fügen Sie das Attribut zur Aktionsmethode:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Alternativ können Sie den Controller einzuschränken und dann zulassen des anonymen Zugriffs auf bestimmte Aktionen mithilfe der `[AllowAnonymous]` Attribut. Im folgenden Beispiel die `Post` Methode ist eingeschränkt, aber die `Get` -Methode erlaubt anonymen Zugriff.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

In den vorherigen Beispielen kann der Filter alle authentifizierten Benutzer auf die eingeschränkten Methoden zugreifen. nur anonyme Benutzer werden beibehalten. Sie können auch den Zugriff auf bestimmte Benutzer oder Benutzer zu bestimmten Rollen einschränken:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> Die **AuthorizeAttribute** Filter für Web-API-Controller befindet sich der **System.Web.Http** Namespace. Es ist ein ähnlich wie Filter für MVC-Controller im die **System.Web.Mvc** Namespace, der mit der Web-API-Controller nicht kompatibel ist.


### <a name="custom-authorization-filters"></a>Benutzerdefinierte Autorisierungsfilter

Leiten Sie zum Schreiben eines benutzerdefinierten Autorisierungs-Filters aus einem der folgenden Typen:

- **AuthorizeAttribute**. Erweitern Sie diese Klasse zum Ausführen von Autorisierungslogik basierend auf den aktuellen Benutzer sowie die Rollen des Benutzers.
- **AuthorizationFilterAttribute**. Erweitern Sie diese Klasse zum synchronen Autorisierungslogik ausführen, die nicht unbedingt auf den aktuellen Benutzer oder Rolle basiert.
- **IAuthorizationFilter**. Diese Schnittstelle zum Ausführen von asynchronen Autorisierungslogik zu implementieren; Angenommen, Ihre Autorisierungslogik asynchrone e/a- oder Netzwerk aufgerufen wird. (Wenn die Autorisierungslogik CPU-gebunden ist, ist die Ableitung einfacher **AuthorizationFilterAttribute**, da keine müssen Sie eine asynchrone Methode zu schreiben.)

Das folgende Diagramm zeigt die Klassenhierarchie für das **AuthorizeAttribute** Klasse.

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorisierung in eine Controlleraktion

In einigen Fällen können Sie eine Anforderung zum fortsetzen, aber ändern Sie das Verhalten basierend auf dem Prinzipal ermöglichen. Beispielsweise kann die Informationen, die Sie zurückgeben abhängig von der Rolle des Benutzers ändern. Innerhalb einer Controllermethode erhalten Sie den aktuellen Prinzipal aus der **ApiController.User** Eigenschaft.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
