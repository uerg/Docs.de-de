---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Konfigurieren von Parametern für die Bereitstellung von Paket | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie Parameterwerte, wie Internet Information Services (IIS)-Web-Anwendungsnamen, Verbindungszeichenfolgen und Dienstendpunkte festlegen...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: 6660ad52ce68932be63e2874a5f6acb34336e575
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829243"
---
<a name="configuring-parameters-for-web-package-deployment"></a>Konfigurieren von Parametern für die Webbereitstellung für Paket
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Dieses Thema beschreibt das Festlegen von Parameterwerten, wie Internet Information Services (IIS)-Web-Anwendungsnamen, Verbindungszeichenfolgen und Dienstendpunkte, wenn Sie ein Webpaket für einen remote-IIS-Webserver bereitstellen.


Wenn Sie ein Webanwendungsprojekt, die Build- und Verpackungsprozess erstellen, wird die drei Schlüsseldateien generiert:

- Ein *[Projektname] ZIP* Datei. Dies ist das Webbereitstellungspaket für Ihr Webprojekt für die Anwendung. Dieses Paket enthält alle Assemblys, Dateien, Datenbankskripts und Ressourcen, die erforderlich sind, um Ihre Webanwendung auf einem remote-IIS-Webserver neu zu erstellen.
- Ein *[Projektname].deploy.cmd* Datei. Enthält einen Satz von parametrisierten Web Deploy (MSDeploy.exe) Befehlen, die Ihr Webbereitstellungspaket auf einem remote-IIS-Webserver zu veröffentlichen.
- Ein *[Projektname]. "SetParameters.xml"* Datei. Dies bietet einen Satz von Parameterwerten, die den MSDeploy.exe-Befehl. Sie können die Werte in dieser Datei zu aktualisieren und übergeben sie in Web Deploy als Befehlszeilenparameter bei der Bereitstellung Ihrer Webpakets.

> [!NOTE]
> Weitere Informationen zu den Build- und Verpackungsprozess, finden Sie unter [erstellen und Verpacken von Webanwendungsprojekten](building-and-packaging-web-application-projects.md).


Die *"SetParameters.xml"* dynamisch aus der Projektdatei der Web-Anwendung und alle Konfigurationsdateien in Ihrem Projekt generiert. Wenn Sie zu erstellen und Packen das Projekt, das Web Publishing Pipeline (WPP) erkennt viele der Variablen automatisch, die wahrscheinlich zwischen bereitstellungsumgebungen, wie die Ziel-IIS-Webanwendung und eine beliebige Datenbank-Verbindungszeichenfolgen ändern. Diese Werte automatisch in das Webbereitstellungspaket parametrisiert und hinzugefügt werden, um die *"SetParameters.xml"* Datei. Z. B., wenn Sie eine Verbindungszeichenfolge zum Hinzufügen der *"Web.config"* Datei in das Webanwendungsprojekt, der Buildprozess erkennt diese Änderung, und fügt einen Eintrag in einen der *"SetParameters.xml"* Datei entsprechend.

In vielen Fällen wird dieser automatische Parametrisierung ausreichen. Wenn Ihre Benutzer benötigen, um andere Einstellungen zwischen bereitstellungsumgebungen, z. B. Einstellungen für Anwendungen und Dienstendpunkt-URLs, variieren Sie müssen jedoch informieren die WPP parametrisieren diese Werte in das Bereitstellungspaket und die entsprechendeEinträgehinzugefügt *"SetParameters.xml"* Datei. Die folgenden Abschnitten wird erläutert, wie dies zu tun.

### <a name="automatic-parameterization"></a>Automatische Parametrisierung

Beim Erstellen und Packen Sie eine Webanwendung, wird die WPP automatisch diese Dinge zu parametrisieren:

- Das Ziel IIS web Application-Pfad und den Namen.
- Alle Verbindungszeichenfolgen in Ihrem *"Web.config"* Datei.
- Verbindungszeichenfolgen für alle Datenbanken, die Sie hinzufügen, die **SQL packen/veröffentlichen** Registerkarte in den Eigenschaftenseiten des Projekts.

Beispielsweise würden Sie zum Erstellen und Verpacken der [Contact Manager](the-contact-manager-solution.md) beispiellösung ohne direkten Zugriff auf die Parametrisierung wird in keiner Weise, die WPP erzeugt dies *ContactManager.Mvc.SetParameters.xml* Datei:


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


In diesem Fall gilt Folgendes:

- Die **IIS-Webanwendungsname** -Parameter ist der IIS-Pfad, in dem Sie die Webanwendung bereitstellen möchten. Standardmäßig ist der Wert stammt aus dem **Web packen/veröffentlichen** Seite in den Eigenschaftenseiten des Projekts.
- Die **ApplicationServices-Web.config Verbindungszeichenfolge** Parameter aus generiert wurde eine **ConnectionStrings/hinzufügen** Element in der *"Web.config"* Datei. Es stellt die Verbindungszeichenfolge, die die Anwendung verwenden soll, wenden Sie sich an der Mitgliedschaftsdatenbank dar. Der Wert, der Sie angeben, werden hier in der bereitgestellten eingesetzt *"Web.config"* Datei. Standardmäßig ist der Wert stammt aus der vor der Bereitstellung *"Web.config"* Datei.

