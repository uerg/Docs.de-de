---
uid: whitepapers/aspnet-and-iis6
title: "Ausführen von ASP.NET 1.1 mit IIS 6.0 | Microsoft Docs"
author: rick-anderson
description: "Während Windows Server 2003 mit IIS 6.0 und ASP.NET 1.1 enthalten, sind diese Komponenten standardmäßig deaktiviert. Dieses Whitepaper enthält Informationen zum Aktivieren von IIS 6.0 ein..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 5a5537bf-2aaa-49e7-839f-9e6522b829d8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-and-iis6
msc.type: content
ms.openlocfilehash: 1fcac7b8bc295ccf4e36189295b6bc2e4d328623
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="running-aspnet-11-with-iis-60"></a><span data-ttu-id="e084e-104">Ausführen von ASP.NET 1.1 mit IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="e084e-104">Running ASP.NET 1.1 with IIS 6.0</span></span>
====================
> <span data-ttu-id="e084e-105">Während Windows Server 2003 mit IIS 6.0 und ASP.NET 1.1 enthalten, sind diese Komponenten standardmäßig deaktiviert.</span><span class="sxs-lookup"><span data-stu-id="e084e-105">While Windows Server 2003 includes both IIS 6.0 and ASP.NET 1.1, these components are disabled by default.</span></span> <span data-ttu-id="e084e-106">Dieses Whitepaper enthält Informationen zum Aktivieren von IIS 6.0 und ASP.NET 1.1 und empfiehlt mehrere Konfigurationseinstellungen, die eine optimale Leistung von IIS und ASP.NET zu erhalten.</span><span class="sxs-lookup"><span data-stu-id="e084e-106">This whitepaper describes how to enable IIS 6.0 and ASP.NET 1.1, and recommends several configuration settings to get the optimal performance from IIS and ASP.NET.</span></span>
> 
> <span data-ttu-id="e084e-107">Gilt für ASP.NET 1.1 und IIS 6.0.</span><span class="sxs-lookup"><span data-stu-id="e084e-107">Applies to ASP.NET 1.1 and IIS 6.0.</span></span>


<span data-ttu-id="e084e-108">Im Lieferumfang von ASP.NET 1.1 ist Windows Server 2003, die auch die neueste Version von Internet Information Server (IIS) Version 6.0 umfasst.</span><span class="sxs-lookup"><span data-stu-id="e084e-108">ASP.NET 1.1 ships with Windows Server 2003, which also includes the latest version of Internet Information Server (IIS) version 6.0.</span></span> <span data-ttu-id="e084e-109">IIS 6.0 und ASP.NET 1.1 sind nahtlos integriert und ASP.NET wird jetzt standardmäßig auf das neue IIS 6.0-Worker-Prozessmodell.</span><span class="sxs-lookup"><span data-stu-id="e084e-109">IIS 6.0 and ASP.NET 1.1 are designed to integrate seamlessly and ASP.NET now defaults to the new IIS 6.0 worker process model.</span></span>

## <a name="aspnet-11-is-not-installed-by-default"></a><span data-ttu-id="e084e-110">ASP.NET 1.1 ist standardmäßig nicht installiert.</span><span class="sxs-lookup"><span data-stu-id="e084e-110">ASP.NET 1.1 is not installed by default</span></span>

<span data-ttu-id="e084e-111">Im Gegensatz zu früheren Versionen von Microsoft Server-Betriebssystemen ist Internet Information Server (IIS) nicht standardmäßig aktiviert. noch wird von ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="e084e-111">Unlike previous versions of Microsoft's server operating systems, Internet Information Server (IIS) is not enabled by default; nor is ASP.NET 1.1.</span></span> <span data-ttu-id="e084e-112">Es gibt zwei Optionen zum Aktivieren von IIS:</span><span class="sxs-lookup"><span data-stu-id="e084e-112">There are two options for enabling IIS:</span></span>

### <a name="enabling-iis-option-1---configure-your-server-wizard"></a><span data-ttu-id="e084e-113">Aktivieren von IIS, Option #1 - Serverkonfigurations-Assistent</span><span class="sxs-lookup"><span data-stu-id="e084e-113">Enabling IIS, option #1 - Configure Your Server Wizard</span></span>

<span data-ttu-id="e084e-114">Windows Server 2003 bereitgestellt wird, eine neue "Serverkonfigurations-Assistent" können Sie den Server in den gewünschten Modus ordnungsgemäß zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e084e-114">Windows Server 2003 ships a new 'Configure Your Server Wizard' to help you properly configure your server in the desired mode.</span></span>

