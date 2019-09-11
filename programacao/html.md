# HTML

Você pode passar parâmetros pela URL, que vai chegar no Index do C\#

Você deve separar os parâmetros com **`?`**

## Javascript

```coffeescript
function SaveNfse(apdId) {
  $.popup({
    title: Strings.Attention,
    content: "Salvo com sucesso. Deseja visualizar?",
    okFunction: function () {
      window.open(`/AccountsPayable/Scheduling?accountsPayableDocumentId=${apdId}?tab=${2}`, '_blank');
      $.popup.close();
    },
    cancelFunction: function () {
      $.popup.close();
    }
  });
}
```

## C\#

```csharp
[AacAuthorize]
    public ActionResult Index(Guid? accountsPayableDocumentId, int? tab = null)
    {
      IAccountsPayableDocumentBusiness accountsPayableDocumentBusiness = BusinessFactory.Instance.GetAccountsPayableDocumentBusiness();
      IAccountsPayableDocumentTypeBusiness accountPayableDocumentTypeBusiness = BusinessFactory.Instance.GetAccountsPayableDocumentTypeBusiness();
      ICompanyBusiness companyBusiness = BusinessFactory.Instance.GetCompanyBusiness();

      if (accountsPayableDocumentId != null && accountsPayableDocumentId != Guid.Empty)
      {

        var apd = accountsPayableDocumentBusiness.Get(accountsPayableDocumentId);
        if (apd != null)
          ViewBag.AccountsPayableDocumentId = accountsPayableDocumentId;
        else
          ViewBag.AccountsPayableDocumentId = null;
      }
      else
      {
        ViewBag.AccountsPayableDocumentId = null;
      }
}
```

## Como esconder textos com `...` quando forem muito grandes

[https://stackoverflow.com/questions/25282269/html-large-text-with-dotted-end](https://stackoverflow.com/questions/25282269/html-large-text-with-dotted-end)

```css
#personName {
    white-space: nowrap;
    width: 300px;
    text-overflow: ellipsis;
    overflow: hidden;
}
```

