# API - Entity Framework

## Migration

Cria o script de migração pra adicionar no banco 

```c
add-migration newMigrationDescription
```

Atualizar o banco com base nas migrações 

```c
update-database
```

Reverte se você rodou o update-database , tem que ter cuidado quando algum dado já foi inserido com foreign key, senão tem que deletar o relacionamento das FKs diretamente no banco para poder rodar

```c
update-database -migration:nomedoArquivodeMigração
```

Remove a última migração adicionado

```c
remove-migration
```



