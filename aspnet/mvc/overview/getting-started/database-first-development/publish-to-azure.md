---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: "MVC Database First-Site in Azure veröffentlichen | Microsoft Docs"
author: tfitzmac
description: "MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt. Dieses Lernprogramm Seri..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: eadc0f2b08df29f80fe53d03cf88cd3cdcecfb12
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2018
---
<a name="publish-mvc-database-first-site-to-azure"></a><span data-ttu-id="986d3-104">Veröffentlichen Sie MVC Database First-Site in Azure.</span><span class="sxs-lookup"><span data-stu-id="986d3-104">Publish MVC Database First site to Azure</span></span>
====================
<span data-ttu-id="986d3-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="986d3-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="986d3-106">MVC, Entity Framework und ASP.NET Gerüstbau verwenden, können Sie eine Webanwendung erstellen, die eine Schnittstelle zu einer vorhandenen Datenbank bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="986d3-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="986d3-107">Diese Reihe von Lernprogrammen wird gezeigt, wie automatisch generieren von Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="986d3-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="986d3-108">Der generierte Code entspricht den Spalten in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="986d3-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="986d3-109">In diesem Teil der Reihe konzentriert sich auf die Web-app und die Datenbank in Azure veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="986d3-109">This part of the series focuses on publishing the web app and database to Azure.</span></span> <span data-ttu-id="986d3-110">Erfahren Sie in diesem Thema erfahren Sie mehr über das Veröffentlichen einer Web-app und einer Datenbank, aber tatsächlich auf die Schritte, die Sie am Anfang des Lernprogramms beginnen müssen.</span><span class="sxs-lookup"><span data-stu-id="986d3-110">You can read this topic to learn about publishing a web app and database, but to actually perform the steps you must start at the beginning of the tutorial.</span></span> <span data-ttu-id="986d3-111">Finden Sie unter [Einstieg](setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="986d3-111">See [Getting Started](setting-up-database.md).</span></span>


## <a name="deploy-your-web-app-on-azure"></a><span data-ttu-id="986d3-112">Ihre Web-app in Azure bereitstellen</span><span class="sxs-lookup"><span data-stu-id="986d3-112">Deploy your web app on Azure</span></span>

<span data-ttu-id="986d3-113">Sie benötigen ein Azure-Konto dieses Lernprogramms:</span><span class="sxs-lookup"><span data-stu-id="986d3-113">You need an Azure account to complete this tutorial:</span></span>

