# AngularJS

## Documentação de Métodos no AFS

[https://sad-us-fm-1.js.live.repo.sdasystems.org/libjs/docs/docs.md\#](https://sad-us-fm-1.js.live.repo.sdasystems.org/libjs/docs/docs.md#)

[https://sad-us-fm-1.js.live.repo.sdasystems.org/locale/docs/docs.md](https://sad-us-fm-1.js.live.repo.sdasystems.org/locale/docs/docs.md)

## Promessa no AngularJs

```javascript
$scope.getSessionInfo = function () {
 
        return new Promise((resolve, reject) => {
            $.ajax({
                type: "POST",
                url: "/AccountsReceiving/CreditLimit/getSessionInfo",
            }).done(function (result) {
 
                resolve(result);
 
                $scope.selected.LegalEntity = result.SessionLegalEntity;
                $scope.$apply();
                $scope.legalEntities = result.legalEntities;
                $scope.GetCreditLimits();
            });
 
        });
```

## Usando filtro de data quando a data vier no formato /\(xxxxxxxx\)

```javascript
{{obj.Date.replace('/Date(','').replace(')/','') | date:'dd/MM/yyyy'}}
```

## Alternativa para Filtros de dinheiro

```javascript
numero.format('n')
numero.format('c')
​
javascript
mas pode ser usado no html

{{numero.format('c')}}
```

## Popups

Exemplo completo de um Popup e suas propriedades

```coffeescript
$.popup({
    title: Strings.Attention,
    content: Strings.ThisProcedureWillNotGenerateADeliveryFileDoYouWantToRegisterThePayment,
    type: 'warning',
    isModal: true,
    isDragabble: false,
    isResizable: false,
    isCloseable: true,
    closeOnEscape: false,
    minHeight: 200,
    minWidth: 400,
    onClose: function () {
        TotalValueRefresh();
    }
    //type: 'warning',
    okFunction: function () {
        $.popup.close();

        var params = SelectData();
        if (!params)
            return params;

        $.ajax({
            type: "POST",
            url: "/AccountsPayable/Payments/RegisterPaymentBank",
            data: $.postify(params)
        }).done(function (result) {
            if (result) {
                $.notify(Strings.PaymentSuccessfullyRegistered, "success");
                window.location.href = '/AccountsPayable/Payments';
                $.busy(true);
            }
        });
    },
    cancelFunction: function () {
        $.popup.close();
    }
});
}
```

Exemplo de um Popup chamando um modal da tela

```coffeescript
$.popup({
    title: Strings.CloneConfiguration,
    content: $(".popup-new-letterhead"),
    width: "615px"
});
```

## Datime

Para ver outra opção de conversão, ver na página de Javascript.

{% page-ref page="javascript.md" %}

Quando a data vier no estilo  `.replace('/Date(','').replace(')/','')`

```coffeescript
{{obj.FinalEffectiveDate.replace('/Date(','').replace(')/','') | date:'dd/MM/yyyy'}}
```

Quando a data vier no estilo `2019/12/11` ou `12/24/2019`

```coffeescript
{{obj.FinalEffectiveDate | date:'dd/MM/yyyy'}}
```

Quando tiver que converter a data para UTC utilizando o método   
**obs:** Utilizar em último caso, nesse caso a data de timestamp estava vindo com um dia a menos, então teve que buscar diretamente as propriedades.

```coffeescript
let utcInitialDate = convertToDate(element.InitialEffectiveDate);
let utcFinalDate = convertToDate(element.FinalEffectiveDate);
element.InitialEffectiveDate = new Date(
    utcInitialDate.getUTCFullYear(), 
    utcInitialDate.getUTCMonth(), 
    utcInitialDate.getUTCDate()
);
element.FinalEffectiveDate = new Date(
    utcFinalDate.getUTCFullYear(), 
    utcFinalDate.getUTCMonth(), 
    utcFinalDate.getUTCDate()
);
```

