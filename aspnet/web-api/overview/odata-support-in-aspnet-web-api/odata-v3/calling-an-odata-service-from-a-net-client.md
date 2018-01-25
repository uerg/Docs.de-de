---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Aufrufen eines OData-Diensts aus einem .NET-Client (c#) | Microsoft Docs
author: MikeWasson
description: Dieses Lernprogramm zeigt, wie auf einen OData-Dienst aus einer C#-Client-Anwendung aufzurufen. Die Versionen der Software verwendet werden, in dem Lernprogramm Visual Studio 2013 (kann mit Visual S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 497102cfa98680f2156a56ff9e36d84b7c820020
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="85b64-104">Aufrufen eines OData-Diensts aus einem .NET-Client (c#)</span><span class="sxs-lookup"><span data-stu-id="85b64-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="85b64-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="85b64-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="85b64-106">Herunterladen des abgeschlossenen Projekts</span><span class="sxs-lookup"><span data-stu-id="85b64-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="85b64-107">Dieses Lernprogramm zeigt, wie auf einen OData-Dienst aus einer C#-Client-Anwendung aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="85b64-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="85b64-108">In diesem Lernprogramm verwendeten Versionen der Software</span><span class="sxs-lookup"><span data-stu-id="85b64-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="85b64-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funktioniert mit Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="85b64-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="85b64-110">WCF Data Services-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="85b64-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="85b64-111">Web API 2.</span><span class="sxs-lookup"><span data-stu-id="85b64-111">Web API 2.</span></span> <span data-ttu-id="85b64-112">(Im Beispiel OData-Dienst wird mithilfe von Web-API 2 erstellt, aber die Clientanwendung hängt nicht von Web-API.)</span><span class="sxs-lookup"><span data-stu-id="85b64-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="85b64-113">In diesem Lernprogramm durchgehen ich Erstellen einer Clientanwendung, die einen OData-Dienst aufruft.</span><span class="sxs-lookup"><span data-stu-id="85b64-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="85b64-114">Die OData-Dienst macht die folgenden Elemente:</span><span class="sxs-lookup"><span data-stu-id="85b64-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="85b64-115">In den folgenden Artikeln wird beschrieben, wie die OData-Dienst im Web-API implementiert.</span><span class="sxs-lookup"><span data-stu-id="85b64-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="85b64-116">(Sie müssen nicht gelesen werden, um dieses Lernprogramm jedoch zu verstehen.)</span><span class="sxs-lookup"><span data-stu-id="85b64-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="85b64-117">Erstellen einen OData-Endpunkt in der Web-API 2</span><span class="sxs-lookup"><span data-stu-id="85b64-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="85b64-118">OData-Entitätsbeziehungen in Web-API 2</span><span class="sxs-lookup"><span data-stu-id="85b64-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="85b64-119">OData-Aktionen in der Web-API 2</span><span class="sxs-lookup"><span data-stu-id="85b64-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="85b64-120">Generieren des Webdienstproxys</span><span class="sxs-lookup"><span data-stu-id="85b64-120">Generate the Service Proxy</span></span>

<span data-ttu-id="85b64-121">Der erste Schritt besteht darin einen neuer Proxy zu generieren.</span><span class="sxs-lookup"><span data-stu-id="85b64-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="85b64-122">Der Dienstproxy ist eine .NET-Klasse, die Methoden für den Zugriff auf den OData-Dienst definiert.</span><span class="sxs-lookup"><span data-stu-id="85b64-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="85b64-123">Der Proxy übersetzt Methodenaufrufe in HTTP-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="85b64-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="85b64-124">Öffnen das OData-Dienst-Projekt in Visual Studio starten.</span><span class="sxs-lookup"><span data-stu-id="85b64-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="85b64-125">Drücken Sie STRG + F5, um den Dienst in IIS Express lokal auszuführen.</span><span class="sxs-lookup"><span data-stu-id="85b64-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="85b64-126">Beachten Sie die lokale Adresse, einschließlich der Portnummer, die Visual Studio zuweist.</span><span class="sxs-lookup"><span data-stu-id="85b64-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="85b64-127">Sie benötigen diese Adresse, wenn Sie den Proxy erstellt.</span><span class="sxs-lookup"><span data-stu-id="85b64-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="85b64-128">Als Nächstes öffnen Sie eine andere Instanz von Visual Studio, und erstellen Sie ein Konsolenanwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="85b64-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="85b64-129">Die Konsolenanwendung werden die OData-Clientanwendung.</span><span class="sxs-lookup"><span data-stu-id="85b64-129">The console application will be our OData client application.</span></span> <span data-ttu-id="85b64-130">(Sie können auch das Projekt der gleichen Lösung wie der Dienst hinzufügen.)</span><span class="sxs-lookup"><span data-stu-id="85b64-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="85b64-131">Die restlichen Schritte finden Sie unter der Konsolenprojekt.</span><span class="sxs-lookup"><span data-stu-id="85b64-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="85b64-132">Klicken Sie im Projektmappen-Explorer mit der Maustaste **Verweise** , und wählen Sie **Hinzufügen eines Dienstverweises**.</span><span class="sxs-lookup"><span data-stu-id="85b64-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="85b64-133">In der **Hinzufügen eines Dienstverweises** Dialogfeld, geben Sie die Adresse des OData-Diensts:</span><span class="sxs-lookup"><span data-stu-id="85b64-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="85b64-134">wobei *Port* ist die Portnummer.</span><span class="sxs-lookup"><span data-stu-id="85b64-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="85b64-135">Für **Namespace**, geben Sie "ProductService".</span><span class="sxs-lookup"><span data-stu-id="85b64-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="85b64-136">Diese Option wird den Namespace der Proxyklasse definiert.</span><span class="sxs-lookup"><span data-stu-id="85b64-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="85b64-137">Klicken Sie auf **Go**.</span><span class="sxs-lookup"><span data-stu-id="85b64-137">Click **Go**.</span></span> <span data-ttu-id="85b64-138">Visual Studio liest die OData-Metadatendokument um die Entitäten im Dienst zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="85b64-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="85b64-139">Klicken Sie auf **OK** die Proxyklasse zu Ihrem Projekt hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="85b64-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="85b64-140">Erstellen Sie eine Instanz der Proxyklasse Service</span><span class="sxs-lookup"><span data-stu-id="85b64-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="85b64-141">Innerhalb der `Main` -Methode, erstellen Sie eine neue Instanz der Proxyklasse, wie folgt:</span><span class="sxs-lookup"><span data-stu-id="85b64-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="85b64-142">Erneut, verwenden Sie die Anzahl der tatsächlich verwendeten Port, auf dem der Dienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="85b64-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="85b64-143">Wenn Sie den Dienst bereitstellen, verwenden Sie den URI des live-Diensts.</span><span class="sxs-lookup"><span data-stu-id="85b64-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="85b64-144">Sie müssen nicht den Proxy zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="85b64-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="85b64-145">Der folgende Code Fügt einen Ereignishandler, der die Anforderungs-URIs an das Konsolenfenster ausgibt.</span><span class="sxs-lookup"><span data-stu-id="85b64-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="85b64-146">Dieser Schritt ist nicht erforderlich, aber es ist interessant, die URIs für jede Abfrage finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="85b64-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="85b64-147">Abfragen des Diensts</span><span class="sxs-lookup"><span data-stu-id="85b64-147">Query the Service</span></span>

<span data-ttu-id="85b64-148">Der folgende Code Ruft die Liste der Produkte aus der OData-Dienst ab.</span><span class="sxs-lookup"><span data-stu-id="85b64-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="85b64-149">Beachten Sie, dass Sie nicht zum Senden von HTTP-Anforderung oder Antwort analysieren Code schreiben müssen.</span><span class="sxs-lookup"><span data-stu-id="85b64-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="85b64-150">Die Proxy-Klasse wird automatisch für die Sie auflisten, wenn die `Container.Products` Sammlung in der **Foreach** Schleife.</span><span class="sxs-lookup"><span data-stu-id="85b64-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="85b64-151">Wenn Sie die Anwendung ausführen, sollte die Ausgabe wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="85b64-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="85b64-152">Verwenden Sie zum Abrufen einer Entität nach der ID einer `where` Klausel.</span><span class="sxs-lookup"><span data-stu-id="85b64-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="85b64-153">Für den Rest dieses Themas, ich zeigen nicht an die gesamte `Main` -Funktion, den Code zum Aufrufen des Diensts erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="85b64-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="85b64-154">Anwenden von Abfrageoptionen</span><span class="sxs-lookup"><span data-stu-id="85b64-154">Apply Query Options</span></span>

<span data-ttu-id="85b64-155">OData definiert [Abfrageoptionen](../supporting-odata-query-options.md) , der zum Filtern, sortieren, Seitendaten usw. verwendet werden können.</span><span class="sxs-lookup"><span data-stu-id="85b64-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="85b64-156">In den Dienstproxy können Sie diese Optionen anwenden, mit verschiedenen LINQ-Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="85b64-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="85b64-157">In diesem Abschnitt zeige ich kurze Beispiele.</span><span class="sxs-lookup"><span data-stu-id="85b64-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="85b64-158">Weitere Informationen finden Sie im Thema [Überlegungen zu LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) auf MSDN.</span><span class="sxs-lookup"><span data-stu-id="85b64-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="85b64-159">Filtern ($filter)</span><span class="sxs-lookup"><span data-stu-id="85b64-159">Filtering ($filter)</span></span>

<span data-ttu-id="85b64-160">Verwenden Sie zum Filtern einer `where` Klausel.</span><span class="sxs-lookup"><span data-stu-id="85b64-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="85b64-161">Im folgenden Beispiel wird gefiltert nach Produktkategorie.</span><span class="sxs-lookup"><span data-stu-id="85b64-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="85b64-162">Dieser Code entspricht den folgenden OData-Abfrage.</span><span class="sxs-lookup"><span data-stu-id="85b64-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="85b64-163">Beachten Sie, die der Proxy konvertiert die `where` Klausel in einer OData `$filter` Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="85b64-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="85b64-164">Sortierung ($orderby)</span><span class="sxs-lookup"><span data-stu-id="85b64-164">Sorting ($orderby)</span></span>

<span data-ttu-id="85b64-165">Verwenden Sie zum Sortieren einer `orderby` Klausel.</span><span class="sxs-lookup"><span data-stu-id="85b64-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="85b64-166">Im folgende Beispiel wird nach dem Preis, vom höchsten zum niedrigsten sortiert.</span><span class="sxs-lookup"><span data-stu-id="85b64-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="85b64-167">Im folgenden wird die entsprechende OData-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="85b64-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="85b64-168">Clientseitige Auslagerung ("$skip" und "$top")</span><span class="sxs-lookup"><span data-stu-id="85b64-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="85b64-169">Bei großen Entitätssätzen sollten der Client die Anzahl der Ergebnisse zu begrenzen.</span><span class="sxs-lookup"><span data-stu-id="85b64-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="85b64-170">Beispielsweise kann ein Client 10 Einträge zu einem Zeitpunkt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="85b64-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="85b64-171">Hierbei spricht *clientseitigen Auslagerung*.</span><span class="sxs-lookup"><span data-stu-id="85b64-171">This is called *client-side paging*.</span></span> <span data-ttu-id="85b64-172">(Es gibt auch [serverseitiges Paging](../supporting-odata-query-options.md#server-paging), in denen der Server die Anzahl der Ergebnisse beschränkt.) Verwenden Sie zum Ausführen einer clientseitigen Auslagerung LINQ **Skip** und **dauern** Methoden.</span><span class="sxs-lookup"><span data-stu-id="85b64-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="85b64-173">Im folgenden Beispiel überspringt die ersten 40 Ergebnisse und die nächsten 10 akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="85b64-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="85b64-174">Hier wird die entsprechende OData-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="85b64-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="85b64-175">Auswählen ($select), und erweitern Sie diese Option (expand, $)</span><span class="sxs-lookup"><span data-stu-id="85b64-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="85b64-176">Um verknüpfte Entitäten einzuschließen, verwenden die `DataServiceQuery<t>.Expand` Methode.</span><span class="sxs-lookup"><span data-stu-id="85b64-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="85b64-177">Beispielsweise enthalten die `Supplier` für jede `Product`:</span><span class="sxs-lookup"><span data-stu-id="85b64-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="85b64-178">Hier wird die entsprechende OData-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="85b64-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="85b64-179">Um die Form der Antwort zu ändern, verwenden Sie die LINQ **wählen** Klausel.</span><span class="sxs-lookup"><span data-stu-id="85b64-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="85b64-180">Im folgenden Beispiel wird nur der Name jedes Produkts, ohne andere Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="85b64-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="85b64-181">Hier wird die entsprechende OData-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="85b64-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="85b64-182">Eine select-Klausel kann es sich um verknüpfte Entitäten enthalten.</span><span class="sxs-lookup"><span data-stu-id="85b64-182">A select clause can include related entities.</span></span> <span data-ttu-id="85b64-183">Rufen Sie in diesem Fall nicht **erweitern**; der Proxy schließt automatisch die Erweiterung in diesem Fall.</span><span class="sxs-lookup"><span data-stu-id="85b64-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="85b64-184">Das folgende Beispiel ruft den Namen und den Hersteller der einzelnen Produkte.</span><span class="sxs-lookup"><span data-stu-id="85b64-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="85b64-185">Im folgenden wird die entsprechende OData-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="85b64-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="85b64-186">Beachten Sie, die dieses enthält die **$expand-** Option.</span><span class="sxs-lookup"><span data-stu-id="85b64-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="85b64-187">Weitere Informationen zu $select und $erweitern, finden Sie unter [mit $select, $expand, und $value in Web-API 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="85b64-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="85b64-188">Fügen Sie eine neue Entität hinzu.</span><span class="sxs-lookup"><span data-stu-id="85b64-188">Add a New Entity</span></span>

<span data-ttu-id="85b64-189">Um eine neue Entität für eine entitätsmenge hinzuzufügen, rufen Sie `AddToEntitySet`, wobei *EntitySet* ist der Name der Entitätenmenge.</span><span class="sxs-lookup"><span data-stu-id="85b64-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="85b64-190">Beispielsweise `AddToProducts` wird ein neues `Product` auf die `Products` Entitätenmenge.</span><span class="sxs-lookup"><span data-stu-id="85b64-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="85b64-191">Wenn Sie den Proxy zu generieren, WCF Data Services erstellt automatisch diese stark typisierte **AddTo** Methoden.</span><span class="sxs-lookup"><span data-stu-id="85b64-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="85b64-192">Verwenden Sie zum Hinzufügen einer Verknüpfung zwischen zwei Entitäten die **AddLink** und **SetLink** Methoden.</span><span class="sxs-lookup"><span data-stu-id="85b64-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="85b64-193">Der folgende Code Fügt einen neuen Lieferanten und ein neues Produkt hinzu und erstellt dann Links zwischen diesen.</span><span class="sxs-lookup"><span data-stu-id="85b64-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="85b64-194">Verwendung **AddLink** Wenn die Navigationseigenschaft eine Auflistung ist.</span><span class="sxs-lookup"><span data-stu-id="85b64-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="85b64-195">In diesem Beispiel werden wir ein Produkt hinzufügen der `Products` Auflistung für den Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="85b64-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="85b64-196">Verwendung **SetLink** die Navigationseigenschaft ist bei einer einzelnen Entität.</span><span class="sxs-lookup"><span data-stu-id="85b64-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="85b64-197">In diesem Beispiel werden wir das Festlegen der `Supplier` auf der Produkt-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="85b64-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="85b64-198">Update / Patch</span><span class="sxs-lookup"><span data-stu-id="85b64-198">Update / Patch</span></span>

<span data-ttu-id="85b64-199">Um eine Entität zu aktualisieren, rufen die **UpdateObject** Methode.</span><span class="sxs-lookup"><span data-stu-id="85b64-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="85b64-200">Das Update wird ausgeführt, wenn Sie rufen **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="85b64-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="85b64-201">Standardmäßig sendet WCF eine HTTP MERGE-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="85b64-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="85b64-202">Die **PatchOnUpdate** Option weist die WCF zum Senden einer HTTP-PATCH.</span><span class="sxs-lookup"><span data-stu-id="85b64-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="85b64-203">Warum PATCH im Vergleich zu verbinden?</span><span class="sxs-lookup"><span data-stu-id="85b64-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="85b64-204">Die ursprüngliche HTTP 1.1-Spezifikation ([RCF 2616](http://tools.ietf.org/html/rfc2616)) alle HTTP-Methode mit der Semantik von "teilweises Update" wurde nicht definiert.</span><span class="sxs-lookup"><span data-stu-id="85b64-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="85b64-205">Unterstützung von teilupdates, definiert die OData-Spezifikation die MERGE-Methode.</span><span class="sxs-lookup"><span data-stu-id="85b64-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="85b64-206">In 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) definiert die PATCH-Methode für teilweise Updates.</span><span class="sxs-lookup"><span data-stu-id="85b64-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="85b64-207">Erfahren Sie einige der Verlauf in diesem [Blogbeitrag](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) auf die WCF Data Services-Blog.</span><span class="sxs-lookup"><span data-stu-id="85b64-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="85b64-208">Heute sind PATCH über MERGE bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="85b64-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="85b64-209">Der OData-Controller erstellt, indem das Gerüst für die Web-API unterstützt beide Methoden.</span><span class="sxs-lookup"><span data-stu-id="85b64-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="85b64-210">Wenn Sie die gesamte Entität (PUT-Semantik) ersetzen möchten, geben Sie die **ReplaceOnUpdate** Option.</span><span class="sxs-lookup"><span data-stu-id="85b64-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="85b64-211">Dies bewirkt, dass WCF zum Senden einer HTTP PUT-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="85b64-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="85b64-212">Löschen einer Entität</span><span class="sxs-lookup"><span data-stu-id="85b64-212">Delete an Entity</span></span>

<span data-ttu-id="85b64-213">Um eine Entität zu löschen, rufen **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="85b64-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="85b64-214">Rufen Sie eine OData-Aktion</span><span class="sxs-lookup"><span data-stu-id="85b64-214">Invoke an OData Action</span></span>

<span data-ttu-id="85b64-215">In OData [Aktionen](odata-actions.md) bieten eine Möglichkeit, serverseitige Verhalten hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind.</span><span class="sxs-lookup"><span data-stu-id="85b64-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="85b64-216">Obwohl das Metadatendokument des OData-Aktionen beschrieben, erstellt die Proxyklasse keine stark typisierte Methoden nicht für sie.</span><span class="sxs-lookup"><span data-stu-id="85b64-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="85b64-217">Sie können immer noch eine OData-Aktion aufrufen, indem die generischen **Execute** Methode.</span><span class="sxs-lookup"><span data-stu-id="85b64-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="85b64-218">Allerdings müssen Sie wissen, die Datentypen der Parameter und der Rückgabewert.</span><span class="sxs-lookup"><span data-stu-id="85b64-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="85b64-219">Z. B. die `RateProduct` Aktion akzeptiert Parameter, die mit dem Namen "Bewertung" vom Typ `Int32` und gibt eine `double`.</span><span class="sxs-lookup"><span data-stu-id="85b64-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="85b64-220">Der folgende Code zeigt, wie diese Aktion aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="85b64-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="85b64-221">Weitere Informationen finden Sie unter[Dienstvorgänge aufrufen und Aktionen](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="85b64-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="85b64-222">Eine Möglichkeit besteht darin, Erweitern der **Container** Klasse eine stark typisierte Methode bereitstellen, die die Aktion aufruft:</span><span class="sxs-lookup"><span data-stu-id="85b64-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
