
```c#
class Hello {
  static void Main(string[] args)
  {
    int a = 5, b = 7;
    const int c = 10;
    var d = 12.2;
    string res = "result is: ";
    Console.WriteLine( res + (a + b + c + d)); // result is: 34.2
    Console.ReadLine();
  }
}
```

#### Types and variables:

**Simple types / primitive Data Types:** 
`short, int, char, float, double` etc. (They are called primitive because they 
are the main built-in types and could be used to build other data types), Enum types, Struct types, Nullable value types.
```c#
int myNum = 5; double myDouble = 5.25; float myFloatNum = 5.99f;
string myText = "Hello"; char myLetter = 'D'; bool myBool = true;

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

#### Type Casting

**Implicit Casting (automatically)** - converting a smaller type to a larger type size.
`char -> int -> long -> float -> double`
```c#
int myInt = 9;
double myDouble = myInt; // Automatic casting: int to double
```

**Explicit Casting (manually)** - converting a larger type to a smaller size type.
`double -> float -> long -> int -> char`
```c#
 double myDouble = 9.78;
 int myInt = (int)myDouble; // Manual casting: double to int

 Console.WriteLine(myDouble); // Outputs 9.78
 Console.WriteLine(myInt); // Outputs 9

 int math = 70; english = 11;
 double total = (double) math/english; // 6.3456788
 int total1 = (int) total; //6
```
#### Type Conversion Methods
```c#
int myInt = 10;
double myDouble = 5.25;
bool myBool = true;

Console.WriteLine(Convert.ToString(myInt)); // convert int to string
Console.WriteLine(Convert.ToDouble(myInt)); // convert int to double
Console.WriteLine(Convert.ToInt32(myDouble)); // convert double to int
Console.WriteLine(Convert.ToString(myBool)); // convert bool to string

int i = 75;
Console.WriteLine(i.ToString());


// TryParse: Convert String and return true or false
string myStr = "Dalim";
bool res = int.TryParse(myStr, out int x);
bool res = int.TryParse(myStr, out _);
Console.WriteLine(res);); // T or F

// Parse: Convert String:
int x = Int32.Parse(Console.ReadLine());
int y = 10;
Console.WriteLine(x + y);
```
#### Conditionals:
```c#
int j = 10;

if (j == 10) {
  Console.WriteLine("Yes");
} else if (j > 10) {
  Console.WriteLine("No");
} else {
  Console.WriteLine("Both");
}
```
**Short Hand If...Else:**
`variable = (condition) ? expressionTrue :  expressionFalse;`

```c#
int time = 20;
string result = (time < 18) ? "day." : "evening.";
Console.WriteLine(result);
```
**Switch Statements**
```c#
int day = 2;
switch (day)
  {
    case 1:
    Console.WriteLine("Saturday.");
        break;
    case 2:
    Console.WriteLine("Sunday.");
        break;
    default:
    Console.WriteLine("Friday”)
        break;
  }
```

#### Loop
```c#
int[] numbers = {1, 2, 3, 4, 5};

for(int i = 0; i < numbers.Length; i++) {
  Console.WriteLine(numbers[i]);
}

foreach(int num in numbers) {
  Console.WriteLine(num);
}
```
#### Arrays
```c#
char[] chars = new char[10];
chars[0] = 'a';
chars[1] = 'b';

string[] letters = {"A", "B", "C"};
int[] mylist = {100, 200};
bool[] answers = {true, false};
```

#### Collections
Why Collections, why not Array?
•	Insert, Delete, removing data from Any Position.
•	Array size is Fixed, but ArrayList is Auto-resizable.

**Collection are two types:**
1. Non-Generic collections: 
•	Auto resize-able but not Type Safe.
•	Ex: `Stack, Queue, LinkedList, SortedList, ArrayList, Hashtable`

```c#
using System.Collections;
ArrayList X = new ArrayList();

IList x = new ArrayList(){100, "Two", 12.5, 20}; // using IList Interface
ICollection x = new ArrayList(); // using ICollection Interface
IEnumerable x = new ArrayList(); // using IEnumerable Interface

X.Add(20);
x.Remove("Ottawa"); //removes specified value
X.Insert(1, 35); // Insert to specific position
x.RemoveAt(1); //Removes specified index. 
x.Clear(); // Removes All Elements

foreach (var item in x)
{
    Console.WriteLine(item);
}
```
2. Generic collections:
•	Auto resize-able and Also Type Safe.
•	Ex: `List`
```c#
List<int> a = new List<int>();
List<string> b = new List<string>();
List<double> c = new List<double>();
a.Add(70); b.Add("John Smith"); c.Add(7.90);
List1.AddRange(10, 20, 0, 40); // add more value
```
#### Add Custom Class Objects in List
```c#
List<int> x = new List<int>() { 80, 90, 100};
foreach (var item in x)
{
    Console.WriteLine(item);
}
```
```c#
class Program {
  public string Name;
  public int Age;
  public double Salary;
  static void Main(string[] args)
  {
    var MyList = new List<Program>();
    MyList.Add(new Program() { Name = "Nazmul", Age = 50, Salary = 3300 });
    MyList.Add(new Program { Name = "Julian", Age = 22, Salary = 3500 });
    MyList.Insert(1, new Program { Name = "Karon", Age = 23, Salary = 2500 });
    
    foreach (Program item in MyList)
    {
      Console.WriteLine($"My Name is: {item.Name}, Age: {item.Age}, and Salary: {item.Salary}");
    }
  }
}
```
```c#
// Add elements using object initializer syntax:
IList<Program> studentList = new List<Program>() {
    new Program(){ StudentID=1, StudentName="Bill"},
    new Program(){ StudentID=2, StudentName="Steve"},
    new Program(){ StudentID=3, StudentName="Ram"},
    new Program(){ StudentID=1, StudentName="Moin"}
};
foreach (Program item1 in studentList)
{
    Console.WriteLine(item1.StudentID +" "+ item1.StudentName);
}
```




