<span data-ttu-id="e084e-115">Beachten Sie, dass zum Ausführen des Assistenten Sie als Administrator - angemeldet werden müssen zum Starten des Assistenten - wechseln Sie zu: Start | Programme | Verwaltung, und wählen Sie "Konfigurieren des Servers".</span><span class="sxs-lookup"><span data-stu-id="e084e-115">To start the wizard - note, to run the wizard you must be logged in as an administrator - go to: Start | Programs | Administrative Tools and select 'Configure Your Server'.</span></span>

<span data-ttu-id="e084e-116">Nach dem Auswählen der Bildschirm "Serverkonfigurations-Assistent" Öffnen sollte angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="e084e-116">Once selected you should see the 'Configure Your Server Wizard' opening screen:</span></span>

![](aspnet-and-iis6/_static/image1.jpg)

<span data-ttu-id="e084e-117">Klicken Sie auf "Weiter &gt;":</span><span class="sxs-lookup"><span data-stu-id="e084e-117">Click 'Next &gt;':</span></span>

![](aspnet-and-iis6/_static/image2.jpg)

<span data-ttu-id="e084e-118">Klicken Sie auf "Weiter &gt;"</span><span class="sxs-lookup"><span data-stu-id="e084e-118">Click 'Next &gt;'</span></span>

![](aspnet-and-iis6/_static/image3.jpg)

<span data-ttu-id="e084e-119">Auf diesem Bildschirm müssen, wählen Sie ' Anwendungsserver (IIS, ASP.NET) als die Optionen zum Konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e084e-119">On this screen you will need to select 'Application server (IIS, ASP.NET) as the options to configure.</span></span>

<span data-ttu-id="e084e-120">Klicken Sie auf "Weiter &gt;".</span><span class="sxs-lookup"><span data-stu-id="e084e-120">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image4.jpg)

<span data-ttu-id="e084e-121">Nach der Auswahl, um den Server als Anwendungsserver konfigurieren, wird dieser Bildschirm angezeigt werden aufgefordert, zusätzlichen Funktionen installiert werden soll.</span><span class="sxs-lookup"><span data-stu-id="e084e-121">After selecting to configure the server as an Application Server, this screen will be displayed prompting what additional capabilities should be installed.</span></span> <span data-ttu-id="e084e-122">Keine Option ist standardmäßig aktiviert.</span><span class="sxs-lookup"><span data-stu-id="e084e-122">Neither option is selected by default.</span></span> <span data-ttu-id="e084e-123">Um ASP.NET automatisch zu aktivieren, müssen Sie auswählen ' ASP aktivieren. NET ".</span><span class="sxs-lookup"><span data-stu-id="e084e-123">To enable ASP.NET automatically, you need to select 'Enable ASP.NET'.</span></span>

<span data-ttu-id="e084e-124">Klicken Sie auf "Weiter &gt;".</span><span class="sxs-lookup"><span data-stu-id="e084e-124">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image5.jpg)

<span data-ttu-id="e084e-125">Dieser Bildschirm wird angezeigt, welche Optionen sind, installiert werden.</span><span class="sxs-lookup"><span data-stu-id="e084e-125">This screen displays what options are to be installed.</span></span>

<span data-ttu-id="e084e-126">Klicken Sie auf "Weiter &gt;".</span><span class="sxs-lookup"><span data-stu-id="e084e-126">Click 'Next &gt;'.</span></span>

![](aspnet-and-iis6/_static/image6.jpg)

<span data-ttu-id="e084e-127">Dieser Bildschirm wird angezeigt, während die ausgewählten Optionen installiert werden.</span><span class="sxs-lookup"><span data-stu-id="e084e-127">You will see this screen while the options you selected are being installed.</span></span> <span data-ttu-id="e084e-128">Es ist normal, andere Dialogfeld angezeigt, die Felder angezeigt, wie Dienste installiert werden.</span><span class="sxs-lookup"><span data-stu-id="e084e-128">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="e084e-129">Sie können außerdem für den Speicherort der Windows 2003 Server-Installations-CD aufgefordert.</span><span class="sxs-lookup"><span data-stu-id="e084e-129">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="e084e-130">Klicken Sie auf "Weiter &gt;" nach dem Abschluss.</span><span class="sxs-lookup"><span data-stu-id="e084e-130">Click 'Next &gt;' when complete.</span></span>

![](aspnet-and-iis6/_static/image7.jpg)

