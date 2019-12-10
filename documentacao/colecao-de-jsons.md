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
	        "IdExternal": "9476B223-4AF7-4C26-C289-702D0005D98F",
            "iva": 1.22,
            "levelDescription": "Descição Level Serviço 1",
            "parcelOrder": 1,
            "serieType": 2,
            "value": 0.00
        }
    ],
    "value": 0.00
}
```

### POST /api/Login

Na aba **body**, utilize a seguinte estrutura:

```javascript
{
    "grantType": "******",
    "userName": "******",
    "password": "******"
}
```

Na aba **Tests**, substitua \(se houver código\) ou adicione \(se não houver\) o pedaço de código abaixo.

```javascript
var jsonData = JSON.parse(responseBody);
pm.environment.set("token", jsonData.token);
```

