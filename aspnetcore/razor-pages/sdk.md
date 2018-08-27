---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: Erfahren Sie, wie Razor Pages in ASP.NET Core codierungsseitige Szenarios einfacher und produktiver gestalten als MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 04/12/2018
uid: razor-pages/sdk
ms.openlocfilehash: 4dd48b13272ed847ff83e8826e10678d732b53f9
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2018
ms.locfileid: "42909065"
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Das [!INCLUDE[](~/includes/2.1-SDK.md)] enthält das MSBuild SDK `Microsoft.NET.Sdk.Razor` (Razor SDK). Das Razor SDK:

* Standardisiert die Vorgänge zum Erstellen, Verpacken und Veröffentlichen von Projekten, die [Razor](xref:mvc/views/razor)-Dateien für auf ASP.NET Core MVC basierende Projekte enthalten.
* Enthält eine vordefinierte Reihe von Zielen, Eigenschaften und Elementen, die das Anpassen der Razor-Dateien ermöglichen.

## <a name="prerequisites"></a>Erforderliche Komponenten

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>Verwenden des Razor SDK

Die meisten Web-Apps müssen nicht ausdrücklich auf das Razor SDK verweisen. 

So verwenden Sie das Razor SDK zum Erstellen von Klassenbibliotheken, die Razor-Ansichten oder Razor-Seiten enthalten:

* Verwenden Sie `Microsoft.NET.Sdk.Razor` anstelle von `Microsoft.NET.Sdk`:
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* In der Regel wird ein Paketverweis auf `Microsoft.AspNetCore.Mvc` benötigt, um zusätzliche Abhängigkeiten zum Erstellen und Kompilieren von Razor-Seiten und Razor-Ansichten zu importieren. Ihr Projekt benötigt mindestens die folgenden Paketverweise:

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 Diese Pakete sind in `Microsoft.AspNetCore.Mvc` enthalten. Das folgende Markup zeigt eine grundlegende *CSPROJ*-Datei, die das Razor SDK verwendet, um Razor-Dateien für eine Razor-Seiten-App in ASP.NET Core zu erstellen:
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a>Eigenschaften

Die folgenden Eigenschaften steuern das Verhalten des Razor SDK im Rahmen eines Projektbuilds:

* `RazorCompileOnBuild`: Kompiliert die Razor-Assembly im Rahmen der Projekterstellung und gibt sie aus, wenn `true` festgelegt wird. Wird standardmäßig auf `true` festgelegt.
* `RazorCompileOnPublish`: Kompiliert die Razor-Assembly im Rahmen der Projektveröffentlichung und gibt sie aus, wenn `true` festgelegt wird. Wird standardmäßig auf `true` festgelegt.

Die folgenden Eigenschaften und Elemente werden zum Konfigurieren der Eingabe und Ausgabe an das Razor SDK verwendet:

| Elemente                                         | Beschreibung                                                                   |
| ------------                                  | -------------                                                                 |
| RazorGenerate                                 | Item-Elemente (*CSHTML*-Dateien), die Eingaben für Codegenerierungsziele sind. |
| RazorCompile                                  | Item-Elemente (CS-Dateien), die Eingaben für Razor-Kompilierungsziele sind. Verwenden Sie dieses ItemGroup-Element, um zusätzliche Dateien für die Kompilierung in die Razor-Assembly anzugeben. |
| RazorTargetAssemblyAttribute                  | Item-Elemente, die für das Codieren von Generate-Attributen für die Razor-Assembly verwendet werden. Zum Beispiel:  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| RazorEmbeddedResource                         | Item-Elemente, die als eingebettete Ressourcen in die Razor-Assembly hinzugefügt werden. |

