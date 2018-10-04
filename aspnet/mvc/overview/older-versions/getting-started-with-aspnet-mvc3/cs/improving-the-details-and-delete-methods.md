---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
title: Verbessern die Details und Delete-Methoden (c#) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, d.h....
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 3f42edd9-c5b8-4712-9055-970f7d38e350
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 255374eb21568d05569f8af6727ad4b558acfc2f
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576439"
---
<a name="improving-the-details-and-delete-methods-c"></a>Verbessern die Details und Delete-Methoden (c#)
====================
durch [Rick Anderson]((https://twitter.com/RickAndMSFT))

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials finden Sie [hier](../../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Dabei handelt es sich eine sicherere, einfacher, führen weitere Funktionen veranschaulicht.
> 
> 
> In diesem Tutorial lernen Sie die Grundlagen zum Erstellen einer ASP.NET MVC-Web-Anwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, handelt es sich eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle auf den folgenden Link installieren: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie einzeln die Voraussetzungen, die über die folgenden Links installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Toolsupdate](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime und Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die erforderlichen Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit C#-Quellcode ist verfügbar, zur Ergänzung dieses Themas. [Laden Sie die C#-Version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie Visual Basic bevorzugen, wechseln Sie zu der [Visual Basic-Version](../vb/intro-to-aspnet-mvc-3.md) in diesem Tutorial.


In diesem Teil des Tutorials erstellen Sie einige Verbesserungen an den automatisch generierten `Details` und `Delete` Methoden. Diese Änderungen sind nicht erforderlich, jedoch mit nur wenigen kleine Codeabschnitte, können Sie problemlos die Anwendung verbessern.

## <a name="improving-the-details-and-delete-methods"></a>Verbessern die Details und Delete-Methoden

Wenn Sie erstellt haben die `Movie` Controller, ASP.NET MVC generierte Code funktioniert hervorragend, aber, können robuster gestaltet mit wenigen kleinen Änderungen.

Öffnen der `Movie` Controller und ändern Sie die `Details` Methode durch Rückgabe `HttpNotFound` Wenn ein Film nicht gefunden. Sie sollten auch ändern, die `Details` Methode, um einen Standardwert für die ID festzulegen, die an ihn übergeben wird. (Sie vorgenommen haben, wie Änderungen an der `Edit` -Methode in der [Teil 6](examining-the-edit-methods-and-edit-view.md) in diesem Tutorial.) Jedoch müssen Sie den Rückgabetyp Ändern der `Details` aus `ViewResult` zu `ActionResult`, da die `HttpNotFound` Methode nicht zurückgegeben. eine `ViewResult` Objekt. Das folgende Beispiel zeigt die geänderte `Details` Methode.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample1.cs)]

Code zunächst ganz einfach suchen nach Daten mithilfe der `Find` Methode. Eine wichtige Sicherheitsfunktion, die an die Methode erstellt wird, dass der Code, der überprüft, ob die `Find` Methode verfügt über einen Film gefunden, bevor der Code versucht, etwas damit zu tun. Z. B. ein Hacker kann Fehler verursachen auf der Website durch Ändern der URL erstellt, indem Sie die Links aus `http://localhost:xxxx/Movies/Details/1` z. B. `http://localhost:xxxx/Movies/Details/12345` (oder einen anderen Wert, der keinen tatsächlichen Film darstellt). Wenn Sie die Überprüfung auf einen null-Film nicht tun, könnte dies zu einem Datenbankfehler führen.

Analog dazu ändern Sie die `Delete` und `DeleteConfirmed` Methoden, die einen Standardwert für den ID-Parameter angeben und zurückzugeben `HttpNotFound` Wenn ein Film nicht gefunden. Die aktualisierte `Delete` Methoden in der `Movie` Controller werden unten angezeigt.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample2.cs)]

Beachten Sie, dass die `Delete` Methode nicht die Daten zu löschen. Das Ausführen eines Löschvorgangs als Reaktion auf eine GET-Anforderung (oder eigentlich eines Bearbeitungs-, Erstellungs- oder sonstigen Vorgangs, der Daten ändern) stellt eine Sicherheitslücke dar. Weitere Informationen hierzu finden Sie unter Stephen Walthers Blogeintrag [Tipp #46 von ASP.NET MVC – verwenden Sie keine Links löschen, da Sicherheitslücken entstehen](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

Die `HttpPost`-Methode, die die Daten löscht, heißt `DeleteConfirmed`, um der HTTP-POST-Methode eine eindeutige Signatur bzw. einen eindeutigen Namen zu geben. Die beiden Methodensignaturen werden nachstehend gezeigt:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample3.cs)]

