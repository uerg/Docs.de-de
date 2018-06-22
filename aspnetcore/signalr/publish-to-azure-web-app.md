---
title: Veröffentlichen einer ASP.NET Core SignalR-app in Azure-Web-App
author: rachelappel
description: Veröffentlichen einer ASP.NET Core SignalR-app in Azure-Web-App
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 0d98c6b24b9695c0af0170173f13902bac5f55ed
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/20/2018
ms.locfileid: "36271917"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Veröffentlichen einer ASP.NET Core SignalR-app an eine Azure-Web-App

[Azure-Web-App](/azure/app-service/app-service-web-overview) ist ein [Microsoft cloud-computing](https://azure.microsoft.com/) Plattformdienst zum Hosten von Web-apps, einschließlich ASP.NET Core.

> [!NOTE]
> Dieser Artikel bezieht sich auf eine ASP.NET Core SignalR-app aus Visual Studio veröffentlichen. Besuchen Sie [SignalR-Diensts für Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) für Weitere Informationen zur Verwendung von SignalR in Azure.

## <a name="publish-the-app"></a>Veröffentlichen der App

Visual Studio bietet integrierte Tools für die Veröffentlichung in einer Azure-Web-App. Visual Studio Code-Benutzer können [Azure-CLI](/cli/azure) Befehle aus, um apps in Azure veröffentlichen. Dieser Artikel behandelt die publishing mit den Tools in Visual Studio. Zum Veröffentlichen einer app mit Azure-CLI finden Sie unter [Veröffentlichen einer ASP.NET Core-app in Azure mit Befehlszeilentools](xref:tutorials/publish-to-azure-webapp-using-cli).

Mit der rechten Maustaste auf das Projekt im **Projektmappen-Explorer** , und wählen Sie **veröffentlichen**. Überprüfen Sie, ob **neu erstellen** eingecheckt wird die **Veröffentlichungsziel auswählen** Dialogfeld, und wählen Sie **veröffentlichen**.

![Kommissionierung veröffentlichen Ziel](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Geben Sie die folgenden Informationen in den **Anwendungsdienst erstellen** Dialogfeld und wählen Sie **erstellen**.

| Element | Beschreibung |
| ---- | ----------- |
| **App-name** | Ein eindeutiger Name der app. |
| **Abonnement** | Das Azure-Abonnement, das die app verwendet. |
| **Ressourcengruppe** | Die Gruppe von verwandten Ressourcen, zu denen die app gehört.  |
| **Hosting-Plan** | Der Tarif für die Web-app. |

![Erstellen der app service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio führt die folgenden Aufgaben aus:

* Erstellt ein Veröffentlichungsprofil mit veröffentlichungseinstellungen.
* Erstellt oder verwendet eine vorhandene *Azure-Web-App* mit den angegebenen Details.
* Die app wird veröffentlicht.
* Startet einen Webbrowser, und klicken Sie mit der veröffentlichten Webanwendung geladen.

Beachten Sie das Format der URL für die app ist *{Anwendungsname} .azurewebsites .net*. Beispielsweise eine app namens `SignalRChattR` verfügt über eine URL, die aussieht `https://signalrchattr.azurewebsites.net`.

Wenn ein HTTP-502.2-Fehler auftritt, finden Sie unter [bereitstellen ASP.NET Core Preview-Version nach Azure App Service](xref:host-and-deploy/azure-apps/index) zur Fehlerbehebung.

## <a name="configure-signalr-web-app"></a>SignalR-Web-app konfigurieren

ASP.NET SignalR Core apps, die veröffentlicht werden, wie Sie benötigen ein Azure-Web-App [ARR Affinität](https://en.wikipedia.org/wiki/Application_Request_Routing) aktiviert. [WebSockets](xref:fundamentals/websockets) aktiviert sein sollten, um den Transport WebSockets-Funktion zu ermöglichen.

Navigieren Sie im Azure-Portal zu **Anwendungseinstellungen** für Ihre Web-app. Legen Sie **WebSockets** auf **auf**, und vergewissern Sie sich **ARR Affinität** ist **auf**.

![Azure Web-app-Einstellungen im Azure-portal](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 WebSockets und andere Transporte [beschränkt werden basierend auf der App Service-Plan](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>Weitere Informationen

* [Veröffentlichen einer ASP.NET Core-app in Azure mit Befehlszeilentools](xref:tutorials/publish-to-azure-webapp-using-cli?tabs=windows)
* [Veröffentlichen einer ASP.NET Core-app in Azure mit Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Hosten und ASP.NET Core Vorschau-apps in Azure bereitstellen](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
