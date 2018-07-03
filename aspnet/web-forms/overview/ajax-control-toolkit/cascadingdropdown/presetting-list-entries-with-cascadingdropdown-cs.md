---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
title: Von Listeneinträgen mit CascadingDropDown (c#) | Microsoft-Dokumentation
author: wenz
description: Das Steuerelement "CascadingDropDown" im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList lädt Werte in Anoth verknüpft...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 04c79748-0f21-4a3b-aba5-e1ce3161c32e
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/presetting-list-entries-with-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: 56093ad098d69039aafa9b4e6ba4e4c2ad91e49b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37361998"
---
<a name="presetting-list-entries-with-cascadingdropdown-c"></a>Von Listeneinträgen mit CascadingDropDown (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown2.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingDropDown2CS.pdf)

> Das Steuerelement "CascadingDropDown" im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList lädt Werte in einer anderen DropDownList verknüpft ist. Mit ein wenig Code ist es möglich, dass ein Listenelement vorab ausgewählt wird, sobald die Daten dynamisch geladen wurden.


## <a name="overview"></a>Übersicht

Das Steuerelement "CascadingDropDown" im AJAX Control Toolkit erweitert ein DropDownList-Steuerelement, sodass Änderungen in einer DropDownList lädt Werte in einer anderen DropDownList verknüpft ist. (Z. B. eine Liste enthält eine Liste der US-Staaten und die folgenden Liste wird dann mit der größten Städte in diesem Zustand gefüllt.) Mit ein wenig Code ist es möglich, dass ein Listenelement vorab ausgewählt wird, sobald die Daten dynamisch geladen wurden.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit, aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample1.aspx)]

Anschließend ist ein DropDownList-Steuerelement erforderlich:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample2.aspx)]

Für diese Liste wird ein CascadingDropDown-Extender Bereitstellen von Informationen von Web-Dienst-URL und -Methode hinzugefügt:

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample3.aspx)]

Der CascadingDropDown-Extender wird zwischenrückruf dann ein Webdienst mit der folgenden Methodensignatur:

[!code-csharp[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample4.cs)]

Die Methode gibt ein Array vom Typ CascadingDropDown-Wert zurück. Der Konstruktor des Typs erwartet, dass zuerst den Listeneintrag Beschriftung, und klicken Sie dann den Wert (HTML `value` Attribut). Wenn das dritte Argument festgelegt wird auf "true", die Liste Element automatisch im Browser ausgewählt ist.

[!code-aspx[Main](presetting-list-entries-with-cascadingdropdown-cs/samples/sample5.aspx)]

Beim Laden der Seite im Browser füllt die Dropdown-Liste mit drei Anbietern zusammen, das zweite Argument wird standardmäßig ausgewählt.


[![Die Liste gefüllt ist, und standardmäßig automatisch ausgewählt.](presetting-list-entries-with-cascadingdropdown-cs/_static/image2.png)](presetting-list-entries-with-cascadingdropdown-cs/_static/image1.png)

Die Liste gefüllt und standardmäßig automatisch ausgewählt wird ([klicken Sie, um das Bild in voller Größe anzeigen](presetting-list-entries-with-cascadingdropdown-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](using-cascadingdropdown-with-a-database-cs.md)
> [Weiter](using-auto-postback-with-cascadingdropdown-cs.md)
