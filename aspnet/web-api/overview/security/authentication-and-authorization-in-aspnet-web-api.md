---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Authentifizierung und Autorisierung in ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: Bietet eine allgemeine Übersicht über Authentifizierung und Autorisierung in ASP.NET Web-API.
ms.author: riande
ms.date: 11/27/2012
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: a78606a74b2149e68e3b01f4fe204f4a13edf4b5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827293"
---
<a name="authentication-and-authorization-in-aspnet-web-api"></a>Authentifizierung und Autorisierung in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Sie haben eine Web-API erstellt, aber Sie möchten nun den Zugriff darauf einschränken. In dieser Artikelreihe betrachten wir einige Optionen zum Sichern einer Webs-API von nicht autorisierten Benutzern. Diese Reihe behandelt die Authentifizierung und Autorisierung.

- *Authentifizierung* ist die Identität des Benutzers kennen. Zum Beispiel Alice meldet sich mit ihrem Benutzernamen und das Kennwort, und der Server verwendet das Kennwort zur Authentifizierung von Alice.
- *Autorisierung* ist die Entscheidung, ob ein Benutzer zum Ausführen einer Aktion zulässig ist. Alice ist z. B. über die Berechtigung zum Abrufen einer Ressource, aber nicht erstellen Sie eine Ressource.

Der erste Artikel in der Reihe bietet eine allgemeine Übersicht über Authentifizierung und Autorisierung in ASP.NET Web-API. Andere Themen werden allgemeine Szenarien mit Authentifizierung für Web-API beschrieben.

> [!NOTE]
> Dank der Personen, die dieser Serie überprüft und wertvolles Feedback bereitgestellt: Rick Anderson, Levi Broderick, Barry Dorrans, Tom Dykstra, Hongmei Ge, David Matson, Daniel Roth, Tim Teebken.


## <a name="authentication"></a>Authentifizierung

Web-API wird davon ausgegangen, dass die Authentifizierung auf dem Host erfolgt. Für Webhosting ist der Host IIS, die HTTP-Module für die Authentifizierung verwendet. Sie können Ihr Projekt verwendet die Authentifizierungsmodule, die direkt in IIS oder ASP.NET konfigurieren, oder Sie können eigene HTTP-Modul zum Ausführen der benutzerdefinierten Authentifizierung schreiben.

