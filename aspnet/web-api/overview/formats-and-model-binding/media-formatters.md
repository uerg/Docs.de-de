---
uid: web-api/overview/formats-and-model-binding/media-formatters
title: Medienformatierer in der ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 4c56f64a-086a-44ce-99c2-4c69604cd7fd
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/formats-and-model-binding/media-formatters
msc.type: authoredcontent
ms.openlocfilehash: 1cb1c7e0f832a0a0160276fbd41facc017e2ae3e
ms.sourcegitcommit: 50d40c83fa641d283c097f986dde5341ebe1b44c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2018
ms.locfileid: "34452599"
---
<a name="media-formatters-in-aspnet-web-api-2"></a><span data-ttu-id="773b3-102">Medienformatierer in der ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="773b3-102">Media Formatters in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="773b3-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="773b3-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="773b3-104">In diesem Lernprogramm wird gezeigt, wie zusätzliche Medienformate in ASP.NET Web-API unterstützt.</span><span class="sxs-lookup"><span data-stu-id="773b3-104">This tutorial shows how to support additional media formats in ASP.NET Web API.</span></span>

## <a name="internet-media-types"></a><span data-ttu-id="773b3-105">Internet-Medientypen</span><span class="sxs-lookup"><span data-stu-id="773b3-105">Internet Media Types</span></span>

<span data-ttu-id="773b3-106">Ein Medientyp, der eine MIME-Typ, gibt das Format der Daten.</span><span class="sxs-lookup"><span data-stu-id="773b3-106">A media type, also called a MIME type, identifies the format of a piece of data.</span></span> <span data-ttu-id="773b3-107">Bei HTTP beschreiben Medientypen das Format des Nachrichtentexts.</span><span class="sxs-lookup"><span data-stu-id="773b3-107">In HTTP, media types describe the format of the message body.</span></span> <span data-ttu-id="773b3-108">Ein Medientyp besteht aus zwei Zeichenfolgen, die einen Typ und Untertyp.</span><span class="sxs-lookup"><span data-stu-id="773b3-108">A media type consists of two strings, a type and a subtype.</span></span> <span data-ttu-id="773b3-109">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="773b3-109">For example:</span></span>

- <span data-ttu-id="773b3-110">Text/html</span><span class="sxs-lookup"><span data-stu-id="773b3-110">text/html</span></span>
- <span data-ttu-id="773b3-111">Image/png</span><span class="sxs-lookup"><span data-stu-id="773b3-111">image/png</span></span>
- <span data-ttu-id="773b3-112">application/json</span><span class="sxs-lookup"><span data-stu-id="773b3-112">application/json</span></span>

<span data-ttu-id="773b3-113">Wenn eine HTTP-Nachricht über einen Entitätskörper enthält, gibt der Content-Type-Header das Format des Nachrichtentexts.</span><span class="sxs-lookup"><span data-stu-id="773b3-113">When an HTTP message contains an entity-body, the Content-Type header specifies the format of the message body.</span></span> <span data-ttu-id="773b3-114">Dieser Wert ermöglicht dem Empfänger, Analysieren des Inhalts des Nachrichtentexts.</span><span class="sxs-lookup"><span data-stu-id="773b3-114">This tells the receiver how to parse the contents of the message body.</span></span>

<span data-ttu-id="773b3-115">Z. B. eine HTTP-Antwort ein PNG-Bild enthält, möglicherweise die Antwort die folgenden Header enthalten.</span><span class="sxs-lookup"><span data-stu-id="773b3-115">For example, if an HTTP response contains a PNG image, the response might have the following headers.</span></span>

[!code-console[Main](media-formatters/samples/sample1.cmd)]

<span data-ttu-id="773b3-116">Wenn der Client eine Anforderungsnachricht sendet, kann er einen Accept-Header umfassen.</span><span class="sxs-lookup"><span data-stu-id="773b3-116">When the client sends a request message, it can include an Accept header.</span></span> <span data-ttu-id="773b3-117">Der Accept-Header informiert, dass es sich bei der Server, auf die Medien den Client Typs vom Server möchte.</span><span class="sxs-lookup"><span data-stu-id="773b3-117">The Accept header tells the server which media type(s) the client wants from the server.</span></span> <span data-ttu-id="773b3-118">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="773b3-118">For example:</span></span>

[!code-console[Main](media-formatters/samples/sample2.cmd)]

<span data-ttu-id="773b3-119">Dieser Header weist den Server, dass der Client entweder HTML, XHTML oder XML-wünscht.</span><span class="sxs-lookup"><span data-stu-id="773b3-119">This header tells the server that the client wants either HTML, XHTML, or XML.</span></span>

<span data-ttu-id="773b3-120">Der Medientyp bestimmt wie serialisiert und deserialisiert den Nachrichtentext für die HTTP-Web-API.</span><span class="sxs-lookup"><span data-stu-id="773b3-120">The media type determines how Web API serializes and deserializes the HTTP message body.</span></span> <span data-ttu-id="773b3-121">Web-API verfügt über integrierte Unterstützung für XML, JSON, BSON und Formular codierte Daten, und Sie können zusätzliche Medientypen unterstützen, indem Sie das Schreiben von einem *medienformatierer*.</span><span class="sxs-lookup"><span data-stu-id="773b3-121">Web API has built-in support for XML, JSON, BSON, and form-urlencoded data, and you can support additional media types by writing a *media formatter*.</span></span>