- <span data-ttu-id="986d3-114">Sie können [öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) -erhalten Sie Gutschriften können Sie kostenpflichtige Azure-Dienste zu testen und sogar nachdem sie verwendet werden bis können Sie das Konto beibehalten und Verwendung frei Azure-Dienste.</span><span class="sxs-lookup"><span data-stu-id="986d3-114">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="986d3-115">Sie können [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -Ihr MSDN-Abonnement erhalten Sie Gutschriften jedes Monats, die Sie für kostenpflichtige Azure-Dienste verwenden können.</span><span class="sxs-lookup"><span data-stu-id="986d3-115">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

<span data-ttu-id="986d3-116">Um Ihre Web-app zu veröffentlichen, mit der rechten Maustaste des Projekts, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="986d3-116">To publish your web app, right-click the project and select **Publish**.</span></span>

![Website veröffentlichen](publish-to-azure/_static/image1.png)

<span data-ttu-id="986d3-118">Wählen Sie Microsoft Azure-Websites.</span><span class="sxs-lookup"><span data-stu-id="986d3-118">Select Microsoft Azure Websites.</span></span>

![Auswählen von Azure](publish-to-azure/_static/image2.png)

<span data-ttu-id="986d3-120">Wenn Sie nicht bei Azure angemeldet sind, geben Sie Ihre Azure-Konto-Anmeldeinformationen.</span><span class="sxs-lookup"><span data-stu-id="986d3-120">If you are not signed in to Azure, provide your Azure account credentials.</span></span> <span data-ttu-id="986d3-121">Wählen Sie anschließend neu, um eine neue Web-app zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="986d3-121">Then, select New to create a new web app.</span></span>

![neue Website](publish-to-azure/_static/image3.png)

<span data-ttu-id="986d3-123">Erstellen Sie einen eindeutigen Namen für Ihre Web-app.</span><span class="sxs-lookup"><span data-stu-id="986d3-123">Create a unique name for your web app.</span></span> <span data-ttu-id="986d3-124">Sie werden wissen, dass der Name eindeutig ist, wenn Sie rechts neben dem Namensfeld ein grünes Häkchen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="986d3-124">You will know the name is unique if you see a green check mark to the right of the name field.</span></span> <span data-ttu-id="986d3-125">Wählen Sie eine Region für Ihre Web-app.</span><span class="sxs-lookup"><span data-stu-id="986d3-125">Select a region for your web app.</span></span> <span data-ttu-id="986d3-126">Wählen Sie **Erstellen neuer Server** für die Datenbank, und geben Sie einen Benutzernamen und ein Kennwort für diesen neuen Datenbankserver.</span><span class="sxs-lookup"><span data-stu-id="986d3-126">Select **Create new server** for the database, and provide a user name and password for this new database server.</span></span> <span data-ttu-id="986d3-127">Wenn Sie fertig sind, klicken Sie auf **erstellen**.</span><span class="sxs-lookup"><span data-stu-id="986d3-127">When finished, click **Create**.</span></span>

![Website erstellen](publish-to-azure/_static/image4.png)

<span data-ttu-id="986d3-129">Die Verbindungswerte werden jetzt alle festgelegt.</span><span class="sxs-lookup"><span data-stu-id="986d3-129">Your connection values are now all set.</span></span> <span data-ttu-id="986d3-130">Sie können diese Werte unverändert lassen.</span><span class="sxs-lookup"><span data-stu-id="986d3-130">You can leave these values unchanged.</span></span>

![connection](publish-to-azure/_static/image5.png)

<span data-ttu-id="986d3-132">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="986d3-132">Click **Next**.</span></span>

<span data-ttu-id="986d3-133">Einstellungen, beachten Sie, dass zwei Verbindungen Datenbank angegebene - ApplicationDBContext und ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="986d3-133">For Settings, notice that two database connections are specified - ApplicationDBContext and ContosoUniversityDataEntities.</span></span> <span data-ttu-id="986d3-134">ApplicationDBContext ist die Verbindung für Benutzertabellen-Konto.</span><span class="sxs-lookup"><span data-stu-id="986d3-134">ApplicationDBContext is the connection for user account tables.</span></span> <span data-ttu-id="986d3-135">Diese Werte werden nur die Verbindungszeichenfolgen für die Datenbanken anzeigen.</span><span class="sxs-lookup"><span data-stu-id="986d3-135">These values only show the connection strings for the databases.</span></span> <span data-ttu-id="986d3-136">Es bedeutet nicht, dass diese Datenbanken veröffentlicht werden, wenn Sie Ihre Website veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="986d3-136">It does not mean that these databases will be published when you publish your site.</span></span> <span data-ttu-id="986d3-137">Veröffentlichen Sie Ihr Datenbankprojekt Sie nach dem Veröffentlichen der Web-app.</span><span class="sxs-lookup"><span data-stu-id="986d3-137">You will publish your database project after you have finished publishing the web app.</span></span>

![](publish-to-azure/_static/image6.png)

<span data-ttu-id="986d3-138">Die Auslassungspunkte (...) neben der Verbindung mit der Datenbank werden Sie die Details der Verbindungszeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="986d3-138">The ellipsis (...) next to the database connection shows you the details of the connection string.</span></span> <span data-ttu-id="986d3-139">Klicken Sie auf die Auslassungspunkte neben ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="986d3-139">Click the ellipsis next to ContosoUniversityDataEntities.</span></span>

![](publish-to-azure/_static/image7.png)

<span data-ttu-id="986d3-140">Notieren Sie den Namen des Datenbankservers und der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="986d3-140">Note the name of the database server and the database.</span></span> <span data-ttu-id="986d3-141">Den Namen des Servers wird nach dem Zufallsprinzip generiert.</span><span class="sxs-lookup"><span data-stu-id="986d3-141">The server name is randomly generated.</span></span> <span data-ttu-id="986d3-142">Der Datenbankname ist einfach der Name Ihres Standorts mit  **\_Db** angefügt.</span><span class="sxs-lookup"><span data-stu-id="986d3-142">The database name is simple the name of your site with **\_db** appended.</span></span> <span data-ttu-id="986d3-143">Sie benötigen beide Namen später, wenn Sie Ihre Datenbank veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="986d3-143">You will need both names later when you publish your database.</span></span>

<span data-ttu-id="986d3-144">Klicken Sie auf **OK** Verbindungsfenster Zeichenfolge Datenbank schließen.</span><span class="sxs-lookup"><span data-stu-id="986d3-144">Click **OK** to close the database connection string window.</span></span>

<span data-ttu-id="986d3-145">Im Fenster "Web veröffentlichen", klicken Sie auf **Weiter** der Vorschau anzeigen.</span><span class="sxs-lookup"><span data-stu-id="986d3-145">In the Publish Web window, click **Next** to see the preview.</span></span>

![](publish-to-azure/_static/image8.png)

<span data-ttu-id="986d3-146">Sie können die Vorschau zum Anzeigen einer Liste der Dateien veröffentlichen starten klicken.</span><span class="sxs-lookup"><span data-stu-id="986d3-146">You can click Start Preview to see a list of the files to publish.</span></span> <span data-ttu-id="986d3-147">Da dies das erste Mal Sie dieser Website veröffentlicht haben ist, ist die Liste alle relevanten Dateien im Projekt aus.</span><span class="sxs-lookup"><span data-stu-id="986d3-147">Since this is the first time you have published this site, the list is every relevant file in the project.</span></span>

<span data-ttu-id="986d3-148">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="986d3-148">Click **Publish**.</span></span>

<span data-ttu-id="986d3-149">Im Ausgabebereich wird das Ergebnis der Publikation angezeigt.</span><span class="sxs-lookup"><span data-stu-id="986d3-149">The Output pane will display the result of your publication.</span></span>

![](publish-to-azure/_static/image9.png)

<span data-ttu-id="986d3-150">Der Standort wird nach der Veröffentlichung Immedialely in einem Webbrowser gestartet.</span><span class="sxs-lookup"><span data-stu-id="986d3-150">After publication, the site is immedialely launched in a web browser.</span></span> <span data-ttu-id="986d3-151">Ihre Website bereitgestellt wurde, und Sie können einen neuen Benutzer mit dem Standort registrieren; Allerdings wurden die Tabellen im Projekt ContosoUniversityData noch nicht veröffentlicht wurde.</span><span class="sxs-lookup"><span data-stu-id="986d3-151">Your site has been deployed and you can register a new user to the site; however, your tables in the ContosoUniversityData project have not yet been published.</span></span> <span data-ttu-id="986d3-152">Wenn Sie auf die Liste der Schüler Link klicken, wird eine Fehlermeldung.</span><span class="sxs-lookup"><span data-stu-id="986d3-152">If you click on the List of students link you will receive an error.</span></span>

## <a name="publish-database-to-sql-azure"></a><span data-ttu-id="986d3-153">Veröffentlichen Sie die Datenbank in SQL Azure</span><span class="sxs-lookup"><span data-stu-id="986d3-153">Publish database to SQL Azure</span></span>

<span data-ttu-id="986d3-154">Vor dem Veröffentlichen der Datenbank, müssen Sie sicherstellen, dass es sich bei Ihrem lokale Computer eine Verbindung mit dem Datenbankserver herstellen kann.</span><span class="sxs-lookup"><span data-stu-id="986d3-154">Before publishing your database, you must make sure your local computer can connect to the database server.</span></span> <span data-ttu-id="986d3-155">Die Firewall für den Datenbankserver schränkt die Computer mit der Datenbank herstellen können.</span><span class="sxs-lookup"><span data-stu-id="986d3-155">The firewall for your database server restricts which machines can connect to the database.</span></span> <span data-ttu-id="986d3-156">Sie müssen die IP-Adresse des Computers zu den zulässigen IP-Adressen für die Firewall hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="986d3-156">You need to add the IP address of your computer to the allowed IP addresses for the firewall.</span></span>

<span data-ttu-id="986d3-157">Melden Sie sich Ihr Azure-Konto über das Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="986d3-157">Login to your Azure account through the Azure portal.</span></span>

<span data-ttu-id="986d3-158">Wählen Sie die neue Datenbank, und wählen Sie **verwalten**.</span><span class="sxs-lookup"><span data-stu-id="986d3-158">Select your new database and select **Manage**.</span></span>

![Verwalten](publish-to-azure/_static/image10.png)

<span data-ttu-id="986d3-160">Sie müssen Ihr Datenbankserver zum Zulassen von Verbindungen von Ihrem Computer konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="986d3-160">You must configure your database server to allow connections from your computer.</span></span> <span data-ttu-id="986d3-161">Wenn Sie auf "verwalten" auswählen, werden Sie gefragt, um die aktuelle IP-Adresse hinzuzufügen, auf dem Datenbankserver erlaubt.</span><span class="sxs-lookup"><span data-stu-id="986d3-161">When you select Manage, you are asked to add the current IP address as permitted to the database server.</span></span> <span data-ttu-id="986d3-162">Wählen Sie Ja.</span><span class="sxs-lookup"><span data-stu-id="986d3-162">Select Yes.</span></span>

![IP-Adresse hinzufügen](publish-to-azure/_static/image11.png)

<span data-ttu-id="986d3-164">Es besteht die Möglichkeit, die die IP-Adresse, die Sie im vorherigen Schritt hinzugefügt haben nicht die einzige IP-Adresse ist, die Sie für Verbindungen zu konfigurieren müssen.</span><span class="sxs-lookup"><span data-stu-id="986d3-164">There is a chance that the IP address you added in the previous step is not the only IP address you need to configure for connections.</span></span> <span data-ttu-id="986d3-165">Sie können versuchen, bei der Anmeldung auf die Datenbank, um festzustellen, ob die Verbindungen ordnungsgemäß eingerichtet wurden.</span><span class="sxs-lookup"><span data-stu-id="986d3-165">You can attempt to login to the database to see if the connections have been properly set up.</span></span> <span data-ttu-id="986d3-166">Geben Sie den Benutzer und das Kennwort an, das Sie zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="986d3-166">Provide the user and password you created earlier.</span></span>

![Anmeldung](publish-to-azure/_static/image12.png)

<span data-ttu-id="986d3-168">Wenn Sie eine Fehlermeldung erhalten, müssen Sie eine andere IP-Adresse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="986d3-168">If you receive an error message, you need to add another IP address.</span></span> <span data-ttu-id="986d3-169">Klicken Sie auf die Fehlermeldung, um weitere Details zum Fehler anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="986d3-169">Click the error message to see more details about error.</span></span> <span data-ttu-id="986d3-170">In den Details sehen Sie die IP-Adresse, die Sie hinzufügen müssen.</span><span class="sxs-lookup"><span data-stu-id="986d3-170">In the details you will see the IP address that you need to add.</span></span> <span data-ttu-id="986d3-171">Beachten Sie diese IP-Adresse ein.</span><span class="sxs-lookup"><span data-stu-id="986d3-171">Note this IP address.</span></span>

![nicht zulässig](publish-to-azure/_static/image13.png)

<span data-ttu-id="986d3-173">Schließen Sie dieses Fenster für die Anmeldung, und kehren Sie zum Azure-Portal zurück.</span><span class="sxs-lookup"><span data-stu-id="986d3-173">Close this login window, and return to the Azure portal.</span></span>

<span data-ttu-id="986d3-174">Navigieren Sie zum Dashboard für Ihre Datenbank.</span><span class="sxs-lookup"><span data-stu-id="986d3-174">Navigate to the Dashboard for your database.</span></span> <span data-ttu-id="986d3-175">Klicken Sie auf **zulässige IP-Adressen verwalten**.</span><span class="sxs-lookup"><span data-stu-id="986d3-175">Click **Manage allowed IP addresses**.</span></span>

![IP-Adressen verwalten](publish-to-azure/_static/image14.png)

<span data-ttu-id="986d3-177">Jetzt müssen Sie die IP-Adresse aus der Fehlermeldung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="986d3-177">You must now add the IP address from the error message.</span></span> <span data-ttu-id="986d3-178">Ändern Sie den Bereich der zulässigen IP-Adressen von der Fehlermeldung eingeschlossen oder fügen Sie dieser IP-Adresse als ein separater Eintrag hinzu.</span><span class="sxs-lookup"><span data-stu-id="986d3-178">Either change the range of allowed IP addresses to include the one from the error message, or add that IP address as a separate entry.</span></span>

![neue Adresse hinzufügen](publish-to-azure/_static/image15.png)

<span data-ttu-id="986d3-180">Speichern Sie die Änderung zu zulässigen IP-Adressen an.</span><span class="sxs-lookup"><span data-stu-id="986d3-180">Save the change to allowed IP addresses.</span></span>

<span data-ttu-id="986d3-181">Klicken Sie auf verwalten, und versuchen Sie es erneut mit der Datenbank anmelden.</span><span class="sxs-lookup"><span data-stu-id="986d3-181">Click Manage, and try logging in again to the database.</span></span> <span data-ttu-id="986d3-182">Sie müssen möglicherweise einige Minuten, bevor die zulässigen IP-Adressen für die Firewall ordnungsgemäß konfiguriert sind.</span><span class="sxs-lookup"><span data-stu-id="986d3-182">You may need to wait a few minutes before the allowed IP addresses are correctly configured for the firewall.</span></span> <span data-ttu-id="986d3-183">Wenn Sie in der Datenbank anmelden können, haben Sie die Verbindung mit der Datenbank eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="986d3-183">When you can successfully log in the database, you have finished setting up your connection to the database.</span></span>

<span data-ttu-id="986d3-184">Sie können dieses Fenster geöffnet lassen, da Sie das Ergebnis der Bereitstellung der Datenbank in Kürze wird geprüft.</span><span class="sxs-lookup"><span data-stu-id="986d3-184">You can leave this management window open because you will check the result of your database deployment shortly.</span></span>

<span data-ttu-id="986d3-185">Dem Datenbankprojekt zurück.</span><span class="sxs-lookup"><span data-stu-id="986d3-185">Return to your database project.</span></span> <span data-ttu-id="986d3-186">Mit der rechten Maustaste des Projekts, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="986d3-186">Right-click the project and select **Publish**.</span></span>

![Veröffentlichen der Datenbank](publish-to-azure/_static/image16.png)

<span data-ttu-id="986d3-188">Wählen Sie im Fenster Datenbank veröffentlichen **bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="986d3-188">In the Publish Database window, select **Edit**.</span></span>

![bearbeiten](publish-to-azure/_static/image17.png)

<span data-ttu-id="986d3-190">Geben Sie den Namen des Datenbankservers und die Anmeldeinformationen für die Authentifizierung des Servers ein.</span><span class="sxs-lookup"><span data-stu-id="986d3-190">Provide the name of the database server and your authentication credentials for the server.</span></span> <span data-ttu-id="986d3-191">Wählen Sie nach der Bereitstellung der Anmeldeinformationen, die Sie aus der Liste der verfügbaren Datenbanken erstellte Datenbank.</span><span class="sxs-lookup"><span data-stu-id="986d3-191">After providing the credentials, select the database you created from the list of available databases.</span></span> <span data-ttu-id="986d3-192">Standardmäßig legt Visual Studio den Namen des Felds Datenbank auf den Namen des Projekts nicht identisch mit der Datenbank, die möglicherweise, den Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="986d3-192">By default, Visual Studio sets the name of the database field to the name of your project which might not be the same as the database you created.</span></span>

![](publish-to-azure/_static/image18.png)

<span data-ttu-id="986d3-193">Klicken Sie auf OK.</span><span class="sxs-lookup"><span data-stu-id="986d3-193">Click OK.</span></span>

<span data-ttu-id="986d3-194">Sie möchten wahrscheinlich dieses Profil zu speichern, damit Sie Updates in der Zukunft veröffentlichen können, ohne alle Verbindungsinformationen erneut eingeben zu müssen.</span><span class="sxs-lookup"><span data-stu-id="986d3-194">You will probably want to save this profile so you can publish updates in the future without re-entering all of the connection information.</span></span> <span data-ttu-id="986d3-195">Klicken Sie auf **Profil erstellen**.</span><span class="sxs-lookup"><span data-stu-id="986d3-195">Select **Create Profile**.</span></span>

![Profil speichern](publish-to-azure/_static/image19.png)

<span data-ttu-id="986d3-197">Es wird eine Datei erstellt, in Ihrem Projekt mit dem Namen **ContosoUniversityData.publish.xml**.</span><span class="sxs-lookup"><span data-stu-id="986d3-197">It will create a file in your project named **ContosoUniversityData.publish.xml**.</span></span> <span data-ttu-id="986d3-198">Laden Sie das nächste Mal mit das Sie die Datenbank in Azure veröffentlichen möchten einfach dieses Profil dürfen.</span><span class="sxs-lookup"><span data-stu-id="986d3-198">The next time you want to publish the database to Azure, simply load that profile.</span></span>

<span data-ttu-id="986d3-199">Klicken Sie nun auf **veröffentlichen** zum Erstellen der Datenbank in Azure.</span><span class="sxs-lookup"><span data-stu-id="986d3-199">Now, click **Publish** to create the database on Azure.</span></span>

![publish](publish-to-azure/_static/image20.png)

<span data-ttu-id="986d3-201">Nach der Ausführung für eine Weile, werden die publishing Ergebnisse angezeigt.</span><span class="sxs-lookup"><span data-stu-id="986d3-201">After running for a while, the publishing results are displayed.</span></span>

![results](publish-to-azure/_static/image21.png)

<span data-ttu-id="986d3-203">Jetzt können Sie zurück in das Verwaltungsportal für Ihre Datenbank wechseln.</span><span class="sxs-lookup"><span data-stu-id="986d3-203">Now, you can go back to the management portal for your database.</span></span> <span data-ttu-id="986d3-204">Aktualisieren Sie die Entwurfsansicht, und beachten Sie, dass die 3 Tabellen mit vorab ausgefüllten Daten bereitgestellt wurden.</span><span class="sxs-lookup"><span data-stu-id="986d3-204">Refresh the design view, and notice the 3 tables with pre-filled data have been deployed.</span></span>

![neue Tabellen](publish-to-azure/_static/image22.png)

<span data-ttu-id="986d3-206">Jetzt sind Sie bereit sind, die Web-app testen, die in Azure bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="986d3-206">Now you are ready to test the web app that is deployed to Azure.</span></span> <span data-ttu-id="986d3-207">Navigieren Sie zu der Web-app in Azure (z. B. http://contosositeexample.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="986d3-207">Navigate to the web app on Azure (such as http://contosositeexample.azurewebsites.net/).</span></span> <span data-ttu-id="986d3-208">Klicken Sie auf den Link, um die Liste der Schüler und die Indexansicht für Studenten sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="986d3-208">Click the link for List of students and you should see the index view for students.</span></span>

![Sicht](publish-to-azure/_static/image23.png)

<span data-ttu-id="986d3-210">Es kann vorkommen, benötigen die Datenbank und die Verbindung eine gewisse Zeit ordnungsgemäß konfiguriert sein.</span><span class="sxs-lookup"><span data-stu-id="986d3-210">Occasionally, the database and connection need a little time to be properly configured.</span></span> <span data-ttu-id="986d3-211">Wenn Sie eine Fehlermeldung erhalten, warten Sie einige Minuten, und wiederholen Sie dann erneut.</span><span class="sxs-lookup"><span data-stu-id="986d3-211">If you receive an error, wait a few minutes and then try again.</span></span>

## <a name="conclusion"></a><span data-ttu-id="986d3-212">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="986d3-212">Conclusion</span></span>

<span data-ttu-id="986d3-213">Diese Reihe bereitgestellt, ein einfaches Beispiel zum Generieren von Code aus einer vorhandenen Datenbank, die Benutzern das Bearbeiten, aktualisieren, erstellen und Löschen von Daten ermöglicht.</span><span class="sxs-lookup"><span data-stu-id="986d3-213">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="986d3-214">Sie verwendet ASP.NET MVC 5, Entity Framework und ASP.NET Gerüstbau, um das Projekt zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="986d3-214">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span>

<span data-ttu-id="986d3-215">Einführende beispielsweise der Code First-Entwicklung finden Sie unter [erste Schritte mit ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="986d3-215">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span>

<span data-ttu-id="986d3-216">Ein komplexeres Beispiel finden Sie unter [Erstellen eines Entity Framework-Datenmodells für eine ASP.NET MVC 4-App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="986d3-216">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="986d3-217">Beachten Sie, dass der DbContext-API, mit denen Sie für die Arbeit mit Daten in Database First identisch mit der API Sie zum Arbeiten mit Daten in Code First verwenden.</span><span class="sxs-lookup"><span data-stu-id="986d3-217">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="986d3-218">Selbst wenn Sie Database First verwenden möchten, können Sie erfahren, wie komplexere Szenarien, z. B. Lesen und Aktualisieren von verknüpften Daten zu behandeln Parallelitätskonflikte usw. aus einem Code First-Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="986d3-218">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="986d3-219">Der einzige Unterschied besteht in der Datenbank, der Context-Klasse und der Entitätsklassen wie erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="986d3-219">The only difference is in how the database, context class, and entity classes are created.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="986d3-220">Vorherige</span><span class="sxs-lookup"><span data-stu-id="986d3-220">Previous</span></span>](enhancing-data-validation.md)
