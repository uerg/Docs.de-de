---
uid: mobile/device-simulators
title: Simulieren von beliebten Mobilgeräten zu Testzwecken | Microsoft-Dokumentation
author: rick-anderson
description: Sie können die Emulatoren für verbreitete mobile Geräte und Browser herunterladen, über die folgenden links
ms.author: aspnetcontent
ms.date: 01/28/2011
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 092186a48a304c1b9e3e140e74f65dbe5a035e66
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37833684"
---
<a name="simulate-popular-mobile-devices-for-testing"></a>Simulieren von beliebten Mobilgeräten zu Testzwecken
====================
> Sie können die Emulatoren für verbreitete mobile Geräte und Browser herunterladen, über die folgenden links


| Gerät bzw. Ihren Browser | Emulator / Simulator |
| --- | --- |
| BrowserStack gehostet, Browser-Virtualisierung ![BrowserStack gehostet, Browser-Virtualisierung](device-simulators/_static/image1.png) | [Browser-gehostete-Virtualisierung BrowserStack](http://browserstack.com) Ihrer lokalen oder Produktions-Umgebung in jedem Browser auf jeder Plattform zu testen. Sie können einen Tunnel zwischen dem Computer und dem BrowserStack-Netzwerk in Ihrem eigenen gehosteten virtuellen Computer erstellen. Stellen Sie sicher, dass zum Abrufen der [BrowserStack Visual Studio-Erweiterung](https://visualstudiogallery.msdn.microsoft.com/2dfa32b1-3c47-439d-b1c5-9e28be18b81c) für eine noch nahtlosere. |
| Windows Phone | [Windows Phone SDK-Downloads](https://dev.windowsphone.com/downloadsdk) der Windows Phone Software Development Kit (SDK) enthält alle Tools, die Sie zum Entwickeln von apps und Spiele für benötigen Windows Phone |
| iPhone / iPod / iPad | [Electric Plum](http://www.electricplum.com/studio.aspx) iPhone und iPad-Simulatoren für Windows als auch eine dynamisch Tool zu entwerfen. Visual Studio 2012 "Mit durchsuchen..."-Option können integriert werden. |
| Android | [Android SDK-homepage](https://developer.android.com/sdk) |
| Opera Mobile / Opera Mini | Neueste Versionen: [Opera-Entwicklertools home](http://www.opera.com/developer/tools/) Opera Mini-4.2: [Online Java-basierten Simulator](http://www.opera.com/mobile/demo/?ver=4) |
| Windows Mobile 6.5.3 | [Windows Mobile 6.5.3 Developer Tool Kit](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) Beachten Sie, dass um den Zugriff auf das Telefon Netzwerk gewähren zu können, Sie auch das VPC-Netzwerkadapter, der benötigen in enthalten [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). Die Verbindung Internet Explorer auf dem Mobiltelefon zu Ihrem Visual Studio Development Server finden Sie unter [Blogbeitrag des Kiran Patil](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |
| Windows Mobile 6.1 | [Emulator-Images für Visual Studio 2005/2008](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=3d6f581e-c093-4b15-ab0c-a2ce5bffdb47) |

Beachten Sie, dass wenn Sie Ihre Anwendung auf einem echten mobilen Gerät anzeigen (Was ist die einzige Option für das iPhone oder iPad, vollständig testen, da kein "true" Emulator für Windows ist) möchten, Sie zum Hosten der Anwendung in IIS oder IIS Express müssen. Sie können nicht einfach Visual Studio integrierten Webserver dazu verwenden, weil es von anderen Computern auf Anforderungen reagiert wird nicht.