Die WPP parametrisiert auch diese Eigenschaften in das Bereitstellungspaket an, das sie generiert. Sie können Werte für diese Eigenschaften bereitstellen, wenn Sie das Bereitstellungspaket installieren. Wenn Sie das Paket manuell über IIS-Manager installieren, wie in beschrieben [Manuelles Installieren von Webpaketen](manually-installing-web-packages.md), der Installations-Assistenten aufgefordert, Werte für alle Parameter angeben. Bei der Installation des Pakets mithilfe der *. "Deploy.cmd"* Datei, wie in beschrieben [Bereitstellen von Webpaketen](deploying-web-packages.md), Web Deploy sieht dieser *"SetParameters.xml"* Datei Geben Sie die Parameterwerte. Sie können die Werte im Bearbeiten der *"SetParameters.xml"* -Datei manuell ein, oder passen Sie die Datei als Teil eines automatisierten Build & Deployment-Prozesses. Dieser Prozess wird weiter unten in diesem Thema ausführlicher beschrieben.

### <a name="custom-parameterization"></a>Benutzerdefinierte Parametrisierung

In komplexeren Bereitstellungsszenarien sollten Sie sich häufig um zusätzliche Eigenschaften zu parametrisieren, bevor Sie das Projekt bereitstellen. Im Allgemeinen sollten Sie parametrisieren, alle Eigenschaften und Einstellungen, die zwischen zielumgebungen variieren. Dazu gehören:

- Dienstendpunkte in der *"Web.config"* Datei.
- Einstellungen für Anwendungen in der *"Web.config"* Datei.
- Alle anderen deklarativen Eigenschaften, denen Sie möchten Benutzer aufgefordert, anzugeben.

Die einfachste Möglichkeit zum Parametrisieren von Rowseteigenschaften wird zum Hinzufügen einer *"Parameters.xml"* Datei in den Stammordner Ihres Webprojekts für die Anwendung. In der Projektmappe Contact Manager, z. B. das ContactManager.Mvc-Projekt enthält eine *"Parameters.xml"* Datei im Stammordner.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Wenn Sie diese Datei öffnen, sehen Sie, dass es sich um ein einzelnes enthält **Parameter** Eintrag. Der Eintrag verwendet eine XML Path Language (XPath)-Abfrage zu suchen und parametrisieren die Endpunkt-URL des Diensts ContactService Windows Communication Foundation (WCF) in der *"Web.config"* Datei.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


Zusätzlich zu parametrisieren die Endpunkt-URL in das Bereitstellungspaket an, fügt die WPP auch einen entsprechenden Eintrag aus, um die *"SetParameters.xml"* -Datei, die zusammen mit dem Bereitstellungspaket generiert wird.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


Wenn Sie das Bereitstellungspaket manuell installieren, fordert IIS-Manager Sie zur Adresse Dienstendpunkts zusammen mit den Eigenschaften, die automatisch parametrisiert wurden. Bei der Installation des Bereitstellungspakets mit der *. "Deploy.cmd"* -Datei, die Sie bearbeiten die *"SetParameters.xml"* Datei, geben Sie einen Wert für die Dienst-Endpunktadresse sowie Werte für die Eigenschaften, die automatisch parametrisiert wurden.

