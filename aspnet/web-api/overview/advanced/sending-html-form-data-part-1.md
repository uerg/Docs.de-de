---
uid: web-api/overview/advanced/sending-html-form-data-part-1
title: 'Senden von HTML-Formulardaten in ASP.NET Web-API: Form-Urlencoded-Daten | Microsoft-Dokumentation'
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/15/2012
ms.assetid: 585351c4-809a-4bf5-bcbe-35d624f565fe
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-1
msc.type: authoredcontent
ms.openlocfilehash: 617b16da20f448bf86e4b99841ad6eeaf8aafe4d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818066"
---
<a name="sending-html-form-data-in-aspnet-web-api-form-urlencoded-data"></a><span data-ttu-id="d50b3-102">Senden von HTML-Formulardaten in ASP.NET Web-API: Form-Urlencoded-Daten</span><span class="sxs-lookup"><span data-stu-id="d50b3-102">Sending HTML Form Data in ASP.NET Web API: Form-urlencoded Data</span></span>
====================
<span data-ttu-id="d50b3-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="d50b3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-1-form-urlencoded-data"></a><span data-ttu-id="d50b3-104">Teil 1: Form-Urlencoded-Daten</span><span class="sxs-lookup"><span data-stu-id="d50b3-104">Part 1: Form-urlencoded Data</span></span>

<span data-ttu-id="d50b3-105">In diesem Artikel veranschaulicht das Form-Urlencoded-Daten an einen Web-API-Controller zu veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="d50b3-105">This article shows how to post form-urlencoded data to a Web API controller.</span></span>

