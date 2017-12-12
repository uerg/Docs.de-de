---
uid: webhooks/diagnostics/logging
title: ASP.NET WebHooks Protokollierung | Microsoft Docs
author: rick-anderson
description: Vorgehensweise in ASP.NET WebHooks Protokollierung.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: f71bc442-5f80-481b-a32c-a0ec18dee9d6
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 5691d5c394b4e606282ba302953e8a06e7d73860
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-logging"></a><span data-ttu-id="8e1db-103">ASP.NET WebHooks, die Protokollierung</span><span class="sxs-lookup"><span data-stu-id="8e1db-103">ASP.NET WebHooks logging</span></span>

<span data-ttu-id="8e1db-104">Microsoft ASP.NET WebHooks wird die Protokollierung als eine Möglichkeit zum Melden von Problemen verwendet.</span><span class="sxs-lookup"><span data-stu-id="8e1db-104">Microsoft ASP.NET WebHooks uses logging as a way of reporting issues and problems.</span></span> <span data-ttu-id="8e1db-105">Standardmäßig werden Protokolle geschrieben, mit [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) verwenden, in dem sie basissicherheit stehen [Ablaufverfolgungslistener](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx) wie alle anderen Protokolldatenstrom.</span><span class="sxs-lookup"><span data-stu-id="8e1db-105">By default logs are written using [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) where they can be manged using [Trace Listeners](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx) like any other log stream.</span></span>

<span data-ttu-id="8e1db-106">Wenn Sie Ihre Web-Anwendung als Azure-Web-App bereitstellen, die Protokolle werden automatisch übernommen und verwaltet werden können, zusammen mit anderen [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) Protokollierung.</span><span class="sxs-lookup"><span data-stu-id="8e1db-106">When deploying your Web Application as an Azure Web App, the logs are automatically picked up and can be managed together with any other [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) logging.</span></span> <span data-ttu-id="8e1db-107">Weitere Informationen finden Sie unter [Aktivieren der diagnoseprotokollierung für webapps in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)</span><span class="sxs-lookup"><span data-stu-id="8e1db-107">For details, please see [Enable diagnostics logging for web apps in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)</span></span>

<span data-ttu-id="8e1db-108">Darüber hinaus können Protokolle abgerufen werden, direkt innerhalb von Visual Studio, wie in beschrieben [Problembehandlung bei einer Web-app in Azure App Service mithilfe von Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span><span class="sxs-lookup"><span data-stu-id="8e1db-108">In addition, logs can be obtained straight from inside Visual Studio as described in [Troubleshoot a web app in Azure App Service using Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).</span></span>

## <a name="redirecting-logs"></a><span data-ttu-id="8e1db-109">Umleiten von Protokollen</span><span class="sxs-lookup"><span data-stu-id="8e1db-109">Redirecting Logs</span></span>

<span data-ttu-id="8e1db-110">Anstatt das Schreiben von Protokollen auf [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace), es ist möglich, eine alternative Protokollierung Implementierung bereitstellen, die direkt an eine Protokoll-Manager, wie z. B. anmelden können [Log4Net](http://logging.apache.org/log4net/) und [NLog ](http://nlog-project.org/).</span><span class="sxs-lookup"><span data-stu-id="8e1db-110">Instead of writing logs to [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace), it is possible to provide an alternate logging implementation that can log directly to a log manager such as [Log4Net](http://logging.apache.org/log4net/) and [NLog](http://nlog-project.org/).</span></span> <span data-ttu-id="8e1db-111">Geben Sie einfach eine Implementierung von [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) und registrieren Sie ihn mit einem Dependency Injection Modul Ihrer Wahl, und er wird durch Microsoft ASP.NET WebHooks Hintergrundworker abrufen.</span><span class="sxs-lookup"><span data-stu-id="8e1db-111">Simply provide an implementation of [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) and register it with a dependency injection engine of your choice and it will get picked up by Microsoft ASP.NET WebHooks.</span></span> <span data-ttu-id="8e1db-112">Finden Sie unter [Abhängigkeitsinjektion in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) Details.</span><span class="sxs-lookup"><span data-stu-id="8e1db-112">Please see [Dependency Injection in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) for details.</span></span>
