---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Der Farbwähler Extendersteuerelement (VB) mit | Microsoft Docs
author: microsoft
description: Farbwähler wird ein ASP.NET AJAX-Extender, der die clientseitige Farbe Kommissionierung Funktionalität mit Benutzeroberfläche in einem Popupsteuerelement bereitstellt. Es kann ASP.NET hinzugefügt werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 3411119f85d7f5c26703b7df40cff24fdf30b81d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874517"
---
<a name="using-the-colorpicker-control-extender-vb"></a>Mithilfe der Farbwähler Extendersteuerelement (VB)
====================
durch [Microsoft](https://github.com/microsoft)

> Farbwähler wird ein ASP.NET AJAX-Extender, der die clientseitige Farbe Kommissionierung Funktionalität mit Benutzeroberfläche in einem Popupsteuerelement bereitstellt. Sie können auf jedes ASP.NET TextBox-Steuerelement angefügt werden. Es ist.


Ziel dieses Lernprogramms wird erläutert, wie Sie die AJAX-Steuerelement-Toolkit Farbwähler Extendersteuerelement verwenden können. Der Farbwähler Extendersteuerelement zeigt ein Popupdialogfeld, das Ihnen ermöglicht, eine Farbe auszuwählen. Der Farbwähler ist hilfreich, wenn Sie eine intuitive Benutzeroberfläche für einen Benutzer auf eine Farbe auswählen bereitstellen möchten.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Ein TextBox-Steuerelement mit dem Farbwähler Extendersteuerelement erweitern

Stellen Sie sich vor, z. B., dass eine Website zu erstellen, die zum Erstellen von benutzerdefinierten Visitenkarten Besucher ermöglicht werden soll. Besucher können Geben Sie den Text für eine Business-Karte und die Farbe auswählen. Die ASP.NET-Seite in Codebeispiel 1 enthält zwei TextBox-Steuerelemente, die mit dem Namen TxtCardText und TxtCardColor. Wenn Sie bei der Übermittlung des Formulars werden die ausgewählten Werte angezeigt (siehe Abbildung 1).


[![Einfaches Formular zum Erstellen einer Business-Karte](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)

**Abbildung 01**: einfaches Formular zum Erstellen einer Visitenkarte ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-colorpicker-control-extender-vb/_static/image2.png))


**1 – CreateCard.aspx auflisten**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

Das Formular im Codebeispiel 1 funktioniert, aber es bietet eine hervorragende Benutzeroberfläche keine. Der Benutzer hat eine Farbe in das Textfeld eingeben. Wenn der Benutzer eine spezielle Farbe - möchte muss z. B. nur die rechte Schattierung Pea Grün- und der Benutzer die HTML-Farbcode ohne keine Hilfe herauszufinden.

Der Farbwähler Extendersteuerelement können Sie um eine bessere benutzererfahrung zu erstellen. Farbwähler wird ein Dialogfeld Farbe angezeigt, wenn Sie den Fokus auf ein TextBox-Steuerelement verschieben (siehe Abbildung 2).


[![Der Farbwähler Extendersteuerelement](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)

**Abbildung 02**: der Farbwähler Extendersteuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-colorpicker-control-extender-vb/_static/image4.png))


Sie müssen zwei Schritte ausführen, um das Extendersteuerelement Farbwähler mit dem Formular im Codebeispiel 1 verwenden:

1. Fügen Sie ein ScriptManager-Steuerelement auf der Seite "
2. Der Farbwähler Extendersteuerelement auf der Seite hinzufügen

Bevor Sie Farbwähler verwenden können, müssen Sie ein ScriptManager auf der Seite hinzufügen. Ein guter Ausgangspunkt ScriptManager hinzufügen wird rechts unterhalb der öffnenden serverseitige &lt;Formular&gt; Tag. Sie können die ScriptManager auf der Seite "(ein ScriptManager befindet sich unter der Registerkarte" AJAX-Erweiterungen ") aus der Toolbox ziehen. Alternativ können Sie das folgende Tag in der Datenquellensicht unter serverseitige Formular Starttag eingeben:

&lt;ASP: ScriptManager-ID = "ScriptManager1" Runat = "Server" /&gt;

Die einfachste Möglichkeit, das Extendersteuerelement Farbwähler zur Seite hinzuzufügen, ist in der Entwurfsansicht. Wenn Sie Ihre Maus auf das Textfeld TxtCardColor zeigen, eine Automatisches Option wird angezeigt, das ermöglicht Sie keinen Extender hinzufügen (siehe Abbildung 3). Wenn Sie diese Option auswählen, erscheint der Extender-Assistent (siehe Abbildung 4).


