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

## Comparando Dates em String

Quando as datas vierem no formato dd/MM/yyyy e você quiser comparar se uma é maior que a outra, basta verificar criando o método abaixo. Ele criar duas datas a partir das informações da string e compara.

```coffeescript
scopeGeneral.compareDateString = function (inititalDate, finalDate) {
 
 const iniDay = inititalDate.substr(0, 2)
 const iniMonth = inititalDate.substr(3, 2)
 const iniYear = inititalDate.substr(6, 4)

 const fnDay = finalDate.substr(0, 2)
 const fnMonth = finalDate.substr(3, 2)
 const fnYear = finalDate.substr(6, 4)
 
 return new Date(iniYear, iniMonth - 1, iniDay) > new Date(fnYear, fnMonth - 1, fnDay)
}
```

## Foreach \(Nested Loop\)

```coffeescript
angular.forEach($scope.entityNotifyConfigList, function (value) {
  angular.forEach(result, function (value2) {
    if (value.Entity.Id === value2.Entity.Id) {
      value.checked = false;
      value.Id = value2.Id;
      value.UserName = value2.UserName;
    }
  });
});
```

## Loading...

```csharp
$.busy(bool);
```

## Redirecionamento

Esse código faz com que você possa enviar o usuário para o link desejado. **$location.absUrl\(\)** traz a url principal dinamicamente, para usar é necessário importar **$location** e **$window** no controller e na função do mesmo.

```coffeescript
angular.module('app').controller("x", 
['$window', '$location', function ($window, $location) {
    $window.open(`${$location.absUrl()}Parameterization/GeneralConfigurations`);
}]);
```

## Argument 'xyz' is not a function, got undefined

Para consertar o erro abaixo, basta adicionar uma referência do arquivo javascript no HTML em que deseja acessar as funções do arquivo JS.

```coffeescript
Error: [ng:areq] Argument 'FinancialAgreementFaturacionController' is not a function, got undefined
```

![](../.gitbook/assets/image%20%287%29.png)



