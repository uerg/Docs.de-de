---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing in ASP.NET Web-API | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 5e3bba70993dafcdd93feed52813ee80697b1038
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374204"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="4bc86-102">Routing in ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="4bc86-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="4bc86-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="4bc86-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="4bc86-104">In diesem Artikel wird beschrieben, wie ASP.NET Web-API die HTTP-Anforderungen an Controller weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="4bc86-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="4bc86-105">Wenn Sie mit ASP.NET MVC vertraut sind, ist ein routing der Web-API-MVC-routing sehr ähnlich.</span><span class="sxs-lookup"><span data-stu-id="4bc86-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="4bc86-106">Der Hauptunterschied besteht darin, dass die Web-API die HTTP-Methode, nicht den URI-Pfad verwendet, um die Aktion auswählen.</span><span class="sxs-lookup"><span data-stu-id="4bc86-106">The main difference is that Web API uses the HTTP method, not the URI path, to select the action.</span></span> <span data-ttu-id="4bc86-107">Sie können auch die MVC-Stil-routing in Web-API verwenden.</span><span class="sxs-lookup"><span data-stu-id="4bc86-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="4bc86-108">In diesem Artikel werden keine Kenntnisse von ASP.NET MVC vorausgesetzt.</span><span class="sxs-lookup"><span data-stu-id="4bc86-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>


## <a name="routing-tables"></a><span data-ttu-id="4bc86-109">Routingtabellen</span><span class="sxs-lookup"><span data-stu-id="4bc86-109">Routing Tables</span></span>

<span data-ttu-id="4bc86-110">In ASP.NET Web-API eine *Controller* ist eine Klasse, die HTTP-Anforderungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="4bc86-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="4bc86-111">Die öffentlichen Methoden des Controllers aufgerufen werden *Aktionsmethoden* oder einfach *Aktionen*.</span><span class="sxs-lookup"><span data-stu-id="4bc86-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="4bc86-112">Wenn die Web-API-Framework eine Anforderung empfängt, leitet sie die Anforderung mit einer Aktion.</span><span class="sxs-lookup"><span data-stu-id="4bc86-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="4bc86-113">Um zu bestimmen, welche Aktion aufrufen, das Framework verwendet eine *Routingtabelle*.</span><span class="sxs-lookup"><span data-stu-id="4bc86-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="4bc86-114">Die Visual Studio-Projektvorlage für Web-API erstellt eine Standardroute:</span><span class="sxs-lookup"><span data-stu-id="4bc86-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="4bc86-115">Diese Route wird definiert, in der Datei "WebApiConfig.cs", die in der App platziert wird\_Anfangsverzeichnis:</span><span class="sxs-lookup"><span data-stu-id="4bc86-115">This route is defined in the WebApiConfig.cs file, which is placed in the App\_Start directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="4bc86-116">Weitere Informationen zu den **WebApiConfig** Klasse, finden Sie unter [Konfigurieren von ASP.NET Web-API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="4bc86-116">For more information about the **WebApiConfig** class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="4bc86-117">Wenn Sie Web-API selbst hosten, müssen Sie die Routingtabelle festlegen, direkt auf die **HttpSelfHostConfiguration** Objekt.</span><span class="sxs-lookup"><span data-stu-id="4bc86-117">If you self-host Web API, you must set the routing table directly on the **HttpSelfHostConfiguration** object.</span></span> <span data-ttu-id="4bc86-118">Weitere Informationen finden Sie unter [selbst hosten einer Web-API](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="4bc86-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="4bc86-119">Jeder Eintrag in der Routingtabelle enthält eine *routenvorlage*.</span><span class="sxs-lookup"><span data-stu-id="4bc86-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="4bc86-120">Die Route-Standardvorlage für Web-API ist &quot;api / {Controller} / {Id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="4bc86-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="4bc86-121">In dieser Vorlage &quot;api&quot; ist ein literal OData-Pfadsegment, und klicken Sie auf {Controller} und {Id} Platzhaltervariablen sind.</span><span class="sxs-lookup"><span data-stu-id="4bc86-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="4bc86-122">Wenn die Web-API-Framework eine HTTP-Anforderung empfängt, versucht, den URI für eine der routenvorlagen in der Routingtabelle überein.</span><span class="sxs-lookup"><span data-stu-id="4bc86-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="4bc86-123">Wenn keine Route entspricht, erhält der Client einen 404-Fehler.</span><span class="sxs-lookup"><span data-stu-id="4bc86-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="4bc86-124">Die folgenden URIs entspricht z. B. die Standardroute:</span><span class="sxs-lookup"><span data-stu-id="4bc86-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="4bc86-125">/ api/contacts</span><span class="sxs-lookup"><span data-stu-id="4bc86-125">/api/contacts</span></span>
- <span data-ttu-id="4bc86-126">/API/Contacts/1</span><span class="sxs-lookup"><span data-stu-id="4bc86-126">/api/contacts/1</span></span>
- <span data-ttu-id="4bc86-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="4bc86-127">/api/products/gizmo1</span></span>

