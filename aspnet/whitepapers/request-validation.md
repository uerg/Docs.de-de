---
uid: whitepapers/request-validation
title: Anforderungsvalidierung - Script-Angriffe verhindert | Microsoft Docs
author: rick-anderson
description: Dieses Dokument beschreibt die Anforderung Überprüfungsfunktion von ASP.NET, wird standardmäßig die Anwendung daran gehindert wird Verarbeitung nicht codierte HTML-Inhalt senden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 0b24fe2193d2c7a858667505bad9ed0b1d70a328
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/30/2018
ms.locfileid: "28883554"
---
<a name="request-validation---preventing-script-attacks"></a><span data-ttu-id="f5890-103">Anforderungsvalidierung - Script-Angriffe zu verhindern</span><span class="sxs-lookup"><span data-stu-id="f5890-103">Request Validation - Preventing Script Attacks</span></span>
====================
> <span data-ttu-id="f5890-104">Dieses Whitepaper beschreibt die Anforderung Überprüfungsfunktion von ASP.NET, wird standardmäßig die Anwendung daran gehindert wird Verarbeitung nicht codierte HTML-Inhalt an den Server gesendet.</span><span class="sxs-lookup"><span data-stu-id="f5890-104">This paper describes the request validation feature of ASP.NET where, by default, the application is prevented from processing unencoded HTML content submitted to the server.</span></span> <span data-ttu-id="f5890-105">Diese Anforderung Überprüfungsfunktion kann deaktiviert werden, wenn die Anwendung entwickelt wurde, um HTML-Daten sicher zu verarbeiten.</span><span class="sxs-lookup"><span data-stu-id="f5890-105">This request validation feature can be disabled when the application has been designed to safely process HTML data.</span></span>
> 
> <span data-ttu-id="f5890-106">Gilt für ASP.NET 1.1 und ASP.NET 2.0.</span><span class="sxs-lookup"><span data-stu-id="f5890-106">Applies to ASP.NET 1.1 and ASP.NET 2.0.</span></span>


<span data-ttu-id="f5890-107">Anforderungsüberprüfung, eine Funktion von ASP.NET seit Version 1.1, wird verhindert, dass den Server akzeptiert Inhalts enthaltendes uncodierten HTML.</span><span class="sxs-lookup"><span data-stu-id="f5890-107">Request validation, a feature of ASP.NET since version 1.1, prevents the server from accepting content containing un-encoded HTML.</span></span> <span data-ttu-id="f5890-108">Dieses Feature wurde entwickelt, um zu verhindern einige Script-Injection-Angriffen, bei dem Client-Skriptcode oder HTML kann werden unbewusst mit einem Server übermittelt, gespeichert und dann an andere Benutzer angezeigt.</span><span class="sxs-lookup"><span data-stu-id="f5890-108">This feature is designed to help prevent some script-injection attacks whereby client script code or HTML can be unknowingly submitted to a server, stored, and then presented to other users.</span></span> <span data-ttu-id="f5890-109">Es wird weiterhin dringend empfohlen, Sie überprüfen, dass alle Eingabedaten und die HTML-Codierung, ihn gegebenenfalls.</span><span class="sxs-lookup"><span data-stu-id="f5890-109">We still strongly recommend that you validate all input data and HTML encode it when appropriate.</span></span>

<span data-ttu-id="f5890-110">Beispielsweise erstellen Sie eine Webseite, die anfordert, die e-Mail-Adresse eines Benutzers und speichert dann, die e-Mail-Adresse in einer Datenbank.</span><span class="sxs-lookup"><span data-stu-id="f5890-110">For example, you create a Web page that requests a user's email address and then stores that email address in a database.</span></span> <span data-ttu-id="f5890-111">Wenn der Benutzer eingibt, &lt;Skript&gt;Warnung ("Hello aus dem Skript")&lt;/SCRIPT&gt; anstatt eine gültige e-Mail-Adresse, wenn diese Daten dargestellt werden, dieses Skript kann ausgeführt werden, wenn der Inhalt nicht ordnungsgemäß codiert wurde.</span><span class="sxs-lookup"><span data-stu-id="f5890-111">If the user enters &lt;SCRIPT&gt;alert("hello from script")&lt;/SCRIPT&gt; instead of a valid email address, when that data is presented, this script can be executed if the content was not properly encoded.</span></span> <span data-ttu-id="f5890-112">Die Anforderung Überprüfungsfunktion von ASP.NET wird dies verhindert.</span><span class="sxs-lookup"><span data-stu-id="f5890-112">The request validation feature of ASP.NET prevents this from happening.</span></span>

