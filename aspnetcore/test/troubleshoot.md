---
title: Problembehandlung bei ASP.NET Core-Projekten
author: Rick-Anderson
description: In diesem Artikel werden Warnungen und Fehler erläutert. Außerdem erfahren Sie, wie die Problembehandlung in ASP.NET Core-Projekten funktioniert.
ms.author: riande
ms.date: 04/05/2018
uid: test/troubleshoot
ms.openlocfilehash: c72dd93f6ba705d7f03ade556c7a037dadeb6295
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938393"
---
# <a name="troubleshoot-aspnet-core-projects"></a>Problembehandlung bei ASP.NET Core-Projekten

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Die folgenden Links erhalten Anleitungen zur Fehlerbehebung:

* [Problembehandlung bei ASP.NET Core in Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
* [Problembehandlung bei ASP.NET Core in IIS](xref:host-and-deploy/iis/troubleshoot)
* [Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [NDC Conference (London, 2018): Diagnostizieren von Problemen in ASP.NET Core-Anwendungen](https://www.youtube.com/watch?v=RYI0DHoIVaA)
* [ASP.NET-Blog: Problembehandlung bei Leistungsproblemen von ASP.NET Core](https://blogs.msdn.microsoft.com/webdev/2018/05/23/asp-net-core-performance-improvements/)

## <a name="net-core-sdk-warnings"></a>.NET Core SDK-Warnungen

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Die 32-Bit und 64-Bit-Versionen von .NET Core SDK sind installiert.

In der **neues Projekt** Dialogfeld für ASP.NET Core, können Sie die folgende Warnung angezeigt:

> Es werden sowohl 32- und 64-Bit-Versionen von .NET Core SDK installiert. Nur Vorlagen aus die 64-Bit-Versionen installiert ' "c:"\\Programmdateien\\Dotnet\\Sdk\\"wird angezeigt.

![Screenshot des Dialogfelds mit die Warnmeldung OneASP.NET](troubleshoot/_static/both32and64bit.png)

Diese Warnung wird angezeigt, wenn sowohl die 32-Bit-(x86) als auch die 64-Bit (x 64) Versionen der [.NET Core SDK](https://www.microsoft.com/net/download/all) installiert sind. Häufige Gründe, die beide Versionen installiert sind enthalten:

* Sie ursprünglich heruntergeladen das .NET Core SDK-Installationsprogramm, das mit einem 32-Bit-Computer, aber klicken Sie dann über kopiert und es auf einem 64-Bit-Computer installiert.
* Das 32-Bit .NET Core SDK wurde durch eine andere Anwendung installiert.
* Die falsche Version wurde heruntergeladen und installiert.

Deinstallieren Sie die 32-Bit .NET Core-SDK, um diese Warnung zu vermeiden. Deinstallieren von **Systemsteuerung** > **Programme und Funktionen** > **deinstallieren oder Ändern eines Programms**. Wenn Sie verstehen, warum die Warnung tritt auf, und deren Auswirkungen, können Sie die Warnung ignorieren.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>Das .NET Core SDK ist an mehreren Speicherorten installiert.

In der **neues Projekt** Dialogfeld für ASP.NET Core, können Sie die folgende Warnung angezeigt:

> Das .NET Core SDK ist an mehreren Speicherorten installiert. Nur Vorlagen aus dem SDK unter "C:\\Programmdateien\\Dotnet\\Sdk\\" wird angezeigt.

![Screenshot des Dialogfelds mit die Warnmeldung OneASP.NET](troubleshoot/_static/multiplelocations.png)

Diese Meldung angezeigt, wenn Sie mindestens eine Installation von .NET Core SDK in einem Verzeichnis außerhalb des haben *C:\\Programmdateien\\Dotnet\\Sdk\\*. Dies geschieht normalerweise, wenn das .NET Core SDK auf einem Computer mit Kopieren/Einfügen, anstatt das MSI-Installationsprogramm bereitgestellt wurde.

Deinstallieren Sie die 32-Bit .NET Core-SDK, um diese Warnung zu vermeiden. Deinstallieren von **Systemsteuerung** > **Programme und Funktionen** > **deinstallieren oder Ändern eines Programms**. Wenn Sie verstehen, warum die Warnung tritt auf, und deren Auswirkungen, können Sie die Warnung ignorieren.

### <a name="no-net-core-sdks-were-detected"></a>Es wurden keine .NET Core SDKs erkannt.

In der **neues Projekt** Dialogfeld für ASP.NET Core, können Sie die folgende Warnung angezeigt:

> Es wurden keine .NET Core SDKs erkannt, stellen Sie sicher, dass sie in der Umgebungsvariablen "PATH" enthalten sind.

![Screenshot des Dialogfelds mit die Warnmeldung OneASP.NET](troubleshoot/_static/NoNetCore.png)

Diese Warnung wird angezeigt, wenn die Umgebungsvariable `PATH` verweist nicht auf alle .NET Core-SDKs auf dem Computer. Um dieses Problem zu beheben:

* Installieren, oder stellen Sie sicher, dass das .NET Core SDK installiert ist.
* Überprüfen Sie, ob die `PATH` Umgebungsvariable verweist auf den Speicherort, in dem das SDK installiert ist. Der Installer normalerweise legt der `PATH`.
