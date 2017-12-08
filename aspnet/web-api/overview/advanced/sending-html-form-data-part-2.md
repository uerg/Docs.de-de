---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'HTML-Formulardaten in ASP.NET Web-API senden: Datei hochladen und Multipart / MIME | Microsoft Docs'
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 3df59aab2a0c43f4a4f5c59530b0655f68d95cc7
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a><span data-ttu-id="c9b96-102">Senden von HTML-Formulardaten in ASP.NET Web-API: hochladen und Multipart / MIME-Datei</span><span class="sxs-lookup"><span data-stu-id="c9b96-102">Sending HTML Form Data in ASP.NET Web API: File Upload and Multipart MIME</span></span>
====================
<span data-ttu-id="c9b96-103">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c9b96-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

## <a name="part-2-file-upload-and-multipart-mime"></a><span data-ttu-id="c9b96-104">Teil 2: Datei hochladen und Multipart / MIME-</span><span class="sxs-lookup"><span data-stu-id="c9b96-104">Part 2: File Upload and Multipart MIME</span></span>

<span data-ttu-id="c9b96-105">Dieses Lernprogramm zeigt, wie zum Hochladen von Dateien in eine Web-API.</span><span class="sxs-lookup"><span data-stu-id="c9b96-105">This tutorial shows how to upload files to a web API.</span></span> <span data-ttu-id="c9b96-106">Es beschreibt auch, wie zum Verarbeiten von mehrteiliger MIME-Daten.</span><span class="sxs-lookup"><span data-stu-id="c9b96-106">It also describes how to process multipart MIME data.</span></span>