## <a name="why-this-feature-is-useful"></a><span data-ttu-id="f5890-113">Warum diese Funktion hilfreich ist.</span><span class="sxs-lookup"><span data-stu-id="f5890-113">Why this feature is useful</span></span>

<span data-ttu-id="f5890-114">Viele Websites sind nicht beachten Sie, dass sie für einfache Skript-Injection-Angriffe geöffnet sind.</span><span class="sxs-lookup"><span data-stu-id="f5890-114">Many sites are not aware that they are open to simple script injection attacks.</span></span> <span data-ttu-id="f5890-115">Der Zweck dieser Art von Angriffen die Website durch Anzeigen von HTML handelt oder potenziell Clientskripts zum Umleiten des Benutzers ein Hacker Standort ausgeführt ist, sind Script-Injection-Angriffe ein Problem, das Webentwickler bewältigen müssen.</span><span class="sxs-lookup"><span data-stu-id="f5890-115">Whether the purpose of these attacks is to deface the site by displaying HTML, or to potentially execute client script to redirect the user to a hacker's site, script injection attacks are a problem that Web developers must contend with.</span></span>

<span data-ttu-id="f5890-116">Script-Injection-Angriffe sind Besorgnis alle Webentwickler, unabhängig davon, ob sie ASP.NET, ASP oder andere webtechnologien für die Entwicklung sind.</span><span class="sxs-lookup"><span data-stu-id="f5890-116">Script injection attacks are a concern of all web developers, whether they are using ASP.NET, ASP, or other web development technologies.</span></span>

<span data-ttu-id="f5890-117">Das Feature zur Validierung von ASP.NET Anforderung verhindert proaktiv diese Angriffe durch nicht codierte HTML-Inhalte vom Server verarbeitet werden, es sei denn, der Entwickler entschieden hat, um zuzulassen, dass der Inhalt nicht zugelassen.</span><span class="sxs-lookup"><span data-stu-id="f5890-117">The ASP.NET request validation feature proactively prevents these attacks by not allowing unencoded HTML content to be processed by the server unless the developer decides to allow that content.</span></span>

## <a name="what-to-expect-error-page"></a><span data-ttu-id="f5890-118">Was Sie erwartet: Fehler (Seite)</span><span class="sxs-lookup"><span data-stu-id="f5890-118">What to expect: Error Page</span></span>

<span data-ttu-id="f5890-119">Der Screenshot zeigt einige Beispiel-ASP-Code:</span><span class="sxs-lookup"><span data-stu-id="f5890-119">The screen shot below shows some sample ASP.NET code:</span></span>

![](request-validation/_static/image1.png)

<span data-ttu-id="f5890-120">Dieser Code Ergebnisse in eine einfache Seite, die Ihnen ermöglicht, geben Sie Text im Textfeld ausgeführt werden, klicken Sie auf die Schaltfläche und zeigt den Text in das Label-Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="f5890-120">Running this code results in a simple page that allows you to enter some text in the textbox, click the button, and display the text in the label control:</span></span>

![](request-validation/_static/image2.png)

<span data-ttu-id="f5890-121">Wurden jedoch JavaScript, z. B. `<script>alert("hello!")</script>` eingegeben und erhalten wir eine Ausnahme übermittelt werden:</span><span class="sxs-lookup"><span data-stu-id="f5890-121">However, were JavaScript, such as `<script>alert("hello!")</script>` to be entered and submitted we would get an exception:</span></span>

![](request-validation/_static/image3.png)

<span data-ttu-id="f5890-122">Die Fehlermeldung gibt an, dass eine "potenziell gefährlichen Request.Form Wert erkannt wurde." und erhalten Sie weitere Informationen in der Beschreibung zu genau den Vorfall und das Verhalten zu ändern.</span><span class="sxs-lookup"><span data-stu-id="f5890-122">The error message states that a 'potentially dangerous Request.Form value was detected' and provides more details in the description as to exactly what occurred and how to change the behavior.</span></span> <span data-ttu-id="f5890-123">Zum Beispiel:</span><span class="sxs-lookup"><span data-stu-id="f5890-123">For example:</span></span>

