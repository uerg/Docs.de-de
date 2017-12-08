---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Ein Benutzersteuerelement und JavaScript (c#) DynamicPopulate mit | Microsoft Docs
author: wenz
description: "Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 3f78da3898d44cc2cf1db6623ebcaf6a73ebbf3e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a><span data-ttu-id="da290-103">Verwenden von DynamicPopulate mit einem Benutzersteuerelement und JavaScript (c#)</span><span class="sxs-lookup"><span data-stu-id="da290-103">Using DynamicPopulate with a User Control And JavaScript (C#)</span></span>
====================
<span data-ttu-id="da290-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="da290-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="da290-105">[Herunterladen von Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="da290-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span></span>

> <span data-ttu-id="da290-106">Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung.</span><span class="sxs-lookup"><span data-stu-id="da290-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="da290-107">Es ist auch möglich, die mit JavaScript-Code für benutzerdefinierte clientseitige Auffüllung auslösen.</span><span class="sxs-lookup"><span data-stu-id="da290-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="da290-108">Besondere Sorgfalt muss jedoch ausgeführt werden, wenn der Extender in ein benutzerdefiniertes Steuerelement befindet.</span><span class="sxs-lookup"><span data-stu-id="da290-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="da290-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="da290-109">Overview</span></span>

<span data-ttu-id="da290-110">Die `DynamicPopulate` -Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung.</span><span class="sxs-lookup"><span data-stu-id="da290-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="da290-111">Es ist auch möglich, die mit JavaScript-Code für benutzerdefinierte clientseitige Auffüllung auslösen.</span><span class="sxs-lookup"><span data-stu-id="da290-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="da290-112">Besondere Sorgfalt muss jedoch ausgeführt werden, wenn der Extender in ein benutzerdefiniertes Steuerelement befindet.</span><span class="sxs-lookup"><span data-stu-id="da290-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="da290-113">Schritte</span><span class="sxs-lookup"><span data-stu-id="da290-113">Steps</span></span>

<span data-ttu-id="da290-114">Erstens, Sie benötigen ein ASP.NET-Webdienst implementiert die Methode aufgerufen werden, indem Sie die `DynamicPopulateExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="da290-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="da290-115">Der Webdienst implementiert die Methode `getDate()` erwartet ein Argument vom Typzeichenfolge aufgerufen `contextKey`, da die `DynamicPopulate` Steuerelement sendet ein Teil der Kontextinformationen mit jedem Aufruf der Web-Dienst.</span><span class="sxs-lookup"><span data-stu-id="da290-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="da290-116">Hier ist der Code (Datei `DynamicPopulate.cs.asmx`) die ruft des aktuellen Datums in einem der drei Formate:</span><span class="sxs-lookup"><span data-stu-id="da290-116">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="da290-117">Im nächsten Schritt erstellen Sie ein neues Benutzersteuerelement (`.ascx` Datei), indem Sie die folgende Deklaration in der ersten Zeile bezeichnet:</span><span class="sxs-lookup"><span data-stu-id="da290-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="da290-118">Ein &lt; `label` &gt; Element wird verwendet, um die Anzeige der Daten vom Server stammt.</span><span class="sxs-lookup"><span data-stu-id="da290-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

<span data-ttu-id="da290-119">Auch in der Benutzersteuerelementdatei verwenden wir drei Optionsfelder, die jeweils einen der drei möglichen Datumsformate, die durch den Webdienst unterstützt darstellt.</span><span class="sxs-lookup"><span data-stu-id="da290-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="da290-120">Klickt der Benutzer auf einem der Optionsfelder auf, wird der Browser JavaScript-Code ausführen, die wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="da290-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

<span data-ttu-id="da290-121">Dieser Code greift auf die `DynamicPopulateExtender` (ungewöhnliche ID keine Gedanken noch, dies wird später behandelt werden) und löst die dynamische Auffüllung mit Daten.</span><span class="sxs-lookup"><span data-stu-id="da290-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="da290-122">Im Kontext des aktuellen Optionsfelds `this.value` bezieht sich auf den Wert also `format1`, `format2` oder `format3` genau wie die Webmethode erwartet.</span><span class="sxs-lookup"><span data-stu-id="da290-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="da290-123">Das einzige noch fehlt im Benutzersteuerelement ist die `DynamicPopulateExtender` steuern, welche links von der Optionsfelder an den Webdienst.</span><span class="sxs-lookup"><span data-stu-id="da290-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="da290-124">Erneut Beachten Sie möglicherweise die ungewöhnliche-ID, die im Steuerelement verwendete: `mcd1$myDate` anstelle von `myDate`.</span><span class="sxs-lookup"><span data-stu-id="da290-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="da290-125">Zuvor den JavaScript-Code verwendet `mcd1_dpe1` für den Zugriff auf die `DynamicPopulateExtender` anstelle von `dpe1`. Diese Benennungsstrategie ist eine spezielle Anforderung bei Verwendung `DynamicPopulateExtender` innerhalb eines Benutzersteuerelements.</span><span class="sxs-lookup"><span data-stu-id="da290-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="da290-126">Darüber hinaus müssen Sie die Benutzer-Verwaltungsgruppen in eine bestimmte Funktion, um damit alles funktioniert eingebettet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="da290-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="da290-127">Erstellen Sie eine neue ASP.NET-Seite, und registrieren Sie ein Tagpräfix für das Benutzersteuerelement, das Sie soeben implementiert haben:</span><span class="sxs-lookup"><span data-stu-id="da290-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

<span data-ttu-id="da290-128">Fügen Sie dann auf die ASP.NET AJAX `ScriptManager` Steuerelement auf der neuen Seite:</span><span class="sxs-lookup"><span data-stu-id="da290-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

<span data-ttu-id="da290-129">Fügen Sie schließlich das Benutzersteuerelement zur Seite.</span><span class="sxs-lookup"><span data-stu-id="da290-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="da290-130">Nur festgelegt werden müssen, dessen `ID` Attribut (und `runat="server"`, natürlich), haben Sie aber auch auf einen bestimmten Namen festlegen: `mcd1` , da dies das Präfix in das Benutzersteuerelement für den Zugriff darauf mithilfe von JavaScript verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="da290-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

<span data-ttu-id="da290-131">Und das ist schon alles!</span><span class="sxs-lookup"><span data-stu-id="da290-131">And that's it!</span></span> <span data-ttu-id="da290-132">Die Seite verhält sich wie erwartet: ein Benutzer klickt auf auf der die Optionsfelder, die das Steuerelement im Toolkit den Webdienst aufruft, und das aktuelle Datum in das gewünschte Format angezeigt.</span><span class="sxs-lookup"><span data-stu-id="da290-132">The page behaves as expected: A user clicks on on of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="da290-133">[![Die Optionsfelder befinden sich in einem Benutzersteuerelement](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="da290-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="da290-134">Die Optionsfelder in einem Benutzersteuerelement befinden ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="da290-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="da290-135">[Zurück](dynamically-populating-a-control-using-javascript-code-cs.md)
[Weiter](dynamically-populating-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="da290-135">[Previous](dynamically-populating-a-control-using-javascript-code-cs.md)
[Next](dynamically-populating-a-control-vb.md)</span></span>
