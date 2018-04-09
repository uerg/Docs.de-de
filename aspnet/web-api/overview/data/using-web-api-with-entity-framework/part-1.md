---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Web-API 2 mit Entity Framework 6 mit | Microsoft Docs
author: MikeWasson
description: In diesem Lernprogramm erfahren Sie, dass die Grundlagen der Erstellung einer Webanwendung mit einer ASP.NET Web API-back-End. Das Lernprogramm verwendet die Entity Framework 6 für das Layout der Daten...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 8e6d381509a121e3036ca3af91ea3b9bd0be33c2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="using-web-api-2-with-entity-framework-6"></a>Mithilfe von Web-API 2 mit Entity Framework 6
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

> In diesem Lernprogramm erfahren Sie, dass die Grundlagen der Erstellung einer Webanwendung mit einer ASP.NET Web API-back-End. Das Lernprogramm verwendet die Entity Framework 6 für die Datenschicht und Knockout.js für die clientseitige JavaScript-Anwendung. Im Lernprogramm wird gezeigt, wie die app auf Azure App Service-Web-Apps bereitzustellen.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>In diesem Lernprogramm verwendeten Versionen der Software
> 
> 
> - Web API 2.1
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> - [Knockout.js](http://knockoutjs.com/) 3.1


Dieses Lernprogramm verwendet ASP.NET Web API 2 mit Entity Framework 6, um eine Webanwendung zu erstellen, die eine Back-End-Datenbank bearbeitet. Hier ist ein Screenshot, der die Anwendung, die Sie erstellen.

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

Die app verwendet einen Entwurf Einzelseiten-Anwendung (SPA). "Einzelseiten-Anwendung" ist der allgemeine Begriff für eine Webanwendung, die einzelne HTML-Seite geladen und aktualisiert dann die Seite dynamisch, anstatt neue Seiten zu laden. Nach dem ersten Laden der Seite finden Sie die app mit dem Server über AJAX-Anforderungen. Die AJAX-Anforderungen return JSON-Daten, die die app verwendet wird, um die Benutzeroberfläche zu aktualisieren.

AJAX ist nicht neu, jedoch heute bestehen die JavaScript-Frameworks, die zum Erstellen und Verwalten einer großen anspruchsvollen SPA-Anwendung zu vereinfachen. Dieses Lernprogramm verwendet [Knockout.js](http://knockoutjs.com/), aber Sie können einem JavaScript-Client-Framework verwenden.

Hier sind die wichtigsten Bausteine für diese app:

- ASP.NET MVC erstellt die HTML-Seite.
- ASP.NET Web API verarbeitet die AJAX-Anforderungen und JSON-Daten zurückgegeben.
- Knockout.js Data-bindet die HTML-Elemente, die JSON-Daten.
- Entity Framework finden Sie in der Datenbank.

## <a name="see-this-app-running-on-azure"></a>Finden Sie unter dieser App auf Azure ausgeführt wird

Möchten Sie nicht mehr benötigen Standort ausgeführt wird, als live-Web-app angezeigt wird? Sie können eine vollständige Version der app beim Azure-Konto bereitstellen, indem Sie einfach auf die Schaltfläche mit den folgenden.

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

Sie benötigen ein Azure-Konto zum Bereitstellen dieser Lösung in Azure. Wenn Sie nicht bereits über ein Konto verfügen, müssen Sie die folgenden Optionen aus:

- [Öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) – Sie erhalten ein Guthaben können Sie kostenpflichtige Azure-Dienste zu testen und sogar nachdem sie verwendet werden bis können Sie das Konto beibehalten und Verwendung frei Azure-Dienste.
- [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) -Ihr MSDN-Abonnement erhalten Sie Gutschriften jedes Monats, die Sie für kostenpflichtige Azure-Dienste verwenden können.

## <a name="create-the-project"></a>Erstellen des Projekts

Öffnen Sie Visual Studio. Aus der **Datei** klicken Sie im Menü **neu**, und wählen Sie dann **Projekt**. (Oder klicken Sie auf **neues Projekt** auf der Startseite.)

In der **neues Projekt** Dialogfeld klicken Sie auf **Web** im linken Bereich und **ASP.NET-Webanwendung** im mittleren Bereich. Nennen Sie das Projekt BookService, und klicken Sie auf **OK**.

[![](part-1/_static/image4.png)](part-1/_static/image3.png)

In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **Web-API** Vorlage.

[![](part-1/_static/image6.png)](part-1/_static/image5.png)

Wenn Sie das Projekt in einem Azure-App-Dienst hosten möchten, lassen Sie die **Host in der Cloud** Feld überprüft.

Klicken Sie auf **OK**, um das Projekt zu erstellen.

## <a name="configure-azure-settings-optional"></a>Konfigurieren von Azure-Einstellungen (Optional)

Wenn Sie bleibt die **Host in der Cloud** Option aktiviert, die Visual Studio werden Sie aufgefordert, Microsoft Azure anmelden

[![](part-1/_static/image8.png)](part-1/_static/image7.png)

Nachdem Sie bei Azure anmelden, werden Sie von Visual Studio aufgefordert, die Web-app konfigurieren. Geben Sie einen Namen für die Website, wählen Sie Ihr Azure-Abonnement, und wählen Sie eine geografische Region. Klicken Sie unter **Datenbankserver**Option **Erstellen neuer Server**. Geben Sie einen Benutzernamen und das Kennwort ein.

[![](part-1/_static/image10.png)](part-1/_static/image9.png)

> [!div class="step-by-step"]
> [Nächste](part-2.md)
