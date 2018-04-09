---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
title: 'Teil 1: Übersicht und Datei -> Neues Projekt | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 1 erläutert Übersicht und Datei -> Neues Projekt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: bd356ca3-5bdb-4067-9dac-c9e9923a86e8
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-1
msc.type: authoredcontent
ms.openlocfilehash: 2082927d18c95563893da199d60347fa15952446
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-1-overview-and-file-new-project"></a>Teil 1: Übersicht und Datei -> Neues Projekt
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.  
>   
> Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.  
>   
> Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 1 behandelt (Übersicht) "und" Datei -&gt;neues Projekt.


## <a name="overview"></a>Übersicht

Das MVC-Music Store ist eine lernprogrammanwendung, die werden vorgestellt und erläutert schrittweise, wie ASP.NET MVC und Visual Web Developer zur Webentwicklung verwendet. Wir werden langsam gestartet werden, damit Anfänger Ebene Webentwicklung auftreten ist in Ordnung.

Die Anwendung, die wir erstellen werden müssen ist eine einfache-Music Store. Es gibt drei Hauptteilen zusammen, um die Anwendung: Warenkorb, Auschecken und Verwaltung.

![](mvc-music-store-part-1/_static/image1.jpg)

Besucher können Alben "Genre" suchen:

![](mvc-music-store-part-1/_static/image2.jpg)

Sie können einen einzelnen Album anzeigen und Einkaufswagen hinzufügen:

![](mvc-music-store-part-1/_static/image3.jpg)

Sie können überprüfen, dass Einkaufswagen entfernt alle Elemente, die sie ohne mehr benötigen:

![](mvc-music-store-part-1/_static/image4.jpg)

Sie zur Kasse gehen werden aufgefordert, diese anmelden oder Registrieren für ein Benutzerkonto an.

![](mvc-music-store-part-1/_static/image1.png)

![](mvc-music-store-part-1/_static/image2.png)

Nach dem Erstellen eines Kontos, können sie die Reihenfolge abschließen, indem Shipping "und" Payment Informationen ausfüllen. Zur Vereinfachung der Dinge wir eine erstaunliche Promotion ausführen: alles ist kostenlos, wenn sie den Promotioncode "FREE" eingeben.

![](mvc-music-store-part-1/_static/image5.jpg)

Nach dem Sortieren, eine einfache Bestätigungsbildschirm angezeigt:

![](mvc-music-store-part-1/_static/image6.jpg)

Zusätzlich zu den Seiten der Kunden Faceing müssen wir auch erstellen einen Administrator-Abschnitt, der eine Liste von Alben aus denen Administratoren erstellen können, bearbeiten, angezeigt wird und Alben zu löschen:

![](mvc-music-store-part-1/_static/image7.jpg)

## <a name="1-file--gt-new-project"></a>1. Datei -&gt; neues Projekt

### <a name="installing-the-software"></a>Installieren der Clientsoftware

In diesem Lernprogramm beginnt mit dem Erstellen eines neuen ASP.NET MVC 3-Projekts verwenden die kostenlose Visual Web Developer 2010 Express (sodass freigegeben ist), und klicken Sie dann wir fügen inkrementell Funktionen, um eine vollständig funktionierende Anwendung zu erstellen. Nebenbei müssen eingegangen mit Masterseiten für konsistente Seitenlayout, mithilfe von AJAX für Seite-Aktualisierungen und Validierung, Benutzername und vieles mehr Datenbankzugriff, Formular Buchung Szenarien, die datenvalidierung.

Sie Schritt für Schritt nachvollziehen können, oder Sie können aus die fertigen Anwendung herunterladen [MVC-Music-Store](https://github.com/evilDave/MVC-Music-Store).

Sie können Visual Studio 2010 SP1 oder Visual Web Developer 2010 Express SP1 (eine kostenlose Version von Visual Studio 2010) verwenden, zum Erstellen der Anwendung. Wir müssen die SQL Server Compact-(auch kostenlos) verwenden, zum Hosten der Datenbank. Bevor Sie beginnen, stellen Sie sicher, dass Sie die unten aufgeführten erforderlichen Komponenten installiert haben.


- [Visual Studio Web Developer Express SP1-Voraussetzungen]
- [ASP.NET MVC 3 Toolsupdate]
- [SQL Server Compact 4.0] – einschließlich der Unterstützung für Common Language Runtime und Tools


### <a name="creating-a-new-aspnet-mvc-3-project"></a>Erstellen eines neuen ASP.NET MVC 3-Projekts

Wir beginnen, indem Sie in Visual Web Developer im Menü Datei "Neues Projekt" auswählen. Daraufhin wird das Dialogfeld "Neues Projekt".

![](mvc-music-store-part-1/_static/image5.png)

Wir wählen Visual c# -&gt; Webvorlagen Gruppe auf der linken Seite, und wählen Sie dann die Vorlage "ASP.NET MVC 3-Webanwendung" in der mittleren Spalte. Namen für das Projekt MvcMusicStore, und drücken Sie die Schaltfläche "OK".

![](mvc-music-store-part-1/_static/image8.jpg)

Dies wird ein sekundärer Dialogfeld angezeigt, die uns einige MVC-spezifischen Einstellungen für unsere Projekt vornehmen können. Wählen Sie Folgendes:

Projektvorlage: Wählen Sie leer

Anzeigen von Datenbankmodul – wählen Sie Razor

Verwenden Sie HTML5 semantic Markup - überprüft

Stellen Sie sicher, dass Ihre Einstellungen wie unten dargestellt sind, und drücken Sie dann die Schaltfläche "OK".

![](mvc-music-store-part-1/_static/image9.jpg)

Dadurch wird unsere Projekt erstellt. Werfen wir einen Blick auf die Ordner, die die Anwendung im Projektmappen-Explorer auf der rechten Seite hinzugefügt wurden.

![](mvc-music-store-part-1/_static/image10.jpg)

Die leere MVC 3-Vorlage nicht vollständig leer ist – er fügt eine grundlegende Ordnerstruktur:

![](mvc-music-store-part-1/_static/image6.png)

ASP.NET MVC binärencoder einige einfachen Benennungskonventionen für Ordnernamen:

| **Ordner** | **Purpose** |
| --- | --- |
| **/Controllers** | Domänencontroller für die vom Browser Eingabe müssen entscheiden, was Sie darin tun, und Antwort an den Benutzer reagieren. |
| **/Views** | Ansichten halten unsere UI-Vorlagen |
| **/Models** | Modelle enthalten, und Bearbeiten von Daten |
| **/Content** | Dieser Ordner enthält, unsere Bilder, CSS und alle anderen statischen Inhalten |
| **/Scripts** | Dieser Ordner enthält unsere JavaScript-Dateien |

Diese Ordner sind auch in einer leeren ASP.NET MVC-Anwendung enthalten, da das ASP.NET MVC-Framework standardmäßig einen Ansatz "Konvention über Konfiguration verwendet", und einige Annahmen Standardwert basierend auf den Ordnerbenennungskonventionen. Suchen z. B. Domänencontrollern für Ansichten im Ordner "Ansichten" standardmäßig ohne dass Sie dies explizit im Code anzugeben. Verwenden von den Standardkonventionen reduziert die Menge an Code, die Sie schreiben, müssen und Sie können auch einfacher von anderen Entwicklern Projekts verstanden. Wir erläutern die folgenden Konventionen für die weitere, wenn wir unsere Anwendung erstellen.

> [!div class="step-by-step"]
> [Nächste](mvc-music-store-part-2.md)
