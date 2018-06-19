---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Teil 7: Mitgliedschaft und Autorisierung | Microsoft Docs'
author: jongalloway
description: Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 7 deckt Mitgliedschaft und Autorisierung.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a0f599da4691c5bb7c8e6f01625fc0e94ce0eac8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879503"
---
<a name="part-7-membership-and-authorization"></a>Teil 7: Mitgliedschaft und Autorisierung
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Das MVC-Music Store ist eine lernprogrammanwendung, die führt und erklärt schrittweise, wie mithilfe von ASP.NET MVC und Visual Studio für die Webentwicklung.  
>   
> Das MVC-Music Store handelt es sich um einfache Beispiel Store Implementierung, die online Musikalben verkauft und implementiert grundlegende standortverwaltung Benutzer anmelden und shopping Cart-Funktionalität.  
>   
> Diese Reihe von Lernprogrammen details aller die Schritte zum Erstellen von ASP.NET MVC-Music Store-beispielanwendung. Teil 7 deckt Mitgliedschaft und Autorisierung.


Unsere Speicher-Manager-Controller ist aktuell für Personen, die Zugriff auf unserer Website zugegriffen werden kann. Ändern Sie diese Option, um die Berechtigungen für Administratoren einschränken.

## <a name="adding-the-accountcontroller-and-views"></a>Hinzufügen von AccountController und Ansichten

Ein Unterschied zwischen der vollständige ASP.NET MVC 3 Web Application-Vorlage und der ASP.NET MVC 3 leere Web Application-Vorlage ist, dass die leere Vorlage eine Konto-Controller enthält. Wir werden eine Konto-Controller hinzufügen, durch Kopieren einiger Dateien aus einer ASP.NET MVC-Anwendung mithilfe der vollständige ASP.NET MVC 3 Web Application-Vorlage erstellt.

Erstellen einer neuen ASP.NET MVC-Anwendung mithilfe der vollständige ASP.NET MVC 3 Web Application-Vorlage, und kopieren Sie die folgenden Dateien in denselben Verzeichnissen im Projekt:

1. Kopieren Sie im Verzeichnis Controller AccountController.cs
2. Kopieren Sie im Verzeichnis Modelle AccountModels
3. Erstellen Sie ein Konto Verzeichnis in das Verzeichnis "Views", und kopieren Sie alle vier Ansichten

Ändern Sie den Namespace für die Klassen für Controller und das Modell aus, damit sie mit MvcMusicStore beginnen. Die AccountController-Klasse sollten den MvcMusicStore.Controllers-Namespace verwenden, und die AccountModels-Klasse sollte den MvcMusicStore.Models-Namespace verwenden.

*Hinweis: Diese Dateien sind auch verfügbar im MvcMusicStore Assets.zip Download aus dem wir unsere Website Design-Dateien am Anfang des Lernprogramms kopiert. Die Mitgliedschaft-Dateien befinden sich im Verzeichnis Code.*

Die aktualisierte Lösung sollte wie folgt aussehen:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Hinzufügen von einem Administrator mit dem Standort der ASP.NET-Konfiguration

Bevor wir in unserer Website Autorisierung erfordern, müssen wir einen Benutzer mit Zugriff erstellen. Die einfachste Möglichkeit zum Erstellen eines Benutzers ist die Verwendung die integrierten ASP.NET-Konfiguration-Website.

Starten Sie durch Klicken auf das Symbol "im Projektmappen-Explorer nach der ASP.NET-Konfiguration-Website.

![](mvc-music-store-part-7/_static/image2.png)

Dadurch wird eine Konfiguration-Website. Klicken Sie auf der Registerkarte "Sicherheit" auf dem Startbildschirm aus, und klicken Sie auf den Link "Rollen aktivieren" in der Mitte des Bildschirms.

![](mvc-music-store-part-7/_static/image3.png)

Klicken Sie auf den Link "Rollen erstellen oder verwalten".

![](mvc-music-store-part-7/_static/image4.png)

Geben Sie den Namen der Rolle "Administrator", und klicken Sie auf die Schaltfläche Rolle hinzufügen.

![](mvc-music-store-part-7/_static/image5.png)

Klicken Sie auf die Schaltfläche "zurück", und klicken Sie auf den Benutzer zum Erstellen eines Links auf der linken Seite.

![](mvc-music-store-part-7/_static/image6.png)

Füllen Sie die Felder für die Benutzerinformationen auf der linken Seite mit den folgenden Informationen ein:

| **Feld** | **Wert** |
| --- | --- |
| **Benutzername** | Administrator |
| **Kennwort** | password123! |
| **Kennwort bestätigen** | password123! |
| **E-Mail** | (eine beliebige e-Mail-Adresse funktioniert) |
| **Sicherheitsfrage** | (beliebig) |
| **Sicherheitsantwort** | (beliebig) |

*Hinweis: Sie können natürlich alle Kennwort verwenden, Sie möchten. Die standardsicherheitseinstellungen für die ein Kennwort erfordern ein Kennwort, 7 Zeichen lang ist und ein nicht alphanumerisches Zeichen enthält.*

Wählen Sie die Rolle "Administrator" für diesen Benutzer aus, und klicken Sie auf die Schaltfläche "Create User".

![](mvc-music-store-part-7/_static/image7.png)

Nun sehen Sie eine Meldung, dass der Benutzer erfolgreich erstellt wurde.

![](mvc-music-store-part-7/_static/image8.png)

Sie können jetzt das Browserfenster schließen.

## <a name="role-based-authorization"></a>Rollenbasierte Autorisierung

Jetzt können wir Beschränken des Zugriffs auf die StoreManagerController mit dem Attribut [Authorize] Gibt an, dass der Benutzer der Rolle Administrator eingreifen Controller in der Klasse den Zugriff auf sein muss.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Hinweis: Das Attribut [Authorize] kann für bestimmte Aktionsmethoden sowie auf Klassenebene Controller platziert werden.*

Jetzt das Navigieren zu /StoreManager wird ein Dialogfeld "Anmelden" geöffnet:

![](mvc-music-store-part-7/_static/image9.png)

Nach der Anmeldung mit unserem neuen Administratorkonto können wir fahren Sie mit dem Album bearbeiten-Bildschirm als vor.

> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-6.md)
> [Weiter](mvc-music-store-part-8.md)
