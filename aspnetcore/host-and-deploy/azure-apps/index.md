---
title: Hosten von ASP.NET Core in Azure App Service
author: guardrex
description: Erfahren Sie, wie Sie ASP.NET Core-Apps in Azure App Service hosten. Entsprechende Informationen werden in diesen nützlichen Ressourcen bereitgestellt.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: c2675f73880a41ee75f6ec13155419945387e109
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a><span data-ttu-id="ef938-103">Hosten von ASP.NET Core in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ef938-103">Host ASP.NET Core on Azure App Service</span></span>

<span data-ttu-id="ef938-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) ist ein [Microsoft Cloud Computing-Plattformdienst](https://azure.microsoft.com/) zum Hosten von Web-Apps. Dazu gehört auch ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ef938-104">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a [Microsoft cloud computing platform service](https://azure.microsoft.com/) for hosting web apps, including ASP.NET Core.</span></span>

## <a name="useful-resources"></a><span data-ttu-id="ef938-105">Nützliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="ef938-105">Useful resources</span></span>

<span data-ttu-id="ef938-106">In der [Web-Apps-Dokumentation](/azure/app-service/) finden Sie die Azure-Apps-Dokumentation, Tutorials, Beispiele und Leitfäden sowie andere Ressourcen.</span><span class="sxs-lookup"><span data-stu-id="ef938-106">The Azure [Web Apps Documentation](/azure/app-service/) is the home for Azure Apps documentation, tutorials, samples, how-to guides, and other resources.</span></span> <span data-ttu-id="ef938-107">Dies sind zwei erwähnenswerte Tutorials, die auf das Hosten von ASP.NET Core-Apps eingehen:</span><span class="sxs-lookup"><span data-stu-id="ef938-107">Two notable tutorials that pertain to hosting ASP.NET Core apps are:</span></span>

[<span data-ttu-id="ef938-108">Schnellstart: Erstellen von ASP.NET Core-Web-Apps in Azure</span><span class="sxs-lookup"><span data-stu-id="ef938-108">Quickstart: Create an ASP.NET Core web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)  
<span data-ttu-id="ef938-109">Verwenden Sie Visual Studio, um ASP.NET Core-Web-Apps zu erstellen und in Azure App Service unter Windows bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="ef938-109">Use Visual Studio to create and deploy an ASP.NET Core web app to Azure App Service on Windows.</span></span>

[<span data-ttu-id="ef938-110">Quickstart: Create a .NET Core web app in App Service on Linux (Schnellstart: Erstellen von .NET Core-Web-Apps in App Service unter Linux)</span><span class="sxs-lookup"><span data-stu-id="ef938-110">Quickstart: Create a .NET Core web app in App Service on Linux</span></span>](/azure/app-service/containers/quickstart-dotnetcore)  
<span data-ttu-id="ef938-111">Verwenden Sie die Befehlszeile, um ASP.NET Core-Web-Apps zu erstellen und in Azure App Service unter Linux bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="ef938-111">Use the command line to create and deploy an ASP.NET Core web app to Azure App Service on Linux.</span></span>

<span data-ttu-id="ef938-112">Die folgenden Artikel sind in der ASP.NET Core-Dokumentation verfügbar:</span><span class="sxs-lookup"><span data-stu-id="ef938-112">The following articles are available in ASP.NET Core documentation:</span></span>

[<span data-ttu-id="ef938-113">Veröffentlichen in Azure mit Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ef938-113">Publish to Azure with Visual Studio</span></span>](xref:tutorials/publish-to-azure-webapp-using-vs)  
<span data-ttu-id="ef938-114">Erfahren Sie, wie eine ASP.NET Core-App in Azure App Service mit Visual Studio veröffentlicht wird.</span><span class="sxs-lookup"><span data-stu-id="ef938-114">Learn how to publish an ASP.NET Core app to Azure App Service using Visual Studio.</span></span>

[<span data-ttu-id="ef938-115">Veröffentlichen in Azure mit CLI-Tools</span><span class="sxs-lookup"><span data-stu-id="ef938-115">Publish to Azure with CLI tools</span></span>](xref:tutorials/publish-to-azure-webapp-using-cli)  
<span data-ttu-id="ef938-116">Erfahren Sie, wie Sie eine ASP.NET Core-App in Azure App Service mithilfe des Git-Befehlszeilenclients veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="ef938-116">Learn how to publish an ASP.NET Core app to Azure App Service using the Git command-line client.</span></span>

[<span data-ttu-id="ef938-117">Continuous Deployment in Azure mit Visual Studio und Git</span><span class="sxs-lookup"><span data-stu-id="ef938-117">Continuous deployment to Azure with Visual Studio and Git</span></span>](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
<span data-ttu-id="ef938-118">Erfahren Sie, wie Sie mit Visual Studio eine ASP.NET Core-Web-App erstellen und sie unter Verwendung von Git für Continuous Deployment in Azure App Service bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="ef938-118">Learn how to create an ASP.NET Core web app using Visual Studio and deploy it to Azure App Service using Git for continuous deployment.</span></span>

[<span data-ttu-id="ef938-119">Continuous Deployment in Azure mit VSTS</span><span class="sxs-lookup"><span data-stu-id="ef938-119">Continuous deployment to Azure with VSTS</span></span>](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
<span data-ttu-id="ef938-120">Richten Sie ein CI-Build für eine ASP.NET Core-App ein, und erstellen Sie dann ein Continuous Deployment-Release für Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="ef938-120">Set up a CI build for an ASP.NET Core app, then create a continuous deployment release to Azure App Service.</span></span>

[<span data-ttu-id="ef938-121">Azure Web App-Sandbox</span><span class="sxs-lookup"><span data-stu-id="ef938-121">Azure Web App sandbox</span></span>](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
<span data-ttu-id="ef938-122">Entdecken Sie die Einschränkungen der Azure App Service-Laufzeitausführung, die durch die Azure Apps-Plattform erzwungen werden.</span><span class="sxs-lookup"><span data-stu-id="ef938-122">Discover Azure App Service runtime execution limitations enforced by the Azure Apps platform.</span></span>

## <a name="application-configuration"></a><span data-ttu-id="ef938-123">Anwendungskonfiguration</span><span class="sxs-lookup"><span data-stu-id="ef938-123">Application configuration</span></span>

<span data-ttu-id="ef938-124">In ASP.NET Core 2.0 und höher bieten drei Pakete im [Metapaket „Microsoft.AspNetCore.All“](xref:fundamentals/metapackage) automatische Protokollierungsfeatures für in Azure App Service bereitgestellte Apps:</span><span class="sxs-lookup"><span data-stu-id="ef938-124">With ASP.NET Core 2.0 and later, three packages in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) provide automatic logging features for apps deployed to Azure App Service:</span></span>

* <span data-ttu-id="ef938-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) verwendet [IHostingStartup](xref:host-and-deploy/platform-specific-configuration), um die ASP.NET Core-Lightup-Integration mit Azure App Service bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="ef938-125">[Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) uses [IHostingStartup](xref:host-and-deploy/platform-specific-configuration) to provide ASP.NET Core lightup integration with Azure App Service.</span></span> <span data-ttu-id="ef938-126">Die hinzugefügten Protokollierungsfeatures werden vom `Microsoft.AspNetCore.AzureAppServicesIntegration`-Paket bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="ef938-126">The added logging features are provided by the `Microsoft.AspNetCore.AzureAppServicesIntegration` package.</span></span>
* <span data-ttu-id="ef938-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) führt [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) aus, um Anbieter für die Azure App Service-Diagnoseprotokollierung zum Paket `Microsoft.Extensions.Logging.AzureAppServices` hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="ef938-127">[Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) executes [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) to add Azure App Service diagnostics logging providers in the `Microsoft.Extensions.Logging.AzureAppServices` package.</span></span>
* <span data-ttu-id="ef938-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) stellt Protokollierungsimplementierungen bereit, um die Azure App Service-Features für Diagnoseprotokolle und Protokollstreaming zu unterstützen.</span><span class="sxs-lookup"><span data-stu-id="ef938-128">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) provides logger implementations to support Azure App Service diagnostics logs and log streaming features.</span></span>

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="ef938-129">Proxyserver und Lastenausgleichsszenarien</span><span class="sxs-lookup"><span data-stu-id="ef938-129">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="ef938-130">Die Middleware für die Integration von IIS, die ForwardedHeadersMiddleware konfiguriert, und das ASP.NET Core-Modul sind so konfiguriert, dass sie das Schema (HTTP/HTTPS) und die Remote-IP-Adresse an die Stelle weiterleiten, von der die Anforderung stammte.</span><span class="sxs-lookup"><span data-stu-id="ef938-130">The IIS Integration Middleware, which configures Forwarded Headers Middleware, and the ASP.NET Core Module are configured to forward the scheme (HTTP/HTTPS) and the remote IP address where the request originated.</span></span> <span data-ttu-id="ef938-131">Möglicherweise ist zusätzliche Konfiguration für Apps erforderlich, die hinter weiteren Proxyservern und Lastenausgleichsmodulen (Load Balancer) gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="ef938-131">Additional configuration might be required for apps hosted behind additional proxy servers and load balancers.</span></span> <span data-ttu-id="ef938-132">Weitere Informationen hierzu feinden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="ef938-132">For more information, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

## <a name="monitoring-and-logging"></a><span data-ttu-id="ef938-133">Überwachung und Protokollierung</span><span class="sxs-lookup"><span data-stu-id="ef938-133">Monitoring and logging</span></span>

<span data-ttu-id="ef938-134">In den folgenden Artikeln finden Sie Informationen zum Überwachen, Protokollieren und zur Problembehandlung:</span><span class="sxs-lookup"><span data-stu-id="ef938-134">For monitoring, logging, and troubleshooting information, see the following articles:</span></span>

[<span data-ttu-id="ef938-135">Vorgehensweise: Überwachen von Apps in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ef938-135">How to: Monitor Apps in Azure App Service</span></span>](/azure/app-service/web-sites-monitor)  
<span data-ttu-id="ef938-136">Erfahren Sie, wie Sie Kontingente und Metrik für Apps und App Service-Pläne prüfen.</span><span class="sxs-lookup"><span data-stu-id="ef938-136">Learn how to review quotas and metrics for apps and App Service plans.</span></span>

[<span data-ttu-id="ef938-137">Aktivieren der Diagnoseprotokollierung für Apps in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ef938-137">Enable diagnostics logging for web apps in Azure App Service</span></span>](/azure/app-service/web-sites-enable-diagnostic-log)  
<span data-ttu-id="ef938-138">Erfahren Sie, wie Sie die Diagnosesprotokollierung für HTTP-Statuscodes, fehlgeschlagene Anforderungen und Webserveraktivitäten aktivieren und auf die Protokolle zugreifen.</span><span class="sxs-lookup"><span data-stu-id="ef938-138">Discover how to enable and access diagnostic logging for HTTP status codes, failed requests, and web server activity.</span></span>

[<span data-ttu-id="ef938-139">Einführung in die Fehlerbehandlung in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef938-139">Introduction to Error Handling in ASP.NET Core</span></span>](xref:fundamentals/error-handling)  
<span data-ttu-id="ef938-140">Lernen Sie gängige Methoden zur Fehlerbehandlung in ASP.NET Core-Apps kennen.</span><span class="sxs-lookup"><span data-stu-id="ef938-140">Understand common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="ef938-141">Problembehandlung bei ASP.NET Core in Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ef938-141">Troubleshoot ASP.NET Core on Azure App Service</span></span>](xref:host-and-deploy/azure-apps/troubleshoot)  
<span data-ttu-id="ef938-142">Erfahren Sie, wie Sie Probleme mit Azure App Service-Bereitstellungen mit ASP.NET Core-Apps diagnostizieren können.</span><span class="sxs-lookup"><span data-stu-id="ef938-142">Learn how to diagnose issues with Azure App Service deployments with ASP.NET Core apps.</span></span>

[<span data-ttu-id="ef938-143">Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef938-143">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>](xref:host-and-deploy/azure-iis-errors-reference)  
<span data-ttu-id="ef938-144">Sehen Sie sich häufig auftretende Fehler und entsprechende Hinweise zur Fehlerbehebung bei der Bereitstellungskonfiguration für von Azure App Service/IIS gehosteten Apps an.</span><span class="sxs-lookup"><span data-stu-id="ef938-144">See the common deployment configuration errors for apps hosted by Azure App Service/IIS with troubleshooting advice.</span></span>

## <a name="data-protection-key-ring-and-deployment-slots"></a><span data-ttu-id="ef938-145">Data Protection-Schlüsselbund und -Bereitstellungsslots</span><span class="sxs-lookup"><span data-stu-id="ef938-145">Data Protection key ring and deployment slots</span></span>

<span data-ttu-id="ef938-146">[Data Protection-Schlüssel](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) werden im Ordner *%HOME%\ASP.NET\DataProtection-Keys* gespeichert.</span><span class="sxs-lookup"><span data-stu-id="ef938-146">[Data Protection keys](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) are persisted to the *%HOME%\ASP.NET\DataProtection-Keys* folder.</span></span> <span data-ttu-id="ef938-147">Dieser Ordner wird von einem Netzwerkspeicher unterstützt und mit allen Computern, auf denen die App gehostet wird, synchronisiert.</span><span class="sxs-lookup"><span data-stu-id="ef938-147">This folder is backed by network storage and is synchronized across all machines hosting the app.</span></span> <span data-ttu-id="ef938-148">Ruhende Schlüssel werden nicht geschützt.</span><span class="sxs-lookup"><span data-stu-id="ef938-148">Keys aren't protected at rest.</span></span> <span data-ttu-id="ef938-149">Dieser Ordner stellt den Schlüsselbund für alle Instanzen einer App in einem einzelnen Bereitstellungsslot bereit.</span><span class="sxs-lookup"><span data-stu-id="ef938-149">This folder supplies the key ring to all instances of an app in a single deployment slot.</span></span> <span data-ttu-id="ef938-150">Separate Bereitstellungsslots, wie Staging und Produktion, verwenden keinen gemeinsamen Schlüsselbund.</span><span class="sxs-lookup"><span data-stu-id="ef938-150">Separate deployment slots, such as Staging and Production, don't share a key ring.</span></span>

<span data-ttu-id="ef938-151">Wenn Sie zwischen Bereitstellungsslots wechseln, können Systeme, die Data Protection verwenden, gespeicherte Daten nicht mit dem Schlüsselbund des vorherigen Slots entschlüsseln.</span><span class="sxs-lookup"><span data-stu-id="ef938-151">When swapping between deployment slots, any system using data protection won't be able to decrypt stored data using the key ring inside the previous slot.</span></span> <span data-ttu-id="ef938-152">ASP.NET-Cookiemiddleware verwendet Data Protection zum Schutz ihrer Cookies.</span><span class="sxs-lookup"><span data-stu-id="ef938-152">ASP.NET Cookie Middleware uses data protection to protect its cookies.</span></span> <span data-ttu-id="ef938-153">Dies führt dazu, dass Benutzer in einer App abgemeldet werden, die Standard-ASP.NET-Cookiemiddleware verwendet.</span><span class="sxs-lookup"><span data-stu-id="ef938-153">This leads to users being signed out of an app that uses the standard ASP.NET Cookie Middleware.</span></span> <span data-ttu-id="ef938-154">Verwende Sie einen externen Schlüsselbundanbieter für eine slotunabhängige Schlüsselbundlösung wie z.B.:</span><span class="sxs-lookup"><span data-stu-id="ef938-154">For a slot-independent key ring solution, use an external key ring provider, such as:</span></span>

* <span data-ttu-id="ef938-155">Azure Blob Storage</span><span class="sxs-lookup"><span data-stu-id="ef938-155">Azure Blob Storage</span></span>
* <span data-ttu-id="ef938-156">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="ef938-156">Azure Key Vault</span></span>
* <span data-ttu-id="ef938-157">SQL-Speicher</span><span class="sxs-lookup"><span data-stu-id="ef938-157">SQL store</span></span>
* <span data-ttu-id="ef938-158">Redis Cache</span><span class="sxs-lookup"><span data-stu-id="ef938-158">Redis cache</span></span>

<span data-ttu-id="ef938-159">Weitere Informationen finden Sie unter [Schlüsselspeicheranbieter](xref:security/data-protection/implementation/key-storage-providers).</span><span class="sxs-lookup"><span data-stu-id="ef938-159">For more information, see [Key storage providers](xref:security/data-protection/implementation/key-storage-providers).</span></span>

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a><span data-ttu-id="ef938-160">Bereitstellen der ASP.NET Core Vorschauversion für Azure App Service</span><span class="sxs-lookup"><span data-stu-id="ef938-160">Deploy ASP.NET Core preview release to Azure App Service</span></span>

<span data-ttu-id="ef938-161">ASP.NET Core-Vorschau-Apps können mit den folgenden Vorgehensweisen für Azure App Service bereitgestellt werden:</span><span class="sxs-lookup"><span data-stu-id="ef938-161">ASP.NET Core preview apps can be deployed to Azure App Service with the following approaches:</span></span>

* [<span data-ttu-id="ef938-162">Installieren der Vorschau-Website-Erweiterung</span><span class="sxs-lookup"><span data-stu-id="ef938-162">Install the preview site extention</span></span>](#site-x)
* [<span data-ttu-id="ef938-163">Bereitstellen der App als eigenständige App</span><span class="sxs-lookup"><span data-stu-id="ef938-163">Deploy the app self contained</span></span>](#self)
* [<span data-ttu-id="ef938-164">Verwenden von Docker mit Web-Apps für Container</span><span class="sxs-lookup"><span data-stu-id="ef938-164">Use Docker with Web Apps for containers</span></span>](#docker)

<span data-ttu-id="ef938-165">Sollten Sie ein Problem mit dem Verwenden der Vorschau-Websiteerweiterung haben, öffnen Sie ein Problem auf [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span><span class="sxs-lookup"><span data-stu-id="ef938-165">If you have a problem using the preview site extension, open an issue on [GitHub](https://github.com/aspnet/azureintegration/issues/new).</span></span>

<a name="site-x"></a>
### <a name="install-the-preview-site-extention"></a><span data-ttu-id="ef938-166">Installieren der Vorschau-Websiteerweiterung</span><span class="sxs-lookup"><span data-stu-id="ef938-166">Install the preview site extention</span></span>

* <span data-ttu-id="ef938-167">Navigieren Sie im Azure-Portal zum Blatt „App Service“.</span><span class="sxs-lookup"><span data-stu-id="ef938-167">From the Azure portal, navigate to the App Service blade.</span></span>
* <span data-ttu-id="ef938-168">Geben Sie „er“ in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="ef938-168">Enter "ex" in the search box.</span></span>
* <span data-ttu-id="ef938-169">Wählen Sie **Erweiterungen** aus.</span><span class="sxs-lookup"><span data-stu-id="ef938-169">Select **Extensions**.</span></span>
* <span data-ttu-id="ef938-170">Wählen Sie „Hinzufügen“ aus.</span><span class="sxs-lookup"><span data-stu-id="ef938-170">Select "Add".</span></span>

![Blatt für Azure-Apps mit vorangehenden Schritten](index/_static/x1.png)

* <span data-ttu-id="ef938-172">Wählen Sie **ASP.NET Core Runtime Extensions** aus.</span><span class="sxs-lookup"><span data-stu-id="ef938-172">Select **ASP.NET Core Runtime Extensions**.</span></span>
* <span data-ttu-id="ef938-173">Wählen Sie **OK** > **OK** aus.</span><span class="sxs-lookup"><span data-stu-id="ef938-173">Select **OK** > **OK**.</span></span>

<span data-ttu-id="ef938-174">Nach Abschluss der Hinzufügevorgänge wird die neueste .NET Core 2.1-Vorschauversion installiert.</span><span class="sxs-lookup"><span data-stu-id="ef938-174">When the add operations completes, the latest .NET Core 2.1 preview is installed.</span></span> <span data-ttu-id="ef938-175">Sie können die Installation überprüfen, indem Sie `dotnet --info` in der Konsole ausführen.</span><span class="sxs-lookup"><span data-stu-id="ef938-175">You can verify the installation by running `dotnet --info` in the console.</span></span> <span data-ttu-id="ef938-176">Auf dem Blatt „App Service“:</span><span class="sxs-lookup"><span data-stu-id="ef938-176">From the App Service blade:</span></span>

* <span data-ttu-id="ef938-177">Geben Sie „kon“ in das Suchfeld ein.</span><span class="sxs-lookup"><span data-stu-id="ef938-177">Enter "con" in the search box.</span></span>
* <span data-ttu-id="ef938-178">Wählen Sie **Konsole** aus.</span><span class="sxs-lookup"><span data-stu-id="ef938-178">Select **Console**.</span></span>
* <span data-ttu-id="ef938-179">Geben Sie `dotnet --info` in der Konsole ein.</span><span class="sxs-lookup"><span data-stu-id="ef938-179">Enter `dotnet --info` in the console.</span></span>

![Blatt für Azure-Apps mit vorangehenden Schritten](index/_static/cons.png)

<span data-ttu-id="ef938-181">Die vorherige Abbildung war aktuell zu der Zeit, als dieser Text geschrieben wurde.</span><span class="sxs-lookup"><span data-stu-id="ef938-181">The preceding image was current at the time this was written.</span></span> <span data-ttu-id="ef938-182">Sie sehen möglicherweise eine andere Version.</span><span class="sxs-lookup"><span data-stu-id="ef938-182">You may see a different version.</span></span>

<span data-ttu-id="ef938-183">`dotnet --info` zeigt den Pfad zu der Websiteerweiterung an, wo die Vorschauversion installiert wurde.</span><span class="sxs-lookup"><span data-stu-id="ef938-183">The `dotnet --info` displays the the path to the site extension where the Preview has been installed.</span></span> <span data-ttu-id="ef938-184">Es ist zu sehen, dass die App aus der Websiteerweiterung statt aus dem Standardspeicherort *ProgramFiles* ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="ef938-184">It shows the app is running from the site extension instead of from the default *ProgramFiles* location.</span></span> <span data-ttu-id="ef938-185">Wenn *ProgramFiles* angezeigt wird, starten Sie die Website neu, und führen Sie `dotnet --info` aus.</span><span class="sxs-lookup"><span data-stu-id="ef938-185">If you see *ProgramFiles*, restart the site and run `dotnet --info`.</span></span>

#### <a name="use-the-preview-site-extention-with-an-arm-template"></a><span data-ttu-id="ef938-186">Verwenden Vorschau-Websiteerweiterung mit einer ARM-Vorlage</span><span class="sxs-lookup"><span data-stu-id="ef938-186">Use the preview site extention with an ARM template</span></span>

<span data-ttu-id="ef938-187">Wenn Sie eine ARM-Vorlage zum Erstellen und Bereitstellen von Anwendungen verwenden, können Sie den Ressourcentyp `siteextensions` verwenden, um die Websiteerweiterung zu einer Web-App hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="ef938-187">If you are using an ARM template to create and deploy applications you can use the `siteextensions` resource type to add the site extension to a Web App.</span></span> <span data-ttu-id="ef938-188">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="ef938-188">For example:</span></span>

[!code-json[Main](index/sample/arm.json?highlight=2)]

<a name="self"></a>
### <a name="deploy-the-app-self-contained"></a><span data-ttu-id="ef938-189">Bereitstellen der App als eigenständige App</span><span class="sxs-lookup"><span data-stu-id="ef938-189">Deploy the app self contained</span></span>

<span data-ttu-id="ef938-190">Sie können eine [eigenständige App](/dotnet/core/deploying/#self-contained-deployments-scd) bereitstellen, die die Vorschau-Runtime enthält, wenn sie bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="ef938-190">You can deploy a [self-contained app](/dotnet/core/deploying/#self-contained-deployments-scd) that carries the preview runtime with it when being deployed.</span></span> <span data-ttu-id="ef938-191">Wenn Sie eine eigenständige App bereitstellen, gilt Folgendes:</span><span class="sxs-lookup"><span data-stu-id="ef938-191">When deploying a self contained app:</span></span>

* <span data-ttu-id="ef938-192">Sie müssen Ihre Website nicht vorbereiten.</span><span class="sxs-lookup"><span data-stu-id="ef938-192">You don’t need to prepare your site.</span></span>
* <span data-ttu-id="ef938-193">Sie müssen Ihre Anwendung anders veröffentlichen, als dies der Fall wäre, wenn Sie eine App bereitstellen, nachdem das SDK auf dem Server installiert ist.</span><span class="sxs-lookup"><span data-stu-id="ef938-193">Requires you to publish your application differently than you would when deploying an app once the SDK is installed on the server.</span></span>

<span data-ttu-id="ef938-194">Eigenständige Apps sind eine Option für alle .NET Core-Anwendungen.</span><span class="sxs-lookup"><span data-stu-id="ef938-194">Self-contained apps are an option for all .NET Core applications.</span></span>

<a name="docker"></a>
### <a name="use-docker-with-web-apps-for-containers"></a><span data-ttu-id="ef938-195">Verwenden von Docker mit Web-Apps für Container</span><span class="sxs-lookup"><span data-stu-id="ef938-195">Use Docker with Web Apps for containers</span></span>

<span data-ttu-id="ef938-196">Der [Docker-Hub](https://hub.docker.com/r/microsoft/aspnetcore/) enthält die neuesten Images für 2.1-Vorschau-Docker.</span><span class="sxs-lookup"><span data-stu-id="ef938-196">The [Docker Hub](https://hub.docker.com/r/microsoft/aspnetcore/) contains the latest 2.1 preview Docker images.</span></span> <span data-ttu-id="ef938-197">Sie können diese als Basisimages verwenden und ganz normal in Web-Apps für Container bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="ef938-197">You can use them as your base image and deploy to Web Apps for Containers as you normally would.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ef938-198">Zusätzliche Ressourcen</span><span class="sxs-lookup"><span data-stu-id="ef938-198">Additional resources</span></span>

* [<span data-ttu-id="ef938-199">Web-Apps-Übersicht (fünfminütiges Übersichtsvideo)</span><span class="sxs-lookup"><span data-stu-id="ef938-199">Web Apps overview (5-minute overview video)</span></span>](/azure/app-service/app-service-web-overview)
* [<span data-ttu-id="ef938-200">Azure App Service: The Best Place to Host your .NET Apps (Azure App Service: Ideal zum Hosten von .NET-Apps; 55-minütiges Video)</span><span class="sxs-lookup"><span data-stu-id="ef938-200">Azure App Service: The Best Place to Host your .NET Apps (55-minute overview video)</span></span>](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [<span data-ttu-id="ef938-201">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (Azure Friday: Azure App Service-Diagnose und -Fehlerbehebung; zwölfminütiges Video)</span><span class="sxs-lookup"><span data-stu-id="ef938-201">Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (12-minute video)</span></span>](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [<span data-ttu-id="ef938-202">Übersicht: Azure App Service-Diagnose</span><span class="sxs-lookup"><span data-stu-id="ef938-202">Azure App Service diagnostics overview</span></span>](/azure/app-service/app-service-diagnostics)

<span data-ttu-id="ef938-203">Azure App Service auf Windows Server verwendet [Internetinformationsdienste (IIS)](https://www.iis.net/).</span><span class="sxs-lookup"><span data-stu-id="ef938-203">Azure App Service on Windows Server uses [Internet Information Services (IIS)](https://www.iis.net/).</span></span> <span data-ttu-id="ef938-204">Die folgenden Artikel gehen auf die zugrunde liegende IIS-Technologie ein:</span><span class="sxs-lookup"><span data-stu-id="ef938-204">The following topics pertain to the underlying IIS technology:</span></span>

* [<span data-ttu-id="ef938-205">Hosten von ASP.NET Core unter Windows mit IIS</span><span class="sxs-lookup"><span data-stu-id="ef938-205">Host ASP.NET Core on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="ef938-206">Einführung in das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="ef938-206">Introduction to ASP.NET Core Module</span></span>](xref:fundamentals/servers/aspnet-core-module)
* [<span data-ttu-id="ef938-207">Konfigurationsreferenz für das ASP.NET Core-Modul</span><span class="sxs-lookup"><span data-stu-id="ef938-207">ASP.NET Core Module configuration reference</span></span>](xref:host-and-deploy/aspnet-core-module)
* [<span data-ttu-id="ef938-208">IIS-Module mit ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ef938-208">IIS Modules with ASP.NET Core</span></span>](xref:host-and-deploy/iis/modules)
* [<span data-ttu-id="ef938-209">Microsoft TechNet-Bibliothek: Windows Server</span><span class="sxs-lookup"><span data-stu-id="ef938-209">Microsoft TechNet Library: Windows Server</span></span>](/windows-server/windows-server-versions)
