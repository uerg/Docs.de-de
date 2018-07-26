---
title: Hosten von ASP.NET Core in Azure App Service
author: guardrex
description: Erfahren Sie, wie Sie ASP.NET Core-Apps in Azure App Service hosten. Entsprechende Informationen werden in diesen nützlichen Ressourcen bereitgestellt.
ms.author: riande
ms.custom: mvc
ms.date: 07/24/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: ece61a3e362ec5e2ff8f415351a0f9257fc72098
ms.sourcegitcommit: b4c7b1a4c48dec0865f27874275c73da1f75e918
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/24/2018
ms.locfileid: "39228610"
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

[Azure Web App-Sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Entdecken Sie die Einschränkungen der Azure App Service-Laufzeitausführung, die durch die Azure Apps-Plattform erzwungen werden.

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a>Anwendungskonfiguration

In ASP.NET Core 2.0 und höher umfassen die folgenden NuGet-Pakete automatische Protokollierungsfeatures für Apps, die für Azure App Service bereitgestellt werden.

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) verwendet [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration), um die ASP.NET Core-Lightup-Integration mit Azure App Service bereitzustellen. Die hinzugefügten Protokollierungsfeatures werden vom `Microsoft.AspNetCore.AzureAppServicesIntegration`-Paket bereitgestellt.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) führt [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) aus, um Anbieter für die Azure App Service-Diagnoseprotokollierung zum Paket `Microsoft.Extensions.Logging.AzureAppServices` hinzuzufügen.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) stellt Protokollierungsimplementierungen bereit, um die Azure App Service-Features für Diagnoseprotokolle und Protokollstreaming zu unterstützen.

Wenn Sie eine Anwendung für .NET Core erstellen und einen Verweis auf das Metapaket [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) herstellen, sind diese Pakete bereits in der Anwendung enthalten. Das neuere Metapaket [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ist nicht in den Paketen enthalten. Wenn Sie eine Anwendung für .NET Framework erstellen oder einen Verweis auf das `Microsoft.AspNetCore.App`-Metapaket herstellen, verweisen Sie auch auf die einzelnen Protokollierungspakete.

::: moniker-end

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxyserver und Lastenausgleichsszenarien

Die Middleware für die Integration von IIS, die ForwardedHeadersMiddleware konfiguriert, und das ASP.NET Core-Modul sind so konfiguriert, dass sie das Schema (HTTP/HTTPS) und die Remote-IP-Adresse an die Stelle weiterleiten, von der die Anforderung stammte. Möglicherweise ist zusätzliche Konfiguration für Apps erforderlich, die hinter weiteren Proxyservern und Lastenausgleichsmodulen (Load Balancer) gehostet werden. Weitere Informationen hierzu feinden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).

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

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>Bereitstellen der ASP.NET Core Vorschauversion für Azure App Service

ASP.NET Core-Vorschau-Apps können mit den folgenden Vorgehensweisen für Azure App Service bereitgestellt werden:

