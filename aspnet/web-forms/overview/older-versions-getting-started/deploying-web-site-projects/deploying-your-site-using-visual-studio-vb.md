---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
title: Bereitstellen einer Website mit Visual Studio (VB) | Microsoft-Dokumentation
author: rick-anderson
description: Visual Studio enthält Tools zum Bereitstellen einer Websites. Weitere Informationen zu diesen Tools in diesem Tutorial.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 977105f3-7987-4e50-8be7-afb53b4ca28a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-vb
msc.type: authoredcontent
ms.openlocfilehash: 127f051d9e9e2e4bd5b180185846ebd67586f41d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402680"
---
<a name="deploying-your-site-using-visual-studio-vb"></a>Bereitstellen einer Website mit Visual Studio (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_vb.pdf)

> Visual Studio enthält Tools zum Bereitstellen einer Websites. Weitere Informationen zu diesen Tools in diesem Tutorial.


## <a name="introduction"></a>Einführung

Im vorherige Tutorial wurde erläutert, wie zum Bereitstellen einer einfachen ASP.NET Web-Anwendung auf einen Webhostinganbieter. Insbesondere das Lernprogramm wurde gezeigt, wie z. B. FileZilla FTP-Client verwenden, um die erforderlichen Dateien aus der Entwicklungsumgebung in die produktionsumgebung zu übertragen. Visual Studio bietet auch integrierte Tools, um die Bereitstellung in einem Webhostinganbieter zu erleichtern. In diesem Tutorial werden zwei dieser Tools: das Tool "Website kopieren", in dem Sie Dateien verschieben in und aus einem Remotewebserver installiert, die über FTP oder FrontPage-Servererweiterungen; und das Tool "Veröffentlichen", das die gesamte Website zu einem bestimmten Ort kopiert.


> [!NOTE]
> Andere Bereitstellung in Zusammenhang stehen Tools von Visual Studio bereitgestellten sind [Projekten](https://msdn.microsoft.com/library/wx3b589t.aspx) und [Web-Bereitstellungsprojekte](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) -Add-In. Web-Setup-Projekte Verpacken einer Website Inhalte und Informationen zur Konfiguration in eine einzelne MSI-Datei. Diese Option ist besonders nützlich für Websites, die in einem Intranet bereitgestellt werden oder für Unternehmen, die eine vorkonfigurierte Webanwendung zu verkaufen, die Kunden auf ihre eigenen Webserver installieren. Das Web Deployment Projects-Add-In ist, dass für eine Visual Studio-Add-In, das erleichtert die Angabe Konfigurationsunterschiede zwischen für entwicklungsumgebungen sowie produktionsumgebungen erstellen. Web-Setup-Projekte werden in dieser tutorialreihe nicht behandelt; Web-Bereitstellungsprojekte werden zusammengefasst, der [ *Common Configuration Unterschiede zwischen Entwicklungs- und Produktionsumgebungen* ](common-configuration-differences-between-development-and-production-vb.md) Tutorial.


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Bereitstellen einer Website mithilfe des Tools zum Kopieren von Website

Tool für Visual Studio Website kopieren ähnelt in der Funktionalität einer eigenständigen FTP-Client. Kurz gesagt, ermöglicht das Kopieren von Websites-Tool für die Verbindung mit einer remote-Website über FTP oder FrontPage-Servererweiterungen. Ähnlich wie FileZillas-Benutzeroberfläche, die Website kopieren-Benutzeroberfläche besteht aus zwei Bereichen: der linke Bereich enthält die lokalen Dateien, während Sie im rechte Bereich auf diese Dateien auf dem Zielserver aufgeführt.

> [!NOTE]
> Das Kopieren von Websites-Tool ist nur für Websiteprojekte zur Verfügung. Visual Studio bietet dieses Tool aus, wenn Sie mit einem Webanwendungsprojekt arbeiten.


Werfen wir einen Blick auf die mit dem Kopieren von Websites-Tool zum Veröffentlichen der Anwendung Buchbewertung für die Produktion. Da das Kopieren von Websites-Tool nur mit Projekten verwendet werden kann, die das Websiteprojekt-Modell verwenden, die, das wir nur mit diesem Tool und das Projekt BookReviewsWSP untersuchen kann. Öffnen Sie das Projekt ein.

Starten Sie das Kopieren von Websites-Tool-Projekt, indem Sie auf das Symbol "Website kopieren" im Projektmappen-Explorer (in Abbildung 1 ist dieses Symbol markiert); Alternativ können Sie die Option "Website kopieren" aus dem Menü für die Website auswählen. Beide Vorgehensweisen startet die Website kopieren-Benutzeroberfläche in Abbildung 1 dargestellt; nur im linken Bereich in Abbildung 1 wird angezeigt, da wir noch für die Verbindung mit einem Remoteserver verfügen.


[![Der Website Tools zum Kopieren der Benutzeroberfläche ist in zwei Bereiche unterteilt](deploying-your-site-using-visual-studio-vb/_static/image2.png)](deploying-your-site-using-visual-studio-vb/_static/image1.png)

**Abbildung 1**: der Website Tools zum Kopieren der Benutzeroberfläche ist in zwei Bereiche unterteilt ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-your-site-using-visual-studio-vb/_static/image3.png))


