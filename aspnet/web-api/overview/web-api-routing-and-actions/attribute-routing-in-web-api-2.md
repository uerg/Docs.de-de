---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Routing in ASP.NET Web API 2-Attribut | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 173add73a150d3e13ae243d6548463da912dadee
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "28038048"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="1b7a8-102">Routing in ASP.NET Web API 2-Attribut</span><span class="sxs-lookup"><span data-stu-id="1b7a8-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="1b7a8-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1b7a8-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1b7a8-104">*Routing* ist wie Web-API einen URI zu einer Aktion entspricht.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="1b7a8-105">Web-API 2 unterstützt einen neuen Typ wird aufgerufen, Routing, *routing-Attribut*.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="1b7a8-106">Wie der Name schon sagt, verwendet das routing-Attribut Attribute zum Definieren von Routen.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="1b7a8-107">Routing-Attribut bietet Ihnen mehr Kontrolle über die URIs in Ihre Web-API.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="1b7a8-108">Beispielsweise können Sie problemlos erstellen URIs, die Hierarchien von Ressourcen zu beschreiben.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="1b7a8-109">Das frühere Format Routing, konventionsbasierte aufgerufen wird routing, weiterhin vollständig unterstützt.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="1b7a8-110">Tatsächlich können Sie beide Verfahren im selben Projekt kombinieren.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="1b7a8-111">In diesem Thema wird gezeigt, wie routing-Attribut zu aktivieren und beschreibt die verschiedenen Optionen für das routing-Attribut.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="1b7a8-112">Eine End-to-End-Lernprogramm, das routing-Attribut verwendet, finden Sie unter [erstellen Sie eine REST-API mit Routing-Attribut in der Web-API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="1b7a8-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="1b7a8-113">Erforderliche Komponenten</span><span class="sxs-lookup"><span data-stu-id="1b7a8-113">Prerequisites</span></span>

