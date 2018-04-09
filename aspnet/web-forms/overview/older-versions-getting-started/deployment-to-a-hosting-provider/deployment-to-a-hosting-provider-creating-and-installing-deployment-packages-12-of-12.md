---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Problembehandlung (12 12) | Microsoft Docs'
author: tdykstra
description: Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual das...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 3fc23eed-921d-4d46-a610-a2d156e4bd03
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2a8342f026498a7cf3ff4a3c158ed177c15b7111
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-troubleshooting-12-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Problembehandlung (12 12)
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie das Web veröffentlichen Update installieren. Eine Einführung in der Reihe, finden Sie unter [im ersten Lernprogramm, in der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, das zeigt Bereitstellungsfeatures nach der RC-Version von Visual Studio 2012 eingeführt und wird gezeigt, wie zum Bereitstellen von SQL Server-Editionen als SQL Server Compact wird gezeigt, wie Windows Azure-Websites bereitstellen, finden Sie unter [ASP.NET Web Deploy Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


Diese Seite beschreibt einige allgemeine Probleme, die auftreten können, wenn Sie eine ASP.NET-Webanwendung mithilfe von Visual Studio bereitstellen. Für jede werden ein oder mehrere mögliche Ursachen und entsprechende Lösungen bereitgestellt.

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Serverfehler in '/' Anwendung - aktuellen Fehlereinstellungen für benutzerdefinierte verhindern, dass die Details des Fehlers Remote angezeigt wird

### <a name="scenario"></a>Szenario

Nach der Bereitstellung eines Standorts mit einem Remotehost, erhalten Sie eine Fehlermeldung, die die Einstellung "CustomErrors" in der Datei "Web.config" erwähnt, aber nicht an welche die eigentliche Ursache des Fehlers wurde:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample1.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Standardmäßig zeigt ASP.NET ausführliche Fehlerinformationen, nur, wenn Ihre Webanwendung auf dem lokalen Computer ausgeführt wird. Sie möchten in der Regel nicht ausführliche Fehlerinformationen angezeigt wird, wenn die Webanwendung über das Internet öffentlich verfügbar ist, da Hacker dieser Informationen können Sie Sicherheitsrisiken in der Anwendung gefunden werden können. Allerdings, wenn Sie eine Website oder Updates auf einer Website bereitstellen, manchmal etwas geht falsch und müssen Sie die tatsächliche Fehlermeldung abzurufen.

Zum Aktivieren der Anwendung ausführlichen Fehlermeldungen angezeigt, wenn sie auf dem Remotehost ausgeführt wird, bearbeiten Sie die Datei "Web.config" festzulegende `customErrors` Modus off, erneute Bereitstellung der Anwendung und die Anwendung erneut auszuführen:

1. Verfügt der Anwendungsdatei "Web.config" ein `customErrors` Element in der `system.web` Change-Element, das `mode` Attribut auf "off". Andernfalls fügen eine `customErrors` Element in der `system.web` Element mit der `mode` -Attribut auf "off" festgelegt ist, wie im folgenden Beispiel gezeigt:

    [!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample2.xml?highlight=3)]
2. Bereitstellen Sie die Anwendung.
3. Führen Sie die Anwendung, und wiederholen Sie, was Sie zuvor ausgeführt haben, die den Fehler verursacht hat. Jetzt können Sie sehen, was die eigentlichen Fehlermeldung ist.
4. Wenn Sie den Fehler behoben haben, Wiederherstellen die ursprünglichen `customErrors` festlegen und erneute Bereitstellung der Anwendung.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Auf einer Webseite verweigert wird, verwendet SQLServer Compact

### <a name="scenario"></a>Szenario

Beim Bereitstellen von eines Standorts, der SQL Server Compact verwendet, und führen Sie eine Seite in die bereitgestellte Website, die auf die Datenbank zugreift, wird die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Das NETZWERKDIENST-Konto auf dem Server muss in der Lage, SQL-Dienst Compact native-Binärdateien zu lesen, die in der *bin\amd64* oder *bin\x86* Ordner, jedoch nicht über Leserechte verfügt über Berechtigungen für diesen Ordner. Set read-Berechtigung für den Netzwerkdienst auf dem *"bin"* Ordner, und stellen Sie sicher, um die Berechtigungen zum Unterordner zu erweitern.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Die Konfigurationsdatei aufgrund unzureichender Berechtigungen kann nicht gelesen werden.

### <a name="scenario"></a>Szenario

Wenn Sie auf der Visual Studio Schaltfläche "Veröffentlichen" zum Bereitstellen einer Anwendung in IIS auf dem lokalen Computer veröffentlichen ein Fehler auftritt und die **Ausgabe** Fenster zeigt eine Fehlermeldung angezeigt, die etwa wie folgt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample4.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Mit nur einem Klick auf IIS auf dem lokalen Computer zu veröffentlichen, werden muss Visual Studio mit Administratorberechtigungen ausgeführt. Schließen Sie Visual Studio, und starten Sie es erneut mit Administratorberechtigungen.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Auf dem Zielcomputer konnte keine Verbindung hergestellt werden... Mithilfe des angegebenen Prozesses

### <a name="scenario"></a>Szenario

Wenn Sie auf der Visual Studio Schaltfläche "Veröffentlichen" zum Bereitstellen einer Anwendung veröffentlichen ein Fehler auftritt und die **Ausgabe** Fenster zeigt eine Fehlermeldung angezeigt, die etwa wie folgt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample5.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Ein Proxyserver wird die Kommunikation mit dem Zielserver unterbrechen. Wählen Sie in der Windows-Systemsteuerung oder im Internet Explorer **Internetoptionen** , und wählen Sie die **Verbindungen** Registerkarte. In der **Interneteigenschaften** (Dialogfeld), klicken Sie auf **LAN-Einstellungen**. In der **Local Area Network (LAN)-Einstellungen** deaktivieren Sie im Dialogfeld die **automatische Suche der Einstellungen** Kontrollkästchen. Klicken Sie dann auf die Schaltfläche "Veröffentlichen" erneut aus.

Wenn das Problem weiterhin besteht, wenden Sie sich an Ihren Systemadministrator, um zu bestimmen, was mit Proxy oder Firewall-Einstellungen erledigt werden kann. Das Problem auftritt, weil einen nicht standardmäßigen Port für die Bereitstellung der Webverwaltungsdienst (8172); Web Deploy verwendet werden. Bei anderen Verbindungen wird Port 80 verwendet, wenn Sie Web Deploy. Wenn Sie mit einem Drittanbieter-Hostinganbieter bereitstellen, verwenden Sie in der Regel der Webverwaltungsdienst.

## <a name="default-net-40-application-pool-does-not-exist"></a>.NET 4.0 Standardanwendungspool ist nicht vorhanden.

### <a name="scenario"></a>Szenario

Wenn Sie eine Anwendung, die .NET Framework 4 erforderlich sind bereitstellen, wird die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample6.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

ASP.NET 4 ist in IIS nicht installiert. Wenn der Server, dem Sie die Bereitstellung Ihrem Entwicklungscomputer ist und Visual Studio 2010 auf dem installiert ist, wird ASP.NET 4 ist auf dem Computer installiert, aber möglicherweise nicht in IIS installiert. Öffnen Sie auf dem Server, dem Sie bereitstellen ein Eingabeaufforderungsfenster mit erhöhten Rechten, und installieren Sie ASP.NET 4 in IIS, indem die folgenden Befehle ausführen:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample7.cmd)]

