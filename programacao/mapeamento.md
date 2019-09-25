# Mapeamento

## Modificando a Propriedade

Sempre quando criar um mapeamento, ir nas propriedades e setar o **`Build Action`** como **`Embedded Resource`**

![](../.gitbook/assets/image.png)

## String x AnsiString

**`String`** é utilizado no mapeamento quando o campo do banco é **`CHAR`**.  
**`AnsiString`** será utilizado no mapeamento quando o campo do banco for **`VARCHAR`**  
  
Exemplo:

```sql
//Banco
Name || varchar
Code || char

//Arquivo .hbm.xml
<property name="Name"              column="Name"              type="AnsiString" />
<property name="Code"              column="Code"              type="String" />
```



