# Manipulação de Dados

## LODASH \_.

### Filtrando com objetos específicos dentro de uma lista

```javascript
// using the `_.matches` callback shorthand
_.filter(result, { 'Service': { 'Kind': 'C' } })

// using the `_.matchesProperty` callback shorthand
_.filter(result, 'Service.Kind', 'C')

// using the `_.property` callback shorthand
_.filter(result, function(item){
  return item.Service.Kind == 'C';
})
```

### Filtando Sublistas

```javascript
//Exemplo de 3 listas, uma dentro da outra
mainList = [
 {ControlNegotiationLite [
  {Billet : 'ExternalNumber' = 0}
 ]},
 {ControlNegotiationLite [
  {Billet : 'ExternalNumber' = 0},
  {Billet : 'ExternalNumber'= 2 }
 ]}
]
 
//Mapeando ControlNegotiationLite
_.chain(mainList)
   .map("ControlNegotiationLite")
   .flatten()
   .value()
//outpout: (2) ControlNegotiationLite
 
//Mapeando Billet dentro de ControlNegotiationLite
_.chain(mainList)
   .map("ControlNegotiationLite")
   .flatten()
   .map("Billets")
   .flatten()
   .value()
//outpout: (3) Billets

//Filtrando Billet dentro de ControlNegotiationLite utilizando 
//o mapeamento anterior
_.filter(_.chain(mainList)
   .map("ControlNegotiationLite")
   .flatten()
   .map("Billets")
   .flatten()
   .value(), function(o) { 
   return o.ExternalNumber != 0; 
});
//outpout: (1) Billet
```

## Some - Booleano

```coffeescript
_.some(scopeGeneral.letterheadList, { 'Active': true, 'SerieType': obj.SerieType })
```

## JAVASCRIPT

### Comparar duas listas e retornar booleano se são iguais ou não

Array é uma class no javascript, aonde possui propriedades e métodos. 'equals' é um método dessa classe.

```javascript
x.equals(y) //true or false
```

Comparar duas listas e retornar os valores iguais

```javascript
arr2.filter(e=> arr1.includes(e))
```

Comprar duas listas e retornar os valores diferentes

```javascript
arr2.filter(e=> !arr1.includes(e))
```

### Alterar propriedade específica de um objeto dentro de um array

![](https://s3.amazonaws.com/notejoy/note_images/248051.1.MicrosoftTeams-image%20%285%29.png)

