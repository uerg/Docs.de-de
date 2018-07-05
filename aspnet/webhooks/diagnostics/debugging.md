---
uid: webhooks/diagnostics/debugging
title: Debuggen von ASP.NET-WebHooks | Microsoft-Dokumentation
author: rick-anderson
description: Wie Sie ASP.NET WebHooks zu debuggen.
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.openlocfilehash: 25fa47202dd31d6ebd7a0e179075333108515274
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37801987"
---
# <a name="aspnet-webhooks-debugging"></a>Debuggen von ASP.NET-WebHooks  

## <a name="debugging-in-azure"></a>Debuggen in Azure

Zum Debuggen der Webanwendung während der Ausführung in Azure finden Sie im Tutorial [Problembehandlung bei Web-Apps in Azure App Service mithilfe von Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/#webserverlogs).

## <a name="debugging-with-source-and-symbols"></a>Debuggen mit Symbolen und Quelle

Zusätzlich zu Ihren eigenen Code-Debuggen, ist es möglich, direkt in Microsoft ASP.NET WebHooks und in der Tat Debuggen aller .NET. Dies funktioniert, unabhängig davon, ob Sie lokal oder Remote zu debuggen. Konfigurieren Sie zunächst Visual Studio, um die Quelle und Symbole finden durch das Aufrufen **Debuggen** und dann **Optionen und Einstellungen**. Legen Sie die Optionen wie folgt:

![Optionen und Einstellungen](_static/SourceSymbols.png)

Fügen Sie dann auf einen Link zu [symbolsource.org](http://symbolsource.org) für das Herunterladen der Quelle und Symbole. Wechseln Sie zu der **Symbole** im oberen Menü auf der Registerkarte, und fügen Sie Folgendes als Symbol Speicherort:

```
http://srv.symbolsource.org/pdb/Public
```

Stellen Sie außerdem sicher, dass das Cacheverzeichnis einen kurzen Namen verfügt; Andernfalls können die Namen der zu lang werden die bewirkt, das die Symbole nicht geladen werden. Ein Beispielpfad ist:

```
C:\SymCache
```

Die Einstellungen sollten wie folgt aussehen:

![Beispiel für Optionen Symbol Speicherort](_static/SymSource.png)
