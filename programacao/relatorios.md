# Relatórios

## Referências

Ver User Story 59329  
Relatórios Referência: **`ReceiptExtract`** \| **`DebtPayment`** \| **`RetentionSimulationReport`**

## Dicas

Criar Dto, criar chamadas de banco, criar arquivo de Report e chamar ele de algum lugar.

Adicionar nas **`propriedades`** do relatório \(botão direito fora do layout\)

```coffeescript
Public Dim Strings As Sda.Afs.View.ReportsLocalization.Localizer
	Protected Overrides Sub OnInit()
		Strings = new Sda.Afs.View.ReportsLocalization.Localizer()
	End Sub
```

E a **`referência`**

```csharp
Sda.Afs.View.ReportsLocalization, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null
```

