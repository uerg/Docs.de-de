---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Teil 2: Datenzugriffsschicht | Microsoft-Dokumentation'
author: JoeStagner
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 2 umfasst das Hinzufügen der Datenzugriffsschicht.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 5b5ae08b157054bf0f3231782127ee3110f36d00
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827368"
---
<a name="part-2-data-access-layer"></a>Teil 2: Datenzugriffsschicht
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außerordentlich einfach es ist, erstellen Sie leistungsstarke, skalierbare Anwendungen für die .NET-Plattform. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 zu verwenden, um eine online-Store, einschließlich der Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 2 umfasst das Hinzufügen der Datenzugriffsschicht.


## <a id="_Toc260221668"></a>  Hinzufügen der Datenzugriffsschicht

Die e-Commerce-Anwendung hängt von zwei Datenbanken.

Für Informationen zu den Kunden verwenden wir die ASP.NET-Mitgliedschaft-Standarddatenbank. Für unseren shopping Cart "und" Produkt-Katalog werden wir eine SQL Express-Datenbank wie folgt implementieren.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Erstellen der Datenbank (Commerce.mdf) in der Anwendung App\_Datenordner wir fortfahren können, um unsere Data Access Layer, die mithilfe von .NET Entity Framework zu erstellen.

Wir erstellen einen Ordner namens "Data\_Zugriff", und klicken Sie mit der rechten Maustaste auf diesen Ordner und wählen Sie "Neues Element hinzufügen".

Geben Sie unter "Installierte Vorlagen" Item "und" und wählen Sie dann "ADO.NET Entity Data Model" EDM\_Commerce.edmx als den Namen, und klicken Sie auf die Schaltfläche "Hinzufügen".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Wählen Sie "Aus Datenbank generieren".

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Speichern und erstellen.

Jetzt können wir unsere erste Feature – eines Product Category-Menüs hinzufügen.

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-1.md)
> [Weiter](tailspin-spyworks-part-3.md)
