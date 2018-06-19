---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellen von zusätzlichen Dateien | Microsoft Docs'
author: tdykstra
description: Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder mit einem Hostinganbieter von Drittanbietern durch wählen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 5cefcedde7715844a7d7a9db1455193564ef9805
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30881762"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellen von zusätzlichen Dateien
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung eines Drittanbieters Hostinganbieter oder Azure App Service-Web-Apps mithilfe von Visual Studio 2012 oder Visual Studio 2010. Informationen über die Reihen finden Sie unter [im ersten Lernprogramm, in der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

Dieses Lernprogramm veranschaulicht das Erweitern der Visual Studio Web veröffentlichen Pipeline um eine zusätzliche Aufgabe auszuführen, während der Bereitstellung. Die Aufgabe besteht darin, zusätzliche Dateien kopieren, die nicht in den Projektordner der Ziel-Website enthalten sind.

Für dieses Lernprogramm müssen Sie eine zusätzliche Datei kopieren: *"robots.txt"*. Diese Datei in die Stagingumgebung, aber nicht für die Produktion bereitgestellt werden soll. In [der Bereitstellung bis hin zur Produktion](deploying-to-production.md) Tutorial, diese Datei dem Projekt hinzugefügt und konfiguriert die Produktion Veröffentlichungsprofil, damit er ausgeschlossen. In diesem Lernprogramm sehen Sie eine alternative Methode für diese Situation, eine, die hilfreich für alle Dateien werden, die Sie bereitstellen möchten, aber nicht in das Projekt enthalten sein soll.

## <a name="move-the-robotstxt-file"></a>Verschieben Sie die Datei "robots.txt"

Zur Vorbereitung einer anderen Methode zur Behandlung *"robots.txt"*, in diesem Abschnitt des Lernprogramms verschieben Sie die Datei in einen Ordner, die nicht im Projekt enthalten ist, und Sie löschen *"robots.txt"* aus den Stagingtabellen Umgebung. Es ist erforderlich, löschen Sie die Datei aus der Stagingtabelle, damit Sie überprüfen können, dass die neue Methode der Bereitstellung von der Datei für diese Umgebung einwandfrei funktioniert.

1. In **Projektmappen-Explorer**, mit der rechten Maustaste die *"robots.txt"* Datei, und klicken Sie auf **aus Projekt ausschließen**.
2. Erstellen Sie einen neuen Ordner im Projektmappenordner und nennen Sie sie mithilfe des Windows-Datei-Explorers *ExtraFiles*.
3. Verschieben der *"robots.txt"* -Datei von der *ContosoUniversity* Projektordner auf den *ExtraFiles* Ordner.

    ![ExtraFiles Ordner](deploying-extra-files/_static/image1.png)
4. Mit dem FTP-Tool Löschen der *"robots.txt"* Datei von der staging-Website.

    Als Alternative können Sie auswählen können **entfernen weiterer Dateien am Ziel** unter **Dateiveröffentlichungsoptionen** auf die **Einstellungen** Registerkarte des Veröffentlichungsprofils Staging- und erneut veröffentlichen Sie, in die Stagingumgebung.

## <a name="update-the-publish-profile-file"></a>Die veröffentlichungsprofildatei aktualisieren

Sie müssen nur *"robots.txt"* in der Stagingumgebung, damit Staging nur Veröffentlichungsprofil aktualisieren, damit Sie sie bereitstellen müssen.

1. Öffnen Sie in Visual Studio *Staging.pubxml*.
2. Am Ende der Datei vor der schließenden `</Project>` kennzeichnen, fügen Sie das folgende Markup hinzu:

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    Dieser Code erstellt ein neues *Ziel* sammelt, die zusätzliche Dateien bereitgestellt werden. Ein Ziel besteht aus einer oder mehrerer Aufgaben, die ausgeführt wird, MSBuild auf der Grundlage von Bedingungen, die Sie angeben.

    Die `Include` Attribut gibt an, dass der Ordner, in dem die Dateien zu finden ist *ExtraFiles*befindet sich auf der gleichen Ebene wie den Projektordner. MSBuild sammelt alle Dateien aus diesem Ordner und rekursiv alle Unterordner (doppelte Sternchen gibt rekursive Unterordner). Mit dem folgenden Code können Sie mehrere Dateien und Dateien ablegen, Unterordner der *ExtraFiles* Ordner und alle bereitgestellt werden.

    Die `DestinationRelativePath` Element gibt an, dass die Ordner und Dateien in den Stammordner der Zielwebsite in der gleichen Datei und die Ordnerstruktur kopiert werden sollen, wie sie in gefunden werden die *ExtraFiles* Ordner. Mussten Sie zum Kopieren der *ExtraFiles* Ordner selbst und die `DestinationRelativePath` -Werte entsprächen *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*.
3. Am Ende der Datei vor der schließenden `</Project>` kennzeichnen, fügen Sie das folgende Markup, das beim Ausführen der neuen Ziels angibt.

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    Dieser Code bewirkt, dass die neue `CustomCollectFiles` Ziel ausgeführt werden, wenn das Ziel, die Dateien in den Zielordner kopiert ausgeführt wird. Es ist ein separater Ziel für Veröffentlichen im Vergleich zu paketerstellung für die Bereitstellung und das neue Ziel wird in beide Ziele eingefügt, für den Fall, dass Sie durch die Veröffentlichung ein Bereitstellungspakets anstelle bereitstellen möchten.

    Die *pubxml* Datei sieht nun wie im folgenden Beispiel:

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. Speichern und schließen Sie die *Staging.pubxml* Datei.

## <a name="publish-to-staging"></a>In die Stagingumgebung zu veröffentlichen

Mit nur einem Klick veröffentlichen oder der Befehlszeile Veröffentlichen der Anwendung über das Staging-Profil.

Bei Verwendung von nur einem Klick zu veröffentlichen, können Sie überprüfen, in der **Vorschau** Fenster, *"robots.txt"* kopiert werden. Verwenden Sie andernfalls Ihre FTP-Tool, um zu überprüfen, ob die *"robots.txt"* Datei im Stammordner der Website ist, nach der Bereitstellung.

## <a name="summary"></a>Zusammenfassung

Dies schließt diese Reihe von Lernprogrammen zum Bereitstellen einer ASP.NET-WEBANWENDUNG zwischen einem Hostinganbieter eines Drittanbieters. Weitere Informationen zu den Themen in diesen Lernprogrammen behandelt, finden Sie unter der [ASP.NET Deployment Content Map](https://go.microsoft.com/fwlink/p/?LinkId=282413).

## <a name="more-information"></a>Weitere Informationen

Wenn Sie zum Arbeiten mit Dateien von MSBuild kennen, können Sie viele andere Bereitstellungsaufgaben durch Schreiben von Code in automatisieren *pubxml* -Dateien (für spezifische Aufgaben) oder das Projekt *. wpp.targets* Datei (für tasks, gelten Sie für alle Profile). Weitere Informationen zu *.pubxml* und *. wpp.targets* finden Sie unter [wie: Bearbeiten von Bereitstellungseinstellungen im Veröffentlichungsprofil (.pubxml)-Dateien und die. wpp.targets Datei in Visual Studio Web Projekte](https://msdn.microsoft.com/library/ff398069). Eine grundlegende Einführung in MSBuild-Code finden Sie unter **der Aufbau einer Projektdatei** in [Reihe für Enterprise-Bereitstellung: Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Arbeiten mit Dateien von MSBuild zum Ausführen von Aufgaben für Ihre eigenen Szenarien finden Sie unter diesem Buch: [innerhalb der Microsoft Build Engine: Verwenden von MSBuild und Team Foundation Build](http://msbuildbook.com) Sayed Ibraham Hashimi und William Bartholomew.

## <a name="acknowledgements"></a>Bestätigungen

Ich möchte die vielen Dank, dass die folgenden Personen, die bedeutende Beiträge auf den Inhalt dieser Reihe von Lernprogrammen vorgenommen:

- [Alberto Poblacion, MVP &amp; MCT, Spanien](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson, Plattform die Entwicklung von Data-MVP, USA
- Harsh Mittal, Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson, Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike PPPoE, Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
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
