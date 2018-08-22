---
uid: web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
title: 'ASP.NET-webbereitstellung mithilfe von Visual Studio: Problembehandlung | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, indem Warnungsprovider...
ms.author: riande
ms.date: 06/01/2015
ms.assetid: c0090595-ab3b-4b9b-9e16-7a1891e8cb2f
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting
msc.type: authoredcontent
ms.openlocfilehash: 65cd5cd9f7d1f9c5fdaea9b0d16bdfd84259efdd
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829278"
---
<a name="aspnet-web-deployment-using-visual-studio-troubleshooting"></a>ASP.NET-webbereitstellung mithilfe von Visual Studio: Problembehandlung
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, mithilfe von Visual Studio 2012 oder Visual Studio 2010. Weitere Informationen über die Reihe finden Sie unter [im ersten Tutorial der Reihe](introduction.md).


Diese Seite beschreibt einige der häufigsten Probleme, die auftreten können, wenn Sie eine ASP.NET-Webanwendung mithilfe von Visual Studio bereitstellen. Für jede Adresse werden ein oder mehrere mögliche Ursachen und entsprechende Lösungen bereitgestellt.

Die gezeigten Szenarien gelten für Azure und Drittanbietern Hostinganbieter. Weitere Informationen zur Problembehandlung in Azure App Service-Web-apps finden Sie unter den folgenden Ressourcen:

