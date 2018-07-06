---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Umwandlungen für die Datei "Web.config" - 3 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: a5afcf4d05d6fa51ad70f7e5bdfafd20002f188a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841757"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Umwandlungen für die Datei "Web.config" - 3 von 12
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, das eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie die Web Publish Update installieren. Eine Einführung in die Reihe, finden Sie unter [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, das zeigt, Bereitstellungsfunktionen, die nach der RC-Version von Visual Studio 2012 eingeführt wurden, zeigt, wie zum Bereitstellen von SQL Server-Editionen als SQL Server Compact und zeigt, wie Sie in Azure App Service-Web-Apps bereitstellen, finden Sie unter [ASP.NET-webbereitstellung Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

In diesem Tutorial erfahren Sie, wie Sie den Prozess des Umschaltens automatisieren die *"Web.config"* -Datei, wenn Sie unterschiedliche zielumgebungen bereitstellen. Die meisten Anwendungen verfügen über Einstellungen der *"Web.config"* -Datei, die muss unterschiedlich sein, wenn die Anwendung bereitgestellt wird. Automatisierung des Prozesses, die diese Änderungen verhindert, dass sie manuell durchgeführt werden jedes Mal, wenn Sie bereitgestellt haben, die aufwendig sein würde und fehleranfällig.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Web.config-Transformationen im Vergleich zu Web Deploy-Parametern

Es gibt zwei Möglichkeiten zum Automatisieren des Prozess des Umschaltens *"Web.config"* Einstellungen der Datei: [Web.config-Transformationen](https://msdn.microsoft.com/library/dd465326.aspx) und [Web Deploy-Parametern](https://msdn.microsoft.com/library/ff398068.aspx). Ein *"Web.config"* Transformationsdatei enthält XML-Markup, der angibt, wie Sie ändern die *"Web.config"* -Datei, wenn er bereitgestellt wird. Sie können die verschiedene Änderungen für bestimmte Buildkonfigurationen und für bestimmte Veröffentlichungsprofile angeben. Die standardmäßige Buildkonfigurationen werden Debug- und, und Sie können benutzerdefinierte Konfigurationen erstellen. Ein Veröffentlichungsprofil entspricht in der Regel in einer zielumgebung. (Erfahren Sie mehr über Veröffentlichungsprofile in die [Bereitstellen in IIS als Testumgebung](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) Tutorial.)

Web Deploy-Parameter können verwendet werden, um viele verschiedene Arten von Einstellungen anzugeben, die konfiguriert werden müssen, während der Bereitstellung, einschließlich Einstellungen, die in gefunden werden *"Web.config"* Dateien. Wenn an *"Web.config"* dateiänderungen, Web Deploy-Parameter sind zum Einrichten komplexer, aber sie sind hilfreich, wenn Sie den Wert festgelegt werden, bis Sie Sie bereitstellen, nicht kennen. Z. B. in einer unternehmensumgebung möglicherweise erstellen Sie eine *Bereitstellungspaket* und weisen Sie ihm eine Person in der IT-Abteilung in einer produktionsumgebung installieren und in der Lage, geben Verbindungszeichenfolgen oder Kennwörter, die Sie nicht der Fall ist, hat kennen.

Für das Szenario, das in diesem Tutorial erfahren Sie alles wissen, die erfolgen, um die *"Web.config"* Datei, sodass Sie nicht benötigen, verwenden Sie Web Deploy-Parameter. Konfigurieren Sie einige Transformationen, die unterscheiden sich abhängig von der Buildkonfiguration verwendet und andere, unterscheiden sich je nach dem Veröffentlichungsprofil ein verwendet.

## <a name="creating-transformation-files-for-publish-profiles"></a>Transformationsdateien erstellen für Veröffentlichungsprofile

In **Projektmappen-Explorer**, erweitern Sie *"Web.config"* , finden Sie unter den *"Web.Debug.config"* und *Datei "Web.Release.config"* Transformationsdateien aufgeführt, die werden standardmäßig für die zwei Standardkonfigurationen für den Build erstellt werden.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Sie können Transformationsdateien für benutzerdefinierte Build-Konfigurationen erstellen, indem Sie mit der rechten Maustaste in der Datei "Web.config", und wählen **Konfigurationstransformationen hinzufügen** aus dem Kontextmenü, aber für dieses Tutorial müssen nicht dies tun.

Sie benötigen zwei weitere Transformationsdateien, zum Konfigurieren von Änderungen, die in das Bereitstellungsziel anstatt in der Buildkonfiguration verknüpft sind. Ein typisches Beispiel dieser Art der Einstellung ist ein WCF-Endpunkt, der für Tests und Produktion unterscheidet. Veröffentlichen Sie in späteren Tutorials Sie erstellen Profile mit dem Namen Test- und Produktionsumgebungen, daher Sie müssen eine *Web.Test.config* Datei und ein *Web.Production.config* Datei.

Transformation für Dateien, die zum Veröffentlichen von Profilen verknüpft sind, müssen manuell erstellt werden. In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und wählen Sie **Ordner in Windows Explorer öffnen**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

In **Windows Explorer**, wählen die *Datei "Web.Release.config"* Datei, kopieren Sie die Datei, und fügen Sie zwei Kopien des Zertifikats. Benennen Sie diese Kopien *Web.Production.config* und *Web.Test.config*, und schließen **Windows Explorer**.

In **Projektmappen-Explorer**, klicken Sie auf **aktualisieren** um die neuen Dateien anzuzeigen.

Wählen Sie die neuen Dateien, mit der rechten Maustaste, und klicken Sie dann auf **zu Projekt** im Kontextmenü.

![Einschließlich der Test- und Produktionsumgebungen Config-Dateien im Projekt](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Um zu verhindern, dass diese Dateien bereitgestellt wird, wählen sie im **Projektmappen-Explorer**, und klicken Sie dann in der **Eigenschaften** Fenster Ändern der **Buildvorgang** Eigenschaft **Content** zu **keine**. (Die Transformationsdateien, die sich auf Konfigurationen basieren werden automatisch aus der Bereitstellung verhindert.)

Sie sind jetzt bereit für die Eingabe *"Web.config"* Transformationen in der *"Web.config"* Transformationsdateien.

## <a name="limiting-error-log-access-to-administrators"></a>Fehler-Log-Zugriff für Administratoren beschränken

Wenn ein Fehler vorliegt, wenn die Anwendung ausgeführt wird, die Anwendung eine generische Fehlerseite anstelle der Seite vom System generierte Fehler zeigt und verwendet [Elmah-NuGet-Paket](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) für Fehler, Protokollierung und berichterstellung. Die `customErrors` Element in der *"Web.config"* Datei gibt die Fehlerseite an:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Um die Seite "Fehler" zu sehen, ändern Sie vorübergehend die `mode` Attribut der `customErrors` Element von "RemoteOnly", "Auf" und führen Sie die Anwendung aus Visual Studio. Ein Fehler verursacht, indem Sie eine ungültige URL wie z. B. anfordern *Studentsxxx.aspx*. Anstatt eine Fehlerseite für IIS-generierte "Seite nicht gefunden", Sie finden Sie unter den *GenericErrorPage.aspx* Seite.

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Um das Fehlerprotokoll anzuzeigen, ersetzen Sie alles in der URL nach der Portnummer *elmah.axd* (im Beispiel im Screenshot `http://localhost:51130/elmah.axd`), und drücken Sie die EINGABETASTE:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Vergessen Sie nicht, legen Sie die `customErrors` Element wieder in den Modus "RemoteOnly", wenn Sie fertig sind.

Auf dem Entwicklungscomputer ist es sinnvoll, kostenlosen Zugriff auf der Seite "Fehler", aber in der Produktion, die ein Sicherheitsrisiko dar. Für am Produktionsstandort, können Sie eine Autorisierungsregel, die Fehler Protokoll Zugriff nur auf Administratoren beschränkt werden, konfigurieren Sie eine Transformation im Hinzufügen der *Web.Production.config* Datei.

Open *Web.Production.config* und fügen Sie einen neuen `location` Element unmittelbar nach dem öffnenden `configuration` zu markieren, wie hier gezeigt. (Stellen Sie sicher, dass Sie nur hinzufügen, die `location` Element und nicht dem umgebenden Markup an, die hier gezeigt wird, nur um einige Kontext bereitzustellen.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

Die `Transform` -Attributwert "Insert" bewirkt, dass dieser `location` Element als ein gleichgeordnetes Element hinzugefügt werden, um alle vorhandenen `location` Elemente in der *"Web.config"* Datei. (Es ist bereits eine `location` -Element, das gibt an, Autorisierung, Regeln für die **Update Gutschriften** Seite.) Wenn Sie nach der Bereitstellung der Produktionswebsite testen, testen Sie, um sicherzustellen, dass diese Autorisierungsregel effektiv ist.

Sie müssen keine Fehler Protokoll den Zugriff beschränken, in der testumgebung, Sie haben diesen Code zum Hinzufügen der *Web.Test.config* Datei.

> [!NOTE] 
> 
> **Sicherheitshinweis** nie Fehlerdetails für die Öffentlichkeit in einer produktionsanwendung anzuzeigen oder diese Informationen in einem öffentlichen Speicherort speichern. Angreifer können Fehlerinformationen verwenden, um Schwachstellen in einem Standort zu ermitteln. Wenn Sie ELMAH in Ihrer eigenen Anwendung verwenden, achten Sie darauf, dass Sie Möglichkeiten untersuchen, in denen ELMAH konfiguriert werden kann, um Sicherheitsrisiken zu minimieren. Das ELMAH-Beispiel in diesem Tutorial sollte nicht die empfohlene Konfiguration betrachtet werden. Es ist ein Beispiel, das ausgewählt wurde, um zu veranschaulichen, wie Sie einen Ordner zu behandeln, dass die Anwendung Dateien erstellt werden muss.


## <a name="setting-an-environment-indicator"></a>Einen Indikator für die Umgebung festlegen

Ein häufiges Szenario ist, damit *"Web.config"* Einstellungen, die in jeder Umgebung unterschiedlich, die Sie zum Bereitstellen sein müssen der Datei. Beispielsweise kann eine Anwendung, die einen WCF-Dienst aufruft, einen anderen Endpunkt in Test-und produktionsumgebungen benötigen. Die Contoso University-Anwendung enthält eine Einstellung dieser Art auch. Diese Einstellung steuert die auf einer Website. einen Indikator angezeigten, der anzeigt, welcher Umgebung Sie, wie z. B. Entwicklung, Test oder Produktion sind. Der Wert der Einstellung bestimmt, ob die Anwendung "(Dev)" angefügt wird oder "(Test)", um die wichtigsten Überschrift in der *Site.Master* Masterseite:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

Der Indikator für die Umgebung wird weggelassen, wenn die Anwendung in der Produktion ausgeführt wird.

Die Contoso University-Webseiten Lesen eines Werts, der festgelegt wird, im `appSettings` in die *"Web.config"* Datei, um zu ermitteln, welche Umgebung in die Anwendung ausgeführt wird:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

Der Wert muss "Test" in der produktionsumgebung im Test-Umgebung und "Prod" sein.

Open *Web.Production.config* und Hinzufügen einer `appSettings` Element unmittelbar vor dem Starttag des der `location` Element, die Sie zuvor hinzugefügt haben:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

Die `xdt:Transform` Attributwert "SetAttributes" gibt an, dass es sich bei der diese Transformation dient zum Ändern der Attributwerte von vorhandenen Elements in der *"Web.config"* Datei. Die `xdt:Locator` Attributwert "Match(key)" gibt an, dass das Element, das geändert werden, die eine, deren `key` Attribut entspricht der `key` dieser Stelle angegebenen Attributs. Nur anderen Attributs die `add` Element `value`, und das ist was geändert werden, wird in der bereitgestellten *"Web.config"* Datei. Dieser Code bewirkt, dass die `value` Attribut der `Environment` `appSettings` Element, "Prod" festgelegt werden, der *"Web.config"* -Datei, die in der produktionsumgebung bereitgestellt wird.

Weisen Sie dann die gleiche Änderung *Web.Test.config* -Datei, mit Ausnahme der Menge der `value` "Test" anstelle von "Prod". Wenn Sie fertig sind, die `appSettings` im Abschnitt *Web.Test.config* sieht wie im folgenden Beispiel:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Deaktivieren des Debugmodus

Für einen Releasebuild möchten Sie nicht aktiviertem Debuggen unabhängig davon, welche, denen Umgebung Sie bereitstellen. Standardmäßig die *Datei "Web.Release.config"* Transform-Datei wird automatisch erstellt, mit Code, der entfernt die `debug` -Attribut aus dem `compilation` Element:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

Die `Transform` Attribut bewirkt, dass die `debug` Attribut weggelassen werden, aus der bereitgestellten *"Web.config"* Datei, wenn Sie einen Releasebuild bereitstellen.

Diese gleichen Transformation ist in Test- und Produktionsumgebungen Transformationsdateien an, da Sie sie erstellt haben, durch die Release-Transform-Datei kopieren. Benötigen Sie es doppelt vorhanden, sodass diese Dateien geöffnet, entfernen Sie die **Kompilierung** -Element, und speichern und schließen Sie jede Datei.

## <a name="setting-connection-strings"></a>Festlegen von Verbindungszeichenfolgen

In den meisten Fällen müssen Sie nicht mehr zum Einrichten der Verbindung Zeichenfolge Transformationen, da Sie die Verbindungszeichenfolgen im Veröffentlichungsprofil angeben können. Jedoch ist es eine Ausnahme aus, wenn Sie eine SQL Server Compact-Datenbank bereitstellen und verwenden Sie Entity Framework Code First-Migrationen zum Aktualisieren der Datenbank auf dem Zielserver. In diesem Szenario müssen Sie eine weitere Verbindungszeichenfolge angeben, die für die Aktualisierung des Datenbankschemas auf dem Server verwendet werden. Informationen zum Einrichten dieser Transformation fügen eine **&lt;ConnectionStrings&gt;** Element unmittelbar nach dem öffnenden **&lt;Konfiguration&gt;** Tag in beiden die *Web.Test.config* und *Web.Production.config* Dateien Transformieren:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

Die `Transform` Attribut gibt an, dass diese Verbindungszeichenfolge hinzugefügt werden die *ConnectionStrings* Element in der bereitgestellten *"Web.config"* Datei. (Der Veröffentlichungsprozess erstellt diese zusätzliche Verbindungszeichenfolge automatisch für Sie, wenn er nicht vorhanden, aber standardmäßig die **ProviderName** ruft-Attributsatz auf `System.Data.SqlClient`, die nicht für SQL Server Compact nicht funktionsfähig. Indem Sie die Verbindungszeichenfolge manuell hinzufügen, verhindern, dass Sie während des Bereitstellungsvorgangs ein Zeichenfolgenelement für die Verbindung mit dem falschen Anbieternamen erstellen.)

Sie haben jetzt alle angegeben die *"Web.config"* Transformationen, die Sie benötigen für die Bereitstellung der Contoso University-Anwendung zum Testen und die Produktion. Im folgenden Tutorial: Sie richten Bereitstellungsaufgaben kümmern, die erfordern, Festlegen von Projekteigenschaften.

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu den Themen in diesem Tutorial, finden Sie unter der Datei "Web.config" Transformation Szenario [ASP.NET die ASP.NET-Bereitstellung](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
