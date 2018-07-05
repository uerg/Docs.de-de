---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Aktionen und Funktionen in OData v4 mithilfe von ASP.NET Web API 2.2 | Microsoft-Dokumentation
author: MikeWasson
description: 'In OData können Aktionen und Funktionen: serverseitige Verhaltensweisen hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind. Dieses Tutorial zeigt, wie Sie...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: c78a9acabcb346b33a4fe53aa0d18ef67ddcaf33
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37371962"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a>Aktionen und Funktionen in OData v4 ASP.NET Web API 2.2 verwendet
====================
durch [Mike Wasson](https://github.com/MikeWasson)

> In OData können Aktionen und Funktionen: serverseitige Verhaltensweisen hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind. Dieses Tutorial veranschaulicht das Hinzufügen von Aktionen und Funktionen OData v4-Endpunkt mit Web API 2.2. Das Tutorial baut auf dem Tutorial [erstellen Sie eine OData v4-Endpunkts mit ASP.NET-Web API 2](create-an-odata-v4-endpoint.md)
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
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


Der Unterschied zwischen *Aktionen* und *Funktionen* besteht darin, dass Aktionen Nebeneffekte haben können und Funktionen nicht. Aktionen und Funktionen können Daten zurückgeben. Einige Verwendungsmöglichkeiten für Aktionen umfassen:

- Komplexe Transaktionen.
- Bearbeiten gleichzeitig mehrere Entitäten.
- Erlauben nur bestimmte Eigenschaften einer Entität aktualisiert.
- Senden von Daten, die nicht auf eine Entität ist.

Funktionen sind hilfreich für die Rückgabe von Informationen, der nicht direkt an eine Entität oder eine Auflistung.

Eine Aktion (oder Funktion) kann eine einzelne Entität oder eine Auflistung abzielen. In OData-Terminologie ist dies die *Bindung*. Sie können auch veranlassen &quot;ungebundenen&quot; Aktionen/Funktionen, die als statische Vorgänge im Dienst aufgerufen werden.

## <a name="example-adding-an-action"></a>Beispiel: Hinzufügen einer Aktion

Definieren Sie eine Aktion, um ein Produkt zu bewerten.

> [!NOTE]
> Dieses Tutorial baut auf dem Lernprogramm [erstellen Sie eine OData v4-Endpunkts mit ASP.NET-Web API 2](create-an-odata-v4-endpoint.md)


Fügen Sie zunächst eine `ProductRating` Modell für die Bewertungen darstellen.

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

Hinzufügen einer **"DbSet"** zu der `ProductsContext` Klasse, sodass EF eine Bewertungen-Tabelle in der Datenbank erstellt wird.

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a>Hinzufügen der Aktion zum EDM

Fügen Sie in WebApiConfig.cs den folgenden Code hinzu:

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

Die **EntityTypeConfiguration.Action** Methode fügt eine Aktion mit den Entity Data Model (EDM). Die **Parameter** Methode gibt einen typisierten Parameter für die Aktion an.

Dieser Code legt auch den Namespace für das EDM fest. Der Namespace ist wichtig, weil der URI für die Aktion den vollständig qualifizierten Aktionsnamen enthält:

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> In einer typischen IIS-Konfiguration wird der Punkt in dieser URL dazu führen, dass IIS den Fehler 404 zurückgeben. Sie können dies beheben, indem Sie den folgenden Abschnitt zu Ihrer Datei "Web.config" hinzufügen:

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a>Fügen Sie eine Controllermethode für die Aktion hinzu.

So aktivieren Sie die &quot;Rate&quot; Aktion fügen die folgende Methode zur `ProductsController`:

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

Beachten Sie, dass der Methodenname der Name der Aktion entspricht. Die **[HttpPost]** Attribut gibt an, die Methode ist eine HTTP POST-Methode.

Um die Aktion aufzurufen, sendet der Client eine HTTP POST-Anforderung wie folgt:

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

Die &quot;Rate&quot; Aktion gebunden ist, die Product-Instanzen, der URI für die Aktion der vollqualifizierten Aktionsname, der an die Entität URI angefügt ist. (Denken Sie daran, dass wir die EDM-Namespace, um festgelegt &quot;ProductService&quot;, deshalb ist der Name der Aktion den vollqualifizierten &quot;ProductService.Rate&quot;.)

Der Text der Anforderung enthält der Action-Parameter als JSON-Nutzlast. Web-API automatisch konvertiert die JSON-Nutzlast, die eine **ODataActionParameters** Objekt, das ist einfach ein Wörterbuch der Parameterwerte. Verwenden Sie dieses Wörterbuch, um die Parameter in Ihre Controllermethode zuzugreifen.

Wenn der Client die Aktionsparameter befinden sich am falschen sendet zu formatieren, den Wert der **ModelState.IsValid** ist "false". Aktivieren Sie dieses Flag in der Controllermethode, und geben Sie einen Fehler zurück, wenn **"IsValid"** ist "false".

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a>Beispiel: Hinzufügen einer Funktion

Nun fügen Sie eine OData-Funktion, die die teuerste Produkt zurück. Wie zuvor im ersten Schritt die Funktion EDM hinzugefügt wird. Fügen Sie in WebApiConfig.cs den folgenden Code hinzu.

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

In diesem Fall wird die Funktion an die Produkte, anstatt einzelne Produktinstanzen gebunden. Clients rufen Sie die Funktion durch Senden einer GET-Anforderung:

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

Hier ist der Controllermethode für diese Funktion:

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

Beachten Sie, dass der Name der Methode den Namen der Funktion entspricht. Die **[HttpGet]** Attribut gibt an, die Methode ist eine HTTP GET-Methode.

So sieht die HTTP-Antwort aus:

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a>Beispiel: Hinzufügen einer ungebundenen-Funktion

Im vorherige Beispiel wurde eine Funktion, die an eine Auflistung gebunden. In diesem nächsten Beispiel erstellen wir eine *ungebundenen* Funktion. Nicht gebundene Funktionen werden als statische Vorgänge im Dienst aufgerufen. Die Funktion in diesem Beispiel wird die Mehrwertsteuer für eine bestimmte Postleitzahl zurück.

Fügen Sie die Funktion zum EDM, in der WebApiConfig-Datei:

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

Beachten Sie, die wir Aufrufen **Funktion** direkt auf die **ODataModelBuilder**, statt den Entitätstyp oder einer Auflistung. Dies weist dem Modell-Generator, dass die Funktion aufgehoben wird.

Hier ist der Controllermethode, die die Funktion implementiert:

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

Es spielt keine Rolle, welche Web-API-Controller Sie diese Methode in platzieren. Konnte er platziert wurde `ProductsController`, oder definieren Sie einen separaten Controller. Die **[ODataRoute]** Attribut definiert die URI-Vorlage für die Funktion.

Hier ist eine beispielanforderung für den Client:

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

Die HTTP-Antwort:

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
