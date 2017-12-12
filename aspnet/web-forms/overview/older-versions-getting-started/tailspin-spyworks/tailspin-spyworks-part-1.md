---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
title: "Teil 1: Neues Projekt für Datei -> | Microsoft Docs"
author: JoeStagner
description: "Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 1 werden (Übersicht) \"und\" Datei/neu Projekt behandelt."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 15d4652b-d5aa-4172-b186-2c7f96ba316d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-1
msc.type: authoredcontent
ms.openlocfilehash: bd840f9f3f5d723e6bc1bb35955a7770634e9483
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="part-1-file--new-project"></a>Teil 1: Datei -> Neues Projekt
====================
durch [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen. Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.
> 
> Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 1 werden (Übersicht) "und" Datei/neu Projekt behandelt.


## <a id="_Toc260221666"></a>Übersicht über

Dieses Lernprogramm ist eine Einführung in ASP.NET Web Forms. Wir werden langsam gestartet werden, damit Anfänger Ebene Webentwicklung auftreten ist in Ordnung.

Die Anwendung, die wir erstellen werden müssen ist eine einfache Online Store.

![](tailspin-spyworks-part-1/_static/image1.jpg)


Besucher können nach Produkten nach Kategorie zu suchen:

![](tailspin-spyworks-part-1/_static/image2.jpg)

Sie können ein einzelnes Produkt anzuzeigen und Einkaufswagen hinzufügen:

![](tailspin-spyworks-part-1/_static/image3.jpg)

Sie können überprüfen, dass Einkaufswagen entfernt alle Elemente, die sie ohne mehr benötigen:

![](tailspin-spyworks-part-1/_static/image4.jpg)

Sie zur Kasse gehen wird zur Eingabe aufgefordert

![](tailspin-spyworks-part-1/_static/image5.jpg)

![](tailspin-spyworks-part-1/_static/image6.jpg)

Nach dem Sortieren, eine einfache Bestätigungsbildschirm angezeigt:

![](tailspin-spyworks-part-1/_static/image7.jpg)


Wir beginnen, indem Sie ein neues ASP.NET Web Forms-Projekt in Visual Studio 2010 erstellen, und wir werden die Funktionen, um eine vollständig funktionierende Anwendung erstellen inkrementell hinzufügen. Nebenbei müssen eingegangen mit Masterseiten für konsistente Seitenlayout, AJAX, Validierung, die Benutzermitgliedschaft und Datenbankzugriff, Listen und Raster, Update von Datenseiten, Validierung.

Sie Schritt für Schritt nachvollziehen können, oder Sie können aus die fertigen Anwendung herunterladen [http://tailspinspyworks.codeplex.com/](http://tailspinspyworks.codeplex.com/)

Sie können Visual Studio 2010 oder das kostenlose Visual Web Developer 2010 aus [https://www.microsoft.com/express/Web/](https://www.microsoft.com/express/Web/). Um die Anwendung zu erstellen, können Sie entweder SQL Server oder den freien SQL Server Express auf Host der Datenbank.

## <a id="_Toc260221667"></a>Datei / neues Projekt

Wir beginnen, indem Sie das neue Projekt über das Menü Datei in Visual Studio auswählen. Daraufhin wird das Dialogfeld "Neues Projekt".

![](tailspin-spyworks-part-1/_static/image8.jpg)

Wir wählen die Visual C#- / Webvorlagen Gruppe auf der linken Seite, und wählen Sie dann die Vorlage "ASP.NET Web-Anwendung" in der mittleren Spalte. Namen für das Projekt TailspinSpyworks, und drücken Sie die Schaltfläche "OK".

![](tailspin-spyworks-part-1/_static/image9.jpg)

Dadurch wird unsere Projekt erstellt. Werfen wir einen Blick auf die Ordner, die in der vorliegenden Anwendung im Projektmappen-Explorer auf der rechten Seite enthalten sind.

![](tailspin-spyworks-part-1/_static/image10.jpg)

Leere Projektmappe nicht vollständig leer ist – er fügt eine grundlegende Ordnerstruktur:

![](tailspin-spyworks-part-1/_static/image1.png)

Beachten Sie die Konventionen der Projektvorlage für ASP.NET 4-Standard implementiert.

- Der Ordner "Konto" implementiert eine einfache Benutzeroberfläche für das ASP. NET Mitgliedschaft Subsystem.
- Der Ordner "Skripts" dient als Repository für Client-Side-JavaScript-Dateien und die Core jQuery JS-Dateien werden standardmäßig zur Verfügung gestellt.
- Der Ordner "Formatvorlagen" wird verwendet, um unsere Website Visualisierungen (Cascading Stylesheets) zu organisieren.

Wenn wir drücken Sie F5, um die Anwendung ausführen und Rendern der Seite "default.aspx" wird Folgendes angezeigt.

![](tailspin-spyworks-part-1/_static/image11.jpg)

Unsere erste Anwendung Erweiterung werden, ersetzen Sie die Datei "Style.css" aus der WebForms-Standardvorlage durch die CSS-Klassen und zugehörige Abbild-Dateien, die visual Asthetics gerendert werden, die wir für unsere Tailspin Spyworks-Anwendung ein.

Nach der Anmeldung rendert unsere Seite "default.aspx" wie folgt aus.

![](tailspin-spyworks-part-1/_static/image12.jpg)

Beachten Sie das Bildhyperlinks oben rechts auf der Seite und die Menüelemente, die der Masterseite hinzugefügt wurden. Nur die Links "Anmelden" und "Konto" zeigen Sie auf Seiten, die vorhanden sind (von der Standardvorlage generiert) und den Rest der Seiten, die wir implementieren, wenn wir unsere Anwendung erstellen.

Wir werden auch die Gestaltungsvorlage in das Verzeichnis Stile zu verschieben. Obwohl dies nur eine Einstellung ist kann es etwas Vereinfachung Wenn wir unsere Anwendung in der Zukunft "skinable" machen möchten.

Nach der Durchführung dieses wir ändern die Gestaltungsvorlage müssen Verweise in der ASPX-Dateien wird standardmäßig generiert, die ASP.NET Web Forms-Seiten.

>[!div class="step-by-step"]
[Nächste](tailspin-spyworks-part-2.md)
