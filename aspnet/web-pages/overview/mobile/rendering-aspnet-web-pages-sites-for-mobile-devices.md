---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Rendern von ASP.NET Web Pages-(Razor)-Websites für Mobilgeräte | Microsoft-Dokumentation
author: tfitzmac
description: 'Dieser Artikel beschreibt, wie Sie Seiten in einer ASP.NET Web Pages (Razor)-Website zu erstellen, die auf mobilen Geräten ordnungsgemäß gerendert wird. Sie lernen Folgendes: wie Sie...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: d5c94af644c0dbc918544fe5112545c348136587
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369417"
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Rendern von ASP.NET Web Pages (Razor)-Websites für Mobilgeräte
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt, wie Sie Seiten in einer ASP.NET Web Pages (Razor)-Website zu erstellen, die auf mobilen Geräten ordnungsgemäß gerendert wird.
> 
> Sie lernen Folgendes:
> 
> - So eine Benennungskonvention zu verwenden, um anzugeben, dass eine Seite, die speziell für mobile Geräte konzipiert ist.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.


ASP.NET Web Pages ermöglicht das Erstellen benutzerdefinierter angezeigt, die zum Rendern von Inhalt auf Mobilgeräten und anderen Geräten.

Die einfachste Möglichkeit zum Erstellen von gerätespezifischen-Seite in einer ASP.NET Web Pages-Website wird mithilfe eines Musters Dateibenennung wie folgt: <em>FileName.</em> <em>Mobile</em><em>.cshtml</em>. Sie können zwei Versionen einer Seite erstellen (z. B. einen benannten <em>MyFile.cshtml</em> und eine mit dem Namen <em>MyFile.Mobile.cshtml</em>). Zur Laufzeit, wenn ein mobiles Gerät fordert <em>MyFile.cshtml</em>, rendert den Inhalt von ASP.NET <em>MyFile.Mobile.cshtml</em>. Andernfalls <em>MyFile.cshtml</em> gerendert wird.

Das folgende Beispiel zeigt, wie mobile renderingaufträge durch Hinzufügen einer Inhaltsseite für mobile Geräte. *Page1.cshtml* enthält Inhalte sowie eine navigationsrandleiste. *Page1.Mobile.cshtml* den gleichen Inhalt enthält, lässt aber die Randleiste.

1. Erstellen Sie in einer ASP.NET Web Pages-Website, eine Datei namens *Page1.cshtml* , und Ersetzen Sie den aktuellen Inhalt durch Folgendes Markup.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Erstellen Sie eine Datei mit dem Namen *Page1.Mobile.cshtml* , und Ersetzen Sie den vorhandenen Inhalt durch Folgendes Markup. Beachten Sie, dass es sich bei die mobile Version der Seite der Navigationsbereich für eine bessere Rendern auf einer kleineren Bildschirm lässt.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Führen Sie einen desktop-Browser und navigieren Sie zu *Page1.cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Führen Sie einen mobilen Browser (oder einen Emulator für mobile Geräte), und navigieren Sie zu *Page1.cshtml*. (Beachten Sie, die Sie nicht einschließen, *.mobile.* als Teil der URL.) Obwohl die Anforderung ist *Page1.cshtml*, rendert ASP.NET *Page1.Mobile.cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Um mobiler Seiten zu testen, können Sie einen Simulator für mobile Geräte, der auf einem Desktopcomputer ausgeführt wird. Mit diesem Tool können Sie Webseiten zu testen, wie sie auf mobilen Geräten aussehen würde (d. h. in der Regel eine viel kleinere anzeigen mit Bereich). Ein Beispiel für einen Simulator ist die [Benutzer-Agent Switcher-Add-On](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) für Mozilla Firefox, sodass Sie verschiedenen mobilen Browsern aus einer desktop-Version von Firefox zu emulieren.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Windows Phone-Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
