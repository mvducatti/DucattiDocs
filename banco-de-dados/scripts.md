# Scripts Úteis

## Abrir/Fechar Mês Contábil

```sql
select * from legalEntity where Name like '%Virtual%'

select * from AccountingMonth where LegalEntityId = 'A24362D2-8C51-42AA-BC28-CDCFD050EA80'

declare @closeMonth uniqueidentifier = '26EF3BED-EE5D-4B74-A0B0-B59CC9C78856'
declare @openMonth uniqueidentifier = 'D17D61DD-C469-4254-99A3-2AEE51956477'

--Setando qual mês para fechar
begin transaction
update AccountingMonth
set ProvisionClosingDate = GETDATE(), ClosingDate = GETDATE()
where AccountingMonthId in(
@openMonth
)
rollback transaction

--Setando qual mês para abrir
begin transaction
update AccountingMonth
set ProvisionClosingDate = null, ClosingDate = null
where AccountingMonthId in(
@openMonth
)
rollback transaction
```

