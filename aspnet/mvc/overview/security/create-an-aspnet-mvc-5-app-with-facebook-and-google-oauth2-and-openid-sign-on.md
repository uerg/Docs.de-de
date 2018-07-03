---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Erstellen von MVC 5-App mit Facebook, Twitter, LinkedIn und Google OAuth2 Sign-on (c#) | Microsoft-Dokumentation
author: Rick-Anderson
description: In diesem Tutorial erfahren Sie, wie Sie eine ASP.NET MVC 5-Webanwendung erstellen, die Benutzern ermöglicht, melden Sie sich mithilfe von OAuth 2.0 mit den Anmeldeinformationen aus einer externen ne...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 8a9528f76b0166175f950543b4b8a7250bdf5100
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388765"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="6fefd-103">Erstellen einer ASP.NET MVC 5-App mit Facebook, Twitter, LinkedIn und Google OAuth2 Sign-on (c#)</span><span class="sxs-lookup"><span data-stu-id="6fefd-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="6fefd-104">durch [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="6fefd-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="6fefd-105">In diesem Tutorial erfahren Sie, wie zum Erstellen einer ASP.NET MVC 5-Webanwendung, die Benutzern ermöglicht, melden Sie sich mit [OAuth 2.0](http://oauth.net/2/) mit den Anmeldeinformationen eines externen Authentifizierungsanbieters, z. B. Facebook, Twitter, LinkedIn, Microsoft oder Google.</span><span class="sxs-lookup"><span data-stu-id="6fefd-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="6fefd-106">Der Einfachheit halber konzentriert sich in diesem Tutorial zum Arbeiten mit Anmeldeinformationen aus Facebook und Google.</span><span class="sxs-lookup"><span data-stu-id="6fefd-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="6fefd-107">Aktivieren diese Anmeldeinformationen in Ihrer Web Sites bietet entscheidende Vorteile, da Sie Millionen Benutzer bereits Konten mit dieser externen Anbietern haben.</span><span class="sxs-lookup"><span data-stu-id="6fefd-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="6fefd-108">Diese Benutzer möglicherweise eher für Ihre Website registrieren können, wenn sie nicht zum Erstellen und speichern einen neuen Satz von Anmeldeinformationen verfügen.</span><span class="sxs-lookup"><span data-stu-id="6fefd-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="6fefd-109">Siehe auch [ASP.NET MVC 5-app mit SMS und e-Mail-Adresse einer zweistufigen Authentifizierung](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="6fefd-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="6fefd-110">Das Tutorial zeigt auch das Hinzufügen von Profildaten für den Benutzer und das Membership-API zu verwenden, um Rollen hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="6fefd-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="6fefd-111">In diesem Tutorial wurde von geschrieben [Rick Anderson](https://blogs.msdn.com/rickAndy) (mir bitte auf Twitter folgen: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="6fefd-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="6fefd-112">Erste Schritte</span><span class="sxs-lookup"><span data-stu-id="6fefd-112">Getting Started</span></span>

<span data-ttu-id="6fefd-113">Zunächst installieren und Ausführen von [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="6fefd-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="6fefd-114">Installieren von Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) oder höher.</span><span class="sxs-lookup"><span data-stu-id="6fefd-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="6fefd-115">Hilfe bei Dropbox, GitHub, Linkedin, Instagram, Puffer, Salesforce, DATENSTROM, Stack Exchange, Tripit, twitch integrierte, Twitter, Yahoo! und vieles mehr, finden Sie in diesem [Beispielprojekt](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="6fefd-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="6fefd-116">Sie müssen Visual Studio installieren [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) oder höher, Google OAuth 2 zu verwenden und Lokales Debuggen ohne SSL-Warnungen.</span><span class="sxs-lookup"><span data-stu-id="6fefd-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="6fefd-117">Klicken Sie auf **neues Projekt** aus der **starten** Seite, oder Sie können, verwenden Sie das Menü, und wählen Sie **Datei**, und klicken Sie dann **neues Projekt**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="6fefd-118">Erstellen Ihrer ersten Anwendung</span><span class="sxs-lookup"><span data-stu-id="6fefd-118">Creating Your First Application</span></span>

<span data-ttu-id="6fefd-119">Klicken Sie auf **neues Projekt**, und wählen Sie dann **Visual C#-** auf der linken Seite, klicken Sie dann **Web** und wählen Sie dann **ASP.NET-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="6fefd-120">Benennen Sie das Projekt "MvcAuth", und klicken Sie dann auf **OK**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="6fefd-121">In der **neues ASP.NET-Projekt** Dialogfeld klicken Sie auf **MVC**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="6fefd-122">Die Authentifizierung ist nicht **einzelne Benutzerkonten**, klicken Sie auf die **Authentifizierung ändern** Schaltfläche, und wählen **einzelne Benutzerkonten**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="6fefd-123">Durch Überprüfen **in der Cloud hosten**, die app wird sehr einfach, die in Azure gehostet werden.</span><span class="sxs-lookup"><span data-stu-id="6fefd-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="6fefd-124">Wenn Sie ausgewählt haben **in der Cloud hosten**, führen Sie das Dialogfeld "konfigurieren".</span><span class="sxs-lookup"><span data-stu-id="6fefd-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="6fefd-125">Verwenden Sie NuGet, um auf die neueste OWIN-Middleware zu aktualisieren</span><span class="sxs-lookup"><span data-stu-id="6fefd-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="6fefd-126">Verwenden Sie den NuGet-Paket-Manager zum Aktualisieren der [OWIN-Middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="6fefd-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="6fefd-127">Wählen Sie **Updates** im linken Menü.</span><span class="sxs-lookup"><span data-stu-id="6fefd-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="6fefd-128">Klicken Sie auf die **alle aktualisieren** Schaltfläche oder Sie können nur OWIN-Pakete (in der nächsten Abbildung dargestellt) suchen:</span><span class="sxs-lookup"><span data-stu-id="6fefd-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="6fefd-129">In der folgenden Abbildung werden nur Pakete der OWIN angezeigt:</span><span class="sxs-lookup"><span data-stu-id="6fefd-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="6fefd-130">Über die Paket-Manager-Konsole (PMC), können Sie eingeben der `Update-Package` -Befehl, der alle Pakete aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="6fefd-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="6fefd-131">Drücken Sie **F5** oder **STRG + F5** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="6fefd-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="6fefd-132">In der folgenden Abbildung ist die Nummer des Ports 1234.</span><span class="sxs-lookup"><span data-stu-id="6fefd-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="6fefd-133">Wenn Sie die Anwendung ausführen, sehen Sie eine andere Portnummer an.</span><span class="sxs-lookup"><span data-stu-id="6fefd-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="6fefd-134">Je nach Größe Ihres Browserfensters, müssen Sie möglicherweise die Symbol "berichtsnavigation", um anzuzeigen, klicken Sie auf die **Startseite**, **zu**, **wenden Sie sich an**, **registrieren**und **melden Sie sich bei** Links.</span><span class="sxs-lookup"><span data-stu-id="6fefd-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="6fefd-135">Einrichten von SSL im Projekt</span><span class="sxs-lookup"><span data-stu-id="6fefd-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="6fefd-136">Um an den Authentifizierungsanbieter, z. B. Google und Facebook zu verbinden, müssen Sie IIS Express einrichten, um die Verwendung von SSL.</span><span class="sxs-lookup"><span data-stu-id="6fefd-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="6fefd-137">Es ist wichtig, nach der Anmeldung mithilfe von SSL nicht und abzulegen und wieder auf HTTP, Ihre Anmeldecookie wird nur als Geheimnis Ihres Benutzernamens und Kennworts und ohne Verwendung von SSL, die Sie in Klartext über das Netzwerk gesendet.</span><span class="sxs-lookup"><span data-stu-id="6fefd-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="6fefd-138">Darüber hinaus bietet Sie haben bereits die Zeit zum Ausführen des Handshakes und Sichern Sie den Kanal (Was ist der Großteil der macht langsamer als HTTP HTTPS) die MVC-Pipeline ausgeführt wird, nach dem Sie angemeldet sind also zurück an den HTTP-Umleitung wird nicht stellen Sie vor der aktuellen Anforderung oder in Zukunft Anforderungen, die viel schneller.</span><span class="sxs-lookup"><span data-stu-id="6fefd-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="6fefd-139">In **Projektmappen-Explorer**, klicken Sie auf die **MvcAuth** Projekt.</span><span class="sxs-lookup"><span data-stu-id="6fefd-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="6fefd-140">Drücken Sie die F4-Taste, um die Projekteigenschaften anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="6fefd-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="6fefd-141">Sie können auch aus der **Ansicht** Menü Sie auswählen können, **Fenster "Eigenschaften"**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="6fefd-142">Änderung **SSL-fähigen** auf "true".</span><span class="sxs-lookup"><span data-stu-id="6fefd-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="6fefd-143">Kopieren Sie die SSL-URL (die `https://localhost:44300/` , wenn Sie andere SSL-Projekte erstellt haben).</span><span class="sxs-lookup"><span data-stu-id="6fefd-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="6fefd-144">In **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf die **MvcAuth** Projekt, und wählen **Eigenschaften**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="6fefd-145">Wählen Sie die **Web** Registerkarte, und fügen Sie die SSL-URL in die **Projekt-Url** Feld.</span><span class="sxs-lookup"><span data-stu-id="6fefd-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="6fefd-146">Speichern Sie die Datei (STRG + S).</span><span class="sxs-lookup"><span data-stu-id="6fefd-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="6fefd-147">Sie benötigen diese URL zum Konfigurieren von Facebook und Google-Authentifizierung-apps.</span><span class="sxs-lookup"><span data-stu-id="6fefd-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="6fefd-148">Hinzufügen der [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) -Attribut auf die `Home` Controller alle Anforderungen erforderlich ist, muss HTTPS verwenden.</span><span class="sxs-lookup"><span data-stu-id="6fefd-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="6fefd-149">Eine sicherere Möglichkeit ist das Hinzufügen der [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) Filter, um die Anwendung.</span><span class="sxs-lookup"><span data-stu-id="6fefd-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="6fefd-150">Finden Sie im Abschnitt &quot;Schützen der Anwendung durch SSL und das Authorize-Attribut von&quot; in meiner tutoral [eine ASP.NET MVC-app mit Authentifizierung und SQL-Datenbank erstellen und Bereitstellen in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="6fefd-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="6fefd-151">Ein Teil der Home-Controller ist unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="6fefd-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="6fefd-152">Drücken Sie STRG+F5, um die Anwendung auszuführen.</span><span class="sxs-lookup"><span data-stu-id="6fefd-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="6fefd-153">Wenn Sie das Zertifikat in der Vergangenheit installiert haben, können Sie das im restlichen Teil dieses Abschnitts überspringen und fahren Sie mit [beim Erstellen einer Google-app for OAuth 2, und verbinden die app auf das Projekt](#goog), führen Sie andernfalls die Anweisungen, um das selbstsignierte vertrauen Zertifikat, das IIS Express generiert hat.</span><span class="sxs-lookup"><span data-stu-id="6fefd-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="6fefd-154">Lesen der **Sicherheitswarnung** Dialogfeld, und klicken Sie dann auf **Ja** ggf. zum Installieren des Zertifikats, das "localhost" darstellt.</span><span class="sxs-lookup"><span data-stu-id="6fefd-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="6fefd-155">Zeigt, d. h. die *Startseite* Seite, und es gibt keine SSL-Warnungen.</span><span class="sxs-lookup"><span data-stu-id="6fefd-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="6fefd-156">Google Chrome ist außerdem das Zertifikat akzeptiert und HTTPS-Inhalt ohne Warnung zeigt.</span><span class="sxs-lookup"><span data-stu-id="6fefd-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="6fefd-157">Firefox verwendet einen eigenen Zertifikatspeicher, damit sie eine Warnung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="6fefd-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="6fefd-158">Sie können problemlos klicken, für die Anwendung **ich verstehe die Risiken**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="6fefd-159">Beim Erstellen einer Google-app for OAuth 2, und verbinden die app auf das Projekt</span><span class="sxs-lookup"><span data-stu-id="6fefd-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="6fefd-160">Aktuelle Google-OAuth-Anweisungen, finden Sie unter [Konfigurieren von Google-Authentifizierung in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="6fefd-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="6fefd-161">Navigieren Sie zu der [Google-Entwicklerkonsole](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="6fefd-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="6fefd-162">Wenn Sie ein Projekt, bevor Sie erstellt haben, wählen Sie **Anmeldeinformationen** in der linken Registerkarte, und wählen Sie dann **erstellen**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="6fefd-163">Klicken Sie auf der linken Registerkarte auf **Anmeldeinformationen**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="6fefd-164">Klicken Sie auf **Anmeldeinformationen erstellen** dann **OAuth-Client-ID**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="6fefd-165">In der **Client-ID erstellen** Dialogfeld behalten Sie den Standardwert **Webanwendung** für den Anwendungstyp.</span><span class="sxs-lookup"><span data-stu-id="6fefd-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="6fefd-166">Legen Sie die **autorisierte JavaScript** Ursprünge, die oben verwendeten SSL-URL (`https://localhost:44300/` , wenn Sie andere SSL-Projekte erstellt haben)</span><span class="sxs-lookup"><span data-stu-id="6fefd-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="6fefd-167">Legen Sie die **autorisierte umleitungs-URI** auf:</span><span class="sxs-lookup"><span data-stu-id="6fefd-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="6fefd-168">Klicken Sie auf das Menüelement des OAuth-Zustimmung-Bildschirm, und setzen Sie Ihre e-Mail-Adresse und Product Name.</span><span class="sxs-lookup"><span data-stu-id="6fefd-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="6fefd-169">Sie haben beim Klicken Sie im Formular **speichern**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="6fefd-170">Klicken Sie auf das Menüelement für die Bibliothek, die nach **Google + API**, klicken Sie darauf, und drücken Sie dann aktivieren.</span><span class="sxs-lookup"><span data-stu-id="6fefd-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="6fefd-171">Die folgende Abbildung zeigt die aktivierte APIs.</span><span class="sxs-lookup"><span data-stu-id="6fefd-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="6fefd-172">Über den Google APIs-API-Manager finden Sie auf die **Anmeldeinformationen** Registerkarte zum Abrufen der **Client-ID**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="6fefd-173">Laden Sie dieses herunter, um eine JSON-Datei mit Geheimnissen aus Anwendungen zu speichern.</span><span class="sxs-lookup"><span data-stu-id="6fefd-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="6fefd-174">Kopieren und Einfügen der **"ClientID"** und **ClientSecret** in die `UseGoogleAuthentication` Methode finden Sie unter den *Startup.Auth.cs* Datei die *App_Start* Ordner.</span><span class="sxs-lookup"><span data-stu-id="6fefd-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="6fefd-175">Die **"ClientID"** und **ClientSecret** unten aufgeführten Werte sind Beispiele und funktionieren nicht.</span><span class="sxs-lookup"><span data-stu-id="6fefd-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="6fefd-176">Sicherheit: vertrauliche Daten niemals in Ihrem Quellcode speichern.</span><span class="sxs-lookup"><span data-stu-id="6fefd-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="6fefd-177">Das Konto und die Anmeldeinformationen werden auf den Code aus, um das Beispiel möglichst einfach zu halten hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="6fefd-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="6fefd-178">Finden Sie unter [bewährte Methoden für die Bereitstellung von Kennwörtern und anderen vertraulichen Daten in ASP.NET und Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="6fefd-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="6fefd-179">Drücken Sie **STRG + F5** erstellen und Ausführen der Anwendungs.</span><span class="sxs-lookup"><span data-stu-id="6fefd-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="6fefd-180">Klicken Sie auf die **melden Sie sich bei** Link.</span><span class="sxs-lookup"><span data-stu-id="6fefd-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="6fefd-181">Klicken Sie unter **verwenden Sie einen anderen Dienst anmelden**, klicken Sie auf **Google**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="6fefd-182">Wenn Sie einen der oben genannten Schritte verpasst haben, erhalten Sie einen HTTP 401-Fehler.</span><span class="sxs-lookup"><span data-stu-id="6fefd-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="6fefd-183">Überprüfen Sie die obigen Schritte erneut.</span><span class="sxs-lookup"><span data-stu-id="6fefd-183">Recheck your steps above.</span></span> <span data-ttu-id="6fefd-184">Wenn Sie eine erforderliche Einstellung verpasst haben (z. B. **Produktname**), fügen Sie das fehlende Element hinzu, und speichern, es dauert einige Minuten, bis die Authentifizierung funktioniert.</span><span class="sxs-lookup"><span data-stu-id="6fefd-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="6fefd-185">Sie werden zur Google-Website umgeleitet, wo Sie Ihre Anmeldeinformationen eingeben.</span><span class="sxs-lookup"><span data-stu-id="6fefd-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="6fefd-186">Nachdem Sie Ihre Anmeldeinformationen eingegeben haben, werden Sie aufgefordert werden, so erteilen Berechtigungen für die Webanwendung, die Sie gerade erstellt haben:</span><span class="sxs-lookup"><span data-stu-id="6fefd-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="6fefd-187">Klicken Sie auf **akzeptieren**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-187">Click **Accept**.</span></span> <span data-ttu-id="6fefd-188">Jetzt werden Sie umgeleitet an die **registrieren** Seite der Anwendung MvcAuth, wo Sie Ihr Google-Konto registrieren.</span><span class="sxs-lookup"><span data-stu-id="6fefd-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="6fefd-189">Sie haben die Möglichkeit, den Namen lokaler e-Mail-Registrierung für Ihr Gmail-Konto zu ändern, aber Sie möchten in der Regel behalten Sie das Standard-e-Mail-Alias (d. h. diejenige, die Sie für die Authentifizierung verwendet).</span><span class="sxs-lookup"><span data-stu-id="6fefd-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="6fefd-190">Klicken Sie auf **Registrieren**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="6fefd-191">Erstellen die app in Facebook, und verbinden die app auf das Projekt</span><span class="sxs-lookup"><span data-stu-id="6fefd-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="6fefd-192">Aktuelle Anweisungen für die Facebook OAuth2-Authentifizierung, finden Sie unter [Konfigurieren der Facebook-Authentifizierung](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="6fefd-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<span data-ttu-id="6fefd-193">Für die Facebook-OAuth2-Authentifizierung müssen Sie in Ihr Projekt einige Einstellungen aus einer Anwendung kopieren, die Sie in Facebook zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6fefd-193">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="6fefd-194">Wechseln Sie in Ihrem Browser zu [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) und melden Sie sich durch Ihre Facebook-Anmeldeinformationen eingeben.</span><span class="sxs-lookup"><span data-stu-id="6fefd-194">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="6fefd-195">Wenn Sie bereits als Facebook-Entwickler registriert sind, klicken Sie auf **registrieren Sie sich als Entwickler** und befolgen Sie die Anweisungen zum Registrieren.</span><span class="sxs-lookup"><span data-stu-id="6fefd-195">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="6fefd-196">Auf der **Apps** auf **Create New App**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-196">On the **Apps** tab, click **Create New App**.</span></span>

    ![Neue app erstellen](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="6fefd-198">Geben Sie eine **Anwendungsnamen** und **Kategorie**, klicken Sie dann auf **Create App**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-198">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="6fefd-199">Die <strong>App Namespace</strong> ist der Teil der URL, die Ihre App Zugriff auf die Facebook-Anwendung für die Authentifizierung verwendet wird (z. B. Https\://apps.facebook.com/{App Namespace}).</span><span class="sxs-lookup"><span data-stu-id="6fefd-199">The <strong>App Namespace</strong> is the part of the URL that your App will use to access the Facebook application for authentication (for example, https\://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="6fefd-200">Wenn Sie nicht angeben einer <strong>App-Namespace</strong>, <strong>App-ID</strong> für die URL verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="6fefd-200">If you don't specify an <strong>App Namespace</strong>, the <strong>App ID</strong> will be used for the URL.</span></span> <span data-ttu-id="6fefd-201">Die <strong>App-ID</strong> ist eine lange vom System generierte Zahl, die Sie im nächsten Schritt sehen werden.</span><span class="sxs-lookup"><span data-stu-id="6fefd-201">The <strong>App ID</strong> is a long system-generated number that you will see in the next step.</span></span>

    ![Dialogfeld "neue App" erstellen](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="6fefd-203">Senden Sie die standard-sicherheitsüberprüfung.</span><span class="sxs-lookup"><span data-stu-id="6fefd-203">Submit the standard security check.</span></span>

    ![Sicherheitsüberprüfung](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="6fefd-205">Wählen Sie **Einstellungen** für den linken Menüleiste![ Facebook-Entwickler-Menüleiste](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="6fefd-205">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="6fefd-206">Auf der **grundlegende** wählen Sie die im Einstellungsabschnitt der Seite **Plattform hinzufügen** um anzugeben, dass Sie eine Website-Anwendung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6fefd-206">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="6fefd-207">![Grundlegende Einstellungen](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="6fefd-207">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="6fefd-208">Wählen Sie **Website** aus den Plattformoptionen.</span><span class="sxs-lookup"><span data-stu-id="6fefd-208">Select **Website** from the platform choices.</span></span>  
  
    ![Plattform-Optionen](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="6fefd-210">Notieren Sie sich Ihre **App-ID** und Ihre **App-Geheimnis** , damit Sie später in diesem Lernprogramm beide Zertifikate in Ihrer MVC-Anwendung hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="6fefd-210">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="6fefd-211">Darüber hinaus fügen Sie Ihre Website-URL (`https://localhost:44300/`) zum Testen Ihrer MVC-Anwendung.</span><span class="sxs-lookup"><span data-stu-id="6fefd-211">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="6fefd-212">Fügen Sie außerdem eine **Contact Email**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-212">Also, add a **Contact Email**.</span></span> <span data-ttu-id="6fefd-213">Wählen Sie dann **Save Changes**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-213">Then, select **Save Changes**.</span></span>   

    ![Basisanwendung Seite "Details"](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="6fefd-215">Beachten Sie, dass Sie nur können für die Authentifizierung mit den e-Mail-Alias ein, die, den Sie registriert haben.</span><span class="sxs-lookup"><span data-stu-id="6fefd-215">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="6fefd-216">Andere Benutzer und Testkonten werden nicht registrieren können.</span><span class="sxs-lookup"><span data-stu-id="6fefd-216">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="6fefd-217">Sie können andere Facebook des Zugriffs auf die Anwendung auf Facebook gewähren **Entwickler** Registerkarte.</span><span class="sxs-lookup"><span data-stu-id="6fefd-217">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="6fefd-218">Öffnen Sie in Visual Studio *App\_Start\Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="6fefd-218">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="6fefd-219">Kopieren Sie die **AppId** und **App-Geheimnis** in die `UseFacebookAuthentication` Methode.</span><span class="sxs-lookup"><span data-stu-id="6fefd-219">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="6fefd-220">Die **AppId** und **App-Geheimnis** unten aufgeführten Werte sind Beispiele und funktionieren nicht.</span><span class="sxs-lookup"><span data-stu-id="6fefd-220">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="6fefd-221">Klicken Sie auf **speichern Änderungen**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-221">Click **Save Changes**.</span></span>
13. <span data-ttu-id="6fefd-222">Drücken Sie **STRG + F5** zum Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="6fefd-222">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="6fefd-223">Wählen Sie **melden Sie sich bei** die Anmeldeseite angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6fefd-223">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="6fefd-224">Klicken Sie auf **Facebook** unter **verwenden Sie einen anderen Dienst anmelden.**</span><span class="sxs-lookup"><span data-stu-id="6fefd-224">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="6fefd-225">Geben Sie Ihre Facebook-Anmeldeinformationen ein.</span><span class="sxs-lookup"><span data-stu-id="6fefd-225">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="6fefd-226">Sie werden aufgefordert, die zum Erteilen der Berechtigung für die Anwendung den Zugriff auf Ihr öffentliches Profil und die Friend-Liste.</span><span class="sxs-lookup"><span data-stu-id="6fefd-226">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Facebook-app-details](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="6fefd-228">Sie sind jetzt angemeldet.</span><span class="sxs-lookup"><span data-stu-id="6fefd-228">You are now logged in.</span></span> <span data-ttu-id="6fefd-229">Sie können dieses Konto jetzt mit der Anwendung registrieren.</span><span class="sxs-lookup"><span data-stu-id="6fefd-229">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="6fefd-230">Wenn Sie sich registrieren, wird ein Eintrag hinzugefügt, um die *Benutzer* Tabelle der Mitgliedschaftsdatenbank.</span><span class="sxs-lookup"><span data-stu-id="6fefd-230">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="6fefd-231">Überprüfen Sie die Mitgliedschaftsdaten</span><span class="sxs-lookup"><span data-stu-id="6fefd-231">Examine the Membership Data</span></span>

<span data-ttu-id="6fefd-232">In der **Ansicht** Menü klicken Sie auf **Server-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-232">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="6fefd-233">Erweitern Sie **DefaultConnection (MvcAuth)**, erweitern Sie **Tabellen**, klicken Sie mit der rechten Maustaste auf **"aspnetusers"** , und klicken Sie auf **Tabellendaten anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-233">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![Tabellendaten "aspnetusers"](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="6fefd-235">Hinzufügen von Profildaten zur Benutzerklasse</span><span class="sxs-lookup"><span data-stu-id="6fefd-235">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="6fefd-236">In diesem Abschnitt fügen Geburtsdatum und home Stadt auf die Benutzerdaten während der Registrierung, Sie wie in der folgenden Abbildung dargestellt.</span><span class="sxs-lookup"><span data-stu-id="6fefd-236">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![REG mit home Town und Geburtst.](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="6fefd-238">Öffnen der *Models\IdentityModels.cs* -Datei und fügen Sie Birth Date als auch zu Hause Town-Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="6fefd-238">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="6fefd-239">Öffnen der *Models\AccountViewModels.cs* -Datei und die Gruppe birth Date als auch zu Hause Town-Eigenschaften in `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="6fefd-239">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="6fefd-240">Öffnen der *controllers\accountcontroller* Datei, und fügen Sie Code für Birth Date als auch zu Hause Stadt in der `ExternalLoginConfirmation` Aktionsmethode wie gezeigt:</span><span class="sxs-lookup"><span data-stu-id="6fefd-240">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="6fefd-241">Geburtsdatum und home Town zum Hinzufügen der *Views\Account\ExternalLoginConfirmation.cshtml* Datei:</span><span class="sxs-lookup"><span data-stu-id="6fefd-241">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="6fefd-242">Löschen Sie die Mitgliedschaftsdatenbank, damit Sie erneut registrieren Sie Ihr Facebook-Konto mit Ihrer Anwendung und stellen Sie sicher, dass Sie die neue Geburtsdatum und home Town-Profilinformationen hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="6fefd-242">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="6fefd-243">Von **Projektmappen-Explorer**, klicken Sie auf die **alle Dateien anzeigen** Symbol, und klicken Sie dann auf Rechtsklick *hinzufügen\_Data\aspnet-MvcAuth -&lt;Datumsstempel&gt;-.mdf* , und klicken Sie auf **löschen**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-243">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="6fefd-244">Von der **Tools** Menü klicken Sie auf **NuGet-Paket-Manager**, klicken Sie dann auf **-Paket-Manager-Konsole** (PMC).</span><span class="sxs-lookup"><span data-stu-id="6fefd-244">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="6fefd-245">Geben Sie die folgenden Befehle aus, in der PMC.</span><span class="sxs-lookup"><span data-stu-id="6fefd-245">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="6fefd-246">Enable-Migrations</span><span class="sxs-lookup"><span data-stu-id="6fefd-246">Enable-Migrations</span></span>
2. <span data-ttu-id="6fefd-247">Add-Migration-Init</span><span class="sxs-lookup"><span data-stu-id="6fefd-247">Add-Migration Init</span></span>
3. <span data-ttu-id="6fefd-248">Datenbank aktualisieren</span><span class="sxs-lookup"><span data-stu-id="6fefd-248">Update-Database</span></span>

<span data-ttu-id="6fefd-249">Führen Sie die Anwendung aus, und Verwenden von FaceBook und Google, sich anzumelden und einige Benutzer zu registrieren.</span><span class="sxs-lookup"><span data-stu-id="6fefd-249">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="6fefd-250">Überprüfen Sie die Mitgliedschaftsdaten</span><span class="sxs-lookup"><span data-stu-id="6fefd-250">Examine the Membership Data</span></span>

<span data-ttu-id="6fefd-251">In der **Ansicht** Menü klicken Sie auf **Server-Explorer**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-251">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="6fefd-252">Klicken Sie mit der rechten Maustaste auf **"aspnetusers"** , und klicken Sie auf **Tabellendaten anzeigen**.</span><span class="sxs-lookup"><span data-stu-id="6fefd-252">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="6fefd-253">Die `HomeTown` und `BirthDate` Felder werden unten angezeigt.</span><span class="sxs-lookup"><span data-stu-id="6fefd-253">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="6fefd-254">Der App abmelden und mit einem anderen Konto anmelden</span><span class="sxs-lookup"><span data-stu-id="6fefd-254">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="6fefd-255">Wenn Sie melden Sie sich an Ihre app mit Facebook, und melden Sie sich, und versuchen Sie es für die Anmeldung werden erneut mit einem anderen Facebook-Konto (mit dem gleichen Browser), Sie sofort mit der vorherigen Facebook-Konto angemeldet sein, die Sie verwendet.</span><span class="sxs-lookup"><span data-stu-id="6fefd-255">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="6fefd-256">Um ein anderes Konto verwenden möchten, müssen Sie navigieren zu Facebook, und melden Sie sich bei Facebook.</span><span class="sxs-lookup"><span data-stu-id="6fefd-256">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="6fefd-257">Das gleiche gilt für alle anderen 3rd Party Authentication-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="6fefd-257">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="6fefd-258">Alternativ können Sie mit einem anderen Konto anmelden, mit einem anderen Browser.</span><span class="sxs-lookup"><span data-stu-id="6fefd-258">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6fefd-259">Nächste Schritte</span><span class="sxs-lookup"><span data-stu-id="6fefd-259">Next Steps</span></span>

<span data-ttu-id="6fefd-260">Finden Sie unter [Einführung in die Yahoo und LinkedIn-OAuth-Sicherheitsanbieter für OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) von Jerrie Pelser Yahoo und LinkedIn-Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="6fefd-260">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="6fefd-261">Finden Sie unter der Jerrie so ziemlich sozialen Netzwerken basierende Anmeldung Schaltflächen für ASP.NET MVC 5, um die Anmeldung für soziale Netzwerke Schaltflächen aktivieren zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="6fefd-261">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="6fefd-262">Führen Sie meine Tutorial [eine ASP.NET MVC-app mit Authentifizierung und SQL-Datenbank erstellen und Bereitstellen in Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), die in diesem Tutorial fortgesetzt, und zeigt die folgende:</span><span class="sxs-lookup"><span data-stu-id="6fefd-262">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="6fefd-263">Informationen zum Bereitstellen Ihrer app in Azure.</span><span class="sxs-lookup"><span data-stu-id="6fefd-263">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="6fefd-264">Wie Sie app-Rollen sichern.</span><span class="sxs-lookup"><span data-stu-id="6fefd-264">How to secure you app with roles.</span></span>
3. <span data-ttu-id="6fefd-265">So sichern Sie Ihre app mit der [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) und [autorisieren](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) Filter.</span><span class="sxs-lookup"><span data-stu-id="6fefd-265">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="6fefd-266">Verwenden die Mitgliedschafts-API Benutzer und Rollen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="6fefd-266">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="6fefd-267">Lassen Sie Feedback auf, wie Sie in diesem Tutorial gefallen hat und was wir verbessern können.</span><span class="sxs-lookup"><span data-stu-id="6fefd-267">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="6fefd-268">Sie können auch neue Themen anfordern [anzeigen Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="6fefd-268">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="6fefd-269">Sie können sogar Fragen und stimmen über neue Funktionen in ASP.NET hinzugefügt werden.</span><span class="sxs-lookup"><span data-stu-id="6fefd-269">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="6fefd-270">Sie können z. B. für ein Tool zum Abstimmen [erstellen und Verwalten von Benutzern und Rollen.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="6fefd-270">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="6fefd-271">Eine gute Erklärung der Funktionsweise von ASP.NET External Authentication Services, finden Sie unter mcmurrays [externe Authentifizierungsdienste](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="6fefd-271">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="6fefd-272">Den Artikel von Robert wird auch im Detail, bei der Aktivierung von Microsoft und Twitter-Authentifizierung.</span><span class="sxs-lookup"><span data-stu-id="6fefd-272">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="6fefd-273">Tom Dykstra die ausgezeichnete [EF/MVC-Tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) für die Arbeit mit dem Entity Framework veranschaulicht.</span><span class="sxs-lookup"><span data-stu-id="6fefd-273">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
