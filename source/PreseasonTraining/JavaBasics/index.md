# Java Basics

## Agenda

* What is Java  
  * A JIT (compiled "Just-In-Time") language  
    * Compiled into Java bytecode  
    * Then bytecode is translated into machine code on target platform  
    * Makes code more cross-platform than C/C++, which is directly compiled  
  * Statically typed  
    * Every variable has a "type"  
    * E.g. "int" \== integer number, "double" \== decimal number, "string" \== text, etc  
    * Lower case type is primitive type: basic type included with java  
    * Upper case type is class type: complex type, see next  
* Object oriented programming  
  * Everything is an "object" of a certain "class"  
  * Each class can have "variables" and "functions"  
  * There's also "constructors", which are methods that run when you create an object  
  * There's also modifiers: "public", "private", "readonly", "static"  
  * Private things can only be accessed inside the object  
  * Static are the same for all object instances of a class  
  * Readonly cant be modified after creation, all readonly are static  
  * Btw, "variable" \== "member" \== "attribute" and "function" \== "method"  
  * Python and C++ also have classes  
  * Example class:

```{code-block} java
:caption: Car.java
:lineno-start: 1

class Car {  
	public Engine engine = new Engine();  
public float fuel = 0; // Unit is gallons  
public float mileage = 0; // Unit is miles  
	  
// The constructor  
public Car(float fuel, float mileage) {  
	this.fuel = fuel;  
	this.mileage = mileage;  
}

// This method drives the car by X miles  
public void driveCar(float miles) {  
	this.mileage += miles;  
	this.fuel -= miles / 33.0; // 33 miles per gallon  
}

// Adds fuel to car  
	public void addFuel(float fuel) {  
		this.fuel += fuel;  
	}  
}	  
```

```{code-block} java
:caption: Main.java
:lineno-start: 1
 
// ...  
Car hondaCivic = new Car(0, 0); // Brand new car with zero mileage  
hondaCivic.addFuel(10.0f); // Filled it up  
hondaCivic.driveCar(30.0f); // Drove the car to IMSA  
// ...  
```

* Libraries  
  * Other code you import, typically written by other people  
  * Consists of classes  
  * Standard library is included with java  
  * WPILib is the library we use for FRC  
  * It has stuff like classes for motors and sensors  
  * We also have a library for our gyroscope written by the manufacturer  
  * And also our own library for reusable code: Titan Algorithms  
* Walkthrough practice: inventory manager

```{code-block} java
:caption: Item.java
:lineno-start: 1
 
class Item {  
	public string Name;  
	public int Quantity;

	public Item(string name, int quantity) {  
		this.Name = name;  
		this.Quantity = quantity;  
	}  
}  
```

```{code-block} java
:caption: Inventory.java
:lineno-start: 1

class Inventory {  
private ArrayList<Item> items = new ArrayList<Item>();

public Inventory() {}

public void addItem(Item item) {  
items.add(item);  
}

public String search(String query) {  
	String result = "";  
	for (int i = 0; i < items.size(); i++) {  
		If (items.get(i).Name.contains(query)) {  
			// Yes you can add strings  
			// "\n" or "\r\n" on Windows is special character for new line  
			result += items.get(i).Name + "\r\n";  
		}  
}   
}	  
}  
```

```{code-block} java
:caption: Main.java
:lineno-start: 1
  
import java.util.Scanner; // Import standard library for print, this is done automatically by VSCode

// Name of our program  
class InventoryManager {  
	public static void main(String[] args) {  
 		// Java’s print function  
    		System.out.println("Welcome to inventory manager.");  
    		System.out.println("Type ‘/add [item name] [quantity]’ to add an item. No spaces in name.");  
		System.out.println("Type ‘/search [item name]’ to find an item.");  
		System.out.println("Type ‘/exit’ to leave the program.");

		// Scanner object used to record user input  
Scanner inputScanner = new Scanner(System.in);

// Our inventory  
Inventory inventory = new Inventory();

		// While loops run if the condition is true  
		boolean isRunning = true;  
		while (isRunning) {  
String userInput = inputScanner.nextLine(); // Get command input  
String[] splittedCommand = userInput.split("\\s+"); // Array with each word

			If (userInput.startsWith("/exit")) {  
				isRunning = false;  
System.out.println("Exiting.");  
			} else if (userInput.startsWith("/add")) {  
				// Item count is a string but has to be converted to an integer type  
				Item newItem = new Item(splittedCommand[1], Integer.parseInt(  
splittedCommand[2]));  
				inventory.addItem(newItem);  
System.out.println("Item added.");  
			} else if (userInput.startsWith("/search")) {  
				String query = splittedCommand[1];  
				System.out.println(inventory.search(query));  
			} else {  
				System.out.println("Unknown command.");  
			}  
		}  
	}  
}  
```