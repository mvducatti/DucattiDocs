# API - Entity Framework

## Migration

### Acessando pelo Visual Studio

![No Visual Studio, entre no caminho abaixo](../.gitbook/assets/image%20%281%29.png)

![Nesse caso, selecione o Sda.Abacom.Py.Infrasctructure](../.gitbook/assets/image%20%288%29.png)

### Criando Migration

Cria o script de migração pra adicionar no banco 

```c
add-migration newMigrationDescription
```

Atualizar o banco com base nas migrações 

```c
update-database
```

### Removendo em Caso de Erro

Reverte se você rodou o update-database , tem que ter cuidado quando algum dado já foi inserido com foreign key, senão tem que deletar o relacionamento das FKs diretamente no banco para poder rodar

```c
update-database -migration:nomedoArquivodeMigração
```

Remove a última migração adicionado

```c
remove-migration
```

### Finalizando

Após criar o migration, o mesmo vai aparecer em dois lugares:

![Pasta Migrations](../.gitbook/assets/image%20%283%29.png)

![Tabela de miration no banco de dados](../.gitbook/assets/image%20%287%29.png)

