---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 'ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellen von Test | Microsoft Docs'
author: tdykstra
description: "Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder mit einem Hostinganbieter von Drittanbietern durch wählen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: 01f72e0240e84944f8ffece9a2dbc5802be4646b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a>ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellen von Test
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung eines Drittanbieters Hostinganbieter oder Azure App Service-Web-Apps mithilfe von Visual Studio 2012 oder Visual Studio 2010. Informationen über die Reihen finden Sie unter [im ersten Lernprogramm, in der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

Dieses Lernprogramm zeigt, wie eine ASP.NET-Webanwendung in IIS auf dem lokalen Computer bereitstellen.

Wenn Sie eine Anwendung entwickeln, testen Sie in der Regel in Visual Studio ausführen. Standardmäßig verwenden Webanwendungsprojekte in Visual Studio 2012 als den Development Webserver IIS Express. IIS Express verhält sich wie die vollständige Variante von IIS als Visual Studio Development Server (auch bekannt als Cassini), der Visual Studio 2010 standardmäßig verwendet. Aber weder Entwicklungswebserver funktioniert genauso wie IIS. Daher ist es möglich, dass eine Anwendung ordnungsgemäß ausgeführt wird, wenn Sie in Visual Studio zu testen, jedoch fehl, wenn er für IIS bereitgestellt wird.

Sie können Ihre Anwendung zuverlässiger folgendermaßen testen:

1. Bereitstellen der Anwendung in IIS auf dem Entwicklungscomputer mithilfe desselben Prozesses, der später verwendet werden, die sie in der produktionsumgebung bereitstellen. Sie können konfigurieren, dass Visual Studio, um IIS zu verwenden, wenn Sie ein Webprojekt ausführen, aber auf diese Weise wird das Verfahren zum Bereitstellen nicht testen. Diese Methode überprüft, ob das Verfahren zum Bereitstellen zusätzlich zu überprüfen, das Ihre Anwendung unter IIS ordnungsgemäß ausgeführt wird.
2. Bereitstellen Sie die Anwendung in einer testumgebung, die nahezu identisch mit der produktionsumgebung. Da die produktionsumgebung für diese Lernprogramme Web-Apps in Azure App Service ist, ist die ideale Umgebung für eine weitere Web-app in Azure App Service erstellt. Sie würden diese zweite Web-app nur zu Testzwecken verwenden, aber es würde die gleiche Weise wie die Produktions-Web-app einrichten festgelegt werden.

Option 2 ist die zuverlässigste Möglichkeit zum Testen, und wenn Sie dies tun, Sie müssen nicht unbedingt über option 1. Allerdings, wenn Sie die Bereitstellung einer Drittanbieter-Anbieter Hostingoption 2 möglicherweise nicht praktikabel sein oder möglicherweise viel Leistung beanspruchen, damit dieses Lernprogrammen Reihen beide Methoden anzeigt. Option 2 Anweisungen der [in der Produktionsumgebung bereitstellen](deploying-to-production.md) Lernprogramm.

Weitere Informationen zur Verwendung von Webservern in Visual Studio finden Sie unter [Webserver in Visual Studio für ASP.NET-Webprojekte](https://msdn.microsoft.com/library/58wxa9w5.aspx).

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](troubleshooting.md).

## <a name="install-iis"></a>Installieren von IIS

Um IIS auf dem Entwicklungscomputer bereitzustellen, benötigen Sie IIS und Web Deploy installiert. Web Deploy wird standardmäßig mit Visual Studio installiert, aber IIS ist nicht in der Standardkonfiguration für Windows 8 oder Windows 7 enthalten. Wenn IIS bereits installiert haben und der Standardanwendungspool in .NET 4 bereits festgelegt ist, fahren Sie mit [im nächsten Abschnitt](#sqlexpress).

1. Mithilfe der [Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx) ist die bevorzugte Möglichkeit, IIS und Web Deploy, installiert werden, da der Webplattform-Installer eine empfohlene Konfiguration für IIS installiert und automatisch die erforderlichen Komponenten für IIS und Web installiert Stellen Sie bei Bedarf bereit.

    Verwenden Sie den folgenden Link zum Ausführen von Webplattform-Installer zum Installieren von IIS und Web Deploy. Wenn Sie IIS, Web Deploy oder ihre erforderlichen Komponenten bereits installiert haben, installiert der Webplattform-Installer nur was fehlt.

    - [Installieren von IIS und Web Deploy mithilfe von WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

    Sie sehen, die darauf hinweist, dass IIS 7 installiert werden soll. Die Verknüpfung funktioniert für IIS 8 unter Windows 8, jedoch für Windows 8, stellen Sie sicher, dass ASP.NET 4.5 installiert ist, indem Sie die folgenden Schritte ausführen:

    1. Open **Systemsteuerung**, **Programme und Funktionen**, **Windows-Funktionen ein- oder ausschalten**.
    2. Erweitern Sie **Internetinformationsdienste (IIS)**, **WWW-Dienste**, und **Anwendungsentwicklungsfeatures**.
    3. Stellen Sie sicher, dass **ASP.NET 4.5** ausgewählt ist.

        ![ASP.NET 4.5 auswählen](deploying-to-iis/_static/image1.png)

Führen Sie nach der Installation von IIS, **IIS-Manager** um sicherzustellen, dass .NET Framework, Version 4 der Standardanwendungspool zugewiesen ist.

1. Drücken Sie WINDOWS + R, öffnen Sie die **ausführen** (Dialogfeld).

    (Oder geben Sie im Windows 8 die "run" auf die **starten** Seite, oder wählen Sie in Windows 7 **ausführen** aus der **starten** Menü. Wenn **ausführen** befindet sich nicht in der **starten** im Menü der Taskleiste mit der rechten Maustaste klicken Sie auf **Eigenschaften**, wählen die **Startmenü** auf **Anpassen**, und wählen Sie **Ausführungsbefehl**.)
2. Geben Sie "Inetmgr", und klicken Sie dann auf **OK**.
3. In der **Verbindungen** Bereich, erweitern Sie den Serverknoten, und wählen Sie **Anwendungspools**. In der **Anwendungspools** Bereich, wenn **DefaultAppPool** ist .NET Frameworkversion 4, wie in der folgenden Abbildung zugewiesen, mit dem nächsten Abschnitt überspringen.

    [![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)
4. Wenn Sie nur zwei Anwendungspools finden Sie unter, und beide .NET Framework 2.0 festgelegt sind, müssen Sie ASP.NET 4 in IIS zu installieren.

    Windows 8 finden Sie unter den Anweisungen im vorherigen Abschnitt für sichergestellt wird, ASP.NET 4.5 installiert ist, oder finden Sie unter [diesem KB-Artikel](https://support.microsoft.com/kb/2736284). Für Windows 7, öffnen Sie ein Eingabeaufforderungsfenster mit der rechten Maustaste **Eingabeaufforderung** in Windows **starten** Menü- und Auswählen von **als Administrator ausführen**. Führen Sie [Aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) So installieren Sie ASP.NET 4 in IIS mit den folgenden Befehlen. (Ersetzen Sie im 32-Bit-Systemen "Framework64" mit "Framework").

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    Dieser Befehl erstellt neuen Anwendungspools für .NET Framework 4, aber der Standardanwendungspool wird weiterhin auf 2.0 festgelegt. Sie müssen eine Anwendung bereitstellen werden, dessen Ziel .NET 4 für diesen Anwendungspool müssen Sie den Anwendungspool in .NET 4 ändern.
5. Wenn Sie geschlossen **IIS-Manager**, führen Sie es erneut aus, erweitern Sie den Serverknoten, und klicken Sie auf **Anwendungspools** zum Anzeigen der **Anwendungspools** erneut auf den Bereich.
6. In der **Anwendungspools** Bereich, klicken Sie auf **DefaultAppPool**, und klicken Sie dann in der **Aktionen** Bereich auf **Grundeinstellungen**.

    [![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)
7. In der **Anwendungspool bearbeiten** ändern Sie im Dialogfeld **.NET Framework-Version** auf **.NET Framework-v4.0.30319** , und klicken Sie auf **OK**.

    [![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)

IIS ist nun bereit für eine Webanwendung zu veröffentlichen, aber bevor Sie dies tun können müssen Sie die Datenbanken, die Sie verwenden in der testumgebung erstellen.

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a>Installieren Sie SQL Server Express

LocalDB ist nicht für die Verwendung konzipiert in IIS für Ihre testumgebung müssen Sie SQL Server Express installiert haben. Wenn Sie Visual Studio 2010 SQL Server Express verwenden, ist standardmäßig bereits installiert. Wenn Sie Visual Studio 2012 verwenden, müssen Sie es installieren.

Um SQL Server Express zu installieren, installieren Sie ihn von [Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) durch Klicken auf [ENU\x64\SQLEXPR\_X64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe) oder [ ENU\x86\SQLEXPR\_X86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe). Falls die falsche Datei gewünscht für Ihr System schlägt fehl installieren und Sie können versuchen, eine andere.

Klicken Sie auf der ersten Seite des SQL Server-Installationscenter auf **eigenständige neue SQL Server-Installation oder Hinzufügen von Funktionen zu einer vorhandenen Installation**, und befolgen Sie die Anweisungen, akzeptieren die Standardoptionen. Übernehmen Sie die Standardeinstellungen im Installations-Assistenten. Weitere Informationen zu Installationsoptionen finden Sie unter [Installieren von SQL Server 2012 vom Installations-Assistenten (Setup)](https://msdn.microsoft.com/library/ms143219.aspx).

## <a name="create-sql-server-express-databases-for-the-test-environment"></a>Erstellen von SQL Server Express-Datenbanken für die testumgebung

Die University Contoso-Anwendung verfügt über zwei Datenbanken: der Mitgliedschaftsdatenbank und der Anwendungsdatenbank. Sie können diese Datenbanken auf zwei separaten Datenbanken oder auf eine einzelne Datenbank bereitstellen. Möglicherweise möchten sie kombinieren, um Datenbankjoins zwischen Ihrer Anwendung und Ihre Mitgliedschaftsdatenbank zu ermöglichen. Wenn Sie mit einem Drittanbieter-Hostinganbieter bereitstellen, möglicherweise Ihren hostingplan auch einen Grund für diese kombinieren angeben. Z. B. des hostenden Anbieters möglicherweise fallen Kosten für mehrere Datenbanken oder möglicherweise mehr als eine Datenbank selbst nicht zulässig.

In diesem Lernprogramm stellen Sie mit zwei Datenbanken in der testumgebung, und klicken Sie auf eine Datenbank in der Staging- und produktionsumgebungen bereit.

Aus der **Ansicht** in Visual Studio wählen Sie im Menü **Server-Explorer** (**Datenbank-Explorer** in Visual Web Developer), und klicken Sie dann mit der rechten Maustaste **Datenverbindungen**  , und wählen Sie **neue SQL Server-Datenbank erstellen**.

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

In der **neue SQL Server-Datenbank erstellen** Dialogfeld Geben Sie ". \SQLExpress" in der **Servernamen** Feld und "Aspnet-ContosoUniversity" in der **neuer Datenbankname** Feld, klicken Sie dann Klicken Sie auf **OK**.

![Erstellen von Aspnet-ContosoUniversity](deploying-to-iis/_static/image9.png)

Führen Sie das gleiche Verfahren zum Erstellen einer neuen SQL Server Express School-Datenbank mit dem Namen "ContosoUniversity" aus.

**Server-Explorer** zeigt nun zwei neuen Datenbanken.

![Neue Datenbanken im Server-Explorer](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a>Erstellen Sie eine Grant-Skript für die neuen Datenbanken

Wenn die Anwendung in IIS auf dem Entwicklungscomputer ausgeführt wird, greift auf die Anwendung die Datenbank mithilfe von Anmeldeinformationen für den Standardanwendungspool. Standardmäßig haben die Identität des Anwendungspools jedoch keine Berechtigung zum Öffnen von Datenbanken. Daher müssen Sie zum Ausführen eines Skripts, um diese Berechtigung zu erteilen. In diesem Abschnitt erstellen Sie das Skript ausgeführt wird weiter unten, um sicherzustellen, dass die Anwendung die Datenbanken öffnen kann, wenn sie in IIS ausgeführt wird.

Mit der rechten Maustaste in der Projektmappe (nicht eines der Projekte), und klicken Sie auf **neues Element hinzufügen**, und erstellen Sie ein neues **SQL-Datei** mit dem Namen *Grant.sql*. Kopieren Sie die folgenden SQL-Befehle in die Datei, und speichern Sie und schließen Sie die Datei:

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> Dieses Skript dient zum Arbeiten mit SQL Server Express 2012 und IIS-Einstellungen in Windows 8 oder Windows 7, wie sie in diesem Lernprogramm angegeben sind. Wenn Sie eine andere Version von SQL Server oder von Windows verwenden, oder wenn Sie IIS auf Ihrem Computer unterschiedlich eingerichtet, Änderungen an diesem Skript möglicherweise erforderlich. Weitere Informationen zu SQL Server-Skripts finden Sie unter [SQL Server-Onlinedokumentation](https://go.microsoft.com/fwlink/?LinkId=132511).


> [!NOTE] 
> 
> **Sicherheitshinweis** dieses Skript gibt Db\_kontobesitzerberechtigungen verfügen, um den Benutzer, die zur Laufzeit in der produktionsumgebung müssen also auf die Datenbank zugreift. In einigen Szenarien empfiehlt es sich um ein Benutzer mit vollständigen Datenbankschema aktualisieren Sie Berechtigungen nur für die Bereitstellung, und geben Sie einen anderen Benutzer, der berechtigt nur zum Lesen und Schreiben von Daten zur Laufzeit. Weitere Informationen finden Sie unter [überprüfen die automatischen Änderungen von "Web.config" für die Code First-Migrationen](#reviewingmigrations) weiter unten in diesem Lernprogramm.


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a>Führen Sie das Skript zum Erteilen der Anwendungsdatenbank

Sie können konfigurieren, dass dem Veröffentlichungsprofil ein, um das Skript zum Erteilen in der Mitgliedschaftsdatenbank während der Bereitstellung ausgeführt werden, da die datenbankbereitstellung der DbDacFx-Anbieter verwendet. Während der Bereitstellung von Code First-Migrationen, die ist, wie Sie die Anwendungsdatenbank bereitstellen, können Sie können keine Skripts ausführen. Aus diesem Grund müssen Sie das Skript vor der Bereitstellung in der Anwendungsdatenbank manuell ausführen.

1. Öffnen Sie in Visual Studio die *Grant.sql* -Datei, die Sie zuvor erstellt haben.
2. Klicken Sie auf **verbinden**. 

    ![Schaltfläche "Verbinden"](deploying-to-iis/_static/image11.png)
3. In der **Verbindung mit Server herstellen** Dialogfeld Geben Sie *. \SQLExpress* als die **Servernamen**, und klicken Sie dann auf **verbinden**.
4. Wählen Sie in der Datenbank-Dropdownliste **ContosoUniversity**, und klicken Sie dann auf **Execute**. 

    ![](deploying-to-iis/_static/image12.png)

Die Standardidentität des Anwendungspools verfügt jetzt über ausreichende Berechtigungen in der Anwendungsdatenbank für Code First-Migrationen Tabellen der Datenbank zu erstellen, wenn die Anwendung ausgeführt wird.

## <a name="publish-to-iis"></a>In IIS veröffentlichen

Es gibt mehrere Möglichkeiten, die Sie in IIS mithilfe von Visual Studio und Web Deploy bereitstellen können:

- Verwenden Sie Visual Studio One-Click-Veröffentlichung.
- Veröffentlichen Sie über die Befehlszeile.
- Erstellen einer *Bereitstellungspaket* und installieren Sie es mithilfe der IIS-Manager-UI. Das Bereitstellungspaket besteht aus einem *ZIP* Datei, enthält alle Dateien und Metadaten, die zum Installieren eines Standorts in IIS erforderlich.
- Erstellen Sie ein Bereitstellungspaket, und installieren Sie es über die Befehlszeile.

Der Prozess, den Sie in den vorherigen Lernprogrammen zum Visual Studio einrichten, damit automatisieren gab es Bereitstellungsaufgaben gilt für alle diese Methoden. In diesen Lernprogrammen verwenden Sie die ersten beiden dieser Methoden. Informationen zur Verwendung von Bereitstellungspaketen finden Sie unter [Bereitstellen einer Web-Anwendung erstellen und Installieren eines Webbereitstellungspakets](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) in der Web Content Bereitstellungskarte für Visual Studio und ASP.NET.

Stellen Sie vor der Veröffentlichung sicher, dass Sie Visual Studio im Administratormodus ausgeführt werden. Wenn Sie nicht sehen **(Administrator)** in der Titelleiste, schließen Sie Visual Studio. In der Windows Server 8 **starten** Seiten- oder Windows 7 **starten** im Menü Maustaste auf das Symbol für die Version von Visual Studio, die Sie verwenden, und wählen Sie **als Administrator ausführen**. Im Administratormodus ist erforderlich für die Veröffentlichung nur wenn Sie in IIS auf dem lokalen Computer veröffentlichen.

### <a name="create-the-publish-profile"></a>Erstellen Sie das Veröffentlichungsprofil

1. In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt ContosoUniversity (nicht auf das Projekt ContosoUniversity.DAL), und wählen Sie **veröffentlichen**.

    Die **Web veröffentlichen** -Assistent wird angezeigt.

    ![Veröffentlichen Sie die Registerkarte "Web-Assistent-Profil"](deploying-to-iis/_static/image13.png)
2. Wählen Sie in der Dropdown-Liste  **&lt;neu... &gt;**. (Mit dem aktuellen Visual Studio-Update installiert, keine Dropdown-Liste vorhanden ist, und die Schaltfläche klicken, um ein neues Profil von Grund auf neu zu erstellen ist **benutzerdefinierte**.)
3. In der **neues Profil** (Dialogfeld), geben Sie "Test", und klicken Sie dann auf **OK**.

    Der Assistent springt automatisch zu den **Verbindung** Registerkarte.
4. In der **Dienst-URL** geben *"localhost"*.
5. In der **Website/Anwendung** geben *Default Web Site/ContosoUniversity*
6. In der **Ziel-URL** Geben Sie`http://localhost/ContosoUniversity`

    Die **Ziel-URL** Einstellung ist nicht erforderlich. Wenn Visual Studio abgeschlossen ist, die Anwendung bereitstellen, wird Ihrem Standardbrowser an diese URL automatisch geöffnet. Wenn Sie nicht möchten, dass den Browser nach der Bereitstellung nicht automatisch geöffnet wird, lassen Sie dieses Feld leer.
7. Klicken Sie auf **Validate Connection** um sicherzustellen, dass die Einstellungen richtig sind und Sie können mit IIS auf dem lokalen Computer verbinden.

    Ein grünes Häkchen überprüft, ob die Verbindung erfolgreich ist.

    ![Veröffentlichen von Web-Assistent – Registerkarte "Datenbankverbindung"](deploying-to-iis/_static/image14.png)
8. Klicken Sie auf **Weiter** um zum Wechseln der **Einstellungen** Registerkarte.
9. Die **Konfiguration** Dropdown-Feld gibt die Buildkonfiguration bereitstellen. Lassen Sie auf den Standardwert der Veröffentlichung festgelegt. Sie wird nicht in diesem Lernprogramm Debug-Builds bereitstellen, werden.
10. Erweitern Sie **Dateiveröffentlichungsoptionen**, und wählen Sie dann **Ausschließen von Dateien aus der App\_Datenordner**.

    In der testumgebung, die Sie in der lokalen SQL Server Express-Instanz erstellten Datenbanken wird auf die Anwendung zugreifen, nicht die MDF-Dateien in den *App\_Daten* Ordner.
11. Lassen Sie die **Precompile während der Veröffentlichung** und **entfernen weiterer Dateien am Ziel** Kontrollkästchen deaktiviert.

    ![Dateiveröffentlichungsoptionen in der Registerkarte "Einstellungen"](deploying-to-iis/_static/image15.png)

    Vorkompilieren von ist eine Option, die hauptsächlich für sehr große Websites hilfreich sind. Sie reduzieren Seite Startzeit zum ersten Mal, wenn eine Seite angefordert wird, nachdem der Standort veröffentlicht wird.

    Sie müssen zusätzliche Dateien entfernt werden, da dies Ihre erste Bereitstellung ist und Sie es keine Dateien im Zielordner noch nicht.

    > [!NOTE] 
    > 
    > [!CAUTION]
    > Bei Auswahl des **entfernen weiterer Dateien** für eine nachfolgende Bereitstellung am selben Standort, stellen Sie sicher, dass Sie die Vorschaufunktion verwenden, damit Sie rechtzeitig sehen, welche Dateien vor der Bereitstellung gelöscht werden. Das erwartete Verhalten ist, dass Web Deploy-Dateien auf dem Zielserver gelöscht werden, die Sie in Ihrem Projekt gelöscht haben. Allerdings wird die gesamte Ordnerstruktur in den Quell- und Zielschemas Ordnern verglichen, und in einigen Szenarien Web Deploy-Dateien, die Sie löschen möchten, nicht löschen kann.
    > 
    > Wenn Sie beim Bereitstellen eines Projekts in den Stammordner in einem Unterordner auf dem Server über eine Webanwendung verfügen, wird z. B. der Unterordner gelöscht. Ein Projekt für die main-Website unter "contoso.com" und ein anderes Projekt für einen Blog unter Objekte Remote möglicherweise. Die Bloganwendung ist in einem Unterordner. Wenn Sie entfernen Weitere Dateien am Ziel auswählen, wenn Sie auf der Hauptwebsite bereitstellen, wird die Bloganwendung gelöscht werden.
    > 
    > Ein weiteres Beispiel Ihrer App\_Datenordner möglicherweise unerwartet gelöscht. Bestimmte Datenbanken wie z. B. SQL Server Compact-Datenbankdateien speichern, in der App\_Datenordner. Nach der ersten Bereitstellung möchte Sie nicht die Datenbankdateien in nachfolgenden Bereitstellungen kopiert beibehalten, sodass Sie ausschließen App auswählen,\_Daten auf der Registerkarte "Web packen/veröffentlichen". Nachdem Sie, dass nicht tun, wenn Sie entfernen weiterer Dateien am Ziel ausgewählt haben, die Datenbankdateien und die App\_Datenordner selbst werden gelöscht, wenn Sie das nächste Mal, die Sie veröffentlichen.

### <a name="configure-deployment-for-the-membership-database"></a>Konfigurieren der Bereitstellung für die Mitgliedschaftsdatenbank

Die folgenden Schritte gelten für die **DefaultConnection** -Datenbank in die **Datenbanken** Abschnitt des Dialogfelds.

1. In der **Remote Verbindungszeichenfolge** Geben Sie die folgende Verbindungszeichenfolge, die auf die neue SQL Server Express Mitgliedschaftsdatenbank verweist.

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    Der Bereitstellungsprozess wird diese Verbindungszeichenfolge in der bereitgestellten Datei "Web.config" versetzt, da **verwenden diese Verbindungszeichenfolge zur Laufzeit** ausgewählt ist.

    Sie können auch abrufen die Verbindungszeichenfolge vom **Server-Explorer**. In **Server-Explorer**, erweitern Sie **Datenverbindungen** , und wählen Sie die  **&lt;Machinename&gt;\sqlexpress.aspnet-ContosoUniversity** Datenbank, klicken Sie dann aus der **Eigenschaften** Fenster Kopie der **Verbindungszeichenfolge** Wert. Dass die Verbindungszeichenfolge eine zusätzliche Einstellung verfügt, die Sie löschen können: `Pooling=False`.
2. Wählen Sie **Datenbank aktualisieren**.

    Dadurch wird das Datenbankschema in der Zieldatenbank, die während der Bereitstellung erstellt werden. In den folgenden Schritten Geben Sie die zusätzliche Skripts, die Sie ausführen müssen: eine Datenbank Zugriff auf den Standardanwendungspool und eins, um Daten bereitzustellen.
3. Klicken Sie auf **Datenbankupdates konfigurieren**.
4. In der **Datenbankupdates konfigurieren** (Dialogfeld), klicken Sie auf **SQL-Skript hinzufügen** und navigieren Sie zu der *Grant.sql* Skript, das Sie zuvor im Projektmappenordner gespeichert.
5. Wiederholen Sie den Vorgang zum Hinzufügen der *Aspnet-Daten-dev.sql* Skript.

    ![Konfigurieren von Datenbank-Updates für die Mitgliedschaftsdatenbank](deploying-to-iis/_static/image16.png)
6. Klicken Sie auf **Schließen**.

### <a name="configure-deployment-for-the-application-database"></a>Konfigurieren der Bereitstellung für die Datenbank der dienstanwendung

Wenn Visual Studio eine Entity Framework erkennt `DbContext` -Klasse, erstellt es einen Eintrag in der **Datenbanken** Abschnitt durch, ein **führen Sie Code First-Migrationen** Kontrollkästchen anstelle von einer  **Aktualisieren der Datenbank** Kontrollkästchen. Für dieses Lernprogramm verwenden Sie das Kontrollkästchen Code First-Migrationen Bereitstellung angeben.

In einigen Szenarien möglicherweise verwenden Sie eine `DbContext` Datenbank, aber Sie DbDacFx-Anbieter anstelle Migrationen zu verwenden, um die Datenbank bereitstellen möchten. In diesem Fall finden Sie unter [wie ich eine Code First Datenbank ohne Migrationen bereitstellen?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) in der ASP.NET Web-Bereitstellung – häufig gestellte Fragen in MSDN.

Die folgenden Schritte gelten für die **SchoolContext** -Datenbank in die **Datenbanken** Abschnitt des Dialogfelds.

1. In der **Remote Verbindungszeichenfolge** Geben Sie die folgende Verbindungszeichenfolge, die auf die neue Datenbank für SQL Server Express-Anwendung verweist.

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    Der Bereitstellungsprozess wird diese Verbindungszeichenfolge in der bereitgestellten Datei "Web.config" versetzt, da **verwenden diese Verbindungszeichenfolge zur Laufzeit** ausgewählt ist.

    Sie erhalten auch die Verbindungszeichenfolge für die Anwendung von **Server-Explorer** Datenbankverbindungszeichenfolge für die gleiche Weise, die Sie erhalten die Mitgliedschaft haben.
2. Wählen Sie **führen Sie Code First-Migrationen (wird beim Anwendungsstart ausgeführt)**.

    Diese Option bewirkt, dass der Bereitstellungsprozess so konfigurieren Sie die bereitgestellte Datei "Web.config" Angeben der `MigrateDatabaseToLatestVersion` Initialisierer. Diese Initialisierer aktualisiert die Datenbank automatisch auf die neueste Version, wenn die Anwendung Zugriff auf die Datenbank zum ersten Mal nach der Bereitstellung.

### <a name="configure-publish-profile-transforms"></a>Konfigurieren von Transformationen Profil veröffentlichen

1. Klicken Sie auf **schließen**, und klicken Sie dann auf **Ja** Wenn werden Sie gefragt, ob Sie Änderungen speichern möchten.
2. In **Projektmappen-Explorer**, erweitern Sie **Eigenschaften**, erweitern Sie **PublishProfiles**.
3. "Rright" Klick *Test.pubxml,* , und klicken Sie dann auf **Config Transformation hinzufügen**.

    ![Konfigurationstransformation-Menü "hinzufügen"](deploying-to-iis/_static/image17.png)

    Visual Studio erstellt die *Web.Test.config* Transform-Datei und öffnet diese.
4. In der *Web.Test.config* Transformationsdatei, fügen Sie den folgenden Code unmittelbar nach dem Öffnen Konfigurationstag.

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    Wenn Sie den Test verwenden Veröffentlichungsprofil, diese Transformation legt den Indikator Umgebung "Test". In der bereitgestellten Website sehen Sie nach der "Contoso University" H1-Überschrift "(Test)".
5. Speichern und schließen Sie die Datei.
6. Mit der rechten Maustaste die *Web.Test.config* Datei, und klicken Sie auf **Vorschau transformieren** um sicherzustellen, dass die Transformation, die Sie codiert die erwartete Änderungen erzeugt.

    Die **"Web.config" Vorschau** Fenster zeigt das Ergebnis des Anwendens sowohl die *Web.Release.config* transformiert und die *Web.Test.config* transformiert.

### <a name="preview-the-deployment-updates"></a>Die Bereitstellungsupdates in der Vorschau anzeigen

1. Öffnen der **Web veröffentlichen** Assistenten erneut aus (mit der rechten Maustaste des ContosoUniversity-Projekts, und klicken Sie auf **veröffentlichen**).
2. In der **Vorschau** Registerkarte, stellen Sie sicher, dass die **Test** Profil ist weiterhin aktiviert, und klicken Sie dann auf **starten Preview** um eine Liste der Dateien anzuzeigen, die kopiert werden.

    ![Schaltfläche "Vorschau"](deploying-to-iis/_static/image18.png)

    ![Veröffentlichungsvorschau](deploying-to-iis/_static/image19.png)

    Sie können auch klicken Sie auf die **vorschaudatenbank** Link, um die Skripts anzuzeigen, die in der Mitgliedschaftsdatenbank ausgeführt wird. (Keine Skripts werden für die Bereitstellung von Code First-Migrationen ausgeführt, sodass es nichts zum für die Datenbank der dienstanwendung in der Vorschau anzeigen ist.)
3. Klicken Sie auf **Veröffentlichen**.

    Wenn Visual Studio nicht im Administratormodus ist, erhalten Sie möglicherweise eine Fehlermeldung angezeigt, die einen Berechtigungsfehler angibt. In diesem Fall schließen Sie Visual Studio, öffnen Sie es im Administratormodus und versuchen Sie, erneut zu veröffentlichen.

    Wenn Visual Studio im Administratormodus, ist die **Ausgabe** Fenster Berichte erfolgreich erstellen und veröffentlichen.

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    Wenn Sie die URL im eingegeben haben die **Ziel-URL** auf dem Veröffentlichungsprofil ein **Verbindung** Registerkarte Browser automatisch geöffnet wird, die Contoso University-Startseite in IIS auf dem lokalen Computer ausgeführt.

## <a name="test-in-the-test-environment"></a>In der testumgebung zu testen

Beachten Sie, dass der Indikator für die Umgebung "(Test)" zeigt anstelle von "(Dev)", die zeigt, dass die *"Web.config"* Transformation für den Indikator für die Umgebung erfolgreich war.

Führen Sie die **Lehrkräfte** Seite, um sicherzustellen, dass Code First der Datenbank mit Instructor-Daten mit Anfangsdaten gefüllt. Wenn Sie diese Seite auswählen, dauert möglicherweise einige Minuten, bis die geladen werden, da der Code First erstellt die Datenbank und führt dann die `Seed` Methode. (sie haben nicht, die ausgeführt werden, wenn Sie auf der Startseite waren, weil die Anwendung versucht hat nicht, Zugriff auf die Datenbank noch.)

Klicken Sie auf die **Studenten** Registerkarte ", um sicherzustellen, dass die bereitgestellte Datenbank kein Studenten hat.

Wählen Sie **Studenten hinzufügen** aus der **Studenten** Menü eine Student hinzufügen, und zeigen Sie die neue Studenten in die **Studenten** Seite, um sicherzustellen, dass Sie erfolgreich in die Datenbank geschrieben werden können .

Aus der **Kurse** klicken Sie im Menü **Update Gutschriften**. Die **Update Gutschriften** Seite erfordert Administratorberechtigungen vorhanden sind, sodass der **anmelden** angezeigt wird. Geben Sie die Anmeldeinformationen des Administratorkontos, die Sie früher ("Admin" und "Devpwd") erstellt. Die **Update Gutschriften** Seite wird angezeigt, die überprüft, ob das Administratorkonto, das Sie im vorherigen Lernprogramm erstellt ordnungsgemäß in der testumgebung bereitgestellt wurde.

Überprüfen Sie, ob ein *Elmah* Ordner vorhanden ist, der *c:\inetpub\wwwroot\ContosoUniversity* mit nur die Platzhalterdatei in diesem Ordner.

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a>Überprüfen Sie die automatischen Änderungen von "Web.config" für die Code First-Migrationen

Öffnen der *"Web.config"* -Datei in der bereitgestellten Anwendung auf *C:\inetpub\wwwroot\ContosoUniversity* sehen Sie, auf dem der Bereitstellungsprozess Code First-Migrationen zu automatisch konfiguriert Aktualisieren Sie die Datenbank, auf die neueste Version.

![](deploying-to-iis/_static/image21.png)

Der Bereitstellungsprozess erstellt auch eine neue Verbindungszeichenfolge für die Code First-Migrationen verwenden ausschließlich für das Datenbankschema zu aktualisieren:

![Database_Publish-Verbindungszeichenfolge](deploying-to-iis/_static/image22.png)

Diese zusätzliche Verbindungszeichenfolge können Sie ein Benutzerkonto für Datenbank-Schema-Updates und ein anderes Benutzerkonto für den Datenzugriff für die Anwendung angeben. Sie könnten z. B. Zuweisen der **Db\_Besitzer** Rolle Code First-Migrationen und **Db\_Datareader** und **Db\_Datawriter**Rollen der Anwendung. Dies ist ein allgemeines Verteidigung Muster, das verhindert, potenziell bösartigen Code in der Anwendung dass über das Datenbankschema zu ändern. (Beispielsweise kann dies auf einen erfolgreichen SQL Injection-Angriff vorkommen.) Dieses Muster wird nicht von diesen Lernprogrammen verwendet werden. Wenn Sie dieses Muster in Ihrem Szenario implementieren möchten, können Sie dies tun, indem Sie die folgenden Schritte ausführen:

1. In der **Einstellungen** auf der Registerkarte die **Web veröffentlichen** -Assistenten geben Sie die Verbindungszeichenfolge, die ein Benutzer mit vollständigen Berechtigungen zum Aktualisieren Schema gibt an, und Deaktivieren der **diese Verbindungszeichenfolge verwenden zur Laufzeit** Kontrollkästchen. In der bereitgestellten Datei "Web.config" Dies ist die `DatabasePublish` Verbindungszeichenfolge.
2. Erstellen Sie eine Web.config-Datei-Transformation für die Verbindungszeichenfolge, die die Anwendung zur Laufzeit verwendet werden soll.

## <a name="summary"></a>Zusammenfassung

Sie haben jetzt Ihre Anwendung in IIS auf dem Entwicklungscomputer bereitgestellt und es getestet.

![Startseite im Test](deploying-to-iis/_static/image23.png)

Dies stellt sicher, dass der Bereitstellungsprozess Inhalt der Anwendung auf den richtigen Speicherort kopiert (mit Ausnahme der Dateien, die Sie nicht bereitstellen möchten), und auch, Web Deploy IIS ordnungsgemäß während der Bereitstellung konfiguriert. In den nächsten Lernprogrammen, führen Sie eine weitere Tests, die eine Bereitstellungsaufgabe findet, die noch nicht durchgeführt: Festlegen von Berechtigungen für Ordner für die *Elmah* Ordner.

## <a name="more-information"></a>Weitere Informationen

Informationen zum Ausführen von IIS oder IIS Express in Visual Studio finden Sie unter den folgenden Ressourcen:

- [Übersicht über IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) auf der Website IIS.net.
- [Einführung in IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) Scott Guthries Blog.
- [Webserver in Visual Studio für ASP.NET-Webprojekte](https://msdn.microsoft.com/library/58wxa9w5.aspx).
- [Unterschiede zwischen IIS und ASP.NET Development Server Core](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) auf der ASP.NET-Website.

Informationen darüber, welche Probleme auftreten kann, wenn Ihre Anwendung in mittlerer Vertrauenswürdigkeit ausgeführt wird, finden Sie unter [Hosten von ASP.NET-Anwendungen im mittlere Vertrauensebene](http://www.4guysfromrolla.com/articles/100307-1.aspx) auf die 4 Guys von Rolla-Website.

>[!div class="step-by-step"]
[Zurück](project-properties.md)
[Weiter](setting-folder-permissions.md)
