---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Erstellen und verwenden eine Hilfsmethode in einer ASP.NET Web Pages (Razor) Website | Microsoft Docs
author: tfitzmac
description: Dieser Artikel beschreibt, wie eine Hilfsprogramm in einer ASP.NET Web Pages (Razor)-Website zu erstellen. Ein Hilfsprogramm ist eine wiederverwendbare Komponente sein, die Code und Markup Perf umfasst...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 5d0c1ae09d8fbc91ff76cd4045d439abafee7736
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Erstellen und verwenden eine Hilfsmethode in einer ASP.NET Web Pages (Razor) Standort
====================
durch [Tom FitzMacken](https://github.com/tfitzmac)

> Dieser Artikel beschreibt, wie eine Hilfsprogramm in einer ASP.NET Web Pages (Razor)-Website zu erstellen. Ein *Helper* ist eine wiederverwendbare Komponente, die Code und Markup zum Ausführen einer Aufgabe, die möglicherweise als zu aufwendig oder komplexe enthält.
> 
> **Lernen Sie:** 
> 
> - Informationen zum Erstellen und eine einfache Hilfsfunktion verwenden.
> 
> Dies sind die Funktionen von ASP.NET im Artikel:
> 
> - Die `@helper` Syntax.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Dieses Lernprogramm funktioniert auch mit ASP.NET Web Pages 2.


## <a name="overview-of-helpers"></a>Übersicht über die Hilfsprogramme

Wenn Sie auf verschiedenen Seiten an Ihrem Standort die gleichen Aufgaben ausführen müssen, können Sie eine Hilfsfunktion verwenden. ASP.NET Web Pages umfasst eine Reihe von Hilfsprogrammen, und es gibt viele weitere, die Sie herunterladen und installieren können. (Eine Liste der integrierten Hilfsprogramme in ASP.NET Web Pages finden Sie der [ASP.NET API Kurzübersicht](https://go.microsoft.com/fwlink/?LinkId=202907).) Wenn keine der vorhandenen Hilfsprogramme Ihre Anforderungen erfüllt, können Sie eine eigene Hilfsfunktion erstellen.

Eine Hilfsprogramm können Sie einen allgemeinen Codeblock über mehrere Seiten verwenden. Nehmen Sie an, dass auf der Seite häufig ein Hinweis Element erstellen möchten, die abgesehen von normale Absätze festgelegt ist. Möglicherweise wird der Hinweis erstellt, als ein `<div>` Element, das formatiert wurde, wird als ein Feld mit einem Rahmen. Anstatt jedes Mal, wenn einen Hinweis angezeigt werden soll, wird diese gleiche Markup zu einer Seite hinzufügen, können Sie das Markup als Hilfsklasse packen. Anschließend können Sie den Hinweis mit einer einzigen Codezeile einfügen an einer beliebigen Stelle Sie ihn benötigen.

Mithilfe eines Hilfsprogramms wie folgt, wird der Code in den einzelnen Seiten vereinfacht und leichter zu lesen. Dies erleichtert auch an Ihren Standort zu warten, da Sie ggf. so ändern Sie die Anmerkungen zu dieser Vorschau können Sie das Markup an einem Ort ändern.

## <a name="creating-a-helper"></a>Erstellen eines-Hilfsprogramms

Dieses Verfahren wird gezeigt, wie Sie das Hilfsprogramm zu erstellen, das die Beachten Sie, wie soeben beschrieben erstellt. Dies ist ein einfaches Beispiel, jedoch die benutzerdefinierten Hilfsmethoden zählen Markup und ASP-Code, die Sie benötigen.

1. Erstellen Sie im Stammordner der Website, die einen Ordner namens *App\_Code*. Dies ist ein reservierter Ordnername in ASP.NET, an denen Sie Code für Komponenten, wie der Hilfsprogramme ablegen.
2. In der *App\_Code* Ordner erstellen Sie ein neues *cshtml* Datei, und nennen Sie sie *MyHelpers.cshtml*.
3. Ersetzen Sie den vorhandenen Inhalt durch Folgendes:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Der Code verwendet die `@helper` Syntax zum Deklarieren eines neuen Hilfsprogramms mit dem Namen `MakeNote`. Diese bestimmten-Hilfsobjekt ermöglicht Ihnen das Übergeben von eines Parameters namens `content` , kann eine Kombination von Text und Markup enthalten. Das Hilfsobjekt fügt die Zeichenfolge in der Anmerkung Textkörper mithilfe der `@content` Variable.

    Beachten Sie, dass die Datei heißt *MyHelpers.cshtml*, aber das Hilfsobjekt heißt `MakeNote`. Sie können mehrere benutzerdefinierte Hilfen in einer einzelnen Datei einfügen.
4. Speichern und schließen Sie die Datei.

## <a name="using-the-helper-in-a-page"></a>Verwenden das Hilfsprogramm auf einer Seite

1. Im Stammordner, erstellen Sie eine neue leere Datei namens *TestHelper.cshtml*.
2. Fügen Sie folgenden Code in die Datei ein:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Um das Hilfsobjekt aufzurufen, Sie erstellt haben, verwenden Sie `@` gefolgt von den Dateinamen, in dem das Hilfsprogramm ist, einem Punkt, und den Hilfsprogramm-Namen. (Wenn Sie mehrere Ordner, in hatten der *App\_Code* Ordner können Sie die Syntax `@FolderName.FileName.HelperName` aufrufen, die Hilfsmethode in einem geschachtelten Ordnerebene). Der Text, den Sie in Anführungszeichen innerhalb der Klammern hinzufügen wird der Text, den das Hilfsprogramm als Teil der Hinweis auf der Webseite angezeigt werden soll.
3. Speichern Sie die Seite, und führen Sie es in einem Browser. Das Hilfsprogramm generiert den Hinweis Element rechts, wenn Sie das Hilfsobjekt aufgerufen: zwischen den zwei Absätzen.

    ![Screenshot der Seite im Browser und wie das Hilfsprogramm Markup generiert, die ein Feld, um den angegebenen Text versetzt.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.jpg)

## <a name="additional-resources"></a>Zusätzliche Ressourcen


[Horizontale Menü als eine Razor-Hilfsprogramm](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). Dieser Blog von Mike Pope zeigt, wie ein horizontales Menü als Hilfsklasse mit Markup, CSS und Code zu erstellen.

[Nutzung von HTML5 in der ASP.NET Web Helpers für WebMatrix und ASP.NET MVC 3-Seiten](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Blogeintrag von Sam Abraham zeigt eine Hilfsprogramm, das eine HTML5 rendert `Canvas` Element.

[Der Unterschied zwischen @Helpers und @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). Dieser Blog von Mike Brind beschreibt `@helper` Syntax und `@function` Syntax und wann Sie jeweils zu verwenden.
