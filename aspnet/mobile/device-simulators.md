---
uid: mobile/device-simulators
title: Simulieren von beliebten Mobilgeräten zu Testzwecken | Microsoft-Dokumentation
author: rick-anderson
description: Sie können die Emulatoren für verbreitete mobile Geräte und Browser herunterladen, über die folgenden links
ms.author: riande
ms.date: 10/11/2018
ms.assetid: bfb5612e-c3ec-4f28-b43b-63d781aa2272
msc.legacyurl: /mobile/device-simulators
msc.type: content
ms.openlocfilehash: 8299295154d23f8fc430676b2c8ad8efc98ad185
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348376"
---
# <a name="simulate-popular-mobile-devices-for-testing"></a>Simulieren von beliebten Mobilgeräten zu Testzwecken

> Sie können die Emulatoren für verbreitete mobile Geräte und Browser herunterladen, über die folgenden Links.

| Gerät bzw. Ihren Browser | Emulator / Simulator |
| --- | --- |
| BrowserStack gehostet, Browser-Virtualisierung ![BrowserStack gehostet, Browser-Virtualisierung](device-simulators/_static/image1.png) | [Browser-gehostete-Virtualisierung BrowserStack](http://browserstack.com) Ihrer lokalen oder Produktions-Umgebung in jedem Browser auf jeder Plattform zu testen. Sie können einen Tunnel zwischen dem Computer und dem BrowserStack-Netzwerk in Ihrem eigenen gehosteten virtuellen Computer erstellen. Stellen Sie sicher, dass zum Abrufen der [BrowserStack Visual Studio-Erweiterung](https://marketplace.visualstudio.com/items?itemName=browserstackcom.BrowserStack) für eine noch nahtlosere. |
| iPhone / iPod / iPad | [Elektrischen Mobile Studio](http://www.electricplum.com/studio.aspx) iPhone und iPad-Simulatoren für Windows als auch eine dynamisch Tool zu entwerfen. |
| Android | [Android Studio](https://developer.android.com/studio/) oder [Visual Studio-Emulator für Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/) |
| Opera Mobile | [Klassische Opera Mobile-Emulator](https://www.opera.com/developer/mobile-emulator) |
| Windows Mobile 6.5.3 | [Windows Mobile 6.5.3 Developer Tool Kit](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=c0213f68-2e01-4e5c-a8b2-35e081dcf1ca&amp;displaylang=en) Beachten Sie, dass um den Zugriff auf das Telefon Netzwerk gewähren zu können, Sie auch das VPC-Netzwerkadapter, der benötigen in enthalten [Virtual PC 2007](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=04d26402-3199-48a3-afa2-2dc0b40a73b6&amp;DisplayLang=en). Die Verbindung Internet Explorer auf dem Mobiltelefon zu Ihrem Visual Studio Development Server finden Sie unter [Blogbeitrag des Kiran Patil](http://kiranpatils.wordpress.com/2009/11/19/access-internetlocal-website-from-your-windows-mobile-device-emulators/). |

> [!NOTE]
> Wenn Ihre Anwendung auf einem echten mobilen Gerät anzuzeigen (Was ist die einzige Option für das iPhone oder iPad, vollständig testen, da kein "true" Emulator für Windows ist) werden sollen, müssen Sie Ihre Anwendung in IIS oder IIS Express hosten. Sie können nicht einfach Visual Studio integrierten Webserver dazu verwenden, weil es von anderen Computern auf Anforderungen reagiert wird nicht.