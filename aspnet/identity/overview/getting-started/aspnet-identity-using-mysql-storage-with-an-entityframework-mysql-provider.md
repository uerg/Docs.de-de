---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: Verwenden von MySQL-Speicher mit einem EntityFramework-MySQL-Anbieter (c#) | Microsoft-Dokumentation'
author: maumar
description: In diesem Tutorial erfahren Sie, wie Sie den Standardmechanismus für die Speicherung von Daten für ASP.NET Identity mit EntityFramework (SQL-Client-Anbieter) mit einer MySQL-installierten hardwarelösungen ersetzen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: ''
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 6b1d57c65cb4197d1b20175415ee73b3e81cb53f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383038"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: Verwenden von MySQL-Speicher mit einem EntityFramework-MySQL-Anbieter (c#)
====================
durch [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> In diesem Tutorial erfahren Sie, wie den Standardmechanismus für die Speicherung von Daten für ersetzen [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) mit EntityFramework (SQL-Client-Anbieter) mit einem MySQL-Anbieter.


In diesem Tutorial werden die folgenden Themen behandelt:

- Erstellen eine MySQL-Datenbank in Azure
- Erstellen einer MVC-Anwendung, die mithilfe von Visual Studio 2013-MVC-Vorlage
- Konfigurieren von EntityFramework zum Arbeiten mit einem MySQL-Datenbankanbieter
- Ausführen der Anwendung die Ergebnisse überprüfen

Am Ende dieses Tutorials müssen Sie eine MVC-Anwendung mit der ASP.NET Identity zu speichern, die eine MySQL-Datenbank verwendet wird, die in Azure gehostet wird.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Erstellen eine MySQL-Datenbankinstanz in Azure

1. Melden Sie sich beim [Azure-Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409) an.
2. Klicken Sie auf **neu** am unteren Rand der Seite, und wählen Sie dann **STORE**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. In der **auswählen und der Add-On-** Assistenten **ClearDB MySQL-Datenbank**, und klicken Sie dann auf die **Weiter** Pfeil am unteren Rand der Frame:  
  
   [Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Behalten Sie den Standardwert **Free** planen, ändern Sie die **Namen** zu **IdentityMySQLDatabase**, wählen Sie die Region, die Ihnen am nächsten liegt, und klicken Sie dann auf die **Weiter** Pfeil am unteren Rand der Frame:  
  
   [Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Klicken Sie auf die **Kauf** Häkchen, um die Datenbankerstellung abzuschließen.  
  
   [Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Nachdem Ihre Datenbank erstellt wurde, können Sie verwalten, von der **-Add-Ons** Registerkarte im Verwaltungsportal. Klicken Sie zum Abrufen der Verbindungsinformationen für Ihre Datenbank auf **VERBINDUNGSINFORMATIONEN** am unteren Rand der Seite:  
  
   [Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Kopieren Sie die Verbindungszeichenfolge durch Klicken auf die Schaltfläche "Kopieren", indem die **"ConnectionString"** ; ein, und speichern sie diese Informationen später in diesem Tutorial verwenden Sie für Ihre MVC-Anwendung:  
  
   [Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Erstellen einer MVC-Anwendungsprojekt

Um die Schritte in diesem Abschnitt des Tutorials ausführen zu können, müssen Sie zuerst installieren [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Sobald Visual Studio installiert wurde, verwenden Sie die folgenden Schritte aus, um ein neues MVC-Anwendungsprojekt zu erstellen:

1. Öffnen Sie Visual Studio 2103.
2. Klicken Sie auf **neues Projekt** aus der **starten** Seite, oder Sie klicken auf die **Datei** Menü und dann **neues Projekt**:  
  
   [Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Wenn die **neues Projekt** Dialogfeld wird angezeigt, erweitern Sie **Visual C#-** in der Liste der Vorlagen, klicken Sie dann auf **Web**, und wählen Sie **ASP.NET-Webanwendung**. Benennen Sie Ihr Projekt **IdentityMySQLDemo** , und klicken Sie dann auf **OK**:  
  
   [Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **MVC** Templatewith Standardoptionen; dadurch konfigurieren **einzelne Benutzerkonten** als Authentifizierungsmethode. Klicken Sie auf **OK**:  
  
   [Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Konfigurieren Sie EntityFramework, um die Arbeit mit einer MySQL-Datenbank

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Die Entity Framework-Assembly für das Projekt aktualisieren

Die MVC-Anwendung, die aus der Visual Studio 2013-Vorlage erstellt wurde, enthält einen Verweis auf die [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) Verpacken, aber es wurden Updates auf die Assembly seit der Veröffentlichung enthalten wichtige leistungsverbesserungen. Um diese neueste Updates in Ihrer Anwendung verwenden möchten, verwenden Sie die folgenden Schritte aus.

1. Öffnen Sie das MVC-Projekt in Visual Studio 2013.
2. Klicken Sie auf **TOOLS**, klicken Sie dann auf **Bibliothekspaket-Manager**, und klicken Sie dann auf **-Paket-Manager-Konsole**:  
  
   [Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. Die **-Paket-Manager-Konsole** wird im unteren Abschnitt der Visual Studio angezeigt. Typ &quot; **Update-Package EntityFramework** &quot; , und drücken Sie die EINGABETASTE:  
  
   [Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Installieren des MySQL-Anbieters für EntityFramework

Damit für EntityFramework, eine Verbindung mit MySQL-Datenbank herstellen kann müssen Sie einen MySQL-Anbieter zu installieren. Zu diesem Zweck öffnen Sie die **-Paket-Manager-Konsole** und &quot; **Install-Package MySql.Data.Entity - Pre-**&quot;, und drücken Sie dann die EINGABETASTE.

> [!NOTE]
> Dies ist eine Vorabversion der Assembly, und es kann daher Fehler enthalten. Sie sollten eine Vorabversion des Anbieters nicht in der Produktion verwenden.


[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.]  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Konfigurationsänderungen für Projekt die Datei "Web.config" für Ihre Anwendung

Registrieren Sie in diesem Abschnitt Sie Entity Framework konfigurieren für den MySQL-Anbieter verwenden, den Sie soeben installiert haben die MySQL-Anbieter-Factory, und fügen Sie die Verbindungszeichenfolge aus Azure hinzu.

> [!NOTE]
> Die folgenden Beispiele enthalten eine bestimmte Assemblyversion für MySql.Data.dll. Wenn die Version der Assembly geändert wird, müssen Sie die entsprechenden Konfigurationseinstellungen mit der richtigen Version ändern.


1. Öffnen Sie die Datei "Web.config" für Ihr Projekt in Visual Studio 2013.
2. Suchen Sie die folgenden Konfigurationseinstellungen, die den standardmäßigen Datenbankanbieter und die Factory für das Entity Framework zu definieren:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Ersetzen Sie die Konfigurationseinstellungen, mit der folgenden, die Entity Framework zur Verwendung der MySQL-Anbieters konfigurieren: 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Suchen Sie die &lt;ConnectionStrings&gt; Abschnitt, und Ersetzen Sie ihn durch den folgenden Code, die definieren, wird die Verbindungszeichenfolge für Ihre MySQL-Datenbank gehostet wird, die in Azure (Beachten Sie, dass der Wert von "ProviderName" auch aus geändert wurde der Original):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Hinzufügen von benutzerdefinierten MigrationHistory-Kontext

Entity Framework Code First verwendet eine **MigrationHistory** Tabelle zum Nachverfolgen Änderungen des Datenmodells und um sicherzustellen, dass die Konsistenz zwischen dem Datenbankschema und dem konzeptionellen Schema. In dieser Tabelle funktioniert nicht für MySQL standardmäßig jedoch, da der Primärschlüssel zu groß ist. Um dieses Problem zu lösen, müssen Sie die Größe des Schlüssels für die Tabelle zu verkleinern. Zu diesem Zweck verwenden Sie die folgenden Schritte aus:

1. Die Schemainformationen für diese Tabelle wird erfasst, eine **"historycontext"**, die sein kann, geändert wie jedes andere **"DbContext"**. Zu diesem Zweck fügen Sie eine neue Klassendatei mit dem Namen **MySqlHistoryContext.cs** auf das Projekt, und Ersetzen Sie den Inhalt durch den folgenden Code:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Als Nächstes müssen Sie zum Konfigurieren von Entity Framework für die geänderte verwenden **"historycontext"**, statt die standardmäßige. Dies kann durch die Nutzung von Funktionen für codebasierte Konfiguration erfolgen. Zu diesem Zweck fügen Sie neue Klassendatei namens **MySqlConfiguration.cs** zu Ihrem Projekt und Ersetzen Sie den Inhalt mit:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Erstellen einen benutzerdefinierten EntityFramework-Initialisierer für ApplicationDbContext

Der MySQL-Anbieter, der in diesem Tutorial vorgestellt wird unterstützt derzeit Entity Framework-Migrationen nicht, sodass Sie Modell-Initialisierer verwenden, um mit der Datenbank herstellen müssen. Da in diesem Tutorial eine MySQL-Instanz in Azure verwendet, müssen Sie einen benutzerdefinierten Initialisierer von Entity Framework zu erstellen.

> [!NOTE]
> Dieser Schritt ist nicht erforderlich, wenn Sie eine Verbindung herstellen mit einer SQL Server-Instanz auf Azure oder bei Verwendung eine Datenbank, die lokal gehostet wird.


Um einen benutzerdefinierten Entity Framework-Initialisierer für MySQL zu erstellen, verwenden Sie die folgenden Schritte aus:

1. Fügen Sie eine neue Klassendatei mit dem Namen **MySqlInitializer.cs** im Projekt, und Ersetzen sie darin Inhalt durch den folgenden Code: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Öffnen der **IdentityModels.cs** Datei für Ihr Projekt, die sich im befindet der **Modelle** Verzeichnis, und Ersetzen Sie deren Inhalt durch Folgendes: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Ausführen der Anwendung, und überprüfen die Datenbank

Nachdem Sie die Schritte in den vorherigen Abschnitten abgeschlossen haben, sollten Sie Ihre Datenbank testen. Zu diesem Zweck verwenden Sie die folgenden Schritte aus:

1. Drücken Sie **STRG + F5** erstellen und Ausführen der Webanwendung.
2. Klicken Sie auf die **registrieren** Registerkarte oben auf der Seite:  
  
   [Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Geben Sie einen neuen Benutzernamen und ein Kennwort ein, und klicken Sie dann auf **registrieren**:  
  
   [Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. An diesem Punkt werden die ASP.NET Identity-Tabellen für die MySQL-Datenbank erstellt, und der Benutzer registriert ist, und der Anwendung angemeldet werden:  
  
   [Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Installieren von MySQL Workbench-Tool, um die Daten zu überprüfen

1. Installieren Sie die **MySQL Workbench** tool die [MySQL-Downloadseite](http://dev.mysql.com/downloads/windows/installer/)
2. Im Installations-Assistenten: **Funktionsauswahl** Registerkarte **MySQL Workbench** unter **Anwendungen** Abschnitt.
3. Starten Sie die app aus, und fügen Sie eine neue Verbindung mit die Verbindungsdaten für die Zeichenfolge aus der Azure MySQL-Datenbank, die Sie an, die dadurch in diesem Tutorial erstellt haben.
4. Überprüfen Sie nach dem Herstellen der Verbindung, die **ASP.NET Identity** Tabellen erstellt werden, auf die **IdentityMySQLDatabase.**
5. Sie sehen, dass alle ASP.NET Identity, dass die Tabellen erstellt werden erforderlich, wie in der folgenden Abbildung gezeigt:  
  
   [Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Überprüfen Sie die **"aspnetusers"** Tabelle z. B. für die Einträge zu überprüfen, wie Sie neue Benutzer registrieren.  
  
   [Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
