---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: "Bestimmen, welche Dateien werden müssen bereitgestellt (c#) | Microsoft Docs"
author: rick-anderson
description: "Welche Dateien in der Entwicklungsumgebung in der produktionsumgebung bereitgestellt werden müssen, richtet sich teilweise auf, ob die ASP.NET-Anwendung uns erstellt wurde..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: ccfaa2311fe7d9bf750276c969eeb08d5c5568de
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="determining-what-files-need-to-be-deployed-c"></a>Bestimmen, welche Dateien werden müssen bereitgestellt (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip) oder [PDF herunterladen](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> Welche Dateien in der Entwicklungsumgebung in der produktionsumgebung bereitgestellt werden müssen, richtet sich teilweise auf, ob die ASP.NET-Anwendung mithilfe von der Website-Modell oder Webanwendungsmodells erstellt wurde. Weitere Informationen zu dieser zwei Projektmodelle und wie das Projektmodell Bereitstellung auswirkt.


## <a name="introduction"></a>Einführung

Bereitstellen einer ASP.NET-Webanwendung umfasst das Kopieren von Dateien ASP.NET bezogene aus der Entwicklungsumgebung in der produktionsumgebung. Die ASP.NET bezogene Dateien umfassen ASP.NET Web-Seitenmarkup und Code und Client- und serverseitige Dateien unterstützen. Unterstützung einer clientseitigen-Dateien handelt es sich um die Dateien von Webseiten verwiesen und diese direkt an den Browser - Bilder, CSS-Dateien und JavaScript-Dateien, z. B. gesendet. Serverseitige-Unterstützungsdateien enthalten, die zum Verarbeiten einer Anforderung auf der Serverseite verwendet werden. Dies schließt Konfigurationsdateien, Webdienste, Klassendateien, typisierte DataSets und LINQ auf SQL-Dateien, o. ä.

Im Allgemeinen alle clientseitige Unterstützungsdateien aus der Entwicklungsumgebung kopiert werden sollen, in der produktionsumgebung kopiert werden, welche serverseitige Unterstützungsdateien hängt jedoch, ob Sie den serverseitigen Code explizit in eine Assembly (eine Kompilieren`.dll` Datei) oder wenn Sie diese Assemblys automatisch generierter Probleme auftreten. In diesem Lernprogramm werden hervorgehoben, welche Dateien bereitgestellt werden, wenn explizit Kompilieren des Codes in einer Assembly und nicht von dieser Kompilierungsschritt automatisch ausgeführt werden müssen.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Explizite Kompilierung im Vergleich zu automatische Kompilierung

ASP.NET Web Pages sind in deklaratives Markup und Quellcode unterteilt. Der Teil von deklarativem Markup enthält, HTML, Web-Steuerelemente und Databinding-Syntax. der Code enthält Ereignishandler in Visual Basic oder C#-Code geschrieben wurde. Die Teile von Markup und Code werden in der Regel in unterschiedlichen Dateien unterteilt: `WebPage.aspx` enthält das deklarative Markup beim `WebPage.aspx.cs` enthält den Code.

Erwägen Sie eine ASP.NET-Seite mit dem Namen Clock.aspx, die ein Bezeichnungsfeld-Steuerelement enthält, dessen Text-Eigenschaft festgelegt ist, um das aktuelle Datum und die Uhrzeit, die beim Laden der Seite, aus. Der Teil von deklarativem Markup (in `Clock.aspx`) enthält das Markup für ein Label-Websteuerelement -`<asp:Label runat="server" id="TimeLabel" />` – während der Codeteil (in `Clock.aspx.cs`) müsste eine `Page_Load` -Ereignishandler durch den folgenden Code:

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

In der Reihenfolge für das Datenbankmodul ASP.NET eine Anforderung für diese Seite, die Seite Codeteil (die `WebPage.aspx.cs` Datei) muss zuerst kompiliert werden. Dieser Kompilierung kann explizit oder automatisch durchgeführt werden soll.

Wenn die Kompilierung explizit erfolgt, und klicken Sie dann die gesamte Anwendung Quellcode wird in eine oder mehrere Assemblys kompiliert (`.dll` Dateien) befindet sich in der Anwendungsverzeichnis `Bin` Verzeichnis. Wenn es sich bei die Kompilierung automatisch ablaufenden Schlüsselaustausches eingerichtet und dann das resultierende automatisch generierte Assembly ist, wird standardmäßig platziert werden, der `Temporary ASP.NET` Dateien (Ordner), das auf `%WINDOWS%\Microsoft.NET\Framework\`  *&lt;Version&gt;*, Obwohl dieser Speicherort unterscheidet sich konfigurierbar über die [ `<compilation>` Element](https://msdn.microsoft.com/en-us/library/s10awwz0.aspx) in `Web.config`. Mit der expliziten Kompilierung müssen Sie eine bestimmte Aktion, die ASP.NET-Anwendung Code in eine Assembly zu kompilieren, und dieser Schritt wird vor der Bereitstellung. Mit der automatische Kompilierung tritt auf, im Verlauf des Vorgangs auf dem Webserver Wenn zuerst die Ressource zugegriffen wird.

Unabhängig davon, welche Kompilierungsmodell verwenden, der Teil von Markup für alle ASP.NET-Seiten (die `WebPage.aspx` Dateien) in der produktionsumgebung kopiert werden müssen. Mit der expliziten Kompilierung müssen Sie kopieren Sie die Assemblys in der `Bin` Ordner, aber Sie müssen nicht den ASP.NET-Seiten Codeteile kopieren (die `WebPage.aspx.cs` Dateien). Automatische Kompilierung müssen Sie die Codedateien für den Bereich kopieren, damit der Code vorhanden ist und kann automatisch kompiliert werden, wenn die Seite besucht wird. Der Teil von Markup für jede ASP.NET-Webseite enthält eine `@Page` -Direktive mit Attributen, die angeben, ob die zugeordneten Seitencode bereits explizit kompiliert wurde, oder gibt an, ob es automatisch kompiliert werden muss. Daher die produktionsumgebung funktioniert nahtlos mit entweder Kompilierungsmodell und Sie müssen nicht gelten spezielle Konfiguration-Einstellungen, um anzugeben, dass explizite oder automatische Kompilierung verwendet wird.

Tabelle 1 werden die Dateien bereitstellen, wenn explizite Kompilierung im Vergleich zu automatische Kompilierung mit zusammengefasst. Beachten Sie, dass unabhängig von der Kompilierung Modell verwenden sollten immer Bereitstellen der Assemblys in der `Bin` Ordner, sofern dieser vorhanden ist. Die `Bin` Ordner enthält die Assemblys, spezifisch für die Web-Anwendung, die den kompilierten Quellcode angeben, wenn das explizite Kompilierungsmodell zu verwenden. Die `Bin` Verzeichnis enthält auch Assemblys aus anderen Projekten und den Open Source- oder Drittanbieter-Assemblys, die Sie verwenden möglicherweise, und diese auf dem Produktionsserver werden müssen. Daher sollten Sie als allgemeine Faustregel, kopieren Sie die `Bin` Ordner bis zur Produktion, bei der Bereitstellung. (Wenn Sie mithilfe des Modells für die automatische Kompilierung und keine externen Assemblys verwenden Sie erhalten keine `Bin` Directory –, die "OK" lautet.)

| **Kompilierungsmodell** | **Werden Teil der Markupdatei bereitgestellt?** | **Werden Quellcodedatei bereitgestellt?** | **Bereitstellen von Assemblys in `Bin` Verzeichnis?** |
| --- | --- | --- | --- |
| Explizite Kompilierung | Ja | Nein | Ja |
| Automatische Kompilierung | Ja | Ja | Ja (falls vorhanden) |

**Tabelle 1:** welche Dateien, die Sie bereitstellen, hängt das Kompilierungsmodell verwendet.

## <a name="taking-a-trip-down-memory-lane"></a>Erstellen eine Reise unten Arbeitsspeicher Lane

Welche Kompilierung Ansatz verwendet wird hängt teilweise von wie die ASP.NET-Anwendung in Visual Studio verwaltet wird. Wurden. NET Cacheobjekts im Jahr 2000 vier verschiedene Versionen von Visual Studio - Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 und Visual Studio 2008 stattgefunden haben. Visual Studio .NET 2002 und 2003 verwaltet ASP.NET-Anwendungen mit der *Webanwendungsprojekt Modell*. Die wichtigsten Funktionen des Webanwendungsprojekts Modells sind:

- Dateien, die Zusammensetzung des Projekts in einer Projektdatei definiert sind. Alle Dateien, die nicht in der Projektdatei definiert werden Teil der Webanwendung von Visual Studio nicht berücksichtigt werden.
- Verwendet die explizite Kompilierung. Beim Erstellen des Projekts die Codedateien innerhalb des Projekts in einer einzelnen Assembly, die in der befindet kompiliert die `Bin` Ordner.

Wenn Microsoft Visual Studio 2005 freigegeben, die sie Unterstützung für das Modell Webanwendungsprojekt gelöscht und durch das Websiteprojekt-Modell ersetzt. Das Website-Projektmodell unterschieden selbst aus dem Modell Webanwendungsprojekt auf folgende Weise:

- Im Dateisystem wird stattdessen verwendet, anstatt eine einzelnes Projekt-Datei, die die Dateien des Projekts ausgeschrieben. Kurz gesagt, gelten alle Dateien innerhalb der Webanwendungsordner (oder Unterordner) als Teil des Projekts.
- Erstellen eines Projekts in Visual Studio erstellt sich nicht auf eine Assembly in die `Bin` Verzeichnis. Stattdessen meldet die Erstellen eines Projekts für die Website Kompilierungsfehler.
- Unterstützung für die automatische Kompilierung. Websiteprojekte werden in der Regel durch Kopieren von Markup und Source-Code in der produktionsumgebung bereitgestellt, obwohl der Code vorkompilierte (explizite-Kompilierung) sein kann.

Microsoft fortgesetzt das Webanwendungsprojekt Modell, wenn sie Visual Studio 2005 Service Pack 1 veröffentlicht. Visual Web Developer wurde jedoch nur Unterstützung für das Websiteprojekt-Modell. Die gute Nachricht ist, dass diese Einschränkung mit Visual Web Developer 2008 Service Pack 1 gelöscht wurde. Heute können Sie ASP.NET-Anwendungen in Visual Studio und Visual Web Developer per das Webanwendungsprojekt-Modell oder das Websiteprojekt-Modell erstellen. Beide Modelle verfügen über ihre vor- und Nachteile. Verweisen auf [Einführung in Webanwendungsprojekte: Vergleichen von Websiteprojekte und Webanwendungsprojekte](https://msdn.microsoft.com/en-us/library/aa730880.aspx#wapp_topic5) für einen Vergleich der beiden Modelle, und um zu entscheiden, welche Projektmodell am besten für Ihre Situation geeignet.

## <a name="exploring-the-sample-web-application"></a>Untersuchen die Beispielwebanwendung

Der Download für dieses Lernprogramm umfasst eine ASP.NET-Anwendung Buch Reviews aufgerufen. Die Website imitiert eine Hobbys-Website, die eine Person erstellen möglicherweise ihre Buch freigeben überprüft werden soll, mit der online-Community. Diese ASP.NET-Webanwendung ist sehr einfach und besteht aus den folgenden Ressourcen:

- `Web.config`, der Anwendungskonfigurationsdatei.
- Eine Gestaltungsvorlage (`Site.master`).
- Sieben verschiedenen ASP.NET-Seiten: 

    - ~`/Default.aspx`-Homepage der Website.
    - ~`/About.aspx`-eine Seite "Über die Website".
    - ~`/Fiction/Default.aspx`-eine Seite mit der Liste die Idee Bücher, die überarbeitet wurden. 

        - ~`/Fiction/Blaze.aspx`-eine Überprüfung der Richard Bachman Novel *Blaze*.
    - ~/`Tech/Default.aspx`-eine Seite mit der Technologie Bücher, die überarbeitet wurden. 

        - ~/`Tech/CYOW.aspx`-eine Überprüfung der *Erstellen Ihrer eigenen Website*.
        - ~/`Tech/TYASP35.aspx`-eine Überprüfung der *Schulen selbst ASP.NET 3.5 in 24 Stunden*.
- Drei verschiedene CSS-Dateien im Ordner "Formatvorlagen".
- Vier Bilddateien - eine Powered by ASP.NET-Logo und Images im Hintergrund der drei überprüft Bücher - alle befindet sich in der `Images` Ordner.
- Ein `Web.sitemap` Datei, die die Siteübersicht definiert und dient zum Anzeigen des Menüs in der `Default.aspx` Seiten in das Stammverzeichnis und `Fiction` und `Tech` Ordner.
- Eine Klassendatei namens `BasePage.cs` , definiert eine Basis `Page` Klasse. Diese Klasse erweitert die Funktionalität der `Page` Klasse durch Festlegen von automatisch die `Title` -Eigenschaft basierend auf der Seite Position in der Siteübersicht. Kurz gesagt, jede ASP.NET Code-Behind-Klasse erweitert `BasePage` (anstelle von `System.Web.UI.Page`) hat den Titel, der auf einen Wert je nach seiner Position in der Siteübersicht festgelegt. Z. B. beim Anzeigen der ~ /`Tech/CYOW.aspx` Seite der Titel "Start: Technologie: Erstellen Ihrer eigenen Website" festgelegt ist.

Abbildung 1 zeigt einen Screenshot der Buch Reviews-Website, wenn Sie über einen Browser angezeigt. Hier sehen Sie die Seite "~ /`Tech/TYASP35.aspx`, die das Buch überprüft *Schulen selbst ASP.NET 3.5 in 24 Stunden*. Die Breadcrumb-Leiste, die am Anfang der Seite ", und klicken Sie im Menü in der linken Spalte umfasst basieren auf der Siteübersichtsstruktur in definierten `Web.sitemap`. Das Bild in der rechten oberen Ecke ist eines der Bilder befindet sich im Buch Cover der `Images` Ordner. Der Website Aussehen und Verhalten werden über das cascading Stylesheet-Regeln wie folgt buchstabiert CSS-Dateien im Ordner "Stile", während der übergreifenden Seitenlayout, in die Masterseite definiert wird definiert `Site.master`.


[![Überprüft die Buch-Website bietet Reviews auf eine Sammlung von Titeln](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**Abbildung 1:** überprüft die Buch-Website bietet Reviews auf eine Sammlung von Titeln ([klicken Sie hier, um das Bild in voller Größe angezeigt](determining-what-files-need-to-be-deployed-cs/_static/image3.png))


Diese Anwendung wird nicht mit eine Datenbank verwendet. Jede Überprüfung wird als einer separaten Webseite in der Anwendung implementiert. In diesem Lernprogramm (und die nächste mehrere Lernprogramme) führen durch das Bereitstellen einer Webanwendung, die über keine andere Datenbank. Allerdings in einem späteren Lernprogramm wir wird diese Anwendung zum Speichern von Bewertungen, Reader Kommentare und andere Informationen in einer Datenbank erhöhen, und untersuchen, welche Schritte müssen ausgeführt werden, um eine datengesteuerte Webanwendung ordnungsgemäß bereitgestellt.

> [!NOTE]
> Diese Lernprogramme zum Hosten von ASP.NET-Anwendungen mit einem Webhostinganbieter zu konzentrieren, und untersuchen Sie Hilfsdateien Themen wie ASP nicht. NET Standortsystem-Karte oder eine Basis mit `Page` Klasse. Weitere Informationen zu diesen Technologien sowie weitere Hintergrundinformationen zu anderen Themen im gesamten Lernprogramm finden Sie in Abschnitt Weitere Themen, am Ende jedes Lernprogramm.


In diesem Lernprogramm wurden zwei Kopien der Webanwendung, die jedes als ein anderer Typ von Visual Studio-Projekt implementiert: BookReviewsWAP ein Webanwendungsprojekt und BookReviewsWSP, eine Website-Projekt. Beide Projekte mit Visual Web Developer 2008 SP1 erstellt wurden, und Verwenden von ASP.NET 3.5 SP1. Diese Projekte zunächst für die Zusammenarbeit mit Entzippen den Inhalt auf Ihrem Desktop. Öffnen das Webanwendungsprojekt (BookReviewsWAP), navigieren zu dem Ordner BookReviewsWAP aus, und doppelklicken Sie auf die Projektmappendatei `BookReviewsWAP.sln`. Um das Websiteprojekt (BookReviewsWSP) zu öffnen, starten Sie Visual Studio und dann wählen Sie im Menü Datei die Option für die Website öffnen, navigieren Sie zu der `BookReviewsWSP` Ordner auf Ihrem Desktop, und klicken Sie auf OK.


Die übrigen zwei Abschnitte in diesem Lernprogramm betrachten welche Dateien müssen Sie in der produktionsumgebung kopieren, wenn Sie die Anwendung bereitstellen. Die nächsten beiden Lernprogramme -  *[Bereitstellen Ihrer Website mithilfe von FTP](deploying-your-site-using-an-ftp-client-cs.md)*  und  *[Bereitstellen Ihrer Website mit Visual Studio](deploying-your-site-using-visual-studio-cs.md)*  -zeigen verschiedene Verwendungsmöglichkeiten Kopieren Sie diese Dateien auf einen Webhostinganbieter.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Bestimmen die Dateien für das Webprojekt für die Anwendung bereitstellen

Das Webanwendungsprojekt-Modell verwendet die explizite Kompilierung - Quellcode des Projekts in einer einzelnen Assembly jedes Mal, wenn Sie den build der Anwendung kompiliert wird. Diese Kompilierung enthält den ASP.NET-Seiten Code-Behind-Dateien (~ /`Default.aspx.cs`, ~ /`About.aspx.cs`usw.), sowie die `BasePage.cs` Klasse. Die resultierende Assembly BookReviewsWAP.dll heißt und befindet sich in der Anwendungsverzeichnis `Bin` Verzeichnis.

Abbildung 2 zeigt die Dateien, die das Buch Reviews-Webanwendungsprojekt zu bilden.


[![Im Projektmappen-Explorer werden die Dateien, die das Webanwendungsprojekt bilden aufgelistet.](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**Abbildung 2**: der Projektmappen-Explorer werden die Dateien, die das Webanwendungsprojekt bilden aufgelistet.


Zum Bereitstellen einer ASP.NET-Anwendung mithilfe von Beginn Modell Webanwendungsprojekt durch Erstellen der Anwendung, um die neuesten Quellcode explizit in eine Assembly kompilieren entwickelt wurden. Als Nächstes kopieren Sie die folgenden Dateien in der produktionsumgebung:

- Die Dateien, die das deklarative Markup für jede ASP.NET enthalten-Seite, wie z. B. ~ /`Default.aspx`, ~ /`About.aspx`und so weiter. Kopieren Sie außerdem von deklarativem Markup für jede Master Seiten und Benutzersteuerelementen.
- Die Assemblys (`.dll` Dateien) in den `Bin` Ordner. Sie müssen nicht die Programmdateien für die Datenbank zu kopieren (`.pdb`) oder XML-Dateien möglicherweise Sie in finden der `Bin` Verzeichnis.

Sie müssen nicht den ASP.NET-Seiten Quellcodedateien in der produktionsumgebung kopieren, Sie brauchen auch So kopieren Sie die `BasePage.cs` Klassendatei.

> [!NOTE]
> Wie in Abbildung 2 gezeigt, die `BasePage` Klasse wird als eine Klassendatei in das Projekt, und platziert im Ordner "mit dem Namen" implementiert `HelperClasses`. Wenn das Projekt kompiliert wird der Code in der `BasePage.cs` Datei wird zusammen mit den ASP.NET-Seiten Code-Behind-Klassen in der einzelnen Assembly kompiliert `BookReviewsWAP.dll.` ASP.NET verfügt über einen besonderen Ordner mit dem Namen `App_Code` , für das Web-Klassendateien enthalten soll Websiteprojekte. Der Code in der `App_Code` Ordner wird automatisch kompiliert und sollte daher nicht mit Webanwendungsprojekte verwendet werden. Stattdessen, platzieren Sie die Anwendung Klassendateien in einem normalen Ordner namens `HelperClasses`, oder `Classes`, oder einer ähnlichen. Alternativ können Sie ein separates Klassenbibliotheksprojekt Klassendateien versehen.


Zusätzlich zum Kopieren der ASP.NET bezogene Markup-Dateien und die Assembly in die `Bin` Ordner, müssen Sie auch die Unterstützung einer clientseitigen-Dateien - Bilder und CSS-Dateien - sowie die anderen serverseitigen Unterstützungsdateien, kopieren `Web.config` und `Web.sitemap`. Diese Client - und serverseitige unterstützen Dateien müssen in der produktionsumgebung, unabhängig davon, ob Sie explizite oder automatische Kompilierung kopiert werden soll.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Bestimmen die Dateien für die Website-Projektdateien bereitstellen

Das Websiteprojekt-Modell unterstützt die automatische Kompilierung, eine Funktion nicht verfügbar, wenn das Webanwendungsprojekt-Modell verwendet. Mit der expliziten Kompilierung müssen Sie Kompilieren von Quellcode des Projekts in eine Assembly und die Assembly in der produktionsumgebung kopieren. Andererseits, mit der automatische Kompilierung kopieren Sie einfach den Quellcode in der produktionsumgebung und Kompilierung von der Laufzeit bei Bedarf nach Bedarf.

Die Menüoption Build in Visual Studio ist in Webanwendungsprojekten und Websiteprojekten vorhanden. Erstellen eine Webanwendungsprojekte kompiliert das Projekt-Quellcode in einer einzelnen Assembly befindet sich in der `Bin` Verzeichnis, das Erstellen von Websiteprojekt nach Fehlern während der Kompilierung überprüft, aber keine Assemblys erstellt. Zum Bereitstellen einer ASP.NET-Anwendung entwickelt, mit dem Websiteprojekt Modell alle möchten ist Kopie die entsprechenden Dateien in der produktionsumgebung ich empfehle Ihnen ersten Build des Projekts, um sicherzustellen, dass keine Kompilierung auftreten.

Abbildung 3 zeigt die Dateien, die das Buch Websiteprojekt Reviews bilden.


 [![Im Projektmappen-Explorer werden die Dateien, die das Websiteprojekt bilden aufgelistet.](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**Abbildung 3**: der Projektmappen-Explorer werden die Dateien, die das Websiteprojekt bilden aufgelistet.


Bereitstellen eines Projekts für die Website umfasst das Kopieren aller Dateien im Zusammenhang von ASP.NET in der produktionsumgebung -, die das Markup Datenseiten für ASP.NET Seiten, Masterseiten und Benutzersteuerelemente, zusammen mit ihren Codedateien umfasst. Sie müssen auch alle Klassendateien, z. B. BasePage.cs kopieren. Beachten Sie, dass die `BasePage.cs` befindet sich in der `App_Code` Ordner, einen speziellen ASP.NET-Ordner in Websiteprojekten für Klassendateien verwendet. Der besondere Ordner für Produktion, sowie, wie die Klassendateien in erstellt werden muss die `App_Code` Ordner in der Entwicklungsumgebung muss kopiert werden, um die `App_Code` Ordner auf dem Produktionsserver.

Zusätzlich zum Kopieren von ASP.NET Markup und Quellcode Codedateien, müssen Sie auch die Unterstützung einer clientseitigen-Dateien - Bilder und CSS-Dateien - sowie die anderen serverseitigen Unterstützungsdateien, kopieren `Web.config` und `Web.sitemap`.

> [!NOTE]
> Website-Projekte können auch explizite Kompilierung. Ein Lernprogramm zukünftige untersucht wie explizit ein Website-Projekt zu kompilieren.


## <a name="summary"></a>Zusammenfassung

Bereitstellung einer ASP.NET-Anwendung umfasst das Kopieren der erforderlichen Dateien aus der Entwicklungsumgebung in der produktionsumgebung. Der genaue Satz von Dateien, die synchronisiert werden müssen, hängt davon ab, ob die ASP.NET-Anwendung Code explizit oder automatisch kompiliert wird. Die Kompilierung Strategie eingesetzt wird von, ob Visual Studio konfiguriert ist zum Verwalten der ASP.NET-Anwendung über das Webanwendungsprojekt-Modell oder das Websiteprojekt Modell beeinflusst.

Das Webanwendungsprojekt-Modell verwendet die explizite Kompilierung und kompiliert das Projekt Code in einer einzelnen Assembly in den `Bin` Ordner. Bei der Bereitstellung der Anwendung, der Teil von Markup für die ASP.NET-Seiten und den Inhalt von der `Bin` Ordner muss sich in der produktionsumgebung abgelegt werden; in der Anwendung – die Codedateien "und" Code-Behind-Klassen, z. B. - Quellcode ist nicht erforderlich. in der produktionsumgebung kopiert werden soll.

Das Websiteprojekt-Modell verwendet automatische Kompilierung standardmäßig, obwohl es explizit ein Website-Projekt kompilieren möglich ist, da wir in Zukunft Lernprogramme angezeigt wird. Bereitstellen einer ASP.NET-Anwendung, die automatische Kompilierung verwendet, erfordert, dass der Teil Markup *und* Quellcode in der produktionsumgebung kopiert werden muss. Der Code wird automatisch in der produktionsumgebung kompiliert, wenn er zum ersten Mal angefordert wird.

Nachdem wir Elemente geprüft haben, welche Dateien zwischen der Entwicklungs- und produktionsumgebungen synchronisiert werden müssen können wir zur Bereitstellung die Anwendung Buch Reviews auf einen Webhostinganbieter bereit.

Viel Spaß beim Programmieren!

### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Übersicht über die ASP.NET-Kompilierung](https://msdn.microsoft.com/en-us/library/ms178466.aspx)
- [ASP.NET-Benutzersteuerelemente](https://msdn.microsoft.com/en-us/library/y6wb1a0e.aspx)
- [ASP wird untersucht. NET Website-Navigation](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Einführung in Webanwendungsprojekte](https://msdn.microsoft.com/en-us/library/aa730880.aspx)
- [Master-Seite-Lernprogramme](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Freigeben von Code einfach seitenübergreifend](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Verwendung einer benutzerdefinierten Basisklasse für die ASP.NET-Seiten Code-Behind-Klassen](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Visual Studio 2005-Website-Projektsystem: Worum handelt es sich, und warum wir Sie tun?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Exemplarische Vorgehensweise: Konvertieren eines Website-Projekts in ein Webanwendungsprojekt in Visual Studio](https://msdn.microsoft.com/en-us/library/aa983476.aspx)

>[!div class="step-by-step"]
[Zurück](asp-net-hosting-options-cs.md)
[Weiter](deploying-your-site-using-an-ftp-client-cs.md)
