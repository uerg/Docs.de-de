---
uid: aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
title: "Auszuführende Aktion nicht in ASP.NET und wie stattdessen vorzugehen ist | Microsoft Docs"
author: tfitzmac
description: "Dieses Thema beschreibt einige häufige Fehler, die Personen innerhalb von ASP.NET-Webprojekten vornehmen. Empfehlungen für was Sie tun sollten, um diese Commo vermeiden..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/08/2014
ms.topic: article
ms.assetid: c39b9965-545c-4b04-8f55-21be7f28a9e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/web-development-best-practices/what-not-to-do-in-aspnet-and-what-to-do-instead
msc.type: authoredcontent
ms.openlocfilehash: 6790cd0deb36c9fb297ccd4df371f763dba17844
ms.sourcegitcommit: 17b025bd33f4474f0deaafc6d0447a4e72bcad87
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2017
---
<a name="what-not-to-do-in-aspnet-and-what-to-do-instead"></a><span data-ttu-id="e5007-104">Auszuführende Aktion nicht in ASP.NET und wie stattdessen vorzugehen ist</span><span class="sxs-lookup"><span data-stu-id="e5007-104">What not to do in ASP.NET, and what to do instead</span></span>
====================
<span data-ttu-id="e5007-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e5007-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e5007-106">Dieses Thema beschreibt einige häufige Fehler, die Personen innerhalb von ASP.NET-Webprojekten vornehmen.</span><span class="sxs-lookup"><span data-stu-id="e5007-106">This topic describes several common mistakes people make within ASP.NET web projects.</span></span> <span data-ttu-id="e5007-107">Empfehlungen für was Sie tun müssen, um diese häufige Fehler zu vermeiden.</span><span class="sxs-lookup"><span data-stu-id="e5007-107">It provides recommendations for what you should do to avoid these common mistakes.</span></span> <span data-ttu-id="e5007-108">Basiert auf einem [Präsentation](http://vimeo.com/68390507) von **Damian Edwards** Conference Norwegisch Entwickler.</span><span class="sxs-lookup"><span data-stu-id="e5007-108">It is based on a [presentation](http://vimeo.com/68390507) by **Damian Edwards** at Norwegian Developers Conference.</span></span>


## <a name="disclaimer"></a><span data-ttu-id="e5007-109">Haftungsausschluss</span><span class="sxs-lookup"><span data-stu-id="e5007-109">Disclaimer</span></span>

<span data-ttu-id="e5007-110">In diesem Thema ist nicht als eine vollständige Anleitung vorgesehen, um sicherzustellen, dass Ihre Anwendung sicheren und effizienten ist.</span><span class="sxs-lookup"><span data-stu-id="e5007-110">This topic is not intended as a complete guide to ensure your application is secure and efficient.</span></span> <span data-ttu-id="e5007-111">Sie müssen noch best Practices für Sicherheit und Leistung folgen, die nicht in diesem Thema beschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="e5007-111">You still need to follow best practices for security and performance that are not outlined in this topic.</span></span> <span data-ttu-id="e5007-112">Es wird nur das Vermeiden von häufigen Fehlern im Zusammenhang mit .NET-Klassen und Prozesse vorgeschlagen.</span><span class="sxs-lookup"><span data-stu-id="e5007-112">It only suggests how to avoid common mistakes related to .NET classes and processes.</span></span>

## <a name="overview"></a><span data-ttu-id="e5007-113">Übersicht</span><span class="sxs-lookup"><span data-stu-id="e5007-113">Overview</span></span>

<span data-ttu-id="e5007-114">Dieses Thema enthält folgende Abschnitte:</span><span class="sxs-lookup"><span data-stu-id="e5007-114">This topic contains the following sections:</span></span>

- [<span data-ttu-id="e5007-115">Einhaltung von Standards</span><span class="sxs-lookup"><span data-stu-id="e5007-115">Standards Compliance</span></span>](#standards)

    - [<span data-ttu-id="e5007-116">Steuerelementadapter</span><span class="sxs-lookup"><span data-stu-id="e5007-116">Control Adapters</span></span>](#adapters)
    - [<span data-ttu-id="e5007-117">Stileigenschaften für Steuerelemente</span><span class="sxs-lookup"><span data-stu-id="e5007-117">Style Properties on Controls</span></span>](#styleprop)
    - [<span data-ttu-id="e5007-118">Seiten- und Steuerelement Rückrufe</span><span class="sxs-lookup"><span data-stu-id="e5007-118">Page and Control Callbacks</span></span>](#callback)
    - [<span data-ttu-id="e5007-119">Erkennung von Browser-Funktion</span><span class="sxs-lookup"><span data-stu-id="e5007-119">Browser Capability Detection</span></span>](#browsercap)
- [<span data-ttu-id="e5007-120">Sicherheit</span><span class="sxs-lookup"><span data-stu-id="e5007-120">Security</span></span>](#security)

    - [<span data-ttu-id="e5007-121">Anforderungsüberprüfung</span><span class="sxs-lookup"><span data-stu-id="e5007-121">Request Validation</span></span>](#validation)
    - [<span data-ttu-id="e5007-122">Formularauthentifizierung und Sitzung</span><span class="sxs-lookup"><span data-stu-id="e5007-122">Cookieless Forms Authentication and Session</span></span>](#cookieless)
    - [<span data-ttu-id="e5007-123">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="e5007-123">EnableViewStateMac</span></span>](#viewstatemac)
    - [<span data-ttu-id="e5007-124">Mittlere Vertrauenswürdigkeit</span><span class="sxs-lookup"><span data-stu-id="e5007-124">Medium Trust</span></span>](#medium)
    - [<span data-ttu-id="e5007-125">&lt;"appSettings"&gt;</span><span class="sxs-lookup"><span data-stu-id="e5007-125">&lt;appSettings&gt;</span></span>](#appsettings)
    - [<span data-ttu-id="e5007-126">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="e5007-126">UrlPathEncode</span></span>](#urlpathencode)
- [<span data-ttu-id="e5007-127">Zuverlässigkeit und Leistung</span><span class="sxs-lookup"><span data-stu-id="e5007-127">Reliability and Performance</span></span>](#performance)

    - [<span data-ttu-id="e5007-128">PreSendRequestHeaders und PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="e5007-128">PreSendRequestHeaders and PreSendRequestContent</span></span>](#presend)
    - [<span data-ttu-id="e5007-129">Asynchrone Page-Ereignisse mit WebForms</span><span class="sxs-lookup"><span data-stu-id="e5007-129">Asynchronous Page Events with Web Forms</span></span>](#asyncevents)
    - [<span data-ttu-id="e5007-130">Auslösen und vergessen Arbeit</span><span class="sxs-lookup"><span data-stu-id="e5007-130">Fire-and-Forget Work</span></span>](#fire)
    - [<span data-ttu-id="e5007-131">Anforderungs-Einheitstextkörper</span><span class="sxs-lookup"><span data-stu-id="e5007-131">Request Entity Body</span></span>](#requestentity)
    - [<span data-ttu-id="e5007-132">Response.Redirect und Response.End</span><span class="sxs-lookup"><span data-stu-id="e5007-132">Response.Redirect and Response.End</span></span>](#redirect)
    - [<span data-ttu-id="e5007-133">EnableViewState und ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="e5007-133">EnableViewState and ViewStateMode</span></span>](#viewstatemode)
    - [<span data-ttu-id="e5007-134">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="e5007-134">SqlMembershipProvider</span></span>](#sqlprovider)
    - [<span data-ttu-id="e5007-135">Anforderungen für den lang ausgeführt (> 110 Sekunden)</span><span class="sxs-lookup"><span data-stu-id="e5007-135">Long Running Requests (>110 seconds)</span></span>](#long)

<a id="standards"></a>

## <a name="standards-compliance"></a><span data-ttu-id="e5007-136">Einhaltung von Standards</span><span class="sxs-lookup"><span data-stu-id="e5007-136">Standards Compliance</span></span>

<a id="adapters"></a>

### <a name="control-adapters"></a><span data-ttu-id="e5007-137">Steuerelementadapter</span><span class="sxs-lookup"><span data-stu-id="e5007-137">Control Adapters</span></span>

<span data-ttu-id="e5007-138">Empfehlung: Beenden Sie die Verwendung von Steuerelementadapter für adaptives Rendering und stattdessen Sie CSS-Medienabfragen und Standards kompatiblen HTML.</span><span class="sxs-lookup"><span data-stu-id="e5007-138">Recommendation: Stop using control adapters for adaptive rendering, and instead use CSS media queries and standards-compliant HTML.</span></span>

<span data-ttu-id="e5007-139">Steuerelemente-Adapter wurden in .NET 2.0 Präsentation Code gerendert, die für verschiedene Geräte und Umgebungen angepasst wurde eingeführt.</span><span class="sxs-lookup"><span data-stu-id="e5007-139">Controls Adapters were introduced in .NET 2.0 to render presentation code that was customized for different devices and environments.</span></span> <span data-ttu-id="e5007-140">Nun kann dieser adaptive Rendern HTML und CSS-erreicht werden.</span><span class="sxs-lookup"><span data-stu-id="e5007-140">Now, this adaptive rendering can be accomplished with CSS and HTML.</span></span> <span data-ttu-id="e5007-141">Sie sollten nicht mehr verwenden Steuerelementadapter und konvertieren alle vorhandenen Adapter in CSS- und HTML.</span><span class="sxs-lookup"><span data-stu-id="e5007-141">You should stop using Control Adapters and convert any existing adapters to CSS and HTML.</span></span>

<span data-ttu-id="e5007-142">Weitere Informationen finden Sie unter [Medienabfragen](http://www.w3.org/TR/css3-mediaqueries/) und [Vorgehensweise: Hinzufügen mobiler Seiten zu Ihren ASP.NET Web Forms / MVC-Anwendung](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="e5007-142">For more information, see [Media Queries](http://www.w3.org/TR/css3-mediaqueries/) and [How To: Add Mobile Pages to Your ASP.NET Web Forms / MVC Application](../../../whitepapers/add-mobile-pages-to-your-aspnet-web-forms-mvc-application.md).</span></span>

<a id="styleprop"></a>

### <a name="style-properties-on-controls"></a><span data-ttu-id="e5007-143">Stileigenschaften für Steuerelemente</span><span class="sxs-lookup"><span data-stu-id="e5007-143">Style Properties on Controls</span></span>

<span data-ttu-id="e5007-144">Empfehlung: Beenden Sie Einstellungswerte in Markup des Steuerelements, und legen Sie stattdessen Formatierung Werte im CSS-Stylesheets.</span><span class="sxs-lookup"><span data-stu-id="e5007-144">Recommendation: Stop setting style values in the control markup, and instead set formatting values in CSS stylesheets.</span></span>

<span data-ttu-id="e5007-145">Webserversteuerelemente enthalten Dutzende von Eigenschaften, die zum Festlegen von Stileigenschaften für in-Line-verwendet werden kann.</span><span class="sxs-lookup"><span data-stu-id="e5007-145">Web server controls contain dozens of properties which can be used to set in-line style properties.</span></span> <span data-ttu-id="e5007-146">Beispielsweise legt die ForeColor-Eigenschaft, die Farbe des Texts für ein Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="e5007-146">For example, the ForeColor property sets the color of the text for a control.</span></span> <span data-ttu-id="e5007-147">Sie können diesen Effekt effizienter durch CSS-Stylesheets erreichen.</span><span class="sxs-lookup"><span data-stu-id="e5007-147">You can accomplish this same effect more efficiently through CSS stylesheets.</span></span> <span data-ttu-id="e5007-148">Stylesheets können Sie zum Zentralisieren der Style-Werte, und vermeiden Sie diese Werte in der gesamten Anwendung festzulegen.</span><span class="sxs-lookup"><span data-stu-id="e5007-148">Stylesheets enable you to centralize style values and avoid setting these values throughout your application.</span></span>

<span data-ttu-id="e5007-149">Das folgende Beispiel zeigt einer CSS-Klasse, die Mengen Text in Rot.</span><span class="sxs-lookup"><span data-stu-id="e5007-149">The following example shows a CSS class the sets text to red.</span></span>

[!code-css[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample1.css)]

<span data-ttu-id="e5007-150">Im nächste Beispiel veranschaulicht die dynamisch anwenden, die CSS-Klasse.</span><span class="sxs-lookup"><span data-stu-id="e5007-150">The next example shows how to dynamically apply the CSS class.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample2.cs)]

<a id="callback"></a>

### <a name="page-and-control-callbacks"></a><span data-ttu-id="e5007-151">Seiten- und Steuerelement Rückrufe</span><span class="sxs-lookup"><span data-stu-id="e5007-151">Page and Control Callbacks</span></span>

<span data-ttu-id="e5007-152">Empfehlung: Beenden Sie die Verwendung der Seiten- und Rückrufe und stattdessen eine der folgenden: AJAX, UpdatePanel, MVC-Aktionsmethoden, Web-API oder SignalR.</span><span class="sxs-lookup"><span data-stu-id="e5007-152">Recommendation: Stop using page and control callbacks, and instead use any of the following: AJAX, UpdatePanel, MVC action methods, Web API, or SignalR.</span></span>

<span data-ttu-id="e5007-153">In früheren Versionen von ASP.NET ermöglichte Seiten- und Rückrufmethoden Teil der Webseite zu aktualisieren, ohne eine gesamte Seite zu aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="e5007-153">In earlier versions of ASP.NET, Page and Control callback methods enabled you to update part of the web page without refreshing an entire page.</span></span> <span data-ttu-id="e5007-154">Erreichen Sie Aktualisierungen von Teilseiten bis jetzt [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/en-US/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web-API](../../../web-api/index.md) oder [SignalR](../../../signalr/index.md).</span><span class="sxs-lookup"><span data-stu-id="e5007-154">You can now accomplish partial-page updates through [AJAX](../../../ajax/index.md), [UpdatePanel](https://msdn.microsoft.com/en-US/library/bb386454.aspx), [MVC](../../../mvc/index.md), [Web API](../../../web-api/index.md) or [SignalR](../../../signalr/index.md).</span></span> <span data-ttu-id="e5007-155">Beenden Sie Rückrufmethoden verwenden, da Probleme mit friendly URLs verursacht werden können und routing.</span><span class="sxs-lookup"><span data-stu-id="e5007-155">You should stop using callback methods because they can cause issues with friendly URLs and routing.</span></span> <span data-ttu-id="e5007-156">Standardmäßig Steuerelemente Rückrufmethoden nicht aktivieren, aber wenn Sie diese Funktion in einem Steuerelement aktiviert haben, sollten Sie es deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="e5007-156">By default, controls do not enable callback methods, but if you enabled this feature in a control, you should disable it.</span></span>

<a id="browsercap"></a>

### <a name="browser-capability-detection"></a><span data-ttu-id="e5007-157">Erkennung von Browser-Funktion</span><span class="sxs-lookup"><span data-stu-id="e5007-157">Browser Capability Detection</span></span>

<span data-ttu-id="e5007-158">Empfehlung: Beenden Sie die Verwendung von statischen Browser-Funktion zur Erkennung und stattdessen verwenden Sie dynamische Funktion Erkennung zu.</span><span class="sxs-lookup"><span data-stu-id="e5007-158">Recommendation: Stop using static browser capability detection, and instead use dynamic feature detection.</span></span>

<span data-ttu-id="e5007-159">Die unterstützten Funktionen für jeden Browser wurden in früheren Versionen von ASP.NET in eine XML-Datei gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e5007-159">In earlier versions of ASP.NET, the supported features for each browser were stored in an XML file.</span></span> <span data-ttu-id="e5007-160">Erkennen von Feature-Unterstützung über die statische Suche ist nicht der beste Ansatz.</span><span class="sxs-lookup"><span data-stu-id="e5007-160">Detecting feature support through a static lookup is not the best approach.</span></span> <span data-ttu-id="e5007-161">Jetzt, Sie können dynamisch erkennen, ein Browser Features wie z. B. mithilfe einer Funktion zur Erkennung Framework unterstützten des [Modernizr](http://modernizr.com/).</span><span class="sxs-lookup"><span data-stu-id="e5007-161">Now, you can dynamically detect a browser's supported features by using a feature detection framework, such as [Modernizr](http://modernizr.com/).</span></span> <span data-ttu-id="e5007-162">Funktion zur Erkennung bestimmt die Unterstützung von versuchen, eine Methode oder Eigenschaft zu verwenden, und klicken Sie dann überprüfen, um festzustellen, ob der Browser das gewünschte Ergebnis hervorgerufen hat.</span><span class="sxs-lookup"><span data-stu-id="e5007-162">Feature detection determines support by attempting to use a method or property and then checking to see if the browser produced the desired result.</span></span> <span data-ttu-id="e5007-163">Modernizr ist standardmäßig in der Anwendungsvorlagen enthalten.</span><span class="sxs-lookup"><span data-stu-id="e5007-163">By default, Modernizr is included in the Web application templates.</span></span>

<a id="security"></a>

## <a name="security"></a><span data-ttu-id="e5007-164">Sicherheit</span><span class="sxs-lookup"><span data-stu-id="e5007-164">Security</span></span>

<a id="validation"></a>

### <a name="request-validation"></a><span data-ttu-id="e5007-165">Anforderungsüberprüfung</span><span class="sxs-lookup"><span data-stu-id="e5007-165">Request Validation</span></span>

<span data-ttu-id="e5007-166">Empfehlung: Validieren von Benutzereingaben und Ausgabe von Benutzern zu codieren.</span><span class="sxs-lookup"><span data-stu-id="e5007-166">Recommendation: Validate user input, and encode output from users.</span></span>

<span data-ttu-id="e5007-167">Anforderungsüberprüfung ist ein Feature von ASP.NET, die untersucht jede Anforderung und die Anforderung beendet, wenn eine erkannten Bedrohung gefunden wird.</span><span class="sxs-lookup"><span data-stu-id="e5007-167">Request validation is a feature of ASP.NET that inspects each request and stops the request if a perceived threat is found.</span></span> <span data-ttu-id="e5007-168">Hängen Sie nicht davon anforderungsüberprüfung für Ihre Anwendung vor Angriffen durch Cross-Site scripting sichern.</span><span class="sxs-lookup"><span data-stu-id="e5007-168">Do not depend on request validation for securing your application against cross-site scripting attacks.</span></span> <span data-ttu-id="e5007-169">Stattdessen überprüfen Sie aller Eingaben von Benutzern und codieren Sie die Ausgabe zu.</span><span class="sxs-lookup"><span data-stu-id="e5007-169">Instead, validate all input from users and encode the output.</span></span> <span data-ttu-id="e5007-170">In manchen Fällen verwenden von regulären Ausdrücken zur Überprüfung der Eingabe, aber in etwas komplizierteren Fällen, die Sie überprüfen sollten Benutzereingaben mithilfe von Klassen in .NET, die bestimmen, ob der Wert übereinstimmt. zulässige Werte.</span><span class="sxs-lookup"><span data-stu-id="e5007-170">In some limited cases, you can use regular expressions to validate the input, but in more complicated cases you should validate user input by using .NET classes that determine if the value matches allowed values.</span></span>

<span data-ttu-id="e5007-171">Das folgende Beispiel zeigt, wie eine statische Methode in der Uri-Klasse verwenden, um zu bestimmen, ob der Uri von einem Benutzer gültig ist.</span><span class="sxs-lookup"><span data-stu-id="e5007-171">The following example shows how to use a static method in the Uri class to determine whether the Uri provided by a user is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample3.cs)]

<span data-ttu-id="e5007-172">Allerdings um den Uri ausreichend zu überprüfen, Sie sollten auch überprüfen um sicherzustellen, dass er gibt `http` oder `https`.</span><span class="sxs-lookup"><span data-stu-id="e5007-172">However, to sufficiently verify the Uri, you should also check to make sure it specifies `http` or `https`.</span></span> <span data-ttu-id="e5007-173">Im folgenden Beispiel wird Instanzmethoden, um sicherzustellen, dass der Uri gültig ist.</span><span class="sxs-lookup"><span data-stu-id="e5007-173">The following example uses instance methods to verify that the Uri is valid.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample4.cs)]

<span data-ttu-id="e5007-174">Codieren Sie Benutzereingaben als HTML-rendern oder einschließlich Benutzereingaben in einer SQL-Abfrage, die Werte, um sicherzustellen, dass bösartiger Code nicht enthalten ist.</span><span class="sxs-lookup"><span data-stu-id="e5007-174">Before rendering user input as HTML or including user input in a SQL query, encode the values to ensure malicious code is not included.</span></span>

<span data-ttu-id="e5007-175">Sie können HTML codieren Sie den Wert im Markup, mit der &lt;%: %&gt; Syntax, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="e5007-175">You can HTML encode the value in markup with the &lt;%: %&gt; syntax, as shown below.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample5.aspx?highlight=1)]

<span data-ttu-id="e5007-176">In Razor-Syntax können Sie HTML codieren mit @, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="e5007-176">Or, in Razor syntax, you can HTML encode with @, as shown below.</span></span>

[!code-cshtml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="e5007-177">Im folgenden Beispiel gezeigt Codieren der HTML wie einen Wert im Code-Behind.</span><span class="sxs-lookup"><span data-stu-id="e5007-177">The next example shows how to HTML encode a value in code-behind.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample7.cs)]

<span data-ttu-id="e5007-178">Um einen Wert für die SQL-Befehle sicher zu codieren, verwenden Sie Parameter des Befehls wie z. B. die ["SqlParameter"](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlparameter.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5007-178">To safely encode a value for SQL commands, use command parameters such as the [SqlParameter](https://msdn.microsoft.com/en-us/library/system.data.sqlclient.sqlparameter.aspx).</span></span> <a id="cookieless"></a>

### <a name="cookieless-forms-authentication-and-session"></a><span data-ttu-id="e5007-179">Formularauthentifizierung und Sitzung</span><span class="sxs-lookup"><span data-stu-id="e5007-179">Cookieless Forms Authentication and Session</span></span>

<span data-ttu-id="e5007-180">Empfehlung: Erfordern Sie Cookies.</span><span class="sxs-lookup"><span data-stu-id="e5007-180">Recommendation: Require cookies.</span></span>

<span data-ttu-id="e5007-181">Übergeben Authentifizierungsinformationen in der Abfragezeichenfolge ist nicht sicher.</span><span class="sxs-lookup"><span data-stu-id="e5007-181">Passing authentication information in the query string is not secure.</span></span> <span data-ttu-id="e5007-182">Erfordern Sie daher Cookies auf, wenn Ihre Anwendung Authentifizierung enthält.</span><span class="sxs-lookup"><span data-stu-id="e5007-182">Therefore, require cookies when your application includes authentication.</span></span> <span data-ttu-id="e5007-183">Wenn Ihr Cookie vertraulichen Informationen gespeichert werden, erwägen Sie, erfordern von SSL für das Cookie.</span><span class="sxs-lookup"><span data-stu-id="e5007-183">If your cookie stores sensitive information, consider requiring SSL for the cookie.</span></span>

<span data-ttu-id="e5007-184">Im folgende Beispiel wird gezeigt, wie in der Datei "Web.config" angeben, dass die Formularauthentifizierung ist einen Cookie erforderlich, der über SSL übermittelt wird.</span><span class="sxs-lookup"><span data-stu-id="e5007-184">The following example shows how to specify in the Web.config file that Forms Authentication requires a cookie that is transmitted over SSL.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample8.xml)]

<a id="viewstatemac"></a>

### <a name="enableviewstatemac"></a><span data-ttu-id="e5007-185">EnableViewStateMac</span><span class="sxs-lookup"><span data-stu-id="e5007-185">EnableViewStateMac</span></span>

<span data-ttu-id="e5007-186">Empfehlung: Nie festlegen auf "false".</span><span class="sxs-lookup"><span data-stu-id="e5007-186">Recommendation: Never set to false.</span></span>

<span data-ttu-id="e5007-187">Standardmäßig EnbableViewStateMac festgelegt ist auf "true".</span><span class="sxs-lookup"><span data-stu-id="e5007-187">By default, EnbableViewStateMac is set to true.</span></span> <span data-ttu-id="e5007-188">Auch wenn die Anwendung kein Ansichtszustand verwendet wird, legen Sie EnableViewStateMac nicht auf "false".</span><span class="sxs-lookup"><span data-stu-id="e5007-188">Even if your application is not using view state, do not set EnableViewStateMac to false.</span></span> <span data-ttu-id="e5007-189">Wenn dieser Wert auf "false" wird die Anwendung anfällig für siteübergreifende machen.</span><span class="sxs-lookup"><span data-stu-id="e5007-189">Setting this value to false will make your application vulnerable to cross-site scripting.</span></span>

<span data-ttu-id="e5007-190">ASP.NET 4.5.2 ab, die die Laufzeit erzwingt **EnableViewStateMac = "true"**.</span><span class="sxs-lookup"><span data-stu-id="e5007-190">Starting with ASP.NET 4.5.2, the runtime enforces **EnableViewStateMac=true**.</span></span> <span data-ttu-id="e5007-191">Auch wenn Sie auf "false" festlegen, wird die Common Language Runtime ignoriert diesen Wert und mit der festgelegte Wert auf "true" wird fortgesetzt.</span><span class="sxs-lookup"><span data-stu-id="e5007-191">Even if you set it to false, the runtime ignores this value and proceeds with the value set to true.</span></span> <span data-ttu-id="e5007-192">Weitere Informationen finden Sie unter [ASP.NET 4.5.2 und EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5007-192">For more information, see [ASP.NET 4.5.2 and EnableViewStateMac](https://blogs.msdn.com/b/webdev/archive/2014/05/07/asp-net-4-5-2-and-enableviewstatemac.aspx).</span></span>

<span data-ttu-id="e5007-193">Im folgende Beispiel wird gezeigt, wie EnableViewStateMac auf "true" festgelegt wird.</span><span class="sxs-lookup"><span data-stu-id="e5007-193">The following example shows how to set EnableViewStateMac to true.</span></span> <span data-ttu-id="e5007-194">Sie müssen nicht tatsächlich legen Sie diesen Wert auf "true", da sie in der Standardeinstellung "true" ist.</span><span class="sxs-lookup"><span data-stu-id="e5007-194">You do not need to actually set this value to true because it is true by default.</span></span> <span data-ttu-id="e5007-195">Wenn Sie es auf "false" auf einer beliebigen Seite in der Anwendung festgelegt haben, müssen Sie diesen Wert sofort beheben.</span><span class="sxs-lookup"><span data-stu-id="e5007-195">However, if you have set it to false on any page in your application, you must immediately correct this value.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample9.aspx)]

<a id="medium"></a>

### <a name="medium-trust"></a><span data-ttu-id="e5007-196">Mittlere Vertrauenswürdigkeit</span><span class="sxs-lookup"><span data-stu-id="e5007-196">Medium Trust</span></span>

<span data-ttu-id="e5007-197">Empfehlung: Hängen Sie nicht davon mittlere Vertrauensebene (oder einer anderen Ebene der Vertrauenswürdigkeit) als Sicherheitsgrenze.</span><span class="sxs-lookup"><span data-stu-id="e5007-197">Recommendation: Do not depend on Medium Trust (or any other trust level) as a security boundary.</span></span>

<span data-ttu-id="e5007-198">Teilweiser Vertrauenswürdigkeit schützt Ihre Anwendung nicht angemessen und sollte nicht verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="e5007-198">Partial trust does not adequately protect your application and should not be used.</span></span> <span data-ttu-id="e5007-199">Verwenden Sie stattdessen volle Vertrauenswürdigkeit, und nicht vertrauenswürdige Anwendungen in separaten Anwendungspools isolieren.</span><span class="sxs-lookup"><span data-stu-id="e5007-199">Instead, use Full Trust, and isolate untrusted applications in separate application pools.</span></span> <span data-ttu-id="e5007-200">Jeder Anwendungspool unter einer eindeutigen Identität, zudem ausführen.</span><span class="sxs-lookup"><span data-stu-id="e5007-200">Also, run each application pool under a unique identity.</span></span> <span data-ttu-id="e5007-201">Weitere Informationen finden Sie unter [ASP.NET teilweise Vertrauenswürdigkeit garantiert nicht Anwendungsisolation](https://support.microsoft.com/kb/2698981).</span><span class="sxs-lookup"><span data-stu-id="e5007-201">For more information, see [ASP.NET Partial Trust does not guarantee application isolation](https://support.microsoft.com/kb/2698981).</span></span>

<a id="appsettings"></a>

### <a name="ltappsettingsgt"></a><span data-ttu-id="e5007-202">&lt;"appSettings"&gt;</span><span class="sxs-lookup"><span data-stu-id="e5007-202">&lt;appSettings&gt;</span></span>

<span data-ttu-id="e5007-203">Empfehlung: Deaktivieren Sie die Sicherheitseinstellungen in nicht &lt;"appSettings"&gt; Element.</span><span class="sxs-lookup"><span data-stu-id="e5007-203">Recommendation: Do not disable security settings in &lt;appSettings&gt; element.</span></span>

<span data-ttu-id="e5007-204">Das Element "appSettings" enthält viele Werte, die für die Sicherheitsupdates erforderlich sind.</span><span class="sxs-lookup"><span data-stu-id="e5007-204">The appSettings element contains many values which are required for security updates.</span></span> <span data-ttu-id="e5007-205">Sie sollten nicht ändern oder deaktivieren diese Werte.</span><span class="sxs-lookup"><span data-stu-id="e5007-205">You should not change or disable these values.</span></span> <span data-ttu-id="e5007-206">Wenn Sie diese Werte bei der Bereitstellung eines Updates deaktivieren müssen, sofort aktivieren Sie wieder nach Abschluss der Bereitstellung.</span><span class="sxs-lookup"><span data-stu-id="e5007-206">If you must disable these values when deploying an update, immediately re-enable after completing deployment.</span></span>

<span data-ttu-id="e5007-207">Weitere Informationen finden Sie unter [ASP.NET AppSettings-Element](https://msdn.microsoft.com/en-us/library/hh975440.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5007-207">For details, see [ASP.NET appSettings Element](https://msdn.microsoft.com/en-us/library/hh975440.aspx).</span></span>

<a id="urlpathencode"></a>

### <a name="urlpathencode"></a><span data-ttu-id="e5007-208">UrlPathEncode</span><span class="sxs-lookup"><span data-stu-id="e5007-208">UrlPathEncode</span></span>

<span data-ttu-id="e5007-209">Empfehlung: Verwenden von [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx) stattdessen.</span><span class="sxs-lookup"><span data-stu-id="e5007-209">Recommendation: Use [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx) instead.</span></span>

<span data-ttu-id="e5007-210">.NET Framework ein sehr spezifischen Browser Kompatibilitätsproblem behoben wurde die UrlPathEncode-Methode hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e5007-210">The UrlPathEncode method was added to the .NET Framework to resolve a very specific browser compatibility problem.</span></span> <span data-ttu-id="e5007-211">Er nicht ordnungsgemäß codiert eine URL und die Anwendung von siteübergreifendem Skripting bietet keinen Schutz.</span><span class="sxs-lookup"><span data-stu-id="e5007-211">It does not adequately encode a URL, and does not protect your application from cross-site scripting.</span></span> <span data-ttu-id="e5007-212">Sie sollten niemals in Ihrer Anwendung verwenden.</span><span class="sxs-lookup"><span data-stu-id="e5007-212">You should never use it in your application.</span></span> <span data-ttu-id="e5007-213">Verwenden Sie stattdessen [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5007-213">Instead, use [UrlEncode](https://msdn.microsoft.com/en-us/library/zttxte6w.aspx).</span></span>

<span data-ttu-id="e5007-214">Im folgende Beispiel wird gezeigt, wie eine codierte URL als ein Abfragezeichenfolgen-Parameter für ein Linksteuerelement übergeben wird.</span><span class="sxs-lookup"><span data-stu-id="e5007-214">The following example shows how to pass an encoded URL as a query string parameter for a hyperlink control.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample10.cs)]

<a id="performance"></a>

## <a name="reliability-and-performance"></a><span data-ttu-id="e5007-215">Zuverlässigkeit und Leistung</span><span class="sxs-lookup"><span data-stu-id="e5007-215">Reliability and Performance</span></span>

<a id="presend"></a>

### <a name="presendrequestheaders-and-presendrequestcontent"></a><span data-ttu-id="e5007-216">PreSendRequestHeaders und PreSendRequestContent</span><span class="sxs-lookup"><span data-stu-id="e5007-216">PreSendRequestHeaders and PreSendRequestContent</span></span>

<span data-ttu-id="e5007-217">Empfehlung: Verwenden Sie diese Ereignisse nicht mit verwalteten Module an.</span><span class="sxs-lookup"><span data-stu-id="e5007-217">Recommendation: Do not use these events with managed modules.</span></span> <span data-ttu-id="e5007-218">Schreiben Sie stattdessen ein natives Modul für IIS zum Ausführen der gewünschten Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="e5007-218">Instead, write a native IIS module to perform the required task.</span></span> <span data-ttu-id="e5007-219">Finden Sie unter [Erstellen von systemeigenem Code HTTP-Modulen](https://msdn.microsoft.com/en-us/library/ms693629.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5007-219">See [Creating Native-Code HTTP Modules](https://msdn.microsoft.com/en-us/library/ms693629.aspx).</span></span>

<span data-ttu-id="e5007-220">Sie können die [PreSendRequestHeaders](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestheaders.aspx) und [PreSendRequestContent](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestcontent.aspx) Ereignisse mit dem systemeigenen IIS-Module.</span><span class="sxs-lookup"><span data-stu-id="e5007-220">You can use the [PreSendRequestHeaders](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestheaders.aspx) and [PreSendRequestContent](https://msdn.microsoft.com/en-us/library/system.web.httpapplication.presendrequestcontent.aspx) events with native IIS modules.</span></span>
> [!WARNING]
> <span data-ttu-id="e5007-221">Verwenden Sie keine `PreSendRequestHeaders` und `PreSendRequestContent` mit verwalteten Modulen, die implementieren `IHttpModule`.</span><span class="sxs-lookup"><span data-stu-id="e5007-221">Do not use `PreSendRequestHeaders` and `PreSendRequestContent` with managed modules that implement `IHttpModule`.</span></span> <span data-ttu-id="e5007-222">Das Festlegen dieser Eigenschaften kann dazu führen, dass Probleme bei asynchronen Anforderungen.</span><span class="sxs-lookup"><span data-stu-id="e5007-222">Setting these properties can cause issues with asynchronous requests.</span></span> <span data-ttu-id="e5007-223">Die Kombination der Anwendung angeforderte Routing (ARR) und Websockets kann zu Ausnahmen für den Verstoß führen, die zum Absturz w3wp verursachen kann.</span><span class="sxs-lookup"><span data-stu-id="e5007-223">The combination of Application Requested Routing (ARR) and websockets might lead to access violation exceptions that can cause w3wp to crash.</span></span> <span data-ttu-id="e5007-224">Z. B. Iiscore! W3_CONTEXT_BASE::GetIsLastNotification + 68 in iiscore.dll verursacht eine Verletzung Zugriffsausnahme (0xC0000005).</span><span class="sxs-lookup"><span data-stu-id="e5007-224">For example, iiscore!W3_CONTEXT_BASE::GetIsLastNotification+68 in iiscore.dll has caused an access violation exception (0xC0000005).</span></span>

<a id="asyncevents"></a>

### <a name="asynchronous-page-events-with-web-forms"></a><span data-ttu-id="e5007-225">Asynchrone Page-Ereignisse mit WebForms</span><span class="sxs-lookup"><span data-stu-id="e5007-225">Asynchronous Page Events with Web Forms</span></span>

<span data-ttu-id="e5007-226">Empfehlung: In Web Forms, zu vermeiden, Schreiben asynchrone "void" Methoden für die Seite-Lebenszyklusereignisse und stattdessen [Page.RegisterAsyncTask](https://msdn.microsoft.com/en-us/library/system.web.ui.page.registerasynctask.aspx) für asynchronen Code.</span><span class="sxs-lookup"><span data-stu-id="e5007-226">Recommendation: In Web Forms, avoid writing async void methods for Page lifecycle events, and instead use [Page.RegisterAsyncTask](https://msdn.microsoft.com/en-us/library/system.web.ui.page.registerasynctask.aspx) for asynchronous code.</span></span>

<span data-ttu-id="e5007-227">Wenn Sie ein Page-Ereignisklasse mit markieren **Async** und **"void"**, Sie können nicht bestimmen, wann der asynchrone Code beendet wurde.</span><span class="sxs-lookup"><span data-stu-id="e5007-227">When you mark a page event with **async** and **void**, you cannot determine when the asynchronous code has finished.</span></span> <span data-ttu-id="e5007-228">Verwenden Sie stattdessen Page.RegisterAsyncTask, um den asynchronen Code auf eine Weise auszuführen, die Ihnen ermöglicht, dessen Abschluss nachzuverfolgen.</span><span class="sxs-lookup"><span data-stu-id="e5007-228">Instead, use Page.RegisterAsyncTask to run the asynchronous code in a way that enables you to track its completion.</span></span>

<span data-ttu-id="e5007-229">Das folgende Beispiel zeigt eine Schaltfläche klicken Sie auf Handler, der asynchronen Code enthält.</span><span class="sxs-lookup"><span data-stu-id="e5007-229">The following example shows a button click handler that contains asynchronous code.</span></span> <span data-ttu-id="e5007-230">Dieses Beispiel enthält einen Zeichenfolgenwert asynchron ausgeführt wird, lesen die dient nur als ein vereinfachtes Beispiel einer asynchronen Aufgabe und nicht als empfohlene Vorgehensweise.</span><span class="sxs-lookup"><span data-stu-id="e5007-230">This example includes reading a string value asynchronously, which is provided only as a simplified example of an asynchronous task and not as a recommended practice.</span></span>

[!code-csharp[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample11.cs)]

<span data-ttu-id="e5007-231">Bei Verwendung von asynchronen Aufgaben können Sie das Zielframework für HTTP-Laufzeit auf 4.5 festgelegt, in der Datei "Web.config".</span><span class="sxs-lookup"><span data-stu-id="e5007-231">If you are using asynchronous Tasks, set the Http runtime target framework to 4.5 in the Web.config file.</span></span> <span data-ttu-id="e5007-232">Durch Festlegen des Zielframeworks auf 4.5 im neuen Synchronisierungskontext, die wurde in .NET 4.5 hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="e5007-232">Setting the target framework to 4.5 turns on the new synchronization context that was added in .NET 4.5.</span></span> <span data-ttu-id="e5007-233">Dieser Wert wird standardmäßig in neuen Projekten in Visual Studio 2012 festgelegt, aber es ist nicht festgelegt werden, wenn Sie ein vorhandenes Projekt arbeiten.</span><span class="sxs-lookup"><span data-stu-id="e5007-233">This value is set by default in new projects in Visual Studio 2012, but is not be set if you are working with an existing project.</span></span>

[!code-xml[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample12.xml)]

<a id="fire"></a>

### <a name="fire-and-forget-work"></a><span data-ttu-id="e5007-234">Auslösen und vergessen Arbeit</span><span class="sxs-lookup"><span data-stu-id="e5007-234">Fire-and-Forget Work</span></span>

<span data-ttu-id="e5007-235">Empfehlung: Bei der Behandlung von einer Anforderung innerhalb von ASP.NET vermeiden Sie starten auslösen und vergessen Arbeit (solche Aufrufen der Methode ThreadPool.QueueUserWorkItem oder erstellen einen Zeitgeber, der wiederholt einen Delegaten aufruft).</span><span class="sxs-lookup"><span data-stu-id="e5007-235">Recommendation: When handling a request within ASP.NET, avoid launching fire-and-forget work (such calling the ThreadPool.QueueUserWorkItem method or creating a timer that repeatedly calls a delegate).</span></span>

<span data-ttu-id="e5007-236">Verfügt Ihre Anwendung Arbeit auslösen und vergessen, die innerhalb von ASP.NET ausgeführt wird, erhalten Ihre Anwendung nicht mehr synchron. Zu jedem Zeitpunkt kann die Anwendungsdomäne zerstört werden dies bedeutet, dass Ihre laufende Prozess den aktuellen Status der Anwendung möglicherweise nicht mehr übereinstimmen.</span><span class="sxs-lookup"><span data-stu-id="e5007-236">If your application has fire-and-forget work that runs within ASP.NET, your application can get out of sync. At any time, the app domain can be destroyed which means your ongoing process may no longer match the current state of the application.</span></span>

<span data-ttu-id="e5007-237">Sie sollten diese Art von Arbeit außerhalb von ASP.NET verschieben.</span><span class="sxs-lookup"><span data-stu-id="e5007-237">You should move this type of work outside of ASP.NET.</span></span> <span data-ttu-id="e5007-238">Sie können ein Web Jobs, Windows-Dienst oder eine workerrolle in Azure verwenden, um derzeit ausgeführte Arbeit auszuführen, und führen Sie diesen Code aus einem anderen Prozess.</span><span class="sxs-lookup"><span data-stu-id="e5007-238">You can use a Web Jobs, Windows Service or a Worker role in Azure to perform ongoing work, and run that code from another process.</span></span>

<span data-ttu-id="e5007-239">Wenn Sie diese Arbeit innerhalb von ASP.NET durchführen müssen, können Sie das NuGet-Paket aufgerufen hinzufügen [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) Ausführen des Codes.</span><span class="sxs-lookup"><span data-stu-id="e5007-239">If you must perform this work within ASP.NET, you can add the Nuget package called [WebBackgrounder](http://www.nuget.org/packages/webbackgrounder) to run the code.</span></span>

<a id="requestentity"></a>

### <a name="request-entity-body"></a><span data-ttu-id="e5007-240">Anforderungs-Einheitstextkörper</span><span class="sxs-lookup"><span data-stu-id="e5007-240">Request Entity Body</span></span>

<span data-ttu-id="e5007-241">Empfehlung: Vermeiden Sie Request.Form oder Request.InputStream lesen, bevor des Handlers des Ereignisses ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="e5007-241">Recommendation: Avoid reading Request.Form or Request.InputStream before the handler's execute event.</span></span>

<span data-ttu-id="e5007-242">Frühestens Request.Form oder Request.InputStream gelesen sollte ausführen während der Ereignishandler Ereignis.</span><span class="sxs-lookup"><span data-stu-id="e5007-242">The earliest you should read from Request.Form or Request.InputStream is during the handler's execute event.</span></span> <span data-ttu-id="e5007-243">In MVC wird der Controller ist der Handler und das Execute-Ereignis ist, wenn die Aktionsmethode ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="e5007-243">In MVC, the Controller is the handler and the execute event is when the action method runs.</span></span> <span data-ttu-id="e5007-244">In Web Forms die Seite wird der Handler und das Execute-Ereignis ist, wenn das Page.Init-Ereignis auslöst.</span><span class="sxs-lookup"><span data-stu-id="e5007-244">In Web Forms, the Page is the handler and the execute event is when the Page.Init event fires.</span></span> <span data-ttu-id="e5007-245">Wenn Sie die Anforderungs-einheitstextkörper vor der Execute-Ereignis lesen, beeinträchtigen Sie bei der Verarbeitung der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="e5007-245">If you read the request entity body earlier than the execute event, you interfere with the processing of the request.</span></span>

<span data-ttu-id="e5007-246">Wenn Sie die Anforderungs-einheitstextkörper vor der Execute-Ereignis zu lesen, verwenden Sie entweder [Request.GetBufferlessInputStream](https://msdn.microsoft.com/en-us/library/ff406798.aspx) oder [Request.GetBufferedInputStream](https://msdn.microsoft.com/en-us/library/system.web.httprequest.getbufferedinputstream.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5007-246">If you need to read the request entity body before the execute event, use either [Request.GetBufferlessInputStream](https://msdn.microsoft.com/en-us/library/ff406798.aspx) or [Request.GetBufferedInputStream](https://msdn.microsoft.com/en-us/library/system.web.httprequest.getbufferedinputstream.aspx).</span></span> <span data-ttu-id="e5007-247">Bei Verwendung von GetBufferlessInputStream den unformatierten Stream aus der Anforderung abrufen und für die Verarbeitung der gesamten Anforderungs verantwortlich.</span><span class="sxs-lookup"><span data-stu-id="e5007-247">When you use GetBufferlessInputStream, you get the raw stream from the request, and assume responsibility for processing the entire request.</span></span> <span data-ttu-id="e5007-248">Nach dem Aufruf von GetBufferlessInputStream sind Request.Form und Request.InputStream nicht verfügbar, da sie nicht von ASP.NET aufgefüllt wurden.</span><span class="sxs-lookup"><span data-stu-id="e5007-248">After calling GetBufferlessInputStream, Request.Form and Request.InputStream are not available because they have not been populated by ASP.NET.</span></span> <span data-ttu-id="e5007-249">Wenn Sie GetBufferedInputStream verwenden, erhalten Sie eine Kopie des Datenstroms aus der Anforderung.</span><span class="sxs-lookup"><span data-stu-id="e5007-249">When you use GetBufferedInputStream, you get a copy of the stream from the request.</span></span> <span data-ttu-id="e5007-250">Request.Form und Request.InputStream sind weiter unten in der Anforderung weiterhin verfügbar, da ASP.NET die andere Kopie füllt.</span><span class="sxs-lookup"><span data-stu-id="e5007-250">Request.Form and Request.InputStream are still available later in the request because ASP.NET populates the other copy.</span></span>

<a id="redirect"></a>

### <a name="responseredirect-and-responseend"></a><span data-ttu-id="e5007-251">Response.Redirect und Response.End</span><span class="sxs-lookup"><span data-stu-id="e5007-251">Response.Redirect and Response.End</span></span>

<span data-ttu-id="e5007-252">Empfehlung: Achten Sie Unterschiede in der Behandlung der Thread nach dem Aufruf [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5007-252">Recommendation: Be aware of differences in how thread is handled after calling [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx).</span></span>

<span data-ttu-id="e5007-253">Die [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx) Methode ruft die Response.End-Methode.</span><span class="sxs-lookup"><span data-stu-id="e5007-253">The [Response.Redirect(String)](https://msdn.microsoft.com/en-us/library/t9dwyts4.aspx) method calls the Response.End method.</span></span> <span data-ttu-id="e5007-254">In einem synchronen Prozess durch den Aufruf von Request.Redirect der aktuelle Thread sofort abgebrochen.</span><span class="sxs-lookup"><span data-stu-id="e5007-254">In a synchronous process, calling Request.Redirect causes the current thread to immediately abort.</span></span> <span data-ttu-id="e5007-255">Allerdings in einem asynchronen Prozess aufrufen Response.Redirect nicht den aktuellen Thread abgebrochen wird, damit die Ausführung von Code für die Anforderung ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="e5007-255">However, in an asynchronous process, calling Response.Redirect does not abort the current thread, so code execution continues for the request.</span></span> <span data-ttu-id="e5007-256">In einem asynchronen Prozess müssen Sie die Methode zum Beenden der Ausführung des Codes die Aufgabe zurückgeben.</span><span class="sxs-lookup"><span data-stu-id="e5007-256">In an asynchronous process, you must return the Task from the method to stop the code execution.</span></span>

<span data-ttu-id="e5007-257">In einer MVC-Projekt sollten Sie nicht Response.Redirect aufrufen.</span><span class="sxs-lookup"><span data-stu-id="e5007-257">In an MVC project, you should not call Response.Redirect.</span></span> <span data-ttu-id="e5007-258">Geben Sie stattdessen ein RedirectResult zurück.</span><span class="sxs-lookup"><span data-stu-id="e5007-258">Instead, return a RedirectResult.</span></span>

<a id="viewstatemode"></a>

### <a name="enableviewstate-and-viewstatemode"></a><span data-ttu-id="e5007-259">EnableViewState und ViewStateMode</span><span class="sxs-lookup"><span data-stu-id="e5007-259">EnableViewState and ViewStateMode</span></span>

<span data-ttu-id="e5007-260">Empfehlung: Verwendung ViewStateMode statt EnableViewState, ermöglichen eine präzise Kontrolle, über den Ansichtszustand von Steuerelemente verwenden.</span><span class="sxs-lookup"><span data-stu-id="e5007-260">Recommendation: Use ViewStateMode, instead of EnableViewState, to provide granular control over which controls use view state.</span></span>

<span data-ttu-id="e5007-261">Wenn Sie auf "false" in der Seitendirektive EnableViewState festlegen, wird Ansichtszustand für alle Steuerelemente auf der Seite ist deaktiviert und kann nicht aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="e5007-261">When you set EnableViewState to false in the Page directive, view state is disabled for all controls within the page and cannot be enabled.</span></span> <span data-ttu-id="e5007-262">Wenn Sie Anzeigen des Status für nur für bestimmte Steuerelemente auf der Seite aktivieren möchten, können Sie ViewStateMode auf deaktiviert festgelegt, für die Seite.</span><span class="sxs-lookup"><span data-stu-id="e5007-262">If you want to enable view state for only certain controls in your page, set ViewStateMode to Disabled for the Page.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample13.aspx)]

<span data-ttu-id="e5007-263">Klicken Sie dann auf nur die Steuerelemente, die tatsächlich Ansichtszustand benötigen ViewStateMode auf Enabled festgelegt.</span><span class="sxs-lookup"><span data-stu-id="e5007-263">Then, set ViewStateMode to Enabled on only the controls that actually need view state.</span></span>

[!code-aspx[Main](what-not-to-do-in-aspnet-and-what-to-do-instead/samples/sample14.aspx)]

<span data-ttu-id="e5007-264">Durch Aktivieren der Ansichtszustand für Steuerelemente, die sie benötigen, können Sie die Größe des Ansichtszustands für Ihre Web-Seiten verkleinern.</span><span class="sxs-lookup"><span data-stu-id="e5007-264">By enabling view state for only the controls that need it, you can shrink the size of the view state for your web pages.</span></span>

<a id="sqlprovider"></a>

### <a name="sqlmembershipprovider"></a><span data-ttu-id="e5007-265">SqlMembershipProvider</span><span class="sxs-lookup"><span data-stu-id="e5007-265">SqlMembershipProvider</span></span>

<span data-ttu-id="e5007-266">Empfehlung: Verwenden von Universal Providers.</span><span class="sxs-lookup"><span data-stu-id="e5007-266">Recommendation: Use Universal Providers.</span></span>

<span data-ttu-id="e5007-267">In der aktuellen Projektvorlagen SqlMembershipProvider wurde ersetzt durch [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), der als ein NuGet-Paket verfügbar ist.</span><span class="sxs-lookup"><span data-stu-id="e5007-267">In the current project templates, SqlMembershipProvider has been replaced by [ASP.NET Universal Providers](http://www.nuget.org/packages/Microsoft.AspNet.Providers), which is available as a NuGet package.</span></span> <span data-ttu-id="e5007-268">Wenn Sie SqlMembershipProvider in einem Projekt arbeiten, die mit einer früheren Version der Vorlagen erstellt wurde, sollten Sie auf Universal Providers wechseln.</span><span class="sxs-lookup"><span data-stu-id="e5007-268">If you are using SqlMembershipProvider in a project that was built with an earlier version of the templates, you should switch to Universal Providers.</span></span> <span data-ttu-id="e5007-269">Universal Providers funktioniert mit allen Datenbanken, die vom Entity Framework unterstützt werden.</span><span class="sxs-lookup"><span data-stu-id="e5007-269">The Universal Providers work with all databases that are supported by Entity Framework.</span></span>

<span data-ttu-id="e5007-270">Weitere Informationen finden Sie unter [Einführung in ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5007-270">For more information, see [Introducing ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx).</span></span>

<a id="long"></a>

### <a name="long-running-requests-110-seconds"></a><span data-ttu-id="e5007-271">Lang ausgeführte Anforderungen (> 110 Sekunden)</span><span class="sxs-lookup"><span data-stu-id="e5007-271">Long-running Requests (>110 seconds)</span></span>

<span data-ttu-id="e5007-272">Empfehlung: Verwenden von [WebSockets](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket.aspx) oder [SignalR](../../../signalr/index.md) für verbundene Clients und die Verwendung asynchroner e/a-Vorgänge.</span><span class="sxs-lookup"><span data-stu-id="e5007-272">Recommendation: Use [WebSockets](https://msdn.microsoft.com/en-us/library/system.net.websockets.websocket.aspx) or [SignalR](../../../signalr/index.md) for connected clients, and use asynchronous I/O operations.</span></span>

<span data-ttu-id="e5007-273">Lang ausgeführte Anforderungen können in der Webanwendung zu unvorhersehbaren Ergebnissen führt und eine schlechte Leistung führen.</span><span class="sxs-lookup"><span data-stu-id="e5007-273">Long-running requests can cause unpredictable results and poor performance in your web application.</span></span> <span data-ttu-id="e5007-274">Die standardmäßige Timeouteinstellung für eine Anforderung beträgt 110 Sekunden.</span><span class="sxs-lookup"><span data-stu-id="e5007-274">The default timeout setting for a request is 110 seconds.</span></span> <span data-ttu-id="e5007-275">Bei Verwendung von Sitzungsstatus mit einer lang andauernden-Anforderung gibt ASP.NET die Sperre für das Sitzungsobjekt nach 110 Sekunden frei.</span><span class="sxs-lookup"><span data-stu-id="e5007-275">If you are using session state with a long-running request, ASP.NET will release the lock on the Session object after 110 seconds.</span></span> <span data-ttu-id="e5007-276">Allerdings kann die Anwendung gerade ein Vorgang für das Sitzungsobjekt sein, wenn die Sperre wird aufgehoben, und der Vorgang kann nicht erfolgreich abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="e5007-276">However, your application might be in the middle of an operation on the Session object when the lock is released, and the operation might not complete successfully.</span></span> <span data-ttu-id="e5007-277">Wenn eine zweite Anforderung vom Benutzer blockiert wird, während die erste Anforderung ausgeführt wird, kann die zweite Anforderung das Sitzungsobjekt in einem inkonsistenten Zustand zugreifen.</span><span class="sxs-lookup"><span data-stu-id="e5007-277">If a second request from the user is blocked while the first request is running, the second request might access the Session object in an inconsistent state.</span></span>

<span data-ttu-id="e5007-278">Wenn Ihre Anwendung blockiert (oder synchrone) e/a-Vorgänge enthält, wird die Anwendung reagiert jedoch nicht.</span><span class="sxs-lookup"><span data-stu-id="e5007-278">If your application includes blocking (or synchronous) I/O operations, the application will be unresponsive.</span></span>

<span data-ttu-id="e5007-279">Um die Leistung zu verbessern, verwenden Sie die asynchrone e/a-Vorgänge in .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="e5007-279">To improve performance, use the asynchronous I/O operations in the .NET Framework.</span></span> <span data-ttu-id="e5007-280">Verwenden Sie außerdem WebSockets oder SignalR, für die Verbindung von Clients mit dem Server.</span><span class="sxs-lookup"><span data-stu-id="e5007-280">Also, use WebSockets or SignalR for connecting clients to the server.</span></span> <span data-ttu-id="e5007-281">Diese Funktionen dienen, lang ausgeführte Anforderungen effizient zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="e5007-281">These features are designed to efficiently handle long-running requests.</span></span>
