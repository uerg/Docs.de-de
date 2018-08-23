---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Installieren eines Hilfsprogramms in einer ASP.NET Web Pages (Razor) Standort | Microsoft-Dokumentation
author: tfitzmac
description: Dieser Artikel beschreibt, wie Sie eine Hilfsprogramm auf einer Website für ASP.NET Web Pages (Razor) zu installieren. Ein Hilfsprogramm ist eine wiederverwendbare Komponente, die Code und Markup pro enthält...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 8629d91e1e297244228898e28f70616c7ccf1acf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836486"
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Installieren eines Hilfsprogramms in einer ASP.NET Web Pages (Razor)-Website
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt, wie Sie eine Hilfsprogramm auf einer Website für ASP.NET Web Pages (Razor) zu installieren. Ein *Helper* ist eine wiederverwendbare Komponente, die enthält Code und Markup zum Ausführen einer Aufgabe, die mühsam oder komplex sein können.
> 
> Sie lernen Folgendes:
> 
> - So installieren Sie eine Hilfsprogramm auf einer Website mit WebMatrix 3 erstellt.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - WebMatrix 3


## <a name="overview-of-helpers"></a>Übersicht über die Hilfsprogramme

Einige Aufgaben, die Benutzer häufig auf Webseiten ausführen möchten, viel Code erfordern oder zusätzliche Kenntnisse erfordern. Beispiele sind ein Diagramm für die Daten angezeigt: Platzieren eine Twitter-Schaltfläche "Folgen" auf einer Seite; Senden einer e-Mail aus Ihrer Website; Abschneiden oder zum Skalieren von Bildern; Verwenden von PayPal für Ihre Website. Um diese Arten von Elementen führen zu erleichtern, können Sie verwenden ASP.NET Web Pages *Hilfsprogramme*. Hilfsmethoden sind Komponenten, die Sie für einen Standort und, mit denen Installation typische Aufgaben mit nur einer oder zwei Zeilen mit Razor-Code.

ASP.NET Web Pages verfügt über einige Hilfsprogramme integriert. Allerdings sind viele Hilfsprogramme in Paketen (-add-ins), die bereitgestellt werden, mit dem NuGet-Paket-Manager verfügbar. NuGet können Sie ein Paket zum Installieren auswählen und dann übernimmt es alle Details der Installation.

## <a name="installing-a-helper-in-webmatrix-3"></a>Installieren eines Hilfsprogramms in WebMatrix 3

1. Klicken Sie in WebMatrix 3, auf die **NuGet** Schaltfläche.

    ![Dialogfeld für NuGet-Katalog in WebMatrix](installing-helpers/_static/image1.png)
2. Dies startet den NuGet-Paket-Manager und die verfügbaren Pakete angezeigt. Geben Sie in das Suchfeld ein Schlüsselwort für das Hilfsprogramm, die, das Sie installieren möchten.

    ![Dialogfeld für NuGet-Katalog in WebMatrix](installing-helpers/_static/image2.png)
3. Wählen Sie das Paket, und klicken Sie dann auf **installieren**. Klicken Sie auf **Ja** Wenn gefragt, ob Sie öffnen möchten, installieren Sie das Paket und anzugeben, dass Sie die Lizenzbedingungen akzeptieren.

     Wenn dies der erste Mal Sie eine Hilfsprogramm installiert haben ist, erstellt NuGet Ordner auf Ihrer Website für den Code, der das Hilfsprogramm bildet.
4. Um ein Hilfsprogramm zu deinstallieren, klicken Sie auf die **Katalog** , zeigen Sie auf die **installiert** Registerkarte, und wählen Sie das Paket deinstalliert werden soll.

## <a name="installing-the-twitter-helper"></a>Installieren die Twitter-Hilfsprogramm

Die neueste Version von der Twitter-API ist nicht kompatibel ist, mit der Twitter-Hilfsprogramm, die, das Sie über NuGet installieren. Lesen Sie stattdessen die [Twitter-Hilfsprogramm mit WebMatrix](twitter-helper.md) Thema enthält Informationen zum Einrichten der Twitter-Hilfsprogramm in Ihrem Projekt.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Einführung in ASP.NET Web Pages 2 – Grundlagen der Programmierung](../getting-started/introducing-razor-syntax-c.md)

[Twitter-Hilfsprogramm mit WebMatrix](twitter-helper.md)
