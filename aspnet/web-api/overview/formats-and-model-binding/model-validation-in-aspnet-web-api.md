---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Modellvalidierung in ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/20/2012
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 611a6466e160387592df678b3b8556625ff8e234
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835164"
---
<a name="model-validation-in-aspnet-web-api"></a>Modellvalidierung in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Wenn ein Client Daten an Ihre Web-API sendet, möchten Sie häufig die Daten vor der Verarbeitung zu überprüfen. In diesem Artikel zeigt, wie Ihre Modelle kommentieren, verwenden Sie die Anmerkungen für die datenvalidierung und Behandeln von Validierungsfehlern in Ihrer Web-API wird.

## <a name="data-annotations"></a>Datenanmerkungen

In ASP.NET Web-API können Sie Attribute aus der [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) Namespace, um Überprüfungsregeln für Eigenschaften für Ihr Modell festgelegt. Beachten Sie folgende Modell:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Wenn Sie die modellvalidierung in ASP.NET MVC verwendet haben, sollte das bekannt vorkommen. Die **erforderlich** Attribut wird angegeben, dass die `Name` Eigenschaft darf nicht null sein. Die **Bereich** Attribut wird angegeben, dass `Weight` muss zwischen 0 und 999 liegen.

Nehmen wir an, dass ein Client eine POST-Anforderung mit der folgenden JSON-Darstellung sendet:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Sie sehen, dass der Client nicht enthält die `Name` -Eigenschaft, die als markiert ist erforderlich. Bei Web-API konvertiert den JSON-Code in eine `Product` Instanz überprüft die `Product` für die Validierungsattribute. In Ihrem-Controlleraktion können Sie überprüfen, ob das Modell gültig ist:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Modellvalidierung garantiert nicht, dass der Clientdaten sicher sind. Zusätzliche Überprüfung kann in anderen Ebenen der Anwendung erforderlich sein. (Z. B. möglicherweise die Datenschicht foreign Key-Einschränkungen erzwingen.) Das Tutorial [mithilfe des Web-API mit Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) untersucht einige dieser Probleme.

**"Unterdimensionierte Posting"**: unterdimensionierte Bereitstellung tritt auf, wenn der Client und einige Eigenschaften zu verwerfen. Nehmen wir beispielsweise an, dass der Client die folgenden sendet:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

Hier ist der Client geben keine Werte für `Price` oder `Weight`. Das JSON-Formatierungsprogramm weist den Standardwert 0 (null), um die fehlenden Eigenschaften.

![](model-validation-in-aspnet-web-api/_static/image1.png)

Der Modellzustand ist ungültig, da 0 (null) einen gültigen Wert für diese Eigenschaften ist. Ob dieses Problem auftritt, hängt von Ihrem Szenario ab. Z. B. in einem Updatevorgang sollten Sie zur Unterscheidung zwischen "Zero" und "nicht festgelegt." Um Clients zum Festlegen eines Werts zu erzwingen, stellen Sie die Eigenschaft NULL-Werte zulässt, und legen die **erforderlich** Attribut:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Overposting"**: ein Client kann auch senden *weitere* Daten als erwartet. Zum Beispiel:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

Hier der JSON-Code enthält eine Eigenschaft ("Color"), die in nicht vorhanden ist die `Product` Modell. In diesem Fall ignoriert der JSON-Formatierungsprogramm einfach diesen Wert. (Das XML-Formatierungsprogramm führt.) Overposting verursacht Probleme auf, wenn Ihr Modell über Eigenschaften, die eigentlich verfügt schreibgeschützt sein. Zum Beispiel:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Sollen nicht zum Aktualisieren der `IsAdmin` Eigenschaft und selbst Administratoren Berechtigungen zu erhöhen! Die sicherste Strategie ist die Verwendung eine Modellklasse, die genau übereinstimmt, was der Client senden darf:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Brad Wilsons Blog-Beitrag "[Eingabeüberprüfung Visual Studio. Modellvalidierung in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"eine gute Beschreibung von unterdimensionierte Bereitstellung und zu viele Angaben gemacht hat. Obwohl im Beitrag zu ASP.NET MVC 2 ist, sind die Probleme weiterhin relevant, die Web-API.


## <a name="handling-validation-errors"></a>Behandeln von Validierungsfehlern

Einen Fehler an den Client werden von Web-API nicht automatisch zurück, wenn die Validierung fehlschlägt. Es ist Aufgabe der Controlleraktion auf den Modellzustand überprüfen und entsprechend reagieren.

Sie können auch erstellen, dass einen Aktionsfilter, um den Modellstatus zu überprüfen, bevor die Controlleraktion aufgerufen wird. Der folgende Code zeigt ein Beispiel:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Wenn die modellvalidierung fehlschlägt, gibt dieser Filter eine HTTP-Antwort, die die Validierungsfehler enthält. In diesem Fall wird die Controlleraktion nicht aufgerufen werden.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Fügen Sie zum Anwenden dieses Filters auf alle Web-API-Controller eine Instanz des Filters, der die **HttpConfiguration.Filters** Auflistung während der Konfiguration:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Eine weitere Option ist der Filter als Attribut individuellen Controllern oder Controlleraktionen festgelegt:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