<span data-ttu-id="773b3-122">Um einen medienformatierer zu erstellen, werden Sie von einer dieser Klassen abgeleitet:</span><span class="sxs-lookup"><span data-stu-id="773b3-122">To create a media formatter, derive from one of these classes:</span></span>

- <span data-ttu-id="773b3-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="773b3-123">[MediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.mediatypeformatter.aspx).</span></span> <span data-ttu-id="773b3-124">In dieser Klasse verwendet den asynchronen Lesevorgang und Write-Methoden.</span><span class="sxs-lookup"><span data-stu-id="773b3-124">This class uses asynchronous read and write methods.</span></span>
- <span data-ttu-id="773b3-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span><span class="sxs-lookup"><span data-stu-id="773b3-125">[BufferedMediaTypeFormatter](https://msdn.microsoft.com/library/system.net.http.formatting.bufferedmediatypeformatter.aspx).</span></span> <span data-ttu-id="773b3-126">Diese Klasse leitet sich von **MediaTypeFormatter** jedoch Sychronous Lese-/Schreibzugriff Methoden verwendet.</span><span class="sxs-lookup"><span data-stu-id="773b3-126">This class derives from **MediaTypeFormatter** but uses sychronous read/write methods.</span></span>

<span data-ttu-id="773b3-127">Ableiten von **BufferedMediaTypeFormatter** ist einfacher, da kein asynchronen Code vorhanden ist, aber es bedeutet auch, dass der aufrufende Thread kann während der e/a blockiert.</span><span class="sxs-lookup"><span data-stu-id="773b3-127">Deriving from **BufferedMediaTypeFormatter** is simpler, because there is no asynchronous code, but it also means the calling thread can block during I/O.</span></span>

## <a name="example-creating-a-csv-media-formatter"></a><span data-ttu-id="773b3-128">Beispiel: Erstellen einer CSV-Medienformatierer</span><span class="sxs-lookup"><span data-stu-id="773b3-128">Example: Creating a CSV Media Formatter</span></span>

