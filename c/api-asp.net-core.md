# AbacomPY

## Migration

### Acessando pelo Visual Studio

![No Visual Studio, entre no caminho abaixo](../.gitbook/assets/image%20%282%29.png)

![Nesse caso, selecione o Sda.Abacom.Py.Infrasctructure](../.gitbook/assets/image%20%289%29.png)

### Criando Migration

Cria o script de migração pra adicionar no banco 

```c
add-migration newMigrationDescription
```

Atualizar o banco com base nas migrações 

```c
update-database
```

### Removendo em Caso de Erro

Reverte se você rodou o update-database , tem que ter cuidado quando algum dado já foi inserido com foreign key, senão tem que deletar o relacionamento das FKs diretamente no banco para poder rodar

```c
update-database -migration:nomedoArquivodeMigração
```

Remove a última migração adicionado

```c
remove-migration
```

### Finalizando

Após criar o migration, o mesmo vai aparecer em dois lugares:

![Pasta Migrations](../.gitbook/assets/image%20%284%29.png)

![Tabela de miration no banco de dados](../.gitbook/assets/image%20%288%29.png)

## Mapeamento

### Nullável

As vezes podemos carregar ou não no Specification o objeto completo, mesmo que o mesmo não seja nulável. Um exemplo disso é na linha 19, que é verificado se através do Specification a propriedade veio nula para não dar erro. 

Um erro comum é que se estiver criando um Objeto no Model, por exemplo Services na linha 17 e vier nulo do Specificatione não estiver validado, vai voltar nulo sempre. Para poder descobrir se essa verificação é necessário ou não, tem que ver o contexto de cada Specification e falar com o analista sobre as possibilidades de quando não for necessário trazer alguma objeto ou propriedade e fazer as verificações necessárias de null.

**obs: Fazer a verificação somente com Objetos complexos \(classes\), não é necessário fazer com propriedades normais, ex: decimal, etc...**

### Exemplo de Mapeamento

```csharp
CreateMap<PrintedInvoice, PrintedInvoiceModel>()
    .ForMember(dest => dest.Id, opt => opt.MapFrom(src => src.Id))
    .ForMember(dest => dest.BeneficiaryName, opt => opt.MapFrom(src => src.BeneficiaryName))
    .ForMember(dest => dest.DueDate, opt => opt.MapFrom(src => src.DueDate.ToString("dd/MM/yyyy")))
    .ForMember(dest => dest.EntityDescription, opt => opt.MapFrom(src => src.EntityDescription))
    .ForMember(dest => dest.EntityDocument, opt => opt.MapFrom(src => src.EntityDocument))
    .ForMember(dest => dest.EntityFullAddress, opt => opt.MapFrom(src => src.EntityFullAddress))
    .ForMember(dest => dest.InterestRate, opt => opt.MapFrom(src => src.InterestRate))
    .ForMember(dest => dest.LateFee, opt => opt.MapFrom(src => src.LateFee))
    .ForMember(dest => dest.ParcelAmount, opt => opt.MapFrom(src => src.ParcelAmount))
    .ForMember(dest => dest.ParcelStartMonth, opt => opt.MapFrom(src => src.ParcelStartMonth))
    .ForMember(dest => dest.PrintedDate, opt => opt.MapFrom(src => src.PrintedDate.ToString("dd/MM/yyyy")))
    .ForMember(dest => dest.ResponsibleDocument, opt => opt.MapFrom(src => src.ResponsibleDocument))
    .ForMember(dest => dest.ResponsibleFiscalDocument, opt => opt.MapFrom(src => src.ResponsibleFiscalDocument))
    .ForMember(dest => dest.ResponsibleName, opt => opt.MapFrom(src => src.ResponsibleName))
    .ForMember(dest => dest.InvoiceId, opt => opt.MapFrom(src => src.InvoiceId))
    .ForMember(dest => dest.Services, opt => opt.MapFrom(src => src.Services))
    .ForMember(dest => dest.UseFiscalDocument, opt => opt.MapFrom(src => src.UseFiscalDocument))
    .ForMember(dest => dest.Type, opt => opt.MapFrom(src => src.Invoice == null ? default : src.Invoice.Type))
    .ForAllOtherMembers(opt => opt.Ignore());
```

### Exemplo de Specification

No exemplo abaixo estamos dizendo que queremos que os objetos Services e Invoice venham preenchidos, então a regra acima de verificar se está nulo não necessariamente seria necessária, porém se você tiver criado uma nova consulta que não traga esses objetos ou não traga um deles, deve ser editado o mapeamento acima, pois o mesmo tem que se adaptar aos Scpeficiations criados pelo usuário ou pelas queries criadas pelo mesmo fora do Specifications \(query com LINQ\).  

**obs: O Specification deve ser somente criado se a propriedade for bastante utilizada, senão a mesma pode ser apena incluída quando for realizada uma query normal sem o uso do Specification**

#### Exemplo de Specification

```csharp
namespace Sda.Abacom.Py.Domain.Specifications
{
    public class PrintedInvoiceSpecification : BaseSpecification<PrintedInvoice>
    {
        public PrintedInvoiceSpecification(int id) : base(x=> x.Id == id)
        {
            AddInclude(x => x.Services);
            AddInclude(x => x.Invoice);
        }
    }
}
```

