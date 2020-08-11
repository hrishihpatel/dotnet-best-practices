# Best Practice Guide for DotNet / C#
Code is easy, anyone can write code. However, writing maintainable code is a crucial part of engineering. In this post, I will talk about some of the best practices that pertain to dotnet community and ways to keep your code simple and maintainable to design standards set forth by the dotnet framework.

# C# Coding Standards and Naming Conventions


| Object Name               | Notation   | Length | Plural | Prefix | Suffix | Abbreviation | Char Mask          | Underscores |
|:--------------------------|:-----------|-------:|:-------|:-------|:-------|:-------------|:-------------------|:------------|
| Class name                | PascalCase |    128 | No     | No     | Yes    | No           | [A-z][0-9]         | No          |
| Constructor name          | PascalCase |    128 | No     | No     | Yes    | No           | [A-z][0-9]         | No          |
| Method name               | PascalCase |    128 | Yes    | No     | No     | No           | [A-z][0-9]         | No          |
| Method arguments          | camelCase  |    128 | Yes    | No     | No     | Yes          | [A-z][0-9]         | No          |
| Local variables           | camelCase  |     50 | Yes    | No     | No     | Yes          | [A-z][0-9]         | No          |
| Constants name            | PascalCase |     50 | No     | No     | No     | No           | [A-z][0-9]         | No          |
| Field name                | camelCase  |     50 | Yes    | No     | No     | Yes          | [A-z][0-9]         | Yes         |
| Properties name           | PascalCase |     50 | Yes    | No     | No     | Yes          | [A-z][0-9]         | No          |
| Delegate name             | PascalCase |    128 | No     | No     | Yes    | Yes          | [A-z]              | No          |
| Enum type name            | PascalCase |    128 | Yes    | No     | No     | No           | [A-z]              | No          |

***Why: consistent with the Microsoft's .NET Framework and easy to read.***

#### 1. Do use PascalCasing for class names and method names:

```csharp
public class ClientActivity
{
  public void ClearStatistics()
  {
    //...
  }
  public void CalculateStatistics()
  {
    //...
  }
}
```

#### 2. Do use camelCasing for method arguments and local variables:

```csharp
public class UserLog
{
  public void Add(LogEvent logEvent)
  {
    int itemCount = logEvent.Items.Count;
    // ...
  }
}
```

#### 3. Do not use Hungarian notation or any other type identification in identifiers

```csharp
// Correct
int counter;
string name;    
// Avoid
int iCounter;
string strName;
```

***Why: consistent with the Microsoft's .NET Framework and Visual Studio IDE makes determining types very easy (via tooltips). In general you want to avoid type indicators in any identifier.***

#### 4. Do not use Screaming Caps for constants or readonly variables:

```csharp
// Correct
public const string ShippingType = "DropShip";
// Avoid
public const string SHIPPINGTYPE = "DropShip";
```


#### 5. Use meaningful names for variables. The following example uses seattleCustomers for customers who are located in Seattle:

```csharp
var seattleCustomers = from customer in customers
  where customer.City == "Seattle" 
  select customer.Name;
```

#### 6. Avoid using Abbreviations. Exceptions: abbreviations commonly used as names, such as Id, Xml, Ftp, Uri.

```csharp    
// Correct
UserGroup userGroup;
Assignment employeeAssignment;     
// Avoid
UserGroup usrGrp;
Assignment empAssignment; 
// Exceptions
CustomerId customerId;
XmlDocument xmlDocument;
FtpHelper ftpHelper;
UriPart uriPart;
```

#### 7. Do use camelCasing for abbreviations 3 characters or more (2 chars are both uppercase):

```csharp  
HtmlHelper htmlHelper;
FtpTransfer ftpTransfer;
UIControl uiControl;
```

***Why: consistent with the Microsoft's .NET Framework. Caps would grab visually too much attention.***

#### 8. Do not use Underscores in identifiers. Exception: you can prefix private fields with an underscore:

```csharp 
// Correct
public DateTime clientAppointment;
public TimeSpan timeLeft;    
// Avoid
public DateTime client_Appointment;
public TimeSpan time_Left; 
// Exception (Class field)
private DateTime _registrationDate;
```


#### 9. Do use predefined type names (C# aliases) like `int`, `float`, `string` for local, parameter and member declarations. Do use .NET Framework names like `Int32`, `Single`, `String` when accessing the type's static members like `Int32.TryParse` or `String.Join`.

