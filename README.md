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
8. [Design Principles of SOLID: OCP](#ocp)
9. [Design Principles of SOLID: LSP](#lsp)

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
  * **A module should be responsible to one, and only one, actor**
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

# <a name="srp">8. OCP: The Open-Closed Principle</a> 
- The definition of OCP is: **A software artifact should be open for extension but closed for modification**
- In other words, the behavior of a software artifact ought to be extendible, without having to modify that artifact
## Example: Financial Information
- We have a webapp that displays summary of financial info that is scrollable and display negative numbers in red color
- A request was made to print a repot in only black and white
- A good software architecture here would reduce the amount of changed code via:
  * Separating the things that change for different reasons (SRP) and then organizing the dependencies between those things properly (DIP)
### SRP Approach
- Following SRP would result in a plan like this: 
  
![Following-SRP-Principle](/img/SRP-Plan.PNG)
- The essential insight here is that generating the report involves 2 separate responsibilities: 
  * The calculation of the reported data
  * The presentation of the data
 
![Partitioning-Classes](/img/Partitioning-Classes.png)
- This sepeation can be done, like above, by having a: **Controller - Interactor - Database - Presenter - Views**
- Open arrowheads are using relationships. Closed arrowheads are implements or inheritance relationships
- Notice that each double line is crossed in one direction only. This means that all component relationships are unidirectional
- `FinancialDataMapper` knows about `FinancialDataGateway` through an implements relationship, but `FinancialGateway` knows nothing at all about `FinancialDataMapper`

### Protection Levels

![unidirectional](/img/unidirectional.png)
- We can see that we have arrows that point toward the components that **we want to protect from change**
- If component A should be protected from changes in component B, then component B should depend on component A
- We want to protect the **Controller** from changes in the **Presenters**
- We want to protect the **Presenters** from changes in the **Views**
- We want to protect the **Interactor** from changes in—well, anything
- Changes to the Database, or the Controller, or the Presenters, or the Views, will have **no impact on the Interactor**
- Why should we protect the Interactor? Because it contains the business rules, and the highest-level policies
- Notice how there's **levels of protection:**
  * Interactor has the highest level, Controller is next, then the Presenter, and last is the Views
- **This is how the OCP works: it separates functionality based on how, why, and when it changes, and then organize them into protection levels**

### Information Hiding
- The `FinancialReportRequester` interface purpose is to protect the `FinancialReportController` from knowing too much about the `Interactor`
- If the interface were not there, the *Controller* would have transitive dependencies on the `FinancialEntities`
- Our first priority is to protect the *Interactor* from changes to the *Controller*, but we also want to protect the *Controller* from changes to the *Interactor* by hiding the internals of the *Interactor*

# <a name="lsp">9. LSP: The Liskov Substitution Principle</a> 
- **Definition:** What is wanted here is something like the following substitution property: If for each object o1 of type S there is an object o2 of type T such that for all programs P defined in terms of T, the behavior of P is unchanged when o1 is substituted for o2 then S is a subtype of T

### Example that follows LSP: 

![License](/img/License.PNG)
- This design conforms to LSP because: the behavior of the `Billing` does not depend on any of the 2 subtypes Both of the subtypes are substitutable for the `License` class

## Example that violates LSP: The Square/Rectangle Problem

![square-rectangle](/img/square-rectangle-prb.PNG)

- Above, `Square` is not a proper subtype of `Rectangle` because the height and width of the Rectangle are independently mutable
- In contrast, the height and width of the `Square` must change together
- Since the User believes it is communicating with a `Rectangle`, it could easily get confused
### Fix
- To adjust to LSP principle in this example, we add mechanisms to the User (such as an if statement) that detects whether the `Rectangle` is a Square
- Since the behavior of the User depends on the types it uses, those types are not substitutable


