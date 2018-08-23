---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Entwickeln von ASP.NET-Apps mit Azure Active Directory | Microsoft-Dokumentation
author: Rick-Anderson
description: Microsoft ASP.NET-Tools für Azure Active Directory vereinfacht die zum Aktivieren der Authentifizierung für Webanwendungen, die in Azure gehostet. Sie können Azure-ne...
ms.author: riande
ms.date: 08/14/2014
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: e2df906d220d738c45006de8b3c92e157ca9e57e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834019"
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Entwickeln von ASP.NET-Apps mit Azure Active Directory
====================
durch [Rick Anderson](https://github.com/Rick-Anderson)

Microsoft ASP.NET-tools für Azure Active Directory durch Aktivieren der Authentifizierung für Web-apps, die auf gehosteten vereinfacht [Azure](https://www.windowsazure.com/home/features/web-sites/). Sie können Azure-Authentifizierung verwenden, um Office 365-Benutzer aus Ihrer Organisation, Unternehmenskonten, die von Ihrem lokalen Active Directory synchronisiert oder in einer eigenen benutzerdefinierten Azure Active Directory-Domäne erstellten Benutzer zu authentifizieren. Aktivieren der Windows Azure-Authentifizierung konfiguriert die Anwendung zum Authentifizieren von Benutzern, die mit einem einzigen [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) Mandanten.

In diesem Tutorial erfahren Sie, wie Sie eine ASP.NET-Anwendung erstellen, die für den mit Azure AD und konfiguriert ist [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD). Außerdem lernen Sie, wie Sie die Graph-API zum Abrufen von Informationen über den derzeit angemeldeten Benutzer aufrufen und wie Sie die Anwendung in Azure bereitstellen.

## <a name="prerequisites"></a>Erforderliche Komponenten

1. [Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) oder [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -Update 3 oder höher ist erforderlich.
3. Ein Azure-Konto. [Klicken Sie hier,](https://azure.microsoft.com/pricing/free-trial/) für eine kostenlose Testversion, wenn Sie nicht bereits ein Konto verfügen.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Fügen Sie einen globalen Administrator in Ihrer Active Directory

1. Melden Sie sich bei der [Azure-Verwaltungsportal](https://manage.windowsazure.com/).
2. Alle Azure-Konten enthalten eine **Standardverzeichnis** – klicken Sie darauf, und klicken Sie dann die **Benutzer** Registerkarte am oberen Rand der Seite (siehe Abbildung unten).
3. Klicken Sie auf fügen Benutzer hinzu.  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Erstellen eines neuen Benutzers mit der **globaler Administrator** Rolle. Klicken Sie auf **Benutzer** aus der oberen Menüleiste, und klicken Sie dann auf die **Add User** auf der Befehlsleiste auf die Schaltfläche.
5. In der **Add User** Dialogfeld Geben Sie einen Namen für den neuen Benutzer, und klicken Sie dann auf den Pfeil nach rechts.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Geben Sie den Benutzernamen ein, und legen Sie die Rolle auf **globaler Administrator**. Globale Administratoren benötigen eine alternative e-Mail-Adresse für die Kennwort-Wiederherstellung. Wenn Sie fertig sind, klicken Sie auf den Pfeil nach rechts.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. Klicken Sie auf der nächsten Seite des Dialogfelds auf **erstellen**. Ein temporäres Kennwort für den neuen Benutzer erstellt und im Dialogfeld angezeigt.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
   Speichern Sie das Kennwort werden Sie zum Ändern des Kennworts nach der ersten Anmeldung erforderlich. Die folgende Abbildung zeigt das neue Administratorkonto an. Sie müssen Azure Active Directory verwenden, zum Anmelden an Ihrer app, die nicht den Microsoft-Konto, das ebenfalls auf dieser Seite angezeigt.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Erstellen einer ASP.NET-Anwendung

Die folgenden Schritte verwenden [Visual Studio Express 2013 für Web](https://www.microsoft.com/download/details.aspx?id=40747), und erfordert [Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. Klicken Sie in Visual Studio auf **Datei** und dann **neues Projekt**. Auf der **neues Projekt** Dialog, wählen Sie die Visual c#-Projekt im linken Menü, und klicken Sie auf **OK**. Sie sollten auch deaktivieren die **Application Insights zu Projekt hinzufügen** , wenn Sie nicht die Funktionalität für Ihre Anwendung benötigen.
2. In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld **MVC**, und klicken Sie dann auf **Authentifizierung ändern**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Auf der **Authentifizierung ändern** wählen Sie im Dialogfeld **Organisationskonten**. Diese Optionen können verwendet werden, registrieren Sie Ihre Anwendung automatisch in Azure AD als auch automatisch konfigurieren Ihrer Anwendung zur Integration in Azure AD. Sie müssen keine verwenden die **Authentifizierung ändern** Dialogfeld registrieren und Konfigurieren Ihrer Anwendung, die allerdings wesentlich vereinfacht. Wenn Sie z. B. Visual Studio 2012 verwenden, können Sie dennoch manuell registrieren der Anwendung im Azure-Verwaltungsportal und aktualisieren Sie die Konfiguration in Azure AD integriert.  
   Wählen Sie in der Dropdown-Menüs, **Cloud - einzelne Organisation** und **einmaliges Anmelden, Verzeichnisdaten lesen**. Geben Sie die Domäne für Azure AD-Verzeichnis, z. B. (in die Bilder unten) *aricka0yahoo.onmicrosoft.com*, und klicken Sie dann auf **OK**. Sie können den Domänennamen für das Standardverzeichnis für das Azure-Portal auf der Registerkarte "Domänen" abrufen (siehe die nächste Abbildung unten).   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
   Die folgende Abbildung zeigt den Domänennamen aus dem Azure-Portal.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > Optional können Sie konfigurieren, der URI der Anwendungs-ID, die durch Klicken auf in Azure AD registriert wird **Weitere Optionen**. Die App-ID-URI ist der eindeutige Bezeichner für eine Anwendung, die in Azure AD registriert und von der Anwendung verwendet werden, um sich bei der Kommunikation mit Azure AD identifizieren. Weitere Informationen zu den App-ID-URI und anderen Eigenschaften der registrierten Anwendungen finden Sie unter [in diesem Thema](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Durch Klicken auf das Kontrollkästchen unterhalb des Felds für die App-ID-URI, können Sie auch auswählen, um eine vorhandene Registrierung in Azure AD zu überschreiben, die den gleichen App-ID-URI verwendet.
4. Nach dem Klicken auf **OK**ein Anmeldedialogfeld wird angezeigt und Sie müssen für die Anmeldung mit einem globalen Administratorkonto (nicht die Microsoft-Konto mit Ihrem Abonnement verknüpften). Wenn Sie ein neues Administratorkonto ein zuvor erstellt haben, müssen Sie das Kennwort ändern, und melden Sie sich erneut mit dem neuen Kennwort.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Nachdem Sie erfolgreich authentifiziert haben, die **neues ASP.NET-Projekt** Dialogfeld zeigt Ihrer authentifizierungswahl (**organisationsweite** ) und das Verzeichnis, in dem die neue Anwendung wird, registriert (*aricka0yahoo.onmicrosoft.com* in der Abbildung unten). Unterhalb dieser Informationen ein, wählen Sie das Kontrollkästchen **in der Cloud hosten**. Wenn dieses Kontrollkästchen aktiviert ist, wird das Projekt als Azure-Web-app bereitgestellt werden und wird für die einfache Veröffentlichung später aktiviert werden. Klicken Sie auf **OK**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. Die **konfigurieren Azure-Website** Dialogfeld wird angezeigt, mit einem automatisch generierter Standortname und Region. Beachten Sie auch das Konto, das Sie im Dialogfeld für die derzeit in angemeldet sind. Sie möchten, um sicherzustellen, dass dieses Konto ist, der Ihrem Azure-Abonnement zugeordnet ist, in der Regel mit einem Microsoft-Konto.

    > [!NOTE]
    > Dieses Projekt erfordert eine Datenbank. Sie müssen eine der vorhandenen Datenbanken auswählen, oder erstellen Sie eine neue. Eine Datenbank ist erforderlich, da das Projekt bereits eine lokale Datenbankdatei verwendet, um eine kleine Menge von Authentifizierungsdaten für die Konfiguration zu speichern. Wenn Sie die Anwendung in einer Azure-Website bereitstellen, ist nicht dieser Datenbank mit der Bereitstellung verpackt, daher müssen Sie auswählen, die in der Cloud zugegriffen werden kann. Klicken Sie auf **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Das Projekt erstellt werden, und Ihre Authentifizierungsoptionen und die Optionen für Web-app werden automatisch mit dem Projekt konfiguriert werden. Sobald der Vorgang abgeschlossen ist, führen Sie das Projekt lokal durch Drücken von **^ F5**. Sie müssen sich mit Ihrem Unternehmenskonto anmelden. Geben Sie den Benutzernamen und das Kennwort für das Konto, das Sie zuvor erstellt haben, und klicken Sie auf **Anmeldung**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Nach der erfolgreichen Anmeldung wird die ASP.NET-Website angezeigt, dass Sie authentifiziert wurden, indem Sie den Benutzernamen in der oberen rechten Ecke der Seite anzeigen.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
   Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:  
   Wert darf nicht null oder leer sein. Parametername: LinkText   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
   finden Sie unter den [Debuggen](#dbg) Abschnitt am Ende des Tutorials.

## <a name="basics-of-the-graph-api"></a>Grundlagen von der Graph-API

[Der Graph-API](https://msdn.microsoft.com/library/azure/hh974476.aspx) ist die programmgesteuerte Schnittstelle, die zum Durchführen von CRUD und andere Vorgänge für Objekte in Azure AD-Verzeichnis verwendet. Wenn Sie beim Erstellen eines neuen Projekts in Visual Studio 2013 eine Organisations-Konto-Option für die Authentifizierung auswählen, wird die Anwendung bereits konfiguriert sein, zum Aufrufen der Graph-API. Dieser Abschnitt wird kurz die Funktionsweise der Graph-API.

1. Klicken Sie in der ausgeführten Anwendung auf, auf den Namen des angemeldeten Benutzers oben rechts auf der Seite. Dadurch gelangen Sie zu der Benutzerprofilseite, dies ist eine Aktion auf dem Home-Controller. Sie werden feststellen, dass die Tabelle enthält die Benutzerinformationen über das Administratorkonto an, dass Sie zuvor erstellt haben. Diese Informationen werden in Ihrem Verzeichnis gespeichert, und der Graph-API zum Abrufen dieser Informationen beim Laden der Seite aufgerufen wird.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Wechseln Sie zurück zu Visual Studio, und erweitern Sie die **Controller** Ordner und öffnen Sie dann die **"HomeController.cs"** Datei. Sehen Sie eine **UserProfile()** Aktion, die Code aus, um ein Token abzurufen, und rufen Sie dann auf die Graph-API enthält. Dieser Code wird unten doppelt vorhanden: 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Um die Graph-API aufzurufen, müssen Sie zuerst ein Token abgerufen. Wenn das Token abgerufen wird, muss der Zeichenfolgenwert in der Authorization-Header für alle nachfolgenden Anforderungen an die Graph-API angefügt werden. Die meisten der oben dargestellten Code behandelt die Details der Authentifizierung bei Azure AD, um ein Token abzurufen, verwenden das Token zum Aufrufen der Graph-API und Transformieren der Antwort, damit es in der Ansicht angezeigt werden kann.

    Erläuterung der wichtigsten Teil wird die folgende hervorgehobene Zeile: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Diese Zeile stellt den Namen des Benutzers, der aus der JSON-Antwort deserialisiert wurde, und wird in der Ansicht angezeigt.

    Rufen Sie die Graph-API, die mithilfe von "HttpClient" und verarbeitet die Rohdaten selbst, aber eine einfachere Möglichkeit ist die Verwendung der [Graph-Clientbibliothek, die über NuGet verfügbar sind](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). Die Clientbibliothek behandelt die unformatierten HTTP-Anforderungen und die Transformation der zurückgegebenen Daten für Sie, und macht es viel einfacher, mit der Graph-API in einer Umgebung mit .NET arbeiten. Sehen Sie sich die zugehörigen Graph-API-Codebeispiele auf [GitHub.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Bereitstellen der Anwendung in Azure

Die folgenden Schritte werden Sie zum Bereitstellen der Anwendung in Azure erläutert. In den obigen Schritten ist verbunden Sie Ihr neue Projekt mit einer Web-app in Azure, daher ist es in wenigen Schritten veröffentlicht werden.

1. Visual Studio mit der Maustaste auf das Projekt, und wählen **veröffentlichen**. Die **Webveröffentlichung** Dialogfeld wird angezeigt, bei dem jede Einstellung, die bereits konfiguriert. Klicken Sie auf die **Weiter** auf Schaltfläche, die **Einstellungen** Seite. Sie können zur Authentifizierung aufgefordert werden, Stellen Sie sicher, dass Sie zur Authentifizierung verwenden, Ihr Azure-Abonnement-Konto (in der Regel ein Microsoft-Konto) und nicht das Konto des Unternehmens, die, das Sie zuvor erstellt haben.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Überprüfen Sie die **Organisationsauthentifizierung aktivieren** Option. In der **Domäne** Geben Sie die Domäne für Ihr Verzeichnis. Von der **Zugriffsebene** Dropdown-Menü, Option **einmaliges Anmelden, Verzeichnisdaten lesen**. Sie werden feststellen, dass die frühere Datenbank, die Sie verwendet bereits, in aufgefüllt wird der **Datenbanken** Abschnitt. Klicken Sie auf **Veröffentlichen**.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio startet die Bereitstellung Ihrer Websites, und klicken Sie dann ein neues Browserfenster wird angezeigt. Sie möglicherweise aufgefordert werden, um wieder auf Ihr Verzeichnis zu authentifizieren. Nachdem Sie authentifiziert wurden, werden Sie auf die neu veröffentlichte Website in Azure umgeleitet.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debuggen der app

Wenn Sie den folgenden Fehler erhalten:   
 Wert darf nicht null oder leer sein. Parametername: LinkText   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

Ersetzen Sie den Code in die *Views\Shared\\_LoginPartial.cshtml* Datei durch Folgendes:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Nach dem Ausführen der app, wenn der angemeldete Benutzer "Null-Benutzer" angezeigt wird, melden Sie sich ab, und melden Sie sich wieder mit dem zuvor erstellten Active Directory-Konto.

Ein ausgezeichnetes Tutorial folgen ist die Rick Rainey [Deep Dive: Azure-Websites und mithilfe von Azure AD Organisationsauthentifizierung](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Weitere Informationen

- [Ausführliche Betrachtung: Azure Websites und in der Organisation mithilfe von Azure AD-Authentifizierung](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Übersicht über die Azure AD Graph-API](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Szenarien mit Authentifizierung in Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Azure AD-Codebeispiele auf GitHub](https://github.com/AzureADSamples)