- [<span data-ttu-id="d50b3-106">Übersicht über die HTML-Formularen</span><span class="sxs-lookup"><span data-stu-id="d50b3-106">Overview of HTML Forms</span></span>](#overview_of_html_forms)
- [<span data-ttu-id="d50b3-107">Senden von komplexen Typen</span><span class="sxs-lookup"><span data-stu-id="d50b3-107">Sending Complex Types</span></span>](#sending_complex_types)
- [<span data-ttu-id="d50b3-108">Senden von Formulardaten per AJAX</span><span class="sxs-lookup"><span data-stu-id="d50b3-108">Sending Form Data via AJAX</span></span>](#sending_form_data_via_ajax)
- [<span data-ttu-id="d50b3-109">Senden von einfachen Typen</span><span class="sxs-lookup"><span data-stu-id="d50b3-109">Sending Simple Types</span></span>](#sending_simple_types)

> [!NOTE]
> <span data-ttu-id="d50b3-110">[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span><span class="sxs-lookup"><span data-stu-id="d50b3-110">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-Sending-a6f9d007).</span></span>


<a id="overview_of_html_forms"></a>
## <a name="overview-of-html-forms"></a><span data-ttu-id="d50b3-111">Übersicht über die HTML-Formularen</span><span class="sxs-lookup"><span data-stu-id="d50b3-111">Overview of HTML Forms</span></span>

<span data-ttu-id="d50b3-112">HTML-Formulare verwenden wird entweder GET oder POST zum Senden von Daten an den Server.</span><span class="sxs-lookup"><span data-stu-id="d50b3-112">HTML forms use either GET or POST to send data to the server.</span></span> <span data-ttu-id="d50b3-113">Die **Methode** Attribut der **Formular** -Element können die HTTP-Methode:</span><span class="sxs-lookup"><span data-stu-id="d50b3-113">The **method** attribute of the **form** element gives the HTTP method:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample1.html)]

<span data-ttu-id="d50b3-114">Die Standardmethode ist GET.</span><span class="sxs-lookup"><span data-stu-id="d50b3-114">The default method is GET.</span></span> <span data-ttu-id="d50b3-115">Wenn das Formular verwendet zu erhalten, das Formular, das Daten in den URI als Zeichenfolge codiert ist.</span><span class="sxs-lookup"><span data-stu-id="d50b3-115">If the form uses GET, the form data is encoded in the URI as a query string.</span></span> <span data-ttu-id="d50b3-116">Wenn das Formular POST verwendet wird, werden die Formulardaten im Anforderungstext platziert.</span><span class="sxs-lookup"><span data-stu-id="d50b3-116">If the form uses POST, the form data is placed in the request body.</span></span> <span data-ttu-id="d50b3-117">Für Gebucht-Daten die **Enctype** Attribut gibt an, das Format des Anforderungstexts:</span><span class="sxs-lookup"><span data-stu-id="d50b3-117">For POSTed data, the **enctype** attribute specifies the format of the request body:</span></span>

| <span data-ttu-id="d50b3-118">Enctype</span><span class="sxs-lookup"><span data-stu-id="d50b3-118">enctype</span></span> | <span data-ttu-id="d50b3-119">Beschreibung</span><span class="sxs-lookup"><span data-stu-id="d50b3-119">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d50b3-120">application/x-www-form-urlencoded</span><span class="sxs-lookup"><span data-stu-id="d50b3-120">application/x-www-form-urlencoded</span></span> | <span data-ttu-id="d50b3-121">Als Name/Wert-Paare, ähnlich wie eine URI-Abfragezeichenfolge ist die Formulardaten codiert.</span><span class="sxs-lookup"><span data-stu-id="d50b3-121">Form data is encoded as name/value pairs, similar to a URI query string.</span></span> <span data-ttu-id="d50b3-122">Dies ist das Standardformat für POST-Methode.</span><span class="sxs-lookup"><span data-stu-id="d50b3-122">This is the default format for POST.</span></span> |
| <span data-ttu-id="d50b3-123">Multipart/Form-data</span><span class="sxs-lookup"><span data-stu-id="d50b3-123">multipart/form-data</span></span> | <span data-ttu-id="d50b3-124">Formulardaten werden als eine mehrteilige MIME-Nachricht codiert.</span><span class="sxs-lookup"><span data-stu-id="d50b3-124">Form data is encoded as a multipart MIME message.</span></span> <span data-ttu-id="d50b3-125">Verwenden Sie dieses Format, wenn Sie eine Datei mit dem Server hochladen.</span><span class="sxs-lookup"><span data-stu-id="d50b3-125">Use this format if you are uploading a file to the server.</span></span> |

<span data-ttu-id="d50b3-126">Teil 1 dieses Artikels untersucht die X-www-form-urlencoded-Format.</span><span class="sxs-lookup"><span data-stu-id="d50b3-126">Part 1 of this article looks at x-www-form-urlencoded format.</span></span> <span data-ttu-id="d50b3-127">[Teil 2](sending-html-form-data-part-2.md) mehrteiligen MIME-Nachrichten beschrieben.</span><span class="sxs-lookup"><span data-stu-id="d50b3-127">[Part 2](sending-html-form-data-part-2.md) describes multipart MIME.</span></span>

<a id="sending_complex_types"></a>
## <a name="sending-complex-types"></a><span data-ttu-id="d50b3-128">Senden von komplexen Typen</span><span class="sxs-lookup"><span data-stu-id="d50b3-128">Sending Complex Types</span></span>

<span data-ttu-id="d50b3-129">Senden Sie in der Regel einen komplexen Typ, bestehend aus Werten aus mehrere Steuerelemente des Formulars.</span><span class="sxs-lookup"><span data-stu-id="d50b3-129">Typically, you will send a complex type, composed of values taken from several form controls.</span></span> <span data-ttu-id="d50b3-130">Betrachten Sie das folgende Modell, das eine statusaktualisierung darstellt:</span><span class="sxs-lookup"><span data-stu-id="d50b3-130">Consider the following model that represents a status update:</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample2.cs)]

<span data-ttu-id="d50b3-131">Hier ist ein Web-API-Controller, die akzeptiert eine `Update` Objekt über POST.</span><span class="sxs-lookup"><span data-stu-id="d50b3-131">Here is a Web API controller that accepts an `Update` object via POST.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample3.cs)]

