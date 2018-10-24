---
title: ASP.NET Core-Modul
author: rick-anderson
description: Erfahren Sie, wie der Kestrel-Webserver durch das ASP.NET Core-Modul IIS oder IIS Express als Reverseproxyserver zu verwenden.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/21/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: 2f73a34b7d311c9e98ad2ecba11894d27bb2aa4d
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/10/2018
ms.locfileid: "48910888"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="14b4c-103">ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="14b4c-103">ASP.NET Core Module</span></span>

<span data-ttu-id="14b4c-104">Von [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) und [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="14b4c-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="14b4c-105">Mit dem ASP.NET Core-Modul können ASP.NET Core-Apps in einem IIS-Arbeitsprozess (*In-Process*) oder hinter IIS in einer Reverseproxy-Konfiguration (*Out-of-Process*) ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="14b4c-105">The ASP.NET Core Module allows ASP.NET Core apps to run in an IIS worker process (*in-process*) or behind IIS in a reverse proxy configuration (*out-of-process*).</span></span> <span data-ttu-id="14b4c-106">IIS bietet mehr Sicherheit für Web-Apps und Features für die Verwaltbarkeit.</span><span class="sxs-lookup"><span data-stu-id="14b4c-106">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="14b4c-107">Mit dem ASP.NET Core-Modul können ASP.NET Core-Apps hinter IIS in einer Reverseproxy-Konfiguration ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="14b4c-107">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="14b4c-108">IIS bietet mehr Sicherheit für Web-Apps und Features für die Verwaltbarkeit.</span><span class="sxs-lookup"><span data-stu-id="14b4c-108">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

<span data-ttu-id="14b4c-109">Unterstützte Windows-Versionen:</span><span class="sxs-lookup"><span data-stu-id="14b4c-109">Supported Windows versions:</span></span>

* <span data-ttu-id="14b4c-110">Windows 7 oder höher</span><span class="sxs-lookup"><span data-stu-id="14b4c-110">Windows 7 or later</span></span>
* <span data-ttu-id="14b4c-111">Windows Server 2008 R2 oder höher</span><span class="sxs-lookup"><span data-stu-id="14b4c-111">Windows Server 2008 R2 or later</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="14b4c-112">Beim In-Process-Hosting verfügt das Modul über eine eigene Serverimplementierung, `IISHttpServer`.</span><span class="sxs-lookup"><span data-stu-id="14b4c-112">When hosting in-process, the module has its own server implementation, `IISHttpServer`.</span></span>

<span data-ttu-id="14b4c-113">Beim Out-of-Process-Hosting funktioniert das Modul nur mit Kestrel.</span><span class="sxs-lookup"><span data-stu-id="14b4c-113">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="14b4c-114">Das Modul ist nicht kompatibel mit [HTTP.sys](xref:fundamentals/servers/httpsys) (ehemals [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="14b4c-114">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="14b4c-115">Das Modul funktioniert nur mit Kestrel.</span><span class="sxs-lookup"><span data-stu-id="14b4c-115">The module only works with Kestrel.</span></span> <span data-ttu-id="14b4c-116">Das Modul ist nicht kompatibel mit [HTTP.sys](xref:fundamentals/servers/httpsys) (ehemals [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="14b4c-116">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

## <a name="aspnet-core-module-description"></a><span data-ttu-id="14b4c-117">Beschreibung des ASP.NET Core-Moduls</span><span class="sxs-lookup"><span data-stu-id="14b4c-117">ASP.NET Core Module description</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="14b4c-118">Das ASP.NET Core-Modul ist ein natives IIS-Modul, das in die IIS-Pipeline integriert wird, um eine dieser Aktionen auszuführen:</span><span class="sxs-lookup"><span data-stu-id="14b4c-118">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="14b4c-119">Hosten einer ASP.NET Core-App innerhalb des IIS-Arbeitsprozesses (`w3wp.exe`), was als [In-Process-Hostingmodell](#in-process-hosting-model) bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="14b4c-119">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>

* <span data-ttu-id="14b4c-120">Weiterleiten von Webanforderungen an eine Back-End-ASP.NET Core-App, die den [Kestrel-Server](xref:fundamentals/servers/kestrel) ausführt, was als [Out-of-Process-Hostingmodell](#out-of-process-hosting-model) bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="14b4c-120">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="14b4c-121">In-Process-Hostingmodell</span><span class="sxs-lookup"><span data-stu-id="14b4c-121">In-process hosting model</span></span>

<span data-ttu-id="14b4c-122">Beim Einsatz von In-Process-Hosting wird eine ASP.NET Core-App im gleichen Prozess wie ihr IIS-Arbeitsprozess ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="14b4c-122">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="14b4c-123">Dadurch wird die Leistungseinbuße der Proxyumleitung von Anforderungen über den Loopbackadapter vermieden, die das Out-of-Process-Hostingmodell mit sich bringt.</span><span class="sxs-lookup"><span data-stu-id="14b4c-123">This removes the performance penalty of proxying requests over the loopback adapter when using the out-of-process hosting model.</span></span> <span data-ttu-id="14b4c-124">IIS erledigt das Prozessmanagement mit dem [Windows-Prozessaktivierungsdienst (Process Activation Service, WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="14b4c-124">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="14b4c-125">Das ASP.NET Core-Modul:</span><span class="sxs-lookup"><span data-stu-id="14b4c-125">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="14b4c-126">Führt die Initialisierung von Apps aus.</span><span class="sxs-lookup"><span data-stu-id="14b4c-126">Performs app initialization.</span></span>
  * <span data-ttu-id="14b4c-127">Lädt die [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="14b4c-127">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="14b4c-128">Ruft `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="14b4c-128">Calls `Program.Main`.</span></span>
* <span data-ttu-id="14b4c-129">Behandelt die Lebensdauer der nativen IIS-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="14b4c-129">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="14b4c-130">Das folgende Diagramm zeigt die Beziehung zwischen IIS, dem ASP.NET Core-Modul und einer In-Process gehosteten App:</span><span class="sxs-lookup"><span data-stu-id="14b4c-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![ASP.NET Core-Modul](aspnet-core-module/_static/ancm-inprocess.png)

<span data-ttu-id="14b4c-132">Eine Anforderung geht aus dem Web beim HTTP.sys-Treiber im Kernelmodus ein.</span><span class="sxs-lookup"><span data-stu-id="14b4c-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="14b4c-133">Der Treiber leitet die native Anforderung an IIS auf dem konfigurierten Port der Webseite weiter, normalerweise 80 (HTTP) oder 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="14b4c-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="14b4c-134">Das Modul empfängt die native Anforderung und übergibt die Steuerung an `IISHttpServer`, wodurch die Anforderung aus einer nativen in eine verwaltete Anforderung überführt wird.</span><span class="sxs-lookup"><span data-stu-id="14b4c-134">The module receives the native request and passes control to `IISHttpServer`, which is what converts the request from native to managed.</span></span>

<span data-ttu-id="14b4c-135">Nachdem `IISHttpServer` die Anforderung erhalten hat, wird die Anforderung in die Middleware-Pipeline von ASP.NET Core eingestellt.</span><span class="sxs-lookup"><span data-stu-id="14b4c-135">After `IISHttpServer` picks up the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="14b4c-136">Die Middleware-Pipeline behandelt die Anforderung und gibt sie als `HttpContext`-Instanz an die App-Logik weiter.</span><span class="sxs-lookup"><span data-stu-id="14b4c-136">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="14b4c-137">Die Antwort der App wird dann an IIS zurückgegeben, wo sie per Push an den HTTP-Client zurückgegeben wird, der die Anforderung initiiert hat.</span><span class="sxs-lookup"><span data-stu-id="14b4c-137">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="14b4c-138">Out-of-Process-Hostingmodell</span><span class="sxs-lookup"><span data-stu-id="14b4c-138">Out-of-process hosting model</span></span>

<span data-ttu-id="14b4c-139">Da ASP.NET Core-Apps in einem Prozess getrennt vom IIS-Arbeitsprozess ausgeführt werden, führt das Modul die Prozessverwaltung durch.</span><span class="sxs-lookup"><span data-stu-id="14b4c-139">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="14b4c-140">Das Modul startet den Prozess für die ASP.NET Core-App, wenn die erste Anforderung eingeht und startet die App neu, wenn sie heruntergefahren wird oder abstürzt.</span><span class="sxs-lookup"><span data-stu-id="14b4c-140">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="14b4c-141">Dies ist im Prinzip das gleiche Verhalten wie bei Apps, die prozessintern ausgeführt und durch den [Windows-Prozessaktivierungsdienst (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="14b4c-141">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="14b4c-142">Das folgende Diagramm zeigt die Beziehung zwischen IIS, dem ASP.NET Core-Modul und einer Out-of-Process gehosteten App:</span><span class="sxs-lookup"><span data-stu-id="14b4c-142">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core-Modul](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="14b4c-144">Anforderungen gehen aus dem Internet an den Treiber „HTTP.sys“ ein, der im Kernelmodus betrieben wird.</span><span class="sxs-lookup"><span data-stu-id="14b4c-144">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="14b4c-145">Der Treiber leitet die Anforderungen an IIS auf dem konfigurierten Port der Webseite weiter, normalerweise 80 (HTTP) oder 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="14b4c-145">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="14b4c-146">Das Modul leitet die Anforderung an Kestrel auf einem zufälligen Port der App weiter, der nicht Port 80 oder 443 entspricht.</span><span class="sxs-lookup"><span data-stu-id="14b4c-146">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="14b4c-147">Das Modul gibt den Port über die Umgebungsvariable beim Start an. Die Middleware für die Integration von IIS konfiguriert den Server so, dass er auf `http://localhost:{port}` lauscht.</span><span class="sxs-lookup"><span data-stu-id="14b4c-147">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="14b4c-148">Zusätzliche Überprüfungen werden durchgeführt. Anforderungen, die nicht vom Modul stammen, werden abgelehnt.</span><span class="sxs-lookup"><span data-stu-id="14b4c-148">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="14b4c-149">Das Modul unterstützt die HTTPS-Weiterleitung nicht. Deshalb werden Anforderungen über HTTP weitergeleitet, selbst wenn sie von IIS über HTTPS empfangen wurden.</span><span class="sxs-lookup"><span data-stu-id="14b4c-149">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="14b4c-150">Nachdem Kestrel die Anforderung vom Modul erhalten hat, wird die Anforderung in die Middleware-Pipeline von ASP.NET Core eingestellt.</span><span class="sxs-lookup"><span data-stu-id="14b4c-150">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="14b4c-151">Die Middleware-Pipeline behandelt die Anforderung und gibt sie als `HttpContext`-Instanz an die App-Logik weiter.</span><span class="sxs-lookup"><span data-stu-id="14b4c-151">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="14b4c-152">Die durch IIS-Integration hinzugefügte Middleware aktualisiert das Schema, die Remote-IP und die Pfadbasis, um der Weiterleitung der Anforderung an Kestrel Rechnung zu tragen.</span><span class="sxs-lookup"><span data-stu-id="14b4c-152">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="14b4c-153">Die Antwort der App wird dann an IIS zurückgegeben, wo sie per Push an den HTTP-Client zurückgegeben wird, der die Anforderung initiiert hat.</span><span class="sxs-lookup"><span data-stu-id="14b4c-153">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="14b4c-154">Das ASP.NET Core-Modul ist ein natives IIS-Modul, das in die IIS-Pipeline integriert wird, um Webanforderungen an Back-End-ASP.NET Core-Apps weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="14b4c-154">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="14b4c-155">Da ASP.NET Core-Apps in einem Prozess getrennt vom IIS-Arbeitsprozess ausgeführt werden, führt das Modul auch Prozessverwaltung durch.</span><span class="sxs-lookup"><span data-stu-id="14b4c-155">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="14b4c-156">Das Modul startet den Prozess für die ASP.NET Core-App, wenn die erste Anforderung eingeht und startet die App neu, wenn sie abstürzt.</span><span class="sxs-lookup"><span data-stu-id="14b4c-156">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="14b4c-157">Dies ist im Prinzip das gleiche Verhalten wie bei ASP.NET 4.x-Apps, die prozessintern in IIS ausgeführt und durch [Windows Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="14b4c-157">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="14b4c-158">Das folgende Diagramm zeigt die Beziehung zwischen IIS, dem ASP.NET Core-Modul und einer App:</span><span class="sxs-lookup"><span data-stu-id="14b4c-158">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core-Modul](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="14b4c-160">Anforderungen gehen aus dem Internet an den Treiber „HTTP.sys“ ein, der im Kernelmodus betrieben wird.</span><span class="sxs-lookup"><span data-stu-id="14b4c-160">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="14b4c-161">Der Treiber leitet die Anforderungen an IIS auf dem konfigurierten Port der Webseite weiter, normalerweise 80 (HTTP) oder 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="14b4c-161">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="14b4c-162">Das Modul leitet die Anforderung an Kestrel auf einem zufälligen Port der App weiter, der nicht Port 80 oder 443 entspricht.</span><span class="sxs-lookup"><span data-stu-id="14b4c-162">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="14b4c-163">Das Modul gibt den Port über die Umgebungsvariable beim Start an. Die Middleware für die Integration von IIS konfiguriert den Server so, dass er auf `http://localhost:{port}` lauscht.</span><span class="sxs-lookup"><span data-stu-id="14b4c-163">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="14b4c-164">Zusätzliche Überprüfungen werden durchgeführt. Anforderungen, die nicht vom Modul stammen, werden abgelehnt.</span><span class="sxs-lookup"><span data-stu-id="14b4c-164">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="14b4c-165">Das Modul unterstützt die HTTPS-Weiterleitung nicht. Deshalb werden Anforderungen über HTTP weitergeleitet, selbst wenn sie von IIS über HTTPS empfangen wurden.</span><span class="sxs-lookup"><span data-stu-id="14b4c-165">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="14b4c-166">Nachdem Kestrel die Anforderung vom Modul erhalten hat, wird die Anforderung in die Middleware-Pipeline von ASP.NET Core eingestellt.</span><span class="sxs-lookup"><span data-stu-id="14b4c-166">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="14b4c-167">Die Middleware-Pipeline behandelt die Anforderung und gibt sie als `HttpContext`-Instanz an die App-Logik weiter.</span><span class="sxs-lookup"><span data-stu-id="14b4c-167">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="14b4c-168">Die durch IIS-Integration hinzugefügte Middleware aktualisiert das Schema, die Remote-IP und die Pfadbasis, um der Weiterleitung der Anforderung an Kestrel Rechnung zu tragen.</span><span class="sxs-lookup"><span data-stu-id="14b4c-168">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="14b4c-169">Die Antwort der App wird dann an IIS zurückgegeben, wo sie per Push an den HTTP-Client zurückgegeben wird, der die Anforderung initiiert hat.</span><span class="sxs-lookup"><span data-stu-id="14b4c-169">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="14b4c-170">Viele native Module, z.B. die Windows-Authentifizierung, bleiben aktiv.</span><span class="sxs-lookup"><span data-stu-id="14b4c-170">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="14b4c-171">Weitere Informationen zu IIS-Modulen, die im Modul aktiv sind, finden Sie unter <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="14b4c-171">To learn more about IIS modules active with the module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="14b4c-172">Das ASP.NET Core-Modul hat noch andere Funktionen.</span><span class="sxs-lookup"><span data-stu-id="14b4c-172">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="14b4c-173">Das Modul kann:</span><span class="sxs-lookup"><span data-stu-id="14b4c-173">The module can:</span></span>

* <span data-ttu-id="14b4c-174">Umgebungsvariablen für den Arbeitsprozess festlegen</span><span class="sxs-lookup"><span data-stu-id="14b4c-174">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="14b4c-175">Die stdout-Ausgabe im Dateispeicher protokollieren, um Probleme beim Start zu beheben</span><span class="sxs-lookup"><span data-stu-id="14b4c-175">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="14b4c-176">Windows-Authentifizierungstoken weiterleiten</span><span class="sxs-lookup"><span data-stu-id="14b4c-176">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="14b4c-177">So installieren und verwenden Sie das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="14b4c-177">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="14b4c-178">Detaillierte Anweisungen zum Installieren und Verwenden des ASP.NET Core-Moduls finden Sie unter <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="14b4c-178">For detailed instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span> <span data-ttu-id="14b4c-179">Informationen zum Konfigurieren des Moduls finden Sie unter <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="14b4c-179">For information on configuring the module, see the <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="14b4c-180">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="14b4c-180">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [<span data-ttu-id="14b4c-181">ASP.NET Core Module GitHub repository (source code) (ASP.NET Core-Modul GitHub-Repository (Quellcode))</span><span class="sxs-lookup"><span data-stu-id="14b4c-181">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
