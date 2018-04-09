---
uid: web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
title: Unterschiedliche Versionen von ASP.NET Web Pages (Razor) parallel ausführen | Microsoft Docs
author: tfitzmac
description: In diesem Artikel wird erläutert, wie ASP.NET Web Pages (Razor)-Websites auf dem gleichen Computer oder Server ausgeführt wird, wenn die Websites konfiguriert sind, unterschiedliche Versionen verwenden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: a861409b-4ae6-4868-9e09-87edfac3535f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/running-v1-and-v2-sites-side-by-side
msc.type: authoredcontent
ms.openlocfilehash: 1729f3201013926b221afc92d23416b0081d8efb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="running-different-versions-of-aspnet-web-pages-razor-side-by-side"></a>Verschiedene Versionen von ASP.NET Web Pages (Razor) nebeneinander ausgeführt
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> In diesem Artikel erläutert das Ausführen von ASP.NET Web Pages (Razor) Websites auf dem gleichen Computer oder Server, wenn die Websites mit verschiedenen Versionen von ASP.NET Web Pages konfiguriert sind.
> 
> Lernen Sie:
> 
> - Was ist das Standardverhalten in ASP.NET aus, wenn Sie mit ASP.NET Web Pages erstellten Websites verfügen.
> - So konfigurieren Sie einen neuen Standort mit einer älteren Version von ASP.NET Web Pages ausgeführt werden.
>   
> 
> Dies ist die ASP.NET-Funktion im Artikel eingeführt:
> 
> - Die `webPages:Version` -Konfigurationseinstellung.
>   
> 
> ## <a name="software-versions"></a>Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2 und ASP.NET Web Pages-1.0.


ASP.NET Web Pages unterstützt die Fähigkeit zum Ausführen von Websites nebeneinander angezeigt werden. Dadurch können Sie weiterhin Ihre älteren ASP.NET Web Pages-Anwendungen ausführen, neue ASP.NET Web Pages-Anwendungen erstellen und alle von ihnen auf dem gleichen Computer ausführen.

Hier sind einige Punkte zu beachten, wenn Sie die Webseiten mit WebMatrix installieren:

- Standardmäßig werden die vorhandene Web Pages-Anwendungen als letzte Version auf Ihrem Computer ausgeführt. (Die Assemblys im globalen Assemblycache (GAC) installiert und werden automatisch verwendet.)
- Wenn Sie eine Website mit einer anderen Version von ASP.NET Web Pages ausführen möchten, können Sie den Standort zu diesem Zweck konfigurieren. Wenn Ihre Website noch keinem *"Web.config"* Datei im Stammverzeichnis der Site, ein neues erstellen und kopieren Sie die folgenden XML-Code hinein, Überschreiben der vorhandenen Inhalts. Wenn der Standort bereits enthält eine *"Web.config"* hinzufügen. ein `<appSettings>` Element wie den folgenden Ausdruck ein, um die `<configuration>` Abschnitt.

    [!code-xml[Main](running-v1-and-v2-sites-side-by-side/samples/sample1.xml)]
  "-Wenn Sie keine Version im Angeben der *" Web.config "* Datei, einen Standort als letzte Version bereitgestellt wird. (Die Assemblys werden in kopiert die *"bin"* Ordner auf der Website bereitgestellt.)
- Neue Anwendungen, die mithilfe der Websitevorlagen im Web Matrix gehören die Assemblys der Web Pages-Version am Standort *"bin"* Ordner.

Im Allgemeinen können Sie immer steuern, welche Version von Webseiten für Ihre Website verwendet werden, mithilfe von NuGet die entsprechenden Assemblys in der Website installieren *"bin"* Ordner. Um Pakete zu suchen, besuchen Sie [NuGet.org](http://NuGet.org).

## <a name="additional-resources"></a>Zusätzliche Ressourcen

[Die wichtigsten Funktionen in ASP.NET Web Pages 2](top-features-in-web-pages-2.md)