> [!NOTE]
> <span data-ttu-id="d50b3-132">Dieser Controller verwendet [Aktion-basiertes routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), sodass die routenvorlage &quot;api / {Controller} / {Action} / {Id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="d50b3-132">This controller uses [action-based routing](../web-api-routing-and-actions/routing-in-aspnet-web-api.md#routing_by_action_name), so the route template is &quot;api/{controller}/{action}/{id}&quot;.</span></span> <span data-ttu-id="d50b3-133">Der Client wird die Daten zu buchen &quot;/api/updates/complex&quot;.</span><span class="sxs-lookup"><span data-stu-id="d50b3-133">The client will post the data to &quot;/api/updates/complex&quot;.</span></span>


<span data-ttu-id="d50b3-134">Jetzt schreiben wir nun ein HTML-Formular für Benutzer, eine statusaktualisierung zu übermitteln.</span><span class="sxs-lookup"><span data-stu-id="d50b3-134">Now let's write an HTML form for users to submit a status update.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample4.html)]

<span data-ttu-id="d50b3-135">Beachten Sie, dass die **Aktion** Attribut des Formulars ist der URI des unsere Controlleraktion.</span><span class="sxs-lookup"><span data-stu-id="d50b3-135">Notice that the **action** attribute on the form is the URI of our controller action.</span></span> <span data-ttu-id="d50b3-136">So sieht das Formular mit einigen in eingegebenen Werten aus:</span><span class="sxs-lookup"><span data-stu-id="d50b3-136">Here is the form with some values entered in:</span></span>

![](sending-html-form-data-part-1/_static/image1.png)

<span data-ttu-id="d50b3-137">Klickt der Benutzer senden, sendet der Browser eine HTTP-Anforderung ähnelt dem folgenden:</span><span class="sxs-lookup"><span data-stu-id="d50b3-137">When the user clicks Submit, the browser sends an HTTP request similar to the following:</span></span>

[!code-console[Main](sending-html-form-data-part-1/samples/sample5.cmd)]

<span data-ttu-id="d50b3-138">Beachten Sie, dass der Anforderungstext Daten aus dem Formular, formatiert als Name/Wert-Paare enthält.</span><span class="sxs-lookup"><span data-stu-id="d50b3-138">Notice that the request body contains the form data, formatted as name/value pairs.</span></span> <span data-ttu-id="d50b3-139">Web-API konvertiert automatisch Name/Wert-Paare in einer Instanz von der `Update` Klasse.</span><span class="sxs-lookup"><span data-stu-id="d50b3-139">Web API automatically converts the name/value pairs into an instance of the `Update` class.</span></span>

<a id="sending_form_data_via_ajax"></a>
## <a name="sending-form-data-via-ajax"></a><span data-ttu-id="d50b3-140">Senden von Formulardaten per AJAX</span><span class="sxs-lookup"><span data-stu-id="d50b3-140">Sending Form Data via AJAX</span></span>

<span data-ttu-id="d50b3-141">Wenn ein Benutzer ein Formular übermittelt, wird der Browser von der aktuellen Seite weg navigiert und rendert den Text der Antwortnachricht.</span><span class="sxs-lookup"><span data-stu-id="d50b3-141">When a user submits a form, the browser navigates away from the current page and renders the body of the response message.</span></span> <span data-ttu-id="d50b3-142">Das ist in Ordnung Wenn die Antwort auf eine HTML-Seite ist.</span><span class="sxs-lookup"><span data-stu-id="d50b3-142">That's OK when the response is an HTML page.</span></span> <span data-ttu-id="d50b3-143">Mit einer Web-API, der Antworttext ist jedoch in der Regel entweder leer ist oder strukturierte Daten, z.B. JSON enthält.</span><span class="sxs-lookup"><span data-stu-id="d50b3-143">With a web API, however, the response body is usually either empty or contains structured data, such as JSON.</span></span> <span data-ttu-id="d50b3-144">In diesem Fall macht es mehr Sinn, die zum Senden von Daten aus dem Formular mit einer AJAX-Anforderung, damit an, dass die Antwort von die Seite verarbeitet werden kann.</span><span class="sxs-lookup"><span data-stu-id="d50b3-144">In that case, it makes more sense to send the form data using an AJAX request, so that the page can process the response.</span></span>

<span data-ttu-id="d50b3-145">Der folgende Code zeigt, wie unter Verwendung von jQuery Formulardaten übermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="d50b3-145">The following code shows how to post form data using jQuery.</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample6.html)]