```csharp
// Correct
string firstName;
int lastIndex;
bool isSaved;
string commaSeparatedNames = String.Join(", ", names);
int index = Int32.Parse(input);
// Avoid
String firstName;
Int32 lastIndex;
Boolean isSaved;
string commaSeparatedNames = string.Join(", ", names);
int index = int.Parse(input);
```

#### 10. Do use implicit type var for local variable declarations. Exception: primitive types (int, string, double, etc) use predefined names. 

```csharp 
var stream = File.Create(path);
var customers = new Dictionary();
// Exceptions
int index = 100;
string timeSheet;
bool isCompleted;
```

#### 11. Do use noun or noun phrases to name a class. 

```csharp 
public class Employee
{
}
public class BusinessLocation
{
}
public class DocumentCollection
{
}
```

#### 12. Do prefix interfaces with the letter I. Interface names are noun (phrases) or adjectives.

```csharp     
public interface IShape
{
}
public interface IShapeCollection
{
}
public interface IGroupable
{
}
```

***Why: consistent with the Microsoft's .NET Framework.***

#### 13. Do name source files according to their main classes. Exception: file names with partial classes reflect their source or purpose, e.g. designer, generated, etc. 

```csharp 
// Located in Task.cs
public partial class Task
{
}
// Located in Task.generated.cs
public partial class Task
{
}
```

#### 14. Do organize namespaces with a clearly defined structure: 

```csharp 
// Examples
namespace Company.Product.Module.SubModule
{
}
namespace Product.Module.Component
{
}
namespace Product.Layer.Module.Group
{
}
```

#### 15. Do vertically align curly brackets: 

```csharp 
// Correct
class Program
{
  static void Main(string[] args)
  {
    //...
  }
}
```

#### 16. Do declare all member variables at the top of a class, with static variables at the very top.

```csharp 
// Correct
public class Account
{
  public static string BankName;
  public static decimal Reserves;      
  public string Number { get; set; }
  public DateTime DateOpened { get; set; }
  public DateTime DateClosed { get; set; }
  public decimal Balance { get; set; }     
  // Constructor
  public Account()
  {
    // ...
  }
}
```

#### 17. Do use singular names for enums. Exception: bit field enums.

```csharp 
// Correct
public enum Color
{
  Red,
  Green,
  Blue,
  Yellow,
  Magenta,
  Cyan
} 
// Exception
[Flags]
public enum Dockings
{
  None = 0,
  Top = 1,
  Right = 2, 
  Bottom = 4,
  Left = 8
}
```

#### 18. Do not explicitly specify a type of an enum or values of enums (except bit fields):

```csharp 
// Don't
public enum Direction : long
{
  North = 1,
  East = 2,
  South = 3,
  West = 4
} 
// Correct
public enum Direction
{
  North,
  East,
  South,
  West
}
```

#### 19. Do not use an "Enum" suffix in enum type names:

```csharp     
// Don't
public enum CoinEnum
{
  Penny,
  Nickel,
  Dime,
  Quarter,
  Dollar
} 
// Correct
public enum Coin
{
  Penny,
  Nickel,
  Dime,
  Quarter,
  Dollar
}
```
#### 20. Do not use "Flag" or "Flags" suffixes in enum type names:

```csharp 
// Don't
[Flags]
public enum DockingsFlags
{
  None = 0,
  Top = 1,
  Right = 2, 
  Bottom = 4,
  Left = 8
}
// Correct
[Flags]
public enum Dockings
{
  None = 0,
  Top = 1,
  Right = 2, 
  Bottom = 4,
  Left = 8
}
```
#### 21. Do use suffix EventArgs at creation of the new classes comprising the information on event:

```csharp 
// Correct
public class BarcodeReadEventArgs : System.EventArgs
{
}
```

#### 22. Do name event handlers (delegates used as types of events) with the "EventHandler" suffix, as shown in the following example:

```csharp 
public delegate void ReadBarcodeEventHandler(object sender, ReadBarcodeEventArgs e);
```

#### 23. Do not create names of parameters in methods (or constructors) which differ only by the register:

```csharp 
// Avoid
private void MyFunction(string name, string Name)
{
  //...
}
```

#### 24. DO use two parameters named sender and e in event handlers. The sender parameter represents the object that raised the event. The sender parameter is typically of type object, even if it is possible to employ a more specific type.

```csharp
public void ReadBarcodeEventHandler(object sender, ReadBarcodeEventArgs e)
{
  //...
}
```

#### 25. Do use suffix Exception at creation of the new classes comprising the information on exception:

