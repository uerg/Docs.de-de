---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET-Identität: Verwenden von MySQL-Speicher mit einer EntityFramework MySQL-Anbieter (c#) | Microsoft Docs'
author: maumar
description: In diesem Lernprogramm wird gezeigt, wie zum Ersetzen des Standardmechanismus für die Speicherung von Daten für ASP.NET Identity mit EntityFramework (SQL-Client-Anbieter) mit einem MySQL bereitstellen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 6018b4f62f95f9abffece536f345d7a16d052aac
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873390"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a><span data-ttu-id="2c1f6-103">ASP.NET Identity: Verwenden von MySQL-Speicher mit EntityFramework MySQL-Anbieter (c#)</span><span class="sxs-lookup"><span data-stu-id="2c1f6-103">ASP.NET Identity: Using MySQL Storage with an EntityFramework MySQL Provider (C#)</span></span>
====================
<span data-ttu-id="2c1f6-104">durch [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="2c1f6-104">by [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span></span>

> <span data-ttu-id="2c1f6-105">Dieses Lernprogramm veranschaulicht das Ersetzen des Standardmechanismus für die Speicherung von Daten für [ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) mit EntityFramework (SQL-Client-Anbieter) mit einem MySQL-Anbieter.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-105">This tutorial shows you how to replace the default data storage mechanism for [**ASP.NET Identity**](introduction-to-aspnet-identity.md) with EntityFramework (SQL client provider) with a MySQL provider.</span></span>


<span data-ttu-id="2c1f6-106">In diesem Lernprogramm werden die folgenden Themen behandelt:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-106">The following topics will be covered in this tutorial:</span></span>

- <span data-ttu-id="2c1f6-107">Erstellen eine MySQL-Datenbank in Azure</span><span class="sxs-lookup"><span data-stu-id="2c1f6-107">Creating a MySQL database on Azure</span></span>
- <span data-ttu-id="2c1f6-108">Erstellen einer MVC-Anwendung mit Visual Studio 2013 MVC-Vorlage</span><span class="sxs-lookup"><span data-stu-id="2c1f6-108">Creating an MVC application using Visual Studio 2013 MVC template</span></span>
- <span data-ttu-id="2c1f6-109">Konfigurieren von EntityFramework zur Bearbeitung eines MySQL-Datenbank-Anbieters</span><span class="sxs-lookup"><span data-stu-id="2c1f6-109">Configuring EntityFramework to work with a MySQL database provider</span></span>
- <span data-ttu-id="2c1f6-110">Ausführen der Anwendung die Ergebnisse überprüfen</span><span class="sxs-lookup"><span data-stu-id="2c1f6-110">Running the application to verify the results</span></span>

<span data-ttu-id="2c1f6-111">Am Ende dieses Lernprogramms müssen Sie eine MVC-Anwendung mit dem ASP.NET Identity zu speichern, die eine MySQL-Datenbank verwendet, die in Azure gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-111">At the end of this tutorial, you will have an MVC application with the ASP.NET Identity store that is using a MySQL database that is hosted in Azure.</span></span>

## <a name="creating-a-mysql-database-instance-on-azure"></a><span data-ttu-id="2c1f6-112">Erstellen eine MySQL-Datenbankinstanz für Azure</span><span class="sxs-lookup"><span data-stu-id="2c1f6-112">Creating a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="2c1f6-113">Melden Sie sich auf die [Azure-Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="2c1f6-113">Log in to the [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span></span>
2. <span data-ttu-id="2c1f6-114">Klicken Sie auf **neu** am unteren Rand der Seite, und wählen Sie dann **STORE**:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-114">Click **NEW** at the bottom of the page, and then select **STORE**:</span></span>  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. <span data-ttu-id="2c1f6-115">In der **auswählen und Add-Ons** Assistenten **ClearDB MySQL-Datenbank**, und klicken Sie dann auf die **Weiter** Pfeil am unteren Rand der Frame:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-115">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database**, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="2c1f6-116">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-116">[Click the following image to expand it.</span></span> <span data-ttu-id="2c1f6-117">]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-117">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. <span data-ttu-id="2c1f6-118">Behalten Sie den Standardwert **frei** planen, ändern Sie die **Namen** auf **IdentityMySQLDatabase**, wählen Sie die Region, die Ihnen am nächsten liegt, und klicken Sie dann auf die **Weiter** Pfeil am unteren Rand der Frame:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-118">Keep the default **Free** plan, change the **NAME** to **IdentityMySQLDatabase**, select the region that is nearest to you, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="2c1f6-119">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-119">[Click the following image to expand it.</span></span> <span data-ttu-id="2c1f6-120">]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-120">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. <span data-ttu-id="2c1f6-121">Klicken Sie auf die **Kauf** Häkchen, um die Datenbankerstellung abgeschlossen.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-121">Click the **PURCHASE** checkmark to complete the database creation.</span></span>  
  
   <span data-ttu-id="2c1f6-122">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-122">[Click the following image to expand it.</span></span> <span data-ttu-id="2c1f6-123">]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-123">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. <span data-ttu-id="2c1f6-124">Nachdem die Datenbank erstellt wurde, können Sie ihn aus verwalten die **ADD-ONS** Registerkarte im Verwaltungsportal.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-124">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span> <span data-ttu-id="2c1f6-125">Um die Verbindungsinformationen für Ihre Datenbank abgerufen werden sollen, klicken Sie auf **VERBINDUNGSINFORMATIONEN** am unteren Rand der Seite:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-125">To retrieve the connection information for your database, click **CONNECTION INFO** at the bottom of the page:</span></span>  
  
   <span data-ttu-id="2c1f6-126">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-126">[Click the following image to expand it.</span></span> <span data-ttu-id="2c1f6-127">]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-127">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. <span data-ttu-id="2c1f6-128">Kopieren Sie die Verbindungszeichenfolge durch Klicken auf die Schaltfläche Kopieren, indem Sie die **"ConnectionString"** ; Feld und speichern sie diese Informationen später in diesem Lernprogramm verwenden Sie für Ihre MVC-Anwendung:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-128">Copy the connection string by clicking on the copy button by the **CONNECTIONSTRING** field and save it; you will use this information later in this tutorial for your MVC application:</span></span>  
  
   <span data-ttu-id="2c1f6-129">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-129">[Click the following image to expand it.</span></span> <span data-ttu-id="2c1f6-130">]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-130">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a><span data-ttu-id="2c1f6-131">Erstellen ein MVC-Anwendungsprojekt</span><span class="sxs-lookup"><span data-stu-id="2c1f6-131">Creating an MVC application project</span></span>

<span data-ttu-id="2c1f6-132">Um die Schritte in diesem Abschnitt des Lernprogramms abgeschlossen haben, müssen Sie zuerst installieren [Visual Studio Express 2013 für Web](https://go.microsoft.com/fwlink/?LinkId=299058) oder [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="2c1f6-132">To complete the steps in this section of the tutorial, you will first need to install [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="2c1f6-133">Nachdem Visual Studio installiert wurde, verwenden Sie die folgenden Schritte aus, um ein neues MVC-Anwendungsprojekt erstellen:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-133">Once Visual Studio has been installed, use the following steps to create a new MVC application project:</span></span>

1. <span data-ttu-id="2c1f6-134">Open Visual Studio 2103.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-134">Open Visual Studio 2103.</span></span>
2. <span data-ttu-id="2c1f6-135">Klicken Sie auf **neues Projekt** aus der **starten** Seite, oder Sie klicken Sie auf die **Datei** Menü und dann **neues Projekt**:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-135">Click **New Project** from the **Start** page, or you can click the **File** menu and then **New Project**:</span></span>  
  
   <span data-ttu-id="2c1f6-136">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-136">[Click the following image to expand it.</span></span> <span data-ttu-id="2c1f6-137">]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-137">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. <span data-ttu-id="2c1f6-138">Wenn die **neues Projekt** im Dialogfeld angezeigt wird, erweitern Sie **Visual C#-** in der Liste der Vorlagen, klicken Sie dann auf **Web**, und wählen Sie **ASP.NET-Webanwendung**.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-138">When the **New Project** dialog box is displayed, expand **Visual C#** in the list of templates, then click **Web**, and select **ASP.NET Web Application**.</span></span> <span data-ttu-id="2c1f6-139">Namen für das Projekt **IdentityMySQLDemo** , und klicken Sie dann auf **OK**:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-139">Name your project **IdentityMySQLDemo** and then click **OK**:</span></span>  
  
   <span data-ttu-id="2c1f6-140">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-140">[Click the following image to expand it.</span></span> <span data-ttu-id="2c1f6-141">]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-141">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. <span data-ttu-id="2c1f6-142">In der **neues ASP.NET-Projekt** wählen Sie im Dialogfeld die **MVC** Templatewith Standardoptionen; Dadurch werden konfigurieren **einzelne Benutzerkonten** als Authentifizierungsmethode.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-142">In the **New ASP.NET Project** dialog, select the **MVC** templatewith the default options; this will configure **Individual User Accounts** as the authentication method.</span></span> <span data-ttu-id="2c1f6-143">Klicken Sie auf **OK**:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-143">Click **OK**:</span></span>  
  
   <span data-ttu-id="2c1f6-144">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-144">[Click the following image to expand it.</span></span> <span data-ttu-id="2c1f6-145">]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-145">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a><span data-ttu-id="2c1f6-146">Konfigurieren von EntityFramework zur Bearbeitung einer MySQL-Datenbank</span><span class="sxs-lookup"><span data-stu-id="2c1f6-146">Configure EntityFramework to work with a MySQL database</span></span>

### <a name="update-the-entity-framework-assembly-for-your-project"></a><span data-ttu-id="2c1f6-147">Aktualisieren Sie die Entity Framework-Assembly für das Projekt</span><span class="sxs-lookup"><span data-stu-id="2c1f6-147">Update the Entity Framework assembly for your project</span></span>

<span data-ttu-id="2c1f6-148">Die MVC-Anwendung, die aus der Visual Studio 2013-Vorlage erstellt wurde, enthält einen Verweis auf die [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) Verpacken, aber es wurden Updates, die auf diese Assembly seit der Version enthalten wichtige wurde leistungsverbesserungen.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-148">The MVC application that was created from the Visual Studio 2013 template contains a reference to the [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) package, but there have been updates to that assembly since its release which contain significant performance improvements.</span></span> <span data-ttu-id="2c1f6-149">Um diese neueste Updates in Ihrer Anwendung verwenden möchten, verwenden Sie die folgenden Schritte aus.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-149">In order to use these latest updates in your application, use the following steps.</span></span>

1. <span data-ttu-id="2c1f6-150">Öffnen Sie das MVC-Projekt in Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-150">Open your MVC project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="2c1f6-151">Klicken Sie auf **TOOLS**, klicken Sie dann auf **Bibliothekspaket-Manager**, und klicken Sie dann auf **Package Manager Console**:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-151">Click **TOOLS**, then click **Library Package Manager**, and then click **Package Manager Console**:</span></span>  
  
   <span data-ttu-id="2c1f6-152">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-152">[Click the following image to expand it.</span></span> <span data-ttu-id="2c1f6-153">]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-153">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. <span data-ttu-id="2c1f6-154">Die **Package Manager Console** erscheint im unteren Abschnitt des Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-154">The **Package Manager Console** will appear in the bottom section of Visual Studio.</span></span> <span data-ttu-id="2c1f6-155">Typ &quot; **das Updatepaket EntityFramework** &quot; und drücken Sie die EINGABETASTE:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-155">Type &quot;**Update-Package EntityFramework**&quot; and press Enter:</span></span>  
  
   <span data-ttu-id="2c1f6-156">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-156">[Click the following image to expand it.</span></span> <span data-ttu-id="2c1f6-157">]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-157">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a><span data-ttu-id="2c1f6-158">Installieren Sie den MySQL-Anbieter für EntityFramework</span><span class="sxs-lookup"><span data-stu-id="2c1f6-158">Install the MySQL provider for EntityFramework</span></span>

<span data-ttu-id="2c1f6-159">In der Reihenfolge für EntityFramework zur Verbindung mit MySQL-Datenbank müssen Sie einen MySQL-Anbieter installieren.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-159">In order for EntityFramework to connect to MySQL database, you need to install a MySQL provider.</span></span> <span data-ttu-id="2c1f6-160">Öffnen Sie hierzu die **Package Manager Console** und Typ &quot; **Install-Package MySql.Data.Entity - Pre**&quot;, und drücken Sie dann die EINGABETASTE.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-160">To do so, open the **Package Manager Console** and type &quot;**Install-Package MySql.Data.Entity -Pre**&quot;, and then press Enter.</span></span>

> [!NOTE]
> <span data-ttu-id="2c1f6-161">Dies ist eine Vorabversion der Assembly, und daher möglicherweise Fehler enthalten.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-161">This is a pre-release version of the assembly, and as such it may contain bugs.</span></span> <span data-ttu-id="2c1f6-162">Sie sollten eine Vorabversion des Anbieters nicht in der Produktion verwenden.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-162">You should not use a pre-release version of the provider in production.</span></span>


<span data-ttu-id="2c1f6-163">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-163">[Click the following image to expand it.]</span></span>  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a><span data-ttu-id="2c1f6-164">Projekt-konfigurationsänderungen vorzunehmen, um die Datei "Web.config" für Ihre Anwendung</span><span class="sxs-lookup"><span data-stu-id="2c1f6-164">Making project configuration changes to the Web.config file for your application</span></span>

<span data-ttu-id="2c1f6-165">Registrieren Sie in diesem Abschnitt Sie Entity Framework konfigurieren um MySQL-Anbieter verwenden, den Sie gerade installiert haben die MySQL-Anbieter-Factory, und fügen Sie der Verbindungszeichenfolge von Azure hinzu.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-165">In this section you will configure the Entity Framework to use the MySQL provider that you just installed, register the MySQL provider factory, and add your connection string from Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="2c1f6-166">In den folgenden Beispielen wird eine bestimmte Assemblyversion für MySql.Data.dll enthalten.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-166">The following examples contain a specific assembly version for MySql.Data.dll.</span></span> <span data-ttu-id="2c1f6-167">Wenn die Assemblyversion geändert wird, müssen Sie die entsprechenden Konfigurationseinstellungen mit der richtigen Version ändern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-167">If the assembly version changes, you will need to modify the appropriate configuration settings with the correct version.</span></span>


1. <span data-ttu-id="2c1f6-168">Öffnen Sie die Datei "Web.config" für das Projekt in Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-168">Open the Web.config file for your project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="2c1f6-169">Suchen Sie die folgenden Konfigurationseinstellungen, die den Standardanbieter für die Datenbank und die Factory für das Entity Framework zu definieren:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-169">Locate the following configuration settings, which define the default database provider and factory for the Entity Framework:</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. <span data-ttu-id="2c1f6-170">Ersetzen Sie diese Konfigurationseinstellungen, durch die folgende Darstellung, die das Entity Framework zur Verwendung von MySQL-Anbieter konfiguriert werden soll:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-170">Replace those configuration settings with the following, which will configure the Entity Framework to use the MySQL provider:</span></span> 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. <span data-ttu-id="2c1f6-171">Suchen Sie die &lt;ConnectionStrings&gt; Abschnitt, und Ersetzen Sie ihn durch den folgenden Code, mit denen definiert wird, wird die Verbindungszeichenfolge für Ihre MySQL-Datenbank gehostet wird, die in Azure (Beachten Sie, dass auch ProviderName-Wert von geändert wurde die Original):</span><span class="sxs-lookup"><span data-stu-id="2c1f6-171">Locate the &lt;connectionStrings&gt; section and replace it with the following code, which will define the connection string for your MySQL database that is hosted on Azure (note that providerName value has also been changed from the original):</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a><span data-ttu-id="2c1f6-172">Hinzufügen von benutzerdefinierten MigrationHistory Kontext</span><span class="sxs-lookup"><span data-stu-id="2c1f6-172">Adding custom MigrationHistory context</span></span>

<span data-ttu-id="2c1f6-173">Entity Framework Code First verwendet eine **MigrationHistory** Tabelle zum Nachverfolgen modelländerungen und um die Konsistenz zwischen dem Datenbankschema und konzeptionelle sicherzustellen.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-173">Entity Framework Code First uses a **MigrationHistory** table to keep track of model changes and to ensure the consistency between the database schema and conceptual schema.</span></span> <span data-ttu-id="2c1f6-174">Allerdings funktioniert in dieser Tabelle für MySQL standardmäßig nicht, da der Primärschlüssel zu groß ist.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-174">However, this table does not work for MySQL by default because the primary key is too large.</span></span> <span data-ttu-id="2c1f6-175">Um diese Situation zu vermeiden, müssen Sie die Größe des Schlüssels für die Tabelle zu verkleinern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-175">To remedy this situation, you will need to shrink the key size for that table.</span></span> <span data-ttu-id="2c1f6-176">Verwenden Sie hierzu die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="2c1f6-177">Die Schemainformationen für diese Tabelle wird erfasst, einem **HistoryContext**, die sein kann wie jeder andere geändert **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-177">The schema information for this table is captured in a **HistoryContext**, which can be modified as any other **DbContext**.</span></span> <span data-ttu-id="2c1f6-178">Zu diesem Zweck fügen Sie eine neue Klassendatei mit dem Namen **MySqlHistoryContext.cs** für das Projekt, und Ersetzen Sie den Inhalt durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-178">To do so, add a new class file named **MySqlHistoryContext.cs** to the project, and replace its contents with the following code:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. <span data-ttu-id="2c1f6-179">Als Nächstes müssen Sie Entity Framework, um das geänderte verwenden konfigurieren **HistoryContext**, statt einen Standardwert.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-179">Next you will need to configure Entity Framework to use the modified **HistoryContext**, rather than default one.</span></span> <span data-ttu-id="2c1f6-180">Dies kann durch die Nutzung der Features codebasierte Konfiguration erfolgen.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-180">This can be done by leveraging code-based configuration features.</span></span> <span data-ttu-id="2c1f6-181">Zu diesem Zweck fügen Sie neue Klassendatei namens **MySqlConfiguration.cs** zu Ihrem Projekt, und Ersetzen Sie den Inhalt mit:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-181">To do so, add new class file named **MySqlConfiguration.cs** to your project and replace its contents with:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a><span data-ttu-id="2c1f6-182">Erstellen einen benutzerdefinierten EntityFramework Initialisierer für ApplicationDbContext</span><span class="sxs-lookup"><span data-stu-id="2c1f6-182">Creating a custom EntityFramework initializer for ApplicationDbContext</span></span>

<span data-ttu-id="2c1f6-183">Der MySQL-Anbieter, der in diesem Lernprogramm Funktionsumfang ist unterstützt derzeit Entity Framework keine Migrationen, daher Sie die Modell-Initialisierer verwenden, um eine Verbindung mit der Datenbank herzustellen müssen.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-183">The MySQL provider that is featured in this tutorial does not currently support Entity Framework migrations, so you will need to use model initializers in order to connect to the database.</span></span> <span data-ttu-id="2c1f6-184">Da in diesem Lernprogramm eine MySQL-Instanz in Azure verwendet wird, müssen Sie einen benutzerdefinierten Initialisierer für Entity Framework zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-184">Because this tutorial is using a MySQL instance on Azure, you will need to create a custom Entity Framework initializer.</span></span>

> [!NOTE]
> <span data-ttu-id="2c1f6-185">Dieser Schritt ist nicht erforderlich, wenn Sie eine Verbindung zu SQL Server-Instanz in Azure oder bei Verwendung eine Datenbank, die lokal gehostet wird.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-185">This step is not required if you are connecting to a SQL Server instance on Azure or if you are using a database that is hosted on premises.</span></span>


<span data-ttu-id="2c1f6-186">Um einen benutzerdefinierten Entity Framework-Initialisierer für MySQL zu erstellen, verwenden Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-186">To create a custom Entity Framework initializer for MySQL, use the following steps:</span></span>

1. <span data-ttu-id="2c1f6-187">Fügen Sie eine neue Klassendatei mit dem Namen **MySqlInitializer.cs** auf das Projekt, und Ersetzen sie Inhalt durch folgenden Code wird:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-187">Add a new class file named **MySqlInitializer.cs** to the project, and replace it's contents with the following code:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. <span data-ttu-id="2c1f6-188">Öffnen der **IdentityModels.cs** für das Projekt, befindet sich im die **Modelle** Verzeichnis und dessen Inhalt durch Folgendes ersetzen:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-188">Open the **IdentityModels.cs** file for your project, which is located in the **Models** directory, and replace it's contents with the following:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a><span data-ttu-id="2c1f6-189">Ausführen der Anwendung, und überprüfen die Datenbank</span><span class="sxs-lookup"><span data-stu-id="2c1f6-189">Running the application and verifying the database</span></span>

<span data-ttu-id="2c1f6-190">Wenn Sie die Schritte in den vorangehenden Abschnitten abgeschlossen haben, sollten Sie Ihre Datenbank testen.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-190">Once you have completed the steps in the preceding sections, you should test your database.</span></span> <span data-ttu-id="2c1f6-191">Verwenden Sie hierzu die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-191">To do so, use the following steps:</span></span>

1. <span data-ttu-id="2c1f6-192">Drücken Sie **STRG + F5** erstellen und Ausführen der Anwendung.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-192">Press **Ctrl + F5** to build and run the web application.</span></span>
2. <span data-ttu-id="2c1f6-193">Klicken Sie auf die **registrieren** Registerkarte oben auf der Seite:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-193">Click the **Register** tab on the top of the page:</span></span>  
  
   <span data-ttu-id="2c1f6-194">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-194">[Click the following image to expand it.</span></span> <span data-ttu-id="2c1f6-195">]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-195">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. <span data-ttu-id="2c1f6-196">Geben Sie einen neuen Benutzernamen und ein Kennwort ein, und klicken Sie dann auf **registrieren**:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-196">Enter a new user name and password, and then click **Register**:</span></span>  
  
   <span data-ttu-id="2c1f6-197">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-197">[Click the following image to expand it.</span></span> <span data-ttu-id="2c1f6-198">]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-198">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. <span data-ttu-id="2c1f6-199">An diesem Punkt werden die ASP.NET Identity-Tabellen für die MySQL-Datenbank erstellt, und der Benutzer registriert und der Anwendung protokolliert wird:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-199">At this point the ASP.NET Identity tables are created on the MySQL Database, and the user is registered and logged into the application:</span></span>  
  
   <span data-ttu-id="2c1f6-200">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-200">[Click the following image to expand it.</span></span> <span data-ttu-id="2c1f6-201">]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-201">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a><span data-ttu-id="2c1f6-202">Installieren von MySQL-Workbench-Tool, um die Daten zu überprüfen</span><span class="sxs-lookup"><span data-stu-id="2c1f6-202">Installing MySQL Workbench tool to verify the data</span></span>

1. <span data-ttu-id="2c1f6-203">Installieren der **MySQL-Workbench** -tool aus dem [downloads für MySQL](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="2c1f6-203">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="2c1f6-204">Im Installations-Assistenten: **Funktionsauswahl** Registerkarte **MySQL-Workbench** unter **Anwendungen** Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-204">In the installation wizard: **Feature Selection** tab, select **MySQL Workbench** under **applications** section.</span></span>
3. <span data-ttu-id="2c1f6-205">Starten Sie die app, und fügen Sie eine neue Verbindung mit die Verbindungsdaten für die Zeichenfolge aus der Azure-MySQL-Datenbank, die Sie an der wünschenswerten dieses Lernprogramms erstellt haben.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-205">Launch the app and add a new connection using the connection string data from the Azure MySQL database you created at the begging of this tutorial.</span></span>
4. <span data-ttu-id="2c1f6-206">Überprüfen Sie nach dem Herstellen der Verbindung, die **ASP.NET Identity** Tabellen erstellt, auf die **IdentityMySQLDatabase.**</span><span class="sxs-lookup"><span data-stu-id="2c1f6-206">After establishing the connection, inspect the **ASP.NET Identity** tables created on the **IdentityMySQLDatabase.**</span></span>
5. <span data-ttu-id="2c1f6-207">Sie sehen, dass alle ASP.NET Identity Tabellen erstellt werden erforderlichen, wie in der folgenden Abbildung gezeigt:</span><span class="sxs-lookup"><span data-stu-id="2c1f6-207">You will see that all ASP.NET Identity required tables are created as shown in the image below:</span></span>  
  
   <span data-ttu-id="2c1f6-208">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-208">[Click the following image to expand it.</span></span> <span data-ttu-id="2c1f6-209">]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-209">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. <span data-ttu-id="2c1f6-210">Überprüfen Sie die **Aspnetusers** Tabelle für die Instanz für die Einträge zu überprüfen, wie Sie neue Benutzer registrieren.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-210">Inspect the **aspnetusers** table for instance to check for the entries as you register new users.</span></span>  
  
   <span data-ttu-id="2c1f6-211">[Klicken Sie auf das folgende Bild aus, um ihn zu erweitern.</span><span class="sxs-lookup"><span data-stu-id="2c1f6-211">[Click the following image to expand it.</span></span> <span data-ttu-id="2c1f6-212">]</span><span class="sxs-lookup"><span data-stu-id="2c1f6-212">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
