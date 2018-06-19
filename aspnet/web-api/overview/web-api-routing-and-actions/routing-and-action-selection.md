---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Routing und Aktionsauswahl in ASP.NET Web-API | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 997582263bd48590b74434ee0ffc6be928fa1e08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
ms.locfileid: "28043417"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="de4e2-102">Routing und Aktionsauswahl in ASP.NET Web-API</span><span class="sxs-lookup"><span data-stu-id="de4e2-102">Routing and Action Selection in ASP.NET Web API</span></span>
====================
<span data-ttu-id="de4e2-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="de4e2-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="de4e2-104">In diesem Artikel wird beschrieben, wie ASP.NET Web-API eine HTTP-Anforderung an eine bestimmte Aktion auf einem Domänencontroller weiterleitet.</span><span class="sxs-lookup"><span data-stu-id="de4e2-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="de4e2-105">Eine allgemeine Übersicht zum routing finden Sie unter [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="de4e2-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>


<span data-ttu-id="de4e2-106">Dieser Artikel beschäftigt sich mit den Details der routing-Prozess.</span><span class="sxs-lookup"><span data-stu-id="de4e2-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="de4e2-107">Wenn Sie ein Web-API-Projekt erstellen, und suchen, die einige Anforderungen erhalten nicht weitergeleitet, die Möglichkeit, die Sie erwarten, wird hoffentlich treten bei diesen Artikel nützlich.</span><span class="sxs-lookup"><span data-stu-id="de4e2-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="de4e2-108">Routing hat drei Hauptphasen:</span><span class="sxs-lookup"><span data-stu-id="de4e2-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="de4e2-109">Übereinstimmende den URI, der eine routenvorlage.</span><span class="sxs-lookup"><span data-stu-id="de4e2-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="de4e2-110">Bei der Auswahl eines Controllers.</span><span class="sxs-lookup"><span data-stu-id="de4e2-110">Selecting a controller.</span></span>
3. <span data-ttu-id="de4e2-111">Aktion auswählen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-111">Selecting an action.</span></span>

<span data-ttu-id="de4e2-112">Sie können einige Teile des Prozesses durch Ihre eigenen benutzerdefinierten Verhalten ersetzen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="de4e2-113">In diesem Artikel wird das standardmäßige Verhalten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="de4e2-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="de4e2-114">Zum Schluss beachten ich überall, wo Sie das Verhalten anpassen können.</span><span class="sxs-lookup"><span data-stu-id="de4e2-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="de4e2-115">Routenvorlagen</span><span class="sxs-lookup"><span data-stu-id="de4e2-115">Route Templates</span></span>

<span data-ttu-id="de4e2-116">Eine routenvorlage ähnelt einem URI-Pfad kann, er jedoch Platzhalterwerte, mit geschweiften Klammern angegeben:</span><span class="sxs-lookup"><span data-stu-id="de4e2-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="de4e2-117">Wenn Sie eine Route erstellen, können Sie Standardwerte für einige oder alle der Platzhalter bereitstellen:</span><span class="sxs-lookup"><span data-stu-id="de4e2-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="de4e2-118">Sie können auch Einschränkungen, angeben, die einschränken, wie einen Platzhalter für ein URI-Segment verglichen werden kann:</span><span class="sxs-lookup"><span data-stu-id="de4e2-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="de4e2-119">Das Framework versucht, die Segmente im URI-Pfad der Vorlage übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="de4e2-120">Literale in der Vorlage müssen genau übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="de4e2-121">Ein Platzhalter entspricht keinem Wert, es sei denn, Sie Einschränkungen angeben.</span><span class="sxs-lookup"><span data-stu-id="de4e2-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="de4e2-122">Das Framework stimmt nicht mit anderen Teile des URIS, z. B. den Hostnamen oder die Abfrageparameter überein.</span><span class="sxs-lookup"><span data-stu-id="de4e2-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="de4e2-123">Das Framework wählt die erste Route in der Routingtabelle, die den URI entspricht.</span><span class="sxs-lookup"><span data-stu-id="de4e2-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="de4e2-124">Es gibt zwei spezielle Platzhalter: "{Controller}" und "{Aktion}".</span><span class="sxs-lookup"><span data-stu-id="de4e2-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="de4e2-125">"{Controller}" enthält den Namen des Controllers.</span><span class="sxs-lookup"><span data-stu-id="de4e2-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="de4e2-126">"{Aktion}" enthält den Namen der Aktion.</span><span class="sxs-lookup"><span data-stu-id="de4e2-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="de4e2-127">Die übliche Konvention werden in der Web-API "{Aktion}" weggelassen werden soll.</span><span class="sxs-lookup"><span data-stu-id="de4e2-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="de4e2-128">der Arbeitszeittabelle</span><span class="sxs-lookup"><span data-stu-id="de4e2-128">Defaults</span></span>