Möglicherweise müssen auch die .NET Framework-Version des Standardanwendungspools manuell festlegen. Weitere Informationen finden Sie unter der [Bereitstellung in IIS als Testumgebung](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) Lernprogramm.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Format der Initialisierungszeichenfolge stimmt nicht beginnend bei Index 0-Spezifikation überein.

### <a name="scenario"></a>Szenario

Nach der Bereitstellung einer Anwendung mit nur einem Klick veröffentlichen Sie, wenn ausführen, die eine Seite, die Zugriff auf die Datenbank Sie die folgende Fehlermeldung angezeigt erhalten:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample8.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Öffnen der *"Web.config"* -Datei in die bereitgestellte Website und das Kontrollkästchen, um festzustellen, ob die Werte der Verbindungszeichenfolgen mit beginnen `$(ReplacableToken_`, wie im folgenden Beispiel:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample9.xml)]

Wenn die Verbindungszeichenfolgen wie Folgendes Beispiel aussehen, bearbeiten Sie die Projektdatei, und fügen Sie die folgende Eigenschaft für die `PropertyGroup` Element, das für alle Konfigurationen zu erstellen:

[!code-xml[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample10.xml)]

Klicken Sie dann bereitstellen Sie die Anwendung erneut.

## <a name="http-500-internal-server-error"></a>HTTP 500 Interner Serverfehler

