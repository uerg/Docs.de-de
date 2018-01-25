---
title: WebListener webserverimplementierung in ASP.NET Core
author: rick-anderson
description: "Führt ein WebListener, einen Webserver für ASP.NET Core unter Windows. Basiert auf Http.Sys-Kernelmodustreiber und ist WebListener eine Alternative zum Kestrel, die für die direkte Verbindung mit dem Internet ohne IIS verwendet werden kann."
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/weblistener
ms.openlocfilehash: 5073a1663ec99a1b161092d74ab035ee9782becd
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="weblistener-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="bf9e9-104">WebListener webserverimplementierung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf9e9-104">WebListener web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="bf9e9-105">Durch [Tom Dykstra](https://github.com/tdykstra) und [Chris Ross](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="bf9e9-105">By [Tom Dykstra](https://github.com/tdykstra) and [Chris Ross](https://github.com/Tratcher)</span></span>

> [!NOTE]
> <span data-ttu-id="bf9e9-106">Dieses Thema gilt nur für ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-106">This topic applies only to ASP.NET Core 1.x.</span></span> <span data-ttu-id="bf9e9-107">In ASP.NET Core 2.0 WebListener heißt [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="bf9e9-107">In ASP.NET Core 2.0, WebListener is named [HTTP.sys](httpsys.md).</span></span>

<span data-ttu-id="bf9e9-108">WebListener ist ein [Webserver für ASP.NET Core](index.md) , die nur unter Windows ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-108">WebListener is a [web server for ASP.NET Core](index.md) that runs only on Windows.</span></span> <span data-ttu-id="bf9e9-109">Es basiert auf der [Http.Sys-Kernelmodustreiber](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="bf9e9-109">It's built on the [Http.Sys kernel mode driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="bf9e9-110">WebListener ist eine Alternative zum [Kestrel](kestrel.md) , für die direkte Verbindung mit dem Internet ohne Rückgriff auf IIS als reverse-Proxy-Server verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-110">WebListener is an alternative to [Kestrel](kestrel.md) that can be used for direct connection to the Internet without relying on IIS as a reverse proxy server.</span></span> <span data-ttu-id="bf9e9-111">In der Tat **WebListener kann nicht mit IIS oder IIS Express verwendet werden, als nicht kompatibel mit ist der [ASP.NET Core-Modul](aspnet-core-module.md).**</span><span class="sxs-lookup"><span data-stu-id="bf9e9-111">In fact, **WebListener can't be used with IIS or IIS Express, as it isn't compatible with the [ASP.NET Core Module](aspnet-core-module.md).**</span></span>

<span data-ttu-id="bf9e9-112">Obwohl WebListener für ASP.NET Core entwickelt wurde, kann es direkt in jeder beliebigen .NET Core oder .NET Framework-Anwendung über verwendet die [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-112">Although WebListener was developed for ASP.NET Core, it can be used directly in any .NET Core or .NET Framework application via the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

<span data-ttu-id="bf9e9-113">WebListener unterstützt die folgenden Funktionen:</span><span class="sxs-lookup"><span data-stu-id="bf9e9-113">WebListener supports the following features:</span></span>

- [<span data-ttu-id="bf9e9-114">Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="bf9e9-114">Windows Authentication</span></span>](xref:security/authentication/windowsauth)
- <span data-ttu-id="bf9e9-115">Anschlussfreigabe</span><span class="sxs-lookup"><span data-stu-id="bf9e9-115">Port sharing</span></span>
- <span data-ttu-id="bf9e9-116">HTTPS mit SNI</span><span class="sxs-lookup"><span data-stu-id="bf9e9-116">HTTPS with SNI</span></span>
- <span data-ttu-id="bf9e9-117">HTTP/2 über TLS (Windows 10)</span><span class="sxs-lookup"><span data-stu-id="bf9e9-117">HTTP/2 over TLS (Windows 10)</span></span>
- <span data-ttu-id="bf9e9-118">Direkte Übertragung</span><span class="sxs-lookup"><span data-stu-id="bf9e9-118">Direct file transmission</span></span>
- <span data-ttu-id="bf9e9-119">Zwischenspeichern von Antworten</span><span class="sxs-lookup"><span data-stu-id="bf9e9-119">Response caching</span></span>
- <span data-ttu-id="bf9e9-120">WebSockets (Windows 8)</span><span class="sxs-lookup"><span data-stu-id="bf9e9-120">WebSockets (Windows 8)</span></span>

<span data-ttu-id="bf9e9-121">Unterstützte Windows-Versionen:</span><span class="sxs-lookup"><span data-stu-id="bf9e9-121">Supported Windows versions:</span></span>

- <span data-ttu-id="bf9e9-122">Windows 7 und Windows Server 2008 R2 und höher</span><span class="sxs-lookup"><span data-stu-id="bf9e9-122">Windows 7 and Windows Server 2008 R2 and later</span></span>

<span data-ttu-id="bf9e9-123">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bf9e9-123">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-weblistener"></a><span data-ttu-id="bf9e9-124">WebListener verwenden</span><span class="sxs-lookup"><span data-stu-id="bf9e9-124">When to use WebListener</span></span>

<span data-ttu-id="bf9e9-125">WebListener eignet sich für Bereitstellungen, in dem Sie der Server direkt mit dem Internet verfügbar zu machen, ohne mit IIS müssen.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-125">WebListener is useful for deployments where you need to expose the server directly to the Internet without using IIS.</span></span>

![WebListener kommuniziert direkt mit dem Internet](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="bf9e9-127">Da er auf Http.Sys basiert, erfordern nicht WebListener einen reverse-Proxy-Server für den Schutz vor Angriffen durch.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-127">Because it's built on Http.Sys, WebListener doesn't require a reverse proxy server for protection against attacks.</span></span> <span data-ttu-id="bf9e9-128">Http.Sys ist ausgereifte Technologie, die für viele Arten von Angriffen geschützt und die Stabilität, Sicherheit und Skalierbarkeit von einem Webserver mit vollem Funktionsumfang.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-128">Http.Sys is mature technology that protects against many kinds of attacks and provides the robustness, security, and scalability of a full-featured web server.</span></span> <span data-ttu-id="bf9e9-129">IIS selbst wird als eine HTTP-Listener auf Http.Sys ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-129">IIS itself runs as an HTTP listener on top of Http.Sys.</span></span> 

<span data-ttu-id="bf9e9-130">WebListener ist auch eine gute Wahl für interne Bereitstellungen, wenn Sie eine der Funktionen benötigen, bietet, dass Sie mithilfe von Kestrel abrufen können.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-130">WebListener is also a good choice for internal deployments when you need one of the features it offers that you can't get by using Kestrel.</span></span>

![WebListener kommuniziert direkt mit Ihrem internen Netzwerk](weblistener/_static/weblistener-to-internal.png)

## <a name="how-to-use-weblistener"></a><span data-ttu-id="bf9e9-132">Gewusst wie: Verwenden von WebListener</span><span class="sxs-lookup"><span data-stu-id="bf9e9-132">How to use WebListener</span></span>

<span data-ttu-id="bf9e9-133">Hier ist eine Übersicht der Setupaufgaben für das Hostbetriebssystem und ASP.NET Core-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-133">Here's an overview of setup tasks for the host OS and your ASP.NET Core application.</span></span>

### <a name="configure-windows-server"></a><span data-ttu-id="bf9e9-134">Konfigurieren von WindowsServer</span><span class="sxs-lookup"><span data-stu-id="bf9e9-134">Configure Windows Server</span></span>

* <span data-ttu-id="bf9e9-135">Installieren Sie die Version von .NET, die Ihre Anwendung benötigt werden, z. B. [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) oder .NET Framework 4.5.1.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-135">Install the version of .NET that your application requires, such as [.NET Core](https://download.microsoft.com/download/0/A/3/0A372822-205D-4A86-BFA7-084D2CBE9EDF/DotNetCore.1.0.1-SDK.1.0.0.Preview2-003133-x64.exe) or .NET Framework 4.5.1.</span></span>

* <span data-ttu-id="bf9e9-136">Zu registrieren Sie URL-Präfixe zum Binden an WebListener und Einrichten von SSL-Zertifikaten</span><span class="sxs-lookup"><span data-stu-id="bf9e9-136">Preregister URL prefixes to bind to WebListener, and set up SSL certificates</span></span>

   <span data-ttu-id="bf9e9-137">Wenn Sie URL-Präfixe in Windows nicht vorab registrieren, müssen Sie die Anwendung mit Administratorrechten ausgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-137">If you don't preregister URL prefixes in Windows, you have to run your application with administrator privileges.</span></span> <span data-ttu-id="bf9e9-138">Die einzige Ausnahme ist, wenn Sie auf "localhost", die über HTTP (nicht "HTTPS") mit einer Portnummer, die größer als 1024 binden. In diesem Fall sind keine Administratorrechte erforderlich.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-138">The only exception is if you bind to localhost using HTTP (not HTTPS) with a port number greater than 1024; in that case administrator privileges aren't required.</span></span>

   <span data-ttu-id="bf9e9-139">Weitere Informationen finden Sie unter [zu Präfixe registrieren und Konfigurieren von SSL](#preregister-url-prefixes-and-configure-ssl) weiter unten in diesem Artikel.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-139">For details, see [How to preregister prefixes and configure SSL](#preregister-url-prefixes-and-configure-ssl) later in this article.</span></span>

* <span data-ttu-id="bf9e9-140">Öffnen Sie die Firewall-Ports zum Zulassen des Datenverkehrs WebListener zu erreichen.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-140">Open firewall ports to allow traffic to reach WebListener.</span></span>

   <span data-ttu-id="bf9e9-141">Sie können netsh.exe verwenden oder [PowerShell-Cmdlets](https://technet.microsoft.com/library/jj554906).</span><span class="sxs-lookup"><span data-stu-id="bf9e9-141">You can use netsh.exe or [PowerShell cmdlets](https://technet.microsoft.com/library/jj554906).</span></span>

<span data-ttu-id="bf9e9-142">Es gibt auch [Http.Sys-registrierungseinstellungen](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="bf9e9-142">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>

### <a name="configure-your-aspnet-core-application"></a><span data-ttu-id="bf9e9-143">Konfigurieren Sie die ASP.NET Core-Anwendung</span><span class="sxs-lookup"><span data-stu-id="bf9e9-143">Configure your ASP.NET Core application</span></span>

* <span data-ttu-id="bf9e9-144">Installieren Sie das NuGet-Paket [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span><span class="sxs-lookup"><span data-stu-id="bf9e9-144">Install the NuGet package [Microsoft.AspNetCore.Server.WebListener](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.WebListener/).</span></span> <span data-ttu-id="bf9e9-145">Dies installiert außerdem [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) als Abhängigkeit.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-145">This also installs [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) as a dependency.</span></span>

* <span data-ttu-id="bf9e9-146">Rufen Sie die `UseWebListener` Erweiterungsmethode auf [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in Ihrer `Main` Methode, dabei werden alle WebListener [Optionen](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) und [Einstellungen](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) , die Sie benötigen , wie im folgenden Beispiel gezeigt:</span><span class="sxs-lookup"><span data-stu-id="bf9e9-146">Call the `UseWebListener` extension method on [WebHostBuilder](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilder) in your `Main` method, specifying any WebListener [options](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.AspNetCore.Server.WebListener/WebListenerOptions.cs) and [settings](https://github.com/aspnet/HttpSysServer/blob/rel/1.1.2/src/Microsoft.Net.Http.Server/WebListenerSettings.cs) that you need, as shown in the following example:</span></span>

  [!code-csharp[](weblistener/sample/Program.cs?name=snippet_Main&highlight=13-17)]

* <span data-ttu-id="bf9e9-147">Konfigurieren von URLs und Ports Lauschen an</span><span class="sxs-lookup"><span data-stu-id="bf9e9-147">Configure URLs and ports to listen on</span></span> 

  <span data-ttu-id="bf9e9-148">Standardmäßig ASP.NET Core bindet an `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-148">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="bf9e9-149">Konfigurieren Sie URL-Präfixe und Ports können Sie die `UseURLs` Erweiterungsmethode, die `urls` Befehlszeilenargument oder das Konfigurationssystem ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-149">To configure URL prefixes and ports, you can use the `UseURLs` extension method, the `urls` command-line argument or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="bf9e9-150">Weitere Informationen finden Sie unter [Hosting](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="bf9e9-150">For more information, see [Hosting](../../fundamentals/hosting.md).</span></span>

  <span data-ttu-id="bf9e9-151">Web-Listener verwendet die [Http.Sys Präfix Zeichenfolgenformate](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span><span class="sxs-lookup"><span data-stu-id="bf9e9-151">Web Listener uses the [Http.Sys prefix string formats](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx).</span></span> <span data-ttu-id="bf9e9-152">Es sind keine formatanforderungen Präfix Zeichenfolge, die für WebListener spezifisch sind.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-152">There are no prefix string format requirements that are specific to WebListener.</span></span>

  > [!NOTE]
  > <span data-ttu-id="bf9e9-153">Stellen Sie sicher, dass Sie angeben, dass die gleichen Präfixzeichenfolgen in `UseUrls` , die Sie auf dem Server zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-153">Make sure that you specify the same prefix strings in `UseUrls` that you preregister on the server.</span></span> 

* <span data-ttu-id="bf9e9-154">Stellen Sie sicher, dass Ihre Anwendung zum Ausführen von IIS oder IIS Express konfiguriert ist.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-154">Make sure your application isn't configured to run IIS or IIS Express.</span></span>

  <span data-ttu-id="bf9e9-155">Ist in Visual Studio das Standardprofil für den Start für IIS Express.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-155">In Visual Studio, the default launch profile is for IIS Express.</span></span>  <span data-ttu-id="bf9e9-156">Zum Ausführen des Projekts als Konsolenanwendung müssen Sie manuell das ausgewählte Profil zu ändern, wie im folgenden Screenshot gezeigt.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-156">To run the project as a console application you have to manually change the selected profile, as shown in the following screen shot.</span></span>

  ![Wählen Sie die Konsole app-Profil](weblistener/_static/vs-choose-profile.png)

## <a name="how-to-use-weblistener-outside-of-aspnet-core"></a><span data-ttu-id="bf9e9-158">Gewusst wie: Verwenden Sie WebListener außerhalb von ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bf9e9-158">How to use WebListener outside of ASP.NET Core</span></span>

* <span data-ttu-id="bf9e9-159">Installieren der [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet-Paket.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-159">Install the [Microsoft.Net.Http.Server](https://www.nuget.org/packages/Microsoft.Net.Http.Server/) NuGet package.</span></span>

* <span data-ttu-id="bf9e9-160">[Zu URL-Präfixe zum Binden an WebListener und Einrichten von SSL-Zertifikaten registrieren](#preregister-url-prefixes-and-configure-ssl) wie bei Verwendung in ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-160">[Preregister URL prefixes to bind to WebListener, and set up SSL certificates](#preregister-url-prefixes-and-configure-ssl) as you would for use in ASP.NET Core.</span></span>

<span data-ttu-id="bf9e9-161">Es gibt auch [Http.Sys-registrierungseinstellungen](https://support.microsoft.com/kb/820129).</span><span class="sxs-lookup"><span data-stu-id="bf9e9-161">There are also [Http.Sys registry settings](https://support.microsoft.com/kb/820129).</span></span>


<span data-ttu-id="bf9e9-162">Hier ist ein Codebeispiel, die WebListener Verwendung außerhalb von ASP.NET Core veranschaulicht:</span><span class="sxs-lookup"><span data-stu-id="bf9e9-162">Here's a code sample that demonstrates WebListener use outside of ASP.NET Core:</span></span>

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

## <a name="preregister-url-prefixes-and-configure-ssl"></a><span data-ttu-id="bf9e9-163">Zu URL-Präfixe registrieren und Konfigurieren von SSL</span><span class="sxs-lookup"><span data-stu-id="bf9e9-163">Preregister URL prefixes and configure SSL</span></span>

<span data-ttu-id="bf9e9-164">Sowohl IIS als auch WebListener basieren auf den zugrunde liegenden Http.Sys-Kernelmodustreiber zum Abhören von Anforderungen und Verarbeitung ursprüngliche.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-164">Both IIS and WebListener rely on the underlying Http.Sys kernel mode driver to listen for requests and do initial processing.</span></span> <span data-ttu-id="bf9e9-165">In IIS bietet die Verwaltungsbenutzeroberfläche eine relativ einfache Möglichkeit, alles zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-165">In IIS, the management UI gives you a relatively easy way to configure everything.</span></span> <span data-ttu-id="bf9e9-166">Allerdings müssen Sie bei Verwendung von WebListener Http.Sys selbst konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-166">However, if you're using WebListener you need to configure Http.Sys yourself.</span></span> <span data-ttu-id="bf9e9-167">Die integrierten Tool netsh.exe ist dafür.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-167">The built-in tool for doing that's netsh.exe.</span></span> 

<span data-ttu-id="bf9e9-168">Am häufigsten auszuführenden Aufgaben, denen Sie für netsh.exe verwenden müssen, sind Reservieren von URL-Präfixe und Zuweisen von SSL-Zertifikate.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-168">The most common tasks you need to use netsh.exe for are reserving URL prefixes and assigning SSL certificates.</span></span>

<span data-ttu-id="bf9e9-169">NetSh.exe ist ein benutzerfreundliches Tool für Anfänger verwenden nicht.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-169">NetSh.exe isn't an easy tool to use for beginners.</span></span> <span data-ttu-id="bf9e9-170">Das folgende Beispiel zeigt die absolute Mindestanforderungen zum Reservieren von URL-Präfixe für die Ports 80 und 443 erforderlich sind:</span><span class="sxs-lookup"><span data-stu-id="bf9e9-170">The following example shows the bare minimum needed to reserve URL prefixes for ports 80 and 443:</span></span>

```console
netsh http add urlacl url=http://+:80/ user=Users
netsh http add urlacl url=https://+:443/ user=Users
```

<span data-ttu-id="bf9e9-171">Im folgende Beispiel wird gezeigt, wie ein SSL-Zertifikat zugewiesen werden:</span><span class="sxs-lookup"><span data-stu-id="bf9e9-171">The following example shows how to assign an SSL certificate:</span></span>

```console
netsh http add sslcert ipport=0.0.0.0:443 certhash=MyCertHash_Here appid={00000000-0000-0000-0000-000000000000}".
```

<span data-ttu-id="bf9e9-172">So sieht die offizielle Dokumentation:</span><span class="sxs-lookup"><span data-stu-id="bf9e9-172">Here is the official reference documentation:</span></span>

* [<span data-ttu-id="bf9e9-173">Netsh-Befehle für Hypertext Transfer-Protokoll (HTTP)</span><span class="sxs-lookup"><span data-stu-id="bf9e9-173">Netsh Commands for Hypertext Transfer Protocol (HTTP)</span></span>](https://technet.microsoft.com/library/cc725882.aspx)
* [<span data-ttu-id="bf9e9-174">UrlPrefix Strings</span><span class="sxs-lookup"><span data-stu-id="bf9e9-174">UrlPrefix Strings</span></span>](https://msdn.microsoft.com/library/windows/desktop/aa364698.aspx)

<span data-ttu-id="bf9e9-175">Die folgenden Ressourcen bieten detaillierte Anweisungen für verschiedene Szenarien.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-175">The following resources provide detailed instructions for several scenarios.</span></span> <span data-ttu-id="bf9e9-176">Artikel, die auf verweisen `HttpListener` gelten gleichermaßen für `WebListener`, wie sowohl auf Http.Sys basieren.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-176">Articles that refer to `HttpListener` apply equally to `WebListener`, as both are based on Http.Sys.</span></span>

* [<span data-ttu-id="bf9e9-177">Vorgehensweise: Konfigurieren eines Ports mit einem SSL-Zertifikat</span><span class="sxs-lookup"><span data-stu-id="bf9e9-177">How to: Configure a Port with an SSL Certificate</span></span>](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/how-to-configure-a-port-with-an-ssl-certificate)
* <span data-ttu-id="bf9e9-178">[HTTPS-Kommunikation - HttpListener basierend Hosting und Clientzertifizierung](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) dies ein Drittanbieter-Blog und ist ziemlich ALT, aber weiterhin enthält nützliche Informationen.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-178">[HTTPS Communication - HttpListener based Hosting and Client Certification](http://sunshaking.blogspot.com/2012/11/https-communication-httplistener-based.html) This is a third-party blog and is fairly old but still has useful information.</span></span>
* <span data-ttu-id="bf9e9-179">[Gewusst wie: Exemplarische Vorgehensweise mithilfe von HttpListener oder HTTP-Server (C++) Code, wie eine einfache SSL-Server nicht verwaltete](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) Dies ist eine ältere Blog mit nützlichen Informationen.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-179">[How To: Walkthrough Using HttpListener or Http Server unmanaged code (C++) as an SSL Simple Server](https://blogs.msdn.microsoft.com/jpsanders/2009/09/29/how-to-walkthrough-using-httplistener-or-http-server-unmanaged-code-c-as-an-ssl-simple-server/) This too is an older blog with useful information.</span></span>
* [<span data-ttu-id="bf9e9-180">Wie richte ich ein .NET Core WebListener mit SSL?</span><span class="sxs-lookup"><span data-stu-id="bf9e9-180">How Do I Set Up A .NET Core WebListener With SSL?</span></span>](https://blogs.msdn.microsoft.com/timomta/2016/11/04/how-do-i-set-up-a-net-core-weblistener-with-ssl/)

<span data-ttu-id="bf9e9-181">Hier sind einige Drittanbieter-Tools, die einfacher zu verwenden als das netsh.exe-Befehlszeile werden können.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-181">Here are some third-party tools that can be easier to use than the netsh.exe command line.</span></span> <span data-ttu-id="bf9e9-182">Diese werden nicht von bereitgestellt oder Unterstützung von Microsoft.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-182">These are not provided by or endorsed by Microsoft.</span></span> <span data-ttu-id="bf9e9-183">Die Tools Ausführen als Administrator standardmäßig, da netsh.exe selbst Administratorrechte erforderlich.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-183">The tools run as administrator by default, since netsh.exe itself requires administrator privileges.</span></span>

* <span data-ttu-id="bf9e9-184">[HTTP.sys Manager](http://httpsysmanager.codeplex.com/) bietet eine Benutzeroberfläche Liste und Konfigurieren von SSL-Zertifikate und Optionen, Präfix Reservierungen und Zertifikatsvertrauenslisten.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-184">[http.sys Manager](http://httpsysmanager.codeplex.com/) provides UI for listing and configuring SSL certificates and options, prefix reservations, and certificate trust lists.</span></span> 
* <span data-ttu-id="bf9e9-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) können Sie aus, oder konfigurieren Sie SSL-Zertifikate und URL-Präfixen.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-185">[HttpConfig](http://www.stevestechspot.com/ABetterHttpcfg.aspx) lets you list or configure SSL certificates and URL prefixes.</span></span> <span data-ttu-id="bf9e9-186">Die Benutzeroberfläche als http.sys Manager verfeinerten und stellt einige weitere Konfigurationsoptionen zur Verfügung, jedoch andernfalls er verfügt über ähnliche Funktionen.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-186">The UI is more refined than http.sys Manager and exposes a few more configuration options, but otherwise it provides similar functionality.</span></span> <span data-ttu-id="bf9e9-187">Eine neue Zertifikatsvertrauensliste (CTL) kann nicht erstellt werden, aber vorhandene zuweisen können.</span><span class="sxs-lookup"><span data-stu-id="bf9e9-187">It cannot create a new certificate trust list (CTL), but can assign existing ones.</span></span>

<span data-ttu-id="bf9e9-188">Zum Generieren von selbstsignierten SSL-Zertifikate, bietet Microsoft Befehlszeilenprogramme: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) und das PowerShell-Cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="bf9e9-188">For generating self-signed SSL certificates, Microsoft provides command-line tools: [MakeCert.exe](https://msdn.microsoft.com/library/windows/desktop/aa386968) and the PowerShell cmdlet [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pki/new-selfsignedcertificate).</span></span> <span data-ttu-id="bf9e9-189">Es gibt auch Drittanbieter-UI-Tools, die Sie zum Generieren von selbstsignierten SSL-Zertifikate erleichtern:</span><span class="sxs-lookup"><span data-stu-id="bf9e9-189">There are also third-party UI tools that make it easier for you to generate self-signed SSL certificates:</span></span>

* [<span data-ttu-id="bf9e9-190">SelfCert</span><span class="sxs-lookup"><span data-stu-id="bf9e9-190">SelfCert</span></span>](https://www.pluralsight.com/blog/software-development/selfcert-create-a-self-signed-certificate-interactively-gui-or-programmatically-in-net)
* [<span data-ttu-id="bf9e9-191">MakeCert-Benutzeroberfläche</span><span class="sxs-lookup"><span data-stu-id="bf9e9-191">Makecert UI</span></span>](http://makecertui.codeplex.com/)

## <a name="next-steps"></a><span data-ttu-id="bf9e9-192">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="bf9e9-192">Next steps</span></span>

<span data-ttu-id="bf9e9-193">Weitere Informationen finden Sie in den folgenden Ressourcen:</span><span class="sxs-lookup"><span data-stu-id="bf9e9-193">For more information, see the following resources:</span></span>

* [<span data-ttu-id="bf9e9-194">Beispiel-app für diesen Artikel</span><span class="sxs-lookup"><span data-stu-id="bf9e9-194">Sample app for this article</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/weblistener/sample)
* [<span data-ttu-id="bf9e9-195">WebListener-Quellcode</span><span class="sxs-lookup"><span data-stu-id="bf9e9-195">WebListener source code</span></span>](https://github.com/aspnet/HttpSysServer/)
* [<span data-ttu-id="bf9e9-196">Hosting</span><span class="sxs-lookup"><span data-stu-id="bf9e9-196">Hosting</span></span>](../hosting.md)
