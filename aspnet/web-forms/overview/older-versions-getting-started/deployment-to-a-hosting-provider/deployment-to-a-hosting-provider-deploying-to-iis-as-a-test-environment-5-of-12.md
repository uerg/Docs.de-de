---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: 'Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen in IIS als Testumgebung - 5, 12 | Microsoft-Dokumentation'
author: tdykstra
description: In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, die eine SQL Server Compact-Datenbank enthält, mithilfe von Visual Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: 70bc1b8e3c5d01470c2cebfbabd0c119e8f6d360
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394715"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a>Bereitstellen einer ASP.NET-Webanwendung mit SQL Server Compact mit Visual Studio oder Visual Web Developer: Bereitstellen in IIS als Testumgebung - 5, 12
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> In dieser tutorialreihe erfahren Sie, wie zum Bereitstellen einer ASP.NET-Anwendung (veröffentlichen) Webanwendungsprojekt, das eine SQL Server Compact-Datenbank mithilfe von Visual Studio 2012 RC oder Visual Studio Express 2012 RC für Web enthält. Sie können auch Visual Studio 2010 verwenden, wenn Sie die Web Publish Update installieren. Eine Einführung in die Reihe, finden Sie unter [im ersten Tutorial der Reihe](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Ein Lernprogramm, das zeigt, Bereitstellungsfunktionen, die nach der RC-Version von Visual Studio 2012 eingeführt wurden, zeigt, wie zum Bereitstellen von SQL Server-Editionen als SQL Server Compact und zeigt, wie Sie in Azure App Service-Web-Apps bereitstellen, finden Sie unter [ASP.NET-webbereitstellung Mithilfe von Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Übersicht

Dieses Tutorial veranschaulicht, wie eine ASP.NET-Webanwendung in IIS auf dem lokalen Computer bereitstellen.

Wenn Sie eine Anwendung entwickeln, testen Sie in der Regel, indem Sie sie in Visual Studio ausführen. Standardmäßig bedeutet dies, dass Sie Visual Studio Development Server (auch als Cassini bezeichnet) verwenden. Visual Studio Development Server erleichtert Ihnen während der Entwicklung in Visual Studio zu testen, aber es funktioniert nicht, genau wie IIS. Daher ist es möglich, dass eine Anwendung ordnungsgemäß ausgeführt wird, wenn Sie es in Visual Studio testen, jedoch fehl, wenn er für IIS in einer hostumgebung bereitgestellt wird.

Sie können Ihre Anwendung zuverlässiger auf folgende Arten testen:

1. Verwenden Sie IIS Express oder vollständige Variante von IIS anstelle von Visual Studio Development Server ein, wenn Sie während der Entwicklung in Visual Studio testen. Diese Methode emuliert die in der Regel genauer wie Ihre Website unter IIS ausgeführt wird. Aber diese Methode nicht getestet Ihres Bereitstellungsprozesses oder überprüfen, dass das Ergebnis des Bereitstellungsprozesses ordnungsgemäß ausgeführt wird.
2. Bereitstellen der Anwendung in IIS auf Ihrem Entwicklungscomputer unter Verwendung des gleichen Prozesses, die Sie später verwenden sie in Ihrer produktionsumgebung bereitstellen. Diese Methode überprüft, ob Ihr Bereitstellungsprozess zusätzlich zu überprüfen, den Ihre Anwendung unter IIS ordnungsgemäß ausgeführt wird.
3. Bereitstellen der Anwendung in einer testumgebung bereit, die so nah wie möglich in der produktionsumgebung bereit ist. Da die produktionsumgebung für diese Tutorials eine Drittanbieter-hosting-Anbieter ist, wäre die ideale testumgebung ein zweites Konto mit dem hosting-Anbieter. Verwenden Sie dieses zweite Konto nur für Tests, aber es würde die gleiche Weise wie das Produktionskonto einrichten festgelegt werden.

In diesem Tutorial wird gezeigt, die Schritte für Option 2. Anleitungen für die Option 3 finden Sie am Ende der [in der Produktionsumgebung bereitstellen](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) Tutorial und am Ende dieses Tutorials stehen Links zu Ressourcen für Option 1 aus.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, sollten Sie unbedingt die [Problembehandlungsseite](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="configuring-the-application-to-run-in-medium-trust"></a>Konfigurieren die Anwendung für die Ausführung in mittlerer Vertrauenswürdigkeit

Ändern Sie eine Einstellung einer "Web.config" vor dem Installieren von IIS und darauf bereitstellen, um den Standort führen mehr zu machen, wie es in einer typischen gemeinsamen Hostingumgebung wird.

Hostinganbieter in der Regel führen Sie Ihre Website *mittlerer Vertrauenswürdigkeit*, was bedeutet, es gibt einige Dinge, darf es nicht tun. Z. B. kann nicht Anwendungscode auf die Windows-Registrierung zugreifen und kann nicht lesen oder Schreiben von Dateien, die außerhalb der Ordnerhierarchie Ihrer Anwendung sind. Wird standardmäßig die Anwendung ausgeführt wird, im *hohe Vertrauensebene* auf dem lokalen Computer, was bedeutet, dass es sich bei die Anwendung möglicherweise mehr Möglichkeiten, die fehlschlagen, wenn Sie sie in der produktionsumgebung bereitstellen können. Damit wird der testumgebung, die mehr genau wider die produktionsumgebung, konfigurieren Sie daher die Anwendung mit mittlerer Vertrauenswürdigkeit ausgeführt.

Fügen Sie in der Datei "Web.config" der Anwendung eine **Vertrauensstellung** Element in der **"System.Web"** Element, wie im folgenden Beispiel gezeigt.

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

Die Anwendung wird jetzt auch auf dem lokalen Computer mittlerer Vertrauenswürdigkeit in IIS ausgeführt. Mit dieser Einstellung können Sie so früh wie möglich alle Versuche vom Anwendungscode, etwas zu tun abfangen, die in der produktionsumgebung ein Fehler auftritt.

> [!NOTE]
> Wenn Sie Entity Framework Code First-Migrationen verwenden, stellen Sie sicher, dass Sie Version 5.0 oder höher installiert. In Entity Framework, Version 4.3 Migrationen erfordert volles Vertrauen um das Datenbankschema zu aktualisieren.


## <a name="installing-iis-and-web-deploy"></a>Installieren von IIS und Web bereitstellen

Zum Bereitstellen in IIS auf dem Entwicklungscomputer müssen Sie IIS und Web Deploy installiert. Diese sind nicht in der Standardkonfiguration von Windows 7 enthalten. Wenn Sie sowohl IIS als auch Web Deploy bereits installiert haben, fahren Sie mit dem nächsten Abschnitt.

Mithilfe der [Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx) ist die bevorzugte Methode für IIS und Web Deploy zu installieren, da der Webplattform-Installer installiert die empfohlene Konfiguration für IIS und automatisch die erforderlichen Komponenten für IIS und Web installiert Stellen Sie bei Bedarf bereit.

Führen Sie zum Installieren von IIS und Web Deploy-Webplattform-Installer mit den folgenden Link. Wenn Sie IIS, Web Deploy oder die erforderlichen Komponenten bereits installiert haben, installiert den Webplattform-Installer nur was fehlt.

- [Installieren von IIS und Web Deploy unter Verwendung von WebPI](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a>Durch Festlegen des Standardanwendungspools auf .NET 4

Führen Sie nach dem Installieren von IIS, **IIS-Manager** um sicherzustellen, dass .NET Framework, Version 4 des Standardanwendungspools zugewiesen ist.

Von der Windows **starten** , wählen Sie im Menü **ausführen**, geben Sie "Inetmgr", und klicken Sie dann auf **OK**. (Wenn die **ausführen** Befehl ist nicht in Ihrem **starten** Menü können Sie drücken, die Windows-Taste und R, um ihn zu öffnen. Oder mit der rechten Maustaste in der Taskleiste, klicken Sie auf **Eigenschaften**, wählen die **Menü "Start"** auf **anpassen**, und wählen Sie **Ausführungsbefehl**.)

In der **Verbindungen** Bereich, erweitern Sie den Serverknoten und wählen Sie **Anwendungspools**. In der **Anwendungspools** Bereich Wenn **DefaultAppPool** ist .NET Frameworkversion 4, wie in der folgenden Abbildung werden zugewiesen, mit dem nächsten Abschnitt überspringen.

[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)

Wenn Sie sehen, dass nur zwei Anwendungspools, und beide Angaben, die auf .NET Framework 2.0 festgelegt sind, müssen Sie ASP.NET 4 in IIS zu installieren:

- Öffnen Sie ein Eingabeaufforderungsfenster, indem Sie mit der rechten Maustaste **Eingabeaufforderung** in der Windows **starten** Menü und auswählen **als Administrator ausführen**. Führen Sie dann [Aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) ASP.NET 4 in IIS mit den folgenden Befehlen installieren. (Ersetzen Sie in der 64-Bit-Systemen "Framework" mit "Framework64").

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    [![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)

    Dieser Befehl erstellt neue Anwendungspools für die .NET Framework 4, aber der Standardanwendungspool wird immer noch auf 2.0 festgelegt. Sie müssen einer Anwendung bereitstellen, das als Ziel .NET 4 an diesen Anwendungspool, müssen Sie den Anwendungspool in .NET 4 zu ändern.

Wenn Sie geschlossen **IIS-Manager**, führen sie erneut aus, erweitern Sie den Serverknoten und klicken Sie auf **Anwendungspools** zum Anzeigen der **Anwendungspools** erneut zum Bereich.

In der **Anwendungspools** Bereich klicken Sie auf **DefaultAppPool**, und klicken Sie dann in der **Aktionen** auf **Grundeinstellungen**.

[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)

In der **Anwendungspool bearbeiten** im Dialogfeld **.NET Framework-Version** zu **.NET Framework-v4.0.30319** , und klicken Sie auf **OK**.

[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)

Sie können jetzt in IIS veröffentlichen.

## <a name="publishing-to-iis"></a>Veröffentlichung in IIS

Es gibt mehrere Möglichkeiten, die Sie mithilfe von Visual Studio 2010 und Web Deploy bereitstellen können:

- Verwenden Sie Visual Studio-One-Click-Veröffentlichung.
- Erstellen Sie eine *Bereitstellungspaket* herunter, und installieren Sie es mithilfe der IIS-Manager-UI. Das Bereitstellungspaket besteht aus einem *ZIP* -Datei, enthält alle Dateien und Metadaten, die zum Installieren eines Standorts in IIS erforderlich.
- Erstellen eines Bereitstellungspakets aus, und installieren Sie es über die Befehlszeile.

Der Prozess, die in Visual Studio einrichten, um automatisieren den vorherigen Tutorials durcharbeiten Bereitstellungsaufgaben gilt für alle diese drei Methoden. In diesen Tutorials verwenden Sie die erste dieser Methoden. Weitere Informationen zur Verwendung von Bereitstellungspaketen, finden Sie unter [ASP.NET die ASP.NET-Bereitstellung](https://msdn.microsoft.com/library/bb386521.aspx).

Stellen Sie sicher, dass Sie Visual Studio im Administratormodus ausgeführt werden, vor der Veröffentlichung. (In der Windows 7 **starten** Menü, mit der rechten Maustaste in des Symbols für die Version von Visual Studio, die Sie verwenden, und wählen Sie **als Administrator ausführen**.) Im Administratormodus ist erforderlich, für die Veröffentlichung nur wenn Sie in IIS auf dem lokalen Computer veröffentlichen.

In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt ContosoUniversity (nicht auf das Projekt ContosoUniversity.DAL), und wählen Sie **veröffentlichen**.

Die **Webveröffentlichung** -Assistent wird angezeigt.

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

Wählen Sie in der Dropdown-Liste  **&lt;neu... &gt;**.

In der **neues Profil** Dialogfeld Geben Sie "Test", und klicken Sie dann auf **OK**.

![Dialogfeld "Neues Profil"](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

Dieser Name ist identisch mit der mittlere Knoten die Web.Test.config umzuwandeln, die Sie zuvor erstellt haben. Diese Übereinstimmung ist, was bewirkt, dass die Web.Test.config Transformationen angewendet werden, wenn Sie mit diesem Profil veröffentlichen.

Der Assistent springt automatisch zu den **Verbindung** Registerkarte.

In der **-Dienst-URL** geben *"localhost"*.

In der **Website/Anwendung** geben *Default Web Site/ContosoUniversity*.

In der **Ziel-URL** geben `http://localhost/ContosoUniversity`.

Die **Ziel-URL** Einstellung ist nicht erforderlich. Wenn Visual Studio abgeschlossen ist, die Anwendung bereitstellen, wird er automatisch Ihren Standardbrowser mit dieser URL geöffnet. Wenn Sie nicht möchten, dass den Browser nach der Bereitstellung automatisch geöffnet wird, lassen Sie dieses Feld leer.

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

Klicken Sie auf **Verbindung überprüfen** zu überprüfen, ob die Einstellungen richtig sind und Sie können mit IIS auf dem lokalen Computer verbinden.

Ein grünes Häkchen stellt sicher, dass die Verbindung erfolgreich ist.

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

Klicken Sie auf **Weiter** , fahren Sie fort, um die **Einstellungen** Registerkarte.

Die **Konfiguration** Dropdown-Feld gibt an, die Build-Konfiguration bereitstellen. Der Standardwert ist die Version, die ist, was Sie wollen.

Lassen Sie die **entfernen weiterer Dateien am Ziel** das Kontrollkästchen deaktiviert. Da dies Ihre erste Bereitstellung ist, gibt es keine Dateien im Zielordner noch.

In der **Datenbanken** Geben Sie den folgenden Wert in das Feld die Verbindungszeichenfolge für **SchoolContext**:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

Während des Bereitstellungsvorgangs wird diese Verbindungszeichenfolge in der bereitgestellten Web.config-Datei einfügen, da **verwenden Sie diese Verbindungszeichenfolge zur Laufzeit** ausgewählt ist.

Außerdem **SchoolContext**Option **anwenden Code First-Migrationen**. Diese Option bewirkt, dass den Bereitstellungsprozess so konfigurieren Sie die bereitgestellte Datei "Web.config", an die `MigrateDatabaseToLatestVersion` Initialisierer. Diese Initialisierer aktualisiert die Datenbank automatisch auf die neueste Version, wenn die Anwendung zum ersten Mal nach der Bereitstellung auf die Datenbank zugreift.

Klicken Sie in das Feld die Verbindungszeichenfolge für **DefaultConnection**, geben Sie den folgenden Wert:

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

Lassen Sie **Aktualisieren einer Datenbank** gelöscht. Die Mitgliedschaftsdatenbank wird bereitgestellt durch Kopieren die SDF-Datei in App\_Daten, und Sie möchten nicht den Bereitstellungsprozess mit dieser Datenbank weitere Schritte.

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

Klicken Sie auf **Weiter** , fahren Sie fort, um die **Vorschau** Registerkarte.

In der **Vorschau** auf **Vorschau starten** um eine Liste der Dateien anzuzeigen, die kopiert werden.

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

Klicken Sie auf **Veröffentlichen**.

Ist Visual Studio nicht im Administratormodus, erhalten Sie möglicherweise eine Fehlermeldung angezeigt, die einen Berechtigungsfehler angibt. In diesem Fall schließen Sie Visual Studio im Administratormodus öffnen, und versuchen Sie es erneut veröffentlichen.

Wenn Visual Studio im Administratormodus, ist die **Ausgabe** Fenster Berichte erfolgreich erstellen und veröffentlichen.

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

Der Browser öffnet automatisch der Contoso University-Startseite in IIS ausgeführt wird, auf dem lokalen Computer.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)

## <a name="testing-in-the-test-environment"></a>In der Testumgebung testen

Beachten Sie, dass der Indikator für die Umgebung "(Test)" zeigt anstelle von "(Dev)", die zeigt, dass die *"Web.config"* Transformation für den Indikator für die Umgebung erfolgreich war.

[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)

Führen Sie die **Schüler/Studenten** Seite, um sicherzustellen, dass die bereitgestellte Datenbank keine Schüler/Studenten verfügt. Bei der Auswahl auf dieser Seite dauert möglicherweise einige Minuten, die geladen werden, da der Code First die Datenbank wird erstellt und führt dann die `Seed` Methode. (Es nicht, die ausgeführt werden, wenn Sie auf der Startseite waren, weil die Anwendung versucht nicht, Zugriff auf die Datenbank noch.)

[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)

Führen Sie die **Dozenten** Seite, um sicherzustellen, dass Code First ein die Datenbank mit "Instructor" Daten Seeding:

[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)

Wählen Sie **hinzufügen Schüler/Studenten** aus der **Schüler/Studenten** Menü ein Schüler/Student hinzufügen, und zeigen Sie den neuen Studenten in der **Schüler/Studenten** Seite, um sicherzustellen, dass Sie erfolgreich in die Datenbank schreiben können :

[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)

[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)

Von der **Kurse** , wählen Sie im Menü **Update Gutschriften**. Die **Update Gutschriften** Seite sind Administratorberechtigungen erforderlich, sodass die **anmelden** angezeigt wird. Geben Sie Anmeldeinformationen für das Administratorkonto, dass Sie frühere ("Admin" und "Pas$ w0rd") erstellt. Die **Update Gutschriften** Seite wird angezeigt, die überprüft, dass das Administratorkonto, das Sie im vorherigen Tutorial erstellt haben, ordnungsgemäß in der testumgebung bereitgestellt wurde.

[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)

[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)

Überprüfen Sie, ob ein *Elmah* Ordner mit dem nur der Platzhalterdatei in ihrem vorhanden ist.

[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a>Überprüfen die Änderungen für die automatische "Web.config" für Code First-Migrationen

Öffnen der *"Web.config"* -Datei in der bereitgestellten Anwendung auf *C:\inetpub\wwwroot\ContosoUniversity* und Sie sehen, in denen während des Bereitstellungsvorgangs Code First-Migrationen zu automatisch konfiguriert Aktualisieren Sie die Datenbank, auf die neueste Version.

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

Der Bereitstellungsprozess erstellt auch eine neue Verbindungszeichenfolge für die Code First-Migrationen verwenden ausschließlich für das Datenbankschema zu aktualisieren:

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

Diese zusätzliche Verbindungszeichenfolge können Sie ein Benutzerkonto für Datenbank-Schema-Updates und ein anderes Benutzerkonto für den Datenzugriff für die Anwendung angeben. Sie könnten z. B. die Datenbank zuweisen\_Rolle "Besitzer" Code First-Migrationen und Db\_Datareader und Db\_Datawriter Rollen der Anwendung. Dies ist ein gängiges Defense-in-Depth-Muster, das verhindert, dass potenziell bösartigen Code in der Anwendung das Datenbankschema zu ändern. (Zum Beispiel könnte dies in einen erfolgreichen SQL Injection-Angriff vorkommen.) Dieses Muster wird nicht von diesen Tutorials verwendet. Es gilt nicht für SQL Server Compact, und es gilt nicht bei der Migration zu SQL Server in einem späteren Tutorial dieser Reihe. Die Cytanium-Website bietet nur ein Benutzerkonto für den Zugriff auf SQL Server-Datenbank, die zur Cytanium erstellt. Wenn Sie dieses Muster in Ihrem Szenario zu implementieren sind, können Sie dies tun, indem Sie die folgenden Schritte ausführen:

1. In der **Einstellungen** Registerkarte die **Webveröffentlichung** -Assistenten geben Sie die Verbindungszeichenfolge, die ein Benutzer mit vollständigen Berechtigungen zum Aktualisieren Schema gibt an, und Deaktivieren der **diese Verbindungszeichenfolge verwenden zur Laufzeit** Kontrollkästchen. In der bereitgestellten Web.config-Datei, dies ist die `DatabasePublish` Verbindungszeichenfolge.
2. Erstellen Sie eine Web.config-Datei-Transformation für die Verbindungszeichenfolge, die Sie die Anwendung zur Laufzeit verwenden soll.

Sie haben nun Ihre Anwendung in IIS auf Ihrem Entwicklungscomputer bereitgestellt und es getestet. Dies stellt sicher, dass während des Bereitstellungsvorgangs der Inhalt der Anwendung auf den richtigen Speicherort kopiert (mit Ausnahme der Dateien, die Sie nicht bereitstellen möchten), und auch, Web Deploy IIS ordnungsgemäß während der Bereitstellung konfiguriert. Führen Sie im nächsten Tutorial, einem weiteren Test, die eine Bereitstellungsaufgabe findet, die noch nicht geschehen: Festlegen von Berechtigungen für den Ordner für die *Elmah* Ordner.

## <a name="more-information"></a>Weitere Informationen

Informationen zum Ausführen von IIS oder IIS Express in Visual Studio finden Sie unter den folgenden Ressourcen:

- [Übersicht über IIS Express](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) auf der Website IIS.net.
- [Einführung in IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) auf Scott Guthries Blog.
- [Vorgehensweise: angeben den Webserver für Webprojekte in Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).
- [Unterschiede zwischen IIS und ASP.NET Development Server Core-](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) auf der ASP.NET-Website.
- [Testen Ihrer ASP.NET MVC- oder Web Forms-Anwendung unter IIS 7 in 30 Sekunden](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) andersons Blog. Dieser Eintrag enthält Beispiele für Gründe Tests mit Visual Studio Development Server (Cassini) nicht so zuverlässig wie Tests in IIS Express ist und warum Testen in IIS Express nicht so zuverlässig wie Tests in IIS ist.

Weitere Informationen darüber, welche Probleme auftreten kann, wenn Ihre Anwendung bei mittlerer Vertrauenswürdigkeit ausgeführt wird, finden Sie unter [Hosting von ASP.NET-Anwendungen in mittlere Vertrauensebene](http://www.4guysfromrolla.com/articles/100307-1.aspx) auf die 4 Guys Rolla-Standorts.

> [!div class="step-by-step"]
> [Zurück](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
> [Weiter](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)