<span data-ttu-id="773b3-129">Das folgende Beispiel zeigt einen medientypformatierer, die ein Produktobjekt in ein Format mit kommagetrennten Werten (CSV) serialisieren kann.</span><span class="sxs-lookup"><span data-stu-id="773b3-129">The following example shows a media type formatter that can serialize a Product object to a comma-separated values (CSV) format.</span></span> <span data-ttu-id="773b3-130">Dieses Beispiel verwendet die Product-Typs definiert, die im Lernprogramm [erstellen eine Web-API, CRUD-Vorgänge unterstützt](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span><span class="sxs-lookup"><span data-stu-id="773b3-130">This example uses the Product type defined in the tutorial [Creating a Web API that Supports CRUD Operations](../older-versions/creating-a-web-api-that-supports-crud-operations.md).</span></span> <span data-ttu-id="773b3-131">Hier ist die Definition des Objekts Produkt:</span><span class="sxs-lookup"><span data-stu-id="773b3-131">Here is the definition of the Product object:</span></span>

[!code-csharp[Main](media-formatters/samples/sample3.cs)]

<span data-ttu-id="773b3-132">Um ein CSV-Formatierungsprogramm zu implementieren, definieren Sie eine Klasse, die abgeleitet **BufferedMediaTypeFormater**:</span><span class="sxs-lookup"><span data-stu-id="773b3-132">To implement a CSV formatter, define a class that derives from **BufferedMediaTypeFormater**:</span></span>

[!code-csharp[Main](media-formatters/samples/sample4.cs)]

<span data-ttu-id="773b3-133">Fügen Sie im Konstruktor die Medientypen, die den Formatierer unterstützt.</span><span class="sxs-lookup"><span data-stu-id="773b3-133">In the constructor, add the media types that the formatter supports.</span></span> <span data-ttu-id="773b3-134">In diesem Beispiel wird der Formatierer einen einziger Medientyp unterstützt &quot;Text/Csv&quot;:</span><span class="sxs-lookup"><span data-stu-id="773b3-134">In this example, the formatter supports a single media type, &quot;text/csv&quot;:</span></span>

[!code-csharp[Main](media-formatters/samples/sample5.cs)]

<span data-ttu-id="773b3-135">Überschreiben Sie die **CanWriteType** Methode, um anzugeben, welchen den Formatierer Typs serialisieren kann:</span><span class="sxs-lookup"><span data-stu-id="773b3-135">Override the **CanWriteType** method to indicate which types the formatter can serialize:</span></span>

[!code-csharp[Main](media-formatters/samples/sample6.cs)]

<span data-ttu-id="773b3-136">In diesem Beispiel kann das Formatierungsprogramm einzelne Serialisieren `Product` sowie Auflistungen von Objekten `Product` Objekte.</span><span class="sxs-lookup"><span data-stu-id="773b3-136">In this example, the formatter can serialize single `Product` objects as well as collections of `Product` objects.</span></span>

<span data-ttu-id="773b3-137">Auf ähnliche Weise, überschreiben die **CanReadType** Methode, um anzugeben, welchen den Formatierer Typs deserialisieren kann.</span><span class="sxs-lookup"><span data-stu-id="773b3-137">Similarly, override the **CanReadType** method to indicate which types the formatter can deserialize.</span></span> <span data-ttu-id="773b3-138">In diesem Beispiel wird das Formatierungsprogramm unterstützt keine Deserialisierung ab, damit die Methode gibt einfach auftragsantwortnachrichten zurück **"false"**.</span><span class="sxs-lookup"><span data-stu-id="773b3-138">In this example, the formatter does not support deserialization, so the method simply returns **false**.</span></span>

[!code-csharp[Main](media-formatters/samples/sample7.cs)]

<span data-ttu-id="773b3-139">Überschreiben Sie abschließend die **WriteToStream** Methode.</span><span class="sxs-lookup"><span data-stu-id="773b3-139">Finally, override the **WriteToStream** method.</span></span> <span data-ttu-id="773b3-140">Diese Methode serialisiert einen Typ durch Schreiben in einen Stream.</span><span class="sxs-lookup"><span data-stu-id="773b3-140">This method serializes a type by writing it to a stream.</span></span> <span data-ttu-id="773b3-141">Wenn der Formatierer Deserialisierung unterstützt, auch überschreiben, die **ReadFromStream** Methode.</span><span class="sxs-lookup"><span data-stu-id="773b3-141">If your formatter supports deserialization, also override the **ReadFromStream** method.</span></span>

[!code-csharp[Main](media-formatters/samples/sample8.cs)]

## <a name="adding-a-media-formatter-to-the-web-api-pipeline"></a><span data-ttu-id="773b3-142">Die Web-API-Pipeline hinzugefügt eine Medienformatierer</span><span class="sxs-lookup"><span data-stu-id="773b3-142">Adding a Media Formatter to the Web API Pipeline</span></span>

<span data-ttu-id="773b3-143">Verwenden Sie zum Hinzufügen einer medientypformatierer für die Web-API-Pipeline die **Formatierungsprogramme** Eigenschaft auf die **HttpConfiguration** Objekt.</span><span class="sxs-lookup"><span data-stu-id="773b3-143">To add a media type formatter to the Web API pipeline, use the **Formatters** property on the **HttpConfiguration** object.</span></span>

[!code-csharp[Main](media-formatters/samples/sample9.cs)]

## <a name="character-encodings"></a><span data-ttu-id="773b3-144">Zeichencodierungen</span><span class="sxs-lookup"><span data-stu-id="773b3-144">Character Encodings</span></span>

<span data-ttu-id="773b3-145">Optional kann ein medienformatierer mehrere zeichencodierungen, z. B. UTF-8 oder ISO 8859-1 unterstützen.</span><span class="sxs-lookup"><span data-stu-id="773b3-145">Optionally, a media formatter can support multiple character encodings, such as UTF-8 or ISO 8859-1.</span></span>

<span data-ttu-id="773b3-146">Fügen Sie im Konstruktor, eine oder mehrere [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) -Typen für die **SupportedEncodings** Auflistung.</span><span class="sxs-lookup"><span data-stu-id="773b3-146">In the constructor, add one or more [System.Text.Encoding](https://msdn.microsoft.com/library/system.text.encoding.aspx) types to the **SupportedEncodings** collection.</span></span> <span data-ttu-id="773b3-147">Fügen Sie zuerst die standardcodierung.</span><span class="sxs-lookup"><span data-stu-id="773b3-147">Put the default encoding first.</span></span>

[!code-csharp[Main](media-formatters/samples/sample10.cs?highlight=6-7)]

<span data-ttu-id="773b3-148">In der **WriteToStream** und **ReadFromStream** Aufrufen von Methoden, [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) , wählen Sie die bevorzugte zeichencodierung.</span><span class="sxs-lookup"><span data-stu-id="773b3-148">In the **WriteToStream** and **ReadFromStream** methods, call [MediaTypeFormatter.SelectCharacterEncoding](https://msdn.microsoft.com/library/hh969054.aspx) to select the preferred character encoding.</span></span> <span data-ttu-id="773b3-149">Diese Methode entspricht der Anforderungsheader für die Liste der unterstützten Codierungen.</span><span class="sxs-lookup"><span data-stu-id="773b3-149">This method matches the request headers against the list of supported encodings.</span></span> <span data-ttu-id="773b3-150">Verwenden Sie das zurückgegebene **Codierung** beim Lesen oder aus dem Stream schreiben:</span><span class="sxs-lookup"><span data-stu-id="773b3-150">Use the returned **Encoding** when you read or write from the stream:</span></span>

[!code-csharp[Main](media-formatters/samples/sample11.cs?highlight=3,5)]