Wenn der Host der Benutzer authentifiziert wird, erstellt es eine *principal*, die eine [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) -Objekt, das den Sicherheitskontext darstellt, unter dem Code ausgeführt wird. Der Host fügt den Prinzipal für den aktuellen Thread durch Festlegen von **Thread.CurrentPrincipal**. Der Prinzipal enthält eine zugeordnete **Identität** Objekt, das Informationen über den Benutzer enthält. Wenn der Benutzer authentifiziert ist, die **Identity.IsAuthenticated** -Eigenschaft gibt **"true"**. Für anonyme Anforderungen **IsAuthenticated** gibt **"false"**. Weitere Informationen zu Prinzipalen finden Sie unter [rollenbasierte Sicherheit](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>HTTP-Nachrichtenhandler für die Authentifizierung

Anstatt den Host für die Authentifizierung zu verwenden, können Sie die Authentifizierungslogik in Einfügen einer [HTTP-Nachrichtenhandler](../advanced/http-message-handlers.md). In diesem Fall wird der Message-Handler überprüft die HTTP-Anforderung und legt den Prinzipal fest.

Wann sollten Sie Meldungshandler für die Authentifizierung verwenden? Hier sind einige Nachteile:

- Ein HTTP-Modul sieht alle Anforderungen, die der ASP.NET-Pipeline durchlaufen. Ein Meldungshandler sieht nur Anforderungen, die auf Web-API weitergeleitet werden.
- Sie können pro Route-Meldungshandler festlegen, dadurch können Sie ein anderes Authentifizierungsschema für eine bestimmte Route anwenden.
- HTTP-Module gelten speziell für IIS. Nachrichtenhandler sind Hosts gegenüber agnostisch, sodass sie mit Web-hosting und Self-hosting verwendet werden können.
- HTTP-Module teilnehmen in IIS-Protokollierung, Überwachung und So weiter.
- HTTP-Module, die weiter oben in der Pipeline ausgeführt werden. Authentifizierung in einem Ereignishandler für die Nachricht verarbeitet, der Prinzipal ist nicht abrufen oder festlegen, bis der Handler ausgeführt wird. Darüber hinaus wird der Prinzipal auf der vorherige Prinzipal zurückgesetzt, wenn die Antwort den Message-Handler verlässt.

Wenn Sie keine Self-hosting unterstützen müssen, ist ein HTTP-Modul in der Regel eine bessere Option. Wenn Sie Self-hosting unterstützen müssen, sollten Sie einen Meldungshandler.

### <a name="setting-the-principal"></a>Den Prinzipal festlegen

Wenn Ihre Anwendung keine Logik für die benutzerdefinierte Authentifizierung durchführt, müssen Sie den Prinzipal auf zwei Stellen festlegen:

- **Thread.CurrentPrincipal**. Diese Eigenschaft ist das Standardverfahren zum Festlegen von den Prinzipal des Threads in .NET.
- **HttpContext.Current.User**. Diese Eigenschaft ist spezifisch für ASP.NET.

Der folgende Code zeigt, wie den Prinzipal festgelegt wird:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Für Webhosting, müssen Sie den Prinzipal an beiden Orten festlegen. Andernfalls kann der Sicherheitskontext inkonsistent werden. Für das Selbsthosting jedoch **"HttpContext.Current"** ist null. Um sicherzustellen, dass Ihr Code Hosts gegenüber agnostisch, überprüfen Sie daher für Null-Zeichen vor dem Zuweisen zu **"HttpContext.Current"**, wie gezeigt.

## <a name="authorization"></a>Autorisierung

Autorisierung in der Pipeline später näher an den Controller. Mit der Sie genauere Entscheidungen zu treffen, wenn Sie den Zugriff auf Ressourcen gewähren.

- *Autorisierungsfilter werden also* ausführen, bevor die Controlleraktion. Wenn die Anforderung nicht autorisiert ist, der Filter gibt eine Fehlerantwort zurück, und die Aktion wird nicht aufgerufen.
- In eine Controlleraktion, erhalten Sie den aktuellen Prinzipal aus der **ApiController.User** Eigenschaft. Beispielsweise können Sie filtern eine Liste der Ressourcen basierend auf den Benutzernamen ein, nur die Ressourcen, die für diesen Benutzer gehören zurückgeben.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Mithilfe der [autorisieren]-Attribut

Web-API bietet einen integrierte Autorisierungsfilter [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Dieser Filter wird überprüft, ob der Benutzer authentifiziert ist. Wenn dies nicht der Fall ist, wird die HTTP-Statuscode 401 (Unauthorized), ohne Aufrufen der Aktion zurückgegeben.

Sie können den Filter Global auf Controllerebene und auf der Ebene der einzelnen Aktionen anwenden.

**Global**: zum Einschränken des Zugriffs für alle Web-API-Controller Hinzufügen der **AuthorizeAttribute** Filter zur globalen Filterliste:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Controller**: zum Einschränken des Zugriffs für einen bestimmten Controller fügen Sie den Filter als Attribut mit dem Controller:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Aktion**: um den Zugriff für bestimmte Aktionen einzuschränken, fügen Sie das Attribut an die Aktionsmethode:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Alternativ können Sie den Controller einzuschränken und Zulassen des anonymen Zugriffs auf bestimmte Aktionen klicken Sie dann mithilfe der `[AllowAnonymous]` Attribut. Im folgenden Beispiel die `Post` Methode ist beschränkt, aber die `Get` -Methode erlaubt anonymen Zugriff.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

In den vorherigen Beispielen kann der Filter allen authentifizierten Benutzern den Zugriff auf den eingeschränkten Methoden; nur anonyme Benutzer werden Sie beibehalten. Sie können auch den Zugriff auf bestimmte Benutzer oder Benutzer in bestimmten Rollen einschränken:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> Die **AuthorizeAttribute** Filter für Web-API-Controller befindet sich in der **"System.Web.http"** Namespace. Es ist ein ähnlicher Filter für MVC-Controller im der **System.Web.Mvc** -Namespace, der mit der Web-API-Controller nicht kompatibel ist.


### <a name="custom-authorization-filters"></a>Benutzerdefinierte Autorisierungsfilter

Um einen eigenen Autorisierungsfilter schreiben, leiten Sie sich von einem dieser Typen:

- **AuthorizeAttribute**. Erweitern Sie diese Klasse, um basierend auf den aktuellen Benutzer und Rollen des Benutzers Autorisierungslogik auszuführen.
- **AuthorizationFilterAttribute**. Erweitern Sie diese Klasse, um synchrone Autorisierungslogik auszuführen, die nicht unbedingt auf den aktuellen Benutzer oder die Rolle basiert.
- **IAuthorizationFilter**. Implementieren Sie diese Schnittstelle, um asynchrone Autorisierungslogik auszuführen. Angenommen, Ihre Autorisierungslogik asynchrone e/A- oder Netzwerk-Aufrufe ausführt. (Wenn der Autorisierungslogik CPU-gebunden ist, ist es einfacher, abgeleitet **AuthorizationFilterAttribute**, da müssen Sie, eine asynchrone Methode zu schreiben.)

Das folgende Diagramm zeigt die Klassenhierarchie für die **AuthorizeAttribute** Klasse.

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorisierung in eine Controlleraktion

In einigen Fällen können Sie eine Anforderung zum Fortfahren, aber ändern das Verhalten basierend auf dem Prinzipal ermöglichen. Beispielsweise kann die Informationen, die Sie zurückgeben abhängig von der Benutzerrolle ändern. Innerhalb einer Controllermethode erhalten Sie den aktuellen Prinzipal aus der **ApiController.User** Eigenschaft.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