<span data-ttu-id="d50b3-146">Die jQuery **übermitteln** -Funktion ersetzt die Formularaktion mit einer neuen Funktion.</span><span class="sxs-lookup"><span data-stu-id="d50b3-146">The jQuery **submit** function replaces the form action with a new function.</span></span> <span data-ttu-id="d50b3-147">Dies überschreibt das Standardverhalten der Senden-Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="d50b3-147">This overrides the default behavior of the Submit button.</span></span> <span data-ttu-id="d50b3-148">Die **Serialisieren** Funktion serialisiert Daten aus dem Formular in Name/Wert-Paaren.</span><span class="sxs-lookup"><span data-stu-id="d50b3-148">The **serialize** function serializes the form data into name/value pairs.</span></span> <span data-ttu-id="d50b3-149">Rufen Sie zum Senden von Daten aus dem Formular an den Server `$.post()`.</span><span class="sxs-lookup"><span data-stu-id="d50b3-149">To send the form data to the server, call `$.post()`.</span></span>

<span data-ttu-id="d50b3-150">Wenn die Anforderung abgeschlossen ist, die `.success()` oder `.error()` Handler dem Benutzer eine entsprechende Meldung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d50b3-150">When the request completes, the `.success()` or `.error()` handler displays an appropriate message to the user.</span></span>

![](sending-html-form-data-part-1/_static/image2.png)

<a id="sending_simple_types"></a>
## <a name="sending-simple-types"></a><span data-ttu-id="d50b3-151">Senden von einfachen Typen</span><span class="sxs-lookup"><span data-stu-id="d50b3-151">Sending Simple Types</span></span>

<span data-ttu-id="d50b3-152">In den vorherigen Abschnitten haben wir einen komplexen Typ, der Web-API mit einer Instanz von einer Modellklasse deserialisiert gesendet.</span><span class="sxs-lookup"><span data-stu-id="d50b3-152">In the previous sections, we sent a complex type, which Web API deserialized to an instance of a model class.</span></span> <span data-ttu-id="d50b3-153">Sie können auch einfache Typen, z. B. eine Zeichenfolge senden.</span><span class="sxs-lookup"><span data-stu-id="d50b3-153">You can also send simple types, such as a string.</span></span>

> [!NOTE]
> <span data-ttu-id="d50b3-154">Senden einen einfachen Typ, berücksichtigen Sie stattdessen den Wert in einem komplexen Typ umschließt.</span><span class="sxs-lookup"><span data-stu-id="d50b3-154">Before sending a simple type, consider wrapping the value in a complex type instead.</span></span> <span data-ttu-id="d50b3-155">Dies bietet Ihnen die Vorteile der modellvalidierung auf der Serverseite und erleichtert es, Ihr Modell zu erweitern, wenn erforderlich.</span><span class="sxs-lookup"><span data-stu-id="d50b3-155">This gives you the benefits of model validation on the server side, and makes it easier to extend your model if needed.</span></span>


<span data-ttu-id="d50b3-156">Die grundlegenden Schritte zum Senden von eines einfachen Typs sind identisch, aber es gibt zwei feine Unterschiede.</span><span class="sxs-lookup"><span data-stu-id="d50b3-156">The basic steps to send a simple type are the same, but there are two subtle differences.</span></span> <span data-ttu-id="d50b3-157">Zunächst im Controller muss, ergänzen Sie den Namen des Parameters mit dem **FromBody** Attribut.</span><span class="sxs-lookup"><span data-stu-id="d50b3-157">First, in the controller, you must decorate the parameter name with the **FromBody** attribute.</span></span>

