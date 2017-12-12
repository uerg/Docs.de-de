---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Konfigurieren von Projekteigenschaften - 4 12 | Microsoft Docs'
author: tdykstra
description: "Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual das..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2ba202a1a0d0ba752576e8906b739cc9e83fde2a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Konfigurieren von Projekteigenschaften - 4 12
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie das Web veröffentlichen Update installieren. Eine Einführung in der Reihe, finden Sie unter [im ersten Lernprogramm, in der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, die die Bereitstellungsfeatures nach der RC-Version von Visual Studio 2012 eingeführt wurde, wird gezeigt, wie SQL Server-Editionen als SQL Server Compact bereitstellen, und zeigt die Vorgehensweise beim Bereitstellen in Azure App Service-Web-Apps, finden Sie unter [ASP.NET Web Deploy Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

Einige Bereitstellungsoptionen werden konfiguriert, in den Projekteigenschaften, die in der Projektdatei gespeichert sind (die *csproj* oder *vbproj* Datei). In den meisten Fällen die Standardwerte für diese Einstellungen sind erwünscht, jedoch können Sie die **Projekteigenschaften** UI integriert Visual Studio arbeiten Sie mit diesen Einstellungen geändert haben. In diesem Lernprogramm überprüfen Sie die bereitstellungseinstellungen in **Projekteigenschaften**. Sie erstellen außerdem eine Platzhalterdatei, die bewirkt, dass einen leeren Ordner bereitgestellt werden.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Konfigurieren von Bereitstellungseinstellungen im Fenster mit Projekteigenschaften

Die meisten Einstellungen, die beeinflussen, was geschieht, während der Bereitstellung sind in das Veröffentlichungsprofil zu enthalten, wie Sie in den folgenden Lernprogrammen sehen. Einige Einstellungen, die Sie kennen sollten befinden sich der **packen/veröffentlichen** Registerkarten für die **Projekteigenschaften** Fenster. Diese Einstellungen sind für jede Buildkonfiguration angegeben –, also können Sie unterschiedliche Einstellungen für ein Releasebuild erstellt haben, als für einen Debugbuild sind.

In **Projektmappen-Explorer**, mit der rechten Maustaste die **ContosoUniversity** -Projekt, wählen **Eigenschaften**, und wählen Sie dann die **Web packen/veröffentlichen**Registerkarte.

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Wenn das Fenster angezeigt wird, wird standardmäßig mit Einstellungen für unabhängig davon, welche Buildkonfiguration für die Projektmappe derzeit aktiv ist. Wenn die **Konfiguration** Feld gibt keine **aktiv (Release)**Option **Version** um Einstellungen für die Releasekonfiguration Build anzuzeigen. Sie müssen sowohl der Test-und produktionsumgebungen Releasebuilds bereitstellen.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

Mit **aktiv (Release)** oder **Version** ausgewählt, die Werte, die gelten, wenn Sie die Bereitstellung mit der Release-Build-Konfiguration angezeigt:

- In der **bereitzustellenden Elemente** Feld **nur Dateien, die zum Ausführen der Anwendung benötigt** ausgewählt ist. Andere Optionen sind **alle Dateien in diesem Projekt** oder **alle Dateien in diesem Projektordner**. Indem Sie die Standardauswahl unverändert lassen vermeiden Sie Quellcodedateien, z. B. bereitstellen. Diese Einstellung ist der Grund, warum die Ordner mit der SQL Server Compact-Binärdateien in das Projekt aufgenommen werden musste. Weitere Informationen zu dieser Einstellung finden Sie unter **Warum nicht alle Dateien im Projekt-Ordner bereitgestellt?** in [ASP.NET Web Application Project Deployment FAQ](https://msdn.microsoft.com/en-us/library/ee942158.aspx).
- **Generierte Debugsymbole ausschließen** ausgewählt ist. Sie wird nicht Debuggen wäre, wenn Sie diese Buildkonfiguration verwenden.
- **Ausschließen von Dateien aus der App\_Datenordner** nicht ausgewählt ist. Die SQL Server Compact-Datei für die Mitgliedschaftsdatenbank ist in diesem Ordner und müssen Sie es bereitstellen. Beim Bereitstellen von Updates, die keine Änderungen an der Datenbank enthalten, müssen Sie dieses Kontrollkästchen aktivieren.
- **Diese Anwendung vor der Veröffentlichung Vorkompilieren** nicht ausgewählt ist. In den meisten Fällen besteht keine Notwendigkeit zum Vorkompilieren Webanwendungsprojekte zur Verfügung. Weitere Informationen zu dieser Option finden Sie unter [packen/veröffentlichen-Registerkarte "Web", Projekteigenschaften](https://msdn.microsoft.com/en-us/library/dd410108(v=vs.110).aspx) und [erweiterte Vorkompilieren Dialog "Einstellungen"](https://msdn.microsoft.com/en-us/library/hh475319(v=vs.110).aspx).
- **Enthalten alle Benutzerdatenbanken auf der Registerkarte "SQL packen/veröffentlichen" konfigurierten** ausgewählt ist, aber diese Option hat keine Auswirkung jetzt, da Sie konfigurieren, werden nicht die **SQL packen/veröffentlichen** Registerkarte. Diese Registerkarte dient zum Legacydatenbank-Bereitstellungsmethode werden verwendet, um die einzige Option für die Bereitstellung von SQL Server-Datenbanken. Verwenden Sie die **SQL packen/veröffentlichen** Registerkarte der [Migrieren zu SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) Lernprogramm.
- Die **Einstellungen für das Webbereitstellungspaket** Abschnitt gilt nicht, da es sich bei Verwendung von nur einem Klick in diesen Lernprogrammen zu veröffentlichen.

Ändern der **Konfiguration** Dropdown-Listenfeld zu debuggen, um die Standardeinstellungen anzuzeigen, für Debugbuilds. Die Werte identisch sind, mit Ausnahme von **generierte Debugsymbole ausschließen** ist deaktiviert, sodass Sie debuggen können, wenn Sie einen Debugbuild bereitstellen.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Machen sicher, dass das Elmah-Ordner bereitstellen

Im vorherigen Lernprogramm haben Sie gesehen der [Elmah NuGet-Paket](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) stellt Funktionen für Fehler, Protokollierung und-berichterstellung bereit. In der Anwendung von Contoso University Elmah konfiguriert wurde zum Speichern von Fehlerdetails in einem Ordner namens *Elmah*:

![ELMAH-Ordner](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Bestimmte Dateien oder Ordner ausschließen, aus Bereitstellung ist eine häufige Anforderung. ein weiteres Beispiel wäre ein Ordner, dem Benutzer Dateien hochladen können. Sie Protokolldateien nicht angezeigt werden sollen oder Hochladen von Dateien, die für die Produktion bereitgestellt werden in der Entwicklungsumgebung erstellt wurden. Und wenn Sie ein Update für die Produktion bereitstellen, die Sie nicht möchten, dass der Vorgang zum Löschen von Dateien, die in der Produktion vorhanden sind. (Abhängig davon, wie Sie eine Bereitstellungsoption festlegen, wenn eine Datei in den Zielstandort, aber nicht den Quellstandort vorhanden ist, bei der Bereitstellung, Web Deploy wird dieser gelöscht aus dem Ziel.)

Weiter oben in diesem Lernprogramm haben Sie gesehen der **bereitzustellenden Elemente** -Option in der **Web packen/veröffentlichen** auf die Registerkarte "festgelegt ist **Dateien nur erforderlich, um diese Anwendung auszuführen**. Daher werden Protokolldateien, die in der Entwicklung von Elmah erstellt werden nicht bereitgestellt ist, was geschehen soll. (Um bereitgestellt zu werden, müssten sie in das Projekt aufgenommen werden und ihre **Buildvorgang** Eigenschaft festgelegt werden, müsste **Content**. Weitere Informationen finden Sie unter **Warum nicht alle Dateien im Projekt-Ordner bereitgestellt?** in [ASP.NET Web Application Project Deployment FAQ](https://msdn.microsoft.com/en-us/library/ee942158.aspx)). Allerdings wird Web Deploy keinen Ordner am Zielstandort erstellen, sofern es mindestens eine Datei zu kopieren. Fügen Sie daher eine *".txt"* -Datei in den Ordner, das als Platzhalter fungiert, damit der Ordner kopiert werden.

In **Projektmappen-Explorer**, mit der rechten Maustaste die *Elmah* Ordner wählen **neues Element hinzufügen**, und erstellen Sie eine Datei mit dem Namen *"Platzhalter.txt"*. Den folgenden Text gelagerte: "This is ein Platzhalterdatei, um sicherzustellen, dass der Ordner bereitstellen." Und speichern Sie die Datei. Das ist alles erfordern, um sicherzustellen, dass Visual Studio stellt diese Datei und der Ordner befindet sich im, da die **Buildvorgang** Eigenschaft *".txt"* Dateien auf festgelegt ist **Content**standardmäßig.

Sie haben jetzt alle die Bereitstellungsaufgaben für die Einrichtung abgeschlossen. In den nächsten Lernprogrammen Sie den Contoso-University Standort in der testumgebung bereitstellen und Testen sie dort.

>[!div class="step-by-step"]
[Zurück](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
[Weiter](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
