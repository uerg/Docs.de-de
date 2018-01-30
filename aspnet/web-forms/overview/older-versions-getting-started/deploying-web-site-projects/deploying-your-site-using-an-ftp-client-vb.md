---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
title: Bereitstellen Ihrer Website mithilfe eines FTP-Clients (VB) | Microsoft Docs
author: rick-anderson
description: Die einfachste Methode zum Bereitstellen einer ASP.NET-Anwendung werden die erforderlichen Dateien manuell aus der Entwicklungsumgebung in der produktionsumgebung kopieren. Vorhanden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 09279194-bcf9-4b59-a09d-c68e5926a758
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-an-ftp-client-vb
msc.type: authoredcontent
ms.openlocfilehash: 7792891aed6f0c5e952018dacb36a1d267cb6ae0
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
---
<a name="deploying-your-site-using-an-ftp-client-vb"></a>Bereitstellen Ihrer Website mithilfe eines FTP-Clients (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_03_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial03_DeployingViaFTP_vb.pdf)

> Die einfachste Methode zum Bereitstellen einer ASP.NET-Anwendung werden die erforderlichen Dateien manuell aus der Entwicklungsumgebung in der produktionsumgebung kopieren. Dieses Lernprogramm veranschaulicht, wie einen FTP-Client zu verwenden, um die Dateien von Ihrem Desktop auf dem Webhostinganbieter abzurufen.


## <a name="introduction"></a>Einführung

Das vorherige Lernprogramm eingeführt, eine einfache Buch Review ASP.NET-Webanwendung, der von einigen wenigen ASP.NET-Seiten, einer Masterseite, eine benutzerdefinierte Basis besteht `Page` Klasse, einer Reihe von Bildern, und drei CSS-Stylesheets. Wir können nun zur Bereitstellung von dieser Anwendung auf einen Webhostinganbieter an diesem Punkt die Anwendung für Personen, die mit einer Verbindung mit dem Internet zugegriffen werden kann kann.


