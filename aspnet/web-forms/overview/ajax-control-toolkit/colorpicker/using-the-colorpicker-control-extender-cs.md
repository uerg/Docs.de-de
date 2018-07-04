---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
title: Mithilfe des ColorPicker-Steuerelement-Extenders (c#) | Microsoft-Dokumentation
author: microsoft
description: ColorPicker ist ein ASP.NET AJAX-Extender, der clientseitige Farbe auswählen Funktionalität mit Benutzeroberfläche in einem Popupsteuerelement enthält. Sie können jeder ASP.NET angefügt werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0d86a1e7-a910-4ab2-b85c-7a9ea6906c39
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: f20928099e2b4db477705cd1634fd28745a328ac
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383903"
---
<a name="using-the-colorpicker-control-extender-c"></a>Mithilfe des ColorPicker-Steuerelement-Extenders (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> ColorPicker ist ein ASP.NET AJAX-Extender, der clientseitige Farbe auswählen Funktionalität mit Benutzeroberfläche in einem Popupsteuerelement enthält. Sie können auf jedes ASP.NET TextBox-Steuerelement angefügt werden. Es ist.


Das Ziel in diesem Tutorial wird beschrieben, wie Sie den AJAX Control Toolkit ColorPicker-Steuerelement-Extender verwenden können. ColorPicker-Steuerelement-Extender zeigt ein Popupdialogfeld, das Ihnen ermöglicht, eine Farbe auszuwählen. Die ColorPicker ist nützlich, wenn Sie eine intuitive Benutzeroberfläche, damit ein Benutzer eine Farbe auswählen bereitstellen möchten.

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a>Ein TextBox-Steuerelement mit dem ColorPicker-Steuerelement-Extender erweitern

Stellen Sie sich vor, z. B., dass eine Website zu erstellen, die Besucher zum Erstellen von benutzerdefinierten Visitenkarten ermöglicht werden soll. Besucher können Geben Sie den Text für eine Visitenkarte und die Farbe auswählen. Die ASP.NET-Seite in Codebeispiel 1 enthält zwei TextBox-Steuerelemente, die mit dem Namen TxtCardText und TxtCardColor. Wenn Sie das Formular übermitteln, werden die ausgewählten Werte angezeigt (siehe Abbildung 1).


[![Einfaches Formular zum Erstellen einer Visitenkarte](using-the-colorpicker-control-extender-cs/_static/image1.jpg)](using-the-colorpicker-control-extender-cs/_static/image1.png)

**Abbildung 01**: einfache Formular zum Erstellen einer Visitenkarte ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-colorpicker-control-extender-cs/_static/image2.png))


**1 – CreateCard.aspx auflisten**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample1.aspx)]

Das Formular in Codebeispiel 1 funktioniert, aber es bietet eine hervorragende benutzerumgebung keine. Der Benutzer hat eine Farbe in das Textfeld eingeben. Wenn der Benutzer eine spezielle Farbe - will muss z. B. nur die richtige Schattierung des hauptzeit-Grün- und der Benutzer den HTML-Farbcode ohne Hilfe ermitteln.

Sie können die ColorPicker-Steuerelement-Extender verwenden, um eine bessere benutzererfahrung zu erstellen. Farbwähler zeigt ein Dialogfeld Farbe aus, wenn auf ein Textfeldsteuerelement den Fokus zu verschieben (siehe Abbildung 2).


[![Der ColorPicker-Steuerelement-Extender](using-the-colorpicker-control-extender-cs/_static/image2.jpg)](using-the-colorpicker-control-extender-cs/_static/image3.png)

**Abbildung 02**: die ColorPicker-Steuerelement-Extender ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-colorpicker-control-extender-cs/_static/image4.png))


Sie müssen zwei Schritte ausführen, um die ColorPicker-Steuerelement-Extender verwenden, mit dem Formular in Codebeispiel 1:

1. Ein ScriptManager-Steuerelement auf der Seite hinzufügen
2. ColorPicker-Steuerelement-Extender zur Seite hinzufügen

Bevor Sie die ColorPicker-Komponente verwenden können, müssen Sie ein ScriptManager-Steuerelement auf Ihrer Seite hinzufügen. Eine gute Möglichkeit zum Hinzufügen von ScriptManager wird rechts neben das öffnendes serverseitige &lt;Formular&gt; Tag. Sie können den ScriptManager auf der Seite aus der Toolbox ziehen (ein ScriptManager befindet sich unter der Registerkarte "AJAX-Erweiterungen"). Alternativ können Sie das folgende Tag in die Datenquellensicht unter dem Starttag des serverseitigen Format eingeben:

&lt;ASP: ScriptManager-ID = "ScriptManager1" Runat = "Server" /&gt;

Die einfachste Möglichkeit zum Hinzufügen des ColorPicker-Extenders-Steuerelements auf der Seite ist in der Entwurfsansicht. Wenn Sie den Mauszeiger auf das Textfeld TxtCardColor zeigen, eine Smarttask Option wird angezeigt, der ermöglicht es Ihnen, einen Extender hinzuzufügen (siehe Abbildung 3). Wenn Sie diese Option auswählen, die Extender-Assistent (siehe Abbildung 4) wird angezeigt.


