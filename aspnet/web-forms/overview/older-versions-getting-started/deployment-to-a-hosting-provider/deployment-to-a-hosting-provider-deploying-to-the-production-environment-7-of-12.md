---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellung in der Produktionsumgebung - 7 von 12 | Microsoft Docs'
author: tdykstra
description: "Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual das..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: b83ab819-2b05-4776-b7b4-79ef78d457a5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12
msc.type: authoredcontent
ms.openlocfilehash: 4aa6766c2c7765f499f5c5380962a5fe443e8c9d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-the-production-environment---7-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellung in der Produktionsumgebung - 7 von 12
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie das Web veröffentlichen Update installieren. Eine Einführung in der Reihe, finden Sie unter [im ersten Lernprogramm, in der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, die die Bereitstellungsfeatures nach der RC-Version von Visual Studio 2012 eingeführt wurde, wird gezeigt, wie SQL Server-Editionen als SQL Server Compact bereitstellen, und zeigt die Vorgehensweise beim Bereitstellen in Azure App Service-Web-Apps, finden Sie unter [ASP.NET Web Deploy Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

In diesem Lernprogramm können Sie auch ein Konto mit einem Hostinganbieter einrichten und Bereitstellen von ASP.NET Webanwendung in der produktionsumgebung mithilfe von Visual Studio einmalklick-Funktion zu veröffentlichen.

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="selecting-a-hosting-provider"></a>Hosting-Anbieter auswählen

Für die University Contoso-Anwendung und dieser Reihe von Lernprogrammen benötigen Sie einen Anbieter, der ASP.NET 4 und Web Deploy unterstützt. Einem bestimmten Hostingunternehmen wurde gewählt, damit die Lernprogramme zu veranschaulichen die vollständige Benutzeroberfläche für eine live-Website bereitgestellt konnte. Jede Hostingunternehmen bietet verschiedene Funktionen sowie die Erfahrung der Bereitstellung auf ihren Servern richtet sich. Allerdings ist das Verfahren in diesem Lernprogramm beschrieben typisch für das generelle Verfahren. Der hosting-Anbieter verwendet für dieses Lernprogramm Cytanium.com, ist eins von vielen, die verfügbar sind, und seine Verwendung in diesem Lernprogramm stellt keine Befürwortung oder Empfehlung dar.

Wenn Sie eigene Hostinganbieter auswählen möchten, können Sie Funktionen und Preisen in Vergleichen die [Katalog von Anbietern](https://www.microsoft.com/web/hosting) auf der Microsoft.com-Website.

## <a name="creating-an-account"></a>Erstellen eines Kontos

Erstellen Sie ein Konto bei Ihrem ausgewählten Anbieter. Wenn eine Unterstützung für eine vollständige SQL Server-Datenbank eine hinzugefügte zusätzliche, Sie müssen nicht für die Auswahl für dieses Lernprogramm, aber Sie benötigen ihn für die [Migrieren zu SQL Server](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md) Tutorial weiter unten in dieser Serie.

Für diese Lernprogramme müssen Sie einen neuen Domänennamen zu registrieren. Sie können testen, um die erfolgreiche Bereitstellung zu überprüfen, indem Sie mit der temporären URL, die vom Anbieter dem Standort zugewiesen.

Nachdem das Konto erstellt wurde, erhalten Sie in der Regel eine e-Mail zur Begrüßung, die alle Informationen enthält, die Sie zum Bereitstellen und Verwalten Ihrer Website benötigen. Die Informationen, die Ihrem Hostinganbieter, die Sie per werden ähnlich wie hier gezeigten abweicht. Der e-Mail zur Begrüßung Cytanium, die auf neue Kontobesitzer gesendet wird, enthält die folgenden Informationen:

- Die URL des Anbieters Bereich Steuerelementsite, dem Sie die Einstellungen für Ihre Website verwalten können. Die ID und Kennwort an, das Sie angegeben haben, sind in diesem Teil der e-Mail zur Begrüßung wiederfinden enthalten. (Beide haben eine Demo-Wert für diese Abbildung geändert wurde.)

    [![Welcome_Email_Control_Panel_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image1.png)
- Die standardmäßige .NET Framework-Version und Informationen, um ihn zu ändern. Viele hosting Standorte standardmäßig auf 2.0 das arbeitet mit ASP.NET-Anwendungen, die das .NET Framework 2.0, 3.0 oder 3.5. Contoso University ist jedoch eine .NET Framework 4-Anwendung, daher Sie diese Einstellung zu ändern müssen. (Für eine ASP.NET 4.5-Anwendung würden Sie die Einstellung .NET 4.0 verwenden.)

    [![Welcome_Email_Framework_Version](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image3.png)
- Die temporäre URL, die Sie verwenden können, um Ihre Website zugreifen. Wenn dieses Konto erstellt wurde, wurde "contosouniversity.com" mit dem vorhandenen Domänennamen eingegeben. Aus diesem Grund die temporäre-URL ist `http://contosouniversity.com.vserver01.cytanium.com`.

    [![Welcome_Email_Temporary_URL](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image5.png)
- Informationen zum Einrichten von Datenbanken und die Verbindungszeichenfolgen, die Sie benötigen, um darauf zugreifen:

    [![Welcome_Email_Database_Info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image7.png)
- Informationen zu Tools und Einstellungen für die Bereitstellung Ihrer Website. (Die e-Mail in Cytanium erwähnt auch WebMatrix, das hier ausgelassen wird).

    [![Welcome_Email_Deploy_info](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image10.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image9.png)

## <a name="setting-the-net-framework-version"></a>Festlegen der .NET Framework-Version

E-Mail zur Begrüßung Cytanium enthält einen Link zu Anweisungen, wie Sie die Version von .NET Framework ändern. Diese Anweisungen Erläutern Sie, dass dies über die Systemsteuerung Cytanium durchgeführt werden kann. Andere Anbieter Steuerelement Bereich Websites, die anders aussehen, oder die sie möglicherweise weisen Sie dazu auf eine andere Weise.

Wechseln Sie zu der die systemsteuerungs-URL. Nachdem Sie sich mit Ihren Benutzernamen und Kennwort sehen Sie die Systemsteuerung.

[![Cytanium_Control_Panel](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image12.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image11.png)

In der **Hosting Leerzeichen** , halten Sie den Zeiger über das Symbol "Web" und wählen Sie **Websites** aus dem Menü.

[![Cytanium_Control_Panel_selecting_Web_Sites](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image14.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image13.png)

In der **Websites** auf **contosouniversity.com** (der Name des Standorts, den Sie verwendet werden, wenn Sie das Konto erstellt haben).

[![Cytanium_Control_Panel_selecting_contosouniversity](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image15.png)

In der **Websiteeigenschaften** aus, wählen die **Erweiterungen** Registerkarte.

[![Cytanium_Control_Panel_Extensions_tab](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image17.png)

Ändern der ASP.NET von **2.0 integrierten Pipeline** auf **4.0 (integrierten Pipeline)**, und klicken Sie dann auf **Update**.

## <a name="publishing-to-the-hosting-provider"></a>Mit dem Hostinganbieter veröffentlichen

E-Mail zur Begrüßung vom hosting-Anbieter enthält alle Einstellungen, die Sie benötigen, um das Projekt veröffentlichen, und Sie können diese Informationen in einem Veröffentlichungsprofil manuell eingeben. Jedoch eine einfachere und weniger fehleranfällig-Methode verwenden Sie zum Konfigurieren der Bereitstellung an den Anbieter: heruntergeladene eine *publishsettings* Datei und importieren Sie es in einem Veröffentlichungsprofil.

Klicken Sie in Ihrem Browser, wechseln Sie zur Systemsteuerung Cytanium, und wählen Sie **Web** und wählen Sie dann **Websites.**

![Systemsteuerung Websites auswählen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image19.png)

Wählen Sie die **contosouniversity.com** Website.

![Systemsteuerung contosouniversity.com auswählen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image20.png)

Wählen Sie die **Webveröffentlichung** Registerkarte.

![Registerkarte mit Linksteuerelementen Bereich Webveröffentlichung](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image21.png)

Erstellen Sie Anmeldeinformationen für das Web veröffentlichen, indem Sie eingeben, einen Benutzernamen und ein Kennwort verwenden. Sie können die gleichen Anmeldeinformationen eingeben, mit denen Sie zur Systemsteuerung anmelden. Klicken Sie dann auf **aktivieren**.

![Systemsteuerung erstellen Sie zum Veröffentlichen verwendeten Anmeldeinformationen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image22.png)

Klicken Sie auf **Veröffentlichungsprofil herunterladen, für diese Website**.

![Control Panel Herunterladen eines Veröffentlichungsprofils](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image23.png)

Wenn Sie aufgefordert werden, öffnen oder speichern Sie die Datei, speichern Sie es.

![Speichern Veröffentlichungsprofil](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image24.png)

In **Projektmappen-Explorer** in Visual Studio mit der rechten Maustaste des ContosoUniversity-Projekts, und wählen Sie **veröffentlichen**. Die **Web veröffentlichen** Dialogfeld wird geöffnet, auf die **Vorschau** Registerkarte mit den **Test** Profil aktiviert werden, da dies die letzte Profil verwenden.

Wählen Sie die **Profil** Registerkarte, und klicken Sie dann auf **Import**.

![Veröffentlichen von Web-Assistent-Schaltfläche "Importieren"](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image25.png)

In der **Importieren von Veröffentlichungseinstellungen** wählen Sie im Dialogfeld die *publishsettings* Datei, die Sie heruntergeladen haben, und klicken Sie auf **öffnen**. Der Assistent wechselt zur Registerkarte "Verbindung" mit alle Felder ausgefüllt.

![Veröffentlichen von Web-Assistent – Registerkarte "Datenbankverbindung"](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image26.png)

Die PUBLISHSETTINGS-Datei setzt die geplante permanente URL für die Website im Ziel-URL, aber wenn Sie diese Domäne noch nicht erworben haben, ersetzen Sie den Wert mit der temporären URL. In diesem Beispiel wird die URL  *[http://contosouniversity.com.vserver01.cytanium.com](http://contosouniversity.com.vserver01.cytanium.com).* Der einzige Zweck dieses Dialogfelds ist, welche URL an den Browser automatisch nach erfolgreich nach der Bereitstellung geöffnet wird. Wenn Sie sie leer lassen, ist die einzige Konsequenz, dass der Browser nicht automatisch nach der Bereitstellung startet.

Klicken Sie auf **Validate Connection** um sicherzustellen, dass die Einstellungen richtig sind und Sie können eine Verbindung mit dem Server herstellen. Wie weiter oben gezeigt, ein grünes Häkchen überprüft, ob die Verbindung erfolgreich ist.

Wenn Sie Validate Connection klicken, wird möglicherweise eine **Zertifikatfehler** (Dialogfeld). Wenn Sie dies tun, stellen Sie sicher, dass der Servername ist, was Sie erwarten. Wenn dies der Fall, wählen Sie **dieses Zertifikat für zukünftige Sitzungen von Visual Studio speichern** , und klicken Sie auf **Accept**. (Dieser Fehler weist darauf hin, die Vermeidung der Kosten für erwerben ein SSL-Zertifikat für die URL, den Sie zum Bereitstellen des hostenden Anbieters ausgewählt wurde. Falls gewünscht, eine sichere Verbindung hergestellt wird, wird die Verwendung eines gültigen Zertifikats, wenden Sie sich an Ihren Hostinganbieter.)

![Zertifikatfehler](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image27.png)

Klicken Sie auf **Weiter**.

In der **Datenbanken** Teil der **Einstellungen** Registerkarte, geben Sie den gleichen Veröffentlichungsprofil der Werte, die Sie für den Test eingegeben haben. Finden Sie die Verbindungszeichenfolgen, die Sie müssen, in der Dropdown-Listen.

- In das Feld die Verbindungszeichenfolge für **SchoolContext,** auswählen`Data Source=|DataDirectory|School-Prod.sdf`
- Klicken Sie unter **SchoolContext**Option **gelten Code First-Migrationen**.
- In das Feld die Verbindungszeichenfolge für **DefaultConnection**wählen`Data Source=|DataDirectory|aspnet-Prod.sdf`
- Klicken Sie unter **DefaultConnection**, lassen Sie **Datenbank aktualisieren** deaktiviert.

![Veröffentlichen Sie die Registerkarte "Einstellungen" der Web-Assistent](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image28.png)

Klicken Sie auf **Weiter**.

In der **Vorschau** auf **starten Preview** um eine Liste der Dateien anzuzeigen, die kopiert werden. Daraufhin wird die gleiche Liste, die Sie zuvor gesehen haben, wenn Sie in IIS auf dem lokalen Computer bereitgestellt.

Vor dem Veröffentlichen, ändern Sie den Namen des Profils, damit die Transformationsdatei Web.Production.config angewendet werden. Wählen Sie die **Profil** Registerkarte, und klicken Sie auf **Profile verwalten**.

![Veröffentlichen von Web Assistenten zum Verwalten von Profilen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image29.png)

In der **Web veröffentlichen Profile bearbeiten** Dialogfeld Feld, wählen Sie das Profil für die Produktion, klicken Sie auf **umbenennen**, und ändern Sie den Profilnamen, bis hin zur Produktion. Klicken Sie dann auf **schließen**.

![Bearbeiten Sie die Profile für Web veröffentlichen (Dialogfeld)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image30.png)

Klicken Sie auf **Veröffentlichen**.

Die Anwendung wird mit dem Hostinganbieter veröffentlicht. Das Ergebnis wird der **Ausgabe** Fenster.

![Fenster "Ausgabe" nach der Bereitstellung](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image31.png)

Browser automatisch geöffnet wird, zu der URL, die im eingegebenen **Ziel-URL** Feld der **Verbindung** auf der Registerkarte die **Web veröffentlichen** Assistenten. Finden Sie unter der gleichen Startseite als beim Ausführen der Website in Visual Studio mit der Ausnahme ist jetzt gibt es keine "(Test)" oder "(Dev)" Umgebung Indikator in der Titelleiste. Dies bedeutet, dass der Indikator Umgebung *"Web.config"* Transformation ordnungsgemäß funktioniert.

> [!NOTE]
> Wenn "(Test)" in der Überschrift weiterhin angezeigt wird, löschen Sie die *Obj* Ordner aus dem Projekt ContosoUniversity und erneut bereitstellen. In Vorabversionen der Software die zuvor angewendeten Transformationsdatei (Web.Test.config) möglicherweise erneut angewendet, obwohl Sie das Profil für die Produktion verwenden.


[![Home_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image33.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image32.png)

Vor dem Ausführen einer Seite, die bewirkt, Zugriff auf die Datenbank dass, stellen Sie sicher, dass Elmah werden alle Fehler zu protokollieren, die auftreten können.

## <a name="setting-folder-permissions-for-elmah"></a>Festlegen von Berechtigungen für die Elmah

Wie Sie aus dem vorherigen Lernprogramm dieser Reihe daran denken, müssen Sie sicherstellen, dass die Anwendung Schreibberechtigungen für den Ordner in der Anwendung verfügt, in dem Elmah Fehlerprotokolldateien speichert. Wenn Sie für IIS lokal auf Ihrem Computer bereitgestellt, legen Sie diese Berechtigungen manuell. In diesem Abschnitt sehen Sie, wie Berechtigungen zur Cytanium festgelegt werden. (Einige Hostinganbieter möglicherweise nicht ermöglichen, Sie zu diesem Zweck, bieten einen oder mehrere vordefinierte Ordner mit Schreibberechtigungen. In diesem Fall müssten Sie die Anwendung verwendet den angegebenen Ordner zu ändern.)

Sie können Berechtigungen für Ordner in der Systemsteuerung Cytanium festlegen. Wechseln Sie an das Steuerelement Bereich URL, und wählen Sie **Manager für Dateiserver**.

[![Cytanium_Control_Panel_with_File_Manager_selected](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image35.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image34.png)

In der **Manager für Dateiserver** wählen Sie im **contosouniversity.com** und dann **Wwwrooot** zum Stammordner der Anwendung finden Sie unter. Klicken Sie auf das Vorhängeschlosssymbol neben **Elmah**.

[![Cytanium_Control_Panel_File_Manager_at_root_folder](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image37.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image36.png)

In der **Datei**/**Ordnerberechtigungen** wählen die **lesen** und **schreiben** Kontrollkästchen für  **contosouniversity.com** , und klicken Sie auf **Festlegen von Berechtigungen**.

[![Cytanium_Control_Panel_File_Folder_Permissions_Elmah](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image39.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image38.png)

Stellen sicher, dass Elmah Schreibzugriff auf die *Elmah* Ordner verursacht einen Fehler, und klicken Sie dann zum Anzeigen des Fehlerberichts Elmah. Fordern Sie eine ungültige URL wie *Studentsxxx.aspx*. Wie bereits zuvor können Sie sehen die *GenericErrorPage.aspx* Seite. Klicken Sie auf die **Abmelden** verknüpfen, und führen Sie *Elmah.axd*. Sie erhalten die **anmelden** Seite zuerst, die überprüft, ob der *"Web.config"* Transformation erfolgreich Elmah Autorisierung hinzugefügt. Nachdem Sie sich anmelden, sehen Sie den Bericht, der der Fehler angezeigt wird, den Sie soeben verursacht.

[![Elmah.axd_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image41.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image40.png)

## <a name="testing-in-the-production-environment"></a>In der Produktionsumgebung testen

Führen Sie die **Studenten** Seite. Die Anwendung versucht, die Datenbank "School" zum ersten Mal zugreifen, wodurch Code First-Migrationen zum Erstellen der Datenbank ausgelöst. Wenn die Seite nach einen Moment Verzögerung angezeigt wird, zeigt es, dass es keine Studenten.

[![Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image43.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image42.png)

Führen Sie die **Lehrkräfte** Seite, um sicherzustellen, dass die Ausgangswerte erfolgreich Instructor-Daten in der Datenbank eingefügt.

[![Instructors_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image45.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image44.png)

Wie in der testumgebung entsprechend, um sicherzustellen, dass Datenbankupdates, in der produktionsumgebung arbeiten, aber Sie in der Regel sollten keine Testdaten in die Produktionsdatenbank eingeben werden sollen. Für dieses Lernprogramm verwenden Sie die gleiche Methode, die Sie im Test ausgeführt haben. In einer realen Anwendung eine Methode zu suchen, die dieser Datenbank überprüft werden sollen Updates sind jedoch erfolgreich ohne Testdaten in der Produktionsdatenbank einführen. In einigen Anwendungen ist es möglicherweise sinnvoll, etwas hinzufügen und löschen Sie sie.

Fügen Sie ein Student hinzu und zeigen Sie dann die Daten, die Sie eingegeben, in haben der **Studenten** Seite, um sicherzustellen, dass Sie Daten in der Datenbank aktualisieren können.

[![Add_Students_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image47.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image46.png)

[![Students_page_with_new_student_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image49.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image48.png)

Überprüfen Sie die Autorisierungsregeln dazu ordnungsgemäß funktionieren **Update Gutschriften** aus der **Kurse** Menü. Die **anmelden** angezeigt wird. Geben Sie die Anmeldeinformationen des Administratorkontos Ihres, klicken Sie auf **anmelden**, und die **Update Gutschriften** angezeigt wird.

[![Log_In_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image51.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image50.png)

Wenn die Anmeldung erfolgreich ist, wird die **Update Gutschriften** angezeigt wird. Dies gibt an, dass die ASP.NET-Mitgliedschaftsdatenbank (mit dem einzelnen Administratorkonto) erfolgreich bereitgestellt wurde.

[![Update_Credits_page_Prod](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image53.png)](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/_static/image52.png)

Sie jetzt erfolgreich bereitgestellt und getestet haben Ihre Website, und er öffentlich verfügbar ist über das Internet.

## <a name="creating-a-more-reliable-test-environment"></a>Erstellen einer zuverlässigeren Testumgebung

Wie in beschrieben die [Bereitstellung in der Umgebung testen](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) Lernprogramm, in die Umgebung für das zuverlässigste wäre ein zweites Konto auf dem Hostinganbieter, die wie bei der Produktion-Konto hat. Dies wäre teurer als lokale IIS als testumgebung verwenden, da müssten Sie eine zweite hosting-Konto registrieren. Aber wenn es Produktion Website Fehler oder Ausfälle verhindert, könnten Sie, dass sie die entsprechenden Kosten rechtfertigen ist.

Die meisten der Prozess zum Erstellen und Bereitstellen von für ein Testkonto ist vergleichbar mit was Sie bereits durchgeführt haben, um bis hin zur Produktion bereitzustellen:

- Erstellen einer *"Web.config"* Transformationsdatei.
- Erstellen Sie ein Konto auf dem Hostinganbieter.
- Erstellen Sie ein neues Veröffentlichungsprofil, und stellen dieses Testkonto bereit.

### <a name="preventing-public-access-to-the-test-site"></a>Öffentlicher Zugriff auf den Test-Standort zu verhindern

Ein wichtiger Aspekt für die Testkonto ist, live im Internet werden, aber nicht öffentlich, ihn zu verwenden. Um den Standort privat bleiben können Sie eine oder mehrere der folgenden Methoden verwenden:

- Wenden Sie sich an den Hostinganbieter zum Firewallregeln festlegen, die Zugriff auf die Website testen nur von IP-Adressen zu, die Sie zum Testen verwenden ermöglichen.
- Verschleiern Sie die URL, sodass sie nicht die öffentliche Website-URL entspricht.
- Verwenden einer *"robots.txt"* Datei, um sicherzustellen, dass Suchmaschinen nicht in den Suchergebnissen die Test-Website und der Bericht Links zu crawlen.

Die erste dieser Methoden ist offensichtlich die sicherste, aber die Prozedur, ist spezifisch für jeden Hostinganbieter und werden in diesem Lernprogramm nicht behandelt. Wenn Sie mit Ihrem Hostinganbieter können nur Ihre IP-Adresse, um die Test-Konto-URL suchen anordnen, müssen Sie theoretisch Suchmaschinen crawling es kümmern. Aber selbst in diesem Fall Bereitstellen einer *"robots.txt"* Datei ist eine gute Idee als Sicherung, für den Fall, dass diese Firewallregel jemals versehentlich deaktiviert ist.

Die *"robots.txt"* Datei wechselt in den Projektordner, und sollte den folgenden Text im es haben:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12/samples/sample1.cmd)]

Die `User-agent` Zeile weist Suchmaschinen, die für alle Search Engine-Webcrawler (Roboter), die Regeln in der Datei gelten und die `Disallow` Zeile gibt an, dass keine Seiten auf der Website gecrawlt werden soll.

Sie möchten wahrscheinlich Suchmaschinen, Ihre Produktionswebsite zu katalogisieren, daher Sie diese Datei produktionsbereitstellung ausgeschlossen müssen. Zu diesem Zweck finden Sie unter **kann ich schließt bestimmte Dateien oder Ordner aus Bereitstellung?** in [ASP.NET Web Application Project Deployment FAQ](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment). Stellen Sie sicher, dass Sie nur für die Produktion Veröffentlichungsprofil den Ausschluss angeben.

Erstellen ein zweites Hostinganbieters-Konto ist ein Ansatz für die Arbeit mit einer testumgebung, die ist nicht erforderlich, aber möglicherweise Folgendes zusätzliche Kosten für die zu. In den folgenden Lernprogrammen werden Sie weiterhin die IIS als Ihre testumgebung verwenden.

In den nächsten Lernprogrammen Sie Anwendungscode zu aktualisieren und die Änderung für die Test- und produktionsumgebungen Umgebungen bereitstellen.

>[!div class="step-by-step"]
[Zurück](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
[Weiter](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
