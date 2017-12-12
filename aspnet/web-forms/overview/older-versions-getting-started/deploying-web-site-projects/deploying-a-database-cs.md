---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
title: Bereitstellen einer Datenbank (c#) | Microsoft Docs
author: rick-anderson
description: "Bereitstellen einer ASP.NET-Webanwendung umfasst das Abrufen von erforderlichen Dateien und Ressourcen aus der Entwicklungsumgebung in der produktionsumgebung. Für da..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: ff537a10-9f1f-43fe-9bcb-3dda161ba8f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: f71e3cd1e81644df7b3dfed363b6f2ca826e610d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="deploying-a-database-c"></a>Bereitstellen einer Datenbank (c#)
====================
durch [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Herunterladen von Code](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_07_CS.zip) oder [PDF herunterladen](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial07_DeployDB_cs.pdf)

> Bereitstellen einer ASP.NET-Webanwendung umfasst das Abrufen von erforderlichen Dateien und Ressourcen aus der Entwicklungsumgebung in der produktionsumgebung. Für datengesteuerte Webanwendungen umfasst dies das Datenbankschema und die Daten an. Dieses Lernprogramm ist die erste Aufgabe in einer Reihe, die die Schritte erforderlich, um die Datenbank aus der Entwicklungsumgebung bis hin zur Produktion erfolgreich bereitzustellen, untersucht.


## <a name="introduction"></a>Einführung

Bereitstellen einer ASP.NET-Webanwendung umfasst das Abrufen von erforderlichen Dateien und Ressourcen aus der Entwicklungsumgebung in der produktionsumgebung. Im Verlauf der letzten sechs Lernprogramme betrachtet Bereitstellen einer einfachen Buch Reviews Web-Anwendung. Diese Demo-Website wurde eine Reihe von serverseitigen Ressourcen - ASP.NET-Seiten, Konfigurationsdateien, besteht eine `Web.sitemap` Datei usw. -zusammen mit einer clientseitigen-Ressourcen wie Bilder und CSS-Dateien. Aber was datengesteuerte Webanwendungen? Ermittelt, welche zusätzlichen Schritte müssen ausgeführt werden, eine Webanwendung bereitstellen, die eine Datenbank verwendet werden?

Über die nächsten mehrere Lernprogramme geht es die erforderlichen Schritte zum Bereitstellen einer Webanwendung eines datengesteuerten. In diesem Lernprogramm beginnt mit dem untersuchen zum Abrufen eines s Datenbankschemas und der Inhalt aus der Entwicklungsumgebung in der produktionsumgebung, während das nachfolgende Lernprogramm die erforderlichen konfigurationsänderungen überprüft. Im Anschluss an, dass wir untersuchen müssen Herausforderungen bei der Bereitstellung einer Datenbank, die die Anwendungsdienste (Mitgliedschaft, Rollen, Profil usw.) verwendet.

## <a name="examining-the-updated-book-reviews-web-application"></a>Untersuchen die Webanwendung der aktualisierten Buch Reviews

Zur Veranschaulichung eine datengesteuerte Webanwendung bereitstellen ich Ve aktualisiert das Buch Reviews-Webanwendung aus einer einfachen, statische Website in einen datengesteuerten. Wie vorher, es zwei Versionen der Anwendung in diesem Lernprogramm s-Download gibt: eines, das verwendet wird, das Webanwendungsprojekt-Modell und die andere das Websiteprojekt-Modell verwendet.

Der aktualisierte Buch Reviews web-Anwendung verwendet ein [SQL Server 2008 Express Edition](https://www.microsoft.com/express/sql/default.aspx) Datenbank, die in der Website s gespeichert ist `App_Data` Ordner (`~/App_Data/Reviews.mdf`). Wenn Sie SQL Server 2008 auf dem Computer installiert haben, sollten die Demo ohne Fehler ausführen. Haben eine ältere Version von SQL Server können Sie installieren den freien SQL Server 2008 Express Edition oder können Sie die Datenbank, die Skripts in diesem Lernprogramm s verfügbar herunterladen, um die Datenbank selbst zu erstellen.

Die `Reviews.mdf` Datenbank enthält vier Tabellen:

- `Genres`-enthält einen Datensatz für jede "Genre", wie z. B. Technology Idee und Business.
- `Books`-enthält einen Datensatz für jede Überprüfung mit Spalten wie `Title`, `GenreId`, `ReviewDate`, und `Review`, o. ä.
- `Authors`– enthält Informationen zu jeder Autor, die zu ein überprüfter Buch beigetragen hat.
- `BooksAuthors`-eine m: n-Join-Tabelle, der angibt, welche Autoren welche Bücher geschrieben haben.
  

Abbildung 1 zeigt ein Diagramm, ER diese vier Tabellen.


[![Die Buch Reviews Web Application-s-Datenbank ist, besteht aus den vier Tabellen](deploying-a-database-cs/_static/image2.jpg)](deploying-a-database-cs/_static/image1.jpg) 

**Abbildung 1**: das Buch Reviews Web-s-Datenbank ist der vier Tabellen umfassen ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-a-database-cs/_static/image3.jpg))


