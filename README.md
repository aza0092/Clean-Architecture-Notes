# Clean Architecture
My personal notes on the book Clean Architecture: A Craftsman's Guide to Software Structure and Design by Robert C. Martin


# Index

1. [What is Design and Architecture](#design-architecture)
2. [A Tale of Two Values](#two-values)
3. [Paradigm Overview](#paradigms)
4. [Structured Programming](#structured)
5. [Object Orient Programming](#oop)
6. [Functional programming](#functional)
7. [Design Principles of SOLID: SRP](#srp)

# <a name="design-architecture">1. What is Design and Architecture</a>

- There is no difference between design and architecture
- 'Architecture' is used in high-level context, whereas 'design' is used in low-level context. But both are part of the whole software design
- The goal of software architecture is to minimize the human resources required to build and maintain the required system
- The best option is for the development organization to recognize and avoid its own overconfidence and to start taking the quality of its software architecture seriously

# <a name="two-values">2. A Tale of Two Values</a>

- Every software system provides two different values to the stakeholders: **behavior and structure**
- Software developers are responsible for ensuring that both those values remain high
- Function or architecture? Which of these two provides the greater value?
- The first value of software —behavior— is urgent but not always particularly important
- The second value of software —architecture— is important but never particularly urgent
- **Software priorities lie within these 4 points:**
    * 1. Urgent and important
    * 2. Not urgent and important
    * 3. Urgent and not important
    * 4. Not urgent and not important
   
# <a name="paradigms">3. Paradigm Overview</a>

- A paradigm tells you which programming structures to use, and when to use them. 
- There are three paradigms: **structured programming - object orient programming - functional programming**
- **Structured programming:** imposes discipline on direct transfer of control (ex: if/else/then)
- **Object orient programming:** imposes discipline on indirect transfer of control (ex: polymorphism/function pointers)
- **Functional programming:** imposes discipline upon assignment (ex: mathematical symobls)

# <a name="structured">4. Structured programming</a>

- Via structured programming, programmers could break down large proposed systems into modules and components that could be further broken down into tiny provable functions

# <a name="oop">5. Object Orient Programming</a>

- OOP supports the proper admixture of: encapsulation, inheritance, and polymorphism
- Encapsulation allows data to be hidden and only functions are known. We see this concept in action as the private data members and the public member functions of a class

# <a name="functional">6. Functional programming</a> 

- Variables in functional languages do not vary/are not modified
- It is important to consider immutable variables because: All race conditions, deadlock conditions, and concurrent update problems are due to mutable variables. 
- You cannot have a race condition or a concurrent update problem if no variable is ever updated. You cannot have deadlocks without mutable locks
- As an architect, you must be asking yourself whether immutability is practicable

## Segregation of Mutability
- One of the most common compromises in regard to immutability is to **segregate the application** into: mutable and immutable components
- Immutable components perform their tasks in a functional way, without using any mutable variables

## Event Sourcing
- Imagine if we added up all the transactions of a bank client, we'd require a lot of processing power
- But we could use event sourcing and have enough storage to perform that
- the **event sourcing** stategy here is we store the transactions, but not the state. When state is required, we simply apply all the transactions from the beginning of time
- We could therefore not update or delete anything, and thus making our application immutable, therefore **functional**
- Event Sourcing ensures that all changes to application state are stored as a sequence of events

# <a name="srp">7. SRP: The Single Responsibility Principle</a> 

- The SOLID principles tell us how to arrange our functions and data structures into classes, and how those classes should be interconnected
- The goal of SOLID for software is to:
  * 1. tolerate changes
  * 2. easy to understand
  * 3. make components used in many software
  
- Each principle of SOLID is as follows:
  * **1. SRP: The Single Responsibility Principle**
    * The best structure for a software is influenced by the social structure of the organization that uses it so that each software module has one, and only one, reason to change
  * **2. OCP: The Open-Closed Principle**
    *  The gist is that for software to be easy to change, it must be designed to allow the behavior of it to be changed by adding new code, rather than changing existing code
  * **3. LSP: The Liskov Substitution Principle**
    *  To build software from interchangeable parts, those parts must adhere to a contract that allows those parts to be substituted one for another
  * **4. ISP: The Interface Segregation Principle**
    *  Avoid depending on things that you don’t use 
  * **5. DIP: The Dependency Inversion Principle**
    * The code that implements high-level policy should not depend on code that implements low-level details. Rather, details should depend on policies

## The Single Responsibility Principle (SRP)
- It is wrong to assume that SRP means that every module should do just one thing. There is a principle like that, a function should do one thing
- Software systems are changed to satisfy users and stakeholders; those users and stakeholders are the “reason to change” that the principle is talking about
- But since users/skateholders change, we'll refer to them as actors
- Thus, the **definition** of The Single Responsibility Principle (SRP) is: 
  * A module should be responsible to one, and only one, actor
- What we mean by a module is just basically a source file or set of functions

## Violations of SRP
### 1. Duplication
![employee-class](/img/employee.PNG)
- An `Employee` class that has three functions, ` calculatePay(), reportHours(), and save()` would violate SRP because:
  *  Those three methods are responsible to **three different actors**
     * The `calculatePay()` method would be specified by the accountant, who reports to the **CFO**
	 * The `reportHours()` method would be specified by HR, who reports to the **COO**
	 * The `save()` method would be specified by the DBA, who reports to the **CTO**
- When a new function like `regularHours()` is added and is used in `calculatePay()`, developers may not pay attention if the method is also used in `regularHours()` which could result in not testing it in the two methods and cause issues after deployment
- **The key here in this example is that the SRP says to separate the code that different actors depend on**

### 2. Merges
- Multiple people changing the same source file for different reasons causes risks
- One solution is to **seperate code that supports different actors**
![seperate-data-from-functions](/img/employeeData.PNG)
- One way to solve the above `Employee` class is to seperate data from functions
![facade-pattern](/img/facade.png)
- However, the downside of this is developers now have 3 classes they have to instantiate and track. A common solution here is to use the Facade pattern
![keep-important-functions-in-Employee](/img/originalEmployee.png)
- The `EmployeeFacade` contains very little code, so the solution is to keep the most important method in the original `Employee` class and then using that class as a Facade for the lesser functions, like above