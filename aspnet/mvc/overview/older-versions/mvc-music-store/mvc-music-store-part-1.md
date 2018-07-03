---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Teil 1: Übersicht und Datei -> Neues Projekt | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 1 Hintergrund Übersicht und Datei -> Neues Projekt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: c03b62db2227c167c68ca5cf8174e4322658d39d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377888"
---
<a name="part-1-overview-and-file-new-project"></a>Teil 1: Übersicht und Datei -> Neues Projekt
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.  
>   
> Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.  
>   
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 1 sind die Übersicht und Datei -&gt;neues Projekt.


## <a name="overview"></a>Übersicht

Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Web Developer verwenden, für die Webentwicklung. Wir langsam starten, damit für Anfänger auf Web-Entwicklung ist in Ordnung.

Die Anwendung, die wir erstellen, ist eine einfache Musik-Speicher. Es gibt drei Hauptkomponenten der Anwendung: Warenkorb, Auschecken und Verwaltung.

![](mvc-music-store-part-1/_static/image1.jpg)

Besucher können Alben nach Genre dorthin navigieren:

![](mvc-music-store-part-1/_static/image2.jpg)

Sie können ein einzelnes Album anzuzeigen und zu Einkaufswagen hinzufügen:

![](mvc-music-store-part-1/_static/image3.jpg)

Sie können überprüfen, Einkaufswagen, entfernen alle Elemente, die sie nicht mehr benötigen:

![](mvc-music-store-part-1/_static/image4.jpg)

Sie zur Kasse gehen fordert sie zum Anmelden oder registrieren Sie sich für ein Benutzerkonto an.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Nach dem Erstellen eines Kontos können sie die Reihenfolge abschließen, indem Sie die Versand- und Zahlung Informationen ausfüllen. Aus Gründen der Einfachheit, führen wir eine erstaunliche Promotion: alle Komponenten sind kostenlos, wenn sie den Promotioncode "FREE" eingeben.

![](mvc-music-store-part-1/_static/image5.jpg)

Nach dem Sortieren, sehen sie ein einfaches Bestätigungsbildschirm angezeigt:

![](mvc-music-store-part-1/_static/image6.jpg)

Zusätzlich zu den Seiten der Kunden-Faceing wir außerdem erstellen eine Administrator-Bereich, der eine Liste der Alben aus denen Administratoren erstellen können, bearbeiten, angezeigt und Alben zu löschen:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. Datei -&gt; neues Projekt

### <a name="installing-the-software"></a>Installieren der Clientsoftware

Durch Erstellen eines neuen ASP.NET MVC 3-Projekts verwenden die kostenlose Visual Web Developer 2010 Express (auf dem frei ist) wird mit diesem Tutorial beginnen, und dann werden wir schrittweise Funktionen zum Erstellen einer vollständigen funktionsfähigen Anwendung hinzufügen. Dabei wird die behandelt mit Masterseiten für konsistente Seitenlayout, mithilfe von AJAX für Seitenupdates und Validierung und Anmeldung des Benutzers Zugriff auf die Datenbank, die Form senden Szenarien, die datenvalidierung.

Sie Schritt für Schritt nachvollziehen können, oder Sie können die fertige Anwendung herunterladen [MVC-Musik-Store](https://github.com/evilDave/MVC-Music-Store).

Sie können entweder Visual Studio 2010 SP1 oder Visual Web Developer 2010 Express SP1 (eine kostenlose Version von Visual Studio 2010) verwenden, um die Anwendung zu erstellen. Wir werden die SQL Server Compact-(ebenfalls kostenlos) verwenden, um die Datenbank hostet. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben.


- [Visual Studio Web Developer Express SP1-Voraussetzungen]
- [ASP.NET MVC 3 Toolsupdate]
- [SQL Server Compact 4.0] – einschließlich Unterstützung für sowohl-Laufzeit und Tools


### <a name="creating-a-new-aspnet-mvc-3-project"></a>Erstellen eines neuen ASP.NET MVC 3-Projekts

Wir beginnen, indem Sie "Neues Projekt" auswählen, über das Menü "Datei" in Visual Web Developer. Daraufhin wird das Dialogfeld "Neues Projekt".

![](mvc-music-store-part-1/_static/image5.png)

Ich wähle Visual c# -&gt; Webvorlagen Gruppe auf der linken Seite, und wählen Sie dann die Vorlage "ASP.NET MVC 3-Webanwendung" in der mittleren Spalte. Benennen Sie Ihr Projekt MvcMusicStore aus, und drücken Sie die Schaltfläche "OK".

![](mvc-music-store-part-1/_static/image8.jpg)

Dadurch wird ein sekundärer Dialogfeld angezeigt, diese Bezeichnung uns ermöglicht, um eine MVC-spezifischen Einstellungen für das Projekt zu machen. Wählen Sie Folgendes:

Projektvorlage: Wählen Sie die leere

Engine anzeigen: Wählen Sie Razor

Verwenden von semantischen HTML5-Markupcode - aktiviert

Stellen Sie sicher, dass Ihre Einstellungen werden wie unten dargestellt, und drücken Sie dann die Schaltfläche "OK".

![](mvc-music-store-part-1/_static/image9.jpg)

Dadurch wird das Projekt erstellt. Werfen wir einen Blick auf den Ordner, in denen die Anwendung im Projektmappen-Explorer auf der rechten Seite hinzugefügt wurden.

![](mvc-music-store-part-1/_static/image10.jpg)

Die leeren MVC 3-Vorlage nicht vollständig leer ist – eine einfache Ordnerstruktur hinzugefügt:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC verwendet einige einfachen Benennungskonventionen für Ordnernamen:

| **Ordner** | **Zweck** |
| --- | --- |
| **/ Controller** | Domänencontroller reagieren, geben Sie im Browser entscheiden, was es, und die Antwort an den Benutzer zurück. |
| **/ Views** | Sichten enthalten unsere UI-Vorlagen |
| **/ Modelle** | Modelle enthalten, und Bearbeiten von Daten |
| **/ Inhalte** | Dieser Ordner enthält unsere Images, CSS und alle anderen statischen Inhalten |
| **/Scripts** | Dieser Ordner enthält unsere JavaScript-Dateien |

Diese Ordner werden auch in einer leeren ASP.NET MVC-Anwendung enthalten, da ASP.NET MVC-Framework standardmäßig einen Ansatz "Konvention geht vor Konfiguration verwendet", und einige Annahmen standardmäßig basierend auf den Ordner Benennungskonventionen zur Verfügung. Suchen z. B. Controller für Ansichten im Ordner "Views" standardmäßig ohne dass Sie dies in Ihrem Code explizit anzugeben. Mit den Standardkonventionen festhalten reduziert die Menge an Code zu schreiben, Sie müssen und kann auch problemlos für andere Entwickler zu Ihrem Projekt. Wir erläutern diesen Konventionen Weitere, wie wir unsere Anwendung zu erstellen.

> [!div class="step-by-step"]
> [Nächste](mvc-music-store-part-2.md)
