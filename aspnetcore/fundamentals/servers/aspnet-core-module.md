---
title: ASP.NET Core-Modul
author: guardrex
description: Erfahren Sie, wie der Kestrel-Webserver durch das ASP.NET Core-Modul IIS oder IIS Express verwenden kann.
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/30/2018
uid: fundamentals/servers/aspnet-core-module
ms.openlocfilehash: d3f3a42dd7aebc425905b865376a584bcf0e5153
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861458"
---
# <a name="aspnet-core-module"></a><span data-ttu-id="7e68e-103">ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="7e68e-103">ASP.NET Core Module</span></span>

<span data-ttu-id="7e68e-104">Von [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl) und [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="7e68e-104">By [Tom Dykstra](https://github.com/tdykstra), [Rick Strahl](https://github.com/RickStrahl), and [Chris Ross](https://github.com/Tratcher)</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7e68e-105">Mit dem ASP.NET Core-Modul können ASP.NET Core-Apps in einem IIS-Arbeitsprozess (*In-Process*) oder hinter IIS in einer Reverseproxy-Konfiguration (*Out-of-Process*) ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="7e68e-105">The ASP.NET Core Module allows ASP.NET Core apps to run in an IIS worker process (*in-process*) or behind IIS in a reverse proxy configuration (*out-of-process*).</span></span> <span data-ttu-id="7e68e-106">IIS bietet mehr Sicherheit für Web-Apps und Features für die Verwaltbarkeit.</span><span class="sxs-lookup"><span data-stu-id="7e68e-106">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7e68e-107">Mit dem ASP.NET Core-Modul können ASP.NET Core-Apps hinter IIS in einer Reverseproxy-Konfiguration ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="7e68e-107">The ASP.NET Core Module allows ASP.NET Core apps to run behind IIS in a reverse proxy configuration.</span></span> <span data-ttu-id="7e68e-108">IIS bietet mehr Sicherheit für Web-Apps und Features für die Verwaltbarkeit.</span><span class="sxs-lookup"><span data-stu-id="7e68e-108">IIS provides advanced web app security and manageability features.</span></span>

::: moniker-end

<span data-ttu-id="7e68e-109">Unterstützte Windows-Versionen:</span><span class="sxs-lookup"><span data-stu-id="7e68e-109">Supported Windows versions:</span></span>

* <span data-ttu-id="7e68e-110">Windows 7 oder höher</span><span class="sxs-lookup"><span data-stu-id="7e68e-110">Windows 7 or later</span></span>
* <span data-ttu-id="7e68e-111">Windows Server 2008 R2 oder höher</span><span class="sxs-lookup"><span data-stu-id="7e68e-111">Windows Server 2008 R2 or later</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7e68e-112">Beim In-Process-Hosting verwendet das Modul eine prozessinterne IIS-Serverimplementierung: IIS-HTTP-Server (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="7e68e-112">When hosting in-process, the module uses an IIS in-process server implementation, IIS HTTP Server (`IISHttpServer`).</span></span>

<span data-ttu-id="7e68e-113">Beim Out-of-Process-Hosting funktioniert das Modul nur mit Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7e68e-113">When hosting out-of-process, the module only works with Kestrel.</span></span> <span data-ttu-id="7e68e-114">Das Modul ist nicht kompatibel mit [HTTP.sys](xref:fundamentals/servers/httpsys) (ehemals [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="7e68e-114">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7e68e-115">Das Modul funktioniert nur mit Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7e68e-115">The module only works with Kestrel.</span></span> <span data-ttu-id="7e68e-116">Das Modul ist nicht kompatibel mit [HTTP.sys](xref:fundamentals/servers/httpsys) (ehemals [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="7e68e-116">The module is incompatible with [HTTP.sys](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener)).</span></span>

::: moniker-end

## <a name="aspnet-core-module-description"></a><span data-ttu-id="7e68e-117">Beschreibung des ASP.NET Core-Moduls</span><span class="sxs-lookup"><span data-stu-id="7e68e-117">ASP.NET Core Module description</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="7e68e-118">Das ASP.NET Core-Modul ist ein natives IIS-Modul, das in die IIS-Pipeline integriert wird, um eine dieser Aktionen auszuführen:</span><span class="sxs-lookup"><span data-stu-id="7e68e-118">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to either:</span></span>

* <span data-ttu-id="7e68e-119">Hosten einer ASP.NET Core-App innerhalb des IIS-Arbeitsprozesses (`w3wp.exe`), was als [In-Process-Hostingmodell](#in-process-hosting-model) bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="7e68e-119">Host an ASP.NET Core app inside of the IIS worker process (`w3wp.exe`), called the [in-process hosting model](#in-process-hosting-model).</span></span>

* <span data-ttu-id="7e68e-120">Weiterleiten von Webanforderungen an eine Back-End-ASP.NET Core-App, die den [Kestrel-Server](xref:fundamentals/servers/kestrel) ausführt, was als [Out-of-Process-Hostingmodell](#out-of-process-hosting-model) bezeichnet wird.</span><span class="sxs-lookup"><span data-stu-id="7e68e-120">Forward web requests to a backend ASP.NET Core app running the [Kestrel server](xref:fundamentals/servers/kestrel), called the [out-of-process hosting model](#out-of-process-hosting-model).</span></span>

### <a name="in-process-hosting-model"></a><span data-ttu-id="7e68e-121">In-Process-Hostingmodell</span><span class="sxs-lookup"><span data-stu-id="7e68e-121">In-process hosting model</span></span>

<span data-ttu-id="7e68e-122">Beim Einsatz von In-Process-Hosting wird eine ASP.NET Core-App im gleichen Prozess wie ihr IIS-Arbeitsprozess ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="7e68e-122">Using in-process hosting, an ASP.NET Core app runs in the same process as its IIS worker process.</span></span> <span data-ttu-id="7e68e-123">Dadurch wird die Leistungseinbuße der Proxyumleitung von Anforderungen über den Loopbackadapter vermieden, die das Out-of-Process-Hostingmodell mit sich bringt.</span><span class="sxs-lookup"><span data-stu-id="7e68e-123">This removes the performance penalty of proxying requests over the loopback adapter when using the out-of-process hosting model.</span></span> <span data-ttu-id="7e68e-124">IIS erledigt das Prozessmanagement mit dem [Windows-Prozessaktivierungsdienst (Process Activation Service, WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span><span class="sxs-lookup"><span data-stu-id="7e68e-124">IIS handles process management with the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="7e68e-125">Das ASP.NET Core-Modul:</span><span class="sxs-lookup"><span data-stu-id="7e68e-125">The ASP.NET Core Module:</span></span>

* <span data-ttu-id="7e68e-126">Führt die Initialisierung von Apps aus.</span><span class="sxs-lookup"><span data-stu-id="7e68e-126">Performs app initialization.</span></span>
  * <span data-ttu-id="7e68e-127">Lädt die [CoreCLR](/dotnet/standard/glossary#coreclr).</span><span class="sxs-lookup"><span data-stu-id="7e68e-127">Loads the [CoreCLR](/dotnet/standard/glossary#coreclr).</span></span>
  * <span data-ttu-id="7e68e-128">Ruft `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="7e68e-128">Calls `Program.Main`.</span></span>
* <span data-ttu-id="7e68e-129">Behandelt die Lebensdauer der nativen IIS-Anforderung.</span><span class="sxs-lookup"><span data-stu-id="7e68e-129">Handles the lifetime of the IIS native request.</span></span>

<span data-ttu-id="7e68e-130">Das folgende Diagramm zeigt die Beziehung zwischen IIS, dem ASP.NET Core-Modul und einer In-Process gehosteten App:</span><span class="sxs-lookup"><span data-stu-id="7e68e-130">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted in-process:</span></span>

![ASP.NET Core-Modul](aspnet-core-module/_static/ancm-inprocess.png)

<span data-ttu-id="7e68e-132">Eine Anforderung geht aus dem Web beim HTTP.sys-Treiber im Kernelmodus ein.</span><span class="sxs-lookup"><span data-stu-id="7e68e-132">A request arrives from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="7e68e-133">Der Treiber leitet die native Anforderung an IIS auf dem konfigurierten Port der Webseite weiter, normalerweise 80 (HTTP) oder 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="7e68e-133">The driver routes the native request to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="7e68e-134">Das Modul empfängt die native Anforderung und übergibt sie an den IIS-HTTP-Server (`IISHttpServer`).</span><span class="sxs-lookup"><span data-stu-id="7e68e-134">The module receives the native request and passes it to IIS HTTP Server (`IISHttpServer`).</span></span> <span data-ttu-id="7e68e-135">Der IIS-HTTP-Server ist eine prozessinterne IIS-Serverimplementierung, die die Anforderung vom nativen Modus in den verwalteten Modus konvertiert.</span><span class="sxs-lookup"><span data-stu-id="7e68e-135">IIS HTTP Server is an IIS in-process server implementation that converts the request from native to managed.</span></span>

<span data-ttu-id="7e68e-136">Nachdem der IIS-HTTP-Server die Anforderung verarbeitet hat, wird die Anforderung per Push an die Middlewarepipeline von ASP.NET Core übertragen.</span><span class="sxs-lookup"><span data-stu-id="7e68e-136">After the IIS HTTP Server processes the request, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="7e68e-137">Die Middleware-Pipeline behandelt die Anforderung und gibt sie als `HttpContext`-Instanz an die App-Logik weiter.</span><span class="sxs-lookup"><span data-stu-id="7e68e-137">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="7e68e-138">Die Antwort der App wird an IIS übergeben, von wo sie per Push an den Client zurückgegeben wird, der die Anforderung initiiert hat.</span><span class="sxs-lookup"><span data-stu-id="7e68e-138">The app's response is passed back to IIS, which pushes it back out to the client that initiated the request.</span></span>

### <a name="out-of-process-hosting-model"></a><span data-ttu-id="7e68e-139">Out-of-Process-Hostingmodell</span><span class="sxs-lookup"><span data-stu-id="7e68e-139">Out-of-process hosting model</span></span>

<span data-ttu-id="7e68e-140">Da ASP.NET Core-Apps in einem Prozess getrennt vom IIS-Arbeitsprozess ausgeführt werden, führt das Modul die Prozessverwaltung durch.</span><span class="sxs-lookup"><span data-stu-id="7e68e-140">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module handles process management.</span></span> <span data-ttu-id="7e68e-141">Das Modul startet den Prozess für die ASP.NET Core-App, wenn die erste Anforderung eingeht und startet die App neu, wenn sie heruntergefahren wird oder abstürzt.</span><span class="sxs-lookup"><span data-stu-id="7e68e-141">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it shuts down or crashes.</span></span> <span data-ttu-id="7e68e-142">Dies ist im Prinzip das gleiche Verhalten wie bei Apps, die prozessintern ausgeführt und durch den [Windows-Prozessaktivierungsdienst (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="7e68e-142">This is essentially the same behavior as seen with apps that run in-process that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="7e68e-143">Das folgende Diagramm zeigt die Beziehung zwischen IIS, dem ASP.NET Core-Modul und einer Out-of-Process gehosteten App:</span><span class="sxs-lookup"><span data-stu-id="7e68e-143">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app hosted out-of-process:</span></span>

![ASP.NET Core-Modul](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="7e68e-145">Anforderungen gehen aus dem Internet an den Treiber „HTTP.sys“ ein, der im Kernelmodus betrieben wird.</span><span class="sxs-lookup"><span data-stu-id="7e68e-145">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="7e68e-146">Der Treiber leitet die Anforderungen an IIS auf dem konfigurierten Port der Webseite weiter, normalerweise 80 (HTTP) oder 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="7e68e-146">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="7e68e-147">Das Modul leitet die Anforderung an Kestrel auf einem zufälligen Port der App weiter, der nicht Port 80 oder 443 entspricht.</span><span class="sxs-lookup"><span data-stu-id="7e68e-147">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="7e68e-148">Das Modul gibt den Port über die Umgebungsvariable beim Start an. Die Middleware für die Integration von IIS konfiguriert den Server so, dass er auf `http://localhost:{port}` lauscht.</span><span class="sxs-lookup"><span data-stu-id="7e68e-148">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="7e68e-149">Zusätzliche Überprüfungen werden durchgeführt. Anforderungen, die nicht vom Modul stammen, werden abgelehnt.</span><span class="sxs-lookup"><span data-stu-id="7e68e-149">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="7e68e-150">Das Modul unterstützt die HTTPS-Weiterleitung nicht. Deshalb werden Anforderungen über HTTP weitergeleitet, selbst wenn sie von IIS über HTTPS empfangen wurden.</span><span class="sxs-lookup"><span data-stu-id="7e68e-150">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="7e68e-151">Nachdem Kestrel die Anforderung vom Modul erhalten hat, wird die Anforderung in die Middleware-Pipeline von ASP.NET Core eingestellt.</span><span class="sxs-lookup"><span data-stu-id="7e68e-151">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="7e68e-152">Die Middleware-Pipeline behandelt die Anforderung und gibt sie als `HttpContext`-Instanz an die App-Logik weiter.</span><span class="sxs-lookup"><span data-stu-id="7e68e-152">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="7e68e-153">Die durch IIS-Integration hinzugefügte Middleware aktualisiert das Schema, die Remote-IP und die Pfadbasis, um der Weiterleitung der Anforderung an Kestrel Rechnung zu tragen.</span><span class="sxs-lookup"><span data-stu-id="7e68e-153">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="7e68e-154">Die Antwort der App wird dann an IIS zurückgegeben, wo sie per Push an den HTTP-Client zurückgegeben wird, der die Anforderung initiiert hat.</span><span class="sxs-lookup"><span data-stu-id="7e68e-154">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7e68e-155">Das ASP.NET Core-Modul ist ein natives IIS-Modul, das in die IIS-Pipeline integriert wird, um Webanforderungen an Back-End-ASP.NET Core-Apps weiterzuleiten.</span><span class="sxs-lookup"><span data-stu-id="7e68e-155">The ASP.NET Core Module is a native IIS module that plugs into the IIS pipeline to forward web requests to backend ASP.NET Core apps.</span></span>

<span data-ttu-id="7e68e-156">Da ASP.NET Core-Apps in einem Prozess getrennt vom IIS-Arbeitsprozess ausgeführt werden, führt das Modul auch Prozessverwaltung durch.</span><span class="sxs-lookup"><span data-stu-id="7e68e-156">Because ASP.NET Core apps run in a process separate from the IIS worker process, the module also handles process management.</span></span> <span data-ttu-id="7e68e-157">Das Modul startet den Prozess für die ASP.NET Core-App, wenn die erste Anforderung eingeht und startet die App neu, wenn sie abstürzt.</span><span class="sxs-lookup"><span data-stu-id="7e68e-157">The module starts the process for the ASP.NET Core app when the first request arrives and restarts the app if it crashes.</span></span> <span data-ttu-id="7e68e-158">Dies ist im Prinzip das gleiche Verhalten wie bei ASP.NET 4.x-Apps, die prozessintern in IIS ausgeführt und durch [Windows Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was) verwaltet werden.</span><span class="sxs-lookup"><span data-stu-id="7e68e-158">This is essentially the same behavior as seen with ASP.NET 4.x apps that run in-process in IIS that are managed by the [Windows Process Activation Service (WAS)](/iis/manage/provisioning-and-managing-iis/features-of-the-windows-process-activation-service-was).</span></span>

<span data-ttu-id="7e68e-159">Das folgende Diagramm zeigt die Beziehung zwischen IIS, dem ASP.NET Core-Modul und einer App:</span><span class="sxs-lookup"><span data-stu-id="7e68e-159">The following diagram illustrates the relationship between IIS, the ASP.NET Core Module, and an app:</span></span>

![ASP.NET Core-Modul](aspnet-core-module/_static/ancm-outofprocess.png)

<span data-ttu-id="7e68e-161">Anforderungen gehen aus dem Internet an den Treiber „HTTP.sys“ ein, der im Kernelmodus betrieben wird.</span><span class="sxs-lookup"><span data-stu-id="7e68e-161">Requests arrive from the web to the kernel-mode HTTP.sys driver.</span></span> <span data-ttu-id="7e68e-162">Der Treiber leitet die Anforderungen an IIS auf dem konfigurierten Port der Webseite weiter, normalerweise 80 (HTTP) oder 443 (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="7e68e-162">The driver routes the requests to IIS on the website's configured port, usually 80 (HTTP) or 443 (HTTPS).</span></span> <span data-ttu-id="7e68e-163">Das Modul leitet die Anforderung an Kestrel auf einem zufälligen Port der App weiter, der nicht Port 80 oder 443 entspricht.</span><span class="sxs-lookup"><span data-stu-id="7e68e-163">The module forwards the requests to Kestrel on a random port for the app, which isn't port 80/443.</span></span>

<span data-ttu-id="7e68e-164">Das Modul gibt den Port über die Umgebungsvariable beim Start an. Die Middleware für die Integration von IIS konfiguriert den Server so, dass er auf `http://localhost:{port}` lauscht.</span><span class="sxs-lookup"><span data-stu-id="7e68e-164">The module specifies the port via an environment variable at startup, and the IIS Integration Middleware configures the server to listen on `http://localhost:{port}`.</span></span> <span data-ttu-id="7e68e-165">Zusätzliche Überprüfungen werden durchgeführt. Anforderungen, die nicht vom Modul stammen, werden abgelehnt.</span><span class="sxs-lookup"><span data-stu-id="7e68e-165">Additional checks are performed, and requests that don't originate from the module are rejected.</span></span> <span data-ttu-id="7e68e-166">Das Modul unterstützt die HTTPS-Weiterleitung nicht. Deshalb werden Anforderungen über HTTP weitergeleitet, selbst wenn sie von IIS über HTTPS empfangen wurden.</span><span class="sxs-lookup"><span data-stu-id="7e68e-166">The module doesn't support HTTPS forwarding, so requests are forwarded over HTTP even if received by IIS over HTTPS.</span></span>

<span data-ttu-id="7e68e-167">Nachdem Kestrel die Anforderung vom Modul erhalten hat, wird die Anforderung in die Middleware-Pipeline von ASP.NET Core eingestellt.</span><span class="sxs-lookup"><span data-stu-id="7e68e-167">After Kestrel picks up the request from the module, the request is pushed into the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="7e68e-168">Die Middleware-Pipeline behandelt die Anforderung und gibt sie als `HttpContext`-Instanz an die App-Logik weiter.</span><span class="sxs-lookup"><span data-stu-id="7e68e-168">The middleware pipeline handles the request and passes it on as an `HttpContext` instance to the app's logic.</span></span> <span data-ttu-id="7e68e-169">Die durch IIS-Integration hinzugefügte Middleware aktualisiert das Schema, die Remote-IP und die Pfadbasis, um der Weiterleitung der Anforderung an Kestrel Rechnung zu tragen.</span><span class="sxs-lookup"><span data-stu-id="7e68e-169">Middleware added by IIS Integration updates the scheme, remote IP, and pathbase to account for forwarding the request to Kestrel.</span></span> <span data-ttu-id="7e68e-170">Die Antwort der App wird dann an IIS zurückgegeben, wo sie per Push an den HTTP-Client zurückgegeben wird, der die Anforderung initiiert hat.</span><span class="sxs-lookup"><span data-stu-id="7e68e-170">The app's response is passed back to IIS, which pushes it back out to the HTTP client that initiated the request.</span></span>

::: moniker-end

<span data-ttu-id="7e68e-171">Viele native Module, z.B. die Windows-Authentifizierung, bleiben aktiv.</span><span class="sxs-lookup"><span data-stu-id="7e68e-171">Many native modules, such as Windows Authentication, remain active.</span></span> <span data-ttu-id="7e68e-172">Weitere Informationen zu IIS-Modulen, die im Modul aktiv sind, finden Sie unter <xref:host-and-deploy/iis/modules>.</span><span class="sxs-lookup"><span data-stu-id="7e68e-172">To learn more about IIS modules active with the module, see <xref:host-and-deploy/iis/modules>.</span></span>

<span data-ttu-id="7e68e-173">Das ASP.NET Core-Modul hat noch andere Funktionen.</span><span class="sxs-lookup"><span data-stu-id="7e68e-173">The ASP.NET Core Module has a few other functions.</span></span> <span data-ttu-id="7e68e-174">Das Modul kann:</span><span class="sxs-lookup"><span data-stu-id="7e68e-174">The module can:</span></span>

* <span data-ttu-id="7e68e-175">Umgebungsvariablen für den Arbeitsprozess festlegen</span><span class="sxs-lookup"><span data-stu-id="7e68e-175">Set environment variables for the worker process.</span></span>
* <span data-ttu-id="7e68e-176">Die stdout-Ausgabe im Dateispeicher protokollieren, um Probleme beim Start zu beheben</span><span class="sxs-lookup"><span data-stu-id="7e68e-176">Log stdout output to file storage for troubleshooting startup issues.</span></span>
* <span data-ttu-id="7e68e-177">Windows-Authentifizierungstoken weiterleiten</span><span class="sxs-lookup"><span data-stu-id="7e68e-177">Forward Windows authentication tokens.</span></span>

## <a name="how-to-install-and-use-the-aspnet-core-module"></a><span data-ttu-id="7e68e-178">So installieren und verwenden Sie das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="7e68e-178">How to install and use the ASP.NET Core Module</span></span>

<span data-ttu-id="7e68e-179">Detaillierte Anweisungen zum Installieren und Verwenden des ASP.NET Core-Moduls finden Sie unter <xref:host-and-deploy/iis/index>.</span><span class="sxs-lookup"><span data-stu-id="7e68e-179">For detailed instructions on how to install and use the ASP.NET Core Module, see <xref:host-and-deploy/iis/index>.</span></span> <span data-ttu-id="7e68e-180">Informationen zum Konfigurieren des Moduls finden Sie unter <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="7e68e-180">For information on configuring the module, see the <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7e68e-181">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="7e68e-181">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/aspnet-core-module>
* [<span data-ttu-id="7e68e-182">ASP.NET Core Module GitHub repository (source code) (ASP.NET Core-Modul GitHub-Repository (Quellcode))</span><span class="sxs-lookup"><span data-stu-id="7e68e-182">ASP.NET Core Module GitHub repository (source code)</span></span>](https://github.com/aspnet/AspNetCoreModule)
* <xref:host-and-deploy/iis/modules>
