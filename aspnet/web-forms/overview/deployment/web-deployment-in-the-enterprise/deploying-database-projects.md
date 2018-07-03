---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Bereitstellen von Datenbankprojekten | Microsoft-Dokumentation
author: jrjlee
description: 'Hinweis: Die Vielzahl von Bereitstellungsszenarios benötigen Sie die Möglichkeit, um inkrementelle Updates in einer bereitgestellten Datenbank zu veröffentlichen. Die Alternative besteht darin, neu erstellen...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 3d261acbd6f8dab60a02c21546d8bc276de486ba
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374962"
---
<a name="deploying-database-projects"></a>Bereitstellen von Datenbankprojekten
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> In vielen Bereitstellungsszenarios benötigen Sie die Möglichkeit, inkrementelle Updates an einer bereitgestellten Datenbank zu veröffentlichen. Die Alternative ist die Datenbank bei jeder Bereitstellung, neu erstellen, was bedeutet, dass Sie alle Daten in der vorhandenen Datenbank verloren gehen. Wenn Sie mit Visual Studio 2010 arbeiten, ist das mit VSDBCMD die empfohlene Vorgehensweise für inkrementelle datenbankveröffentlichung aus. Allerdings wird die nächste Version von Visual Studio und das Web Publishing-Pipeline (WPP) Tools enthalten, die unterstützt inkrementelle direkt zu veröffentlichen.


Wenn Sie die beispiellösung Contact Manager in Visual Studio 2010 öffnen, sehen Sie sich, dass das Projekt einen Ordner "Properties" enthält, die vier Dateien enthält.

![](deploying-database-projects/_static/image1.png)

Zusammen mit der Projektdatei (*ContactManager.Database.dbproj* in diesem Fall), diese Dateien steuern verschiedene Aspekte des Build & Deployment-Prozesses:

- Die *Database.sqlcmdvars* Datei enthält die Werte für alle SQLCMD-Variablen, die Sie verwenden, wenn Sie das Projekt bereitstellen. Jeder Projektmappenkonfiguration (z. B. "Debug" und "Release") kann es sich um eine andere SQLCMDVARS-Datei angegeben.
- Die *Database.sqldeployment* Datei enthält die Bereitstellung-spezifische Einstellungen wie angibt, ob die Sortierung definiert, die in Ihrem Projekt oder die Sortierung des Zielservers, verwendet angibt, ob die Zieldatenbank neu erstellt. jede Zeit, oder ändern Sie einfach die vorhandene Datenbank auf dem neuesten Stand zu bringen und so weiter. Jeder Projektmappenkonfiguration kann es sich um eine andere SQLDEPLOYMENT-Datei angeben.
- Die *Database.sqlpermissions* Datei ist ein XML-Dokument, das Sie verwenden können, um Berechtigungen zu definieren, in die Zieldatenbank hinzufügen möchten. Alle Projektmappenkonfigurationen verwenden die gleichen SQLPERMISSIONS-Datei.
- Die *Database.sqlsettings* Datei gibt an, die auf Datenbankebene Eigenschaften beim Erstellen der Datenbank, wie bei der Sortierung zu verwenden, das Verhalten der Vergleichsoperatoren, und so weiter. Alle Projektmappenkonfigurationen verwenden die gleiche sqlsettings--Datei.

Es ist, sollten Sie sich einen Moment Zeit, diese Dateien in Visual Studio öffnen, und machen Sie sich mit dem Inhalt vertraut.

Wenn Sie ein Datenbankprojekt erstellen, erstellt der Buildprozess zwei Dateien:

- Ein *Datenbankschema* (DBSCHEMA-Datei). Hier wird beschrieben, das Schema der Datenbank, die Sie in der XML-Format erstellen möchten.
- Ein *Bereitstellungsmanifest* (DEPLOYMANIFEST-Datei). Diese Datei enthält alle Informationen, die zum Erstellen und Bereitstellen der Datenbank erforderlich. Es verweist auf die DBSCHEMA-Datei zusammen mit anderen Ressourcen, wie in den bereitstellungsanweisungen (die SQLDEPLOYMENT-Datei) und alle SQL­Skripts vor oder nach der Bereitstellung.

