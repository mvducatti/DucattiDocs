---
description: Trabalhando com filtros
---

# Filtros JS, LINQ, Lodash

## **LINQ C\#**

### Trabalhando com 2 listas

Retornando valores iguais

```csharp
var foo = list1.Where(x => list2.Select(y => y.id).Contains(x.id));
```

Retornando valores diferentes

```csharp
var foo = list1.Where(x => !list2.Select(y => y.id).Contains(x.id));
```

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

