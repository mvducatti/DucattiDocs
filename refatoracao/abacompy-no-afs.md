# AbacomPY no AFS

## SerieService

```csharp
public async Task<ApiServiceOutput<HttpResponseMessage>> DeleteSerieWithHttpMessagesAsync2(int id, Dictionary<string, List<string>> customHeaders = null, CancellationToken cancellationToken = default(CancellationToken))
{
    // Construct URL
    var _baseUrl = Client.BaseUri.AbsoluteUri;
    var _url = new System.Uri(new System.Uri(_baseUrl + (_baseUrl.EndsWith("/") ? "" : "/")), "api/Serie/{id}").ToString();
    _url = _url.Replace("{id}", System.Uri.EscapeDataString(SafeJsonConvert.SerializeObject(id, Client.SerializationSettings).Trim('"')));

    // Create HTTP transport objects
    var _httpRequest = new HttpRequestMessage();
    _httpRequest.Method = HttpMethod.Delete;
    _httpRequest.RequestUri = new System.Uri(_url);

    // Set Headers
    if (customHeaders != null)
    {
        foreach (var _header in customHeaders)
        {
            if (_httpRequest.Headers.Contains(_header.Key))
            {
                _httpRequest.Headers.Remove(_header.Key);
            }
            _httpRequest.Headers.TryAddWithoutValidation(_header.Key, _header.Value);
        }
    }

    // Serialize Request
    HttpResponseMessage _httpResponse = await Client.HttpClient.SendAsync(_httpRequest).ConfigureAwait(false);

    HttpStatusCode _statusCode = _httpResponse.StatusCode;
    
    return new ApiServiceOutput<HttpResponseMessage>(_httpResponse, _statusCode);
}
```

## SerieServiceExtension

```csharp
public static async Task Delete2(this ISerieService operations, int id)
{
    await operations.DeleteAsync2(id);
}

public static async Task<ApiServiceOutput<HttpResponseMessage>> DeleteAsync2(this ISerieService operations, int id, CancellationToken cancellationToken = default(CancellationToken))
{
    return await operations.DeleteSerieWithHttpMessagesAsync2(id, null, cancellationToken);
}
```

## SerieController

```csharp
ServiceOutput<AbacomPYClient> client = AbacomPYClientFactory.GetInstance();
if (!client.Success) return Json(new { success = false, message = client.Message });

ApiServiceOutput<HttpResponseMessage> output = client.Result.Serie.DeleteSerie2(serieId).GetAwaiter().GetResult();
```

## ApiServiceOutput

```csharp
using System;
using System.Collections.Generic;
using System.Net;
 
namespace Sda.Afs.Entities.SharedKernel
{
    public class ApiServiceOutput<T>
    {
        private List<string> messages = new List<string>();
 
        public bool Success => ValidateStatusCode();
        public string Message => string.Join("#", messages);
        public T Result { get; set; }
        public HttpStatusCode httpResult { get; set; } 
 
        private bool ValidateStatusCode()
        {
            switch (httpResult)
            {
                case HttpStatusCode.OK:
                case HttpStatusCode.Created:
                case HttpStatusCode.NoContent:
                    return true;
                default:
                    return false;
            }
        }
 
        public ApiServiceOutput()
        {
            this.httpResult = HttpStatusCode.OK;
        }
 
        public ApiServiceOutput(string message)
        {
            httpResult = HttpStatusCode.BadRequest;
            messages.Add(message ?? throw new ArgumentNullException(nameof(message)));
            Result = default(T);
        }
 
        public ApiServiceOutput(T result)
        {
            httpResult = HttpStatusCode.OK;
            Result = result;
        }
 
        public ApiServiceOutput(T result, HttpStatusCode statusCode)
        {
            httpResult = statusCode;
            Result = result;
        }
 
        public ApiServiceOutput(string message, HttpStatusCode statusCode)
        {
            httpResult = statusCode;
            messages.Add(message ?? throw new ArgumentNullException(nameof(message)));
 
        }
 
        public ApiServiceOutput(HttpStatusCode statusCode)
        {
            httpResult = statusCode;
        }
 
        public ApiServiceOutput(Exception e)
        {
            this.httpResult = HttpStatusCode.InternalServerError;
            this.AddMessage(e.Message);
        }
 
        public void AddMessage(string message)
        {
            if (Success)
            {
                httpResult = HttpStatusCode.BadRequest;
            }
 
            this.messages.Add(message ?? throw new ArgumentNullException(nameof(message)));
        }
 
        public void AddMessage(string message, HttpStatusCode statusCode)
        {
            httpResult = statusCode;
            this.messages.Add(message ?? throw new ArgumentNullException(nameof(message)));
        }
    }
}
```

