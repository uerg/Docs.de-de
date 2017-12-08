---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Verwenden HoverMenu mit einem Wiederholungsmodul-Steuerelement (VB) | Microsoft Docs
author: wenz
description: "Das Steuerelement HoverMenu im AJAX-Steuerelement-Toolkit bietet einen einfachen Popup-Effekt: Wenn der Mauszeiger über ein Element bewegt wird, ein Popupfenster angezeigt wird, an einer speziell..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 77759afe00f341e15ee8e0f52a469d3b7c08ea89
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a>Verwenden HoverMenu mit einem Wiederholungsmodul-Steuerelement (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)

> Das Steuerelement HoverMenu im AJAX-Steuerelement-Toolkit bietet einen einfachen Popup-Effekt: Wenn der Mauszeiger über ein Element bewegt wird, ein Popupfenster angezeigt wird, an einer angegebenen Position. Es ist auch möglich, dieses Steuerelement in ein Repeater zu verwenden.


## <a name="overview"></a>Übersicht

Die `HoverMenu` Steuerelement im AJAX-Steuerelement-Toolkit stellt eine einfache Popup Auswirkung: Wenn der Mauszeiger über ein Element bewegt wird, ein Popupfenster angezeigt wird, an einer angegebenen Position. Es ist auch möglich, dieses Steuerelement in ein Repeater zu verwenden.

## <a name="steps"></a>Schritte

Erstens ist eine Datenquelle erforderlich. Dieses Beispiel verwendet die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition. Die Datenbank ist ein optionaler Teil einer Visual Studio-Installation (einschließlich express Edition) und steht auch als separater Download unter [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Die AdventureWorks-Datenbank ist Teil der SQL Server 2005 Samples and Sample Databases (download [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung der Microsoft SQL Server Management Studio Express ([Https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)), und fügen die `AdventureWorks.mdf` Datenbankdatei.

In diesem Beispiel gehen wir davon aus, dass die Instanz von SQL Server 2005 Express Edition aufgerufen wird `SQLEXPRESS` und befindet sich auf dem gleichen Computer wie der Webserver; Dies ist auch die Standardeinstellungen. Wenn Ihr Setup abweicht, müssen Sie passen Sie die Verbindungsinformationen für die Datenbank.

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

Fügen Sie dann eine Datenquelle auf der Seite "ein. Um eine begrenzte Menge von Daten zu verwenden, wählen wir nur die ersten fünf Einträge in der Vendor-Tabelle der AdventureWorks-Datenbank. Wenn Sie die Visual Studio-Assistent zum Erstellen der Datenquelle verwenden, beachten Sie, dass ein Fehler in der aktuellen Version nicht den Tabellennamen Präfixlänge ist (`Vendor`) mit `Purchasing`. Das folgende Markup zeigt die richtige Syntax:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

Als Nächstes fügen Sie ein Panel die als modales Popupdialogfeld dient:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

Jetzt die `HoverMenuExtender` , kommt es zu. So, dass jedes Element in der Datenquelle einen eigenen Popup erhält, muss der Extender versetzt werden, innerhalb des wiederholungsmoduls `<ItemTemplate>` Abschnitt. Dies ist das Markup:

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

Jetzt jedes Element in der Datenquelle ein Popups auf der rechten Seite zeigt (`PopupPosition` Attribut) nach einer Verzögerung von 50 Millisekunden (`PopDelay` Attribut).


[![Menü wird neben jedem Element im wiederholungsmodul angezeigt.](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)

Menü angezeigt wird, neben jedem Element im wiederholungsmodul ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))

>[!div class="step-by-step"]
[Zurück](using-hovermenu-with-a-repeater-control-cs.md)