```csharp 
// Correct
public class BarcodeReadException : System.Exception
{
}
```

#### 26. Do use prefix Any, Is, Have or similar keywords for boolean identifier :

```csharp 
// Correct
public static bool IsNullOrEmpty(string value) {
    return (value == null || value.Length == 0);
}
```

# Optimize Syntax
In this section, we will look at some of the basic things that you should utalize to make your code much cleaner.

#### 1. User of ExpressionBody

```csharp
//Avoid  
public ActionResult Dashboard()  
{  
    return View();  
}  
//Do  
public ActionResult Dashboard() => View();
```

#### 2. null or empty conditions

```csharp

//Avoid  
var varName = "faisal";  
if (varName != null && varName != "")  
{  
   //code  
}  
//Do  
var varName = "faisal";  
if (!string.IsNullOrEmpty(varName))  
{  
    //code  
}
```
#### 3. null coalescing expression

```csharp
Test test = new Test();  
//Avoid  
var varName = test.Name != null ? test.Name : "";  
//Do  
var varName = test.Name ?? "";
```

#### 4. object initializer

```csharp
//Avoid  
Test test = new Test();  
test.Id = 1;  
test.Name = "faisal";  
//Do  
var test = new Test  
{  
   Id = 1,  
   Name = "faisal"  
};
```
#### 5. use the ?. operator:

```csharp
//Avoid  
var empName = "";  
Session["Name"] = "Faisal Pathan";  
if (Session["Name"] != null)  
{  
   empName = Session["Name"].ToString();  
}  
else  
{  
     empName = "";  
}  
//Do  
var empName = "";  
Session["Name"] = "Faisal Pathan";  
empName = Session["Name"]?.ToString() ?? "";
```

#### 6. string interpolation

```csharp
Test test = new Test();  
//Avoid  
 var details = string.Format("{0}, you are welcome, Your Id is {1}", test.Name , test.Id + "_emp");  
//Do  
var details = $"{test.Name}, you are welcome, Your Id is {test.Id}_emp";
```

# Async AWait
Asynchronous programming is a means of parallel programming in which a unit of work runs separately from the main application thread and notifies the calling thread of its completion, failure or progress…

The main benefits one can gain from using asynchronous programming are improved application performance and responsiveness. One particularly well suited application for the asynchronous pattern is providing a responsive UI in a client application while running a computationally or resource expensive operation.

The .NET Framework provides a few avenues to get on the ramp to asynchronous programming. Some of your implementation choices from the most basic to complex include using background workers, invoking a method asynchronously from a delegate, or implementing the IAsynchResult interface. All of these options allow you to multi-thread your application without ever having to manage your own threads. The .NET Framework asynchronous APIs handle this drudgery for you.

# Variables

#### 1. Avoid nesting too deeply and return early
Too many if else statements can make the code hard to follow. **Explicit is better than implicit**.

**Bad:**

```csharp
public bool IsShopOpen(string day)
{
    if (!string.IsNullOrEmpty(day))
    {
        day = day.ToLower();
        if (day == "friday")
        {
            return true;
        }
        else if (day == "saturday")
        {
            return true;
        }
        else if (day == "sunday")
        {
            return true;
        }
        else
        {
            return false;
        }
    }
    else
    {
        return false;
    }

}
```

**Good:**

```csharp
public bool IsShopOpen(string day)
{
    if (string.IsNullOrEmpty(day))
    {
        return false;
    }

    var openingDays = new[] { "friday", "saturday", "sunday" };
    return openingDays.Any(d => d == day.ToLower());
}
```
#### 2. Avoid mental mapping
Don’t force the reader of your code to translate what the variable means. **Explicit is better than implicit**.

**Bad:**

```csharp
var l = new[] { "Austin", "New York", "San Francisco" };

for (var i = 0; i < l.Count(); i++)
{
    var li = l[i];
    DoStuff();
    DoSomeOtherStuff();

    // ...
    // ...
    // ...
    // Wait, what is `li` for again?
    Dispatch(li);
}
```

**Good:**

```csharp
var locations = new[] { "Austin", "New York", "San Francisco" };

foreach (var location in locations)
{
    DoStuff();
    DoSomeOtherStuff();

    // ...
    // ...
    // ...
    Dispatch(location);
}
```

#### 3. Avoid magic string
Magic strings are string values that are specified directly within application code that have an impact on the application’s behavior. Frequently, such strings will end up being duplicated within the system, and since they cannot automatically be updated using refactoring tools, they become a common source of bugs when changes are made to some strings but not others.

