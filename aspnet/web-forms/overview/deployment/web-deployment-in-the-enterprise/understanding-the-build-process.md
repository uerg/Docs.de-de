---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
title: Grundlegendes zu den Buildprozess | Microsoft Docs
author: jrjlee
description: Dieses Thema enthält eine exemplarische Vorgehensweise eines Enterprise-Skalierung Build- und Bereitstellungsprozess-Prozesses. In diesem Thema beschriebene Ansatz verwendet benutzerdefinierte Microsoft Build Engin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 5b982451-547b-4a2f-a5dc-79bc64d84d40
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-build-process
msc.type: authoredcontent
ms.openlocfilehash: 4544a5e6212ea9b1247062dc35edc135ff7ca354
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30887999"
---
<a name="understanding-the-build-process"></a>Grundlegendes zu den Buildprozess
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema enthält eine exemplarische Vorgehensweise eines Enterprise-Skalierung Build- und Bereitstellungsprozess-Prozesses. In diesem Thema beschriebene Ansatz verwendet benutzerdefinierte Microsoft Build Engine (MSBuild)-Projektdateien, um eine präzisere Kontrolle über alle Aspekte des Prozesses zu ermöglichen. In den Projektdateien werden benutzerdefinierte MSBuild-Ziele verwendet, um die Bereitstellung von Hilfsprogrammen wie das Webbereitstellungstool (MSDeploy.exe) für das Internet Information Services (IIS) und des Datenbank-bereitstellungs-Hilfsprogramms VSDBCMD.exe ausgeführt werden.
> 
> > [!NOTE]
> > Im vorherigen Thema [verstehen die Projektdatei](understanding-the-project-file.md)beschrieben Schlüsselkomponenten von MSBuild-Projektdatei angegeben, und das Konzept der Split-Projektdateien zur Unterstützung der Bereitstellung für mehrere zielumgebungen eingeführt. Wenn Sie noch nicht mit diesen Konzepten vertraut sind, lesen Sie [verstehen die Projektdatei](understanding-the-project-file.md) bevor Sie in diesem Thema arbeiten.


Dieses Thema ist Teil einer Reihe von Lernprogrammen, die auf der Basis der Enterprise-bereitstellungsanforderungen eines fiktiven Unternehmens mit dem Namen Fabrikam, Inc. Dieses Lernprogramm Zeichenreihe verwendet eine beispiellösung&#x2014;der [Kontakt-Manager-Lösung](the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einer realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, einen Windows Communication Foundation (WCF)-Dienst, und ein Datenbankprojekt.

Die Bereitstellungsmethode das Herzstück mit diesen Lernprogrammen basiert auf in beschriebene Ansatz der Teilung Projekt Datei [verstehen die Projektdatei](understanding-the-project-file.md), in dem durch der Buildprozess gesteuert wird zwei Projektdateien&#x2014;enthält Erstellen Sie für jede zielumgebung und enthält umgebungsspezifische Einstellungen für Build- und Bereitstellungsprozess geltenden Anweisungen, an. Zur Buildzeit ist die Unabhängigkeit von der Umgebung-Projektdatei, einen vollständigen Satz von Buildanweisungen bilden die Projektdatei umgebungsspezifische zusammengeführt.

## <a name="build-and-deployment-overview"></a>Build und Bereitstellung (Übersicht)

In der [Kontakt-Manager-Lösung](the-contact-manager-solution.md), steuern erstellungs-und Bereitstellung von drei Dateien:

- Ein *universal Projektdatei* (*Publish.proj*). Dieses Objekt enthält Build- und Bereitstellungsprozess-Anweisungen, die zwischen zielumgebungen nicht ändern.
- Ein *umgebungsspezifische Projektdatei* (*Env Dev.proj*). Dieses Objekt enthält Build- und Bereitstellungsprozess Einstellungen, die für eine bestimmte zielumgebung spezifisch sind. Beispielsweise können Sie die *Env Dev.proj* Datei geben Sie die Einstellungen für einen Entwickler oder Test-Umgebung, und erstellen eine andere Datei mit dem Namen *Env Stage.proj* zum Bereitstellen von Einstellungen für eine Staging Umgebung.
- Ein *Befehlsdatei* (*veröffentlichen Dev.cmd*). Dieses Objekt enthält eine MSBuild.exe-Befehl, der angibt, welches Projekt Dateien ausgeführt werden soll. Sie können eine Befehlsdatei für jede zielumgebung erstellen, in dem jede Datei MSBuild.exe-Befehl enthält, der eine andere umgebungsspezifische Projektdatei angibt. Auf diese Weise können Entwickler auf verschiedenen Umgebungen bereitstellen, durch die entsprechenden Befehlsdatei ausführen.

