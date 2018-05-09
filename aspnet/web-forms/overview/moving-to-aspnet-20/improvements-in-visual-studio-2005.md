---
uid: web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
title: Verbesserungen in Visual Studio 2005 | Microsoft Docs
author: microsoft
description: Visual Studio 2005 stellt eine lange Liste von Verbesserungen und Erweiterungen für Webprojekte Web-Anwendungsentwickler.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 72d90cd0-b3d9-454c-b2eb-ed0d9812f32c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/improvements-in-visual-studio-2005
msc.type: authoredcontent
ms.openlocfilehash: aafc59980e807677d6023110d324365ce92bb5fc
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/05/2018
---
<a name="improvements-in-visual-studio-2005"></a>Verbesserungen in Visual Studio 2005
====================
durch [Microsoft](https://github.com/microsoft)

> Visual Studio 2005 stellt eine lange Liste von Verbesserungen und Erweiterungen für Webprojekte Web-Anwendungsentwickler.


Visual Studio 2005 stellt eine lange Liste von Verbesserungen und Erweiterungen für Webprojekte Web-Anwendungsentwickler. So leistungsfähig wie Visual Studio .NET 2002 und 2003 sind, wurden viele Beschwerden wie Webprojekte verarbeitet wurden. Visual Studio 2005 fügt eine Reihe neuer Features hinzu, um diese Beschwerden zu beheben. Für die Benutzer die Möglichkeit zu bevorzugen, dass Visual Studio .NET 2003 Kompilierung von Webanwendungen behandelt, finden Sie unter [Webanwendungsprojekte](https://go.microsoft.com/fwlink/?LinkId=57870).

In diesem Modul behandelt Sie gut Verbesserungen bei der Erstellung des Webprojekts, Verwaltungs- und Entwicklungstools. Decken Sie in einem späteren Modul gut Verbesserungen bei der Erstellung von Webprojekten und deren Bereitstellung.

## <a name="frontpage-server-extensions"></a>FrontPage-Servererweiterungen

Visual Studio .NET 2002 und 2003 erforderlich, die FrontPage-Servererweiterungen auf das Feld zu erstellen oder Webprojekte zu erstellen. Entwickler haben die Wahl zwischen zwei verschiedenen Zugriffsmodi (Zugriffsmodus FrontPage-Servererweiterungen oder Datei), beide FrontPage-Servererweiterungen verwendet, um Aufgaben wie das Stammverzeichnis der Anwendung in IIS usw. festlegen.

Visual Studio 2005 entfernt die Abhängigkeit von FrontPage-Servererweiterungen für lokalen Projekte. Visual Studio 2005 greift auf die IIS-Metabasis direkt statt die FrontPage-Servererweiterungen. Visual Studio 2005 bietet nun auch Unterstützung für FTP-Remoteserver Projektzugriff ermöglicht eine ohne FrontPage-Servererweiterungen.

Für Entwickler mit FrontPage-Servererweiterungen in ihren Projekten verwenden möchten, ist die Option trotzdem verfügbar. Allerdings ist basierend auf starke Feedback finden Sie in der Entwicklercommunity ASP.NET, es nicht erforderlich.

> [!NOTE]
> Die FrontPage-Servererweiterungen sind weiterhin für remote projekterstellung "," Öffnen "" usw. erforderlich.


## <a name="aspnet-development-server"></a>ASP.NET Development Server

Im Lieferumfang von Visual Studio 2005 ist eines neuen Web Servers ASP.NET Development Server aufgerufen. (Diese Web-Server wurde zuvor als Cassini bezeichnet.)

Es gibt mehrere Vorteile, den ASP.NET Development Server.

- Es ist jetzt möglich für nicht-Administratoren zum Entwickeln und Debuggen auf einem Webserver.
- Der ASP.NET Development Server ordnet virtuelle Verzeichnisse dynamisch an einem beliebigen Speicherort im Dateisystem für flexible Projektverzeichnisse ermöglicht.
- Benutzer unter Windows XP Professional, die bereits IIS verwenden, werden neue Webanwendungen erstellen, die die Datei oder einen Ordner-Struktur von die Standardwebsite in IIS nicht auswirkt.

Keine spezielle Konfiguration ist erforderlich, den ASP.NET Development Server nutzen. Wenn ein Webprojekt, die im Dateisystem gehostet wird gedebuggt oder durchsucht wird, startet Visual Studio 2005 eine Instanz von ASP.NET Development Server automatisch auf einen zufälligen Port, um die Anforderung zu erfüllen.

Weitere Informationen werden auf dem ASP.NET Development Server weiter unten in diesem Modul behandelt.

## <a name="improved-file-management"></a>Verbesserte Verwaltung

In Visual Studio 2002 und 2003 gespeichert eine Projektdatei (.vbproj für VB.NET) und csproj für C#-Informationen für alle Dateien in der Webanwendung an. Die Projektmappen-Explorer-Anzeige basiert darauf, dass die Dateiinformationen in der Projektdatei. Aus diesem Grund wird im Projektmappen-Explorer häufig ungenaue Informationen in Fällen angezeigt, auf dem externen Editoren verwendet wurden. Visual Studio 2002 und 2003 würde häufig Änderungen der Datenbankdatei überschrieben oder nicht die neueste Version der Dateien angezeigt.

Visual Studio 2005 wird sofort mit der Projektdatei. Stattdessen wird die Datei- und Ordnerberechtigungen Informationen direkt von der Festplatte, was zu einer genau Anzeige der Dateien in Ihrem Projekt gelesen. Da der Ordner "Verweise" im Visual Studio 2002 und 2003 keinen tatsächlichen Ordner in der Web-Anwendung darstellt, entfernt Visual Studio 2005 auch die Ordner "Verweise" im Projektmappen-Explorer. Um die Verweise für das Projekt in Visual Studio 2005 zuzugreifen, sollten Sie die Eigenschaftenseiten für das Projekt verwenden.

## <a name="creating-web-projects"></a>Erstellen von Webprojekten

Webentwickler haben viele neue Optionen, die für die projekterstellung in Visual Studio 2005. Websites können jetzt erstellt werden überall im Dateisystem und können dann gedebuggt werden oder mithilfe von neuen ASP.NET Development Server durchsucht. Entwickler können auch neuen Websites über FTP erstellen.

Klicken Sie hier, um ein video zur exemplarischen Vorgehensweise erstellen Sie Webprojekte in Visual Studio 2005 anzuzeigen.


![](improvements-in-visual-studio-2005/_static/image1.png)


[Open Vollbild-Video](improvements-in-visual-studio-2005/_static/creating_projects1.wmv)


### <a name="file-system-projects"></a>Datei-System-Projekte

In dem video zur exemplarischen Vorgehensweise haben Sie gesehen, können Sie auswählen, Websites auf dem Dateisystem auf dem lokalen Computer oder an einem Remotestandort über eine Dateifreigabe zu erstellen. Websites, die im Dateisystem erstellt werden, die durchsucht werden und debuggt mithilfe von ASP.NET Development Server.

> [!NOTE]
> Der ASP.NET Development Server möglicherweise etwas Verwirrung für Kunden. Wenn Sie ein Webprojekt auf dem Dateisystem in IISs Verzeichnisstruktur (d. h. "c:" / Inetpub/Wwwroot) erstellt wird, wird der Website weiterhin über den ASP.NET Development Server, wenn Sie in Visual Studio 2005 gestartet durchsucht werden. Aus diesem Grund ist eine IIS-Konfiguration (d. h. Authentifizierungsmethoden) nicht anwendbar.


Das Standard-Web-Projekt entfernt auch viel der Aufwand von enthält nur eine Seite "default.aspx" default.cs-Datei und einen Ordner "App/_Data". Die Datei "Web.config" und die spezielle Ordner (d. h. app/_code) werden hinzugefügt, wie sie benötigt werden. Das Webprojekt enthält nur die Dateien und Ordner, die Sie benötigen.

### <a name="http-projects"></a>HTTP-Projekte

HTTP-Projekte können entweder Projekte sein, die auf eine lokale IIS-Website oder auf einer remoten Website erstellt werden. Der Standardspeicherort für das Projekt ist `http://localhost`. Wenn Sie die Schaltfläche zum Durchsuchen klicken, stehen zwei HTTP-Optionen: lokale IIS und Remote-Standortsystemserver. Der Hauptunterschied in dieser beiden Optionen wird die Methode, in der die Website-Informationen angezeigt werden, in das Dialogfeld "Speicherort auswählen" und wie die Dateien an den Webserver kopiert werden.

Die Option lokale IIS liest die Standortinformationen aus der Metabase auf dem lokalen Computer und Dateien werden mit dem Dateisystem kopiert. Die Remote-Standortsystemserver-Option verwendet die FrontPage-Servererweiterungen und die Standortinformationen und Kennwortdateien über HTTP und FrontPage Server Extensions RPC aufruft.

> [!NOTE]
> Vs###/_tmp.htm Datei- und get/_aspx/_ver.aspx werden nicht mehr verwendet, um Versionsinformationen zu bestimmen.


Die Standardoption für die HTTP-ist die lokale IIS. Diese Option liest die IIS-Metabasis, um zu bestimmen, welche Websites zur Verfügung stehen und den Speicherort der Inhalte zu erstellen. Sie können einen anderen Ordner oder das virtuelle Verzeichnis auswählen, indem Sie sie in der Strukturansicht auswählen. Sie können auch ein neues virtuelles Verzeichnis erstellen, markieren die Ordner als Anwendungen sowie Löschen vorhandener virtueller Verzeichnisse in diesem Dialogfeld.


![Das Dialogfeld Speicherort auswählen](improvements-in-visual-studio-2005/_static/image1.gif)

**Abbildung 1**: der Speicherort-Dialogfeld auswählen


Anders als in früheren Versionen von Visual Studio, wenn Sie eine Überprüfung auf die **verwenden Secure Sockets Layer** CheckBox- und das SSL-Zertifikat entspricht nicht der URL, die Sie durchsuchen, es wird ein Sicherheitshinweis Dialogfeld gefragt werden, ob Sie würden angezeigt werden Möchten Sie den Vorgang fortzusetzen. Verwenden Visual Studio .NET 2003 sind, wenn das Zertifikat ein mit einem nicht war, würde das Erstellen des Projekts fehlschlagen.


![Sicherheit Warnung zur SSL-Zertifikat](improvements-in-visual-studio-2005/_static/image2.gif)

**Abbildung 2**: Sicherheitswarnung bezüglich der SSL-Zertifikat


### <a name="note-on-host-headers"></a>Hinweis zur Host-Header

Wenn Sie eine Webanwendung auf einer Website an eine bestimmte IP-Adresse gebunden erstellen, müssen Sie stellen Sie sicher, dass ein Hostheader konfiguriert ist. Visual Studio erstellen Sie andernfalls die-Website unter `http://localhost`, aber die IP-Adresse wird nicht ordnungsgemäß aufgelöst, wenn die Website durchsucht hat oder aus der IDE debuggen.

Wenn Sie die Option Remote-Standortsystemserver auswählen, ändert das Dialogfeld zur Eingabe der Ziel-URL für die neue Website zu erlauben. Diese URL muss auf einem Server, die die FrontPage-Servererweiterungen aktiviert wurde. Wenn Sie mit dem lokalen Webserver mit FrontPage-Servererweiterungen arbeiten möchten, können Sie mithilfe der Option Remote-Standortsystemserver und eine lokale URL angeben.


![Erstellen einer Website auf einem Remoteserver](improvements-in-visual-studio-2005/_static/image1.jpg)

**Abbildung 3**: Erstellen einer Website auf einem Remoteserver


Wenn beim Erstellen einer Anwendung an einem Remotestandort über SSL, das SSL-Zertifikat nicht übereinstimmt, ist Bestätigungsdialogfeld unterscheidet sich etwas im Dialogfeld angezeigt, wenn die lokale IIS-Option verwenden.


![Die Remote-Standortsystemserver-Sicherheitswarnung](improvements-in-visual-studio-2005/_static/image3.gif)

**Abbildung 4**: die Remote-Standortsystemserver-Sicherheitswarnung


<a id="_Toc116100243"></a>

#### <a name="ftp"></a>FTP

Visual Studio 2005 stellt die Option zum Erstellen von Websites über FTP. Wenn Sie diese Option verwenden, wird die IDE die Dateien im temp-Ordner Benutzers lokal erstellt und anschließend FTP verwendet, um die Dateien im FTP-Speicherort zu verschieben.

> [!NOTE]
> Speicherort des temporären Ordners ist "c:" / Documents and Settings /&lt;Benutzer&gt;/lokale Einstellungen/Temp/VWDWebCache/&lt;Server&gt;/_&lt;Anwendungsname&gt;


Wenn Sie die FTP-Option verwenden zu können, wird ein Dialogfeld Speicherort auswählen angezeigt. Geben Sie die erforderlichen Verbindungsinformationen für den FTP-, in diesem Dialogfeld an, wie unten dargestellt.


![Die Option für FTP-Speicherort-Dialogfeld](improvements-in-visual-studio-2005/_static/image2.jpg)

**Abbildung 5**: die Option für FTP-Speicherort-Dialogfeld


## <a name="lab-setup-ftp-site-and-create-a-project"></a>Testlabors: Einrichtung des FTP Standort, und erstellen Sie ein Projekt

Die folgenden Schritte konfigurieren die FTP-Site so, dass ein Benutzer einen Speicherort verfügt, den nur über FTP auf hochladen können.

### <a name="install-the-ftp-service"></a>Installieren Sie den FTP-Dienst

1. Öffnen Sie die Software, wählen Sie die Windows-Komponenten hinzufügen/entfernen
2. Wählen Sie Internet Information Services (Anwendungsserver unter Windows 2003), und klicken Sie auf **Details**.
3. Überprüfen Sie **Protokoll FTP (File Transfer)-Dienst** , und klicken Sie auf **OK**.
4. Klicken Sie auf **Weiter** an den FTP-Dienst zu installieren.

### <a name="create-a-new-folder-for-content"></a>Erstellen Sie einen neuen Ordner für Inhalt

1. Erstellen Sie im Windows-Explorer einen Ordner mit dem Namen **"user1"** in "c:" / Inetpub/Wwwroot.

#### <a name="configure-folders-and-permissions-on-folders"></a>Konfigurieren Sie die Ordner und Berechtigungen für Ordner.

1. Öffnen Sie das MMC-Snap-in Internetinformationsdienste (IIS), von der Verwaltung. Sie verfügen jetzt über eine FTP-Sites Ordner unter dem Computerknoten-Name.
2. Erweitern Sie **FTP-Sites**.
3. Mit der rechten Maustaste die **Default FTP-Site**Option **neu**, klicken Sie dann **virtuelles Verzeichnis**, klicken Sie dann auf **Weiter**.
4. Geben Sie **"user1"** für den Namen des virtuellen Verzeichnisses und klicken Sie auf **Weiter**.
5. Geben Sie **"c:" / Inetpub/Wwwroot/User1** für den Pfad und klicken Sie auf **Weiter**.
6. Klicken Sie auf **Weiter** und dann **Fertig stellen** um den Assistenten abzuschließen.
7. Mit der rechten Maustaste die **"user1"** virtuelles Verzeichnis unter Default FTP-Site, und wählen **Eigenschaften**.
8. Überprüfen Sie die **schreiben** Kontrollkästchen und klicken Sie auf **OK** um das Dialogfeld zu schließen.
9. Mit der rechten Maustaste **Default FTP-Site** , und wählen Sie **Eigenschaften**.
10. Auf der **Sicherheitskonten** Registerkarte aus, deaktivieren Sie **anonyme Verbindungen zulassen**.
11. Klicken Sie auf **Ja** im Dialogfeld gefragt, ob Sie den Vorgang fortsetzen.
12. Klicken Sie auf **OK** um das Dialogfeld zu schließen.
13. Erweitern Sie die **Default Web Site** unter der **Websites** Knoten.
14. Mit der rechten Maustaste die **"user1"** Verzeichnis, und wählen Sie **Eigenschaften**
15. In der **Anwendungseinstellungen** auf **erstellen** um den Ordner wie eine Anwendung zu markieren.
16. Klicken Sie auf **OK** um das Dialogfeld zu schließen.
17. Schließen Sie das MMC-Snap-in Internetinformationsdienste (IIS).

### <a name="create-web-project"></a>Webprojekt erstellen

1. Open Visual Studio 2005.
2. Aus der **Datei** klicken Sie im Menü **neue Website**.
3. In der **Speicherort** Dropdownliste wählen **FTP**.
4. Klicken Sie auf **Durchsuchen**.
5. Geben Sie **"localhost"** in der **Server** Textfeld.
6. Geben Sie **"user1"** in das Textfeld "Directory" ein.
7. Klicken Sie auf **öffnen**. Der FTP-Speicherort wird in das Dialogfeld "neue Website" eingegeben werden.
8. Klicken Sie auf **OK**.
9. Deaktivieren Sie **Anonymous anmelden** im Dialogfeld "FTP-Anmeldung" Geben Sie Ihre Anmeldeinformationen ein, und klicken Sie auf **OK**.
10. Was ist die URL für das Projekt? (Die URL für das Projekt wird im Projektmappen-Explorer angezeigt werden.)
11. Aus der **erstellen** klicken Sie im Menü **Website erstellen** oder **Projektmappe**.
12. Mit der rechten Maustaste auf "default.aspx" im Projektmappen-Explorer, und wählen Sie **in Browser anzeigen**.
13. Geben Sie im Dialogfeld "Website-URL ist erforderlich" `http://localhost/user1` für die URL und auf **OK**.

> [!NOTE]
> Wenn Sie einen Fehler, der angibt, eines Fehler beim Laden der Typ /_Default erhalten, stellen Sie sicher, dass Sie ASP.NET 2.0 auf Ihrer Website und nicht von einer früheren Version ausgeführt werden. Sie können über die Registerkarte ASP.NET in Internetinformationsdienste (IIS) vornehmen.


## <a name="opening-web-projects"></a>Webprojekte öffnen

Öffnen Webprojekte ähnelt der Projekte erstellen. In den folgenden Abschnitten rufen Punkte zu haben, für die während der Arbeit in der IDE. Außerdem werden behandelt, arbeiten mit Webprojekte über HTTP und FTP.

Wählen Sie zum Öffnen eines Webprojekts Website öffnen, über das Menü Datei. Sie werden aufgefordert, mit dem gleichen Speicherort auswählen-Dialogfeld zuvor behandelt und die gleichen vier Optionen zur Verfügung: File System, lokale IIS, FTP und Remote-Standortsystemserver.

<a id="_Toc116100245"></a>

## <a name="file-system"></a>Dateisystem

Wie zuvor in diesem Modul angegeben wird, verwendet Visual Studio nicht mehr eine Projektdatei aus. Falls gewünscht, um eine Website aus dem Dateisystem zu öffnen, müssen Sie daher, tatsächlich einen beliebigen Ordner aus, dem Sie möchten, auswählen, auch wenn der Ordner, den Sie auswählen, nicht als ein Webprojekt in Visual Studio anfänglich erstellt wurde. Beispielsweise können Sie auswählen, zum Ordner "Eigene Dokumente" als eine Website zu öffnen, und Visual Studio problemlos öffnen und Anzeigen von Dateien, wie unten dargestellt.


![Meine Dokumente geöffnet, wie eine Website](improvements-in-visual-studio-2005/_static/image3.jpg)

**Abbildung 6**: *Meine Dokumente* als eine Website geöffnet


Da Visual Studio nur zusätzliche Dateien und Ordner bei Bedarf erstellt, werden keine zusätzlichen Dateien oder Ordner auf den Speicherort hinzugefügt, die Sie öffnen. Ein Nebeneffekt dieser Architektur ist, dass Sie schachteln von Websites auf dem Dateisystem verhindert. Betrachten Sie beispielsweise die folgende Verzeichnisstruktur.

Webprojekt unter "c:" / MyWebSite

Eine andere Webprojekt unter "c:" / MyWebSite/Nested

Wenn Sie auf der Website unter "c:" / MyWebSite öffnen, wird die geschachtelte Ordner als Unterordner der betreffenden Anwendung angezeigt.

<a id="_Toc116100246"></a>

## <a name="http"></a>HTTP

Wenn Sie Websites über HTTP zu öffnen, werden Einstellungen über die IIS-Metabasis (lokale IIS) oder mithilfe von FrontPage-Servererweiterungen (Remotesite) gelesen Wenn geschachtelte Webanwendungen sind, werden diese ebenfalls mit einem Symbol identifiziert werden, wie eine Anwendung angezeigt. Wenn Sie bei der Arbeit mit Webanwendungen in FrontPage vertraut sind, entspricht das Verhalten in Visual Studio 2005.

Obwohl Visual Studio ein Symbol für Anwendungen angezeigt werden, die unterhalb der Anwendung geschachtelt sind, die derzeit in der IDE geöffnet wird, können sie nicht erweitert werden, um ihren Inhalt anzuzeigen. Sie können jedoch darauf, um sie zu öffnen doppelklicken. In diesem Fall wird ein Dialogfeld dazu aufgefordert, entweder Öffnen Sie die Webanwendung (und die aktuell geöffnete Projektmappe ersetzen) angezeigt werden, oder Sie die Web-Anwendung der aktuellen Projektmappe hinzuzufügen.


![Durch Doppelklicken auf ein Symbol für geschachtelte Anwendung bietet Ihnen in diesem Dialogfeld](improvements-in-visual-studio-2005/_static/image4.jpg)

**Abbildung 7**: durch Doppelklicken auf ein Symbol für geschachtelte Anwendung bietet Ihnen in diesem Dialogfeld


<a id="_Toc116100247"></a>

## <a name="ftp-site"></a>FTP-Site

Wenn Sie eine Website über FTP öffnen, werden die Dateien alle lokal auf Ihrem Ordner "temp" kopiert. Der vollständige Pfad für den lokalen Speicherort im Bereich "Eigenschaften" für das Projekt angezeigt, und es wird im folgenden Format erstellt.

"C:" / Documents and Settings /&lt;Benutzer&gt;/lokale Einstellungen/Temp/VWDWebCache/&lt;Server&gt;/_&lt;Anwendungsname&gt;

Wenn Sie FTP verwenden, müssen Visual Studio die base-URL für Ihr Projekt angeben, sodass Sie sie durchsuchen können, wie unten dargestellt. Wenn Sie eine base-URL nicht angeben, wird Visual Studio Sie dafür erstmalig bitten Sie versuchen, eine Seite in der Website zu durchsuchen.


![Eine Base-URL angeben für FTP-Sites](improvements-in-visual-studio-2005/_static/image5.jpg)

**Abbildung 8**: Angeben einer Basis-URL für FTP-Sites


## <a name="improvements-in-compilation"></a>Verbesserungen bei der Kompilierung

Arbeiten mit Webanwendungen in Visual Studio 2005 ist deutlich schneller als in vorherigen Versionen. Dies ist in keine kleinen Änderungen in der Kompilierung Architektur fällig.

In Visual Studio 2002 und 2003 kompiliert wurden Webanwendungen in eine primäre Assembly, die sich im Ordner "/ bin" befinden. In Visual Studio 2005 wurde ein Ordner "App/_Code" hinzugefügt. Klassen und anderen nicht-UI-Code werden in den Ordner App/_Code hinzugefügt. Wenn Visual Studio das Projekt erstellt wurde, werden alle Dateien im Ordner "App/_Code" in eine einzelne App/_Code.dll-Datei kompiliert. Das Ergebnis dieser Änderung ist, dass nachfolgende Builds viel schneller als in früheren Versionen sind.

> [!NOTE]
> MSBuild-Befehlszeilen-Hilfsprogramm kann auch zum Erstellen von ASP.NET Web-Anwendungen verwendet werden. Dieses Tool wird im Modul 9 abgedeckt werden.


Eine andere Erweiterung der Kompilierung wird die neue Seite "Build"-Option im Menü. Dieses Feature ermöglicht Entwicklern das nur die aktuelle Seite (zusammen mit, Kurs und Abhängigkeiten) neu erstellen, damit Änderungen schneller kompiliert werden können. Da c# kein Hintergrund-Kompilierung zum Zweck der Aktualisierung der IntelliSense usw. bereitstellt, werden sie ungemein, Ihr dieses Feature profitieren, da es für IntelliSense schnell aktualisiert werden, indem Sie einfach neu erstellen einer einzelnen Seite zulässt.

Der Build-Eigenschaften für ein Projekt können Sie zum Konfigurieren des Typs des Builds, das auftritt, bevor die Startseite ausgeführt wird. Entwickler können wählen, um nur die aktuelle Seite zu erstellen, sodass Visual Studio Debuggen von Anwendungen schneller nach codeänderungen starten können.


![Der Buildvorgang für die Seite starten](improvements-in-visual-studio-2005/_static/image6.jpg)

**Abbildung 9**: der Buildvorgang für die Seite starten


Eine weitere hervorragende Verbesserung zu Visual Studio und die Architektur für ASP.NET befindet sich im Bereich von bearbeiten und fortfahren. In Visual Studio 2005 können Entwickler mit dem Debuggen eines Projekts beginnen und codeänderungen am Projekt vornehmen, ohne den Debugger zu trennen. In der Tat fügen Sie buchstäblich Debuggen können einem Projekt eine neue Klasse hinzu, fügen Sie Code für diese Klasse, fügen Sie Code hinzu, auf die Seite, die eine neue Instanz dieser Klasse erstellt und führen Sie eine Methode der Klasse, ohne dafür das Trennen des Debuggers aus. Den neuen Code ausführen wird als solcher genauso einfach wie das Aktualisieren des Browsers!

Klicken Sie hier, um ein video zur exemplarischen Vorgehensweise der Bearbeitung und weiterhin Feature in Visual Studio 2005.


![](improvements-in-visual-studio-2005/_static/image2.png)


[Open Vollbild-Video](improvements-in-visual-studio-2005/_static/editcontinue1.wmv)


Die stabile bearbeiten und Fortsetzen von Funktionen in ASP.NET 2.0 und Visual Studio 2005 ist aufgrund einer Änderung der Architektur für ASP.NET-Anwendungen. In ASP.NET 1.x, Anwendungen, die in Visual Studio 2002/2003 erstellten kompiliert wurden, in eine primäre Assembly, die in den Ordner "/ bin" gespeichert wurde. Alle Klassen, Seiten, usw. für die Anwendung in eine DLL kompiliert wurden. Klicken Sie dann zur Laufzeit, würde ASP.NET kompilieren alle Steuerelemente, Markup und ASP.NET-Code in Seiten, und kopieren Sie diese DLLs in den temporären ASP.NET-Ordner.

In Visual Studio 2005 mit ASP.NET 2.0, die zwei Kompilierung Modelle Gliedern oben (eine für Visual Studio) und eine für ASP.NET zur Laufzeit müssen mit einer allgemeinen Kompilierungsmodell zusammengeführt. Das bedeutet, dass alle Kompilierungsfehler nun während der Entwicklungsphase statt zur Laufzeit abgefangen werden. Darüber hinaus können für den Designer und IntelliSense-Unterstützung für Funktionen wie Benutzersteuerelemente und Masterseiten.

Klicken Sie hier, um designerunterstützung für Benutzersteuerelemente eine Videodemonstration anzuzeigen.


![](improvements-in-visual-studio-2005/_static/image3.png)


[Open Vollbild-Video](improvements-in-visual-studio-2005/_static/usercontrols1.wmv)


> [!NOTE]
> Wenn ein Benutzersteuerelement aus einer Seite entfernt wird die @Register Richtlinie bleibt im Markup und sollte manuell entfernt werden, um Parserfehler zu vermeiden, wenn das Benutzersteuerelement von der Website gelöscht wird.


Eine weitere Verbesserung in das Visual Studio-Kompilierungsmodell ist das Veröffentlichen einer Website-Feature. Da der Veröffentlichungsfunktion kompiliert, die Website vor, können Entwickler die hinzugefügte Leistung bei Bedarf kompilieren ohne nutzen. Auch kompiliert alle Quellcode im Ordner "App/_Code" in eine DLL, damit kein Quellcode bereitgestellt werden kann.


![Das Dialogfeld "Website veröffentlichen"](improvements-in-visual-studio-2005/_static/image7.jpg)

**Abbildung 10**: das Dialogfeld "Website veröffentlichen"


> [!NOTE]
> Das Hilfsprogramm aspnet/_compile.exe kann auch eine ASP.NET-Webanwendung Vorkompilieren verwendet werden. Dieses Tool wird im Modul 9 abgedeckt werden.


Wenn Sie die Veröffentlichen einer Website, die vorkompilierten Dateien im Ordner "Temporary ASP.NET Files" gespeichert werden wie unten dargestellt. Dateien mit einem *Compiled* Dateierweiterung sind XML-Dateien, die Abhängigkeiten für bestimmte DLLs zu definieren. Alle Steuerelemente Webform oder Benutzer in zufälligen DLLs, die mit beginnen kompiliert werden *App /_Web /_*.

Wenn Sie lassen die *dieser vorkompilierte Website aktualisierbar sein* Kontrollkästchen aktiviert, Markup innerhalb Ihrer Steuerelemente Webforms und Benutzer werden nicht vorkompiliert in eine DLL, sodass Sie nach der Bereitstellung Änderungen vornehmen. Wenn Sie das Markup Sperren lieber, damit Änderungen an der bereitgestellte Inhalt nicht zulässig sind, deaktivieren Sie dieses Kontrollkästchen.

Die *Verwendung fester benennen und eine einzelne Seitenassemblys* Kontrollkästchen ermöglicht das Batch-Kompilierung zu deaktivieren, sodass jede Seite in einer Assembly mit festen Namen kompiliert wird. Lassen Sie dieses Kontrollkästchen deaktiviert lassen Batch-Kompilierung nutzen.

Die *Assemblys starke Namen ermöglichen, auf vorkompilierte* Kontrollkästchen können Sie mit starken Namen der vorkompilierten Assemblys.

> [!NOTE]
> In ASP.NET 1.x, Assemblys mit starkem Namen in den globalen Assemblycache (GAC) installiert werden mussten. In ASP.NET 2.0 sind Sie nicht erforderlich, um Assemblys mit starkem Namen im globalen Assemblycache zu installieren.


![Eine vorkompilierte Applikationen ASP.NET-Dateien](improvements-in-visual-studio-2005/_static/image8.jpg)

**Abbildung 11**: eine vorkompilierte Applikationen ASP.NET-Dateien


> [!NOTE]
> Es wurde keine Datei "Web.config"-Datei, in der Anwendung. Es gab, es würde aufgerufen wurden, *"PrecompiledApp.config"* nach dem Veröffentlichen von Web site-Prozess.


## <a name="improvements-in-deployment"></a>Verbesserungen bei der Bereitstellung

Als Visual Studio 2002 und 2003, bietet Visual Studio 2005 eine Funktion Projekt kopieren. Allerdings wird die Funktion hat sich in Visual Studio 2005 beefed wurde und heißt jetzt Website kopieren.

Das Dialogfeld "Website kopieren" wird in einem linken Rahmen und ein rechtes Frame aufgeteilt. Linken Bereich ist der Quellwebsite Rechte Frame aufgerufen und der Remotewebsite. Aufpassen, die einige Entwickler verwechseln könnte ist, dass die Website im rechten Bereich angezeigt nicht unbedingt ein Remotestandort ist. Es kann eine Website im lokalen Dateisystem oder in der lokalen Instanz von IIS handeln. Darüber hinaus die Website im linken Bereich angezeigten ist nicht unbedingt der Quellwebsite weil im Dialogfeld Sie von der remote-Website veröffentlichen können *auf* der Quellwebsite.

Wenn Sie ein Projekt auf einer remoten Website kopieren, muss diesem Standort die FrontPage-Servererweiterungen installiert haben. Wenn dies nicht der Fall ist, müssen Sie eine Verbindung herstellen über FTP. Wenn Sie ein Projekt mit der lokalen IIS-Instanz kopieren, sind die FrontPage-Servererweiterungen andererseits, nicht erforderlich.

> [!NOTE]
> Wenn Sie versuchen, eine neue Website auf dem lokalen IIS-Instanz zu erstellen, und die FrontPage 2002 Servererweiterungen installiert sind, erhalten Sie eine Fehlermeldung, die besagt, dass das Erstellen von Websites auf einem SharePoint-Server nicht unterstützt wird. In diesem Fall müssen Sie die Möglichkeit, die FrontPage-Servererweiterungen 2000 installieren oder entfernen die FrontPage-Servererweiterungen.


Klicken Sie hier, um ein video zur exemplarischen Vorgehensweise der Funktion "Website kopieren".


![](improvements-in-visual-studio-2005/_static/image4.png)


[Open Vollbild-Video](improvements-in-visual-studio-2005/_static/copysite1.wmv)


## <a name="improvements-in-debugging"></a>Verbesserungen beim Debuggen

Es gibt vier wichtige Verbesserungen beim Debuggen in Visual Studio 2005.

- Lokal als ohne Administratorrechte Debuggen ist möglich, ausgegeben.
- Das Debug-Attribut für das Compilation-Element ist jetzt standardmäßig "false".
- Remotedebuggen-Setup und Konfiguration ist einfacher als zuvor.
- Sie können jetzt eine Website, die über eine FTP-Speicherort geöffnet Debuggen.

## <a name="debugging-as-a-non-administrator"></a>Als ohne Administratorrechte Debuggen

Das Hinzufügen von ASP.NET Development Server ermöglicht Nichtadministratoren ASP.NET-Anwendungen direkt einsatzbereiten einfacher zu debuggen. Beim Debuggen einer ASP.NET-Anwendung auf dem lokalen Dateisystem ausgeführt wird, startet Visual Studio den ASP.NET Development Server im Kontext des angemeldeten Benutzers. Dieser Benutzer kann dann die Anwendung ohne zusätzliche Konfiguration Debuggen.

## <a name="debug-is-false-by-default"></a>Debuggen ist "false" in der Standardeinstellung

In ASP.NET 1.x, die *Debuggen* Attribut in der *Kompilierung* -Element der Datei "Web.config" wurde festgelegt, um *"true"* standardmäßig. Es immer wurde empfohlen, dass Entwickler dieses Attribut, um festgelegt *"false"* vor der Bereitstellung einer Anwendung bis hin zur Produktion, aber da die meisten Entwickler die Konsequenzen, lassen Sie das Debug-Attributsatz nicht vollständig verstehen "true", sie einfach von links als-ist.

Die schwerwiegendsten Problem mit der Debug-Attribut auf True festgelegt ist, dass es ASP.NETs Batch Kompilierungsmodell deaktiviert werden. Daher wird jede Seite in einer separaten DLL kompiliert. Wenn eine Anwendung besteht aus Tausenden von Seiten (nicht vor der irgendeine Weise), Web bedeutet werden von der Anwendung mehrere Tausend kleine DLLs erstellt. Während diese DLLs nur eine geringe Größe sind, sind sie nicht in eine bestimmte Position im Arbeitsspeicher geladen. Daher dazu führen, dass Fragmentierung im Arbeitsspeicher und auf OutOfMemoryException Vorkommen beitragen können.

In ASP.NET 2.0 ist das Debug-Attribut standardmäßig auf "false" festgelegt. Wie Sie sich bereits gesehen haben, wenn ein Entwickler eine ASP.NET-Anwendung in Visual Studio 2005 debuggt, wird er aufgefordert, eine Datei "Web.config" mit aktiviertem Debuggen hinzufügen. Auf diese Weise verursacht die gleichen Nachteile, die in ASP.NET vorhanden waren 1.x, aber jetzt der Entwickler wird deutlich, dass das Attribut auf "false" zurückgesetzt werden soll, bevor Sie die Anwendung bis hin zur Produktion zu fortfahren gewarnt.

## <a name="remote-debugging-setup-and-configuration"></a>Remote Debugging – Setup und Konfiguration

In Visual Studio 2002/2003 datenaktualisierungsarchitektur Machine Debug-Manager (mdm.exe) und der Prozess vs7jit.exe Remotedebuggen. Aus diesem Grund wurde die remote debugging Problembehandlung häufig ein schwarzes Kästchen für Kunden und war häufig nicht wesentlich besser für PSS.

Visual Studio 2005 entfernt die Abhängigkeit von den Prozessen mdm.exe und vs7jit.exe. Stattdessen wird jetzt den Remote Debug Monitor-Dienst (msvsmon.exe).

Die Anforderung für das Debuggen in Visual Studio 2005 Remote ist ganz einfach. Sie müssen msvsmon.exe auf dem Remoteserver vor dem Debuggen ausführen. Können Sie Remote Debug Monitor aus der Visual Studio-CD installieren oder Sie können einfach msvsmon.exe aus einer Freigabe ausführen, ohne dass etwas überhaupt auf dem Webserver installiert.

Wenn Sie msvsmon.exe ausführen, ist es wahrscheinlich, dass es zu Ports, die für das Remotedebuggen blockiert darüber beschweren wird. Glücklicherweise können Sie einfach die Ports direkt aus dem Dialogfeld mit Warnung aufheben, wie unten dargestellt.


![Benachrichtigung, dass die Windows-Firewall für das Remotedebugging blockiert ist](improvements-in-visual-studio-2005/_static/image9.jpg)

**Abbildung 12**: Benachrichtigung, dass Windows-Firewall ist Remotedebuggen blockieren


Nachdem Sie die Ports, die für das Debuggen erforderlich "Blockierung aufgehoben" haben, sehen Sie den Remotedebugmonitor, wie unten dargestellt. Von dieser Schnittstelle können Verbindungen zu überwachen und Debuggen von Berechtigungen leicht ändern.


![Remote Debug Monitor](improvements-in-visual-studio-2005/_static/image10.jpg)

**Abbildung 13**: Remote Debug Monitor


Es ist auch möglich, Remotedebuggen eine Webanwendung über FTP geöffnet. Die Schritte sind identisch mit denen zuvor abgedeckt. Allerdings müssen Sie eine base-URL für das Durchsuchen der FTP-Projekt wie weiter oben in diesem Modul angeben.

## <a name="lab-2"></a>Lab 2

## <a name="remote-debugging-with-visual-studio-2005"></a>Remotedebuggen mit Visual Studio 2005

Diese Übung wird exemplarisch Remotedebuggen mit Visual Studio 2005.

Klicken Sie hier, um dieser Anleitung ein video zur exemplarischen Vorgehensweise.


![](improvements-in-visual-studio-2005/_static/image5.png)


[Open Vollbild-Video](improvements-in-visual-studio-2005/_static/remdebug1.wmv)


Diese Übungseinheit benötigen Sie zwei Computer, einem ausgeführten Visual Studio 2005 und die anderen ausgeführten IIS 5 oder höher.

1. Öffnen Sie Visual Studio 2005, und erstellen Sie eine neue Website auf dem Remoteserver.

> [!NOTE]
> Sie können die Website auf einer Remoteinstanz von IIS oder über FTP erstellen.


1. Suchen Sie aus den Remotewebserver msvsmon.exe, auf dem Entwicklungscomputer über einen UNC-Pfad, und führen Sie es aus.  
 Der Standardspeicherort für msvsmon.exe ist //server/c$/Program Programme/Microsoft Visual Studio 8/Common7/IDE/Remote Debugger/X86.
2. Wenn Sie dazu aufgefordert werden, um die Blockierung von Ports für das Remotedebuggen, dazu verwenden.
3. Öffnen Sie aus dem Entwicklungscomputer an den Code-Behind für "default.aspx", und legen Sie einen Haltepunkt in der Seite "/ _Load-Methode.
4. Starten Sie das Debuggen vom Entwicklungscomputer.

Sie sollten den Haltepunkt wie erwartet.

## <a name="aspnet-development-server"></a>ASP.NET Development Server

Visual Studio 2005 enthaltenen Weve bereits erläutert mit einem Webserver, der den ASP.NET Development Server aufgerufen. (ASP.NET Development Server wird manchmal als Cassini bezeichnet.) Diese Web-Server ist eine praktische Möglichkeit zum Durchsuchen und Debuggen von Webanwendungen, die im Dateisystem ausgeführt.

Der ASP.NET Development Server ist eine eingeschränkte Webserver. Lässt keine Remoteverbindungen, ermöglicht es keine Anforderungen von anderer Benutzer als der Benutzer, die der Web-Server gestartet wurde. Es verfügt auch nicht über die Fähigkeit ASP-Seiten bedient. Nur die Ressourcen für ASP.NET und HTML-Ressourcen (z. B. Bilder, CSS-Dateien usw.) verarbeitet werden.

Der ASP.NET Development Server über die Befehlszeile gestartet werden kann, durch Ausführen der Datei WebDev.WebServer.exe c:/Windows/Microsoft.NET/Framework/v2.0./ */*  /  */*/*. Das folgende Dialogfeld zeigt die Parameter, die verfügbar sind.


![](improvements-in-visual-studio-2005/_static/image11.jpg)

**Abbildung 14**


> [!NOTE]
> Der ASP.NET Development Server wird nicht unterstützt, wenn explizit über die Befehlszeile gestartet wird.
