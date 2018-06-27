---
title: Erweitern einer App mit einer externen Assembly in ASP.NET Core mit IHostingStartup
author: guardrex
description: Erfahren Sie, wie Sie eine ASP.NET Core-App aus einer externen Assembly mithilfe einer Implementierung von IHostingStartup erweitern.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: bd9605dd8efee2c3ba8bc82a81554cace40630be
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278082"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>Erweitern einer App mit einer externen Assembly in ASP.NET Core mit IHostingStartup

Von [Luke Latham](https://github.com/guardrex)

Mit einer [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup)-Implementierung können einer App beim Start von einer externen Assemblys aus, die sich außerhalb der `Startup`-Klasse der App befindet, Erweiterungen hinzugefügt werden Eine externe Toolbibliothek kann beispielsweise eine Implementierung von `IHostingStartup` verwenden, um zusätzliche Konfigurationsanbieter oder -dienste für eine App bereitzustellen. `IHostingStartup` *ist in ASP.NET Core 2.0 und höher verfügbar.*

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample))

## <a name="discover-loaded-hosting-startup-assemblies"></a>Ermitteln geladener Hostingstartassemblys

Aktivieren Sie die Protokollierung, und überprüfen Sie die Anwendungsprotokolle, um Hostingstartassemblys zu ermitteln, die von der App oder von Bibliotheken geladen wurden. Fehler, die beim Laden von Assemblys auftreten, werden protokolliert. Geladene Hostingstartassemblys werden auf der Debugebene protokolliert. Außerdem werden alle Fehler protokolliert.

Die Beispiel-App liest den [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) in ein `string`-Array und zeigt das Ergebnis auf der Indexseite der App an:

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Deaktivieren des automatischen Ladens von Hostingstartassemblys

Es gibt zwei Möglichkeiten zum Deaktivieren des automatischen Ladens von Hostingstartassemblys:

