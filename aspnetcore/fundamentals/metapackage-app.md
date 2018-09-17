---
title: Das Metapaket „Microsoft.AspNetCore.App“ für ASP.NET Core 2.1 und höher
author: Rick-Anderson
description: Das Metapaket „Microsoft.AspNetCore.App“ enthält alle unterstützten ASP.NET Core- und Entity Framework Core-Pakete.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: d27c3aa53d6edd235006dc136f09558395e15b6e
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538452"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a>Das Metapaket „Microsoft.AspNetCore.App“ für ASP.NET Core 2.1

Für dieses Feature ist ASP.NET Core 2.1 und höher für .NET Core 2.1 und höher erforderlich.

Das [Metapaket](/dotnet/core/packages#metapackages) [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.App) für ASP.NET Core:

* Enthält keine Drittanbieterabhängigkeiten außer [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) und [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/). Diese Drittanbieterabhängigkeiten sind erforderlich, damit die Features der wichtigsten Frameworks funktionieren.
* Enthält alle unterstützten Pakete des ASP.NET Core-Teams außer denen, die Drittanbieterabhängigkeiten enthalten (von den bereits erwähnten abgesehen).
* Enthält alle unterstützten Pakete des Entity Framework Core-Teams außer denen, die Drittanbieterabhängigkeiten enthalten (von den bereits erwähnten abgesehen).

Im Paket `Microsoft.AspNetCore.App` sind alle Features von ASP.NET Core 2.1 und höher und Entity Framework Core 2.1 und höher enthalten. Die Standardprojektvorlagen für ASP.NET Core 2.1 und höher verwenden dieses Paket. Es wird empfohlen, dass Anwendungen für ASP.NET Core 2.1 und höher und Entity Framework Core 2.1 und höher das Paket `Microsoft.AspNetCore.App` verwenden.

Die Versionsnummer des Metapakets `Microsoft.AspNetCore.App` stellt die ASP.NET Core-Version und die Entity Framework Core-Version dar.

Durch die Verwendung des Metapakets `Microsoft.AspNetCore.App` werden Versionseinschränkungen bereitgestellt, die Ihre App schützen:

* Wenn ein Paket enthalten wird, das eine transitive (nicht direkte) Abhängigkeit von einem Paket in `Microsoft.AspNetCore.App` besitzt und diese Versionsnummern sich unterscheiden, generiert NuGet einen Fehler.
* Andere Pakete, die zu Ihrer App hinzugefügt werden, können die Version der in `Microsoft.AspNetCore.App` enthaltenen Pakete nicht ändern.
* Die Konsistenz von Versionen stellt eine zuverlässige Verwendbarkeit sicher. `Microsoft.AspNetCore.App` wurde dafür entwickelt, um nicht getestete Versionskombinationen von verwandten Komponenten zu verhindern, die in derselben App zusammen verwendet werden.

Anwendungen, die das Metapaket `Microsoft.AspNetCore.App` verwenden, profitieren automatisch vom freigegebenen ASP.NET Core-Framework. Bei Verwendung des Metapakets `Microsoft.AspNetCore.App` werden mit der Anwendung **keine** Objekte aus den referenzierten NuGet-Paketen für ASP.NET Core bereitgestellt. Das freigegebene ASP.NET Core-Framework enthält diese Objekte. Die Objekte im freigegebenen Framework sind zur Verbesserung der Startzeit der Anwendung vorkompiliert. Weitere Informationen zu freigegebenen Frameworks finden Sie unter [.NET Core distribution packaging (Packen von .NET Core-Verteilungen)](/dotnet/core/build/distribution-packaging).

Die folgende Projektdatei verweist auf das Metapaket `Microsoft.AspNetCore.App` für ASP.NET Core und ist eine typische ASP.NET Core 2.1-Vorlage.

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" Version="2.1.4" />
  </ItemGroup>

</Project>
```

Mit der Versionsnummer im Verweis `Microsoft.AspNetCore.App` ist **nicht** garantiert, dass diese Version des freigegebenen Frameworks verwendet wird. Es kann zum Beispiel sein, dass die Version `2.1.1` angegeben ist, aber bereits Version `2.1.3` installiert wurde. In diesem Fall verwendet die App die Version `2.1.3`. Sie können das Rollforwardverhalten für Patches und/oder kleinere Updates deaktivieren. Dies wird jedoch nicht empfohlen. Weitere Informationen zum Rollforwardverhalten von Paketversionen finden Sie unter [dotnet host-Rollforward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).

## <a name="update-aspnet-core"></a>Aktualisieren von ASP.NET Core

Das [Metapaket](/dotnet/core/packages#metapackages) `Microsoft.AspNetCore.App` zählt nicht zu den herkömmlichen Paketen, die über NuGet aktualisiert werden. Ähnlich wie bei `Microsoft.NETCore.App` stellt `Microsoft.AspNetCore.App` eine freigegebene Runtime dar, die über eine spezielle Semantik für die Versionsverwaltung verfügt, die außerhalb von NuGet behandelt wird. Weitere Informationen finden Sie unter [Pakete, Metapakete und Frameworks](/dotnet/core/packages).

So aktualisieren Sie ASP.NET Core:

* Auf den Entwicklungscomputern und Buildservern: Laden Sie das [.NET Core SDK](https://www.microsoft.com/net/download) herunter, und installieren Sie es.
* Auf Bereitstellungsservern: Laden Sie die [.NET Core-Runtime](https://www.microsoft.com/net/download) herunter, und installieren Sie sie.

 Für Anwendungen wird beim Neustart der Anwendungen ein Rollforward auf die neueste installierte Version ausgeführt. Die Versionsnummer von `Microsoft.AspNetCore.App` muss in der Projektdatei nicht aktualisiert werden. Weitere Informationen finden Sie unter [Von Frameworks abhängige Apps führen einen Rollforward aus](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).

Wenn Ihre Anwendung zuvor `Microsoft.AspNetCore.All` verwendet hat, finden Sie weitere Informationen unter [Migrieren von Microsoft.AspNetCore.All zu Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).
