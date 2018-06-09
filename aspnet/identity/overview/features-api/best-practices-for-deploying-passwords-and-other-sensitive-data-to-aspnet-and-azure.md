---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Bewährte Methoden für die Bereitstellung von Kennwörtern und andere sensible Daten für ASP.NET und Azure App Service | Microsoft Docs
author: Rick-Anderson
description: In diesem Lernprogramm wird gezeigt, wie der Code sicher zu speichern und Zugriff auf sichere Informationen. Der wichtigste Punkt ist, dass Sie niemals Kennwörter oder andere Sen speichern...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2015
ms.topic: article
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 995d9a088e3095f36a01d2adb19ec08e6a6d1b3e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "28033020"
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Bewährte Methoden für die Bereitstellung von Kennwörtern und andere sensible Daten für ASP.NET und Azure App Service
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

> In diesem Lernprogramm wird gezeigt, wie der Code sicher zu speichern und Zugriff auf sichere Informationen. Der wichtigste Punkt ist, sollten Sie niemals Kennwörter oder andere vertraulichen Daten im Quellcode speichern und Produktion geheime Schlüssel verwenden, im Modus für Entwicklung und Tests keine.
> 
> Im Beispielcode wird eine einfache WebJob-Konsolen-app und eine ASP.NET MVC-app, die Zugriff auf eine Datenbank Verbindung Zeichenfolge Kennwort Twilio, Google und SendGrid sichere Schlüssel benötigt.
> 
> Lokale wird Einstellungen und PHP ebenfalls erwähnt.


