---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: "Rendern von ASP.NET Web-Seiten (Razor)-Websites für Mobile Geräte | Microsoft Docs"
author: tfitzmac
description: "Dieser Artikel beschreibt, wie Seiten in einer ASP.NET Web Pages (Razor) Standort zu erstellen, die auf mobilen Geräten ordnungsgemäß gerendert werden. Sie lernen: Vorgehensweise Sie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: 899bbdef82d689be81cd77ea6805e0484fb614aa
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Rendern von ASP.NET Web Pages (Razor)-Websites für Mobile Geräte
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt, wie Seiten in einer ASP.NET Web Pages (Razor) Standort zu erstellen, die auf mobilen Geräten ordnungsgemäß gerendert werden.
> 
> Lernen Sie:
> 
> - Wie Sie eine Benennungskonvention zu verwenden, um anzugeben, dass eine Seite, die speziell für mobile Geräte entwickelt wurde.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2.


ASP.NET Web Pages können Sie benutzerdefinierte zeigt zum Rendern von Inhalt auf Mobile oder anderen Geräten zu erstellen.

Die einfachste Möglichkeit zum Erstellen von gerätespezifischen-Seite in einer ASP.NET Web Pages-Website wird mit einem Dateibenennung Muster wie folgt: *FileName. **Mobile**cshtml*. Sie können zwei Versionen einer Seite erstellen (z. B. eines Namens *MyFile.cshtml* und dasjenige mit dem Namen *MyFile.Mobile.cshtml*). Zur Laufzeit, wenn ein mobiles Gerät anfordert *MyFile.cshtml*, ASP.NET rendert den Inhalt von *MyFile.Mobile.cshtml*. Andernfalls *MyFile.cshtml* gerendert wird.

Das folgende Beispiel zeigt, wie mobile Rendering durch Hinzufügen einer Inhaltsseite für mobile Geräte aktivieren. *Page1.cshtml* enthält Inhalte sowie eine navigationsrandleiste. *Page1.Mobile.cshtml* den gleichen Inhalt enthält, lässt aber die Randleiste.

1. Erstellen Sie in einer ASP.NET Web Pages-Website, eine Datei namens *Page1.cshtml* , und Ersetzen Sie den aktuellen Inhalt durch Folgendes Markup.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Erstellen Sie eine Datei mit dem Namen *Page1.Mobile.cshtml* , und Ersetzen Sie den vorhandenen Inhalt durch Folgendes Markup. Beachten Sie, dass die mobile Version der Seite im Abschnitt Navigation besser Rendern auf einen kleineren Bildschirm wird ausgelassen.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Führen Sie einen Desktopbrowser, und navigieren Sie zu *Page1.cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Führen Sie einen mobilen Browser (oder einen Emulator für mobile Geräte), und navigieren Sie zu *Page1.cshtml*. (Beachten Sie, die Sie kein *.mobile.* als Teil der URL). Obwohl die Anforderung ist *Page1.cshtml*, rendert ASP.NET *Page1.Mobile.cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Um mobiler Seiten zu testen, können Sie einen Mobilgerät-Simulator aus, der auf einem desktop-Computer ausgeführt wird. Mit diesem Tool können Sie die Webseiten zu testen, wie sie auf mobilen Geräten aussehen würde (d. h. in der Regel mit einer wesentlich kleineren anzeigen Bereich). Ein Beispiel für einen Simulator ist die [Benutzer-Agent Switcher Add-on](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) für Mozilla Firefox, dem können Sie die verschiedenen mobile Browsern aus einer desktop-Version von Firefox emulieren.


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Windows Phone-Emulator](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
