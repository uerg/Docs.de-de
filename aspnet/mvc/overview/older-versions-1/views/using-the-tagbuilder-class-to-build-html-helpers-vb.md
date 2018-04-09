---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: Verwenden der TagBuilder-Klasse zum Erstellen von HTML-Hilfsmethoden (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther stellt Ihnen eine nützliche Hilfsprogrammklasse in ASP.NET MVC-Framework mit dem Namen der TagBuilder-Klasse. Sie können einfach die TagBuilder-Klasse, um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: 2b72e08dff646f66252f210543230186cab6e641
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a><span data-ttu-id="eca62-104">Verwenden die TagBuilder-Klasse zum Erstellen von HTML-Hilfsmethoden (VB)</span><span class="sxs-lookup"><span data-stu-id="eca62-104">Using the TagBuilder Class to Build HTML Helpers (VB)</span></span>
====================
<span data-ttu-id="eca62-105">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="eca62-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="eca62-106">Stephen Walther stellt Ihnen eine nützliche Hilfsprogrammklasse in ASP.NET MVC-Framework mit dem Namen der TagBuilder-Klasse.</span><span class="sxs-lookup"><span data-stu-id="eca62-106">Stephen Walther introduces you to a useful utility class in the ASP.NET MVC framework named the TagBuilder class.</span></span> <span data-ttu-id="eca62-107">Sie können die TagBuilder-Klasse verwenden, problemlos HTML-Tags erstellen.</span><span class="sxs-lookup"><span data-stu-id="eca62-107">You can use the TagBuilder class to easily build HTML tags.</span></span>


<span data-ttu-id="eca62-108">ASP.NET MVC-Framework umfasst eine nützliche Hilfsprogrammklasse, die mit dem Namen der TagBuilder-Klasse, die Sie beim Erstellen von HTML-Hilfsmethoden verwenden können.</span><span class="sxs-lookup"><span data-stu-id="eca62-108">The ASP.NET MVC framework includes a useful utility class named the TagBuilder class that you can use when building HTML helpers.</span></span> <span data-ttu-id="eca62-109">Die TagBuilder-Klasse, können wie der Name der Klasse bereits vermuten lässt, Sie problemlos HTML-Tags erstellen.</span><span class="sxs-lookup"><span data-stu-id="eca62-109">The TagBuilder class, as the name of the class suggests, enables you to easily build HTML tags.</span></span> <span data-ttu-id="eca62-110">In diesem kurzen Tutorial werden Sie einen Überblick über die TagBuilder-Klasse bereitgestellt, und erfahren Sie, wie Sie diese Klasse verwenden, beim Erstellen einer einfachen HTML-Hilfsobjekt, der HTML rendert &lt;Img&gt; Tags.</span><span class="sxs-lookup"><span data-stu-id="eca62-110">In this brief tutorial, you are provided with an overview of the TagBuilder class and you learn how to use this class when building a simple HTML helper that renders HTML &lt;img&gt; tags.</span></span>

## <a name="overview-of-the-tagbuilder-class"></a><span data-ttu-id="eca62-111">Übersicht über die TagBuilder-Klasse</span><span class="sxs-lookup"><span data-stu-id="eca62-111">Overview of the TagBuilder Class</span></span>

<span data-ttu-id="eca62-112">Die Klasse TagBuilder ist im System.Web.Mvc-Namespace enthalten.</span><span class="sxs-lookup"><span data-stu-id="eca62-112">The TagBuilder class is contained in the System.Web.Mvc namespace.</span></span> <span data-ttu-id="eca62-113">Sie verfügt über fünf Methoden:</span><span class="sxs-lookup"><span data-stu-id="eca62-113">It has five methods:</span></span>

- <span data-ttu-id="eca62-114">AddCssClass() – ermöglicht das Hinzufügen einer neuen *Klasse = ""* ein Tag-Attribut.</span><span class="sxs-lookup"><span data-stu-id="eca62-114">AddCssClass() – Enables you to add a new *class=""* attribute to a tag.</span></span>
- <span data-ttu-id="eca62-115">GenerateId() – können Sie ein Tag ein Id-Attribut hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="eca62-115">GenerateId() – Enables you to add an id attribute to a tag.</span></span> <span data-ttu-id="eca62-116">Diese Methode automatisch ersetzt Punkte in der Id (Standardmäßig werden Punkte durch Unterstriche ersetzt)</span><span class="sxs-lookup"><span data-stu-id="eca62-116">This method automatically replaces periods in the id (by default, periods are replaced by underscores)</span></span>
- <span data-ttu-id="eca62-117">MergeAttribute() – ermöglicht es Ihnen, um ein Tag Attribute hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="eca62-117">MergeAttribute() – Enables you to add attributes to a tag.</span></span> <span data-ttu-id="eca62-118">Es gibt mehrere Überladungen dieser Methode.</span><span class="sxs-lookup"><span data-stu-id="eca62-118">There are multiple overloads of this method.</span></span>
- <span data-ttu-id="eca62-119">SetInnerText() – können Sie den inneren Text des Tags festlegen.</span><span class="sxs-lookup"><span data-stu-id="eca62-119">SetInnerText() – Enables you to set the inner text of the tag.</span></span> <span data-ttu-id="eca62-120">Der innere Text ist HTML-Codierung automatisch.</span><span class="sxs-lookup"><span data-stu-id="eca62-120">The inner text is HTML encode automatically.</span></span>
- <span data-ttu-id="eca62-121">ToString() – können Sie das Tag zu rendern.</span><span class="sxs-lookup"><span data-stu-id="eca62-121">ToString() – Enables you to render the tag.</span></span> <span data-ttu-id="eca62-122">Sie können angeben, ob ein normaler Tag, ein Starttag, ein Endtag oder eines selbstschließenden Tags erstellt werden soll.</span><span class="sxs-lookup"><span data-stu-id="eca62-122">You can specify whether you want to create a normal tag, a start tag, an end tag, or a self-closing tag.</span></span>
  

<span data-ttu-id="eca62-123">Die TagBuilder-Klasse verfügt über vier wichtige Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="eca62-123">The TagBuilder class has four important properties:</span></span>

- <span data-ttu-id="eca62-124">Attribute – alle Attribute des Tags darstellt.</span><span class="sxs-lookup"><span data-stu-id="eca62-124">Attributes – Represents all of the attributes of the tag.</span></span>
- <span data-ttu-id="eca62-125">IdAttributeDotReplacement – stellt das Zeichen, die von der GenerateId()-Methode verwendet, um Zeiträume (der Standardwert ist ein Unterstrich) zu ersetzen.</span><span class="sxs-lookup"><span data-stu-id="eca62-125">IdAttributeDotReplacement – Represents the character used by the GenerateId() method to replace periods (the default is an underscore).</span></span>
- <span data-ttu-id="eca62-126">InnerHTML – stellt die inneren Inhalte des Tags dar.</span><span class="sxs-lookup"><span data-stu-id="eca62-126">InnerHTML – Represents the inner contents of the tag.</span></span> <span data-ttu-id="eca62-127">Diese Eigenschaft eine Zeichenfolge zuweisen *nicht* HTML-Codierung die Zeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="eca62-127">Assigning a string to this property *does not* HTML encode the string.</span></span>
- <span data-ttu-id="eca62-128">TagName – stellt den Namen des Tags.</span><span class="sxs-lookup"><span data-stu-id="eca62-128">TagName – Represents the name of the tag.</span></span>

<span data-ttu-id="eca62-129">Diese Methoden und Eigenschaften geben Ihnen alle grundlegenden Methoden und Eigenschaften, die Sie benötigen, um ein HTML-Tag zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="eca62-129">These methods and properties give you all of the basic methods and properties that you need to build up an HTML tag.</span></span> <span data-ttu-id="eca62-130">Sie müssen nicht wirklich die TagBuilder-Klasse verwenden zu können.</span><span class="sxs-lookup"><span data-stu-id="eca62-130">You don't really need to use the TagBuilder class.</span></span> <span data-ttu-id="eca62-131">Sie können stattdessen eine StringBuilder-Klasse.</span><span class="sxs-lookup"><span data-stu-id="eca62-131">You could use a StringBuilder class instead.</span></span> <span data-ttu-id="eca62-132">Allerdings wird die TagBuilder-Klasse das Leben etwas einfacher.</span><span class="sxs-lookup"><span data-stu-id="eca62-132">However, the TagBuilder class makes your life a little easier.</span></span>

## <a name="creating-an-image-html-helper"></a><span data-ttu-id="eca62-133">Erstellen eine Bild-HTML-Hilfe</span><span class="sxs-lookup"><span data-stu-id="eca62-133">Creating an Image HTML Helper</span></span>

<span data-ttu-id="eca62-134">Wenn Sie eine Instanz der Klasse TagBuilder erstellen, übergeben Sie den Namen des Tags, die Sie an den Konstruktor TagBuilder erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="eca62-134">When you create an instance of the TagBuilder class, you pass the name of the tag that you want to build to the TagBuilder constructor.</span></span> <span data-ttu-id="eca62-135">Anschließend rufen Sie Methoden wie z. B. die AddCssClass und MergeAttribute()-Methode, um die Attribute des Tags ändern.</span><span class="sxs-lookup"><span data-stu-id="eca62-135">Next, you can call methods like the AddCssClass and MergeAttribute() methods to modify the attributes of the tag.</span></span> <span data-ttu-id="eca62-136">Zum Schluss rufen Sie die ToString()-Methode, um das Tag zu rendern.</span><span class="sxs-lookup"><span data-stu-id="eca62-136">Finally, you call the ToString() method to render the tag.</span></span>

<span data-ttu-id="eca62-137">Auflisten von 1 enthält z. B. ein Bild HTML-Hilfsobjekt.</span><span class="sxs-lookup"><span data-stu-id="eca62-137">For example, Listing 1 contains an Image HTML helper.</span></span> <span data-ttu-id="eca62-138">Das Image-Hilfsprogramm wird mit einem TagBuilder, die ein HTML-darstellt intern implementiert &lt;Img&gt; Tag.</span><span class="sxs-lookup"><span data-stu-id="eca62-138">The Image helper is implemented internally with a TagBuilder that represents an HTML &lt;img&gt; tag.</span></span>

<span data-ttu-id="eca62-139">**1 – Helpers\ImageHelper.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="eca62-139">**Listing 1 – Helpers\ImageHelper.vb**</span></span>

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

<span data-ttu-id="eca62-140">Das Modul im Codebeispiel 1 enthält zwei überladene Methoden, die mit dem Namen Image().</span><span class="sxs-lookup"><span data-stu-id="eca62-140">The module in Listing 1 contains two overloaded methods named Image().</span></span> <span data-ttu-id="eca62-141">Wenn Sie die Image()-Methode aufrufen, können Sie ein Objekt übergeben, die einen Satz von HTML-Attribute oder nicht darstellt.</span><span class="sxs-lookup"><span data-stu-id="eca62-141">When you call the Image() method, you can pass an object which represents a set of HTML attributes or not.</span></span>

<span data-ttu-id="eca62-142">Beachten Sie, wie die TagBuilder.MergeAttribute()-Methode verwendet wird, die TagBuilder einzelner Attribute wie z. B. das Src-Attribut hinzu.</span><span class="sxs-lookup"><span data-stu-id="eca62-142">Notice how the TagBuilder.MergeAttribute() method is used to add individual attributes such as the src attribute to the TagBuilder.</span></span> <span data-ttu-id="eca62-143">Beachten Sie außerdem, wie die TagBuilder.MergeAttributes()-Methode verwendet wird, um eine Auflistung von Attributen der TagBuilder hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="eca62-143">Notice, furthermore, how the TagBuilder.MergeAttributes() method is used to add a collection of attributes to the TagBuilder.</span></span> <span data-ttu-id="eca62-144">Die Methode MergeAttributes() akzeptiert ein Wörterbuch&lt;string, object&gt; Parameter.</span><span class="sxs-lookup"><span data-stu-id="eca62-144">The MergeAttributes() method accepts a Dictionary&lt;string,object&gt; parameter.</span></span> <span data-ttu-id="eca62-145">Die RouteValueDictionary-Klasse wird verwendet, um das Objekt, das die Auflistung von Attributen in einem Wörterbuch darstellt zu konvertieren&lt;string, object&gt;.</span><span class="sxs-lookup"><span data-stu-id="eca62-145">The RouteValueDictionary class is used to convert the Object representing the collection of attributes into a Dictionary&lt;string,object&gt;.</span></span>

<span data-ttu-id="eca62-146">Nachdem das Image-Hilfsobjekt erstellt wurde, können Sie das Hilfsprogramm in Ihre ASP.NET MVC-Ansichten genau wie die standardmäßigen HTML-Hilfsmethoden verwenden.</span><span class="sxs-lookup"><span data-stu-id="eca62-146">After you create the Image helper, you can use the helper in your ASP.NET MVC views just like any of the other standard HTML helpers.</span></span> <span data-ttu-id="eca62-147">Die Ansicht im Codebeispiel 2 verwendet das Image-Hilfsobjekt dasselbe Bild von einer Xbox zweimal angezeigt (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="eca62-147">The view in Listing 2 uses the Image helper to display the same image of an Xbox twice (see Figure 1).</span></span> <span data-ttu-id="eca62-148">Das Hilfsobjekt Image() wird mit und ohne eine HTML-attributauflistung aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="eca62-148">The Image() helper is called both with and without an HTML attribute collection.</span></span>

<span data-ttu-id="eca62-149">**Auflisten von 2 – Home\Index.aspx**</span><span class="sxs-lookup"><span data-stu-id="eca62-149">**Listing 2 – Home\Index.aspx**</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]


<span data-ttu-id="eca62-150">[![Das Dialogfeld "Neues Projekt"](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="eca62-150">[![The New Project dialog box](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)</span></span>

<span data-ttu-id="eca62-151">**Abbildung 01**: Verwenden der Image-Hilfsmethode ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="eca62-151">**Figure 01**: Using the Image helper([Click to view full-size image](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))</span></span>


<span data-ttu-id="eca62-152">Beachten Sie, dass Sie den Namespace, der dem Image-Hilfsprogramm am oberen Rand der Ansicht Index.aspx zugeordnete importieren müssen.</span><span class="sxs-lookup"><span data-stu-id="eca62-152">Notice that you must import the namespace associated with the Image helper at the top of the Index.aspx view.</span></span> <span data-ttu-id="eca62-153">Das Hilfsprogramm wird durch die folgende Direktive importiert:</span><span class="sxs-lookup"><span data-stu-id="eca62-153">The helper is imported with the following directive:</span></span>

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

<span data-ttu-id="eca62-154">In einem Visual Basic-Anwendung ist der Standardnamespace identisch mit den Namen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="eca62-154">In a Visual Basic application, the default namespace is the same as the name of the application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="eca62-155">[Zurück](creating-custom-html-helpers-vb.md)
> [Weiter](creating-page-layouts-with-view-master-pages-vb.md)</span><span class="sxs-lookup"><span data-stu-id="eca62-155">[Previous](creating-custom-html-helpers-vb.md)
[Next](creating-page-layouts-with-view-master-pages-vb.md)</span></span>
