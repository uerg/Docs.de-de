---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: Mithilfe von AJAX Control Toolkit-Steuerelementen und Steuerelement-Extendern (VB) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie ASP.NET-Seiten AJAX Control Toolkit-Steuerelemente und Extender hinzugefügt.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: f1cf40ec3ba299ee2702258a93aa1192a2112e7f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835201"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a>Verwenden von AJAX Control Toolkit-Steuerelementen und Steuerelement-Extendern (VB)
====================
durch [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie ASP.NET-Seiten AJAX Control Toolkit-Steuerelemente und Extender hinzugefügt.


Das AJAX Control Toolkit enthält eine Reihe von Steuerelementen und Steuerelement-Extendern. In diesem kurzen Tutorial erfahren Sie, wie Steuerelemente und Extender hinzuzufügen, zu einer ASP.NET-Seite.

> [!NOTE] 
> 
> Anweisungen zum Installieren von AJAX Control Toolkit, und Hinzufügen von AJAX Control Toolkit zur Toolbox Visual Studio/Visual Web Developer, finden Sie im Tutorial [erste Schritte mit dem AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).


## <a name="using-ajax-control-toolkit-controls"></a>Mithilfe von AJAX Control Toolkit-Steuerelementen

Ein AJAX Control Toolkit-Steuerelement funktioniert genauso wie ein normaler ASP.NET-Steuerelement. Sie können das Steuerelement aus der Toolbox auf einer ASP.NET-Seite ziehen. Sie können das Steuerelement auf der Seite in der Entwurfsansicht oder die Datenquellensicht hinzufügen.

Es ist eine spezielle Anforderung, bei der Verwendung der Steuerelemente aus dem AJAX Control Toolkit. Die Seite muss ein ScriptManager-Steuerelement enthalten. Das ScriptManager-Steuerelement ist dafür verantwortlich, alle notwendigen JavaScript das AJAX Control Toolkit-Steuerelemente erforderlich ist.

Die Registerkarte "AJAX Control Toolkit" enthält beispielsweise ein Steuerelement mit dem Namen der Editor-Steuerelement. Dieses Steuerelement zeigt einen umfassenden HTML-Editor. Um das Editor-Steuerelement zu einer Seite hinzufügen, gehen Sie wie folgt vor:

1. Erstellen Sie eine neue ASP.NET-Seite mit dem Namen ShowEditor.aspx
2. Wählen Sie das ScriptManager-Steuerelement vom der Registerkarte "AJAX-Erweiterungen" in der Toolbox, und ziehen Sie das Steuerelement auf der Seite.
3. Wählen Sie die Editor-Steuerelement vom dem AJAX Control Toolkit-Registerkarte in der Toolbox, und ziehen Sie das Steuerelement auf der Seite (siehe Abbildung 1). Der Designer sollte wie in Abbildung 2 aussehen.
4. Führen Sie die Website, indem Sie durch Auswählen der Menüoption **Debuggen, Debugging starten** oder F5 drücken.
5. Die Seite in Abbildung 3 sollte angezeigt werden.


[![Wählen das HTML-Editor-Steuerelement](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)

**Abbildung 01**: Auswählen des HTML-Editor-Steuerelements ([klicken Sie, um das Bild in voller Größe anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))


[![Visual Studio-Designer mit ScriptManager "und" Bearbeiten-Steuerelement](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)

**Abbildung 02**: Visual Studio-Designer mit ScriptManager "und" Bearbeiten-Steuerelement ([klicken Sie, um das Bild in voller Größe anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))


[![Die Seite "DisplayEditor.aspx"](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)

**Abbildung 03**: die DisplayEditor.aspx-Seite ([klicken Sie, um das Bild in voller Größe anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>Mithilfe von AJAX Control Toolkit-Steuerelement-Extendern

Das AJAX Control Toolkit enthält auch die Steuerelement-Extendern. Wie der Name schon sagt, wird ein Extender-Steuerelements die Funktionalität eines vorhandenen Steuerelements erweitert. Beispielsweise erweitert einen ConfirmButton-Extender-Steuerelement das standardmäßige ASP.NET Schaltflächen-Steuerelement. Der Extender ändert das Verhalten des Schaltfläche Steuerelements s, damit die Schaltfläche ein Bestätigungsdialogfeld angezeigt, wenn Sie darauf klicken.

Ein Extender-Steuerelements, wie ein AJAX Control Toolkit-Steuerelement erfordert ein ScriptManager-Steuerelement. Sie müssen ein ScriptManager-Steuerelement zu einer Seite hinzufügen, bevor Sie Steuerelement-Extendern auf der Seite verwenden.

Um einen ConfirmButton-Extender-Steuerelement verwenden, gehen Sie wie folgt vor:

1. Erstellen Sie eine neue ASP.NET-Seite mit dem Namen ShowConfirmButton.aspx
2. Fügen Sie ein ScriptManager-Steuerelement auf der Seite durch Ziehen des Steuerelements auf der Seite vom der Registerkarte "AJAX-Erweiterungen".
3. Fügen Sie eine standardmäßige Schaltflächen-Steuerelement auf der Seite vom die Registerkarte "Standard" die Schaltfläche mit den in der Toolbox auf die Entwurfsoberfläche ziehen.
4. Klicken Sie auf die **Extender hinzufügen** aufgabenoption (siehe Abbildung 4).
5. Wählen Sie im Dialogfeld für die auswählen-Extender ConfirmButtonExtender (siehe Abbildung 5), und klicken Sie auf die Schaltfläche "OK".
6. Wählen Sie das Schaltflächen-Steuerelement im Designer, und erweitern Sie den Extender, "Button1"\_ConfirmButtonExtender-Knotens im Eigenschaftenfenster (siehe Abbildung 6). Weisen Sie den Wert *wirklich?* der ConfirmText-Eigenschaft.
7. Führen Sie die Seite im Menü die Option **Debuggen, Debugging starten** oder die F5-Taste drücken.


[![Die registerkartenoption Extender hinzufügen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)

**Abbildung 04**: Extender für das Hinzufügen von Task-Option ([klicken Sie, um das Bild in voller Größe anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))


[![Auswählen des ConfirmButton-Extenders-Steuerelements](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)

**Abbildung 05**: Auswählen des ConfirmButton-Extenders-Steuerelements ([klicken Sie, um das Bild in voller Größe anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))


[![Festlegen einer Eigenschaft ConfirmButton-Steuerelements](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)

**Abbildung 06**: Festlegen einer Eigenschaft ConfirmButton-Steuerelements ([klicken Sie, um das Bild in voller Größe anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))


Wenn die Seite geöffnet wird, sollte eine Schaltfläche angezeigt werden. Wenn Sie die Schaltfläche klicken, erhalten Sie im Bestätigungsdialogfeld in Abbildung 7.


[![Das Dialogfeld "Bestätigung" anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)

**Abbildung 07**: das Dialogfeld "Bestätigung" angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))


Beachten Sie, dass Sie normalerweise nicht den einen Extender-Steuerelements auf einer Seite ziehen. Verwenden Sie stattdessen die **Extender hinzufügen** aufgabenoption Extender an ein Steuerelement hinzufügen, die Sie bereits zu einer Seite hinzugefügt haben. Beachten Sie außerdem, dass Sie Control Extender-Eigenschaften festzulegen, öffnen das Eigenschaftenblatt für das Steuerelement erweitert wird.

Ein einzelnes ASP.NET-Steuerelement kann durch mehrere Steuerelement-Extendern erweitert werden. Das Eigenschaftenblatt für das Steuerelement erweitert wird, werden alle der Steuerelement-Extendern, die dem Steuerelement zugeordnete aufgelistet.

> [!div class="step-by-step"]
> [Zurück](get-started-with-the-ajax-control-toolkit-vb.md)
> [Weiter](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)
