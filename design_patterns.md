
STATUS: WIP

Q: What is the root source of all problems in software development?
A: Change - Software must be adaptable to frequent changes

Q: What is the core problem of adaptable software and software development in general?
A: Dependencies - It all comes down to dependencies in some way or another
     - Physical dependencies
     - Logical dependencies
     - Artificial dependencies (this is often what makes it so hard to change things)


Core guidelines:
- Design software for easy change and extension
  - Rule: Don't guess! If you expect change, prefer design that makes this change easy.
- Design for readability
  - Rule: Spend time to find good names for all entities
- Design for testability

---------------------
Core principles:
- Single Responsibility Principle (SRP) - Advices to seperate concerns to isolate and simplify change.
- Don't Repeat Yourself (DRY) - Advice to reduce duplication in order to simplify change.
- Open-Closed Principle (OCP) - Advices to prefer design that simplifies the extension by types or operations. Optimally, you should be able to add new code without modifying existing code.
   - Open to extension, closed for modification.
- Keep it Simple (KIS)

TODAY: We value the value-semantic style : but the principles are the same?

---------------------

Problems:
  - Inheritance : Using inheritance naively to solve an arbitrary problem easily leads to
    - ... many derived classes
    - ... ridiculous class names
    - ... deep inheritance hierarchies
    - ... duplication between similar implementations (DRY)
    - ... (almost) impossible to extend at some point (OCP)
    - ... impeded maintanance
    - If this happens, applying the "Strategy design pattern" for dependency injection
      might help to organize the code.

--------------------- DESIGN PATTERNS -------------------------

A design pattern ...
- ... has a name
- ... carries an intent
- ... aims at reducing dependencies
- ... provides some sort of abstraction
- ... has proven to work over the years

Design patterns:
- The Strategy Design Pattern (dependency injection)
  - Same as Policy Design Pattern?
  - Extracts implementation details and isolates them (SRP)
  - Creates the opportuinity for easy change 
  - Reduced duplication (DRY)
- The Template MEthod Design Pattern
  - TODO
- The Adapter Pattern (Same as Strategy?)
  - TODO
- The Factory Pattern
  - TODO
- The State Pattern
  - TODO

References:
- Klaus Iglberger, Back to Basics: Designing Classes, CppCon 2021
- The Gang-of-Four, Origin of 23 of the most commonly used design patterns.
