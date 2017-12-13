---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET IIS-Verzeichnisse Zugriff verweigert | Microsoft Docs
author: rick-anderson
description: "In diesem Whitepaper wird beschrieben, was Sie tun müssen, wenn eine Anforderung an die ASP.NET-Anwendung, der Fehler \"Zugriff verweigert DirectoryName Verzeichnis zurückgegeben. Fehler beim s..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: 64118ac7a5f280775106d2dc7636923b08f28d89
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="b184f-104">ASP.NET IIS-Verzeichnisse der Zugriff verweigert</span><span class="sxs-lookup"><span data-stu-id="b184f-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="b184f-105">In diesem Whitepaper wird beschrieben, was Sie tun müssen, wenn eine Anforderung an die ASP.NET-Anwendung, den Fehler zurückgegeben "Zugriff auf verweigert *DirectoryName* Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="b184f-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="b184f-106">Fehler beim Starten der Überwachung Directory Chaanges."</span><span class="sxs-lookup"><span data-stu-id="b184f-106">Failed to start monitoring directory chaanges."</span></span>
> 
> <span data-ttu-id="b184f-107">Gilt für ASP.NET 1.0 und ASP.NET 1.1.</span><span class="sxs-lookup"><span data-stu-id="b184f-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="b184f-108">ASP.NET V1 RTM jetzt wird ausgeführt, die eine weniger privilegierte Windowskonto – als das Konto "ASPNET" auf einem lokalen Computer registriert.</span><span class="sxs-lookup"><span data-stu-id="b184f-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="b184f-109">Auf einige Systeme gesperrt dieses Konto möglicherweise nicht standardmäßig Zugriff auf eine Website Verzeichnisse, das Stammverzeichnis der Anwendung oder das Stammverzeichnis der Website gelesen.</span><span class="sxs-lookup"><span data-stu-id="b184f-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="b184f-110">In diesem Fall erhalten Sie die folgende Fehlermeldung beim Anfordern von Seiten aus einer bestimmten Webanwendung:</span><span class="sxs-lookup"><span data-stu-id="b184f-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="b184f-111">Um dieses Problem zu beheben, müssen Sie die Sicherheitsberechtigungen für die entsprechenden Verzeichnisse zu ändern.</span><span class="sxs-lookup"><span data-stu-id="b184f-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="b184f-112">Insbesondere ASP.NET erfordert, lesen, ausführen und Listenzugriff für das ASPNET-Konto für das Stammverzeichnis der Website (z. B.: c:\inetpub\wwwroot oder einem alternativen Standort-Verzeichnis, die Sie möglicherweise in IIS konfiguriert haben), das Inhaltsverzeichnis und Stammverzeichnis der Anwendung Um bei Änderungen der Konfigurationsdateien überwacht werden.</span><span class="sxs-lookup"><span data-stu-id="b184f-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="b184f-113">Das Stammverzeichnis der Anwendung entspricht der Ordnerpfad, der das virtuelle Verzeichnis der Anwendung in der IIS-Verwaltungstool (Inetmgr) zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="b184f-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="b184f-114">Betrachten Sie beispielsweise die folgende Anwendungshierarchie unter dem Ordner "Wwwroot" aus.</span><span class="sxs-lookup"><span data-stu-id="b184f-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="b184f-115">In diesem Beispiel benötigt das ASPNET-Konto der Lese-und Schreibberechtigungen für Inhalte in der "MyApp" und das Verzeichnis "Wwwroot" oben definiert.</span><span class="sxs-lookup"><span data-stu-id="b184f-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="b184f-116">Eine einzelne vererbte ACL im Stammordner kann auch optional für beide Verzeichnisse verwendet werden, wenn sie geschachtelt sind.</span><span class="sxs-lookup"><span data-stu-id="b184f-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="b184f-117">Um Berechtigungen in ein Verzeichnis hinzuzufügen, führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="b184f-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="b184f-118">Mit dem Windows-Explorer, navigieren Sie zum Verzeichnis</span><span class="sxs-lookup"><span data-stu-id="b184f-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="b184f-119">Klicken Sie mit der rechten Maustaste auf den Ordner, und wählen Sie "Eigenschaften"</span><span class="sxs-lookup"><span data-stu-id="b184f-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="b184f-120">Navigieren Sie zur Registerkarte "Sicherheit" auf das Dialogfeld "Eigenschaft"</span><span class="sxs-lookup"><span data-stu-id="b184f-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="b184f-121">Klicken Sie auf die Schaltfläche "Hinzufügen", und geben Sie den Computernamen gefolgt vom Namen ASPNET-Konto.</span><span class="sxs-lookup"><span data-stu-id="b184f-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="b184f-122">Auf einem Computer mit dem Namen "Webdev" "", würden Sie z. B. Webdev\ASPNET eingeben und "OK" erreicht.</span><span class="sxs-lookup"><span data-stu-id="b184f-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="b184f-123">Stellen Sie sicher, dass das ASPNET-Konto verfügt über das "Read &amp; ausführen", "Ordnerinhalt auflisten" und "Lesen" Kontrollkästchen aktiviert.</span><span class="sxs-lookup"><span data-stu-id="b184f-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="b184f-124">Drücken Sie OK, um das Dialogfeld zu schließen und speichern Sie die Änderungen.</span><span class="sxs-lookup"><span data-stu-id="b184f-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="b184f-125">Falls gewünscht, können diese Änderungen mithilfe von Skripts oder "cacls.exe"-Tool, das geliefert wird mit Windows automatisiert werden.</span><span class="sxs-lookup"><span data-stu-id="b184f-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="b184f-126">Weitere Informationen über das ASPNET-Konto, finden Sie unter der [FAQ Dokument](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="b184f-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="b184f-127">Wenn eine bestimmte Webanwendung davon abhängig, ob Schreibzugriff oder Ändern von Berechtigungen für einen bestimmten Ordner oder eine Datei, kann dies durch das gleiche Verfahren, und überprüfen die Kontrollkästchen für "Write" und/oder "Ändern" erteilt werden.</span><span class="sxs-lookup"><span data-stu-id="b184f-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="b184f-128">Auf Computern, die es ermöglichen, "Jeder" oder die Benutzergruppe lesen Zugriff auf diese Verzeichnisse (Dies ist die Standardkonfiguration), keine Probleme auftreten, und ist die oben genannten Schritte nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="b184f-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
