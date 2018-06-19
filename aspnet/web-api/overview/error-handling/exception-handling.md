---
uid: web-api/overview/error-handling/exception-handling
title: Ausnahmebehandlung in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506959"
---
<a name="exception-handling-in-aspnet-web-api"></a>Ausnahmebehandlung in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Dieser Artikel beschreibt die Fehler- und Ausnahmebehandlung in ASP.NET Web-API.

- [HttpResponseException](#httpresponserexception)
- [Ausnahmefilter](#exception_filters)
- [Ausnahmefilter registrieren](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Was geschieht, wenn ein Web-API-Controller eine nicht abgefangene Ausnahme ausgelöst? Standardmäßig werden die meisten Ausnahmen in einer HTTP-Antwort mit dem Statuscode 500 Internal Server Error übersetzt.

Die **HttpResponseException** Typ ist ein Sonderfall. Diese Ausnahme gibt alle HTTP-Statuscode an, die Sie in der Ausnahmekonstruktor angeben. Beispielsweise gibt die folgende Methode 404 Nichtgefunden wird, zurück, wenn die *Id* -Parameter ist ungültig.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Mehr Kontrolle über die Antwort kann auch die gesamte Antwortnachricht zu erstellen und fügen Sie ihn mit dem **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Ausnahmefilter

Sie können anpassen, wie Ausnahmen von Web-API durch Schreiben von behandelt eine *Ausnahmefilter*. Ein Ausnahmefilter wird ausgeführt, wenn eine Controllermethode nicht behandelte Ausnahme auslöst, wird *nicht* ein **HttpResponseException** Ausnahme. Die **HttpResponseException** Typ ist ein Sonderfall, da dieser Standard entwickelt wurde speziell für die Rückgabe einer HTTP-Antwort.

Ausnahmefilter implementieren die **System.Web.Http.Filters.IExceptionFilter** Schnittstelle. Die einfachste Methode zum Schreiben eines Ausnahmefilters ist die Ableitung der **System.Web.Http.Filters.ExceptionFilterAttribute** Klasse, und überschreiben die **OnException** Methode.

> [!NOTE]
> Ausnahmefilter in ASP.NET Web-API sind in ASP.NET MVC vergleichbar. Allerdings werden sie in einem separaten Namespace und die Funktion separat deklariert. Insbesondere die **von HandleErrorAttribute** Klasse zur Verwendung in MVC-Web-API-Controller ausgelöste Ausnahmen nicht behandelt.


Hier ist ein Filter, der konvertiert **NotImplementedException** Ausnahmen in HTTP-Status code 501, nicht implementiert:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

Die **Antwort** Eigenschaft von der **HttpActionExecutedContext** Objekt enthält die HTTP-Antwortnachricht, die an den Client gesendet werden sollen.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Ausnahmefilter registrieren

Es gibt mehrere Möglichkeiten zum Registrieren eines Web-API-Ausnahmefilters aus:

- Von der Aktion
- Vom Netzwerkcontroller
- Global

Um den Filter auf eine bestimmte Aktion anwenden, fügen Sie den Filter auf die Aktion als Attribut hinzu:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Zum Anwenden des Filters für alle Aktionen auf einem Domänencontroller fügen Sie den Filter auf die Controllerklasse als Attribut hinzu:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Um den Filter Global auf alle Web-API-Controller anzuwenden, fügen Sie eine Instanz des Filters, der die **GlobalConfiguration.Configuration.Filters** Auflistung. Batchverarbeitungsorchestrierung Filter in dieser Sammlung gelten für alle Web-API-Controlleraktion.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Wenn Sie die Projektvorlage "ASP.NET MVC 4-Webanwendung" verwenden, um das Projekt zu erstellen, speichern Sie Ihre Web-API-Konfigurationscode innerhalb der `WebApiConfig` -Klasse, die in der App befindet\_Startordner:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

Die **HttpError** Objekt bietet eine konsistente Möglichkeit, Fehlerinformationen im Antworttext zurückgegeben. Im folgende Beispiel wird gezeigt, wie das zurückzugebende HTTP-Statuscode 404 (Nichtgefunden) mit einem **HttpError** im Antworttext.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** ist eine Erweiterungsmethode definiert der **System.Net.Http.HttpRequestMessageExtensions** Klasse. Intern **CreateErrorResponse** erstellt eine **HttpError** Instanz, und erstellt dann ein **HttpResponseMessage** , enthält die **HttpError**.

Wenn die Methode erfolgreich ist, gibt es in diesem Beispiel das Produkt in der HTTP-Antwort zurück Wenn das angeforderte Produkt nicht gefunden wird, die HTTP-Antwort enthält jedoch eine **HttpError** im Hauptteil Anforderung. Die Antwort kann wie folgt aussehen:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Beachten Sie, dass die **HttpError** in diesem Beispiel wird in JSON serialisiert wurde. Ein Vorteil der Verwendung von **HttpError** ist, dass darin über dasselbe [Inhalt Aushandlung](../formats-and-model-binding/content-negotiation.md) und Serialisierungsprozess wie jede andere stark typisiertes Modell.

### <a name="httperror-and-model-validation"></a>HttpError und Modellvalidierung

Für die modellüberprüfung, können Sie der Modellzustand, übergeben **CreateErrorResponse**, die Validierungsfehler in die Antwort eingeschlossen werden sollen:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

In diesem Beispiel wird möglicherweise die folgende Antwort zurück:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Weitere Informationen zur modellvalidierung finden Sie unter [Modellvalidierung in ASP.NET Web API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Verwenden von HttpError mit HttpResponseException

In den vorherigen Beispielen zurückgegeben ein **HttpResponseMessage** Nachricht aus der Controlleraktion, aber Sie können auch **HttpResponseException** zurückzugebenden ein **HttpError**. Auf diese Weise können Sie ein stark typisiertes Modell. im Erfolgsfall normalen zurück, bei der Rückgabe von weiterhin **HttpError** , wenn ein Fehler aufgetreten ist:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