Zum Bereitstellen von unserer Website müssen wir zuerst mit dem Webhostinganbieter verbinden. Klicken Sie auf die Schaltfläche "Verbinden", am oberen Rand der Benutzeroberfläche für die Website kopieren. Dies zeigt das Dialogfeld "Website öffnen" in Abbildung 2 dargestellt.

Sie können an die Zielwebsite verbinden, indem Sie eine der vier Optionen aus der linken Seite auswählen:

- **Dateisystem** -wählen Sie diese Option, um Ihre Website auf einen Ordner oder einer Netzwerkfreigabe zugegriffen werden kann von Ihrem Computer bereitstellen.
- **Lokale IIS** -verwenden Sie diese Option zum Bereitstellen der Website an den IIS-Webserver auf Ihrem Computer installiert.
- **FTP-Site** -Verbindung mit einer remoten Website über FTP.
- **Remote-Standortsystemserver** -Verbindung mit einer remote-Website mit FrontPage-Servererweiterungen.

Die meisten Web-Host-Anbieter unterstützen, FTP, aber weniger FrontPage-Servererweiterung Unterstützung bieten. Aus diesem Grund habe ich die FTP-Site-Option ausgewählt und dann die Verbindungsinformationen eingegeben, wie in Abbildung 2 dargestellt.


[![Geben Sie die Zielwebsite](deploying-your-site-using-visual-studio-vb/_static/image5.png)](deploying-your-site-using-visual-studio-vb/_static/image4.png)

**Abbildung 2**: Geben Sie die Zielwebsite ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-your-site-using-visual-studio-vb/_static/image6.png))


Nach der verbindungsherstellung wird das Kopieren von Websites-Tool lädt die Dateien am Remotestandort im rechten Bereich und gibt den Status der einzelnen Dateien: neue, gelöschte, geänderte oder Unchanged. Sie können eine Datei vom lokalen Standort zum remote-standortsystemserver oder eine-umgekehrt kopieren.

Fügen Sie eine neue Seite auf das Projekt BookReviewsWSP und diese dann bereitstellen, damit wir das Tool "Website kopieren" in Aktion sehen können. Erstellen Sie eine neue ASP.NET-Seite in Visual Studio, in das Stammverzeichnis, das mit dem Namen `Privacy.aspx`. Haben Sie die Seite, die die Masterseite verwenden `Site.master` und die Datenschutzrichtlinie Ihres Standorts zu dieser Seite hinzufügen. Abbildung 3 zeigt Visual Studio, nachdem dieser Seite erstellt wurde.


[![Fügen Sie eine neue Seite mit dem Namen &lt;Code&gt;Privacy.aspx&lt;/code&gt; zum Stammordner der Website](deploying-your-site-using-visual-studio-vb/_static/image8.png)](deploying-your-site-using-visual-studio-vb/_static/image7.png)

