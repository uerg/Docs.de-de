---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
title: Verbessern die Details und Delete-Methoden (VB) | Microsoft Docs
author: Rick-Anderson
description: In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: c5c14ef0-c128-4dc1-8c01-7f0fdb09e411
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 0c662510ce9a80e0e808af0eec2561ecdaa12c01
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="improving-the-details-and-delete-methods-vb"></a>Verbessern die Details und Delete-Methoden (VB)
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Lernprogramm erfahren Sie die Grundlagen der Erstellung einer ASP.NET MVC-Webanwendung mithilfe von Microsoft Visual Web Developer 2010 Express Service Pack 1, also eine kostenlose Version von Microsoft Visual Studio. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben. Sie können alle installieren, indem Sie auf den folgenden Link: [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Alternativ können Sie die erforderlichen Komponenten, die über die folgenden Links einzeln installieren:
> 
> - [Visual Studio Web Developer Express SP1-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(Common Language Runtime + Tools unterstützen)
> 
> Wenn Sie Visual Studio 2010 anstelle von Visual Web Developer 2010 verwenden, die Komponenten installieren, indem Sie auf den folgenden Link: [Visual Studio 2010-Voraussetzungen](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Ein Visual Web Developer-Projekt mit Quellcode VB.NET ist zu diesem Thema steht zur Verfügung. [Laden Sie die Version VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Wenn Sie C#-bevorzugen, wechseln Sie zu der [C#-Version](../cs/improving-the-details-and-delete-methods.md) dieses Lernprogramm.


In diesem Teil des Lernprogramms, treffen Sie einige Verbesserungen, die automatisch generierte `Details` und `Delete` Methoden. Diese Änderungen sind nicht erforderlich, aber mit nur wenigen kleine Codeabschnitte, können Sie problemlos optimieren der Anwendung.

## <a name="improving-the-details-and-delete-methods"></a>Verbessern die Details und Delete-Methoden

Wenn Sie migrationsgerüst der `Movie` Controller, ASP.NET MVC generiert Code, die arbeitet hervorragend, aber, erfolgen mit nur wenigen kleinere Änderungen unter Umständen stabiler.

Öffnen der `Movie` Controller und Ändern der `Details` Methode wird durch zurückgeben `HttpNotFound` Wenn ein Film nicht gefunden. Sie sollten auch ändern, die `Details` Methode, um einen Standardwert für die ID einzurichten, die an sie übergeben wird. (Sie vorgenommen haben ähnliche Änderungen an der `Edit` Methode in [Teil 6](examining-the-edit-methods-and-edit-view.md) dieses Lernprogramms.) Sie müssen jedoch ändern, den Rückgabetyp der der `Details` Methode aus `ViewResult` auf `ActionResult`, da die `HttpNotFound` Methodenrückgabewert keine `ViewResult` Objekt. Das folgende Beispiel zeigt die geänderte `Details` Methode.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample1.vb)]

Code zuerst ganz einfach zu suchenden Daten mithilfe der `Find` Methode. Eine wichtige Sicherheitsfunktion, die wir in die Methode erstellt wird, dass der Code, der überprüft, ob die `Find` Methode verfügt über einen Film gefunden, bevor der Code versucht, die mit ihm keine Wirkung. Angenommen, ein Hacker kann Fehler verursachen bei der Website durch Änderung der URL erstellt, indem die Links von `http://localhost:xxxx/Movies/Details/1` z. B. `http://localhost:xxxx/Movies/Details/12345` (oder einen anderen Wert, der einen tatsächliche Film darzustellen nicht). Wenn Sie die Prüfung auf null Film nicht tun, könnte dies zu einem Datenbankfehler führen.

Auf ähnliche Weise ändern die `Delete` und `DeleteConfirmed` Methoden, um einen Standardwert für den ID-Parameter angeben und für die Rückgabe `HttpNotFound` Wenn ein Film nicht gefunden. Die aktualisierte `Delete` Methoden in der `Movie` Controller sind unten aufgeführt.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample2.vb)]

Beachten Sie, dass die `Delete` Methode löscht die Daten. Das Ausführen eines Löschvorgangs als Reaktion auf eine GET-Anforderung (oder eigentlich eines Bearbeitungs-, Erstellungs- oder sonstigen Vorgangs, der Daten ändern) stellt eine Sicherheitslücke dar. Weitere Informationen hierzu finden Sie unter Stephen Walthers Blogeintrag [Tipp #46 von ASP.NET MVC – verwenden Sie keine Links zu löschen, da Sicherheitslücken entstehen](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

Die `HttpPost`-Methode, die die Daten löscht, heißt `DeleteConfirmed`, um der HTTP-POST-Methode eine eindeutige Signatur bzw. einen eindeutigen Namen zu geben. Die beiden Methodensignaturen werden nachstehend gezeigt:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample3.vb)]

