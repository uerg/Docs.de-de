---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: Bereitstellen einer Datenbank (c#) | Microsoft-Dokumentation
author: rick-anderson
description: Bereitstellen einer ASP.NET-Webanwendung umfasst das Abrufen der erforderlichen Dateien und Ressourcen aus der Entwicklungsumgebung in der produktionsumgebung bereit. Für Daten...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: ca9ce2b41cfd10504304c30bc965e446a7188120
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826952"
---
<a name="deploying-a-database-c"></a>Bereitstellen einer Datenbank (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Code herunterladen](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> Bereitstellen einer ASP.NET-Webanwendung umfasst das Abrufen der erforderlichen Dateien und Ressourcen aus der Entwicklungsumgebung in der produktionsumgebung bereit. Für datengesteuerte Webanwendungen umfasst dies das Datenbankschema und Daten. Dieses Tutorial ist der erste in einer Reihe, in die erforderlichen Schritte zum erfolgreich auf die Datenbank aus der Entwicklungsumgebung für die Produktion bereitstellen.


## <a name="introduction"></a>Einführung

Bereitstellen einer ASP.NET-Webanwendung umfasst das Abrufen der erforderlichen Dateien und Ressourcen aus der Entwicklungsumgebung in der produktionsumgebung bereit. Im Laufe der letzten sechs Tutorials haben Sie eine einfache Book Reviews-Webanwendung bereitstellen. Diese demowebsite wurde eine Reihe von Ressourcen mit serverseitigen - ASP.NET-Seiten, Konfigurationsdateien, umfasst eine `Web.sitemap` -Datei und So weiter – zusammen mit der clientseitigen-Ressourcen wie Bilder und CSS-Dateien. Aber wie sieht es mit datengesteuerten Webanwendungen? Ermittelt, welche zusätzlichen Schritte müssen ausgeführt werden, um eine Webanwendung bereitstellen, die eine Datenbank verwendet werden?

Über die nächsten mehrere Tutorials werden wir die erforderlichen Schritte zum Bereitstellen einer datengesteuerten Webanwendung behandelt. In diesem Tutorial wird anhand der zum Abrufen eines s Datenbankschemas und der Inhalt aus der Entwicklungsumgebung in der produktionsumgebung während der nachfolgende Tutorial, die erforderlichen konfigurationsänderungen untersucht gestartet. Befolgen, dass wir erläutern werde Herausforderungen bei der Bereitstellung einer Datenbank, die der die Anwendungsdienste (Mitgliedschaft, Rollen, Profil und So weiter) verwendet.

## <a name="examining-the-updated-book-reviews-web-application"></a>Untersuchen die aktualisierte Book Reviews-Webanwendung

Um zu zeigen, Bereitstellung einer datengesteuerten Webanwendung, ich Ve der Book Reviews-Webanwendung auf einer einfachen, statische Website zu einer datengesteuerten aktualisiert. Wie vorher, es zwei Versionen der Anwendung in diesem Tutorial s-Download gibt: verwendet das Webanwendungsprojekt-Modell und eine, die das Websiteprojekt-Modell verwendet.

Der aktualisierte Book Reviews-web-Anwendung verwendet eine [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) -Datenbank, die in der Website s gespeichert sind `App_Data` Ordner (`~/App_Data/Reviews.mdf`). Wenn Sie SQL Server 2008 auf Ihrem Computer installiert haben, sollten die Demo ohne Fehler ausführen. Wenn man von einer älteren Version von SQL Server können Sie entweder die kostenlose SQL Server 2008 Express Edition zu installieren, oder können Sie die Datenbank mit dem Herunterladen des Skripts, die in diesem Tutorial s zur Verfügung, um die Datenbank selbst zu erstellen.

Die `Reviews.mdf` Datenbank enthält vier Tabellen:

- `Genres` – enthält einen Datensatz für jedes Genre, z. B. Technologie, Fiction und Business.
- `Books` – enthält einen Datensatz für jede Kritik, mit Spalten, z. B. `Title`, `GenreId`, `ReviewDate`, und `Review`, u. a.
- `Authors` – enthält Informationen zu jeder Autor, die eine Book Reviews beigetragen hat.
- `BooksAuthors` -eine m: n Jointabelle, der angibt, welche Autoren, welche Bücher geschrieben haben.
  

Abbildung 1 zeigt ein ER-Diagramm dieser vier Tabellen.


[![Die Book Reviews-Webanwendung-s-Datenbank ist, besteht aus den vier Tabellen](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**Abbildung 1**: The Book Reviews-Webanwendung-s-Datenbank ist, besteht aus den vier Tabellen ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-a-database-cs/_static/image3.jpg))


Die vorherige Version der Website Book Reviews hatte eine separate ASP.NET-Seite für jedes Buch. Beispielsweise gab es eine Seite namens `~/Tech/TYASP35.aspx` , enthalten die Überprüfung für *bringen Sie sich ASP.NET 3.5 in 24 Stunden*. Neue datengesteuerte Version der Website bietet die Überprüfungen gespeichert, in der Datenbank und eine einzelne ASP.NET-Seite Review.aspx?ID=*BookId*, wird angezeigt, die die Überprüfung für das angegebene Buch. Es ist ebenfalls ein Genre.aspx?ID=*GenreId* Seite, geprüfte Bücher in der angegebenen "Genre" aufgeführt.

Abbildung 2 und 3 zeigt die `Genre.aspx` und `Review.aspx` Seiten in Aktion. Beachten Sie die URL in der Adressleiste für jede Seite. In Abbildung 2 It s Genre.aspx? ID = C 47-82a0-c8ec75de7e0e der 85d164ba-1123-4. Da 85d164ba-1123-4c47-82a0-c8ec75de7e0e ist die `GenreId` Wert für "Genre" der Technologie, die Seite "s" Überschrift-Lesevorgänge "Überprüft die Technologie" und der Aufzählung listet diese Bewertungen auf der Website, die unter diesem "Genre" fallen.


[![Das Genre-Technologieseite](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**Abbildung 2**: die Technologieseite "Genre" ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-a-database-cs/_static/image6.jpg))


[![Die Überprüfung für bringen Sie sich selbst ASP.NET 3.5 in 24 Stunden](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**Abbildung 3**: die Überprüfung für *bringen Sie sich ASP.NET 3.5 in 24 Stunden* ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-a-database-cs/_static/image9.jpg))


Die Webanwendung Book Reviews enthält auch eine Verwaltungsabschnitt, in dem Administratoren können hinzufügen, bearbeiten und löschen Genres, Reviews, und Informationen zu erstellen. Derzeit kann jedem Besucher im Bereich Verwaltung zugreifen. In einem späteren Tutorial wir fügen Unterstützung für Benutzerkonten und lassen nur autorisierte Benutzer in der Verwaltungsseiten.

Wenn Sie die Anwendung Book Reviews Bitte denken Sie daran, die dient zur Veranschaulichung eine datengesteuerte Anwendung bereitstellen herunterladen. Es treten keine bewährte Methoden so weit wie Anwendungsentwurf. Es ist beispielsweise keine separate Data Access Layer (DAL); die ASP.NET-Seiten kommunizieren direkt mit der Datenbank über die SqlDataSource-Steuerelement oder ADO.NET-Code in der CodeBehind-Klassen. Ausführlichere Informationen über das Erstellen von datengesteuerten Anwendungen, die über eine geschichtete Architektur, finden Sie in meinem [ *arbeiten mit Daten* Tutorials](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Datenbanken auf die Entwicklung im Vergleich zur Produktion

Wenn Sie die Entwicklung einer datengesteuerten Webanwendung beginnen müssen Sie eine Datenbankverbindungs-Zeichenfolge, angeben, die der App-Details zum Herstellen einer Verbindung mit der Datenbank bereitstellt. Diese Verbindungszeichenfolge gibt an, unter anderem, den Datenbankserver, den Datenbanknamen und Sicherheitsinformationen. In den meisten Fällen die von der Anwendung während der Entwicklung verwendete Datenbank unterscheidet sich von die Datenbank verwendet wird, wenn es s in der Produktion. Es gibt viele Vorteile des Verwendens von anderen Datenbanken für die Entwicklung und Produktion. Mit einer anderen Datenbank in die Entwicklung bedeutet, dass Don ' t über versehentlich ändern und Löschen von live-Daten Gedanken machen. Außerdem können Sie die fügen dummy-Testdaten oder Änderungen an das Datenmodell vornehmen, ohne sich Gedanken über die Auswirkungen auf die Anwendung in der Produktion. Der Nachteil bei der Verwendung einer anderen Datenbank in der Entwicklungs- und produktionsumgebungen ist, wenn die Anwendung ist die Datenbank und relevanten Änderungen am Datenbankschema s bereitgestellt, oder Daten müssen ebenfalls bereitgestellt werden.

Vor der ersten Bereitstellung hat nur eine Instanz der Datenbank vorhanden ist und diese Instanz befindet sich in der Entwicklungsumgebung. Beim Bereitstellen der Anwendung in der Produktion zum ersten Mal müssen wir nicht nur kopieren die benötigten Dateien für serverseitige und clientseitige, sondern auch die Datenbank aus der Entwicklungsumgebung kopieren, in der produktionsumgebung bereit. Dies ist, wo wir rechts stehen nun mit der Webanwendung von Buchbesprechungen – in die Datenbank befindet sich die `App_Data` Ordner in der Entwicklungsumgebung verfügt jedoch nicht noch in der produktionsumgebung veröffentlicht wurde, um.

Nach der Bereitstellung der Anwendungs sollten Sie die zwei Kopien der Datenbank vorhanden sind. Die Anwendung Laufe der Zeit werden möglicherweise neue Features hinzugefügt, eine Änderung des Datenmodells erforderlich (z. B. das Hinzufügen neuer Spalten zu vorhandenen Tabellen, Änderungen an vorhandenen Spalten neue Tabellen hinzufügen und so weiter). Wenn die Webanwendung als Nächstes bereitgestellt wird, werden die Änderungen auf die Datenbank in der Entwicklungsumgebung angewendet, seit die letzte Bereitstellung auf die Produktionsdatenbank angewendet werden muss. Einige Strategien für die Verwaltung von diesem Prozess werden in einem späteren Tutorial erläutert. Dieses Tutorial konzentriert sich auf die gesamte Datenbank aus der Entwicklungsumgebung für die Produktion bereitstellen.

## <a name="deploying-the-database-to-the-production-environment"></a>Bereitstellen der Datenbank in der Produktionsumgebung

Die restlichen Verlauf dieses Lernprogramms untersucht, wie die Datenbank aus der Entwicklungsumgebung in der produktionsumgebung bereitstellen. Wenn Sie auf die Sie befolgen muss sicherstellen, dass Ihr Konto mit Ihrem Webhostinganbieter Microsoft SQL Server enthält Datenbanken unterstützen. Sie müssen auch einige Informationen, nämlich den Namen des Datenbankservers, der Name der Datenbank und den Benutzernamen und das Kennwort für die Verbindung mit der Datenbank verwendet haben.

Wie weiter oben in diesem Tutorial erwähnt, ist die Book Reviews-Website-s-Datenbank einer SQL Server 2008 Express Edition-Datenbank gespeichert, der `App_Data` Ordner. Würde eignet, Grund, die Bereitstellung einer Datenbank so einfach wie das Kopieren von wäre die `App_Data` Ordner aus der Entwicklungsumgebung in der produktionsumgebung bereit. Die meisten Web-Host-Anbieter unterstützen jedoch keine hosting-Datenbanken in der `App_Data` Ordner aus Gründen der Sicherheit. Webhosts wird stattdessen ein Konto auf einem SQL Server-Datenbank-Server in ihrer Umgebung bereitstellen. Bereitstellen der Datenbank in Ihrer Entwicklungsumgebung, in der produktionsumgebung ist erforderlich, die Datenbanken auf dem Webserver Host s-Datenbank registriert.

Wie erhalten Sie Ihre Datenbank aus der Entwicklungsumgebung in der produktionsumgebung? Es gibt mehrere Möglichkeiten, je nachdem, welche Ihrer Web-Host-Angebote Dienste dazu. Mit einigen Hosts, z. B. DiscountASP.NET, können Sie eine Sicherung der Datenbank oder den tatsächlichen FTP `.mdf` zu Ihrer Website, und klicken Sie dann in der Systemsteuerung, stellt die Sicherungsdatei oder das `.mdf` Datei mit dem SQL Server-Datenbankserver. Solche Tools Bereitstellen der Datenbank ist so einfach wie das Kopieren der `App_Data` Ordner für die produktionsumgebung und dann über die Systemsteuerung anfügen. Dies ist vielleicht die schnellste und einfachste Möglichkeit, Ihre Datenbank zum ersten Mal veröffentlichen.

Ein anderer Ansatz ist die Verwendung der Datenbankveröffentlichungs-Assistenten. Der Datenbankveröffentlichungs-Assistent ist eine Windows-desktop-Anwendung, die die SQL-Befehle zum Erstellen von den Schemas der Datenbank-s - Tabellen, gespeicherte Prozeduren, Sichten, benutzerdefinierte Funktionen usw. – und optional die Daten in den Tabellen generiert wird. Sie können dann eine Verbindung herstellen, mit dem Web-Host-Anbieter-s-Datenbankserver aus, über SQL Server Management Studio und führen Sie dann dieses Skript aus, um die Datenbank auf die Produktion zu duplizieren. Noch besser, wenn Ihre Webhostinganbieter Microsoft s unterstützt [Datenbank-Veröffentlichungsdienste](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) können Sie das Skript, von dem Datenbankveröffentlichungs-Assistenten automatisch ausgeführt wird, auf dem Datenbankserver in Ihrem Namen generiert haben. Da der Datenbankveröffentlichungs-Assistenten ein Skript, die das s-Datenbank-Schema und Daten erstellt generiert, müssen sie funktioniert unabhängig davon, ob Ihre Webhostinganbieter Features wie das Anfügen einer hochgeladenen bietet `.mdf` Datei.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>Generieren die SQL-Befehle, um das Datenbankschema und Daten mit dem Datenbankveröffentlichungs-Assistent zu erstellen.

Lassen Sie s, die durch die Verwendung der Datenbankveröffentlichungs-Assistent zum Bereitstellen der Book Reviews-Datenbank in der produktionsumgebung geführt. Bei Verwendung von Visual Studio 2008 oder höher ist, wird der Datenbankveröffentlichungs-Assistent ist bereits installiert. Wenn Sie Visual Studio 2005 verwenden, dann Sie zuerst müssen [herunterladen und installieren](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) des Assistenten.

Öffnen Sie Visual Studio, und navigieren Sie zu der `Reviews.mdf` Datenbank. Wenn Sie Visual Web Developer verwenden, wechseln Sie zum Datenbank-Explorer; Wenn Sie Visual Studio verwenden, verwenden Sie den Server-Explorer. Abbildung 4 zeigt die `Reviews.mdf` Datenbank in der Datenbank-Explorer in Visual Web Developer. Wie in Abbildung 4 gezeigt, die `Reviews.mdf` Datenbank besteht aus vier Tabellen, drei gespeicherten Prozeduren und eine benutzerdefinierte Funktion.


[![Suchen Sie die Datenbank in der Datenbank-Explorer oder Server-Explorer](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**Abbildung 4**: Suchen Sie die Datenbank in der Datenbank-Explorer oder Server-Explorer ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-a-database-cs/_static/image12.jpg))


Mit der rechten Maustaste auf den Datenbanknamen aus, und wählen Sie im Kontextmenü die Option "Für Anbieter veröffentlichen". Dadurch wird der Datenbankveröffentlichungs-Assistent gestartet (siehe Abbildung 5). Klicken Sie auf Weiter, um nach dem Begrüßungsbildschirm angezeigt.


[![Der Datenbankveröffentlichungs-Assistenten-Begrüßungsbildschirm](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**Abbildung 5**: die Datenbank veröffentlichen-Assistent Begrüßungsbildschirm ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-a-database-cs/_static/image15.jpg))


Der zweite Bildschirm des Assistenten führt die Datenbanken, die für den Datenbankveröffentlichungs-Assistenten zugänglich und können Sie auswählen, ob Sie das Skript für alle Objekte in der ausgewählten Datenbank oder der Objekte, die Skripts auswählen. Wählen Sie die entsprechende Datenbank, und lassen Sie die "Skript alle Objekte in der ausgewählten Datenbank auf"-Option aktiviert.

> [!NOTE]
> Wenn Sie die Fehlermeldung "in der Datenbank keine Objekte vorhanden sind *DatabaseName* der Typen, die von diesem Assistenten skriptfähige" Wenn weiter auf dem Bildschirm in Abbildung 6 dargestellten klicken, stellen Sie sicher, dass der Pfad zu Ihrer Datenbankdatei nicht übermäßig lang ist. Es wurde festgestellt, dass dieser Fehler auftreten kann, wenn der Pfad zur Datenbankdatei zu lang ist.


[![Der Datenbankveröffentlichungs-Assistenten-Begrüßungsbildschirm](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**Abbildung 6**: die Datenbank veröffentlichen-Assistent Begrüßungsbildschirm ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-a-database-cs/_static/image18.jpg))


Auf dem nächsten Bildschirm können Skriptdatei generieren oder, wenn Ihre Web-Host dies unterstützt, veröffentlichen die Datenbank direkt in Ihrem Web-Host-Anbieter-s-Datenbank-Server. Wie in Abbildung 7 dargestellt, habe ich das Skript in die Datei geschrieben `C:\REVIEWS.MDF.sql`.


[![Skripterstellung für die Datenbank in eine Datei oder direkt in Ihre Webhostinganbieter veröffentlichen](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**Abbildung 7**: Skripterstellung für die Datenbank in eine Datei oder direkt in Ihre Webhostinganbieter veröffentlichen ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-a-database-cs/_static/image21.jpg))


Im folgenden Bildschirm aufgefordert, für eine Vielzahl von Skriptoptionen. Sie können angeben, ob das Skript die Drop-Anweisungen, um diese vorhandene Objekte entfernen enthalten soll. Dies ist standardmäßig auf "true", dies ist in Ordnung, wenn eine Datenbank zum ersten Mal bereitstellen. Sie können auch angeben, ob die Zieldatenbank, SQL Server 2000, SQL Server 2005 oder SQL Server 2008 ist. Schließlich können Sie angeben, ob das Schema und Daten, das Skript nur die Daten oder nur das Schema. Das Schema ist die Auflistung der Datenbankobjekte, die Tabellen, gespeicherte Prozeduren, Ansichten und So weiter. Die Daten sind die Informationen in den Tabellen befinden.

Wie in Abbildung 8 dargestellt, ich gibt es den Assistenten so konfiguriert, dass das Löschen vorhandener Datenbankobjekte, die zum Skript für eine SQL Server 2008-Datenbank zu generieren und veröffentlichen sowohl das Schema und Daten.


[![Geben Sie die Veröffentlichung Optionen](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**Abbildung 8**: Geben Sie Optionen für die Veröffentlichung ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-a-database-cs/_static/image24.jpg))


Die letzten beiden Bildschirme fassen zusammen, die Aktionen, die ausgeführt werden, und klicken Sie dann zeigt den Status der Erstellung von Skripts. Das Ergebnis der Ausführung des Assistenten ist, dass wir eine Skriptdatei, die die zum Erstellen der Datenbank in der Produktion, und füllen diese mit den gleichen Daten wie zur Entwicklung von SQL-Befehle enthält.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Die SQL‑Befehle ausführen, auf die Datenbank der Produktion-Umgebung

Das Skript, das die SQL-Befehle zum Erstellen der Datenbank und die Daten enthält, die verbleibt, werden nun Ausführen des Skripts auf die Produktionsdatenbank. Einige Anbieter von Host bieten ein Textfeld in der Systemsteuerung, in dem Sie SQL-Befehlen auszuführenden für Ihre Datenbank eingeben können. Wenn Sie über eine sehr große Skriptdatei verfügen. diese Option funktioniert möglicherweise nicht (die `REVIEWS.MDF.sql` Skriptdatei ist mehr als 425 KB groß, z. B.).

Ein besserer Ansatz ist zur direkten Verbindung mit der Produktions-Datenbankserver mithilfe von SQL Server Management Studio (SSMS). Wenn Sie eine nicht - Express-Edition von SQL Server auf Ihrem Computer installiert haben müssen Sie wahrscheinlich bereits SSMS installiert. Andernfalls können Sie [herunterladen und installieren](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) eine kostenlose Version von SQL Server Management Studio Express Edition.

Starten Sie SSMS, und Verbinden mit Ihrer Web-Host s-Datenbankserver mithilfe der Informationen von Ihrem Web-Host-Anbieter.


[![Verbinden Sie mit Ihrem Web-Host-Anbieter-s-Datenbank-Server](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**Abbildung 9**: Verbinden mit Ihrer Webhostinganbieter-s-Datenbank-Server ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-a-database-cs/_static/image27.jpg))


Erweitern Sie die Registerkarte "Datenbanken" aus, und suchen Sie Ihre Datenbank. Klicken Sie auf die Schaltfläche "neue Abfrage" in der oberen linken Ecke der Symbolleiste, fügen Sie in der SQL-Befehle aus der Skriptdatei, die von der Datenbankveröffentlichungs-Assistent erstellt, und klicken Sie auf die Schaltfläche "ausführen", um diese Befehle auf dem Produktionsserver für die Datenbank auszuführen. Wenn Ihre Skriptdatei besonders groß ist, dauert es einige Minuten, um die Befehle auszuführen.


[![Verbinden Sie mit Ihrem Web-Host-Anbieter-s-Datenbank-Server](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**Abbildung 10**: Verbinden mit Ihrer Webhostinganbieter-s-Datenbank-Server ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-a-database-cs/_static/image30.jpg))


Alles, was, s wird es! An diesem Punkt wurde in der Produktion die Entwicklungsdatenbank dupliziert. Wenn Sie die Datenbank in SSMS aktualisieren sollte die neue Datenbankobjekte angezeigt werden. Abbildung 11 zeigt die Produktionstabellen-s-Datenbank gespeicherten Prozeduren und benutzerdefinierte Funktionen, die auf der Entwicklungsdatenbank spiegeln. Und da wir den Datenbankveröffentlichungs-Assistent zum Veröffentlichen der das angewiesen, s die Produktionstabellen-Datenbank die gleichen Daten wie die Entwicklung-s-Datenbanktabellen zu dem Zeitpunkt, die der Assistent ausgeführt wurde. Abbildung 12 zeigt die Daten in die `Books` Tabelle auf die Produktionsdatenbank.


[![Die Datenbankobjekte haben in der Produktionsdatenbank dupliziert](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**Abbildung 11**: die Datenbank Objekte haben dupliziert wurde in der Produktionsdatenbank ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-a-database-cs/_static/image33.jpg))


[![Die Produktionsdatenbank enthält die gleichen Daten wie in der Entwicklungsdatenbank](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**Abbildung 12**: die Produktionsdatenbank enthält die gleichen Daten wie in der Entwicklungsdatenbank ([klicken Sie, um das Bild in voller Größe anzeigen](deploying-a-database-cs/_static/image36.jpg))


Wir haben die Entwicklungsdatenbank an dieser Stelle nur in der produktionsumgebung bereitgestellt. Wir haben noch nicht untersucht, Bereitstellen der Webanwendung selbst oder untersucht, welche Änderungen an der Konfiguration erforderlich sind, um die Anwendung auf Produktions-mithilfe der Produktionsdatenbank. Diese Probleme werden im nächsten Tutorial behandelt.

## <a name="summary"></a>Zusammenfassung

Bereitstellung einer datengesteuerten Webanwendung erfordert das Kopieren der Datenbank, die bei der Entwicklung in der produktionsumgebung verwendet. Viele Web-Host-Anbieter bieten Tools zum Vereinfachen der Bereitstellung einer Datenbank. FTP Sie z. B. mit DiscountASP.NET Ihrer Datenbank `.mdf` Datei (oder eine Sicherung), und fügen Sie die Datenbank mit dem Datenbankserver, in der Systemsteuerung. Eine weitere Möglichkeit, die funktioniert unabhängig davon, welche Funktionen Ihrer Web-Host-Anbieter-Angebote ist Microsoft s Datenbankveröffentlichungs-Assistent-Tool, das ein Skript für die SQL-Befehle zum Erstellen der Development-s-Datenbankschema und die Daten generiert. Nachdem das Skript generiert wurde, können Sie sie auf die Produktionsdatenbank ausführen.

Nun, da die Book Reviews Web Application-s-Datenbank für die Produktion ist, können wir die Anwendung bereitstellen. Allerdings s-Konfigurationsinformationen der Web-Anwendung gibt die Verbindungszeichenfolge in der Datenbank, und dieser Verbindungszeichenfolge verweist auf die Entwicklungsdatenbank. Wir müssen diese Verbindungszeichenfolgendaten zu aktualisieren, wenn den Standort für die Produktion bereitstellen. Im nächste Tutorial betrachtet diese Konfigurationsunterschiede und führt durch die Schritte, die die datengesteuerte Book Reviews-Website für die Produktion veröffentlichen.

Viel Spaß beim Programmieren!

#### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Tutorial erläutert finden Sie in den folgenden Ressourcen:

- [Herunterladen der Microsoft SQL Server Datenbankveröffentlichungs-Assistent 1.1](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Herunterladen der Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

> [!div class="step-by-step"]
> [Zurück](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
> [Weiter](configuring-the-production-web-application-to-use-the-production-database-cs.md)