In der Beispielprojektmappe können Sie diese drei Dateien im Ordner "Publish-Lösung" finden.

![](understanding-the-build-process/_static/image1.png)

Bevor Sie sich diese Dateien im Detail ansehen, werfen wir einen Blick auf die Funktionsweise des Gesamtprozess Build an, wenn Sie diesen Ansatz verwenden. Auf hoher Ebene sieht der Build- und Bereitstellungsprozess Prozess:

![](understanding-the-build-process/_static/image2.png)

Das erste Ereignis ist, dass die beiden Projektdateien&#x2014;, universelle Build- und Bereitstellungsprozess-Anweisungen enthält, und enthält umgebungsspezifische Einstellungen&#x2014;in einer Projektdatei zusammengeführt. MSBuild kann dann die Anweisungen in der Projektdatei. Jedes der Projekte in der Projektmappe, mit der Projektdatei für jedes Projekt erstellt. Er ruft dann andere Tools, wie das Webbereitstellungstool (MSDeploy.exe) und das Dienstprogramm VSDBCMD Ihre Webinhalte und die Datenbanken in der zielumgebung bereitgestellt.

Von Anfang bis Ende führt erstellungs-und Bereitstellung diese Aufgaben aus:

1. Löscht den Inhalt des Ausgabeverzeichnisses als Vorbereitung für eine neue Build.
2. Es wird jedes Projekt in der Projektmappe erstellt:

    1. Für Webprojekte&#x2014;web in diesem Fall eine ASP.NET MVC-Webanwendung und einen WCF-Dienst&#x2014;des Buildprozesses erstellt ein Webbereitstellungspaket für jedes Projekt.
    2. Für Datenbankprojekte wird während des Erstellungsprozesses ein Bereitstellungsmanifest (DEPLOYMANIFEST-Datei) für jedes Projekt erstellt.
3. Verwendet das Hilfsprogramm VSDBCMD.exe bereitzustellende jedes Datenbankprojekt in der Projektmappe, die über verschiedene Eigenschaften von Projektdateien&#x2014;eine Ziel-Verbindungszeichenfolge und einen Datenbanknamen&#x2014;zusammen mit der DEPLOYMANIFEST-Datei.
4. Das Hilfsprogramm MSDeploy.exe verwendet, um die Bereitstellung jedes Webprojekt in der Projektmappe, die verschiedene Eigenschaften von Projektdateien verwenden, um den Bereitstellungsprozess zu steuern.

Die beispiellösung können Sie diesen Vorgang im Detail verfolgen.

> [!NOTE]
> Anleitungen zum Anpassen der umgebungsspezifische-Projektdateien für eigene Server-Umgebungen finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="invoking-the-build-and-deployment-process"></a>Aufrufen der Build- und Bereitstellungsprozess

Um die Kontakt-Manager-Lösung in einer testumgebung für Entwickler bereitzustellen, führt der Entwickler die *veröffentlichen Dev.cmd* Befehlsdatei. Dies ruft MSBuild.exe angeben *Publish.proj* wie die Projektdatei ausgeführt und *Env Dev.proj* als Parameterwert.


[!code-console[Main](understanding-the-build-process/samples/sample1.cmd)]


