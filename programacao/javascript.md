# Javascript

## Trabalhando com Guid

[https://www.wikitechy.com/technology/create-guid-uuid-javascript/](https://www.wikitechy.com/technology/create-guid-uuid-javascript/)

## String\(\) e parseInt\(\)

```coffeescript
console.log(String(132))
console.log(parseInt("123"))
> "132"
> 123
```

## Postify\(\)

Dessa forma quando o parâmetro é enviado ele cria a propriedade com o nome do parâmetro para ser recebido no C\#.

```coffeescript
$.postify({ entityIdExternal})
```

## Converter TimeStamp

Essa função é global no projeto do AFS, basta chamar diretamente.

```coffeescript
function convertToDate1(param) {
    if (param.substring(10, 19) === "T00:00:00")
        return new Date(param);
    if (!!param && !angular.isDate(param)) 
        return new Date(parseInt(param.substring(6, 24)));
    return param;
}
```

## Enviando QueryString

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

## Remover Zeros à Esquerda

```coffeescript
teste = "0001234"
teste.replace(/^0+/, '')
//"1234"
```

## Formatar CNPJ

```coffeescript
cpnj.split('.').join("").split('/').join("").split('-').join("");
```

