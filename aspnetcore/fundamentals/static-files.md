---
title: Arbeiten mit statischen Dateien in ASP.NET Core
author: rick-anderson
description: Informationen Sie zum Arbeiten mit statischen Dateien in ASP.NET Core.
keywords: ASP.NET Core, statische Dateien, statische Assets, HTML, CSS und JavaScript
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0751576a1391f26f045c3f8c42ea39c0ff6e5d9
ms.sourcegitcommit: e4fb6b13be56a0fb2f2778623740a047d6489227
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2017
---
# <a name="working-with-static-files-in-aspnet-core"></a><span data-ttu-id="53fff-104">Arbeiten mit statischen Dateien in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53fff-104">Working with static files in ASP.NET Core</span></span>

<a name="fundamentals-static-files"></a>

<span data-ttu-id="53fff-105">Von [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="53fff-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="53fff-106">Statische Dateien, z. B. HTML, CSS, Bild und JavaScript, sind Ressourcen, die eine ASP.NET Core app direkt an Clients fungieren kann.</span><span class="sxs-lookup"><span data-stu-id="53fff-106">Static files, such as HTML, CSS, image, and JavaScript, are assets that an ASP.NET Core app can serve directly to clients.</span></span>

<span data-ttu-id="53fff-107">[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="53fff-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serving-static-files"></a><span data-ttu-id="53fff-108">Bereitstellen statischer Dateien</span><span class="sxs-lookup"><span data-stu-id="53fff-108">Serving static files</span></span>

<span data-ttu-id="53fff-109">Statische Dateien befinden sich in der Regel der `web root` (*\<Content-Stamm > / "Wwwroot"*) Ordner.</span><span class="sxs-lookup"><span data-stu-id="53fff-109">Static files are typically located in the `web root` (*\<content-root>/wwwroot*) folder.</span></span> <span data-ttu-id="53fff-110">Finden Sie unter [Content Stamm](xref:fundamentals/index#content-root) und [Stammweb](xref:fundamentals/index#web-root) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="53fff-110">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span> <span data-ttu-id="53fff-111">In der Regel legen Sie Inhalt Stammverzeichnis der aktuellen Verzeichnis zu sein, damit Ihres Projekts `web root` wird in der Entwicklung gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="53fff-111">You generally set the content root to be the current directory so that your project's `web root` will be found while in development.</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]

<span data-ttu-id="53fff-112">Statische Dateien gespeichert werden können, in einem beliebigen Ordner unter dem `web root` und mit einem relativen Pfad mit dem Stamm zugegriffen.</span><span class="sxs-lookup"><span data-stu-id="53fff-112">Static files can be stored in any folder under the `web root` and accessed with a relative path to that root.</span></span> <span data-ttu-id="53fff-113">Beispielsweise, wenn Sie einen Standard-Webanwendungsprojekt mithilfe von Visual Studio erstellen, sind mehrere Ordner erstellt, die innerhalb der *"Wwwroot"* Ordner - *Css*, *Bilder*, und *Js*.</span><span class="sxs-lookup"><span data-stu-id="53fff-113">For example, when you create a default Web application project using Visual Studio, there are several folders created within the *wwwroot*  folder - *css*, *images*, and *js*.</span></span> <span data-ttu-id="53fff-114">Der URI ein Bild im Zugriff auf die *Bilder* Unterordner:</span><span class="sxs-lookup"><span data-stu-id="53fff-114">The URI to access an image in the *images* subfolder:</span></span>

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

<span data-ttu-id="53fff-115">Damit kann für statische Dateien verarbeitet werden, müssen Sie konfigurieren die [Middleware](middleware.md) um statische Dateien an die Pipeline hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="53fff-115">In order for static files to be served, you must configure the [Middleware](middleware.md) to add static files to the pipeline.</span></span> <span data-ttu-id="53fff-116">Die Middleware für statische Dateien konfiguriert werden kann, durch Hinzufügen einer Abhängigkeit auf der *Microsoft.AspNetCore.StaticFiles* Paket in Ihr Projekt aufrufen und anschließend die `UseStaticFiles` Erweiterungsmethode über `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="53fff-116">The static file middleware can be configured by adding a dependency on the *Microsoft.AspNetCore.StaticFiles* package to your project and then calling the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

<span data-ttu-id="53fff-117">`app.UseStaticFiles();`Stellt die Dateien im `web root` (*"Wwwroot"* standardmäßig) servable.</span><span class="sxs-lookup"><span data-stu-id="53fff-117">`app.UseStaticFiles();` makes the files in `web root` (*wwwroot* by default) servable.</span></span> <span data-ttu-id="53fff-118">Später zeige ich wie andere Verzeichnisinhalt mit servable `UseStaticFiles`.</span><span class="sxs-lookup"><span data-stu-id="53fff-118">Later I'll show how to make other directory contents servable with `UseStaticFiles`.</span></span>

<span data-ttu-id="53fff-119">Sie müssen das NuGet-Paket "Microsoft.AspNetCore.StaticFiles" einschließen.</span><span class="sxs-lookup"><span data-stu-id="53fff-119">You must include the NuGet package "Microsoft.AspNetCore.StaticFiles".</span></span>

> [!NOTE]
> <span data-ttu-id="53fff-120">`web root`wird standardmäßig auf die *"Wwwroot"* Directory, aber Sie können festlegen, die `web root` Verzeichnis mit `UseWebRoot`.</span><span class="sxs-lookup"><span data-stu-id="53fff-120">`web root` defaults to the *wwwroot* directory, but you can set the `web root` directory with `UseWebRoot`.</span></span>

<span data-ttu-id="53fff-121">Angenommen, Sie eine Projekthierarchie haben, in dem Sie dienen möchten auf statischen Dateien sich außerhalb befinden, der `web root`.</span><span class="sxs-lookup"><span data-stu-id="53fff-121">Suppose you have a project hierarchy where the static files you wish to serve are outside the `web root`.</span></span> <span data-ttu-id="53fff-122">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="53fff-122">For example:</span></span>

* <span data-ttu-id="53fff-123">wwwroot</span><span class="sxs-lookup"><span data-stu-id="53fff-123">wwwroot</span></span>
  * <span data-ttu-id="53fff-124">CSS</span><span class="sxs-lookup"><span data-stu-id="53fff-124">css</span></span>
  * <span data-ttu-id="53fff-125">Bilder</span><span class="sxs-lookup"><span data-stu-id="53fff-125">images</span></span>
  * <span data-ttu-id="53fff-126">...</span><span class="sxs-lookup"><span data-stu-id="53fff-126">...</span></span>
* <span data-ttu-id="53fff-127">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="53fff-127">MyStaticFiles</span></span>
  * <span data-ttu-id="53fff-128">Test.PNG</span><span class="sxs-lookup"><span data-stu-id="53fff-128">test.png</span></span>

<span data-ttu-id="53fff-129">Für eine dienstanforderung zum Zugreifen auf *test.png*, konfigurieren Sie die Middleware für statische Dateien wie folgt:</span><span class="sxs-lookup"><span data-stu-id="53fff-129">For a request to access *test.png*, configure the static files middleware as follows:</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]

<span data-ttu-id="53fff-130">Eine Anforderung zum `http://<app>/StaticFiles/test.png` dient der *test.png* Datei.</span><span class="sxs-lookup"><span data-stu-id="53fff-130">A request to `http://<app>/StaticFiles/test.png` will serve the *test.png* file.</span></span>

<span data-ttu-id="53fff-131">`StaticFileOptions()`Antwort-Header kann festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="53fff-131">`StaticFileOptions()` can set response headers.</span></span> <span data-ttu-id="53fff-132">Der folgende Code z. B. richtet aus Bereitstellung statischer Dateien der *"Wwwroot"* Ordner und legt die `Cache-Control` Header zum sie 10 Minuten (600 Sekunden) öffentlich zwischengespeichert werden:</span><span class="sxs-lookup"><span data-stu-id="53fff-132">For example, the code below sets up static file serving from the *wwwroot* folder and sets the `Cache-Control` header to make them publicly cacheable for 10 minutes (600 seconds):</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]

<span data-ttu-id="53fff-133">Die [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) Methode steht in den [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) Paket.</span><span class="sxs-lookup"><span data-stu-id="53fff-133">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method is available from the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span> <span data-ttu-id="53fff-134">Hinzufügen `using Microsoft.AspNetCore.Http;` auf Ihre *Csharp* Datei, wenn die Methode nicht verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="53fff-134">Add `using Microsoft.AspNetCore.Http;` to your *csharp* file if the method is unavailable.</span></span>

![Antwort-Header, die mit der Cache-Control-Header wurde hinzugefügt](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="53fff-136">Statische Autorisierung</span><span class="sxs-lookup"><span data-stu-id="53fff-136">Static file authorization</span></span>

<span data-ttu-id="53fff-137">Das Modul für statische Dateien bietet **keine** autorisierungsüberprüfungen vorgenommen.</span><span class="sxs-lookup"><span data-stu-id="53fff-137">The static file module provides **no** authorization checks.</span></span> <span data-ttu-id="53fff-138">Alle Dateien von bedient, u. a. die *"Wwwroot"* öffentlich verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="53fff-138">Any files served by it, including those under *wwwroot* are publicly available.</span></span> <span data-ttu-id="53fff-139">Um Dateien anhand der Autorisierung zu dienen:</span><span class="sxs-lookup"><span data-stu-id="53fff-139">To serve files based on authorization:</span></span>

* <span data-ttu-id="53fff-140">Speichern Sie sie außerhalb von *"Wwwroot"* und einem beliebigen Verzeichnis zugegriffen werden kann, um die Middleware für statische Dateien **und**</span><span class="sxs-lookup"><span data-stu-id="53fff-140">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>

* <span data-ttu-id="53fff-141">Dienen sie über eine Controlleraktion Zurückgeben einer `FileResult` Autorisierung wird angewendet</span><span class="sxs-lookup"><span data-stu-id="53fff-141">Serve them through a controller action, returning a `FileResult` where authorization is applied</span></span>

## <a name="enabling-directory-browsing"></a><span data-ttu-id="53fff-142">Aktivieren die Verzeichnissuche</span><span class="sxs-lookup"><span data-stu-id="53fff-142">Enabling directory browsing</span></span>

<span data-ttu-id="53fff-143">Verzeichnissuche ermöglicht dem Benutzer für Ihre Web-app, um eine Liste von Verzeichnissen und Dateien in einem angegebenen Verzeichnis anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="53fff-143">Directory browsing allows the user of your web app to see a list of directories and files within a specified directory.</span></span> <span data-ttu-id="53fff-144">Verzeichnissuche ist aus Sicherheitsgründen standardmäßig deaktiviert (siehe [Überlegungen](#considerations)).</span><span class="sxs-lookup"><span data-stu-id="53fff-144">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="53fff-145">Um die Verzeichnissuche zu aktivieren, rufen die `UseDirectoryBrowser` Erweiterungsmethode über `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="53fff-145">To enable directory browsing, call the `UseDirectoryBrowser` extension method from  `Startup.Configure`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]

<span data-ttu-id="53fff-146">Und fügen Sie die erforderlichen Dienste durch Aufrufen von `AddDirectoryBrowser` Erweiterungsmethode über `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="53fff-146">And add required services by calling `AddDirectoryBrowser` extension method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]

<span data-ttu-id="53fff-147">Der obige Code ermöglicht, Verzeichnissuche, der die *"Wwwroot" / Bilder* Ordner mit die URL http://\<app > / MyImages, mit Links zu einzelnen Dateien und Ordner:</span><span class="sxs-lookup"><span data-stu-id="53fff-147">The code above allows directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages, with links to each file and folder:</span></span>

![Verzeichnissuche](static-files/_static/dir-browse.png)

<span data-ttu-id="53fff-149">Finden Sie unter [Überlegungen](#considerations) auf die Sicherheitsrisiken beim Durchsuchen aktivieren.</span><span class="sxs-lookup"><span data-stu-id="53fff-149">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="53fff-150">Beachten Sie die beiden `app.UseStaticFiles` aufrufen.</span><span class="sxs-lookup"><span data-stu-id="53fff-150">Note the two `app.UseStaticFiles` calls.</span></span> <span data-ttu-id="53fff-151">Die erste Vorlage ist erforderlich, um CSS, Bilder und JavaScript in dienen der *"Wwwroot"* Ordners und der zweite Aufruf für die Verzeichnissuche, der die *"Wwwroot"-Images* Ordner mit die URL http://\<app > / MyImages:</span><span class="sxs-lookup"><span data-stu-id="53fff-151">The first one is required to serve the CSS, images and JavaScript in the *wwwroot* folder, and the second call for directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages:</span></span>

[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]

## <a name="serving-a-default-document"></a><span data-ttu-id="53fff-152">Ein Standarddokument bedient</span><span class="sxs-lookup"><span data-stu-id="53fff-152">Serving a default document</span></span>

<span data-ttu-id="53fff-153">Festlegen einer Standardstartseite kann Besucher der Website eines volumenprogramms starten, wenn Ihre Site besuchen.</span><span class="sxs-lookup"><span data-stu-id="53fff-153">Setting a default home page gives site visitors a place to start when visiting your site.</span></span> <span data-ttu-id="53fff-154">Rufen Sie in der Reihenfolge für Ihre Web-app, ohne dass der Benutzer den URI vollqualifiziert eine Standardseite dient, die `UseDefaultFiles` Erweiterungsmethode über `Startup.Configure` wie folgt.</span><span class="sxs-lookup"><span data-stu-id="53fff-154">In order for your Web app to serve a default page without the user having to fully qualify the URI, call the `UseDefaultFiles` extension method from `Startup.Configure` as follows.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]

> [!NOTE]
> <span data-ttu-id="53fff-155">`UseDefaultFiles`muss aufgerufen werden, bevor `UseStaticFiles` Standarddatei dienen.</span><span class="sxs-lookup"><span data-stu-id="53fff-155">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="53fff-156">`UseDefaultFiles`ist ein erneute URL-Writer, der tatsächlich die Datei dienen.</span><span class="sxs-lookup"><span data-stu-id="53fff-156">`UseDefaultFiles` is a URL re-writer that doesn't actually serve the file.</span></span> <span data-ttu-id="53fff-157">Sie müssen die Middleware für statische Dateien aktivieren (`UseStaticFiles`), die die Datei dient.</span><span class="sxs-lookup"><span data-stu-id="53fff-157">You must enable the static file middleware (`UseStaticFiles`) to serve the file.</span></span>

<span data-ttu-id="53fff-158">Mit `UseDefaultFiles`, Anforderungen an einen Ordner gesucht werden:</span><span class="sxs-lookup"><span data-stu-id="53fff-158">With `UseDefaultFiles`, requests to a folder will search for:</span></span>

* <span data-ttu-id="53fff-159">default.htm</span><span class="sxs-lookup"><span data-stu-id="53fff-159">default.htm</span></span>
* <span data-ttu-id="53fff-160">"default.HTML"</span><span class="sxs-lookup"><span data-stu-id="53fff-160">default.html</span></span>
* <span data-ttu-id="53fff-161">Index.htm</span><span class="sxs-lookup"><span data-stu-id="53fff-161">index.htm</span></span>
* <span data-ttu-id="53fff-162">index.html</span><span class="sxs-lookup"><span data-stu-id="53fff-162">index.html</span></span>

<span data-ttu-id="53fff-163">Die erste Datei, die aus der Liste gefunden wird, als ob die Anforderung den voll gekennzeichneten URI war (obwohl die Browser-URL angezeigt, die angeforderten URI weiterhin) bedient.</span><span class="sxs-lookup"><span data-stu-id="53fff-163">The first file found from the list will be served as if the request was the fully qualified URI (although the browser URL will continue to show the URI requested).</span></span>

<span data-ttu-id="53fff-164">Der folgende Code zeigt, wie so ändern Sie den Standardnamen für die Datei in *mydefault.html*.</span><span class="sxs-lookup"><span data-stu-id="53fff-164">The following code shows how to change the default file name to *mydefault.html*.</span></span>

[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]

## <a name="usefileserver"></a><span data-ttu-id="53fff-165">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="53fff-165">UseFileServer</span></span>

<span data-ttu-id="53fff-166">`UseFileServer`kombiniert die Funktionalität der `UseStaticFiles`, `UseDefaultFiles`, und `UseDirectoryBrowser`.</span><span class="sxs-lookup"><span data-stu-id="53fff-166">`UseFileServer` combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="53fff-167">Der folgende Code ermöglicht, statische Dateien und die Standarddatei, die bereitgestellt werden, aber keine Verzeichnissuche zulässt:</span><span class="sxs-lookup"><span data-stu-id="53fff-167">The following code enables static files and the default file to be served, but does not allow directory browsing:</span></span>

```csharp
app.UseFileServer();
   ```

<span data-ttu-id="53fff-168">Der folgende Code ermöglicht, statische Dateien "," Standarddateien "und" Verzeichnis durchsuchen:</span><span class="sxs-lookup"><span data-stu-id="53fff-168">The following code enables static files, default files and  directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

<span data-ttu-id="53fff-169">Finden Sie unter [Überlegungen](#considerations) auf die Sicherheitsrisiken beim Durchsuchen aktivieren.</span><span class="sxs-lookup"><span data-stu-id="53fff-169">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span> <span data-ttu-id="53fff-170">Wie bei `UseStaticFiles`, `UseDefaultFiles`, und `UseDirectoryBrowser`, wenn Sie möchten, dass Sie Dateien dienen, die außerhalb der `web root`, instanziieren und konfigurieren eine `FileServerOptions` -Objekt, das Sie als Parameter an übergeben `UseFileServer`.</span><span class="sxs-lookup"><span data-stu-id="53fff-170">As with `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`, if you wish to serve files that exist outside the `web root`, you instantiate and configure an `FileServerOptions` object that you pass as a parameter to `UseFileServer`.</span></span> <span data-ttu-id="53fff-171">Betrachten Sie z. B. die folgenden Verzeichnishierarchie in der Web-app:</span><span class="sxs-lookup"><span data-stu-id="53fff-171">For example, given the following directory hierarchy in your Web app:</span></span>

* <span data-ttu-id="53fff-172">wwwroot</span><span class="sxs-lookup"><span data-stu-id="53fff-172">wwwroot</span></span>

  * <span data-ttu-id="53fff-173">CSS</span><span class="sxs-lookup"><span data-stu-id="53fff-173">css</span></span>

  * <span data-ttu-id="53fff-174">Bilder</span><span class="sxs-lookup"><span data-stu-id="53fff-174">images</span></span>

  * <span data-ttu-id="53fff-175">...</span><span class="sxs-lookup"><span data-stu-id="53fff-175">...</span></span>

* <span data-ttu-id="53fff-176">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="53fff-176">MyStaticFiles</span></span>

  * <span data-ttu-id="53fff-177">Test.PNG</span><span class="sxs-lookup"><span data-stu-id="53fff-177">test.png</span></span>

  * <span data-ttu-id="53fff-178">"default.HTML"</span><span class="sxs-lookup"><span data-stu-id="53fff-178">default.html</span></span>

<span data-ttu-id="53fff-179">Verwenden die oben genannten hierarchiebeispiel, möglicherweise möchten statische Dateien, Standarddateien und Durchsuchen ermöglichen die `MyStaticFiles` Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="53fff-179">Using the hierarchy example above, you might want to enable static files, default files, and browsing for the `MyStaticFiles` directory.</span></span> <span data-ttu-id="53fff-180">Im folgenden Codeausschnitt erfolgt, die mit einem einzigen Aufruf `FileServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="53fff-180">In the following code snippet, that is accomplished with a single call to `FileServerOptions`.</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]

<span data-ttu-id="53fff-181">Wenn `enableDirectoryBrowsing` festgelegt ist, um `true` aufrufen, müssen Sie `AddDirectoryBrowser` Erweiterungsmethode über `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="53fff-181">If `enableDirectoryBrowsing` is set to `true` you are required to call `AddDirectoryBrowser` extension method from  `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]

<span data-ttu-id="53fff-182">Verwenden die Dateihierarchie und oben angegeben Codes:</span><span class="sxs-lookup"><span data-stu-id="53fff-182">Using the file hierarchy and code above:</span></span>

| <span data-ttu-id="53fff-183">URI</span><span class="sxs-lookup"><span data-stu-id="53fff-183">URI</span></span>            |                             <span data-ttu-id="53fff-184">Antwort</span><span class="sxs-lookup"><span data-stu-id="53fff-184">Response</span></span>  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      <span data-ttu-id="53fff-185">MyStaticFiles/test.png</span><span class="sxs-lookup"><span data-stu-id="53fff-185">MyStaticFiles/test.png</span></span> |
| `http://<app>/StaticFiles`              |     <span data-ttu-id="53fff-186">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="53fff-186">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="53fff-187">Wenn keine Dateien standardmäßig in sind die *MyStaticFiles* Directory, http://\<app > / StaticFiles returns die Verzeichnisliste mit klickbaren Links:</span><span class="sxs-lookup"><span data-stu-id="53fff-187">If no default named files are in the *MyStaticFiles* directory, http://\<app>/StaticFiles returns the directory listing with clickable links:</span></span>

![Liste der statischen Dateien](static-files/_static/db2.PNG)

> [!NOTE]
> <span data-ttu-id="53fff-189">`UseDefaultFiles`und `UseDirectoryBrowser` dauert die Url http://\<app > / StaticFiles ohne nachstehenden Schrägstrich und Ursache eine clientseitige Umleitung zu http://\<app > /StaticFiles/ (das Hinzufügen des nachstehenden Schrägstrich).</span><span class="sxs-lookup"><span data-stu-id="53fff-189">`UseDefaultFiles` and `UseDirectoryBrowser` will take the url http://\<app>/StaticFiles without the trailing slash and cause a client side redirect to http://\<app>/StaticFiles/ (adding the trailing slash).</span></span> <span data-ttu-id="53fff-190">Ohne die nachgestellten Schrägstrich relativen URLs in den Dokumenten würden falsch sein.</span><span class="sxs-lookup"><span data-stu-id="53fff-190">Without the trailing slash relative URLs within the documents would be incorrect.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="53fff-191">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="53fff-191">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="53fff-192">Die `FileExtensionContentTypeProvider` Klasse enthält eine Auflistung, die MIME-Inhaltstypen Dateierweiterungen zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="53fff-192">The `FileExtensionContentTypeProvider` class contains a  collection that maps file extensions to MIME content types.</span></span> <span data-ttu-id="53fff-193">Im folgenden Beispiel wird mehrere Dateierweiterungen bekannten MIME-Typen registriert sind, ersetzt das ".rtf" und "mp4" wird entfernt.</span><span class="sxs-lookup"><span data-stu-id="53fff-193">In the following sample, several file extensions are registered to known MIME types, the ".rtf" is replaced, and ".mp4" is removed.</span></span>

[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]

<span data-ttu-id="53fff-194">Finden Sie unter [MIME-Inhaltstypen](http://www.iana.org/assignments/media-types/media-types.xhtml).</span><span class="sxs-lookup"><span data-stu-id="53fff-194">See   [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="53fff-195">Nicht standardmäßige Inhaltstypen</span><span class="sxs-lookup"><span data-stu-id="53fff-195">Non-standard content types</span></span>

<span data-ttu-id="53fff-196">Die ASP.NET-Middleware für statische Dateien erkennt fast 400 bekannten Dateiinhaltstypen.</span><span class="sxs-lookup"><span data-stu-id="53fff-196">The ASP.NET static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="53fff-197">Wenn der Benutzer eine Datei mit einer unbekannten Dateityp anfordert, gibt die Middleware für statische Dateien Antworten HTTP 404 (nicht gefunden) zurück.</span><span class="sxs-lookup"><span data-stu-id="53fff-197">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not found) response.</span></span> <span data-ttu-id="53fff-198">Wenn Verzeichnissuche aktiviert ist, wird ein Link zur Datei angezeigt werden, aber die URI einen HTTP 404-Fehler zurück.</span><span class="sxs-lookup"><span data-stu-id="53fff-198">If directory browsing is enabled, a link to the file will be displayed, but the URI will return an HTTP 404 error.</span></span>

<span data-ttu-id="53fff-199">Der folgende Code ermöglicht bedient unbekannte Typen und die unbekannte Datei als Bild rendert.</span><span class="sxs-lookup"><span data-stu-id="53fff-199">The following code enables serving unknown types and will render the unknown file as an image.</span></span>

[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]

<span data-ttu-id="53fff-200">Mit dem Code wird eine Anforderung für eine Datei mit einem unbekannten Inhaltstyp als Bild zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="53fff-200">With the code above, a request for a file with an unknown content type will be returned as an image.</span></span>

>[!WARNING]
> <span data-ttu-id="53fff-201">Aktivieren der `ServeUnknownFileTypes` stellt ein Sicherheitsrisiko, und verwenden, wird abgeraten.</span><span class="sxs-lookup"><span data-stu-id="53fff-201">Enabling `ServeUnknownFileTypes` is a security risk and using it is discouraged.</span></span>  <span data-ttu-id="53fff-202">`FileExtensionContentTypeProvider`(oben erläuterten) bietet eine sicherere Alternative zum Bereitstellen von Dateien mit standarderweiterungen.</span><span class="sxs-lookup"><span data-stu-id="53fff-202">`FileExtensionContentTypeProvider`  (explained above) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="53fff-203">Weitere Überlegungen</span><span class="sxs-lookup"><span data-stu-id="53fff-203">Considerations</span></span>

>[!WARNING]
> <span data-ttu-id="53fff-204">`UseDirectoryBrowser`und `UseStaticFiles` kann Offenlegung von geheimen Schlüsseln.</span><span class="sxs-lookup"><span data-stu-id="53fff-204">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="53fff-205">Es wird empfohlen, die Sie **nicht** Enable Verzeichnissuche in der Produktion.</span><span class="sxs-lookup"><span data-stu-id="53fff-205">We recommend that you **not** enable directory browsing in production.</span></span> <span data-ttu-id="53fff-206">Achten Sie darauf über die Verzeichnisse mit aktivieren `UseStaticFiles` oder `UseDirectoryBrowser` wie das gesamte Verzeichnis und alle Unterverzeichnisse zugegriffen werden können.</span><span class="sxs-lookup"><span data-stu-id="53fff-206">Be careful about which directories you enable with `UseStaticFiles` or `UseDirectoryBrowser` as the entire directory and all sub-directories will be accessible.</span></span> <span data-ttu-id="53fff-207">Es wird empfohlen, öffentliche Inhalte wie z. B. im Verzeichnis eigenen beibehalten  *\<Inhalts-Stamm > / "Wwwroot"*, Weg von Sichten, Konfigurationsdateien usw.</span><span class="sxs-lookup"><span data-stu-id="53fff-207">We recommend keeping public content in its own directory such as *\<content root>/wwwroot*, away from application views, configuration files, etc.</span></span>

* <span data-ttu-id="53fff-208">Die URLs für Inhalte, die verfügbar gemacht, mit `UseDirectoryBrowser` und `UseStaticFiles` unterliegen die Groß-/Kleinschreibung und zeichenbeschränkungen von ihren zugrunde liegenden Dateisystem.</span><span class="sxs-lookup"><span data-stu-id="53fff-208">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of their underlying file system.</span></span> <span data-ttu-id="53fff-209">Z. B. Windows wird Groß-/Kleinschreibung nicht beachtet, aber Mac und Linux befinden sich nicht.</span><span class="sxs-lookup"><span data-stu-id="53fff-209">For example, Windows is case insensitive, but Mac and Linux are not.</span></span>

* <span data-ttu-id="53fff-210">ASP.NET Core-Anwendungen, die in IIS gehostete verwenden ASP.NET Core-Modul, um alle Anforderungen an die Anwendung dies umfasst auch Anforderungen für statische Dateien weitergeleitet.</span><span class="sxs-lookup"><span data-stu-id="53fff-210">ASP.NET Core applications hosted in IIS use the ASP.NET Core Module to forward all requests to the application including requests for static files.</span></span> <span data-ttu-id="53fff-211">IIS Handler für statische Dateien wird nicht verwendet werden, weil er nicht abrufen, eine Möglichkeit zum Verarbeiten von Anforderungen, bevor sie das ASP.NET Core-Modul verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="53fff-211">The IIS static file handler is not used because it doesn't get a chance to handle requests before they are handled by the ASP.NET Core Module.</span></span>

* <span data-ttu-id="53fff-212">So entfernen Sie die IIS-Handler für statische Dateien (auf dem Server oder Website-Ebene):</span><span class="sxs-lookup"><span data-stu-id="53fff-212">To remove the IIS static file handler (at the server or website level):</span></span>

     * <span data-ttu-id="53fff-213">Navigieren Sie zu der **Module** Funktion</span><span class="sxs-lookup"><span data-stu-id="53fff-213">Navigate to the **Modules** feature</span></span>

     * <span data-ttu-id="53fff-214">Wählen Sie **StaticFileModule** in der Liste</span><span class="sxs-lookup"><span data-stu-id="53fff-214">Select **StaticFileModule** in the list</span></span>

     * <span data-ttu-id="53fff-215">Tippen Sie auf **entfernen** in der **Aktionen** Randleiste</span><span class="sxs-lookup"><span data-stu-id="53fff-215">Tap **Remove** in the **Actions** sidebar</span></span>

>[!WARNING]
> <span data-ttu-id="53fff-216">Wenn die IIS-Handler für statische Dateien aktiviert ist **und** der ASP.NET Core-Modul (ANCM) ist nicht richtig konfiguriert (z. B. wenn *"Web.config"* wurde nicht bereitgestellt), werden statische Dateien verarbeitet werden.</span><span class="sxs-lookup"><span data-stu-id="53fff-216">If the IIS static file handler is enabled **and** the ASP.NET Core Module (ANCM) is not correctly configured (for example if *web.config* was not deployed), static files will be served.</span></span>

* <span data-ttu-id="53fff-217">Codedateien (z. B. c# und Razor) sollte außerhalb der app-Projekt `web root` (*"Wwwroot"* standardmäßig).</span><span class="sxs-lookup"><span data-stu-id="53fff-217">Code files (including c# and Razor) should be placed outside of the app project's `web root` (*wwwroot* by default).</span></span> <span data-ttu-id="53fff-218">Dies erstellt eine saubere Trennung zwischen den Inhalt Ihrer app Client Side und serverseitigen Source Code, wodurch verhindert wird, dass serverseitigen Code zugreifen.</span><span class="sxs-lookup"><span data-stu-id="53fff-218">This creates a clean separation between your app's client side content and server side source code, which prevents server side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="53fff-219">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="53fff-219">Additional Resources</span></span>

* [<span data-ttu-id="53fff-220">Middleware</span><span class="sxs-lookup"><span data-stu-id="53fff-220">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="53fff-221">Einführung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53fff-221">Introduction to ASP.NET Core</span></span>](../index.md)
