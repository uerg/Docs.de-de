---
title: Erweitern einer App mit einer externen Assembly in ASP.NET Core mit IHostingStartup
author: guardrex
description: Erfahren Sie, wie Sie eine ASP.NET Core-App aus einer externen Assembly mithilfe einer Implementierung von IHostingStartup erweitern.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: a06c2da04c1631f5811a535c891ca5190b0d8864
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207536"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a>Erweitern einer App mit einer externen Assembly in ASP.NET Core mit IHostingStartup

Von [Luke Latham](https://github.com/guardrex)

Mit einer [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup)-Implementierung (Hostingstart) werden Verbesserungen an einer App beim Start von einer externen Assemblys aus vorgenommen. Eine externe Bibliothek kann beispielsweise eine Hostingstartimplementierung verwenden, um zusätzliche Konfigurationsanbieter oder -dienste für eine App bereitzustellen. `IHostingStartup` *ist in ASP.NET Core 2.0 und höher verfügbar*.

[Anzeigen oder Herunterladen von Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([Vorgehensweise zum Herunterladen](xref:index#how-to-download-a-sample))

## <a name="hostingstartup-attribute"></a>Das Attribut HostingStartup

Ein [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute)-Attribut gibt an, ob eine Hostingstartassembly vorhanden ist, die zur Laufzeit aktiviert werden kann.

Bei der Einstiegsassembly oder bei der Assembly, die die Klasse `Startup` enthält, wird automatisch geprüft, ob sie das Attribut `HostingStartup` enthält. Die Liste der Assemblys für die Suche nach `HostingStartup`-Attributen wird zur Laufzeit aus der Konfiguration in [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) geladen. Die Liste der Assemblys, die aus der Ermittlung ausgeschlossen werden sollen, wird aus [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey) geladen. Weitere Informationen finden Sie unter [Webhost: Hostingstartassemblys](xref:fundamentals/host/web-host#hosting-startup-assemblies) und [Web Host: Hosting Startup Exclude Assemblies (Webhost: Hostingstartausschlussassemblys)](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).

Im folgenden Beispiel lautet der Namespace der Hostingstartassembly `StartupEnhancement`. `StartupEnhancementHostingStartup` ist die Klasse mit dem Hostingstartcode:

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

Das Attribut `HostingStartup` befindet sich in der Regel in der `IHostingStartup`-Implementierungsklassendatei der Hostingstartassembly.

## <a name="discover-loaded-hosting-startup-assemblies"></a>Ermitteln geladener Hostingstartassemblys

Zum Ermitteln von geladenen Hostingstartassemblys aktivieren Sie die Protokollierung und überprüfen die Protokolle der App. Fehler, die beim Laden von Assemblys auftreten, werden protokolliert. Geladene Hostingstartassemblys werden auf der Debugebene protokolliert. Außerdem werden alle Fehler protokolliert.

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a>Deaktivieren des automatischen Ladens von Hostingstartassemblys

::: moniker range=">= aspnetcore-2.1"

Um das automatische Laden von Hostingstartassemblys zu deaktivieren, verwenden Sie einen der folgenden Ansätze:

* Um zu verhindern, dass alle Hostingstartassemblys geladen werden, legen Sie eine der folgenden Einstellungen auf `true` oder `1` fest:
  * Hostkonfigurationseinstellung [Verhindern des Hostingstarts](xref:fundamentals/host/web-host#prevent-hosting-startup).
  * Die Umgebungsvariable `ASPNETCORE_PREVENTHOSTINGSTARTUP`
* Um zu verhindern, dass bestimmte Hostingstartassemblys geladen werden, legen Sie eine der folgenden Einstellungen auf eine durch Semikolons getrennte Zeichenfolge mit Hostingstartassemblys fest, die beim Starten ausgeschlossen werden sollen:
  * Hostkonfigurationseinstellung [Hostingstartausschlussassemblys](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).
  * Die Umgebungsvariable `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Um das automatische Laden von Hostingstartassemblys zu deaktivieren, legen Sie eine der folgenden Einstellungen auf `true` oder `1` fest:

* Hostkonfigurationseinstellung [Verhindern des Hostingstarts](xref:fundamentals/host/web-host#prevent-hosting-startup).
* Die Umgebungsvariable `ASPNETCORE_PREVENTHOSTINGSTARTUP`

::: moniker-end

Wenn sowohl die Konfigurationseinstellung für den Host als auch die Umgebungsvariable festgelegt werden, wird das Verhalten durch die Hosteinstellung gesteuert.

Durch das Deaktivieren von Hostingstartassemblys mithilfe der Hosteinstellung oder der Umgebungsvariable wird die Assembly global deaktiviert. Einige Eigenschaften einer App können ebenfalls deaktiviert werden.

## <a name="project"></a>Projekt

Erstellen Sie einen Hostingstart mit einem der folgenden Projekttypen:

* [Klassenbibliothek](#class-library)
* [Konsolen-App ohne Einstiegspunkt](#console-app-without-an-entry-point)

### <a name="class-library"></a>Klassenbibliothek

In einer Klassenbibliothek kann eine Hostingstarterweiterung bereitgestellt werden. Die Bibliothek enthält ein `HostingStartup`-Attribut.

Der [Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) enthält die Razor Pages-App *HostingStartupApp* und die Klassenbibliothek *HostingStartupLibrary*. Die Klassenbibliothek:

* Enthält die Hostingstartklasse `ServiceKeyInjection`, die `IHostingStartup` implementiert. `ServiceKeyInjection` fügt mithilfe des Anbieters der Konfiguration im Arbeitsspeicher ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)) der App-Konfiguration zwei Dienstzeichenfolgen hinzu.
* Enthält ein `HostingStartup`-Attribut, das den Namespace und die Klasse des Hostingstarts angibt.

Die [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure)-Methode der Klasse `ServiceKeyInjection` verwendet [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder), um einer App Erweiterungen hinzuzufügen. `IHostingStartup.Configure` wird in der Hoststartassembly von der Runtime vor `Startup.Configure` im Benutzercode aufgerufen. Dadurch kann Benutzercode jede Konfiguration der Hoststartassembly überschreiben.

*HostingStartupLibrary/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

Die Indexseite der App liest und rendert die Konfigurationswerte für die beiden Schlüssel, die von der Hostingstartassembly der Klassenbibliothek festgelegt werden:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

Der [Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) enthält darüber hinaus auch ein NuGet-Paketprojekt, das den separaten Hostingstart *HostingStartupPackage* bereitstellt. Das Paket weist dieselben Merkmale wie die bereits beschriebene Klassenbibliothek auf. Das Paket:

* Enthält die Hostingstartklasse `ServiceKeyInjection`, die `IHostingStartup` implementiert. `ServiceKeyInjection` fügt der App-Konfiguration zwei Dienstzeichenfolgen hinzu.
* Enthält ein `HostingStartup`-Attribut.

*HostingStartupPackage/ServiceKeyInjection.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

Die Indexseite der App liest und rendert die Konfigurationswerte für die beiden Schlüssel, die von der Hostingstartassembly der Bibliothek festgelegt werden:

*HostingStartupApp/Pages/Index.cshtml.cs*:

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a>Konsolen-App ohne Einstiegspunkt

*Dieses Verfahren ist nur für .NET Core-Apps, nicht jedoch für .NET Framework verfügbar.*

Eine dynamische Hostingstarterweiterung, die zur Aktivierung keinen Kompilierungszeitverweis erfordert, kann in einer Konsolen-App ohne Einstiegspunkt bereitgestellt werden. Die App enthält ein `HostingStartup`-Attribut. So erstellen Sie einen dynamischen Hostingstart:

1. Aus der Klasse, die die `IHostingStartup`-Implementierung enthält wird eine Implementierungsbibliothek erstellt. Die Implementierungsbibliothek wird wie ein gewöhnliches Paket behandelt.
1. Eine Konsolen-App ohne Einstiegspunkt verweist auf das Paket der Implementierungsbibliothek. Eine Konsolen-App wird aus folgenden Gründen verwendet:
   * Eine Abhängigkeitendatei ist ein ausführbares App-Objekt, sodass eine Bibliothek keine Abhängigkeitendateien bereitstellen kann.
   * Eine Bibliothek kann dem [Laufzeitpaketspeicher](/dotnet/core/deploying/runtime-store), der ein ausführbares Projekt benötigt, das die freigegebene Laufzeit als Ziel verwendet, nicht direkt hinzugefügt werden.
1. Die Konsolen-App wird veröffentlicht, um die Abhängigkeiten des Hostingstarts abzurufen. Die Veröffentlichung der Konsolen-App hat unter anderem zur Folge, dass nicht verwendete Abhängigkeiten aus der Abhängigkeitendatei entfernt werden.
1. Die App und die dazu gehörende Abhängigkeitendatei werden im Laufzeitpaketspeicher abgelegt. Damit die Hostingstartassembly und die entsprechende Abhängigkeitendatei erkannt werden, wird auf sie in zwei Umgebungsvariablen verwiesen.

Die Konsolen-App verweist auf das Paket [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

Ein [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute)-Attribut identifiziert eine Klasse bei der Erstellung von [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) als eine Implementierung von `IHostingStartup` zum Laden und Ausführen. Im folgenden Beispiel ist der Namespace `StartupEnhancement` und die Klasse ist `StartupEnhancementHostingStartup`:

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

Eine Klasse implementiert `IHostingStartup`. Die [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure)-Methode der Klasse verwendet [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder), um einer App Erweiterungen hinzuzufügen. `IHostingStartup.Configure` wird in der Hoststartassembly von der Runtime vor `Startup.Configure` im Benutzercode aufgerufen. Dadurch kann Benutzercode jede Konfiguration der Hoststartassembly überschreiben.

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

Die Datei für die Abhängigkeiten (*\*.deps.json*) legt beim Erstellen eine `IHostingStartup`-Projekts den `runtime`-Speicherort der Assembly auf den *bin*-Ordner fest:

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

Nur ein Teil der Datei wird angezeigt. `StartupEnhancement` ist der Name der Assembly im Beispiel.

## <a name="specify-the-hosting-startup-assembly"></a>Angeben der Hostingstartassembly

Geben Sie für einen von der Klassenbibliothek oder von der Konsolen-App bereitgestellten Hostingstart den Namen der Hostingstartassembly in der Umgebungsvariablen `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` an. Die Umgebungsvariable ist eine durch Semikolons getrennte Liste von Assemblys.

Nur in Hostingstartassemblys wird nach dem `HostingStartup`-Attribut gesucht. Damit die Beispiel-App *HostingStartupApp* die weiter oben beschriebenen Hostingstarts erkennt, wird für die Umgebungsvariable der folgende Wert festgelegt:

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

Eine Hostingstartassembly kann auch mithilfe der Hostkonfigurationseinstellung [Hostingstartassemblys](xref:fundamentals/host/web-host#hosting-startup-assemblies) festgelegt werden.

Wenn mehrere Hoststartassemblys vorhanden sind, werden ihre [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure)-Methoden in der Reihenfolge der aufgelisteten Assemblys ausgeführt.

## <a name="activation"></a>Aktivierung

Optionen für die Hostingstartaktivierung:

* [Laufzeitspeicher](#runtime-store) &ndash; Für die Aktivierung ist kein Kompilierzeitverweis erforderlich. Die Beispiel-App legt die Hostingstartassembly und die Abhängigkeitendateien im Ordner *deployment* ab, um die Bereitstellung des Hostingstarts in einer Umgebung mit mehreren Computern zu erleichtern. Der Ordner *deployment* enthält darüber hinaus auch ein PowerShell-Skript, das im Bereitstellungssystem Umgebungsvariablen erstellt oder ändert, um den Hostingstart zu ermöglichen.
* Kompilierzeitverweis für die Aktivierung erforderlich
  * [NuGet-Paket](#nuget-package)
  * [Ordner „Bin“ des Projekts](#project-bin-folder)

### <a name="runtime-store"></a>Laufzeitspeicher

Die Hostingstartimplementierung wird im [Laufzeitspeicher](/dotnet/core/deploying/runtime-store) abgelegt. Ein Kompilierzeitverweis auf die Assembly wird von der erweiterten App nicht benötigt.

Nach dem Erstellen des Hostingstarts dient die Projektdatei des Hostingstarts als Manifestdatei für den Befehl [dotnet store](/dotnet/core/tools/dotnet-store).

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

Mit diesem Befehl werden die Hostingstartassembly und andere Abhängigkeiten, die nicht Teil des freigegebenen Frameworks sind, im Laufzeitspeicher des Benutzerprofils an der folgenden Stelle abgelegt:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

Wenn Sie die Assembly und die Abhängigkeiten zur globalen Verwendung ablegen möchten, fügen Sie dem `dotnet store`-Befehl die `-o|--output`-Option mit dem folgenden Pfad hinzu:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

**Ändern und Ablegen der Abhängigkeitendatei des Hostingstarts**

Der Runtime-Speicherort wird in der *\*.deps.json*-Datei angegeben. Das `runtime`-Element muss den Speicherort der Laufzeitassembly der Erweiterung angeben, um die Erweiterung zu aktivieren. Setzen Sie dem `runtime`-Speicherort `lib/<TARGET_FRAMEWORK_MONIKER>/` voran:

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

Im Beispielcode (*StartupDiagnostics*-Projekt) wird die Änderung der *\*.deps.json*-Datei von einem [PowerShell](/powershell/scripting/powershell-scripting)-Skript durchgeführt. Das PowerShell-Skript wird automatisch von einem Buildziel in der Projektdatei gestartet.

Die *\*.deps.json*-Datei der Implementierung muss sich an einem erreichbaren Speicherort befinden.

Platzieren Sie die Datei im Ordner *additonalDeps* der `.dotnet`-Einstellung des Benutzerprofils für die Verwendung pro Benutzer:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

Platzieren Sie die Datei im *additonalDeps*-Ordner der .NET Core-Installation für die globale Verwendung:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

Beachten Sie, dass die freigegebene Frameworkversion die Version der freigegebenen Runtime angibt, die die Ziel-App verwendet. Die freigegebene Runtime wird in der *\*.runtimeconfig.json*-Datei angezeigt. In der Beispiel-App (*HostingStartupApp*) wird die freigegebene Laufzeit in der Datei *HostingStartupApp.runtimeconfig.json* angegeben.

**Angeben der Abhängigkeitendatei des Hostingstarts**

Der Speicherort der *\*.deps.json*-Datei der Implementierung ist in der Umgebungsvariablen `DOTNET_ADDITIONAL_DEPS` angegeben.

Wenn die Datei im Ordner *.dotnet* des Benutzerprofils abgelegt wird, legen Sie den Wert der Umgebungsvariablen wie folgt fest:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

Wenn die Datei in der .NET Core-Installation für die globale Verwendung platziert wird, geben Sie den vollständigen Pfad zur Datei an:

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

Damit die Beispiel-App (*HostingStartupApp*) die Abhängigkeitendatei (*HostingStartupApp.runtimeconfig.json*) findet, wird die Abhängigkeitendatei im Profil des Benutzers abgelegt.

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Legen Sie für die Umgebungsvariable `DOTNET_ADDITIONAL_DEPS` den folgenden Wert fest:

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[macOS](#tab/macos)

Legen Sie für die Umgebungsvariable `DOTNET_ADDITIONAL_DEPS` den folgenden Wert fest:

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Legen Sie für die Umgebungsvariable `DOTNET_ADDITIONAL_DEPS` den folgenden Wert fest:

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

Beispiele zum Festlegen von Umgebungsvariablen für verschiedene Betriebssysteme finden Sie unter [Use multiple environments (Verwenden mehrerer Umgebungen)](xref:fundamentals/environments).

**Bereitstellung**

Um die Bereitstellung eines Hostingstarts in einer Umgebung mit mehreren Computern zu erleichtern, erstellt die Beispiel-App in der veröffentlichten Ausgabe einen *deployment*-Ordner, der Folgendes enthält:

* Die Hostingstartassembly.
* Die Abhängigkeitendatei des Hostingstarts.
* Ein PowerShell-Skript, das die `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` und `DOTNET_ADDITIONAL_DEPS` erstellt oder ändert, um die Aktivierung des Hostingstarts zu unterstützen. Führen Sie das Skript über eine PowerShell-Administratoreingabeaufforderung auf dem Bereitstellungssystem aus.

### <a name="nuget-package"></a>NuGet-Paket

In einem NuGet-Paket kann eine Hostingstarterweiterung bereitgestellt werden. Das Paket enthält ein `HostingStartup`-Attribut. Die vom Paket bereitgestellten Hostingstarttypen werden für die App mit einem der folgenden Verfahren verfügbar gemacht:

* Die Projektdatei der erweiterten App erstellt einen Paketverweis für den Hostingstart in der Projektdatei der App (Kompilierzeitverweis). Wenn ein Kompilierzeitverweis eingerichtet ist, werden die Hostingstartassembly und alle dazu gehörenden Abhängigkeiten in die Abhängigkeitendatei der App (*\*.deps.json*) eingebunden. Dieses Verfahren gilt für ein in [nuget.org](https://www.nuget.org/) veröffentlichtes Hostingstartassembly-Paket.
* Die Abhängigkeitendatei des Hostingstarts wird für die erweiterte App wie im Abschnitt [Laufzeitspeicher](#runtime-store) beschrieben (ohne Kompilierzeitverweis) verfügbar gemacht.

Weitere Informationen zu NuGet-Paketen und den Laufzeitspeicher finden Sie in den folgenden Themen:

* [So erstellen Sie ein NuGet-Paket mit plattformübergreifenden Tools](/dotnet/core/deploying/creating-nuget-packages)
* [Veröffentlichen von Paketen](/nuget/create-packages/publish-a-package)
* [Laufzeitpaketspeicher](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a>Ordner „Bin“ des Projekts

Eine Hostingstarterweiterung kann durch eine mittels *bin* bereitgestellte Assembly in der erweiterten App bereitgestellt werden. Die von der Assembly bereitgestellten Hostingstarttypen werden für die App mit einem der folgenden Verfahren verfügbar gemacht:

* Die Projektdatei der erweiterten App enthält einen Assemblyverweis auf den Hostingstart (Kompilierzeitverweis). Wenn ein Kompilierzeitverweis eingerichtet ist, werden die Hostingstartassembly und alle dazu gehörenden Abhängigkeiten in die Abhängigkeitendatei der App (*\*.deps.json*) eingebunden. Dieses Verfahren wird angewendet, wenn das Bereitstellungsszenario verlangt, dass die kompilierte Assembly der Hostingstartbibliothek (DLL-Datei) in das verwendete Projekt oder an einen Speicherort verschoben wird, auf den das verwendete Projekt zugreifen kann, und ein Kompilierzeitverweis auf die Assembly des Hostingstarts erstellt wird.
* Die Abhängigkeitendatei des Hostingstarts wird für die erweiterte App wie im Abschnitt [Laufzeitspeicher](#runtime-store) beschrieben (ohne Kompilierzeitverweis) verfügbar gemacht.

## <a name="sample-code"></a>Beispielcode

Der [Beispielcode](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([Herunterladen](xref:index#how-to-download-a-sample)) zeigt Szenarios für die Hostingstartimplementierung:

* Zwei Hostingstartassemblys (Klassenbibliotheken) legen zwei Schlüssel-Wert-Paare der Konfiguration im Arbeitsspeicher fest:
  * NuGet-Paket (*HostingStartupPackage*)
  * Klassenbibliothek (*HostingStartupLibrary*)
* Ein Hostingstart wird über eine im Laufzeitspeicher bereitgestellte Assembly (*StartupDiagnostics*) aktiviert. Die Assembly fügt der App zum Start zwei Middlewares hinzu, die Diagnoseinformationen zu folgenden Komponenten bereitstellen:
  * Registrierte Dienste
  * Adresse (Schema, Host, Basispfad, Pfad, Abfragezeichenfolge)
  * Verbindung (Remote-IP, Remoteport, lokale IP, lokaler Port, Clientzertifikat)
  * Anforderungsheader
  * Umgebungsvariablen

So führen Sie das Beispiel aus:

**Aktivierung über ein NuGet-Paket**

1. Kompilieren Sie das Paket *HostingStartupPackage* mit dem Befehl [dotnet pack](/dotnet/core/tools/dotnet-pack).
1. Fügen Sie den Assemblynamen des Pakets *HostingStartupPackage* zur Umgebungsvariablen `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` hinzu.
1. Kompilieren Sie die App, und führen Sie diese aus. In der erweiterten App ist ein Paketverweis vorhanden (Kompilierzeitverweis). Eine `<PropertyGroup>` in der Projektdatei der App gibt die Ausgabe des Paketprojekts (*../HostingStartupPackage/bin/Debug*) als Paketquelle an. Dadurch kann die Anwendung das Paket verwenden, ohne das Paket in [nuget.org](https://www.nuget.org/) hochzuladen. Weitere Informationen finden Sie in den Hinweisen in der Projektdatei von HostingStartupApp.

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. Die von der Indexseite gerenderten Schlüsselwerte der Dienstkonfiguration entsprechen den durch die `ServiceKeyInjection.Configure`-Methode des Pakets festgelegten Werten.

Wenn Sie am Projekt *HostingStartupPackage* Änderungen vornehmen und das Projekt erneut kompilieren, bereinigen Sie die lokalen NuGet-Paketcaches, um sicherzustellen, dass die *HostingStartupApp* das aktualisierte Paket erhält und kein veraltetes Paket aus dem lokalen Cache. Um die lokalen NuGet-Caches zu bereinigen, führen Sie den folgenden [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals)-Befehl aus:

```console
dotnet nuget locals all --clear
```

**Aktivierung über eine Klassenbibliothek**

1. Kompilieren Sie die Klassenbibliothek *HostingStartupLibrary* mit dem Befehl [dotnet build](/dotnet/core/tools/dotnet-build).
1. Fügen Sie der Umgebungsvariablen `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` den Assemblynamen *HostingStartupLibrary* der Klassenbibliothek hinzu.
1. Stellen Sie die Assembly der Klassenbibliothek mittels *bin* für die App bereit, indem Sie die Datei *HostingStartupLibrary.dll* aus der kompilierten Ausgabe der Klassenbibliothek in den Ordner *bin/Debug* der App kopieren.
1. Kompilieren Sie die App, und führen Sie diese aus. Eine `<ItemGroup>` in der Projektdatei der App verweist auf die Assembly der Klassenbibliothek (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (Kompilierzeitverweis). Weitere Informationen finden Sie in den Hinweisen in der Projektdatei von HostingStartupApp.

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. Die von der Indexseite gerenderten Schlüsselwerte der Dienstkonfiguration entsprechen den durch die `ServiceKeyInjection.Configure`-Methode der Klassenbibliothek festgelegten Werten.

**Aktivierung über eine mittels Laufzeitspeicher bereitgestellten Assembly**

1. Im Projekt *StartupDiagnostics* wird [PowerShell](/powershell/scripting/powershell-scripting) verwendet, um die Datei *StartupDiagnostics.deps.json* zu bearbeiten. PowerShell ist standardmäßig auf Windows-Betriebssystemen ab Windows 7 SP1 und Windows Server 2008 R2 SP1 installiert. Im Artikel [Installing Windows PowerShell (Installieren von Windows PowerShell)](/powershell/scripting/setup/installing-powershell#powershell-core) erfahren Sie, wie Sie PowerShell auf anderen Plattformen nutzen können.
1. Erstellen Sie das Projekt *StartupDiagnostics*. Nachdem das Projekt erstellt wurde, werden von einem Buildziel in der Projektdatei automatisch folgende Aktionen ausgeführt:
   * Löst das PowerShell-Skript aus, um die *StartupDiagnostics.deps.json*-Datei zu ändern.
   * Verschiebt die Datei *StartupDiagnostics.deps.json* in den Ordner *additionalDeps* des Benutzerprofils.
1. Führen Sie den Befehl `dotnet store` über eine Eingabeaufforderung im Verzeichnis des Hostingstarts aus, um die Assembly und die dazu gehörenden Abhängigkeiten im Laufzeitspeicher des Benutzerprofils zu speichern:

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   Bei Windows wird für den Befehl der `win7-x64` [Laufzeitbezeichner (Runtime Identifier (RID))](/dotnet/core/rid-catalog) verwendet. Wenn der Hostingstart für eine andere Laufzeit bereitgestellt wird, muss die entsprechende RID eingegeben werden.
1. Legen Sie die folgenden Umgebungsvariablen fest:
   * Fügen Sie der Umgebungsvariablen `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` den Assemblynamen *StartupDiagnostics* hinzu.
   * Legen Sie bei Windows die Umgebungsvariable `DOTNET_ADDITIONAL_DEPS` auf `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\` fest. Legen Sie bei macOS/Linux die Umgebungsvariable `DOTNET_ADDITIONAL_DEPS` auf `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, wobei `<USER>` das Benutzerprofil ist, das den Hostingstart enthält.
1. Führen Sie die Beispiel-App aus.
1. Fordern Sie den Endpunkt `/services` an, um die registrierten Dienste der App anzuzeigen. Fordern Sie den Endpunkt `/diag` an, um Diagnoseinformationen anzuzeigen.