Ausführliche Informationen zum Erstellen einer *"Parameters.xml"* finden Sie unter [Vorgehensweise: Verwenden von Parametern zum Konfigurieren von Einstellungen bei der ein Bereitstellungspaket ist installiert](https://msdn.microsoft.com/library/ff398068.aspx). Die Prozedur mit dem Namen **Bereitstellungsparameter für Web.config-dateieinstellungen verwendet** enthält schrittweise Anleitungen.

## <a name="modifying-the-setparametersxml-file"></a>Ändern die Datei "SetParameters.xml"

Wenn Sie planen, das Web-Anwendungspaket manuell bereitstellen&#x2014;entweder durch Ausführen der *. "Deploy.cmd"* Datei oder durch Ausführen von MSDeploy.exe über die Befehlszeile&#x2014;nichts beenden Sie die manuell bearbeitet die  *"SetParameters.xml"* Datei vor der Bereitstellung. Wenn Sie eine unternehmensweite Lösung arbeiten, müssen Sie jedoch ein Web-Application-Paket als Teil eines größeren, automatisierten Build & Deployment-Prozesses bereitstellen. In diesem Szenario benötigen Sie Microsoft Build Engine (MSBuild) so ändern Sie die *"SetParameters.xml"* -Datei für Sie. Sie erreichen dies, indem Sie MSBuild **XmlPoke** Aufgabe.

Die [Contact Manager-beispiellösung](the-contact-manager-solution.md) veranschaulicht diesen Prozess. Die Codebeispiele, die Folgen haben bearbeitet wurde, um nur die Details anzuzeigen, die in diesem Beispiel relevant sind.

> [!NOTE]
> Eine umfassendere Übersicht des Projektmodells-Datei in die Projektmappe, und eine Einführung in benutzerdefinierte Projektdateien im Allgemeinen finden Sie unter [Grundlegendes zur Projektdatei](understanding-the-project-file.md) und [Verständnis des Prozesses erstellen](understanding-the-build-process.md).


Zunächst werden die Parameterwerte von Interesse sind als Eigenschaften in der Umgebung spezifischen Projektdatei definiert (z. B. *Env-Dev.proj*).


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> Anleitungen zum Anpassen der umgebungsspezifischen Projektdateien für Ihre eigenen serverumgebungen finden Sie unter [Konfigurieren von Bereitstellungseigenschaften für eine Zielumgebung](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Als Nächstes die *Publish.proj* -Datei importiert, diese Eigenschaften. Da jede *"SetParameters.xml"* Datei zugeordnet ist eine *. "Deploy.cmd"* -Datei, und wir werden die Projektdatei, die jeweils aufrufen soll *. "Deploy.cmd"* Datei des Projekts erstellt die Datei eine MSBuild *Element* für jede *. "Deploy.cmd"* Datei, und definiert die Eigenschaften von Interesse sind, als *Elementmetadaten*.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


In diesem Fall gilt Folgendes:

- Die **ParametersXml** Metadatenwert gibt die Position von der *"SetParameters.xml"* Datei.
- Die **IisWebAppName** Wert ist der IIS-Pfad zu dem Sie die Webanwendung bereitstellen möchten.
- Die **MembershipDBConnectionString** Wert ist die Verbindungszeichenfolge für die Mitgliedschaftsdatenbank und die **MembershipDBConnectionName** Wert ist die **Namen** Attribut des entsprechenden Parameters in der *"SetParameters.xml"* Datei.
- Die **ServiceEndpointValue** Wert ist die Endpunktadresse für den WCF-Dienst auf dem Zielserver, und die **ServiceEndpointParamName** Wert ist das Namensattribut des entsprechenden Parameters in die *"SetParameters.xml"* Datei.

In der *Publish.proj* -Datei, die **PublishWebPackages** verwendet als Ziel der **XmlPoke** Aufgabe so ändern Sie diese Werte in der *"SetParameters.xml"* Datei.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


Sie werden feststellen, dass jedes **XmlPoke** Task gibt vier Attributwerte:

- Die **XmlInputPath** -Attribut weist der Aufgabe, wo die Datei zu finden, Sie ändern möchten.
- Die **Abfrage** -Attribut ist eine XPath-Abfrage, die den XML-Knoten identifiziert, Sie ändern möchten.
- Die **Wert** -Attribut ist der neue Wert in der ausgewählten XML-Knoten eingefügt werden soll.
- Die **Bedingung** Attribut gibt die Kriterien, die auf dem die Aufgabe ausgeführt werden sollen oder nicht ausgeführt werden. In diesen Fällen ist die Bedingung wird sichergestellt, dass Sie nicht, zum Einfügen eines Werts null oder leer sein, in versuchen der *"SetParameters.xml"* Datei.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschrieben die Rolle der *"SetParameters.xml"* Datei, und erläutert, wie es bei der Erstellung eines Webanwendungsprojekts generiert wird. Er erläutert, wie Sie durch Hinzufügen zusätzliche Einstellungen parametrisieren können eine *"Parameters.xml"* Datei zum Projekt. Es wurde außerdem beschrieben, wie Sie ändern können die *"SetParameters.xml"* Datei als Teil eines größeren, automatisierten Buildprozesses, mit der **XmlPoke** Task in den Projektdateien.

Im nächsten Thema, [Bereitstellen von Webpaketen](deploying-web-packages.md), beschreibt, wie Sie eine Web-Paket entweder mit bereitstellen können die *. "Deploy.cmd"* Datei oder mithilfe von MSDeploy.exe-Befehle direkt. In beiden Fällen können Sie angeben Ihrer *"SetParameters.xml"* Datei als Bereitstellungsparameter angeben.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zum Erstellen von Webpaketen, finden Sie unter [erstellen und Verpacken von Webanwendungsprojekten](building-and-packaging-web-application-projects.md). Anleitung für das tatsächlich ein Webpaket bereitgestellt werden, finden Sie unter [Bereitstellen von Webpaketen](deploying-web-packages.md). Eine schrittweise Anleitung zum Erstellen einer *"Parameters.xml"* finden Sie unter [Vorgehensweise: Verwenden von Parametern zum Konfigurieren von Einstellungen bei der ein Bereitstellungspaket ist installiert](https://msdn.microsoft.com/library/ff398068.aspx).

Weitere allgemeine Informationen zur Parametrisierung in Web Deploy, finden Sie unter [Bereitstellen von Web-Parametrisierung in Aktion](https://go.microsoft.com/?linkid=9805119) (Blogbeitrag).

> [!div class="step-by-step"]
> [Zurück](building-and-packaging-web-application-projects.md)
> [Weiter](deploying-web-packages.md)
