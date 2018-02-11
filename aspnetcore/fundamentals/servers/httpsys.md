---
title: Implementierung des Http.sys-Webservers in ASP.NET Core
author: rick-anderson
description: "Einführung in den Http.sys-Webserver für ASP.NET Core unter Windows Http.sys basiert auf dem Http.Sys-Kernelmodustreiber, stellt eine Alternative zu Kestrel dar und kann zum Herstellen einer direkten Verbindung mit dem Internet ohne Internetinformationsdienste (IIS) verwendet werden."
manager: wpickett
ms.author: riande
ms.date: 08/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/httpsys
ms.openlocfilehash: f36a86fc67e715165afd0c38992f9767f90cf3b4
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
# <a name="httpsys-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="315c5-104">Implementierung des Http.sys-Webservers in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="315c5-104">HTTP.sys web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="315c5-105">Von [Tom Dykstra](https://github.com/tdykstra) und [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="315c5-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="315c5-106">Dieser Artikel bezieht sich nur auf ASP.NET Core 2.0 und höher.</span><span class="sxs-lookup"><span data-stu-id="315c5-106">This topic applies only to ASP.NET Core 2.0 and later.</span></span> <span data-ttu-id="315c5-107">In früheren Versionen von ASP.NET Core wird HTTP.sys als [WebListener](xref:fundamentals/servers/weblistener) bezeichnet.</span><span class="sxs-lookup"><span data-stu-id="315c5-107">In earlier versions of ASP.NET Core, HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener).</span></span>