> [!NOTE]
> <span data-ttu-id="c9b96-107">[Herunterladen des abgeschlossenen Projekts](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span><span class="sxs-lookup"><span data-stu-id="c9b96-107">[Download the completed project](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).</span></span>


<span data-ttu-id="c9b96-108">Hier ist ein Beispiel für ein HTML-Formular für das Hochladen einer Datei ein:</span><span class="sxs-lookup"><span data-stu-id="c9b96-108">Here is an example of an HTML form for uploading a file:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

<span data-ttu-id="c9b96-109">Dieses Formular enthält, eines Texteingabe-Steuerelements und Eingabesteuerelement einer Datei.</span><span class="sxs-lookup"><span data-stu-id="c9b96-109">This form contains a text input control and a file input control.</span></span> <span data-ttu-id="c9b96-110">Wenn ein Formular Eingabesteuerelement einer Datei enthält die **Enctype** Attribut muss immer werden &quot;Multipart/Form-Data&quot;, der angibt, dass das Formular als einer mehrteiligen MIME-Nachricht gesendet werden.</span><span class="sxs-lookup"><span data-stu-id="c9b96-110">When a form contains a file input control, the **enctype** attribute should always be &quot;multipart/form-data&quot;, which specifies that the form will be sent as a multipart MIME message.</span></span>

<span data-ttu-id="c9b96-111">Das Format einer mehrteiligen MIME-Nachricht ist am einfachsten, zu verstehen, betrachten ein Beispiel für eine Anforderung:</span><span class="sxs-lookup"><span data-stu-id="c9b96-111">The format of a multipart MIME message is easiest to understand by looking at an example request:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

<span data-ttu-id="c9b96-112">Diese Meldung wird in zwei unterteilt *Teile*, eine für jede Form-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="c9b96-112">This message is divided into two *parts*, one for each form control.</span></span> <span data-ttu-id="c9b96-113">Teil-Grenzen werden durch die Zeilen angezeigt, die mithilfe von Gedankenstrichen beginnen.</span><span class="sxs-lookup"><span data-stu-id="c9b96-113">Part boundaries are indicated by the lines that start with dashes.</span></span>

> [!NOTE]
> <span data-ttu-id="c9b96-114">Die Grenze Teil umfasst eine zufällige-Komponente (&quot;41184676334&quot;), stellen Sie sicher, dass die Trennungszeichenfolge nicht versehentlich in einem Nachrichtenteil angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="c9b96-114">The part boundary includes a random component (&quot;41184676334&quot;) to ensure that the boundary string does not accidentally appear inside a message part.</span></span>


<span data-ttu-id="c9b96-115">Jeder Teil der Nachricht enthält eine oder mehrere Header, gefolgt von den Inhalts des Nachrichtenteils.</span><span class="sxs-lookup"><span data-stu-id="c9b96-115">Each message part contains one or more headers, followed by the part contents.</span></span>

- <span data-ttu-id="c9b96-116">Der Content-Disposition-Header enthält den Namen des Steuerelements.</span><span class="sxs-lookup"><span data-stu-id="c9b96-116">The Content-Disposition header includes the name of the control.</span></span> <span data-ttu-id="c9b96-117">Für Dateien enthält er auch den Dateinamen ein.</span><span class="sxs-lookup"><span data-stu-id="c9b96-117">For files, it also contains the file name.</span></span>
- <span data-ttu-id="c9b96-118">Der Content-Type-Header beschreibt die Daten in den Bereich.</span><span class="sxs-lookup"><span data-stu-id="c9b96-118">The Content-Type header describes the data in the part.</span></span> <span data-ttu-id="c9b96-119">Wenn dieser Header fehlt, ist die Standardeinstellung Text/Plain an.</span><span class="sxs-lookup"><span data-stu-id="c9b96-119">If this header is omitted, the default is text/plain.</span></span>

<span data-ttu-id="c9b96-120">Im vorherigen Beispiel hochgeladen der Benutzer eine Datei namens GrandCanyon.jpg, mit dem Inhaltstyp Image/Jpeg; und der Wert der Texteingabe war &quot;Summer Urlaub&quot;.</span><span class="sxs-lookup"><span data-stu-id="c9b96-120">In the previous example, the user uploaded a file named GrandCanyon.jpg, with content type image/jpeg; and the value of the text input was &quot;Summer Vacation&quot;.</span></span>

## <a name="file-upload"></a><span data-ttu-id="c9b96-121">Datei hochladen</span><span class="sxs-lookup"><span data-stu-id="c9b96-121">File Upload</span></span>

<span data-ttu-id="c9b96-122">Jetzt sehen wir uns einen Web-API-Controller, die Dateien aus einer mehrteiligen MIME-Nachricht liest.</span><span class="sxs-lookup"><span data-stu-id="c9b96-122">Now let's look at a Web API controller that reads files from a multipart MIME message.</span></span> <span data-ttu-id="c9b96-123">Der Controller wird für die Dateien asynchron gelesen werden.</span><span class="sxs-lookup"><span data-stu-id="c9b96-123">The controller will read the files asynchronously.</span></span> <span data-ttu-id="c9b96-124">Web-API unterstützt die asynchrone Aktionen, die mit der [aufgabenbasierten Programmiermodell](https://msdn.microsoft.com/library/dd460693.aspx).</span><span class="sxs-lookup"><span data-stu-id="c9b96-124">Web API supports asynchronous actions using the [task-based programming model](https://msdn.microsoft.com/library/dd460693.aspx).</span></span> <span data-ttu-id="c9b96-125">Zunächst hier stammt der Code bei der Sie Ausrichtung auf .NET Framework 4.5, das unterstützt die **Async** und **"await"** Schlüsselwörter.</span><span class="sxs-lookup"><span data-stu-id="c9b96-125">First, here is the code if you are targeting .NET Framework 4.5, which supports the **async** and **await** keywords.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

<span data-ttu-id="c9b96-126">Beachten Sie, dass der Controlleraktion keine Parameter akzeptiert.</span><span class="sxs-lookup"><span data-stu-id="c9b96-126">Notice that the controller action does not take any parameters.</span></span> <span data-ttu-id="c9b96-127">Da wir den Hauptteil der Anforderung innerhalb der Aktion verarbeiten, ohne einen medientypformatierer aufzurufen ist.</span><span class="sxs-lookup"><span data-stu-id="c9b96-127">That's because we process the request body inside the action, without invoking a media-type formatter.</span></span>

<span data-ttu-id="c9b96-128">Die **IsMultipartContent** Methode überprüft, ob die Anforderung eine mehrteilige MIME-Nachricht enthält.</span><span class="sxs-lookup"><span data-stu-id="c9b96-128">The **IsMultipartContent** method checks whether the request contains a multipart MIME message.</span></span> <span data-ttu-id="c9b96-129">Wenn dies nicht der Fall ist, wird der Controller zurückgegeben, Statuscode "HTTP" 415 (nicht unterstützter Medientyp).</span><span class="sxs-lookup"><span data-stu-id="c9b96-129">If not, the controller returns HTTP status code 415 (Unsupported Media Type).</span></span>

<span data-ttu-id="c9b96-130">Die **MultipartFormDataStreamProvider** Klasse ist Hilfsobjekt, das Dateistreams für hochgeladene Dateien belegt wird.</span><span class="sxs-lookup"><span data-stu-id="c9b96-130">The **MultipartFormDataStreamProvider** class is a helper object that allocates file streams for uploaded files.</span></span> <span data-ttu-id="c9b96-131">Um den mehrteiligen MIME-Nachricht zu lesen, rufen Sie die **ReadAsMultipartAsync** Methode.</span><span class="sxs-lookup"><span data-stu-id="c9b96-131">To read the multipart MIME message, call the **ReadAsMultipartAsync** method.</span></span> <span data-ttu-id="c9b96-132">Diese Methode alle Teile der Nachricht extrahiert und schreibt sie in die Datenströme gebotenen der **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="c9b96-132">This method extracts all of the message parts and writes them into the streams provided by the **MultipartFormDataStreamProvider**.</span></span>

<span data-ttu-id="c9b96-133">Wenn die Methode abgeschlossen ist, erhalten Sie Informationen zu den Dateien aus dem **FileData** -Eigenschaft, die eine Auflistung von **MultipartFileData** Objekte.</span><span class="sxs-lookup"><span data-stu-id="c9b96-133">When the method completes, you can get information about the files from the **FileData** property, which is a collection of **MultipartFileData** objects.</span></span>

- <span data-ttu-id="c9b96-134">**MultipartFileData.FileName** ist der Name der lokalen Datei auf dem Server, in dem die Datei gespeichert wurde.</span><span class="sxs-lookup"><span data-stu-id="c9b96-134">**MultipartFileData.FileName** is the local file name on the server, where the file was saved.</span></span>
- <span data-ttu-id="c9b96-135">**MultipartFileData.Headers** enthält der Header Teil (*nicht* Anforderungsheader).</span><span class="sxs-lookup"><span data-stu-id="c9b96-135">**MultipartFileData.Headers** contains the part header (*not* the request header).</span></span> <span data-ttu-id="c9b96-136">Hiermit können Sie den Zugriff auf die Inhalte\_Disposition "und" Content-Type-Header.</span><span class="sxs-lookup"><span data-stu-id="c9b96-136">You can use this to access the Content\_Disposition and Content-Type headers.</span></span>

<span data-ttu-id="c9b96-137">Wie der Name bereits vermuten lässt, **ReadAsMultipartAsync** ist eine asynchrone Methode.</span><span class="sxs-lookup"><span data-stu-id="c9b96-137">As the name suggests, **ReadAsMultipartAsync** is an asynchronous method.</span></span> <span data-ttu-id="c9b96-138">Verwenden Sie Aufgaben nach Abschluss der Methode, eine [Fortsetzungsaufgabe](https://msdn.microsoft.com/en-us/library/ee372288.aspx) (.NET 4.0) oder die **"await"** Schlüsselwort (.NET 4.5).</span><span class="sxs-lookup"><span data-stu-id="c9b96-138">To perform work after the method completes, use a [continuation task](https://msdn.microsoft.com/en-us/library/ee372288.aspx) (.NET 4.0) or the **await** keyword (.NET 4.5).</span></span>

<span data-ttu-id="c9b96-139">Hier ist die .NET Framework 4.0-Version des vorherigen Codes ein:</span><span class="sxs-lookup"><span data-stu-id="c9b96-139">Here is the .NET Framework 4.0 version of the previous code:</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a><span data-ttu-id="c9b96-140">Lesen von Formulardaten-Steuerelement</span><span class="sxs-lookup"><span data-stu-id="c9b96-140">Reading Form Control Data</span></span>

<span data-ttu-id="c9b96-141">Das HTML-Formular, das zuvor gezeigten mussten ein Texteingabe-Steuerelements.</span><span class="sxs-lookup"><span data-stu-id="c9b96-141">The HTML form that I showed earlier had a text input control.</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

<span data-ttu-id="c9b96-142">Sie erhalten den Wert des Steuerelements aus der **FormData** Eigenschaft von der **MultipartFormDataStreamProvider**.</span><span class="sxs-lookup"><span data-stu-id="c9b96-142">You can get the value of the control from the **FormData** property of the **MultipartFormDataStreamProvider**.</span></span>

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

<span data-ttu-id="c9b96-143">**FormData** ist ein **NameValueCollection** enthält Name/Wert-Paare für die Steuerelemente des Formulars.</span><span class="sxs-lookup"><span data-stu-id="c9b96-143">**FormData** is a **NameValueCollection** that contains name/value pairs for the form controls.</span></span> <span data-ttu-id="c9b96-144">Die Auflistung kann doppelte Schlüssel enthalten.</span><span class="sxs-lookup"><span data-stu-id="c9b96-144">The collection can contain duplicate keys.</span></span> <span data-ttu-id="c9b96-145">Betrachten Sie dieses Formular ein:</span><span class="sxs-lookup"><span data-stu-id="c9b96-145">Consider this form:</span></span>

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

<span data-ttu-id="c9b96-146">Der Anforderungstext kann wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="c9b96-146">The request body might look like this:</span></span>

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

<span data-ttu-id="c9b96-147">In diesem Fall die **FormData** Auflistung würde die folgenden Schlüssel/Wert-Paare enthalten:</span><span class="sxs-lookup"><span data-stu-id="c9b96-147">In that case, the **FormData** collection would contain the following key/value pairs:</span></span>

- <span data-ttu-id="c9b96-148">Reise: Round-Trip</span><span class="sxs-lookup"><span data-stu-id="c9b96-148">trip: round-trip</span></span>
- <span data-ttu-id="c9b96-149">Optionen: ohne Unterbrechung bis</span><span class="sxs-lookup"><span data-stu-id="c9b96-149">options: nonstop</span></span>
- <span data-ttu-id="c9b96-150">Optionen: Datumsangaben</span><span class="sxs-lookup"><span data-stu-id="c9b96-150">options: dates</span></span>
- <span data-ttu-id="c9b96-151">Arbeitsplätze: Fenster</span><span class="sxs-lookup"><span data-stu-id="c9b96-151">seat: window</span></span>
