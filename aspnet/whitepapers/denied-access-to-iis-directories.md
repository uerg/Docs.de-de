---
uid: whitepapers/denied-access-to-iis-directories
title: ASP.NET – Verweigerung des Zugriffs auf IIS-Verzeichnisse | Microsoft-Dokumentation
author: rick-anderson
description: In diesem Whitepaper wird beschrieben, was Sie tun müssen, wenn eine Anforderung an die ASP.NET-Anwendung gibt, den Fehler "Zugriff verweigert Verzeichnisname-Verzeichnis zurück. Fehler beim s...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 3cb27b8a-354f-4332-bfe0-232b13bbf8aa
msc.legacyurl: /whitepapers/denied-access-to-iis-directories
msc.type: content
ms.openlocfilehash: c3a14f51df7aaf5c5935cf60ee4e687c10048e91
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835254"
---
<a name="aspnet-denied-access-to-iis-directories"></a><span data-ttu-id="0935a-104">ASP.NET – Verweigerung des Zugriffs auf IIS-Verzeichnisse</span><span class="sxs-lookup"><span data-stu-id="0935a-104">ASP.NET Denied Access to IIS Directories</span></span>
====================
> <span data-ttu-id="0935a-105">In diesem Whitepaper wird beschrieben, was Sie tun müssen, wenn eine Anforderung an Ihre ASP.NET-Anwendung den Fehler zurückgibt "verweigert den Zugriff auf *Verzeichnisname* Verzeichnis.</span><span class="sxs-lookup"><span data-stu-id="0935a-105">This whitepaper describes what you must do if a request to your ASP.NET application returns the error, "Denied Access to *DirectoryName* directory.</span></span> <span data-ttu-id="0935a-106">Fehler beim Starten der Überwachung von verzeichnisänderungen."</span><span class="sxs-lookup"><span data-stu-id="0935a-106">Failed to start monitoring directory changes."</span></span>
> 
> <span data-ttu-id="0935a-107">ASP.NET 1.1 zu ASP.NET 1.0 angewendet.</span><span class="sxs-lookup"><span data-stu-id="0935a-107">Applies to ASP.NET 1.0 and ASP.NET 1.1.</span></span>


<span data-ttu-id="0935a-108">ASP.NET V1 RTM lässt sich jetzt mit einem Less privilegiertes Windowskonto – als das Konto "ASPNET" auf einem lokalen Computer registriert ist.</span><span class="sxs-lookup"><span data-stu-id="0935a-108">ASP.NET V1 RTM now runs using a less privileged windows account - registered as the "ASPNET" account on a local machine.</span></span>

<span data-ttu-id="0935a-109">Auf einigen Systemen gesperrt dieses Konto möglicherweise nicht standardmäßig Security Zugriff auf Inhalte einer Website-Verzeichnisse, Stammverzeichnis der Anwendung oder das Stammverzeichnis der Website gelesen.</span><span class="sxs-lookup"><span data-stu-id="0935a-109">On some locked down systems, this account may not by default have read security access to a website's content directories, the application root directory, or the web site root directory.</span></span> <span data-ttu-id="0935a-110">In diesem Fall erhalten Sie den folgenden Fehler beim Anfordern von Seiten in einer bestimmten Webanwendung:</span><span class="sxs-lookup"><span data-stu-id="0935a-110">In this case you will receive the following error when requesting pages from a given web application:</span></span>

![](denied-access-to-iis-directories/_static/image1.jpg)

<span data-ttu-id="0935a-111">Zum Beheben dieses Problems müssen Sie die Sicherheitsberechtigungen für die entsprechenden Verzeichnisse zu ändern.</span><span class="sxs-lookup"><span data-stu-id="0935a-111">To fix this you will need to change the security permissions on the appropriate directories.</span></span>

<span data-ttu-id="0935a-112">Insbesondere ASP.NET erfordert, lesen, ausführen und den Zugriff für das ASPNET-Konto für den Websitestamm Liste (z. B.: c:\inetpub\wwwroot oder einem alternativen Standort-Verzeichnis, das Sie möglicherweise in IIS konfiguriert haben), das Inhaltsverzeichnis und Stammverzeichnis der Anwendung Um für Änderungen der Konfigurationsdateien zu überwachen.</span><span class="sxs-lookup"><span data-stu-id="0935a-112">Specifically, ASP.NET requires read, execute, and list access for the ASPNET account for the web site root (for example: c:\inetpub\wwwroot or any alternative site directory you may have configured in IIS), the content directory and the application root directory in order to monitor for configuration file changes.</span></span> <span data-ttu-id="0935a-113">Stammverzeichnis der Anwendung entspricht der Pfad des virtuellen Verzeichnisses in das IIS-Verwaltungstool (Inetmgr) zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="0935a-113">The application root corresponds to the folder path associated with the application virtual directory in the IIS Administration tool (inetmgr).</span></span>

<span data-ttu-id="0935a-114">Betrachten Sie beispielsweise die folgende Anwendungshierarchie unter dem Ordner "Wwwroot".</span><span class="sxs-lookup"><span data-stu-id="0935a-114">For example, consider the following application hierarchy under the wwwroot folder.</span></span>

