# ASP.NET-Core-Web-API-using-SQL-Server
In this article, we will figure out how to make Web App(MVC) project with ASP.NET Core(v6.0)We will begin with realizing what a Web App(MVC) and Entity Framewor is, next, we will make another Web API project in Visual Studio and afterward we will consume the API from a .NET client. All actions was made in  Visual Studio 2022 on Window 11 machine with installed SQL Server.

### What is Web App(MVC)?
A Web App is an application programming interface a web browser. It is a web development concept, usually limited to a web application's client-side (including any web frameworks being used). MVC (Model-View-Controller) is an application building architecture that separates the user interface, application data, and business logic. This architecture consists of three concepts: model, view, and controller. Explore more about [ASP.NET MVC](https://learn.microsoft.com/en-us/aspnet/mvc/overview/older-versions-1/overview/asp-net-mvc-overview)

### What is Entity Framework?
Entity Framework is an Object Relational Mapper (ORM) which is a type of tool that simplifies mapping between objects in your software to the tables and columns of a relational database.

## EF Core in ASP.NET Core Web API with Sql Server

This guide explains setting up a production-ready ASP.NET Core Web API using Entity Framework with SQL Server. Our Web API can perform basic REST operations.

### In this article, you will learn how to
- Create a simple ASP.NET Core Web API which do REST Operations using Entity Framework(with SQL Server)
- Entity Framework with SQL 
- Run and interact with it

### Required Tools
* ASP.NET Core
* Visual Studio 2022
* SQL Server Express 2019

## Step 1: SQL Server Installation 
Following the steps to install SQL Server in your windows 11

