---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Entwickeln von ASP.NET-Apps mit Azure Active Directory | Microsoft Docs
author: Rick-Anderson
description: "Microsoft ASP.NET-Tools für Azure Active Directory vereinfacht die Authentifizierung für auf Azure gehostete Webanwendungen zu aktivieren. Sie können Azure authentifizieren..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2014
ms.topic: article
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 1ef0468d5f5c17480b23ac88983f30fe6f4979c0
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="developing-aspnet-apps-with-azure-active-directory"></a>Entwickeln von ASP.NET-Apps mit Azure Active Directory
====================
Durch [Rick Anderson](https://github.com/Rick-Anderson)

> Microsoft ASP.NET tools für Azure Active Directory-Authentifizierung für auf gehostete Webanwendungen aktivieren problemlos [Azure](https://www.windowsazure.com/home/features/web-sites/). Sie können Azure-Authentifizierung verwenden, Office 365-Benutzer aus Ihrer Organisation, Unternehmenskonten aus Ihrem lokalen Active Directory synchronisiert oder in Ihrer eigenen benutzerdefinierten Azure Active Directory-Domäne erstellten Benutzer zu authentifizieren. Aktivieren von Windows Azure-Authentifizierung konfiguriert Ihre Anwendung zum Authentifizieren von Benutzern, die mit einem einzigen [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) Mandanten.
> 
>  Dieses Lernprogramm wurde von Rick Anderson geschrieben.[@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)


In diesem Lernprogramm erfahren Sie, wie Sie eine ASP.NET-Anwendung erstellen, die für die Anmeldung bei konfiguriert ist [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure AD). Außerdem erfahren Sie, wie der Graph-API zum Abrufen von Informationen über den derzeit angemeldeten Benutzer aufrufen und die Anwendung in Azure bereitzustellen.

## <a name="prerequisites"></a>Erforderliche Komponenten

1. [Visual Studio Express 2013 für Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express) oder [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads).
2. [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) -Update 3 oder höher ist erforderlich.
3. Ein Azure-Konto. [Klicken Sie hier](https://azure.microsoft.com/pricing/free-trial/) für eine kostenlose Testversion, wenn Sie nicht bereits ein Konto verfügen.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Fügen Sie einen globalen Administrator zu Active Directory

1. Melden Sie sich bei der [Azure-Verwaltungsportal](https://manage.windowsazure.com/).
2. Alle Azure-Konten enthalten eine **Standardverzeichnis** – klicken Sie darauf, und klicken Sie auf die **Benutzer** Registerkarte am oberen Rand der Seite (siehe folgende Abbildung).
3. Klicken Sie hier fügen Benutzer hinzu.  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Erstellen eines neuen Benutzers mit dem **Globaladministrator** Rolle. Klicken Sie auf **Benutzer** aus der oberen Menüleiste, und klicken Sie dann auf die **Add User** auf der Befehlsleiste auf die Schaltfläche.
5. In der **Add User** Dialogfeld, geben Sie einen Namen für den neuen Benutzer, und klicken Sie dann auf den Pfeil nach rechts.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Geben Sie den Benutzernamen ein, und legen Sie die Rolle auf **Globaladministrator**. Globale Administratoren benötigen eine alternative e-Mail-Adresse zu Wiederherstellungszwecken Kennwort an. Wenn Sie fertig sind, klicken Sie auf den Pfeil nach rechts.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. Klicken Sie auf der nächsten Seite des Dialogfelds auf **erstellen**. Ein temporäres Kennwort für den neuen Benutzer erstellt und im Dialogfeld angezeigt.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)  
  
 Speichern Sie das Kennwort werden Sie zum Ändern des Kennworts nach der ersten Anmeldung in benötigt. Die folgende Abbildung zeigt das neue Administratorkonto ein. Sie müssen die Azure Active Directory verwenden, bei der app, und nicht die Microsoft-Konto, das auch auf dieser Seite angezeigten anmelden.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Erstellen einer ASP.NET-Anwendung

Die folgenden Schritte verwenden [Visual Studio Express 2013 für Web](https://www.microsoft.com/download/details.aspx?id=40747), und erfordert [Visual Studio 2013 Update 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. Klicken Sie in Visual Studio auf **Datei** und dann **neues Projekt**. Auf der **neues Projekt** Dialog, wählen Sie die Visual C#-Web im linken Menü Projekt, und klicken Sie auf **OK**. Sie sollten auch deaktivieren die **Application Insights zu Projekt hinzufügen** Wenn Sie nicht die Funktionalität für Ihre Anwendung möchten.
2. In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld **MVC**, und klicken Sie dann auf **Authentifizierung ändern**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Auf der **Authentifizierung ändern** wählen Sie im Dialogfeld **Organisationskonten**. Diese Optionen können automatisch registrieren Ihrer Anwendung in Azure AD als auch automatisch Konfigurieren der Anwendung zur Integration in Azure AD verwendet werden. Sie müssen nicht verwenden die **Authentifizierung ändern** Dialogfeld zum Registrieren und konfigurieren die Anwendung, sondern kann es viel einfacher. Wenn Sie z. B. Visual Studio 2012 verwenden, können Sie dennoch manuell registrieren Sie die Anwendung im Azure-Verwaltungsportal und aktualisieren Sie die Konfiguration für die Integration von Azure AD.  
 Wählen Sie in der Dropdown-Menüs **Cloud – einzelne Organisation** und **einmaliges Anmelden, Verzeichnisdaten lesen**. Geben Sie die Domäne für Ihr Azure AD-Verzeichnis, z. B. (in der folgenden Images) *aricka0yahoo.onmicrosoft.com*, und klicken Sie dann auf **OK**. Sie können den Domänennamen aus der Registerkarte "Domänen" abrufen, für das Standard-Verzeichnis im Azure-Portal (finden Sie in der nächsten Abbildung unten).   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)  
  
 Die folgende Abbildung zeigt den Domänennamen vom Azure-Portal.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)  

    > [!NOTE]
    > Optional können Sie die Anwendung-ID-URI, der durch Klicken auf in Azure AD registriert werden, konfigurieren **Weitere Optionen**. Die App-ID-URI ist der eindeutige Bezeichner für eine Anwendung, die in Azure AD registriert und von der Anwendung verwendet werden, um sich bei der Kommunikation mit Azure AD identifizieren. Weitere Informationen zu den App-ID-URI und andere Eigenschaften der registrierten Anwendungen, finden Sie unter [in diesem Thema](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Indem Sie das Kontrollkästchen unter dem Feld "App-ID-URI" aktivieren, können Sie auch auswählen, um eine vorhandene Registrierung in Azure AD zu überschreiben, die die gleiche App-ID-URI verwendet.
4. Nach dem Klicken auf **OK**, ein Anmeldedialogfeld wird angezeigt, und Sie müssen über ein globales Administratorkonto (nicht die Microsoft-Konto mit Ihrem Abonnement verknüpft ist) anmelden. Wenn Sie ein neues Administratorkonto ein zuvor erstellt haben, müssen Sie dazu das Kennwort ändern, und klicken Sie dann erneut mit dem neuen Kennwort anmelden.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Nachdem Sie erfolgreich authentifiziert haben, die **neues ASP.NET-Projekt** Dialogfeld zeigt Ihrer authentifizierungswahl (**organisationsweite** ) und das Verzeichnis, in dem die neue Anwendung wird, registriert (*aricka0yahoo.onmicrosoft.com* in der folgenden Abbildung). Unterhalb dieser Informationen das Kontrollkästchen mit der Bezeichnung **Host in der Cloud**. Wenn dieses Kontrollkästchen aktiviert ist, wird das Projekt wird als eine Azure-Web-app bereitgestellt werden und wird für einfache später veröffentlichen aktiviert werden. Klicken Sie auf **OK**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. Die **Azure-Website konfigurieren** Dialogfeld wird angezeigt, mit einem automatisch generierten Standortnamen und Region. Beachten Sie außerdem das Konto, das Sie gerade in das Dialogfeld "angemeldet sind. Sie möchten sicherstellen, dass dieses Konto ist, der Ihrem Azure-Abonnement zugeordnet ist, in der Regel ein Microsoft-Konto.

    > [!NOTE]
    > Dieses Projekt ist eine Datenbank erforderlich. Wählen Sie eine der vorhandenen Datenbanken aus, oder erstellen Sie eine neue werden müssen. Eine Datenbank ist erforderlich, da das Projekt bereits eine lokale Datenbankdatei verwendet, um eine kleine Menge von Authentifizierungsdaten für die Konfiguration zu speichern. Wenn Sie die Anwendung für eine Azure-Website bereitstellen, ist nicht mit der Bereitstellung dieser Datenbank verpackt, daher müssen Sie eine davon auszuwählen, die in der Cloud zugänglich ist. Klicken Sie auf **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Das Projekt wird erstellt, und die Authentifizierungsoptionen und Optionen für Web-app werden automatisch mit dem Projekt konfiguriert werden. Nachdem dieser Vorgang abgeschlossen wurde, führen Sie das Projekt lokal durch Drücken von **^ F5**. Sie müssen sich mit Ihrem Unternehmenskonto anmelden. Geben Sie den Benutzernamen und das Kennwort für das Konto, das Sie zuvor erstellt haben, und klicken Sie auf **anmelden**.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Nach der erfolgreichen Anmeldung wird die ASP.NET-Website angezeigt, dass Sie authentifiziert haben, indem der Benutzername in der oberen rechten Ecke der Seite angezeigt.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)  
  
 Wenn Sie eine Fehlermeldung erhalten, müssen Sie Folgendes tun:  
 Wert darf nicht null oder leer sein. Parametername: LinkText   
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)  
  
 finden Sie unter der [Debuggen](#dbg) Abschnitt am Ende des Lernprogramms.

## <a name="basics-of-the-graph-api"></a>Grundlagen der Graph-API

[Der Graph-API](https://msdn.microsoft.com/library/azure/hh974476.aspx) ist die programmgesteuerte Schnittstelle, die zum Ausführen von CRUD und andere Vorgänge für Objekte in Ihrem Azure AD-Verzeichnis verwendet. Wenn Sie eine Option Organisations-Konto für die Authentifizierung beim Erstellen eines neuen Projekts in Visual Studio 2013 auswählen, wird die Anwendung bereits konfiguriert sein, zum Aufrufen der Graph-API. Dieser Abschnitt wird kurz Funktionsweise der Graph-API.

1. Klicken Sie in der ausgeführten Anwendung auf, auf den Namen des angemeldeten Benutzers oben rechts auf der Seite. Dadurch gelangen Sie zu der Seite "Benutzerprofil", also eine Aktion auf dem Home-Controller. Sie werden bemerken, dass die Tabelle enthält die Benutzerinformationen zu dem Administratorkonto an, dass Sie zuvor erstellt haben. Diese Informationen werden in Ihrem Verzeichnis gespeichert, und der Graph-API zum Abrufen dieser Informationen beim Laden der Seite aufgerufen wird.   
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Wechseln Sie zurück zu Visual Studio, und erweitern Sie die **Controller** Ordner, und öffnen Sie dann die **HomeController.cs** Datei. Sehen Sie eine **UserProfile()** Aktion, die Code zum Abrufen eines Tokens, und rufen Sie dann auf die Graph-API enthält. Dieser Code wird unten dupliziert: 

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Um die Graph-API aufzurufen, müssen Sie zunächst ein Token abgerufen. Wenn das Token abgerufen wird, muss der Zeichenfolgenwert in der Authorization-Header für alle nachfolgenden Anforderungen an die Graph-API angefügt werden. Die meisten Code über behandelt die Details der Authentifizierung in Azure AD, um ein Token abzurufen, verwenden das Token, einen Aufruf der Graph-API und Transformieren die Antwort, damit sie in der Ansicht angezeigt werden kann.

    Erläuterung der wichtigsten Teil ist die folgende hervorgehobene Zeile: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Diese Zeile stellt den Namen des Benutzers, die aus der JSON-Antwort deserialisiert wurde, und wird in der Ansicht angezeigt.

    Rufen Sie die Graph-API, die mit "HttpClient" und die unformatierten Daten zu behandeln, jedoch eine einfachere Möglichkeit ist die Verwendung der [Graph-Clientbibliothek, die über NuGet verfügbar ist](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). Die Clientbibliothek behandelt die unformatierten HTTP-Anforderungen und die Transformation der zurückgegebenen Daten für Sie und kann es viel einfacher, mit der Graph-API in einer Umgebung mit .NET arbeiten. Verwandte Codebeispiele für die Graph-API finden Sie [GitHub.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Bereitstellen der Anwendung in Azure

Die folgenden Schritte aus erfahren Sie, wie Sie die Anwendung in Azure bereitzustellen. In den vorherigen Schritten verbunden Sie das neue Projekt mit einer Web-app in Azure, damit es in wenigen Schritten veröffentlicht werden kann.

1. Visual Studio mit der Maustaste auf das Projekt und wählen Sie **veröffentlichen**. Die **Web veröffentlichen** wird ein Dialogfeld angezeigt, bei dem jede Einstellung, die bereits konfiguriert. Klicken Sie auf die **Weiter** Schaltfläche fahren Sie mit der **Einstellungen** Seite. Möglicherweise werden Sie zur Authentifizierung aufgefordert; Stellen Sie sicher, dass Sie zur Authentifizierung verwenden, Ihre Azure-Abonnementkontos (in der Regel ein Microsoft-Konto) und nicht die Organisations-Konto, das Sie zuvor erstellt haben.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Überprüfen Sie die **Organisations-Authentifizierung aktivieren** Option. In der **Domäne** Feld, geben Sie die Domäne für Ihr Verzeichnis. Aus der **Zugriffsebene** Dropdownliste, wählen **einmaliges Anmelden, Verzeichnisdaten lesen**. Sie werden feststellen, dass die vorherige Datenbank, die Sie verwendet bereits aufgefüllt wird die **Datenbanken** Abschnitt. Klicken Sie auf **Veröffentlichen**.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio wird Ihrer Website bereitstellen, und klicken Sie dann ein neues Browserfenster angezeigt wird. Sie werden möglicherweise aufgefordert, auf Ihr Verzeichnis erneut zu authentifizieren. Nachdem Sie authentifiziert haben, müssen Sie auf die neu veröffentlichte Website in Azure umgeleitet werden.  
  
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debuggen der app

Wenn Sie die folgende Fehlermeldung angezeigt:   
 Wert darf nicht null oder leer sein. Parametername: LinkText   
   
![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)  
  

Ersetzen Sie den Code in der *Views\Shared\\_LoginPartial.cshtml* Datei durch Folgendes:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Nach dem Ausführen der app, wenn der angemeldete Benutzer "Null-Benutzer" angezeigt wird, melden Sie sich ab, und melden Sie sich wieder mit der Active Directory-Konto, das Sie zuvor erstellt haben.

Eine hervorragende Lernprogramm anzuwendendes ist Rick Rainey des [Deep Dive: Azure-Websites und organisatorische Authentifizierung mithilfe von Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Weitere Informationen

- [Deep Dive: Azure-Websites und Organisationseinheit mithilfe von Azure AD-Authentifizierung](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Übersicht über Azure AD Graph-API](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Authentifizierungsszenarien in Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Azure AD-Codebeispiele auf GitHub](https://github.com/AzureADSamples)
