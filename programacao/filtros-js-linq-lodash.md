# Filtros JS, LINQ, Lodash

## **LINQ C\#**

### Comparando Listas de Tipos Diferentes

Retornando valores iguais

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
                    
public class Program
{
    public static void Main()
    {
		var list1 = new List<PaymentTypeClass>() { new PaymentTypeClass(1, "Dinheiro"), new PaymentTypeClass(2, "Cheque")};
		var list2 = new List<PaymentTypeClass>() { new PaymentTypeClass(1, "Dinheiro")};
		var listFinal = list1.Where(x => list2.Select(y => y.Id).Contains(x.Id));
		
		foreach (var item in listFinal) {
			Console.WriteLine(item.Name);
		}
		
	}
	
	public class PaymentTypeClass {
		public int Id { get; set; }
		public string Name { get; set; }
		
		public PaymentTypeClass(int id, string name){
			this.Id = id;
			this.Name = name;
		}
	}
}
```

Retornando valores diferentes

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
                    
public class Program
{
    public static void Main()
    {
		var list1 = new List<PaymentTypeClass>() { new PaymentTypeClass(1, "Dinheiro"), new PaymentTypeClass(2, "Cheque")};
		var list2 = new List<PaymentTypeClass>() { new PaymentTypeClass(1, "Dinheiro")};
		var listFinal = list1.Where(x => !list2.Select(y => y.Id).Contains(x.Id));
		
		foreach (var item in listFinal) {
			Console.WriteLine(item.Name);
		}
		
	}
	
	public class PaymentTypeClass {
		public int Id { get; set; }
		public string Name { get; set; }
		
		public PaymentTypeClass(int id, string name){
			this.Id = id;
			this.Name = name;
		}
	}
}
```

### Comparando Listas de Tipos Iguais

Retornando valores diferentes

```csharp
var resultado = lista1.Except(lista2).ToList();
```

Retornando valores iguais

```csharp
var resultado = lista1.Contains(lista2).ToList();
```

### SelectMany

Se você tiver uma lista de Documentos dentro da classe pessoa ex:

```csharp
public class Person
{
    IList<Document> Documents { get; set; }
}
```

e você quer somente os documentos você deve utilizar .SelectMany\(\) ao invés de .Select\(\)

```csharp
basicConfigType.GetAll().Select(x => x.PaymentTypes).ToList();
// Retorna IEnumerable<List<Document>> - ERRADO
basicConfigType.GetAll().SelectMany(x => x.PaymentTypes).ToList();
// Retorna List<PaymentType> - CORRETO
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

