---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'ASP.NET Web-Bereitstellung mit Visual Studio: Projekteigenschaften | Microsoft Docs'
author: tdykstra
description: Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder mit einem Hostinganbieter von Drittanbietern durch wählen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: fba3f089bf1693eec873b08b4bc50e3accba06ee
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "30879880"
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>ASP.NET Web-Bereitstellung mit Visual Studio: Projekteigenschaften
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung eines Drittanbieters Hostinganbieter oder Azure App Service-Web-Apps mithilfe von Visual Studio 2012 oder Visual Studio 2010. Informationen über die Reihen finden Sie unter [im ersten Lernprogramm, in der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

Einige Bereitstellungsoptionen werden konfiguriert, in den Projekteigenschaften, die in der Projektdatei gespeichert sind (die *csproj* oder *vbproj* Datei). In den meisten Fällen die Standardwerte für diese Einstellungen sind erwünscht, jedoch können Sie die **Projekteigenschaften** UI integriert Visual Studio arbeiten Sie mit diesen Einstellungen geändert haben. In diesem Lernprogramm überprüfen Sie die bereitstellungseinstellungen in **Projekteigenschaften**. Sie erstellen außerdem eine Platzhalterdatei, die bewirkt, dass einen leeren Ordner bereitgestellt werden.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Konfigurieren von bereitstellungseinstellungen im Fenster mit Projekteigenschaften

Die meisten Einstellungen, die beeinflussen, was geschieht, während der Bereitstellung sind in das Veröffentlichungsprofil zu enthalten, wie Sie in den folgenden Lernprogrammen sehen. Einige Einstellungen, die Sie kennen sollten befinden sich der **packen/veröffentlichen** Registerkarten für die **Projekteigenschaften** Fenster. Diese Einstellungen sind für jede Buildkonfiguration angegeben –, also können Sie unterschiedliche Einstellungen für ein Releasebuild erstellt haben, als für einen Debugbuild sind.

In **Projektmappen-Explorer**, mit der rechten Maustaste die **ContosoUniversity** -Projekt, wählen **Eigenschaften**, und wählen Sie dann die **Web packen/veröffentlichen**Registerkarte.

![Registerkarte "Web packen/veröffentlichen"](project-properties/_static/image1.png)

Wenn das Fenster angezeigt wird, wird standardmäßig mit Einstellungen für unabhängig davon, welche Buildkonfiguration für die Projektmappe derzeit aktiv ist. Wenn die **Konfiguration** Feld gibt keine **aktiv (Release)** Option **Version** um Einstellungen für die Releasekonfiguration Build anzuzeigen. Sie müssen sowohl der Test-und produktionsumgebungen Releasebuilds bereitstellen.

![Auswählen der Releasebuildkonfiguration](project-properties/_static/image2.png)

Mit **aktiv (Release)** oder **Version** ausgewählt, die Werte, die gelten, wenn Sie die Bereitstellung mit der Release-Build-Konfiguration angezeigt:

- In der **bereitzustellenden Elemente** Feld **nur Dateien, die zum Ausführen der Anwendung benötigt** ausgewählt ist. Andere Optionen sind **alle Dateien in diesem Projekt** oder **alle Dateien in diesem Projektordner**. Indem Sie die Standardauswahl unverändert lassen vermeiden Sie Quellcodedateien, z. B. bereitstellen. Diese Einstellung ist der Grund, warum die Ordner mit der SQL Server Compact-Binärdateien in das Projekt aufgenommen werden musste. Weitere Informationen zu dieser Einstellung finden Sie unter **Warum nicht alle Dateien im Projekt-Ordner bereitgestellt?** in [ASP.NET Web Application Project Deployment FAQ](https://msdn.microsoft.com/library/ee942158.aspx).
- **Generierte Debugsymbole ausschließen** ausgewählt ist. Sie wird nicht Debuggen wäre, wenn Sie diese Buildkonfiguration verwenden.
- **Schließen Sie alle Datenbanken in der Registerkarte "SQL packen/veröffentlichen" konfigurierten** ausgewählt ist. Gibt an, ob Visual Studio-Datenbanken sowie die Dateien bereitstellen. Obwohl das Kontrollkästchen bezeichnen nur erwähnt die **SQL packen/veröffentlichen** Registerkarte Deaktivieren dieses Kontrollkästchens würde auch deaktivieren, die Bereitstellung, die im Veröffentlichungsprofil konfiguriert ist. Sie werden, die später auf diese Weise werden, damit Sie dieses Kontrollkästchen aktiviert bleiben muss. Die **SQL packen/veröffentlichen** Registerkarte für eine Methode, die Sie in diesen Lernprogrammen verwenden veröffentlichen Legacydatenbank verwendet wird.
- Die **Einstellungen für das Webbereitstellungspaket** Abschnitt gilt nicht, da es sich bei Verwendung von nur einem Klick in diesen Lernprogrammen zu veröffentlichen.

Ändern der **Konfiguration** Dropdown-Listenfeld zu debuggen, um die Standardeinstellungen anzuzeigen, für Debugbuilds. Die Werte identisch sind, mit Ausnahme von **generierte Debugsymbole ausschließen** ist deaktiviert, sodass Sie debuggen können, wenn Sie einen Debugbuild bereitstellen.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Stellen Sie sicher, dass das Elmah-Ordner bereitstellen

Im vorherigen Lernprogramm haben Sie gesehen der [Elmah NuGet-Paket](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) stellt Funktionen für Fehler, Protokollierung und-berichterstellung bereit. In der Anwendung von Contoso University Elmah konfiguriert wurde zum Speichern von Fehlerdetails in einem Ordner namens *Elmah*:

![ELMAH-Ordner](project-properties/_static/image3.png)

Bestimmte Dateien oder Ordner ausschließen, aus Bereitstellung ist eine häufige Anforderung. ein weiteres Beispiel wäre ein Ordner, dem Benutzer Dateien hochladen können. Sie Protokolldateien nicht angezeigt werden sollen oder Hochladen von Dateien, die für die Produktion bereitgestellt werden in der Entwicklungsumgebung erstellt wurden. Und wenn Sie ein Update für die Produktion bereitstellen, die Sie nicht möchten, dass der Vorgang zum Löschen von Dateien, die in der Produktion vorhanden sind. (Abhängig davon, wie Sie eine Bereitstellungsoption festlegen, wenn eine Datei in den Zielstandort, aber nicht den Quellstandort vorhanden ist, bei der Bereitstellung, Web Deploy wird dieser gelöscht aus dem Ziel.)

Weiter oben in diesem Lernprogramm haben Sie gesehen der **bereitzustellenden Elemente** -Option in der **Web packen/veröffentlichen** auf die Registerkarte "festgelegt ist **Dateien nur erforderlich, um diese Anwendung auszuführen**. Daher werden Protokolldateien, die in der Entwicklung von Elmah erstellt werden nicht bereitgestellt ist, was geschehen soll. (Um bereitgestellt zu werden, müssten sie in das Projekt aufgenommen werden und ihre **Buildvorgang** Eigenschaft festgelegt werden, müsste **Content**. Weitere Informationen finden Sie unter **Warum nicht alle Dateien im Projekt-Ordner bereitgestellt?** in [ASP.NET Web Application Project Deployment FAQ](https://msdn.microsoft.com/library/ee942158.aspx)). Allerdings wird Web Deploy keinen Ordner am Zielstandort erstellen, sofern es mindestens eine Datei zu kopieren. Fügen Sie daher eine *".txt"* -Datei in den Ordner, das als Platzhalter fungiert, damit der Ordner kopiert werden.

In **Projektmappen-Explorer**, mit der rechten Maustaste die *Elmah* Ordner wählen **neues Element hinzufügen**, und erstellen Sie eine Textdatei namens *"Platzhalter.txt"*. Den folgenden Text gelagerte: "This is ein Platzhalterdatei, um sicherzustellen, dass der Ordner bereitstellen." Und speichern Sie die Datei. Das ist alles erfordern, um sicherzustellen, dass Visual Studio stellt diese Datei und der Ordner befindet sich im, da die **Buildvorgang** Eigenschaft *".txt"* Dateien auf festgelegt ist **Content**standardmäßig.

## <a name="summary"></a>Zusammenfassung

Sie haben jetzt alle die Bereitstellungsaufgaben für die Einrichtung abgeschlossen. In den nächsten Lernprogrammen Sie den Contoso-University Standort in der testumgebung bereitstellen und Testen sie dort.

> [!div class="step-by-step"]
> [Zurück](web-config-transformations.md)
> [Weiter](deploying-to-iis.md)
