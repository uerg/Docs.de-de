---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Erstellen einer benutzerdefinierten Routeneinschränkung (VB) | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther wird veranschaulicht, wie Sie eine benutzerdefinierten routeneinschränkung erstellen können. Implementieren wir ein einfaches benutzerdefiniertes Einschränkung, die verhindert, dass eine Route wird abgeglichen, w...
ms.author: riande
ms.date: 02/16/2009
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: d088380152adcb025857176b4396cab48fa64b66
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835779"
---
<a name="creating-a-custom-route-constraint-vb"></a>Erstellen einer benutzerdefinierten Routeneinschränkung (VB)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther wird veranschaulicht, wie Sie eine benutzerdefinierten routeneinschränkung erstellen können. Wir implementieren eine einfache benutzerdefinierte Einschränkung, die verhindert, dass eine Route wird eine Übereinstimmung, wenn der Browser von einem Remotecomputer angefordert werden.


Das Ziel in diesem Tutorial wird veranschaulicht, wie Sie eine benutzerdefinierten routeneinschränkung erstellen können. Eine benutzerdefinierte Route-Einschränkung können Sie verhindern, dass eine Route abgeglichen wird, es sei denn, eine benutzerdefinierte Bedingung erfüllt ist.

In diesem Tutorial erstellen wir eine routeneinschränkung "localhost". Die Localhost-routeneinschränkung entspricht nur Anforderungen, die von dem lokalen Computer. Remoteanforderungen aus über das Internet werden nicht verglichen.

Sie implementieren eine benutzerdefinierten routeneinschränkung durch Implementieren der IRouteConstraint-Schnittstelle. Dies ist eine sehr einfache Schnittstelle, die eine einzelne Methode beschrieben:

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

Die Methode gibt einen booleschen Wert zurück. Wenn Sie auf "false" zurückgeben, wird nicht die Route, die mit der Einschränkung verknüpft sind die Browseranforderung übereinstimmen.

Die Einschränkung "localhost", die in Codebeispiel 1 enthalten ist.

**1 – LocalhostConstraint.vb auflisten**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

Die Einschränkung in Codebeispiel 1 nutzt die IsLocal-Eigenschaft, die von den HttpRequest-Klasse verfügbar gemacht werden. Diese Eigenschaft gibt true zurück, wenn die IP-Adresse der Anforderung entweder 127.0.0.1 ist oder die IP-Adresse der Anforderung, die IP-Adresse des Servers identisch ist.

Sie verwenden eine benutzerdefinierte Einschränkung in einer Route, die in der Datei "Global.asax" definiert. Die Datei "Global.asax" in Liste 2 verwendet die Einschränkung "localhost", um zu verhindern, dass jeder Benutzer eine Seite "Administrator" angefordert, es sei denn, sie die Anforderung vom lokalen Server machen. Eine Anforderung für /Admin/DeleteAll schlägt beispielsweise fehl, wenn von einem Remoteserver hergestellt.

**Codebeispiel 2 – "Global.asax"**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

Die Einschränkung "localhost" wird in der Definition der Admin-Route verwendet. Diese Route wird nicht von einer remote-Browser-Anforderung zugeordnet werden. Beachten Sie jedoch, dass andere in Global.asax definierten Routen unter Umständen die gleiche Anforderung übereinstimmt. Es ist wichtig zu verstehen, dass eine Einschränkung verhindert, dass eine bestimmte Route Abgleich eine Anforderung, und nicht alle Routen, die in der Datei "Global.asax" definiert.

Beachten Sie, dass die Standardroute aus der Datei "Global.asax" in Liste 2 auskommentiert worden ist. Wenn Sie die Standardroute einschließen, entsprechen die Standardroute Anforderungen für den Admin-Controller. In diesem Fall können Remotebenutzer weiterhin Aktionen des Controllers Admin aufrufen, obwohl ihre Anforderungen die Admin-Route übereinstimmen würde nicht.

> [!div class="step-by-step"]
> [Vorherige](creating-a-route-constraint-vb.md)
