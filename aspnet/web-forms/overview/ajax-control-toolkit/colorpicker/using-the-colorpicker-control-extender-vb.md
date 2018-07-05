---
uid: web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
title: Verwendung des ColorPicker-Steuerelement-Extenders (VB) | Microsoft-Dokumentation
author: microsoft
description: ColorPicker ist ein ASP.NET AJAX-Extender, der clientseitige Farbe auswählen Funktionalität mit Benutzeroberfläche in einem Popupsteuerelement enthält. Sie können jeder ASP.NET angefügt werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 577ae07b-a872-4818-a804-bca489b40ad0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/colorpicker/using-the-colorpicker-control-extender-vb
msc.type: authoredcontent
ms.openlocfilehash: cad012dd1ce93714ecb127bf3543d5c65803aba9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374178"
---
<a name="using-the-colorpicker-control-extender-vb"></a><span data-ttu-id="13229-104">Verwendung des ColorPicker-Steuerelement-Extenders (VB)</span><span class="sxs-lookup"><span data-stu-id="13229-104">Using the ColorPicker Control Extender (VB)</span></span>
====================
<span data-ttu-id="13229-105">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="13229-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="13229-106">ColorPicker ist ein ASP.NET AJAX-Extender, der clientseitige Farbe auswählen Funktionalität mit Benutzeroberfläche in einem Popupsteuerelement enthält.</span><span class="sxs-lookup"><span data-stu-id="13229-106">ColorPicker is an ASP.NET AJAX extender that provides client-side color-picking functionality with UI in a popup control.</span></span> <span data-ttu-id="13229-107">Sie können auf jedes ASP.NET TextBox-Steuerelement angefügt werden.</span><span class="sxs-lookup"><span data-stu-id="13229-107">It can be attached to any ASP.NET TextBox control.</span></span> <span data-ttu-id="13229-108">Es ist.</span><span class="sxs-lookup"><span data-stu-id="13229-108">It.</span></span>


<span data-ttu-id="13229-109">Das Ziel in diesem Tutorial wird beschrieben, wie Sie den AJAX Control Toolkit ColorPicker-Steuerelement-Extender verwenden können.</span><span class="sxs-lookup"><span data-stu-id="13229-109">The goal of this tutorial is to explain how you can use the AJAX Control Toolkit ColorPicker control extender.</span></span> <span data-ttu-id="13229-110">ColorPicker-Steuerelement-Extender zeigt ein Popupdialogfeld, das Ihnen ermöglicht, eine Farbe auszuwählen.</span><span class="sxs-lookup"><span data-stu-id="13229-110">The ColorPicker control extender displays a popup dialog that enables you to select a color.</span></span> <span data-ttu-id="13229-111">Die ColorPicker ist nützlich, wenn Sie eine intuitive Benutzeroberfläche, damit ein Benutzer eine Farbe auswählen bereitstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="13229-111">The ColorPicker is useful whenever you want to provide an intuitive user interface for a user to pick a color.</span></span>

## <a name="extending-a-textbox-control-with-the-colorpicker-control-extender"></a><span data-ttu-id="13229-112">Ein TextBox-Steuerelement mit dem ColorPicker-Steuerelement-Extender erweitern</span><span class="sxs-lookup"><span data-stu-id="13229-112">Extending a TextBox Control with the ColorPicker Control Extender</span></span>

<span data-ttu-id="13229-113">Stellen Sie sich vor, z. B., dass eine Website zu erstellen, die Besucher zum Erstellen von benutzerdefinierten Visitenkarten ermöglicht werden soll.</span><span class="sxs-lookup"><span data-stu-id="13229-113">Imagine, for example, that you want to create a website that enables visitors to create customized business cards.</span></span> <span data-ttu-id="13229-114">Besucher können Geben Sie den Text für eine Visitenkarte und die Farbe auswählen.</span><span class="sxs-lookup"><span data-stu-id="13229-114">Visitors can enter the text for a business card and pick the color.</span></span> <span data-ttu-id="13229-115">Die ASP.NET-Seite in Codebeispiel 1 enthält zwei TextBox-Steuerelemente, die mit dem Namen TxtCardText und TxtCardColor.</span><span class="sxs-lookup"><span data-stu-id="13229-115">The ASP.NET page in Listing 1 contains two TextBox controls named txtCardText and txtCardColor.</span></span> <span data-ttu-id="13229-116">Wenn Sie das Formular übermitteln, werden die ausgewählten Werte angezeigt (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="13229-116">When you submit the form, the selected values are displayed (see Figure 1).</span></span>


<span data-ttu-id="13229-117">[![Einfaches Formular zum Erstellen einer Visitenkarte](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="13229-117">[![Simple form for creating a business card](using-the-colorpicker-control-extender-vb/_static/image1.jpg)](using-the-colorpicker-control-extender-vb/_static/image1.png)</span></span>

<span data-ttu-id="13229-118">**Abbildung 01**: einfache Formular zum Erstellen einer Visitenkarte ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-colorpicker-control-extender-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="13229-118">**Figure 01**: Simple form for creating a business card ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image2.png))</span></span>


<span data-ttu-id="13229-119">**1 – CreateCard.aspx auflisten**</span><span class="sxs-lookup"><span data-stu-id="13229-119">**Listing 1 - CreateCard.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample1.aspx)]

