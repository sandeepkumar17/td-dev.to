---
published: true
title: '.NET 6.0 - gRPC Server and Client implementation'
cover_image: 'https://raw.githubusercontent.com/sandeepkumar17/td-dev.to/master/assets/blog-cover/gprc-dot-net-core.png'
description: 'Example of 6 gRPC implementation using .NET Core 6.0'
tags: grpc, csharp, dotnet, programming
series:
canonical_url:
---

In this article, we will learn about the basics of gRPC, its usage, and finally, implement the Server and Client using .NET Core 6.0.

## gRPC explained:

gRPC (Google Remote Procedure Calls) was initially created by Google, which has used a single general-purpose RPC infrastructure called Stubby to connect the large number of microservices running within and across its data centers for over a decade.
In March 2015, Google decided to build the next version of Stubby and make it open source and the result was gRPC.

- gRPC is a modern open-source high-performance Remote Procedure Call (RPC) framework.
- It implements APIs using HTTP/2 which can run in any environment
- gRPC has two parts
  - **gRPC Protocol** - As mentioned above it uses HTTP/2 which provides a lot of advantages over traditional Http1.x
  - **Data Serialization** - by default gRPC uses Protobuf for the serialization and as an intermediator between client and server.
- gRPC clients and servers intercommunicate using a variety of environments and machines.
- It supports many languages like Java, C#, Go, Ruby, and Python, check [the full list here](https://grpc.io/docs/languages/)
- It supports pluggable auth, tracing, load balancing, and health checking.

#### Different scenarios in which we use gRPC
- When we use microservice architecture and we use that for internal communication from one or more servers.
- gRPC is handy when performance is on high priority with low latency.
- It is useful when we require duplex communication between services with different types of data.
- gRPC is useful in the last mile of distributed computing to connect devices, mobile applications, and browsers to backend services.

## gRPC vs REST:
  
**REST** has been one of the highly used API frameworks and with gRPC grabbing the limelight, let's see the comparison on various parameters.
 
| |REST|gRPC|
|:-----|:-----|:-----|
|Protocol|HTTP/1.1|HTTP/2.0|
|Request-Response Model|Only supports Request Response as based on HTTP/1.1|Supports All Types Of Streaming as based on HTTP/2|
|Serialization|JSON/XML|protobuf|
|Payload Format|Serialization like JSON/XML|Strong Typing|
|Browser Support|Yes|No|
 
## Pros and Cons of gRPC:

**Pros**
- With duplex communication, it supports bi-directional streaming with HTTP/2-based transport
- Performance is one of the talking points of gRPC and it is faster than REST and SOAP.
- gRPC services are highly efficient on wire and with a simple service definition framework.
- gRPC messages are lightweight compared to other types such as JSON.
- Client libraries supporting the 11 languages

**Cons**
- Browser support is very limited.
- Since it uses binary data which is not easily readable compared to JSON or XML.


## gRPC Implementation

### Prerequisites
- Visual Studio 2022
- .NET 6.0

### Set Up gRPC Service:
- Open Visual Studio 2022 and create a new gRPC project with the name **GrpcCoreService** and select `.NET 6.0` under the Framework option.
![Core Service Setup](./assets/gprc_01.png 'Core Service Setup')
![Core Service Setup](./assets/gprc_02.png 'Core Service Setup')
![Core Service Setup](./assets/gprc_03.png 'Core Service Setup')
- Review the default project folder structure.
  - **Protos:** contains all the gRPC server asset files, such as `greet.proto`
  - **Services:** Contains the implementation of the gRPC services.
  - **Root Folder:** `appSettings.json` contains service the configuration and `Program.cs` contains code that configures app behavior.
![Folder Structure](./assets/gprc_04.png 'Folder Structure')
 
- Right-click on the `greet.proto` and click on Properties and verify that gRPC Stub Classes is set to **Server only**.
![Proto_Prop](./assets/gprc_05.png 'Proto Properties')
 
