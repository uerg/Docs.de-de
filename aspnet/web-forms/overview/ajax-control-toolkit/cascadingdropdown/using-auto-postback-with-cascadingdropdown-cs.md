---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Verwenden von Auto-Postback mit CascadingDropDown (c#) | Microsoft Docs
author: wenz
description: "Das CascadingDropDown-Steuerelement in das AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, damit, dass Änderungen in einer DropDownList lädt Werte in Anoth verknüpft..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: cd103283f46223d5158e58227bb53c00c74bc7d9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a>Verwenden von Auto-Postback mit CascadingDropDown (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)

> Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet. Jedoch bei der CascadingDropDown-Steuerelement ASP verwenden. NET DropDownList-Steuerelement AutoPostBack-Feature funktioniert nicht, da eine (nicht erforderlich) Postback selbst asynchron laden von Daten in der Liste generiert werden. Dieser Effekt kann vermieden werden, durch einige JavaScript-Code.


## <a name="overview"></a>Übersicht

Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet. (Z. B. eine Liste enthält eine Liste von US-Bundesstaaten und die folgenden Liste wird dann mit großen Städten in diesem Zustand gefüllt.) Jedoch bei der CascadingDropDown-Steuerelement ASP verwenden. NET DropDownList-Steuerelement AutoPostBack-Feature funktioniert nicht, da eine (nicht erforderlich) Postback selbst asynchron laden von Daten in der Liste generiert werden. Dieser Effekt kann vermieden werden, durch einige JavaScript-Code.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der &lt; `form` &gt; Element):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

Klicken Sie dann, ist ein DropDownList-Steuerelement erforderlich:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

Für diese Liste wird ein CascadingDropDown Extender Bereitstellung von Web-Dienst-URL und -Methode hinzugefügt:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

Der Extender CascadingDropDown ruft dann asynchron einen Webdienst mit der folgenden Methodensignatur:

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

Die Methode gibt ein Array vom Typ CascadingDropDown-Wert. Typkonstruktor erwartet zuerst den Listeneintrag Beschriftung, und klicken Sie dann den Wert (HTML `value` Attribut).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

Beim Laden der Seite im Browser füllt die Dropdown-Liste mit den drei Anbietern dem zweiten Ausdruck wird vorab ausgewählt. ASP.NET definiert außerdem die `__doPostBack()` JavaScript-Methode. Sobald die Seite geladen wurde, wird dieser JavaScript-Funktionsaufruf der Dropdownliste aus, nur wenn jedoch auch noch Elemente darin hinzugefügt. Wenn in der Liste keine Elemente vorhanden sind, ist die Control-Toolkit derzeit, von lädt, sodass die JavaScript-Code einen Timeout verwendet und in einer halben Sekunde erneut versucht.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

Auf diese Weise wird ein Postback nur ausgeführt, wenn in der Liste tatsächlich Elemente vorhanden sind und der Benutzer einen Eintrag wählt.


[![Wenn Sie ein Listenelement aktivieren, wird einen postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)

Wenn Sie ein Listenelement aktivieren, wird einen Postback ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))

>[!div class="step-by-step"]
[Zurück](presetting-list-entries-with-cascadingdropdown-cs.md)
[Weiter](filling-a-list-using-cascadingdropdown-vb.md)