<span data-ttu-id="de4e2-129">Wenn Sie die Standardwerte angeben, entspricht die Route einen URI, den dieser Segmente nicht vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="de4e2-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="de4e2-130">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="de4e2-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="de4e2-131">Der URI "`http://localhost/api/products`" diese Route.</span><span class="sxs-lookup"><span data-stu-id="de4e2-131">The URI "`http://localhost/api/products`" matches this route.</span></span> <span data-ttu-id="de4e2-132">Das Segment "{Kategorie}" ist den Standardwert "alle" zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="de4e2-132">The "{category}" segment is assigned the default value "all".</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="de4e2-133">Routenwörterbuchs</span><span class="sxs-lookup"><span data-stu-id="de4e2-133">Route Dictionary</span></span>

<span data-ttu-id="de4e2-134">Wenn das Framework eine Übereinstimmung für einen URI findet, erstellt es ein Wörterbuch, das den Wert für jeden Platzhalter enthält.</span><span class="sxs-lookup"><span data-stu-id="de4e2-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="de4e2-135">Die Schlüssel sind die Platzhalternamen, nicht einschließlich der geschweiften Klammern.</span><span class="sxs-lookup"><span data-stu-id="de4e2-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="de4e2-136">Die Werte stammen aus dem URI-Pfad oder von den Standardwerten.</span><span class="sxs-lookup"><span data-stu-id="de4e2-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="de4e2-137">Das Wörterbuch wird gespeichert, der **IHttpRouteData** Objekt.</span><span class="sxs-lookup"><span data-stu-id="de4e2-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="de4e2-138">Während dieser Phase übereinstimmende Route werden die speziellen "{Controller}" und "{Aktion}" Platzhalter wie die anderen Platzhalter behandelt.</span><span class="sxs-lookup"><span data-stu-id="de4e2-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="de4e2-139">Sie werden einfach in das Wörterbuch mit den anderen Werten gespeichert.</span><span class="sxs-lookup"><span data-stu-id="de4e2-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="de4e2-140">Ein Standardwert kann den speziellen Wert ist **RouteParameter.Optional**.</span><span class="sxs-lookup"><span data-stu-id="de4e2-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="de4e2-141">Wenn ein Platzhalter für diesen Wert zugewiesen wird, wird der Wert des Routenwörterbuchs nicht hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="de4e2-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="de4e2-142">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="de4e2-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="de4e2-143">Für den URI-Pfad "-api/Products" enthält des Routenwörterbuchs:</span><span class="sxs-lookup"><span data-stu-id="de4e2-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="de4e2-144">Controller: "Products"</span><span class="sxs-lookup"><span data-stu-id="de4e2-144">controller: "products"</span></span>
- <span data-ttu-id="de4e2-145">category: "all"</span><span class="sxs-lookup"><span data-stu-id="de4e2-145">category: "all"</span></span>

<span data-ttu-id="de4e2-146">Für "api/Produkte/Toys/123" wird die Routenwörterbuchs enthalten:</span><span class="sxs-lookup"><span data-stu-id="de4e2-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="de4e2-147">Controller: "Products"</span><span class="sxs-lookup"><span data-stu-id="de4e2-147">controller: "products"</span></span>
- <span data-ttu-id="de4e2-148">Kategorie: "Toys"</span><span class="sxs-lookup"><span data-stu-id="de4e2-148">category: "toys"</span></span>
- <span data-ttu-id="de4e2-149">id: "123"</span><span class="sxs-lookup"><span data-stu-id="de4e2-149">id: "123"</span></span>

