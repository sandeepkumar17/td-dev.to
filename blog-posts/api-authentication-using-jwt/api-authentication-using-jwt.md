---
published: true
title: '.NET 8.0 - JWT Token Authentication Using The Example API'
cover_image: 'https://raw.githubusercontent.com/sandeepkumar17/td-dev.to/master/assets/blog-cover/dotnet-8-jwt.jpg'
description: 'Example of .NET 8.0 API using Clean Architecture to demonstrate the JWT Authentication mechanism.'
tags: dotnet, api, security, programming
series:
canonical_url:
---

## Understanding API Authentication Using JWT Bearer Tokens
In the modern landscape of web development, securing APIs is paramount. One of the most robust methods to achieve this is through API authentication using JWT (JSON Web Tokens) as Bearer tokens. This blog post will delve into the what, why, and how of JWT Bearer Token authentication.

## What is JWT?
JWT, or JSON Web Token, is an open standard (RFC 7519) for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA or ECDSA.

## Structure of a JWT
A JWT comprises three parts separated by dots (.): Header, Payload, and Signature.

1. **Header:** The header typically consists of two parts: the type of token (JWT) and the signing algorithm (e.g., HMAC SHA256 or RSA).
```
{
  "alg": "HS256",
  "typ": "JWT"
}
```
2. **Payload:** The payload contains the claims. Claims are statements about an entity (typically, the user) and additional data. There are three types of claims: registered, public, and private claims.
```
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```
3. **Signature:** To create the signature part, you have to take the encoded header, the encoded payload, a secret, and the algorithm specified in the header, and sign that.

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret
)
```

## Why Use JWT for API Authentication?
1. **Statelessness:** JWTs are stateless; the server doesn't need to store session information. This makes them scalable and reduces the load on the server.
2. **Security:** JWTs are signed so that the recipient can verify the token's authenticity.
3. **Compact:** JWTs are compact, making them efficient to send in HTTP headers.
4. **Interoperability:** As a JSON-based standard, JWTs are easy to use across different programming languages and platforms.

## How JWT Bearer Token Authentication Works
Here's a step-by-step explanation of how JWT Bearer Token authentication typically works:

1. **Client Login:** The client sends a login request with user credentials to the server.
2. **Server Verification:** The server verifies the credentials. The server creates a JWT with the user's information if they are correct.
3. **Token Issuance:** The server sends the JWT back to the client. This token is stored on the client side, usually in local storage or a cookie.
4. **Subsequent Requests:** For each subsequent request, the client includes the JWT in the Authorization header as a Bearer token.
```
Authorization: Bearer <token>
```
5. **Token Verification:** The server verifies the token's signature and checks the token's validity (expiration time, issuer, etc.). If valid, the server processes the request. If not, it returns an unauthorized error.

## JWT Implementation Example:
Let's walk through the JWT token implementation in .NET 8.0 API using the *Clean Architecture.*

### Solution and Project Setup:
First of all, set up DB and its objects, you can use the scripts shared under the `AuthDemo.Infrastructure/Sql` folder of the code sample.

Once our back end is ready, Open Visual Studio 2022 and setup the required projects using the Clean Architecture, if you want to learn more about the Clean Architecture implementation please [go through this article](https://dev.to/techiesdiary/net-60-clean-architecture-using-repository-pattern-and-dapper-with-logging-and-unit-testing-1nd9).

**Set Up Core Layer:** Under the solution, create a new Class Library project and name it `AuthDemo.Core`.
-	Add a new folder `Entities` and add a new entity class with the name `User`.

**Set Up Application Layer:** Add another Class Library Project and name it `AuthDemo.Application`.
- Add a reference to the `Core` project.
- Add a new folder `Interfaces` and create a new interface and name it as `IUserRepository`.
- Also, create a new interface, and name it `IUnitOfWork` to implement Unit of Work.

**Set Up Infrastructure Layer:** Add a new Class Library Project and name it `AuthDemo.Infrastructure`.
-	Add the required packages to be used in this project.
```
Install-Package Dapper
Install-Package Microsoft.Extensions.Configuration
Install-Package Microsoft.Extensions.DependencyInjection.Abstractions
Install-Package System.Data.SqlClient
```
-	Add the reference to projects (`Application`, and `Core`), and add a new folder `Repository`.
- After that let’s implement the `IUserRepository` interface, by creating a new class `UserRepository`.
- Also, implement the `IUnitOfWork` interface, by creating a new class `UnitOfWork`
-	Finally, register the interfaces with implementations to the .NET Core service container. Add a new class static `ServiceCollectionExtension` and add the RegisterServices method under it by injecting `IServiceCollection`.
-	Later, we will register this under the API’s ConfigureService method.

**Set up API Project:**  Add a new .NET 8.0 Web API project and name it `AuthDemoApi`.
-	Add the reference to projects (`Application`, and `Infrastructure`), and add the below packages.
```
Install-Package Swashbuckle.AspNetCore
Install-Package Microsoft.IdentityModel.Protocols
Install-Package System.IdentityModel.Tokens.Jwt
Install-Package Microsoft.IdentityModel.JsonWebTokens
Install-Package Microsoft.AspNetCore.Authentication.JwtBearer
```
-	Set up the `appsettings.json` file to manage the API settings and replace your DB connection string under the `ConnectionStrings` section.
```
"ConnectionStrings": {
  //Update values in the connection string.
  "DBConnection": "Data Source=localhost\\SQLEXPRESS; Initial Catalog=AuthDemoDB; Trusted_Connection=True;MultipleActiveResultSets=true"
}
```
- Add a secret key to verify and sign the JWT tokens.
```
"AppSettings": {
  //Replace it with your secret key to verify and sign the JWT tokens, It can be any string.
  "Secret": "8c8624e2-2afc-76a5-649e-9b9bf15cf6d3"
}
```
-	Configure Startup settings, such as RegisterServices (defined under the `AuthDemo.Infrastructure` project), and add the Swagger UI (with `Bearer` as the authentication scheme).

-	Remove the default controller/model classes and add two classes (`AuthenticateRequest` and `AuthenticateResponse`) under the Model folder, to handle API requests and responses.

- Add a `Helper` folder and add the below classes.
  - `AppSettings` - to map the the options from `appsettings.json` file.
  - `AuthorizeAttribute` - to validate the authorization.
  - `Common` - Add a GenerateJwtToken method to generate the JWT token.
  - `JwtMiddleware` - To validate the token and attach the user to context on successful Jwt validation.

- Add a new controller and name it `UsersController`.
  - Implement `Authenticate` API to validate the user and generate the token.
  - `GetAll` API to return all users, and add `Authorize` attribute to put it behind the API security.

**Review the final project structure:**

![Project Structure](./assets/auth_proj_01.png 'Project Structure')

## Run and Test API:
Run the project and test the API methods.

- Swagger UI
![Swagger UI](./assets/auth_01.png 'Swagger UI')

- Running API without authentication throws a `401 - Unauthorizeed` error.
![API Error](./assets/auth_02.png 'API Error')

- Authenticate the user and get a JWT token using the `Authenticate` API.
![Authenticate](./assets/auth_03.png 'Authenticate')

- Add API Authorization.
![API Authorization](./assets/auth_04.png 'API Authorization')

- **GET** - Get All users.
![API GET All](./assets/auth_05.png 'API GET All')

## NOTE:
Check the source code here.
{% github https://github.com/sandeepkumar17/AuthDemoApi %}

If you have any comments or suggestions, please leave them behind in the comments section below.
