---
title: Implementierung des Webservers WebListener in ASP.NET Core
author: rick-anderson
description: Erfahren Sie mehr über WebListener, einen Webserver für ASP.NET Core unter Windows, der für die direkte Verbindung mit dem Internet ohne IIS verwendet werden kann.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2018
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 5602c1ddbe76879587de12bcd82722c103dee03f
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751462"
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="b4614-103">Implementierung des Webservers WebListener in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b4614-103">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="b4614-104">Von [Tom Dykstra](https://github.com/tdykstra) und [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="b4614-104">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="b4614-105">Dieser Artikel bezieht sich nur auf ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="b4614-105">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="b4614-106">In ASP.NET Core 2.0 wird WebListener [HTTP.sys](httpsys.md) genannt.</span><span class="sxs-lookup"><span data-stu-id="b4614-106">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="b4614-107">Der WebListener ist ein [Webserver für ASP.NET Core](index.md), der nur unter Windows ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="b4614-107">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="b4614-108">Er basiert auf dem [Http.sys-Kernelmodustreiber](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="b4614-108">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="b4614-109">Er stellt eine Alternative zu [Kestrel](kestrel.md) dar und kann zum Herstellen einer direkten Verbindung mit dem Internet ohne Internetinformationsdienste (IIS) als Reverseproxyserver verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b4614-109">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="b4614-110">**Der WebListener kann nicht mit IIS oder IIS Express verwendet werden, da er nicht mit dem [ASP.NET Core-Modul kompatibel](aspnet-core-module.md) ist.**</span><span class="sxs-lookup"><span data-stu-id="b4614-110">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="b4614-111">Obwohl der WebListener für ASP.NET Core entwickelt wurde, kann er in allen .NET Core oder .NET Framework-Anwendungen über das NuGet-Paket [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b4614-111">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="b4614-112">Der WebListener unterstützt die folgenden Features:</span><span class="sxs-lookup"><span data-stu-id="b4614-112">WebListener supports the following features:</span></span>

- [<span data-ttu-id="b4614-113">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="b4614-113">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="b4614-114">Portfreigabe</span><span class="sxs-lookup"><span data-stu-id="b4614-114">Port sharing</span></span>
- <span data-ttu-id="b4614-115">HTTPS mit SNI</span><span class="sxs-lookup"><span data-stu-id="b4614-115">HTTPS with SNI</span></span>
- <span data-ttu-id="b4614-116">HTTP/2 über TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="b4614-116">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="b4614-117">Direkte Dateiübertragung</span><span class="sxs-lookup"><span data-stu-id="b4614-117">Direct file transmission</span></span>
- <span data-ttu-id="b4614-118">Zwischenspeichern von Antworten</span><span class="sxs-lookup"><span data-stu-id="b4614-118">Response caching</span></span>
- <span data-ttu-id="b4614-119">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="b4614-119">WebSockets (Windows 8)</span></span>

<span data-ttu-id="b4614-120">Unterstützte Windows-Versionen:</span><span class="sxs-lookup"><span data-stu-id="b4614-120">Supported Windows versions:</span></span>

- <span data-ttu-id="b4614-121">Windows 7 und Windows Server 2008 R2 und höher</span><span class="sxs-lookup"><span data-stu-id="b4614-121">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="b4614-122">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b4614-122">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="b4614-123">Empfohlene Verwendung des WebListeners</span><span class="sxs-lookup"><span data-stu-id="b4614-123">When to use WebListener</span></span>

<span data-ttu-id="b4614-124">Der WebListener unterstützt Sie bei Bereitstellungen in Rahmen derer Sie ohne IIS den Server direkt mit dem Internet verbinden müssen.</span><span class="sxs-lookup"><span data-stu-id="b4614-124">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![WebListener kommuniziert direkt mit dem Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="b4614-126">Da der WebListener auf HTTP.sys basiert, ist kein Reverseproxyserver für den Schutz gegen Angriffe erforderlich.</span><span class="sxs-lookup"><span data-stu-id="b4614-126">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="b4614-127">Bei Http.Sys handelt es sich um eine ausgereifte Technologie, die einen Schutz gegen viele Arten von Angriffen darstellt, und die Stabilität, Sicherheit und Skalierbarkeit eines vollständigen Webservers bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="b4614-127">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="b4614-128">IIS wird neben Http.Sys auch als HTTP-Listener ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="b4614-128">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span>

<span data-ttu-id="b4614-129">Der WebListener bietet außerdem eine gute Möglichkeit zur internen Bereitstellung, wenn Sie eins der im Lieferumfang enthaltenen Features benötigen, die Kestrel nicht anbietet.</span><span class="sxs-lookup"><span data-stu-id="b4614-129">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![WebListener kommuniziert direkt mit Ihrem internen Netzwerk](weblistener/_static/weblistener-to-internal.png)

## <a name="kernel-mode-authentication-with-kerberos"></a><span data-ttu-id="b4614-131">Kernelmodusauthentifizierung mit Kerberos</span><span class="sxs-lookup"><span data-stu-id="b4614-131">Kernel mode authentication with Kerberos</span></span>

<span data-ttu-id="b4614-132">WebListener delegiert zur Kernelmodusauthentifizierung mit dem Kerberos-Authentifizierungsprotokoll.</span><span class="sxs-lookup"><span data-stu-id="b4614-132">WebListener delegates to kernel mode authentication with the Kerberos authentication protocol.</span></span> <span data-ttu-id="b4614-133">Die Benutzermodusauthentifizierung wird nicht von Kerberos und WebListener unterstützt.</span><span class="sxs-lookup"><span data-stu-id="b4614-133">User mode authentication isn't supported with Kerberos and WebListener.</span></span> <span data-ttu-id="b4614-134">Das Computerkonto muss zum Entschlüsseln des Kerberos-Tokens bzw. -Tickets verwendet werden, das von Active Directory abgerufen und zur Authentifizierung des Benutzers vom Client an den Server weitergeleitet wird.</span><span class="sxs-lookup"><span data-stu-id="b4614-134">The machine account must be used to decrypt the Kerberos token/ticket that's obtained from Active Directory and forwarded by the client to the server to authenticate the user.</span></span> <span data-ttu-id="b4614-135">Registrieren Sie den Dienstprinzipalnamen (SPN) für den Host, nicht für den Benutzer der App.</span><span class="sxs-lookup"><span data-stu-id="b4614-135">Register the Service Principal Name (SPN) for the host, not the user of the app.</span></span>

## <a name="how-to-use-weblistener"></a><span data-ttu-id="b4614-136">Verwendung des WebListeners</span><span class="sxs-lookup"><span data-stu-id="b4614-136">How to use WebListener</span></span>

<span data-ttu-id="b4614-137">Im Folgenden erhalten Sie eine Übersicht zum Einrichten von Aufgaben für das Hostbetriebssystem und Ihre ASP.NET Core-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="b4614-137">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="b4614-138">Konfigurieren von Windows Server</span><span class="sxs-lookup"><span data-stu-id="b4614-138">Configure Windows Server</span></span>

* <span data-ttu-id="b4614-139">Installieren Sie die für Ihre Anwendung erforderliche .NET-Version – z.B. [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) oder .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="b4614-139">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="b4614-140">Registrieren Sie URL-Präfixe vorab, um sie an den WebListener zu binden, und richten Sie SSL-Zertifikate ein.</span><span class="sxs-lookup"><span data-stu-id="b4614-140">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="b4614-141">Wenn Sie unter Windows vorab keine URL-Präfixe registrieren, müssen Sie Ihre Anwendung mit Administratorberechtigungen ausführen.</span><span class="sxs-lookup"><span data-stu-id="b4614-141">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="b4614-142">Die einzige Ausnahme besteht, wenn Sie mithilfe von HTTP (nicht HTTPS) eine Verbindung mit dem Localhost herstellen und die Portnummer größer als 1024 ist. In diesem Fall sind keine Administratorberechtigungen erforderlich.</span><span class="sxs-lookup"><span data-stu-id="b4614-142">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="b4614-143">Weitere Informationen finden Sie im Abschnitt [Registrieren von URL-Präfixen im Voraus und Konfigurieren von SSL](#preregister-url-prefixes-and-configure-ssl).</span><span class="sxs-lookup"><span data-stu-id="b4614-143">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="b4614-144">Öffnen Sie Firewallports, damit der Datenverkehr den WebListener erreicht.</span><span class="sxs-lookup"><span data-stu-id="b4614-144">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="b4614-145">Sie können dafür „netsh.exe“ oder [PowerShell-Cmdlets](https://technet.microsoft.com/library/jj554906) verwenden.</span><span class="sxs-lookup"><span data-stu-id="b4614-145">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="b4614-146">Außerdem gibt es [Einstellungen für die Http.Sys-Registrierung](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="b4614-146">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="b4614-147">Konfigurieren Ihrer ASP.NET Core-Anwendung</span><span class="sxs-lookup"><span data-stu-id="b4614-147">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="b4614-148">Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="b4614-148">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="b4614-149">Dadurch wird auch [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) als Abhängigkeit installiert.</span><span class="sxs-lookup"><span data-stu-id="b4614-149">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="b4614-150">Rufen Sie die Erweiterungsmethode `UseWebListener` auf [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) in der Methode `Main` ab, und legen Sie dabei wie im folgenden Beispiel dargestellt alle benötigten WebListener-[Optionen](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) und -[Einstellungen](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) fest:</span><span class="sxs-lookup"><span data-stu-id="b4614-150">Call the `UseWebListener` extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="b4614-151">Konfigurieren von URLs und Ports, die überwacht werden sollen</span><span class="sxs-lookup"><span data-stu-id="b4614-151">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="b4614-152">Standardmäßig ist ASP.NET Core an `http://localhost:5000` gebunden.</span><span class="sxs-lookup"><span data-stu-id="b4614-152">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="b4614-153">Wenn Sie URL-Präfixe und Ports konfigurieren möchten, können Sie die Erweiterungsmethode `UseURLs`, das Befehlszeilenargument `urls` oder das ASP.NET Core-Konfigurationssystem verwenden.</span><span class="sxs-lookup"><span data-stu-id="b4614-153">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="b4614-154">Weitere Informationen finden Sie unter [Host in ASP.NET Core (Hosten in ASP.NET Core)](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="b4614-154">For more information, see [Host in ASP.NET Core(xref:fundamentals/host/index).</span></span>

  <span data-ttu-id="b4614-155">Der WebListener verwendet die [Präfixzeichenfolgenformate von Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="b4614-155">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="b4614-156">Es gibt keine spezifischen Anforderungen an Präfixzeichenfolgenformate für Präfixe für den WebListener.</span><span class="sxs-lookup"><span data-stu-id="b4614-156">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!WARNING]
  > <span data-ttu-id="b4614-157">Allgemeine Platzhalterbindungen (`http://*:80/` und `http://+:80`) dürfen **nicht** verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b4614-157">Top-level wildcard bindings (`http://*:80/` and `http://+:80`) should **not** be used.</span></span> <span data-ttu-id="b4614-158">Platzhalterbindungen auf oberster Ebene gefährden die Sicherheit Ihrer App.</span><span class="sxs-lookup"><span data-stu-id="b4614-158">Top-level wildcard bindings can open up your app to security vulnerabilities.</span></span> <span data-ttu-id="b4614-159">Dies gilt für starke und schwache Platzhalter.</span><span class="sxs-lookup"><span data-stu-id="b4614-159">This applies to both strong and weak wildcards.</span></span> <span data-ttu-id="b4614-160">Verwenden Sie statt Platzhaltern explizite Hostnamen.</span><span class="sxs-lookup"><span data-stu-id="b4614-160">Use explicit host names rather than wildcards.</span></span> <span data-ttu-id="b4614-161">Platzhalterbindungen in untergeordneten Domänen (z.B. `*.mysub.com`) verursachen kein Sicherheitsrisiko, wenn Sie die gesamte übergeordnete Domäne steuern (im Gegensatz zu `*.com`, das angreifbar ist).</span><span class="sxs-lookup"><span data-stu-id="b4614-161">Subdomain wildcard binding (for example, `*.mysub.com`) doesn't have this security risk if you control the entire parent domain (as opposed to `*.com`, which is vulnerable).</span></span> <span data-ttu-id="b4614-162">Weitere Informationen finden Sie unter [rfc7230 im Abschnitt 5.4](https://tools.ietf.org/html/rfc7230#section-5.4).</span><span class="sxs-lookup"><span data-stu-id="b4614-162">See [rfc7230 section-5.4](https://tools.ietf.org/html/rfc7230#section-5.4) for more information.</span></span>

  > [!NOTE]
  > <span data-ttu-id="b4614-163">Vergewissern Sie sich, dass Sie dieselben Präfixzeichenfolgen in `UseUrls` angeben, die Sie auf dem Server vorab registriert haben.</span><span class="sxs-lookup"><span data-stu-id="b4614-163">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="b4614-164">Vergewissern Sie sich außerdem, dass Ihre Anwendung nicht für IIS oder IIS Express konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="b4614-164">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="b4614-165">In Visual Studio ist das Standardstartprofil auf IIS Express ausgerichtet.</span><span class="sxs-lookup"><span data-stu-id="b4614-165">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="b4614-166">Wenn das Projekt als Konsolenanwendung ausgeführt werden soll, müssen Sie wie im folgenden Screenshot dargestellt das ausgewählte Profil manuell ändern.</span><span class="sxs-lookup"><span data-stu-id="b4614-166">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Auswählen des Profils der App-Konsole](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="b4614-168">Verwendung des WebListeners außerhalb von ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b4614-168">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="b4614-169">Installieren Sie das NuGet-Paket [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/).</span><span class="sxs-lookup"><span data-stu-id="b4614-169">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="b4614-170">[Registrieren Sie URL-Präfixe vorab, um sie an den WebListener zu binden, und richten Sie SSL-Zertifikate ein](#preregister-url-prefixes-and-configure-ssl), wie Sie es auch für die Verwendung in ASP.NET Core tun würden.</span><span class="sxs-lookup"><span data-stu-id="b4614-170">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="b4614-171">Außerdem gibt es [Einstellungen für die Http.Sys-Registrierung](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="b4614-171">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="b4614-172">Im Folgenden ist ein Codebeispiel dargestellt, in dem Sie die Verwendung des WebListeners außerhalb von ASP.NET Core sehen können:</span><span class="sxs-lookup"><span data-stu-id="b4614-172">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

```csharp
var settings = new WebListenerSettings();
settings.UrlPrefixes.Add("http://localhost:8080");

using (WebListener listener = new WebListener(settings))
{
    listener.Start();

    while (true)
    {
        var context = await listener.AcceptAsync();
        byte[] bytes = Encoding.ASCII.GetBytes("Hello World: " + DateTime.Now);
        context.Response.ContentLength = bytes.Length;
        context.Response.ContentType = "text/plain";

        await context.Response.Body.WriteAsync(bytes, 0, bytes.Length);
        context.Dispose();
    }
}
```

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="b4614-173">Registrieren von URL-Präfixen im Voraus und Konfigurieren von SSL</span><span class="sxs-lookup"><span data-stu-id="b4614-173">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="b4614-174">Sowohl IIS als auch der WebListener sind abhängig von dem zugrunde liegenden Http.Sys-Kernelmodustreiber, der Anforderungen überwacht und Vorverarbeitungen durchführt.</span><span class="sxs-lookup"><span data-stu-id="b4614-174">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="b4614-175">In IIS stellt die Benutzeroberfläche für die Verwaltung eine relativ einfache Möglichkeit zum Konfigurieren dar.</span><span class="sxs-lookup"><span data-stu-id="b4614-175">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="b4614-176">Wenn Sie hingegen WebListener verwenden, müssen Sie Http.Sys manuell konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="b4614-176">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="b4614-177">Dafür ist das Tool „netsh.exe“ in WebListener integriert.</span><span class="sxs-lookup"><span data-stu-id="b4614-177">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="b4614-178">„netsh.exe“ wird vor allem dazu verwendet, URL-Präfixe zu reservieren und SSL-Zertifikate zuzuweisen.</span><span class="sxs-lookup"><span data-stu-id="b4614-178">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="b4614-179">„Netsh.exe“ ist ein benutzerfreundliches Tool, das auch von Anfängern verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="b4614-179">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="b4614-180">Im folgenden Beispiel wird dargestellt, welche Elemente mindestens benötigt werden, um URL-Präfixe für die Ports 80 und 443 zu reservieren:</span><span class="sxs-lookup"><span data-stu-id="b4614-180">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="b4614-181">Im folgenden Beispiel wird dargestellt, wie Sie ein SSL-Zertifikat zuweisen:</span><span class="sxs-lookup"><span data-stu-id="b4614-181">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid="{00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="b4614-182">Die offizielle Referenzdokumentation finden Sie unter den folgenden Links:</span><span class="sxs-lookup"><span data-stu-id="b4614-182">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="b4614-183">Netsh Commands for Hypertext Transfer Protocol (HTTP) (Netsh-Befehle für Hypertext Transfer-Protokolle (HTTP))</span><span class="sxs-lookup"><span data-stu-id="b4614-183">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="b4614-184">UrlPrefix Strings (UrlPrefix-Zeichenfolgen)</span><span class="sxs-lookup"><span data-stu-id="b4614-184">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="b4614-185">In den folgenden Ressourcen finden Sie detaillierte Anweisungen zu verschiedenen Szenarios.</span><span class="sxs-lookup"><span data-stu-id="b4614-185">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="b4614-186">Artikel, die sich auf `HttpListener` beziehen, beziehen sich auch auf `WebListener`, da beide Tools auf Http.Sys basieren.</span><span class="sxs-lookup"><span data-stu-id="b4614-186">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="b4614-187">Vorgehensweise: Konfigurieren eines Ports mit einem SSL-Zertifikat</span><span class="sxs-lookup"><span data-stu-id="b4614-187">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="b4614-188">[HTTPS Communication – HttpListener based Hosting and Client Certification (HTTPS-Kommunikation: HttpListener basierend auf Hosting- und Clientzertifizierung)](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) Hierbei handelt es sich um einen Blog eines Drittanbieters, der zwar schon recht alt ist, aber trotzdem nützliche Informationen enthält.</span><span class="sxs-lookup"><span data-stu-id="b4614-188">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="b4614-189">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server (Vorgehensweise: Exemplarische Vorgehensweise zum Verwenden von nicht verwaltetem Code (C++) für HttpListener oder Http-Server als einfacher SSL-Server)](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) Hierbei handelt es sich ebenfalls um einen älteren Blog mit nützlichen Informationen.</span><span class="sxs-lookup"><span data-stu-id="b4614-189">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="b4614-190">How Do I Set Up A .NET Core WebListener With SSL? (Einrichten eines .NET Core-WebListeners mit SSL)</span><span class="sxs-lookup"><span data-stu-id="b4614-190">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="b4614-191">Die Verwendung der folgenden Tools von Drittanbietern ist möglicherweise einfacher als die Verwendung der netsh.exe-Befehlszeile.</span><span class="sxs-lookup"><span data-stu-id="b4614-191">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="b4614-192">Diese Tools werden allerdings von Microsoft weder bereitgestellt noch unterstützt.</span><span class="sxs-lookup"><span data-stu-id="b4614-192">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="b4614-193">Die Tools werden standardmäßig als Administratoren ausgeführt, da „netsh.exe“ Administratorberechtigungen erfordert.</span><span class="sxs-lookup"><span data-stu-id="b4614-193">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="b4614-194">Der [Http.sys-Manager](http://httpsysmanager.codeplex.com/) stellt eine Benutzeroberfläche bereit, die zum Auflisten und Konfigurieren von SSL-Zertifikaten und -Optionen, Präfixreservierungen und Zertifikatsvertrauenslisten verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="b4614-194">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="b4614-195">Mithilfe von [HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) können Sie SSL-Zertifikate und URL-Präfixe auflisten und konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="b4614-195">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="b4614-196">Die Benutzeroberfläche ist besser optimiert als beim Http.sys-Manager und beinhaltet einige zusätzliche Optionen für die Konfiguration. Ansonsten sind die Funktionen allerdings ähnlich.</span><span class="sxs-lookup"><span data-stu-id="b4614-196">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="b4614-197">Es können zwar keine neuen Zertifikatsvertrauenslisten erstellt werden, jedoch können bereits vorhandene Listen hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="b4614-197">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="b4614-198">Wenn Sie selbstsignierte SSL-Zertifikate generieren möchten, können Sie die Befehlszeilenprogramme [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) und das PowerShell-Cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate) von Microsoft verwenden.</span><span class="sxs-lookup"><span data-stu-id="b4614-198">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="b4614-199">Es gibt außerdem Benutzeroberflächentools, die Ihnen das Generieren selbstsignierter SSL-Zertifikate erleichtern:</span><span class="sxs-lookup"><span data-stu-id="b4614-199">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="b4614-200">SelfCert</span><span class="sxs-lookup"><span data-stu-id="b4614-200">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="b4614-201">Makecert UI</span><span class="sxs-lookup"><span data-stu-id="b4614-201">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="b4614-202">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="b4614-202">Next steps</span></span>

<span data-ttu-id="b4614-203">Weitere Informationen finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="b4614-203">For more information, see the following resources:</span></span>

* [<span data-ttu-id="b4614-204">Beispiel-App zu diesem Artikel</span><span class="sxs-lookup"><span data-stu-id="b4614-204">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="b4614-205">WebListener source code (Quellcode für den WebListener)</span><span class="sxs-lookup"><span data-stu-id="b4614-205">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="b4614-206">Hosting</span><span class="sxs-lookup"><span data-stu-id="b4614-206">Hosting</span></span>](xref:fundamentals/host/index)
