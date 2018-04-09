---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Erstellen eines Domänencontrollers (c#) | Microsoft Docs
author: StephenWalther
description: In diesem Lernprogramm wird Stephen Walther veranschaulicht, wie Sie einen Controller einer ASP.NET MVC-Anwendung hinzufügen können.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 86966f1064d09419e2102542c6d14c4162d153e4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-controller-c"></a>Erstellen eines Domänencontrollers (c#)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> In diesem Lernprogramm wird Stephen Walther veranschaulicht, wie Sie einen Controller einer ASP.NET MVC-Anwendung hinzufügen können.


Ziel dieses Lernprogramms wird erläutert, wie Sie die neue ASP.NET MVC Controllers erstellen können. Erfahren Sie, wie zum Erstellen von Controllern mithilfe der Visual Studio, Controller hinzufügen "und erstellen eine Klassendatei per hand katalogisiert wird.

### <a name="using-the-add-controller-menu-option"></a>Mit der Menüoption Controller hinzufügen

Die einfachste Möglichkeit zum Erstellen eines neuen Controllers ist mit der rechten Maustaste in des Ordners "Controller" im Projektmappen-Explorer von Visual Studio-Fenster, und wählen Sie die **hinzufügen, Controller** Menüoption (siehe Abbildung 1). Durch das Auswählen dieser Option wird die **Controller hinzufügen** Dialogfeld (siehe Abbildung 2).


[![Das Dialogfeld "Neues Projekt"](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)

**Abbildung 01**: Hinzufügen eines neuen Controllers ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-controller-cs/_static/image2.png))


[![Das Dialogfeld "Neues Projekt"](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)

**Abbildung 02**: der Controller der hinzufügen-Dialogfeld ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-controller-cs/_static/image4.png))


Beachten Sie, die in der erste Teil des Controllernamens hervorgehoben ist die **Controller hinzufügen** Dialogfeld. Jeder Controllername muss mit dem Suffix enden *Controller*. Sie können z. B. einen Controller namens erstellen *ProductController* jedoch nicht auf einen Controller aus, die mit dem Namen *Produkt*.


Wenn Sie einen Controller erstellen, die fehlen die *Controller* suffix, und Sie können zum Aufrufen des Controllers nicht. Führen Sie dies – ich haben zahllose Stunden damit Meine abgelaufenen nach dem Herstellen dieser Fehler verschwendet.


**1 – Controllers\ProductController.cs auflisten**

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

Sie sollten Domänencontroller immer im Ordner "Controller" erstellen. Andernfalls Sie müssen die Konventionen der ASP.NET MVC verletzen, und andere Entwickler müssen nacheinander schwieriger verstehen Ihre Anwendung.

### <a name="scaffolding-action-methods"></a>Gerüstbau Aktionsmethoden

Wenn Sie einen Controller erstellen, stehen Ihnen die Option zum automatischen Generieren von erstellen, aktualisieren und Details Aktionsmethoden (siehe Abbildung 3). Bei Auswahl dieser Option wird die Controllerklasse auflisten 2 generiert.


[![Aktionsmethoden erstellen automatisch](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)

**Abbildung 03**: Aktionsmethoden automatisch erstellt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-controller-cs/_static/image6.png))


**Auflisten von 2 – Controllers\CustomerController.cs**

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

Diese generierten Methoden werden Stub. Sie müssen die eigentliche Logik zum Erstellen, aktualisieren und Anzeigen von Details für einen Kunden selbst hinzufügen. Allerdings die Stubmethoden mit nice Ausgangspunkt bereitgestellt.

### <a name="creating-a-controller-class"></a>Erstellen einer Controllerklasse

Der ASP.NET MVC-Controller ist nur eine Klasse. Falls gewünscht, können Sie ignorieren die geeigneten Controller Gerüstbau für Visual Studio und erstellen eine Controllerklasse per hand katalogisiert wird. Führen Sie folgende Schritte aus:

1. Mit der rechten Maustaste in des Ordners Controller, und wählen Sie die Menüoption **hinzufügen, neue Element** , und wählen Sie die **Klasse** Vorlage (siehe Abbildung 4).
2. Benennen Sie die neue Klasse PersonController.cs, und klicken Sie auf die **hinzufügen** Schaltfläche.
3. Ändern Sie die resultierende Klassendatei, sodass die Klasse von der Basisklasse System.Web.Mvc.Controller (Siehe auflisten von 3) erbt.


[![Erstellen einer neuen Klasse](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)

**Abbildung 04**: Erstellen einer neuen Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-controller-cs/_static/image8.png))


**3 – Controllers\PersonController.cs auflisten**

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

Der Controller im Codebeispiel 3 verfügbar macht, eine Aktion, die mit dem Namen Index(), die die Zeichenfolge "Hello World!" zurückgibt. Sie können diese Controlleraktion durch Ausführen der Anwendung und zum Anfordern einer URL wie folgt aufrufen:

`http://localhost:40071/Person`

> [!NOTE]
> 
> Der ASP.NET Development Server verwendet eine zufällige Portnummer (z. B. 40071). Wenn Sie eine URL zum Aufrufen eines Controllers eingeben, müssen Sie die richtige Portnummer angeben. Sie können die Nummer des Ports bestimmen, indem Sie mit dem Mauszeiger der Maus auf das Symbol für den ASP.NET Development Server im Infobereich von Windows (untere Ecke des Bildschirms).
> 
> [!div class="step-by-step"]
> [Zurück](adding-dynamic-content-to-a-cached-page-cs.md)
> [Weiter](creating-an-action-cs.md)
