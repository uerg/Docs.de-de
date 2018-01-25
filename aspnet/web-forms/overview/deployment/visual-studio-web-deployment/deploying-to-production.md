---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellung bis hin zur Produktion | Microsoft Docs'
author: tdykstra
description: "Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder mit einem Hostinganbieter von Drittanbietern durch wählen..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: abd3f3f78dd9a9e6394e2f61aa9bd692810ca875
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>ASP.NET Web-Bereitstellung mit Visual Studio: Bereitstellung bis hin zur Produktion
====================
Durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Diese Reihe von Lernprogrammen wird gezeigt, wie bereitstellen (veröffentlichen) aus einer ASP.NET web-Anwendung eines Drittanbieters Hostinganbieter oder Azure App Service-Web-Apps mithilfe von Visual Studio 2012 oder Visual Studio 2010. Informationen über die Reihen finden Sie unter [im ersten Lernprogramm, in der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

In diesem Lernprogramm Sie ein Microsoft Azure-Konto einrichten, Staging-und produktionsumgebungen zu erstellen und Bereitstellen Ihrer ASP.NET-Webanwendung auf die Staging- und produktionsumgebungen mithilfe von Visual Studio einmalklick-Funktion zu veröffentlichen.

Falls gewünscht, können Sie mit einem Drittanbieter-Hostinganbieter bereitstellen. Die meisten der in diesem Lernprogramm beschriebene Verfahren sind identisch für einen Hostinganbieter oder für Azure, mit dem Unterschied, dass jeder Anbieter eine eigene Benutzeroberfläche für die Verwaltung von Websites und -Konto verfügt. Sie erhalten einen Hostinganbieter in der [Katalog von Anbietern](https://www.microsoft.com/web/hosting) auf der Microsoft.com-Website.

Hinweis: Wenn Sie eine Fehlermeldung erhalten, oder etwas funktioniert nicht, wenn Sie das Lernprogramm absolvieren, überprüfen Sie die Seite "Problembehandlung" in diesem Lernprogramm Reihe sein.

## <a name="get-a-microsoft-azure-account"></a>Richten Sie ein Microsoft Azure-Konto

Wenn Sie ein Azure-Konto noch nicht haben, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose Azure-Testversion](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Erstellen einer Stagingumgebung

> [!NOTE]
> Da dieses Lernprogramm geschrieben wurde, hinzugefügt Azure App Service ein neues Feature, um viele der Prozesse zum Erstellen von Staging-und produktionsumgebungen zu automatisieren. Finden Sie unter [Einrichten von Stagingumgebungen für webapps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).


Wie in beschrieben die [bereitstellen, um das Lernprogramm Umgebung testen](deploying-to-iis.md), wird der zuverlässige testumgebung ist eine Website auf dem Hostinganbieter, die wie bei der Website für die Produktion hat. Viele Hostinganbietern, müssten Sie die Vorteile dieser gegen erhebliche zusätzliche Kosten abwägen, aber in Azure erstellen Sie eine zusätzliche kostenloses Web-app als der staging-app. Sie benötigen auch eine Datenbank und die zusätzliche Kosten für, die über die Ausgaben für die Produktionsdatenbank wird entweder "keiner" sein oder minimalen. In Azure bezahlen Sie für die Menge an Datenbankspeicher, die Sie verwenden, anstatt für jede Datenbank, und die Menge an zusätzlichem Speicher, die Teil einer Stagingumgebung verwendet werden, werden minimal sein.

Wie in beschrieben die [bereitstellen, um das Lernprogramm Testumgebung](deploying-to-iis.md), Staging und Produktion, Sie sind im Begriff, die zwei Datenbanken in einer Datenbank bereitstellen. Wenn Sie sie trennen möchten, würde der Prozess identisch sein, Sie eine zusätzliche Datenbank für jede Umgebung erstellen, und Sie die richtige Zielzeichenfolge für jede Datenbank Wählen bei der Erstellung des Veröffentlichungsprofils.

In diesem Abschnitt des Lernprogramms erstellen Sie eine Web-app und die Datenbank für die Stagingumgebung verwendet, und Sie in die Stagingumgebung bereitstellen und Testen Sie es vor dem Erstellen und in der produktionsumgebung bereitstellen.

> [!NOTE]
> Die folgenden Schritte zeigen, wie eine Web-app in Azure App Service mithilfe der Azure-Verwaltungsportal zu erstellen. In der neuesten Version des Azure SDK können Sie auch dazu ohne Visual Studio mithilfe von Server-Explorer verlassen zu müssen. In Visual Studio 2013 können Sie eine Web-app auch direkt über das Dialogfeld "Veröffentlichen" erstellen. Weitere Informationen finden Sie unter [eine ASP.NET Web-app in Azure App Service erstellen.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. In der [Azure-Verwaltungsportal](https://manage.windowsazure.com/), klicken Sie auf **Websites**, und klicken Sie dann auf **neu**.
2. Klicken Sie auf **Website**, und klicken Sie dann auf **Benutzerdefiniert erstellen**.

    Die **neue Website - Benutzerdefiniert erstellen** -Assistent wird geöffnet. Die **Benutzerdefiniert erstellen** -Assistenten können Sie eine Website und eine Datenbank zur gleichen Zeit zu erstellen.
3. In der **Website erstellen** Schritt des Assistenten geben Sie eine Zeichenfolge in der **URL** Feld als eindeutige URL für Ihre Anwendung verwendet die Stagingumgebung. Geben Sie beispielsweise ContosoUniversity-staging123 (einschließlich Zufallszahlen am Ende, damit es eindeutig ist, für den Fall, dass ContosoUniversity Staging berücksichtigt wird).

    Die vollständige URL besteht aus Ihnen hier eingegebenen plus das Suffix, das neben dem Textfeld angezeigt.
4. In der **Region** Dropdown-Liste, wählen Sie die Region, die Ihnen am nächsten liegt.

    Diese Einstellung gibt an, das Rechenzentrum, die Ihre Web-app ausgeführt wird.
5. In der **Datenbank** Dropdown- Liste **erstellen Sie eine neue SQL­Datenbank**.
6. In der **Name der Datenbankverbindungszeichenfolge** Feld, behalten Sie den Standardwert *DefaultConnection*.
7. Klicken Sie auf den Pfeil nach rechts am unteren Rand der Box.

    Die folgende Abbildung zeigt die **Website erstellen** Dialog mit Beispielwerte darin. Die URL und die Region, die von Ihnen eingegebene wird abweichen.

    ![Erstellen der Website Schritt](deploying-to-production/_static/image1.png)

    Der Assistent springt zu der **angeben von datenbankeinstellungen** Schritt.
8. In der **Namen** geben *ContosoUniversity* plus eine zufällige Zahl ein, damit sie eindeutig, z. B. *ContosoUniversity123*.
9. In der **Server** wählen Sie im **neue SQL-Datenbankserver**.
10. Geben Sie einen Administratornamen und das Kennwort ein.

    Sie sind keinen vorhandenen Namen und Kennwort hier eingeben. Sie sind ein neuer Name und Kennwort, das Sie definieren jetzt, um Sie später verwenden, wenn der Zugriff auf die Datenbank eingeben.
11. In der **Region** wählen Sie die gleiche Region, die Sie für die Web-app ausgewählt haben.

    Halten den Webserver und dem Datenbankserver in derselben Region bietet Ihnen die beste Leistung und Kosten minimiert.
12. Klicken Sie auf das Häkchen am unteren Rand Feld, um anzugeben, dass Sie fertig sind.

    Die folgende Abbildung zeigt die **angeben von datenbankeinstellungen** Dialog mit Beispielwerte darin. Die eingegebenen Werte können abweichen.

    ![Datenbank-Einstellungen Schritt erstellen Sie neue Website - mit Datenbank-Assistenten](deploying-to-production/_static/image2.png)

    Das Verwaltungsportal auf der Seite "Websites" zurückgibt und die **Status** Spalte wird angezeigt, die Web-app erstellt wird. Nach einer Weile (in der Regel weniger als eine Minute) die **Status** Spalte zeigt, dass die Web-app wurde erfolgreich erstellt wurde. In der Navigationsleiste auf der linken Seite die Anzahl der Web-apps, die Sie in Ihrem Konto haben wird neben der **Websites** Symbol und die Anzahl von Datenbanken wird neben der **SQL-Datenbanken** Symbol.

    ![Seite "Websites" der erstellten Website-Verwaltungsportal](deploying-to-production/_static/image3.png)

    Ihre Web-app-Name wird von der Beispiel-app in der Abbildung abweichen.

## <a name="deploy-the-application-to-staging"></a>Die Anwendung in die Stagingumgebung bereitstellen

Nun, dass Sie eine Web-app und die Datenbank für die Stagingumgebung erstellt haben, können Sie das Projekt darauf bereitstellen.

> [!NOTE]
> Diese Anweisungen gezeigt, wie durch Herunterladen ein Veröffentlichungsprofils erstellen eine *publishsettings* -Datei, die nicht nur für Azure, sondern auch für Drittanbieter-Hostinganbieter funktioniert. Die neuesten ermöglicht Azure SDK auch direkt in Azure aus Visual Studio verbinden, und wählen aus einer Liste von webapps, die Sie in Ihrem Azure-Konto verfügen. In Visual Studio 2013, Sie können sich anmelden in Azure aus der **Web Publish** Dialogfeld oder aus der **Server-Explorer** Fenster. Weitere Informationen finden Sie unter [erstellen eine ASP.NET Web-app in Azure App Service](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).


### <a name="download-the-publishsettings-file"></a>Herunterladen der PUBLISHSETTINGS-Datei

1. Klicken Sie auf den Namen der Web-app, die Sie gerade erstellt haben.

    ![Klicken Sie auf der Website, um zum Dashboard zu wechseln.](deploying-to-production/_static/image4.png)
2. Klicken Sie unter **kurzer Blick** in der **Dashboard** auf **Herunterladen eines Veröffentlichungsprofils**.

    ![Herunterladen des Veröffentlichungsprofils link](deploying-to-production/_static/image5.png)

    Dieser Schritt lädt eine Datei, die alle Einstellungen enthält, die Sie zum Bereitstellen einer Anwendung für Ihre Web-app benötigen. Sie müssen diese Datei in Visual Studio importieren, daher ist es nicht auf diese Informationen manuell eingeben.
3. Speichern Sie die *publishsettings* Datei in einem Ordner, die Sie über Visual Studio zugreifen können.

    ![Speichern die PUBLISHSETTINGS-Datei](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Sicherheit – die *publishsettings* -Datei enthält die (nicht codierten) Anmeldeinformationen, die zur Verwaltung der Azure-Abonnements und-Dienste verwendet werden. Die bewährte Sicherheitsmethode für diese Datei ist vorübergehend außerhalb von Quellverzeichnissen (z. B. im Ordner Libraries\Documents) zu speichern, und es dann zu löschen, nachdem der Import abgeschlossen ist. Ein böswilliger Benutzer, die erhält Zugriff auf die *publishsettings* Datei erstellen, bearbeiten und löschen Sie die Azure-Dienste kann.

### <a name="create-a-publish-profile"></a>Erstellen eines Veröffentlichungsprofils

1. In Visual Studio mit der Maustaste des Projekts ContosoUniversity im **Projektmappen-Explorer** , und wählen Sie **veröffentlichen** aus dem Kontextmenü.

    Die **Web veröffentlichen** -Assistent wird geöffnet.
2. Klicken Sie auf die **Profil** Registerkarte.
3. Klicken Sie auf **Import**.
4. Navigieren Sie zu der *publishsettings* zuvor heruntergeladene Datei, und klicken Sie dann auf **öffnen**.

    ![Veröffentlichungseinstellungen-Dialogfeld "Importieren"](deploying-to-production/_static/image7.png)
5. In der **Verbindung** auf **Validate Connection** um sicherzustellen, dass die Einstellungen richtig sind.

    Wenn die Verbindung erfolgreich überprüft wurde, wird ein grünes Häkchen neben gezeigt die **Validate Connection** Schaltfläche.

    Für einige Hostinganbieter erhalten die Möglichkeit, wenn Sie auf **Validate Connection**, wird möglicherweise eine **Zertifikatfehler** (Dialogfeld). Wenn Sie dies tun, stellen Sie sicher, dass der Servername ist, was Sie erwarten. Wenn der Servername korrekt ist, wählen Sie **dieses Zertifikat für zukünftige Sitzungen von Visual Studio speichern** , und klicken Sie auf **Accept**. (Dieser Fehler weist darauf hin, die Vermeidung der Kosten für erwerben ein SSL-Zertifikat für die URL, den Sie zum Bereitstellen des hostenden Anbieters ausgewählt wurde. Falls gewünscht, eine sichere Verbindung hergestellt wird, wird die Verwendung eines gültigen Zertifikats, wenden Sie sich an Ihren Hostinganbieter.)
6. Klicken Sie auf **Weiter**.

    ![Symbol "Verbindung erfolgreich" und die Schaltfläche "Weiter", auf der Registerkarte "Verbindung"](deploying-to-production/_static/image8.png)
7. In der **Einstellungen** Registerkarte **Dateiveröffentlichungsoptionen**, und wählen Sie dann **Ausschließen von Dateien aus der App\_Datenordner**.

    Informationen zu den anderen Optionen unter **Dateiveröffentlichungsoptionen**, finden Sie unter der [deploying an IIS](deploying-to-iis.md) Lernprogramm. Der Screenshot wird, wird angegeben, die das Ergebnis von diesem Schritt und die folgenden Konfigurationsschritte für die Datenbank am Ende der Schritte für die Datenbank ist.
8. Klicken Sie unter **DefaultConnection** in der **Datenbanken** Abschnitt, konfigurieren Sie datenbankbereitstellung für die Mitgliedschaftsdatenbank.
9. 1. Wählen Sie **Datenbank aktualisieren**.

        Die **Remote Verbindungszeichenfolge** Feld direkt unterhalb **DefaultConnection** sich mit der Verbindungszeichenfolge aus der PUBLISHSETTINGS-Datei gefüllt ist. Die Verbindungszeichenfolge enthält SQL Server-Anmeldeinformationen, die im nur-Text gespeichert sind die *pubxml* Datei. Falls gewünscht, sie es dauerhaft zu speichern, können Sie sie aus dem Veröffentlichungsprofil ein entfernen, nachdem die Datenbank bereitgestellt wird und bewahren sie stattdessen in Azure. Weitere Informationen finden Sie unter [wie Sie Ihre Datenbank ASP.NET Verbindungszeichenfolgen schützen bei der Bereitstellung in Azure aus Quelle](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) Scott Hanselman Blog.
    2. Klicken Sie auf **Datenbankupdates konfigurieren**.
    3. In der **Datenbankupdates konfigurieren** (Dialogfeld), klicken Sie auf **SQL-Skript hinzufügen**.
    4. In der **SQL-Skript hinzufügen** Feld, navigieren Sie zu der *Aspnet-Daten-prod.sql* Skript, das Sie zuvor im Projektmappenordner gespeichert, und klicken Sie dann auf **öffnen**.
    5. Schließen der **Datenbankupdates konfigurieren** (Dialogfeld).
10. Klicken Sie unter **SchoolContext** in der **Datenbanken** Abschnitt **führen Sie Code First-Migrationen (wird beim Anwendungsstart ausgeführt)**.

    Zeigt Visual Studio **führen Sie Code First-Migrationen** anstelle von **Update Database** für `DbContext` Klassen. Gegebenenfalls Migrationen zum Bereitstellen einer Datenbank, die Sie zugreifen, indem Sie mit den DbDacFx-Anbieter verwenden ein `DbContext` Klasse, finden Sie unter [wie ich eine Code First Datenbank ohne Migrationen bereitstellen?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) in der Web-Bereitstellung – häufig gestellte Fragen für Visual Studio und ASP.NET auf MSDN.

    Die **Einstellungen** Registerkarte sieht nun wie im folgenden Beispiel:

    ![Registerkarte "Einstellungen" für das staging](deploying-to-production/_static/image9.png)
11. Führen Sie die folgenden Schritte aus, um das Profil zu speichern und benennen Sie sie um *Staging*:

    1. Klicken Sie auf die **Profil** Registerkarte, und klicken Sie dann auf **Profile verwalten**.
    2. Der Import erstellt zwei neue Profile, eine für FTP und eine für Web Deploy. Sie konfiguriert das Web Deploy-Profil: Benennen Sie dieses Profil, um *Staging*.

        ![Profil in die Stagingumgebung umbenennen](deploying-to-production/_static/image10.png)
    3. Schließen der **Web veröffentlichen Profile bearbeiten** (Dialogfeld).
    4. Schließen der **Web veröffentlichen** Assistenten.

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Konfigurieren Sie für den Indikator Umgebung Transformation Profil veröffentlichen

> [!NOTE]
> In diesem Abschnitt wird gezeigt, wie Sie eine Transformation "Web.config" für den Indikator für die Umgebung einrichten. Da der Indikator wird die `<appSettings>` -Element, haben Sie eine Alternative zur Transformation für das angeben, wenn Sie auf Azure App Service bereitstellen. Weitere Informationen finden Sie unter [angeben "Web.config"-Einstellungen in Azure](web-config-transformations.md#watransforms).


1. In **Projektmappen-Explorer**, erweitern Sie **Eigenschaften**, und erweitern Sie dann **PublishProfiles**.
2. Mit der rechten Maustaste *Staging.pubxml*, und klicken Sie dann auf **Config Transformation hinzufügen**.

    ![Fügen Sie für das Staging Konfigurationstransformation hinzu](deploying-to-production/_static/image11.png)

    Visual Studio erstellt die *Web.Staging.config* Transform-Datei und öffnet diese.
3. In der *Web.Staging.config* Transformationsdatei, fügen Sie den folgenden Code unmittelbar nach dem Öffnen `configuration` Tag.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Wenn Sie das Veröffentlichungsprofil Staging verwenden, legt diese Transformation den Indikator "Umgebung", "Prod" fest. Wird ein Suffix z. B. "(Dev)" oder "(Test)" nach der "Contoso University" H1-Überschrift nicht in der bereitgestellten Web-app angezeigt.
4. Mit der rechten Maustaste die *Web.Staging.config* Datei, und klicken Sie auf **Vorschau transformieren** um sicherzustellen, dass die Transformation, die Sie codiert die erwartete Änderungen erzeugt.

    Die **"Web.config" Vorschau** Fenster zeigt das Ergebnis des Anwendens sowohl die *Web.Release.config* transformiert und die *Web.Staging.config* transformiert.

### <a name="prevent-public-use-of-the-test-app"></a>Verhindern von öffentlichen Verwendung der Test-app

Ein wichtiger Aspekt für die staging-app ist, live im Internet werden, aber nicht öffentlich, ihn zu verwenden. Um die Wahrscheinlichkeit zu minimieren, dass Personen findet und verwendet werden, können Sie eine oder mehrere der folgenden Methoden verwenden:

- Legen Sie Firewallregeln festlegen, die Zugriff auf die staging-app von IP-Adressen nur möglich, mit denen Sie Staging zu testen.
- Verwenden Sie eine verborgene URL, die nicht möglich, zu erraten, wäre.
- Erstellen einer *"robots.txt"* Datei, um sicherzustellen, dass Suchmaschinen nicht in den Suchergebnissen die Test-app und der Bericht Links zu crawlen.

Die erste dieser Methoden ist am effektivsten, jedoch wird in diesem Lernprogramm nicht behandelt, da es erfordert, dass Sie ein Azure-Cloud-Dienst anstelle von Azure App Service bereitstellen. Weitere Informationen zu Cloud-Dienste und IP-Einschränkungen in Azure finden Sie unter [Ausführungsmodelle von Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) und [Block bestimmte IP-Adressen aus den Zugriff auf eine Webrolle](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Wenn Sie mit einem Drittanbieter-Hostinganbieter bereitstellen, wenden Sie sich an den Anbieter, um herauszufinden, wie IP-Einschränkungen implementieren.

In diesem Lernprogramm erstellen Sie eine *"robots.txt"* Datei.

1. In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und klicken Sie auf **neues Element hinzufügen**.
2. Erstellen Sie ein neues **Textdatei** mit dem Namen *"robots.txt"*, und legen Sie den folgenden Text in diese:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    Die `User-agent` Zeile weist Suchmaschinen, die für alle Search Engine-Webcrawler (Roboter), die Regeln in der Datei gelten und die `Disallow` Zeile gibt an, dass keine Seiten auf der Website gecrawlt werden soll.

    Sie möchten den Suchmaschinen auf Ihre Produktions-app zu katalogisieren, daher Sie diese Datei ausschließen der produktionsbereitstellung müssen. Dazu, dass Sie konfigurieren, müssen eine Einstellung in der Produktion Veröffentlichungsprofil bei ihrer Erstellung.

### <a name="deploy-to-staging"></a>In die Stagingumgebung bereitstellen

1. Öffnen der **Web veröffentlichen** Assistenten, indem Sie mit der rechten Maustaste des University Contoso-Projekts und auf **veröffentlichen**.
2. Stellen Sie sicher, dass die **Staging** Profil ausgewählt ist.
3. Klicken Sie auf **Veröffentlichen**.

    Die **Ausgabe** Fenster zeigt, welche Bereitstellungsaktionen ausgeführt wurden, und gibt erfolgreichen Abschluss der Bereitstellung. Die Standard-Browser wird automatisch an die URL der bereitgestellten Web-app geöffnet.

## <a name="test-in-the-staging-environment"></a>In der Stagingumgebung testen

Beachten Sie, dass der Indikator für die Umgebung nicht vorhanden ist (es ist kein "(Test)" oder "(Dev)" nach der H1-Überschrift, die zeigt, dass die *"Web.config"* Transformation für den Indikator für die Umgebung erfolgreich war.

![Startseite-staging](deploying-to-production/_static/image12.png)

Führen Sie die **Studenten** Seite, um zu überprüfen, ob die bereitgestellte Datenbank kein Studenten verfügt.

Führen Sie die **Lehrkräfte** Seite, um sicherzustellen, dass Code First mit Anfangsdaten die Datenbank mit Instructor-Daten gefüllt:

Wählen Sie **Studenten hinzufügen** aus der **Studenten** Menü eine Student hinzufügen, und zeigen Sie die neue Studenten in die **Studenten** Seite, um sicherzustellen, dass Sie erfolgreich in die Datenbank geschrieben werden können .

Aus der **Kurse** auf **Update Gutschriften**. Die **Update Gutschriften** Seite erfordert Administratorberechtigungen vorhanden sind, sodass der **anmelden** angezeigt wird. Geben Sie die Anmeldeinformationen des Administratorkontos, die Sie früher ("Admin" und "Prodpwd") erstellt. Die **Update Gutschriften** Seite wird angezeigt, die überprüft, ob das Administratorkonto, das Sie im vorherigen Lernprogramm erstellt ordnungsgemäß in der testumgebung bereitgestellt wurde.

Fordern Sie eine ungültige URL auf einen Fehler verursachen, dass ELMAH nachverfolgt wird, und fordern dann die ELMAH Fehlerbericht an. Wenn Sie mit einem Drittanbieter-Hostinganbieter bereitstellen, finden Sie wahrscheinlich, dass der Bericht aus demselben Grund, die sie im vorherigen Lernprogramm leer war leer ist. Sie müssen den Hostinganbieter Konto-Verwaltungstools zu verwenden, so konfigurieren Sie Berechtigungen für Ordner ELMAH zum Schreiben in die Log-Ordner zu aktivieren.

Die Anwendung, die Sie erstellt haben, wird jetzt in der Cloud in eine Web-app ausgeführt werden, die genau wie für die Produktion verwendet wird. Da alles ordnungsgemäß funktioniert, besteht der nächste Schritt in der produktionsumgebung bereitstellen.

## <a name="deploy-to-production"></a>Für die Produktion bereitstellen

Das Verfahren zum Erstellen einer Produktions-Web-app und Bereitstellung bis hin zur Produktion wird genauso wie bei Staging, außer dass Sie ausschließen möchten die *"robots.txt"* von Bereitstellung. Hierzu bearbeiten Sie das Veröffentlichungsprofil.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Erstellen Sie die produktionsumgebung und Veröffentlichungsprofil für die Produktion

1. Erstellen Sie die Produktions-Web-app und die Datenbank in Azure, das gleiche Verfahren, die Sie für das Staging verwendet.

    Wenn Sie die Datenbank erstellen, können Sie wählen Sie es auf dem gleichen Server ablegen, die Sie zuvor erstellt haben oder einen neuen Server erstellen.
2. Herunterladen der *publishsettings* Datei.
3. Erstellen Sie das Veröffentlichungsprofil durch Importieren der Produktions *publishsettings* Datei, die das gleiche Verfahren, die Sie für das Staging verwendet.

    Vergessen Sie nicht so konfigurieren Sie das Bereitstellungsskript Daten unter **DefaultConnection** in der **Datenbanken** Teil der **Einstellungen** Registerkarte.
4. Benennen Sie das Veröffentlichungsprofil auf *Produktion*.
5. Konfigurieren Sie eine Publish-Profil für die Umgebung Indikator, der das gleiche Verfahren, die Sie für das Staging verwendet...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Bearbeiten Sie die Datei "pubxml", um "robots.txt" ausschließen

Das Veröffentlichungsprofil Dateien heißen &lt;Profilename&gt;*.pubxml* und befinden sich in der *PublishProfiles* Ordner. Die *PublishProfiles* Ordner befindet sich unter der *Eigenschaften* Ordner in einer Web-Anwendung in C#-Projekt, unter der *Mein Projekt* Ordner in einem VB-Webprojekt Anwendung oder unter der *App\_Daten* Ordner in einem Web-app-Projekt. Jede *pubxml* -Datei enthält Einstellungen, die für eine gelten Veröffentlichungsprofil. Die Werte, die Sie im Web veröffentlichen-Assistenten geben Sie in diesen Dateien gespeichert sind, und können Sie diese zum Erstellen oder Ändern von Einstellungen, die in der Visual Studio-Benutzeroberfläche zur Verfügung gestellt werden nicht bearbeiten.

Standardmäßig *pubxml* Dateien im Projekt enthalten sind, beim Erstellen eines Veröffentlichungsprofils, jedoch Sie sie aus dem Projekt ausschließen können und Visual Studio werden Sie weiterhin verwenden. Visual Studio sucht der *PublishProfiles* Ordner für *pubxml* Dateien, unabhängig davon, ob sie im Projekt enthalten sind.

Für jede *pubxml* Datei besteht eine *. pubxml.user* Datei. Die *. pubxml.user* -Datei enthält das verschlüsselte Kennwort aus, wenn Sie ausgewählt haben die **Kennwort speichern** -Option wird standardmäßig, die sie aus dem Projekt ausgeschlossen wird.

Ein *pubxml* -Datei enthält die Einstellungen, die einen bestimmten Veröffentlichungsprofil betreffen. Wenn Sie Einstellungen konfigurieren, die für alle Profile anwenden möchten, können Sie erstellen eine *. wpp.targets* Datei. Während des Erstellungsprozesses importiert diese Dateien in den *csproj* oder *vbproj* Projektdatei, damit die meisten Einstellungen, die Sie in der Projektdatei konfigurieren können, die in diesen Dateien konfiguriert werden können. Weitere Informationen zu *pubxml* Dateien und *. wpp.targets* finden Sie unter [wie: Bearbeiten von Bereitstellungseinstellungen im Veröffentlichungsprofil (.pubxml)-Dateien und die. wpp.targets Datei in Visual Studio Webprojekte](https://msdn.microsoft.com/library/ff398069.aspx).

1. In **Projektmappen-Explorer**, erweitern Sie **Eigenschaften** und erweitern Sie **PublishProfiles**.
2. Mit der rechten Maustaste *Production.pubxml* , und klicken Sie auf **öffnen**.

    ![Öffnen Sie die Datei "pubxml"](deploying-to-production/_static/image13.png)
3. Mit der rechten Maustaste *Production.pubxml* , und klicken Sie auf **öffnen**.
4. Fügen Sie die folgenden Zeilen unmittelbar vor der schließenden `PropertyGroup` Element:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    Die Datei "pubxml" sieht nun wie im folgenden Beispiel:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Weitere Informationen zur Vorgehensweise beim Ausschließen von Dateien und Ordnern finden Sie unter [kann ich schließt bestimmte Dateien oder Ordner aus Bereitstellung?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) in der **Web-Bereitstellung – häufig gestellte Fragen für Visual Studio und ASP.NET** auf MSDN.

### <a name="deploy-to-production"></a>Für die Produktion bereitstellen

1. Öffnen der **Web veröffentlichen** Assistenten verwenden, stellen Sie sicher, dass die **Produktion** Veröffentlichungsprofil ausgewählt ist, und klicken Sie dann auf **starten Vorschau** auf die **Vorschau**Tab, um zu überprüfen, ob die *"robots.txt"* Datei wird zur Produktions-app nicht kopiert werden.

    ![Vorschau von Dateien bis hin zur Produktion veröffentlicht werden](deploying-to-production/_static/image14.png)

    Überprüfen Sie die Liste der Dateien, die kopiert werden. Sie sehen, dass alle der *cs* Dateien, einschließlich *. aspx.vb*, *. aspx.designer.cs*, *Master.cs*, und  *Master.Designer.cs* Dateien werden ausgelassen. Alle mit diesem Code kompiliert wurde in der *ContosoUniversity.dll* und *ContosUniversity.pdb* Dateien, die finden Sie in der *"bin"* Ordner. Da nur die *DLL* ist erforderlich, um ausführen, die die Anwendung, und Sie zuvor angegebenen, dass nur Dateien benötigt zum Ausführen der Anwendung bereitgestellt werden soll, keine *cs* Dateien wurden in das Ziel kopiert Umgebung. Die *Obj* Ordner und die *ContosoUniversity.csproj* und *. csproj.user* Dateien aus demselben Grund ausgelassen werden.

    Klicken Sie auf **veröffentlichen** in der produktionsumgebung bereitstellen.
2. Testen Sie in der Produktion, das gleiche Verfahren, die Sie für das Staging verwendet.

    Alles ist identisch mit Staging mit Ausnahme der URL und die Abwesenheit der *"robots.txt"* Datei.

## <a name="summary"></a>Zusammenfassung

Sie haben jetzt erfolgreich bereitgestellt und Ihre Web-app getestet, und er öffentlich verfügbar ist über das Internet.

![Auf der Startseite Produktion](deploying-to-production/_static/image15.png)

In den nächsten Lernprogrammen Sie Anwendungscode zu aktualisieren und die Änderung für den Test, Staging und Produktion Umgebungen bereitstellen.

> [!NOTE]
> Während die Anwendung in der produktionsumgebung eingesetzt wird sollten Sie einen Wiederherstellungsplan implementieren. D. h., Sie sollten werden in regelmäßigen Abständen Sichern der Datenbanken aus der Produktions-app an einem sicheren Speicherort, und Generationen solcher Sicherungen beibehalten werden soll. Wenn Sie die Datenbank aktualisieren, sollten Sie eine Sicherungskopie von unmittelbar vor der Änderung. Klicken Sie dann, wenn Sie ein Fehler unterläuft und nicht erst ermitteln, nachdem Sie es in der produktionsumgebung bereitgestellt haben, noch werden Sie können zum Wiederherstellen der Datenbank in den Zustand, der vorlag, bevor er beschädigt wurde. Weitere Informationen finden Sie unter [Azure SQL-Datenbank sichern und Wiederherstellen](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).


> [!NOTE]
> In diesem Lernprogramm der SQL Server ist Edition, das Sie bereitstellen, Azure SQL-Datenbank. Während des Bereitstellungsprozesses für andere Editionen von SQL Server vergleichbar ist, kann eine Anwendung für die Produktion speziellen Code für Azure SQL-Datenbank in einigen Szenarien erforderlich. Weitere Informationen finden Sie unter [arbeiten mit Azure SQL-Datenbank](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) und [auswählen zwischen SQL Server und Azure SQL-Datenbank](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).

>[!div class="step-by-step"]
[Zurück](setting-folder-permissions.md)
[Weiter](deploying-a-code-update.md)
