---
title: 'ASP.NET Core MVC mit Entity Framework Core: Datenmodell (5 von 10)'
author: rick-anderson
description: In diesem Tutorial fügen Sie weitere Entitäten und Beziehungen hinzu und passen das Datenmodell an, indem Sie Regeln zur Formatierung, Validierung und Zuordnung angeben.
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/complex-data-model
ms.openlocfilehash: 1d3c69c8c658b5ca2f0253b790b0dc75d44d3064
ms.sourcegitcommit: c6ed2f00c7a08223d79090396b85793718b0dd69
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/29/2018
ms.locfileid: "37093113"
---
# <a name="aspnet-core-mvc-with-ef-core---data-model---5-of-10"></a><span data-ttu-id="ede59-103">ASP.NET Core MVC mit Entity Framework Core: Datenmodell (5 von 10)</span><span class="sxs-lookup"><span data-stu-id="ede59-103">ASP.NET Core MVC with EF Core - Data Model - 5 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ede59-104">Von [Tom Dykstra](https://github.com/tdykstra) und [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ede59-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ede59-105">Die Contoso University-Beispielwebanwendung veranschaulicht, wie ASP.NET Core MVC-Webanwendungen mit Entity Framework Core und Visual Studio erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="ede59-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="ede59-106">Informationen zu dieser Tutorialreihe finden Sie im [ersten Tutorial der Reihe](intro.md).</span><span class="sxs-lookup"><span data-stu-id="ede59-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="ede59-107">In den vorherigen Tutorials haben Sie mit einem einfachen Datenmodell gearbeitet, das aus drei Entitäten bestand.</span><span class="sxs-lookup"><span data-stu-id="ede59-107">In the previous tutorials, you worked with a simple data model that was composed of three entities.</span></span> <span data-ttu-id="ede59-108">In diesem Tutorial fügen Sie weitere Entitäten und Beziehungen hinzu und passen das Datenmodell an, indem Sie die Regeln zur Formatierung, Validierung und Datenbankzuordnung angeben.</span><span class="sxs-lookup"><span data-stu-id="ede59-108">In this tutorial, you'll add more entities and relationships and you'll customize the data model by specifying formatting, validation, and database mapping rules.</span></span>

<span data-ttu-id="ede59-109">Wenn Sie dies erledigt haben, bilden die Entitätsklassen ein vollständiges Datenmodell, das in der folgenden Abbildung dargestellt wird:</span><span class="sxs-lookup"><span data-stu-id="ede59-109">When you're finished, the entity classes will make up the completed data model that's shown in the following illustration:</span></span>

![Entitätsdiagramm](complex-data-model/_static/diagram.png)

## <a name="customize-the-data-model-by-using-attributes"></a><span data-ttu-id="ede59-111">Anpassen des Datenmodells mithilfe von Attributen</span><span class="sxs-lookup"><span data-stu-id="ede59-111">Customize the Data Model by Using Attributes</span></span>

<span data-ttu-id="ede59-112">In diesem Abschnitt wird das Anpassen des Datenmodells mithilfe von Attributen erläutert, die Regeln zur Formatierung, Validierung und Datenbankzuordnung angeben.</span><span class="sxs-lookup"><span data-stu-id="ede59-112">In this section you'll see how to customize the data model by using attributes that specify formatting, validation, and database mapping rules.</span></span> <span data-ttu-id="ede59-113">In den folgenden Abschnitten erstellen Sie dann das vollständige Datenmodell „School“, indem Sie Attribute zu den bereits erstellten Klassen hinzufügen und neue Klassen für die verbleibenden Entitätstypen im Modell erstellen.</span><span class="sxs-lookup"><span data-stu-id="ede59-113">Then in several of the following sections you'll create the complete School data model by adding attributes to the classes you already created and creating new classes for the remaining entity types in the model.</span></span>

### <a name="the-datatype-attribute"></a><span data-ttu-id="ede59-114">Das DataType-Attribut</span><span class="sxs-lookup"><span data-stu-id="ede59-114">The DataType attribute</span></span>

<span data-ttu-id="ede59-115">Für die Anmeldedaten von Studenten zeigen alle Webseiten derzeit die Zeit und das Datum an, obwohl für dieses Feld nur das Datum relevant ist.</span><span class="sxs-lookup"><span data-stu-id="ede59-115">For student enrollment dates, all of the web pages currently display the time along with the date, although all you care about for this field is the date.</span></span> <span data-ttu-id="ede59-116">Indem Sie Attribute für die Datenanmerkung verwenden, können Sie eine Codeänderungen vornehmen, durch die das Anzeigeformat in jeder Ansicht korrigiert wird, in der die Daten angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="ede59-116">By using data annotation attributes, you can make one code change that will fix the display format in every view that shows the data.</span></span> <span data-ttu-id="ede59-117">Sie können dies beispielhaft testen, indem Sie ein Attribut zur `EnrollmentDate`-Eigenschaft der `Student`-Klasse hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="ede59-117">To see an example of how to do that, you'll add an attribute to the `EnrollmentDate` property in the `Student` class.</span></span>

<span data-ttu-id="ede59-118">Fügen Sie in *Models/Student.cs* eine `using`-Anweisung für den `System.ComponentModel.DataAnnotations`-Namespace hinzu, und fügen Sie wie im folgenden Beispiel dargestellt die Attribute `DataType` und `DisplayFormat` zur `EnrollmentDate`-Eigenschaft hinzu:</span><span class="sxs-lookup"><span data-stu-id="ede59-118">In *Models/Student.cs*, add a `using` statement for the `System.ComponentModel.DataAnnotations` namespace and add `DataType` and `DisplayFormat` attributes to the `EnrollmentDate` property, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_DataType&highlight=3,12-13)]

