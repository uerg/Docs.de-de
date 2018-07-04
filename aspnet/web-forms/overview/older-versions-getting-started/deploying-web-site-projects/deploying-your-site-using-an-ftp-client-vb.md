---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
title: Bereitstellen einer Website mithilfe eines FTP-Clients (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Die einfachste Möglichkeit zum Bereitstellen einer ASP.NET-Anwendung ist, kopieren die erforderlichen Dateien manuell aus der Entwicklungsumgebung in der produktionsumgebung bereit. Auf Ihrer...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 09279194-bcf9-4b59-a09d-c68e5926a758
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
msc.type: authoredcontent
ms.openlocfilehash: e886e07309654f34c7890f1e3ac79c7732a2e015
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367760"
---
<a name="deploying-your-site-using-an-ftp-client-vb"></a>Bereitstellen einer Website mithilfe eines FTP-Clients (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_vb.pdf)

> Die einfachste Möglichkeit zum Bereitstellen einer ASP.NET-Anwendung ist, kopieren die erforderlichen Dateien manuell aus der Entwicklungsumgebung in der produktionsumgebung bereit. In diesem Tutorial veranschaulicht, wie einen FTP-Client zu verwenden, um die Dateien auf dem Desktop auf dem Webhostinganbieter abzurufen.


## <a name="introduction"></a>Einführung

Das vorherige Lernprogramm eingeführt, eine einfache Buch lesen Sie ASP.NET-Webanwendung, die eine Reihe von ASP.NET-Seiten, eine Masterseite, eine benutzerdefinierte Basis besteht `Page` Klasse, durch eine Anzahl von Bildern, und drei CSS-Stylesheets. Wir können nun zum Bereitstellen der Anwendung auf einen Webhostinganbieter an diesem Punkt die Anwendung für alle Benutzer mit einer Verbindung mit dem Internet zugegriffen werden kann kann.


In unseren Gesprächen in der [ *bestimmen was Dateien müssen bereitgestellt werden* ](determining-what-files-need-to-be-deployed-vb.md) Tutorial, wir wissen, welche Dateien auf dem Webhostinganbieter kopiert werden müssen. (Denken Sie daran, dass abhängt, welche Dateien kopiert werden, ob die Anwendung explizit oder automatisch kompiliert wird.) Aber wie bekommen wir die Dateien aus der Entwicklungsumgebung (in unserem Desktop) bis zu der produktionsumgebung (dem Webserver, von dem Webhostinganbieter verwaltet werden)? Die [ **F** Ile **T** Ransfer **P** Rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) ist ein häufig verwendetes Protokoll zum Kopieren von Dateien von einem Computer auf einen anderen über ein Netzwerk. Eine andere Möglichkeit besteht, FrontPage Server Extensions (FPSE). Dieses Tutorial konzentriert sich zur Verwendung von eigenständigen FTP-Client-Software, die erforderlichen Dateien aus der Entwicklungsumgebung in der produktionsumgebung bereitstellen.

> [!NOTE]
> Visual Studio enthält Tools zum Veröffentlichen von Websites über FTP; Diese Tools als auch einen Blick auf die Tools, mit denen FPSE, werden im nächsten Tutorial behandelt.


Kopieren Sie die Dateien mithilfe von FTP, die wir benötigen eine *FTP-Client* in der Entwicklungsumgebung. FTP-Client ist eine Anwendung, die entwickelt wird, um Dateien vom Computer zu kopieren, es installiert ist, auf einem Computer, auf denen ausgeführt wird, wird, ein *FTP-Server*. (Wenn Ihre Webhostinganbieter Übertragung von Dateien über FTP unterstützt, wie die meisten der Fall ist, dann besteht ein FTP-Server, die auf ihren Webservern ausführen.) Eine Reihe von FTP-Client-Anwendungen stehen zur Verfügung. Webbrowser kann sogar double als FTP-Client. Ist Mein bevorzugtes FTP-Client und die ich in diesem Tutorial verwenden [FileZilla](http://filezilla-project.org/), eine kostenlose, Open-Source-FTP-Client, der für Windows, Linux und Macintosh-Computer verfügbar ist. Alle FTP-Client funktioniert, allerdings also gerne verwenden Sie den Client mit am besten vertraut sind.

Wenn Sie auf die Sie befolgen werden zum Erstellen eines Kontos mit einem Webhostinganbieter aus, bevor Sie in diesem Tutorial oder alle weiteren abschließen kann benötigen. Wie im vorherigen Tutorial erwähnt, stehen eine Reihe mit einem breiten Spektrum von Preise, Funktionen und Quality of Service, Web-Host-Anbieter-Unternehmen. Für diese tutorialreihe ich verwenden [Discount ASP.NET](http://discountasp.net) als meine Webhost-Anbieter, aber Sie können nutzen alle Webhostinganbieter, solange sie die ASP.NET-Version unterstützen, Ihre Website wurde in entwickelt. (Die in diesen Tutorials wurden mithilfe von ASP.NET 3.5 erstellt.) Darüber hinaus, da wir die Dateien auf dem Webhostinganbieter kopiert werden unbedingt mithilfe von FTP in diesem Tutorial aus, und in Zukunft Anwendungen Ihre Webhostinganbieter FTP-Zugriff auf ihren Webservern unterstützt. Fast alle Anbieter von Host bieten diese Funktion, aber Sie sollten überprüfen, ehe Sie sich anmelden.

## <a name="deploying-the-book-review-web-application-project"></a>Das Buch Review-Webanwendungsprojekt bereitstellen

Denken Sie daran, dass es zwei Versionen der Webanwendung Buchbewertung gibt: eine über das Webanwendungsprojekt-Modell (BookReviewsWAP) und der andere mit dem Websiteprojekt-Modell (BookReviewsWSP) implementiert. Der Projekttyp wirkt sich darauf aus, ob der Standort, automatisch oder explizit kompiliert wird, und das Kompilierungsmodell bestimmt, welche Dateien bereitgestellt werden müssen. Daher untersuchen wir die Projekte BookReviewsWAP und BookReviewsWSP separat Bereitstellen der BookReviewsWAP ab. Nehmen Sie einen Moment Zeit, um diese zwei ASP.NET-Anwendungen herunterladen, wenn Sie nicht bereits getan haben.

Starten Sie das Projekt BookReviewsWAP durch Navigieren zu den `BookReviewsWAP` Ordner und doppelklicken auf die `BookReviewsWAP.sln` Datei. Vor der Bereitstellung des Projekts ist es wichtig, zu erstellen, um sicherzustellen, dass alle Änderungen am Quellcode in der kompilierten Assembly enthalten sind. Zum Erstellen des Projekts finden Sie unter dem Menü "Build", und wählen Sie die Menüoption BookReviewsWAP erstellen. Dies kompiliert den Quellcode in das Projekt, in eine einzelne Assembly, `BookReviewsWAP.dll`, befindet sich der `Bin` Ordner.

Wir können nun die erforderlichen Dateien bereitstellen. FTP-Client zu starten, und Verbinden mit dem Webserver unter Ihrer Webhostinganbieter. (Bei der Registrierung mit einem Webhostinganbieter sie werden per e-Mail senden Sie Informationen zum Herstellen einer Verbindung mit dem FTP-Server; dazu gehören die Adresse für den FTP-Server als auch einen Benutzernamen und Kennwort).

Kopieren Sie die folgenden Dateien in den Stammordner der Website an Ihre Web-Host-Anbieter auf dem Desktop. Wenn Sie FTP in den Webserver an die Web-Anbieter gehostet werden Sie wahrscheinlich im Stammverzeichnis der Website. Allerdings müssen einige Anbieter von Host einen Unterordner namens `www` oder `wwwroot` , dient als Stammordner für Ihre Websitedateien. Schließlich Wenn FTPing die Dateien unter Umständen müssen Sie die entsprechende Ordnerstruktur in der produktionsumgebung - erstellen die `Bin` Ordner die `Fiction` Ordner die `Images` Ordner, und So weiter.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Der vollständige Inhalt der dem `Styles` Ordner
- Der vollständige Inhalt der dem `Images` Ordner (und dessen Unterordner, `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

Abbildung 1 zeigt die FileZilla, nachdem die erforderlichen Dateien kopiert wurden. FileZilla zeigt die Dateien auf dem lokalen Computer, auf der linken Seite und die Dateien auf dem Remotecomputer, auf der rechten Seite. Wie in Abbildung 1, die ASP.NET-Quellcodedateien, wie z. B. dargestellt `About.aspx.vb`, auf dem lokalen Computer (Entwicklungsumgebung) sind, jedoch wurden nicht auf dem Webhostinganbieter (in der produktionsumgebung) kopiert, da Codedateien nicht bereitgestellt werden, wenn Sie verwenden müssen explizite Kompilierung.

> [!NOTE]
> Es gibt keinen Schaden an, dass die Quellcodedateien auf dem Produktionsserver, wie sie ignoriert werden. ASP.NET verbietet die HTTP-Anforderungen an die Quellcodedateien standardmäßig so, dass auch wenn die Quellcodedateien auf dem Produktionsserver vorhanden sind die Besucher Ihrer Website zugegriffen werden. (D. h., wenn ein Benutzer versucht, besuchen `http://www.yoursite.com/Default.aspx.vb` sie erhalten einer Fehlerseite angezeigt, aus der hervorgeht, die diese Arten von Dateien – `.vb` Dateien – sind unzulässig.)


[![Verwenden Sie einen FTP-Client, um die erforderlichen Dateien auf dem Desktop an den Webserver, auf dem Webhostinganbieter zu kopieren.](deploying-your-site-using-an-ftp-client-vb/_static/image2.png)](deploying-your-site-using-an-ftp-client-vb/_static/image1.png)

**Abbildung 1**: FTP-Client verwenden, um die erforderlichen Dateien von Ihrem Desktop aus an den Webserver, auf dem Webhostinganbieter zu kopieren ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-your-site-using-an-ftp-client-vb/_static/image3.png))


Nach der Bereitstellung Ihrer Website können Sie die Website testen. Wenn Sie einen Domänennamen erworben haben, und die DNS-Einstellungen konfiguriert ordnungsgemäß, können Sie Ihre Website besuchen, durch Ihren Domänennamen eingeben. Sie können auch Ihre Webhostinganbieter sollte haben angegeben Sie mit einer URL zu Ihrer Website, die etwa folgendermaßen aussehen *Accountname*. *Webhostprovider*.com oder *Webhostprovider*.com /*Accountname*. Die URL für mein Konto auf ASP.NET Discount ist z. B.: `http://httpruntime.web703.discountasp.net`.

Abbildung 2 zeigt die bereitgestellte Book Reviews-Website. Beachten Sie, dass ich es zu ASP Discount anzeigen möchte. NET Servern, auf `http://httpruntime.web703.discountasp.net`. Zu diesem Zeitpunkt kann jeder Benutzer mit einer Verbindung mit dem Internet meiner Website anzeigen. Wie wir erwarten, wird die Website sieht aus, und verhält sich genauso wie bei der es sich in der Entwicklungsumgebung testen.

> [!NOTE]
> Wenn Sie beim Anzeigen der Anwendung können Sie sicherstellen, dass ein Fehler auftreten, haben Sie den richtigen Satz von Dateien bereitgestellt. Als Nächstes überprüfen Sie die Fehlermeldung, um festzustellen, ob es keine Anhaltspunkte über das Problem zeigt. Danach können Sie Ihre Web-Host des Unternehmens Helpdesk aktivieren oder Ihre Frage im Forum geeigneten, unter dem [ASP.NET-Foren](https://forums.asp.net/).


[![Die Book Reviews-Website ist für alle Benutzer mit einer Internetverbindung nun zugegriffen werden kann.](deploying-your-site-using-an-ftp-client-vb/_static/image5.png)](deploying-your-site-using-an-ftp-client-vb/_static/image4.png)

**Abbildung 2**: die Book Reviews-Website kann nun zugegriffen werden, für alle Benutzer mit einer Internetverbindung ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-your-site-using-an-ftp-client-vb/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>Das Buch lesen Sie Website-Projekt bereitstellen

Wenn Sie eine ASP.NET-Anwendung bereitstellen, die automatische Kompilierung, z. B. das Websiteprojekt BookReviewsWSP verwendet es gibt keine kompilierte Assembly in den `Bin` Ordner. Daher müssen die Webanwendung Quellcodedateien in der produktionsumgebung bereitgestellt werden. Betrachten Sie diesen Prozess.

Wie Sie mit dem Webprojekt für die Anwendung ist es klug, ersten Build der Anwendung vor der Bereitstellung. Beim Erstellen eines Websiteprojekts keine Assembly erstellt wird, wird auf der Seite Fehler während der Kompilierung überprüft. Mehr, um diese Fehler nun zu suchen, statt Sie müssen einen Besucher Ihrer Website ermitteln sie für Sie!

Sobald Sie das Projekt erfolgreich erstellt haben, verwenden Sie FTP-Client, kopieren Sie die folgenden Dateien in den Stammordner der Website an Ihre Web-Host-Anbieter. Möglicherweise müssen Sie die entsprechende Ordnerstruktur in der produktionsumgebung zu erstellen.

> [!NOTE]
> Wenn Sie das Projekt soll aber dennoch versuchen, beim Bereitstellen des Projekts BookReviewsWSP BookReviewsWAP bereits bereitgestellt, zuerst löschen Sie aller Dateien auf dem Webserver, die bei der Bereitstellung von BookReviewsWAP hochgeladen wurden, und klicken Sie dann bereitstellen Sie die Dateien für BookReviewsWSP.


- `~/Default.aspx`
- `~/Default.aspx.vb`
- `~/About.aspx`
- `~/About.aspx.vb`
- `~/Site.master`
- `~/Site.master.vb`
- `~/Web.config`
- `~/Web.sitemap`
- Der vollständige Inhalt der dem `Styles` Ordner
- Der vollständige Inhalt der dem `Images` Ordner (und dessen Unterordner, `BookCovers`)
- `~/App_Code/BasePage.vb`
- `~/Fiction/Default.aspx`
- `~/Fiction/Default.aspx.vb`
- `~/Fiction/Blaze.aspx`
- `~/Fiction/Blaze.aspx.vb`
- `~/Tech/Default.aspx`
- `~/Tech/Default.aspx.vb`
- `~/Tech/CYOW.aspx`
- `~/Tech/CYOW.aspx.vb`
- `~/Tech/TYASP35.aspx`
- `~/Tech/TYASP35.aspx.vb`

Abbildung 3 zeigt FileZilla, nachdem Sie die benötigten Dateien kopiert haben. Wie Sie sehen können, die ASP.NET Quellcodedateien, z. B. `About.aspx.vb`, auf dem lokalen Computer (Entwicklungsumgebung) und dem Webhostinganbieter (in der produktionsumgebung) vorhanden sind, da Codedateien werden, wenn automatische bereitgestellt müssen Kompilierung.


[![Verwenden Sie einen FTP-Client die notwendigen Dateien auf dem Desktop an den Webserver, auf dem Webhostinganbieter zu kopieren.](deploying-your-site-using-an-ftp-client-vb/_static/image8.png)](deploying-your-site-using-an-ftp-client-vb/_static/image7.png)

**Abbildung 3**: FTP-Client verwenden, um die erforderlichen Dateien von Ihrem Desktop aus an den Webserver, auf dem Webhostinganbieter zu kopieren ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-your-site-using-an-ftp-client-vb/_static/image9.png))


Die benutzerfreundlichkeit ist von der Anwendung Kompilierungsmodell nicht betroffen. Die gleichen ASP.NET-Seiten zugegriffen werden, und ihr Aussehen und Verhalten sich identisch, ob die Website erstellt wurde, verwenden das Webanwendungsprojekt-Modell oder das Websiteprojekt-Modell.

## <a name="updating-a-web-application-on-production"></a>Aktualisieren von einer Webanwendung auf die Produktion

Web-Anwendungsentwicklung und-Bereitstellung sind kein Einmaliger Vorgang. Z. B. beim Erstellen der Website Buchbewertung ich die verschiedenen Seiten erstellt und den zugehörigen Code auf meinem PC (Entwicklungsumgebung) geschrieben. Nach einem bestimmten stabilen Zustand zu erreichen, bereitgestellt ich meine Anwendung, damit andere konnte von der Website, und Lesen Sie Meine Berichte. Aber Bereitstellung markiert das Ende des Meine entwicklungstätigkeit auf dieser Website nicht. Ich kann weitere Buchbesprechungen hinzufügen oder implementieren neue Funktionen wie meine Besucher Rate Bücher zulassen oder eigene Kommentare. Solche Erweiterungen würden entwickelt werden, in der Entwicklungsumgebung und abschließend bereitgestellt werden müssen. Entwicklung und Bereitstellung, sind daher zyklisch. Sie entwickeln eine Anwendung, und klicken Sie dann bereitstellen. Während die Website Live geschaltet ist und in der Produktion ist, werden neue Features hinzugefügt und behobenen Fehler werden im Laufe der Zeit, die erfordert, die Anwendung erneut bereitstellen. Und so weiter und so weiter.

Wie zu beim erneut bereitstellen einer Webanwendung, die Sie nur neue und geänderte Dateien zu kopieren erwarten müssen. Besteht keine Notwendigkeit, die nicht geänderten Seiten erneut bereitstellen, oder Server- oder clientseitiger Unterstützungsdateien (obwohl es keinen Schaden gibt in diesem Fall).

> [!NOTE]
> Dabei ist zu bedenken, wenn explizite Kompilierung ist, dass jedes Mal, wenn Sie eine neue ASP.NET-Seite zum Projekt hinzufügen oder Code-bezogene Änderungen vornehmen, müssen Sie Ihrem Projekt neu erstellen, in dem die Assembly im aktualisiert die `Bin` Ordner. Daher müssen Sie diese aktualisierte Assembly in der produktionsumgebung kopieren, bei der Aktualisierung von einer Webanwendung auf Produktion (zusammen mit den anderen neuen und aktualisierten Inhalt).


Auch verstehen, dass alle Änderungen an der `Web.config` oder Dateien in die `Bin` Directory beendet und neu gestartet wird der Website-Anwendungspools. Wenn des Sitzungszustands gespeichert ist, mit der `InProc` -Modus (Standard) und dann die Besucher der Site verlieren den Sitzungsstatus immer diese wichtigsten Dateien geändert werden. Um dieses Fehlers zu vermeiden, sollten Sie das Speichern der Sitzung mit dem `StateServer` oder `SQLServer` Modi. Weitere Informationen zu diesem Thema lesen [Sitzungszustandsmodus](https://msdn.microsoft.com/library/ms178586.aspx).

Zum Schluss Bedenken Sie, die erneute Bereitstellung einer Anwendungs kann zwischen ein paar Sekunden und dauern einige Minuten, je nach Anzahl und Größe der Dateien, die in der produktionsumgebung kopiert werden müssen. Während dieses Zeitraums können Benutzer Ihre Website besuchen Fehler oder Verhalten auftreten. Sie können "deaktivieren" die gesamte Anwendung durch Hinzufügen einer Seite mit dem Namen `App_Offline.htm` Root-Verzeichnis der Anwendung, die Ihren Benutzern erklärt, dass der Standort nicht verfügbar für die Wartung (oder was auch immer ist) und sein Sichern in Kürze. Wenn die `App_Offline.htm` Datei vorhanden ist, wird die ASP.NET-Laufzeit alle eingehende Anforderungen auf dieser Seite umgeleitet.

## <a name="summary"></a>Zusammenfassung

Bereitstellen einer Webanwendung umfasst das Kopieren der erforderlichen Dateien aus der Entwicklungsumgebung in der produktionsumgebung bereit. Die häufigste Methode, die mit dem Dateien über ein Netzwerk übertragen werden, wird die FTP File Transfer Protocol (), und die meisten Web-Host-Anbieter unterstützt die FTP-Zugriff auf ihren Webservern. In diesem Tutorial wurde erläutert, wie einen FTP-Client zu verwenden, um die benötigten Dateien auf dem Webserver bereitstellen. Nach der Bereitstellung kann die Website von jedem Benutzer mit einer Verbindung mit dem Internet besucht werden.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [App\_Offline.htm und arbeiten rund um die Funktion "Internet Explorer-freundliche-Fehler"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Sitzungszustandsmodus](https://msdn.microsoft.com/library/ms178586.aspx)

> [!div class="step-by-step"]
> [Zurück](determining-what-files-need-to-be-deployed-vb.md)
> [Weiter](deploying-your-site-using-visual-studio-vb.md)
