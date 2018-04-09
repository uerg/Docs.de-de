---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Grundlegendes zur Projektdatei | Microsoft Docs
author: jrjlee
description: Projektdateien Microsoft Build Engine (MSBuild) bilden den Kern des Prozesses Build- und Bereitstellungsprozess. Dieses Thema beginnt mit eine konzeptionelle Übersicht über MSBuild...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 49d1d4fbe48cd4f073e774d8a9c6c0c011bd3319
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="understanding-the-project-file"></a>Grundlegendes zu der Projektdatei
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Projektdateien Microsoft Build Engine (MSBuild) bilden den Kern des Prozesses Build- und Bereitstellungsprozess. Dieses Thema beginnt mit eine konzeptionelle Übersicht über MSBuild und die Projektdatei. Es beschreibt die wichtigsten Komponenten, die Sie stoßen werden, wenn Sie die Projektdateien arbeiten und funktioniert über ein Beispiel der Nutzung von Projektdateien echten Anwendungen bereitgestellt werden können.
> 
> Lernen Sie:
> 
> - MSBuild verwendet wie MSBuild-Projektdateien, um Projekte zu erstellen.
> - Bereitstellungstechnologien, z. B. die Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy) wie MSBuild integriert.
> - Kennenlernen der Schlüsselkomponenten einer Projektdatei.
> - Wie können Sie Projektdateien erstellen und komplexe Anwendungen bereitstellen.


## <a name="msbuild-and-the-project-file"></a>MSBuild und der Projektdatei

Beim Erstellen und Projektmappen in Visual Studio erstellen, verwendet Visual Studio MSBuild jedes Projekt in der Projektmappe zu erstellen. Alle Visual Studio-Projekt enthält eine MSBuild-Projektdatei mit der Erweiterung, die den Typ des Projekts wiedergibt&#x2014;z. B. ein C#-Projekt (csproj), ein Projekt Visual Basic.NET (.vbproj) oder ein Datenbankprojekt (.dbproj). Um ein Projekt erstellen, muss MSBuild die Projektdatei, die dem Projekt zugeordneten verarbeiten. Die Projektdatei ist eine XML-Dokument mit den Informationen und Anweisungen, MSBuild, um das Erstellen des Projekts, z. B. den Inhalt einschließen, die plattformanforderungen, die Versionsinformationen, die Webserver oder die datenbankeinstellungen für Server muss, und die Aufgaben, die ausgeführt werden müssen.

