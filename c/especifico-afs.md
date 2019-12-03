# Específico AFS

## Acessando Itens da Sessão

É possível acessar propriedades da sessão atual e resgatar o seu valor quando necessário, evitando assim códigos maiores para a mesma busca.

```csharp
SessionBusiness.Current.SdaSystem
```

Salvando Corretamente utilizando Business e Rollback utilizando como exemplo UserEntityPreventNotificationBusiness.cs e RequestController.cs

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

## **Exemplo de Controller**

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

## Tradução de Propriedade \(AFS\)

Alterando a propriedade de um objeto quando receber um valor. Esse códiog traduz quando a classe for receber valores de alguma chamada que irá popular a classe, no caso em específico estamos realizando a tradução do mesmo através de um métood criado no próprio AFS de traduzir strings, porém o exemplo serve como modelo para quando for necessário realizar algo que seja parecido, como realizar alguma conta antes de atribuir o valor da propriedade, por exemplo.

```csharp
namespace Sda.Afs.Entities
{
    public partial class CollectionType
    {
        private string _Description;

        [DataMember]
        public virtual string Description
        {
            get
            {
                return this._Description.TranslateString();
            }
            set
            {
                _Description = value;
            }
        }
    }
}
```

## DataFactory

Alternativa para acessar o Data dentro do próprio data sem necessidade de  instanciação

```csharp
DataFactory.Instance.GetAccountingItemData.GetAll();
```

## Tutorial - Erro no StringDesigner

![Fazer Unload do projeto Sda.Afs.Resources](https://s3.amazonaws.com/notejoy/note_images/348040.1.Image%202019-06-10%20at%2014.39.13.png)

![Clicar em Editar](https://s3.amazonaws.com/notejoy/note_images/348040.1.Image%202019-06-10%20at%2014.40.06.png)

![Apagar o Strings1](https://s3.amazonaws.com/notejoy/note_images/348040.1.Image%202019-06-10%20at%2014.40.42.png)

![Clicar em Reload Project](https://s3.amazonaws.com/notejoy/note_images/348040.1.Image%202019-06-10%20at%2014.41.38.png)

## Mapeamento

### Modificando a Propriedade

Sempre quando criar um mapeamento, ir nas propriedades e setar o **`Build Action`** como **`Embedded Resource`**

![](../.gitbook/assets/image%20%289%29.png)

### String x AnsiString

**`String`** é utilizado no mapeamento quando o campo do banco é **`CHAR`**.  
**`AnsiString`** será utilizado no mapeamento quando o campo do banco for **`VARCHAR`**  
  
Exemplo:

```sql
//Banco
Name || varchar
Code || char

//Arquivo .hbm.xml
<property name="Name"              column="Name"              type="AnsiString" />
<property name="Code"              column="Code"              type="String" />
```

## Criação de Telas

### Observações Iniciais

* Usar **`$.busy(true);`** no início da chamada e **`$.busy(false);`** no final da chamada.
* usar **`http`** em vez de **`Ajax`** nas chamadas ao banco.
* A tela já está utilizando o bootstrap automaticamente.

### **Passos Iniciais**

1. Gerar **`Controller, View`** e arquivo **`Javascript`.**
2. Criar tela e caminho no **`BaseController.cs`.**
3. Gerar script de acesso na pasta de scripts.
4. Importar do **`BaseController`** para exibir na tela em vez de Controller.
5. Adicionar **`[AacAuthorize]`** acima do public **`ActionResult Index()`** da tela.

