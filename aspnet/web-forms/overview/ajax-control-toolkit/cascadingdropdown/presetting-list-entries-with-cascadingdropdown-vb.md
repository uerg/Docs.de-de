---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
title: Vorheriges festlegen Listeneinträge mit CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: Das CascadingDropDown-Steuerelement in das AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, damit, dass Änderungen in einer DropDownList lädt Werte in Anoth verknüpft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ec61ced7-bbca-4bdd-aa3b-80878f295181
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: f74e6ac80b756240870d9406a03db11c610093aa
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="presetting-list-entries-with-cascadingdropdown-vb"></a>Vorheriges festlegen Listeneinträge mit CascadingDropDown (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/CascadingDropDown2VB.pdf)

> Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet. Mit ein wenig Code ist es möglich, dass ein Listenelement vorab ausgewählt wird, sobald die Daten dynamisch geladen wurden.


## <a name="overview"></a>Übersicht

Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet. (Z. B. eine Liste enthält eine Liste von US-Bundesstaaten und die folgenden Liste wird dann mit großen Städten in diesem Zustand gefüllt.) Mit ein wenig Code ist es möglich, dass ein Listenelement vorab ausgewählt wird, sobald die Daten dynamisch geladen wurden.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample1.aspx)]

Klicken Sie dann, ist ein DropDownList-Steuerelement erforderlich:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample2.aspx)]

Für diese Liste wird ein CascadingDropDown Extender Bereitstellung von Web-Dienst-URL und -Methode hinzugefügt:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample3.aspx)]

Der Extender CascadingDropDown ruft dann asynchron einen Webdienst mit der folgenden Methodensignatur:

[!code-vb[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample4.vb)]

Die Methode gibt ein Array vom Typ CascadingDropDown-Wert. Typkonstruktor erwartet zuerst den Listeneintrag Beschriftung, und klicken Sie dann den Wert (HTML `value` Attribut). Wenn das dritte Argument festgelegt ist auf "true", wird die Liste Element automatisch im Browser ausgewählt ist.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-vb/samples/sample5.aspx)]

Beim Laden der Seite im Browser füllt die Dropdown-Liste mit den drei Anbietern dem zweiten Ausdruck wird vorab ausgewählt.


[![Die Liste gefüllt und standardmäßig automatisch ausgewählt.](presetting-list-entries-with-cascadingdropdown-vb/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-vb/_static/image1.png)

Die Liste aufgefüllt und automatisch vorausgewählt ([klicken Sie hier, um das Bild in voller Größe angezeigt](presetting-list-entries-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](using-cascadingdropdown-with-a-database-vb.md)
> [Weiter](using-auto-postback-with-cascadingdropdown-vb.md)
