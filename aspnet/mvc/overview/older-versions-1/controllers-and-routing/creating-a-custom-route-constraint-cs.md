---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
title: Erstellen einer benutzerdefinierten Routeneinschränkung (c#) | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther wird veranschaulicht, wie Sie eine benutzerdefinierten routeneinschränkung erstellen können. Implementieren wir ein einfaches benutzerdefiniertes Einschränkung, die verhindert, dass eine Route wird abgeglichen, w...
ms.author: aspnetcontent
ms.date: 02/16/2009
ms.assetid: a4f4bf4e-abcc-4650-8f43-527e48b52fe6
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-cs
msc.type: authoredcontent
ms.openlocfilehash: e21e7e027cf66f390fc37ec08a07ae007e8242c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37831812"
---
<a name="creating-a-custom-route-constraint-c"></a>Erstellen einer benutzerdefinierten Routeneinschränkung (c#)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther wird veranschaulicht, wie Sie eine benutzerdefinierten routeneinschränkung erstellen können. Wir implementieren eine einfache benutzerdefinierte Einschränkung, die verhindert, dass eine Route wird eine Übereinstimmung, wenn der Browser von einem Remotecomputer angefordert werden.


Das Ziel in diesem Tutorial wird veranschaulicht, wie Sie eine benutzerdefinierten routeneinschränkung erstellen können. Eine benutzerdefinierte Route-Einschränkung können Sie verhindern, dass eine Route abgeglichen wird, es sei denn, eine benutzerdefinierte Bedingung erfüllt ist.

In diesem Tutorial erstellen wir eine routeneinschränkung "localhost". Die Localhost-routeneinschränkung entspricht nur Anforderungen, die von dem lokalen Computer. Remoteanforderungen aus über das Internet werden nicht verglichen.

Sie implementieren eine benutzerdefinierten routeneinschränkung durch Implementieren der IRouteConstraint-Schnittstelle. Dies ist eine sehr einfache Schnittstelle, die eine einzelne Methode beschrieben:

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample1.cs)]

Die Methode gibt einen booleschen Wert zurück. Wenn Sie auf "false" zurückgibt, wird nicht die Route, die mit der Einschränkung verknüpft sind die Browseranforderung übereinstimmen.

Die Einschränkung "localhost", die in Codebeispiel 1 enthalten ist.

**1 – LocalhostConstraint.cs auflisten**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample2.cs)]

Die Einschränkung in Codebeispiel 1 nutzt die IsLocal-Eigenschaft, die von den HttpRequest-Klasse verfügbar gemacht werden. Diese Eigenschaft gibt true zurück, wenn die IP-Adresse der Anforderung entweder 127.0.0.1 ist oder die IP-Adresse der Anforderung, die IP-Adresse des Servers identisch ist.

Sie verwenden eine benutzerdefinierte Einschränkung in einer Route, die in der Datei "Global.asax" definiert. Die Datei "Global.asax" in Liste 2 verwendet die Einschränkung "localhost", um zu verhindern, dass jeder Benutzer eine Seite "Administrator" angefordert, es sei denn, sie die Anforderung vom lokalen Server machen. Eine Anforderung für /Admin/DeleteAll schlägt beispielsweise fehl, wenn von einem Remoteserver hergestellt.

**Codebeispiel 2 – "Global.asax"**

[!code-csharp[Main](creating-a-custom-route-constraint-cs/samples/sample3.cs)]

Die Einschränkung "localhost" wird in der Definition der Admin-Route verwendet. Diese Route wird nicht von einer remote-Browser-Anforderung zugeordnet werden. Beachten Sie jedoch, dass andere in Global.asax definierten Routen unter Umständen die gleiche Anforderung übereinstimmt. Es ist wichtig zu verstehen, dass eine Einschränkung verhindert, dass eine bestimmte Route Abgleich eine Anforderung, und nicht alle Routen, die in der Datei "Global.asax" definiert.

Beachten Sie, dass die Standardroute aus der Datei "Global.asax" in Liste 2 auskommentiert worden ist. Wenn Sie die Standardroute einschließen, entsprechen die Standardroute Anforderungen für den Admin-Controller. In diesem Fall können Remotebenutzer weiterhin Aktionen des Controllers Admin aufrufen, obwohl ihre Anforderungen die Admin-Route übereinstimmen würde nicht.

> [!div class="step-by-step"]
> [Zurück](creating-a-route-constraint-cs.md)
> [Weiter](asp-net-mvc-controller-overview-vb.md)
