# APT.NET-Core-Web-API-using-SQL-Server
In this article, we will figure out how to make Web API project with ASP.NET Core(v6.0)We will begin with realizing what a Web API and `EF migrations` is, next, we will make another Web API project in Visual Studio and afterward we will consume the API from a .NET client. All actions was made in  Visual Studio 2022 on Window 11 machine with installed SQL Server.

### What is Web API?
A Web API is an application programming interface for either a web server or a web browser. It is a web development concept, usually limited to a web application's client-side (including any web frameworks being used), and thus usually does not include web server or browser implementation details such as SAPIs or APIs unless publicly accessible by a remote web application

### What is Entity Framework?
Entity Framework is an Object Relational Mapper (ORM) which is a type of tool that simplifies mapping between objects in your software to the tables and columns of a relational database.

## EF Core in ASP.NET Core Web API with Sql Server

This guide explains setting up a production-ready ASP.NET Core Web API using Entity Framework with SQL Server. Our Web API can perform basic REST operations.

### In this article, you will learn how to
- Create a simple ASP.NET Core Web API which do REST Operations using Entity Framework(with SQL Server)
- `EF migrations` with SQL 
- Run and interact with it

### Required Tools
* ASP.NET Core
* Virtual Studio 2019
* MySQL Workbench
* Postman
