---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: "Veröffentlichen Sie die App in Azure-Azure App Service | Microsoft Docs"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 08994815cb339800619caacdcb8d717e9986f9d5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Veröffentlichen Sie die App in Azure-Azure App Service
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Herunterladen des abgeschlossenen Projekts](https://github.com/MikeWasson/BookService)

Als letzten Schritt veröffentlichen Sie die Anwendung in Azure. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **veröffentlichen**.

![](part-10/_static/image1.png)

Auf **veröffentlichen** Ruft die **Web veröffentlichen** Dialogfeld. Wenn Sie diese Option aktiviert **Host in der Cloud** bei Erstellung des Projekts, und klicken Sie dann auf die Verbindung und Einstellungen bereits konfiguriert sind. In diesem Fall klicken Sie einfach auf die **Einstellungen** und prüfen Sie &quot;führen Sie Code First-Migrationen&quot;. (Wenn Sie eine Überprüfung auf nicht **Host in der Cloud** am Anfang, und führen Sie dann die Schritte in der [nächsten Abschnitt](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Um die app bereitstellen möchten, klicken Sie auf **veröffentlichen**. Sie können die publishing Status in Anzeigen der **Webaktivität veröffentlichen** Fenster. (Aus der **Ansicht** klicken Sie im Menü **Weitere Fenster**, und wählen Sie dann **Webaktivität veröffentlichen**.)

![](part-10/_static/image4.png)

Beendigung Visual Studio Bereitstellen der app der Standardbrowser automatisch geöffnet wird, um die URL der Website bereitgestellt, und die Anwendung, die Sie erstellt haben, wird nun in der Cloud ausgeführt. Die URL in die Adressleiste des Browsers zeigt, dass die Website aus dem Internet geladen wird.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Bereitstellung in einer neuen Website

Wenn Sie nicht überprüfen **Host in der Cloud** beim Erstellen des Projekts können Sie jetzt eine neue Web-app konfigurieren. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste des Projekts, und wählen **veröffentlichen**. Wählen Sie die **Profil** Registerkarte, und klicken Sie auf **Microsoft Azure-Websites**. Wenn Sie derzeit in Azure angemeldet sind, werden Sie aufgefordert, sich anzumelden.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

In der **vorhandene Websites** Dialogfeld klicken Sie auf **neu**.

![](part-10/_static/image9.png)

Geben Sie einen Standortnamen an. Wählen Sie Ihr Azure-Abonnement und die Region an. Unter **Datenbankserver**Option **Erstellen neuer Server**, oder wählen Sie einen vorhandenen Server. Klicken Sie auf **Erstellen**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Klicken Sie auf die **Einstellungen** und prüfen Sie &quot;führen Sie Code First-Migrationen&quot;. Klicken Sie dann auf **veröffentlichen**.

>[!div class="step-by-step"]
[Zurück](part-9.md)
