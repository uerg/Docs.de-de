---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Ein Profil erstellen und Debuggen von ASP.NET MVC-Anwendung mit Glimpse | Microsoft Docs
author: Rick-Anderson
description: Glimpse ist verbergen und wachsenden-Familie von open Source-NuGet-Pakete, die detaillierte Leistung bietet, Debuggen und Diagnoseinformationen für ASP.NET ein...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 6ac23256c57116de81c7bf690d5ce743301c75ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a><span data-ttu-id="a83ef-103">Ein Profil erstellen und Debuggen von ASP.NET MVC-Anwendung mit Glimpse</span><span class="sxs-lookup"><span data-stu-id="a83ef-103">Profile and debug your ASP.NET MVC app with Glimpse</span></span>
====================
<span data-ttu-id="a83ef-104">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="a83ef-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="a83ef-105">Glimpse ist eine verbergen und wachsenden-Familie von open Source-NuGet-Pakete, die detaillierte Leistung bietet, debugging und Diagnoseinformationen für ASP.NET-Apps.</span><span class="sxs-lookup"><span data-stu-id="a83ef-105">Glimpse is a thriving and growing family of open source NuGet packages that provides detailed performance, debugging and diagnostic information for ASP.NET apps.</span></span> <span data-ttu-id="a83ef-106">Es ist sehr einfach zu installieren, einfache, extrem schnelle und wichtige Leistungsmetriken am unteren Rand jeder Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a83ef-106">It's trivial to install, lightweight, ultra-fast, and displays key performance metrics at the bottom of every page.</span></span> <span data-ttu-id="a83ef-107">Sie können einen Drilldown in Ihre app ausführen, wenn Sie benötigen, um herauszufinden, auf dem Server was.</span><span class="sxs-lookup"><span data-stu-id="a83ef-107">It allows you to drill down into your app when you need to find out what's going on at the server.</span></span> <span data-ttu-id="a83ef-108">Glimpse bietet so viel wertvolle Informationen empfehlen wir, dass Sie es in der gesamten Entwicklungszyklus, einschließlich der Azure-testumgebung verwenden.</span><span class="sxs-lookup"><span data-stu-id="a83ef-108">Glimpse provides so much valuable information we recommend you use it throughout your development cycle, including your Azure test environment.</span></span> <span data-ttu-id="a83ef-109">Während [Fiddler](http://www.telerik.com/fiddler) und [F-12 Entwicklungstools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) bieten eine clientseitige Sicht Glimpse bietet eine detaillierte Übersicht über den Server.</span><span class="sxs-lookup"><span data-stu-id="a83ef-109">While [Fiddler](http://www.telerik.com/fiddler) and the [F-12 development tools](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) provide a client side view, Glimpse provides a detailed view from the server.</span></span> <span data-ttu-id="a83ef-110">Dieses Lernprogramm konzentriert sich auf die mit der Glimpse ASP.NET MVC und EF-Paketen, aber viele andere Pakete verfügbar sind.</span><span class="sxs-lookup"><span data-stu-id="a83ef-110">This tutorial will focus on using the Glimpse ASP.NET MVC and EF packages, but many other packages are available.</span></span> <span data-ttu-id="a83ef-111">Nach Möglichkeit wird ich eine Verknüpfung in die entsprechende [Notizenlayouts Docs](http://getglimpse.com/Docs/) die ich zu gewährleisten.</span><span class="sxs-lookup"><span data-stu-id="a83ef-111">Where possible I will link to the appropriate [Glimpse docs](http://getglimpse.com/Docs/) which I help maintain.</span></span> <span data-ttu-id="a83ef-112">Glimpse ist ein open-Source-Projekt, Sie zu können tragen zu den Quellcode und Dokumente.</span><span class="sxs-lookup"><span data-stu-id="a83ef-112">Glimpse is an open source project, you too can contribute to the source code and the docs.</span></span>


- [<span data-ttu-id="a83ef-113">Installieren von Glimpse</span><span class="sxs-lookup"><span data-stu-id="a83ef-113">Installing Glimpse</span></span>](#ig)
- [<span data-ttu-id="a83ef-114">Aktivieren von Glimpse für "localhost"</span><span class="sxs-lookup"><span data-stu-id="a83ef-114">Enable Glimpse for localhost</span></span>](#eg)
- [<span data-ttu-id="a83ef-115">Die Registerkarte "Zeitplan"</span><span class="sxs-lookup"><span data-stu-id="a83ef-115">The Timeline tab</span></span>](#Time)
- [<span data-ttu-id="a83ef-116">Modellbindung</span><span class="sxs-lookup"><span data-stu-id="a83ef-116">Model Binding</span></span>](#mb)
- [<span data-ttu-id="a83ef-117">Routen</span><span class="sxs-lookup"><span data-stu-id="a83ef-117">Routes</span></span>](#route)
- [<span data-ttu-id="a83ef-118">Mithilfe von Glimpse in Azure</span><span class="sxs-lookup"><span data-stu-id="a83ef-118">Using Glimpse on Azure</span></span>](#da)
- <span data-ttu-id="a83ef-119">[Additional Resources](#addRes) (Zusätzliche MSBuild-Ressourcen)</span><span class="sxs-lookup"><span data-stu-id="a83ef-119">[Additional Resources](#addRes)</span></span>

<a id="ig"></a>
## <a name="installing-glimpse"></a><span data-ttu-id="a83ef-120">Installieren von Glimpse</span><span class="sxs-lookup"><span data-stu-id="a83ef-120">Installing Glimpse</span></span>

<span data-ttu-id="a83ef-121">Sie können Glimpse installieren, von der NuGet-Paket-Manager-Konsole oder von der **NuGet-Pakete verwalten** Konsole.</span><span class="sxs-lookup"><span data-stu-id="a83ef-121">You can install Glimpse from the NuGet package manager console or from the **Manage NuGet Packages** console.</span></span> <span data-ttu-id="a83ef-122">Für diese Demo werde ich die Mvc5 und EF6 Pakete installieren:</span><span class="sxs-lookup"><span data-stu-id="a83ef-122">For this demo, I'll install the Mvc5 and EF6 packages:</span></span>

![Installieren von Glimpse über NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

<span data-ttu-id="a83ef-124">Suchen Sie nach *Glimpse.EF*</span><span class="sxs-lookup"><span data-stu-id="a83ef-124">Search for *Glimpse.EF*</span></span>

![Glimpse.EF von NuGet-Installation dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

<span data-ttu-id="a83ef-126">Durch Auswahl **installierte Pakete**, sehen Sie die abhängigen Glimpse-Module installiert:</span><span class="sxs-lookup"><span data-stu-id="a83ef-126">By selecting **Installed packages**, you can see the Glimpse dependent modules installed:</span></span>

![Glimpse Pakete aus DLg installiert](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

<span data-ttu-id="a83ef-128">Die folgenden Befehle installieren Glimpse MVC5 und EF6 Module aus der Paket-Manager-Konsole:</span><span class="sxs-lookup"><span data-stu-id="a83ef-128">The following commands install Glimpse MVC5 and EF6 modules from the package manager console:</span></span>

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a><span data-ttu-id="a83ef-129">Aktivieren von Glimpse für "localhost"</span><span class="sxs-lookup"><span data-stu-id="a83ef-129">Enable Glimpse for localhost</span></span>

<span data-ttu-id="a83ef-130">Navigieren Sie zu http://localhost: &lt;port #&gt;/glimpse.axd und klicken Sie auf die <strong>Glimpse einschalten</strong> Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="a83ef-130">Navigate to http://localhost:&lt;port #&gt;/glimpse.axd and click the <strong>Turn Glimpse On</strong> button.</span></span>

![Seite "Glimpse Axd"](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

<span data-ttu-id="a83ef-132">Wenn Sie die Favoritenleiste angezeigt haben, können Sie ziehen und die Glimpse Schaltflächen löschen und deren hinzufügen als Bookmarklets:</span><span class="sxs-lookup"><span data-stu-id="a83ef-132">If you have your favorites bar displayed, you can drag and drop the Glimpse buttons and add them as bookmarklets:</span></span>

![IE mit Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

<span data-ttu-id="a83ef-134">Sie können jetzt Ihre app navigieren und die **Köpfe oben anzeigen** (HUD) wird am unteren Rand der Seite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a83ef-134">You can now navigate your app, and the **Heads Up Display** (HUD) is shown at the bottom of the page.</span></span>

![Seite mit HUD Kontakt-Manager](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

<span data-ttu-id="a83ef-136">Die [Glimpse HUD Seite](http://getglimpse.com/Docs/Heads-up-Display) die Zeitsteuerungsinformationen oben angezeigten details.</span><span class="sxs-lookup"><span data-stu-id="a83ef-136">The [Glimpse HUD page](http://getglimpse.com/Docs/Heads-up-Display) details the timing information shown above.</span></span> <span data-ttu-id="a83ef-137">Die unaufdringlichen Leistung die HUD angezeigt können ein Problem hinweisen sofort - benachrichtigt werden, bevor Sie auf den Testzyklus zugreifen können.</span><span class="sxs-lookup"><span data-stu-id="a83ef-137">The unobtrusive performance data the HUD displays can notify you of a problem immediately - before you get to the test cycle.</span></span> <span data-ttu-id="a83ef-138">Durch Klicken auf die &quot;g&quot; in der unteren rechten Ecke öffnet die Glimpse Bereich:</span><span class="sxs-lookup"><span data-stu-id="a83ef-138">Clicking on the &quot;g&quot; in the lower right corner brings up the Glimpse panel:</span></span>

![Glimpse Bereich](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

<span data-ttu-id="a83ef-140">In der Abbildung oben die [Registerkarte "Ausführung"](http://getglimpse.com/Docs/Execution-Tab) ausgewählt ist, zeigt die Details der zeitlichen Steuerung der Aktionen und Filter an, in der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="a83ef-140">In the image above, the [Execution tab](http://getglimpse.com/Docs/Execution-Tab) is selected, which shows timing details of the actions and filters in the pipeline.</span></span> <span data-ttu-id="a83ef-141">Sehen Sie meine [Überwachung beenden Filter Timer](http://www.nuget.org/packages/StopWatch/) starten in Phase 6 der Pipeline.</span><span class="sxs-lookup"><span data-stu-id="a83ef-141">You can see my [Stop Watch filter timer](http://www.nuget.org/packages/StopWatch/) start at stage 6 of the pipeline.</span></span> <span data-ttu-id="a83ef-142">Während Meine Lightweight-Zeitgeber bereitstellen, kann nützliche Daten Profil/Timing Cachefehler Zeit aufgewendet hat bei der Autorisierung und Rendern der Ansicht.</span><span class="sxs-lookup"><span data-stu-id="a83ef-142">While my light weight timer can provide useful profile/timing data, it misses all the time spent in authorization and rendering the view.</span></span> <span data-ttu-id="a83ef-143">Informieren Sie sich über Mein Zeitgeber [ein Profil erstellen und die Uhrzeit einer ASP.NET MVC-Anwendung ganz nach Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span><span class="sxs-lookup"><span data-stu-id="a83ef-143">You can read about my timer at [Profile and Time your ASP.NET MVC app all the way to Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx).</span></span> <span data-ttu-id="a83ef-144">Die [Registerkarten](http://getglimpse.com/Docs/Tabs) Seite enthält Links zu ausführlichen Informationen auf jeder Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="a83ef-144">The [Tabs](http://getglimpse.com/Docs/Tabs) page provides links to detailed information on each tab.</span></span>

<a id="Time"></a>
## <a name="the-timeline-tab"></a><span data-ttu-id="a83ef-145">Die Registerkarte "Zeitplan"</span><span class="sxs-lookup"><span data-stu-id="a83ef-145">The Timeline tab</span></span>

<span data-ttu-id="a83ef-146">Ich Tom Dykstras verändert wurde ausstehenden [EF 6/MVC 5-Lernprogramm](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) durch den folgenden Code zu ändern, mit dem Controller Lehrkräfte:</span><span class="sxs-lookup"><span data-stu-id="a83ef-146">I've modified Tom Dykstra's outstanding [EF 6/MVC 5 tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) with the following code change to the instructors controller:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

<span data-ttu-id="a83ef-147">Der obige Code ermöglicht, dass in der Abfragezeichenfolge übergeben (`eager`) Steuerelement eager oder explizite des Ladens der Daten.</span><span class="sxs-lookup"><span data-stu-id="a83ef-147">The code above allows me to pass in query string (`eager`) to control eager or explicit loading of data.</span></span> <span data-ttu-id="a83ef-148">Explizites Laden dient in der folgenden Abbildung und die zeitlichen Steuerung Seite zeigt jede Registrierung geladen werden, der `Index` Aktionsmethode:</span><span class="sxs-lookup"><span data-stu-id="a83ef-148">In the image below, explicit loading is used and the timing page shows each enrollment loaded in the `Index` action method:</span></span>

![Explizites Laden](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

<span data-ttu-id="a83ef-150">Im folgenden Code Eager angegeben wird, und jede Registrierung wird abgerufen, nach der `Index` Ansicht aufgerufen wird:</span><span class="sxs-lookup"><span data-stu-id="a83ef-150">In the following code, eager is specified, and each enrollment is fetched after the `Index` view is called:</span></span>

![Eager angegeben ist](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

<span data-ttu-id="a83ef-152">Sie können ein Zeitsegment abzurufenden ausführliche Zeitsteuerungsinformationen zeigen:</span><span class="sxs-lookup"><span data-stu-id="a83ef-152">You can hover over a time segment to get detailed timing information:</span></span>

![Wenn darauf gezeigt wird, um ausführliche Zeitsteuerungsdaten finden Sie unter](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a><span data-ttu-id="a83ef-154">Modellbindung</span><span class="sxs-lookup"><span data-stu-id="a83ef-154">Model Binding</span></span>

<span data-ttu-id="a83ef-155">Die [Bindung-Registerkarte "Modell"](http://getglimpse.com/Docs/Model-Binding-Tab) bietet eine Fülle von Informationen, um besser zu verstehen, wie Ihre Formularvariablen gebunden sind und warum einige nicht gebunden werden wie zu erwarten.</span><span class="sxs-lookup"><span data-stu-id="a83ef-155">The [model binding tab](http://getglimpse.com/Docs/Model-Binding-Tab) provides a wealth of information to help you understand how your form variables are bound and why some are not bound as would expect.</span></span> <span data-ttu-id="a83ef-156">Die folgende Abbildung zeigt die **?**</span><span class="sxs-lookup"><span data-stu-id="a83ef-156">The image below shows the **?**</span></span> <span data-ttu-id="a83ef-157">Symbol ", die Sie klicken können, um die Glimpse-Hilfeseite für die jeweilige Funktion anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="a83ef-157">icon, which you can click on to bring up the glimpse help page for that feature.</span></span>

![Glimpse modellbindung anzeigen](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a><span data-ttu-id="a83ef-159">Routen</span><span class="sxs-lookup"><span data-stu-id="a83ef-159">Routes</span></span>

 <span data-ttu-id="a83ef-160">Die Registerkarte Glimpse Routen kann können Sie das Debuggen und zu verstehen, routing.</span><span class="sxs-lookup"><span data-stu-id="a83ef-160">The Glimpse Routes tab will can help you debug and understand routing.</span></span> <span data-ttu-id="a83ef-161">In der folgenden Abbildung wird die Produkt-Route ausgewählt ist (und werden in Grün, eine Konvention Glimpse).</span><span class="sxs-lookup"><span data-stu-id="a83ef-161">In the image below, the product route is selected (and it shows in green, a Glimpse convention).</span></span> <span data-ttu-id="a83ef-162">![Produktname ausgewählt](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route Einschränkungen, Bereiche und Daten Token werden ebenfalls angezeigt.</span><span class="sxs-lookup"><span data-stu-id="a83ef-162">![product name selected](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) Route constraints, Areas and data tokens are also displayed.</span></span> <span data-ttu-id="a83ef-163">Finden Sie unter [Glimpse Routen](http://getglimpse.com/Docs/Routes-Tab) und [Routing-Attribut in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) für Weitere Informationen.</span><span class="sxs-lookup"><span data-stu-id="a83ef-163">See [Glimpse Routes](http://getglimpse.com/Docs/Routes-Tab) and [Attribute Routing in ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) for more information.</span></span> 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a><span data-ttu-id="a83ef-164">Mithilfe von Glimpse in Azure</span><span class="sxs-lookup"><span data-stu-id="a83ef-164">Using Glimpse on Azure</span></span>

<span data-ttu-id="a83ef-165">Die Standardsicherheitsrichtlinie Glimpse kann nur Glimpse Daten vom lokalen Host angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="a83ef-165">The Glimpse default security policy only allows Glimpse data to be displayed from local host.</span></span> <span data-ttu-id="a83ef-166">Sie können diese Sicherheitsrichtlinie ändern, damit Sie diese Daten auf einem Remoteserver (z. B. eine Web-app in Azure) anzeigen können.</span><span class="sxs-lookup"><span data-stu-id="a83ef-166">You can change this security policy so you can view this data on a remote server (such as a web app on Azure).</span></span> <span data-ttu-id="a83ef-167">Für testumgebungen in Azure, fügen Sie die hervorgehobenen markieren bis zum Ende der *web.confg* Datei Glimpse zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="a83ef-167">For test environments on Azure, add the highlighted mark up to the bottom of the *web.confg* file to enable Glimpse:</span></span>

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

<span data-ttu-id="a83ef-168">Durch diese Änderung allein kann jeder Benutzer Daten Glimpse an einem Remotestandort sehen.</span><span class="sxs-lookup"><span data-stu-id="a83ef-168">With this change alone, any user can see your Glimpse data on a remote site.</span></span> <span data-ttu-id="a83ef-169">Betrachten Sie das Markup oben auf die Veröffentlichungsprofile hinzufügen, sodass sie nur eine angewendeten bereitgestellt hat bei der Verwendung dieses Veröffentlichungsprofil (z. B. Ihre Azure-Test-Proifle.) Um Einblick Daten zu beschränken, fügen wir der `canViewGlimpseData` Rolle und Benutzer in dieser Rolle zum Anzeigen von Glimpse Daten sind nur zulässig.</span><span class="sxs-lookup"><span data-stu-id="a83ef-169">Consider adding the markup above to a publish profile so it's only deployed an applyed when you use that publish profile (for example, your Azure test proifle.) To restrict Glimpse data, we will add the `canViewGlimpseData` role and only allow users in this role to view Glimpse data.</span></span>

<span data-ttu-id="a83ef-170">Entfernen Sie die Kommentare aus der *GlimpseSecurityPolicy.cs* Datei und ändern Sie die [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) aus aufrufen `Administrator` auf die `canViewGlimpseData` Rolle:</span><span class="sxs-lookup"><span data-stu-id="a83ef-170">Remove the comments from the *GlimpseSecurityPolicy.cs* file and change the [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) call from `Administrator` to the `canViewGlimpseData` role:</span></span>

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> <span data-ttu-id="a83ef-171">Sicherheit – konnte von Glimpse bereitgestellten umfangreichen Daten die Sicherheit Ihrer App verfügbar machen.</span><span class="sxs-lookup"><span data-stu-id="a83ef-171">Security - The rich data provided by Glimpse could expose the security of your app.</span></span> <span data-ttu-id="a83ef-172">Microsoft hat eine sicherheitsüberwachung von Glimpse nicht für die Verwendung auf Produktionen-apps ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="a83ef-172">Microsoft has not performed a security audit of Glimpse for use on productions apps.</span></span>


<span data-ttu-id="a83ef-173">Informationen zum Hinzufügen von Rollen finden Sie unter meinem [Secure ASP.NET MVC 5-Web-app mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure bereitstellen](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="a83ef-173">For information on adding roles, see my [Deploy a Secure ASP.NET MVC 5 web app with Membership, OAuth, and SQL Database to Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="a83ef-174">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="a83ef-174">Additional Resources</span></span>

- [<span data-ttu-id="a83ef-175">Bereitstellen Sie eine sichere ASP.NET MVC 5-app mit Mitgliedschaft, OAuth und SQL-Datenbank in Azure.</span><span class="sxs-lookup"><span data-stu-id="a83ef-175">Deploy a Secure ASP.NET MVC 5 app with Membership, OAuth, and SQL Database to Azure</span></span>](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- <span data-ttu-id="a83ef-176">[Konfiguration Notizenlayouts](http://getglimpse.com/Docs/Configuration) -Doc-Seite zum Konfigurieren von Registerkarten, Laufzeitrichtlinie, Protokollierung und vieles mehr.</span><span class="sxs-lookup"><span data-stu-id="a83ef-176">[Glimpse Configuration](http://getglimpse.com/Docs/Configuration) - Doc page on configuring tabs, runtime policy, logging and more.</span></span>
