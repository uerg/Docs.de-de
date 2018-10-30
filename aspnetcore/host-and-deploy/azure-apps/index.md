---
title: Bereitstellen von ASP.NET Core-Apps in Azure App Service
author: guardrex
description: Dieser Artikel enthält Links zu Azure-Host- und Bereitstellungsressourcen.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: host-and-deploy/azure-apps/index
ms.openlocfilehash: b238630d6f762e2b9fad1060f8150185bcf413fe
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090227"
---
# <a name="deploy-aspnet-core-apps-to-azure-app-service"></a>Bereitstellen von ASP.NET Core-Apps in Azure App Service

[Azure App Service](https://azure.microsoft.com/services/app-service/) ist ein [Microsoft Cloud Computing-Plattformdienst](https://azure.microsoft.com/) zum Hosten von Web-Apps. Dazu gehört auch ASP.NET Core.

## <a name="useful-resources"></a>Nützliche Ressourcen

In der [Web-Apps-Dokumentation](/azure/app-service/) finden Sie die Azure-Apps-Dokumentation, Tutorials, Beispiele und Leitfäden sowie andere Ressourcen. Dies sind zwei erwähnenswerte Tutorials, die auf das Hosten von ASP.NET Core-Apps eingehen:

[Schnellstart: Erstellen von ASP.NET Core-Web-Apps in Azure](/azure/app-service/app-service-web-get-started-dotnet)  
Verwenden Sie Visual Studio, um ASP.NET Core-Web-Apps zu erstellen und in Azure App Service unter Windows bereitzustellen.

[Quickstart: Create a .NET Core web app in App Service on Linux (Schnellstart: Erstellen von .NET Core-Web-Apps in App Service unter Linux)](/azure/app-service/containers/quickstart-dotnetcore)  
Verwenden Sie die Befehlszeile, um ASP.NET Core-Web-Apps zu erstellen und in Azure App Service unter Linux bereitzustellen.

Die folgenden Artikel sind in der ASP.NET Core-Dokumentation verfügbar:

<xref:tutorials/publish-to-azure-webapp-using-vs>  
Erfahren Sie, wie eine ASP.NET Core-App in Azure App Service mit Visual Studio veröffentlicht wird.

<xref:host-and-deploy/azure-apps/azure-continuous-deployment>  
Erfahren Sie, wie Sie mit Visual Studio eine ASP.NET Core-Web-App erstellen und sie unter Verwendung von Git für Continuous Deployment in Azure App Service bereitstellen.

[Erstellen Sie Ihre erste Pipeline mit Azure Pipelines](/azure/devops/pipelines/get-started-yaml)  
Richten Sie ein CI-Build für eine ASP.NET Core-App ein, und erstellen Sie dann ein Continuous Deployment-Release für Azure App Service.

[Azure Web App-Sandbox](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox)  
Entdecken Sie die Einschränkungen der Azure App Service-Laufzeitausführung, die durch die Azure Apps-Plattform erzwungen werden.

::: moniker range=">= aspnetcore-2.0"

## <a name="application-configuration"></a>Anwendungskonfiguration

Die folgenden NuGet-Pakete bieten automatische Protokollierungsfeatures für Apps, die für Azure App Service bereitgestellt werden:

* [Microsoft.AspNetCore.AzureAppServices.HostingStartup](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServices.HostingStartup/) verwendet [IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration), um die ASP.NET Core-Lightup-Integration mit Azure App Service bereitzustellen. Die hinzugefügten Protokollierungsfeatures werden vom `Microsoft.AspNetCore.AzureAppServicesIntegration`-Paket bereitgestellt.
* [Microsoft.AspNetCore.AzureAppServicesIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.AzureAppServicesIntegration/) führt [AddAzureWebAppDiagnostics](/dotnet/api/microsoft.extensions.logging.azureappservicesloggerfactoryextensions.addazurewebappdiagnostics) aus, um Anbieter für die Azure App Service-Diagnoseprotokollierung zum Paket `Microsoft.Extensions.Logging.AzureAppServices` hinzuzufügen.
* [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices/) stellt Protokollierungsimplementierungen bereit, um die Azure App Service-Features für Diagnoseprotokolle und Protokollstreaming zu unterstützen.

Wenn Sie eine Anwendung für .NET Core erstellen und einen Verweis auf das Metapaket [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) herstellen, sind die vorherigen Pakete in der Anwendung enthalten. Das Metapaket [Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) ist nicht in den Paketen enthalten. Wenn Sie eine Anwendung für .NET Framework erstellen oder einen Verweis auf das `Microsoft.AspNetCore.App`-Metapaket herstellen, verweisen Sie auch auf die einzelnen Protokollierungspakete.

::: moniker-end

## <a name="override-app-configuration-using-the-azure-portal"></a>Überschreiben der App-Konfiguration im Azure-Portal

Im Bereich **App-Einstellungen** auf dem Blatt **Anwendungseinstellungen** können Sie die Umgebungsvariablen für die App festlegen. Umgebungsvariablen können von [Umgebungsvariablen-Konfigurationsanbietern](xref:fundamentals/configuration/index#environment-variables-configuration-provider) verarbeitet werden.

Wenn eine App-Einstellung im Azure-Portal erstellt oder geändert und die Schaltfläche **Speichern** ausgewählt wird, wird die Azure-App neu gestartet. Die Umgebungsvariable steht der App nach dem Neustart des Diensts zur Verfügung.

Wenn eine App den [Webhost](xref:fundamentals/host/web-host) verwendet und den Host mit [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) erstellt, verwenden Umgebungsvariablen, die den Host konfigurieren, das Präfix `ASPNETCORE_`. Weitere Informationen finden Sie unter <xref:fundamentals/host/web-host> und unter [Umgebungsvariablen-Konfigurationsanbieter](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

Wenn eine App den [generischen Host](xref:fundamentals/host/generic-host) verwendet, werden Umgebungsvariablen nicht standardmäßig in die App-Konfiguration geladen, und der Konfigurationsanbieter muss vom Entwickler hinzugefügt werden. Der Entwickler legt das Präfix der Umgebungsvariablen fest, wenn der Konfigurationsanbieter hinzugefügt wird. Weitere Informationen finden Sie unter <xref:fundamentals/host/generic-host> und unter [Umgebungsvariablen-Konfigurationsanbieter](xref:fundamentals/configuration/index#environment-variables-configuration-provider).

## <a name="proxy-server-and-load-balancer-scenarios"></a>Proxyserver und Lastenausgleichsszenarien

Die Middleware für die Integration von IIS, die ForwardedHeadersMiddleware konfiguriert, und das ASP.NET Core-Modul sind so konfiguriert, dass sie das Schema (HTTP/HTTPS) und die Remote-IP-Adresse an die Stelle weiterleiten, von der die Anforderung stammte. Möglicherweise ist zusätzliche Konfiguration für Apps erforderlich, die hinter weiteren Proxyservern und Lastenausgleichsmodulen (Load Balancer) gehostet werden. Weitere Informationen hierzu feinden Sie unter [Konfigurieren von ASP.NET Core zur Verwendung mit Proxyservern und Lastenausgleich](xref:host-and-deploy/proxy-load-balancer).

## <a name="monitoring-and-logging"></a>Überwachung und Protokollierung

In den folgenden Artikeln finden Sie Informationen zum Überwachen, Protokollieren und zur Problembehandlung:

[Vorgehensweise: Überwachen von Apps in Azure App Service](/azure/app-service/web-sites-monitor)  
Erfahren Sie, wie Sie Kontingente und Metrik für Apps und App Service-Pläne prüfen.

[Aktivieren der Diagnoseprotokollierung für Apps in Azure App Service](/azure/app-service/web-sites-enable-diagnostic-log)  
Erfahren Sie, wie Sie die Diagnosesprotokollierung für HTTP-Statuscodes, fehlgeschlagene Anforderungen und Webserveraktivitäten aktivieren und auf die Protokolle zugreifen.

<xref:fundamentals/error-handling>  
Lernen Sie gängige Methoden zur Fehlerbehandlung in ASP.NET Core-Apps kennen.

<xref:host-and-deploy/azure-apps/troubleshoot>  
Erfahren Sie, wie Sie Probleme mit Azure App Service-Bereitstellungen mit ASP.NET Core-Apps diagnostizieren können.

<xref:host-and-deploy/azure-iis-errors-reference>  
Sehen Sie sich häufig auftretende Fehler und entsprechende Hinweise zur Fehlerbehebung bei der Bereitstellungskonfiguration für von Azure App Service/IIS gehosteten Apps an.

## <a name="data-protection-key-ring-and-deployment-slots"></a>Data Protection-Schlüsselbund und -Bereitstellungsslots

[Data Protection-Schlüssel](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) werden im Ordner *%HOME%\ASP.NET\DataProtection-Keys* gespeichert. Dieser Ordner wird von einem Netzwerkspeicher unterstützt und mit allen Computern, auf denen die App gehostet wird, synchronisiert. Ruhende Schlüssel werden nicht geschützt. Dieser Ordner stellt den Schlüsselbund für alle Instanzen einer App in einem einzelnen Bereitstellungsslot bereit. Separate Bereitstellungsslots, wie Staging und Produktion, verwenden keinen gemeinsamen Schlüsselbund.

Wenn Sie zwischen Bereitstellungsslots wechseln, können Systeme, die Data Protection verwenden, gespeicherte Daten nicht mit dem Schlüsselbund des vorherigen Slots entschlüsseln. ASP.NET-Cookiemiddleware verwendet Data Protection zum Schutz ihrer Cookies. Dies führt dazu, dass Benutzer in einer App abgemeldet werden, die Standard-ASP.NET-Cookiemiddleware verwendet. Verwende Sie einen externen Schlüsselbundanbieter für eine slotunabhängige Schlüsselbundlösung wie z.B.:

* Azure Blob Storage
* Azure Key Vault
* SQL-Speicher
* Redis Cache

Weitere Informationen finden Sie unter <xref:security/data-protection/implementation/key-storage-providers>.

## <a name="deploy-aspnet-core-preview-release-to-azure-app-service"></a>Bereitstellen der ASP.NET Core Vorschauversion für Azure App Service

Verwenden Sie einen der folgenden Ansätze:

* [Installieren der Websiteerweiterung (Vorschau)](#install-the-preview-site-extension).
* [Bereitstellen der App als eigenständige App](#deploy-the-app-self-contained).
* [Verwenden von Docker mit Web-Apps für Container](#use-docker-with-web-apps-for-containers).

### <a name="install-the-preview-site-extension"></a>Installieren der Websiteerweiterung (Vorschau)

Sollte ein Problem mit dem Verwenden der Vorschau der Websiteerweiterung auftreten, erstellen Sie ein Problem auf [GitHub](https://github.com/aspnet/azureintegration/issues/new).

1. Navigieren Sie aus dem Azure-Portal zum Blatt „App Service“.
1. Wählen Sie die Web-App aus.
1. Geben Sie „ex“ in das Suchfeld ein, oder scrollen Sie in der Liste der Verwaltungsabschnitte bis zu **ENTWICKLUNGSTOOLS** nach unten.
1. Wählen Sie **ENTWICKLUNGSTOOLS** > **Erweiterungen** aus.
1. Wählen Sie **Hinzufügen** aus.
1. Wählen Sie die Erweiterung **ASP.NET Core &lt;x.y&gt; (x86) Runtime** aus der Liste aus. Dabei ist `<x.y>` die ASP.NET Core-Vorschauversion (z.B. **ASP.NET Core 2.2 (x86) Runtime**). Die x86 Runtime eignet sich für [frameworkabhängige Bereitstellungen](/dotnet/core/deploying/#framework-dependent-deployments-fdd), die Out-of-Process-Hosting durch das ASP.NET Core-Modul verwenden.
1. Klicken Sie auf **OK**, um die rechtlichen Bedingungen zu akzeptieren.
1. Wählen Sie **OK** aus, um die Erweiterung zu installieren.

Nach Abschluss dieses Vorgangs wird die neueste .NET Core-Vorschauversion installiert. Überprüfen Sie die Installation:

1. Wählen Sie **Erweiterte Tools** unter **ENTWICKLUNGSTOOLS** aus.
1. Wählen Sie **Start** auf dem Blatt **Erweiterte Tools** aus.
1. Wählen Sie das Menüelement **Debugkonsole** > **PowerShell** aus.
1. Führen Sie in der PowerShell-Eingabeaufforderung den folgenden Befehl aus. Ersetzen Sie im Befehl die ASP.NET Core-Runtimeversion für `<x.y>`:

   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x86\
   ```
   Wenn die installierte Vorschauruntime für ASP.NET Core 2.2 vorgesehen ist, lautet der Befehl folgendermaßen:
   ```powershell
   Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x86\
   ```
   Der Befehl gibt `True` zurück, wenn die x64-Vorschauruntime installiert ist.

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> Die Plattformarchitektur (x86/x64) einer App Services-App wird auf dem Blatt **Anwendungseinstellungen** unter **Allgemeine Einstellungen** für Apps festgelegt, die auf einer Computeebene der A-Serie oder einer besseren Hostingebene gehostet werden. Wenn die Anwendung im In-Process-Modus ausgeführt wird und die Plattformarchitektur für 64-Bit (x64) konfiguriert ist, verwendet das ASP.NET Core-Modul die 64-Bit-Vorschauruntime, falls vorhanden. Installieren Sie die Erweiterung **ASP.NET Core &lt;x.y&gt; (x64) Runtime** (z.B. **ASP.NET Core 2.2 (x64) Runtime**).
>
> Nach der Installation der x64-Vorschauruntime führen Sie den folgenden Befehl im Kudu PowerShell-Befehlsfenster aus, um die Installation zu überprüfen. Ersetzen Sie im Befehl die ASP.NET Core-Runtimeversion für `<x.y>`:
>
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.<x.y>.x64\
> ```
> Wenn die installierte Vorschauruntime für ASP.NET Core 2.2 vorgesehen ist, lautet der Befehl folgendermaßen:
> ```powershell
> Test-Path D:\home\SiteExtensions\AspNetCoreRuntime.2.2.x64\
> ```
> Der Befehl gibt `True` zurück, wenn die x64-Vorschauruntime installiert ist.

::: moniker-end

> [!NOTE]
> **ASP.NET Core-Erweiterungen** aktivieren zusätzliche Funktionen für ASP.NET Core in Azure App Services, z.B. Azure-Protokollierung. Die Erweiterung wird automatisch installiert, wenn die Bereitstellung aus Visual Studio erfolgt. Wenn die Erweiterung nicht installiert ist, installieren Sie sie für die App.

**Verwenden der Vorschau-Websiteerweiterung mit einer ARM-Vorlage**

Wenn Sie eine ARM-Vorlage zum Erstellen und Bereitstellen von Anwendungen verwenden, können Sie den Ressourcentyp `siteextensions` verwenden, um die Websiteerweiterung zu einer Web-App hinzuzufügen. Zum Beispiel:

[!code-json[](index/sample/arm.json?highlight=2)]

### <a name="deploy-the-app-self-contained"></a>Bereitstellen der App als eigenständige App

Eine [eigenständige Bereitstellung](/dotnet/core/deploying/#self-contained-deployments-scd), die auf eine Vorschauruntime abzielt, enthält die Vorschauruntime in der Bereitstellung.

Wenn Sie eine eigenständige App bereitstellen, gilt Folgendes:

* Die Site in Azure App Service erfordert nicht die [Vorschau-Websiteerweiterung](#install-the-preview-site-extension).
* Die App muss gemäß einem anderen Ansatz veröffentlicht werden, als beim Veröffentlichen für eine [frameworkabhängige Bereitstellung](/dotnet/core/deploying#framework-dependent-deployments-fdd).

#### <a name="publish-from-visual-studio"></a>Veröffentlichen aus Visual Studio

1. Wählen Sie in der Visual Studio-Symbolleiste **Erstellen** > **{Anwendungsname} veröffentlichen** aus.
1. Vergewissern Sie sich im Dialogfeld **Veröffentlichungsziel auswählen**, dass **App Service** ausgewählt ist.
1. Wählen Sie **Erweitert** aus. Das Dialogfeld **Veröffentlichen** wird geöffnet.
1. Führen Sie im Dialogfeld **Veröffentlichen** folgende Schritte aus:
   * Überprüfen Sie, ob die Konfiguration **Release** ausgewählt ist.
   * Öffnen Sie die Dropdownliste **Bereitstellungsmodus**, und wählen Sie **Eigenständig** aus.
   * Wählen Sie die Zielruntime in der Dropdownliste **Zielruntime** aus. Die Standardeinstellung ist `win-x86`.
   * Wenn Sie zusätzliche Dateien bei der Bereitstellung entfernen müssen, öffnen Sie **Dateiveröffentlichungsoptionen**, und aktivieren Sie das Kontrollkästchen, um zusätzliche Dateien am Ziel zu entfernen.
   * Klicken Sie auf **Speichern**.
1. Sie können eine neue Website erstellen oder eine vorhandene aktualisieren, indem Sie die folgenden Anweisungen des Veröffentlichungs-Assistenten befolgen.

#### <a name="publish-using-command-line-interface-cli-tools"></a>Veröffentlichen mithilfe des Befehlszeilenschnittstelle-Tools (CLI)

1. Geben Sie in der Projektdatei eine oder mehrere [Runtimebezeichner (RIDs)](/dotnet/core/rid-catalog) an. Verwenden Sie die Singularform `<RuntimeIdentifier>` für eine einzelne RID oder die Pluralform `<RuntimeIdentifiers>`, um eine durch Semikolons getrennte Liste von RIDs bereitzustellen. Im folgenden Beispiel wird die RID `win-x86` angegeben:

   ```xml
   <PropertyGroup>
     <TargetFramework>netcoreapp2.1</TargetFramework>
     <RuntimeIdentifier>win-x86</RuntimeIdentifier>
   </PropertyGroup>
   ```
1. Veröffentlichen Sie aus einer Befehlsshell die App in der Releasekonfiguration für die Runtime des Hosts mit dem Befehl [dotnet publish](/dotnet/core/tools/dotnet-publish). Im folgenden Beispiel wird die App für die RID `win-x86` veröffentlicht. Die für die Option `--runtime` angegebene RID muss in der Eigenschaft `<RuntimeIdentifier>` (oder `<RuntimeIdentifiers>`) in der Projektdatei angegeben werden.

   ```console
   dotnet publish --configuration Release --runtime win-x86
   ```
1. Verschieben Sie den Inhalt des Verzeichnisses *bin/Release/{ZIELFRAMEWORK}/{RUNTIME-ID}/publish* auf die Site in App Service.

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