* [Installieren der Websiteerweiterung (Vorschau)](#install-the-preview-site-extension)
* [Bereitstellung der App als eigenständige App](#deploy-the-app-self-contained)
* [Verwenden von Docker mit Web-Apps für Container](#use-docker-with-web-apps-for-containers)

Sollte ein Problem mit dem Verwenden der Vorschau der Websiteerweiterung auftreten, erstellen Sie ein Problem auf [GitHub](https://github.com/aspnet/azureintegration/issues/new).

### <a name="install-the-preview-site-extension"></a>Installieren der Websiteerweiterung (Vorschau)

1. Navigieren Sie im Azure-Portal zum Blatt „App Service“.
1. Wählen Sie die Web-App aus.
1. Geben Sie „ex“ in das Suchfeld ein, oder scrollen Sie in der Liste der Verwaltungsbereiche nach unten bis **ENTWICKLUNGSTOOLS**.
1. Wählen Sie **ENTWICKLUNGSTOOLS** > **Erweiterungen** aus.
1. Wählen Sie **Hinzufügen** aus.

   ![Blatt für Azure-Apps mit vorangehenden Schritten](index/_static/x1.png)

1. Wählen Sie **ASP.NET Core-Erweiterungen** aus.
1. Klicken Sie auf **OK**, um die rechtlichen Bedingungen zu akzeptieren.
1. Wählen Sie **OK** aus, um die Erweiterung zu installieren.

Nach Abschluss der Hinzufügevorgänge wird die neueste .NET Core-Vorschauversion installiert. Überprüfen Sie, ob die Installation erfolgreich war, indem Sie `dotnet --info` in der Konsole ausführen. Führen Sie Folgendes auf dem Blatt **App Service** durch:

1. Geben Sie „con“ in das Suchfeld ein, oder scrollen Sie in der Liste der Verwaltungsbereiche nach unten bis **ENTWICKLUNGSTOOLS**.
1. Wählen Sie **ENTWICKLUNGSTOOLS** > **Konsole** aus.
1. Geben Sie `dotnet --info` in der Konsole ein.

Wenn es sich bei Version `2.1.300-preview1-008174` um die aktuelle Vorschauversion handelt, wird durch Ausführen von `dotnet --info` in der Eingabeaufforderung folgende Ausgabe abgerufen:

![Blatt für Azure-Apps mit vorangehenden Schritten](index/_static/cons.png)

Bei der im vorangehenden Bild dargestellten Version von ASP.NET Core, `2.1.300-preview1-008174`, handelt es sich um ein Beispiel. Die aktuelle Vorschauversion von ASP.NET Core zum Zeitpunkt der Konfiguration der Websiteerweiterung wird angezeigt, wenn Sie `dotnet --info` ausführen.

`dotnet --info` zeigt den Pfad zu der Websiteerweiterung an, wo die Vorschauversion installiert wurde. Es ist zu sehen, dass die App aus der Websiteerweiterung statt aus dem Standardspeicherort *ProgramFiles* ausgeführt wird. Wenn *ProgramFiles* angezeigt wird, starten Sie die Website neu, und führen Sie `dotnet --info` aus.

**Verwenden der Vorschau-Websiteerweiterung mit einer ARM-Vorlage**

Wenn Sie eine ARM-Vorlage zum Erstellen und Bereitstellen von Anwendungen verwenden, können Sie den Ressourcentyp `siteextensions` verwenden, um die Websiteerweiterung zu einer Web-App hinzuzufügen. Zum Beispiel:

[!code-json[Main](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a>Bereitstellen der App als eigenständige App

Eine [eigenständige App](/dotnet/core/deploying/#self-contained-deployments-scd), die die Vorschau-Runtime während der Bereitstellung enthält, kann bereitgestellt werden. Wenn Sie eine eigenständige App bereitstellen, gilt Folgendes:

* Die Website muss nicht vorbereitet werden.
* Die App muss anders als beim Veröffentlichen für eine frameworkabhängige Bereitstellung mit der freigegebenen Runtime und dem Host auf dem Server bereitgestellt werden.

Eigenständige Apps sind eine Option für alle ASP.NET Core-Anwendungen.

### <a name="use-docker-with-web-apps-for-containers"></a>Verwenden von Docker mit Web-Apps für Container

Der [Docker-Hub](https://hub.docker.com/r/microsoft/aspnetcore/) enthält die aktuellen Images für die Docker-Vorschauversion. Die Images können als Basisimage verwendet werden. Verwenden Sie das Image, und führen Sie wie gewohnt eine Bereitstellung für Web-Apps für Container durch.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Web-Apps-Übersicht (fünfminütiges Übersichtsvideo)](/azure/app-service/app-service-web-overview)
* [Azure App Service: The Best Place to Host your .NET Apps (Azure App Service: Ideal zum Hosten von .NET-Apps; 55-minütiges Video)](https://channel9.msdn.com/events/dotnetConf/2017/T222)
* [Azure Friday: Azure App Service Diagnostic and Troubleshooting Experience (Azure Friday: Azure App Service-Diagnose und -Fehlerbehebung; zwölfminütiges Video)](https://channel9.msdn.com/Shows/Azure-Friday/Azure-App-Service-Diagnostic-and-Troubleshooting-Experience)
* [Übersicht: Azure App Service-Diagnose](/azure/app-service/app-service-diagnostics)
* <xref:host-and-deploy/web-farm>

Azure App Service auf Windows Server verwendet [Internetinformationsdienste (IIS)](https://www.iis.net/). Die folgenden Artikel gehen auf die zugrunde liegende IIS-Technologie ein:

* <xref:host-and-deploy/iis/index>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/iis/modules>
* [Microsoft TechNet-Bibliothek: Windows Server](/windows-server/windows-server-versions)
