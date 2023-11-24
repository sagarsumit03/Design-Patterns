
# Java

**Java is a Compiled + Interpreted Language. First the Code is compiled into bytecode resulting in .class file. After this the code is interpreted/executed at runtime**

## JAVA Versions

  1.5:
  
  Generics
  AutoBoxing/Unboxing
  Static import
  Annotaions

1.6:          

JDBC 4.0 API

1.7:          

Strings in switch statement
Automatic Resource Management in try/catch:
`try (Scanner sc = new Scanner(System.in)){...}`

1.8:          

Lambda expressions
Stream API
JAVA Annotation Type:
`@NotNull String s`
PermGen removal from GC

1.9:      

HTTP 2.0 client
JShell

10: 

Local-Variable Type Inference:
`var s = "Sumit"` //Infers from the assigned value that variable s is a String type.

11:

Strings and Files got a couple new methods:

```java
"Marco".isBlank();
"Mar\nco".lines();
"Marco ".strip();
```

13:

Switch Statement: Switch expressions can now return a value. And you can use a lambda-style syntax for your expressions, without the fall-through/break issues.

Old switch statements looked like this:

```java
switch(status) { 
	case SUBSCRIBER: 
			// code block 
			break; 
	case FREE_TRIAL:
			// code block 
			break;
	default:
			// code block
	}
```

Whereas with Java 13, switch statements can look like this:

```java
boolean result = switch (status) {
	case SUBSCRIBER -> true;
	case FREE_TRIAL -> false; 
	default -> throw new IllegalArgumentException("something is murky!");
}
```

AND:
Multiline Strings (Preview). You can finally do this in Java:

```java
String htmlBeforeJava13 = "<html>\\n" +
													" <body>\\n" +
													" <p>Hello, world</p>\\n" +
													" </body>\\n" +
													"</html>\\n";
String htmlWithJava13 = """ 
												<html>
														<body> 
																<p>Hello, world</p>
														</body> 
												</html> 
												""";
```

14:

Switch statement became standard.

Helpful NullPointerExceptions Finally NullPointerExceptions describe exactly which variable was null. 

```java
author.age = 35;
--- Exception in thread "main" java.lang.NullPointerException: Cannot assign field "age" because "author" is null
```

Text-Blocks / Multiline Strings Introduced as an experimental feature in Java 13 (see above), multiline strings are now production-ready.

AND Sealed Classes - Preview:

If you ever wanted to have an even closer grip on who is allowed to subclass your classes, there’s now the sealed feature. 

```java
public abstract sealed class Shape permits Circle, Rectangle, Square {...}
```

This means that while the class is public, the only classes allowed to subclass Shape are Circle, Rectangle and Square.

---

### The **compiler is not backwards compatible** because bytecode generated with Java5 JDK won't run in Java 1.4 jvm (unless compiled with the -target 1.4 flag). But **the JVM is backwards compatible**, as it can run older bytecodes.

In brief, we can say:

> JDK's are (usually) forward compatible.
JRE's are (usually) backward compatible.
> 

---

JDK:
Java Development Kit is the core component of Java Environment and provides all the tools, executables and binaries required to compile, debug and execute a Java Program. JDK is a platform-specific software and that’s why we have separate installers for Windows Mac, and Unix system

JVM:
When we run a program, JVM is responsible for converting Byte code to the machine specific code. JVM is also platform dependent and provides core java functions like memory management, garbage collection, security etc

`JVM is called virtual because it provides an interface that does not depend on the underlying operating system and machine hardware. This independence from hardware and the operating system is what makes java program write-once-run-anywhere.`

JRE:
JRE consists of JVM and java binaries and other classes to execute any program successfully. JRE doesn’t contain any development tools like java compiler, debugger etc.

![https://beginnersbook.com/wp-content/uploads/2013/05/jdk.jpg](https://beginnersbook.com/wp-content/uploads/2013/05/jdk.jpg)

---

GC works in two simple steps known as Mark and Sweep:

Mark – it is where the garbage collector identifies which pieces of memory are in use and which are not
Sweep – this step removes objects identified during the “mark” phase.

Java 8 PermGen and Metaspace
PermGen is replaced with Metaspace in Oracle/Sun JDK8, which is very similar. The main difference is that Metaspace can expand at runtime.

**Parallel/Throughput GC**: This is JVM’s default collector in JDK 8.

---

### Annotation:

The first main distinction between kinds of annotation is whether **they're used at compile time and then discarded** (like @Override) or placed in the compiled class file and **available at runtime** (like Spring's @Component)
When compiling code with annotations, the compiler sees the annotation just like it sees other modifiers on source elements, like access modifiers (public/private) or final. When it encounters an annotation, it runs an annotation processor, which is like a plug-in class that says it's interested a specific annotation. The annotation processor generally uses the Reflection API to inspect the elements being compiled and may simply run checks on them, modify them, or generate new code to be compiled. @Override is an example of the first; it uses the Reflection API to make sure it can find a match for the method signature in one of the superclasses and uses the Messager to cause a compile error if it can't.

