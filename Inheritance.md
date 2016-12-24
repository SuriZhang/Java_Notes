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
