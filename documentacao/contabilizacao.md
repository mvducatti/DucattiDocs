# Contabilização

**TL;DR** A contabilização nada mais é que a distribuição do dinheiro recebido para contas em específico.

1. Podem existir diversos AccountItem para um grupo \(**AccounItem é cada linha de conta dentro de um tipo selecionado na tela de Configuração Contábil**\), porém cada AccountItem possui somente uma conta \(o Account Item é uma conta em si\), que são as contas de banco para realizar a contabilização.
2. Na linguagem do AFS, **contabilização/contabilizar** significa criar o Lote Contábil \(Journal e os JournalItens\).
3. Journal \(lote contábil\), contém diversos JournalItem.
4. JournalItem por sua vez vão conter os dados das contas e os valores a serem depositados \(**porém sem ligação no banco com o AccountItem, isso é feito no C\#**\). Os valores vem do objetos a serem contabilizados.
5. Os objetos a serem contabilizados se relacionarão com a tabela Journal. Caso null o é porque o item do objeto ainda não foi contabilizado, senão é porque já ocorreu a contabilização \(**ex de objetos a serem contabilizados: CurrentAccount, TransactionAfs...**\).

## Estrutura da Contabilização

Batch é o caixa que recebeu as transações durante o período que esteve aberto, então quando fechar o caixa, vão existir diversos Transactions para aquele Batch específico. Dentro de cada TransactionAfs vai existir do detalhe da transação \(TransactionDetail\)...

* Batch \(Caixa\)
* Journal
* JournalItem \(item que está sendo contabilizado\)
* CurrentAccount
* TransactionAfs
* TransactionDetail
* ParcelDetail
* Parcel
* SaleProduct
* Product
* ART, ServiceGroup \(caixa pega somente pela entidade legal. As contas de crédito/débito vai para contas de caixa, as contas de débito/crédito vão pro accounts receivable type ou service group\)
* AccountingItem

