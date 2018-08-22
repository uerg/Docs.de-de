---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Verbesserungen in Visual Studio 2005 | Microsoft-Dokumentation
author: microsoft
description: Visual Studio 2005 bietet Entwickler eine lange Liste der Verbesserungen und Erweiterungen für Webprojekte.
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: 60259ceb99de536410aa5f53db64fb2dca68bf66
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826662"
---
<a name="improvements-in-visual-studio-2005"></a>Verbesserungen in Visual Studio 2005
====================
durch [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 bietet Entwickler eine lange Liste der Verbesserungen und Erweiterungen für Webprojekte.


Visual Studio 2005 bietet Entwickler eine lange Liste der Verbesserungen und Erweiterungen für Webprojekte. So leistungsfähig wie Visual Studio .NET 2002 und 2003 sind, gab es viele Klagen wie Webprojekte verarbeitet wurden. Visual Studio 2005 fügt eine Reihe neuer Features hinzu, um die Adresse diese Beschwerden. Für diejenigen, die die Methode bevorzugen, dass Visual Studio .NET 2003 Kompilierung von Webanwendungen behandelt, finden Sie unter [Web Application Projects](https://go.microsoft.com/fwlink/?LinkId=57870).

Werden Sie in diesem Modul auch Verbesserungen bei der Erstellung des Webprojekts, Verwaltung und Entwicklung. Behandeln Sie in einem späteren-Modul auch Verbesserungen beim Erstellen von Webprojekten und bereitgestellt werden.

## <a name="frontpage-server-extensions"></a>FrontPage-Servererweiterungen

Visual Studio .NET 2002 und 2003 erforderlich, FrontPage-Servererweiterungen auf das Kontrollkästchen zum Erstellen oder Webprojekte erstellen. Entwickler haben die Wahl zwischen zwei verschiedenen Zugriffsmodi (Zugriffsmodus FrontPage Server Extensions "oder"-Datei), beide verwendet FrontPage-Servererweiterungen für Aufgaben wie das Stammverzeichnis der Anwendung in IIS usw. festlegen.

Visual Studio 2005 wird die Abhängigkeit von FrontPage-Servererweiterungen für lokale Projekte entfernt. Visual Studio 2005 greift nun auf die IIS-Metabasis direkt anstelle der FrontPage-Servererweiterungen. Visual Studio 2005 fügt auch Unterstützung für FTP, die für remote-Projektzugriff ermöglicht, ohne die FrontPage-Servererweiterungen.

Für Entwickler, die FrontPage-Servererweiterungen in ihren Projekten verwenden möchten, ist die Möglichkeit weiterhin verfügbar. Allerdings ist basierend auf rückmeldungen aus der ASP.NET Developer Community, es nicht erforderlich.

> [!NOTE]
> FrontPage-Servererweiterungen sind immer noch für remote-Projekt erstellen, öffnen usw. erforderlich.


## <a name="aspnet-development-server"></a>ASP.NET Development Server

Im Lieferumfang von Visual Studio 2005 ist eines neuen Webserver namens ASP.NET Development Server. (Dieser Webserver wurde zuvor als Cassini bezeichnet.)

Es gibt mehrere Vorteile, den ASP.NET Development Server.

- Es ist jetzt möglich, für nicht-Administratoren zum Entwickeln und Debuggen auf einem Webserver.
- Der ASP.NET Development Server ordnet virtuelle Verzeichnisse dynamisch an einem beliebigen Speicherort im Dateisystem ermöglicht flexible Projektspeicherorte.
- Benutzer unter Windows XP Professional, die bereits von IIS mithilfe werden jetzt neue Webanwendungen erstellen, die die Datei oder Ordner-Struktur von die Standardwebsite in IIS nicht beeinträchtigen.

Keine besondere Konfiguration ist erforderlich, um den ASP.NET Development Server nutzen. Wenn ein Webprojekt, die im Dateisystem gehostet wird debuggt oder durchsucht wird, wird Visual Studio 2005 auf einem zufälligen Port für die Anforderung automatisch eine Instanz des ASP.NET Development Server gestartet.

Weitere Informationen werden auf dem ASP.NET Development Server weiter unten in diesem Modul behandelt.

## <a name="improved-file-management"></a>Verbesserte Verwaltung

In Visual Studio 2002 und 2003 gespeichert eine Projektdatei (.vbproj für VB.NET) und csproj für C#-Informationen für alle Dateien in der Webanwendung an. Die Projektmappen-Explorer-Anzeige basiert auf die Informationen in der Projektdatei. Aus diesem Grund visualisierungselements im Projektmappen-Explorer häufig unzutreffenden Informationen in Fällen, in denen Externe Editoren verwendet wurden. Visual Studio 2002 und 2003 würde häufig überschreiben Änderungen der Datenbankdatei oder die neueste Version der Dateien nicht angezeigt.

Visual Studio 2005 wird sofort in der Projektdatei. Stattdessen werden die Datei- und Informationen direkt von der Festplatte, was eine genaue Anzeige der Dateien in Ihrem Projekt gelesen. Da der Ordner "Verweise" im Visual Studio 2002 und 2003 keinen tatsächlichen Ordnernamen in der Webanwendung darstellt, entfernt Visual Studio 2005 auch die Ordner "Verweise" im Projektmappen-Explorer. Um die Verweise für das Projekt in Visual Studio 2005 zuzugreifen, sollten Sie die Eigenschaftenseiten für das Projekt verwenden.

## <a name="creating-web-projects"></a>Erstellen von Webprojekten

Webentwickler haben viele neue Optionen, die für die projekterstellung in Visual Studio 2005. Websites können jetzt erstellt werden an einer beliebigen Stelle im Dateisystem und kann anschließend gedebuggt werden oder durchsucht, mit dem neuen ASP.NET Development Server. Entwickler können auch neuen Websites mithilfe von FTP erstellen.

Klicken Sie hier, um ein video zur exemplarischen Vorgehensweise zum Erstellen von Webprojekten in Visual Studio 2005 anzuzeigen.


![](improvements-in-visual-studio-2005/_static/image1.png)


[Open Vollbild-Video](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>File System-Projekte

In den video zur exemplarischen Vorgehensweise haben Sie gesehen, können Sie Websites auf dem Dateisystem auf dem lokalen Computer oder an einem Remotestandort über eine Dateifreigabe erstellen. Websites, die im Dateisystem erstellt werden, die durchsucht werden und mithilfe der ASP.NET Development Server gedebuggt.

> [!NOTE]
> Der ASP.NET Development Server kann für Kunden verwirrend. Wenn ein Webprojekt im Dateisystem in IISs Verzeichnisstruktur (z. B. "c:" / Inetpub/Wwwroot) erstellt wird, wird die Website weiterhin über den ASP.NET Development Server beim Start von Visual Studio 2005 durchsucht werden. Aus diesem Grund ist eine IIS-Konfiguration (d. h. Authentifizierungsmethoden) nicht anwendbar.


Das Standard-Web-Projekt entfernt auch viele der Aufwand von enthält nur eine Seite "default.aspx" default.cs-Datei und einen Ordner "App/_Data". Die Datei "Web.config" und den Ordner mit Sonderfunktionen (d. h. app/_code) werden hinzugefügt, wenn sie benötigt werden. Das Webprojekt enthält nur die Dateien und Ordner, die Sie benötigen.

### <a name="http-projects"></a>HTTP-Projekte

HTTP-Projekten können Projekte handeln, die auf eine lokale IIS-Website oder auf eine remote-Website erstellt werden. Der Standardspeicherort für das Projekt ist `http://localhost`. Wenn Sie auf die Schaltfläche zum Durchsuchen klicken, stehen Ihnen zwei Optionen für die http-: lokale IIS und Remote-Standortsystemserver. Der Hauptunterschied besteht darin, in diesen beiden Optionen ist die Methode, die in der die Websiteinformationen angezeigt wird, in das Dialogfeld Speicherort auswählen und wie die Dateien an den Webserver kopiert werden.

Die lokale IIS-Option liest die Standortinformationen aus der Metabasis auf dem lokalen Computer aus, und Dateien werden mit dem Dateisystem kopiert. Die Option Remote-Standortsystemserver verwendet werden, die FrontPage-Servererweiterungen und Standortinformationen und Dateien mithilfe von HTTP und FrontPage Server Extensions für RPC-Aufrufe.

> [!NOTE]
> Die vs###/_tmp.htm-Datei, und get/_aspx/_ver.aspx werden nicht mehr verwendet, um Versionsinformationen zu bestimmen.


Die Standardoption für die HTTP-ist die lokale IIS. Diese Option, liest der IIS-Metabasis, um zu bestimmen, welche Websites zur Verfügung stehen und den Speicherort aus, um Inhalte zu erstellen. Sie können einen anderen Ordner oder virtuelle Verzeichnis auswählen, indem Sie ihn in der Strukturansicht auswählen. Sie können auch ein neues virtuelles Verzeichnis erstellen, markieren die Ordner als Anwendungen, sowie Löschen vorhandener virtueller Verzeichnisse in diesem Dialogfeld.


![Der Speicherort-Dialogfeld "auswählen"](improvements-in-visual-studio-2005/_static/image1.gif)

**Abbildung 1**: der Speicherort-Dialogfeld "auswählen"


Anders als in früheren Versionen von Visual Studio, wenn Sie überprüfen die **Secure Sockets Layer verwenden** aktiviert und das SSL-Zertifikat entspricht nicht der URL, die Sie durchsuchen, wird eine Sicherheitswarnung Dialogfeld gefragt werden, ob Sie es tun würden angezeigt Möchten Sie fortfahren. Verwenden Visual Studio .NET 2003 sind, wenn das Zertifikat ein mit einem nicht vorhanden war, würde das Erstellen des Projekts fehlschlagen.


![Security Warnungen in Bezug auf SSL-Zertifikat](improvements-in-visual-studio-2005/_static/image2.gif)

**Abbildung 2**: Security-Warnung in Bezug auf SSL-Zertifikat


### <a name="note-on-host-headers"></a>Hinweis zur Host-Header

Wenn Sie eine Webanwendung auf einer Website, die an eine bestimmte IP-Adresse gebunden erstellen, müssen Sie sicherzustellen, dass ein Hostheaders konfiguriert ist. Visual Studio erstellt, andernfalls die Website unter `http://localhost`, aber die IP-Adresse wird nicht ordnungsgemäß aufgelöst, wenn die Website durchsucht hat oder aus der IDE debuggen.

Wenn Sie die Remote-Standortsystemserver-Option auswählen, ändert sich das Dialogfeld um Sie zur Eingabe der Ziel-URL für die neue Website zu ermöglichen. Diese URL muss auf einem Server, die die FrontPage-Servererweiterungen aktiviert wurde. Wenn Sie mit dem lokalen Webserver mit der FrontPage-Servererweiterungen arbeiten möchten, können Sie verwenden Sie die Remote-Standortsystemserver-Option und geben Sie eine lokale URL.


![Erstellen einer Website auf einem Remoteserver](improvements-in-visual-studio-2005/_static/image1.jpg)

**Abbildung 3**: Erstellen einer Website auf einem Remoteserver


Beim Erstellen einer Anwendung an einem Remotestandort über SSL an, wenn das SSL-Zertifikat nicht übereinstimmt, wird das Dialogfeld "Bestätigung" etwas anders als in dem Dialogfeld angezeigt, wenn die lokale IIS-Option verwenden.


![Die Remote-Standortsystemserver-Sicherheitswarnung](improvements-in-visual-studio-2005/_static/image3.gif)

**Abbildung 4**: die Sicherheitswarnung des Remote-Standortsystemserver


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 wird die Option zum Erstellen von Websites über FTP eingeführt. Wenn Sie diese Option verwenden, wird die IDE die Dateien im temporären Ordner Benutzers lokal erstellt und verwendet dann FTP zum Verschieben der Dateien auf den FTP-Speicherort.

> [!NOTE]
> Speicherort des temporären Ordners ist "c:" / Dokumente und Einstellungen /&lt;Benutzer&gt;/lokale Einstellungen/Temp/VWDWebCache/&lt;Server&gt;/_&lt;Anwendungsname&gt;


Wenn Sie die FTP-Option verwenden zu können, wird ein Speicherort auswählen-Dialogfeld angezeigt werden. Sie geben Sie die erforderlichen FTP-Verbindungsinformationen, in diesem Dialogfeld wie unten dargestellt.


![Die Option für FTP-Speicherort-Dialogfeld](improvements-in-visual-studio-2005/_static/image2.jpg)

**Abbildung 5**: die Option für FTP-Speicherort-Dialogfeld


## <a name="lab-setup-ftp-site-and-create-a-project"></a>Testumgebung: Setup einer FTP-Standort, und erstellen Sie ein Projekt

Die folgenden Schritte aus konfigurieren die FTP-Site so, dass ein Benutzer einen Speicherort verfügt, den nur sie per FTP in hochladen können.

### <a name="install-the-ftp-service"></a>Installieren Sie den FTP-Dienst

1. Öffnen Sie die Software, wählen Sie die Windows-Komponenten hinzufügen/entfernen
2. Wählen Sie die Internet Information Services (Anwendungsserver unter Windows 2003), und klicken Sie auf **Details**.
3. Überprüfen Sie **Protokolls FTP (File Transfer)-Dienst** , und klicken Sie auf **OK**.
4. Klicken Sie auf **Weiter** den FTP-Dienst zu installieren.

### <a name="create-a-new-folder-for-content"></a>Erstellen Sie einen neuen Ordner für Inhalt

1. Erstellen Sie im Windows-Explorer einen neuen Ordner namens **"user1"** in "c:/Inetpub/wwwroot".

#### <a name="configure-folders-and-permissions-on-folders"></a>Konfigurieren Sie die Ordner und Berechtigungen für Ordner.

1. Öffnen Sie das MMC-Snap-in Internet Information Services, über die Verwaltung. Sie verfügen jetzt über einen FTP-Sites-Ordner unter dem Knoten für die Computer Name.
2. Erweitern Sie **FTP-Sites**.
3. Mit der rechten Maustaste die **Standard-FTP-Site**Option **neu**, klicken Sie dann **virtuelles Verzeichnis**, klicken Sie dann auf **Weiter**.
4. Geben Sie **"user1"** für den Namen des virtuellen Verzeichnisses und klicken Sie auf **Weiter**.
5. Geben Sie **c:/Inetpub/Wwwroot/User1** für den Pfad und den auf **Weiter**.
6. Klicken Sie auf **Weiter** und dann **Fertig stellen** um den Assistenten abzuschließen.
7. Mit der rechten Maustaste die **"user1"** virtuellen Verzeichnisses unter Standard-FTP-Site, und wählen **Eigenschaften**.
8. Überprüfen Sie die **schreiben** Kontrollkästchen und klicken Sie auf **OK** um das Dialogfeld zu schließen.
9. Mit der rechten Maustaste **Standard-FTP-Site** , und wählen Sie **Eigenschaften**.
10. Auf der **Sicherheitskonten** Registerkarte aus, deaktivieren Sie **anonyme Verbindungen zulassen**.
11. Klicken Sie auf **Ja** im Dialogfeld gefragt, ob Sie den Vorgang fortsetzen.
12. Klicken Sie auf **OK** um das Dialogfeld zu schließen.
13. Erweitern Sie die **Default Web Site** unter der **Websites** Knoten.
14. Mit der rechten Maustaste die **"user1"** und wählen **Eigenschaften**
15. In der **Anwendungseinstellungen** auf **erstellen** um den Ordner wie eine Anwendung zu markieren.
16. Klicken Sie auf **OK** um das Dialogfeld zu schließen.
17. Schließen Sie das MMC-Snap-in Internet Information Services.

### <a name="create-web-project"></a>Erstellen von Web-Projekt

1. Öffnen Sie Visual Studio 2005.
2. Von der **Datei** , wählen Sie im Menü **neue Website**.
3. In der **Speicherort** Dropdownliste wählen **FTP**.
4. Klicken Sie auf **Durchsuchen**.
5. Geben Sie **"localhost"** in die **Server** Textfeld.
6. Geben Sie **"user1"** in das Textfeld "Directory" ein.
7. Klicken Sie auf **öffnen**. Der FTP-Speicherort wird in das Dialogfeld "neue Website" eingegeben werden.
8. Klicken Sie auf **OK**.
9. Deaktivieren Sie **anonyme Anmeldung** Geben Sie im Dialogfeld FTP-Anmeldung Ihre Anmeldeinformationen ein, und klicken Sie auf **OK**.
10. Wie lautet die URL für das Projekt? (Die URL für das Projekt wird im Projektmappen-Explorer angezeigt werden.)
11. Von der **erstellen** , wählen Sie im Menü **Website erstellen** oder **Projektmappe**.
12. Mit der rechten Maustaste auf "default.aspx" im Projektmappen-Explorer, und wählen Sie **in Browser anzeigen**.
13. Geben Sie im Dialogfeld Website-URL ist erforderlich, ein `http://localhost/user1` für die URL und klicken Sie auf **OK**.

> [!NOTE]
> Wenn Sie einen Fehler enthält, die dadurch entstehen, dass der Typ /_Default laden erhalten, stellen Sie sicher, dass Sie ASP.NET 2.0 auf Ihrer Website und nicht auf eine frühere Version ausführen. Sie können dies tun, auf der Registerkarte ASP.NET in IIS.


## <a name="opening-web-projects"></a>Öffnen von Webprojekten

Öffnen von Webprojekten ist ähnlich wie beim Erstellen von Projekten. In den folgenden Abschnitten aufrufen Bereiche, für die bei der Arbeit in der IDE verfolgen. Dazu gehört auch die Arbeit mit Webprojekten, die über HTTP und FTP.

Wählen Sie zum Öffnen eines Webprojekts Website öffnen, über das Menü Datei. Sie werden aufgefordert, mit dem gleichen Speicherort auswählen-Dialogfeld zuvor behandelt, und Sie haben die gleichen vier Optionen, die Ihnen zur Verfügung: File System, lokalen IIS, FTP und Remote-Standortsystemserver.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Dateisystem

Wie zuvor in diesem Modul angegeben wird, verwendet Visual Studio nicht mehr eine Projektdatei. Wenn Sie eine Website aus dem Dateisystem öffnen möchten, müssen Sie daher tatsächlich einen beliebigen Ordner aus, dem Sie möchten, auswählen, auch wenn der Ordner, den Sie auswählen, nicht als ein Webprojekt in Visual Studio ursprünglich erstellt wurde. Beispielsweise können Sie den Ordner "Eigene Dateien" als eine Website zu öffnen und Visual Studio problemlos öffnen und Anzeigen von Dateien, wie unten dargestellt.


![Eigene Dateien, die als eine Website geöffnet](improvements-in-visual-studio-2005/_static/image3.jpg)

**Abbildung 6**: *Meine Dokumente* als eine Website geöffnet


Da Visual Studio nur zusätzliche Dateien und Ordner bei Bedarf erstellt wird, werden keine zusätzlichen Dateien oder Ordner auf den Speicherort hinzugefügt, die Sie öffnen. Ein Nebeneffekt dieser Architektur ist, dass Sie schachteln von Websites auf dem System verhindert. Betrachten Sie beispielsweise die folgende Verzeichnisstruktur.

Web-Projekt unter "c:" / MyWebSite

Eine andere Web-Projekt unter "c:" / MyWebSite/Nested

Wenn Sie auf der Website unter "c:" / MyWebSite öffnen, wird der Ordner "geschachtelt" als einen untergeordneten Ordner der Anwendung angezeigt.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Beim Öffnen von Websites über HTTP gelesen werden Einstellungen entweder über die IIS-Metabasis (lokale IIS) oder mithilfe von FrontPage-Servererweiterungen (Remote-Standortsystemserver). Wenn geschachtelte Webanwendungen vorhanden sind, werden diese ebenfalls mit einem Symbol, identifiziert sie als Anwendung angezeigt. Wenn Sie bei der Arbeit mit FrontPage-Web-Anwendungen vertraut sind, ähnelt das Verhalten in Visual Studio 2005.

Obwohl Visual Studio ein Symbol für Anwendungen angezeigt wird, die unter der Anwendung geschachtelt sind, die derzeit in der IDE geöffnet wird, können sie nicht erweitert werden, um ihren Inhalt anzuzeigen. Sie können jedoch darauf, um sie zu öffnen doppelklicken. Wenn Sie dies tun, wird ein Dialogfeld dazu aufgefordert, entweder die Webanwendung öffnen (und die aktuell geöffnete Projektmappe zu ersetzen) angezeigt werden, oder Sie fügen Sie der Webanwendung in der aktuellen Lösung hinzu.


![Durch Doppelklicken auf eine geschachtelte Anwendungssymbol bietet Ihnen dieses Dialogfeld](improvements-in-visual-studio-2005/_static/image4.jpg)

**Abbildung 7**: durch Doppelklicken auf eine geschachtelte Anwendungssymbol bietet Ihnen dieses Dialogfeld


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>FTP-Site

Wenn Sie eine Website über FTP öffnen, werden die Dateien alle lokal zum temp-Ordner kopiert. Der vollständige Pfad für den lokalen Speicherort wird im Bereich "Eigenschaften" für das Projekt und mithilfe des folgenden Formats erstellt.

C:/Dokumente und Einstellungen /&lt;Benutzer&gt;/lokale Einstellungen/Temp/VWDWebCache/&lt;Server&gt;/_&lt;Anwendungsname&gt;

Wenn Sie FTP verwenden zu können, müssen Visual Studio die base-URL für Ihr Projekt angeben, sodass Sie sie durchsuchen können, wie unten dargestellt. Wenn Sie eine base-URL nicht angeben, fordert Visual Studio Sie es beim ersten Sie versuchen, eine Seite der Website zu durchsuchen.


![Angeben einer Basis-URL für FTP-Sites](improvements-in-visual-studio-2005/_static/image5.jpg)

**Abbildung 8**: Angeben einer Basis-URL für FTP-Sites


## <a name="improvements-in-compilation"></a>Verbesserungen bei der Kompilierung

Arbeiten mit Webanwendungen in Visual Studio 2005 ist deutlich schneller als in vorherigen Versionen. Dies ist keine kleinen Teil auf die Änderungen in der Kompilierung-Architektur.

In Visual Studio 2002 und 2003 kompiliert wurden die Webanwendungen in einer primären Assembly, die sich im Ordner "/ bin" befinden. In Visual Studio 2005 wurde ein App/_Code-Ordner hinzugefügt. Klassen und anderen nicht-UI-Code in den Ordner der App/_Code hinzugefügt. Wenn Visual Studio das Projekt erstellt wurde, werden alle Dateien im Ordner "App/_Code" in eine einzelne App/_Code.dll-Datei kompiliert. Das Ergebnis dieser Änderung ist, dass nachfolgende Builds wesentlich schneller als in früheren Versionen.

> [!NOTE]
> Die MSBuild-Befehlszeilen-Hilfsprogramm kann auch zum Erstellen von ASP.NET Web-Anwendungen verwendet werden. Dieses Tool wird im Modul 9 abgedeckt werden.


Eine weitere Verbesserung der Kompilierung wird die neue Seite "erstellen"-Option im Menü erstellen. Dieses Feature kann ein Entwickler nur die aktuelle Seite (zusammen mit, Kursen und Abhängigkeiten) neu zu erstellen, damit Änderungen schneller kompiliert werden können. Weil c# keine Hintergrundkompilierung zum Zweck der Aktualisierung von IntelliSense, usw. anbieten, werden sie unglaublich von dieser Funktion profitieren, da er für IntelliSense schnell aktualisiert werden, indem Sie einfach neu erstellen einer einzelnen Seite kann.

Der Build-Eigenschaften für ein Projekt können Sie den Typ des Builds zu konfigurieren, das auftritt, bevor die Startseite ausgeführt wird. Entwickler können auswählen, um nur die aktuelle Seite zu erstellen, damit Visual Studio Debuggen von Anwendungen nach codeänderungen schneller starten können.


![Der Buildvorgang für die Seite starten](improvements-in-visual-studio-2005/_static/image6.jpg)

**Abbildung 9**: der Buildvorgang für die Seite starten


Eine weitere hervorragende Verbesserung von Visual Studio und die Architektur für ASP.NET befindet sich in den Bereich der Bearbeiten und fortfahren. In Visual Studio 2005 können Entwickler mit dem Debuggen eines Projekts beginnen und nehmen Sie codeänderungen vor auf das Projekt, ohne den Debugger trennen. In der Tat fügen Sie buchstäblich Debuggen können ein Projekt eine neue Klasse hinzu, fügen Sie Code auf diese Klasse hinzu, fügen Sie Code hinzu, um die Seite, die eine neue Instanz dieser Klasse erstellt und führen Sie eine Methode der Klasse, ohne den Debugger trennen. Ausführen des neuen Codes ist wirklich so einfach wie das Aktualisieren des Browsers!

Klicken Sie hier, um ein video zur exemplarischen Vorgehensweise der Bearbeitung und Features in Visual Studio 2005 fortgesetzt.


![](improvements-in-visual-studio-2005/_static/image2.png)


[Open Vollbild-Video](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


Die robuste bearbeiten und Fortsetzen von Funktionen in ASP.NET 2.0 und Visual Studio 2005 liegt eine architektonische Änderung für ASP.NET-Anwendungen. In ASP.NET 1.x in Visual Studio 2002/2003 erstellten Anwendungen kompiliert wurden, in einer primären Assembly, die in den Ordner "/ bin" gespeichert wurde. Alle Klassen, Seiten usw. für die Anwendung, eine DLL kompiliert wurden. Klicken Sie dann zur Laufzeit würde ASP.NET Kompilieren aller Steuerelemente, Markup und ASP-Code in Seiten, und kopieren Sie diese DLLs in den temporären ASP.NET-Ordner.

In Visual Studio 2005 mithilfe von ASP.NET 2.0, die Kompilierung von zwei Modelle, sich über (eine für Visual Studio) und eine für ASP.NET zur Laufzeit haben eine allgemeine Kompilierungsmodell zusammengeführt wurde. Das bedeutet, dass alle Kompilierungsprobleme jetzt während der Entwicklungsphase statt zur Laufzeit abgefangen werden. Außerdem können für den Designer und IntelliSense-Unterstützung für Features wie z. B. Benutzersteuerelementen und Masterseiten.

Klicken Sie hier, um ein video zur exemplarischen Vorgehensweise des Designer-Unterstützung für Benutzersteuerelemente finden Sie unter.


![](improvements-in-visual-studio-2005/_static/image3.png)


[Open Vollbild-Video](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> Wenn ein Steuerelement aus einer Seite entfernt wird die @Register Richtlinie bleibt im Markup und müssen manuell entfernt werden, um die Parserfehler zu vermeiden, wenn das Benutzersteuerelement auf der Website gelöscht wird.


Eine weitere Verbesserung in das Kompilierungsmodell von Visual Studio ist das Feature für die Website veröffentlichen. Da die Funktion "Veröffentlichen" führt die Vorkompilierung der Website, können Entwickler die zusätzliche Leistung ohne Kompilieren bei Bedarf nutzen. Er führt die Vorkompilierung auch sämtlichen Quellcode im Ordner "App/_Code" in eine DLL, damit kein Quellcode bereitgestellt werden kann.


![Das Dialogfeld "Website veröffentlichen"](improvements-in-visual-studio-2005/_static/image7.jpg)

**Abbildung 10**: Dialogfeld "Website veröffentlichen"


> [!NOTE]
> Das Hilfsprogramm aspnet/_compile.exe kann auch eine ASP.NET-Webanwendung vorkompiliert verwendet werden. Dieses Tool wird im Modul 9 abgedeckt werden.


Wenn Sie Veröffentlichen einer Website, die vorkompilierten Dateien im Ordner "Temporary ASP.NET Files" gespeichert werden wie unten dargestellt. Dateien mit einem *Compiled* Dateierweiterung sind XML-Dateien, die Abhängigkeiten für bestimmte DLLs zu definieren. Alle Webform bzw. Benutzersteuerelementen in zufälligen DLLs, die mit beginnen kompiliert werden *App /_Web /_*.

Wenn Sie lassen die *aktualisierbarkeit dieser vorkompilierten Site zulassen* aktiviert, Markup innerhalb Ihrer Steuerelemente Webforms und Benutzer werden nicht in der vorkompilierten in eine DLL, sodass Sie nach der Bereitstellung ändern. Wenn Sie das Markup Sperren lieber, sodass Änderungen an der bereitgestellte Inhalt nicht zulässig sind, deaktivieren Sie dieses Kontrollkästchen.

Die *feste benennungen und einzelne Seitenassemblys verwenden* Kontrollkästchen können Sie Batch-Kompilierung zu deaktivieren, sodass jede Seite in einer Assembly mit festen Namen kompiliert wird. Lassen dieses Kontrollkästchen deaktiviert, können Sie Batch-Kompilierung nutzen.

Die *aktivieren starken Namen für vorkompilierte Assemblys* Kontrollkästchen können Sie starke Namen die vorkompilierten Assemblys.

> [!NOTE]
> In ASP.NET 1.x, musste, dass Assemblys mit starkem Namen in den globalen Assemblycache (GAC) installiert werden. In ASP.NET 2.0 sind Sie nicht erforderlich, um Assemblys mit starkem Namen im globalen Assemblycache zu installieren.


![Eine vorkompilierte ASP.NET-Dateien für Anwendungen](improvements-in-visual-studio-2005/_static/image8.jpg)

**Abbildung 11**: eine vorkompilierte ASP.NET-Dateien für Anwendungen


> [!NOTE]
> In der Anwendung, die oben genannten gab es keine Datei "Web.config". Es gab, es würde aufgerufen wurden *"PrecompiledApp.config"* nach "Web veröffentlichen" site verarbeiten.


## <a name="improvements-in-deployment"></a>Verbesserungen bei der Bereitstellung

Als bietet Visual Studio 2005 mit Visual Studio 2002 und 2003, ein Feature Projekt kopieren. Allerdings das Feature wurde wurde verbesserten in Visual Studio 2005 und heißt jetzt Website kopieren.

Das Dialogfeld "Website kopieren" wird in einem linken Rahmen und ein rechtes Frame unterteilt. Im linken Bereich wird der Quellwebsite Rechte Frame aufgerufen und der Remotewebsite. Eine Sache, die einige Entwickler verwirren könnte ist, dass die Website, die im rechten Bereich angezeigt, nicht unbedingt einem Remotestandort ist. Es kann eine Website, auf dem lokalen Dateisystem oder auf der lokalen Instanz von IIS handeln. Darüber hinaus die Website im linken Bereich angezeigt, ist nicht unbedingt der Quellwebsite weil im Dialogfeld, die Sie von der remote-Website veröffentlichen können *zu* der Quellwebsite.

Wenn Sie ein Projekt in einer Remotewebsite kopieren, müssen diesem Standort die FrontPage-Servererweiterungen installiert ist. Wenn dies nicht der Fall ist, müssen Sie über FTP eine Verbindung herstellen. Wenn Sie ein Projekt mit der lokalen IIS-Instanz kopieren, sind die FrontPage-Servererweiterungen auf der anderen Seite nicht erforderlich.

> [!NOTE]
> Wenn Sie versuchen, eine neue Website für die lokale IIS-Instanz zu erstellen, und die FrontPage 2002 Servererweiterungen installiert sind, erhalten Sie eine Fehlermeldung angezeigt, die besagt, dass das Erstellen von Websites auf einem SharePoint-Server nicht unterstützt wird. In diesem Fall müssen Sie die Möglichkeit, die FrontPage 2000 Server-Erweiterungen installieren oder entfernen die FrontPage-Servererweiterungen.


Klicken Sie hier, um ein video zur exemplarischen Vorgehensweise der Funktion "Website kopieren".


![](improvements-in-visual-studio-2005/_static/image4.png)


[Open Vollbild-Video](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>Verbesserungen beim Debuggen

Es gibt vier wichtige Verbesserungen im Debuggen in Visual Studio 2005.

- Lokales Debuggen, als Nichtadministrator ist standardmäßig nicht möglich.
- Das Debug-Attribut für das Compilation-Element ist jetzt standardmäßig "false".
- Für das Remotedebuggen-Setup und Konfiguration ist einfacher als zuvor.
- Sie können nun eine Website, die über eine FTP-Speicherort geöffnet Debuggen.

## <a name="debugging-as-a-non-administrator"></a>Debuggen als nicht-Administrator

Das Hinzufügen von ASP.NET Development Server kann nicht-Administratoren problemlos Debuggen von ASP.NET-Anwendungen anwendungstelemetriedaten. Beim Debuggen einer ASP.NET-Anwendung auf dem lokalen Dateisystem ausgeführt wird, startet Visual Studio den ASP.NET Development Server im Kontext des angemeldeten Benutzers. Dieser Benutzer kann anschließend die Anwendung ohne zusätzliche Konfiguration Debuggen.

## <a name="debug-is-false-by-default"></a>"Debug" ist "false" werden standardmäßig

In ASP.NET 1.x, die *Debuggen* -Attribut in der *Kompilierung* -Element der Datei "Web.config" wurde festgelegt, um *"true"* standardmäßig. Es hat immer wurde empfohlen, dass Entwickler dieses Attribut auf *"false"* vor der Bereitstellung einer Anwendung für die Produktion, aber da die meisten Entwickler die Folgen der Beibehaltung von der Debug-Attributsatz nicht vollständig verstehen True gibt an, sie einfach von links als-ist.

Das schwerwiegendste Problem mit der Debug-Attribut auf True festgelegt ist, dass der Batch-Kompilierungsmodell ASP.NETs deaktiviert. Daher wird jede Seite in einer separaten DLL kompiliert. Wenn ein Webdienst, die Anwendung aus Tausenden von Seiten (nicht hier über beispiellose Möglichkeiten der keinesfalls) besteht, das bedeutet, dass werden von dieser Anwendung mehrere Tausend kleine DLLs erstellt. Während diese DLLs nur eine geringe Größe sind, sind sie nicht in die bestimmte Position im Arbeitsspeicher geladen. Aus diesem Grund zur Fragmentierung führen, im Arbeitsspeicher und seinen möglichen Beitrag zu OutOfMemoryException vorkommen.

In ASP.NET 2.0 ist das Debug-Attribut standardmäßig auf "false" festgelegt. Wie Sie sich bereits gesehen haben, wenn ein Entwickler eine ASP.NET-Anwendung in Visual Studio 2005 vorgenommen, wird er aufgefordert, eine Datei "Web.config" mit aktiviertem Debuggen hinzufügen. Auf diese Weise fallen die gleichen Nachteile, die in ASP.NET vorhanden waren 1.x, aber nun der Entwickler wird deutlich, dass das Attribut auf "false" zurückgesetzt werden soll, vor dem Verschieben der Anwendung für die Produktion gewarnt.

## <a name="remote-debugging-setup-and-configuration"></a>Remote Debugging – Setup und Konfiguration

In Visual Studio 2002/2003, verlassen auf dem Computer Debug-Manager (mdm.exe) und den vs7jit.exe des Remotedebuggens. Aus diesem Grund remote Debuggen Problembehandlung wurde häufig eine Blackbox für Kunden, und dies war häufig nicht viel besser für PSS.

Visual Studio 2005 wird die Abhängigkeit von den Prozessen mdm.exe und vs7jit.exe entfernt. Stattdessen wird jetzt den Remote Debug Monitor-Dienst (msvsmon.exe).

Die Anforderung für das Debuggen in Visual Studio 2005 Remote ist recht einfach. Sie müssen führen Sie msvsmon.exe auf dem Remoteserver vor dem Debuggen. Können Sie die Remote Debug Monitor aus der Visual Studio-CD installieren oder Sie können einfach msvsmon.exe aus einer Freigabe ausführen, ohne dass etwas überhaupt auf dem Webserver installiert.

Wenn Sie auf "msvsmon.exe" ausführen, ist es wahrscheinlich, dass es zu Ports, die für das Remotedebugging blockiert beschweren wird. Glücklicherweise können Sie einfach die Ports aus direkt in das Dialogfeld "Warnung" zulassen, wie unten dargestellt.


![Benachrichtigung, dass Windows-Firewall blockiert das Remotedebuggen ist.](improvements-in-visual-studio-2005/_static/image9.jpg)

**Abbildung 12**: Benachrichtigung, dass Windows-Firewall ist Remotedebuggen blockieren


Nachdem Sie die Ports für das Debuggen Blockierung aufgehoben wurde, sehen Sie den Remotedebugmonitor, wie unten dargestellt. Von dieser Schnittstelle können Verbindungen überwachen und leicht Debuggen von Berechtigungen zu ändern.


![Den Remotedebugmonitor](improvements-in-visual-studio-2005/_static/image10.jpg)

**Abbildung 13**: des Remotedebugmonitors


Es ist auch möglich, eine Webanwendung, die geöffnet wird, über FTP Remote zu debuggen. Die Schritte sind identisch mit denen zuvor beschrieben. Allerdings müssen Sie eine base-URL angeben, für das Durchsuchen des FTP-Projekts, wie weiter oben in diesem Modul erläutert.

## <a name="lab-2"></a>Übungseinheit 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Remotedebuggen mit Visual Studio 2005

Dieses Lab führt Sie durch das Remotedebuggen mit Visual Studio 2005.

Klicken Sie hier, um eine Videodemonstration dieser Anleitung.


![](improvements-in-visual-studio-2005/_static/image5.png)


[Open Vollbild-Video](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


Diese Übungseinheit benötigen Sie zwei Computer, einen ausgeführten Visual Studio 2005 und die anderen ausgeführten IIS 5 oder höher.

1. Öffnen Sie Visual Studio 2005 und erstellen Sie eine neue Website auf dem Remoteserver.

> [!NOTE]
> Sie können die Website auf einer Remoteinstanz von IIS oder per FTP erstellen.


1. Suchen Sie von der remote-Webserver msvsmon.exe, auf dem Entwicklungscomputer, die über einen UNC-Pfad, und führen Sie es.  
 Der Standardspeicherort für msvsmon.exe ist //server/c$/Program Programme/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/X86.
2. Wenn Sie dazu aufgefordert werden, um die Blockierung von Ports für das Remotedebuggen aufheben, dazu.
3. Öffnen Sie den Code-Behind für "default.aspx", und legen Sie einen Haltepunkt in der Seite/_laden-Methode, aus dem Entwicklungscomputer.
4. Starten des Debuggen von den Entwicklungscomputer.

Sie sollten beim Haltepunkt ankommen, wie erwartet.

## <a name="aspnet-development-server"></a>ASP.NET Development Server

Als sechsteilige bereits erläutert ist Visual Studio 2005 mit einem Webserver, der Namen der ASP.NET Development Server enthalten. (ASP.NET Development Server wird manchmal als Cassini bezeichnet.) Dieser Webserver ist eine praktische Möglichkeit zum Durchsuchen und zu Debuggen von Webanwendungen, die im Dateisystem ausgeführt werden.

Der ASP.NET Development Server ist ein Webserver beschränkt. Remoteverbindungen ist nicht zulässig, Anforderungen von Benutzern als der Benutzer, die der Webserver gestartet ist nicht zulässig. Es muss auch nicht die Möglichkeit, ASP-Seiten bereitstellt. Nur ASP.NET-Ressourcen und HTML-Ressourcen (einschließlich der Bilder, CSS-Dateien usw.) verarbeitet werden.

Der ASP.NET Development Server kann über die Befehlszeile gestartet werden, durch Ausführen der Datei WebDev.WebServer.exe c:/Windows/Microsoft.NET/Framework/v2.0./*/* /  */*/*. Das folgende Dialogfeld zeigt die Parameter, die verfügbar sind.


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Abbildung 14:**


> [!NOTE]
> Der ASP.NET Development Server wird nicht unterstützt, wenn explizit über die Befehlszeile gestartet wird.
