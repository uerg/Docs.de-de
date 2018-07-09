---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatisieren Sie alles (erstellen realer Cloud-Apps mit Azure) | Microsoft-Dokumentation
author: MikeWasson
description: Die Building Real World Cloud Apps mit Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: 87b697847845ab88943e7a659ccd9487b9408d5a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37839634"
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatisieren Sie alles (erstellen realer Cloud-Apps mit Azure)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download korrigieren Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps mit Azure** e-Book basiert darauf, dass eine Präsentation von Scott Guthrie entwickelt wurde. Es wird erläutert, 13 Muster und Methoden, die Ihnen helfen können, werden erfolgreiche Entwicklung von Web-apps für die Cloud. Eine Einführung in das e-Book, finden Sie unter [im ersten Kapitel](introduction.md).


Die ersten drei Muster, denen wir betrachten werde wenden nämlich für alle Software-Entwicklungsprojekt, sondern vor allem für Cloud-Projekte. Dieses Muster eignet sich zur Automatisierung von Aufgaben bei der Anwendungsentwicklung. Es ist ein wichtiges Thema, da der manuelle Prozesse langsame und fehleranfällige sind; Automatisieren beliebig viele wie möglich einen schnellen, zuverlässigen und agilen Workflow einrichten können. Es ist eindeutig wichtig ist, für die Cloudentwicklung, da Sie viele Aufgaben leicht, die schwierig oder unmöglich automatisieren können, die in einer lokalen Umgebung zu automatisieren. Sie können z. B. festlegen, um den gesamten Test Umgebungen, einschließlich der neuen Webserver und Back-End-VMs, Datenbanken, blob-Speicher (Datei), Warteschlangen usw.

## <a name="devops-workflow"></a>DevOps-Workflow

Hören Sie immer den Begriff "DevOps". Der Begriff entwickelt aus darauf, dass Sie Entwicklungs- und Betriebsaufgaben in Verbindung zu integrieren, um die Software effizient entwickeln müssen. Die Art des Workflows, die Sie aktivieren möchten ist eine in der Sie können eine app entwickeln, bereitstellen, Lernen von produktiven Einsatz des Zertifikats, ändern Sie ihn als Reaktion auf was Sie gelernt haben, und wiederholen Sie den Zyklus schnell und zuverlässig.

Einige erfolgreiche Cloud-Entwicklerteams bereitstellen mehrmals täglich um eine live-Umgebung. Das Azure-Team verwendet, um eine größere bereitzustellen aktualisieren 2 bis 3 Monate, aber jetzt geringfügige Updates von Versionen jeder 2 bis 3 Tage und die Hauptversionsnummer Versionen 2 und 3-Wochen. Dieser Rhythmus eingehe ist wirklich Sie reagieren auf Kundenfeedback hilfreich.

Zu diesem Zweck müssen Sie einen Zyklus für Entwicklung und Bereitstellung zu aktivieren, der wiederholbare, zuverlässige und vorhersagbare und niedrige Zykluszeit ist.

![DevOps-workflow](automate-everything/_static/image1.png)

Die Zeitspanne zwischen Wenn Sie eine Idee für eine Funktion haben, und wenn die Kunden verwenden und Bereitstellen von Feedback muss also so kurz wie möglich sein. Der ersten drei Muster – automatisieren Sie alles, quellcodeverwaltung und continuous Integration und Delivery – werden alle Informationen zu bewährten Methoden zur Verfügung, die wir empfehlen, um diese Art von Vorgang.

## <a name="azure-management-scripts"></a>Azure-Verwaltungsskripts

In der [dieses e-Book – Einführung](introduction.md), Sie haben gesehen, die webbasierte Konsole, das Azure-Verwaltungsportal. Das Verwaltungsportal können Sie zum Überwachen und Verwalten aller Ressourcen, die Sie in Azure bereitgestellt haben. Es ist eine einfache Möglichkeit zum Erstellen und Löschen von Diensten wie Web-apps und VMs, konfigurieren Sie diese Dienste, Dienstvorgang zu überwachen und so weiter. Es eignet sich hervorragend, aber seine Verwendung ist ein manueller Prozess. Wenn Sie wollen eine produktionsanwendung beliebiger Größe zu entwickeln, und insbesondere in einer teamumgebung sollten Sie die Benutzeroberfläche, um zu erfahren, und erkunden Sie Azure-Portals durchlaufen, und klicken Sie dann die Vorgänge automatisieren, die Sie wiederholt durchführen müssen.

