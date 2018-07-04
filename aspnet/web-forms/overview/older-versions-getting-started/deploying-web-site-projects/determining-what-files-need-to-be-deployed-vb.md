---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
title: Bestimmen, welche Dateien werden müssen (VB) bereitgestellten | Microsoft-Dokumentation
author: rick-anderson
description: Was müssen Dateien aus der Entwicklungsumgebung bereitgestellt werden, in der produktionsumgebung hängt zum Teil, ob die ASP.NET-Anwendung uns erstellt wurde...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: ea918f62-c9d6-4a7f-9bc6-e054d3764b2c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
msc.type: authoredcontent
ms.openlocfilehash: 89b45bff705996b799242ad55f156083f310af97
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37380388"
---
<a name="determining-what-files-need-to-be-deployed-vb"></a>Bestimmen, welche Dateien müssen sein bereitgestellt (VB)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_VB.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_vb.pdf)

> Welche Dateien aus der Entwicklungsumgebung in der produktionsumgebung bereitgestellt werden müssen, richtet sich teilweise auf, ob die ASP.NET-Anwendung erstellt wurde mit dem Modell der Website oder -Modell. Weitere Informationen finden Sie Informationen zu diesen beiden Projektmodellen und wie das Modell auf eine Bereitstellung auswirkt.


## <a name="introduction"></a>Einführung

Bereitstellen einer ASP.NET-Webanwendung umfasst das Kopieren von ASP.NET-bezogenen Dateien aus der Entwicklungsumgebung in der produktionsumgebung bereit. Die ASP.NET-bezogene Dateien enthalten Markup einer Webseite ASP.NET und Code und Client- und serverseitige Unterstützungsdateien. Unterstützungsdateien für die clientseitige sind diese Dateien verwiesen wird, von Ihren Webseiten und z. B. direkt an den Browser - Bilder, CSS-Dateien und JavaScript-Dateien gesendet. Serverseitige-Unterstützungsdateien enthalten, die zum Verarbeiten einer Anforderung auf der Serverseite verwendet werden. Dies schließt-Konfigurationsdateien, Webdienste, Klassendateien, typisierter DataSets und LINQ zu SQL-Dateien, u. a. an.

Im Allgemeinen alle Dateien von der clientseitigen Unterstützung in der Entwicklungsumgebung kopiert werden sollen, in der produktionsumgebung kopiert werden, welche Dateien für die serverseitige Unterstützung hängt jedoch, ob Sie den serverseitigen Code explizit in eine Assembly (eine Kompilieren`.dll` Datei) oder wenn Sie diese Assemblys, die automatisch generiert haben. Dieses Tutorial wird beschrieben, welche Dateien bereitgestellt werden, wenn explizit Kompilieren des Codes in eine Assembly und nicht von dieser Kompilierungsschritt automatisch ausgeführt werden müssen.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Explizite Kompilierung und die automatische Kompilierung

ASP.NET Web Pages sind in deklarativem Markup und Quellcode Code unterteilt. Der deklarativen Markup Teil enthält HTML-Steuerelemente und Databinding-Syntax; Der Codeteil enthält die Ereignishandler, die in Visual Basic oder C#-Code geschrieben wurde. Die Teile von Markup und Code in der Regel in unterschiedlichen Dateien getrennt: `WebPage.aspx` enthält die deklarative Markup beim `WebPage.aspx.vb` enthält den Code.

Erwägen Sie eine ASP.NET-Seite mit dem Namen `Clock.aspx` , die ein Label-Steuerelement, dessen Text-Eigenschaft festgelegt ist, um das aktuelle Datum und die Uhrzeit, die beim Laden der Seite, enthält. Der deklarativen Markup-Teil (in `Clock.aspx`) enthält das Markup für ein Label-Steuerelement - `<asp:Label runat="server" id="TimeLabel" />` – während der Codeteil (in `Clock.aspx.vb`) hätte eine `Page_Load` -Ereignishandler durch den folgenden Code:

[!code-vb[Main](determining-what-files-need-to-be-deployed-vb/samples/sample1.vb)]

