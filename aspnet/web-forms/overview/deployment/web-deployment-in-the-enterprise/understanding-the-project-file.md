---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
title: Grundlegendes zur Projektdatei | Microsoft-Dokumentation
author: jrjlee
description: Microsoft Build Engine (MSBuild) Projektdateien bilden den Kern des Build & Deployment-Prozesses. Dieses Thema beginnt mit eine konzeptionelle Übersicht über MSBuild...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 07978d9d-341c-4524-bcba-62976f390f77
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/understanding-the-project-file
msc.type: authoredcontent
ms.openlocfilehash: 89c5c7906ccfc453195b788cbc6393dc74cda1fb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377203"
---
<a name="understanding-the-project-file"></a>Grundlegendes zur Projektdatei
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Microsoft Build Engine (MSBuild) Projektdateien bilden den Kern des Build & Deployment-Prozesses. Dieses Thema beginnt mit eine konzeptionelle Übersicht über MSBuild und der Projektdatei. Es wird beschrieben, die wichtigsten Komponenten, die Sie stoßen werden, wenn Sie mit dem Projektdateien arbeiten, und anhand eines Beispiels, wie Sie die Projektdateien verwenden können, zum Bereitstellen von realen Anwendungen funktioniert.
> 
> Sie lernen Folgendes:
> 
> - Wie MSBuild MSBuild-Projektdateien verwendet, um Projekte zu erstellen.
> - Wie MSBuild bereitstellungstechnologien, z. B. das Internet Information Services (IIS)-Webbereitstellungstool (Web Deploy) integriert werden.
> - Informationen zum Verständnis der wichtigsten Komponenten einer Projektdatei.
> - Wie können Sie Projektdateien erstellen und Bereitstellen von komplexen Anwendungen.


## <a name="msbuild-and-the-project-file"></a>MSBuild und der Projektdatei

Beim Erstellen und Projektmappen in Visual Studio erstellen, verwendet Visual Studio MSBuild, um jedes Projekt in der Projektmappe zu erstellen. Jedes Visual Studio-Projekt enthält, mit der Dateierweiterung, die den Typ des Projekts wiedergibt eine MSBuild-Projektdatei&#x2014;z. B. eine c#-Projekt (csproj), ein Projekt Visual Basic.NET (.vbproj) oder ein Datenbankprojekt (.dbproj). Um ein Projekt erstellen, muss MSBuild die Projektdatei, die dem Projekt zugeordneten verarbeiten. Die Projektdatei ist ein XML-Dokument, das enthält alle Informationen und Anweisungen, dass MSBuild zum Erstellen des Projekts, z. B. den Inhalt einschließen, die plattformanforderungen, Versionsinformationen, Webserver oder datenbankeinstellungen für Server muss, und die Aufgaben, die ausgeführt werden müssen.