<span data-ttu-id="e084e-131">Klicken Sie auf "Fertig stellen": der Windows Server 2003 ist zur Unterstützung von IIS 6.0 und ASP.NET 1.1 jetzt konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="e084e-131">Click 'Finish' - the Windows Server 2003 is now configured to support IIS 6.0 and ASP.NET 1.1.</span></span>

### <a name="enabling-iis-option-2---manually-configuring-iis-and-aspnet"></a><span data-ttu-id="e084e-132">Aktivieren von IIS, Option #2: Manuelles Konfigurieren von IIS und ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e084e-132">Enabling IIS, option #2 - Manually configuring IIS and ASP.NET</span></span>

<span data-ttu-id="e084e-133">Wenn Sie nicht, verwenden Sie die "Serverkonfigurations-Assistent möchten" können Sie optional die IIS 6.0 und ASP.NET 1.1 verwenden "Programme hinzufügen oder entfernen" in der Systemsteuerung installieren.</span><span class="sxs-lookup"><span data-stu-id="e084e-133">If you do not wish to use the 'Configure Your Server Wizard' you can optionally install IIS 6.0 and ASP.NET 1.1 using 'Add or Remove Programs' from the Control Panel.</span></span>

<span data-ttu-id="e084e-134">Öffnen Sie zunächst die Systemsteuerung:</span><span class="sxs-lookup"><span data-stu-id="e084e-134">First open the Control Panel:</span></span>

![](aspnet-and-iis6/_static/image8.jpg)

<span data-ttu-id="e084e-135">Klicken Sie dann auf "Hinzufügen/entfernen Windows Components" Öffnen den Assistenten zum Komponenten von Windows auf:</span><span class="sxs-lookup"><span data-stu-id="e084e-135">Next, click on 'Add/Remove Windows Components' which will open the 'Windows Components Wizard':</span></span>

![](aspnet-and-iis6/_static/image9.jpg)

<span data-ttu-id="e084e-136">Markieren Sie, und überprüfen Sie 'Anwendungsserver', und klicken Sie dann auf 'Details'?</span><span class="sxs-lookup"><span data-stu-id="e084e-136">Highlight and check 'Application Server' and then click the 'Details?'</span></span> <span data-ttu-id="e084e-137">Schaltfläche:</span><span class="sxs-lookup"><span data-stu-id="e084e-137">button:</span></span>

![](aspnet-and-iis6/_static/image10.jpg)

<span data-ttu-id="e084e-138">Überprüfen Sie zum Installieren von ASP.NET ' ASP. NET ".</span><span class="sxs-lookup"><span data-stu-id="e084e-138">To install ASP.NET, check 'ASP.NET'.</span></span>

<span data-ttu-id="e084e-139">Klicken Sie auf "OK", um zum Assistenten für Windows-Komponenten zurückgegeben.</span><span class="sxs-lookup"><span data-stu-id="e084e-139">Click 'OK' to return to the Windows Component Wizard.</span></span> <span data-ttu-id="e084e-140">Klicken Sie auf "Weiter &gt;" klicken Sie im Assistenten für Windows-Komponente zu Beginn der Installation:</span><span class="sxs-lookup"><span data-stu-id="e084e-140">Click 'Next &gt;' from the Windows Component Wizard to begin installing:</span></span>

![](aspnet-and-iis6/_static/image11.jpg)

<span data-ttu-id="e084e-141">Es ist normal, andere Dialogfeld angezeigt, die Felder angezeigt, wie Dienste installiert werden.</span><span class="sxs-lookup"><span data-stu-id="e084e-141">It is normal to see other dialog boxes appear as services are being installed.</span></span> <span data-ttu-id="e084e-142">Sie können außerdem für den Speicherort der Windows 2003 Server-Installations-CD aufgefordert.</span><span class="sxs-lookup"><span data-stu-id="e084e-142">You may additionally be prompted for the location of the Windows 2003 Server installation CD.</span></span>

<span data-ttu-id="e084e-143">Wenn die Installation abgeschlossen ist, sehen Sie der letzten Seite des Assistenten-Komponente für Windows:</span><span class="sxs-lookup"><span data-stu-id="e084e-143">When installation is complete you will see the last screen of the Windows Component Wizard:</span></span>

![](aspnet-and-iis6/_static/image12.jpg)

<span data-ttu-id="e084e-144">IIS 6.0 und ASP.NET 1.1 sind nun konfiguriert und verfügbar.</span><span class="sxs-lookup"><span data-stu-id="e084e-144">IIS 6.0 and ASP.NET 1.1 are now configured and available.</span></span>

