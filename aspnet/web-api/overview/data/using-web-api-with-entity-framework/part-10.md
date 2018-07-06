---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Veröffentlichen Sie die App in Azure, Azure App Service | Microsoft-Dokumentation
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 66dde7b54ce084eed873afae56fd686d0dc8795f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808226"
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Veröffentlichen Sie die App in Azure, Azure App Service
====================
durch [Mike Wasson](https://github.com/MikeWasson)

[Abgeschlossenes Projekt herunterladen](https://github.com/MikeWasson/BookService)

Als letzten Schritt veröffentlichen Sie die Anwendung in Azure. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **veröffentlichen**.

![](part-10/_static/image1.png)

Auf **veröffentlichen** Ruft die **Webveröffentlichung** Dialogfeld. Wenn Sie aktiviert **Host in der Cloud** Wenn Sie zuerst das Projekt, und klicken Sie dann auf die Verbindung erstellt haben und die Einstellungen bereits konfiguriert sind. In diesem Fall, klicken Sie einfach auf die **Einstellungen** Registerkarte, und überprüfen Sie &quot;Execute Code First Migrations&quot;. (Wenn Sie eine Überprüfung auf nicht **Host in der Cloud** am Anfang, und befolgen Sie dann die Schritte in der [nächsten Abschnitt](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Klicken Sie zum Bereitstellen der app auf **veröffentlichen**. Sehen Sie den veröffentlichungsfortschritt in die **Webveröffentlichungsaktivität** Fenster. (Aus der **Ansicht** , wählen Sie im Menü **Other Windows**, und wählen Sie dann **Webveröffentlichungsaktivität**.)

![](part-10/_static/image4.png)

Wenn Visual Studio abgeschlossen ist, das Bereitstellen der app, an die URL der bereitgestellten Website automatisch im Standardbrowser geöffnet wird, und die Anwendung, die Sie erstellt haben, wird jetzt in der Cloud ausgeführt. Die URL in die Adressleiste des Browsers zeigt, dass die Website aus dem Internet geladen wird.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Bereitstellung in einer neuen Website

Wenn Sie nicht aktiviert haben **Host in der Cloud** , wenn Sie zuerst das Projekt erstellt haben, können Sie eine neue Web-app jetzt konfigurieren. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste in des Projekts, und wählen **veröffentlichen**. Wählen Sie die **Profil** Registerkarte, und klicken Sie auf **Microsoft Azure Websites**. Wenn Sie sich derzeit in Azure angemeldet sind, werden Sie aufgefordert werden, für die Anmeldung.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

In der **vorhandene Websites** Dialogfeld klicken Sie auf **neu**.

![](part-10/_static/image9.png)

Geben Sie einen Standortnamen an. Wählen Sie Ihr Azure-Abonnement und die Region an. Klicken Sie unter **Datenbankserver**Option **neuen Server erstellen**, oder wählen Sie einen vorhandenen Server. Klicken Sie auf **Erstellen**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Klicken Sie auf die **Einstellungen** Registerkarte, und überprüfen Sie &quot;Execute Code First Migrations&quot;. Klicken Sie dann auf **veröffentlichen**.

> [!div class="step-by-step"]
> [Vorherige](part-9.md)
