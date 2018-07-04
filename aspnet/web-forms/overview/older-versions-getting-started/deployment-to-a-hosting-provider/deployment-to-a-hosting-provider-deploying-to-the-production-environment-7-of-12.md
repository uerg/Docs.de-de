---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellung in der Produktionsumgebung - 7 von 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: 26a832fd336f886ba1ddfb1682930afa4df56c58
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383635"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellung in der Produktionsumgebung - 7 von 12
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, das eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie die Web Publish Update installieren. Eine Einführung in die Reihe, finden Sie unter [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, das zeigt, Bereitstellungsfunktionen, die nach der RC-Version von Visual Studio 2012 eingeführt wurden, zeigt, wie zum Bereitstellen von SQL Server-Editionen als SQL Server Compact und zeigt, wie Sie in Azure App Service-Web-Apps bereitstellen, finden Sie unter [ASP.NET-webbereitstellung Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

In diesem Tutorial haben Sie ein Konto bei einem Hostinganbieter Online einrichten und Bereitstellen Ihrer ASP.NET Web-Anwendung in der produktionsumgebung mit nur einem Klick über das Visual Studio veröffentlichen Features.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Einen Hosting-Anbieter auswählen

Für die Contoso University-Anwendung und dieser Reihe von Tutorials benötigen Sie einen Anbieter, der ASP.NET 4 und Web Deploy unterstützt. Einem bestimmten Hostinganbieter wurde ausgewählt, sodass in den Tutorials veranschaulichen, der alle Funktionen nutzen, der Bereitstellung in einer live-Website konnte. Jede Hostingunternehmen verfügt über verschiedene Funktionen, und die benutzerfreundlichkeit der Bereitstellung von auf ihren Servern variiert ein wenig. In diesem Tutorial beschriebene Prozess ist jedoch meist für den gesamten Prozess. Der hosting-Anbieter verwendet, die für dieses Tutorial Cytanium.com, ist einer von vielen, die verfügbar sind stellt, und seine Verwendung in diesem Lernprogramm keine Befürwortung oder Empfehlung.

Wenn Sie auf Ihrem eigenen Hostinganbieter bereit sind, können Sie vergleichen, Features und Preise in der [Katalog von Anbietern](https://www.microsoft.com/web/hosting) auf der Website Microsoft.com/Web.

## <a name="creating-an-account"></a>Erstellen eines Kontos

Erstellen Sie ein Konto bei Ihrem ausgewählten Anbieter an. Wenn Unterstützung für die vollständige SQL Server-Datenbank eine hinzugefügte zusätzliche, Sie müssen nicht für die Auswahl für dieses Tutorial, aber Sie benötigen sie für die [Migrieren zu SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) Tutorial weiter unten in dieser Serie.

Für diese Tutorials müssen Sie einen neuen Domänennamen zu registrieren. Sie können testen, um die erfolgreichen Bereitstellung zu überprüfen, indem Sie mit der temporären URL, die vom Anbieter dem Standort zugewiesen werden soll.

Nachdem das Konto erstellt wurde, erhalten Sie in der Regel eine e-Mail zur Begrüßung, die alle Informationen enthält, die Sie zum Bereitstellen und Verwalten Ihrer Website benötigen. Die Informationen, die der Hostinganbieter zur Verfügung, die Sie gesendet werden ähnlich wie hier gezeigt. Cytanium e-Mail zur Begrüßung, die auf neuen Kontobesitzer gesendet wird, enthält die folgenden Informationen:

- Die URL des Anbieters Bereich Steuerelementsite, dem Sie die Einstellungen für Ihre Website verwalten können. Die ID und das angegebene Kennwort sind in diesem Teil der e-Mail zur Begrüßung als einfachen Verweis enthalten. (Beide wurden in einer Demo-Wert in dieser Abbildung geändert.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- Die standardmäßige .NET Framework-Version und Informationen zum Ändern. Viele hosting Websites werden standardmäßig auf 2.0, dies funktioniert für ASP.NET-Anwendungen, die mit der Zielplattform .NET Framework 2.0, 3.0 oder 3.5. Contoso University ist jedoch eine .NET Framework 4-Anwendung, sodass Sie diese Einstellung zu ändern. (Für eine ASP.NET 4.5-Anwendung würden Sie die .NET 4.0-Einstellung verwenden.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- Die temporären URL, die Sie verwenden können, um Ihre Website zugreifen. Wenn dieses Konto erstellt wurde, wurde "contosouniversity.com" mit dem Namen der vorhandenen Domäne eingegeben. Aus diesem Grund wird von die temporäre URL `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Informationen zum Einrichten von Datenbanken und die Verbindungszeichenfolgen, die Sie benötigen, um darauf zugreifen:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Informationen zu Tools und Einstellungen für das Bereitstellen einer Website. (Die e-Mail in Cytanium erwähnt auch WebMatrix, das hier ausgelassen wird).

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Festlegen der .NET Framework-Version

E-Mail zur Begrüßung Cytanium enthält einen Link zu Anweisungen zum Ändern der Version von .NET Framework. Diese Anweisungen Erläutern Sie, dass dies über die Systemsteuerung Cytanium durchgeführt werden kann. Andere Anbieter haben Control Panel-Websites, die sieht anders aus, oder sie möglicherweise weisen Sie hierzu auf andere Weise.

Wechseln Sie zu der die systemsteuerungs-URL. Nach der Anmeldung mit Ihrem Benutzernamen und Kennwort sehen Sie die Systemsteuerung.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

In der **Hosting Leerzeichen** , halten Sie den Zeiger über das Symbol "Web" und wählen Sie **Websites** aus dem Menü.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

In der **Websites** auf **contosouniversity.com** (der Name der Website, die Sie verwendet werden, wenn Sie das Konto erstellt haben).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

In der **Websiteeigenschaften** Kontrollkästchen die **Erweiterungen** Registerkarte.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Ändern von ASP.NET aus **2.0 integrierte Pipeline** zu **4.0 (integrierte Pipeline)**, und klicken Sie dann auf **Update**.

## <a name="publishing-to-the-hosting-provider"></a>Veröffentlichen auf dem Hostinganbieter

E-Mail zur Begrüßung vom hosting-Anbieter enthält alle Einstellungen, die Sie benötigen, um das Projekt veröffentlichen, und Sie können diese Informationen in einem Veröffentlichungsprofil manuell eingeben. Jedoch eine einfachere und weniger fehleranfällig-Methode verwenden Sie zum Konfigurieren der Bereitstellung an den Anbieter: Sie herunterladen werden eine *.publishsettings* Datei, und klicken Sie in einem Veröffentlichungsprofil importieren.

Klicken Sie in Ihrem Browser wechseln Sie zur Systemsteuerung Cytanium, und wählen Sie **Web** und wählen Sie dann **Websites.**

![Systemsteuerung Websites auswählen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Wählen Sie die **contosouniversity.com** Website.

![Systemsteuerung contosouniversity.com auswählen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Wählen Sie die **Webveröffentlichung** Registerkarte.

![Registerkarte mit Webveröffentlichung Panel-Steuerelement](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Erstellen Sie Anmeldeinformationen für das Web veröffentlichen, indem Sie einen Benutzernamen und ein Kennwort einzugeben. Sie können die gleichen Anmeldeinformationen eingeben, mit denen Sie die Systemsteuerung anmelden. Klicken Sie dann auf **aktivieren**.

![Systemsteuerung zum Veröffentlichen verwendeten Anmeldeinformationen zu erstellen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Klicken Sie auf **Veröffentlichungsprofil herunterladen, für diese Website**.

![Veröffentlichungsprofil Control Panel herunterladen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Wenn Sie aufgefordert werden, öffnen oder speichern Sie die Datei, speichern Sie sie.

![Veröffentlichungsprofil zu speichern](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

In **Projektmappen-Explorer** in Visual Studio mit der rechten Maustaste in des ContosoUniversity-Projekts, und wählen Sie **veröffentlichen**. Die **Webveröffentlichung** Dialogfeld wird geöffnet, auf die **Vorschau** Registerkarte mit der **Test** Profil aktiviert werden, da dies die letzte Profil ist Sie verwendet haben.

Wählen Sie die **Profil** Registerkarte, und klicken Sie dann auf **Import**.

![Veröffentlichen von Web-Assistent – Schaltfläche "Importieren"](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

In der **Importieren von Veröffentlichungseinstellungen** wählen Sie im Dialogfeld die *.publishsettings* Datei, die Sie heruntergeladen haben, und klicken Sie auf **öffnen**. Der Assistent wechselt zur Registerkarte "Verbindung" mit der alle Felder ausgefüllt.

![Veröffentlichen von Web-Assistent – Registerkarte "Verbindung"](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

Die PUBLISHSETTINGS-Datei wird die geplante permanente URL für den Standort im Ziel-URL, aber wenn Sie diese Domäne noch nicht erworben haben, ersetzen Sie den Wert mit der temporären URL. In diesem Beispiel wird die URL  *[ http://contosouniversity.com.vserver01.cytanium.com ](http://contosouniversity.com.vserver01.cytanium.com).* Der einzige Zweck der dieses Feld ist, an welche URL, die Browser automatisch nach erfolgreich nach der Bereitstellung geöffnet wird. Wenn Sie sie leer lassen, ist die einzige Folge an, dass der Browser nicht automatisch nach der Bereitstellung startet.

Klicken Sie auf **Verbindung überprüfen** zu überprüfen, ob die Einstellungen richtig sind und Sie können eine Verbindung mit dem Server herstellen. Wie Sie gesehen haben, überprüft ein grünes Häkchen an, dass die Verbindung erfolgreich ist.

Wenn Sie die Verbindung überprüfen klicken, können Sie sehen eine **Zertifikatfehler** Dialogfeld. Wenn Sie dies tun, stellen Sie sicher, dass der Servername ist, was Sie erwarten. Wenn es sich handelt, wählen Sie **speichern Sie dieses Zertifikat für zukünftige Sitzungen von Visual Studio** , und klicken Sie auf **Accept**. (Dieser Fehler weist darauf hin, dass der Hostinganbieter ausgewählt hat, vermeiden Sie die Kosten für den Erwerb eines SSL-Zertifikats für die URL, die Sie bereitstellen. Wenn Sie eine sichere Verbindung mit einem gültigen Zertifikat einrichten möchten, wenden Sie sich an Ihren Hostinganbieter.)

![Zertifikatfehler](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Klicken Sie auf **Weiter**.

In der **Datenbanken** Teil der **Einstellungen** Registerkarte, geben Sie den gleichen Sie das Veröffentlichungsprofil für den Test eingegebenen Werte. Sie finden die Verbindungszeichenfolgen, die Sie benötigen in der Dropdown-Liste.

- Klicken Sie in das Feld die Verbindungszeichenfolge für **SchoolContext,** auswählen `Data Source=|DataDirectory|School-Prod.sdf`
- Klicken Sie unter **SchoolContext**Option **anwenden Code First-Migrationen**.
- Klicken Sie in das Feld die Verbindungszeichenfolge für **DefaultConnection**wählen `Data Source=|DataDirectory|aspnet-Prod.sdf`
- Klicken Sie unter **DefaultConnection**, lassen Sie **Aktualisieren einer Datenbank** gelöscht.

![Veröffentlichen Sie die Registerkarte "Einstellungen" der Web-Assistent](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Klicken Sie auf **Weiter**.

In der **Vorschau** auf **Vorschau starten** um eine Liste der Dateien anzuzeigen, die kopiert werden. Daraufhin wird die gleiche Liste, die Sie zuvor gesehen haben, als Sie in IIS bereitgestellt haben, auf dem lokalen Computer.

Vor der Veröffentlichung ändern Sie den Namen des Profils, sodass Ihre Web.Production.config-Transformationsdatei angewendet werden. Wählen Sie die **Profil** Registerkarte, und klicken Sie auf **Profile verwalten**.

![Veröffentlichen Sie Profile für Web-Assistent zum Verwalten](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

In der **Profile für Web veröffentlichen bearbeiten** Dialogfeld ein, wählen Sie das produktionsprofil, klicken Sie auf **umbenennen**, und ändern Sie den Namen des Profils für die Produktion. Klicken Sie dann auf **schließen**.

![Bearbeiten Sie im Dialogfeld Web veröffentlichen Profile](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Klicken Sie auf **Veröffentlichen**.

Die Anwendung veröffentlicht wird an den Hostinganbieter. Das Ergebnis zeigt, der **Ausgabe** Fenster.

![Fenster "Ausgabe" nach der Bereitstellung](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Der Browser automatisch geöffnet wird, an die URL, die Sie in eingegeben **Ziel-URL** Feld der **Verbindung** auf der Registerkarte der **Webveröffentlichung** Assistenten. Sie finden Sie auf der gleichen, die beim Ausführen der Website in Visual Studio, allerdings gibt es jetzt keine "(Test)" oder "(Dev)" Umgebung Indikator in der Titelleiste angezeigt. Dies bedeutet, dass der Indikator für die Umgebung *"Web.config"* Transformation ordnungsgemäß funktioniert.

> [!NOTE]
> Wenn Sie weiterhin "(Test)" in der Überschrift angezeigt wird, löschen Sie die *Obj* Ordner für die ContosoUniversity-Projekt und erneut bereitstellen. In Vorabversionen der Software die zuvor angewendeten Transformationsdatei (Web.Test.config) möglicherweise erneut angewendet werden, obwohl Sie das Profil für die Produktion verwenden.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Vor dem Ausführen einer Seite, die Zugriff auf die Datenbank bewirkt, dass stellen Sie sicher, dass Elmah werden alle Fehler zu protokollieren, die auftreten können.

## <a name="setting-folder-permissions-for-elmah"></a>Festlegen von Ordnerberechtigungen nach Elmah

Wie Sie aus dem vorherigen Lernprogramm dieser Reihe vergessen, müssen Sie sicherstellen, dass die Anwendung Schreibberechtigungen für den Ordner in Ihrer Anwendung, in dem Elmah-Protokolldateien speichert. Wenn Sie für IIS lokal auf Ihrem Computer bereitgestellt, legen Sie die Berechtigungen manuell. In diesem Abschnitt sehen Sie, wie Berechtigungen zur Cytanium festgelegt werden. (Einige hosting-Anbieter möglicherweise nicht möglich, Sie möchten, bieten einen oder mehrere vordefinierte Ordner mit Schreibberechtigungen. In diesem Fall müssten Sie die Anwendung verwendet die angegebenen Ordner zu ändern.)

Sie können Berechtigungen für Ordner in der Systemsteuerung Cytanium festlegen. Navigieren Sie im Bereich für das Steuerelement die URL ein, und wählen **Manager für Dateiserver**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

In der **Manager für Dateiserver** Kontrollkästchen **contosouniversity.com** und dann **Wwwrooot** zum Stammordner der Anwendung finden Sie unter. Klicken Sie auf das Schlosssymbol neben **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

In der **Datei**/**Ordnerberechtigungen** wählen Sie im Fenster der **lesen** und **schreiben** Kontrollkästchen für  **contosouniversity.com** , und klicken Sie auf **Festlegen von Berechtigungen**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Stellen sicher, dass Elmah Schreibzugriff auf die *Elmah* Ordner mit dem verursacht einen Fehler, und klicken Sie dann Anzeigen des Fehlerberichts Elmah. Fordern Sie eine ungültige URL wie *Studentsxxx.aspx*. Wie vorher, Sie sehen die *GenericErrorPage.aspx* Seite. Klicken Sie auf die **Abmelden** verknüpfen, und führen Sie *Elmah.axd*. Erhalten Sie die **anmelden** Seite zuerst, die überprüft, ob die *"Web.config"* Transformation hat Elmah Autorisierung wurde erfolgreich hinzugefügt. Nachdem Sie sich angemeldet haben, sehen Sie den Bericht, der der Fehler angezeigt wird, die, den Sie gerade verursacht hat.

[![ELMAH.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>Testen in der Produktionsumgebung

Führen Sie die **Schüler/Studenten** Seite. Die Anwendung versucht, die Zugriff auf die Datenbank "School" zum ersten Mal, die Code First-Migrationen zum Erstellen der Datenbank auslöst. Wenn die Seite nach Zeit Verzögerung angezeigt wird, zeigt er, dass es keine Schüler/Studenten.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Führen Sie die **Dozenten** Seite, um sicherzustellen, dass die Seed-Daten erfolgreich auf "Instructor" Daten in der Datenbank eingefügt.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Wie Sie in der testumgebung, die Sie stellen Sie sicher, dass Datenbank-Updates in der produktionsumgebung arbeiten, aber in der Regel nicht Testdaten in die Produktionsdatenbank eingeben möchten möchten. In diesem Tutorial verwenden Sie die gleiche Methode, die in Tests der Fall war. In einer echten Anwendung empfiehlt, eine Methode zu suchen, die überprüft, die Datenbank ob Updates sind jedoch erfolgreich ohne Testdaten in der Produktionsdatenbank. In einigen Anwendungen ist es möglicherweise sinnvoll, etwas hinzufügen und löschen Sie sie.

Fügen Sie eine "Student" hinzu, und zeigen Sie dann auf die Daten, die Sie eingegeben, in haben der **Schüler/Studenten** Seite, um sicherzustellen, dass Sie Daten in der Datenbank aktualisieren können.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Überprüfen Sie, ob Autorisierungsregeln ordnungsgemäß, indem Sie die Auswahl funktionieren **Update Gutschriften** aus der **Kurse** im Menü. Die **anmelden** angezeigt wird. Geben Sie Ihre Administrator-Anmeldeinformationen, klicken Sie auf **anmelden**, und die **Update Gutschriften** angezeigt wird.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Wenn die Anmeldung erfolgreich ist, wird die **Update Gutschriften** angezeigt wird. Dies gibt an, dass die ASP.NET-Mitgliedschaftsdatenbank (mit dem einzelnen Administratorkonto) erfolgreich bereitgestellt wurde.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Sie jetzt erfolgreich bereitgestellt und getestet haben Ihre Website, und es ist öffentlich über das Internet verfügbar.

## <a name="creating-a-more-reliable-test-environment"></a>Erstellen einer zuverlässigeren Testumgebung

Wie unter der [Bereitstellung in der Umgebung testen](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) Tutorial, in die zuverlässigste testumgebung wäre ein zweites Konto auf dem Hostinganbieter, die wie das Produktionskonto ist. Dies wäre teurer als die lokale IIS als testumgebung verwenden, da Sie müssten für eine zweite hosting-Konto zu registrieren. Aber wenn kann von Produktionsfehlern-Website oder Ausfälle können Sie festlegen, dass es sich bei der Aufwand ist.

Die meisten der Prozess zum Erstellen und Bereitstellen eines Testkontos ist vergleichbar mit was Sie bereits durchgeführt haben, um für die Produktion bereitstellen:

- Erstellen Sie eine *"Web.config"* Transformationsdatei.
- Erstellen Sie ein Konto auf dem Hostinganbieter ein.
- Erstellen Sie ein neues Veröffentlichungsprofil und auf das Test-Konto bereitstellen.

### <a name="preventing-public-access-to-the-test-site"></a>Verhindern öffentlichen Zugriff auf

Ein wichtiger Aspekt für das Test-Konto ist, dass sie auf das Internet aktiv werden, möchten jedoch nicht die öffentliche Verwendung. Um den Standort privat bleiben können Sie eine oder mehrere der folgenden Methoden:

- Wenden Sie sich an den Hostinganbieter um Firewall-Regeln festzulegen, die Zugriff auf die Website testen nur von IP-Adressen zu, die Sie zum Testen verwenden ermöglichen.
- Verbergen Sie die URL, damit es nicht an die öffentliche Website-URL ähnlich ist.
- Verwenden einer *robots.txt* Datei, um sicherzustellen, dass Suchmaschinen nicht in den Suchergebnissen die Verknüpfungen des Test-Website und Bericht darauf crawlen.

Die erste dieser Methoden ist natürlich die sicherste Einstellung, aber die Prozedur, ist spezifisch für jeden Hostinganbieter und werden in diesem Tutorial nicht behandelt. Wenn Sie mit Ihrem Hostinganbieter können nur Ihre IP-Adresse, um die Konto-URL von Test suchen anordnen, müssen Sie theoretisch crawling es Suchmaschinen kümmern. Aber selbst in diesem Fall die Bereitstellung einer *robots.txt* Datei ist eine gute Idee als Sicherung ein, für den Fall, dass die Firewallregel versehentlich deaktiviert ist.

Die *robots.txt* -Datei wird in Ihrem Projektordner, und verfügen über den folgenden Text in diese:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

Die `User-agent` Zeile wird die Suchmaschinen, die für alle Web Suchmaschinencrawlern (Roboter), die Regeln in der Datei gelten und die `Disallow` Zeile gibt an, dass keine Seiten auf der Website durchsucht werden soll.

Sie möchten wahrscheinlich Suchmaschinen, Ihre Produktionswebsite zu katalogisieren, daher Sie diese Datei aus der produktionsbereitstellung ausschließen müssen. Zu diesem Zweck finden Sie unter **kann ich ausschließen bestimmte Dateien oder Ordner von der Bereitstellung?** in [ASP.NET Web Application Project-Bereitstellung – häufig gestellte Fragen](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Stellen Sie sicher, dass Sie der Ausschlussliste angeben, nur für die Produktion Veröffentlichungsprofil.

Erstellen ein zweites Hostinganbieters-Konto ist ein Ansatz für die Arbeit mit einer testumgebung, die ist nicht erforderlich, aber unter Umständen zu den zusätzlichen Aufwand. In den folgenden Tutorials werden Sie weiterhin IIS die Test-Umgebung zu verwenden.

Im nächsten Tutorial werden Sie Anwendungscode aktualisieren und die Änderung in den Test- und produktionsumgebungen Umgebungen bereitstellen.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
