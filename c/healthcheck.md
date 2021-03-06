# HealthCheck

## Documentação

{% embed url="https://docs.microsoft.com/en-us/aspnet/core/host-and-deploy/health-checks?view=aspnetcore-3.0" caption="Documentação para implementar" %}

{% embed url="https://jeremylindsayni.wordpress.com/2019/09/09/healthcheck-endpoints-in-c-in-mvc-projects-using-asp-net-core-and-writing-results-to-azure-application-insights/" caption="Artigo de como implementar Healthcheck-UI" %}

{% embed url="https://medium.com/swlh/how-to-implement-healthcheck-api-in-microservices-architecture-with-net-core-a5882369b016" caption="Outro artigo de como implementar Healthcheck-UI" %}

{% embed url="https://github.com/Xabaril/AspNetCore.Diagnostics.HealthChecks" caption="Biblioteca do projeto HealthCheck" %}

{% embed url="https://docs.sdasystems.org/infra/ops/pt/bestpractives.html\#health-check" caption="Documentação do IATec sobre HealthCheck" %}

## FullFramework

{% embed url="https://rimdev.io/using-aspnet-core-health-checks-with-aspnet-full-framework/" %}

{% embed url="https://www.nuget.org/packages/RimDev.AspNet.Diagnostics.HealthChecks" %}

{% embed url="https://github.com/ritterim/RimDev.AspNet.Diagnostics.HealthChecks" %}

## Implementação

[https://gist.github.com/SuricateCan/fbd6d8586a7184610eebbef673ebba3d](https://gist.github.com/SuricateCan/fbd6d8586a7184610eebbef673ebba3d)

{% embed url="https://gist.github.com/SuricateCan/fbd6d8586a7184610eebbef673ebba3d" %}

### Código de Webhook

```csharp
setup.AddWebhookNotification(
    name:"Slack", 
    uri: "https://hooks.slack.com/services/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",
    payload: "{\"text\":\"[[LIVENESS]] is failing with error message [[FAILURE]]\"}",
    restorePayload: "{\"text\":\"[[LIVENESS]] is restored\"}"
);
```

```csharp
//Configurando Endpoint do HealthCheck para configuração e utilização interna do HealthCheck-UI
endpoints.MapHealthChecks("/hc");
endpoints.MapHealthChecksUI(delegate (Options options)
{
    options.UIPath = "/hc-ui";
    options.AddCustomStylesheet("./Customization/custom.css");
});
```

```csharp
//Configurando rota de recursos para o HealthCheck UI
services.AddHealthChecksUI(setupSettings: setup =>
{
    setup.SetMinimumSecondsBetweenFailureNotifications(60);
    setup.AddHealthCheckEndpoint("Basic healthcheck", "/hc");
});
```

```csharp
//Rota para utilização interna do HealthCheck-UI
app.UseHealthChecks("/hc", new HealthCheckOptions()
{
    Predicate = _ => true,
    ResponseWriter = UIResponseWriter.WriteHealthCheckUIResponse
});
```

