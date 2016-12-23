# Objects, Memory Models, and Scope

#### Defining Classes and Creating Objects  
Objects in real world: Map, Shape, Location, Color, Window, etc.

A **class** is a type of data. An **object** is one such piece of data with associated functionality.

Example: (in file SimpleLocation.java)
```java
public class SimpleLocation{
  public double latitude; // member variables
  public double longitude;

  public SimpleLocation(double lat, double lon){ // special method: constructor
    this. latitude = lat;
    this. longitude = lon;
  }
  public double distance(SimpleLocation other){ // method
    ...
  }
}
```

**Member variables** exist throughout the class, represents essential piece of data the objects need to store. Declared inside class, but outside of methods.  
**Methods** are the things the class can do.  
**Constructor** is a special method called when creating a new object. It does not have a return type.(Eg: ```public ClassName(arg1, arg2, ...)```)  

Example: (in file LocationTester.java)
```java
public class LocationTester{
  public static void main(String[] args){
    // creating new objects
    SimpleLocation ucsd =  
      new SimpleLocation(32.9, -117.2);
    SimpleLocation lima =  
      new SimpleLocation(-12.0, -77.0);
    System.out.println(ucsd.distance(lima));
  }
}
```
Breakdown of ```ucsd.distance(lima)```:  
```java
public double distance(SimpleLocation other){
  return getDist(this.latitude, this.longitude,
    other.latitude, other.longitude);
}
// lima is passed into parameter *other* and used
// in the distance method. ucsd is the calling
// object referenced by *this*.
```

Keyword **this** refers to the calling object (i.e. which object is calling the method).


#### Overloading Methods  
The new SimpleLocation class: (in file SimpleLocation.java)
```java
public class SimpleLocation{
  // Member variables not shown
  public SimpleLocation(){ // default constructor, no parameters
    this. latitude = 32.9;
    this. longitude = -117.2;
  }
  public SimpleLocation(double lat, double lon){ // parameter constructor
    this. latitude = lat;
    this. longitude = lon;
  }
}
```
**Default constructor** is the construct that can be invoked without passing any data at all. **Parameter constructor** is the constructor that takes data and passes to the variables to create new objects. The two different kinds of constructors can exist at the same time.

**Overloading** problem occurs here since there are two different copies of constructors that both create new objects but the way they are assigned values and types differ slightly.

We can also overload methods: (in file SimpleLocation.java)  
```java
public class SimpleLocation{
  // code omitted here
  public double distance(SimpleLocation other){
    ...
  }
  public double distance(double otherLat, double otherLon){
    ...
  }
}
```

Things to notice when overriding a method:
- Return type is **NOT** part of the method signature.  
- Overloaded methods **CAN** have different return types.  
- Changing the return type is **NOT ENOUGH** for overloading, parameters mush be different.  

A wrong example:
```java
public class SimpleLocation{
  // Code omitted here
  public double distance(SimpleLocation other){
    ...
  }
  public int distance(SimpleLocation other){
    ...
  }
}
```


#### Public & Private  
```java
public class SimpleLocation{
  public double latitude; // member variables
  public double longitude;
  ...
  public double distance(SimpleLocation other){
    ...
  }
}
```
**Public** means the variable/ method can be accessed from any class.

```java
public calss SimpleLocataion{
  private double latitude;
  private double longitude;

  public SimpleLocation(double lat, double lon){
    this. latitude = lat; // allowed
    this. longitude = lon; // allowed
  }
}
```
If we try to access *lima.latitude* in another class LocationTester in the file LocationTester.java (e.g. ```lima.latitude = -12.04```) will cause an error.

**Private** means the variable/ method can only be accessed inside this class definition.  
*Pro tip*: Make member variables private (and methods either public or private) to protect data and enhance security.  
For methods, if one is made for world use, then make it public; if one is used as helper methods, then make it private.

To give the proper level of access of the member variables, we use **"getter and setter"** methods.

Example(getter):
```java
public class SimpleLocation{
  private double latitude;
  private double longitude;

  public double getLatitude(){ // getter
    return this.latitude;
  }
}
```
The purpose of a **getter** is to expose a private member variable outside of its class. In the example above, the member variable *latitude* is still private, but can be accessed by other classes through the getter method.  
Now in the file LocataionTester.java, we are allowed to access *lima*'s latitude by calling ```lima.getLatitude()``` (e.g. ```System.out.println(lima.getLatitude()));``` is allowed).

We can provide a **setter** method to allow user change the values of member variables.  
```java
public class SimpleLocation{
  private double latitude;
  private double longitude;

  public double getLatitude(){ // getter
    ...
  }
  public void setLatitude(double lat){ // setter
    this. latitude = lat;
  }
}
```
Having the getters and setters gives us more control of member variables. We can check and make sure a legal value is passed to latitude.
```java
public void setLatitude(double lat){
  if(lat < -180 || lat > 180){ // out of range
    System.out.println("Illegal value for latitude");
  } else{
    this. latitude = lat;
  }
}
```


#### Drawing Memory Models with Primitive Data  

**Primitive types of data** includes *int*, *double*, *float*, *short*, *long*, *char*, *boolean*, *byte*.  
**Object types of data** includes *arrays* and *classes*.  
Example of the two types of data:
```java
int var1 = 52;
SimpleLocation ucsd = new SimpleLocation(32.9, -117.2);
// stored in heap, ucsd is a reference to the location of
// the created SimpleLocataion Object.
```


#### Scope
