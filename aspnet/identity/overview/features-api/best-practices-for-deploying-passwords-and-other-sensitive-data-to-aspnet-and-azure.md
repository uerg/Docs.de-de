---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Bewährte Methoden für die Bereitstellung von Kennwörtern und anderen vertraulichen Daten in ASP.NET und Azure App Service | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial wird gezeigt, wie der Code sicher zu speichern und auf sichere Informationen zugreifen. Der wichtigste Punkt ist, dass Sie niemals Kennwörter oder andere Sen speichern sollten...
ms.author: riande
ms.date: 05/21/2015
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 8b5d6bf9fad72218341e4e0b90144da01abea3aa
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577536"
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Bewährte Methoden für die Bereitstellung von Kennwörtern und anderen vertraulichen Daten in ASP.NET und Azure App Service
====================
durch [Rick Anderson]((https://twitter.com/RickAndMSFT))

> In diesem Tutorial wird gezeigt, wie der Code sicher zu speichern und auf sichere Informationen zugreifen. Der wichtigste Punkt ist, sollten Sie niemals Kennwörter oder andere sensiblen Daten im Quellcode speichern, und Sie sollte nicht produktionsgeheimnisse in Entwicklungs- und Test-Modus verwenden.
> 
> Der Beispielcode ist eine einfache WebJob-Konsolen-app und einer ASP.NET MVC-Anwendung, die Zugriff auf eine Datenbank Zeichenfolge Kennwort, Twilio, Google und SendGrid sicher Verbindungsschlüssel benötigt.
> 
> Lokale wird Einstellungen und PHP ebenfalls erwähnt.


- [Arbeiten mit Kennwörtern in der Entwicklungsumgebung](#pwd)
- [Arbeiten mit Verbindungszeichenfolgen in der Entwicklungsumgebung](#con)
- [WebJobs-Konsolen-apps](#wj)
- [Bereitstellen von Geheimnissen in Azure](#da)
- [Hinweise für die lokale und PHP](#not)
- [Additional Resources](#addRes) (Zusätzliche MSBuild-Ressourcen)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Arbeiten mit Kennwörtern in der Entwicklungsumgebung

Häufig Lernprogrammen sensible Daten im Quellcode hoffentlich mit Vorsicht zu genießen, dass vertrauliche Daten niemals in Quellcode gespeichert werden sollten. Z. B. meine [ASP.NET MVC 5-app mit SMS und e-Mail-2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) Lernprogramm wird Folgendes in die *"Web.config"* Datei:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

Die *"Web.config"* Datei Quellcode ist, damit diese Geheimnisse nicht in dieser Datei gespeichert werden sollen. Glücklicherweise die `<appSettings>` Element verfügt über eine `file` -Attribut, das können Sie eine externe Datei angeben, die vertrauliche app-Konfigurationseinstellungen enthält. Sie können alle Ihre Geheimnisse in eine externe Datei verschieben, solange die externe Datei in Ihre Quellstruktur nicht aktiviert ist. Im folgenden Markup, die Datei z. B. *AppSettingsSecrets.config* enthält alle app-Geheimnisse:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

Das Markup in der externen Datei (*AppSettingsSecrets.config* in diesem Beispiel), ist das gleiche Markup finden Sie der *"Web.config"* Datei:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

Die ASP.NET-Laufzeit führt den Inhalt der externen Datei mit dem Markup im &lt;"appSettings"&gt; Element. Die Laufzeit wird das Dateiattribut ignoriert, wenn die angegebene Datei nicht gefunden werden kann.

> [!WARNING]
> Sicherheit – fügen Sie keine Ihrer *Geheimnisse config* Datei zu Ihrem Projekt, oder checken Sie es in die quellcodeverwaltung. Standardmäßig legt Visual Studio die `Build Action` zu `Content`, was bedeutet, dass die Datei bereitgestellt wird. Weitere Informationen finden Sie unter [Warum nicht alle Dateien im Projektordner bereitgestellt?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Sie können zwar eine beliebige Erweiterung für verwenden die *Geheimnisse config* -Datei, es wird empfohlen, die sie behalten *config*, wie die Konfigurationsdateien von IIS nicht verarbeitet werden. Beachten Sie auch, dass die *AppSettingsSecrets.config* Datei ist zwei Directory Ebenen oberhalb von der *"Web.config"* Datei, sodass sie vollständig aus dem Projektmappenverzeichnis ist. Durch das Verschieben der Datei aus dem Projektmappenverzeichnis &quot;Git hinzufügen \* &quot; wird nicht in Ihrem Repository hinzufügen.


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Arbeiten mit Verbindungszeichenfolgen in der Entwicklungsumgebung

Visual Studio erstellt die neue ASP.NET-Projekten, mit denen [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). LocalDB, die speziell für die Entwicklungsumgebung erstellt wurde. Es nicht erforderlich, ein Kennwort, aus diesem Grund nicht müssen Sie nichts tun, um zu verhindern, dass Geheimnisse in Ihren Quellcode überprüft wird. Einige Entwicklungsteams verwenden Sie den Vollversionen von SQL Server (oder andere DBMS), die ein Kennwort erfordern.

Sie können die `configSource` Attribut, um die gesamte ersetzen `<connectionStrings>` Markup. Im Gegensatz zu den `<appSettings>` `file` -Attribut, das das Markup führt die `configSource` -Attribut ersetzt das Markup. Das folgende Markup zeigt die `configSource` -Attribut in der *"Web.config"* Datei:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Bei Verwendung der `configSource` -Attribut an, wie oben gezeigt, um die Verbindungszeichenfolgen in eine externe Datei zu verschieben und Visual Studio eine neue Website erstellen, sie nicht erkennen, verwenden Sie eine Datenbank, und Sie erhalten nicht die Möglichkeit, konfigurieren die Datenbank bei der Sie Pu Veröffentlichen in Azure aus Visual Studio. Bei Verwendung der `configSource` -Attribut, Sie können PowerShell verwenden, erstellen und Bereitstellen Ihrer Website und die Datenbank, oder Sie können erstellen die Website und die Datenbank im Portal vor der Veröffentlichung. Die [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) Skript erstellt eine neue Website und die Datenbank.


> [!WARNING]
> Sicherheit – im Gegensatz zu den *AppSettingsSecrets.config* -Datei muss die externe Verbindung für Zeichenfolgen-Datei im gleichen Verzeichnis als Stamm *"Web.config"* -Datei, müssen Sie Vorsichtsmaßnahmen ergreifen, um Sie sicherstellen, dass Überprüfen Sie nicht in Ihrem Quellrepository.


> [!NOTE]
> **Sicherheitswarnung auf die Datei mit Geheimnissen:** eine bewährte Methode ist keine produktionsgeheimnisse in Test- und entwicklungsumgebungen verwendet. Produktions-Kennwörtern in Test- oder Entwicklungsumgebung mit Speicherverlusten dieser geheimen Schlüssel.


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>WebJobs-Konsolen-apps

Die *"App.config"* Datei ein, die eine Konsolen-app unterstützt keine relativen Pfade, aber absolute Pfade werden unterstützt. Sie können einen absoluten Pfad verwenden, auf um Ihre Geheimnisse aus dem Projektverzeichnis zu verschieben. Das folgende Markup zeigt die Geheimnisse in die *C:\secrets\AppSettingsSecrets.config* -Datei und nicht sensible Daten in die *"App.config"* Datei.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Bereitstellen von Geheimnissen in Azure

Wenn Sie Ihre Web-app in Azure bereitstellen der *AppSettingsSecrets.config* Datei nicht bereitgestellt. (das ist, was Sie möchten). Könnten Sie zum Wechseln der [Azure-Verwaltungsportal](https://azure.microsoft.com/services/management-portal/) und legen Sie sie manuell dafür:

1. Wechseln Sie zu [ https://portal.azure.com ](https://portal.azure.com), und melden Sie sich mit Ihren Azure-Anmeldeinformationen.
2. Klicken Sie auf **Durchsuchen &gt; Web-Apps**, klicken Sie dann auf den Namen der Web-app.
3. Klicken Sie auf **alle Einstellungen &gt; Anwendungseinstellungen**.

Die **Anwendungseinstellungen** und **Verbindungszeichenfolge** Werte überschreiben die gleichen Einstellungen in der *"Web.config"* Datei. In unserem Beispiel wir nicht diese Einstellungen Bereitstellen in Azure, aber das wäre, wenn diese Schlüssel der *"Web.config"* -Datei, die Einstellungen im Portal angezeigt würde Vorrang.

Eine bewährte Methode ist, führen eine [DevOps-Workflow](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) und [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (oder ein anderes Framework, z. B. [Chef](http://www.opscode.com/chef/) oder [Puppet](http://puppetlabs.com/puppet/what-is-puppet)), Das Festlegen dieser Werte in Azure zu automatisieren. Das folgende PowerShell-Skript verwendet [Export-CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) So exportieren Sie die verschlüsselten geheimen Schlüssel auf den Datenträger:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

'Im obigen Skript Name' ist der Name des geheimen Schlüssels, z. B. "&quot;FB\_AppSecret&quot; oder"TwitterSecret". Sie können die ".credential"-Datei erstellt, indem Sie das Skript in Ihrem Browser anzeigen. Der folgende Codeausschnitt überprüft alle Dateien mit Anmeldeinformationen und die geheimen Schlüssel für die benannte Web-app:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Sicherheit – keine Kennwörter oder andere geheime Schlüssel im PowerShell-Skript, tun dies der Fall ist widerlegen den Zweck der Verwendung eines PowerShell-Skripts zur Bereitstellung von sensiblen Daten enthalten. Die [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) Cmdlet bietet einen sicheren Mechanismus, um ein Kennwort zu erhalten. Benutzeroberflächen-Eingabeaufforderung verwenden, kann die Speicherverluste verursachen, ein Kennwort verhindern.


### <a name="deploying-db-connection-strings"></a>Bereitstellung von DB-Verbindungszeichenfolgen

DB-Verbindungszeichenfolgen werden auf app-Einstellungen auf ähnliche Weise behandelt. Wenn Sie Ihre Web-app aus Visual Studio bereitstellen, wird die Verbindungszeichenfolge für Sie konfiguriert werden. Sie können dies im Portal überprüfen. Die empfohlene Methode zum Festlegen der Verbindungszeichenfolge wird mit PowerShell. Ein Beispiel für ein PowerShell-Skript den erstellt eine Website und die Datenbank und legt die Verbindungszeichenfolge fest auf der Website herunterladen [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) aus der [Azure Skriptbibliothek](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Anmerkungen zu dieser Version für PHP

Da die Schlüssel-Wert-Paare für beide **app-Einstellungen** und **Verbindungszeichenfolgen** befinden sich in Umgebungsvariablen in Azure App Service, Entwickler, die alle Web-app-Frameworks (z. B. PHP) können problemlos verwenden Diese Werte abrufen. Finden Sie unter der Stefan Schackow [Windows Azure-Websites: wie Anwendungszeichenfolgen und Verbindungszeichenfolgen](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) Blog-Beitrag, der einen PHP-Ausschnitt zum Lesen von app-Einstellungen und Verbindungszeichenfolgen anzeigt.

## <a name="notes-for-on-premises-servers"></a>Hinweise für die auf lokalen Servern

Wenn Sie an einem lokalen Webserver bereitstellen, können Sie Schutz für Geheimnisse durch [Verschlüsseln der Konfigurationsabschnitte von Konfigurationsdateien](https://msdn.microsoft.com/library/ff647398.aspx). Als Alternative können Sie den gleichen Ansatz für die Azure-Websites empfohlen: behalten Sie entwicklungseinstellungen, in Konfigurationsdateien, und verwenden Sie die Werte von Umgebungsvariablen für die Produktionseinstellungen. In diesem Fall, müssen Sie jedoch Code der Anwendung für die Funktionalität zu schreiben, die automatisch auf Azure Websites ist: Einstellungen aus Umgebungsvariablen abrufen und diese Werte anstelle von Einstellungen in der Konfigurationsdatei verwenden, oder verwenden Sie die Einstellungen in der Konfigurationsdatei beim Umgebungsvariablen wurden nicht gefunden.

<a id="addRes"></a>
## <a name="additional-resources"></a>Zusätzliche Ressourcen

Ein Beispiel für ein PowerShell-Skript, erstellt eine Web-app und Datenbank, legt fest, die Verbindungszeichenfolge + app-Einstellungen, Downloads [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) aus der [Azure Skriptbibliothek](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Finden Sie unter der Stefan Schackow [Windows Azure-Websites: wie Anwendungs- und Verbindung Zeichenfolgen arbeiten](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Besonderer Dank gilt Barry Dorrans ( [ @blowdart ](https://twitter.com/blowdart) ) und Carlos Farre geprüft.