MSBuild-Projektdateien basieren auf der [MSBuild XML-Schema](https://msdn.microsoft.com/library/5dy88c2e.aspx), und daher während des Erstellungsprozesses vollständig geöffnet und transparent. Darüber hinaus Sie Visual Studio zu installieren, um das MSBuild-Modul verwenden, müssen nicht&#x2014;MSBuild.exe ausführbaren Datei ist Teil von .NET Framework und Sie können über eine Eingabeaufforderung ausführen. Als Entwickler können Sie erstellen Ihre eigenen MSBuild-Projektdateien mit der MSBuild-XML-Schema erzwingen hochentwickelte und eine präzisere Kontrolle über die wie Ihre Projekte erstellt und bereitgestellt werden. Diese benutzerdefinierte Projektdateien arbeiten auf genau die gleiche Weise wie die Projektdateien, die Visual Studio automatisch generiert.

> [!NOTE]
> Sie können auch MSBuild-Projektdateien mit dem Team Build-Dienst in der Team Foundation Server (TFS) verwenden. Beispielsweise können Sie die Projektdateien in fortlaufende Integration (CI)-Szenarien zur automatischen Bereitstellung in einer testumgebung aus, bei der neue Code eingecheckt wird. Weitere Informationen finden Sie unter [Konfigurieren von Team Foundation Server für die automatisierte Bereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).


### <a name="project-file-naming-conventions"></a>Benennungskonventionen für Projektdatei

Wenn Sie eine eigene Projektdateien erstellen, können Sie eine Dateierweiterung, die Ihnen gefallen. Um Ihre Lösungen für andere verständlich vereinfachen, sollten Sie jedoch diese allgemeinen Konventionen verwenden:

- Verwenden Sie die Erweiterung ".proj", wenn Sie eine Projektdatei erstellen, die Projekte erstellt.
- Verwenden Sie die targets-Erweiterung, bei der Erstellung einer wiederverwendbaren Projektdatei in andere Projektdateien importiert. Dateien mit einer TARGETS-Erweiterung in der Regel nicht "erstellen" alles selbst, sie einfach enthalten Anweisungen, die Sie in Ihrem ".proj"-Dateien importieren können.

### <a name="integration-with-deployment-technologies"></a>Integration mit Bereitstellungstechnologien

Wenn Sie mit Webanwendungsprojekte in Visual Studio 2010, wie PHP-Webanwendungen und ASP.NET MVC-Webanwendungen, gearbeitet haben, wissen Sie, dass diese Projekte integrierten Unterstützung für das Verpacken und Bereitstellen der Webanwendung in einer zielumgebung enthalten. Die **Eigenschaften** Seiten für diese Projekte schließen **Web packen/veröffentlichen** und **SQL packen/veröffentlichen** Registerkarten, die Sie verwenden können, so konfigurieren Sie wie die Bestandteile Ihrer Anwendung werden verpackt und bereitgestellt. Dies zeigt die **Web packen/veröffentlichen** Registerkarte:

![](understanding-the-project-file/_static/image1.png)

Die zugrundeliegende Technologie hinter diesen Funktionen wird als Webdienst veröffentlichen Pipeline (WPP) bezeichnet. Die WPP bringt im Wesentlichen MSBuild und [Web Deploy](https://go.microsoft.com/?linkid=9805122) zusammen, um einen vollständigen Build, Paket- und Bereitstellung-Prozess für Ihre Webanwendungen bereitzustellen.

Die gute Nachricht ist, dass Sie die Integrationspunkte, die die WPP bereitstellt nutzen können, wenn Sie benutzerdefinierte Projektdateien für Webprojekte erstellen. Sie können Anweisungen zur Bereitstellung in der Projektdatei, können Sie Ihre Projekte erstellen, Web-Bereitstellungspakete erstellen und installieren diese Pakete auf Remoteservern über ein einzelnes Projekt Datei- und eines einzelnen Aufrufs von MSBuild enthalten. Sie können auch andere ausführbare Dateien als Teil des Buildprozesses aufrufen. Beispielsweise können Sie das VSDBCMD.exe-Befehlszeilentool zum Bereitstellen einer Datenbank aus einer Schemadatei ausführen. Im Verlauf dieses Themas erfahren Sie, wie Sie diese Funktionen zu den Anforderungen Ihrer Bereitstellungsszenarios nutzen können.

> [!NOTE]
> Weitere Informationen zur Funktionsweise der Bereitstellungsprozess der Web-Anwendung finden Sie unter [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698.aspx).


## <a name="the-anatomy-of-a-project-file"></a>Die Struktur einer Projektdatei

Bevor Sie die Build-Prozess im Detail betrachten, ist es lohnt einen Augenblick, mit die grundlegende Struktur einer MSBuild-Projektdatei vertraut. Dieser Abschnitt enthält eine Übersicht über die häufigeren Elemente, die Sie treffen, wenn Sie überprüfen, bearbeiten oder erstellen eine Projektdatei. Insbesondere erfahren Sie:

- Gewusst wie: Verwenden Sie *Eigenschaften* zum Verwalten von Variablen für den Buildprozess.
- Gewusst wie: Verwenden Sie *Elemente* um die Eingaben für den Buildprozess zu identifizieren, wie Codedateien.
- Gewusst wie: verwenden *Ziele* und *Aufgaben* zum Bereitstellen von Anweisungen zur Ausführung für MSBuild mit *Eigenschaften* und *Elemente* an anderer Stelle definiert die Projektdatei.

Dies zeigt die Beziehung zwischen der Schlüsselelemente im MSBuild-Projektdatei angegeben:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>Project-Element

Die [Projekt](https://msdn.microsoft.com/library/bcxfsh87.aspx) Element ist das Stammelement in jeder Projektdatei. Zusätzlich zum Identifizieren der XML-Schema für die Projektdatei der **Projekt** Element kann Attribute, um anzugeben, die Einstiegspunkte für den Buildprozess einschließen. Z. B. in der [Kontakt-Manager-beispiellösung](the-contact-manager-solution.md), die *Publish.proj* Datei gibt an, dass der Build gestartet werden soll, durch das Ziel mit dem Namen aufrufen **FullPublish**.


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>Eigenschaften und Bedingungen

Eine Projektdatei muss in der Regel enthalten zahlreiche unterschiedliche Arten von Informationen, um erfolgreich zu erstellen und Bereitstellen der Projekte. Servernamen, Verbindungszeichenfolgen, Anmeldeinformationen, Buildkonfigurationen, Quelle und Ziel-Dateipfade und andere Informationen, die Sie enthalten, das zur Unterstützung zum anpassen möchten, können diese Angaben enthalten. In einer Projektdatei Eigenschaften definiert werden müssen, in einem [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) Element. MSBuild-Eigenschaften werden von Schlüssel-Wert-Paaren bestehen. Innerhalb der **PropertyGroup** Element den Elementnamen definiert, den Schlüssel und den Inhalt des Elements definiert, den Wert der Eigenschaft. Sie können z. B. Eigenschaften, die mit dem Namen definieren **ServerName** und **"ConnectionString"** , eine statische Server Name und Verbindung der Zeichenfolge zu speichern.


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


Um einen Eigenschaftswert abzurufen, verwenden Sie das Format <strong>$(</strong><em>PropertyName</em><strong>)</strong><em>.</em> Beispielsweise, um das Abrufen des Werts der <strong>ServerName</strong> -Eigenschaft geben Sie:


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> Sie sehen, Beispiele, wie und wann die Eigenschaftswerte weiter unten in diesem Thema verwenden.


Einbetten von Informationen als statische Eigenschaften in einer Projektdatei ist nicht immer der optimale Ansatz zur Verwaltung des Buildprozesses. Möchten Sie in eine Vielzahl von Szenarien die Informationen aus anderen Quellen abrufen, oder den Benutzer die Anmeldeinformationen von der Befehlszeile aus zu ermöglichen. MSBuild können Sie einen beliebigen Eigenschaftswert als Befehlszeilenparameter angeben. Der Benutzer kann z. B. einen Wert für bereitstellen **ServerName** wann er ausgeführt wird MSBuild.exe über die Befehlszeile.


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> Weitere Informationen zu den Argumenten und -Switches Sie mit MSBuild.exe können, finden Sie unter [MSBuild-Befehlszeilenreferenz](https://msdn.microsoft.com/library/ms164311.aspx).


Die gleiche Eigenschaftensyntax können Sie um die Werte der Umgebungsvariablen und integrierte Projekteigenschaften zu erhalten. Viele häufig verwendete Eigenschaften, die für Sie definiert sind können, und Sie sie in den Projektdateien durch den Namen des entsprechenden Parameters einschließen. Z. B. zum Abrufen der aktuellen Projektplattform&#x2014;z. B. **X86** oder **"anycpu"**&#x2014;Sie zählen die **$(Platform)** eigenschaftsreferenz im der Projektdatei. Weitere Informationen finden Sie unter [Makros für Buildbefehle und-Eigenschaften](https://msdn.microsoft.com/library/c02as0cs.aspx), [gemeinsame MSBuild-Projekteigenschaften](https://msdn.microsoft.com/library/bb629394.aspx), und [reservierte Eigenschaften](https://msdn.microsoft.com/library/ms164309.aspx).

Eigenschaften werden häufig in Verbindung mit verwendet *Bedingungen*. Die meisten MSBuild-Elemente unterstützen die **Bedingung** -Attribut, das können Sie die Kriterien angeben, nach dem Auswerten MSBuild das Element. Betrachten Sie beispielsweise diese Eigenschaftsdefinition:


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


Wenn MSBuild diese Eigenschaftsdefinition verarbeitet, es zunächst geprüft, ob ein **$(OutputRoot)** Eigenschaftswert steht. Wenn der Wert der Eigenschaft leer ist&#x2014;in anderen Worten: der Benutzer einen Wert für diese Eigenschaft angegeben wurde nicht&#x2014;ergibt die Bedingung **"true"** und den Wert der Eigenschaft auf festgelegt ist **... \Publish\Out**. Wenn der Benutzer einen Wert für diese Eigenschaft bereitgestellt hat, die Bedingung ausgewertet wird, um **"false"** und der statischen Eigenschaft-Wert wird nicht verwendet.

Weitere Informationen über die verschiedenen Möglichkeiten, in dem Sie Bedingungen angeben können, finden Sie unter [MSBuild-Bedingungen](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Elemente und Elementgruppen

Einer der Rollen der Projektdatei wichtigen ist die Eingaben für den Buildprozess zu definieren. In der Regel werden diese Eingaben Dateien&#x2014;Codedateien, Konfigurationsdateien, Befehlsdateien und alle anderen Dateien, die Sie benötigen, verarbeiten oder kopieren Sie als Teil des Buildprozesses. In der MSBuild-Projekt-Schema werden diese Eingaben durch dargestellt [Element](https://msdn.microsoft.com/library/ms164283.aspx) Elemente. In einer Projektdatei Elemente definiert werden müssen, innerhalb einer [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) Element. Ebenso wie **Eigenschaft** Elemente, Sie können den Namen einer **Element** Element jedoch beliebig. Sie müssen jedoch angeben, ein **Include** Attribut identifiziert die Datei oder der Platzhalter, der das Element darstellt.


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


Durch Angeben von mehreren **Element** Elemente mit dem gleichen Namen, erstellen Sie effektiv eine benannte Liste von Ressourcen. Eine gute Möglichkeit, dies in Aktion zu sehen ist, in einem der Projektdateien ansehen, die Visual Studio erstellt. Z. B. die *ContactManager.Mvc.csproj* Datei in der Beispielprojektmappe umfasst zahlreiche Elementgruppen, die jeweils mit mehreren identisch mit dem Namen **Element** Elemente.


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


Auf diese Weise wird die Projektdatei festzuhalten und MSBuild zum Erstellen von Listen von Dateien, die auf die gleiche Weise verarbeitet werden müssen&#x2014;der **Verweis** Liste enthält die Assemblys, die für eine erfolgreiche Erstellung vorhanden sein müssen die  **Kompilieren Sie** zählen Codedateien, die kompiliert werden müssen, und die **Content** Liste enthält Ressourcen, die kopiert werden müssen, die unverändert. Betrachten wir wie während des Erstellungsprozesses verweist, und verwendet diese Elemente weiter unten in diesem Thema.

ReportItem-Elemente können auch einschließen, [ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx) untergeordnete Elemente. Diese sind benutzerdefinierte Schlüssel-Wert-Paare und Eigenschaften, die speziell für dieses Element sind im Wesentlichen darstellen. Angenommen, ein Großteil der **Kompilieren** ReportItem-Elemente in der Projektdatei enthalten **DependentUpon** untergeordnete Elemente.


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> Zusätzlich zu benutzerdefinierten Elementmetadaten werden alle Elemente auf verschiedene allgemeine Metadaten bei der Erstellung zugewiesen. Weitere Informationen finden Sie unter [Well-known Item Metadata (Bekannte Elementmetadaten)](https://msdn.microsoft.com/library/ms164313.aspx).


Sie können erstellen **ItemGroup** Elemente innerhalb der Stammebene **Projekt** Element oder in bestimmten **Ziel** Elemente. **ItemGroup** Elemente unterstützen auch **Bedingung** Attribute, die Sie die Eingaben für den Buildprozess entsprechend Bedingungen wie z. B. die Projektkonfiguration oder Plattform anpassen können.

### <a name="targets-and-tasks"></a>Ziele und Aufgaben

In der MSBuild-Schema eine [Aufgabe](https://msdn.microsoft.com/library/77f2hx1s.aspx) -Element stellt eine einzelne Build-Anweisung (oder eine Aufgabe) dar. MSBuild ist hierin enthalten zahlreiche vordefinierte Tasks. Zum Beispiel:

- Die **Kopie** Task kopiert Dateien an einem neuen Speicherort.
- Die **Csc** Aufgabe wird den Visual C#-Compiler aufgerufen.
- Die **Vbc** Aufgabe wird den Visual Basic-Compiler aufgerufen.
- Die **Exec** Task ausgeführt wird, ein angegebenes Programm.
- Die **Nachricht** Task schreibt eine Meldung, die eine Protokollierung.

> [!NOTE]
> Vollständige Informationen zu Aufgaben, die unmittelbar verfügbar sind, finden Sie unter [Referenz zu MSBuild-Aufgaben](https://msdn.microsoft.com/library/7z253716.aspx). Weitere Informationen zu Aufgaben, wie Sie eigene benutzerdefinierte Tasks erstellen finden Sie unter [MSBuild-Aufgaben](https://msdn.microsoft.com/library/ms171466.aspx).


Aufgaben müssen immer enthaltenen [Ziel](https://msdn.microsoft.com/library/t50z2hka.aspx) Elemente. Ein **Ziel** Element ist ein Satz von einer oder mehreren Aufgaben, die nacheinander ausgeführt werden, und eine Projektdatei kann mehrere Ziele enthalten. Wenn Sie eine Aufgabe oder eine Reihe von Aufgaben ausführen möchten, rufen Sie das Ziel, das sie enthält. Nehmen wir beispielsweise an, dass Sie eine einfachen Projektdatei verfügen, die eine Meldung protokolliert.


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


Sie können das Ziel in der Befehlszeile aufrufen, indem Sie mit der **/t** wechseln, um das Ziel anzugeben.


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


Sie können auch Hinzufügen einer **DefaultTargets** -Attribut auf die **Projekt** Element, um die Ziele anzugeben, die Sie aufrufen möchten.


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


In diesem Fall müssen Sie das Ziel in der Befehlszeile angeben. Sie können einfach die Projektdatei angeben und MSBuild wird aufgerufen, die **FullPublish** Ziel für Sie.


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


Ziele und Aufgaben zählen **Bedingung** Attribute. Sie können daher ganze Ziele oder einzelne Aufgaben zu unterdrücken, wenn bestimmte Bedingungen erfüllt sind.

Wenn Sie nützliche Aufgaben und Ziele erstellen, müssen Sie im Allgemeinen beziehen sich auf die Eigenschaften und Elemente, die Sie an anderer Stelle in der Projektdatei definiert haben:

- Geben Sie für die Verwendung ein Eigenschaftswerts <strong>$(</strong><em>PropertyName</em><strong>)</strong>, wobei <em>PropertyName</em> ist der Name des der <strong>-Eigenschaft</strong> Element oder den Namen des Parameters.
- Um ein Element zu verwenden, geben <strong>@(</strong><em>ItemName</em><strong>)</strong>, wobei <em>ItemName</em> ist der Name des der <strong>Element</strong> Element.

> [!NOTE]
> Denken Sie daran, dass wenn Sie mehrere Elemente mit dem gleichen Namen erstellen, Sie eine Liste erstellen. Im Gegensatz dazu, wenn Sie mehrere Eigenschaften mit demselben Namen zu erstellen, Sie geben, der letzte Eigenschaftenwert überschreibt alle vorherigen Eigenschaften mit demselben Namen&#x2014;eine Eigenschaft kann nur einen einzelnen Wert enthalten.


Beispielsweise ist in der *Publish.proj* Datei in der Beispielprojektmappe, sehen Sie sich die **BuildProjects** Ziel.


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


In diesem Beispiel können Sie diese wichtige Punkte beachten:

- Wenn die **BuildingInTeamBuild** Parameter angegeben ist, und hat den Wert **"true"**, keinen der Tasks in diesem Ziel wird ausgeführt.
- Das Ziel enthält eine einzelne Instanz der [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) Aufgabe. Mit diesem Task können Sie die anderen MSBuild-Projekte erstellen.
- Die **ProjectsToBuild** Element an die Aufgabe übergeben wird. Dieses Element kann eine Liste von Projektmappen- oder Projektdateien, indem alle definierten dargestellt **ProjectsToBuild** Element der Elemente innerhalb einer Elementgruppe. In diesem Fall die **ProjectsToBuild** Element bezieht sich auf eine einzelne Projektmappe.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Die Eigenschaftswerte, die zum Übergeben der **MSBuild** Aufgabe enthalten Parameter, die mit dem Namen **OutputRoot** und **Konfiguration**. Wenn sie nicht sind diese Parameterwerte, wenn sie bereitgestellt werden oder statische Eigenschaftswerte festgelegt.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

Sie können auch sehen, die **MSBuild** Vorgang ruft ein Ziel mit dem Namen **erstellen**. Dies ist eine mehrere integrierte Ziele, die häufig in Visual Studio-Projektdateien und stehen zur Verfügung, die Sie in Ihre benutzerdefinierte Projektdateien verwendet wird, z. B. werden **erstellen**, **Bereinigen**, **Rebuild**, und **veröffentlichen**. Erfahren Sie mehr über die Verwendung von Zielen und Aufgaben Steuern des Buildprozesses und über die **MSBuild** Aufgabe insbesondere weiter unten in diesem Thema.

> [!NOTE]
> Weitere Informationen zu Zielen finden Sie unter [MSBuild-Ziele](https://msdn.microsoft.com/library/ms171462.aspx).


## <a name="splitting-project-files-to-support-multiple-environments"></a>Aufteilen von Projektdateien, um mehrere Umgebungen zu unterstützen.

Angenommen, Sie möchten eine Lösung in mehreren Umgebungen, wie von Testservern, staging Plattformen und produktionsumgebungen bereitstellen können. Die Konfiguration kann erheblich variieren, zwischen diesen Umgebungen&#x2014;nicht nur im Hinblick auf den Servernamen, Verbindungszeichenfolgen und So weiter, sondern auch potenziell im Hinblick auf Anmeldeinformationen, Sicherheitseinstellungen und viele andere Faktoren. Wenn Sie dies regelmäßig durchführen müssen, ist es nicht sehr zweckmäßig, um mehrere Eigenschaften in der Projektdatei zu bearbeiten, jedes Mal, wenn Sie die zielumgebung wechseln. Trotzdem ist eine ideale Lösung eine endlose Liste von Eigenschaftswerten für den Buildprozess bereitgestellt werden müssen.

Glücklicherweise ist eine Alternative vorhanden. MSBuild können Sie die Buildkonfiguration auf mehreren Projektdateien aufgeteilt. Um die Funktionsweise zu sehen, in der Beispielprojektmappe Beachten Sie, dass es zwei benutzerdefinierte Projektdateien:

- *Publish.proj*mit Elementen, Eigenschaften und Ziele, die gelten für alle Umgebungen.
- *Env Dev.proj*, enthält Eigenschaften, die für eine entwicklerumgebung spezifisch sind.

Beachten Sie, dass jetzt die *Publish.proj* -Datei enthält eine [Import](https://msdn.microsoft.com/library/92x05xfs.aspx) Element direkt unterhalb der öffnenden **Projekt** Tag.


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


Die **importieren** Element wird verwendet, um den Inhalt von einem anderen MSBuild-Projektdatei in die aktuelle MSBuild-Projektdatei importieren. In diesem Fall die **TargetEnvPropsFile** Parameter enthält der Dateiname der Projektdatei, die Sie importieren möchten. Sie können einen Wert für diesen Parameter bereitstellen, wenn Sie MSBuild ausführen.


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


Dies führt tatsächlich den Inhalt von zwei Dateien in einer Projektdatei zusammen. Bei dieser Vorgehensweise können Sie eine Projektdatei mit der universellen Buildkonfiguration und mehrere zusätzliche Projektdateien mit umgebungsspezifische Eigenschaften erstellen. Folglich kann einfach einen Befehl mit einem anderen Wert aus die Projektmappe in eine andere Umgebung bereitstellen.

![](understanding-the-project-file/_static/image3.png)

Teilen die Projektdateien auf diese Weise wird empfohlen, folgen. Es ermöglicht Entwicklern Bereitstellung in mehreren Umgebungen durch Ausführen eines einzelnen Befehls die Duplizierung der universal Buildeigenschaften über mehrere Projektdateien zu vermeiden.

> [!NOTE]
> Anleitungen zum Anpassen der umgebungsspezifische-Projektdateien für eigene Server-Umgebungen finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema eine allgemeine Einführung in MSBuild-Projektdateien bereitgestellt und erläutert, wie Sie eigene benutzerdefinierte Projektdateien zum Steuern des Buildprozesses erstellen können. Es wurde auch das Konzept der Projektdateien in universelle Buildanweisungen und umgebungsspezifische Buildeigenschaften, erleichtern das Erstellen und Bereitstellen von Projekten an mehrere Ziele aufgeteilt.

Im nächsten Thema [Verständnis des Build-Prozesses](understanding-the-build-process.md), bietet mehr Einblick in die Verwendung zu Steuerelement Build- und Bereitstellungsprozess Projektdateien werden Sie durch die Bereitstellung einer Lösung mit einer realistischen Maß an Komplexität.

## <a name="further-reading"></a>Weiterführende Themen

Eine ausführliche Einführung in die Projektdateien und die WPP finden Sie unter [innerhalb der Microsoft Build Engine: Verwenden von MSBuild und Team Foundation Build](http://amzn.com/0735645248) Sayed Ibrahim Hashimi und William Bartholomew, ISBN-Nummer: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Zurück](setting-up-the-contact-manager-solution.md)
> [Weiter](understanding-the-build-process.md)
