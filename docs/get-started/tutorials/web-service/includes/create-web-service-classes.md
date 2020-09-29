REST requests are made over HTTP using the same HTTP verbs that web browsers use to retrieve pages and to send data to servers. In this exercise, you will create a class that uses the GET verb to retrieve data from the [OpenWeatherMap](https://openweathermap.org/) web API. This web API can be used to retrieve weather forecast data for a specified location. Using this web API requires that you sign up for an API key.

> [!div class="nextstepaction"]
> [Sign up for API key](https://home.openweathermap.org/users/sign_up)

# [Visual Studio](#tab/vswin)

1. In **Solution Explorer**, in the **WebServiceTutorial** project, add a new class named `Constants` to the project. Then, in **Constants.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public const string OpenWeatherMapEndpoint = "https://api.openweathermap.org/data/2.5/weather";
            public const string OpenWeatherMapAPIKey = "INSERT_API_KEY_HERE";
        }
    }
    ```

    This code defines two constants. The `OpenWeatherMapEndpoint` constant defines the endpoint against which web requests will be made, and the `OpenWeatherMapAPIKey` constant defines your personal API key for the [OpenWeatherMap](https://openweathermap.org/) service.

    > [!IMPORTANT]
    > You must set your personal OpenWeatherMap API key as the value of the `OpenWeatherMapAPIKey` constant.

1. In **Solution Explorer**, in the **WebServicesTutorial** project, add a new class named `WeatherData` to the project. Then, in **WeatherData.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class WeatherData
        {
            [JsonProperty("name")]
            public string Title { get; set; }

            [JsonProperty("weather")]
            public Weather[] Weather { get; set; }

            [JsonProperty("main")]
            public Main Main { get; set; }

            [JsonProperty("visibility")]
            public long Visibility { get; set; }

            [JsonProperty("wind")]
            public Wind Wind { get; set; }
        }

        public class Main
        {
            [JsonProperty("temp")]
            public double Temperature { get; set; }

            [JsonProperty("humidity")]
            public long Humidity { get; set; }
        }

        public class Weather
        {
            [JsonProperty("main")]
            public string Visibility { get; set; }
        }

        public class Wind
        {
            [JsonProperty("speed")]
            public double Speed { get; set; }
        }
    }
    ```

    This code defines four classes that are used to model the JSON data retrieved from the web service. Each property is decorated with a `JsonProperty` attribute, containing a JSON field name. Newtonsoft.Json will use this mapping of JSON field names to CLR properties when deserializing the JSON data into model objects.

    > [!NOTE]
    > The class definitions above have been simplified, and do not fully model the JSON data retrieved from the web service. For a complete data model example, see the [Weather App](/samples/xamarin/xamarin-forms-samples/weather/) sample.

