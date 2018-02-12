---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: "ASP.NET-Identität: Verwenden von MySQL-Speicher mit einer EntityFramework MySQL-Anbieter (c#) | Microsoft Docs"
author: maumar
description: "In diesem Lernprogramm wird gezeigt, wie zum Ersetzen des Standardmechanismus für die Speicherung von Daten für ASP.NET Identity mit EntityFramework (SQL-Client-Anbieter) mit einem MySQL bereitstellen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 82341724286a53f7883df324a391beeae3a9e2bd
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a>ASP.NET Identity: Verwenden von MySQL-Speicher mit EntityFramework MySQL-Anbieter (c#)
====================
durch [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)

> Dieses Lernprogramm veranschaulicht das Ersetzen des Standardmechanismus für die Speicherung von Daten für [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) mit EntityFramework (SQL-Client-Anbieter) mit einem MySQL-Anbieter.


In diesem Lernprogramm werden die folgenden Themen behandelt:

- Erstellen eine MySQL-Datenbank in Azure
- Erstellen einer MVC-Anwendung mit Visual Studio 2013 MVC-Vorlage
- Konfigurieren von EntityFramework zur Bearbeitung eines MySQL-Datenbank-Anbieters
- Ausführen der Anwendung die Ergebnisse überprüfen

Am Ende dieses Lernprogramms müssen Sie eine MVC-Anwendung mit dem ASP.NET Identity zu speichern, die eine MySQL-Datenbank verwendet, die in Azure gehostet wird.

## <a name="creating-a-mysql-database-instance-on-azure"></a>Erstellen eine MySQL-Datenbankinstanz für Azure

1. Melden Sie sich auf die [Azure-Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).
2. Klicken Sie auf **neu** am unteren Rand der Seite, und wählen Sie dann **STORE**:  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. In der **auswählen und Add-Ons** Assistenten **ClearDB MySQL-Datenbank**, und klicken Sie dann auf die **Weiter** Pfeil am unteren Rand der Frame:  
  
 [Klicken Sie auf das folgende Bild aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. Behalten Sie den Standardwert **frei** planen, ändern Sie die **Namen** auf **IdentityMySQLDatabase**, wählen Sie die Region, die Ihnen am nächsten liegt, und klicken Sie dann auf die **Weiter** Pfeil am unteren Rand der Frame:  
  
 [Klicken Sie auf das folgende Bild aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. Klicken Sie auf die **Kauf** Häkchen, um die Datenbankerstellung abgeschlossen.  
  
 [Klicken Sie auf das folgende Bild aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. Nachdem die Datenbank erstellt wurde, können Sie ihn aus verwalten die **ADD-ONS** Registerkarte im Verwaltungsportal. Um die Verbindungsinformationen für Ihre Datenbank abgerufen werden sollen, klicken Sie auf **VERBINDUNGSINFORMATIONEN** am unteren Rand der Seite:  
  
 [Klicken Sie auf das folgende Bild aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. Kopieren Sie die Verbindungszeichenfolge durch Klicken auf die Schaltfläche Kopieren, indem Sie die **"ConnectionString"** ; Feld und speichern sie diese Informationen später in diesem Lernprogramm verwenden Sie für Ihre MVC-Anwendung:  
  
 [Klicken Sie auf das folgende Bild aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a>Erstellen ein MVC-Anwendungsprojekt

Um die Schritte in diesem Abschnitt des Lernprogramms abgeschlossen haben, müssen Sie zuerst installieren [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Nachdem Visual Studio installiert wurde, verwenden Sie die folgenden Schritte aus, um ein neues MVC-Anwendungsprojekt erstellen:

1. Open Visual Studio 2103.
2. Klicken Sie auf **neues Projekt** aus der **starten** Seite, oder Sie klicken Sie auf die **Datei** Menü und dann **neues Projekt**:  
  
 [Klicken Sie auf das folgende Bild aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. Wenn die **neues Projekt** im Dialogfeld angezeigt wird, erweitern Sie **Visual C#-** in der Liste der Vorlagen, klicken Sie dann auf **Web**, und wählen Sie **ASP.NET-Webanwendung**. Namen für das Projekt **IdentityMySQLDemo** , und klicken Sie dann auf **OK**:  
  
 [Klicken Sie auf das folgende Bild aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **MVC** Templatewith Standardoptionen; Dadurch werden konfigurieren **einzelne Benutzerkonten** als Authentifizierungsmethode. Klicken Sie auf **OK**:  
  
 [Klicken Sie auf das folgende Bild aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a>Konfigurieren von EntityFramework zur Bearbeitung einer MySQL-Datenbank

### <a name="update-the-entity-framework-assembly-for-your-project"></a>Aktualisieren Sie die Entity Framework-Assembly für das Projekt

Die MVC-Anwendung, die aus der Visual Studio 2013-Vorlage erstellt wurde, enthält einen Verweis auf die [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) Verpacken, aber es wurden Updates, die auf diese Assembly seit der Version enthalten wichtige wurde leistungsverbesserungen. Um diese neueste Updates in Ihrer Anwendung verwenden möchten, verwenden Sie die folgenden Schritte aus.

1. Öffnen Sie das MVC-Projekt in Visual Studio 2013.
2. Klicken Sie auf **TOOLS**, klicken Sie dann auf **Bibliothekspaket-Manager**, und klicken Sie dann auf **Package Manager Console**:  
  
 [Klicken Sie auf das folgende Bild aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. Die **Package Manager Console** erscheint im unteren Abschnitt des Visual Studio. Typ &quot; **das Updatepaket EntityFramework** &quot; und drücken Sie die EINGABETASTE:  
  
 [Klicken Sie auf das folgende Bild aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a>Installieren Sie den MySQL-Anbieter für EntityFramework

In der Reihenfolge für EntityFramework zur Verbindung mit MySQL-Datenbank müssen Sie einen MySQL-Anbieter installieren. Öffnen Sie hierzu die **Package Manager Console** und Typ &quot; **Install-Package MySql.Data.Entity - Pre**&quot;, und drücken Sie dann die EINGABETASTE.

> [!NOTE]
> Dies ist eine Vorabversion der Assembly, und daher möglicherweise Fehler enthalten. Sie sollten eine Vorabversion des Anbieters nicht in der Produktion verwenden.


[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.]  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a>Projekt-konfigurationsänderungen vorzunehmen, um die Datei "Web.config" für Ihre Anwendung

Registrieren Sie in diesem Abschnitt Sie Entity Framework konfigurieren um MySQL-Anbieter verwenden, den Sie gerade installiert haben die MySQL-Anbieter-Factory, und fügen Sie der Verbindungszeichenfolge von Azure hinzu.

> [!NOTE]
> In den folgenden Beispielen wird eine bestimmte Assemblyversion für MySql.Data.dll enthalten. Wenn die Assemblyversion geändert wird, müssen Sie die entsprechenden Konfigurationseinstellungen mit der richtigen Version ändern.


1. Öffnen Sie die Datei "Web.config" für das Projekt in Visual Studio 2013.
2. Suchen Sie die folgenden Konfigurationseinstellungen, die den Standardanbieter für die Datenbank und die Factory für das Entity Framework zu definieren:

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. Ersetzen Sie diese Konfigurationseinstellungen, durch die folgende Darstellung, die das Entity Framework zur Verwendung von MySQL-Anbieter konfiguriert werden soll: 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. Suchen Sie die &lt;ConnectionStrings&gt; Abschnitt, und Ersetzen Sie ihn durch den folgenden Code, mit denen definiert wird, wird die Verbindungszeichenfolge für Ihre MySQL-Datenbank gehostet wird, die in Azure (Beachten Sie, dass auch ProviderName-Wert von geändert wurde die Original):

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a>Hinzufügen von benutzerdefinierten MigrationHistory Kontext

Entity Framework Code First verwendet eine **MigrationHistory** Tabelle zum Nachverfolgen modelländerungen und um die Konsistenz zwischen dem Datenbankschema und konzeptionelle sicherzustellen. Allerdings funktioniert in dieser Tabelle für MySQL standardmäßig nicht, da der Primärschlüssel zu groß ist. Um diese Situation zu vermeiden, müssen Sie die Größe des Schlüssels für die Tabelle zu verkleinern. Verwenden Sie hierzu die folgenden Schritte aus:

1. Die Schemainformationen für diese Tabelle wird erfasst, einem **HistoryContext**, die sein kann wie jeder andere geändert **DbContext**. Zu diesem Zweck fügen Sie eine neue Klassendatei mit dem Namen **MySqlHistoryContext.cs** für das Projekt, und Ersetzen Sie den Inhalt durch folgenden Code:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. Als Nächstes müssen Sie Entity Framework, um das geänderte verwenden konfigurieren **HistoryContext**, statt einen Standardwert. Dies kann durch die Nutzung der Features codebasierte Konfiguration erfolgen. Zu diesem Zweck fügen Sie neue Klassendatei namens **MySqlConfiguration.cs** zu Ihrem Projekt, und Ersetzen Sie den Inhalt mit:

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a>Erstellen einen benutzerdefinierten EntityFramework Initialisierer für ApplicationDbContext

Der MySQL-Anbieter, der in diesem Lernprogramm Funktionsumfang ist unterstützt derzeit Entity Framework keine Migrationen, daher Sie die Modell-Initialisierer verwenden, um eine Verbindung mit der Datenbank herzustellen müssen. Da in diesem Lernprogramm eine MySQL-Instanz in Azure verwendet wird, müssen Sie einen benutzerdefinierten Initialisierer für Entity Framework zu erstellen.

> [!NOTE]
> Dieser Schritt ist nicht erforderlich, wenn Sie eine Verbindung zu SQL Server-Instanz in Azure oder bei Verwendung eine Datenbank, die lokal gehostet wird.


Um einen benutzerdefinierten Entity Framework-Initialisierer für MySQL zu erstellen, verwenden Sie die folgenden Schritte aus:

1. Fügen Sie eine neue Klassendatei mit dem Namen **MySqlInitializer.cs** auf das Projekt, und Ersetzen sie Inhalt durch folgenden Code wird: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. Öffnen der **IdentityModels.cs** für das Projekt, befindet sich im die **Modelle** Verzeichnis und dessen Inhalt durch Folgendes ersetzen: 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a>Ausführen der Anwendung, und überprüfen die Datenbank

Wenn Sie die Schritte in den vorangehenden Abschnitten abgeschlossen haben, sollten Sie Ihre Datenbank testen. Verwenden Sie hierzu die folgenden Schritte aus:

1. Drücken Sie **STRG + F5** erstellen und Ausführen der Anwendung.
2. Klicken Sie auf die **registrieren** Registerkarte oben auf der Seite:  
  
 [Klicken Sie auf das folgende Bild aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. Geben Sie einen neuen Benutzernamen und ein Kennwort ein, und klicken Sie dann auf **registrieren**:  
  
 [Klicken Sie auf das folgende Bild aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. An diesem Punkt werden die ASP.NET Identity-Tabellen für die MySQL-Datenbank erstellt, und der Benutzer registriert und der Anwendung protokolliert wird:  
  
 [Klicken Sie auf das folgende Bild aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a>Installieren von MySQL-Workbench-Tool, um die Daten zu überprüfen

1. Installieren der **MySQL-Workbench** -tool aus dem [downloads für MySQL](http://dev.mysql.com/downloads/windows/installer/)
2. Im Installations-Assistenten: **Funktionsauswahl** Registerkarte **MySQL-Workbench** unter **Anwendungen** Abschnitt.
3. Starten Sie die app, und fügen Sie eine neue Verbindung mit die Verbindungsdaten für die Zeichenfolge aus der Azure-MySQL-Datenbank, die Sie an der wünschenswerten dieses Lernprogramms erstellt haben.
4. Überprüfen Sie nach dem Herstellen der Verbindung, die **ASP.NET Identity** Tabellen erstellt, auf die **IdentityMySQLDatabase.**
5. Sie sehen, dass alle ASP.NET Identity Tabellen erstellt werden erforderlichen, wie in der folgenden Abbildung gezeigt:  
  
 [Klicken Sie auf das folgende Bild aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. Überprüfen Sie die **Aspnetusers** Tabelle für die Instanz für die Einträge zu überprüfen, wie Sie neue Benutzer registrieren.  
  
 [Klicken Sie auf das folgende Bild aus, um ihn zu erweitern. ]  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