<span data-ttu-id="de4e2-150">Die Standardwerte können auch einen Wert, der nicht, eine beliebige Stelle angezeigt wird in der routenvorlage einschließen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="de4e2-151">Wenn die Route übereinstimmt, wird dieser Wert im Wörterbuch gespeichert.</span><span class="sxs-lookup"><span data-stu-id="de4e2-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="de4e2-152">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="de4e2-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="de4e2-153">Wenn der URI-Pfad "Root/api/8" ist, wird das Wörterbuch zwei Werte enthalten:</span><span class="sxs-lookup"><span data-stu-id="de4e2-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="de4e2-154">Controller: "Customers"</span><span class="sxs-lookup"><span data-stu-id="de4e2-154">controller: "customers"</span></span>
- <span data-ttu-id="de4e2-155">id: "8"</span><span class="sxs-lookup"><span data-stu-id="de4e2-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="de4e2-156">Auswählen eines Controllers</span><span class="sxs-lookup"><span data-stu-id="de4e2-156">Selecting a Controller</span></span>

<span data-ttu-id="de4e2-157">Auswahl der Domänencontroller erfolgt durch die **IHttpControllerSelector.SelectController** Methode.</span><span class="sxs-lookup"><span data-stu-id="de4e2-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="de4e2-158">Diese Methode nimmt ein **HttpRequestMessage** Instanz und gibt eine **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="de4e2-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="de4e2-159">Die standardmäßige Implementierung erfolgt durch die **DefaultHttpControllerSelector** Klasse.</span><span class="sxs-lookup"><span data-stu-id="de4e2-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="de4e2-160">Diese Klasse verwendet eine einfache Algorithmen:</span><span class="sxs-lookup"><span data-stu-id="de4e2-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="de4e2-161">Suchen Sie in der Routenwörterbuchs für den Schlüssel "Controller".</span><span class="sxs-lookup"><span data-stu-id="de4e2-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="de4e2-162">Der Wert für diesen Schlüssel, und fügen Sie die Zeichenfolge "Controller" zum Abrufen des Namens der Controller-Typ.</span><span class="sxs-lookup"><span data-stu-id="de4e2-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="de4e2-163">Suchen Sie nach einem Web-API-Controller mit diesem Namen geben.</span><span class="sxs-lookup"><span data-stu-id="de4e2-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="de4e2-164">Des Routenwörterbuchs enthält die Schlüssel / Wert-Paar "Controller" = "Products", z. B. der Typ des Controllers "ProductsController" ist.</span><span class="sxs-lookup"><span data-stu-id="de4e2-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="de4e2-165">Wenn keine übereinstimmenden Typ oder mehrere Übereinstimmungen vorhanden ist, gibt das Framework einen Fehler an den Client zurück.</span><span class="sxs-lookup"><span data-stu-id="de4e2-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="de4e2-166">Schritt 3 **DefaultHttpControllerSelector** verwendet die **IHttpControllerTypeResolver** Schnittstelle, um die Liste der Web-API-Controllertypen abzurufen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="de4e2-167">Die standardmäßige Implementierung des **IHttpControllerTypeResolver** gibt alle öffentlichen Klassen implementieren (a) **IHttpController**, (b) sind nicht abstrakt und (c) verfügen über einen Namen, die mit "Controller" enden.</span><span class="sxs-lookup"><span data-stu-id="de4e2-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="de4e2-168">Aktionsauswahl</span><span class="sxs-lookup"><span data-stu-id="de4e2-168">Action Selection</span></span>

