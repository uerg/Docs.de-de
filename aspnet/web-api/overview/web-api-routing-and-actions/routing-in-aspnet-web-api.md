---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Routing in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26509259"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="e30b5-102">Routing in ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="e30b5-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="e30b5-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e30b5-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="e30b5-104">In diesem Artikel wird beschrieben, wie ASP.NET Web-API-HTTP-Anfragen an Domänencontroller weiterleitet.</span><span class="sxs-lookup"><span data-stu-id="e30b5-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="e30b5-105">Wenn Sie mit ASP.NET MVC vertraut sind, ist das routing der Web-API MVC-routing sehr ähnlich.</span><span class="sxs-lookup"><span data-stu-id="e30b5-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="e30b5-106">Der Hauptunterschied besteht darin, dass Web-API den HTTP-Methode, die nicht den URI-Pfad verwendet, um die Aktion auswählen.</span><span class="sxs-lookup"><span data-stu-id="e30b5-106">The main difference is that Web API uses the HTTP method, not the URI path, to select the action.</span></span> <span data-ttu-id="e30b5-107">Sie können auch die MVC-Stil routing in der Web-API verwenden.</span><span class="sxs-lookup"><span data-stu-id="e30b5-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="e30b5-108">In diesem Artikel geht keine Kenntnisse über ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="e30b5-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>


## <a name="routing-tables"></a><span data-ttu-id="e30b5-109">Routingtabellen</span><span class="sxs-lookup"><span data-stu-id="e30b5-109">Routing Tables</span></span>