Die common Language Runtime (CLR) erfordert überladene Methoden, um eine eindeutige Signatur (gleichnamigen, andere Liste von Parametern) aufweisen. Hier benötigen Sie jedoch zwei Delete-Methoden – eine für GET – und eine für POST, dass beide die gleiche Signatur erforderlich ist. (Beide müssen eine einzelne ganze Zahl als Parameter akzeptieren.)

Um dies zu sortieren, können Sie mehrere Dinge tun. Eine besteht darin, die Methoden unterschiedlich benennen. Was haben wir in er das vorhergehende Beispiel ist. Dies bringt jedoch ein kleines Problem mit sich: ASP.NET ordnet Segmente einer URL anhand des Namens zu Aktionsmethoden zu. Wenn Sie die Methode umbenennen sollten, ist das Routing normalerweise nicht in der Lage, diese Methode zu finden. Die Lösung besteht (wie im Beispiel) im Hinzufügen des `ActionName("Delete")`-Attributs zur `DeleteConfirmed`-Methode. Diese Zuordnung effektiv für das routing-System wird, sodass eine URL, die enthält <em>/Delete/</em>für einen POST Anforderung findet die `DeleteConfirmed` Methode.

Eine andere Möglichkeit, ein Problem mit den Methoden zu vermeiden, die über identische Namen und Signaturen verfügen, besteht darin die Signatur der POST-Methode, um einen nicht verwendeten Parameter umfassen künstlich zu ändern. Z. B. einige Entwickler Parametertyp hinzufügen `FormCollection` , an die POST-Methode übergeben wird, und klicken Sie dann einfach nicht den Parameter verwenden:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample4.vb)]

## <a name="wrapping-up"></a>Zusammenfassung

Sie haben jetzt eine vollständige ASP.NET MVC-Anwendung, die Daten in einer SQL Server Compact-Datenbank speichert. Sie können erstellen, lesen, aktualisieren, löschen und suchen Sie nach Filme.

![](improving-the-details-and-delete-methods/_static/image1.png)

Dieses Lernprogramm wurde Ihnen den Einstieg, Controller, Ansichten zuordnen, und übergeben hartcodierte Daten vornehmen. Klicken Sie dann erstellt und ein Datenmodell entwickelt. Entity Framework Code First, die eine Datenbank erstellt, aus dem Datenmodell bei Bedarf, und das ASP.NET MVC-Gerüstbau System automatisch generiert, die Aktionsmethoden und Ansichten für grundlegende CRUD-Vorgänge. Sie haben dann eine Search-Formular, mit denen Benutzer die Datenbank durchsuchen kann hinzugefügt. Sie die Datenbank um eine neue Spalte mit Daten enthalten geändert und aktualisiert dann zwei Seiten zum Erstellen und diese neuen Daten anzuzeigen. Überprüfung durch markieren das Datenmodell mit Attributen hinzugefügt der `DataAnnotations` Namespace. Die resultierende Validierung wird auf dem Client und auf dem Server ausgeführt.

Wenn Sie die Anwendung bereitstellen möchten, empfiehlt es sich zum ersten Testen der Anwendung auf dem lokalen IIS 7-Server. Sie können dies [Webplattform-Installer](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) Link zur IIS-Einstellung für ASP.NET-Anwendungen aktivieren. Finden Sie unter den folgenden Links für die Bereitstellung:

- [ASP.NET Bereitstellung-Inhaltszuordnung](https://msdn.microsoft.com/library/dd394698.aspx)
- [Aktivieren von IIS 7.x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Bereitstellung von Webanwendungen-Projekte](https://msdn.microsoft.com/library/dd394698.aspx)

Jetzt sollten Sie auf unserer Intermediate-Stufe zu verschieben, auf [Erstellen eines Entity Framework-Datenmodells für eine ASP.NET MVC-Anwendung](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) und [MVC Music Store](../../mvc-music-store/mvc-music-store-part-1.md) Lernprogramme zum Durchsuchen der [ASP.NET MSDN-Artikel](https://msdn.microsoft.com/library/gg416514(VS.98).aspx), und beim Auschecken der viele Videos und Ressourcen auf [ https://asp.net/mvc ](https://asp.net/mvc) , sogar noch stärker ASP.NET MVC vertraut zu machen! Die [ASP.NET MVC-Foren](https://forums.asp.net/1146.aspx) sind ein idealer Ausgangspunkt, um Fragen zu stellen.

Viel Erfolg!

– Scott Hanselman ([ http://hanselman.com ](http://hanselman.com) und [ @shanselman ](http://twitter.com/shanselman) auf Twitter) und Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Vorherige](adding-validation-to-the-model.md)
