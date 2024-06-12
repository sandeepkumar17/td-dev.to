---
published: true
title: '.NET 6.0 - Clean Architecture using Repository Pattern and Dapper with Logging and Unit Testing'
cover_image: 'https://raw.githubusercontent.com/sandeepkumar17/td-dev.to/master/assets/blog-cover/net-core.png'
description: 'Example of .NET6.0 CRUD API using Clean Architecture'
tags: csharp, api, dotnet, programming
series:
canonical_url:
---

In this article, we will learn about clean architecture and will walk you through a sample CRUD API in .NET 6.0.

We will use the following tools, technologies, and frameworks in this sample:
- [Visual Studio 2022](https://visualstudio.microsoft.com/vs/) and [.NET 6.0](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)
-	[C#](https://docs.microsoft.com/en-us/dotnet/csharp/)
-	MS SQL DB
-	[Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)
-	[Dapper (mini ORM)](https://github.com/DapperLib/Dapper)
-	Repository Pattern
-	Unit of Work
-	Swagger UI
-	API Authentication (Key Based)
-	[Logging (using log4net)](https://logging.apache.org/log4net/release/manual/configuration.html)
-	[Unit Testing (MSTest Project)](https://docs.microsoft.com/en-us/visualstudio/test/walkthrough-creating-and-running-unit-tests-for-managed-code)

**Before starting with the sample app, let us understand the clean architecture and its benefits.**

> The goal of software architecture is to minimize the human resources required to build and maintain the required system. ― Robert C. Martin, Clean Architecture

## Clean Architecture explained:
Clean Architecture is the system architecture guideline proposed by [Robert C. Martin also known as Uncle Bob](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html). It is derived from many architectural guidelines such as Hexagonal Architecture, Onion Architecture, etc.
-	The main concept of clean architecture is that the core logic of the application is changed rarely so it will be independent and considered core.
-	The overriding rule that makes this architecture work is The Dependency Rule. This rule says that source code dependencies can only point inwards and nothing in an inner circle can know anything at all about something in an outer circle.
-	By separating the software into layers, and conforming to The Dependency Rule, you will create an intrinsically testable system, with all the benefits that imply. When any of the external parts of the system become obsolete, like the database, or the web framework, you can replace those obsolete elements with a minimum of fuss.
-	In clean architecture, the domain and application layers remain in the center of the design which is known as the core of the application.
  -	The domain layer contains enterprise logic, and the application layer contains business logic.
  -	Enterprise logic can be shared across many related systems, but business logic is not sharable as it is designed for specific business needs.
  -	If you do not have an enterprise and are just writing a single application, then these entities are the business objects of the application.

**Advantages of clean architecture:**
-	Frameworks Independent - The architecture does not depend on the existence of some library of feature-laden software. This allows you to use such frameworks as tools.
-	UI Independent - It is loosely coupled with the UI layer. So, you can change the UI without changing the core business.
-	Independent of Database - You can swap out SQL Server or Oracle, for Mongo, Bigtable, CouchDB, or something else. Your business rules are not bound to the database.
-	Highly maintainable - It follows the separation of concern.
-	Highly Testable - Apps built using this approach, especially the core domain model and its business rules, are extremely testable.

So now we have an understanding of clean architecture. Before starting the sample API let us briefly review the Dapper.

## Dapper explained:
-	Dapper is a simple Object Mapper or a Micro-ORM responsible for mapping between database and programming language.
-	Dapper was created by the Stack Overflow team to address their issues and open-source it. Dapper used at Stack Overflow itself showcases its strength.
-	It drastically reduces the database access code and focuses on getting database tasks done instead of being full-on ORM.
-	It can be integrated with any database such as SQL Server, Oracle, SQLite, MySQL, PostgreSQL, etc.
-	If DB is already designed, then using Dapper is an optimal and efficient option.
-	Performance: Dapper is faster at querying data compared to the performance of the Entity Framework. This is because Dapper works directly with the RAW SQL and hence the time delay is relatively less.

Along with Dapper in this article, we will use Repository Pattern and Unit of Work and show you how Dapper can be used in an ASP.NET 6.0 API following Repository Pattern and Unit of Work.

## Solution and Project setup:
First of all, create a new table that’ll be used to perform the CRUD operation. You can use the scripts shared under the `CleanArch.Sql/Scripts` folder of the code sample.

Once our back end is ready, Open Visual Studio 2022 and create a blank solution project, and name it to `CleanArch`.

![CleanArch Solution](./assets/ca_01.png 'CleanArch Solution')

**Set Up Core Layer:** Under the solution, create a new Class Library project and name it `CleanArch.Core`.
![Core Layer](./assets/ca_02.png 'Core Layer')

•	Add a new folder `Entities` and add a new entity class with the name `Contact`.

{% gist https://gist.github.com/sandeepkumar17/25e8ebdff8ff5b22407ee4b7bf401f77 %}

> One thing to note down here is that the Core layer should not depend on any other Project or Layer. This is very important while working with Clean Architecture.

**Set Up Application Layer:** Add another Class Library Project and name it `CleanArch.Application`.

![CleanArch Application](./assets/ca_03.png 'CleanArch Application')

- Add a new folder `Application` and under this, we will define the interfaces that will be implemented at another layer.
-	Create a generic `IRepository` interface and define the CRUD methods.

{% gist https://gist.github.com/sandeepkumar17/e402961922e5a4f72e6d2b8a73302f63 %}

- Add a reference to the `Core` project, The Application project always depends only on the `Core` Project.
- After that add a Contact specific repository (`IContactRepository`), and inherit it from `IRepository`
- Also, create a new interface, and name it `IUnitOfWork` since we will be using Unit of Work in our implementation.

{% gist https://gist.github.com/sandeepkumar17/0e3ded0885783eaeb7c1f558ca87c0c9 %}

- As we are also implementing the logging, so add an `ILogger` interface and add methods for different log levels.

{% gist https://gist.github.com/sandeepkumar17/a0d3a5a402b2a550f8e42d4bcc75ce02 %}

**Set Up Logging:** Add a new Class Library Project (`CleanArch.Logging`)

![CleanArch Logging](./assets/ca_04.png 'CleanArch Logging')

-	We will be using the Log4Net library for logging, hence install the `log4net` package from the NuGet Package Manager.
-	Add a reference to the `Application` project and after that add a new class `Logger` and implement the `ILogger` interface.

{% gist https://gist.github.com/sandeepkumar17/10abb4e1dae0810119bc236aebb1c46d %}

**Set Up SQL Project:** Add a new Class Library Project (`CleanArch.Sql`). We’ll be using this project to manage the Dapper Queries.

![CleanArch SQL](./assets/ca_05.png 'CleanArch SQL')

-	Add a new folder `Queries` and add a new class under it `ContactQueries` (to manage dapper queries for the `Contact` object).

{% gist https://gist.github.com/sandeepkumar17/ec3760c7465bbe9d67f8d0d2004325c6 %}

-	Besides that, the `Scripts` folder is added that contains prerequisite scripts of the table used in the sample.

**Set Up Infrastructure Layer:** Since our base code is ready, now add a new Class Library Project and name it `CleanArch.Infrastructure`.

![CleanArch Infrastructure](./assets/ca_06.png 'CleanArch Infrastructure')

-	Add the required packages to be used in this project.
```
Install-Package Dapper
Install-Package Microsoft.Extensions.Configuration
Install-Package Microsoft.Extensions.DependencyInjection.Abstractions
Install-Package System.Data.SqlClient
```
-	Add the reference to projects (`Application`, `Core`, and `Sql`), and add a new folder `Repository`.
- After that let’s implement the `IContactRepository` interface, by creating a new class `ContactRepository` and injecting `IConfiguration` to get the connection string from `appsettings.json`

{% gist https://gist.github.com/sandeepkumar17/7f28c82e929cb86c7714042cd8fec3fb %}

- Also, implement the `IUnitOfWork` interface, by creating a new class `UnitOfWork`

{% gist https://gist.github.com/sandeepkumar17/de1f272ed82bc5b158f6783e112a4001 %}

-	Finally, register the interfaces with implementations to the .NET Core service container. Add a new class static `ServiceCollectionExtension` and add the RegisterServices method under it by injecting `IServiceCollection`.
-	Later, we will register this under the API’s ConfigureService method.

{% gist https://gist.github.com/sandeepkumar17/0b2ec69c2abd0facdf0e8009290c90a6 %}

**Set up API Project:**  Add a new .NET 6.0 Web API project and name it `CleanArch.Api`.

![CleanArch API 01](./assets/ca_07.png 'CleanArch API 01')
![CleanArch API 02](./assets/ca_08.png 'CleanArch API 02')

-	Add the reference to projects (`Application`, `Infrastructure`, and `Logging`), and add the `Swashbuckle.AspNetCore` package.
-	Set up the `appsettings.json` file to manage the API settings and replace your DB connection string under the `ConnectionStrings` section.

{% gist https://gist.github.com/sandeepkumar17/d566650743976e34f39b9533ff9fdc30 %}

- Add log4net.config and add logging-related settings under it. Make sure to set its `Copy to Output Directory` property to `Copy Always`.

{% gist https://gist.github.com/sandeepkumar17/06b49b1404314df6fecd50a1e1575b80 %}

-	Configure Startup settings, such as RegisterServices (defined under `CleanArch.Infrastructure` project), configure log4net and add the Swagger UI (with authentication scheme).

{% gist https://gist.github.com/sandeepkumar17/b80c3cfb6f3280f496614ddb5bf01fc0 %}

-	Remove the default controller/model classes and add a new class under Model (`ApiResponse`), to manage a generic response format for API responses.

{% gist https://gist.github.com/sandeepkumar17/57fb0c9713ad5fc5926cfcd184ac1df9 %}

-	Add a new controller and name it `AuthController`, to implement Not Authorized implementation since we will be using the key-based authentication.

{% gist https://gist.github.com/sandeepkumar17/ba31ded08bbe35bc74634b02722d3e52 %}

- Add `AuthorizationFilter`, as shown below to manage the API key-based authentication.
  - This allows authentication based on the main key and secondary keys.
  - We can add multiple secondary keys and we can turn on or off their usage from `appsettings`.
  - This will help to keep our main key safe and distribute the secondary keys to different clients on a need basis.

{% gist https://gist.github.com/sandeepkumar17/bf4c11acb3c7ff9ae013b5d34e40ac8b %}

-	Add a new controller and name it `BaseApiController`, this controller will contain the common implementation and will serve as a base controller for all other API controllers.

{% gist https://gist.github.com/sandeepkumar17/3fc9a6f071dfb7429a9eb99c06397595 %}

-	Finally, add a new API controller to expose the Contact API by injecting an object type of `IUnitOfWork` and adding all the CRUD operations.

{% gist https://gist.github.com/sandeepkumar17/71d0241ae96c043ea824e302ec6d34b8 %}

**Set up a Test Project:**  Add a new MSTest Test project and name it `CleanArch.Test` and add the below packages.

```
Install-Package Microsoft.Extensions.Configuration
Install-Package MSTest.TestFramework
Install-Package MSTest.TestAdapter
Install-Package Moq
```
![CleanArch Test](./assets/ca_09.png 'CleanArch Test')

- After that create a new class `ContactControllerShould` and set up all the possible test cases, review the code of the `CleanArch.Test` project for further understanding.

- Review the project structure in the solution explorer.
![Project Structure](./assets/ca_10.png 'Project Structure')

## Build and Run Test Cases:
- Build the solution and run the code coverage, this will run all the test cases and show you the test code coverage.
![Code Coverage](./assets/ca_11.png 'Code Coverage')

## Run and Test API:
Run the project and test all the CRUD API methods. (Make sure `CleanArch.Api` is set as a startup project)

- Swagger UI
![Swagger UI](./assets/ca_12.png 'Swagger UI')

- Running API without authentication throws an error.
![API Error](./assets/ca_13.png 'API Error')

- Add API Authorization.
![API Authorization](./assets/ca_14.png 'API Authorization')

- **POST** - Add new record.
![API POST](./assets/ca_15.png 'API POST')

- **GET** - Get All records.
![API GET All](./assets/ca_16.png 'API GET All')

- **PUT** - Update the existing record.
![API PUT](./assets/ca_17.png 'API PUT')

- **GET** - Get single record.
![API GET](./assets/ca_18.png 'API GET')

- **DELETE** - Delete the existing record.
![API Delete](./assets/ca_19.png 'API Delete')

## NOTE:
Check the entire [source code here](https://github.com/sandeepkumar17/CleanArch).

If you have any comments or suggestions, please leave them behind in the comments section below.
