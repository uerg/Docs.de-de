---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Profilerstellung und Debuggen der ASP.NET MVC-app mit Glimpse | Microsoft-Dokumentation
author: Rick-Anderson
description: Glimpse ist erfolgreich sein und wachsende Familie von open-Source-NuGet-Pakete, die ausführliche Leistung bietet, Debuggen und Diagnoseinformationen für ASP.NET ein...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 94a72f22cbcd7fa84528dde502cceaa1e26dcaa1
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577287"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="462a0-103">Profilerstellung und Debuggen der ASP.NET MVC-app mit Glimpse</span><span class="sxs-lookup"><span data-stu-id="462a0-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="462a0-104">durch [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="462a0-104">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="462a0-105">Glimpse ist erfolgreich sein und wachsenden Familie von open-Source-NuGet-Pakete, die ausführliche Leistung bietet, Debuggen und Diagnoseinformationen für ASP.NET-Apps.</span><span class="sxs-lookup"><span data-stu-id="462a0-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="462a0-106">Dabei handelt es sich sehr einfach zu installieren, einfache, extrem schnellen, wichtige Leistungsmetriken am unteren Rand jeder Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="462a0-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="462a0-107">Sie können einen Drilldown in Ihre app ausführen beim müssen Sie herausfinden, was auf dem Server vor sich geht.</span><span class="sxs-lookup"><span data-stu-id="462a0-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="462a0-108">Glimpse bietet so viel wertvolle Informationen, die es wird, dass Sie es während des gesamten Entwicklungszyklus empfohlen, einschließlich Ihrer Azure-Test-Umgebung verwenden.</span><span class="sxs-lookup"><span data-stu-id="462a0-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="462a0-109">Während [Fiddler](http://www.telerik.com/fiddler) und [F-12 Entwicklungstools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) eine clientseitige Sicht Glimpse bietet eine detaillierte Übersicht vom Server.</span><span class="sxs-lookup"><span data-stu-id="462a0-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="462a0-110">Dieses Tutorial konzentriert sich auf die mit dem Blick auf ASP.NET MVC und EF-Pakete, aber viele andere Pakete verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="462a0-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="462a0-111">Nach Möglichkeit werden ich auf die entsprechenden verknüpfen [Glimpse-Docs](http://getglimpse.com/Docs/) die ich dabei helfen, verwalten.</span><span class="sxs-lookup"><span data-stu-id="462a0-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="462a0-112">Glimpse ist ein open-Source-Projekt, Sie zu seinen möglichen Beitrag zu den Quellcode und Dokumentation.</span><span class="sxs-lookup"><span data-stu-id="462a0-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="462a0-113">Installieren von Glimpse</span><span class="sxs-lookup"><span data-stu-id="462a0-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="462a0-114">Aktivieren von Glimpse für "localhost"</span><span class="sxs-lookup"><span data-stu-id="462a0-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="462a0-115">Die Registerkarte "Zeitplan"</span><span class="sxs-lookup"><span data-stu-id="462a0-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="462a0-116">Modellbindung</span><span class="sxs-lookup"><span data-stu-id="462a0-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="462a0-117">Routen</span><span class="sxs-lookup"><span data-stu-id="462a0-117">Routes</span></span>](#route)
- [<span data-ttu-id="462a0-118">Verwenden von Glimpse in Azure</span><span class="sxs-lookup"><span data-stu-id="462a0-118">Using Glimpse on Azure</span></span>](#da)
- <span data-ttu-id="462a0-119">[Additional Resources](#addRes) (Zusätzliche MSBuild-Ressourcen)</span><span class="sxs-lookup"><span data-stu-id="462a0-119">[Additional Resources](#addRes)</span></span>

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="462a0-120">Installieren von Glimpse</span><span class="sxs-lookup"><span data-stu-id="462a0-120">Installing Glimpse</span></span>

<span data-ttu-id="462a0-121">Sie können die Glimpse installieren, von der NuGet-Paket-Manager-Konsole oder von der **NuGet-Pakete verwalten** Konsole.</span><span class="sxs-lookup"><span data-stu-id="462a0-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="462a0-122">Für diese Demo werde ich die Mvc5- und EF6-Pakete zu installieren:</span><span class="sxs-lookup"><span data-stu-id="462a0-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![Installieren von Glimpse über NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="462a0-124">Suchen Sie nach *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="462a0-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF von NuGet installieren dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="462a0-126">Durch Auswahl **installierte Pakete**, sehen Sie die abhängigen Glimpse-Module installiert:</span><span class="sxs-lookup"><span data-stu-id="462a0-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Glimpse-Pakete aus DLg installiert](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="462a0-128">Die folgenden Befehle installieren Glimpse MVC5- und EF6-Module aus der Paket-Manager-Konsole:</span><span class="sxs-lookup"><span data-stu-id="462a0-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="462a0-129">Aktivieren von Glimpse für "localhost"</span><span class="sxs-lookup"><span data-stu-id="462a0-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="462a0-130">Navigieren Sie zu http://localhost:&lt; port #&gt;/glimpse.axd, und klicken Sie auf die <strong>Glimpse einschalten</strong> Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="462a0-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Glimpse Axd Seite](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="462a0-132">Wenn Sie Ihre Favoritenleiste angezeigt haben, können Sie ziehen und legen die Schaltflächen Einblick und deren hinzufügen als Bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="462a0-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![IE mit Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="462a0-134">Sie können jetzt Ihre app navigieren und die **Mal "Kopf" nach oben anzeigen** (HUD) wird am unteren Rand der Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="462a0-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Kontakt-Manager-Seite mit HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="462a0-136">Die [Glimpse HUD-Seite](http://getglimpse.com/Docs/Heads-up-Display) details der oben gezeigte Informationen zur zeitlichen Steuerung.</span><span class="sxs-lookup"><span data-stu-id="462a0-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="462a0-137">Die unaufdringliche Leistung dem HUD angezeigt können Sie für ein Problem sofort - benachrichtigen, bevor Sie den Testszyklus abrufen müssen.</span><span class="sxs-lookup"><span data-stu-id="462a0-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="462a0-138">Durch Klicken auf die &quot;g&quot; in der unteren rechten Ecke der Glimpse-Bereich wird:</span><span class="sxs-lookup"><span data-stu-id="462a0-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Glimpse-Bereich](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="462a0-140">In der Abbildung oben die [Registerkarte "Ausführung"](http://getglimpse.com/Docs/Execution-Tab) ausgewählt ist, zeigt die Details der zeitlichen Steuerung der Aktionen und Filter in der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="462a0-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="462a0-141">Sehen Sie meine [Überwachung beenden Filter Timer](http://www.nuget.org/packages/StopWatch/) in Phase 6 von der Pipeline beginnen.</span><span class="sxs-lookup"><span data-stu-id="462a0-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="462a0-142">Während meiner Lightweight-Timer bieten, kann nützliche Profil/Timing Data Erkennung fehlerhaft sein sollte die Zeit aufgewendet wird, bei der Autorisierung und zum Rendern der Ansicht.</span><span class="sxs-lookup"><span data-stu-id="462a0-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="462a0-143">Informieren Sie sich über meine Timer auf [Profil und die Uhrzeit Ihrer ASP.NET MVC-app in Azure ganz](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="462a0-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="462a0-144">Die [Registerkarten](http://getglimpse.com/Docs/Tabs) Seite enthält Links zu detaillierten Informationen auf jeder Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="462a0-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="462a0-145">Die Registerkarte "Zeitplan"</span><span class="sxs-lookup"><span data-stu-id="462a0-145">The Timeline tab</span></span>

<span data-ttu-id="462a0-146">Ich habe Tom Dykstras geändert ausstehenden [EF 6/MVC 5-Tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) , des Controllers des Dozenten mit dem folgenden Code zu ändern:</span><span class="sxs-lookup"><span data-stu-id="462a0-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="462a0-147">Der obige Code kann ich in einer Abfragezeichenfolge übergeben (`eager`) zum Steuern eager oder explizite Laden von Daten.</span><span class="sxs-lookup"><span data-stu-id="462a0-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="462a0-148">Explizites Laden werden in der folgenden Abbildung und die zeitlichen Steuerung Seite zeigt jede Registrierung geladen werden, der `Index` Aktionsmethode:</span><span class="sxs-lookup"><span data-stu-id="462a0-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![Explizites Laden](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="462a0-150">In den folgenden Code, mittels Eager Load angegeben ist, und jede Registrierung wird abgerufen, nachdem die `Index` Ansicht aufgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="462a0-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![mittels Eager Load ist angegeben.](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="462a0-152">Sie können auf ein Zeitsegment zum Abrufen von ausführliche Zeitsteuerungsinformationen zeigen:</span><span class="sxs-lookup"><span data-stu-id="462a0-152">You can hover over a time segment to get detailed timing information:</span></span>

![Wenn darauf gezeigt wird, um ausführliche Zeitsteuerungsdaten finden Sie unter](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="462a0-154">Modellbindung</span><span class="sxs-lookup"><span data-stu-id="462a0-154">Model Binding</span></span>

<span data-ttu-id="462a0-155">Die [Modell Registerkarte ' Bindung '](http://getglimpse.com/Docs/Model-Binding-Tab) bietet eine Fülle von Informationen, die Ihnen zu verstehen, wie Ihre Formularvariablen gebunden sind und warum einige nicht gebunden werden wie zu erwarten ist.</span><span class="sxs-lookup"><span data-stu-id="462a0-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="462a0-156">Die folgende Abbildung zeigt die **?**</span><span class="sxs-lookup"><span data-stu-id="462a0-156">The image below shows the **?**</span></span> <span data-ttu-id="462a0-157">Symbol, den Sie klicken können, um die Glimpse-Hilfeseite für diese Funktion aufzurufen.</span><span class="sxs-lookup"><span data-stu-id="462a0-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![Glimpse-modellbindung anzeigen](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="462a0-159">Routen</span><span class="sxs-lookup"><span data-stu-id="462a0-159">Routes</span></span>

 <span data-ttu-id="462a0-160">Die Registerkarte Glimpse Routen können können Sie das Debuggen und zu verstehen, routing.</span><span class="sxs-lookup"><span data-stu-id="462a0-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="462a0-161">In der folgenden Abbildung die Produkt-Route ausgewählt ist (und werden in Grün und eine Konvention Glimpse).</span><span class="sxs-lookup"><span data-stu-id="462a0-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="462a0-162">![Produktname ausgewählt](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route, Bereiche und Datentoken werden ebenfalls angezeigt.</span><span class="sxs-lookup"><span data-stu-id="462a0-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="462a0-163">Finden Sie unter [Glimpse Routen](http://getglimpse.com/Docs/Routes-Tab) und [Attribut-Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="462a0-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="462a0-164">Verwenden von Glimpse in Azure</span><span class="sxs-lookup"><span data-stu-id="462a0-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="462a0-165">Die Standardsicherheitsrichtlinie für Glimpse lässt nur Einblick in Daten vom lokalen Host angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="462a0-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="462a0-166">Sie können diese Sicherheitsrichtlinie ändern, damit Sie diese Daten auf einem Remoteserver (z. B. eine Web-app in Azure) anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="462a0-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="462a0-167">Für testumgebungen in Azure, fügen Sie die hervorgehobene Markierung bis zum Ende der *web.confg* Datei um Einblick zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="462a0-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="462a0-168">Durch diese Änderung nur sehen alle Benutzer Ihre Glimpse-Daten an einem Remotestandort.</span><span class="sxs-lookup"><span data-stu-id="462a0-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="462a0-169">Betrachten Sie das Markup oben zu einem Veröffentlichungsprofil hinzufügen, sodass sie nur eine angewendeten bereitgestellt hat bei der Verwendung dieses Veröffentlichungsprofils (beispielsweise Ihr Azure-Test-Proifle.) Um Einblick in die Datenübertragungszeiten einzuschränken, fügen wir die `canViewGlimpseData` Rolle und nur Benutzer in dieser Rolle aus, um Einblick in Daten anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="462a0-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="462a0-170">Entfernen Sie die Kommentare aus der *GlimpseSecurityPolicy.cs* und Ändern der ["IsInRole"](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) aus aufrufen `Administrator` auf die `canViewGlimpseData` Rolle:</span><span class="sxs-lookup"><span data-stu-id="462a0-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="462a0-171">Sicherheit – konnte die umfangreichen Daten Einblick in die Sicherheit Ihrer App verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="462a0-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="462a0-172">Microsoft hat eine sicherheitsüberwachung von Glimpse nicht für die Verwendung auf Produktionen apps ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="462a0-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="462a0-173">Informationen zum Hinzufügen von Rollen finden Sie in meinem [bereitstellen eine sicheren ASP.NET MVC 5-Web-app mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) Tutorial.</span><span class="sxs-lookup"><span data-stu-id="462a0-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="462a0-174">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="462a0-174">Additional Resources</span></span>

- [<span data-ttu-id="462a0-175">Bereitstellen einer sicheren ASP.NET MVC 5-app mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure</span><span class="sxs-lookup"><span data-stu-id="462a0-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="462a0-176">[Glimpse-Konfiguration](http://getglimpse.com/Docs/Configuration) -Doc-Seite zum Konfigurieren von Registerkarten, Laufzeitrichtlinie, Protokollierung und vieles mehr.</span><span class="sxs-lookup"><span data-stu-id="462a0-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