<span data-ttu-id="1b7a8-114">[Visual Studio-2017](https://www.visualstudio.com/vs/) Community, Professional oder Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="1b7a8-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition</span></span>

<span data-ttu-id="1b7a8-115">Verwenden Sie alternativ NuGet-Paket-Manager zum Installieren der erforderlichen Pakete an.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="1b7a8-116">Aus der **Tools** in Visual Studio, wählen Sie im Menü **Bibliothekspaket-Manager**, und wählen Sie dann **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-116">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="1b7a8-117">Geben Sie den folgenden Befehl in der Paket-Manager-Konsole aus:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="1b7a8-118">Warum zurückführen Routing?</span><span class="sxs-lookup"><span data-stu-id="1b7a8-118">Why Attribute Routing?</span></span>

<span data-ttu-id="1b7a8-119">Der ersten Version von Web-API verwendet *konventionsbasierter* routing.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="1b7a8-120">In diesem Typ von routing Sie definieren eine oder mehr routenvorlagen, die im Grunde werden parametrisiert Zeichenfolgen.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="1b7a8-121">Wenn das Framework eine Anforderung empfängt, liegt eine Übereinstimmung mit den URI für die routenvorlage.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="1b7a8-122">(Weitere Informationen zum routing konventionsbasierte finden Sie unter [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="1b7a8-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="1b7a8-123">Ein Vorteil von routing konventionsbasierte ist, dass die Vorlagen werden an einem Ort definiert und die Senderegeln konsistent über alle Controller angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="1b7a8-124">Leider ist routing konventionsbasierte schwer zu bestimmten URI-Muster unterstützt, die häufig in Rest-APIs sind.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="1b7a8-125">Beispielsweise Ressourcen häufig untergeordneten Ressourcen enthalten: Kunden Bestellungen haben, Filme Akteure, Bücher weisen Autoren und usw.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="1b7a8-126">Es ist zum Erstellen von URIs, die diese Beziehungen widerspiegeln natürliche:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="1b7a8-127">Dieser URI ist schwierig, mithilfe von routing konventionsbasierte zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="1b7a8-128">Obwohl es möglich ist, skalieren nicht die Ergebnisse, gut, wenn Sie viele Domänencontroller oder Ressourcentypen zur Verfügung haben.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="1b7a8-129">Mit routing-Attribut ist es triviale so definieren Sie eine Route für diesen URI.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="1b7a8-130">Ein Attribut wird einfach an die Controlleraktion hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="1b7a8-131">Hier sind einige weitere Muster, die einfach routing macht-Attribut ein.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="1b7a8-132">**API-versionsverwaltung**</span><span class="sxs-lookup"><span data-stu-id="1b7a8-132">**API versioning**</span></span>

<span data-ttu-id="1b7a8-133">In diesem Beispiel "/ api/v1/Products" wäre an einen anderen Controller als "/ api/v2/Products".</span><span class="sxs-lookup"><span data-stu-id="1b7a8-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`  
`/api/v2/products`

<span data-ttu-id="1b7a8-134">**Überladene URI-Segmente**</span><span class="sxs-lookup"><span data-stu-id="1b7a8-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="1b7a8-135">In diesem Beispiel "1" ist eine Auftragsnummer "Ausstehend" ordnet einer Auflistung.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`  
`/orders/pending`

<span data-ttu-id="1b7a8-136">**Mehrere Parametertypen**</span><span class="sxs-lookup"><span data-stu-id="1b7a8-136">**Multiple parameter types**</span></span>

<span data-ttu-id="1b7a8-137">In diesem Beispiel "1" ist eine Auftragsnummer ein, aber "2013/06/16" gibt ein Datum an.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="1b7a8-138">Aktivieren der Routing-Attribut</span><span class="sxs-lookup"><span data-stu-id="1b7a8-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="1b7a8-139">Rufen Sie zum Aktivieren der routing-Attribut **MapHttpAttributeRoutes** während der Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="1b7a8-140">Diese Erweiterungsmethode ist definiert, der **System.Web.Http.HttpConfigurationExtensions** Klasse.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="1b7a8-141">Routing-Attribut kann zusammen mit [konventionsbasierter](routing-in-aspnet-web-api.md) routing.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="1b7a8-142">Rufen Sie zum Definieren von Routen konventionsbasierte der **MapHttpRoute** Methode.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="1b7a8-143">Weitere Informationen zum Konfigurieren von Web-API finden Sie unter [Konfigurieren von ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="1b7a8-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="1b7a8-144">Hinweis: Migrieren von Web-API 1</span><span class="sxs-lookup"><span data-stu-id="1b7a8-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="1b7a8-145">Vor der Web-API 2 generiert die Projektvorlagen für Web-API-Code wie folgt:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="1b7a8-146">Wenn routing-Attribut aktiviert ist, wird dieser Code eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="1b7a8-147">Wenn Sie ein vorhandenes Web-API-Projekt zur Verwendung von routing-Attribut aktualisieren, stellen Sie sicher, dieser Konfigurationscode folgt aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="1b7a8-148">Weitere Informationen finden Sie unter [Konfigurieren des Web-API mit ASP.NET-Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="1b7a8-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="1b7a8-149">Hinzufügen von Routenattributen</span><span class="sxs-lookup"><span data-stu-id="1b7a8-149">Adding Route Attributes</span></span>

<span data-ttu-id="1b7a8-150">Hier ist ein Beispiel für eine Route mit einem Attribut definiert:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="1b7a8-151">Die Zeichenfolge &quot;Kunden / {CustomerId} / orders&quot; ist die URI-Vorlage für die Route.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="1b7a8-152">Web-API versucht, die im Anforderungs-URI der Vorlage übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="1b7a8-153">In diesem Beispiel "Customers" und "Orders" literal Segmente sind, und "{CustomerId}" ist eine Variable-Parameter.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="1b7a8-154">Die folgenden URIs würde diese Vorlage übereinstimmen:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="1b7a8-155">Sie können einschränken, den Datenabgleich mithilfe [Einschränkungen](#constraints), weiter unten in diesem Thema beschrieben.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="1b7a8-156">Beachten Sie, dass die &quot;{CustomerId}&quot; Parameter in der routenvorlage entspricht dem Namen der *CustomerId* Parameter der Methode.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="1b7a8-157">Beim Web-API-Controlleraktion aufgerufen wird, wird versucht, die Routenparameter zu binden.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="1b7a8-158">Angenommen, wenn der URI `http://example.com/customers/1/orders`, Web-API versucht, den Wert "1" zu binden, um die *CustomerId* Parameter in der Aktion.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="1b7a8-159">Eine URI-Vorlage kann mehrere Parameter haben:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="1b7a8-160">Alle Domänencontroller-Methoden, die nicht über eine Route-Attribut verfügen verwenden konventionsbasierte routing.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="1b7a8-161">Auf diese Weise können Sie beide Arten von routing im selben Projekt kombinieren.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="1b7a8-162">HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="1b7a8-162">HTTP Methods</span></span>

<span data-ttu-id="1b7a8-163">Web-API wählt auch Aktionen, die auf der Grundlage der HTTP-Methode der Anforderung (GET, POST usw.).</span><span class="sxs-lookup"><span data-stu-id="1b7a8-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="1b7a8-164">Standardmäßig sucht Web-API für die Groß-/Kleinschreibung Übereinstimmung mit dem Start des Methodennamens Controller.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="1b7a8-165">Z. B. einen Controllermethode, die mit dem Namen `PutCustomers` eine HTTP PUT-Anforderung übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="1b7a8-166">Sie können diese Konvention überschreiben werden, indem die Methode mit die folgenden Attributen:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="1b7a8-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="1b7a8-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="1b7a8-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="1b7a8-168">**[HttpGet]**</span></span>
- <span data-ttu-id="1b7a8-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="1b7a8-169">**[HttpHead]**</span></span>
- <span data-ttu-id="1b7a8-170">**["HttpOptions"]**</span><span class="sxs-lookup"><span data-stu-id="1b7a8-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="1b7a8-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="1b7a8-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="1b7a8-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="1b7a8-172">**[HttpPost]**</span></span>
- <span data-ttu-id="1b7a8-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="1b7a8-173">**[HttpPut]**</span></span>

<span data-ttu-id="1b7a8-174">Das folgende Beispiel ordnet die CreateBook-Methode für HTTP POST-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="1b7a8-175">Für alle anderen HTTP-Methoden, einschließlich der nicht standardmäßige Methoden verwenden das **AcceptVerbs** Attribut, das eine Liste der HTTP-Methoden behandelt.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="1b7a8-176">Routenpräfixe</span><span class="sxs-lookup"><span data-stu-id="1b7a8-176">Route Prefixes</span></span>

<span data-ttu-id="1b7a8-177">Häufig, um die Routen in einem Controller alle mit dem gleichen Präfix beginnen.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="1b7a8-178">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="1b7a8-179">Sie können ein gemeinsames Präfix für einen gesamten Controller festlegen, mit der **[Routenpräfix]** Attribut:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="1b7a8-180">Verwenden Sie eine Tilde (~) für das Attribut für die Methode, um das Routenpräfix überschreiben:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="1b7a8-181">Das Routenpräfix kann Parameter enthalten:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="1b7a8-182">Routeneinschränkungen</span><span class="sxs-lookup"><span data-stu-id="1b7a8-182">Route Constraints</span></span>

<span data-ttu-id="1b7a8-183">Routeneinschränkungen können Sie einschränken, wie die Parameter in der routenvorlage abgeglichen werden.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="1b7a8-184">Die allgemeine Syntax lautet &quot;{Parameter: Einschränkung}&quot;.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="1b7a8-185">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="1b7a8-186">Hier werden die ersten Route wird nur ausgewählt, wenn die &quot;Id&quot; die URI-Segment ist eine ganze Zahl.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="1b7a8-187">Andernfalls wird die zweite Route ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="1b7a8-188">Die folgende Tabelle enthält die Einschränkungen, die unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="1b7a8-189">Constraint</span><span class="sxs-lookup"><span data-stu-id="1b7a8-189">Constraint</span></span> | <span data-ttu-id="1b7a8-190">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="1b7a8-190">Description</span></span> | <span data-ttu-id="1b7a8-191">Beispiel</span><span class="sxs-lookup"><span data-stu-id="1b7a8-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="1b7a8-192">Alpha</span><span class="sxs-lookup"><span data-stu-id="1b7a8-192">alpha</span></span> | <span data-ttu-id="1b7a8-193">Übereinstimmungen Groß- oder Kleinbuchstaben Zeichen des lateinischen Alphabets (a-Z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="1b7a8-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="1b7a8-194">{x:alpha}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-194">{x:alpha}</span></span> |
| <span data-ttu-id="1b7a8-195">bool</span><span class="sxs-lookup"><span data-stu-id="1b7a8-195">bool</span></span> | <span data-ttu-id="1b7a8-196">Entspricht einem booleschen Wert.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-196">Matches a Boolean value.</span></span> | <span data-ttu-id="1b7a8-197">{x:bool}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-197">{x:bool}</span></span> |
| <span data-ttu-id="1b7a8-198">datetime</span><span class="sxs-lookup"><span data-stu-id="1b7a8-198">datetime</span></span> | <span data-ttu-id="1b7a8-199">Entspricht einem **"DateTime"** Wert.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="1b7a8-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-200">{x:datetime}</span></span> |
| <span data-ttu-id="1b7a8-201">decimal</span><span class="sxs-lookup"><span data-stu-id="1b7a8-201">decimal</span></span> | <span data-ttu-id="1b7a8-202">Entspricht einen decimal-Wert.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-202">Matches a decimal value.</span></span> | <span data-ttu-id="1b7a8-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-203">{x:decimal}</span></span> |
| <span data-ttu-id="1b7a8-204">double</span><span class="sxs-lookup"><span data-stu-id="1b7a8-204">double</span></span> | <span data-ttu-id="1b7a8-205">Entspricht einer 64-Bit-Gleitkommawert.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="1b7a8-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-206">{x:double}</span></span> |
| <span data-ttu-id="1b7a8-207">float</span><span class="sxs-lookup"><span data-stu-id="1b7a8-207">float</span></span> | <span data-ttu-id="1b7a8-208">Entspricht einem 32-Bit-Gleitkommawert.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="1b7a8-209">{x:float}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-209">{x:float}</span></span> |
| <span data-ttu-id="1b7a8-210">guid</span><span class="sxs-lookup"><span data-stu-id="1b7a8-210">guid</span></span> | <span data-ttu-id="1b7a8-211">Entspricht einen GUID-Wert.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-211">Matches a GUID value.</span></span> | <span data-ttu-id="1b7a8-212">{x:guid}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-212">{x:guid}</span></span> |
| <span data-ttu-id="1b7a8-213">int</span><span class="sxs-lookup"><span data-stu-id="1b7a8-213">int</span></span> | <span data-ttu-id="1b7a8-214">Entspricht einem 32-Bit-Ganzzahl-Wert.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="1b7a8-215">{x:int}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-215">{x:int}</span></span> |
| <span data-ttu-id="1b7a8-216">Länge</span><span class="sxs-lookup"><span data-stu-id="1b7a8-216">length</span></span> | <span data-ttu-id="1b7a8-217">Entspricht einer Zeichenfolge mit der angegebenen Länge oder innerhalb eines bestimmten Bereichs Länge.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="1b7a8-218">{X: length(6)} {X: length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="1b7a8-219">long</span><span class="sxs-lookup"><span data-stu-id="1b7a8-219">long</span></span> | <span data-ttu-id="1b7a8-220">Entspricht einem 64-Bit-Ganzzahl-Wert.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="1b7a8-221">{x:long}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-221">{x:long}</span></span> |
| <span data-ttu-id="1b7a8-222">max</span><span class="sxs-lookup"><span data-stu-id="1b7a8-222">max</span></span> | <span data-ttu-id="1b7a8-223">Entspricht einer ganzen Zahl mit einem Maximalwert.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="1b7a8-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-224">{x:max(10)}</span></span> |
| <span data-ttu-id="1b7a8-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="1b7a8-225">maxlength</span></span> | <span data-ttu-id="1b7a8-226">Entspricht einer Zeichenfolge mit einer maximalen Länge.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="1b7a8-227">{X: maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="1b7a8-228">Min.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-228">min</span></span> | <span data-ttu-id="1b7a8-229">Entspricht einer ganzen Zahl mit einem Mindestwert.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="1b7a8-230">{X: min(10)}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-230">{x:min(10)}</span></span> |
| <span data-ttu-id="1b7a8-231">"minLength"</span><span class="sxs-lookup"><span data-stu-id="1b7a8-231">minlength</span></span> | <span data-ttu-id="1b7a8-232">Mit einer Zeichenfolge mit einer minimalen Länge überein.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="1b7a8-233">{X: minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="1b7a8-234">range</span><span class="sxs-lookup"><span data-stu-id="1b7a8-234">range</span></span> | <span data-ttu-id="1b7a8-235">Entspricht einer ganzen Zahl innerhalb eines Bereichs von Werten.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="1b7a8-236">{X: range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="1b7a8-237">regex</span><span class="sxs-lookup"><span data-stu-id="1b7a8-237">regex</span></span> | <span data-ttu-id="1b7a8-238">Mit einem regulären Ausdruck übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-238">Matches a regular expression.</span></span> | <span data-ttu-id="1b7a8-239">{X: regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="1b7a8-240">Beachten Sie, dass einige Einschränkungen, wie z. B. &quot;min&quot;, Argumente in Klammern.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="1b7a8-241">Sie können mehrere Einschränkungen auf einen Parameter, die durch einen Doppelpunkt getrennt anwenden.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="1b7a8-242">Benutzerdefinierte Routeneinschränkungen</span><span class="sxs-lookup"><span data-stu-id="1b7a8-242">Custom Route Constraints</span></span>

<span data-ttu-id="1b7a8-243">Sie können benutzerdefinierte routeneinschränkungen erstellen, durch die Implementierung der **IHttpRouteConstraint** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="1b7a8-244">Die folgende Einschränkung schränkt z. B. einen Parameter auf einen Wert ungleich NULL ganze Zahl an.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="1b7a8-245">Der folgende Code zeigt, wie Sie die Einschränkung zu registrieren:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="1b7a8-246">Jetzt können Sie die Einschränkung in Ihre Routen anwenden:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="1b7a8-247">Sie können auch die gesamte ersetzen **DefaultInlineConstraintResolver** Klasse durch Implementieren der **IInlineConstraintResolver** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="1b7a8-248">Auf diese Weise wird ersetzt alle integrierten Einschränkungen, es sei denn, Ihre Implementierung von **IInlineConstraintResolver** speziell hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="1b7a8-249">Optionale URI-Parameter und Standardwerte</span><span class="sxs-lookup"><span data-stu-id="1b7a8-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="1b7a8-250">Sie können einen URI-Parameter optional machen, indem ein Fragezeichen routenparameters hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="1b7a8-251">Wenn ein Routenparameter optional ist, müssen Sie einen Standardwert für den Methodenparameter definieren.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="1b7a8-252">In diesem Beispiel `/api/books/locale/1033` und `/api/books/locale` dieselbe Ressource zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="1b7a8-253">Alternativ können Sie einen Standardwert in der routenvorlage wie folgt angeben:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="1b7a8-254">Dies ist fast identisch mit dem vorherigen Beispiel, jedoch ist es ein geringfügigen Unterschied des Verhaltens, wenn der Standardwert angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="1b7a8-255">Im ersten Beispiel ("{Lcid?}") ist der Standardwert 1033 an, direkt an der Methodenparameter, zugewiesen, damit die Parameter dieser genauen Wert hat.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="1b7a8-256">Im zweiten Beispiel ("{Lcid = 1033}"), der Wert von "1033" durchläuft die modellbindung.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="1b7a8-257">Der Standardmodellbinder werden in den numerischen Wert 1033 "1033" konvertiert.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="1b7a8-258">Sie können jedoch in einen benutzerdefinierten Modellbinder anschließen die etwas anderes sind möglicherweise.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="1b7a8-259">(In den meisten Fällen, wenn Sie benutzerdefinierte Modellbinder in Ihrer Pipeline haben die beiden Formen entsprechende werden.)</span><span class="sxs-lookup"><span data-stu-id="1b7a8-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="1b7a8-260">Routennamen</span><span class="sxs-lookup"><span data-stu-id="1b7a8-260">Route Names</span></span>

<span data-ttu-id="1b7a8-261">In der Web-API hat jede Route einen Namen an.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-261">In Web API, every route has a name.</span></span> <span data-ttu-id="1b7a8-262">Routennamen eignen sich zum Generieren von Links, sodass Sie einen Link in einer HTTP-Antwort enthalten können.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="1b7a8-263">Der Routenname, legen die **Namen** -Eigenschaft des Attributs.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="1b7a8-264">Im folgenden Beispiel wird veranschaulicht, wie den Routennamen festlegen sowie den Routennamen verwenden, wenn einen Link zu generieren.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="1b7a8-265">Routenreihenfolge</span><span class="sxs-lookup"><span data-stu-id="1b7a8-265">Route Order</span></span>

<span data-ttu-id="1b7a8-266">Wenn das Framework versucht, einen URI mit einer Route übereinstimmen, wird er die Routen in einer bestimmten Reihenfolge ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="1b7a8-267">Um die Reihenfolge anzugeben, legen die **RouteOrder** -Eigenschaft für die routenattribut.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="1b7a8-268">Niedrigere Werte werden zuerst ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-268">Lower values are evaluated first.</span></span> <span data-ttu-id="1b7a8-269">Der standardreihenfolgenwert ist 0 (null).</span><span class="sxs-lookup"><span data-stu-id="1b7a8-269">The default order value is zero.</span></span>

<span data-ttu-id="1b7a8-270">Hier wird die gesamte Sortierung festgelegt:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="1b7a8-271">Vergleichen der **RouteOrder** -Eigenschaft des Attributs Route.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="1b7a8-272">Betrachten Sie jede URI-Segment in der routenvorlage aus.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="1b7a8-273">Bestellen Sie für jedes Segment wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-273">For each segment, order as follows:</span></span> 

    1. <span data-ttu-id="1b7a8-274">Literal-Segmente.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-274">Literal segments.</span></span>
    2. <span data-ttu-id="1b7a8-275">Routenparameter mit Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="1b7a8-276">Routenparameter ohne Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="1b7a8-277">Parameter-platzhaltersegmente mit Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="1b7a8-278">Parameter-platzhaltersegmente ohne Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="1b7a8-279">Bei einem gleichwertigen Objekt werden Routen nach einem Ordinalzeichenfolgenvergleich sortiert ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) für die routenvorlage.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="1b7a8-280">Im Folgenden ein Beispiel.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-280">Here is an example.</span></span> <span data-ttu-id="1b7a8-281">Nehmen Sie an, dass Sie die folgenden Controller definieren:</span><span class="sxs-lookup"><span data-stu-id="1b7a8-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="1b7a8-282">Mit diesen Routen werden folgendermaßen sortiert.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="1b7a8-283">Aufträge/details</span><span class="sxs-lookup"><span data-stu-id="1b7a8-283">orders/details</span></span>
2. <span data-ttu-id="1b7a8-284">Aufträge / {Id}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-284">orders/{id}</span></span>
3. <span data-ttu-id="1b7a8-285">Aufträge / {Kundenname}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-285">orders/{customerName}</span></span>
4. <span data-ttu-id="1b7a8-286">Aufträge / {\*Date}</span><span class="sxs-lookup"><span data-stu-id="1b7a8-286">orders/{\*date}</span></span>
5. <span data-ttu-id="1b7a8-287">Aufträge / ausstehend</span><span class="sxs-lookup"><span data-stu-id="1b7a8-287">orders/pending</span></span>

<span data-ttu-id="1b7a8-288">Beachten Sie, dass "Details" ein literal Segment ist und vor der "{Id}" wird angezeigt, aber "Ausstehend" wird, zuletzt angezeigt da die **RouteOrder** Eigenschaft ist 1.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="1b7a8-289">(In diesem Beispiel wird vorausgesetzt, dass sind keine Kunden mit dem Namen "Details" oder "Ausstehend".</span><span class="sxs-lookup"><span data-stu-id="1b7a8-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="1b7a8-290">Im Allgemeinen versucht, mehrdeutige Routen zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="1b7a8-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="1b7a8-291">In diesem Beispiel eine bessere routenvorlage für `GetByCustomer` ist "Kunden / {Kundenname}")</span><span class="sxs-lookup"><span data-stu-id="1b7a8-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