Nahezu alle, die Sie manuell im Verwaltungsportal oder über Visual Studio können kann auch durch Aufrufen der REST-Verwaltungs-API erfolgen. Sie können mithilfe von Skripts schreiben [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), oder Sie können ein open-Source-Framework wie z. B. [Chef](http://www.opscode.com/chef/) oder [Puppet](http://puppetlabs.com/puppet/what-is-puppet). Sie können auch das Befehlszeilentool "Bash" in einer Mac- oder Linux-Umgebung verwenden. Azure verfügt über Skript-APIs für alle diese verschiedenen Umgebungen und auf ihm ein [.NET Verwaltungs-API](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) für den Fall, dass Code anstelle von Skript geschrieben werden soll.

Für die Fix It-app haben wir einige Windows PowerShell-Skripts, die die Prozesse von einer testumgebung erstellen und Bereitstellen des Projekts für diese Umgebung zu automatisieren erstellt, und wir werden einige der den Inhalt des Skripts überprüfen.

## <a name="environment-creation-script"></a>Das Skript zum Umgebung erstellen

Das erste Skript, wir betrachten, den Namen *New-AzureWebsiteEnv.ps1*. Erstellt eine Azure-Umgebung, dass Sie den Fix It app zu Testzwecken bereitstellen können. Die wichtigsten Aufgaben, die dieses Skript führt lauten wie folgt:

- Erstellen einer Web-app.
- Erstellen Sie ein Speicherkonto. (Erforderlich, für Blobs und Warteschlangen, wie Sie in späteren Kapiteln sehen werden.)
- Erstellen Sie eine SQL-Datenbankserver und zwei Datenbanken: eine Datenbank und eine Mitgliedschaftsdatenbank.
- Store Einstellungen in Azure, die die app verwendet wird, auf die Storage-Konto und die Datenbanken zugreifen.
- Erstellen der Einstellungsdateien an, die zum Automatisieren der Bereitstellung verwendet werden.

### <a name="run-the-script"></a>Führen Sie das Skript


> [!NOTE]
> Dieser Teil des Kapitels zeigt Beispiele für Skripts und Befehle, die Sie eingeben, um sie auszuführen. Diese Demo auch nicht alles, was Sie brauchen kennen, um die Skripts ausgeführt werden. How-to--It-Anleitung finden Sie [Anhang: die beheben Sie diese Beispielanwendung](the-fix-it-sample-application.md#deploybase).


Führen Sie ein PowerShell-Skript, das Azure-Dienste verwaltet, müssen Sie die Azure PowerShell-Konsole installieren und konfigurieren sie für die Arbeit mit Ihrem Azure-Abonnement. Nachdem Sie eingerichtet haben, können Sie das Fix It-Umgebung-Erstellungsskript mit einem Befehl wie diesen ausführen:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

Die `Name` Parameter gibt den Namen verwendet werden, wenn die Datenbank und Storage-Konten erstellen und die `SqlDatabasePassword` Parameter gibt das Kennwort für das Administratorkonto ein, die für SQL-Datenbank erstellt werden. Es gibt andere Parameter, die, denen Sie verwenden können, die wir später betrachten werde.

![PowerShell-Fenster](automate-everything/_static/image2.png)

Nach Abschluss der skriptausführung können Sie im Verwaltungsportal sehen, was erstellt wurde. Finden Sie zwei Datenbanken:

![Databases](automate-everything/_static/image3.png)

Ein Speicherkonto:

![Storage-Konto](automate-everything/_static/image4.png)

Und eine Web-app:

![Website](automate-everything/_static/image5.png)

Auf der **konfigurieren** Registerkarte für die Web-app sehen Sie, dass die speicherkontoeinstellungen hat und SQL-Datenbank-Verbindungszeichenfolgen Sie für die Lösung diese app richten.

!["appSettings" und connectionStrings](automate-everything/_static/image6.png)

Die *Automation* Ordner jetzt enthält auch eine  *&lt;Websitename&gt;pubxml* Datei. Diese Datei speichert die Einstellungen, die MSBuild verwenden werden, um die Bereitstellung der Anwendung in der Azure-Umgebung, die gerade erstellt haben. Zum Beispiel:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Wie Sie sehen können, das Skript hat eine vollständigen Test-Umgebung erstellt, und der gesamte Prozess in ungefähr 90 Sekunden erfolgt.

Wenn jemand in Ihrem Team eine testumgebung erstellen möchte, können sie einfach das Skript ausführen. Das ist, nicht nur, sondern auch sie davon überzeugt, dass sie eine identisch mit der Umgebung verwenden, die Sie verwenden werden können. Sie konnten nicht ganz so sicher, wenn alle Schritte manuell einrichten wurde durch mithilfe des Verwaltungsportals UI sein.

### <a name="a-look-at-the-scripts"></a>Ein Blick auf die Skripts

Es gibt tatsächlich drei Skripts, die diese Aufgabe übernimmt. Sie rufen Sie eine von der Befehlszeile aus, und verwendet er automatisch die anderen beiden Sie einige der Aufgaben:

- *Neue AzureWebSiteEnv.ps1* enthält das Hauptskript.

    - *Neue AzureStorage.ps1* erstellt das Speicherkonto.
    - *Neue AzureSql.ps1* die Datenbanken erstellt.

### <a name="parameters-in-the-main-script"></a>Parameter in das Hauptskript

Das Hauptskript *New-AzureWebSiteEnv.ps1*, mehrere Parameter definiert:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

Es sind zwei Parameter erforderlich:

- Der Name der Web-app, die das Skript erstellt werden soll. (Dies wird auch verwendet, für die URL: `<name>.azurewebsites.net`.)
- Das Kennwort für den Administrator des Datenbankservers, der das Skript erstellt werden soll.

Optionale Parameter können Sie den Standort des Rechenzentrums (standardmäßig "West US"), Name des Datenbankservers Administrator (standardmäßig "Dbuser") und eine Firewallregel für den Datenbankserver angeben.

### <a name="create-the-web-app"></a>Erstellen Sie die Web-app

Das erste, was das Skript wird die Web-app zu erstellen, indem die `New-AzureWebsite` -Cmdlet übergeben, die Web-app-Parameter-Werte für Name und Speicherort:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Erstellen des Speicherkontos

Wird, das Hauptskript ausgeführt der <em>New-AzureStorage.ps1</em> Skript zu erstellen, angeben "<em>&lt;Websitename&gt;</em>Speicher" für den Namen des Speicherkontos, und den gleichen Standort des Rechenzentrums als die Web-app.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*Neue AzureStorage.ps1* Aufrufe der `New-AzureStorageAccount` -Cmdlet zum Erstellen von Speicherkonto, und es gibt das Konto und -Schlüsselwerte. Die Anwendung benötigen diese Werte, um die Blobs und Warteschlangen im Speicherkonto zugreifen zu können.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Sie können nicht immer ein neues Speicherkonto erstellen möchten; Sie können das Skript verbessern, durch das Hinzufügen eines Parameters, das optional weist ein vorhandenes Speicherkonto verwenden.

### <a name="create-the-databases"></a>Erstellen Sie die Datenbanken

Das Hauptskript führt dann das datenbankerstellungsskript, *New-AzureSql.ps1*, nach dem Einrichten der Standarddatenbank und firewall Regelnamen:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

Das datenbankerstellungsskript Ruft ab, den Entwickler-Computer-IP-Adresse und eine Firewallregel festgelegt, damit der Entwickler-Computer herstellen und des Servers verwalten kann. Das Skript zum Erstellen der Datenbank durchläuft dann verschiedene Schritte zum Einrichten von Datenbanken:

- Der Server erstellt, mit der `New-AzureSqlDatabaseServer` Cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Erstellt die Firewallregeln zum Aktivieren der Entwickler-Computer, die zum Verwalten des Servers, und aktivieren Sie die Web-app eine Verbindung damit herstellen. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Erstellt einen Datenbankkontext, der den Servernamen und Anmeldeinformationen enthält, mit der `New-AzureSqlDatabaseServerContext` Cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` ist eine Funktion im Skript, das Aufrufen der `ConvertTo-SecureString` Cmdlet verschlüsselt das Kennwort und gibt eine `PSCredential` Objekt vom selben Typ, der die `Get-Credential` -Cmdlet wird zurückgegeben.
- Erstellt die Datenbank und die Mitgliedschaftsdatenbank mithilfe der `New-AzureSqlDatabase` Cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Ruft eine lokal definierte Funktion Tocreates eine Verbindungszeichenfolge für die einzelnen Datenbanken an. Die Anwendung wird diese Verbindungszeichenfolgen verwenden, um den Zugriff auf Datenbanken. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString ist eine Funktion, die im Skript definiert, das die Verbindungszeichenfolge aus der Parameterwerte, die für sie erstellt.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Gibt eine Hashtabelle mit den Namen des Datenbankservers und die Verbindungszeichenfolgen zurück.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

Die Fix It-app verwendet separate Mitgliedschafts- und dienstanwendungs-Datenbanken. Es ist auch möglich, Daten von Mitgliedschafts- und Anwendungsdaten in einer einzelnen Datenbank abgelegt werden sollen.

### <a name="store-app-settings-and-connection-strings"></a>Store-app-Einstellungen und Verbindungszeichenfolgen

Azure verfügt über ein Feature, können Sie zum Speichern von Einstellungen und Verbindungszeichenfolgen, die automatisch zu überschreiben, was für die Anwendung zurückgegeben wird, wenn versucht wird, lesen Sie, die `appSettings` oder `connectionStrings` Auflistungen in der Datei "Web.config". Dies ist eine Alternative zum Anwenden von [Web.config-Transformationen](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) bei der Bereitstellung. Weitere Informationen finden Sie unter [sensible Daten im Azure Store](source-control.md#appsettings) weiter unten in diesem e-Book.

Das Skript zum Erstellen der Umgebung in Azure alle speichert die `appSettings` und `connectionStrings` Werte, die die Anwendung muss Zugriff auf das Speicherkonto und die Datenbanken, bei der Ausführung in Azure.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/) ist ein Framework, Telemetriedaten, die wir in veranschaulichen die [Überwachung und Telemetrie](monitoring-and-telemetry.md) Kapitel. Das Skript zum Erstellen der Umgebung wird auch die Web-app, um sicherzustellen, dass er die New Relic-Einstellungen übernimmt neu gestartet.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Vorbereiten für die Bereitstellung

Das Skript zum Erstellen der Umgebung wird am Ende des Prozesses zwei Funktionen zum Erstellen von Dateien, die vom Bereitstellungsskript verwendet werden.

Eine dieser Funktionen ein Veröffentlichungsprofil erstellt *(&lt;Websitename&gt;pubxml* Datei). Der Code Ruft die Azure-REST-API zum Abrufen der Einstellungen für die Veröffentlichung, und speichert die Informationen in einem *.publishsettings* Datei. Anschließend er die Informationen aus dieser Datei zusammen mit einer Vorlagendatei verwendet (*pubxml.template*) zum Erstellen der *pubxml* -Datei, die das Veröffentlichungsprofil enthält. Dieser zweistufige Prozess simuliert, was Sie in Visual Studio ausführen: Herunterladen einer *.publishsettings* Datei und importieren, um ein Veröffentlichungsprofil erstellen.

Die andere Funktion verwendet einen anderen Vorlagendatei (Website-environment.template) zum Erstellen einer *Website-environment.xml* die Einstellungen enthält, verwenden Sie das Bereitstellungsskript wird zusammen mit, der *pubxml*Datei.

### <a name="troubleshooting-and-error-handling"></a>Problembehandlung und Fehlerbehandlung

Skripts sind, wie Programme: Diese können erfolglos sein, und wenn dies der Fall möchten Sie wissen, wie Sie über den Fehler und die Ursache können. Aus diesem Grund ändert das Skript zum Erstellen der Umgebung den Wert für die `VerbosePreference` befehlsshellvariable `SilentlyContinue` zu `Continue` , damit alle ausführliche Meldungen angezeigt werden. Er ändert auch den Wert des der `ErrorActionPreference` befehlsshellvariable `Continue` zu `Stop`, damit das Skript beendet, selbst wenn sie Fehler ohne Abbruch auftritt:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Bevor sie alle Vorgänge ausführt, speichert das Skript die Startzeit, damit es die verstrichene Zeit berechnen kann, wenn es abgeschlossen ist:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Nachdem sie ihre Arbeit abgeschlossen hat, zeigt das Skript die verstrichene Zeit:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

Und für jede Schlüsselvorgang schreibt das Skript ausführliche Meldungen, z. B.:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Bereitstellungsskript

Was die *New-AzureWebsiteEnv.ps1* -Skripts für die umgebungserstellung, die *veröffentlichen-AzureWebsite.ps1* -Skripts für die anwendungsbereitstellung.

Das Bereitstellungsskript Ruft den Namen der Web-app aus dem *Website-environment.xml* Datei, die durch das Skript zum Erstellen der Umgebung erstellt.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Es ruft das Kennwort für die Bereitstellung Benutzer aus der *.publishsettings* Datei:

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Er führt die [MSBuild](http://msbuildbook.com/) -Befehl, der erstellt und das Projekt bereitgestellt:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

Und wenn Sie angegeben haben die `Launch` Parameter in der Befehlszeile angegeben wird, ruft der `Show-AzureWebsite` Cmdlet, um Ihren Standardbrowser mit Website-URL zu öffnen.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

Sie können das Bereitstellungsskript mit einem Befehl wie diesen ausführen:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

Und wenn dies abgeschlossen ist, der Browser geöffnet wird, mit dem Standort ausgeführt wird, in der Cloud Die `<websitename>.azurewebsites.net` URL.

![Beheben sie die app, die in Windows Azure bereitgestellt werden.](automate-everything/_static/image7.png)

## <a name="summary"></a>Zusammenfassung

Mit diesen Skripts können Sie sicher sein, dass immer die gleichen Schritte in der gleichen Reihenfolge mit den gleichen Optionen ausgeführt werden. Dadurch wird sichergestellt, dass jeder Entwickler im Team nicht etwas verpassen etwas manipuliert oder benutzerdefinierte auf seinem eigenen Computer, die praktisch die gleiche Weise in ein anderes Teammitglied Umgebung oder in der Produktion nicht funktioniert etwas bereitstellen.

Auf ähnliche Weise können Sie die meisten Azure-Verwaltungsfunktionen automatisieren, die Sie, im Verwaltungsportal, ausführen können mithilfe der REST-API, Windows PowerShell-Skripts, einer .NET-Sprache verwendet API oder ein Bash-Hilfsprogramm, das Ausführen unter Linux oder Mac.

In der [im nächsten Kapitel](source-control.md) wir sehen Sie sich den Quellcode und erläutern, warum es wichtig, Ihre Skripts in Ihrem Quellcoderepository einzuschließen ist.

## <a name="resources"></a>Ressourcen

- [Installieren und konfigurieren Sie Windows PowerShell für Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Erläutert, wie Sie Azure PowerShell-Cmdlets installieren und wie Sie das Zertifikat zu installieren, dass Sie auf Ihrem Computer zum Verwalten von Azure berücksichtigen müssen. Dies ist ein idealer Ausgangspunkt, um zu beginnen, da sie auch Links zu Ressourcen zum Erlernen von PowerShell selbst verfügt.
- [Azure-Skriptcenter](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). WindowsAzure.com-Portal, um Ressourcen zum Entwickeln von Skripts, die zur Verwaltung von Azure-Dienste mit Links zu Erste Schritte-Tutorials, Cmdlet-Referenz Dokumentationen und Quellcode-Code und Beispielskripts
- [Wochenende Skripter: Erste Schritte mit Azure und PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). In einem Blog für Windows PowerShell bietet dieser Beitrag eine hervorragende Einführung in die Verwendung von PowerShell für Azure-Management-Funktionen.
- [Installieren und Konfigurieren der plattformübergreifenden Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Erste-Schritte-Tutorial für ein Azure Skript-Framework, die auf Mac und Linux sowie Windows-Systemen funktioniert.
- [Befehlszeilen-Tools-Abschnitt des Themas Herunterladen der Azure-SDKs und-Tools](https://azure.microsoft.com/downloads/). Portalseite für Dokumentation und Downloads, die im Zusammenhang mit Befehlszeilen-Tools für Azure.
- [Automatisieren alles, was mit der Azure-Verwaltungsbibliotheken und .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman führt die .NET Verwaltungs-API für Azure.
- [Mithilfe von Windows PowerShell-Skripts zum Veröffentlichen in Entwicklungs- und Testumgebungen](https://msdn.microsoft.com/library/azure/dn642480.aspx). MSDN-Dokumentation, die erklärt, wie Sie mit Skripts, die Visual Studio generiert automatisch für Webprojekte zu veröffentlichen.
- [PowerShell-Tools für Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Visual Studio-Erweiterung, die sprachunterstützung für die Windows PowerShell in Visual Studio hinzugefügt.

> [!div class="step-by-step"]
> [Zurück](introduction.md)
> [Weiter](source-control.md)
