---
title: Veröffentlichen einer ASP.NET Core SignalR-app in Azure-Web-App
author: tdykstra
description: Veröffentlichen einer ASP.NET Core SignalR-app in Azure-Web-App
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/20/2018
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: a6a0e44f5c67fefdac6bd26b3772c23e75f8bfc1
ms.sourcegitcommit: 32f5ee0690604d451f61e9a5c28881c9fcf85738
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/29/2018
ms.locfileid: "47454725"
---
# <a name="publish-an-aspnet-core-signalr-app-to-an-azure-web-app"></a>Veröffentlichen einer ASP.NET Core SignalR-app zu einer Azure-Web-App

[Azure-Web-App](/azure/app-service/app-service-web-overview) ist eine [Microsoft cloud-computing](https://azure.microsoft.com/) Plattformdienst zum Hosten von Web-apps, einschließlich ASP.NET Core.

> [!NOTE]
> In diesem Artikel bezieht sich auf eine ASP.NET Core SignalR-app aus Visual Studio veröffentlichen. Besuchen Sie [SignalR-Diensts für Azure](https://azure.microsoft.com/en-gb/services/signalr-service?) für Weitere Informationen zur Verwendung von SignalR in Azure.

## <a name="publish-the-app"></a>Veröffentlichen der App

Visual Studio bietet integrierte Tools für die Veröffentlichung in einer Azure-Web-App. Visual Studio Code-Benutzer können [Azure-Befehlszeilenschnittstelle](/cli/azure) Befehle aus, um apps in Azure veröffentlichen. Dieser Artikel behandelt die Veröffentlichung mit den Tools in Visual Studio. Zum Veröffentlichen einer app mithilfe von Azure-Befehlszeilenschnittstelle finden Sie unter [veröffentlichen eine ASP.NET Core-app in Azure mit Befehlszeilentools](/azure/app-service/app-service-web-get-started-dotnet).

Mit der rechten Maustaste auf das Projekt im **Projektmappen-Explorer** , und wählen Sie **veröffentlichen**. Überprüfen Sie, ob **neu erstellen** aktiviert ist, der **Veröffentlichungsziel** Dialogfeld ein, und wählen Sie **veröffentlichen**.

![Wählen Sie als Veröffentlichungsziel](publish-to-azure-web-app/_static/pick-publish-target-dialog.png)

Geben Sie die folgende Informationen in den **App Service erstellen** Dialogfeld, und klicken **erstellen**.

| Element | Beschreibung |
| ---- | ----------- |
| **App-name** | Ein eindeutiger Name der app. |
| **Abonnement** | Das Azure-Abonnement, das die app verwendet. |
| **Ressourcengruppe** | Die Gruppe von zugehörigen Ressourcen zu denen die app gehört.  |
| **Hostingplan** | Der Tarif für die Web-app. |

![Erstellen von app service](publish-to-azure-web-app/_static/create-app-service-dialog.png)

Visual Studio führt die folgenden Aufgaben aus:

* Erstellt ein Veröffentlichungsprofil mit veröffentlichungseinstellungen.
* Erstellt oder verwendet eine vorhandene *Azure-Web-App* mit den angegebenen Details.
* Die app wird veröffentlicht.
* Startet einen Browser mit der veröffentlichten Web-app geladen.

Beachten Sie, dass das Format der URL für die app *app {Name}.azurewebsites.NET".net*. Beispielsweise eine app namens `SignalRChattR` verfügt über eine URL, der aussieht wie `https://signalrchattr.azurewebsites.net`.

Wenn ein HTTP-502.2-Fehler auftritt, finden Sie unter [Bereitstellen von ASP.NET Core Vorschauversion für Azure App Service](xref:host-and-deploy/azure-apps/index) zur Fehlerbehebung.

## <a name="configure-signalr-web-app"></a>SignalR-Web-app konfigurieren

ASP.NET Core SignalR-apps, die veröffentlicht werden, wie eine Azure-Web-App benötigen [ARR-Affinität](https://en.wikipedia.org/wiki/Application_Request_Routing) aktiviert. [WebSockets](xref:fundamentals/websockets) aktiviert sein sollte, um die WebSockets-Übertragung-Funktion zu ermöglichen.

Navigieren Sie im Azure-Portal zu **Anwendungseinstellungen** für Ihre Web-app. Legen Sie **WebSockets** zu **auf**, und vergewissern Sie sich **ARR-Affinität** ist **auf**.

![Azure Web-app-Einstellungen im Azure-portal](publish-to-azure-web-app/_static/azure-web-app-settings.png)

 WebSockets und andere Transporte [sind basierend auf den App Service-Plan beschränkt](/azure/azure-subscription-service-limits#app-service-limits).

## <a name="related-resources"></a>Weitere Informationen

* [Veröffentlichen einer ASP.NET Core-Apps in Azure mit Befehlszeilentools](/azure/app-service/app-service-web-get-started-dotnet)
* [Veröffentlichen einer ASP.NET Core-Apps in Azure mit Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs)
* [Hosten und Bereitstellen von ASP.NET Core-Vorschau-apps in Azure](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
