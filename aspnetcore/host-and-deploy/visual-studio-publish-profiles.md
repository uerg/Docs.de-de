---
title: Visual Studio die Veröffentlichungsprofile für ASP.NET Core-app-Bereitstellung
author: rick-anderson
description: Informationen zum Erstellen in Visual Studio die Veröffentlichungsprofile und verwenden sie zum Verwalten von ASP.NET Core-app-Bereitstellungen an verschiedene Ziele.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Visual Studio die Veröffentlichungsprofile für ASP.NET Core-app-Bereitstellung

Von [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieses Dokument konzentriert sich auf das Erstellen und Verwenden von Visual Studio 2017 mit Veröffentlichungsprofile. Die Veröffentlichungsprofile, die mit Visual Studio erstellt werden, können von MSBuild und Visual Studio 2017 ausgeführt werden. Anweisungen zum Veröffentlichen in Azure finden Sie unter [Veröffentlichen einer ASP.NET Core-Web-App in Azure App Service mit Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).

Die folgende Projektdatei erstellt wurde, mit dem Befehl `dotnet new mvc`:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

Die `<Project>` des Elements `Sdk` Attribut führt die folgenden Aufgaben:

* Importiert die Eigenschaftendatei aus *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* am Anfang.
* Es importiert am Ende die Zieledatei von *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*.

Der Standardspeicherort für `MSBuildSDKsPath` (mit Visual Studio 2017 Enterprise) ist der Ordner *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

Die `Microsoft.NET.Sdk.Web` SDK abhängig:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Dadurch werden die folgenden Eigenschaften und Ziele, die importiert werden:

* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*
* *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*

Veröffentlichen Sie Targets-Import, legen Sie rechts von Zielen, die auf der Grundlage der Publish-Methode verwendet.

Wenn MSBuild oder Visual Studio ein Projekt geladen wird, werden folgende allgemeinen Aktionen ausgeführt:

* Erstellen des Projekts
* Berechnen der zu veröffentlichenden Dateien
* Veröffentlichen der Dateien auf dem Ziel

## <a name="compute-project-items"></a>Berechnen der Projektelemente

Wenn das Projekt geladen ist, werden die Projektelemente (Dateien) berechnet. Die `item type`-Attribut bestimmt, wie die Datei verarbeitet wird. Standardmäßig sind *CS*-Dateien in der `Compile`-Elementliste enthalten. Dateien in der `Compile`-Elementliste werden kompiliert.

Die `Content` Elementliste enthält Dateien, die zusätzlich zu den Buildausgaben veröffentlicht werden. Standardmäßig Dateien gemäß dem Muster `wwwroot/**` befinden sich die `Content` Element. Die `wwwroot/\*\*` [Globmodus Muster](https://gruntjs.com/configuring-tasks#globbing-patterns) entspricht allen Dateien in der *"Wwwroot"* Ordner **und** Unterordner. Um die Veröffentlichungsliste explizit eine Datei hinzuzufügen, fügen Sie die Datei direkt in die *csproj* Datei wie gezeigt in [Includedateien](#include-files).

Bei der Auswahl der **veröffentlichen** Schaltfläche in Visual Studio oder beim Veröffentlichen von der Befehlszeile aus:

* Die Eigenschaften und Elemente werden berechnet (die Dateien, die für den Build benötigt werden).
* **Visual Studio nur**: NuGet-Pakete werden wiederhergestellt. (Die Wiederherstellung muss explizit vom Benutzer auf der CLI durchgeführt werden.)
* Das Projekt wird erstellt.
* Die zu veröffentlichenden Elemente werden berechnet (die Dateien, die für die Veröffentlichung benötigt werden).
* Das Projekt veröffentlicht wird (die berechnete Dateien werden in der Veröffentlichung kopiert).

Wenn eine ASP.NET Core-Projekt verweist auf `Microsoft.NET.Sdk.Web` in der Projektdatei ein *app_offline.htm* Datei befindet sich im Stammverzeichnis des Verzeichnisses der Web-app. Wenn die Datei vorhanden ist, fährt das ASP.NET Core Module die App ordnungsgemäß herunter und verarbeitet die Datei *app_offline.htm* während der Bereitstellung. Weitere Informationen finden Sie unter [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).

## <a name="basic-command-line-publishing"></a>Grundlegende Befehlszeile veröffentlichen

Befehlszeilen-Veröffentlichung funktioniert auf allen Plattformen mit .NET Core-Unterstützung und erfordert nicht das Visual Studio. In den Beispielen unten die [Dotnet veröffentlichen](/dotnet/core/tools/dotnet-publish) aus dem Projektverzeichnis ausgeführt wird (enthält die *csproj* Datei). Wenn dies nicht explizit in den Projektordner übergeben, in der Projektdateipfad. Zum Beispiel:

```console
dotnet publish C:\Webs\Web1
```

Führen Sie die folgenden Befehle zum Erstellen und Veröffentlichen eine Web-App aus:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

Die [Dotnet veröffentlichen](/dotnet/core/tools/dotnet-publish) Befehl erzeugt eine Ausgabe ähnlich der folgenden:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

Der Standardveröffentlichungsordner ist `bin\$(Configuration)\netcoreapp<version>\publish`. Die Standardeinstellung für `$(Configuration)` ist *Debuggen*. Im vorhergehenden Beispiel die `<TargetFramework>` ist `netcoreapp2.0`.

`dotnet publish -h` zeigt Hilfeinformationen zum Veröffentlichen an.

Der folgende Befehl gibt einen `Release`-Build und das Veröffentlichungsverzeichnis an:

```console
dotnet publish -c Release -o C:\MyWebs\test
```

Die [Dotnet veröffentlichen](/dotnet/core/tools/dotnet-publish) Befehl ruft MSBuild das aufruft der `Publish` Ziel. Übergeben von Parametern `dotnet publish` an MSBuild übergeben werden. Der Parameter `-c` wird der MSBuild-Eigenschaft `Configuration` zugeordnet. Der Parameter `-o` wird `OutputPath` zugeordnet.

MSBuild-Eigenschaften können mithilfe einer der folgenden Formate übergeben werden:

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Der folgende Befehl veröffentlicht einen `Release`-Build auf einer Netzwerkfreigabe:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Die Netzwerkfreigabe wird durch führende Schrägstriche (*//r8/*) angegeben und funktioniert auf allen Plattformen, die von .NET Core unterstützt werden.

Versichern Sie sich, dass die veröffentlichte App für die Bereitstellung nicht ausgeführt wird. Die Dateien im Ordner *publish* sind gesperrt, wenn die App ausgeführt wird. Die Bereitstellung kann nicht erfolgen, da die gesperrten Dateien nicht kopiert werden können.

## <a name="publish-profiles"></a>Veröffentlichungsprofile

Dieser Abschnitt verwendet Visual Studio 2017 ein Veröffentlichungsprofil zu erstellen. Nach der Erstellung ist die Veröffentlichung aus Visual Studio oder der Befehlszeile zur Verfügung.

Veröffentlichen von Profilen können während der Veröffentlichung vereinfachen, und eine beliebige Anzahl von Profilen kann vorhanden sein. Erstellen eines Veröffentlichungsprofils in Visual Studio durch Auswahl einer der folgenden Pfade:

* Mit der rechten Maustaste des Projekts im Projektmappen-Explorer, und wählen Sie **veröffentlichen**.
* Wählen Sie **veröffentlichen &lt;Project_name&gt;**  aus der **erstellen** Menü.

Die **veröffentlichen** Registerkarte der Seite "app Kapazitäten" wird angezeigt. Wenn das Projekt ein Veröffentlichungsprofil fehlt, wird die folgende Seite angezeigt:

![Die Registerkarte "Veröffentlichen" auf der Seite "app Kapazitäten"](visual-studio-publish-profiles/_static/az.png)

Wenn **Ordner** ist ausgewählt, geben Sie einen Ordnerpfad ein, um die veröffentlichten Objekte zu speichern. Der Standardordner ist *Bin\Release\PublishOutput*. Klicken Sie auf die **Profil erstellen** Schaltfläche, um den Vorgang abzuschließen.

Sobald ein Veröffentlichungsprofil erstellt wurde, die **veröffentlichen** Registerkarte ändert. Das neu erstellte Profil wird in einer Dropdownliste angezeigt. Klicken Sie auf **neues Profil erstellen** ein anderes neues Profil erstellen.

![Die Registerkarte "Veröffentlichen" mit FolderProfile Seite "app Kapazitäten"](visual-studio-publish-profiles/_static/create_new.png)

Der Webpublishing-Assistent unterstützt folgende Veröffentlichungsziele:

* Azure App Service
* Azure Virtual Machines
* IIS, FTP usw. (für alle Webserver)
* Ordner
* Profil importieren

Weitere Informationen finden Sie unter [welche Veröffentlichungsoptionen für mich geeignet sind](/visualstudio/ide/not-in-toc/web-publish-options).

Beim Erstellen eines Veröffentlichungsprofils mit Visual Studio eine *Eigenschaften/PublishProfiles/&lt;Profile_name&gt;pubxml* MSBuild-Datei wird erstellt. Die *pubxml* -Datei ist eine MSBuild-Datei und enthält Konfigurationseinstellungen zu veröffentlichen. Diese Datei kann geändert werden, um den Build anpassen und Veröffentlichen von Prozess. Diese Datei wird vom Veröffentlichungsprozess gelesen. `<LastUsedBuildConfiguration>` ist ein Sonderfall, da er eine globale Eigenschaft und sollte nicht in jeder Datei, die im Build importiert werden. Finden Sie unter [MSBuild: Gewusst wie: festlegen die Konfigurationseigenschaft](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) für Weitere Informationen.

Beim Veröffentlichen in einer Azure-Ziel der *pubxml* -Datei enthält Ihre Azure-Abonnement-ID. Mit diesem Zieltyp wird diese Datei zur quellcodeverwaltung hinzufügen abgeraten. Beim Veröffentlichen auf einem nicht-Azure-Ziel ist sicher ist, überprüfen Sie der *pubxml* Datei.

Vertrauliche Informationen (z. B. das Kennwort für die Veröffentlichung) verschlüsselt wird, auf eine pro Benutzer/Computerebene zur Verfügung. Befindet sich in der *Eigenschaften/PublishProfiles/&lt;Profile_name&gt;. pubxml.user* Datei. Da diese Datei vertraulichen Informationen gespeichert werden kann, sollten nicht in die quellcodeverwaltung überprüft werden.

Einen Überblick über die Vorgehensweise beim Veröffentlichen einer Web-app auf ASP.NET Core finden Sie unter [Host und Bereitstellen von](xref:host-and-deploy/index). Die MSBuild-Aufgaben und Ziele, die zum Veröffentlichen einer app ASP.NET Core werden Open Source-am https://github.com/aspnet/websdk.

`dotnet publish` können MSDeploy, Ordner und [Kudu](https://github.com/projectkudu/kudu/wiki) Veröffentlichungsprofile:

Ordner (funktioniert plattformübergreifende):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

MSDeploy (derzeit dieser funktioniert nur unter Windows seit MSDeploy plattformübergreifende ist nicht):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

MSDeploy-Paket (derzeit dieser funktioniert nur unter Windows seit MSDeploy plattformübergreifende ist nicht):

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

In den vorherigen Beispielen **nicht** übergeben `deployonbuild` auf `dotnet publish`.

Weitere Informationen finden Sie unter [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` unterstützt die Kudu-APIs für die Veröffentlichung in Azure aus einer beliebigen Plattform. Visual Studio veröffentlichen unterstützt die Kudu-APIs, sondern von WebSDK unterstützt wird, für die plattformübergreifende in Azure veröffentlichen.

Hinzufügen ein Veröffentlichungsprofils, das *Eigenschaften/PublishProfiles* Ordner mit dem folgenden Inhalt:

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

Führen Sie den folgenden Befehl zum Komprimieren der Inhalte veröffentlichen und mithilfe der Kudu-APIs in Azure zu veröffentlichen:

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Legen Sie die folgenden MSBuild-Eigenschaften fest, wenn Sie ein Veröffentlichungsprofil verwenden:

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

Bei der Veröffentlichung mit einem Profil mit dem Namen *FolderProfile*, einen der folgenden Befehle können ausgeführt werden:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Beim Aufrufen von [Dotnet Build](/dotnet/core/tools/dotnet-build), ruft er `msbuild` führen Sie den Build, und Veröffentlichen von Prozess. Aufruf eines `dotnet build` oder `msbuild` entspricht, wenn in einem Ordner Profil übergeben. Beim Aufrufen von MSBuild direkt unter Windows wird die .NET Framework-Version von MSBuild verwendet. Die Veröffentlichung mit MSDeploy ist aktuell auf Windows-Computer beschränkt. Wenn Sie `dotnet build` von einem Profil ohne Ordner aufrufen, wird MSBuild aufgerufen. MSBuild verwendet MSDeploy für Profile ohne Ordner. Wenn Sie `dotnet build` von einem Profil ohne Ordner aufrufen, wird MSBuild (mithilfe von MSDeploy) aufgerufen. Dies führt zu einem Fehler (auch beim Ausführen auf einer Windows-Plattform). Rufen Sie MSBuild direkt auf, um mit einem Profil ohne Ordner zu veröffentlichen.

Der folgende Ordner „Veröffentlichungsprofil“ wurde mit Visual Studio erstellt und veröffentlicht in eine Netzwerkfreigabe.

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

Beachten Sie, dass `<LastUsedBuildConfiguration>` auf `Release` festgelegt ist. Bei der Veröffentlichung von Visual Studio wird der `<LastUsedBuildConfiguration>`-Wert für die Konfigurationseigenschaft mithilfe des Werts festgelegt, der beim Starten des Veröffentlichungsprozesses vorliegt. Die `<LastUsedBuildConfiguration>` Konfigurationseigenschaft Sonderregeln und sollte nicht in einer importierten MSBuild-Datei überschrieben werden. Diese Eigenschaft kann über die Befehlszeile überschrieben werden.

Verwenden die .NET Core CLI:

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

Verwenden von MSBuild:

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Veröffentlichen auf einen MSDeploy-Endpunkt von der Befehlszeile

Mithilfe der .NET Core CLI oder MSBuild kann die Publishing erreicht werden. `dotnet publish` wird im Kontext von .NET Core ausgeführt. Die `msbuild` -Befehl erfordert .NET Framework, die sie auf Windows-Umgebungen beschränkt.

Die einfachste Möglichkeit zum Veröffentlichen mit MSDeploy ist die, zunächst ein Veröffentlichungsprofil in Visual Studio 2017 zu erstellen und das Profil von der Befehlszeile aus zu verwenden.

Im folgenden Beispiel wird eine ASP.NET Core-Web-app erstellt (mit `dotnet new mvc`), und ein Azure-Veröffentlichungsprofil mit Visual Studio hinzugefügt.

Führen Sie `msbuild` aus einem **Developer-Eingabeaufforderung für VS 2017**. Die Developer-Eingabeaufforderung hat den richtigen *msbuild.exe* in der Pfadangabe mit einem Satz der MSBuild-Variablen.

MSBuild verwendet die folgende Syntax:

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

Abrufen der `Password` aus der  *\<Publish-Name >. PublishSettings* Datei. Herunterladen der *. PublishSettings* Datei aus:

* Projektmappen-Explorer: Mit der rechten Maustaste auf die Web-App, und wählen Sie **Veröffentlichungsprofil herunterladen**.
* Azure-Portal: Klicken Sie auf **Get Veröffentlichungsprofil** für die Web-App **Übersicht** Bereich.

`Username` kann im Veröffentlichungsprofil gefunden werden.

Das folgende Beispiel verwendet die *Web11112 - Web Deploy* Veröffentlichungsprofil:

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a>Ausschließen von Dateien

Beim Veröffentlichen von ASP.NET Core-Apps sind die Buildartefakte und die Inhalte des Ordners *wwwroot* enthalten. `msbuild` unterstützt [Globmuster](https://gruntjs.com/configuring-tasks#globbing-patterns). Beispielsweise die folgenden `<Content>` Element schließt den gesamten Text (*".txt"*)-Dateien aus der *"Wwwroot" / Content* Ordner und allen Unterordnern.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Das vorhergehende Markup kann ein Veröffentlichungsprofil hinzugefügt werden oder die *csproj* Datei. Wenn es zu der *CSPROJ*-Datei hinzugefügt wird, wird die Regel zu allen Veröffentlichungsprofilen im Projekt hinzugefügt.

Die folgenden `<MsDeploySkipRules>` Element schließt alle Dateien aus dem *"Wwwroot" / Content* Ordner:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` Löschen wird nicht die *überspringen* Ziele vom Bereitstellungsstandort. `<Content>` Ziel-Dateien und Ordner werden vom Bereitstellungsstandort gelöscht. Nehmen Sie beispielsweise an, dass eine bereitgestellte Web-app die folgenden Dateien wurde:

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Wenn die folgenden `<MsDeploySkipRules>` Elemente hinzugefügt werden, werden diese Dateien wäre nicht am Bereitstellungsstandort gelöscht.

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

Die vorangehenden `<MsDeploySkipRules>` Elemente zu verhindern, dass die *übersprungen* Dateien bereitgestellt wird. Es wird nicht die Dateien löschen, nachdem sie bereitgestellt werden.

Die folgenden `<Content>` Element löscht Zieldateien am Bereitstellungsstandort:

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Mit der Bereitstellung über die Befehlszeile mit dem vorangehenden `<Content>` Element führt zu folgender Ausgabe:

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a>Includedateien

Das folgende Markup enthält ein *Bilder* Ordner außerhalb des Projektverzeichnisses auf die *"Wwwroot"-Images* Ordner der Website veröffentlichen:

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

Das Markup kann der *CSPROJ*-Datei oder dem Veröffentlichungsprofil hinzugefügt werden. Wenn es hinzugefügt wird die *csproj* -Datei, die sie in jedes Veröffentlichungsprofil im Projekt enthalten ist.

Das folgende hervorgehobene Markup zeigt Folgendes:

* Kopieren einer Datei von außerhalb des Projekts in den Ordner *wwwroot*.
* Ausschließen des Ordners *wwwroot\Content*.
* Ausschließen von *Views\Home\About2.cshtml*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

Weitere Bereitstellungsbeispiele finden Sie unter [WebSDK Readme (WebSDK-Infodatei)](https://github.com/aspnet/websdk).

## <a name="run-a-target-before-or-after-publishing"></a>Führen Sie ein Ziel vor oder nach der Veröffentlichung aus.

Die integrierte `BeforePublish` und `AfterPublish` Ziele führen Sie ein Ziel, vor oder nach dem Veröffentlichungsziel. Fügen Sie die folgenden Elemente hinzu, um das Veröffentlichungsprofil Konsole Nachrichten vor und nach der Veröffentlichung protokolliert:

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Veröffentlichen Sie auf einem Server unter Verwendung eines nicht vertrauenswürdigen Zertifikats

Hinzufügen der `<AllowUntrustedCertificate>` Eigenschaft mit einem Wert von `True` auf dem Veröffentlichungsprofil ein:

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Der Kudu-Dienst

Verwenden Sie zum Anzeigen der Dateien in einer Azure App Service-Web-app-Bereitstellung die [Kudu Service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Anfügen der `scm` -Token genutzt, um die Web-app-Name. Zum Beispiel:

| URL                                    | Ergebnis       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Webanwendung      |
| `http://mysite.scm.azurewebsites.net/` | Kudu-Dienst |

Wählen Sie die [Debug-Konsole](https://github.com/projectkudu/kudu/wiki/Kudu-console) Menüelement zum Anzeigen, bearbeiten, löschen oder Hinzufügen von Dateien.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) vereinfacht die Bereitstellung der Web-apps und Websites so, dass IIS-Servern.
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Datei Probleme, und fordern Sie Funktionen für die Bereitstellung.
