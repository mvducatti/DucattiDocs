# Coleção de Jsons

## ABACOM\_PRY\_DEV

### POST /api/InvoiceRebate

Todos os campos mostrados abaixo são obrigatórios para criação de InvoiceRebate

```javascript
{
    "canceller": false,
    "invoiceId": 9,
    "number": 1,
    "printedDate": "2019-12-03",
    "serieId": 17,
    "services": [
        {
            "description": "Serviço 1",
            "detail": "Detalhe Serviço 1",
	        "IdExternal": "0CB318FC-3EF0-41D0-BF0C-2BF150166FE6",
            "iva": 1.22,
            "levelDescription": "Descição Level Serviço 1",
            "parcelOrder": 1,
            "printedInvoiceId": 3,
            "serieType": 2,
            "value": 300.00
        }
    ],
    "value": 300.00
}
```

### POST /api/Login

Na aba **body**, utilize o código abaixo

```javascript
{
    "grantType": "password",
    "userName": "afs",
    "password": "whi7/4Ndpq1n#"
}
```

Na aba **Tests**, substitua \(se houver código\) ou adicione \(se não houver\) o pedaçõ de código abaixo

```javascript
var jsonData = JSON.parse(responseBody);
pm.environment.set("token", jsonData.token);
```

