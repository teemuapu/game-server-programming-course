# Assignment 1

The purpose of the following exercise is to get you familiar with .NET concepts to get you ready to do some serious server programming.

The exercise uses an API for the HSL City Bikes. If you don't care for City Bikes you can feel free to do a variation of this exercise with any other public API. Here are some game related API's (some of which might require registeration):

https://www.programmableweb.com/news/top-10-games-apis-eve-online-riot-games-battle.net/analysis/2015/11/25

---

## 1. Create new application

Create a console application in a empty folder with `dotnet new console` -command. This application will print out the number of available city bikes in a requested station.

Start by modifying the Program.cs with the following lines of code:

```C#
static async Task Main(string[] args)
{
    Console.WriteLine(args[0]);
}
```

It will make the application to print the commandline argument passed.

Run the app with the following command `dotnet run {station_name}` inside the project folder (the `{station_name}` can be any random string at this point).

---

## 2. Create an interface

Create the following interface:

```C#
public interface ICityBikeDataFetcher
{
    Task<int> GetBikeCountInStation(string stationName);
}
```

---

## 3. Add a dependency

Next we will add a dependency to a JSON-library which we will use to parse the JSON-payload returned from the API into a C# object graph.

Inside the project folder:

- Use `dotnet add package NewtonSoft.Json`
- Use `dotnet restore` command to resolve the dependency.

---

## 4. Create a class implementing the interface

Create a class called RealTimeCityBikeDataFetcher which implements the ICityBikeDataFetcher and queries the API `http://api.digitransit.fi/routing/v1/routers/hsl/bike_rental` (You can copy paste the API Url into your browser and see what it returns)

### Hints for implementation:

- Create an instance from the class `System.Net.Http.HttpClient`, and use it to do a request with a method called `GetStringAsync`
- There is an example how to use the `HttpClient` here: https://docs.microsoft.com/en-us/dotnet/api/system.net.http.httpclient?view=netcore-3.1
- Remember to use `async` and `await` keywords in your implementation of a asynchronous method. You can make your `Main` method asynchronous as well! (make it return a **Task**).
- Next you need to define your own `BikeRentalStationList` class that correspond the Json the API returns. That is, it should contain properties with same names and types as what is in the Json.
- Deserialize the string to a C# object using `JsonConvert.DeserializeObject<BikeRentalStationList>`
- Find the information you are looking for from the `BikeRentalStationList` (bike count in a certain station) and return it as the result from the method

---

## 5. Throw an exception

Throw ArgumentException (provided by .NET framework) if the user calls the "GetBikeCountInStation" with a string which contains numbers. Catch it in the calling code and print "Invalid argument: " and the message property of the exception.

---

## 6. Create and throw your own exception

Create your own Exception called NotFoundException. Throw it if the station can not be found. Catch it in the calling code and print "Not found: " and the message property of the exception.

---

## 7. Create an alternative implementation

Create a class called OfflineCityBikeDataFetcher which also implements the ICityBikeDataFetcher. Get a file called `bikedata.txt` from the repository of this course. Find a way to read the requested data from the file.

---

## 8. Implement commandline arguments

Make the console application to accept an additional string argument, `offline` or `realtime`, and decide the implementation of ICityBikeDataFetcher based on that.
