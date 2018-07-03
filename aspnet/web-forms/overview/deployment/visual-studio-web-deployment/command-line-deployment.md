---
uid: web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
title: 'ASP.NET-webbereitstellung mithilfe von Visual Studio: Befehlszeilenbereitstellung | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, indem Warnungsprovider...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 82b8dea0-f062-4ee4-8784-3ffa30fbb1ca
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment
msc.type: authoredcontent
ms.openlocfilehash: 5673a4733257fae88fb66a3da43dfceb98c3b37a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37390873"
---
<a name="aspnet-web-deployment-using-visual-studio-command-line-deployment"></a>ASP.NET-webbereitstellung mithilfe von Visual Studio: Befehlszeilenbereitstellung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, mithilfe von Visual Studio 2012 oder Visual Studio 2010. Weitere Informationen über die Reihe finden Sie unter [im ersten Tutorial der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

In diesem Tutorial erfahren Sie, wie Sie das Visual Studio Web aufrufen Pipeline über die Befehlszeile zu veröffentlichen. Dies ist nützlich für Szenarien, in denen Sie möchten [Automatisierung des Bereitstellungsprozesses](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) anstatt dies manuell in Visual Studio, in der Regel mithilfe einer [source Code Versionskontrollsystem](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md).

## <a name="make-a-change-to-deploy"></a>Nehmen Sie eine Änderung bereitstellen

Die Seite "Info" wird derzeit den Code der Vorlage angezeigt.

![Zu den Seiten mit Vorlagencode](command-line-deployment/_static/image1.png)

Sie müssen, die durch Code ersetzen, die eine Zusammenfassung der Student Registrierung angezeigt.

Öffnen der *About.aspx* Seite, löschen Sie alle Markups innerhalb der `MainContent` `Content` -Element und fügt das folgende Markup an seiner Stelle:

[!code-aspx[Main](command-line-deployment/samples/sample1.aspx)]

Führen Sie das Projekt, und wählen Sie die **zu** Seite.

![Seite „Info“](command-line-deployment/_static/image2.png)

## <a name="deploy-to-test-by-using-the-command-line"></a>Test über die Befehlszeile bereitstellen

Sie wird nicht eine andere Datenbank ändern, also deaktivieren DbDacFx-datenbankbereitstellung für die Datenbank Aspnet-ContosoUniversity bereitstellen. Öffnen der **Webveröffentlichung** Assistenten, und die in jeder der drei Veröffentlichungsprofile, deaktivieren die **Update Database** auf das Kontrollkästchen der **Einstellungen** Registerkarte.

Suchen Sie in der Windows 8-Startbildschirm **Developer-Eingabeaufforderung für VS2012**.

Mit der rechten Maustaste in des Symbols für **Developer-Eingabeaufforderung für VS2012** , und klicken Sie auf **als Administrator ausführen**.

Geben Sie den folgenden Befehl an der Eingabeaufforderung, und Ersetzen Sie dabei den Pfad zur Projektmappendatei mit dem Pfad zu Ihrer Lösungsdatei an:

[!code-console[Main](command-line-deployment/samples/sample2.cmd)]

MSBuild wird die Projektmappe erstellt und in der testumgebung bereitstellt.

![Befehlszeilenausgabe](command-line-deployment/_static/image3.png)

Öffnen Sie einen Browser, und wechseln Sie zu `http://localhost/ContosoUniversity`, klicken Sie dann auf die **zu** Seite, um sicherzustellen, dass die Bereitstellung erfolgreich war.

Wenn Sie alle Schüler und Studenten im Test erstellt haben, sehen Sie eine leere Seite unter dem **Statistiken der Studentendaten Text** Überschrift. Wechseln Sie zu der **Schüler/Studenten** auf **hinzufügen "Student"**, und fügen Sie einige Schüler/Studenten hinzu, und wieder die **zu** Seite, um die Statistiken der Studentendaten finden Sie unter.

![Zu den Seiten im Test-Umgebung](command-line-deployment/_static/image4.png)

## <a name="key-command-line-options"></a>Key-Befehlszeilenoptionen

Der Befehl, der eingegebene Pfad der Projektmappe und zwei Eigenschaften an MSBuild übergeben werden:

[!code-console[Main](command-line-deployment/samples/sample3.cmd)]

### <a name="deploying-the-solution-versus-deploying-individual-projects"></a>Bereitstellen der Lösung und der Bereitstellung von einzelner Projekten

Angeben der Projektmappendatei bewirkt, dass alle Projekte in der Projektmappe erstellt werden sollen. Wenn Sie mehrere Webprojekte in der Projektmappe verfügen, gilt die folgende MSBuild-Verhalten auf:

- Die Eigenschaften, die Sie in der Befehlszeile angeben, werden jedem Projekt übergeben. Daher muss jedes Webprojekt ein Veröffentlichungsprofil mit dem Namen verfügen, die Sie angeben. Bei Angabe von `/p:PublishProfile=Test`, jedes Webprojekt benötigen ein Veröffentlichungsprofil mit dem Namen *Test*.
- Wenn ein anderes noch nicht erstellt werden, könnten Sie ein Projekt erfolgreich veröffentlichen. Weitere Informationen finden Sie in der Stack Overflow-Thread [MSBuild ein Fehler auftritt, mit zwei Paketen](http://stackoverflow.com/questions/14226451/msbuild-fails-with-two-packages).

Wenn Sie ein einzelnes Projekt anstelle einer Projektmappe angeben, müssen Sie einen Parameter hinzuzufügen, der angibt, die Visual Studio-Version. Wenn Sie Visual Studio 2012 verwenden wäre wie im folgenden Beispiel der Befehlszeile aus:

[!code-console[Main](command-line-deployment/samples/sample4.cmd?highlight=1)]

Die Versionsnummer für Visual Studio 2010 ist 10,0. Weitere Informationen finden Sie unter [Visual Studio-Projekt zur Anwendungskompatibilität und VisualStudioVersion](http://sedodream.com/2012/08/19/VisualStudioProjectCompatabilityAndVisualStudioVersion.aspx) Sayed Hashimi-Blog.

### <a name="specifying-the-publish-profile"></a>Das Veröffentlichungsprofil angeben

Sie können das Veröffentlichungsprofil angeben, nach Namen oder den vollständigen Pfad zu der *pubxml* Datei, wie im folgenden Beispiel gezeigt:

[!code-console[Main](command-line-deployment/samples/sample5.cmd?highlight=1)]

### <a name="web-publish-methods-supported-for-command-line-publishing"></a>Webveröffentlichung mit Methoden, die für die Befehlszeilen-Veröffentlichung unterstützt

Veröffentlichen von drei Methoden werden für die Veröffentlichung über die Befehlszeile unterstützt:

- `MSDeploy` – Mit Web Deploy veröffentlichen.
- `Package` – Durch Erstellen einer Web Deploy-Paket veröffentlichen. Sie müssen das Paket separat von der MSBuild-Befehl installieren, der erstellt wird.
- `FileSystem` – Kopieren von Dateien in einem bestimmten Ordner veröffentlichen.

### <a name="specifying-the-build-configuration-and-platform"></a>Angeben der Build-Konfiguration und Plattform

Die Buildkonfiguration und Plattform müssen in Visual Studio oder in der Befehlszeile festgelegt werden. Die Veröffentlichungsprofile enthalten Eigenschaften, die mit dem Namen sind `LastUsedBuildConfiguration` und `LastUsedPlatform`, aber Sie können diese Eigenschaften festlegen, um zu bestimmen, wie das Projekt erstellt wird. Weitere Informationen finden Sie unter [MSBuild: festlegen die Konfigurationseigenschaft](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) Sayed Hashimi-Blog.

## <a name="deploy-to-staging"></a>In der Stagingumgebung bereitstellen

Um in Azure bereitstellen, müssen Sie das Kennwort an die Befehlszeile hinzufügen. Wenn Sie das Kennwort im Veröffentlichungsprofil in Visual Studio gespeichert, es wurde in verschlüsselter Form gespeichert in der Ihre *. pubxml.user* Datei. Diese Datei wird von MSBuild nicht zugegriffen werden, wenn Sie eine Bereitstellung über die Befehlszeile ausführen, müssen Sie das Kennwort in einem Parameter über die Befehlszeile zu übergeben.

1. Kopieren Sie das Kennwort, die Sie benötigen die *.publishsettings* zuvor für die staging-Website heruntergeladene Datei. Das Kennwort ist der Wert der `userPWD` -Attribut für die Web Deploy `publishProfile` Element.

    ![Web Deploy-Kennwort](command-line-deployment/_static/image5.png)
2. Suchen Sie in der Windows 8-Startbildschirm **Developer-Eingabeaufforderung für VS2012**, und klicken Sie auf das Symbol, um die Eingabeaufforderung zu öffnen. (Sie haben nicht als Administrator dieses Mal öffnen, da Sie in IIS auf dem lokalen Computer bereitstellen.)
3. Geben Sie den folgenden Befehl an der Eingabeaufforderung, und Ersetzen Sie den Pfad zur Projektmappendatei durch den Pfad zu Ihrer Lösungsdatei hinzufügen und das Kennwort mit Ihrem Kennwort an:

    [!code-console[Main](command-line-deployment/samples/sample6.cmd)]

    Beachten Sie, dass diese über die Befehlszeile einen zusätzlichen Parameter enthält: `/p:AllowUntrustedCertificate=true`. In diesem Tutorial geschrieben wird, die `AllowUntrustedCertificate` Eigenschaft muss festgelegt werden, wenn Sie über die Befehlszeile in Azure veröffentlichen. Wenn die Lösung für diesen Fehler veröffentlicht wird, benötigen Sie kein diesen Parameter.
4. Öffnen Sie einen Browser und navigieren Sie zu der URL der staging-Website, und klicken Sie dann auf die **zu** Seite, um sicherzustellen, dass die Bereitstellung erfolgreich war.

    Wie Sie für die testumgebung gesehen haben, müssen Sie möglicherweise erstellen Sie einige Schüler/Studenten, um die Statistiken auf finden Sie unter den **zu** Seite.

## <a name="deploy-to-production"></a>Für die Produktion bereitstellen

Der Prozess für die Bereitstellung in der Produktion ähnelt der Prozess für das Staging.

1. Kopieren Sie das Kennwort, die Sie benötigen die *.publishsettings* heruntergeladene Datei, zuvor für die Website für die Produktion.
2. Open **Developer-Eingabeaufforderung für VS2012**.
3. Geben Sie den folgenden Befehl an der Eingabeaufforderung, und Ersetzen Sie den Pfad zur Projektmappendatei durch den Pfad zu Ihrer Lösungsdatei hinzufügen und das Kennwort mit Ihrem Kennwort an:

    [!code-console[Main](command-line-deployment/samples/sample7.cmd)]

    Für einen Standort für Produktion, wenn es auch Änderung an einer Datenbank Bestand, in der Regel kopieren Sie die *app\_offline.htm* Datei an den Standort vor der Bereitstellung und löschen Sie ihn nach der erfolgreichen Bereitstellung.
4. Öffnen Sie einen Browser und navigieren Sie zu der URL der staging-Website, und klicken Sie dann auf die **zu** Seite, um sicherzustellen, dass die Bereitstellung erfolgreich war.

## <a name="summary"></a>Zusammenfassung

Sie haben nun ein Anwendungsupdate bereitgestellt, über die Befehlszeile.

![Zu den Seiten im Test-Umgebung](command-line-deployment/_static/image6.png)

Im nächsten Tutorial, sehen Sie ein Beispiel zum Erweitern Sie im Web veröffentlichen Pipeline. Im Beispiel wird zeigen, wie Dateien bereitstellen, die nicht im Projekt enthalten sind.

> [!div class="step-by-step"]
> [Zurück](deploying-a-database-update.md)
> [Weiter](deploying-extra-files.md)
