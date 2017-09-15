---
title: "Veröffentlichen einer ASP.NET Core-App in Azure mit Visual Studio"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: c4df5f4551760fbc4ed4b11362d249b24f186e00
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>Veröffentlichen einer ASP.NET Core-Web-App in Azure App Service mit Visual Studio

Von [Rick Anderson](https://twitter.com/RickAndMSFT) und [Cesar Blum Silveira](https://github.com/cesarbs)

## <a name="set-up-the-development-environment"></a>Einrichten der Entwicklungsumgebung

* Installieren Sie das neueste [Azure SDK für Visual Studio](https://www.visualstudio.com/features/azure-tools-vs). Das SDK installiert Visual Studio, falls noch nicht vorhanden.

> [!NOTE]
> Die SDK-Installation kann mehr als 30 Minuten dauern, wenn Ihr Computer nur wenige der Abhängigkeiten aufweist.

* Installieren Sie [.NET Core und die Visual Studio-Tools](http://go.microsoft.com/fwlink/?LinkID=798306).

* Überprüfen Sie Ihr [Azure-Konto](https://portal.azure.com/). Sie können [ein kostenloses Azure-Konto erhalten](https://azure.microsoft.com/pricing/free-trial/) oder [Leistungen für Visual Studio-Abonnenten aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

## <a name="create-a-web-app"></a>Erstellen einer Web-App

Tippen Sie auf der Visual Studio-Startseite, auf **Neues Projekt...**.

![Startseite](publish-to-azure-webapp-using-vs/_static/new_project.png)

Alternativ können Sie in den Menüs ein neues Projekt erstellen. Tippen Sie auf **Datei > Neu > Projekt...**.

![Menü „Datei“](publish-to-azure-webapp-using-vs/_static/alt_new_project.png)

Vervollständigen Sie das Dialogfeld **Neues Projekt**:

* Tippen Sie im linken Bereich auf **Web**.

* Tippen Sie im mittleren Bereich auf **ASP.NET Core-Webanwendung (.NET Core)**.

* Tippen Sie auf **OK**.

![Dialogfeld „Neues Projekt“](publish-to-azure-webapp-using-vs/_static/new_prj.png)

Gehen Sie im Dialogfeld **ASP.NET Core-Webanwendung (.NET Core)** so vor:

* Tippen Sie auf **Webanwendung**.

* Vergewissern Sie sich, dass **Authentifizierung** auf **Einzelne Benutzerkonten** festgelegt ist.

* Vergewissern Sie sich, dass **Host in der Cloud** **nicht** aktiviert ist.

* Tippen Sie auf **OK**.

![Dialogfeld „Neue ASP.NET Core-Webanwendung (.NET Core)“](publish-to-azure-webapp-using-vs/_static/noath.png)

## <a name="test-the-app-locally"></a>Lokales Testen der App

* Drücken Sie **STRG+F5**, um die App lokal auszuführen.

* Tippen Sie auf die Links **About** und **Contact**. Je nach Größe Ihres Geräts müssen Sie möglicherweise auf das Navigationssymbol klicken, um die Links anzuzeigen.

![Auf „localhost“ in Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/show.png)

* Tippen Sie auf **Registrieren**, und registrieren Sie einen neuen Benutzer. Sie können eine fiktive E-Mail-Adresse verwenden. Sobald Sie diese übermitteln, erhalten Sie die folgende Fehlermeldung:

![Interner Serverfehler: Fehler bei einem Datenbankvorgang beim Verarbeiten der Anforderung. SQL-Ausnahme: Die Datenbank kann nicht geöffnet werden. Das Anwenden vorhandener Migrationen für den Datenbankkontext der Anwendung kann dieses Problem beheben.](publish-to-azure-webapp-using-vs/_static/mig.png)

Sie können das Problem auf zwei Arten beheben:

* Tippen Sie auf **Migrationen anwenden**, und aktualisieren Sie die Seite. Oder

* Führen Sie an einer Eingabeaufforderung im Verzeichnis des Projekts den folgenden Befehl aus:

  <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

  ```
  dotnet ef database update
     ```

Die App zeigt die zum Registrieren des neuen Benutzers verwendete E-Mail-Adresse und den Link **Log off**.

![In Microsoft Edge geöffnete Webanwendung Der Link „Register“ wird durch den Text „Hello abc@example.com!“ ersetzt.](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>Bereitstellen der App in Azure

Stellen Sie sicher, dass die zur Bereitstellung veröffentlichte App nicht ausgeführt wird. Die Dateien im Ordner *publish* sind gesperrt, wenn die App ausgeführt wird. Die Bereitstellung kann nicht erfolgen, da die gesperrten Dateien nicht kopiert werden können.

Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf das Projekt, und wählen Sie **Veröffentlichen...** aus.

![Geöffnetes Kontextmenü mit hervorgehobenem Link „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/pub.png)

Tippen Sie im Dialogfeld **Veröffentlichen** auf **Microsoft Azure App Service**.

![Dialogfeld „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/maas1.png)

Tippen Sie auf **Neu...**, um eine neue Ressourcengruppe zu erstellen. Das Erstellen einer neuen Ressourcengruppe vereinfacht das Löschen aller Azure-Ressourcen, die Sie in diesem Tutorial erstellen.

![Dialogfeld „App Service“](publish-to-azure-webapp-using-vs/_static/newrg1.png)

Erstellen Sie eine neue Ressourcengruppe und einen App Service-Plan:

* Tippen Sie für die Ressourcengruppe auf **Neu...**, und geben Sie einen Namen für die neue Ressourcengruppe ein.

* Tippen Sie für den App Service-Plan auf **Neu...**, und wählen Sie einen Standort in Ihrer Nähe aus. Sie können den generierten Standardnamen beibehalten.

* Tippen Sie auf **Weitere Azure Services durchsuchen**, um eine neue Datenbank zu erstellen.

![Dialogfeld „Neue Ressourcengruppe“: Bereich „Hosting“](publish-to-azure-webapp-using-vs/_static/cas.png)

* Tippen Sie auf das grüne Symbol **+**, um eine neue SQL-Datenbank zu erstellen.

![Dialogfeld „Neue Ressourcengruppe“: Bereich „Dienste“](publish-to-azure-webapp-using-vs/_static/sql.png)

* Tippen Sie im Dialogfeld **SQL-Datenbank konfigurieren** auf **Neu...**, um einen neuen Datenbankserver zu erstellen.

![Dialogfeld „SQL-Datenbank konfigurieren“](publish-to-azure-webapp-using-vs/_static/conf.png)

* Geben Sie einen Administratorbenutzernamen und ein Kennwort ein, und tippen Sie dann auf **OK**. Merken Sie sich den Benutzernamen und das Kennwort, den/das Sie in diesem Schritt erstellen. Sie können den standardmäßigen **Servernamen** beibehalten.

![Dialogfeld „SQL Server konfigurieren“](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

> [!NOTE]
> „admin“ ist als Benutzername des Administrators nicht zulässig.

* Tippen Sie im Dialogfeld **SQL-Datenbank konfigurieren** auf **OK**.

![Dialogfeld „SQL-Datenbank konfigurieren“](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* Tippen Sie im Dialogfeld **App Service erstellen** auf **Erstellen**.

![Dialogfeld „App Service erstellen“](publish-to-azure-webapp-using-vs/_static/create_as.png)

* Tippen Sie im Dialogfeld **Veröffentlichen** auf **Weiter**.

![Dialogfeld „Veröffentlichen“: Bereich „Verbindung“](publish-to-azure-webapp-using-vs/_static/pubc.png)

* Gehen Sie im Bereich **Einstellungen** im Dialogfeld **Veröffentlichen** so vor:

  * Erweitern Sie **Datenbanken**, und aktivieren Sie **Diese Verbindungszeichenfolge zur Laufzeit verwenden**.

  * Erweitern Sie **Entity Framework-Migrationen**, und aktivieren Sie **Diese Migration auf Veröffentlichung anwenden**.

* Tippen Sie auf **Veröffentlichen**, und warten Sie, bis Visual Studio die Veröffentlichung der App abgeschlossen hat.

![Dialogfeld „Veröffentlichen“: Bereich „Einstellungen“](publish-to-azure-webapp-using-vs/_static/pubs.png)

Visual Studio veröffentlicht die App in Azure und startet die Cloud-App in Ihrem Browser.

### <a name="test-your-app-in-azure"></a>Testen der App in Azure

* Testen Sie die Links **About** und **Contact**.

* Registrieren eines neuen Benutzers

![In Azure App Service in Microsoft Edge geöffnete Webanwendung](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="update-the-app"></a>Aktualisieren der App

* Bearbeiten Sie die Razor-Ansichtsdatei `Views/Home/About.cshtml`, und ändern Sie ihren Inhalt. Beispiel:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [7]}} -->

```html
@{
       ViewData["Title"] = "About";
   }
   <h2>@ViewData["Title"].</h2>
   <h3>@ViewData["Message"]</h3>

   <p>My updated about page.</p>
   ```

* Klicken Sie mit der rechten Maustaste auf das Projekt, und tippen Sie erneut auf **Veröffentlichen**.

![Geöffnetes Kontextmenü mit hervorgehobenem Link „Veröffentlichen“](publish-to-azure-webapp-using-vs/_static/pub.png)

* Nachdem die Anwendung veröffentlicht wurde, vergewissern Sie sich, dass die vorgenommenen Änderungen in Azure verfügbar sind.

### <a name="clean-up"></a>Bereinigen

Sobald Sie das Testen der App abgeschlossen haben, wechseln Sie zum [Azure-Portal](https://portal.azure.com/), und löschen Sie die App.

* Wählen Sie **Ressourcengruppen** aus, und tippen Sie dann auf die Ressourcengruppe, die Sie erstellt haben.

![Azure-Portal: Ressourcengruppen im Menü auf der Randleiste](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* Tippen Sie auf dem Blatt **Ressourcengruppe** auf **Löschen**.

![Azure-Portal: Blatt „Ressourcengruppen“](publish-to-azure-webapp-using-vs/_static/rgd.png)

* Geben Sie den Namen der Ressourcengruppe ein, und tippen Sie auf **Löschen**. Ihre App und alle anderen in diesem Tutorial erstellten Ressourcen werden nun aus Azure gelöscht.

### <a name="next-steps"></a>Nächste Schritte

* [Erste Schritte mit ASP.NET Core MVC und Visual Studio](first-mvc-app/start-mvc.md)

* [Einführung in ASP.NET Core](../index.md)

* [Grundlagen](../fundamentals/index.md)
