# Consultas de Dados

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