Die vorherige Version der Website Buch Reviews hatte eine separaten ASP.NET-Seite für jedes Buch. Beispielsweise ist eine Seite mit dem Namen `~/Tech/TYASP35.aspx` , enthalten die Prüfung für *Schulen selbst ASP.NET 3.5 in 24 Stunden*. Diese neue datengesteuerte Version der Website hat, prüft der in der Datenbank und eine einzelne ASP.NET-Seite Review.aspx?ID= gespeichert*BookId*, die anzeigt, dass der Überprüfung für das angegebene Buch. Ebenso ist ein Genre.aspx?ID=*GenreId* Seite mit einer Liste von überprüften Bücher in der angegebenen "Genre".

Abbildung 2 und 3 zeigt die `Genre.aspx` und `Review.aspx` Seiten in Aktion. Beachten Sie die URL in der Adressleiste für jede Seite aus. In Abbildung 2 It s Genre.aspx? ID = C 47-82a0-c8ec75de7e0e der 85d164ba-1123-4. Da 85d164ba-1123-4c47-82a0-c8ec75de7e0e ist die `GenreId` Wert für die Technologie "Genre", die Seite "s" Überschrift Lesevorgänge "Technology untersucht" und der Aufzählung listet diese Reviews auf der Website, die unter diesem "Genre" liegen.


[![Die Seite "Genre" Technologie "](deploying-a-database-cs/_static/image5.jpg)](deploying-a-database-cs/_static/image4.jpg) 

**Abbildung 2**: die Technologie "Genre" Page ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-a-database-cs/_static/image6.jpg))


[![Die Prüfung für Schulen selbst ASP.NET 3.5 in 24 Stunden](deploying-a-database-cs/_static/image8.jpg)](deploying-a-database-cs/_static/image7.jpg) 

**Abbildung 3**: die Prüfung für *Schulen selbst ASP.NET 3.5 in 24 Stunden* ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-a-database-cs/_static/image9.jpg))


Die Web-Anwendung prüft Buch enthält auch ein Bereich "Verwaltung", in dem Administratoren können hinzufügen, bearbeiten und löschen Genres Reviews und Informationen erstellen. Derzeit kann Besucher den Bereich "Verwaltung" zugreifen. In einem späteren Lernprogramm wir Unterstützung für Benutzerkonten hinzufügen und lassen Sie nur autorisierte Benutzer in der Administrationsseiten.

Wenn Sie die Anwendung Buch Reviews Bitte denken Sie daran, ihren Zweck zur Veranschaulichung eine datengesteuerte Anwendung bereitstellen herunterladen. Es ist keine bewährte so weit wie Anwendungsentwurf verwendet werden. Beispielsweise ist keine separate Data Access Layer (DAL); die ASP.NET-Seiten kommunizieren direkt mit der Datenbank über die SqlDataSource-Steuerelement oder ADO.NET-Code in ihren Code-Behind-Klassen. Eine eingehendere Betrachtung von datengesteuerten Anwendungen, die mit einer mehrstufigen Architektur erstellen, finden Sie in meinem [ *arbeiten mit Daten* Lernprogramme](../../data-access/index.md).

