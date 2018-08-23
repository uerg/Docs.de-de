---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'ASP.NET-webbereitstellung mithilfe von Visual Studio: Projekteigenschaften | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, indem Warnungsprovider...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: 85b2aa982c56f030c2de0f3ac2ad0dca780f3f38
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838385"
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>ASP.NET-webbereitstellung mithilfe von Visual Studio: Projekteigenschaften
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, mithilfe von Visual Studio 2012 oder Visual Studio 2010. Weitere Informationen über die Reihe finden Sie unter [im ersten Tutorial der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

Einige Optionen für die Bereitstellung konfiguriert sind, in den Projekteigenschaften, die in der Projektdatei gespeichert werden (die *csproj* oder *vbproj* Datei). In den meisten Fällen die Standardwerte für diese Einstellungen sind erwünscht, aber Sie können die **Projekteigenschaften** UI integriert Visual Studio, um mit diesen Einstellungen zu arbeiten, wenn Sie diese Einstellungen geändert haben. In diesem Tutorial überprüfen Sie die bereitstellungseinstellungen in **Projekteigenschaften**. Außerdem erstellen Sie eine Platzhalterdatei, die bewirkt, dass einen leeren Ordner bereitgestellt werden.

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>Konfigurieren von bereitstellungseinstellungen im Fenster mit Projekteigenschaften

Im Veröffentlichungsprofil, sind die meisten Einstellungen, die beeinflussen, was geschieht, während der Bereitstellung enthalten, wie Sie in den folgenden Tutorials sehen werden. Einige Einstellungen, die Sie kennen sollten befinden sich in der **packen/veröffentlichen** Registerkarten der **Projekteigenschaften** Fenster. Diese Einstellungen sind für jede Buildkonfiguration angegeben –, also können Sie verschiedene Einstellungen für ein Releasebuild erstellt haben, als für einen Debugbuild vorhanden sind.

In **Projektmappen-Explorer**, mit der rechten Maustaste die **ContosoUniversity** -Projekt, wählen **Eigenschaften**, und wählen Sie dann die **Web packen/veröffentlichen**Registerkarte.

![Registerkarte "Web packen/veröffentlichen"](project-properties/_static/image1.png)

Wenn das Fenster angezeigt wird, wird standardmäßig mit Einstellungen für den beliebigen Buildkonfiguration derzeit für die Lösung aktiv ist. Wenn der **Konfiguration** Feld gibt keine **aktiv (Release)** wählen **Version** um Einstellungen für die Release-Build-Konfiguration anzuzeigen. Sie stellen Releasebuilds, sowohl Ihre Test-und produktionsumgebungen bereit.

![Release-Buildkonfiguration auswählen](project-properties/_static/image2.png)

Mit **aktiv (Release)** oder **Version** ausgewählt, die Werte, die gelten, bei der Bereitstellung mithilfe der Releasekonfiguration für den Build angezeigt:

- In der **bereitzustellenden Elemente** Feld **nur Dateien, die zum Ausführen der Anwendung benötigt** ausgewählt ist. Andere Optionen sind **alle Dateien in diesem Projekt** oder **alle Dateien in diesem Projektordner**. Indem Sie die Standardauswahl unverändert lassen vermeiden Sie die Bereitstellung von Quellcodedateien, z. B. Diese Einstellung ist der Grund, warum die Ordner mit den SQL Server Compact binären Dateien im Projekt enthalten sein musste. Weitere Informationen zu dieser Einstellung finden Sie unter **Warum nicht alle Dateien im Projektordner bereitgestellt?** in [ASP.NET Web Application Project-Bereitstellung – häufig gestellte Fragen](https://msdn.microsoft.com/library/ee942158.aspx).
- **Generierte Debugsymbole ausschließen** ausgewählt ist. Sie wird nicht Debuggen werden, wenn Sie diese Buildkonfiguration verwenden.
- **Enthalten alle Benutzerdatenbanken auf der Registerkarte "SQL packen/veröffentlichen" konfigurierten** ausgewählt ist. Gibt an, ob es sich bei Visual Studio-Datenbanken sowie die Dateien bereitgestellt wird. Obwohl Sie das Kontrollkästchen Bezeichnung nur erwähnt die **SQL packen/veröffentlichen** Registerkarte das Deaktivieren dieses Kontrollkästchens wird auch deaktivieren, datenbankbereitstellung, die im Veröffentlichungsprofil konfiguriert ist. Sie werden, die später durchführen, damit Sie dieses Kontrollkästchen aktiviert bleiben müssen. Die **SQL packen/veröffentlichen** Registerkarte dient für eine ältere Datenbank veröffentlichen die Methode, die Sie nicht in diesen Tutorials verwenden werden.
- Die **Einstellungen für das Webbereitstellungspaket** Abschnitt gilt nicht, da es sich bei Verwendung von nur einem Klick in diesen Tutorials zu veröffentlichen.

Ändern der **Konfiguration** Dropdown-Listenfeld zu debuggen, um die Standardeinstellungen anzuzeigen, für Debugbuilds. Die Werte sind identisch, außer **generierte Debugsymbole ausschließen** deaktiviert ist, damit Sie debuggen können, wenn Sie einen Debugbuild bereitstellen.

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Stellen Sie sicher, dass der Elmah-Ordner bereitgestellt wird

Im vorherigen Tutorial haben Sie gesehen die [Elmah-NuGet-Paket](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) stellt Funktionen für Fehler, Protokollierung und berichterstellung bereit. In der Contoso University-Anwendung Elmah konfiguriert wurde zum Speichern von Fehlerdetails in einem Ordner namens *Elmah*:

![ELMAH-Ordner](project-properties/_static/image3.png)

Ausschließen von bestimmte Dateien oder Ordner von der Bereitstellung ist eine häufige Anforderung an. ein weiteres Beispiel wäre ein Ordner, dem Benutzer Dateien hochladen können. Sie möchten nicht die Protokolldateien oder Hochladen von Dateien, die in Ihrer Entwicklungsumgebung für die Produktion bereitgestellt werden, erstellt wurden. Und wenn Sie ein Update für die Produktion bereitstellen, die Sie nicht möchten, dass der Prozess der Dateien zu löschen, die in der produktionsumgebung vorhanden sind. (Je nachdem, wie Sie eine Bereitstellungsoption festlegen, wenn eine Datei in den Zielstandort, aber nicht für den Quellstandort vorhanden, beim Bereitstellen ist, werden die Web Deploy aus das Ziel.)

Weiter oben in diesem Tutorial haben Sie gesehen die **bereitzustellenden Elemente** option die **Web packen/veröffentlichen** Registerkarte nastaven NA hodnotu **Dateien nur erforderlich, zum Ausführen dieser Anwendung**. Daher werden Protokolldateien, die in der Entwicklung von Elmah erstellt werden nicht bereitgestellt werden die ist, was geschehen soll. (Um bereitgestellt werden, müssten sie in das Projekt einbezogen werden und deren **Buildvorgang** -Eigenschaft müssen festgelegt werden, um **Content**. Weitere Informationen finden Sie unter **Warum nicht alle Dateien im Projektordner bereitgestellt?** in [ASP.NET Web Application Project-Bereitstellung – häufig gestellte Fragen](https://msdn.microsoft.com/library/ee942158.aspx)). Allerdings wird Web Deploy keinen Ordner erstellen am Zielstandort, sofern es mindestens eine Datei zu kopieren. Fügen Sie daher eine *.txt* Datei in den Ordner, die als Platzhalter fungiert, damit der Ordner kopiert werden.

In **Projektmappen-Explorer**, mit der rechten Maustaste die *Elmah* Ordner **neues Element hinzufügen**, und erstellen Sie eine Textdatei namens *"Platzhalter.txt"*. Platzieren Sie den folgenden Text in er: "Dies ist eine Platzhalterdatei, um sicherzustellen, dass der Ordner bereitgestellt wird." ein, und speichern Sie die Datei. Das ist alles, was Sie ist, um sicherzustellen, dass Visual Studio stellt diese Datei und den Ordner, der es sich befindet, da die **Buildvorgang** Eigenschaft *.txt* Dateien nastaven NA hodnotu **Inhalt**standardmäßig.

## <a name="summary"></a>Zusammenfassung

Sie haben jetzt alle die Bereitstellungsaufgaben für die Einrichtung abgeschlossen. Im nächsten Tutorial Sie den Standort Contoso University in der testumgebung bereitstellen und Testen sie dort.

> [!div class="step-by-step"]
> [Zurück](web-config-transformations.md)
> [Weiter](deploying-to-iis.md)