CREATING AN ANNOTATION:
1. declare it using the @interface keyword:`public @interface JsonSerializable { }`
2. add meta-annotations to specify the scope and the target:

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.Type)
public @interface JsonSerializable {
}

```

### Garbage Collection:

![Java%20125629a036d342ae886cf232be523260/Untitled.png](Java%20125629a036d342ae886cf232be523260/Untitled.png)

1. **Young** Generation
    
    **New objects are allocated in Young generation.**
    
    **Young** Generation consists of >
    
    - 1a) **Eden**,
    - 1b) **S0 (Survivor** space 0**)**
    - 1c) **S1 (Survivor** space 1**)**
    
    Minor garbage collection occurs in Young Generation. Some of the objects which aren't cleaned up survive in young generation and gets aged. Eventually such objects are moved from young to old generation.
    
2. The **Old Generation** is used for storing the long surviving aged objects (Some of the objects which aren't cleaned up **survive in young generation and gets aged**.  Eventually such objects are **moved from young to old generation**).
    
    ***Major garbage collection** occurs in* **Old Generation.**
    
    All the non-daemon threads running in application are stopped during major garbage collections.
    
    [Daemon threads](http://www.javamadesoeasy.com/2015/03/daemon-threads-12-salient-features-of.html) performs major garbage collection.
    

3. Permanent generation Space contains metadata required by JVM to describe the classes and methods used in the application. The permanent generation space is populated at runtime by JVM based on classes in use in the application.

The permanent generation is included in a full garbage collection in java.

PermGen is the memory area in Heap that is used by the JVM to store class and method objects
if there are many classes that are getting added in permGen,
it would throw

```java
java.lang.OutOfMemoryError: PermGen space
```

Metaspace is NOT part of Heap. Rather Metaspace is part of Native Memory (process memory) which is only limited by the Host Operating System.

![http://karunsubramanian.com/wp-content/uploads/2014/07/Java8-heap.jpg](http://karunsubramanian.com/wp-content/uploads/2014/07/Java8-heap.jpg)

While you will NOT run out of PermGen space anymore (since there is NO PermGen), you may consume excessive Native memory making the total process size large. The issue is, if your application loads lots of classes (and/or interned strings), you may actually bring down the Entire Server.
`So its important to put flag for metaspace size in config`

Heap is divided into sections which allows easy allocation and cleanup. Eden Space is the place where new objects are allocated. When Eden is about to reach its limit, there is a Minor GC which clears up objects available for GC from Eden space to one of the Survivor spaces (S0 and S1). Also the objects available for GC in one of the survivor spaces are cleared and remaining objects move to the other survivor space. So at a time there is always one survivor space which is empty. After n (depends on JVM implementation) such iteration objects which have survived are moved to the old memory. Whenever old memory is on the verge of being a full Major GC occurs and unused resources of old memory are cleared.

- For any given object, `finalize()` will be called only once (at most) by the garbage collector. Also, the garbage collector is not guaranteed to run at any specific time.
- The `java.lang.Object.finalize()` is called by the garbage collector on an object when garbage collection determines that there are no more references to the object. A subclass overrides the finalize method to dispose of system resources or to perform other cleanup.
    
    > `finalize()` is deprecated.
    > 
    
    ```java
    Car car= new Car();
    car.finalize();
    ```
    
- if overridding `finalize()` it is good programming practice to use a try-catch-finally statement and to always call super.finalize(). This is a safety measure to ensure you do not inadvertently miss closing a resource used by the objects calling class
    
    ```java
    protected void finalize() throws Throwable {
        try {
            close();        // close open files
        } finally {
            super.finalize();
        }
    }
    ```
    
## Heap vs Stack

Each Java virtual machine thread has a private Java virtual machine stack, created at the same time as the thread.
**local variables and methods are on the Java stack**.
**All Objects are created in HEAP and the reference is in the stack.**
It Causes StackOverflow Error.

**String pool is created in now ( java 8 ) heap memory instead of permgen. to store string literals: `String s = "abc"`**

**STRING POOL Doesn't have String Literals**

String pools are hash table containing the reference to the String object residing in heap in this case `"abc"` 

The Java virtual machine has a heap that is shared among all Java virtual machine threads. The heap is the runtime data area from which **memory for all class instances and arrays is allocated**
The memory leaks will be in the heap. It Causes OutOfMemoryError. 

---

### JVM Architechture

![Java%20125629a036d342ae886cf232be523260/Untitled%201.png](Java%20125629a036d342ae886cf232be523260/Untitled%201.png)

1. Class Loader Subsystem: Classloader is used to load class files.
2. Runtime Data Areas: 
    1. Method Area: Method area stores data for each and every class like fields,constant pool,method’s data and information.
    2. Heap: Heap is place where all objects are stored in JVM.
    3. Java Thread: each and every thread has its own stack.
    4. Program counter Registers: the address of instructions currently being executed and address of next instruction as well.
3. Execution Engine: 
    1. JIT compiler: JIT compiler compiles bytecodes to machine code at run time and improves the performance of Java applications.
    2. Garbage Collector Garbage collection is the process by which JVM clears objects (unused objects) from heap to reclaim heap space.
4. Native method interface is an interface that connects JVM with the native method libraries for executing native methods.

## Object Orientation Design

### SOLID:

 1. **Single Responsibility Principle**: 
 A class should have one reason to change. 
	 ```java
	 public class Invoice{
	     public void AddInvoice(){ 
	         ...
	     }
	     public void DeleteInvoice(){ 
	         ...
	     }
	     public void GenerateReport(){ 
	         //this function should be moved to different class
	    }
	    public void EmailReport() { 
	         //this function should be moved to different class
	    }
	}
	``` 
 3. **Open Closed Principle**: 
 _Entities should be open for extension, but closed for modification._
 you can create classes and extend them (by creating a subclass), but you shouldnt  modify the original class. Like adding a new field is fine, but not modifying existing fields. 
 
 4. **Liskov Substitution Principle:** 
 A class that expects an object of type animal should work if a subclass dog and a subclass cat are passed. Also Animal should not have a bark function. As cats cant bark.
			 **Bad example**--

	```java
	public class Bird{
	    public void fly(){}
	}
	public class Duck extends Bird{}
     ```
     The duck can fly because it is a bird, but what about this:
	```java
	public class Ostrich extends Bird{}
	```
	Ostrich is a bird, but it can't fly, Ostrich class is a subtype of class Bird, but it shouldn't be able to use the fly method, that means we are breaking the LSP principle.
**Good example**

	```java
	public class Bird{}
	public class FlyingBirds extends Bird{
	    public void fly(){}
	}
	public class Duck extends FlyingBirds{}
	public class Ostrich extends Bird{} 
	```
 5. **Interface Separation**: 
 An **interface** shouldn't **force** a **class** to implement methods that it won't be using.
 Keep your interfaces the smallest you can. A teacher that also is a student should implement both the IStudent and ITeacher interfaces, instead of a single big interface called IStudentAndTeacher.
 
 6. **Dependency Inversion (NOT dependency Injection)** :  
Imagine you have a car and your different components are:
	```java
		1.  Client: You as the person driving the car.
		2.  High-Level Modules: The steering wheel and the gas/brake peddles.
		3.  Low-Level Modules: Engine
	``` 
	**Abstractions don't depend on details.**
	For me, it doesn't matter whether my engine has changed or not, I still should be able to drive my car the same way.
	**Details should depend upon abstractions.**
	I would not want an engine that causes the brake to double the speed. Meaning a engine should do what it says drive the car.
	```java
		interface DatabaseInterface {
		    public function get();
		    public function insert();
		    public function update();
		    public function delete();
		}
		class MySQLDatabase implements DatabaseInterface {
		    //all implemented functions get(), insert() ....
		    }
		}

		class MongoDB implements DatabaseInterface {
		     //all implemented functions get(), insert() ....
		}

		class BudgetReport {
		    private DatabaseInterface database;

		    public BudgetReport(DatabaseInterface database)
		    {
		        this.database = database;
		    }

		    public function open(){
		        this.database.get();
		    }

		    public function save(){
		        this.database.insert();
		    }
		    ...
		}

		// Main Function: Client
		DatabaseInterface mysql = new MySQLDatabase();
		report_mysql = new BudgetReport(mysql);

		report_mysql.open();

		DatabaseInterface mongo = new MongoDB();
		report_mongo = new BudgetReport(mongo);

		report_mongo.open();
	```
	Now here BudgetReport doesnt care about the underlying implementation whether its mySQL or MongoDB.  This also works with Open-Close Principle, as we can add a new database class that implements the `DatabaseInterface` and we dont need to change BudgetReport class.

	Another Example: 

	```java
	interface Logger {
	   public void write(String message);
	}
	 
	class FileLogger implements Logger {
	   public void write(String message) {
	       // write to file
	   }
	}
	 
	class StandardOutLogger implements Logger {
	   public void write(String message) {
	       // write to standard out
	   }
	}
	 
	public void doStuff(Logger logger) {
	   // do stuff
	   logger.write("some message")
	}

	```
	If you’re writing code that needs a logger, you don’t want to limit yourself to writing to files, because you don’t care. You just call the  `write`  method and let the concrete class sort it out.

### Abstraction

abstraction is achieved by interfaces and abstract classes . Interfaces allows you to abstract the implemetation completely while abstract classes allow partial abstraction as well.

### Encapsulation

Make members of a class as private.
Define public setter and getter methods to modify and view the variables' values and access them outside the class only through getters and setters

### Polymorphism

1.Compile-time polymorphism/static binding/early binding/overloading. 

2.Run-time polymorphism /dynamic binding/late binding/overriding.

### Inheritance

Inheritance is declared using the extends keyword.
Inheritance is an "is-a" relationship. Composition is a "has-a".

### Association

The association relationship indicates that a class knows about, and holds a reference to, another class. Associations can be described as a "has-a" relationship because the typical implementation in Java is through the use of an instance field.
Association in Java:

1. Two separate classes are associated through their objects.
2. The two classes are unrelated, each can exist without the other one.
3. Can be a one-to-one, one-to-many, many-to-one, or many-to-many relationship.

Aggregation is a narrower kind of association. It occurs when there’s a one-way (HAS-A) relationship between the two classes you associate through their objects. **For example, every Passenger has a Car but a Car doesn’t necessarily have a Passenger.**

Aggregation in Java:

1. One-directional association.
2. Represents a HAS-A relationship between two classes.
3. Only one class is dependent on the other.

Composition
Compositionis a stricter form of aggregation. It occurs when the two classes you associate are mutually dependent on each other and can’t exist without each other. For Example, Car has an Engine. here either Car or Engine cannot
exist without each other.
Composition in Java:

1. A restricted form of aggregation
2. Represents a PART-OF relationship between two classes
3. Both classes are dependent on each other
4. If one class ceases to exist, the other can’t survive alone.

### Java is always pass-by-value. Unfortunately, when we pass the value of an object, we are passing the reference to it.

---

- Null is not a valid object instance, so there is no memory allocated for it. It is simply a value that indicates that the object reference is not currently referring to an object.
- Each Java application uses an independent JVM.
Each JVM is a separate process, and that means there is no sharing of stacks, heaps, etcetera. (Generally, the only things that might be shared are the read only segments that hold the code of the core JVM and native libraries ... in the same way that normal processes might share code segments.
Garbage collection operates on each JVM independently.

---

## Reflection

Java Reflection is the process of analyzing and modifying all the capabilities of a class at runtime. Reflection API in Java is used to manipulate class and its members which include fields, methods, constructor, etc. at runtime.

Say you have an object of an unknown type in Java, and you would like to call a 'doSomething' method on it if one exists. Java's static typing system isn't really designed to support this unless the object conforms to a known interface, but using reflection, your code can look at the object and find out if it has a method called 'doSomething' and then call it if you want to.
So, to give you a code example of this in Java (imagine the object in question is foo) :

```java
Method method = foo.getClass().getMethod("doSomething", null);
method.invoke(foo, null);   //The null indicates there are
```

no parameters being passed to the foo method

*One very common use case in Java is the usage with annotations. JUnit 4, for example, will use reflection to look through your classes for methods tagged with the @Test annotation, and will then call them when running the unit test.*

```java
Class.forName(property.getProperty("DB_DRIVER_CLASS"));
```

---

## Cloneable Interface

- It is used to indicate that a class allows a bitwise copy of an object (that is, a clone) to be made. If you try to call clone( ) on a class that does not implement Cloneable, a CloneNotSupportedException is thrown. When a clone is made, the constructor for the object being cloned is not called.
- clone repeatedly up the chain until you have cloned an object, you have a shallow copy of the object. The clone generally shares state with the object being cloned. If that state is mutable, *you don't have two independent objects. If you modify one, the other changes as well*. And all of a sudden, you get random behavior.

### Marker interfaces

Marker Interfaces in java are interfaces with no members declared in them. They are just an empty interfaces used to mark or identify a special operation. For example, Cloneable interface is used to mark cloning operation and Serializable interface is used to mark serialization and deserialization of an object. **Now annotations are preferred as they don't propagate to sub-classes.**

### Inner interface

Inner interface is also called nested interface, which means declare an interface inside of another interface. Map.Entry is an nested Interface.

```java
public interface Map {
    interface Entry{
        int getKey();
    }
    void clear();
}
```

## CLASS

- Default access: a class with default access can be seen only by classes within the same package
- Public access: all classes in the Java Universe (JU) have access to a public class. The class can now be instantiated from all other classes, and any class is now free to subclass (extend from) it—unless, that is, the class is also marked with the nonaccess modifier final.
- Marking a class as `strictfp` means that any method code in the class will conform to the IEEE 754 standard rules for floating points. Without that modifier,floating points used in the methods might behave in a platform-dependent way.
- Final Classes When used in a class declaration, the final keyword means the class can't be subclassed. In other words, no other class can ever extend (inherit from) a final class, and any attempts to do so will give you a compiler error.`Can't subclass final classes.`
- Abstract Classes An abstract class can never be instantiated. Its sole purpose, mission in life, raison d'être, is to be extended (subclassed).
i.e. can’t do:`Car car = new Car();`

