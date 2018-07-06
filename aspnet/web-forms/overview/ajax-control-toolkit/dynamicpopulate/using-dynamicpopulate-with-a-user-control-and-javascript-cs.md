---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Verwenden von DynamicPopulate mit einem Benutzersteuerelement und JavaScript (c#) | Microsoft-Dokumentation
author: wenz
description: Das DynamicPopulate-Steuerelement in ASP.NET AJAX Control Toolkit Aufrufe eines Webdiensts (oder eine Seitenmethode) und füllt den resultierenden Wert in ein Zielsteuerelement, auf t...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 3a380f24a521153cba084de21014380df49efabd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835171"
---
<a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a><span data-ttu-id="3abb3-103">Verwenden von DynamicPopulate mit einem Benutzersteuerelement und JavaScript (c#)</span><span class="sxs-lookup"><span data-stu-id="3abb3-103">Using DynamicPopulate with a User Control And JavaScript (C#)</span></span>
====================
<span data-ttu-id="3abb3-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="3abb3-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="3abb3-105">[Code herunterladen](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="3abb3-105">[Download Code](http://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)</span></span>

> <span data-ttu-id="3abb3-106">Das DynamicPopulate-Steuerelement in ASP.NET AJAX Control Toolkit Aufrufe eines Webdiensts (oder eine Seitenmethode) und füllt den resultierenden Wert in einem Zielsteuerelement auf der Seite, ohne eine seitenaktualisierung.</span><span class="sxs-lookup"><span data-stu-id="3abb3-106">The DynamicPopulate control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="3abb3-107">Es ist auch möglich, um die Auffüllung mithilfe von benutzerdefinierten JavaScript-Code für die clientseitige auszulösen.</span><span class="sxs-lookup"><span data-stu-id="3abb3-107">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="3abb3-108">Jedoch besondere Sorgfalt ausgeführt werden, wenn der Extender in einem Benutzersteuerelement befinden muss.</span><span class="sxs-lookup"><span data-stu-id="3abb3-108">However special care has to be taken when the extender resides in a user control.</span></span>


## <a name="overview"></a><span data-ttu-id="3abb3-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="3abb3-109">Overview</span></span>

<span data-ttu-id="3abb3-110">Die `DynamicPopulate` -Steuerelement in ASP.NET AJAX Control Toolkit Aufrufe eines Webdiensts (oder eine Seitenmethode), und füllt den resultierenden Wert in einem Zielsteuerelement auf der Seite, ohne eine seitenaktualisierung.</span><span class="sxs-lookup"><span data-stu-id="3abb3-110">The `DynamicPopulate` control in the ASP.NET AJAX Control Toolkit calls a web service (or page method) and fills the resulting value into a target control on the page, without a page refresh.</span></span> <span data-ttu-id="3abb3-111">Es ist auch möglich, um die Auffüllung mithilfe von benutzerdefinierten JavaScript-Code für die clientseitige auszulösen.</span><span class="sxs-lookup"><span data-stu-id="3abb3-111">It is also possible to trigger the population using custom client-side JavaScript code.</span></span> <span data-ttu-id="3abb3-112">Jedoch besondere Sorgfalt ausgeführt werden, wenn der Extender in einem Benutzersteuerelement befinden muss.</span><span class="sxs-lookup"><span data-stu-id="3abb3-112">However special care has to be taken when the extender resides in a user control.</span></span>

## <a name="steps"></a><span data-ttu-id="3abb3-113">Schritte</span><span class="sxs-lookup"><span data-stu-id="3abb3-113">Steps</span></span>

<span data-ttu-id="3abb3-114">Zunächst einmal benötigen Sie eines ASP.NET-Webdiensts die implementiert wird, die Methode aufgerufen werden, indem Sie die `DynamicPopulateExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="3abb3-114">First of all, you need an ASP.NET Web Service which implements the method to be called by the `DynamicPopulateExtender` control.</span></span> <span data-ttu-id="3abb3-115">Der Webdienst implementiert, die Methode `getDate()` erwartet ein Argument vom Typzeichenfolge, mit dem Namen `contextKey`, da die `DynamicPopulate` Steuerelement sendet ein Teil der Informationen zum Sitzungskontext mit jeder Aufruf des Webdiensts.</span><span class="sxs-lookup"><span data-stu-id="3abb3-115">The web service implements the method `getDate()` that expects one argument of type string, called `contextKey`, since the `DynamicPopulate` control sends one piece of context information with each web service call.</span></span> <span data-ttu-id="3abb3-116">Hier ist der Code (Datei `DynamicPopulate.cs.asmx`) die ruft des aktuellen Datums in der folgenden drei Formaten:</span><span class="sxs-lookup"><span data-stu-id="3abb3-116">Here is the code (file `DynamicPopulate.cs.asmx`) which retrieves the current date in one of three formats:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

<span data-ttu-id="3abb3-117">Im nächsten Schritt erstellen Sie ein neues Benutzersteuerelement (`.ascx` Datei), indem Sie die folgende Deklaration in der ersten Zeile bezeichnet:</span><span class="sxs-lookup"><span data-stu-id="3abb3-117">In the next step, create a new user control (`.ascx` file), denoted by the following declaration in its first line:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

<span data-ttu-id="3abb3-118">Ein &lt; `label` &gt; Element zum Anzeigen der Daten vom Server verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="3abb3-118">A &lt;`label`&gt; element will be used to display the data coming from the server.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

<span data-ttu-id="3abb3-119">Auch in die Benutzersteuerelement-Datei verwenden wir drei Optionsfeldern, die jeweils einen der drei mögliche vom Webdienst unterstütztes Datumsformat darstellt.</span><span class="sxs-lookup"><span data-stu-id="3abb3-119">Also in the user control file, we will use three radio buttons, each one representing one of the three possible date formats supported by the web service.</span></span> <span data-ttu-id="3abb3-120">Klickt der Benutzer auf eines der Optionsfelder auf, wird der Browser JavaScript-Code ausführen, die wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="3abb3-120">When the user clicks on one of the radio buttons, the browser will execute JavaScript code which looks like this:</span></span>

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

<span data-ttu-id="3abb3-121">Dieser Code greift auf die `DynamicPopulateExtender` (aber keine Sorge, über die ungewöhnliche-ID noch, dies wird später behandelt) und löst die dynamische Auffüllung mit Daten.</span><span class="sxs-lookup"><span data-stu-id="3abb3-121">This code accesses the `DynamicPopulateExtender` (do not worry about the strange ID yet, this will be covered later on) and triggers the dynamic population with data.</span></span> <span data-ttu-id="3abb3-122">Im Kontext des aktuellen Optionsfelds `this.value` bezieht sich auf den Wert handelt es sich `format1`, `format2` oder `format3` genau wie die Webmethode erwartet.</span><span class="sxs-lookup"><span data-stu-id="3abb3-122">In the context of the current radio button, `this.value` refers to its value which is `format1`, `format2` or `format3` exactly what the web method expects.</span></span>

<span data-ttu-id="3abb3-123">Ist das einzige, was fehlt im Steuerelement noch die `DynamicPopulateExtender` steuern, welche die Optionsfelder mit dem Webdienst verknüpft ist.</span><span class="sxs-lookup"><span data-stu-id="3abb3-123">The only thing missing in the user control yet is the `DynamicPopulateExtender` control which links the radio buttons to the web service.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

<span data-ttu-id="3abb3-124">Sie können erneut Notieren Sie die ungewöhnliche ID, die im Steuerelement verwendete: `mcd1$myDate` anstelle von `myDate`.</span><span class="sxs-lookup"><span data-stu-id="3abb3-124">Again you may note the strange ID used in the control: `mcd1$myDate` instead of `myDate`.</span></span> <span data-ttu-id="3abb3-125">Zuvor den JavaScript-Code verwendet `mcd1_dpe1` für den Zugriff auf die `DynamicPopulateExtender` anstelle von `dpe1`. Diese Benennungskonvention Strategie ist eine besondere Anforderung, bei Verwendung `DynamicPopulateExtender` in einem Benutzersteuerelement.</span><span class="sxs-lookup"><span data-stu-id="3abb3-125">Previously, the JavaScript code used `mcd1_dpe1` to access the `DynamicPopulateExtender` instead of `dpe1`.This naming strategy is a special requirement when using `DynamicPopulateExtender` within a user control.</span></span> <span data-ttu-id="3abb3-126">Darüber hinaus müssen Sie die Benutzer-Verwaltungsgruppen in bestimmte Funktion, um alle dafür erforderlichen einzubetten.</span><span class="sxs-lookup"><span data-stu-id="3abb3-126">Furthermore, you have to embed the user contol in a specific way to make it all work.</span></span> <span data-ttu-id="3abb3-127">Erstellen Sie eine neue ASP.NET-Seite und registrieren Sie ein Tagpräfix für das Benutzersteuerelement, die, das Sie gerade implementiert haben:</span><span class="sxs-lookup"><span data-stu-id="3abb3-127">Create a new ASP.NET page and register a tag prefix for the user control you have just implemented:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

<span data-ttu-id="3abb3-128">Schließen Sie dann auf der ASP.NET AJAX `ScriptManager` Steuerelement auf der neuen Seite:</span><span class="sxs-lookup"><span data-stu-id="3abb3-128">Then, include the ASP.NET AJAX `ScriptManager` control on the new page:</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

<span data-ttu-id="3abb3-129">Fügen Sie abschließend das Benutzersteuerelement auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="3abb3-129">Finally, add the user control to the page.</span></span> <span data-ttu-id="3abb3-130">Sie müssen nur Festlegen der `ID` Attribut (und `runat="server"`und natürlich), Sie haben jedoch auch zu einem bestimmten Namen festlegen: `mcd1` da dies das Präfix in das Benutzersteuerelement verwendet, um die Zugriffsberechtigung mithilfe von JavaScript ist.</span><span class="sxs-lookup"><span data-stu-id="3abb3-130">You only have to set its `ID` attribute (and `runat="server"`, of course), but you also have to set it to a specific name: `mcd1` since this is the prefix used within the user control to access it using JavaScript.</span></span>

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

<span data-ttu-id="3abb3-131">Und das ist schon alles!</span><span class="sxs-lookup"><span data-stu-id="3abb3-131">And that's it!</span></span> <span data-ttu-id="3abb3-132">Die Seite wie erwartet verhält: ein Benutzer klickt auf eines der Optionsfelder, die das Steuerelement im Toolkit den Webdienst aufruft, und das aktuelle Datum im gewünschten Format angezeigt.</span><span class="sxs-lookup"><span data-stu-id="3abb3-132">The page behaves as expected: A user clicks on one of the radio buttons, the control in the Toolkit calls the web service and displays the current date in the desired format.</span></span>


<span data-ttu-id="3abb3-133">[![Die Optionsfelder befinden sich in einem Benutzersteuerelement](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="3abb3-133">[![The radio buttons reside in a user control](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)</span></span>

<span data-ttu-id="3abb3-134">Die Optionsfelder in einem Benutzersteuerelement befinden ([klicken Sie, um das Bild in voller Größe anzeigen](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="3abb3-134">The radio buttons reside in a user control ([Click to view full-size image](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3abb3-135">[Zurück](dynamically-populating-a-control-using-javascript-code-cs.md)
> [Weiter](dynamically-populating-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="3abb3-135">[Previous](dynamically-populating-a-control-using-javascript-code-cs.md)
[Next](dynamically-populating-a-control-vb.md)</span></span>