## <a name="recommended-settings"></a><span data-ttu-id="e084e-145">Empfohlene Einstellungen</span><span class="sxs-lookup"><span data-stu-id="e084e-145">Recommended Settings</span></span>

<span data-ttu-id="e084e-146">Beim Ausführen von ASP.NET 1.1 mit IIS 6.0 stehen mehrere Konfigurationseinstellungen, die empfohlen werden, um die optimale Leistung von ASP.NET zu erhalten:</span><span class="sxs-lookup"><span data-stu-id="e084e-146">When running ASP.NET 1.1 with IIS 6.0 there are several configuration settings that are recommended to get the optimal performance from ASP.NET:</span></span>

- <span data-ttu-id="e084e-147">Konfigurieren von Worker-Prozess-Arbeitsspeichergrenze</span><span class="sxs-lookup"><span data-stu-id="e084e-147">Configuring worker process memory limits</span></span>
- <span data-ttu-id="e084e-148">Konfigurieren von Arbeitsprozessen</span><span class="sxs-lookup"><span data-stu-id="e084e-148">Configuring worker process recycling</span></span>

### <a name="configuring-worker-process-memory-limits"></a><span data-ttu-id="e084e-149">Konfigurieren von Worker-Prozess-Arbeitsspeichergrenze</span><span class="sxs-lookup"><span data-stu-id="e084e-149">Configuring worker process memory limits</span></span>

<span data-ttu-id="e084e-150">Standardmäßig wird IIS 6.0 keine die Menge an Arbeitsspeicher begrenzen, die IIS verwenden darf.</span><span class="sxs-lookup"><span data-stu-id="e084e-150">By default IIS 6.0 does not set a limit on the amount of memory that IIS is allowed to use.</span></span> <span data-ttu-id="e084e-151">ASP IST. NET Cache-Funktion basiert auf eine Einschränkung des Arbeitsspeichers, damit der Cache proaktiv nicht verwendete Elemente aus dem Arbeitsspeicher entfernen kann.</span><span class="sxs-lookup"><span data-stu-id="e084e-151">ASP.NET's Cache feature relies on a limitation of memory so the Cache can proactively remove unused items from memory.</span></span>

<span data-ttu-id="e084e-152">Es wird empfohlen, dass Sie die Wiederverwendung von IIS 6.0 Funktion Arbeitsspeicher konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="e084e-152">It is recommended that you configure the memory recycling feature of IIS 6.0.</span></span> <span data-ttu-id="e084e-153">So konfigurieren Sie diese öffnen Internetinformationsdienste-Manager (Start | Programme | Verwaltung | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="e084e-153">To configure this open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="e084e-154">Sobald geöffnet ist, erweitern Sie den Ordner "Anwendungspools":</span><span class="sxs-lookup"><span data-stu-id="e084e-154">Once open, expand the 'Application Pools' folder:</span></span>

<span data-ttu-id="e084e-155">Für jeden Anwendungspool:</span><span class="sxs-lookup"><span data-stu-id="e084e-155">For each application pool:</span></span>

![](aspnet-and-iis6/_static/image13.jpg)

1. <span data-ttu-id="e084e-156">Mit der rechten Maustaste auf den Anwendungspool, z. B. "DefaultAppPool", und wählen Sie "Eigenschaften":</span><span class="sxs-lookup"><span data-stu-id="e084e-156">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image14.jpg)

2. <span data-ttu-id="e084e-157">Aktivieren Sie als Nächstes Arbeitsspeicher wiederverwenden, indem Sie entweder auf "maximal verbrauchter Speicher (in MB):".</span><span class="sxs-lookup"><span data-stu-id="e084e-157">Next, enable Memory recycling by clicking on either 'Maximum used memory (in megabytes):'.</span></span> <span data-ttu-id="e084e-158">Der Wert darf nicht größer als die Menge des Arbeitsspeichers (nicht virtuell) auf dem Server sein, eine gute Näherung 60 % des physischen Arbeitsspeichers, d. h. für einen Server mit 512MB physischen Arbeitsspeicher Select 310.</span><span class="sxs-lookup"><span data-stu-id="e084e-158">The value should not be more than the amount of physical (not virtual) memory on the server, a good approximation is 60% of the physical memory, i.e. for a server with 512MB of physical memory select 310.</span></span> <span data-ttu-id="e084e-159">Es wird außerdem empfohlen, dass die maximale 800 MB nicht überschreiten, wenn Sie einen Adressraum von 2 GB verwenden.</span><span class="sxs-lookup"><span data-stu-id="e084e-159">It is also recommended that the maximum not exceed 800MB when using a 2GB address space.</span></span> <span data-ttu-id="e084e-160">Wenn der Speicherbereich für die Adresse des Servers/3 GB ist, kann die Höchstwert des Arbeitsspeichers für den Arbeitsprozess so hoch wie 1 800 MB sein:</span><span class="sxs-lookup"><span data-stu-id="e084e-160">If the memory address space of the server is 3GB, the maximum memory limit for the worker process can be as high as 1,800MB:</span></span>

