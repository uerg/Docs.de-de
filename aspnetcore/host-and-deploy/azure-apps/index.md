---
title: Hosten von ASP.NET Core in Azure App Service
author: guardrex
description: "Erfahren Sie, wie Sie ASP.NET Core-Apps in Azure App Service hosten. Entsprechende Informationen werden in diesen nützlichen Ressourcen bereitgestellt."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/29/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: 34b009d3a298803256c9a06debe6e5026418429a
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/03/2018
---
# <a name="host-aspnet-core-on-azure-app-service"></a>Hosten von ASP.NET Core in Azure App Service

[Azure App Service](https://azure.microsoft.com/services/app-service/) ist ein [Microsoft Cloud Computing-Plattformdienst](https://azure.microsoft.com/) zum Hosten von Web-Apps. Dazu gehört auch ASP.NET Core.

## <a name="useful-resources"></a>Nützliche Ressourcen

In der [Web-Apps-Dokumentation](/azure/app-service/) finden Sie die Azure-Apps-Dokumentation, Tutorials, Beispiele und Leitfäden sowie andere Ressourcen. Dies sind zwei erwähnenswerte Tutorials, die auf das Hosten von ASP.NET Core-Apps eingehen:

[Schnellstart: Erstellen von ASP.NET Core-Web-Apps in Azure](/azure/app-service/app-service-web-get-started-dotnet)  
Verwenden Sie Visual Studio, um ASP.NET Core-Web-Apps zu erstellen und in Azure App Service unter Windows bereitzustellen.

[Quickstart: Create a .NET Core web app in App Service on Linux (Schnellstart: Erstellen von .NET Core-Web-Apps in App Service unter Linux)](/azure/app-service/containers/quickstart-dotnetcore)  
Verwenden Sie die Befehlszeile, um ASP.NET Core-Web-Apps zu erstellen und in Azure App Service unter Linux bereitzustellen.

Die folgenden Artikel sind in der ASP.NET Core-Dokumentation verfügbar:

[Veröffentlichen in Azure mit Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)  
Erfahren Sie, wie eine ASP.NET Core-App in Azure App Service mit Visual Studio veröffentlicht wird.

[Veröffentlichen in Azure mit CLI-Tools](xref:tutorials/publish-to-azure-webapp-using-cli)  
Erfahren Sie, wie Sie eine ASP.NET Core-App in Azure App Service mithilfe des Git-Befehlszeilenclients veröffentlichen.

[Continuous Deployment in Azure mit Visual Studio und Git](xref:host-and-deploy/azure-apps/azure-continuous-deployment)  
Erfahren Sie, wie Sie mit Visual Studio eine ASP.NET Core-Web-App erstellen und sie unter Verwendung von Git für Continuous Deployment in Azure App Service bereitstellen.

[Continuous Deployment in Azure mit VSTS](https://www.visualstudio.com/docs/build/aspnet/core/quick-to-azure)  
Richten Sie ein CI-Build für eine ASP.NET Core-App ein, und erstellen Sie dann ein Continuous Deployment-Release für Azure App Service.

## <a name="application-configuration"></a>Anwendungskonfiguration

In ASP.NET Core 2.0 und höher bieten drei Pakete im [Metapaket „Microsoft.AspNetCore.All“](xref:fundamentals/metapackage) automatische Protokollierungsfeatures für in Azure App Service bereitgestellte Apps:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) verwendet [IHostingStartup](xref:host-and-deploy/ihostingstartup), um die ASP.NET Core-Lightup-Integration mit Azure App Service bereitzustellen. Die hinzugefügten Protokollierungsfeatures werden vom `Microsoft.AspNetCore.AzureAppServicesIntegration`-Paket bereitgestellt.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) führt [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) aus, um Anbieter für die Azure App Service-Diagnoseprotokollierung zum Paket `Microsoft.Extensions.Logging.AzureAppServices` hinzuzufügen.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) stellt Protokollierungsimplementierungen bereit, um die Azure App Service-Features für Diagnoseprotokolle und Protokollstreaming zu unterstützen.

