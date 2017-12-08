---
uid: ajax/cdn/overview
title: Microsoft Ajax Content Delivery Network | Microsoft Docs
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/14/2017
ms.topic: article
ms.assetid: 8935bf14-ca6d-4a4e-9dbe-b96ce74cef49
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /ajax/cdn
msc.type: content
ms.openlocfilehash: 2955888bc745fa3d49e40d364196283f16ad95ef
ms.sourcegitcommit: 9ecd4e9fb0c40c3693dab079eab1ff94b461c922
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/28/2017
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="7b5fe-102">Microsoft Ajax Content Delivery Network</span><span class="sxs-lookup"><span data-stu-id="7b5fe-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="7b5fe-103">Hinweis: Das Microsoft Ajax-CDN kann keine SLA-Größe mit einer Azure-CDN.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="7b5fe-104">Inhaltsverzeichnis</span><span class="sxs-lookup"><span data-stu-id="7b5fe-104">Table of Contents</span></span>

<span data-ttu-id="7b5fe-105">**[AJAX.Microsoft.com in ajax.aspnetcdn.com umbenannt](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="7b5fe-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="7b5fe-106">**[Unterstützung von Visual Studio .vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="7b5fe-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="7b5fe-107">**[Mithilfe von ASP.NET Ajax vom CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="7b5fe-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="7b5fe-108">**[Unter Verwendung von jQuery vom CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="7b5fe-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="7b5fe-109">**[Unter Verwendung von jQuery UI vom CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="7b5fe-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="7b5fe-110">**[Drittanbieter-Dateien auf dem CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="7b5fe-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="7b5fe-111">jQuery-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="7b5fe-112">Migrieren von jQuery-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="7b5fe-113">jQuery UI-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="7b5fe-114">jQuery-Validierung Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="7b5fe-115">jQuery Mobile-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="7b5fe-116">jQuery-Vorlagen-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="7b5fe-117">jQuery-Zyklus Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="7b5fe-118">jQuery DataTables Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="7b5fe-119">Modernizr Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="7b5fe-120">JSHint Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="7b5fe-121">Knockout Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="7b5fe-122">Bei der Globalisierung Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="7b5fe-123">Reagieren Sie auf das CDN Versionen</span><span class="sxs-lookup"><span data-stu-id="7b5fe-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="7b5fe-124">Bootstrap-Versionen auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="7b5fe-125">Bootstrap TouchCarousel Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="7b5fe-126">Hammer.js Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="7b5fe-127">ASP.NET Web Forms und Ajax-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="7b5fe-128">ASP.NET MVC frei, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="7b5fe-129">ASP.NET SignalR frei, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="7b5fe-130">Der Microsoft Ajax Content Delivery Network (CDN) hostet beliebten Drittanbieter-JavaScript-Bibliotheken wie z. B. jQuery und ermöglicht es Ihnen, einfach die Webanwendungen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="7b5fe-131">Sie können z. B. starten, unter Verwendung von jQuery der gehostet wird auf diesem CDN einfach durch Hinzufügen einer &lt;Skript&gt; -Tag, um die Seite, die auf ajax.aspnetcdn.com verweist.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="7b5fe-132">Durch das CDN nutzen, können Sie die Leistung der Ajax-Anwendungen erheblich verbessern.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="7b5fe-133">Auf Servern, die auf der ganzen Welt werden der Inhalt des CDN zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="7b5fe-134">Darüber hinaus ermöglicht das CDN Browsern zum Wiederverwenden von zwischengespeicherten Drittanbieter-JavaScript-Dateien für Websites, die in unterschiedlichen Domänen befinden.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="7b5fe-135">Das CDN unterstützt SSL (HTTPS) an, für den Fall, dass Sie einer Webseite mithilfe des Secure Sockets Layers bereitstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="7b5fe-136">Das CDN hostet die folgenden Drittanbieter-Skriptbibliotheken hochgeladen worden sein, und für Sie lizenziert sind, die von den Besitzern von diesen Bibliotheken:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="7b5fe-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="7b5fe-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="7b5fe-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="7b5fe-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="7b5fe-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="7b5fe-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="7b5fe-140">jQuery-Validierung (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="7b5fe-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="7b5fe-141">jQuery-Zyklus (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="7b5fe-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="7b5fe-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="7b5fe-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="7b5fe-143">Das Microsoft Ajax-CDN umfasst außerdem die folgenden Bibliotheken, die von Microsoft hochgeladen wurden:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="7b5fe-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="7b5fe-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="7b5fe-145">ASP.NET MVC-JavaScript-Dateien</span><span class="sxs-lookup"><span data-stu-id="7b5fe-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="7b5fe-146">ASP.NET SignalR JavaScript-Dateien</span><span class="sxs-lookup"><span data-stu-id="7b5fe-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="7b5fe-147">Microsoft erhebt keine Ansprüche für den Besitz von Drittanbieter-Bibliotheken, die auf diesem CDN gehostet.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="7b5fe-148">Diese Bibliotheken sind den Urheberrechtsinhabern der Bibliotheken entschieden haben.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="7b5fe-149">Alle Rechte, die Sie möglicherweise herunterladen und verwenden diese Bibliotheken werden ausschließlich von der jeweiligen Urheberrechtsinhabern gewährt.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="7b5fe-150">Da keine Microsoft-Bibliotheken sind, bietet Microsoft keine GEWÄHRLEISTUNGEN oder geistiges Eigentum Rechte Lizenzen (einschließlich keine implizite Patentrechte), für die Drittanbieter-Bibliotheken, die auf diesem CDN gehostet.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="7b5fe-151">Wenn Sie JavaScript-Bibliothek senden möchten, und die Bibliothek eine der oben JavaScript-Bibliotheken ist (wie auf http://trends.builtwith.com aufgeführt) oder Erweiterungen/Plugins, um diese Bibliotheken, die (a) beliebten; oder (b) hilfreich für auf ASP.NET verwenden, und wenden Sie sich an AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="7b5fe-152">AJAX.Microsoft.com in ajax.aspnetcdn.com umbenannt</span><span class="sxs-lookup"><span data-stu-id="7b5fe-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="7b5fe-153">Das CDN verwendet, um den Domänennamen "Microsoft.com" verwenden und wurde geändert, um den Domänennamen aspnetcdn.com verwenden.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="7b5fe-154">Diese Änderung wurde vorgenommen, um die Leistung zu erhöhen, da bei ein Browser mit die Domäne "Microsoft.com" verwiesen wird er alle Cookies aus dieser Domäne über das Netz mit jeder Anforderung sendet.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="7b5fe-155">Durch das Umbenennen eines Domänennamens als "Microsoft.com" kann die Leistung von möglichst alle Aufgaben auf 25 % erhöht werden.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="7b5fe-156">Beachten Sie ajax.microsoft.com wird zwar weiterhin ajax.aspnetcdn.com wird jedoch empfohlen.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="7b5fe-157">Altes Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="7b5fe-158">Neues Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="7b5fe-159">Unterstützung von Visual Studio .vsdoc</span><span class="sxs-lookup"><span data-stu-id="7b5fe-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="7b5fe-160">Die .vsdoc Dateien ordnungsgemäß zu mit Visual Studio 2008 verwenden, müssen Sie sicherstellen, dass Sie Visual Studio 2008 SP1 verfügen, und der Hotfix für das Vsdoc-Dateien installiert.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="7b5fe-161">Sie können diese hier abrufen:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-161">You can get these from here:</span></span>

- [<span data-ttu-id="7b5fe-162">Herunterladen der Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Herunterladen der Visual Studio 2008 SP1")
- [<span data-ttu-id="7b5fe-163">Herunterladen von .vsdoc Hotfix für Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 ".vsdoc Hotfix für Visual Studio 2008 SP1 herunterladen")

<span data-ttu-id="7b5fe-164">Visual Studio 2010 unterstützt .vsdoc Dateien ohne zusätzliche Patches.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="7b5fe-165">Mithilfe von ASP.NET Ajax vom CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="7b5fe-166">Wenn ASP.NET 4 zu verwenden, können Sie alle Anforderungen für ASP.NET Frameworkskripts auf das CDN umleiten.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="7b5fe-167">Abrufen von Skripts aus dem CDN anstelle von Ihren lokalen Webserver kann die Leistung von öffentlichen ASP.NET-Websites erheblich verbessern.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="7b5fe-168">Verwenden Sie die ScriptManager EnableCDN-Eigenschaft, um alle ASP.NET Framework Skript Anforderungen an das Microsoft Ajax-CDN umleiten:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="7b5fe-169">Unter Verwendung von jQuery vom CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="7b5fe-170">Sie können die jQuery-Skripts, das durch Hinzufügen der folgenden Script-Element zu einer Seite in der Webanwendung auf CDN gehostet:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="7b5fe-171">Das CDN enthält auch die verkleinerte Version des jQuery-Skripts, die Sie abrufen können mithilfe des folgenden Elements:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="7b5fe-172">Damit wird die Seite fallback auf jQuery aus einem lokalen Pfad auf eine eigene Website laden, wenn das CDN nicht verfügbar ist, erfolgt, fügen Sie das folgende Element sofort nach dem Element, das Verweisen auf das CDN hinzu:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="7b5fe-173">Die folgenden Beispielseite verwendet die CDN-Version der jQuery-Bibliothek (mit Fallback auf eine lokale Kopie), zeigen Sie den Inhalt von einem Div-Element, wenn auf eine Schaltfläche geklickt wird.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="7b5fe-174">Sie können erfahren Sie mehr über jQuery und eine lokale Kopie des jQuery herunterzuladen, besuchen die [jQuery](http://jquery.com/) Website.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="7b5fe-175">Unter Verwendung von jQuery UI vom CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="7b5fe-176">Das CDN hostet auch die jQuery UI-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="7b5fe-177">Die jQuery UI-Bibliothek enthält einen umfangreichen Satz von Widgets und Effekte, die Sie in Ihrer ASP.NET-Anwendungen verwenden können.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="7b5fe-178">Die folgende Seite wird beispielsweise veranschaulicht, wie Sie dem jQuery UI Datepicker im Kontext einer ASP.NET Web Forms-Anwendung verwenden können, um ein Popupkalender angezeigt:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="7b5fe-179">Wenn Sie den Fokus auf das Textfeld mit der Tastatur zu verschieben, wird ein Kalender angezeigt:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Erstellt mit Datepicker-Popupkalenders](overview/_static/image1.png)

<span data-ttu-id="7b5fe-181">Beachten Sie, dass im obigen Code drei Dateien aus dem CDN aufnehmen müssen:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="7b5fe-182">Die jQuery-Bibliothek &mdash; die jQuery UI-Bibliothek für die jQuery-Bibliothek abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="7b5fe-183">Sie müssen die jQuery-Bibliothek auf der Seite vor dem Hinzufügen der jQuery UI-Bibliothek hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="7b5fe-184">Der jQuery UI-Bibliothek &mdash; die jQuery UI-Bibliothek enthält alle Benutzeroberflächeneffekte jQuery und Widgets, z. B. das Datepicker-Widget in der oben genannten Seite verwendet.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="7b5fe-185">Ein Design der jQuery UI &mdash; jQuery UI unterstützt verschiedene Designs.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="7b5fe-186">Die Seite enthält einen Link in einer CSS-Datei, die das Design "Redmond" zu importieren.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="7b5fe-187">Alle von der standardmäßigen jQuery UI-Designs werden auf das CDN gehostet.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="7b5fe-188">[Besuchen Sie diese Seite](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 auf das Microsoft Ajax-CDN") Miniaturansichten für jeden Design anzeigen.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="7b5fe-189">Weitere Informationen zu der jQuery UI-Bibliothek finden Sie auf der offiziellen [jQuery UI Website](http://jQueryUI.com "jQuery UI Website").</span><span class="sxs-lookup"><span data-stu-id="7b5fe-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="7b5fe-190">Drittanbieter-Dateien auf dem CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="7b5fe-191">Das CDN hostet einiger der beliebtesten JavaScript-Bibliotheken von Drittanbietern.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="7b5fe-192">Microsoft erhebt keine Ansprüche für den Besitz von Drittanbieter-Bibliotheken, die auf diesem CDN gehostet.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="7b5fe-193">Diese Bibliotheken sind den Urheberrechtsinhabern der Bibliotheken entschieden haben.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="7b5fe-194">Alle Rechte, die Sie möglicherweise herunterladen und verwenden diese Bibliotheken werden ausschließlich von der jeweiligen Urheberrechtsinhabern gewährt.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="7b5fe-195">Da keine Microsoft-Bibliotheken sind, bietet Microsoft keine GEWÄHRLEISTUNGEN oder geistiges Eigentum Rechte Lizenzen (einschließlich keine implizite Patentrechte), für die Drittanbieter-Bibliotheken, die auf diesem CDN gehostet.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-196">jQuery-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-197">Die folgenden Versionen von jQuery, die auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="7b5fe-198">jQuery-Version 3.2.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-198">jQuery version 3.2.1</span></span>
- <span data-ttu-id="7b5fe-199">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="7b5fe-200">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="7b5fe-201">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="7b5fe-202">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="7b5fe-203">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="7b5fe-204">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="7b5fe-205">jQuery-Version 3.2.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-205">jQuery version 3.2.0</span></span>

- <span data-ttu-id="7b5fe-206">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="7b5fe-207">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="7b5fe-208">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="7b5fe-209">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="7b5fe-210">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="7b5fe-211">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="7b5fe-212">jQuery-Version 3.1.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-212">jQuery version 3.1.1</span></span>

- <span data-ttu-id="7b5fe-213">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="7b5fe-214">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="7b5fe-215">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="7b5fe-216">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="7b5fe-217">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="7b5fe-218">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="7b5fe-219">jQuery-Version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-219">jQuery version 3.1.0</span></span>

- <span data-ttu-id="7b5fe-220">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="7b5fe-221">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="7b5fe-222">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="7b5fe-223">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="7b5fe-224">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="7b5fe-225">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="7b5fe-226">jQuery-Version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-226">jQuery version 3.0.0</span></span>

- <span data-ttu-id="7b5fe-227">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="7b5fe-228">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="7b5fe-229">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="7b5fe-230">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="7b5fe-231">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="7b5fe-232">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="7b5fe-233">jQuery-Version 2.2.4</span><span class="sxs-lookup"><span data-stu-id="7b5fe-233">jQuery version 2.2.4</span></span>

- <span data-ttu-id="7b5fe-234">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="7b5fe-235">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="7b5fe-236">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="7b5fe-237">jQuery-Version 2.2.3</span><span class="sxs-lookup"><span data-stu-id="7b5fe-237">jQuery version 2.2.3</span></span>

- <span data-ttu-id="7b5fe-238">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="7b5fe-239">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="7b5fe-240">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-240">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="7b5fe-241">jQuery-Version 2.2.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-241">jQuery version 2.2.2</span></span>

- <span data-ttu-id="7b5fe-242">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="7b5fe-243">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="7b5fe-244">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-244">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="7b5fe-245">jQuery-Version 2.2.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-245">jQuery version 2.2.1</span></span>

- <span data-ttu-id="7b5fe-246">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="7b5fe-247">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="7b5fe-248">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-248">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="7b5fe-249">jQuery-Version 2.2.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-249">jQuery version 2.2.0</span></span>

- <span data-ttu-id="7b5fe-250">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="7b5fe-251">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="7b5fe-252">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-252">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="7b5fe-253">jQuery-Version 2.1.4</span><span class="sxs-lookup"><span data-stu-id="7b5fe-253">jQuery version 2.1.4</span></span>

- <span data-ttu-id="7b5fe-254">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="7b5fe-255">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="7b5fe-256">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-256">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="7b5fe-257">jQuery-Version 2.1.3</span><span class="sxs-lookup"><span data-stu-id="7b5fe-257">jQuery version 2.1.3</span></span>

- <span data-ttu-id="7b5fe-258">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="7b5fe-259">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="7b5fe-260">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-260">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="7b5fe-261">jQuery-Version 2.1.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-261">jQuery version 2.1.2</span></span>

- <span data-ttu-id="7b5fe-262">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="7b5fe-263">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="7b5fe-264">jQuery-Version 2.1.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-264">jQuery version 2.1.1</span></span>

- <span data-ttu-id="7b5fe-265">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="7b5fe-266">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="7b5fe-267">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="7b5fe-268">jQuery-Version 2.1.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-268">jQuery version 2.1.0</span></span>

- <span data-ttu-id="7b5fe-269">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="7b5fe-270">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="7b5fe-271">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-271">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="7b5fe-272">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="7b5fe-273">jQuery-Version 2.0.3</span><span class="sxs-lookup"><span data-stu-id="7b5fe-273">jQuery version 2.0.3</span></span>

- <span data-ttu-id="7b5fe-274">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="7b5fe-275">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-275">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="7b5fe-276">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="7b5fe-277">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="7b5fe-278">jQuery-Version 2.0.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-278">jQuery version 2.0.2</span></span>

- <span data-ttu-id="7b5fe-279">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="7b5fe-280">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-280">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="7b5fe-281">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="7b5fe-282">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="7b5fe-283">jQuery-Version 2.0.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-283">jQuery version 2.0.1</span></span>

- <span data-ttu-id="7b5fe-284">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="7b5fe-285">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-285">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="7b5fe-286">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="7b5fe-287">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="7b5fe-288">jQuery-Version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-288">jQuery version 2.0.0</span></span>

- <span data-ttu-id="7b5fe-289">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="7b5fe-290">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-290">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="7b5fe-291">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="7b5fe-292">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="7b5fe-293">jQuery-Version 1.12.4</span><span class="sxs-lookup"><span data-stu-id="7b5fe-293">jQuery version 1.12.4</span></span>

- <span data-ttu-id="7b5fe-294">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="7b5fe-295">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-295">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="7b5fe-296">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="7b5fe-297">jQuery-Version 1.12.3</span><span class="sxs-lookup"><span data-stu-id="7b5fe-297">jQuery version 1.12.3</span></span>

- <span data-ttu-id="7b5fe-298">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="7b5fe-299">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="7b5fe-300">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-300">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="7b5fe-301">jQuery-Version 1.12.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-301">jQuery version 1.12.2</span></span>

- <span data-ttu-id="7b5fe-302">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="7b5fe-303">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="7b5fe-304">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-304">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="7b5fe-305">jQuery-Version 1.12.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-305">jQuery version 1.12.1</span></span>

- <span data-ttu-id="7b5fe-306">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="7b5fe-307">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="7b5fe-308">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-308">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="7b5fe-309">jQuery-Version 1.12.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-309">jQuery version 1.12.0</span></span>

- <span data-ttu-id="7b5fe-310">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="7b5fe-311">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="7b5fe-312">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-312">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="7b5fe-313">jQuery-Version 1.11.3</span><span class="sxs-lookup"><span data-stu-id="7b5fe-313">jQuery version 1.11.3</span></span>

- <span data-ttu-id="7b5fe-314">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="7b5fe-315">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="7b5fe-316">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-316">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="7b5fe-317">jQuery-Version 1.11.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-317">jQuery version 1.11.2</span></span>

- <span data-ttu-id="7b5fe-318">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="7b5fe-319">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="7b5fe-320">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-320">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="7b5fe-321">jQuery-Version 1.11.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-321">jQuery version 1.11.1</span></span>

- <span data-ttu-id="7b5fe-322">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="7b5fe-323">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="7b5fe-324">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-324">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="7b5fe-325">jQuery-Version 1.11.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-325">jQuery version 1.11.0</span></span>

- <span data-ttu-id="7b5fe-326">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="7b5fe-327">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="7b5fe-328">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-328">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="7b5fe-329">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="7b5fe-330">Version der jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-330">jQuery version 1.10.2</span></span>

- <span data-ttu-id="7b5fe-331">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="7b5fe-332">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-332">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="7b5fe-333">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="7b5fe-334">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="7b5fe-335">jQuery-Version 1.10.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-335">jQuery version 1.10.1</span></span>

- <span data-ttu-id="7b5fe-336">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="7b5fe-337">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-337">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="7b5fe-338">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="7b5fe-339">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="7b5fe-340">jQuery-Version 1.10.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-340">jQuery version 1.10.0</span></span>

- <span data-ttu-id="7b5fe-341">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="7b5fe-342">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-342">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="7b5fe-343">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="7b5fe-344">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="7b5fe-345">jQuery-Version 1.9.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-345">jQuery version 1.9.1</span></span>

- <span data-ttu-id="7b5fe-346">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="7b5fe-347">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-347">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="7b5fe-348">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="7b5fe-349">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="7b5fe-350">jQuery-Version 1.9.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-350">jQuery version 1.9.0</span></span>

- <span data-ttu-id="7b5fe-351">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="7b5fe-352">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-352">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="7b5fe-353">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="7b5fe-354">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="7b5fe-355">jQuery-Version 1.8.3</span><span class="sxs-lookup"><span data-stu-id="7b5fe-355">jQuery version 1.8.3</span></span>

- <span data-ttu-id="7b5fe-356">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="7b5fe-357">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-357">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="7b5fe-358">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="7b5fe-359">jQuery-Version 1.8.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-359">jQuery version 1.8.2</span></span>

- <span data-ttu-id="7b5fe-360">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="7b5fe-361">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="7b5fe-362">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-362">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="7b5fe-363">jQuery-Version 1.8.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-363">jQuery version 1.8.1</span></span>

- <span data-ttu-id="7b5fe-364">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="7b5fe-365">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="7b5fe-366">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-366">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="7b5fe-367">jQuery-Version 1.8.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-367">jQuery version 1.8.0</span></span>

- <span data-ttu-id="7b5fe-368">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="7b5fe-369">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="7b5fe-370">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-370">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="7b5fe-371">jQuery-Version 1.7.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-371">jQuery version 1.7.2</span></span>

- <span data-ttu-id="7b5fe-372">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="7b5fe-373">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="7b5fe-374">jQuery-Version 1.7.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-374">jQuery version 1.7.1</span></span>

- <span data-ttu-id="7b5fe-375">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="7b5fe-376">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="7b5fe-377">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="7b5fe-378">jQuery-Version 1.7</span><span class="sxs-lookup"><span data-stu-id="7b5fe-378">jQuery version 1.7</span></span>

- <span data-ttu-id="7b5fe-379">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="7b5fe-380">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="7b5fe-381">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-381">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="7b5fe-382">jQuery-Version 1.6.4</span><span class="sxs-lookup"><span data-stu-id="7b5fe-382">jQuery version 1.6.4</span></span>

- <span data-ttu-id="7b5fe-383">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="7b5fe-384">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="7b5fe-385">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-385">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="7b5fe-386">jQuery-Version 1.6.3</span><span class="sxs-lookup"><span data-stu-id="7b5fe-386">jQuery version 1.6.3</span></span>

- <span data-ttu-id="7b5fe-387">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="7b5fe-388">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="7b5fe-389">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-389">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="7b5fe-390">jQuery-Version 1.6.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-390">jQuery version 1.6.2</span></span>

- <span data-ttu-id="7b5fe-391">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="7b5fe-392">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="7b5fe-393">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-393">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="7b5fe-394">jQuery-Version 1.6.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-394">jQuery version 1.6.1</span></span>

- <span data-ttu-id="7b5fe-395">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="7b5fe-396">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="7b5fe-397">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-397">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="7b5fe-398">jQuery-Version 1.6</span><span class="sxs-lookup"><span data-stu-id="7b5fe-398">jQuery version 1.6</span></span>

- <span data-ttu-id="7b5fe-399">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="7b5fe-400">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="7b5fe-401">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-401">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="7b5fe-402">jQuery-Version 1.5.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-402">jQuery version 1.5.2</span></span>

- <span data-ttu-id="7b5fe-403">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="7b5fe-404">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="7b5fe-405">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-405">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="7b5fe-406">jQuery-Version 1.5.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-406">jQuery version 1.5.1</span></span>

- <span data-ttu-id="7b5fe-407">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="7b5fe-408">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="7b5fe-409">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-409">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="7b5fe-410">jQuery-Version 1.5</span><span class="sxs-lookup"><span data-stu-id="7b5fe-410">jQuery version 1.5</span></span>

- <span data-ttu-id="7b5fe-411">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="7b5fe-412">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="7b5fe-413">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-413">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="7b5fe-414">jQuery-Version 1.4.4</span><span class="sxs-lookup"><span data-stu-id="7b5fe-414">jQuery version 1.4.4</span></span>

- <span data-ttu-id="7b5fe-415">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="7b5fe-416">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="7b5fe-417">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-417">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="7b5fe-418">jQuery-Version 1.4.3</span><span class="sxs-lookup"><span data-stu-id="7b5fe-418">jQuery version 1.4.3</span></span>

- <span data-ttu-id="7b5fe-419">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="7b5fe-420">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="7b5fe-421">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-421">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="7b5fe-422">jQuery-Version 1.4.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-422">jQuery version 1.4.2</span></span>

- <span data-ttu-id="7b5fe-423">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="7b5fe-424">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="7b5fe-425">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-425">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="7b5fe-426">jQuery-Version 1.4.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-426">jQuery version 1.4.1</span></span>

- <span data-ttu-id="7b5fe-427">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="7b5fe-428">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="7b5fe-429">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-429">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="7b5fe-430">jQuery-Version 1.4</span><span class="sxs-lookup"><span data-stu-id="7b5fe-430">jQuery version 1.4</span></span>

- <span data-ttu-id="7b5fe-431">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="7b5fe-432">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="7b5fe-433">Version der jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-433">jQuery version 1.3.2</span></span>

- <span data-ttu-id="7b5fe-434">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="7b5fe-435">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="7b5fe-436">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="7b5fe-437">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.Min-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-437">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-438">Migrieren von jQuery-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-438">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-439">Die folgenden Versionen von jQuery migrieren, die auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-439">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="7b5fe-440">jQuery migrieren Version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-440">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="7b5fe-441">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-441">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="7b5fe-442">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-442">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="7b5fe-443">jQuery migrieren Version 1.2.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-443">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="7b5fe-444">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-444">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="7b5fe-445">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-445">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="7b5fe-446">jQuery migrieren Version 1.2.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-446">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="7b5fe-447">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-447">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="7b5fe-448">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="7b5fe-449">jQuery migrieren Version 1.1.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-449">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="7b5fe-450">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-450">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="7b5fe-451">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="7b5fe-452">jQuery migrieren Version 1.1.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-452">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="7b5fe-453">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-453">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="7b5fe-454">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="7b5fe-455">jQuery migrieren Version 1.0.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-455">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="7b5fe-456">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-456">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="7b5fe-457">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-458">jQuery UI-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-458">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-459">Die folgenden Versionen der jQuery UI-Bibliothek, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-459">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="7b5fe-460">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-460">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7b5fe-461">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-461">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-462">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-462">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-463">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="7b5fe-463">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-464">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="7b5fe-464">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-465">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-465">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-466">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-466">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-467">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-467">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-468">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="7b5fe-468">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-469">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="7b5fe-469">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-470">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-470">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery 1.10.2-Benutzeroberfläche auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-471">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-471">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-472">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-472">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-473">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-473">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-474">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-474">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-475">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-475">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-476">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="7b5fe-476">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-477">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="7b5fe-477">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-478">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="7b5fe-478">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-479">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="7b5fe-479">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-480">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="7b5fe-480">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-481">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="7b5fe-481">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-482">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="7b5fe-482">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-483">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="7b5fe-483">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-484">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="7b5fe-484">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-485">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="7b5fe-485">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-486">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="7b5fe-486">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-487">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="7b5fe-487">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-488">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="7b5fe-488">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-489">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="7b5fe-489">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-490">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="7b5fe-490">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-491">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="7b5fe-491">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-492">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="7b5fe-492">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-493">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="7b5fe-493">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-494">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="7b5fe-494">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-495">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="7b5fe-495">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-496">jQuery-Validierung Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-496">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-497">Die folgenden Versionen der jQuery-Validierung-Bibliothek, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-497">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="7b5fe-498">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-498">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7b5fe-499">jQuery Validate 1.17.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-499">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery-Validierung 1.17.0")
- [<span data-ttu-id="7b5fe-500">jQuery Validate 1.16.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-500">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery-Validierung 1.16.0")
- [<span data-ttu-id="7b5fe-501">jQuery Validate 1.15.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-501">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery-Validierung 1.15.1")
- [<span data-ttu-id="7b5fe-502">jQuery Validate 1.15.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-502">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery-Validierung 1.15.0")
- [<span data-ttu-id="7b5fe-503">jQuery Validate 1.14.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-503">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery-Validierung 1.14.0")
- [<span data-ttu-id="7b5fe-504">jQuery Validate 1.13.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-504">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery-Validierung 1.13.1")
- [<span data-ttu-id="7b5fe-505">jQuery Validate 1.13.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-505">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery-Validierung 1.13.0")
- [<span data-ttu-id="7b5fe-506">jQuery Validate 1.12.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-506">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery-Validierung 1.12.0")
- [<span data-ttu-id="7b5fe-507">jQuery Validate 1.11.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-507">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery-Validierung 1.11.1")
- [<span data-ttu-id="7b5fe-508">jQuery Validate 1.11.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-508">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery-Validierung 1.11.0")
- [<span data-ttu-id="7b5fe-509">jQuery Validate 1.10.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-509">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery-Validierung 1.10.0")
- [<span data-ttu-id="7b5fe-510">jQuery Validate 1.9</span><span class="sxs-lookup"><span data-stu-id="7b5fe-510">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate Version 1.9")
- [<span data-ttu-id="7b5fe-511">jQuery Validate 1.8.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-511">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate Version 1.8.1")
- [<span data-ttu-id="7b5fe-512">jQuery Validate 1.8</span><span class="sxs-lookup"><span data-stu-id="7b5fe-512">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate Version 1.8")
- [<span data-ttu-id="7b5fe-513">jQuery Validate 1.7</span><span class="sxs-lookup"><span data-stu-id="7b5fe-513">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate Version 1.7")
- [<span data-ttu-id="7b5fe-514">jQuery Validate 1.6</span><span class="sxs-lookup"><span data-stu-id="7b5fe-514">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery Validate 1.6")
- [<span data-ttu-id="7b5fe-515">jQuery Validate 1.5.5</span><span class="sxs-lookup"><span data-stu-id="7b5fe-515">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery Validate 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-516">jQuery Mobile-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-516">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-517">Die folgenden Versionen der jQuery Mobile-Bibliothek, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-517">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="7b5fe-518">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-518">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7b5fe-519">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="7b5fe-519">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-520">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-520">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-521">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-521">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-522">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-522">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-523">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-523">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-524">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-524">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-525">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-525">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-526">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-526">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-527">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-527">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-528">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-528">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-529">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-529">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-530">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-530">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-531">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-531">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-532">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-532">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-533">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-533">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-534">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-534">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="7b5fe-535">jQuery Mobile 1.0 Beta 3</span><span class="sxs-lookup"><span data-stu-id="7b5fe-535">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 Klicken Sie auf das Microsoft Ajax-CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-536">jQuery-Vorlagen-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-536">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-537">Die folgenden Versionen der jQuery-Vorlagen-Plug-Ins, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-537">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="7b5fe-538">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-538">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7b5fe-539">jQuery-Vorlagen Beta 1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-539">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery-Vorlagen Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-540">jQuery-Zyklus Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-540">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-541">Die folgenden Versionen der jQuery-Zyklus-Plug-Ins, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-541">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="7b5fe-542">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-542">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7b5fe-543">jQuery-Zyklus 2,99</span><span class="sxs-lookup"><span data-stu-id="7b5fe-543">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery Zyklus 2,99")
- [<span data-ttu-id="7b5fe-544">jQuery-Zyklus 2.94</span><span class="sxs-lookup"><span data-stu-id="7b5fe-544">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Zyklus 2.94")
- [<span data-ttu-id="7b5fe-545">jQuery-Zyklus 2,88</span><span class="sxs-lookup"><span data-stu-id="7b5fe-545">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery Zyklus 2,88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-546">jQuery DataTables Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-546">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-547">Die folgenden Versionen der jQuery DataTables-Plug-Ins, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-547">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="7b5fe-548">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-548">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7b5fe-549">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="7b5fe-549">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="7b5fe-550">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="7b5fe-550">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="7b5fe-551">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="7b5fe-551">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="7b5fe-552">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="7b5fe-552">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="7b5fe-553">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-553">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="7b5fe-554">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-554">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="7b5fe-555">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-555">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="7b5fe-556">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-556">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-557">Modernizr Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-557">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-558">Die folgenden Versionen von [Modernizr](http://www.modernizr.com "Modernizr") auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-558">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="7b5fe-559">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-559">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="7b5fe-560">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-560">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="7b5fe-561">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-561">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="7b5fe-562">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-562">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="7b5fe-563">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-563">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="7b5fe-564">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-564">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-565">JSHint Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-565">JSHint Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-566">Die folgenden Versionen von [JSHint](http://www.jshint.com "JSHint") auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-566">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="7b5fe-567">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-567">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-568">Knockout Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-568">Knockout Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-569">Die folgenden Versionen von [Knockout](http://www.knockoutjs.com "Knockout") auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-569">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="7b5fe-570">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-570">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="7b5fe-571">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-571">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="7b5fe-572">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-572">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="7b5fe-573">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-573">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="7b5fe-574">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-574">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="7b5fe-575">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-575">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="7b5fe-576">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-576">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="7b5fe-577">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="7b5fe-578">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="7b5fe-579">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="7b5fe-580">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="7b5fe-581">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="7b5fe-582">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="7b5fe-583">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="7b5fe-584">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="7b5fe-585">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="7b5fe-586">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="7b5fe-587">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="7b5fe-588">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="7b5fe-589">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.Debug.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-590">Bei der Globalisierung Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-590">Globalize Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-591">Die folgenden Versionen von [Globalize](https://github.com/jquery/globalize "Globalize") auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-591">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="7b5fe-592">Bei der Version 1.0.0 Globalisierung</span><span class="sxs-lookup"><span data-stu-id="7b5fe-592">Globalize version 1.0.0</span></span>

- <span data-ttu-id="7b5fe-593">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-593">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="7b5fe-594">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/Node-Main.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-594">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="7b5fe-595">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-595">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="7b5fe-596">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Date.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-596">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="7b5fe-597">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Message.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-597">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="7b5fe-598">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-598">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="7b5fe-599">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Plural.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-599">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="7b5fe-600">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/relative-Time.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="7b5fe-601">Bei der Globalisierung Version 0.1.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-601">Globalize version 0.1.1</span></span>

- <span data-ttu-id="7b5fe-602">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-602">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="7b5fe-603">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-603">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="7b5fe-604">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/Cultures/globalize.Cultures.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-604">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="7b5fe-605">alle Kulturen</span><span class="sxs-lookup"><span data-stu-id="7b5fe-605">all cultures</span></span>
- <span data-ttu-id="7b5fe-606">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/Cultures/globalize.Culture. {Kulturcode} .js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-606">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="7b5fe-607">Ersetzen Sie "{Kulturcode}" durch den gewünschten Kulturcode, z. B. Microsoft globalize.culture.en GB.js== Dateien auf das CDN == diese Bibliotheken von Microsoft hochgeladen wurden.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-607">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-608">Reagieren Sie auf das CDN Versionen</span><span class="sxs-lookup"><span data-stu-id="7b5fe-608">Respond Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-609">Die folgenden Versionen von [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") reagieren auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-609">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="7b5fe-610">Version 1.4.2 reagieren</span><span class="sxs-lookup"><span data-stu-id="7b5fe-610">Respond version 1.4.2</span></span>

- <span data-ttu-id="7b5fe-611">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-611">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="7b5fe-612">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-612">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="7b5fe-613">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.AddListener.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-613">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="7b5fe-614">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.AddListener.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-614">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="7b5fe-615">Version 1.4.1 reagieren</span><span class="sxs-lookup"><span data-stu-id="7b5fe-615">Respond version 1.4.1</span></span>

- <span data-ttu-id="7b5fe-616">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-616">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="7b5fe-617">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-617">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="7b5fe-618">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.AddListener.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="7b5fe-619">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.AddListener.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="7b5fe-620">Version 1.4.0 reagieren</span><span class="sxs-lookup"><span data-stu-id="7b5fe-620">Respond version 1.4.0</span></span>

- <span data-ttu-id="7b5fe-621">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="7b5fe-622">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-622">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="7b5fe-623">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.AddListener.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="7b5fe-624">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.AddListener.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="7b5fe-625">Version 1.3.0 reagieren</span><span class="sxs-lookup"><span data-stu-id="7b5fe-625">Respond version 1.3.0</span></span>

- <span data-ttu-id="7b5fe-626">http://AJAX.aspnetcdn.com/AJAX/Respond/1.3.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-626">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="7b5fe-627">Die Version 1.2.0 reagieren</span><span class="sxs-lookup"><span data-stu-id="7b5fe-627">Respond version 1.2.0</span></span>

- <span data-ttu-id="7b5fe-628">http://AJAX.aspnetcdn.com/AJAX/Respond/1.2.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-628">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-629">Bootstrap-Versionen auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-629">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-630">Die folgenden Versionen von [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") Bootstrap auf dem CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-630">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="7b5fe-631">Bootstrap Version 3.3.7</span><span class="sxs-lookup"><span data-stu-id="7b5fe-631">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="7b5fe-632">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-632">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-633">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-633">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-634">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-634">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-635">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-635">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="7b5fe-636">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-636">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-637">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-637">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="7b5fe-638">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-638">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="7b5fe-639">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="7b5fe-640">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="7b5fe-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="7b5fe-641">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="7b5fe-642">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="7b5fe-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="7b5fe-643">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="7b5fe-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="7b5fe-644">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="7b5fe-645">Bootstrap Version 3.3.6</span><span class="sxs-lookup"><span data-stu-id="7b5fe-645">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="7b5fe-646">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-647">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-648">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-649">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="7b5fe-650">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-651">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="7b5fe-652">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-652">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="7b5fe-653">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="7b5fe-654">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="7b5fe-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="7b5fe-655">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="7b5fe-656">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="7b5fe-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="7b5fe-657">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="7b5fe-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="7b5fe-658">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="7b5fe-659">Bootstrap Version 3.3.5</span><span class="sxs-lookup"><span data-stu-id="7b5fe-659">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="7b5fe-660">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-661">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-662">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-663">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="7b5fe-664">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-665">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="7b5fe-666">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-666">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="7b5fe-667">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="7b5fe-668">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="7b5fe-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="7b5fe-669">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="7b5fe-670">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="7b5fe-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="7b5fe-671">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="7b5fe-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="7b5fe-672">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="7b5fe-673">Bootstrap Version 3.3.4</span><span class="sxs-lookup"><span data-stu-id="7b5fe-673">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="7b5fe-674">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-675">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-676">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-677">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="7b5fe-678">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-679">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="7b5fe-680">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-680">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="7b5fe-681">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="7b5fe-682">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="7b5fe-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="7b5fe-683">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="7b5fe-684">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="7b5fe-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="7b5fe-685">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="7b5fe-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="7b5fe-686">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="7b5fe-687">Bootstrap Version 3.3.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-687">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="7b5fe-688">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-689">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-690">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-691">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="7b5fe-692">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-693">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="7b5fe-694">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-694">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="7b5fe-695">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="7b5fe-696">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="7b5fe-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="7b5fe-697">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="7b5fe-698">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="7b5fe-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="7b5fe-699">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="7b5fe-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="7b5fe-700">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="7b5fe-701">Bootstrap Version 3.3.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-701">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="7b5fe-702">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-703">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-704">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-705">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="7b5fe-706">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-707">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="7b5fe-708">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-708">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="7b5fe-709">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="7b5fe-710">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="7b5fe-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="7b5fe-711">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="7b5fe-712">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="7b5fe-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="7b5fe-713">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="7b5fe-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="7b5fe-714">Bootstrap Version 3.3.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-714">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="7b5fe-715">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-716">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-717">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-718">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="7b5fe-719">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-720">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="7b5fe-721">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-721">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="7b5fe-722">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="7b5fe-723">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="7b5fe-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="7b5fe-724">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="7b5fe-725">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="7b5fe-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="7b5fe-726">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="7b5fe-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="7b5fe-727">Bootstrap Version 3.2.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-727">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="7b5fe-728">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-729">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-730">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-731">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="7b5fe-732">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-733">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="7b5fe-734">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-734">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="7b5fe-735">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="7b5fe-736">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="7b5fe-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="7b5fe-737">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="7b5fe-738">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="7b5fe-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="7b5fe-739">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="7b5fe-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="7b5fe-740">Bootstrap Version 3.1.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-740">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="7b5fe-741">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-742">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-743">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-744">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="7b5fe-745">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-746">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="7b5fe-747">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-747">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="7b5fe-748">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="7b5fe-749">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="7b5fe-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="7b5fe-750">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="7b5fe-751">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="7b5fe-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="7b5fe-752">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="7b5fe-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="7b5fe-753">Bootstrap Version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-753">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="7b5fe-754">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-755">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-756">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-757">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="7b5fe-758">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-759">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="7b5fe-760">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-760">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="7b5fe-761">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="7b5fe-762">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="7b5fe-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="7b5fe-763">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="7b5fe-764">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="7b5fe-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="7b5fe-765">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="7b5fe-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="7b5fe-766">Bootstrap Version 3.0.3.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-766">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="7b5fe-767">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-768">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-769">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-770">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-771">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="7b5fe-772">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="7b5fe-773">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="7b5fe-773">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="7b5fe-774">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="7b5fe-775">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="7b5fe-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="7b5fe-776">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="7b5fe-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="7b5fe-777">Bootstrap Version 3.0.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-777">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="7b5fe-778">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-779">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-780">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-781">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-782">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="7b5fe-783">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="7b5fe-784">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="7b5fe-784">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="7b5fe-785">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="7b5fe-786">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="7b5fe-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="7b5fe-787">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="7b5fe-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="7b5fe-788">Bootstrap Version 3.0.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-788">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="7b5fe-789">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-790">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-791">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-792">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-793">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="7b5fe-794">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="7b5fe-795">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="7b5fe-795">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="7b5fe-796">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="7b5fe-797">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="7b5fe-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="7b5fe-798">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="7b5fe-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="7b5fe-799">Bootstrap-Version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-799">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="7b5fe-800">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-801">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-802">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-803">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-804">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="7b5fe-805">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="7b5fe-806">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="7b5fe-806">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="7b5fe-807">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="7b5fe-808">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="7b5fe-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="7b5fe-809">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="7b5fe-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="7b5fe-810">Bootstrap-Version 2.3.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-810">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="7b5fe-811">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-811">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-812">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-812">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-813">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-813">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-814">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-814">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-815">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-Responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-815">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="7b5fe-816">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-Responsive.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-816">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="7b5fe-817">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-817">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="7b5fe-818">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings-White.PNG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="7b5fe-819">Bootstrap Version 2.3.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-819">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="7b5fe-820">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="7b5fe-821">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="7b5fe-822">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="7b5fe-823">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="7b5fe-824">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-Responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="7b5fe-825">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-Responsive.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="7b5fe-826">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-826">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="7b5fe-827">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings-White.PNG</span><span class="sxs-lookup"><span data-stu-id="7b5fe-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-828">Bootstrap TouchCarousel Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-828">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-829">Die folgenden Versionen von [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel Versionen auf das CDN gehostet werden :</span><span class="sxs-lookup"><span data-stu-id="7b5fe-829">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="7b5fe-830">Bootstrap TouchCarousel Version 0.8.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-830">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="7b5fe-831">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-Carousel/0.8.0/CSS/Bootstrap-Touch-Carousel.CSS</span><span class="sxs-lookup"><span data-stu-id="7b5fe-831">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="7b5fe-832">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-Carousel/0.8.0/js/Bootstrap-Touch-Carousel.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-832">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-833">Hammer.js Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-833">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-834">Die folgenden Versionen von [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js Versionen auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-834">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="7b5fe-835">Hammer.js Version 2.0.4</span><span class="sxs-lookup"><span data-stu-id="7b5fe-835">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="7b5fe-836">http://AJAX.aspnetcdn.com/AJAX/Hammer.js/2.0.4/Hammer.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-836">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="7b5fe-837">http://AJAX.aspnetcdn.com/AJAX/Hammer.js/2.0.4/Hammer.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-837">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="7b5fe-838">http://AJAX.aspnetcdn.com/AJAX/Hammer.js/2.0.4/Hammer.Min.Map</span><span class="sxs-lookup"><span data-stu-id="7b5fe-838">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-839">ASP.NET Web Forms und Ajax-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-839">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-840">Die folgenden Versionen von ASP.NET Ajax-Bibliothek, die auf das CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-840">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="7b5fe-841">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="7b5fe-841">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="7b5fe-842">ASP.NET Web Forms- und Ajax-Version 4.5.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-842">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "ASP.NET Web Forms- und Ajax 4.5.2")
- [<span data-ttu-id="7b5fe-843">ASP.NET Web Forms- und Ajax-Version 4</span><span class="sxs-lookup"><span data-stu-id="7b5fe-843">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "ASP.NET Web Forms- und Ajax 4")
- [<span data-ttu-id="7b5fe-844">ASP.NET Ajax Version 3.5</span><span class="sxs-lookup"><span data-stu-id="7b5fe-844">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-845">ASP.NET MVC frei, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-845">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-846">Die folgenden ASP.NET MVC-JavaScript-Dateien, die auf diesem CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-846">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="7b5fe-847">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="7b5fe-847">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="7b5fe-848">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-848">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="7b5fe-849">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-849">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="7b5fe-850">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-850">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="7b5fe-851">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-851">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="7b5fe-852">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-852">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="7b5fe-853">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-853">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="7b5fe-854">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-854">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="7b5fe-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-855">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="7b5fe-856">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-856">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="7b5fe-857">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-857">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="7b5fe-858">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-858">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="7b5fe-859">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-859">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="7b5fe-860">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-860">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="7b5fe-861">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-861">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="7b5fe-862">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-862">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="7b5fe-863">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-863">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="7b5fe-864">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-864">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="7b5fe-865">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-865">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="7b5fe-866">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-866">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="7b5fe-867">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-867">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="7b5fe-868">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-868">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="7b5fe-869">ASP.NET MVC 1,0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-869">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="7b5fe-870">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-870">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="7b5fe-871">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-871">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="7b5fe-872">ASP.NET SignalR frei, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="7b5fe-872">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="7b5fe-873">Die folgenden ASP.NET SignalR JavaScript-Dateien, die auf diesem CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="7b5fe-873">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="7b5fe-874">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-874">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="7b5fe-875">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-875">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="7b5fe-876">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-876">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="7b5fe-877">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-877">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="7b5fe-878">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-878">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="7b5fe-879">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-879">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="7b5fe-880">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-880">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="7b5fe-881">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-881">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="7b5fe-882">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="7b5fe-883">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-883">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="7b5fe-884">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-884">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="7b5fe-885">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="7b5fe-886">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="7b5fe-886">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="7b5fe-887">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-887">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="7b5fe-888">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="7b5fe-889">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-889">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="7b5fe-890">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-890">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="7b5fe-891">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="7b5fe-892">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-892">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="7b5fe-893">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-893">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="7b5fe-894">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="7b5fe-895">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-895">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="7b5fe-896">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-896">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="7b5fe-897">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="7b5fe-898">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="7b5fe-898">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="7b5fe-899">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-899">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="7b5fe-900">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="7b5fe-901">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="7b5fe-901">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="7b5fe-902">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-902">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="7b5fe-903">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="7b5fe-904">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-904">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="7b5fe-905">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-905">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="7b5fe-906">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="7b5fe-907">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="7b5fe-907">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="7b5fe-908">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-908">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="7b5fe-909">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="7b5fe-910">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="7b5fe-910">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="7b5fe-911">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-911">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="7b5fe-912">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="7b5fe-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="7b5fe-913">Weitere Informationen zu den Nutzungsbedingungen für das CDN, finden Sie unter [Microsoft Ajax-CDN Nutzungsbedingungen](https://www.asp.net/terms-of-use "Microsoft Ajax-CDN Nutzungsbedingungen").</span><span class="sxs-lookup"><span data-stu-id="7b5fe-913">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
