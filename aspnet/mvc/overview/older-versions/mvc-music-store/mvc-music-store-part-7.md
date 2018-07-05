---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Teil 7: Mitgliedschaft und Autorisierung | Microsoft-Dokumentation'
author: jongalloway
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 7 behandelt die Mitgliedschaft und Autorisierung.
ms.author: aspnetcontent
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 1f2ad9de3a21366931efe6ca2466f4efc23a6192
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802538"
---
<a name="part-7-membership-and-authorization"></a>Teil 7: Mitgliedschaft und Autorisierung
====================
durch [Jon Galloway](https://github.com/jongalloway)

> Die MVC Music Store ist ein lernprogrammanwendung, die eingeführt und erläutert Schritt für Schritt, wie ASP.NET MVC und Visual Studio für die Webentwicklung verwenden.  
>   
> Die MVC Music Store ist eine Implementierung eines einfachen Beispiels die Alben online verkauft und implementiert grundlegende Verwaltung, Benutzeranmeldung und shopping Cart-Funktionalität.  
>   
> Dieser tutorialreihe werden alle Schritte ausgeführt, um die ASP.NET MVC Music Store-beispielanwendung zu erstellen. Teil 7 behandelt die Mitgliedschaft und Autorisierung.


Unsere Store Manager-Controller ist derzeit für alle Benutzer Zugriff auf unserer Website zugegriffen werden kann. Wir ändern Sie diese Option, um die Berechtigungen für Administratoren einschränken.

## <a name="adding-the-accountcontroller-and-views"></a>Hinzufügen der AccountController und Ansichten

Ein Unterschied zwischen der vollständigen Vorlage für ASP.NET MVC 3-Webanwendung und die Vorlage für ASP.NET MVC 3 leere Webanwendung ist, dass die leere Vorlage eine Konto-Controller nicht enthält. Wir fügen eine Konto-Controller durch Kopieren einige Dateien eine neue ASP.NET MVC-Anwendung, die aus der vollständigen Vorlage für ASP.NET MVC 3-Webanwendung erstellt.

Erstellen einer neuen ASP.NET MVC-Anwendung mithilfe der vollständigen Vorlage für ASP.NET MVC 3-Webanwendung, und kopieren Sie die folgenden Dateien in denselben Verzeichnissen in Ihrem Projekt:

1. Kopieren Sie im Verzeichnis Controller AccountController.cs
2. Kopieren Sie im Verzeichnis Modelle AccountModels
3. Erstellen Sie ein Verzeichnis in das Verzeichnis "Views", und kopieren Sie alle vier Ansichten in

Ändern Sie den Namespace für die Controller- und Model-Klassen, damit sie mit MvcMusicStore beginnen. Die AccountController-Klasse sollten den MvcMusicStore.Controllers-Namespace verwenden, und die AccountModels-Klasse sollte den MvcMusicStore.Models-Namespace verwenden.

*Hinweis: Diese Dateien sind auch im MvcMusicStore-Assets.zip Download aus dem wir unsere Website Design-Dateien am Anfang des Tutorials kopiert haben. Die Mitgliedschaft-Dateien befinden sich im Verzeichnis "Code".*

Die aktualisierte Projektmappe sollte wie folgt aussehen:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Hinzufügen von einem Administrator mit dem Standort der ASP.NET-Konfiguration

Bevor wir auf unserer Website Autorisierung erforderlich ist, müssen wir einen Benutzer mit Zugriff zu erstellen. Die einfachste Möglichkeit zum Erstellen eines Benutzers ist die Verwendung die integrierten ASP.NET-Konfiguration-Website.

Starten Sie die ASP.NET-Konfiguration-Website, indem Sie auf das Symbol im Projektmappen-Explorer nach.

![](mvc-music-store-part-7/_static/image2.png)

Dadurch wird eine Konfigurationswebsite. Klicken Sie auf der Registerkarte "Sicherheit" auf dem Startbildschirm ein, und klicken Sie auf den Link "Rollen aktivieren" in der Mitte des Bildschirms.

![](mvc-music-store-part-7/_static/image3.png)

Klicken Sie auf den Link "Rollen erstellen oder verwalten".

![](mvc-music-store-part-7/_static/image4.png)

Geben Sie den Rollennamen "Administrator", und klicken Sie auf die Schaltfläche Rolle hinzufügen.

![](mvc-music-store-part-7/_static/image5.png)

Klicken Sie auf die Schaltfläche "zurück", und klicken Sie dann auf den Link "Benutzer erstellen" auf der linken Seite.

![](mvc-music-store-part-7/_static/image6.png)

Geben Sie die Felder für die Benutzerinformationen auf der linken Seite mit den folgenden Informationen:

| **Feld** | **Wert** |
| --- | --- |
| **Benutzername** | Administrator |
| **Kennwort** | password123! |
| **Kennwort bestätigen** | password123! |
| **E-Mail** | (eine beliebige e-Mail-Adresse funktioniert) |
| **Sicherheitsfrage** | (beliebig) |
| **Sicherheitsantwort** | (beliebig) |

*Hinweis: Sie können natürlich alle Kennwort verwenden, die Sie möchten. Die standardsicherheitseinstellungen für das Kennwort müssen es sich um ein Kennwort, das 7 Zeichen lang und enthält ein nicht alphanumerisches Zeichen.*

Wählen Sie die Rolle "Administrator" für diesen Benutzer aus, und klicken Sie auf die Schaltfläche "Benutzer erstellen".

![](mvc-music-store-part-7/_static/image7.png)

An diesem Punkt sehen Sie eine Meldung angezeigt, dass der Benutzer erfolgreich erstellt wurde.

![](mvc-music-store-part-7/_static/image8.png)

Sie können das Browserfenster jetzt schließen.

## <a name="role-based-authorization"></a>Rollenbasierte Autorisierung

Jetzt können wir Beschränken des Zugriffs auf die StoreManagerController mit dem [Authorize]-Attribut gibt an, dass der Benutzer muss in jeder beliebigen Controlleraktion in der Klasse den Zugriff auf die Rolle "Administrator" sein.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Hinweis: Die [Authorize]-Attribut kann sowohl auf bestimmte Methoden als auch auf Klassenebene Controller platziert werden.*

Nun Navigieren zu /StoreManager wird ein Dialogfeld Anmelden:

![](mvc-music-store-part-7/_static/image9.png)

Nach der Anmeldung mit unserem neuen Administrator-Konto können wir auf dem Bildschirm Album bearbeiten als vor dem wechseln.

> [!div class="step-by-step"]
> [Zurück](mvc-music-store-part-6.md)
> [Weiter](mvc-music-store-part-8.md)