`AnotherClass.java:7: class Car is an abstract class.It can't be instantiated.`

You can, however,put nonabstract methods in an abstract class.

- A superclass can’t be instantiated because there can’t be any constructor in it.
- But we can instatntiate it using it as inner class in subclass.

	```java
	abstract class My {
	    public void myMethod() {
	        System.out.print("Abstract");
	    }
	}
	class Poly extends My {
	    public static void main(String a[]) {
	        My m = new My() {};
	        m.myMethod();
	    }
	}
	```

- *You can't mark a class as both abstract and final. They have nearly opposite meanings. An abstract class must be subclassed, whereas a final class must not be subclassed. If you see this combination of abstract and final modifiers, used for a class or method declaration, the code will not compile.*
    
    *modifier static not allowed here*
    
    ```java
    public class HelloWorld{
        static class Hello{}
        class InnerClass {}
    // arguments are passed using the text field below this editor
        public static void main(String[] args)
        {
            HelloWorld helloWorld = new HelloWorld();
            HelloWorld.Hello h= new  HelloWorld.Hello();
            HelloWorld.InnerClass i = helloWorld.new Hello();
        }
    }
    
    ```
    

---

### STATIC KEYWORD:

In Java, static keyword is mainly used for memory management. It can be used with variables, methods, blocks and nested classes. It is a keyword which is used to share the same variable or method of a given class. `Basically, static is used for a constant variable or a method that is same for every instance of a class.`

