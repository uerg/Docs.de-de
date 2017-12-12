---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
title: "Füllen eine Liste mit CascadingDropDown (c#) | Microsoft Docs"
author: wenz
description: "Das CascadingDropDown-Steuerelement in das AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, damit, dass Änderungen in einer DropDownList lädt Werte in Anoth verknüpft..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f949aafa-fe57-43b0-b722-f0dd33a900be
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-cs
msc.type: authoredcontent
ms.openlocfilehash: e5e0f11a815632aff9e17dc0f783f7eba2753995
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="filling-a-list-using-cascadingdropdown-c"></a>Füllen eine Liste mit CascadingDropDown (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0CS.pdf)

> Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet. (Z. B. eine Liste enthält eine Liste von US-Bundesstaaten und die folgenden Liste wird dann mit großen Städten in diesem Zustand gefüllt.) Die erste Abfrage zur Lösung ist, tatsächlich eine Dropdownliste mit diesem Steuerelement geben.


## <a name="overview"></a>Übersicht

Das Steuerelement CascadingDropDown im AJAX-Steuerelement-Toolkit erweitert ein DropDownList-Steuerelements, sodass Änderungen in einer DropDownList lädt Werten in einer anderen DropDownList zugeordnet. (Z. B. eine Liste enthält eine Liste von US-Bundesstaaten und die folgenden Liste wird dann mit großen Städten in diesem Zustand gefüllt.) Die erste Abfrage zur Lösung ist, tatsächlich eine Dropdownliste mit diesem Steuerelement geben.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample1.aspx)]

Klicken Sie dann, ist ein DropDownList-Steuerelement erforderlich:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample2.aspx)]

Für diese Liste wird ein CascadingDropDown Extender hinzugefügt. Eine asynchrone Anforderung sendet an einen Webdienst dann eine Liste von Einträgen, die in der Liste angezeigt werden zurückgegeben wird. Damit dies funktioniert müssen die folgenden CascadingDropDown Attribute festgelegt werden:

- `ServicePath`: Die URL eines Webdiensts, die Übermittlung von Einträge in der Liste
- `ServiceMethod`: Einträge in der Liste zu übermitteln Web-Methode
- `TargetControlID`: Die ID der Dropdownliste
- `Category`: Kategorie Informationen, die beim Aufruf der Web-Methode übermittelt wird
- `PromptText`: Der Text angezeigt, wenn Sie Daten asynchron vom Server zu laden

Hier wird das Markup für die `CascadingDropDown` Element. Der einzige Unterschied zwischen c# und VB ist der Name des Webdiensts verknüpft:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample3.aspx)]

Der JavaScript-Code, der von der `CascadingDropDown` Extender Ruft eine Webdienstmethode mit der folgenden Signatur:

[!code-csharp[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample4.cs)]

Damit der wichtigste Aspekt ist, dass die Methode ein Array vom Typ zurückgeben muss `CascadingDropDownNameValue` (definiert durch das ASP.NET AJAX-Steuerelement-Toolkit). In der `CascadingDropDownNameValue` Konstruktor zuerst den Listeneintrag Text, und klicken Sie dann ihren Wert angegeben werden müssen, ebenso wie `<option value="VALUE">NAME</option>` in HTML führen würde. Hier sind einige Beispieldaten:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-cs/samples/sample5.aspx)]

Beim Laden der Seite im Browser wird die Liste mit den drei Anbietern gefüllt werden ausgelöst.


[![Die Liste wird automatisch ausgefüllt.](filling-a-list-using-cascadingdropdown-cs/_static/image2.png)](filling-a-list-using-cascadingdropdown-cs/_static/image1.png)

Die Liste wird automatisch ausgefüllt, ([klicken Sie hier, um das Bild in voller Größe angezeigt](filling-a-list-using-cascadingdropdown-cs/_static/image3.png))

>[!div class="step-by-step"]
[Nächste](using-cascadingdropdown-with-a-database-cs.md)
