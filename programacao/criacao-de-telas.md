---
description: Criando telas no AFS
---

# Criação de Telas

## Observações Iniciais

* Usar **`$.busy(true);`** no início da chamada e **`$.busy(false);`** no final da chamada.
* usar **`http`** em vez de **`Ajax`** nas chamadas ao banco.
* A tela já está utilizando o bootstrap automaticamente.

## **Passos Iniciais**

1. Gerar **`Controller, View`** e arquivo **`Javascript`.**
2. Criar tela e caminho no **`BaseController.cs`.**
3. Gerar script de acesso na pasta de scripts.
4. Importar do **`BaseController`** para exibir na tela em vez de Controller.
5. Adicionar **`[AacAuthorize]`** acima do public **`ActionResult Index()`** da tela.

## **Telas e Exemplos Com Imagens**

![](https://s3.amazonaws.com/notejoy/note_images/235721.3.2019-01-31%2011_27_02-.png)

![](https://s3.amazonaws.com/notejoy/note_images/235721.2.2019-01-31%2011_28_31-Microsoft%20Teams.png)

![](https://s3.amazonaws.com/notejoy/note_images/235721.2.2019-01-31%2011_24_54-.png)

![](https://s3.amazonaws.com/notejoy/note_images/235721.2.2019-01-31%2011_21_18-.png)

