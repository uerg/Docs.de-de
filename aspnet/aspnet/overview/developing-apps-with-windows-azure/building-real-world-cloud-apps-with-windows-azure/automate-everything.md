---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatisieren Sie alles (Real-World Cloud Apps with Azure erstellen) | Microsoft Docs
author: MikeWasson
description: "Die Building Real World Cloud Apps with Azure-e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die er können..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: aa8bb895ed6eaa0ef4c5752f475ea7c911544ef2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatisieren Sie alles (Real-World Cloud Apps with Azure erstellen)
====================
durch [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Download Behebungsskript Projekt](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) oder [E-Book herunterladen](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Die **Building Real World Cloud Apps with Azure** e-Book basiert auf einer Präsentation von Scott Guthrie entwickelt. Es wird erläutert, 13 Muster und Vorgehensweisen, die Ihnen helfen können erfolgreich ausgeführt entwickeln Web-apps für die Cloud. Eine Einführung in die e-Book herunterladen, finden Sie unter [im Kapitel über das erste](introduction.md).


Die ersten drei Muster, die beim weiteren wenden nämlich alle Software-Entwicklungsprojekt, vor allem aber Cloud-Projekte. Dieses Muster wird zur Automatisierung von Aufgaben bei der Anwendungsentwicklung. Es ist ein wichtiges Thema, da der manuelle Prozesse langsam und fehleranfällig sind; Automatisieren so viele wie möglich hilft, die eine schnelle, zuverlässige und agile-Workflows einrichten. Es ist eindeutig für Cloudentwicklung da problemlos viele Aufgaben automatisiert werden können, die schwer oder gar nicht in einer lokalen Umgebung zu automatisieren. Sie können z. B. gesamte Test einrichten Umgebungen neuer Web-Server und die Back-End-VMs, Datenbanken, blob-Speicher (Dateispeicher), Warteschlangen usw.

## <a name="devops-workflow"></a>DevOps-Workflow

Hören Sie immer den Begriff "DevOps". Der Begriff entwickelt, aus einer Erkennung von Entwicklungs-und Vorgänge zu integrieren, um Software effizient zu entwickeln. Die Art des Workflows, die Sie aktivieren möchten, ist in der können eine app entwickeln, bereitstellen, lerngrundlage Einsatz des Zertifikats in der Produktion, als Antwort auf das Gelernte zu ändern, und wiederholen Sie den Zyklus schnell und zuverlässig.

Einige Entwicklungsteams erfolgreich Cloud bereitstellen mehrmals täglich zu einer live-Umgebung. Zum Bereitstellen von einem Azure-Team aktualisieren alle 2 bis 3 Monate, aber jetzt weniger wichtigen Updates von Versionen jeder 2 bis 3 Tage und die Hauptversionsnummer frei alle 2 bis 3 Wochen. In diesem Rhythmus erste hilft ungemein Sie Kundenfeedback reaktionsfähig ist.

Zu diesem Zweck müssen Sie eine Schleife für Entwicklung und Bereitstellung aktivieren, die wiederholbare, zuverlässige und vorhersagbare und niedrigen Zykluszeit hat.

![DevOps-workflow](automate-everything/_static/image1.png)

Die Zeitspanne zwischen, wenn Sie eine Idee für eine Funktion haben, und wenn die Kunden verwenden, und Übermitteln von Feedback muss also so kurz wie möglich sein. Automatisieren von der ersten drei Muster – alles Datenquellen-Steuerelements und fortlaufende Integration und Bereitstellung – werden alle zu bewährten Methoden, die wir empfehlen, damit diese Art des Vorgehens können.

## <a name="azure-management-scripts"></a>Azure-Verwaltungsskripts

In der [Einführung in diese e-Book](introduction.md), haben Sie gesehen der webbasierten Konsole, die Azure-Verwaltungsportal. Das Verwaltungsportal können Sie zum Überwachen und Verwalten aller Ressourcen, die Sie in Azure bereitgestellt haben. Es ist eine einfache Möglichkeit zum Erstellen und Löschen von Diensten, z. B. webapps und virtuellen Computern, konfigurieren Sie diese Dienste, Dienstvorgang überwachen und usw. Es ist ein großartiges Tool verwenden, ist ein manueller Prozess. Wenn Sie nun eine produktionsanwendung beliebiger Größe zu entwickeln, und insbesondere in einer teamumgebung sollten Sie über die Benutzeroberfläche, um zu erfahren und Untersuchen von Azure-Portal wechseln und dann automatisieren die Prozesse, die Sie wiederholt auf diese Weise werden müssen.

Nahezu alles, die Sie manuell im Verwaltungsportal oder in Visual Studio tun kann auch durch Aufrufen der REST-API erfolgen. Sie können mit Skripts schreiben [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), oder Sie können z. B. eine open Source-Framework verwenden [Chef](http://www.opscode.com/chef/) oder [Puppet](http://puppetlabs.com/puppet/what-is-puppet). Sie können auch das Befehlszeilentool "Bash" in einer Umgebung Mac- oder Linux. In Azure sind Skripting APIs für diese verschiedenen Umgebungen und auf ihm ein [.NET Verwaltungs-API](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) für den Fall, dass Sie Code anstelle von Skript schreiben möchten.

Für die app zu beheben, die wir einige Windows PowerShell-Skripts, die die Prozesse zum Erstellen einer testumgebung und Bereitstellung des Projekts zu dieser Umgebung automatisieren erstellt haben, und wir ein Teil des konfigurationselementinhalts diese Skripts werden.

## <a name="environment-creation-script"></a>Erstellungsskript Umgebung

Das erste Skript Wir betrachten namens *neu AzureWebsiteEnv.ps1*. Erstellt eine Azure-Umgebung, die korrigieren app zu Testzwecken bereitgestellt werden kann. Die wichtigsten Aufgaben, die dieses Skript führt lauten wie folgt:

- Erstellen Sie eine Web-app.
- Erstellen Sie ein Speicherkonto. (Erforderlich, für die Blobs und Warteschlangen, wie Sie in den Kapiteln sehen.)
- Erstellen Sie eine SQL-Datenbankserver und zwei Datenbanken: eine Anwendungsdatenbank und eine Mitgliedschaftsdatenbank.
- Speichern von Einstellungen in Azure, mit dem die app wird auf das Speicherkonto und die Datenbanken zugreifen.
- Erstellen Sie Einstellungsdateien, die zum Automatisieren der Bereitstellung verwendet werden.

### <a name="run-the-script"></a>Führen Sie das Skript


> [!NOTE]
> In diesem Teil des Kapitels werden Beispiele für Skripts und Befehle, die Sie eingeben, um sie auszuführen. Diese eine Demo und stellt keinen alles, was Sie wissen, um die Skripts ausgeführt werden. Schrittweise Vorgehensweise-to--It, finden Sie unter [Anhang: die korrigieren Sie diese Beispielanwendung](the-fix-it-sample-application.md#deploybase).


Zum Ausführen eines PowerShell-Skripts, das Azure-Dienste verwaltet, müssen Sie die Azure PowerShell-Konsole installieren und konfigurieren diese für das Arbeiten mit Ihrem Azure-Abonnement. Nachdem Sie haben eingerichtet, können Sie beheben diese Umgebung Skript zum Erstellen der mit einem Befehl wie diese ausführen:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

Die `Name` Parameter gibt den Namen verwendet werden, wenn die Datenbank und Speicher-Konten erstellen und die `SqlDatabasePassword` Parameter gibt das Kennwort für das Administratorkonto ein, die für SQL-Datenbank erstellt werden soll. Es gibt andere Parameter, denen Sie verwenden können, die wir später betrachten müssen.

![PowerShell-Fenster](automate-everything/_static/image2.png)

Nach Abschluss des Skripts können Sie im Verwaltungsportal sehen, was erstellt wurde. Finden Sie zwei Datenbanken:

![Databases](automate-everything/_static/image3.png)

Ein Speicherkonto:

![Speicherkonto](automate-everything/_static/image4.png)

Und einer Web-app:

![Website](automate-everything/_static/image5.png)

Auf der **konfigurieren** Registerkarte für die Web-app finden Sie unter er hat die speicherkontoeinstellungen und SQL-Datenbank-Verbindungszeichenfolgen für die Korrektur es app.

!["appSettings" und connectionStrings](automate-everything/_static/image6.png)

Die *Automatisierung* Ordner jetzt auch enthält eine  *&lt;angegebenen "Websitename"&gt;pubxml* Datei. Diese Datei speichert, die auf die Anwendung auf der Azure-Umgebung bereitzustellen, die gerade erstellte MSBuild verwenden. Zum Beispiel:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Wie Sie sehen können, das Skript hat eine vollständige Umgebung erstellt, und der gesamte Prozess erfolgt in ungefähr 90 Sekunden.

Wenn jemand in Ihrem Team eine testumgebung erstellen möchte, können sie einfach das Skript ausführen. Nicht nur ist es schneller, sondern auch können sie sicher, dass sie eine identisch mit der Umgebung verwenden, die Sie verwenden werden. Sie konnten nicht ganz so sicher, wenn "Jeder" Dinge manuell einrichten wurde über das Verwaltungsportal UI sein.

### <a name="a-look-at-the-scripts"></a>Ein Blick auf die Skripts

Es gibt tatsächlich drei Skripts, die diese Arbeit ausführen. Sie rufen Sie eine von der Befehlszeile aus, und die anderen beiden automatisch verwendet, um einige Aufgaben sind:

- *Neue AzureWebSiteEnv.ps1* ist das Hauptskript.

    - *Neue AzureStorage.ps1* das Speicherkonto erstellt.
    - *Neue AzureSql.ps1* erstellt die Datenbanken.

### <a name="parameters-in-the-main-script"></a>Parameter in das Hauptskript

Das Hauptskript *neu AzureWebSiteEnv.ps1*, mehrere Parameter definiert:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

Es sind zwei Parameter erforderlich:

- Der Name der Web-app, die das Skript erstellt werden soll. (Dies wird auch verwendet, für die URL: `<name>.azurewebsites.net`.)
- Das Kennwort für den Administrator des Datenbankservers, der das Skript erstellt werden soll.

Optionale Parameter können Sie Standort des Rechenzentrums (Standardwert "West US"), Datenbankserveradministrators (Standardwert "Dbuser") und eine Firewallregel für den Datenbankserver angeben.

### <a name="create-the-web-app"></a>Die Web-app erstellen

Als Erstes wird das Skript wird die Web-app erstellen, indem die `New-AzureWebsite` -Cmdlet übergeben, die Web-app Name und Speicherort Parameterwerte:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Das Speicherkonto erstellen

Dann das Hauptskript führt die *neu AzureStorage.ps1* Skript zu erstellen, angeben "*&lt;angegebenen" Websitename "&gt;*Speicher" für den speicherkontonamen und den gleichen Standort des Rechenzentrums als die Web-app.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*Neue AzureStorage.ps1* Aufrufe der `New-AzureStorageAccount` Cmdlet zum Erstellen des Speicherkontos an, und es wird das Konto und den Schlüsselwerten zurückgegeben. Die Anwendung benötigen diese Werte, um die Blobs und Warteschlangen im Speicherkonto zugreifen zu können.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Sie können nicht immer ein neues Speicherkonto erstellen möchten; Sie können das Skript erhöhen, indem Hinzufügen eines Parameters, das optional anweist, um ein vorhandenes Speicherkonto verwenden.

### <a name="create-the-databases"></a>Erstellen der Datenbanken

Das Hauptskript führt dann die Datenbank-Erstellungsskript *neu AzureSql.ps1*, nach dem Einrichten der Standarddatenbank und firewall-Regelnamen:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

Das Skript zum Erstellen der Datenbank die Entwickler-Computer IP-Adresse abgerufen und eine Firewallregel Wartefunktion aktiviert, sodass der Entwickler-Computer herstellen und den Server verwalten kann. Das Skript zum Erstellen der Datenbank durchläuft dann verschiedene Schritte zum Einrichten von Datenbanken:

- Erstellt den Server mithilfe der `New-AzureSqlDatabaseServer` Cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Erstellt Firewallregeln zum Aktivieren der Entwickler-Computer zum Verwalten des Servers und die Web-app zum Herstellen der Verbindung zu aktivieren. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Erstellt einen Datenbankkontext, der über den Servernamen und Anmeldeinformationen, enthält die `New-AzureSqlDatabaseServerContext` Cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText`ist eine Funktion in das Skript, das Aufrufen der `ConvertTo-SecureString` -Cmdlet zum Verschlüsseln des Kennworts und gibt eine `PSCredential` Objekt vom selben Typ, der die `Get-Credential` -Cmdlet zurückgegeben.
- Erstellt die dienstanwendungs-Datenbank und der Mitgliedschaftsdatenbank mithilfe der `New-AzureSqlDatabase` Cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Ruft eine lokal definierte Funktion Tocreates eine Verbindungszeichenfolge für die einzelnen Datenbanken. Die Anwendung wird diese Verbindungszeichenfolgen verwenden, um den Zugriff auf Datenbanken. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString ist eine Funktion, die im Skript definiert, das die Verbindungszeichenfolge aus der Parameterwerte, die für sie erstellt.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Gibt eine Hashtabelle mit den Namen des Datenbankservers und die Verbindungszeichenfolgen an.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

Die app zu beheben verwendet separate Mitgliedschaft und Anwendungsdatenbanken. Es ist auch möglich, Mitgliedschaft und Anwendungsdaten in einer einzelnen Datenbank eingefügt werden soll.

### <a name="store-app-settings-and-connection-strings"></a>Store-app-Einstellungen und Verbindungszeichenfolgen

Azure ist ein Feature, mit dem Sie zum Speichern von Einstellungen und Verbindungszeichenfolgen, die automatisch zu überschreiben, was an die Anwendung zurückgegeben wird, wenn versucht wird, lesen Sie, die `appSettings` oder `connectionStrings` Sammlungen in der Datei "Web.config". Dies ist eine Alternative zum Anwenden von ["Web.config" Transformationen](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) bei der Bereitstellung. Weitere Informationen finden Sie unter [sensible Daten in Azure speichern](source-control.md#appsettings) weiter unten in diesem e-Book.

Das Skript zum Erstellen der Umgebung in Azure alle speichert die `appSettings` und `connectionStrings` Werte, die die Anwendung muss das Speicherkonto und die Datenbanken zugreifen, wenn sie in Azure ausgeführt wird.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/) ist ein Telemetrie-Framework, die wir in veranschaulichen die [Überwachung und Telemetrie](monitoring-and-telemetry.md) Kapitel. Das Skript zum Erstellen der Umgebung wird auch die Web-app, um sicherzustellen, dass es die New Relic-Einstellungen übernimmt neu gestartet.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Vorbereiten für die Bereitstellung

Am Ende des Prozesses Ruft das Skript zum Erstellen der Umgebung zwei Funktionen zum Erstellen von Dateien, die durch das Bereitstellungsskript verwendet werden.

Eine dieser Funktionen erstellt einen Veröffentlichungsprofil *(&lt;angegebenen "Websitename"&gt;pubxml* Datei). Der Code Ruft die Azure-REST-API zum Abrufen der Einstellungen für die Veröffentlichung, und speichert die Informationen in einem *publishsettings* Datei. Anschließend wird die Informationen aus dieser Datei zusammen mit einer Vorlagendatei (*pubxml.template*) zum Erstellen der *pubxml* -Datei, die das Veröffentlichungsprofil enthält. Diesen Schritten wird simuliert, was Sie in Visual Studio tun: Herunterladen einer *publishsettings* Datei und importieren, um ein Veröffentlichungsprofil zu erstellen.

Verwendet die andere Funktion einer anderen Vorlagendatei (Website-environment.template) zum Erstellen einer *Website environment.xml* Datei, die Einstellungen enthält, verwenden Sie das Bereitstellungsskript wird zusammen mit, den *pubxml*Datei.

### <a name="troubleshooting-and-error-handling"></a>Problembehandlung und Fehlerbehandlung

Skripts sind, wie Programme: kann fehlschlagen, und wenn dies der Fall möchten Sie wissen, wie Sie zu dem Fehler und dessen Ursache können. Aus diesem Grund ändert das Skript zum Erstellen der Umgebung den Wert für die `VerbosePreference` Variable aus `SilentlyContinue` auf `Continue` , damit alle ausführliche Meldungen angezeigt werden. Ändert sich auch, den Wert von der `ErrorActionPreference` Variable aus `Continue` auf `Stop`, damit das Skript beendet, selbst wenn es Fehler ohne Abbruch auftritt:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Bevor sie Arbeit ausführt, speichert das Skript die Startzeit, damit es die verstrichene Zeit berechnen kann, wenn er abgeschlossen ist:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Nachdem sie ihre Arbeit abgeschlossen hat, zeigt das Skript die verstrichene Zeit:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

Und für jede Schlüsselvorgang schreibt das Skript ausführliche Meldungen, z. B.:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Bereitstellungsskript

Was die *neu AzureWebsiteEnv.ps1* -Skripts für die umgebungserstellung, die *veröffentlichen AzureWebsite.ps1* -Skripts für die anwendungsbereitstellung.

Das Bereitstellungsskript Ruft den Namen der Web-app aus dem *Website environment.xml* Datei, die durch das Skript zum Erstellen der Umgebung erstellt.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Es ruft das Benutzerkennwort Bereitstellung aus der *publishsettings* Datei:

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Er führt die [MSBuild](http://msbuildbook.com/) Befehl, die builds und Bereitstellen des Projekts:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

Und wenn Sie angegeben haben die `Launch` Parameter in der Befehlszeile angegeben wird, ruft er die `Show-AzureWebsite` Cmdlet, um den Standardbrowser zur Website-URL zu öffnen.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

Sie können das Bereitstellungsskript mit einem Befehl wie diese ausführen:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

Und wenn dies erfolgt ist, der Browser geöffnet wird, mit dem Standort ausgeführt wird, in der Cloud an den `<websitename>.azurewebsites.net` URL.

![Korrigieren Sie diesen app-Bereitstellung in Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>Zusammenfassung

Mit diesen Skripts können Sie sicher sein, dass die gleichen Schritte immer in der gleichen Reihenfolge mit denselben Optionen ausgeführt werden. Dadurch wird sichergestellt, dass jedem Entwickler im Team nicht etwas verpassen oder Fehler etwas machen und etwas auf einem eigenen Computer die tatsächlich die gleiche Weise in ein anderes Teammitglied Umgebung oder in der Produktion ungeeigneten benutzerdefinierte bereitstellen.

Auf ähnliche Weise können Sie die meisten Azure-Verwaltungsfunktionen automatisieren, die Sie durchführen können, im Verwaltungsportal, mithilfe der REST-API, Windows PowerShell-Skripts, eine Sprache .NET API oder ein Bash-Hilfsprogramm, die unter Linux oder Mac ausgeführt werden können

In der [nächsten Kapitels](source-control.md) Wir betrachten Quellcode und erläutern, warum es wichtig, die Skripts in dem Quellcoderepository eingeschlossen ist.

## <a name="resources"></a>Ressourcen

- [Installieren und Konfigurieren von WindowsPowerShell für Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Erläutert, wie Sie Azure PowerShell-Cmdlets installieren und wie das Zertifikat zu installieren, dass Sie auf Ihrem Computer zur Verwaltung von Azure berücksichtigen müssen. Dies ist ein idealer Ausgangspunkt, um zu beginnen, da sie auch Links zu Ressourcen zum Erlernen von PowerShell selbst hat.
- [Azure Script Center](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). WindowsAzure.com-Portal, um Ressourcen für das Entwickeln von Skripts, die Azure-Dienste mit Links zu abrufen gestarteten Lernprogramme, Referenz-Dokumentation und Quelle Codes Cmdlets und Beispielskripts zu verwalten.
- [Wochenende Skripter: Erste Schritte mit Azure und PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). In einem Blog für Windows PowerShell bietet diesen Beitrag eine gute Einführung in mithilfe von PowerShell für Azure-Verwaltungsfunktionen.
- [Installieren und Konfigurieren der plattformübergreifenden Azure-Befehlszeilenschnittstelle](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Erste-Schritte-Lernprogramm für ein Azure scripting Framework, die für Mac und Linux sowie Windows Systemen verwendet werden kann.
- [Befehlszeilentools Abschnitt des Themas Herunterladen der Azure-SDKs und Tools](https://azure.microsoft.com/downloads/). Portalseite zum Dokumentationen und Downloads, die im Zusammenhang mit Befehlszeilentools für Azure.
- [Automatisieren alles, was mit der Azure-Verwaltungsbibliotheken und .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman führt die .NET Verwaltungs-API für Azure.
- [Mithilfe von Windows PowerShell-Skripts zum Veröffentlichen in Entwicklungs- und Testumgebungen](https://msdn.microsoft.com/library/azure/dn642480.aspx). MSDN-Dokumentation, die erläutert, wie veröffentlichungsskripts, die Visual Studio generiert automatisch für Webprojekte.
- [PowerShell-Tools für Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Visual Studio-Erweiterung, die in Visual Studio die sprachunterstützung für Windows PowerShell hinzugefügt.

>[!div class="step-by-step"]
[Zurück](introduction.md)
[Weiter](source-control.md)
