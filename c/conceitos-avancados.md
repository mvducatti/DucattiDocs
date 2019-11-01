# Conceitos Avançados

### Override

Para poder dar override em métodos no C\#, o método ou objeto deve estar setado como

```csharp
virtual
```

### Polimorfismo

[https://pt.wikipedia.org/wiki/Polimorfismo\_\(inform%C3%A1tica\)](https://pt.wikipedia.org/wiki/Polimorfismo_%28inform%C3%A1tica%29)

Inserindo objetos em um model a partir de um object genérico

```csharp
var result = query.List<object>();
IList<ServiceExternal> dataResult = new List<ServiceExternal>();

foreach (object[] item in result){
	ServiceExternal obj  = new ServiceExternal
		{
			ContractId = (Guid)item[0],
			ServiceId = (Guid)item[1],
			Kind = (string)item[2],
			AccountsReceivableTypeId = (Guid)item[3],
			IsPercent = (bool)item[4],
			Description = (string)item[5],
			Code = (string)item[6]
		};
		dataResult.Add(obj);
}
```

### Referência \(ref\)

Com a referência você pode enviar o objeto ques está sendo usado atualmente e alterar os valores do mesmo. Num método normal, se você fizesse isso ele não manteria as alterações assim que acabasse o método, porém como estamos passando a referência do mesmo objeto, as alterações serão significativas no mesmo.   
  
No caso abaixo está sendo verificado se o objeto e as propriedades do mesmo são nulo, se as propriedades forem nulas, então seta o objeto inteiro para nulo e continua normalmente. A diferença é que como foi passado como referência, o objeto que foi passado como parâmetro vai ficar nulo devido à alteração dentro do método. Deve-se usar **`ref`** com muita responsabilidade.

```csharp
//Chamando o método passando o objeto como diferente de null
ValidatePersonAddress(ref personAddress)

public bool ValidatePersonAddress(ref PersonAddress personAddress)
{
    if (personAddress != null)
    {
        if (personAddress.ZipCode == null && personAddress.PersonAddressType == null && personAddress.Street1 == null && personAddress.Street2 == null &&
            personAddress.Number == null && personAddress.District == null && personAddress.Locality == null)
        {
            personAddress = null;
            return false;
        }
        else
        {
            return true;
        };
    }
    else
    {
        return true;
    }
}
```

### Sessão em Classes

O C\# Trabalha com sessão, mesmo em objetos. Se eu tenho um **`Billet originalBillet`** e um **`Billet billet`** e alterar a propriedade de um deles, o outro vai ser afetado, diferentemente do Javascript.

Para resolver esse problema quando às vezes precisamos somente de uma cópia para realizar consultas, enfim, deve-se utilizar o Copy. Para uma classe utilizar o copy, deve-se implementar a interface IClonable do C\#.

Para realizar uma cópia, basta inserir o código a seguir no objeto instanciado da mesma classe

```csharp
Billet originalBillet;
originalBillet = billet.Clone() as Billet;
```

![](https://s3.amazonaws.com/notejoy/note_images/248985.1.Image%202019-05-09%20at%2009.22.54.png)

![](https://s3.amazonaws.com/notejoy/note_images/248985.1.Image%202019-05-09%20at%2009.22.15.png)

![](https://s3.amazonaws.com/notejoy/note_images/248985.1.Image%202019-05-09%20at%2009.22.39.png)

### **Begin Transaction e Rollback Transaction**

obs: Se o **`dataAccess.BeginTransaction();`** e o **`dataAccess.RollbackTransaction();`** estiverem dentro do **`foreach`**, eles ignoram apenas uma pessoa salva e não retornam erro. No exemplo abaixo está envolvendo todo o loop, ou seja, se der erro em um, vai parar a transação de todos.

```csharp
 public void SaveConfiguration(IList<UserEntityPreventNotification> notifyConfigSave, IList<UserEntityPreventNotification> notifyConfigDelete)
    {
      try
      {
        dataAccess.BeginTransaction();

        if (notifyConfigDelete.Count > 0)
        {
          foreach (var item in notifyConfigDelete)
          {
            item.UserName = SessionBusiness.Current.UserInfo.UserName;
            dataAccess.Delete(item);
          }
        }

        if (notifyConfigSave.Count > 0)
        {
          foreach (var item in notifyConfigSave)
          {
            item.UserName = SessionBusiness.Current.UserInfo.UserName;
            dataAccess.Save(item);
          }
        }

        dataAccess.CommitTransaction();
      }
      catch (Exception)
      {
        dataAccess.RollbackTransaction();
        throw;
      }
    }
```

### Propriedades null/not null\(?\)

 Se o .Locality no exemplo abaixo não for encontrado, vai gerar o seguinte erro: **`Object reference not set to an instance of an object.`**

Com ponto de **`?`** evitamos esse erro, pois trabalha tanto com o valor **`null`** quanto com o valor que não for **`null`**.

```csharp
Locality locality = BusinessFactory.Instance.GetEntityAddressBusiness()
    .GetByEntityId(entity.Id)?.Locality;
```

### Dictionary c/ Group By

Exemplo de como podemos trabalhar o Group By com o Dictionary, pois dessa forma é mais fácil de trabalhar com o retorno do mesmo.

```csharp
IAccountsPayableDocumentBusiness accounts = BusinessFactory.Instance.GetAccountsPayableDocumentBusiness();
           EventBusiness eventBusiness = new EventBusiness();
 
           Event evt = eventBusiness.Get(eventId);
           List<TaxCollectionDocument> tcdList = evt.EventItemList.Select(p => p.TaxCollectionDocument).ToList();
           this.ValidateTaxCollectionList(tcdList);
 
           var EventDocuments = tcdList.GroupBy(x => x.AccountsPayableDocument.MainDocument)
               .ToDictionary(gpr => gpr.Key, gpr => gpr.ToList())
               .Select(x => new
               {
                   MainDoc = new
                   {
                       x.Key.Id,
                       x.Key.Number,
                       Date = String.Format("{0:dd/MM/yyyy}", x.Key.Date),
                       AuthorizationDate = String.Format("{0:dd/MM/yyyy}", x.Key.AuthorizationDate),
                       x.Key.Vendor.FullName,
                       x.Key.Value,
                       x.Key.SerialNumber,
                       x.Key.LegalEntity.Name,
                       x.Key.AccountsPayableDocumentType.Abbreviation,
                       Docs = x.Value.Select(f => new
                       {
                           AccountsPayableDocumentId = f.AccountsPayableDocument.Id,
                           AccountsPayableDocumentLegalEntity = f.AccountsPayableDocument.LegalEntity,
                           f.AccountsPayableDocument.Value,
                           Type = f.AccountsPayableDocument.AccountsPayableDocumentType.Abbreviation,
                           TaxCollectionType = f.TaxCollectionType.Tax.Description,
                           Date = String.Format("{0:dd/MM/yyyy}", f.AccountsPayableDocument.Date),
                           AutorizationDate = String.Format("{0:dd/MM/yyyy}", f.AccountsPayableDocument.AuthorizationDate),
                           Vendor = f.AccountsPayableDocument.Vendor.FullName,
                           IsActive = f.IsActive ? "Ativo" : "Inativo"
                       }).ToList()
                   }
               }).ToList();
 
           return Json(EventDocuments, JsonRequestBehavior.AllowGet);
}
```

### Recursividade

Exemplo de método sendo trabalhado com recursividade para verificar em qual dos "parents" se encontra a informação requerida.

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
                    
public class Program
{
    public static void Main()
    {
        var myEntity = new Entity
        {
            Id = 3,
            Name = "Unasp Virtual",
            Own = false,
            Parent = new Entity
            {
                Id = 2,
                Name = "UNASP",
                Own = false,
                Parent = new Entity
                {
                    Id = 1,
                    Name = "UCB",
                    Own = true,
                    //Descomentar o código abaixo para fazer com que
                    //o método encontre a informação
                    //PaymentType = new List<string>(){ "Dinheiro", "Cheque" }
                }
            }
        };
        
        var paymentTypeList = GetPaymentType(myEntity);
        
        if(paymentTypeList != null && paymentTypeList.Any())
        {
            Console.WriteLine("Exibir na tela opções");
        }
        else
        {
            Console.WriteLine("Não foi configurado");
        }
        
    }
    
    public static List<string> GetPaymentType(Entity entity)
    {
        if(entity.Own)
        {
            Console.WriteLine(entity.Name);
            return entity.PaymentType;
        }   
        else
        {
            if(entity.Parent != null)
                return GetPaymentType(entity.Parent);
            else
                return null;
        }
    }
    
    
    public class Entity
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public Entity Parent { get; set; }
        public bool Own { get; set; }
        public List<string> PaymentType { get; set; }
    }
}
```

### Enviando Parâmetros Alternativa

Dessa forma \(igual no Dart\), podemos declarar o nome da propriedade que é um parâmetro para visualizar melhor para onde estamos enviando os dados. Muito útil quando temos apenas `fun(true)`, no caso poderia ser alterado para `fun(isAtive: true)`

```csharp
//Chamando o método
IList<SerieModel> serieList = client.GetById(entityIdExternal: entityIdExternal);

//Método
public IList<SerieModel> GetById (Guid entityIdExternal) {
...
}
```