## <a name="databases-on-development-versus-production"></a>Datenbanken auf die Entwicklung und Produktion

Beim Starten von Entwicklung einer datengesteuerten Webanwendung müssen Sie eine Datenbankverbindungszeichenfolge angeben, die der App-Details zum Herstellen einer Verbindung mit der Datenbank bereitstellt. Diese Verbindungszeichenfolge gibt an, unter anderem, Datenbankserver, den Datenbanknamen und Sicherheitsinformationen. In den meisten Fällen die von der Anwendung während der Entwicklung verwendeten Datenbank unterscheidet sich von die Datenbank verwendet wird, wenn es s in der Produktion. Es gibt viele Vorteile des Verwendens von anderen Datenbanken für Entwicklung und Produktion. Mit einem anderen Entwicklung bedeutet, dass Sie die Datenbank Don ' t kümmern, versehentlich ändern oder Löschen von live-Daten haben. Außerdem können Sie das Speichern in dummy Testdaten oder wichtige Änderungen in das Datenmodell ohne die Auswirkungen auf die Anwendung in der Produktion kümmern. Der Nachteil daran, dass eine andere Datenbank in der Entwicklungs- und produktionsumgebungen ist, wenn die Anwendung ist der Datenbank und alle relevanten Änderungen am Datenbankschema s bereitgestellt oder Daten müssen ebenfalls bereitgestellt werden.

Vor der ersten Bereitstellung nur eine Instanz der Datenbank vorhanden ist, und diese Instanz befindet sich in der Entwicklungsumgebung. Beim Bereitstellen der Anwendung bis hin zur Produktion zum ersten Mal müssen wir nicht nur die erforderlichen Dateien für serverseitige und clientseitige kopieren, sondern kopieren Sie außerdem die Datenbank aus der Entwicklungsumgebung in der produktionsumgebung. Dies ist, in dem wir rechten stehen jetzt mit der Webanwendung des Buchs Reviews - die Datenbank befindet sich in der `App_Data` Ordner, in unserem Entwicklungsumgebung, enthält jedoch nicht noch in der produktionsumgebung abgelegten.

Nachdem die Anwendung bereitgestellt wurde sind zwei Kopien der Datenbank. Bei fortschreitender die Anwendung werden möglicherweise neue Funktionen hinzugefügt, eine Änderung des Datenmodells Kombination (z. B. vorhandenen Tabellen neue Spalten hinzufügen, Änderungen an vorhandenen Spalten neue Tabellen hinzugefügt werden und so weiter). Wenn die Webanwendung als Nächstes bereitgestellt wird, werden die Änderungen auf die Datenbank in der Entwicklungsumgebung angewendet, seit die letzten Bereitstellung auf der Produktionsdatenbank angewendet werden muss. Einige Strategien für die Verwaltung von diesem Prozess werden in einem späteren Lernprogramm erläutert. Dieses Lernprogramm konzentriert sich auf die gesamte Datenbank in der Entwicklungsumgebung für die Produktion bereitstellen.

## <a name="deploying-the-database-to-the-production-environment"></a>Bereitstellen der Datenbank in der Produktionsumgebung

Im weiteren Verlauf dieses Lernprogramms untersucht, wie die Datenbank aus der Entwicklungsumgebung in der produktionsumgebung bereitstellen. Wenn Sie zusammen Sie vorgehen muss sicherstellen, dass Ihr Konto mit Ihrem Webhostinganbieter Microsoft SQL Server enthält Datenbanken unterstützen. Sie müssen auch einige Informationen zur hand, nämlich den Namen des Datenbankservers, den Datenbanknamen und den Benutzernamen und das Kennwort für die Verbindung mit der Datenbank verwendet haben.

