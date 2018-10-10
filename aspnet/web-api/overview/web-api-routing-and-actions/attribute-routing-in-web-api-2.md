---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Attributrouting in der ASP.NET Web API 2 | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: d16dcc618bf6c60714179601db14f4dd2a9e41ce
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912151"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="c3c7c-102">Attribut-Routing in ASP.NET Web-API 2</span><span class="sxs-lookup"><span data-stu-id="c3c7c-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="c3c7c-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c3c7c-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c3c7c-104">*Routing* ist wie Web-API einen URI zu einer Aktion entspricht.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="c3c7c-105">Web-API 2 unterstützt einen neuen Typ Namens des Routings, *attributrouting*.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="c3c7c-106">Wie der Name schon sagt, wird das attributrouting Attribute zum Definieren von Routen verwendet.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="c3c7c-107">Attribut-routing bietet Ihnen mehr Kontrolle über die URIs in Ihrer Web-API.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="c3c7c-108">Beispielsweise können Sie ganz einfach erstellen URIs, die Hierarchien von Ressourcen beschreiben.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="c3c7c-109">Das frühere Format des Routings, aufgerufen wird, auf Konventionen basierendes wird routing weiterhin vollständig unterstützt.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="c3c7c-110">In der Tat können Sie beide Verfahren im selben Projekt kombinieren.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="c3c7c-111">In diesem Thema wird gezeigt, wie das attributrouting aktivieren und beschreibt die verschiedenen Optionen für die Attribut-routing.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="c3c7c-112">Ein End-to-End-Lernprogramm, das das attributrouting verwendet, finden Sie unter [erstellen Sie eine REST-API mit Attributrouting in der Web-API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="c3c7c-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c3c7c-113">Vorraussetzungen</span><span class="sxs-lookup"><span data-stu-id="c3c7c-113">Prerequisites</span></span>

<span data-ttu-id="c3c7c-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional oder Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="c3c7c-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional, or Enterprise edition</span></span>

<span data-ttu-id="c3c7c-115">Alternativ verwenden Sie NuGet-Paket-Manager, um die erforderlichen Pakete zu installieren.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="c3c7c-116">Aus der **Tools** in Visual Studio, wählen Sie im Menü **NuGet Paket-Manager**, dann **Paket-Manager Konsole**.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-116">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="c3c7c-117">Geben Sie im Fenster Paket-Manager-Konsole den folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="c3c7c-118">Warum Attributrouting?</span><span class="sxs-lookup"><span data-stu-id="c3c7c-118">Why Attribute Routing?</span></span>

<span data-ttu-id="c3c7c-119">Die erste Version von Web-API verwendet *auf Konventionen basierendes* routing.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="c3c7c-120">In diesem Typ des Routings Sie definieren eine oder weitere routenvorlagen, die im Wesentlichen sind parametrisierte Zeichenfolgen aus.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="c3c7c-121">Wenn das Framework eine Anforderung empfängt, liegt eine Übereinstimmung der URI für die routenvorlage mit.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="c3c7c-122">(Weitere Informationen zum konventionsbasierten routing finden Sie unter [Routing in ASP.NET Web-API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c3c7c-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="c3c7c-123">Ein Vorteil des konventionsbasierten Routings ist, dass Vorlagen in einem einzigen Ort definiert sind, und die Routingregeln werden konsistent auf alle Controller angewendet.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="c3c7c-124">Leider ist das konventionsbasierte routing schwer zu bestimmten URI-Muster unterstützen, die in die RESTful-APIs gelten.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="c3c7c-125">Beispielsweise Ressourcen häufig untergeordnete Ressourcen enthalten: Kunden Bestellungen, Filme haben, Actors, Bücher, Autoren von Steuerelementen und so weiter.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="c3c7c-126">Es ist normal, Erstellen von URIs, die diese Beziehungen widerspiegeln:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="c3c7c-127">Dieser Typ von URI ist schwierig, erstellen Sie mithilfe der Konventionen basierten routing.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="c3c7c-128">Obwohl es möglich ist, skalieren nicht die Ergebnisse, gut, wenn Sie viele Controller oder Ressourcentypen zur Verfügung haben.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="c3c7c-129">Das attributrouting ist es einfach, eine Route für diesen URI zu definieren.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="c3c7c-130">Sie fügen einfach ein Attribut, um die Controlleraktion:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="c3c7c-131">Hier sind einige andere Muster, die Attribut einfach routing macht.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="c3c7c-132">**API-versionsverwaltung**</span><span class="sxs-lookup"><span data-stu-id="c3c7c-132">**API versioning**</span></span>

<span data-ttu-id="c3c7c-133">In diesem Beispiel "/ api/v1/Produkte" wäre weitergeleitet, auf einen anderen Controller als "/ api/v2/Produkte".</span><span class="sxs-lookup"><span data-stu-id="c3c7c-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`
`/api/v2/products`

<span data-ttu-id="c3c7c-134">**Überladene URI-Segmente**</span><span class="sxs-lookup"><span data-stu-id="c3c7c-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="c3c7c-135">In diesem Beispiel ist "1" ist eine Bestellnummer, aber "Ausstehend" ist einer Sammlung zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`
`/orders/pending`

<span data-ttu-id="c3c7c-136">**Mehrere Parametertypen**</span><span class="sxs-lookup"><span data-stu-id="c3c7c-136">**Multiple parameter types**</span></span>

<span data-ttu-id="c3c7c-137">In diesem Beispiel ist "1" ist eine Bestellnummer, aber "2013/06/16" gibt ein Datum an.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="c3c7c-138">Aktivieren der Attribut-Routing</span><span class="sxs-lookup"><span data-stu-id="c3c7c-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="c3c7c-139">Rufen Sie zum Aktivieren der Attribut-routing **MapHttpAttributeRoutes** während der Konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="c3c7c-140">Diese Erweiterungsmethode wird definiert, der **System.Web.Http.HttpConfigurationExtensions** Klasse.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="c3c7c-141">Beim attributrouting kann kombiniert werden, mit [auf Konventionen basierendes](routing-in-aspnet-web-api.md) routing.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="c3c7c-142">Rufen Sie zum Definieren von Routen auf Konventionen basierendes der **"maphttproute"** Methode.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="c3c7c-143">Weitere Informationen zum Konfigurieren von Web-API finden Sie unter [Konfigurieren von ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="c3c7c-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="c3c7c-144">Hinweis: Die Migration von Web-API 1</span><span class="sxs-lookup"><span data-stu-id="c3c7c-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="c3c7c-145">Vor der Web-API 2 generiert die Projektvorlagen für Web-API-Code wie folgt:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="c3c7c-146">Wenn die Attribut-routing aktiviert ist, wird dieser Code eine Ausnahme ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="c3c7c-147">Wenn Sie ein vorhandenes Web-API-Projekt zu verwenden, das attributrouting aktualisieren, müssen Sie dieser Konfigurationscode folgt aktualisieren:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="c3c7c-148">Weitere Informationen finden Sie unter [Konfigurieren des Web-API mit Hosten von ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="c3c7c-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="c3c7c-149">Hinzufügen von Routenattributen</span><span class="sxs-lookup"><span data-stu-id="c3c7c-149">Adding Route Attributes</span></span>

<span data-ttu-id="c3c7c-150">Hier ist ein Beispiel für eine Route mit einem Attribut definiert:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="c3c7c-151">Die Zeichenfolge &quot;Customers / {CustomerId} / Bestellungen&quot; ist die URI-Vorlage für die Route.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="c3c7c-152">Web-API versucht, die im Anforderungs-URI der Vorlage übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="c3c7c-153">In diesem Beispiel "Customers" und "Orders" sind literal-Segmente, und "{CustomerId}" ist eine Variable-Parameter.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="c3c7c-154">Die folgenden URIs entsprechen diese Vorlage:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="c3c7c-155">Sie können einschränken, den entsprechenden mit [Einschränkungen](#constraints), die weiter unten in diesem Thema beschrieben.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="c3c7c-156">Beachten Sie, dass die &quot;{CustomerId}&quot; Parameter in der routenvorlage übereinstimmt, den Namen der *"CustomerID"* Parameter in der Methode.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="c3c7c-157">Wenn die Web-API die Controlleraktion aufgerufen wird, versucht, die die Routenparameter zu binden.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="c3c7c-158">Wenn der URI ist z. B. `http://example.com/customers/1/orders`, Web-API versucht, den Wert "1" zu binden, um die *"CustomerID"* Parameter in der Aktion.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="c3c7c-159">Eine URI-Vorlage kann mehrere Parameter haben:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="c3c7c-160">Alle Methoden des Controllers, die nicht über eine Route-Attribut verfügen verwenden konventionsbasierten routing.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="c3c7c-161">Auf diese Weise können Sie beide Arten von routing im selben Projekt kombinieren.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="c3c7c-162">HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="c3c7c-162">HTTP Methods</span></span>

<span data-ttu-id="c3c7c-163">Web-API wählt auch Aktionen basierend auf den HTTP-Methode der Anforderung (GET, POST usw.).</span><span class="sxs-lookup"><span data-stu-id="c3c7c-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="c3c7c-164">Standardmäßig sucht die Web-API für die Groß-/Kleinschreibung Übereinstimmung mit dem Anfang der Name der Controller-Methode.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="c3c7c-165">Z. B. eine Controllermethode, die mit dem Namen `PutCustomers` eine HTTP PUT-Anforderung übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="c3c7c-166">Sie können diese Konvention überschreiben werden, indem die Methode mit einem der folgenden Attribute:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="c3c7c-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="c3c7c-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="c3c7c-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="c3c7c-168">**[HttpGet]**</span></span>
- <span data-ttu-id="c3c7c-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="c3c7c-169">**[HttpHead]**</span></span>
- <span data-ttu-id="c3c7c-170">**["HttpOptions"]**</span><span class="sxs-lookup"><span data-stu-id="c3c7c-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="c3c7c-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="c3c7c-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="c3c7c-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="c3c7c-172">**[HttpPost]**</span></span>
- <span data-ttu-id="c3c7c-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="c3c7c-173">**[HttpPut]**</span></span>

<span data-ttu-id="c3c7c-174">Im folgende Beispiel ordnet die CreateBook-Methode auf HTTP-POST-Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="c3c7c-175">Für alle anderen HTTP-Methoden, einschließlich der nicht standardmäßige Methoden verwenden die **AcceptVerbs** -Attribut, das eine Liste von HTTP-Methoden akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="c3c7c-176">Routenpräfixe</span><span class="sxs-lookup"><span data-stu-id="c3c7c-176">Route Prefixes</span></span>

<span data-ttu-id="c3c7c-177">Häufig die Routen in einem Controller, die alle mit dem gleichen Präfix beginnen.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="c3c7c-178">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="c3c7c-179">Sie können ein gemeinsames Präfix für einen gesamten Controller festlegen, mit der **[RoutePrefix]** Attribut:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="c3c7c-180">Verwenden Sie eine Tilde (~) auf das Method-Attribut, um das Routenpräfix außer Kraft setzen:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="c3c7c-181">Das Routenpräfix kann Parameter enthalten:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="c3c7c-182">Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="c3c7c-182">Route Constraints</span></span>

<span data-ttu-id="c3c7c-183">Einschränkungen können Sie einschränken, wie die Parameter in der routenvorlage abgeglichen werden.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="c3c7c-184">Die allgemeine Syntax lautet &quot;: {parametereinschränkungen}&quot;.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="c3c7c-185">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="c3c7c-186">Hier werden die erste Route wird nur ausgewählt, wenn die &quot;Id&quot; -Segment des URI ist eine ganze Zahl.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="c3c7c-187">Andernfalls wird die zweite Route ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="c3c7c-188">Die folgende Tabelle enthält die Einschränkungen, die unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="c3c7c-189">Constraint</span><span class="sxs-lookup"><span data-stu-id="c3c7c-189">Constraint</span></span> | <span data-ttu-id="c3c7c-190">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="c3c7c-190">Description</span></span> | <span data-ttu-id="c3c7c-191">Beispiel</span><span class="sxs-lookup"><span data-stu-id="c3c7c-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c3c7c-192">Alpha</span><span class="sxs-lookup"><span data-stu-id="c3c7c-192">alpha</span></span> | <span data-ttu-id="c3c7c-193">Übereinstimmungen in Großbuchstaben oder Kleinbuchstaben Zeichen des lateinischen Alphabets (a-Z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="c3c7c-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="c3c7c-194">{x:alpha}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-194">{x:alpha}</span></span> |
| <span data-ttu-id="c3c7c-195">bool</span><span class="sxs-lookup"><span data-stu-id="c3c7c-195">bool</span></span> | <span data-ttu-id="c3c7c-196">Entspricht einem booleschen Wert.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-196">Matches a Boolean value.</span></span> | <span data-ttu-id="c3c7c-197">{x:bool}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-197">{x:bool}</span></span> |
| <span data-ttu-id="c3c7c-198">datetime</span><span class="sxs-lookup"><span data-stu-id="c3c7c-198">datetime</span></span> | <span data-ttu-id="c3c7c-199">Entspricht einem **"DateTime"** Wert.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="c3c7c-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-200">{x:datetime}</span></span> |
| <span data-ttu-id="c3c7c-201">decimal</span><span class="sxs-lookup"><span data-stu-id="c3c7c-201">decimal</span></span> | <span data-ttu-id="c3c7c-202">Entspricht einem decimal-Wert.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-202">Matches a decimal value.</span></span> | <span data-ttu-id="c3c7c-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-203">{x:decimal}</span></span> |
| <span data-ttu-id="c3c7c-204">double</span><span class="sxs-lookup"><span data-stu-id="c3c7c-204">double</span></span> | <span data-ttu-id="c3c7c-205">Entspricht einen 64-Bit-Gleitkommawert.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="c3c7c-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-206">{x:double}</span></span> |
| <span data-ttu-id="c3c7c-207">float</span><span class="sxs-lookup"><span data-stu-id="c3c7c-207">float</span></span> | <span data-ttu-id="c3c7c-208">Entspricht einen 32-Bit-Gleitkommawert.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="c3c7c-209">{x:float}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-209">{x:float}</span></span> |
| <span data-ttu-id="c3c7c-210">guid</span><span class="sxs-lookup"><span data-stu-id="c3c7c-210">guid</span></span> | <span data-ttu-id="c3c7c-211">Entspricht einem GUID-Wert.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-211">Matches a GUID value.</span></span> | <span data-ttu-id="c3c7c-212">{x:guid}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-212">{x:guid}</span></span> |
| <span data-ttu-id="c3c7c-213">int</span><span class="sxs-lookup"><span data-stu-id="c3c7c-213">int</span></span> | <span data-ttu-id="c3c7c-214">Entspricht einem 32-Bit-Ganzzahl-Wert.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="c3c7c-215">{x:int}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-215">{x:int}</span></span> |
| <span data-ttu-id="c3c7c-216">Länge</span><span class="sxs-lookup"><span data-stu-id="c3c7c-216">length</span></span> | <span data-ttu-id="c3c7c-217">Entspricht einer Zeichenfolge mit der angegebenen Länge oder innerhalb eines bestimmten Bereichs Länge.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="c3c7c-218">{X: length(6)} {X: length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="c3c7c-219">long</span><span class="sxs-lookup"><span data-stu-id="c3c7c-219">long</span></span> | <span data-ttu-id="c3c7c-220">Entspricht einem 64-Bit-Ganzzahl-Wert.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="c3c7c-221">{x:long}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-221">{x:long}</span></span> |
| <span data-ttu-id="c3c7c-222">max</span><span class="sxs-lookup"><span data-stu-id="c3c7c-222">max</span></span> | <span data-ttu-id="c3c7c-223">Entspricht einer ganzen Zahl mit einem maximalen Wert.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="c3c7c-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-224">{x:max(10)}</span></span> |
| <span data-ttu-id="c3c7c-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="c3c7c-225">maxlength</span></span> | <span data-ttu-id="c3c7c-226">Eine Zeichenfolge mit einer maximalen Länge entspricht.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="c3c7c-227">{X: maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="c3c7c-228">Min.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-228">min</span></span> | <span data-ttu-id="c3c7c-229">Entspricht einer ganzen Zahl mit einem Mindestwert.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="c3c7c-230">{X: min(10)}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-230">{x:min(10)}</span></span> |
| <span data-ttu-id="c3c7c-231">MinLength</span><span class="sxs-lookup"><span data-stu-id="c3c7c-231">minlength</span></span> | <span data-ttu-id="c3c7c-232">Entspricht einer Zeichenfolge mit einer Mindestlänge.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="c3c7c-233">{X: minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="c3c7c-234">range</span><span class="sxs-lookup"><span data-stu-id="c3c7c-234">range</span></span> | <span data-ttu-id="c3c7c-235">Entspricht einer ganzen Zahl in einem Bereich von Werten.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="c3c7c-236">{X: range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="c3c7c-237">regex</span><span class="sxs-lookup"><span data-stu-id="c3c7c-237">regex</span></span> | <span data-ttu-id="c3c7c-238">Mit einem regulären Ausdruck übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-238">Matches a regular expression.</span></span> | <span data-ttu-id="c3c7c-239">{X: regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="c3c7c-240">Beachten Sie, dass einige der Einschränkungen, wie z. B. &quot;min&quot;, akzeptieren Sie die Argumente in Klammern.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="c3c7c-241">Sie können mehrere Einschränkungen auf einen Parameter durch einen Doppelpunkt getrennt anwenden.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="c3c7c-242">Benutzerdefinierte Route-Einschränkungen</span><span class="sxs-lookup"><span data-stu-id="c3c7c-242">Custom Route Constraints</span></span>

<span data-ttu-id="c3c7c-243">Sie können benutzerdefinierte routeneinschränkungen erstellen, durch die Implementierung der **IHttpRouteConstraint** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="c3c7c-244">Die folgende Einschränkung schränkt z.B. einen Parameter auf einen Wert ungleich NULL ganze Zahl.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="c3c7c-245">Der folgende Code zeigt, wie Sie die Einschränkung zu registrieren:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="c3c7c-246">Jetzt können Sie die Einschränkung in Ihre Routen anwenden:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="c3c7c-247">Sie können auch die gesamte ersetzen **DefaultInlineConstraintResolver** Klasse durch die Implementierung der **IInlineConstraintResolver** Schnittstelle.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="c3c7c-248">Auf diese Weise ersetzt die integrierte Einschränkungen, es sei denn, Ihre Implementierung von **IInlineConstraintResolver** speziell hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="c3c7c-249">Optionaler URI-Parameter mit Standardwerten</span><span class="sxs-lookup"><span data-stu-id="c3c7c-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="c3c7c-250">Sie können einen URI-Parameter optional konfigurieren, indem Sie ein Fragezeichen routenparameters hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="c3c7c-251">Wenn ein Routenparameter optional ist, müssen Sie einen Standardwert für den Methodenparameter definieren.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="c3c7c-252">In diesem Beispiel `/api/books/locale/1033` und `/api/books/locale` dieselbe Ressource zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="c3c7c-253">Alternativ können Sie einen Standardwert in die routenvorlage, wie folgt angeben:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="c3c7c-254">Dies ist fast identisch mit dem vorherigen Beispiel, aber es wird ein geringfügigen Unterschied des Verhaltens, wenn der Standardwert angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="c3c7c-255">Im ersten Beispiel ("{Lcid?}") wird standardmäßig den Wert 1033 direkt der Methodenparameter, zugewiesen hat also der Parameter dieses genauen Wert.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="c3c7c-256">Im zweiten Beispiel ("{Lcid = 1033}"), der Standardwert von "1033" durchläuft die modellbindung.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="c3c7c-257">Der Standardmodellbinder werden auf den numerischen Wert 1033 "1033" konvertiert.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="c3c7c-258">Allerdings können Sie in einen benutzerdefinierten Modellbinder, einfügen, die etwas anderes erledigen kann.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="c3c7c-259">(In den meisten Fällen, es sei denn, Sie benutzerdefinierte modellbindungen in Ihrer Pipeline, werden die beiden Formen entsprechen.)</span><span class="sxs-lookup"><span data-stu-id="c3c7c-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="c3c7c-260">Routennamen</span><span class="sxs-lookup"><span data-stu-id="c3c7c-260">Route Names</span></span>

<span data-ttu-id="c3c7c-261">In der Web-API hat jede Route einen Namen an.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-261">In Web API, every route has a name.</span></span> <span data-ttu-id="c3c7c-262">Routennamen eignen sich zum Generieren von Links, damit Sie einen Link in einer HTTP-Antwort einschließen können.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="c3c7c-263">Der Routenname, legen die **Namen** -Eigenschaft des Attributs.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="c3c7c-264">Das folgende Beispiel zeigt, wie der Routenname festgelegt, und wie der Routenname zu verwenden, wenn Sie einen Link zu generieren.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="c3c7c-265">Reihenfolge</span><span class="sxs-lookup"><span data-stu-id="c3c7c-265">Route Order</span></span>

<span data-ttu-id="c3c7c-266">Wenn Sie das Framework versucht, die einen URI mit einer Route übereinstimmen, wird die Routen in einer bestimmten Reihenfolge ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="c3c7c-267">Legen Sie zum Angeben der Reihenfolge der **RouteOrder** Eigenschaft für die Route-Attribut.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="c3c7c-268">Niedrigere Werte werden zuerst ausgewertet.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-268">Lower values are evaluated first.</span></span> <span data-ttu-id="c3c7c-269">Der Wert der Standardreihenfolge ist 0 (null).</span><span class="sxs-lookup"><span data-stu-id="c3c7c-269">The default order value is zero.</span></span>

<span data-ttu-id="c3c7c-270">Hier ist wie der gesamten Sortierung bestimmt wird:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="c3c7c-271">Vergleichen Sie die **RouteOrder** -Eigenschaft des Attributs Route.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="c3c7c-272">Sehen Sie sich jede URI-Segment in der routenvorlage.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="c3c7c-273">Bestellen Sie für die einzelnen Segmente wie folgt:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-273">For each segment, order as follows:</span></span>

    1. <span data-ttu-id="c3c7c-274">Literal-Segmente.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-274">Literal segments.</span></span>
    2. <span data-ttu-id="c3c7c-275">Der Routenparameter mit Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="c3c7c-276">Der Routenparameter ohne Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="c3c7c-277">Parameter-platzhaltersegmente mit Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="c3c7c-278">Parameter-platzhaltersegmente ohne Einschränkungen.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="c3c7c-279">Routen werden im Fall von einem gleichwertigen Objekt ist durch einen Ordinalzeichenfolgenvergleich sortiert ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) von der routenvorlage.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="c3c7c-280">Im Folgenden ein Beispiel.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-280">Here is an example.</span></span> <span data-ttu-id="c3c7c-281">Nehmen wir an, dass Sie den folgenden Controller definieren:</span><span class="sxs-lookup"><span data-stu-id="c3c7c-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="c3c7c-282">Diese Routen werden wie folgt sortiert.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="c3c7c-283">Aufträge/details</span><span class="sxs-lookup"><span data-stu-id="c3c7c-283">orders/details</span></span>
2. <span data-ttu-id="c3c7c-284">Aufträge / {Id}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-284">orders/{id}</span></span>
3. <span data-ttu-id="c3c7c-285">Aufträge / {CustomerName}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-285">orders/{customerName}</span></span>
4. <span data-ttu-id="c3c7c-286">Aufträge / {\*Datum}</span><span class="sxs-lookup"><span data-stu-id="c3c7c-286">orders/{\*date}</span></span>
5. <span data-ttu-id="c3c7c-287">Aufträge / ausstehend</span><span class="sxs-lookup"><span data-stu-id="c3c7c-287">orders/pending</span></span>

<span data-ttu-id="c3c7c-288">Beachten Sie, dass "Details" ein literal-Segment ist und vor "{Id}" angezeigt wird, aber "Ausstehend" angezeigt wird: zuletzt die **RouteOrder** -Eigenschaft ist 1.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="c3c7c-289">(In diesem Beispiel wird vorausgesetzt, dass sind keine Kunden namens "Details" oder "Ausstehend".</span><span class="sxs-lookup"><span data-stu-id="c3c7c-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="c3c7c-290">Versuchen Sie es im Allgemeinen zu mehrdeutige Routen zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="c3c7c-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="c3c7c-291">In diesem Beispiel für eine bessere routenvorlage `GetByCustomer` ist "Customers / {CustomerName}")</span><span class="sxs-lookup"><span data-stu-id="c3c7c-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
