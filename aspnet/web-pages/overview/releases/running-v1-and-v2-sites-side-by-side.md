---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Unterschiedliche Versionen von ASP.NET Web Pages (Razor) parallel ausführen | Microsoft-Dokumentation
author: tfitzmac
description: In diesem Artikel wird erläutert, wie Sie Websites mit ASP.NET Web Pages (Razor) auf dem gleichen Computer oder Server ausführen, wenn die Websites konfiguriert werden, um verschiedene Versionen verwenden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: aa2bf41054540cc67b7c0b4669e356e732fed8d1
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402456"
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>Verschiedene Versionen von ASP.NET Web Pages (Razor) ausführen parallel
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel wird erläutert, wie Websites mit ASP.NET Web Pages (Razor) auf dem gleichen Computer oder Server ausgeführt wird, wenn die Websites konfiguriert werden, um verschiedene Versionen von ASP.NET Web Pages verwenden.
> 
> Sie lernen Folgendes:
> 
> - Was ist das Standardverhalten in ASP.NET aus, wenn Sie mit ASP.NET Web Pages erstellten Websites verfügen.
> - Vorgehensweise: konfigurieren ein neues Standorts mit einer älteren Version von ASP.NET Web Pages ausführen.
>   
> 
> Dies ist die ASP.NET-Funktion, die in diesem Artikel eingeführt:
> 
> - Die `webPages:Version` Konfigurationseinstellung.
>   
> 
> ## <a name="software-versions"></a>Software-Versionen
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2 und ASP.NET Web Pages-1.0.


ASP.NET Web Pages unterstützt die Möglichkeit, Websites parallel auszuführen. Dadurch können Sie weiterhin Ihre älteren ASP.NET Web Pages-Anwendungen ausführen, neue ASP.NET Web Pages-Anwendungen erstellen und alle auf demselben Computer ausgeführt wird.

Hier sind einige Punkte zu beachten, wenn Sie die Web Pages mit WebMatrix installieren:

- Standardmäßig werden der vorhandene Web Pages-Anwendungen als letzte Version auf Ihrem Computer ausgeführt. (Die Assemblys im globalen Assemblycache (GAC) installiert und werden automatisch verwendet.)
- Wenn Sie einen Standort mithilfe einer anderen Version von ASP.NET Web Pages ausführen möchten, können Sie den Standort zu diesem Zweck konfigurieren. Wenn Ihre Website noch keinem *"Web.config"* Datei im Stammverzeichnis der Website, eine neue erstellen, und kopieren Sie das folgende XML, die vorhandene Inhalte überschrieben. Wenn die Website bereits enthält eine *"Web.config"* hinzufügen. ein `<appSettings>` Element wie den folgenden Ausdruck ein, um die `<configuration>` Abschnitt.

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  "– Wenn Sie eine Version in nicht angeben der *" Web.config "* -Datei, einen Standort als die aktuelle Version bereitgestellt wird. (Die Assemblys in kopiert die *Bin* Ordner auf der bereitgestellten Website.)
- Neue Anwendungen, die mithilfe der Websitevorlagen im Web Matrix schließen Sie den Assemblys der Version von Webseiten in der Website *Bin* Ordner.

Im Allgemeinen, Sie können immer steuern, welche Version von Webseiten mit Ihrer Website verwendet werden soll, mithilfe von NuGet die entsprechenden Assemblys in der Website installieren *Bin* Ordner. Um Pakete zu finden, besuchen Sie [NuGet.org](http://NuGet.org).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Die wichtigsten Funktionen in ASP.NET Web Pages 2](top-features-in-web-pages-2.md)