<span data-ttu-id="f5890-124">Anforderungsüberprüfung hat einen potenziell gefährliche Client Eingabewert erkannt, und die Verarbeitung der Anforderung wurde abgebrochen.</span><span class="sxs-lookup"><span data-stu-id="f5890-124">Request validation has detected a potentially dangerous client input value, and processing of the request has been aborted.</span></span> <span data-ttu-id="f5890-125">Dieser Wert zeigt möglicherweise einen Versuch gefährdet die Sicherheit Ihrer Anwendung, z. B. einen Cross-Site scripting-Angriff an.</span><span class="sxs-lookup"><span data-stu-id="f5890-125">This value may indicate an attempt to compromise the security of your application, such as a cross-site scripting attack.</span></span> <span data-ttu-id="f5890-126">Sie können die Anforderungsvalidierung deaktivieren, indem `validateRequest=false` in der Seitendirektive oder im Konfigurationsabschnitt.</span><span class="sxs-lookup"><span data-stu-id="f5890-126">You can disable request validation by setting `validateRequest=false` in the Page directive or in the configuration section.</span></span> <span data-ttu-id="f5890-127">Es wird jedoch dringend empfohlen, dass die Anwendung explizit in diesem Fall alle Eingaben überprüfen.</span><span class="sxs-lookup"><span data-stu-id="f5890-127">However, it is strongly recommended that your application explicitly check all inputs in this case.</span></span>

## <a name="disabling-request-validation-on-a-page"></a><span data-ttu-id="f5890-128">Deaktivieren der Anforderungsvalidierung auf einer Seite</span><span class="sxs-lookup"><span data-stu-id="f5890-128">Disabling request validation on a page</span></span>

<span data-ttu-id="f5890-129">Um Anforderungsvalidierung auf einer Seite zu deaktivieren, müssen Sie festlegen, der `validateRequest` -Attribut der Seitendirektive auf `false`:</span><span class="sxs-lookup"><span data-stu-id="f5890-129">To disable request validation on a page you must set the `validateRequest` attribute of the Page directive to `false`:</span></span>

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> <span data-ttu-id="f5890-130">Bei der anforderungsüberprüfung deaktiviert ist, kann Inhalte auf eine Seite gesendet werden; Es ist die Verantwortung des Entwicklers Seite um sicherzustellen, dass der Inhalt korrekt codiert oder verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="f5890-130">When request validation is disabled, content can be submitted to a page; it is the responsibility of the page developer to ensure that content is properly encoded or processed.</span></span>

## <a name="disabling-request-validation-for-your-application"></a><span data-ttu-id="f5890-131">Deaktivieren der Anforderungsvalidierung für Ihre Anwendung</span><span class="sxs-lookup"><span data-stu-id="f5890-131">Disabling request validation for your application</span></span>

<span data-ttu-id="f5890-132">Um die Anforderungsvalidierung für Ihre Anwendung zu deaktivieren, müssen Sie ändern oder erstellen Sie eine Datei "Web.config" für Ihre Anwendung und legen Sie das Attribut ValidateRequest der `<pages />` Abschnitt `false`:</span><span class="sxs-lookup"><span data-stu-id="f5890-132">To disable request validation for your application, you must modify or create a Web.config file for your application and set the validateRequest attribute of the `<pages />` section to `false`:</span></span>

[!code-xml[Main](request-validation/samples/sample2.xml)]

<span data-ttu-id="f5890-133">Wenn Sie die Anforderungsvalidierung für alle Anwendungen auf dem Server deaktivieren möchten, können Sie diese Änderung der Datei "Machine.config" vornehmen.</span><span class="sxs-lookup"><span data-stu-id="f5890-133">If you wish to disable request validation for all applications on your server, you can make this modification to your Machine.config file.</span></span>

