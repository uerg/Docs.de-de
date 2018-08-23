---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: 'Teil 1: Datei -> Neues Projekt | Microsoft-Dokumentation'
author: JoeStagner
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 1 enthält die Übersicht über "und" Datei/neu Projekt.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: d2e4b36c9029e86eea9b09974839e96e9aa39ced
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828040"
---
<a name="part-1-file--new-project"></a>Teil 1: Datei -> Neues Projekt
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außerordentlich einfach es ist, erstellen Sie leistungsstarke, skalierbare Anwendungen für die .NET-Plattform. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 zu verwenden, um eine online-Store, einschließlich der Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 1 enthält die Übersicht über "und" Datei/neu Projekt.


## <a id="_Toc260221666"></a>  Übersicht über die

Dieses Tutorial bietet eine Einführung in ASP.NET WebForms. Wir langsam starten, damit für Anfänger auf Web-Entwicklung ist in Ordnung.

Die Anwendung, die wir erstellen, ist es sich um einen einfachen Onlineshop.

![](tailspin-spyworks-part-1/_static/image1.jpg)


Besucher können Produkte nach Kategorie durchsuchen:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Sie können ein einzelnes Produkt anzuzeigen und zu Einkaufswagen hinzufügen:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Sie können überprüfen, Einkaufswagen, entfernen alle Elemente, die sie nicht mehr benötigen:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Sie zur Kasse gehen werden sie aufgefordert.

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Nach dem Sortieren, sehen sie ein einfaches Bestätigungsbildschirm angezeigt:

![](tailspin-spyworks-part-1/_static/image7.jpg)


Wir beginnen, indem das Erstellen eines neuen ASP.NET WebForms-Projekts in Visual Studio 2010, und wir werden Funktionen zum Erstellen einer vollständigen funktionsfähigen Anwendung inkrementell hinzufügen. Dabei wird die behandelt mit Masterseiten für konsistente Seitenlayout, AJAX, Validierung, Benutzermitgliedschaft und mehr Zugriff auf die Datenbank, Listen und Raster, Update von Datenseiten, datenüberprüfung.

Sie Schritt für Schritt nachvollziehen können, oder Sie können die fertige Anwendung herunterladen [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Sie können entweder Visual Studio 2010 oder die kostenlose Visual Web Developer 2010 aus [ https://www.microsoft.com/express/Web/ ](https://www.microsoft.com/express/Web/). Um die Anwendung zu erstellen, können Sie entweder SQL Server oder die kostenlose SQL Server Express, die die Datenbank hosten.

## <a id="_Toc260221667"></a>  Datei / neu Projekt

Wir beginnen, indem Sie das neue Projekt über das Menü "Datei" in Visual Studio auswählen. Daraufhin wird das Dialogfeld "Neues Projekt".

![](tailspin-spyworks-part-1/_static/image8.jpg)

Ich wähle Visual c# / Web-Vorlagen auf der linken Seite, und wählen Sie dann die Vorlage "ASP.NET Web Application" in der mittleren Spalte. Benennen Sie Ihr Projekt TailspinSpyworks aus, und drücken Sie die Schaltfläche "OK".

![](tailspin-spyworks-part-1/_static/image9.jpg)

Dadurch wird das Projekt erstellt. Werfen wir einen Blick auf die Ordner, die in unserer Anwendung im Projektmappen-Explorer auf der rechten Seite enthalten sind.

![](tailspin-spyworks-part-1/_static/image10.jpg)

Die leere Projektmappe nicht vollständig leer ist – eine einfache Ordnerstruktur hinzugefügt:

![](tailspin-spyworks-part-1/_static/image1.png)

Beachten Sie die Konventionen, die von der Projektvorlage für ASP.NET 4 standardmäßig implementiert.

- Der Ordner "Konto" implementiert eine einfache Benutzeroberfläche für ASP. NET Subsystems für die Mitgliedschaft.
- Der Ordner "Scripts" dient als Repository für clientseitige JavaScript-Dateien, und die Core-jQuery-JS-Dateien werden standardmäßig zur Verfügung gestellt.
- Der Ordner "Stile" wird verwendet, um unsere Website Visuals (CSS-Stylesheets) organisieren

Wenn wir F5 drücken, um die Anwendung auszuführen und das Rendern der Seite "default.aspx" wird Folgendes angezeigt.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Ersetzen Sie die Datei "Style.CSS" aus der Standardvorlage für Web Forms, mit dem CSS-Klassen und das zugehörige Image-Dateien, die das visual Asthetics gerendert werden, die wir für unsere Anwendung Tailspin Spyworks möchten, ist unsere erste Anwendung-Erweiterung werden.

Danach rendert unsere Seite "default.aspx" wie folgt aus.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Beachten Sie, dass die Abbildung Links oben rechts auf der Seite und die Menüelemente, die auf die Masterseite hinzugefügt wurden. Zeigen Sie nur die Links "Sign In" und "Konto" auf Seiten, die vorhanden sind (generiert durch die Standardvorlage) und den Rest der Seiten, die wir implementiert wird, wenn wir unsere Anwendung erstellen.

Wir werden auch die Masterseite in das Verzeichnis Stile zu verschieben. Obwohl dies nur eine Einstellung ist kann es etwas einfacher zu machen, wenn wir uns entschieden, unsere Anwendung in der Zukunft "skinable" zu machen.

Danach ändern Sie die Masterseite müssen Verweise in allen ASPX-Dateien standardmäßig generiert der ASP.NET Web Forms-Seiten.

> [!div class="step-by-step"]
> [Nächste](tailspin-spyworks-part-2.md)
