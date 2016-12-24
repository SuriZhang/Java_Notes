# Inheritance

#### Inheritance

**Inheritance and polymorphism** are good for complex, large projects.  

*ProTip*:  
1. Keep common behavior in one class.  
2. Split different behavior into separate classes.  
3. Keep all of the objects in a single data structure.  

Example:
```java
public class Person{ // base class/ super class/ parent class
  private String name;
  ...
}

public class Student extends Person{ // derived class/ sub class/ child class
  // private String name;
  private double gpa;

  public double getGPA(){
    return this. gpa;
  }
  ...
}

public class Faculty extends Person{
  // private String name;
  private double salary;

  public double getSalary(){
    return this. salary;
  }
  ...
}
```
The keyword **extends** means "inherit from", *public instance variables*, *public methods* and *private instance variables* are inherited.  
**Notice**: private variables can be accessed ONLY through public methods (getters and setters).

**UML diagram** contains *class name*, *member variables* and *methods*.  

Example:
```java
Person p = new Person(); // allowed
Person p = new Student(); // allowed
Student s = new Person(); // NOT allowed

Person[] p = new Person[3]; // a Person array
p[0] = new Person();
p[1] = new Student();
p[2] = new Faculty();
```
A Person array can store Student and Faculty objects.  
The *Person p* and *Student s* are **references**, *new Person()* and *new Student()* are **objects**. The compile time decisions depend on **reference type**, the runtime decisions depend on **object type**.


**Visibility Modifiers** include *public*, *protected*, *package* and *private* (listed from less restrictive to more restrictive).
Visibility | Accessibility
-----------|--------------
**Public** | can be accessed from ANY class.  
**Protected** | can be accessed from SAME class, SAME package, ANY subclass.
**Package** | can be accessed from SAME class, SOME package. 
**Private** | can be accessed from the SAME class.

Breakdown of ```Student s = new Student();```:  
**new** allocates space; **Student()** is the constructor, **this** is passed to the constructor.  
Objects are created from **inside out** in Java (i.e. subclass -> superclass -> indirect superclass). In this line of code, we can interpret as *Student()* -> *Person()* -> *Object()*.

**Compiler rules for class construction**:  
1. if no superclass, compiler inserts: ```extends Object```  
```java
public class Person extends Object{
  private String name;
  ...
} 
```
2. if no constructor, Java will automatically generate one for each class.
```java
public class Person extends Object{
  private String name;
  public person(){
    ...
  }
}
```
3. the first line must be either ```this(args)```(some class constructor call) or ```super(args)```(super class constructor call); otherwise, Java will insert ```super();```(a call to the default super class constructor).
```java
public class Person extends Object{
  private String name;

  public Person(){
    super(); // inserted by Java when compiling
  }
}
```

Initializing *name* variable in *Person*:
```java
public class Person extends Object{
  private String name;

  public Person(String n){
    super(); // fix the error below by switching the order
    this. name = n;
    // super(); <- this will cause compiler error since 
    // super(); must be the first line 
  }
}
```
Similarly, 
```java
public class Student extends Person{
  public Student(String n){
    /*super();
    this. name = n;*/
    super(n); // <- initializing *name* without a 
    // public setter
  }
  public Student(){ // no-arg constructor,
    this("Student");  // use a default value for *name* 
    // with the *same class constructor, this*
  }
}
```

Comparison between overloading and overriding:
**Overloading** |  **Overriding** |
----------------| ----------------|
*SAME* class has same method name with *DIFFERENT* parameters | *SUBCLASS* has same method name with the *SAME* parameters as the superclass |

Example of *toString* Method overrode from Object Class:  
```java
public class Person{
  private String name;

  public String getName(){
    return this. name;
  }
  public String toString(){
    return this.getName();
  }
}
-----------------------------------------------------
Person p = new Person("Tim");
System.out.println(p.toString()); // calls Person's 
// toString() method
System.out.println(p); // println automatically calls 
// toString()
```

In Student class:
```java
public class Student extends Person{
  private int studentID;

  public int getID(){
    return studentID;
  }
  public String toString(){
    // return this. getSID() + ": " + this.getName();
    return this. getSID() + ": " + super. toString(); 
    // super refers to superclass, in case Student class 
    // changes
  }
}
-----------------------------------------------------
Student s = new Student("Cara", 1234);
System.out.println(s); // calls Student toString()
```
> 1234: Cara


#### Polymorphism

```java
Person s = new Student("Cara", 1234);
System.out.println(s);
```
> 1234: Cara

**Polymorphism** is a superclass reference to subclass object.

UML:  
Person | Student | Faculty |  
------|--------|--------|  
String name | int StudentID | String employeeID |  
String getName() | int getSID() | String getEID() |  
String toString() | String toString() | String toString() |  

```java
Person p[] = new Person[3];
p[0] = new Person("Tim");
p[1] = new Student("Cara", 1234);
p[2] = new Faculty("Mia", "ABCD");

for(int i = 0; i < p.length; ++i){
  System.out.println(p[i]);
  // calls toString() method depends on dynamic type
}
```
>Tim  
> 1234: Cara  
> ABCD: Mia  


**Compile Time Rules for Polymorphism**:    
1. Compiler ONLY knows *reference type*  
2. Can only look in reference type class for method  
3. Outputs a method signature  

**Run Time Rules for Polymorphism**:
1. Follow exact *runtime type* of object to find method    
2. Must match compile time method signature ti appropriate method in actual object's class  