### <a name="scenario"></a>Szenario

Beim Ausführen der bereitgestellten Website sehen Sie die folgende Fehlermeldung angezeigt, ohne spezifische Angaben, der angibt, der Ursache des Fehlers:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample11.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Es gibt zahlreiche Ursachen von Fehlern 500 allerdings eine mögliche Ursache, wenn Sie diese Lernprogramme vorgehen handelt es sich, dass Sie ein XML-Element in der falschen Stelle in einem XML-Transformation Dateien einfügen. Beispielsweise würden Sie diesen Fehler erhalten, wenn Sie die Transformation einfügen, das eingefügt eine `<location>` Element unter `<system.web>` statt direkt unter `<configuration>`. Die Lösung besteht in diesem Fall die XML-Transformation-Datei zu korrigieren und erneut bereitstellen.

## <a name="http-50021-internal-server-error"></a>HTTP-500.21 Interner Serverfehler

### <a name="scenario"></a>Szenario

Wenn Sie den bereitgestellten Standort ausführen, sehen Sie die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample12.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Die Website wurde bereitgestellt, Ziele, die ASP.NET 4, aber ASP.NET 4 ist nicht in IIS auf dem Server registriert. Klicken Sie auf dem Server öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten, und registrieren Sie ASP.NET 4, indem Sie die folgenden Befehle ausführen:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample13.cmd)]

Möglicherweise müssen auch die .NET Framework-Version des Standardanwendungspools manuell festlegen. Weitere Informationen finden Sie unter der [Bereitstellung in IIS als Testumgebung](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) Lernprogramm.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Fehler bei der Anmeldung öffnende SQL Server Express-Datenbank in-App\_Daten

### <a name="scenario"></a>Szenario

Sie auf dem laufenden der *"Web.config"* Verbindungszeichenfolge für die SQL Server Express-Datenbank als Datei ein *mdf* Datei Ihre *App\_Daten* Ordner und die erste Ausführung der Anwendung, die Sie sehen die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample14.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Der Name des der *mdf* Datei kann nicht mit dem Namen einer beliebigen SQL Server Express-Datenbank, die jemals auf Ihrem Computer vorhanden ist übereinstimmen, auch wenn Sie gelöscht der *mdf* Datei bereits vorhandene Datenbank. Ändern Sie den Namen des der *mdf* Datei in einen Namen, die nie als Datenbanknamen und Änderung verwendet wurde die *"Web.config"* zu den neuen Namen zu verwendende Datei an. Als Alternative können Sie [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) gelöscht zuvor vorhandenen SQL Server Express-Datenbanken.

## <a name="model-compatibility-cannot-be-checked"></a>Modell Kompatibilität können nicht überprüft werden

### <a name="scenario"></a>Szenario

