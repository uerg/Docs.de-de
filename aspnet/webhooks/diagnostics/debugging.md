---
uid: webhooks/diagnostics/debugging
title: Debuggen von ASP.NET-WebHooks | Microsoft-Dokumentation
author: rick-anderson
description: Wie Sie ASP.NET WebHooks zu debuggen.
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 467da78b-3c35-4c51-8b08-77a32379e4a8
ms.technology: ''
ms.openlocfilehash: 5c567fb51db008526f59e09fce5bb60b66f6e479
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389058"
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
