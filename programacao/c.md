# C\#

## Override

Para poder dar override em métodos no C\#, o método ou objeto deve estar setado como

```csharp
virtual
```

## Polimorfismo

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

## Model

Se necessário passar um Model, aonde você sabe que será somente utilizado o Id dele na frente, instanciar um objeto do Model passando o Id que você já tem como propriedade. Isso irá gerar um Model com apenas o Id.

```csharp
Guid entityId = '2C6CA733-7D0F-416C-8D11-2E8C5F31E0CF'
function foo(new Entity() { Id = entityId })
```

## Acessando Itens da Sessão

É possível acessar propriedades da sessão atual e resgatar o seu valor quando necessário, evitando assim códigos maiores para a mesma busca.

```csharp
SessionBusiness.Current.SdaSystem
```

Salvando Corretamente utilizando Business e Rollback utilizando como exemplo UserEntityPreventNotificationBusiness.cs e RequestController.cs

## **Controller**

Exemplo de Controller passando informações para a **`View`**

```csharp
public JsonResult SaveNotificationConfiguration(IList<UserEntityPreventNotification> notifyConfigSave, IList<UserEntityPreventNotification> notifyConfigDelete)
    {
      UserEntityPreventNotificationBusiness business = new UserEntityPreventNotificationBusiness();
      try
      {
        business.SaveConfiguration(notifyConfigSave, notifyConfigDelete);
        return Json(new { Completed = true, Message = "Alterações salvadas com sucesso" });
      }
      catch (Exception e)
      {
        return Json(new { Completed = false, e.Message });
      }
    }
```

## **Business**

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

## **Objetos em sessão**

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

## GROUP BY

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

## Conversando entre dois projetos

Quando dois projetos do AFS tentam se conversar e um dele dá um throw e for chamado em outro método, pode-se utilizar o **`try catch`** em volta daquele método para poder tratar a exceção da forma como desejar, ao invés de mostrar diretamente para o cliente

```csharp
try {
	foo(); //foo de outro projeto interno do AFS (ex: método do Business sendo chamado no Controller), aonde o tratamento de erros pode dar algum conflito
} catch (exception e) {
	return e.Message
}
```

## Relatório

Exibir ou esconder campos baseado no valor

```csharp
=iif(IsNothing(Fields!YourField.Value),True,False)
```

## Reflection

Quando houver uma Reflection no código, tentar achar o cenário correto de quando ela vai chamar o método que você precisa e apertar **`F11`**.

```csharp
EnhancedField field = (EnhancedField)method.Invoke(this, parameter); //Aperte F11 aqui para ser redirecionado magicamente para o método que você quer (se estiver dentro do cenário correto)
```

## Switch 

Exemplo onde vários cases podem se comportar apenas como um.

```csharp
switch (billetFinancialAgreement.FinancialAccount.Company.Code)
      {
        case "033":  //Santander
          bank = new BancoSantander();
          break;
        case "237":  //Bradesco
          bank = new BancoBradesco();
          break;
 
        case "001":  //Banco do Brasil
        case "399":  //HSBC
        case "085":  //Cooperativa Central de Crédito Urbano
        case "341":  //Banco Itaú SA
        case "021":  //Banco do Estado do Espírito Santo
        case "070":  //Branco de Brasília
        case "756":  //Sicoob
        case "104":  //Caixa
        case "041":  //Banrisul
        case "748":  //Banco Sicredi
        case "077":  //Banco Inter S.A.
        default:
          throw new Exception($"{Strings.FunctionalityNotImplementedForTheBank} {billetFinancialAgreement.FinancialAccount.Company.Name}"); //Traduzir
      }
```

## Comparação de Guid, quando o mesmo vem como String e o objeto esperado é um Guid

```csharp
item.ContractId == new Guid("834712f0-794c-4e22-bfab-0a7bbaba5e44")
```

## Alterar String vindo do Resources

```csharp
using System;
					
public class Program
{
	public static void Main()
	{
		var STRINGS_DIGITE_DOC = "Digite um {0} válido";
		// ------
		var documentTypeMainDescription = "CNPJ";
		Console.WriteLine(string.Format(STRINGS_DIGITE_DOC, documentTypeMainDescription));
	}
}
```

## Instanciação de objeto com valores

```csharp
Country = new Country { Code = "BR", Id = 1 }
```

## Referência \(ref\)

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

## Formatando Máscaras Dinamicamente

Dessa forma, você passa o valor que deseja que seja formatado e a máscara em forma de string. 

```csharp
string cpf = "368924373";
string mask = "00.000.000-0";

cpf.FormatWithMask(cpf , mask )
//Output: 36.892.437-3

public static string FormatWithMask(this string input, string mask)
        {
            if (string.IsNullOrWhiteSpace(mask))
                return input;
 
            var output = string.Empty;
            var index = 0;
 
            foreach (var m in mask)
            {
                if (m == '0')
                {
                    if (index < input.Length)
                    {
                        output += input[index];
                        index++;
                    }
                }
                else
                    output += m;
            }
 
            return output;
        }
```

