# AFS

## HTML

```csharp
//** DIRETIVA **//

//Controlar grid e quantos items fica em cada.
//obs: Usar <dir-paginate> no lugar de ng-repeate
//obs2: Utilizar <dir-pagination> no lugar desejado para deixar 
//o controlar de páginas
dir-paginate="item in facturasToEliminate | itemsPerPage:6"
<dir-pagination-controls max-size="10" boundary-links="true"></dir-pagination-controls>
```

## Permitir somente números no input

Utilizar a tag 'number-input' e o tipo text no input em que deseja que seja somente numérico. Esse number input já identifica o país tambem.

