---
title: ASP.NET Core-Modul
author: rick-anderson
description: Erfahren Sie, wie der Kestrel-Webserver durch das ASP.NET Core-Modul IIS oder IIS Express als Reverseproxyserver zu verwenden.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/23/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 789b0b2e74405256bb0e247ae5ea9697e3cb28e5
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-module"></a><span data-ttu-id="72a9b-103">ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="72a9b-103">ASP.NET Core Module</span></span>

<span data-ttu-id="72a9b-104">Von [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) und [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="72a9b-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span> 

<span data-ttu-id="72a9b-105">Mit dem ASP.NET Core-Modul können ASP.NET Core-Apps hinter IIS in einer Reverseproxy-Konfiguration ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="72a9b-105">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="72a9b-106">IIS bietet mehr Sicherheit für Web-Apps und Features für die Verwaltbarkeit.</span><span class="sxs-lookup"><span data-stu-id="72a9b-106">IIS provides advanced web app security and manageability features.</span></span>

<span data-ttu-id="72a9b-107">Unterstützte Windows-Versionen:</span><span class="sxs-lookup"><span data-stu-id="72a9b-107">Supported Windows versions:</span></span>

* <span data-ttu-id="72a9b-108">Windows 7 oder höher</span><span class="sxs-lookup"><span data-stu-id="72a9b-108">Windows 7 or later</span></span>
* <span data-ttu-id="72a9b-109">Windows Server 2008 R2 oder höher</span><span class="sxs-lookup"><span data-stu-id="72a9b-109">Windows Server 2008 R2 or later</span></span>

<span data-ttu-id="72a9b-110">Das ASP.NET Core-Modul funktioniert nur mit Kestrel.</span><span class="sxs-lookup"><span data-stu-id="72a9b-110">The ASP.NET Core Module only works with Kestrel.</span></span> <span data-ttu-id="72a9b-111">Das Modul ist nicht kompatibel mit [HTTP.sys](xref:fundamentals/servers/httpsys) (ehemals [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="72a9b-111">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

## <a name="aspnet-core-module-description"></a><span data-ttu-id="72a9b-112">Beschreibung des ASP.NET Core-Moduls</span><span class="sxs-lookup"><span data-stu-id="72a9b-112">ASP.NET Core Module description</span></span>

<span data-ttu-id="72a9b-113">Das ASP.NET Core-Modul ist ein natives IIS-Modul, das in die IIS-Pipeline integriert wird, um Webanforderungen an Back-End-ASP.NET Core-Apps umzuleiten.</span><span class="sxs-lookup"><span data-stu-id="72a9b-113">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to redirect web requests to backend ASP.NET Core apps.</span></span> <span data-ttu-id="72a9b-114">Viele native Module, z.B. die Windows-Authentifizierung, bleiben aktiv.</span><span class="sxs-lookup"><span data-stu-id="72a9b-114">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="72a9b-115">Weitere Informationen zu IIS-Modulen, die im Modul aktiv sind, finden Sie unter [IIS modules (IIS-Module)](xref:host-and-deploy/iis/modules).</span><span class="sxs-lookup"><span data-stu-id="72a9b-115">To learn more about IIS modules active with the module, see [IIS modules](xref:host-and-deploy/iis/modules).</span></span>

<span data-ttu-id="72a9b-116">Da ASP.NET Core-Apps in einem Prozess getrennt vom IIS-Arbeitsprozess ausgeführt werden, führt das Modul auch Prozessverwaltung durch.</span><span class="sxs-lookup"><span data-stu-id="72a9b-116">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="72a9b-117">Das Modul startet den Prozess für die ASP.NET Core-App, wenn die erste Anforderung eingeht und startet die App neu, wenn sie abstürzt.</span><span class="sxs-lookup"><span data-stu-id="72a9b-117">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="72a9b-118">Dies ist im Prinzip das gleiche Verhalten wie bei ASP.NET 4.x-Apps, die prozessintern in IIS ausgeführt und durch [Windows Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="72a9b-118">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="72a9b-119">Das folgende Diagramm zeigt die Beziehung zwischen IIS, dem ASP.NET Core-Modul und ASP.NET Core-Apps:</span><span class="sxs-lookup"><span data-stu-id="72a9b-119">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and ASP.NET Core apps:</span></span>

![ASP.NET Core-Modul](aspnet-core-module/_static/ancm.png)

<span data-ttu-id="72a9b-121">Anforderungen gehen aus dem Internet an den Treiber „HTTP.sys“ ein, der im Kernelmodus betrieben wird.</span><span class="sxs-lookup"><span data-stu-id="72a9b-121">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="72a9b-122">Der Treiber leitet die Anforderungen an IIS auf dem konfigurierten Port der Webseite weiter, normalerweise 80 (HTTP) oder 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="72a9b-122">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="72a9b-123">Das Modul leitet die Anforderung an Kestrel auf einem zufälligen Port der App weiter, der nicht Port 80 oder 443 entspricht.</span><span class="sxs-lookup"><span data-stu-id="72a9b-123">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="72a9b-124">Das Modul gibt den Port über die Umgebungsvariable beim Start an. Die Middleware für die Integration von IIS konfiguriert den Server so, dass er auf `http://localhost:{port}` lauscht.</span><span class="sxs-lookup"><span data-stu-id="72a9b-124">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="72a9b-125">Zusätzliche Überprüfungen werden durchgeführt. Anforderungen, die nicht vom Modul stammen, werden abgelehnt.</span><span class="sxs-lookup"><span data-stu-id="72a9b-125">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="72a9b-126">Das Modul unterstützt die HTTPS-Weiterleitung nicht. Deshalb werden Anforderungen über HTTP weitergeleitet, selbst wenn sie von IIS über HTTPS empfangen wurden.</span><span class="sxs-lookup"><span data-stu-id="72a9b-126">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="72a9b-127">Nachdem Kestrel eine Anforderung vom Modul erhalten hat, wird die Anforderung in die Middleware-Pipeline von ASP.NET Core aufgenommen.</span><span class="sxs-lookup"><span data-stu-id="72a9b-127">After Kestrel picks up a request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="72a9b-128">Die Middleware-Pipeline behandelt die Anforderung und gibt sie als `HttpContext`-Instanz an die App-Logik weiter.</span><span class="sxs-lookup"><span data-stu-id="72a9b-128">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="72a9b-129">Die Antwort der App wird dann an IIS zurückgegeben, wo sie per Push an den HTTP-Client zurückgegeben wird, der die Anforderung initiiert hat.</span><span class="sxs-lookup"><span data-stu-id="72a9b-129">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

<span data-ttu-id="72a9b-130">Das ASP.NET Core-Modul hat noch andere Funktionen.</span><span class="sxs-lookup"><span data-stu-id="72a9b-130">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="72a9b-131">Das Modul kann:</span><span class="sxs-lookup"><span data-stu-id="72a9b-131">The module can:</span></span>

* <span data-ttu-id="72a9b-132">Umgebungsvariablen für den Arbeitsprozess festlegen</span><span class="sxs-lookup"><span data-stu-id="72a9b-132">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="72a9b-133">Die stdout-Ausgabe im Dateispeicher protokollieren, um Probleme beim Start zu beheben</span><span class="sxs-lookup"><span data-stu-id="72a9b-133">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="72a9b-134">Windows-Authentifizierungstoken weiterleiten</span><span class="sxs-lookup"><span data-stu-id="72a9b-134">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="72a9b-135">So installieren und verwenden Sie das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="72a9b-135">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="72a9b-136">Detaillierte Anweisungen zum Installieren und Verwenden des ASP.NET Core-Moduls finden Sie unter [Hosten von ASP.NET Core unter Windows mit IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="72a9b-136">For detailed instructions on how to install and use the ASP.NET Core Module, see [Host on Windows with IIS](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="72a9b-137">Informationen zum Konfigurieren des Moduls finden Sie unter [ASP.NET Core Module configuration reference (Konfigurationsreferenz für das ASP.NET Core-Modul)](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="72a9b-137">For information on configuring the module, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="72a9b-138">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="72a9b-138">Additional resources</span></span>

* [<span data-ttu-id="72a9b-139">Hosten unter Windows mit IIS</span><span class="sxs-lookup"><span data-stu-id="72a9b-139">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="72a9b-140">Konfigurationsreferenz für das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="72a9b-140">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="72a9b-141">ASP.NET Core Module GitHub repository (source code) (ASP.NET Core-Modul GitHub-Repository (Quellcode))</span><span class="sxs-lookup"><span data-stu-id="72a9b-141">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