### Download SQL Server Express
1. Go to this [link](https://www.microsoft.com/en-us/sql-server/sql-server-downloads).
2. Scroll down until you find the Express edition of SQL Server.
3. Click Download now to start the download.

### Run the Installation
Once the download is complete, open the download folder and find the installation file. Run the file to start the installation process.

### Choose Installation Type
1. After starting the install process, you can choose between three installation types.
2. For this project, we are using the Custom installation type. Click the center tile to choose this option.
3. Specify the install location and click Install to start downloading the setup files.
4. Select the `New SQL Server stand-alone installation or add features to an existing installation` option to start the install process.
5. The following screen gives you an overview of the SQL Express Server license terms. Check the box next to `I accept the license terms and Privacy Statement` and click `Next` to continue.
6. Check the box next to`Use Microsoft Update to check for updates` to include updates to SQL Server 2019 in the scheduled Windows updates. Click `Next` to continue.
7. The `Install Rules` screen helps identify potential problems with the installation. Any entries showing a Failed status must be resolved before you proceed with the installation. If there are no failed entries, click `Next` to continue.
8. On the `Feature Selection` screen, check the boxes in the `Features` section to choose which elements of SQL Server 2019 to install, and define the install directories. Click `Next` to continue.
9. The `Instance Configuration` screen lets you choose between the default and custom instance names. For this tutorial, we are using the `Named instance` option and keeping the default suggested names. Click `Next` to continue.
10. The following screen lets you install Java with the current installation or specify a path if you already have it installed. Click `Next` to continue.
11. The `Database Engine Configuration` screen lets you specify the authentication mode for your SQL server. For this tutorial, we are using the `Mixed Mode` option and adding the current user as an administrator. Click `Next` to continue.
12. The next two screens require you to consent to installing Microsoft R Open and Python, respectively. Click `Accept` and `Next` on both to continue.
13. Once the installation is complete, the new screen displays an overview of the installed features. Click `Close` to finish the installation.

### Test Connection to SQL Server Express
1. In the login window, choose the SQL Server Authentication option and use the default Login (sa) and the password you set up during the SQL Server 2019 setup.
2. Click `Connect` to try to connect to the server.
If the login window closes without any issues and you have access to the SQL Server Management Studio main window, this means the connection works properly.

## Step 2: Create a Database
1. In Object Explorer, connect to an instance of the SQL Server Database Engine and then expand that instance.
2. Right-click Databases, and then select New Database.
3. In New Database, enter a database name.
4. To create the database by accepting all default values, select `OK`.

## Step 3: Create ASP.NET Core Web App(MVC)
1. Run Visual Studio and Get started menu > Create a New Project.
2. Search and select the ASP.NET Web App(Model-View-Controller) and clik `Next`.
3. Name the project `Freelancers` and click `Create`.

## Step 4: Adding Dependencies in ASP.NET Core
Before we start our project need a few dependencies. We will add them all by NuGet Package Manager.

The list of packages is below:
- Microsoft.EntityFrameworkCore.SqlServer

## Step 5: Creating Model and Database Context
Firstly create folder named Model, then create

``freelancers.cs``
```C#
using System.ComponentModel.DataAnnotations;
using System.ComponentModel.DataAnnotations.Schema;
namespace Freelancers.Models
{
    public class freelancers
    {
        [Key]
        [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
        public long Id { get; set; }
        
        [Required]
        [Column(TypeName = "nvarchar(100)")]
        public string username { get; set; }
        
        [Required]
        [Column(TypeName = "nvarchar(100)")]
        public string email { get; set; }
        
        [Required]
        [Column(TypeName = "varchar(11)")]
        public string phoneNumber { get; set; }

        [Required]
        [Column(TypeName = "nvarchar(100)")]
        public string skillsets { get; set; }

        [Required]
        [Column(TypeName = "nvarchar(100)")]
        public string hobby { get; set; }

    }
}
```

Then Create DBContext file named ``ApiContext``

```C#
using Microsoft.EntityFrameworkCore;
using Freelancers.Models;

namespace Freelancers.Data
{
    public class ApiContext : DbContext
    {
        public DbSet<freelancers> freelancers { get; set; }
        public ApiContext(DbContextOptions<ApiContext> options)
            : base(options)
        {

        }
    }
}
```

## Step 6: Setup the Application Connection
To setup the connection with server, this project used `appsetins.json` file.

```C#
"ConnectionStrings": {
    "DefaultConnection": "DATA SOURCE=LENOVO-01\\SQLEXPRESS;DATABASE=FreelancersDb;UID=sa;PWD=password;TrustServerCertificate=true"
  }
```

Import `Entity Framework` and `Model` in `Program.cs` file

```C#
using Microsoft.EntityFrameworkCore;
using Freelancers.Data;
```

Then add service on `Program.cs` file

```C#
// Add services to the container.
builder.Services.AddControllersWithViews();
builder.Services.AddDbContext<ApiContext>
    (opt => opt.UseSqlServer("name=ConnectionStrings:DefaultConnection"));

builder.Services.AddControllers();
```

Then create `Controller` Class. 

## Step 7: HomeController
Import Model and Data in `HomeConroller.cs` file

```C#
using Freelancers.Models;
using Freelancers.Data;
```

Set the DbContext in `HomeController.cs` file

```C#
private readonly ApiContext _context;

        public HomeController(ApiContext context)
        {
            _context = context;
        }
```

Edit the `Index()` to return the list of freelances

```C# 
// Get all
        [HttpGet()]
        public IActionResult Index()
        {
            return View(_context.freelancers.ToList());
        }
```

Create IActionResult for `Create.cshtml` view.

```C#
public IActionResult Create()
        {
            return View();
        }
```

Then add the HttpPost into controller

```C#
//Create
        [HttpPost]
        public IActionResult CreateFreelance(freelancers list)
        {
            var ID = _context.freelancers.Find(list.Id);
            if (ID == null)
                _context.freelancers.Add(list);
            _context.SaveChanges();
            return RedirectToAction(nameof(Index));
        }
```

Continue with HttpDelete

```C#
//Delete
[Route("/Home/Delete")]
        [HttpDelete]
        public IActionResult Delete(freelancers list)
        {
            var result = _context.freelancers.FirstOrDefault(e => e.Id == list.Id);
            if (result == null)
                return new JsonResult(NotFound());
            else
                 _context.freelancers.Remove(result);
            _context.SaveChanges();
            return new JsonResult(Ok(result));
        }
```

Lastly, HttpPut

```C#
//Edit
[Route("/Home/Edit")]
        [HttpPut]
        public JsonResult Edit(freelancers list)
        {
            var freelancerInDb = _context.freelancers.Find(list.Id);

            if (freelancerInDb == null)
                return new JsonResult(NotFound());

            freelancers updatedFreelance = freelancerInDb;
            updatedFreelance.username = list.username;
            updatedFreelance.email = list.email;
            updatedFreelance.phoneNumber = list.phoneNumber;
            updatedFreelance.skillsets = list.skillsets;
            updatedFreelance.hobby = list.hobby;

            _context.SaveChanges();

            return new JsonResult(GetAll());

        }
```

GetAll() that redirect to index page view

```C#
// Get all
        [HttpGet()]
        public IActionResult GetAll()
        {
            return RedirectToAction(nameof(Index));
        }
```

Now, design UI that can link between controller and model using JSON. 
Click this [link](https://www.youtube.com/watch?v=U5FvZyWJfHs) to see how this project work