![](aspnet-and-iis6/_static/image15.jpg)

<span data-ttu-id="e084e-161">Klicken Sie auf "Übernehmen" und "OK", um das Eigenschaftendialogfeld zu beenden.</span><span class="sxs-lookup"><span data-stu-id="e084e-161">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="e084e-162">Wiederholen Sie diesen Schritt für alle Anwendungspools verfügbar.</span><span class="sxs-lookup"><span data-stu-id="e084e-162">Repeat this for all available application pools.</span></span>

### <a name="configuring-worker-recycling"></a><span data-ttu-id="e084e-163">Konfigurieren von Worker-Wiederverwendung</span><span class="sxs-lookup"><span data-stu-id="e084e-163">Configuring worker recycling</span></span>

<span data-ttu-id="e084e-164">Standardmäßig wird IIS 6.0 konfiguriert, um einen Arbeitsprozess alle 29 Stunden durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="e084e-164">By default IIS 6.0 is configured to recycle its worker process every 29 hours.</span></span> <span data-ttu-id="e084e-165">Dies ist etwas aggressiver für eine Anwendung, die ASP.NET ausführt, und es wird empfohlen, dass automatische Arbeitsprozessen deaktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="e084e-165">This is a bit aggressive for an application running ASP.NET and it is recommended that automatic worker process recycling is disabled.</span></span>

<span data-ttu-id="e084e-166">Öffnen Sie zum Deaktivieren der automatischen Arbeitsprozessen zuerst Internetinformationsdienste-Manager (Start | Programme | Verwaltung | Internet Information Services).</span><span class="sxs-lookup"><span data-stu-id="e084e-166">To disable automatic worker process recycling, first open Internet Information Services Manager (Start | Programs | Administrative Tools | Internet Information Services).</span></span> <span data-ttu-id="e084e-167">Sobald geöffnet ist, erweitern Sie den Ordner "Anwendungspools":</span><span class="sxs-lookup"><span data-stu-id="e084e-167">Once open, expand the 'Application Pools' folder:</span></span>

![](aspnet-and-iis6/_static/image16.jpg)

<span data-ttu-id="e084e-168">Für jeden Anwendungspool:</span><span class="sxs-lookup"><span data-stu-id="e084e-168">For each application pool:</span></span>

1. <span data-ttu-id="e084e-169">Mit der rechten Maustaste auf den Anwendungspool, z. B. "DefaultAppPool", und wählen Sie "Eigenschaften":</span><span class="sxs-lookup"><span data-stu-id="e084e-169">Right-click on the application pool, e.g. 'DefaultAppPool', and select 'Properties':</span></span>

![](aspnet-and-iis6/_static/image17.jpg)

2. <span data-ttu-id="e084e-170">Deaktivieren Sie "Workerprozess wiederverwenden (in Minuten):":</span><span class="sxs-lookup"><span data-stu-id="e084e-170">Uncheck 'Recycle worker process (in minutes):':</span></span>

![](aspnet-and-iis6/_static/image18.jpg)

<span data-ttu-id="e084e-171">Klicken Sie auf "Übernehmen" und "OK", um das Eigenschaftendialogfeld zu beenden.</span><span class="sxs-lookup"><span data-stu-id="e084e-171">Click 'Apply' and the 'OK' to exit the properties dialog.</span></span> <span data-ttu-id="e084e-172">Wiederholen Sie diesen Schritt für alle Anwendungspools verfügbar.</span><span class="sxs-lookup"><span data-stu-id="e084e-172">Repeat this for all available application pools.</span></span>

## <a name="granting-write-access-to-the-file-system"></a><span data-ttu-id="e084e-173">Erteilen Schreibzugriff auf das Dateisystem</span><span class="sxs-lookup"><span data-stu-id="e084e-173">Granting write access to the file system</span></span>