Sie auf dem laufenden der *"Web.config"* Datei die Verbindungszeichenfolge so, dass in einer neuen SQL Server Express-Datenbank verweist, und beim ersten die Anwendung ausführen die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample15.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Wenn der Name der Datenbank, die Sie in der Datei "Web.config" gesteckt je verwendet wurde, bevor auf Ihrem Computer eine Datenbank mit einigen Tabellen in der sie möglicherweise schon vorhanden sind. Wählen Sie einen neuen Namen, die auf Ihrem Computer vor und die Änderung nicht verwendet wurde die *"Web.config"* Datei, zeigen Sie auf diesem neuen Datenbanknamen verwenden. Als Alternative können Sie [SQL Server Express-Hilfsprogramm](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) oder [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) beim Löschen der vorhandenen Datenbank.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>SQL-Fehler, wenn ein Skript versucht, das Erstellen von Benutzern oder Rollen

### <a name="scenario"></a>Szenario

Verwenden Sie datenbankbereitstellung konfiguriert werden, auf die **SQL packen/veröffentlichen** Registerkarte SQL-Skripts, die während der Bereitstellung ausgeführt umfassen Create User oder Create Role-Befehle und Skript erzeugt die Ausführung dieser Befehle ausgeführt werden. Möglicherweise ausführlichere Meldungen, wie die folgende angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample16.cmd)]

Wenn dieser Fehler tritt auf, wenn Sie in die Bereitstellung konfiguriert haben die **Web veröffentlichen** Assistenten statt über das **SQL packen/veröffentlichen** Registerkarte, erstellen Sie einen Thread in der [Konfiguration und Bereitstellung](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) Forum und die Lösung wird diese Seite "Problembehandlung" hinzugefügt werden.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Berechtigung zum Erstellen von Benutzern bzw. Rollen keine für das Benutzerkonto, das Sie verwenden, um die Bereitstellung durchzuführen. Hostinganbieter kann z. B. Zuweisen der `db_datareader`, `db_datawriter`, und `db_ddladmin` Rollen mit dem Benutzerkonto, mit dem es für Sie eingerichtet. Diese reichen für die meisten Datenbankobjekte erstellen, aber nicht für Benutzer oder Rollen erstellen. Eine Möglichkeit, den Fehler zu vermeiden, wird durch das Ausschließen von Benutzern und Rollen von datenbankbereitstellung. Hierzu können Sie durch Bearbeiten der `PreSource` -Element für die Datenbank automatisch generierte Skript so an, dass sie die folgenden Attribute enthalten:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample17.cmd)]

