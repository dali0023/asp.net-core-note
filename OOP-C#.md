## Method:
<Access Specifier> <Return Type> <Method Name>(Parameter List) {
   Method Body
}



**Method and variable are two types:**

**Static:**
it’s global, don’t need to create object. From same class we can use it directly, but we need to give permission to declare it public for other class.

**Non-Static:** we need to create object to access data.

## OOP
```c#
namespace ConsoleApp4
{
    class Abc
    {
        public static string city = "Dhaka"; // Static Variable
        public int phone = 999; // Non Static Variable
        public static void country()
        {
            Console.WriteLine("I am from Abc");
        }
        public int numbers()
        {
            return 80; // Non Static Method
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(Abc.city); // Static Variable doesn't need object instance
            Abc.country(); // Static Method doesn't need object instance

            Abc abc1 = new Abc();
            Console.WriteLine(abc1.phone); // object needs for non-static
            Console.WriteLine(abc1.numbers());
            Console.ReadLine();
        }
    }
}

```

#### Access Modifiers: 

**Public:** it visibles the data, means we can use it from different class by creating instance.

**Protected:** we can use it by inheritance.

**Private:** Normally We cannot access private data from other classes. But if we want, we have to access, we need to follow Encapsulation. 

**N: B: By Default A method or variable is Private.**
