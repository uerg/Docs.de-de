---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Erstellen eines Controllers (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther wird in diesem Tutorial veranschaulicht, wie Sie einen Controller zu einer ASP.NET MVC-Anwendung hinzufügen können.
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: b24aeabc6fd4192f44f83e0c0b2150e87ef6f501
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833062"
---
<a name="creating-a-controller-vb"></a>Erstellen eines Controllers (VB)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther wird in diesem Tutorial veranschaulicht, wie Sie einen Controller zu einer ASP.NET MVC-Anwendung hinzufügen können.


Das Ziel in diesem Tutorial wird beschrieben, wie Sie die neue ASP.NET MVC Controllers erstellen können. Erfahren Sie, wie Controller nach mithilfe der Visual Studio, Controller hinzufügen "-" und erstellen eine Datei manuell zu erstellen.

### <a name="using-the-add-controller-menu-option"></a>Mithilfe der Menüoption Controller hinzufügen

Die einfachste Möglichkeit zum Erstellen eines neuen Controllers ist mit der rechten Maustaste in den Ordner "Controllers" im Projektmappen-Explorer von Visual Studio-Fenster, und wählen Sie die **hinzufügen, Controller** Menüoption (siehe Abbildung 1). Durch Auswählen dieser Menüoption wird das **Controller hinzufügen** Dialogfeld (siehe Abbildung 2).


[![Das Dialogfeld "Neues Projekt"](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)

**Abbildung 01**: Hinzufügen eines neuen Controllers ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-controller-vb/_static/image2.png))


[![Das Dialogfeld "Neues Projekt"](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)

**Abbildung 02**: die Controller der hinzufügen-Dialogfeld ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-controller-vb/_static/image4.png))


Beachten Sie, das der erste Teil des Controllernamens, in ausgewählt ist der **Controller hinzufügen** Dialogfeld. Jeder Controllername muss mit der Endung *Controller*. Sie können z. B. einen Controller namens erstellen *ProductController* jedoch nicht auf einen Controller, mit dem Namen *Produkt*.


Wenn Sie einen Controller erstellen, die fehlen die *Controller* suffix, und klicken Sie dann Sie nicht den Controller aufrufen können. Davon wird abgeraten, denn ich haben sich zahllose Stunden für mein Leben, nachdem Sie diesen Fehler, verschwendet.


**1 – Controllers\ProductController.vb auflisten**

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

Sie sollten immer im Ordner "Controllers" Controller erstellen. Andernfalls Sie verletzt werden die Konventionen der ASP.NET MVC und andere Entwickler haben schwieriger verstehen Ihre Anwendung.

### <a name="scaffolding-action-methods"></a>Gerüstbau für Aktionsmethoden

Wenn Sie einen Controller erstellen, haben Sie die Option zum automatischen Generieren von Aktionsmethoden für Create, Update und Details (siehe Abbildung 3). Bei Auswahl dieser Option wird die Controller-Klasse im Codebeispiel 2 generiert.


[![Aktionsmethoden erstellen automatisch](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)

**Abbildung 03**: Aktionsmethoden automatisch erstellt ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-controller-vb/_static/image6.png))


**Codebeispiel 2 - Controllers\CustomerController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

Diese generierten Methoden sind Stubmethoden. Sie müssen die eigentliche Logik zum Erstellen, aktualisieren und Anzeigen von Details für einen Kunden selbst hinzufügen. Aber die Stubmethoden bieten Ihnen gute Ausgangspunkt.

### <a name="creating-a-controller-class"></a>Erstellen eine Controller-Klasse

Die ASP.NET MVC-Controller ist nur eine Klasse. Falls gewünscht, können Sie ignorieren den einfachen Controller Gerüstbau für Visual Studio und erstellen manuell eine Controller-Klasse. Führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste in den Ordner "Controllers", und wählen Sie die Menüoption **hinzufügen, neue Element** , und wählen Sie die **Klasse** Vorlage (siehe Abbildung 4).
2. Nennen Sie die neue Klasse PersonController.vb, und klicken Sie auf die **hinzufügen** Schaltfläche.
3. Ändern Sie die resultierende Klassendatei, sodass die Klasse von der Basisklasse für den System.Web.Mvc.Controller (siehe Codebeispiel 3) erbt.


[![Erstellen einer neuen Klasse](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)

**Abbildung 04**: Erstellen einer neuen Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-controller-vb/_static/image8.png))


**Codebeispiel 3 - Controllers\PersonController.vb**

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

Der Controller in Programmausdruck 3 bietet es sich um eine Aktion, die mit dem Namen Index(), die die Zeichenfolge "Hello World!" zurückgibt. Sie können diese Controlleraktion aufrufen, indem Sie Ausführung Ihrer Anwendung und zum Anfordern einer URL wie folgt:

`http://localhost:40071/Person`

> [!NOTE]
> 
> Der ASP.NET Development Server verwendet eine beliebige Portnummer (z. B. 40071). Wenn Sie eine URL zum Aufrufen eines Controllers eingeben zu können, müssen Sie die richtige Portnummer angeben. Sie können die Nummer des Ports bestimmen, durch Bewegen des Mauszeigers über dem Symbol für den ASP.NET Development Server in den Windows-Benachrichtigungsbereich (unten rechts des Bildschirms).
> 
> [!div class="step-by-step"]
> [Zurück](adding-dynamic-content-to-a-cached-page-vb.md)
> [Weiter](creating-an-action-vb.md)
