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