> [!NOTE]
> Die **/fl** wechseln (Kurzform für **/FileLogger**) protokolliert die Buildausgabe in einer Datei namens *msbuild.log* im aktuellen Verzeichnis. Weitere Informationen finden Sie unter der [MSBuild-Befehlszeilenreferenz](https://msdn.microsoft.com/library/ms164311.aspx).


An diesem Punkt MSBuild ausgeführt wird, lädt die *Publish.proj* Datei- und beginnt die Verarbeitung der Anweisungen darin. Die erste Anweisung weist MSBuild das Projekt importieren-Datei mit der **TargetEnvPropsFile** gibt Parameter an.


[!code-xml[Main](understanding-the-build-process/samples/sample2.xml)]


Die **TargetEnvPropsFile** Parameter gibt die *Env Dev.proj* Datei, damit MSBuild führt den Inhalt der zusammen die *Env Dev.proj* -Datei in die  *Publish.proj* Datei.

Die weiter Elemente, die in der zusammengeführten Projektdatei MSBuild vorfindet sind Eigenschaftengruppen. Eigenschaften werden in der Reihenfolge verarbeitet, in der Datei angezeigt. MSBuild erstellt ein Schlüssel-Wert-Paar für jede Eigenschaft bereitstellen, dass alle angegebenen Bedingungen erfüllt sind. Eigenschaften, die später in der Datei definierten überschreibt alle Eigenschaften mit dem gleichen Namen, die weiter oben in der Datei definiert. Betrachten Sie beispielsweise die **OutputRoot** Eigenschaften.


[!code-xml[Main](understanding-the-build-process/samples/sample3.xml)]


Wenn MSBuild verarbeitet die erste **OutputRoot** Element bietet einen entsprechend benannten Parameter wurde nicht angegeben, wird der Wert von der **OutputRoot** -Eigenschaft **... \Publish\Out**. Wenn es trifft die zweite **OutputRoot** Element, wenn die Bedingung ergibt **"true"**, überschreibt es den Wert des der **OutputRoot** Eigenschaft mit dem Wert von der **"OutDir"** Parameter.

Das nächste Element, das erkennt MSBuild ist ein einzelnes Elementgruppe enthält ein Element mit dem Namen **ProjectsToBuild**.


[!code-xml[Main](understanding-the-build-process/samples/sample4.xml)]


MSBuild verarbeitet diese Anweisung erstellen Sie zunächst eine Liste der mit dem Namen **ProjectsToBuild**. In diesem Fall die Elementliste enthält einen einzelnen Wert&#x2014;den Pfad und Dateinamen der Projektmappendatei.

An diesem Punkt werden die übrigen Elemente Ziele. Ziele werden aus den Eigenschaften und Elemente unterschiedlich verarbeitet&#x2014;im wesentlichen Ziele werden nicht verarbeitet, es sei denn, sie werden explizit vom Benutzer angegebenen oder, indem ein anderes Konstrukt in der Projektdatei aufgerufen. Bedenken Sie, dass das öffnende **Projekt** Tag enthält eine **DefaultTargets** Attribut.


[!code-xml[Main](understanding-the-build-process/samples/sample5.xml)]


Dies weist MSBuild zum Aufrufen der **FullPublish** Ziel, falls sind nicht angegeben, wann MSBuild.exe aufgerufen wird. Die **FullPublish** Ziel keine Aufgaben enthalten; stattdessen gibt es einfach eine Liste der Abhängigkeiten.


[!code-xml[Main](understanding-the-build-process/samples/sample6.xml)]


Diese Abhängigkeit weist MSBuild zum Ausführen dieser in Reihenfolge der **FullPublish** Ziel, muss es diese Liste der Ziele in der angegebenen Reihenfolge aufgerufen:

1. Es muss Aufrufen der **Bereinigen** Ziel.
2. Es muss Aufrufen der **BuildProjects** Ziel.
3. Es muss Aufrufen der **GatherPackagesForPublishing** Ziel.
4. Es muss Aufrufen der **PublishDbPackages** Ziel.
5. Es muss Aufrufen der **PublishWebPackages** Ziel.

### <a name="the-clean-target"></a>Das Clean-Ziel

Die **Bereinigen** Ziel löscht im Grunde das Ausgabeverzeichnis und der gesamte Inhalt, als Vorbereitung für einen neuen Build.


[!code-xml[Main](understanding-the-build-process/samples/sample7.xml)]


Beachten Sie, die das Ziel enthält eine **ItemGroup** Element. Beim Definieren von Eigenschaften oder Elemente in einem **Ziel** Element, erstellen Sie *dynamische* Eigenschaften und Elemente. Das heißt, werden nicht die Eigenschaften oder Elemente verarbeitet, bis das Ziel ausgeführt wird. Das Ausgabeverzeichnis möglicherweise nicht vorhanden oder bis der Buildprozess eingeleitet wurde, alle Dateien enthalten, so erstellt werden kann die  **\_FilesToDelete** als ein statisches Element aufgeführt; Sie müssen warten, bis die Ausführung ausgeführt wird. Daher erstellen Sie die Liste als dynamische Element innerhalb des Ziels.

> [!NOTE]
> In diesem Fall, da die **Bereinigen** Ziel ist die erste ausgeführt werden, besteht keine Notwendigkeit, eine dynamische Gruppe verwenden. Es ist jedoch empfohlen, dynamische Eigenschaften und Elemente in diesem Szenario verwenden, wie Ziele in einer anderen Reihenfolge späteren Zeitpunkt ausgeführt werden sollen.  
> Sie sollten auch darauf ausgerichtet, zum Vermeiden der Deklaration von Elementen, die nie verwendet wird. Wenn Sie Elemente, die durch ein bestimmtes Ziel nur verwendet werden vorliegen, sollten Sie sie in das Ziel entfernen unnötigen Aufwand zum Erstellungsprozess platziert werden.


Dynamische Elemente reservieren, die **Bereinigen** Ziel ist recht einfach und nutzt die integrierte **Nachricht**, **löschen**, und **RemoveDir**von Aufgaben an:

1. Senden einer Nachricht an die Protokollierung.
2. Erstellen Sie eine Liste der zu löschenden Dateien an.
3. Löschen Sie die Dateien an.
4. Entfernen Sie das Ausgabeverzeichnis an.

### <a name="the-buildprojects-target"></a>Das Ziel BuildProjects

Die **BuildProjects** Ziel im Grunde genommen alle Projekte in der Beispielprojektmappe erstellt.


[!code-xml[Main](understanding-the-build-process/samples/sample8.xml)]


Dieses Ziel wurde im vorherigen Thema ausführlich beschrieben [verstehen die Projektdatei](understanding-the-project-file.md), um zu veranschaulichen, wie Aufgaben und Ziele Eigenschaften und Elemente verweisen. An diesem Punkt sind vor allem interessiert die **MSBuild** Aufgabe. Sie können diese Aufgabe verwenden, um mehrere Projekte zu erstellen. Der Task erstellt eine neue Instanz von MSBuild.exe nicht; die derzeit ausgeführte Instanz verwendet, um jedes Projekt zu erstellen. Das wichtige Punkte in diesem Beispiel werden die Bereitstellungseigenschaften:

- Die **DeployOnBuild** Eigenschaft weist MSBuild alle Anweisungen zur Bereitstellung in den projekteinstellungen ausgeführt, wenn der Build für jedes Projekt abgeschlossen ist.
- Die **DeployTarget** Eigenschaft identifiziert das Ziel, das Sie nach der Erstellung des Projekts aufrufen möchten. In diesem Fall die **Paket** Ziel erstellt die Projektausgabe in ein Webpaket zur Bereitstellung geeignete.

> [!NOTE]
> Die **Paket** -Ziel Ruft die Web veröffentlichen Pipeline (WPP), die Integration von MSBuild und Web Deploy bietet. Wenn Sie einen Blick auf die integrierte Ziele, die WPP bereitstellt, lesen Sie möchten die *Microsoft.Web.Publishing.targets* Datei im Ordner "% PROGRAMFILES (x 86) %\MSBuild\Microsoft\VisualStudio\v10.0\Web".


### <a name="the-gatherpackagesforpublishing-target"></a>Das Ziel GatherPackagesForPublishing

Wenn Sie untersuchen die **GatherPackagesForPublishing** Ziel, werden Sie feststellen, dass es tatsächlich alle Aufgaben enthält. Stattdessen enthält es eine einzelnes Element-Gruppe, die drei dynamischen Elemente definiert.


[!code-xml[Main](understanding-the-build-process/samples/sample9.xml)]


Diese Elemente beziehen sich auf die Bereitstellungspakete aus, die erstellt wurden die **BuildProjects** Ziel ausgeführt wurde. Sie konnten nicht definieren diese Elemente statisch in der Projektdatei, da die Dateien, die auf dem die Elemente verweisen bis vorhanden sind, nicht die **BuildProjects** Ziel ausgeführt wird. Stattdessen die Elemente müssen definiert werden dynamisch in ein Ziel, das nicht, erst aufgerufen wird nach der **BuildProjects** Ziel ausgeführt wird.

Die Elemente werden nicht in diesem Ziel verwendet&#x2014;dieses Ziel einfach erstellt werden, die Elemente und die Metadaten jeder Elementwert zugeordnet. Sobald diese Elemente verarbeitet werden, die **PublishPackages** Element enthält zwei Werte und den Pfad zu der *ContactManager.Mvc.deploy.cmd* Datei und den Pfad zu der  *ContactManager.Service.deploy.cmd* Datei. Web Deploy erstellt diese Dateien als Teil der Web-Paket für jedes Projekt, und dies sind die Dateien, die Sie aufrufen müssen auf dem Zielserver, um die Pakete bereitstellen. Wenn Sie eine dieser Dateien zu öffnen, sehen Sie im Grunde einen Befehl "MSDeploy.exe" mit verschiedenen Parameterwerten für die Build-spezifische.

Die **DbPublishPackages** Element enthält einen einzelnen Wert, der den Pfad zu der *ContactManager.Database.deploymanifest* Datei.

> [!NOTE]
> Eine DEPLOYMANIFEST-Datei wird generiert, wenn Sie ein Datenbankprojekt erstellen und das gleiche Schema wie eine MSBuild-Projektdatei verwendet. Sie enthält alle Informationen, die zum Bereitstellen einer Datenbank, einschließlich der Speicherort des Datenbankschemas (.dbschema) und Details zum beliebiger anderer Skripts vor und nach der Bereitstellung erforderlich sind. Weitere Informationen finden Sie unter [eine Übersicht über die der Datenbank zu erstellen und die Bereitstellung](https://msdn.microsoft.com/library/aa833165.aspx).


Erfahren Sie mehr darüber, wie Pakete und die Bereitstellungsmanifeste für die Datenbank erstellt und verwendet [erstellen und Packen Webanwendungsprojekte](building-and-packaging-web-application-projects.md) und [Datenbankprojekte bereitstellen](deploying-database-projects.md).

### <a name="the-publishdbpackages-target"></a>Das Ziel PublishDbPackages

Kurz gesagt der **PublishDbPackages** -Ziel Ruft die VSDBCMD-Dienstprogramm zum Bereitstellen der **ContactManager** Datenbank in einer zielumgebung. Das Konfigurieren von datenbankbereitstellung umfasst viele Entscheidungen und feinen Unterschieden bei vertraut, und erfahren Sie mehr über den in [Datenbankprojekte bereitstellen](deploying-database-projects.md) und [Anpassen von Datenbank-Bereitstellungen für mehrere Umgebungen](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). In diesem Thema konzentrieren wir uns auf dieses Ziel wie tatsächlich funktioniert.

Erstens ist zu beachten, dass das öffnende Tag enthält eine **Ausgaben** Attribut.


[!code-xml[Main](understanding-the-build-process/samples/sample10.xml)]


Dies ist ein Beispiel der *Batchverarbeitung von Zielen*. In MSBuild-Projektdateien ist Batchverarbeitung eine Technik zum Durchlaufen der Auflistungen aus. Der Wert des der **Ausgaben** -Attribut, **"% (DbPublishPackages.Identity)"**, bezieht sich auf die **Identität** Metadata-Eigenschaft von der **DbPublishPackages**  Elementliste. Diese Notation **Outputs=%***(ItemList.ItemMetadataName)*, wird als übersetzt:

- Teilen die Elemente in **DbPublishPackages** in Batches von Elementen, die die gleiche **Identität** Metadatenwert.
- Führen Sie das Ziel einmal pro Batch.

> [!NOTE]
> **Identität** ist eines der [integrierte Metadatenwerte](https://msdn.microsoft.com/library/ms164313.aspx) , die jedes Element bei der Erstellung zugewiesen. Er verweist auf den Wert von der **Include** Attribut in der **Element** Element&#x2014;also den Pfad und den Dateinamen des Elements.


In diesem Fall, da es nie mehr als ein Element mit dem gleichen Pfad und Dateiname vorhanden sein sollte, arbeiten es im Wesentlichen mit Batchgrößen eines. Das Ziel wird einmal für jedes Datenbankpaket ausgeführt.

Sehen Sie eine ähnliche Notation in der  **\_Cmd** -Eigenschaft, die einen VSDBCMD-Befehl mit der entsprechenden Switches erstellt.


[!code-xml[Main](understanding-the-build-process/samples/sample11.xml)]


In diesem Fall **%(DbPublishPackages.DatabaseConnectionString)**, **%(DbPublishPackages.TargetDatabase)**, und **%(DbPublishPackages.FullPath)** alle finden Sie unter Metadatenwerte, der die **DbPublishPackages** -Elementauflistung. Die  **\_Cmd** Eigenschaft wird verwendet, durch die **Exec** Task, der den Befehl aufruft.


[!code-xml[Main](understanding-the-build-process/samples/sample12.xml)]


Als Ergebnis dieser Notation der **Exec** aufgabenerstellung basierend auf eindeutige Kombinationen von Batches den **DatabaseConnectionString**, **TargetDatabase**, und **FullPath** Metadatenwerte und die Aufgabe wird einmal für jeden Batch ausgeführt. Dies ist ein Beispiel der *Aufgabenbatchverarbeitung*. Jedoch, da die Zielebene Batchverarbeitung bereits unsere Item-Auflistung in einzelne-Item-Batches unterteilt wurde die **Exec** Task nur einmal für jede Iteration des Ziels ausgeführt. Das heißt, wird diese Aufgabe des Hilfsprogramms VSDBCMD einmal für jede Datenbankpaket in der Projektmappe aufgerufen.

> [!NOTE]
> Weitere Informationen zu Ziel- und Batchverarbeitung von Aufgaben, finden Sie unter MSBuild [Batchverarbeitung](https://msdn.microsoft.com/library/ms171473.aspx), [Elementmetadaten bei der Batchverarbeitung von Zielen](https://msdn.microsoft.com/library/ms228229.aspx), und [Elementmetadaten bei der Batchverarbeitung von Aufgaben](https://msdn.microsoft.com/library/ms171474.aspx).


### <a name="the-publishwebpackages-target"></a>Das Ziel PublishWebPackages

Zu diesem Zeitpunkt aufgerufen haben die **BuildProjects** Ziel, die in der Beispielprojektmappe ein Webbereitstellungspaket für jedes Projekt generiert. Begleitende jedes Paket ist eine *deploy.cmd* Datei, die die MSDeploy.exe Befehle zum Bereitstellen des Pakets in der zielumgebung enthält, und ein *SetParameters.xml* Datei, der angibt, die Details zu der zielumgebung erforderlich. Sie haben auch aufgerufen, die **GatherPackagesForPublishing** Ziels, das eine Element-Auflistung mit generiert die *deploy.cmd* Dateien, die Sie interessiert sind. Im Wesentlichen die **PublishWebPackages** Ziel führt diese Funktionen:

- Es bearbeitet die *SetParameters.xml* -Datei für jedes Paket in den richtigen Details für die zielumgebung mithilfe der **XmlPoke** Aufgabe.
- Ruft die *deploy.cmd* Datei für jedes Paket, mit der entsprechenden Switches.

Ebenso wie die **PublishDbPackages** Ziel, die **PublishWebPackages** -Ziel verwendet Batchverarbeitung von Zielen, um sicherzustellen, dass das Ziel einmal für jedes Webpaket ausgeführt wird.


[!code-xml[Main](understanding-the-build-process/samples/sample13.xml)]


Innerhalb des Ziels der **Exec** Task dient zum Ausführen der *deploy.cmd* -Datei für jede Web-Paket.


[!code-xml[Main](understanding-the-build-process/samples/sample14.xml)]


Weitere Informationen zum Konfigurieren der Bereitstellung von Webpaketen finden Sie unter [erstellen und Packen Webanwendungsprojekte](building-and-packaging-web-application-projects.md).

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema bereitgestellten eine exemplarische Vorgehensweise wie Split-Projektdateien zum Steuern des Build- und Bereitstellungsprozess Prozess von Anfang bis Ende für die Kontakt-Manager-beispiellösung verwendet werden. Bei diesem Ansatz kann Ausführen komplexer, Enterprise-Bereitstellungen in einem einzelnen, wiederholbare Schritt einfach, indem Sie eine Befehlsdatei umgebungsspezifische ausführen.

## <a name="further-reading"></a>Weiterführende Themen

Eine ausführliche Einführung in die Projektdateien und die WPP finden Sie unter [innerhalb der Microsoft Build Engine: Verwenden von MSBuild und Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi und William Bartholomew, ISBN-Nummer: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Zurück](understanding-the-project-file.md)
> [Weiter](building-and-packaging-web-application-projects.md)
