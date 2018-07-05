---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Grundlegendes zum Buildprozess | Microsoft-Dokumentation
author: jrjlee
description: Dieses Thema enthält eine exemplarische Vorgehensweise für eine unternehmensweite Erstellung und Bereitstellung. In diesem Thema beschriebene Ansatz verwendet die benutzerdefinierte Microsoft Build Engin...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 581b7e996bf5aa4c76a6bf3d55a758677c0bf897
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804315"
---
<a name="understanding-the-build-process"></a>Grundlegendes zum Buildprozess
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema enthält eine exemplarische Vorgehensweise für eine unternehmensweite Erstellung und Bereitstellung. In diesem Thema beschriebene Ansatz verwendet benutzerdefinierte Microsoft Build Engine (MSBuild)-Projektdateien, um eine präzisere Kontrolle über alle Aspekte des Prozesses bereitzustellen. In den Projektdateien werden benutzerdefinierte MSBuild-Ziele verwendet, um die Bereitstellung-Hilfsprogramme wie das Webbereitstellungstool (MSDeploy.exe) für das Internet Information Services (IIS) und des Datenbank-bereitstellungs-Hilfsprogramms VSDBCMD.exe ausgeführt werden.
> 
> > [!NOTE]
> > Im vorherigen Thema [Grundlegendes zur Projektdatei](understanding-the-project-file.md), die wichtigsten Komponenten einer MSBuild-Projektdatei beschrieben und führte das Konzept der Split-Projektdateien Bereitstellung in mehreren zielumgebungen zu unterstützen. Wenn Sie nicht bereits mit diesen Konzepten vertraut sind, überprüfen Sie [Grundlegendes zur Projektdatei](understanding-the-project-file.md) vor dem Durcharbeiten dieses Themas.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Grundlegendes zur Projektdatei](understanding-the-project-file.md), in dem der Buildprozess durch gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie die Anweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Build & Deployment-Einstellungen gelten. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="build-and-deployment-overview"></a>Build und Bereitstellung (Übersicht)

In der [Contact Manager-Lösung](the-contact-manager-solution.md), drei Dateien Steuern des Build & Deployment-Prozesses:

- Ein *universelle Projektdatei* (*Publish.proj*). Build & Deployment-Anweisungen, die nicht zwischen zielumgebungen ändern enthält.
- Ein *umgebungsspezifische Projektdatei* (*Env-Dev.proj*). Diese enthält Build- und bereitstellungs-Einstellungen, die für ein bestimmtes zielumgebung spezifisch sind. Beispielsweise können Sie die *Env-Dev.proj* Datei aus, geben Sie die Einstellungen für einen Entwickler oder testumgebung, und erstellen Sie eine andere Datei mit dem Namen *Env-Stage.proj* Einstellungen für eine Stagingumgebung bereitstellen Umgebung.
- Ein *Befehlsdatei* (*veröffentlichen-Dev.cmd*). Diese Datei enthält eine MSBuild.exe-Befehl, der angibt, welches Projekt Dateien ausgeführt werden soll. Sie können eine Befehlsdatei für jede zielumgebung, erstellen, die, in dem jede Datei einen MSBuild.exe-Befehl enthält, der eine andere Umgebung spezifischen Projektdatei angibt. Dadurch wird den Entwickler, die in unterschiedlichen Umgebungen bereitstellen, einfach durch Ausführen der entsprechenden Befehl-Datei.

In der beispiellösung finden Sie diese drei Dateien im Ordner "Publish-Lösung".

![](understanding-the-build-process/_static/image1.png)

Bevor Sie sich diese Dateien im Detail ansehen, werfen wir einen Blick auf die Funktionsweise am gesamten Buildprozess an, wenn Sie diesen Ansatz verwenden. Auf einer hohen Ebene sieht der Build & Deployment-Prozess folgendermaßen aus:

![](understanding-the-build-process/_static/image2.png)

