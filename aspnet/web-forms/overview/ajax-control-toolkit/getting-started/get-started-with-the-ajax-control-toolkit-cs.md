---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Erste Schritte mit dem AJAX Control Toolkit (c#) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, Sie müssen wissen, erste Schritte mit dem AJAX Control Toolkit.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: 6ecf716b78a789ca72e8b35e0be3e1fd0b957052
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835800"
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a>Erste Schritte mit dem AJAX Control Toolkit (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> Erfahren Sie, Sie müssen wissen, erste Schritte mit dem AJAX Control Toolkit.


Das AJAX Control Toolkit enthält mehr als 30 kostenlosen Steuerelemente, die Sie in Ihren ASP.NET-Anwendungen verwenden können. In diesem Tutorial erfahren Sie, wie das AJAX Control Toolkit herunterladen und Ihre Toolbox Visual Studio/Visual Web Developer Express Toolkit-Steuerelementen hinzufügen.

## <a name="downloading-the-ajax-control-toolkit"></a>Das AJAX Control Toolkit herunterladen

Die [AJAX Control Toolkit](http://devexpress.com/act) ist ein open-Source-Projekt entwickelt wurde, durch die Mitglieder der ASP.NET-Community und das ASP.NET-Team. 


[![Das AJAX Control Toolkit herunterladen](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**Abbildung 01**: das AJAX Control Toolkit herunterladen ([klicken Sie, um das Bild in voller Größe anzeigen](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))


Nachdem Sie die Datei heruntergeladen haben, müssen Sie die Datei zu entsperren. Mit der rechten Maustaste in der das, wählen Sie Eigenschaften und klicken Sie auf die **Unblock** Schaltfläche (siehe Abbildung 2).


[![Aufhebung der Blockierung der ZIP-AJAX Control Toolkit-Datei](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**Abbildung 02**: Aufhebung der Blockierung der ZIP-AJAX Control Toolkit-Datei ([klicken Sie, um das Bild in voller Größe anzeigen](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))


Nachdem Sie die Blockierung der Datei aufzuheben, können Sie die Datei entpacken: mit der rechten Maustaste in der das, und wählen Sie die **alle extrahieren** Option des Menüs. Jetzt können wir das Toolkit zur Visual Studio/Visual Web Developer Toolbox hinzufügen.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Hinzufügen von AJAX Control Toolkit zur Toolbox

Die einfachste Möglichkeit zum Verwenden von AJAX Control Toolkit wird der Toolbox von Visual Studio/Visual Web Developer das Toolkit hinzugefügt (siehe Abbildung 3). Auf diese Weise können Sie einfach ziehen ein Toolkit-Steuerelement auf eine Seite, wenn Sie sie verwenden möchten.


[![AJAX Control Toolkit wird in Toolbox angezeigt.](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**Abbildung 03**: AJAX Control Toolkit wird in Toolbox angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))


Zunächst müssen Sie eine AJAX Control Toolkit-Registerkarte der Toolbox hinzu. Gehen Sie wie folgt vor.

1. Erstellen Sie eine neue ASP.NET-Website durch Auswählen der Menüoption Datei neue Website. Doppelklicken Sie auf der "default.aspx" im Projektmappen-Explorer-Fenster, um die Datei im Editor zu öffnen.
2. Mit der rechten Maustaste in der Toolbox unter der Registerkarte "Allgemein", und wählen Sie die Menüoption **Registerkarte hinzufügen** (siehe Abbildung 4).
3. Geben Sie eine neue Registerkarte mit dem Namen AJAX Control Toolkit.


[![Hinzufügen einer neuen Registerkarte](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**Abbildung 04**: Hinzufügen einer neuen Registerkarte ([klicken Sie, um das Bild in voller Größe anzeigen](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))


Als Nächstes müssen Sie die neue Registerkarte dem AJAX Control Toolkit-Steuerelemente hinzufügen. Führen Sie folgende Schritte aus:

- Mit der rechten Maustaste unterhalb der Registerkarte "AJAX Control Toolkit", und wählen Sie die Menüoption **"Elemente auswählen" (siehe Abbildung 5)**.
- Navigieren Sie zum Speicherort, in dem Sie das AJAX Control Toolkit entpackt und wählen Sie die Assembly AjaxControlToolkit.dll.


[![Wählen Sie Elemente aus der Toolbox hinzu](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**Abbildung 05**: Wählen Sie Elemente aus der Toolbox hinzuzufügende ([klicken Sie, um das Bild in voller Größe anzeigen](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))


Nachdem Sie diese Schritte abgeschlossen haben, werden sämtliche Toolkit-Steuerelementen in der Toolbox angezeigt.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Ein Upgrade auf eine neue Version des Toolkits

Wenn Sie eine ältere Version des Toolkits verwenden würde, und nun verschieben, müssen hier eine höhere Version sind die empfohlenen Schritte:

- Binärdateien - löschen die alte Version der Assembly AjaxControlToolkit.dll aus Ihrem Ordner "Website" Bin ".
- Toolboxelemente - löschen die Registerkarte "AJAX Control Toolkit" und führen Sie die Schritte aus, um die Registerkarte mit der neuen Version der Assembly AjaxControlToolkit.dll neu zu erstellen.

> [!div class="step-by-step"]
> [Nächste](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
