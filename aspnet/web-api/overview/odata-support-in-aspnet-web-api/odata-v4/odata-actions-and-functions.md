---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Aktionen und Funktionen in OData v4 mithilfe von ASP.NET Web-API 2.2 | Microsoft Docs
author: MikeWasson
description: "In OData sind die Aktionen und Funktionen eine Möglichkeit, serverseitige Verhalten hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind. In diesem Lernprogramm wird gezeigt, wie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Aktionen und Funktionen in OData v4 mithilfe von ASP.NET Web-API 2.2
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> In OData sind die Aktionen und Funktionen eine Möglichkeit, serverseitige Verhalten hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind. Dieses Lernprogramm zeigt, wie ein OData v4-Endpunkt mithilfe von Web-API 2.2 Aktionen und Funktionen hinzu. Das Lernprogramm baut auf das Lernprogramm [erstellen Sie eine OData v4-Endpunkt mit ASP.NET-Web API 2](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - Web-API 2.2
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Lernprogramm-Versionen
> 
> OData-Version 3, finden Sie unter [OData-Aktionen in ASP.NET Web API 2](../odata-v3/odata-actions.md).


Der Unterschied zwischen *Aktionen* und *Funktionen* ist, dass Aktionen können Nebeneffekte haben, und die Funktionen nicht jedoch. Aktionen und Funktionen können Daten zurückgeben. Einige Verwendungsmöglichkeiten für Aktionen aufgeführt:

- Komplexen Transaktionen.
- Bearbeiten gleichzeitig mehrere Entitäten.
- Zulassen, dass Updates nur für bestimmte Eigenschaften einer Entität.
- Senden von Daten, die eine Entität nicht vorhanden ist.

Funktionen sind hilfreich für die Rückgabe von Informationen, die nicht entsprechen direkt an eine Entität oder eine Auflistung.

Eine Aktion (oder Funktion) kann eine einzelne Entität oder eine Auflistung abzielen. In der Terminologie von OData-Dies ist die *Bindung*. Sie können auch veranlassen &quot;ungebundenen&quot; Aktionen/Funktionen, die als statische Vorgänge für den Dienst aufgerufen werden.

## <a name="example-adding-an-action"></a>Beispiel: Eine Aktion hinzufügen

Definieren Sie eine Aktion aus, um ein Produkt bewerten.

> [!NOTE]
> Dieses Lernprogramm baut auf das Lernprogramm [erstellen Sie eine OData v4-Endpunkt mit ASP.NET-Web API 2](create-an-odata-v4-endpoint.md)


Fügen Sie zunächst eine `ProductRating` Modell, um die Bewertungen darstellen.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Auch hinzufügen, eine **DbSet** auf die `ProductsContext` Klasse, sodass EF eine Bewertungen-Tabelle in der Datenbank erstellen.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Fügen Sie die Aktion zum EDM

Fügen Sie in WebApiConfig.cs den folgenden Code hinzu:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

Die **EntityTypeConfiguration.Action** Methode fügt eine Aktion mit den Entity Data Model (EDM). Die **Parameter** Methode gibt einen typisierten Parameter für die Aktion an.

Dieser Code legt auch den Namespace für das EDM fest. Der Namespace ist wichtig, weil der URI für die Aktion der eine vollqualifizierte Aktionsname enthält:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> In einer IIS-Standardkonfiguration wird der Punkt in dieser URL dazu führen, dass IIS den Fehler 404 zurückgeben. Sie können dieses Problem umgehen, indem Sie im folgenden Abschnitt Ihrer Datei "Web.config" hinzufügen:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Fügen Sie einen Controllermethode für die Aktion hinzu

So aktivieren Sie die &quot;Rate&quot; Aktion, fügen Sie die folgende Methode hinzu `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Beachten Sie, dass der Name der Methode der Aktionsname entspricht. Die **[HttpPost]** Attribut gibt an, die Methode ist eine HTTP POST-Methode.

Um die Aktion aufzurufen, sendet der Client eine HTTP POST-Anforderung wie folgt:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

Die &quot;Rate&quot; Aktion an Produktinstanzen, gebunden ist, damit der URI für die Aktion der vollqualifizierten Aktionsname, der auf die Entität URI angefügt ist. (Beachten Sie, dass wir den EDM-Namespace eingerichtet &quot;ProductService&quot;, also der voll gekennzeichneten Aktionsnamen &quot;ProductService.Rate&quot;.)

Der Text der Anforderung enthält der Action-Parameter als ein JSON-Nutzlast. Web-API automatisch konvertiert, die JSON-Nutzlast ein **ODataActionParameters** Objekt, das nur ein Wörterbuch der Parameterwerte. Verwenden Sie dieses Wörterbuch, um die Parameter in der Controllermethode zuzugreifen.

Wenn der Client sendet, der Action-Parameter in der falschen zu formatieren, den Wert der **ModelState.IsValid** lautet "false". Überprüfen Sie dieses Flag in der Controllermethode, und geben Sie einen Fehler zurück, wenn **IsValid** lautet "false".

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Beispiel: Hinzufügen einer Funktion

Nun fügen Sie eine OData-Funktion, die die teuerste Produkt zurückgibt. Wie zuvor im ersten Schritt die Funktion EDM hinzugefügt wird. Fügen Sie in WebApiConfig.cs den folgenden Code ein.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

In diesem Fall wird die Funktion an die Sammlung von Produkten, anstatt einzelne Produktinstanzen gebunden. Clients rufen Sie die Funktion durch Senden einer GET-Anforderung:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

So sieht die Controllermethode für diese Funktion aus:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Beachten Sie, dass der Name der Methode den Namen der Funktion übereinstimmt. Die **[HttpGet]** Attribut gibt an, die Methode ist eine HTTP GET-Methode.

So sieht die HTTP-Antwort aus:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Beispiel: Hinzufügen einer ungebundenen-Funktion

Im vorherige Beispiel wurde eine Funktion, die an eine Auflistung gebunden. Im nächsten Beispiel erstellen wir eine *ungebundenen* Funktion. Nicht gebundene Funktionen werden als statische Vorgänge für den Dienst aufgerufen. Die Funktion in diesem Beispiel wird die Steuer für eine bestimmte Postleitzahl zurück.

Fügen Sie die Funktion zum EDM, in der Datei "webapiconfig":

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Beachten Sie, die wir Aufrufen **Funktion** direkt auf die **ODataModelBuilder**statt der Entitätstyp oder einer Auflistung. Dies weist dem Modell-Generator an, dass die Funktion aufgehoben wird.

Hier ist die Controllermethode, die die Funktion implementiert:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Es spielt keine Rolle, welche Web-API-Controller Sie diese Methode in platzieren. Sie konnten fügen Sie ihn in `ProductsController`, oder definieren Sie einen separaten Controller. Die **[ODataRoute]** Attribut definiert die URI-Vorlage für die Funktion.

Hier ist eine Beispiel für Client-Anforderung:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

Die HTTP-Antwort:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
