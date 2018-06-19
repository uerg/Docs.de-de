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
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="1f005-103">ASP.NET WebHooks Empfänger Abhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="1f005-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="1f005-104">Microsoft ASP.NET WebHooks ist mit Abhängigkeitsinjektion Bedenken ausgelegt.</span><span class="sxs-lookup"><span data-stu-id="1f005-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="1f005-105">Die meisten Abhängigkeiten im System können mit anderen Implementierungen, die mit einem Dependency Injection-Modul ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="1f005-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="1f005-106">Finden Sie unter [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) eine Liste der Empfänger Abhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="1f005-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="1f005-107">Wenn keine Abhängigkeit registriert wurde, wird eine standardmäßige Implementierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="1f005-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="1f005-108">Finden Sie unter [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) eine Liste der standardmäßigen Implementierungen.</span><span class="sxs-lookup"><span data-stu-id="1f005-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
