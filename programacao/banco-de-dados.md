# Banco de Dados

## Infinite Scroll

Para realizar um **`INFINITE SCROLL`** podemos utilizar a seguinte função:

```sql
SELECT *
FROM DocumentType
ORDER BY DocumentTypeId
OFFSET (4) ROWS FETCH NEXT (2) ROWS ONLY
```

Colocamos em **`OFFSET`** o index da posição inicial de busca de dados. Em **`ROWS FETCH NEXT`** colocamos quantos dados queremos buscar a partir daquela posição.

Baseando-se nessas informações, pode-se ver que o **`OFFSET`** pode ser iniciado em 0, para buscar a partir do primeiro índica da tabela e **`ROWS FETCH`** inicia com o número de dados que você quer trazer da primeira vez. Assim que buscar os dados, o número de **`ROWS FETCH NEXT`** deve ser setado em **`OFFSET`**, para que os próximos dados sejam procurados a partir do próximo item, logo após o último que foi buscado sempre incrementando com o valor de **`ROWS FETCH NEXT`**.

Exemplo:

```sql
OFFSET (0) ROWS FETCH NEXT(3) || [0,1,2,3,4,5,6,7,8,9,10] -- 0,1,2
OFFSET (3) ROWS FETCH NEXT(3) || [0,1,2,3,4,5,6,7,8,9,10] -- 3,4,5
OFFSET (6) ROWS FETCH NEXT(3) || [0,1,2,3,4,5,6,7,8,9,10] -- 6,7,8
...
```