<span data-ttu-id="4bc86-128">Allerdings der folgende URI stimmt nicht überein, da es fehlen die &quot;api&quot; Segment:</span><span class="sxs-lookup"><span data-stu-id="4bc86-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="4bc86-129">/ Contacts/1</span><span class="sxs-lookup"><span data-stu-id="4bc86-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="4bc86-130">Der Grund für die Verwendung von "api" in der Route ist Konflikte mit ASP.NET MVC-Routings zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="4bc86-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="4bc86-131">Auf diese Weise kann man &quot;/kontaktiert&quot; wechseln Sie zu einem MVC-Controller und &quot;/api/Contacts&quot; wechseln Sie zu einem Web-API-Controller.</span><span class="sxs-lookup"><span data-stu-id="4bc86-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="4bc86-132">Wenn Sie diese Konvention nicht gefällt, können Sie natürlich die Standardroutingtabelle ändern.</span><span class="sxs-lookup"><span data-stu-id="4bc86-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="4bc86-133">Wenn eine übereinstimmende Route gefunden wurde, wählt Web-API-Controller und die Aktion:</span><span class="sxs-lookup"><span data-stu-id="4bc86-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="4bc86-134">Um den Controller zu suchen, Web-API fügt &quot;Controller&quot; auf den Wert des der *{Controller}* Variable.</span><span class="sxs-lookup"><span data-stu-id="4bc86-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="4bc86-135">Um die Aktion zu suchen, Web-API untersucht die HTTP-Methode, und sucht dann nach einer Aktion, deren Name mit diesem Namen der HTTP-Methode beginnt.</span><span class="sxs-lookup"><span data-stu-id="4bc86-135">To find the action, Web API looks at the HTTP method, and then looks for an action whose name begins with that HTTP method name.</span></span> <span data-ttu-id="4bc86-136">Mit einer GET-Anforderung Web-API sieht für eine Aktion, die mit beginnt &quot;abrufen... &quot;, z. B. &quot;GetContact&quot; oder &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="4bc86-136">For example, with a GET request, Web API looks for an action that starts with &quot;Get...&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="4bc86-137">Diese Konvention gilt nur für abrufen, POST, PUT und DELETE-Methoden.</span><span class="sxs-lookup"><span data-stu-id="4bc86-137">This convention applies only to GET, POST, PUT, and DELETE methods.</span></span> <span data-ttu-id="4bc86-138">Sie können andere HTTP-Methoden aktivieren, auf dem Controller mithilfe von Attributen.</span><span class="sxs-lookup"><span data-stu-id="4bc86-138">You can enable other HTTP methods by using attributes on your controller.</span></span> <span data-ttu-id="4bc86-139">Wir sehen später ein Beispiel.</span><span class="sxs-lookup"><span data-stu-id="4bc86-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="4bc86-140">Andere Platzhaltervariablen in der routenvorlage, z. B. *{Id},* Action-Parameter zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="4bc86-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="4bc86-141">Sehen wir uns ein Beispiel.</span><span class="sxs-lookup"><span data-stu-id="4bc86-141">Let's look at an example.</span></span> <span data-ttu-id="4bc86-142">Nehmen wir an, dass Sie den folgenden Controller definieren:</span><span class="sxs-lookup"><span data-stu-id="4bc86-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="4bc86-143">Hier sind einige möglichen HTTP-Anforderungen zusammen mit der Aktion, die für die einzelnen aufgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="4bc86-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="4bc86-144">HTTP-Methode</span><span class="sxs-lookup"><span data-stu-id="4bc86-144">HTTP Method</span></span> | <span data-ttu-id="4bc86-145">URI-Pfad</span><span class="sxs-lookup"><span data-stu-id="4bc86-145">URI Path</span></span> | <span data-ttu-id="4bc86-146">Aktion</span><span class="sxs-lookup"><span data-stu-id="4bc86-146">Action</span></span> | <span data-ttu-id="4bc86-147">Parameter</span><span class="sxs-lookup"><span data-stu-id="4bc86-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="4bc86-148">GET</span><span class="sxs-lookup"><span data-stu-id="4bc86-148">GET</span></span> | <span data-ttu-id="4bc86-149">API/Produkte</span><span class="sxs-lookup"><span data-stu-id="4bc86-149">api/products</span></span> | <span data-ttu-id="4bc86-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="4bc86-150">GetAllProducts</span></span> | <span data-ttu-id="4bc86-151">*(keine)*</span><span class="sxs-lookup"><span data-stu-id="4bc86-151">*(none)*</span></span> |
| <span data-ttu-id="4bc86-152">GET</span><span class="sxs-lookup"><span data-stu-id="4bc86-152">GET</span></span> | <span data-ttu-id="4bc86-153">API/Produkte/4</span><span class="sxs-lookup"><span data-stu-id="4bc86-153">api/products/4</span></span> | <span data-ttu-id="4bc86-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="4bc86-154">GetProductById</span></span> | <span data-ttu-id="4bc86-155">4</span><span class="sxs-lookup"><span data-stu-id="4bc86-155">4</span></span> |
| <span data-ttu-id="4bc86-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="4bc86-156">DELETE</span></span> | <span data-ttu-id="4bc86-157">API/Produkte/4</span><span class="sxs-lookup"><span data-stu-id="4bc86-157">api/products/4</span></span> | <span data-ttu-id="4bc86-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="4bc86-158">DeleteProduct</span></span> | <span data-ttu-id="4bc86-159">4</span><span class="sxs-lookup"><span data-stu-id="4bc86-159">4</span></span> |
| <span data-ttu-id="4bc86-160">POST</span><span class="sxs-lookup"><span data-stu-id="4bc86-160">POST</span></span> | <span data-ttu-id="4bc86-161">API/Produkte</span><span class="sxs-lookup"><span data-stu-id="4bc86-161">api/products</span></span> | <span data-ttu-id="4bc86-162">*(keine Übereinstimmung)*</span><span class="sxs-lookup"><span data-stu-id="4bc86-162">*(no match)*</span></span> |  |

