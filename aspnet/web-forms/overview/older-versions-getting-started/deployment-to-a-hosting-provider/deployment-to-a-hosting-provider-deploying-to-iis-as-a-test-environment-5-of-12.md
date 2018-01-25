---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen von IIS als Testumgebung - 5, 12 | Microsoft Docs'
author: tdykstra
description: "Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual das..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: a7995844ee6ed19efa130c4f6c019214d6652ea7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen von IIS als Testumgebung - 5, 12
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Diese Reihe von Lernprogrammen wird gezeigt, wie das Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für das Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie das Web veröffentlichen Update installieren. Eine Einführung in der Reihe, finden Sie unter [im ersten Lernprogramm, in der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, die die Bereitstellungsfeatures nach der RC-Version von Visual Studio 2012 eingeführt wurde, wird gezeigt, wie SQL Server-Editionen als SQL Server Compact bereitstellen, und zeigt die Vorgehensweise beim Bereitstellen in Azure App Service-Web-Apps, finden Sie unter [ASP.NET Web Deploy Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

Dieses Lernprogramm zeigt, wie eine ASP.NET-Webanwendung in IIS auf dem lokalen Computer bereitstellen.

Wenn Sie eine Anwendung entwickeln, testen Sie in der Regel in Visual Studio ausführen. In der Standardeinstellung bedeutet dies, dass Sie Visual Studio Development Server (auch bekannt als Cassini) verwenden. Visual Studio Development Server ganz einfach während der Entwicklung in Visual Studio zu testen, aber es funktioniert nicht genau wie IIS. Daher ist es möglich, dass eine Anwendung ordnungsgemäß ausgeführt wird, wenn Sie in Visual Studio zu testen, jedoch fehl, wenn er in einer Hostingumgebung für IIS bereitgestellt wird.

Sie können Ihre Anwendung zuverlässiger folgendermaßen testen:

1. Verwenden Sie IIS Express oder vollständige Variante von IIS anstelle der Visual Studio Development Server, wenn Sie während der Entwicklung in Visual Studio testen. Diese Methode emuliert in der Regel genauer wie Ihre Website unter IIS ausgeführt wird. Diese Methode jedoch nicht testen das Verfahren zum Bereitstellen oder überprüfen, dass das Ergebnis des Bereitstellungsprozesses ordnungsgemäß ausgeführt wird.
2. Bereitstellen der Anwendung in IIS auf dem Entwicklungscomputer mithilfe desselben Prozesses, der später verwendet werden, die sie in der produktionsumgebung bereitstellen. Diese Methode überprüft, ob das Verfahren zum Bereitstellen zusätzlich zu überprüfen, das Ihre Anwendung unter IIS ordnungsgemäß ausgeführt wird.
3. Bereitstellen der Anwendung in einer testumgebung, die so nah wie möglich in Ihrer produktionsumgebung erforderlich ist. Da die produktionsumgebung für diese Lernprogramme einem Hostinganbieter eines Drittanbieters ist, wäre die ideale Umgebung für das ein zweites Konto mit dem Hostinganbieter. Verwenden Sie dieses zweite Konto nur zu Testzwecken, aber es würde die gleiche Weise wie das Konto für die Produktion einrichten festgelegt werden.

Dieses Lernprogramm zeigt die Schritte für die Option 2. Option 3 Anweisungen am Ende der [in der Produktionsumgebung bereitstellen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Lernprogramm und am Ende dieses Lernprogramms stehen Links zu Ressourcen für Option 1 aus.

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, müssen Sie überprüfen die [Website](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Konfigurieren die Anwendung für die Ausführung in mittlerer Vertrauenswürdigkeit

Ändern Sie eine Einstellung einer "Web.config" vor dem Installieren von IIS, und darauf bereitstellen, um den Standort ausführen mehr vornehmen zu können, wie es in einer typischen freigegebenen Hostingumgebung wird.

Hostinganbieter erhalten die Möglichkeit in der Regel führen Sie Ihre Website *mittlerer Vertrauenswürdigkeit*, d. h. Es gibt einige Dinge, darf es nicht tun. Beispielsweise kann nicht Anwendungscode den Zugriff auf die Windows-Registrierung und kann nicht lesen oder Schreiben von Dateien, die außerhalb der Ordnerhierarchie für Ihre Anwendung sind. Standardmäßig die Anwendung ausgeführt wird, im *hohe Vertrauenswürdigkeit* auf dem lokalen Computer, was bedeutet, dass die Anwendung möglicherweise können Aktionen ausgeführt werden, die ein Fehler auftritt, während der Bereitstellung bis hin zur Produktion. Um der Umgebung, die mehrere genau wieder, die produktionsumgebung zu machen, werden Sie daher die Anwendung mit mittlerer Vertrauenswürdigkeit ausführen konfigurieren.

Fügen Sie in der Datei "Web.config" der Anwendung eine **Vertrauensstellung** Element in der **system.web** Element, wie im folgenden Beispiel gezeigt.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

Die Anwendung wird nun in mittlerer vetrauenswürdigkeit in IIS auch auf dem lokalen Computer ausgeführt. Mit dieser Einstellung können Sie so früh wie möglich alle Versuche von Anwendungscode eine Aktion abfangen, die in der Produktion ein Fehler auftritt.

> [!NOTE]
> Wenn Sie Entity Framework Code First-Migrationen verwenden, stellen Sie sicher, dass Sie Version 5.0 oder höher installiert. In Entity Framework, Version 4.3 Migrationen erfordert volle Vertrauenswürdigkeit um das Datenbankschema zu aktualisieren.


## <a name="installing-iis-and-web-deploy"></a>Installieren von IIS und Web bereitstellen

Um IIS auf dem Entwicklungscomputer bereitzustellen, benötigen Sie IIS und Web Deploy installiert. Diese sind nicht in der Standardkonfiguration von Windows 7 enthalten. Wenn Sie sowohl IIS als auch Web Deploy bereits installiert haben, fahren Sie mit dem nächsten Abschnitt.

Mithilfe der [Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx) ist die bevorzugte Möglichkeit, IIS und Web Deploy, installiert werden, da der Webplattform-Installer eine empfohlene Konfiguration für IIS installiert und automatisch die erforderlichen Komponenten für IIS und Web installiert Stellen Sie bei Bedarf bereit.

Verwenden Sie den folgenden Link zum Ausführen von Webplattform-Installer zum Installieren von IIS und Web Deploy. Wenn Sie IIS, Web Deploy oder ihre erforderlichen Komponenten bereits installiert haben, installiert der Webplattform-Installer nur was fehlt.

- [Installieren von IIS und Web Deploy mithilfe von WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Festlegen des Standardanwendungspools in .NET 4

Führen Sie nach der Installation von IIS, **IIS-Manager** um sicherzustellen, dass .NET Framework, Version 4 der Standardanwendungspool zugewiesen ist.

Vom Windows **starten** klicken Sie im Menü **ausführen**, geben Sie "Inetmgr", und klicken Sie dann auf **OK**. (Wenn die **ausführen** Befehl befindet sich nicht in Ihrem **starten** Menü können Sie Drücken der Windows-Taste und R, um ihn zu öffnen. Oder mit der rechten Maustaste in der Taskleiste, klicken Sie auf **Eigenschaften**, wählen die **Startmenü** Registerkarte, klicken Sie auf **anpassen**, und wählen Sie **Ausführungsbefehl**.)

In der **Verbindungen** Bereich, erweitern Sie den Serverknoten, und wählen Sie **Anwendungspools**. In der **Anwendungspools** Bereich, wenn **DefaultAppPool** ist .NET Frameworkversion 4, wie in der folgenden Abbildung zugewiesen, mit dem nächsten Abschnitt überspringen.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Wenn Sie nur zwei Anwendungspools finden Sie unter, und beide .NET Framework 2.0 festgelegt sind, müssen Sie ASP.NET 4 in IIS zu installieren:

- Öffnen Sie ein Eingabeaufforderungsfenster, indem Sie mit der rechten Maustaste **Eingabeaufforderung** in Windows **starten** Menü- und Auswählen von **als Administrator ausführen**. Führen Sie [Aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) So installieren Sie ASP.NET 4 in IIS mit den folgenden Befehlen. (Ersetzen Sie in 64-Bit-Systemen "Framework" mit "Framework64").

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Dieser Befehl erstellt neuen Anwendungspools für .NET Framework 4, aber der Standardanwendungspool wird weiterhin auf 2.0 festgelegt. Sie müssen eine Anwendung bereitstellen werden, dessen Ziel .NET 4 für diesen Anwendungspool müssen Sie den Anwendungspool in .NET 4 ändern.

Wenn Sie geschlossen **IIS-Manager**, führen Sie es erneut aus, erweitern Sie den Serverknoten, und klicken Sie auf **Anwendungspools** zum Anzeigen der **Anwendungspools** erneut auf den Bereich.

In der **Anwendungspools** Bereich, klicken Sie auf **DefaultAppPool**, und klicken Sie dann in der **Aktionen** Bereich auf **Grundeinstellungen**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

In der **Anwendungspool bearbeiten** ändern Sie im Dialogfeld **.NET Framework-Version** auf **.NET Framework-v4.0.30319** , und klicken Sie auf **OK**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Sie können nun in IIS veröffentlichen.

## <a name="publishing-to-iis"></a>Veröffentlichen in IIS

Es gibt mehrere Möglichkeiten, die Sie mithilfe von Visual Studio 2010 und Web Deploy bereitstellen können:

- Verwenden Sie Visual Studio One-Click-Veröffentlichung.
- Erstellen einer *Bereitstellungspaket* und installieren Sie es mithilfe der IIS-Manager-UI. Das Bereitstellungspaket besteht aus einem *ZIP* Datei, enthält alle Dateien und Metadaten, die zum Installieren eines Standorts in IIS erforderlich.
- Erstellen Sie ein Bereitstellungspaket, und installieren Sie es über die Befehlszeile.

Der Prozess, den Sie in den vorherigen Lernprogrammen zum Visual Studio einrichten, damit automatisieren gab es Bereitstellungsaufgaben gilt für alle diese drei Methoden. In diesen Lernprogrammen verwenden Sie die erste dieser Methoden. Informationen zur Verwendung von Bereitstellungspaketen finden Sie unter [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx).

Stellen Sie vor der Veröffentlichung sicher, dass Sie Visual Studio im Administratormodus ausgeführt werden. (In Windows 7 **starten** im Menü Maustaste auf das Symbol für die Version von Visual Studio, die Sie verwenden, und wählen Sie **als Administrator ausführen**.) Im Administratormodus ist erforderlich für die Veröffentlichung nur wenn Sie in IIS auf dem lokalen Computer veröffentlichen.

In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt ContosoUniversity (nicht auf das Projekt ContosoUniversity.DAL), und wählen Sie **veröffentlichen**.

Die **Web veröffentlichen** -Assistent wird angezeigt.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Wählen Sie in der Dropdown-Liste  **&lt;neu... &gt;**.

In der **neues Profil** (Dialogfeld), geben Sie "Test", und klicken Sie dann auf **OK**.

![Dialogfeld "Neues Profil"](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Dieser Name ist identisch mit der mittleren Knoten die Web.Test.config Datei transformieren, die Sie zuvor erstellt haben. Diese Zuordnung ist, was bewirkt, dass die Web.Test.config Transformationen angewendet werden, wenn Sie mit diesem Profil veröffentlichen.

Der Assistent springt automatisch zu den **Verbindung** Registerkarte.

In der **Dienst-URL** geben *"localhost"*.

In der **Website/Anwendung** geben *Default Web Site/ContosoUniversity*.

In der **Ziel-URL** geben `http://localhost/ContosoUniversity`.

Die **Ziel-URL** Einstellung ist nicht erforderlich. Wenn Visual Studio abgeschlossen ist, die Anwendung bereitstellen, wird Ihrem Standardbrowser an diese URL automatisch geöffnet. Wenn Sie nicht möchten, dass den Browser nach der Bereitstellung nicht automatisch geöffnet wird, lassen Sie dieses Feld leer.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Klicken Sie auf **Validate Connection** um sicherzustellen, dass die Einstellungen richtig sind und Sie können mit IIS auf dem lokalen Computer verbinden.

Ein grünes Häkchen überprüft, ob die Verbindung erfolgreich ist.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Klicken Sie auf **Weiter** um zum Wechseln der **Einstellungen** Registerkarte.

Die **Konfiguration** Dropdown-Feld gibt die Buildkonfiguration bereitstellen. Der Standardwert ist die Version, was Sie gewünscht ist.

Lassen Sie die **entfernen weiterer Dateien am Ziel** Kontrollkästchen deaktiviert. Da dies Ihre erste Bereitstellung ist, gibt es keine Dateien im Zielordner noch.

In der **Datenbanken** Abschnitt, geben Sie den folgenden Wert in das Feld die Verbindungszeichenfolge für **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Der Bereitstellungsprozess wird diese Verbindungszeichenfolge in der bereitgestellten Datei "Web.config" versetzt, da **verwenden diese Verbindungszeichenfolge zur Laufzeit** ausgewählt ist.

Auch unter **SchoolContext**Option **gelten Code First-Migrationen**. Diese Option bewirkt, dass der Bereitstellungsprozess so konfigurieren Sie die bereitgestellte Datei "Web.config" Angeben der `MigrateDatabaseToLatestVersion` Initialisierer. Diese Initialisierer aktualisiert die Datenbank automatisch auf die neueste Version, wenn die Anwendung Zugriff auf die Datenbank zum ersten Mal nach der Bereitstellung.

In das Feld die Verbindungszeichenfolge für **DefaultConnection**, geben Sie den folgenden Wert:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Lassen Sie **Datenbank aktualisieren** deaktiviert. Die Mitgliedschaftsdatenbank durch Kopieren die SDF-Datei in die App bereitgestellt werden\_Daten und Sie nicht möchten, der Bereitstellungsprozess mit dieser Datenbank weitere Schritte sind.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Klicken Sie auf **Weiter** um zum Wechseln der **Vorschau** Registerkarte.

In der **Vorschau** auf **starten Preview** um eine Liste der Dateien anzuzeigen, die kopiert werden.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Klicken Sie auf **Veröffentlichen**.

Wenn Visual Studio nicht im Administratormodus ist, erhalten Sie möglicherweise eine Fehlermeldung angezeigt, die einen Berechtigungsfehler angibt. In diesem Fall schließen Sie Visual Studio, öffnen Sie es im Administratormodus und versuchen Sie, erneut zu veröffentlichen.

Wenn Visual Studio im Administratormodus, ist die **Ausgabe** Fenster Berichte erfolgreich erstellen und veröffentlichen.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Der Browser wird automatisch geöffnet, um zur Startseite der Contoso-University in IIS auf dem lokalen Computer ausgeführt.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>Testen in der Testumgebung

Beachten Sie, dass der Indikator für die Umgebung "(Test)" zeigt anstelle von "(Dev)", die zeigt, dass die *"Web.config"* Transformation für den Indikator für die Umgebung erfolgreich war.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Führen Sie die **Studenten** Seite, um zu überprüfen, ob die bereitgestellte Datenbank kein Studenten verfügt. Bei Auswahl dieser Seite dauert möglicherweise einige Minuten, bis die geladen werden, da der Code First erstellt die Datenbank und führt dann die `Seed` Methode. (sie haben nicht, die ausgeführt werden, wenn Sie auf der Startseite waren, weil die Anwendung versucht hat nicht, Zugriff auf die Datenbank noch.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Führen Sie die **Lehrkräfte** Seite, um sicherzustellen, dass Code First mit Anfangsdaten die Datenbank mit Instructor-Daten gefüllt:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Wählen Sie **Studenten hinzufügen** aus der **Studenten** Menü eine Student hinzufügen, und zeigen Sie die neue Studenten in die **Studenten** Seite, um sicherzustellen, dass Sie erfolgreich in die Datenbank geschrieben werden können :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Aus der **Kurse** klicken Sie im Menü **Update Gutschriften**. Die **Update Gutschriften** Seite erfordert Administratorberechtigungen vorhanden sind, sodass der **anmelden** angezeigt wird. Geben Sie die Anmeldeinformationen des Administratorkontos, die Sie früher ("Admin" und "Pas$ w0rd") erstellt. Die **Update Gutschriften** Seite wird angezeigt, die überprüft, ob das Administratorkonto, das Sie im vorherigen Lernprogramm erstellt ordnungsgemäß in der testumgebung bereitgestellt wurde.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Überprüfen Sie, ob ein *Elmah* Ordner mit nur die Platzhalterdatei darin vorhanden ist.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Überprüfen die Änderungen für die automatische "Web.config" für die Code First-Migrationen

Öffnen der *"Web.config"* -Datei in der bereitgestellten Anwendung auf *C:\inetpub\wwwroot\ContosoUniversity* sehen Sie, auf dem der Bereitstellungsprozess Code First-Migrationen zu automatisch konfiguriert Aktualisieren Sie die Datenbank, auf die neueste Version.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Der Bereitstellungsprozess erstellt auch eine neue Verbindungszeichenfolge für die Code First-Migrationen verwenden ausschließlich für das Datenbankschema zu aktualisieren:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Diese zusätzliche Verbindungszeichenfolge können Sie ein Benutzerkonto für Datenbank-Schema-Updates und ein anderes Benutzerkonto für den Datenzugriff für die Anwendung angeben. Sie könnten z. B. zuweisen die Db\_Besitzer Code First-Migrationen und DB-\_Datareader und Db\_Datawriter Rollen der Anwendung. Dies ist ein allgemeines Verteidigung Muster, das verhindert, potenziell bösartigen Code in der Anwendung dass über das Datenbankschema zu ändern. (Beispielsweise kann dies auf einen erfolgreichen SQL Injection-Angriff vorkommen.) Dieses Muster wird nicht von diesen Lernprogrammen verwendet werden. Es gilt nicht für SQL Server Compact, und es gilt nicht, wenn die Migration zu SQL Server in einem späteren Lernprogramm dieser Reihe. Die Cytanium-Website bietet nur ein Benutzerkonto für den Zugriff auf die SQL Server-Datenbank, die Sie am Cytanium erstellen. Wenn Sie dieses Muster in Ihrem Szenario implementieren können, können Sie dies tun, indem Sie die folgenden Schritte ausführen:

1. In der **Einstellungen** auf der Registerkarte die **Web veröffentlichen** -Assistenten geben Sie die Verbindungszeichenfolge, die ein Benutzer mit vollständigen Berechtigungen zum Aktualisieren Schema gibt an, und Deaktivieren der **diese Verbindungszeichenfolge verwenden zur Laufzeit** Kontrollkästchen. In der bereitgestellten Datei "Web.config" Dies ist die `DatabasePublish` Verbindungszeichenfolge.
2. Erstellen Sie eine Web.config-Datei-Transformation für die Verbindungszeichenfolge, die die Anwendung zur Laufzeit verwendet werden soll.

Sie haben jetzt Ihre Anwendung in IIS auf dem Entwicklungscomputer bereitgestellt und es getestet. Dies stellt sicher, dass der Bereitstellungsprozess Inhalt der Anwendung auf den richtigen Speicherort kopiert (mit Ausnahme der Dateien, die Sie nicht bereitstellen möchten), und auch, Web Deploy IIS ordnungsgemäß während der Bereitstellung konfiguriert. In den nächsten Lernprogrammen, führen Sie eine weitere Tests, die eine Bereitstellungsaufgabe findet, die noch nicht durchgeführt: Festlegen von Berechtigungen für Ordner für die *Elmah* Ordner.

## <a name="more-information"></a>Weitere Informationen

Informationen zum Ausführen von IIS oder IIS Express in Visual Studio finden Sie unter den folgenden Ressourcen:

- [Übersicht über IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) auf der Website IIS.net.
- [Einführung in IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) Scott Guthries Blog.
- [Vorgehensweise: Angeben des Webservers für Webprojekte in Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Unterschiede zwischen IIS und ASP.NET Development Server Core](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) auf der ASP.NET-Website.
- [Testen Ihrer ASP.NET MVC- oder Web Forms-Anwendung unter IIS 7 in 30 Sekunden](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) in Rick andersons Blog. Dieser Eintrag enthält Beispiele für Gründe Tests mit Visual Studio Development Server (Cassini) nicht so zuverlässig wie Testen in IIS Express ist und warum Testen in IIS Express nicht so zuverlässig wie Testen in IIS ist.

Informationen darüber, welche Probleme auftreten kann, wenn Ihre Anwendung in mittlerer Vertrauenswürdigkeit ausgeführt wird, finden Sie unter [Hosten von ASP.NET-Anwendungen im mittlere Vertrauensebene](http://www.4guysfromrolla.com/articles/100307-1.aspx) auf die 4 Guys von Rolla-Website.

>[!div class="step-by-step"]
[Zurück](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
[Weiter](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
