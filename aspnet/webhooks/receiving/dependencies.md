---
uid: webhooks/receiving/dependencies
title: ASP.NET WebHooks Empfänger Abhängigkeiten | Microsoft Docs
author: rick-anderson
description: Empfänger-Abhängigkeiten und Abhängigkeitsinjektion in ASP.NET WebHooks.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529909"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>ASP.NET WebHooks Empfänger Abhängigkeiten

Microsoft ASP.NET WebHooks ist mit Abhängigkeitsinjektion Bedenken ausgelegt. Die meisten Abhängigkeiten im System können mit anderen Implementierungen, die mit einem Dependency Injection-Modul ersetzt werden.

Finden Sie unter [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) eine Liste der Empfänger Abhängigkeiten. Wenn keine Abhängigkeit registriert wurde, wird eine standardmäßige Implementierung verwendet. Finden Sie unter [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) eine Liste der standardmäßigen Implementierungen.
