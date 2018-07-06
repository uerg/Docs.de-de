---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'ASP.NET-webbereitstellung mithilfe von Visual Studio: Bereitstellen von zusätzlichen Dateien | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, indem Warnungsprovider...
ms.author: aspnetcontent
ms.date: 03/23/2015
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 609c0be968e8f38d7be6e5f5c96a569a9a35c2eb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820849"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>ASP.NET-webbereitstellung mithilfe von Visual Studio: Bereitstellen von zusätzlichen Dateien
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, mithilfe von Visual Studio 2012 oder Visual Studio 2010. Weitere Informationen über die Reihe finden Sie unter [im ersten Tutorial der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

Dieses Tutorial veranschaulicht, wie erweitern das Visual Studio Web veröffentlichen Pipeline, um eine zusätzliche Aufgabe während der Bereitstellung durchzuführen. Die Aufgabe besteht darin, zusätzliche Dateien kopieren, die nicht in den Projektordner, der Ziel-Website befinden.

In diesem Tutorial kopieren Sie eine zusätzliche Datei: *robots.txt*. Diese Datei in die Stagingumgebung, aber nicht in die Produktion bereitstellen möchten. In [Bereitstellen in der Produktion](deploying-to-production.md) Tutorial, das Sie diese Datei dem Projekt hinzugefügt und konfiguriert die Produktion Veröffentlichungsprofil, um sie auszuschließen. In diesem Tutorial sehen Sie eine alternative Methode für diese Situation, eine, die für alle Dateien nützlich sind, die Sie bereitstellen möchten, aber nicht im Projekt enthalten sein sollen.

## <a name="move-the-robotstxt-file"></a>Verschieben Sie die Datei "robots.txt"

Zur Vorbereitung der Behandlung einer anderen Methode *robots.txt*, in diesem Abschnitt des Lernprogramms verschieben Sie die Datei in einen Ordner, die nicht im Projekt enthalten ist und Sie löschen *robots.txt* aus den Stagingtabellen Umgebung. Es ist erforderlich, löschen Sie die Datei aus der Staging-, damit Sie überprüfen können, dass die neue Methode der Bereitstellung der das für diese Umgebung ordnungsgemäß funktioniert.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die *robots.txt* Datei, und klicken Sie auf **aus Projekt ausschließen**.
2. Erstellen Sie einen neuen Ordner im Projektmappenordner und nennen Sie sie mithilfe des Windows-Datei-Explorers *ExtraFiles*.
3. Verschieben der *robots.txt* -Datei aus der *ContosoUniversity* Projektordners mit dem die *ExtraFiles* Ordner.

    ![ExtraFiles-Ordner](deploying-extra-files/_static/image1.png)
4. Löschen Sie Ihre FTP-Tool, mit der *robots.txt* Datei von der staging-Website.

    Als Alternative können Sie auswählen können **entfernen weiterer Dateien am Ziel** unter **Dateiveröffentlichungsoptionen** auf die **Einstellungen** Registerkarte des Veröffentlichungsprofils Staging, und Veröffentlichen Sie erneut in die Stagingumgebung.

## <a name="update-the-publish-profile-file"></a>Aktualisieren Sie das Veröffentlichungsprofil

Sie müssen nur *robots.txt* in der Stagingumgebung verfügbar, sodass Staging das einzige Veröffentlichungsprofil, das Sie aktualisieren, damit Sie sie bereitstellen möchten.

1. Öffnen Sie in Visual Studio *Staging.pubxml*.
2. Am Ende der Datei vor dem schließenden `</Project>` markieren, fügen Sie das folgende Markup hinzu:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Dieser Code erstellt ein neues *Ziel* sammelt, die zusätzliche Dateien bereitgestellt werden. Ein Ziel besteht aus einer oder mehrere Aufgaben, die ausgeführt wird, dass Sie MSBuild auf der Grundlage von Bedingungen, die Sie angeben.

    Die `Include` Attribut gibt an, dass der Ordner, in dem sich die Dateien befinden *ExtraFiles*auf derselben Ebene wie der Projektordner. MSBuild sammelt alle Dateien in diesem Ordner und rekursiv Unterordnern (das doppelte Sternchen gibt rekursiven Unterordner). Mit diesem Code können Sie mehrere Dateien und Dateien platzieren, in Unterordnern innerhalb der *ExtraFiles* Ordner und alle bereitgestellt werden.

    Die `DestinationRelativePath` Element gibt an, dass die Ordner und Dateien in den Stammordner der Ziel-Website, in der gleichen Struktur von Dateien und Ordner kopiert werden sollen, wie in der sie gefunden werden die *ExtraFiles* Ordner. Wenn Sie, kopieren Sie möchten die *ExtraFiles* Ordner selbst, die `DestinationRelativePath` -Werte entsprächen dann *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. Am Ende der Datei vor dem schließenden `</Project>` markieren, fügen Sie das folgende Markup, der angibt, wann das neue Ziel ausgeführt.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Dieser Code bewirkt, dass die neue `CustomCollectFiles` Ziel ausgeführt werden, wenn das Ziel, die Dateien in den Zielordner kopiert ausgeführt wird. Es ist ein separater Ziel für die zu veröffentlichen, im Vergleich zu paketerstellung für die Bereitstellung und das neue Ziel wird in beide Ziele für den Fall, dass Sie entscheiden, bereitstellen, indem Sie die Veröffentlichung ein Bereitstellungspakets anstelle eingefügt.

    Die *pubxml* Datei sieht nun wie im folgenden Beispiel:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Speichern und schließen Sie die *Staging.pubxml* Datei.

## <a name="publish-to-staging"></a>In der Stagingumgebung veröffentlichen

Mit One-Click-Veröffentlichung oder der Befehlszeile, die die Anwendung zu veröffentlichen, über das Staging-Profil.

Bei Verwendung von nur einem Klick zu veröffentlichen, können Sie überprüfen, in der **Vorschau** Fenster, *robots.txt* kopiert werden. Andernfalls verwenden Sie Ihre FTP-Tool, um zu überprüfen, ob die *robots.txt* Datei ist im Stammordner der Website nach der Bereitstellung.

## <a name="summary"></a>Zusammenfassung

Dies schließt diese tutorialreihe zum Bereitstellen einer ASP.NET-Webanwendung zu einem Hostinganbieter von Drittanbietern. Weitere Informationen zu den Themen in diesen Tutorials behandelt, finden Sie unter den [ASP.NET die ASP.NET-Bereitstellung](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Weitere Informationen

Wenn Sie zum Arbeiten mit Dateien von MSBuild kennen, können Sie weitere Bereitstellungsaufgaben automatisieren, indem Schreiben von Code in *pubxml* -Dateien (für Profile-spezifische Aufgaben) oder das Projekt *. wpp.targets* Datei (für tasks, gelten Sie für alle Profile). Weitere Informationen zu *pubxml* und *. wpp.targets* finden Sie unter [Vorgehensweise: Bearbeiten der Bereitstellungseinstellungen in veröffentlichen Veröffentlichungsprofildateien (.pubxml)-Dateien und die. wpp.targets-Datei in Visual Studio Web Projekte](https://msdn.microsoft.com/library/ff398069). Eine grundlegende Einführung in MSBuild-Code, finden Sie unter **Anatomie einer Projektdatei** in [Unternehmensbereitstellung Serie: Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Arbeiten mit Dateien von MSBuild zum Ausführen von Aufgaben für Ihre eigenen Szenarien finden Sie unter diesem Buch: [innerhalb der Microsoft Build Engine: Verwenden von MSBuild und Team Foundation Build](http://msbuildbook.com) von Sayed Hashimi von Ibraham und William Bartholomew.

## <a name="acknowledgements"></a>Bestätigungen

Ich möchte die vielen Dank, dass die folgenden Personen, die wichtige Beiträge zu den Inhalt dieser tutorialreihe vorgenommen:

- [Alberto Poblacion, MVP &amp; MCT, Spanien](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, Entwicklung von Data Platform MVP, Vereinigte Staaten
- Harsh Mittal, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike Papst, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava, Microsoft
- [Raffaele Rialdi, Italien](http://www.iamraf.net/)
- [Rick Anderson, Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [Zurück](command-line-deployment.md)
> [Weiter](troubleshooting.md)
