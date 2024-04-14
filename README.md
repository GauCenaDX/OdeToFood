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
	* [Adding Validation Checks](#adding-validation-checks)
	* [Using Model State and Showing Validation Errors](#using-model-state-and-showing-validation-errors)
	* [Following Post Redirect Get Pattern](#following-post-redirect-get-pattern)
	* [Building a Create Restaurant Page](#building-a-create-restaurant-page)
	* [Adding Create to the Data Access Service](#adding-create-to-the-data-access-service)
	* [Handling Create and Update Logic](#handling-create-and-update-logic)
	* [Confirming the Last Operation](#confirming-the-last-operation)
* [Working with SQL Server and the Entity Framework Core](#working-with-sql-server-and-the-entity-framework-core)
	* [Installing the Entity Framework](#installing-the-entity-framework)
	* [Implementing an Entity Framework DbContext](#implementing-an-entity-framework-dbcontext)
	* [Using the Entity Framework Tools](#using-the-entity-framework-tools)
	* [Adding Connection Strings and Registering Services](#adding-connection-strings-and-registering-services)
	* [Adding Database Migrations](#adding-database-migrations)

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

![Modify restaurant info][ModelBindingHttpPost1]

![Save updated restaurant info][ModelBindingHttpPost2]

---

### Adding Validation Checks

* Fix the issue where we lose our list of cuisines when editing a restaurant.
* Enforce validation check for restaurant name and location.

---

### Using Model State and Showing Validation Errors

* Modify the Edit page model to only update data when Model State is valid.
* Use asp-validation-for tag helper to show validation errors.

![Show Validation Errors for Name and Location][UsingModelStateAndShowingValidationErrors]

---

### Following Post Redirect Get Pattern

Leaving the user on a page with Http Post operation is dangerous, because they can refresh the page and another Post opertion will be performed.

So using the Post/Redirect/Get pattern, when user edits restaurant information and clicks 'Save', let's redirect them to the Detail page so they can see their changes.

![User edits restaurant information][FollowPostRedirectGetPattern1]

![Redirect user to Detail page][FollowPostRedirectGetPattern2]

---

### Building a Create Restaurant Page

A lot of our markup and some of our logic are duplicated between edit and create. They're trying to do the same job just from a different starting point. We will use the Edit page to both create a new restaurant, as well as edit an existing restaurant.

Add a button called 'Add New Restaurant' on the restaurant List page. This button will redirect user to the Editing page.

![Add New Restaurant button on List page][BuildACreateRestaurantPage1]

![Redirect to Edit page when user create new restaurant][BuildACreateRestaurantPage2]

---

### Adding Create to the Data Access Service

In our data source (OdeToFood.Data), we should create an Add() method (a service) to add a new Restaurant to our existing restaurant list.

Note: To simulate what a real database would do, we're going to generate an id for the new Restaurant here also.


---

### Handling Create and Update Logic

A way to tell if we should be updating a restaurant instead of adding a  restaurant is to look at the restaurant Id property. If the restaurant Id is 0, assuming our database will never use an Id value of 0, we could say any restaurant with an Id value less than 1 or equal to 0 must be a new restaurant that we need to add to the data source. Otherwise, if the restaurant has an Id that is greater than 0, then we need to update an existing restaurant.

![Enter new Restaurant information][HandlingCreateAndUpdateLogic1]

![Show newly added Restaurant on Detail page][HandlingCreateAndUpdateLogic2]

![Newly added Restaurant shown in Restaurant List page][HandlingCreateAndUpdateLogic3]

---

### Confirming the Last Operation

When we commit our restaurant changes and redirect user to the Detail page, we like the Detail page to show some sort of message like 'Restaurant saved."

So how can we pass along information from this Edit page to the Detail page when the Detail page is made using a separate http request?

Use the TempData data structure and Model Binding to accomplish this task.

![Edit a Restaurant][ConfirmingTheLastOperation1]

![Confirmed Message is displayed on Detail page][ConfirmingTheLastOperation2]

[Back to top](#top)<br><br>

## Working with SQL Server and the Entity Framework Core

### Installing the Entity Framework

Install 3 NuGet Entity Framework packages for OdeToFood.Data project:
1. Microsoft.EntityFrameworkCore
2. Microsoft.EntityFrameworkCore.SqlServer
3. Microsoft.EntityFrameworkCore.Design

Note 1: Choose the latest stable version that compatible with your project. For example, I installed 6.0.28 version for .NET 6.0.

Note 2: Check OdeToFood project's Dependencies to see if the Microsoft.AspNetCore.App NuGet package is installed. It is a metapackage that included all Entity Framework package. If you don't have it already installed, then you may need to install the same Enity Framework packages mentioned above in OdeToFood project as well.

### Implementing an Entity Framework DbContext

We know we want to store restaurant information, so let's go into the OdeToFood.Data project and add a new class called OdeToFoodDbContext. This class should inherit from an Entity Framework class named DbContext.

Add a public property of type DbSet of Restaurant since that is what we'd like to store in the database. This property will be called Restaurants.

### Using the Entity Framework Tools

The Entity Framework can change the schema of the database so that this database can store all information available for a restaurant. This is a feature of the Entity Framework known as **migrations**.

In order to create a migration, we need to use the dotnet commandline interface. However, the **dotnet ef** tools is no longer part of the .NET Core SDK. So we have to install it separatedly.

For .NET 6 projetct, run:

> <samp>dotnet tool install --globaldotnet-ef --version 6.*</samp>

Then we can use "dotnet-ef" command for the Entity Framework (EF) Core command-line tool.

	Credit to: https://stackoverflow.com/questions/57066856/command-dotnet-ef-not-found

Change directory to **OdeToFood.Data** project and run these commands for information about our DBContext (OdeToFoodDbContext):

> <samp>dotnet-ef dbcontext</samp>

> <samp>dotnet-ef dbcontext list</samp>

> <samp>dotnet-ef dbcontext info</samp>

### Adding Connection Strings and Registering Services

1. Adding **Connection String** to appsettings.json
2. Add **OdeToFoodDbContext service** to Program.cs using AddDbContextPool<> method from Entity Framework.
3. In OdeToFoodDbContext.cs, we need to provide a **constructor** so that the framework can pass in the connection string and the other options that this DbContext needs to know about to work with the database.

### Adding Database Migrations

Use dotnet-ef tool to:

1. Check information about the OdeToFoodDbContext

	> <samp>dotnet-ef dbcontext info -s \<startupProject\></samp>

2. Execute our first migration to create the database and Restaurants table

	> <samp>dotnet-ef migrations add \<migrationName\> -s \<startupProject\></samp>

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
[ModelBindingHttpPost1]: /GitImages/model-binding-http-post-1.png
[ModelBindingHttpPost2]: /GitImages/model-binding-http-post-2.png
[UsingModelStateAndShowingValidationErrors]: /GitImages/using-model-state-and-showing-validation-errors.png
[FollowPostRedirectGetPattern1]: /GitImages/follow-post-redirect-get-pattern-1.png
[FollowPostRedirectGetPattern2]: /GitImages/follow-post-redirect-get-pattern-2.png
[BuildACreateRestaurantPage1]: /GitImages/build-a-create-restaurant-page-1.png
[BuildACreateRestaurantPage2]: /GitImages/build-a-create-restaurant-page-2.png
[HandlingCreateAndUpdateLogic1]: /GitImages/handling-create-and-update-logic-1.png
[HandlingCreateAndUpdateLogic2]: /GitImages/handling-create-and-update-logic-2.png
[HandlingCreateAndUpdateLogic3]: /GitImages/handling-create-and-update-logic-3.png
[ConfirmingTheLastOperation1]: /GitImages/confirming-the-last-operation-1.jpg
[ConfirmingTheLastOperation2]: /GitImages/confirming-the-last-operation-2.jpg