<span data-ttu-id="ede59-119">Das `DataType`-Attribut wird verwendet, um einen Datentyp anzugeben, der spezifischer als der datenbankinterne Typ ist.</span><span class="sxs-lookup"><span data-stu-id="ede59-119">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="ede59-120">In diesem Fall soll nur das Datum verfolgt werden, nicht das Datum und die Zeit.</span><span class="sxs-lookup"><span data-stu-id="ede59-120">In this case we only want to keep track of the date, not the date and time.</span></span> <span data-ttu-id="ede59-121">Die `DataType`-Enumeration stellt viele Datentypen bereit, z.B. „Date“, „Time“, „PhoneNumber“, „Currency“, „EmailAddress“ usw.</span><span class="sxs-lookup"><span data-stu-id="ede59-121">The  `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="ede59-122">Das `DataType`-Attribut kann der Anwendung auch ermöglichen, typspezifische Features bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="ede59-122">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="ede59-123">Beispielsweise kann ein `mailto:`-Link für `DataType.EmailAddress` erstellt werden, und eine Datumsauswahl kann in Browsern mit Unterstützung für HTML5 für `DataType.Date` bereitgestellt werden.</span><span class="sxs-lookup"><span data-stu-id="ede59-123">For example, a `mailto:` link can be created for `DataType.EmailAddress`, and a date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="ede59-124">Das `DataType`-Attribut gibt `data-`-Attribute (ausgesprochen „Datadash“) von HTML5 aus, die von HTML5-Browsern genutzt werden können.</span><span class="sxs-lookup"><span data-stu-id="ede59-124">The `DataType` attribute emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers can understand.</span></span> <span data-ttu-id="ede59-125">Die `DataType`-Attribute bieten keine Validierung.</span><span class="sxs-lookup"><span data-stu-id="ede59-125">The `DataType` attributes don't provide any validation.</span></span>

<span data-ttu-id="ede59-126">`DataType.Date` gibt nicht das Format des Datums an, das angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="ede59-126">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="ede59-127">Standardmäßig wird das Datenfeld gemäß den Standardformaten basierend auf der CultureInfo-Klasse des Servers angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ede59-127">By default, the data field is displayed according to the default formats based on the server's CultureInfo.</span></span>

<span data-ttu-id="ede59-128">Das `DisplayFormat`-Attribut dient zum expliziten Angeben des Datumsformats:</span><span class="sxs-lookup"><span data-stu-id="ede59-128">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

<span data-ttu-id="ede59-129">Die Einstellung `ApplyFormatInEditMode` gibt an, dass die Formatierung auch angewendet werden soll, wenn der Wert in einem Textfeld zur Bearbeitung angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="ede59-129">The `ApplyFormatInEditMode` setting specifies that the formatting should also be applied when the value is displayed in a text box for editing.</span></span> <span data-ttu-id="ede59-130">(Dies könnte für einige Felder nützlich sein, z.B. soll für Währungsangaben das Währungssymbol nicht im Textfeld für die Bearbeitung enthalten sein.)</span><span class="sxs-lookup"><span data-stu-id="ede59-130">(You might not want that for some fields -- for example, for currency values, you might not want the currency symbol in the text box for editing.)</span></span>

<span data-ttu-id="ede59-131">Sie können das `DisplayFormat`-Attribut eigenständig verwenden, meistens empfiehlt es sich jedoch, ebenfalls das `DataType`-Attribut zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ede59-131">You can use the `DisplayFormat` attribute by itself, but it's generally a good idea to use the `DataType` attribute also.</span></span> <span data-ttu-id="ede59-132">Das `DataType`-Attribut übermittelt die Semantik der Daten im Gegensatz zu deren Rendern auf einem Bildschirm. Es bietet folgende Vorteile, die `DisplayFormat` nicht bietet:</span><span class="sxs-lookup"><span data-stu-id="ede59-132">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with `DisplayFormat`:</span></span>

* <span data-ttu-id="ede59-133">Der Browser kann HTML5-Features aktivieren (z.B. zum Anzeigen eines Kalendersteuerelements, des dem Gebietsschema entsprechenden Währungssymbols, von E-Mail-Links, einigen clientseitigen Eingabevalidierungen usw.).</span><span class="sxs-lookup"><span data-stu-id="ede59-133">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, some client-side input validation, etc.).</span></span>

* <span data-ttu-id="ede59-134">Standardmäßig rendert der Browser Daten mit dem ordnungsgemäßen auf Ihrem Gebietsschema basierenden Format.</span><span class="sxs-lookup"><span data-stu-id="ede59-134">By default, the browser will render data using the correct format based on your locale.</span></span>

<span data-ttu-id="ede59-135">Weitere Informationen finden Sie in der [Dokumentation zum \<input>-Taghilfsprogramm](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span><span class="sxs-lookup"><span data-stu-id="ede59-135">For more information, see the [\<input> tag helper documentation](../../mvc/views/working-with-forms.md#the-input-tag-helper).</span></span>

<span data-ttu-id="ede59-136">Führen Sie die App aus, und wechseln Sie zur Indexseite „Studenten“, um festzustellen, dass beim Registrierungsdatum nicht mehr die Uhrzeit angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="ede59-136">Run the app, go to the Students Index page and notice that times are no longer displayed for the enrollment dates.</span></span> <span data-ttu-id="ede59-137">Dies gilt für jede Ansicht, die das Modell „Student“ verwendet.</span><span class="sxs-lookup"><span data-stu-id="ede59-137">The same will be true for any view that uses the Student model.</span></span>

![Indexseite „Studenten“, die Datumsangaben ohne Uhrzeit anzeigt](complex-data-model/_static/dates-no-times.png)

### <a name="the-stringlength-attribute"></a><span data-ttu-id="ede59-139">Das StringLength-Attribut</span><span class="sxs-lookup"><span data-stu-id="ede59-139">The StringLength attribute</span></span>

<span data-ttu-id="ede59-140">Sie können ebenfalls Regeln für die Datenvalidierung und Meldungen für Validierungsfehler mithilfe von Attributen angeben.</span><span class="sxs-lookup"><span data-stu-id="ede59-140">You can also specify data validation rules and validation error messages using attributes.</span></span> <span data-ttu-id="ede59-141">Das `StringLength`-Attribut legt die maximale Länge in der Datenbank fest und stellt die clientseitige und serverseitige Validierung für ASP.NET MVC bereit.</span><span class="sxs-lookup"><span data-stu-id="ede59-141">The `StringLength` attribute sets the maximum length  in the database and provides client side and server side validation for ASP.NET MVC.</span></span> <span data-ttu-id="ede59-142">Sie können ebenfalls die mindestens erforderliche Zeichenfolgenlänge in diesem Attribut angeben, dieser Wert hat jedoch keine Auswirkung auf das Datenbankschema.</span><span class="sxs-lookup"><span data-stu-id="ede59-142">You can also specify the minimum string length in this attribute, but the minimum value has no impact on the database schema.</span></span>

<span data-ttu-id="ede59-143">Angenommen, Sie möchten sicherstellen, dass Benutzer nicht mehr als 50 Zeichen für einen Namen eingeben.</span><span class="sxs-lookup"><span data-stu-id="ede59-143">Suppose you want to ensure that users don't enter more than 50 characters for a name.</span></span> <span data-ttu-id="ede59-144">Fügen Sie zum Hinzufügen dieser Einschränkung wie im folgenden Beispiel dargestellt `StringLength`-Attribute zu den `LastName`- und `FirstMidName`-Eigenschaften hinzu:</span><span class="sxs-lookup"><span data-stu-id="ede59-144">To add this limitation, add `StringLength` attributes to the `LastName` and `FirstMidName` properties, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_StringLength&highlight=10,12)]

<span data-ttu-id="ede59-145">Das `StringLength`-Attribut verhindert nicht, dass ein Benutzer einen Leerraum als Namen eingibt.</span><span class="sxs-lookup"><span data-stu-id="ede59-145">The `StringLength` attribute won't prevent a user from entering white space for a name.</span></span> <span data-ttu-id="ede59-146">Sie können das `RegularExpression`-Attribut verwenden, um Beschränkungen auf die Eingabe anzuwenden.</span><span class="sxs-lookup"><span data-stu-id="ede59-146">You can use the `RegularExpression` attribute to apply restrictions to the input.</span></span> <span data-ttu-id="ede59-147">Folgender Code erfordert beispielsweise, dass das erste Zeichen ein Großbuchstabe sein muss und die restlichen Zeichen alphabetisch sein müssen:</span><span class="sxs-lookup"><span data-stu-id="ede59-147">For example, the following code requires the first character to be upper case and the remaining characters to be alphabetical:</span></span>

```csharp
[RegularExpression(@"^[A-Z]+[a-zA-Z""'\s-]*$")]
```

<span data-ttu-id="ede59-148">Das `MaxLength`-Attribut stellt ähnliche Funktionalitäten wie das `StringLength`-Attribut bereit, ermöglicht jedoch keine clientseitige Validierung.</span><span class="sxs-lookup"><span data-stu-id="ede59-148">The `MaxLength` attribute provides functionality similar to the `StringLength` attribute but doesn't provide client side validation.</span></span>

<span data-ttu-id="ede59-149">Das Datenbankmodell hat sich nun auf eine Weise geändert, die eine Änderung des Datenbankschemas erfordert.</span><span class="sxs-lookup"><span data-stu-id="ede59-149">The database model has now changed in a way that requires a change in the database schema.</span></span> <span data-ttu-id="ede59-150">Verwenden Sie Migrationen, um das Schema ohne Verlust der Daten zu aktualisieren, die Sie mithilfe der Benutzeroberfläche der Anwendung zur Datenbank hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="ede59-150">You'll use migrations to update the schema without losing any data that you may have added to the database by using the application UI.</span></span>

<span data-ttu-id="ede59-151">Speichern Sie Ihre Änderungen, und erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="ede59-151">Save your changes and build the project.</span></span> <span data-ttu-id="ede59-152">Öffnen Sie anschließend das Befehlsfenster im Projektordner, und geben Sie folgenden Befehl ein:</span><span class="sxs-lookup"><span data-stu-id="ede59-152">Then open the command window in the project folder and enter the following commands:</span></span>

```console
dotnet ef migrations add MaxLengthOnNames
```

```console
dotnet ef database update
```

<span data-ttu-id="ede59-153">Der `migrations add`-Befehl gibt eine Warnung aus, dass es zu Datenverlust kommen kann, da die maximale Länge durch die Änderung um zwei Spalten verkürzt wurde.</span><span class="sxs-lookup"><span data-stu-id="ede59-153">The `migrations add` command warns that data loss may occur, because the change makes the maximum length shorter for two columns.</span></span>  <span data-ttu-id="ede59-154">Durch Migrationen wird eine Datei namens *\<Zeitstempel>_MaxLengthOnNames.cs* erstellt.</span><span class="sxs-lookup"><span data-stu-id="ede59-154">Migrations creates a file named *\<timeStamp>_MaxLengthOnNames.cs*.</span></span> <span data-ttu-id="ede59-155">Diese Datei enthält Code in der `Up`-Methode, durch den die Datenbank dem aktuellen Datenmodell entsprechend aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="ede59-155">This file contains code in the `Up` method that will update the database to match the current data model.</span></span> <span data-ttu-id="ede59-156">Der Code wurde durch den `database update`-Befehl ausgeführt.</span><span class="sxs-lookup"><span data-stu-id="ede59-156">The `database update` command ran that code.</span></span>

<span data-ttu-id="ede59-157">Der Zeitstempel, der dem Namen der Migrationsdatei vorangestellt ist, wird von Entity Framework verwendet, um die Migrationen zu sortieren.</span><span class="sxs-lookup"><span data-stu-id="ede59-157">The timestamp prefixed to the migrations file name is used by Entity Framework to order the migrations.</span></span> <span data-ttu-id="ede59-158">Sie können mehrere Migrationen erstellen, bevor Sie den Befehl „update-database“ ausführen. Anschließend werden alle Migrationen in der Reihenfolge angewendet, in der Sie erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="ede59-158">You can create multiple migrations before running the update-database command, and then all of the migrations are applied in the order in which they were created.</span></span>

<span data-ttu-id="ede59-159">Führen Sie die App aus, klicken Sie auf die Registerkarte **Studenten** und dann auf **Neu erstellen**, und geben Sie einen beliebigen Namen ein, der länger als 50 Zeichen ist.</span><span class="sxs-lookup"><span data-stu-id="ede59-159">Run the app, select the **Students** tab, click **Create New**, and enter either name longer than 50 characters.</span></span> <span data-ttu-id="ede59-160">Wenn Sie auf **Erstellen** klicken, zeigt die clientseitige Validierung eine Fehlermeldung an.</span><span class="sxs-lookup"><span data-stu-id="ede59-160">When you click **Create**, client side validation shows an error message.</span></span>

![Indexseite „Studenten“, die Fehler für die Zeichenfolgenlänge anzeigt](complex-data-model/_static/string-length-errors.png)

### <a name="the-column-attribute"></a><span data-ttu-id="ede59-162">Das Column-Attribut</span><span class="sxs-lookup"><span data-stu-id="ede59-162">The Column attribute</span></span>

<span data-ttu-id="ede59-163">Sie können ebenfalls Attribute verwenden, um zu steuern, wie Ihre Klassen und Eigenschaften der Datenbank zugeordnet werden.</span><span class="sxs-lookup"><span data-stu-id="ede59-163">You can also use attributes to control how your classes and properties are mapped to the database.</span></span> <span data-ttu-id="ede59-164">Angenommen, Sie haben den Namen `FirstMidName` für das Feld „first-name“ verwendet, da das Feld ebenfalls einen Zweitnamen enthalten kann.</span><span class="sxs-lookup"><span data-stu-id="ede59-164">Suppose you had used the name `FirstMidName` for the first-name field because the field might also contain a middle name.</span></span> <span data-ttu-id="ede59-165">Sie möchten jedoch, dass die Datenbankspalte in `FirstName` umbenannt wird, damit Benutzer, die Ad-hob-Abfragen für die Datenbank schreiben, an diesen Namen gewöhnt sind.</span><span class="sxs-lookup"><span data-stu-id="ede59-165">But you want the database column to be named `FirstName`, because users who will be writing ad-hoc queries against the database are accustomed to that name.</span></span> <span data-ttu-id="ede59-166">Sie können das `Column`-Attribut verwenden, um diese Zuordnung durchzuführen.</span><span class="sxs-lookup"><span data-stu-id="ede59-166">To make this mapping, you can use the `Column` attribute.</span></span>

<span data-ttu-id="ede59-167">Das `Column`-Attribut gibt an, dass bei der Erstellung der Datenbank die Spalte des `Student`-Elements, die der `FirstMidName`-Eigenschaft zugeordnet ist, den Namen `FirstName` erhält.</span><span class="sxs-lookup"><span data-stu-id="ede59-167">The `Column` attribute specifies that when the database is created, the column of the `Student` table that maps to the `FirstMidName` property will be named `FirstName`.</span></span> <span data-ttu-id="ede59-168">Wenn Ihr Code also auf `Student.FirstMidName` verweist, stammen die Daten aus der `FirstName`-Spalte der `Student`-Tabelle oder werden in dieser aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="ede59-168">In other words, when your code refers to `Student.FirstMidName`, the data will come from or be updated in the `FirstName` column of the `Student` table.</span></span> <span data-ttu-id="ede59-169">Wenn Sie keine Spaltennamen angeben, erhalten diese den gleichen Namen wie die Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="ede59-169">If you don't specify column names, they're given the same name as the property name.</span></span>

<span data-ttu-id="ede59-170">Fügen Sie in der Datei *Student.cs* eine `using`-Anweisung für `System.ComponentModel.DataAnnotations.Schema` hinzu, und fügen Sie wie im folgenden hervorgehobenen Code dargestellt das Attribut für den Spaltennamen zur `FirstMidName`-Eigenschaft hinzu:</span><span class="sxs-lookup"><span data-stu-id="ede59-170">In the *Student.cs* file, add a `using` statement for `System.ComponentModel.DataAnnotations.Schema` and add the column name attribute to the `FirstMidName` property, as shown in the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Column&highlight=4,14)]

<span data-ttu-id="ede59-171">Durch das Hinzufügen des `Column`-Attributs wird das Modell geändert, das `SchoolContext` unterstützt, sodass keine Übereinstimmung mit der Datenbank vorliegt.</span><span class="sxs-lookup"><span data-stu-id="ede59-171">The addition of the `Column` attribute changes the model backing the `SchoolContext`, so it won't match the database.</span></span>

<span data-ttu-id="ede59-172">Speichern Sie Ihre Änderungen, und erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="ede59-172">Save your changes and build the project.</span></span> <span data-ttu-id="ede59-173">Öffnen Sie anschließend das Befehlsfenster im Projektordner, und geben Sie folgenden Befehl ein, um eine weitere Migration zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="ede59-173">Then open the command window in the project folder and enter the following commands to create another migration:</span></span>

```console
dotnet ef migrations add ColumnFirstName
```

```console
dotnet ef database update
```

<span data-ttu-id="ede59-174">Öffnen Sie im **SQL Server-Objekt-Explorer** den Tabellen-Designer „Student“, indem Sie auf die Tabelle **Student** doppelklicken.</span><span class="sxs-lookup"><span data-stu-id="ede59-174">In **SQL Server Object Explorer**, open the Student table designer by double-clicking the **Student** table.</span></span>

![Tabelle „Students“ im SSOX nach der Migration](complex-data-model/_static/ssox-after-migration.png)

<span data-ttu-id="ede59-176">Bevor Sie die ersten beiden Migrationen angewendet haben, wiesen die Namensspalten den Typ „nvarchar(MAX)“ auf.</span><span class="sxs-lookup"><span data-stu-id="ede59-176">Before you applied the first two migrations, the name columns were of type nvarchar(MAX).</span></span> <span data-ttu-id="ede59-177">Diese besitzen nun den Typ „nvarchar(50)“, und der Spaltenname hat sich von „FirstMidName“ in „FirstName“ geändert.</span><span class="sxs-lookup"><span data-stu-id="ede59-177">They're now nvarchar(50) and the column name has changed from FirstMidName to FirstName.</span></span>

> [!Note]
> <span data-ttu-id="ede59-178">Wenn Sie vor dem Erstellen aller Entitätsklassen in den folgenden Abschnitten versuchen, zu kompilieren, werden möglicherweise Compilerfehler angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ede59-178">If you try to compile before you finish creating all of the entity classes in the following sections, you might get compiler errors.</span></span>

## <a name="final-changes-to-the-student-entity"></a><span data-ttu-id="ede59-179">Letzte Änderungen an der Entität „Student“</span><span class="sxs-lookup"><span data-stu-id="ede59-179">Final changes to the Student entity</span></span>

![Entität „Student“](complex-data-model/_static/student-entity.png)

<span data-ttu-id="ede59-181">Ersetzen Sie in *Models/Student.cs* den Code, den Sie zuvor hinzugefügt haben, durch folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="ede59-181">In *Models/Student.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="ede59-182">Die Änderungen werden hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="ede59-182">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_BeforeInheritance&highlight=11,13,15,18,22,24-31)]

### <a name="the-required-attribute"></a><span data-ttu-id="ede59-183">Das Attribut „Required“</span><span class="sxs-lookup"><span data-stu-id="ede59-183">The Required attribute</span></span>

<span data-ttu-id="ede59-184">Durch das `Required`-Attribut werden die Name-Eigenschaften zu Pflichtfeldern.</span><span class="sxs-lookup"><span data-stu-id="ede59-184">The `Required` attribute makes the name properties required fields.</span></span> <span data-ttu-id="ede59-185">Das `Required`-Attribut ist für nicht auf NULL festlegbare Typen wie Werttypen (DateTime, int, double, float usw.) nicht erforderlich.</span><span class="sxs-lookup"><span data-stu-id="ede59-185">The `Required` attribute isn't needed for non-nullable types such as value types (DateTime, int, double, float, etc.).</span></span> <span data-ttu-id="ede59-186">Typen, die nicht auf NULL festgelegt werden können, werden automatisch als Pflichtfelder behandelt.</span><span class="sxs-lookup"><span data-stu-id="ede59-186">Types that can't be null are automatically treated as required fields.</span></span>

<span data-ttu-id="ede59-187">Sie können das `Required`-Attribut entfernen und durch einen Parameter mit der mindestens erforderlichen Länge für das `StringLength`-Attribut ersetzen:</span><span class="sxs-lookup"><span data-stu-id="ede59-187">You could remove the `Required` attribute and replace it with a minimum length parameter for the `StringLength` attribute:</span></span>

```csharp
[Display(Name = "Last Name")]
[StringLength(50, MinimumLength=1)]
public string LastName { get; set; }
```

### <a name="the-display-attribute"></a><span data-ttu-id="ede59-188">Das Display-Attribut</span><span class="sxs-lookup"><span data-stu-id="ede59-188">The Display attribute</span></span>

<span data-ttu-id="ede59-189">Das `Display`-Attribut gibt an, dass die Beschriftung der Textfelder in jeder Instanz „First Name“, „Last Name“, „Full Name“ und „Enrollment Date“ lauten soll, statt dem Eigenschaftsnamen zu entsprechen (bei dem die Wörter nicht durch Leerräume getrennt werden).</span><span class="sxs-lookup"><span data-stu-id="ede59-189">The `Display` attribute specifies that the caption for the text boxes should be "First Name", "Last Name", "Full Name", and "Enrollment Date" instead of the property name in each instance (which has no space dividing the words).</span></span>

### <a name="the-fullname-calculated-property"></a><span data-ttu-id="ede59-190">Die berechnete FullName-Eigenschaft</span><span class="sxs-lookup"><span data-stu-id="ede59-190">The FullName calculated property</span></span>

<span data-ttu-id="ede59-191">Bei `FullName` handelt es sich um eine berechnete Eigenschaft, die einen Wert zurückgibt, der durch das Verketten von zwei weiteren Eigenschaften erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="ede59-191">`FullName` is a calculated property that returns a value that's created by concatenating two other properties.</span></span> <span data-ttu-id="ede59-192">Daher verfügt diese nur über einen get-Accessor, und keine `FullName`-Spalte wird in der Datenbank generiert.</span><span class="sxs-lookup"><span data-stu-id="ede59-192">Therefore it has only a get accessor, and no `FullName` column will be generated in the database.</span></span>

## <a name="create-the-instructor-entity"></a><span data-ttu-id="ede59-193">Erstellen der Entität „Instructor“</span><span class="sxs-lookup"><span data-stu-id="ede59-193">Create the Instructor Entity</span></span>

![Entität „Instructor“](complex-data-model/_static/instructor-entity.png)

<span data-ttu-id="ede59-195">Erstellen Sie *Models/Instructor.cs*, indem Sie den Vorlagencode durch folgenden Code ersetzen:</span><span class="sxs-lookup"><span data-stu-id="ede59-195">Create *Models/Instructor.cs*, replacing the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_BeforeInheritance)]

<span data-ttu-id="ede59-196">Beachten Sie, dass einige Eigenschaften in den Entitäten „Student“ und „Instructor“ identisch sind.</span><span class="sxs-lookup"><span data-stu-id="ede59-196">Notice that several properties are the same in the Student and Instructor entities.</span></span> <span data-ttu-id="ede59-197">Im folgenden Tutorial [Implementing Inheritance (Implementierung von Vererbung)](inheritance.md) gestalten Sie diesen Code um, um die Redundanzen zu entfernen.</span><span class="sxs-lookup"><span data-stu-id="ede59-197">In the [Implementing Inheritance](inheritance.md) tutorial later in this series, you'll refactor this code to eliminate the redundancy.</span></span>

<span data-ttu-id="ede59-198">Sie können mehrere Attribute in einer Zeile einfügen, also können Sie das `HireDate`-Attribut folgendermaßen schreiben:</span><span class="sxs-lookup"><span data-stu-id="ede59-198">You can put multiple attributes on one line, so you could also write the `HireDate` attributes as follows:</span></span>

```csharp
[DataType(DataType.Date),Display(Name = "Hire Date"),DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
```

### <a name="the-courseassignments-and-officeassignment-navigation-properties"></a><span data-ttu-id="ede59-199">Die Navigationseigenschaften „CourseAssignments“ und „OfficeAssignment“</span><span class="sxs-lookup"><span data-stu-id="ede59-199">The CourseAssignments and OfficeAssignment navigation properties</span></span>

<span data-ttu-id="ede59-200">Bei den Eigenschaften `CourseAssignments` und `OfficeAssignment` handelt es sich um Navigationseigenschaften.</span><span class="sxs-lookup"><span data-stu-id="ede59-200">The `CourseAssignments` and `OfficeAssignment` properties are navigation properties.</span></span>

<span data-ttu-id="ede59-201">Da ein Dozent eine beliebige Anzahl von Kursen geben kann, wird `CourseAssignments` als Auflistung definiert.</span><span class="sxs-lookup"><span data-stu-id="ede59-201">An instructor can teach any number of courses, so `CourseAssignments` is defined as a collection.</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

<span data-ttu-id="ede59-202">Wenn eine Navigationseigenschaft mehrere Entitäten enthalten kann, muss deren Typ aus einer Liste bestehen, in der Einträge hinzugefügt, gelöscht und aktualisiert werden können.</span><span class="sxs-lookup"><span data-stu-id="ede59-202">If a navigation property can hold multiple entities, its type must be a list in which entries can be added, deleted, and updated.</span></span>  <span data-ttu-id="ede59-203">Sie können `ICollection<T>` oder einen Typ wie `List<T>` oder `HashSet<T>` angeben.</span><span class="sxs-lookup"><span data-stu-id="ede59-203">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="ede59-204">Wenn Sie `ICollection<T>` angeben, erstellt Entity Framework standardmäßig eine `HashSet<T>`-Auflistung.</span><span class="sxs-lookup"><span data-stu-id="ede59-204">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

<span data-ttu-id="ede59-205">Warum es sich dabei um `CourseAssignment`-Entitäten handelt, wird im Folgenden im Abschnitt zu m:n-Beziehungen erläutert.</span><span class="sxs-lookup"><span data-stu-id="ede59-205">The reason why these are `CourseAssignment` entities is explained below in the section about many-to-many relationships.</span></span>

<span data-ttu-id="ede59-206">Die Geschäftsregeln der Contoso University besagen, dass ein Dozent maximal ein Büro besitzen kann. Die `OfficeAssignment`-Eigenschaft enthält also eine einzige OfficeAssignment-Entität (diese kann NULL sein, wenn kein Büro zugewiesen ist).</span><span class="sxs-lookup"><span data-stu-id="ede59-206">Contoso University business rules state that an instructor can only have at most one office, so the `OfficeAssignment` property holds a single OfficeAssignment entity (which may be null if no office is assigned).</span></span>

```csharp
public OfficeAssignment OfficeAssignment { get; set; }
```

## <a name="create-the-officeassignment-entity"></a><span data-ttu-id="ede59-207">Erstellen der Entität „OfficeAssignment“</span><span class="sxs-lookup"><span data-stu-id="ede59-207">Create the OfficeAssignment entity</span></span>

![Entität „OfficeAssignment“](complex-data-model/_static/officeassignment-entity.png)

<span data-ttu-id="ede59-209">Erstellen Sie *Models/OfficeAssignment.cs* mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="ede59-209">Create *Models/OfficeAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/OfficeAssignment.cs)]

### <a name="the-key-attribute"></a><span data-ttu-id="ede59-210">Das Key-Attribut</span><span class="sxs-lookup"><span data-stu-id="ede59-210">The Key attribute</span></span>

<span data-ttu-id="ede59-211">Es gibt eine 1:0..1-Beziehung zwischen den Entitäten „Instructor“ und „OfficeAssignment“.</span><span class="sxs-lookup"><span data-stu-id="ede59-211">There's a one-to-zero-or-one relationship  between the Instructor and the OfficeAssignment entities.</span></span> <span data-ttu-id="ede59-212">Eine Bürozuweisung ist nur in Beziehung zu dem Dozenten vorhanden, dem sie zugewiesen ist. Daher ist deren Primärschlüssel auch der Fremdschlüssel für die Entität „Instructor“.</span><span class="sxs-lookup"><span data-stu-id="ede59-212">An office assignment only exists in relation to the instructor it's assigned to, and therefore its primary key is also its foreign key to the Instructor entity.</span></span> <span data-ttu-id="ede59-213">Entity Framework erkennt „InstructorID“ jedoch nicht automatisch als Primärschlüssel dieser Entität an, da dessen Name nicht der Namenskonvention „ID“ oder „classnameID“ folgt.</span><span class="sxs-lookup"><span data-stu-id="ede59-213">But the Entity Framework can't automatically recognize InstructorID as the primary key of this entity because its name doesn't follow the ID or classnameID naming convention.</span></span> <span data-ttu-id="ede59-214">Deshalb wird das `Key`-Attribut verwendet, um „InstructorID“ als Schlüssel zu erkennen:</span><span class="sxs-lookup"><span data-stu-id="ede59-214">Therefore, the `Key` attribute is used to identify it as the key:</span></span>

```csharp
[Key]
public int InstructorID { get; set; }
```

<span data-ttu-id="ede59-215">Sie können ebenfalls das `Key`-Attribut verwenden, wenn die Entität einen eigenen primären Schlüssel besitzt, Sie der Eigenschaft jedoch einen anderen Namen als „classnameID“ oder „ID“ geben möchten.</span><span class="sxs-lookup"><span data-stu-id="ede59-215">You can also use the `Key` attribute if the entity does have its own primary key but you want to name the property something other than classnameID or ID.</span></span>

<span data-ttu-id="ede59-216">Standardmäßig behandelt Entity Framework den Schlüssel nicht als datenbankgeneriert, da die Spalte für eine identifizierende Beziehung vorgesehen ist.</span><span class="sxs-lookup"><span data-stu-id="ede59-216">By default, EF treats the key as non-database-generated because the column is for an identifying relationship.</span></span>

### <a name="the-instructor-navigation-property"></a><span data-ttu-id="ede59-217">Die Navigationseigenschaft „Instructor“</span><span class="sxs-lookup"><span data-stu-id="ede59-217">The Instructor navigation property</span></span>

<span data-ttu-id="ede59-218">Die Entität „Instructor“ verfügt über eine auf NULL festlegbare `OfficeAssignment`-Navigationseigenschaft (da einem Dozenten möglicherweise kein Büro zugewiesen ist). Die OfficeAssignment-Entität verfügt über eine nicht auf NULL festlegbare `Instructor`-Navigationseigenschaft (da eine Bürozuweisung nicht ohne einen Dozenten vorhanden sein kann – `InstructorID` ist nicht auf NULL festlegbar).</span><span class="sxs-lookup"><span data-stu-id="ede59-218">The Instructor entity has a nullable `OfficeAssignment` navigation property (because an instructor might not have an office assignment), and the OfficeAssignment entity has a non-nullable `Instructor` navigation property (because an office assignment can't exist without an instructor -- `InstructorID` is non-nullable).</span></span> <span data-ttu-id="ede59-219">Wenn die Entität „Instructor“ mit der Entität „OfficeAssignment“ verknüpft ist, besitzt jede Entität in den Navigationseigenschaften einen Verweis auf die andere.</span><span class="sxs-lookup"><span data-stu-id="ede59-219">When an Instructor entity has a related OfficeAssignment entity, each entity will have a reference to the other one in its navigation property.</span></span>

<span data-ttu-id="ede59-220">Sie können ein `[Required]`-Attribut zur Navigationseigenschaft „Instructor“ hinzufügen, um anzugeben, dass ein zugehöriger Dozent vorhanden sein muss. Dies ist jedoch nicht notwendig, da der Fremdschlüssel `InstructorID` (bei dem es sich ebenfalls um den Schlüssel für diese Tabelle handelt) nicht auf NULL festlegbar ist.</span><span class="sxs-lookup"><span data-stu-id="ede59-220">You could put a `[Required]` attribute on the Instructor navigation property to specify that there must be a related instructor, but you don't have to do that because the `InstructorID` foreign key (which is also the key to this table) is non-nullable.</span></span>

## <a name="modify-the-course-entity"></a><span data-ttu-id="ede59-221">Ändern der Entität „Course“</span><span class="sxs-lookup"><span data-stu-id="ede59-221">Modify the Course Entity</span></span>

![Entität „Course“](complex-data-model/_static/course-entity.png)

<span data-ttu-id="ede59-223">Ersetzen Sie in *Models/Course.cs* den Code, den Sie zuvor hinzugefügt haben, durch folgenden Code.</span><span class="sxs-lookup"><span data-stu-id="ede59-223">In *Models/Course.cs*, replace the code you added earlier with the following code.</span></span> <span data-ttu-id="ede59-224">Die Änderungen werden hervorgehoben.</span><span class="sxs-lookup"><span data-stu-id="ede59-224">The changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Final&highlight=2,10,13,16,19,21,23)]

<span data-ttu-id="ede59-225">Die Entität „Course“ besitzt die Fremdschlüsseleigenschaft `DepartmentID`, die auf die zugehörige Entität „Department“ zeigt und die Navigationseigenschaft `Department` besitzt.</span><span class="sxs-lookup"><span data-stu-id="ede59-225">The course entity has a foreign key property `DepartmentID` which points to the related Department entity and it has a `Department` navigation property.</span></span>

<span data-ttu-id="ede59-226">Entity Framework erfordert nicht, dass Sie eine Fremdschlüsseleigenschaft zu Ihrem Datenmodell hinzufügen, wenn eine Navigationseigenschaft für eine verknüpfte Entität vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="ede59-226">The Entity Framework doesn't require you to add a foreign key property to your data model when you have a navigation property for a related entity.</span></span>  <span data-ttu-id="ede59-227">Entity Framework erstellt an den erforderlichen Stellen automatisch Fremdschlüssel in der Datenbank und erstellt [Schatteneigenschaften](https://docs.microsoft.com/ef/core/modeling/shadow-properties) für diese.</span><span class="sxs-lookup"><span data-stu-id="ede59-227">EF automatically creates foreign keys in the database wherever they're needed and creates [shadow properties](https://docs.microsoft.com/ef/core/modeling/shadow-properties) for them.</span></span> <span data-ttu-id="ede59-228">Durch Fremdschlüssel im Datenmodell können Updates einfacher und effizienter durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ede59-228">But having the foreign key in the data model can make updates simpler and more efficient.</span></span> <span data-ttu-id="ede59-229">Wenn Sie beispielsweise die Entität „Course“ zum Bearbeiten abrufen, ist die Entität „Department“ NULL, wenn Sie diese nicht laden. Wenn Sie die Entität „Course“ also aktualisieren, müssen Sie ebenfalls die Entität „Department“ abrufen.</span><span class="sxs-lookup"><span data-stu-id="ede59-229">For example, when you fetch a course entity to edit, the  Department entity is null if you don't load it, so when you update the course entity, you would have to first fetch the Department entity.</span></span> <span data-ttu-id="ede59-230">Wenn die Fremdschlüsseleigenschaft `DepartmentID` im Datenmodell vorhanden ist, müssen Sie die Entität „Department“ vor dem Update nicht abrufen.</span><span class="sxs-lookup"><span data-stu-id="ede59-230">When the foreign key property `DepartmentID` is included in the data model, you don't need to fetch the Department entity before you update.</span></span>

### <a name="the-databasegenerated-attribute"></a><span data-ttu-id="ede59-231">Das Attribut „DatabaseGenerated“</span><span class="sxs-lookup"><span data-stu-id="ede59-231">The DatabaseGenerated attribute</span></span>

<span data-ttu-id="ede59-232">Das `DatabaseGenerated`-Attribut mit dem `None`-Parameter der `CourseID`-Eigenschaft gibt an, dass Primärschlüsselwerte vom Benutzer bereitgestellt und nicht von der Datenbank generiert werden.</span><span class="sxs-lookup"><span data-stu-id="ede59-232">The `DatabaseGenerated` attribute with the `None` parameter on the `CourseID` property specifies that primary key values are provided by the user rather than generated by the database.</span></span>

```csharp
[DatabaseGenerated(DatabaseGeneratedOption.None)]
[Display(Name = "Number")]
public int CourseID { get; set; }
```

<span data-ttu-id="ede59-233">Standardmäßig geht Entity Framework davon aus, dass Primärschlüsselwerte von der Datenbank generiert werden.</span><span class="sxs-lookup"><span data-stu-id="ede59-233">By default, Entity Framework assumes that primary key values are generated by the database.</span></span> <span data-ttu-id="ede59-234">Das ist in den meisten Fällen erwünscht.</span><span class="sxs-lookup"><span data-stu-id="ede59-234">That's what you want in most scenarios.</span></span> <span data-ttu-id="ede59-235">Für Course-Entitäten verwenden Sie jedoch eine benutzerdefinierte Kursnummer, z.B. eine 1000er-Reihe für eine Abteilung, eine 2000er-Reihe für eine andere usw.</span><span class="sxs-lookup"><span data-stu-id="ede59-235">However, for Course entities, you'll use a user-specified course number such as a 1000 series for one department, a 2000 series for another department, and so on.</span></span>

<span data-ttu-id="ede59-236">Das `DatabaseGenerated`-Attribut kann ebenfalls verwendet werden, um Standardwerte zu generieren. Im Fall von Datenbankspalten wird es verwendet, um das Datum zu erfassen, an dem eine Zeile erstellt oder aktualisiert wurde.</span><span class="sxs-lookup"><span data-stu-id="ede59-236">The `DatabaseGenerated` attribute can also be used to generate default values, as in the case of database columns used to record the date a row was created or updated.</span></span>  <span data-ttu-id="ede59-237">Weitere Informationen finden Sie unter [Generated Properties (Generierte Eigenschaften)](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="ede59-237">For more information, see [Generated Properties](https://docs.microsoft.com/ef/core/modeling/generated-properties).</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="ede59-238">Fremdschlüssel- und Navigationseigenschaften</span><span class="sxs-lookup"><span data-stu-id="ede59-238">Foreign key and navigation properties</span></span>

<span data-ttu-id="ede59-239">Die Fremdschlüssel- und Navigationseigenschaften in der Entität „Course“ stellen folgende Beziehungen dar:</span><span class="sxs-lookup"><span data-stu-id="ede59-239">The foreign key properties and navigation properties in the Course entity reflect the following relationships:</span></span>

<span data-ttu-id="ede59-240">Ein Kurs ist einer Abteilung zugeordnet, sodass es aus den oben genannten Gründen eine `DepartmentID`-Fremdschlüsseleigenschaft und eine `Department`-Navigationseigenschaft gibt.</span><span class="sxs-lookup"><span data-stu-id="ede59-240">A course is assigned to one department, so there's a `DepartmentID` foreign key and a `Department` navigation property for the reasons mentioned above.</span></span>

```csharp
public int DepartmentID { get; set; }
public Department Department { get; set; }
```

<span data-ttu-id="ede59-241">In einem Kurs können beliebig viele Studenten angemeldet sein, darum stellt die `Enrollments`-Navigationseigenschaft eine Auflistung dar:</span><span class="sxs-lookup"><span data-stu-id="ede59-241">A course can have any number of students enrolled in it, so the `Enrollments` navigation property is a collection:</span></span>

```csharp
public ICollection<Enrollment> Enrollments { get; set; }
```

<span data-ttu-id="ede59-242">Ein Kurs kann von mehreren Dozenten unterrichtet werden, darum stellt die `CourseAssignments`-Navigationseigenschaft eine Auflistung dar (der Typ `CourseAssignment` wird [später](#many-to-many-relationships) erklärt):</span><span class="sxs-lookup"><span data-stu-id="ede59-242">A course may be taught by multiple instructors, so the `CourseAssignments` navigation property is a collection (the type `CourseAssignment` is explained [later](#many-to-many-relationships)):</span></span>

```csharp
public ICollection<CourseAssignment> CourseAssignments { get; set; }
```

## <a name="create-the-department-entity"></a><span data-ttu-id="ede59-243">Erstellen der Entität „Department“</span><span class="sxs-lookup"><span data-stu-id="ede59-243">Create the Department entity</span></span>

![Entität „Department“](complex-data-model/_static/department-entity.png)


<span data-ttu-id="ede59-245">Erstellen Sie *Models/Department.cs* mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="ede59-245">Create *Models/Department.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Begin)]

### <a name="the-column-attribute"></a><span data-ttu-id="ede59-246">Das Column-Attribut</span><span class="sxs-lookup"><span data-stu-id="ede59-246">The Column attribute</span></span>

<span data-ttu-id="ede59-247">Zuvor haben Sie das `Column`-Attribut verwendet, um die Zuordnung des Spaltennamens zu ändern.</span><span class="sxs-lookup"><span data-stu-id="ede59-247">Earlier you used the `Column` attribute to change column name mapping.</span></span> <span data-ttu-id="ede59-248">Im Code für die Entität „Department“ wird das `Column`-Attribut verwendet, um die SQL-Datentypzuordnung zu ändern, sodass die Spalte mithilfe des SQL Server-Typs „money“ in der Datenbank definiert wird:</span><span class="sxs-lookup"><span data-stu-id="ede59-248">In the code for the Department entity, the `Column` attribute is being used to change SQL data type mapping so that the column will be defined using the SQL Server money type in the database:</span></span>

```csharp
[Column(TypeName="money")]
public decimal Budget { get; set; }
```

<span data-ttu-id="ede59-249">Die Spaltenzuordnung ist im Allgemeinen nicht erforderlich, da Entity Framework den geeigneten SQL Server-Datentyp auf dem CLR-Typ basierend auswählt, den Sie für die Eigenschaft definieren.</span><span class="sxs-lookup"><span data-stu-id="ede59-249">Column mapping is generally not required, because the Entity Framework chooses the appropriate SQL Server data type based on the CLR type that you define for the property.</span></span> <span data-ttu-id="ede59-250">Der CLR-Typ `decimal` wird dem SQL Server-Typ `decimal` zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="ede59-250">The CLR `decimal` type maps to a SQL Server `decimal` type.</span></span> <span data-ttu-id="ede59-251">In diesem Fall wissen Sie jedoch, dass die Spalte Währungsbeträge enthält. Hierfür ist der Datentyp „money“ besser geeignet.</span><span class="sxs-lookup"><span data-stu-id="ede59-251">But in this case you know that the column will be holding currency amounts, and the money data type is more appropriate for that.</span></span>

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="ede59-252">Fremdschlüssel- und Navigationseigenschaften</span><span class="sxs-lookup"><span data-stu-id="ede59-252">Foreign key and navigation properties</span></span>

<span data-ttu-id="ede59-253">Die Fremdschlüssel- und Navigationseigenschaften stellen folgende Beziehungen dar:</span><span class="sxs-lookup"><span data-stu-id="ede59-253">The foreign key and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="ede59-254">Eine Abteilung kann einen Administrator besitzen oder nicht, und bei einem Administrator handelt es sich immer um einen Dozenten.</span><span class="sxs-lookup"><span data-stu-id="ede59-254">A department may or may not have an administrator, and an administrator is always an instructor.</span></span> <span data-ttu-id="ede59-255">Deshalb wird die `InstructorID`-Eigenschaft als Fremdschlüssel zur Entität „Instructor“ hinzugefügt, und ein Fragezeichen wird nach der Typbezeichnung `int` hinzugefügt, um die Eigenschaft als auf NULL festlegbar zu markieren.</span><span class="sxs-lookup"><span data-stu-id="ede59-255">Therefore the `InstructorID` property is included as the foreign key to the Instructor entity, and a question mark is added after the `int` type designation to mark the property as nullable.</span></span> <span data-ttu-id="ede59-256">Die Navigationseigenschaft heißt `Administrator`, enthält jedoch eine Instructor-Entität:</span><span class="sxs-lookup"><span data-stu-id="ede59-256">The navigation property is named `Administrator` but holds an Instructor entity:</span></span>

```csharp
public int? InstructorID { get; set; }
public Instructor Administrator { get; set; }
```

<span data-ttu-id="ede59-257">Eine Abteilung kann viele Kurse aufweisen, darum gibt es die Navigationseigenschaft „Courses“:</span><span class="sxs-lookup"><span data-stu-id="ede59-257">A department may have many courses, so there's a Courses navigation property:</span></span>

```csharp
public ICollection<Course> Courses { get; set; }
```

> [!NOTE]
> <span data-ttu-id="ede59-258">Gemäß der Konvention aktiviert Entity Framework das kaskadierende Delete für nicht auf NULL festlegbare Fremdschlüssel und für m:n-Beziehungen.</span><span class="sxs-lookup"><span data-stu-id="ede59-258">By convention, the Entity Framework enables cascade delete for non-nullable foreign keys and for many-to-many relationships.</span></span> <span data-ttu-id="ede59-259">Dies kann zu zirkulären Regeln für kaskadierende Deletes führen, wodurch eine Ausnahme ausgelöst wird, wenn Sie versuchen, eine Migration hinzuzufügen.</span><span class="sxs-lookup"><span data-stu-id="ede59-259">This can result in circular cascade delete rules, which will cause an exception when you try to add a migration.</span></span> <span data-ttu-id="ede59-260">Wenn Sie die Eigenschaft „Department.InstructorID“ nicht als auf NULL festlegbar definiert haben, würde Entity Framework eine Regel für kaskadierende Deletes konfigurieren, damit der Dozent gelöscht wird, wenn Sie die Abteilung löschen. Dies ist jedoch nicht erwünscht.</span><span class="sxs-lookup"><span data-stu-id="ede59-260">For example, if you didn't define the Department.InstructorID property as nullable, EF would configure a cascade delete rule to delete the instructor when you delete the department, which isn't what you want to have happen.</span></span> <span data-ttu-id="ede59-261">Wenn Ihre Geschäftsregeln erfordern, dass die `InstructorID`-Eigenschaft nicht auf NULL festlegbar ist, müssen Sie folgende Fluent-API-Anweisung verwenden, um kaskadierende Deletes für die Eigenschaft zu deaktivieren:</span><span class="sxs-lookup"><span data-stu-id="ede59-261">If your business rules required the `InstructorID` property to be non-nullable, you would have to use the following fluent API statement to disable cascade delete on the relationship:</span></span>
> ```csharp
> modelBuilder.Entity<Department>()
>    .HasOne(d => d.Administrator)
>    .WithMany()
>    .OnDelete(DeleteBehavior.Restrict)
> ```

## <a name="modify-the-enrollment-entity"></a><span data-ttu-id="ede59-262">Ändern der Entität „Enrollment“</span><span class="sxs-lookup"><span data-stu-id="ede59-262">Modify the Enrollment entity</span></span>

![Entität „Enrollment“](complex-data-model/_static/enrollment-entity.png)

<span data-ttu-id="ede59-264">Ersetzen Sie in *Models/Enrollment.cs* den Code, den Sie zuvor hinzugefügt haben, durch folgenden Code:</span><span class="sxs-lookup"><span data-stu-id="ede59-264">In *Models/Enrollment.cs*, replace the code you added earlier with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Final&highlight=1-2,16)]

### <a name="foreign-key-and-navigation-properties"></a><span data-ttu-id="ede59-265">Fremdschlüssel- und Navigationseigenschaften</span><span class="sxs-lookup"><span data-stu-id="ede59-265">Foreign key and navigation properties</span></span>

<span data-ttu-id="ede59-266">Die Fremdschlüssel- und Navigationseigenschaften stellen folgende Beziehungen dar:</span><span class="sxs-lookup"><span data-stu-id="ede59-266">The foreign key properties and navigation properties reflect the following relationships:</span></span>

<span data-ttu-id="ede59-267">Ein Anmeldungsdatensatz gilt für einen einzelnen Kurs, sodass es eine `CourseID`-Fremdschlüsseleigenschaft und eine `Course`-Navigationseigenschaft gibt:</span><span class="sxs-lookup"><span data-stu-id="ede59-267">An enrollment record is for a single course, so there's a `CourseID` foreign key property and a `Course` navigation property:</span></span>

```csharp
public int CourseID { get; set; }
public Course Course { get; set; }
```

<span data-ttu-id="ede59-268">Ein Anmeldungsdatensatz gilt für einen einzelnen Studenten, sodass es eine `StudentID`-Fremdschlüsseleigenschaft und eine `Student`-Navigationseigenschaft gibt:</span><span class="sxs-lookup"><span data-stu-id="ede59-268">An enrollment record is for a single student, so there's a `StudentID` foreign key property and a `Student` navigation property:</span></span>

```csharp
public int StudentID { get; set; }
public Student Student { get; set; }
```

## <a name="many-to-many-relationships"></a><span data-ttu-id="ede59-269">n:n-Beziehungen</span><span class="sxs-lookup"><span data-stu-id="ede59-269">Many-to-Many Relationships</span></span>

<span data-ttu-id="ede59-270">Es besteht eine m:n-Beziehung zwischen den Entitäten „Student“ und „Course“, und die Entitätsfunktionen „Enrollment“ liegen als m:n-Jointabelle *mit Nutzlast* in der Datenbank vor.</span><span class="sxs-lookup"><span data-stu-id="ede59-270">There's a many-to-many relationship between the Student and Course entities, and the Enrollment entity functions as a many-to-many join table *with payload* in the database.</span></span> <span data-ttu-id="ede59-271">„Mit Nutzlast“ bedeutet, dass die Tabelle „Enrollment“ außer den Fremdschlüsseln für die verknüpften Tabellen (in diesem Fall ein Primärschlüssel und eine Grade-Eigenschaft) zusätzliche Daten enthält.</span><span class="sxs-lookup"><span data-stu-id="ede59-271">"With payload" means that the Enrollment table contains additional data besides foreign keys for the joined tables (in this case, a primary key and a Grade property).</span></span>

<span data-ttu-id="ede59-272">Die folgende Abbildung stellt dar, wie diese Beziehungen in einem Entitätsdiagramm aussehen.</span><span class="sxs-lookup"><span data-stu-id="ede59-272">The following illustration shows what these relationships look like in an entity diagram.</span></span> <span data-ttu-id="ede59-273">(Dieses Diagramm wurde mithilfe der Entity Framework Power Tools für Entity Framework 6.x erstellt. Das Erstellen des Diagramms ist nicht Teil des Tutorials, sondern wird hier nur zur Veranschaulichung verwendet.)</span><span class="sxs-lookup"><span data-stu-id="ede59-273">(This diagram was generated using the Entity Framework Power Tools for EF 6.x; creating the diagram isn't part of the tutorial, it's just being used here as an illustration.)</span></span>

![m:n-Beziehung zwischen „Student“ und „Course“](complex-data-model/_static/student-course.png)

<span data-ttu-id="ede59-275">Jede Beziehung weist an einem Ende „1“ und am anderen Ende „\*“ auf, wodurch eine 1:n-Beziehung dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="ede59-275">Each relationship line has a 1 at one end and an asterisk (\*) at the other, indicating a one-to-many relationship.</span></span>

<span data-ttu-id="ede59-276">Wenn in der Tabelle „Enrollment“ nicht die Information „Grade“ enthalten wäre, müsste diese nur die beiden Fremdschlüssel „CourseID“ und „StudentID“ enthalten.</span><span class="sxs-lookup"><span data-stu-id="ede59-276">If the Enrollment table didn't include grade information, it would only need to contain the two foreign keys CourseID and StudentID.</span></span> <span data-ttu-id="ede59-277">In diesem Fall würde es sich um eine m:n-Jointabelle ohne Nutzlast (oder um eine reine Jointabelle) in der Datenbank handeln.</span><span class="sxs-lookup"><span data-stu-id="ede59-277">In that case, it would be a many-to-many join table without payload (or a pure join table) in the database.</span></span> <span data-ttu-id="ede59-278">Die Entitäten „Instructor“ und „Course“ weisen diese Art von m:n-Beziehung auf. Als Nächstes sollten Sie also eine Entitätsklasse erstellen, die als Jointabelle ohne Nutzlast fungiert.</span><span class="sxs-lookup"><span data-stu-id="ede59-278">The Instructor and Course entities have that kind of many-to-many relationship, and your next step is to create an entity class to function as a join table without payload.</span></span>

<span data-ttu-id="ede59-279">(Entity Framework 6.x unterstützt implizite Jointabellen für m:n-Beziehungen, Entity Framework Core jedoch nicht.</span><span class="sxs-lookup"><span data-stu-id="ede59-279">(EF 6.x supports implicit join tables for many-to-many relationships, but EF Core doesn't.</span></span> <span data-ttu-id="ede59-280">Weitere Informationen finden Sie in der [Diskussion im GitHub-Repository für Entity Framework Core](https://github.com/aspnet/EntityFramework/issues/1368).)</span><span class="sxs-lookup"><span data-stu-id="ede59-280">For more information, see the [discussion in the EF Core GitHub repository](https://github.com/aspnet/EntityFramework/issues/1368).)</span></span>

## <a name="the-courseassignment-entity"></a><span data-ttu-id="ede59-281">Die Entität „CourseAssignment“</span><span class="sxs-lookup"><span data-stu-id="ede59-281">The CourseAssignment entity</span></span>

![Entität „CourseAssignment“](complex-data-model/_static/courseassignment-entity.png)

<span data-ttu-id="ede59-283">Erstellen Sie *Models/CourseAssignment.cs* mit folgendem Code:</span><span class="sxs-lookup"><span data-stu-id="ede59-283">Create *Models/CourseAssignment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/CourseAssignment.cs)]

### <a name="join-entity-names"></a><span data-ttu-id="ede59-284">Namen von Joinentitäten</span><span class="sxs-lookup"><span data-stu-id="ede59-284">Join entity names</span></span>

<span data-ttu-id="ede59-285">Eine Jointabelle ist in der Datenbank für die m:n-Beziehung zwischen „Instructor“ und „Course“ erforderlich. Diese muss über eine Entitätenmenge dargestellt werden.</span><span class="sxs-lookup"><span data-stu-id="ede59-285">A join table is required in the database for the Instructor-to-Courses many-to-many relationship, and it has to be represented by an entity set.</span></span> <span data-ttu-id="ede59-286">Es ist üblich, eine Joinentität `EntityName1EntityName2` zu benennen, was in diesem Fall `CourseInstructor` entsprechen würde.</span><span class="sxs-lookup"><span data-stu-id="ede59-286">It's common to name a join entity `EntityName1EntityName2`, which in this case would be `CourseInstructor`.</span></span> <span data-ttu-id="ede59-287">Es wird jedoch empfohlen, einen Namen auszuwählen, der die Beziehung beschreibt.</span><span class="sxs-lookup"><span data-stu-id="ede59-287">However, we recommend that you choose a name that describes the relationship.</span></span> <span data-ttu-id="ede59-288">Datenmodelle fangen einfach an und werden dann größer. Mit der Zeit werden Joins ohne Nutzlast Nutzlasten hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="ede59-288">Data models start out simple and grow, with no-payload joins frequently getting payloads later.</span></span> <span data-ttu-id="ede59-289">Wenn Sie mit einem aussagekräftigen Entitätsnamen beginnen, müssen Sie diesen später nicht ändern.</span><span class="sxs-lookup"><span data-stu-id="ede59-289">If you start with a descriptive entity name, you won't have to change the name later.</span></span> <span data-ttu-id="ede59-290">Im Idealfall verfügt die Joinentität über einen eigenen, nachvollziehbaren Namen (der möglicherweise aus nur einem Wort besteht) in der Geschäftsdomäne.</span><span class="sxs-lookup"><span data-stu-id="ede59-290">Ideally, the join entity would have its own natural (possibly single word) name in the business domain.</span></span> <span data-ttu-id="ede59-291">„Books“ und „Customers“ könnten beispielsweise über „Ratings“ verknüpft werden.</span><span class="sxs-lookup"><span data-stu-id="ede59-291">For example, Books and Customers could be linked through Ratings.</span></span> <span data-ttu-id="ede59-292">Für diese Beziehung ist `CourseAssignment` besser als `CourseInstructor` geeignet.</span><span class="sxs-lookup"><span data-stu-id="ede59-292">For this relationship, `CourseAssignment` is a better choice than `CourseInstructor`.</span></span>

### <a name="composite-key"></a><span data-ttu-id="ede59-293">Zusammengesetzte Schlüssel</span><span class="sxs-lookup"><span data-stu-id="ede59-293">Composite key</span></span>

<span data-ttu-id="ede59-294">Da die Fremdschlüssel nicht auf NULL festlegbar sind und zusammen jede Zeile der Tabelle eindeutig identifizieren, ist kein separater Primärschlüssel erforderlich.</span><span class="sxs-lookup"><span data-stu-id="ede59-294">Since the foreign keys are not nullable and together uniquely identify each row of the table, there's no need for a separate primary key.</span></span> <span data-ttu-id="ede59-295">Die Eigenschaften *InstructorID* und *CourseID* fungieren als zusammengesetzter Primärschlüssel.</span><span class="sxs-lookup"><span data-stu-id="ede59-295">The *InstructorID* and *CourseID* properties should function as a composite primary key.</span></span> <span data-ttu-id="ede59-296">Die einzige Möglichkeit zum Identifizieren von zusammengesetzten Primärschlüsseln für Entity Framework besteht in der Verwendung der *Fluent-API*. Dies kann nicht mithilfe von Attributen durchgeführt werden.</span><span class="sxs-lookup"><span data-stu-id="ede59-296">The only way to identify composite primary keys to EF is by using the *fluent API* (it can't be done by using attributes).</span></span> <span data-ttu-id="ede59-297">Weitere Informationen zum Konfigurieren von zusammengesetzten Primärschlüsseln finden Sie im nächsten Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="ede59-297">You'll see how to configure the composite primary key in the next section.</span></span>

<span data-ttu-id="ede59-298">Durch den zusammengesetzten Schlüssel wird sichergestellt, dass nicht mehrere Zeilen für denselben Dozenten und Kurs vorhanden sein können, obwohl es mehrere Zeilen für einen Kurs und mehrere Zeilen für einen Dozenten gibt.</span><span class="sxs-lookup"><span data-stu-id="ede59-298">The composite key ensures that while you can have multiple rows for one course, and multiple rows for one instructor, you can't have multiple rows for the same instructor and course.</span></span> <span data-ttu-id="ede59-299">Die Joinentität `Enrollment` definiert ihren eigenen Primärschlüssel, dadurch sind Duplikate dieser Art möglich.</span><span class="sxs-lookup"><span data-stu-id="ede59-299">The `Enrollment` join entity defines its own primary key, so duplicates of this sort are possible.</span></span> <span data-ttu-id="ede59-300">Sie können einen eindeutigen Index zu den Feldern mit Fremdschlüsseln hinzufügen, um solche Duplikate zu verhindern, oder `Enrollment` mit einem zusammengesetzten Primärschlüssel konfigurieren, der `CourseAssignment` ähnelt.</span><span class="sxs-lookup"><span data-stu-id="ede59-300">To prevent such duplicates, you could add a unique index on the foreign key fields, or configure `Enrollment` with a primary composite key similar to `CourseAssignment`.</span></span> <span data-ttu-id="ede59-301">Weitere Informationen finden Sie unter [Indizes](https://docs.microsoft.com/ef/core/modeling/indexes).</span><span class="sxs-lookup"><span data-stu-id="ede59-301">For more information, see [Indexes](https://docs.microsoft.com/ef/core/modeling/indexes).</span></span>

## <a name="update-the-database-context"></a><span data-ttu-id="ede59-302">Aktualisieren des Datenbankkontexts</span><span class="sxs-lookup"><span data-stu-id="ede59-302">Update the database context</span></span>

<span data-ttu-id="ede59-303">Fügen Sie folgenden hervorgehobenen Code zur Datei *Data/SchoolContext.cs* hinzu:</span><span class="sxs-lookup"><span data-stu-id="ede59-303">Add the following highlighted code to the *Data/SchoolContext.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_BeforeInheritance&highlight=15-18,25-31)]

<span data-ttu-id="ede59-304">Durch diesen Code werden neue Entitäten hinzugefügt, und der zusammengesetzte Primärschlüssel der Entität „CourseAssignment“ wird konfiguriert.</span><span class="sxs-lookup"><span data-stu-id="ede59-304">This code adds the new entities and configures the CourseAssignment entity's composite primary key.</span></span>

## <a name="fluent-api-alternative-to-attributes"></a><span data-ttu-id="ede59-305">Fluent-API-Alternativen für Attribute</span><span class="sxs-lookup"><span data-stu-id="ede59-305">Fluent API alternative to attributes</span></span>

<span data-ttu-id="ede59-306">Der Code in der `OnModelCreating`-Methode der `DbContext`-Klasse verwendet die *Fluent-API* zum Konfigurieren des Verhaltens von Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="ede59-306">The code in the `OnModelCreating` method of the `DbContext` class uses the *fluent API* to configure EF behavior.</span></span> <span data-ttu-id="ede59-307">Die API wird als „Fluent“ bezeichnet, da sie häufig durch das Verketten mehrerer Methodenaufrufe zu einer einzigen Anweisung verwendet wird. Ein Beispiel hierfür finden Sie in der [EF Core documentation (Dokumentation zu Entity Framework)](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span><span class="sxs-lookup"><span data-stu-id="ede59-307">The API is called "fluent" because it's often used by stringing a series of method calls together into a single statement, as in this example from the [EF Core documentation](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Blog>()
        .Property(b => b.Url)
        .IsRequired();
}
```

<span data-ttu-id="ede59-308">In diesem Tutorial verwendet Sie die Fluent-API nur für Datenbankzuordnungen, die nicht mithilfe von Attributen durchgeführt werden können.</span><span class="sxs-lookup"><span data-stu-id="ede59-308">In this tutorial, you're using the fluent API only for database mapping that you can't do with attributes.</span></span> <span data-ttu-id="ede59-309">Sie können die Fluent-API jedoch ebenfalls verwenden, um den Großteil der Regeln für die Formatierung, Validierung und Zuordnung anzugeben, die Sie mithilfe von Attributen festlegen können.</span><span class="sxs-lookup"><span data-stu-id="ede59-309">However, you can also use the fluent API to specify most of the formatting, validation, and mapping rules that you can do by using attributes.</span></span> <span data-ttu-id="ede59-310">Manche Attribute, z.B. `MinimumLength`, können nicht mit der Fluent-API angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="ede59-310">Some attributes such as `MinimumLength` can't be applied with the fluent API.</span></span> <span data-ttu-id="ede59-311">Wie bereits erwähnt, ändert `MinimumLength` das Schema nicht, sondern wendet lediglich eine client- und serverseitige Validierungsregel an.</span><span class="sxs-lookup"><span data-stu-id="ede59-311">As mentioned previously, `MinimumLength` doesn't change the schema, it only applies a client and server side validation rule.</span></span>

<span data-ttu-id="ede59-312">Einige Entwickler bevorzugen die exklusive Verwendung der Fluent-API, um ihre Entitätsklassen „rein“ zu halten.</span><span class="sxs-lookup"><span data-stu-id="ede59-312">Some developers prefer to use the fluent API exclusively so that they can keep their entity classes "clean."</span></span> <span data-ttu-id="ede59-313">Sie können Attribute und die Fluent-API verwenden, wenn Sie möchten. Einige Anpassungen können nur mithilfe der Fluent-API vorgenommen werden. Im Allgemeinen wird jedoch empfohlen, einen der beiden Ansätze auszuwählen und diesen so konsistent wie möglich zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="ede59-313">You can mix attributes and fluent API if you want, and there are a few customizations that can only be done by using fluent API, but in general the recommended practice is to choose one of these two approaches and use that consistently as much as possible.</span></span> <span data-ttu-id="ede59-314">Wenn Sie beide verwenden, beachten Sie, dass Attribute durch die Fluent-API überschrieben werden, wenn ein Konflikt vorliegt.</span><span class="sxs-lookup"><span data-stu-id="ede59-314">If you do use both, note that wherever there's a conflict, Fluent API overrides attributes.</span></span>

<span data-ttu-id="ede59-315">Weitere Informationen zu Attributen und Fluent-API im Vergleich finden Sie unter [Methods of configuration (Konfigurationsmethoden)](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span><span class="sxs-lookup"><span data-stu-id="ede59-315">For more information about attributes vs. fluent API, see [Methods of configuration](https://docs.microsoft.com/ef/core/modeling/#methods-of-configuration).</span></span>

## <a name="entity-diagram-showing-relationships"></a><span data-ttu-id="ede59-316">Entitätsdiagramm mit angezeigten Beziehungen</span><span class="sxs-lookup"><span data-stu-id="ede59-316">Entity Diagram Showing Relationships</span></span>

<span data-ttu-id="ede59-317">Die folgende Abbildung stellt das Diagramm dar, das von Entity Framework Power Tools für das vollständige Modell „School“ erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="ede59-317">The following illustration shows the diagram that the Entity Framework Power Tools create for the completed School model.</span></span>

![Entitätsdiagramm](complex-data-model/_static/diagram.png)

<span data-ttu-id="ede59-319">Neben den Linien für 1:n-Beziehungen (1:\*) werden die Linien für „Eins-bis-Null-oder Eins“-Beziehungen (1:0..1) zwischen den Entitäten „Instructor“ und „OfficeAssignment“ und die Linien für 0..1:n-Beziehungen (0..1:*) zwischen den Entitäten „Instructor“ und „Department“ dargestellt.</span><span class="sxs-lookup"><span data-stu-id="ede59-319">Besides the one-to-many relationship lines (1 to \*), you can see here the one-to-zero-or-one relationship line (1 to 0..1) between the Instructor and OfficeAssignment entities and the zero-or-one-to-many relationship line (0..1 to *) between the Instructor and Department entities.</span></span>

## <a name="seed-the-database-with-test-data"></a><span data-ttu-id="ede59-320">Ausführen eines Seedings für die Datenbank mit Testdaten</span><span class="sxs-lookup"><span data-stu-id="ede59-320">Seed the Database with Test Data</span></span>

<span data-ttu-id="ede59-321">Ersetzen Sie den Code in der Datei *Data/DbInitializer.cs* durch folgenden Code, um Startwertdaten für die neu erstellten Entitäten bereitzustellen.</span><span class="sxs-lookup"><span data-stu-id="ede59-321">Replace the code in the *Data/DbInitializer.cs* file with the following code in order to provide seed data for the new entities you've created.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Final)]

<span data-ttu-id="ede59-322">Wie im ersten Tutorial dargestellt werden durch diesen Code überwiegend neue Entitätsobjekte dargestellt und für die Tests erforderliche Beispieldaten in Eigenschaften geladen.</span><span class="sxs-lookup"><span data-stu-id="ede59-322">As you saw in the first tutorial, most of this code simply creates new entity objects and loads sample data into properties as required for testing.</span></span> <span data-ttu-id="ede59-323">Achten Sie darauf, wie die m:n-Beziehungen verarbeitet werden: Der Code erstellt Beziehungen, indem Entitäten in den Joinentitätenmengen `Enrollments` und `CourseAssignment` erstellt werden.</span><span class="sxs-lookup"><span data-stu-id="ede59-323">Notice how the many-to-many relationships are handled: the code creates relationships by creating entities in the `Enrollments` and `CourseAssignment` join entity sets.</span></span>

## <a name="add-a-migration"></a><span data-ttu-id="ede59-324">Hinzufügen einer Migration</span><span class="sxs-lookup"><span data-stu-id="ede59-324">Add a migration</span></span>

<span data-ttu-id="ede59-325">Speichern Sie Ihre Änderungen, und erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="ede59-325">Save your changes and build the project.</span></span> <span data-ttu-id="ede59-326">Öffnen Sie dann das Befehlsfenster im Projektordner, und geben Sie den Befehl `migrations add` ein (verwenden Sie den Befehl „update-database“ noch nicht):</span><span class="sxs-lookup"><span data-stu-id="ede59-326">Then open the command window in the project folder and enter the `migrations add` command (don't do the update-database command yet):</span></span>

```console
dotnet ef migrations add ComplexDataModel
```

<span data-ttu-id="ede59-327">Sie erhalten eine Warnung über möglichen Datenverlust.</span><span class="sxs-lookup"><span data-stu-id="ede59-327">You get a warning about possible data loss.</span></span>

```text
An operation was scaffolded that may result in the loss of data. Please review the migration for accuracy.
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="ede59-328">Wenn Sie versucht haben, den Befehl `database update` zu diesem Zeitpunkt auszuführen (führen Sie diesen noch nicht aus), erhalten Sie folgende Fehlermeldung:</span><span class="sxs-lookup"><span data-stu-id="ede59-328">If you tried to run the `database update` command at this point (don't do it yet), you would get the following error:</span></span>

> <span data-ttu-id="ede59-329">Die ALTER TABLE-Anweisung steht in Konflikt mit der FOREIGN KEY-Einschränkung „FK_dbo.Course_dbo.Department_DepartmentID“.</span><span class="sxs-lookup"><span data-stu-id="ede59-329">The ALTER TABLE statement conflicted with the FOREIGN KEY constraint "FK_dbo.Course_dbo.Department_DepartmentID".</span></span> <span data-ttu-id="ede59-330">Der Konflikt trat in der „ContosoUniversity“-Datenbank, Tabelle „dbo.Department“, Spalte „DepartmentID“ auf.</span><span class="sxs-lookup"><span data-stu-id="ede59-330">The conflict occurred in database "ContosoUniversity", table "dbo.Department", column 'DepartmentID'.</span></span>

<span data-ttu-id="ede59-331">In einigen Fällen müssen Sie beim Ausführen von Migrationen mit vorhandenen Daten Stub-Daten in die Datenbank einfügen, um die Fremdschlüsseleinschränkungen zu erfüllen.</span><span class="sxs-lookup"><span data-stu-id="ede59-331">Sometimes when you execute migrations with existing data, you need to insert stub data into the database to satisfy foreign key constraints.</span></span> <span data-ttu-id="ede59-332">Der generierte Code in der `Up`-Methode fügt den nicht auf NULL festlegbaren Fremdschlüssel „DepartmentID“ zur Tabelle „Course“ hinzu.</span><span class="sxs-lookup"><span data-stu-id="ede59-332">The generated code in the `Up` method adds a non-nullable DepartmentID foreign key to the Course table.</span></span> <span data-ttu-id="ede59-333">Wenn beim Ausführen des Codes bereits Zeilen in der Tabelle „Course“ vorhanden sind, schlägt der `AddColumn`-Vorgang fehl, da SQL Server keinen Wert in eine Spalte einfügen kann, die nicht NULL sein darf.</span><span class="sxs-lookup"><span data-stu-id="ede59-333">If there are already rows in the Course table when the code runs, the `AddColumn` operation fails because SQL Server doesn't know what value to put in the column that can't be null.</span></span> <span data-ttu-id="ede59-334">In diesem Tutorial führen Sie die Migration auf eine neue Datenbank durch. In einer Produktionsanwendung müssten Sie jedoch dafür sorgen, dass die Migration vorhandene Daten verarbeiten kann. Ein Beispiel hierfür finden Sie in den folgenden Anweisungen.</span><span class="sxs-lookup"><span data-stu-id="ede59-334">For this tutorial you'll run the migration on a new database, but in a production application you'd have to make the migration handle existing data, so the following directions show an example of how to do that.</span></span>

<span data-ttu-id="ede59-335">Damit diese Migration mit vorhandenen Daten funktioniert, müssen Sie den Code ändern, damit dieser der neuen Spalte einen Standardwert zuweist, und eine Stub-Abteilung namens „Temp“ erstellen, die als Standardabteilung fungiert.</span><span class="sxs-lookup"><span data-stu-id="ede59-335">To make this migration work with existing data you have to change the code to give the new column a default value, and create a stub department named "Temp" to act as the default department.</span></span> <span data-ttu-id="ede59-336">Folglich sind alle vorhandenen Course-Zeilen mit der Abteilung „Temp“ verknüpft, nachdem die Methode `Up` ausgeführt wurde.</span><span class="sxs-lookup"><span data-stu-id="ede59-336">As a result, existing Course rows will all be related to the "Temp" department after the `Up` method runs.</span></span>

* <span data-ttu-id="ede59-337">Öffnen Sie die Datei *{timestamp}_ComplexDataModel.cs*.</span><span class="sxs-lookup"><span data-stu-id="ede59-337">Open the *{timestamp}_ComplexDataModel.cs* file.</span></span>

* <span data-ttu-id="ede59-338">Kommentieren Sie die Codezeile aus, die die Spalte „DepartmentID“ zur Tabelle „Course“ hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="ede59-338">Comment out the line of code that adds the DepartmentID column to the Course table.</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CommentOut&highlight=9-13)]

* <span data-ttu-id="ede59-339">Fügen Sie folgenden hervorgehobenen Code nach dem Code hinzu, der die Tabelle „Department“ erstellt:</span><span class="sxs-lookup"><span data-stu-id="ede59-339">Add the following highlighted code after the code that creates the Department table:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/20170215234014_ComplexDataModel.cs?name=snippet_CreateDefaultValue&highlight=22-32)]

<span data-ttu-id="ede59-340">In einer Produktionsanwendung würden Sie Code oder Skripts schreiben, um Department-Zeilen hinzuzufügen und die Course-Zeilen mit den neuen Department-Zeilen zu verknüpfen.</span><span class="sxs-lookup"><span data-stu-id="ede59-340">In a production application, you would write code or scripts to add Department rows and relate Course rows to the new Department rows.</span></span> <span data-ttu-id="ede59-341">Danach benötigen Sie die Abteilung „Temp“ und den Standardwert für die Spalte „Course.DepartmentID“ nicht mehr.</span><span class="sxs-lookup"><span data-stu-id="ede59-341">You would then no longer need the "Temp" department or the default value on the Course.DepartmentID column.</span></span>

<span data-ttu-id="ede59-342">Speichern Sie Ihre Änderungen, und erstellen Sie das Projekt.</span><span class="sxs-lookup"><span data-stu-id="ede59-342">Save your changes and build the project.</span></span>

## <a name="change-the-connection-string-and-update-the-database"></a><span data-ttu-id="ede59-343">Ändern der Verbindungszeichenfolge und Aktualisieren der Datenbank</span><span class="sxs-lookup"><span data-stu-id="ede59-343">Change the connection string and update the database</span></span>

<span data-ttu-id="ede59-344">Sie haben nun neuen Code zur `DbInitializer`-Klasse hinzugefügt, der Startwertdaten für neue Entitäten zu einer leeren Datenbank hinzufügt.</span><span class="sxs-lookup"><span data-stu-id="ede59-344">You now have new code in the `DbInitializer` class that adds seed data for the new entities to an empty database.</span></span> <span data-ttu-id="ede59-345">Ändern Sie den Namen der Datenbank in der Verbindungszeichenfolge in *appsetting.json* in „ContosoUniversity3“ oder in einen anderen Namen, der auf Ihrem Computer bisher nicht verwendet wird, damit Entity Framework eine neue, leere Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="ede59-345">To make EF create a new empty database, change the name of the database in the connection string in *appsettings.json* to ContosoUniversity3 or some other name that you haven't used on the computer you're using.</span></span>

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=ContosoUniversity3;Trusted_Connection=True;MultipleActiveResultSets=true"
  },
```

<span data-ttu-id="ede59-346">Speichern Sie Ihre Änderungen an *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="ede59-346">Save your change to *appsettings.json*.</span></span>

> [!NOTE]
> <span data-ttu-id="ede59-347">Alternativ zum Ändern des Datenbanknamens können Sie die Datenbank auch löschen.</span><span class="sxs-lookup"><span data-stu-id="ede59-347">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="ede59-348">Verwenden Sie den **SQL Server-Objekt-Explorer** (SSOX) oder den CLI-Befehl `database drop`:</span><span class="sxs-lookup"><span data-stu-id="ede59-348">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```

<span data-ttu-id="ede59-349">Nachdem Sie den Datenbanknamen geändert oder die Datenbank gelöscht haben, führen Sie den Befehl `database update` im Befehlsfenster aus, um die Migrationen auszuführen.</span><span class="sxs-lookup"><span data-stu-id="ede59-349">After you have changed the database name or deleted the database, run the `database update` command in the command window to execute the migrations.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="ede59-350">Führen Sie die App aus, damit die `DbInitializer.Initialize`-Methode ausgeführt wird und die neue Datenbank auffüllt.</span><span class="sxs-lookup"><span data-stu-id="ede59-350">Run the app to cause the `DbInitializer.Initialize` method to run and populate the new database.</span></span>

<span data-ttu-id="ede59-351">Öffnen Sie die Datenbank wie zuvor im SSOX, und erweitern Sie den Knoten **Tabellen**, um festzustellen, dass alle Tabellen erstellt wurden.</span><span class="sxs-lookup"><span data-stu-id="ede59-351">Open the database in SSOX as you did earlier, and expand the **Tables** node to see that all of the tables have been created.</span></span> <span data-ttu-id="ede59-352">(Wenn Sie SSOX immer noch geöffnet haben, klicken Sie auf die Schaltfläche **Aktualisieren**.)</span><span class="sxs-lookup"><span data-stu-id="ede59-352">(If you still have SSOX open from the earlier time, click the **Refresh** button.)</span></span>

![Tabellen im SSOX](complex-data-model/_static/ssox-tables.png)

<span data-ttu-id="ede59-354">Führen Sie die App aus, um den Initialisierungscode auszulösen, der das Seeding für die Datenbank ausführt.</span><span class="sxs-lookup"><span data-stu-id="ede59-354">Run the app to trigger the initializer code that seeds the database.</span></span>

<span data-ttu-id="ede59-355">Klicken Sie mit der rechten Maustaste auf die Tabelle **CourseAssignment**, und klicken Sie auf **Daten anzeigen**, um zu überprüfen, dass diese Daten enthält.</span><span class="sxs-lookup"><span data-stu-id="ede59-355">Right-click the **CourseAssignment** table and select **View Data** to verify that it has data in it.</span></span>

![CourseAssignment-Daten im SSOX](complex-data-model/_static/ssox-ci-data.png)

## <a name="summary"></a><span data-ttu-id="ede59-357">Zusammenfassung</span><span class="sxs-lookup"><span data-stu-id="ede59-357">Summary</span></span>

<span data-ttu-id="ede59-358">Sie besitzen nun ein komplexeres Datenmodell und eine zugehörige Datenbank.</span><span class="sxs-lookup"><span data-stu-id="ede59-358">You now have a more complex data model and corresponding database.</span></span> <span data-ttu-id="ede59-359">Im folgenden Tutorial erfahren Sie mehr über das Zugreifen auf zugehörige Daten.</span><span class="sxs-lookup"><span data-stu-id="ede59-359">In the following tutorial, you'll learn more about how to access related data.</span></span>
::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="ede59-360">[Zurück](migrations.md)
> [Weiter](read-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="ede59-360">[Previous](migrations.md)
[Next](read-related-data.md)</span></span>
