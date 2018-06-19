---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Erstellen Sie ein neues ASP.NET MVC-Projekt | Microsoft Docs
author: microsoft
description: Schritt 1 wird gezeigt, wie die grundlegenden NerdDinner Anwendungsstruktur an Position eingefügt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: d15ca67f0ddd8db6842bc5112996ae2dee433536
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869256"
---
<a name="create-a-new-aspnet-mvc-project"></a>Erstellen eines neuen ASP.NET MVC-Projekts
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 1 mit einem kostenlosen ["NerdDinner" Anwendung Lernprogramm](introducing-the-nerddinner-tutorial.md) , die Durchläufe-durch die Schritte zum Erstellen einer kleinen, aber abgeschlossen haben, Webanwendung, die mithilfe von ASP.NET MVC-1.
> 
> Schritt 1 wird gezeigt, wie die grundlegenden NerdDinner Anwendungsstruktur an Position eingefügt.
> 
> Bei Verwendung von ASP.NET MVC 3 empfehlen wir führen Sie die [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Lernprogramme.


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner, Schritt 1: Datei -&gt;neues Projekt

Wir beginnen, unsere NerdDinner Anwendung durch Auswählen der **Datei -&gt;neues Projekt** Menüelement in Visual Studio 2008 oder die kostenlose Visual Web Developer 2008 Express.

Hierdurch wird das Dialogfeld "Neues Projekt" angezeigt. Um eine neue ASP.NET MVC-Anwendung zu erstellen, müssen wir wählen Sie den Knoten "Web" auf der linken Seite des Dialogfelds und wählen Sie dann die Projektvorlage "ASP.NET MVC-Webanwendung" auf der rechten Seite:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Wichtig: Stellen Sie sicher, dass Sie heruntergeladen und ASP.NET MVC – andernfalls es in das Dialogfeld "Neues Projekt" angezeigt wird nicht installiert haben. Können Sie V2 von der [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) , wenn Sie es noch nicht installiert haben (ASP.NET MVC finden Sie in der "Web Platform -&gt;Frameworks und Laufzeiten" Abschnitt).*

Das neue Projekt Namen, die wir werden zum Erstellen von "NerdDinner", und klicken Sie dann auf die Schaltfläche "ok", um ihn zu erstellen.

Beim Klicken auf "ok" wird Visual Studio ein Dialogfeld angezeigt, die wir erstellen Sie ein Komponententestprojekt für die neue Anwendung auch optional auffordert. Testprojekt für diese Komponente ermöglicht es, zu der automatisierten Tests zu erstellen, durch die die Funktionalität und Verhalten der Anwendung überprüft (etwas wir behandeln wie weiter unten in diesem Lernprogramm Aufgabe).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

Die "Testframework" Dropdownliste oben im Dialogfeld wird aufgefüllt, alle verfügbaren ASP.NET-MVC Unit Test Projektvorlagen auf dem Computer installiert. Versionen können für MBUnit, NUnit und XUnit heruntergeladen werden. Die integrierte Visual Studio-Komponententest-Frameworks wird ebenfalls unterstützt.

*Hinweis: Die Visual Studio-Komponententestframework ist nur mit Visual Studio 2008 Professional und höheren Versionen verfügbar. Wenn Sie Visual Studio 2008 Standard Edition oder Visual Web Developer 2008 Express verwenden, müssen Sie herunterladen und installieren die Erweiterungen NUnit, MBUnit oder XUnit für ASP.NET MVC, in der Reihenfolge für diesen Dialog angezeigt werden. Das Dialogfeld wird nicht angezeigt, wenn keine Testframeworks installiert.*

Wir verwenden den Standardnamen für die "NerdDinner.Tests" für das Testprojekt, das wir erstellen, und verwenden Sie die Option für "Visual Studio Einheit testen" Framework. Beim Klicken auf erstellt die Schaltfläche "ok" Visual Studio eine Lösung für uns darin - eine für die Webanwendung und eine für unsere Komponententests zwei Projekte:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Untersuchen die Verzeichnisstruktur NerdDinner

Wenn Sie eine neue ASP.NET MVC-Anwendung mit Visual Studio erstellen, fügt automatisch eine Reihe von Dateien und Verzeichnisse zum Projekt hinzu:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

ASP.NET MVC-Projekte in der Standardeinstellung sind Verzeichnisse vorhanden sechs auf oberster Ebene:

| **Verzeichnis** | **Purpose** |
| --- | --- |
| **/Controllers** | Wo Sie Controllerklassen platziert, die URL-Anforderungen zu verarbeiten |
| **/Models** | Legen Sie Sie, in denen Klassen, die darstellen und Bearbeiten von Daten |
| **/Views** | In dem Sie Benutzeroberflächen-Vorlagendateien einfügen, die für renderingausgabe zuständig sind |
| **/Scripts** | Hier können Sie JavaScript-Bibliothek-Dateien und Skripts (. js) einfügen |
| **/Content** | Hier können Sie CSS- und Bilddateien und anderen nicht-dynamische/nicht-JavaScript-Inhalt einfügen |
| **/App\_Data** | Sofern Sie Datendateien gespeichert, Lese-/Schreibzugriff werden soll. |

ASP.NET MVC ist diese Struktur nicht erforderlich. In der Tat Entwickler, die auf große Anwendungen arbeiten in der Regel unterteilt die Anwendung oben an verschiedenen Projekten, die es mehr verwaltbar zu machen (z. B.: Modellklassen Daten gehen Sie jedoch oftmals in einem separaten Klassenbibliotheksprojekt aus der Webanwendung). Die Standardprojektstruktur bieten jedoch nice Directory Standardkonvention, mit denen wir unsere anwendungsaspekten sauber zu halten.

Wenn wir das Verzeichnis /Controllers erweitern fest wir, dass Visual Studio dem Projekt zwei Controllerklassen – HomeController "und" AccountController – standardmäßig hinzugefügt:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Wenn wir das Verzeichnis /Views erweitern, finden wir drei Unterverzeichnisse – / Home, AccountType und Beispiele – sowie mehrere Vorlage, die darin enthaltenen Dateien standardmäßig ebenfalls dem Projekt hinzugefügt wurden:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Wenn wir die Scripts-Verzeichnisse und/Content erweitern, wird erkennbar, dass eine "Site.CSS" ändern-Datei, die zum Formatieren von HTML stets auf dem Standort verwendet wird, als auch JavaScript-Bibliotheken, die ASP.NET AJAX und jQuery aktivieren können innerhalb der Anwendung unterstützen:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Wenn wir das Projekt NerdDinner.Tests erweitern finden wir zwei Klassen, die Komponententests für unsere Controllerklassen enthalten:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Diese Standarddateien hinzugefügt, die von Visual Studio bieten uns eine grundlegende Struktur für eine funktionierende Anwendung - mit Startseite zur Seite, die Anmeldung/Abmeldung/Registrierung Seiten Konto und ein nicht behandelter Fehler-Seite (alle wired von Volltextkatalogen und direktes arbeiten) abgeschlossen.

### <a name="running-the-nerddinner-application"></a>Ausführen der Anwendung NerdDinner

Wir können das Projekt ausführen, indem Sie die Auswahl entweder die **Debug -&gt;Debuggen** oder **Debug -&gt;Starten ohne Debugging** Menüelemente:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Dies wird starten integrierte ASP.NET Web-Server, der mit Visual Studio enthalten ist, und führen Sie die Anwendung:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Im folgenden ist die Startseite für unser neues Projekt (URL: "/") bei der Ausführung:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Klicken auf die Registerkarte "About" wird eine zu Seite (URL: "/ Home oder bald"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Auf den Link "Anmelden", in der oberen rechten Ecke, führt zu einer Anmeldeseite (URL: "/ Account/LogOn")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Wenn wir haben ein Anmeldekonto klicken wir den Link Register (URL: "/ Account/Register") zu erstellen:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Der Code zum Implementieren der oben genannten Home, und die Abmeldung / register Funktionalität wurde in der Standardeinstellung hinzugefügt, wenn wir unser neue Projekt erstellt. Wir werden sie als Ausgangspunkt der Anwendung verwenden.

### <a name="testing-the-nerddinner-application"></a>Testen der Anwendung NerdDinner

Wenn wir die Professional Edition oder eine höhere Version von Visual Studio 2008 verwenden, können wir die integrierten innerhalb von Visual Studio-IDE-Unterstützung für Komponententests, zum Testen des Projekts:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Auswahl einer der oben erwähnten Optionen wird im Bereich "Testergebnisse" in der IDE öffnen, und geben Sie uns mit erfolgreich/Fehler-Status für die 27 Komponententests in unserer neuen Projekt enthalten, die die integrierte Funktion abdecken:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Weiter unten in diesem Lernprogramm wir sprechen Sie mehr über automatisierte Tests und fügen zusätzliche Komponententests, die die Anwendungsfunktionalität abdecken, die wir implementieren.

### <a name="next-step"></a>Nächster Schritt

Wir haben jetzt eine grundlegende Anwendungsstruktur vorhanden. Nehmen wir jetzt [Erstellen einer Datenbank zum Speichern von unseren Anwendungsdaten](create-a-database.md).

> [!div class="step-by-step"]
> [Zurück](introducing-the-nerddinner-tutorial.md)
> [Weiter](create-a-database.md)
