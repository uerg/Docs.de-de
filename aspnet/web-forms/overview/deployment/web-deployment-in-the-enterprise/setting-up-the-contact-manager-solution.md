---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Die Kontakt-Manager-Lösung einrichten | Microsoft Docs
author: jrjlee
description: In diesem Thema wird beschrieben, wie herunterladen und konfigurieren die Projektmappe Contact Manager lokal auf einer Arbeitsstation Developer ausgeführt wird.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: e8fb24f5b2d96d864d1aa6bc0f78644773de00ab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="setting-up-the-contact-manager-solution"></a>Die Kontakt-Manager-Lösung einrichten
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie herunterladen und konfigurieren die Projektmappe Contact Manager lokal auf einer Arbeitsstation Developer ausgeführt wird.


## <a name="system-requirements"></a>Systemanforderungen

Die Kontakt-Manager-Projektmappe lokal ausführen und die anderen in diesem Lernprogramm beschriebenen Aufgaben ausführen können, müssen Sie zum Installieren der Software auf der Arbeitsstation Developer:

- Visual Studio 2010 Service Pack 1, Premium oder Ultimate Edition
- Internetinformationsdienste (IIS) 7.5 Express
- SQL Server Express 2008 R2
- IIS Webbereitstellungstool (Web Deploy) 2.1 oder höher
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1