<span data-ttu-id="e084e-174">Wenn Ihre Anwendung Schreibzugriff auf das Dateisystem benötigt und Sie NTFS verwenden, müssen Sie ein Zugriff Zugriffssteuerungsliste (ACL) auf den Ordner oder eine Datei zum Gewähren des Zugriffs von ASP.NET zu ändern.</span><span class="sxs-lookup"><span data-stu-id="e084e-174">If your application requires write access to the file system and you are using NTFS you will need to modify an Access Control List (ACL) on the folder or file to grant ASP.NET access to.</span></span>

<span data-ttu-id="e084e-175">Beispielsweise ASP.NET gewähren Schreibzugriff auf die c:\inetpub\wwwroot zunächst öffnen Sie Explorer und navigieren Sie zu dem Verzeichnis:</span><span class="sxs-lookup"><span data-stu-id="e084e-175">For example, to grant ASP.NET write access to the c:\inetpub\wwwroot first open explorer and navigate to the directory:</span></span>

![](aspnet-and-iis6/_static/image19.jpg)

<span data-ttu-id="e084e-176">Als Nächstes mit der rechten Maustaste auf das Verzeichnis, z. B. "Wwwroot" und wählen Sie Eigenschaften aus.</span><span class="sxs-lookup"><span data-stu-id="e084e-176">Next, right-click on the directory, e.g. 'wwwroot' and select properties.</span></span> <span data-ttu-id="e084e-177">Nachdem das Dialogfeld geöffnet wurde, wählen Sie die Registerkarte "Sicherheit":</span><span class="sxs-lookup"><span data-stu-id="e084e-177">After the properties dialog opens, select the 'Security' tab:</span></span>

![](aspnet-and-iis6/_static/image20.jpg)

<span data-ttu-id="e084e-178">Das Verzeichnis c:\inetpub\wwwroot\ ist eine spezielle Verzeichnis in, das die spezielle Gruppe von IIS 6.0 "IIS\_WPG' Read wurde bereits zugewiesen &amp; Berechtigungen ausführen, Ordnerinhalt auflisten und lesen.</span><span class="sxs-lookup"><span data-stu-id="e084e-178">The c:\inetpub\wwwroot\ directory is a special directory in that the special IIS 6.0 group 'IIS\_WPG' is already granted Read &amp; Execute, List Folder Contents, and Read permissions.</span></span> <span data-ttu-id="e084e-179">Um über die Schreibberechtigung erteilen, müssen Sie jedoch das Kontrollkästchen Zulassen klicken Sie zum Schreiben auf:</span><span class="sxs-lookup"><span data-stu-id="e084e-179">However, to grant Write permission, you need to click the Allow checkbox for Write:</span></span>

![](aspnet-and-iis6/_static/image21.jpg)

<span data-ttu-id="e084e-180">IIS 6.0 verfügt jetzt über die Schreibberechtigung für diesen Ordner.</span><span class="sxs-lookup"><span data-stu-id="e084e-180">IIS 6.0 now has write permission on this folder.</span></span> <span data-ttu-id="e084e-181">Gewähren Sie Schreibberechtigungen für andere Ordner folgendermaßen – Beachten Sie, müssen Sie möglicherweise IIS hinzufügen\_WPG-Gruppe, wenn sie nicht bereits vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="e084e-181">To grant write permissions on other folders, follow these steps - note, you may need to add the IIS\_WPG group if it does not already exist.</span></span>

> [!CAUTION]
> <span data-ttu-id="e084e-182">Erteilen von Schreibberechtigungen für IIS\_WPG lässt ASP.NET-Anwendung in dieses Verzeichnis geschrieben.</span><span class="sxs-lookup"><span data-stu-id="e084e-182">Granting write permission to IIS\_WPG will allow any ASP.NET application to write to this directory.</span></span>

## <a name="supporting-integrated-authentication-with-sql-server"></a><span data-ttu-id="e084e-183">Die integrierte Authentifizierung mit SQL Server unterstützen</span><span class="sxs-lookup"><span data-stu-id="e084e-183">Supporting integrated authentication with SQL Server</span></span>