Wie weiter oben in diesem Lernprogramm erwähnt, ist die Buch Reviews Website s-Datenbank einer SQL Server 2008 Express Edition-Datenbank gespeichert, der `App_Data` Ordner. Es würde, Grund, das Bereitstellen einer solchen Datenbank so einfach wie das Kopieren von wäre stehen die `App_Data` Ordner aus der Entwicklungsumgebung in der produktionsumgebung. Die meisten Web Hostanbieter unterstützen jedoch keine hosting Datenbanken in der `App_Data` Ordner aus Gründen der Sicherheit. Stattdessen geben Webhosts ein Konto auf einem SQL Server-Datenbankserver in ihrer Umgebung. Bereitstellen der Datenbank in Ihrer Entwicklungsumgebung in der produktionsumgebung erfordert Abrufen Ihrer Datenbank auf dem Webserver Host s-Datenbank registriert.

Wie erhalten Sie die Datenbank aus der Entwicklungsumgebung in der produktionsumgebung? Es gibt mehrere Möglichkeiten, dieses Ziel erreichen, je nachdem, welche Dienste von Ihrem Web Host bietet. Mit einigen Hosts, z. B. DiscountASP.NET, können Sie eine Sicherung der Datenbank oder den tatsächlichen FTP `.mdf` zu Ihrer Website und klicken Sie dann in der Systemsteuerung stellt die Sicherungsdatei oder das Anfügen der `.mdf` Datei mit dem SQL Server-Datenbankserver. Mit solchen Tools Bereitstellen der Datenbank ist so einfach wie das Kopieren der `App_Data` Ordner für die produktionsumgebung und klicken Sie dann über die Systemsteuerung anzufügen. Dies ist vielleicht die einfachste und schnellste Methode zum Veröffentlichen der Datenbank zum ersten Mal.

Ein anderer Ansatz ist die Verwendung der Datenbankveröffentlichungs-Assistenten. Der Datenbankveröffentlichungs-Assistent ist ein Windows-desktop-Anwendung, die die SQL-Befehle zum Erstellen von s Datenbankschema - Tabellen, gespeicherte Prozeduren, Sichten, benutzerdefinierte Funktionen usw.- und optional die Daten in die Tabellen generieren. Sie können dann eine Verbindung herstellen, mit dem Web Host Anbieter s Datenbankserver über SQL Server Management Studio und führen Sie dieses Skript aus, um die Datenbank auf die Produktion zu duplizieren. Noch besser, wenn Ihre Webhostinganbieter Microsoft s unterstützt [Datenbank-Veröffentlichungsdienste](http://www.codeplex.com/sqlhost/Wiki/View.aspx?title=Database%20Publishing%20Services&amp;referringTitle=Home) können Sie das Skript generiert, die für den Datenbankveröffentlichungs-Assistent automatisch auf dem Datenbankserver in Ihrem Auftrag ausgeführt haben. Da der Datenbankveröffentlichungs-Assistenten ein Skript, der das s-Datenbank-Schema und Daten erstellt generiert, funktioniert es unabhängig davon, ob Ihre Webhostinganbieter Funktionen wie das Anfügen einer hochgeladenen bietet `.mdf` Datei.

### <a name="generating-the-sql-commands-to-create-the-database-schema-and-data-using-the-database-publishing-wizard"></a>So erstellen das Datenbankschema und die Daten, die mit der Datenbank, die Publishing-Assistent die SQL-Befehle generieren

Lassen Sie s mit den Datenbankveröffentlichungs-Assistenten zum Bereitstellen der Buch Reviews-Datenbank für die Produktion durchlaufen. Bei Verwendung von Visual Studio 2008 oder höher ist der Datenbankveröffentlichungs-Assistenten bereits installiert. Wenn Sie Visual Studio 2005 verwenden, müssen Sie zuerst [herunterladen und Installieren von](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en) des Assistenten.

Öffnen Sie Visual Studio, und navigieren Sie zu der `Reviews.mdf` Datenbank. Wenn Sie Visual Web Developer verwenden, wechseln Sie zu der Datenbank-Explorer; Wenn Sie Visual Studio verwenden, verwenden Sie den Server-Explorer. Abbildung 4 zeigt die `Reviews.mdf` Datenbank in der Datenbank-Explorer in Visual Web Developer. Wie in Abbildung 4 gezeigt, die `Reviews.mdf` Datenbank besteht aus vier Tabellen, drei gespeicherten Prozeduren und eine benutzerdefinierte Funktion.


[![Suchen Sie die Datenbank in der Datenbank-Explorer oder den Server-Explorer](deploying-a-database-cs/_static/image11.jpg)](deploying-a-database-cs/_static/image10.jpg) 

**Abbildung 4**: Suchen Sie die Datenbank in der Datenbank-Explorer oder den Server-Explorer ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-a-database-cs/_static/image12.jpg))


