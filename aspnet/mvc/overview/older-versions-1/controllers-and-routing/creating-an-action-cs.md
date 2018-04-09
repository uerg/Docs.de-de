---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
title: Erstellen einer Aktion (c#) | Microsoft Docs
author: microsoft
description: Erfahren Sie, wie Sie ein ASP.NET MVC-Controller eine neue Aktion hinzufügen. Informationen Sie zu den Anforderungen für eine Methode einer Aktion sein.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: cb33b28c-3025-4bd1-a1fa-eaa3af7bb56f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-an-action-cs
msc.type: authoredcontent
ms.openlocfilehash: 7c6145902db59b07e96a5563b138c1a6323946b2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="creating-an-action-c"></a>Erstellen einer Aktion (c#)
====================
durch [Microsoft](https://github.com/microsoft)

> Erfahren Sie, wie Sie ein ASP.NET MVC-Controller eine neue Aktion hinzufügen. Informationen Sie zu den Anforderungen für eine Methode einer Aktion sein.


Ziel dieses Lernprogramms wird erläutert, wie Sie eine neue Controlleraktion erstellen können. Sie erfahren Sie mehr über die Anforderungen einer Aktionsmethode. Sie erfahren außerdem, wie Sie verhindern, dass eine Methode als Aktion verfügbar gemacht werden.

## <a name="adding-an-action-to-a-controller"></a>Hinzufügen einer Aktion zu einem Domänencontroller

Sie fügen eine neue Aktion an einen Controller hinzu, indem Sie eine neue Methode mit dem Controller. Der Controller im Codebeispiel 1 enthält beispielsweise eine Aktion, die mit dem Namen Index() und eine Aktion, die mit dem Namen SayHello(). Beide Methoden werden als Aktivitäten verfügbar gemacht.

**1 – Controllers\HomeController. cs auflisten**

[!code-csharp[Main](creating-an-action-cs/samples/sample1.cs)]

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

Wenn Sie eine öffentliche Methode in einer Controllerklasse erstellen müssen und Sie nicht die Methode als eine Controlleraktion verfügbar machen möchten, können Sie die Methode verhindern, aufgerufen wird, mit dem Attribut [NonAction]. Der Controller im Codebeispiel 2 enthält z. B. eine öffentliche Methode mit dem Namen CompanySecrets(), die mit dem [NonAction]-Attribut ergänzt wird.

**Auflisten von 2 – Controllers\WorkController.cs**

[!code-csharp[Main](creating-an-action-cs/samples/sample2.cs)]

Wenn Sie versuchen, eine Controlleraktion CompanySecrets() aufrufen durch Eingabe von /Work/CompanySecrets in die Adressleiste des Browsers klicken Sie dann erhalten die Fehlermeldung in Abbildung 1 Sie.


[![Aufrufen einer NonAction-Methode](creating-an-action-cs/_static/image1.jpg)](creating-an-action-cs/_static/image1.png)

**Abbildung 01**: Aufrufen einer Methode NonAction ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-an-action-cs/_static/image2.png))

> [!div class="step-by-step"]
> [Zurück](creating-a-controller-cs.md)
> [Weiter](asp-net-mvc-routing-overview-vb.md)
