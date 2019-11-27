# Conceitos Diversos

## Model

Se necessário passar um Model, aonde você sabe que será somente utilizado o Id dele na frente, instanciar um objeto do Model passando o Id que você já tem como propriedade. Isso irá gerar um Model com apenas o Id.

```csharp
Guid entityId = '2C6CA733-7D0F-416C-8D11-2E8C5F31E0CF'
function foo(new Entity() { Id = entityId })
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

## Instanciação c/ Valores

```csharp
Country = new Country { Code = "BR", Id = 1 }
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

## Comparando Enums / Dispose

Comparando Enums a partir dos seus índices. Dispose é quando você precisa instanciar uma classe, porém não quero utilizá-la, quer apenas rodar uma método. O `_ =` ajuda a economizar o espaço que uma variável seria criada.

```csharp
_ = DocumentFactory.ResetFactory(Enum.GetName(typeof(CountryCodeEnum), (int)countryEnum));
```

## IsNullOrWhiteSpace

Trabalhar com IsNullOrWhiteSpace é melhor porque além de verificar se é null ou WhiteSpace adiciona o método Trim\(\).

```csharp
string empty= " ";
String.IsNullOrWhiteSpace(empty); //true

string empty= "Lorem Ipsum";
String.IsNullOrWhiteSpace(empty); //false
```

## Filtrar números de uma string

```csharp
using System;
using System.Linq; //Importar para utilizar o .Where();

string cpf = "337   98dsasaddas96489!@¨#%¨*@#%¨*@%¨!#%!&*@¨#¨&!@0";
string teste = new string(cpf.Where(c => char.IsDigit(c)).ToArray());
Console.WriteLine(teste); //"33798964890"
```

## ToString\(""\)

Você pode alterar dentro do método ToString\(\), como visto no exemplo abaixo.

```csharp
birthdate.toString("dd-MM-yyyy")
```

## Casts

> Because C\# is statically-typed at compile time, after a variable is declared, it cannot be declared again or assigned a value of another type unless that type is implicitly convertible to the variable's type. For example, the `string` cannot be implicitly converted to `int`. Therefore, after you declare `i` as an `int`, you cannot assign the string "Hello" to it, as the following code shows
>
> [https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/types/casting-and-type-conversions](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/types/casting-and-type-conversions)

```csharp
# Exemplos comuns de cast
//Primeira forma
(int)v (string)w (double)x (decimal)y (bool)z
//Segunda forma
v as int w as string x as double y as decimal z as bool
```

```csharp
# Exemplos avançados de cast

## Neste exemplo existe uma função que recebe uma data nulável
## Dependendo dos métodos utilizados dentro do DateTime, pdoe gerar erro de 
## cast para o parâmetro passado

## Abaixo será mostrar uma forma errada e a forma correta de realizar a conversão

//Função principal
GetSerieListByFilter(DateTime? myDate = null)

//Varíavel de tempo
DateTime y = DateTime().Now();

//Chamando a função e passando a variável

## Forma Errada ❌
foo.GetSerieListByFilter(
    myDate: y.HasValue ? y.Value.ToUniversalTime() : null
);

## Forma Correta ✔
foo.GetSerieListByFilter(
    myDate: y.HasValue ? (DateTime?)y.Value.ToUniversalTime() : null
);

## Vai dar erro, porque ToUniversalTime() devolve um DateTime e 
## esperado é um 'DateTime?', nesse caso podemos converter
## 'y.Value.ToUniversalTime()' DateTime? com o cast (DateTime?) 

##Conclusão
//Podemos converter classes e tipos além dos tradicionais string, int, ...
```

