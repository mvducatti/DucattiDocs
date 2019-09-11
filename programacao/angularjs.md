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

