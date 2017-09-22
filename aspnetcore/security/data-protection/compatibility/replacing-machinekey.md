---
title: Ersetzen "< MachineKey >" in ASP.NET
author: rick-anderson
description: Ersetzen "< MachineKey >" in ASP.NET
keywords: ASP.NET Core, Sicherheit, < MachineKey > machineKey
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5ac13589-3837-4b4d-8abe-81f843942120
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/compatibility/replacing-machinekey
ms.openlocfilehash: 8c00c05a1120e65f503b70229466fcad561bc6a9
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/22/2017
---
# <a name="replacing-machinekey-in-aspnet"></a><span data-ttu-id="80e9e-104">Ersetzen von `<machineKey>` in ASP.NET</span><span class="sxs-lookup"><span data-stu-id="80e9e-104">Replacing `<machineKey>` in ASP.NET</span></span>

<a name=compatibility-replacing-machinekey></a>

<span data-ttu-id="80e9e-105">Die Implementierung der `<machineKey>` Element in ASP.NET [ist ersetzbare](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span><span class="sxs-lookup"><span data-stu-id="80e9e-105">The implementation of the `<machineKey>` element in ASP.NET [is replaceable](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/).</span></span> <span data-ttu-id="80e9e-106">Dadurch werden die meisten Aufrufe von ASP.NET kryptografischen Routinen über eine Ersetzung Daten Schutzmechanismus einschließlich der neuen Datenschutzsystem geleitet werden.</span><span class="sxs-lookup"><span data-stu-id="80e9e-106">This allows most calls to ASP.NET cryptographic routines to be routed through a replacement data protection mechanism, including the new data protection system.</span></span>

## <a name="package-installation"></a><span data-ttu-id="80e9e-107">Paketinstallation</span><span class="sxs-lookup"><span data-stu-id="80e9e-107">Package installation</span></span>

> [!NOTE]
> <span data-ttu-id="80e9e-108">Die neue Datenschutzsystem kann nur in einer vorhandenen ASP.NET-Anwendung mit der Zielversion .NET 4.5.1 installiert oder höher sein.</span><span class="sxs-lookup"><span data-stu-id="80e9e-108">The new data protection system can only be installed into an existing ASP.NET application targeting .NET 4.5.1 or higher.</span></span> <span data-ttu-id="80e9e-109">Installation wird fehlschlagen, wenn die Anwendung als Ziel .NET 4.5 verwendet oder zu verringern.</span><span class="sxs-lookup"><span data-stu-id="80e9e-109">Installation will fail if the application targets .NET 4.5 or lower.</span></span>

<span data-ttu-id="80e9e-110">Um die neue Datenschutzsystem in einem vorhandenen 4.5.1+ ASP.NET-Projekt zu installieren, installieren Sie das Paket Microsoft.AspNetCore.DataProtection.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="80e9e-110">To install the new data protection system into an existing ASP.NET 4.5.1+ project, install the package Microsoft.AspNetCore.DataProtection.SystemWeb.</span></span> <span data-ttu-id="80e9e-111">Dies wird instanziiert, die Schutz System mithilfe der [Standardkonfiguration](../configuration/default-settings.md#data-protection-default-settings) Einstellungen.</span><span class="sxs-lookup"><span data-stu-id="80e9e-111">This will instantiate the data protection system using the [default configuration](../configuration/default-settings.md#data-protection-default-settings) settings.</span></span>

<span data-ttu-id="80e9e-112">Wenn Sie das Paket installieren, fügt eine Zeile in *"Web.config"* , informiert, dass ASP.NET für die Verwendung [am Kryptografievorgänge](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), einschließlich Formularauthentifizierung, Ansichtszustand und Aufrufe an MachineKey.Protect.</span><span class="sxs-lookup"><span data-stu-id="80e9e-112">When you install the package, it inserts a line into *Web.config* that tells ASP.NET to use it for [most cryptographic operations](https://blogs.msdn.microsoft.com/webdev/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2/), including forms authentication, view state, and calls to MachineKey.Protect.</span></span> <span data-ttu-id="80e9e-113">Die Zeile, die eingefügt wird, lautet wie folgt.</span><span class="sxs-lookup"><span data-stu-id="80e9e-113">The line that's inserted reads as follows.</span></span>

```xml
<machineKey compatibilityMode="Framework45" dataProtectorType="..." />
```

>[!TIP]
> <span data-ttu-id="80e9e-114">Sie können feststellen, ob das neue Data Protection System aktiv ist, durch Überprüfen von Feldern wie etwa `__VIEWSTATE`, die mit "CfDJ8" beginnen soll, wie im folgenden Beispiel.</span><span class="sxs-lookup"><span data-stu-id="80e9e-114">You can tell if the new data protection system is active by inspecting fields like `__VIEWSTATE`, which should begin with "CfDJ8" as in the example below.</span></span> <span data-ttu-id="80e9e-115">"CfDJ8" wird die base64-Darstellung des Headers Magic "09 d0 C9 d0", die eine Nutzlast, die durch die Datenschutzsystem geschützt identifiziert.</span><span class="sxs-lookup"><span data-stu-id="80e9e-115">"CfDJ8" is the base64 representation of the magic "09 F0 C9 F0" header that identifies a payload protected by the data protection system.</span></span>

```html
<input type="hidden" name="__VIEWSTATE" id="__VIEWSTATE" value="CfDJ8AWPr2EQPTBGs3L2GCZOpk..." />
```

## <a name="package-configuration"></a><span data-ttu-id="80e9e-116">Paketkonfiguration</span><span class="sxs-lookup"><span data-stu-id="80e9e-116">Package configuration</span></span>

<span data-ttu-id="80e9e-117">Die Datenschutzsystem instanziiert wird mit einer Standardkonfiguration für die Einrichtung von 0 (null).</span><span class="sxs-lookup"><span data-stu-id="80e9e-117">The data protection system is instantiated with a default zero-setup configuration.</span></span> <span data-ttu-id="80e9e-118">Jedoch, da standardmäßig Schlüssel im lokalen Dateisystem beibehalten werden, dies für Anwendungen funktioniert nicht, die in einer Farm bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="80e9e-118">However, since by default keys are persisted to the local file system, this won't work for applications which are deployed in a farm.</span></span> <span data-ttu-id="80e9e-119">Zum Beheben dieses Problems können Sie die Konfiguration durch Erstellen eines Typs welche Unterklassen DataProtectionStartup bereitstellen und überschreibt die ConfigureServices-Methode.</span><span class="sxs-lookup"><span data-stu-id="80e9e-119">To resolve this, you can provide configuration by creating a type which subclasses DataProtectionStartup and overrides its ConfigureServices method.</span></span>

<span data-ttu-id="80e9e-120">Im folgenden finden Sie ein Beispiel für eine benutzerdefinierte Daten Schutz Starttyp konfiguriert sowohl, auf dem Schlüssel gespeichert sind und wie sie im Ruhezustand verschlüsselt sind.</span><span class="sxs-lookup"><span data-stu-id="80e9e-120">Below is an example of a custom data protection startup type which configured both where keys are persisted and how they're encrypted at rest.</span></span> <span data-ttu-id="80e9e-121">Dabei wird auch die Standardrichtlinie für app-Isolation, durch die Bereitstellung der eigenen Anwendungsnamens.</span><span class="sxs-lookup"><span data-stu-id="80e9e-121">It also overrides the default app isolation policy by providing its own application name.</span></span>

```csharp
using System;
using System.IO;
using Microsoft.AspNetCore.DataProtection;
using Microsoft.AspNetCore.DataProtection.SystemWeb;
using Microsoft.Extensions.DependencyInjection;

namespace DataProtectionDemo
{
    public class MyDataProtectionStartup : DataProtectionStartup
    {
        public override void ConfigureServices(IServiceCollection services)
        {
            services.AddDataProtection()
                .SetApplicationName("my-app")
                .PersistKeysToFileSystem(new DirectoryInfo(@"\\server\share\myapp-keys\"))
                .ProtectKeysWithCertificate("thumbprint");
        }
    }
}
```

>[!TIP]
> <span data-ttu-id="80e9e-122">Sie können auch `<machineKey applicationName="my-app" ... />` anstelle eines expliziten Aufrufs von SetApplicationName.</span><span class="sxs-lookup"><span data-stu-id="80e9e-122">You can also use `<machineKey applicationName="my-app" ... />` in place of an explicit call to SetApplicationName.</span></span> <span data-ttu-id="80e9e-123">Hierbei handelt es sich um eine benutzerfreundliche-Mechanismus zum Vermeiden des Entwicklers einen DataProtectionStartup-abgeleiteten Typ zu erstellen, wenn wurde alles, was sie verwenden wollten, konfigurieren Sie den Namen der Anwendung festlegen.</span><span class="sxs-lookup"><span data-stu-id="80e9e-123">This is a convenience mechanism to avoid forcing the developer to create a DataProtectionStartup-derived type if all they wanted to configure was setting the application name.</span></span>

<span data-ttu-id="80e9e-124">Um diese benutzerdefinierte Konfiguration zu aktivieren, wechseln Sie zurück zur Datei "Web.config", und suchen Sie nach der `<appSettings>` Element, das das Paket zu installieren, auf die Datei "App.config" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="80e9e-124">To enable this custom configuration, go back to Web.config and look for the `<appSettings>` element that the package install added to the config file.</span></span> <span data-ttu-id="80e9e-125">Es sieht wie das folgende Markup:</span><span class="sxs-lookup"><span data-stu-id="80e9e-125">It will look like the following markup:</span></span>

```xml
<appSettings>
  <!--
  If you want to customize the behavior of the ASP.NET Core Data Protection stack, set the
  "aspnet:dataProtectionStartupType" switch below to be the fully-qualified name of a
  type which subclasses Microsoft.AspNetCore.DataProtection.SystemWeb.DataProtectionStartup.
  -->
  <add key="aspnet:dataProtectionStartupType" value="" />
</appSettings>
```

<span data-ttu-id="80e9e-126">Füllen Sie die leeren Wert festgelegt und die Assembly qualifizierte Name des Typs DataProtectionStartup abgeleitet, die Sie gerade erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="80e9e-126">Fill in the blank value with the assembly-qualified name of the DataProtectionStartup-derived type you just created.</span></span> <span data-ttu-id="80e9e-127">Wenn der Name der Anwendung DataProtectionDemo ist, würde dies wie folgt aussehen die unten.</span><span class="sxs-lookup"><span data-stu-id="80e9e-127">If the name of the application is DataProtectionDemo, this would look like the below.</span></span>

```xml
<add key="aspnet:dataProtectionStartupType"
     value="DataProtectionDemo.MyDataProtectionStartup, DataProtectionDemo" />
```

<span data-ttu-id="80e9e-128">Die neu konfigurierte Datenschutzsystem kann jetzt zur Verwendung in der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="80e9e-128">The newly-configured data protection system is now ready for use inside the application.</span></span>
