---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
title: Erstellen einer Aktion (VB) | Microsoft-Dokumentation
author: microsoft
description: Erfahren Sie, wie Sie ASP.NET MVC-Controller eine neue Aktion hinzufügen. Informationen Sie zu den Anforderungen für eine Methode, um eine Aktion werden.
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: c8d93e11-ef78-4a30-afbc-f30419000a60
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-vb
msc.type: authoredcontent
ms.openlocfilehash: 6ec69f5aa9f7a789cb533a8dc04dca952adf35d0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824045"
---
<a name="creating-an-action-vb"></a>Erstellen einer Aktion (VB)
====================
durch [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie Sie ASP.NET MVC-Controller eine neue Aktion hinzufügen. Informationen Sie zu den Anforderungen für eine Methode, um eine Aktion werden.


Das Ziel in diesem Tutorial wird beschrieben, wie Sie eine neue Controlleraktion erstellen können. Sie erfahren Sie mehr über die Anforderungen von einer Aktionsmethode. Sie erfahren außerdem, wie Sie verhindern, dass eine Methode als eine Aktion verfügbar gemacht werden.

## <a name="adding-an-action-to-a-controller"></a>Hinzufügen einer Aktion zu einem Controller

Sie können eine neue Aktion auf einen Controller hinzufügen, durch Hinzufügen einer neuen Methode mit dem Controller. Der Controller in Codebeispiel 1 enthält beispielsweise eine Aktion, die mit dem Namen Index() und eine Aktion, die mit dem Namen SayHello(). Beide Methoden werden als Aktivitäten verfügbar gemacht.

**1 – Controllers\HomeController.vb auflisten**

[!code-vb[Main](creating-an-action-vb/samples/sample1.vb)]

Um das Universum als Aktion verfügbar gemacht werden, muss eine Methode bestimmte Anforderungen erfüllen:

- Die Methode muss öffentlich sein.
- Die Methode darf nicht mit einer statischen Methode sein.
- Die Methode kann nicht über eine Erweiterungsmethode sein.
- Die Methode kann nicht über einen Konstruktor, Getter oder Setter sein.
- Die Methode keine offene generische Typen.
- Die Methode ist keine Methode der Basisklasse Controller.
- Die Methode darf keine enthalten **Ref** oder **out** Parameter.

Beachten Sie, dass es keine Einschränkungen für den Rückgabetyp der eine Controlleraktion gibt. Eine Controlleraktion kann es sich um eine Zeichenfolge, einen datetime-Wert, eine Instanz der Random-Klasse, oder "void" zurückgeben. ASP.NET MVC-Framework konvertiert jeder Rückgabetyp, die nicht auf ein Aktionsergebnis in eine Zeichenfolge ist und die Zeichenfolge an den Browser zu rendern.

Wenn Sie eine beliebige Methode, die diese Anforderungen an einen Controller nicht verletzt hinzufügen, wird die Methode als eine Controlleraktion verfügbar gemacht. Achten Sie hier. Eine Controlleraktion kann von jedem Benutzer mit dem Internet verbundenen aufgerufen werden. Erstellen Sie eine Controlleraktion DeleteMyWebsite() nicht der Fall, z. B.

## <a name="preventing-a-public-method-from-being-invoked"></a>Verhindert, dass eine öffentliche Methode aufgerufen wird

Wenn Sie eine öffentliche Methode in einer Controllerklasse zu erstellen müssen, nicht die Methode als eine Controlleraktion verfügbar machen möchten, und Sie verhindern, dass die Methode aufgerufen wird können, mithilfe der &lt;NonAction&gt; Attribut. Der Controller im Codebeispiel 2 enthält beispielsweise eine öffentliche Methode mit dem Namen CompanySecrets(), die mit ergänzt wird die &lt;NonAction&gt; Attribut.

**Codebeispiel 2 - Controllers\WorkController.vb**

[!code-vb[Main](creating-an-action-vb/samples/sample2.vb)]

Wenn Sie versuchen, die Controlleraktion CompanySecrets() aufrufen, indem Sie die Eingabe /Work/CompanySecrets in die Adressleiste des Browsers klicken Sie dann erhalten die Fehlermeldung in Abbildung 1 Sie.


[![Aufrufen einer NonAction-Methode](creating-an-action-vb/_static/image1.jpg)](creating-an-action-vb/_static/image1.png)

**Abbildung 01**: Aufrufen einer Methode NonAction ([klicken Sie, um das Bild in voller Größe anzeigen](creating-an-action-vb/_static/image2.png))

> [!div class="step-by-step"]
> [Zurück](creating-a-controller-vb.md)
> [Weiter](aspnet-mvc-controllers-overview-cs.md)