<span data-ttu-id="de4e2-169">Nach der Auswahl des Controllers, wählt das Framework die Aktion durch Aufrufen der **IHttpActionSelector.SelectAction** Methode.</span><span class="sxs-lookup"><span data-stu-id="de4e2-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="de4e2-170">Diese Methode nimmt ein **HttpControllerContext** und gibt eine **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="de4e2-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="de4e2-171">Die standardmäßige Implementierung erfolgt durch die **ApiControllerActionSelector** Klasse.</span><span class="sxs-lookup"><span data-stu-id="de4e2-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="de4e2-172">Um eine Aktion auswählen, prüft es Folgendes:</span><span class="sxs-lookup"><span data-stu-id="de4e2-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="de4e2-173">Die HTTP-Methode der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="de4e2-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="de4e2-174">Der Platzhalter für "{Aktion}" in der routenvorlage, falls vorhanden.</span><span class="sxs-lookup"><span data-stu-id="de4e2-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="de4e2-175">Die Parameter der Aktionen auf dem Controller.</span><span class="sxs-lookup"><span data-stu-id="de4e2-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="de4e2-176">Vor dem ansehen des Auswahl-Algorithmus, müssen wir einige Dinge Controlleraktionen zu verstehen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="de4e2-177">**Welche Methoden auf dem Controller "Aktionen" berücksichtigt werden?**</span><span class="sxs-lookup"><span data-stu-id="de4e2-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="de4e2-178">Wenn Sie eine Aktion auswählen, prüft das Framework nur öffentliche Instanzmethoden auf dem Controller.</span><span class="sxs-lookup"><span data-stu-id="de4e2-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="de4e2-179">Darüber hinaus schließt er ["spezielle Name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) Methoden (Konstruktoren, Ereignisse, operatorüberladungen usw.) und von geerbten Methoden der **ApiController** Klasse.</span><span class="sxs-lookup"><span data-stu-id="de4e2-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="de4e2-180">**HTTP-Methoden.**</span><span class="sxs-lookup"><span data-stu-id="de4e2-180">**HTTP Methods.**</span></span> <span data-ttu-id="de4e2-181">Das Framework entscheidet sich nur auf Aktionen, die entsprechen die HTTP-Methode der Anforderung, wie folgt bestimmt:</span><span class="sxs-lookup"><span data-stu-id="de4e2-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="de4e2-182">Sie können die HTTP-Methode mit einem Attribut angeben: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **"HttpOptions"**, **HttpPatch**, **HttpPost**, oder **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="de4e2-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="de4e2-183">Andernfalls, wenn der Name der Controllermethode mit "Get", "Post", "Put", "Delete", "Head", "Optionen" oder "Patch-" beginnt, klicken Sie dann gemäß der Konvention die Aktion unterstützt HTTP-Methode.</span><span class="sxs-lookup"><span data-stu-id="de4e2-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="de4e2-184">Wenn keine der genannten Möglichkeiten, unterstützt die Methode POST aus.</span><span class="sxs-lookup"><span data-stu-id="de4e2-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="de4e2-185">**Parameterbindungen.**</span><span class="sxs-lookup"><span data-stu-id="de4e2-185">**Parameter Bindings.**</span></span> <span data-ttu-id="de4e2-186">Eine parameterbindung ist wie Web-API einen Wert für einen Parameter erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="de4e2-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="de4e2-187">So sieht die Standardregel für die parameterbindung aus:</span><span class="sxs-lookup"><span data-stu-id="de4e2-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="de4e2-188">Einfache Typen stammen aus dem URI.</span><span class="sxs-lookup"><span data-stu-id="de4e2-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="de4e2-189">Komplexe Typen stammen aus dem Anforderungstext.</span><span class="sxs-lookup"><span data-stu-id="de4e2-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="de4e2-190">Zu einfachen Typen gehören alle von der [.NET Framework-primitive Typen](https://msdn.microsoft.com/library/system.type.isprimitive), plus **"DateTime"**, **Decimal**, **Guid**, **Zeichenfolge** , und **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="de4e2-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="de4e2-191">Für jede Aktion kann höchstens einen Parameter Anforderungstext lesen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="de4e2-192">Es ist möglich, die Standardregeln für die Bindung zu überschreiben.</span><span class="sxs-lookup"><span data-stu-id="de4e2-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="de4e2-193">Finden Sie unter [WebAPI parameterbindung hinter den Kulissen](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="de4e2-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>


