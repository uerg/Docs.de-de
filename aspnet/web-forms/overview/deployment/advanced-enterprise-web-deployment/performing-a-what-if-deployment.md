---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Ausführen eines Was-wäre, wenn Bereitstellung | Microsoft-Dokumentation
author: jrjlee
description: In diesem Thema wird beschrieben, wie "Was geschieht, wenn" ausführen (oder simulierte)-Bereitstellungen mithilfe der Internetinformationsdienste (Internet Information Services, IIS)-Webbereitstellungstool (Web Deploy) und V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: f50053c3b231a1d174df51ce7aa4c6013192bca8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378098"
---
<a name="performing-a-what-if-deployment"></a>Durchführen einer Bereitstellung "What If"
====================
durch [Jason Lee](https://github.com/jrjlee)

[PDF herunterladen](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> In diesem Thema wird beschrieben, wie "Was-wäre-wenn" ausführen (oder simulierte) mit dem Internet Information Services (IIS)-Webbereitstellungstool (Web Deploy) und VSDBCMD Bereitstellungen. Dadurch können Sie die Auswirkungen Ihrer Bereitstellungslogik in einer bestimmten zielumgebung zu bestimmen, bevor Sie tatsächlich Ihre Anwendung bereitstellen.


In diesem Thema ist Teil einer Reihe von Tutorials, die auf der Basis der bereitstellungsanforderungen Enterprise ein fiktives Unternehmen, die mit dem Namen Fabrikam, Inc. Dieser tutorialreihe verwendet eine beispiellösung&#x2014;der [Contact Manager-Lösung](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;zur Darstellung einer Webanwendung mit einem realistischen Maß an Komplexität, einschließlich einer ASP.NET MVC 3-Anwendung, eine Windows-Kommunikation Foundation (WCF)-Dienst und ein Datenbankprojekt.

Die Methode für die Bereitstellung das Kernstück des in diesen Tutorials basiert auf den geteilten Projekt Dateiansatz beschrieben, die [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in dem der Build & Deployment-Prozess von gesteuert wird zwei Projektdateien&#x2014;eine die Buildanweisungen, die für jede zielumgebung, und enthält umgebungsspezifische Einstellungen für Build & Deployment gelten enthält. Zur Erstellungszeit wird die umgebungsspezifischen-Projektdatei in die Unabhängigkeit von der Umgebung Projektdatei, um einen vollständigen Satz von einrichtungsanweisungen bilden zusammengeführt.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Ausführen einer "Was-wäre-wenn" Bereitstellung für Webpakete

Web Deploy enthält Funktionen, die Sie Bereitstellungen in "what if" durchführen kann (oder Testversion) Modus. Wenn Sie Elemente in "what if"-Modus bereitstellen, generiert das Web Deploy eine Protokolldatei, mussten Sie die Bereitstellung ausgeführt, wobei jedoch eigentlich nichts ändert, auf dem Zielserver. Überprüfen die Protokolldatei kann Ihnen helfen, welche Auswirkungen zu verstehen, die Bereitstellung auf dem Zielserver, insbesondere muss:

- Was wird hinzugefügt.
- Was wird aktualisiert.
- Welche gelöscht werden.

Da eine "Was-wäre-wenn" Bereitstellung tatsächlich Änderungen nicht auf dem Zielserver, was immer nicht möglich ist, ist die Vorhersage, ob eine Bereitstellung erfolgreich ist, wird.

Siehe [Bereitstellen von Webpaketen](../web-deployment-in-the-enterprise/deploying-web-packages.md), Bereitstellen von Webpaketen, die auf zwei Arten mit Web Deploy können&#x2014;mithilfe des Befehlszeilenprogramms MSDeploy.exe direkt oder durch Ausführen der *. "Deploy.cmd"* Datei der Buildprozess generiert.

Wenn Sie MSDeploy.exe direkt verwenden, können Sie eine Bereitstellung "Was-wäre-wenn" ausführen, durch das Hinzufügen der **– Whatif** Flag, um den Befehl. Z. B. um auszuwerten, was passieren würde, wenn Sie das Paket ContactManager.Mvc.zip in einer Stagingumgebung bereitgestellt, sieht der MSDeploy-Befehl folgendermaßen aus:


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


Wenn Sie mit den Ergebnissen der Bereitstellungskonfiguration "Was-wäre-wenn" zufrieden sind, können Sie entfernen die **– Whatif** Flag, um eine live-Bereitstellung auszuführen.

> [!NOTE]
> Weitere Informationen zu den Befehlszeilenoptionen für MSDeploy.exe finden Sie unter [Web Deploy-Vorgang Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx).


Bei Verwendung der *. "Deploy.cmd"* -Datei können Sie eine Bereitstellung "Was-wäre-wenn" ausführen, indem die einschließlich der **/t /** flag (Testversion) moduskennzeichnung anstelle von der **/y** -Flag ("yes" oder Update-Modus) in der Befehl. Beispielsweise, um auszuwerten, was passieren würde, wenn Sie durch Ausführen des Pakets ContactManager.Mvc.zip bereitgestellt der *. "Deploy.cmd"* -Datei des Befehls sollte diesem ähneln:


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


Wenn Sie mit den Ergebnissen der Bereitstellungskonfiguration "Testmodus" zufrieden sind, können Sie ersetzen die **/t /** flag mit einer **/y** Flag zum Ausführen einer aktiven Bereitstellung:


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> Weitere Informationen zu Befehlszeilenoptionen für *. "Deploy.cmd"* finden Sie unter [Vorgehensweise: Installieren eines Bereitstellung Pakets mithilfe der Datei "Deploy.cmd"](https://msdn.microsoft.com/library/ff356104.aspx). Wenn das Ausführen der *. "Deploy.cmd"* Datei ohne Angabe von Flags, die Eingabeaufforderung zeigt eine Liste der verfügbaren Flags.


## <a name="performing-a-what-if-deployment-for-databases"></a>Ausführen einer "Was-wäre-wenn" Bereitstellung für Datenbanken

In diesem Abschnitt wird davon ausgegangen, dass Sie das Dienstprogramm VSDBCMD nutzen, inkrementelle, Schema-basierte Datenbank-Bereitstellung durchzuführen. Dieser Ansatz wird ausführlicher beschrieben [Bereitstellen von Datenbankprojekten](../web-deployment-in-the-enterprise/deploying-database-projects.md). Es wird empfohlen, dass Sie sich vertraut mit diesem Thema machen, bevor Sie die hier beschriebenen Konzepte gelten.

Bei Verwendung von VSDBCMD in **bereitstellen** -Modus können Sie die **dd /** (oder **/DeployToDatabase**) flag, das steuert, ob VSDBCMD tatsächlich stellt die Datenbank bereit oder nur generiert ein Skript für die Bereitstellung. Wenn Sie eine DBSCHEMA-Datei bereitstellen, ist dies das Verhalten auf:

- Bei Angabe von **/dd+** oder **dd /**, VSDBCMD wird ein Bereitstellungsskript generieren und Bereitstellen der Datenbank.
- Bei Angabe von **/dd-** oder ohne den Schalter, VSDBCMD wird nur ein Bereitstellungsskript generiert.

> [!NOTE]
> Wenn Sie eine Datei DeployManifest anstelle einer DBSCHEMA-Datei, das Verhalten der bereitstellen, die **dd /** Schalter ist wesentlich komplizierter. Im Wesentlichen VSDBCMD ignoriert den Wert des der **dd /** wechseln, wenn die DEPLOYMANIFEST-Datei enthält eine **DeployToDatabase** Element mit einem Wert von **"true"**. [Bereitstellen von Datenbankprojekten](../web-deployment-in-the-enterprise/deploying-database-projects.md) wird dieses Verhalten vollständig beschrieben.


Beispielsweise, um das Generieren eines Bereitstellungsskripts für die **ContactManager** Datenbank ohne Sie tatsächlich Bereitstellen der Datenbank, die den VSDBCMD-Befehl sollte diesem ähneln:


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD ist ein Bereitstellungstool differenziellen Datenbank- und das Bereitstellungsskript wird als solche dynamisch generiert, um alle der SQL-Befehle erforderlich, die die aktuelle Datenbank zu aktualisieren, falls, die dem angegebenen Schema vorhanden enthalten. Überprüfen das Bereitstellungsskript ist eine gute Möglichkeit, um zu bestimmen, was die Bereitstellung auswirken wird in der aktuellen Datenbank und die darin enthaltenen Daten. Sie sollten z. B. um zu bestimmen:

- Gibt an, ob alle vorhandenen Tabellen entfernt werden, und gibt an, ob dies zu Datenverlust führt.
- Gibt an, ob die Reihenfolge der Vorgänge ein Risiko des Datenverlusts, z. B. enthält, wenn Sie teilen oder Zusammenführen von Tabellen.

Wenn Sie mit dem Bereitstellungsskript zufrieden sind, können Sie die VSDBCMD mit Wiederholen einer **/dd+** Flag, um die Änderungen zu übernehmen. Alternativ können Sie das Bereitstellungsskript aus, um Ihre Anforderungen erfüllen, und führen sie dann manuell auf dem Datenbankserver bearbeiten.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Die Integration von "What If"-Funktionalität in benutzerdefinierten Projektdateien

In komplexeren Bereitstellungsszenarien, sollten Sie zu eine benutzerdefinierte Microsoft Build Engine (MSBuild)-Projektdatei zu verwenden, um Ihre Build- und bereitstellungs-Logik zu kapseln, wie in beschrieben [Grundlegendes zur Projektdatei](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Z. B. in der [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) beispiellösung, die *Publish.proj* Datei:

- Erstellt die Lösung.
- Wird die Web Deploy-Packen und Bereitstellen der Anwendung ContactManager.Mvc verwendet.
- Wird die Web Deploy-Packen und Bereitstellen der Anwendung ContactManager.Service verwendet.
- Stellt die **ContactManager** Datenbank.

Wenn Sie die Bereitstellung von mehreren Webpakete bzw. Datenbanken in einen Prozess Schritt für Schritt auf diese Weise integrieren, sollten Sie auch die Möglichkeit, die gesamte Bereitstellung in einem "what if"-Modus ausführen.

Die *Publish.proj* Datei veranschaulicht, wie Sie dies tun können. Zunächst müssen Sie eine Eigenschaft zum Speichern des Werts "Was-wäre-wenn" zu erstellen:


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


In diesem Fall haben Sie eine Eigenschaft mit dem Namen erstellt **"WhatIf"** hat den Standardwert des **"false"**. Benutzer können diesen Wert durch Festlegen der Eigenschaft zu überschreiben **"true"** in einen Befehlszeilenparameter, wie Sie gleich sehen werden.

Der nächste Schritt besteht, eine beliebige Web Deploy zu parametrisieren und VSDBCMD Befehle, sodass die Flags enthalten die **"WhatIf"** -Eigenschaftswert. Z. B. das nächste Ziel (stammt aus der *Publish.proj* Datei, und vereinfacht) ausgeführt wird die *. "Deploy.cmd"* Datei ein Webpaket bereitgestellt. Standardmäßig enthält der Befehl eine **/y** Switch ("yes" oder Update-Modus). Wenn **"WhatIf"** nastaven NA hodnotu **"true"**, dies wird durch ersetzt eine **/t /** Switch (Test- oder "Was-wäre-wenn"-Modus).


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


Auf ähnliche Weise verwendet das nächste Ziel das VSDBCMD-Dienstprogramm zum Bereitstellen einer Datenbank. Standardmäßig eine **dd /** Switch ist nicht enthalten. Dies bedeutet, dass VSDBCMD ein Bereitstellungsskript generiert, aber die Datenbank nicht bereitgestellt werden&#x2014;in anderen Worten: ein "Was-wäre-wenn"-Szenario. Wenn die **"WhatIf"** Eigenschaft ist nicht festgelegt, um **"true"**, **dd /** Schalter hinzugefügt wird und VSDBCMD wird die Datenbank bereitstellen.


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


Sie können den gleichen Ansatz verwenden, parametrisieren Sie alle relevanten Befehle in Ihrer Projektdatei hinzu. Wenn Sie eine "Was-wäre-wenn"-Bereitstellung ausführen möchten, können Sie dann geben Sie einfach eine **"WhatIf"** Eigenschaftswert von der Befehlszeile aus:


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


Auf diese Weise können Sie eine "Was-wäre-wenn" Bereitstellung für alle Projektkomponenten in einem einzigen Schritt ausführen.

## <a name="conclusion"></a>Schlussbemerkung

In diesem Thema beschrieben, wie "Was-wäre-wenn"-Bereitstellungen mithilfe von Web Deploy, VSDBCMD und MSBuild ausführen. Eine "Was-wäre-wenn"-Bereitstellung können Sie die Auswirkungen der vorgeschlagenen Bereitstellung ausgewertet, bevor Sie Änderungen in der zielumgebung tatsächlich vornehmen.

## <a name="further-reading"></a>Weiterführende Themen

Weitere Informationen zu Web Deploy-Befehlszeilensyntax, finden Sie unter [Web Deploy-Vorgang Settings](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Anleitungen zu den Befehlszeilenoptionen, die bei der Verwendung der *. "Deploy.cmd"* finden Sie unter [Vorgehensweise: Installieren eines Bereitstellung Pakets mithilfe der Datei "Deploy.cmd"](https://msdn.microsoft.com/library/ff356104.aspx). Anweisungen VSDBCMD Befehlszeilensyntax, finden Sie unter [Befehlszeilenreferenz für VSDBCMD. EXE-Datei ("Bereitstellung" und "Schema importieren")](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Zurück](advanced-enterprise-web-deployment.md)
> [Weiter](customizing-database-deployments-for-multiple-environments.md)
