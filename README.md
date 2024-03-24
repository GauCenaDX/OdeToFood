# OdeToFood
## From the course ASP.NET Core Fundamentals by Scott Allen

Author Git Repository: https://github.com/OdeToCode/OdeToFood <br>
My Git Repository: https://github.com/GauCenaDX/OdeToFood

<a name="top"></a>

* [Drilling into Data](#drilling-into-data)
	* [Creating project and adding Razor Page](#creating-project-and-adding-razor-page)
	* [Injecting and Using Configuration](#injecting-and-using-configuration)
	* [Creating and working with Models/Entities in a separate project](#creating-and-working-with-models-aka-entities-in-a-separate-project)
	* [Building a Data Access Service](#building-a-data-access-service)
	* [Registering a Data Service](#registering-a-data-service)
	* [Building a Page Model](#building-a-page-model)
	* [Displaying a Table of Restaurants](#displaying-a-table-of-restaurants)
* [Working with Models and Model Binding](#working-with-models-and-model-binding)
	* [Building a Search Form](#building-a-search-form)
	* [Finding Restaurants by Name](#finding-restaurants-by-name)
	* [Binding to a Query String](#binding-to-a-query-string)
	* [Using Model Binding and Tag Helpers](#using-model-binding-and-tag-helpers)
	* [Building a Detail Page](#building-a-detail-page)
	* [Linking to the Details](#linking-to-the-details)
	* [Specifying Page Route](#specifying-page-route)
	* [Fetching Restaurant by Id](#fetching-restaurant-by-id)
	* [Handling Bad Requests](#handling-bad-requests)
* [Editing Data with Razor Pages](#editing-data-with-razor-pages)
	* [Creating the Restaurant Edit Page](#creating-the-restaurant-edit-page)
	* [Building an Edit Form with Tag Helpers](#building-an-edit-form-with-tag-helpers)
	* [Model Binding an HTTP POST Operation](#model-binding-an-http-post-operation)

## Drilling into Data

### Creating project and adding Razor Page
1. Create the project
2. Add a link to the navigation menu to go to the Restaurant page
   * /Restaurants/List
3. Add the Razor page List.cshtml and check if the page works

---

### Injecting and Using Configuration
1. Add a Message property with value "Hello World!" in the List Page Model
2. Display that Message on the Razor Page
3. Add a Message property with the value "Hello from appsettings!" to appsettings.json
4. Display that Message on the Razor Page instead

---

### Creating and working with Models aka Entities in a separate project
1. Add a new Class Library project name "OdeToFood.Core"
2. Create a new class named "Restaurants"
3. Define what information we want to store about a restaurant
   * Id, Name, Location, CuisineType

---

### Building a Data Access Service
1. Add another Class Library project to the solution called "OdeToFood.Data"
2. Add an Interface called "IRestaurantData"
   * This interface requires a method called "GetAll()" which returns an IEnumerable of Restaurant
3. Implement the GetAll() method from the interface by using a class called "InMemoryRestaurantData"
   * Create a list of restaurants, make up 3 restaurant objects and add them to this list
   * In the GetAll() method, use LINQ query to select every restaurant and order them by name

---

### Registering a Data Service
1. Tell the framework that whenever a component like a Razor Page needs something that implements IRestaurantData, it should provide that component with InMemoryRestaurantData (use the Singleton design pattern)
2. Inject IRestaurantData service to the List Page Model, call it "restaurantData"

---

### Building a Page Model
1. In the List Page Model, create a new public property of IEnumerable of Restaurant type called "Restaurants"
2. Use the provided service IRestaurantData, fetch the restaurants and store them in the public property Restaurants

---

### Displaying a Table of Restaurants
Take the list of restaurants that we prepared in our system and display them in a table.

[Back to top](#top)<br><br>

## Working with Models and Model Binding

### Building a Search Form

Add a Search Bar and a button with Search Icon on the /Restaurant/List page. We can use [Font Awesome](https://fontawesome.com/kits) for search icon.

![Search Bar and Search Button][SearchBarAndSearchButton]

---

### Finding Restaurants by Name

In IRestaurantData.cs from OdeToFood.Data project, change GetAll() data service to GetRestaurantsByName(). This method allows for passing in a string parameter that represents the name of the restaurant or the partial name that we're going to match.

* If a user passes an empty name of a null name, we'll return all the restaurants.
* Add the WHERE operator to the LINQ query.
	* Check if the parameter is null or empty, or someone has passed in a name (or the starting part of the name of a restaurant).

---

### Binding to a Query String

1. Give the input search box a name, "searchTerm".
2. Add a parameter with the same name "searchTerm" to our OnGet() method.
3. Modify the OnGet() method to work with our changes.

![Enter 'Sc' as search term][BindingToAQueryString1]

![Return only Scott's Pizza][BindingToAQueryString2]

---

### Using Model Binding and Tag Helpers

1. Add a public property named "SearchTerm".
2. Assign BindProperty attribute to make it both input model and output model
	* Make sure it works with GET request.
3. Use asp-for tag helper to bind this SearchTerm property with the input search box.

![Retained search term][UsingModelBindingAndTagHelper]

---

### Building a Detail Page

1. Add a Razor Page named "Detail" under Restaurants folder.
2. Have a pulic property of type Restaurant. Name it "Restaurant".
3. For now, when receive a GET request, a empty object of Restaurant type will be assigned to this Restaurant property.
4. Build a simple UI on the Razor Page. This page should show the restaurant's name, the id, the location and the type of cuisine.
5. Provide a navigation button that will take a user back to the list of all restaurants. Name this button "All Restaurants".

![Empty restaurant detail page][BuildingADetailPage]

---

### Linking to the Details

1. Update OnGet() method in the Detail page to allow passing in a restaurant ID.
2. On the List page, we want to add another table cell to the right side of the table, and inside of the cell we're going to add an action that will allow us to get to the Detail page.
	* Use asp-for tag helper to redirect user to the Detail page,
	* Use asp-route tag helper to pass paremeter between pages.

![Added Zoom-in icon][LinkingToTheDetails1]

![Detail page shows restaurant ID][LinkingToTheDetails2]

---

### Specifying Page Route

Use @page directive to make restaurant Id as part of the URL path to reach Detail page.

![Using URL Path format][SpecifyingPageRoute]

---

### Fetching Restaurant by Id

IRestaurantDate should provide a method to return a restaurant object when providing an id.

![Get to Detail Page from List][FetchingRestaurantById1]
![Display Restaurant Object by Id][FetchingRestaurantById2]

A null object exception will be thrown if user put in an invalid id on the URL path.

![Exception Thrown for Invalid Id][FetchingRestaurantById3]

---

### Handling Bad Requests

When user provid an invalid restaurant ID, redirect user to NotFound page by make use of IActionResult return type and RedirectToPage() helper method.

![Redirect to NotFound page][HandlingBadRequests]

[Back to top](#top)<br><br>

## Editing Data with Razor Pages

### Creating the Restaurant Edit Page

Add a link on the List page to allow user to get to the Edit page.

![Edit Button on List Page][CreatingRestaurantEditPage1]
![Edit Page][CreatingRestaurantEditPage2]

---

### Building an Edit Form with Tag Helpers

Build an Edit form to allow user to edit the restaurant name, location and cuisine type. We also want to include the Id of the restaurant as a form value because we want the form to include all the information that we need to put together to update a restaurant. However, we don't want the user to edit the Id.

Tips:

* Make us of hidden input type, asp-for, asp-items
* Use GetEnumSelectList() to create a collection of SelectListItem

![Edit Form][BuildingEditFormWithTagHelpers]

---

### Model Binding an HTTP POST Operation

When user click on the Save button, the restaurant with the associated id will be updated with the new input from the form.

![Save updated restaurant info][ModelBindingHttpPost]

[Back to top](#top)<br><br>



[SearchBarAndSearchButton]: /GitImages/create-search-form.jpg
[BindingToAQueryString1]: /GitImages/binding-to-a-query-string-1.jpg
[BindingToAQueryString2]: /GitImages/binding-to-a-query-string-2.jpg
[UsingModelBindingAndTagHelper]: /GitImages/using-model-binding-and-tag-helper.jpg
[BuildingADetailPage]: /GitImages/building-a-detail-page.jpg
[LinkingToTheDetails1]: /GitImages/linking-to-the-details-1.jpg
[LinkingToTheDetails2]: /GitImages/linking-to-the-details-2.jpg
[SpecifyingPageRoute]: /GitImages/specifying-page-route.jpg
[FetchingRestaurantById1]: /GitImages/fetching-restaurant-by-id-1.png
[FetchingRestaurantById2]: /GitImages/fetching-restaurant-by-id-2.png
[FetchingRestaurantById3]: /GitImages/fetching-restaurant-by-id-3.png
[HandlingBadRequests]: /GitImages/handling-bad-requests.png
[CreatingRestaurantEditPage1]: /GitImages/creating-the-restaurant-edit-page-1.png
[CreatingRestaurantEditPage2]: /GitImages/creating-the-restaurant-edit-page-2.png
[BuildingEditFormWithTagHelpers]: /GitImages/building-edit-form-with-tag-helpers.png
[ModelBindingHttpPost]: /GitImages/model-binding-http-post.png