<span data-ttu-id="de4e2-194">Hier ist der Aktion Auswahl-Algorithmus, mit diesen Hintergrund.</span><span class="sxs-lookup"><span data-stu-id="de4e2-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="de4e2-195">Erstellen Sie eine Liste aller Aktionen auf dem Controller an, die die HTTP-Anforderungsmethode entsprechen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="de4e2-196">Wenn des Routenwörterbuchs ein "Aktion"-Eintrag vorhanden ist, entfernen Sie die Aktionen, die, deren Name nicht mit diesem Wert übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="de4e2-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="de4e2-197">Versuchen Sie, entsprechend der Action-Parameter an den URI wie folgt:</span><span class="sxs-lookup"><span data-stu-id="de4e2-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="de4e2-198">Erhalten Sie für jede Aktion eine Liste der Parameter, die einen einfachen Typ sind, in dem die Bindung ruft des Parameters aus dem URI.</span><span class="sxs-lookup"><span data-stu-id="de4e2-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="de4e2-199">Schließen Sie die optionale Parameter.</span><span class="sxs-lookup"><span data-stu-id="de4e2-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="de4e2-200">Versuchen Sie, eine Übereinstimmung mit jeder Parametername in des Routenwörterbuchs oder in der URI-Abfragezeichenfolge zu suchen, aus dieser Liste.</span><span class="sxs-lookup"><span data-stu-id="de4e2-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="de4e2-201">Übereinstimmungen werden Groß-/Kleinschreibung und hängen die Reihenfolge der Parameter nicht.</span><span class="sxs-lookup"><span data-stu-id="de4e2-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="de4e2-202">Wählen Sie eine Aktion aus, in dem alle Parameter in der Liste eine Übereinstimmung im URI verfügt.</span><span class="sxs-lookup"><span data-stu-id="de4e2-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="de4e2-203">Wenn mehr, einer Aktion diese Kriterien erfüllt, wählen Sie das Laufwerk mit der meisten Parameter entspricht.</span><span class="sxs-lookup"><span data-stu-id="de4e2-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="de4e2-204">Ignorieren von Aktionen mit den **[NonAction]** Attribut.</span><span class="sxs-lookup"><span data-stu-id="de4e2-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="de4e2-205">Schritt 3 # ist der am häufigsten verwirrend.</span><span class="sxs-lookup"><span data-stu-id="de4e2-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="de4e2-206">Die Grundidee ist, dass ein Parameter den Wert entweder aus dem URI, aus dem Anforderungstext oder aus einer benutzerdefinierten Bindung abrufen kann.</span><span class="sxs-lookup"><span data-stu-id="de4e2-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="de4e2-207">Für Parameter, die aus dem URI stammen, möchten wir stellen Sie sicher, dass der URI tatsächlich einen Wert für diesen Parameter, die in den Pfad (über des Routenwörterbuchs) oder in der Abfragezeichenfolge enthält.</span><span class="sxs-lookup"><span data-stu-id="de4e2-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="de4e2-208">Betrachten Sie beispielsweise die folgende Aktion aus:</span><span class="sxs-lookup"><span data-stu-id="de4e2-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="de4e2-209">Die *Id* Parameter bindet an den URI.</span><span class="sxs-lookup"><span data-stu-id="de4e2-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="de4e2-210">Diese Aktion kann daher nur einen URI übereinstimmen, die einen Wert für "Id", in der Routenwörterbuchs oder in der Abfragezeichenfolge enthalten.</span><span class="sxs-lookup"><span data-stu-id="de4e2-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="de4e2-211">Optionale Parameter sind eine Ausnahme aus, da sie optional sind.</span><span class="sxs-lookup"><span data-stu-id="de4e2-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="de4e2-212">Ist es für einen optionalen Parameter OK aus, wenn die Bindung den Wert aus dem URI abrufen kann.</span><span class="sxs-lookup"><span data-stu-id="de4e2-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="de4e2-213">Komplexe Typen sind eine Ausnahme für einen anderen Grund.</span><span class="sxs-lookup"><span data-stu-id="de4e2-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="de4e2-214">Ein komplexer Typ kann nur durch eine benutzerdefinierte Bindung an den URI binden.</span><span class="sxs-lookup"><span data-stu-id="de4e2-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="de4e2-215">Aber in diesem Fall das Framework nicht wissen im voraus, ob der Parameter an einen bestimmten URI gebunden werden würde.</span><span class="sxs-lookup"><span data-stu-id="de4e2-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="de4e2-216">Um herauszufinden, müssten sie die Bindung aufrufen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="de4e2-217">Das Ziel des Algorithmus Auswahl ist, wählen Sie eine Aktion aus der statischen Beschreibung vor dem Aufrufen von Bindungen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="de4e2-218">Aus diesem Grund werden komplexe Typen aus dem übereinstimmenden Algorithmus ausgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="de4e2-219">Nachdem die Aktion aktiviert ist, werden alle parameterbindungen aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="de4e2-220">Zusammenfassung:</span><span class="sxs-lookup"><span data-stu-id="de4e2-220">Summary:</span></span>

