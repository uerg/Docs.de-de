---
uid: mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
title: Einführung in das Lernprogramm NerdDinner | Microsoft Docs
author: shanselman
description: Die beste Möglichkeit, erfahren, ein neues Framework besteht darin mit erstellen. In diesem Lernprogramm erläutert, wie eine kleine, aber vollständige, Anwendung, die mit ASP.NET.. erstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 397522d5-0402-4b94-b810-a2fb564f869d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/introducing-the-nerddinner-tutorial
msc.type: authoredcontent
ms.openlocfilehash: 3d925a7dc89fc0c742468653c5c138a0f1d71231
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868554"
---
<a name="introducing-the-nerddinner-tutorial"></a>Einführung in das Lernprogramm NerdDinner
====================
durch [Scott Hanselman](https://github.com/shanselman)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Die beste Möglichkeit, erfahren, ein neues Framework besteht darin mit erstellen. Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen einer kleines, jedoch abgeschlossen, Anwendung, die mithilfe von ASP.NET MVC-1, und eine Einführung in die entsprechende kernbegriffe.
> 
> Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.


## <a name="nerddinner-tutorial"></a>NerdDinner-Lernprogramm

Die beste Möglichkeit, erfahren, ein neues Framework besteht darin mit erstellen. Dieses Lernprogramm führt Sie durch die Schritte zum Erstellen einer kleines, jedoch abgeschlossen haben, mithilfe von ASP.NET MVC, Anwendung und eine Einführung in die entsprechende kernbegriffe.

Die Anwendung, die wir erstellen möchten, wird "NerdDinner" aufgerufen. NerdDinner bietet eine einfache Möglichkeit für Benutzer zum Suchen und Organisieren von Abendessen online:

![](introducing-the-nerddinner-tutorial/_static/image1.png)

NerdDinner kann registrierte Benutzer erstellen, bearbeiten und Löschen von Abendessen angezeigt. Er erzwingt einen konsistenten Satz von Überprüfung und Geschäftsregeln in der Anwendung:

![](introducing-the-nerddinner-tutorial/_static/image2.png)

Besucher können eine AJAX-basierte Zuordnung um zu suchende bevorstehende Abendessen in der Nähe sie gespeichert werden:

![](introducing-the-nerddinner-tutorial/_static/image3.png)

Klicken eine Dinner gelangen sie zur eine Seite "Details", in denen erfahren sie mehr darüber:

![](introducing-the-nerddinner-tutorial/_static/image4.png)

Wenn sie interessiert der Teilnahme an der Dinner können auf der Website registrieren oder anmelden:

![](introducing-the-nerddinner-tutorial/_static/image5.png)

Sie können einen AJAX-basierten Antwort-Link, um das Ereignis studiert klicken:

![](introducing-the-nerddinner-tutorial/_static/image6.png)

![](introducing-the-nerddinner-tutorial/_static/image7.png)

### <a name="implementing-nerddinner"></a>NerdDinner implementieren

Wir werden unsere NerdDinner Anwendung beginnen, indem Sie mithilfe von Datei -&gt;Befehl Neues Projekt in Visual Studio zum Erstellen eines brandneuen ASP.NET MVC-Projekts. Wir wird dann schrittweise Funktionen erweitert werden. Dabei werden folgende Themen behandelt:

1. [Vorgehensweise: Erstellen Sie ein neues ASP.NET MVC-Projekt](# "erstellen Sie ein neues ASP.NET MVC-Projekt")
2. [Gewusst wie: Erstellen einer Datenbank](# "erstellen Sie eine Datenbank")
3. [Gewusst wie: Erstellen eines Modells mit Business Regel Überprüfungen](# "Erstellen eines Modells mit Business Regel Überprüfungen")
4. [Gewusst wie: Controller und Ansichten verwenden, um eine Liste/Detail-Benutzeroberfläche implementieren](# "verwenden Controller und Ansichten zum Implementieren einer Auflistung/Detail-Benutzeroberfläche")
5. [Gewusst wie: Bereitstellen von CRUD (erstellen, lesen, aktualisieren und löschen) Daten bilden Eintrag Unterstützung](# "bieten CRUD (Create, Read, Update, Delete) Daten Formular Dateneingabe zu unterstützen")
6. [Zum Verwenden von ViewData und Implementierung von Klassen ViewModel](# "ViewData verwenden und Implementieren von ViewModel-Klassen")
7. [Gewusst wie: Verwenden Sie die Benutzeroberfläche mit Masterseiten und Replikatsgruppenelemente erneut](# "Benutzeroberfläche mithilfe von Masterseiten erneut verwenden und Replikatsgruppenelemente")
8. [Zum Implementieren von Paging effiziente](# "implementieren effizient Daten Paging")
9. [Sichern von Anwendungen mithilfe von Authentifizierung und Autorisierung](# "sichere Anwendungen mithilfe von Authentifizierung und Autorisierung")
10. [Wie Sie mithilfe von AJAX dynamische Updates bereitstellen](# "AJAX verwenden, um dynamische Updates bieten")
11. [Wie AJAX zuordnungsszenarien implementieren mit](# "AJAX verwenden, um Zuordnungsszenarien implementieren")
12. [So aktivieren Sie automatisierte Komponententests](# "automatisierte Komponententest aktivieren")

Sie können Ihr eigenes Exemplar des NerdDinner Erstellen von Grund auf neu, indem Abschließen jeder Schritt wir Exemplarische Vorgehensweise in diesem Kapitel. Alternativ können Sie eine abgeschlossene Version des Quellcodes hier herunterladen: [NerdDinner auf GitHub](https://github.com/AspNetMVPSamples/NerdDinner). Sie können auch optional auch [Laden Sie eine kostenlose PDF-Version dieses Lernprogramms](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf) , wenn Sie das Lernprogramm offline lesen möchten.

Sie können Visual Studio 2008 oder die kostenlose Visual Web Developer 2008 Express verwenden, zum Erstellen der Anwendung. Sie können SQL Server oder den freien SQL Server Express für die Datenbank verwenden.

Sie können ASP.NET MVC, Visual Web Developer 2008 Express und SQL Server Express (kostenlos) mithilfe von V2 installieren die [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)

### <a name="now-lets-get-started"></a>Jetzt sehen wir beginnen...

Wir haben behandelt, was NerdDinner ist, wir werden unsere Hüllen als Rollup angegeben, und Code schreiben.

Wir beginnen, indem Sie mithilfe von Datei -&gt;neues Projekt in Visual Studio die NerdDinner-Anwendung erstellen.

> [!div class="step-by-step"]
> [Nächste](create-a-new-aspnet-mvc-project.md)