MSBuild-Projektdateien werden basierend auf den [MSBuild XML-Schema](https://msdn.microsoft.com/library/5dy88c2e.aspx), und daher der Buildprozess vollständig geöffnet und transparent. Darüber hinaus, nicht müssen Sie Visual Studio zu installieren, um die MSBuild-Engine verwenden&#x2014;ausführbaren Datei MSBuild.exe ist Teil von .NET Framework, und Sie können es über eine Eingabeaufforderung ausführen. Als Entwickler können Sie erstellen Ihre eigenen MSBuild-Projektdateien, mit der MSBuild-XML-Schema, um zu erzwingen komplexer und eine präzise Steuerung wie Ihre Projekte erstellt und bereitgestellt werden. Diese benutzerdefinierte Projektdateien funktioniert auf genau die gleiche Weise wie die Projektdateien, die Visual Studio automatisch generiert.

> [!NOTE]
> Sie können auch MSBuild-Projektdateien mit dem Dienst Team Build in Team Foundation Server (TFS) verwenden. Beispielsweise können Sie die Projektdateien in Szenarien mit fortlaufender Integration (CI) zur automatischen Bereitstellung in einer testumgebung aus, bei der neuer Code eingecheckt wird. Weitere Informationen finden Sie unter [Konfigurieren von Team Foundation Server für die automatisierte Bereitstellung](../configuring-team-foundation-server-for-web-deployment/configuring-team-foundation-server-for-web-deployment.md).


### <a name="project-file-naming-conventions"></a>Benennungskonventionen für Projektdatei

Wenn Sie Ihre eigenen Projektdateien erstellen, können Sie eine Dateierweiterung für gewünschte. Um Ihre Lösungen für andere Benutzer zu vereinfachen, sollten Sie jedoch diese häufig verwendete Konventionen verwenden:

- Verwenden Sie die proj-Erweiterung aus, wenn Sie eine Projektdatei erstellen, die Projekte erstellt.
- Verwenden Sie die Erweiterung targets, bei der Erstellung einer wiederverwendbaren Projektdatei in andere Projektdateien importiert. Dateien mit der Erweiterung targets erstellen in der Regel nicht alles selbst, die sie einfach Anweisungen enthalten, die Sie in Ihre proj-Dateien importieren können.

### <a name="integration-with-deployment-technologies"></a>Integration mit Bereitstellungstechnologien

Wenn Sie mit Webanwendungsprojekten in Visual Studio 2010 wie ASP.NET-Webanwendungen und ASP.NET MVC-Webanwendungen, gearbeitet haben, wissen Sie, dass diese Projekte die integrierten Unterstützung für das Packen und Bereitstellen der Webanwendung in einer zielumgebung enthalten. Die **Eigenschaften** Seiten für diese Projekte enthalten **Web packen/veröffentlichen** und **SQL packen/veröffentlichen** Registerkarten an, die Sie verwenden können, so konfigurieren Sie wie die Komponenten Ihrer Anwendung werden verpackt und bereitgestellt. Dadurch wird die **Web packen/veröffentlichen** Registerkarte:

![](understanding-the-project-file/_static/image1.png)

Die zugrunde liegende Technologie hinter dieser Funktionen wird als Web Publishing Pipeline (WPP) bezeichnet. Die WPP bietet im Wesentlichen MSBuild und [Web Deploy](https://go.microsoft.com/?linkid=9805122) zusammen, um einem durchgehenden Prozess von Builds, Paket und Bereitstellung für Ihre Webanwendungen bereit.

Die gute Nachricht ist, dass Sie die Integrationspunkte, die die WPP bereitstellt nutzen können, bei der Erstellung von benutzerdefinierten Projektdateien für Webprojekte. Sie können Anweisungen zur Bereitstellung in Ihrer Projektdatei einschließen, können Sie Ihre Projekte erstellen, Erstellen von Bereitstellungspaketen für Web und Installieren der Pakete auf Remoteservern über eine einzelne Projektdatei und einem einzelnen Aufruf zu MSBuild. Sie können auch jede andere ausführbare Dateien als Teil des Buildprozesses aufrufen. Beispielsweise können Sie das Befehlszeilentool "VSDBCMD.exe" zum Bereitstellen einer Datenbank aus einer Schemadatei ausführen. Im Verlauf dieses Themas erfahren Sie, wie Sie diese Funktionen, die die Anforderungen Ihrer Bereitstellungsszenarios nutzen können.

> [!NOTE]
> Weitere Informationen zur Funktionsweise von des Bereitstellungsprozess für Web-Anwendung finden Sie unter [ASP.NET Web Application Project Deployment Overview](https://msdn.microsoft.com/library/dd394698.aspx).


## <a name="the-anatomy-of-a-project-file"></a>Anatomie einer Projektdatei

Bevor Sie mit dem Buildprozess im Detail ansehen, ist es lohnt sich einen Moment, sich mit der grundlegenden Struktur einer MSBuild-Projektdatei vertraut machen konnten. Dieser Abschnitt enthält eine Übersicht über die häufigsten Elemente, die auftreten müssen, wenn Sie überprüfen, bearbeiten oder erstellen eine Projektdatei. Insbesondere lernen Sie Folgendes:

- Gewusst wie: Verwenden Sie *Eigenschaften* Variablen für den Buildvorgang zu verwalten.
- Gewusst wie: Verwenden Sie *Elemente* um die Eingaben für den Buildprozess zu identifizieren, wie Sie Codedateien.
- Gewusst wie: Verwenden Sie *Ziele* und *Aufgaben* MSBuild, Anweisungen zur Ausführung anbieten mit *Eigenschaften* und *Elemente* an anderer Stelle definiert die Projektdatei.

Dies zeigt die Beziehung zwischen die wichtigsten Elemente in einem MSBuild-Projektdatei:

![](understanding-the-project-file/_static/image2.png)

### <a name="the-project-element"></a>Project-Element

Die [Projekt](https://msdn.microsoft.com/library/bcxfsh87.aspx) Element ist das Stammelement jeder Projektdatei. Zusätzlich zur Identifizierung der XML-Schema für die Projektdatei, die **Projekt** Element kann enthalten Attribute, um die Einstiegspunkte für den Buildprozess angegeben. Z. B. in der [Contact Manager-beispiellösung](the-contact-manager-solution.md), *Publish.proj* legt fest, dass der Build gestartet werden soll, durch den Aufruf des Ziels, mit dem Namen **FullPublish**.


[!code-xml[Main](understanding-the-project-file/samples/sample1.xml)]


### <a name="properties-and-conditions"></a>Eigenschaften und Bedingungen

Eine Projektdatei muss in der Regel bieten viele unterschiedliche Arten von Informationen, um erfolgreich zu erstellen und Bereitstellen der Projekte. Servernamen, Verbindungszeichenfolgen, Anmeldeinformationen, mit Projektmappenbuild-Konfigurationen, Quelle und Ziel-Dateipfade und andere Informationen, die zur Unterstützung der Anpassung enthalten sein sollen, können diese Angaben enthalten. In einer Projektdatei Eigenschaften definiert werden müssen, innerhalb einer [PropertyGroup](https://msdn.microsoft.com/library/t4w159bs.aspx) Element. MSBuild-Eigenschaften bestehen aus Schlüssel-Wert-Paaren. In der **PropertyGroup** -Element, den Elementnamen definiert, die Schlüssel und den Inhalt des Elements definiert, den Wert der Eigenschaft. Sie können z. B. Eigenschaften, die mit dem Namen definieren **ServerName** und **"ConnectionString"** auf eine statische Server und die Verbindungszeichenfolge der Zeichenfolge zu speichern.


[!code-xml[Main](understanding-the-project-file/samples/sample2.xml)]


Um einen Eigenschaftswert abzurufen, verwenden Sie das Format <strong>$(</strong><em>PropertyName</em><strong>)</strong><em>.</em> Beispielsweise, um das Abrufen des Werts der <strong>ServerName</strong> -Eigenschaft, geben Sie Folgendes ein:


[!code-powershell[Main](understanding-the-project-file/samples/sample3.ps1)]


> [!NOTE]
> Beispiele dafür, wie und wann Sie die Eigenschaftswerte verwenden, weiter unten in diesem Thema angezeigt.


Einbetten von Informationen als statische Eigenschaften in einer Projektdatei ist nicht immer den idealen Ansatz für die Verwaltung des Buildprozesses. In vielen Szenarien sollten Sie die Informationen aus anderen Quellen abrufen oder Entscheidungsgewalt für den Benutzer, die Informationen über die Eingabeaufforderung bereitzustellen. MSBuild können Sie einen beliebigen Eigenschaftswert als Befehlszeilenparameter angeben. Beispielsweise kann der Benutzer einen Wert für bereitstellen **ServerName** wann er ausgeführt wird MSBuild.exe über die Befehlszeile.


[!code-console[Main](understanding-the-project-file/samples/sample4.cmd)]


> [!NOTE]
> Weitere Informationen zu die Argumente und Schalter Sie mit MSBuild.exe können, finden Sie unter [MSBuild-Befehlszeilenreferenz](https://msdn.microsoft.com/library/ms164311.aspx).


Sie können die gleiche Eigenschaftensyntax verwenden, um die Werte der Umgebungsvariablen und integrierten Eigenschaften zu erhalten. Viele häufig verwendete Eigenschaften, die für Sie definiert sind, und können Sie sie in den Projektdateien durch den Namen des entsprechenden Parameters einschließen. Beispielsweise zum Abrufen der aktuellen Projektplattform&#x2014;z. B. **X86** oder **"anycpu"**&#x2014;zählen Sie die **$(Platform)** eigenschaftsreferenz im der Projektdatei. Weitere Informationen finden Sie unter [Makros für Buildbefehle und-Eigenschaften](https://msdn.microsoft.com/library/c02as0cs.aspx), [gemeinsame MSBuild-Projekteigenschaften](https://msdn.microsoft.com/library/bb629394.aspx), und [reservierte Eigenschaften](https://msdn.microsoft.com/library/ms164309.aspx).

Eigenschaften werden häufig in Verbindung mit verwendet *Bedingungen*. Die meisten MSBuild-Elemente unterstützen die **Bedingung** -Attribut, das können Sie die Kriterien angeben, auf dem MSBuild sollte das Element ausgewertet. Betrachten Sie z. B. dieser Eigenschaftendefinition ein:


[!code-xml[Main](understanding-the-project-file/samples/sample5.xml)]


Wenn dieser Eigenschaftendefinition von MSBuild verarbeitet wird, wird zunächst geprüft, ob ein **$(OutputRoot)** Eigenschaftswert steht. Wenn Sie den Wert der Eigenschaft leer ist&#x2014;in anderen Worten: der Benutzer einen Wert für diese Eigenschaft angegeben wurde nicht&#x2014;ergibt die Bedingung **"true"** und den Wert der Eigenschaft auf festgelegt ist **... \Publish\Out**. Wenn der Benutzer einen Wert für diese Eigenschaft angegeben ist, ergibt die Bedingung **"false"** und der statischen Eigenschaftswert wird nicht verwendet.

Weitere Informationen zu den verschiedenen Methoden, die in dem Sie Bedingungen angeben können, finden Sie unter [MSBuild-Bedingungen](https://msdn.microsoft.com/library/7szfhaft.aspx).

### <a name="items-and-item-groups"></a>Elemente und Elementgruppen

Eine wichtige Funktion von der Projektdatei ist die Eingaben für den Buildprozess zu definieren. In der Regel sind diese Eingaben Dateien&#x2014;Codedateien, Konfigurationsdateien, Befehlsdateien und anderen Dateien, die Sie zum Verarbeiten, oder kopieren Sie benötigen als Teil des Buildprozesses. Diese Eingaben werden in der MSBuild-Projekt-Schema durch dargestellt [Element](https://msdn.microsoft.com/library/ms164283.aspx) Elemente. In einer Projektdatei Elemente definiert werden müssen, innerhalb einer [ItemGroup](https://msdn.microsoft.com/library/646dk05y.aspx) Element. Genau wie **Eigenschaft** Elemente, Sie können den Namen einer **Element** Element beliebig. Allerdings müssen Sie angeben einer **Include** Attribut identifiziert die Datei oder ein Platzhalter, der das Element darstellt.


[!code-xml[Main](understanding-the-project-file/samples/sample6.xml)]


Durch Angeben von mehreren **Element** Elemente mit dem gleichen Namen, erstellen Sie effektiv eine benannte Liste von Ressourcen. Eine gute Möglichkeit, dies in Aktion zu sehen ist, einen Blick in einem der Projektdateien zu werfen, die Visual Studio erstellt. Z. B. die *ContactManager.Mvc.csproj* -Datei in der Beispielprojektmappe enthält viele Elementgruppen, die jeweils mehrere gleichnamige **Element** Elemente.


[!code-xml[Main](understanding-the-project-file/samples/sample7.xml)]


Auf diese Weise die Projektdatei wird MSBuild anweisen, Listen, Dateien zu erstellen, die auf die gleiche Weise verarbeitet werden müssen&#x2014;der **Verweis** Liste enthält Assemblys, die für einen erfolgreichen Build, eingerichtet werden müssen die  **Kompilieren Sie** Liste enthält die Codedateien, die kompiliert werden müssen, und die **Content** Liste enthält Ressourcen, die kopiert werden müssen, die unverändert. Betrachten wir wie der Buildprozess verweist, und verwenden diese Elemente später in diesem Thema.

Elemente können auch enthalten [ItemMetadata](https://msdn.microsoft.com/library/ms164284.aspx) untergeordnete Elemente. Diese sind benutzerdefinierte Schlüssel-Wert-Paare und Eigenschaften, die speziell für dieses Element sind im Wesentlichen darstellen. Z. B. einen Großteil der **Kompilieren** Elemente in der Projektdatei enthalten **DependentUpon** untergeordnete Elemente.


[!code-xml[Main](understanding-the-project-file/samples/sample8.xml)]


> [!NOTE]
> Zusätzlich zu Benutzer erstellte Elementmetadaten werden alle Elemente auf verschiedene allgemeine Metadaten bei der Erstellung zugewiesen. Weitere Informationen finden Sie unter [Well-known Item Metadata (Bekannte Elementmetadaten)](https://msdn.microsoft.com/library/ms164313.aspx).


Sie können erstellen **ItemGroup** Elemente innerhalb der Stammebene **Projekt** Element oder in bestimmten **Ziel** Elemente. **ItemGroup** Elemente unterstützen auch **Bedingung** Attribute, die Sie die Eingaben für den Buildprozess gemäß den Bedingungen wie die Konfiguration des Projekts oder der Plattform anpassen können.

### <a name="targets-and-tasks"></a>Ziele und Aufgaben

In der MSBuild-Schema eine [Aufgabe](https://msdn.microsoft.com/library/77f2hx1s.aspx) -Element stellt dar, einen einzelnen Build-Anweisung (oder Aufgabe). MSBuild umfasst eine Vielzahl von vordefinierten Aufgaben. Zum Beispiel:

- Die **Kopie** Task kopiert Dateien an einem neuen Speicherort.
- Die **Csc** -Aufgabe aufruft, Visual c#-Compiler.
- Die **Vbc** -Aufgabe aufruft, Visual Basic-Compiler.
- Die **Exec** Task ausgeführt wird, ein bestimmtes Programm.
- Die **Nachricht** Task schreibt eine Meldung an eine Protokollierung.

> [!NOTE]
> Vollständige Details zu Aufgaben, die standardmäßig verfügbar sind, finden Sie unter [MSBuild-Aufgabenreferenz](https://msdn.microsoft.com/library/7z253716.aspx). Weitere Informationen zu Aufgaben, wie Sie eigene benutzerdefinierte Aufgaben zu erstellen, finden Sie unter [MSBuild-Aufgaben](https://msdn.microsoft.com/library/ms171466.aspx).


Aufgaben müssen immer enthaltenen [Ziel](https://msdn.microsoft.com/library/t50z2hka.aspx) Elemente. Ein **Ziel** Element ist ein Satz von einer oder mehrerer Aufgaben, die nacheinander ausgeführt werden, und eine Projektdatei kann mehrere Ziele enthalten. Wenn Sie eine Aufgabe oder eine Reihe von Aufgaben ausführen möchten, rufen Sie das Ziel, das sie enthält. Nehmen wir beispielsweise an, dass Sie eine einfache Projektdatei verfügen, die eine Meldung protokolliert.


[!code-xml[Main](understanding-the-project-file/samples/sample9.xml)]


Sie können das Ziel in der Befehlszeile aufrufen, indem Sie mit der **/t /** verwenden, um das Ziel anzugeben.


[!code-console[Main](understanding-the-project-file/samples/sample10.cmd)]


Sie können auch hinzufügen, eine **DefaultTargets** -Attribut auf die **Projekt** Element, an die Ziele, die Sie aufrufen möchten.


[!code-xml[Main](understanding-the-project-file/samples/sample11.xml)]


In diesem Fall müssen Sie das Ziel in der Befehlszeile angeben. Können Sie einfach die Projektdatei angeben, und ruft MSBuild den **FullPublish** Ziel für Sie.


[!code-console[Main](understanding-the-project-file/samples/sample12.cmd)]


Ziele und die Aufgaben zählen **Bedingung** Attribute. Daher können Sie auswählen, um die gesamte Ziele oder einzelne Aufgaben zu unterdrücken, wenn bestimmte Bedingungen erfüllt sind.

Wenn Sie nützliche Aufgaben und Ziele erstellen, müssen Sie im Allgemeinen finden in die Eigenschaften und Elemente, die Sie an anderer Stelle in der Projektdatei definiert haben:

- Um einen Eigenschaftswert zu verwenden, geben <strong>$(</strong><em>PropertyName</em><strong>)</strong>, wobei <em>PropertyName</em> ist der Name des der <strong>-Eigenschaft</strong> Element oder den Namen des Parameters.
- Um ein Element zu verwenden, geben <strong>@(</strong><em>ItemName</em><strong>)</strong>, wobei <em>ItemName</em> ist der Name des der <strong>Element</strong> Element.

> [!NOTE]
> Denken Sie daran, dass wenn Sie mehrere Elemente mit dem gleichen Namen erstellen, Sie eine Liste erstellen. Im Gegensatz dazu, wenn Sie mehrere Eigenschaften mit dem gleichen Namen erstellen, der letzte Eigenschaftenwert, die Sie bereitstellen, überschreibt alle vorherigen Eigenschaften mit dem gleichen Namen&#x2014;eine Eigenschaft kann nur einen einzelnen Wert enthalten.


Z. B. in der *Publish.proj* Datei in der Beispielprojektmappe, sehen Sie sich die **BuildProjects** Ziel.


[!code-xml[Main](understanding-the-project-file/samples/sample13.xml)]


In diesem Beispiel können Sie diese wichtigen Punkte beachten:

- Wenn die **BuildingInTeamBuild** Parameter angegeben ist, und hat den Wert **"true"**, keinen der Tasks in diesem Ziel wird ausgeführt.
- Das Ziel enthält eine einzelne Instanz der [MSBuild](https://msdn.microsoft.com/library/z7f65y0d.aspx) Aufgabe. Mit diesem Task können Sie die anderen MSBuild-Projekte zu erstellen.
- Die **ProjectsToBuild** Element an die Aufgabe übergeben wird. Dieses Element kann eine Liste der Projektmappen- oder Projektdateien, durch alle definierten darstellen **ProjectsToBuild** item-Elemente in einer Elementgruppe. In diesem Fall die **ProjectsToBuild** Element bezieht sich auf einer einzelnen Projektmappe-Datei.

    [!code-xml[Main](understanding-the-project-file/samples/sample14.xml)]
- Die Eigenschaftswerte, die an die **MSBuild** Task enthalten Parameter, die mit dem Namen **OutputRoot** und **Konfiguration**. Diese werden Parameterwerte, wenn sie bereitgestellt werden, oder statische Eigenschaftswerte festgelegt, wenn sie nicht sind.

    [!code-xml[Main](understanding-the-project-file/samples/sample15.xml)]

Sie können auch sehen, die die **MSBuild** -Aufgabe aufruft, ein Ziel mit dem Namen **erstellen**. Dies ist eine der mehrere integrierte Ziele, die häufig in Visual Studio-Projektdateien und die Ihnen zur Verfügung, in den benutzerdefinierten Projektdateien, verwendet wird, wie z. B. sind **erstellen**, **Bereinigen**, **Rebuild**, und **veröffentlichen**. Erfahren Sie mehr über die Verwendung von Zielen und Aufgaben, um zu steuern des Buildprozesses, und über die **MSBuild** Aufgabe, insbesondere dann, weiter unten in diesem Thema.

> [!NOTE]
> Weitere Informationen zu Zielen finden Sie unter [MSBuild-Ziele](https://msdn.microsoft.com/library/ms171462.aspx).


## <a name="splitting-project-files-to-support-multiple-environments"></a>Aufteilen von Projektdateien, um die Unterstützung mehrerer Umgebungen

Angenommen, Sie möchten, um eine Lösung für mehrere Umgebungen wie Testserver, staging-Plattformen und produktionsumgebungen bereitstellen zu können. Die Konfiguration kann erheblich variieren, zwischen diesen Umgebungen&#x2014;nicht nur im Hinblick auf den Servernamen, Verbindungszeichenfolgen und So weiter, sondern auch potenziell in Bezug auf die Anmeldeinformationen, Sicherheitseinstellungen und viele andere Faktoren. Wenn Sie dies regelmäßig tun müssen, ist es nicht wirklich zweckmäßiger, um mehrere Eigenschaften in der Projektdatei, zu bearbeiten, jedes Mal, wenn Sie die zielumgebung wechseln. Es ist auch eine ideale Lösung zu eine endlose Liste der Eigenschaftswerte des Build-Prozesses zur Verfügung gestellt werden müssen.

Es gibt glücklicherweise eine Alternative. MSBuild können Sie die Buildkonfiguration auf mehrere Projektdateien aufgeteilt. Zur Veranschaulichung, in der Beispielprojektmappe, beachten Sie, dass es zwei benutzerdefinierte Projektdateien:

- *Publish.proj*mit Elementen, Eigenschaften und Ziele sind für alle Umgebungen.
- *Env-Dev.proj*, die Eigenschaften enthält, die für eine entwicklerumgebung spezifisch sind.

Beachten Sie, dass jetzt die *Publish.proj* -Datei enthält eine [Import](https://msdn.microsoft.com/library/92x05xfs.aspx) -Element direkt unter der öffnenden **Projekt** Tag.


[!code-xml[Main](understanding-the-project-file/samples/sample16.xml)]


Die **importieren** Element wird verwendet, um den Inhalt einer anderen MSBuild-Projektdatei in die aktuelle MSBuild-Projektdatei importieren. In diesem Fall die **TargetEnvPropsFile** Parameter enthält den Dateinamen der zu importierenden Projektdatei. Sie können einen Wert für diesen Parameter bereitstellen, wenn Sie MSBuild ausführen.


[!code-console[Main](understanding-the-project-file/samples/sample17.cmd)]


Dies führt effektiv den Inhalt der beiden Dateien in eine einzelne Projektdatei zusammen. Mit diesem Ansatz können Sie eine Projektdatei mit der universal Buildkonfiguration und mehrere zusätzliche Projektdateien, die mit umgebungsspezifische Eigenschaften erstellen. Daher kann einfach einen Befehl ausführen, mit dem eines anderen Parameterwertes die Lösung in einer anderen Umgebung bereitzustellen.

![](understanding-the-project-file/_static/image3.png)

Teilen Ihre Projektdateien auf diese Weise hat sich bewährt befolgen. Dieser Dienst ermöglicht Entwicklern in mehreren Umgebungen bereitstellen, mit einem einzigen Befehl zugleich die Duplizierung von universellen Buildeigenschaften in mehreren Projektdateien.

> [!NOTE]
> Anleitungen zum Anpassen der umgebungsspezifischen Projektdateien für Ihre eigenen serverumgebungen finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema bereitgestellten eine allgemeine Einführung in MSBuild-Projektdateien und erläutert, wie Sie Ihre eigenen benutzerdefinierten Projektdateien zum Steuern des Buildprozesses erstellen können. Einführung in auch das Konzept der Projektdateien in universelle einrichtungsanweisungen und umgebungsspezifische Buildeigenschaften, zu vereinfachen, erstellen und Bereitstellen von Projekten an mehrere Ziele aufgeteilt.

Im nächsten Thema, [Verständnis des Prozesses erstellen](understanding-the-build-process.md), bietet weitere Einblicke, wie Sie die Projektdateien auf die Erstellung und Bereitstellung verwenden können, werden Sie schrittweise durch die Bereitstellung einer Lösung mit einem realistischen Maß an Komplexität.

## <a name="further-reading"></a>Weiterführende Themen

Eine ausführlichere Einführung in Projektdateien und die Apps, finden Sie unter [innerhalb der Microsoft Build Engine: Verwenden von MSBuild und Team Foundation Build](http://amzn.com/0735645248) von Sayed Ibrahim Hashimi und William Bartholomew, ISBN: 978-0-7356-4524-0.

> [!div class="step-by-step"]
> [Zurück](setting-up-the-contact-manager-solution.md)
> [Weiter](understanding-the-build-process.md)