| Eigenschaft                                      | Beschreibung                                                                   |
| ------------                                  | -------------                                                                 |
| RazorTargetName                               | Dateiname (ohne Erweiterung) der Assembly, die von Razor erstellt wurde. | 
| RazorOutputPath                               | Das Razor-Ausgabeverzeichnis.                                      |
| RazorCompileToolset                           | Wird verwendet, um das Toolset für die Erstellung der Razor-Assembly zu bestimmen. Gültige Werte sind `Implicit` und `PrecompilationTool`. |
| EnableDefaultContentItems                     | Enthält bestimmte Dateitypen als Inhalt im Projekt, z.B. *CSHTML*-Dateien, wenn `true` festgelegt ist. Außerdem sind alle Dateien unter *wwwroot* und alle Konfigurationsdateien enthalten, wenn darauf über Microsoft.NET.Sdk.Web verwiesen wird.         |
| EnableDefaultRazorGenerateItems               | Enthält *CSHTML*-Dateien aus `Content`-Elementen in `RazorGenerate`-Elementen, wenn `true` festgelegt ist. |
| GenerateRazorTargetAssemblyInfo               | Generiert eine *CS*-Datei, die von `RazorAssemblyAttribute` angegebene Attribute enthält, und schließt diese in der Kompilierungsausgabe ein, wenn `true` festgelegt ist. |
| EnableDefaultRazorTargetAssemblyInfoAttributes | Fügt einen Standardsatz von Assembly-Attributen zu `RazorAssemblyAttribute` hinzu, wenn `true` festgelegt ist. |
| CopyRazorGenerateFilesToPublishDirectory       | Kopiert RazorGenerate-Elemente (*CSHTML*-Dateien) in das Veröffentlichungsverzeichnis, wenn `true` festgelegt ist. In der Regel sind Razor-Dateien nicht für eine veröffentlichte Anwendung erforderlich, wenn sie an der Kompilierung zum Zeitpunkt der Erstellung oder Veröffentlichung beteiligt sind. Wird standardmäßig auf `false` festgelegt. |
| CopyRefAssembliesToPublishDirectory            | Kopiert Referenzassembly-Elemente in das Veröffentlichungsverzeichnis, wenn `true` festgelegt ist. In der Regel sind Referenzassemblys nicht für eine veröffentlichte Anwendung erforderlich, wenn die Razor-Kompilierung zum Zeitpunkt der Erstellung oder Veröffentlichung stattfindet. Wird auf `true` festgelegt, wenn Ihre veröffentlichte Anwendung Runtime-Kompilierung erfordert, z.B. wenn sie zur Runtime CSHTML-Dateien ändert oder Ansichten einbettet. Wird standardmäßig auf `false` festgelegt. |
| IncludeRazorContentInPack                      | Alle Razor-Inhaltselemente (*CSHTML*-Dateien) werden für die Aufnahme in das generierte NuGet-Paket markiert, wenn `true` festgelegt ist. Wird standardmäßig auf `false` festgelegt. |
| EmbedRazorGenerateSources | Fügt RazorGenerate-Elemente (*CSHTML*-Dateien) als eingebettete Dateien in die generierte Razor-Assembly hinzu, wenn `true` festgelegt ist. Wird standardmäßig auf `false` festgelegt. |
| UseRazorBuildServer                           | Verwendet einen dauerhaften Buildserverprozess, um die Auslastung durch die Codegenerierung zu verlagern, wenn `true` festgelegt ist. Wird standardmäßig auf den Wert `UseSharedCompilation` festgelegt. |

### <a name="targets"></a>Ziele
Das Razor SDK definiert zwei Hauptziele:

* `RazorGenerate`: Der Code generiert *CS*-Dateien aus RazorGenerate-Item-Elementen. Verwenden Sie die Eigenschaft `RazorGenerateDependsOn`, um zusätzliche Ziele anzugeben, die vor oder nach diesem Ziel ausgeführt werden können.
* `RazorCompile`: Kompiliert generierte *CS*-Dateien in eine Razor-Assembly. Verwenden Sie `RazorCompileDependsOn`, um zusätzliche Ziele anzugeben, die vor oder nach diesem Ziel ausgeführt werden können.

### <a name="runtime-compilation-of-razor-views"></a>Kompilierung von Razor-Ansichten zur Laufzeit

* Standardmäßig veröffentlicht das Razor SDK keine Verweisassemblys, die für das Durchführen der Laufzeitkompilierung erforderlich sind. Dadurch kommt es zu Kompilierungsfehlern, wenn das Anwendungsmodell von der Laufzeitkompilierung abhängt (z.B. wenn die App eingebettete Ansichten verwendet oder Ansichten nach dem Veröffentlichen der App verändert werden). Legen Sie `CopyRefAssembliesToPublishDirectory` auf `true` fest, um mit dem Veröffentlichen von Verweisassemblys fortzufahren.

* Stellen Sie für Webanwendungen sicher, dass das `Microsoft.NET.Sdk.Web` SDK als Ziel verwendet wird.