Mit der rechten Maustaste auf den Datenbanknamen, und wählen Sie die Option "Für Anbieter veröffentlichen" aus dem Kontextmenü. Dadurch wird der Datenbankveröffentlichungs-Assistent (siehe Abbildung 5). Klicken Sie auf Weiter, um nach den Begrüßungsbildschirm.


[![Der Datenbank, die Publishing Splash-Bildschirm des Assistenten](deploying-a-database-cs/_static/image14.jpg)](deploying-a-database-cs/_static/image13.jpg) 

**Abbildung 5**: die Datenbank Publishing-Assistent Begrüßungsbildschirm ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-a-database-cs/_static/image15.jpg))


Die zweiten Bildschirm des Assistenten führt die Datenbanken, die für den Datenbankveröffentlichungs-Assistenten verfügbar und können Sie auswählen, ob für alle Objekte in der ausgewählten Datenbank, ein Skript oder die Objekte für das Skript auswählen. Wählen Sie die entsprechende Datenbank aus, und lassen Sie die Option "Skript alle Objekte in der ausgewählten Datenbank" überprüft.

> [!NOTE]
> Wenn Sie die Fehlermeldung "Es sind keine Objekte in der Datenbank *DatabaseName* der Typen, die von diesem Assistenten haben" beim Klicken auf "weiter auf dem Bildschirm in Abbildung 6 dargestellten" Stellen Sie sicher, dass der Pfad der Datenbankdatei nicht übermäßig lange ist. Es wurde festgestellt, dass dieser Fehler auftreten kann, wenn der Pfad zur Datenbankdatei zu lang ist.


[![Der Datenbank, die Publishing Splash-Bildschirm des Assistenten](deploying-a-database-cs/_static/image17.jpg)](deploying-a-database-cs/_static/image16.jpg) 

**Abbildung 6**: die Datenbank Publishing-Assistent Begrüßungsbildschirm ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-a-database-cs/_static/image18.jpg))


Aus dem nächsten Bildschirm können Sie eine Skriptdatei generieren oder wenn Ihre Web-Host dies unterstützt, veröffentlichen die Datenbank direkt mit dem Web Host-Anbieter-s-Datenbankserver. Wie Abbildung 7 zeigt ein Beispiel, habe ich das Skript in die Datei geschrieben `C:\REVIEWS.MDF.sql`.


[![Skript für die Datenbank in eine Datei oder direkt in Ihren Webhostinganbieter veröffentlichen](deploying-a-database-cs/_static/image20.jpg)](deploying-a-database-cs/_static/image19.jpg) 

**Abbildung 7**: Skript für die Datenbank in eine Datei oder direkt in Ihren Webhostinganbieter veröffentlichen ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-a-database-cs/_static/image21.jpg))


Im folgenden Bildschirm werden Sie für eine Vielzahl von Optionen für Skripts aufgefordert. Sie können angeben, ob das Skript Drop-Anweisungen, um diese vorhandenen Objekte entfernen enthalten soll. Der Standardwert lautet "true", das ist in Ordnung, wenn eine Datenbank zum ersten Mal bereitstellen. Sie können auch angeben, ob die Zieldatenbank, SQL Server 2000, SQL Server 2005 oder SQL Server 2008 ist. Sie können schließlich angeben, ob das Schema und Daten, das Skript nur die Daten oder nur das Schema. Das Schema ist die Auflistung der Datenbankobjekte, Tabellen, gespeicherte Prozeduren, Sichten und So weiter. Die Daten sind die Informationen in den Tabellen befinden.

