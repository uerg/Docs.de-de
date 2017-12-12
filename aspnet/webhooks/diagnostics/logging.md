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
# <a name="aspnet-webhooks-logging"></a>ASP.NET WebHooks, die Protokollierung

Microsoft ASP.NET WebHooks wird die Protokollierung als eine Möglichkeit zum Melden von Problemen verwendet. Standardmäßig werden Protokolle geschrieben, mit [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) verwenden, in dem sie basissicherheit stehen [Ablaufverfolgungslistener](https://msdn.microsoft.com/en-us/library/system.diagnostics.tracelistener.aspx) wie alle anderen Protokolldatenstrom.

Wenn Sie Ihre Web-Anwendung als Azure-Web-App bereitstellen, die Protokolle werden automatisch übernommen und verwaltet werden können, zusammen mit anderen [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace) Protokollierung. Weitere Informationen finden Sie unter [Aktivieren der diagnoseprotokollierung für webapps in Azure App Service](https://azure.microsoft.com/en-us/documentation/articles/web-sites-enable-diagnostic-log/)

Darüber hinaus können Protokolle abgerufen werden, direkt innerhalb von Visual Studio, wie in beschrieben [Problembehandlung bei einer Web-app in Azure App Service mithilfe von Visual Studio](https://azure.microsoft.com/en-us/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Umleiten von Protokollen

Anstatt das Schreiben von Protokollen auf [System.Diagnostics.Trace](https://msdn.microsoft.com/en-us/library/system.diagnostics.trace), es ist möglich, eine alternative Protokollierung Implementierung bereitstellen, die direkt an eine Protokoll-Manager, wie z. B. anmelden können [Log4Net](http://logging.apache.org/log4net/) und [NLog ](http://nlog-project.org/). Geben Sie einfach eine Implementierung von [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) und registrieren Sie ihn mit einem Dependency Injection Modul Ihrer Wahl, und er wird durch Microsoft ASP.NET WebHooks Hintergrundworker abrufen. Finden Sie unter [Abhängigkeitsinjektion in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) Details.
