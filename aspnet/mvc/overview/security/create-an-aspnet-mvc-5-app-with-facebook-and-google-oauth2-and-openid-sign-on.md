---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Erstellen von MVC 5-App mit Facebook, Twitter, LinkedIn und Google "oauth2" Sign-on (c#) | Microsoft Docs
author: Rick-Anderson
description: In diesem Lernprogramm wird gezeigt, wie eine ASP.NET MVC 5-Webanwendung zu erstellen, die Benutzern ermöglicht, melden Sie sich mithilfe von OAuth 2.0 mit Anmeldeinformationen aus einer externen authentifizieren...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: c289c209b50f0c2c1f2d8b15a3aedeaebf671d0b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="59efb-103">Erstellen einer ASP.NET MVC 5-App mit Facebook, Twitter, LinkedIn und Google "oauth2" Sign-on (c#)</span><span class="sxs-lookup"><span data-stu-id="59efb-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="59efb-104">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="59efb-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="59efb-105">Dieses Lernprogramm veranschaulicht das Erstellen einer ASP.NET MVC 5-Webanwendung, die Benutzern ermöglicht, melden Sie sich mit [OAuth 2.0](http://oauth.net/2/) mit Anmeldeinformationen eines externen Authentifizierungsanbieters, z. B. Facebook, Twitter, LinkedIn, Microsoft oder Google.</span><span class="sxs-lookup"><span data-stu-id="59efb-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="59efb-106">Der Einfachheit halber konzentriert sich dieses Lernprogramms zum Arbeiten mit den Anmeldeinformationen von Facebook und Google.</span><span class="sxs-lookup"><span data-stu-id="59efb-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="59efb-107">Aktivieren diese Anmeldeinformationen auf Ihre Websites bietet einen erheblichen Vorteil Schreibberechtigung Millionen Benutzer bereits Konten mit diesen externen Anbietern.</span><span class="sxs-lookup"><span data-stu-id="59efb-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="59efb-108">Diese Benutzer möglicherweise eher für Ihre Website registrieren können, wenn sie nicht zum Erstellen und speichern einen neuen Satz von Anmeldeinformationen verfügen.</span><span class="sxs-lookup"><span data-stu-id="59efb-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="59efb-109">Siehe auch [ASP.NET MVC 5-Anwendung mit SMS und e-Mail-Zweifaktorenauthentifizierung](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="59efb-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="59efb-110">Das Lernprogramm zeigt auch zum Hinzufügen von Profildaten für den Benutzer sowie zum Membership-API zu verwenden, um Rollen hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="59efb-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="59efb-111">Dieses Lernprogramm wurde von geschrieben [Rick Anderson](https://blogs.msdn.com/rickAndy) (folgen Sie mir bitte auf Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="59efb-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="59efb-112">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="59efb-112">Getting Started</span></span>

<span data-ttu-id="59efb-113">Starten, indem Sie installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="59efb-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="59efb-114">Installieren von Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) oder höher.</span><span class="sxs-lookup"><span data-stu-id="59efb-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="59efb-115">Hilfe bei Dropbox, GitHub, Linkedin, Instagram, Puffer, Salesforce, DATENSTROM, Stapel Exchange, Tripit, Twitch, Twitter, Yahoo! und vieles mehr, finden Sie in diesem [Beispielprojekt](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="59efb-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="59efb-116">Sie müssen Visual Studio installieren [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) oder höher, Google OAuth 2 verwenden und lokal ohne SSL-Warnungen zu debuggen.</span><span class="sxs-lookup"><span data-stu-id="59efb-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="59efb-117">Klicken Sie auf **neues Projekt** aus der **starten** Seite, oder Sie können, verwenden Sie das Menü, und wählen **Datei**, und klicken Sie dann **neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="59efb-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="59efb-118">Erstellen einer ersten Anwendung</span><span class="sxs-lookup"><span data-stu-id="59efb-118">Creating Your First Application</span></span>

<span data-ttu-id="59efb-119">Klicken Sie auf **neues Projekt**, und wählen Sie dann **Visual C#-** auf der linken Seite, klicken Sie dann **Web** und wählen Sie dann **ASP.NET-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="59efb-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="59efb-120">Namen für das Projekt "MvcAuth", und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="59efb-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="59efb-121">In der **neues ASP.NET-Projekt** Dialogfeld klicken Sie auf **MVC**.</span><span class="sxs-lookup"><span data-stu-id="59efb-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="59efb-122">Wenn die Authentifizierung nicht **einzelne Benutzerkonten**, klicken Sie auf die **Authentifizierung ändern** Schaltfläche und wählen Sie **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="59efb-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="59efb-123">Durch Überprüfen **Host in der Cloud**, wird die app sehr einfach, die in Azure gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="59efb-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="59efb-124">Wenn Sie ausgewählt haben **Host in der Cloud**, führen Sie das Dialogfeld "konfigurieren".</span><span class="sxs-lookup"><span data-stu-id="59efb-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="59efb-125">Mithilfe von NuGet auf die neueste owin-Middleware aktualisieren</span><span class="sxs-lookup"><span data-stu-id="59efb-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="59efb-126">Verwenden Sie den NuGet-Paket-Manager beim Aktualisieren der [OWIN-Middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="59efb-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="59efb-127">Wählen Sie **Updates** im linken Menü.</span><span class="sxs-lookup"><span data-stu-id="59efb-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="59efb-128">Klicken Sie auf die **alle aktualisieren** Schaltfläche oder Sie können nur owin-Pakete (in der nächsten Abbildung dargestellt) suchen:</span><span class="sxs-lookup"><span data-stu-id="59efb-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="59efb-129">In der folgenden Abbildung werden nur die OWIN-Pakete angezeigt:</span><span class="sxs-lookup"><span data-stu-id="59efb-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="59efb-130">Aus Paket-Manager-Konsole (PMC), geben Sie den `Update-Package` Befehl, der alle Pakete aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="59efb-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="59efb-131">Drücken Sie **F5** oder **STRG + F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="59efb-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="59efb-132">In der folgenden Abbildung ist die Portnummer 1234.</span><span class="sxs-lookup"><span data-stu-id="59efb-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="59efb-133">Wenn Sie die Anwendung ausführen, sehen Sie eine andere Portnummer an.</span><span class="sxs-lookup"><span data-stu-id="59efb-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="59efb-134">Je nach Größe Ihres Browserfensters, müssen Sie möglicherweise das Symbol "Navigation" anzeigen klicken Sie auf die **Startseite**, **zu**, **Kontakt**, **registrieren**und **melden Sie sich** Links.</span><span class="sxs-lookup"><span data-stu-id="59efb-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="59efb-135">Einrichten von SSL in das Projekt</span><span class="sxs-lookup"><span data-stu-id="59efb-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="59efb-136">Zum Authentifizierungsanbieter z. B. Google und Facebook herstellen, müssen Sie die Installation von IIS Express einrichten, um die Verwendung von SSL.</span><span class="sxs-lookup"><span data-stu-id="59efb-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="59efb-137">Es ist wichtig, die mithilfe von SSL nach der Anmeldung nicht und abzulegen und wieder auf HTTP, Ihre Anmeldecookie wird nur als geheimer Schlüssel Ihres Benutzernamens und Kennworts und ohne Verwendung von SSL, die Sie in Klartext über das Netzwerk gesendet.</span><span class="sxs-lookup"><span data-stu-id="59efb-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="59efb-138">Darüber hinaus haben Sie bereits die Zeitdauer zum Ausführen des Handshakes und Sichern Sie die (das ist der Großteil der macht langsamer als HTTP HTTPS) ausgeführt, bevor die MVC-Pipeline ausgeführt wird, also wieder in den HTTP-umleiten, nachdem Sie angemeldet sind Stellen wird nicht die aktuelle Anforderung oder zukünftige Anforderungen, die wesentlich schneller.</span><span class="sxs-lookup"><span data-stu-id="59efb-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="59efb-139">In **Projektmappen-Explorer**, klicken Sie auf die **MvcAuth** Projekt.</span><span class="sxs-lookup"><span data-stu-id="59efb-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="59efb-140">Drücken Sie die F4-Taste, um die Projekteigenschaften anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="59efb-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="59efb-141">Alternativ können Sie aus der **Ansicht** Menü Sie auswählen können, **Fenster "Eigenschaften"**.</span><span class="sxs-lookup"><span data-stu-id="59efb-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="59efb-142">Änderung **SSL aktiviert** auf "true".</span><span class="sxs-lookup"><span data-stu-id="59efb-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="59efb-143">Kopieren Sie die SSL-URL (der ist `https://localhost:44300/` , wenn Sie andere SSL-Projekte erstellt haben).</span><span class="sxs-lookup"><span data-stu-id="59efb-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="59efb-144">In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die **MvcAuth** Projekt, und wählen Sie **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="59efb-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="59efb-145">Wählen Sie die **Web** Registerkarte, und fügen Sie die SSL-URL in die **Projekt-Url** Feld.</span><span class="sxs-lookup"><span data-stu-id="59efb-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="59efb-146">Speichern Sie die Datei (STRG + S).</span><span class="sxs-lookup"><span data-stu-id="59efb-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="59efb-147">Sie benötigen diese URL, Facebook und Google-Authentifizierung-apps zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="59efb-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="59efb-148">Hinzufügen der [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) -Attribut auf die `Home` Controller aus, um alle Anforderungen benötigen muss HTTPS verwenden.</span><span class="sxs-lookup"><span data-stu-id="59efb-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="59efb-149">Ein sicherer Ansatz ist, fügen die [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) Filter zur Anwendung.</span><span class="sxs-lookup"><span data-stu-id="59efb-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="59efb-150">Finden Sie im Abschnitt &quot;schützen Sie die Anwendung mit SSL und dem Attribut autorisieren&quot; in meinem tutoral [eine ASP.NET MVC-app mit Authentifizierung und SQL-Datenbank erstellen und Bereitstellen von Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="59efb-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="59efb-151">Ein Teil der Home-Controller ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="59efb-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="59efb-152">Drücken Sie STRG+F5, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="59efb-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="59efb-153">Wenn Sie das Zertifikat in der Vergangenheit installiert haben, überspringen Sie die restlichen Teil dieses Abschnitts und springen zu [beim Erstellen einer Google-app für OAuth-2, und verbinden die app auf das Projekt](#goog), führen Sie andernfalls die Anweisungen, um das selbstsignierte vertrauen Zertifikat, das IIS Express generiert hat.</span><span class="sxs-lookup"><span data-stu-id="59efb-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="59efb-154">Lesen der **Sicherheitswarnung** Dialogfeld und klicken Sie dann auf **Ja** gegebenenfalls zum Installieren des Zertifikats, das "localhost" darstellt.</span><span class="sxs-lookup"><span data-stu-id="59efb-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="59efb-155">Zeigt, d. h. die *Home* Seite, und es werden keine SSL-Warnungen.</span><span class="sxs-lookup"><span data-stu-id="59efb-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="59efb-156">Google Chrome auch akzeptiert das Zertifikat und HTTPS-Inhalt ohne eine Warnung wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="59efb-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="59efb-157">Firefox verwendet einen eigenen Zertifikatspeicher an, damit sie eine Warnung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="59efb-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="59efb-158">Sie können problemlos klicken, für die Anwendung **ich verstehe, dass die Risiken**.</span><span class="sxs-lookup"><span data-stu-id="59efb-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="59efb-159">Erstellen einer Google-app für OAuth 2, und verbinden die app zum Projekt</span><span class="sxs-lookup"><span data-stu-id="59efb-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="59efb-160">Aktuelle Google OAuth-Anweisungen finden Sie unter [Konfigurieren von Google-Authentifizierung in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="59efb-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="59efb-161">Navigieren Sie zu der [Google-Entwicklerkonsole](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="59efb-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="59efb-162">Wenn Sie ein Projekt, bevor Sie erstellt haben, wählen Sie **Anmeldeinformationen** die Registerkarte "Links", und wählen Sie dann **erstellen**.</span><span class="sxs-lookup"><span data-stu-id="59efb-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="59efb-163">Klicken Sie in der linken Registerkarte auf **Anmeldeinformationen**.</span><span class="sxs-lookup"><span data-stu-id="59efb-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="59efb-164">Klicken Sie auf **Anmeldeinformationen erstellen** dann **OAuth-Client-ID**.</span><span class="sxs-lookup"><span data-stu-id="59efb-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="59efb-165">In der **Client-ID erstellen** Dialogfeld, behalten Sie den Standardwert **-Webanwendung** für den Anwendungstyp.</span><span class="sxs-lookup"><span data-stu-id="59efb-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="59efb-166">Festlegen der **autorisiert JavaScript** Ursprünge auf dem oben verwendeten SSL-URL (`https://localhost:44300/` , wenn Sie andere SSL-Projekte erstellt haben)</span><span class="sxs-lookup"><span data-stu-id="59efb-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="59efb-167">Legen Sie die **autorisierte umleitungs-URI** an:</span><span class="sxs-lookup"><span data-stu-id="59efb-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="59efb-168">Klicken Sie auf das Menüelement des OAuth-Zustimmung-Bildschirm, und legen Sie die e-Mail-Adresse und Product Name.</span><span class="sxs-lookup"><span data-stu-id="59efb-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="59efb-169">Wenn Sie das Formular auf abgeschlossen haben **speichern**.</span><span class="sxs-lookup"><span data-stu-id="59efb-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="59efb-170">Klicken Sie auf das Menüelement für die Bibliothek, die nach **Google + API**, klicken Sie darauf, und drücken Sie dann aktivieren.</span><span class="sxs-lookup"><span data-stu-id="59efb-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="59efb-171">Die folgende Abbildung zeigt die aktivierte APIs.</span><span class="sxs-lookup"><span data-stu-id="59efb-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="59efb-172">In der Google APIs API-Manager finden Sie auf der **Anmeldeinformationen** Registerkarte zum Abrufen der **Clientkennung**.</span><span class="sxs-lookup"><span data-stu-id="59efb-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="59efb-173">Herunterladen eine JSON-Datei mit geheimen Schlüsseln die Anwendung zu speichern.</span><span class="sxs-lookup"><span data-stu-id="59efb-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="59efb-174">Kopieren und Einfügen der **ClientId** und **ClientSecret** in der `UseGoogleAuthentication` Methode gefunden, der *Startup.Auth.cs* in der Datei die *App_Start* Ordner.</span><span class="sxs-lookup"><span data-stu-id="59efb-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="59efb-175">Die **ClientId** und **ClientSecret** unten aufgeführten Werte sind Beispiele und funktionieren nicht.</span><span class="sxs-lookup"><span data-stu-id="59efb-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="59efb-176">Sicherheit – sensible Daten nie im Quellcode speichern.</span><span class="sxs-lookup"><span data-stu-id="59efb-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="59efb-177">Das Konto und die Anmeldeinformationen werden der Code oben, um das Beispiel einfach zu halten hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="59efb-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="59efb-178">Finden Sie unter [bewährte Methoden für die Bereitstellung von Kennwörtern und andere sensible Daten für ASP.NET und Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="59efb-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="59efb-179">Drücken Sie **STRG + F5** erstellen und Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="59efb-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="59efb-180">Klicken Sie auf die **melden Sie sich** Link.</span><span class="sxs-lookup"><span data-stu-id="59efb-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="59efb-181">Klicken Sie unter **einen anderen Dienst zum Anmelden verwenden**, klicken Sie auf **Google**.</span><span class="sxs-lookup"><span data-stu-id="59efb-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="59efb-182">Wenn Sie eine der oben aufgeführten Schritte verpasst haben, erhalten Sie einen HTTP 401-Fehler.</span><span class="sxs-lookup"><span data-stu-id="59efb-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="59efb-183">Überprüfen Sie die obigen Schritte erneut.</span><span class="sxs-lookup"><span data-stu-id="59efb-183">Recheck your steps above.</span></span> <span data-ttu-id="59efb-184">Wenn Sie eine erforderliche Einstellung verpasst haben (z. B. **Produktname**), fügen Sie das fehlende Element und speichern; dauert einige Minuten, bis die Authentifizierung funktioniert.</span><span class="sxs-lookup"><span data-stu-id="59efb-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="59efb-185">Sie werden auf der Google-Website umgeleitet werden, in dem Sie Ihre Anmeldeinformationen eingeben.</span><span class="sxs-lookup"><span data-stu-id="59efb-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="59efb-186">Nachdem Sie Ihre Anmeldeinformationen eingegeben haben, werden Sie aufgefordert, Berechtigungen für die Webanwendung zu geben, die Sie soeben erstellt haben:</span><span class="sxs-lookup"><span data-stu-id="59efb-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="59efb-187">Klicken Sie auf **akzeptieren**.</span><span class="sxs-lookup"><span data-stu-id="59efb-187">Click **Accept**.</span></span> <span data-ttu-id="59efb-188">Nun werden Sie umgeleitet an die **registrieren** Seite der Anwendung MvcAuth, wo Sie Ihre Google-Konto registrieren.</span><span class="sxs-lookup"><span data-stu-id="59efb-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="59efb-189">Sie haben die Möglichkeit, das Ändern des lokalen e-Mail-Registrierung für Ihr Konto Gmail verwendet, aber im Allgemeinen das standardmäßige e-Mail-Alias (d. h., die Sie für die Authentifizierung verwendet eine) bleiben sollen.</span><span class="sxs-lookup"><span data-stu-id="59efb-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="59efb-190">Klicken Sie auf **Registrieren**.</span><span class="sxs-lookup"><span data-stu-id="59efb-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="59efb-191">Erstellen die app in Facebook, und verbinden die app zum Projekt</span><span class="sxs-lookup"><span data-stu-id="59efb-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="59efb-192">Aktuelle Facebook OAuth2-Authentifizierung-Anweisungen finden Sie unter [Konfigurieren von Facebook-Authentifizierung](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="59efb-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<span data-ttu-id="59efb-193">Für die Facebook OAuth2-Authentifizierung müssen Sie dem Projekt einige Einstellungen aus einer Anwendung kopieren, die Sie in Facebook zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="59efb-193">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="59efb-194">In Ihrem Browser, navigieren Sie zu [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) und melden Sie sich, indem Sie Ihre Facebook-Anmeldeinformationen eingeben.</span><span class="sxs-lookup"><span data-stu-id="59efb-194">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="59efb-195">Wenn Sie bereits als ein Facebook-Entwickler registriert werden nicht, klicken Sie auf **registrieren Sie als Entwickler** und befolgen Sie die Anweisungen zum Registrieren.</span><span class="sxs-lookup"><span data-stu-id="59efb-195">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="59efb-196">Auf der **Apps** auf **neue App**.</span><span class="sxs-lookup"><span data-stu-id="59efb-196">On the **Apps** tab, click **Create New App**.</span></span>

    ![Neue app erstellen](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="59efb-198">Geben Sie eine **Anwendungsnamen** und **Kategorie**, klicken Sie dann auf **erstellen App**.</span><span class="sxs-lookup"><span data-stu-id="59efb-198">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="59efb-199">Dies muss innerhalb der Facebook eindeutig sein.</span><span class="sxs-lookup"><span data-stu-id="59efb-199">This must be unique across Facebook.</span></span> <span data-ttu-id="59efb-200">Die <strong>App Namespace</strong> ist der Teil der URL, die zum Zugriff auf die Facebook-Anwendung für die Authentifizierung Ihrer App verwenden (z. B. https://apps.facebook.com/{App Namespace}).</span><span class="sxs-lookup"><span data-stu-id="59efb-200">The <strong>App Namespace</strong> is the part of the URL that your App will use to access the Facebook application for authentication (for example, https://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="59efb-201">Wenn Sie nicht angeben einer <strong>App Namespace</strong>, die <strong>App-ID</strong> für die URL verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="59efb-201">If you don't specify an <strong>App Namespace</strong>, the <strong>App ID</strong> will be used for the URL.</span></span> <span data-ttu-id="59efb-202">Die <strong>App-ID</strong> ist eine lange vom System generierte Zahl, die Sie im nächsten Schritt sehen.</span><span class="sxs-lookup"><span data-stu-id="59efb-202">The <strong>App ID</strong> is a long system-generated number that you will see in the next step.</span></span>

    ![Neuen App-Dialogfeld "erstellen"](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="59efb-204">Senden Sie die Standardsicherheit Kontrollkästchen.</span><span class="sxs-lookup"><span data-stu-id="59efb-204">Submit the standard security check.</span></span>

    ![Sicherheitsüberprüfung](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="59efb-206">Wählen Sie **Einstellungen** für den linken Menü überwachungsabfragebalkens![ Facebook-Entwickler-Menüleiste](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="59efb-206">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="59efb-207">Auf der **grundlegende** Abschnitt der Seite Einstellungen wählen **Plattform hinzufügen** um anzugeben, dass Sie eine Websiteanwendung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="59efb-207">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="59efb-208">![Grundlegende Einstellungen](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="59efb-208">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="59efb-209">Wählen Sie **Website** aus den Plattformoptionen.</span><span class="sxs-lookup"><span data-stu-id="59efb-209">Select **Website** from the platform choices.</span></span>  
  
    ![Plattform-Optionen](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="59efb-211">Notieren Sie sich Ihre **App-ID** und Ihre **App-Geheimnis** , damit Sie später in diesem Lernprogramm beide Zertifikate in der MVC-Anwendung hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="59efb-211">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="59efb-212">Darüber hinaus fügen Sie der Website-URL (`https://localhost:44300/`) zum Testen Ihrer MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="59efb-212">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="59efb-213">Fügen Sie außerdem eine **Contact Email**.</span><span class="sxs-lookup"><span data-stu-id="59efb-213">Also, add a **Contact Email**.</span></span> <span data-ttu-id="59efb-214">Aktivieren Sie das Kontrollkästchen **Änderungen speichern**.</span><span class="sxs-lookup"><span data-stu-id="59efb-214">Then, select **Save Changes**.</span></span>   

    ![Seite "Details" basisanwendung](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="59efb-216">Beachten Sie, dass Sie nur authentifizieren mit dem e-Mail-Alias, den Sie registriert haben.</span><span class="sxs-lookup"><span data-stu-id="59efb-216">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="59efb-217">Andere Benutzer und Testkonten werden nicht registrieren können.</span><span class="sxs-lookup"><span data-stu-id="59efb-217">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="59efb-218">Sie können andere Facebook Konten Zugriff auf die Anwendung auf die Facebook gewähren **Developer Rollen** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="59efb-218">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="59efb-219">Öffnen Sie in Visual Studio *App\_Start\Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="59efb-219">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="59efb-220">Kopieren Sie die **AppId** und **App-Geheimnis** in die `UseFacebookAuthentication` Methode.</span><span class="sxs-lookup"><span data-stu-id="59efb-220">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="59efb-221">Die **AppId** und **App-Geheimnis** unten aufgeführten Werte sind Beispiele und funktionieren nicht.</span><span class="sxs-lookup"><span data-stu-id="59efb-221">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="59efb-222">Klicken Sie auf **Änderungen speichern**.</span><span class="sxs-lookup"><span data-stu-id="59efb-222">Click **Save Changes**.</span></span>
13. <span data-ttu-id="59efb-223">Drücken Sie **STRG + F5** um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="59efb-223">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="59efb-224">Wählen Sie **melden Sie sich** auf die Anmeldeseite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="59efb-224">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="59efb-225">Klicken Sie auf **Facebook** unter **einen anderen Dienst zum Anmelden verwenden.**</span><span class="sxs-lookup"><span data-stu-id="59efb-225">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="59efb-226">Geben Sie Ihre Facebook-Anmeldeinformationen ein.</span><span class="sxs-lookup"><span data-stu-id="59efb-226">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="59efb-227">Sie werden aufgefordert, die Berechtigung für die Anwendung den Zugriff auf Ihr öffentliches Profil und "Friend" Liste erteilen.</span><span class="sxs-lookup"><span data-stu-id="59efb-227">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Facebook-app-details](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="59efb-229">Sie sind jetzt angemeldet.</span><span class="sxs-lookup"><span data-stu-id="59efb-229">You are now logged in.</span></span> <span data-ttu-id="59efb-230">Sie können dieses Konto jetzt mit der Anwendung registrieren.</span><span class="sxs-lookup"><span data-stu-id="59efb-230">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="59efb-231">Wenn Sie sich registrieren, wird ein Eintrag hinzugefügt, um die *Benutzer* Tabelle der Mitgliedschaftsdatenbank.</span><span class="sxs-lookup"><span data-stu-id="59efb-231">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="59efb-232">Überprüfen Sie die Daten der Benutzergruppenmitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="59efb-232">Examine the Membership Data</span></span>

<span data-ttu-id="59efb-233">In der **Ansicht** Menü klicken Sie auf **Server-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="59efb-233">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="59efb-234">Erweitern Sie **DefaultConnection (MvcAuth)**, erweitern Sie **Tabellen**, klicken Sie mit der rechten Maustaste auf **AspNetUsers** , und klicken Sie auf **Tabellendaten anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="59efb-234">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![Tabellendaten aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="59efb-236">Hinzufügen von Profildaten für die Benutzerklasse</span><span class="sxs-lookup"><span data-stu-id="59efb-236">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="59efb-237">In diesem Abschnitt fügen Geburtsdatum und home Stadt auf die Daten des Benutzers während der Registrierung, Sie wie in der folgenden Abbildung gezeigt.</span><span class="sxs-lookup"><span data-stu-id="59efb-237">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![REG mit home Stadt und Geburtst.](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="59efb-239">Öffnen der *Models\IdentityModels.cs* Datei und der Eigenschaften für die Datums- und von zuhause Örtlichkeit Geburtsdatum hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="59efb-239">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="59efb-240">Öffnen der *Models\AccountViewModels.cs* Datei- und welche birth Date und von zuhause Örtlichkeit Eigenschaften in `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="59efb-240">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="59efb-241">Öffnen der *Controllers\AccountController.cs* Datei, und fügen Sie Code für Birth Date und von zuhause Stadt, in der `ExternalLoginConfirmation` Aktionsmethode wie gezeigt:</span><span class="sxs-lookup"><span data-stu-id="59efb-241">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="59efb-242">Geburtsdatum und home Örtlichkeit zum Hinzufügen der *Views\Account\ExternalLoginConfirmation.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="59efb-242">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="59efb-243">Löschen Sie die Mitgliedschaftsdatenbank, damit Sie erneut Ihrer Facebook-Konto mit Ihrer Anwendung registrieren und stellen Sie sicher, dass Sie die neue Geburtsdatum und Profilinformationen home Örtlichkeit hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="59efb-243">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="59efb-244">Von **Projektmappen-Explorer**, klicken Sie auf die **alle Dateien anzeigen** Symbol und dann mit der rechten Maustaste *hinzufügen\_Data\aspnet-MvcAuth -&lt;Datumsstempel&gt;mdf* , und klicken Sie auf **löschen**.</span><span class="sxs-lookup"><span data-stu-id="59efb-244">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="59efb-245">Aus der **Tools** Menü klicken Sie auf **NuGet-Paket-Manager**, klicken Sie dann auf **Package Manager Console** (PMC).</span><span class="sxs-lookup"><span data-stu-id="59efb-245">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="59efb-246">Geben Sie die folgenden Befehle in der Systemmonitor aus.</span><span class="sxs-lookup"><span data-stu-id="59efb-246">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="59efb-247">Enable-Migrationen</span><span class="sxs-lookup"><span data-stu-id="59efb-247">Enable-Migrations</span></span>
2. <span data-ttu-id="59efb-248">Init hinzufügen-Migration</span><span class="sxs-lookup"><span data-stu-id="59efb-248">Add-Migration Init</span></span>
3. <span data-ttu-id="59efb-249">Datenbank aktualisieren</span><span class="sxs-lookup"><span data-stu-id="59efb-249">Update-Database</span></span>

<span data-ttu-id="59efb-250">Führen Sie die Anwendung aus, und Verwenden von FaceBook und Google anmelden und registrieren einige Benutzer.</span><span class="sxs-lookup"><span data-stu-id="59efb-250">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="59efb-251">Überprüfen Sie die Daten der Benutzergruppenmitgliedschaft</span><span class="sxs-lookup"><span data-stu-id="59efb-251">Examine the Membership Data</span></span>

<span data-ttu-id="59efb-252">In der **Ansicht** Menü klicken Sie auf **Server-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="59efb-252">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="59efb-253">Klicken Sie mit der rechten Maustaste auf **AspNetUsers** , und klicken Sie auf **Tabellendaten anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="59efb-253">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="59efb-254">Die `HomeTown` und `BirthDate` Felder unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="59efb-254">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="59efb-255">Ihre App abmelden und mit einem anderen Konto anmelden</span><span class="sxs-lookup"><span data-stu-id="59efb-255">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="59efb-256">Melden Sie sich bei Ihrer app mit Facebook, und klicken Sie dann melden Sie sich ab und versucht, melden Sie sich werden erneut mit einem anderen Facebook-Konto (mit dem gleichen Browser), Sie sofort auf den vorherigen Facebook-Konto angemeldet sein, die Sie verwendet.</span><span class="sxs-lookup"><span data-stu-id="59efb-256">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="59efb-257">Um ein anderes Konto verwenden zu können, müssen Sie zum Facebook zu navigieren, und melden Sie sich am Facebook.</span><span class="sxs-lookup"><span data-stu-id="59efb-257">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="59efb-258">Die gleiche Regel gilt für alle anderen 3rd Party Authentifizierungsanbieter.</span><span class="sxs-lookup"><span data-stu-id="59efb-258">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="59efb-259">Alternativ können Sie mit einem anderen Konto anmelden, mit einem anderen Browser.</span><span class="sxs-lookup"><span data-stu-id="59efb-259">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="59efb-260">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="59efb-260">Next Steps</span></span>

<span data-ttu-id="59efb-261">Finden Sie unter [Einführung in die Yahoo und LinkedIn OAuth Sicherheitsanbieter für OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) von Jerrie Pelser Anweisungen Yahoo und LinkedIn aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="59efb-261">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="59efb-262">Finden Sie unter der Jerrie ziemlich sozialen Anmeldeschaltflächen für ASP.NET MVC 5 abzurufenden sozialen Anmeldeschaltflächen aktivieren.</span><span class="sxs-lookup"><span data-stu-id="59efb-262">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="59efb-263">Führen Sie meine Lernprogramm [eine ASP.NET MVC-app mit Authentifizierung und SQL-Datenbank erstellen und Bereitstellen von Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), die in diesem Lernprogramm fortgesetzt und zeigt die folgende:</span><span class="sxs-lookup"><span data-stu-id="59efb-263">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="59efb-264">Wie Sie Ihre app in Azure bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="59efb-264">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="59efb-265">Wie Sie Apps mit Rollen gesichert.</span><span class="sxs-lookup"><span data-stu-id="59efb-265">How to secure you app with roles.</span></span>
3. <span data-ttu-id="59efb-266">Zum Sichern Ihrer Apps mit der [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) und [autorisieren](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) Filter.</span><span class="sxs-lookup"><span data-stu-id="59efb-266">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="59efb-267">Wie die Mitgliedschafts-API zu verwenden, um Benutzer und Rollen hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="59efb-267">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="59efb-268">Lassen Sie Sie Feedback auf wie in diesem Lernprogramm mögen und was wir weiter verbessern können.</span><span class="sxs-lookup"><span data-stu-id="59efb-268">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="59efb-269">Sie können auch neue Themen am anfordern [Me wie mit Code anzeigen](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="59efb-269">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="59efb-270">Sie können sogar angefordert und Abstimmen über neue Funktionen in ASP.NET hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="59efb-270">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="59efb-271">Sie können z. B. für ein Tool zum Abstimmen [erstellen und Verwalten von Benutzern und Rollen.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="59efb-271">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="59efb-272">Eine gute Erklärung der Funktionsweise von ASP.NET externen Authentifizierungsdienste, finden Sie unter mcmurrays [externen Authentifizierungsdienste](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="59efb-272">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="59efb-273">Roberts Artikel geht auch ins Detail, bei der Aktivierung von Microsoft und Twitter-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="59efb-273">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="59efb-274">Tom Dykstra des ausgezeichnete [EF/MVC-Lernprogramm](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) wird gezeigt, wie mit dem Entity Framework funktionieren.</span><span class="sxs-lookup"><span data-stu-id="59efb-274">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