`C:\inetpub\wwwroot\myapp\default.aspx`

<span data-ttu-id="0935a-115">In diesem Beispiel benötigt das ASPNET-Konto die Leseberechtigungen für den Inhalt in die "MyApp" und das Verzeichnis "Wwwroot" oben definierten.</span><span class="sxs-lookup"><span data-stu-id="0935a-115">For this example, the ASPNET account needs the read permissions defined above for content in both the myapp and the wwwroot directory.</span></span> <span data-ttu-id="0935a-116">Eine einzelne vererbte ACL für den Stammordner kann auch optional für beide Verzeichnisse verwendet werden, wenn sie geschachtelt sind.</span><span class="sxs-lookup"><span data-stu-id="0935a-116">A single inherited ACL on the root folder can also be optionally used for both directories if they're nested.</span></span>

<span data-ttu-id="0935a-117">Um Berechtigungen in ein Verzeichnis hinzuzufügen, führen Sie die folgenden Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="0935a-117">To add permissions to a directory, perform the following steps:</span></span>

- <span data-ttu-id="0935a-118">Mit dem Windows-Explorer, navigieren Sie zum Verzeichnis</span><span class="sxs-lookup"><span data-stu-id="0935a-118">Using the Windows explorer, navigate to the directory</span></span>
- <span data-ttu-id="0935a-119">Klicken Sie mit der rechten Maustaste auf den Ordner "Verzeichnis", und wählen Sie "Eigenschaften"</span><span class="sxs-lookup"><span data-stu-id="0935a-119">Right click on the directory folder and choose "Properties"</span></span>
- <span data-ttu-id="0935a-120">Navigieren Sie zur Registerkarte "Sicherheit" auf das Dialogfeld "Eigenschaft"</span><span class="sxs-lookup"><span data-stu-id="0935a-120">Navigate to the "Security" tab on the property dialog</span></span>
- <span data-ttu-id="0935a-121">Klicken Sie auf die Schaltfläche "Hinzufügen", und geben Sie den Computernamen gefolgt vom Namen ASPNET-Konto.</span><span class="sxs-lookup"><span data-stu-id="0935a-121">Click the "Add" button and enter the machine name followed by the ASPNET account name.</span></span> <span data-ttu-id="0935a-122">Beispielsweise würde Sie auf einem Computer mit dem Namen "Webdev" "", geben Sie Webdev\ASPNET und klicke auf "OK".</span><span class="sxs-lookup"><span data-stu-id="0935a-122">For example, on a machine named "webdev", you would enter webdev\ASPNET and hit "OK".</span></span>
- <span data-ttu-id="0935a-123">Stellen Sie sicher, dass das ASPNET-Konto verfügt über die "Read &amp; ausführen", "Ordnerinhalt auflisten" und "Lesen" Kontrollkästchen aktiviert.</span><span class="sxs-lookup"><span data-stu-id="0935a-123">Ensure that the ASPNET account has the "Read &amp; Execute", "List Folder Contents", and "Read" checkboxes checked.</span></span>
- <span data-ttu-id="0935a-124">Klicke auf OK, um das Dialogfeld zu schließen und speichern Sie die Änderungen.</span><span class="sxs-lookup"><span data-stu-id="0935a-124">Hit OK to dismiss the dialog and save the changes.</span></span>

![](denied-access-to-iis-directories/_static/image2.jpg)

<span data-ttu-id="0935a-125">Falls gewünscht, können diese Änderungen mithilfe von Skripts oder dem Tool "cacls.exe", das bereitgestellt wird, mit Windows automatisiert werden.</span><span class="sxs-lookup"><span data-stu-id="0935a-125">If desired, these changes can be automated using scripts or the "cacls.exe" tool that ships with Windows.</span></span> <span data-ttu-id="0935a-126">Weitere Informationen über das ASPNET-Konto, finden Sie unter den [FAQ-Dokument](https://go.microsoft.com/fwlink/?LinkId=5828).</span><span class="sxs-lookup"><span data-stu-id="0935a-126">For more information on the ASPNET account, please see the [FAQ document](https://go.microsoft.com/fwlink/?LinkId=5828).</span></span>

<span data-ttu-id="0935a-127">Wenn eine bestimmte Webanwendung basiert auf schreiben, oder Ändern von Berechtigungen für die Datei oder eines bestimmten Ordners, kann dies durch die gleichen Schritte aus, und überprüfen die Kontrollkästchen für "Write" und/oder "Ändern" erteilt werden.</span><span class="sxs-lookup"><span data-stu-id="0935a-127">If a given web application relies on having write or modify permissions to a particular folder or file, this can be granted by following the same procedure and checking the "Write" and/or "Modify" checkboxes.</span></span>

<span data-ttu-id="0935a-128">Auf Computern, die alle Benutzer oder den Benutzern Gruppe den Zugriff auf diese Verzeichnisse (was die Standardkonfiguration ist) ermöglichen, werden keine Probleme auftreten, und die oben genannten Schritte nicht erforderlich sein.</span><span class="sxs-lookup"><span data-stu-id="0935a-128">On machines that allow Everyone or the Users group read access to these directories (which is the default configuration), no issues will be encountered and the above steps will not be required.</span></span>
