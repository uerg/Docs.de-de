---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'ASP.NET-webbereitstellung mithilfe von Visual Studio: Bereitstellung in der Produktion | Microsoft-Dokumentation'
author: tdykstra
description: Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, indem Warnungsprovider...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: a91feedb9ac09b2a70ca256c72d312a1e78ecbc8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827876"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>ASP.NET-webbereitstellung mithilfe von Visual Studio: in einer Produktionsumgebung bereitstellen
====================
durch [Tom Dykstra](https://github.com/tdykstra)

[Startprojekt herunterladen](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Dieser tutorialreihe erfahren Sie, wie bereitzustellende (veröffentlichen) aus einer ASP.NET web-Anwendung auf Azure App Service-Web-Apps oder bei einem Hostinganbieter von Drittanbietern, mithilfe von Visual Studio 2012 oder Visual Studio 2010. Weitere Informationen über die Reihe finden Sie unter [im ersten Tutorial der Reihe](introduction.md).


## <a name="overview"></a>Übersicht

In diesem Tutorial haben Sie ein Microsoft Azure-Konto einrichten, Staging-und produktionsumgebungen zu erstellen und Bereitstellen Ihrer ASP.NET-Webanwendung in der die Stagingumgebung und produktionsumgebungen mit nur einem Klick über das Visual Studio veröffentlichen Features.

Falls gewünscht, können Sie bei einem Hostinganbieter von Drittanbietern bereitstellen. Die meisten der in diesem Tutorial beschriebenen Verfahren sind identisch für einen Hostinganbieter oder für Azure, mit dem Unterschied, dass jeder Anbieter eine eigene Benutzeroberfläche für die Konto-und-Website verfügt. Sie finden einen Hostinganbieter in der [Katalog von Anbietern](https://www.microsoft.com/web/hosting) auf der Microsoft.com-Website.

Erinnerung: Wenn Sie eine Fehlermeldung erhalten, oder etwas nicht funktioniert, wie Sie das Lernprogramm durchzuarbeiten, müssen Sie die Seite "Problembehandlung" in diesem Tutorial überprüfen.

## <a name="get-a-microsoft-azure-account"></a>Fordern Sie ein Microsoft Azure-Konto

Wenn Sie noch kein Azure-Konto besitzen, können Sie in wenigen Minuten ein kostenloses Testkonto erstellen. Weitere Informationen finden Sie unter [kostenlose Azure-Testversion](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Erstellen einer Stagingumgebung

> [!NOTE]
> Da in diesem Tutorial geschrieben wurde, wird eine neue Funktion um viele der Prozesse zum Erstellen von Staging-und produktionsumgebungen zu automatisieren von Azure App Service hinzugefügt. Finden Sie unter [Einrichten von Stagingumgebungen für Web-apps in Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).


Siehe die [bereitstellen, mit dem Tutorial für die Testumgebung,](deploying-to-iis.md), wird die zuverlässigen Test-Umgebung ist eine Website, auf dem Hostinganbieter, die der Produktionswebsite wie hat. Auf viele Hostinganbieter müssen die Vorteile von für erhebliche zusätzliche Kosten abwägen, aber Sie können eine zusätzliche kostenlose Web-app in Azure erstellen, als Ihre staging-app. Sie benötigen auch eine Datenbank, und die zusätzliche Kosten für, die über die Kosten für die Produktionsdatenbank wird entweder keine "oder" minimal. In Azure bezahlen Sie für die Menge an Datenbankspeicher, die Sie verwenden, anstatt für jede Datenbank, und die Menge an zusätzlichem Speicher, den Sie in der Stagingumgebung verwenden wird minimal sein.

Siehe die [bereitstellen, mit dem Test-Umgebung Tutorial](deploying-to-iis.md), Staging und Produktion, die Sie nun die beiden Datenbanken in einer Datenbank bereitstellen. Falls gewünscht, können sie getrennt zu halten, wird der Prozess identisch sein, außer dass Sie eine zusätzliche Datenbank für jede Umgebung erstellen, und Sie die richtige Ziel-Zeichenfolge für jede Datenbank Wählen bei der Erstellung des Veröffentlichungsprofils wird.

In diesem Abschnitt des Tutorials erstellen Sie eine Web-app und die Datenbank, die für die Stagingumgebung verwendet, und Sie in der Stagingumgebung bereitstellen und Testen Sie es vor dem Erstellen und in der produktionsumgebung bereitstellen.

> [!NOTE]
> Die folgenden Schritte zeigen, wie Sie eine Web-app in Azure App Service zu erstellen, indem Sie mithilfe des Azure-Verwaltungsportals. In der neuesten Version des Azure SDK können Sie auch dazu Visual Studio mithilfe des Server-Explorer heraus. In Visual Studio 2013 können Sie auch eine Web-app direkt aus dem Dialogfeld "Veröffentlichen" erstellen. Weitere Informationen finden Sie unter [ASP.NET Web-app in Azure App Service erstellen.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. In der [Azure-Verwaltungsportal](https://manage.windowsazure.com/), klicken Sie auf **Websites**, und klicken Sie dann auf **neu**.
2. Klicken Sie auf **Website**, und klicken Sie dann auf **Benutzerdefiniert erstellen**.

    Die **neue Website - Benutzerdefiniert erstellen** -Assistent wird geöffnet. Die **Benutzerdefiniert erstellen** -Assistenten können Sie eine Website und einer Datenbank zur gleichen Zeit zu erstellen.
3. In der **Website erstellen** Schritt des Assistenten geben Sie in einer Zeichenfolge in die **URL** Feld als die eindeutige URL für Ihre Anwendung verwendet die Stagingumgebung. Geben Sie beispielsweise ContosoUniversity-staging123 (einschließlich Zufallszahlen am Ende, damit es eindeutig ist, für den Fall, dass ContosoUniversity-Staging durchgeführt wird).

    Die vollständige URL besteht aus hier eingegebenen plus das Suffix, das neben dem Textfeld angezeigt.
4. In der **Region** Dropdown-Liste, wählen Sie die Region, die Ihnen am nächsten ist.

    Diese Einstellung gibt an, welches Datencenter Ihrer Web-app ausgeführt wird.
5. In der **Datenbank** Dropdown- Liste **Erstellen einer neuen SQL-Datenbank**.
6. In der **Name der Datenbankverbindungszeichenfolge** lassen den Standardwert *DefaultConnection*.
7. Klicken Sie auf den Pfeil nach rechts unten auf der das Feld.

    Die folgende Abbildung zeigt die **Website erstellen** Dialogfeld mit der Beispielwerte in es. Die URL und Region, die Sie eingegeben haben werden.

    ![Erstellen eines Website-Schritt](deploying-to-production/_static/image1.png)

    Der Assistent wechselt zur der **datenbankeinstellungen angeben** Schritt.
8. In der **Namen** geben *ContosoUniversity* plus eine zufällige Zahl ein, damit sie eindeutig, z. B. *ContosoUniversity123*.
9. In der **Server** Kontrollkästchen **neue SQL-Datenbankserver**.
10. Geben Sie einen Administratornamen und das Kennwort ein.

    Sie sind nicht keinen vorhandenen Namen und das Kennwort hier eingeben. Geben Sie einen neuen Namen und das Kennwort, das Sie hier zur späteren Verwendung, wenn Sie Zugriff auf die Datenbank definieren.
11. In der **Region** auf der gleichen Region, die Sie für die Web-app ausgewählt haben.

    Der Webserver und Datenbankserver in derselben Region bietet Ihnen die beste Leistung und Kosten minimiert.
12. Klicken Sie auf das Häkchen unten auf der das Feld, um anzugeben, dass Sie fertig sind.

    Die folgende Abbildung zeigt die **datenbankeinstellungen angeben** Dialogfeld mit der Beispielwerte in es. Die von den von die Ihnen eingegebenen Werte können abweichen.

    ![Schritt Datenbank der Website, neu zu erstellen, mit Datenbank-Assistenten](deploying-to-production/_static/image2.png)

    Das Verwaltungsportal auf der Seite "Websites" zurückgibt und die **Status** Spalte zeigt, dass die Web-app erstellt wird. Nach einer Weile (in der Regel weniger als einer Minute) die **Status** Spalte wird angezeigt, die Web-app erfolgreich erstellt wurde. In der Navigationsleiste auf der linken Seite, die Anzahl der Web-apps, die Sie in Ihrem Konto haben wird neben der **Websites** Symbol und die Anzahl der Datenbanken wird neben der **SQL-Datenbanken** Symbol.

    ![Websites-Seite des erstellten Website-Verwaltungsportals](deploying-to-production/_static/image3.png)

    Die Namen Ihrer Web-app wird von der Beispiel-app in der Abbildung abweichen.

## <a name="deploy-the-application-to-staging"></a>Die Anwendung in der Stagingumgebung bereitstellen

Nun, dass Sie eine WebApp und Datenbank für die Stagingumgebung erstellt haben, können Sie das Projekt darauf bereitstellen.

> [!NOTE]
> Diese Anweisungen zeigen, wie Sie ein Veröffentlichungsprofil herunterladen erstellen eine *.publishsettings* -Datei, die nicht nur für Azure, sondern auch für Drittanbieter-Hostinganbieter funktioniert. Die neueste Version können Azure SDK Sie auch direkt in Azure aus Visual Studio verbinden, und wählen Sie aus einer Liste von Web-apps, die Sie in Ihrem Azure-Konto verfügen. In Visual Studio 2013, Sie können sich bei Azure anzumelden aus der **Webveröffentlichung** Dialogfeld oder über die **Server-Explorer** Fenster. Weitere Informationen finden Sie unter [ASP.NET Web-app in Azure App Service erstellen](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).


### <a name="download-the-publishsettings-file"></a>Herunterladen der PUBLISHSETTINGS-Datei

1. Klicken Sie auf den Namen der Web-app, die Sie gerade erstellt haben.

    ![Klicken Sie auf der Website zum Dashboard wechseln](deploying-to-production/_static/image4.png)
2. Klicken Sie unter **Blick** in die **Dashboard** auf **Veröffentlichungsprofil herunterladen**.

    ![Veröffentlichungsprofil Downloadlink](deploying-to-production/_static/image5.png)

    Dieser Schritt wird eine Datei, die alle Einstellungen, die Sie benötigen enthält, um die Bereitstellung einer Anwendung auf Ihrer Web-app heruntergeladen. Importieren Sie diese Datei in Visual Studio müssen Sie nicht, diese Informationen manuell einzugeben.
3. Speichern Sie die *.publishsettings* Datei in einem Ordner, die Sie in Visual Studio zugreifen können.

    ![Speichern die PUBLISHSETTINGS-Datei](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Sicherheit – die *.publishsettings* -Datei enthält Ihre (unverschlüsselten) Anmeldeinformationen, die verwendet werden, um Ihre Azure-Abonnements und Dienste zu verwalten. Die bewährte Sicherheitsmethode für diese Datei wird zum vorübergehenden speichern außerhalb Ihrer quellcodeverzeichnisse (z. B. im Ordner "Libraries\Documents"), und löschen Sie sie nach Abschluss des Importvorgangs. Ein böswilliger Benutzer, erhält Zugriff auf, die *.publishsettings* Datei bearbeiten, erstellen und löschen Sie Ihre Azure-Dienste kann.

### <a name="create-a-publish-profile"></a>Erstellen eines Veröffentlichungsprofils

1. In Visual Studio mit der Maustaste des Projekts "ContosoUniversity" in **Projektmappen-Explorer** , und wählen Sie **veröffentlichen** aus dem Kontextmenü.

    Die **Webveröffentlichung** -Assistent wird geöffnet.
2. Klicken Sie auf die **Profil** Registerkarte.
3. Klicken Sie auf **Import**.
4. Navigieren Sie zu der *.publishsettings* Datei, die Sie zuvor heruntergeladen haben, und klicken Sie dann auf **öffnen**.

    ![Importieren Sie im Dialogfeld Veröffentlichungseinstellungen](deploying-to-production/_static/image7.png)
5. In der **Verbindung** auf **Verbindung überprüfen** um sicherzustellen, dass die Einstellungen richtig sind.

    Wenn die Verbindung validiert wurde, wird ein grünes Häkchen neben gezeigt die **Verbindung überprüfen** Schaltfläche.

    Für einige hosting-Anbieter, wenn Sie auf **Verbindung überprüfen**, möglicherweise eine **Zertifikatfehler** Dialogfeld. Wenn Sie dies tun, stellen Sie sicher, dass der Servername ist, was Sie erwarten. Wenn der Servername korrekt ist, wählen Sie **speichern Sie dieses Zertifikat für zukünftige Sitzungen von Visual Studio** , und klicken Sie auf **Accept**. (Dieser Fehler weist darauf hin, dass der Hostinganbieter ausgewählt hat, vermeiden Sie die Kosten für den Erwerb eines SSL-Zertifikats für die URL, die Sie bereitstellen. Wenn Sie eine sichere Verbindung mit einem gültigen Zertifikat einrichten möchten, wenden Sie sich an Ihren Hostinganbieter.)
6. Klicken Sie auf **Weiter**.

    ![Symbol für erfolgreichen Verbindung und die Schaltfläche "Weiter", Registerkarte "Verbindung"](deploying-to-production/_static/image8.png)
7. In der **Einstellungen** Registerkarte **Dateiveröffentlichungsoptionen**, und wählen Sie dann **Ausschließen von Dateien aus der App\_Datenordner**.

    Informationen zu den anderen Optionen unter **Dateiveröffentlichungsoptionen**, finden Sie unter den [Bereitstellen in IIS](deploying-to-iis.md) Tutorial. Der Screenshot, das angezeigt wird, die das Ergebnis dieser Schritt und die folgenden Konfigurationsschritte für die Datenbank am Ende der Schritte für die Datenbank ist.
8. Klicken Sie unter **DefaultConnection** in die **Datenbanken** Abschnitt, konfigurieren Sie die Bereitstellung der Datenbank für die Mitgliedschaftsdatenbank.
9. 1. Wählen Sie **Aktualisieren einer Datenbank**.

        Die **remoteverbindungszeichenfolge** Feld direkt unterhalb **DefaultConnection** , die mit der Verbindungszeichenfolge aus der .publishsettings-Datei gefüllt ist. Die Verbindungszeichenfolge enthält Anmeldeinformationen mit SQL Server, die im nur-Text gespeichert sind die *pubxml* Datei. Wenn Sie nicht diese es dauerhaft speichern möchten, können Sie entfernen sie aus dem Veröffentlichungsprofil ein, nach der Bereitstellung der Datenbank und stattdessen in Azure zu speichern. Weitere Informationen finden Sie unter [wie Sie Ihre ASP.NET-Datenbank-Verbindungszeichenfolgen schützen beim Bereitstellen in Azure aus der Quelle](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) Scott Hanselman Blog.
      2. Klicken Sie auf **Datenbankupdates konfigurieren**.
      3. In der **Datenbankupdates konfigurieren** Dialogfeld klicken Sie auf **SQL-Skript hinzufügen**.
      4. In der **SQL-Skript hinzufügen** navigieren Sie zu der *Aspnet-Daten-prod.sql* -Skript, das Sie zuvor im Projektmappenordner gespeichert, und klicken Sie dann auf **öffnen**.
      5. Schließen der **Datenbankupdates konfigurieren** Dialogfeld.
10. Klicken Sie unter **SchoolContext** in die **Datenbanken** wählen Sie im Abschnitt **Code First-Migrationen ausführen (wird beim Anwendungsstart ausgeführt)**.

    Visual Studio zeigt **Execute Code First Migrations** anstelle von **Update Database** für `DbContext` Klassen. Sollten Sie verwenden den DbDacFx-Anbieter statt Migrationen zum Bereitstellen einer Datenbank, die Sie zugreifen, indem Sie mit einem `DbContext` Klasse, finden Sie unter [wie Stelle ich eine Code First-Datenbank ohne Migrationen bereit?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) in der Web-Bereitstellung – häufig gestellte Fragen für Visual Studio und von ASP.NET auf MSDN.

    Die **Einstellungen** Registerkarte sieht nun wie im folgenden Beispiel:

    ![Registerkarte "Einstellungen" für das staging](deploying-to-production/_static/image9.png)
11. Führen Sie die folgenden Schritte aus, um das Profil zu speichern, und benennen Sie sie in *Staging*:

    1. Klicken Sie auf die **Profil** Registerkarte, und klicken Sie dann auf **Profile verwalten**.
    2. Der Import erstellt zwei neue Profile, die für FTP und eine für Web Deploy. Sie konfiguriert haben, das Web Deploy-Profil: Dieses Profil umbenennen *Staging*.

        ![Umbenennen Sie Profil, in der Stagingumgebung](deploying-to-production/_static/image10.png)
    3. Schließen der **Profile für Web veröffentlichen bearbeiten** Dialogfeld.
    4. Schließen der **Webveröffentlichung** Assistenten.

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Konfigurieren Sie eine Transformation des Veröffentlichen-Profil für den Indikator für die Umgebung

> [!NOTE]
> In diesem Abschnitt wird gezeigt, wie Sie eine Transformation der Datei "Web.config" für den Indikator für die Umgebung einrichten. Da der Indikator wird der `<appSettings>` -Element, Sie haben eine weitere Alternative für die Transformation angeben, wenn Sie in Azure App Service bereitstellen. Weitere Informationen finden Sie unter [Einstellungen für die Angabe von "Web.config" in Azure](web-config-transformations.md#watransforms).


1. In **Projektmappen-Explorer**, erweitern Sie **Eigenschaften**, und erweitern Sie dann **PublishProfiles**.
2. Mit der rechten Maustaste *Staging.pubxml*, und klicken Sie dann auf **Config Transformation hinzufügen**.

    ![Fügen Sie Konfigurationstransformation hinzu, für das staging](deploying-to-production/_static/image11.png)

    Visual Studio erstellt die *Web.Staging.config* Transform-Datei und öffnet sie.
3. In der *Web.Staging.config* Transformationsdatei, fügen Sie den folgenden Code unmittelbar nach dem öffnenden `configuration` Tag.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Wenn Sie das Staging-Veröffentlichungsprofil verwenden, wird diese Transformation den Indikator "Umgebung", "Prod". Wird ein Suffix, z. B. "(Dev)" oder "(Test)" nach dem "Contoso University" H1-Header nicht in der bereitgestellten Web-app angezeigt.
4. Mit der rechten Maustaste die *Web.Staging.config* Datei, und klicken Sie auf **Vorschau transformieren** um sicherzustellen, dass die Transformation, die Sie programmiert die erwarteten Änderungen erzeugt.

    Die **Vorschau der Datei "Web.config"** Fenster zeigt das Ergebnis der Anwendung sowohl die *Datei "Web.Release.config"* transformiert und die *Web.Staging.config* transformiert.

### <a name="prevent-public-use-of-the-test-app"></a>Verhindern Sie die öffentliche Verwendung der Test-app

Ein wichtiger Aspekt für die staging-app ist, dass sie auf das Internet aktiv werden, möchten jedoch nicht die öffentliche Verwendung. Um die Wahrscheinlichkeit zu minimieren, dass Benutzer findet und verwendet wird, können Sie eine oder mehrere der folgenden Methoden:

- Legen Sie Firewallregeln festlegen, die Zugriff auf die staging-app nur über IP-Adressen, die Sie verwenden zuzulassen, um die Stagingumgebung testen.
- Verwenden einer verborgenen URL, die nicht möglich, zu erraten ist.
- Erstellen Sie eine *robots.txt* Datei, um sicherzustellen, dass Suchmaschinen nicht in den Suchergebnissen die Test-app und Bericht-Links, crawlen.

Der erste dieser Methoden ist die am effektivsten, aber es wird in diesem Tutorial nicht behandelt, denn sie erfordert, dass Sie ein Azure-Cloud-Dienst anstelle von Azure App Service bereitstellen. Weitere Informationen zu Cloud Services und IP-Einschränkungen in Azure finden Sie [Ausführungsmodelle von Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) und [Block bestimmte IP-Adressen den Zugriff auf eine Webrolle](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Wenn Sie bei einem Hostinganbieter von Drittanbietern bereitstellen, wenden Sie sich an den Anbieter aus, um herauszufinden, wie Sie IP-Einschränkungen zu implementieren.

In diesem Tutorial erstellen Sie eine *robots.txt* Datei.

1. In **Projektmappen-Explorer**mit der rechten Maustaste auf das ContosoUniversity-Projekt, und klicken Sie auf **neues Element hinzufügen**.
2. Erstellen Sie ein neues **Textdatei** mit dem Namen *robots.txt*, und fügen Sie den folgenden Text in diese:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    Die `User-agent` Zeile wird die Suchmaschinen, die für alle Web Suchmaschinencrawlern (Roboter), die Regeln in der Datei gelten und die `Disallow` Zeile gibt an, dass keine Seiten auf der Website durchsucht werden soll.

    Sie möchten, Suchmaschinen in den Katalog Ihrer Produktions-app, daher Sie diese Datei aus der produktionsbereitstellung ausgeschlossen müssen werden. Zu diesem Zweck, Sie konfigurieren, werden eine Einstellung in der Produktion Veröffentlichungsprofil bei ihrer Erstellung.

### <a name="deploy-to-staging"></a>In der Stagingumgebung bereitstellen

1. Öffnen der **Webveröffentlichung** Assistenten, indem Sie mit der rechten Maustaste in der Contoso University-Projekt, und auf **veröffentlichen**.
2. Stellen Sie sicher, dass die **Staging** Profil ausgewählt ist.
3. Klicken Sie auf **Veröffentlichen**.

    Die **Ausgabe** Fenster zeigt, welche Bereitstellungsaktionen ausgeführt wurden, und meldet die erfolgreiche Durchführung der Bereitstellung. Der Standardbrowser wird automatisch an die URL der bereitgestellten Web-app geöffnet.

## <a name="test-in-the-staging-environment"></a>In der Stagingumgebung testen

Beachten Sie, dass die Umgebung nicht vorhanden ist (es ist kein "(Test)" oder "(Dev)" nach dem die H1-Überschrift, das anzeigt, die die *"Web.config"* Transformation für den Indikator für die Umgebung erfolgreich war.

![Auf der Startseite staging](deploying-to-production/_static/image12.png)

Führen Sie die **Schüler/Studenten** Seite, um sicherzustellen, dass die bereitgestellte Datenbank keine Schüler/Studenten verfügt.

Führen Sie die **Dozenten** Seite, um sicherzustellen, dass Code First ein die Datenbank mit "Instructor" Daten Seeding:

Wählen Sie **hinzufügen Schüler/Studenten** aus der **Schüler/Studenten** Menü ein Schüler/Student hinzufügen, und zeigen Sie den neuen Studenten in der **Schüler/Studenten** Seite, um sicherzustellen, dass Sie erfolgreich in die Datenbank schreiben können .

Von der **Kurse** auf **Update Gutschriften**. Die **Update Gutschriften** Seite sind Administratorberechtigungen erforderlich, sodass die **anmelden** angezeigt wird. Geben Sie Anmeldeinformationen für das Administratorkonto, dass Sie frühere ("Admin" und "Prodpwd") erstellt. Die **Update Gutschriften** Seite wird angezeigt, die überprüft, dass das Administratorkonto, das Sie im vorherigen Tutorial erstellt haben, ordnungsgemäß in der testumgebung bereitgestellt wurde.

Fordern Sie eine ungültige URL auf einen Fehler verursachen, ELMAH nachverfolgen, und fordern Sie dann auf den Fehlerbericht ELMAH. Wenn Sie bei einem Hostinganbieter von Drittanbietern bereitstellen, finden Sie wahrscheinlich, dass der Bericht aus demselben Grund, die sie im vorherigen Tutorial leer war leer ist. Sie müssen den Hostinganbieter Konto-Verwaltungstools zu verwenden, so konfigurieren Sie Berechtigungen für Ordner ELMAH zum Schreiben in den Ordner "Log" zu aktivieren.

Die Anwendung, die Sie erstellt haben, wird jetzt in einer Web-app in der Cloud ausgeführt werden, die genau wie für die Produktion verwenden werden. Da alles ordnungsgemäß funktioniert, besteht der nächste Schritt, in der produktionsumgebung bereitstellen.

## <a name="deploy-to-production"></a>Für die Produktion bereitstellen

Das Verfahren zum Erstellen einer Produktions-Web-Apps und in einer produktionsumgebung bereitstellen ist identisch mit dem Staging und die mit dem Unterschied, dass Sie ausschließen möchten die *robots.txt* von der Bereitstellung. Hierzu bearbeiten Sie das Veröffentlichungsprofil.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Erstellen die produktionsumgebung und die Produktions-Veröffentlichungsprofil

1. Erstellen Sie die Produktions-Web-app und Datenbank in Azure, die gleichen Schritte, die Sie für das Staging verwendet.

    Wenn Sie die Datenbank erstellen, können Sie es auf dem gleichen Server platzieren, die Sie zuvor erstellt haben, oder Erstellen eines neuen Servers.
2. Herunterladen der *.publishsettings* Datei.
3. Erstellen Sie das Veröffentlichungsprofil durch Importieren der Produktions *.publishsettings* -Datei, die gleichen Schritte, die Sie für das Staging verwendet.

    Vergessen Sie nicht so konfigurieren Sie das Skript des Daten-Bereitstellung unter **DefaultConnection** in die **Datenbanken** Teil der **Einstellungen** Registerkarte.
4. Benennen Sie das Veröffentlichungsprofil zu *Produktion*.
5. Konfigurieren Sie eine veröffentlichen-Profil für den Indikator des Umgebung, die gleichen Schritte, die Sie für das Staging verwendet...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Bearbeiten Sie die pubxml-Datei zum Ausschließen von "robots.txt"

Veröffentlichungsprofil Dateien heißen &lt;Profilename&gt;*pubxml* und befinden sich in der *PublishProfiles* Ordner. Die *PublishProfiles* Ordner befindet sich in der *Eigenschaften* Ordner in einer Web-Anwendung in C#-Projekt, unter der *Mein Projekt* Ordner in ein Webanwendungsprojekt in VB oder unter der *App\_Daten* Ordner in einem Web-app-Projekt. Jede *pubxml* Datei enthält Einstellungen, die für eine gelten Veröffentlichungsprofil. Die im Assistenten für Webveröffentlichung eingegebene Werte werden in diesen Dateien gespeichert und können bearbeitet werden zum Erstellen oder Ändern von Einstellungen, die in Visual Studio-Benutzeroberfläche verfügbar gemacht werden.

In der Standardeinstellung *pubxml* Dateien im Projekt enthalten sind, beim Erstellen eines Veröffentlichungsprofils, jedoch Sie sie aus dem Projekt ausschließen können und Visual Studio weiterhin werden verwendet wird. Visual Studio sucht der *PublishProfiles* Ordner für *pubxml* -Dateien, unabhängig davon, ob sie im Projekt enthalten sind.

Für jede *pubxml* Datei gibt es eine *. pubxml.user* Datei. Die *. pubxml.user* -Datei enthält das verschlüsselte Kennwort aus, wenn Sie ausgewählt haben die **Kennwort speichern** auswählen, wird standardmäßig aus dem Projekt ausgeschlossen.

Ein *pubxml* -Datei enthält die Einstellungen, die zu einem bestimmten Veröffentlichungsprofil beziehen. Wenn Sie möchten die Einstellungen zu konfigurieren, die für alle Profile gelten, können Sie erstellen eine *. wpp.targets* Datei. Der Buildprozess importiert diese Dateien in die *csproj* oder *vbproj* Projektdatei, damit die meisten Einstellungen, die Sie in der Projektdatei konfigurieren können, die in diesen Dateien konfiguriert werden können. Weitere Informationen zu *pubxml* Dateien und *. wpp.targets* finden Sie unter [Vorgehensweise: Bearbeiten der Bereitstellungseinstellungen in veröffentlichen Veröffentlichungsprofildateien (.pubxml)-Dateien und die. wpp.targets-Datei in Visual Studio Webprojekte](https://msdn.microsoft.com/library/ff398069.aspx).

1. In **Projektmappen-Explorer**, erweitern Sie **Eigenschaften** und erweitern Sie **PublishProfiles**.
2. Mit der rechten Maustaste *Production.pubxml* , und klicken Sie auf **öffnen**.

    ![Öffnen Sie die pubxml-Datei](deploying-to-production/_static/image13.png)
3. Mit der rechten Maustaste *Production.pubxml* , und klicken Sie auf **öffnen**.
4. Fügen Sie die folgenden Zeilen direkt vor dem Endtag `PropertyGroup` Element:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    Die pubxml-Datei sieht nun wie im folgenden Beispiel aus:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Weitere Informationen zur Vorgehensweise beim Ausschließen von Dateien und Ordnern finden Sie unter [kann ich ausschließen bestimmte Dateien oder Ordner von der Bereitstellung?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) in die **Web-Bereitstellung – häufig gestellte Fragen für Visual Studio und ASP.NET** auf MSDN.

### <a name="deploy-to-production"></a>Für die Produktion bereitstellen

1. Öffnen der **Webveröffentlichung** Assistenten verwenden, stellen Sie sicher, dass die **Produktion** Veröffentlichungsprofil ausgewählt ist, und klicken Sie dann auf **Vorschau starten** auf die **Vorschau**Tab, um zu überprüfen, ob die *robots.txt* Datei wird nicht in die Produktions-app kopiert werden.

    ![Vorschau der Dateien, die in der produktionsumgebung veröffentlicht werden](deploying-to-production/_static/image14.png)

    Überprüfen Sie die Liste der Dateien, die kopiert werden. Sie sehen, dass alle der *cs* -Dateien, einschließlich *. aspx.cs*, *. aspx.designer.cs*, *Master.cs*, und  *Master.Designer.cs* Dateien werden ausgelassen. All dieser Code kompiliert wurde in der *ContosoUniversity.dll* und *ContosUniversity.pdb* Dateien, die finden Sie in der *Bin* Ordner. Da nur die *DLL* ist erforderlich, um ausführen, die die Anwendung, und Sie zuvor angegeben, dass nur Dateien, die zum Ausführen der Anwendung bereitgestellt werden sollen, ohne *cs* Dateien auf dem Zielserver kopiert wurden Umgebung. Die *Obj* Ordner und die *"contosouniversity.csproj"* und *. csproj.user* Dateien aus demselben Grund ausgelassen werden.

    Klicken Sie auf **veröffentlichen** in der produktionsumgebung bereitstellen.
2. Testen Sie in der produktionsumgebung dieselben Schritten anwenden, die Sie für das Staging verwendet.

    Alles ist identisch mit Staging mit Ausnahme der URL und das Fehlen der *robots.txt* Datei.

## <a name="summary"></a>Zusammenfassung

Sie jetzt erfolgreich bereitgestellt und getestet haben Ihre Web-app, und es ist öffentlich über das Internet verfügbar.

![Auf der Startseite Produktion](deploying-to-production/_static/image15.png)

Im nächsten Tutorial Sie Anwendungscode aktualisieren und die Änderung in die Test-, Staging- und produktionsumgebungen-Umgebungen bereitstellen.

> [!NOTE]
> Während der Verwendung Ihrer Anwendung in der produktionsumgebung ist sollten Sie einen Wiederherstellungsplan implementieren. D.h., Sie sollten in regelmäßigen Abständen Sichern Ihrer Datenbanken von der Produktions-app an einem sicheren Speicherort, und mehrere Generationen von solchen Sicherungen beibehalten werden soll. Wenn Sie die Datenbank aktualisieren, sollten Sie eine Sicherungskopie von unmittelbar vor der Änderung. Klicken Sie dann, wenn Sie ein Fehler unterläuft und nicht erst erkennen, nachdem Sie es in der produktionsumgebung bereitgestellt haben, noch werden Sie die Datenbank in den Zustand wiederherstellen, die er sich befand, bevor er beschädigt. Weitere Informationen finden Sie unter [Azure SQL-Datenbank-Sicherung und-Wiederherstellung](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> In diesem Tutorial die SQL Server ist die Edition, die Sie bereitstellen, Azure SQL-Datenbank. Während des Bereitstellungsprozesses andere Editionen von SQL Server vergleichbar ist, kann Anwendung in einer realen produktionsumgebung speziellen Code für Azure SQL-Datenbank in einigen Szenarien benötigen. Weitere Informationen finden Sie unter [arbeiten mit Azure SQL-Datenbank](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) und [Entscheidung zwischen SQL Server und Azure SQL-Datenbank](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Zurück](setting-folder-permissions.md)
> [Weiter](deploying-a-code-update.md)
