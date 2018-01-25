---
uid: mobile/device-simulators
title: "Verbreitete Mobile Geräte zu Testzwecken simulieren | Microsoft Docs"
author: rick-anderson
description: "Sie können die Emulatoren für verbreitete mobile Geräte und Browser herunterladen, anhand dieser links"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/28/2011
ms.topic: article
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: a8293a5bff9ed73f177be2d9928d8d686c4f311d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="simulate-popular-mobile-devices-for-testing"></a>Verbreitete Mobile Geräte zu Testzwecken simulieren
====================
> Sie können die Emulatoren für verbreitete mobile Geräte und Browser herunterladen, anhand dieser links


| Gerät bzw. Ihren Browser | Emulator / Simulator |
| --- | --- |
| BrowserStack gehostet Browser Virtualisierung ![BrowserStack gehostet Browser Virtualisierung](device-simulators/_static/image1.png) | [Browser-gehostete-Virtualisierung BrowserStack](http://browserstack.com) Testen Ihrer lokalen oder Produktion-Umgebung in jedem Browser auf einer beliebigen Plattform. Sie können einen Tunnel zwischen Ihrem Computer und das Netzwerk BrowserStack in Ihren eigenen gehosteten virtuellen Computer erstellen. Stellen Sie sicher, zum Abrufen der [BrowserStack Visual Studio-Erweiterung](https://visualstudiogallery.msdn.microsoft.com/2dfa32b1-3c47-439d-b1c5-9e28be18b81c) ein sogar noch stärker nahtlosen zu erzielen. |
| Windows Phone | [Windows Phone SDK-Downloads](https://dev.windowsphone.com/downloadsdk) der Windows Phone Software Development Kit (SDK) enthält alle Tools, die Sie benötigen für die Entwicklung von apps und Spiele für Windows Phone |
| iPhone / iPod / iPad | [Electric Plum](http://www.electricplum.com/studio.aspx) iPhone und iPad-Simulatoren für Windows als auch eine reagierend entwerfen Tool. In Visual Studio 2012 "Durchsuchen mit..."-Option können integriert werden. |
| Android | [Android SDK-homepage](https://developer.android.com/sdk) |
| Opera Mobile / Opera Mini | Neueste Versionen: [Opera-Entwicklertools home](http://www.opera.com/developer/tools/) Opera Mini-4.2: [Online Java-basierte Simulator](http://www.opera.com/mobile/demo/?ver=4) |
| Windows Mobile 6.5.3 | [Windows Mobile 6.5.3 Developer Toolkit](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) Beachten Sie, dass um den Netzwerkzugriff Phone gewähren zu können, Sie auch den virtuellen Computer-Netzwerkadapter, der benötigen in enthalten [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). Verbindung der Internet Explorer auf dem Mobiltelefon zu Ihrem Visual Studio Development Server, finden Sie unter [Blogbeitrag des Kiran Patil](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |
| Windows Mobile 6.1 | [Emulatorabbilder für Visual Studio 2005/2008](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=3d6f581e-c093-4b15-ab0c-a2ce5bffdb47) |

Beachten Sie, wenn Sie die Anwendung auf einem echten mobilen Gerät anzuzeigen (das ist die einzige Option für das iPhone oder iPad, vollständig testen, da es kein "true" Emulator für Windows ist) möchten, müssen Sie zum Hosten der Anwendung in IIS oder IIS Express. Sie können nicht Visual Studio integrierten Webserver problemlos dazu verwenden, weil er von anderen Computern auf Anforderungen reagiert wird nicht.