Informationen zur Bearbeitung der `PreSource` Element in der Projektdatei finden Sie unter [Vorgehensweise: Bearbeiten von Bereitstellungseinstellungen in der Projektdatei](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Wenn die Benutzer oder Rollen in der Entwicklungsdatenbank in der Zieldatenbank werden müssen, wenden Sie sich an Ihrem Hostinganbieter, um Unterstützung zu erhalten.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>SQL Server-Timeoutfehler beim Ausführen von benutzerdefinierten Skripts während der Bereitstellung

### <a name="scenario"></a>Szenario

Sie während der Bereitstellung auszuführenden benutzerdefinierten SQL-Skripts angegeben haben, und wenn sie Web Deploy ausgeführt wurde, sie Timeout eintritt.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Mehrere Skripts mit verschiedenen Transaktionsmodi ausgeführt, kann zu Timeout-Fehlern führen. Standardmäßig automatisch generierten Skripts, die in einer Transaktion ausgeführt, aber benutzerdefinierte Skripts nicht der Fall. Bei Auswahl der **Daten und/oder Schema aus einer vorhandenen Datenbank mithilfe von Pull** option die **SQL packen/veröffentlichen** Registerkarte, wenn Sie ein benutzerdefinierte SQL-Skript hinzufügen, müssen Sie transaktionseinstellungen für einige Skripts ändern, damit Alle Skripts verwenden die gleichen transaktionseinstellungen. Weitere Informationen finden Sie unter [Vorgehensweise: Bereitstellen einer Datenbank mit einem Webanwendungsprojekt](https://msdn.microsoft.com/library/dd465343.aspx).

Wenn Sie transaktionseinstellungen konfiguriert haben, sodass alle identisch sind, aber diese Fehlermeldung weiterhin erhalten, ist eine mögliche problemumgehung die Skripts separat ausführen. In der **Datenbankskripts** Raster in die **packen/veröffentlichen** Registerkarte "SQL", Deaktivieren der **Include** Kontrollkästchen für das Skript, das den Timeoutfehler verursacht hat, dann das Projekt veröffentlichen. Kehren Sie zurück in die **Datenbankskripts** Raster, wählen Sie dieses Skripts **Include** Kontrollkästchen, und Deaktivieren der **Include** Kontrollkästchen für die anderen Skripts. Klicken Sie dann veröffentlichen Sie das Projekt erneut. Dieses Mal beim Veröffentlichen, wird nur die ausgewählten benutzerdefinierten Skripts ausgeführt.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Streamen von Daten von Website-Manifest ist noch nicht verfügbar

### <a name="scenario"></a>Szenario

Beim Installieren Sie ein Paket mit der *deploy.cmd* -Datei mit der `t` (Test)-Option, die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample18.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Die Fehlermeldung bedeutet, dass der Befehl einen Testbericht erzeugen kann. Der Befehl möglicherweise jedoch ausgeführt, wenn Sie mithilfe der `y` (tatsächliche Installationsoption). Die Meldung gibt nur an, dass ein Problem mit dem Befehl im Testmodus vorliegt.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Diese Anwendung erfordert ManagedRuntimeVersion v4. 0

### <a name="scenario"></a>Szenario

Wenn Sie versuchen, bereitstellen, wird die folgende Fehlermeldung angezeigt:

 Fehler: Die Streamdaten von "Sitemanifest/DbFullSql [@path="C:\TEMP\AdventureWorksGrant.sql']/sqlScript"ist noch nicht verfügbar. Der Anwendungspool, den Sie verwenden möchten hat die 'ManagedRuntimeVersion'-Eigenschaft auf "v2. 0" festgelegt. Diese Anwendung erfordert "v4. 0". 

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

ASP.NET 4 ist in IIS nicht installiert. Wenn der Server, dem Sie die Bereitstellung Ihrem Entwicklungscomputer ist und Visual Studio 2010 auf dem installiert ist, wird ASP.NET 4 ist auf dem Computer installiert, aber möglicherweise nicht in IIS installiert. Öffnen Sie auf dem Server, dem Sie bereitstellen ein Eingabeaufforderungsfenster mit erhöhten Rechten, und installieren Sie ASP.NET 4 in IIS, indem die folgenden Befehle ausführen:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample19.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Microsoft.Web.Deployment.DeploymentProviderOptions umgewandelt

### <a name="scenario"></a>Szenario

Wenn Sie ein Paket bereitstellen, wird die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample20.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Sie versuchen, das zum Bereitstellen von IIS-Manager über die Bereitstellung 1.1 Benutzeroberfläche auf einem Server mit Web Deploy 2.0 installiert. Wenn Sie das IIS-Verwaltungstool Remote bereitstellen, indem ein Kontrollkästchen-Paket importieren verwenden die **neue Features verfügbar** (Dialogfeld), wenn Sie die Verbindung herstellen. (Dieses Dialogfeld kann nur einmal angezeigt, wenn die Verbindung erstmalig eingerichtet wird. Um die Verbindung deaktivieren und erneut zu beginnen, IIS-Manager zu schließen und erneut gestartet werden Sie durch Eingabe `inetmgr /reset` an der Eingabeaufforderung.) Wenn eine der Funktionen aufgeführt ist **Webbenutzeroberfläche bereitstellen**, und es wurde eine Versionsnummer niedriger als 8 ist, müssen Sie die Bereitstellung für Server möglicherweise 1.1 und 2.0-Versionen von Web Deploy installiert. Um von einem Client bereitstellen, die 2.0 installiert ist, muss der Server nur Web Deploy 2.0 installiert haben. Sie müssen Ihrem Hostinganbieter zum Beheben dieses Problems zu erhalten.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Kann nicht geladen werden die systemeigenen Komponenten von SQL Server Compact

### <a name="scenario"></a>Szenario

Wenn Sie den bereitgestellten Standort ausführen, sehen Sie die folgende Fehlermeldung angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample21.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Der bereitgestellte Standort verfügt nicht über *amd64* und *X86* Unterordner mit der systemeigenen Assemblys werden unter der Anwendungsverzeichnis *"bin"* Ordner. Auf einem Computer, SQL Server Compact installiert ist, befinden sich die systemeigenen Assemblys im *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. Die beste Möglichkeit, erhalten die richtigen Dateien in den richtigen Ordnern in einem Visual Studio-Projekt ist das NuGet SqlServerCompact-Paket installieren. Paketinstallation Fügt eine Postbuild-Skript aus, um die systemeigenen Assemblys in kopieren *amd64* und *X86*. Damit für diese bereitgestellt werden können müssen Sie jedoch manuell in das Projekt aufgenommen. Weitere Informationen finden Sie unter der [Bereitstellen von SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) Lernprogramm.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>"Pfad ist ungültig" Fehler nach dem Bereitstellen einer Entity Framework Code First-Anwendung

### <a name="scenario"></a>Szenario

Bereitstellung einer Anwendung, die Entity Framework Code First-Migrationen und einem DBMS z. B. SQL Server Compact verwendet wird, das die Datenbank in einer Datei in die App speichert\_Datenordner. Sie müssen Code First-Migrationen so konfiguriert, dass um die Datenbank nach der ersten Bereitstellung zu erstellen. Wenn Sie die Anwendung ausführen, erhalten Sie eine Fehlermeldung wie im folgenden Beispiel:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample22.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Code wird zuerst die Datenbank, aber die App zu erstellen versucht\_Datenordner ist nicht vorhanden. Entweder haben nicht Sie alle Dateien haben, in der *App\_Daten* Ordner, wenn Sie bereitgestellt haben, oder Sie ausgewählt **ausschließen App\_Daten** auf die **Web packen/veröffentlichen** auf der Registerkarte die **Projekteigenschaften** Fenster. Der Bereitstellungsprozess wird keinen Ordner auf dem Server erstellen, wenn es sind keine Dateien im Ordner "" an den Server kopiert werden soll. Bereits die Datenbank am Standort eingerichtet haben, wird der Bereitstellungsprozess die Dateien löschen und die *App\_Daten* Ordner selbst bei Auswahl **entfernen weiterer Dateien am Ziel** in Das Veröffentlichungsprofil. Um das Problem zu beheben, speichern Sie eine Platzhalterdatei, z. B. einer TXT-Datei in die *App\_Daten* Ordner stellen Sie sicher, dass Sie keine **ausschließen App\_Daten** ausgewählt, und stellen Sie erneut bereit. 

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"COM-Objekt, das vom zugrunde liegenden RCW getrennt wurde kann nicht verwendet werden."

### <a name="scenario"></a>Szenario

Sie wurden erfolgreich mit nur einem Klick zum Bereitstellen der Anwendung zu veröffentlichen und starten Sie dann Sie dieser Fehler angezeigt:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample23.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Schließen und Neustart von Visual Studio ist in der Regel alle, die erforderlich ist, um diesen Fehler zu beheben.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Bereitstellung schlägt fehl, da Benutzer Anmeldeinformationen verwendet für die Publishing haben keine SetACL Autorität

### <a name="scenario"></a>Szenario

Publishing schlägt fehl mit Fehler, der angibt berechtigt nicht, Berechtigungen für Ordner festlegen (das Benutzerkonto, das Sie verwenden keinen SetACL Autorität).

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Standardmäßig legt Visual Studio Leseberechtigungen für den Stammordner des Standorts und Schreibberechtigungen für die App\_Datenordner. Wenn Sie wissen, dass die Standardberechtigungen für den Standortordner auf Ihre Richtigkeit, und müssen nicht festgelegt werden, deaktivieren Sie dieses Verhalten durch Hinzufügen von **&lt;IncludeSetACLProviderOn Ziel&gt;"false"&lt;/ IncludeSetACLProviderOnDestination&gt;** die veröffentlichungsprofildatei (auf einem einzigen Profil wirkt sich auf) oder die Datei wpp.targets (um die Auswirkung auf die von allen Profilen). Informationen zur Bearbeitung dieser Dateien finden Sie unter [Vorgehensweise: Bearbeiten von Bereitstellungseinstellungen in Profildateien (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). 

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>"Zugriff verweigert" Fehler, wenn die Anwendung versucht, in einen Anwendungsordner zu schreiben

### <a name="scenario"></a>Szenario

Ihre Anwendungsfehler beim Erstellen oder Bearbeiten einer Datei in einem Anwendungsordner versucht, da sie nicht über die Autorität für Schreibzugriff für diesen Ordner verfügt.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Standardmäßig legt Visual Studio Leseberechtigungen für den Stammordner des Standorts und Schreibberechtigungen für die App\_Datenordner. Wenn Ihre Anwendung Schreibzugriff auf ein Unterordner benötigt, können Sie Berechtigungen für diesen Ordner festlegen, entsprechend der [Einstellung Ordnerberechtigungen](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md) und [in der Produktionsumgebung bereitstellen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Lernprogramme. Wenn Ihre Anwendung Schreibzugriff auf den Stammordner der Website benötigt, müssen Sie verhindern, dass nur-Lese-Zugriff auf den Stammordner festlegen, durch Hinzufügen von **&lt;IncludeSetACLProviderOn Ziel&gt;"false"&lt;/ IncludeSetACLProviderOnDestination&gt;** die veröffentlichungsprofildatei (auf einem einzigen Profil wirkt sich auf) oder die Datei wpp.targets (um die Auswirkung auf die von allen Profilen). Informationen zur Bearbeitung dieser Dateien finden Sie unter [Vorgehensweise: Bearbeiten von Bereitstellungseinstellungen in Profildateien (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx). <a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Fehler bei der Konfiguration - TargetFramework-Attributs verweist auf eine Version, die höher als die installierte Version von .NET Framework ist

### <a name="scenario"></a>Szenario

Sie wurden erfolgreich veröffentlicht ein Webprojekt, die ASP.NET 4.5 ausgerichtet ist, aber beim Ausführen der Anwendung (mit der `customErrors` -Modus auf "in der Datei" Web.config "deaktiviert" festgelegt) erhalten Sie die folgende Fehlermeldung:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample24.cmd)]

Das Feld Quellfehler die Fehlerseite nennt die folgende Zeile aus der Datei "Web.config" als die Ursache des Fehlers:

[!code-console[Main](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12/samples/sample25.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Der Server unterstützt nicht ASP.NET 4.5. Wenden Sie sich an den Hostinganbieter, um zu bestimmen, wann und ob Unterstützung für ASP.NET 4.5 hinzugefügt werden kann. Wenn Sie den Server ein Upgrade nicht möglich ist, müssen Sie ein Webprojekt bereitstellen, die ASP.NET 4 oder früher ausgerichtet ist stattdessen. Wenn Sie eine ASP.NET 4 oder früher Webprojekt mit dem gleichen Ziel bereitstellen, wählen Sie die **entfernen weiterer Dateien am Ziel** Kontrollkästchen auf der **Einstellungen** auf der Registerkarte die **"Web veröffentlichen"**Assistenten. Wenn Sie nicht auswählen **entfernen weiterer Dateien am Ziel**, werden Sie weiterhin erhalten, die Seite "Fehler bei der Konfiguration".

Das Projekt **Eigenschaften** Windows enthält die Dropdownliste für ein Ziel-Framework, aber Sie können nicht das Problem beheben, indem Sie nur ändern, die von **.NET Framework 4.5** auf **.NET Framework 4**. Wenn Sie das Zielframework in einer früheren Frameworkversion ändern, wird das Projekt verfügen weiterhin über Verweise auf die neuere Frameworkversion-Assemblys und wird nicht ausgeführt. Sie müssen manuell die Verweise ändern oder Erstellen eines neuen Projekts, das .NET Framework 4 oder früher ausgerichtet ist. Weitere Informationen finden Sie unter [.NET Framework als Ziel für Websites](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Vorherige](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