<span data-ttu-id="e30b5-110">In ASP.NET Web-API eine *Controller* ist eine Klasse, die HTTP-Anforderungen verarbeitet.</span><span class="sxs-lookup"><span data-stu-id="e30b5-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="e30b5-111">Die öffentlichen Methoden des Controllers heißen *Aktionsmethoden* oder einfach *Aktionen*.</span><span class="sxs-lookup"><span data-stu-id="e30b5-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="e30b5-112">Wenn die Web-API-Framework eine Anforderung empfängt, leitet die Anforderung an eine Aktion weiter.</span><span class="sxs-lookup"><span data-stu-id="e30b5-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="e30b5-113">Um zu bestimmen, welche Aktion aufrufen, das Framework verwendet eine *Routingtabelle*.</span><span class="sxs-lookup"><span data-stu-id="e30b5-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="e30b5-114">Die Visual Studio-Projektvorlage für Web-API erstellt eine Standardroute an:</span><span class="sxs-lookup"><span data-stu-id="e30b5-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="e30b5-115">Diese Route wird definiert, in der WebApiConfig.cs-Datei, die in der App abgelegt wird\_Startverzeichnis:</span><span class="sxs-lookup"><span data-stu-id="e30b5-115">This route is defined in the WebApiConfig.cs file, which is placed in the App\_Start directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="e30b5-116">Weitere Informationen zu den **"webapiconfig"** Klasse, finden Sie unter [Konfigurieren von ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e30b5-116">For more information about the **WebApiConfig** class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="e30b5-117">Wenn Sie Web-API selbst hosten, müssen Sie die Routingtabelle festlegen, direkt auf die **HttpSelfHostConfiguration** Objekt.</span><span class="sxs-lookup"><span data-stu-id="e30b5-117">If you self-host Web API, you must set the routing table directly on the **HttpSelfHostConfiguration** object.</span></span> <span data-ttu-id="e30b5-118">Weitere Informationen finden Sie unter [Selbsthosting einer Web-API](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="e30b5-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="e30b5-119">Jeder Eintrag in der Routingtabelle enthält eine *routenvorlage*.</span><span class="sxs-lookup"><span data-stu-id="e30b5-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="e30b5-120">Die Standardvorlage für die Route für die Web-API ist &quot;api / {Controller} / {Id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="e30b5-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="e30b5-121">In dieser Vorlage &quot;api&quot; ist ein literal Pfadsegment und {Controller} und {Id} Platzhaltervariablen sind.</span><span class="sxs-lookup"><span data-stu-id="e30b5-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="e30b5-122">Wenn die Web-API-Framework eine HTTP-Anforderung empfängt, wird versucht, den URI für eine der routenvorlagen in der Routingtabelle zuzuordnen.</span><span class="sxs-lookup"><span data-stu-id="e30b5-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="e30b5-123">Wenn keine Route übereinstimmt, empfängt der Client einen 404-Fehler.</span><span class="sxs-lookup"><span data-stu-id="e30b5-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="e30b5-124">Z. B. die folgenden URIs entspricht die Standardroute:</span><span class="sxs-lookup"><span data-stu-id="e30b5-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="e30b5-125">/ api/Kontakte</span><span class="sxs-lookup"><span data-stu-id="e30b5-125">/api/contacts</span></span>
- <span data-ttu-id="e30b5-126">/API/Contacts/1</span><span class="sxs-lookup"><span data-stu-id="e30b5-126">/api/contacts/1</span></span>
- <span data-ttu-id="e30b5-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="e30b5-127">/api/products/gizmo1</span></span>

<span data-ttu-id="e30b5-128">Allerdings der folgende URI stimmt nicht überein, da es fehlt die &quot;api&quot; Segment:</span><span class="sxs-lookup"><span data-stu-id="e30b5-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="e30b5-129">/ Contacts/1</span><span class="sxs-lookup"><span data-stu-id="e30b5-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="e30b5-130">Der Grund für die Verwendung von "-api" in der Route ist mit ASP.NET MVC-Routings Konflikte zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="e30b5-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="e30b5-131">Auf diese Weise haben Sie &quot;/Kontakte&quot; wechseln Sie zu einer MVC-Controller und &quot;/api/Kontakte&quot; wechseln Sie zu einem Web-API-Controller.</span><span class="sxs-lookup"><span data-stu-id="e30b5-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="e30b5-132">Wenn Sie diese Konvention nicht mögen, können Sie natürlich die Routentabelle Standardeinstellung ändern.</span><span class="sxs-lookup"><span data-stu-id="e30b5-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="e30b5-133">Sobald eine übereinstimmende Route gefunden wird, wählt Web-API der Controller und die Aktion an:</span><span class="sxs-lookup"><span data-stu-id="e30b5-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="e30b5-134">Um den Controller zu suchen, Web-API fügt &quot;Controller&quot; auf den Wert von der *{Controller}* Variable.</span><span class="sxs-lookup"><span data-stu-id="e30b5-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="e30b5-135">Um die Aktion zu suchen, Web-API prüft die HTTP-Methode, und sucht dann nach einer Aktion mit diesem Namen der HTTP-Methode, deren Name beginnt.</span><span class="sxs-lookup"><span data-stu-id="e30b5-135">To find the action, Web API looks at the HTTP method, and then looks for an action whose name begins with that HTTP method name.</span></span> <span data-ttu-id="e30b5-136">Z. B. mit einer GET-Anforderung Web-API sucht eine Aktion, die mit beginnt &quot;abrufen... &quot;, wie z. B. &quot;GetContact&quot; oder &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="e30b5-136">For example, with a GET request, Web API looks for an action that starts with &quot;Get...&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="e30b5-137">Diese Konvention gilt nur für GET, POST, PUT- und DELETE-Methoden.</span><span class="sxs-lookup"><span data-stu-id="e30b5-137">This convention applies only to GET, POST, PUT, and DELETE methods.</span></span> <span data-ttu-id="e30b5-138">Sie können andere HTTP-Methoden mithilfe von Attributen auf dem Controller aktivieren.</span><span class="sxs-lookup"><span data-stu-id="e30b5-138">You can enable other HTTP methods by using attributes on your controller.</span></span> <span data-ttu-id="e30b5-139">Wir sehen später ein Beispiel an.</span><span class="sxs-lookup"><span data-stu-id="e30b5-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="e30b5-140">Andere Platzhaltervariablen in der routenvorlage, wie z. B. *{Id},* Aktionsparameter zugeordnet sind.</span><span class="sxs-lookup"><span data-stu-id="e30b5-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="e30b5-141">Sehen wir uns ein Beispiel.</span><span class="sxs-lookup"><span data-stu-id="e30b5-141">Let's look at an example.</span></span> <span data-ttu-id="e30b5-142">Nehmen wir an, dass Sie die folgenden Controller definieren:</span><span class="sxs-lookup"><span data-stu-id="e30b5-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="e30b5-143">Hier sind einige möglichen HTTP-Anforderungen, zusammen mit der Aktion, die für die einzelnen aufgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="e30b5-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="e30b5-144">HTTP-Methode</span><span class="sxs-lookup"><span data-stu-id="e30b5-144">HTTP Method</span></span> | <span data-ttu-id="e30b5-145">URI-Pfad</span><span class="sxs-lookup"><span data-stu-id="e30b5-145">URI Path</span></span> | <span data-ttu-id="e30b5-146">Aktion</span><span class="sxs-lookup"><span data-stu-id="e30b5-146">Action</span></span> | <span data-ttu-id="e30b5-147">Parameter</span><span class="sxs-lookup"><span data-stu-id="e30b5-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="e30b5-148">GET</span><span class="sxs-lookup"><span data-stu-id="e30b5-148">GET</span></span> | <span data-ttu-id="e30b5-149">API-Produkte</span><span class="sxs-lookup"><span data-stu-id="e30b5-149">api/products</span></span> | <span data-ttu-id="e30b5-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="e30b5-150">GetAllProducts</span></span> | <span data-ttu-id="e30b5-151">*(keine)*</span><span class="sxs-lookup"><span data-stu-id="e30b5-151">*(none)*</span></span> |
| <span data-ttu-id="e30b5-152">GET</span><span class="sxs-lookup"><span data-stu-id="e30b5-152">GET</span></span> | <span data-ttu-id="e30b5-153">-API/Produkte/4</span><span class="sxs-lookup"><span data-stu-id="e30b5-153">api/products/4</span></span> | <span data-ttu-id="e30b5-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="e30b5-154">GetProductById</span></span> | <span data-ttu-id="e30b5-155">4</span><span class="sxs-lookup"><span data-stu-id="e30b5-155">4</span></span> |
| <span data-ttu-id="e30b5-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="e30b5-156">DELETE</span></span> | <span data-ttu-id="e30b5-157">-API/Produkte/4</span><span class="sxs-lookup"><span data-stu-id="e30b5-157">api/products/4</span></span> | <span data-ttu-id="e30b5-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="e30b5-158">DeleteProduct</span></span> | <span data-ttu-id="e30b5-159">4</span><span class="sxs-lookup"><span data-stu-id="e30b5-159">4</span></span> |
| <span data-ttu-id="e30b5-160">BEREITSTELLEN</span><span class="sxs-lookup"><span data-stu-id="e30b5-160">POST</span></span> | <span data-ttu-id="e30b5-161">API-Produkte</span><span class="sxs-lookup"><span data-stu-id="e30b5-161">api/products</span></span> | <span data-ttu-id="e30b5-162">*(keine Übereinstimmung)*</span><span class="sxs-lookup"><span data-stu-id="e30b5-162">*(no match)*</span></span> |  |

<span data-ttu-id="e30b5-163">Beachten Sie, dass die *{Id}* Segment des URIS, falls vorhanden, zugeordnet ist die *Id* Parameter der Aktion.</span><span class="sxs-lookup"><span data-stu-id="e30b5-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="e30b5-164">In diesem Beispiel wird der Controller definiert zwei GET-Methoden, mit einer *Id* Parameter und eine ohne Parameter.</span><span class="sxs-lookup"><span data-stu-id="e30b5-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="e30b5-165">Beachten Sie auch, die die POST-Anforderung fehl, weil der Domänencontroller nicht definiert wird, eine &quot;Post... &quot; Methode.</span><span class="sxs-lookup"><span data-stu-id="e30b5-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="e30b5-166">Routing Varianten</span><span class="sxs-lookup"><span data-stu-id="e30b5-166">Routing Variations</span></span>

<span data-ttu-id="e30b5-167">Im vorherigen Abschnitt beschrieben, die grundlegende Routingmechanismus von ASP.NET Web-API.</span><span class="sxs-lookup"><span data-stu-id="e30b5-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="e30b5-168">Dieser Abschnitt beschreibt einige Varianten.</span><span class="sxs-lookup"><span data-stu-id="e30b5-168">This section describes some variations.</span></span>

### <a name="http-methods"></a><span data-ttu-id="e30b5-169">HTTP-Methoden</span><span class="sxs-lookup"><span data-stu-id="e30b5-169">HTTP Methods</span></span>

<span data-ttu-id="e30b5-170">Anstatt die Namenskonvention für HTTP-Methoden zu verwenden, können Sie explizit die HTTP-Methode für eine Aktion angeben, werden, indem die Aktionsmethode mit den **HttpGet**, **HttpPut**, **HttpPost** , oder **HttpDelete** Attribut.</span><span class="sxs-lookup"><span data-stu-id="e30b5-170">Instead of using the naming convention for HTTP methods, you can explicitly specify the HTTP method for an action by decorating the action method with the **HttpGet**, **HttpPut**, **HttpPost**, or **HttpDelete** attribute.</span></span>

<span data-ttu-id="e30b5-171">Im folgenden Beispiel wird die Methode FindProduct GET-Anforderungen zugeordnet:</span><span class="sxs-lookup"><span data-stu-id="e30b5-171">In the following example, the FindProduct method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="e30b5-172">Um mehrere HTTP-Methoden für eine Aktion, oder um einen HTTP-Methoden als GET, PUT, POST und DELETE, verwenden Sie die **AcceptVerbs** Attribut, das eine Liste der HTTP-Methoden behandelt.</span><span class="sxs-lookup"><span data-stu-id="e30b5-172">To allow multiple HTTP methods for an action, or to allow HTTP methods other than GET, PUT, POST, and DELETE, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="e30b5-173">Weiterleitung und der Aktionsname</span><span class="sxs-lookup"><span data-stu-id="e30b5-173">Routing by Action Name</span></span>

<span data-ttu-id="e30b5-174">Mit dem routing Standardvorlage verwendet Web-API die HTTP-Methode, um die Aktion auswählen.</span><span class="sxs-lookup"><span data-stu-id="e30b5-174">With the default routing template, Web API uses the HTTP method to select the action.</span></span> <span data-ttu-id="e30b5-175">Allerdings können Sie auch eine Route erstellen, in dem der Aktionsname im URI enthalten ist:</span><span class="sxs-lookup"><span data-stu-id="e30b5-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="e30b5-176">In dieser routenvorlage die *{Aktion}* Parameter den Namen der Aktionsmethode auf dem Controller.</span><span class="sxs-lookup"><span data-stu-id="e30b5-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="e30b5-177">Verwenden Sie in diesem Format Routing Attribute, um die zulässigen HTTP-Methoden angeben.</span><span class="sxs-lookup"><span data-stu-id="e30b5-177">With this style of routing, use attributes to specify the allowed HTTP methods.</span></span> <span data-ttu-id="e30b5-178">Angenommen Sie, dass der Controller die folgende Methode hat:</span><span class="sxs-lookup"><span data-stu-id="e30b5-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="e30b5-179">In diesem Fall würde eine GET-Anforderung für "api/Produkte/Informationen/1" die Details-Methode zuordnen.</span><span class="sxs-lookup"><span data-stu-id="e30b5-179">In this case, a GET request for "api/products/details/1" would map to the Details method.</span></span> <span data-ttu-id="e30b5-180">Diese Art der routing ähnelt dem ASP.NET MVC und kann für eine RPC-Stil API geeignet sein.</span><span class="sxs-lookup"><span data-stu-id="e30b5-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="e30b5-181">Sie können den Aktionsnamen überschreiben, indem die **ActionName** Attribut.</span><span class="sxs-lookup"><span data-stu-id="e30b5-181">You can override the action name by using the **ActionName** attribute.</span></span> <span data-ttu-id="e30b5-182">Im folgenden Beispiel sind zwei Aktionen, die zugeordnet &quot;-api/Produkte/Miniaturansicht/*Id*. GET unterstützt, und die andere POST unterstützt:</span><span class="sxs-lookup"><span data-stu-id="e30b5-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="e30b5-183">Nicht-Aktionen</span><span class="sxs-lookup"><span data-stu-id="e30b5-183">Non-Actions</span></span>

<span data-ttu-id="e30b5-184">Um zu verhindern, dass eine Methode erste als Aktion aufgerufen wird, verwenden die **NonAction** Attribut.</span><span class="sxs-lookup"><span data-stu-id="e30b5-184">To prevent a method from getting invoked as an action, use the **NonAction** attribute.</span></span> <span data-ttu-id="e30b5-185">Dies signalisiert das Framework, dass die Methode nicht auf eine Aktion ist, selbst wenn andernfalls die Senderegeln abgeglichen werden würden.</span><span class="sxs-lookup"><span data-stu-id="e30b5-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="e30b5-186">Weiterführende Themen</span><span class="sxs-lookup"><span data-stu-id="e30b5-186">Further Reading</span></span>

<span data-ttu-id="e30b5-187">In diesem Thema bereitgestellten einen allgemeinen Überblick über das routing.</span><span class="sxs-lookup"><span data-stu-id="e30b5-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="e30b5-188">Weitere Informationen finden Sie unter [Routing und Aktionsauswahl](routing-and-action-selection.md), das genau wie das Framework einen URI zu einer Route übereinstimmt, wählt einen Controller aus und wählt dann die aufzurufende Aktion beschreibt.</span><span class="sxs-lookup"><span data-stu-id="e30b5-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