Aus unserem Diskussionen in der [ *bestimmen was-Dateien müssen bereitgestellt werden* ](determining-what-files-need-to-be-deployed-vb.md) Tutorial, wir wissen, welche Dateien auf dem Webhostinganbieter kopiert werden müssen. (Beachten Sie, welche Dateien kopiert werden abhängig, ob die Anwendung explizit oder automatisch kompiliert wird.) Aber wie erhalten wir die Dateien aus der Entwicklungsumgebung (unsere Desktop) bis zu der produktionsumgebung (dem Webserver, von dem Webhostinganbieter verwaltet wird)? Die [ **F** Ile **T** Ransfer **P** Rotocol (FTP)](http://en.wikipedia.org/wiki/File_Transfer_Protocol) ist ein häufig verwendetes Protokoll zum Kopieren von Dateien auf einem Computer über ein Netzwerk. Eine weitere Option besteht FrontPage Server Extensions (FPSE). Dieses Lernprogramm konzentriert sich auf eigenständigen FTP-Client-Software verwenden, die erforderlichen Dateien in der Entwicklungsumgebung in der produktionsumgebung bereitstellen.

> [!NOTE]
> Visual Studio enthält Tools zum Veröffentlichen von Websites über FTP; Diese Tools sowie einen Blick auf die Tools, mit denen FPSE, fallen in den nächsten Lernprogrammen.


So kopieren Sie die Dateien über FTP wir müssen ein *FTP-Client* in der Entwicklungsumgebung. Ein FTP-Client ist eine Anwendung, die beim Kopieren von Dateien aus dem Computer, die es installiert wird, für einen Computer mit einem *FTP-Server*. (Wenn Ihre Webhostinganbieter Übertragung von Dateien über FTP unterstützt, wie die meisten, besteht eine FTP-Server, die auf ihre Webserver ausgeführt wird.) Es gibt diverse der FTP-Clientanwendungen verfügbar. Ihr Browser kann sogar double als FTP-Client. Meine bevorzugte FTP-Client, und ich für dieses Lernprogramm verwenden ist [FileZilla](http://filezilla-project.org/), eine kostenlose, Open Source-FTP-Client, die für Windows, Linux und Macs verfügbar ist. Alle FTP-Client funktionieren, jedoch daher gerne verwenden Sie den Client, die Sie am häufigsten mit vertraut sind.

Wenn Sie zusammen Sie vorgehen werden zum Erstellen eines Kontos mit einem Webhostinganbieter, bevor Sie dieses Lernprogramm oder nachfolgender Felder abgeschlossen können müssen. Wie im vorherigen Lernprogramm erwähnt, sind eine Reihe von Web Host Anbieter Unternehmen mit einer großen Bandbreite von Preise, Funktionen und Quality of Service. Für dieses Lernprogramm Reihe ich mithilfe [Discount ASP.NET](http://discountasp.net) als meine Webhost-Anbieter, aber Sie können folgen alle Webhostinganbieter solange sie die Version von ASP.NET unterstützen, Ihre Website in entwickelt wird. (Diese Lernprogramme wurden mithilfe von ASP.NET 3.5 erstellt.) Darüber hinaus, da wir die Dateien auf dem Webhostinganbieter Kopieren unbedingt mithilfe von FTP in diesem Lernprogramm aus, und in Zukunft diejenigen Ihrer Webhostinganbieter FTP-Zugriff auf ihre Webserver unterstützt. Nahezu alle Web Host-Anbieter bieten diese Funktion, aber Sie sollten überprüfen, vor einer Registrierung.

## <a name="deploying-the-book-review-web-application-project"></a>Das Buch Review-Webanwendungsprojekt bereitstellen

Beachten Sie, dass es zwei Versionen der Webanwendung Book-Überprüfung gibt: eine mit das Webanwendungsprojekt-Modell (BookReviewsWAP) und der andere mit dem Websiteprojekt-Modell (BookReviewsWSP) implementiert. Der Projekttyp wirkt sich darauf aus, ob der Standort automatisch oder explizit kompiliert wird, und diese Kompilierungsmodell bestimmt, welche Dateien bereitgestellt werden müssen. Folglich werden untersucht die Projekte BookReviewsWAP und BookReviewsWSP separat Bereitstellen der BookReviewsWAP ab. Nehmen Sie einen Moment Zeit, um diese beiden ASP.NET-Anwendungen herunterladen, wenn Sie nicht bereits getan haben.

Starten Sie das Projekt BookReviewsWAP durch Navigieren zu den `BookReviewsWAP` Ordner und doppelklicken Sie auf die `BookReviewsWAP.sln` Datei. Vor der Bereitstellung des Projekts ist es wichtig, zu erstellen, um sicherzustellen, dass alle Änderungen an den Quellcode in der kompilierten Assembly enthalten sind. Zum Erstellen des Projekts im Menü erstellen werden, und wählen Sie die Menüoption BookReviewsWAP erstellen. Dies kompiliert den Quellcode in das Projekt in einer einzelnen Assembly `BookReviewsWAP.dll`, die befindet sich der `Bin` Ordner.

Wir können jetzt die notwendigen Dateien bereitstellen. Starten Sie Ihre FTP-Client, und Herstellen einer Verbindung mit dem Webserver mit Ihrem Webhostinganbieter. (Wenn Sie sich mit einem Webhostinganbieter registrieren sie werden eine e-Mail an Sie Informationen zum Herstellen einer Verbindung mit dem FTP-Server; Dies schließt die Adresse für den FTP-Server als auch einen Benutzernamen und ein Kennwort).

Kopieren Sie die folgenden Dateien von Ihrem Desktop in den Stammordner der Website an Ihre Webhostinganbieter. Wenn Sie FTP in den Webserver an die Web-Anbieter gehostet werden Sie wahrscheinlich im Stammverzeichnis der Website. Allerdings müssen einige Web Hosten von Anbietern einen Unterordner mit dem Namen `www` oder `wwwroot` , die als Stammordner für Ihre Websitedateien dient. Schließlich bei FTPing die Dateien müssen Sie möglicherweise beim Erstellen der entsprechenden Ordnerstruktur in der produktionsumgebung - der `Bin` Ordner, der `Fiction` Ordner, der `Images` Ordner und So weiter.

- `~/Default.aspx`
- `~/About.aspx`
- `~/Site.master`
- `~/Web.config`
- `~/Web.sitemap`
- Den gesamten Inhalt des der `Styles` Ordner
- Der vollständige Inhalt der der `Images` Ordner (und dessen Unterordner `BookCovers`)
- `~/Fiction/Default.aspx`
- `~/Fiction/Blaze.aspx`
- `~/Tech/Default.aspx`
- `~/Tech/CYOW.aspx`
- `~/Tech/TYASP35.aspx`
- `~/Bin/BookReviewsWAP.dll`

Abbildung 1 zeigt FileZilla aus, nachdem die erforderlichen Dateien kopiert wurden. FileZilla werden die Dateien auf dem lokalen Computer auf der linken Seite und die Dateien auf dem Remotecomputer, auf der rechten Seite angezeigt. Wie in Abbildung 1, die ASP.NET-Quellcodedateien, z. B. gezeigt `About.aspx.vb`, auf dem lokalen Computer (der Entwicklungsumgebung) sind, jedoch wurden nicht auf dem Webhostinganbieter (produktionsumgebung) kopiert, da Codedateien nicht bereitgestellt werden, wenn verwenden müssen explizite Kompilierung.

> [!NOTE]
> Es gibt keinen Schaden weist die Quellcodedateien auf dem Produktionsserver auf, da sie ignoriert werden. ASP.NET verbietet HTTP-Anforderungen an Quellcodedateien standardmäßig, sodass auch, wenn die Quellcodedateien auf dem Produktionsserver vorhanden sind sie nicht zugegriffen werden kann, um die Besucher Ihrer Website. (D. h., wenn ein Benutzer versucht, besuchen `http://www.yoursite.com/Default.aspx.vb` erhalten sie einer Fehlerseite, aus der hervorgeht, die diese Dateitypen - `.vb` Dateien - sind unzulässig.)


[![Verwenden Sie einen FTP-Client, um die erforderlichen Dateien von Ihrem Desktop an den Webserver auf dem Webhostinganbieter zu kopieren.](deploying-your-site-using-an-ftp-client-vb/_static/image2.png)](deploying-your-site-using-an-ftp-client-vb/_static/image1.png)

**Abbildung 1**: ein FTP-Client verwenden, um die erforderlichen Dateien von Ihrem Desktop aus an den Webserver auf dem Webhostinganbieter zu kopieren ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-your-site-using-an-ftp-client-vb/_static/image3.png))


Nehmen Sie einen Moment Zeit, um den Standort zu testen, nach der Bereitstellung Ihrer Website. Wenn Sie einen Domänennamen erworben und die DNS-Einstellungen konfiguriert ordnungsgemäß, finden Sie Ihre Site durch Ihren Domänennamen eingeben. Alternativ können Sie Ihre Webhostinganbieter darf haben bereitgestellt Sie mit einer URL zu Ihrer Website, die sieht etwa wie *Accountname*. *Webhostprovider*".com" oder *Webhostprovider*".com" /*Accountname*. Die URL für mein Konto auf Discount ASP.NET ist beispielsweise: `http://httpruntime.web703.discountasp.net`.

Abbildung 2 zeigt die bereitgestellte Buch Reviews-Website. Beachten Sie, dass ich es auf ASP Discount anzeigen möchte. NET Servern, am `http://httpruntime.web703.discountasp.net`. Zu diesem Zeitpunkt konnte jeder Benutzer mit einer Verbindung mit dem Internet Meine Website anzeigen. Wie wir erwarten, wird der Standort sucht und verhält sich ebenso wie es in der Entwicklungsumgebung testen.

> [!NOTE]
> Wenn Sie eine Fehlermeldung erhalten, wenn Ihre Anwendung anzeigen nehmen einen Moment Zeit, um sicherzustellen, dass bereitgestellt Sie den korrekten Satz von Dateien. Als Nächstes überprüfen Sie die Fehlermeldung, um festzustellen, ob es zeigen, dass keine Anhaltspunkte, um das Problem. Danach können Ihre Web-Host des Unternehmens Helpdesk zu aktivieren oder Posten Sie Ihre Frage im entsprechenden Forum unter der [ASP.NET Foren](https://forums.asp.net/).


[![Das Buch Reviews Standort ist für Personen, die mit einer Internetverbindung jetzt zugegriffen werden kann.](deploying-your-site-using-an-ftp-client-vb/_static/image5.png)](deploying-your-site-using-an-ftp-client-vb/_static/image4.png)

**Abbildung 2**: das Buch Reviews Standort ist für Personen, die mit einer Internetverbindung jetzt zugegriffen werden kann ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-your-site-using-an-ftp-client-vb/_static/image6.png))


## <a name="deploying-the-book-review-web-site-project"></a>Das Buch Review Website-Projekt bereitstellen

Wenn Sie eine ASP.NET-Anwendung bereitstellen, die automatische Kompilierung, z. B. das Websiteprojekt BookReviewsWSP verwendet besteht keine kompilierte Assembly in die `Bin` Ordner. Daher müssen die Webanwendung Quellcodedateien in der produktionsumgebung bereitgestellt werden. Wir führen Sie durch diesen Prozess.

Wie bei das Webanwendungsprojekt es ratsam, erste Build der Anwendung ist vor der Bereitstellung. Beim Erstellen eines Projekts für die Website keine Assembly erstellt wird, überprüft es auf alle Kompilierungsfehler auf der Seite. Diese Fehler gesucht werden jetzt zu einer besseren statt einen Besucher Ihrer Website müssen sie für Sie ermitteln!

Nachdem Sie das Projekt erfolgreich erstellt haben, verwenden Sie die FTP-Client so kopieren Sie die folgenden Dateien in den Stammordner der Website an Ihre Webhostinganbieter. Möglicherweise müssen Sie die entsprechende Ordnerstruktur in der produktionsumgebung zu erstellen.

> [!NOTE]
> Wenn Sie das Projekt jedoch weiterhin versuchen, beim Bereitstellen des Projekts BookReviewsWSP möchten BookReviewsWAP bereits bereitgestellt, zuerst löschen Sie aller Dateien auf dem Webserver, die bei der Bereitstellung von BookReviewsWAP hochgeladen wurden, und klicken Sie dann bereitstellen Sie die Dateien für BookReviewsWSP.


- `~/Default.aspx`
- `~/Default.aspx.vb`
- `~/About.aspx`
- `~/About.aspx.vb`
- `~/Site.master`
- `~/Site.master.vb`
- `~/Web.config`
- `~/Web.sitemap`
- Den gesamten Inhalt des der `Styles` Ordner
- Der vollständige Inhalt der der `Images` Ordner (und dessen Unterordner `BookCovers`)
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

Abbildung 3 zeigt FileZilla nach dem Kopieren von Dateien erforderlich sind. Wie Sie sehen können, die ASP.NET Quellcodedateien, z. B. `About.aspx.vb`, auf dem lokalen Computer (der Entwicklungsumgebung) und dem Webhostinganbieter (produktionsumgebung) vorhanden sind, da Codedateien werden, wenn automatische bereitgestellt müssen Kompilierung.


[![Verwenden Sie einen FTP-Client so kopieren Sie die erforderlichen Dateien von Ihrem Desktop an den Webserver auf dem Webhostinganbieter](deploying-your-site-using-an-ftp-client-vb/_static/image8.png)](deploying-your-site-using-an-ftp-client-vb/_static/image7.png)

**Abbildung 3**: ein FTP-Client verwenden, um die erforderlichen Dateien von Ihrem Desktop aus an den Webserver auf dem Webhostinganbieter zu kopieren ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-your-site-using-an-ftp-client-vb/_static/image9.png))


Erforderlichen Erfahrungsgrad des Benutzers wird durch die Anwendung Kompilierungsmodell nicht beeinflusst. Die gleichen ASP.NET-Seiten zugegriffen werden und Aussehen und Verhalten sich identisch, ob die Website über das Webanwendungsprojekt-Modell oder das Websiteprojekt-Modell erstellt wurde.

## <a name="updating-a-web-application-on-production"></a>Aktualisieren eine Webanwendung für die Produktion

Web-Anwendungsentwicklung und Bereitstellung sind kein Einmaliger Vorgang. Z. B. beim Erstellen der Website Buch Review ich erstellt die verschiedenen Seiten und den zugehörigen Code auf meinem Computer Personal (Entwicklungsumgebung) geschrieben. Nach Erreichen von einem bestimmten stabilen Zustand befinden, bereitgestellt ich meine Anwendung, damit andere Benutzer der Website und Meine Berichte lesen konnte. Aber Bereitstellung markiert das Ende des Meine Entwicklung für diese Website nicht. Ich kann weitere Buch Reviews hinzufügen oder neue Funktionen, z. B. schreibbarkeit Meine Besucher Rate Bücher zu implementieren, oder lassen Sie ihre eigenen Kommentare. Solche Erweiterungen würde entwickelt werden, in der Entwicklungsumgebung und abschließend würden bereitgestellt werden müssen. Entwicklung und Bereitstellung, sind deshalb zyklisch. Sie eine Anwendung entwickeln und bereitstellen. Während die Website aktiv ist und in der Produktion, neue Funktionen hinzugefügt, und behoben werden im Laufe der Zeit die aktualisiert werden, die Anwendung erneut bereitzustellen. Und so weiter und so weiter.

Wie zu erwarten, wenn eine Webanwendung erneut bereitstellen, die Sie nur neue und geänderte Dateien zu kopieren müssen. Besteht keine Notwendigkeit, die nicht geänderten Seiten erneut bereitstellen oder Server oder die clientseitige Unterstützungsdateien (es gibt zwar keinen Schaden in diesem Fall).

> [!NOTE]
> Eins zu bedenken, wenn explizite Kompilierung ist, dass jedes Mal, wenn Sie dem Projekt eine neue ASP.NET-Seite hinzufügen oder Code-bezogene Änderungen vorzunehmen, müssen Sie das Projekt erneut erstellen, der aktualisiert wird, die Assembly in den `Bin` Ordner. Daher müssen Sie diese aktualisierte Assembly bis hin zur Produktion zu kopieren, wenn eine Webanwendung für die Produktion (zusammen mit den anderen neuen und aktualisierten Inhalt) zu aktualisieren.


Müssen zudem wissen, dass alle Änderungen an der `Web.config` oder die Dateien in den `Bin` Directory beendet und startet der Website-Anwendungspool neu. Wenn des Sitzungsstatus gespeichert ist, mit der `InProc` Modus (Standard) und Besucher Ihrer Website verlieren ihre Sitzungsstatus bei einer Änderung dieser Schlüsseldateien sind. Um dieses Fehlers zu vermeiden, sollten Sie die speichern-Sitzung unter Verwendung der `StateServer` oder `SQLServer` Modi. Weitere Informationen zu diesem Thema finden Sie unter [Sitzungszustandsmodus](https://msdn.microsoft.com/library/ms178586.aspx).

Schließlich Bedenken Sie, mit denen erneut bereitstellen einer Anwendung an einer beliebigen Stelle kann von ein paar Sekunden mehrere Minuten je nach Anzahl und Größe der Dateien, die in der produktionsumgebung kopiert werden müssen. Während dieses Zeitraums können Benutzer, die Ihre Site besuchen Fehler oder ungewöhnliches Verhalten auftreten. Sie können "deaktivieren" die gesamte Anwendung durch Hinzufügen einer Seite mit dem Namen `App_Offline.htm` zum Stammverzeichnis Ihrer Anwendung, die Ihre Benutzer erläutert, dass die Website nicht verfügbar für Wartung (oder alle von Ihnen gewünschten ist) und wird sichern wird in Kürze sein. Wenn die `App_Offline.htm` Datei vorhanden ist, den die ASP.NET-Laufzeit leitet alle eingehende Anforderungen zu dieser Seite.

## <a name="summary"></a>Zusammenfassung

Bereitstellen einer Webanwendung umfasst das Kopieren der erforderlichen Dateien aus der Entwicklungsumgebung in der produktionsumgebung. Die häufigste Methode, die durch die Dateien über ein Netzwerk übertragen werden, wird das Protokoll FTP (File Transfer), und die meisten Web Hostanbieter unterstützen FTP-Zugriff auf ihre Webserver. In diesem Lernprogramm wurde erläutert, wie einen FTP-Client verwenden, um die benötigten Dateien auf dem Webserver bereitstellen. Nach der Bereitstellung kann die Website von einem Benutzer mit einer Verbindung mit dem Internet besucht werden!

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [App\_Offline.htm und das Umgehen von der Funktion "Internet Explorer-freundliche Errors"](https://weblogs.asp.net/scottgu/App_5F00_Offline.htm-and-working-around-the-_2200_IE-Friendly-Errors_2200_-feature)
- [Sitzungsstatus Modi](https://msdn.microsoft.com/library/ms178586.aspx)

>[!div class="step-by-step"]
[Zurück](determining-what-files-need-to-be-deployed-vb.md)
[Weiter](deploying-your-site-using-visual-studio-vb.md)