## <a name="monitoring-and-logging"></a>Überwachung und Protokollierung

In den folgenden Artikeln finden Sie Informationen zum Überwachen, Protokollieren und zur Problembehandlung:

[Vorgehensweise: Überwachen von Apps in Azure App Service](/azure/app-service/web-sites-monitor)  
Erfahren Sie, wie Sie Kontingente und Metrik für Apps und App Service-Pläne prüfen.

[Aktivieren der Diagnoseprotokollierung für Apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log)  
Erfahren Sie, wie Sie die Diagnosesprotokollierung für HTTP-Statuscodes, fehlgeschlagene Anforderungen und Webserveraktivitäten aktivieren und auf die Protokolle zugreifen.

[Einführung in die Fehlerbehandlung in ASP.NET Core](xref:fundamentals/error-handling)  
Lernen Sie gängige Methoden zur Fehlerbehandlung in ASP.NET Core-Apps kennen.

[Problembehandlung bei ASP.NET Core in Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)  
Erfahren Sie, wie Sie Probleme mit Azure App Service-Bereitstellungen mit ASP.NET Core-Apps diagnostizieren können.

[Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)  
Sehen Sie sich häufig auftretende Fehler und entsprechende Hinweise zur Fehlerbehebung bei der Bereitstellungskonfiguration für von Azure App Service/IIS gehosteten Apps an.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Data Protection-Schlüsselbund und -Bereitstellungsslots

[Data Protection-Schlüssel](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) werden im Ordner *%HOME%\ASP.NET\DataProtection-Keys* gespeichert. Dieser Ordner wird von einem Netzwerkspeicher unterstützt und mit allen Computern, auf denen die App gehostet wird, synchronisiert. Ruhende Schlüssel werden nicht geschützt. Dieser Ordner stellt den Schlüsselbund für alle Instanzen einer App in einem einzelnen Bereitstellungsslot bereit. Separate Bereitstellungsslots, wie Staging und Produktion, verwenden keinen gemeinsamen Schlüsselbund.

Wenn Sie zwischen Bereitstellungsslots wechseln, können Systeme, die Data Protection verwenden, gespeicherte Daten nicht mit dem Schlüsselbund des vorherigen Slots entschlüsseln. ASP.NET-Cookiemiddleware verwendet Data Protection zum Schutz ihrer Cookies. Dies führt dazu, dass Benutzer in einer App abgemeldet werden, die Standard-ASP.NET-Cookiemiddleware verwendet. Verwende Sie einen externen Schlüsselbundanbieter für eine slotunabhängige Schlüsselbundlösung wie z.B.:

* Azure Blob Storage
* Azure Key Vault
* SQL-Speicher
* Redis Cache

Weitere Informationen finden Sie unter [Schlüsselspeicheranbieter](xref:security/data-protection/implementation/key-storage-providers).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Web-Apps-Übersicht (fünfminütiges Übersichtsvideo)](/azure/app-service/app-service-web-overview)
* [Azure App Service: The Best Place to Host your .NET Apps (Azure App Service: Ideal zum Hosten von .NET-Apps; 55-minütiges Video)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (Azure Friday: Azure App Service-Diagnose und -Fehlerbehebung; zwölfminütiges Video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Übersicht: Azure App Service-Diagnose](/azure/app-service/app-service-diagnostics)

Azure App Service auf Windows Server verwendet [Internetinformationsdienste (IIS)](https://www.iis.net/). Die folgenden Artikel gehen auf die zugrunde liegende IIS-Technologie ein:

* [Hosten von ASP.NET Core unter Windows mit IIS](xref:host-and-deploy/iis/index)
* [Einführung in das ASP.NET Core-Modul](xref:fundamentals/servers/aspnet-core-module)
* [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module)
* [Verwenden von IIS-Modulen mit ASP.NET Core](xref:host-and-deploy/iis/modules)
* [Microsoft TechNet-Bibliothek: Windows Server](https://docs.microsoft.com/windows-server/windows-server-versions)
