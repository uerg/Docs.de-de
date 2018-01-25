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
ms.openlocfilehash: 042d20e38a9bc4f1e9792f6e3ff5be11a1eaa882
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
# <a name="aspnet-webhooks-logging"></a>ASP.NET WebHooks, die Protokollierung

Microsoft ASP.NET WebHooks wird die Protokollierung als eine Möglichkeit zum Melden von Problemen verwendet. Standardmäßig werden Protokolle geschrieben, mit [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) verwenden, in dem sie basissicherheit stehen [Ablaufverfolgungslistener](https://msdn.microsoft.com/library/system.diagnostics.tracelistener.aspx) wie alle anderen Protokolldatenstrom.

Wenn Sie Ihre Web-Anwendung als Azure-Web-App bereitstellen, die Protokolle werden automatisch übernommen und verwaltet werden können, zusammen mit anderen [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace) Protokollierung. Weitere Informationen finden Sie unter [Aktivieren der diagnoseprotokollierung für webapps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)

Darüber hinaus können Protokolle abgerufen werden, direkt innerhalb von Visual Studio, wie in beschrieben [Problembehandlung bei einer Web-app in Azure App Service mithilfe von Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="redirecting-logs"></a>Umleiten von Protokollen

Anstatt das Schreiben von Protokollen auf [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace), es ist möglich, eine alternative Protokollierung Implementierung bereitstellen, die direkt an eine Protokoll-Manager, wie z. B. anmelden können [Log4Net](http://logging.apache.org/log4net/) und [NLog ](http://nlog-project.org/). Geben Sie einfach eine Implementierung von [ILogger](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Common/Diagnostics/ILogger.cs) und registrieren Sie ihn mit einem Dependency Injection Modul Ihrer Wahl, und er wird durch Microsoft ASP.NET WebHooks Hintergrundworker abrufen. Finden Sie unter [Abhängigkeitsinjektion in ASP.NET Web API 2](https://www.asp.net/web-api/overview/advanced/dependency-injection) Details.