**Abbildung 3**: Hinzufügen einer neuen Seite mit dem Namen `Privacy.aspx` zum Stammordner der Website ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-your-site-using-visual-studio-vb/_static/image9.png))


Als Nächstes geben Sie an der Benutzeroberfläche für die Website kopieren zurück. Wie in Abbildung 4 gezeigt, enthält im linke Bereich nun die neuen Dateien - `Policy.aspx` und `Policy.aspx.vb`. Diese Dateien sind, markiert, mit einem Pfeilsymbol und einen Status der neuen, gibt an, dass sie am lokalen Standort, aber nicht auf der Remotewebsite vorhanden sind.


[![Das Tool für die Copy-Website enthält neue &lt;Code&gt;Privacy.aspx&lt;/code&gt; Seite im linken Bereich](deploying-your-site-using-visual-studio-vb/_static/image11.png)](deploying-your-site-using-visual-studio-vb/_static/image10.png)

**Abbildung 4**: das Tool für die Copy-Website enthält neue `Privacy.aspx` Seite im linken Bereich ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-your-site-using-visual-studio-vb/_static/image12.png))


Zum Bereitstellen von neuen Dateien, wählen Sie sie aus, und klicken Sie dann auf das Pfeilsymbol, um sie mit dem Remotestandort zu übertragen. Nach Abschluss die Übertragung der `Policy.aspx` und `Policy.aspx.vb` Dateien vorhanden sind, auf der lokalen und remote-Website mit dem Status unverändert.

Zusammen mit der neue Dateien auflisten, werden die Tools zum Kopieren alle Dateien, die sich zwischen den lokalen und remote-Standorten unterscheiden. Um dies in Aktion zu sehen, zurück zu den `Privacy.aspx` Seite, und fügen Sie ein paar mehr Wörter zur Datenschutzrichtlinie. Speichern Sie die Seite, und klicken Sie dann zurück an das Tool für die Website kopieren. Wie in Abbildung 5 gezeigt, die `Privacy.aspx` Seite im linken Bereich weist den Status geändert, der angibt, dass sie mit dem Remotestandort nicht mehr synchron ist.


[![Das Tool zum Kopieren von Website gibt an, dass die &lt;Code&gt;Privacy.aspx&lt;/code&gt; Seite geändert wurde](deploying-your-site-using-visual-studio-vb/_static/image14.png)](deploying-your-site-using-visual-studio-vb/_static/image13.png)

**Abbildung 5**: das Tool zum Kopieren von Website gibt an, dass die `Privacy.aspx` Seite geändert wurde ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-your-site-using-visual-studio-vb/_static/image15.png))


Die Tools zum Kopieren gibt auch an, wenn eine Datei seit letztem Kopiervorgang gelöscht wurde. Löschen der `Privacy.aspx` aus den lokalen Projekt, und aktualisieren Sie das Tool für die Website kopieren. Die `Privacy.aspx` und `Privacy.aspx.vb` verbleiben im linken Bereich aufgelisteten, jedoch haben Sie den Status gelöscht, der angibt, dass sie seit letztem Kopiervorgang entfernt wurden.

## <a name="publishing-a-web-application"></a>Veröffentlichen einer Webanwendung

Eine weitere Möglichkeit zum Bereitstellen Ihrer Web-Anwendung aus Visual Studio ist die Verwendung der Option "Veröffentlichen", über das Menü "Build" zugegriffen werden kann. Die Option "Veröffentlichen" explizit kompiliert die Anwendung und kopiert dann alle erforderlichen Dateien bis zur angegebenen Remotestandort. Wie wir in Kürze sehen werden, ist die Option "Veröffentlichen" mehr als das Tool für die Website kopieren wir. Während das Kopieren von Websites-Tool Sie die Dateien auf dem lokalen Standorten und Remotestandorten können und ermöglicht das Hochladen oder einzelne Dateien bei Bedarf heruntergeladen werden, wird die Option "Veröffentlichen" die gesamte Webanwendung bereitgestellt.


