---
title: Arbeiten Sie mit statischen Dateien in ASP.NET Core
author: rick-anderson
description: Erfahren Sie, wie dienen, statische Dateien gesichert werden, und konfigurieren statischen Datei hosting Middleware-Verhalten in einer ASP.NET Core-Web-app.
keywords: ASP.NET Core, statische Dateien, statische Assets, HTML, CSS und JavaScript
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 912923860939a1d1dd91ccc79862e23f9095d161
ms.sourcegitcommit: a3e88639a6bcf8fb4d634036dac93130c464a097
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/18/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a><span data-ttu-id="f8825-104">Arbeiten Sie mit statischen Dateien in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f8825-104">Work with static files in ASP.NET Core</span></span>

<span data-ttu-id="f8825-105">Durch [Rick Anderson](https://twitter.com/RickAndMSFT) und [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="f8825-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="f8825-106">Statische Dateien, z. B. HTML, CSS, Bilder und JavaScript, sind Objekte, die eine ASP.NET Core app direkt an Clients dient.</span><span class="sxs-lookup"><span data-stu-id="f8825-106">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="f8825-107">Einige Konfigurationen ist erforderlich, um die Bereitstellung dieser Dateien ausgeführt werden kann.</span><span class="sxs-lookup"><span data-stu-id="f8825-107">Some configuration is required to enable to serving of these files.</span></span>

<span data-ttu-id="f8825-108">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f8825-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="f8825-109">Statischen Dateien</span><span class="sxs-lookup"><span data-stu-id="f8825-109">Serve static files</span></span>

<span data-ttu-id="f8825-110">Statische Dateien sind im Web-Stammverzeichnis des Projekts gespeichert.</span><span class="sxs-lookup"><span data-stu-id="f8825-110">Static files are stored within your project's web root directory.</span></span> <span data-ttu-id="f8825-111">Das Standardverzeichnis ist  *\<Content_root > / "Wwwroot"*, aber sie kann geändert werden, über die [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) Methode.</span><span class="sxs-lookup"><span data-stu-id="f8825-111">The default directory is *\<content_root>/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="f8825-112">Finden Sie unter [Content Stamm](xref:fundamentals/index#content-root) und [Stammweb](xref:fundamentals/index#web-root) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="f8825-112">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="f8825-113">Webhost, der die app muss für das Stammverzeichnis der Inhalte zur Kenntnis.</span><span class="sxs-lookup"><span data-stu-id="f8825-113">The app's web host must be made aware of the content root directory.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f8825-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f8825-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f8825-115">Die `WebHost.CreateDefaultBuilder` Methode legt die Inhalten-Stamm im aktuellen Verzeichnis:</span><span class="sxs-lookup"><span data-stu-id="f8825-115">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f8825-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f8825-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f8825-117">Legen Sie die Inhalten-Stamm auf das aktuelle Verzeichnis durch den Aufruf [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) innerhalb eines `Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="f8825-117">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

<span data-ttu-id="f8825-118">Statische Dateien werden über einen Pfad relativ zum Webstamm zugegriffen.</span><span class="sxs-lookup"><span data-stu-id="f8825-118">Static files are accessible via a path relative to the web root.</span></span> <span data-ttu-id="f8825-119">Z. B. die **Webanwendung** -Projektvorlage enthält mehrere Ordner innerhalb der *"Wwwroot"* Ordner:</span><span class="sxs-lookup"><span data-stu-id="f8825-119">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="f8825-120">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="f8825-120">**wwwroot**</span></span>
  * <span data-ttu-id="f8825-121">**css**</span><span class="sxs-lookup"><span data-stu-id="f8825-121">**css**</span></span>
  * <span data-ttu-id="f8825-122">**images**</span><span class="sxs-lookup"><span data-stu-id="f8825-122">**images**</span></span>
  * <span data-ttu-id="f8825-123">**js**</span><span class="sxs-lookup"><span data-stu-id="f8825-123">**js**</span></span>

<span data-ttu-id="f8825-124">URI-Format, um den Zugriff auf eine Datei in die *Bilder* Unterordner *http://\<einfügen >/Images /\<Bilddateiname >*.</span><span class="sxs-lookup"><span data-stu-id="f8825-124">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="f8825-125">Beispielsweise *Http://localhost:9189/images/banner3.svg*.</span><span class="sxs-lookup"><span data-stu-id="f8825-125">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f8825-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f8825-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="f8825-127">Wenn .NET Framework abzielen, fügen Sie der [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) Paket Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="f8825-127">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span> <span data-ttu-id="f8825-128">Wenn Sie .NET Core als Ziel der [Microsoft.AspNetCore.All Metapackage](xref:fundamentals/metapackage) dieses Paket enthält.</span><span class="sxs-lookup"><span data-stu-id="f8825-128">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f8825-129">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f8825-129">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="f8825-130">Hinzufügen der [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) Paket Ihrem Projekt.</span><span class="sxs-lookup"><span data-stu-id="f8825-130">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span>

---

<span data-ttu-id="f8825-131">Konfigurieren der [Middleware](xref:fundamentals/middleware) können die Bereitstellung statischer Dateien.</span><span class="sxs-lookup"><span data-stu-id="f8825-131">Configure the [middleware](xref:fundamentals/middleware) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="f8825-132">Dateien Sie in der Webstamm</span><span class="sxs-lookup"><span data-stu-id="f8825-132">Serve files inside of web root</span></span>

<span data-ttu-id="f8825-133">Aufrufen der [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) Methode innerhalb `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="f8825-133">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="f8825-134">Die parameterlose `UseStaticFiles` -methodenüberladung, die Dateien im Webstammverzeichnis als servable markiert.</span><span class="sxs-lookup"><span data-stu-id="f8825-134">The parameterless `UseStaticFiles` method overload marks the files in web root as servable.</span></span> <span data-ttu-id="f8825-135">Die folgenden Markup Verweise *wwwroot/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="f8825-135">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="f8825-136">Bereitstellen von Dateien außerhalb der Webstamm</span><span class="sxs-lookup"><span data-stu-id="f8825-136">Serve files outside of web root</span></span>

<span data-ttu-id="f8825-137">Beachten Sie, eine Verzeichnishierarchie, in der statischen Dateien verarbeitet werden außerhalb der Webstamm befinden:</span><span class="sxs-lookup"><span data-stu-id="f8825-137">Consider a directory hierarchy in which the static files to be served reside outside of the web root:</span></span>

* <span data-ttu-id="f8825-138">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="f8825-138">**wwwroot**</span></span>
  * <span data-ttu-id="f8825-139">**css**</span><span class="sxs-lookup"><span data-stu-id="f8825-139">**css**</span></span>
  * <span data-ttu-id="f8825-140">**images**</span><span class="sxs-lookup"><span data-stu-id="f8825-140">**images**</span></span>
  * <span data-ttu-id="f8825-141">**js**</span><span class="sxs-lookup"><span data-stu-id="f8825-141">**js**</span></span>
* <span data-ttu-id="f8825-142">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="f8825-142">**MyStaticFiles**</span></span>
  * <span data-ttu-id="f8825-143">**images**</span><span class="sxs-lookup"><span data-stu-id="f8825-143">**images**</span></span>
      * <span data-ttu-id="f8825-144">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="f8825-144">*banner1.svg*</span></span>

<span data-ttu-id="f8825-145">Eine Anforderung erreichen der *banner1.svg* Datei durch die Middleware für statische Dateien wie folgt konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="f8825-145">A request can access the *banner1.svg* file by configuring the static file middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="f8825-146">Im vorangehenden Code der *MyStaticFiles* Verzeichnishierarchie wird öffentlich verfügbar gemacht über die *StaticFiles* URI-Segment.</span><span class="sxs-lookup"><span data-stu-id="f8825-146">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="f8825-147">Eine Anforderung zum *http://\<einfügen > /StaticFiles/images/banner1.svg* dient der *banner1.svg* Datei.</span><span class="sxs-lookup"><span data-stu-id="f8825-147">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="f8825-148">Die folgenden Markup Verweise *MyStaticFiles/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="f8825-148">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="f8825-149">HTTP-Antwortheader festlegen</span><span class="sxs-lookup"><span data-stu-id="f8825-149">Set HTTP response headers</span></span>

<span data-ttu-id="f8825-150">Ein [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) Objekt kann verwendet werden, um HTTP-Antwortheader festlegen.</span><span class="sxs-lookup"><span data-stu-id="f8825-150">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="f8825-151">Zusätzlich zum Konfigurieren von statischen dateibereitstellung über das Stammverzeichnis, den folgenden code wird die `Cache-Control` Header:</span><span class="sxs-lookup"><span data-stu-id="f8825-151">In addition to configuring static file serving from the web root, the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="f8825-152">Die [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) Methode vorhanden ist, der [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) Paket.</span><span class="sxs-lookup"><span data-stu-id="f8825-152">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="f8825-153">Die Dateien wurden für 10 Minuten (600 Sekunden) öffentlich zwischenspeicherbaren vorgenommen:</span><span class="sxs-lookup"><span data-stu-id="f8825-153">The files have been made publicly cacheable for 10 minutes (600 seconds):</span></span>

![Antwort-Header, die mit der Cache-Control-Header wurde hinzugefügt](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="f8825-155">Statische Autorisierung</span><span class="sxs-lookup"><span data-stu-id="f8825-155">Static file authorization</span></span>

<span data-ttu-id="f8825-156">Die Middleware für statische Dateien stellt keine autorisierungsprüfungen bereit.</span><span class="sxs-lookup"><span data-stu-id="f8825-156">The static file middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="f8825-157">Alle Dateien von bedient, u. a. die *"Wwwroot"*, öffentlich zugänglich sind.</span><span class="sxs-lookup"><span data-stu-id="f8825-157">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="f8825-158">Um Dateien anhand der Autorisierung zu dienen:</span><span class="sxs-lookup"><span data-stu-id="f8825-158">To serve files based on authorization:</span></span>

* <span data-ttu-id="f8825-159">Speichern Sie sie außerhalb von *"Wwwroot"* und einem beliebigen Verzeichnis zugegriffen werden kann, um die Middleware für statische Dateien **und**</span><span class="sxs-lookup"><span data-stu-id="f8825-159">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>
* <span data-ttu-id="f8825-160">Verarbeiten Sie sie über eine Aktionsmethode, die auf die Autorisierung angewendet wird.</span><span class="sxs-lookup"><span data-stu-id="f8825-160">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="f8825-161">Zurückgeben einer [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) Objekt:</span><span class="sxs-lookup"><span data-stu-id="f8825-161">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="f8825-162">Aktiviert die Verzeichnissuche</span><span class="sxs-lookup"><span data-stu-id="f8825-162">Enable directory browsing</span></span>

<span data-ttu-id="f8825-163">Verzeichnissuche kann Benutzer Ihre Web-app finden in einer Verzeichnisliste und Dateien in einem angegebenen Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="f8825-163">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="f8825-164">Verzeichnissuche ist aus Sicherheitsgründen standardmäßig deaktiviert (siehe [Überlegungen](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="f8825-164">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="f8825-165">Aktiviert Verzeichnissuche durch Aufrufen der [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) Methode in `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="f8825-165">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="f8825-166">Fügen Sie die erforderlichen Dienste durch Aufrufen der [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) Methode `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="f8825-166">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="f8825-167">Der vorangehende Code ermöglicht es, Verzeichnissuche, der die *"Wwwroot" / Bilder* Ordner mit der URL *http://\<einfügen > / MyImages*, mit Links zu einzelnen Dateien und Ordner:</span><span class="sxs-lookup"><span data-stu-id="f8825-167">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![Verzeichnissuche](static-files/_static/dir-browse.png)

<span data-ttu-id="f8825-169">Finden Sie unter [Überlegungen](#considerations) auf die Sicherheitsrisiken beim Durchsuchen aktivieren.</span><span class="sxs-lookup"><span data-stu-id="f8825-169">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="f8825-170">Beachten Sie die beiden `UseStaticFiles` im folgenden Beispiel aufruft.</span><span class="sxs-lookup"><span data-stu-id="f8825-170">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="f8825-171">Der erste Aufruf ermöglicht die Bereitstellung statischer Dateien in der *"Wwwroot"* Ordner.</span><span class="sxs-lookup"><span data-stu-id="f8825-171">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="f8825-172">Der zweite Aufruf aktiviert die Verzeichnissuche, der die *"Wwwroot" / Bilder* Ordner mit der URL *http://\<einfügen > / MyImages*:</span><span class="sxs-lookup"><span data-stu-id="f8825-172">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="f8825-173">Ein Standarddokument dienen</span><span class="sxs-lookup"><span data-stu-id="f8825-173">Serve a default document</span></span>

<span data-ttu-id="f8825-174">Festlegen einer Standard-Startseite bietet Besucher einen logischen Ausgangspunkt aus, wenn Ihre Site besuchen.</span><span class="sxs-lookup"><span data-stu-id="f8825-174">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="f8825-175">Aufrufen, ohne dass der Benutzer, die vollständig qualifizieren den URI eine Standardseite helfen, die [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) Methode `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="f8825-175">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="f8825-176">`UseDefaultFiles`muss aufgerufen werden, bevor `UseStaticFiles` Standarddatei dienen.</span><span class="sxs-lookup"><span data-stu-id="f8825-176">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="f8825-177">`UseDefaultFiles`ist eine URL-Rewrite-Modul, das tatsächlich die Datei dienen nicht.</span><span class="sxs-lookup"><span data-stu-id="f8825-177">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="f8825-178">Aktivieren Sie die Middleware für statische Dateien über `UseStaticFiles` auf die Datei bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="f8825-178">Enable the static file middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="f8825-179">Mit `UseDefaultFiles`, Anforderungen an einen Ordner suchen nach:</span><span class="sxs-lookup"><span data-stu-id="f8825-179">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="f8825-180">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="f8825-180">*default.htm*</span></span>
* <span data-ttu-id="f8825-181">*default.html*</span><span class="sxs-lookup"><span data-stu-id="f8825-181">*default.html*</span></span>
* <span data-ttu-id="f8825-182">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="f8825-182">*index.htm*</span></span>
* <span data-ttu-id="f8825-183">*index.html*</span><span class="sxs-lookup"><span data-stu-id="f8825-183">*index.html*</span></span>

<span data-ttu-id="f8825-184">Die erste Datei, die aus der Liste gefunden wird verarbeitet, als würde sich die Anforderung den voll gekennzeichneten URI.</span><span class="sxs-lookup"><span data-stu-id="f8825-184">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="f8825-185">Die Browser-URL wird entsprechend der angeforderten URI fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="f8825-185">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="f8825-186">Der folgende Code ändert den Standardnamen für die Datei in *mydefault.html*:</span><span class="sxs-lookup"><span data-stu-id="f8825-186">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="f8825-187">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="f8825-187">UseFileServer</span></span>

<span data-ttu-id="f8825-188">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) kombiniert die Funktionalität der `UseStaticFiles`, `UseDefaultFiles`, und `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="f8825-188">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="f8825-189">Der folgende Code ermöglicht die Bereitstellung statischer Dateien und die Standarddatei.</span><span class="sxs-lookup"><span data-stu-id="f8825-189">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="f8825-190">Verzeichnissuche ist nicht aktiviert.</span><span class="sxs-lookup"><span data-stu-id="f8825-190">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="f8825-191">Der folgende Code baut auf die parameterlose Überladung und Verzeichnissuche aktivieren:</span><span class="sxs-lookup"><span data-stu-id="f8825-191">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="f8825-192">Betrachten Sie die folgenden Verzeichnishierarchie aus:</span><span class="sxs-lookup"><span data-stu-id="f8825-192">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="f8825-193">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="f8825-193">**wwwroot**</span></span>
  * <span data-ttu-id="f8825-194">**css**</span><span class="sxs-lookup"><span data-stu-id="f8825-194">**css**</span></span>
  * <span data-ttu-id="f8825-195">**images**</span><span class="sxs-lookup"><span data-stu-id="f8825-195">**images**</span></span>
  * <span data-ttu-id="f8825-196">**js**</span><span class="sxs-lookup"><span data-stu-id="f8825-196">**js**</span></span>
* <span data-ttu-id="f8825-197">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="f8825-197">**MyStaticFiles**</span></span>
  * <span data-ttu-id="f8825-198">**images**</span><span class="sxs-lookup"><span data-stu-id="f8825-198">**images**</span></span>
      * <span data-ttu-id="f8825-199">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="f8825-199">*banner1.svg*</span></span>
  * <span data-ttu-id="f8825-200">*default.html*</span><span class="sxs-lookup"><span data-stu-id="f8825-200">*default.html*</span></span>

<span data-ttu-id="f8825-201">Der folgende Code zu ermöglichen, statische Dateien und Standarddateien und Verzeichnissuche von `MyStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="f8825-201">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="f8825-202">`AddDirectoryBrowser`muss aufgerufen werden, wenn die `EnableDirectoryBrowsing` Eigenschaftswert ist `true`:</span><span class="sxs-lookup"><span data-stu-id="f8825-202">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="f8825-203">Mithilfe der Dateihierarchie und vorangehenden Codes, beheben URLs wie folgt:</span><span class="sxs-lookup"><span data-stu-id="f8825-203">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="f8825-204">URI</span><span class="sxs-lookup"><span data-stu-id="f8825-204">URI</span></span>            |                             <span data-ttu-id="f8825-205">Antwort</span><span class="sxs-lookup"><span data-stu-id="f8825-205">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="f8825-206">*http://\<server_address>/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="f8825-206">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="f8825-207">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="f8825-207">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="f8825-208">*http://\<server_address>/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="f8825-208">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="f8825-209">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="f8825-209">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="f8825-210">Wenn keine Datei mit dem Namen standardmäßig in existiert die *MyStaticFiles* Verzeichnis *http://\<einfügen > / StaticFiles* gibt die Verzeichnisliste mit klickbaren Links:</span><span class="sxs-lookup"><span data-stu-id="f8825-210">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![Liste der statischen Dateien](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="f8825-212">`UseDefaultFiles`und `UseDirectoryBrowser` verwenden Sie die URL *http://\<einfügen > / StaticFiles* ohne dem nachgestellten Schrägstrich auslösen eine clientseitige Umleitung zu *http://\<einfügen > / StaticFiles /*.</span><span class="sxs-lookup"><span data-stu-id="f8825-212">`UseDefaultFiles` and `UseDirectoryBrowser` use the URL *http://\<server_address>/StaticFiles* without the trailing slash to trigger a client-side redirect to *http://\<server_address>/StaticFiles/*.</span></span> <span data-ttu-id="f8825-213">Beachten Sie die nachstehenden Schrägstrich.</span><span class="sxs-lookup"><span data-stu-id="f8825-213">Notice the addition of the trailing slash.</span></span> <span data-ttu-id="f8825-214">Relativer URLs in den Dokumenten gelten ohne einen nachstehenden Schrägstrich ungültig.</span><span class="sxs-lookup"><span data-stu-id="f8825-214">Relative URLs within the documents are deemed invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="f8825-215">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="f8825-215">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="f8825-216">Die [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) Klasse enthält eine `Mappings` Eigenschaft dient als eine Zuordnung von Dateierweiterungen, MIME-Inhaltstypen.</span><span class="sxs-lookup"><span data-stu-id="f8825-216">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="f8825-217">Im folgenden Beispiel werden mehrere Dateierweiterungen bekannten MIME-Typen registriert.</span><span class="sxs-lookup"><span data-stu-id="f8825-217">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="f8825-218">Die *.rtf* Erweiterung wird ersetzt, und *MP4* wird entfernt.</span><span class="sxs-lookup"><span data-stu-id="f8825-218">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="f8825-219">Finden Sie unter [MIME-Inhaltstypen](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="f8825-219">See [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="f8825-220">Nicht standardmäßige Inhaltstypen</span><span class="sxs-lookup"><span data-stu-id="f8825-220">Non-standard content types</span></span>

<span data-ttu-id="f8825-221">Die Middleware für statische Dateien erkennt fast 400 bekannten Dateiinhaltstypen.</span><span class="sxs-lookup"><span data-stu-id="f8825-221">The static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="f8825-222">Wenn der Benutzer eine Datei mit einer unbekannten Dateityp anfordert, gibt die Middleware für statische Dateien Antworten HTTP 404 (Nichtgefunden) zurück.</span><span class="sxs-lookup"><span data-stu-id="f8825-222">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not Found) response.</span></span> <span data-ttu-id="f8825-223">Wenn die Verzeichnissuche aktiviert ist, wird ein Link zur Datei angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f8825-223">If directory browsing is enabled, a link to the file is displayed.</span></span> <span data-ttu-id="f8825-224">Der URI zurückgegeben ein HTTP 404-Fehler.</span><span class="sxs-lookup"><span data-stu-id="f8825-224">The URI returns an HTTP 404 error.</span></span>

<span data-ttu-id="f8825-225">Der folgende Code ermöglicht, dienen die unbekannte Typen und rendert die unbekannte Datei als Bild:</span><span class="sxs-lookup"><span data-stu-id="f8825-225">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="f8825-226">Das vorhergehende Codebeispiel wird eine Anforderung für eine Datei mit einem unbekannten Inhaltstyp als Bild zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="f8825-226">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="f8825-227">Aktivieren der [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) stellt ein Sicherheitsrisiko dar.</span><span class="sxs-lookup"><span data-stu-id="f8825-227">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="f8825-228">Es ist standardmäßig deaktiviert, und seine Verwendung wird abgeraten.</span><span class="sxs-lookup"><span data-stu-id="f8825-228">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="f8825-229">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span><span class="sxs-lookup"><span data-stu-id="f8825-229">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="f8825-230">Weitere Überlegungen</span><span class="sxs-lookup"><span data-stu-id="f8825-230">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="f8825-231">`UseDirectoryBrowser`und `UseStaticFiles` kann Offenlegung von geheimen Schlüsseln.</span><span class="sxs-lookup"><span data-stu-id="f8825-231">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="f8825-232">Deaktivieren von Verzeichnissuche in der Produktion wird dringend empfohlen.</span><span class="sxs-lookup"><span data-stu-id="f8825-232">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="f8825-233">Überprüfen Sie sorgfältig, welche Verzeichnisse aktiviert sind, über `UseStaticFiles` oder `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="f8825-233">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="f8825-234">Das gesamte Verzeichnis und seinen Unterverzeichnissen werden öffentlich zugegriffen werden kann.</span><span class="sxs-lookup"><span data-stu-id="f8825-234">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="f8825-235">Store-Dateien für die Bereitstellung für die Öffentlichkeit geeignet ist in einem dedizierten Verzeichnis, z. B.  *\<Content_root > / "Wwwroot"*.</span><span class="sxs-lookup"><span data-stu-id="f8825-235">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="f8825-236">Trennen Sie diese Dateien von MVC-Ansichten mit Razor-Seiten (nur 2.x), Konfigurationsdateien usw. ein.</span><span class="sxs-lookup"><span data-stu-id="f8825-236">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="f8825-237">Die URLs für Inhalte, die verfügbar gemacht, mit `UseDirectoryBrowser` und `UseStaticFiles` unterliegen die Groß-/Kleinschreibung und zeichenbeschränkungen des zugrunde liegenden Dateisystems.</span><span class="sxs-lookup"><span data-stu-id="f8825-237">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="f8825-238">Windows ist z. B. Groß-/Kleinschreibung beachten&mdash;Mac und Linux nicht.</span><span class="sxs-lookup"><span data-stu-id="f8825-238">For example, Windows is case insensitive&mdash;Mac and Linux aren't.</span></span>

* <span data-ttu-id="f8825-239">ASP.NET Core apps gehostet wird, verwendet IIS die [ASP.NET Core Modul (ANCM)](xref:fundamentals/servers/aspnet-core-module) alle Anforderungen an die app, einschließlich statische dateianforderungen weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="f8825-239">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module (ANCM)](xref:fundamentals/servers/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="f8825-240">Die IIS-Handler für statische Dateien wird nicht verwendet.</span><span class="sxs-lookup"><span data-stu-id="f8825-240">The IIS static file handler isn't used.</span></span> <span data-ttu-id="f8825-241">Er verfügt über keine Möglichkeit zum Verarbeiten von Anforderungen, bevor sie von der ANCM behandelt werden.</span><span class="sxs-lookup"><span data-stu-id="f8825-241">It has no chance to handle requests before they're handled by the ANCM.</span></span>

* <span data-ttu-id="f8825-242">Führen Sie die folgenden Schritte im IIS-Manager, um IIS-Handler für statische Dateien auf den Server oder die Website-Ebene zu entfernen:</span><span class="sxs-lookup"><span data-stu-id="f8825-242">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="f8825-243">Navigieren Sie zu der **Module** Funktion.</span><span class="sxs-lookup"><span data-stu-id="f8825-243">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="f8825-244">Wählen Sie **StaticFileModule** in der Liste.</span><span class="sxs-lookup"><span data-stu-id="f8825-244">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="f8825-245">Klicken Sie auf **entfernen** in der **Aktionen** Randleiste.</span><span class="sxs-lookup"><span data-stu-id="f8825-245">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="f8825-246">Wenn die IIS-Handler für statische Dateien aktiviert ist **und** der ANCM ist falsch konfiguriert, um statische Dateien verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="f8825-246">If the IIS static file handler is enabled **and** the ANCM is configured incorrectly, static files are served.</span></span> <span data-ttu-id="f8825-247">Dies geschieht beispielsweise, wenn die *"Web.config"* Datei wird nicht bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="f8825-247">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="f8825-248">Platzieren Sie die Codedateien (einschließlich *cs* und *cshtml*) außerhalb der Web-Stamm des app-Projekts.</span><span class="sxs-lookup"><span data-stu-id="f8825-248">Place code files (including *.cs* and *.cshtml*) outside of the app project's web root.</span></span> <span data-ttu-id="f8825-249">Eine logische Trennung wird zwischen der app die clientseitige Inhalt und serverbasierten Code aus diesem Grund erstellt.</span><span class="sxs-lookup"><span data-stu-id="f8825-249">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="f8825-250">Dadurch wird verhindert, dass serverseitiger Code zugreifen.</span><span class="sxs-lookup"><span data-stu-id="f8825-250">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8825-251">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="f8825-251">Additional resources</span></span>

* [<span data-ttu-id="f8825-252">Middleware</span><span class="sxs-lookup"><span data-stu-id="f8825-252">Middleware</span></span>](xref:fundamentals/middleware)

* [<span data-ttu-id="f8825-253">Einführung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f8825-253">Introduction to ASP.NET Core</span></span>](xref:index)