<span data-ttu-id="4bc86-163">Beachten Sie, dass die *{Id}* -Segment des URI, falls vorhanden, zugeordnet ist die *Id* Parameter der Aktion.</span><span class="sxs-lookup"><span data-stu-id="4bc86-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="4bc86-164">In diesem Beispiel ist der Controller definiert zwei GET-Methoden, mit einem *Id* Parameter und einen ohne Parameter.</span><span class="sxs-lookup"><span data-stu-id="4bc86-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="4bc86-165">Beachten Sie auch, die die POST-Anforderung fehl, da der Controller nicht definiert ist, eine &quot;Post... &quot; Methode.</span><span class="sxs-lookup"><span data-stu-id="4bc86-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="4bc86-166">Routing-Varianten</span><span class="sxs-lookup"><span data-stu-id="4bc86-166">Routing Variations</span></span>

<span data-ttu-id="4bc86-167">Im vorherigen Abschnitt beschrieben, die grundlegende Routingmechanismus von ASP.NET Web-API.</span><span class="sxs-lookup"><span data-stu-id="4bc86-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="4bc86-168">Dieser Abschnitt beschreibt einige Varianten.</span><span class="sxs-lookup"><span data-stu-id="4bc86-168">This section describes some variations.</span></span>

### <a name="http-methods"></a><span data-ttu-id="4bc86-169">HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="4bc86-169">HTTP Methods</span></span>

<span data-ttu-id="4bc86-170">Anstatt die Namenskonvention für HTTP-Methoden zu verwenden, können Sie explizit die HTTP-Methode für eine Aktion angeben, werden, indem die Aktionsmethode mit den **HttpGet**, **HttpPut**, **HttpPost** , oder **HttpDelete** Attribut.</span><span class="sxs-lookup"><span data-stu-id="4bc86-170">Instead of using the naming convention for HTTP methods, you can explicitly specify the HTTP method for an action by decorating the action method with the **HttpGet**, **HttpPut**, **HttpPost**, or **HttpDelete** attribute.</span></span>

