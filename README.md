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