- <span data-ttu-id="de4e2-221">Die Aktion muss die HTTP-Methode der Anforderung überein.</span><span class="sxs-lookup"><span data-stu-id="de4e2-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="de4e2-222">Der Aktionsname muss den Eintrag "Aktion" in der Routenwörterbuchs angefordertes übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="de4e2-223">Für jeden Parameter der Aktion stammt aus dem URI, der Parameter muss dann der Name des Parameters des Routenwörterbuchs oder im URI-Abfragezeichenfolge gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="de4e2-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="de4e2-224">(Optionale Parameter und Parameter mit komplexen Typen sind ausgenommen.)</span><span class="sxs-lookup"><span data-stu-id="de4e2-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="de4e2-225">Versuchen Sie es mit der höchsten Anzahl von Parametern übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="de4e2-226">Die beste Übereinstimmung möglicherweise eine Methode ohne Parameter.</span><span class="sxs-lookup"><span data-stu-id="de4e2-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="de4e2-227">Erweitertes Beispiel</span><span class="sxs-lookup"><span data-stu-id="de4e2-227">Extended Example</span></span>

<span data-ttu-id="de4e2-228">Routen:</span><span class="sxs-lookup"><span data-stu-id="de4e2-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="de4e2-229">Controller:</span><span class="sxs-lookup"><span data-stu-id="de4e2-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="de4e2-230">HTTP-Anforderung:</span><span class="sxs-lookup"><span data-stu-id="de4e2-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="de4e2-231">Übereinstimmende Route</span><span class="sxs-lookup"><span data-stu-id="de4e2-231">Route Matching</span></span>

<span data-ttu-id="de4e2-232">Der URI mit der Route, die mit dem Namen "DefaultApi" übereinstimmt.</span><span class="sxs-lookup"><span data-stu-id="de4e2-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="de4e2-233">Die Route-Wörterbuch enthält die folgenden Einträge:</span><span class="sxs-lookup"><span data-stu-id="de4e2-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="de4e2-234">Controller: "Products"</span><span class="sxs-lookup"><span data-stu-id="de4e2-234">controller: "products"</span></span>
- <span data-ttu-id="de4e2-235">id: "1"</span><span class="sxs-lookup"><span data-stu-id="de4e2-235">id: "1"</span></span>

<span data-ttu-id="de4e2-236">Des Routenwörterbuchs enthält nicht den Abfragezeichenfolgen-Parametern, "Version" und "Details", aber diese werden weiterhin während Aktionsauswahl berücksichtigt werden.</span><span class="sxs-lookup"><span data-stu-id="de4e2-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="de4e2-237">Auswahl der Domänencontroller</span><span class="sxs-lookup"><span data-stu-id="de4e2-237">Controller Selection</span></span>

<span data-ttu-id="de4e2-238">Von der "Controller" Eintrag im Wörterbuch Route ist der Typ des Controllers ist `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="de4e2-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="de4e2-239">Aktionsauswahl</span><span class="sxs-lookup"><span data-stu-id="de4e2-239">Action Selection</span></span>

<span data-ttu-id="de4e2-240">Die HTTP-Anforderung ist eine GET-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="de4e2-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="de4e2-241">Die Controlleraktionen, die GET unterstützen sind `GetAll`, `GetById`, und `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="de4e2-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="de4e2-242">Des Routenwörterbuchs enthält keinen Eintrag für "Aktion", daher wir brauchen mit dem Aktionsnamen übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="de4e2-243">Als Nächstes verwenden wir Parameternamen für die Aktionen entsprechend betrachten die GET-Aktionen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="de4e2-244">Aktion</span><span class="sxs-lookup"><span data-stu-id="de4e2-244">Action</span></span> | <span data-ttu-id="de4e2-245">Parameter zur Übereinstimmung</span><span class="sxs-lookup"><span data-stu-id="de4e2-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="de4e2-246">Keine</span><span class="sxs-lookup"><span data-stu-id="de4e2-246">none</span></span> |
| `GetById` | <span data-ttu-id="de4e2-247">"id"</span><span class="sxs-lookup"><span data-stu-id="de4e2-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="de4e2-248">"Name"</span><span class="sxs-lookup"><span data-stu-id="de4e2-248">"name"</span></span> |