<span data-ttu-id="4bc86-171">Im folgenden Beispiel wird die Methode FindProduct GET-Anforderungen zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="4bc86-171">In the following example, the FindProduct method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="4bc86-172">Um mehrere HTTP-Methoden für eine Aktion zu ermöglichen, oder zum Zulassen von HTTP-Methoden als GET, PUT, POST und DELETE, verwenden Sie die **AcceptVerbs** -Attribut, das eine Liste von HTTP-Methoden akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="4bc86-172">To allow multiple HTTP methods for an action, or to allow HTTP methods other than GET, PUT, POST, and DELETE, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="4bc86-173">Routing durch den Namen der Aktion</span><span class="sxs-lookup"><span data-stu-id="4bc86-173">Routing by Action Name</span></span>

<span data-ttu-id="4bc86-174">Mit der Standard-routing-Vorlage verwendet die Web-API die HTTP-Methode zum Auswählen der Aktion.</span><span class="sxs-lookup"><span data-stu-id="4bc86-174">With the default routing template, Web API uses the HTTP method to select the action.</span></span> <span data-ttu-id="4bc86-175">Allerdings können Sie auch eine Route erstellen, in dem der Name der Aktion im URI enthalten ist:</span><span class="sxs-lookup"><span data-stu-id="4bc86-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="4bc86-176">In dieser routenvorlage die *{Action}* Parameternamen die Aktionsmethode im Controller.</span><span class="sxs-lookup"><span data-stu-id="4bc86-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="4bc86-177">Verwenden Sie in diesem Format des Routings Attribute, um die zulässigen HTTP-Methoden geben.</span><span class="sxs-lookup"><span data-stu-id="4bc86-177">With this style of routing, use attributes to specify the allowed HTTP methods.</span></span> <span data-ttu-id="4bc86-178">Nehmen wir beispielsweise an, dass Ihre Controller die folgende Methode verfügt:</span><span class="sxs-lookup"><span data-stu-id="4bc86-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="4bc86-179">In diesem Fall würden eine GET-Anforderung für "api/Produkte/Details/1" die Details-Methode zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="4bc86-179">In this case, a GET request for "api/products/details/1" would map to the Details method.</span></span> <span data-ttu-id="4bc86-180">Diese Art von routing ist vergleichbar mit ASP.NET MVC, und für eine RPC-Stil API eignet sich möglicherweise.</span><span class="sxs-lookup"><span data-stu-id="4bc86-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="4bc86-181">Sie können den Aktionsnamen überschreiben, indem die **ActionName** Attribut.</span><span class="sxs-lookup"><span data-stu-id="4bc86-181">You can override the action name by using the **ActionName** attribute.</span></span> <span data-ttu-id="4bc86-182">Im folgenden Beispiel sind zwei Aktionen, die den zuordnen &quot;-api/Produkte/Miniaturansicht/*Id*. Eine GET unterstützt, und die andere POST unterstützt:</span><span class="sxs-lookup"><span data-stu-id="4bc86-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="4bc86-183">Nicht-Aktionen</span><span class="sxs-lookup"><span data-stu-id="4bc86-183">Non-Actions</span></span>

<span data-ttu-id="4bc86-184">Um zu verhindern, dass eine Methode abrufen als Aktion aufgerufen wird, verwenden Sie die **NonAction** Attribut.</span><span class="sxs-lookup"><span data-stu-id="4bc86-184">To prevent a method from getting invoked as an action, use the **NonAction** attribute.</span></span> <span data-ttu-id="4bc86-185">Dies signalisiert das Framework, dass die Methode keine Aktion, auch wenn sie andernfalls die Senderegeln übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="4bc86-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="4bc86-186">Weiterführende Themen</span><span class="sxs-lookup"><span data-stu-id="4bc86-186">Further Reading</span></span>

<span data-ttu-id="4bc86-187">In diesem Thema bereitgestellten einen allgemeinen Überblick über die Weiterleitung an.</span><span class="sxs-lookup"><span data-stu-id="4bc86-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="4bc86-188">Weitere Informationen finden Sie unter [Routing- und Aktionsauswahl](routing-and-action-selection.md), das beschreibt, genau wie das Framework einen URI zu einer Route übereinstimmt, wählt einen Controller und wählt dann die aufzurufende Aktion.</span><span class="sxs-lookup"><span data-stu-id="4bc86-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
