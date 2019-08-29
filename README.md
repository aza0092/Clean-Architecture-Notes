# Clean Architecture
My personal notes on the book Clean Architecture: A Craftsman's Guide to Software Structure and Design by Robert C. Martin


# Index

1. [What is Design and Architecture](#design-architecture)
2. [A Tale of Two Values](#two-values)
3. [Paradigm Overview](#paradigms)
4. [Structured Programming](#structured)
5. [Object Orient Programming](#oop)
6. [Functional programming](#functional)

# <a name="design-architecture">What is Design and Architecture</a>

- There is no difference between design and architecture
- 'Architecture' is used in high-level context, whereas 'design' is used in low-level context. But both are part of the whole software design
- The goal of software architecture is to minimize the human resources required to build and maintain the required system
- The best option is for the development organization to recognize and avoid its own overconfidence and to start taking the quality of its software architecture seriously

# <a name="two-values">A Tale of Two Values</a>

- Every software system provides two different values to the stakeholders: **behavior and structure**
- Software developers are responsible for ensuring that both those values remain high
- Function or architecture? Which of these two provides the greater value?
- The first value of software —behavior— is urgent but not always particularly important
- The second value of software —architecture— is important but never particularly urgent
- **Software priorities lie within these 4 points:**
   1. Urgent and important
   2. Not urgent and important
   3. Urgent and not important
   4. Not urgent and not important
   
# <a name="paradigms">Paradigm Overview</a>

- A paradigm tells you which programming structures to use, and when to use them. 
- There are three paradigms: **structured programming - object orient programming - functional programming**
- **Structured programming:** imposes discipline on direct transfer of control (ex: if/else/then)
- **Object orient programming:** imposes discipline on indirect transfer of control (ex: polymorphism/function pointers)
- **Functional programming:** imposes discipline upon assignment (ex: mathematical symobls)

# <a name="structured">Structured programming</a>

- Via structured programming, programmers could break down large proposed systems into modules and components that could be further broken down into tiny provable functions

# <a name="oop">Object Orient Programming</a>

- OOP supports the proper admixture of: encapsulation, inheritance, and polymorphism
- Encapsulation allows data to be hidden and only functions are known. We see this concept in action as the private data members and the public member functions of a class

# <a name="functional">Functional programming</a> 

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
