---
title: "Erstellen von Veröffentlichungsprofilen für Visual Studio und MSBuild"
author: rick-anderson
description: "Erläutert das Webpublishing in Visual Studio."
keywords: "ASP.NET Core, Webpublishing, Veröffentlichung, MSBuild, Webbereitstellung, dotnet publish, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: 872e8c99734a0913770281d9d87b1e30c250ab0a
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a>Erstellen von Veröffentlichungsprofilen für Visual Studio und MSBuild zum Bereitstellen von ASP.NET Core-Apps

Von [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) und [Rick Anderson](https://twitter.com/RickAndMSFT)

Dieser Artikel konzertiert sich auf die Verwendung von Visual Studio 2017 zum Erstellen von Veröffentlichungsprofilen. Die Veröffentlichungsprofile, die mit Visual Studio erstellt werden, können von MSBuild und Visual Studio 2017 ausgeführt werden.

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
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

Das `Sdk`-Attribut im `<Project>`-Element (in der ersten Zeile) des obenstehenden Markups bewirkt Folgendes:

* Es importiert am Anfang die `props`-Datei von *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props*.
* Es importiert am Ende die Zieledatei von *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*.

Der Standardspeicherort für `MSBuildSDKsPath` (mit Visual Studio 2017 Enterprise) ist der Ordner *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.

`Microsoft.NET.Sdk.Web` ist abhängig von:

* *Microsoft.NET.Sdk.Web.ProjectSystem*
* *Microsoft.NET.Sdk.Publish*

Dadurch werden folgende `props` und `targets` importiert:

* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props
* $(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets

Durch „publish targets“ wird basierend auf der verwendeten „publish“-Methode die richtige Gruppe von Zielen importiert.

Wenn in MSBuild oder Visual Studio ein Projekt geladen wird, werden die folgenden Aktionen auf hoher Ebene ausgeführt:

* Erstellen des Projekts
* Berechnen der zu veröffentlichenden Dateien
* Veröffentlichen der Dateien auf dem Ziel

### <a name="compute-project-items"></a>Berechnen der Projektelemente

Wenn das Projekt geladen ist, werden die Projektelemente (Dateien) berechnet. Die `item type`-Attribut bestimmt, wie die Datei verarbeitet wird. Standardmäßig sind *CS*-Dateien in der `Compile`-Elementliste enthalten. Dateien in der `Compile`-Elementliste werden kompiliert.

Die `Content`-Elementliste enthält Dateien, die zusätzlich zu den Buildausgaben veröffentlicht werden. Standardmäßig sind Dateien, die mit dem Muster „wwwroot/**“ übereinstimmen, im `Content`-Element enthalten. [wwwroot/** ist ein Globmuster](https://gruntjs.com/configuring-tasks#globbing-patterns), das alle Dateien im *wwwroot*-Ordner **und** in Unterordnern angibt. Wenn Sie eine Datei explizit der Veröffentlichungsliste hinzufügen müssen, können Sie die Datei wie in [Einfügen von Dateien](#including-files) gezeigt direkt der *CSPROJ*-Datei hinzufügen.

Wenn Sie auf die Schaltfläche **Veröffentlichen** in Visual Studio klicken oder aus der Befehlszeile veröffentlichen:

- Die Eigenschaften und Elemente werden berechnet (die Dateien, die für den Build benötigt werden).
- Nur Visual Studio: NuGet-Pakete werden wiederhergestellt.  (Die Wiederherstellung muss explizit vom Benutzer auf der CLI durchgeführt werden.)
- Das Projekt wird erstellt.
- Die zu veröffentlichenden Elemente werden berechnet (die Dateien, die für die Veröffentlichung benötigt werden).
- Das Projekt wird veröffentlicht. (Die berechneten Dateien werden in das Veröffentlichungsziel kopiert.)

## <a name="simple-command-line-publishing"></a>Einfaches Veröffentlichen über die Befehlszeile

Dieser Abschnitt funktioniert auf allen von .NET Core unterstützten Plattformen und erfordert nicht Visual Studio. In den untenstehenden Beispielen wird der Befehl `dotnet publish` aus dem Projektverzeichnis (das die *CSPROJ*-Datei enthält) ausgeführt. Wenn Sie nicht im Projektordner sind, können Sie explizit den Projektdateipfad übergeben. Zum Beispiel:

```console
dotnet publish  c:/webs/web1
```

Führen Sie die folgenden Befehle zum Erstellen und Veröffentlichen eine Web-App aus:

```console
dotnet new mvc
dotnet restore
dotnet publish
```

Durch `dotnet publish` wird eine Ausgabe erzeugt, die Folgender ähnelt:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.1.548.43366
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp1.1\Web1.dll
```

Der Standardveröffentlichungsordner ist `bin\$(Configuration)\netcoreapp<version>\publish`. Der Standardwert für `$(Configuration)` ist „Debuggen“. Im obigen Beispiel ist `<TargetFramework>` `netcoreapp1.1`. Der tatsächliche Pfad im obigen Beispiel ist *bin\Debug\netcoreapp1.1\publish*.

`dotnet publish -h` zeigt Hilfeinformationen zum Veröffentlichen an.

Der folgende Befehl gibt einen `Release`-Build und das Veröffentlichungsverzeichnis an:

```console
dotnet publish -c Release -o C:/MyWebs/test
```

Der Befehl `dotnet publish` ruft `MSBuild` auf, welches das `Publish`-Ziel aufruft. Alle Parameter, die an `dotnet publish` übergeben werden, werden an `MSBuild` übergeben. Der Parameter `-c` wird der MSBuild-Eigenschaft `Configuration` zugeordnet. Der Parameter `-o` wird `OutputPath` zugeordnet.

Sie können MSBuild-Eigenschaften mithilfe von einem der folgenden Formate übergeben:

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

Der folgende Befehl veröffentlicht einen `Release`-Build auf einer Netzwerkfreigabe:

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Die Netzwerkfreigabe wird durch führende Schrägstriche (*//r8/*) angegeben und funktioniert auf allen Plattformen, die von .NET Core unterstützt werden.

Versichern Sie sich, dass die veröffentlichte App für die Bereitstellung nicht ausgeführt wird. Die Dateien im Ordner *publish* sind gesperrt, wenn die App ausgeführt wird. Die Bereitstellung kann nicht erfolgen, da die gesperrten Dateien nicht kopiert werden können.

## <a name="publish-profiles"></a>Veröffentlichungsprofile

In diesem Abschnitt wird Visual Studio 2017 und höher verwendet, um Veröffentlichungsprofile zu erstellen. Nach dem Erstellen können Sie von Visual Studio oder der Befehlszeile veröffentlichen.

Veröffentlichungsprofile können den Veröffentlichungsprozess vereinfachen. Sie können mehrere Veröffentlichungsprofile besitzen. Klicken Sie zum Erstellen eines Veröffentlichungsprofils in Visual Studio mit der rechten Maustaste auf das Projekt im Projektmappen-Explorer, und klicken Sie auf **Veröffentlichen**. Alternativ können Sie auf **Veröffentlichen\<Projektname>** im Buildmenü klicken. Die Registerkarte **Veröffentlichen** der Seite „Anwendungsfunktionen“ wird angezeigt. Wenn das Projekt kein Veröffentlichungsprofil enthält, wird die folgende Seite angezeigt:

![Die Registerkarte „**Veröffentlichen**“ der Seite „Anwendungsfunktionen“, die Azure (ausgewählt), IIS, FTB und Ordner anzeigt. Zeigt auch die Optionsfelder „Neu erstellen“ und „Beenden auswählen“.](web-publishing-vs/_static/az.png)

Wenn **Ordner** ausgewählt ist, erstellt die Schaltfläche **Veröffentlichen** den Ordner „Veröffentlichungsprofil“ und veröffentlicht.

![Die Registerkarte „**Veröffentlichen**“ der Seite „Anwendungsfunktionen“, die Azure, IIS, FTB und Ordner anzeigt.](web-publishing-vs/_static/pub1.png)

Sobald ein Veröffentlichungsprofil erstellt ist, ändert sich die Registerkarte **Veröffentlichen**, und Sie können auf **Neues Profil erstellen** klicken, um ein neues Profil zu erstellen.

![Die Registerkarte „**Veröffentlichen**“ der Seite „Anwendungsfunktionen“, die das im letzten Schritt erstellte „FolderProfile“ und den Link „Neues Profil erstellen“ anzeigt.](web-publishing-vs/_static/create_new.png)

Der Webpublishing-Assistent unterstützt folgende Veröffentlichungsziele:

- Microsoft Azure App Service
- IIS, FTP usw. (für alle Webserver)
- Ordner
- Profil importieren (ermöglicht Ihnen das Importieren eines Profils).
- Microsoft Azure Virtual Machines

Weitere Informationen finden Sie unter [What publishing options are right for me? (Welche Optionen für die Veröffentlichung sind für mich geeignet?)](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options)

Wenn Sie ein Veröffentlichungsprofil mit Visual Studio erstellen, wird eine *Properties/PublishProfiles/\<publish name>.pubxml*-MSBuild-Datei erstellt. Diese *PUBXML*-Datei ist eine MSBuild-Datei und enthält die Konfigurationseinstellungen für die Veröffentlichung. Sie können diese Datei ändern, um den Build- und Veröffentlichungsprozess anzupassen. Diese Datei wird vom Veröffentlichungsprozess gelesen. `<LastUsedBuildConfiguration>` ist ein Sonderfall, da es sich dabei um eine globale Eigenschaft handelt, die nicht in einer Datei enthalten sein sollte, die in den Build importiert wurde. Weitere Informationen finden Sie unter [MSBuild: how to set the configuration property (MSBuild: Festlegen der Konfigurationseigenschaft)](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).
Die *PUBXML*-Datei sollte nicht in die Quellcodeverwaltung eingecheckt sein, da sie von der *USER*-Datei abhängig ist. Die *USER*-Datei sollte niemals in die Quellcodeverwaltung eingecheckt sein, da sie vertrauliche Informationen enthalten kann und nur für einen Benutzer und einen Computer gültig ist.

Vertrauliche Informationen (z.B. das Verschlüsselungskennwort) werden auf einer Ebene pro Benutzer/Computer verschlüsselt und in der Datei *Properties/PublishProfiles/\<publish name>.pubxml.user* gespeichert. Da diese Datei vertrauliche Informationen enthalten kann, sollte sie **nicht** in die Quellverwaltung eingecheckt werden.

Einen Überblick über das Veröffentlichen einer Web-App auf ASP.NET Core finden Sie unter [Publishing and Deployment (Veröffentlichung und Bereitstellung)](index.md). Bei [Publishing and Deployment (Veröffentlichung und Bereitstellung)](index.md) handelt es sich um ein Open Source-Projekt auf https://github.com/aspnet/websdk.

Aktuell kann `dotnet publish` keine Veröffentlichungsprofile verwenden. Verwenden Sie `dotnet build` zum Verwenden von Veröffentlichungsprofilen. `dotnet build` ruft MSBuild im Projekt auf. Rufen Sie alternativ direkt `msbuild` auf.

Legen Sie die folgenden MSBuild-Eigenschaften fest, wenn Sie ein Veröffentlichungsprofil verwenden:

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

Beispielsweise können Sie einen der untenstehenden Befehle ausführen, wenn Sie mit einem Profil namens *FolderProfile* veröffentlichen.

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Wenn Sie `dotnet build` aufrufen, wird dieses `msbuild` aufrufen, um den Build- und Veröffentlichungsprozess auszuführen. Das Aufrufen von `dotnet build` oder `msbuild` ist im Wesentlichen äquivalent, wenn Sie in ein Ordnerprofil übergeben. Wenn Sie MSBuild direkt in Windows aufrufen, erhalten Sie die .NET Framework-Version von MSBuild.  Die Veröffentlichung mit MSDeploy ist aktuell auf Windows-Computer beschränkt. Wenn Sie `dotnet build` von einem Profil ohne Ordner aufrufen, wird MSBuild aufgerufen. MSBuild verwendet MSDeploy für Profile ohne Ordner. Wenn Sie `dotnet build` von einem Profil ohne Ordner aufrufen, wird MSBuild (mithilfe von MSDeploy) aufgerufen. Dies führt zu einem Fehler (auch beim Ausführen auf einer Windows-Plattform). Rufen Sie MSBuild direkt auf, um mit einem Profil ohne Ordner zu veröffentlichen.

Der folgende Ordner „Veröffentlichungsprofil“ wurde mit Visual Studio erstellt und veröffentlicht in eine Netzwerkfreigabe.

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

Beachten Sie, dass `<LastUsedBuildConfiguration>` auf `Release` festgelegt ist.  Bei der Veröffentlichung von Visual Studio wird der `<LastUsedBuildConfiguration>`-Wert für die Konfigurationseigenschaft mithilfe des Werts festgelegt, der beim Starten des Veröffentlichungsprozesses vorliegt. Die `<LastUsedBuildConfiguration>`-Konfigurationseigenschaft ist ein Sonderfall und sollte nicht in einer importierte MSBuild-Datei überschrieben werden. Sie können diese Eigenschaft von der Befehlszeile aus überschreiben. Zum Beispiel:

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

Verwenden von MSBuild:

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Veröffentlichen auf einen MSDeploy-Endpunkt von der Befehlszeile

Wie bereits erwähnt können Sie mithilfe des Befehls `dotnet publish` oder `msbuild` veröffentlichen. `dotnet publish` wird im Kontext von .NET Core ausgeführt. `msbuild` erfordert .NET Framework und ist daher auf Windows-Umgebungen beschränkt.

Die einfachste Möglichkeit zum Veröffentlichen mit MSDeploy ist die, zunächst ein Veröffentlichungsprofil in Visual Studio 2017 zu erstellen und das Profil von der Befehlszeile aus zu verwenden.

Im folgenden Beispiel wurde eine ASP.NET Core-App (mithilfe von `dotnet new mvc`) erstellt und ein Azure-Veröffentlichungsprofil mit Visual Studio hinzugefügt.

Führen Sie `msbuild` von einer **Developer-Eingabeaufforderung für VS 2017** aus. Die Developer-Eingabeaufforderung verfügt in ihrem Pfad über die richtige *msbuild.exe*-Datei und über einige festgelegte MSBuild-Variablen.

MSBuild verwendet die folgende Syntax:

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

Sie erhalten das `Password` von der Datei *\<Publish name>.PublishSettings*. Hier können Sie die Datei *.PublishSettings* herunterladen:

- Projektmappen-Explorer: Klicken Sie mit der rechten Maustaste auf Web-App, und klicken Sie auf **Veröffentlichungsprofil herunterladen**.
- Im Azure-Verwaltungsportal: Klicken Sie im Blatt „Web-App“ auf **Veröffentlichungsprofil abrufen**.

`Username` kann im Veröffentlichungsprofil gefunden werden.

Das folgende Beispiel verwendet das Veröffentlichungsprofil „Web11112 – Web Deploy“:

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a>Ausschließen von Dateien

Beim Veröffentlichen von ASP.NET Core-Apps sind die Buildartefakte und die Inhalte des Ordners *wwwroot* enthalten. `msbuild` unterstützt [Globmuster](https://gruntjs.com/configuring-tasks#globbing-patterns). Das folgende `<Content>`-Elementmarkup schließt zum Beispiel alle Textdateien (*TXT*) aus dem Ordner *wwwroot/content* und all seinen Unterordnern aus.

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
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` löscht nicht die *Skip*-Ziele von der Bereitstellungsseite. Von `<Content>` angezielte Dateien und Ordner werden von der Bereitstellungsseite gelöscht. Nehmen Sie beispielsweise an, dass Sie eine Web-App mit den folgenden Dateien bereitgestellt hätten:

- *Views/Home/About1.cshtml*
- *Views/Home/About2.cshtml*
- *Views/Home/About3.cshtml*

Wenn Sie das folgende `<MsDeploySkipRules>`-Markup hinzufügen, würden diese Dateien nicht von der Bereitstellungsseite gelöscht werden.

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

Das oben gezeigte `<MsDeploySkipRules>`-Markup verhindert, dass die *übersprungenen* Dateien bereitgestellt werden. Es löscht diese Dateien jedoch nicht, nachdem sie bereitgestellt wurden.

Das folgende `<Content>`-Markup löscht die angezielten Dateien auf der Bereitstellungsseite:

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Das Verwenden der Befehlszeilenbereitstellung mit dem obenstehenden `<Content>`-Markup würde zu einer Ausgabe ähnlich der Folgenden führen:

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

Das folgende Markup kann verwendet werden, um einen *Bilder*-Ordner außerhalb des Projektverzeichnisses in den Ordner *wwwroot/images* auf der Website einzuschließen.

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

Das Markup kann der *CSPROJ*-Datei oder dem Veröffentlichungsprofil hinzugefügt werden. Wenn es der *CSPROJ*-Datei hinzugefügt wird, wird es in jedes Veröffentlichungsprofil im Projekt eingeschlossen.

Das folgende hervorgehobene Markup zeigt Folgendes:

* Kopieren einer Datei von außerhalb des Projekts in den Ordner *wwwroot*.
* Ausschließen des Ordners *wwwroot\Content*.
* Ausschließen von *Views\Home\About2.cshtml*.

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

Weitere Bereitstellungsbeispiele finden Sie unter [WebSDK Readme (WebSDK-Infodatei)](https://github.com/aspnet/websdk).

### <a name="run-a-target-before-or-after-publishing"></a>Führen Sie ein Ziel vor oder nach der Veröffentlichung aus.

Die integrierten Ziele `BeforePublish` und `AfterPublish` können verwendet werden, um ein Ziel vor oder nach dem Veröffentlichungsziel auszuführen. Das folgende Markup kann dem Veröffentlichungsprofil hinzugefügt werden, um Nachrichten an die Konsolenausgabe vor und nach der Veröffentlichung zu protokollieren:

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a>Der Kudu-Dienst

Verwenden Sie zum Anzeigen der Dateien in Ihrer Azure-Web-App den [Kudu-Dienst](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Fügen Sie den Token `scm` an den Namen Ihrer Web-App an. Zum Beispiel:

| URL               | Ergebnis|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | Webanwendung |
| `http://mysite.scm.azurewebsites.net/` | Kudu-Dienst |

Klicken Sie auf das Menüelement [Debugging-Konsole](https://github.com/projectkudu/kudu/wiki/Kudu-console), um Dateien anzuzeigen, zu bearbeiten, zu löschen oder hinzuzufügen.

## <a name="additional-resources"></a>Zusätzliche Ressourcen

- [Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) erleichtert das Bereitstellen von Webanwendungen und Websites auf IIS-Server.

- [https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Probleme melden und Features für die Bereitstellung anfordern.
