---
uid: web-api/overview/security/integrated-windows-authentication
title: Integrierte Windows-Authentifizierung | Microsoft Docs
author: MikeWasson
description: Beschreibt die Verwendung von integrierter Windows-Authentifizierung in ASP.NET Web-API.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/18/2012
ms.topic: article
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: bf5f55d98d61cdfdd246a847f41a6f1c65f00bfc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="integrated-windows-authentication"></a><span data-ttu-id="c9880-103">Integrierte Windows-Authentifizierung</span><span class="sxs-lookup"><span data-stu-id="c9880-103">Integrated Windows Authentication</span></span>
====================
<span data-ttu-id="c9880-104">durch [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c9880-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c9880-105">Integrierte Windows-Authentifizierung kann Benutzer sich mit ihrer Windows-Anmeldeinformationen, die mit Kerberos oder NTLM anzumelden.</span><span class="sxs-lookup"><span data-stu-id="c9880-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="c9880-106">Der Client sendet die Anmeldeinformationen in der Authorization-Header.</span><span class="sxs-lookup"><span data-stu-id="c9880-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="c9880-107">Windows-Authentifizierung ist für Intranetumgebungen am besten geeignet.</span><span class="sxs-lookup"><span data-stu-id="c9880-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="c9880-108">Weitere Informationen finden Sie unter [Windows-Authentifizierung](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="c9880-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="c9880-109">Vorteile</span><span class="sxs-lookup"><span data-stu-id="c9880-109">Advantages</span></span> | <span data-ttu-id="c9880-110">Nachteile</span><span class="sxs-lookup"><span data-stu-id="c9880-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="c9880-111">-In IIS erstellt.</span><span class="sxs-lookup"><span data-stu-id="c9880-111">- Built into IIS.</span></span> <span data-ttu-id="c9880-112">-Die Anmeldeinformationen des Benutzers in der Anforderung sendet keine.</span><span class="sxs-lookup"><span data-stu-id="c9880-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="c9880-113">– Wenn der Clientcomputer mit der Domäne (z. B. Intranetanwendung) gehört, muss der Benutzer nicht um Anmeldeinformationen einzugeben.</span><span class="sxs-lookup"><span data-stu-id="c9880-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="c9880-114">-Für Internetanwendungen empfohlen nicht.</span><span class="sxs-lookup"><span data-stu-id="c9880-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="c9880-115">-Unterstützung für Kerberos oder NTLM in der Client erfordert.</span><span class="sxs-lookup"><span data-stu-id="c9880-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="c9880-116">-Client muss in Active Directory-Domäne sein.</span><span class="sxs-lookup"><span data-stu-id="c9880-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="c9880-117">Wenn Ihre Anwendung in Azure gehostet wird, und einer lokalen Active Directory-Domäne haben, sollten Sie Verbund Ihrer lokalen AD mit Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c9880-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="c9880-118">Auf diese Weise können Benutzer sich mit ihren lokalen Anmeldeinformationen anmelden, aber die Authentifizierung erfolgt durch Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c9880-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="c9880-119">Weitere Informationen finden Sie unter [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="c9880-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>


<span data-ttu-id="c9880-120">Wählen Sie zum Erstellen einer Anwendung, die integrierte Windows-Authentifizierung verwendet, die "Intranetanwendung"-Vorlage im Assistenten für MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c9880-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="c9880-121">Diese Projektvorlage fügt die folgende Einstellung in der Datei "Web.config":</span><span class="sxs-lookup"><span data-stu-id="c9880-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="c9880-122">Auf der Clientseite integrierte Windows-Authentifizierung funktioniert, mit jedem Browser, die unterstützt die [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) -Authentifizierungsschema, das meisten wichtigen Browser enthält.</span><span class="sxs-lookup"><span data-stu-id="c9880-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="c9880-123">Für .NET-Clientanwendungen die **"HttpClient"** Klasse unterstützt die Windows-Authentifizierung:</span><span class="sxs-lookup"><span data-stu-id="c9880-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="c9880-124">Windows-Authentifizierung ist anfällig für standortübergreifenden Angriffen Request (Forgery).</span><span class="sxs-lookup"><span data-stu-id="c9880-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="c9880-125">Finden Sie unter [verhindern von Angriffen Cross-Site Request (Forgery)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="c9880-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