> [!CAUTION]
> <span data-ttu-id="f5890-134">Bei der anforderungsüberprüfung deaktiviert ist, kann Inhalte für Ihre Anwendung gesendet werden; Es ist die Verantwortung des Anwendungsentwicklers, um sicherzustellen, dass der Inhalt korrekt codiert oder verarbeitet wird.</span><span class="sxs-lookup"><span data-stu-id="f5890-134">When request validation is disabled, content can be submitted to your application; it is the responsibility of the application developer to ensure that content is properly encoded or processed.</span></span>

<span data-ttu-id="f5890-135">Im folgenden Code wird geändert, um die Anforderungsvalidierung zu deaktivieren:</span><span class="sxs-lookup"><span data-stu-id="f5890-135">The code below is modified to turn off request validation:</span></span>

![](request-validation/_static/image4.png)

<span data-ttu-id="f5890-136">Wenn Sie der folgenden JavaScript-Code in das Textfeld eingegeben wurde nun `<script>alert("hello!")</script>` wäre das Ergebnis:</span><span class="sxs-lookup"><span data-stu-id="f5890-136">Now if the following JavaScript was entered into the textbox `<script>alert("hello!")</script>` the result would be:</span></span>

![](request-validation/_static/image5.png)

<span data-ttu-id="f5890-137">Um dies zu vermeiden, mit der Anforderungsvalidierung deaktiviert zu verhindern, müssen wir HTML-Codierung Inhalt.</span><span class="sxs-lookup"><span data-stu-id="f5890-137">To prevent this from happening, with request validation turned off, we need to HTML encode the content.</span></span>

## <a name="how-to-html-encode-content"></a><span data-ttu-id="f5890-138">Wie in HTML codieren Inhalt</span><span class="sxs-lookup"><span data-stu-id="f5890-138">How to HTML encode content</span></span>

<span data-ttu-id="f5890-139">Wenn Sie die Anforderungsvalidierung deaktiviert haben, ist es empfiehlt sich, HTML-codieren Inhalte, die für die zukünftige Verwendung gespeichert werden soll.</span><span class="sxs-lookup"><span data-stu-id="f5890-139">If you have disabled request validation, it is good practice to HTML-encode content that will be stored for future use.</span></span> <span data-ttu-id="f5890-140">HTML-Codierung wird automatisch ersetzen "&lt;'oder'&gt;' (zusammen mit mehreren anderen Symbolen) mit ihren entsprechenden HTML-codierte Darstellung.</span><span class="sxs-lookup"><span data-stu-id="f5890-140">HTML encoding will automatically replace any ‘&lt;' or ‘&gt;' (together with several other symbols) with their corresponding HTML encoded representation.</span></span> <span data-ttu-id="f5890-141">Z. B. "&lt;"wird ersetzt durch"&amp;Lt;" und "&gt;"wird ersetzt durch"&amp;Gt;".</span><span class="sxs-lookup"><span data-stu-id="f5890-141">For example, ‘&lt;' is replaced by ‘&amp;lt;' and ‘&gt;' is replaced by ‘&amp;gt;'.</span></span> <span data-ttu-id="f5890-142">Browser verwenden diese speziellen Codes zum Anzeigen der "&lt;'oder'&gt;" im Browser.</span><span class="sxs-lookup"><span data-stu-id="f5890-142">Browsers use these special codes to display the ‘&lt;' or ‘&gt;' in the browser.</span></span>

<span data-ttu-id="f5890-143">Inhalt kann problemlos HTML-codiert sein, auf dem Server mithilfe der `Server.HtmlEncode(string)` API.</span><span class="sxs-lookup"><span data-stu-id="f5890-143">Content can be easily HTML-encoded on the server using the `Server.HtmlEncode(string)` API.</span></span> <span data-ttu-id="f5890-144">Inhalt kann auch sein problemlos HTML-decodiert, d. h. auf standard HTML mit zurückgesetzt die `Server.HtmlDecode(string)` Methode.</span><span class="sxs-lookup"><span data-stu-id="f5890-144">Content can also be easily HTML-decoded, that is, reverted back to standard HTML using the `Server.HtmlDecode(string)` method.</span></span>

![](request-validation/_static/image6.png)

<span data-ttu-id="f5890-145">Hatte:</span><span class="sxs-lookup"><span data-stu-id="f5890-145">Resulting in:</span></span>

![](request-validation/_static/image7.png)
