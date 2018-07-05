---
uid: mvc/overview/getting-started/database-first-development/publish-to-azure
title: Veröffentlichen von MVC Database First-Website in Azure | Microsoft-Dokumentation
author: tfitzmac
description: Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt. Dieses Tutorial Seri...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/22/2014
ms.topic: article
ms.assetid: 7131f1c1-cef3-4396-ab44-ed4519676546
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/database-first-development/publish-to-azure
msc.type: authoredcontent
ms.openlocfilehash: 5189d8ee92c6abac31d80ca4efdb06500e72126a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387269"
---
<a name="publish-mvc-database-first-site-to-azure"></a><span data-ttu-id="e08f2-104">Veröffentlichen Sie MVC Database First-Website in Azure.</span><span class="sxs-lookup"><span data-stu-id="e08f2-104">Publish MVC Database First site to Azure</span></span>
====================
<span data-ttu-id="e08f2-105">durch [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e08f2-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e08f2-106">Verwenden MVC, Entity Framework und ASP.NET-Gerüstbau, können Sie eine Webanwendung erstellen, die eine Schnittstelle für eine vorhandene Datenbank bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="e08f2-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="e08f2-107">Dieser tutorialreihe erfahren Sie, wie Sie automatisch generierter Code, der ermöglicht Benutzern das anzeigen, bearbeiten, erstellen und Löschen von Daten, die in einer Datenbanktabelle gespeichert.</span><span class="sxs-lookup"><span data-stu-id="e08f2-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="e08f2-108">Der generierte Code entspricht die Spalten in der Datenbanktabelle.</span><span class="sxs-lookup"><span data-stu-id="e08f2-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="e08f2-109">Dieser Teil der Serie konzentriert sich auf die WebApp und Datenbank in Azure veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-109">This part of the series focuses on publishing the web app and database to Azure.</span></span> <span data-ttu-id="e08f2-110">Sie können in diesem Thema erfahren Sie mehr über das Veröffentlichen einer WebApp und Datenbank, sondern tatsächlich der Schritte, die Sie am Anfang des Tutorials starten müssen, lesen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-110">You can read this topic to learn about publishing a web app and database, but to actually perform the steps you must start at the beginning of the tutorial.</span></span> <span data-ttu-id="e08f2-111">Finden Sie unter [Einstieg](setting-up-database.md).</span><span class="sxs-lookup"><span data-stu-id="e08f2-111">See [Getting Started](setting-up-database.md).</span></span>


## <a name="deploy-your-web-app-on-azure"></a><span data-ttu-id="e08f2-112">Bereitstellen Sie Ihrer Web-app in Azure</span><span class="sxs-lookup"><span data-stu-id="e08f2-112">Deploy your web app on Azure</span></span>

<span data-ttu-id="e08f2-113">Sie benötigen ein Azure-Konto zum Durchführen dieses Tutorials:</span><span class="sxs-lookup"><span data-stu-id="e08f2-113">You need an Azure account to complete this tutorial:</span></span>

- <span data-ttu-id="e08f2-114">Sie können [öffnen Sie ein Azure-Konto kostenlos](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) – Sie erhalten ein Guthaben zum Ausprobieren Zahlungspflichtiger Azure-Dienste nutzen können, und auch nach dem sie verwendet werden, bis können Sie das Konto behalten und kostenlose Azure-Dienste.</span><span class="sxs-lookup"><span data-stu-id="e08f2-114">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="e08f2-115">Sie können [MSDN-abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) -Ihr MSDN-Abonnement ein monatliches Guthaben, die Sie für zahlungspflichtige Azure-Dienste verwenden können.</span><span class="sxs-lookup"><span data-stu-id="e08f2-115">You can [activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

<span data-ttu-id="e08f2-116">Klicken Sie zum Veröffentlichen Ihrer Web-app mit der rechten Maustaste in des Projekts, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="e08f2-116">To publish your web app, right-click the project and select **Publish**.</span></span>

![Website veröffentlichen](publish-to-azure/_static/image1.png)

<span data-ttu-id="e08f2-118">Wählen Sie die Microsoft Azure-Websites.</span><span class="sxs-lookup"><span data-stu-id="e08f2-118">Select Microsoft Azure Websites.</span></span>

![Auswählen von Azure](publish-to-azure/_static/image2.png)

<span data-ttu-id="e08f2-120">Wenn Sie nicht bei Azure angemeldet sind, geben Sie Ihre Azure-Konto-Anmeldeinformationen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-120">If you are not signed in to Azure, provide your Azure account credentials.</span></span> <span data-ttu-id="e08f2-121">Wählen Sie dann neu, um eine neue Web-app zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-121">Then, select New to create a new web app.</span></span>

![neue Website](publish-to-azure/_static/image3.png)

<span data-ttu-id="e08f2-123">Erstellen Sie einen eindeutigen Namen für Ihre Web-app.</span><span class="sxs-lookup"><span data-stu-id="e08f2-123">Create a unique name for your web app.</span></span> <span data-ttu-id="e08f2-124">Lernen Sie, dass der Name eindeutig ist. Wenn Sie rechts neben dem Namensfeld ein grünes Häkchen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e08f2-124">You will know the name is unique if you see a green check mark to the right of the name field.</span></span> <span data-ttu-id="e08f2-125">Wählen Sie eine Region für Ihre Web-app.</span><span class="sxs-lookup"><span data-stu-id="e08f2-125">Select a region for your web app.</span></span> <span data-ttu-id="e08f2-126">Wählen Sie **neuen Server erstellen** für die Datenbank, und geben Sie einen Benutzernamen und ein Kennwort für diesen neuen Datenbankserver.</span><span class="sxs-lookup"><span data-stu-id="e08f2-126">Select **Create new server** for the database, and provide a user name and password for this new database server.</span></span> <span data-ttu-id="e08f2-127">Klicken Sie abschließend auf **erstellen**.</span><span class="sxs-lookup"><span data-stu-id="e08f2-127">When finished, click **Create**.</span></span>

![Website erstellen](publish-to-azure/_static/image4.png)

<span data-ttu-id="e08f2-129">Die Werte für Ihre Verbindung sind nun festgelegt.</span><span class="sxs-lookup"><span data-stu-id="e08f2-129">Your connection values are now all set.</span></span> <span data-ttu-id="e08f2-130">Sie können diese Werte unverändert lassen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-130">You can leave these values unchanged.</span></span>

![connection](publish-to-azure/_static/image5.png)

<span data-ttu-id="e08f2-132">Klicken Sie auf **Weiter**.</span><span class="sxs-lookup"><span data-stu-id="e08f2-132">Click **Next**.</span></span>

<span data-ttu-id="e08f2-133">Beachten Sie, dass zwei Datenbank-Verbindungen für die Einstellungen werden angegeben - ApplicationDBContext und ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="e08f2-133">For Settings, notice that two database connections are specified - ApplicationDBContext and ContosoUniversityDataEntities.</span></span> <span data-ttu-id="e08f2-134">ApplicationDBContext ist die Verbindung für Benutzer-Tabellen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-134">ApplicationDBContext is the connection for user account tables.</span></span> <span data-ttu-id="e08f2-135">Diese Werte zeigen nur die Verbindungszeichenfolgen für die Datenbanken an.</span><span class="sxs-lookup"><span data-stu-id="e08f2-135">These values only show the connection strings for the databases.</span></span> <span data-ttu-id="e08f2-136">Es bedeutet nicht, dass diese Datenbanken veröffentlicht werden, wenn Sie Ihre Website zu veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-136">It does not mean that these databases will be published when you publish your site.</span></span> <span data-ttu-id="e08f2-137">Veröffentlichen Sie Ihr Datenbankprojekt nach dem Veröffentlichen der Web-app.</span><span class="sxs-lookup"><span data-stu-id="e08f2-137">You will publish your database project after you have finished publishing the web app.</span></span>

![](publish-to-azure/_static/image6.png)

<span data-ttu-id="e08f2-138">Die Auslassungspunkte (...) neben der Verbindung mit der zeigt Sie die Details der Verbindungszeichenfolge.</span><span class="sxs-lookup"><span data-stu-id="e08f2-138">The ellipsis (...) next to the database connection shows you the details of the connection string.</span></span> <span data-ttu-id="e08f2-139">Klicken Sie auf die Auslassungspunkte neben ContosoUniversityDataEntities.</span><span class="sxs-lookup"><span data-stu-id="e08f2-139">Click the ellipsis next to ContosoUniversityDataEntities.</span></span>

![](publish-to-azure/_static/image7.png)

<span data-ttu-id="e08f2-140">Notieren Sie den Namen des Datenbankservers und der Datenbank.</span><span class="sxs-lookup"><span data-stu-id="e08f2-140">Note the name of the database server and the database.</span></span> <span data-ttu-id="e08f2-141">Den Namen des Servers wird nach dem Zufallsprinzip generiert.</span><span class="sxs-lookup"><span data-stu-id="e08f2-141">The server name is randomly generated.</span></span> <span data-ttu-id="e08f2-142">Der Name der Datenbank ist einfach der Name Ihrer Website mit  **\_Db** angefügt.</span><span class="sxs-lookup"><span data-stu-id="e08f2-142">The database name is simple the name of your site with **\_db** appended.</span></span> <span data-ttu-id="e08f2-143">Sie benötigen beide Namen später, wenn Sie Ihre Datenbank veröffentlichen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-143">You will need both names later when you publish your database.</span></span>

<span data-ttu-id="e08f2-144">Klicken Sie auf **OK** um die Datenbank-Verbindung Zeichenfolge-Fenster zu schließen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-144">Click **OK** to close the database connection string window.</span></span>

<span data-ttu-id="e08f2-145">Klicken Sie in das Fenster "Web veröffentlichen" auf **Weiter** um die Vorschau anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-145">In the Publish Web window, click **Next** to see the preview.</span></span>

![](publish-to-azure/_static/image8.png)

<span data-ttu-id="e08f2-146">Sie können die Vorschau starten, um eine Liste der Dateien zum Veröffentlichen finden Sie unter klicken.</span><span class="sxs-lookup"><span data-stu-id="e08f2-146">You can click Start Preview to see a list of the files to publish.</span></span> <span data-ttu-id="e08f2-147">Da dies beim ersten Sie auf dieser Website veröffentlicht haben ist, ist die Liste alle relevanten Datei im Projekt.</span><span class="sxs-lookup"><span data-stu-id="e08f2-147">Since this is the first time you have published this site, the list is every relevant file in the project.</span></span>

<span data-ttu-id="e08f2-148">Klicken Sie auf **Veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="e08f2-148">Click **Publish**.</span></span>

<span data-ttu-id="e08f2-149">Der Ausgabebereich zeigt das Ergebnis der Veröffentlichung.</span><span class="sxs-lookup"><span data-stu-id="e08f2-149">The Output pane will display the result of your publication.</span></span>

![](publish-to-azure/_static/image9.png)

<span data-ttu-id="e08f2-150">Nach der Veröffentlichung ist die Website Immedialely in einem Webbrowser gestartet.</span><span class="sxs-lookup"><span data-stu-id="e08f2-150">After publication, the site is immedialely launched in a web browser.</span></span> <span data-ttu-id="e08f2-151">Ihre Website bereitgestellt wurde, und Sie können einen neuen Benutzer auf die Website registrieren; Allerdings haben die Tabellen im Projekt ContosoUniversityData noch nicht veröffentlicht wurde.</span><span class="sxs-lookup"><span data-stu-id="e08f2-151">Your site has been deployed and you can register a new user to the site; however, your tables in the ContosoUniversityData project have not yet been published.</span></span> <span data-ttu-id="e08f2-152">Wenn Sie auf die Liste der Schüler/Studenten-Link klicken, erhalten Sie einen Fehler.</span><span class="sxs-lookup"><span data-stu-id="e08f2-152">If you click on the List of students link you will receive an error.</span></span>

## <a name="publish-database-to-sql-azure"></a><span data-ttu-id="e08f2-153">Veröffentlichen Sie die Datenbank zu SQL Azure</span><span class="sxs-lookup"><span data-stu-id="e08f2-153">Publish database to SQL Azure</span></span>

<span data-ttu-id="e08f2-154">Vor der Veröffentlichung der Datenbank, müssen Sie sicherstellen, dass es sich bei Ihrem lokale Computer eine Verbindung mit dem Datenbankserver herstellen kann.</span><span class="sxs-lookup"><span data-stu-id="e08f2-154">Before publishing your database, you must make sure your local computer can connect to the database server.</span></span> <span data-ttu-id="e08f2-155">Die Firewall für Ihren Datenbankserver schränkt die Computer mit der Datenbank herstellen können.</span><span class="sxs-lookup"><span data-stu-id="e08f2-155">The firewall for your database server restricts which machines can connect to the database.</span></span> <span data-ttu-id="e08f2-156">Sie müssen die IP-Adresse des Computers den zulässigen IP-Adressen für die Firewall hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-156">You need to add the IP address of your computer to the allowed IP addresses for the firewall.</span></span>

<span data-ttu-id="e08f2-157">Melden Sie sich beim Azure-Konto über das Azure-Portal.</span><span class="sxs-lookup"><span data-stu-id="e08f2-157">Login to your Azure account through the Azure portal.</span></span>

<span data-ttu-id="e08f2-158">Wählen Sie Ihre neue Datenbank und **verwalten**.</span><span class="sxs-lookup"><span data-stu-id="e08f2-158">Select your new database and select **Manage**.</span></span>

![Verwalten von](publish-to-azure/_static/image10.png)

<span data-ttu-id="e08f2-160">Sie müssen Ihren Datenbankserver, um Verbindungen von Ihrem Computer zu ermöglichen, konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e08f2-160">You must configure your database server to allow connections from your computer.</span></span> <span data-ttu-id="e08f2-161">Wenn Sie auf "verwalten" auswählen, werden Sie aufgefordert, um die aktuelle IP-Adresse hinzuzufügen, wie mit dem Datenbankserver her.</span><span class="sxs-lookup"><span data-stu-id="e08f2-161">When you select Manage, you are asked to add the current IP address as permitted to the database server.</span></span> <span data-ttu-id="e08f2-162">Wählen Sie Ja.</span><span class="sxs-lookup"><span data-stu-id="e08f2-162">Select Yes.</span></span>

![IP-Adresse hinzufügen](publish-to-azure/_static/image11.png)

<span data-ttu-id="e08f2-164">Es besteht die Möglichkeit, die die IP-Adresse, die Sie im vorherigen Schritt hinzugefügt haben nicht die einzige IP-Adresse ist, die Sie für Verbindungen zu konfigurieren müssen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-164">There is a chance that the IP address you added in the previous step is not the only IP address you need to configure for connections.</span></span> <span data-ttu-id="e08f2-165">Sie können versuchen, an die Datenbank aus, um festzustellen, ob die Verbindungen ordnungsgemäß eingerichtet wurden.</span><span class="sxs-lookup"><span data-stu-id="e08f2-165">You can attempt to login to the database to see if the connections have been properly set up.</span></span> <span data-ttu-id="e08f2-166">Geben Sie dem Benutzernamen und Kennwort, die Sie zuvor erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="e08f2-166">Provide the user and password you created earlier.</span></span>

![Anmeldung](publish-to-azure/_static/image12.png)

<span data-ttu-id="e08f2-168">Wenn Sie eine Fehlermeldung erhalten, müssen Sie eine andere IP-Adresse hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-168">If you receive an error message, you need to add another IP address.</span></span> <span data-ttu-id="e08f2-169">Klicken Sie auf die Fehlermeldung, um weitere Details zum Fehler anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-169">Click the error message to see more details about error.</span></span> <span data-ttu-id="e08f2-170">In den Details sehen Sie die IP-Adresse, die Sie hinzufügen müssen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-170">In the details you will see the IP address that you need to add.</span></span> <span data-ttu-id="e08f2-171">Beachten Sie diese IP-Adresse ein.</span><span class="sxs-lookup"><span data-stu-id="e08f2-171">Note this IP address.</span></span>

![nicht zulässig.](publish-to-azure/_static/image13.png)

<span data-ttu-id="e08f2-173">Schließen Sie dieses Fenster für die Anmeldung, und kehren Sie zum Azure-Portal zurück.</span><span class="sxs-lookup"><span data-stu-id="e08f2-173">Close this login window, and return to the Azure portal.</span></span>

<span data-ttu-id="e08f2-174">Navigieren Sie zum Dashboard für Ihre Datenbank.</span><span class="sxs-lookup"><span data-stu-id="e08f2-174">Navigate to the Dashboard for your database.</span></span> <span data-ttu-id="e08f2-175">Klicken Sie auf **zulässige IP-Adressen verwalten**.</span><span class="sxs-lookup"><span data-stu-id="e08f2-175">Click **Manage allowed IP addresses**.</span></span>

![IP-Adressen verwalten](publish-to-azure/_static/image14.png)

<span data-ttu-id="e08f2-177">Sie müssen nun die IP-Adresse aus der Fehlermeldung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-177">You must now add the IP address from the error message.</span></span> <span data-ttu-id="e08f2-178">Ändern Sie den Bereich der zulässigen IP-Adressen, die aus der Fehlermeldung enthält, oder fügen Sie die IP-Adresse als einen separaten Eintrag hinzu.</span><span class="sxs-lookup"><span data-stu-id="e08f2-178">Either change the range of allowed IP addresses to include the one from the error message, or add that IP address as a separate entry.</span></span>

![neue Adresse hinzufügen](publish-to-azure/_static/image15.png)

<span data-ttu-id="e08f2-180">Speichern Sie die Änderung in zulässige IP-Adressen ein.</span><span class="sxs-lookup"><span data-stu-id="e08f2-180">Save the change to allowed IP addresses.</span></span>

<span data-ttu-id="e08f2-181">Klicken Sie auf verwalten, und versuchen Sie erneut mit der Datenbank anzumelden.</span><span class="sxs-lookup"><span data-stu-id="e08f2-181">Click Manage, and try logging in again to the database.</span></span> <span data-ttu-id="e08f2-182">Möglicherweise müssen Sie warten einige Minuten, bevor die zulässigen IP-Adressen für die Firewall ordnungsgemäß konfiguriert sind.</span><span class="sxs-lookup"><span data-stu-id="e08f2-182">You may need to wait a few minutes before the allowed IP addresses are correctly configured for the firewall.</span></span> <span data-ttu-id="e08f2-183">Wenn Sie in der Datenbank anmelden können, müssen Sie die Verbindung mit der Datenbank eingerichtet.</span><span class="sxs-lookup"><span data-stu-id="e08f2-183">When you can successfully log in the database, you have finished setting up your connection to the database.</span></span>

<span data-ttu-id="e08f2-184">Sie können dieses Fenster Management geöffnet lassen, da Sie das Ergebnis der Bereitstellung der Datenbank in Kürze überprüft werden.</span><span class="sxs-lookup"><span data-stu-id="e08f2-184">You can leave this management window open because you will check the result of your database deployment shortly.</span></span>

<span data-ttu-id="e08f2-185">Geben Sie dem Datenbankprojekt zurück.</span><span class="sxs-lookup"><span data-stu-id="e08f2-185">Return to your database project.</span></span> <span data-ttu-id="e08f2-186">Mit der rechten Maustaste in des Projekts, und wählen Sie **veröffentlichen**.</span><span class="sxs-lookup"><span data-stu-id="e08f2-186">Right-click the project and select **Publish**.</span></span>

![Veröffentlichen der Datenbank](publish-to-azure/_static/image16.png)

<span data-ttu-id="e08f2-188">Wählen Sie im Fenster Datenbank veröffentlichen **bearbeiten**.</span><span class="sxs-lookup"><span data-stu-id="e08f2-188">In the Publish Database window, select **Edit**.</span></span>

![bearbeiten](publish-to-azure/_static/image17.png)

<span data-ttu-id="e08f2-190">Geben Sie den Namen des Datenbankservers und die Anmeldeinformationen für die Authentifizierung für den Server ein.</span><span class="sxs-lookup"><span data-stu-id="e08f2-190">Provide the name of the database server and your authentication credentials for the server.</span></span> <span data-ttu-id="e08f2-191">Wählen Sie die Datenbank, die Sie aus der Liste der verfügbaren Datenbanken erstellt haben, nach der Eingabe der Anmeldeinformationen.</span><span class="sxs-lookup"><span data-stu-id="e08f2-191">After providing the credentials, select the database you created from the list of available databases.</span></span> <span data-ttu-id="e08f2-192">Standardmäßig legt Visual Studio den Namen der Datenbank-Felds, auf den Namen des Projekts, die nicht identisch mit der Datenbank möglicherweise, die Sie erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="e08f2-192">By default, Visual Studio sets the name of the database field to the name of your project which might not be the same as the database you created.</span></span>

![](publish-to-azure/_static/image18.png)

<span data-ttu-id="e08f2-193">Klicken Sie auf OK.</span><span class="sxs-lookup"><span data-stu-id="e08f2-193">Click OK.</span></span>

<span data-ttu-id="e08f2-194">Wahrscheinlich möchten dieses Profil zu speichern, damit Sie Updates in der Zukunft veröffentlichen können, ohne Sie alle Verbindungsinformationen erneut eingeben.</span><span class="sxs-lookup"><span data-stu-id="e08f2-194">You will probably want to save this profile so you can publish updates in the future without re-entering all of the connection information.</span></span> <span data-ttu-id="e08f2-195">Klicken Sie auf **Profil erstellen**.</span><span class="sxs-lookup"><span data-stu-id="e08f2-195">Select **Create Profile**.</span></span>

![Profil speichern](publish-to-azure/_static/image19.png)

<span data-ttu-id="e08f2-197">Es erstellt eine Datei in Ihrem Projekt mit dem Namen **ContosoUniversityData.publish.xml**.</span><span class="sxs-lookup"><span data-stu-id="e08f2-197">It will create a file in your project named **ContosoUniversityData.publish.xml**.</span></span> <span data-ttu-id="e08f2-198">Laden Sie das nächste Mal mit das Sie die Datenbank in Azure veröffentlichen möchten einfach dieses Profil.</span><span class="sxs-lookup"><span data-stu-id="e08f2-198">The next time you want to publish the database to Azure, simply load that profile.</span></span>

<span data-ttu-id="e08f2-199">Klicken Sie nun **veröffentlichen** zum Erstellen der Datenbank in Azure.</span><span class="sxs-lookup"><span data-stu-id="e08f2-199">Now, click **Publish** to create the database on Azure.</span></span>

![publish](publish-to-azure/_static/image20.png)

<span data-ttu-id="e08f2-201">Nach der Ausführung eine Weile werden die Veröffentlichen von Ergebnissen angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e08f2-201">After running for a while, the publishing results are displayed.</span></span>

![results](publish-to-azure/_static/image21.png)

<span data-ttu-id="e08f2-203">Jetzt können Sie in das Verwaltungsportal für Ihre Datenbank zurückkehren.</span><span class="sxs-lookup"><span data-stu-id="e08f2-203">Now, you can go back to the management portal for your database.</span></span> <span data-ttu-id="e08f2-204">Aktualisieren Sie die Entwurfsansicht zu sehen, und beachten Sie, dass die 3 Tabellen mit vorab ausgefüllten Daten bereitgestellt wurden.</span><span class="sxs-lookup"><span data-stu-id="e08f2-204">Refresh the design view, and notice the 3 tables with pre-filled data have been deployed.</span></span>

![neue Tabellen](publish-to-azure/_static/image22.png)

<span data-ttu-id="e08f2-206">Jetzt können Sie sich zum Testen der Web-app, die in Azure bereitgestellt wird.</span><span class="sxs-lookup"><span data-stu-id="e08f2-206">Now you are ready to test the web app that is deployed to Azure.</span></span> <span data-ttu-id="e08f2-207">Navigieren Sie zu der Web-app in Azure (z. B. http://contosositeexample.azurewebsites.net/).</span><span class="sxs-lookup"><span data-stu-id="e08f2-207">Navigate to the web app on Azure (such as http://contosositeexample.azurewebsites.net/).</span></span> <span data-ttu-id="e08f2-208">Klicken Sie auf den Link, um die Liste der Schüler/Studenten und die Indexansicht für Schüler/Studenten wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e08f2-208">Click the link for List of students and you should see the index view for students.</span></span>

![Sicht](publish-to-azure/_static/image23.png)

<span data-ttu-id="e08f2-210">In einigen Fällen benötigen in die Datenbank und die Verbindung etwas Zeit, um ordnungsgemäß konfiguriert werden.</span><span class="sxs-lookup"><span data-stu-id="e08f2-210">Occasionally, the database and connection need a little time to be properly configured.</span></span> <span data-ttu-id="e08f2-211">Wenn Sie eine Fehlermeldung erhalten, warten Sie einige Minuten, und versuchen Sie es dann erneut.</span><span class="sxs-lookup"><span data-stu-id="e08f2-211">If you receive an error, wait a few minutes and then try again.</span></span>

## <a name="conclusion"></a><span data-ttu-id="e08f2-212">Schlussbemerkung</span><span class="sxs-lookup"><span data-stu-id="e08f2-212">Conclusion</span></span>

<span data-ttu-id="e08f2-213">Diese Reihe bereitgestellt, ein einfaches Beispiel dafür, wie zum Generieren von Code aus einer vorhandenen Datenbank, in dem Benutzer bearbeiten, aktualisieren, erstellen und Löschen von Daten.</span><span class="sxs-lookup"><span data-stu-id="e08f2-213">This series provided a simple example of how to generate code from an existing database that enables users to edit, update, create and delete data.</span></span> <span data-ttu-id="e08f2-214">Er verwendet ASP.NET MVC 5, Entity Framework und ASP.NET Scaffolding zum Erstellen des Projekts.</span><span class="sxs-lookup"><span data-stu-id="e08f2-214">It used ASP.NET MVC 5, Entity Framework and ASP.NET Scaffolding to create the project.</span></span>

<span data-ttu-id="e08f2-215">Ein einführendes Beispiel Code First-Entwicklung, finden Sie unter [erste Schritte mit ASP.NET MVC 5](../introduction/getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="e08f2-215">For an introductory example of Code First development, see [Getting Started with ASP.NET MVC 5](../introduction/getting-started.md).</span></span>

<span data-ttu-id="e08f2-216">Ein komplexeres Beispiel, finden Sie unter [erstellen ein Entity Framework-Datenmodells für eine ASP.NET MVC 4-App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="e08f2-216">For a more advanced example, see [Creating an Entity Framework Data Model for an ASP.NET MVC 4 App](../getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="e08f2-217">Beachten Sie, dass die DbContext-API, mit denen Sie für die Arbeit mit Daten in der ersten Datenbank identisch mit der API Sie verwenden für die Arbeit mit Daten in Code First.</span><span class="sxs-lookup"><span data-stu-id="e08f2-217">Note that the DbContext API that you use for working with data in Database First is the same as the API you use for working with data in Code First.</span></span> <span data-ttu-id="e08f2-218">Auch wenn Sie Database First verwenden möchten, erhalten Sie wie Sie die Verarbeitung komplexerer Szenarien wie z. B. das Lesen und aktualisieren die zugehörige Daten, Behandlung von nebenläufigkeitskonflikten führen, und so weiter aus einem Code First-Lernprogramm.</span><span class="sxs-lookup"><span data-stu-id="e08f2-218">Even if you intend to use Database First, you can learn how to handle more complex scenarios such as reading and updating related data, handling concurrency conflicts, and so forth from a Code First tutorial.</span></span> <span data-ttu-id="e08f2-219">Der einzige Unterschied besteht im wie die Datenbank, die Context-Klasse und die Entitätsklassen erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="e08f2-219">The only difference is in how the database, context class, and entity classes are created.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e08f2-220">Vorherige</span><span class="sxs-lookup"><span data-stu-id="e08f2-220">Previous</span></span>](enhancing-data-validation.md)
