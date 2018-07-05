---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Aufrufen von Web-API aus einer Windows Phone 8-Anwendung (c#) | Microsoft-Dokumentation
author: rmcmurray
description: Erstellen Sie eine vollständige End-to-End-Szenario bestehend aus einer ASP.NET Web-API-Anwendung, die einen Katalog mit Büchern, die eine Windows Phone 8-Anwendung bereitstellt.
ms.author: aspnetcontent
ms.date: 10/09/2013
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 6b7a833818424cbf3a3bf9e1e14e5b2864742c38
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805041"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Aufrufen von Web-API aus einer Windows Phone 8-Anwendung (c#)
====================
durch [Robert McMurray](https://github.com/rmcmurray)

In diesem Tutorial erfahren Sie, wie erstellen Sie eine vollständige End-to-End-Szenario bestehend aus einer ASP.NET Web-API-Anwendung, die einen Katalog mit Büchern, die eine Windows Phone 8-Anwendung bereitstellt.

### <a name="overview"></a>Übersicht

RESTful-Dienste wie ASP.NET-Web-API, vereinfachen die Erstellung von HTTP-basierte Anwendungen für Entwickler durch das abstrahieren der Architektur für die serverseitige und clientseitige Anwendungen. Statt ein proprietäres Protokoll der Socket-basierte für die Kommunikation, Web-API-Entwickler müssen einfach die erforderlichen HTTP-Methoden für die entsprechende Anwendung zu veröffentlichen (zum Beispiel: GET, POST, PUT, DELETE), und Entwicklern von Clientanwendungen ausschließlich verarbeiten müssen die HTTP-Methoden, die für ihre Anwendung erforderlich sind.

In diesem End-to-End-Tutorial lernen Sie, wie Sie Web-API verwenden, um die folgenden Projekte zu erstellen:

- In der [ersten Teil dieses Lernprogramms](#STEP1), erstellen Sie eine ASP.NET Web-API-Anwendung, alle Vorgänge zum Verwalten einer Bücherkatalog Create, Read, Update und Delete (CRUD) unterstützt. Diese Anwendung verwendet die [Beispiel-XML-Datei (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) auf MSDN.
- In der [zweiter Teil dieses Lernprogramms](#STEP2), erstellen Sie eine interaktive Windows Phone 8-Anwendung, die Daten aus Ihrer Web-API-Anwendung abruft.

#### <a name="prerequisites"></a>Erforderliche Komponenten

- Visual Studio 2013 mit dem Windows Phone 8 SDK
- Windows 8 oder höher auf einem 64-Bit-System mit Hyper-V installiert
- Eine Liste der zusätzlichen Anforderungen, finden Sie unter den *Systemanforderungen* im Abschnitt der [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) Downloadseite.

> [!NOTE]
> Wenn Sie beabsichtigen, um die Konnektivität zwischen Web-API und Windows Phone 8-Projekte auf dem lokalen System zu testen, müssen Sie die Anweisungen in der *[Web-API-Anwendungen auf einem lokalen mit der Windows Phone 8-Emulator Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* Artikel zum Einrichten Ihrer testumgebung.


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Schritt 1: Erstellen der Web-API angegeben, dass-Projekt

Der erste Schritt in diesem End-to-End-Tutorial ist ein Web-API-Projekt zu erstellen, die alle CRUD-Vorgänge unterstützt. Beachten Sie, dass Sie diese Lösung in das Windows Phone-Anwendungsprojekt hinzufügen [Schritt2](#STEP2) in diesem Tutorial.

1. Open **Visual Studio 2013**.
2. Klicken Sie auf **Datei**, klicken Sie dann **neue**, und klicken Sie dann **Projekt**.
3. Wenn die **neues Projekt** Dialogfeld wird angezeigt, erweitern Sie **installiert**, klicken Sie dann **Vorlagen**, klicken Sie dann **Visual C#-**, und klicken Sie dann **Web**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Klicken Sie auf Bild zu erweitern                                                                |


4. Markieren Sie **ASP.NET-Webanwendung**, geben Sie **BookStore** für den Projektnamen ein, und klicken Sie dann auf **OK**.
5. Wenn die **neues ASP.NET-Projekt** Dialogfeld angezeigt wird, wählen die **Web-API-** Vorlage, und klicken Sie dann auf **OK**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Klicken Sie auf Bild zu erweitern                                                                |


6. Wenn die Web-API-Projekt geöffnet wird, wird entfernen Sie den Beispielcontroller aus dem Projekt:

    1. Erweitern Sie die **Controller** Ordner im Projektmappen-Explorer.
    2. Mit der rechten Maustaste die **ValuesController.cs** Datei, und klicken Sie dann auf **löschen**.
    3. Klicken Sie auf **OK** Wenn aufgefordert, den Löschvorgang zu bestätigen.
7. Hinzufügen einer XML-Datendatei für das Web-API-Projekt; Diese Datei enthält den Inhalt des Katalogs Bookstore:

   1. Mit der rechten Maustaste die **App\_Daten** Ordner im Projektmappen-Explorer, klicken Sie dann auf **hinzufügen**, und klicken Sie dann auf **neues Element**.
   2. Wenn die **neues Element hinzufügen** Dialogfeld wird angezeigt, markieren Sie die **XML-Datei** Vorlage.
   3. Nennen Sie die Datei **Books.xml**, und klicken Sie dann auf **hinzufügen**.
   4. Wenn die **Books.xml** Datei geöffnet wird, ersetzen Sie den Code in der Datei mit den XML-Code aus dem Beispiel **books.xml** -Datei auf der MSDN: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. Speichern Sie und schließen Sie die XML-Datei.

8. Der Bookstore-Modell für das Web-API-Projekt hinzufügen; Dieses Modell enthält die Logik für Create, Read, Update und Delete (CRUD) für die Anwendung angegeben, dass:

   1. Mit der rechten Maustaste die **Modelle** Ordner im Projektmappen-Explorer, klicken Sie dann auf **hinzufügen**, und klicken Sie dann auf **Klasse**.
   2. Wenn die **neues Element hinzufügen** Dialogfeld wird angezeigt, benennen Sie die Klassendatei **BookDetails.cs**, und klicken Sie dann auf **hinzufügen**.
   3. Wenn die **BookDetails.cs** Datei geöffnet wird, ersetzen Sie den Code in der Datei durch Folgendes: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Speichern und schließen Sie die **BookDetails.cs** Datei.

9. Fügen Sie der Bookstore-Controller für das Web-API-Projekt hinzu:

   1. Mit der rechten Maustaste die **Controller** Ordner im Projektmappen-Explorer, klicken Sie dann auf **hinzufügen**, und klicken Sie dann auf **Controller**.
   2. Wenn die **Gerüst hinzufügen** Dialogfeld wird angezeigt, markieren Sie **Web API 2-Controller – leer**, und klicken Sie dann auf **hinzufügen**.
   3. Wenn die **Controller hinzufügen** Dialogfeld wird angezeigt, nennen Sie den Controller **BooksController**, und klicken Sie dann auf **hinzufügen**.
   4. Wenn die **BooksController.cs** Datei geöffnet wird, ersetzen Sie den Code in der Datei durch Folgendes: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Speichern und schließen Sie die **BooksController.cs** Datei.

10. Erstellen Sie die Web-API-Anwendung auf Fehler überprüfen.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Schritt 2: Hinzufügen des Windows Phone 8 Bookstore-Katalog-Projekts

Der nächste Schritt dieses End-to-End-Szenario ist die Erstellung die kataloganwendung für Windows Phone 8. Diese Anwendung verwendet die *Windows Phone Databound App* Vorlage für die Standardbenutzeroberfläche und verwendet die Web-API-Anwendung, die Sie in erstellt [Schritt 1](#STEP1) in diesem Tutorial als Datenquelle.

1. Mit der rechten Maustaste die **BookStore** -Lösung in die im Projektmappen-Explorer, klicken Sie dann auf **hinzufügen**, und klicken Sie dann **neues Projekt**.
2. Wenn die **neues Projekt** Dialogfeld wird angezeigt, erweitern Sie **installiert**, klicken Sie dann **Visual C#-**, und klicken Sie dann **Windows Phone**.
3. Markieren Sie **Windows Phone Databound App**, geben Sie **BookCatalog** für den Namen ein, und klicken Sie dann auf **OK**.
4. Das Json.NET NuGet-Paket zum Hinzufügen der **BookCatalog** Projekt:

    1. Mit der rechten Maustaste **Verweise** für die **BookCatalog** Projekt im Projektmappen-Explorer, und klicken Sie dann auf **NuGet-Pakete verwalten**.
    2. Wenn die **NuGet-Pakete verwalten** Dialogfeld wird angezeigt, erweitern Sie die **Online** aus, und markieren Sie **nuget.org**.
    3. Geben Sie **Json.NET** in die Suche ein, und klicken Sie auf das Symbol "Suche".
    4. Markieren Sie **Json.NET** in den Suchergebnissen, und klicken Sie dann auf **installieren**.
    5. Wenn die Installation abgeschlossen ist, klicken Sie auf **schließen**.
5. Hinzufügen der **BookDetails** Modell der **BookCatalog** Projekteigenschaften; enthält ein generisches Modell der Bookstore-Klasse:

   1. Mit der rechten Maustaste die **BookCatalog** Projekt im Projektmappen-Explorer, und klicken Sie dann **hinzufügen**, und klicken Sie dann auf **neuer Ordner**.
   2. Nennen Sie diesen Ordner **Modelle**.
   3. Mit der rechten Maustaste die **Modelle** Ordner im Projektmappen-Explorer, klicken Sie dann auf **hinzufügen**, und klicken Sie dann auf **Klasse**.
   4. Wenn die **neues Element hinzufügen** Dialogfeld wird angezeigt, benennen Sie die Klassendatei **BookDetails.cs**, und klicken Sie dann auf **hinzufügen**.
   5. Wenn die **BookDetails.cs** Datei geöffnet wird, ersetzen Sie den Code in der Datei durch Folgendes: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Speichern und schließen Sie die **BookDetails.cs** Datei.

6. Update der **"MainViewModel.cs"** Klasse einbeziehen, die Funktionalität für die Kommunikation mit der BookStore-Web-API-Anwendung:

   1. Erweitern Sie die **ViewModels** Ordner im Projektmappen-Explorer, und doppelklicken Sie dann auf die **"MainViewModel.cs"** Datei.
   2. Wenn die **"MainViewModel.cs"** Datei wird geöffnet, ersetzen Sie den Code in der Datei mit den folgenden; Beachten Sie, dass Sie für das update benötigen die `apiUrl` Konstante mit der richtigen URL Ihrer Web-API: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Speichern und schließen Sie die **"MainViewModel.cs"** Datei.

7. Update der **"MainPage.xaml"** Datei den Namen der Anwendung anpassen:

   1. Doppelklicken Sie auf die **"MainPage.xaml"** Datei im Projektmappen-Explorer.
   2. Wenn die **"MainPage.xaml"** Datei geöffnet wird, suchen Sie nach den folgenden Code: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Ersetzen Sie diese Zeilen durch Folgendes: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Speichern und schließen Sie die **"MainPage.xaml"** Datei.

8. Update der **DetailsPage.xaml** Datei, um die angezeigten Elemente anzupassen:

   1. Doppelklicken Sie auf die **DetailsPage.xaml** Datei im Projektmappen-Explorer.
   2. Wenn die **DetailsPage.xaml** Datei geöffnet wird, suchen Sie nach den folgenden Code: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Ersetzen Sie diese Zeilen durch Folgendes: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Speichern und schließen Sie die **DetailsPage.xaml** Datei.

9. Erstellen Sie die Windows Phone-Anwendung auf Fehler überprüfen.

### <a name="step-3-testing-the-end-to-end-solution"></a>Schritt 3: Testen der End-to-End-Lösung

Siehe die *Voraussetzungen* Abschnitt dieses Tutorials, die beim Testen der Konnektivität zwischen Web-API und Windows Phone 8-Projekte auf dem lokalen System, müssen Sie die Anleitung in der *[ Web-API-Anwendungen auf einem lokalen Computer mit Windows Phone 8 Emulator](https://go.microsoft.com/fwlink/?LinkId=324014)* Artikel zum Einrichten Ihrer testumgebung.

Nachdem Sie die testumgebung konfiguriert haben, müssen Sie die Windows Phone-Anwendung als Startprojekt festlegen. Markieren Sie hierzu die **BookCatalog** im Projektmappen-Explorer, und klicken Sie dann auf Anwendung **als Startprojekt festlegen**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Klicken Sie auf Bild zu erweitern |

Wenn Sie F5 drücken, startet Visual Studio sowohl die Windows Phone-Emulator, der angezeigt wird eine &quot;warten&quot; Nachricht, während die Anwendungsdaten aus Ihrer Web-API abgerufen werden:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Klicken Sie auf Bild zu erweitern |

Wenn alles erfolgreich ist, sehen Sie den Katalog angezeigt:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Klicken Sie auf Bild zu erweitern |

Wenn Sie sich auf alle Buchtitel tippen, wird die Anwendung die Buch-Beschreibung angezeigt:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Klicken Sie auf Bild zu erweitern |

Wenn die Anwendung nicht mit Ihrer Web-API kommunizieren kann, wird eine Fehlermeldung angezeigt:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Klicken Sie auf Bild zu erweitern |

Wenn Sie auf die Fehlermeldung tippen, werden zusätzliche Details zum Fehler angezeigt:


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Klicken Sie auf Bild zu erweitern                                                                 |

