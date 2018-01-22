---
title: HTTP.sys webserverimplementierung in ASP.NET Core
author: rick-anderson
description: "Führt ein HTTP.sys, einen Webserver für ASP.NET Core unter Windows. HTTP.sys ist basiert auf Http.Sys-Kernelmodustreiber und ist eine Alternative zum Kestrel, die für die direkte Verbindung mit dem Internet ohne IIS verwendet werden kann."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/httpsys
ms.openlocfilehash: 60301e1e3eb96f51e86ef9f8be61f5fd8a4c009c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/19/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="c9613-104">HTTP.sys webserverimplementierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c9613-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="c9613-105">Durch [Tom Dykstra](https://github.com/tdykstra) und [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="c9613-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="c9613-106">Dieses Thema gilt nur für ASP.NET Core 2.0 und höher.</span><span class="sxs-lookup"><span data-stu-id="c9613-106">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="c9613-107">In früheren Versionen von ASP.NET Core HTTP.sys heißt [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="c9613-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="c9613-108">HTTP.sys ist eine [Webserver für ASP.NET Core](index.md) , die nur unter Windows ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="c9613-108">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="c9613-109">Es basiert auf der [Http.Sys-Kernelmodustreiber](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="c9613-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="c9613-110">HTTP.sys ist eine Alternative zum [Kestrel](kestrel.md) , einige Funktionen, die nicht von Kestel bietet.</span><span class="sxs-lookup"><span data-stu-id="c9613-110">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="c9613-111">**"Http.sys" kann nicht mit IIS oder IIS Express verwendet werden, da es inkompatibel mit ist der [ASP.NET Core-Modul](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="c9613-111">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="c9613-112">HTTP.sys unterstützt die folgenden Funktionen:</span><span class="sxs-lookup"><span data-stu-id="c9613-112">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="c9613-113">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="c9613-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="c9613-114">Anschlussfreigabe</span><span class="sxs-lookup"><span data-stu-id="c9613-114">Port sharing</span></span>
- <span data-ttu-id="c9613-115">HTTPS mit SNI</span><span class="sxs-lookup"><span data-stu-id="c9613-115">HTTPS with SNI</span></span>
- <span data-ttu-id="c9613-116">HTTP/2 über TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="c9613-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="c9613-117">Direkte Übertragung</span><span class="sxs-lookup"><span data-stu-id="c9613-117">Direct file transmission</span></span>
- <span data-ttu-id="c9613-118">Zwischenspeichern von Antworten</span><span class="sxs-lookup"><span data-stu-id="c9613-118">Response caching</span></span>
- <span data-ttu-id="c9613-119">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="c9613-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="c9613-120">Unterstützte Windows-Versionen:</span><span class="sxs-lookup"><span data-stu-id="c9613-120">Supported Windows versions:</span></span>

- <span data-ttu-id="c9613-121">Windows 7 und Windows Server 2008 R2 und höher</span><span class="sxs-lookup"><span data-stu-id="c9613-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="c9613-122">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c9613-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="c9613-123">Verwendung von "http.sys"</span><span class="sxs-lookup"><span data-stu-id="c9613-123">When to use HTTP.sys</span></span>

<span data-ttu-id="c9613-124">HTTP.sys ist nützlich für Bereitstellungen, in dem Sie der Server direkt mit dem Internet verfügbar zu machen, ohne mit IIS müssen.</span><span class="sxs-lookup"><span data-stu-id="c9613-124">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![HTTP.sys kommuniziert direkt mit dem Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="c9613-126">Da er auf Http.Sys basiert, erfordern nicht HTTP.sys einen reverse-Proxy-Server für den Schutz vor Angriffen durch.</span><span class="sxs-lookup"><span data-stu-id="c9613-126">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="c9613-127">Http.Sys ist ausgereifte Technologie, die für viele Arten von Angriffen geschützt und die Stabilität, Sicherheit und Skalierbarkeit von einem Webserver mit vollem Funktionsumfang.</span><span class="sxs-lookup"><span data-stu-id="c9613-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="c9613-128">IIS selbst wird als eine HTTP-Listener auf Http.Sys ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="c9613-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="c9613-129">HTTP.sys ist eine gute Wahl für interne Bereitstellungen an, wenn Sie eine Funktion nicht verfügbar in Kestrel, z. B. Windows-Authentifizierung benötigen.</span><span class="sxs-lookup"><span data-stu-id="c9613-129">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![HTTP.sys kommuniziert direkt mit Ihrem internen Netzwerk](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="c9613-131">Gewusst wie: Verwenden von "http.sys"</span><span class="sxs-lookup"><span data-stu-id="c9613-131">How to use HTTP.sys</span></span>

<span data-ttu-id="c9613-132">Hier ist eine Übersicht der Setupaufgaben für das Hostbetriebssystem und ASP.NET Core-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="c9613-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="c9613-133">Konfigurieren von WindowsServer</span><span class="sxs-lookup"><span data-stu-id="c9613-133">Configure Windows Server</span></span>

* <span data-ttu-id="c9613-134">Installieren Sie die Version von .NET, die Ihre Anwendung benötigt werden, z. B. [.NET Core](https://www.microsoft.com/net/download/core) oder [.NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="c9613-134">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="c9613-135">Zu registrieren Sie URL-Präfixe zum Binden an HTTP.sys und Einrichten von SSL-Zertifikaten</span><span class="sxs-lookup"><span data-stu-id="c9613-135">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="c9613-136">Wenn Sie URL-Präfixe in Windows nicht vorab registrieren, müssen Sie die Anwendung mit Administratorrechten ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="c9613-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="c9613-137">Die einzige Ausnahme ist, wenn Sie auf "localhost", die über HTTP (nicht "HTTPS") mit einer Portnummer, die größer als 1024 binden. In diesem Fall sind keine Administratorrechte erforderlich.</span><span class="sxs-lookup"><span data-stu-id="c9613-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="c9613-138">Weitere Informationen finden Sie unter [zu Präfixe registrieren und Konfigurieren von SSL](#preregister-url-prefixes-and-configure-ssl) weiter unten in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="c9613-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="c9613-139">Öffnen Sie die Firewall-Ports zum Zulassen des Datenverkehrs HTTP.sys zu erreichen.</span><span class="sxs-lookup"><span data-stu-id="c9613-139">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="c9613-140">Sie können *netsh.exe* oder [PowerShell-Cmdlets](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="c9613-140">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="c9613-141">Es gibt auch [Http.Sys-registrierungseinstellungen](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="c9613-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="c9613-142">Konfigurieren Sie die ASP.NET Core-Anwendung mithilfe von HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="c9613-142">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="c9613-143">Keine Paketinstallation ist erforderlich, wenn Sie mithilfe der [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) Metapackage.</span><span class="sxs-lookup"><span data-stu-id="c9613-143">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="c9613-144">Die [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) Paket ist in der Metapackage enthalten.</span><span class="sxs-lookup"><span data-stu-id="c9613-144">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="c9613-145">Rufen Sie die `UseHttpSys` Erweiterungsmethode auf `WebHostBuilder` in Ihrer `Main` Methode, dabei werden alle [HTTP.sys Optionen](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) , die Sie benötigen, wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="c9613-145">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="c9613-146">HTTP.sys-Optionen konfigurieren</span><span class="sxs-lookup"><span data-stu-id="c9613-146">Configure HTTP.sys options</span></span>

<span data-ttu-id="c9613-147">Hier sind einige der HTTP.sys-Einstellungen und Grenzwerte, die Sie konfigurieren können.</span><span class="sxs-lookup"><span data-stu-id="c9613-147">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="c9613-148">**Maximaler Client-Verbindungen**</span><span class="sxs-lookup"><span data-stu-id="c9613-148">**Maximum client connections**</span></span>

<span data-ttu-id="c9613-149">Die maximale Anzahl von gleichzeitig geöffneten TCP-Verbindungen kann festgelegt werden, für die gesamte Anwendung mithilfe des folgenden Codes in *"Program.cs"*:</span><span class="sxs-lookup"><span data-stu-id="c9613-149">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="c9613-150">Die maximale Anzahl von Verbindungen ist standardmäßig unbegrenzt (Null).</span><span class="sxs-lookup"><span data-stu-id="c9613-150">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="c9613-151">**Maximale Anforderungsgröße-Text**</span><span class="sxs-lookup"><span data-stu-id="c9613-151">**Maximum request body size**</span></span>

<span data-ttu-id="c9613-152">Die Standardgröße der Höchstwert für die Anforderung Text ist 30,000,000 Bytes, also ungefähr 28.6 MB.</span><span class="sxs-lookup"><span data-stu-id="c9613-152">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="c9613-153">Die empfohlene Methode zum Überschreiben Sie des Grenzwert von in einer ASP.NET-MVC-Anwendung Core ist die Verwendung der [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) Attribut auf eine Aktionsmethode:</span><span class="sxs-lookup"><span data-stu-id="c9613-153">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="c9613-154">Hier ist ein Beispiel, das veranschaulicht, wie die Einschränkung für die gesamte Anwendung, jede Anforderung zu konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="c9613-154">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="c9613-155">Sie können die Einstellung für eine bestimmte Anforderung in überschreiben *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="c9613-155">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="c9613-156">Eine Ausnahme wird ausgelöst, wenn Sie versuchen, den Grenzwert für eine Anforderung zu konfigurieren, nach dem Start der Anwendung Lesen der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="c9613-156">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="c9613-157">Ist ein `IsReadOnly` -Eigenschaft, die Aufschluss darüber gibt Wenn die `MaxRequestBodySize` Eigenschaft befindet sich im schreibgeschützten Zustand, d. h., es ist zu spät so konfigurieren Sie den Grenzwert.</span><span class="sxs-lookup"><span data-stu-id="c9613-157">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="c9613-158">Informationen zu den anderen HTTP.sys-Optionen finden Sie unter [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="c9613-158">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="c9613-159">Konfigurieren von URLs und Ports Lauschen an</span><span class="sxs-lookup"><span data-stu-id="c9613-159">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="c9613-160">Standardmäßig ASP.NET Core bindet an `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="c9613-160">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="c9613-161">Um URL-Präfixe und Ports konfigurieren, können Sie die `UseUrls` Erweiterungsmethode, die `urls` Befehlszeilenargument, das die Umgebungsvariable ASPNETCORE_URLS oder die `UrlPrefixes` Eigenschaft [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="c9613-161">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="c9613-162">Im folgenden Codebeispiel wird mit `UrlPrefixes`.</span><span class="sxs-lookup"><span data-stu-id="c9613-162">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="c9613-163">Ein Vorteil der `UrlPrefixes` ist, dass Sie eine Fehlermeldung sofort abrufen, wenn Sie versuchen, ein Präfix hinzuzufügen, die falsch formatiert ist.</span><span class="sxs-lookup"><span data-stu-id="c9613-163">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that is formatted wrong.</span></span> <span data-ttu-id="c9613-164">Ein Vorteil der `UseUrls` (mit freigegebenen `urls` und ASPNETCORE_URLS) ist, dass Sie leichter zwischen Kestrel und HTTP.sys wechseln können.</span><span class="sxs-lookup"><span data-stu-id="c9613-164">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="c9613-165">Wenn Sie beides verwenden `UseUrls` (oder `urls` oder ASPNETCORE_URLS) und `UrlPrefixes`, die Einstellungen in `UrlPrefixes` außer Kraft setzen in `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="c9613-165">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="c9613-166">Weitere Informationen finden Sie unter [Hosting](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="c9613-166">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="c9613-167">"Http.sys" verwendet die [HTTP Server API UrlPrefix Zeichenfolgenformate](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="c9613-167">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="c9613-168">Stellen Sie sicher, dass Sie angeben, dass die gleichen Präfixzeichenfolgen in `UseUrls` oder `UrlPrefixes` , die Sie auf dem Server zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="c9613-168">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="c9613-169">Verwenden Sie keine IIS</span><span class="sxs-lookup"><span data-stu-id="c9613-169">Don't use IIS</span></span>

<span data-ttu-id="c9613-170">Stellen Sie sicher, dass Ihre Anwendung zum Ausführen von IIS oder IIS Express konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="c9613-170">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="c9613-171">Ist in Visual Studio das Standardprofil für den Start für IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c9613-171">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="c9613-172">Um das Projekt als Konsolenanwendung ausführen, ändern Sie manuell das ausgewählte Profil, wie im folgenden Screenshot gezeigt.</span><span class="sxs-lookup"><span data-stu-id="c9613-172">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Wählen Sie die Konsole app-Profil](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="c9613-174">Zu URL-Präfixe registrieren und Konfigurieren von SSL</span><span class="sxs-lookup"><span data-stu-id="c9613-174">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="c9613-175">Sowohl IIS als auch HTTP.sys basieren auf den zugrunde liegenden Http.Sys-Kernelmodustreiber zum Abhören von Anforderungen, und der ersten Verarbeitung.</span><span class="sxs-lookup"><span data-stu-id="c9613-175">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="c9613-176">In IIS bietet die Verwaltungsbenutzeroberfläche eine relativ einfache Möglichkeit, alles zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="c9613-176">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="c9613-177">Allerdings müssen Sie Http.Sys selbst konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="c9613-177">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="c9613-178">Die integriertes Tool, d. h. dafür *netsh.exe*.</span><span class="sxs-lookup"><span data-stu-id="c9613-178">The built-in tool for doing that is *netsh.exe*.</span></span> 

<span data-ttu-id="c9613-179">Mit *netsh.exe* können Sie reservieren von URL-Präfixe und Zuweisen von SSL-Zertifikate.</span><span class="sxs-lookup"><span data-stu-id="c9613-179">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="c9613-180">Die Tools sind Administratorrechte erforderlich.</span><span class="sxs-lookup"><span data-stu-id="c9613-180">The tool requires administrative privileges.</span></span>

<span data-ttu-id="c9613-181">Das folgende Beispiel zeigt die mindestens erforderlich, um die URL-Präfixe für die Ports 80 und 443 zu reservieren:</span><span class="sxs-lookup"><span data-stu-id="c9613-181">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="c9613-182">Im folgende Beispiel wird gezeigt, wie ein SSL-Zertifikat zugewiesen werden:</span><span class="sxs-lookup"><span data-stu-id="c9613-182">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="c9613-183">Hier ist die Referenzdokumentation für *netsh.exe*:</span><span class="sxs-lookup"><span data-stu-id="c9613-183">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="c9613-184">Netsh-Befehle für Hypertext Transfer-Protokoll (HTTP)</span><span class="sxs-lookup"><span data-stu-id="c9613-184">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="c9613-185">UrlPrefix Strings</span><span class="sxs-lookup"><span data-stu-id="c9613-185">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="c9613-186">Die folgenden Ressourcen bieten detaillierte Anweisungen für verschiedene Szenarien.</span><span class="sxs-lookup"><span data-stu-id="c9613-186">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="c9613-187">Artikel, die auf HttpListener verweisen gelten gleichermaßen für HTTP.sys, wie sowohl auf Http.Sys basieren.</span><span class="sxs-lookup"><span data-stu-id="c9613-187">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="c9613-188">Vorgehensweise: Konfigurieren eines Ports mit einem SSL-Zertifikat</span><span class="sxs-lookup"><span data-stu-id="c9613-188">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="c9613-189">[HTTPS-Kommunikation - HttpListener basierend Hosting und Clientzertifizierung](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) dies ein Drittanbieter-Blog und ist ziemlich ALT, aber weiterhin enthält nützliche Informationen.</span><span class="sxs-lookup"><span data-stu-id="c9613-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="c9613-190">[Gewusst wie: Exemplarische Vorgehensweise mithilfe von HttpListener oder HTTP-Server (C++) Code, wie eine einfache SSL-Server nicht verwaltete](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) Dies ist eine ältere Blog mit nützlichen Informationen.</span><span class="sxs-lookup"><span data-stu-id="c9613-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="c9613-191">Hier sind einige Drittanbieter-Tools, die einfacher zu verwenden als die *netsh.exe* über die Befehlszeile.</span><span class="sxs-lookup"><span data-stu-id="c9613-191">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="c9613-192">Diese werden nicht von bereitgestellt oder Unterstützung von Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c9613-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="c9613-193">Die Tools als Administrator ausführen standardmäßig, da *netsh.exe* selbst sind Administratorrechte erforderlich.</span><span class="sxs-lookup"><span data-stu-id="c9613-193">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="c9613-194">[HTTP.sys Manager](http://httpsysmanager.codeplex.com/) bietet eine Benutzeroberfläche Liste und Konfigurieren von SSL-Zertifikate und Optionen, Präfix Reservierungen und Zertifikatsvertrauenslisten.</span><span class="sxs-lookup"><span data-stu-id="c9613-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="c9613-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) können Sie aus, oder konfigurieren Sie SSL-Zertifikate und URL-Präfixen.</span><span class="sxs-lookup"><span data-stu-id="c9613-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="c9613-196">Die Benutzeroberfläche als http.sys Manager verfeinerten und stellt einige weitere Konfigurationsoptionen zur Verfügung, jedoch andernfalls er verfügt über ähnliche Funktionen.</span><span class="sxs-lookup"><span data-stu-id="c9613-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="c9613-197">Eine neue Zertifikatsvertrauensliste (CTL) kann nicht erstellt werden, aber vorhandene zuweisen können.</span><span class="sxs-lookup"><span data-stu-id="c9613-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="c9613-198">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="c9613-198">Next steps</span></span>

<span data-ttu-id="c9613-199">Weitere Informationen finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="c9613-199">For more information, see the following resources:</span></span>

* [<span data-ttu-id="c9613-200">Beispiel-app für diesen Artikel</span><span class="sxs-lookup"><span data-stu-id="c9613-200">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="c9613-201">HTTP.sys-Quellcode</span><span class="sxs-lookup"><span data-stu-id="c9613-201">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="c9613-202">Hosting</span><span class="sxs-lookup"><span data-stu-id="c9613-202">Hosting</span></span>](xref:fundamentals/hosting)