<span data-ttu-id="315c5-108">HTTP.sys ist ein [Webserver für ASP.NET Core](index.md), der nur unter Windows ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="315c5-108">HTTP.sys is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="315c5-109">Er basiert auf dem [Http.Sys-Kernelmodustreiber](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="315c5-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="315c5-110">HTTP.sys stellt eine Alternative zu [Kestrel](kestrel.md) dar und bietet sogar noch mehr Features.</span><span class="sxs-lookup"><span data-stu-id="315c5-110">HTTP.sys is an alternative to [Kestrel](kestrel.md) that offers some features that Kestel doesn't.</span></span> <span data-ttu-id="315c5-111">**HTTP.sys kann nicht mit IIS oder IIS Express verwendet werden, da keine Kompatibilität mit dem [ASP.NET Core-Modul](aspnet-core-module.md) vorliegt.**</span><span class="sxs-lookup"><span data-stu-id="315c5-111">**HTTP.sys can't be used with IIS or IIS Express, as it's incompatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="315c5-112">HTTP.sys unterstützt die folgenden Features:</span><span class="sxs-lookup"><span data-stu-id="315c5-112">HTTP.sys supports the following features:</span></span>

- [<span data-ttu-id="315c5-113">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="315c5-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="315c5-114">Portfreigabe</span><span class="sxs-lookup"><span data-stu-id="315c5-114">Port sharing</span></span>
- <span data-ttu-id="315c5-115">HTTPS mit SNI</span><span class="sxs-lookup"><span data-stu-id="315c5-115">HTTPS with SNI</span></span>
- <span data-ttu-id="315c5-116">HTTP/2 über TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="315c5-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="315c5-117">Direkte Dateiübertragung</span><span class="sxs-lookup"><span data-stu-id="315c5-117">Direct file transmission</span></span>
- <span data-ttu-id="315c5-118">Zwischenspeichern von Antworten</span><span class="sxs-lookup"><span data-stu-id="315c5-118">Response caching</span></span>
- <span data-ttu-id="315c5-119">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="315c5-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="315c5-120">Unterstützte Windows-Versionen:</span><span class="sxs-lookup"><span data-stu-id="315c5-120">Supported Windows versions:</span></span>

- <span data-ttu-id="315c5-121">Windows 7 und Windows Server 2008 R2 und höher</span><span class="sxs-lookup"><span data-stu-id="315c5-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="315c5-122">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="315c5-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-httpsys"></a><span data-ttu-id="315c5-123">Empfohlene Verwendung von HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="315c5-123">When to use HTTP.sys</span></span>

<span data-ttu-id="315c5-124">HTTP.sys unterstützt Sie bei Bereitstellungen, bei denen Sie den Server ohne IIS direkt mit dem Internet verbinden müssen.</span><span class="sxs-lookup"><span data-stu-id="315c5-124">HTTP.sys is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![HTTP.sys kommuniziert direkt mit dem Internet](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="315c5-126">Da HTTP.sys auf HTTP.Sys basiert, ist kein Reverseproxyserver für den Schutz gegen Angriffe erforderlich.</span><span class="sxs-lookup"><span data-stu-id="315c5-126">Because it's built on Http.Sys, HTTP.sys doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="315c5-127">Bei Http.Sys handelt es sich um eine ausgereifte Technologie, die einen Schutz gegen viele Arten von Angriffen darstellt, und die Stabilität, Sicherheit und Skalierbarkeit eines vollständigen Webservers bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="315c5-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="315c5-128">IIS wird neben Http.Sys auch als HTTP-Listener ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="315c5-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="315c5-129">HTTP.sys eignet sich gut für interne Bereitstellungen, wenn Sie ein Feature benötigen, das in Kestrel nicht verfügbar ist, z.B. die Windows-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="315c5-129">HTTP.sys is a good choice for internal deployments when you need a feature not available in Kestrel, such as Windows authentication.</span></span>

![HTTP.sys kommuniziert direkt mit Ihrem internen Netzwerk](httpsys/_static/httpsys-to-internal.png)

## <a name="how-to-use-httpsys"></a><span data-ttu-id="315c5-131">Empfohlene Verwendung von HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="315c5-131">How to use HTTP.sys</span></span>

<span data-ttu-id="315c5-132">Im Folgenden erhalten Sie eine Übersicht zum Einrichten von Aufgaben für das Hostbetriebssystem und Ihre ASP.NET Core-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="315c5-132">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="315c5-133">Konfigurieren von Windows Server</span><span class="sxs-lookup"><span data-stu-id="315c5-133">Configure Windows Server</span></span>

* <span data-ttu-id="315c5-134">Installieren Sie die für Ihre Anwendung erforderliche .NET-Version, z.B. [.NET Core](https://www.microsoft.com/net/download/core) oder [.NET Framework](https://www.microsoft.com/net/download/framework).</span><span class="sxs-lookup"><span data-stu-id="315c5-134">Install the version of .NET that your application requires, such as [.NET Core](https://www.microsoft.com/net/download/core) or [.NET Framework](https://www.microsoft.com/net/download/framework).</span></span>

* <span data-ttu-id="315c5-135">Registrieren Sie URL-Präfixe vorab, um sie an HTTP.sys zu binden, und richten Sie SSL-Zertifikate ein.</span><span class="sxs-lookup"><span data-stu-id="315c5-135">Preregister URL prefixes to bind to HTTP.sys, and set up SSL certificates</span></span>

   <span data-ttu-id="315c5-136">Wenn Sie unter Windows vorab keine URL-Präfixe registrieren, müssen Sie Ihre Anwendung mit Administratorberechtigungen ausführen.</span><span class="sxs-lookup"><span data-stu-id="315c5-136">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="315c5-137">Die einzige Ausnahme besteht, wenn Sie mithilfe von HTTP (nicht HTTPS) eine Verbindung mit dem Localhost herstellen und die Portnummer größer als 1024 ist. In diesem Fall sind keine Administratorberechtigungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="315c5-137">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case, administrator privileges aren't required.</span></span>

   <span data-ttu-id="315c5-138">Weitere Informationen finden Sie im Abschnitt [Registrieren von URL-Präfixen im Voraus und Konfigurieren von SSL](#preregister-url-prefixes-and-configure-ssl).</span><span class="sxs-lookup"><span data-stu-id="315c5-138">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="315c5-139">Öffnen Sie Firewallports, damit der Datenverkehr HTTP.sys erreicht.</span><span class="sxs-lookup"><span data-stu-id="315c5-139">Open firewall ports to allow traffic to reach HTTP.sys.</span></span>

   <span data-ttu-id="315c5-140">Sie können dafür *netsh.exe* oder [PowerShell-Cmdlets](https://technet.microsoft.com/library/jj554906) verwenden.</span><span class="sxs-lookup"><span data-stu-id="315c5-140">You can use *netsh.exe* or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="315c5-141">Außerdem gibt es [Einstellungen für die Http.Sys-Registrierung](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="315c5-141">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application-to-use-httpsys"></a><span data-ttu-id="315c5-142">Konfigurieren Ihrer ASP.NET Core-Anwendung zur Verwendung von HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="315c5-142">Configure your ASP.NET Core application to use HTTP.sys</span></span>

* <span data-ttu-id="315c5-143">Wenn Sie das [Microsoft.AspNetCore.All](xref:fundamentals/metapackage)-Metapaket verwenden, ist keine Paketinstallation notwendig.</span><span class="sxs-lookup"><span data-stu-id="315c5-143">No package install is needed if you use the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="315c5-144">Das [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)-Paket ist im Metapaket enthalten.</span><span class="sxs-lookup"><span data-stu-id="315c5-144">The [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/) package is included in the metapackage.</span></span>

* <span data-ttu-id="315c5-145">Rufen Sie die Erweiterungsmethode `UseHttpSys` unter `WebHostBuilder` in Ihrer `Main`-Methode auf, und geben Sie dabei wie im folgenden Beispiel dargestellt alle erforderlichen [HTTP.sys-Optionen](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) an:</span><span class="sxs-lookup"><span data-stu-id="315c5-145">Call the `UseHttpSys` extension method on `WebHostBuilder` in your `Main` method, specifying any [HTTP.sys options](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=11-19)]

### <a name="configure-httpsys-options"></a><span data-ttu-id="315c5-146">Konfigurieren von HTTP.sys-Optionen</span><span class="sxs-lookup"><span data-stu-id="315c5-146">Configure HTTP.sys options</span></span>

<span data-ttu-id="315c5-147">Im Folgenden werden einige Einstellungen und Einschränkungen für HTTP.sys aufgeführt, die Sie konfigurieren können.</span><span class="sxs-lookup"><span data-stu-id="315c5-147">Here are some of the HTTP.sys settings and limits that you can configure.</span></span>

<span data-ttu-id="315c5-148">**Maximale Anzahl der Clientverbindungen**</span><span class="sxs-lookup"><span data-stu-id="315c5-148">**Maximum client connections**</span></span>

<span data-ttu-id="315c5-149">Die maximale Anzahl von gleichzeitig geöffneten TCP-Verbindungen kann mithilfe von folgendem Code in *Program.cs* für die gesamte Anwendung festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="315c5-149">The maximum number of concurrent open TCP connections can be set for the entire application with the following code in *Program.cs*:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=5)]

<span data-ttu-id="315c5-150">Die maximale Anzahl von Verbindungen ist standardmäßig nicht begrenzt (NULL).</span><span class="sxs-lookup"><span data-stu-id="315c5-150">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="315c5-151">**Maximale Größe des Anforderungstexts**</span><span class="sxs-lookup"><span data-stu-id="315c5-151">**Maximum request body size**</span></span>

<span data-ttu-id="315c5-152">Die maximale Größe des Anforderungstexts beträgt standardmäßig 30.000.000 Byte, also ungefähr 28,6 MB.</span><span class="sxs-lookup"><span data-stu-id="315c5-152">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span>

<span data-ttu-id="315c5-153">Die empfohlene Methode zur Außerkraftsetzung des Grenzwerts in einer ASP.NET Core-MVC-App besteht im Verwenden des [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs)-Attributs in einer Aktionsmethode:</span><span class="sxs-lookup"><span data-stu-id="315c5-153">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="315c5-154">Im folgenden Beispiel wird veranschaulicht, wie die Einschränkung für die gesamte Anwendung und alle Anfragen konfiguriert wird:</span><span class="sxs-lookup"><span data-stu-id="315c5-154">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Options&highlight=6)]

<span data-ttu-id="315c5-155">Sie können diese Einstellung für eine bestimmte Anforderung in *Startup.cs* außer Kraft setzen:</span><span class="sxs-lookup"><span data-stu-id="315c5-155">You can override the setting on a specific request in *Startup.cs*:</span></span>

[!code-csharp[](httpsys/sample/Startup.cs?name=snippet_Configure&highlight=9-10)]
 
<span data-ttu-id="315c5-156">Eine Ausnahme wird ausgelöst, wenn Sie versuchen, den Grenzwert einer Anforderung zu konfigurieren, nachdem die Anwendung bereits mit dem Lesen der Anforderung begonnen hat.</span><span class="sxs-lookup"><span data-stu-id="315c5-156">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="315c5-157">Es gibt eine `IsReadOnly`-Eigenschaft, die Ihnen mitteilt, wenn sich die `MaxRequestBodySize`-Eigenschaft im schreibgeschützten Zustand befindet, also wenn der Grenzwert nicht mehr konfiguriert werden kann.</span><span class="sxs-lookup"><span data-stu-id="315c5-157">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="315c5-158">Informationen zu anderen HTTP.sys-Optionen finden Site unter [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="315c5-158">For information about other HTTP.sys options, see [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> 

### <a name="configure-urls-and-ports-to-listen-on"></a><span data-ttu-id="315c5-159">Konfigurieren von URLs und Ports, die überwacht werden sollen</span><span class="sxs-lookup"><span data-stu-id="315c5-159">Configure URLs and ports to listen on</span></span> 

<span data-ttu-id="315c5-160">Standardmäßig ist ASP.NET Core an `http://localhost:5000` gebunden.</span><span class="sxs-lookup"><span data-stu-id="315c5-160">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="315c5-161">Wenn Sie URL-Präfixe und Ports konfigurieren möchten, können Sie die Erweiterungsmethode `UseUrls`, das Befehlszeilenargument `urls`, die Umgebungsvariable „ASPNETCORE_URLS“ oder die Eigenschaft `UrlPrefixes` unter [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs) verwenden.</span><span class="sxs-lookup"><span data-stu-id="315c5-161">To configure URL prefixes and ports, you can use the `UseUrls` extension method, the `urls` command-line argument, the ASPNETCORE_URLS environment variable, or the `UrlPrefixes` property on [HttpSysOptions](https://github.com/aspnet/HttpSysServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.HttpSys/HttpSysOptions.cs).</span></span> <span data-ttu-id="315c5-162">Im folgenden Codebeispiel wird `UrlPrefixes` verwendet.</span><span class="sxs-lookup"><span data-stu-id="315c5-162">The following code example uses `UrlPrefixes`.</span></span>

[!code-csharp[](httpsys/sample/Program.cs?name=snippet_Main&highlight=17)]

<span data-ttu-id="315c5-163">Der Vorteil von `UrlPrefixes` ist, dass Sie umgehend eine Fehlermeldung erhalten, wenn Sie versuchen, ein Präfix hinzuzufügen, das nicht richtig formatiert ist.</span><span class="sxs-lookup"><span data-stu-id="315c5-163">An advantage of `UrlPrefixes` is that you get an error message immediately if you try to add a prefix that's formatted wrong.</span></span> <span data-ttu-id="315c5-164">Der Vorteil von `UseUrls` (für `urls` und „ASPNETCORE_URLS“ freigegeben) ist, dass Sie einfacher zwischen Kestrel und HTTP.sys wechseln können.</span><span class="sxs-lookup"><span data-stu-id="315c5-164">An advantage of `UseUrls` (shared with `urls` and ASPNETCORE_URLS) is that you can more easily switch between Kestrel and HTTP.sys.</span></span>

<span data-ttu-id="315c5-165">Wenn Sie sowohl `UseUrls` (bzw. `urls` oder „ASPNETCORE_URLS“) als auch `UrlPrefixes` verwenden, überschreiben die Einstellungen in `UrlPrefixes` die Einstellungen in `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="315c5-165">If you use both `UseUrls` (or `urls` or ASPNETCORE_URLS) and `UrlPrefixes`, the settings in `UrlPrefixes` override the ones in `UseUrls`.</span></span> <span data-ttu-id="315c5-166">Weitere Informationen finden Sie unter [Hosting](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="315c5-166">For more information, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="315c5-167">HTTP.sys verwendet die [HTTP Server-API-UrlPrefix-Zeichenfolgenformate](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="315c5-167">HTTP.sys uses the [HTTP Server API UrlPrefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span>

> [!NOTE]
> <span data-ttu-id="315c5-168">Vergewissern Sie sich, dass Sie dieselben Präfixzeichenfolgen in `UseUrls` oder `UrlPrefixes` verwenden, die Sie auf dem Server vorab registriert haben.</span><span class="sxs-lookup"><span data-stu-id="315c5-168">Make sure that you specify the same prefix strings in `UseUrls` or `UrlPrefixes` that you preregister on the server.</span></span> 

### <a name="dont-use-iis"></a><span data-ttu-id="315c5-169">Verwenden Sie keine Internetinformationsdienste (IIS).</span><span class="sxs-lookup"><span data-stu-id="315c5-169">Don't use IIS</span></span>

<span data-ttu-id="315c5-170">Vergewissern Sie sich außerdem, dass Ihre Anwendung nicht für IIS oder IIS Express konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="315c5-170">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

<span data-ttu-id="315c5-171">In Visual Studio ist das Standardstartprofil auf IIS Express ausgerichtet.</span><span class="sxs-lookup"><span data-stu-id="315c5-171">In Visual Studio, the default launch profile is for IIS Express.</span></span> <span data-ttu-id="315c5-172">Wenn das Projekt als Konsolenanwendung ausgeführt werden soll, ändern Sie wie im folgenden Screenshot dargestellt das ausgewählte Profil manuell.</span><span class="sxs-lookup"><span data-stu-id="315c5-172">To run the project as a console application, manually change the selected profile, as shown in the following screen shot.</span></span>

![Auswählen des Profils der App-Konsole](httpsys/_static/vs-choose-profile.png)

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="315c5-174">Registrieren von URL-Präfixen im Voraus und Konfigurieren von SSL</span><span class="sxs-lookup"><span data-stu-id="315c5-174">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="315c5-175">Sowohl IIS als auch HTTP.sys sind abhängig von dem zugrunde liegenden Http.Sys-Kernelmodustreiber, der Anforderungen überwacht und Vorverarbeitungen durchführt.</span><span class="sxs-lookup"><span data-stu-id="315c5-175">Both IIS and HTTP.sys rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="315c5-176">In IIS stellt die Benutzeroberfläche für die Verwaltung eine relativ einfache Möglichkeit zum Konfigurieren dar.</span><span class="sxs-lookup"><span data-stu-id="315c5-176">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="315c5-177">Sie müssen HTTP.Sys hingegen manuell konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="315c5-177">However, you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="315c5-178">Dafür ist das Tool *netsh.exe* in WebListener integriert.</span><span class="sxs-lookup"><span data-stu-id="315c5-178">The built-in tool for doing that's *netsh.exe*.</span></span> 

<span data-ttu-id="315c5-179">Mithilfe von *netsh.exe* können Sie URL-Präfixe reservieren und SSL-Zertifikate zuweisen.</span><span class="sxs-lookup"><span data-stu-id="315c5-179">With *netsh.exe* you can reserve URL prefixes and assign SSL certificates.</span></span> <span data-ttu-id="315c5-180">Für das Tool sind Administratorrechte erforderlich.</span><span class="sxs-lookup"><span data-stu-id="315c5-180">The tool requires administrative privileges.</span></span>

<span data-ttu-id="315c5-181">Im folgenden Beispiel wird dargestellt, welche Elemente mindestens benötigt werden, um URL-Präfixe für die Ports 80 und 443 zu reservieren:</span><span class="sxs-lookup"><span data-stu-id="315c5-181">The following example shows the minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="315c5-182">Im folgenden Beispiel wird dargestellt, wie Sie ein SSL-Zertifikat zuweisen:</span><span class="sxs-lookup"><span data-stu-id="315c5-182">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}"
```

<span data-ttu-id="315c5-183">Die Referenzdokumentation für *netsh.exe* finden Sie hier:</span><span class="sxs-lookup"><span data-stu-id="315c5-183">Here is the reference documentation for *netsh.exe*:</span></span>

* [<span data-ttu-id="315c5-184">Netsh Commands for Hypertext Transfer Protocol (HTTP) (Netsh-Befehle für Hypertext Transfer-Protokolle (HTTP))</span><span class="sxs-lookup"><span data-stu-id="315c5-184">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="315c5-185">UrlPrefix Strings (UrlPrefix-Zeichenfolgen)</span><span class="sxs-lookup"><span data-stu-id="315c5-185">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="315c5-186">In den folgenden Ressourcen finden Sie detaillierte Anweisungen zu verschiedenen Szenarios.</span><span class="sxs-lookup"><span data-stu-id="315c5-186">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="315c5-187">Artikel, die sich auf HttpListener beziehen, beziehen sich auch auf HTTP.sys, da beide Tools auf Http.Sys basieren.</span><span class="sxs-lookup"><span data-stu-id="315c5-187">Articles that refer to HttpListener apply equally to HTTP.sys, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="315c5-188">Vorgehensweise: Konfigurieren eines Ports mit einem SSL-Zertifikat</span><span class="sxs-lookup"><span data-stu-id="315c5-188">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="315c5-189">[HTTPS Communication – HttpListener based Hosting and Client Certification (HTTPS-Kommunikation: HttpListener basierend auf Hosting- und Clientzertifizierung)](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) Hierbei handelt es sich um einen Blog eines Drittanbieters, der zwar schon recht alt ist, aber trotzdem nützliche Informationen enthält.</span><span class="sxs-lookup"><span data-stu-id="315c5-189">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="315c5-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server (Vorgehensweise: Exemplarische Vorgehensweise zum Verwenden von nicht verwaltetem Code (C++) für HttpListener oder Http-Server als einfacher SSL-Server)](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/): Hierbei handelt es sich ebenfalls um einen älteren Blog mit nützlichen Informationen.</span><span class="sxs-lookup"><span data-stu-id="315c5-190">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>

<span data-ttu-id="315c5-191">Die Verwendung der folgenden Tools von Drittanbietern ist möglicherweise einfacher als die Verwendung der *netsh.exe*-Befehlszeile.</span><span class="sxs-lookup"><span data-stu-id="315c5-191">Here are some third-party tools that can be easier to use than the *netsh.exe* command line.</span></span> <span data-ttu-id="315c5-192">Diese Tools werden allerdings von Microsoft weder bereitgestellt noch unterstützt.</span><span class="sxs-lookup"><span data-stu-id="315c5-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="315c5-193">Die Tools werden standardmäßig als Administratoren ausgeführt, da *netsh.exe* Administratorberechtigungen erfordert.</span><span class="sxs-lookup"><span data-stu-id="315c5-193">The tools run as administrator by default, since *netsh.exe* itself requires administrator privileges.</span></span>

* <span data-ttu-id="315c5-194">Der [Http.sys-Manager](http://httpsysmanager.codeplex.com/) stellt eine Benutzeroberfläche bereit, die zum Auflisten und Konfigurieren von SSL-Zertifikaten und -Optionen, Präfixreservierungen und Zertifikatvertrauenslisten verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="315c5-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="315c5-195">Mithilfe von [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) können Sie SSL-Zertifikate und URL-Präfixe auflisten und konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="315c5-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="315c5-196">Die Benutzeroberfläche ist besser optimiert als beim Http.sys-Manager und beinhaltet einige zusätzliche Optionen für die Konfiguration. Ansonsten sind die Funktionen allerdings ähnlich.</span><span class="sxs-lookup"><span data-stu-id="315c5-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="315c5-197">Es können zwar keine neuen Zertifikatsvertrauenslisten erstellt werden, jedoch können bereits vorhandene Listen hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="315c5-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

## <a name="next-steps"></a><span data-ttu-id="315c5-198">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="315c5-198">Next steps</span></span>

<span data-ttu-id="315c5-199">Weitere Informationen finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="315c5-199">For more information, see the following resources:</span></span>

* [<span data-ttu-id="315c5-200">Beispiel-App zu diesem Artikel</span><span class="sxs-lookup"><span data-stu-id="315c5-200">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/httpsys/sample)
* [<span data-ttu-id="315c5-201">HTTP.sys-Quellcode</span><span class="sxs-lookup"><span data-stu-id="315c5-201">HTTP.sys source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="315c5-202">Hosting</span><span class="sxs-lookup"><span data-stu-id="315c5-202">Hosting</span></span>](xref:fundamentals/hosting)