- [Problembehandlung in einer Web-App in Azure App Service mithilfe von Visual Studio](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-troubleshoot-visual-studio/)
- [Überwachen von Web-Apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-monitor//)
- [Ankündigung der Version von Windows Azure SDK 2.0 für .NET](http://https://weblogs.asp.net/scottgu/announcing-the-release-of-windows-azure-sdk-2-0-for-net) (Scottgus Blog, zeigt, wie zum Abrufen von Diagnoseprotokollen in Visual Studio)

## <a name="server-error-in--application---current-custom-error-settings-prevent-details-of-the-error-from-being-viewed-remotely"></a>Serverfehler in '/' Anwendung – aktuelle Einstellungen für benutzerdefinierte Fehler verhindern, dass Details des Fehlers angezeigt wird, Remote

### <a name="scenario"></a>Szenario

Nach dem Bereitstellen einer Websites mit einem Remotehost, erhalten Sie eine Fehlermeldung angezeigt, die die CustomErrors-Einstellung in der Datei "Web.config" erwähnt, aber nicht angegeben, was die eigentliche Ursache des Fehlers:

[!code-xml[Main](troubleshooting/samples/sample1.xml)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Standardmäßig zeigt ASP.NET ausführliche Fehlerinformationen, nur, wenn Ihre Webanwendung auf dem lokalen Computer ausgeführt wird. Im Allgemeinen möchten nicht ausführliche Fehlerinformationen angezeigt wird, wenn die Webanwendung über das Internet öffentlich verfügbar ist, da Hacker können diese Informationen verwenden, um Schwachstellen in der Anwendung zu finden sein können. Wenn Sie jedoch Sie einen Standort oder Updates an einem Standort bereitstellen, manchmal etwas schief gehen und müssen Sie die eigentliche Fehlermeldung zu erhalten.

Zum Aktivieren der Anwendung detaillierte Fehlermeldungen angezeigt, wenn sie auf dem Remotehost ausgeführt wird, bearbeiten Sie die Datei "Web.config" zum Festlegen von CustomErrors-Modus aus, die Anwendung erneut bereitstellen, und führen Sie die Anwendung erneut aus:

1. Wenn die Web.config-Datei der Anwendung im thesystem.web Element AcustomErrors-Element enthält, ändern Sie Themode-Attribut auf "off". Fügen Sie AcustomErrors Element andernfalls im thesystem.web-Element mit Themode-Attribut auf "off" festlegen, wie im folgenden Beispiel gezeigt: 

    [!code-xml[Main](troubleshooting/samples/sample2.xml)]
2. Bereitstellen Sie die Anwendung.
3. Führen Sie die Anwendung, und wiederholen Sie den zuvor, die den Fehler verursacht hat. Jetzt können Sie sehen, was die eigentliche Fehlermeldung ist.
4. Wenn Sie den Fehler behoben haben, stellen Sie die ursprüngliche CustomErrors-Einstellung aus, und Bereitstellen Sie die Anwendung erneut.

## <a name="cannot-createshadow-copy-contosouniversity-when-that-file-already-exists"></a>Kann nicht erstellt/Shadow Copy "ContosoUniversity", wenn die Datei bereits vorhanden ist.

### <a name="scenario"></a>Szenario

Wenn Sie versuchen, ein Projekt in Visual Studio ausführen, erhalten Sie eine Fehlerseite mit einer Meldung wie im folgenden Beispiel:

Serverfehler in "/" Anwendung. Kann nicht erstellt/Shadow Copy "ContosoUniversity", wenn die Datei bereits vorhanden ist.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Warten Sie eine Minute, und aktualisieren Sie den Browser, oder Kompilieren Sie die Site neu, und führen Sie es erneut aus.

## <a name="access-is-denied-in-a-web-page-that-uses-sql-server-compact"></a>Auf einer Webseite authentifiziert werden, verwendet SQLServer Compact

### <a name="scenario"></a>Szenario

Beim Bereitstellen einer Website, die SQL Server Compact verwendet, und Sie eine Seite in der bereitgestellten Website, die Zugriff auf die Datenbank ausführen, sehen Sie die folgende Fehlermeldung angezeigt:

Der Zugriff wird verweigert. (Ausnahme von HRESULT: 0 x 80070005 (E\_ACCESSDENIED))

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Das NETZWERKDIENST-Konto auf dem Server muss in der Lage, SQL-Dienst Compact native-Binärdateien zu lesen, die in der *bin\amd64* oder *bin\x86* Ordner, aber es ist keine Leseberechtigungen für diese Ordner. Set read-Berechtigung für den Netzwerkdienst auf die *Bin* Ordner, und stellen Sie sicher, um die Berechtigungen, um Unterordner zu erweitern.

## <a name="cannot-read-configuration-file-due-to-insufficient-permissions"></a>Die Konfigurationsdatei aufgrund unzureichender Berechtigungen kann nicht gelesen werden.

### <a name="scenario"></a>Szenario

Wenn Sie auf der Visual Studio Schaltfläche "Veröffentlichen" zum Bereitstellen einer Anwendung in IIS auf dem lokalen Computer veröffentlichen ein Fehler auftritt und die **Ausgabe** Fenster wird eine Fehlermeldung angezeigt, die etwa wie folgt:

Fehler beim Lesen der IIS-Konfigurationsdatei "Computer/Umleitung". Die Identität, die Ausführung dieses Vorgangs wurde... Fehler: Die Konfigurationsdatei aufgrund unzureichender Berechtigungen kann nicht gelesen werden.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Mit nur einem Klick in IIS auf dem lokalen Computer veröffentlichen, müssen Sie ausführen Visual Studio mit Administratorberechtigungen. Schließen Sie Visual Studio, und starten Sie ihn mit Administratorberechtigungen neu.

## <a name="could-not-connect-to-the-destination-computer--using-the-specified-process"></a>Auf dem Zielcomputer konnte keine Verbindung hergestellt werden... Mithilfe des angegebenen Prozesses

### <a name="scenario"></a>Szenario

Wenn Sie auf der Visual Studio Schaltfläche "Veröffentlichen" zum Bereitstellen einer Anwendung, veröffentlichen ein Fehler auftritt und die **Ausgabe** Fenster wird eine Fehlermeldung angezeigt, die etwa wie folgt:

[!code-console[Main](troubleshooting/samples/sample3.cmd)]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Ein Proxyserver wird die Kommunikation mit dem Zielserver unterbrechen. Wählen Sie in der Windows-Systemsteuerung oder im Internet Explorer **Internetoptionen** , und wählen Sie die **Verbindungen** Registerkarte. In der **Interneteigenschaften** Dialogfeld klicken Sie auf **LAN-Einstellungen**. In der **Local Area Network (LAN) Einstellungen** Dialogfeld das Kontrollkästchen der **automatische Suche der Einstellungen** Kontrollkästchen. Klicken Sie dann auf die Schaltfläche "Veröffentlichen" erneut aus.

Wenn das Problem weiterhin besteht, wenden Sie sich an Ihren Systemadministrator, um zu bestimmen, was mit Proxy oder Firewall-Einstellungen erfolgen kann. Das Problem auftritt, da Sie einen nicht standardmäßigen Port für die Web-Management-Dienst-Bereitstellung (8172), Web Deploy verwendet werden. Bei anderen Verbindungen verwendet das Web Deploy-Port 80. Wenn Sie bei einem Hostinganbieter von Drittanbietern bereitstellen, verwenden Sie in der Regel den Web-Management-Dienst.

## <a name="default-net-40-application-pool-does-not-exist"></a>Standard .NET 4.0-Anwendungspool ist nicht vorhanden.

### <a name="scenario"></a>Szenario

Wenn Sie eine Anwendung, die .NET Framework 4 erforderlich sind bereitstellen, sehen Sie die folgende Fehlermeldung angezeigt:

Der Standardanwendungspool für .NET 4.0 ist nicht vorhanden, oder die Anwendung konnte nicht hinzugefügt werden. Stellen Sie sicher, dass ASP.NET 4.0 auf diesem Computer installiert ist.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

ASP.NET 4 ist in IIS nicht installiert. Wenn der Server für die Bereitstellung werden Ihrem Entwicklungscomputer und verfügt über Visual Studio 2010 installiert, ASP.NET 4 ist auf dem Computer installiert, aber Sie werden möglicherweise nicht in IIS installiert. Öffnen Sie auf dem Server, dem Sie bereitstellen eine Eingabeaufforderung mit erhöhten Rechten, und installieren Sie ASP.NET 4 in IIS, indem Sie die folgenden Befehle ausführen:

[!code-console[Main](troubleshooting/samples/sample4.cmd)]

Sie müssen möglicherweise auch die .NET Framework-Version von den Standardanwendungspool manuell festlegen. Weitere Informationen finden Sie unter der Bereitstellung von IIS als Testumgebung Tutorial dieser Reihe.

## <a name="format-of-the-initialization-string-does-not-conform-to-specification-starting-at-index-0"></a>Format der Initialisierungszeichenfolge entspricht nicht die Spezifikation, die beginnend bei Index 0.

### <a name="scenario"></a>Szenario

Nach der Bereitstellung einer Anwendung mit nur einem Klick veröffentlichen Sie, wenn Sie eine Seite, die Zugriff auf die Datenbank ausführen Sie die folgende Fehlermeldung erhalten:

Format der Initialisierungszeichenfolge entspricht nicht die Spezifikation, die beginnend bei Index 0.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Öffnen der *"Web.config"* Datei in der bereitgestellten Website und das Kontrollkästchen, um festzustellen, ob die Werte der Verbindungszeichenfolgen, die mit $ beginnen (ReplacableToken\_, wie im folgenden Beispiel:

[!code-xml[Main](troubleshooting/samples/sample5.xml)]

Die Verbindungszeichenfolgen wie im folgenden Beispiel zu sehen, bearbeiten Sie die Projektdatei, und fügen Sie die folgende Eigenschaft auf das PropertyGroup-Element, das für alle Buildkonfigurationen:

[!code-xml[Main](troubleshooting/samples/sample6.xml)]

Klicken Sie dann bereitstellen Sie die Anwendung erneut.

## <a name="http-500-internal-server-error"></a>HTTP 500 Interner Serverfehler

### <a name="scenario"></a>Szenario

Wenn Sie auf die bereitgestellte Website ausführen, sehen Sie die folgende Fehlermeldung angezeigt, ohne spezifische Angaben, der angibt, der Ursache des Fehlers:

HTTP-Fehler 500 – Interner Serverfehler.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Es gibt viele Ursachen für Fehler vom Typ 500 eine mögliche Ursache, wenn Sie in diesen Tutorials durcharbeiten ist allerdings, dass Sie ein XML-Element in der falschen Stelle in einem der Web.config-Transformationsdateien einfügen. Beispielsweise würden Sie diese Fehlermeldung erhalten, wenn Sie die Transformation einfügen, die Fügt eine &lt;Speicherort&gt; Element unter &lt;"System.Web"&gt; anstelle von direkt unter &lt;Konfiguration&gt;. Sie können das Vorschaufeature des Web.config-Transformation verwenden, um sicherzustellen, dass die Transformationen wie vorgesehen funktionieren. Die Lösung, wenn Sie eine Transformation finden, die nicht ordnungsgemäß codiert wurde, ist die Transformationsdatei korrigieren und erneut bereitstellen. Wenn ein Fehler nicht offensichtlich ist, kommentieren Sie Transformationen und erneut bereitstellen, um die finden Sie unter den 500-Fehler verursacht.

## <a name="http-50021-internal-server-error"></a>HTTP-500.21 Interner Serverfehler

### <a name="scenario"></a>Szenario

Wenn Sie auf die bereitgestellte Website ausführen, sehen Sie die folgende Fehlermeldung angezeigt:

HTTP-Fehler 500.21 - Interner Serverfehler. Handler "PageHandlerFactory-integriert" hat ein ungültiges Modul "ManagedPipelineHandler" in der Modulliste an.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Die Website haben Sie Ziele, die ASP.NET 4, aber ASP.NET 4 ist nicht registriert in IIS auf dem Server bereitgestellt. Klicken Sie auf dem Server öffnen Sie eine Eingabeaufforderung mit erhöhten Rechten, und registrieren Sie ASP.NET 4, indem Sie die folgenden Befehle ausführen:

[!code-console[Main](troubleshooting/samples/sample7.cmd)]

Sie müssen möglicherweise auch die .NET Framework-Version von den Standardanwendungspool manuell festlegen. Weitere Informationen finden Sie unter der Bereitstellung von IIS als Testumgebung Tutorial dieser Reihe.

## <a name="login-failed-opening-sql-server-express-database-in-appdata"></a>Fehler bei der Anmeldung öffnen SQL Server Express-Datenbank in-App\_Daten

### <a name="scenario"></a>Szenario

Sie aktualisiert die *"Web.config"* Datei die Verbindungszeichenfolge für SQL Server Express-Datenbank als eine *mdf* Datei Ihre *App\_Daten* Ordner und die erste Ausführung der Anwendung, die Sie sehen die folgende Fehlermeldung angezeigt:

System.Data.SqlClient.SqlException: Datenbank "DatabaseName" von der Anmeldung angeforderte kann kann nicht geöffnet werden. Die Anmeldung ist fehlgeschlagen.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Der Name des der *mdf* Datei kann nicht mit dem Namen einer beliebigen SQL Server Express-Datenbank, die jemals auf Ihrem Computer vorhanden ist übereinstimmen, auch wenn Sie gelöscht der *mdf* -Datei der bereits vorhandene Datenbank. Ändern Sie den Namen des der *mdf* Datei einen Namen, die als Name der Datenbank und ändern Sie eine noch nie verwendet wurde die *"Web.config"* Datei, die den neuen Namen verwenden. Als Alternative können Sie [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) So löschen Sie die bereits vorhandene SQL Server Express-Datenbanken.

## <a name="model-compatibility-cannot-be-checked"></a>Modell-Kompatibilität können nicht überprüft werden

### <a name="scenario"></a>Szenario

Sie aktualisiert den *"Web.config"* Datei die Verbindungszeichenfolge so in eine neue SQL Server Express-Datenbank verweist, und beim ersten der Anwendung ausführen die folgende Fehlermeldung angezeigt:

Modell-Kompatibilität kann nicht überprüft werden, da die Datenbank nicht über die Modellmetadaten enthält. Stellen Sie sicher, dass die Konventionen DbModelBuilder IncludeMetadataConvention hinzugefügt wurde.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Wenn Sie den Datenbanknamen in der Datei "Web.config" Einfügen wurde je genutzt, bevor auf Ihrem Computer eine Datenbank bereits mit einigen Tabellen vorhanden sein kann. Wählen Sie einen neuen Namen, die nicht auf Ihrem Computer vor der Änderung verwendet wurde die *"Web.config"* Datei, zeigen Sie auf diesem neuen Datenbanknamen verwenden. Als Alternative können Sie [SQL Server Express-Hilfsprogramm](https://www.microsoft.com/download/details.aspx?DisplayLang=en&amp;id=3990) oder [SQL Server Management Studio Express](https://www.microsoft.com/download/details.aspx?displaylang=en&amp;id=7593) zum Löschen der vorhandenen Datenbank.

## <a name="sql-error-when-a-script-attempts-to-create-users-or-roles"></a>SQL-Fehler, wenn ein Skript versucht, Benutzer oder Rollen erstellen

### <a name="scenario"></a>Szenario

Verwenden Sie die datenbankbereitstellung auf konfiguriert die **SQL packen/veröffentlichen** Registerkarte SQL-Skripts, die während der Bereitstellung ausgeführt enthalten Create User oder Create Role Befehle und Skripts Ausführung ein Fehler auftritt, wenn diese Befehle ausgeführt werden. Detailliertere Meldungen, wie im folgenden wird möglicherweise angezeigt:

[!code-console[Main](troubleshooting/samples/sample8.cmd)]

Wenn dieser Fehler tritt auf, wenn Sie die datenbankbereitstellung im konfiguriert haben die **Webveröffentlichung** Assistenten anstelle der **SQL packen/veröffentlichen** Registerkarte, erstellen Sie einen Thread in der [Konfiguration und Bereitstellung](https://forums.asp.net/26.aspx/1?Configuration+and+Deployment) Forum und die Lösung wird auf dieser Seite zur Problembehandlung hinzugefügt werden.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Berechtigung zum Erstellen von Benutzern bzw. Rollen keine für das Benutzerkonto an, das Sie verwenden, um die Bereitstellung durchzuführen. Beispielsweise kann der Hostinganbieter die Datenbank zuweisen\_Datareader, Db\_Datawriter, und Db\_Ddladmin Rollen mit dem Benutzerkonto, das für Sie eingerichtet. Diese sind ausreichend für die meisten Datenbankobjekte erstellen, aber nicht für die Erstellung von Benutzern oder Rollen. Eine Möglichkeit zur Vermeidung des Fehlers ist durch das Ausschließen von Benutzern und Rollen von der Bereitstellung der Datenbank. Hierzu bearbeiten das PreSource-Element für automatisch generierte Skript die Datenbank, die folgenden Attribute enthalten:

[!code-console[Main](troubleshooting/samples/sample9.cmd)]

Informationen dazu, wie Sie das PreSource-Element in der Projektdatei bearbeiten, finden Sie unter [Vorgehensweise: Bearbeiten der Bereitstellungseinstellungen in der Projektdatei](https://msdn.microsoft.com/library/ff398069(v=vs.100).aspx). Wenn die Benutzer oder Rollen in der Entwicklungsdatenbank in der Zieldatenbank müssen, wenden Sie sich an dem Hostinganbieter zur Verfügung, um Unterstützung zu erhalten.

## <a name="sql-server-timeout-error-when-running-custom-scripts-during-deployment"></a>SQL Server Timeout-Fehler beim Ausführen von benutzerdefinierten Skripts während der Bereitstellung

### <a name="scenario"></a>Szenario

Sie haben angegeben, benutzerdefinierte SQL-Skripts, die während der Bereitstellung ausgeführt, und wenn Web Deploy sie ausgeführt wird, sie ein Timeout.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Die Ausführung mehrerer Skripts, die verschiedene Modi kann zu Timeout-Fehlern führen. Standardmäßig automatisch generierten Skripts, die in einer Transaktion ausgeführt, aber nicht für benutzerdefinierte Skripts. Bei Auswahl der **Daten und/oder Schema aus einer vorhandenen Datenbank extrahieren** option die **SQL packen/veröffentlichen** Registerkarte, wenn Sie ein benutzerdefinierte SQL-Skript hinzufügen, müssen Sie für das Transaktionsprotokoll auf einige Skripts ändern, damit Alle Skripts verwenden die gleichen transaktionseinstellungen. Weitere Informationen finden Sie unter [Vorgehensweise: Bereitstellen einer Datenbank mit einem Webanwendungsprojekt](https://msdn.microsoft.com/library/dd465343.aspx).

Wenn Sie für das Transaktionsprotokoll konfiguriert haben, sodass alle identisch sind, aber weiterhin diese Fehlermeldung erhalten, ist eine mögliche problemumgehung, um die Skripts einzeln auszuführen. In der **Datenbankskripts** Raster in der **packen/veröffentlichen** SQL Registerkarte die **Include** für das Skript, das den Timeoutfehler bewirkt, dass das Kontrollkästchen klicken Sie dann das Projekt veröffentlichen. Wechseln Sie zurück in die **Datenbankskripts** Raster auswählen des Skripts **Include** Kontrollkästchen, und Deaktivieren der **einschließen** Kontrollkästchen für die anderen Skripts. Veröffentlichen Sie das Projekt dann erneut. Dieses Mal, wenn Sie veröffentlicht haben, wird nur die ausgewählten benutzerdefinierten Skripts ausgeführt.

## <a name="stream-data-of-site-manifest-is-not-yet-available"></a>Stream-Daten von Website-Manifest ist noch nicht verfügbar

### <a name="scenario"></a>Szenario

Wenn Sie Installieren eines Pakets mithilfe der *"Deploy.cmd"* Datei mit der Option t (Test), die Sie sehen der folgende Fehlermeldung angezeigt:

Fehler: Das Streamen von Daten von ' Sitemanifest/DbFullSql [@path= 'C:\TEMP\AdventureWorksGrant.sql']/sqlScript' ist noch nicht verfügbar.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Die Fehlermeldung bedeutet, dass der Befehl erstellt einen Testbericht kann nicht an. Allerdings kann den Befehl ausführen, bei der Verwendung der Option y (tatsächlichen Installation). Die Meldung gibt es nur an, dass ein Problem mit dem Befehl im Testmodus vorliegt.

## <a name="this-application-requires-managedruntimeversion-v40"></a>Diese Anwendung erfordert ManagedRuntimeVersion v4. 0

### <a name="scenario"></a>Szenario

Wenn Sie versuchen, bereitzustellen, die folgende Fehlermeldung angezeigt:

Der Anwendungspool, den Sie verwenden möchten, hat die 'ManagedRuntimeVersion'-Eigenschaft auf 'v2. 0' festgelegt. Diese Anwendung erfordert "v4. 0".

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

ASP.NET 4 ist in IIS nicht installiert. Wenn der Server für die Bereitstellung werden Ihrem Entwicklungscomputer und verfügt über Visual Studio 2010 installiert, ASP.NET 4 ist auf dem Computer installiert, aber Sie werden möglicherweise nicht in IIS installiert. Öffnen Sie auf dem Server, dem Sie bereitstellen eine Eingabeaufforderung mit erhöhten Rechten, und installieren Sie ASP.NET 4 in IIS, indem Sie die folgenden Befehle ausführen:

[!code-console[Main](troubleshooting/samples/sample10.cmd)]

## <a name="unable-to-cast-microsoftwebdeploymentdeploymentprovideroptions"></a>Kann nicht Microsoft.Web.Deployment.DeploymentProviderOptions umgewandelt.

### <a name="scenario"></a>Szenario

Wenn Sie ein Paket bereitstellen, sehen Sie die folgende Fehlermeldung angezeigt:

Wandelt ein Objekt vom Typ "Microsoft.Web.Deployment.DeploymentProviderOptions', 'Microsoft.Web.Deployment.DeploymentProviderOptions' nicht möglich.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Sie versuchen, zum Bereitstellen von IIS-Manager über die Bereitstellung 1.1 Benutzeroberfläche auf einem Server mit Web Deploy 2.0 installiert. Wenn Sie im IIS-Remotezugriff-Verwaltungstool zum Bereitstellen von Importieren eines Pakets, überprüfen Sie verwenden die **neue Features verfügbar** im Dialogfeld, wenn Sie die Verbindung herstellen. (Dieses Dialogfeld können Sie möglicherweise nur einmal angezeigt, wenn die Verbindung erstmalig eingerichtet wird. Deaktivieren Sie die Verbindung, und beginnen, IIS-Manager zu schließen und neu zu starten Sie durch Eingabe von Inetmgr/reset an der Eingabeaufforderung.) Wenn eine der Funktionen aufgeführt ist **Webbenutzeroberfläche bereitstellen**, und eine Versionsnummer kleiner als 8 hat, hat des Servers, die Sie bereitstellen, möglicherweise 1.1 und 2.0-Versionen von Web Deploy installiert. Zum Bereitstellen von von einem Client, der 2.0 installiert ist, muss der Server nur Web Deploy 2.0 installiert haben. Sie müssen Ihr Hostinganbieter, um dieses Problem zu beheben. Wenden Sie sich an.

## <a name="unable-to-load-the-native-components-of-sql-server-compact"></a>Kann nicht geladen werden die systemeigenen Komponenten von SQL Server Compact

### <a name="scenario"></a>Szenario

Wenn Sie auf die bereitgestellte Website ausführen, sehen Sie die folgende Fehlermeldung angezeigt:

Fehler beim Laden der systemeigenen Komponenten von SQL Server Compact, die ADO.NET-Anbieter 8482-Version entspricht. Installieren Sie die richtige Version von SQL Server Compact. KB-Artikel 974247 für Weitere Informationen finden Sie unter.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Die bereitgestellte Website verfügt nicht über *amd64* und *X86* Unterordner mit die systemeigenen Assemblys in der sie in der Anwendung *Bin* Ordner. Bei einem Computer, SQL Server Compact installiert ist, befinden sich die systemeigenen Assemblys *C:\Program Files\Microsoft SQL Server Compact Edition\v4.0\Private*. Die beste Möglichkeit, die richtigen Dateien in den richtigen Ordnern in einem Visual Studio-Projekt zu erhalten, ist zum Installieren des Pakets von NuGet SqlServerCompact. Paketinstallation Fügt ein Skript nach dem Erstellen, kopieren Sie die systemeigenen Assemblys in *amd64* und *X86*. Damit für diese bereitgestellt werden kann müssen Sie jedoch manuell in das Projekt aufgenommen. Weitere Informationen finden Sie unter den [Bereitstellen von SQL Server Compact](../../older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) Tutorial.

## <a name="path-is-not-valid-error-after-deploying-an-entity-framework-code-first-application"></a>Fehler nach dem Bereitstellen einer Entity Framework Code First-Anwendung "Pfad ist nicht gültig."

### <a name="scenario"></a>Szenario

Bereitstellung einer Anwendung, die Entity Framework Code First-Migrationen und ein DBMS wie SQL Server Compact verwendet, die die Datenbank in eine Datei in der App gespeichert\_Datenordner. Sie verfügen über Code First-Migrationen so konfiguriert, dass um die Datenbank nach der ersten Bereitstellung zu erstellen. Wenn Sie die Anwendung ausführen, erhalten Sie eine Fehlermeldung wie im folgenden Beispiel:

Der Pfad ist ungültig. Überprüfen Sie das Verzeichnis für die Datenbank an. [Path = c:\inetpub\wwwroot\App\_Data\DatabaseName.sdf]

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Code wird zuerst die Datenbank, aber die App zu erstellen versucht\_Data-Ordner ist nicht vorhanden. Entweder Sie alle Dateien in haben nicht die *App\_Daten* Ordner, wenn Sie bereitgestellt haben, oder Sie ausgewählt **Ausschließen von App\_Daten** auf der **Web packen/veröffentlichen** Fenster "Projekteigenschaften" auf der Registerkarte. Während des Bereitstellungsvorgangs wird keinen Ordner auf dem Server erstellen, wenn keine Dateien vorhanden sind, in dem Ordner auf dem Server kopiert werden sollen. Wenn Sie bereits die Datenbank, die auf der Website eingerichtet haben, werden während des Bereitstellungsvorgangs der Dateien gelöscht und die *App\_Daten* Ordner selbst bei Auswahl **entfernen weiterer Dateien am Ziel** in Das Veröffentlichungsprofil. Um das Problem zu beheben, speichern Sie eine Platzhalterdatei, z. B. einer TXT-Datei in die *App\_Daten* Ordner stellen Sie sicher, dass Sie keine **Ausschließen von App\_Daten** ausgewählt, und stellen Sie erneut bereit.

## <a name="com-object-that-has-been-separated-from-its-underlying-rcw-cannot-be-used"></a>"COM-Objekt, das vom zugrunde liegenden RCW getrennt wurde kann nicht verwendet werden."

### <a name="scenario"></a>Szenario

Sie wurden erfolgreich mit nur einem Klick zum Bereitstellen Ihrer Anwendung zu veröffentlichen und starten dann dieser Fehler angezeigt:

Fehler bei der webbereitstellungstasks. (Die Anforderung an die URL der remote-Agent konnte nicht abgeschlossen "<https://serverurl.com/msdeploy.axd?site=sitename>".)  
 Die Anforderung an die URL der remote-Agent konnte nicht abgeschlossen "<https://url/msdeploy.axd?site=sitename>".  
Die Anforderung wurde abgebrochen: die Anforderung wurde abgebrochen.  
COM-Objekt, das vom zugrunde liegenden RCW getrennt wurde, kann nicht verwendet werden.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Schließen und Neustart von Visual Studio ist in der Regel alles, die was erforderlich ist, um diesen Fehler zu beheben.

## <a name="deployment-fails-because-user-credentials-used-for-publishing-dont-have-setacl-authority"></a>Bereitstellung schlägt fehl, da Benutzer Anmeldeinformationen zum Veröffentlichen, müssen keine SetACL Autorität

### <a name="scenario"></a>Szenario

Veröffentlichung tritt ein, mit der ein Fehler, Sie haben keine Autorität zum Einrichten von Berechtigungen für Ordner (das Benutzerkonto an, das Sie verwenden keine SetACL Authority).

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Standardmäßig legt der Visual Studio Leseberechtigungen für den Stammordner der Website und Schreibberechtigungen für die App\_Datenordner. Wenn Sie wissen, dass die Standardberechtigungen für Website-Ordner richtig sind und müssen nicht festgelegt werden soll, deaktivieren Sie dieses Verhalten durch Hinzufügen von **&lt;IncludeSetACLProviderOn Ziel&gt;"false"&lt;/ IncludeSetACLProviderOnDestination&gt;** der veröffentlichungsprofildatei (um die Auswirkung auf die ein einzelnes Profil) oder der wpp.targets-Datei (für alle Profile gelten). Informationen dazu, wie Sie diese Dateien zu bearbeiten, finden Sie unter [Vorgehensweise: Bearbeiten der Bereitstellungseinstellungen in Veröffentlichungsprofildateien (.pubxml) Dateien](https://msdn.microsoft.com/library/ff398069.aspx).

## <a name="access-denied-errors-when-the-application-tries-to-write-to-an-application-folder"></a>Zugriff verweigert wird, wenn die Anwendung versucht, die in einen Anwendungsordner zu schreiben

### <a name="scenario"></a>Szenario

Ihre Anwendungsfehler, wenn versucht wird, erstellen oder Bearbeiten einer Datei in einen von der Anwendung, da sie nicht über die Autorität für die Schreibzugriff für diesen Ordner verfügt.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Standardmäßig legt der Visual Studio Leseberechtigungen für den Stammordner der Website und Schreibberechtigungen für die App\_Datenordner. Wenn Ihre Anwendung über Schreibzugriff auf einen untergeordneten Ordner benötigt, können Sie Berechtigungen für diesen Ordner festlegen, wie in den Ordner festlegen von Berechtigungen und Bereitstellen von in die Produktionsumgebung Tutorials in dieser Reihe dargestellt. Wenn Ihre Anwendung über Schreibzugriff auf den Stammordner der Website erfordert, müssen Sie schreibgeschützten Zugriff auf den Stammordner festlegen, durch das Hinzufügen verhindert **&lt;IncludeSetACLProviderOn Ziel&gt;"false"&lt;/ IncludeSetACLProviderOnDestination&gt;** der veröffentlichungsprofildatei (um die Auswirkung auf die ein einzelnes Profil) oder der wpp.targets-Datei (für alle Profile gelten). Informationen dazu, wie Sie diese Dateien zu bearbeiten, finden Sie unter [Vorgehensweise: Bearbeiten der Bereitstellungseinstellungen in Veröffentlichungsprofildateien (.pubxml) Dateien](https://msdn.microsoft.com/library/ff398069.aspx).

<a id="aspnet45error"></a>

## <a name="configuration-error---targetframework-attribute-references-a-version-that-is-later-than-the-installed-version-of-the-net-framework"></a>Fehler bei der Konfiguration - TargetFramework-Attributs verweist auf eine Version, die älter als die installierte Version von .NET Framework

### <a name="scenario"></a>Szenario

Sie wurde erfolgreich veröffentlicht ein Webprojekt, das auf ASP.NET 4.5 abzielt, aber beim Ausführen der Anwendungs (mit den CustomErrors-Modus auf "off" in der Datei Web.config festgelegt) erhalten Sie die folgende Fehlermeldung:

Die "TargetFramework"-Attribut in der &lt;Kompilierung&gt; -Element der Datei "Web.config" wird nur für die Ziel-Version 4.0 und höher von .NET Framework verwendet (z. B. "&lt;Kompilierung TargetFramework ="4.0"&gt;'). Das Attribut "TargetFramework" verweist auf derzeit eine Version, die älter als die installierte Version von .NET Framework. Geben Sie eine gültige Zielversion von .NET Framework, oder installieren Sie die erforderliche Version von .NET Framework.

Die Quelle Fehlermeldungsfeld der Fehlerseite hebt die folgende Zeile aus "Web.config" als Ursache des Fehlers:

&lt;compilation targetFramework="4.5" /&gt;

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Der Server unterstützt nicht ASP.NET 4.5. Wenden Sie sich an den Hostinganbieter, um zu bestimmen, wann und ob die Unterstützung für ASP.NET 4.5 hinzugefügt werden kann. Wenn Sie den Server ein Upgrade nicht möglich ist, müssen Sie ein Webprojekt bereitstellen, die auf ASP.NET 4 oder früher abzielt stattdessen.

Wenn Sie eine ASP.NET 4 oder früher Webprojekt für dasselbe Ziel bereitstellen, wählen Sie die **entfernen weiterer Dateien am Ziel** auf das Kontrollkästchen der **Einstellungen** Registerkarte die **"Web veröffentlichen"** Assistenten. Wenn Sie nicht auswählen **entfernen weiterer Dateien am Ziel**, wird weiterhin die Fehler bei der Konfiguration-Seite.

Das Projekt **Eigenschaften** Windows enthält die Dropdownliste für eine Ziel-Framework, aber dieses Problem kann nicht aufgelöst werden, indem Sie einfach, die von **.NET Framework 4.5** zu **.NET Framework 4**. Wenn Sie das Zielframework in einer früheren Frameworkversion ändern, wird das Projekt wird immer noch Verweise auf die neuere Frameworkversion-Assemblys und wird nicht ausgeführt. Sie müssen manuell ändern Sie diese Verweise, oder Erstellen eines neuen Projekts, das auf .NET Framework 4 oder früher abzielt. Weitere Informationen finden Sie unter [.NET Framework als Ziel für Websites](https://msdn.microsoft.com/library/bb398791(v=vs.100).aspx).

## <a name="medium-trust-errors"></a>Fehler von mittlerer Vertrauenswürdigkeit

### <a name="scenario"></a>Szenario

Wenn Sie Ihre Anwendung in der Produktion ausführen, ruft er einen Fehler im Zusammenhang mit mittlerer Vertrauenswürdigkeit ab.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

Viele Drittanbieter-Hostinganbieter führen Sie Ihre Website in mittlerer Vertrauenswürdigkeit, d. Es gibt einige Dinge h. zu tun ist unzulässig. Z. B. kann nicht Anwendungscode auf die Windows-Registrierung zugreifen und kann nicht lesen oder Schreiben von Dateien, die außerhalb der Ordnerhierarchie Ihrer Anwendung sind. In der Standardeinstellung die Anwendung ausgeführt wird, im *volle Vertrauenswürdigkeit* auf dem lokalen Computer, was bedeutet, dass es sich bei die Anwendung möglicherweise mehr Möglichkeiten, die fehlschlagen, wenn Sie sie in der produktionsumgebung bereitstellen können.

Sie können die Anwendung mittlerer Vertrauenswürdigkeit in der lokalen IIS-Umgebung ausgeführt werden soll, um die Problembehandlung konfigurieren. Zu diesem Zweck öffnen Sie die Anwendung *"Web.config"* Datei, und fügen eine **Vertrauensstellung** Element in der **"System.Web"** Element, wie im folgenden Beispiel gezeigt.

[!code-xml[Main](troubleshooting/samples/sample11.xml)]

Die Anwendung wird jetzt auch auf dem lokalen Computer mittlerer Vertrauenswürdigkeit in IIS ausgeführt.

Nicht geschieht, wenn Sie in Azure App Service bereitstellen werden, da Azure nicht mit mittleren Vertrauenswürdigkeit erfordert. Zu dem Zeitpunkt, die in diesem Tutorial im Februar 2012 geschrieben wird verursacht mit dieser Methode zu Ihrer Anwendung bei mittlerer Vertrauenswürdigkeit ausgeführt wird, einen Fehler in Azure.

Wenn Sie Entity Framework Code First-Migrationen verwenden, und Sie bereitstellen für einen Hostinganbieter, der Ihre Anwendung bei mittlerer Vertrauenswürdigkeit ausgeführt wird, stellen Sie sicher, dass Sie Version 5.0 oder höher installiert. In Entity Framework, Version 4.3 Migrationen erfordert volles Vertrauen um das Datenbankschema zu aktualisieren.

## <a name="http-40417-not-found-error"></a>HTTP 404.17 wurde nicht gefunden

### <a name="scenario"></a>Szenario

Beim Ausführen der bereitgestellten Website auf Ihrem Entwicklungscomputer in IIS, sehen Sie die folgende Fehlermeldung angezeigt, die meldet, dass der Server "default.aspx" nicht verarbeiten kann:

HTTP Error 404.17 –, die nicht gefunden.

Der angeforderte Inhalt Skript, und es wird nicht vom Handler statische Dateien verarbeitet werden.

### <a name="possible-cause-and-solution"></a>Mögliche Ursache und Lösung

ASP.NET 4.5 möglicherweise nicht auf Ihrem Computer installiert. Finden Sie die Schritte in der Bereitstellung von IIS als Testumgebung Tutorial dieser Reihe, die erläutern, wie Sie ASP.NET 4.5 zu installieren.

> [!div class="step-by-step"]
> [Vorherige](deploying-extra-files.md)
