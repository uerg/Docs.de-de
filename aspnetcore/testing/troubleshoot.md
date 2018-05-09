---
title: Problembehandlung für ASP.NET Core
author: Rick-Anderson
description: Grundlagen und Problembehandlung für Warnungen und Fehlern mit Assistenten für ASP.NET Core-Projekte.
manager: wpickett
ms.author: riande
ms.date: 04/05/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: testing/troubleshoot
ms.openlocfilehash: f2c785bfe27ddd67db0313b8ee1c077a8cc06e05
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2018
---
# <a name="troubleshoot-aspnet-core-projects"></a>Problembehandlung bei ASP.NET Core-Projekten

Von [Rick Anderson](https://twitter.com/RickAndMSFT)

Die folgenden Links erhalten Sie Anleitungen zur Fehlerbehebung:

* [Problembehandlung bei ASP.NET Core in Azure App Service](xref:host-and-deploy/azure-apps/troubleshoot)
* [Problembehandlung bei ASP.NET Core in IIS](xref:host-and-deploy/iis/troubleshoot)
* [Referenz zu häufigen Fehlern bei Azure App Service und IIS mit ASP.NET Core](xref:host-and-deploy/azure-iis-errors-reference)
* [YouTube: Diagnostizieren von Problemen in ASP.NET Core-Anwendungen](https://www.youtube.com/watch?v=RYI0DHoIVaA)

<a name="sdk"></a>
## <a name="net-core-sdk-warnings"></a>.NET Core SDK-Warnungen

### <a name="both-the-32-bit-and-64-bit-versions-of-the-net-core-sdk-are-installed"></a>Sowohl die 32-Bit und 64-Bit-Versionen von .NET Core SDK installiert sind
In der **neues Projekt** Dialogfeld für ASP.NET Core, können Sie die folgende Warnung angezeigt: 

    Both 32 and 64 bit versions of the .NET Core SDK are installed. Only templates from the 64 bit version(s) installed at C:\Program Files\dotnet\sdk\" will be displayed.

![Screenshot des Dialogfelds OneASP.NET mit der Warnung](troubleshoot/_static/both32and64bit.png)

Diese Warnung wird angezeigt, wenn (x86) 32-Bit und 64-Bit (x 64)-Versionen der [.NET Core SDK](https://www.microsoft.com/net/download/all) installiert sind. Häufige Gründe, die beide Versionen installiert werden können umfassen:

* Sie ursprünglich ein 32-Bit-Computer mit .NET Core SDK-Installationsprogramm heruntergeladen aber anschließend kopiert er über und ihn auf einem 64-Bit-Computer installiert. 
* Die 32-Bit-.NET Core SDK wurde durch eine andere Anwendung installiert.
* Die falsche Version wurde heruntergeladen und installiert.

Deinstallieren Sie die 32-Bit-.NET Core SDK, um diese Warnung zu vermeiden. Deinstallieren von **Systemsteuerung** > **Programme und Funktionen** > **deinstallieren oder Ändern eines Programms**. Wenn Sie verstehen, warum die Warnung wird ausgegeben, und deren Auswirkungen, können Sie die Warnung ignorieren.

### <a name="the-net-core-sdk-is-installed-in-multiple-locations"></a>.NET Core SDK ist in mehreren Speicherorten installiert.
In der **neues Projekt** Dialogfeld für ASP.NET Core möglicherweise die folgende Warnung angezeigt: 

 .NET Core SDK ist in mehreren Speicherorten installiert. Nur Vorlagen für die SDKs an installierten "c:\Programme\Microsoft Files\dotnet\sdk\' wird angezeigt.

![Screenshot des Dialogfelds OneASP.NET mit der Warnung](troubleshoot/_static/multiplelocations.png)

Diese Meldung wird angezeigt, wenn Sie mindestens eine Installation von .NET Core SDK in einem Verzeichnis außerhalb von haben * c:\Programme\Microsoft Files\dotnet\sdk\*. Dies geschieht in der Regel, die bei .NET Core SDK auf einem Computer mit Kopieren/Einfügen anstelle der MSI-Installationsprogramm bereitgestellt wurde.

Deinstallieren Sie die 32-Bit-.NET Core SDK, um diese Warnung zu vermeiden. Deinstallieren von **Systemsteuerung** > **Programme und Funktionen** > **deinstallieren oder Ändern eines Programms**. Wenn Sie verstehen, warum die Warnung wird ausgegeben, und deren Auswirkungen, können Sie die Warnung ignorieren.

### <a name="no-net-core-sdks-were-detected"></a>Es wurden keine .NET Core-SDKs erkannt.
In der **neues Projekt** Dialogfeld für ASP.NET Core möglicherweise die folgende Warnung angezeigt: 

**Keine .NET Core-SDKs erkannt wurden, stellen Sie sicher, dass sie in der Umgebungsvariablen "PATH" enthalten sind**

![Screenshot des Dialogfelds OneASP.NET mit der Warnung](troubleshoot/_static/NoNetCore.png)

Diese Warnung wird angezeigt, wenn die Umgebungsvariable `PATH` zeigt nicht auf eine beliebige .NET Core-SDKs auf dem Computer. Um dieses Problem zu lösen:

* Installieren, oder stellen Sie sicher, dass die .NET Core SDK installiert ist.
* Überprüfen Sie die `PATH` -Umgebungsvariable zeigt auf den Speicherort, die das SDK installiert ist. Normalerweise legt der Installer die `PATH`.