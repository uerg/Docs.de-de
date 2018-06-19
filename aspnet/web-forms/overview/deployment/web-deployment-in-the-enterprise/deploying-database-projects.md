---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Bereitstellen von Datenbankprojekten | Microsoft Docs
author: jrjlee
description: 'Hinweis: In vielen Bereitstellungsszenarios benötigen Sie die Möglichkeit, inkrementelle Updates an einer bereitgestellten Datenbank zu veröffentlichen. Die Alternative besteht darin, erneut erstellen...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: a0b3871ea098b549271bce2b9d5f0c24f9ca8a9c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30882500"
---
<a name="deploying-database-projects"></a>Bereitstellen von Datenbankprojekten
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> In vielen Bereitstellungsszenarios benötigen Sie die Möglichkeit, inkrementelle Updates an einer bereitgestellten Datenbank zu veröffentlichen. Die Alternative ist die Datenbank bei jeder Bereitstellung neu erstellen, was bedeutet, dass Sie keine Daten in der vorhandenen Datenbank verloren. Bei der Arbeit mit Visual Studio 2010 ist mit VSDBCMD den empfohlenen Ansatz für inkrementelle datenbankveröffentlichung. Allerdings wird die nächste Version von Visual Studio und die Publishing Web Pipeline (WPP) Tools enthalten, die unterstützt inkrementelle direkt zu veröffentlichen.


Wenn Sie die Projektmappe Contact Manager in Visual Studio 2010 öffnen, sehen Sie sich, dass das Datenbankprojekt ein Ordner "Konfigurationseigenschaften" enthält, die vier Dateien enthält.

![](deploying-database-projects/_static/image1.png)

Zusammen mit der Projektdatei (*ContactManager.Database.dbproj* in diesem Fall), diese Dateien werden verschiedene Aspekte des Prozesses Build- und Bereitstellungsprozess gesteuert:

- Die *Database.sqlcmdvars* Datei enthält die Werte für alle SQLCMD-Variablen, die Sie verwenden, wenn Sie das Projekt bereitstellen. Jeder Projektmappenkonfiguration (z. B. Debug und Release) kann eine andere SQLCMDVARS-Datei angegeben.
- Die *Database.sqldeployment* Datei bietet bereitstellungsspezifischen-Einstellungen, z. B., ob die Sortierung in Ihrem Projekt oder die Sortierung des Zielservers, definiert werden soll, ob die Zieldatenbank erneut erstellen jedes Uhrzeit aus, oder ändern Sie einfach die vorhandene Datenbank auf dem neuesten Stand zu bringen und so weiter. Jeder Projektmappenkonfiguration kann eine andere SQLDEPLOYMENT-Datei angegeben.
- Die *Database.sqlpermissions* Datei ist ein XML-Dokument, das Sie verwenden können, um alle Berechtigungen zu definieren, in die Zieldatenbank hinzufügen möchten. Alle Projektmappenkonfigurationen verwenden die gleiche SQLPERMISSIONS-Datei.
- Die *Database.sqlsettings* -Datei gibt die Datenbankebene Eigenschaften beim Erstellen der Datenbank, wie die Sortierung zu verwenden, das Verhalten der Vergleichsoperatoren, und so weiter verwendet. Alle Projektmappenkonfigurationen verwenden die gleiche sqlsettings--Datei.

Es ist lohnt, können Sie diese Dateien in Visual Studio öffnen, und machen Sie sich mit dem Inhalt vertraut.

Wenn Sie ein Datenbankprojekt erstellen, erstellt der Buildprozess zwei Dateien:

- Ein *Datenbankschema* (DBSCHEMA-Datei). Beschreibt das Schema der Datenbank, die Sie im XML-Format erstellen möchten.
- Ein *Bereitstellungsmanifest* (DEPLOYMANIFEST-Datei). Diese Datei enthält alle Informationen, die zum Erstellen und Bereitstellen der Datenbank erforderlich. Er verweist auf die DBSCHEMA-Datei zusammen mit anderen Ressourcen, wie z. B. Anweisungen zur Bereitstellung (die SQLDEPLOYMENT-Datei) und alle SQL­Skripts vor oder nach der Bereitstellung.