### Static Block

To initialize your **static variables**, you can declare a static block that gets executed exactly once, when the class is first loaded.

```java
public class BlockExample{
		// static variable
		static int j = 10;
		static int n;
		 
		// static block
		static {
		System.out.println("Static block initialized.");
		n = j * 8;
		}
}
```

### Static variables

Are, essentially, global variables. Basically, all the instances of the class share the same static variable.

### Static Method:

Methods declared as static can have the following restrictions:

- They can directly call other static methods only.
- They can access static data directly.

> Static Methods Can't access non-static ( instance) variables. as they are not related to instance of a class. but instance functions can call static variable.
> 

### Static Class:

Java has no way of making a top-level class static but you can simulate a static class like this:

- Declare your class `final` - Prevents extension of the class since extending a static class makes no sense
- Make the constructor `private` - Prevents instantiation by client code as it makes no sense to instantiate a static class
- Make **all** the members and functions of the class `static` - Since the class cannot be instantiated no instance methods can be called or instance fields accessed
- Note that the compiler will not prevent you from declaring an instance (non-static) member. The issue will only show up if you attempt to call the instance member.

> What good are static classes? A **good use of a static class** is in defining one-off, **utility and/or library classes** where instantiation would not make sense. A great **example is the Math class** that contains some mathematical constants such as PI and E and simply provides mathematical calculations. Requiring instantiation in such a case would be unnecessary and confusing. Notice that it is final and all of its members are static. If Java allowed top-level classes to be declared static then the Math class would indeed be static.

