---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
title: Verwenden von AJAX-Steuerelement-Toolkit-Steuerelemente und -Extender (c#) | Microsoft Docs
author: microsoft
description: Erfahren Sie, wie ASP.NET-Seiten AJAX-Steuerelement-Toolkit-Steuerelemente und Extender hinzugefügt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: c1e6b51c-3bc3-4bf7-9916-9991197af3dd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-cs
msc.type: authoredcontent
ms.openlocfilehash: 3d7cea2452db01ca116849ffb17631db3b935668
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873494"
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-c"></a>Verwenden von AJAX-Steuerelement-Toolkit-Steuerelemente und -Extender (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie ASP.NET-Seiten AJAX-Steuerelement-Toolkit-Steuerelemente und Extender hinzugefügt.


Das AJAX-Steuerelement-Toolkit enthält einen Satz von Steuerelementen und -Extender. In diesem kurzen Tutorial lernen Sie zum Hinzufügen von Steuerelementen und -Extender zu einer ASP.NET-Seite.

> [!NOTE] 
> 
> Für Anweisungen zum Installieren des AJAX-Steuerelement-Toolkit und Hinzufügen von AJAX-Steuerelement-Toolkit zur Visual Studio/Visual Web Developer-Toolbox finden Sie im Lernprogramm [erste Schritte mit AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-cs.md).


## <a name="using-ajax-control-toolkit-controls"></a>Verwenden von AJAX-Steuerelement-Toolkit-Steuerelemente

Ein AJAX-Steuerelement-Toolkit-Steuerelement funktioniert genauso wie eine normale ASP.NET-Steuerelements. Sie können das Steuerelement aus der Toolbox auf einer ASP.NET-Seite ziehen. Sie können das Steuerelement auf der Seite in der Entwurfsansicht oder Datenquellensicht hinzufügen.

Es ist eine spezielle Anforderung bei Verwendung der Steuerelemente aus dem AJAX-Steuerelement-Toolkit. Die Seite muss ein ScriptManager-Steuerelement enthalten. Die ScriptManager-Steuerelement ist verantwortlich für einschließlich sämtlicher in der die erforderlichen JavaScript-Code für die AJAX-Steuerelement-Toolkit-Steuerelemente erforderlich.

Die Registerkarte "AJAX Control Toolkit" enthält beispielsweise ein Steuerelement mit dem Namen der Editor-Steuerelement. Dieses Steuerelement zeigt einen reichhaltigen HTML-Editor. Führen Sie diese Schritte aus, um die Editor-Steuerelement zu einer Seite hinzufügen:

1. Erstellen Sie eine neue, mit dem Namen ShowEditor.aspx ASP.NET-Seite
2. Wählen Sie das ScriptManager-Steuerelement vom der Registerkarte "AJAX-Erweiterungen" in der Toolbox, und ziehen Sie das Steuerelement auf der Seite.
3. Wählen Sie die Editor-Steuerelement vom die AJAX-Steuerelement-Toolkit-Registerkarte in der Toolbox, und ziehen Sie das Steuerelement auf der Seite (siehe Abbildung 1). Der Designer sollte wie in Abbildung 2 aussehen.
4. Führen Sie die Website im Menü die Option **Debuggen, Debugging starten** oder die F5-Taste drücken.
5. Die Seite in Abbildung 3 sollte angezeigt werden.


[![Auswählen des HTML-Editor-Steuerelements](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image1.png)

**Abbildung 01**: Auswählen der HTML-Editor-Steuerelements ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.png))


[![Visual Studio-Designer mit ScriptManager und Edit-Steuerelement](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.png)

**Abbildung 02**: Visual Studio-Designer mit ScriptManager und Edit-Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.png))


[![Die Seite "DisplayEditor.aspx"](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.png)

**Abbildung 03**: die DisplayEditor.aspx-Seite ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.png))


## <a name="using-ajax-control-toolkit-control-extenders"></a>AJAX Extender-Steuerelement-Toolkit-

