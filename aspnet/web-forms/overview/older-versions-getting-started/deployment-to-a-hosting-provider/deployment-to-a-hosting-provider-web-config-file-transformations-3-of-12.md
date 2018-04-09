---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: "Web.config" Dateitransformationen - 3 12 | Microsoft Docs'
author: tdykstra
description: Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual das...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: 86eb74ca35e8804978127412e2276eeee9d615dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: "Web.config" Dateitransformationen - 3 von 12
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie das Web veröffentlichen Update installieren. Eine Einführung in der Reihe, finden Sie unter [im ersten Lernprogramm, in der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, die die Bereitstellungsfeatures nach der RC-Version von Visual Studio 2012 eingeführt wurde, wird gezeigt, wie SQL Server-Editionen als SQL Server Compact bereitstellen, und zeigt die Vorgehensweise beim Bereitstellen in Azure App Service-Web-Apps, finden Sie unter [ASP.NET Web Deploy Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

Dieses Lernprogramm veranschaulicht das Automatisieren einer Änderung der *"Web.config"* Datei, wenn Sie unterschiedliche zielumgebungen bereitstellen. Die meisten Anwendungen gelten die Einstellungen der *"Web.config"* -Datei, die verschiedene sein muss, wenn die Anwendung bereitgestellt wird. Automatisieren des Prozesses, der diesen Änderungen verhindert, dass sie manuell jedes Mal, wenn Sie bereitstellen, die wäre mühsam und fehleranfällig ausgeführt werden müssen.

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>"Web.config" Transformationen im Vergleich zu Web bereitstellen, Parameter

Es gibt zwei Möglichkeiten zum Automatisieren von veränderlichen *"Web.config"* Settings-Datei: ["Web.config" Transformationen](https://msdn.microsoft.com/library/dd465326.aspx) und [Web Deploy-Parameter](https://msdn.microsoft.com/library/ff398068.aspx). Ein *"Web.config"* Transformationsdatei enthält XML-Markup, der angibt, wie Sie ändern die *"Web.config"* Datei bei der Bereitstellung. Sie können die verschiedene Änderungen für bestimmte Buildkonfigurationen und für bestimmte Veröffentlichungsprofile angeben. Die standardmäßige Buildkonfigurationen werden Debug- und, und Sie können benutzerdefinierte Buildkonfigurationen erstellen. Ein Veröffentlichungsprofil entspricht in der Regel eine zielumgebung. (Erfahren Sie mehr über Veröffentlichungsprofile in der [Bereitstellung in IIS als Testumgebung](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) Lernprogramm.)

Web Deploy-Parameter können verwendet werden, können viele verschiedene Arten von Einstellungen angeben, die konfiguriert werden müssen, während der Bereitstellung, einschließlich der Einstellungen, die im gefunden *"Web.config"* Dateien. Wenn zur Angabe *"Web.config"* Datei ändert, Web Deploy-Parameter sind komplexer eingerichtet, aber sie sind hilfreich, wenn Sie den Wert festgelegt werden, bevor Sie bereitstellen, nicht kennen. Z. B. in einer unternehmensumgebung möglicherweise erstellen Sie eine *Bereitstellungspaket* und weisen Sie ihm eine Person in der IT-Abteilung in einer produktionsumgebung installieren und diese Person hat in der Lage, geben Verbindungszeichenfolgen oder Kennwörter, die Sie nicht kennen.

Für das Szenario, die in diesem Lernprogramm abdeckt, wissen Sie alle Elemente, die ausgeführt werden, zu der *"Web.config"* Datei, sodass Sie nicht benötigen, verwenden Sie Web Deploy-Parameter. Sie werden einige Transformationen, die unterscheiden sich abhängig von der Buildkonfiguration verwendet, und einige, unterscheiden sich je nach verwendete Veröffentlichungsprofil, konfigurieren.

## <a name="creating-transformation-files-for-publish-profiles"></a>Erstellen von Transformationsdateien für Veröffentlichungsprofile

In **Projektmappen-Explorer**, erweitern Sie *"Web.config"* , finden Sie unter der *"Web.Debug.config"* und *Web.Release.config* Transformation-Dateien mit werden standardmäßig für die zwei standardmäßigen Projektmappenbuild-Konfigurationen erstellt werden.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Sie können Transformationsdateien für benutzerdefinierte Buildkonfigurationen erstellen, indem Sie mit der rechten Maustaste in der Datei "Web.config" und **hinzufügen Config transformiert** aus dem Kontextmenü, aber für dieses Lernprogramm müssen Sie nicht nachholen.

Sie benötigen mindestens zwei Transformationsdateien, zum Konfigurieren von Änderungen, die an das Bereitstellungsziel statt an die Buildkonfiguration verknüpft sind. Ein typisches Beispiel für diese Art der Einstellung ist ein WCF-Endpunkt, der für Tests und produktionsumgebung unterschiedlich ist. Veröffentlichen Sie in späteren Lernprogrammen, Sie erstellen, Profile mit dem Namen Test- und Produktionsumgebungen, daher Sie müssen eine *Web.Test.config* Datei und ein *Web.Production.config* Datei.

Transformation für Dateien, die zum Veröffentlichen von Profilen gebunden sind, müssen manuell erstellt werden. In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und wählen Sie **Ordner in Windows Explorer öffnen**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

In **Windows­Explorer**, wählen die *Web.Release.config* Datei, kopieren Sie die Datei, und fügen Sie zwei Kopien. Benennen Sie diese Kopien *Web.Production.config* und *Web.Test.config*, schließen Sie dann **Windows Explorer**.

In **Projektmappen-Explorer**, klicken Sie auf **aktualisieren** um die neuen Dateien anzuzeigen.

Wählen Sie die neuen Dateien, mit der rechten Maustaste, und klicken Sie dann auf **Projekt** im Kontextmenü.

![Einschließlich der Test- und Config-Dateien im Projekt](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Um zu verhindern, dass diese Dateien bereitgestellt wird, wählen sie im **Projektmappen-Explorer**, und klicken Sie dann in der **Eigenschaften** Ändern der **Buildvorgang** Eigenschaft von **Content** auf **keine**. (Die Transformationsdateien, die abhängig von Buildkonfigurationen werden automatisch aus der Bereitstellung verhindert.)

Sie können nun zur Eingabe *"Web.config"* Transformationen in der *"Web.config"* Transformationsdateien.

## <a name="limiting-error-log-access-to-administrators"></a>Fehler beim Protokoll Zugriff auf Administratoren beschränken

Wenn ein Fehler aufgetreten ist, während die Anwendung ausgeführt wird und die Anwendung eine generische Fehlerseite anstelle der vom System generierte Fehlerseite zeigt verwendet [Elmah NuGet-Paket](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) für Fehler, Protokollierung und-berichterstellung. Die `customErrors` Element in der *"Web.config"* Datei gibt die Fehlerseite an:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Um die Seite "Fehler" angezeigt wird, ändern Sie vorübergehend die `mode` Attribut des der `customErrors` Element aus "RemoteOnly" auf "On" und führen Sie die Anwendung aus Visual Studio. Verursacht einen Fehler durch eine ungültige URL, z. B. anfordern *Studentsxxx.aspx*. Anstelle einer Fehlerseite IIS generiert "Seite nicht gefunden" angezeigt der *GenericErrorPage.aspx* Seite.

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Um das Fehlerprotokoll angezeigt wird, ersetzen Sie alles in der URL die Portnummer mit *elmah.axd* (für das Beispiel in dem Screenshot `http://localhost:51130/elmah.axd`), und drücken Sie die EINGABETASTE:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Vergessen Sie nicht, legen Sie die `customErrors` Element wieder in den Modus "RemoteOnly", wenn Sie fertig sind.

Auf dem Entwicklungscomputer empfiehlt es sich um kostenlosen Zugriff auf die Fehlerseite-Protokoll, aber in der Produktion, die ein Sicherheitsrisiko zu ermöglichen. Für die Produktionsstandort können Sie eine Autorisierungsregel, die Fehler Protokoll Zugriff nur für Administratoren zu beschränken, durch Konfigurieren einer Transformation im Hinzufügen der *Web.Production.config* Datei.

Open *Web.Production.config* und fügen Sie einen neuen `location` -Element unmittelbar nach dem Öffnen `configuration` kennzeichnen, wie hier gezeigt. (Stellen Sie sicher, dass Sie nur hinzufügen, die `location` Element- und nicht dem umgebenden Markup der hier lediglich zur Bereitstellung von einigen Kontext angezeigt wird.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

Die `Transform` Attribut-Wert von "Insert" bewirkt, dass `location` Element als ein gleichgeordnetes Element hinzugefügt werden, um alle vorhandenen `location` Elemente in der *"Web.config"* Datei. (Es ist bereits eine `location` Element, der angibt, Autorisierung von Regeln für die **Update Gutschriften** Seite.) Beim Testen des Produktionsstandorts nach der Bereitstellung werden Sie testen, um sicherzustellen, dass diese Autorisierungsregel effektiv ist.

Sie keine Fehler den Zugriff auf Ereignisprotokoll in der testumgebung zu beschränken, daher ist es nicht diesen Code zum Hinzufügen der *Web.Test.config* Datei.

> [!NOTE] 
> 
> **Sicherheitshinweis** nie Fehlerdetails an die Öffentlichkeit in einer produktionsanwendung anzuzeigen oder diese Informationen in einem öffentlichen Speicherort speichern. Fehlerinformationen können Angreifer um Schwachstellen in einem Standort zu ermitteln. Wenn Sie in Ihrer eigenen Anwendung ELMAH verwenden, achten Sie darauf, dass Sie Möglichkeiten untersuchen, in denen ELMAH konfiguriert werden kann, um Sicherheitsrisiken zu minimieren. Das ELMAH-Beispiel in diesem Lernprogramm sollte nicht die empfohlene Konfiguration verwendet werden. Es ist ein Beispiel, das ausgewählt wurde, um zu veranschaulichen, wie einen Ordner behandeln zu können, die Anwendung auf die Dateien erstellt werden muss.


## <a name="setting-an-environment-indicator"></a>Festlegen eines Indikators Umgebung

Ein häufiges Szenario ist, damit *"Web.config"* Datei Einstellungen, die in jeder Umgebung unterschiedlich sein müssen, das Sie bereitstellen. Z. B. möglicherweise eine Anwendung, die einen WCF-Dienst aufruft, einen anderen Endpunkt in Test-und produktionsumgebungen. Die Anwendung von Contoso University enthält auch eine Einstellung dieser Art. Diese Einstellung steuert auf den Seiten einer Website einen Indikator angezeigten, der Sie der Umgebung Sie informiert, wie z. B. Entwicklung, Test oder Produktion sind. Der Wert der Einstellung bestimmt, ob die Anwendung "(Dev)" angefügt werden, oder "(Test)", um die wichtigsten Überschrift in der *Site.Master* Gestaltungsvorlage:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

Der Indikator für die Umgebung wird ausgelassen, wenn die Anwendung in der produktionsumgebung ausgeführt wird.

Contoso University Web Pages Lesen eines Werts, der festgelegt wird, in `appSettings` in der *"Web.config"* Datei, um zu bestimmen, welche Umgebung in die Anwendung ausgeführt wird:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

Der Wert muss "Test" in der testumgebung, und klicken Sie auf "Prod" in der produktionsumgebung.

Öffnen *Web.Production.config* und Hinzufügen einer `appSettings` Element unmittelbar vor der öffnenden Tags von der `location` Element, die Sie zuvor hinzugefügt haben:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

Die `xdt:Transform` -Attributwert "SetAttributes" gibt an, dass der Zweck dieser Transformation so ändern Sie die Attributwerte eines vorhandenen Elements in der *"Web.config"* Datei. Die `xdt:Locator` -Attributwert "Match(key)" gibt an, dass das Element geändert werden, die dem Zertifikat entspricht, dessen `key` Attribut entspricht der `key` hier angegebenen Attributs. Nur anderen Attributs der `add` Element ist `value`, und was geändert werden, wird in der bereitgestellten *"Web.config"* Datei. Dieser Code bewirkt, dass die `value` Attribut der `Environment` `appSettings` Element auf "Prod" festgelegt werden, der *"Web.config"* -Datei, die in der produktionsumgebung bereitgestellt wird.

Anschließend weisen die gleiche Änderung *Web.Test.config* -Datei, mit Ausnahme der Satz der `value` "Test" anstelle von "Prod". Wenn Sie fertig sind, die `appSettings` im Abschnitt *Web.Test.config* wird im folgenden Beispiel aussehen:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Deaktivieren den Debugmodus

Für einen Releasebuild sollten Sie keine Debugfunktion unabhängig davon, welcher, denen Umgebung bereitgestellt werden. Standardmäßig die *Web.Release.config* Transform-Datei wird automatisch erstellt, durch Code, der entfernt die `debug` -Attribut aus dem `compilation` Element:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

Die `Transform` -Attribut Ursachen der `debug` Attribut weggelassen werden, aus der bereitgestellten *"Web.config"* Datei, wenn Sie einen Releasebuild bereitstellen.

Diese dieselbe Transformation ist in Test- und Produktionsumgebungen Transformationsdateien an, da Sie sie erstellt haben, kopieren Sie die Transformationsdatei Version. Ist dies die dupliziert es nicht erforderlich, das Öffnen aller Dateien, entfernen Sie die **Kompilierung** -Element, und speichern und schließen Sie jede Datei.

## <a name="setting-connection-strings"></a>Festlegen von Verbindungszeichenfolgen

In den meisten Fällen benötigen Sie keine Verbindung Zeichenfolge Transformationen, einrichten, da Sie Verbindungszeichenfolgen im Veröffentlichungsprofil angeben können. Jedoch ist es eine Ausnahme aus, wenn Sie eine SQL Server Compact-Datenbank bereitstellen, und Sie zum Aktualisieren der Datenbank auf dem Zielserver Entity Framework Code First-Migrationen verwenden. In diesem Szenario müssen Sie eine zusätzliche Verbindungszeichenfolge angeben, die auf dem Server verwendet werden soll, für das Datenbankschema aktualisieren. Informationen zum Einrichten dieser Transformation fügen eine **&lt;ConnectionStrings&gt;** -Element unmittelbar nach dem Öffnen **&lt;Konfiguration&gt;** Tag in beiden die *Web.Test.config* und *Web.Production.config* Dateien Transformieren:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

Die `Transform` Attribut gibt an, dass diese Verbindungszeichenfolge hinzugefügt werden die *ConnectionStrings* Element in der bereitgestellten *"Web.config"* Datei. (Des Veröffentlichungsprozesses erstellt diese zusätzliche Verbindungszeichenfolge automatisch, wenn er nicht vorhanden ist, aber standardmäßig die **ProviderName** ruft-Attributsatz zur `System.Data.SqlClient`, die nicht für SQL Server Compact nicht funktioniert. Indem Sie die Verbindungszeichenfolge manuell hinzufügen, verhindert, dass Sie den Bereitstellungsprozess erstellen eine Zeichenfolge Verbindungselement mit dem falschen Anbieternamen.)

Sie haben jetzt alle angegeben die *"Web.config"* Transformationen, die Sie benötigen für die Bereitstellung der Universität von Contoso-Anwendung zum Testen und Produktion. Im folgenden Lernprogramm Sie müssen der Einrichtung Bereitstellungsaufgaben darauf achten, die erfordern, Festlegen von Projekteigenschaften.

## <a name="more-information"></a>Weitere Informationen

Weitere Informationen zu den von diesem Lernprogramm behandelten Themen finden Sie unter "Web.config" Transformation für Szenario in [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
