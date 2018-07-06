---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Konfigurieren von Projekteigenschaften - 4 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 4aa1e55f21e74ae8da67d6d13c35a8de5693ec3a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37396265"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Konfigurieren von Projekteigenschaften - 4 von 12
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, das eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie die Web Publish Update installieren. Eine Einführung in die Reihe, finden Sie unter [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, das zeigt, Bereitstellungsfunktionen, die nach der RC-Version von Visual Studio 2012 eingeführt wurden, zeigt, wie zum Bereitstellen von SQL Server-Editionen als SQL Server Compact und zeigt, wie Sie in Azure App Service-Web-Apps bereitstellen, finden Sie unter [ASP.NET-webbereitstellung Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

Einige Optionen für die Bereitstellung konfiguriert sind, in den Projekteigenschaften, die in der Projektdatei gespeichert werden (die *csproj* oder *vbproj* Datei). In den meisten Fällen die Standardwerte für diese Einstellungen sind erwünscht, aber Sie können die **Projekteigenschaften** UI integriert Visual Studio, um mit diesen Einstellungen zu arbeiten, wenn Sie diese Einstellungen geändert haben. In diesem Tutorial überprüfen Sie die bereitstellungseinstellungen in **Projekteigenschaften**. Außerdem erstellen Sie eine Platzhalterdatei, die bewirkt, dass einen leeren Ordner bereitgestellt werden.

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>Konfigurieren von Bereitstellungseinstellungen in Fenster mit den Projekteigenschaften

Im Veröffentlichungsprofil, sind die meisten Einstellungen, die beeinflussen, was geschieht, während der Bereitstellung enthalten, wie Sie in den folgenden Tutorials sehen werden. Einige Einstellungen, die Sie kennen sollten befinden sich in der **packen/veröffentlichen** Registerkarten der **Projekteigenschaften** Fenster. Diese Einstellungen sind für jede Buildkonfiguration angegeben –, also können Sie verschiedene Einstellungen für ein Releasebuild erstellt haben, als für einen Debugbuild vorhanden sind.

In **Projektmappen-Explorer**, mit der rechten Maustaste die **ContosoUniversity** -Projekt, wählen **Eigenschaften**, und wählen Sie dann die **Web packen/veröffentlichen**Registerkarte.

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

Wenn das Fenster angezeigt wird, wird standardmäßig mit Einstellungen für den beliebigen Buildkonfiguration derzeit für die Lösung aktiv ist. Wenn der **Konfiguration** Feld gibt keine **aktiv (Release)** wählen **Version** um Einstellungen für die Release-Build-Konfiguration anzuzeigen. Sie stellen Releasebuilds, sowohl Ihre Test-und produktionsumgebungen bereit.

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

Mit **aktiv (Release)** oder **Version** ausgewählt, die Werte, die gelten, bei der Bereitstellung mithilfe der Releasekonfiguration für den Build angezeigt:

- In der **bereitzustellenden Elemente** Feld **nur Dateien, die zum Ausführen der Anwendung benötigt** ausgewählt ist. Andere Optionen sind **alle Dateien in diesem Projekt** oder **alle Dateien in diesem Projektordner**. Indem Sie die Standardauswahl unverändert lassen vermeiden Sie die Bereitstellung von Quellcodedateien, z. B. Diese Einstellung ist der Grund, warum die Ordner mit den SQL Server Compact binären Dateien im Projekt enthalten sein musste. Weitere Informationen zu dieser Einstellung finden Sie unter **Warum nicht alle Dateien im Projektordner bereitgestellt?** in [ASP.NET Web Application Project-Bereitstellung – häufig gestellte Fragen](https://msdn.microsoft.com/library/ee942158.aspx).
- **Generierte Debugsymbole ausschließen** ausgewählt ist. Sie wird nicht Debuggen werden, wenn Sie diese Buildkonfiguration verwenden.
- **Ausschließen von Dateien aus der App\_Datenordner** nicht ausgewählt ist. Ihre SQL Server Compact-Datei für die Mitgliedschaftsdatenbank in diesem Ordner ist und Sie bereitstellen. Beim Bereitstellen von Updates, die keine Änderungen in der Datenbank enthalten, müssen Sie dieses Kontrollkästchen aktivieren.
- **Diese Anwendung vor der Veröffentlichung Vorkompilieren** nicht ausgewählt ist. In den meisten Szenarien besteht keine Notwendigkeit zum Vorkompilieren von Webanwendungsprojekten zur Verfügung. Weitere Informationen zu dieser Option finden Sie unter [Registerkarte Web packen/veröffentlichen "," Project Properties](https://msdn.microsoft.com/library/dd410108(v=vs.110).aspx) und [erweiterte vorkompilieren, Dialogfeld "Einstellungen"](https://msdn.microsoft.com/library/hh475319(v=vs.110).aspx).
- **Enthalten alle Benutzerdatenbanken auf der Registerkarte "SQL packen/veröffentlichen" konfigurierten** ausgewählt ist, aber diese Option hat keine Auswirkungen jetzt, da Sie konfigurieren, werden nicht die **SQL packen/veröffentlichen** Registerkarte. Diese Registerkarte ist für eine Legacydatenbank-Bereitstellungsmethode, mit der die einzige Option für die Bereitstellung von SQL Server-Datenbanken. Verwenden Sie die **SQL packen/veröffentlichen** Registerkarte die [Migrieren zu SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) Tutorial.
- Die **Einstellungen für das Webbereitstellungspaket** Abschnitt gilt nicht, da es sich bei Verwendung von nur einem Klick in diesen Tutorials zu veröffentlichen.

Ändern der **Konfiguration** Dropdown-Listenfeld zu debuggen, um die Standardeinstellungen anzuzeigen, für Debugbuilds. Die Werte sind identisch, außer **generierte Debugsymbole ausschließen** deaktiviert ist, damit Sie debuggen können, wenn Sie einen Debugbuild bereitstellen.

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Machen sicher, dass das Elmah-Ordner bereitgestellt wird

Im vorherigen Tutorial haben Sie gesehen die [Elmah-NuGet-Paket](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) stellt Funktionen für Fehler, Protokollierung und berichterstellung bereit. In der Contoso University-Anwendung Elmah konfiguriert wurde zum Speichern von Fehlerdetails in einem Ordner namens *Elmah*:

![ELMAH-Ordner](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

Ausschließen von bestimmte Dateien oder Ordner von der Bereitstellung ist eine häufige Anforderung an. ein weiteres Beispiel wäre ein Ordner, dem Benutzer Dateien hochladen können. Sie möchten nicht die Protokolldateien oder Hochladen von Dateien, die in Ihrer Entwicklungsumgebung für die Produktion bereitgestellt werden, erstellt wurden. Und wenn Sie ein Update für die Produktion bereitstellen, die Sie nicht möchten, dass der Prozess der Dateien zu löschen, die in der produktionsumgebung vorhanden sind. (Je nachdem, wie Sie eine Bereitstellungsoption festlegen, wenn eine Datei in den Zielstandort, aber nicht für den Quellstandort vorhanden, beim Bereitstellen ist, werden die Web Deploy aus das Ziel.)

Weiter oben in diesem Tutorial haben Sie gesehen die **bereitzustellenden Elemente** option die **Web packen/veröffentlichen** Registerkarte nastaven NA hodnotu **Dateien nur erforderlich, zum Ausführen dieser Anwendung**. Daher werden Protokolldateien, die in der Entwicklung von Elmah erstellt werden nicht bereitgestellt werden die ist, was geschehen soll. (Um bereitgestellt werden, müssten sie in das Projekt einbezogen werden und deren **Buildvorgang** -Eigenschaft müssen festgelegt werden, um **Content**. Weitere Informationen finden Sie unter **Warum nicht alle Dateien im Projektordner bereitgestellt?** in [ASP.NET Web Application Project-Bereitstellung – häufig gestellte Fragen](https://msdn.microsoft.com/library/ee942158.aspx)). Allerdings wird Web Deploy keinen Ordner erstellen am Zielstandort, sofern es mindestens eine Datei zu kopieren. Fügen Sie daher eine *.txt* Datei in den Ordner, die als Platzhalter fungiert, damit der Ordner kopiert werden.

In **Projektmappen-Explorer**, mit der rechten Maustaste die *Elmah* Ordner **neues Element hinzufügen**, und erstellen Sie eine Datei mit dem Namen *"Platzhalter.txt"*. Platzieren Sie den folgenden Text in er: "Dies ist eine Platzhalterdatei, um sicherzustellen, dass der Ordner bereitgestellt wird." ein, und speichern Sie die Datei. Das ist alles, was Sie ist, um sicherzustellen, dass Visual Studio stellt diese Datei und den Ordner, der es sich befindet, da die **Buildvorgang** Eigenschaft *.txt* Dateien nastaven NA hodnotu **Inhalt**standardmäßig.

Sie haben jetzt alle die Bereitstellungsaufgaben für die Einrichtung abgeschlossen. Im nächsten Tutorial Sie den Standort Contoso University in der testumgebung bereitstellen und Testen sie dort.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
