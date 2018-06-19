---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
title: Ein Benutzersteuerelement und JavaScript (VB) DynamicPopulate mit | Microsoft Docs
author: wenz
description: Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 778b9009-76f2-4665-940e-afc0e35bc917
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 715973ef4923e635ec2a860d00d55f13a5f8b2d4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873000"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-vb"></a><span data-ttu-id="b2ebe-103">Verwenden von DynamicPopulate mit einem Benutzersteuerelement und JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="b2ebe-103">Using DynamicPopulate with a User Control And JavaScript (VB)</span></span>
====================
<span data-ttu-id="b2ebe-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b2ebe-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b2ebe-105">[Herunterladen von Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="b2ebe-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2VB.pdf)</span></span>

> <span data-ttu-id="b2ebe-106">Das DynamicPopulate-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="b2ebe-107">Es ist auch möglich, die mit JavaScript-Code für benutzerdefinierte clientseitige Auffüllung auslösen.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="b2ebe-108">Besondere Sorgfalt muss jedoch ausgeführt werden, wenn der Extender in ein benutzerdefiniertes Steuerelement befindet.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="b2ebe-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="b2ebe-109">Overview</span></span>

<span data-ttu-id="b2ebe-110">Die `DynamicPopulate` -Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit Aufrufen eines Webdiensts (oder die Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement auf der Seite, ohne eine Seite-Aktualisierung.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="b2ebe-111">Es ist auch möglich, die mit JavaScript-Code für benutzerdefinierte clientseitige Auffüllung auslösen.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="b2ebe-112">Besondere Sorgfalt muss jedoch ausgeführt werden, wenn der Extender in ein benutzerdefiniertes Steuerelement befindet.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="b2ebe-113">Schritte</span><span class="sxs-lookup"><span data-stu-id="b2ebe-113">Steps</span></span>

<span data-ttu-id="b2ebe-114">Erstens, Sie benötigen ein ASP.NET-Webdienst implementiert die Methode aufgerufen werden, indem Sie die `DynamicPopulateExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="b2ebe-115">Der Webdienst implementiert die Methode `getDate()` erwartet ein Argument vom Typzeichenfolge aufgerufen `contextKey`, da die `DynamicPopulate` Steuerelement sendet ein Teil der Kontextinformationen mit jedem Aufruf der Web-Dienst.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="b2ebe-116">Hier ist der Code (Dateien `DynamicPopulate.vb.asmx`) die ruft des aktuellen Datums in einem der drei Formate:</span><span class="sxs-lookup"><span data-stu-id="b2ebe-116">Here is the code (files `DynamicPopulate.vb.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="b2ebe-117">Im nächsten Schritt erstellen Sie ein neues Benutzersteuerelement (`.ascx` Datei), indem Sie die folgende Deklaration in der ersten Zeile bezeichnet:</span><span class="sxs-lookup"><span data-stu-id="b2ebe-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="b2ebe-118">Ein &lt; `label` &gt; Element wird verwendet, um die Anzeige der Daten vom Server stammt.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample3.aspx)]

<span data-ttu-id="b2ebe-119">Auch in der Benutzersteuerelementdatei verwenden wir drei Optionsfelder, die jeweils einen der drei möglichen Datumsformate, die durch den Webdienst unterstützt darstellt.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="b2ebe-120">Klickt der Benutzer auf einem der Optionsfelder auf, wird der Browser JavaScript-Code ausführen, die wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="b2ebe-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample4.ps1)]

<span data-ttu-id="b2ebe-121">Dieser Code greift auf die `DynamicPopulateExtender` (ungewöhnliche ID keine Gedanken noch, dies wird später behandelt werden) und löst die dynamische Auffüllung mit Daten.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="b2ebe-122">Im Kontext des aktuellen Optionsfelds `this.value` bezieht sich auf den Wert also `format1`, `format2` oder `format3` genau wie die Webmethode erwartet.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="b2ebe-123">Das einzige noch fehlt im Benutzersteuerelement ist die `DynamicPopulateExtender` steuern, welche links von der Optionsfelder an den Webdienst.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="b2ebe-124">Erneut Beachten Sie möglicherweise die ungewöhnliche-ID, die im Steuerelement verwendete: `mcd1$myDate` anstelle von `myDate`.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="b2ebe-125">Zuvor den JavaScript-Code verwendet `mcd1_dpe1` für den Zugriff auf die `DynamicPopulateExtender` anstelle von `dpe1`. Diese Benennungsstrategie ist eine spezielle Anforderung bei Verwendung `DynamicPopulateExtender` innerhalb eines Benutzersteuerelements.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="b2ebe-126">Darüber hinaus müssen Sie die Benutzer-Verwaltungsgruppen in eine bestimmte Funktion, um damit alles funktioniert eingebettet werden sollen.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="b2ebe-127">Erstellen Sie eine neue ASP.NET-Seite, und registrieren Sie ein Tagpräfix für das Benutzersteuerelement, das Sie soeben implementiert haben:</span><span class="sxs-lookup"><span data-stu-id="b2ebe-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample6.aspx)]

<span data-ttu-id="b2ebe-128">Fügen Sie dann auf die ASP.NET AJAX `ScriptManager` Steuerelement auf der neuen Seite:</span><span class="sxs-lookup"><span data-stu-id="b2ebe-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample7.aspx)]

<span data-ttu-id="b2ebe-129">Fügen Sie schließlich das Benutzersteuerelement zur Seite.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="b2ebe-130">Nur festgelegt werden müssen, dessen `ID` Attribut (und `runat="server"`, natürlich), haben Sie aber auch auf einen bestimmten Namen festlegen: `mcd1` , da dies das Präfix in das Benutzersteuerelement für den Zugriff darauf mithilfe von JavaScript verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-vb/samples/sample8.aspx)]

<span data-ttu-id="b2ebe-131">Und das ist schon alles!</span><span class="sxs-lookup"><span data-stu-id="b2ebe-131">And that's it!</span></span> <span data-ttu-id="b2ebe-132">Die Seite verhält sich wie erwartet: ein Benutzer auf eines der Optionsfelder klickt, das Steuerelement im Toolkit den Webdienst aufruft, und das aktuelle Datum in das gewünschte Format angezeigt.</span><span class="sxs-lookup"><span data-stu-id="b2ebe-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="b2ebe-133">[![Die Optionsfelder befinden sich in einem Benutzersteuerelement](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b2ebe-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="b2ebe-134">Die Optionsfelder in einem Benutzersteuerelement befinden ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b2ebe-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b2ebe-135">Vorherige</span><span class="sxs-lookup"><span data-stu-id="b2ebe-135">Previous</span></span>](dynamically-populating-a-control-using-javascript-code-vb.md)
