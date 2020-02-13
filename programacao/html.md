# CSS Tricks

## Como esconder textos com `...` quando forem muito grandes

[https://stackoverflow.com/questions/25282269/html-large-text-with-dotted-end](https://stackoverflow.com/questions/25282269/html-large-text-with-dotted-end)

```css
#personName {
    white-space: nowrap;
    width: 300px;
    text-overflow: ellipsis;
    overflow: hidden;
}
```

## Jogar todos os items para o lado direito da tela

```css
#pagination-factura-controller {
    float:right;
}
```

Pode-se usar também a propriedade "right" dentro do elemento. obs: ela funciona apenas nas versões antigas do CSS.

## Fazer Dropdown aparecer fora do popup

![](../.gitbook/assets/image%20%2818%29.png)

Na maioria das vezes quando temos um dropdown grande dentro de um popup com tamanho limitado, o mesmo acaba aparecendo dentro do popup e criando uma barra de rolagem na lateral do mesmo. Para consertar esse problema e o dropdown aparecer corretamente igual na imagem acima, deve-se adicionar a propriedade "overflow: visible" dentro do body do popup.

Para adicionar dentro do body, você deve utilizar o código

```css
// CSS
.overflowed {
    overflow: visible !important;
}
```

```d
// JS
function () {
    // Abrindo popup
    $.popup({});
    // Deve ser chamado após o popup ser carregado, senão a classe não vai ser adicionad
    $(".ui-dialog-content").addClass("overflowed");
}
```

