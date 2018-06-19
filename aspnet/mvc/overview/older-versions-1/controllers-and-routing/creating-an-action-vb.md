---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Erstellen einer Aktion (VB) | Microsoft Docs
author: microsoft
description: Erfahren Sie, wie Sie ein ASP.NET MVC-Controller eine neue Aktion hinzufügen. Informationen Sie zu den Anforderungen für eine Methode einer Aktion sein.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: c77e4738444c61d60bdd78a50b36f98be41fc271
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867891"
---
<a name="creating-an-action-vb"></a>Erstellen einer Aktion (VB)
====================
durch [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie Sie ein ASP.NET MVC-Controller eine neue Aktion hinzufügen. Informationen Sie zu den Anforderungen für eine Methode einer Aktion sein.


Ziel dieses Lernprogramms wird erläutert, wie Sie eine neue Controlleraktion erstellen können. Sie erfahren Sie mehr über die Anforderungen einer Aktionsmethode. Sie erfahren außerdem, wie Sie verhindern, dass eine Methode als Aktion verfügbar gemacht werden.

## <a name="adding-an-action-to-a-controller"></a>Hinzufügen einer Aktion zu einem Domänencontroller

Sie fügen eine neue Aktion an einen Controller hinzu, indem Sie eine neue Methode mit dem Controller. Der Controller im Codebeispiel 1 enthält beispielsweise eine Aktion, die mit dem Namen Index() und eine Aktion, die mit dem Namen SayHello(). Beide Methoden werden als Aktivitäten verfügbar gemacht.

**1 – Controllers\HomeController.vb auflisten**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Um die Universe als Aktion zur Verfügung gestellt werden, muss eine Methode bestimmte Anforderungen erfüllen:

- Die Methode muss öffentlich sein.
- Die Methode kann nicht auf eine statische Methode sein.
- Die Methode kann nicht über eine Erweiterungsmethode sein.
- Die Methode darf nicht über einen Konstruktor, Getter oder Setter sein.
- Die Methode darf keine offene generische Typen enthalten.
- Die Methode ist keine Methode der Basisklasse Controller.
- Die Methode sind keine **Ref** oder **out** Parameter.

Beachten Sie, dass es keine Einschränkungen auf den Rückgabetyp der eine Controlleraktion gibt. Eine Controlleraktion kann es sich um eine Zeichenfolge, einen datetime-Wert, eine Instanz der Klasse zufällige oder "void" zurückgeben. ASP.NET MVC-Framework konvertiert alle Rückgabetyp, die nicht in eine Zeichenfolge ein Aktionsergebnis ist und die Zeichenfolge an den Browser zu rendern.

Wenn Sie eine beliebige Methode, die diese Anforderungen an einen Controller nicht verletzt hinzufügen, wird die Methode als eine Controlleraktion verfügbar gemacht. Achten Sie hier. Von jedem Benutzer mit dem Internet verbunden kann eine Controlleraktion aufgerufen werden. Erstellen Sie eine Controlleraktion DeleteMyWebsite() nicht, z. B.

## <a name="preventing-a-public-method-from-being-invoked"></a>Verhindert, dass eine öffentliche Methode aufgerufen wird

Wenn Sie eine öffentliche Methode in einer Controllerklasse erstellen müssen, nicht die Methode als eine Controlleraktion verfügbar machen möchten, und Sie verhindern, dass die Methode aufgerufen wird können, mithilfe der &lt;NonAction&gt; Attribut. Der Controller im Codebeispiel 2 enthält z. B. eine öffentliche Methode mit dem Namen CompanySecrets(), die mit ergänzt wird die &lt;NonAction&gt; Attribut.

**Auflisten von 2 – Controllers\WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Wenn Sie versuchen, eine Controlleraktion CompanySecrets() aufrufen durch Eingabe von /Work/CompanySecrets in die Adressleiste des Browsers klicken Sie dann erhalten die Fehlermeldung in Abbildung 1 Sie.


[![Aufrufen einer NonAction-Methode](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Abbildung 01**: Aufrufen einer Methode NonAction ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [Zurück](creating-a-controller-vb.md)
> [Weiter](aspnet-mvc-controllers-overview-cs.md)
