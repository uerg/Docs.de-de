---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Der Farbwähler Extendersteuerelement (VB) mit | Microsoft Docs
author: microsoft
description: Farbwähler wird ein ASP.NET AJAX-Extender, der die clientseitige Farbe Kommissionierung Funktionalität mit Benutzeroberfläche in einem Popupsteuerelement bereitstellt. Es kann ASP.NET hinzugefügt werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: 3411119f85d7f5c26703b7df40cff24fdf30b81d
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="using-the-colorpicker-control-extender-vb"></a><span data-ttu-id="bc847-104">Mithilfe der Farbwähler Extendersteuerelement (VB)</span><span class="sxs-lookup"><span data-stu-id="bc847-104">Using the ColorPicker Control Extender (VB)</span></span>
====================
<span data-ttu-id="bc847-105">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="bc847-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="bc847-106">Farbwähler wird ein ASP.NET AJAX-Extender, der die clientseitige Farbe Kommissionierung Funktionalität mit Benutzeroberfläche in einem Popupsteuerelement bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="bc847-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="bc847-107">Sie können auf jedes ASP.NET TextBox-Steuerelement angefügt werden.</span><span class="sxs-lookup"><span data-stu-id="bc847-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="bc847-108">Es ist.</span><span class="sxs-lookup"><span data-stu-id="bc847-108">It.</span></span>


