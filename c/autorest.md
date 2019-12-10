# AutoRest

## Basics

O AutoRest mapeia para o C\# através de um JSON.

Deve-se alterar o yaml \(linha 13\), pois é a referência do arquivo JSON

No csharp \(linha 16\) deve-se alterar o namespace, que é pra onde ele será mapeado

```csharp
# Geração automática do cliente Abacom
> see https://aka.ms/autorest

## Fazendo funcionar
Para atualizar o cliente, simplesmente instale o AutoRest via `npm` (`npm install -g autorest`) e depois execute (no mesmo caminho deste arquivo):
> `autorest readme.md`

---

## Configuração
Essas são as configurações que serão usadas pelo AutoRest:

``` yaml 
input-file: http://localhost:1321/swagger/v1/swagger.json

csharp:
  namespace: Sda.Afs.Abacom.Py
  override-client-name: AbacomPyClient
  output-folder: ./
  
```

---

## Problemas Conhecidos
- Enums não são gerados corretamente. São gerados como int. Essa é uma limitação da especificação OpenAPI e não há muito o que possamos fazer. [Fonte](https://github.com/domaindrivendev/Swashbuckle/issues/1113)

## Outros recursos relevantes
  - http://azure.github.io/autorest/
  - https://github.com/Azure/autorest
  - https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/ClientRuntime/Microsoft.Rest.ClientRuntime
```

Rodar o comando 'autorest' na pasta em que se encontra o arquivo.md no projeto desejado.

![](../.gitbook/assets/image%20%281%29.png)

## Erros \[Resolução\]

_**Error: File '**_[_**http://localhost:1321/swagger/v1/swagger.json**_](http://localhost:1321/swagger/v1/swagger.json)_**' is not a valid OpenAPI 2.0 definition \(expected 'swagger: 2.0'\)**_

#### Solução

Criar um projeto novo de aspNetCore com as classes e métodos para gerar o AutoRest corretamente.