Als erstes, das ausgeführt wird, die für die beiden Dateien Projekt&#x2014;universelle Build & Deployment-Anweisungen enthält, und enthält umgebungsspezifische Einstellungen&#x2014;werden in eine einzelne Projektdatei zusammengeführt. MSBuild arbeitet dann die Anweisungen in der Projektdatei. Jedes der Projekte in der Lösung, mit der Projektdatei für jedes Projekt erstellt. Es ruft dann andere Tools, z.B. Web Deploy (MSDeploy.exe) und das VSDBCMD-Hilfsprogramm für die Bereitstellung von Ihrer Web-Inhalte und Datenbanken in der zielumgebung.

Von Anfang bis Ende führt der Build & Deployment-Prozess folgende Aufgaben aus:

1. Löscht den Inhalt des Ausgabeverzeichnisses, als Vorbereitung für einen neuen Build.
2. Es wird jedes Projekt in der Lösung erstellt:

    1. Für Webprojekte&#x2014;in diesem Fall eine ASP.NET MVC-Webanwendung und einer WCF web Service&#x2014;der Buildprozess erstellt ein Webbereitstellungspaket für die einzelnen Projekte.
    2. Für Datenbankprojekte erstellt der Buildprozess ein Bereitstellungsmanifest (DEPLOYMANIFEST-Datei) für jedes Projekt.
3. Es verwendet das VSDBCMD.exe-Dienstprogramm zum Bereitstellen von jedes Datenbankprojekt in der Projektmappe, die über verschiedene Eigenschaften aus den Projektdateien&#x2014;eine Ziel-Verbindungszeichenfolge und einen Datenbanknamen&#x2014;zusammen mit der DEPLOYMANIFEST-Datei.
4. Er verwendet das MSDeploy.exe-Dienstprogramm zum Bereitstellen von jedem Webprojekt in der Projektmappe, die verschiedene Eigenschaften aus den Projektdateien verwenden, um den Bereitstellungsprozess zu steuern.

Sie können die beispiellösung verwenden, um diesen Prozess im Detail zu verfolgen.

> [!NOTE]
> Anleitungen zum Anpassen der umgebungsspezifischen Projektdateien für Ihre eigenen serverumgebungen finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="invoking-the-build-and-deployment-process"></a>Aufrufen von Build- und Bereitstellungsprozess