1. In **Solution Explorer**, in the **WebServiceTutorial** project, add a new class named `RestService` to the project. Then, in **RestService.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
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

            public async Task<WeatherData> GetWeatherDataAsync(string uri)
            {
                WeatherData weatherData = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        weatherData = JsonConvert.DeserializeObject<WeatherData>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return weatherData;
            }
        }
    }
    ```

    This code defines a single method, `GetWeatherDataAsync`, that retrieves weather data for a specified location from the [OpenWeatherMap](https://openweathermap.org/) web API. This method uses the `HttpClient.GetAsync` method to send a GET request to the web API specified by the `uri` argument. The web API sends a response that is stored in a `HttpResponseMessage` object. The response includes a HTTP status code, which indicates whether the HTTP request succeeded or failed. Provided that the request succeeds, the web API responds with HTTP status code 200 (OK), and a JSON response, which is in the `HttpResponseMessage.Content` property. This JSON data is read to a `string` using the `HttpContent.ReadAsStringAsync` method, before being deserialized into a `WeatherData` object using the `JsonConvert.DeserializeObject` method. This method uses the mappings between JSON field names and CLR properties, that are defined in the `WeatherData` class, to perform the deserialization.

1. Build the solution to ensure there are no errors.

# [Visual Studio for Mac](#tab/vsmac)

1. In **Solution Pad**, in the **WebServiceTutorial** project, add a new class named `Constants` to the project. Then, in **Constants.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    namespace WebServiceTutorial
    {
        public static class Constants
        {
            public static string OpenWeatherMapEndpoint = "https://api.openweathermap.org/data/2.5/weather";
            public static string OpenWeatherMapAPIKey = "INSERT_API_KEY_HERE";
        }
    }
    ```

    This code defines two constants. The `OpenWeatherMapEndpoint` constant defines the endpoint against which web requests will be made, and the `OpenWeatherMapAPIKey` constant defines your personal API key for the [OpenWeatherMap](https://openweathermap.org/) service.

    > [!IMPORTANT]
    > You must set your personal OpenWeatherMap API key as the value of the `OpenWeatherMapAPIKey` constant.

1. In **Solution Pad**, in the **WebServicesTutorial** project, add a new class named `WeatherData` to the project. Then, in **WeatherData.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using Newtonsoft.Json;

    namespace WebServiceTutorial
    {
        public class WeatherData
        {
            [JsonProperty("name")]
            public string Title { get; set; }

            [JsonProperty("weather")]
            public Weather[] Weather { get; set; }

            [JsonProperty("main")]
            public Main Main { get; set; }

            [JsonProperty("visibility")]
            public long Visibility { get; set; }

            [JsonProperty("wind")]
            public Wind Wind { get; set; }
        }

        public class Main
        {
            [JsonProperty("temp")]
            public double Temperature { get; set; }

            [JsonProperty("humidity")]
            public long Humidity { get; set; }
        }

        public class Weather
        {
            [JsonProperty("main")]
            public string Visibility { get; set; }
        }

        public class Wind
        {
            [JsonProperty("speed")]
            public double Speed { get; set; }
        }
    }
    ```

    This code defines four classes that are used to model the JSON data retrieved from the web service. Each property is decorated with a `JsonProperty` attribute, containing a JSON field name. Newtonsoft.Json will use this mapping of JSON field names to CLR properties when deserializing the JSON data into model objects.

    > [!NOTE]
    > The class definitions above have been simplified, and do not fully model the JSON data retrieved from the web service. For a complete data model example, see the [Weather App](/samples/xamarin/xamarin-forms-samples/weather/) sample.

1. In **Solution Pad**, in the **WebServiceTutorial** project, add a new class named `RestService` to the project. Then, in **RestService.cs**, remove all of the template code and replace it with the following code:

    ```csharp
    using System;
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

            public async Task<WeatherData> GetWeatherDataAsync(string uri)
            {
                WeatherData weatherData = null;
                try
                {
                    HttpResponseMessage response = await _client.GetAsync(uri);
                    if (response.IsSuccessStatusCode)
                    {
                        string content = await response.Content.ReadAsStringAsync();
                        weatherData = JsonConvert.DeserializeObject<WeatherData>(content);
                    }
                }
                catch (Exception ex)
                {
                    Debug.WriteLine("\tERROR {0}", ex.Message);
                }

                return weatherData;
            }
        }
    }
    ```

    This code defines a single method, `GetWeatherDataAsync`, that retrieves weather data for a specified location from the [OpenWeatherMap](https://openweathermap.org/) web API. This method uses the `HttpClient.GetAsync` method to send a GET request to the web API specified by the `uri` argument. The web API sends a response that is stored in a `HttpResponseMessage` object. The response includes a HTTP status code, which indicates whether the HTTP request succeeded or failed. Provided that the request succeeds, the web API responds with HTTP status code 200 (OK), and a JSON response, which is in the `HttpResponseMessage.Content` property. This JSON data is read to a `string` using the `HttpContent.ReadAsStringAsync` method, before being deserialized into a `WeatherData` object using the `JsonConvert.DeserializeObject` method. This method uses the mappings between JSON field names and CLR properties, that are defined in the `WeatherData` class, to perform the deserialization.

1. Build the solution to ensure there are no errors.