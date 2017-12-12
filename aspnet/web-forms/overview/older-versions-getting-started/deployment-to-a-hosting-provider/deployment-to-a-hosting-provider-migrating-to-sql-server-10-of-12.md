---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Migrieren zu SQL Server - 10 12 | Microsoft Docs'
author: tdykstra
description: "Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual das..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: 31d83a11488212ab0ff83494d5e896ffcbeaa8a4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Migrieren zu SQL Server - 10 12
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie das Web veröffentlichen Update installieren. Eine Einführung in der Reihe, finden Sie unter [im ersten Lernprogramm, in der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, die die Bereitstellungsfeatures nach der RC-Version von Visual Studio 2012 eingeführt wurde, wird gezeigt, wie SQL Server-Editionen als SQL Server Compact bereitstellen, und zeigt die Vorgehensweise beim Bereitstellen in Azure App Service-Web-Apps, finden Sie unter [ASP.NET Web Deploy Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

In diesem Lernprogramm wird gezeigt, wie von SQL Server Compact mit SQL Server zu migrieren. Ein Grund dafür sollen ist SQL Server-Funktionen nutzen, die SQL Server Compact, z. B. gespeicherte Prozeduren, Trigger, Sichten oder Replikation nicht unterstützt. Weitere Informationen zu den Unterschieden zwischen SQL Server Compact und SQL Server finden Sie unter der [Bereitstellen von SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) Lernprogramm.

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express im Vergleich zur Vollversion von SQL Server für die Entwicklung

Nachdem Sie entschieden haben, auf SQL Server aktualisieren, können Sie SQL Server- oder SQL Server Express in Ihrer Entwicklungs- und Test verwenden möchten. Zusätzlich zu den Unterschieden in Toolsupport und Funktionen des Datenbankmoduls gibt es Unterschiede in Implementierungen von Schlüsselspeicheranbietern zwischen SQL Server Compact und anderen Versionen von SQL Server. Diese Unterschiede können dazu führen, dass den gleichen Code, um unterschiedliche Ergebnisse zu generieren. Deshalb, wenn Sie SQL Server Compact als Ihre Entwicklungsdatenbank beibehalten möchten, sollten Sie sorgfältig Ihre Website in SQL Server oder SQL Server Express in einer testumgebung vor jeder Bereitstellung bis hin zur Produktion testen.

Im Gegensatz zu SQL Server Compact SQL Server Express ist im Prinzip das gleiche Datenbankmodul und den gleichen Anbieter für .NET als Vollversion von SQL Server verwendet. Wenn Sie mit SQL Server Express testen, können Sie darauf vertrauen, dieselben Ergebnisse abrufen, wie Sie mit SQL Server werden. Können Sie die meisten den gleichen Datenbanktools mit SQL Server Express, die mit SQL Server verwendet werden können (eine wichtige Ausnahme wird [SQL Server Profiler](https://msdn.microsoft.com/en-us/library/ms181091.aspx)), und darüber hinaus andere Funktionen wie gespeicherte Prozeduren, Sichten, Trigger, der SQL Server unterstützt. und die Replikation. (Sie müssen in der Regel jedoch Vollversion von SQL Server in einer Produktionswebsite verwenden. SQL Server Express kann in einer freigegebenen Hostingumgebung ausführen, jedoch nicht für, die entwickelt wurde, und viele hosting-Anbieter nicht unterstützt wird.)

Bei Verwendung von Visual Studio 2012 wählen Sie in der Regel SQL Server Express LocalDB für Ihre Entwicklungsumgebung ein, da dies ist, was standardmäßig mit Visual Studio installiert ist. Allerdings funktioniert LocalDB nicht in IIS, deshalb für Ihre testumgebung Sie SQL Server oder SQL Server Express verwenden müssen.

### <a name="combining-databases-versus-keeping-them-separate"></a>Kombinieren von Datenbanken im Vergleich zu separaten aktualisieren

Die University Contoso-Anwendung verfügt über zwei SQL Server Compact-Datenbanken: der Mitgliedschaftsdatenbank (*aspnet.sdf*) und die Anwendungsdatenbank (*School.sdf*). Beim Migrieren, können Sie diese Datenbanken auf zwei separaten Datenbanken oder auf eine einzelne Datenbank migrieren. Möglicherweise möchten sie kombinieren, um Datenbankjoins zwischen Ihrer Anwendung und Ihre Mitgliedschaftsdatenbank zu ermöglichen. Ihren hostingplan möglicherweise auch einen Grund für diese kombinieren angeben. Z. B. des hostenden Anbieters möglicherweise fallen Kosten für mehrere Datenbanken oder möglicherweise mehr als eine Datenbank selbst nicht zulässig. Also bei der Cytanium Lite-Konto, das für dieses Lernprogramm verwendet wird, wodurch nur eine einzelne SQL Server-Datenbank hostet.

In diesem Lernprogramm fügen Sie auf diese Weise zwei Datenbanken migrieren:

- Migrieren Sie zu zwei LocalDB-Datenbanken in der Entwicklungsumgebung.
- Migrieren Sie zu zwei SQL Server Express-Datenbanken in der testumgebung.
- Migrieren Sie zu einem kombinierten vollständige SQL Server-Datenbank in der produktionsumgebung.

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="installing-sql-server-express"></a>Installieren von SQL Server Express

SQL Server Express wird automatisch mit Visual Studio 2010 standardmäßig installiert, aber standardmäßig ist es nicht mit Visual Studio 2012 installiert. Klicken Sie zum Installieren von SQL Server 2012 Express auf den folgenden link

- [SQLServer Express 2012](https://www.microsoft.com/en-us/download/details.aspx?id=29062)

Wählen Sie *ENU/X64/SQLEXPR\_X64\_ENU.exe* oder *ENU/X86/SQLEXPR\_X86\_ENU.exe*, und akzeptieren Sie die Standardeinstellung im Installations-Assistenten Einstellungen. Weitere Informationen zu Installationsoptionen finden Sie unter [Installieren von SQL Server 2012 vom Installations-Assistenten (Setup)](https://msdn.microsoft.com/en-us/library/ms143219.aspx).

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>Erstellen von SQL Server Express-Datenbanken für die Testumgebung

Der nächste Schritt besteht darin die ASP.NET-Mitgliedschaft und School-Datenbanken zu erstellen.

Aus der **Ansicht** Menü die Option **Server-Explorer** (**Datenbank-Explorer** in Visual Web Developer), und klicken Sie dann mit der rechten Maustaste **Datenverbindungen**, und wählen Sie **neue SQL Server-Datenbank erstellen**.

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

In der **neue SQL Server-Datenbank erstellen** Dialogfeld Geben Sie ". \SQLExpress" in der **Servernamen** Feld und "Aspnet-Test" in der **neuer Datenbankname** Feld, und klicken Sie auf **OK**.

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

Führen Sie das gleiche Verfahren zum Erstellen einer neuen SQL Server Express School-Datenbank mit dem Namen "School-Test" aus.

(Sie "Test" auf diesen Datenbanknamen anfügen daran, dass später Sie eine zusätzliche Instanz von jeder Datenbank für die Entwicklungsumgebung erstellen, und Sie in der Lage, die zwei Sätze von Datenbanken zu unterscheiden sein müssen.)

**Server-Explorer** zeigt nun zwei neuen Datenbanken.

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>Erstellen eine Grant-Skript für die neuen Datenbanken

Wenn die Anwendung in IIS auf dem Entwicklungscomputer ausgeführt wird, greift auf die Anwendung die Datenbank mithilfe von Anmeldeinformationen für den Standardanwendungspool. Standardmäßig haben die Identität des Anwendungspools jedoch keine Berechtigung zum Öffnen von Datenbanken. Daher müssen Sie zum Ausführen eines Skripts, um diese Berechtigung zu erteilen. In diesem Abschnitt erstellen Sie das Skript ausgeführt wird weiter unten, um sicherzustellen, dass die Anwendung die Datenbanken öffnen kann, wenn sie in IIS ausgeführt wird.

In der Projektmappe *SolutionFiles* Ordner, die Sie in erstellt die [in der Produktionsumgebung bereitstellen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Lernprogramm erstellen Sie eine neue SQL-Datei mit dem Namen *Grant.sql*. Kopieren Sie die folgenden SQL-Befehle in die Datei, und speichern Sie und schließen Sie die Datei:

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> Dieses Skript dient zum Arbeiten mit SQL Server 2008 und mit den IIS-Einstellungen in Windows 7, wie sie in diesem Lernprogramm angegeben sind. Wenn Sie eine andere Version von SQL Server oder von Windows verwenden, oder wenn Sie IIS auf Ihrem Computer unterschiedlich eingerichtet, Änderungen an diesem Skript möglicherweise erforderlich. Weitere Informationen zu SQL Server-Skripts finden Sie unter [SQL Server-Onlinedokumentation](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Sicherheitshinweis** dieses Skript gibt Db\_kontobesitzerberechtigungen verfügen, um den Benutzer, die zur Laufzeit in der produktionsumgebung müssen also auf die Datenbank zugreift. In einigen Szenarien empfiehlt es sich um ein Benutzer mit vollständigen Datenbankschema aktualisieren Sie Berechtigungen nur für die Bereitstellung, und geben Sie einen anderen Benutzer, der berechtigt nur zum Lesen und Schreiben von Daten zur Laufzeit. Weitere Informationen finden Sie unter **überprüfen die automatischen Änderungen von "Web.config" für die Code First-Migrationen** in [Bereitstellung in IIS als Testumgebung](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).


## <a name="configuring-database-deployment-for-the-test-environment"></a>Konfigurieren von Datenbankbereitstellung für die Testumgebung

Als Nächstes konfigurieren Sie Visual Studio, damit sie die folgenden Aufgaben für jede Datenbank ausführen kann:

- Generiert ein SQL­Skript, das die Quelldatenbank-Struktur (Tabellen, Spalten, Einschränkungen usw.) in der Zieldatenbank erstellt.
- Generiert ein SQL­Skript, das Einfügen der Daten von der Quelldatenbank in die Tabellen in der Zieldatenbank an.
- Führen Sie die generierten Skripts und das Skript zum erteilen, das Sie in der Zieldatenbank erstellt.

Öffnen der **Projekteigenschaften** und wählen Sie die **SQL packen/veröffentlichen** Registerkarte.

Stellen Sie sicher, dass **aktiv (Release)** oder **Release** ausgewählt ist, der **Konfiguration** Dropdown-Liste.

Klicken Sie auf **aktivieren Sie diese Seite**.

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

Die **SQL packen/veröffentlichen** Registerkarte ist normalerweise deaktiviert, da sie eine ältere Bereitstellungsmethode angibt. In den meisten Szenarien sollten Sie die Bereitstellung im Konfigurieren der **Web veröffentlichen** Assistenten. Migrieren von SQL Server Compact in SQL Server oder SQL Server Express ist ein Sonderfall, der für den diese Methode eine gute Wahl ist.

Klicken Sie auf **Import aus der Datei "Web.config"**.

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio sucht nach Verbindungszeichenfolgen in der *"Web.config"* Datei sucht nach einer für die Mitgliedschaftsdatenbank und für die Datenbank "School", und fügt eine Zeile für jede Verbindungszeichenfolge in der **Datenbankeinträge**  Tabelle. Die gefundenen Verbindungszeichenfolgen sind für die vorhandenen SQL Server Compact-Datenbanken, und im nächste Schritt konfigurieren wie und wo diese Datenbanken bereitstellen.

Eingegebene Bereitstellung datenbankeinstellungen in der **Eintrag Datenbankdetails** weiter unten im Abschnitt der **Datenbankeinträge** Tabelle. Die Einstellungen angezeigt, der **Eintrag Datenbankdetails** Abschnitt beziehen sich auf den je in Zeile der **Datenbankeinträge** Tabelle ausgewählt ist, wie in der folgenden Abbildung dargestellt.

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurieren von Bereitstellungseinstellungen für die Mitgliedschaftsdatenbank

Wählen Sie die **DefaultConnection Bereitstellung** Zeile in der **Datenbankeinträge** Tabelle, um Einstellungen zu konfigurieren, die für die Mitgliedschaftsdatenbank gelten.

In **Verbindungszeichenfolge für Zieldatenbank**, geben Sie eine Verbindungszeichenfolge, die auf die neue SQL Server Express Mitgliedschaftsdatenbank verweist. Sie erhalten die Verbindungszeichenfolge, Sie müssen **Server-Explorer**. In **Server-Explorer**, erweitern Sie **Datenverbindungen** , und wählen Sie die **AspnetTest** Datenbank, klicken Sie dann aus der **Eigenschaften** Fenster Kopieren der **Verbindungszeichenfolge** Wert.

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

Hier wird dieselbe Verbindungszeichenfolge reproduziert werden:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

Kopieren Sie diese Verbindungszeichenfolge in **Verbindungszeichenfolge für Zieldatenbank** in der **SQL packen/veröffentlichen** Registerkarte.

Stellen Sie sicher, dass **Daten und/oder Schema aus einer vorhandenen Datenbank mithilfe von Pull** ausgewählt ist. Dies ist, was bewirkt, dass SQL-Skripts automatisch generiert und in der Zieldatenbank ausgeführt werden.

Die **Verbindungszeichenfolge für die Quelldatenbank** Wert extrahiert wird, aus der *"Web.config"* Datei- und verweist auf die Entwicklung SQL Server Compact-Datenbank. Dies ist die Quelldatenbank, die verwendet wird, um die Skripts generieren, die weiter unten in der Zieldatenbank ausgeführt wird. Da Sie die Produktionsversion der Datenbank bereitstellen möchten, ändern Sie "Aspnet-Dev.sdf" in "Aspnet-Prod.sdf".

Änderung **Datenbank Skriptoptionen** aus **nur Schema** auf **Schema und Daten**, da Sie zum Kopieren der Daten (Benutzerkonten und Rollen), sowie die Struktur der Datenbank möchten.

Um die Bereitstellung konfigurieren, um die Grant-Skripts ausführen, die Sie zuvor erstellt haben, müssen Sie sie zum Hinzufügen der **Datenbankskripts** Abschnitt. Klicken Sie auf **Skript hinzufügen**, und klicken Sie in der **SQL-Skripts hinzufügen** Dialogfeld Feld, navigieren Sie zu dem Ordner, in dem Sie das Skript zum Erteilen gespeichert (Dies ist der Ordner, der die Projektmappendatei enthält). Wählen Sie die Datei mit dem Namen *Grant.sql*, und klicken Sie auf **öffnen**.

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

Die Einstellungen für die **DefaultConnection Bereitstellung** für die Zeile im **Datenbankeinträge** jetzt wie in der folgenden Abbildung aussehen:

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurieren von Bereitstellungseinstellungen für die Datenbank "School"

Wählen Sie als Nächstes die **SchoolContext Bereitstellung** Zeile in der **Datenbankeinträge** Tabelle, um die bereitstellungseinstellungen für die Datenbank "School" zu konfigurieren.

Sie können die gleiche Methode, die Sie zuvor beim Abrufen der Verbindungszeichenfolge für die neue SQL Server Express-Datenbank verwendet. Kopieren Sie diese Verbindungszeichenfolge in **Verbindungszeichenfolge für Zieldatenbank** in der **SQL packen/veröffentlichen** Registerkarte.

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

Stellen Sie sicher, dass **Daten und/oder Schema aus einer vorhandenen Datenbank mithilfe von Pull** ausgewählt ist.

Die **Verbindungszeichenfolge für die Quelldatenbank** Wert extrahiert wird, aus der *"Web.config"* Datei- und verweist auf die Entwicklung SQL Server Compact-Datenbank. Ändern Sie "School-Dev.sdf" in "School-Prod.sdf", um die Produktionsversion der Datenbank bereitzustellen. (Eine Datei School Prod.sdf nie erstellt, in der App\_Datenordner, sodass Sie die Datei aus der testumgebung für die App kopieren müssen\_Datenordner im ContosoUniversity-Projektordner höher.)

Änderung **Datenbank Skriptoptionen** auf **Schema und Daten**.

Auch das Skript zum Gewähren von Lese- und Schreibberechtigung für diese Datenbank die Identität des Anwendungspools, fügen Sie daher ausgeführt werden soll die *Grant.sql* -Skriptdatei aus, wie Sie für die Mitgliedschaftsdatenbank ausgeführt haben.

Wenn Sie fertig sind, die Einstellungen für die **SchoolContext Bereitstellung** für die Zeile im **Datenbankeinträge** der folgenden Abbildung aussehen:

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

Speichern Sie die Änderungen an der **SQL packen/veröffentlichen** Registerkarte.

Kopieren der *School-Prod.sdf* -Datei von der *c:\inetpub\wwwroot\ContosoUniversity\App\_Daten* Ordner die *App\_Daten* Ordner in Das ContosoUniversity-Projekt.

### <a name="specifying-transacted-mode-for-the-grant-script"></a>Angeben des transaktiven Modus für das Skript zum Erteilen

Der Bereitstellungsprozess generiert Skripts, die das Datenbankschema und die Daten bereitstellen. Standardmäßig werden diese Skripts in einer Transaktion ausführen. Allerdings werden benutzerdefinierte Skripts (z. B. die Grant-Skripts) standardmäßig nicht in einer Transaktion ausgeführt. Wenn der Bereitstellungsprozess Transaktionsmodi enthält, erhalten beim Ausführen von Skripts während der Bereitstellung möglicherweise ein Timeoutfehler auftreten. In diesem Abschnitt haben Sie die Projektdatei bearbeiten, zum Konfigurieren der benutzerdefinierten Skripts, die in einer Transaktion ausgeführt.

In **Projektmappen-Explorer**, mit der rechten Maustaste die **ContosoUniversity** Projekt, und wählen Sie **Projekt entladen**.

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

Klicken Sie dann mit der rechten Maustaste erneut auf des Projekts, und wählen Sie **ContosoUniversity.csproj bearbeiten**.

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Der Visual Studio-Editor zeigt den XML-Inhalt der Projektdatei. Beachten Sie, dass es mehrere gibt `PropertyGroup` Elemente. (In der Abbildung wird der Inhalt der `PropertyGroup` haben Elemente ausgelassen wurden.)

![Projekt-Datei-Editor-Fenster](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

Der ersten Abfrage, die keine `Condition` -Attribut angegeben wird, ist für die Einstellungen, die unabhängig von gelten erstellen. Eine `PropertyGroup` Element gilt nur für die Debug-Buildkonfiguration (Beachten Sie die `Condition` Attribut), eine gilt nur für die Release-Build-Konfiguration und einer gilt nur für die Test-Build-Konfiguration. Innerhalb der `PropertyGroup` -Element für die Releasebuildkonfiguration, sehen Sie eine `PublishDatabaseSettings` Element, das die Einstellungen enthält, die Sie auf eingegeben der **SQL packen/veröffentlichen** Registerkarte. Es ist ein `Object` -Element, dem diese Skripts Grant entspricht, angegeben (Beachten Sie, dass die beiden Instanzen von "Grant.sql"). Standardmäßig die `Transacted` Attribut von der `Source` Element für jedes Skript Grant ist `False`.

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

Ändern Sie den Wert, der die `Transacted` Attribut des der `Source` Element `True`.

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

Speichern und schließen Sie die Projektdatei anschließend mit der Maustaste des Projekts im **Projektmappen-Explorer** , und wählen Sie **Projekt erneut laden**.

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>Einrichten von "Web.config" Transformationen für die Verbindungszeichenfolgen

Die Verbindungszeichenfolgen für die neue SQL Express, die Sie eingegeben haben Datenbanken, auf die **SQL packen/veröffentlichen** Registerkarte werden nur für das Aktualisieren der Zieldatenbank während der Bereitstellung von Web Deploy verwendet. Sie benötigen weiterhin einrichten *"Web.config"* Transformationen, damit die Verbindungszeichenfolgen in der bereitgestellten *"Web.config"* Datei, zeigen Sie auf den neuen SQL Server Express-Datenbanken. (Bei Verwendung der **SQL packen/veröffentlichen** Registerkarte Verbindungszeichenfolgen kann nicht konfiguriert werden, in dem Veröffentlichungsprofil ein.)

Öffnen *Web.Test.config* , und Ersetzen Sie die `connectionStrings` Element mit dem `connectionStrings` Element im folgenden Beispiel. (Stellen Sie sicher, dass Sie nur das ConnectionStrings-Element nicht von den umgebenden Code kopieren, das hier gezeigt wird, um den Kontext bereitzustellen.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

Dieser Code bewirkt, dass die `connectionString` und `providerName` Attribute der einzelnen `add` zu ersetzende Element in der bereitgestellten *"Web.config"* Datei. Diese Verbindungszeichenfolgen sind nicht identisch mit derjenigen, die Sie eingegeben, in haben der **SQL packen/veröffentlichen** Registerkarte. Die Einstellung "MultipleActiveResultSets = True" wurde hinzugefügt werden, da es für das Entity Framework und Universal Providers erforderlich ist.

## <a name="installing-sql-server-compact"></a>Installieren von SQL Server Compact

Das SqlServerCompact NuGet-Paket bietet SQL Server Compact-Datenbank Modul Assemblys für die Anwendung von Contoso-Universität. Aber jetzt nicht die Anwendung jedoch Web Deploy, die auf die SQL Server Compact-Datenbanken zu lesen, um das Erstellen von Skripts in SQL Server-Datenbanken ausgeführt werden muss. Um Web Deploy wird zum Lesen von SQL Server Compact-Datenbanken zu aktivieren, installieren Sie SQL Server Compact auf dem Entwicklungscomputer über den folgenden Link: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).

## <a name="deploying-to-the-test-environment"></a>Bereitstellung in der Testumgebung

Um in der testumgebung zu veröffentlichen, müssen Sie ein Veröffentlichungsprofil zu erstellen, die für die Verwendung konfiguriert ist die **SQL packen/veröffentlichen** Registerkarte für die Datenbank, die anstelle der Einstellungen für die Veröffentlichung Profil Datenbank veröffentlichen.

Löschen Sie zuerst das vorhandene Profil für den Test.

In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und klicken Sie auf **veröffentlichen**.

Wählen Sie die **Profil** Registerkarte.

Klicken Sie auf **Profile verwalten**.

Wählen Sie **Test**, klicken Sie auf **entfernen**, und klicken Sie dann auf **schließen**.

Schließen der **Web veröffentlichen** Assistenten, um diese Änderung zu speichern.

Als Nächstes erstellen Sie ein neues Testprofil, und verwenden sie zum Veröffentlichen des Projekts.

In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und klicken Sie auf **veröffentlichen**.

Wählen Sie die **Profil** Registerkarte.

Wählen Sie  **&lt;neu... &gt;**  aus der Dropdownliste aus, und geben Sie "Test" als Namen des Profils.

In der **Dienst-URL** geben *"localhost"*.

In der **Website/Anwendung** geben *Default Web Site/ContosoUniversity*.

In der **Ziel-URL** geben `http://localhost/ContosoUniversity/`.

Klicken Sie auf **Weiter**.

Die **Einstellungen** Registerkarte warnt Sie, den **SQL packen/veröffentlichen** Registerkarte konfiguriert wurde, und er bietet Ihnen die Gelegenheit zu überschreiben, indem Sie Sie aktivieren auf die neue Datenbank, die publishing Verbesserungen. Für diese Bereitstellung möchte Sie nicht außer Kraft setzen die **SQL packen/veröffentlichen** Registerkarte Einstellungen, klicken Sie daher einfach **Weiter**.

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

Eine Nachricht auf der **Vorschau** Registerkarte gibt an, dass **veröffentlichen sind keine Datenbanken ausgewählt**, aber dies bedeutet, dass die datenbankveröffentlichung im Veröffentlichungsprofil nicht konfiguriert ist.

Klicken Sie auf **Veröffentlichen**.

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio stellt die Anwendung bereit und öffnet den Browser, um die Startseite der Website in der testumgebung. Führen Sie die Lehrkräfte-Seite, um anzuzeigen, dass die gleichen Daten angezeigt, die Sie zuvor gesehen haben. Führen Sie die **hinzufügen Studenten** , hinzufügen einen neuen Studenten, und zeigen Sie dann den neue Studenten in die **Studenten** Seite. Dies stellt sicher, dass Sie die Datenbank zu aktualisieren. Wählen Sie die **Update Gutschriften** Seite (dazu müssen anzumelden), stellen Sie sicher, dass die Mitgliedschaftsdatenbank bereitgestellt wurde und Sie darauf zugreifen.

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>Erstellen eine SQL Server-Datenbank für die Produktionsumgebung

Nun, dass Sie in der testumgebung bereitgestellt haben, können Sie die Bereitstellung bis hin zur Produktion eingerichtet. Sie beginnen Sie für die testumgebung ausgeführt haben, durch das Erstellen einer Datenbank für die Bereitstellung auf. Wie bereits aus der Übersicht erwähnt, kann Cytanium Lite hostingplan nur eine einzelne SQL Server-Datenbank, sodass Sie nur eine Datenbank, nicht zwei eingerichtet werden. Alle Tabellen und Daten aus der Mitgliedschaft und School SQL Server Compact-Datenbanken werden in einer SQL Server-Datenbank in der Produktion bereitgestellt werden.

Wechseln Sie zur Systemsteuerung Cytanium am [http://panel.cytanium.com](http://panel.cytanium.com). Halten Sie die Maus über **Datenbanken** , und klicken Sie dann auf **SQL Server 2008**.

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

In der **SQL Server 2008** auf **Create Database**.

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

Nennen Sie die Datenbank "School", und klicken Sie auf **speichern**. (Die Seite fügt automatisch das Präfix "Contosou", damit der effektiven Name "ContosouSchool" werden.)

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

Klicken Sie auf der gleichen Seite auf **Create User**. Auf Cytaniums-Servern, statt die integrierte Sicherheit von Windows, und lassen die Identität des Anwendungspools die Datenbank zu öffnen erstellen Sie einen Benutzer, der beim Öffnen der Datenbank aufweist. Fügen Sie die Anmeldeinformationen des Benutzers auf die Verbindungszeichenfolgen, die in der Produktion *"Web.config"* Datei. In diesem Schritt erstellen Sie diese Anmeldeinformationen an.

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

Füllen Sie die erforderlichen Felder in der **SQL Benutzereigenschaften** Seite:

- Geben Sie "ContosoUniversityUser" als Namen ein.
- Geben Sie ein Kennwort ein.
- Wählen Sie **ContosouSchool** als Standarddatenbank.
- Wählen Sie die **ContosouSchool** Kontrollkästchen.

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>Konfigurieren von Datenbankbereitstellung für die Produktionsumgebung

Nachdem Sie zum Einrichten von bereitstellungseinstellungen für Datenbank bereit sind die **SQL packen/veröffentlichen** Registerkarte, die Sie zuvor für die testumgebung ausgeführt haben.

Öffnen der **Projekteigenschaften** wählen die **SQL packen/veröffentlichen** Registerkarte, und stellen Sie sicher, dass **aktiv (Release)** oder **Version** ist im ausgewählten der **Konfiguration** Dropdown-Liste.

Wenn Sie die bereitstellungseinstellungen für jede Datenbank konfigurieren, ist der wesentliche Unterschied zwischen Sie für Produktions- und Testcomputer Umgebungen Folgendes in der Konfiguration von Verbindungszeichenfolgen. Für die testumgebung eingegebene Datenbankverbindungszeichenfolgen anderes Ziel, aber für die produktionsumgebung wird die Ziel-Verbindungszeichenfolge werden für beide Datenbanken identisch. Dies ist, da Sie beide Datenbanken für eine Datenbank in der Produktion bereitstellen.

### <a name="configuring-deployment-settings-for-the-membership-database"></a>Konfigurieren von Bereitstellungseinstellungen für die Mitgliedschaftsdatenbank

Wählen Sie zum Konfigurieren von Einstellungen, die für die Mitgliedschaftsdatenbank gelten die **DefaultConnection Bereitstellung** Zeile in der **Datenbankeinträge** Tabelle.

In **Verbindungszeichenfolge für Zieldatenbank**, geben Sie eine Verbindungszeichenfolge, die in der neuen SQL Server-Produktionsdatenbank verweist, die Sie gerade erstellt haben. Sie können die Verbindungszeichenfolge aus der e-Mail zur Begrüßung abrufen. Der relevante Teil der e-Mail enthält das folgende Beispiel-Verbindungszeichenfolge:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

Nachdem Sie die drei Variablen zu ersetzen, sieht die Verbindungszeichenfolge, die Sie benötigen in diesem Beispiel aus:

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

Kopieren Sie diese Verbindungszeichenfolge in **Verbindungszeichenfolge für Zieldatenbank** in der **SQL packen/veröffentlichen** Registerkarte.

Stellen Sie sicher, dass **Daten und/oder Schema aus einer vorhandenen Datenbank mithilfe von Pull** noch ausgewählt, und dass **Datenbank Skriptoptionen** ist immer noch **Schema und Daten**.

In der **Datenbankskripts** deaktivieren Sie das Kontrollkästchen neben dem Grant.sql-Skript.

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>Konfigurieren von Bereitstellungseinstellungen für die Datenbank "School"

Wählen Sie als Nächstes die **SchoolContext Bereitstellung** Zeile in der **Datenbankeinträge** Tabelle, um die Einstellungen in der Datenbank "School" zu konfigurieren.

Kopieren Sie die gleiche Verbindungszeichenfolge in **Verbindungszeichenfolge für Zieldatenbank** , die Sie in das Feld für die Mitgliedschaftsdatenbank kopiert.

Stellen Sie sicher, dass **Daten und/oder Schema aus einer vorhandenen Datenbank mithilfe von Pull** noch ausgewählt, und dass **Datenbank Skriptoptionen** ist immer noch **Schema und Daten**.

In der **Datenbankskripts** deaktivieren Sie das Kontrollkästchen neben dem Grant.sql-Skript.

Speichern Sie die Änderungen an der **SQL packen/veröffentlichen** Registerkarte.

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>Einrichten von "Web.config" transformiert für die Verbindungszeichenfolgen für Produktionsdatenbanken

Legen Sie als Nächstes einrichten *"Web.config"* Transformationen, damit die Verbindungszeichenfolgen in der bereitgestellten *"Web.config"* Datei, zeigen Sie auf die neue Produktionsdatenbank. Die Verbindungszeichenfolge, die Sie eingegeben haben, auf die **SQL packen/veröffentlichen** Registerkarte für Web Deploy zu verwenden ist identisch mit der Ausnahme des hinzugefügten verwenden die Anwendung muss die `MultipleResultSets` Option.

Öffnen *Web.Production.config* , und Ersetzen Sie die `connectionStrings` Element mit einer `connectionStrings` -Element, das im folgenden Beispiel ähnelt. (Nur Kopieren der `connectionStrings` -Element, das nicht den umgebenden Tags, die bereitgestellt werden, um den Kontext anzuzeigen.)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

Daraufhin manchmal Beratung ermöglicht, die Aufschluss darüber immer Verschlüsseln von Verbindungszeichenfolgen in der *"Web.config"* Datei. Dies ist möglicherweise geeignet, bei der Bereitstellung auf Servern im Netzwerk Ihren eigenen Unternehmens wurden. Wenn Sie auf einer freigegebenen Hostingumgebung bereitstellen, jedoch können Sie die Sicherheitsmaßnahmen des Hostinganbieters vertrauen, und es ist nicht erforderlich oder praktikabel ist, das Verschlüsseln von Verbindungszeichenfolgen.

## <a name="deploying-to-the-production-environment"></a>In der Produktionsumgebung bereitstellen

Nun können Sie zur Bereitstellung bis hin zur Produktion bereit. Web Deploy wird gelesen, die SQL Server Compact-Datenbanken in Ihrem Projekts *App\_Daten* Ordner und alle Tabellen und Daten in der SQL Server-Produktionsdatenbank neu zu erstellen. Damit eine Veröffentlichung mithilfe der **Web packen/veröffentlichen** tabulatoreinstellungen, müssen Sie ein neues Veröffentlichungsprofil für die Produktion zu erstellen.

Löschen Sie zuerst das vorhandene Profil für die Produktion, wie das Profil Test zuvor.

In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und klicken Sie auf **veröffentlichen**.

Wählen Sie die **Profil** Registerkarte.

Klicken Sie auf **Profile verwalten**.

Wählen Sie **Produktion**, klicken Sie auf **entfernen**, und klicken Sie dann auf **schließen**.

Schließen der **Web veröffentlichen** Assistenten, um diese Änderung zu speichern.

Als Nächstes erstellen Sie ein neues Profil für die Produktion und verwenden sie zum Veröffentlichen des Projekts.

In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und klicken Sie auf **veröffentlichen**.

Wählen Sie die **Profil** Registerkarte.

Klicken Sie auf **Import**, und wählen Sie die PUBLISHSETTINGS-Datei, die Sie zuvor heruntergeladen haben.

Auf der **Verbindung** Ändern der **Ziel-URL** mit der richtigen temporäre URL, die in diesem Beispiel wird http://contosouniversity.com.vserver01.cytanium.com.

Benennen Sie das Profil, bis hin zur Produktion. (Wählen Sie die **Profil** Registerkarte, und klicken Sie auf **Profile verwalten** nachholen).

Schließen der **Web veröffentlichen** Assistenten, um die Änderungen zu speichern.

In einer realen Anwendung, in der die Datenbank in der Produktion wird wurde aktualisiert, würden Sie zwei zusätzliche Schritte jetzt tun, vor dem Veröffentlichen:

1. Hochladen *app\_offline.htm*, entsprechend der [in der Produktionsumgebung bereitstellen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Lernprogramm.
2. Verwenden der **Manager für Dateiserver** Funktion der Systemsteuerung Cytanium zum Kopieren der *Aspnet-Prod.sdf* und *School-Prod.sdf* Dateien von des Produktionsstandorts auf die *App\_Daten* Ordner des Projekts ContosoUniversity. Dadurch wird sichergestellt, dass die Daten, die Sie in die neue SQL Server-Datenbank bereitstellen, die neuesten Updates vorgenommen werden, indem Ihre Produktionswebsite enthält.

In der **Web eine klicken Sie auf Publish** Symbolleiste, stellen Sie sicher, dass die **Produktion** Profil ausgewählt ist, und klicken Sie dann auf **veröffentlichen**.

Wenn Sie hochgeladen *app\_offline.htm* vor dem Veröffentlichen, müssen Sie die **Manager für Dateiserver** Hilfsprogramm in der Systemsteuerung Cytanium löschen *app\_offline.* Htm, bevor Sie testen. Sie können auch zur gleichen Zeit Löschen der *.sdf* Dateien aus der *App\_Daten* Ordner.

Sie können jetzt öffnen Sie einen Browser, und wechseln Sie zu der URL der öffentlichen Website zum Testen der Anwendung über der gleichen Weise wie Sie nach der Bereitstellung in der testumgebung durchgeführt haben.

## <a name="switching-to-sql-server-express-localdb-in-development"></a>Wechseln zu SQL Server Express LocalDB in der Entwicklung

Wie in der Übersicht erwähnt, ist es im Allgemeinen empfohlen, das gleiche Datenbankmodul bei der Entwicklung verwenden, die Sie in Test- und produktionsumgebungen verwenden. (Beachten Sie, dass der Vorteil bei der Entwicklung mit SQL Server Express besteht darin, dass die Datenbank die gleiche in Ihrer Entwicklungs-, Test- und produktionsumgebung Umgebungen geeignet ist.) In diesem Abschnitt richten des Projekts ContosoUniversity Sie SQL Server Express LocalDB zu verwenden, wenn Sie die Anwendung aus Visual Studio ausführen.

Die einfachste Möglichkeit, diese Migration durchführen können Code First ist und das Mitgliedschaftssystem sowohl neue Entwicklung-Datenbanken für Sie erstellen. Verwenden diese Methode zum Migrieren, sind drei Schritte erforderlich:

1. Ändern Sie die Verbindungszeichenfolgen, um neue SQL Express LocalDB-Datenbanken angeben.
2. Führen Sie die Websiteverwaltungs-Tool, um einen Benutzer mit Administratorrechten zu erstellen. Dadurch wird die Mitgliedschaftsdatenbank erstellt.
3. Verwenden Sie die Code First-Migrationen Update-Database-Befehl zum Erstellen und die Anwendungsdatenbank seed.

### <a name="updating-connection-strings-in-the-webconfig-file"></a>Aktualisieren von Verbindungszeichenfolgen in der Datei "Web.config"

Öffnen der *"Web.config"* -Datei und ersetzen die `connectionStrings` -Element durch folgenden Code:

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>Erstellen die Mitgliedschaftsdatenbank

In **Projektmappen-Explorer**, wählen Sie das ContosoUniversity-Projekt, und klicken Sie dann auf **ASP.NET-Konfiguration** in der **Projekt** Menü.

Wählen Sie die Registerkarte "Sicherheit".

Klicken Sie auf **erstellen oder Verwalten von Rollen**, und erstellen Sie eine **Administrator** Rolle.

Zurück zur Registerkarte "Sicherheit".

Klicken Sie auf **Benutzer erstellen**, und wählen Sie dann die **Administrator** Kontrollkästchen, und erstellen Sie einen Benutzer namens "Admin"

Schließen der **Websiteverwaltungs-Tool**.

### <a name="creating-the-school-database"></a>Erstellen die Datenbank "School"

Öffnen Sie das Fenster für die Paket-Manager-Konsole.

In der **Standardprojekt** Dropdown-Liste, wählen Sie das Projekt ContosoUniversity.DAL.

Geben Sie folgenden Befehl ein:

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Code First-Migrationen gilt der anfänglichen Migrations, die die Datenbank erstellt und wendet dann die Migration AddBirthDate aus, und Sie dann bei der Ausführung der Seed-Methode.

Führen Sie den Standort durch Drücken von STRG + F5. Führen Sie für die Test- und produktionsumgebungen Umgebungen ausgeführt haben, die **hinzufügen Studenten** , hinzufügen einen neuen Studenten, und zeigen Sie dann den neue Studenten in die **Studenten** Seite. Dies stellt sicher, dass die Datenbank "School" erstellt und initialisiert wurde und dass Sie über Lese- und Schreibberechtigungen.

Wählen Sie die **Update Gutschriften** Seite, und melden Sie sich vergewissern Sie sich, dass die Mitgliedschaftsdatenbank bereitgestellt wurde und Sie darauf zugreifen können. Wenn Sie Ihre Benutzerkonten nicht migriert haben, erstellen Sie ein Administratorkonto, und wählen Sie dann die **Update Gutschriften** Seite, um sicherzustellen, dass er funktioniert.

## <a name="cleaning-up-sql-server-compact-files"></a>Bereinigen von SQL Server Compact-Dateien

Sie benötigen nicht mehr und NuGet-Pakete, die zur Unterstützung von SQL Server Compact enthalten sind. Wenn Sie möchten (dieser Schritt ist nicht erforderlich), können Sie den Befehl nicht benötigte Dateien und Verweise.

In **Projektmappen-Explorer**, löschen die *.sdf* Dateien aus der *App\_Daten* Ordner und die *amd64* und *X86* Ordner aus der *"bin"* Ordner.

In **Projektmappen-Explorer**mit der rechten Maustaste auf die Projektmappe (nicht eines der Projekte), und klicken Sie dann auf **NuGet-Pakete für Projektmappe verwalten**.

Im linken Bereich des der **NuGet-Pakete verwalten** wählen Sie im Dialogfeld **installierte Pakete**.

Wählen Sie die **EntityFramework.SqlServerCompact** Verpacken, und klicken Sie auf **verwalten**.

In der **wählen Projekte** (Dialogfeld), werden beide Projekte ausgewählt. Um das Paket in beiden Projekten zu deinstallieren, deaktivieren Sie beide Kontrollkästchen, und klicken Sie auf **OK**.

Klicken Sie im Dialogfeld mit der Frage, wenn Sie die abhängigen Pakete auch deinstallieren möchten, klicken Sie auf "Nein". Einer davon ist das Entity Framework-Paket, das beibehalten werden müssen.

Führen Sie dasselbe Verfahren zum Deinstallieren der **SqlServerCompact** Paket. (Die Pakete müssen in genau dieser Reihenfolge deinstalliert werden, da die **EntityFramework.SqlServerCompact** Paket hängt die **SqlServerCompact** Paket.)

Sie haben jetzt erfolgreich nach SQL Server Express und die Vollversion von SQL Server migriert. In den nächsten sehen Lernprogramm machen Sie eine andere Datenbank ändern, und Sie Änderungen an der Datenbank bereitstellen, wenn die Test- und produktionsumgebungen Datenbanken SQL Server Express und die Vollversion von SQL Server verwenden.

>[!div class="step-by-step"]
[Zurück](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
[Weiter](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