- [Arbeiten mit Kennwörtern in der Entwicklungsumgebung](#pwd)
- [Arbeiten mit Verbindungszeichenfolgen in der Entwicklungsumgebung](#con)
- [WebJobs-Konsolen-apps](#wj)
- [Geheime Schlüssel in Azure bereitstellen.](#da)
- [Hinweise für lokale und PHP](#not)
- [Additional Resources](#addRes) (Zusätzliche MSBuild-Ressourcen)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Arbeiten mit Kennwörtern in der Entwicklungsumgebung

Tutorials erfahren häufig vertrauliche Daten im Quellcode hoffentlich mit einer Einschränkung, dass sensible Daten nie im Quellcode gespeichert werden sollten. Z. B. Mein [ASP.NET MVC 5-Anwendung mit SMS und e-Mail-2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) Lernprogramm erfahren Sie Folgendes in die *"Web.config"* Datei:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

Die *"Web.config"* Datei Quellcode ist, damit dieser geheimen Schlüssel nie in dieser Datei gespeichert werden sollen. Glücklicherweise der `<appSettings>` Element verfügt über eine `file` -Attribut, Ihnen die ermöglicht Angabe eine externe Datei, die vertrauliche app-Konfigurationseinstellungen enthält. Solange die externe Datei nicht in Ihrer ursprünglichen Struktur aktiviert ist, können Sie Ihre vertraulichen Daten in eine externe Datei verschieben. Im folgenden Markup, die Datei z. B. *AppSettingsSecrets.config* enthält alle geheime app-Schlüssel:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Das Markup in der externen Datei (*AppSettingsSecrets.config* in diesem Beispiel), wird das gleiche Markup wurde gefunden, der *"Web.config"* Datei:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

Die ASP.NET-Laufzeit führt den Inhalt der externen Datei mit dem Markup im &lt;"appSettings"&gt; Element. Die Common Language Runtime ignoriert das Dateiattribut ", wenn die angegebene Datei nicht gefunden werden kann.

> [!WARNING]
> Sicherheit – fügen Sie keine Ihrer *geheime Schlüssel config* -Datei in Ihrem Projekt, oder checken Sie es in die quellcodeverwaltung. Standardmäßig legt Visual Studio die `Build Action` zu `Content`, was bedeutet, dass die Datei bereitgestellt wird. Weitere Informationen finden Sie unter [Warum nicht alle Dateien im Projekt-Ordner bereitgestellt?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Obwohl Sie können eine beliebige Erweiterung für die *geheime Schlüssel config* Datei, es wird empfohlen, ihn zu *config*, wie die Konfigurationsdateien von IIS nicht verarbeitet werden. Beachten Sie auch, dass die *AppSettingsSecrets.config* Datei ist zwei Directory Ebenen oberhalb von der *"Web.config"* Datei, damit es vollständig außerhalb des dem Projektmappenverzeichnis liegt. Durch Verschieben der Datei aus dem Projektmappenverzeichnis &quot;Git hinzufügen \* &quot; wird nicht an Ihr Repository hinzufügen.


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Arbeiten mit Verbindungszeichenfolgen in der Entwicklungsumgebung

Visual Studio erstellt neue ASP.NET-Projekte, mit denen [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). LocalDB, die speziell für die Entwicklungsumgebung erstellt wurde. Es sind ein Kennwort erforderlich, daher brauchen Sie gar nichts Unternehmen, um zu verhindern, dass Kennwörter in Ihren Quellcode überprüft wird. Einige Entwicklungsteams verwenden Sie die Vollversion von SQL Server (oder andere DBMS), die ein Kennwort erfordern.

Sie können die `configSource` Attribut zum Ersetzen des gesamtes `<connectionStrings>` Markup. Im Gegensatz zu den `<appSettings>` `file` -Attribut, das das Markup, führt der `configSource` -Attribut ersetzt das Markup. Das folgende Markup zeigt die `configSource` Attribut in der *"Web.config"* Datei:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Bei Verwendung der `configSource` -Attribut an, wie oben gezeigt, um die Verbindungszeichenfolgen in eine externe Datei zu verschieben und Visual Studio eine neue Website erstellen, er kann nicht erkennen Sie eine Datenbank, und rufen Sie nicht die Möglichkeit, konfigurieren die Datenbank bei der Sie Pu Veröffentlichen in Azure aus Visual Studio. Bei Verwendung der `configSource` -Attribut, mithilfe von PowerShell zum Erstellen und Bereitstellen Ihrer Website und die Datenbank, oder Sie können Erstellen der Website und die Datenbank in das Portal vor dem veröffentlichen. Die [neu AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) Skript erstellt eine neue Website und der Datenbank.


> [!WARNING]
> Sicherheit – im Gegensatz zu den *AppSettingsSecrets.config* -Datei muss die externe Verbindung Zeichenfolgen-Datei im gleichen Verzeichnis wie der Stamm *"Web.config"* Datei, daher Sie Vorsichtsmaßnahmen treffen, um sicherzustellen, dass Sie müssen Überprüfen Sie nicht in Ihrem Quellrepository.


> [!NOTE]
> **Sicherheitswarnung für geheime Schlüssel Datei:** ist ein bewährtes Produktion geheime Schlüssel in Test- und nicht zu verwenden. Produktion Kennwörter in Test- oder Entwicklungsumgebung mit Speicherverlusten dieser geheimen Schlüssel.


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>WebJobs-Konsolen-apps

Die *"App.config"* vom eine Konsolen-app verwendete Datei relative Pfade unterstützt, aber absolute Pfade werden unterstützt. Einen absoluten Pfad können Sie um Ihre vertraulichen Daten aus dem Projektverzeichnis zu verschieben. Das folgende Markup zeigt der geheimen Schlüssel in der *C:\secrets\AppSettingsSecrets.config* Datei und nicht vertrauliche Daten in der *"App.config"* Datei.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Geheime Schlüssel in Azure bereitstellen.

Wenn Sie Ihre Web-app in Azure, Bereitstellen der *AppSettingsSecrets.config* (das gewünschte) Datei wird nicht bereitgestellt werden. Konnte zur der [Azure-Verwaltungsportal](https://azure.microsoft.com/services/management-portal/) und richten Sie sie manuell, um dies vorzunehmen:

1. Wechseln Sie zu [ https://portal.azure.com ](https://portal.azure.com), und melden Sie sich mit Ihrem Azure-Anmeldeinformationen.
2. Klicken Sie auf **Durchsuchen &gt; Web-Apps**, klicken Sie dann auf den Namen des Web-app.
3. Klicken Sie auf **alle Einstellungen &gt; Anwendungseinstellungen**.

Der **Anwendungseinstellungen** und **Verbindungszeichenfolge** Werte überschreiben die gleichen Einstellungen in der *"Web.config"* Datei. In unserem Beispiel wir nicht diese Einstellungen in Azure bereitstellen, aber das wäre, wenn diese Schlüssel der *"Web.config"* Datei, die die Einstellungen im Portal angezeigt würde haben Vorrang vor.

Eine bewährte Methode besteht darin, befolgen eine [DevOps-Workflow](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) und [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (oder einem anderen Framework, z. B. [Chef](http://www.opscode.com/chef/) oder [Puppet](http://puppetlabs.com/puppet/what-is-puppet)), diese Einstellungswerte in Azure zu automatisieren. Das folgende PowerShell-Skript verwendet [Export CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) So exportieren Sie die verschlüsselten geheimen Schlüssel auf den Datenträger:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

In das obige Skript ist "Name" der Name des geheimen Schlüssels, z. B. "&quot;FB\_AppSecret&quot; oder"TwitterSecret". Sie können die Datei ".credential" unter Verwendung des Skripts im Browser anzeigen. Der Codeausschnitt unten testet die Anmeldeinformationen-Dateien und legt die geheimen Schlüssel für die benannte Web-app:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Sicherheit – keine Kennwörter oder andere geheime Schlüssel in das PowerShell-Skript, das auf diese Weise entspricht dies der Fall ist der Zweck der Verwendung von PowerShell-Skript zum Bereitstellen von sensiblen Daten enthalten. Die [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) Cmdlet bietet einen sicheren Mechanismus zum Abrufen eines Kennworts. Eine UI-Eingabe kann zu verhindern, dass ein Kennwort zu gelangen.


### <a name="deploying-db-connection-strings"></a>Bereitstellen von DB-Verbindungszeichenfolgen

DB-Verbindungszeichenfolgen werden an den Appeinstellungen auf ähnliche Weise behandelt. Wenn Sie Ihre Web-app aus Visual Studio bereitstellen, wird die Verbindungszeichenfolge für Sie konfiguriert werden. Dies ist im Portal zu überprüfen. Die empfohlene Methode zum Festlegen der Verbindungszeichenfolge wird mit PowerShell. Ein Beispiel für ein PowerShell-Skript die eine Website und die Datenbank erstellt und wird die Verbindungszeichenfolge auf der Website herunterladen [neu AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) aus der [-Skript für Azure-Bibliothek](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Hinweise für PHP

Seit der Schlüssel-/ Wertpaare für beide **Anwendungseinstellungen** und **Verbindungszeichenfolgen** sind gespeichert in Umgebungsvariablen in Azure App Service, Entwickler, die alle Web-app-Frameworks (z. B. PHP) können problemlos verwenden Diese Werte abrufen. Finden Sie unter der Stefan Schackow [Windows Azure-Websites: wie Anwendungszeichenfolgen und Verbindung Zeichenfolgen können](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) Blogbeitrag, die in einen PHP-Ausschnitt zum Lesen von Anwendungseinstellungen und Verbindungszeichenfolgen dargestellt wird.

## <a name="notes-for-on-premises-servers"></a>Anmerkungen zu dieser lokalen Server

Wenn Sie zu einer lokalen Webservern bereitstellen, helfen Sie sichere Kennwörter durch [Verschlüsseln der Konfigurationsabschnitte Konfigurationsdateien](https://msdn.microsoft.com/library/ff647398.aspx). Als Alternative können Sie den gleichen Ansatz für Azure-Websites empfohlen: Beibehalten der entwicklungseinstellungen in Konfigurationsdateien und verwendet Sie Werte für Umgebungsvariablen für die Produktion. In diesem Fall jedoch, Sie schreiben müssen, Anwendungscode für die Funktionalität, die in Azure-Websites automatisch ausgeführt wird: Abrufen von Einstellungen aus der Umgebungsvariablen und anhand dieser Werte anstelle Dienstkonfigurations-dateieinstellungen bzw. Dienstkonfigurations-dateieinstellungen bei Umgebungsvariablen werden nicht gefunden.

<a id="addRes"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

Für ein Beispiel für ein PowerShell-Skript, das erstellt wird, eine Web-app + Datenbank, legt fest, die Verbindungszeichenfolge + app-Einstellungen, Download [neu AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) aus der [-Skript für Azure-Bibliothek](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Finden Sie unter der Stefan Schackow [Windows Azure-Websites: wie Anwendungszeichenfolgen und Verbindung Zeichenfolgen arbeiten](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Besonderer Dank gilt Barry Dorrans ( [ @blowdart ](https://twitter.com/blowdart) ) und Carlos Farre für die Überprüfung.