[![Einen Extender hinzufügen](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)

**Abbildung 03**: Hinzufügen eines Extenders ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-colorpicker-control-extender-vb/_static/image6.png))


[![Ein Extendersteuerelement mit dem Assistenten für Extender auswählen](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)

**Abbildung 04**: ein Extendersteuerelement mit dem Assistenten für Extender auswählen ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-colorpicker-control-extender-vb/_static/image8.png))


Sie können den Farbwähler Extender zum Erweitern der TxtCardColor Textfeld mit dem Farbwähler Extender auswählen. Klicken Sie auf OK, um das Dialogfeld zu schließen.

Nachdem Sie diese Änderungen vorgenommen haben, sieht die Quelle für die Seite 2 aufgelistet.

**Auflisten von 2 – CreateCard.aspx (mit Farbwähler)**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

Beachten Sie, dass die Seite jetzt ein ColorPickerExtender-Steuerelement enthält, die direkt unterhalb der TxtCardColor TextBox-Steuerelement angezeigt wird. Das Steuerelement ColorPickerExtender erweitert TxtCardColor-Steuerelement, sodass sie ein Dialogfeld für die Farbauswahl angezeigt.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Verwenden einer Schaltfläche, um das Dialogfeld für die Farbauswahl zu starten.

Der Farbwähler Extender unterstützt die folgenden Eigenschaften:

- PopupButtonId - die ID einer Schaltfläche auf der Seite, die bewirkt, dass das Dialogfeld für die Farbauswahl angezeigt werden.
- PopupPosition - Position relativ zum Zielsteuerelement, der das Dialogfeld für die Farbauswahl. Mögliche Werte sind Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, rechts und Links (der Standardwert ist BottomLeft).
- SampleControlId - die ID eines Steuerelements, in der ausgewählte Farbe angezeigt.
- SelectedColor - Ausgangsfarbe, die vom Farbwähler ausgewählt.

Diese Eigenschaften können Sie anpassen, wie das Dialogfeld für die Farbauswahl angezeigt wird und wie die ausgewählte Farbe angezeigt wird. Die Seite im Codebeispiel 3 wird veranschaulicht, wie Sie einige dieser Eigenschaften.

**3 – CreateCardButton.aspx auflisten**

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

Die Seite im Codebeispiel 3 enthält eine Farbe auswählen Schaltfläche (siehe Abbildung 5). Wenn Sie auf diese Schaltfläche klicken, wird das Dialogfeld für die Farbauswahl über das Textfeld angezeigt. Wenn Sie eine Farbe aus dem Dialogfeld auswählen wird die ausgewählte Farbe als die Farbe des Hintergrunds der LblSample Label-Steuerelement angezeigt.

Die ColorPicker PopupButtonID-Eigenschaft wird verwendet, die Schaltfläche "Farbe auswählen" den Farbwähler Extender zugeordnet werden soll. Wenn Sie einen Wert für die PopupButtonID-Eigenschaft angeben, wird das Dialogfeld für die Farbauswahl nicht mehr angezeigt, wenn das Zielsteuerelement den Fokus besitzt. Sie müssen die Schaltfläche, um das Dialogfeld anzeigen klicken.

Die SampleControlID-Eigenschaft wird verwendet, um ein Steuerelement zuzuordnen, in dem die ausgewählte Farbe mit Farbwähler angezeigt. Der Farbwähler ändert die Hintergrundfarbe des Steuerelements, die zurzeit ausgewählte Farbe an.


[![Das Dialogfeld für die Farbauswahl mit einer Schaltfläche anzeigen](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)

**Abbildung 05**: das Dialogfeld für die Farbauswahl mit einer Schaltfläche anzeigen ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-colorpicker-control-extender-vb/_static/image10.png))


## <a name="summary"></a>Zusammenfassung

In diesem Lernprogramm haben Sie gelernt, wie das Extendersteuerelement Farbwähler zu verwenden, um ein Dialogfeld für die Farbauswahl Popup anzuzeigen. Zunächst können wir untersucht, wie Sie das Dialogfeld anzeigen, wenn ein TextBox-Steuerelement den Fokus verschoben wird. Als Nächstes haben Sie gelernt, wie Sie eine Schaltfläche zu erstellen, die in dem Dialogfeld für die Farbauswahl angezeigt, auf die Schaltfläche geklickt wird.

> [!div class="step-by-step"]
> [Vorherige](using-the-colorpicker-control-extender-cs.md)