<span data-ttu-id="13229-120">Das Formular in Codebeispiel 1 funktioniert, aber es bietet eine hervorragende benutzerumgebung keine.</span><span class="sxs-lookup"><span data-stu-id="13229-120">The form in Listing 1 works, but it does not provide a great user experience.</span></span> <span data-ttu-id="13229-121">Der Benutzer hat eine Farbe in das Textfeld eingeben.</span><span class="sxs-lookup"><span data-stu-id="13229-121">The user has to type a color into the textbox.</span></span> <span data-ttu-id="13229-122">Wenn der Benutzer eine spezielle Farbe - will muss z. B. nur die richtige Schattierung des hauptzeit-Grün- und der Benutzer den HTML-Farbcode ohne Hilfe ermitteln.</span><span class="sxs-lookup"><span data-stu-id="13229-122">If the user wants a specialized color - for example, just the right shade of pea green - then the user must figure out the HTML color code without any help.</span></span>

<span data-ttu-id="13229-123">Sie können die ColorPicker-Steuerelement-Extender verwenden, um eine bessere benutzererfahrung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="13229-123">You can use the ColorPicker control extender to create a better user experience.</span></span> <span data-ttu-id="13229-124">Farbwähler zeigt ein Dialogfeld Farbe aus, wenn auf ein Textfeldsteuerelement den Fokus zu verschieben (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="13229-124">The ColorPicker displays a color dialog when you move focus to a TextBox control (see Figure 2).</span></span>


<span data-ttu-id="13229-125">[![Der ColorPicker-Steuerelement-Extender](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="13229-125">[![The ColorPicker Control Extender](using-the-colorpicker-control-extender-vb/_static/image2.jpg)](using-the-colorpicker-control-extender-vb/_static/image3.png)</span></span>

<span data-ttu-id="13229-126">**Abbildung 02**: die ColorPicker-Steuerelement-Extender ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-colorpicker-control-extender-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="13229-126">**Figure 02**: The ColorPicker Control Extender ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image4.png))</span></span>


<span data-ttu-id="13229-127">Sie müssen zwei Schritte ausführen, um die ColorPicker-Steuerelement-Extender verwenden, mit dem Formular in Codebeispiel 1:</span><span class="sxs-lookup"><span data-stu-id="13229-127">You need to complete two steps to use the ColorPicker control extender with the form in Listing 1:</span></span>

1. <span data-ttu-id="13229-128">Ein ScriptManager-Steuerelement auf der Seite hinzufügen</span><span class="sxs-lookup"><span data-stu-id="13229-128">Add a ScriptManager control to the page</span></span>
2. <span data-ttu-id="13229-129">ColorPicker-Steuerelement-Extender zur Seite hinzufügen</span><span class="sxs-lookup"><span data-stu-id="13229-129">Add the ColorPicker control extender to the page</span></span>

<span data-ttu-id="13229-130">Bevor Sie die ColorPicker-Komponente verwenden können, müssen Sie ein ScriptManager-Steuerelement auf Ihrer Seite hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="13229-130">Before you can use the ColorPicker, you must add a ScriptManager to your page.</span></span> <span data-ttu-id="13229-131">Eine gute Möglichkeit zum Hinzufügen von ScriptManager wird rechts neben das öffnendes serverseitige &lt;Formular&gt; Tag.</span><span class="sxs-lookup"><span data-stu-id="13229-131">A good place to add the ScriptManager is right below the opening server-side &lt;form&gt; tag.</span></span> <span data-ttu-id="13229-132">Sie können den ScriptManager auf der Seite aus der Toolbox ziehen (ein ScriptManager befindet sich unter der Registerkarte "AJAX-Erweiterungen").</span><span class="sxs-lookup"><span data-stu-id="13229-132">You can drag the ScriptManager onto the page from the toolbox (the ScriptManager is located under the AJAX Extensions tab).</span></span> <span data-ttu-id="13229-133">Alternativ können Sie das folgende Tag in die Datenquellensicht unter dem Starttag des serverseitigen Format eingeben:</span><span class="sxs-lookup"><span data-stu-id="13229-133">Alternatively, you can type the following tag into Source View beneath the opening server-side form tag:</span></span>

<span data-ttu-id="13229-134">&lt;ASP: ScriptManager-ID = "ScriptManager1" Runat = "Server" /&gt;</span><span class="sxs-lookup"><span data-stu-id="13229-134">&lt;asp:ScriptManager ID="ScriptManager1" runat="server" /&gt;</span></span>

<span data-ttu-id="13229-135">Die einfachste Möglichkeit zum Hinzufügen des ColorPicker-Extenders-Steuerelements auf der Seite ist in der Entwurfsansicht.</span><span class="sxs-lookup"><span data-stu-id="13229-135">The easiest way to add the ColorPicker control extender to the page is in Design View.</span></span> <span data-ttu-id="13229-136">Wenn Sie den Mauszeiger auf das Textfeld TxtCardColor zeigen, eine Smarttask Option wird angezeigt, der ermöglicht es Ihnen, einen Extender hinzuzufügen (siehe Abbildung 3).</span><span class="sxs-lookup"><span data-stu-id="13229-136">If you hover your mouse over the txtCardColor TextBox, a smart task option appears the enables you to add an extender (see Figure 3).</span></span> <span data-ttu-id="13229-137">Wenn Sie diese Option auswählen, die Extender-Assistent (siehe Abbildung 4) wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="13229-137">If you pick this option, the Extender Wizard appears (see Figure 4).</span></span>


<span data-ttu-id="13229-138">[![Hinzufügen eines Extenders](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="13229-138">[![Adding an extender](using-the-colorpicker-control-extender-vb/_static/image3.jpg)](using-the-colorpicker-control-extender-vb/_static/image5.png)</span></span>

<span data-ttu-id="13229-139">**Abbildung 03**: Hinzufügen eines Extenders ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-colorpicker-control-extender-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="13229-139">**Figure 03**: Adding an extender ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image6.png))</span></span>


<span data-ttu-id="13229-140">[![Einen Extender-Steuerelements mit dem Assistenten für Extender auswählen](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="13229-140">[![Selecting a control extender with the Extender Wizard](using-the-colorpicker-control-extender-vb/_static/image4.jpg)](using-the-colorpicker-control-extender-vb/_static/image7.png)</span></span>

<span data-ttu-id="13229-141">**Abbildung 04**: Auswählen eines mit dem Assistenten für Extender-Steuerelement-Extenders ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-colorpicker-control-extender-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="13229-141">**Figure 04**: Selecting a control extender with the Extender Wizard ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image8.png))</span></span>


<span data-ttu-id="13229-142">Sie können den ColorPicker-Extender erweitern die TxtCardColor Textfeld mit dem ColorPicker-Extender auswählen.</span><span class="sxs-lookup"><span data-stu-id="13229-142">You can pick the ColorPicker extender to extend the txtCardColor TextBox with the ColorPicker extender.</span></span> <span data-ttu-id="13229-143">Klicken Sie auf OK, um das Dialogfeld zu schließen.</span><span class="sxs-lookup"><span data-stu-id="13229-143">Click OK to close the dialog.</span></span>

<span data-ttu-id="13229-144">Nachdem Sie diese Änderungen vorgenommen haben, sieht die Quelle für die Seite Codebeispiel 2.</span><span class="sxs-lookup"><span data-stu-id="13229-144">After you make these changes, the source for the page looks like Listing 2.</span></span>

<span data-ttu-id="13229-145">**Codebeispiel 2 - CreateCard.aspx (mit ColorPicker)**</span><span class="sxs-lookup"><span data-stu-id="13229-145">**Listing 2 - CreateCard.aspx (with ColorPicker)**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample2.aspx)]

