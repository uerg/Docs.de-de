---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Model-Validierung in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/20/2012
ms.topic: article
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 409a91eceb8baa48a7dded1b850d59a27cec2c60
ms.sourcegitcommit: 5ae0c125ee3bbd324edef3818d1d160f4dd84602
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/17/2018
---
<a name="model-validation-in-aspnet-web-api"></a>Modellvalidierung in ASP.NET Web-API
====================
durch [Mike Wasson](https://github.com/MikeWasson)

Wenn ein Client Daten an Ihre Web-API sendet, möchten Sie häufig zum Überprüfen der Daten vor dem Ausführen der Verarbeitung. In diesem Artikel werden die Kommentieren die Modelle, verwenden die Anmerkungen zur datenüberprüfung und Behandeln von Validierungsfehlern in Ihre Web-API veranschaulicht.

## <a name="data-annotations"></a>Datenanmerkungen

In ASP.NET Web-API können Sie Attribute aus der [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) Namespace Überprüfungsregeln für Eigenschaften für das Modell festlegen. Betrachten Sie das folgende Modell:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Wenn Sie modellvalidierung in ASP.NET MVC verwendet haben, sollte dies bekannt vorkommen. Die **erforderlich** Attribut besagt, dass die `Name` Eigenschaft darf nicht null sein. Die **Bereich** Attribut laut `Weight` muss zwischen 0 und 999 liegen.

Nehmen wir an, dass ein Client eine POST-Anforderung mit der folgenden JSON-Darstellung sendet:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

Beachten Sie, dass der Client nicht eingeschlossen war die `Name` -Eigenschaft, die als markiert ist erforderlich. Beim Web-API in JSON konvertiert eine `Product` Instanz überprüft die `Product` gegen die Validierungsattribute. In der Controlleraktion können Sie überprüfen, ob das Modell gültig ist:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

Modellvalidierung garantiert nicht, dass Clientdaten sicher ist. Zusätzliche Validierung möglicherweise in anderen Ebenen der Anwendung benötigt werden. Die Datenschicht (z. B. u. u. foreign Key-Einschränkungen eingehalten werden.) Das Lernprogramm [mithilfe des Web-API mit Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) untersucht einige dieser Probleme.

**"Unzureichende Buchung"**: Unzureichende Buchung tritt auf, wenn der Client und einige Eigenschaften zu verwerfen. Nehmen Sie z. B. an, dass der Client die folgenden sendet:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

Der Client hat hier keinen Werte für `Price` oder `Weight`. Der JSON-Formatierer weist den Standardwert 0 (null), um die fehlenden Eigenschaften.

![](model-validation-in-aspnet-web-api/_static/image1.png)

Der Modellzustand ist gültig, da NULL ein gültiger Wert für diese Eigenschaften ist. Ob dies ein Problem aufgetreten ist, hängt von Ihrem Szenario ab. Beispielsweise kann in einem Updatevorgang sollen zur Unterscheidung zwischen "0" und "nicht festgelegt." Um Clients zum Festlegen eines Werts zu erzwingen, stellen Sie die Eigenschaft NULL-Werte zulässt, und legen die **erforderlich** Attribut:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Stark buchen"**: ein Client kann auch senden *weitere* Daten als erwartet. Zum Beispiel:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

Hier der JSON-Code enthält eine Eigenschaft ("Color"), die in nicht vorhanden ist die `Product` Modell. In diesem Fall wird das JSON-Formatierungsprogramm einfach dieser Wert ignoriert. (Der XML-Formatierer hat die gleiche Funktion.) Übermäßige Buchung verursacht Probleme, wenn Ihr Modell über Eigenschaften, die Sie verfügt schreibgeschützt sein sollte. Zum Beispiel:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Sie nicht möchten, dass Benutzer beim Aktualisieren der `IsAdmin` Eigenschaft und erhöhen Sie selbst auf Administratoren! Die sicherste Strategie besteht darin, eine Modellklasse verwenden, die exakt übereinstimmt, was der Client zum Senden darf:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Brad Wilsons Blog-Post "[Input Validation Vs. Model-Validierung in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)"verfügt über eine gute Erörterung von unzureichende Buchung und zu senden. Obwohl im Beitrag zu ASP.NET MVC 2 ist, sind die Probleme weiterhin relevant, Web-API.


## <a name="handling-validation-errors"></a>Behandeln von Validierungsfehlern

Einen Fehler an den Client werden von Web-API nicht automatisch zurück, wenn die Validierung fehlschlägt. Es liegt im Ermessen der Controlleraktion Modellzustand überprüfen und entsprechend reagieren.

Sie können auch erstellen, einen Aktionsfilter, um dem Modellzustand zu überprüfen, bevor der Controlleraktion aufgerufen wird. Der folgende Code zeigt ein Beispiel:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Wenn die modellvalidierung fehlschlägt, gibt dieser Filter eine HTTP-Antwort, die die Validierungsfehler enthält. In diesem Fall wird der Controlleraktion nicht aufgerufen.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Fügen Sie zum Anwenden dieser Filter auf alle Web-API-Controller eine Instanz des Filters, der die **HttpConfiguration.Filters** Auflistung während der Konfiguration:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Eine andere Möglichkeit besteht in der Filter als Attribut auf einzelnen Domänencontrollern oder Controlleraktionen festgelegt:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
