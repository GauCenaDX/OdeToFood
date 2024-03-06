# OdeToFood
## From the course ASP.NET Core Fundamentals by Scott Allen

Author Git Repository: https://github.com/OdeToCode/OdeToFood <br>
My Git Repository: https://github.com/GauCenaDX/OdeToFood


## Drilling into Data

### Creating project and adding Razor Page
1. Create the project
2. Add a link to the navigation menu to go to the Restaurant page
   * /Restaurants/List
3. Add the Razor page List.cshtml and check if the page works

### Injecting and Using Configuration
1. Add a Message property with value "Hello World!" in the List Page Model
2. Display that Message on the Razor Page
3. Add a Message property with the value "Hello from appsettings!" to appsettings.json
4. Display that Message on the Razor Page instead

### Creating and working with Models/Entities in a separate project
1. Add a new Class Library project name "OdeToFood.Core"
2. Create a new class named "Restaurants"
3. Define what information we want to store about a restaurant
   * Id, Name, Location, CuisineType

### Building a Data Access Service
1. Add another Class Library project to the solution called "OdeToFood.Data"
2. Add an Interface called "IRestaurantData"
   * This interface requires a method called "GetAll()" which returns an IEnumerable of Restaurant
3. Implement the GetAll() method from the interface by using a class called "InMemoryRestaurantData"
   * Create a list of restaurants, make up 3 restaurant objects and add them to this list
   * In the GetAll() method, use LINQ query to select every restaurant and order them by name

### Registering a Data Service
1. Tell the framework that whenever a component like a Razor Page needs something that implements IRestaurantData, it should provide that component with InMemoryRestaurantData (use the Singleton design pattern)
2. Inject IRestaurantData service to the List Page Model, call it "restaurantData"

### Building a Page Model
1. In the List Page Model, create a new public property of IEnumerable of Restaurant type called "Restaurants"
2. Use the provided service IRestaurantData, fetch the restaurants and store them in the public property Restaurants

### Displaying a Table of Restaurants
Take the list of restaurants that we prepared in our system and display them in a table.

## Working with Models and Model Binding

### Building a Search Form

Add a Search Bar and a button with Search Icon on the /Restaurant/List page. We can use [Font Awesome](https://fontawesome.com/kits) for search icon.

![Search Bar and Search Button][SearchBarAndSearchButton]


[SearchBarAndSearchButton]: /GitImages/create-search-form.jpg
