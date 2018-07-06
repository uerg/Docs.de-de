---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Mithilfe von Web-API 2 mit Entitätsframework 6 | Microsoft-Dokumentation
author: MikeWasson
description: In diesem Tutorial erfahren Sie, dass die Grundlagen der Erstellung einer Web-Anwendung mit einer ASP.NET Web-API-back-End. Das Lernprogramm verwendet Entity Framework 6 für das Layout der Daten...
ms.author: aspnetcontent
ms.date: 05/28/2015
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: bc853413e814e6ef1a44841d114853003d7328f9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827232"
---
<a name="using-web-api-2-with-entity-framework-6"></a>Mithilfe von Web-API 2 mit Entitätsframework 6
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](https://github.com/MikeWasson/BookService)

> In diesem Tutorial erfahren Sie, dass die Grundlagen der Erstellung einer Web-Anwendung mit einer ASP.NET Web-API-back-End. Das Lernprogramm verwendet Entity Framework 6 für die Datenschicht, und klicken Sie auf "Knockout.js", für die clientseitige JavaScript-Anwendung. Das Tutorial veranschaulicht auch zum Bereitstellen der app in Azure App Service-Web-Apps.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Softwareversionen, die in diesem Tutorial verwendet werden.
> 
> 
> - Web-API 2.1
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - ["Knockout.js"](http://knockoutjs.com/) 3.1


Dieses Tutorial verwendet ASP.NET Web API 2 mit Entity Framework 6, um eine Webanwendung erstellen, die eine Back-End-Datenbank ändert. Hier ist ein Screenshot der Anwendung, die Sie erstellen.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

Die app verwendet eine Single-Page-Anwendung (SPA). "Single-Page-Anwendung" ist der allgemeine Begriff für eine Webanwendung, die eine einzelne HTML-Seite lädt, und klicken Sie dann die Seite wird dynamisch aktualisiert, anstatt neue Seiten laden. Nach dem ersten Laden der Seite werden die app mit dem Server über AJAX-Anforderungen. Die AJAX-Anforderungen JSON-Daten zurückgeben, die die app verwendet wird, um die Benutzeroberfläche zu aktualisieren.

AJAX ist nicht neu, aber heute bestehen die JavaScript-Frameworks, die zum Erstellen und Verwalten einer großen anspruchsvollen SPA-Anwendung zu erleichtern. Dieses Tutorial verwendet ["Knockout.js"](http://knockoutjs.com/), aber Sie können alle JavaScript-Clientframework verwenden.

Hier sind die wichtigsten Bausteine für diese app aus:

- ASP.NET MVC erstellt, die HTML-Seite.
- ASP.NET Web-API verarbeitet die AJAX-Anforderungen und JSON-Daten zurückgegeben.
- Knockout.js-Datenbindung der HTML-Elemente, die JSON-Daten.
- Entitätsframework finden Sie in der Datenbank.

## <a name="see-this-app-running-on-azure"></a>Finden Sie unter dieser App in Azure ausführen

Möchten Sie die fertige Website als live-Web-app ausgeführt wird, finden Sie unter? Sie können eine vollständige Version der app beim Azure-Konto bereitstellen, indem Sie einfach die folgende Schaltfläche.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Sie benötigen ein Azure-Konto zum Bereitstellen dieser Lösung in Azure. Wenn Sie nicht bereits über ein Konto verfügen, müssen Sie die folgenden Optionen aus:

- [Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – Sie erhalten ein Guthaben zum Ausprobieren Zahlungspflichtiger Azure-Dienste nutzen können, und auch nach dem sie verwendet werden, bis können Sie das Konto behalten und kostenlose Azure-Dienste.
- [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Ihr MSDN-Abonnement ein monatliches Guthaben, die Sie für zahlungspflichtige Azure-Dienste verwenden können.

## <a name="create-the-project"></a>Erstellen des Projekts

Öffnen Sie Visual Studio. Von der **Datei** , wählen Sie im Menü **neu**, und wählen Sie dann **Projekt**. (Oder klicken Sie auf **neues Projekt** auf der Startseite.)

In der **neues Projekt** Dialogfeld klicken Sie auf **Web** im linken Bereich und **ASP.NET-Webanwendung** im mittleren Bereich. Nennen Sie das Projekt BookService, und klicken Sie auf **OK**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **Web-API-** Vorlage.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Wenn Sie das Projekt in einer Azure App Service hosten möchten, lassen Sie die **in der Cloud hosten** Feld aktiviert.

Klicken Sie auf **OK**, um das Projekt zu erstellen.

## <a name="configure-azure-settings-optional"></a>Konfigurieren von Azure-Einstellungen (Optional)

Wenn Sie links die **Host in der Cloud** Option aktiviert, die Visual Studio fordert Sie zur Anmeldung beim Microsoft Azure

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

Nachdem Sie bei Azure anmelden, fordert Visual Studio Sie so konfigurieren Sie die Web-app. Geben Sie einen Namen für die Website, wählen Sie Ihr Azure-Abonnement, und wählen Sie eine geografische Region. Klicken Sie unter **Datenbankserver**Option **neuen Server erstellen**. Geben Sie einen Benutzernamen und das Kennwort ein.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [Nächste](part-2.md)
