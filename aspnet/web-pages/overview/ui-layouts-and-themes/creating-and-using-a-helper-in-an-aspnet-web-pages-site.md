---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Erstellen und Verwenden eines Hilfsprogramms in einer ASP.NET Web Pages (Razor) Standort | Microsoft-Dokumentation
author: Rick-Anderson
description: Dieser Artikel beschreibt, wie Sie eine Hilfsprogramm auf einer Website für ASP.NET Web Pages (Razor) zu erstellen. Ein Hilfsprogramm ist eine wiederverwendbare Komponente, die Code und Markup Perf enthält...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 3b54ba8a35a354eb0562a47866cd8b5dcb66bc86
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021507"
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Erstellen und Verwenden eines Hilfsprogramms in einer ASP.NET Web Pages (Razor)-Website
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt, wie Sie eine Hilfsprogramm auf einer Website für ASP.NET Web Pages (Razor) zu erstellen. Ein *Helper* ist eine wiederverwendbare Komponente, die enthält Code und Markup zum Ausführen einer Aufgabe, die mühsam oder komplex sein können.
> 
> **Sie lernen Folgendes:** 
> 
> - Informationen zum Erstellen und verwenden Sie eine einfache Hilfsprogramm.
> 
> Dies sind die Funktionen von ASP.NET in diesem Artikel:
> 
> - Die `@helper` Syntax.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> In diesem Tutorial funktioniert auch mit ASP.NET Web Pages 2.


## <a name="overview-of-helpers"></a>Übersicht über die Hilfsprogramme

Wenn Sie die gleichen Aufgaben auf verschiedenen Seiten auf Ihrer Website ausführen möchten, können Sie eine Hilfsprogramm verwenden. ASP.NET Web Pages umfasst eine Reihe von Hilfsprogrammen, und es gibt jedoch zahlreiche weitere, die Sie herunterladen und installieren können. (Eine Liste der integrierten Hilfsprogramme in ASP.NET Web Pages finden Sie der [ASP.NET-API-Kurzübersicht](https://go.microsoft.com/fwlink/?LinkId=202907).) Wenn keine der vorhandenen Hilfsprogramme Ihre Anforderungen erfüllt, können Sie Ihre eigenen Hilfsprogramm erstellen.

Eine Hilfsprogramm können Sie einen allgemeinen Block von Code über mehrere Seiten zu verwenden. Nehmen wir an, dass auf Ihrer Seite häufig ein Hinweis-Element erstellen möchten, die abgesehen von der normale Absätze festgelegt ist. Vielleicht wird als Hinweis erstellt eine `<div>` Element, das formatiert wurde, wird als Feld mit einem Rahmen. Anstatt diese dasselbe Markup zu einer Seite hinzufügen, jedes Mal, wenn Sie sich anzeigen möchten, können Sie das Markup als Hilfsprogramm packen. Sie können anschließend den Hinweis mit einer einzelnen Codezeile einfügen an einer beliebigen Stelle Sie ihn benötigen.

Verwenden eines Hilfsprogramms wie folgt, wird der Code in den einzelnen Seiten einfacher und leichter zu lesen. Es erleichtert auch Wartung Ihres Standorts, da Sie bei Bedarf ändern, wie die Anmerkungen zu dieser zu suchen, das Markup an einem Ort ändern können.

## <a name="creating-a-helper"></a>Erstellen eines Hilfsprogramms

Dieses Verfahren zeigt, wie Sie das Hilfsprogramm zu erstellen, das die Beachten Sie, wie gerade beschrieben erstellt. Dies ist ein einfaches Beispiel, aber die benutzerdefinierten Hilfsmethoden zählen alle Markups und ASP-Code, die Sie benötigen.

1. Erstellen Sie im Stammordner der Website, einen Ordner namens *App\_Code*. Dies ist ein reservierter Ordnername in ASP.NET können Code für Komponenten wie Hilfsprogramme ordnen Sie.
2. In der *App\_Code* Ordner erstellen Sie ein neues *.cshtml* Datei und nennen Sie sie *MyHelpers.cshtml*.
3. Ersetzen Sie den vorhandenen Inhalt durch Folgendes:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Der Code verwendet die `@helper` Syntax zum Deklarieren Sie einer neuen Hilfsprogramms, mit dem Namen `MakeNote`. Diese bestimmte-Hilfsobjekt ermöglicht Ihnen das Übergeben eines Parameters namens `content` , die eine Kombination von Text und Markup enthalten kann. Das Hilfsprogramm fügt die Zeichenfolge in die Hinweis-Text mithilfe der `@content` Variable.

    Beachten Sie, dass die Datei heißt *MyHelpers.cshtml*, aber das Hilfsprogramm wird mit dem Namen `MakeNote`. Sie können mehrere benutzerdefinierte Hilfen in einer einzelnen Datei einfügen.
4. Speichern und schließen Sie die Datei.

## <a name="using-the-helper-in-a-page"></a>Unter Verwendung des Hilfsprogramms auf einer Seite

1. In den Stammordner, erstellen Sie eine neue leere Datei namens *TestHelper.cshtml*.
2. Fügen Sie folgenden Code in die Datei ein:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Verwenden Sie zum Aufrufen der Hilfsmethode Erstellung `@` gefolgt von den Dateinamen, in dem das Hilfsprogramm ist, einen Punkt, und klicken Sie dann den Hilfsprogramm-Namen. (Wenn Sie mehrere Ordner, in haben der *App\_Code* Ordner können Sie die Syntax `@FolderName.FileName.HelperName` aufrufen, die Hilfsmethode in einer beliebigen geschachtelte Ordner der obersten Ebene). Der Text, den Sie in Anführungszeichen innerhalb der Klammern hinzufügen, ist der Text, den das Hilfsprogramm als Teil der Notiz auf der Webseite angezeigt wird.
3. Speichern Sie die Seite, und führen Sie sie in einem Browser. Das Hilfsprogramm generiert das Hinweis Element direkt, wenn Sie das Hilfsprogramm aufgerufen: zwischen den beiden Absätze tauschen.

    ![Screenshot die Seite im Browser, und wie das Hilfsprogramm Markup generiert, die einen Rahmen um den angegebenen Text wird angezeigt.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Horizontales Menü als ein Razor-Hilfsprogramm](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). In diesem Blogeintrag von Mike Pope veranschaulicht, wie ein horizontales Menü als ein Hilfsprogramm über Markup, CSS und Code zu erstellen.

[Nutzen HTML5 in der ASP.NET Web Helpers für WebMatrix und ASP.NET MVC3 Seiten](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). In diesem Blogeintrag von Sam Abraham zeigt eine Hilfsprogramm, das ein HTML5 rendert `Canvas` Element.

[Der Unterschied zwischen @Helpers und @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). In diesem Blogeintrag von Mike Brind beschreibt `@helper` Syntax und `@function` Syntax und wann Sie jeweils zu verwenden.