Die common Language Runtime (CLR) erfordert überladene Methoden, um eine eindeutige Signatur (denselben Namen, unterschiedliche Liste von Parametern) aufweisen. Hier benötigen Sie jedoch zwei Delete-Methoden – eine für GET und eine für POST, dass beide die gleiche Signatur erfordern. (Beide müssen eine einzelne ganze Zahl als Parameter akzeptieren.)

Um dies zu sortieren, können Sie ein paar Dinge tun. Eine besteht darin, die Methoden unterschiedlich zu benennen. Das ist was wir in der er die vorangehenden Beispiel getan haben. Dies bringt jedoch ein kleines Problem mit sich: ASP.NET ordnet Segmente einer URL anhand des Namens zu Aktionsmethoden zu. Wenn Sie die Methode umbenennen sollten, ist das Routing normalerweise nicht in der Lage, diese Methode zu finden. Die Lösung besteht (wie im Beispiel) im Hinzufügen des `ActionName("Delete")`-Attributs zur `DeleteConfirmed`-Methode. Dies effektiv führt die Zuordnung für das Routingsystem so, dass eine URL, die beinhaltet <em>/Delete/</em>für eine POST-Anforderung findet die `DeleteConfirmed` Methode.

Eine andere Möglichkeit, ein Problem mit den Methoden zu vermeiden, die identische Namen Signaturen und werden künstlich ändern Sie die Signatur der POST-Methode, um ein nicht verwendeter Parameter einzuschließen. Beispielsweise einige Entwickler einen Parametertyp hinzufügen `FormCollection` , das an die POST-Methode übergeben wird, und klicken Sie dann einfach nicht den Parameter verwenden:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="wrapping-up"></a>Zusammenfassung

Sie haben jetzt eine vollständige ASP.NET MVC-Anwendung, die Daten in einer SQL Server Compact-Datenbank speichert. Sie können zu erstellen, lesen, aktualisieren, löschen und suchen Sie nach Filme.

![](improving-the-details-and-delete-methods/_static/image1.png)

Dieses Lernprogramm haben Sie die Schritte der Controller mit Ansichten verknüpft werden, und übergeben, um hartcodierte Daten vornehmen. Klicken Sie dann Sie erstellt wurden und ein Datenmodell. Entity Framework Code First eine Datenbank aus dem Datenmodell dynamisch erstellt, und das System der ASP.NET MVC-Gerüstbau automatisch generiert, die Aktionsmethoden und Ansichten für grundlegende CRUD-Vorgänge. Hinzufügen klicken Sie dann ein Suchformular, mit denen Benutzer die Datenbank durchsuchen kann. Sie die Datenbank enthält eine neue Spalte mit Daten geändert und aktualisiert dann zwei Seiten zum Erstellen und diese neuen Daten anzuzeigen. Sie Überprüfung hinzugefügt, durch markieren das Datenmodell mit Attributen aus dem `DataAnnotations` Namespace. Die resultierende Überprüfung wird ausgeführt, auf dem Client und auf dem Server.

Wenn Sie Ihre Anwendung bereitstellen möchten, ist es hilfreich, zunächst die Anwendung auf dem lokalen IIS 7-Server zu testen. Sie können dies verwenden [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) Link zur IIS-Einstellung für ASP.NET-Anwendungen aktivieren. Finden Sie unter den folgenden Links für die Bereitstellung:

- [Einstieg in ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [Aktivieren von IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Bereitstellung von Webanwendungen-Projekte](https://msdn.microsoft.com/library/dd394698.aspx)

Jetzt sollten Sie mit unserem fortgeschrittene Fortfahren [erstellen ein Entity Framework-Datenmodells für eine ASP.NET MVC-Anwendung](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) und [MVC Music Store](../../mvc-music-store/mvc-music-store-part-1.md) Tutorials, um zu untersuchen der [ASP.NET MSDN-Artikeln für](https://msdn.microsoft.com/library/gg416514(VS.98).aspx), und überprüfen, die viele Videos und Ressourcen zu [ https://asp.net/mvc ](https://asp.net/mvc) auch weitere Informationen zur ASP.NET MVC! Die [ASP.NET MVC-Foren](https://forums.asp.net/1146.aspx) sind hervorragend, um Fragen zu stellen.

Viel Erfolg!

— Scott Hanselman ([ http://hanselman.com ](http://hanselman.com) und [ @shanselman ](http://twitter.com/shanselman) auf Twitter) und Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Vorherige](adding-validation-to-the-model.md)