[!code-csharp[Main](sending-html-form-data-part-1/samples/sample7.cs?highlight=3)]

<span data-ttu-id="d50b3-158">Web-API versucht standardmäßig einfache Typen aus der Anforderungs-URI abrufen.</span><span class="sxs-lookup"><span data-stu-id="d50b3-158">By default, Web API tries to get simple types from the request URI.</span></span> <span data-ttu-id="d50b3-159">Die **FromBody** -Attribut weist die Web-API zum Lesen des Werts aus dem Anforderungstext.</span><span class="sxs-lookup"><span data-stu-id="d50b3-159">The **FromBody** attribute tells Web API to read the value from the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="d50b3-160">Web-API liest den Antworttext nur einen Parameter einer Aktion aus dem Anforderungstext stammen kann wird höchstens einmal zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="d50b3-160">Web API reads the response body at most once, so only one parameter of an action can come from the request body.</span></span> <span data-ttu-id="d50b3-161">Wenn Sie mehrere Werte aus dem Anforderungstext abrufen müssen, definieren Sie einen komplexen Typ.</span><span class="sxs-lookup"><span data-stu-id="d50b3-161">If you need to get multiple values from the request body, define a complex type.</span></span>


<span data-ttu-id="d50b3-162">Zweitens muss der Client zum Senden von des Wert mit dem folgenden Format ein:</span><span class="sxs-lookup"><span data-stu-id="d50b3-162">Second, the client needs to send the value with the following format:</span></span>

[!code-xml[Main](sending-html-form-data-part-1/samples/sample8.xml)]

<span data-ttu-id="d50b3-163">Insbesondere muss der Namensteil von Name/Wert-Paar für einen einfachen Typ leer sein.</span><span class="sxs-lookup"><span data-stu-id="d50b3-163">Specifically, the name portion of the name/value pair must be empty for a simple type.</span></span> <span data-ttu-id="d50b3-164">Nicht alle Browser unterstützen diese für die HTML-Formularen, aber Sie erstellen dieses Format im Skript wie folgt:</span><span class="sxs-lookup"><span data-stu-id="d50b3-164">Not all browsers support this for HTML forms, but you create this format in script as follows:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample9.js)]

<span data-ttu-id="d50b3-165">Hier ist ein Beispielformular:</span><span class="sxs-lookup"><span data-stu-id="d50b3-165">Here is an example form:</span></span>

[!code-html[Main](sending-html-form-data-part-1/samples/sample10.html)]

<span data-ttu-id="d50b3-166">Und hier ist das Skript aus, um den Formularwert zu übermitteln.</span><span class="sxs-lookup"><span data-stu-id="d50b3-166">And here is the script to submit the form value.</span></span> <span data-ttu-id="d50b3-167">Der einzige Unterschied besteht aus dem vorherigen Skript wird das übergebene Argument der **Posten** Funktion.</span><span class="sxs-lookup"><span data-stu-id="d50b3-167">The only difference from the previous script is the argument passed into the **post** function.</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample11.js?highlight=2)]

<span data-ttu-id="d50b3-168">Sie können den gleichen Ansatz verwenden, um ein Array von einfachen Typen zu senden:</span><span class="sxs-lookup"><span data-stu-id="d50b3-168">You can use the same approach to send an array of simple types:</span></span>

[!code-javascript[Main](sending-html-form-data-part-1/samples/sample12.js)]

## <a name="additional-resources"></a><span data-ttu-id="d50b3-169">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="d50b3-169">Additional Resources</span></span>

[<span data-ttu-id="d50b3-170">Teil 2: Dateiupload und mehrteiligen MIME-</span><span class="sxs-lookup"><span data-stu-id="d50b3-170">Part 2: File Upload and Multipart MIME</span></span>](sending-html-form-data-part-2.md)
