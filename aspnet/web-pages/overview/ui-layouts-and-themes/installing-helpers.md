---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Installieren eine Hilfsmethode in einer ASP.NET Web-Seiten (Razor) Standort | Microsoft Docs
author: tfitzmac
description: "Dieser Artikel beschreibt, wie eine Hilfsprogramm auf einer Website für ASP.NET Web Pages (Razor) installieren. Ein Hilfsprogramm ist eine wiederverwendbare Komponente sein, die Code und Markup zum pro umfasst..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2014
ms.topic: article
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 842c5a56d14314217c1e6ad6d48ded28d3cc5b4e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Installieren eine Hilfsprogramm an einem Standort der ASP.NET Web Pages (Razor)
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt, wie eine Hilfsprogramm auf einer Website für ASP.NET Web Pages (Razor) installieren. Ein *Helper* ist eine wiederverwendbare Komponente, die Code und Markup zum Ausführen einer Aufgabe, die möglicherweise als zu aufwendig oder komplexe enthält.
> 
> Lernen Sie:
> 
> - So installieren Sie eine Hilfsprogramm in einer Website mit WebMatrix 3 erstellt.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - WebMatrix-3


## <a name="overview-of-helpers"></a>Übersicht über die Hilfsprogramme

Einige Aufgaben, die Benutzer häufig auf Webseiten viel Code erfordern, oder zusätzliche Kenntnisse erfordern. Beispiele für ein Diagramm für Daten anzeigen; Platzieren eine Twitter-Schaltfläche "Follow" auf einer Seite; sendende e-Mail aus Ihrer Website; Zuschneiden oder Ändern der Bildgröße; Verwenden von PayPal für Ihre Website. Um diese Arten von Aufgaben zu erleichtern, können Sie mithilfe von ASP.NET Web Pages *Hilfsprogramme*. Hilfsprogramme sind Komponenten, für deren Installation für einen Standort und, mit denen Sie typische Aufgaben mit nur einer Zeile oder zwei der Razor-Code.

ASP.NET Web Pages verfügt über einige integrierte Hilfsprogramme. Viele Hilfsprogramme stehen jedoch in Paketen (-add-ins), die mit der NuGet-Paket-Manager bereitgestellt werden. NuGet ermöglicht die Auswahl ein Pakets installiert, und klicken Sie dann diese übernimmt alle Details der Installation.

## <a name="installing-a-helper-in-webmatrix-3"></a>Eine Hilfsprogramm installieren in WebMatrix 3

1. Klicken Sie in WebMatrix-3, auf die **NuGet** Schaltfläche.

    ![Im Dialogfeld NuGet Gallery WebMatrix](installing-helpers/_static/image1.png)
2. Dies startet den NuGet-Paket-Manager und die verfügbaren Pakete angezeigt. Geben Sie in das Suchfeld ein Schlüsselwort für das Hilfsprogramm, die, das Sie installieren möchten.

    ![Im Dialogfeld NuGet Gallery WebMatrix](installing-helpers/_static/image2.png)
- Wählen Sie das Paket, und klicken Sie dann auf **installieren**. Klicken Sie auf **Ja** Wenn gefragt, ob Sie verwenden möchten, installieren Sie das Paket und anzugeben, dass Sie ihnen zustimmen.

    Wenn dies der erste Mal Sie eine Hilfsprogramm installiert haben ist, erstellt NuGet Ordner in Ihre Website für den Code, der das Hilfsprogramm bildet.
- Um ein Hilfsprogramm deinstallieren möchten, klicken Sie auf die **Katalog** Schaltfläche, klicken Sie auf die **installiert** Registerkarte, und wählen Sie das Paket deinstalliert werden soll.

## <a name="installing-the-twitter-helper"></a>Installieren das Twitter-Hilfsprogramm

Die neueste Version von der Twitter-API ist nicht kompatibel mit der Twitter-Hilfsprogramm, das Sie über NuGet installieren. Lesen Sie stattdessen die [Twitter-Hilfsprogramm mit WebMatrix](twitter-helper.md) Thema Informationen zum Einrichten der Twitter-Hilfsprogramm in Ihrem Projekt.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Einführung in ASP.NET Web Pages 2 - Grundlagen der Programmierung](../getting-started/introducing-razor-syntax-c.md)

[Twitter-Hilfsprogramm mit WebMatrix](twitter-helper.md)