#### Exemplo de Include

```csharp
PrintedInvoice printedInvoice = await repositoryPrintedInvoice
                .Query(include: x => x.Include(x => x.Services))
                .FirstOrDefaultAsync(x => x.InvoiceId == id);
```

utilizando ThenInclude\(\) para preencher

```csharp
PrintedInvoice printedInvoice = await repositoryPrintedInvoice
                .Query(include: x => x.Include(x => x.Services))
                .FirstOrDefaultAsync(x => x.InvoiceId == id);
```

### Exemplo de consulta com LINQ

Igual as consultas realizadas no AFS, porém ao invés de utilizar SQL, HQL e Criteria, é utilizado o LINQ.

```csharp
Entity entity = 
    await repositoryEntity
    .Query()
    .FirstOrDefaultAsync(x=>x.IdExternal == idExternal);
```

### Entities

O mapeamento de entidade representa o banco de dados. Aí você pergunta: "Por que tem classes dentro de uma classe que reprensenta o banco?". A resposta é simples: essas classes são a representação codificada de **foreign keys**. Então para o InvoiceId do banco, existe a tabela Invoice que é a representação do relacionamento da tabela PrintedInvoice e Invoice.

```csharp
namespace Sda.Abacom.Py.Domain.Entities
{
    public class PrintedInvoice
    {
        public int Id { get; set; }
        public string BeneficiaryName { get; set; }
        public DateTime DueDate { get; set; }
        public string EntityDescription { get; set; }
        public string EntityDocument { get; set; }
        public string EntityFullAddress { get; set; }
        public decimal? InterestRate { get; set; }
        public decimal? LateFee { get; set; }
        public int ParcelAmount { get; set; }
        public int ParcelStartMonth { get; set; }
        public DateTime PrintedDate { get; set; }
        public string ResponsibleDocument { get; set; }
        public string ResponsibleFiscalDocument { get; set; }
        public string ResponsibleName { get; set; }
        public Invoice Invoice { get; set; }
        public int InvoiceId { get; set; }
        public ICollection<Service> Services { get; set; }
        public bool UseFiscalDocument { get; set; }
    }
}
```

#### **Observações**

**TL;DR:** Se eu tenho y dentro de x **\(x =&gt; y\)** e y é uma inserção nova, dentro buscar y a partir do Id fornecido e preencher o model a ser salvo, senão o Entity vai entender que o y também vai ser um objeto novo e vai dar erro porque o Id já existe. Se eu tenho z dentro de y dentro de x **\(x =&gt; y =&gt; z\)** e y também é uma inserção nova, porém o z já deve existir, devo carregar o objeto z para não dar o erro comentado anteriormente. Se nenhum deles já deve existir previamente, não há necessidade de buscar no banco porque não vai encontrar niguém 🤠

Ao tentar salvar, você tem que carregar os objeto dentro dessa classe. No exemplo acima deverá ser carregado o Invoice. Isso porque, como explicado anteriormente, mostra o relacionamento de tabelas e se o objeto não for carregado, o Entity vai entender que está sendo criado um novo objeto \(isso quando por exemplo, para criar um PrintedInvoice já deve existir um Invoice, por isso foi carregado\). Isso funciona sucessivamente para classes e sub-classes.

ex: Se um InvoiceRebate tiver que ser criado, a classe Serie dentro dele tem que ser buscado. Agora se a classe Serie também for nova \(por regra de negócio\), deve-se buscar a classe Entity dentro da mesma e assim sucessivamente.

Deve-se também, se fizer mudanças em algum objeto, salvar as mudanças antes de salvar o objeto principal, senão o banco vai entender que está sendo realizado um update.

```csharp
//Buscando
input.Invoice = await repositoryInvoice.FindAsync(input.InvoiceId);
    //Validadando e alterando a propriedade
    if (input.Canceller)
        input.Invoice.Status = InvoiceStatusEnum.Cancelled;
    //Salvando a alteração
    repositoryInvoice.SaveChanges();
    //Salvando o objeto principal
    this.repositoryInvoiceRebate.Add(input);
    await this.repositoryInvoiceRebate.SaveChangesAsync();
```

### Models

Models são a representação do que vai ser apresentado ao cliente.

```csharp
namespace Sda.Abacom.Py.Domain.Models
{
    public class PrintedInvoiceModel
    {
        public int Id { get; set; }
        public string BeneficiaryName { get; set; }
        public string DueDate { get; set; }
        public string EntityDescription { get; set; }
        public string EntityDocument { get; set; }
        public string EntityFullAddress { get; set; }
        public decimal? InterestRate { get; set; }
        public int InvoiceId { get; set; }
        public string InvoiceNumber { get; set; }
        public decimal? LateFee { get; set; }
        public int ParcelAmount { get; set; }
        public int ParcelStartMonth { get; set; }
        public string PrintedDate { get; set; }
        public string ResponsibleDocument { get; set; }
        public string ResponsibleFiscalDocument { get; set; }
        public string ResponsibleName { get; set; }
        public bool UseFiscalDocument { get; set; }
        public InvoiceTypeEnum Type { get; set; }
        public ICollection<ServiceModel> Services { get; set; }
    }
}
```

