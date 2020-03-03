# Manipulação de Dados

## HQL

Ao contrário do SQL normal que você utiliza o nome literal das colunas no banco, no HQL os nomes das colunas são os mesmos utilizados e mapeados dentro da classe do C\#.

**obs:** Mesmo se você tiver uma classe de referência dentro da sua classe, deve ser feito um inner join

Exemplo 1

```csharp
string hql = @" SELECT ct
                FROM CollectionTypeCountry ctc
                INNER JOIN ctc.CollectionType ct
                INNER JOIN ctc.Country c
                WHERE c.Code = :CountryCode";

NHibernate.IQuery query = GetDefaultSession().CreateQuery(hql);
query.SetParameter("CountryCode", countryCode);
return query.List<CollectionType>();
```

Exemplo 2

```csharp
string hql = @" SELECT b
                FROM BasicConfiguration as b
                WHERE b.Entity.Id = dbo.f_GetEntityFromBasicConfiguration(:EntityId,:SdaSystemId,:type)
                                    and b.SdaSystem.Id = :SdaSystemId ";
 
var query = GetDefaultSession().CreateQuery(hql);
query.SetGuid("EntityId", entity.Id);
query.SetGuid("SdaSystemId", sdaSystem.Id);
query.SetString("type", type);

return query.UniqueResult<BasicConfiguration>();
```

## **LINQ**

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

### FirstorDefault\(\)

obs: se não encontrar ninguém, o resultado desse método vai ser nulo.

## Clonando Dados

### IClonable

Ao clonar um objeto devemos tomar cuidado para não copiarmos a instância do mesmo, por exemplo.

```csharp
// criando p1 e atribuindo o mesmo em p2
Person p1 = new Person(name: "marcos", age: 21);
Person p2 = p1;
// alteradno idade de p1
person1.age = 47;
// exibindo idade de p2
Console.WriteLine(person2.age);
// output: 47
```

O que aconteceu acima é que a referência de p1 continua existindo dentro de p2, ou seja, qualquer alteração em p1 vai influenciar no resultado de p2 e vice versa.

Para conseguir clonar corretamente, deve seguir o exemplo abaixo com **IClonable**.

```csharp
// criando p1 e atribuindo o mesmo em p2 como clone
Person p1 = new Person(name: "marcos", age: 21);
Person p2 = p1.Clone() as Person;
// alteradno idade de p1
p1.age = 47;
// exibindo idade de p2
Console.WriteLine(p2.age);
// output: 21

public class Person : ICloneable
{
    public Person () {}

    public string name { get; set; }
    public int? age { get; set; }

    public object Clone = () => return this.MemberwiseClone();
}
```

### LINQ Lazy Loading

Ao clonar os dados, se fizermos igual está abaixo, sem utilizar o ToList\(\) na hora de exibir os dados após a propriedade idade da lista original ser setado como nulo, vai ser exibido nulo na outra lista também.

Por que isso acontece?

Basicamente o **Linq** funciona no estilo Lazy Loading, basicamente na **linha 15** eu ainda não recebi os dados, somente especifiquei que os dados vão ser copiados quando eu chamar a **copiedList** novamente, ou seja, não é exatamente na **linha 15** que os dados vão ser atribuídos, vai ser na **linha 28**.

```csharp
// ###################
// ### SEM TOLIST() ##
// ###################

// Instanciando Person
Person person1 = new Person(name: "marcos", age: 25);
Person person2 = new Person(name: "victor", age: 23);

// Adicionando na lista a ser copiada
List<Person> originalList = new List<Person>();
originalList.Add(person1);
originalList.Add(person2);

// Copiando idade da lista original
List<int> copiedList = originalList.Select(x => x.age);

// Removendo propriedade idade da lista original
RemoveAgeFromList(originalList); 

// Function to set property to null
RemoveAgeFromList () {
    foreach (item in originalList) {
        item.age = null;
    }
}

// Printando idade da lista que recebeu os valores da lista original
foreach (item in copiedList) {
    Console.WriteLine(item.age);
    // output: null!!!!
}
```

O **ToList\(\)** basicamente vai funcionar como um **$.apply\(\) do AngularJs**, realizando o recebimento de dados em tempo real. Funcionam de maneira semelhante **FirstOrDefault**, **SingleOrDefault**, **Single**, **First** ou quando pega uma posiçã específica da lista a partir do index. Para resolvermos esse problema, somente precisamos adicionar a seguinte modificação.

```csharp
// Copiando idade da lista original da forma correta
List<int> copiedList = originalList.Select(x => x.age).ToList();
```