In der Reihenfolge für die ASP.NET-Engine eine Anforderung für diese Seite, Codeteil der Seite (die *`WebPage`* `.aspx.vb` Datei) muss zunächst kompiliert werden. Dieser Kompilierung kann explizit oder automatisch erfolgen muss.

Wenn die Kompilierung explizit erfolgt, und klicken Sie dann die gesamte Anwendung Quellcode in eine oder mehrere Assemblys kompiliert (`.dll` Dateien) befindet sich in der Anwendung `Bin` Verzeichnis. In angeordnet, wenn es sich bei die Kompilierung automatisch erfolgt, und klicken Sie dann das resultierende automatisch generierte Assembly ist, wird standardmäßig die `Temporary ASP.NET Files` Ordner, in dem finden Sie unter `%WINDOWS%\Microsoft.NET\Framework\<version>`, obwohl hier über konfigurierbar ist die [ &lt; Kompilierung&gt; Element](https://msdn.microsoft.com/library/s10awwz0.aspx) in `Web.config`. Mit der expliziten Kompilierung Sie ergreifen müssen einige ASP.NET den Code der Anwendung in eine Assembly kompiliert, und dieser Schritt tritt vor der Bereitstellung. Bei der automatischen Kompilierung tritt ein, der Kompilierungsprozess auf dem Webserver beim ersten Zugriff auf die Ressource.

Unabhängig davon, welche Kompilierungsmodell verwenden, der Teil von Markup für alle ASP.NET-Seiten (die `WebPage.aspx` Dateien) in der produktionsumgebung kopiert werden müssen. Mit der expliziten Kompilierung müssen Sie sich die Assemblys im Kopieren der `Bin` Ordner, aber Sie müssen nicht die ASP.NET-Seiten Code Teile kopieren (die `WebPage.aspx.vb` Dateien). Bei der automatischen Kompilierung müssen Sie sich die Codedateien für das Teil zu kopieren, damit der Code vorhanden ist und kann automatisch kompiliert werden, wenn die Seite besucht wird. Der Teil von Markup für jede ASP.NET-Webseite umfasst eine `@Page` -Direktive zusammen mit Attributen, die angeben, ob der Code der Seite zugeordnete bereits explizit kompiliert wurde, oder gibt an, ob es automatisch kompiliert werden muss. Daher die produktionsumgebung kann nahtlos mit entweder Kompilierungsmodell arbeiten, und Sie müssen nicht gelten spezielle Konfigurationseinstellungen, um anzugeben, dass explizite oder automatische Kompilierung verwendet wird.

In Tabelle 1 sind die verschiedenen Dateien bereitstellen, wenn explizite Kompilierung und die automatische Kompilierung verwenden. Beachten Sie, dass unabhängig von der Kompilierung Modell Sie sollten immer Bereitstellen der Assemblys in der `Bin` Ordner, sofern dieser vorhanden ist. Die `Bin` Ordner enthält die Assemblys für die Webanwendung, die den kompilierten Quellcode enthalten, wenn das Modell für die explizite Kompilierung verwendet. Die `Bin` Verzeichnis auch enthält die Assemblys aus anderen Projekten sowie alle Open Source- oder Drittanbieter-Assemblys, die Sie verwenden und diese auf dem Produktionsserver werden müssen. Daher wird eine allgemeine Faustregel, kopieren Sie die `Bin` Ordner bis zur Produktion, die bei der Bereitstellung. (Wenn Sie mithilfe des Modells für die automatische Kompilierung und keine externen Assemblys verwenden keine `Bin` Directory –, die "OK" lautet.)

| **Kompilierungsmodell** | **Werden Teil-Markup-Datei bereitgestellt?** | **Bereitstellen von Quellcodedatei?** | **Bereitstellen von Assemblys in `Bin` Verzeichnis?** |
| --- | --- | --- | --- |
| Explizite Kompilierung | Ja | Nein | Ja |
| Automatische Kompilierung | Ja | Ja | Ja (falls vorhanden) |

**Tabelle 1: Welche Dateien Sie bereitstellen, hängt das Kompilierungsmodell verwendet.**

## <a name="taking-a-trip-down-memory-lane"></a>Einen tagesausflug nach-unten-Leitungs-Arbeitsspeicher

Welche Kompilierung-Ansatz verwendet wird, hängt teilweise von wie die ASP.NET-Anwendung in Visual Studio verwaltet wird. Da. NET Einführung im Jahr 2000 es vier verschiedene Versionen von Visual Studio - Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 und Visual Studio 2008 gab. Visual Studio .NET 2002 und 2003 verwaltet werden ASP.NET-Anwendungen mit der *Webanwendungsprojekt* Modell. Die wichtigsten Features von das Webanwendungsprojekt-Modell sind:

- Die Dateien, die Zusammensetzung des Projekts werden in einer Projektdatei definiert. Alle Dateien, die nicht in der Projektdatei definiert werden die Web-Anwendung von Visual Studio nicht berücksichtigt werden.
- Verwendet die explizite Kompilierung. Die Codedateien innerhalb des Projekts beim Erstellen des Projekts kompiliert werden, in eine einzelne Assembly, die in gespeichert wird die `Bin` Ordner.

Wenn Microsoft Visual Studio 2005 veröffentlicht, die sie Unterstützung für das Webanwendungsprojekt-Modell und mit dem Websiteprojekt-Modell ersetzt. Das Websiteprojekt-Modell unterschieden sich selbst durch die *Webanwendungsprojekt* Modell, es gibt folgende Möglichkeiten:

- Anstatt eine einzelne Projektdatei, die die Dateien des Projekts ausgeschrieben, wird das Dateisystem wird stattdessen verwendet. Kurz gesagt, gelten alle Dateien im Ordner der Web-Anwendung (oder Unterordner) als Teil des Projekts.
- Erstellen eines Projekts in Visual Studio erstellt sich nicht auf eine Assembly in den `Bin` Verzeichnis. Stattdessen meldet die Erstellung eines Websiteprojekts Fehlermeldungen während der Kompilierung.
- Unterstützung für die automatische Kompilierung. Websiteprojekte werden in der Regel durch Kopieren des Markup und Quellcode-Codes in der produktionsumgebung bereitgestellt, obwohl der Code vorkompilierte (explizite Kompilierung) sein kann.

Microsoft das Webanwendungsprojekt-Modell heraus wieder aktiviert, wenn sie Visual Studio 2005 Service Pack 1 veröffentlicht. Jedoch weiterhin Visual Web Developer nur Unterstützung für das Websiteprojekt-Modell. Die gute Nachricht ist, dass diese Einschränkung mit Visual Web Developer 2008 Service Pack 1 gelöscht wurde. Heute können Sie ASP.NET-Anwendungen in Visual Studio (und Visual Web Developer), über das Webanwendungsprojekt-Modell oder das Websiteprojekt-Modell erstellen. Beide Modelle haben ihre vor- und Nachteile. Finden Sie unter [Einführung in Webanwendungsprojekte: Vergleich von Websiteprojekten und Webanwendungsprojekten](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) einen Vergleich der beiden Modelle, und um zu entscheiden, welche Projektmodell eignet sich für Ihre Situation am besten geeignet.

## <a name="exploring-the-sample-web-application"></a>Untersuchen die Beispielwebanwendung

Der Download für dieses Lernprogramm enthält eine ASP.NET-Anwendung namens Book Reviews an. Die Website imitiert eine Hobby-Website, die ein Benutzer möglicherweise erstellen Sie mit ihrem Buch freigeben überprüft werden soll, mit der online-Community. Diese Webanwendung ASP.NET ist sehr einfach und besteht aus den folgenden Ressourcen:

- `Web.config`, die Anwendungskonfigurationsdatei.
- Eine Masterseite (`Site.master`).
- Sieben verschiedenen ASP.NET-Seiten:

    - ~/`Default.aspx` -der Website-Homepage.
    - ~/`About.aspx` -eine Seite "Über die Website".
    - ~/`Fiction/Default.aspx` -eine Seite mit der Liste der Science-Fiction-Bücher, die überarbeitet wurden.

        - ~/`Fiction/Blaze.aspx` – eine Übersicht über das neuartige Richard Bachman *Blaze*.
    - ~/`Tech/Default.aspx` -eine Seite mit der Liste der technologiebücher, die überarbeitet wurden.

        - ~/`Tech/CYOW.aspx` -eine Beschreibung der *Erstellen Ihrer eigenen Website*.
        - ~/`Tech/TYASP35.aspx` -eine Beschreibung der *bringen Sie sich ASP.NET 3.5 in 24 Stunden*.
- Drei verschiedener CSS-Dateien in die `Styles` Ordner.
- Vier Bilddateien - eine Powered by Logo ASP.NET und Images im Hintergrund der drei überprüft Bücher - alle der `Images` Ordner.
- Ein `Web.sitemap` Datei, die die Sitemap definiert und dient zum Anzeigen des Menüs in die `Default.aspx` Seiten in das Stammverzeichnis und `Fiction` und `Tech` Ordner.
- Eine Klassendatei namens `BasePage.vb` , definiert eine Basis `Page` Klasse. Diese Klasse erweitert die Funktionalität der `Page` Klasse, indem Sie automatisch Festlegen der `Title` -Eigenschaft basierend auf der Seite Position in der Siteübersicht. Kurz gesagt, jeder ASP.NET Code-Behind-Klasse, die erweitert `BasePage` (anstelle von `System.Web.UI.Page`) hat den Titel, der auf einen Wert abhängig von seiner Position in der Siteübersicht festgelegt. Z. B. beim Anzeigen der ~ /`Tech/CYOW.aspx` Seite der Titel "Start: Technologie: Erstellen Ihrer eigenen Website" festgelegt ist.

Abbildung 1 zeigt einen Screenshot der Book Reviews-Website über einen Browser angezeigt. Hier sehen Sie die Seite "~ / Tech/TYASP35.aspx, überprüft das Buch *bringen Sie sich ASP.NET 3.5 in 24 Stunden*. Die Breadcrumb-Leiste, die am oberen Rand der Seite, und klicken Sie im Menü in der linken Spalte umfasst basieren auf der Site Map-Struktur, die in definierten `Web.sitemap`. Das Bild in der rechten oberen Ecke eines Images an den Buch wird erläutert, ist die `Images` Ordner. Erscheinungsbild der Website definiert sind, über das cascading Stylesheet-Regeln wie folgt buchstabiert die CSS-Dateien in die `Styles` Ordner während des übergreifende Seitenlayouts, in die Masterseite definiert wird `Site.master`.


[![Die Book Reviews-Website bietet eine Sammlung von Titeln-Bewertungen](determining-what-files-need-to-be-deployed-vb/_static/image2.png)](determining-what-files-need-to-be-deployed-vb/_static/image1.png)

**Abbildung 1**: The Book Reviews-Website bietet eine Sammlung von Titeln-Bewertungen ([klicken Sie, um das Bild in voller Größe anzeigen](determining-what-files-need-to-be-deployed-vb/_static/image3.png))


Diese Anwendung wird nicht mit eine Datenbank verwendet. Jede Überprüfung wird als separate Webseite in der Anwendung implementiert. In diesem Tutorial (und die nächste mehrere Lernprogramme) führen durch die Bereitstellung einer Webanwendung, die nicht über eine Datenbank verfügt. Allerdings in einem späteren Tutorial wir erhöhen Sie diese Anwendung zum Speichern von Überprüfungen, Leserkommentare und andere Informationen in einer Datenbank und wird untersucht, welche Schritte müssen ausgeführt werden, um eine datengesteuerte Webanwendung ordnungsgemäß bereitgestellt.

> [!NOTE]
> In diesen Tutorials Hosten von ASP.NET-Anwendungen mit einem Webhostinganbieter konzentrieren, und es werden zusätzliche Themen wie ASP nicht untersuchen. NET Standortsystem-Zuordnung oder mit einer Seite-Basisklasse. Weitere Informationen zu diesen Technologien und Weitere Hintergrundinformationen zu anderen Themen im Lernprogramm behandelt finden Sie am Ende jedes Lernprogramm im Abschnitt Weitere nützliche Informationen.


Dieses Tutorial "Download" hat zwei Kopien der Webanwendung, die jeweils als einen anderen Typ von Visual Studio-Projekt implementiert: BookReviewsWAP, eine Web Application Project und BookReviewsWSP, eines Websiteprojekts. Beide Projekte mit Visual Web Developer 2008 SP1 erstellt wurden, und Verwenden von ASP.NET 3.5 SP1. Starten zum Arbeiten mit diesen Projekten Entzippen Sie den Inhalt auf Ihrem Desktop. Um das Webanwendungsprojekt (BookReviewsWAP) zu öffnen, navigieren Sie zu der `BookReviewsWAP` Ordner und doppelklicken Sie auf die Projektmappendatei, `BookReviewsWAP.sln`. Um das Websiteprojekt (BookReviewsWSP) zu öffnen, starten Sie Visual Studio, und klicken Sie dann aus dem Menü "Datei", wählen Sie die Option "Website öffnen", navigieren Sie zu der `BookReviewsWSP` Ordner auf Ihrem Desktop, und klicken Sie auf OK.


Die verbleibenden zwei Abschnitte in diesem Tutorial betrachten welche Dateien müssen Sie in der produktionsumgebung zu kopieren, beim Bereitstellen der Anwendung. Die nächsten beiden Lernprogramme - [ *Bereitstellen Ihrer Website mithilfe von FTP* ](deploying-your-site-using-an-ftp-client-vb.md) und [ *Bereitstellen Ihrer Website mit Visual Studio* ](deploying-your-site-using-visual-studio-vb.md) -zeigen verschiedene Verwendungsmöglichkeiten Kopieren Sie diese Dateien auf einen Webhostinganbieter.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Bestimmen die Dateien für das Webprojekt für die Anwendung bereitstellen.

Das Webanwendungsprojekt-Modell verwendet explizite Kompilierung - Quellcode des Projekts kompiliert wird, in eine einzelne Assembly jedes Mal, wenn Sie die Anwendung zu erstellen. Diese Kompilierung enthält die ASP.NET-Seiten Code-Behind-Dateien (~ /`Default.aspx.vb`, ~ /`About.aspx.vb`und so weiter), als auch die `BasePage.vb` Klasse. Die resultierende Assembly ist mit dem Namen `BookReviewsWAP.dll` und befindet sich in der Anwendung `Bin` Verzeichnis.

Abbildung 2 zeigt die Dateien, aus denen das Book Reviews-Webanwendungsprojekt.


[![Im Projektmappen-Explorer listet die Dateien, die das Webanwendungsprojekt zu bilden.](determining-what-files-need-to-be-deployed-vb/_static/image5.png)](determining-what-files-need-to-be-deployed-vb/_static/image4.png)

**Abbildung 2**: der Projektmappen-Explorer werden die Dateien, die das Webanwendungsprojekt umfassen aufgeführt.


> [!NOTE]
> Wie in Abbildung 2 gezeigt, werden die ASP.NET-Seiten Code-Behind-Dateien für ein Webanwendungsprojekt in Visual Basic nicht im Projektmappen-Explorer angezeigt. Zum Anzeigen der Code-Behind-Klasse für eine Seite mit der rechten Maustaste im Projektmappen-Explorer auf der Seite, und wählen Sie Code anzeigen.


Zum Bereitstellen einer ASP.NET-Anwendung entwickelt, mit dem Webanwendungsprojekt-Modell-Start durch Erstellen der Anwendung aus, um die neuesten Quellcode explizit in eine Assembly zu kompilieren. Kopieren Sie als Nächstes die folgenden Dateien in der produktionsumgebung bereit:

- Dateien mit der deklarativen Markup für jede ASP.NET-Seite, wie z. B. ~ /`Default.aspx`, ~ /`About.aspx`und so weiter. Kopieren Sie außerdem um deklarativen Markup für jede Master-Seiten und Benutzersteuerelemente.
- Die Assemblys (`.dll` Dateien) in der `Bin` Ordner. Sie müssen sich nicht um die Programmdateien für die Datenbank zu kopieren (`.pdb`) oder XML-Dateien Sie unter Umständen in finden der `Bin` Verzeichnis.

Sie müssen sich nicht um die ASP.NET-Seiten Quellcodedateien in der produktionsumgebung zu kopieren, Sie brauchen auch zum Kopieren der `BasePage.vb` Klassendatei.

> [!NOTE]
> Wie in Abbildung 2 gezeigt, die `BasePage` -Klasse wird implementiert, als eine Datei im Projekt im Ordner mit dem Namen abgelegt `HelperClasses`. Wenn das Projekt kompiliert wird der Code in die `BasePage.vb` Datei sowie die ASP.NET-Seiten CodeBehind-Klassen in die einzelne Assembly kompiliert wird `BookReviewsWAP.dll`. ASP.NET verfügt über einen speziellen Ordner mit dem Namen `App_Code` , das zum Speichern von Klassendateien für Websiteprojekte geeignet ist. Der Code in die `App_Code` Ordner wird automatisch kompiliert, und sollte daher nicht mit Webanwendungsprojekten verwendet werden. Stattdessen sollten Sie den Dateien Ihrer Anwendung Klasse platzieren, in einem normalen Ordner namens `HelperClasses`, oder `Classes`, oder etwas Ähnliches. Alternativ können Sie Klassendateien in ein separates Klassenbibliotheksprojekt platzieren.


Zusätzlich zum Kopieren der ASP.NET bezogene Markup-Dateien und der Assembly in den `Bin` Ordner müssen Sie auch die clientseitige Unterstützung-Dateien – die Images und CSS-Dateien – sowie die anderen serverseitigen Supportdateien kopieren `Web.config` und `Web.sitemap`. Diese Client - und serverseitige Unterstützung Dateien müssen in der produktionsumgebung bereit, unabhängig davon, ob Sie explizite oder automatische Kompilierung kopiert werden soll.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Bestimmen die Dateien für die Website-Projektdateien bereitstellen

Das Websiteprojekt-Modell unterstützt die automatische Kompilierung, ein Feature nicht verfügbar, wenn das Webanwendungsprojekt-Modell verwendet. Mit explizite Kompilierung müssen Sie Quellcode Ihres Projekts in eine Assembly kompilieren und kopieren diese Assembly in der produktionsumgebung bereit. Auf der anderen Seite mit automatische Kompilierung Sie einfach den Quellcode in der produktionsumgebung kopieren, und es wird von der Laufzeit bei Bedarf kompiliert, je nach Bedarf.

Die Menüoption "Build" in Visual Studio ist sowohl für Webanwendungsprojekte als auch für Websiteprojekte in vorhanden. Erstellen einer Web Application Projects Quellcode des Projekts kompiliert, in eine einzelne Assembly befindet sich in der `Bin` Verzeichnis, das Erstellen eines Websiteprojekts alle während der Kompilierung auf Fehler überprüft, aber keine Assemblys erstellt. Zum Bereitstellen einer ASP.NET-Anwendung entwickelt, mit dem Websiteprojekt-Modell alle erforderlichen Vorgehensweise ist kopieren die entsprechenden Dateien in der produktionsumgebung sollten Sie zum ersten Erstellen des Projekts, um sicherzustellen, dass keine Kompilierungsfehler vorhanden sind.

Abbildung 3 zeigt die Dateien, aus denen das Book Reviews-Websiteprojekt besteht.


[![Im Projektmappen-Explorer listet die Dateien, die das Websiteprojekt zu bilden.](determining-what-files-need-to-be-deployed-vb/_static/image7.png)](determining-what-files-need-to-be-deployed-vb/_static/image6.png)

**Abbildung 3**: der Projektmappen-Explorer werden die Dateien, die das Websiteprojekt umfassen aufgeführt.


Bereitstellen eines Projekts für die Website umfasst das Kopieren aller ASP.NET-bezogene Dateien in der produktionsumgebung -, die die Markupseiten für ASP.NET Seiten, Masterseiten und Benutzersteuerelemente, zusammen mit ihren Codedateien enthält. Sie müssen auch Sie jede Klassendateien, kopieren Sie z. B. `BasePage.vb`. Beachten Sie, dass die `BasePage.vb` Datei befindet sich der `App_Code` Ordner, in dem eine spezielle ASP.NET-Ordner in Websiteprojekte für Klassendateien verwendet wird. Der besondere Ordner als dem Klassendateien in Produktion, ebenfalls erstellt werden muss die `App_Code` Ordner in der Entwicklungsumgebung kopiert werden muss, um die `App_Code` Ordner in der Produktion.

Zusätzlich zum Kopieren von Codedateien für das ASP.NET Markup und Quellcode, müssen Sie auch die clientseitige Unterstützung-Dateien – die Images und CSS-Dateien – sowie die anderen serverseitigen Supportdateien kopieren `Web.config` und `Web.sitemap`.

> [!NOTE]
> Website-Projekten können auch explizite Kompilierung verwenden. Eine zukünftige Tutorial werden explizit einem Websiteprojekt Kompilierung untersucht.


## <a name="summary"></a>Zusammenfassung

Bereitstellen einer ASP.NET-Anwendung umfasst das Kopieren der erforderlichen Dateien aus der Entwicklungsumgebung in der produktionsumgebung bereit. Der genaue Satz von Dateien, die synchronisiert werden müssen, hängt davon ab, ob ASP.NET den Code der Anwendung explizit oder automatisch kompiliert wird. Die Strategie für die Kompilierung eingesetzt wird von, ob Visual Studio konfiguriert ist zum Verwalten der ASP.NET-Anwendung verwenden, das Webanwendungsprojekt-Modell oder das Websiteprojekt-Modell beeinflusst.

Das Webanwendungsprojekt-Modell verwendet die explizite Kompilierung und kompiliert Sie den Projektcode in einer Assembly in den `Bin` Ordner. Bei der Bereitstellung der Anwendung, die Teil von Markup für die ASP.NET-Seiten und den Inhalt der dem `Bin` Ordner muss sich in der produktionsumgebung abgelegt werden, der Quellcode in der Anwendung – die Codedateien und CodeBehind-Klassen, z. B. – ist nicht erforderlich. in der produktionsumgebung kopiert werden.

Das Websiteprojekt-Modell verwendet die automatische Kompilierung standardmäßig, obwohl es möglich, explizit ein Websiteprojekt zu kompilieren wie wir in zukünftigen Lernprogrammen sehen. Bereitstellen einer ASP.NET-Anwendung aus, die automatische Kompilierung verwendet, erfordert, dass der Teil des Markup *und* Quellcode kopiert werden muss, in der produktionsumgebung bereit. Der Code wird in der produktionsumgebung automatisch kompiliert, wenn er zum ersten Mal angefordert wird.

Nun, da wir untersucht haben, welche Dateien zwischen den Umgebungen für Entwicklungs- und produktionsumgebungen synchronisiert werden müssen können wir die Book Reviews-Anwendung in einem Webhostinganbieter bereitzustellen.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Übersicht über die ASP.NET-Kompilierung](https://msdn.microsoft.com/library/ms178466.aspx)
- [Benutzersteuerelemente von ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Untersuchen ASP. NET Website-Navigation](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Einführung in Webanwendungsprojekte](https://msdn.microsoft.com/library/aa730880.aspx)
- [Master-Seite-Lernprogramme](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Freigeben von Code zwischen den Seiten](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Verwenden eine benutzerdefinierte Basisklasse für Ihre ASP.NET-Seiten CodeBehind-Klassen](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Visual Studio 2005-Website-Projektsystem: Worum handelt es sich, und warum haben wir es getan?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Exemplarische Vorgehensweise: Konvertieren eines Websiteprojekts in ein Webanwendungsprojekt in Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Zurück](asp-net-hosting-options-vb.md)
> [Weiter](deploying-your-site-using-an-ftp-client-vb.md)