<span data-ttu-id="13229-146">Beachten Sie, dass die Seite jetzt ein ColorPickerExtender-Steuerelement enthält, die direkt unterhalb der TxtCardColor TextBox-Steuerelement angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="13229-146">Notice that the page now contains a ColorPickerExtender control that appears directly below the txtCardColor TextBox control.</span></span> <span data-ttu-id="13229-147">Das Steuerelement ColorPickerExtender erweitert das Steuerelement TxtCardColor, damit sie ein Dialogfeld für die Farbauswahl angezeigt.</span><span class="sxs-lookup"><span data-stu-id="13229-147">The ColorPickerExtender control extends the txtCardColor control so that it displays a color picker dialog.</span></span>

## <a name="using-a-button-to-launch-the-color-picker-dialog"></a><span data-ttu-id="13229-148">Mithilfe einer Schaltfläche, um das Dialogfeld für die Farbauswahl zu starten.</span><span class="sxs-lookup"><span data-stu-id="13229-148">Using a Button to Launch the Color Picker Dialog</span></span>

<span data-ttu-id="13229-149">Der ColorPicker-Extender unterstützt die folgenden Eigenschaften:</span><span class="sxs-lookup"><span data-stu-id="13229-149">The ColorPicker extender supports the following properties:</span></span>

