
## How To Use EFCore?

- Create application of your choice 
- Install the database of your choice
- Install Database provider for the database
- Create Model and database Context Class
- Write Linq Queries To Intract With The DataBase
## Approaches to Work With EFCore

### Code First Approach
- We Create the domain Classes,later we create the database from our Code.

### Database First Approach
- We Design the databases,later we create classes.
#  Steps for Connecting .Net to Database

### ✅ **1. Install Required NuGet Package**

- To work With sql server we have to install Microsoft.EntityFrameworkCore.SqlServer.(This is Sql server Specific Package)
- ==**Microsoft.EntityFrameworkCore.SqlServer**==:-This is a database provider for sql Server database.
- The Database Provider is a framework or Library wich allows our application to be used with Microsoft sql server Databse.

![[image.jpeg]]
==Importent:- FOR EACH DATABASE DATABASE PROVIDER WILL BE DIFFERENT.==


Tools -> Nuget Package Manager ->Manage Nuget Package Solution ->Browse Core.Sql ->
Microsoft .Entity.FrameworkCore.SqlServer(2 Option)->Install
Then you can Check if it is Installed or Not in the installed Apps Section.

### ✅ **2. Define the Connection String**

A **connection string** tells EF how to connect to your SQL Server database.

**Example**: Add this to your `appsettings.json` file:
```csharp
 "ConnectionStrings": {
   "DefaultConnection": "Server=UIGLT228\\SQLSERVER2019;Database=Employees;User Id=sa;Password=Password123;TrustServerCertificate=True;MultipleActiveResultSets=true"
 },

```
`Server`: Your SQL Server instance name.
`Database`: Name of your database.
`Trusted_Connection=True`: Uses Windows Authentication.
`TrustServerCertificate=True`: Required for local dev environments (avoids SSL issues).

### ✅ **3. Create Your Entity Class**
This class represents a **table** in your database.
```Csharp
public class Employee
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

### ✅ **4. Create the `DbContext` Class**

- The **DbContext** is the main bridge between your application and the database.

- Entity Framework uses a DbContext class to represent your database and its tables. Create a new file named “AppDbContext.cs” in your project and define your DbContext class:


```Csharp

namespace WebApp.Demo.Models
{
    public class ApplicationDbContext: DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
      : base(options) { }

        public DbSet<Employee> Employees { get; set; }
        public DbSet<Role> Roles { get; set; }
        //how many Tables are there that much of Properties want to create...
    }
}
```

- The DbSet<> is used to represent the tables that your entities represent.
- 🔍 **Explanation:**

- `DbContext` is the **main class** EF uses to interact with the database.
    
- `DbSet<Employee>` means you are creating a **table named Employees** in the database.
    
- The constructor passes `options` (like the connection string) to the base class.

## YouTube Refferences About DB Context

### What Is Db Context ?
- Db Context is a Class wich is Used To Intaract with the Database.

### How To Create Db Context?
- To Create a Class That derived From DbContext.


  
  ### ✅ **5. Register DbContext in `Program.cs`**

- This connects the DbContext to the connection string during app startup.

```Csharp


// Add services to the container.
builder.Services.AddControllersWithViews();


 builder.Services.AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(builder.Configuration.GetConnectionString("DefaultConnection")));

```

This tells .NET: “Use SQL Server and this connection string when using `AppDbContext`.”

### ✅ **6. Apply Migrations and Create the Database**

Migrations generate the SQL scripts that EF will use to create your database tables.

```bash

CopyEdit

`dotnet ef migrations add InitialCreate dotnet ef database update`

✔️ This will create the database and table based on your `DbContext` and model.
```

# Or
### Manually Create DataBase And Tables .


### ✅ **7. Use the DbContext in Your Code (like in Controller or Service)**

 ### Create a constructor in EmployeeController and call your AppDbContext.
 
 ```Csharp
 public class EmployeeController : Controller
 {
     private readonly ApplicationDbContext _context;
     public EmployeeController(ApplicationDbContext context) {

        
		 

     }
     public IActionResult Index()
     {
         var employee = _context.Employees.ToList();
         return View(employee);
     }
 }
```