<span data-ttu-id="e084e-184">Integrierte Authentifizierung ermöglicht für SQL Server, Windows NT-Authentifizierung zu nutzen, so überprüfen Sie die SQL Server-Anmeldekontos.</span><span class="sxs-lookup"><span data-stu-id="e084e-184">Integrated authentication allows for SQL Server to leverage Windows NT authentication to validate SQL Server logon accounts.</span></span> <span data-ttu-id="e084e-185">Dies ermöglicht es dem Benutzer, die standardmäßige SQL Server-Anmeldevorgang zu umgehen.</span><span class="sxs-lookup"><span data-stu-id="e084e-185">This allows the user to bypass the standard SQL Server logon process.</span></span> <span data-ttu-id="e084e-186">Bei diesem Ansatz kann ein Netzwerkbenutzers eine SQL Server-Datenbank zugreifen, ohne eine separate Anmelde-ID oder ein Kennwort angeben, da SQL Server aus dem Windows NT Network-Security-Prozess die Benutzer- und Kennwortinformationen erhält.</span><span class="sxs-lookup"><span data-stu-id="e084e-186">With this approach, a network user can access a SQL Server database without supplying a separate logon identification or password because SQL Server obtains the user and password information from the Windows NT network security process.</span></span>

<span data-ttu-id="e084e-187">Auswählen der integrierten Authentifizierung für ASP.NET-Anwendungen ist eine gute Wahl, da keine Anmeldeinformationen in der Verbindungszeichenfolge für die Anwendung gespeichert sind.</span><span class="sxs-lookup"><span data-stu-id="e084e-187">Choosing integrated authentication for ASP.NET applications is a good choice because no credentials are ever stored within your connection string for your application.</span></span> <span data-ttu-id="e084e-188">Stattdessen wird die Verbindungszeichenfolge zur Verbindung mit SQL wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="e084e-188">Rather the connection string used to connect to SQL will look as follows:</span></span>

`"server=localhost; database=Northwind;Trusted_Connection=true"`

<span data-ttu-id="e084e-189">Diese Verbindungszeichenfolge weist SQL Server die Windows-Anmeldeinformationen, der die Anwendung versucht, den Zugriff auf SQL Server verwenden.</span><span class="sxs-lookup"><span data-stu-id="e084e-189">This connection string tells SQL Server to use the Windows credentials of the application attempting to access SQL Server.</span></span> <span data-ttu-id="e084e-190">Im Fall von für ASP.NET/IIS werden erteilt 6 wäre dies ein Konto in der IIS-\_WPG-Gruppe.</span><span class="sxs-lookup"><span data-stu-id="e084e-190">In the case of ASP.NET/IIS 6 this would be an account in the IIS\_WPG group.</span></span>

<span data-ttu-id="e084e-191">Zum Aktivieren der integrierten Authentifizierung zwischen SQL Server und ASP.NET müssen Sie zunächst sicherstellen, dass SQL Server, für die integrierte Authentifizierung oder gemischten Authentifizierungsmodus konfiguriert ist - überprüfen Sie mit Ihrem DBA um dies zu ermitteln.</span><span class="sxs-lookup"><span data-stu-id="e084e-191">To enable integrated authentication between SQL Server and ASP.NET, you will need to first ensure that SQL Server is configured for either Integrated authentication or Mixed-Mode authentication - check with your DBA to determine this.</span></span> <span data-ttu-id="e084e-192">Wenn SQL Server in einem der folgenden beiden Modi ist, können Sie die integrierte Authentifizierung verwenden.</span><span class="sxs-lookup"><span data-stu-id="e084e-192">If SQL Server is in one of these two modes, you can use integrated authentication.</span></span>

<span data-ttu-id="e084e-193">Öffnen Sie SQL Server Enterprise Manager (Start | Programme | Microsoft SQL Server | Enterprise Manager), wählen Sie den entsprechenden Server, und erweitern Sie den Ordner Sicherheit:</span><span class="sxs-lookup"><span data-stu-id="e084e-193">Open SQL Server Enterprise Manager (Start | Programs | Microsoft SQL Server | Enterprise Manager), select the appropriate server, and expand the Security folder:</span></span>

![](aspnet-and-iis6/_static/image22.jpg)

<span data-ttu-id="e084e-194">Wenn "BUILTINT\IIS\_WPG" Gruppe nicht vorhanden ist, mit der rechten Maustaste auf Anmeldungen aus, und wählen Sie "Neue Anmeldung":</span><span class="sxs-lookup"><span data-stu-id="e084e-194">If 'BUILTINT\IIS\_WPG' group is not listed, right-click on Logins and select 'New Login':</span></span>

![](aspnet-and-iis6/_static/image23.jpg)

<span data-ttu-id="e084e-195">In der "Name:" Geben Sie entweder ein Textfeld "[Server/Domänenname] \IIS\_WPG", klicken Sie auf die Schaltfläche mit den Auslassungspunkten, um die Auswahl der Windows NT-Benutzer/Gruppe zu öffnen:</span><span class="sxs-lookup"><span data-stu-id="e084e-195">In the 'Name:' textbox either enter '[Server/Domain Name]\IIS\_WPG' or click on the ellipses button to open the Windows NT user/group picker:</span></span>

