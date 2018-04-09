---
title: Visual Studio die Veröffentlichungsprofile für ASP.NET Core-app-Bereitstellung
author: rick-anderson
description: Erfahren Sie, wie erstellen Veröffentlichungsprofile für ASP.NET Core-apps in Visual Studio.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 64c96f572c42c56480cfe2bd58f926d54eddf35e
ms.sourcegitcommit: 71b93b42cbce8a9b1a12c4d88391e75a4dfb6162
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Visual Studio die Veröffentlichungsprofile für ASP.NET Core-app-Bereitstellung

Von [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieser Artikel konzertiert sich auf die Verwendung von Visual Studio 2017 zum Erstellen von Veröffentlichungsprofilen. Die Veröffentlichungsprofile, die mit Visual Studio erstellt werden, können von MSBuild und Visual Studio 2017 ausgeführt werden. Dieser Artikel enthält Informationen zum Veröffentlichungsprozess. Anweisungen zum Veröffentlichen in Azure finden Sie unter [Veröffentlichen einer ASP.NET Core-Web-App in Azure App Service mit Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).

Die folgende *CSPROJ*-Datei wurde mit dem Befehl `dotnet new mvc` erstellt:

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

Das `Sdk`-Attribut im `<Project>`-Element (in der ersten Zeile) des obenstehenden Markups bewirkt Folgendes:

* Importiert die Eigenschaftendatei aus *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* am Anfang.
* Es importiert am Ende die Zieledatei von *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*.

Der Standardspeicherort für `MSBuildSDKsPath` (mit Visual Studio 2017 Enterprise) ist der Ordner *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

`Microsoft.NET.Sdk.Web` ist abhängig von:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Dadurch werden die folgenden Eigenschaften und Ziele, die importiert werden:

* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets

Veröffentlichen Sie Targets-Import, legen Sie rechts von Zielen, die auf der Grundlage der Publish-Methode verwendet.

Wenn in MSBuild oder Visual Studio ein Projekt geladen wird, werden die folgenden Aktionen auf hoher Ebene ausgeführt:

* Erstellen des Projekts
* Berechnen der zu veröffentlichenden Dateien
* Veröffentlichen der Dateien auf dem Ziel

## <a name="compute-project-items"></a>Berechnen der Projektelemente

Wenn das Projekt geladen ist, werden die Projektelemente (Dateien) berechnet. Die `item type`-Attribut bestimmt, wie die Datei verarbeitet wird. Standardmäßig sind *CS*-Dateien in der `Compile`-Elementliste enthalten. Dateien in der `Compile`-Elementliste werden kompiliert.

Die `Content` Elementliste enthält Dateien, die zusätzlich zu den Buildausgaben veröffentlicht werden. Standardmäßig Dateien gemäß dem Muster `wwwroot/**` befinden sich die `Content` Element. ["Wwwroot" /\* \* ist ein Muster Globmodus](https://gruntjs.com/configuring-tasks#globbing-patterns) , die angibt, dass alle Dateien in der *"Wwwroot"* Ordner **und** Unterordner. Um eine Datei explizit der Veröffentlichungsliste hinzuzufügen, fügen Sie die Datei wie in [Einfügen von Dateien](#including-files) gezeigt direkt der *CSPROJ*-Datei hinzu.

Bei der Auswahl der **veröffentlichen** Schaltfläche in Visual Studio oder beim Veröffentlichen von der Befehlszeile aus:

* Die Eigenschaften und Elemente werden berechnet (die Dateien, die für den Build benötigt werden).
* Nur Visual Studio: NuGet-Pakete werden wiederhergestellt. (Die Wiederherstellung muss explizit vom Benutzer auf der CLI durchgeführt werden.)
* Das Projekt wird erstellt.
* Die zu veröffentlichenden Elemente werden berechnet (die Dateien, die für die Veröffentlichung benötigt werden).
* Das Projekt wird veröffentlicht. (Die berechneten Dateien werden in das Veröffentlichungsziel kopiert.)

Wenn eine ASP.NET Core-Projekt verweist auf `Microsoft.NET.Sdk.Web` in der Projektdatei ein *app_offline.htm* Datei befindet sich im Stammverzeichnis des Verzeichnisses der Web-app. Wenn die Datei vorhanden ist, fährt das ASP.NET Core Module die App ordnungsgemäß herunter und verarbeitet die Datei *app_offline.htm* während der Bereitstellung. Weitere Informationen finden Sie unter [Konfigurationsreferenz für das ASP.NET Core-Modul](xref:host-and-deploy/aspnet-core-module#appofflinehtm).

## <a name="basic-command-line-publishing"></a>Grundlegende Befehlszeile veröffentlichen

Befehlszeilen-Veröffentlichung funktioniert auf allen .NET Core unterstützten Plattformen und erfordert nicht das Visual Studio. In den Beispielen unten die [Dotnet veröffentlichen](/dotnet/core/tools/dotnet-publish) aus dem Projektverzeichnis ausgeführt wird (enthält die *csproj* Datei). Wenn dies nicht explizit in den Projektordner übergeben, in der Projektdateipfad. Zum Beispiel:

```console
dotnet publish c:/webs/web1
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

Der Standardveröffentlichungsordner ist `bin\$(Configuration)\netcoreapp<version>\publish`. Der Standardwert für `$(Configuration)` ist „Debuggen“. Im obigen Beispiel ist `<TargetFramework>` `netcoreapp2.0`.

`dotnet publish -h` zeigt Hilfeinformationen zum Veröffentlichen an.

Der folgende Befehl gibt einen `Release`-Build und das Veröffentlichungsverzeichnis an:

```console
dotnet publish -c Release -o C:/MyWebs/test
```

Die [Dotnet veröffentlichen](/dotnet/core/tools/dotnet-publish) Befehl ruft die ruft MSBuild den `Publish` Ziel. Übergeben von Parametern `dotnet publish` an MSBuild übergeben werden. Der Parameter `-c` wird der MSBuild-Eigenschaft `Configuration` zugeordnet. Der Parameter `-o` wird `OutputPath` zugeordnet.

MSBuild-Eigenschaften können mithilfe einer der folgenden Formate übergeben werden:

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Der folgende Befehl veröffentlicht einen `Release`-Build auf einer Netzwerkfreigabe:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Die Netzwerkfreigabe wird durch führende Schrägstriche (*//r8/*) angegeben und funktioniert auf allen Plattformen, die von .NET Core unterstützt werden.

Versichern Sie sich, dass die veröffentlichte App für die Bereitstellung nicht ausgeführt wird. Die Dateien im Ordner *publish* sind gesperrt, wenn die App ausgeführt wird. Die Bereitstellung kann nicht erfolgen, da die gesperrten Dateien nicht kopiert werden können.

## <a name="publish-profiles"></a>Veröffentlichungsprofile

In diesem Abschnitt wird Visual Studio 2017 und höher verwendet, um Veröffentlichungsprofile zu erstellen. Nach der Erstellung ist die Veröffentlichung aus Visual Studio oder der Befehlszeile zur Verfügung.

Veröffentlichungsprofile können den Veröffentlichungsprozess vereinfachen. Mehrere Veröffentlichungsprofile können vorhanden sein. Klicken Sie zum Erstellen eines Veröffentlichungsprofils in Visual Studio mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und wählen Sie **veröffentlichen**. Wählen Sie alternativ **veröffentlichen \<Projektname >** im Menü erstellen. Die Registerkarte **Veröffentlichen** der Seite „Anwendungsfunktionen“ wird angezeigt. Wenn das Projekt kein Veröffentlichungsprofil enthält, wird die folgende Seite angezeigt:

![Die Registerkarte "Veröffentlichen" auf der Anwendungsseite-Kapazitäten mit Azure "," IIS "," FTB "," Ordner mit Azure ausgewählt. Zeigt auch die Optionsfelder „Neu erstellen“ und „Beenden auswählen“.](visual-studio-publish-profiles/_static/az.png)

Wenn **Ordner** ausgewählt ist, erstellt die Schaltfläche **Veröffentlichen** den Ordner „Veröffentlichungsprofil“ und veröffentlicht.

![Die Registerkarte „**Veröffentlichen**“ der Seite „Anwendungsfunktionen“, die Azure, IIS, FTB und Ordner anzeigt.](visual-studio-publish-profiles/_static/pub1.png)

Sobald ein Veröffentlichungsprofil erstellt wurde, die **veröffentlichen** Änderungen, und wählen Sie **neues Profil erstellen** um ein neues Profil zu erstellen.

![Die Registerkarte „**Veröffentlichen**“ der Seite „Anwendungsfunktionen“, die das im letzten Schritt erstellte „FolderProfile“ und den Link „Neues Profil erstellen“ anzeigt.](visual-studio-publish-profiles/_static/create_new.png)

Der Webpublishing-Assistent unterstützt folgende Veröffentlichungsziele:

* Microsoft Azure App Service
* IIS, FTP usw. (für alle Webserver)
* Ordner
* Der Profil importieren (Import des Benutzerprofils ermöglicht).
* Microsoft Azure Virtual Machines

Weitere Informationen finden Sie unter [What publishing options are right for me? (Welche Optionen für die Veröffentlichung sind für mich geeignet?)](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options)

Beim Erstellen eines Veröffentlichungsprofils mit Visual Studio eine *Eigenschaften/PublishProfiles/\<Veröffentlichungsname > pubxml* MSBuild-Datei wird erstellt. Diese *PUBXML*-Datei ist eine MSBuild-Datei und enthält die Konfigurationseinstellungen für die Veröffentlichung. Diese Datei kann geändert werden, um den Build anpassen und Veröffentlichen von Prozess. Diese Datei wird vom Veröffentlichungsprozess gelesen. `<LastUsedBuildConfiguration>` ist ein Sonderfall, da er eine globale Eigenschaft und sollte nicht in jeder Datei, die im Build importiert werden. Weitere Informationen finden Sie unter [MSBuild: how to set the configuration property (MSBuild: Festlegen der Konfigurationseigenschaft)](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).

Die *pubxml* Datei darf nicht in die quellcodeverwaltung überprüft werden, da er abhängt der *User* Datei. Die *USER*-Datei sollte niemals in die Quellcodeverwaltung eingecheckt sein, da sie vertrauliche Informationen enthalten kann und nur für einen Benutzer und einen Computer gültig ist.

Vertrauliche Informationen (z.B. das Verschlüsselungskennwort) werden auf einer Ebene pro Benutzer/Computer verschlüsselt und in der Datei *Properties/PublishProfiles/\<publish name>.pubxml.user* gespeichert. Da diese Datei vertrauliche Informationen enthalten kann, sollte sie **nicht** in die Quellverwaltung eingecheckt werden.

Einen Überblick über die Vorgehensweise beim Veröffentlichen einer Web-app auf ASP.NET Core finden Sie unter [Host und Bereitstellen von](index.md). [Hosten und Bereitstellen von](index.md) ist ein open-Source-Projekt am https://github.com/aspnet/websdk.

`dotnet publish` können MSDeploy, Ordner und [KUDU](https://github.com/projectkudu/kudu/wiki) Veröffentlichungsprofile:
 
Ordner (funktioniert plattformübergreifende): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`

MSDeploy (derzeit dieser funktioniert nur unter Windows seit MSDeploy plattformübergreifende ist nicht): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`

MSDeploy-Paket (derzeit dieser funktioniert nur unter Windows seit MSDeploy plattformübergreifende ist nicht): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`

In den vorherigen Beispielen **nicht** übergeben `deployonbuild` auf `dotnet publish`.

Weitere Informationen finden Sie unter [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` unterstützt KUDU-APIs zur Veröffentlichung in Azure von jeder Plattform aus. Visual Studio veröffentlichen unterstützt die KUDU-APIs, aber es wird unterstützt von Websdk für Cross-Plattform in Azure veröffentlichen.

Fügen Sie dem Ordner *Properties/PublishProfiles* ein Veröffentlichungsprofil mit folgendem Inhalt hinzu:

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

Ausführen des folgenden Befehls komprimiert, um die Inhalte veröffentlichen und mithilfe der KUDU-APIs in Azure zu veröffentlichen:

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

Legen Sie die folgenden MSBuild-Eigenschaften fest, wenn Sie ein Veröffentlichungsprofil verwenden:

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

Bei der Veröffentlichung mit einem Profil mit dem Namen *FolderProfile*, einen der folgenden Befehle können ausgeführt werden:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Beim Aufrufen von [Dotnet Build](/dotnet/core/tools/dotnet-build), ruft er `msbuild` führen Sie den Build, und Veröffentlichen von Prozess. Aufrufen von `dotnet build` oder `msbuild` entspricht im Wesentlichen bei der Übergabe in einem Ordner-Profil. Beim Aufrufen von MSBuild direkt unter Windows wird die .NET Framework-Version von MSBuild verwendet. Die Veröffentlichung mit MSDeploy ist aktuell auf Windows-Computer beschränkt. Wenn Sie `dotnet build` von einem Profil ohne Ordner aufrufen, wird MSBuild aufgerufen. MSBuild verwendet MSDeploy für Profile ohne Ordner. Wenn Sie `dotnet build` von einem Profil ohne Ordner aufrufen, wird MSBuild (mithilfe von MSDeploy) aufgerufen. Dies führt zu einem Fehler (auch beim Ausführen auf einer Windows-Plattform). Rufen Sie MSBuild direkt auf, um mit einem Profil ohne Ordner zu veröffentlichen.

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

Beachten Sie, dass `<LastUsedBuildConfiguration>` auf `Release` festgelegt ist. Bei der Veröffentlichung von Visual Studio wird der `<LastUsedBuildConfiguration>`-Wert für die Konfigurationseigenschaft mithilfe des Werts festgelegt, der beim Starten des Veröffentlichungsprozesses vorliegt. Die `<LastUsedBuildConfiguration>` Konfigurationseigenschaft Sonderregeln und sollte nicht in einer importierten MSBuild-Datei überschrieben werden. Diese Eigenschaft kann über die Befehlszeile überschrieben werden. Zum Beispiel:

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Verwenden von MSBuild:

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Veröffentlichen auf einen MSDeploy-Endpunkt von der Befehlszeile

Wie bereits erwähnt, Veröffentlichung kann erreicht werden, mithilfe von `dotnet publish` oder `msbuild` Befehl. `dotnet publish` wird im Kontext von .NET Core ausgeführt. Die `msbuild` Befehl erfordert mindestens .NET Framework und ist daher auf Windows-Umgebungen beschränkt.

Die einfachste Möglichkeit zum Veröffentlichen mit MSDeploy ist die, zunächst ein Veröffentlichungsprofil in Visual Studio 2017 zu erstellen und das Profil von der Befehlszeile aus zu verwenden.

Im folgenden Beispiel wird eine ASP.NET Core-Web-app erstellt (mit `dotnet new mvc`), und ein Azure-Veröffentlichungsprofil mit Visual Studio hinzugefügt.

Führen Sie `msbuild` aus einem **Developer-Eingabeaufforderung für VS 2017**. Die Developer-Eingabeaufforderung hat den richtigen *msbuild.exe* in der Pfadangabe mit einem Satz der MSBuild-Variablen.

MSBuild verwendet die folgende Syntax:

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

Abrufen der `Password` aus der  *\<Publish-Name >. PublishSettings* Datei. Herunterladen der *. PublishSettings* Datei aus:

* Projektmappen-Explorer: Mit der rechten Maustaste auf die Web-App, und wählen Sie **Veröffentlichungsprofil herunterladen**.
* Im Azure-Verwaltungsportal: Wählen Sie **Get Veröffentlichungsprofil** aus dem Blatt "Web-App".

`Username` kann im Veröffentlichungsprofil gefunden werden.

Das folgende Beispiel verwendet das Veröffentlichungsprofil „Web11112 – Web Deploy“:

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a>Ausschließen von Dateien

Beim Veröffentlichen von ASP.NET Core-Apps sind die Buildartefakte und die Inhalte des Ordners *wwwroot* enthalten. `msbuild` unterstützt [Globmuster](https://gruntjs.com/configuring-tasks#globbing-patterns). Beispielsweise die folgenden `<Content>` Elementknoten dem Elementmarkup schließt den gesamten Text (*".txt"*)-Dateien aus der *"Wwwroot" / Content* Ordner und allen Unterordnern.

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Das obenstehende Markup kann zu einem Veröffentlichungsprofil oder der *CSPROJ*-Datei hinzugefügt werden. Wenn es zu der *CSPROJ*-Datei hinzugefügt wird, wird die Regel zu allen Veröffentlichungsprofilen im Projekt hinzugefügt.

Das folgende `<MsDeploySkipRules>`-Elementmarkup schließt alle Dateien aus dem Ordner *wwwroot/content* aus:

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

Wenn die folgenden `<MsDeploySkipRules>` Markup hinzugefügt wird, wird diese Dateien wäre nicht am Bereitstellungsstandort gelöscht werden.

``` xml
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

Die `<MsDeploySkipRules>` Markup oben gezeigten wird verhindert, dass die *übersprungen* Dateien nicht Depoyed, aber diese Dateien wird nicht löschen, nachdem sie bereitgestellt werden.

Die folgenden `<Content>` Markup löscht Zieldateien am Bereitstellungsstandort:

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Verwenden die Bereitstellung über die Befehlszeile mit der `<Content>` Markup oben in der Ausgabe ähnlich der folgenden Ergebnisse:

``` console
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

## <a name="including-files"></a>Einfügen von Dateien

Das folgende Markup enthält ein *Bilder* Ordner außerhalb des Projektverzeichnisses auf die *"Wwwroot"-Images* Ordner der Website veröffentlichen:

``` xml
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

Die integrierte `BeforePublish` und `AfterPublish` Ziele können verwendet werden, um ein Ziel vor oder nach dem Veröffentlichungsziel auszuführen. Das folgende Markup kann dem Veröffentlichungsprofil hinzugefügt werden, um Nachrichten an die Konsolenausgabe vor und nach der Veröffentlichung zu protokollieren:

``` xml
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

Zum Anzeigen der Dateien in der ein Azure-Apps Service Web-app-Bereitstellung, verwenden die [Kudu Service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Anfügen der `scm` -Token genutzt, um den Namen der Web-app. Zum Beispiel:

| URL                                    | Ergebnis      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | Webanwendung     |
| `http://mysite.scm.azurewebsites.net/` | Kudu-Dienst |

Klicken Sie auf das Menüelement [Debugging-Konsole](https://github.com/projectkudu/kudu/wiki/Kudu-console), um Dateien anzuzeigen, zu bearbeiten, zu löschen oder hinzuzufügen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) vereinfacht die Bereitstellung der Web-apps und Websites so, dass IIS-Servern.
* [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Datei Probleme, und fordern Sie Funktionen für die Bereitstellung.
