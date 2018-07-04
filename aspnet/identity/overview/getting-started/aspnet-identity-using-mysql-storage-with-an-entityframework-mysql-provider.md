---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: Verwenden von MySQL-Speicher mit einem EntityFramework-MySQL-Anbieter (c#) | Microsoft-Dokumentation'
author: maumar
description: In diesem Tutorial erfahren Sie, wie Sie den Standardmechanismus für die Speicherung von Daten für ASP.NET Identity mit EntityFramework (SQL-Client-Anbieter) mit einer MySQL-installierten hardwarelösungen ersetzen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: ''
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 6b1d57c65cb4197d1b20175415ee73b3e81cb53f
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37383038"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a><span data-ttu-id="0676f-103">ASP.NET Identity: Verwenden von MySQL-Speicher mit einem EntityFramework-MySQL-Anbieter (c#)</span><span class="sxs-lookup"><span data-stu-id="0676f-103">ASP.NET Identity: Using MySQL Storage with an EntityFramework MySQL Provider (C#)</span></span>
====================
<span data-ttu-id="0676f-104">durch [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="0676f-104">by [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span></span>

> <span data-ttu-id="0676f-105">In diesem Tutorial erfahren Sie, wie den Standardmechanismus für die Speicherung von Daten für ersetzen [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) mit EntityFramework (SQL-Client-Anbieter) mit einem MySQL-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="0676f-105">This tutorial shows you how to replace the default data storage mechanism for [**ASP.NET Identity**](introduction-to-aspnet-identity.md) with EntityFramework (SQL client provider) with a MySQL provider.</span></span>


<span data-ttu-id="0676f-106">In diesem Tutorial werden die folgenden Themen behandelt:</span><span class="sxs-lookup"><span data-stu-id="0676f-106">The following topics will be covered in this tutorial:</span></span>

- <span data-ttu-id="0676f-107">Erstellen eine MySQL-Datenbank in Azure</span><span class="sxs-lookup"><span data-stu-id="0676f-107">Creating a MySQL database on Azure</span></span>
- <span data-ttu-id="0676f-108">Erstellen einer MVC-Anwendung, die mithilfe von Visual Studio 2013-MVC-Vorlage</span><span class="sxs-lookup"><span data-stu-id="0676f-108">Creating an MVC application using Visual Studio 2013 MVC template</span></span>
- <span data-ttu-id="0676f-109">Konfigurieren von EntityFramework zum Arbeiten mit einem MySQL-Datenbankanbieter</span><span class="sxs-lookup"><span data-stu-id="0676f-109">Configuring EntityFramework to work with a MySQL database provider</span></span>
- <span data-ttu-id="0676f-110">Ausführen der Anwendung die Ergebnisse überprüfen</span><span class="sxs-lookup"><span data-stu-id="0676f-110">Running the application to verify the results</span></span>

<span data-ttu-id="0676f-111">Am Ende dieses Tutorials müssen Sie eine MVC-Anwendung mit der ASP.NET Identity zu speichern, die eine MySQL-Datenbank verwendet wird, die in Azure gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="0676f-111">At the end of this tutorial, you will have an MVC application with the ASP.NET Identity store that is using a MySQL database that is hosted in Azure.</span></span>

## <a name="creating-a-mysql-database-instance-on-azure"></a><span data-ttu-id="0676f-112">Erstellen eine MySQL-Datenbankinstanz in Azure</span><span class="sxs-lookup"><span data-stu-id="0676f-112">Creating a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="0676f-113">Melden Sie sich beim [Azure-Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409) an.</span><span class="sxs-lookup"><span data-stu-id="0676f-113">Log in to the [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span></span>
2. <span data-ttu-id="0676f-114">Klicken Sie auf **neu** am unteren Rand der Seite, und wählen Sie dann **STORE**:</span><span class="sxs-lookup"><span data-stu-id="0676f-114">Click **NEW** at the bottom of the page, and then select **STORE**:</span></span>  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. <span data-ttu-id="0676f-115">In der **auswählen und der Add-On-** Assistenten **ClearDB MySQL-Datenbank**, und klicken Sie dann auf die **Weiter** Pfeil am unteren Rand der Frame:</span><span class="sxs-lookup"><span data-stu-id="0676f-115">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database**, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="0676f-116">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="0676f-116">[Click the following image to expand it.</span></span> <span data-ttu-id="0676f-117">]</span><span class="sxs-lookup"><span data-stu-id="0676f-117">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. <span data-ttu-id="0676f-118">Behalten Sie den Standardwert **Free** planen, ändern Sie die **Namen** zu **IdentityMySQLDatabase**, wählen Sie die Region, die Ihnen am nächsten liegt, und klicken Sie dann auf die **Weiter** Pfeil am unteren Rand der Frame:</span><span class="sxs-lookup"><span data-stu-id="0676f-118">Keep the default **Free** plan, change the **NAME** to **IdentityMySQLDatabase**, select the region that is nearest to you, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="0676f-119">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="0676f-119">[Click the following image to expand it.</span></span> <span data-ttu-id="0676f-120">]</span><span class="sxs-lookup"><span data-stu-id="0676f-120">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. <span data-ttu-id="0676f-121">Klicken Sie auf die **Kauf** Häkchen, um die Datenbankerstellung abzuschließen.</span><span class="sxs-lookup"><span data-stu-id="0676f-121">Click the **PURCHASE** checkmark to complete the database creation.</span></span>  
  
   <span data-ttu-id="0676f-122">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="0676f-122">[Click the following image to expand it.</span></span> <span data-ttu-id="0676f-123">]</span><span class="sxs-lookup"><span data-stu-id="0676f-123">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. <span data-ttu-id="0676f-124">Nachdem Ihre Datenbank erstellt wurde, können Sie verwalten, von der **-Add-Ons** Registerkarte im Verwaltungsportal.</span><span class="sxs-lookup"><span data-stu-id="0676f-124">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span> <span data-ttu-id="0676f-125">Klicken Sie zum Abrufen der Verbindungsinformationen für Ihre Datenbank auf **VERBINDUNGSINFORMATIONEN** am unteren Rand der Seite:</span><span class="sxs-lookup"><span data-stu-id="0676f-125">To retrieve the connection information for your database, click **CONNECTION INFO** at the bottom of the page:</span></span>  
  
   <span data-ttu-id="0676f-126">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="0676f-126">[Click the following image to expand it.</span></span> <span data-ttu-id="0676f-127">]</span><span class="sxs-lookup"><span data-stu-id="0676f-127">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. <span data-ttu-id="0676f-128">Kopieren Sie die Verbindungszeichenfolge durch Klicken auf die Schaltfläche "Kopieren", indem die **"ConnectionString"** ; ein, und speichern sie diese Informationen später in diesem Tutorial verwenden Sie für Ihre MVC-Anwendung:</span><span class="sxs-lookup"><span data-stu-id="0676f-128">Copy the connection string by clicking on the copy button by the **CONNECTIONSTRING** field and save it; you will use this information later in this tutorial for your MVC application:</span></span>  
  
   <span data-ttu-id="0676f-129">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="0676f-129">[Click the following image to expand it.</span></span> <span data-ttu-id="0676f-130">]</span><span class="sxs-lookup"><span data-stu-id="0676f-130">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a><span data-ttu-id="0676f-131">Erstellen einer MVC-Anwendungsprojekt</span><span class="sxs-lookup"><span data-stu-id="0676f-131">Creating an MVC application project</span></span>

<span data-ttu-id="0676f-132">Um die Schritte in diesem Abschnitt des Tutorials ausführen zu können, müssen Sie zuerst installieren [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="0676f-132">To complete the steps in this section of the tutorial, you will first need to install [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="0676f-133">Sobald Visual Studio installiert wurde, verwenden Sie die folgenden Schritte aus, um ein neues MVC-Anwendungsprojekt zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="0676f-133">Once Visual Studio has been installed, use the following steps to create a new MVC application project:</span></span>

1. <span data-ttu-id="0676f-134">Öffnen Sie Visual Studio 2103.</span><span class="sxs-lookup"><span data-stu-id="0676f-134">Open Visual Studio 2103.</span></span>
2. <span data-ttu-id="0676f-135">Klicken Sie auf **neues Projekt** aus der **starten** Seite, oder Sie klicken auf die **Datei** Menü und dann **neues Projekt**:</span><span class="sxs-lookup"><span data-stu-id="0676f-135">Click **New Project** from the **Start** page, or you can click the **File** menu and then **New Project**:</span></span>  
  
   <span data-ttu-id="0676f-136">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="0676f-136">[Click the following image to expand it.</span></span> <span data-ttu-id="0676f-137">]</span><span class="sxs-lookup"><span data-stu-id="0676f-137">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. <span data-ttu-id="0676f-138">Wenn die **neues Projekt** Dialogfeld wird angezeigt, erweitern Sie **Visual C#-** in der Liste der Vorlagen, klicken Sie dann auf **Web**, und wählen Sie **ASP.NET-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="0676f-138">When the **New Project** dialog box is displayed, expand **Visual C#** in the list of templates, then click **Web**, and select **ASP.NET Web Application**.</span></span> <span data-ttu-id="0676f-139">Benennen Sie Ihr Projekt **IdentityMySQLDemo** , und klicken Sie dann auf **OK**:</span><span class="sxs-lookup"><span data-stu-id="0676f-139">Name your project **IdentityMySQLDemo** and then click **OK**:</span></span>  
  
   <span data-ttu-id="0676f-140">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="0676f-140">[Click the following image to expand it.</span></span> <span data-ttu-id="0676f-141">]</span><span class="sxs-lookup"><span data-stu-id="0676f-141">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. <span data-ttu-id="0676f-142">In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **MVC** Templatewith Standardoptionen; dadurch konfigurieren **einzelne Benutzerkonten** als Authentifizierungsmethode.</span><span class="sxs-lookup"><span data-stu-id="0676f-142">In the **New ASP.NET Project** dialog, select the **MVC** templatewith the default options; this will configure **Individual User Accounts** as the authentication method.</span></span> <span data-ttu-id="0676f-143">Klicken Sie auf **OK**:</span><span class="sxs-lookup"><span data-stu-id="0676f-143">Click **OK**:</span></span>  
  
   <span data-ttu-id="0676f-144">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="0676f-144">[Click the following image to expand it.</span></span> <span data-ttu-id="0676f-145">]</span><span class="sxs-lookup"><span data-stu-id="0676f-145">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a><span data-ttu-id="0676f-146">Konfigurieren Sie EntityFramework, um die Arbeit mit einer MySQL-Datenbank</span><span class="sxs-lookup"><span data-stu-id="0676f-146">Configure EntityFramework to work with a MySQL database</span></span>

### <a name="update-the-entity-framework-assembly-for-your-project"></a><span data-ttu-id="0676f-147">Die Entity Framework-Assembly für das Projekt aktualisieren</span><span class="sxs-lookup"><span data-stu-id="0676f-147">Update the Entity Framework assembly for your project</span></span>

<span data-ttu-id="0676f-148">Die MVC-Anwendung, die aus der Visual Studio 2013-Vorlage erstellt wurde, enthält einen Verweis auf die [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) Verpacken, aber es wurden Updates auf die Assembly seit der Veröffentlichung enthalten wichtige leistungsverbesserungen.</span><span class="sxs-lookup"><span data-stu-id="0676f-148">The MVC application that was created from the Visual Studio 2013 template contains a reference to the [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) package, but there have been updates to that assembly since its release which contain significant performance improvements.</span></span> <span data-ttu-id="0676f-149">Um diese neueste Updates in Ihrer Anwendung verwenden möchten, verwenden Sie die folgenden Schritte aus.</span><span class="sxs-lookup"><span data-stu-id="0676f-149">In order to use these latest updates in your application, use the following steps.</span></span>

1. <span data-ttu-id="0676f-150">Öffnen Sie das MVC-Projekt in Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="0676f-150">Open your MVC project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="0676f-151">Klicken Sie auf **TOOLS**, klicken Sie dann auf **Bibliothekspaket-Manager**, und klicken Sie dann auf **-Paket-Manager-Konsole**:</span><span class="sxs-lookup"><span data-stu-id="0676f-151">Click **TOOLS**, then click **Library Package Manager**, and then click **Package Manager Console**:</span></span>  
  
   <span data-ttu-id="0676f-152">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="0676f-152">[Click the following image to expand it.</span></span> <span data-ttu-id="0676f-153">]</span><span class="sxs-lookup"><span data-stu-id="0676f-153">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. <span data-ttu-id="0676f-154">Die **-Paket-Manager-Konsole** wird im unteren Abschnitt der Visual Studio angezeigt.</span><span class="sxs-lookup"><span data-stu-id="0676f-154">The **Package Manager Console** will appear in the bottom section of Visual Studio.</span></span> <span data-ttu-id="0676f-155">Typ &quot; **Update-Package EntityFramework** &quot; , und drücken Sie die EINGABETASTE:</span><span class="sxs-lookup"><span data-stu-id="0676f-155">Type &quot;**Update-Package EntityFramework**&quot; and press Enter:</span></span>  
  
   <span data-ttu-id="0676f-156">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="0676f-156">[Click the following image to expand it.</span></span> <span data-ttu-id="0676f-157">]</span><span class="sxs-lookup"><span data-stu-id="0676f-157">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a><span data-ttu-id="0676f-158">Installieren des MySQL-Anbieters für EntityFramework</span><span class="sxs-lookup"><span data-stu-id="0676f-158">Install the MySQL provider for EntityFramework</span></span>

<span data-ttu-id="0676f-159">Damit für EntityFramework, eine Verbindung mit MySQL-Datenbank herstellen kann müssen Sie einen MySQL-Anbieter zu installieren.</span><span class="sxs-lookup"><span data-stu-id="0676f-159">In order for EntityFramework to connect to MySQL database, you need to install a MySQL provider.</span></span> <span data-ttu-id="0676f-160">Zu diesem Zweck öffnen Sie die **-Paket-Manager-Konsole** und &quot; **Install-Package MySql.Data.Entity - Pre-**&quot;, und drücken Sie dann die EINGABETASTE.</span><span class="sxs-lookup"><span data-stu-id="0676f-160">To do so, open the **Package Manager Console** and type &quot;**Install-Package MySql.Data.Entity -Pre**&quot;, and then press Enter.</span></span>

> [!NOTE]
> <span data-ttu-id="0676f-161">Dies ist eine Vorabversion der Assembly, und es kann daher Fehler enthalten.</span><span class="sxs-lookup"><span data-stu-id="0676f-161">This is a pre-release version of the assembly, and as such it may contain bugs.</span></span> <span data-ttu-id="0676f-162">Sie sollten eine Vorabversion des Anbieters nicht in der Produktion verwenden.</span><span class="sxs-lookup"><span data-stu-id="0676f-162">You should not use a pre-release version of the provider in production.</span></span>


<span data-ttu-id="0676f-163">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.]</span><span class="sxs-lookup"><span data-stu-id="0676f-163">[Click the following image to expand it.]</span></span>  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a><span data-ttu-id="0676f-164">Konfigurationsänderungen für Projekt die Datei "Web.config" für Ihre Anwendung</span><span class="sxs-lookup"><span data-stu-id="0676f-164">Making project configuration changes to the Web.config file for your application</span></span>

<span data-ttu-id="0676f-165">Registrieren Sie in diesem Abschnitt Sie Entity Framework konfigurieren für den MySQL-Anbieter verwenden, den Sie soeben installiert haben die MySQL-Anbieter-Factory, und fügen Sie die Verbindungszeichenfolge aus Azure hinzu.</span><span class="sxs-lookup"><span data-stu-id="0676f-165">In this section you will configure the Entity Framework to use the MySQL provider that you just installed, register the MySQL provider factory, and add your connection string from Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="0676f-166">Die folgenden Beispiele enthalten eine bestimmte Assemblyversion für MySql.Data.dll.</span><span class="sxs-lookup"><span data-stu-id="0676f-166">The following examples contain a specific assembly version for MySql.Data.dll.</span></span> <span data-ttu-id="0676f-167">Wenn die Version der Assembly geändert wird, müssen Sie die entsprechenden Konfigurationseinstellungen mit der richtigen Version ändern.</span><span class="sxs-lookup"><span data-stu-id="0676f-167">If the assembly version changes, you will need to modify the appropriate configuration settings with the correct version.</span></span>


1. <span data-ttu-id="0676f-168">Öffnen Sie die Datei "Web.config" für Ihr Projekt in Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="0676f-168">Open the Web.config file for your project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="0676f-169">Suchen Sie die folgenden Konfigurationseinstellungen, die den standardmäßigen Datenbankanbieter und die Factory für das Entity Framework zu definieren:</span><span class="sxs-lookup"><span data-stu-id="0676f-169">Locate the following configuration settings, which define the default database provider and factory for the Entity Framework:</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. <span data-ttu-id="0676f-170">Ersetzen Sie die Konfigurationseinstellungen, mit der folgenden, die Entity Framework zur Verwendung der MySQL-Anbieters konfigurieren:</span><span class="sxs-lookup"><span data-stu-id="0676f-170">Replace those configuration settings with the following, which will configure the Entity Framework to use the MySQL provider:</span></span> 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. <span data-ttu-id="0676f-171">Suchen Sie die &lt;ConnectionStrings&gt; Abschnitt, und Ersetzen Sie ihn durch den folgenden Code, die definieren, wird die Verbindungszeichenfolge für Ihre MySQL-Datenbank gehostet wird, die in Azure (Beachten Sie, dass der Wert von "ProviderName" auch aus geändert wurde der Original):</span><span class="sxs-lookup"><span data-stu-id="0676f-171">Locate the &lt;connectionStrings&gt; section and replace it with the following code, which will define the connection string for your MySQL database that is hosted on Azure (note that providerName value has also been changed from the original):</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a><span data-ttu-id="0676f-172">Hinzufügen von benutzerdefinierten MigrationHistory-Kontext</span><span class="sxs-lookup"><span data-stu-id="0676f-172">Adding custom MigrationHistory context</span></span>

<span data-ttu-id="0676f-173">Entity Framework Code First verwendet eine **MigrationHistory** Tabelle zum Nachverfolgen Änderungen des Datenmodells und um sicherzustellen, dass die Konsistenz zwischen dem Datenbankschema und dem konzeptionellen Schema.</span><span class="sxs-lookup"><span data-stu-id="0676f-173">Entity Framework Code First uses a **MigrationHistory** table to keep track of model changes and to ensure the consistency between the database schema and conceptual schema.</span></span> <span data-ttu-id="0676f-174">In dieser Tabelle funktioniert nicht für MySQL standardmäßig jedoch, da der Primärschlüssel zu groß ist.</span><span class="sxs-lookup"><span data-stu-id="0676f-174">However, this table does not work for MySQL by default because the primary key is too large.</span></span> <span data-ttu-id="0676f-175">Um dieses Problem zu lösen, müssen Sie die Größe des Schlüssels für die Tabelle zu verkleinern.</span><span class="sxs-lookup"><span data-stu-id="0676f-175">To remedy this situation, you will need to shrink the key size for that table.</span></span> <span data-ttu-id="0676f-176">Zu diesem Zweck verwenden Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="0676f-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="0676f-177">Die Schemainformationen für diese Tabelle wird erfasst, eine **"historycontext"**, die sein kann, geändert wie jedes andere **"DbContext"**.</span><span class="sxs-lookup"><span data-stu-id="0676f-177">The schema information for this table is captured in a **HistoryContext**, which can be modified as any other **DbContext**.</span></span> <span data-ttu-id="0676f-178">Zu diesem Zweck fügen Sie eine neue Klassendatei mit dem Namen **MySqlHistoryContext.cs** auf das Projekt, und Ersetzen Sie den Inhalt durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0676f-178">To do so, add a new class file named **MySqlHistoryContext.cs** to the project, and replace its contents with the following code:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. <span data-ttu-id="0676f-179">Als Nächstes müssen Sie zum Konfigurieren von Entity Framework für die geänderte verwenden **"historycontext"**, statt die standardmäßige.</span><span class="sxs-lookup"><span data-stu-id="0676f-179">Next you will need to configure Entity Framework to use the modified **HistoryContext**, rather than default one.</span></span> <span data-ttu-id="0676f-180">Dies kann durch die Nutzung von Funktionen für codebasierte Konfiguration erfolgen.</span><span class="sxs-lookup"><span data-stu-id="0676f-180">This can be done by leveraging code-based configuration features.</span></span> <span data-ttu-id="0676f-181">Zu diesem Zweck fügen Sie neue Klassendatei namens **MySqlConfiguration.cs** zu Ihrem Projekt und Ersetzen Sie den Inhalt mit:</span><span class="sxs-lookup"><span data-stu-id="0676f-181">To do so, add new class file named **MySqlConfiguration.cs** to your project and replace its contents with:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a><span data-ttu-id="0676f-182">Erstellen einen benutzerdefinierten EntityFramework-Initialisierer für ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="0676f-182">Creating a custom EntityFramework initializer for ApplicationDbContext</span></span>

<span data-ttu-id="0676f-183">Der MySQL-Anbieter, der in diesem Tutorial vorgestellt wird unterstützt derzeit Entity Framework-Migrationen nicht, sodass Sie Modell-Initialisierer verwenden, um mit der Datenbank herstellen müssen.</span><span class="sxs-lookup"><span data-stu-id="0676f-183">The MySQL provider that is featured in this tutorial does not currently support Entity Framework migrations, so you will need to use model initializers in order to connect to the database.</span></span> <span data-ttu-id="0676f-184">Da in diesem Tutorial eine MySQL-Instanz in Azure verwendet, müssen Sie einen benutzerdefinierten Initialisierer von Entity Framework zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="0676f-184">Because this tutorial is using a MySQL instance on Azure, you will need to create a custom Entity Framework initializer.</span></span>

> [!NOTE]
> <span data-ttu-id="0676f-185">Dieser Schritt ist nicht erforderlich, wenn Sie eine Verbindung herstellen mit einer SQL Server-Instanz auf Azure oder bei Verwendung eine Datenbank, die lokal gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="0676f-185">This step is not required if you are connecting to a SQL Server instance on Azure or if you are using a database that is hosted on premises.</span></span>


<span data-ttu-id="0676f-186">Um einen benutzerdefinierten Entity Framework-Initialisierer für MySQL zu erstellen, verwenden Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="0676f-186">To create a custom Entity Framework initializer for MySQL, use the following steps:</span></span>

1. <span data-ttu-id="0676f-187">Fügen Sie eine neue Klassendatei mit dem Namen **MySqlInitializer.cs** im Projekt, und Ersetzen sie darin Inhalt durch den folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="0676f-187">Add a new class file named **MySqlInitializer.cs** to the project, and replace it's contents with the following code:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. <span data-ttu-id="0676f-188">Öffnen der **IdentityModels.cs** Datei für Ihr Projekt, die sich im befindet der **Modelle** Verzeichnis, und Ersetzen Sie deren Inhalt durch Folgendes:</span><span class="sxs-lookup"><span data-stu-id="0676f-188">Open the **IdentityModels.cs** file for your project, which is located in the **Models** directory, and replace it's contents with the following:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a><span data-ttu-id="0676f-189">Ausführen der Anwendung, und überprüfen die Datenbank</span><span class="sxs-lookup"><span data-stu-id="0676f-189">Running the application and verifying the database</span></span>

<span data-ttu-id="0676f-190">Nachdem Sie die Schritte in den vorherigen Abschnitten abgeschlossen haben, sollten Sie Ihre Datenbank testen.</span><span class="sxs-lookup"><span data-stu-id="0676f-190">Once you have completed the steps in the preceding sections, you should test your database.</span></span> <span data-ttu-id="0676f-191">Zu diesem Zweck verwenden Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="0676f-191">To do so, use the following steps:</span></span>

1. <span data-ttu-id="0676f-192">Drücken Sie **STRG + F5** erstellen und Ausführen der Webanwendung.</span><span class="sxs-lookup"><span data-stu-id="0676f-192">Press **Ctrl + F5** to build and run the web application.</span></span>
2. <span data-ttu-id="0676f-193">Klicken Sie auf die **registrieren** Registerkarte oben auf der Seite:</span><span class="sxs-lookup"><span data-stu-id="0676f-193">Click the **Register** tab on the top of the page:</span></span>  
  
   <span data-ttu-id="0676f-194">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="0676f-194">[Click the following image to expand it.</span></span> <span data-ttu-id="0676f-195">]</span><span class="sxs-lookup"><span data-stu-id="0676f-195">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. <span data-ttu-id="0676f-196">Geben Sie einen neuen Benutzernamen und ein Kennwort ein, und klicken Sie dann auf **registrieren**:</span><span class="sxs-lookup"><span data-stu-id="0676f-196">Enter a new user name and password, and then click **Register**:</span></span>  
  
   <span data-ttu-id="0676f-197">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="0676f-197">[Click the following image to expand it.</span></span> <span data-ttu-id="0676f-198">]</span><span class="sxs-lookup"><span data-stu-id="0676f-198">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. <span data-ttu-id="0676f-199">An diesem Punkt werden die ASP.NET Identity-Tabellen für die MySQL-Datenbank erstellt, und der Benutzer registriert ist, und der Anwendung angemeldet werden:</span><span class="sxs-lookup"><span data-stu-id="0676f-199">At this point the ASP.NET Identity tables are created on the MySQL Database, and the user is registered and logged into the application:</span></span>  
  
   <span data-ttu-id="0676f-200">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="0676f-200">[Click the following image to expand it.</span></span> <span data-ttu-id="0676f-201">]</span><span class="sxs-lookup"><span data-stu-id="0676f-201">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a><span data-ttu-id="0676f-202">Installieren von MySQL Workbench-Tool, um die Daten zu überprüfen</span><span class="sxs-lookup"><span data-stu-id="0676f-202">Installing MySQL Workbench tool to verify the data</span></span>

1. <span data-ttu-id="0676f-203">Installieren Sie die **MySQL Workbench** tool die [MySQL-Downloadseite](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="0676f-203">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="0676f-204">Im Installations-Assistenten: **Funktionsauswahl** Registerkarte **MySQL Workbench** unter **Anwendungen** Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="0676f-204">In the installation wizard: **Feature Selection** tab, select **MySQL Workbench** under **applications** section.</span></span>
3. <span data-ttu-id="0676f-205">Starten Sie die app aus, und fügen Sie eine neue Verbindung mit die Verbindungsdaten für die Zeichenfolge aus der Azure MySQL-Datenbank, die Sie an, die dadurch in diesem Tutorial erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="0676f-205">Launch the app and add a new connection using the connection string data from the Azure MySQL database you created at the begging of this tutorial.</span></span>
4. <span data-ttu-id="0676f-206">Überprüfen Sie nach dem Herstellen der Verbindung, die **ASP.NET Identity** Tabellen erstellt werden, auf die **IdentityMySQLDatabase.**</span><span class="sxs-lookup"><span data-stu-id="0676f-206">After establishing the connection, inspect the **ASP.NET Identity** tables created on the **IdentityMySQLDatabase.**</span></span>
5. <span data-ttu-id="0676f-207">Sie sehen, dass alle ASP.NET Identity, dass die Tabellen erstellt werden erforderlich, wie in der folgenden Abbildung gezeigt:</span><span class="sxs-lookup"><span data-stu-id="0676f-207">You will see that all ASP.NET Identity required tables are created as shown in the image below:</span></span>  
  
   <span data-ttu-id="0676f-208">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="0676f-208">[Click the following image to expand it.</span></span> <span data-ttu-id="0676f-209">]</span><span class="sxs-lookup"><span data-stu-id="0676f-209">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. <span data-ttu-id="0676f-210">Überprüfen Sie die **"aspnetusers"** Tabelle z. B. für die Einträge zu überprüfen, wie Sie neue Benutzer registrieren.</span><span class="sxs-lookup"><span data-stu-id="0676f-210">Inspect the **aspnetusers** table for instance to check for the entries as you register new users.</span></span>  
  
   <span data-ttu-id="0676f-211">[Klicken Sie auf die folgende Abbildung aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="0676f-211">[Click the following image to expand it.</span></span> <span data-ttu-id="0676f-212">]</span><span class="sxs-lookup"><span data-stu-id="0676f-212">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