Zusätzlich zum Kopieren alle erforderlichen Dateien an, mit dem angegebenen Remotestandort, wird die Anwendung auch explizit von der Option "Veröffentlichen" kompiliert. Angesichts der Tatsache, dass die Web Application Projects müssen muss explizit kompiliert Es überrascht nicht stammen, dass die Option "Veröffentlichen" für die Web Application Projects verfügbar ist. Was etwas überraschend sein mag, ist, dass die Option "Veröffentlichen" auch für Websiteprojekte verfügbar ist. Wie erwähnt in der [ *bestimmen was Dateien müssen bereitgestellt werden* ](determining-what-files-need-to-be-deployed-vb.md) Tutorial Websiteprojekte explizit durch einen Prozess aus, um genannte kompiliert werden kann *Vorkompilierung*. Dieses Tutorial konzentriert sich auf die Web Application Projects mit der Option "Veröffentlichen"; eine zukünftige Tutorial wird die Vorkompilierung, an welchem Punkt wir um Blick auf die Verwendung der Option "Veröffentlichen" mit Websiteprojekten zurückgeben.

> [!NOTE]
> Obwohl die Option "Veröffentlichen" in Visual Studio für Websiteprojekten und Webanwendungsprojekten verfügbar ist, bietet Visual Web Developer nur die Option "Veröffentlichen" für Webanwendungsprojekte.


Sehen wir uns, die mit der Option "Veröffentlichen" Book Reviews-Anwendung bereitstellen. Zunächst öffnen in Visual Studio BookReviewsWAP (das Webanwendungsprojekt). Wählen Sie das BookReviewsWAP Build-Projekt, aus dem Menü "Veröffentlichen". Daraufhin wird ein Dialogfeld, das Ziel, neben anderen Konfigurationsoptionen dazu aufgefordert werden (siehe Abbildung 6). Viel können z. B. mit dem Tool für die Website kopieren Sie einen Speicherort eingeben, der auf einem lokalen Ordner, eine lokale Website in IIS, eine remote-Website, die FrontPage-Servererweiterungen oder ein FTP-Serveradresse unterstützt verweist. Sie können auswählen, ob die Dateien auf dem remote-Web-Server mit den bereitgestellten Dateien zu ersetzen oder der gesamte Inhalt auf der Remotewebsite vor der Veröffentlichung zu löschen. Sie können auch angeben, ob kopiert:

- Nur die Dateien im Projekt erforderlich, um die Anwendung auszuführen, die den nicht benötigten Quellcode und das Projekt verknüpfte Dateien weglässt.
- Alle Projektdateien, die die Quellcodedateien enthält und Visual Studio-Projektdateien, wie die Projektmappendatei.
- Alle Dateien im Quellprojektordner, die kopiert alle Dateien im Quellprojektordner unabhängig davon, ob sie im Projekt enthalten sind.

Es gibt auch eine Option zum Hochladen von den Inhalt der `App_Data` Ordner.


[![Geben Sie die Zielwebsite](deploying-your-site-using-visual-studio-vb/_static/image17.png)](deploying-your-site-using-visual-studio-vb/_static/image16.png)

**Abbildung 6**: Geben Sie die Zielwebsite ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-your-site-using-visual-studio-vb/_static/image18.png))


Für die Anwendung Buchbewertung enthält die remote-standortsystemserver bereitgestellten beim Kopieren der BookReviewsWSP Projekts über die Tools zum Kopieren die Dateien. Daher müssen wir die Option "Veröffentlichen" starten, indem alle vorhandenen Inhalte löschen. Lassen Sie uns einfach kopieren Sie außerdem viele Realisierungslinks enthalten die produktionsumgebung nicht benötigte Quelldateien und die Projektdateien, anstatt die erforderlichen Dateien. Klicken Sie nachdem Sie diese Optionen angegeben haben auf die Schaltfläche "Veröffentlichen". Den nächsten Sekunden wird Visual Studio die erforderlichen Dateien an den Zielstandort Bereitstellen über den Fortschritt im Ausgabefenster anzeigen.