- <span data-ttu-id="13229-150">PopupButtonId - die ID einer Schaltfläche auf der Seite, die bewirkt, dass das Dialogfeld für die Farbauswahl angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="13229-150">PopupButtonId - The ID of a button on the page that causes the color picker dialog to appear.</span></span>
- <span data-ttu-id="13229-151">PopupPosition - die Position relativ zum Zielsteuerelement, der das Dialogfeld für die Farbauswahl.</span><span class="sxs-lookup"><span data-stu-id="13229-151">PopupPosition - The position, relative to the target control, of the color picker dialog.</span></span> <span data-ttu-id="13229-152">Mögliche Werte sind absolut, Center, BottomLeft, BottomRight, TopLeft, oberen, rechten, und Links (der Standardwert ist BottomLeft).</span><span class="sxs-lookup"><span data-stu-id="13229-152">Possible values are Absolute, Center, BottomLeft, BottomRight, TopLeft, TopRight, Right, and Left (the default is BottomLeft).</span></span>
- <span data-ttu-id="13229-153">SampleControlId: die ID eines Steuerelements, in der ausgewählte Farbe angezeigt.</span><span class="sxs-lookup"><span data-stu-id="13229-153">SampleControlId - The ID of a control that displays the selected color.</span></span>
- <span data-ttu-id="13229-154">SelectedColor - die Ausgangsfarbe, die durch die ColorPicker ausgewählt.</span><span class="sxs-lookup"><span data-stu-id="13229-154">SelectedColor - The initial color selected by the ColorPicker.</span></span>

<span data-ttu-id="13229-155">Sie können diese Eigenschaften verwenden, um festzulegen, wie das Dialogfeld für die Farbauswahl angezeigt wird und wie die ausgewählte Farbe angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="13229-155">You can use these properties to customize how the color picker dialog is displayed and how the selected color is displayed.</span></span> <span data-ttu-id="13229-156">Die Seite in Codebeispiel 3 wird veranschaulicht, wie Sie einige dieser Eigenschaften verwenden können.</span><span class="sxs-lookup"><span data-stu-id="13229-156">The page in Listing 3 illustrates how you can use several of these properties.</span></span>

<span data-ttu-id="13229-157">**Codebeispiel 3 - CreateCardButton.aspx**</span><span class="sxs-lookup"><span data-stu-id="13229-157">**Listing 3 - CreateCardButton.aspx**</span></span>

[!code-aspx[Main](using-the-colorpicker-control-extender-vb/samples/sample3.aspx)]

