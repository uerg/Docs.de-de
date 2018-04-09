---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 'ASP.NET Web-Bereitstellung mit Visual Studio: Problembehandlung | Microsoft Docs'
author: tdykstra
description: Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder mit einem Hostinganbieter von Drittanbietern durch wählen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 15bda09c59afaf9e5449c68c5206bb28de245541
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>ASP.NET Web-Bereitstellung mit Visual Studio: Problembehandlung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung eines Drittanbieters Hostinganbieter oder Azure App Service-Web-Apps mithilfe von Visual Studio 2012 oder Visual Studio 2010. Informationen über die Reihen finden Sie unter [im ersten Lernprogramm, in der Reihe](introduction.md).


Diese Seite beschreibt einige allgemeine Probleme, die auftreten können, wenn Sie eine ASP.NET-Webanwendung mithilfe von Visual Studio bereitstellen. Für jede werden ein oder mehrere mögliche Ursachen und entsprechende Lösungen bereitgestellt.

Die Szenarien gezeigt gelten für Azure und Drittanbieter-Hostinganbieter. Weitere Informationen zur Problembehandlung in Azure App Service-Web-apps finden Sie unter den folgenden Ressourcen:

- [Problembehandlung bei einer Web-app in Azure App Service mithilfe von Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [Überwachen von Web-Apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- [Die Version von Windows Azure SDK 2.0 für .NET Ankündigung](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (Scottgus Blog verwenden, zeigt, wie beim Abrufen der Diagnoseprotokolle in Visual Studio)

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Serverfehler in '/' Anwendung - aktuellen Fehlereinstellungen für benutzerdefinierte verhindern, dass die Details des Fehlers Remote angezeigt wird

### <a name="scenario"></a>Szenario

Nach der Bereitstellung eines Standorts mit einem Remotehost, erhalten Sie eine Fehlermeldung, die die Einstellung "CustomErrors" in der Datei "Web.config" erwähnt, aber nicht an welche die eigentliche Ursache des Fehlers wurde:

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Standardmäßig zeigt ASP.NET ausführliche Fehlerinformationen, nur, wenn Ihre Webanwendung auf dem lokalen Computer ausgeführt wird. Sie möchten in der Regel nicht ausführliche Fehlerinformationen angezeigt wird, wenn die Webanwendung über das Internet öffentlich verfügbar ist, da Hacker dieser Informationen können Sie Sicherheitsrisiken in der Anwendung gefunden werden können. Allerdings, wenn Sie eine Website oder Updates auf einer Website bereitstellen, manchmal etwas geht falsch und müssen Sie die tatsächliche Fehlermeldung abzurufen.

Zum Aktivieren der Anwendung ausführlichen Fehlermeldungen angezeigt, wenn sie auf dem Remotehost ausgeführt wird, bearbeiten Sie die Datei "Web.config" CustomErrors Mode abgegrenzt, erneute Bereitstellung der Anwendung und führen Sie die Anwendung erneut aus:

1. Wenn der Anwendungsdatei "Web.config" AcustomErrors Element im thesystem.web-Element enthält, ändern Sie Themode-Attribut auf "off". Fügen Sie AcustomErrors Element andernfalls im thesystem.web-Element mit Themode-Attribut auf "off" festlegen, wie im folgenden Beispiel gezeigt: 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. Bereitstellen Sie die Anwendung.
3. Führen Sie die Anwendung, und wiederholen Sie, was Sie zuvor ausgeführt haben, die den Fehler verursacht hat. Jetzt können Sie sehen, was die eigentlichen Fehlermeldung ist.
4. Wenn Sie den Fehler behoben haben, Wiederherstellen der ursprünglichen CustomErrors-Einstellung und erneute Bereitstellung der Anwendung.

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>Kann nicht erstellt/Schatten kopieren "ContosoUniversity", wenn diese Datei bereits vorhanden ist.

### <a name="scenario"></a>Szenario

Wenn Sie versuchen, ein Projekt in Visual Studio auszuführen, erhalten Sie eine Fehlerseite mit einer Meldung wie im folgenden Beispiel:

Serverfehler in "/" Anwendung. Kann nicht erstellt/Schatten kopieren "ContosoUniversity", wenn diese Datei bereits vorhanden ist.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Eine Minute lang warten und den Browser zu aktualisieren oder erneut kompilieren der Website ein, und versuchen Sie es erneut ausführen.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Auf einer Webseite verweigert wird, verwendet SQLServer Compact

### <a name="scenario"></a>Szenario

Beim Bereitstellen von eines Standorts, der SQL Server Compact verwendet, und führen Sie eine Seite in die bereitgestellte Website, die auf die Datenbank zugreift, wird die folgende Fehlermeldung angezeigt:

Der Zugriff wird verweigert. (Ausnahme von HRESULT: 0 x 80070005 (E\_ACCESSDENIED))

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Das NETZWERKDIENST-Konto auf dem Server muss in der Lage, SQL-Dienst Compact native-Binärdateien zu lesen, die in der *bin\amd64* oder *bin\x86* Ordner, jedoch nicht über Leserechte verfügt über Berechtigungen für diesen Ordner. Set read-Berechtigung für den Netzwerkdienst auf dem *"bin"* Ordner, und stellen Sie sicher, um die Berechtigungen zum Unterordner zu erweitern.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Die Konfigurationsdatei aufgrund unzureichender Berechtigungen kann nicht gelesen werden.

### <a name="scenario"></a>Szenario

Wenn Sie auf der Visual Studio Schaltfläche "Veröffentlichen" zum Bereitstellen einer Anwendung in IIS auf dem lokalen Computer veröffentlichen ein Fehler auftritt und die **Ausgabe** Fenster zeigt eine Fehlermeldung angezeigt, die etwa wie folgt:

Fehler beim Lesen der IIS-Konfigurationsdatei "Computer/Umleitung". Die Identität, die Ausführung dieses Vorgangs wurde... Fehler: Die Konfigurationsdatei aufgrund unzureichender Berechtigungen kann nicht gelesen werden.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Mit nur einem Klick auf IIS auf dem lokalen Computer zu veröffentlichen, werden muss Visual Studio mit Administratorberechtigungen ausgeführt. Schließen Sie Visual Studio, und starten Sie es erneut mit Administratorberechtigungen.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Auf dem Zielcomputer konnte keine Verbindung hergestellt werden... Mithilfe des angegebenen Prozesses

### <a name="scenario"></a>Szenario

Wenn Sie auf der Visual Studio Schaltfläche "Veröffentlichen" zum Bereitstellen einer Anwendung veröffentlichen ein Fehler auftritt und die **Ausgabe** Fenster zeigt eine Fehlermeldung angezeigt, die etwa wie folgt:

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Ein Proxyserver wird die Kommunikation mit dem Zielserver unterbrechen. Wählen Sie in der Windows-Systemsteuerung oder im Internet Explorer **Internetoptionen** , und wählen Sie die **Verbindungen** Registerkarte. In der **Interneteigenschaften** (Dialogfeld), klicken Sie auf **LAN-Einstellungen**. In der **Local Area Network (LAN)-Einstellungen** deaktivieren Sie im Dialogfeld die **automatische Suche der Einstellungen** Kontrollkästchen. Klicken Sie dann auf die Schaltfläche "Veröffentlichen" erneut aus.

Wenn das Problem weiterhin besteht, wenden Sie sich an Ihren Systemadministrator, um zu bestimmen, was mit Proxy oder Firewall-Einstellungen erledigt werden kann. Das Problem auftritt, weil einen nicht standardmäßigen Port für die Bereitstellung der Webverwaltungsdienst (8172); Web Deploy verwendet werden. Bei anderen Verbindungen wird Port 80 verwendet, wenn Sie Web Deploy. Wenn Sie mit einem Drittanbieter-Hostinganbieter bereitstellen, verwenden Sie in der Regel der Webverwaltungsdienst.

## <a name="default-net-40-application-pool-does-not-exist"></a>.NET 4.0 Standardanwendungspool ist nicht vorhanden.

### <a name="scenario"></a>Szenario

Wenn Sie eine Anwendung, die .NET Framework 4 erforderlich sind bereitstellen, wird die folgende Fehlermeldung angezeigt:

Der standardmäßige .NET 4.0-Anwendungspool ist nicht vorhanden, oder die Anwendung konnte nicht hinzugefügt werden. Stellen Sie sicher, dass ASP.NET 4.0 auf diesem Computer installiert ist.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

ASP.NET 4 ist in IIS nicht installiert. Wenn der Server, dem Sie die Bereitstellung Ihrem Entwicklungscomputer ist und Visual Studio 2010 auf dem installiert ist, wird ASP.NET 4 ist auf dem Computer installiert, aber möglicherweise nicht in IIS installiert. Öffnen Sie auf dem Server, dem Sie bereitstellen ein Eingabeaufforderungsfenster mit erhöhten Rechten, und installieren Sie ASP.NET 4 in IIS, indem die folgenden Befehle ausführen:

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

Möglicherweise müssen auch die .NET Framework-Version des Standardanwendungspools manuell festlegen. Weitere Informationen finden Sie unter der Bereitstellung in IIS als eine Testumgebung Lernprogramm dieser Reihe.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Format der Initialisierungszeichenfolge stimmt nicht beginnend bei Index 0-Spezifikation überein.

### <a name="scenario"></a>Szenario

Nach der Bereitstellung einer Anwendung mit nur einem Klick veröffentlichen Sie, wenn ausführen, die eine Seite, die Zugriff auf die Datenbank Sie die folgende Fehlermeldung angezeigt erhalten:

Format der Initialisierungszeichenfolge stimmt nicht beginnend bei Index 0-Spezifikation überein.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Öffnen der *"Web.config"* -Datei in die bereitgestellte Website und das Kontrollkästchen, um festzustellen, ob die Werte der Verbindungszeichenfolgen beginnen mit $(ReplacableToken\_, wie im folgenden Beispiel:

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

Wenn die Verbindungszeichenfolgen wie Folgendes Beispiel aussehen, Bearbeiten der Projektdatei, und fügen Sie die folgende Eigenschaft auf die PropertyGroup-Element, das für alle Buildkonfigurationen ist:

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

Klicken Sie dann bereitstellen Sie die Anwendung erneut.

## <a name="http-500-internal-server-error"></a>HTTP 500 Interner Serverfehler

### <a name="scenario"></a>Szenario

Beim Ausführen der bereitgestellten Website sehen Sie die folgende Fehlermeldung angezeigt, ohne spezifische Angaben, der angibt, der Ursache des Fehlers:

HTTP-Fehler 500 – Interner Serverfehler.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Es gibt zahlreiche Ursachen von Fehlern 500 allerdings eine mögliche Ursache, wenn Sie diese Lernprogramme vorgehen handelt es sich, dass Sie ein XML-Element in der falschen Stelle in einer Web.config-Transformationsdateien einfügen. Beispielsweise würden Sie diesen Fehler erhalten, wenn Sie die Transformation einfügen, das eingefügt eine &lt;Speicherort&gt; Element unter &lt;system.web&gt; statt direkt unter &lt;Konfiguration&gt;. Die Datei "Web.config" Transform-Vorschaufeature können, stellen Sie sicher, dass die Transformationen wie vorgesehen funktionieren. Die Lösung, wenn Sie eine Transformation finden, die nicht ordnungsgemäß codiert wurde, ist die Transformationsdatei korrigieren und erneut bereitstellen. Wenn ein Fehler nicht offensichtlich ist, kommentieren Sie Transformationen und erneutes Bereitstellen, um festzustellen, was den Fehler 500 verursacht.

## <a name="http-50021-internal-server-error"></a>HTTP-500.21 Interner Serverfehler

### <a name="scenario"></a>Szenario

Wenn Sie den bereitgestellten Standort ausführen, sehen Sie die folgende Fehlermeldung angezeigt:

HTTP-Fehler 500.21 - Interner Serverfehler. Handler "PageHandlerFactory-integriert" weist das ungültige Modul "ManagedPipelineHandler" in der Modulliste.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Die Website wurde bereitgestellt, Ziele, die ASP.NET 4, aber ASP.NET 4 ist nicht in IIS auf dem Server registriert. Klicken Sie auf dem Server öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten, und registrieren Sie ASP.NET 4, indem Sie die folgenden Befehle ausführen:

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

Möglicherweise müssen auch die .NET Framework-Version des Standardanwendungspools manuell festlegen. Weitere Informationen finden Sie unter der Bereitstellung in IIS als eine Testumgebung Lernprogramm dieser Reihe.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Fehler bei der Anmeldung öffnende SQL Server Express-Datenbank in-App\_Daten

### <a name="scenario"></a>Szenario

Sie auf dem laufenden der *"Web.config"* Verbindungszeichenfolge für die SQL Server Express-Datenbank als Datei ein *mdf* Datei Ihre *App\_Daten* Ordner und die erste Ausführung der Anwendung, die Sie sehen die folgende Fehlermeldung angezeigt:

System.Data.SqlClient.SqlException: Datenbank "Datenbankname" von der Anmeldung angeforderte kann kann nicht geöffnet werden. Fehler bei der Anmeldung.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Der Name des der *mdf* Datei kann nicht mit dem Namen einer beliebigen SQL Server Express-Datenbank, die jemals auf Ihrem Computer vorhanden ist übereinstimmen, auch wenn Sie gelöscht der *mdf* Datei bereits vorhandene Datenbank. Ändern Sie den Namen des der *mdf* Datei in einen Namen, die nie als Datenbanknamen und Änderung verwendet wurde die *"Web.config"* zu den neuen Namen zu verwendende Datei an. Als Alternative können Sie [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) gelöscht zuvor vorhandenen SQL Server Express-Datenbanken.

## <a name="model-compatibility-cannot-be-checked"></a>Modell Kompatibilität können nicht überprüft werden

### <a name="scenario"></a>Szenario

Sie auf dem laufenden der *"Web.config"* Datei die Verbindungszeichenfolge so, dass in einer neuen SQL Server Express-Datenbank verweist, und beim ersten die Anwendung ausführen die folgende Fehlermeldung angezeigt:

Die modellkompatibilität kann nicht überprüft werden, da die Datenbank keine Modellmetadaten enthält. Stellen Sie sicher, dass die DbModelBuilder-Konventionen IncludeMetadataConvention hinzugefügt wurde.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Wenn der Name der Datenbank, die Sie in der Datei "Web.config" gesteckt je verwendet wurde, bevor auf Ihrem Computer eine Datenbank mit einigen Tabellen in der sie möglicherweise schon vorhanden sind. Wählen Sie einen neuen Namen, die auf Ihrem Computer vor und die Änderung nicht verwendet wurde die *"Web.config"* Datei, zeigen Sie auf diesem neuen Datenbanknamen verwenden. Als Alternative können Sie [SQL Server Express-Hilfsprogramm](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) oder [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) beim Löschen der vorhandenen Datenbank.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>SQL-Fehler, wenn ein Skript versucht, das Erstellen von Benutzern oder Rollen

### <a name="scenario"></a>Szenario

Verwenden Sie datenbankbereitstellung konfiguriert werden, auf die **SQL packen/veröffentlichen** Registerkarte SQL-Skripts, die während der Bereitstellung ausgeführt umfassen Create User oder Create Role-Befehle und Skript erzeugt die Ausführung dieser Befehle ausgeführt werden. Möglicherweise ausführlichere Meldungen, wie die folgende angezeigt:

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

Wenn dieser Fehler tritt auf, wenn Sie in die Bereitstellung konfiguriert haben die **Web veröffentlichen** Assistenten statt über das **SQL packen/veröffentlichen** Registerkarte, erstellen Sie einen Thread in der [Konfiguration und Bereitstellung](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) Forum und die Lösung wird diese Seite "Problembehandlung" hinzugefügt werden.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Berechtigung zum Erstellen von Benutzern bzw. Rollen keine für das Benutzerkonto, das Sie verwenden, um die Bereitstellung durchzuführen. Hostinganbieter kann z. B. die Db zuweisen\_Datareader Db\_Datawriter, und Db\_Ddladmin Rollen mit dem Benutzerkonto, mit dem es für Sie eingerichtet. Diese reichen für die meisten Datenbankobjekte erstellen, aber nicht für Benutzer oder Rollen erstellen. Eine Möglichkeit, den Fehler zu vermeiden, wird durch das Ausschließen von Benutzern und Rollen von datenbankbereitstellung. Hierzu können Sie durch Bearbeiten der PreSource-Element für automatisch generierte Skript die Datenbank, in die folgenden Attribute enthalten:

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

Informationen dazu, wie das PreSource-Element in der Projektdatei bearbeiten, finden Sie unter [Vorgehensweise: Bearbeiten von Bereitstellungseinstellungen in der Projektdatei](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Wenn die Benutzer oder Rollen in der Entwicklungsdatenbank in der Zieldatenbank werden müssen, wenden Sie sich an Ihrem Hostinganbieter, um Unterstützung zu erhalten.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>SQL Server-Timeoutfehler beim Ausführen von benutzerdefinierten Skripts während der Bereitstellung

### <a name="scenario"></a>Szenario

Sie während der Bereitstellung auszuführenden benutzerdefinierten SQL-Skripts angegeben haben, und wenn sie Web Deploy ausgeführt wurde, sie Timeout eintritt.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Mehrere Skripts mit verschiedenen Transaktionsmodi ausgeführt, kann zu Timeout-Fehlern führen. Standardmäßig automatisch generierten Skripts, die in einer Transaktion ausgeführt, aber benutzerdefinierte Skripts nicht der Fall. Bei Auswahl der **Daten und/oder Schema aus einer vorhandenen Datenbank mithilfe von Pull** option die **SQL packen/veröffentlichen** Registerkarte, wenn Sie ein benutzerdefinierte SQL-Skript hinzufügen, müssen Sie transaktionseinstellungen für einige Skripts ändern, damit Alle Skripts verwenden die gleichen transaktionseinstellungen. Weitere Informationen finden Sie unter [Vorgehensweise: Bereitstellen einer Datenbank mit einem Webanwendungsprojekt](https://msdn.microsoft.com/library/dd465343.aspx).

Wenn Sie transaktionseinstellungen konfiguriert haben, sodass alle identisch sind, aber diese Fehlermeldung weiterhin erhalten, ist eine mögliche problemumgehung die Skripts separat ausführen. In der **Datenbankskripts** Raster in die **packen/veröffentlichen** Registerkarte "SQL", Deaktivieren der **Include** Kontrollkästchen für das Skript, das den Timeoutfehler verursacht hat, dann das Projekt veröffentlichen. Kehren Sie zurück in die **Datenbankskripts** Raster, wählen Sie dieses Skripts **Include** Kontrollkästchen, und Deaktivieren der **Include** Kontrollkästchen für die anderen Skripts. Klicken Sie dann veröffentlichen Sie das Projekt erneut. Dieses Mal beim Veröffentlichen, wird nur die ausgewählten benutzerdefinierten Skripts ausgeführt.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Streamen von Daten von Website-Manifest ist noch nicht verfügbar

### <a name="scenario"></a>Szenario

Wenn Sie installieren ein Paket mit der *deploy.cmd* Datei mit der Option t (Test), die Sie sehen der folgende Fehlermeldung angezeigt:

Fehler: Die Streamdaten von "Sitemanifest/DbFullSql [@path="C:\TEMP\AdventureWorksGrant.sql']/sqlScript"ist noch nicht verfügbar.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Die Fehlermeldung bedeutet, dass der Befehl einen Testbericht erzeugen kann. Jedoch möglicherweise den Befehl ausführen, wenn Sie die (tatsächliche Installationsoption) "y" verwenden. Die Meldung gibt nur an, dass ein Problem mit dem Befehl im Testmodus vorliegt.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Diese Anwendung erfordert ManagedRuntimeVersion v4. 0

### <a name="scenario"></a>Szenario

Wenn Sie versuchen, bereitstellen, wird die folgende Fehlermeldung angezeigt:

Der Anwendungspool, den Sie verwenden möchten hat die 'ManagedRuntimeVersion'-Eigenschaft auf "v2. 0" festgelegt. Diese Anwendung erfordert "v4. 0".

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

ASP.NET 4 ist in IIS nicht installiert. Wenn der Server, dem Sie die Bereitstellung Ihrem Entwicklungscomputer ist und Visual Studio 2010 auf dem installiert ist, wird ASP.NET 4 ist auf dem Computer installiert, aber möglicherweise nicht in IIS installiert. Öffnen Sie auf dem Server, dem Sie bereitstellen ein Eingabeaufforderungsfenster mit erhöhten Rechten, und installieren Sie ASP.NET 4 in IIS, indem die folgenden Befehle ausführen:

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Microsoft.Web.Deployment.DeploymentProviderOptions umgewandelt

### <a name="scenario"></a>Szenario

Wenn Sie ein Paket bereitstellen, wird die folgende Fehlermeldung angezeigt:

Kann nicht Objekt vom Typ "Microsoft.Web.Deployment.DeploymentProviderOptions", "Microsoft.Web.Deployment.DeploymentProviderOptions" umgewandelt werden.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Sie versuchen, das zum Bereitstellen von IIS-Manager über die Bereitstellung 1.1 Benutzeroberfläche auf einem Server mit Web Deploy 2.0 installiert. Wenn Sie das IIS-Verwaltungstool Remote bereitstellen, indem ein Kontrollkästchen-Paket importieren verwenden die **neue Features verfügbar** (Dialogfeld), wenn Sie die Verbindung herstellen. (Dieses Dialogfeld kann nur einmal angezeigt, wenn die Verbindung erstmalig eingerichtet wird. Um die Verbindung deaktivieren und erneut zu beginnen, IIS-Manager zu schließen und erneut gestartet werden Sie durch Eingabe von Inetmgr/zurücksetzen an der Eingabeaufforderung.) Wenn eine der Funktionen aufgeführt ist **Webbenutzeroberfläche bereitstellen**, und es wurde eine Versionsnummer niedriger als 8 ist, müssen Sie die Bereitstellung für Server möglicherweise 1.1 und 2.0-Versionen von Web Deploy installiert. Um von einem Client bereitstellen, die 2.0 installiert ist, muss der Server nur Web Deploy 2.0 installiert haben. Sie müssen Ihrem Hostinganbieter zum Beheben dieses Problems zu erhalten.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Kann nicht geladen werden die systemeigenen Komponenten von SQL Server Compact

### <a name="scenario"></a>Szenario

Wenn Sie den bereitgestellten Standort ausführen, sehen Sie die folgende Fehlermeldung angezeigt:

Fehler beim Laden der systemeigenen Komponenten von SQL Server Compact, des Anbieters ADO.NET Version 8482 entspricht. Installieren Sie die richtige Version von SQL Server Compact. Finden Sie im Knowledge Base-Artikel 974247 Weitere Details.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Der bereitgestellte Standort verfügt nicht über *amd64* und *X86* Unterordner mit der systemeigenen Assemblys werden unter der Anwendungsverzeichnis *"bin"* Ordner. Auf einem Computer, SQL Server Compact installiert ist, befinden sich die systemeigenen Assemblys im *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. Die beste Möglichkeit, erhalten die richtigen Dateien in den richtigen Ordnern in einem Visual Studio-Projekt ist das NuGet SqlServerCompact-Paket installieren. Paketinstallation Fügt eine Postbuild-Skript aus, um die systemeigenen Assemblys in kopieren *amd64* und *X86*. Damit für diese bereitgestellt werden können müssen Sie jedoch manuell in das Projekt aufgenommen. Weitere Informationen finden Sie unter der [Bereitstellen von SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) Lernprogramm.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>"Pfad ist ungültig" Fehler nach dem Bereitstellen einer Entity Framework Code First-Anwendung

### <a name="scenario"></a>Szenario

Bereitstellung einer Anwendung, die Entity Framework Code First-Migrationen und einem DBMS z. B. SQL Server Compact verwendet wird, das die Datenbank in einer Datei in die App speichert\_Datenordner. Sie müssen Code First-Migrationen so konfiguriert, dass um die Datenbank nach der ersten Bereitstellung zu erstellen. Wenn Sie die Anwendung ausführen, erhalten Sie eine Fehlermeldung wie im folgenden Beispiel:

Der Pfad ist ungültig. Überprüfen Sie das Verzeichnis für die Datenbank an. [Path = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf ]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Code wird zuerst die Datenbank, aber die App zu erstellen versucht\_Datenordner ist nicht vorhanden. Entweder haben nicht Sie alle Dateien haben, in der *App\_Daten* Ordner, wenn Sie bereitgestellt haben, oder Sie ausgewählt **ausschließen App\_Daten** auf die **Web packen/veröffentlichen** Registerkarte dem Fenster "Projekteigenschaften". Der Bereitstellungsprozess wird keinen Ordner auf dem Server erstellen, wenn es sind keine Dateien im Ordner "" an den Server kopiert werden soll. Bereits die Datenbank am Standort eingerichtet haben, wird der Bereitstellungsprozess die Dateien löschen und die *App\_Daten* Ordner selbst bei Auswahl **entfernen weiterer Dateien am Ziel** in Das Veröffentlichungsprofil. Um das Problem zu beheben, speichern Sie eine Platzhalterdatei, z. B. einer TXT-Datei in die *App\_Daten* Ordner stellen Sie sicher, dass Sie keine **ausschließen App\_Daten** ausgewählt, und stellen Sie erneut bereit.

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"COM-Objekt, das vom zugrunde liegenden RCW getrennt wurde kann nicht verwendet werden."

### <a name="scenario"></a>Szenario

Sie wurden erfolgreich mit nur einem Klick zum Bereitstellen der Anwendung zu veröffentlichen und starten Sie dann Sie dieser Fehler angezeigt:

Fehler bei der webbereitstellungstasks. (Die Anforderung an die remote-Agent-URL konnte nicht abgeschlossen werden "<https://serverurl.com/msdeploy.axd?site=sitename>".)  
 Die Anforderung an die remote-Agent-URL konnte nicht abgeschlossen werden "<https://url/msdeploy.axd?site=sitename>".  
Die Anforderung wurde abgebrochen: die Anforderung wurde abgebrochen.  
COM-Objekt, das vom zugrunde liegenden RCW getrennt wurde, kann nicht verwendet werden.

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

Standardmäßig legt Visual Studio Leseberechtigungen für den Stammordner des Standorts und Schreibberechtigungen für die App\_Datenordner. Wenn Ihre Anwendung Schreibzugriff auf ein Unterordner benötigt, können Sie Berechtigungen für diesen Ordner als im Ordner und Bereitstellen von der Einstellung für die Produktionsumgebung Lernprogramme in dieser Serie gezeigt festlegen. Wenn Ihre Anwendung Schreibzugriff auf den Stammordner der Website benötigt, müssen Sie verhindern, dass nur-Lese-Zugriff auf den Stammordner festlegen, durch Hinzufügen von **&lt;IncludeSetACLProviderOn Ziel&gt;"false"&lt;/ IncludeSetACLProviderOnDestination&gt;** die veröffentlichungsprofildatei (auf einem einzigen Profil wirkt sich auf) oder die Datei wpp.targets (um die Auswirkung auf die von allen Profilen). Informationen zur Bearbeitung dieser Dateien finden Sie unter [Vorgehensweise: Bearbeiten von Bereitstellungseinstellungen in Profildateien (.pubxml)](https://msdn.microsoft.com/library/ff398069.aspx).

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Fehler bei der Konfiguration - TargetFramework-Attributs verweist auf eine Version, die höher als die installierte Version von .NET Framework ist

### <a name="scenario"></a>Szenario

Sie wurden erfolgreich veröffentlicht ein Webprojekt, die ASP.NET 4.5 abzielen, aber beim Ausführen der Anwendung (mit den CustomErrors-Modus auf "off" in der Datei Web.config festgelegt) erhalten Sie die folgende Fehlermeldung:

Die "TargetFramework"-Attribut in der &lt;Kompilierung&gt; -Element der Datei "Web.config" wird nur für Zielversion 4.0 und höher von .NET Framework verwendet (z. B. "&lt;Kompilierung TargetFramework ="4.0"&gt;'). Das Attribut "TargetFramework" verweist auf derzeit eine Version, die höher als die installierte Version von .NET Framework ist. Geben Sie eine gültige Zielversion von .NET Framework, oder installieren Sie die erforderliche Version von .NET Framework.

Das Feld Quellfehler die Fehlerseite nennt die folgende Zeile aus der Datei "Web.config" als die Ursache des Fehlers:

&lt;compilation targetFramework="4.5" /&gt;

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Der Server unterstützt nicht ASP.NET 4.5. Wenden Sie sich an den Hostinganbieter, um zu bestimmen, wann und ob Unterstützung für ASP.NET 4.5 hinzugefügt werden kann. Wenn Sie den Server ein Upgrade nicht möglich ist, müssen Sie ein Webprojekt bereitstellen, die ASP.NET 4 oder früher ausgerichtet ist stattdessen.

Wenn Sie eine ASP.NET 4 oder früher Webprojekt mit dem gleichen Ziel bereitstellen, wählen Sie die **entfernen weiterer Dateien am Ziel** Kontrollkästchen auf der **Einstellungen** auf der Registerkarte die **"Web veröffentlichen"**Assistenten. Wenn Sie nicht auswählen **entfernen weiterer Dateien am Ziel**, werden Sie weiterhin erhalten, die Seite "Fehler bei der Konfiguration".

Das Projekt **Eigenschaften** Windows enthält die Dropdownliste für ein Ziel-Framework, aber Sie können nicht das Problem beheben, indem Sie nur ändern, die von **.NET Framework 4.5** auf **.NET Framework 4**. Wenn Sie das Zielframework in einer früheren Frameworkversion ändern, wird das Projekt verfügen weiterhin über Verweise auf die neuere Frameworkversion-Assemblys und wird nicht ausgeführt. Sie müssen manuell die Verweise ändern oder Erstellen eines neuen Projekts, das .NET Framework 4 oder früher ausgerichtet ist. Weitere Informationen finden Sie unter [.NET Framework als Ziel für Websites](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

## <a name="medium-trust-errors"></a>Mittlere Vertrauensstellungsfehler

### <a name="scenario"></a>Szenario

Wenn Sie Ihre Anwendung in der Produktion ausführen, ruft er ein Fehler im Zusammenhang mit mittlerer Vertrauenswürdigkeit ab.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Viele Drittanbieter-Hostinganbieter führen Ihre Website in mittlerer Vertrauenswürdigkeit, d. Es gibt einige Dinge, die es keine Erlaubnis hat h., führen. Beispielsweise kann nicht Anwendungscode den Zugriff auf die Windows-Registrierung und kann nicht lesen oder Schreiben von Dateien, die außerhalb der Ordnerhierarchie für Ihre Anwendung sind. Standardmäßig die Anwendung ausgeführt wird, im *volle Vertrauenswürdigkeit* auf dem lokalen Computer, was bedeutet, dass die Anwendung möglicherweise können Aktionen ausgeführt werden, die ein Fehler auftritt, während der Bereitstellung bis hin zur Produktion.

Sie können die Anwendung mittlere Vertrauensebene in den lokalen IIS-Umgebung ausgeführt werden, um die Problembehandlung konfigurieren. Zu diesem Zweck öffnen Sie die Anwendung *"Web.config"* Datei, und fügen Sie eine **Vertrauensstellung** Element in der **system.web** Element, wie im folgenden Beispiel gezeigt.

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

Die Anwendung wird nun in mittlerer vetrauenswürdigkeit in IIS auch auf dem lokalen Computer ausgeführt.

Keine dies durchführen, wenn Sie die Bereitstellung für Azure App Service, da Azure keine mittleren Vertrauenswürdigkeit erforderlich sind. Zum Zeitpunkt dieses Lernprogramms im Februar 2012 geschrieben wird verursacht mit dieser Methode, um Ihre Anwendung mittlerer Vertrauenswürdigkeit ausgeführt wird, einen Fehler in Azure.

Wenn Sie Entity Framework Code First-Migrationen verwenden, und Sie die Bereitstellung für einen Hostinganbieter, der Ihre Anwendung mittlerer Vertrauenswürdigkeit ausgeführt wird, stellen Sie sicher, dass Sie Version 5.0 oder höher installiert. In Entity Framework, Version 4.3 Migrationen erfordert volle Vertrauenswürdigkeit um das Datenbankschema zu aktualisieren.

## <a name="http-40417-not-found-error"></a>HTTP 404.17 wurde nicht gefunden

### <a name="scenario"></a>Szenario

Beim Ausführen der bereitgestellten Website auf dem Entwicklungscomputer in IIS finden Sie die folgende Fehlermeldung angezeigt, die meldet, dass der Server "default.aspx" verarbeiten kann:

HTTP-Fehler 404.17 - wurde nicht gefunden.

Der angeforderte Inhalt Skript und wird nicht vom Handler für statische Dateien verarbeitet werden.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

ASP.NET 4.5 möglicherweise nicht auf Ihrem Computer installiert werden. Sehen Sie die Schritte in der Bereitstellung in IIS als eine Testumgebung Lernprogramm dieser Reihe, die erläutern, wie ASP.NET 4.5 installiert.

> [!div class="step-by-step"]
> [Vorherige](deploying-extra-files.md)
