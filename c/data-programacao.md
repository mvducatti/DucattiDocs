# Data - Programação

## Casos de Uso

### View - Exibindo data para o usuário

Quando é encessário mostrar a data para o usuário, a data deve ser convertida para o TimeZone. Utiliza-se o método **DatimeZone.ConvertToTimeZone\(\);**

### **Salvando no Banco //Business**

Todas as horas devem ser salvas no banco utilizando o **DateTime.UtcNow\(\).**

_obs: Em alguns casos, esse método vai ser utilizado para consultas que requerem o dia atual._

### **Consultas**

Utilza-se os métodos **DateTimeZone.StartDate\(\)** para início de data da consulta e **DateTimeZone.EndDate\(\)** para o período final da data, pois garante que fica 23:59:59, pois se os dados forem salvos após 00:00:00, não vão ser buscados no banco e vão faltar dados.

## Tela para Comprar Datas

{% embed url="http://localhost:1078/Home/Datetimeinfo" %}

![](../.gitbook/assets/image%20%282%29.png)