<span data-ttu-id="13229-158">Die Seite in Programmausdruck 3 enthält eine Farbe auswählen Schaltfläche (siehe Abbildung 5).</span><span class="sxs-lookup"><span data-stu-id="13229-158">The page in Listing 3 includes a Pick Color button (see Figure 5).</span></span> <span data-ttu-id="13229-159">Wenn Sie diese Schaltfläche klicken, wird das Dialogfeld für die Farbauswahl angezeigt, über das Textfeld ein.</span><span class="sxs-lookup"><span data-stu-id="13229-159">When you click this button, the color picker dialog appears above the TextBox.</span></span> <span data-ttu-id="13229-160">Bei Auswahl eine Farbe aus dem Dialogfeld wird die ausgewählte Farbe als Farbe des Hintergrunds der LblSample Label-Steuerelement angezeigt.</span><span class="sxs-lookup"><span data-stu-id="13229-160">If you select a color from the dialog then the selected color appears as the background color of the lblSample Label control.</span></span>

<span data-ttu-id="13229-161">Die ColorPicker PopupButtonID-Eigenschaft wird verwendet, der ColorPicker-Extender die Schaltfläche "Farbe auswählen" zugeordnet werden soll.</span><span class="sxs-lookup"><span data-stu-id="13229-161">The ColorPicker PopupButtonID property is used to associate the Pick Color button with the ColorPicker extender.</span></span> <span data-ttu-id="13229-162">Wenn Sie einen Wert für die PopupButtonID-Eigenschaft angeben, wird das Dialogfeld für die Farbauswahl nicht mehr angezeigt, wenn das Zielsteuerelement den Fokus besitzt.</span><span class="sxs-lookup"><span data-stu-id="13229-162">When you supply a value for the PopupButtonID property, the color picker dialog no longer appears when the target control has focus.</span></span> <span data-ttu-id="13229-163">Sie müssen die Schaltfläche, um die Anzeige des Dialogfelds klicken.</span><span class="sxs-lookup"><span data-stu-id="13229-163">You must click the button to display the dialog.</span></span>

<span data-ttu-id="13229-164">Die SampleControlID-Eigenschaft wird verwendet, um ein Steuerelement zuzuordnen, die die ausgewählte Farbe mit der ColorPicker anzeigt.</span><span class="sxs-lookup"><span data-stu-id="13229-164">The SampleControlID property is used to associate a control that displays the selected color with the ColorPicker.</span></span> <span data-ttu-id="13229-165">Die ColorPicker ändert die Hintergrundfarbe des Steuerelements, die zurzeit ausgewählte Farbe an.</span><span class="sxs-lookup"><span data-stu-id="13229-165">The ColorPicker changes the background color of this control to the currently selected color.</span></span>


<span data-ttu-id="13229-166">[![Das Dialogfeld für die Farbauswahl mit einer Schaltfläche anzeigen](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="13229-166">[![Displaying the color picker dialog with a button](using-the-colorpicker-control-extender-vb/_static/image5.jpg)](using-the-colorpicker-control-extender-vb/_static/image9.png)</span></span>

<span data-ttu-id="13229-167">**Abbildung 05**: das Dialogfeld mit einer Schaltfläche für die Farbauswahl angezeigt ([klicken Sie, um das Bild in voller Größe anzeigen](using-the-colorpicker-control-extender-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="13229-167">**Figure 05**: Displaying the color picker dialog with a button ([Click to view full-size image](using-the-colorpicker-control-extender-vb/_static/image10.png))</span></span>


## <a name="summary"></a><span data-ttu-id="13229-168">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="13229-168">Summary</span></span>

<span data-ttu-id="13229-169">In diesem Tutorial haben Sie gelernt, wie die ColorPicker-Steuerelement-Extender zu verwenden, um ein Dialogfeld für die Farbauswahl Popup anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="13229-169">In this tutorial, you learned how to use the ColorPicker control extender to display a popup color picker dialog.</span></span> <span data-ttu-id="13229-170">Zunächst können wir untersucht, wie Sie das Dialogfeld anzeigen können, wenn der Fokus in einem TextBox-Steuerelement verschoben wird.</span><span class="sxs-lookup"><span data-stu-id="13229-170">First, we examined how you can display the dialog when focus is moved to a TextBox control.</span></span> <span data-ttu-id="13229-171">Als Nächstes, wie Sie eine Schaltfläche zu erstellen, in dem das Dialogfeld für die Farbauswahl angezeigt, wenn die Schaltfläche geklickt wird.</span><span class="sxs-lookup"><span data-stu-id="13229-171">Next, you learned how to create a button that displays the color picker dialog when the button is clicked.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="13229-172">Vorherige</span><span class="sxs-lookup"><span data-stu-id="13229-172">Previous</span></span>](using-the-colorpicker-control-extender-cs.md)