Dies zeigt die Beziehung zwischen den folgenden Ressourcen:

![](deploying-database-projects/_static/image2.png)

Wie Sie sehen können, sind die sqlsettings- und die Datei sqlpermissions Eingaben für den Buildprozess. Zusammen mit der Datenbankprojektdatei werden diese Dateien verwendet, die Schemadatei für die Datenbank zu erstellen. SQLDEPLOYMENT-Datei und der SQLCMDVARS-Datei, die während des Erstellungsprozesses unverändert durchlaufen. Das Bereitstellungsmanifest gibt den Speicherort der das Datenbankschema, die SQLDEPLOYMENT-Datei, die SQLCMDVARS-Datei und alle SQL­Skripts vor oder nach der Bereitstellung an.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>Gründe für die Verwendung VSDBCMD ein Datenbankprojekt bereitstellen?

Es gibt verschiedene unterschiedliche Ansätze zum Bereitstellen von Datenbankprojekten. Allerdings sind nicht alle von ihnen für die Bereitstellung eines Datenbankprojekts auf Remoteservern in einer unternehmensumgebung geeignet ist. Erwägen Sie, aus einer Datenbank-projektbereitstellung soll. Im Enterprise-Bereitstellungsszenarien werden wohl eher möchten:

- Die Fähigkeit zum Bereitstellen des Datenbankprojekts von einem Remotestandort aus.
- Die Fähigkeit, inkrementelle Updates zu einer vorhandenen Datenbank vornehmen.
- Die Fähigkeit, Skripts vor der Bereitstellung oder Skripts nach der Bereitstellung enthalten.
- Die Fähigkeit, die Bereitstellung für mehrere zielumgebungen anzupassen.
- Die Möglichkeit zum Bereitstellen von Datenbankprojekt als Teil einer größeren, in der Regel ein Skript erstellt, einstufiger lösungsbereitstellung.

Es gibt drei wichtigsten Ansätze, die Sie verwenden können, um ein Datenbankprojekt bereitstellen:

