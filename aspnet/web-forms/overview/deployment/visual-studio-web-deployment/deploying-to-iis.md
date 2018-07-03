---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'ASP.NET-webbereitstellung mithilfe von Visual Studio: Bereitstellen für Testzwecke | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, indem Warnungsprovider...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: 8c5034dd4948d96c5722e2dcc960cc0349241a1a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365684"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>ASP.NET-webbereitstellung mithilfe von Visual Studio: Bereitstellen für Testzwecke
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, mithilfe von Visual Studio 2012 oder Visual Studio 2010. Weitere Informationen über die Reihe finden Sie unter [im ersten Tutorial der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

Dieses Tutorial veranschaulicht, wie eine ASP.NET-Webanwendung in IIS auf dem lokalen Computer bereitstellen.

Wenn Sie eine Anwendung entwickeln, testen Sie in der Regel, indem Sie sie in Visual Studio ausführen. In der Standardeinstellung verwenden Webanwendungsprojekte in Visual Studio 2012 als Entwicklungswebserver IIS Express. IIS Express verhält sich ähnlich wie der vollständigen Variante von IIS als die Visual Studio Development Server (auch als Cassini bezeichnet), die Visual Studio 2010 wird standardmäßig verwendet. Aber weder Entwicklungswebserver funktioniert genauso wie IIS. Daher ist es möglich, dass eine Anwendung ordnungsgemäß ausgeführt wird, wenn Sie es in Visual Studio testen, aber fehlschlagen, wenn es in IIS bereitgestellt wird.

Sie können Ihre Anwendung zuverlässiger auf folgende Arten testen:

1. Bereitstellen der Anwendung in IIS auf Ihrem Entwicklungscomputer unter Verwendung des gleichen Prozesses, die Sie später verwenden sie in Ihrer produktionsumgebung bereitstellen. Sie können Visual Studio, um IIS zu verwenden, wenn ein Webprojekt ausführen, aber dies nicht Ihres Bereitstellungsprozesses testen würde konfigurieren. Diese Methode überprüft, ob Ihr Bereitstellungsprozess zusätzlich zu überprüfen, den Ihre Anwendung unter IIS ordnungsgemäß ausgeführt wird.
2. Bereitstellen der Anwendung in einer testumgebung bereit, die nahezu identisch mit der produktionsumgebung bereit ist. Da die produktionsumgebung für diese Tutorials in Azure App Service-Web-Apps ist, ist die ideale Umgebung eine zusätzliche Web-app, die in Azure App Service erstellt. Verwenden Sie diese zweite Web-app nur zum Testen, aber sie würden sich die gleiche Weise wie die Produktions-Web-app festgelegt werden.

Option 2 ist die zuverlässigste Möglichkeit, um zu testen, und wenn Sie dies tun, Sie müssen nicht unbedingt über Option 1. Jedoch wenn Sie zum Bereitstellen von Drittanbietern Anbieter Hostingoption 2 möglicherweise nicht möglich oder aufwändig sein, damit diese lernprogrammreihe beide Methoden veranschaulicht. Anleitungen für Option 2 finden Sie unter den [in der Produktionsumgebung bereitstellen](deploying-to-production.md) Tutorial.

Weitere Informationen zur Verwendung von Webservern in Visual Studio finden Sie unter [Webserver in Visual Studio für ASP.NET-Webprojekte](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](troubleshooting.md).

## <a name="install-iis"></a>Installieren von IIS

Zum Bereitstellen in IIS auf dem Entwicklungscomputer müssen Sie IIS und Web Deploy installiert. Web Deploy wird standardmäßig mit Visual Studio installiert, aber IIS ist nicht in der Standardkonfiguration für Windows 8 oder Windows 7 enthalten. Wenn Sie IIS bereits installiert haben und der Standardanwendungspool in .NET 4 bereits festgelegt ist, fahren Sie mit [im nächsten Abschnitt](#sqlexpress).

1. Mithilfe der [Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx) ist die bevorzugte Methode für IIS und Web Deploy zu installieren, da der Webplattform-Installer installiert die empfohlene Konfiguration für IIS und automatisch die erforderlichen Komponenten für IIS und Web installiert Stellen Sie bei Bedarf bereit.

    Führen Sie zum Installieren von IIS und Web Deploy-Webplattform-Installer mit den folgenden Link. Wenn Sie IIS, Web Deploy oder die erforderlichen Komponenten bereits installiert haben, installiert den Webplattform-Installer nur was fehlt.

   - [Installieren von IIS und Web Deploy unter Verwendung von WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

     Sie sehen, die darauf hinweist, dass IIS 7 installiert werden soll. Der Link funktioniert für IIS 8 unter Windows 8, aber für Windows 8, stellen Sie sicher, dass ASP.NET 4.5 installiert ist, indem Sie die folgenden Schritte ausführen:

   - Open **Systemsteuerung**, **Programme und Funktionen**, **Aktivieren von Windows-Funktionen ein- oder ausschalten**.
   - Erweitern Sie **Internetinformationsdienste**, **WWW-Dienste**, und **Anwendungsentwicklungsfeatures**.
   - Stellen Sie sicher, dass **ASP.NET 4.5** ausgewählt ist.

      ![Wählen Sie ASP.NET 4.5](deploying-to-iis/_static/image1.png)

Führen Sie nach dem Installieren von IIS, **IIS-Manager** um sicherzustellen, dass .NET Framework, Version 4 des Standardanwendungspools zugewiesen ist.

1. Drücken Sie WINDOWS + R, zum Öffnen der **ausführen** Dialogfeld.

    (Oder geben Sie im Windows 8 die "run" auf die **starten** Seite, oder wählen Sie in Windows 7 **ausführen** aus der **starten** Menü. Wenn **ausführen** befindet sich nicht in der **starten** im Menü die Taskleiste, klicken Sie auf **Eigenschaften**, wählen die **Menü "Start"** auf **Anpassen**, und wählen Sie **Ausführungsbefehl**.)
2. Geben Sie "Inetmgr", und klicken Sie dann auf **OK**.
3. In der **Verbindungen** Bereich, erweitern Sie den Serverknoten und wählen Sie **Anwendungspools**. In der **Anwendungspools** Bereich Wenn **DefaultAppPool** ist .NET Frameworkversion 4, wie in der folgenden Abbildung werden zugewiesen, mit dem nächsten Abschnitt überspringen.

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. Wenn Sie sehen, dass nur zwei Anwendungspools, und beide Angaben, die auf .NET Framework 2.0 festgelegt sind, müssen Sie ASP.NET 4 in IIS zu installieren.

    Windows 8 finden Sie unter den Anweisungen im vorherigen Abschnitt sicherstellen, ASP.NET 4.5 installiert ist, oder finden Sie unter [diesem KB-Artikel](https://support.microsoft.com/kb/2736284). Für Windows 7, öffnen Sie ein Eingabeaufforderungsfenster, indem Sie mit der rechten Maustaste **Eingabeaufforderung** in der Windows **starten** Menü und auswählen **als Administrator ausführen**. Führen Sie dann [Aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) ASP.NET 4 in IIS mit den folgenden Befehlen installieren. (Ersetzen Sie in 32-Bit-Systemen, die "Framework64" mit "Framework").

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    Dieser Befehl erstellt neue Anwendungspools für die .NET Framework 4, aber der Standardanwendungspool wird immer noch auf 2.0 festgelegt. Sie müssen einer Anwendung bereitstellen, das als Ziel .NET 4 an diesen Anwendungspool, müssen Sie den Anwendungspool in .NET 4 zu ändern.
5. Wenn Sie geschlossen **IIS-Manager**, führen sie erneut aus, erweitern Sie den Serverknoten und klicken Sie auf **Anwendungspools** zum Anzeigen der **Anwendungspools** erneut zum Bereich.
6. In der **Anwendungspools** Bereich klicken Sie auf **DefaultAppPool**, und klicken Sie dann in der **Aktionen** auf **Grundeinstellungen**.

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. In der **Anwendungspool bearbeiten** im Dialogfeld **.NET Framework-Version** zu **.NET Framework-v4.0.30319** , und klicken Sie auf **OK**.

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

IIS ist nun bereit für Sie eine Webanwendung zu veröffentlichen, aber bevor Sie hierzu müssen Sie die Datenbanken, die Sie verwenden in der testumgebung zu erstellen.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Installieren von SQL Server Express

LocalDB ist nicht für entwickelt in IIS für Ihre testumgebung müssen Sie SQL Server Express installiert haben. Wenn Sie Visual Studio 2010 SQL Server Express verwenden, ist standardmäßig bereits installiert. Wenn Sie Visual Studio 2012 verwenden, müssen Sie es zu installieren.

Um SQL Server Express zu installieren, installieren Sie sie über [Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) durch Klicken auf [ENU\x64\SQLEXPR\_X64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe) oder [ ENU\x86\SQLEXPR\_X86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe). Wenn Sie die falsche Version für Ihr System es kann nicht installiert und können Sie versuchen, der andere Controller.

Klicken Sie auf der ersten Seite des SQL Server-Installationscenter auf **eigenständige neue SQL Server-Installation oder Hinzufügen von Funktionen zu einer vorhandenen Installation**, und befolgen Sie die Anweisungen, übernehmen die Standardoptionen. Übernehmen Sie im Installations-Assistenten die Standardeinstellungen aus. Weitere Informationen zu Installationsoptionen finden Sie unter [Installieren von SQL Server 2012 vom Installations-Assistenten (Setup)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Erstellen von SQL Server Express-Datenbanken, für die testumgebung

Die Contoso University-Anwendung verfügt über zwei Datenbanken: die Mitgliedschaftsdatenbank und die Datenbank. Sie können diese Datenbanken auf zwei separate Datenbanken oder auf eine einzelne Datenbank bereitstellen. Sie möchten diese kombinieren, um Datenbankjoins zwischen Ihrer Anwendung und Ihre Mitgliedschaftsdatenbank zu ermöglichen. Wenn Sie bei einem Hostinganbieter von Drittanbietern bereitstellen, kann Ihren hostingplan auch keinen Grund, diese zu kombinieren angegeben. Z. B. der Hostinganbieter möglicherweise mehr für mehrere Datenbanken berechnet oder u. u. nicht mehr als eine Datenbank selbst zu.

In diesem Tutorial stellen Sie zwei Datenbanken in der testumgebung und eine Datenbank in der Staging- und produktionsumgebungen bereit.

Von der **Ansicht** Menü in Visual Studio auf **Server-Explorer** (**Datenbank-Explorer** in Visual Web Developer), und klicken Sie dann mit der rechten Maustaste **Datenverbindungen**  , und wählen Sie **neue SQL Server-Datenbank erstellen**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

In der **neue SQL Server-Datenbank erstellen** Dialogfeld Geben Sie ". \SQLExpress" in der **Servernamen** Box als auch "Aspnet-ContosoUniversity" in der **neuen Datenbanknamen** Feld, klicken Sie dann Klicken Sie auf **OK**.

![Erstellen von Aspnet-ContosoUniversity](deploying-to-iis/_static/image9.png)

Führen Sie das gleiche Verfahren zum Erstellen einer neuen SQL Server Express School-Datenbank, die mit dem Namen "ContosoUniversity".

**Server-Explorer** zeigt jetzt die zwei neuen Datenbanken.

![Neue Datenbanken im Server-Explorer](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Erstellen Sie ein Skript gewähren, für die neuen Datenbanken

Wenn die Anwendung in IIS auf Ihrem Entwicklungscomputer ausgeführt wird, greift auf die Anwendung die Datenbank mithilfe von Anmeldeinformationen für den Standardanwendungspool. Standardmäßig haben die Identität des Anwendungspools allerdings nicht Berechtigung zum Öffnen von Datenbanken. Daher müssen Sie zum Ausführen eines Skripts, um diese Berechtigung zu erteilen. In diesem Abschnitt erstellen Sie das Skript, dass Sie weiter unten, um sicherzustellen, dass die Anwendung auf die Datenbanken öffnen kann, bei der Ausführung in IIS ausgeführt werden.

Mit der rechten Maustaste in der Projektmappe (nicht auf eines der Projekte), und klicken Sie auf **neues Element hinzufügen**, und klicken Sie dann erstellen Sie ein neues **SQL-Datei** mit dem Namen *Grant.sql*. Kopieren Sie die folgenden SQL-Befehle in die Datei, und speichern Sie und schließen Sie die Datei:

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> Dieses Skript dient zum Arbeiten mit SQL Server Express 2012 und die IIS-Einstellungen in Windows 8 oder Windows 7, wie sie in diesem Tutorial angegeben sind. Wenn Sie eine andere Version von SQL Server oder Windows verwenden, oder wenn Sie IIS auf Ihrem Computer anders festlegen, können Änderungen an diesem Skript erforderlich sein. Weitere Informationen zu SQL Server-Skripts, finden Sie unter [SQL Server-Onlinedokumentation](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Sicherheitshinweis** dieses Skript gibt Db\_Owner-Berechtigungen für den Benutzer, die Zugriff auf die Datenbank zur Laufzeit handelt es sich in der produktionsumgebung wiederholen müssen. In einigen Fällen empfiehlt es sich um einen Benutzer mit vollständigen Datenbankschema aktualisieren von Berechtigungen nur für die Bereitstellung, und geben Sie für die Laufzeit einen anderen Benutzer, der berechtigt nur zum Lesen und Schreiben von Daten. Weitere Informationen finden Sie unter [überprüfen die automatischen Änderungen der Datei "Web.config" für Code First-Migrationen](#reviewingmigrations) weiter unten in diesem Tutorial.


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Führen Sie das Skript zum erteilen, in der Datenbank

Sie können konfigurieren, dass das Veröffentlichungsprofil, um das Skript zum Erteilen in der Mitgliedschaftsdatenbank während der Bereitstellung ausgeführt wird, da es sich bei die datenbankbereitstellung der DbDacFx-Anbieter verwendet. Während der Bereitstellung von Code First-Migrationen, die ist, wie Sie die Datenbank bereitstellen möchten, können Sie können keine Skripts ausführen. Aus diesem Grund müssen Sie manuell das Skript vor der Bereitstellung in der Datenbank ausführen.

1. Öffnen Sie in Visual Studio die *Grant.sql* -Datei, die Sie zuvor erstellt haben.
2. Klicken Sie auf **Verbinden**. 

    ![Schaltfläche "Verbinden"](deploying-to-iis/_static/image11.png)
3. In der **Herstellen einer Verbindung mit Server** Dialogfeld Geben Sie *. \SQLExpress* als die **Servernamen**, und klicken Sie dann auf **Connect**.
4. Wählen Sie in der Datenbank-Dropdown-Liste **ContosoUniversity**, und klicken Sie dann auf **Execute**. 

    ![](deploying-to-iis/_static/image12.png)

Die Standardidentität des Anwendungspools verfügt jetzt über ausreichende Berechtigungen in der Anwendungsdatenbank für Code First-Migrationen auf die Tabellen der Datenbank zu erstellen, wenn die Anwendung ausgeführt wird.

## <a name="publish-to-iis"></a>In IIS veröffentlichen

Es gibt mehrere Möglichkeiten, die Sie IIS mithilfe von Visual Studio und Web Deploy bereitstellen können:

- Verwenden Sie Visual Studio-One-Click-Veröffentlichung.
- Veröffentlichen Sie über die Befehlszeile.
- Erstellen Sie eine *Bereitstellungspaket* herunter, und installieren Sie es mithilfe der IIS-Manager-UI. Das Bereitstellungspaket besteht aus einem *ZIP* -Datei, enthält alle Dateien und Metadaten, die zum Installieren eines Standorts in IIS erforderlich.
- Erstellen eines Bereitstellungspakets aus, und installieren Sie es über die Befehlszeile.

Der Prozess, die in Visual Studio einrichten, um automatisieren den vorherigen Tutorials durcharbeiten Bereitstellungsaufgaben gilt für alle diese Methoden. In diesen Tutorials verwenden Sie die ersten beiden dieser Methoden. Weitere Informationen zur Verwendung von Bereitstellungspaketen, finden Sie unter [Bereitstellen einer Web-Anwendung erstellen und Installieren eines Webbereitstellungspakets](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) in den Einstieg in die Webbereitstellung für Visual Studio und ASP.NET.

Stellen Sie sicher, dass Sie Visual Studio im Administratormodus ausgeführt werden, vor der Veröffentlichung. Wenn Sie nicht sehen **(Administrator)** in der Titelleiste angezeigt wird, schließen Sie Visual Studio. In Windows 8 **starten** Seite oder die Windows 7 **starten** Menü, mit der rechten Maustaste in des Symbols für die Version von Visual Studio, die Sie verwenden, und wählen Sie **als Administrator ausführen**. Im Administratormodus ist erforderlich, für die Veröffentlichung nur wenn Sie in IIS auf dem lokalen Computer veröffentlichen.

### <a name="create-the-publish-profile"></a>Erstellen Sie das Veröffentlichungsprofil

1. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt ContosoUniversity (nicht auf das Projekt ContosoUniversity.DAL), und wählen Sie **veröffentlichen**.

    Die **Webveröffentlichung** -Assistent wird angezeigt.

    ![Veröffentlichen Sie die Assistenten "Web", Registerkarte "Profil"](deploying-to-iis/_static/image13.png)
2. Wählen Sie in der Dropdown-Liste  **&lt;neu... &gt;**. (Mit dem aktuellen Visual Studio-Update installiert, keine Dropdown-Liste vorhanden ist, und die Schaltfläche zu klicken, um ein neues Profil von Grund auf neu zu erstellen ist **benutzerdefinierte**.)
3. In der **neues Profil** Dialogfeld Geben Sie "Test", und klicken Sie dann auf **OK**.

    Der Assistent springt automatisch zu den **Verbindung** Registerkarte.
4. In der **-Dienst-URL** geben *"localhost"*.
5. In der **Website/Anwendung** geben *Default Web Site/ContosoUniversity*
6. In der **Ziel-URL** Geben Sie `http://localhost/ContosoUniversity`

    Die **Ziel-URL** Einstellung ist nicht erforderlich. Wenn Visual Studio abgeschlossen ist, die Anwendung bereitstellen, wird er automatisch Ihren Standardbrowser mit dieser URL geöffnet. Wenn Sie nicht möchten, dass den Browser nach der Bereitstellung automatisch geöffnet wird, lassen Sie dieses Feld leer.
7. Klicken Sie auf **Verbindung überprüfen** zu überprüfen, ob die Einstellungen richtig sind und Sie können mit IIS auf dem lokalen Computer verbinden.

    Ein grünes Häkchen stellt sicher, dass die Verbindung erfolgreich ist.

    ![Veröffentlichen von Web-Assistent – Registerkarte "Verbindung"](deploying-to-iis/_static/image14.png)
8. Klicken Sie auf **Weiter** , fahren Sie fort, um die **Einstellungen** Registerkarte.
9. Die **Konfiguration** Dropdown-Feld gibt an, die Build-Konfiguration bereitstellen. Lassen Sie auf den Standardwert der Veröffentlichung festgelegt werden soll. Sie wird nicht in diesem Tutorial Debug-Builds bereitgestellt werden.
10. Erweitern Sie **Dateiveröffentlichungsoptionen**, und wählen Sie dann **Ausschließen von Dateien aus der App\_Datenordner**.

    In der testumgebung, die die Anwendung greift auf die Datenbanken, die Sie in der lokalen SQL Server Express-Instanz erstellt haben, nicht die MDF-Dateien in die *App\_Daten* Ordner.
11. Lassen Sie die **während der Veröffentlichung Vorkompilieren** und **entfernen weiterer Dateien am Ziel** Kontrollkästchen deaktiviert.

    ![Dateiveröffentlichungsoptionen in der Registerkarte "Einstellungen"](deploying-to-iis/_static/image15.png)

    Vorkompilieren ist eine Option, die in erster Linie für sehr große Websites eignet. Sie können Seite Startzeit zum ersten Mal reduzieren, die eine Seite angefordert wird, nachdem der Standort veröffentlicht wird.

    Sie müssen zusätzliche Dateien zu entfernen, da dies Ihre erste Bereitstellung und es keine Dateien im Zielordner noch nicht.

    > [!NOTE] 
    > 
    > [!CAUTION]
    > Bei Auswahl von **entfernen weiterer Dateien** für eine nachfolgende Bereitstellung desselben Standorts sicher, dass verwenden Sie das Vorschaufeature, sodass Sie im Voraus sehen, welche Dateien vor der Bereitstellung gelöscht werden. Das erwartete Verhalten ist, dass Web Deploy-Dateien auf dem Zielserver gelöscht werden, die Sie in Ihrem Projekt gelöscht haben. Allerdings wird die gesamte Ordnerstruktur unter der Quell-und Ziel verglichen, und in einigen Szenarien Web Deploy-Dateien, die Sie löschen möchten, nicht löschen kann.
    > 
    > Z. B. Wenn Sie eine Webanwendung in einem Unterordner auf dem Server verfügen, wenn Sie ein Projekt in den Stammordner bereitstellen, wird der Unterordner gelöscht werden. Sie können ein Projekt für die Hauptwebsite auf "contoso.com" und ein anderes Projekt für einen Blog unter Objekte Remote verfügen. Die Bloganwendung ist in einem Unterordner. Wenn Sie bei der Bereitstellung der Hauptwebsite von entfernen weiterer Dateien am Ziel auswählen, wird die Bloganwendung gelöscht werden.
    > 
    > Ein weiteres Beispiel, Ihre App\_Datenordner möglicherweise unerwartet gelöscht. Bestimmte Datenbanken wie SQL Server Compact-Datenbankdateien speichern, in der App\_Datenordner. Nach der ersten Bereitstellung nicht Sie möchten die Datenbankdateien in nachfolgenden Bereitstellungen kopieren beibehalten, sodass Sie die App schließen auswählen\_Daten auf der Registerkarte "Web packen/veröffentlichen". Danach, wenn Sie entfernen weiterer Dateien am Ziel, die ausgewählt, die Datenbankdateien und die App haben\_Datenordner selbst werden gelöscht, wenn Sie das nächste Mal, die Sie veröffentlichen.

### <a name="configure-deployment-for-the-membership-database"></a>Konfigurieren der Bereitstellung für die Mitgliedschaftsdatenbank

Die folgenden Schritte gelten für die **DefaultConnection** -Datenbank in die **Datenbanken** Abschnitt des Dialogfelds.

1. In der **remoteverbindungszeichenfolge** Geben Sie die folgende Verbindungszeichenfolge, die auf der neuen SQL Server Express-Mitgliedschaftsdatenbank verweist.

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    Während des Bereitstellungsvorgangs wird diese Verbindungszeichenfolge in der bereitgestellten Web.config-Datei einfügen, da **verwenden Sie diese Verbindungszeichenfolge zur Laufzeit** ausgewählt ist.

    Sie können auch die Verbindungszeichenfolge aus abrufen **Server-Explorer**. In **Server-Explorer**, erweitern Sie **Datenverbindungen** , und wählen Sie die  **&lt;"MachineName"&gt;\sqlexpress.aspnet-ContosoUniversity** Datenbank, klicken Sie dann aus der **Eigenschaften** Fenster Kopieren der **Verbindungszeichenfolge** Wert. Dass die Verbindungszeichenfolge eine weitere Einstellung haben, die Sie löschen können: `Pooling=False`.
2. Wählen Sie **Aktualisieren einer Datenbank**.

    Dadurch wird das Datenbankschema in der Zieldatenbank, die während der Bereitstellung erstellt werden. In den folgenden Schritten Geben Sie die zusätzlichen Skripts, die Sie ausführen müssen: eine zum Gewähren des Datenbankzugriffs an den Standardanwendungspool und zur Bereitstellung von Daten.
3. Klicken Sie auf **Datenbankupdates konfigurieren**.
4. In der **Datenbankupdates konfigurieren** Dialogfeld klicken Sie auf **SQL-Skript hinzufügen** und navigieren Sie zu der *Grant.sql* -Skript, das Sie zuvor im Projektmappenordner gespeichert haben.
5. Wiederholen Sie den Vorgang zum Hinzufügen der *Aspnet-Daten-dev.sql* Skript.

    ![Konfigurieren von Datenbank-Updates für die Mitgliedschaftsdatenbank](deploying-to-iis/_static/image16.png)
6. Klicken Sie auf **Schließen**.

### <a name="configure-deployment-for-the-application-database"></a>Die Bereitstellung für die Datenbank konfigurieren

Wenn Visual Studio ein Entity Framework erkennt `DbContext` -Klasse, erstellt es einen Eintrag im der **Datenbanken** Abschnitt eine **Execute Code First Migrations** Kontrollkästchen anstelle von einer  **Aktualisieren der Datenbank** Kontrollkästchen. In diesem Tutorial verwenden Sie das Kontrollkästchen, an der Bereitstellung von Code First-Migrationen.

In einigen Szenarien, verwenden Sie möglicherweise eine `DbContext` Datenbank, aber Sie möchten den DbDacFx-Anbieter statt Migrationen zu verwenden, zum Bereitstellen der Datenbank. In diesem Fall finden Sie unter [wie Stelle ich eine Code First-Datenbank ohne Migrationen bereit?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) in der ASP.NET Web-Bereitstellung – häufig gestellte Fragen auf MSDN.

Die folgenden Schritte gelten für die **SchoolContext** -Datenbank in die **Datenbanken** Abschnitt des Dialogfelds.

1. In der **remoteverbindungszeichenfolge** Geben Sie die folgende Verbindungszeichenfolge, die auf die neue Datenbank für SQL Server Express-Anwendung verweist.

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    Während des Bereitstellungsvorgangs wird diese Verbindungszeichenfolge in der bereitgestellten Web.config-Datei einfügen, da **verwenden Sie diese Verbindungszeichenfolge zur Laufzeit** ausgewählt ist.

    Außerdem erhalten Sie die Anwendung Datenbank-Verbindungszeichenfolge aus **Server-Explorer** die gleiche Weise, wie man die Mitgliedschaft in Datenbank-Verbindungszeichenfolge.
2. Wählen Sie **Code First-Migrationen ausführen (wird beim Anwendungsstart ausgeführt)**.

    Diese Option bewirkt, dass den Bereitstellungsprozess so konfigurieren Sie die bereitgestellte Datei "Web.config", an die `MigrateDatabaseToLatestVersion` Initialisierer. Diese Initialisierer aktualisiert die Datenbank automatisch auf die neueste Version, wenn die Anwendung zum ersten Mal nach der Bereitstellung auf die Datenbank zugreift.

### <a name="configure-publish-profile-transforms"></a>Konfigurieren von Transformationen von Profil veröffentlichen

1. Klicken Sie auf **schließen**, und klicken Sie dann auf **Ja** Wenn Sie gefragt werden, wenn Sie Änderungen speichern möchten.
2. In **Projektmappen-Explorer**, erweitern Sie **Eigenschaften**, erweitern Sie **PublishProfiles**.
3. Rright-Click- *Test.pubxml,* , und klicken Sie dann auf **Config Transformation hinzufügen**.

    ![Konfigurationstransformation-Menü "hinzufügen"](deploying-to-iis/_static/image17.png)

    Visual Studio erstellt die *Web.Test.config* Transform-Datei und öffnet sie.
4. In der *Web.Test.config* Transformationsdatei, fügen Sie den folgenden Code unmittelbar nach dem öffnenden Konfigurationstag.

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Wenn Sie den Test verwenden das Veröffentlichungsprofil, diese Transformation legt den Indikator "Umgebung" zu "Test". Auf der bereitgestellten Website sehen Sie nach dem "Contoso University" H1-Überschrift "(Test)".
5. Speichern und schließen Sie die Datei.
6. Mit der rechten Maustaste die *Web.Test.config* Datei, und klicken Sie auf **Vorschau transformieren** um sicherzustellen, dass die Transformation, die Sie programmiert die erwarteten Änderungen erzeugt.

    Die **Vorschau der Datei "Web.config"** Fenster zeigt das Ergebnis der Anwendung sowohl die *Datei "Web.Release.config"* transformiert und die *Web.Test.config* transformiert.

### <a name="preview-the-deployment-updates"></a>Vorschau der bereitgestellten updates

1. Öffnen der **Webveröffentlichung** Assistenten erneut aus (mit der rechten Maustaste in des ContosoUniversity-Projekts, und klicken Sie auf **veröffentlichen**).
2. In der **Vorschau** Registerkarte, stellen Sie sicher, dass die **Test** Profil wird noch ausgewählt ist, und klicken Sie dann auf **Vorschau starten** um eine Liste der Dateien anzuzeigen, die kopiert werden.

    ![Schaltfläche "Vorschau"](deploying-to-iis/_static/image18.png)

    ![Vorschau veröffentlichen](deploying-to-iis/_static/image19.png)

    Sie können auch klicken die **datenbankvorschau** Link zum Anzeigen der Skripts, die in der Mitgliedschaftsdatenbank ausgeführt werden. (Keine Skripts werden für die Bereitstellung von Code First-Migrationen, damit keine Vorschau für die Datenbank ausgeführt.)
3. Klicken Sie auf **Veröffentlichen**.

    Ist Visual Studio nicht im Administratormodus, erhalten Sie möglicherweise eine Fehlermeldung angezeigt, die einen Berechtigungsfehler angibt. In diesem Fall schließen Sie Visual Studio im Administratormodus öffnen, und versuchen Sie es erneut veröffentlichen.

    Wenn Visual Studio im Administratormodus, ist die **Ausgabe** Fenster Berichte erfolgreich erstellen und veröffentlichen.

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    Wenn Sie die URL eingegeben haben die **Ziel-URL** Feld auf das Veröffentlichungsprofil **Verbindung** Registerkarte im Browser automatisch geöffnet wird, die Contoso University-Startseite in IIS ausgeführt wird, auf dem lokalen Computer.

## <a name="test-in-the-test-environment"></a>In der testumgebung zu testen

Beachten Sie, dass der Indikator für die Umgebung "(Test)" zeigt anstelle von "(Dev)", die zeigt, dass die *"Web.config"* Transformation für den Indikator für die Umgebung erfolgreich war.

Führen Sie die **Dozenten** Seite, um sicherzustellen, dass Code First die Datenbank mit Daten für "Instructor" mit Anfangsdaten gefüllt. Wenn Sie diese Seite auswählen, dauert möglicherweise einige Minuten, die geladen werden, da der Code First die Datenbank wird erstellt und führt dann die `Seed` Methode. (Es nicht, die ausgeführt werden, wenn Sie auf der Startseite waren, weil die Anwendung versucht nicht, Zugriff auf die Datenbank noch.)

Klicken Sie auf die **Schüler/Studenten** Tab, um sicherzustellen, dass die bereitgestellte Datenbank keine Schüler/Studenten verfügt.

Wählen Sie **hinzufügen Schüler/Studenten** aus der **Schüler/Studenten** Menü ein Schüler/Student hinzufügen, und zeigen Sie den neuen Studenten in der **Schüler/Studenten** Seite, um sicherzustellen, dass Sie erfolgreich in die Datenbank schreiben können .

Von der **Kurse** , wählen Sie im Menü **Update Gutschriften**. Die **Update Gutschriften** Seite sind Administratorberechtigungen erforderlich, sodass die **anmelden** angezeigt wird. Geben Sie Anmeldeinformationen für das Administratorkonto, dass Sie frühere ("Admin" und "Devpwd") erstellt. Die **Update Gutschriften** Seite wird angezeigt, die überprüft, dass das Administratorkonto, das Sie im vorherigen Tutorial erstellt haben, ordnungsgemäß in der testumgebung bereitgestellt wurde.

Überprüfen Sie, ob ein *Elmah* Ordner vorhanden ist, der *c:\inetpub\wwwroot\ContosoUniversity* nur der Platzhalterdatei in ihrem Ordner.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Überprüfen Sie die automatische Web.config-Änderungen für Code First-Migrationen

Öffnen der *"Web.config"* -Datei in der bereitgestellten Anwendung auf *C:\inetpub\wwwroot\ContosoUniversity* und Sie sehen, in denen während des Bereitstellungsvorgangs Code First-Migrationen zu automatisch konfiguriert Aktualisieren Sie die Datenbank, auf die neueste Version.

![](deploying-to-iis/_static/image21.png)

Der Bereitstellungsprozess erstellt auch eine neue Verbindungszeichenfolge für die Code First-Migrationen verwenden ausschließlich für das Datenbankschema zu aktualisieren:

![Database_Publish-Verbindungszeichenfolge](deploying-to-iis/_static/image22.png)

Diese zusätzliche Verbindungszeichenfolge können Sie ein Benutzerkonto für Datenbank-Schema-Updates und ein anderes Benutzerkonto für den Datenzugriff für die Anwendung angeben. Sie könnten z. B. Zuweisen der **Db\_Besitzer** zu Code First-Migrationen, Rolle und **Db\_Datareader** und **Db\_Datawriter**Rollen der Anwendung. Dies ist ein gängiges Defense-in-Depth-Muster, das verhindert, dass potenziell bösartigen Code in der Anwendung das Datenbankschema zu ändern. (Zum Beispiel könnte dies in einen erfolgreichen SQL Injection-Angriff vorkommen.) Dieses Muster wird nicht von diesen Tutorials verwendet. Wenn Sie dieses Muster in Ihrem Szenario implementieren möchten, können Sie dies tun, indem Sie die folgenden Schritte ausführen:

1. In der **Einstellungen** Registerkarte die **Webveröffentlichung** -Assistenten geben Sie die Verbindungszeichenfolge, die ein Benutzer mit vollständigen Berechtigungen zum Aktualisieren Schema gibt an, und Deaktivieren der **diese Verbindungszeichenfolge verwenden zur Laufzeit** Kontrollkästchen. In der bereitgestellten Web.config-Datei, dies ist die `DatabasePublish` Verbindungszeichenfolge.
2. Erstellen Sie eine Web.config-Datei-Transformation für die Verbindungszeichenfolge, die Sie die Anwendung zur Laufzeit verwenden soll.

## <a name="summary"></a>Zusammenfassung

Sie haben nun Ihre Anwendung in IIS auf Ihrem Entwicklungscomputer bereitgestellt und es getestet.

![Startseite im Test](deploying-to-iis/_static/image23.png)

Dies stellt sicher, dass während des Bereitstellungsvorgangs der Inhalt der Anwendung auf den richtigen Speicherort kopiert (mit Ausnahme der Dateien, die Sie nicht bereitstellen möchten), und auch, Web Deploy IIS ordnungsgemäß während der Bereitstellung konfiguriert. Führen Sie im nächsten Tutorial, einem weiteren Test, die eine Bereitstellungsaufgabe findet, die noch nicht geschehen: Festlegen von Berechtigungen für den Ordner für die *Elmah* Ordner.

## <a name="more-information"></a>Weitere Informationen

Informationen zum Ausführen von IIS oder IIS Express in Visual Studio finden Sie unter den folgenden Ressourcen:

- [Übersicht über IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) auf der Website IIS.net.
- [Einführung in IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) auf Scott Guthries Blog.
- [Webserver in Visual Studio für ASP.NET-Webprojekte](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Unterschiede zwischen IIS und ASP.NET Development Server Core-](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) auf der ASP.NET-Website.

Weitere Informationen darüber, welche Probleme auftreten kann, wenn Ihre Anwendung bei mittlerer Vertrauenswürdigkeit ausgeführt wird, finden Sie unter [Hosting von ASP.NET-Anwendungen in mittlere Vertrauensebene](http://www.4guysfromrolla.com/articles/100307-1.aspx) auf die 4 Guys Rolla-Standorts.

> [!div class="step-by-step"]
> [Zurück](project-properties.md)
> [Weiter](setting-folder-permissions.md)
