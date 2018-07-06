---
uid: web-api/overview/error-handling/exception-handling
title: Behandlung von Ausnahmen in ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 2db00c1ab241e0dad051c2a3bcd3b0e4bbf4d5c4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807660"
---
<a name="exception-handling-in-aspnet-web-api"></a>Behandlung von Ausnahmen in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Dieser Artikel beschreibt die Fehler- und Ausnahmebehandlung in ASP.NET Web-API.

- [HttpResponseException](#httpresponserexception)
- [Ausnahmefilter](#exception_filters)
- [Ausnahmefilter registrieren](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

Was geschieht, wenn ein Web-API-Controller eine unerwartete Ausnahme auslöst? Standardmäßig werden die meisten Ausnahmen in einer HTTP-Antwort mit dem Statuscode 500 Internal Server Error übersetzt.

Die **HttpResponseException** Typ ist ein besonderer Fall. Diese Ausnahme gibt alle HTTP-Statuscode, die Sie in der Ausnahmekonstruktor angeben. Beispielsweise gibt die folgende Methode 404-Antwort, nicht gefunden wird, zurück, wenn die *Id* -Parameter ist ungültig.

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

Mehr Kontrolle über die Antwort zu erhalten, können Sie auch die gesamte Antwortnachricht zu erstellen und fügen Sie ihn mit der **HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>Ausnahmefilter

Sie können anpassen, wie Web-API-Ausnahmen durch das Schreiben von behandelt eine *Ausnahmefilter*. Ein Ausnahmefilter wird immer dann ausgeführt, wenn eine Controllermethode eine unbehandelte Ausnahme auslöst, die *nicht* ein **HttpResponseException** Ausnahme. Die **HttpResponseException** Typ ist ein Sonderfall, da es nur speziell für eine HTTP-Antwort zurückgeben.

Ausnahmefilter implementieren die **System.Web.Http.Filters.IExceptionFilter** Schnittstelle. Die einfachste Möglichkeit zum Schreiben eines Ausnahmefilters ist die Ableitung der **System.Web.Http.Filters.ExceptionFilterAttribute** Klasse, und überschreiben die **OnException** Methode.

> [!NOTE]
> Ausnahmefilter in ASP.NET Web-API sind ähnlich wie in ASP.NET MVC. Allerdings sind sie in einem separaten Namespace und die Funktion separat deklariert. Insbesondere die **HandleErrorAttribute** im MVC verwendete Klasse nicht von Web-API-Controllern ausgelöste Ausnahmen behandelt.


Hier ist ein Filter, der konvertiert **NotImplementedException** Ausnahmen in HTTP-Status code 501, nicht implementiert:

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

Die **Antwort** Eigenschaft der **HttpActionExecutedContext** Objekt enthält die HTTP-Antwortnachricht, die an den Client gesendet werden.

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>Ausnahmefilter registrieren

Es gibt mehrere Möglichkeiten zum Registrieren einer Web-API-Ausnahmefilters:

- Nach Aktion
- Vom controller
- Global

Um den Filter auf eine bestimmte Aktion anzuwenden, fügen Sie den Filter auf die Aktion als Attribut hinzu:

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

Um den Filter auf alle Aktionen auf einem Controller anzuwenden, fügen Sie den Filter der Controllerklasse als Attribut hinzu:

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

Um den Filter Global auf alle Web-API-Controller anzuwenden, fügen Sie eine Instanz des Filters, der die **GlobalConfiguration.Configuration.Filters** Auflistung. Batchverarbeitungsorchestrierung-Filter in dieser Auflistung können auf jeder beliebigen Controlleraktion der Web-API.

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

Wenn Sie die Projektvorlage "ASP.NET MVC 4-Webanwendung" verwenden, um Ihr Projekt erstellen, fügen Sie Ihrer Web-API-Konfigurationscode in die `WebApiConfig` -Klasse, die in der App befindet\_Startordner:

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

Die **HttpError** -Objekt bietet eine einheitliche Methode zum Zurückgeben von Fehlerinformationen im Antworttext. Das folgende Beispiel zeigt, wie HTTP-Statuscode 404 (nicht gefunden) zurückgeben mit einem **HttpError** im Antworttext.

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse** ist eine Erweiterungsmethode definiert der **System.Net.Http.HttpRequestMessageExtensions** Klasse. Intern **CreateErrorResponse** erstellt eine **HttpError** -Instanz, und erstellt dann ein **HttpResponseMessage** , enthält die **HttpError**.

In diesem Beispiel wenn die Methode erfolgreich ist, ist wird das Produkt in der HTTP-Antwort. Aber wenn das angeforderte Produkt nicht gefunden wird, die HTTP-Antwort enthält ein **HttpError** im Hauptteil Anforderung. Die Antwort sieht wie folgt aus:

[!code-console[Main](exception-handling/samples/sample9.cmd)]

Beachten Sie, dass die **HttpError** in JSON serialisiert wurde, die in diesem Beispiel. Ein Vorteil der Verwendung **HttpError** besteht darin, dass er über dasselbe geht [Inhalte-Aushandlung](../formats-and-model-binding/content-negotiation.md) und Serialisierung verarbeiten, wie jede andere stark typisiertes Modell.

### <a name="httperror-and-model-validation"></a>HttpError und zur Modellüberprüfung

Sie können für die modellüberprüfung den Modellzustand, übergeben **CreateErrorResponse**, um Fehler bei der Validierung in der Antwort enthalten:

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

In diesem Beispiel gibt möglicherweise die folgende Antwort zurück:

[!code-console[Main](exception-handling/samples/sample11.cmd)]

Weitere Informationen zur modellvalidierung finden Sie unter [Model Validation in ASP.NET Web-API](../formats-and-model-binding/model-validation-in-aspnet-web-api.md).

### <a name="using-httperror-with-httpresponseexception"></a>Verwenden HttpError mit HttpResponseException

Zurück in den vorherigen Beispielen ein **HttpResponseMessage** Nachricht aus der Controlleraktion, aber Sie können auch **HttpResponseException** zurückzugebenden ein **HttpError**. Dadurch können Sie ein stark typisiertes Modell bei normalen Erfolg zurückgegeben, bei der Rückgabe von weiterhin **HttpError** , wenn ein Fehler auftritt:

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