Wie in Abbildung 8 dargestellt, ich Ve wurde so konfiguriert, dass um vorhandene Datenbankobjekte zu löschen Assistenten zum Generieren von Skripts für eine SQL Server 2008-Datenbank sowie das Schema und die Daten zu veröffentlichen.


[![Geben Sie die Publishing Optionen](deploying-a-database-cs/_static/image23.jpg)](deploying-a-database-cs/_static/image22.jpg) 

**Abbildung 8**: Geben Sie die Publishing Optionen ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-a-database-cs/_static/image24.jpg))


Die letzten zwei Bildschirme fassen zusammen, die Aktionen, die sind im Begriff, die ausgeführt werden, und klicken Sie dann den Status des Skripts angezeigt wird. Das Ergebnis der Assistent ausgeführt wird, dass wir eine Skriptdatei mit der SQL-Befehle, die zum Erstellen der Datenbank für die Produktion und weisen Sie ihm den gleichen Daten wie bei der Entwicklung erforderlich sind.

### <a name="executing-the-sql-commands-on-the-production-environment-database"></a>Die SQL-Befehle ausführen in der Umgebung Produktionsdatenbank

Das Skript, das die SQL-Befehle zum Erstellen der Datenbank und seine Daten enthält, die verbleibt, werden nun mit dem Ausführen des Skripts auf der Produktionsdatenbank. Einige Web Host-Anbieter bieten ein Textfeld in der Systemsteuerung, in dem Sie SQL-Befehle zum Ausführen auf einer bestehenden Datenbank eingeben können. Wenn Sie eine sehr große Skriptdatei haben, verknüpfen Sie diese Option möglicherweise nicht funktioniert (die `REVIEWS.MDF.sql` Skriptdatei ist über 425 KB groß, z. B.).

Ein besserer Ansatz ist zur direkten Verbindung mit dem Produktions-Datenbankserver mithilfe von SQL Server Management Studio (SSMS). Wenn Sie eine nicht - Express Edition von SQL Server auf Ihrem Computer installiert haben müssen Sie wahrscheinlich bereits SSMS installiert. Andernfalls können Sie [herunterladen und installieren](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en) kostenlos von SQL Server Management Studio Express Edition.

Starten Sie SSMS aus, und Verbinden mit dem Web Host s-Datenbankserver mit den Informationen, die von Ihrem Web-Host-Anbieter bereitgestellt.


[![Herstellen einer Verbindung mit dem Web Host-Anbieter-s-Datenbankserver](deploying-a-database-cs/_static/image26.jpg)](deploying-a-database-cs/_static/image25.jpg) 

**Abbildung 9**: Herstellen einer Verbindung mit Ihren Webhostinganbieter s Datenbankserver ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-a-database-cs/_static/image27.jpg))


Erweitern Sie die Registerkarte "Datenbanken", und suchen Sie Ihre Datenbank. Klicken Sie auf die Schaltfläche "neue Abfrage" in der oberen linken Ecke der Symbolleiste, fügen Sie in der SQL-Befehle aus der Skriptdatei, die von der Datenbankveröffentlichungs-Assistent erstellt, und klicken Sie auf die Schaltfläche ausführen, um diese Befehle auf dem Produktionsserver Datenbank auszuführen. Wenn Ihre Skriptdatei besonders groß ist, dauert es mehrere Minuten zur Ausführung der Befehle.


[![Herstellen einer Verbindung mit dem Web Host-Anbieter-s-Datenbankserver](deploying-a-database-cs/_static/image29.jpg)](deploying-a-database-cs/_static/image28.jpg) 

**Abbildung 10**: Herstellen einer Verbindung mit Ihren Webhostinganbieter s Datenbankserver ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-a-database-cs/_static/image30.jpg))