## Interface

- All interface methods are implicitly public and abstract. In other words, you do not need to actually type the public or abstract modifiers in the method declaration, but the method is still always public and abstract.
- Because interface methods are abstract, they cannot be marked final.
- All variables defined in an interface must be public, static, and final—in other words, interfaces can declare only constants, not instance variables.
- interfaces are implicitly abstract whether you type abstract or not.declarations are legal, and functionally identical:
    
    ```java
    public abstract interface Rollable { }
    public interface Rollable { }
    ```
    
- An interface can extend one or more other interfaces.
- An interface cannot extend anything but another interface.
- An interface cannot implement another interface or class.
- If we have a method in Interface with signature void doStuff(); we can’t implement the interface and have a method as void doStuff() in the subclass, as we see all the methods in interface are abstract and PUBLIC, but as soon as we implement it in class the void doStuff() in class has a DEFAULT access, hence the implementation has narrowed the access, and it will throw error.
    
    ```java
    Attempting to assign weaker privilege; was public
    ```
    
- For a subclass, if a member of its superclass is declared public, the subclass inherits that member regardless of whether both classes are in the same package.
- When a member is declared private, a subclass can't inherit it.
- You can, however, declare a matching method in the subclass. But regardless of how it looks, it is not an overriding method! It is simply a method that happens to have the same name as a private method (which you're not supposed to know about) in the superclass.
- The subclass can see the protected member only through inheritance. NO DOT OPERATOR.
- Once the subclass-outside-the-package inherits the protected member, that member (as inherited by the subclass) becomes private to any code outside the subclass,
i.e. if the SuperClass is A in package com.sumit.A and Subclass B is in package com.sumit.B
B can Access A’s member variable protected int x=0; only using inheritance that is B extends A. not like
    
    ```java
    public class B extends A{
        A a = new A();
        System.out.println(a.x); //This will not compile.
    }
    ```
    

	```java
	In Addition if a class  C under same package as B, com.sumit.B , will not be able to access “x” as in the class B the inherited variable x acts as private. So if C does

	public class C{ //same package as B
	B b = new B();
	System.out.println(b.x); //will throw exception.
	}
	```

- any local variable declared with an access modifier will not compile.
[https://javaranch.com/campfire/StoryCups.jsp](https://javaranch.com/campfire/StoryCups.jsp)

### Default Interface

Default Methods allowed java to have multiple Inheritance.
but if we have 2 interface with same method we will get compiler error,
if we haven't implemented it in child class.

*we can have static method in interfaces as well. This helps it becoming a utility
interface. AND BY DEFAULT YOU CAN'T OVERRIDE STATIC METHOD*

```java
interface I1 {
    default void m() {
        System.out.println("in I1");
    }
}

interface I2 {
    default void m() {
        System.out.print("in I2");
    }
    static void hello(){
        System.out.print("I am static method");
    }
}

public class HelloWorld implements I1, I2 {

    @Override
    public void m() {
        I1.super.m();
        System.out.print("in m()");
    }

    public static void main(String[] args) {
        HelloWorld h = new HelloWorld();
        h.m();
    }
}
// RETURNS in I1 in m();
```

---

## Contructor

- Constructors can't be marked static (they are after all associated with object instantiation), they can't be marked final or abstract (because they can't be overridden).
- local variable must be initialized before you try to use it. The compiler will reject any code that tries to use a local variable that hasn't been assigned a value, because—unlike instance variables—local variables don't get default values.
- If you mark an instance variable as `transient`, you're telling the JVM to skip (ignore) this variable when you attempt to serialize the object containing it.
- Attempting to access an instance variable from a static context (typically from main():
    
    ```java
    class ScopeErrors {
        int x = 5;
        public static void main(String[] args) {
        x++; // won't compile, x is an 'instance' variable
        //and main is a static function.
        }
    }
    ```
    


## Method Overriding

- In every case, when an exact match isn't found, the JVM uses the method with the smallest argument that is wider than the parameter.
the compiler will choose widening over boxing`Widening > Boxing > var-args`

*it's not legal to widen from one wrapper class to another, because the wrapper classes are peers to one another.*

```java
public class HelloWorld {

    public void m(int i){
        System.out.println("int");
    }

    public void m(float i){
        System.out.println("float");
    }
    public static void main(String[] args) throws Exception{
        HelloWorld h = new HelloWorld();
        byte b = 10;
        h.m(b);
    }
}
```

- You CANNOT widen and then box. (An int can't become a Long.)
- You can box and then widen. (An int can become an Object, via Integer.)
    
    ```java
    class WidenAndBox {
        static void go(Long x) {
            System.out.println("Long");
        }
    
        public static void main(String[] args) {
            byte b = 5;
            go(b); // must widen then box - illegal
        }
    }
    ```
    

ANOTHER EXAMPLE:

```java
public class HelloWorld {

    public void m(List i) {
        System.out.println("list");
    }

    public void m(Collection i) {
        System.out.println("collection");
    }

    public void m(ArrayList i) {
        System.out.println("array");
    }

    public static void main(String[] args) throws Exception {
        HelloWorld h = new HelloWorld();
        List<Integer> l = new ArrayList<Integer>();
        h.m(l);
    }
}
```

Widening primitive conversion (§5.1.2) is applied to convert either or both operands as specified by the following rules:

- If either operand is of type `double`, the other is converted to `double`.
- Otherwise, if either operand is of type `float`, the other is converted to `float`.
- Otherwise, if either operand is of type `long`, the other is converted to `long`.
- **Otherwise, both operands are converted to type `int`.**

**The left side defines the OVERLOADING and right side defines OVERRIDING.**

### OVERLOADING

Two methods will be treated as overloaded if both follow the mandatory rules below:

> Both must have the same method name.
Both must have different argument lists.
> 

And if both methods follow the above mandatory rules, then they may or may not:

> Have different return types.
Have different access modifiers.
Throw different checked or unchecked exceptions.

### OVERRIDING

- It must have the same method name.
- It must have the same arguments.
- It must have the same return type. From Java 5 onward, the return type can also be a subclass (subclasses are a covariant type to their parents).
- It must not have a more restrictive access modifier (if parent --> protected then child --> private is not allowed).
- It must not throw new or broader checked exceptions.

And if both overriding methods follow the above mandatory rules, then they:

- May have a less restrictive access modifier (if parent --> protected then child --> public is allowed).
- May throw fewer or narrower checked exceptions or any unchecked exception.

## Collections

```java
Collection cs = new ArrayList<String>(); //this will work fine.
Collection cs = new HashSet<String>();
```

But it won’t compile:
```
Collection cs =new HashMap<String, Integer>(); 
//as Hashmap is not a part of Collection.
```

### Synchronized vs Concurrent Collection

- Synchronized collections locks the whole collection
- Concurrent collection uses lock stripping. For example, the ConcurrentHashMap divides the whole map into several segments and locks only the relevant segments, which allows multiple threads to access other segments of same ConcurrentHashMap without locking. But if the Map grows longer its costly to lock the whole MAP. Instead it doesn’t matter to ConcurrentHashMap as it uses a part to lock.

- CopyOnWriteArrayList allows multiple reader threads to read without synchronization and when a write happens it copies the whole ArrayList and swap with a newer one.
- Synchronized ArrayList is a synchronized collection while CopyOnWriteArrayList is a concurrent collection.
CopyOnWriteArray is more scalable than Sync ArrayList.
- The Iterator returned from synchronized ArrayList is a fail fast but iterator returned by CopyOnWriteArrayList is a fail-safe iterator i.e. it will not throw ConcurrentModifcationException even when the list is modified
- *NOTE: the size of ArrayList if its big then obviously cost of copying after a write operation is high enough to compensate the cost of locking but if ArrayList is really tiny then you can still use CopyOnWriteArrayList.*

### Sorting a Hashmap by Values

```java
Map<String, String> hashMap = new HashMap<>();
//... add some random values.
Entry<String, String> entries = Map.entrySet();
Comparator<Entry<String, String>> valueComparator = new Comparator<Entry<String,String>>() {

      @Override
      public int compare(Entry<String, String> e1, Entry<String, String> e2) {
          String v1 = e1.getValue();
          String v2 = e2.getValue();
          return v1.compareTo(v2);
      }
  };

// Sort method needs a List, so let's first convert Set to List in Java
List<Entry<String, String>> listOfEntries = new ArrayList<>(entries);

// sorting HashMap by values using comparator
Collections.sort(listOfEntries, valueComparator);

LinkedHashMap<String, String> sortedByValue = new LinkedHashMap<>(listOfEntries.size());

// copying entries from List to Map
for(Entry<String, String> entry : listOfEntries){
    sortedByValue.put(entry.getKey(), entry.getValue());
}
```
### Fail Fast Iterator:

- Enhanced for loop is fail fast.

- Fail Fast Iterator: Means any structural modification made to ArrayList like `adding or removing elements` during Iteration will throw java.util.ConcurrentModificationException.
```java
Iterator<String> iterator=arrayList.iterator();
while(iterator.hasNext()){
	//iterator.next() should be called else we will face other errors.
    System.out.println(iterator.next());
        arrayList.add("ind"); //Exception in thread "main" java.util.ConcurrentModificationException
        arrayList.remove("var"); //Exception in thread "main" java.util.ConcurrentModificationException
        it.remove(); // no Exception.
        //iterator doesnt have an add() function.
}
```


### ArrayList

- java.util.ArrayList is Resizable-array implementation of the java.util.List interface. ArrayList `implements RandomAccess(Marker interface)` to indicate that they support fast random access (i.e. index based access) in java.
- Iterator returned by java.util.ArrayList is fail-fast. Means any structural modification made to ArrayList like `adding or removing elements` during Iteration will throw java.util.ConcurrentModificationException.

`Removing element by using iterator is allowed.`



Element has been added during iteration which will cause ConcurrentModificationException to be thrown.
`Enhanced for Loop is also Fail Fast.`

- Default initial capacity of ArrayList is 10. ArrayList is resized by 50% of it’s current size.
- Initially when a ArrayList is created its size is 0, As soon as first element is added, using add(i), where i=1, ArrayList is initialized to it’s default capacity of 10.

### LinkedList

- java.util.LinkedList is implemented using Doubly-linked list.
- `Get method iterates on nodes sequentially to get element on specified index. Hence, offering O(n) complexity.`
- Iterator returned by LinkedList is fail fast in java.

### HashSet

- java.util.HashSet is implementation of the java.util.Set interface.

- HashSet enables us to store element, it does not allow us to store duplicate elements in java.
- `iterator returned by java.util.HashSet is fail-fast.`
- HashSet is internally implemented using java.util.HashMap in java.

```java
// Dummy value to associate with an Object in the backing Map
private static final Object PRESENT = new Object();

/**
 * Constructs a new, empty set; the backing <tt>HashMap</tt> instance has
 * default initial capacity (16) and load factor (0.75).
 */
public HashSet() {
    map = new HashMap<>();
}
/**
 * Adds the specified element to this set if it is not already present.
 * More formally, adds the specified element <tt>e</tt> to this set if
 * this set contains no element <tt>e2</tt> such that
 * <tt>(e==null&nbsp;?&nbsp;e2==null&nbsp;:&nbsp;e.equals(e2))</tt>.
 * If this set already contains the element, the call leaves the set
 * unchanged and returns <tt>false</tt>.
 *
 * @param e element to be added to this set
 * @return <tt>true</tt> if this set did not already contain the specified
 * element
 */
public boolean add(E e) {
    return map.put(e, PRESENT)==null;
}
		
```

- Null elements - One null element can be added in HashSet in java.
- What is Load Factor in java: Default load factor is 0.75
That means when set will be 75% filled, it’s capacity will be doubled.

### HashMap

- java.util.HashMap is implementation of the java.util.Map interface.
- HashMap enables us to store data in key-value pair form.
- iterator returned is fail fast in java.
- `The HashCode will define the bucket in which a Element goes and Equal defines if the Element gets overridden or not`
- `TreeMap does not allow to store null key but allow many null values.`
- LinkedHashMap uses doubly linked lists and TreeMap uses Red black tree.
- `Hashing` means generating a (hopefully) unique number that represents a value.
    - Computing a hash is not necessarily constant-time with regard to the value being hashed.
    For example, computing the hash of a string involves iterating the string, and isn't constant-time with regard to the size of the string.
-  **JAVA 8 HASHMAP IMPROVEMENTS**
    - TREEIFY_THRESHOLD = 8
    - Buckets containing a large number of colliding keys will store their entries in a balanced tree instead of a linked list after certain threshold is reached.
    - This tree is a Red-Black tree. It is first sorted by hash code. If the hash codes are the same, it uses the compareTo method of Comparable 
    - Red-black tree is balanced.
    - If entries are removed from the map, the number of entries in the bucket might reduce such that this tree structure is no longer necessary. That's what the UNTREEIFY_THRESHOLD = 6 is for. If the number of elements in a bucket drops below six, we might as well go back to using a linked list.
    
    - While converting the list to binary tree, hashcode is used as a branching variable. If there are two different hashcodes in the same bucket, one is considered bigger and goes to the right of the tree and other one to the left. But when both the hashcodes are equal, HashMap assumes that the keys are comparable, and compares the key to determine the direction so that some order can be maintained.

### HashCode and Equals

1. if hasCode is returning 1 and equals is returning true,
   The elements goes into the same bucket. Already existing entries will get overridden.
2. If hashCode is returning 1 and equals is returning false,
   it gets into the same bucket, but because of equals it doesn't override.
3. if hashCode is returning computed value for each element,
   it goes into different buckets so even if you do equals true, only objects with same reference will get overriden. 

## EXCEPTION

Checked exceptions are checked at compile-time. It means if a method is throwing a checked exception then it should handle the exception using try-catch block or it should declare the exception using throws keyword, otherwise the program will give a compilation error.
example: `SQLException, IOExeception, ClassNotFoundException`

Unchecked exceptions are not checked at compile time. It means if your program is throwing an unchecked exception and even if you didn’t handle/declare that exception, the program won’t give a compilation error.
example: `NullPointerException, ArrayIndexOutOfBoundException, IllegalArgumentException, NumberFormtException, ArithematicException`

NumberFormtException

```java
String s = "FOOBAR";
int i = Integer.parseInt(s)
```
### 3 ways to create objects of a class 'Class'

- By using` Class.forName()`:

		Class c1 = Class.forName("Class1");
- By using `getClass()` :

		Class1 c = new Class();
		Class c1 = c.getClass();
- By using `.class` :

		Class c1 = Class1.class;

## Serialization:

- When we Deserialize class ( class which has been modified after Serialization and also class declare SerialVersionUID) its gets DeSerialized successfully.
- When we Deserialize class ( class which has been modified after Serialization and also class doesn’t declare SerialVersionUID) InvalidClassException is thrown.

Serialization allows Compatible and Incompatible Changes:

- compatible changes:
    - adding new fields.
    - Changing access modifier of a field
    - changing a field from static to non static (similar to adding new field).
- incompatible changes:
    - removing fields.
    - changing from nonstatic to static fields (similar to deleting old fields).

> If Serialization is not present we can use JSON to transmit over network and Hibernate to persist in DB.
> 
- ArrayList, HashSet and HashMap implements Serializable interface, so if we will use them as member of class they will get Serialized and DeSerialized as well
- If Serializable has been implemented - constructor is not called during DeSerialization process.
- If superclass has implemented Serializable - constructor is not called during DeSerialization process.
- If superclass has not implemented Serializable - constructor is called during DeSerialization process.
- Objects and primitives would be initialized the default value `when deserialized` if they were not part of serialization.
- If superClass has implemented Serializable that means subclass is also Serializable (as subclass always inherits all features from its parent class), `for avoiding Serialization in sub-class we can define writeObject() method and throw NotSerializableException()` from there as done below.
    
    ```java
    private void writeObject(ObjectOutputStream os) throws NotSerializableException {
        throw new NotSerializableException("This class cannot be Serialized");
    }
    ```
    

# Immutable Class:

An immutable object is one that will not change state after it is instantiated.

In general, an immutable object can be made by defining a class which does not have any of its members exposed, and does not have any setters.

```java
class ImmutableInt {
    private final int value;
    private final List<T> list;

    public ImmutableInt(int i, List<T> list) {
        // creates a new ArrayList and keeps a reference to it.
        this.list = new ArrayList(list);
        value = i;
    }

    public int getValue() {
        return value;
    }

    public List<T> getList() {
        // return a copy of the list so the internal state cannot be altered
        return new ArrayList(list);
		}
}
```

Reflection breaks immutability since we can use it to change non-constant final fields.

An example of a Reflection-resistant immutable object would be the following:

```java
static class Immutable {
    // This field is a constant, and cannot be changed using Reflection
    private final int value = Integer.MIN_VALUE;

    public int getValue() {
        return value;
    }
}

public static void main(String[] args) throws NoSuchFieldException, SecurityException, IllegalArgumentException, IllegalAccessException {
    final Immutable immutable = new Immutable();

    final Field f = Immutable.class.getDeclaredField("value");
    f.setAccessible(true);

    System.out.println(immutable.getValue());
    f.set(immutable, Integer.MAX_VALUE);
    System.out.println(immutable.getValue());
}
```

This can’t be broken. 

Even with Serialization we can break immutability if someone has access to the byte stream they can alter the bytes and they break immutability.


### Differences between Interface and Abstract class:

|  Interface | Abstract Class |
|--|--|
| Interfaces dont extend classes | Abstract classes extends one or more Interfaces  |
|Interfaces dont have constructors | Classes have constructors |
| All functions in interfaces need to be implemented by the subclasses | Only Abstract functions needs to be implemented. |
|Interfaces dont have synchronized functions. | Instance menthods in Abstract class can be synchronized. But **Abstract Functions are not synchronized.** |
|All the variables in Interface are public, [static](http://www.javamadesoeasy.com/2015/05/static-keyword-in-java-variable-method.html) and [final](http://www.javamadesoeasy.com/2015/05/final-keyword-in-java-20-salient.html) by default. Interface variables are also known as constants. | Abstract class can have private, instance variables as well.



[JAVA 8](https://www.notion.so/JAVA-8-3c7b3b54902f481da4b6d484734db240?pvs=21)