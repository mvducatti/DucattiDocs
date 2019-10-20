# AFS - Paraguay

## LastGenerateNumber

### HTML

```markup
<div class="row addLetterheadRow" ng-show="isViewOnly">
            <div class="col-md-12">
                <label>@Strings.LastGeneratedNumber</label>
                <div class="row">
                    <div class="col-md-12">
                        <label><strong>{{newLetterhead.LastGeneratedNumber}}</strong></label>
                    </div>
                </div>
            </div>
        </div>
```

### AngularJs

Function

```coffeescript
scopeGeneral.GetLastGeneratedNumber = (obj) => {
        return new Promise((resolve, reject) => {
            let params = {
                serieTypeEnum: obj.SerieType,
                entityId: scopeTree.selectedEntity.id
            };    
 
            $.ajax({
                url: "/Parameterization/Serie/GetLastGeneratedNumber",
                type: 'POST',
                data: $.postify(params),
            }).done(function (result) {
                if (result.success) {
                    resolve(result.number)
                } else {
                    showError(result.message);
                }
            });
 
        })
    }
```

viewLetterhead \(function\)

```coffeescript
if (obj.LastGeneratedNumber == undefined) {
    scopeGeneral.GetLastGeneratedNumber(obj).then(number => {
        obj.LastGeneratedNumber = number
        scopeGeneral.fillFields(obj);

        scopeGeneral.$digest()
    });            
}       
else {
    scopeGeneral.fillFields(obj);
}
```

