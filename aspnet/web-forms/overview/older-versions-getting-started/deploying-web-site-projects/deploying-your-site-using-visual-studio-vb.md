---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: Bereitstellen Ihrer Website mithilfe von Visual Studio (VB) | Microsoft Docs
author: rick-anderson
description: "Visual Studio enthält Tools zum Bereitstellen einer Websites. Erfahren Sie mehr über diese Tools in diesem Lernprogramm aus."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: 362f8391f3352b3abf00045bca0c212cd850b17f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="deploying-your-site-using-visual-studio-vb"></a>Bereitstellen Ihrer Website mithilfe von Visual Studio (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip) oder [PDF herunterladen](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> Visual Studio enthält Tools zum Bereitstellen einer Websites. Erfahren Sie mehr über diese Tools in diesem Lernprogramm aus.


## <a name="introduction"></a>Einführung

Das vorherigen Lernprogramm erläutert, wie Sie eine einfache ASP.NET-Webanwendung auf einen Webhostinganbieter bereitstellen. Insbesondere das Lernprogramm wurde gezeigt, wie einen FTP-Client wie FileZilla verwenden, um die erforderlichen Dateien in der Entwicklungsumgebung in die produktionsumgebung zu übertragen. Visual Studio bietet außerdem integrierte Tools, um die Bereitstellung in einem Webhostinganbieter zu vereinfachen. In diesem Lernprogramm untersucht werden zwei der folgenden Tools: das Tool "Website kopieren", in dem Sie Dateien verschieben in und aus einem Remotewebserver installiert, über FTP oder FrontPage-Servererweiterungen; das Veröffentlichen Tool und die gesamte Website in einem angegebenen Speicherort kopiert.


> [!NOTE]
> Andere bereitstellungsbezogenen Tools von Visual Studio enthalten [Setup Webprojekte](https://msdn.microsoft.com/library/wx3b589t.aspx) und [Bereitstellung Webprojekte](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) Add-In. Setup Webprojekte Verpacken einer Website Inhalt und Konfigurationsinformationen in eine einzelne MSI-Datei. Diese Option ist besonders nützlich für Websites, die in einem Intranet bereitgestellt werden oder für Unternehmen, die eine vorkonfigurierte Webanwendung verkaufen, die Kunden auf ihre eigenen Web-Server installieren. Das Web Bereitstellung Projekte-Add-In ist eine Visual Studio-Add-In, mit der Angabe Konfigurationsunterschiede zwischen ermöglicht für Entwicklung und produktionsumgebungen erstellt. Websetup-Projekte sind nicht in diesem Lernprogramm Reihe erörtert. Web-Bereitstellungsprojekte werden zusammengefasst, der [ *Common Configuration Unterschiede zwischen Entwicklungs- und Produktionsumgebungen* ](common-configuration-differences-between-development-and-production-vb.md) Lernprogramm.


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Bereitstellen Ihrer Website mithilfe der Kopie Websiteverwaltungs-Tool

Visual Studio-Kopie Websiteverwaltungs-Tool ist Funktionalität ähnelt einem eigenständigen FTP-Client. Kurz gesagt, können mit der Kopie Websiteverwaltungs-Tool Herstellen einer Verbindung mit einer remoten Website über FTP oder FrontPage-Servererweiterungen. Ähnelt FileZillas-Benutzeroberfläche, die Website kopieren-Benutzeroberfläche besteht aus zwei Bereichen: der linke Bereich enthält die lokalen Dateien, während Sie im rechten Bereich diese Dateien auf dem Zielserver aufgeführt.

> [!NOTE]
> Die Kopie Websiteverwaltungs-Tool ist nur für Websiteprojekte verfügbar. Visual Studio bietet dieses Tool aus, bei der Arbeit mit einem Webanwendungsprojekt.


Werfen wir einen Blick auf die mithilfe der Kopie Websiteverwaltungs-Tool die Anwendung Buch überprüfen Sie, bis hin zur Produktion veröffentlichen. Da die Kopie Websiteverwaltungs-Tool nur mit Projekten verwendet werden kann, die das Websiteprojekt-Modell verwenden, das wir nur mit diesem Tool mit dem Projekt BookReviewsWSP untersuchen können. Öffnen Sie das Projekt.

Starten Sie das Tool Kopie Websiteprojekt, indem Sie auf das Symbol "Website kopieren" im Projektmappen-Explorer (dieses Symbol wird in Abbildung 1 mit Kreis); Alternativ können Sie die Option für die Website kopieren aus dem Menü Website auswählen. Beide Vorgehensweisen startet die Website kopieren-Benutzeroberfläche, die in Abbildung 1 gezeigten; linke Bereich in Abbildung 1 wird aufgefüllt, da wir noch über die Verbindung mit einem Remoteserver.


[![Benutzeroberfläche für die Kopie Websiteverwaltungs-Tool ist in zwei Bereiche unterteilt](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**Abbildung 1**: die Kopie Website Datentool-Benutzeroberfläche ist in zwei Bereiche unterteilt ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-your-site-using-visual-studio-vb/_static/image3.png))


Zum Bereitstellen von unserer Website muss zuerst mit dem Webhostinganbieter verbinden. Klicken Sie auf die Schaltfläche "Verbinden" am oberen Rand der Website kopieren-Benutzeroberfläche. Dies zeigt das Dialogfeld "Website öffnen" in Abbildung 2 dargestellt.

Sie können dazu eine der vier Optionen aus der linken Seite an die Zielwebsite verbinden:

- **Dateisystem** -wählen Sie diese Option, um Ihre Website für einen Ordner oder eine Netzwerkfreigabe zugegriffen werden kann von Ihrem Computer bereitstellen.
- **Lokale IIS** -verwenden Sie diese Option zum Bereitstellen der Website mit dem IIS-Webserver auf Ihrem Computer installiert.
- **FTP-Site** -Herstellen einer Verbindung mit einer remoten Website über FTP.
- **Remote-Standortsystemserver** -Herstellen einer Verbindung mit einer remote-Website mithilfe von FrontPage-Servererweiterungen.

Die meisten Web Hostanbieter unterstützen, FTP, aber weniger FrontPage-Servererweiterungen Unterstützung bieten. Aus diesem Grund haben ich die FTP-Site-Option ausgewählt, und klicken Sie dann die Verbindungsinformationen eingegeben, wie in Abbildung 2 dargestellt.


[![Geben Sie den Ziel-Website](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**Abbildung 2**: Geben Sie die Ziel-Website ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-your-site-using-visual-studio-vb/_static/image6.png))


Nachdem Sie eine Verbindung herstellen, lädt die Dateien am Remotestandort im rechten Bereich und gibt den Status der einzelnen Dateien die Kopie Websiteverwaltungs-Tool: neu sind, gelöscht, geändert oder Unchanged. Sie können eine Datei aus dem lokalen Standort zum remote-standortsystemserver oder eine umgekehrt kopieren.

Fügen Sie eine neue Seite auf das Projekt BookReviewsWSP und dann bereitstellen, damit wir das Tool "Website kopieren" in Aktion sehen können. Erstellen Sie eine neue ASP.NET-Seite in Visual Studio in das Stammverzeichnis, das mit dem Namen `Privacy.aspx`. Haben Sie die Seite, die die Masterseite verwenden `Site.master` und Datenschutzrichtlinie des Standorts zu dieser Seite hinzufügen. Abbildung 3 zeigt Visual Studio aus, nachdem dieser Seite erstellt wurde.


[![Hinzufügen einer neuen Seite mit dem Namen &lt;Code&gt;Privacy.aspx&lt;/code&gt; zum Stammordner der Website](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**Abbildung 3**: Hinzufügen einer neuen Seite mit dem Namen `Privacy.aspx` zum Stammordner der Website ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-your-site-using-visual-studio-vb/_static/image9.png))


Kehren Sie zur Benutzeroberfläche Website kopieren. Wie in Abbildung 4 gezeigt, enthält der linken nun die neuen Dateien - `Policy.aspx` und `Policy.aspx.vb`. Diese Dateien sind, mit einem Symbol "Pfeil" und einen Status der neuen, gibt an, dass sie auf dem lokalen Standortserver, jedoch nicht auf der Remotewebsite vorhanden sind gekennzeichnet.


[![Die Kopie Websiteverwaltungs-Tool enthält das neue &lt;Code&gt;Privacy.aspx&lt;/code&gt; Seite im linken Bereich](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**Abbildung 4**: die Kopie Websiteverwaltungs-Tool enthält das neue `Privacy.aspx` Seite im linken Bereich ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-your-site-using-visual-studio-vb/_static/image12.png))


Die neue bereitzustellende Dateien, wählen Sie sie aus, und klicken Sie dann auf das Pfeilsymbol, um sie mit dem Remotestandort zu übertragen. Nach Abschluss die Übertragung der `Policy.aspx` und `Policy.aspx.vb` Dateien vorhanden sind, auf die lokale und remote-Standorte mit dem Status unverändert.

Zusammen mit der neue Dateien auflistet, kennzeichnet die Kopie Websiteverwaltungs-Tool alle Dateien, die zwischen den lokalen und remote-Standorten zu unterscheiden. Um dies in Aktion sehen, zurückgeben, um die `Privacy.aspx` Seite und die Datenschutzrichtlinie einige weitere Wörter hinzuzufügen. Speichern Sie die Seite, und fahren Sie dann mit der Kopie Websiteverwaltungs-Tool. Wie in Abbildung 5 gezeigt, die `Privacy.aspx` Seite im linken Bereich weist den Status geändert, der angibt, dass er mit dem Remotestandort nicht synchron ist.


[![Die Kopie Websiteverwaltungs-Tool gibt an, dass die &lt;Code&gt;Privacy.aspx&lt;/code&gt; Seite geändert wurde](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**Abbildung 5**: die Kopie Websiteverwaltungs-Tool gibt an, dass die `Privacy.aspx` Seite geändert wurde ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-your-site-using-visual-studio-vb/_static/image15.png))


Die Kopie Websiteverwaltungs-Tool ist auch ersichtlich, ob eine Datei seit der letzten Kopiervorgangs gelöscht wurde. Löschen Sie die `Privacy.aspx` aus den lokalen Projekt, und Aktualisieren der Kopie Websiteverwaltungs-Tool. Die `Privacy.aspx` und `Privacy.aspx.vb` Dateien bleiben im linken Bereich aufgelisteten, aber sein gelöschte Status gibt an, dass sie seit der letzten Kopiervorgangs entfernt wurden.

## <a name="publishing-a-web-application"></a>Veröffentlichen einer Webanwendung

Eine andere Möglichkeit, die Webanwendung in Visual Studio bereitstellen, ist die Option "Veröffentlichen" verwendet wird, die über das Menü Build zugänglich ist. Die Option "Veröffentlichen" explizit die Anwendung kompiliert und kopiert dann alle erforderlichen Dateien bis zu dem angegebenen Remotestandort. Wir werden in Kürze sehen, ist die Option "Veröffentlichen" mehr als die Kopie Websiteverwaltungs-Tool stumpfen. Während der Kopie Websiteverwaltungs-Tool Sie die Dateien auf den lokalen und remote-Standorten zu untersuchen können und ermöglicht es Ihnen, hochladen oder Herunterladen von einzelnen Dateien nach Bedarf, wird die Option "Veröffentlichen" die gesamte Webanwendung bereitgestellt.


Zusätzlich zum Kopieren alle erforderlichen Dateien mit dem angegebenen Remotestandort, wird die Option "Veröffentlichen" auch explizit die Anwendung kompiliert. Webanwendungsprojekte werden müssen, muss explizit kompiliert es als keine Überraschung liegen, dass die Option "Veröffentlichen" für Webanwendungsprojekte verfügbar ist. Was sind möglicherweise ein wenig überraschend ist, dass auch die Option "Veröffentlichen" für Website-Projekte verfügbar ist. Wie in notiert die [ *bestimmen was-Dateien müssen bereitgestellt werden* ](determining-what-files-need-to-be-deployed-vb.md) Lernprogramm Websiteprojekte kann explizit kompiliert werden, bis dieser Prozess wird als bezeichnet *Vorkompilierung*. Dieses Lernprogramm konzentriert sich auf Webanwendungsprojekte mit der Option "Veröffentlichen"; ein Lernprogramm zukünftige erforschen Vorkompilierung, an diesem Punkt wir um ansehen, verwenden die Option "Veröffentlichen" mit Websiteprojekte zurückkehren.

> [!NOTE]
> Während die Option "Veröffentlichen" in Visual Studio für Websiteprojekte und Webanwendungsprojekte verfügbar ist, bietet Visual Web Developer nur die Option "Veröffentlichen" für Webanwendungsprojekte.


Betrachten Sie die Buch Reviews-Anwendung mithilfe der Option "Veröffentlichen" bereitgestellt. Starten Sie, indem Sie BookReviewsWAP (das Webanwendungsprojekt) in Visual Studio öffnen. Wählen Sie im Veröffentlichen des Projekts BookReviewsWAP erstellen. Daraufhin wird ein Dialogfeld, das den Zielspeicherort an, neben anderen Konfigurationsoptionen dazu aufgefordert werden (siehe Abbildung 6). Wesentlich können z. B. mit dem Tool Website kopieren Sie einen Speicherort eingeben, der auf einen lokalen Ordner, eine lokale Website auf IIS, einer remote-Website, die FrontPage-Servererweiterungen oder eine FTP-Serveradresse unterstützt. Sie können auswählen, ob die Dateien auf dem remote-Webserver mit den bereitgestellten Dateien zu ersetzen oder löschen alle Inhalte auf der Remotewebsite vor der Veröffentlichung. Sie können auch angeben, ob kopiert:

- Nur die Dateien im Projekt erforderlich, um die Anwendung auszuführen, die den nicht benötigten Quellcode und Dateien projektbezogenen weglässt.
- Alle Projektdateien, die die Quellcodedateien enthält und Visual Studio-Projektdateien, wie die Projektmappendatei.
- Alle Dateien im Quellprojektordner, der kopiert alle Dateien im Projekt Quellordner unabhängig davon, ob sie im Projekt enthalten sind.

Es gibt auch eine Option aus, um den Inhalt Hochladen der `App_Data` Ordner.


[![Geben Sie den Ziel-Website](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**Abbildung 6**: Geben Sie die Ziel-Website ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-your-site-using-visual-studio-vb/_static/image18.png))


Für die Anwendung Buch Review enthält Remotestandort nicht änderbar Dateien bereitgestellt, wenn das BookReviewsWSP Projekt über die Kopie Websiteverwaltungs-Tool zu kopieren. Wir haben deshalb die Option "Veröffentlichen" starten, indem Sie alle vorhandenen Inhalte löschen. Außerdem nehmen wir einfach kopieren Dateien erforderlich sind, anstatt viele Realisierungslinks enthalten die produktionsumgebung nicht benötigte Quelldateien und die Projektdateien. Erst nach dem Ändern dieser Optionen klicken Sie auf die Schaltfläche "Veröffentlichen". Visual Studio stellt über den nächsten Sekunden die erforderlichen Dateien an den Zielstandort bereit ihren Status im Ausgabefenster anzeigen.

Abbildung 7 zeigt die Dateien auf dem FTP-Site an, nach dem Abschluss des Veröffentlichungsvorgangs. Beachten Sie, dass nur die Markupseiten und die erforderlichen sever - und die clientseitige Unterstützungsdateien hochgeladen wurden.


[![Nur die benötigten Dateien wurden in der Produktionsumgebung veröffentlicht.](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**Abbildung 7**: nur die benötigten Dateien wurden veröffentlicht in der Produktionsumgebung ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-your-site-using-visual-studio-vb/_static/image21.png))


Die Option "Veröffentlichen" ist ein kleiner vor-Tool als die Kopie Websiteverwaltungs-Tool. Während der Kopie Websiteverwaltungs-Tool können Sie überprüfen die Dateien auf den lokalen und remote-Standorten und finden Sie unter unterschieden, bietet die Option "Veröffentlichen" Schnittstelle nicht. Darüber hinaus können mit der Kopie Websiteverwaltungs-Tool einmalige Änderungen erforderlich, hochladen oder einzelne Dateien löschen. Die Option "Veröffentlichen" lässt keine solche eine präzisere Kontrolle; Stattdessen veröffentlicht die *gesamten* Anwendung. Dieses Verhalten hat vor- und Nachteile. Auf der Seite plus wissen Sie, wenn Sie die Option "Veröffentlichen" verwenden, die Sie eine wichtige Datei hochladen beseitigt werden wird nicht. Jedoch berücksichtigen, was geschieht, wenn Sie eine kleine Änderung, um eine sehr große Website vorgenommen haben - klicken Sie mit der Option "Veröffentlichen" können nicht aktualisiert werden, diese Seite oder zwei, die geändert wurde, aber stattdessen müssen Sie warten, während Visual Studio den gesamten Standort bereitstellt.

Es ist nicht ungewöhnlich, dass es gibt bestimmte Dateien, deren Inhalt zwischen dem Produktions- und entwicklungsumgebungen ist unterschiedlich. Ein gutes Beispiel ist die Konfigurationsdatei der Anwendung `Web.config`. Da die Option "Veröffentlichen" Blind die Webanwendungsdateien kopiert überschreibt sie benutzerdefinierte Konfigurationsdateien der produktionsumgebung mit der Version in der Entwicklungsumgebung. Das nachfolgende Lernprogramm in diesem Thema wird erklärt, und bietet Tipps zum Bereitstellen einer Webanwendung aus, wenn solche Unterschiede vorhanden sind.

## <a name="summary"></a>Zusammenfassung

Bereitstellen einer Websites umfasst die erforderlichen Dateien in der Entwicklungsumgebung in die produktionsumgebung kopieren. Das vorherige Lernprogramm wurde gezeigt, wie Dateien, die mit einem FTP-Client wie FileZilla übertragen. In diesem Lernprogramm untersucht zwei Bereitstellungstools in Visual Studio: die Kopie Websiteverwaltungs-Tool und die Option "Veröffentlichen". Die Kopie Websiteverwaltungs-Tool ähnelt ein FTP-Client, es eine zweiteilige Benutzeroberfläche Auflisten der Dateien auf dem lokalen Computer und einem angegebenen Remotecomputer, die wurde mit hochladen und Herunterladen von Dateien zwischen den beiden Computern erleichtert wird. Die Option "Veröffentlichen" ist einem mehr stumpfen Tool, das explizit kompiliert das Projekt, und klicken Sie dann die gesamte Anwendung in das angegebene Ziel bereitstellt.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Kopieren die Website mit der Kopie Websiteverwaltungs-Tool](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Wie I: bereitgestellt einer Website mithilfe der Website-Kopiertool](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (Video)
- [Gewusst wie: Veröffentlichen von Webanwendungsprojekten](https://msdn.microsoft.com/library/aa983453.aspx)
- [Gewusst wie: Veröffentlichen von Websites](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Setup- und Bereitstellungsprojekte in Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

>[!div class="step-by-step"]
[Zurück](deploying-your-site-using-an-ftp-client-vb.md)
[Weiter](common-configuration-differences-between-development-and-production-vb.md)
