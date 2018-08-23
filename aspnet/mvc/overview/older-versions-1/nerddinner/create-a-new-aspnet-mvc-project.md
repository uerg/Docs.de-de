---
uid: mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
title: Erstellen eines neuen ASP.NET MVC-Projekts | Microsoft-Dokumentation
author: microsoft
description: Schritt 1 erfahren Sie, wie Sie die grundlegende Struktur des NerdDinner-Anwendung platziert.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 7e0e9928-8fdc-4b74-9882-55fac0976628
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-new-aspnet-mvc-project
msc.type: authoredcontent
ms.openlocfilehash: 3f34f17aa35dbfed2d52daf615c8dc81be6e7847
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835327"
---
<a name="create-a-new-aspnet-mvc-project"></a>Erstellen eines neuen ASP.NET MVC-Projekts
====================
durch [Microsoft](https://github.com/microsoft)

[PDF herunterladen](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Dies ist Schritt 1 von einem kostenlosen ["NerdDinner"-webanwendungstutorial](introducing-the-nerddinner-tutorial.md) , die führt – Exemplarische Vorgehensweise erstellen eine kleine, jedoch abgeschlossen haben, Web-Anwendung mithilfe von ASP.NET MVC-1.
> 
> Schritt 1 erfahren Sie, wie Sie die grundlegende Struktur des NerdDinner-Anwendung platziert.
> 
> Wenn Sie ASP.NET MVC 3 verwenden, sollten Sie Sie folgen den [erste Schritte mit MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) oder [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) Tutorials.


## <a name="nerddinner-step-1-file-gtnew-project"></a>NerdDinner, Schritt 1: Datei -&gt;neues Projekt

Zunächst müssen wir unsere NerdDinner-Anwendung auswählen der **Dateien&gt;neues Projekt** Menüelement innerhalb von Visual Studio 2008 oder die kostenlose Visual Web Developer 2008 Express.

Hierdurch wird das Dialogfeld "Neues Projekt" angezeigt. Um eine neue ASP.NET MVC-Anwendung zu erstellen, werden wir wählen Sie den Knoten "Web" auf der linken Seite des Dialogfelds, und wählen Sie dann die Projektvorlage "ASP.NET MVC-Webanwendung", auf der rechten Seite:

![](create-a-new-aspnet-mvc-project/_static/image1.png)

*Wichtig: Stellen Sie sicher, dass Sie heruntergeladen und installiert ASP.NET MVC – andernfalls ihn in das Dialogfeld "Neues Projekt" nicht angezeigt haben. Version 2 der können die [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) , wenn Sie es noch nicht installiert haben (ASP.NET MVC finden Sie in der "Web Platform -&gt;Frameworks und Laufzeiten" Abschnitt).*

Das neue Projekt nennen müssen wir zum Erstellen von "NerdDinner", und klicken Sie dann auf die Schaltfläche "ok", um ihn zu erstellen.

Beim Klicken auf "ok" wird Visual Studio zusätzliche Dialogfeld angezeigt, die wir erstellen Sie ein Komponententestprojekt für die neue Anwendung auch optional auffordert. Diese Komponententestprojekt kann wir zum Erstellen automatisierter Tests, die die Funktionalität und Verhalten der Anwendung zu überprüfen (etwa darum wie Aufgaben weiter unten in diesem Tutorial).

![](create-a-new-aspnet-mvc-project/_static/image2.png)

Die Dropdownliste "Test-Framework" im Dialogfeld für die oben genannten wird mit allen verfügbaren ASP.NET MVC Komponententest-Projektvorlagen auf dem Computer installierten aufgefüllt. Versionen können für NUnit, XUnit und MBUnit heruntergeladen werden. Die integrierte Visual Studio-Komponententest-Framework wird ebenfalls unterstützt.

*Hinweis: Das Visual Studio-Komponententestframework ist nur mit Visual Studio 2008 Professional und höheren Versionen verfügbar. Wenn es sich bei Verwendung von Visual Studio 2008 Standard Edition oder Visual Web Developer 2008 Express müssen Sie zum Herunterladen und installieren die NUnit, MBUnit oder XUnit-Erweiterungen für ASP.NET MVC, in der Reihenfolge für diesen Dialog angezeigt werden. Das Dialogfeld wird nicht angezeigt, wenn es keinen Testframeworks aus installiert sind.*

Wir verwenden den Standardnamen "NerdDinner.Tests" für das Testprojekt, das wir erstellen und verwenden Sie die Framework-Option "Visual Studio Unit Test". Beim Klicken auf wird die Schaltfläche "ok" Visual Studio eine Lösung für uns mit zwei Projekten darin - eines für die Webanwendung und eines für unsere Komponententests erstellt:

![](create-a-new-aspnet-mvc-project/_static/image3.png)

### <a name="examining-the-nerddinner-directory-structure"></a>Untersuchen die NerdDinner-Verzeichnisstruktur

Wenn Sie eine neue ASP.NET MVC-Anwendung mit Visual Studio erstellen, fügt es automatisch eine Anzahl von Dateien und Verzeichnisse zum Projekt hinzu:

![](create-a-new-aspnet-mvc-project/_static/image4.png)

ASP.NET MVC-Projekte in der Standardeinstellung haben sechs Verzeichnisse der obersten Ebene:

| **Verzeichnis** | **Zweck** |
| --- | --- |
| **/ Controller** | Ordnen Sie Controller-Klassen, die URL-Anforderungen verarbeiten. |
| **/ Modelle** | Ordnen Sie Klassen, die darstellen, und Bearbeiten von Daten |
| **/ Views** | Ordnen Sie UI-Vorlagendateien, die für die renderingausgabe verantwortlich sind |
| **/Scripts** | Fügen Sie JavaScript-Bibliothek-Dateien und Skripts (. js) hinzu |
| **/ Inhalte** | Fügen Sie CSS- und Bilddateien und andere nicht-dynamisch/nicht-JavaScript-Inhalte hinzu |
| **/ App\_Daten** | Wenn Sie die Datendateien gespeichert, die Lese-/Schreibzugriff werden soll. |

ASP.NET MVC ist diese Struktur nicht erforderlich. In der Tat Entwickler, die auf große Anwendungen arbeiten in der Regel partitioniert die Anwendung sich über mehrere Projekte, um es leichter zu machen (z. B.: Data Model-Klassen in einem separaten Klassenbibliothekprojekt häufig von der Webanwendung wechseln). Die Standardprojektstruktur, stellt jedoch gute Standardkonvention Verzeichnis bereit, mit denen wir unsere anwendungsaspekten zu halten.

Wenn wir das Verzeichnis /Controllers erweitern, werden wir feststellen, dass es sich bei Visual Studio dem Projekt zwei Controller-Klassen – "HomeController" und "AccountController" – standardmäßig hinzugefügt:

![](create-a-new-aspnet-mvc-project/_static/image5.png)

Wenn wir das Verzeichnis "/ Views" erweitern, werden wir drei Unterverzeichnisse: / Home "/ Account" und freigegebene – sowie mehrere Vorlagen, die Dateien in diese auch zum Projekt hinzugefügt wurden, standardmäßig feststellen:

![](create-a-new-aspnet-mvc-project/_static/image6.png)

Wenn wir den "/ Scripts"-Verzeichnissen und Inhaltsdatei erweitern, stellen wir fest, dass eine Datei "Site.CSS", die zum Formatieren von HTML auf der Website verwendet wird, sowie JavaScript-Bibliotheken, mit die ASP.NET AJAX und jQuery können innerhalb der Anwendung unterstützen:

![](create-a-new-aspnet-mvc-project/_static/image7.png)

Wenn wir erweitern, dass das Projekt NerdDinner.Tests finden wir zwei Klassen, die Komponententests für die Controllerklassen enthalten:

![](create-a-new-aspnet-mvc-project/_static/image8.png)

Diese Standarddateien hinzugefügt, die von Visual Studio bieten uns eine grundlegende Struktur für eine funktionierende Anwendung - Startseite angezeigt, über die Seite, kontoseiten für Anmeldung/Abmeldung/Registrierung und eine nicht behandelte Fehlerseite (alle verbunden und funktionsfähig ist, standardmäßig) abgeschlossen.

### <a name="running-the-nerddinner-application"></a>Ausführen der NerdDinner-Anwendung

Wir können das Projekt ausführen, durch Auswählen der **Debug -&gt;Debuggen starten** oder **Debug -&gt;Starten ohne Debugging** Menüelemente:

![](create-a-new-aspnet-mvc-project/_static/image9.png)

Dies startet den integrierten ASP.NET Web-Server, der mit Visual Studio enthalten ist, und die Anwendung auszuführen:

![](create-a-new-aspnet-mvc-project/_static/image10.png)

Im folgenden ist die Startseite für das neue Projekt (URL: "/") bei der Ausführung:

![](create-a-new-aspnet-mvc-project/_static/image11.png)

Klicken Sie auf der Registerkarte "About" wird eine Infoseite (URL: "/ Home/About"):

![](create-a-new-aspnet-mvc-project/_static/image12.png)

Klicken Sie auf den Link "Anmelden" auf der oberen rechten Ecke führt uns zu einer Anmeldeseite (URL: "/ Account/LogOn")

![](create-a-new-aspnet-mvc-project/_static/image13.png)

Wenn wir haben ein Anmeldekonto klicken wir auf den Link "Register" (URL: "/ Account/Register") zu erstellen:

![](create-a-new-aspnet-mvc-project/_static/image14.png)

Der Code zum Implementieren von der oben genannten privat, und der Abmeldung / register Funktionalität wurde in der Standardeinstellung hinzugefügt, wenn wir unser neues Projekt erstellt. Wir verwenden es als Ausgangspunkt der Anwendung.

### <a name="testing-the-nerddinner-application"></a>Testen der NerdDinner-Anwendung

Wenn wir die Professional Edition oder eine höhere Version von Visual Studio 2008 verwenden, können wir das integrierte UnitTests in Visual Studio-IDE-Unterstützung verwenden, auf um das Projekt zu testen:

![](create-a-new-aspnet-mvc-project/_static/image15.png)

Auswahl einer der oben genannten Optionen werden im Bereich "Testergebnisse" in der IDE zu öffnen und uns erfolgreich/Fehler-Status für die 27 Komponententests in unserer neuen Projekt enthalten, die die integrierte Funktion abdecken:

![](create-a-new-aspnet-mvc-project/_static/image16.png)

Weiter unten in diesem Tutorial wir sprechen über automatisierte Tests und fügen zusätzliche Komponententests, die die Funktion der Anwendung behandelt, die wir implementieren.

### <a name="next-step"></a>Nächster Schritt

Wir haben jetzt eine einfache Anwendungsstruktur vorhanden. Lassen Sie uns nun [erstellen Sie eine Datenbank zum Speichern unserer Anwendungsdaten](create-a-database.md).

> [!div class="step-by-step"]
> [Zurück](introducing-the-nerddinner-tutorial.md)
> [Weiter](create-a-database.md)
