---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Konfigurieren von Parametern für die Bereitstellung von Paketen | Microsoft Docs
author: jrjlee
description: In diesem Thema wird beschrieben, wie Parameterwerte, z. B. Internetinformationsdienste (Internet Information Services, IIS)-Web-Anwendungsnamen, Verbindungszeichenfolgen und Dienstendpunkte festlegen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: 7be08f1a1fb7232911a44cf64e2e784dbb95ff48
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880400"
---
<a name="configuring-parameters-for-web-package-deployment"></a>Konfigurieren von Parametern für die Bereitstellung von Paketen
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt das Festlegen von Parameterwerten, wie Internetinformationsdienste (Internet Information Services, IIS)-Web-Anwendungsnamen, Verbindungszeichenfolgen und Dienstendpunkte, wenn Sie eine Webpaket auf einem remote-IIS-Webserver bereitstellen.


Bei der Erstellung von einem Webanwendungsprojekt, die Build- und Verpackungsprozesses wird die drei Schlüsseldateien generiert:

- Ein *[Projektname] ZIP* Datei. Dies ist das Webbereitstellungspaket für Ihr Webprojekt für die Anwendung. Dieses Paket enthält alle Assemblys, Dateien, die Datenbankskripts und Ressourcen, die erforderlich sind, um Ihre Webanwendung auf einem Remotewebserver IIS neu zu erstellen.
- Ein *[Projektname].deploy.cmd* Datei. Dieses enthält eine Reihe von parametrisierten Web Deploy (MSDeploy.exe)-Befehlen, die Ihre Webbereitstellungspaket auf einem remote-IIS-Webserver veröffentlichen.
- Ein *[Projektname]. SetParameters.xml* Datei. Dies bietet einen Satz von Parameterwerten an den MSDeploy.exe-Befehl. Sie können aktualisieren Sie die Werte in dieser Datei und übergeben sie in Web Deploy als Befehlszeilenparameter beim Bereitstellen von Web-Paket.

> [!NOTE]
> Weitere Informationen zu den Build- und Verpackungsprozess ausgeführt, finden Sie unter [erstellen und Packen Webanwendungsprojekte](building-and-packaging-web-application-projects.md).


Die *SetParameters.xml* dynamisch aus der Projektdatei, Web-Anwendung und alle Konfigurationsdateien innerhalb des Projekts generiert. Wenn Sie erstellen und Packen Sie das Projekt sowie die Publishing Web Pipeline (WPP) erkennt viele der Variablen automatisch, die wahrscheinlich zwischen bereitstellungsumgebungen, wie das Ziel-IIS-Webanwendung und alle Datenbank-Verbindungszeichenfolgen ändern. Diese Werte automatisch in das Webbereitstellungspaket parametrisiert und hinzugefügt werden, um die *SetParameters.xml* Datei. Angenommen, Sie fügen eine Verbindungszeichenfolge für die *"Web.config"* Datei in das Webanwendungsprojekt, während des Erstellungsprozesses erkennt diese Änderung, und fügen einen Eintrag in einen der *SetParameters.xml* Datei entsprechend.

In vielen Fällen wird diese automatische Parametrisierung ausreichend sein. Jedoch, wenn Ihre Benutzer benötigen, um andere Einstellungen zwischen bereitstellungsumgebungen, wie z. B. Anwendungseinstellungen für die oder Dienst-Endpunkt-URLs zu verändern müssen Sie die WPP parametrisiert werden diese Werte im Bereitstellungspaket und die entsprechendeEinträgehinzugefügt*SetParameters.xml* Datei. Die folgenden Abschnitten wird erläutert, wie dies tun.

### <a name="automatic-parameterization"></a>Automatische Parametrisierung

Beim Erstellen und Packen Sie eine Webanwendung wird die WPP automatisch diese Dinge parametrisieren:

- Das Ziel IIS web-Anwendungspfad und Name.
- Verbindungszeichenfolgen in Ihrer *"Web.config"* Datei.
- Verbindungszeichenfolgen für alle Datenbanken, die Sie, um hinzufügen die **SQL packen/veröffentlichen** Registerkarte in den Eigenschaftenseiten des Projekts.

Beispielsweise würden Sie zum Erstellen und Packen Sie die [Vorgesetzten Kontakts](the-contact-manager-solution.md) Beispielprojektmappe ohne berühren der Parametrisierung in keiner Weise WPP würde dies generieren *ContactManager.Mvc.SetParameters.xml* Datei:


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


In diesem Fall gilt Folgendes:

- Die **IIS Web Application Name** Parameter ist, den IIS-Pfad, in dem die Webanwendung bereitgestellt werden soll. Der Standardwert stammt aus dem **Web packen/veröffentlichen** Seite auf den Eigenschaftenseiten des Projekts.
- Die **ApplicationServices-Datei "Web.config" Verbindungszeichenfolge** -Parameter generiert wurde eine **ConnectionStrings/hinzufügen** Element in der *"Web.config"* Datei. Es stellt die Verbindungszeichenfolge, die die Anwendung verwendet werden sollen, kontaktiert die Mitgliedschaftsdatenbank dar. Sie geben Sie an dieser Stelle wird sein Wert in der bereitgestellten eingesetzt *"Web.config"* Datei. Der Standardwert stammt aus dem vor der Bereitstellung *"Web.config"* Datei.