### Set Up Client Application:
- Add a Console App Project with the name **GrpcClientApp** and select the required configurations.
![Client Setup](./assets/gprc_06.png 'Client Setup')
![Client Setup](./assets/gprc_07.png 'Client Setup')
![Client Setup](./assets/gprc_08.png 'Client Setup')
- Add the required packages to the client app project.
  ```
  Install-Package Grpc.Net.Client
  Install-Package Google.Protobuf
  Install-Package Grpc.Tools
  ```
- Create a **Protos** folder and copy the `Protos\greet.proto` file from the **GrpcCoreService** under this folder.
- Update the namespace inside the `greet.proto` file to the project's namespace:
  ```
  option csharp_namespace = "GrpcClientApp";
  ```
- After that right click on `greet.proto` and click on **Properties** and set the gRPC Stub Classes to **Client only**.
![Proto_Client_Prop](./assets/gprc_09.png 'Proto Client Properties')

- Finally, edit the **GrpcClientApp.csproj** project file:
  ```
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
  </ItemGroup>
  ```
- Once this is completed update the `Program.cs` file to call the greeter service.
 
{% gist https://gist.github.com/sandeepkumar17/6d3d840380b8e545ede3c978e9de76d8 %}

> Make sure to use the port number mentioned in the `launchSettings.json` files of the **GrpcCoreService** project.
  
### Run and Test:
- **Set Up StartUp Project:** Configure both projects as startup projects in the proper order.
![Set Up StartUp Project](./assets/gprc_10.png 'Set Up StartUp Project')
- Run the project and review the output of both the gRPC service and the client in the Console window.
- gRPC Service Window:
![Service Output](./assets/gprc_13.png 'Service Output')
- gRPC Client Window:
![Client Output](./assets/gprc_11.png 'Client Output')

### Setup new service and client:
So far we have tested the default greeter service and client. 

Now let's move ahead and set up a new service and client which consumes this new service.

#### Setup employee service:
- To start with add the `employee.proto` file under the `Protos` folder and add the following code.

{% gist https://gist.github.com/sandeepkumar17/04d0e0e464885a10b38681bae65512ac %}

- Under the `Services` folder add the `EmployeeService.cs` and implement it as shown below.

{% gist https://gist.github.com/sandeepkumar17/d149eedde66cdc115b3c71cf4c6e8e4e %}

- Inject this new service under the `Program.cs`.

  ```
  app.MapGrpcService<EmployeeService>();
  ```

#### Update the client app to consume the employee service:
- Copy the `employee.proto` file from the **GrpcCoreService** and update the namespace:

  ```
  option csharp_namespace = "GrpcClientApp";
  ```
- After that right click on the `employee.proto` and click on **Properties** and set the gRPC Stub Classes to **Client only**
- And make sure the proto file is added in the **GrpcClientApp.csproj** project file:

  ```
  <ItemGroup>
    <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
    <Protobuf Include="Protos\employee.proto" GrpcServices="Client" />
  </ItemGroup>
  ```
- Finally, update the `Program.cs` file to call the `employee` service.

{% gist https://gist.github.com/sandeepkumar17/d149eedde66cdc115b3c71cf4c6e8e4e %}
 
- Once you are done with changes, you can verify the folders and files under **Solution Explorer**.
![Project Structure](./assets/gprc_14.png 'Project Structure')

### Run and Test the new service:
- Run the project and search for the employee by passing the Employee ID in the Console Window
![Employee Output 01](./assets/gprc_12.png 'Employee Output 01')
![Employee Output 02](./assets/gprc_15.png 'Employee Output 02')

## NOTE:
Check the entire [source code here](https://github.com/sandeepkumar17/GrpcCoreService).

If you have any comments or suggestions, please leave them behind in the comments section below and If you've found a typo, a sentence that could be improved or anything else that should be updated on this blog post, please go directly to the [blog repository](https://github.com/sandeepkumar17/td-dev.to) and open a new pull request with your changes.
 