Dies zeigt die Beziehung zwischen diesen Ressourcen:

![](deploying-database-projects/_static/image2.png)

Wie Sie sehen können, sind die sqlsettings- und die Datei sqlpermissions Eingaben für den Buildprozess. Zusammen mit der Datenbank-Projektdatei werden diese Dateien verwendet, um die Schemadatei für die Datenbank zu erstellen. Die SQLDEPLOYMENT-Datei und der SQLCMDVARS-Datei pass-through-Buildprozess unverändert. Das Bereitstellungsmanifest gibt den Speicherort der das Schema der Datenbank, die SQLDEPLOYMENT-Datei, die SQLCMDVARS-Datei und alle SQL­Skripts vor oder nach der Bereitstellung an.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Warum verwenden Sie VSDBCMD, um ein Datenbankprojekt bereitstellen?

Es gibt verschiedene Ansätze zum Bereitstellen von Datenbankprojekten. Allerdings sind nicht alle von ihnen für die Bereitstellung eines Datenbankprojekts auf Remoteserver in einer unternehmensumgebung geeignet ist. Erwägen Sie die gewünschten aus einer Datenbank-Projekt-Bereitstellung. In Enterprise-Bereitstellungsszenarien sind Sie eher die:

- Die Fähigkeit zum Bereitstellen des Datenbankprojekts von einem Remotestandort aus.
- Die Fähigkeit, inkrementelle Updates an einer vorhandenen Datenbank vornehmen.
- Die Möglichkeit, Skripts vor der Bereitstellung oder Skripts nach der Bereitstellung enthalten.
- Die Fähigkeit zum Anpassen der Bereitstellung in zielumgebungen mit mehreren.
- Die Möglichkeit zum Bereitstellen des Datenbankprojekts als Teil eines größeren, in der Regel ein Skript erstellt, Bereitstellung von einem Schritt.

Es gibt drei wichtigsten Ansätze, die Sie verwenden können, um ein Datenbankprojekt bereitstellen:

