---
uid: mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
title: TagBuilder-Klasse zum Erstellen von HTML-Hilfsprogrammen (VB) mit | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther stellt ein hilfreiches Dienstprogramm-Klasse in ASP.NET MVC-Framework mit dem Namen der TagBuilder-Klasse. Sie können die TagBuilder-Klasse, um problemlos verwenden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: ec26f264-d0ea-4031-9943-825505a3ac4b
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/views/using-the-tagbuilder-class-to-build-html-helpers-vb
msc.type: authoredcontent
ms.openlocfilehash: b95073a53e73ebe4035ef9b8dcdf84dc3c4febee
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370956"
---
<a name="using-the-tagbuilder-class-to-build-html-helpers-vb"></a>Verwenden der TagBuilder-Klasse zum Erstellen von HTML-Hilfsprogrammen (VB)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther stellt ein hilfreiches Dienstprogramm-Klasse in ASP.NET MVC-Framework mit dem Namen der TagBuilder-Klasse. Sie können die TagBuilder-Klasse verwenden, auf einfache Weise HTML-Tags erstellen.


ASP.NET MVC-Framework umfasst eine hilfreiches Dienstprogramm-Klasse, die mit dem Namen der TagBuilder-Klasse, die Sie beim Erstellen von HTML-Hilfsprogramme verwenden können. TagBuilder-Klasse, können wie der Name der Klasse bereits vermuten lässt, Sie ganz einfach HTML-Tags erstellen. In diesem kurzen Tutorial finden Sie eine Übersicht über die TagBuilder-Klasse, und erfahren Sie, wie Sie diese Klasse verwenden, beim Rendern von HTML erstellen eine einfache HTML-Hilfsobjekt, &lt;Img&gt; Tags.

## <a name="overview-of-the-tagbuilder-class"></a>Übersicht über die TagBuilder-Klasse

TagBuilder-Klasse ist in der System.Web.Mvc-Namespace enthalten. Er hat fünf Methoden:

- AddCssClass() – ermöglicht Ihnen das Hinzufügen eines neuen *Klasse = ""* eine Tag-Attribut.
- GenerateId() – können Sie ein Tag ein Id-Attribut hinzu. Diese Methode ersetzt automatisch Punkte in der Id (in der Standardeinstellung werden Punkte durch Unterstriche ersetzt)
- MergeAttribute() – können Sie Attribute, die ein Tag hinzuzufügen. Es gibt mehrere Überladungen dieser Methode.
- SetInnerText() – können Sie den inneren Text des Tags festlegen. Der innere Text ist HTML-Codierung.
- ToString() – können Sie das Tag zu rendern. Sie können angeben, ob es sich bei einem normalen Tag, ein Starttag, ein Endtag oder ein selbstschließendes Tag erstellen möchten.
  

TagBuilder-Klasse verfügt über vier wichtige Eigenschaften:

- Attribute – alle Attribute des Tags darstellt.
- IdAttributeDotReplacement – stellt das Zeichen, die von der GenerateId()-Methode verwendet werden, um die Zeiträume (der Standardwert ist ein Unterstrich) ersetzt.
- InnerHTML – stellt den inneren Inhalt des Tags dar. Diese Eigenschaft eine Zeichenfolge zuweisen *nicht* HTML-Codierung die Zeichenfolge.
- TagName: stellt den Namen des Tags dar.

Diese Methoden und Eigenschaften erhalten Sie alle grundlegenden Methoden und Eigenschaften, die Sie benötigen, um ein HTML-Tag zu erstellen. Sie müssen unbedingt die TagBuilder-Klasse verwenden zu können. Sie können stattdessen eine StringBuilder-Klasse verwenden. Allerdings TagBuilder-Klasse das Leben macht etwas leichter.

## <a name="creating-an-image-html-helper"></a>Erstellen ein Image-HTML-Hilfsprogramm

Wenn Sie eine Instanz der TagBuilder-Klasse erstellen, übergeben Sie den Namen des Tags, die Sie an den Konstruktor TagBuilder erstellen möchten. Anschließend rufen Sie Methoden wie die AddCssClass und MergeAttribute()-Methoden, die Attribute des Tags ändern. Schließlich rufen Sie die ToString()-Methode zum Rendern des Tags.

Liste 1 enthält beispielsweise ein Bild HTML-Hilfsprogramm. Das Image-Hilfsprogramm wird intern implementiert, mit einem TagBuilder, die eine HTML-darstellt &lt;Img&gt; Tag.

**Codebeispiel 1 – Helpers\ImageHelper.vb**

[!code-vb[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample1.vb)]

Das Modul in Codebeispiel 1 enthält zwei überladene Methoden, die mit dem Namen Image(). Wenn Sie die Image()-Methode aufrufen, können Sie ein Objekt übergeben, steht eine Reihe von HTML-Attribute oder nicht.

Beachten Sie, wie die TagBuilder.MergeAttribute()-Methode verwendet wird, die TagBuilder einzelner Attribute wie z. B. das Src-Attribut hinzu. Beachten Sie außerdem, wie die TagBuilder.MergeAttributes()-Methode verwendet wird, eine Auflistung von Attributen der TagBuilder hinzu. Die MergeAttributes()-Methode akzeptiert ein Wörterbuch&lt;string, object&gt; Parameter. Die RouteValueDictionary-Klasse wird verwendet, um das Objekt, das die Auflistung von Attributen in ein Wörterbuch darstellt konvertieren&lt;string, object&gt;.

Nach der Erstellung der Image-Hilfe können Sie das Hilfsprogramm in Ihren ASP.NET MVC-Ansichten, genau wie die standardmäßigen HTML-Hilfsprogramme verwenden. Die Ansicht im Codebeispiel 2 verwendet das Image-Hilfsprogramm, um dem gleichen Image von einer Xbox zweimal anzuzeigen (siehe Abbildung 1). Das Hilfsprogramm Image() heißt mit und ohne eine Auflistung der HTML-Attribute.

**Codebeispiel 2 – Home\Index.aspx**

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample2.aspx)]


[![Das Dialogfeld "Neues Projekt"](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.jpg)](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image1.png)

**Abbildung 01**: mit dem Image-Hilfsprogramm ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-tagbuilder-class-to-build-html-helpers-vb/_static/image2.png))


Beachten Sie, dass Sie mit dem Image-Helfer am oberen Rand der Ansicht Index.aspx verbundene Namespaces importieren müssen. Das Hilfsprogramm wird mit der folgenden Anweisung importiert:

[!code-aspx[Main](using-the-tagbuilder-class-to-build-html-helpers-vb/samples/sample3.aspx)]

In Visual Basic-Anwendungen ist der Standardnamespace der identisch mit dem Namen der Anwendung.

> [!div class="step-by-step"]
> [Zurück](creating-custom-html-helpers-vb.md)
> [Weiter](creating-page-layouts-with-view-master-pages-vb.md)
