---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Verwenden von Auto-Postback mit CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Das CascadingDropDown-Steuerelement in das AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, damit, dass Änderungen in einer DropDownList lädt Werte in Anoth verknüpft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: e8a48692dd96ee6a647bfb57ce2915b4e85544fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="using-auto-postback-with-cascadingdropdown-vb"></a>Verwenden von Auto-Postback mit CascadingDropDown (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)

> Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet. Jedoch bei der CascadingDropDown-Steuerelement ASP verwenden. NET DropDownList-Steuerelement AutoPostBack-Feature funktioniert nicht, da eine (nicht erforderlich) Postback selbst asynchron laden von Daten in der Liste generiert werden. Dieser Effekt kann vermieden werden, durch einige JavaScript-Code.


## <a name="overview"></a>Übersicht

Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet. (Z. B. eine Liste enthält eine Liste von US-Bundesstaaten und die folgenden Liste wird dann mit großen Städten in diesem Zustand gefüllt.) Jedoch bei der CascadingDropDown-Steuerelement ASP verwenden. NET DropDownList-Steuerelement AutoPostBack-Feature funktioniert nicht, da eine (nicht erforderlich) Postback selbst asynchron laden von Daten in der Liste generiert werden. Dieser Effekt kann vermieden werden, durch einige JavaScript-Code.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der &lt; `form` &gt; Element):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

Klicken Sie dann, ist ein DropDownList-Steuerelement erforderlich:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

Für diese Liste wird ein CascadingDropDown Extender Bereitstellung von Web-Dienst-URL und -Methode hinzugefügt:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

Der Extender CascadingDropDown ruft dann asynchron einen Webdienst mit der folgenden Methodensignatur:

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

Die Methode gibt ein Array vom Typ CascadingDropDown-Wert. Typkonstruktor erwartet zuerst den Listeneintrag Beschriftung, und klicken Sie dann den Wert (HTML `value` Attribut).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

Beim Laden der Seite im Browser füllt die Dropdown-Liste mit den drei Anbietern dem zweiten Ausdruck wird vorab ausgewählt. ASP.NET definiert außerdem die `__doPostBack()` JavaScript-Methode. Sobald die Seite geladen wurde, wird dieser JavaScript-Funktionsaufruf der Dropdownliste aus, nur wenn jedoch auch noch Elemente darin hinzugefügt. Wenn in der Liste keine Elemente vorhanden sind, ist die Control-Toolkit derzeit, von lädt, sodass die JavaScript-Code einen Timeout verwendet und in einer halben Sekunde erneut versucht.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

Auf diese Weise wird ein Postback nur ausgeführt, wenn in der Liste tatsächlich Elemente vorhanden sind und der Benutzer einen Eintrag wählt.


[![Wenn Sie ein Listenelement aktivieren, wird einen postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)

Wenn Sie ein Listenelement aktivieren, wird einen Postback ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Vorherige](presetting-list-entries-with-cascadingdropdown-vb.md)