Abbildung 7 zeigt die Dateien auf der FTP-Site nach Abschluss des Veröffentlichungsvorgangs. Beachten Sie, dass nur die Markupseiten und die erforderlichen Server - und Client-Side Supportdateien hochgeladen wurden.


[![Nur die benötigten Dateien wurden in der Produktionsumgebung veröffentlicht werden.](deploying-your-site-using-visual-studio-vb/_static/image20.png)](deploying-your-site-using-visual-studio-vb/_static/image19.png)

**Abbildung 7**: nur die erforderlichen Dateien wurden veröffentlicht in der Produktionsumgebung ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-your-site-using-visual-studio-vb/_static/image21.png))


Die Option "Veröffentlichen" ist ein weniger facettenreichen Tool als das Tool für die Website kopieren. Während die Website kopieren-Tool können Sie überprüfen die Dateien auf dem lokalen Standorten und Remotestandorten und Unterschiede festzustellen, bietet die Option "Veröffentlichen" Schnittstelle nicht. Darüber hinaus ermöglicht das Kopieren von Websites-Tool einmalige Änderungen, hochladen oder einzelne Dateien werden gelöscht. Die Option "Veröffentlichen" lässt sich nicht auf eine differenzierte Kontrolle aus. Stattdessen veröffentlicht es den *gesamte* Anwendung. Dieses Verhalten hat vor- und Nachteile. Auf der Seite plus wissen Sie, wenn die Option "Veröffentlichen" verwenden, die Sie Sie vergessen wird nicht, um eine wichtige Datei hochzuladen. Aber bedenken, was geschieht, wenn Sie eine kleine Änderung, auf eine große Website vorgenommen haben, klicken Sie mit der Option "Veröffentlichen" kann nicht aktualisiert werden, Seite oder zwei, die geändert wurde, aber stattdessen müssen Sie warten, während Visual Studio den gesamten Standort bereitgestellt.

Es ist nicht ungewöhnlich, dass es gibt bestimmte Dateien, deren Inhalt zwischen dem Produktions- und entwicklungsumgebungen ist unterschiedlich. Ein Beispiel ist die Konfigurationsdatei der Anwendung `Web.config`. Da die Option "Veröffentlichen" Blind die Webanwendungsdateien kopiert überschreibt es die produktionsumgebung benutzerdefinierte Konfigurationsdateien mit der Version in der Entwicklungsumgebung. Im nachfolgenden Tutorial in diesem Thema erläutert und bietet Tipps für die Bereitstellung einer Webanwendung aus, wenn solche Unterschiede vorhanden sind.

## <a name="summary"></a>Zusammenfassung

Bereitstellen einer Websites umfasst das Kopieren der erforderlichen Dateien aus der Entwicklungsumgebung in der produktionsumgebung bereit. Das vorherige Lernprogramm wurde gezeigt, wie zum Übertragen von Dateien, die mit einem FTP-Client wie FileZilla. In diesem Tutorial untersucht zwei Bereitstellungstools in Visual Studio: das Tool für die Website kopieren "und" die Option "Veröffentlichen". Das Tool für die Website kopieren ähnelt einem FTP-Client, es eine zweiteilige Benutzeroberfläche, die die Dateien auf dem lokalen Computer und einem angegebenen Remotecomputer wurde, mit der sie zum Hochladen oder Herunterladen von Dateien zwischen den beiden Computern problemlos, auflisten. Die Option "Veröffentlichen" ist ein mehr Antworten Tool, das explizit kompiliert das Projekt, und klicken Sie dann die gesamte Anwendung in das angegebene Ziel bereitgestellt.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Kopieren die Website mit dem Tool zum Kopieren von Website](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [Wie stellen sich I: auf einer Website mithilfe des Tools zum Kopieren von Website](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (Video)
- [Gewusst wie: Veröffentlichen von Webanwendungsprojekten](https://msdn.microsoft.com/library/aa983453.aspx)
- [Gewusst wie: Veröffentlichen von Websites](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [Setup und Bereitstellungsprojekte in Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Zurück](deploying-your-site-using-an-ftp-client-vb.md)
> [Weiter](common-configuration-differences-between-development-and-production-vb.md)