Die WPP parametrisiert auch diese Eigenschaften in das Bereitstellungspaket, das sie generiert. Sie können Werte für diese Eigenschaften bereitstellen, wenn Sie das Bereitstellungspaket installieren. Wenn Sie das Paket manuell über IIS-Manager installieren, wie in beschrieben [Webpaketen manuell installieren](manually-installing-web-packages.md), der Installations-Assistenten aufgefordert, Werte für alle Parameter angeben. Bei der Installation des Pakets Remoteverwendung der *. deploy.cmd* Datei, wie in beschrieben [Bereitstellen von Webpaketen](deploying-web-packages.md), Web Deploy sieht in dieser *SetParameters.xml* Datei wird in Geben Sie die Parameterwerte. Sie können die Werte im Bearbeiten der *SetParameters.xml* -Datei manuell ein, oder passen Sie die Datei als Teil eines automatisierten Build- und Bereitstellungsprozess-Prozesses. Dieser Prozess wird weiter unten in diesem Thema ausführlicher beschrieben.

### <a name="custom-parameterization"></a>Benutzerdefinierte Parametrisierung

In komplexeren Bereitstellungsszenarios sollten Sie häufig zusätzliche Eigenschaften zu parametrisieren, bevor Sie das Projekt bereitstellen. Im Allgemeinen sollten Sie parametrisiert werden alle Eigenschaften und Einstellungen, die zwischen zielumgebungen variiert. Dazu gehören:

- Dienstendpunkte in der *"Web.config"* Datei.
- Einstellungen für Anwendungen in der *"Web.config"* Datei.
- Geben Sie Benutzer auffordern, andere deklarativen Eigenschaften, denen Sie möchten.

Die einfachste Möglichkeit, diese Eigenschaften zu parametrisieren hinzufügen wird ein *parameters.xml* Datei in den Stammordner, der das Webanwendungsprojekt. Angenommen, in der Projektmappe Contact Manager ContactManager.Mvc-Projekt enthält eine *parameters.xml* Datei im Stammordner.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Wenn Sie diese Datei öffnen, sehen Sie, dass es sich um ein einzelnes enthält **Parameter** Eintrag. Der Eintrag verwendet eine Abfrage der XML Path Language (XPath) zum Suchen und parametrisieren die Endpunkt-URL des Diensts ContactService Windows Communication Foundation (WCF) in der *"Web.config"* Datei.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


Zusätzlich zu parametrisieren die Endpunkt-URL in das Bereitstellungspaket, fügt die WPP auch einen entsprechenden Eintrag zur der *SetParameters.xml* Datei, die zusammen mit dem Bereitstellungspaket generiert wird.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


Wenn Sie das Bereitstellungspaket manuell installieren, fordert IIS-Manager Sie zur Adresse Dienstendpunkts zusammen mit den Eigenschaften, die automatisch parametrisiert wurden. Bei der Installation des Bereitstellungspakets durch Ausführen der *. deploy.cmd* -Datei, die Sie bearbeiten die *SetParameters.xml* Datei, geben Sie einen Wert für die Dienst-Endpunktadresse sowie Werte für die Eigenschaften, die automatisch parametrisiert wurden.