**Bad**

```csharp
if (userRole == "Admin")
{
    // logic in here
}
```

**Good**

```csharp
const string ADMIN_ROLE = "Admin"
if (userRole == ADMIN_ROLE)
{
    // logic in here
}
```

Using this we only have to change in centralize place and others will adapt it.

#### 4. Don't add unneeded context
If your class/object name tells you something, don't repeat that in your variable name.

**Bad:**

```csharp
public class Car
{
    public string CarMake { get; set; }
    public string CarModel { get; set; }
    public string CarColor { get; set; }

    //...
}
```

**Good:**

```csharp
public class Car
{
    public string Make { get; set; }
    public string Model { get; set; }
    public string Color { get; set; }

    //...
}
```

# Classes

#### 1. Use method chaining
This pattern is very useful and commonly used in many libraries. It allows your code to be expressive, and less verbose.
For that reason, use method chaining and take a look at how clean your code will be.

**Good:**

```csharp
public static class ListExtensions
{
    public static List<T> FluentAdd<T>(this List<T> list, T item)
    {
        list.Add(item);
        return list;
    }

    public static List<T> FluentClear<T>(this List<T> list)
    {
        list.Clear();
        return list;
    }

    public static List<T> FluentForEach<T>(this List<T> list, Action<T> action)
    {
        list.ForEach(action);
        return list;
    }

    public static List<T> FluentInsert<T>(this List<T> list, int index, T item)
    {
        list.Insert(index, item);
        return list;
    }

    public static List<T> FluentRemoveAt<T>(this List<T> list, int index)
    {
        list.RemoveAt(index);
        return list;
    }

    public static List<T> FluentReverse<T>(this List<T> list)
    {
        list.Reverse();
        return list;
    }
}

internal static void ListFluentExtensions()
{
    var list = new List<int>() { 1, 2, 3, 4, 5 }
        .FluentAdd(1)
        .FluentInsert(0, 0)
        .FluentRemoveAt(1)
        .FluentReverse()
        .FluentForEach(value => value.WriteLine())
        .FluentClear();
}
```

#### 2. Prefer composition over inheritance
As stated famously in [_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns) by the Gang of Four,
you should prefer composition over inheritance where you can. There are lots of good reasons to use inheritance and lots of good reasons to use composition.

The main point for this maxim is that if your mind instinctively goes for inheritance, try to think if composition could model your problem better. In some cases it can.

You might be wondering then, "when should I use inheritance?" It
depends on your problem at hand, but this is a decent list of when inheritance makes more sense than composition:

1. Your inheritance represents an "is-a" relationship and not a "has-a" relationship (Human->Animal vs. User->UserDetails).
2. You can reuse code from the base classes (Humans can move like all animals).
3. You want to make global changes to derived classes by changing a base class (Change the caloric expenditure of all animals when they move).

**Bad:**

```csharp
class Employee
{
    private string Name { get; set; }
    private string Email { get; set; }

    public Employee(string name, string email)
    {
        Name = name;
        Email = email;
    }

    // ...
}

// Bad because Employees "have" tax data.
// EmployeeTaxData is not a type of Employee

class EmployeeTaxData : Employee
{
    private string Name { get; }
    private string Email { get; }

    public EmployeeTaxData(string name, string email, string ssn, string salary)
    {
         // ...
    }

    // ...
}
```

**Good:**

```csharp
class EmployeeTaxData
{
    public string Ssn { get; }
    public string Salary { get; }

    public EmployeeTaxData(string ssn, string salary)
    {
        Ssn = ssn;
        Salary = salary;
    }

    // ...
}

class Employee
{
    public string Name { get; }
    public string Email { get; }
    public EmployeeTaxData TaxData { get; }

    public Employee(string name, string email)
    {
        Name = name;
        Email = email;
    }

    public void SetTax(string ssn, double salary)
    {
        TaxData = new EmployeeTaxData(ssn, salary);
    }

    // ...
}
```


## Offical Reference

1. [MSDN General Naming Conventions](http://msdn.microsoft.com/en-us/library/ms229045(v=vs.110).aspx)
2. [DoFactory C# Coding Standards and Naming Conventions](http://www.dofactory.com/reference/csharp-coding-standards) 
3. [MSDN Naming Guidelines](http://msdn.microsoft.com/en-us/library/xzf533w0%28v=vs.71%29.aspx)
4. [MSDN Framework Design Guidelines](http://msdn.microsoft.com/en-us/library/ms229042.aspx)
