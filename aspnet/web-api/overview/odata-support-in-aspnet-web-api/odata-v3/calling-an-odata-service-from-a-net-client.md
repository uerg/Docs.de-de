---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Aufrufen eines OData-Diensts aus einem .NET-Client (c#) | Microsoft-Dokumentation
author: MikeWasson
description: Dieses Tutorial zeigt, wie auf einen OData-Dienst über eine C#-Client-Anwendung aufgerufen wird. Softwareversionen, die verwendet werden, in dem Tutorial Visual Studio 2013 (kann mit Visual S...
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 45d4d1304d2c628d2ab0d09925d2050bf6f58c60
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831268"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="31eb4-104">Aufrufen eines OData-Diensts aus einem .NET-Client (c#)</span><span class="sxs-lookup"><span data-stu-id="31eb4-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="31eb4-105">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="31eb4-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="31eb4-106">Abgeschlossenes Projekt herunterladen</span><span class="sxs-lookup"><span data-stu-id="31eb4-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="31eb4-107">Dieses Tutorial zeigt, wie auf einen OData-Dienst über eine C#-Client-Anwendung aufgerufen wird.</span><span class="sxs-lookup"><span data-stu-id="31eb4-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="31eb4-108">Softwareversionen, die in diesem Tutorial verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="31eb4-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="31eb4-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (funktioniert mit Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="31eb4-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="31eb4-110">WCF Data Services-Clientbibliothek</span><span class="sxs-lookup"><span data-stu-id="31eb4-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="31eb4-111">Web-API 2.</span><span class="sxs-lookup"><span data-stu-id="31eb4-111">Web API 2.</span></span> <span data-ttu-id="31eb4-112">(Im Beispiel OData-Dienst wird mithilfe von Web-API 2 erstellt, die Client-Anwendung ist jedoch nicht erforderlich für Web-API.)</span><span class="sxs-lookup"><span data-stu-id="31eb4-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="31eb4-113">In diesem Tutorial erläutere ich, wie Sie erstellen eine Clientanwendung, die einen OData-Dienst aufruft.</span><span class="sxs-lookup"><span data-stu-id="31eb4-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="31eb4-114">Der OData-Dienst stellt die folgenden Entitäten:</span><span class="sxs-lookup"><span data-stu-id="31eb4-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="31eb4-115">In den folgenden Artikeln wird beschrieben, wie den OData-Dienst in Web-API zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="31eb4-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="31eb4-116">(Sie müssen nicht gelesen werden, damit dieses Lernprogramm jedoch klar ist.)</span><span class="sxs-lookup"><span data-stu-id="31eb4-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="31eb4-117">Erstellen eines OData-Endpunkts in Web-API 2</span><span class="sxs-lookup"><span data-stu-id="31eb4-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="31eb4-118">OData-Entität-Beziehungen in der Web-API 2</span><span class="sxs-lookup"><span data-stu-id="31eb4-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="31eb4-119">OData-Aktionen in der Web-API 2</span><span class="sxs-lookup"><span data-stu-id="31eb4-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="31eb4-120">Generieren des Webdienstproxys</span><span class="sxs-lookup"><span data-stu-id="31eb4-120">Generate the Service Proxy</span></span>

<span data-ttu-id="31eb4-121">Der erste Schritt ist einen neuer Proxy zu generieren.</span><span class="sxs-lookup"><span data-stu-id="31eb4-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="31eb4-122">Der-Dienstproxy ist eine .NET-Klasse, die Methoden für den Zugriff auf den OData-Dienst definiert.</span><span class="sxs-lookup"><span data-stu-id="31eb4-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="31eb4-123">Der Proxy übersetzt Methodenaufrufe in HTTP-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="31eb4-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="31eb4-124">Zunächst öffnen das OData-Dienst-Projekt in Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="31eb4-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="31eb4-125">Drücken Sie STRG + F5, um den Dienst lokal in IIS Express ausführen.</span><span class="sxs-lookup"><span data-stu-id="31eb4-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="31eb4-126">Beachten Sie die lokale Adresse, einschließlich der Portnummer, die von Visual Studio zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="31eb4-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="31eb4-127">Sie benötigen diese Adresse, wenn Sie den Proxy erstellen.</span><span class="sxs-lookup"><span data-stu-id="31eb4-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="31eb4-128">Als Nächstes öffnen Sie eine andere Instanz von Visual Studio, und erstellen Sie ein Konsolenanwendungsprojekt.</span><span class="sxs-lookup"><span data-stu-id="31eb4-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="31eb4-129">Die Konsolenanwendung werden die OData-Client-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="31eb4-129">The console application will be our OData client application.</span></span> <span data-ttu-id="31eb4-130">(Sie können auch das Projekt der gleichen Projektmappe wie der Dienst hinzufügen.)</span><span class="sxs-lookup"><span data-stu-id="31eb4-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="31eb4-131">Die verbleibenden Schritte finden Sie das Konsolen-Projekt.</span><span class="sxs-lookup"><span data-stu-id="31eb4-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="31eb4-132">Klicken Sie im Projektmappen-Explorer mit der Maustaste **Verweise** , und wählen Sie **Hinzufügen eines Dienstverweises**.</span><span class="sxs-lookup"><span data-stu-id="31eb4-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="31eb4-133">In der **Hinzufügen eines Dienstverweises** Dialogfeld Geben Sie die Adresse des OData-Diensts:</span><span class="sxs-lookup"><span data-stu-id="31eb4-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="31eb4-134">wo *Port* gibt die Portnummer an.</span><span class="sxs-lookup"><span data-stu-id="31eb4-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="31eb4-135">Für **Namespace**, geben Sie "ProductService".</span><span class="sxs-lookup"><span data-stu-id="31eb4-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="31eb4-136">Diese Option definiert den Namespace der Proxyklasse.</span><span class="sxs-lookup"><span data-stu-id="31eb4-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="31eb4-137">Klicken Sie auf **Go**.</span><span class="sxs-lookup"><span data-stu-id="31eb4-137">Click **Go**.</span></span> <span data-ttu-id="31eb4-138">Visual Studio liest Metadatendokument des OData, um die Entitäten in den Dienst zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="31eb4-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="31eb4-139">Klicken Sie auf **OK** die Proxyklasse zu Ihrem Projekt hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="31eb4-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="31eb4-140">Erstellen Sie eine Instanz der Proxyklasse Service</span><span class="sxs-lookup"><span data-stu-id="31eb4-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="31eb4-141">Innerhalb Ihrer `Main` -Methode, erstellen Sie eine neue Instanz der Proxyklasse, wie folgt:</span><span class="sxs-lookup"><span data-stu-id="31eb4-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="31eb4-142">In diesem Fall verwenden Sie die tatsächliche Portnummer an, in dem der Dienst ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="31eb4-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="31eb4-143">Wenn Sie Ihren Dienst bereitstellen, verwenden Sie den URI des live-Diensts.</span><span class="sxs-lookup"><span data-stu-id="31eb4-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="31eb4-144">Sie müssen nicht den Proxy zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="31eb4-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="31eb4-145">Der folgende Code Fügt einen Ereignishandler, der die Anforderungs-URIs in das Konsolenfenster ausgibt.</span><span class="sxs-lookup"><span data-stu-id="31eb4-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="31eb4-146">Dieser Schritt ist nicht erforderlich, aber es ist interessant, die URIs für jede Abfrage finden Sie unter.</span><span class="sxs-lookup"><span data-stu-id="31eb4-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="31eb4-147">Abfragen des Diensts</span><span class="sxs-lookup"><span data-stu-id="31eb4-147">Query the Service</span></span>

<span data-ttu-id="31eb4-148">Der folgende Code Ruft die Liste der Produkte ab, aus dem OData-Dienst.</span><span class="sxs-lookup"><span data-stu-id="31eb4-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="31eb4-149">Beachten Sie, dass Sie nicht zum Schreiben von Code zum Senden der HTTP-Anforderung oder Antwort analysiert werden müssen.</span><span class="sxs-lookup"><span data-stu-id="31eb4-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="31eb4-150">Die Proxyklasse wird automatisch für die Sie auflisten, wenn die `Container.Products` Sammlung in der **Foreach** Schleife.</span><span class="sxs-lookup"><span data-stu-id="31eb4-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="31eb4-151">Wenn Sie die Anwendung ausführen, sollte die Ausgabe wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="31eb4-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="31eb4-152">Verwenden Sie zum Abrufen einer Entität nach der ID einer `where` Klausel.</span><span class="sxs-lookup"><span data-stu-id="31eb4-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="31eb4-153">Für den Rest dieses Themas, ich nicht die gesamte angezeigt `Main` Funktion, den Code zum Aufrufen des Diensts erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="31eb4-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="31eb4-154">Anwenden von Abfrageoptionen</span><span class="sxs-lookup"><span data-stu-id="31eb4-154">Apply Query Options</span></span>

<span data-ttu-id="31eb4-155">OData definiert [Abfrageoptionen](../supporting-odata-query-options.md) , die zum Filtern, sortieren, Seitendaten und So weiter verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="31eb4-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="31eb4-156">In der Proxyklasse können Sie diese Optionen anwenden, mit verschiedenen LINQ-Ausdrücke.</span><span class="sxs-lookup"><span data-stu-id="31eb4-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="31eb4-157">In diesem Abschnitt zeige ich, kurze Beispiele für.</span><span class="sxs-lookup"><span data-stu-id="31eb4-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="31eb4-158">Weitere Informationen finden Sie im Thema [Überlegungen zu LINQ (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) auf MSDN.</span><span class="sxs-lookup"><span data-stu-id="31eb4-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="31eb4-159">Filtern ($filter)</span><span class="sxs-lookup"><span data-stu-id="31eb4-159">Filtering ($filter)</span></span>

<span data-ttu-id="31eb4-160">Verwenden Sie zum Filtern einer `where` Klausel.</span><span class="sxs-lookup"><span data-stu-id="31eb4-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="31eb4-161">Das folgende Beispiel filtert nach Produktkategorie.</span><span class="sxs-lookup"><span data-stu-id="31eb4-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="31eb4-162">Dieser Code entspricht der folgenden OData-Abfrage.</span><span class="sxs-lookup"><span data-stu-id="31eb4-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="31eb4-163">Beachten Sie, die der Proxy konvertiert die `where` Klausel für einen OData- `$filter` Ausdruck.</span><span class="sxs-lookup"><span data-stu-id="31eb4-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="31eb4-164">Sortierung ($orderby)</span><span class="sxs-lookup"><span data-stu-id="31eb4-164">Sorting ($orderby)</span></span>

<span data-ttu-id="31eb4-165">Verwenden Sie zum Sortieren einer `orderby` Klausel.</span><span class="sxs-lookup"><span data-stu-id="31eb4-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="31eb4-166">Im folgenden Beispiel wird sortiert nach Preis, vom höchsten zum niedrigsten.</span><span class="sxs-lookup"><span data-stu-id="31eb4-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="31eb4-167">Hier ist die entsprechenden OData-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="31eb4-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="31eb4-168">Clientseitige Auslagerung ("$skip" und "$top")</span><span class="sxs-lookup"><span data-stu-id="31eb4-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="31eb4-169">Bei großen Entitätssätzen sollten der Client die Anzahl der Ergebnisse zu beschränken.</span><span class="sxs-lookup"><span data-stu-id="31eb4-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="31eb4-170">Beispielsweise kann ein Client 10 Einträge zu einem Zeitpunkt anzeigen.</span><span class="sxs-lookup"><span data-stu-id="31eb4-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="31eb4-171">Dies wird als bezeichnet *clientseitigen Auslagerung*.</span><span class="sxs-lookup"><span data-stu-id="31eb4-171">This is called *client-side paging*.</span></span> <span data-ttu-id="31eb4-172">(Es gibt auch [serverseitiges Paging](../supporting-odata-query-options.md#server-paging), in denen der Server die Anzahl der Ergebnisse beschränkt.) Verwenden Sie zum Ausführen der clientseitigen Auslagerung LINQ **überspringen** und **dauern** Methoden.</span><span class="sxs-lookup"><span data-stu-id="31eb4-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="31eb4-173">Im folgenden Beispiel überspringt die ersten 40 Ergebnisse und die nächsten 10 verwendet.</span><span class="sxs-lookup"><span data-stu-id="31eb4-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="31eb4-174">So sieht die entsprechenden OData-Anforderung aus:</span><span class="sxs-lookup"><span data-stu-id="31eb4-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="31eb4-175">Option ($select), und erweitern ($expand)</span><span class="sxs-lookup"><span data-stu-id="31eb4-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="31eb4-176">Um verknüpfte Entitäten einzuschließen, verwenden die `DataServiceQuery<t>.Expand` Methode.</span><span class="sxs-lookup"><span data-stu-id="31eb4-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="31eb4-177">Beispielsweise enthalten die `Supplier` für jede `Product`:</span><span class="sxs-lookup"><span data-stu-id="31eb4-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="31eb4-178">So sieht die entsprechenden OData-Anforderung aus:</span><span class="sxs-lookup"><span data-stu-id="31eb4-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="31eb4-179">Um die Form der Antwort zu ändern, verwenden Sie LINQ **wählen** Klausel.</span><span class="sxs-lookup"><span data-stu-id="31eb4-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="31eb4-180">Im folgenden Beispiel wird nur der Name jedes Produkts, ohne andere Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="31eb4-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="31eb4-181">So sieht die entsprechenden OData-Anforderung aus:</span><span class="sxs-lookup"><span data-stu-id="31eb4-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="31eb4-182">Eine select-Klausel kann es sich um verknüpfte Entitäten enthalten.</span><span class="sxs-lookup"><span data-stu-id="31eb4-182">A select clause can include related entities.</span></span> <span data-ttu-id="31eb4-183">Rufen Sie in diesem Fall nicht **erweitern**; der Proxy schließt automatisch die Erweiterung in diesem Fall.</span><span class="sxs-lookup"><span data-stu-id="31eb4-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="31eb4-184">Im folgende Beispiel ruft den Namen und den Lieferanten für jedes Produkt ab.</span><span class="sxs-lookup"><span data-stu-id="31eb4-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="31eb4-185">Hier ist die entsprechenden OData-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="31eb4-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="31eb4-186">Beachten Sie, die es enthält die **$expand-** Option.</span><span class="sxs-lookup"><span data-stu-id="31eb4-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="31eb4-187">Weitere Informationen zu $select und $expand erweitern, finden Sie unter [verwenden $select, $expand, und $value in Web-API 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="31eb4-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="31eb4-188">Eine neue Entität hinzufügen</span><span class="sxs-lookup"><span data-stu-id="31eb4-188">Add a New Entity</span></span>

<span data-ttu-id="31eb4-189">Um eine neue Entität für eine entitätsmenge hinzuzufügen, rufen `AddToEntitySet`, wobei *EntitySet* ist der Name der Entitätenmenge.</span><span class="sxs-lookup"><span data-stu-id="31eb4-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="31eb4-190">Z. B. `AddToProducts` Fügt ein neues `Product` auf die `Products` Entitätenmenge.</span><span class="sxs-lookup"><span data-stu-id="31eb4-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="31eb4-191">Wenn Sie den Proxy zu generieren, WCF Data Services erstellt automatisch diese stark typisierte **AddTo** Methoden.</span><span class="sxs-lookup"><span data-stu-id="31eb4-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="31eb4-192">Verwenden Sie zum Hinzufügen einer Verknüpfung zwischen zwei Entitäten die **AddLink** und **SetLink** Methoden.</span><span class="sxs-lookup"><span data-stu-id="31eb4-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="31eb4-193">Der folgende Code Fügt einen neuen Lieferanten und ein neues Produkt hinzu und erstellt dann Links zwischen diesen beiden.</span><span class="sxs-lookup"><span data-stu-id="31eb4-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="31eb4-194">Verwendung **AddLink** bei die Navigationseigenschaft eine Auflistung ist.</span><span class="sxs-lookup"><span data-stu-id="31eb4-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="31eb4-195">In diesem Beispiel werden wir ein Produkt hinzufügen der `Products` Auflistung auf den Lieferanten.</span><span class="sxs-lookup"><span data-stu-id="31eb4-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="31eb4-196">Verwendung **SetLink** bei die Navigationseigenschaft eine einzelne Entität ist.</span><span class="sxs-lookup"><span data-stu-id="31eb4-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="31eb4-197">In diesem Beispiel legen wir die `Supplier` Eigenschaft für das Produkt.</span><span class="sxs-lookup"><span data-stu-id="31eb4-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="31eb4-198">Aktualisieren und Patchen</span><span class="sxs-lookup"><span data-stu-id="31eb4-198">Update / Patch</span></span>

<span data-ttu-id="31eb4-199">Rufen Sie zum Aktualisieren einer Entitätstyps die **UpdateObject** Methode.</span><span class="sxs-lookup"><span data-stu-id="31eb4-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="31eb4-200">Die Aktualisierung wird ausgeführt, wenn Sie aufrufen **"SaveChanges"**.</span><span class="sxs-lookup"><span data-stu-id="31eb4-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="31eb4-201">Standardmäßig sendet WCF eine HTTP MERGE-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="31eb4-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="31eb4-202">Die **PatchOnUpdate** -Option weist WCF an, die eine HTTP-PATCH stattdessen senden.</span><span class="sxs-lookup"><span data-stu-id="31eb4-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="31eb4-203">Warum im Vergleich zu MERGE PATCH?</span><span class="sxs-lookup"><span data-stu-id="31eb4-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="31eb4-204">Die ursprüngliche HTTP 1.1-Spezifikation ([RCF 2616](http://tools.ietf.org/html/rfc2616)) keiner HTTP-Methode, mit der Semantik von "partielle Aktualisierung" definieren.</span><span class="sxs-lookup"><span data-stu-id="31eb4-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="31eb4-205">Die OData-Spezifikation definiert die MERGE-Methode, um teilupdates zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="31eb4-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="31eb4-206">In 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) definiert die PATCH-Methode für die partielle Aktualisierungen.</span><span class="sxs-lookup"><span data-stu-id="31eb4-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="31eb4-207">Finden Sie einige der in diesem Verlauf [Blogbeitrag](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) auf die WCF Data Services-Blog.</span><span class="sxs-lookup"><span data-stu-id="31eb4-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="31eb4-208">PATCH wird heute über MERGE bevorzugt.</span><span class="sxs-lookup"><span data-stu-id="31eb4-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="31eb4-209">Die von der Web-API-Gerüstbau erstellten OData-Controller unterstützt beide Methoden.</span><span class="sxs-lookup"><span data-stu-id="31eb4-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="31eb4-210">Wenn Sie die gesamte Entität (PUT-Semantik) ersetzen möchten, geben Sie die **ReplaceOnUpdate** Option.</span><span class="sxs-lookup"><span data-stu-id="31eb4-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="31eb4-211">Dies bewirkt, dass WCF eine HTTP PUT-Anforderung senden.</span><span class="sxs-lookup"><span data-stu-id="31eb4-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="31eb4-212">Löschen einer Entität</span><span class="sxs-lookup"><span data-stu-id="31eb4-212">Delete an Entity</span></span>

<span data-ttu-id="31eb4-213">Um eine Entität zu löschen, rufen **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="31eb4-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="31eb4-214">Rufen Sie eine OData-Aktion</span><span class="sxs-lookup"><span data-stu-id="31eb4-214">Invoke an OData Action</span></span>

<span data-ttu-id="31eb4-215">In OData [Aktionen](odata-actions.md) sind eine Möglichkeit, serverseitige Verhaltensweisen hinzufügen, die nicht einfach als CRUD-Vorgänge für Entitäten definiert sind.</span><span class="sxs-lookup"><span data-stu-id="31eb4-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="31eb4-216">Obwohl Metadatendokument des OData, die Aktionen beschrieben, erstellt die Proxyklasse keine stark typisierte Methoden keine für sie.</span><span class="sxs-lookup"><span data-stu-id="31eb4-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="31eb4-217">Sie können immer noch eine OData-Aktion aufrufen, über die generische **Execute** Methode.</span><span class="sxs-lookup"><span data-stu-id="31eb4-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="31eb4-218">Allerdings müssen Sie wissen, die Datentypen der Parameter und der zurückgegebene Wert.</span><span class="sxs-lookup"><span data-stu-id="31eb4-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="31eb4-219">Z. B. die `RateProduct` Parameter mit dem Namen "Rating" vom Typ "Aktion" `Int32` und gibt eine `double`.</span><span class="sxs-lookup"><span data-stu-id="31eb4-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="31eb4-220">Der folgende Code zeigt, wie Sie diese Aktion aufrufen.</span><span class="sxs-lookup"><span data-stu-id="31eb4-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="31eb4-221">Weitere Informationen finden Sie unter[Aufrufen von Dienstvorgängen und-Aktionen](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="31eb4-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="31eb4-222">Eine Möglichkeit ist, erweitern Sie die **Container** Klasse, um eine stark typisierte Methode bereitzustellen, die die Aktion aufruft:</span><span class="sxs-lookup"><span data-stu-id="31eb4-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
