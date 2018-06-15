---
title: Problembehandlung bei ASP.NET Core-Projekten
author: Rick-Anderson
description: Grundlagen und Problembehandlung für Warnungen und Fehlern mit Assistenten für ASP.NET Core-Projekte.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: test/troubleshoot
ms.openlocfilehash: 8ff8ddaf4a35a02db650ff429ffbbf4e76a53ecf
ms.sourcegitcommit: 4e3497bda0c3e5011ffba3717eb61a1d46c61c15
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2018
ms.locfileid: "35613004"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Problembehandlung bei ASP.NET Core-Projekten

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Die folgenden Links erhalten Sie Anleitungen zur Fehlerbehebung:

* [Problembehandlung bei ASP.NET Core in Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
* [Problembehandlung bei ASP.NET Core in IIS](xref:host-and-deploy/iis/troubleshoot)
* [Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [NDC Konferenz (London 2018): Diagnostizieren von Problemen in ASP.NET-Anwendungen für Core](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [ASP.NET-Blog: Beheben von Leistungsproblemen für ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>.NET Core SDK-Warnungen

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Sowohl die 32-Bit und 64-Bit-Versionen von .NET Core SDK installiert sind

In der **neues Projekt** Dialogfeld für ASP.NET Core, können Sie die folgende Warnung angezeigt:

> 32- und 64-Bit-Versionen von .NET Core SDK installiert sind. Nur Vorlagen für die 64-Bit-Versionen installiert "" c: "\\Programmdateien\\Dotnet\\Sdk\\" wird angezeigt.

![Screenshot des Dialogfelds OneASP.NET mit der Warnung](troubleshoot/_static/both32and64bit.png)

Diese Warnung wird angezeigt, wenn (x86) 32-Bit und 64-Bit (x 64)-Versionen der [.NET Core SDK](https://www.microsoft.com/net/download/all) installiert sind. Häufige Ursachen für die beide Versionen installiert werden können umfassen:

* Sie ursprünglich heruntergeladen eine 32-Bit-Computer mit .NET Core SDK-Installationsprogramm aber anschließend kopiert er über und ihn auf einem 64-Bit-Computer installiert.
* Die 32-Bit-.NET Core SDK wurde durch eine andere Anwendung installiert.
* Die falsche Version wurde heruntergeladen und installiert.

Deinstallieren Sie die 32-Bit-.NET Core SDK, um diese Warnung zu vermeiden. Deinstallieren von **Systemsteuerung** > **Programme und Funktionen** > **deinstallieren oder Ändern eines Programms**. Wenn Sie verstehen, warum die Warnung wird ausgegeben, und deren Auswirkungen, können Sie die Warnung ignorieren.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK ist in mehreren Speicherorten installiert.

In der **neues Projekt** Dialogfeld für ASP.NET Core, können Sie die folgende Warnung angezeigt:

> .NET Core SDK ist in mehreren Speicherorten installiert. Nur Vorlagen für die SDKs an installierten "" c: "\\Programmdateien\\Dotnet\\Sdk\\" wird angezeigt.

![Screenshot des Dialogfelds OneASP.NET mit der Warnung](troubleshoot/_static/multiplelocations.png)

Diese Meldung wird angezeigt, wenn Sie mindestens eine Installation von .NET Core SDK in einem Verzeichnis außerhalb von haben *"c:"\\Programmdateien\\Dotnet\\Sdk\\*. Dies geschieht normalerweise, wenn der .NET Core SDK auf einem Computer mit Kopieren/Einfügen anstelle der MSI-Installationsprogramm bereitgestellt wurde.

Deinstallieren Sie die 32-Bit-.NET Core SDK, um diese Warnung zu vermeiden. Deinstallieren von **Systemsteuerung** > **Programme und Funktionen** > **deinstallieren oder Ändern eines Programms**. Wenn Sie verstehen, warum die Warnung wird ausgegeben, und deren Auswirkungen, können Sie die Warnung ignorieren.

### <a name="no-net-core-sdks-were-detected"></a>Es wurden keine .NET Core-SDKs erkannt.

In der **neues Projekt** Dialogfeld für ASP.NET Core, können Sie die folgende Warnung angezeigt:

> Keine .NET Core-SDKs erkannt wurden, stellen Sie sicher, dass sie in der Umgebungsvariablen "PATH" enthalten sind.

![Screenshot des Dialogfelds OneASP.NET mit der Warnung](troubleshoot/_static/NoNetCore.png)

Diese Warnung wird angezeigt, wenn die Umgebungsvariable `PATH` zeigt nicht auf eine beliebige .NET Core-SDKs auf dem Computer. Um dieses Problem zu lösen:

* Installieren, oder stellen Sie sicher, dass die .NET Core SDK installiert ist.
* Überprüfen Sie die `PATH` -Umgebungsvariable zeigt auf den Speicherort, die das SDK installiert ist. Normalerweise legt der Installer die `PATH`.

::: moniker range=">= aspnetcore-2.1"

### <a name="use-of-ihtmlhelperpartial-may-result-in-app-deadlocks"></a>Verwendung von IHtmlHelper.Partial möglicherweise in app-Deadlocks führen.

In ASP.NET Core 2.1 und höher Aufrufen `Html.Partial` führt zu einer Warnung Analyzer aufgrund der Möglichkeit von Deadlocks. Die Warnung angezeigt wird:

> Verwendung von IHtmlHelper.Partial kann in der anwendungsdeadlocks führen. Erwägen Sie `<partial>` Tag Helper oder `IHtmlHelper.PartialAsync`.

Aufrufe von `@Html.Partial` ersetzt werden sollte, indem `@await Html.PartialAsync` oder das Hilfsprogramm writeid `<partial name="_Partial" />`.

::: moniker-end
