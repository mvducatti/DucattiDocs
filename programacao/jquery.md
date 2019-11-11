# JQuery

## Datepicker - Data Atual p/ Default

```coffeescript
$(document).ready(function () {
  	$(".date-own").datepicker({
    		format: "yyyy",
    		viewMode: "years",
    		minViewMode: "years",
  	}).datepicker("setDate", 'now');
	});
```

## Popup

O popup as vezes dá problema no javascript de d.apply na hora de dar **$.popup.close\(\)**, quando isso acontecer, adicionar um callback com função vazia para resolver o erro.

```coffeescript
$.popup({
    title: Strings.Attention,
    content: result.Message,
    type: 'danger',
    closeOnEscape: true,
    close: true,
    okFunction: function () {
        $.popup.close();
    },
    callback: function () { }
}); 
```

## Valor de uma Classe

 Algumas vezes para pegar o valor de uma classe temos que usar **attr**, porém ele retorna uma string. Como alternativa, se quiser retornar e checar booleano, pode-se usar o **is**, que retorna o valor se a string.

```coffeescript
$('#combo-period').attr("aria-disabled") //retorna "boolean"
$('#combo-period').is("aria-disabled") //retorna boolean
```

