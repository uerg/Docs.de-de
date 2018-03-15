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
ms.openlocfilehash: f1225f06e5218d893e3f49b2ccc67d56365b30e5
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
<a name="microsoft-ajax-content-delivery-network"></a><span data-ttu-id="03239-102">Microsoft Ajax Content Delivery Network</span><span class="sxs-lookup"><span data-stu-id="03239-102">Microsoft Ajax Content Delivery Network</span></span>
====================
<span data-ttu-id="03239-103">Hinweis: Das Microsoft Ajax-CDN kann keine SLA-Größe mit einer Azure-CDN.</span><span class="sxs-lookup"><span data-stu-id="03239-103">Note: The Microsoft Ajax CDN has no SLA above and beyond using an Azure CDN.</span></span>

## <a name="table-of-contents"></a><span data-ttu-id="03239-104">Inhaltsverzeichnis</span><span class="sxs-lookup"><span data-stu-id="03239-104">Table of Contents</span></span>

<span data-ttu-id="03239-105">**[AJAX.Microsoft.com in ajax.aspnetcdn.com umbenannt](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span><span class="sxs-lookup"><span data-stu-id="03239-105">**[ajax.microsoft.com renamed to ajax.aspnetcdn.com](#ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18)**</span></span>  
<span data-ttu-id="03239-106">**[Unterstützung von Visual Studio .vsdoc](#Visual_Studio_vsdoc_Support_19)**</span><span class="sxs-lookup"><span data-stu-id="03239-106">**[Visual Studio .vsdoc Support](#Visual_Studio_vsdoc_Support_19)**</span></span>  
<span data-ttu-id="03239-107">**[Mithilfe von ASP.NET Ajax vom CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span><span class="sxs-lookup"><span data-stu-id="03239-107">**[Using ASP.NET Ajax from the CDN](#Using_ASPNET_Ajax_from_the_CDN_20)**</span></span>  
<span data-ttu-id="03239-108">**[Unter Verwendung von jQuery vom CDN](#Using_jQuery_from_the_CDN_21)**</span><span class="sxs-lookup"><span data-stu-id="03239-108">**[Using jQuery from the CDN](#Using_jQuery_from_the_CDN_21)**</span></span>  
<span data-ttu-id="03239-109">**[Unter Verwendung von jQuery UI vom CDN](#Using_jQuery_UI_from_the_CDN_22)**</span><span class="sxs-lookup"><span data-stu-id="03239-109">**[Using jQuery UI from the CDN](#Using_jQuery_UI_from_the_CDN_22)**</span></span>  
<span data-ttu-id="03239-110">**[Drittanbieter-Dateien auf dem CDN](#Third-Party_Files_on_the_CDN_23)**</span><span class="sxs-lookup"><span data-stu-id="03239-110">**[Third-Party Files on the CDN](#Third-Party_Files_on_the_CDN_23)**</span></span>  
  
 [<span data-ttu-id="03239-111">jQuery-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-111">jQuery Releases on the CDN</span></span>](#jQuery_Releases_on_the_CDN_0)  
 [<span data-ttu-id="03239-112">Migrieren von jQuery-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-112">jQuery Migrate Releases on the CDN</span></span>](#jQuery_Migrate_Releases_on_the_CDN_1)  
 [<span data-ttu-id="03239-113">jQuery UI-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-113">jQuery UI Releases on the CDN</span></span>](#jQuery_UI_Releases_on_the_CDN_2)  
 [<span data-ttu-id="03239-114">jQuery-Validierung Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-114">jQuery Validation Releases on the CDN</span></span>](#jQuery_Validation_Releases_on_the_CDN_3)  
 [<span data-ttu-id="03239-115">jQuery Mobile-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-115">jQuery Mobile Releases on the CDN</span></span>](#jQuery_Mobile_Releases_on_the_CDN_4)  
 [<span data-ttu-id="03239-116">jQuery-Vorlagen-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-116">jQuery Templates Releases on the CDN</span></span>](#jQuery_Templates_Releases_on_the_CDN_5)  
 [<span data-ttu-id="03239-117">jQuery-Zyklus Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-117">jQuery Cycle Releases on the CDN</span></span>](#jQuery_Cycle_Releases_on_the_CDN_6)  
 [<span data-ttu-id="03239-118">jQuery DataTables Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-118">jQuery DataTables Releases on the CDN</span></span>](#jQuery_DataTables_Releases_on_the_CDN_7)  
 [<span data-ttu-id="03239-119">Modernizr Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-119">Modernizr Releases on the CDN</span></span>](#Modernizr_Releases_on_the_CDN_8)  
 [<span data-ttu-id="03239-120">JSHint Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-120">JSHint Releases on the CDN</span></span>](#JSHint_Releases_on_the_CDN_10)  
 [<span data-ttu-id="03239-121">Knockout Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-121">Knockout Releases on the CDN</span></span>](#Knockout_Releases_on_the_CDN_11)  
 [<span data-ttu-id="03239-122">Bei der Globalisierung Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-122">Globalize Releases on the CDN</span></span>](#Globalize_Releases_on_the_CDN_12)  
 [<span data-ttu-id="03239-123">Reagieren Sie auf das CDN Versionen</span><span class="sxs-lookup"><span data-stu-id="03239-123">Respond Releases on the CDN</span></span>](#Respond_Releases_on_the_CDN_13)  
 [<span data-ttu-id="03239-124">Bootstrap-Versionen auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-124">Bootstrap Releases on the CDN</span></span>](#Bootstrap_Releases_on_the_CDN_14)  
 [<span data-ttu-id="03239-125">Bootstrap TouchCarousel Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-125">Bootstrap TouchCarousel Releases on the CDN</span></span>](#BootstrapTouchCarousel_Releases_on_the_CDN_18)  
 [<span data-ttu-id="03239-126">Hammer.js Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-126">Hammer.js Releases on the CDN</span></span>](#Hammerjs_Releases_on_the_CDN_19)  
 [<span data-ttu-id="03239-127">ASP.NET Web Forms und Ajax-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-127">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>](#ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15)  
 [<span data-ttu-id="03239-128">ASP.NET MVC frei, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-128">ASP.NET MVC Releases on the CDN</span></span>](#ASPNET_MVC_Releases_on_the_CDN_16)  
 [<span data-ttu-id="03239-129">ASP.NET SignalR frei, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-129">ASP.NET SignalR Releases on the CDN</span></span>](#ASPNET_SignalR_Releases_on_the_CDN_17)

<span data-ttu-id="03239-130">Der Microsoft Ajax Content Delivery Network (CDN) hostet beliebten Drittanbieter-JavaScript-Bibliotheken wie z. B. jQuery und ermöglicht es Ihnen, einfach die Webanwendungen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="03239-130">The Microsoft Ajax Content Delivery Network (CDN) hosts popular third party JavaScript libraries such as jQuery and enables you to easily add them to your Web applications.</span></span> <span data-ttu-id="03239-131">Sie können z. B. starten, unter Verwendung von jQuery der gehostet wird auf diesem CDN einfach durch Hinzufügen einer &lt;Skript&gt; -Tag, um die Seite, die auf ajax.aspnetcdn.com verweist.</span><span class="sxs-lookup"><span data-stu-id="03239-131">For example, you can start using jQuery which is hosted on this CDN simply by adding a &lt;script&gt; tag to your page that points to ajax.aspnetcdn.com.</span></span>

<span data-ttu-id="03239-132">Durch das CDN nutzen, können Sie die Leistung der Ajax-Anwendungen erheblich verbessern.</span><span class="sxs-lookup"><span data-stu-id="03239-132">By taking advantage of the CDN, you can significantly improve the performance of your Ajax applications.</span></span> <span data-ttu-id="03239-133">Auf Servern, die auf der ganzen Welt werden der Inhalt des CDN zwischengespeichert.</span><span class="sxs-lookup"><span data-stu-id="03239-133">The contents of the CDN are cached on servers located around the world.</span></span> <span data-ttu-id="03239-134">Darüber hinaus ermöglicht das CDN Browsern zum Wiederverwenden von zwischengespeicherten Drittanbieter-JavaScript-Dateien für Websites, die in unterschiedlichen Domänen befinden.</span><span class="sxs-lookup"><span data-stu-id="03239-134">In addition, the CDN enables browsers to reuse cached third party JavaScript files for web sites that are located in different domains.</span></span>

<span data-ttu-id="03239-135">Das CDN unterstützt SSL (HTTPS) an, für den Fall, dass Sie einer Webseite mithilfe des Secure Sockets Layers bereitstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="03239-135">The CDN supports SSL (HTTPS) in case you need to serve a web page using the Secure Sockets Layer.</span></span>

<span data-ttu-id="03239-136">Das CDN hostet die folgenden Drittanbieter-Skriptbibliotheken hochgeladen worden sein, und für Sie lizenziert sind, die von den Besitzern von diesen Bibliotheken:</span><span class="sxs-lookup"><span data-stu-id="03239-136">The CDN hosts the following third party script libraries which have been uploaded, and are licensed to you, by the owners of those libraries:</span></span>

- <span data-ttu-id="03239-137">jQuery (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="03239-137">jQuery (www.jquery.com)</span></span>
- <span data-ttu-id="03239-138">jQuery UI (www.jqueryui.com)</span><span class="sxs-lookup"><span data-stu-id="03239-138">jQuery UI (www.jqueryui.com)</span></span>
- <span data-ttu-id="03239-139">jQuery Mobile (www.jquerymobile.com)</span><span class="sxs-lookup"><span data-stu-id="03239-139">jQuery Mobile (www.jquerymobile.com)</span></span>
- <span data-ttu-id="03239-140">jQuery-Validierung (www.jquery.com)</span><span class="sxs-lookup"><span data-stu-id="03239-140">jQuery Validation (www.jquery.com)</span></span>
- <span data-ttu-id="03239-141">jQuery-Zyklus (www.malsup.com/jquery/cycle/)</span><span class="sxs-lookup"><span data-stu-id="03239-141">jQuery Cycle (www.malsup.com/jquery/cycle/)</span></span>
- <span data-ttu-id="03239-142">jQuery DataTables)http://datatables.net/)</span><span class="sxs-lookup"><span data-stu-id="03239-142">jQuery DataTables (http://datatables.net/)</span></span>

<span data-ttu-id="03239-143">Das Microsoft Ajax-CDN umfasst außerdem die folgenden Bibliotheken, die von Microsoft hochgeladen wurden:</span><span class="sxs-lookup"><span data-stu-id="03239-143">The Microsoft Ajax CDN also includes the following libraries which have been uploaded by Microsoft:</span></span>

- <span data-ttu-id="03239-144">ASP.NET Ajax</span><span class="sxs-lookup"><span data-stu-id="03239-144">ASP.NET Ajax</span></span>
- <span data-ttu-id="03239-145">ASP.NET MVC JavaScript Files</span><span class="sxs-lookup"><span data-stu-id="03239-145">ASP.NET MVC JavaScript Files</span></span>
- <span data-ttu-id="03239-146">ASP.NET SignalR JavaScript-Dateien</span><span class="sxs-lookup"><span data-stu-id="03239-146">ASP.NET SignalR JavaScript Files</span></span>

<span data-ttu-id="03239-147">Microsoft erhebt keine Ansprüche für den Besitz von Drittanbieter-Bibliotheken, die auf diesem CDN gehostet.</span><span class="sxs-lookup"><span data-stu-id="03239-147">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="03239-148">Diese Bibliotheken sind den Urheberrechtsinhabern der Bibliotheken entschieden haben.</span><span class="sxs-lookup"><span data-stu-id="03239-148">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="03239-149">Alle Rechte, die Sie möglicherweise herunterladen und verwenden diese Bibliotheken werden ausschließlich von der jeweiligen Urheberrechtsinhabern gewährt.</span><span class="sxs-lookup"><span data-stu-id="03239-149">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="03239-150">Da keine Microsoft-Bibliotheken sind, bietet Microsoft keine GEWÄHRLEISTUNGEN oder geistiges Eigentum Rechte Lizenzen (einschließlich keine implizite Patentrechte), für die Drittanbieter-Bibliotheken, die auf diesem CDN gehostet.</span><span class="sxs-lookup"><span data-stu-id="03239-150">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<span data-ttu-id="03239-151">Wenn Sie JavaScript-Bibliothek senden möchten, und die Bibliothek eine von der obersten JavaScript-Bibliotheken ist (wie aufgeführt http://trends.builtwith.com) oder Erweiterungen/Plugins an diese Bibliotheken, die (a) beliebten; oder (b) für die Verwendung in ASP.NET hilfreich sind, dann wenden Sie sich an AjaxCDNSubmission@Microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="03239-151">If you wish to submit your JavaScript library and your library is one of the top JavaScript libraries (as listed on http://trends.builtwith.com) or extensions/plugins to these libraries that are (a) popular; or (b) helpful for use on ASP.NET then please contact AjaxCDNSubmission@Microsoft.com.</span></span>

<a id="ajaxmicrosoftcom_renamed_to_ajaxaspnetcdncom_18"></a>

## <a name="ajaxmicrosoftcom-renamed-to-ajaxaspnetcdncom"></a><span data-ttu-id="03239-152">AJAX.Microsoft.com in ajax.aspnetcdn.com umbenannt</span><span class="sxs-lookup"><span data-stu-id="03239-152">ajax.microsoft.com renamed to ajax.aspnetcdn.com</span></span>

<span data-ttu-id="03239-153">Das CDN verwendet, um den Domänennamen "Microsoft.com" verwenden und wurde geändert, um den Domänennamen aspnetcdn.com verwenden.</span><span class="sxs-lookup"><span data-stu-id="03239-153">The CDN used to use the microsoft.com domain name and has been changed to use the aspnetcdn.com domain name.</span></span> <span data-ttu-id="03239-154">Diese Änderung wurde vorgenommen, um die Leistung zu erhöhen, da bei ein Browser mit die Domäne "Microsoft.com" verwiesen wird er alle Cookies aus dieser Domäne über das Netz mit jeder Anforderung sendet.</span><span class="sxs-lookup"><span data-stu-id="03239-154">This change was made to increase performance because when a browser referenced the microsoft.com domain it would send any cookies from that domain across the wire with each request.</span></span> <span data-ttu-id="03239-155">Durch das Umbenennen eines Domänennamens als "Microsoft.com" kann die Leistung von möglichst alle Aufgaben auf 25 % erhöht werden.</span><span class="sxs-lookup"><span data-stu-id="03239-155">By renaming to a domain name other than microsoft.com performance can be increased by as much to 25%.</span></span> <span data-ttu-id="03239-156">Beachten Sie ajax.microsoft.com wird zwar weiterhin ajax.aspnetcdn.com wird jedoch empfohlen.</span><span class="sxs-lookup"><span data-stu-id="03239-156">Note ajax.microsoft.com will continue to function but ajax.aspnetcdn.com is recommended.</span></span>

- <span data-ttu-id="03239-157">Altes Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="03239-157">Old Format: http://ajax.microsoft.com/ajax/jQuery/jquery-1.8.0.js</span></span>
- <span data-ttu-id="03239-158">Neues Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span><span class="sxs-lookup"><span data-stu-id="03239-158">New Format: http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js</span></span>

<a id="Visual_Studio_vsdoc_Support_19"></a>

## <a name="visual-studio-vsdoc-support"></a><span data-ttu-id="03239-159">Unterstützung von Visual Studio .vsdoc</span><span class="sxs-lookup"><span data-stu-id="03239-159">Visual Studio .vsdoc Support</span></span>

<span data-ttu-id="03239-160">Die .vsdoc Dateien ordnungsgemäß zu mit Visual Studio 2008 verwenden, müssen Sie sicherstellen, dass Sie Visual Studio 2008 SP1 verfügen, und der Hotfix für das Vsdoc-Dateien installiert.</span><span class="sxs-lookup"><span data-stu-id="03239-160">To use the .vsdoc files properly with Visual Studio 2008 you need to make sure that you have VS 2008 SP1 installed and the hotfix for vsdoc files installed.</span></span> <span data-ttu-id="03239-161">Sie können diese hier abrufen:</span><span class="sxs-lookup"><span data-stu-id="03239-161">You can get these from here:</span></span>

- [<span data-ttu-id="03239-162">Herunterladen der Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="03239-162">Download Visual Studio 2008 SP1</span></span>](https://www.microsoft.com/downloads/en/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en "Herunterladen der Visual Studio 2008 SP1")
- [<span data-ttu-id="03239-163">Herunterladen von .vsdoc Hotfix für Visual Studio 2008 SP1</span><span class="sxs-lookup"><span data-stu-id="03239-163">Download .vsdoc hotfix for Visual Studio 2008 SP1</span></span>](https://code.msdn.microsoft.com/KB958502/Release/ProjectReleases.aspx?ReleaseId=1736 ".vsdoc Hotfix für Visual Studio 2008 SP1 herunterladen")

<span data-ttu-id="03239-164">Visual Studio 2010 unterstützt .vsdoc Dateien ohne zusätzliche Patches.</span><span class="sxs-lookup"><span data-stu-id="03239-164">Visual Studio 2010 supports .vsdoc files without any additional patches.</span></span>

<a id="Using_ASPNET_Ajax_from_the_CDN_20"></a>

## <a name="using-aspnet-ajax-from-the-cdn"></a><span data-ttu-id="03239-165">Mithilfe von ASP.NET Ajax vom CDN</span><span class="sxs-lookup"><span data-stu-id="03239-165">Using ASP.NET Ajax from the CDN</span></span>

<span data-ttu-id="03239-166">Wenn ASP.NET 4 zu verwenden, können Sie alle Anforderungen für ASP.NET Frameworkskripts auf das CDN umleiten.</span><span class="sxs-lookup"><span data-stu-id="03239-166">When using ASP.NET 4, you can redirect all requests for ASP.NET framework scripts to the CDN.</span></span> <span data-ttu-id="03239-167">Abrufen von Skripts aus dem CDN anstelle von Ihren lokalen Webserver kann die Leistung von öffentlichen ASP.NET-Websites erheblich verbessern.</span><span class="sxs-lookup"><span data-stu-id="03239-167">Retrieving scripts from the CDN instead of your local web server can substantially improve the performance of public ASP.NET websites.</span></span>

<span data-ttu-id="03239-168">Verwenden Sie die ScriptManager EnableCDN-Eigenschaft, um alle ASP.NET Framework Skript Anforderungen an das Microsoft Ajax-CDN umleiten:</span><span class="sxs-lookup"><span data-stu-id="03239-168">Use the ScriptManager EnableCDN property to redirect all ASP.NET framework script requests to the Microsoft Ajax CDN:</span></span>

[!code-aspx[Main](overview/samples/sample1.aspx)]

<a id="Using_jQuery_from_the_CDN_21"></a>

## <a name="using-jquery-from-the-cdn"></a><span data-ttu-id="03239-169">Unter Verwendung von jQuery vom CDN</span><span class="sxs-lookup"><span data-stu-id="03239-169">Using jQuery from the CDN</span></span>

<span data-ttu-id="03239-170">Sie können die jQuery-Skripts, das durch Hinzufügen der folgenden Script-Element zu einer Seite in der Webanwendung auf CDN gehostet:</span><span class="sxs-lookup"><span data-stu-id="03239-170">You can use jQuery scripts hosted on CDN in your Web application by adding the following script element to a page:</span></span>

[!code-html[Main](overview/samples/sample2.html)]

<span data-ttu-id="03239-171">Das CDN enthält auch die verkleinerte Version des jQuery-Skripts, die Sie abrufen können mithilfe des folgenden Elements:</span><span class="sxs-lookup"><span data-stu-id="03239-171">The CDN also includes the minified version of the jQuery script, which you can get using the following element:</span></span>

[!code-html[Main](overview/samples/sample3.html)]

<span data-ttu-id="03239-172">Damit wird die Seite fallback auf jQuery aus einem lokalen Pfad auf eine eigene Website laden, wenn das CDN nicht verfügbar ist, erfolgt, fügen Sie das folgende Element sofort nach dem Element, das Verweisen auf das CDN hinzu:</span><span class="sxs-lookup"><span data-stu-id="03239-172">To allow your page to fallback to loading jQuery from a local path on your own website if the CDN happens to be unavailable, add the following element immediately after the element referencing the CDN:</span></span>

[!code-html[Main](overview/samples/sample4.html)]

<span data-ttu-id="03239-173">Die folgenden Beispielseite verwendet die CDN-Version der jQuery-Bibliothek (mit Fallback auf eine lokale Kopie), zeigen Sie den Inhalt von einem Div-Element, wenn auf eine Schaltfläche geklickt wird.</span><span class="sxs-lookup"><span data-stu-id="03239-173">The following sample page uses the CDN version of the jQuery library (with fallback to a local copy) to display the contents of a div element when a button is clicked.</span></span>

[!code-html[Main](overview/samples/sample5.html)]

<span data-ttu-id="03239-174">Sie können erfahren Sie mehr über jQuery und eine lokale Kopie des jQuery herunterzuladen, besuchen die [jQuery](http://jquery.com/) Website.</span><span class="sxs-lookup"><span data-stu-id="03239-174">You can learn more about jQuery and download a local copy of jQuery by visiting the [jQuery](http://jquery.com/) Web site.</span></span>

<a id="Using_jQuery_UI_from_the_CDN_22"></a>

## <a name="using-jquery-ui-from-the-cdn"></a><span data-ttu-id="03239-175">Unter Verwendung von jQuery UI vom CDN</span><span class="sxs-lookup"><span data-stu-id="03239-175">Using jQuery UI from the CDN</span></span>

<span data-ttu-id="03239-176">Das CDN hostet auch die jQuery UI-Bibliothek.</span><span class="sxs-lookup"><span data-stu-id="03239-176">The CDN also hosts the jQuery UI library.</span></span> <span data-ttu-id="03239-177">Die jQuery UI-Bibliothek enthält einen umfangreichen Satz von Widgets und Effekte, die Sie in Ihrer ASP.NET-Anwendungen verwenden können.</span><span class="sxs-lookup"><span data-stu-id="03239-177">The jQuery UI library includes a rich set of widgets and effects that you can use in your ASP.NET applications.</span></span> <span data-ttu-id="03239-178">Die folgende Seite wird beispielsweise veranschaulicht, wie Sie dem jQuery UI Datepicker im Kontext einer ASP.NET Web Forms-Anwendung verwenden können, um ein Popupkalender angezeigt:</span><span class="sxs-lookup"><span data-stu-id="03239-178">For example, the following page illustrates how you can use the jQuery UI Datepicker in the context of an ASP.NET Web Forms application to display a pop-up calendar:</span></span>

[!code-aspx[Main](overview/samples/sample6.aspx)]

<span data-ttu-id="03239-179">Wenn Sie den Fokus auf das Textfeld mit der Tastatur zu verschieben, wird ein Kalender angezeigt:</span><span class="sxs-lookup"><span data-stu-id="03239-179">When you move focus to the TextBox using your keyboard, a calendar is displayed:</span></span>

![Erstellt mit Datepicker-Popupkalenders](overview/_static/image1.png)

<span data-ttu-id="03239-181">Beachten Sie, dass im obigen Code drei Dateien aus dem CDN aufnehmen müssen:</span><span class="sxs-lookup"><span data-stu-id="03239-181">Notice that you must include three files from the CDN in the code above:</span></span>

- <span data-ttu-id="03239-182">Die jQuery-Bibliothek &mdash; die jQuery UI-Bibliothek für die jQuery-Bibliothek abhängig ist.</span><span class="sxs-lookup"><span data-stu-id="03239-182">The jQuery library &mdash; The jQuery UI library depends on the jQuery library.</span></span> <span data-ttu-id="03239-183">Sie müssen die jQuery-Bibliothek auf der Seite vor dem Hinzufügen der jQuery UI-Bibliothek hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="03239-183">You must add the jQuery library to your page before you add the jQuery UI library.</span></span>
- <span data-ttu-id="03239-184">Der jQuery UI-Bibliothek &mdash; die jQuery UI-Bibliothek enthält alle Benutzeroberflächeneffekte jQuery und Widgets, z. B. das Datepicker-Widget in der oben genannten Seite verwendet.</span><span class="sxs-lookup"><span data-stu-id="03239-184">The jQuery UI library &mdash; The jQuery UI library contains all of the jQuery UI effects and widgets such as the Datepicker widget used in the page above.</span></span>
- <span data-ttu-id="03239-185">Ein Design der jQuery UI &mdash; jQuery UI unterstützt verschiedene Designs.</span><span class="sxs-lookup"><span data-stu-id="03239-185">A jQuery UI theme &mdash; The jQuery UI supports different themes.</span></span> <span data-ttu-id="03239-186">Die Seite enthält einen Link in einer CSS-Datei, die das Design "Redmond" zu importieren.</span><span class="sxs-lookup"><span data-stu-id="03239-186">The page above includes a link to a CSS file to import the Redmond theme.</span></span>

<span data-ttu-id="03239-187">Alle von der standardmäßigen jQuery UI-Designs werden auf das CDN gehostet.</span><span class="sxs-lookup"><span data-stu-id="03239-187">All of the standard jQuery UI themes are hosted on the CDN.</span></span> <span data-ttu-id="03239-188">[Besuchen Sie diese Seite](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 auf das Microsoft Ajax-CDN") Miniaturansichten für jeden Design anzeigen.</span><span class="sxs-lookup"><span data-stu-id="03239-188">[Visit this page](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 on the Microsoft Ajax CDN") to view thumbnails for each theme.</span></span>

<span data-ttu-id="03239-189">Weitere Informationen zu der jQuery UI-Bibliothek finden Sie auf der offiziellen [jQuery UI Website](http://jQueryUI.com "jQuery UI Website").</span><span class="sxs-lookup"><span data-stu-id="03239-189">To learn more about the jQuery UI library, visit the official [jQuery UI website](http://jQueryUI.com "jQuery UI website").</span></span>

<a id="Third-Party_Files_on_the_CDN_23"></a>

## <a name="third-party-files-on-the-cdn"></a><span data-ttu-id="03239-190">Drittanbieter-Dateien auf dem CDN</span><span class="sxs-lookup"><span data-stu-id="03239-190">Third-Party Files on the CDN</span></span>

<span data-ttu-id="03239-191">Das CDN hostet einiger der beliebtesten JavaScript-Bibliotheken von Drittanbietern.</span><span class="sxs-lookup"><span data-stu-id="03239-191">The CDN hosts some of the most popular third party JavaScript libraries.</span></span> <span data-ttu-id="03239-192">Microsoft erhebt keine Ansprüche für den Besitz von Drittanbieter-Bibliotheken, die auf diesem CDN gehostet.</span><span class="sxs-lookup"><span data-stu-id="03239-192">Microsoft does not claim ownership of any third-party libraries hosted on this CDN.</span></span> <span data-ttu-id="03239-193">Diese Bibliotheken sind den Urheberrechtsinhabern der Bibliotheken entschieden haben.</span><span class="sxs-lookup"><span data-stu-id="03239-193">The copyright owners of the libraries are licensing these libraries to you.</span></span> <span data-ttu-id="03239-194">Alle Rechte, die Sie möglicherweise herunterladen und verwenden diese Bibliotheken werden ausschließlich von der jeweiligen Urheberrechtsinhabern gewährt.</span><span class="sxs-lookup"><span data-stu-id="03239-194">Any rights that you may have to download and use such libraries are granted solely by the respective copyright owners.</span></span> <span data-ttu-id="03239-195">Da keine Microsoft-Bibliotheken sind, bietet Microsoft keine GEWÄHRLEISTUNGEN oder geistiges Eigentum Rechte Lizenzen (einschließlich keine implizite Patentrechte), für die Drittanbieter-Bibliotheken, die auf diesem CDN gehostet.</span><span class="sxs-lookup"><span data-stu-id="03239-195">Because these are not Microsoft libraries, Microsoft provides no warranties or intellectual property rights licenses (including no implied patent rights) for the third party libraries hosted on this CDN.</span></span>

<a id="jQuery_Releases_on_the_CDN_0"></a>

### <a name="jquery-releases-on-the-cdn"></a><span data-ttu-id="03239-196">jQuery-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-196">jQuery Releases on the CDN</span></span>

<span data-ttu-id="03239-197">Die folgenden Versionen von jQuery, die auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="03239-197">The following releases of jQuery are hosted on the CDN:</span></span>

#### <a name="jquery-version-331"></a><span data-ttu-id="03239-198">jQuery version 3.3.1</span><span class="sxs-lookup"><span data-stu-id="03239-198">jQuery version 3.3.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.3.1.slim.min.map

#### <a name="jquery-version-321"></a><span data-ttu-id="03239-199">jQuery version 3.2.1</span><span class="sxs-lookup"><span data-stu-id="03239-199">jQuery version 3.2.1</span></span>
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.1.slim.min.map

#### <a name="jquery-version-320"></a><span data-ttu-id="03239-200">jQuery version 3.2.0</span><span class="sxs-lookup"><span data-stu-id="03239-200">jQuery version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.2.0.slim.min.map

#### <a name="jquery-version-311"></a><span data-ttu-id="03239-201">jQuery-Version 3.1.1</span><span class="sxs-lookup"><span data-stu-id="03239-201">jQuery version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.1.slim.min.map

#### <a name="jquery-version-310"></a><span data-ttu-id="03239-202">jQuery-Version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="03239-202">jQuery version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.1.0.slim.min.map

#### <a name="jquery-version-300"></a><span data-ttu-id="03239-203">jQuery version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="03239-203">jQuery version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.min.map
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-3.0.0.slim.min.map

#### <a name="jquery-version-224"></a><span data-ttu-id="03239-204">jQuery version 2.2.4</span><span class="sxs-lookup"><span data-stu-id="03239-204">jQuery version 2.2.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.4.min.map

#### <a name="jquery-version-223"></a><span data-ttu-id="03239-205">jQuery-Version 2.2.3</span><span class="sxs-lookup"><span data-stu-id="03239-205">jQuery version 2.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.3.min.map

#### <a name="jquery-version-222"></a><span data-ttu-id="03239-206">jQuery-Version 2.2.2</span><span class="sxs-lookup"><span data-stu-id="03239-206">jQuery version 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.2.min.map

#### <a name="jquery-version-221"></a><span data-ttu-id="03239-207">jQuery-Version 2.2.1</span><span class="sxs-lookup"><span data-stu-id="03239-207">jQuery version 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.1.min.map

#### <a name="jquery-version-220"></a><span data-ttu-id="03239-208">jQuery-Version 2.2.0</span><span class="sxs-lookup"><span data-stu-id="03239-208">jQuery version 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.2.0.min.map

#### <a name="jquery-version-214"></a><span data-ttu-id="03239-209">jQuery-Version 2.1.4</span><span class="sxs-lookup"><span data-stu-id="03239-209">jQuery version 2.1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.4.min.map

#### <a name="jquery-version-213"></a><span data-ttu-id="03239-210">jQuery-Version 2.1.3</span><span class="sxs-lookup"><span data-stu-id="03239-210">jQuery version 2.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.3.min.map

#### <a name="jquery-version-212"></a><span data-ttu-id="03239-211">jQuery-Version 2.1.2</span><span class="sxs-lookup"><span data-stu-id="03239-211">jQuery version 2.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.2.min.js

#### <a name="jquery-version-211"></a><span data-ttu-id="03239-212">jQuery-Version 2.1.1</span><span class="sxs-lookup"><span data-stu-id="03239-212">jQuery version 2.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.1.min.map

#### <a name="jquery-version-210"></a><span data-ttu-id="03239-213">jQuery-Version 2.1.0</span><span class="sxs-lookup"><span data-stu-id="03239-213">jQuery version 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.1.0.min.map

#### <a name="jquery-version-203"></a><span data-ttu-id="03239-214">jQuery-Version 2.0.3</span><span class="sxs-lookup"><span data-stu-id="03239-214">jQuery version 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.3.min.map

#### <a name="jquery-version-202"></a><span data-ttu-id="03239-215">jQuery-Version 2.0.2</span><span class="sxs-lookup"><span data-stu-id="03239-215">jQuery version 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.2.min.map

#### <a name="jquery-version-201"></a><span data-ttu-id="03239-216">jQuery-Version 2.0.1</span><span class="sxs-lookup"><span data-stu-id="03239-216">jQuery version 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.1.min.map

#### <a name="jquery-version-200"></a><span data-ttu-id="03239-217">jQuery-Version 2.0.0</span><span class="sxs-lookup"><span data-stu-id="03239-217">jQuery version 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-2.0.0.min.map

#### <a name="jquery-version-1124"></a><span data-ttu-id="03239-218">jQuery-Version 1.12.4</span><span class="sxs-lookup"><span data-stu-id="03239-218">jQuery version 1.12.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.4.min.map

#### <a name="jquery-version-1123"></a><span data-ttu-id="03239-219">jQuery-Version 1.12.3</span><span class="sxs-lookup"><span data-stu-id="03239-219">jQuery version 1.12.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.3.min.map

#### <a name="jquery-version-1122"></a><span data-ttu-id="03239-220">jQuery-Version 1.12.2</span><span class="sxs-lookup"><span data-stu-id="03239-220">jQuery version 1.12.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.2.min.map

#### <a name="jquery-version-1121"></a><span data-ttu-id="03239-221">jQuery-Version 1.12.1</span><span class="sxs-lookup"><span data-stu-id="03239-221">jQuery version 1.12.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.1.min.map

#### <a name="jquery-version-1120"></a><span data-ttu-id="03239-222">jQuery-Version 1.12.0</span><span class="sxs-lookup"><span data-stu-id="03239-222">jQuery version 1.12.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.12.0.min.map

#### <a name="jquery-version-1113"></a><span data-ttu-id="03239-223">jQuery-Version 1.11.3</span><span class="sxs-lookup"><span data-stu-id="03239-223">jQuery version 1.11.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.3.min.map

#### <a name="jquery-version-1112"></a><span data-ttu-id="03239-224">jQuery-Version 1.11.2</span><span class="sxs-lookup"><span data-stu-id="03239-224">jQuery version 1.11.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.2.min.map

#### <a name="jquery-version-1111"></a><span data-ttu-id="03239-225">jQuery-Version 1.11.1</span><span class="sxs-lookup"><span data-stu-id="03239-225">jQuery version 1.11.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.1.min.map

#### <a name="jquery-version-1110"></a><span data-ttu-id="03239-226">jQuery-Version 1.11.0</span><span class="sxs-lookup"><span data-stu-id="03239-226">jQuery version 1.11.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.11.0.min.map

#### <a name="jquery-version-1102"></a><span data-ttu-id="03239-227">Version der jQuery 1.10.2</span><span class="sxs-lookup"><span data-stu-id="03239-227">jQuery version 1.10.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.2.min.map

#### <a name="jquery-version-1101"></a><span data-ttu-id="03239-228">jQuery-Version 1.10.1</span><span class="sxs-lookup"><span data-stu-id="03239-228">jQuery version 1.10.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.1.min.map

#### <a name="jquery-version-1100"></a><span data-ttu-id="03239-229">jQuery-Version 1.10.0</span><span class="sxs-lookup"><span data-stu-id="03239-229">jQuery version 1.10.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.10.0.min.map

#### <a name="jquery-version-191"></a><span data-ttu-id="03239-230">jQuery-Version 1.9.1</span><span class="sxs-lookup"><span data-stu-id="03239-230">jQuery version 1.9.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.1.min.map

#### <a name="jquery-version-190"></a><span data-ttu-id="03239-231">jQuery version 1.9.0</span><span class="sxs-lookup"><span data-stu-id="03239-231">jQuery version 1.9.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.9.0.min.map

#### <a name="jquery-version-183"></a><span data-ttu-id="03239-232">jQuery-Version 1.8.3</span><span class="sxs-lookup"><span data-stu-id="03239-232">jQuery version 1.8.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.3-vsdoc.js

#### <a name="jquery-version-182"></a><span data-ttu-id="03239-233">jQuery-Version 1.8.2</span><span class="sxs-lookup"><span data-stu-id="03239-233">jQuery version 1.8.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.2-vsdoc.js

#### <a name="jquery-version-181"></a><span data-ttu-id="03239-234">jQuery-Version 1.8.1</span><span class="sxs-lookup"><span data-stu-id="03239-234">jQuery version 1.8.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.1-vsdoc.js

#### <a name="jquery-version-180"></a><span data-ttu-id="03239-235">jQuery-Version 1.8.0</span><span class="sxs-lookup"><span data-stu-id="03239-235">jQuery version 1.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.8.0-vsdoc.js

#### <a name="jquery-version-172"></a><span data-ttu-id="03239-236">jQuery-Version 1.7.2</span><span class="sxs-lookup"><span data-stu-id="03239-236">jQuery version 1.7.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.2.min.js

#### <a name="jquery-version-171"></a><span data-ttu-id="03239-237">jQuery-Version 1.7.1</span><span class="sxs-lookup"><span data-stu-id="03239-237">jQuery version 1.7.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.1-vsdoc.js

#### <a name="jquery-version-17"></a><span data-ttu-id="03239-238">jQuery-Version 1.7</span><span class="sxs-lookup"><span data-stu-id="03239-238">jQuery version 1.7</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.7-vsdoc.js

#### <a name="jquery-version-164"></a><span data-ttu-id="03239-239">jQuery-Version 1.6.4</span><span class="sxs-lookup"><span data-stu-id="03239-239">jQuery version 1.6.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.4-vsdoc.js

#### <a name="jquery-version-163"></a><span data-ttu-id="03239-240">jQuery-Version 1.6.3</span><span class="sxs-lookup"><span data-stu-id="03239-240">jQuery version 1.6.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.3-vsdoc.js

#### <a name="jquery-version-162"></a><span data-ttu-id="03239-241">jQuery-Version 1.6.2</span><span class="sxs-lookup"><span data-stu-id="03239-241">jQuery version 1.6.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.2-vsdoc.js

#### <a name="jquery-version-161"></a><span data-ttu-id="03239-242">jQuery-Version 1.6.1</span><span class="sxs-lookup"><span data-stu-id="03239-242">jQuery version 1.6.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.1-vsdoc.js

#### <a name="jquery-version-16"></a><span data-ttu-id="03239-243">jQuery-Version 1.6</span><span class="sxs-lookup"><span data-stu-id="03239-243">jQuery version 1.6</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.6-vsdoc.js

#### <a name="jquery-version-152"></a><span data-ttu-id="03239-244">jQuery version 1.5.2</span><span class="sxs-lookup"><span data-stu-id="03239-244">jQuery version 1.5.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.2-vsdoc.js

#### <a name="jquery-version-151"></a><span data-ttu-id="03239-245">jQuery version 1.5.1</span><span class="sxs-lookup"><span data-stu-id="03239-245">jQuery version 1.5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.1-vsdoc.js

#### <a name="jquery-version-15"></a><span data-ttu-id="03239-246">jQuery-Version 1.5</span><span class="sxs-lookup"><span data-stu-id="03239-246">jQuery version 1.5</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.5-vsdoc.js

#### <a name="jquery-version-144"></a><span data-ttu-id="03239-247">jQuery-Version 1.4.4</span><span class="sxs-lookup"><span data-stu-id="03239-247">jQuery version 1.4.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.4-vsdoc.js

#### <a name="jquery-version-143"></a><span data-ttu-id="03239-248">jQuery-Version 1.4.3</span><span class="sxs-lookup"><span data-stu-id="03239-248">jQuery version 1.4.3</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.3-vsdoc.js

#### <a name="jquery-version-142"></a><span data-ttu-id="03239-249">jQuery-Version 1.4.2</span><span class="sxs-lookup"><span data-stu-id="03239-249">jQuery version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.2-vsdoc.js

#### <a name="jquery-version-141"></a><span data-ttu-id="03239-250">jQuery-Version 1.4.1</span><span class="sxs-lookup"><span data-stu-id="03239-250">jQuery version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.1-vsdoc.js

#### <a name="jquery-version-14"></a><span data-ttu-id="03239-251">jQuery-Version 1.4</span><span class="sxs-lookup"><span data-stu-id="03239-251">jQuery version 1.4</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.4.min.js

#### <a name="jquery-version-132"></a><span data-ttu-id="03239-252">Version der jQuery 1.3.2</span><span class="sxs-lookup"><span data-stu-id="03239-252">jQuery version 1.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2-vsdoc.js
- http://ajax.aspnetcdn.com/ajax/jQuery/jquery-1.3.2.min-vsdoc.js

<a id="jQuery_Migrate_Releases_on_the_CDN_1"></a>

### <a name="jquery-migrate-releases-on-the-cdn"></a><span data-ttu-id="03239-253">Migrieren von jQuery-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-253">jQuery Migrate Releases on the CDN</span></span>

<span data-ttu-id="03239-254">Die folgenden Versionen von jQuery migrieren, die auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="03239-254">The following releases of jQuery Migrate are hosted on the CDN:</span></span>

#### <a name="jquery-migrate-version-300"></a><span data-ttu-id="03239-255">jQuery migrieren Version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="03239-255">jQuery Migrate version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-3.0.0.min.js

#### <a name="jquery-migrate-version-121"></a><span data-ttu-id="03239-256">jQuery migrieren Version 1.2.1</span><span class="sxs-lookup"><span data-stu-id="03239-256">jQuery Migrate version 1.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.1.min.js

<span data-ttu-id="03239-257">jQuery migrieren Version 1.2.0</span><span class="sxs-lookup"><span data-stu-id="03239-257">jQuery Migrate version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.2.0.min.js

#### <a name="jquery-migrate-version-111"></a><span data-ttu-id="03239-258">jQuery migrieren Version 1.1.1</span><span class="sxs-lookup"><span data-stu-id="03239-258">jQuery Migrate version 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.1.min.js

#### <a name="jquery-migrate-version-110"></a><span data-ttu-id="03239-259">jQuery migrieren Version 1.1.0</span><span class="sxs-lookup"><span data-stu-id="03239-259">jQuery Migrate version 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.1.0.min.js

#### <a name="jquery-migrate-version-100"></a><span data-ttu-id="03239-260">jQuery migrieren Version 1.0.0</span><span class="sxs-lookup"><span data-stu-id="03239-260">jQuery Migrate version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.js
- http://ajax.aspnetcdn.com/ajax/jquery.migrate/jquery-migrate-1.0.0.min.js

<a id="jQuery_UI_Releases_on_the_CDN_2"></a>

### <a name="jquery-ui-releases-on-the-cdn"></a><span data-ttu-id="03239-261">jQuery UI-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-261">jQuery UI Releases on the CDN</span></span>

<span data-ttu-id="03239-262">Die folgenden Versionen der jQuery UI-Bibliothek, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="03239-262">The following releases of the jQuery UI library are hosted on this CDN.</span></span> <span data-ttu-id="03239-263">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="03239-263">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="03239-264">jQuery UI 1.12.1</span><span class="sxs-lookup"><span data-stu-id="03239-264">jQuery UI 1.12.1</span></span>](jquery-ui/cdnjqueryui1121.md "jQuery UI 1.12.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-265">jQuery UI 1.12.0</span><span class="sxs-lookup"><span data-stu-id="03239-265">jQuery UI 1.12.0</span></span>](jquery-ui/cdnjqueryui1120.md "jQuery UI 1.12.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-266">jQuery UI 1.11.4</span><span class="sxs-lookup"><span data-stu-id="03239-266">jQuery UI 1.11.4</span></span>](jquery-ui/cdnjqueryui1114.md "jQuery UI 1.11.4 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-267">jQuery UI 1.11.3</span><span class="sxs-lookup"><span data-stu-id="03239-267">jQuery UI 1.11.3</span></span>](jquery-ui/cdnjqueryui1113.md "jQuery UI 1.11.3 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-268">jQuery UI 1.11.2</span><span class="sxs-lookup"><span data-stu-id="03239-268">jQuery UI 1.11.2</span></span>](jquery-ui/cdnjqueryui1112.md "jQuery UI 1.11.2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-269">jQuery UI 1.11.1</span><span class="sxs-lookup"><span data-stu-id="03239-269">jQuery UI 1.11.1</span></span>](jquery-ui/cdnjqueryui1111.md "jQuery UI 1.11.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-270">jQuery UI 1.11.0</span><span class="sxs-lookup"><span data-stu-id="03239-270">jQuery UI 1.11.0</span></span>](jquery-ui/cdnjqueryui1110.md "jQuery UI 1.11.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-271">jQuery UI 1.10.4</span><span class="sxs-lookup"><span data-stu-id="03239-271">jQuery UI 1.10.4</span></span>](jquery-ui/cdnjqueryui1104.md "jQuery UI 1.10.4 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-272">jQuery UI 1.10.3</span><span class="sxs-lookup"><span data-stu-id="03239-272">jQuery UI 1.10.3</span></span>](jquery-ui/cdnjqueryui1103.md "jQuery UI 1.10.3 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-273">jQuery UI 1.10.2</span><span class="sxs-lookup"><span data-stu-id="03239-273">jQuery UI 1.10.2</span></span>](jquery-ui/cdnjqueryui1102.md "jQuery 1.10.2-Benutzeroberfläche auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-274">jQuery UI 1.10.1</span><span class="sxs-lookup"><span data-stu-id="03239-274">jQuery UI 1.10.1</span></span>](jquery-ui/cdnjqueryui1101.md "jQuery UI 1.10.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-275">jQuery UI 1.10.0</span><span class="sxs-lookup"><span data-stu-id="03239-275">jQuery UI 1.10.0</span></span>](jquery-ui/cdnjqueryui1100.md "jQuery UI 1.10.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-276">jQuery UI 1.9.2</span><span class="sxs-lookup"><span data-stu-id="03239-276">jQuery UI 1.9.2</span></span>](jquery-ui/cdnjqueryui192.md "jQuery UI 1.9.2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-277">jQuery UI 1.9.1</span><span class="sxs-lookup"><span data-stu-id="03239-277">jQuery UI 1.9.1</span></span>](jquery-ui/cdnjqueryui191.md "jQuery UI 1.9.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-278">jQuery UI 1.9.0</span><span class="sxs-lookup"><span data-stu-id="03239-278">jQuery UI 1.9.0</span></span>](jquery-ui/cdnjqueryui190.md "jQuery UI 1.9.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-279">jQuery UI 1.8.24</span><span class="sxs-lookup"><span data-stu-id="03239-279">jQuery UI 1.8.24</span></span>](jquery-ui/cdnjqueryui1824.md "jQuery UI 1.8.24 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-280">jQuery UI 1.8.23</span><span class="sxs-lookup"><span data-stu-id="03239-280">jQuery UI 1.8.23</span></span>](jquery-ui/cdnjqueryui1823.md "jQuery UI 1.8.23 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-281">jQuery UI 1.8.22</span><span class="sxs-lookup"><span data-stu-id="03239-281">jQuery UI 1.8.22</span></span>](jquery-ui/cdnjqueryui1822.md "jQuery UI 1.8.22 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-282">jQuery UI 1.8.21</span><span class="sxs-lookup"><span data-stu-id="03239-282">jQuery UI 1.8.21</span></span>](jquery-ui/cdnjqueryui1821.md "jQuery UI 1.8.21 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-283">jQuery UI 1.8.20</span><span class="sxs-lookup"><span data-stu-id="03239-283">jQuery UI 1.8.20</span></span>](jquery-ui/cdnjqueryui1820.md "jQuery UI 1.8.20 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-284">jQuery UI 1.8.19</span><span class="sxs-lookup"><span data-stu-id="03239-284">jQuery UI 1.8.19</span></span>](jquery-ui/cdnjqueryui1819.md "jQuery UI 1.8.19 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-285">jQuery UI 1.8.18</span><span class="sxs-lookup"><span data-stu-id="03239-285">jQuery UI 1.8.18</span></span>](jquery-ui/cdnjqueryui1818.md "jQuery UI 1.8.18 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-286">jQuery UI 1.8.17</span><span class="sxs-lookup"><span data-stu-id="03239-286">jQuery UI 1.8.17</span></span>](jquery-ui/cdnjqueryui1817.md "jQuery UI 1.8.17 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-287">jQuery UI 1.8.16</span><span class="sxs-lookup"><span data-stu-id="03239-287">jQuery UI 1.8.16</span></span>](jquery-ui/cdnjqueryui1816.md "jQuery UI 1.8.16 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-288">jQuery UI 1.8.15</span><span class="sxs-lookup"><span data-stu-id="03239-288">jQuery UI 1.8.15</span></span>](jquery-ui/cdnjqueryui1815.md "jQuery UI 1.8.15 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-289">jQuery UI 1.8.14</span><span class="sxs-lookup"><span data-stu-id="03239-289">jQuery UI 1.8.14</span></span>](jquery-ui/cdnjqueryui1814.md "jQuery UI 1.8.14 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-290">jQuery UI 1.8.13</span><span class="sxs-lookup"><span data-stu-id="03239-290">jQuery UI 1.8.13</span></span>](jquery-ui/cdnjqueryui1813.md "jQuery UI 1.8.13 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-291">jQuery UI 1.8.12</span><span class="sxs-lookup"><span data-stu-id="03239-291">jQuery UI 1.8.12</span></span>](jquery-ui/cdnjqueryui1812.md "jQuery UI 1.8.12 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-292">jQuery UI 1.8.11</span><span class="sxs-lookup"><span data-stu-id="03239-292">jQuery UI 1.8.11</span></span>](jquery-ui/cdnjqueryui1811.md "jQuery UI 1.8.11 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-293">jQuery UI 1.8.10</span><span class="sxs-lookup"><span data-stu-id="03239-293">jQuery UI 1.8.10</span></span>](jquery-ui/cdnjqueryui1910.md "jQuery UI 1.8.10 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-294">jQuery UI 1.8.9</span><span class="sxs-lookup"><span data-stu-id="03239-294">jQuery UI 1.8.9</span></span>](jquery-ui/cdnjqueryui189.md "jQuery UI 1.8.9 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-295">jQuery UI 1.8.8</span><span class="sxs-lookup"><span data-stu-id="03239-295">jQuery UI 1.8.8</span></span>](jquery-ui/cdnjqueryui188.md "jQuery UI 1.8.8 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-296">jQuery UI 1.8.7</span><span class="sxs-lookup"><span data-stu-id="03239-296">jQuery UI 1.8.7</span></span>](jquery-ui/cdnjqueryui187.md "jQuery UI 1.8.7 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-297">jQuery UI 1.8.6</span><span class="sxs-lookup"><span data-stu-id="03239-297">jQuery UI 1.8.6</span></span>](jquery-ui/cdnjqueryui186.md "jQuery UI 1.8.6 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-298">jQuery UI 1.8.5</span><span class="sxs-lookup"><span data-stu-id="03239-298">jQuery UI 1.8.5</span></span>](jquery-ui/cdnjqueryui185.md "jQuery UI 1.8.5")

<a id="jQuery_Validation_Releases_on_the_CDN_3"></a>

### <a name="jquery-validation-releases-on-the-cdn"></a><span data-ttu-id="03239-299">jQuery-Validierung Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-299">jQuery Validation Releases on the CDN</span></span>

<span data-ttu-id="03239-300">Die folgenden Versionen der jQuery-Validierung-Bibliothek, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="03239-300">The following releases of the jQuery Validation library are hosted on this CDN.</span></span> <span data-ttu-id="03239-301">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="03239-301">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="03239-302">jQuery Validate 1.17.0</span><span class="sxs-lookup"><span data-stu-id="03239-302">jQuery Validate 1.17.0</span></span>](jquery-validate/cdnjqueryvalidate1170.md "jQuery Validation 1.17.0")
- [<span data-ttu-id="03239-303">jQuery Validate 1.16.0</span><span class="sxs-lookup"><span data-stu-id="03239-303">jQuery Validate 1.16.0</span></span>](jquery-validate/cdnjqueryvalidate1160.md "jQuery Validation 1.16.0")
- [<span data-ttu-id="03239-304">jQuery Validate 1.15.1</span><span class="sxs-lookup"><span data-stu-id="03239-304">jQuery Validate 1.15.1</span></span>](jquery-validate/cdnjqueryvalidate1151.md "jQuery Validation 1.15.1")
- [<span data-ttu-id="03239-305">jQuery Validate 1.15.0</span><span class="sxs-lookup"><span data-stu-id="03239-305">jQuery Validate 1.15.0</span></span>](jquery-validate/cdnjqueryvalidate1150.md "jQuery Validation 1.15.0")
- [<span data-ttu-id="03239-306">jQuery Validate 1.14.0</span><span class="sxs-lookup"><span data-stu-id="03239-306">jQuery Validate 1.14.0</span></span>](jquery-validate/cdnjqueryvalidate1140.md "jQuery Validation 1.14.0")
- [<span data-ttu-id="03239-307">jQuery Validate 1.13.1</span><span class="sxs-lookup"><span data-stu-id="03239-307">jQuery Validate 1.13.1</span></span>](jquery-validate/cdnjqueryvalidate1131.md "jQuery Validation 1.13.1")
- [<span data-ttu-id="03239-308">jQuery Validate 1.13.0</span><span class="sxs-lookup"><span data-stu-id="03239-308">jQuery Validate 1.13.0</span></span>](jquery-validate/cdnjqueryvalidate1130.md "jQuery Validation 1.13.0")
- [<span data-ttu-id="03239-309">jQuery Validate 1.12.0</span><span class="sxs-lookup"><span data-stu-id="03239-309">jQuery Validate 1.12.0</span></span>](jquery-validate/cdnjqueryvalidate1120.md "jQuery Validation 1.12.0")
- [<span data-ttu-id="03239-310">jQuery Validate 1.11.1</span><span class="sxs-lookup"><span data-stu-id="03239-310">jQuery Validate 1.11.1</span></span>](jquery-validate/cdnjqueryvalidate1111.md "jQuery Validation 1.11.1")
- [<span data-ttu-id="03239-311">jQuery Validate 1.11.0</span><span class="sxs-lookup"><span data-stu-id="03239-311">jQuery Validate 1.11.0</span></span>](jquery-validate/cdnjqueryvalidate111.md "jQuery Validation 1.11.0")
- [<span data-ttu-id="03239-312">jQuery Validate 1.10.0</span><span class="sxs-lookup"><span data-stu-id="03239-312">jQuery Validate 1.10.0</span></span>](jquery-validate/cdnjqueryvalidate110.md "jQuery Validation 1.10.0")
- [<span data-ttu-id="03239-313">jQuery Validate 1.9</span><span class="sxs-lookup"><span data-stu-id="03239-313">jQuery Validate 1.9</span></span>](jquery-validate/cdnjqueryvalidate19.md "jquery.validate version 1.9")
- [<span data-ttu-id="03239-314">jQuery Validate 1.8.1</span><span class="sxs-lookup"><span data-stu-id="03239-314">jQuery Validate 1.8.1</span></span>](jquery-validate/cdnjqueryvalidate181.md "jquery.validate version 1.8.1")
- [<span data-ttu-id="03239-315">jQuery Validate 1.8</span><span class="sxs-lookup"><span data-stu-id="03239-315">jQuery Validate 1.8</span></span>](jquery-validate/cdnjqueryvalidate18.md "jquery.validate version 1.8")
- [<span data-ttu-id="03239-316">jQuery Validate 1.7</span><span class="sxs-lookup"><span data-stu-id="03239-316">jQuery Validate 1.7</span></span>](jquery-validate/cdnjqueryvalidate17.md "jquery.validate version 1.7")
- [<span data-ttu-id="03239-317">jQuery Validate 1.6</span><span class="sxs-lookup"><span data-stu-id="03239-317">jQuery Validate 1.6</span></span>](jquery-validate/cdnjqueryvalidate16.md "jQuery Validate 1.6")
- [<span data-ttu-id="03239-318">jQuery Validate 1.5.5</span><span class="sxs-lookup"><span data-stu-id="03239-318">jQuery Validate 1.5.5</span></span>](jquery-validate/cdnjqueryvalidate155.md "jQuery Validate 1.5.5")

<a id="jQuery_Mobile_Releases_on_the_CDN_4"></a>

### <a name="jquery-mobile-releases-on-the-cdn"></a><span data-ttu-id="03239-319">jQuery Mobile-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-319">jQuery Mobile Releases on the CDN</span></span>

<span data-ttu-id="03239-320">Die folgenden Versionen der jQuery Mobile-Bibliothek, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="03239-320">The following releases of the jQuery Mobile library are hosted on this CDN.</span></span> <span data-ttu-id="03239-321">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="03239-321">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="03239-322">jQuery Mobile 1.4.5</span><span class="sxs-lookup"><span data-stu-id="03239-322">jQuery Mobile 1.4.5</span></span>](jquery-mobile/cdnjquerymobile145.md "jQuery Mobile 1.4.5 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-323">jQuery Mobile 1.4.2</span><span class="sxs-lookup"><span data-stu-id="03239-323">jQuery Mobile 1.4.2</span></span>](jquery-mobile/cdnjquerymobile142.md "jQuery Mobile 1.4.2 on the Microsoft Ajax CDN")
- [<span data-ttu-id="03239-324">jQuery Mobile 1.4.1</span><span class="sxs-lookup"><span data-stu-id="03239-324">jQuery Mobile 1.4.1</span></span>](jquery-mobile/cdnjquerymobile141.md "jQuery Mobile 1.4.1 on the Microsoft Ajax CDN")
- [<span data-ttu-id="03239-325">jQuery Mobile 1.4.0</span><span class="sxs-lookup"><span data-stu-id="03239-325">jQuery Mobile 1.4.0</span></span>](jquery-mobile/cdnjquerymobile140.md "jQuery Mobile 1.4.0 on the Microsoft Ajax CDN")
- [<span data-ttu-id="03239-326">jQuery Mobile 1.3.2</span><span class="sxs-lookup"><span data-stu-id="03239-326">jQuery Mobile 1.3.2</span></span>](jquery-mobile/cdnjquerymobile132.md "jQuery Mobile 1.3.2 on the Microsoft Ajax CDN")
- [<span data-ttu-id="03239-327">jQuery Mobile 1.3.1</span><span class="sxs-lookup"><span data-stu-id="03239-327">jQuery Mobile 1.3.1</span></span>](jquery-mobile/cdnjquerymobile131.md "jQuery Mobile 1.3.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-328">jQuery Mobile 1.3.0</span><span class="sxs-lookup"><span data-stu-id="03239-328">jQuery Mobile 1.3.0</span></span>](jquery-mobile/cdnjquerymobile130.md "jQuery Mobile 1.3.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-329">jQuery Mobile 1.2.0</span><span class="sxs-lookup"><span data-stu-id="03239-329">jQuery Mobile 1.2.0</span></span>](jquery-mobile/cdnjquerymobile120.md "jQuery Mobile 1.2.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-330">jQuery Mobile 1.1.2</span><span class="sxs-lookup"><span data-stu-id="03239-330">jQuery Mobile 1.1.2</span></span>](jquery-mobile/cdnjquerymobile112.md "jQuery Mobile 1.1.2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-331">jQuery Mobile 1.1.1</span><span class="sxs-lookup"><span data-stu-id="03239-331">jQuery Mobile 1.1.1</span></span>](jquery-mobile/cdnjquerymobile111.md "jQuery Mobile 1.1.1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-332">jQuery Mobile 1.1.0</span><span class="sxs-lookup"><span data-stu-id="03239-332">jQuery Mobile 1.1.0</span></span>](jquery-mobile/cdnjquerymobile110.md "jQuery Mobile 1.1.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-333">jQuery Mobile 1.1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="03239-333">jQuery Mobile 1.1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile110rc2.md "jQuery Mobile 1.1.0 RC2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-334">jQuery Mobile 1.0.1</span><span class="sxs-lookup"><span data-stu-id="03239-334">jQuery Mobile 1.0.1</span></span>](jquery-mobile/cdnjquerymobile101.md "jQuery Mobile 1.0.1 on the Microsoft Ajax CDN")
- [<span data-ttu-id="03239-335">jQuery Mobile 1.0</span><span class="sxs-lookup"><span data-stu-id="03239-335">jQuery Mobile 1.0</span></span>](jquery-mobile/cdnjquerymobile10.md "jQuery Mobile 1.0 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-336">jQuery Mobile 1.0 RC 2</span><span class="sxs-lookup"><span data-stu-id="03239-336">jQuery Mobile 1.0 RC 2</span></span>](jquery-mobile/cdnjquerymobile10rc2.md "jQuery Mobile 1.0 RC2 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-337">jQuery Mobile 1.0 RC 1</span><span class="sxs-lookup"><span data-stu-id="03239-337">jQuery Mobile 1.0 RC 1</span></span>](jquery-mobile/cdnjquerymobile10rc1.md "jQuery Mobile 1.0 RC1 auf das Microsoft Ajax-CDN")
- [<span data-ttu-id="03239-338">jQuery Mobile 1.0 Beta 3</span><span class="sxs-lookup"><span data-stu-id="03239-338">jQuery Mobile 1.0 beta 3</span></span>](jquery-mobile/cdnjquerymobile10b3.md "jQuery Mobile 1.0 Beta 3 Klicken Sie auf das Microsoft Ajax-CDN")

<a id="jQuery_Templates_Releases_on_the_CDN_5"></a>

### <a name="jquery-templates-releases-on-the-cdn"></a><span data-ttu-id="03239-339">jQuery-Vorlagen-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-339">jQuery Templates Releases on the CDN</span></span>

<span data-ttu-id="03239-340">Die folgenden Versionen der jQuery-Vorlagen-Plug-Ins, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="03239-340">The following releases of the jQuery Templates plugin are hosted on this CDN.</span></span> <span data-ttu-id="03239-341">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="03239-341">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="03239-342">jQuery-Vorlagen Beta 1</span><span class="sxs-lookup"><span data-stu-id="03239-342">jQuery Templates Beta 1</span></span>](jquery-templates/cdnjquerytemplatesbeta1.md "jQuery-Vorlagen Beta 1")

<a id="jQuery_Cycle_Releases_on_the_CDN_6"></a>

### <a name="jquery-cycle-releases-on-the-cdn"></a><span data-ttu-id="03239-343">jQuery-Zyklus Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-343">jQuery Cycle Releases on the CDN</span></span>

<span data-ttu-id="03239-344">Die folgenden Versionen der jQuery-Zyklus-Plug-Ins, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="03239-344">The following releases of the jQuery Cycle plugin are hosted on this CDN.</span></span> <span data-ttu-id="03239-345">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="03239-345">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="03239-346">jQuery-Zyklus 2,99</span><span class="sxs-lookup"><span data-stu-id="03239-346">jQuery Cycle 2.99</span></span>](jquery-cycle/cdnjquerycycle299.md "jQuery Zyklus 2,99")
- [<span data-ttu-id="03239-347">jQuery-Zyklus 2.94</span><span class="sxs-lookup"><span data-stu-id="03239-347">jQuery Cycle 2.94</span></span>](jquery-cycle/cdnjquerycycle294.md "jQuery Zyklus 2.94")
- [<span data-ttu-id="03239-348">jQuery-Zyklus 2,88</span><span class="sxs-lookup"><span data-stu-id="03239-348">jQuery Cycle 2.88</span></span>](jquery-cycle/cdnjquerycycle288.md "jQuery Zyklus 2,88")

<a id="jQuery_DataTables_Releases_on_the_CDN_7"></a>

### <a name="jquery-datatables-releases-on-the-cdn"></a><span data-ttu-id="03239-349">jQuery DataTables Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-349">jQuery DataTables Releases on the CDN</span></span>

<span data-ttu-id="03239-350">Die folgenden Versionen der jQuery DataTables-Plug-Ins, die auf diesem CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="03239-350">The following releases of the jQuery DataTables plugin are hosted on this CDN.</span></span> <span data-ttu-id="03239-351">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="03239-351">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="03239-352">jQuery DataTables 1.10.5</span><span class="sxs-lookup"><span data-stu-id="03239-352">jQuery DataTables 1.10.5</span></span>](jquery-datatables/cdnjquerydatatables105.md "jQuery DataTables 1.10.5")
- [<span data-ttu-id="03239-353">jQuery DataTables 1.10.4</span><span class="sxs-lookup"><span data-stu-id="03239-353">jQuery DataTables 1.10.4</span></span>](jquery-datatables/cdnjquerydatatables104.md "jQuery DataTables 1.10.4")
- [<span data-ttu-id="03239-354">jQuery DataTables 1.9.4</span><span class="sxs-lookup"><span data-stu-id="03239-354">jQuery DataTables 1.9.4</span></span>](jquery-datatables/cdnjquerydatatables194.md "jQuery DataTables 1.9.4")
- [<span data-ttu-id="03239-355">jQuery DataTables 1.9.3</span><span class="sxs-lookup"><span data-stu-id="03239-355">jQuery DataTables 1.9.3</span></span>](jquery-datatables/cdnjquerydatatables193.md "jQuery DataTables 1.9.3")
- [<span data-ttu-id="03239-356">jQuery DataTables 1.9.2</span><span class="sxs-lookup"><span data-stu-id="03239-356">jQuery DataTables 1.9.2</span></span>](jquery-datatables/cdnjquerydatatables192.md "jQuery DataTables 1.9.2")
- [<span data-ttu-id="03239-357">jQuery DataTables 1.9.1</span><span class="sxs-lookup"><span data-stu-id="03239-357">jQuery DataTables 1.9.1</span></span>](jquery-datatables/cdnjquerydatatables191.md "jQuery DataTables 1.9.1")
- [<span data-ttu-id="03239-358">jQuery DataTables 1.9.0</span><span class="sxs-lookup"><span data-stu-id="03239-358">jQuery DataTables 1.9.0</span></span>](jquery-datatables/cdnjquerydatatables190.md "jQuery DataTables 1.9.0")
- [<span data-ttu-id="03239-359">jQuery DataTables 1.8.2</span><span class="sxs-lookup"><span data-stu-id="03239-359">jQuery DataTables 1.8.2</span></span>](jquery-datatables/cdnjquerydatatables182.md "jQuery DataTables 1.8.2")

<a id="Modernizr_Releases_on_the_CDN_8"></a>

### <a name="modernizr-releases-on-the-cdn"></a><span data-ttu-id="03239-360">Modernizr Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-360">Modernizr Releases on the CDN</span></span>

<span data-ttu-id="03239-361">Die folgenden Versionen von [Modernizr](http://www.modernizr.com "Modernizr") auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="03239-361">The following releases of [Modernizr](http://www.modernizr.com "Modernizr") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.8.3.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.7.1.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.6.2.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-1.7-development-only.js
- http://ajax.aspnetcdn.com/ajax/modernizr/modernizr-2.0.6-development-only.js

<a id="JSHint_Releases_on_the_CDN_10"></a>

### <a name="jshint-releases-on-the-cdn"></a><span data-ttu-id="03239-362">JSHint Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-362">JSHint Releases on the CDN</span></span>

<span data-ttu-id="03239-363">Die folgenden Versionen von [JSHint](http://www.jshint.com "JSHint") auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="03239-363">The following releases of [JSHint](http://www.jshint.com "JSHint") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/jshint/r07/jshint.js

<a id="Knockout_Releases_on_the_CDN_11"></a>

### <a name="knockout-releases-on-the-cdn"></a><span data-ttu-id="03239-364">Knockout Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-364">Knockout Releases on the CDN</span></span>

<span data-ttu-id="03239-365">Die folgenden Versionen von [Knockout](http://www.knockoutjs.com "Knockout") auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="03239-365">The following releases of [Knockout](http://www.knockoutjs.com "Knockout") are hosted on the CDN:</span></span>

- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-2.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.0.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.1.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.2.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.3.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.0.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.1.debug.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.js
- http://ajax.aspnetcdn.com/ajax/knockout/knockout-3.4.2.debug.js

<a id="Globalize_Releases_on_the_CDN_12"></a>

### <a name="globalize-releases-on-the-cdn"></a><span data-ttu-id="03239-366">Bei der Globalisierung Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-366">Globalize Releases on the CDN</span></span>

<span data-ttu-id="03239-367">Die folgenden Versionen von [Globalize](https://github.com/jquery/globalize "Globalize") auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="03239-367">The following releases of [Globalize](https://github.com/jquery/globalize "Globalize") are hosted on the CDN:</span></span>

#### <a name="globalize-version-100"></a><span data-ttu-id="03239-368">Globalize version 1.0.0</span><span class="sxs-lookup"><span data-stu-id="03239-368">Globalize version 1.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/node-main.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/currency.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/date.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/message.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/number.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/plural.js
- http://ajax.aspnetcdn.com/ajax/globalize/1.0.0/globalize/relative-time.js

#### <a name="globalize-version-011"></a><span data-ttu-id="03239-369">Bei der Globalisierung Version 0.1.1</span><span class="sxs-lookup"><span data-stu-id="03239-369">Globalize version 0.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.min.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/globalize.js
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.cultures.js

    - <span data-ttu-id="03239-370">alle Kulturen</span><span class="sxs-lookup"><span data-stu-id="03239-370">all cultures</span></span>
- http://ajax.aspnetcdn.com/ajax/globalize/0.1.1/cultures/globalize.culture.{culture-code}.js

    - <span data-ttu-id="03239-371">Ersetzen Sie "{Kulturcode}" durch den gewünschten Kulturcode, z. B. Microsoft globalize.culture.en GB.js== Dateien auf das CDN == diese Bibliotheken von Microsoft hochgeladen wurden.</span><span class="sxs-lookup"><span data-stu-id="03239-371">Replace "{culture-code}" with the desired culture code, e.g. globalize.culture.en-GB.js== Microsoft Files on the CDN ==These libraries were uploaded by Microsoft.</span></span>

<a id="Respond_Releases_on_the_CDN_13"></a>

### <a name="respond-releases-on-the-cdn"></a><span data-ttu-id="03239-372">Reagieren Sie auf das CDN Versionen</span><span class="sxs-lookup"><span data-stu-id="03239-372">Respond Releases on the CDN</span></span>

<span data-ttu-id="03239-373">Die folgenden Versionen von [ https://github.com/scottjehl/Respond ] (https://github.com/scottjehl/Respond " https://github.com/scottjehl/Respond ") reagieren auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="03239-373">The following releases of [https://github.com/scottjehl/Respond](https://github.com/scottjehl/Respond "https://github.com/scottjehl/Respond") Respond are hosted on the CDN:</span></span>

#### <a name="respond-version-142"></a><span data-ttu-id="03239-374">Version 1.4.2 reagieren</span><span class="sxs-lookup"><span data-stu-id="03239-374">Respond version 1.4.2</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.2/respond.matchmedia.addListener.min.js

#### <a name="respond-version-141"></a><span data-ttu-id="03239-375">Version 1.4.1 reagieren</span><span class="sxs-lookup"><span data-stu-id="03239-375">Respond version 1.4.1</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.1/respond.matchmedia.addListener.min.js

#### <a name="respond-version-140"></a><span data-ttu-id="03239-376">Version 1.4.0 reagieren</span><span class="sxs-lookup"><span data-stu-id="03239-376">Respond version 1.4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.min.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.js
- http://ajax.aspnetcdn.com/ajax/respond/1.4.0/respond.matchmedia.addListener.min.js

#### <a name="respond-version-130"></a><span data-ttu-id="03239-377">Version 1.3.0 reagieren</span><span class="sxs-lookup"><span data-stu-id="03239-377">Respond version 1.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.3.0/respond.js

#### <a name="respond-version-120"></a><span data-ttu-id="03239-378">Die Version 1.2.0 reagieren</span><span class="sxs-lookup"><span data-stu-id="03239-378">Respond version 1.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/respond/1.2.0/respond.js

<a id="Bootstrap_Releases_on_the_CDN_14"></a>

### <a name="bootstrap-releases-on-the-cdn"></a><span data-ttu-id="03239-379">Bootstrap-Versionen auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-379">Bootstrap Releases on the CDN</span></span>

<span data-ttu-id="03239-380">Die folgenden Versionen von [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") Bootstrap auf dem CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="03239-380">The following releases of [getbootstrap.com](http://getbootstrap.com "getbootstrap.com") bootstrap are hosted on the CDN:</span></span>

#### <a name="bootstrap-version-400"></a><span data-ttu-id="03239-381">Bootstrap Version 4.0.0</span><span class="sxs-lookup"><span data-stu-id="03239-381">Bootstrap version 4.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/4.0.0/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-337"></a><span data-ttu-id="03239-382">Bootstrap Version 3.3.7</span><span class="sxs-lookup"><span data-stu-id="03239-382">Bootstrap version 3.3.7</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.7/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-336"></a><span data-ttu-id="03239-383">Bootstrap Version 3.3.6</span><span class="sxs-lookup"><span data-stu-id="03239-383">Bootstrap version 3.3.6</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.6/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-335"></a><span data-ttu-id="03239-384">Bootstrap Version 3.3.5</span><span class="sxs-lookup"><span data-stu-id="03239-384">Bootstrap version 3.3.5</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.5/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-334"></a><span data-ttu-id="03239-385">Bootstrap Version 3.3.4</span><span class="sxs-lookup"><span data-stu-id="03239-385">Bootstrap version 3.3.4</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.4/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-332"></a><span data-ttu-id="03239-386">Bootstrap Version 3.3.2</span><span class="sxs-lookup"><span data-stu-id="03239-386">Bootstrap version 3.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.2/fonts/glyphicons-halflings-regular.woff2

#### <a name="bootstrap-version-331"></a><span data-ttu-id="03239-387">Bootstrap Version 3.3.1</span><span class="sxs-lookup"><span data-stu-id="03239-387">Bootstrap version 3.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-330"></a><span data-ttu-id="03239-388">Bootstrap Version 3.3.0</span><span class="sxs-lookup"><span data-stu-id="03239-388">Bootstrap version 3.3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.3.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-320"></a><span data-ttu-id="03239-389">Bootstrap Version 3.2.0</span><span class="sxs-lookup"><span data-stu-id="03239-389">Bootstrap version 3.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.2.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-311"></a><span data-ttu-id="03239-390">Bootstrap Version 3.1.1</span><span class="sxs-lookup"><span data-stu-id="03239-390">Bootstrap version 3.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-310"></a><span data-ttu-id="03239-391">Bootstrap Version 3.1.0</span><span class="sxs-lookup"><span data-stu-id="03239-391">Bootstrap version 3.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.css.map
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.1.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-303"></a><span data-ttu-id="03239-392">Bootstrap Version 3.0.3.</span><span class="sxs-lookup"><span data-stu-id="03239-392">Bootstrap version 3.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.3/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-302"></a><span data-ttu-id="03239-393">Bootstrap Version 3.0.2</span><span class="sxs-lookup"><span data-stu-id="03239-393">Bootstrap version 3.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.2/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-301"></a><span data-ttu-id="03239-394">Bootstrap Version 3.0.1</span><span class="sxs-lookup"><span data-stu-id="03239-394">Bootstrap version 3.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.1/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-300"></a><span data-ttu-id="03239-395">Bootstrap-Version 3.0.0</span><span class="sxs-lookup"><span data-stu-id="03239-395">Bootstrap version 3.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/css/bootstrap-theme.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.eot
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.svg
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.ttf
- http://ajax.aspnetcdn.com/ajax/bootstrap/3.0.0/fonts/glyphicons-halflings-regular.woff

#### <a name="bootstrap-version-232"></a><span data-ttu-id="03239-396">Bootstrap-Version 2.3.2</span><span class="sxs-lookup"><span data-stu-id="03239-396">Bootstrap version 2.3.2</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.2/img/glyphicons-halflings-white.png

#### <a name="bootstrap-version-231"></a><span data-ttu-id="03239-397">Bootstrap Version 2.3.1</span><span class="sxs-lookup"><span data-stu-id="03239-397">Bootstrap version 2.3.1</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/bootstrap.min.js
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/css/bootstrap-responsive.min.css
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings.png
- http://ajax.aspnetcdn.com/ajax/bootstrap/2.3.1/img/glyphicons-halflings-white.png

<a id="BootstrapTouchCarousel_Releases_on_the_CDN_18"></a>

### <a name="bootstrap-touchcarousel-releases-on-the-cdn"></a><span data-ttu-id="03239-398">Bootstrap TouchCarousel Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-398">Bootstrap TouchCarousel Releases on the CDN</span></span>

<span data-ttu-id="03239-399">Die folgenden Versionen von [ https://github.com/ixisio/bootstrap-touch-carousel ] (https://github.com/ixisio/bootstrap-touch-carousel " https://github.com/ixisio/bootstrap-touch-carousel ") Bootstrap TouchCarousel Versionen auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="03239-399">The following releases of [https://github.com/ixisio/bootstrap-touch-carousel](https://github.com/ixisio/bootstrap-touch-carousel "https://github.com/ixisio/bootstrap-touch-carousel") Bootstrap TouchCarousel releases are hosted on the CDN:</span></span>

#### <a name="bootstrap-touchcarousel-version-080"></a><span data-ttu-id="03239-400">Bootstrap TouchCarousel Version 0.8.0</span><span class="sxs-lookup"><span data-stu-id="03239-400">Bootstrap TouchCarousel version 0.8.0</span></span>

- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/css/bootstrap-touch-carousel.css
- http://ajax.aspnetcdn.com/ajax/bootstrap-touch-carousel/0.8.0/js/bootstrap-touch-carousel.js

<a id="Hammerjs_Releases_on_the_CDN_19"></a>

### <a name="hammerjs-releases-on-the-cdn"></a><span data-ttu-id="03239-401">Hammer.js Releases auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-401">Hammer.js Releases on the CDN</span></span>

<span data-ttu-id="03239-402">Die folgenden Versionen von [ http://hammerjs.github.io/ ] (http://hammerjs.github.io/ " http://hammerjs.github.io/ ") Hammer.js Versionen auf das CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="03239-402">The following releases of [http://hammerjs.github.io/](http://hammerjs.github.io/ "http://hammerjs.github.io/") Hammer.js releases are hosted on the CDN:</span></span>

#### <a name="hammerjs-version-204"></a><span data-ttu-id="03239-403">Hammer.js Version 2.0.4</span><span class="sxs-lookup"><span data-stu-id="03239-403">Hammer.js version 2.0.4</span></span>

- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.js
- http://ajax.aspnetcdn.com/ajax/hammer.js/2.0.4/hammer.min.map

<a id="ASPNET_Web_Forms_and_Ajax_Releases_on_the_CDN_15"></a>

### <a name="aspnet-web-forms-and-ajax-releases-on-the-cdn"></a><span data-ttu-id="03239-404">ASP.NET Web Forms und Ajax-Versionen, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-404">ASP.NET Web Forms and Ajax Releases on the CDN</span></span>

<span data-ttu-id="03239-405">Die folgenden Versionen von ASP.NET Ajax-Bibliothek, die auf das CDN gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="03239-405">The following releases of the ASP.NET Ajax Library are hosted on the CDN.</span></span> <span data-ttu-id="03239-406">Klicken Sie auf jeder Link, um die tatsächliche Liste von Dateien anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="03239-406">Click each link to see the actual list of files.</span></span>

- [<span data-ttu-id="03239-407">ASP.NET Web Forms- und Ajax-Version 4.5.2</span><span class="sxs-lookup"><span data-stu-id="03239-407">ASP.NET Web Forms and Ajax version 4.5.2</span></span>](cdnajax452.md "ASP.NET Web Forms- und Ajax 4.5.2")
- [<span data-ttu-id="03239-408">ASP.NET Web Forms- und Ajax-Version 4</span><span class="sxs-lookup"><span data-stu-id="03239-408">ASP.NET Web Forms and Ajax version 4</span></span>](cdnajax4.md "ASP.NET Web Forms- und Ajax 4")
- [<span data-ttu-id="03239-409">ASP.NET Ajax Version 3.5</span><span class="sxs-lookup"><span data-stu-id="03239-409">ASP.NET Ajax version 3.5</span></span>](cdnajax35.md "ASP.NET Ajax 3.5")

<a id="ASPNET_MVC_Releases_on_the_CDN_16"></a>

### <a name="aspnet-mvc-releases-on-the-cdn"></a><span data-ttu-id="03239-410">ASP.NET MVC frei, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-410">ASP.NET MVC Releases on the CDN</span></span>

<span data-ttu-id="03239-411">Die folgenden ASP.NET MVC-JavaScript-Dateien, die auf diesem CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="03239-411">The following ASP.NET MVC JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-mvc-523"></a><span data-ttu-id="03239-412">ASP.NET MVC 5.2.3</span><span class="sxs-lookup"><span data-stu-id="03239-412">ASP.NET MVC 5.2.3</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.2.3/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-51"></a><span data-ttu-id="03239-413">ASP.NET MVC 5.1</span><span class="sxs-lookup"><span data-stu-id="03239-413">ASP.NET MVC 5.1</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.1/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-50"></a><span data-ttu-id="03239-414">ASP.NET MVC 5.0</span><span class="sxs-lookup"><span data-stu-id="03239-414">ASP.NET MVC 5.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/5.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-40"></a><span data-ttu-id="03239-415">ASP.NET MVC 4.0</span><span class="sxs-lookup"><span data-stu-id="03239-415">ASP.NET MVC 4.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/4.0/jquery.validate.unobtrusive.min.js

#### <a name="aspnet-mvc-30"></a><span data-ttu-id="03239-416">ASP.NET MVC 3.0</span><span class="sxs-lookup"><span data-stu-id="03239-416">ASP.NET MVC 3.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.unobtrusive-ajax.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/jquery.validate.unobtrusive.min.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/3.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-20"></a><span data-ttu-id="03239-417">ASP.NET MVC 2.0</span><span class="sxs-lookup"><span data-stu-id="03239-417">ASP.NET MVC 2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/2.0/MicrosoftMvcAjax.debug.js

#### <a name="aspnet-mvc-10"></a><span data-ttu-id="03239-418">ASP.NET MVC 1.0</span><span class="sxs-lookup"><span data-stu-id="03239-418">ASP.NET MVC 1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.js
- http://ajax.aspnetcdn.com/ajax/mvc/1.0/MicrosoftMvcAjax.debug.js

<a id="ASPNET_SignalR_Releases_on_the_CDN_17"></a>

### <a name="aspnet-signalr-releases-on-the-cdn"></a><span data-ttu-id="03239-419">ASP.NET SignalR frei, auf das CDN</span><span class="sxs-lookup"><span data-stu-id="03239-419">ASP.NET SignalR Releases on the CDN</span></span>

<span data-ttu-id="03239-420">Die folgenden ASP.NET SignalR JavaScript-Dateien, die auf diesem CDN gehostet werden:</span><span class="sxs-lookup"><span data-stu-id="03239-420">The following ASP.NET SignalR JavaScript files are hosted on this CDN:</span></span>

#### <a name="aspnet-signalr-222"></a><span data-ttu-id="03239-421">ASP.NET SignalR 2.2.2</span><span class="sxs-lookup"><span data-stu-id="03239-421">ASP.NET SignalR 2.2.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.2.js

#### <a name="aspnet-signalr-221"></a><span data-ttu-id="03239-422">ASP.NET SignalR 2.2.1</span><span class="sxs-lookup"><span data-stu-id="03239-422">ASP.NET SignalR 2.2.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.1.js

#### <a name="aspnet-signalr-220"></a><span data-ttu-id="03239-423">ASP.NET SignalR 2.2.0</span><span class="sxs-lookup"><span data-stu-id="03239-423">ASP.NET SignalR 2.2.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.2.0.js

#### <a name="aspnet-signalr-210"></a><span data-ttu-id="03239-424">ASP.NET SignalR 2.1.0</span><span class="sxs-lookup"><span data-stu-id="03239-424">ASP.NET SignalR 2.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.1.0.js

#### <a name="aspnet-signalr-203"></a><span data-ttu-id="03239-425">ASP.NET SignalR 2.0.3</span><span class="sxs-lookup"><span data-stu-id="03239-425">ASP.NET SignalR 2.0.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.3.js

#### <a name="aspnet-signalr-202"></a><span data-ttu-id="03239-426">ASP.NET SignalR 2.0.2</span><span class="sxs-lookup"><span data-stu-id="03239-426">ASP.NET SignalR 2.0.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.2.js

#### <a name="aspnet-signalr-201"></a><span data-ttu-id="03239-427">ASP.NET SignalR 2.0.1</span><span class="sxs-lookup"><span data-stu-id="03239-427">ASP.NET SignalR 2.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.1.js

#### <a name="aspnet-signalr-200"></a><span data-ttu-id="03239-428">ASP.NET SignalR 2.0.0</span><span class="sxs-lookup"><span data-stu-id="03239-428">ASP.NET SignalR 2.0.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-2.0.0.js

#### <a name="aspnet-signalr-113"></a><span data-ttu-id="03239-429">ASP.NET SignalR 1.1.3</span><span class="sxs-lookup"><span data-stu-id="03239-429">ASP.NET SignalR 1.1.3</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.3.js

#### <a name="aspnet-signalr-112"></a><span data-ttu-id="03239-430">ASP.NET SignalR 1.1.2</span><span class="sxs-lookup"><span data-stu-id="03239-430">ASP.NET SignalR 1.1.2</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.2.js

#### <a name="aspnet-signalr-111"></a><span data-ttu-id="03239-431">ASP.NET SignalR 1.1.1</span><span class="sxs-lookup"><span data-stu-id="03239-431">ASP.NET SignalR 1.1.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.1.js

#### <a name="aspnet-signalr-110"></a><span data-ttu-id="03239-432">ASP.NET SignalR 1.1.0</span><span class="sxs-lookup"><span data-stu-id="03239-432">ASP.NET SignalR 1.1.0</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.1.0.js

#### <a name="aspnet-signalr-101"></a><span data-ttu-id="03239-433">ASP.NET SignalR 1.0.1</span><span class="sxs-lookup"><span data-stu-id="03239-433">ASP.NET SignalR 1.0.1</span></span>

- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.min.js
- http://ajax.aspnetcdn.com/ajax/signalr/jquery.signalr-1.0.1.js

<span data-ttu-id="03239-434">Weitere Informationen zu den Nutzungsbedingungen für das CDN, finden Sie unter [Microsoft Ajax-CDN Nutzungsbedingungen](https://www.asp.net/terms-of-use "Microsoft Ajax-CDN Nutzungsbedingungen").</span><span class="sxs-lookup"><span data-stu-id="03239-434">For information about the terms of use for the CDN, see [Microsoft Ajax CDN Terms of Use](https://www.asp.net/terms-of-use "Microsoft Ajax CDN Terms of Use").</span></span>