<span data-ttu-id="de4e2-249">Beachten Sie, dass die *Version* Parameter `GetById` nicht berücksichtigt, da es sich um einen optionalen Parameter handelt.</span><span class="sxs-lookup"><span data-stu-id="de4e2-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="de4e2-250">Die `GetAll` Methode entspricht im Grunde.</span><span class="sxs-lookup"><span data-stu-id="de4e2-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="de4e2-251">Die `GetById` Methode auch entspricht, da die Routenwörterbuchs "Id" enthält.</span><span class="sxs-lookup"><span data-stu-id="de4e2-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="de4e2-252">Die `FindProductsByName` Methode stimmt nicht überein.</span><span class="sxs-lookup"><span data-stu-id="de4e2-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="de4e2-253">Die `GetById` wins-Methode, da es sich um einen Parameter, und keine Parameter für entspricht `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="de4e2-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="de4e2-254">Die Methode wird mit den folgenden Parameterwerte aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="de4e2-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="de4e2-255">*id* = 1</span><span class="sxs-lookup"><span data-stu-id="de4e2-255">*id* = 1</span></span>
- <span data-ttu-id="de4e2-256">*Version* = 1.5</span><span class="sxs-lookup"><span data-stu-id="de4e2-256">*version* = 1.5</span></span>

<span data-ttu-id="de4e2-257">Beachten Sie, dass, obwohl *Version* nicht verwendet wurde, in der Auswahl-Algorithmus stammt der Wert des Parameters aus der URI-Abfragezeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="de4e2-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="de4e2-258">Erweiterungspunkte</span><span class="sxs-lookup"><span data-stu-id="de4e2-258">Extension Points</span></span>

<span data-ttu-id="de4e2-259">Web-API stellt Erweiterungspunkte bereit, für einige Teile der routing-Prozess.</span><span class="sxs-lookup"><span data-stu-id="de4e2-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="de4e2-260">Interface</span><span class="sxs-lookup"><span data-stu-id="de4e2-260">Interface</span></span> | <span data-ttu-id="de4e2-261">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="de4e2-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="de4e2-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="de4e2-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="de4e2-263">Wählt den Controller.</span><span class="sxs-lookup"><span data-stu-id="de4e2-263">Selects the controller.</span></span> |
| <span data-ttu-id="de4e2-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="de4e2-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="de4e2-265">Ruft die Liste der Controllertypen ab.</span><span class="sxs-lookup"><span data-stu-id="de4e2-265">Gets the list of controller types.</span></span> <span data-ttu-id="de4e2-266">Die **DefaultHttpControllerSelector** wählt den Controllertyp aus dieser Liste.</span><span class="sxs-lookup"><span data-stu-id="de4e2-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="de4e2-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="de4e2-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="de4e2-268">Ruft die Liste der Projektassemblys ab.</span><span class="sxs-lookup"><span data-stu-id="de4e2-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="de4e2-269">Die **IHttpControllerTypeResolver** Schnittstelle verwendet diese Liste, um die Controllertypen gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="de4e2-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="de4e2-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="de4e2-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="de4e2-271">Erstellt neue Domänencontroller-Instanzen.</span><span class="sxs-lookup"><span data-stu-id="de4e2-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="de4e2-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="de4e2-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="de4e2-273">Wählt die Aktion an.</span><span class="sxs-lookup"><span data-stu-id="de4e2-273">Selects the action.</span></span> |
| <span data-ttu-id="de4e2-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="de4e2-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="de4e2-275">Ruft die Aktion an.</span><span class="sxs-lookup"><span data-stu-id="de4e2-275">Invokes the action.</span></span> |

<span data-ttu-id="de4e2-276">Verwenden, um eine eigene Implementierung für den folgenden Schnittstellen ermöglichen die **Services** Sammlung auf die **HttpConfiguration** Objekt:</span><span class="sxs-lookup"><span data-stu-id="de4e2-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