![](aspnet-and-iis6/_static/image24.jpg)

<span data-ttu-id="e084e-196">Wählen Sie den aktuellen Computer IIS\_WPG-Gruppe, und klicken Sie auf 'Hinzufügen', und OK, um die Auswahl zu schließen.</span><span class="sxs-lookup"><span data-stu-id="e084e-196">Select the current machine's IIS\_WPG group and click 'Add' and OK to close the picker.</span></span>

<span data-ttu-id="e084e-197">Sie müssen auch die Standarddatenbank und die Berechtigungen zum Zugriff auf die Datenbank festlegen.</span><span class="sxs-lookup"><span data-stu-id="e084e-197">You then need to also set the default database and the permissions to access the database.</span></span> <span data-ttu-id="e084e-198">Zum Festlegen der Standarddatenbank wählen Sie aus der Dropdownliste aus, z. B. unten Northwind ausgewählt ist:</span><span class="sxs-lookup"><span data-stu-id="e084e-198">To set the default database choose from the drop down list, e.g. below Northwind is selected:</span></span>

![](aspnet-and-iis6/_static/image25.jpg)

<span data-ttu-id="e084e-199">Klicken Sie dann auf der Registerkarte "Datenbankzugriff" auf:</span><span class="sxs-lookup"><span data-stu-id="e084e-199">Next, click on the Database Access tab:</span></span>

![](aspnet-and-iis6/_static/image26.jpg)

<span data-ttu-id="e084e-200">Klicken Sie auf das Kontrollkästchen "zulassen" für jede Datenbank, die Sie Zugriff auf zulassen möchten.</span><span class="sxs-lookup"><span data-stu-id="e084e-200">Click on the Permit checkbox for every database that you wish to allow access to.</span></span> <span data-ttu-id="e084e-201">Sie müssen auch Datenbankrollen auswählen Überprüfung Db\_Besitzer stellt sicher, dass die Anmeldung verfügt über alle erforderlichen Berechtigungen zum Verwalten und verwenden die ausgewählte Datenbank.</span><span class="sxs-lookup"><span data-stu-id="e084e-201">You will also need to select database roles, checking db\_owner will ensure your login has all necessary permissions to manage and use the selected database.</span></span>

<span data-ttu-id="e084e-202">Klicken Sie auf OK, um das Dialogfeld "Eigenschaft" zu beenden.</span><span class="sxs-lookup"><span data-stu-id="e084e-202">Click OK to exit the property dialog.</span></span> <span data-ttu-id="e084e-203">Ihre ASP.NET-Anwendung ist jetzt für die Unterstützung der integrierten SQL Server-Authentifizierung konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="e084e-203">Your ASP.NET application is now configured to support integrated SQL Server authentication.</span></span>

## <a name="dont-run-aspnet-10-in-iis-60-native-mode"></a><span data-ttu-id="e084e-204">Führen Sie ASP.NET 1.0 nicht im einheitlichen Modus von IIS 6.0</span><span class="sxs-lookup"><span data-stu-id="e084e-204">Don't run ASP.NET 1.0 in IIS 6.0 native mode</span></span>

<span data-ttu-id="e084e-205">ASP.NET 1.0 auf IIS 6.0 wird nur in IIS 5-Kompatibilitätsmodus unterstützt.</span><span class="sxs-lookup"><span data-stu-id="e084e-205">ASP.NET 1.0 on IIS 6.0 is only supported in IIS 5 compatibility mode.</span></span>

<span data-ttu-id="e084e-206">Zum Konfigurieren von ASP.NET 1.0 im IIS 5.0-Kompatibilitätsmodus ausgeführt öffnen Sie Internetdienste-Manager zu, und klicken Sie mit der rechten Maustaste auf Websites, und wählen Sie Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="e084e-206">To configure ASP.NET 1.0 to run in IIS 5.0 compatibility mode, open Internet Services Manager and right click Web Sites and select properties:</span></span>

![](aspnet-and-iis6/_static/image27.jpg)

<span data-ttu-id="e084e-207">Wechseln Sie zur Registerkarte "Dienst", und überprüfen? Ausführen WWW-Dienst im IIS 5.0-Isolationsmodus?</span><span class="sxs-lookup"><span data-stu-id="e084e-207">Switch to the Service Tab and check ?Run WWW Service in IIS 5.0 Isolation Mode?:</span></span>

![](aspnet-and-iis6/_static/image28.jpg)