<span data-ttu-id="bc847-109">Ziel dieses Lernprogramms wird erläutert, wie Sie die AJAX-Steuerelement-Toolkit Farbwähler Extendersteuerelement verwenden können.</span><span class="sxs-lookup"><span data-stu-id="bc847-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="bc847-110">Der Farbwähler Extendersteuerelement zeigt ein Popupdialogfeld, das Ihnen ermöglicht, eine Farbe auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="bc847-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="bc847-111">Der Farbwähler ist hilfreich, wenn Sie eine intuitive Benutzeroberfläche für einen Benutzer auf eine Farbe auswählen bereitstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="bc847-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="bc847-112">Ein TextBox-Steuerelement mit dem Farbwähler Extendersteuerelement erweitern</span><span class="sxs-lookup"><span data-stu-id="bc847-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="bc847-113">Stellen Sie sich vor, z. B., dass eine Website zu erstellen, die zum Erstellen von benutzerdefinierten Visitenkarten Besucher ermöglicht werden soll.</span><span class="sxs-lookup"><span data-stu-id="bc847-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="bc847-114">Besucher können Geben Sie den Text für eine Business-Karte und die Farbe auswählen.</span><span class="sxs-lookup"><span data-stu-id="bc847-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="bc847-115">Die ASP.NET-Seite in Codebeispiel 1 enthält zwei TextBox-Steuerelemente, die mit dem Namen TxtCardText und TxtCardColor.</span><span class="sxs-lookup"><span data-stu-id="bc847-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="bc847-116">Wenn Sie bei der Übermittlung des Formulars werden die ausgewählten Werte angezeigt (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="bc847-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="bc847-117">[![Einfaches Formular zum Erstellen einer Business-Karte](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bc847-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)</span></span>

<span data-ttu-id="bc847-118">**Abbildung 01**: einfaches Formular zum Erstellen einer Visitenkarte ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-colorpicker-control-extender-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="bc847-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image2.png))</span></span>


<span data-ttu-id="bc847-119">**1 – CreateCard.aspx auflisten**</span><span class="sxs-lookup"><span data-stu-id="bc847-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

<span data-ttu-id="bc847-120">Das Formular im Codebeispiel 1 funktioniert, aber es bietet eine hervorragende Benutzeroberfläche keine.</span><span class="sxs-lookup"><span data-stu-id="bc847-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="bc847-121">Der Benutzer hat eine Farbe in das Textfeld eingeben.</span><span class="sxs-lookup"><span data-stu-id="bc847-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="bc847-122">Wenn der Benutzer eine spezielle Farbe - möchte muss z. B. nur die rechte Schattierung Pea Grün- und der Benutzer die HTML-Farbcode ohne keine Hilfe herauszufinden.</span><span class="sxs-lookup"><span data-stu-id="bc847-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="bc847-123">Der Farbwähler Extendersteuerelement können Sie um eine bessere benutzererfahrung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="bc847-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="bc847-124">Farbwähler wird ein Dialogfeld Farbe angezeigt, wenn Sie den Fokus auf ein TextBox-Steuerelement verschieben (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="bc847-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="bc847-125">[![Der Farbwähler Extendersteuerelement](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="bc847-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)</span></span>

<span data-ttu-id="bc847-126">**Abbildung 02**: der Farbwähler Extendersteuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-colorpicker-control-extender-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="bc847-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image4.png))</span></span>


<span data-ttu-id="bc847-127">Sie müssen zwei Schritte ausführen, um das Extendersteuerelement Farbwähler mit dem Formular im Codebeispiel 1 verwenden:</span><span class="sxs-lookup"><span data-stu-id="bc847-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="bc847-128">Fügen Sie ein ScriptManager-Steuerelement auf der Seite "</span><span class="sxs-lookup"><span data-stu-id="bc847-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="bc847-129">Der Farbwähler Extendersteuerelement auf der Seite hinzufügen</span><span class="sxs-lookup"><span data-stu-id="bc847-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="bc847-130">Bevor Sie Farbwähler verwenden können, müssen Sie ein ScriptManager auf der Seite hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="bc847-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="bc847-131">Ein guter Ausgangspunkt ScriptManager hinzufügen wird rechts unterhalb der öffnenden serverseitige &lt;Formular&gt; Tag.</span><span class="sxs-lookup"><span data-stu-id="bc847-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="bc847-132">Sie können die ScriptManager auf der Seite "(ein ScriptManager befindet sich unter der Registerkarte" AJAX-Erweiterungen ") aus der Toolbox ziehen.</span><span class="sxs-lookup"><span data-stu-id="bc847-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="bc847-133">Alternativ können Sie das folgende Tag in der Datenquellensicht unter serverseitige Formular Starttag eingeben:</span><span class="sxs-lookup"><span data-stu-id="bc847-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="bc847-134">&lt;ASP: ScriptManager-ID = "ScriptManager1" Runat = "Server" /&gt;</span><span class="sxs-lookup"><span data-stu-id="bc847-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="bc847-135">Die einfachste Möglichkeit, das Extendersteuerelement Farbwähler zur Seite hinzuzufügen, ist in der Entwurfsansicht.</span><span class="sxs-lookup"><span data-stu-id="bc847-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="bc847-136">Wenn Sie Ihre Maus auf das Textfeld TxtCardColor zeigen, eine Automatisches Option wird angezeigt, das ermöglicht Sie keinen Extender hinzufügen (siehe Abbildung 3).</span><span class="sxs-lookup"><span data-stu-id="bc847-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="bc847-137">Wenn Sie diese Option auswählen, erscheint der Extender-Assistent (siehe Abbildung 4).</span><span class="sxs-lookup"><span data-stu-id="bc847-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="bc847-138">[![Einen Extender hinzufügen](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="bc847-138">[![Adding an extender](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)</span></span>

<span data-ttu-id="bc847-139">**Abbildung 03**: Hinzufügen eines Extenders ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-colorpicker-control-extender-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="bc847-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image6.png))</span></span>


<span data-ttu-id="bc847-140">[![Ein Extendersteuerelement mit dem Assistenten für Extender auswählen](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="bc847-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)</span></span>

<span data-ttu-id="bc847-141">**Abbildung 04**: ein Extendersteuerelement mit dem Assistenten für Extender auswählen ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-colorpicker-control-extender-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="bc847-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image8.png))</span></span>


<span data-ttu-id="bc847-142">Sie können den Farbwähler Extender zum Erweitern der TxtCardColor Textfeld mit dem Farbwähler Extender auswählen.</span><span class="sxs-lookup"><span data-stu-id="bc847-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="bc847-143">Klicken Sie auf OK, um das Dialogfeld zu schließen.</span><span class="sxs-lookup"><span data-stu-id="bc847-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="bc847-144">Nachdem Sie diese Änderungen vorgenommen haben, sieht die Quelle für die Seite 2 aufgelistet.</span><span class="sxs-lookup"><span data-stu-id="bc847-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="bc847-145">**Auflisten von 2 – CreateCard.aspx (mit Farbwähler)**</span><span class="sxs-lookup"><span data-stu-id="bc847-145">**Listing 2 - CreateCard.aspx (with ColorPicker)**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

<span data-ttu-id="bc847-146">Beachten Sie, dass die Seite jetzt ein ColorPickerExtender-Steuerelement enthält, die direkt unterhalb der TxtCardColor TextBox-Steuerelement angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="bc847-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="bc847-147">Das Steuerelement ColorPickerExtender erweitert TxtCardColor-Steuerelement, sodass sie ein Dialogfeld für die Farbauswahl angezeigt.</span><span class="sxs-lookup"><span data-stu-id="bc847-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="bc847-148">Verwenden einer Schaltfläche, um das Dialogfeld für die Farbauswahl zu starten.</span><span class="sxs-lookup"><span data-stu-id="bc847-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="bc847-149">Der Farbwähler Extender unterstützt die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="bc847-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="bc847-150">PopupButtonId - die ID einer Schaltfläche auf der Seite, die bewirkt, dass das Dialogfeld für die Farbauswahl angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="bc847-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="bc847-151">PopupPosition - Position relativ zum Zielsteuerelement, der das Dialogfeld für die Farbauswahl.</span><span class="sxs-lookup"><span data-stu-id="bc847-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="bc847-152">Mögliche Werte sind Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, rechts und Links (der Standardwert ist BottomLeft).</span><span class="sxs-lookup"><span data-stu-id="bc847-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="bc847-153">SampleControlId - die ID eines Steuerelements, in der ausgewählte Farbe angezeigt.</span><span class="sxs-lookup"><span data-stu-id="bc847-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="bc847-154">SelectedColor - Ausgangsfarbe, die vom Farbwähler ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="bc847-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="bc847-155">Diese Eigenschaften können Sie anpassen, wie das Dialogfeld für die Farbauswahl angezeigt wird und wie die ausgewählte Farbe angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="bc847-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="bc847-156">Die Seite im Codebeispiel 3 wird veranschaulicht, wie Sie einige dieser Eigenschaften.</span><span class="sxs-lookup"><span data-stu-id="bc847-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="bc847-157">**3 – CreateCardButton.aspx auflisten**</span><span class="sxs-lookup"><span data-stu-id="bc847-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

<span data-ttu-id="bc847-158">Die Seite im Codebeispiel 3 enthält eine Farbe auswählen Schaltfläche (siehe Abbildung 5).</span><span class="sxs-lookup"><span data-stu-id="bc847-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="bc847-159">Wenn Sie auf diese Schaltfläche klicken, wird das Dialogfeld für die Farbauswahl über das Textfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="bc847-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="bc847-160">Wenn Sie eine Farbe aus dem Dialogfeld auswählen wird die ausgewählte Farbe als die Farbe des Hintergrunds der LblSample Label-Steuerelement angezeigt.</span><span class="sxs-lookup"><span data-stu-id="bc847-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="bc847-161">Die ColorPicker PopupButtonID-Eigenschaft wird verwendet, die Schaltfläche "Farbe auswählen" den Farbwähler Extender zugeordnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="bc847-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="bc847-162">Wenn Sie einen Wert für die PopupButtonID-Eigenschaft angeben, wird das Dialogfeld für die Farbauswahl nicht mehr angezeigt, wenn das Zielsteuerelement den Fokus besitzt.</span><span class="sxs-lookup"><span data-stu-id="bc847-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="bc847-163">Sie müssen die Schaltfläche, um das Dialogfeld anzeigen klicken.</span><span class="sxs-lookup"><span data-stu-id="bc847-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="bc847-164">Die SampleControlID-Eigenschaft wird verwendet, um ein Steuerelement zuzuordnen, in dem die ausgewählte Farbe mit Farbwähler angezeigt.</span><span class="sxs-lookup"><span data-stu-id="bc847-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="bc847-165">Der Farbwähler ändert die Hintergrundfarbe des Steuerelements, die zurzeit ausgewählte Farbe an.</span><span class="sxs-lookup"><span data-stu-id="bc847-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="bc847-166">[![Das Dialogfeld für die Farbauswahl mit einer Schaltfläche anzeigen](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="bc847-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)</span></span>

<span data-ttu-id="bc847-167">**Abbildung 05**: das Dialogfeld für die Farbauswahl mit einer Schaltfläche anzeigen ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-colorpicker-control-extender-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="bc847-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="bc847-168">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="bc847-168">Summary</span></span>

<span data-ttu-id="bc847-169">In diesem Lernprogramm haben Sie gelernt, wie das Extendersteuerelement Farbwähler zu verwenden, um ein Dialogfeld für die Farbauswahl Popup anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="bc847-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="bc847-170">Zunächst können wir untersucht, wie Sie das Dialogfeld anzeigen, wenn ein TextBox-Steuerelement den Fokus verschoben wird.</span><span class="sxs-lookup"><span data-stu-id="bc847-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="bc847-171">Als Nächstes haben Sie gelernt, wie Sie eine Schaltfläche zu erstellen, die in dem Dialogfeld für die Farbauswahl angezeigt, auf die Schaltfläche geklickt wird.</span><span class="sxs-lookup"><span data-stu-id="bc847-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="bc847-172">Vorherige</span><span class="sxs-lookup"><span data-stu-id="bc847-172">Previous</span></span>](using-the-colorpicker-control-extender-cs.md)