Das AJAX-Steuerelement-Toolkit enthält auch-Extender. Wie der Name andeutet, reicht ein Extendersteuerelement die Funktionalität von einem vorhandenen Steuerelement. Das Extendersteuerelement ConfirmButton erstreckt sich beispielsweise das standardmäßige ASP.NET Schaltflächen-Steuerelement aus. Der Extender ändert das Steuerelement s Verhalten, damit die Schaltfläche ein Bestätigungsdialogfeld angezeigt, wenn Sie darauf klicken.

Ein Extendersteuerelement, genau wie ein AJAX-Steuerelement-Toolkit-Steuerelement erfordert ein ScriptManager-Steuerelement. Sie müssen ein ScriptManager-Steuerelement zu einer Seite hinzufügen, bevor Sie beginnen mit-Extender auf der Seite.

Führen Sie diese Schritte aus, um das Extendersteuerelement ConfirmButton verwenden:

1. Erstellen Sie eine neue, mit dem Namen ShowConfirmButton.aspx ASP.NET-Seite
2. Fügen Sie ein ScriptManager-Steuerelement durch Ziehen des Steuerelements auf der Seite vom der Registerkarte "AJAX-Erweiterungen" auf der Seite hinzu.
3. Fügen Sie ein standard-Schaltflächen-Steuerelement auf der Seite durch die Schaltfläche vom die Registerkarte "Standard" in der Toolbox auf die Entwurfsoberfläche ziehen.
4. Klicken Sie auf die **Extender hinzufügen** Aufgabe Option (siehe Abbildung 4).
5. Wählen Sie im Dialogfeld Extender auswählen ConfirmButtonExtender (siehe Abbildung 5), und klicken Sie auf die Schaltfläche "OK".
6. Wählen Sie im Designer das Schaltflächen-Steuerelement, und erweitern Sie den Extender Button1\_ConfirmButtonExtender Knoten im Eigenschaftenfenster angezeigt (siehe Abbildung 6). Weisen Sie den Wert *wirklich?* der ConfirmText-Eigenschaft.
7. Führen Sie die Seite, indem Sie im Menü die Option **Debuggen, Debugging starten** oder drücken Sie die F5-Taste.


[![Die Task-Option Extender hinzufügen](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.png)

**Abbildung 04**: Extender für das Hinzufügen von Task-Option ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image8.png))


[![Das Extendersteuerelement ConfirmButton auswählen](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image9.png)

**Abbildung 05**: Auswählen der ConfirmButton Extendersteuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image10.png))


[![Festlegen einer Eigenschaft ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image11.png)

**Abbildung 06**: Festlegen einer Eigenschaft ConfirmButton ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image12.png))


Wenn die Seite geöffnet wird, sollte eine Schaltfläche angezeigt werden. Wenn Sie die Schaltfläche klicken, erhalten Sie im Bestätigungsdialogfeld in Abbildung 7.


[![Bestätigungsdialogfeld anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image13.png)

**Abbildung 07**: Anzeigen von Bestätigungsdialogfeld ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-ajax-control-toolkit-controls-and-control-extenders-cs/_static/image14.png))


Beachten Sie, dass Sie nicht normalerweise ein Extendersteuerelement auf einer Seite ziehen. Verwenden Sie stattdessen die **Extender hinzufügen** Aufgabe Option aus, um einen Extender an ein Steuerelement hinzufügen, die Sie bereits zu einer Seite hinzugefügt haben. Beachten Sie außerdem, dass Sie Steuerelement Extendereigenschaften durch Öffnen der Eigenschaftenseite für das Steuerelement zu vergrößernden festzulegen.

Eine einzelne ASP.NET-Steuerelements kann von mehreren-Extender erweitert werden. Das Eigenschaftenblatt für das Steuerelement zu vergrößernden Listet alle Extender Steuerelement dem Steuerelement zugeordnet.

> [!div class="step-by-step"]
> [Zurück](get-started-with-the-ajax-control-toolkit-cs.md)
> [Weiter](creating-a-custom-ajax-control-toolkit-control-extender-cs.md)
