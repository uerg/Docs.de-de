> [!WARNING]
> Aus Sicherheitsgründen müssen Sie Daten von `GET`-Anforderungen in die Seitenmodelleigenschaften einbinden. Überprüfen Sie die Benutzereingaben, bevor Sie sie den Eigenschaften zuordnen. Die Entscheidung für diese Bindung von `GET` ist von Vorteil, wenn Sie auf Szenarios eingehen, die von Abfragezeichenfolgen oder von Werten für Routen abhängig sind.
>
> Legen Sie die `SupportsGet`-Eigenschaft des [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute)-Attributs auf `true`: `[BindProperty(SupportsGet = true)]` fest, um eine Eigenschaft an `GET`-Anforderungen zu binden.
