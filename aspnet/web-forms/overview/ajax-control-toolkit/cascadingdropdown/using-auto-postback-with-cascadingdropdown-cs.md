---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
title: Verwenden von automatischem Postback mit CascadingDropDown (c#) | Microsoft-Dokumentation
author: wenz
description: Das Steuerelement "CascadingDropDown" im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList lädt Werte in Anoth verknüpft...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 6755d8d9-14be-4a1d-86e5-1a6110f3dea8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: bece4f78b46c44db988e04e0e987450d94c37aab
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819379"
---
<a name="using-auto-postback-with-cascadingdropdown-c"></a>Verwenden von automatischem Postback mit CascadingDropDown (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3CS.pdf)

> Das Steuerelement "CascadingDropDown" im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList lädt Werte in einer anderen DropDownList verknüpft ist. Jedoch beim Einsatz des CascadingDropDown-Steuerelements, ASP. NET DropDownList-Steuerelement AutoPostBack-Feature funktioniert nicht, da ein (nicht erforderlich) Postback selbst asynchron laden von Daten in der Liste generiert werden. Dieser Effekt kann vermieden werden, und Javascriptcode.


## <a name="overview"></a>Übersicht

Das Steuerelement "CascadingDropDown" im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList lädt Werte in einer anderen DropDownList verknüpft ist. (Z. B. eine Liste enthält eine Liste der US-Staaten und die folgenden Liste wird dann mit der größten Städte in diesem Zustand gefüllt.) Jedoch beim Einsatz des CascadingDropDown-Steuerelements, ASP. NET DropDownList-Steuerelement AutoPostBack-Feature funktioniert nicht, da ein (nicht erforderlich) Postback selbst asynchron laden von Daten in der Liste generiert werden. Dieser Effekt kann vermieden werden, und Javascriptcode.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit, aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden muss (jedoch innerhalb der &lt; `form` &gt; Element):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample1.aspx)]

Anschließend ist ein DropDownList-Steuerelement erforderlich:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample2.aspx)]

Für diese Liste wird ein CascadingDropDown-Extender Bereitstellen von Informationen von Web-Dienst-URL und -Methode hinzugefügt:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample3.aspx)]

Der CascadingDropDown-Extender wird zwischenrückruf dann ein Webdienst mit der folgenden Methodensignatur:

[!code-csharp[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample4.cs)]

Die Methode gibt ein Array vom Typ CascadingDropDown-Wert zurück. Der Konstruktor des Typs erwartet, dass zuerst den Listeneintrag Beschriftung, und klicken Sie dann den Wert (HTML `value` Attribut).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample5.aspx)]

Beim Laden der Seite im Browser füllt die Dropdown-Liste mit drei Anbietern zusammen, das zweite Argument wird standardmäßig ausgewählt. ASP.NET darüber hinaus definiert die `__doPostBack()` JavaScript-Methode. Sobald die Seite geladen wurde, wird diese JavaScript-Aufruf der Dropdown-Liste, aber nur wenn sind Elemente darin hinzugefügt. Wenn in der Liste keine Elemente vorhanden sind, wird das Steuerelement-Toolkit derzeit, geladen und der JavaScript-Code verwendet einen Timeout in einer halben Sekunde erneut versucht.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-cs/samples/sample6.html)]

Auf diese Weise wird ein Postback nur ausgeführt, wenn in der Liste tatsächlich Elemente vorhanden sind und der Benutzer einen Eintrag wählt.


[![Wählen ein Listenelement auslöst ein postback](using-auto-postback-with-cascadingdropdown-cs/_static/image2.png)](using-auto-postback-with-cascadingdropdown-cs/_static/image1.png)

Wenn Sie ein Listenelement wird einen Postback ([klicken Sie, um das Bild in voller Größe anzeigen](using-auto-postback-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](presetting-list-entries-with-cascadingdropdown-cs.md)
> [Weiter](filling-a-list-using-cascadingdropdown-vb.md)
