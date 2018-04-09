---
uid: web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
title: Erste Schritte mit der AJAX-Steuerelement-Toolkit (c#) | Microsoft Docs
author: microsoft
description: Erfahren Sie, müssen Sie wissen, erste Schritte mit dem Toolkit für AJAX-Steuerelement.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 16dc5c11-65be-4eae-a818-9fad7f8259c6
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/get-started-with-the-ajax-control-toolkit-cs
msc.type: authoredcontent
ms.openlocfilehash: e6a7a8d45f32a33eaacf3c42b52a02d2ada1aab6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="get-started-with-the-ajax-control-toolkit-c"></a>Erste Schritte mit der AJAX-Steuerelement-Toolkit (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> Erfahren Sie, müssen Sie wissen, erste Schritte mit dem Toolkit für AJAX-Steuerelement.


Das AJAX-Steuerelement-Toolkit enthält mehr als 30 freien Steuerelemente, die Sie in Ihrer ASP.NET-Anwendungen verwenden können. In diesem Lernprogramm erfahren Sie, wie eine AJAX-Steuerelement-Toolkit herunterladen und Ihre Visual Studio/Visual Web Developer Express Toolbox Toolkit-Steuerelemente hinzuzufügen.

## <a name="downloading-the-ajax-control-toolkit"></a>Herunterladen des Toolkits der AJAX-Steuerelement

Die [AJAX Control Toolkit](http://devexpress.com/act) ist ein open Source-Projekt entwickelt wurde, durch die Mitglieder der ASP.NET-Community und das Team ASP.NET. 


[![Herunterladen des Toolkits der AJAX-Steuerelement](get-started-with-the-ajax-control-toolkit-cs/_static/image1.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image1.png)

**Abbildung 01**: Herunterladen des Toolkits der AJAX-Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](get-started-with-the-ajax-control-toolkit-cs/_static/image2.png))


Nachdem Sie die Datei heruntergeladen haben, müssen Sie die Datei zu entsperren. Mit der rechten Maustaste in der das, wählen Sie Eigenschaften aus, und klicken Sie auf die **zum Aufheben der Sperre** Schaltfläche (siehe Abbildung 2).


[![Zum Aufheben der Blockierung der AJAX-Steuerelement-Toolkit-ZIP-Datei](get-started-with-the-ajax-control-toolkit-cs/_static/image2.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image3.png)

**Abbildung 02**: zum Aufheben der Blockierung der AJAX-Steuerelement-Toolkit-ZIP-Datei ([klicken Sie hier, um das Bild in voller Größe angezeigt](get-started-with-the-ajax-control-toolkit-cs/_static/image4.png))


Nachdem Sie die Datei aufheben, können Sie die Datei entpacken: mit der rechten Maustaste in der das, und wählen Sie die **alle extrahieren** Option des Menüs. Jetzt können wir das Toolkit zur Visual Studio/Visual Web Developer-Toolbox hinzu.

## <a name="adding-the-ajax-control-toolkit-to-the-toolbox"></a>Hinzufügen von AJAX-Steuerelement-Toolkit zur Toolbox

Die einfachste Möglichkeit zum Verwenden von AJAX-Steuerelement-Toolkit wird die Toolbox von Visual Studio/Visual Web Developer das Toolkit hinzugefügt (siehe Abbildung 3). Auf diese Weise können Sie einfach ziehen ein Steuerelement Toolkit auf eine Seite, wenn Sie sie verwenden möchten.


[![AJAX-Steuerelement-Toolkit wird in der Toolbox angezeigt.](get-started-with-the-ajax-control-toolkit-cs/_static/image3.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image5.png)

**Abbildung 03**: AJAX-Steuerelement-Toolkit wird in der Toolbox angezeigt ([klicken Sie hier, um das Bild in voller Größe angezeigt](get-started-with-the-ajax-control-toolkit-cs/_static/image6.png))


Zunächst müssen Sie ein AJAX-Steuerelement-Toolkit-Registerkarte zur Toolbox hinzu. Gehen Sie folgendermaßen vor.

1. Erstellen Sie eine neue ASP.NET-Website dazu die Menüoption Datei neue Website an. Doppelklicken Sie auf die "default.aspx" im Projektmappen-Explorer-Fenster, um die Datei im Editor zu öffnen.
2. Mit der rechten Maustaste in der Toolbox unter der Registerkarte "Allgemein", und wählen Sie die Menüoption **Registerkarte hinzufügen** (siehe Abbildung 4).
3. Geben Sie eine neue Registerkarte mit dem Namen AJAX Control Toolkit.


[![Hinzufügen einer neuen Registerkarte](get-started-with-the-ajax-control-toolkit-cs/_static/image4.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image7.png)

**Abbildung 04**: Hinzufügen einer neuen Registerkarte ([klicken Sie hier, um das Bild in voller Größe angezeigt](get-started-with-the-ajax-control-toolkit-cs/_static/image8.png))


Als Nächstes müssen Sie die neue Registerkarte die AJAX-Steuerelement-Toolkit-Steuerelemente hinzufügen. Führen Sie folgende Schritte aus:

- Mit der rechten Maustaste unterhalb der Registerkarte "AJAX Control Toolkit", und wählen Sie die Menüoption **Elemente auswählen (siehe Abbildung 5)**.
- Navigieren Sie zum Speicherort, in dem das AJAX-Steuerelement-Toolkit entzippt und wählen Sie die AjaxControlToolkit.dll-Assembly.


[![Wählen Sie Elemente aus der Toolbox hinzu](get-started-with-the-ajax-control-toolkit-cs/_static/image5.jpg)](get-started-with-the-ajax-control-toolkit-cs/_static/image9.png)

**Abbildung 05**: Wählen Sie Elemente aus der Toolbox hinzu ([klicken Sie hier, um das Bild in voller Größe angezeigt](get-started-with-the-ajax-control-toolkit-cs/_static/image10.png))


Nachdem Sie diese Schritte abgeschlossen haben, werden alle Toolkit-Steuerelemente in der Toolbox angezeigt.

## <a name="upgrading-to-a-new-version-of-the-toolkit"></a>Upgrade auf eine neue Version des Toolkits

Wenn Sie eine ältere Version des Toolkits verwenden und müssen nun auf Verschieben hier eine höhere Version sind die empfohlenen Schritte:

- Binärdateien - löschen Sie die alte Version der Assembly AjaxControlToolkit.dll aus Ihrer Website-Ordner "bin".
- Toolboxelemente - löschen die Registerkarte "AJAX Control Toolkit" und führen Sie die obengenannten Schritte, um die Registerkarte mit der neuen Version der Assembly AjaxControlToolkit.dll neu zu erstellen.

> [!div class="step-by-step"]
> [Nächste](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
