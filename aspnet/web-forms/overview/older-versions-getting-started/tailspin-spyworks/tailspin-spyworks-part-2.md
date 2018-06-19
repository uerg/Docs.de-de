---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Teil 2: Datenzugriffsebene | Microsoft Docs'
author: JoeStagner
description: Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 2 wird das Hinzufügen von der Datenzugriffsebene behandelt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f734b04a0f4cec3c33bc5b42ef283ea64cdb463
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890488"
---
<a name="part-2-data-access-layer"></a>Teil 2: Die Datenzugriffsebene
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 2 wird das Hinzufügen von der Datenzugriffsebene behandelt.


## <a id="_Toc260221668"></a>  Hinzufügen von der Datenzugriffsebene

Unsere e-Commerce-Anwendung hängt von zwei Datenbanken.

Für Kundeninformationen verwenden wir die ASP.NET-Mitgliedschaft-Standarddatenbank. Für unsere Warenkorb Einkaufswagen und Product Catalog implementieren wir eine SQL Express-Datenbank wie folgt.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Mit dem Erstellen der Datenbank (Commerce.mdf) in der Anwendungskonfigurationsdatei App\_Datenordner können wir unsere Datenzugriffsebene, die mit dem .NET Entity Framework erstellen fortfahren.

Wir erstellen einen Ordner mit dem Namen "Data\_Zugriff" und sie mit der rechten Maustaste auf den Ordner, und wählen Sie "Neues Element hinzufügen".

Geben Sie unter "Installierte Vorlagen" Element und wählen Sie dann "ADO.NET Entity Data Model" EDM\_Commerce.edmx als den Namen, und klicken Sie auf die Schaltfläche "Hinzufügen".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Wählen Sie "Aus Datenbank generieren".

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Speichern und erstellen.

Jetzt können wir unsere erste Funktion – ein Product Category-Menü hinzu.

> [!div class="step-by-step"]
> [Zurück](tailspin-spyworks-part-1.md)
> [Weiter](tailspin-spyworks-part-3.md)