* Festlegen der Hostkonfigurationseinstellung [Verhindern des Hostingstarts](xref:fundamentals/host/web-host#prevent-hosting-startup).
* Festlegen der Umgebungsvariable `ASPNETCORE_PREVENTHOSTINGSTARTUP`.

Wenn entweder die Hosteinstellung oder die Umgebungsvariable auf `true` oder `1` festgelegt ist, werden Hostingstartassemblys nicht automatisch geladen. Wenn beide festgelegt sind, wird das Verhalten durch die Hosteinstellung gesteuert.

Durch das Deaktivieren von Hostingstartassemblys mithilfe der Hosteinstellung oder der Umgebungsvariable, werden sie global deaktiviert. Einige Eigenschaften einer App können ebenfalls deaktiviert werden. Momentan ist es nicht möglich, eine von einer Bibliothek hinzugefügte Hostingstartassembly gezielt zu deaktivieren, außer die Bibliothek bietet ihre eigene Konfigurationsoption. Eine zukünftige Version soll die Möglichkeit bieten, Hostingstartassemblys selektiv zu deaktivieren (siehe [GitHub issue aspnet/Hosting #1243 (GitHub-Problem ASP.NET/Hosting #1243)](https://github.com/aspnet/Hosting/pull/1243)).

## <a name="implement-ihostingstartup"></a>Implementieren von IHostingStartup

### <a name="create-the-assembly"></a>Erstellen der Assembly

Eine `IHostingStartup`-Erweiterung wird als eine Assembly bereitgestellt, die auf einer Konsolen-App ohne Einstiegspunkt basiert. Die Assembly referenziert das Paket [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

Ein [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute)-Attribut identifiziert eine Klasse bei der Erstellung von [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) als eine Implementierung von `IHostingStartup` zum Laden und Ausführen. Im folgenden Beispiel ist der Namespace `StartupEnhancement` und die Klasse ist `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

Eine Klasse implementiert `IHostingStartup`. Die [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure)-Methode der Klasse verwendet [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder), um einer App Erweiterungen hinzuzufügen. `IHostingStartup.Configure` wird in der Hoststartassembly von der Runtime vor `Startup.Configure` im Benutzercode aufgerufen. Dadurch kann Benutzercode jede Konfiguration der Hoststartassembly überschreiben.

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Die Datei für die Abhängigkeiten (*\*.deps.json*) legt beim Erstellen eine `IHostingStartup`-Projekts den `runtime`-Speicherort der Assembly auf den *bin*-Ordner fest:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Nur ein Teil der Datei wird angezeigt. `StartupEnhancement` ist der Name der Assembly im Beispiel.

### <a name="update-the-dependencies-file"></a>Aktualisieren der Abhängigkeitsdatei

Der Runtime-Speicherort wird in der *\*.deps.json*-Datei angegeben. Das `runtime`-Element muss den Speicherort der Runtime-Assembly der Erweiterung angeben, um die Erweiterung zu aktivieren. Setzen Sie dem `runtime`-Speicherort `lib/<TARGET_FRAMEWORK_MONIKER>/` voran:

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

In der Beispiel-App wird die Änderung der *\*.deps.json*-Datei von einem [PowerShell-Skript](/powershell/scripting/powershell-scripting) durchgeführt. Das PowerShell-Skript wird automatisch von einem Buildziel in der Projektdatei gestartet.

### <a name="enhancement-activation"></a>Aktivierung der Erweiterung

**Platzieren der Assemblydatei**

Die Assemblydatei der `IHostingStartup`-Implementierung muss über *bin* in der App bereitgestellt oder im [Laufzeitspeicher](/dotnet/core/deploying/runtime-store) platziert werden:

Platzieren Sie die Assembly im Laufzeitspeicher des Benutzerprofils für die Verwendung pro Benutzer unter:

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

Platzieren Sie die Assembly im Laufzeitspeicher in der .NET Core-Installation für die globale Verwendung unter:

```
<DRIVE>\Program Files\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

Beim Bereitstellen der Assembly in den Laufzeitspeicher kann die Symboldatei ebenfalls bereitgestellt werden, was jedoch nicht erforderlich ist, damit die Erweiterung funktioniert.

**Platzieren der Abhängigkeitsdatei**

Die *\*.deps.json*-Datei der Implementierung muss sich an einem erreichbaren Speicherort befinden.

Platzieren Sie die Datei im `additonalDeps`-Ordner der `.dotnet`-Einstellung des Benutzerprofils für die Verwendung pro Benutzer:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

Platzieren Sie die Datei im `additonalDeps`-Ordner der .NET Core-Installation für die globale Verwendung:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

Beachten Sie, dass die freigegebene Frameworkversion die Version der freigegebenen Runtime angibt, die die Ziel-App verwendet. Die freigegebene Runtime wird in der *\*.runtimeconfig.json*-Datei angezeigt. In der Beispiel-App wird die freigegebene Runtime in der Datei *HostingStartupSample.runtimeconfig.json* angegeben.

**Festlegen von Umgebungsvariablen**

Legen Sie die folgenden Umgebungsvariablen im Kontext der App fest, die die Erweiterung verwendet.

ASPNETCORE_HOSTINGSTARTUPASSEMBLIES

Nur Hostingstartassemblys werden auf `HostingStartupAttribute` überprüft. Der Assemblyname der Implementierung wird in dieser Umgebungsvariable bereitgestellt. In der Beispiel-App wird dieser Wert auf `StartupDiagnostics` festgelegt.

Der Wert kann auch mithilfe der Hostkonfigurationseinstellung [Hostingstartassemblys](xref:fundamentals/host/web-host#hosting-startup-assemblies) festgelegt werden.

Wenn mehrere Hoststartassemblys vorhanden sind, werden ihre [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure)-Methoden in der Reihenfolge der aufgelisteten Assemblys ausgeführt.

DOTNET_ADDITIONAL_DEPS

Beschreibt den Speicherort der *\*.deps.json*-Datei der Implementierung.

Wenn die Datei im *DOTNET*-Ordner des Benutzerprofils für die Verwendung pro Benutzer platziert ist:

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

Wenn die Datei in der .NET Core-Installation für die globale Verwendung platziert wird, geben Sie den vollständigen Pfad zur Datei an:

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

In der Beispiel-App wird dieser Wert auf Folgendes festgelegt:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

Beispiele zum Festlegen von Umgebungsvariablen für verschiedene Betriebssysteme finden Sie unter [Use multiple environments (Verwenden mehrerer Umgebungen)](xref:fundamentals/environments).

## <a name="sample-app"></a>Beispiel-App

In der [Beispiel-App](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([Vorgehensweise zum Herunterladen](xref:tutorials/index#how-to-download-a-sample)) wird `IHostingStartup` verwendet, um ein Diagnosetool zu erstellen. Das Tool fügt der App zum Start zwei Middlewares hinzu, die Diagnoseinformationen bereitstellen:

* Registrierte Dienste
* Adresse: Schema, Host, Basispfad, Pfad, Abfragezeichenfolge
* Verbindung: Remote-IP, Remoteport, lokale IP, lokaler Port, Clientzertifikat
* Anforderungsheader
* Umgebungsvariablen

So führen Sie das Beispiel aus:

1. Im Startdiagnoseprojekt wird [PowerShell](/powershell/scripting/powershell-scripting) verwendet, um die *StartupDiagnostics.deps.json*-Datei zu bearbeiten. PowerShell ist standardmäßig auf Windows-Betriebssystemen ab Windows 7 SP1 und Windows Server 2008 R2 SP1 installiert. Im Artikel [Installing Windows PowerShell (Installieren von Windows PowerShell)](/powershell/scripting/setup/installing-windows-powershell) erfahren Sie, wie Sie PowerShell auf anderen Plattformen nutzen können.
2. Erstellen Sie das Startdiagnoseprojekt. Ein Buildziel in der Projektdatei:
   * Verschiebt die Assembly und Symboldateien in den Laufzeitspeicher des Benutzerprofils.
   * Löst das PowerShell-Skript aus, um die *StartupDiagnostics.deps.json*-Datei zu ändern.
   * Verschiebt die *StartupDiagnostics.deps.json*-Datei in der `additionalDeps`-Ordner des Benutzerprofils.
3. Legen Sie die folgenden Umgebungsvariablen fest:
    * `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`
    * `DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`
4. Führen Sie die Beispiel-App aus.
5. Fordern Sie den Endpunkt `/services` an, um die registrierten Dienste der App anzuzeigen. Fordern Sie den Endpunkt `/diag` an, um Diagnoseinformationen anzuzeigen.
