---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Migrieren zu SQL Server - 10 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: bc0bca18d2f6e4cdbb527ab75952e9a50eb49b20
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827360"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Migrieren zu SQL Server - 10 12
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, das eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie die Web Publish Update installieren. Eine Einführung in die Reihe, finden Sie unter [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, das zeigt, Bereitstellungsfunktionen, die nach der RC-Version von Visual Studio 2012 eingeführt wurden, zeigt, wie zum Bereitstellen von SQL Server-Editionen als SQL Server Compact und zeigt, wie Sie in Azure App Service-Web-Apps bereitstellen, finden Sie unter [ASP.NET-webbereitstellung Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

In diesem Tutorial erfahren Sie, wie von SQL Server Compact in SQL Server migriert. Ein Grund, dass Sie dies tun möchten, ist SQL Server-Funktionen nutzen, die SQL Server Compact, z. B. gespeicherte Prozeduren, Trigger, Ansichten oder Replikation nicht unterstützt. Weitere Informationen zu den Unterschieden zwischen SQL Server Compact und SQL Server finden Sie unter den [Bereitstellen von SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) Tutorial.

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express im Vergleich zu vollständigen SQL Server für die Entwicklung

Nachdem Sie entschieden haben, um ein upgrade auf SQL Server, empfiehlt es sich um SQL Server oder SQL Server Express in Ihre Entwicklungs- und testumgebungen zu verwenden. Zusätzlich zu den unterschieden, mit der Toolsupport und Funktionen der Datenbank-Engine gibt es Unterschiede providerimplementierungen zwischen SQL Server Compact und anderen Versionen von SQL Server. Diese Unterschiede können dazu führen, dass den gleichen Code, um unterschiedliche Ergebnisse zu generieren. Aus diesem Grund, wenn Sie SQL Server Compact als Ihrer Entwicklungsdatenbank beibehalten möchten, sollten Sie gründlich Ihrer Website in SQL Server oder SQL Server Express in einer testumgebung vor jeder Bereitstellung zur Produktion testen.

Im Gegensatz zu SQL Server Compact SQL Server Express im Wesentlichen die gleiche Datenbank-Engine und den gleichen Anbieter für .NET als Vollversion von SQL Server verwendet. Wenn Sie mit SQL Server Express testen, können Sie darauf vertrauen, die gleichen Ergebnisse erhalten, wie Sie mit SQL Server. Sie können die meisten Tools für die gleiche mit SQL Server Express, mit denen Sie mit SQL Server (eine wichtige Ausnahme wird [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)), und andere Funktionen von SQL Server wie gespeicherte Prozeduren, Sichten, Trigger, unterstützt und die Replikation. (Sie müssen in der Regel eine Produktionswebsite jedoch vollständige SQL-Server verwenden. SQL Server Express in einer freigegebenen Hostingumgebung führen kann, aber sie wurde nicht dafür entwickelt, und viele Hostinganbieter nicht unterstützen.)

Wenn Sie Visual Studio 2012 verwenden, wählen Sie in der Regel SQL Server Express LocalDB für Ihre Entwicklungsumgebung ein, denn das ist was standardmäßig mit Visual Studio installiert ist. Allerdings funktioniert LocalDB nicht in IIS, damit für Ihre testumgebung Sie entweder SQL Server oder SQL Server Express verwenden müssen.

### <a name="combining-databases-versus-keeping-them-separate"></a>Kombinieren von Datenbanken im Vergleich zu separaten sorgen

Die Contoso University-Anwendung verfügt über zwei SQL Server Compact-Datenbanken: der Mitgliedschaftsdatenbank (*aspnet.sdf*) und die Anwendungsdatenbank (*School.sdf*). Bei der Migration können Sie diese Datenbanken auf zwei separate Datenbanken oder auf eine einzelne Datenbank migrieren. Sie möchten diese kombinieren, um Datenbankjoins zwischen Ihrer Anwendung und Ihre Mitgliedschaftsdatenbank zu ermöglichen. Ihren hostingplan gewährleistet auch einen Grund, diese zu kombinieren. Z. B. der Hostinganbieter möglicherweise mehr für mehrere Datenbanken berechnet oder u. u. nicht mehr als eine Datenbank selbst zu. Das ist der Fall bei der Cytanium Lite-hosting-Konto, das für dieses Tutorial verwendet wird, die nur eine einzelne SQL Server-Datenbank ermöglicht.

In diesem Tutorial werden Sie auf diese Weise zwei Datenbanken migrieren:

- Migrieren Sie zu zwei LocalDB-Datenbanken in der Entwicklungsumgebung.
- Migrieren Sie zu zwei SQL Server Express-Datenbanken in der testumgebung.
- Migrieren Sie zu einem kombinierten vollständigen SQL Server-Datenbank in der produktionsumgebung.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>Installieren von SQL Server Express

SQL Server Express wird automatisch standardmäßig mit Visual Studio 2010 installiert, aber es ist nicht standardmäßig installiert mit Visual Studio 2012. Klicken Sie auf den folgenden Link, um SQL Server 2012 Express zu installieren,

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

Wählen Sie *ENU/X64/SQLEXPR\_X64\_ENU.exe* oder *ENU/X86/SQLEXPR\_X86\_ENU.exe*, und übernehmen Sie den Standardnamen im Installations-Assistenten Einstellungen. Weitere Informationen zu Installationsoptionen finden Sie unter [Installieren von SQL Server 2012 vom Installations-Assistenten (Setup)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Erstellen von SQL Server Express-Datenbanken für die Testumgebung

Der nächste Schritt besteht darin die ASP.NET-Mitgliedschaft und School-Datenbanken zu erstellen.

Von der **Ansicht** Menü die Option **Server-Explorer** (**Datenbank-Explorer** in Visual Web Developer), und klicken Sie dann mit der rechten Maustaste **Datenverbindungen**, und wählen Sie **neue SQL Server-Datenbank erstellen**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

In der **neue SQL Server-Datenbank erstellen** Dialogfeld Geben Sie ". \SQLExpress" in der **Servernamen** Box und "Aspnet-Test" in der **neuen Datenbanknamen** , und klicken Sie dann **OK**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Führen Sie das gleiche Verfahren zum Erstellen einer neuen SQL Server Express School-Datenbank, die mit dem Namen "Schule-Test" aus.

(Sie sind "Test" auf diese Datenbanknamen anfügen, da später Sie eine zusätzliche Instanz von jeder Datenbank für die Entwicklungsumgebung erstellen, und Sie in der Lage, die zwei Sätze von Datenbanken zu unterscheiden sein müssen.)

**Server-Explorer** zeigt jetzt die zwei neuen Datenbanken.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Skripterstellung für die neuen Datenbanken erteilen

Wenn die Anwendung in IIS auf Ihrem Entwicklungscomputer ausgeführt wird, greift auf die Anwendung die Datenbank mithilfe von Anmeldeinformationen für den Standardanwendungspool. Standardmäßig haben die Identität des Anwendungspools allerdings nicht Berechtigung zum Öffnen von Datenbanken. Daher müssen Sie zum Ausführen eines Skripts, um diese Berechtigung zu erteilen. In diesem Abschnitt erstellen Sie das Skript, dass Sie weiter unten, um sicherzustellen, dass die Anwendung auf die Datenbanken öffnen kann, bei der Ausführung in IIS ausgeführt werden.

In der Projektmappe *SolutionFiles* im erstellten Ordner die [in der Produktionsumgebung bereitstellen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Tutorial erstellen Sie eine neue SQL-Datei mit dem Namen *Grant.sql*. Kopieren Sie die folgenden SQL-Befehle in die Datei, und speichern Sie und schließen Sie die Datei:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Dieses Skript dient zum Arbeiten mit SQL Server 2008 und die IIS-Einstellungen in Windows 7, wie sie in diesem Tutorial angegeben sind. Wenn Sie eine andere Version von SQL Server oder Windows verwenden, oder wenn Sie IIS auf Ihrem Computer anders festlegen, können Änderungen an diesem Skript erforderlich sein. Weitere Informationen zu SQL Server-Skripts, finden Sie unter [SQL Server-Onlinedokumentation](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Sicherheitshinweis** dieses Skript gibt Db\_Owner-Berechtigungen für den Benutzer, die Zugriff auf die Datenbank zur Laufzeit handelt es sich in der produktionsumgebung wiederholen müssen. In einigen Fällen empfiehlt es sich um einen Benutzer mit vollständigen Datenbankschema aktualisieren von Berechtigungen nur für die Bereitstellung, und geben Sie für die Laufzeit einen anderen Benutzer, der berechtigt nur zum Lesen und Schreiben von Daten. Weitere Informationen finden Sie unter **überprüfen die automatischen Änderungen der Datei "Web.config" für Code First-Migrationen** in [Bereitstellen in IIS als Testumgebung](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).


## <a name="configuring-database-deployment-for-the-test-environment"></a>Konfigurieren von Datenbankbereitstellung für die Testumgebung

Als Nächstes konfigurieren Sie Visual Studio, sodass sie die folgenden Aufgaben für jede Datenbank ausführen wird:

- Generieren eines SQL-Skripts, das die Quelldatenbank-Struktur (Tabellen, Spalten, Einschränkungen usw.) in der Zieldatenbank erstellt.
- Generieren eines SQL-Skripts, das Daten von der Quelldatenbank in die Tabellen in der Zieldatenbank einfügt.
- Führen Sie die generierten Skripts und das Skript zum erteilen, das Sie erstellt haben, in der Zieldatenbank.

Öffnen der **Projekteigenschaften** Fenster, und wählen die **SQL packen/veröffentlichen** Registerkarte.

Stellen Sie sicher, dass **aktiv (Release)** oder **Version** ausgewählt ist, der **Konfiguration** Dropdown-Liste.

Klicken Sie auf **aktivieren Sie diese Seite**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

Die **SQL packen/veröffentlichen** Registerkarte ist normalerweise deaktiviert, da sie über eine legacybereitstellung-Methode angibt. In den meisten Fällen sollten Sie konfigurieren, datenbankbereitstellung, in der **Webveröffentlichung** Assistenten. Migrieren von SQL Server Compact in SQL Server oder SQL Server Express ist ein Sonderfall, der für den diese Methode eine gute Wahl ist.

Klicken Sie auf **aus Web.config importieren**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio sucht nach Verbindungszeichenfolgen in der *"Web.config"* Datei findet, eine für die Mitgliedschaftsdatenbank und eine für die Datenbank "School" und fügt eine Zeile für jede Verbindungszeichenfolge in der **Datenbankeinträge**  Tabelle. Die gefundenen Verbindungszeichenfolgen für die vorhandenen SQL Server Compact-Datenbanken sind, und der nächste Schritt so konfigurieren Sie wie und wo diese Datenbanken bereitstellen.

Sie geben Sie die Bereitstellung der datenbankeinstellungen in der **Datenbankeintragsdetails** weiter unten im Abschnitt der **Datenbankeinträge** Tabelle. In den Systemeinstellungen angezeigten der **Datenbankeintragsdetails** Abschnitt beziehen sich je nachdem, was in Zeile die **Datenbankeinträge** Tabelle aktiviert ist, wie in der folgenden Abbildung dargestellt.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurieren von Bereitstellungseinstellungen für die Mitgliedschaftsdatenbank

Wählen Sie die **DefaultConnection-Deployment** Zeile in der **Datenbankeinträge** Tabelle, um Einstellungen zu konfigurieren, die für die Mitgliedschaftsdatenbank gelten.

In **Verbindungszeichenfolge für Zieldatenbank**, geben Sie eine Verbindungszeichenfolge, die auf der neuen SQL Server Express-Mitgliedschaftsdatenbank verweist. Sie erhalten die Verbindungszeichenfolge, die Sie benötigen aus **Server-Explorer**. In **Server-Explorer**, erweitern Sie **Datenverbindungen** , und wählen Sie die **AspnetTest** -Datenbank, klicken Sie dann aus der **Eigenschaften** Fenster Kopieren der **Verbindungszeichenfolge** Wert.

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

Die gleiche Verbindungszeichenfolge reproduziert wird hier:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Kopieren Sie die Verbindungszeichenfolge in **Verbindungszeichenfolge für Zieldatenbank** in die **SQL packen/veröffentlichen** Registerkarte.

Stellen Sie sicher, dass **Daten und/oder Schema aus einer vorhandenen Datenbank extrahieren** ausgewählt ist. Dies ist, was bewirkt, dass SQL-Skripts automatisch generiert und in der Zieldatenbank ausgeführt werden.

Die **Verbindungszeichenfolge für die Quelldatenbank** Wert wird extrahiert, von der *"Web.config"* Datei- und verweist auf die Entwicklung SQL Server Compact-Datenbank. Dies ist die Quelldatenbank, die verwendet wird, um die Skripts zu generieren, die weiter unten in der Zieldatenbank ausgeführt werden. Da Sie die Produktionsversion der Datenbank bereitstellen möchten, ändern Sie "Aspnet-Dev.sdf" in "Aspnet-Prod.sdf" ein.

Änderung **Datenbank Skriptoptionen** aus **nur Schema** zu **Schema und Daten**, da Sie Ihre Daten (Benutzerkonten und Rollen) zu kopieren, sowie die Struktur der Datenbank möchten.

Zum Konfigurieren der Bereitstellung führen Sie die Grant-Skripts, die Sie zuvor erstellt haben, müssen Sie auf die **Datenbankskripts** Abschnitt. Klicken Sie auf **Skript hinzufügen**, und klicken Sie in der **SQL-Skripts hinzufügen** Dialogfeld navigieren Sie zu dem Ordner, in dem Sie das Skript zum Erteilen (Dies ist der Ordner mit Ihrer Projektmappendatei). Wählen Sie die Datei mit dem Namen *Grant.sql*, und klicken Sie auf **öffnen**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

Die Einstellungen für die **DefaultConnection-Deployment** für die Zeile im **Datenbankeinträge** jetzt wie folgt in der folgenden Abbildung aussehen:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurieren von Bereitstellungseinstellungen für die Datenbank "School"

Wählen Sie als Nächstes die **SchoolContext Bereitstellung** Zeile in der **Datenbankeinträge** Tabelle, um die bereitstellungseinstellungen für die Datenbank "School" zu konfigurieren.

Sie können die gleiche Methode verwenden, die Sie zuvor beim Abrufen der Verbindungszeichenfolge für die neue SQL Server Express-Datenbank. Kopieren Sie die Verbindungszeichenfolge in **Verbindungszeichenfolge für Zieldatenbank** in die **SQL packen/veröffentlichen** Registerkarte.

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Stellen Sie sicher, dass **Daten und/oder Schema aus einer vorhandenen Datenbank extrahieren** ausgewählt ist.

Die **Verbindungszeichenfolge für die Quelldatenbank** Wert wird extrahiert, von der *"Web.config"* Datei- und verweist auf die Entwicklung SQL Server Compact-Datenbank. Ändern Sie "Schule-Dev.sdf" auf "School-Prod.sdf", um die Produktionsversion der Datenbank bereitzustellen. (Sie noch nie eine School-Prod.sdf-Datei erstellt, in der App\_Datenordner, damit Sie diese Datei aus der testumgebung für die App kopieren werden\_Datenordner in der ContosoUniversity Projektordner weiter unten.)

Änderung **Datenbank Skriptoptionen** zu **Schema und Daten**.

Möchten Sie führen das Skript zum Gewähren von Lese- und Schreibberechtigungen für diese Datenbank für die Identität des Anwendungspools, fügen Sie daher die *Grant.sql* Skriptdatei wie für die Mitgliedschaftsdatenbank.

Wenn Sie fertig sind, die Einstellungen für die **SchoolContext Bereitstellung** für die Zeile im **Datenbankeinträge** in der folgenden Abbildung aussehen:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Speichern Sie die Änderungen an der **SQL packen/veröffentlichen** Registerkarte.

Kopieren der *School-Prod.sdf* -Datei aus der *c:\inetpub\wwwroot\ContosoUniversity\App\_Daten* Ordner, um die *App\_Daten* Ordner Das ContosoUniversity-Projekt.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Angeben von transaktiven Modus für das Skript zum Erteilen

Während des Bereitstellungsvorgangs generiert Skripts, die das Datenbankschema und Daten bereitstellen. Standardmäßig werden diese Skripts in einer Transaktion ausführen. Benutzerdefinierte Skripts (z. B. die Grant-Skripts) in der Standardeinstellung werden jedoch nicht in einer Transaktion ausgeführt werden. Wenn während des Bereitstellungsvorgangs Transaktionsmodi enthält, erhalten beim Ausführen von Skripts während der Bereitstellung möglicherweise ein Timeoutfehler auftreten. In diesem Abschnitt bearbeiten Sie die Projektdatei, um den benutzerdefinierten Skripts zum Ausführen in einer Transaktion zu konfigurieren.

In **Projektmappen-Explorer**, mit der rechten Maustaste die **ContosoUniversity** Projekt, und wählen **Projekt entladen**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Klicken Sie dann mit der rechten Maustaste erneut auf des Projekts, und wählen Sie **"contosouniversity.csproj" Bearbeiten**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Visual Studio-Editor erfahren Sie, den Inhalt der XML-Datei des Projekts. Beachten Sie, dass es mehrere gibt `PropertyGroup` Elemente. (In der Abbildung wird der Inhalt der `PropertyGroup` Elemente wurden ausgelassen wurde.)

![Project-Datei-Editor-Fenster](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

Erstens, ohne `Condition` Attribut, das ist für die Einstellungen, die gelten unabhängig von Konfiguration erstellen. Eine `PropertyGroup` Element gilt nur für Debug-Buildkonfiguration (Beachten Sie die `Condition` Attribut), eine gilt nur für die Buildkonfiguration für die Version und eine gilt nur für die Test-Build-Konfiguration. In der `PropertyGroup` -Element für die Releasekonfiguration erstellen, sehen Sie eine `PublishDatabaseSettings` Element, das die Einstellungen enthält, die Sie auf eingegeben der **SQL packen/veröffentlichen** Registerkarte. Es gibt eine `Object` -Element, dem die Grant-Skripts entspricht, angegeben (Beachten Sie, dass die beiden Instanzen von "Grant.sql"). In der Standardeinstellung die `Transacted` Attribut der `Source` für jedes Skript zum Erteilen von ist `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Ändern Sie den Wert von der `Transacted` Attribut der `Source` Element `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Speichern und schließen Sie die Projektdatei per Rechtsklick das Projekt im **Projektmappen-Explorer** , und wählen Sie **Projekt erneut laden**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Einrichten der Web.Config-Transformationen für die Verbindungszeichenfolgen

Die Verbindungszeichenfolgen für die neue SQL Express, die Sie eingegeben haben, auf Datenbanken die **SQL packen/veröffentlichen** Registerkarte werden nur für die Aktualisierung der Zieldatenbank während der Bereitstellung von Web Deploy verwendet. Sie müssen trotzdem einrichten *"Web.config"* Transformationen, damit die Verbindungszeichenfolgen in der bereitgestellten *"Web.config"* Datei, zeigen Sie auf den neuen SQL Server Express-Datenbanken. (Bei Verwendung der **SQL packen/veröffentlichen** Registerkarte Konfigurieren von Verbindungszeichenfolgen kann nicht im Veröffentlichungsprofil.)

Open *Web.Test.config* , und Ersetzen Sie die `connectionStrings` -Element mit der `connectionStrings` Element im folgenden Beispiel. (Stellen Sie sicher, dass Sie nur das ConnectionStrings-Element, nicht den umgebenden Code kopieren, die hier angezeigt wird, um den Kontext bereitzustellen.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Dieser Code bewirkt, dass die `connectionString` und `providerName` Attribute der einzelnen `add` zu ersetzende Element in der bereitgestellten *"Web.config"* Datei. Diese Verbindungszeichenfolgen sind nicht identisch mit der Sie eingegeben, in haben denen die **SQL packen/veröffentlichen** Registerkarte. Die Einstellung "MultipleActiveResultSets = True" wurde hinzugefügt werden, da dies für das Entity Framework und das Universal Providers erforderlich ist.

## <a name="installing-sql-server-compact"></a>Installieren von SQL Server Compact

Das SqlServerCompact NuGet-Paket bietet SQL Server Compact-Datenbank-Engine-Assemblys für die Contoso University-Anwendung. Aber jetzt nicht die Anwendung jedoch Web Deploy, die die SQL Server Compact-Datenbanken, lesen, um Skripts zum Ausführen in SQL Server-Datenbanken zu erstellen sein müssen. Um Web Deploy, um das Lesen von SQL Server Compact-Datenbanken zu aktivieren, installieren Sie SQL Server Compact auf dem Entwicklungscomputer mit den folgenden Link: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Bereitstellung in der Testumgebung

Um in der testumgebung zu veröffentlichen, müssen Sie ein Veröffentlichungsprofil erstellen, die für die Verwendung konfiguriert ist die **SQL packen/veröffentlichen** Registerkarte für die Datenbank, die anstelle von Einstellungen für das Veröffentlichungsprofil Datenbank veröffentlichen.

Löschen Sie zuerst das vorhandene Profil für den Test ein.

In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und klicken Sie auf **veröffentlichen**.

Wählen Sie die **Profil** Registerkarte.

Klicken Sie auf **Profile verwalten**.

Wählen Sie **Test**, klicken Sie auf **entfernen**, und klicken Sie dann auf **schließen**.

Schließen der **Webveröffentlichung** Assistenten, um diese Änderung zu speichern.

Als Nächstes erstellen Sie ein neues Testprofil aus, und verwenden Sie, um das Projekt veröffentlichen.

In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und klicken Sie auf **veröffentlichen**.

Wählen Sie die **Profil** Registerkarte.

Wählen Sie **&lt;neu... &gt;** aus der Dropdownliste aus, und geben Sie "Test" als Namen des Profils.

In der **-Dienst-URL** geben *"localhost"*.

In der **Website/Anwendung** geben *Default Web Site/ContosoUniversity*.

In der **Ziel-URL** geben `http://localhost/ContosoUniversity/`.

Klicken Sie auf **Weiter**.

Die **Einstellungen** Registerkarte eine Warnung aus, die die **SQL packen/veröffentlichen** Registerkarte konfiguriert wurde, und es bietet Ihnen die Möglichkeit zum Überschreiben, indem Sie aktivieren auf die neue Datenbank mit der Veröffentlichung von Verbesserungen. Für diese Bereitstellung möchten Sie nicht außer Kraft setzen der **SQL packen/veröffentlichen** Registerkarte Einstellungen, und klicken Sie einfach nur die auf **Weiter**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Eine Nachricht in die **Vorschau** Registerkarte gibt an, dass **keine Datenbanken ausgewählt werden. zum Veröffentlichen**, aber dies bedeutet nur, dass die datenbankveröffentlichung im Veröffentlichungsprofil nicht konfiguriert ist.

Klicken Sie auf **Veröffentlichen**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio stellt die Anwendung bereit und öffnet den Browser zur Startseite der Website in der testumgebung. Führen Sie die "Dozenten", um festzustellen, ob es sich um die gleichen Daten anzeigt, die Sie bereits gesehen haben. Führen Sie die **hinzufügen Schüler/Studenten** Seite, fügen Sie einen neuen Studenten hinzu und zeigen Sie dann den neuen Studenten in der **Schüler/Studenten** Seite. Hiermit wird überprüft, ob es sich bei der Sie die Datenbank aktualisieren können. Wählen Sie die **Update Gutschriften** Seite (Sie benötigen für die Anmeldung), stellen Sie sicher, dass die Mitgliedschaftsdatenbank bereitgestellt wurde und Sie darauf zugreifen.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Erstellen eine SQL Server-Datenbank für die Produktionsumgebung

Nun, dass Sie in der testumgebung bereitgestellt haben, können Sie die Bereitstellung in der Produktion einrichten. Sie beginnen, wie Sie für die testumgebung durch Erstellen einer Datenbank für die Bereitstellung auf. Wie Sie aus der Übersicht wissen, lässt der Lite Cytanium hostingplan nur eine einzelne SQL Server-Datenbank, sodass Sie nur eine Datenbank, nicht zwei eingerichtet werden. Alle Tabellen und Daten aus der Mitgliedschaft und School SQL Server Compact-Datenbanken werden in einer SQL Server-Datenbank in der Produktion bereitgestellt werden.

Wechseln Sie zur Systemsteuerung Cytanium am [ http://panel.cytanium.com ](http://panel.cytanium.com). Halten Sie die Maus über **Datenbanken** , und klicken Sie dann auf **SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

In der **SQL Server 2008** auf **Create Database**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Nennen Sie die Datenbank "School", und klicken Sie auf **speichern**. (Die Seite fügt automatisch das Präfix "Contosou", damit der effektiven Name "ContosouSchool" werden.)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

Klicken Sie auf der gleichen Seite auf **Create User**. Auf Cytanium Server, statt mithilfe der integrierten Windows-Sicherheit, und lassen die Identität des Anwendungspools die Datenbank zu öffnen erstellen Sie einen Benutzer, der Berechtigung zum Öffnen der Datenbank verfügt. Fügen Sie die Anmeldeinformationen des Benutzers auf die Verbindungszeichenfolgen, die in der Produktion wechseln *"Web.config"* Datei. In diesem Schritt erstellen Sie die Anmeldeinformationen an.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Geben Sie die erforderlichen Felder in der **SQL Benutzereigenschaften** Seite:

- Geben Sie "ContosoUniversityUser" als Namen ein.
- Geben Sie ein Kennwort ein.
- Wählen Sie **ContosouSchool** als Standarddatenbank.
- Wählen Sie die **ContosouSchool** Kontrollkästchen.

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Konfigurieren von Datenbankbereitstellung für die Produktionsumgebung

Nachdem Sie bereit sind, Einrichten der Bereitstellung der datenbankeinstellungen in der **SQL packen/veröffentlichen** Registerkarte wie zuvor für die testumgebung.

Öffnen der **Projekteigenschaften** wählen Sie im Fenster der **SQL packen/veröffentlichen** Registerkarte, und stellen Sie sicher, dass **aktiv (Release)** oder **Version** ist ausgewählte in die **Konfiguration** Dropdown-Liste.

Wenn Sie die bereitstellungseinstellungen für jede Datenbank konfigurieren, ist der entscheidende Unterschied zwischen der Funktion für Produktions- und testumgebungen, in der Konfiguration der Verbindungszeichenfolgen. Für die testumgebung Sie Datenbank-Verbindungszeichenfolgen anderes Ziel, kann aber für die produktionsumgebung die Ziel-Verbindungszeichenfolge für beide Datenbanken identisch. Dies ist, da Sie beide Datenbanken in einer Datenbank in einer produktionsumgebung bereitstellen.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurieren von Bereitstellungseinstellungen für die Mitgliedschaftsdatenbank

Wählen Sie zum Konfigurieren von Einstellungen, die für die Mitgliedschaftsdatenbank gelten, die **DefaultConnection-Deployment** Zeile in der **Datenbankeinträge** Tabelle.

In **Verbindungszeichenfolge für Zieldatenbank**, geben Sie eine Verbindungszeichenfolge, die in der neuen SQL Server-Produktionsdatenbank verweist, die Sie gerade erstellt haben. Sie können die Verbindungszeichenfolge in Ihrer begrüßungs-e-Mail abrufen. Der relevante Teil der e-Mail enthält die folgende Beispiel-Verbindungszeichenfolge:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Nachdem Sie die drei Variablen ersetzt haben, sieht die Verbindungszeichenfolge benötigen Sie wie im folgenden Beispiel aus:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Kopieren Sie die Verbindungszeichenfolge in **Verbindungszeichenfolge für Zieldatenbank** in die **SQL packen/veröffentlichen** Registerkarte.

Stellen Sie sicher, dass **Daten und/oder Schema aus einer vorhandenen Datenbank extrahieren** ist noch ausgewählt ist, und dass **Datenbank Skriptoptionen** ist immer noch **Schema und Daten**.

In der **Datenbankskripts** deaktivieren Sie das Kontrollkästchen neben dem Grant.sql-Skript.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurieren von Bereitstellungseinstellungen für die Datenbank "School"

Wählen Sie als Nächstes die **SchoolContext Bereitstellung** Zeile in der **Datenbankeinträge** Tabelle, um die Einstellungen für die Datenbank "School" zu konfigurieren.

Kopieren Sie die gleiche Verbindungszeichenfolge in **Verbindungszeichenfolge für Zieldatenbank** , die Sie in das Feld für die Mitgliedschaftsdatenbank kopiert.

Stellen Sie sicher, dass **Daten und/oder Schema aus einer vorhandenen Datenbank extrahieren** ist noch ausgewählt ist, und dass **Datenbank Skriptoptionen** ist immer noch **Schema und Daten**.

In der **Datenbankskripts** deaktivieren Sie das Kontrollkästchen neben dem Grant.sql-Skript.

Speichern Sie die Änderungen an der **SQL packen/veröffentlichen** Registerkarte.

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Einrichten der Datei "Web.config" transformiert für die Verbindungszeichenfolgen auf Produktionsdatenbanken

Als Nächstes richten Sie *"Web.config"* Transformationen, damit die Verbindungszeichenfolgen in der bereitgestellten *"Web.config"* Datei, die auf die neue Produktionsdatenbank zu verweisen. Die Verbindungszeichenfolge, die Sie eingegeben haben, auf die **SQL packen/veröffentlichen** Registerkarte für Web Deploy zu verwenden, ist identisch mit der die Anwendung benötigt, mit Ausnahme der Hinzufügung von der `MultipleResultSets` Option.

Öffnen *Web.Production.config* , und Ersetzen Sie die `connectionStrings` -Element mit einem `connectionStrings` -Element, das wie im folgenden Beispiel aussieht. (Nur Kopieren der `connectionStrings` -Element, das nicht den umgebenden Tags, die bereitgestellt werden, um den Kontext anzuzeigen.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

Daraufhin manchmal Ratschläge, die Aufschluss darüber gibt, um immer Verschlüsseln von Verbindungszeichenfolgen in der *"Web.config"* Datei. Dies ist möglicherweise geeignet, wenn Sie auf Server im Netzwerk Ihres eigenen Unternehmens bereitstellen. Wenn Sie in einer freigegebenen Hostingumgebung bereitstellen, jedoch können Sie die Sicherheitsmaßnahmen des Hostinganbieters vertrauen, und es ist nicht erforderlich oder praktikabel ist, Verschlüsseln von Verbindungszeichenfolgen.

## <a name="deploying-to-the-production-environment"></a>In der Produktionsumgebung bereitstellen

Sie nun können für die Produktion bereitstellen. Web Deploy liest die SQL Server Compact-Datenbanken in Ihrem Projekts die *App\_Daten* Ordner und alle Tabellen und Daten in der SQL Server-Produktionsdatenbank neu zu erstellen. Zum Veröffentlichen der **Web packen/veröffentlichen** tabulatoreinstellungen, müssen Sie ein neues Veröffentlichungsprofil für die Produktion zu erstellen.

Löschen Sie zuerst das vorhandene Profil für die Produktion, wie Sie das Test-Profil.

In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und klicken Sie auf **veröffentlichen**.

Wählen Sie die **Profil** Registerkarte.

Klicken Sie auf **Profile verwalten**.

Wählen Sie **Produktion**, klicken Sie auf **entfernen**, und klicken Sie dann auf **schließen**.

Schließen der **Webveröffentlichung** Assistenten, um diese Änderung zu speichern.

Als Nächstes erstellen Sie ein neues produktionsprofil, und verwenden Sie, um das Projekt veröffentlichen.

In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und klicken Sie auf **veröffentlichen**.

Wählen Sie die **Profil** Registerkarte.

Klicken Sie auf **Import**, und wählen Sie die PUBLISHSETTINGS-Datei, die Sie zuvor heruntergeladen haben.

Auf der **Verbindung** Registerkarte die **Ziel-URL** auf die richtige temporären URL, die in diesem Beispiel wird http://contosouniversity.com.vserver01.cytanium.com.

Benennen Sie das Profil für die Produktion. (Wählen Sie die **Profil** Registerkarte, und klicken Sie auf **Profile verwalten** dafür).

Schließen der **Webveröffentlichung** Assistenten, um die Änderungen zu speichern.

In einer realen Anwendung, in der die Datenbank in der Produktion aktualisiert wurde, würden Sie zwei zusätzliche Schritte jetzt tun, bevor Sie veröffentlichen:

1. Hochladen von *app\_offline.htm*Siehe die [in der Produktionsumgebung bereitstellen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Tutorial.
2. Verwenden der **Manager für Dateiserver** Feature der Systemsteuerung Cytanium zum Kopieren der *Aspnet-Prod.sdf* und *School-Prod.sdf* Dateien aus der Produktionsstandort in der *App\_Daten* -Ordner des Projekts ContosoUniversity. Dadurch wird sichergestellt, dass die Daten, die Sie für die neue SQL Server-Datenbank bereitstellen, um die neuesten Updates, die von Ihrer Produktionswebsite enthält.

In der **klicken Sie auf Webveröffentlichung mit einem** Symbolleiste, stellen Sie sicher, dass die **Produktion** Profil ausgewählt ist, und klicken Sie dann auf **veröffentlichen**.

Wenn Sie hochgeladen haben <em>app\_offline.htm</em> vor der Veröffentlichung, müssen Sie mit der <strong>Manager für Dateiserver</strong> Hilfsprogramm in der Systemsteuerung Cytanium löschen <em>app\_offline.</em> Htm, bevor Sie testen. Sie können auch gleichzeitig löschen. die <em>.sdf</em> Dateien aus dem <em>App\_Daten</em> Ordner.

Sie können jetzt öffnen Sie einen Browser und öffnen Sie die URL Ihrer öffentlichen Website zum Testen der Anwendung der gleichen Weise wie Sie nach der Bereitstellung in der testumgebung.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Wechseln auf SQL Server Express LocalDB in der Entwicklung

Wie in der Übersicht erläutert wurde, ist es im Allgemeinen empfohlen, die die gleiche Datenbank-Engine in der Entwicklung zu verwenden, die Sie in Test- und produktionsumgebungen verwenden. (Beachten Sie, dass der Vorteil bei der Entwicklung mit SQL Server Express ist, dass die Datenbank genauso in Ihren Umgebungen für Entwicklungs-, Test- und produktionsumgebungen funktioniert.) In diesem Abschnitt werden Sie das Projekt ContosoUniversity einrichten, SQL Server Express LocalDB zu verwenden, wenn Sie die Anwendung aus Visual Studio ausführen.

Die einfachste Möglichkeit zum Ausführen dieser Migration besteht darin, Code First und das Mitgliedschaftssystem sowohl neue Datenbanken für die Entwicklung für Sie erstellen. Mit dieser Methode zum Migrieren, sind drei Schritte erforderlich:

1. Ändern Sie die Verbindungszeichenfolgen, um neue SQL Express LocalDB-Datenbanken anzugeben.
2. Führen Sie die Web Site Administration Tool, um einen Benutzer mit Administratorrechten zu erstellen. Dadurch wird die Mitgliedschaftsdatenbank erstellt.
3. Verwenden Sie den Code First-Migrationen-Update-Database-Befehl zum Erstellen und das Seeding der Datenbank.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Aktualisieren von Verbindungszeichenfolgen in der Datei "Web.config"

Öffnen der *"Web.config"* -Datei und Ersetzen Sie die `connectionStrings` Element mit dem folgenden Code:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Erstellen der Mitgliedschaftsdatenbank

In **Projektmappen-Explorer**, wählen Sie das ContosoUniversity-Projekt, und klicken Sie dann auf **ASP.NET-Konfiguration** in die **Projekt** Menü.

Wählen Sie die Registerkarte "Sicherheit".

Klicken Sie auf **erstellen oder Verwalten von Rollen**, und erstellen Sie eine **Administrator** Rolle.

Zurück zur Registerkarte "Sicherheit".

Klicken Sie auf **Benutzer erstellen**, und wählen Sie dann die **Administrator** Kontrollkästchen, und erstellen Sie einen Benutzer namens "Admin"

Schließen der **Websiteverwaltungs-Tool**.

### <a name="creating-the-school-database"></a>Erstellen die Datenbank "School"

Öffnen Sie das Fenster der Paket-Manager-Konsole.

In der **Standardprojekt** Dropdown-Liste, wählen Sie das Projekt ContosoUniversity.DAL.

Geben Sie folgenden Befehl ein:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Code First-Migrationen angewendet wird, die anfängliche Migration, die die Datenbank wird erstellt und wendet dann die Migration AddBirthDate, dann führt er die Seed-Methode.

Führen Sie die Website durch Drücken von STRG + F5. Wie für die Test- und produktionsumgebungen Umgebungen führen die **hinzufügen Schüler/Studenten** Seite, fügen Sie einen neuen Studenten hinzu und zeigen Sie dann den neuen Studenten in der **Schüler/Studenten** Seite. Dadurch wird sichergestellt, dass die Datenbank "School" erstellt und initialisiert wurde und Sie verfügen über Lese- und Schreibzugriff darauf.

Wählen Sie die **Update Gutschriften** Seite, und melden Sie sich stellen Sie sicher, dass die Mitgliedschaftsdatenbank bereitgestellt wurde und Sie darauf zugreifen können. Wenn Sie Ihre Benutzerkonten nicht migriert haben, erstellen Sie ein Administratorkonto, und wählen Sie dann die **Update Gutschriften** Seite, um sicherzustellen, dass sie funktioniert.

## <a name="cleaning-up-sql-server-compact-files"></a>Bereinigen von SQL Server Compact-Dateien

Sie brauchen mehr Dateien und NuGet-Pakete, die zur Unterstützung von SQL Server Compact enthalten sind. Wenn Sie möchten (dieser Schritt ist nicht erforderlich), Sie können nicht benötigte Dateien und Verweise bereinigen.

In **Projektmappen-Explorer**, löschen Sie die *.sdf* Dateien aus der *App\_Daten* Ordner und die *amd64* und *X86* Ordner aus der *Bin* Ordner.

In **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe (nicht auf eines der Projekte), und klicken Sie dann auf **NuGet-Pakete für Projektmappe verwalten**.

Klicken Sie im linken Bereich die **NuGet-Pakete verwalten** wählen Sie im Dialogfeld **installierte Pakete**.

Wählen Sie die **EntityFramework.SqlServerCompact** packen, und klicken Sie auf **verwalten**.

In der **Projekte auswählen** (Dialogfeld), werden beide Projekte ausgewählt. Um das Paket in beiden Projekten deinstallieren möchten, deaktivieren Sie beide Kontrollkästchen, und klicken Sie dann **OK**.

Klicken Sie im Dialogfeld mit der Frage, wenn Sie die abhängigen Pakete auch deinstallieren möchten, klicken Sie auf Nein. Einer davon ist das Entity Framework-Paket, das Sie beibehalten müssen.

Das gleiche Verfahren zum Deinstallieren der **SqlServerCompact** Paket. (Die Pakete müssen in genau dieser Reihenfolge deinstalliert werden, da die **EntityFramework.SqlServerCompact** -Paket benötigt die **SqlServerCompact** Paket.)

Sie haben nun erfolgreich SQL Server Express und die Vollversion von SQL Server migriert. In den nächsten sehen Tutorial machen Sie eine andere Datenbank ändern, und Sie Änderungen in der Datenbank bereitstellen, wenn Ihre Test- und produktionsumgebungen Datenbanken SQL Server Express und die Vollversion von SQL Server verwenden.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
