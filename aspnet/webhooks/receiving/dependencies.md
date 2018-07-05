---
uid: webhooks/receiving/dependencies
title: Empfängerabhängigkeiten ASP.NET-WebHooks | Microsoft-Dokumentation
author: rick-anderson
description: Empfängerabhängigkeiten und Abhängigkeitsinjektion in ASP.NET WebHooks.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 05dcfac121e7974fd83c5b3736616479574944a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817836"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a><span data-ttu-id="25fef-103">ASP.NET WebHooks empfängerabhängigkeiten</span><span class="sxs-lookup"><span data-stu-id="25fef-103">ASP.NET WebHooks receiver dependencies</span></span>

<span data-ttu-id="25fef-104">Microsoft ASP.NET WebHooks dient über Dependency Injection, denken Sie daran.</span><span class="sxs-lookup"><span data-stu-id="25fef-104">Microsoft ASP.NET WebHooks is designed with dependency injection in mind.</span></span> <span data-ttu-id="25fef-105">Die meisten Abhängigkeiten in das System können durch alternative Implementierung, die mithilfe eines Dependency Injection ersetzt werden.</span><span class="sxs-lookup"><span data-stu-id="25fef-105">Most dependencies in the system can be replaced with alternative implementations using a dependency injection engine.</span></span>

<span data-ttu-id="25fef-106">Informieren Sie sich [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) für eine Liste der empfängerabhängigkeiten.</span><span class="sxs-lookup"><span data-stu-id="25fef-106">Please see [DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs) for a list of receiver dependencies.</span></span> <span data-ttu-id="25fef-107">Wenn keine Abhängigkeit registriert wurde, wird eine standardmäßige Implementierung verwendet.</span><span class="sxs-lookup"><span data-stu-id="25fef-107">If no dependency has been registered, a default implementation is used.</span></span> <span data-ttu-id="25fef-108">Informieren Sie sich [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) eine Liste der standardimplementierungen.</span><span class="sxs-lookup"><span data-stu-id="25fef-108">Please see [ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs) for a list of default implementations.</span></span>
