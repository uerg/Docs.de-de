---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
title: Erstellen eine benutzerdefinierte Route-Einschränkung (VB) | Microsoft Docs
author: StephenWalther
description: Stephen Walther wird veranschaulicht, wie Sie eine benutzerdefinierte routeneinschränkung erstellen können. Implementieren wir eine einfache benutzerdefinierte Einschränkung, die verhindert, dass eine Route wird abgeglichen, w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: 892edb27-1cc2-4eaf-8314-dbc2efc6228a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-custom-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 094077fa0cb546f4cc91dbf074f8014e62b3b19c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867498"
---
<a name="creating-a-custom-route-constraint-vb"></a>Erstellen eine benutzerdefinierte Route-Einschränkung (VB)
====================
durch [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther wird veranschaulicht, wie Sie eine benutzerdefinierte routeneinschränkung erstellen können. Implementieren wir eine einfache benutzerdefinierte Einschränkung, die verhindert, dass eine Route abgeglichen wird, wenn der Browser von einem Remotecomputer angefordert werden.


Das Ziel dieses Lernprogramms wird veranschaulicht, wie Sie eine benutzerdefinierte routeneinschränkung erstellen können. Eine benutzerdefinierte routeneinschränkung können Sie verhindern, dass eine Route abgeglichen wird, es sei denn, eine benutzerdefinierte Bedingung verglichen wird.

In diesem Lernprogramm erstellen wir eine routeneinschränkung "localhost". Die "localhost" routeneinschränkung entspricht nur Anforderungen, die von dem lokalen Computer. Remoteanforderungen aus über das Internet werden nicht verglichen.

Implementieren Sie eine benutzerdefinierte routeneinschränkung durch Implementieren der Schnittstelle IRouteConstraint. Hierbei handelt es sich um eine sehr einfache Schnittstelle die eine einzelne Methode beschreibt:

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample1.vb)]

Die Methode gibt einen booleschen Wert zurück. Wenn Sie auf "false" zurückgeben, überein nicht die Route mit der Einschränkung verknüpft sind die Browseranforderung.

Die Einschränkung "localhost" ist im Codebeispiel 1 enthalten.

**1 – LocalhostConstraint.vb auflisten**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample2.vb)]

Die Einschränkung in der Liste 1 nutzt die IsLocal-Eigenschaft, die von der HttpRequest-Klasse verfügbar gemacht werden. Diese Eigenschaft gibt "true" zurück, wenn die IP-Adresse der Anforderung entweder 127.0.0.1 ist oder wenn die IP-Adresse der Anforderung mit der IP-Adresse des Servers übereinstimmt.

Sie verwenden eine benutzerdefinierte Einschränkung in einer Route, die in der Datei "Global.asax" definiert. Die Datei "Global.asax" in der Liste 2 verwendet die Einschränkung "localhost" um zu verhindern, dass jeder Benutzer eine Seite "Admin" angefordert, es sei denn, sie stellen Sie die Anforderung vom lokalen Server. Eine Anforderung für /Admin/DeleteAll schlägt beispielsweise fehl, wenn von einem Remoteserver hergestellt.

**Auflisten von 2 – "Global.asax"**

[!code-vb[Main](creating-a-custom-route-constraint-vb/samples/sample3.vb)]

Die Einschränkung "localhost" wird in der Definition der Admin-Route verwendet. Diese Route wird nicht von einer remote-Browser-Anforderung zugeordnet werden. Beachten Sie jedoch, dass andere Routen in Global.asax definierten u. u. mit derselben Anforderung übereinstimmt. Es ist wichtig zu verstehen, dass eine Einschränkung verhindert, dass eine bestimmte Route eine Anforderung gefunden und nicht alle Routen in der Datei "Global.asax" definiert.

Beachten Sie, dass die Standardroute aus der Datei "Global.asax" auflisten 2 auskommentiert worden ist. Wenn Sie die Standardroute einschließen, entsprechen die Standardroute Anforderungen für den Admin-Controller. In diesem Fall aufrufen Remotebenutzer, obwohl ihre Anforderungen die Admin-Route übereinstimmen würde nicht weiterhin Aktionen des Admin-Controllers.

> [!div class="step-by-step"]
> [Vorherige](creating-a-route-constraint-vb.md)
