# Programação Banco de Dados

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

## Exemplo Padrão de Segurança em Script

```sql
-- Segurança 1 pt.1
-- Marks the starting point of an explicit, local transaction.
BEGIN TRANSACTION

-- Segurança 2
-- When SET XACT_ABORT is ON, if a Transact-SQL statement raises a run-time error, 
-- the entire transaction is terminated and rolled back.
set xact_abort on

alter table DocumentType
add IsIdentifier bit not null default 0;

alter table DocumentType
add NumberMask varchar(255);

-- Segurança 3
-- As vezes dentro de uma transaction, alter tables ou outras funções
-- não garantem que o próximo select vai funcionar. O exec garante que a query
-- seja rodada nas melhores condições
exec('
update DocumentType
set IsIdentifier = 1 
where DocumentTypeId in (''5E1B4D19-D937-42EF-B5BD-3F638DE68B80'', ''E89F85D8-8F6B-4B82-B082-3959284B02BC'')
')

select * from DocumentType

-- Segurança 1 pt.2
-- The ROLLBACK command in SQL Server is generally used to undo the transaction 
-- that have not been saved to the database. Is Generally used for tests.
ROLLBACK TRANSACTION
```

## Trabalhando com Transactions

Você pode dar nome à uma transaction, selecionar o código excluindo a linha de rollback, testar no ambiente o que precisa ser testado e após isso dar rollback na mesma transaction, assim fica uma forma fácil, limpa e segura de testar os scripts.

```sql
begin transaction PaymentTypeTransactionUpdate

set xact_abort on

delete from BasicConfigurationPaymentType

DECLARE @cnt INT = 1;

WHILE @cnt <= 12
BEGIN
	insert into BasicConfigurationPaymentType
	select bc.BasicConfigurationId, @cnt from BasicConfiguration bc
	inner join Entity e on e.EntityId = bc.EntityId
	where e.CountryId = 1
   SET @cnt = @cnt + 1;
END;

select * from BasicConfigurationPaymentType

rollback transaction PaymentTypeTransactionUpdate

--SELECT * FRom PaymentType
```

## Cast

Quando for necessário realizar uma conversão de valores para inserir um dado novo no banco de dados podemos usar o CAST

```sql
INSERT INTO MoneyReport values (
    CAST('255,55' AS MONEY), 
    CAST('0x00D1CCE9771AE7554D4...' as varbinary)
)
```

