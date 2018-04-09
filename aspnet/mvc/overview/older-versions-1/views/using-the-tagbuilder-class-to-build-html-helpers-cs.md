---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
title: Verwenden der TagBuilder-Klasse zum Erstellen von HTML-Hilfsmethoden (c#) | Microsoft Docs
author: StephenWalther
description: Stephen Walther stellt Ihnen eine nützliche Hilfsprogrammklasse in ASP.NET MVC-Framework mit dem Namen der TagBuilder-Klasse. Sie können einfach die TagBuilder-Klasse, um...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 3975a52f-bd15-4edd-8f3d-1df93672515b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 6c0e8e4e3a733f2cc8690dc85e3006bce6c661d2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-c"></a>Verwenden die TagBuilder-Klasse zum Erstellen von HTML-Hilfsmethoden (c#)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther stellt Ihnen eine nützliche Hilfsprogrammklasse in ASP.NET MVC-Framework mit dem Namen der TagBuilder-Klasse. Sie können die TagBuilder-Klasse verwenden, problemlos HTML-Tags erstellen.


ASP.NET MVC-Framework umfasst eine nützliche Hilfsprogrammklasse, die mit dem Namen der TagBuilder-Klasse, die Sie beim Erstellen von HTML-Hilfsmethoden verwenden können. Die TagBuilder-Klasse, können wie der Name der Klasse bereits vermuten lässt, Sie problemlos HTML-Tags erstellen. In diesem kurzen Tutorial werden Sie einen Überblick über die TagBuilder-Klasse bereitgestellt, und erfahren Sie, wie Sie diese Klasse verwenden, beim Erstellen einer einfachen HTML-Hilfsobjekt, der HTML rendert &lt;Img&gt; Tags.

## <a name="overview-of-the-tagbuilder-class"></a>Übersicht über die TagBuilder-Klasse

Die Klasse TagBuilder ist im System.Web.Mvc-Namespace enthalten. Sie verfügt über fünf Methoden:

- AddCssClass() - können Sie zum Hinzufügen einer neuen *Klasse = ""* ein Tag-Attribut.
- GenerateId() - Enables you to add an id attribute to a tag. Diese Methode automatisch ersetzt Punkte in der Id (Standardmäßig werden Punkte durch Unterstriche ersetzt)
- MergeAttribute() - können Sie ein Tag Attribute hinzuzufügen. Es gibt mehrere Überladungen dieser Methode.
- SetInnerText() - können Sie den inneren Text des Tags festlegen. Der innere Text ist HTML-Codierung automatisch.
- ToString() - können Sie das Tag zu rendern. Sie können angeben, ob ein normaler Tag, ein Starttag, ein Endtag oder eines selbstschließenden Tags erstellt werden soll.
  

Die TagBuilder-Klasse verfügt über vier wichtige Eigenschaften:

- Attribute – alle Attribute des Tags darstellt.
- IdAttributeDotReplacement - darstellt, das Zeichen, die von der GenerateId()-Methode verwendet, um Zeiträume (der Standardwert ist ein Unterstrich) zu ersetzen.
- InnerHTML - stellt die inneren Inhalte des Tags dar. Diese Eigenschaft eine Zeichenfolge zuweisen *nicht* HTML-Codierung die Zeichenfolge.
- TagName - stellt den Namen des Tags.

Diese Methoden und Eigenschaften geben Ihnen alle grundlegenden Methoden und Eigenschaften, die Sie benötigen, um ein HTML-Tag zu erstellen. Sie müssen nicht wirklich die TagBuilder-Klasse verwenden zu können. Sie können stattdessen eine StringBuilder-Klasse. Allerdings wird die TagBuilder-Klasse das Leben etwas einfacher.

## <a name="creating-an-image-html-helper"></a>Erstellen eine Bild-HTML-Hilfe

Wenn Sie eine Instanz der Klasse TagBuilder erstellen, übergeben Sie den Namen des Tags, die Sie an den Konstruktor TagBuilder erstellen möchten. Anschließend rufen Sie Methoden wie z. B. die AddCssClass und MergeAttribute()-Methode, um die Attribute des Tags ändern. Zum Schluss rufen Sie die ToString()-Methode, um das Tag zu rendern.

Auflisten von 1 enthält z. B. ein Bild HTML-Hilfsobjekt. Das Image-Hilfsprogramm wird mit einem TagBuilder, die ein HTML-darstellt intern implementiert &lt;Img&gt; Tag.

**1 – Helpers\ImageHelper.cs auflisten**

[!code-csharp[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample1.cs)]

Die Klasse im Codebeispiel 1 enthält zwei statische überladene Methoden, die mit dem Namen Bild. Wenn Sie die Image()-Methode aufrufen, können Sie ein Objekt übergeben, die einen Satz von HTML-Attribute oder nicht darstellt.

Beachten Sie, wie die TagBuilder.MergeAttribute()-Methode verwendet wird, die TagBuilder einzelner Attribute wie z. B. das Src-Attribut hinzu. Beachten Sie außerdem, wie die TagBuilder.MergeAttributes()-Methode verwendet wird, um eine Auflistung von Attributen der TagBuilder hinzufügen. Die Methode MergeAttributes() akzeptiert ein Wörterbuch&lt;string, object&gt; Parameter. Die RouteValueDictionary-Klasse wird verwendet, um das Objekt, das die Auflistung von Attributen in einem Wörterbuch darstellt zu konvertieren&lt;string, object&gt;.

Nachdem das Image-Hilfsobjekt erstellt wurde, können Sie das Hilfsprogramm in Ihre ASP.NET MVC-Ansichten genau wie die standardmäßigen HTML-Hilfsmethoden verwenden. Die Ansicht im Codebeispiel 2 verwendet das Image-Hilfsobjekt dasselbe Bild von einer Xbox zweimal angezeigt (siehe Abbildung 1). Das Hilfsobjekt Image() wird mit und ohne eine HTML-attributauflistung aufgerufen.

**Auflisten von 2 – Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample2.aspx)]


[![Das Dialogfeld "Neues Projekt"](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image1.png)

**Abbildung 01**: Verwenden der Image-Hilfsmethode ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-tagbuilder-class-to-build-html-helpers-cs/_static/image2.png))


Beachten Sie, dass Sie den Namespace, der dem Image-Hilfsprogramm am oberen Rand der Ansicht Index.aspx zugeordnete importieren müssen. Das Hilfsprogramm wird durch die folgende Direktive importiert:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-cs/samples/sample3.aspx)]

> [!div class="step-by-step"]
> [Zurück](creating-custom-html-helpers-cs.md)
> [Weiter](creating-page-layouts-with-view-master-pages-cs.md)