- Sie können die neue Bereitstellungsfunktion mit der Datenbank-Projekttyp in Visual Studio 2010 verwenden. Beim Erstellen und ein Datenbankprojekt in Visual Studio 2010 bereitstellen, wird während des Bereitstellungsvorgangs das Bereitstellungsmanifest zum Generieren einer SQL-basierte Bereitstellung-Datei, die spezifisch für die Buildkonfiguration verwendet. Dadurch wird die Datenbank erstellt, wenn es nicht bereits vorhanden ist oder alle notwendigen Änderungen an der Datenbank vornehmen, wenn sie bereits vorhanden ist. Können Sie SQLCMD.exe diese Datei auf dem Zielserver ausgeführt, oder Sie können Visual Studio zum Erstellen und führen Sie die Datei festlegen. Der Nachteil dieses Ansatzes ist, dass Sie die Kontrolle über die bereitstellungseinstellungen nur eingeschränkte haben. Sie müssen häufig auch so ändern Sie die SQL-Bereitstellung-Datei, um umgebungsspezifische Werte bereitzustellen. Nur können Sie diesen Ansatz aus einem Computer mit Visual Studio 2010 installiert, und die Entwickler müssen wissen, und geben Sie Verbindungszeichenfolgen und Anmeldeinformationen für alle zielumgebungen.
- Sie können das Internet Information Services (IIS)-Webbereitstellungstool (Web Deploy) verwenden, um [Bereitstellen einer Datenbank als Teil eines Webanwendungsprojekts](https://msdn.microsoft.com/library/dd465343.aspx). Dieser Ansatz ist jedoch sehr viel komplizierter, wenn Sie ein Datenbankprojekt bereitstellen, anstatt einfach eine vorhandene lokale Datenbank auf einem Zielserver zu replizieren möchten. Sie können die Web Deploy, um das SQL-Bereitstellungsskript ausführen, das das Datenbankprojekt generiert, aber zu diesem Zweck konfigurieren, müssen Sie eine benutzerdefinierte WPP Targets-Datei für das Webanwendungsprojekt zu erstellen. Dadurch wird der Bereitstellungsprozess eine beträchtliche Menge an Komplexität hinzugefügt. Darüber hinaus unterstützt Web Deploy direkt inkrementelle Updates für vorhandene Datenbanken nicht. Weitere Informationen zu diesem Ansatz finden Sie unter [erweitern die Veröffentlichung Webpipeline Datenbankprojekt Paket bereitgestellten SQL-Datei](https://go.microsoft.com/?linkid=9805121).
- Sie können das VSDBCMD-Dienstprogramm verwenden, um der Datenbank bereitzustellen, über das Datenbankschema oder das Bereitstellungsmanifest. Sie können ein MSBuild-Ziel VSDBCMD.exe aufrufen Hier können Sie die Datenbanken als Teil eines größeren, im Skript enthaltenen Deployment-Prozesses zu veröffentlichen. Sie können angeben, überschreiben die Variablen in Ihren SQLCMDVARS-Datei und viele andere Eigenschaften der von einem VSDBCMD-Befehl, mit der Sie die Bereitstellung für unterschiedliche Umgebungen anpassen, ohne zu mehreren Buildkonfigurationen erstellen können. VSDBCMD Differenzierung stellt Funktionen bereit, was, dass nur die erforderlichen Änderungen bedeutet an eine Zieldatenbank mit Ihr Datenbankschema erleichtern. VSDBCMD bietet auch eine Vielzahl von Befehlszeilenoptionen, die Sie eine präzisere Kontrolle über den Bereitstellungsprozess bieten.

In dieser Übersicht sehen Sie sich, dass MSBuild VSDBCMD mit den Ansatz zu einem typischen Unternehmensszenario für die Bereitstellung am besten geeignet ist:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Unterstützt die Bereitstellung von Remotezugriff? | Ja | Ja | Ja |
| Unterstützt inkrementelle Updates werden? | Ja | Nein | Ja |
| Unterstützt Skripts, Pre-und post-Bereitstellung? | Ja | Ja | Ja |
| Unterstützt die Bereitstellung von mehreren Umgebungen? | Eingeschränkt | Eingeschränkt | Ja |
| Unterstützt die skriptgesteuerte Bereitstellung? | Eingeschränkt | Ja | Ja |

Im weiteren Verlauf dieses Themas wird die Verwendung von VSDBCMD mit MSBuild zum Bereitstellen von Datenbankprojekten beschrieben.

## <a name="understanding-the-deployment-process"></a>Grundlegendes zu den Bereitstellungsprozess

Die VSDBCMD-Dienstprogramm können Sie eine Datenbank mit dem Datenbankschema (die DBSCHEMA-Datei) oder das Bereitstellungsmanifest (die DEPLOYMANIFEST-Datei) bereitstellen. In der Praxis ist verwenden fast immer das Bereitstellungsmanifest, Sie, wie das Bereitstellungsmanifest können Sie die Standardwerte für die verschiedenen Eigenschaften der Bereitstellung zur Verfügung gestellt, und identifizieren alle vor oder nach der Bereitstellung SQL­Skripts, die Sie ausführen möchten. Mit diesem Befehl VSDBCMD dient beispielsweise zum Bereitstellen der **ContactManager** Datenbank mit einem Datenbankserver in einer testumgebung:


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


In diesem Fall gilt Folgendes:

- Die **/a** (oder **/Action**) Option gibt an, was VSDBCMD tun soll. Sie können dies festlegen, um **Import** oder **bereitstellen**. Die **Import** Option wird verwendet, um eine DBSCHEMA-Datei aus einer vorhandenen Datenbank generieren und die **bereitstellen** Option wird verwendet, um eine DBSCHEMA-Datei in eine Zieldatenbank bereitzustellen.
- Die **/manifest** (oder **MANIFESTFILE**) Switch identifiziert die DEPLOYMANIFEST-Datei, die Sie bereitstellen möchten. Wenn Sie die DBSCHEMA-Datei verwenden möchten, verwenden Sie die **/Modell** (oder **/ModelFile**) wechseln.
- Die **/cs** (oder **/ConnectionString**)-Switch bietet die Verbindungszeichenfolge für den Zielserver für die Datenbank. Beachten Sie, dass dies der Name der Datenbank umfassen keine&#x2014;VSDBCMD muss an den Server zum Erstellen der Datenbank; eine Verbindung herstellen Es muss nicht mit einer einzelnen Datenbank herstellen. Wenn Ihre DEPLOYMANIFEST-Datei eine Verbindungszeichenfolge enthält, können Sie diese Option weglassen. Wenn Sie den Schalter trotzdem verwenden, wird der Switch-Wert, der DeployManifest-Wert überschrieben.
- Die <strong>/p:TargetDatabase</strong> -Eigenschaft gibt den Namen, die Sie in die Zieldatenbank bei der Erstellung zuweisen möchten. Dies überschreibt den Wert für die <strong>TargetDatabase</strong> Eigenschaft in der Datei DeployManifest. Sie können die <strong>/p:</strong> <em>[Eigenschaftenname]</em>Syntax, um eine Vielzahl von Eigenschaften für die Bereitstellung festzulegen und um alle SQLCMD-Variablen zu überschreiben, die in der SQLCMDVARS-Datei deklariert.
- Die **/dd+** (oder **/DeployToDatabase+**) diese Option gibt an, dass Sie eine Bereitstellung erstellen, und klicken Sie in der zielumgebung bereitstellen möchten. Bei Angabe von **/dd-**, oder lassen Sie den Switch, VSDBCMD ein Bereitstellungsskript generiert, aber nicht in der zielumgebung bereitgestellt. Dieser Schalter ist häufig die Quelle der Verwirrung und wird im nächsten Abschnitt ausführlicher erläutert.
- Die **/script** (oder **/DeploymentScriptFile**) Option gibt an, in dem Sie das Bereitstellungsskript generieren möchten. Dieser Wert wirkt sich nicht auf den Bereitstellungsprozess aus.

Weitere Informationen zu VSDBCMD, finden Sie unter [Befehlszeilenreferenz für VSDBCMD. EXE-Datei ("Bereitstellung" und "Schema importieren")](https://msdn.microsoft.com/library/dd193283.aspx) und [wie: Vorbereiten einer Datenbank für die Bereitstellung über eine Eingabeaufforderung mit VSDBCMD. EXE-Datei](https://msdn.microsoft.com/library/dd193258.aspx).

Ein Beispiel, wie Sie aus einer MSBuild-Projektdatei VSDBCMD verwenden können, finden Sie unter [Verständnis des Prozesses erstellen](understanding-the-build-process.md). Beispiele für die Bereitstellung der datenbankeinstellungen für mehrere Umgebungen konfigurieren, finden Sie unter [Anpassen von Datenbankbereitstellungen für mehrere Umgebungen](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Grundlegendes zu den DeployToDatabase-Schalter

Das Verhalten der **dd /** oder **/DeployToDatabase** Switch, hängt davon ab, ob Sie VSDBCMD mit einer DBSCHEMA-Datei oder eine DEPLOYMANIFEST-Datei verwenden. Wenn Sie eine DBSCHEMA-Datei verwenden, ist das Verhalten recht einfach:

- Bei Angabe von **/dd+** oder **dd /**, VSDBCMD wird ein Bereitstellungsskript generieren und Bereitstellen der Datenbank.
- Bei Angabe von **/dd-** oder ohne den Schalter, VSDBCMD wird nur ein Bereitstellungsskript generiert.

Wenn Sie eine Datei DeployManifest verwenden, ist das Verhalten wesentlich komplizierter. Dies ist, da die DEPLOYMANIFEST-Datei in einen Eigenschaftsnamen **DeployToDatabase** , wird auch bestimmt, ob die Datenbank bereitgestellt wird.


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


Der Wert dieser Eigenschaft wird gemäß den Eigenschaften des Datenbankprojekts festgelegt. Setzen Sie die **bereitstellen-Aktion** zu **erstellen Sie ein Skript für die Bereitstellung (. SQL)**, der Wert **"false"**. Setzen Sie die **bereitstellen-Aktion** zu **Bereitstellungsskript (. SQL) erstellen und Bereitstellen von in die Datenbank**, der Wert **"true"**.

> [!NOTE]
> Diese Einstellungen sind mit einem bestimmten Build-Konfiguration und Plattform zugeordnet. Angenommen, Sie konfigurieren, dass die Einstellungen für die **Debuggen** Konfiguration, und klicken Sie dann veröffentlichen Sie mithilfe der **Version** Konfiguration Ihre Einstellungen nicht verwendet werden.


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> In diesem Szenario die **bereitstellen-Aktion** sollte immer festgelegt werden, um **erstellen Sie ein Skript für die Bereitstellung (. SQL)**, da Sie nicht, dass Visual Studio 2010 zum Bereitstellen Ihrer Datenbank möchten. Das heißt, die **DeployToDatabase** Eigenschaft muss immer **"false"**.


Wenn eine **DeployToDatabase** -Eigenschaft angegeben ist, die **dd /** Switch wird die Eigenschaft nur überschreiben, wenn der Eigenschaftswert ist **"false"**:

- Wenn die **DeployToDatabase** Eigenschaft **"false"**, und geben Sie **/dd+** oder **dd /**, VSDBCMD überschreibt die  **DeployToDatabase** Eigenschaft und die Datenbank bereitstellen.
- Wenn die **DeployToDatabase** Eigenschaft **"false"**, und geben Sie **/dd-** oder ohne den Schalter, VSDBCMD die Datenbank nicht bereitgestellt.
- Wenn die **DeployToDatabase** Eigenschaft **"true"**, VSDBCMD ignoriert den Schalter und die Datenbank bereitstellen.
- In jedem Fall, unabhängig davon, ob Sie die Datenbank sowie bereitstellen, wird ein Bereitstellungsskript generiert.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema wird eine Übersicht über den Build & Deployment-Prozess für Datenbankprojekte in Visual Studio 2010 bereitgestellt. Es wird beschrieben, wie Sie VSDBCMD.exe mit MSBuild verwenden können, um die unternehmensweite Bereitstellung unterstützt.

Weitere Informationen dazu, wie dies in der Praxis funktioniert, finden Sie unter [Anpassen von Datenbankbereitstellungen für mehrere Umgebungen](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Weiterführende Themen

Informationen zum datenbankbereitstellungen anpassen, indem Sie eine separate Bereitstellung-Konfigurationsdatei für jede Umgebung erstellen, finden Sie unter [Anpassen von Datenbankbereitstellungen für mehrere Umgebungen](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Anleitungen zum Konfigurieren von Datenbank-Rollenmitgliedschaften durch Ausführen eines Skripts nach der Bereitstellung finden Sie unter [Bereitstellen von Datenbankrollenmitgliedschaften in Test-Umgebungen](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Anleitungen zum Verwalten Sie einige besondere Anforderungen die Mitgliedschaft von Datenbanken zu erzwingen, finden Sie unter [Bereitstellen von Datenbankrollenmitgliedschaften in Enterprise-Umgebungen](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Diese Themen auf MSDN finden Sie umfassendere Anleitung und Hintergrundinformationen für Visual Studio Database Projects und den Bereitstellungsprozess für die Datenbank:

- [Visual Studio 2010 SQL Server-Datenbankprojekte](https://msdn.microsoft.com/library/ff678491.aspx)
- [Verwalten von Datenbankänderungen](https://msdn.microsoft.com/library/aa833404.aspx)
- [Vorgehensweise: Vorbereiten einer Datenbank für die Bereitstellung über eine Eingabeaufforderung mit VSDBCMD. EXE-DATEI](https://msdn.microsoft.com/library/dd193258.aspx)
- [Eine Übersicht über Datenbank-Build und Bereitstellung](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Zurück](deploying-web-packages.md)
> [Weiter](creating-and-running-a-deployment-command-file.md)
