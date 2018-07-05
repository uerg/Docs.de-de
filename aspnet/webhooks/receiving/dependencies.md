---
uid: webhooks/receiving/dependencies
title: Empfängerabhängigkeiten ASP.NET-WebHooks | Microsoft-Dokumentation
author: rick-anderson
description: Empfängerabhängigkeiten und Abhängigkeitsinjektion in ASP.NET WebHooks.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.openlocfilehash: cf45e3d2e45e4b7882b42d9aa0931e18c08f76b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37401183"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="bb718-103">ASP.NET WebHooks empfängerabhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="bb718-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="bb718-104">Microsoft ASP.NET WebHooks dient über Dependency Injection, denken Sie daran.</span><span class="sxs-lookup"><span data-stu-id="bb718-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="bb718-105">Die meisten Abhängigkeiten in das System können durch alternative Implementierung, die mithilfe eines Dependency Injection ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="bb718-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="bb718-106">Informieren Sie sich [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) für eine Liste der empfängerabhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="bb718-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="bb718-107">Wenn keine Abhängigkeit registriert wurde, wird eine standardmäßige Implementierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="bb718-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="bb718-108">Informieren Sie sich [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) eine Liste der standardimplementierungen.</span><span class="sxs-lookup"><span data-stu-id="bb718-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
