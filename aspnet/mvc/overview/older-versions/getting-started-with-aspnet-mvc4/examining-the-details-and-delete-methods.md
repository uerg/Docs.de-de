---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
title: Untersuchen die Details und Delete-Methoden | Microsoft-Dokumentation
author: Rick-Anderson
description: 'Hinweis: Eine aktualisierte Version dieses Tutorials ist hier verfügbar, dass das ASP.NET MVC 5 und Visual Studio 2013 verwendet. Es ist eine sicherere, viel einfacher zu folgen und demo...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 11425ff3-09fc-4efa-be9a-b53bce503460
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: fc007c7c0a6fdb818e39ef01437f5f9cf3d14513
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37375327"
---
<a name="examining-the-details-and-delete-methods"></a>Untersuchen die Details und Delete-Methoden
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Eine aktualisierte Version dieses Tutorials finden Sie [hier](../../getting-started/introduction/getting-started.md) , ASP.NET MVC 5 und Visual Studio 2013 verwendet. Dabei handelt es sich eine sicherere, einfacher, führen weitere Funktionen veranschaulicht.


In diesem Teil des Tutorials müssen Sie überprüfen die automatisch generierte `Details` und `Delete` Methoden.

## <a name="examining-the-details-and-delete-methods"></a>Untersuchen die Details und Delete-Methoden

Öffnen der `Movie` Controller, und untersuchen Sie die `Details` Methode.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

Die MVC-Gerüstbau-Engine, die diese Aktionsmethode erstellt hat, fügt einen Kommentar mit einer HTTP-Anforderung, die die Methode aufruft. In diesem Fall ist es eine `GET` -Anforderung mit drei URL-Segmente, die `Movies` Controller die `Details` Methode und eine `ID` Wert.

Code zunächst ganz einfach suchen nach Daten mithilfe der `Find` Methode. Eine wichtige Sicherheitsfunktion, die an die Methode erstellt wird, dass der Code, der überprüft, ob die `Find` Methode verfügt über einen Film gefunden, bevor der Code versucht, etwas damit zu tun. Z. B. ein Hacker kann Fehler verursachen auf der Website durch Ändern der URL erstellt, indem Sie die Links aus `http://localhost:xxxx/Movies/Details/1` z. B. `http://localhost:xxxx/Movies/Details/12345` (oder einen anderen Wert, der keinen tatsächlichen Film darstellt). Wenn Sie für einen null-Film nicht aktiviert haben, ergibt eine null-Movie eines Datenbankfehlers.

Untersuchen Sie die Methoden `Delete` und `DeleteConfirmed`.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Beachten Sie, dass die `HTTP Get``Delete` Methode nicht den angegebenen Film nicht gelöscht, es gibt eine Ansicht des Films, können Sie übermitteln (`HttpPost`) das Löschen... Das Ausführen eines Löschvorgangs als Reaktion auf eine GET-Anforderung (oder eigentlich eines Bearbeitungs-, Erstellungs- oder sonstigen Vorgangs, der Daten ändern) stellt eine Sicherheitslücke dar. Weitere Informationen hierzu finden Sie unter Stephen Walthers Blogeintrag [Tipp #46 von ASP.NET MVC – verwenden Sie keine Links löschen, da Sicherheitslücken entstehen](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

Die `HttpPost`-Methode, die die Daten löscht, heißt `DeleteConfirmed`, um der HTTP-POST-Methode eine eindeutige Signatur bzw. einen eindeutigen Namen zu geben. Die beiden Methodensignaturen werden nachstehend gezeigt:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Die Common Language Runtime (CLR) erfordert überladene Methoden, um eine eindeutige Parametersignatur zu erhalten (selber Methodenname, aber unterschiedliche Liste von Parametern). Hier benötigen Sie jedoch zwei Delete-Methoden – eine für GET und eine für POST beide die gleiche Parametersignatur haben. (Beide müssen eine einzelne ganze Zahl als Parameter akzeptieren.)

Um dies zu sortieren, können Sie ein paar Dinge tun. Eine besteht darin, die Methoden unterschiedlich zu benennen. Dafür das der Gerüstbaumechanismus im vorherigen Beispiel gesorgt. Dies bringt jedoch ein kleines Problem mit sich: ASP.NET ordnet Segmente einer URL anhand des Namens zu Aktionsmethoden zu. Wenn Sie die Methode umbenennen sollten, ist das Routing normalerweise nicht in der Lage, diese Methode zu finden. Die Lösung besteht (wie im Beispiel) im Hinzufügen des `ActionName("Delete")`-Attributs zur `DeleteConfirmed`-Methode. Dies effektiv führt die Zuordnung für das Routingsystem so, dass eine URL, die beinhaltet <em>/Delete/</em>für eine POST-Anforderung findet die `DeleteConfirmed` Methode.

Um ein Problem mit den Methoden zu vermeiden, die identische Namen und Signaturen haben eine andere Möglichkeit besteht darin die Signatur der POST-Methode, um ein nicht verwendeter Parameter einzuschließen künstlich zu ändern. Beispielsweise einige Entwickler einen Parametertyp hinzufügen `FormCollection` , das an die POST-Methode übergeben wird, und klicken Sie dann einfach nicht den Parameter verwenden:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Zusammenfassung

Sie haben jetzt eine vollständige ASP.NET MVC-Anwendung, die Daten in eine lokale DB-Datenbank speichert. Sie können zu erstellen, lesen, aktualisieren, löschen und suchen Sie nach Filme.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie erstellt und eine Webanwendung getestet haben, besteht der nächste Schritt, um sie an andere Personen zu verwenden, über das Internet verfügbar zu machen. Zu diesem Zweck müssen Sie es auf einen Webhostinganbieter bereitstellen. Microsoft bietet kostenloses Webhosting für bis zu 10 Websites in einem [kostenlose Windows Azure-Testkonto](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Ich schlage vor, Sie als Nächstes führen Sie meine Tutorial [bereitstellen eine sicheren ASP.NET MVC-app mit Mitgliedschaft, OAuth und SQL-Datenbank auf einem Windows Azure-Website](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Ein ausgezeichnetes Tutorial ist Tom Dykstras fortgeschrittene [erstellen ein Entity Framework-Datenmodells für eine ASP.NET MVC-Anwendung](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md). [Stackoverflow](http://stackoverflow.com/help) und [ASP.NET MVC-Foren](https://forums.asp.net/1146.aspx) sind eine gute platziert werden, um Fragen zu stellen. Führen Sie [mich](https://twitter.com/RickAndMSFT) auf Twitter, sodass Sie Updates auf meinem neuesten Lernprogramme zugreifen können.

Feedback ist Willkommen.

– [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
– [Scott Hanselman](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Vorherige](adding-validation-to-the-model.md)