Umfassende Informationen zum Erstellen einer *parameters.xml* finden Sie unter [wie: Verwenden von Parametern zum Konfigurieren von Einstellungen beim ein Bereitstellungspaket installiert ist](https://msdn.microsoft.com/library/ff398068.aspx). Die Prozedur mit dem Namen **Bereitstellungsparameter für Einstellungen in der Datei "Web.config" verwendet** enthält schrittweise Anleitungen.

## <a name="modifying-the-setparametersxml-file"></a>Ändern die Datei SetParameters.Xml

Wenn Sie das Web-Anwendungspaket manuell bereitstellen möchten&#x2014;entweder durch Ausführen der *. deploy.cmd* Datei oder durch Ausführen von MSDeploy.exe über die Befehlszeile&#x2014;es ist nichts zum Beenden Sie die manuelle Bearbeitung der  *SetParameters.xml* Datei vor der Bereitstellung. Wenn Sie eine Enterprise-Skalierung-Lösung arbeiten, müssen Sie ein webanwendungspaket als Teil eines größeren, automatisierte Build- und Bereitstellungsprozess Prozesses bereitstellen. In diesem Szenario benötigen Sie die Microsoft Build Engine (MSBuild) so ändern Sie die *SetParameters.xml* -Datei für Sie. Hierzu können Sie mithilfe von MSBuild **XmlPoke** Aufgabe.

Die [Vorgesetzten Kontakts Beispielprojektmappe](the-contact-manager-solution.md) zeigt diesen Vorgang. Die Codebeispiele, die Folgen haben bearbeitet, um nur die Details anzuzeigen, die in diesem Beispiel relevant sind.

> [!NOTE]
> Eine umfassendere Übersicht über das Projektmodell-Datei in der Beispielprojektmappe und eine Einführung in benutzerdefinierte Projektdateien im Allgemeinen finden Sie unter [verstehen die Projektdatei](understanding-the-project-file.md) und [Verständnis des Build-Prozesses](understanding-the-build-process.md).


Zunächst werden die Parameterwerte von Interesse sind als Eigenschaften in der Projektdatei umgebungsspezifische definiert (z. B. *Env Dev.proj*).


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> Anleitungen zum Anpassen der umgebungsspezifische-Projektdateien für eigene Server-Umgebungen finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Als Nächstes wird die *Publish.proj* Datei importiert, diese Eigenschaften. Da jede *SetParameters.xml* Datei zugeordnet ist ein *. deploy.cmd* Datei möchten, und es letztlich die Projektdatei, um jede aufzurufen *. deploy.cmd* Datei des Projekts erstellt eine MSBuild *Element* für jede *. deploy.cmd* Datei und definiert die Eigenschaften von Interesse sind, als *Elementmetadaten*.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


In diesem Fall gilt Folgendes:

- Die **ParametersXml** Metadatenwert gibt die Position von der *SetParameters.xml* Datei.
- Die **IisWebAppName** Wert ist, den IIS-Pfad zu dem die Webanwendung bereitgestellt werden soll.
- Die **MembershipDBConnectionString** Wert ist die Verbindungszeichenfolge für die Mitgliedschaftsdatenbank und die **MembershipDBConnectionName** Wert ist die **Namen** Attribut des entsprechenden Parameters in der *SetParameters.xml* Datei.
- Die **ServiceEndpointValue** Wert ist die Endpunktadresse für den WCF-Dienst auf dem Zielserver und die **ServiceEndpointParamName** Wert ist das Namensattribut des entsprechenden Parameters in die *SetParameters.xml* Datei.

Schließlich in der *Publish.proj* Datei, die **PublishWebPackages** verwendet als Ziel der **XmlPoke** Task so ändern Sie diese Werte in der *SetParameters.xml* Datei.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


Sie werden bemerken, dass jedes **XmlPoke** Aufgabe werden die vier Werte des Attributs angegeben:

- Die **XmlInputPath** -Attribut weist der Aufgabe, wo Sie die Datei finden Sie ändern möchten.
- Die **Abfrage** Attribut ist eine XPath-Abfrage, die den XML-Knoten identifiziert Sie ändern möchten.
- Die **Wert** Attribut ist der neue Wert in der ausgewählten XML-Knoten eingefügt werden soll.
- Die **Bedingung** Attribut gibt die Kriterien, die auf dem der Task ausgeführt werden sollen oder nicht ausgeführt. In diesen Fällen die Bedingung wird sichergestellt, dass Sie nicht zum Einfügen eines Werts null oder leer sein, in versuchen der *SetParameters.xml* Datei.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschrieben die Rolle der *SetParameters.xml* Datei, und erläutert, wie er generiert wird, wenn Sie ein Webanwendungsprojekt erstellen. Diese erläutert, wie Sie zusätzliche Einstellungen durch Hinzufügen von parametrisieren können eine *parameters.xml* Datei zum Projekt. Es auch beschrieben, wie Sie ändern können die *SetParameters.xml* Datei als Teil einer größeren, automatisierte Buildprozesses mithilfe der **XmlPoke** Aufgabe in den Projektdateien.

Im nächsten Thema [Bereitstellen von Webpaketen](deploying-web-packages.md), beschreibt, wie Sie eine Web-Paket entweder durch Ausführen von bereitstellen können die *. deploy.cmd* Datei oder mithilfe von MSDeploy.exe-Befehle direkt. In beiden Fällen geben Sie Ihre *SetParameters.xml* Datei als Bereitstellungsparameter.

## <a name="further-reading"></a>Weiterführende Themen

Informationen zum Erstellen von Webpaketen finden Sie unter [erstellen und Packen Webanwendungsprojekte](building-and-packaging-web-application-projects.md). Hinweise zum tatsächlich ein Webpaket bereitgestellt werden, finden Sie unter [Bereitstellen von Webpaketen](deploying-web-packages.md). Eine schrittweise Anleitung zum Erstellen einer *parameters.xml* finden Sie unter [wie: Verwenden von Parametern zum Konfigurieren von Einstellungen beim ein Bereitstellungspaket installiert ist](https://msdn.microsoft.com/library/ff398068.aspx).

Weitere allgemeine Informationen zur Parametrisierung im Web Deploy, finden Sie unter [Web bereitstellen Parametrisierung in Aktion](https://go.microsoft.com/?linkid=9805119) (Blogbeitrag).

> [!div class="step-by-step"]
> [Zurück](building-and-packaging-web-application-projects.md)
> [Weiter](deploying-web-packages.md)