- Sie können die neue Bereitstellungsfunktion mit dem Datenbank-Projekttyp in Visual Studio 2010 verwenden. Beim Erstellen und einer Datenbankprojekt in Visual Studio 2010 bereitstellen, verwendet der Bereitstellungsprozess das Bereitstellungsmanifest, um eine SQL-basierte Bereitstellung-Datei, die spezifisch für die Buildkonfiguration zu generieren. Dadurch wird die Datenbank erstellt, wenn er nicht bereits vorhanden ist oder alle notwendigen Änderungen an der Datenbank vornehmen, wenn sie bereits vorhanden ist. Verwenden Sie SQLCMD.exe, um diese Datei auf dem Zielserver auszuführen, oder Sie können Visual Studio erstellen und führen Sie die Datei festlegen. Der Nachteil dieses Ansatzes ist, dass Sie nur über eingeschränkte Kontrolle über die bereitstellungseinstellungen haben. Sie müssen möglicherweise häufig auch so ändern Sie die SQL-Bereitstellungsdatei um umgebungsspezifische Werte bereitzustellen. Nur können Sie diesen Ansatz aus einem Computer mit Visual Studio 2010 installiert, und der Entwickler müssten kennen, und geben Sie Verbindungszeichenfolgen und Anmeldeinformationen für alle zielumgebungen.
- Sie können die Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy) [Bereitstellen einer Datenbank als Teil eines Webanwendungsprojekts](https://msdn.microsoft.com/library/dd465343.aspx). Dieser Ansatz ist jedoch viel komplexer, wenn Sie ein Datenbankprojekt bereitstellen, anstatt eine vorhandene lokale Datenbank auf einen Zielserver einfach zu replizieren möchten. Sie können Web Deploy zum Ausführen von SQL-Bereitstellungsskript, das das Datenbankprojekt generiert, aber zu diesem Zweck konfigurieren, müssen Sie eine benutzerdefinierte WPP Targets-Datei für das Webanwendungsprojekt zu erstellen. Dadurch wird der Bereitstellungsprozess eine beträchtliche Menge an Komplexität hinzugefügt. Darüber hinaus ist Web Deploy direkt inkrementelle Updates unterstützt nicht zu vorhandenen Datenbanken. Weitere Informationen zu diesen Ansatz, finden Sie unter [die Publishing Web-Pipeline Paket Datenbankprojekt erweitern bereitgestellten SQL-Datei](https://go.microsoft.com/?linkid=9805121).
- Das Hilfsprogramm VSDBCMD können Sie die Datenbank bereitstellen über das Datenbankschema oder das Bereitstellungsmanifest. Sie können VSDBCMD.exe im ein MSBuild-Ziel aufrufen, die Sie Datenbanken als Teil einer größeren, skriptgesteuerten Bereitstellungsprozess veröffentlichen können. Sie können die Variablen in der SQLCMDVARS-Datei und viele andere Eigenschaften der von einem VSDBCMD-Befehl, der Ihnen ermöglicht, Ihre Bereitstellung für unterschiedliche Umgebungen anpassen, ohne mehrere Buildkonfigurationen erstellen, überschreiben. VSDBCMD Differenzierung stellt Funktionen bereit, was bedeutet, dass nur die erforderlichen Änderungen zum Ausrichten einer Zieldatenbank mit Datenbankschema erleichtern. VSDBCMD bietet außerdem eine Vielzahl von Befehlszeilenoptionen, die Sie eine präzisere Kontrolle über den Bereitstellungsprozess erteilen.

In dieser Übersicht sehen Sie sich, dass MSBuild VSDBCMD mit den Ansatz zu einem typischen Unternehmensszenario für die Bereitstellung am besten geeignet ist:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| Unterstützt die Remotebereitstellung? | Ja | Ja | Ja |
| Unterstützt inkrementelle Updates? | Ja | Nein | Ja |
| Unterstützt Skripts, vor und nach-Bereitstellung? | Ja | Ja | Ja |
| Unterstützt die Bereitstellung von Multi-Umgebung? | Eingeschränkt | Eingeschränkt | Ja |
| Unterstützt skriptgesteuerten Bereitstellung? | Eingeschränkt | Ja | Ja |

Im weiteren Verlauf dieses Themas beschreibt die Verwendung von VSDBCMD mit MSBuild-Datenbankprojekte bereitstellen.

## <a name="understanding-the-deployment-process"></a>Grundlegendes zu den Bereitstellungsprozess

Das Hilfsprogramm VSDBCMD können Sie eine Datenbank mithilfe der Datenbankschema (die DBSCHEMA-Datei) oder das Bereitstellungsmanifest (die DEPLOYMANIFEST-Datei) bereitstellen. In der Praxis ist verwenden fast immer das Bereitstellungsmanifest, Sie als das Bereitstellungsmanifest können Sie die Standardwerte für die verschiedenen Eigenschaften der Bereitstellung zur Verfügung gestellt und identifizieren alle vor oder nach der Bereitstellung SQL­Skripts, die Sie ausführen möchten. Mit diesem Befehl VSDBCMD beispielsweise dient zum Bereitstellen der **ContactManager** Datenbank mit einem Datenbankserver in einer testumgebung:


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


In diesem Fall gilt Folgendes:

- Die **/a** (oder **/Action**) Switch gibt an, was VSDBCMD möchten. Sie können festlegen, um **Import** oder **bereitstellen**. Die **Import** Option wird verwendet, um eine DBSCHEMA-Datei aus einer vorhandenen Datenbank generieren und die **bereitstellen** Option wird verwendet, um eine DBSCHEMA-Datei in einer Zieldatenbank bereitzustellen.
- Die **/manifest** (oder **/ManifestFile**) Switch identifiziert die DEPLOYMANIFEST-Datei, die Sie bereitstellen möchten. Wenn Sie stattdessen die DBSCHEMA-Datei verwenden möchten, verwenden Sie die **/Modell** (oder **/ModelFile**) wechseln.
- Die **/cs** (oder **' / ConnectionString '**)-Switch bietet die Verbindungszeichenfolge für den Zielserver für die Datenbank. Beachten Sie, dass dies der Name der Datenbank nicht&#x2014;VSDBCMD muss für die Verbindung mit dem Server zum Erstellen der Datenbank; Er muss nicht mit einer einzelnen Datenbank hergestellt. Wenn Ihre DEPLOYMANIFEST-Datei eine Verbindungszeichenfolge enthält, können Sie diese Option weglassen. Wenn Sie den Switch trotzdem verwenden, wird der Switch-Wert der DeployManifest-Wert überschrieben.
- Die <strong>/p:TargetDatabase</strong> Eigenschaft enthält den Namen, die Sie in die Zieldatenbank bei der Erstellung zuweisen möchten. Dies überschreibt den Wert für die <strong>TargetDatabase</strong> Eigenschaft in der DEPLOYMANIFEST-Datei. Sie können die <strong>/p:</strong> <em>[Eigenschaftenname]</em>Syntax, um eine Vielzahl von Bereitstellungseigenschaften festgelegt und alle SQLCMD-Variablen zu überschreiben, die in der SQLCMDVARS-Datei deklariert.
- Die **/dd+** (oder **/DeployToDatabase+**) gibt an, dass eine Bereitstellung erstellen, und klicken Sie auf die zielumgebung bereitgestellt werden sollen. Bei Angabe von **/dd-**, ohne den Schalter bzw. VSDBCMD ein Bereitstellungsskript generiert, aber nicht in der zielumgebung bereitgestellt. Dieser Schalter ist oft die Quelle der zu Verwirrung und wird ausführlicher im nächsten Abschnitt erläutert.
- Die **/script** (oder **/DeploymentScriptFile**) Switch gibt an, in dem Sie das Bereitstellungsskript generieren möchten. Dieser Wert wirkt sich nicht auf den Bereitstellungsprozess aus.

Weitere Informationen zu VSDBCMD, finden Sie unter [Command-Line Reference for VSDBCMD. EXE-Datei (Bereitstellung und Schemaimport)](https://msdn.microsoft.com/library/dd193283.aspx) und [Vorgehensweise: Vorbereiten einer Datenbank für die Bereitstellung über eine Eingabeaufforderung mit VSDBCMD. EXE-Datei](https://msdn.microsoft.com/library/dd193258.aspx).

Ein Beispiel, wie Sie aus einer MSBuild-Projektdatei VSDBCMD verwenden können, finden Sie unter [Verständnis des Build-Prozesses](understanding-the-build-process.md). Beispiele für Datenbank-bereitstellungseinstellungen für mehrere Umgebungen zu konfigurieren, finden Sie unter [Anpassen von Datenbank-Bereitstellungen für mehrere Umgebungen](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Grundlegendes zu den DeployToDatabase-Switch

Das Verhalten der **/dd** oder **/DeployToDatabase** Switch, hängt davon ab, ob Sie mit einer DBSCHEMA oder eine Datei DeployManifest VSDBCMD verwenden. Wenn Sie eine DBSCHEMA-Datei verwenden, ist das Verhalten recht einfach:

- Bei Angabe von **/dd+** oder **/dd**, VSDBCMD ein Bereitstellungsskript generiert und die Datenbank bereitstellen.
- Bei Angabe von **/dd-** oder ohne den Schalter, VSDBCMD wird nur ein Bereitstellungsskript generiert.

Wenn Sie eine Datei DeployManifest verwenden, ist das Verhalten viel komplizierter. Dies ist, da die DEPLOYMANIFEST-Datei in einen Eigenschaftsnamen **DeployToDatabase** , wird auch bestimmt, ob die Datenbank bereitgestellt wird.


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


Der Wert dieser Eigenschaft wird entsprechend die Eigenschaften des Datenbankprojekts festgelegt. Wenn Sie festlegen, die **Bereitstellungsvorgang** auf **erstellen Sie ein Skript für die Bereitstellung (.sql)**, der Wert **"false"**. Wenn Sie festlegen, die **Bereitstellungsvorgang** zu **Bereitstellungsskript (.sql) erstellen und Bereitstellen in der Datenbank**, der Wert **"true"**.

> [!NOTE]
> Diese Einstellungen sind mit einem bestimmten Build-Konfiguration und Plattform verknüpft. Angenommen, Sie konfigurieren, dass die Einstellungen für die **Debuggen** Konfiguration und veröffentlichen Sie dann mit der **Version** Konfiguration, die Einstellungen werden nicht verwendet werden.


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> In diesem Szenario die **Bereitstellungsvorgang** sollte immer festgelegt werden, um **erstellen Sie ein Skript für die Bereitstellung (.sql)**, da Sie nicht, dass Visual Studio 2010, Ihre Datenbank bereitzustellen möchten. Das heißt, die **DeployToDatabase** Eigenschaft muss immer **"false"**.


Wenn eine **DeployToDatabase** Eigenschaft wird angegeben, die **/dd** Switch wird die Eigenschaft nur überschreiben, wenn der Eigenschaftswert ist **"false"**:

- Wenn die **DeployToDatabase** Eigenschaft ist **"false"**, und geben Sie **/dd+** oder **/dd**, VSDBCMD überschreibt die  **DeployToDatabase** Eigenschaft und die Datenbank bereitstellen.
- Wenn die **DeployToDatabase** Eigenschaft **"false"**, und geben Sie **/dd-** oder ohne den Schalter, VSDBCMD stellt die Datenbank nicht bereit.
- Wenn die **DeployToDatabase** Eigenschaft **"true"**, VSDBCMD ignoriert die Schalter und die Datenbank bereitstellen.
- In jedem Fall, unabhängig davon, ob Sie die Datenbank als auch bereitstellen, wird ein Bereitstellungsskript generiert.

## <a name="conclusion"></a>Schlussbemerkung

Dieses Thema liefert einen Überblick über die erstellungs-und Bereitstellung für Datenbankprojekte in Visual Studio 2010. Es wird beschrieben, wie Sie VSDBCMD.exe mit MSBuild verwenden können, um die Bereitstellung des Enterprise-Skalierung zu unterstützen.

Weitere Informationen zur Funktionsweise in der Praxis finden Sie unter [Anpassen von Datenbank-Bereitstellungen für mehrere Umgebungen](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Weiterführende Themen

Informationen zum Datenbank-Bereitstellungen anpassen, indem Sie eine separate Bereitstellungskonfigurationsdatei für jede Umgebung erstellen, finden Sie unter [Anpassen von Datenbank-Bereitstellungen für mehrere Umgebungen](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Anleitungen zum Konfigurieren von Datenbank-Rollenmitgliedschaften durch Ausführen eines Skripts nach der Bereitstellung finden Sie unter [Bereitstellen von Datenbank-Rollenmitgliedschaften auf Testumgebungen](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Um Hilfe bei der Verwaltung der Teil der individuelle Herausforderungen, Mitgliedschaft Datenbanken zu erzwingen, finden Sie unter [Mitgliedschaft-Datenbanken bereitstellen, um Unternehmensumgebungen](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Die folgenden Themen auf MSDN enthalten umfassenderen Anweisungen und Hintergrundinformationen zu Visual Studio-Datenbankprojekte und den Datenbank-Bereitstellungsprozess:

- [Visual Studio 2010 SQL Server Database Projects](https://msdn.microsoft.com/library/ff678491.aspx)
- [Verwalten von Datenbankänderungen](https://msdn.microsoft.com/library/aa833404.aspx)
- [Vorgehensweise: Vorbereiten einer Datenbank für die Bereitstellung über eine Eingabeaufforderung mit VSDBCMD. EXE-DATEI](https://msdn.microsoft.com/library/dd193258.aspx)
- [Eine Übersicht über Datenbank-Build und Bereitstellung](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Zurück](deploying-web-packages.md)
> [Weiter](creating-and-running-a-deployment-command-file.md)