[![Hinzufügen eines Extenders](using-the-colorpicker-control-extender-cs/_static/image3.jpg)](using-the-colorpicker-control-extender-cs/_static/image5.png)

**Abbildung 03**: Hinzufügen eines Extenders ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-colorpicker-control-extender-cs/_static/image6.png))


[![Einen Extender-Steuerelements mit dem Assistenten für Extender auswählen](using-the-colorpicker-control-extender-cs/_static/image4.jpg)](using-the-colorpicker-control-extender-cs/_static/image7.png)

**Abbildung 04**: Auswählen eines mit dem Assistenten für Extender-Steuerelement-Extenders ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-colorpicker-control-extender-cs/_static/image8.png))


Sie können den ColorPicker-Extender erweitern die TxtCardColor Textfeld mit dem ColorPicker-Extender auswählen. Klicken Sie auf OK, um das Dialogfeld zu schließen.

Nachdem Sie diese Änderungen vorgenommen haben, sieht die Quelle für die Seite Codebeispiel 2.

Codebeispiel 2 - CreateCard.aspx (mit ColorPicker)

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample2.aspx)]

Beachten Sie, dass die Seite jetzt ein ColorPickerExtender-Steuerelement enthält, die direkt unterhalb der TxtCardColor TextBox-Steuerelement angezeigt wird. Das Steuerelement ColorPickerExtender erweitert das Steuerelement TxtCardColor, damit sie ein Dialogfeld für die Farbauswahl angezeigt.

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a>Mithilfe einer Schaltfläche, um das Dialogfeld für die Farbauswahl zu starten.

Der ColorPicker-Extender unterstützt die folgenden Eigenschaften:

- PopupButtonId - die ID einer Schaltfläche auf der Seite, die bewirkt, dass das Dialogfeld für die Farbauswahl angezeigt werden.
- PopupPosition - die Position relativ zum Zielsteuerelement, der das Dialogfeld für die Farbauswahl. Mögliche Werte sind absolut, Center, BottomLeft, BottomRight, TopLeft, oberen, rechten, und Links (der Standardwert ist BottomLeft).
- SampleControlId: die ID eines Steuerelements, in der ausgewählte Farbe angezeigt.
- SelectedColor - die Ausgangsfarbe, die durch die ColorPicker ausgewählt.

Sie können diese Eigenschaften verwenden, um festzulegen, wie das Dialogfeld für die Farbauswahl angezeigt wird und wie die ausgewählte Farbe angezeigt wird. Die Seite in Codebeispiel 3 wird veranschaulicht, wie Sie einige dieser Eigenschaften verwenden können.

**Codebeispiel 3 - CreateCardButton.aspx**

[!code-aspx[Main](using-the-colorpicker-control-extender-cs/samples/sample3.aspx)]

Die Seite in Programmausdruck 3 enthält eine Farbe auswählen Schaltfläche (siehe Abbildung 5). Wenn Sie diese Schaltfläche klicken, wird das Dialogfeld für die Farbauswahl angezeigt, über das Textfeld ein. Bei Auswahl eine Farbe aus dem Dialogfeld wird die ausgewählte Farbe als Farbe des Hintergrunds der LblSample Label-Steuerelement angezeigt.

Die ColorPicker PopupButtonID-Eigenschaft wird verwendet, der ColorPicker-Extender die Schaltfläche "Farbe auswählen" zugeordnet werden soll. Wenn Sie einen Wert für die PopupButtonID-Eigenschaft angeben, wird das Dialogfeld für die Farbauswahl nicht mehr angezeigt, wenn das Zielsteuerelement den Fokus besitzt. Sie müssen die Schaltfläche, um die Anzeige des Dialogfelds klicken.

Die SampleControlID-Eigenschaft wird verwendet, um ein Steuerelement zuzuordnen, die die ausgewählte Farbe mit der ColorPicker anzeigt. Die ColorPicker ändert die Hintergrundfarbe des Steuerelements, die zurzeit ausgewählte Farbe an.


[![Das Dialogfeld für die Farbauswahl mit einer Schaltfläche anzeigen](using-the-colorpicker-control-extender-cs/_static/image5.jpg)](using-the-colorpicker-control-extender-cs/_static/image9.png)

**Abbildung 05**: das Dialogfeld mit einer Schaltfläche für die Farbauswahl angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-colorpicker-control-extender-cs/_static/image10.png))


## <a name="summary"></a>Zusammenfassung

In diesem Tutorial haben Sie gelernt, wie die ColorPicker-Steuerelement-Extender zu verwenden, um ein Dialogfeld für die Farbauswahl Popup anzuzeigen. Zunächst können wir untersucht, wie Sie das Dialogfeld anzeigen können, wenn der Fokus in einem TextBox-Steuerelement verschoben wird. Als Nächstes, wie Sie eine Schaltfläche zu erstellen, in dem das Dialogfeld für die Farbauswahl angezeigt, wenn die Schaltfläche geklickt wird.

> [!div class="step-by-step"]
> [Nächste](using-the-colorpicker-control-extender-vb.md)