S alles ist es! An diesem Punkt wurde bis hin zur Produktion die Entwicklungsdatenbank dupliziert. Wenn Sie die Datenbank in SSMS aktualisieren sollten die neue Datenbankobjekte angezeigt werden. Abbildung 11 zeigt Produktions s, Datenbanktabellen, gespeicherten Prozeduren und benutzerdefinierte Funktionen, die die in der Entwicklungsdatenbank spiegeln. Und da wir den Datenbankveröffentlichungs-Assistenten zum Veröffentlichen von Daten angewiesen, die Tabellen in der Produktion s die gleichen Daten wie die Development-s-Datenbanktabellen zu dem Zeitpunkt, die der Assistent ausgeführt wurde. Abbildung 12 zeigt die Daten in der `Books` Tabelle in der Produktionsdatenbank.


[![Die Datenbankobjekte haben in der Produktionsdatenbank dupliziert](deploying-a-database-cs/_static/image32.jpg)](deploying-a-database-cs/_static/image31.jpg) 

**Abbildung 11**: die Datenbank Objekte haben dupliziert wurde in der Produktionsdatenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-a-database-cs/_static/image33.jpg))


[![Die Produktionsdatenbank enthält die gleichen Daten wie in der Entwicklungsdatenbank](deploying-a-database-cs/_static/image35.jpg)](deploying-a-database-cs/_static/image34.jpg) 

**Abbildung 12**: die Produktionsdatenbank enthält die gleichen Daten wie in der Entwicklungsdatenbank ([klicken Sie hier, um das Bild in voller Größe angezeigt](deploying-a-database-cs/_static/image36.jpg))


Zu diesem Zeitpunkt haben wir nur die Entwicklungsdatenbank bis hin zur Produktion bereitgestellt. Wir noch nicht zur Bereitstellung der Webanwendung selbst gesucht oder untersucht, welche Änderungen an der Konfiguration erforderlich sind, damit die Anwendung in Produktion die Produktionsdatenbank verwenden. Wir behandeln dieser Probleme in den nächsten Lernprogrammen!

## <a name="summary"></a>Zusammenfassung

Bereitstellen einer datengesteuerte Webanwendung erfordert Kopieren der Datenbank, die während der Entwicklung in der produktionsumgebung verwendet. Viele Web Hostanbieter bieten Tools vereinfachen das Bereitstellen einer Datenbank. FTP Sie z. B. mit DiscountASP.NET Datenbankansicht `.mdf` Datei (oder eine Sicherung) und fügen Sie die Datenbank mit dem Datenbankserver her, in der Systemsteuerung. Eine andere Möglichkeit, die funktioniert, unabhängig davon, welche Funktionen Ihrer Web Host Anbieter angeboten ist Microsoft s Datenbankveröffentlichungs-Assistent-Tool, die ein Skript für SQL-Befehle zum Erstellen der Entwicklung s Datenbankschema und die Daten generiert. Sobald dieses Skript generiert wurde, können Sie sie auf die Produktionsdatenbank ausführen.

Die Buch Reviews Web Application s-Datenbank auf Produktion befindet können wir die Anwendung bereitstellen. Allerdings s-Konfigurationsinformationen der Web-Anwendung gibt die Verbindungszeichenfolge für die Datenbank an, und dieser Verbindungszeichenfolge verweist auf die Entwicklungsdatenbank. Wir müssen diese Verbindungszeichenfolgendaten zu aktualisieren, wenn den Standort bis hin zur Produktion bereitstellen. Im nächste Lernprogramm betrachtet diese Konfigurationsunterschiede und führt Sie durch die Schritte erforderlich, um die Website für die datengesteuerten Buch Reviews bis hin zur Produktion veröffentlichen.

Viel Spaß beim Programmieren!

#### <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu den Themen in diesem Lernprogramm erläutert finden Sie in den folgenden Ressourcen:

- [Herunterladen der Microsoft SQL Server-Datenbank, die Publishing-Assistent 1.1](https://www.microsoft.com/downloads/details.aspx?familyid=56E5B1C5-BF17-42E0-A410-371A838E570A&amp;displaylang=en)
- [Herunterladen der Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?FamilyId=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796&amp;displaylang=en)

>[!div class="step-by-step"]
[Zurück](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
[Weiter](configuring-the-production-web-application-to-use-the-production-database-cs.md)
