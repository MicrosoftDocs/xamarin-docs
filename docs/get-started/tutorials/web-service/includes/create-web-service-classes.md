REST requests are made over HTTP using the same HTTP verbs that web browsers use to retrieve pages and to send data to servers. In this exercise, you will create a class that uses the GET verb to retrieve .NET repository data from the GitHub web API.

# [Visual Studio](#tab/vswin)

1. In **Solution Explorer**, in the **WebServiceTutorial** project, add a new class named `Constants` to the project. Then, in **Constants.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public const string GitHubReposEndpoint = "https://api.github.com/orgs/dotnet/repos";
        }
    }
    ```

    This code defines a single constant - the endpoint against which web requests will be made.

1. In **Solution Explorer**, in the **WebServicesTutorial** project, add a new class named `Repository` to the project. Then, in **Repository.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class Repository
        {
            [JsonProperty("name")]
            public string Name { get; set; }

            [JsonProperty("description")]
            public string Description { get; set; }

            [JsonProperty("html_url")]
            public Uri GitHubHomeUrl { get; set; }

            [JsonProperty("homepage")]
            public Uri Homepage { get; set; }

            [JsonProperty("watchers")]
            public int Watchers { get; set; }
        }
    }
    ```

    This code defines a `Repository` class that's used to model the JSON data retrieved from the web service. Each property is decorated with a `JsonProperty` attribute, containing a JSON field name. Newtonsoft.Json will use this mapping of JSON field names to CLR properties when deserializing the JSON data into model objects.

    > [!NOTE]
    > The class definition above has been simplified, and does not fully model the JSON data retrieved from the web service.

1. In **Solution Explorer**, in the **WebServiceTutorial** project, add a new class named `RestService` to the project. Then, in **RestService.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Diagnostics;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class RestService
        {
            HttpClient _client;

            public RestService()
            {
                _client = new HttpClient();
            }

            public async Task<List<Repository>> GetRepositoriesAsync(string uri)
            {
                List<Repository> repositories = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        repositories = JsonConvert.DeserializeObject<List<Repository>>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return repositories;
            }
        }
    }
    ```

    This code defines a single method, `GetRepositoriesAsync`, that retrieves .NET repository data from the GitHub web API. This method uses the `HttpClient.GetAsync` method to send a GET request to the web API specified by the `uri` argument. The web API sends a response that is stored in a `HttpResponseMessage` object. The response includes a HTTP status code, which indicates whether the HTTP request succeeded or failed. Provided that the request succeeds, the web API responds with HTTP status code 200 (OK), and a JSON response, which is in the `HttpResponseMessage.Content` property. This JSON data is read to a `string` using the `HttpContent.ReadAsStringAsync` method, before being deserialized into a `List<Repository>` object using the `JsonConvert.DeserializeObject` method. This method uses the mappings between JSON field names and CLR properties, that are defined in the `Repository` class, to perform the deserialization.

1. Build the solution to ensure there are no errors.

# [Visual Studio for Mac](#tab/vsmac)

1. In **Solution Pad**, in the **WebServiceTutorial** project, add a new class named `Constants` to the project. Then, in **Constants.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public const string GitHubReposEndpoint = "https://api.github.com/orgs/dotnet/repos";
        }
    }
    ```

    This code defines a single constant - the endpoint against which web requests will be made.

1. In **Solution Pad**, in the **WebServicesTutorial** project, add a new class named `Repository` to the project. Then, in **Repository.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class Repository
        {
            [JsonProperty("name")]
            public string Name { get; set; }

            [JsonProperty("description")]
            public string Description { get; set; }

            [JsonProperty("html_url")]
            public Uri GitHubHomeUrl { get; set; }

            [JsonProperty("homepage")]
            public Uri Homepage { get; set; }

            [JsonProperty("watchers")]
            public int Watchers { get; set; }
        }
    }
    ```

    This code defines a `Repository` class that's used to model the JSON data retrieved from the web service. Each property is decorated with a `JsonProperty` attribute, containing a JSON field name. Newtonsoft.Json will use this mapping of JSON field names to CLR properties when deserializing the JSON data into model objects.

    > [!NOTE]
    > The class definition above has been simplified, and does not fully model the JSON data retrieved from the web service.

1. In **Solution Pad**, in the **WebServiceTutorial** project, add a new class named `RestService` to the project. Then, in **RestService.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Diagnostics;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class RestService
        {
            HttpClient _client;

            public RestService()
            {
                _client = new HttpClient();
            }

            public async Task<List<Repository>> GetRepositoriesAsync(string uri)
            {
                List<Repository> repositories = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        repositories = JsonConvert.DeserializeObject<List<Repository>>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return repositories;
            }
        }
    }
    ```

    This code defines a single method, `GetRepositoriesAsync`, that retrieves .NET repository data from the GitHub web API. This method uses the `HttpClient.GetAsync` method to send a GET request to the web API specified by the `uri` argument. The web API sends a response that is stored in a `HttpResponseMessage` object. The response includes a HTTP status code, which indicates whether the HTTP request succeeded or failed. Provided that the request succeeds, the web API responds with HTTP status code 200 (OK), and a JSON response, which is in the `HttpResponseMessage.Content` property. This JSON data is read to a `string` using the `HttpContent.ReadAsStringAsync` method, before being deserialized into a `List<Repository>` object using the `JsonConvert.DeserializeObject` method. This method uses the mappings between JSON field names and CLR properties, that are defined in the `Repository` class, to perform the deserialization.

1. Build the solution to ensure there are no errors.