Mit Ausnahme von Visual Studio 2010 können Sie herunterladen und installieren Sie die neuesten Versionen aller dieser Produkte und Komponenten über die [Webplattform-Installer](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Herunterladen Sie und extrahieren Sie die Projektmappe

Sie können die Kontakt-Manager-beispielanwendung in der MSDN Code Gallery herunterladen [hier](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Konfigurieren Sie und starten Sie die Projektmappe

Zum Konfigurieren und die Projektmappe Contact Manager auf dem lokalen Computer ausführen, müssen Sie diese allgemeinen Schritte ausführen:

1. Wenn Sie noch keine besitzen, erstellen Sie eine lokale Datenbank für ASP.NET-Anwendungsdienste mit die standardmitgliedschafts- und Management-Funktionen aktiviert.
2. Bearbeiten von Verbindungszeichenfolgen in der *"Web.config"* Dateien auf der lokalen SQL Server Express-Instanz verweisen.
3. Führen Sie die Projektmappe aus Visual Studio 2010.

Der übrige Teil dieses Abschnitts enthält weitere Hinweise zum jede dieser Aufgaben abschließen.

**Zum Erstellen der Datenbank für die Anwendungsdienste**

1. Öffnen Sie eine Visual Studio 2010-Eingabeaufforderung. Zu diesem Zweck die **starten** Sie im Menü **Programme**, klicken Sie auf **Microsoft Visual Studio 2010**, klicken Sie auf **Visual Studio-Tools**, und klicken Sie dann Klicken Sie auf **Visual Studio-Eingabeaufforderung (2010)**.
2. Geben Sie diesen Befehl an der Eingabeaufforderung, und drücken Sie dann die EINGABETASTE:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Verwenden der **– C** Schalters, der die Verbindungszeichenfolge für den Datenbankserver angeben.
    2. Verwenden der **– ein** Switches an die Anwendung services-Funktionen, die der Datenbank hinzugefügt werden sollen. In diesem Fall **m** gibt an, dass die Unterstützung für den Mitgliedschaftsanbieter hinzugefügt werden soll und **r** gibt an, dass die Unterstützung für die Rollen-Manager hinzugefügt werden soll.
    3. Verwenden der **– d** Switch einen Namen für Ihre Anwendung-Services-Datenbank an. Wenn Sie diese Option weglassen, erstellt das Hilfsprogramm eine Datenbank mit dem Standardnamen der **Aspnetdb**.
3. Wenn die Datenbank erfolgreich erstellt wurde, wird der Eingabeaufforderung eine Bestätigung angezeigt.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Weitere Informationen zu den Aspnet\_Regsql-Dienstprogramm finden Sie unter [ASP.NET SQL Server-Registrierungstool (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).


Der nächste Schritt besteht, um sicherzustellen, dass die Verbindungszeichenfolgen in der Contact-Manager-Lösung für Ihre lokale Instanz von SQL Server Express zeigen.

**Zum Aktualisieren der Verbindungszeichenfolgen**

1. Öffnen Sie die Kontakt-Manager-Projektmappe in Visual Studio 2010.
2. In der **Projektmappen-Explorer** Fenster, erweitern Sie die **ContactManager.Mvc** Projekt, und doppelklicken Sie dann auf die **"Web.config"** Knoten.

    > [!NOTE]
    > Das ContactManager.Mvc-Projekt enthält zwei *"Web.config"* Dateien. Sie müssen die Datei auf Projektebene bearbeiten.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. In der **ConnectionStrings** Element, stellen Sie sicher, dass die Verbindungszeichenfolge mit dem Namen **ApplicationServices** verweist auf die lokale Datenbank für ASP.NET-Anwendungsdienste.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. In der **Projektmappen-Explorer** Fenster, erweitern Sie die **ContactManager.Service** Projekt, und doppelklicken Sie dann auf die **"Web.config"** Knoten.

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. In der **ConnectionStrings** Elements in der Verbindungszeichenfolge mit dem Namen **ContactManagerContext**, überprüfen Sie, ob die **Datenquelle** Eigenschaftensatz für Ihre lokale Instanz von SQL Server Server Express. Sie müssen nichts in der Verbindungszeichenfolge ändern.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Speichern Sie alle geöffneten Dateien.

Sie sollten jetzt bereit, führen Sie die Projektmappe Contact Manager auf dem lokalen Computer sein.

> [!NOTE]
> Wenn Sie folgende Schritte ausführen, ohne zunächst eine Datenbank erstellt, wird ASP.NET die Datenbank erstellen erstmalig Sie versuchen, einen Benutzer zu erstellen. Allerdings werden Ihnen die Datenbank manuell erstellen deutlich mehr Kontrolle über die Anwendung Dienste Funktionsgruppe, die Sie unterstützen möchten.


**Die Projektmappe Contact Manager ausführen**

1. Drücken Sie F5 in Visual Studio 2010.
2. Internet Explorer gestartet wird und die URL der Kontakt-Manager ASP.NET MVC 3-Anwendung anfordert. Standardmäßig zeigt die Anwendung die **alle Kontakte** Seite.

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Fügen Sie einige Kontakte hinzu, und vergewissern Sie sich, dass die Anwendung erwartungsgemäß funktioniert.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Navigieren Sie zu `http://localhost:50114/Account/Register` (passen Sie die URL aus, wenn Sie die Anwendung auf einen anderen Port gehostet sind). Fügen Sie einen Benutzernamen, eine e-Mail-Adresse und ein Kennwort ein, und stellen Sie sicher, dass Sie ein Konto wurde erfolgreich registriert werden können.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Navigieren Sie zu `http://localhost:50114/Account/LogOn` (passen Sie die URL aus, wenn Sie die Anwendung auf einen anderen Port gehostet sind). Stellen Sie sicher, dass Ihre Anmeldung das Konto verwenden, das Sie gerade erstellt haben.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Schließen Sie Internet Explorer zum Beenden des Debuggens.

## <a name="conclusion"></a>Schlussbemerkung

An diesem Punkt sollte die Projektmappe Contact Manager vollständig konfiguriert werden auf dem lokalen Computer ausgeführt werden. Sie können die Projektmappe als Referenz verwenden, bei der Arbeit durch die anderen Themen in diesem Lernprogramm.

Im nächsten Thema [verstehen die Projektdatei](understanding-the-project-file.md), wird erläutert, wie Sie die benutzerdefinierte Microsoft Build Engine (MSBuild)-Projektdateien innerhalb der Projektmappe Contact Manager verwenden können, um den Bereitstellungsprozess zu steuern.

> [!div class="step-by-step"]
> [Zurück](the-contact-manager-solution.md)
> [Weiter](understanding-the-project-file.md)
