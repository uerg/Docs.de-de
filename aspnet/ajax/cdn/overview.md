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
ms.openlocfilehash: f69f707ba64d13fc372b7bc44718c9dcf8cec6e2
ms.sourcegitcommit: 3f491f887074310fc0f145cd01a670aa63b969e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/22/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="b27eb-102">Microsoft Ajax Content Delivery Network</span><span class="sxs-lookup"><span data-stu-id="b27eb-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="b27eb-103">Hinweis: Das Microsoft Ajax-CDN kann keine SLA-Größe mit einer Azure-CDN.</span><span class="sxs-lookup"><span data-stu-id="b27eb-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="b27eb-104">Inhaltsverzeichnis</span><span class="sxs-lookup"><span data-stu-id="b27eb-104">Table of Contents</span></span>

<span data-ttu-id="b27eb-105">**[AJAX.Microsoft.com in ajax.aspnetcdn.com umbenannt](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="b27eb-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="b27eb-106">**[Unterstützung von Visual Studio .vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="b27eb-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="b27eb-107">**[Mithilfe von ASP.NET Ajax vom CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="b27eb-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="b27eb-108">**[Unter Verwendung von jQuery vom CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="b27eb-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="b27eb-109">**[Unter Verwendung von jQuery UI vom CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="b27eb-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="b27eb-110">**[Drittanbieter-Dateien auf dem CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="b27eb-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="b27eb-111">jQuery-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="b27eb-112">Migrieren von jQuery-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="b27eb-113">jQuery UI-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="b27eb-114">jQuery-Validierung Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="b27eb-115">jQuery Mobile-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="b27eb-116">jQuery-Vorlagen-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="b27eb-117">jQuery-Zyklus Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="b27eb-118">jQuery DataTables Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="b27eb-119">Modernizr Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="b27eb-120">JSHint Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="b27eb-121">Knockout Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="b27eb-122">Bei der Globalisierung Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="b27eb-123">Reagieren Sie auf das CDN Versionen</span><span class="sxs-lookup"><span data-stu-id="b27eb-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="b27eb-124">Bootstrap-Versionen auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="b27eb-125">Bootstrap TouchCarousel Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="b27eb-126">Hammer.js Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="b27eb-127">ASP.NET Web Forms und Ajax-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="b27eb-128">ASP.NET MVC frei, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="b27eb-129">ASP.NET SignalR frei, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="b27eb-130">Der Microsoft Ajax Content Delivery Network (CDN) hostet beliebten Drittanbieter-JavaScript-Bibliotheken wie z. B. jQuery und ermöglicht es Ihnen, einfach die Webanwendungen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b27eb-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="b27eb-131">Sie können z. B. starten, unter Verwendung von jQuery der gehostet wird auf diesem CDN einfach durch Hinzufügen einer &lt;Skript&gt; -Tag, um die Seite, die auf ajax.aspnetcdn.com verweist.</span><span class="sxs-lookup"><span data-stu-id="b27eb-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="b27eb-132">Durch das CDN nutzen, können Sie die Leistung der Ajax-Anwendungen erheblich verbessern.</span><span class="sxs-lookup"><span data-stu-id="b27eb-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="b27eb-133">Auf Servern, die auf der ganzen Welt werden der Inhalt des CDN zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="b27eb-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="b27eb-134">Darüber hinaus ermöglicht das CDN Browsern zum Wiederverwenden von zwischengespeicherten Drittanbieter-JavaScript-Dateien für Websites, die in unterschiedlichen Domänen befinden.</span><span class="sxs-lookup"><span data-stu-id="b27eb-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="b27eb-135">Das CDN unterstützt SSL (HTTPS) an, für den Fall, dass Sie einer Webseite mithilfe des Secure Sockets Layers bereitstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="b27eb-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="b27eb-136">Das CDN hostet die folgenden Drittanbieter-Skriptbibliotheken hochgeladen worden sein, und für Sie lizenziert sind, die von den Besitzern von diesen Bibliotheken:</span><span class="sxs-lookup"><span data-stu-id="b27eb-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="b27eb-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="b27eb-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="b27eb-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="b27eb-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="b27eb-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="b27eb-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="b27eb-140">jQuery-Validierung (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="b27eb-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="b27eb-141">jQuery-Zyklus (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="b27eb-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="b27eb-142">jQuery DataTables (http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="b27eb-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="b27eb-143">Das Microsoft Ajax-CDN umfasst außerdem die folgenden Bibliotheken, die von Microsoft hochgeladen wurden:</span><span class="sxs-lookup"><span data-stu-id="b27eb-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="b27eb-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="b27eb-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="b27eb-145">ASP.NET MVC-JavaScript-Dateien</span><span class="sxs-lookup"><span data-stu-id="b27eb-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="b27eb-146">ASP.NET SignalR JavaScript-Dateien</span><span class="sxs-lookup"><span data-stu-id="b27eb-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="b27eb-147">Microsoft erhebt keine Ansprüche für den Besitz von Drittanbieter-Bibliotheken, die auf diesem CDN gehostet.</span><span class="sxs-lookup"><span data-stu-id="b27eb-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="b27eb-148">Diese Bibliotheken sind den Urheberrechtsinhabern der Bibliotheken entschieden haben.</span><span class="sxs-lookup"><span data-stu-id="b27eb-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="b27eb-149">Alle Rechte, die Sie möglicherweise herunterladen und verwenden diese Bibliotheken werden ausschließlich von der jeweiligen Urheberrechtsinhabern gewährt.</span><span class="sxs-lookup"><span data-stu-id="b27eb-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="b27eb-150">Da keine Microsoft-Bibliotheken sind, bietet Microsoft keine GEWÄHRLEISTUNGEN oder geistiges Eigentum Rechte Lizenzen (einschließlich keine implizite Patentrechte), für die Drittanbieter-Bibliotheken, die auf diesem CDN gehostet.</span><span class="sxs-lookup"><span data-stu-id="b27eb-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="b27eb-151">Wenn Sie JavaScript-Bibliothek senden möchten, und die Bibliothek eine der oben JavaScript-Bibliotheken ist (wie auf http://trends.builtwith.com aufgeführt) oder Erweiterungen/Plugins, um diese Bibliotheken, die (a) beliebten; oder (b) hilfreich für auf ASP.NET verwenden, und wenden Sie sich an AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="b27eb-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="b27eb-152">AJAX.Microsoft.com in ajax.aspnetcdn.com umbenannt</span><span class="sxs-lookup"><span data-stu-id="b27eb-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="b27eb-153">Das CDN verwendet, um den Domänennamen "Microsoft.com" verwenden und wurde geändert, um den Domänennamen aspnetcdn.com verwenden.</span><span class="sxs-lookup"><span data-stu-id="b27eb-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="b27eb-154">Diese Änderung wurde vorgenommen, um die Leistung zu erhöhen, da bei ein Browser mit die Domäne "Microsoft.com" verwiesen wird er alle Cookies aus dieser Domäne über das Netz mit jeder Anforderung sendet.</span><span class="sxs-lookup"><span data-stu-id="b27eb-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="b27eb-155">Durch das Umbenennen eines Domänennamens als "Microsoft.com" kann die Leistung von möglichst alle Aufgaben auf 25 % erhöht werden.</span><span class="sxs-lookup"><span data-stu-id="b27eb-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="b27eb-156">Beachten Sie ajax.microsoft.com wird zwar weiterhin ajax.aspnetcdn.com wird jedoch empfohlen.</span><span class="sxs-lookup"><span data-stu-id="b27eb-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="b27eb-157">Altes Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="b27eb-158">Neues Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="b27eb-159">Unterstützung von Visual Studio .vsdoc</span><span class="sxs-lookup"><span data-stu-id="b27eb-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="b27eb-160">Die .vsdoc Dateien ordnungsgemäß zu mit Visual Studio 2008 verwenden, müssen Sie sicherstellen, dass Sie Visual Studio 2008 SP1 verfügen, und der Hotfix für das Vsdoc-Dateien installiert.</span><span class="sxs-lookup"><span data-stu-id="b27eb-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="b27eb-161">Sie können diese hier abrufen:</span><span class="sxs-lookup"><span data-stu-id="b27eb-161">You can get these from here:</span></span>

- [<span data-ttu-id="b27eb-162">Herunterladen der Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="b27eb-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Herunterladen der Visual Studio 2008 SP1")
- [<span data-ttu-id="b27eb-163">Herunterladen von .vsdoc Hotfix für Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="b27eb-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 ".vsdoc Hotfix für Visual Studio 2008 SP1 herunterladen")

<span data-ttu-id="b27eb-164">Visual Studio 2010 unterstützt .vsdoc Dateien ohne zusätzliche Patches.</span><span class="sxs-lookup"><span data-stu-id="b27eb-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="b27eb-165">Mithilfe von ASP.NET Ajax vom CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="b27eb-166">Wenn ASP.NET 4 zu verwenden, können Sie alle Anforderungen für ASP.NET Frameworkskripts auf das CDN umleiten.</span><span class="sxs-lookup"><span data-stu-id="b27eb-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="b27eb-167">Abrufen von Skripts aus dem CDN anstelle von Ihren lokalen Webserver kann die Leistung von öffentlichen ASP.NET-Websites erheblich verbessern.</span><span class="sxs-lookup"><span data-stu-id="b27eb-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="b27eb-168">Verwenden Sie die ScriptManager EnableCDN-Eigenschaft, um alle ASP.NET Framework Skript Anforderungen an das Microsoft Ajax-CDN umleiten:</span><span class="sxs-lookup"><span data-stu-id="b27eb-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="b27eb-169">Unter Verwendung von jQuery vom CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="b27eb-170">Sie können die jQuery-Skripts, das durch Hinzufügen der folgenden Script-Element zu einer Seite in der Webanwendung auf CDN gehostet:</span><span class="sxs-lookup"><span data-stu-id="b27eb-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="b27eb-171">Das CDN enthält auch die verkleinerte Version des jQuery-Skripts, die Sie abrufen können mithilfe des folgenden Elements:</span><span class="sxs-lookup"><span data-stu-id="b27eb-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="b27eb-172">Damit wird die Seite fallback auf jQuery aus einem lokalen Pfad auf eine eigene Website laden, wenn das CDN nicht verfügbar ist, erfolgt, fügen Sie das folgende Element sofort nach dem Element, das Verweisen auf das CDN hinzu:</span><span class="sxs-lookup"><span data-stu-id="b27eb-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="b27eb-173">Die folgenden Beispielseite verwendet die CDN-Version der jQuery-Bibliothek (mit Fallback auf eine lokale Kopie), zeigen Sie den Inhalt von einem Div-Element, wenn auf eine Schaltfläche geklickt wird.</span><span class="sxs-lookup"><span data-stu-id="b27eb-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="b27eb-174">Sie können erfahren Sie mehr über jQuery und eine lokale Kopie des jQuery herunterzuladen, besuchen die [jQuery](http://jquery.com/) Website.</span><span class="sxs-lookup"><span data-stu-id="b27eb-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="b27eb-175">Unter Verwendung von jQuery UI vom CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="b27eb-176">Das CDN hostet auch die jQuery UI-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="b27eb-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="b27eb-177">Die jQuery UI-Bibliothek enthält einen umfangreichen Satz von Widgets und Effekte, die Sie in Ihrer ASP.NET-Anwendungen verwenden können.</span><span class="sxs-lookup"><span data-stu-id="b27eb-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="b27eb-178">Die folgende Seite wird beispielsweise veranschaulicht, wie Sie dem jQuery UI Datepicker im Kontext einer ASP.NET Web Forms-Anwendung verwenden können, um ein Popupkalender angezeigt:</span><span class="sxs-lookup"><span data-stu-id="b27eb-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="b27eb-179">Wenn Sie den Fokus auf das Textfeld mit der Tastatur zu verschieben, wird ein Kalender angezeigt:</span><span class="sxs-lookup"><span data-stu-id="b27eb-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Erstellt mit Datepicker-Popupkalenders](overview/_static/image1.png)

<span data-ttu-id="b27eb-181">Beachten Sie, dass im obigen Code drei Dateien aus dem CDN aufnehmen müssen:</span><span class="sxs-lookup"><span data-stu-id="b27eb-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="b27eb-182">Die jQuery-Bibliothek &mdash; die jQuery UI-Bibliothek für die jQuery-Bibliothek abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="b27eb-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="b27eb-183">Sie müssen die jQuery-Bibliothek auf der Seite vor dem Hinzufügen der jQuery UI-Bibliothek hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b27eb-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="b27eb-184">Der jQuery UI-Bibliothek &mdash; die jQuery UI-Bibliothek enthält alle Benutzeroberflächeneffekte jQuery und Widgets, z. B. das Datepicker-Widget in der oben genannten Seite verwendet.</span><span class="sxs-lookup"><span data-stu-id="b27eb-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="b27eb-185">Ein Design der jQuery UI &mdash; jQuery UI unterstützt verschiedene Designs.</span><span class="sxs-lookup"><span data-stu-id="b27eb-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="b27eb-186">Die Seite enthält einen Link in einer CSS-Datei, die das Design "Redmond" zu importieren.</span><span class="sxs-lookup"><span data-stu-id="b27eb-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="b27eb-187">Alle von der standardmäßigen jQuery UI-Designs werden auf das CDN gehostet.</span><span class="sxs-lookup"><span data-stu-id="b27eb-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="b27eb-188">[Besuchen Sie diese Seite](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 auf das Microsoft Ajax-CDN") Miniaturansichten für jeden Design anzeigen.</span><span class="sxs-lookup"><span data-stu-id="b27eb-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="b27eb-189">Weitere Informationen zu der jQuery UI-Bibliothek finden Sie auf der offiziellen [jQuery UI Website](http://jQueryUI.com "jQuery UI Website").</span><span class="sxs-lookup"><span data-stu-id="b27eb-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="b27eb-190">Drittanbieter-Dateien auf dem CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="b27eb-191">Das CDN hostet einiger der beliebtesten JavaScript-Bibliotheken von Drittanbietern.</span><span class="sxs-lookup"><span data-stu-id="b27eb-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="b27eb-192">Microsoft erhebt keine Ansprüche für den Besitz von Drittanbieter-Bibliotheken, die auf diesem CDN gehostet.</span><span class="sxs-lookup"><span data-stu-id="b27eb-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="b27eb-193">Diese Bibliotheken sind den Urheberrechtsinhabern der Bibliotheken entschieden haben.</span><span class="sxs-lookup"><span data-stu-id="b27eb-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="b27eb-194">Alle Rechte, die Sie möglicherweise herunterladen und verwenden diese Bibliotheken werden ausschließlich von der jeweiligen Urheberrechtsinhabern gewährt.</span><span class="sxs-lookup"><span data-stu-id="b27eb-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="b27eb-195">Da keine Microsoft-Bibliotheken sind, bietet Microsoft keine GEWÄHRLEISTUNGEN oder geistiges Eigentum Rechte Lizenzen (einschließlich keine implizite Patentrechte), für die Drittanbieter-Bibliotheken, die auf diesem CDN gehostet.</span><span class="sxs-lookup"><span data-stu-id="b27eb-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="b27eb-196">jQuery-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="b27eb-197">Die folgenden Versionen von jQuery, die auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="b27eb-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="b27eb-198">jQuery-Version 3.3.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-198">jQuery version 3.3.1</span></span>
- <span data-ttu-id="b27eb-199">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-199">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js</span></span>
- <span data-ttu-id="b27eb-200">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-200">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js</span></span>
- <span data-ttu-id="b27eb-201">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-201">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map</span></span>
- <span data-ttu-id="b27eb-202">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-202">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js</span></span>
- <span data-ttu-id="b27eb-203">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-203">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js</span></span>
- <span data-ttu-id="b27eb-204">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.3.1.Slim.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-204">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map</span></span>

#### <a name="jquery-version-321"></a><span data-ttu-id="b27eb-205">jQuery-Version 3.2.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-205">jQuery version 3.2.1</span></span>
- <span data-ttu-id="b27eb-206">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-206">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js</span></span>
- <span data-ttu-id="b27eb-207">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-207">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js</span></span>
- <span data-ttu-id="b27eb-208">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-208">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map</span></span>
- <span data-ttu-id="b27eb-209">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-209">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js</span></span>
- <span data-ttu-id="b27eb-210">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-210">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js</span></span>
- <span data-ttu-id="b27eb-211">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.1.Slim.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-211">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map</span></span>

#### <a name="jquery-version-320"></a><span data-ttu-id="b27eb-212">jQuery-Version 3.2.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-212">jQuery version 3.2.0</span></span>

- <span data-ttu-id="b27eb-213">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-213">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js</span></span>
- <span data-ttu-id="b27eb-214">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-214">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js</span></span>
- <span data-ttu-id="b27eb-215">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-215">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map</span></span>
- <span data-ttu-id="b27eb-216">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-216">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js</span></span>
- <span data-ttu-id="b27eb-217">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-217">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js</span></span>
- <span data-ttu-id="b27eb-218">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.2.0.Slim.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-218">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map</span></span>

#### <a name="jquery-version-311"></a><span data-ttu-id="b27eb-219">jQuery-Version 3.1.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-219">jQuery version 3.1.1</span></span>

- <span data-ttu-id="b27eb-220">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-220">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js</span></span>
- <span data-ttu-id="b27eb-221">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-221">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js</span></span>
- <span data-ttu-id="b27eb-222">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-222">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map</span></span>
- <span data-ttu-id="b27eb-223">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-223">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js</span></span>
- <span data-ttu-id="b27eb-224">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-224">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js</span></span>
- <span data-ttu-id="b27eb-225">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.1.Slim.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-225">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map</span></span>

#### <a name="jquery-version-310"></a><span data-ttu-id="b27eb-226">jQuery-Version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-226">jQuery version 3.1.0</span></span>

- <span data-ttu-id="b27eb-227">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-227">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js</span></span>
- <span data-ttu-id="b27eb-228">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-228">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js</span></span>
- <span data-ttu-id="b27eb-229">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-229">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map</span></span>
- <span data-ttu-id="b27eb-230">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-230">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js</span></span>
- <span data-ttu-id="b27eb-231">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-231">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js</span></span>
- <span data-ttu-id="b27eb-232">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.1.0.Slim.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-232">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map</span></span>

#### <a name="jquery-version-300"></a><span data-ttu-id="b27eb-233">jQuery-Version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-233">jQuery version 3.0.0</span></span>

- <span data-ttu-id="b27eb-234">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-234">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js</span></span>
- <span data-ttu-id="b27eb-235">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-235">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js</span></span>
- <span data-ttu-id="b27eb-236">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-236">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map</span></span>
- <span data-ttu-id="b27eb-237">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-237">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js</span></span>
- <span data-ttu-id="b27eb-238">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-238">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js</span></span>
- <span data-ttu-id="b27eb-239">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-3.0.0.Slim.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-239">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map</span></span>

#### <a name="jquery-version-224"></a><span data-ttu-id="b27eb-240">jQuery-Version 2.2.4</span><span class="sxs-lookup"><span data-stu-id="b27eb-240">jQuery version 2.2.4</span></span>

- <span data-ttu-id="b27eb-241">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-241">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js</span></span>
- <span data-ttu-id="b27eb-242">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-242">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js</span></span>
- <span data-ttu-id="b27eb-243">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.4.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-243">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map</span></span>

#### <a name="jquery-version-223"></a><span data-ttu-id="b27eb-244">jQuery-Version 2.2.3</span><span class="sxs-lookup"><span data-stu-id="b27eb-244">jQuery version 2.2.3</span></span>

- <span data-ttu-id="b27eb-245">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-245">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js</span></span>
- <span data-ttu-id="b27eb-246">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-246">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js</span></span>
- <span data-ttu-id="b27eb-247">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.3.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-247">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map</span></span>

#### <a name="jquery-version-222"></a><span data-ttu-id="b27eb-248">jQuery-Version 2.2.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-248">jQuery version 2.2.2</span></span>

- <span data-ttu-id="b27eb-249">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-249">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js</span></span>
- <span data-ttu-id="b27eb-250">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-250">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js</span></span>
- <span data-ttu-id="b27eb-251">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.2.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-251">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map</span></span>

#### <a name="jquery-version-221"></a><span data-ttu-id="b27eb-252">jQuery-Version 2.2.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-252">jQuery version 2.2.1</span></span>

- <span data-ttu-id="b27eb-253">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-253">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js</span></span>
- <span data-ttu-id="b27eb-254">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-254">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js</span></span>
- <span data-ttu-id="b27eb-255">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-255">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map</span></span>

#### <a name="jquery-version-220"></a><span data-ttu-id="b27eb-256">jQuery-Version 2.2.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-256">jQuery version 2.2.0</span></span>

- <span data-ttu-id="b27eb-257">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-257">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js</span></span>
- <span data-ttu-id="b27eb-258">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-258">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js</span></span>
- <span data-ttu-id="b27eb-259">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.2.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-259">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map</span></span>

#### <a name="jquery-version-214"></a><span data-ttu-id="b27eb-260">jQuery-Version 2.1.4</span><span class="sxs-lookup"><span data-stu-id="b27eb-260">jQuery version 2.1.4</span></span>

- <span data-ttu-id="b27eb-261">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-261">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js</span></span>
- <span data-ttu-id="b27eb-262">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-262">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js</span></span>
- <span data-ttu-id="b27eb-263">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.4.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-263">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map</span></span>

#### <a name="jquery-version-213"></a><span data-ttu-id="b27eb-264">jQuery-Version 2.1.3</span><span class="sxs-lookup"><span data-stu-id="b27eb-264">jQuery version 2.1.3</span></span>

- <span data-ttu-id="b27eb-265">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-265">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js</span></span>
- <span data-ttu-id="b27eb-266">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-266">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js</span></span>
- <span data-ttu-id="b27eb-267">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.3.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-267">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map</span></span>

#### <a name="jquery-version-212"></a><span data-ttu-id="b27eb-268">jQuery-Version 2.1.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-268">jQuery version 2.1.2</span></span>

- <span data-ttu-id="b27eb-269">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-269">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js</span></span>
- <span data-ttu-id="b27eb-270">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-270">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js</span></span>

#### <a name="jquery-version-211"></a><span data-ttu-id="b27eb-271">jQuery-Version 2.1.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-271">jQuery version 2.1.1</span></span>

- <span data-ttu-id="b27eb-272">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-272">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js</span></span>
- <span data-ttu-id="b27eb-273">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-273">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js</span></span>
- <span data-ttu-id="b27eb-274">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-274">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map</span></span>

#### <a name="jquery-version-210"></a><span data-ttu-id="b27eb-275">jQuery-Version 2.1.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-275">jQuery version 2.1.0</span></span>

- <span data-ttu-id="b27eb-276">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-276">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js</span></span>
- <span data-ttu-id="b27eb-277">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-277">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js</span></span>
- <span data-ttu-id="b27eb-278">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-278">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js</span></span>
- <span data-ttu-id="b27eb-279">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.1.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-279">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map</span></span>

#### <a name="jquery-version-203"></a><span data-ttu-id="b27eb-280">jQuery-Version 2.0.3</span><span class="sxs-lookup"><span data-stu-id="b27eb-280">jQuery version 2.0.3</span></span>

- <span data-ttu-id="b27eb-281">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-281">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js</span></span>
- <span data-ttu-id="b27eb-282">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-282">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js</span></span>
- <span data-ttu-id="b27eb-283">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-283">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js</span></span>
- <span data-ttu-id="b27eb-284">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.3.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-284">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map</span></span>

#### <a name="jquery-version-202"></a><span data-ttu-id="b27eb-285">jQuery-Version 2.0.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-285">jQuery version 2.0.2</span></span>

- <span data-ttu-id="b27eb-286">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-286">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js</span></span>
- <span data-ttu-id="b27eb-287">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-287">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js</span></span>
- <span data-ttu-id="b27eb-288">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-288">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js</span></span>
- <span data-ttu-id="b27eb-289">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.2.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-289">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map</span></span>

#### <a name="jquery-version-201"></a><span data-ttu-id="b27eb-290">jQuery-Version 2.0.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-290">jQuery version 2.0.1</span></span>

- <span data-ttu-id="b27eb-291">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-291">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js</span></span>
- <span data-ttu-id="b27eb-292">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-292">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js</span></span>
- <span data-ttu-id="b27eb-293">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-293">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js</span></span>
- <span data-ttu-id="b27eb-294">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-294">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map</span></span>

#### <a name="jquery-version-200"></a><span data-ttu-id="b27eb-295">jQuery-Version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-295">jQuery version 2.0.0</span></span>

- <span data-ttu-id="b27eb-296">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-296">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js</span></span>
- <span data-ttu-id="b27eb-297">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-297">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js</span></span>
- <span data-ttu-id="b27eb-298">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-298">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js</span></span>
- <span data-ttu-id="b27eb-299">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-2.0.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-299">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map</span></span>

#### <a name="jquery-version-1124"></a><span data-ttu-id="b27eb-300">jQuery-Version 1.12.4</span><span class="sxs-lookup"><span data-stu-id="b27eb-300">jQuery version 1.12.4</span></span>

- <span data-ttu-id="b27eb-301">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-301">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js</span></span>
- <span data-ttu-id="b27eb-302">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-302">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js</span></span>
- <span data-ttu-id="b27eb-303">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.4.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-303">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map</span></span>

#### <a name="jquery-version-1123"></a><span data-ttu-id="b27eb-304">jQuery-Version 1.12.3</span><span class="sxs-lookup"><span data-stu-id="b27eb-304">jQuery version 1.12.3</span></span>

- <span data-ttu-id="b27eb-305">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-305">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js</span></span>
- <span data-ttu-id="b27eb-306">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-306">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js</span></span>
- <span data-ttu-id="b27eb-307">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.3.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-307">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map</span></span>

#### <a name="jquery-version-1122"></a><span data-ttu-id="b27eb-308">jQuery-Version 1.12.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-308">jQuery version 1.12.2</span></span>

- <span data-ttu-id="b27eb-309">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-309">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js</span></span>
- <span data-ttu-id="b27eb-310">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-310">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js</span></span>
- <span data-ttu-id="b27eb-311">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.2.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-311">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map</span></span>

#### <a name="jquery-version-1121"></a><span data-ttu-id="b27eb-312">jQuery-Version 1.12.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-312">jQuery version 1.12.1</span></span>

- <span data-ttu-id="b27eb-313">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-313">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js</span></span>
- <span data-ttu-id="b27eb-314">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-314">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js</span></span>
- <span data-ttu-id="b27eb-315">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-315">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map</span></span>

#### <a name="jquery-version-1120"></a><span data-ttu-id="b27eb-316">jQuery-Version 1.12.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-316">jQuery version 1.12.0</span></span>

- <span data-ttu-id="b27eb-317">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-317">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js</span></span>
- <span data-ttu-id="b27eb-318">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-318">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js</span></span>
- <span data-ttu-id="b27eb-319">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.12.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-319">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map</span></span>

#### <a name="jquery-version-1113"></a><span data-ttu-id="b27eb-320">jQuery-Version 1.11.3</span><span class="sxs-lookup"><span data-stu-id="b27eb-320">jQuery version 1.11.3</span></span>

- <span data-ttu-id="b27eb-321">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-321">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js</span></span>
- <span data-ttu-id="b27eb-322">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-322">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js</span></span>
- <span data-ttu-id="b27eb-323">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.3.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-323">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map</span></span>

#### <a name="jquery-version-1112"></a><span data-ttu-id="b27eb-324">jQuery-Version 1.11.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-324">jQuery version 1.11.2</span></span>

- <span data-ttu-id="b27eb-325">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-325">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js</span></span>
- <span data-ttu-id="b27eb-326">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-326">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js</span></span>
- <span data-ttu-id="b27eb-327">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.2.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-327">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map</span></span>

#### <a name="jquery-version-1111"></a><span data-ttu-id="b27eb-328">jQuery-Version 1.11.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-328">jQuery version 1.11.1</span></span>

- <span data-ttu-id="b27eb-329">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-329">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js</span></span>
- <span data-ttu-id="b27eb-330">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-330">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js</span></span>
- <span data-ttu-id="b27eb-331">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-331">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map</span></span>

#### <a name="jquery-version-1110"></a><span data-ttu-id="b27eb-332">jQuery-Version 1.11.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-332">jQuery version 1.11.0</span></span>

- <span data-ttu-id="b27eb-333">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-333">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js</span></span>
- <span data-ttu-id="b27eb-334">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-334">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js</span></span>
- <span data-ttu-id="b27eb-335">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-335">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js</span></span>
- <span data-ttu-id="b27eb-336">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.11.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-336">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map</span></span>

#### <a name="jquery-version-1102"></a><span data-ttu-id="b27eb-337">Version der jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-337">jQuery version 1.10.2</span></span>

- <span data-ttu-id="b27eb-338">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-338">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js</span></span>
- <span data-ttu-id="b27eb-339">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-339">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js</span></span>
- <span data-ttu-id="b27eb-340">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-340">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js</span></span>
- <span data-ttu-id="b27eb-341">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.2.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-341">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map</span></span>

#### <a name="jquery-version-1101"></a><span data-ttu-id="b27eb-342">jQuery-Version 1.10.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-342">jQuery version 1.10.1</span></span>

- <span data-ttu-id="b27eb-343">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-343">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js</span></span>
- <span data-ttu-id="b27eb-344">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-344">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js</span></span>
- <span data-ttu-id="b27eb-345">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-345">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js</span></span>
- <span data-ttu-id="b27eb-346">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-346">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map</span></span>

#### <a name="jquery-version-1100"></a><span data-ttu-id="b27eb-347">jQuery-Version 1.10.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-347">jQuery version 1.10.0</span></span>

- <span data-ttu-id="b27eb-348">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-348">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js</span></span>
- <span data-ttu-id="b27eb-349">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-349">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js</span></span>
- <span data-ttu-id="b27eb-350">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-350">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js</span></span>
- <span data-ttu-id="b27eb-351">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.10.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-351">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map</span></span>

#### <a name="jquery-version-191"></a><span data-ttu-id="b27eb-352">jQuery-Version 1.9.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-352">jQuery version 1.9.1</span></span>

- <span data-ttu-id="b27eb-353">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-353">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js</span></span>
- <span data-ttu-id="b27eb-354">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-354">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js</span></span>
- <span data-ttu-id="b27eb-355">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-355">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js</span></span>
- <span data-ttu-id="b27eb-356">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.1.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-356">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map</span></span>

#### <a name="jquery-version-190"></a><span data-ttu-id="b27eb-357">jQuery-Version 1.9.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-357">jQuery version 1.9.0</span></span>

- <span data-ttu-id="b27eb-358">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-358">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js</span></span>
- <span data-ttu-id="b27eb-359">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-359">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js</span></span>
- <span data-ttu-id="b27eb-360">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-360">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js</span></span>
- <span data-ttu-id="b27eb-361">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.9.0.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-361">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map</span></span>

#### <a name="jquery-version-183"></a><span data-ttu-id="b27eb-362">jQuery-Version 1.8.3</span><span class="sxs-lookup"><span data-stu-id="b27eb-362">jQuery version 1.8.3</span></span>

- <span data-ttu-id="b27eb-363">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-363">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js</span></span>
- <span data-ttu-id="b27eb-364">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-364">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js</span></span>
- <span data-ttu-id="b27eb-365">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-365">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js</span></span>

#### <a name="jquery-version-182"></a><span data-ttu-id="b27eb-366">jQuery-Version 1.8.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-366">jQuery version 1.8.2</span></span>

- <span data-ttu-id="b27eb-367">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-367">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js</span></span>
- <span data-ttu-id="b27eb-368">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-368">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js</span></span>
- <span data-ttu-id="b27eb-369">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-369">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js</span></span>

#### <a name="jquery-version-181"></a><span data-ttu-id="b27eb-370">jQuery-Version 1.8.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-370">jQuery version 1.8.1</span></span>

- <span data-ttu-id="b27eb-371">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-371">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js</span></span>
- <span data-ttu-id="b27eb-372">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-372">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js</span></span>
- <span data-ttu-id="b27eb-373">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-373">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js</span></span>

#### <a name="jquery-version-180"></a><span data-ttu-id="b27eb-374">jQuery-Version 1.8.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-374">jQuery version 1.8.0</span></span>

- <span data-ttu-id="b27eb-375">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-375">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="b27eb-376">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-376">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js</span></span>
- <span data-ttu-id="b27eb-377">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.8.0-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-377">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js</span></span>

#### <a name="jquery-version-172"></a><span data-ttu-id="b27eb-378">jQuery-Version 1.7.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-378">jQuery version 1.7.2</span></span>

- <span data-ttu-id="b27eb-379">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-379">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js</span></span>
- <span data-ttu-id="b27eb-380">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-380">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js</span></span>

#### <a name="jquery-version-171"></a><span data-ttu-id="b27eb-381">jQuery-Version 1.7.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-381">jQuery version 1.7.1</span></span>

- <span data-ttu-id="b27eb-382">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-382">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js</span></span>
- <span data-ttu-id="b27eb-383">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-383">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js</span></span>
- <span data-ttu-id="b27eb-384">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-384">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js</span></span>

#### <a name="jquery-version-17"></a><span data-ttu-id="b27eb-385">jQuery-Version 1.7</span><span class="sxs-lookup"><span data-stu-id="b27eb-385">jQuery version 1.7</span></span>

- <span data-ttu-id="b27eb-386">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-386">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js</span></span>
- <span data-ttu-id="b27eb-387">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-387">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js</span></span>
- <span data-ttu-id="b27eb-388">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.7-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-388">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js</span></span>

#### <a name="jquery-version-164"></a><span data-ttu-id="b27eb-389">jQuery-Version 1.6.4</span><span class="sxs-lookup"><span data-stu-id="b27eb-389">jQuery version 1.6.4</span></span>

- <span data-ttu-id="b27eb-390">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-390">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js</span></span>
- <span data-ttu-id="b27eb-391">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-391">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js</span></span>
- <span data-ttu-id="b27eb-392">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-392">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js</span></span>

#### <a name="jquery-version-163"></a><span data-ttu-id="b27eb-393">jQuery-Version 1.6.3</span><span class="sxs-lookup"><span data-stu-id="b27eb-393">jQuery version 1.6.3</span></span>

- <span data-ttu-id="b27eb-394">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-394">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js</span></span>
- <span data-ttu-id="b27eb-395">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-395">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js</span></span>
- <span data-ttu-id="b27eb-396">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-396">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js</span></span>

#### <a name="jquery-version-162"></a><span data-ttu-id="b27eb-397">jQuery-Version 1.6.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-397">jQuery version 1.6.2</span></span>

- <span data-ttu-id="b27eb-398">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-398">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js</span></span>
- <span data-ttu-id="b27eb-399">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-399">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js</span></span>
- <span data-ttu-id="b27eb-400">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-400">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js</span></span>

#### <a name="jquery-version-161"></a><span data-ttu-id="b27eb-401">jQuery-Version 1.6.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-401">jQuery version 1.6.1</span></span>

- <span data-ttu-id="b27eb-402">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-402">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js</span></span>
- <span data-ttu-id="b27eb-403">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-403">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js</span></span>
- <span data-ttu-id="b27eb-404">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-404">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js</span></span>

#### <a name="jquery-version-16"></a><span data-ttu-id="b27eb-405">jQuery-Version 1.6</span><span class="sxs-lookup"><span data-stu-id="b27eb-405">jQuery version 1.6</span></span>

- <span data-ttu-id="b27eb-406">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-406">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js</span></span>
- <span data-ttu-id="b27eb-407">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-407">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js</span></span>
- <span data-ttu-id="b27eb-408">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.6-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-408">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js</span></span>

#### <a name="jquery-version-152"></a><span data-ttu-id="b27eb-409">jQuery-Version 1.5.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-409">jQuery version 1.5.2</span></span>

- <span data-ttu-id="b27eb-410">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-410">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js</span></span>
- <span data-ttu-id="b27eb-411">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-411">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js</span></span>
- <span data-ttu-id="b27eb-412">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-412">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js</span></span>

#### <a name="jquery-version-151"></a><span data-ttu-id="b27eb-413">jQuery-Version 1.5.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-413">jQuery version 1.5.1</span></span>

- <span data-ttu-id="b27eb-414">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-414">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js</span></span>
- <span data-ttu-id="b27eb-415">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-415">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js</span></span>
- <span data-ttu-id="b27eb-416">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-416">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js</span></span>

#### <a name="jquery-version-15"></a><span data-ttu-id="b27eb-417">jQuery-Version 1.5</span><span class="sxs-lookup"><span data-stu-id="b27eb-417">jQuery version 1.5</span></span>

- <span data-ttu-id="b27eb-418">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-418">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js</span></span>
- <span data-ttu-id="b27eb-419">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-419">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js</span></span>
- <span data-ttu-id="b27eb-420">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.5-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-420">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js</span></span>

#### <a name="jquery-version-144"></a><span data-ttu-id="b27eb-421">jQuery-Version 1.4.4</span><span class="sxs-lookup"><span data-stu-id="b27eb-421">jQuery version 1.4.4</span></span>

- <span data-ttu-id="b27eb-422">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-422">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js</span></span>
- <span data-ttu-id="b27eb-423">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-423">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js</span></span>
- <span data-ttu-id="b27eb-424">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.4-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-424">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js</span></span>

#### <a name="jquery-version-143"></a><span data-ttu-id="b27eb-425">jQuery-Version 1.4.3</span><span class="sxs-lookup"><span data-stu-id="b27eb-425">jQuery version 1.4.3</span></span>

- <span data-ttu-id="b27eb-426">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-426">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js</span></span>
- <span data-ttu-id="b27eb-427">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-427">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js</span></span>
- <span data-ttu-id="b27eb-428">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.3-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-428">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js</span></span>

#### <a name="jquery-version-142"></a><span data-ttu-id="b27eb-429">jQuery-Version 1.4.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-429">jQuery version 1.4.2</span></span>

- <span data-ttu-id="b27eb-430">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-430">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js</span></span>
- <span data-ttu-id="b27eb-431">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-431">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js</span></span>
- <span data-ttu-id="b27eb-432">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-432">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js</span></span>

#### <a name="jquery-version-141"></a><span data-ttu-id="b27eb-433">jQuery-Version 1.4.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-433">jQuery version 1.4.1</span></span>

- <span data-ttu-id="b27eb-434">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-434">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js</span></span>
- <span data-ttu-id="b27eb-435">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-435">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js</span></span>
- <span data-ttu-id="b27eb-436">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.1-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-436">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js</span></span>

#### <a name="jquery-version-14"></a><span data-ttu-id="b27eb-437">jQuery-Version 1.4</span><span class="sxs-lookup"><span data-stu-id="b27eb-437">jQuery version 1.4</span></span>

- <span data-ttu-id="b27eb-438">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-438">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js</span></span>
- <span data-ttu-id="b27eb-439">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.4.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-439">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js</span></span>

#### <a name="jquery-version-132"></a><span data-ttu-id="b27eb-440">Version der jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-440">jQuery version 1.3.2</span></span>

- <span data-ttu-id="b27eb-441">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-441">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js</span></span>
- <span data-ttu-id="b27eb-442">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-442">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js</span></span>
- <span data-ttu-id="b27eb-443">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-443">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js</span></span>
- <span data-ttu-id="b27eb-444">http://AJAX.aspnetcdn.com/AJAX/jQuery/jQuery-1.3.2.Min-vsdoc.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-444">http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js</span></span>

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="b27eb-445">Migrieren von jQuery-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-445">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="b27eb-446">Die folgenden Versionen von jQuery migrieren, die auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="b27eb-446">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="b27eb-447">jQuery migrieren Version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-447">jQuery Migrate version 3.0.0</span></span>

- <span data-ttu-id="b27eb-448">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-448">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js</span></span>
- <span data-ttu-id="b27eb-449">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-3.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-449">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js</span></span>

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="b27eb-450">jQuery migrieren Version 1.2.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-450">jQuery Migrate version 1.2.1</span></span>

- <span data-ttu-id="b27eb-451">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-451">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js</span></span>
- <span data-ttu-id="b27eb-452">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-452">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js</span></span>

<span data-ttu-id="b27eb-453">jQuery migrieren Version 1.2.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-453">jQuery Migrate version 1.2.0</span></span>

- <span data-ttu-id="b27eb-454">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-454">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js</span></span>
- <span data-ttu-id="b27eb-455">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-455">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js</span></span>

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="b27eb-456">jQuery migrieren Version 1.1.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-456">jQuery Migrate version 1.1.1</span></span>

- <span data-ttu-id="b27eb-457">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-457">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js</span></span>
- <span data-ttu-id="b27eb-458">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-458">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js</span></span>

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="b27eb-459">jQuery migrieren Version 1.1.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-459">jQuery Migrate version 1.1.0</span></span>

- <span data-ttu-id="b27eb-460">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-460">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js</span></span>
- <span data-ttu-id="b27eb-461">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-461">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js</span></span>

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="b27eb-462">jQuery migrieren Version 1.0.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-462">jQuery Migrate version 1.0.0</span></span>

- <span data-ttu-id="b27eb-463">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-463">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js</span></span>
- <span data-ttu-id="b27eb-464">http://AJAX.aspnetcdn.com/AJAX/jQuery.Migrate/jQuery-Migrate-1.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-464">http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js</span></span>

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="b27eb-465">jQuery UI-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-465">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="b27eb-466">Die folgenden Versionen der jQuery UI-Bibliothek, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="b27eb-466">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="b27eb-467">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b27eb-467">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="b27eb-468">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-468">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-469">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-469">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-470">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="b27eb-470">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-471">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="b27eb-471">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-472">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-472">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-473">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-473">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-474">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-474">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-475">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="b27eb-475">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-476">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="b27eb-476">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-477">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-477">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery 1.10.2-Benutzeroberfläche auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-478">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-478">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-479">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-479">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-480">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-480">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-481">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-481">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-482">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-482">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-483">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="b27eb-483">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-484">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="b27eb-484">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-485">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="b27eb-485">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-486">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="b27eb-486">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-487">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="b27eb-487">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-488">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="b27eb-488">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-489">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="b27eb-489">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-490">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="b27eb-490">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-491">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="b27eb-491">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-492">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="b27eb-492">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-493">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="b27eb-493">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-494">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="b27eb-494">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-495">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="b27eb-495">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-496">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="b27eb-496">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-497">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="b27eb-497">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-498">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="b27eb-498">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-499">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="b27eb-499">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-500">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="b27eb-500">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-501">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="b27eb-501">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-502">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="b27eb-502">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="b27eb-503">jQuery-Validierung Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-503">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="b27eb-504">Die folgenden Versionen der jQuery-Validierung-Bibliothek, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="b27eb-504">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="b27eb-505">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b27eb-505">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="b27eb-506">jQuery Validate 1.17.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-506">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery-Validierung 1.17.0")
- [<span data-ttu-id="b27eb-507">jQuery Validate 1.16.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-507">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery-Validierung 1.16.0")
- [<span data-ttu-id="b27eb-508">jQuery Validate 1.15.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-508">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery-Validierung 1.15.1")
- [<span data-ttu-id="b27eb-509">jQuery Validate 1.15.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-509">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery-Validierung 1.15.0")
- [<span data-ttu-id="b27eb-510">jQuery Validate 1.14.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-510">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery-Validierung 1.14.0")
- [<span data-ttu-id="b27eb-511">jQuery Validate 1.13.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-511">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery-Validierung 1.13.1")
- [<span data-ttu-id="b27eb-512">jQuery Validate 1.13.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-512">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery-Validierung 1.13.0")
- [<span data-ttu-id="b27eb-513">jQuery Validate 1.12.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-513">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery-Validierung 1.12.0")
- [<span data-ttu-id="b27eb-514">jQuery Validate 1.11.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-514">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery-Validierung 1.11.1")
- [<span data-ttu-id="b27eb-515">jQuery Validate 1.11.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-515">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery-Validierung 1.11.0")
- [<span data-ttu-id="b27eb-516">jQuery Validate 1.10.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-516">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery-Validierung 1.10.0")
- [<span data-ttu-id="b27eb-517">jQuery Validate 1.9</span><span class="sxs-lookup"><span data-stu-id="b27eb-517">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate Version 1.9")
- [<span data-ttu-id="b27eb-518">jQuery Validate 1.8.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-518">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate Version 1.8.1")
- [<span data-ttu-id="b27eb-519">jQuery Validate 1.8</span><span class="sxs-lookup"><span data-stu-id="b27eb-519">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate Version 1.8")
- [<span data-ttu-id="b27eb-520">jQuery Validate 1.7</span><span class="sxs-lookup"><span data-stu-id="b27eb-520">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate Version 1.7")
- [<span data-ttu-id="b27eb-521">jQuery Validate 1.6</span><span class="sxs-lookup"><span data-stu-id="b27eb-521">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery Validate 1.6")
- [<span data-ttu-id="b27eb-522">jQuery Validate 1.5.5</span><span class="sxs-lookup"><span data-stu-id="b27eb-522">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery Validate 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="b27eb-523">jQuery Mobile-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-523">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="b27eb-524">Die folgenden Versionen der jQuery Mobile-Bibliothek, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="b27eb-524">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="b27eb-525">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b27eb-525">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="b27eb-526">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="b27eb-526">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-527">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-527">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-528">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-528">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-529">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-529">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-530">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-530">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-531">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-531">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-532">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-532">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-533">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-533">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-534">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-534">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-535">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-535">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-536">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-536">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-537">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="b27eb-537">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-538">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-538">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-539">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-539">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-540">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="b27eb-540">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-541">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="b27eb-541">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="b27eb-542">jQuery Mobile 1.0 Beta 3</span><span class="sxs-lookup"><span data-stu-id="b27eb-542">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 Klicken Sie auf das Microsoft Ajax-CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="b27eb-543">jQuery-Vorlagen-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-543">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="b27eb-544">Die folgenden Versionen der jQuery-Vorlagen-Plug-Ins, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="b27eb-544">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="b27eb-545">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b27eb-545">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="b27eb-546">jQuery-Vorlagen Beta 1</span><span class="sxs-lookup"><span data-stu-id="b27eb-546">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery-Vorlagen Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="b27eb-547">jQuery-Zyklus Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-547">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="b27eb-548">Die folgenden Versionen der jQuery-Zyklus-Plug-Ins, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="b27eb-548">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="b27eb-549">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b27eb-549">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="b27eb-550">jQuery-Zyklus 2,99</span><span class="sxs-lookup"><span data-stu-id="b27eb-550">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery Zyklus 2,99")
- [<span data-ttu-id="b27eb-551">jQuery-Zyklus 2.94</span><span class="sxs-lookup"><span data-stu-id="b27eb-551">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Zyklus 2.94")
- [<span data-ttu-id="b27eb-552">jQuery-Zyklus 2,88</span><span class="sxs-lookup"><span data-stu-id="b27eb-552">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery Zyklus 2,88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="b27eb-553">jQuery DataTables Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-553">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="b27eb-554">Die folgenden Versionen der jQuery DataTables-Plug-Ins, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="b27eb-554">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="b27eb-555">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b27eb-555">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="b27eb-556">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="b27eb-556">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="b27eb-557">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="b27eb-557">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="b27eb-558">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="b27eb-558">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="b27eb-559">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="b27eb-559">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="b27eb-560">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-560">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="b27eb-561">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-561">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="b27eb-562">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-562">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="b27eb-563">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-563">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="b27eb-564">Modernizr Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-564">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="b27eb-565">Die folgenden Versionen von [Modernizr](http://www.modernizr.com "Modernizr") auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="b27eb-565">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- <span data-ttu-id="b27eb-566">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.8.3.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-566">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js</span></span>
- <span data-ttu-id="b27eb-567">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-567">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js</span></span>
- <span data-ttu-id="b27eb-568">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.7.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-568">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js</span></span>
- <span data-ttu-id="b27eb-569">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.6.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-569">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js</span></span>
- <span data-ttu-id="b27eb-570">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-1.7-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-570">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js</span></span>
- <span data-ttu-id="b27eb-571">http://AJAX.aspnetcdn.com/AJAX/Modernizr/Modernizr-2.0.6-Development-only.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-571">http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js</span></span>

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="b27eb-572">JSHint Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-572">JSHint Releases on the CDN</span></span>

<span data-ttu-id="b27eb-573">Die folgenden Versionen von [JSHint](http://www.jshint.com "JSHint") auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="b27eb-573">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- <span data-ttu-id="b27eb-574">http://AJAX.aspnetcdn.com/AJAX/jshint/r07/jshint.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-574">http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js</span></span>

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="b27eb-575">Knockout Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-575">Knockout Releases on the CDN</span></span>

<span data-ttu-id="b27eb-576">Die folgenden Versionen von [Knockout](http://www.knockoutjs.com "Knockout") auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="b27eb-576">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- <span data-ttu-id="b27eb-577">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-577">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js</span></span>
- <span data-ttu-id="b27eb-578">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-578">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js</span></span>
- <span data-ttu-id="b27eb-579">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-579">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js</span></span>
- <span data-ttu-id="b27eb-580">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-580">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js</span></span>
- <span data-ttu-id="b27eb-581">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-581">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js</span></span>
- <span data-ttu-id="b27eb-582">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-2.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-582">http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js</span></span>
- <span data-ttu-id="b27eb-583">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-583">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js</span></span>
- <span data-ttu-id="b27eb-584">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.0.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-584">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js</span></span>
- <span data-ttu-id="b27eb-585">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-585">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js</span></span>
- <span data-ttu-id="b27eb-586">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.1.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-586">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js</span></span>
- <span data-ttu-id="b27eb-587">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-587">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js</span></span>
- <span data-ttu-id="b27eb-588">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.2.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-588">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js</span></span>
- <span data-ttu-id="b27eb-589">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-589">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js</span></span>
- <span data-ttu-id="b27eb-590">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.3.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-590">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js</span></span>
- <span data-ttu-id="b27eb-591">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-591">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js</span></span>
- <span data-ttu-id="b27eb-592">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.0.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-592">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js</span></span>
- <span data-ttu-id="b27eb-593">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-593">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js</span></span>
- <span data-ttu-id="b27eb-594">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.1.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-594">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js</span></span>
- <span data-ttu-id="b27eb-595">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-595">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js</span></span>
- <span data-ttu-id="b27eb-596">http://AJAX.aspnetcdn.com/AJAX/Knockout/Knockout-3.4.2.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-596">http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js</span></span>

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="b27eb-597">Bei der Globalisierung Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-597">Globalize Releases on the CDN</span></span>

<span data-ttu-id="b27eb-598">Die folgenden Versionen von [Globalize](https://github.com/jquery/globalize "Globalize") auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="b27eb-598">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="b27eb-599">Bei der Version 1.0.0 Globalisierung</span><span class="sxs-lookup"><span data-stu-id="b27eb-599">Globalize version 1.0.0</span></span>

- <span data-ttu-id="b27eb-600">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-600">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js</span></span>
- <span data-ttu-id="b27eb-601">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/Node-Main.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-601">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js</span></span>
- <span data-ttu-id="b27eb-602">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Currency.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-602">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js</span></span>
- <span data-ttu-id="b27eb-603">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Date.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-603">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js</span></span>
- <span data-ttu-id="b27eb-604">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Message.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-604">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js</span></span>
- <span data-ttu-id="b27eb-605">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Number.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-605">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js</span></span>
- <span data-ttu-id="b27eb-606">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/Plural.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-606">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js</span></span>
- <span data-ttu-id="b27eb-607">http://AJAX.aspnetcdn.com/AJAX/globalize/1.0.0/globalize/relative-Time.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-607">http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js</span></span>

#### <a name="globalize-version-011"></a><span data-ttu-id="b27eb-608">Bei der Globalisierung Version 0.1.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-608">Globalize version 0.1.1</span></span>

- <span data-ttu-id="b27eb-609">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-609">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js</span></span>
- <span data-ttu-id="b27eb-610">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/globalize.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-610">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js</span></span>
- <span data-ttu-id="b27eb-611">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/Cultures/globalize.Cultures.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-611">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js</span></span>

    - <span data-ttu-id="b27eb-612">alle Kulturen</span><span class="sxs-lookup"><span data-stu-id="b27eb-612">all cultures</span></span>
- <span data-ttu-id="b27eb-613">http://AJAX.aspnetcdn.com/AJAX/globalize/0.1.1/Cultures/globalize.Culture. {Kulturcode} .js</span><span class="sxs-lookup"><span data-stu-id="b27eb-613">http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js</span></span>

    - <span data-ttu-id="b27eb-614">Ersetzen Sie "{Kulturcode}" durch den gewünschten Kulturcode, z. B. Microsoft globalize.culture.en GB.js== Dateien auf das CDN == diese Bibliotheken von Microsoft hochgeladen wurden.</span><span class="sxs-lookup"><span data-stu-id="b27eb-614">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="b27eb-615">Reagieren Sie auf das CDN Versionen</span><span class="sxs-lookup"><span data-stu-id="b27eb-615">Respond Releases on the CDN</span></span>

<span data-ttu-id="b27eb-616">Die folgenden Versionen von [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") reagieren auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="b27eb-616">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="b27eb-617">Version 1.4.2 reagieren</span><span class="sxs-lookup"><span data-stu-id="b27eb-617">Respond version 1.4.2</span></span>

- <span data-ttu-id="b27eb-618">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-618">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js</span></span>
- <span data-ttu-id="b27eb-619">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-619">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js</span></span>
- <span data-ttu-id="b27eb-620">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.AddListener.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-620">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="b27eb-621">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.2/Respond.matchmedia.AddListener.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-621">http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-141"></a><span data-ttu-id="b27eb-622">Version 1.4.1 reagieren</span><span class="sxs-lookup"><span data-stu-id="b27eb-622">Respond version 1.4.1</span></span>

- <span data-ttu-id="b27eb-623">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-623">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js</span></span>
- <span data-ttu-id="b27eb-624">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-624">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js</span></span>
- <span data-ttu-id="b27eb-625">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.AddListener.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-625">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="b27eb-626">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.1/Respond.matchmedia.AddListener.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-626">http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-140"></a><span data-ttu-id="b27eb-627">Version 1.4.0 reagieren</span><span class="sxs-lookup"><span data-stu-id="b27eb-627">Respond version 1.4.0</span></span>

- <span data-ttu-id="b27eb-628">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-628">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js</span></span>
- <span data-ttu-id="b27eb-629">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-629">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js</span></span>
- <span data-ttu-id="b27eb-630">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.AddListener.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-630">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js</span></span>
- <span data-ttu-id="b27eb-631">http://AJAX.aspnetcdn.com/AJAX/Respond/1.4.0/Respond.matchmedia.AddListener.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-631">http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js</span></span>

#### <a name="respond-version-130"></a><span data-ttu-id="b27eb-632">Version 1.3.0 reagieren</span><span class="sxs-lookup"><span data-stu-id="b27eb-632">Respond version 1.3.0</span></span>

- <span data-ttu-id="b27eb-633">http://AJAX.aspnetcdn.com/AJAX/Respond/1.3.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-633">http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js</span></span>

#### <a name="respond-version-120"></a><span data-ttu-id="b27eb-634">Die Version 1.2.0 reagieren</span><span class="sxs-lookup"><span data-stu-id="b27eb-634">Respond version 1.2.0</span></span>

- <span data-ttu-id="b27eb-635">http://AJAX.aspnetcdn.com/AJAX/Respond/1.2.0/Respond.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-635">http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js</span></span>

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="b27eb-636">Bootstrap-Versionen auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-636">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="b27eb-637">Die folgenden Versionen von [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") Bootstrap auf dem CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="b27eb-637">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-337"></a><span data-ttu-id="b27eb-638">Bootstrap Version 3.3.7</span><span class="sxs-lookup"><span data-stu-id="b27eb-638">Bootstrap version 3.3.7</span></span>

- <span data-ttu-id="b27eb-639">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-639">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-640">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-640">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-641">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-641">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-642">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-642">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b27eb-643">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-643">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-644">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-644">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b27eb-645">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-645">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b27eb-646">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-646">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b27eb-647">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="b27eb-647">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b27eb-648">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b27eb-648">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b27eb-649">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b27eb-649">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b27eb-650">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="b27eb-650">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="b27eb-651">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.7/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="b27eb-651">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-336"></a><span data-ttu-id="b27eb-652">Bootstrap Version 3.3.6</span><span class="sxs-lookup"><span data-stu-id="b27eb-652">Bootstrap version 3.3.6</span></span>

- <span data-ttu-id="b27eb-653">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-653">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-654">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-654">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-655">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-655">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-656">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-656">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b27eb-657">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-657">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-658">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-658">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b27eb-659">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-659">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b27eb-660">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-660">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b27eb-661">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="b27eb-661">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b27eb-662">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b27eb-662">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b27eb-663">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b27eb-663">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b27eb-664">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="b27eb-664">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="b27eb-665">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.6/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="b27eb-665">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-335"></a><span data-ttu-id="b27eb-666">Bootstrap Version 3.3.5</span><span class="sxs-lookup"><span data-stu-id="b27eb-666">Bootstrap version 3.3.5</span></span>

- <span data-ttu-id="b27eb-667">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-667">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-668">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-668">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-669">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-669">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-670">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-670">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b27eb-671">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-671">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-672">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-672">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b27eb-673">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-673">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b27eb-674">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-674">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b27eb-675">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="b27eb-675">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b27eb-676">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b27eb-676">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b27eb-677">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b27eb-677">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b27eb-678">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="b27eb-678">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="b27eb-679">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.5/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="b27eb-679">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-334"></a><span data-ttu-id="b27eb-680">Bootstrap Version 3.3.4</span><span class="sxs-lookup"><span data-stu-id="b27eb-680">Bootstrap version 3.3.4</span></span>

- <span data-ttu-id="b27eb-681">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-681">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-682">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-682">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-683">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-683">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-684">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-684">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b27eb-685">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-685">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-686">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-686">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b27eb-687">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-687">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b27eb-688">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-688">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b27eb-689">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="b27eb-689">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b27eb-690">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b27eb-690">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b27eb-691">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b27eb-691">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b27eb-692">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="b27eb-692">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="b27eb-693">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.4/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="b27eb-693">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-332"></a><span data-ttu-id="b27eb-694">Bootstrap Version 3.3.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-694">Bootstrap version 3.3.2</span></span>

- <span data-ttu-id="b27eb-695">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-695">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-696">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-696">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-697">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-697">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-698">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-698">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b27eb-699">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-699">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-700">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-700">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b27eb-701">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-701">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b27eb-702">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-702">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b27eb-703">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="b27eb-703">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b27eb-704">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b27eb-704">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b27eb-705">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b27eb-705">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b27eb-706">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="b27eb-706">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff</span></span>
- <span data-ttu-id="b27eb-707">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.2/Fonts/glyphicons-halflings-Regular.woff2</span><span class="sxs-lookup"><span data-stu-id="b27eb-707">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2</span></span>

#### <a name="bootstrap-version-331"></a><span data-ttu-id="b27eb-708">Bootstrap Version 3.3.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-708">Bootstrap version 3.3.1</span></span>

- <span data-ttu-id="b27eb-709">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-709">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-710">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-710">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-711">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-711">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-712">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-712">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b27eb-713">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-713">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-714">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-714">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b27eb-715">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-715">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b27eb-716">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-716">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b27eb-717">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="b27eb-717">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b27eb-718">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b27eb-718">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b27eb-719">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b27eb-719">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b27eb-720">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="b27eb-720">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-330"></a><span data-ttu-id="b27eb-721">Bootstrap Version 3.3.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-721">Bootstrap version 3.3.0</span></span>

- <span data-ttu-id="b27eb-722">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-722">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-723">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-723">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-724">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-724">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-725">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-725">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b27eb-726">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-726">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-727">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-727">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b27eb-728">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-728">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b27eb-729">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-729">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b27eb-730">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="b27eb-730">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b27eb-731">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b27eb-731">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b27eb-732">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b27eb-732">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b27eb-733">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.3.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="b27eb-733">http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-320"></a><span data-ttu-id="b27eb-734">Bootstrap Version 3.2.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-734">Bootstrap version 3.2.0</span></span>

- <span data-ttu-id="b27eb-735">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-735">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-736">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-736">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-737">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-737">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-738">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-738">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b27eb-739">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-739">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-740">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-740">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b27eb-741">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-741">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b27eb-742">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-742">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b27eb-743">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="b27eb-743">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b27eb-744">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b27eb-744">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b27eb-745">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b27eb-745">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b27eb-746">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.2.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="b27eb-746">http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-311"></a><span data-ttu-id="b27eb-747">Bootstrap Version 3.1.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-747">Bootstrap version 3.1.1</span></span>

- <span data-ttu-id="b27eb-748">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-748">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-749">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-749">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-750">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-750">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-751">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-751">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b27eb-752">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-752">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-753">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-753">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b27eb-754">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-754">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b27eb-755">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-755">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b27eb-756">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="b27eb-756">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b27eb-757">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b27eb-757">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b27eb-758">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b27eb-758">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b27eb-759">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="b27eb-759">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-310"></a><span data-ttu-id="b27eb-760">Bootstrap Version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-760">Bootstrap version 3.1.0</span></span>

- <span data-ttu-id="b27eb-761">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-761">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-762">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-762">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-763">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-763">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-764">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-764">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map</span></span>
- <span data-ttu-id="b27eb-765">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-765">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-766">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-766">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b27eb-767">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.CSS.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-767">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map</span></span>
- <span data-ttu-id="b27eb-768">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-768">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b27eb-769">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="b27eb-769">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b27eb-770">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b27eb-770">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b27eb-771">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b27eb-771">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b27eb-772">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.1.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="b27eb-772">http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-303"></a><span data-ttu-id="b27eb-773">Bootstrap Version 3.0.3.</span><span class="sxs-lookup"><span data-stu-id="b27eb-773">Bootstrap version 3.0.3</span></span>

- <span data-ttu-id="b27eb-774">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-774">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-775">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-775">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-776">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-776">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-777">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-777">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-778">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-778">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b27eb-779">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-779">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b27eb-780">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="b27eb-780">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b27eb-781">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b27eb-781">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b27eb-782">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b27eb-782">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b27eb-783">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.3/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="b27eb-783">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-302"></a><span data-ttu-id="b27eb-784">Bootstrap Version 3.0.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-784">Bootstrap version 3.0.2</span></span>

- <span data-ttu-id="b27eb-785">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-785">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-786">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-786">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-787">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-787">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-788">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-788">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-789">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-789">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b27eb-790">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-790">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b27eb-791">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="b27eb-791">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b27eb-792">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b27eb-792">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b27eb-793">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b27eb-793">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b27eb-794">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.2/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="b27eb-794">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-301"></a><span data-ttu-id="b27eb-795">Bootstrap Version 3.0.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-795">Bootstrap version 3.0.1</span></span>

- <span data-ttu-id="b27eb-796">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-796">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-797">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-797">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-798">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-798">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-799">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-799">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-800">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-800">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b27eb-801">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-801">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b27eb-802">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="b27eb-802">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b27eb-803">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b27eb-803">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b27eb-804">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b27eb-804">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b27eb-805">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.1/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="b27eb-805">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-300"></a><span data-ttu-id="b27eb-806">Bootstrap-Version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-806">Bootstrap version 3.0.0</span></span>

- <span data-ttu-id="b27eb-807">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-807">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-808">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-808">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-809">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-809">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-810">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-810">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-811">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-811">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css</span></span>
- <span data-ttu-id="b27eb-812">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/CSS/Bootstrap-Theme.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-812">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css</span></span>
- <span data-ttu-id="b27eb-813">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.eot</span><span class="sxs-lookup"><span data-stu-id="b27eb-813">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot</span></span>
- <span data-ttu-id="b27eb-814">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.SVG</span><span class="sxs-lookup"><span data-stu-id="b27eb-814">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg</span></span>
- <span data-ttu-id="b27eb-815">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.ttf</span><span class="sxs-lookup"><span data-stu-id="b27eb-815">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf</span></span>
- <span data-ttu-id="b27eb-816">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/3.0.0/Fonts/glyphicons-halflings-Regular.woff</span><span class="sxs-lookup"><span data-stu-id="b27eb-816">http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff</span></span>

#### <a name="bootstrap-version-232"></a><span data-ttu-id="b27eb-817">Bootstrap-Version 2.3.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-817">Bootstrap version 2.3.2</span></span>

- <span data-ttu-id="b27eb-818">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-818">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-819">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-819">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-820">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-820">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-821">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-821">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-822">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-Responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-822">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="b27eb-823">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/CSS/Bootstrap-Responsive.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-823">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="b27eb-824">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="b27eb-824">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="b27eb-825">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.2/img/glyphicons-halflings-White.PNG</span><span class="sxs-lookup"><span data-stu-id="b27eb-825">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png</span></span>

#### <a name="bootstrap-version-231"></a><span data-ttu-id="b27eb-826">Bootstrap Version 2.3.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-826">Bootstrap version 2.3.1</span></span>

- <span data-ttu-id="b27eb-827">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-827">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js</span></span>
- <span data-ttu-id="b27eb-828">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/Bootstrap.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-828">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js</span></span>
- <span data-ttu-id="b27eb-829">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-829">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css</span></span>
- <span data-ttu-id="b27eb-830">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-830">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css</span></span>
- <span data-ttu-id="b27eb-831">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-Responsive.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-831">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css</span></span>
- <span data-ttu-id="b27eb-832">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/CSS/Bootstrap-Responsive.Min.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-832">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css</span></span>
- <span data-ttu-id="b27eb-833">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings.PNG</span><span class="sxs-lookup"><span data-stu-id="b27eb-833">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png</span></span>
- <span data-ttu-id="b27eb-834">http://AJAX.aspnetcdn.com/AJAX/Bootstrap/2.3.1/img/glyphicons-halflings-White.PNG</span><span class="sxs-lookup"><span data-stu-id="b27eb-834">http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png</span></span>

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="b27eb-835">Bootstrap TouchCarousel Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-835">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="b27eb-836">Die folgenden Versionen von [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel Versionen auf das CDN gehostet werden :</span><span class="sxs-lookup"><span data-stu-id="b27eb-836">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="b27eb-837">Bootstrap TouchCarousel Version 0.8.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-837">Bootstrap TouchCarousel version 0.8.0</span></span>

- <span data-ttu-id="b27eb-838">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-Carousel/0.8.0/CSS/Bootstrap-Touch-Carousel.CSS</span><span class="sxs-lookup"><span data-stu-id="b27eb-838">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css</span></span>
- <span data-ttu-id="b27eb-839">http://AJAX.aspnetcdn.com/AJAX/Bootstrap-Touch-Carousel/0.8.0/js/Bootstrap-Touch-Carousel.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-839">http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js</span></span>

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="b27eb-840">Hammer.js Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-840">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="b27eb-841">Die folgenden Versionen von [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js Versionen auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="b27eb-841">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="b27eb-842">Hammer.js Version 2.0.4</span><span class="sxs-lookup"><span data-stu-id="b27eb-842">Hammer.js version 2.0.4</span></span>

- <span data-ttu-id="b27eb-843">http://AJAX.aspnetcdn.com/AJAX/Hammer.js/2.0.4/Hammer.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-843">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js</span></span>
- <span data-ttu-id="b27eb-844">http://AJAX.aspnetcdn.com/AJAX/Hammer.js/2.0.4/Hammer.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-844">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js</span></span>
- <span data-ttu-id="b27eb-845">http://AJAX.aspnetcdn.com/AJAX/Hammer.js/2.0.4/Hammer.Min.Map</span><span class="sxs-lookup"><span data-stu-id="b27eb-845">http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map</span></span>

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="b27eb-846">ASP.NET Web Forms und Ajax-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-846">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="b27eb-847">Die folgenden Versionen von ASP.NET Ajax-Bibliothek, die auf das CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="b27eb-847">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="b27eb-848">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b27eb-848">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="b27eb-849">ASP.NET Web Forms- und Ajax-Version 4.5.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-849">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "ASP.NET Web Forms- und Ajax 4.5.2")
- [<span data-ttu-id="b27eb-850">ASP.NET Web Forms- und Ajax-Version 4</span><span class="sxs-lookup"><span data-stu-id="b27eb-850">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "ASP.NET Web Forms- und Ajax 4")
- [<span data-ttu-id="b27eb-851">ASP.NET Ajax Version 3.5</span><span class="sxs-lookup"><span data-stu-id="b27eb-851">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="b27eb-852">ASP.NET MVC frei, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-852">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="b27eb-853">Die folgenden ASP.NET MVC-JavaScript-Dateien, die auf diesem CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="b27eb-853">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="b27eb-854">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="b27eb-854">ASP.NET MVC 5.2.3</span></span>

- <span data-ttu-id="b27eb-855">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-855">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="b27eb-856">http://AJAX.aspnetcdn.com/AJAX/MVC/5.2.3/jQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-856">http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="b27eb-857">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-857">ASP.NET MVC 5.1</span></span>

- <span data-ttu-id="b27eb-858">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-858">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="b27eb-859">http://AJAX.aspnetcdn.com/AJAX/MVC/5.1/jQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-859">http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="b27eb-860">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-860">ASP.NET MVC 5.0</span></span>

- <span data-ttu-id="b27eb-861">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-861">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="b27eb-862">http://AJAX.aspnetcdn.com/AJAX/MVC/5.0/jQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-862">http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="b27eb-863">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-863">ASP.NET MVC 4.0</span></span>

- <span data-ttu-id="b27eb-864">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-864">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="b27eb-865">http://AJAX.aspnetcdn.com/AJAX/MVC/4.0/jQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-865">http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js</span></span>

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="b27eb-866">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-866">ASP.NET MVC 3.0</span></span>

- <span data-ttu-id="b27eb-867">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-867">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js</span></span>
- <span data-ttu-id="b27eb-868">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.unobtrusive-AJAX.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-868">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js</span></span>
- <span data-ttu-id="b27eb-869">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-869">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js</span></span>
- <span data-ttu-id="b27eb-870">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/jQuery.Validate.unobtrusive.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-870">http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js</span></span>
- <span data-ttu-id="b27eb-871">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-871">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="b27eb-872">http://AJAX.aspnetcdn.com/AJAX/MVC/3.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-872">http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="b27eb-873">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-873">ASP.NET MVC 2.0</span></span>

- <span data-ttu-id="b27eb-874">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-874">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="b27eb-875">http://AJAX.aspnetcdn.com/AJAX/MVC/2.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-875">http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js</span></span>

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="b27eb-876">ASP.NET MVC 1,0</span><span class="sxs-lookup"><span data-stu-id="b27eb-876">ASP.NET MVC 1.0</span></span>

- <span data-ttu-id="b27eb-877">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-877">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js</span></span>
- <span data-ttu-id="b27eb-878">http://AJAX.aspnetcdn.com/AJAX/MVC/1.0/MicrosoftMvcAjax.Debug.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-878">http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js</span></span>

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="b27eb-879">ASP.NET SignalR frei, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="b27eb-879">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="b27eb-880">Die folgenden ASP.NET SignalR JavaScript-Dateien, die auf diesem CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="b27eb-880">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="b27eb-881">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-881">ASP.NET SignalR 2.2.2</span></span>

- <span data-ttu-id="b27eb-882">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-882">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js</span></span>
- <span data-ttu-id="b27eb-883">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-883">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js</span></span>

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="b27eb-884">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-884">ASP.NET SignalR 2.2.1</span></span>

- <span data-ttu-id="b27eb-885">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-885">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js</span></span>
- <span data-ttu-id="b27eb-886">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-886">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js</span></span>

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="b27eb-887">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-887">ASP.NET SignalR 2.2.0</span></span>

- <span data-ttu-id="b27eb-888">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-888">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js</span></span>
- <span data-ttu-id="b27eb-889">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.2.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-889">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js</span></span>

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="b27eb-890">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-890">ASP.NET SignalR 2.1.0</span></span>

- <span data-ttu-id="b27eb-891">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-891">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js</span></span>
- <span data-ttu-id="b27eb-892">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.1.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-892">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js</span></span>

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="b27eb-893">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="b27eb-893">ASP.NET SignalR 2.0.3</span></span>

- <span data-ttu-id="b27eb-894">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-894">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js</span></span>
- <span data-ttu-id="b27eb-895">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.3.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-895">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js</span></span>

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="b27eb-896">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-896">ASP.NET SignalR 2.0.2</span></span>

- <span data-ttu-id="b27eb-897">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-897">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js</span></span>
- <span data-ttu-id="b27eb-898">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-898">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js</span></span>

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="b27eb-899">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-899">ASP.NET SignalR 2.0.1</span></span>

- <span data-ttu-id="b27eb-900">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-900">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js</span></span>
- <span data-ttu-id="b27eb-901">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-901">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js</span></span>

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="b27eb-902">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-902">ASP.NET SignalR 2.0.0</span></span>

- <span data-ttu-id="b27eb-903">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-903">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js</span></span>
- <span data-ttu-id="b27eb-904">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-2.0.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-904">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js</span></span>

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="b27eb-905">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="b27eb-905">ASP.NET SignalR 1.1.3</span></span>

- <span data-ttu-id="b27eb-906">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-906">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js</span></span>
- <span data-ttu-id="b27eb-907">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.3.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-907">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js</span></span>

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="b27eb-908">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="b27eb-908">ASP.NET SignalR 1.1.2</span></span>

- <span data-ttu-id="b27eb-909">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-909">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js</span></span>
- <span data-ttu-id="b27eb-910">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.2.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-910">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js</span></span>

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="b27eb-911">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-911">ASP.NET SignalR 1.1.1</span></span>

- <span data-ttu-id="b27eb-912">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-912">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js</span></span>
- <span data-ttu-id="b27eb-913">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-913">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js</span></span>

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="b27eb-914">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="b27eb-914">ASP.NET SignalR 1.1.0</span></span>

- <span data-ttu-id="b27eb-915">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-915">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js</span></span>
- <span data-ttu-id="b27eb-916">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.1.0.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-916">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js</span></span>

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="b27eb-917">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="b27eb-917">ASP.NET SignalR 1.0.1</span></span>

- <span data-ttu-id="b27eb-918">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.Min.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-918">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js</span></span>
- <span data-ttu-id="b27eb-919">http://AJAX.aspnetcdn.com/AJAX/SignalR/jQuery.SignalR-1.0.1.js</span><span class="sxs-lookup"><span data-stu-id="b27eb-919">http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js</span></span>

<span data-ttu-id="b27eb-920">Weitere Informationen zu den Nutzungsbedingungen für das CDN, finden Sie unter [Microsoft Ajax-CDN Nutzungsbedingungen](https://www.asp.net/terms-of-use "Microsoft Ajax-CDN Nutzungsbedingungen").</span><span class="sxs-lookup"><span data-stu-id="b27eb-920">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
