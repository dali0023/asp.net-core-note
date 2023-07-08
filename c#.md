
```c#
class Hello {
  // main method
  static void Main(string[] args)
  {
    Console.WriteLine("Hello, world!");
  }
}
```
**Comments**
```c#
// Single-line comment
/* Multi-line 
   comment */
```

**Variables**

**Types and variables:**
There are two kinds of types in C#:
**Value types:**
**Simple types / primitive Data Types:** `short, int, char, float, double` etc. (They are called primitive because they 
are the main built-in types and could be used to build other data types), Enum types, Struct types, Nullable value types.
```c#
int myNum = 5; double myDouble = 5.25; float myFloatNum = 5.99f;
string myText = "Hello";char myLetter = 'D'; bool myBool = true;

// Constant:
const int myNum = 15;
myNum = 20; // error

// Declare Many Variables:
int x = 5, y = 6, z = 50;
Console.WriteLine(x + y + z);

// Local Variable/ Implicitly type variable: var
var collage = "Algonquin"; var year = 2020; var TuitionFees = 34670.50;
```

**Reference types:** `Class types, Interface types, Array types, Delegate types, Object`: You can assign values of any type to variables of type object.

```c#
Class Ref: Properties, Methods, Events, etc.
class Employee
   {
       public string Name;
       public string City;
   }

class Program
    {
        public string name;        
        // Reference Variable that has two veriable(City, Name) 
        
        static void Main(string[] args)
        {
            Program obj1 = new Program();
            obj1.name = "Algonquin College";
            Console.WriteLine(obj1.name);
 
            Employee obj2;
            obj2 = new Employee();
            obj2.eName = "Nazmul Hasan";
            Console.WriteLine(obj2.eName);
            Console.ReadLine();
        }
    }
```