Um Contact Manager-Lösung in einer testumgebung für die Entwickler bereitzustellen, führt der Entwickler die *veröffentlichen-Dev.cmd* Befehlsdatei. Dies ruft MSBuild.exe, Angeben von *Publish.proj* wie die Projektdatei zum Ausführen und *Env-Dev.proj* als Parameterwert.


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> Die **/fl** wechseln (kurz für **/FileLogger**) protokolliert die Buildausgabe in einer Datei namens *msbuild.log* im aktuellen Verzeichnis. Weitere Informationen finden Sie unter den [MSBuild-Befehlszeilenreferenz](https://msdn.microsoft.com/library/ms164311.aspx).


An diesem Punkt MSBuild gestartet wird, lädt die *Publish.proj* -Datei und beginnt die Verarbeitung der Anweisungen in dieser. Die erste Anweisung informiert MSBuild, um das Projekt zu importieren, die Datei der **TargetEnvPropsFile** angegeben wird.


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


Die **TargetEnvPropsFile** Parameter gibt an, die *Env-Dev.proj* Datei, sodass Sie MSBuild den Inhalt der führt die *Env-Dev.proj* Eingabedatei in die  *Publish.proj* Datei.

Die nächsten Elemente, die von MSBuild in der zusammengeführten Datei trifft sind Gruppen von Eigenschaften. Eigenschaften werden in der Reihenfolge verarbeitet, in denen sie in der Datei angezeigt werden. MSBuild erstellt ein Schlüssel-Wert-Paar für jede Eigenschaft, vorausgesetzt, dass alle angegebenen Bedingungen erfüllt sind. In der Datei später definierte Eigenschaften überschreibt alle Eigenschaften, die mit dem gleichen Namen, die weiter oben in der Datei definiert. Betrachten Sie beispielsweise die **OutputRoot** Eigenschaften.


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


Wenn MSBuild verarbeitet die erste **OutputRoot** -Element, ein ähnlich benannten Parameter wurde nicht angegeben ist, legt er den Wert des der **OutputRoot** Eigenschaft **... \Publish\Out**. Wenn es trifft die zweite **OutputRoot** Element, wenn das Ergebnis der bedingungsauswertung **"true"**, er überschreibt den Wert des der **OutputRoot** Eigenschaft mit dem Wert von der **OutDir** Parameter.

Das nächste Element, das MSBuild feststellt wird ein einzelnes Elementgruppe enthält ein Element mit dem Namen **ProjectsToBuild**.


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild verarbeitet diese Anweisung durch eine Elementliste erstellen **ProjectsToBuild**. In diesem Fall die Liste enthält einen einzelnen Wert&#x2014;den Pfad und Dateinamen der Projektmappendatei.

An diesem Punkt sind die übrigen Elemente Ziele. Ziele werden aus den Eigenschaften und Elemente unterschiedlich verarbeitet&#x2014;im wesentlichen Ziele werden nicht verarbeitet, es sei denn, sie werden explizit vom Benutzer angegeben wird oder durch ein anderes Konstrukt in der Projektdatei aufgerufen. Bedenken Sie, dass das öffnendes **Projekt** Tag enthält eine **DefaultTargets** Attribut.


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


Dies weist MSBuild zum Aufrufen der **FullPublish** Ziel, wenn Ziele nicht angegeben, wenn MSBuild.exe aufgerufen wird. Die **FullPublish** Ziel keine Aufgaben enthalten; stattdessen gibt er einfach eine Liste der Abhängigkeiten.


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


Mit dieser Abhängigkeit informiert MSBuild, in Reihenfolge zum Ausführen der **FullPublish** Ziel muss es diese Liste von Zielen in der angegebenen Reihenfolge aufrufen:

1. Sie müssen aufrufen, die **Bereinigen** Ziel.
2. Sie müssen aufrufen, die **BuildProjects** Ziel.
3. Sie müssen aufrufen, die **GatherPackagesForPublishing** Ziel.
4. Sie müssen aufrufen, die **PublishDbPackages** Ziel.
5. Sie müssen aufrufen, die **PublishWebPackages** Ziel.

### <a name="the-clean-target"></a>Das Clean-Ziel

Die **Bereinigen** Ziel löscht im Grunde das Ausgabeverzeichnis und seinen gesamten Inhalt, als Vorbereitung für einen neuen Build.


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


Beachten Sie, die das Ziel enthält eine **ItemGroup** Element. Beim Definieren von Eigenschaften oder Elemente in einem **Ziel** -Element, erstellen Sie *dynamische* Eigenschaften und Elemente. Das heißt, nicht die Eigenschaften oder Elemente verarbeitet, bis das Ziel ausgeführt wird. Das Ausgabeverzeichnis möglicherweise nicht vorhanden oder alle Dateien bis der Buildprozess eingeleitet wurde, enthalten, sodass Sie nicht erstellt werden können die  **\_FilesToDelete** als ein statisches Element aufgeführt; Sie müssen warten, bis die Ausführung ausgeführt wird. Daher erstellen Sie die Liste als dynamische Element innerhalb des Ziels.

> [!NOTE]
> In diesem Fall, da die **Bereinigen** Ziel ist die erste ausgeführt werden, gibt es keine Notwendigkeit, eine dynamische Gruppe verwenden. Allerdings ist es empfiehlt sich, die dynamischen Eigenschaften und Elemente in dieser Art von Szenario zu verwenden, wie möglicherweise Zielen in einer anderen Reihenfolge zu einem bestimmten Zeitpunkt ausgeführt werden soll.  
> Sie sollten auch versuchen, um zu vermeiden, deklarieren Elemente, die nie verwendet werden. Wenn Sie Elemente, die nur ein bestimmtes Ziel verwendet werden, sollten Sie in das Ziel, das Entfernen unnötigen Aufwand zum Erstellungsprozess platziert.


Dynamischen Elemente, da die **Bereinigen** Ziel ist recht einfach, und verwendet die integrierte **Nachricht**, **löschen**, und **RemoveDir**Aufgaben an:

1. Senden einer Nachricht an die Protokollierung.
2. Erstellen Sie eine Liste der zu löschenden Dateien an.
3. Löschen Sie die Dateien an.
4. Entfernen Sie das Ausgabeverzeichnis an.

### <a name="the-buildprojects-target"></a>Das Ziel BuildProjects

Die **BuildProjects** Ziel im Grunde alle Projekte in der Beispielprojektmappe erstellt.


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


Dieses Ziel wurde im vorherigen Thema ausführlich beschrieben [Grundlegendes zur Projektdatei](understanding-the-project-file.md), um zu veranschaulichen, wie Aufgaben und Zielen Eigenschaften und Elemente verweisen. An diesem Punkt sind Sie vor allem interessiert die **MSBuild** Aufgabe. Sie können diese Aufgabe verwenden, um mehrere Projekte zu erstellen. Der Task erstellt eine neue Instanz von MSBuild.exe keine; die derzeit ausgeführte Instanz verwendet, um jedes Projekt erstellt werden. Der wichtige Punkte in diesem Beispiel werden die Eigenschaften der Bereitstellung:

- Die **DeployOnBuild** Eigenschaft weist MSBuild alle Anweisungen zur Bereitstellung in den projekteinstellungen ausgeführt werden, wenn der Build jedes Projekts abgeschlossen ist.
- Die **DeployTarget** identifiziert das Ziel die gewünschte aufzurufen, nachdem das Projekt erstellt wird. In diesem Fall die **Paket** Ziel erstellt die Projektausgabe in ein Webpaket bereitzustellenden.

> [!NOTE]
> Die **Paket** Ziel aufruft, das Web Publishing Pipeline (WPP), die Integration von MSBuild und Web Deploy bereitstellt. Wenn Sie einen Blick auf die integrierten Ziele, die die WPP bereitstellt, lesen Sie möchten die *Microsoft.Web.Publishing.targets* -Datei im Ordner "% PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web".


### <a name="the-gatherpackagesforpublishing-target"></a>Das Ziel GatherPackagesForPublishing

Wenn Sie untersuchen die **GatherPackagesForPublishing** Ziel, werden Sie feststellen, dass es tatsächlich alle Aufgaben, die nicht enthält. Stattdessen enthält sie eine einzelnes Element-Gruppe, die drei dynamische Elemente definiert.


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


Diese Elemente beziehen sich auf die Bereitstellungspakete aus, die erstellt wurden die **BuildProjects** Ziel ausgeführt wurde. Sie konnten nicht definieren, dieser Elemente statisch in der Projektdatei, da die Dateien, die auf dem die Elemente verweisen bis vorhanden sind, nicht die **BuildProjects** Ziel ausgeführt wird. Stattdessen die Elemente müssen definiert werden dynamisch in ein Ziel, das nicht, bis aufgerufen wird nach der **BuildProjects** Ziel ausgeführt wird.

Die Elemente werden nicht in diesem Ziel verwendet&#x2014;dieses Ziel einfach erstellt, die Elemente und die jeder Wert der Elements zugeordneten Metadaten. Sobald diese Elemente verarbeitet werden, die **PublishPackages** Element enthält zwei Werte und den Pfad zu der *ContactManager.Mvc.deploy.cmd* -Datei und den Pfad zu der  *ContactManager.Service.deploy.cmd* Datei. Web Deploy erstellt diese Dateien als Teil des Webpakets für jedes Projekt, und Hierbei handelt es sich um die Dateien, die Sie aufrufen, müssen auf dem Zielserver, um die Pakete bereitstellen. Wenn Sie eine dieser Dateien öffnen, sehen Sie einen Befehl "MSDeploy.exe" mit verschiedenen Parameterwerten für die Build-spezifische im Grunde genommen.

Die **DbPublishPackages** Element enthält einen einzelnen Wert, der den Pfad zu der *ContactManager.Database.deploymanifest* Datei.

> [!NOTE]
> Wenn Sie ein Datenbankprojekt erstellen und dabei wird das gleiche Schema als ein MSBuild-Projektdatei verwendet eine DEPLOYMANIFEST-Datei generiert. Sie enthält alle erforderlichen Informationen zum Bereitstellen einer Datenbank, einschließlich der Speicherort des Datenbankschemas (.dbschema) und Details von Skripts vor und nach der Bereitstellung. Weitere Informationen finden Sie unter [An Overview of Database Build und Bereitstellung](https://msdn.microsoft.com/library/aa833165.aspx).


Erfahren Sie mehr darüber, wie Pakete und Bereitstellungsmanifesten für die Datenbank erstellt und verwendet [erstellen und Verpacken von Webanwendungsprojekten](building-and-packaging-web-application-projects.md) und [Bereitstellen von Datenbankprojekten](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>Das Ziel PublishDbPackages

Kurz gesagt der **PublishDbPackages** Ziel aufruft, das VSDBCMD-Dienstprogramm zum Bereitstellen der **ContactManager** Datenbank in einer zielumgebung. Konfigurieren von datenbankbereitstellung umfasst viele Entscheidungen und Details in Bezug auf, und erfahren Sie mehr dazu im [Bereitstellen von Datenbankprojekten](deploying-database-projects.md) und [Anpassen von Datenbankbereitstellungen für mehrere Umgebungen](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). In diesem Thema konzentrieren wir uns auf, wie dieses Ziel tatsächlich funktioniert.

Erstens ist zu beachten, dass das öffnende Tag enthält eine **Ausgaben** Attribut.


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


Dies ist ein Beispiel der *Zielbatchverarbeitung*. In MSBuild-Projektdateien ist Batchverarbeitung eine Technik zum iterieren durch Sammlungen aus. Der Wert des der **Ausgaben** -Attribut, **"% (DbPublishPackages.Identity)"**, bezieht sich auf die **Identität** Metadata-Eigenschaft von der **DbPublishPackages**  Elementliste. Diese Notation, **Outputs=%***(ItemList.ItemMetadataName)*, wird als übersetzt:

- Teilen die Elemente in **DbPublishPackages** in Batches von Elementen, die denselben Container **Identität** Metadatenwert.
- Führen Sie das Ziel einmal pro Batch.

> [!NOTE]
> **Identität** ist eines der der [integrierte Metadatenwerte](https://msdn.microsoft.com/library/ms164313.aspx) , die jedem Element bei der Erstellung zugewiesen ist. Er verweist auf den Wert des der **Include** -Attribut in der **Element** Element&#x2014;in anderen Worten: der Pfad und Dateiname des Elements.


In diesem Fall, da es nie mehr als ein Element mit dem gleichen Pfad und Dateinamen vorhanden sein sollte, arbeiten es im Wesentlichen mit Batches mit einer Größe von einem. Das Ziel wird einmal für jedes Datenbankpaket ausgeführt.

Sehen Sie eine ähnliche Notation in der  **\_Cmd** -Eigenschaft, die einen VSDBCMD-Befehl mit den entsprechenden Schaltern erstellt.


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


In diesem Fall **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, und **%(DbPublishPackages.FullPath)** verweisen alle auf Metadatenwerte, der die **DbPublishPackages** Elementauflistung. Die  **\_Cmd** Eigenschaft wird verwendet, durch die **Exec** Aufgabe, das den Befehl aufruft.


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


Durch diese Notation die **Exec** aufgabenerstellung Batches, die basierend auf eindeutige Kombinationen aus der **DatabaseConnectionString**, **TargetDatabase**, und **FullPath** Metadatenwerte und die Aufgabe wird einmal für jeden Batch ausgeführt. Dies ist ein Beispiel der *Aufgabenbatchverarbeitung*. Jedoch, da die Zielebene Batchverarbeitung bereits unsere Elementauflistung in einzelnen Element Batches, unterteilt hat die **Exec** Task nur einmal für jede Iteration des Ziels ausgeführt. Das heißt, ruft diese Aufgabe das Dienstprogramm VSDBCMD einmal für jedes Datenbankpaket in der Projektmappe.

> [!NOTE]
> Weitere Informationen zu Ziel- und Batchverarbeitung von Aufgaben, finden Sie unter MSBuild [Batchverarbeitung](https://msdn.microsoft.com/library/ms171473.aspx), [Elementmetadaten bei der Batchverarbeitung von Zielen](https://msdn.microsoft.com/library/ms228229.aspx), und [Item Metadata in Task-Batchverarbeitung](https://msdn.microsoft.com/library/ms171474.aspx).


### <a name="the-publishwebpackages-target"></a>Das Ziel PublishWebPackages

Von diesem Punkt haben Sie aufgerufen der **BuildProjects** Ziel, die einem Webbereitstellungspaket für jedes Projekt in der beispiellösung generiert. Zugehörige jedes Paket ist eine *"Deploy.cmd"* -Datei, die die MSDeploy.exe-Befehle, die erforderlich sind, zur Bereitstellung des Pakets in der zielumgebung enthält, und ein *"SetParameters.xml"* -Datei der die erforderlichen Informationen von der zielumgebung. Sie haben ebenfalls aufgerufen, die **GatherPackagesForPublishing** Ziel, das eine Element-Auflistung mit generiert die *"Deploy.cmd"* Dateien, die Sie interessiert. Im Wesentlichen die **PublishWebPackages** Ziel führt diese Funktionen:

- Es bearbeitet die *"SetParameters.xml"* -Datei für jedes Paket in den richtigen Details für die zielumgebung mit der **XmlPoke** Aufgabe.
- Ruft die *"Deploy.cmd"* -Datei für jedes Paket, das mit den entsprechenden Schaltern.

Ebenso wie die **PublishDbPackages** Ziel der **PublishWebPackages** Zielbatchverarbeitung-Ziel verwendet, um sicherzustellen, dass das Ziel einmal für jede Webpaket ausgeführt wird.


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


Innerhalb des Ziels das **Exec** Aufgabe dient zum Ausführen der *"Deploy.cmd"* -Datei für jede Webpaket.


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Weitere Informationen zum Konfigurieren der Bereitstellung von Webpaketen finden Sie unter [erstellen und Verpacken von Webanwendungsprojekten](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema bereitgestellten eine exemplarische Vorgehensweise wie Split-Projektdateien zur Kontrolle des Build & Deployment-Prozesses von Anfang bis Ende für die Kontakt-Manager-beispiellösung verwendet werden. Mit diesem Ansatz kann Ausführen komplexer und Enterprise-Bereitstellungen in einem einzigen, wiederholbaren Schritt einfach, indem Sie eine bestimmte-Befehlsdatei ausführen.

## <a name="further-reading"></a>Weiterführende Themen

Eine ausführlichere Einführung in Projektdateien und die Apps, finden Sie unter [innerhalb der Microsoft Build Engine: Verwenden von MSBuild und Team Foundation Build](http://amzn.com/0735645248) von Sayed Ibrahim Hashimi und William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Zurück](understanding-the-project-file.md)
> [Weiter](building-and-packaging-web-application-projects.md)
