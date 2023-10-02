# Clean Coding Principles

## Contents

- [Background](#background)
- [Introduction](#introduction)
- [Meaningful Names](#meaningful-names)
  - [General Rules](#general-rules)
  - [Naming Patterns](#naming-patterns)
- [Functions](#functions)
  - [General Rules](#general-rules-1)
- [Comments](#comments)
  - [Avoid In Most Cases](#avoid-in-most-cases)
  - [Good Comments](#good-comments)
  - [Bad Comments](#bad-comments)
- [Formatting](#formatting)
  - [General Rules](#general-rules-2)
- [Objects and Data Structures](#objects-and-data-structures)
  - [Background](#background-1)
  - [General Rules](#general-rules-3)
- [Error Handling](#error-handling)
  - [General Rules](#general-rules-4)
- [Boundaries](#boundaries)
  - [Background](#background-2)
  - [General Rules](#general-rules-5)
- [Unit Tests](#unit-tests)
  - [Test Driven Development (TDD)](#test-driven-development-(tdd))
  - [Three Laws](#three-laws)
  - [General Rules](#general-rules-6)
- [Classes](#classes)
  - [General Rules](#general-rules-7)
- [Systems](#systems)
  - [General Rules](#general-rules-8)
- [Code Smells](#code-smells)
  - [Definition](#definition)
  - [Types](#types)

## Background

These coding principles are heavily based on the book 'Clean Code: A Handbook of Agile Software Craftsmanship', written by Robert C. Martin.

## Introduction

Code that is clean and easy to understand reduces the cognitive load for future maintainers, making it easier to debug, extend, and refactor. It should be written as if it's to be maintained by someone else, because that is what actually happens!

## Meaningful Names

### General Rules

#### Use Intention-Revealing Names

Choose names that clearly express the intention behind the variable, method, or class. This should give enough information for a reader to understand its purpose without needing to dig into the implementation.

```python
# Bad
x = 42

# Good
age = 42
```

#### Avoid Disinformation

Don't use names that could be misleading or suggest a wrong type. For instance, avoid using the lowercase letter `l` or the uppercase letter `O` as variable names, as they could be mistaken for the numbers `1` and `0`, respectively.

```python
# Bad
l = 1

# Good
length = 1
```

#### Make Meaningful Distinctions

Avoid names that are easy to misspell or sound similar but have different meanings. For instance, avoid having both `controller` and `control` as class names in the same codebase unless they serve different purposes.

```python
# Bad
def control(): pass
def controller(): pass
class UserManager(): pass
class UserManagement(): pass

# Good
def user_control(): pass
def system_controller(): pass
class UserServices(): pass
class UserAdministration(): pass
```

#### Use Pronounceable Names

Names should be easily pronounceable. This makes discussion easier and avoids confusion.

```python
# Bad
cstmrId = 1

# Good
customer_id = 1
```

#### Boolean Variables

Prefix booleans with `is`, `has`, `can`, or similar, e.g., `isDone`, `hasPermission`.

```java
// Bad
boolean done;
boolean p;

// Good
boolean isDone;
boolean hasPermission;

```

#### Use Searchable Names

The name should be unique enough that a search throughout the codebase returns only the instances you are interested in. Short/single-letter names are *only* acceptable when used as local variables within a short function.

```java
// Bad
int s;

// Good
int speed;
```

#### Avoid Encodings

Encoding in this context means writing a names in a certain way that it 'retains' information about *what* the variable is. For example:

1. Making the first letter of the name represent its type.
2. Prefixing interfaces with the letter 'I'.

(2) is particularly common, but unnecessary! An interface is just another 'thing' - consumers needn't know that they're dealing with an interface or a concrete class.

```java
// Bad
interface IShape { ... }
String strName = "John";
int iAge = 25;

// Good
interface Shape { ... }
String name = "John";
int age = 25;
```

#### Avoid Mental Mapping

Don’t make the reader translate or do mental mapping. For example, if you are using an abbreviation, it should be one that's universally understood, like `HTTP` or `ID`.

#### Don't Be Cute

Avoid clever and/or humorous names. Keep it simple - say what you mean and mean what you say.

#### Use Solution Domain Names

'Solution Domain' = Where the technical stuff happens.

Using Solution Domain Names just means using commonly-known technical terminology within the industry (i.e. Software Engineering) to describe things. For example, algorithm names, pattern names, maths terms etc.

```javascript
// Bad
function doStuff() { ... }
const users = [...];
class Processor { ... }

// Good
function quickSort() { ... }
const userQueue = [...];
class AbstractFactory { ... }
```

#### Use Problem Domain Names

'Problem Domain' = Where the requirements are defined (i.e. the customer).

Using Problem Domain Names just means using terminology from the 'real-life' area that the software relates to.

```javascript
// Bad
function calculate() { ... }

// Good
function calculateMortgage() { ... }
```

#### Add Meaningful Context

Names should live within a well-named class, function or namespace to provide 'full' meaning about what the variable *really* refers to.

For example, a `name` variable on its own could relate to almost anything, but in the scope of a `Animal` class it is clear what it refers to.

```javascript
// Bad
const duration = 5;
function validate() { ... }
function onClick() { ... }

// Good
const durationDays = 5;
function validateUserInput() { ... }
function onLoginButtonClick() { ... }
```

#### Don't Add Gratuitous Context

'Gratuitous' means 'done without good reason'.

This just means that context should not be added to a name when it is not required.

### Naming Patterns

#### Classes

Use a *noun* or *noun phrase* for the name of a class. This means that the name of the class could be the subject of a verb.

Avoid using verbs to name a class - a class shouldn't be a 'do-er', it should be a 'thing'. For example, avoid using 'Manager' and 'Processor' in class names.

Avoid using vague and ambiguous terminology in class names e.g. 'Data', 'Info' etc.

#### Functions

Use a *verb* or *verb phrase* for the name of a function. This means that the name reads as a 'do-er' entity.

##### Accessors, Mutators and Predicates

- Accessors should be prefixed with `get`.
- Mutators should be prefixed with `set`.
- Predicates should be prefixed with `is`/`has` etc.

##### Boolean Return Values

These functions should read like a yes-no question. For example:

```javascript
function isReady() { ... }
function hasProperty() { ... }
function canEdit() { ... }
```

##### Overloaded Constructors

Overloaded constructors should be accessed via static factory methods that describe the arguments. For example:

```javascript
// Bad
const foo1 = new Foo({ bar: true }); 
const foo2 = new Foo({ baz: true }); 

// Good
const foo1 = Foo.withBar(true);
const foo2 = Foo.withBaz(true);
```

#### Boolean Variables/Properties

These should be prefixed with `is`, `has` or `can`. For example:

```javascript
// Bad
const ready = true;

// Good
const isReady = true;
```

Avoid negatives, which can lead to an extra layer of thinking and makes the name a little more verbose. For example:

```javascript
// Bad
const isNotReady = true;
if (isNotReady) { ... }

// Good
const isReady = false;
if (!isReady) { ... }
```

#### Constants

Use all uppercase with underscores. For example:

```javascript
const MAX_COUNT = 10;
```

#### Enums

- Use a singular name for the type and all caps for members. For example:

```javascript
enum Outcome {
    SUCCESS,
    ERROR
}
```

## Functions

### General Rules

#### Small

They should not be long! This keeps each function easy to read and understand.

#### Single Responsibility

Functions should do one thing. They should do it well. They should do it only.

For example, an error handling function should *only* do error handling. In such a function, nothing should appear after the try-catch-finally blocks:

```javascript
function runProcess() {
    try {
        run();
    } catch(error) {
        processError(error);
    } finally {
        notifyEnd();
    }
}
```

#### Descriptive Name

The name of a function should leave no surprises to the reader when they read the body of the function. If the function is kept small and responsible for one thing only, naming it well shouldn't be too difficult. Sometimes, longer names are required, which is fine.

#### Number of Arguments

The fewer the number of arguments, the better. 0 is better than 1, which is better than 2, etc. There are a couple of reasons:

- Ease of understanding for the reader.
- Reduction of test cases (more arguments = more permutations to test).

> Avoid more than three arguments!

#### Reducing Arguments

To reduce the argument count for a function, an object can be use to wrap a subset of the arguments. Whilst appearing like cheating, it isn't. Provided the arguments are 'grouped' together in a logical way.

#### Boolean Arguments

Boolean arguments are an instant indication that a function is doing more than one thing. The function should instead be split into two functions that each handle the different scenario.

> Boolean arguments are bad and should be avoided!

#### No Side Effects

A side-effect is a hidden behaviour within a function. It's something that the function name doesn't clearly indicate will happen, leading to surprises.

#### Avoid Output Arguments

> This basically translates as *don't mutate input arguments*.

An output argument is an argument to a function that is used to return data to the caller. Typically, this means the argument is *mutated*.

Output arguments should *usually* be avoided, for a few reasons:

- Readability (It can be hard to know from reading the function what it does).
- Surprise Factor (It isn't necessarily obvious that the input will be mutated, without digging into the implementation).
- Testing Difficulty

The main reason to use output arguments is when working with large data structures. Cloning them is costly in terms of memory and performance, so mutating them may be preferable.

Example of an output argument:

```javascript
function squareAndStore(arr) {
    for (let i = 0; i < arr.length; i++) {
        arr[i] = arr[i] * arr[i];
    }
}

const numbers = [1, 2, 3];
squareAndStore(numbers);
console.log(numbers);  // [1, 4, 9]
```

#### Command Query Separation

> Functions should either *do* something or *answer* something.

For example, a function should either change the state of an object, or return some information about that object.

```typescript
// Bad - What does the return value mean?
function set(key: string, value: string): boolean;
...
if (set('isUser', false)) { ... }
```

#### Extract Try-Catch Blocks

Try-Catch blocks add extra nesting and make everything that little bit less readable. Extract the bodies of these blocks into their own functions.

For example:

```javascript
try {
    foo();
} catch(error) {
    processError(error);
}
```

## Comments

### Avoid In Most Cases

> The first rule about comments: don't write comments!

There are some cases when comments are useful and/or necessary, but using them should be the exception and not the rule.

Reasons:

- Code should be self-explanatory. There is no need for comments to explain how code works if the code is written well!
- Comments soon become outdated.

Instead of using a comment, use a function/variable instead!

### Good Comments

#### Legal

Enterprise software is typically required to have legal statements in-code e.g. copyright.

#### Explanation of Intent

Comments can be useful to explain *why* a certain decision was taken, that would not be immediately obvious to explain.

#### Clarification

Sometimes, obscure arguments/return types can be supplemented with a comment to give them a more readable representation. This can be useful if the code being used cannot be altered (e.g. it's from a third party or the standard library).

#### Warning of Consequences

A comment can be used as an extra layer of safety, to inform other developers about some non-trivial consequence of running a particular piece of code.

#### TODO Comments

TODO comments are useful to indicate intent of future changes that are planned. But such comments should only be added with a reference to the work that is scheduled (e.g. a Jira ticket reference).

#### API Documentation

Docblock comments can be used to provide human-readable documentation for the interface of a piece of software, which is useful for external consumers to understand the behaviour and contract.

### Bad Comments

- Nonsensical/misleading comments (i.e. a comment that is difficult to understand).
- Redundant comments (i.e. a comment that is no longer relevant).
- Mandated comments (i.e. docblocks for functions/properties that don't need them).

## Formatting

### General Rules

#### Vertical Openness Between Concepts

> Use blank lines to separate different 'concepts' in code.

For example, a blank line should surround the following:

- Groups of imports.
- Groups of class property definitions.
- Methods.

#### Vertical Distance

> Concepts that are closely-related should be kept vertically close to each other.

For example:

- Variable declarations should live close to where the variables are used.
- Dependent functions should live close to one another, and the caller should live above the callee. (This makes sense given that we typically read top to bottom).

**NOTE:** This does not apply to class instance variables, which should be declared at the top of the class.

#### Conceptual Affinity

Code that is related to a similar concept should be grouped together.

## Objects and Data Structures

### Background

#### Objects

- Hide their data behind abstractions.
- Expose functions that operate on that data.

#### Data Structures

- Expose their data.
- Have no meaningful functions.

### General Rules

#### Law of Demeter

> A piece of code should not know about the internals of the objects it uses.

This law is a 'principle of least knowledge', summarised as:

- Each unit should have only limited knowledge about other units: only units "closely" related to the current unit.
- Each unit should only talk to its friends; don't talk to strangers.
- Only talk to your immediate friends.

This is important in keeping code *loosely-coupled*.

For example:

```javascript
// Bad
class Bank {
    transferMoney(customer, amount) {
        customer.getWallet().debit(amount);
    }
}

// Good
class Bank {
    transferMoney(customer, amount) {
        customer.transferMoney(amount);
    }
}
```

> NOTE: The Law of Demeter does not apply if accessing data structures, given their data structure is meant to be exposed.

#### Data Transfer Objects (DTOs)

A DTO is a data structure with public properties and no functions. They are used for serializing the state of an object for communication (e.g. over a network) that requires converting the state of an object into another format.

## Error Handling

### General Rules

#### Exceptions Over Error Codes

Error codes should be avoided for a few reasons:

- They violate Command Query Separation (A function that returns an error code is *doing* something and returning some data).
- The caller must deal with the error immediately (This means error handling code is intermingled with happy path code.).
- Code becomes heavily nested as each different error code is considered.

```javascript
// Bad
if (foo(bar) === ERROR_CODES.OK) {
    if (baz(bar) === ERROR_CODES.OK) {
        ...
    } else {
        logError('error in baz');
    }
} else {
    logError('error in foo');
}

// Good
try {
    foo(bar);
    baz(bar);
    ...
} catch (error) {
    logError(error);
}
```

#### Begin with Try-Catch-Finally

When writing code that could throw exceptions, it's good to begin with the try-catch-finally block. Reasons:

- It means the unhappy path scenario is not treated as an afterthought - it is considered in tandem with the happy path.
- The error handling code is *immediately* separated from the happy path code (Separation of Concerns).

#### Add Context to Exceptions

Each thrown exception should be *useful*. They should contain enough information to determine the source of the error and where it was thrown.

> Error messages should be informative and provide context of the *intent* of the failed operation.

#### Tailor Exceptions to Callers

Exceptions should be created bearing in mind how they will be used by callers. Ask the questions:

- Will this exception be useful?
- Will this exception make sense?

#### Use the Special Case Pattern

The Special Case Pattern is a way to *implicitly* handle 'exceptional' cases, without resorting to explicit checking/error handling. It involves returning an object that adheres to the same interface as normal-case objects but with different and simplified behaviour.

Benefits:

- Readability (It reduces the number of conditional checks).
- Leveraging of Polymorphism, making it easier to extend in the future by adding new cases.

##### Example (No Special Case Pattern)

```typescript
class Plan {
    getMonthlyRate(): number {
        return 100;
    }
}

class Customer {
    private hasPlan: boolean;

    constructor(hasPlan: boolean) {
        this.hasPlan = hasPlan;
    }

    getPlan(): Plan | null {
        return this.hasPlan ? new Plan() : null;
    }
}

const customer1 = new Customer(true);
const customer2 = new Customer(false);

const plan1 = customer1.getPlan();
const plan2 = customer2.getPlan();

if (plan1) {
    console.log(plan1.getMonthlyRate()); // 100
} else {
    console.log('No plan for customer 1');
}

if (plan2) {
    console.log(plan2.getMonthlyRate());
} else {
    console.log('No plan for customer 2'); // "No plan for customer 2"
}
```

##### Example (Special Case Pattern)

```typescript
class Plan {
    getMonthlyRate(): number {
        return 100;
    }
}

class NullPlan extends Plan {
    getMonthlyRate(): number {
        return 0;
    }
}

class Customer {
    private hasPlan: boolean;

    constructor(hasPlan: boolean) {
        this.hasPlan = hasPlan;
    }

    getPlan(): Plan {
        return this.hasPlan ? new Plan() : new NullPlan();
    }
}

const customer1 = new Customer(true);
const customer2 = new Customer(false);

const plan1 = customer1.getPlan();
const plan2 = customer2.getPlan();

console.log(plan1.getMonthlyRate()); // 100
console.log(plan2.getMonthlyRate()); // 0
```

#### Don't Return Null

Allowing null to be returned from a function provides immediate overhead for callers to check for it. It also adds a risk of a null pointer exception!

Instead:

- Throw an exception.
- Use a Special Case object (see above).

#### Don't Pass Null

Similar to the above, passing null into a function mandates the need for null checks within, which is extra overhead. It is also not very readable - what does passing null *mean*?

## Boundaries

### Background

In software, a 'boundary' represents where two different pieces of code 'meet'. Boundaries exist at all levels, for example:

- Between applications.
- Between microservices within an application.
- Between 'layers' within a codebase (e.g. service layer - domain layer).

A common boundary to deal with is that with third-party code, that cannot be controlled.

### General Rules

#### Use Interfaces

Using interfaces to define a boundary is beneficial for a few reasons:

- It abstracts away the complexity of internal implementation.
- It allows for loose-coupling to a specific implementation, making it easy to swap out for a different implementation.

#### Use Generic Data

Data crossing a boundary should be suitably generic and in its simplest form.

#### No Data Leaks

Data that crosses a boundary should not 'leak' data that reveals the underlying implementation.

## Unit Tests

### Test Driven Development (TDD)

#### Summary

TDD is a methodology that emphasizes writing tests before writing the actual production code. It aims to create clean, simple, and bug-free code by iteratively writing tests, implementing code, and refactoring.

#### Benefits

- Bug Prevention (Issues are identified early).
- Simplified Debugging (Given the iterative nature, issues are easy to diagnose given that changes between 'clean' states are small).
- Self-documenting Tests (Given that each new bit of behaviour is supplemented with a test, the suite of tests describes all aspects of the code's behaviour).
- Ease of Refactoring (A full suite of tests makes refactoring safer).

### Three Laws

1. You may not write production code until you have written a failing unit test.
2. You may not write more of a unit test than is sufficient to fail, and not compiling is failing.
3. You may not write more production code than is sufficient to pass the currently failing test.

### General Rules

#### Test Code === Production Code

Test code is *just* as important as production code, so all of the clean code practices should still apply.

#### Single Concept per Test

As with normal functions, unit tests should have a *single responsibility*. This means they should do one thing, rather they should *test* one thing. Often, this means using a single assertion per test. Although, this doesn't always make sense.

#### Keep Setup Separate

It is good practice to separate the setup (i.e. preparing mocks, stubbing data, configuring the test environment) from the tests themselves. Reasons:

- Readability
- Extensibility (Adding another test becomes trivial, there is no need to re-do any of the setup)
- Small + focused tests (It becomes incredibly easy to see what is being tested)

#### F.I.R.S.T Principles

- **Fast**: Tests should run quickly.
- **Independent**: Tests should not depend on each other.
- **Repeatable**: Tests should be repeatable in any environment.
- **Self-Validating**: Tests should either pass or fail, without requiring manual inspection.
- **Timely**: Tests should be written just before the production code that makes them pass.

## Classes

### General Rules

#### Organisation

From top to bottom:

- Public static constants
- Private static variables
- Private instance variables
- Public methods
- Private methods

#### No Public Instance Variables

Typically, there is no need for public instance variables and they should be avoided. Reasons:

- Loss of control (Without enforcing change through a well-defined interface, there is no control over how the variable can be changed)
- Encapsulation (The details of an objects internal implementation should be hidden)
- Tight Coupling (Changing the internal implementation can be difficult if other classes directly reference it)

#### Small

A Class should be small, not just in terms of lines of code but also in terms of responsibilities.

#### Clear Name

The name of a class should clearly indicate its responsibilities. Vague names like 'Manager' or 'Processor' should be avoided.

> As a rule of thumb, you should be able to describe a class in about 25 words without using conjunctions like 'if', 'and', 'or', or 'but'.

#### Single Responsibility Principle (SRP)

A class should have only one reason to change.

#### High Cohesion

Cohesion refers to how closely the methods and variables of a class are related to each other. A method is 'cohesive' if it uses most/all of a class's instance variables.

Why is high cohesion good? If the methods and variables of a class are co-dependent, this means that they 'belong together' in logic manner.

> A class should have a small number of instance variables.
> Low cohesion is an indication that a class should be split into multiple classes.

#### Open-Closed Principle (OCP)

The Open-Closed Principle states that a class should be open for extension but closed for modification.

- **Open for Extension**: The addition of new functionality should be able to be achieved without modifying existing code.
- **Closed for Modification**: Once code has been developed and tested, it should not be altered to accommodate new functionality.

#### Dependency Inversion Principle (DIP)

High-level classes that provide complex business logic should not depend on low-level classes that are responsible for low-level operations. Both should depend on *abstractions*.

An abstraction is something that defines a contract rather than a concrete implementation i.e. an interface or an abstract class.

Benefits:

- Decoupling: Different implementations can seamlessly be swapped in.
- Testability: Dependencies can easily be mocked, making writing tests much easier.

## Systems

### General Rules

#### Separate Construction from Use

Systems should separate the startup process from the runtime logic. The startup process involves constructing application objects and wiring dependencies. This can be achieved by:

- Moving all aspects of 'construction' to 'main'.
- Design the rest of the system (the application) assuming all dependencies have been created and wired up.
- Have 'main' pass all dependencies to the application.

#### Abstract Factory Pattern

A design pattern that provides an interface for creating families of related or dependent objects without specifying their concrete classes.

Process:

1. **Client Code**: The client code requests an object from a factory object instead of creating it directly.
2. **Factory Interface**: The client uses a factory interface to create the object, which allows the client to use different factory implementations without changing the client code.
3. **Concrete Factory**: The concrete factory implements the factory interface and returns the concrete product.

Benefits:

- Loose Coupling (The client code is decoupled from the concrete classes).
- Consistency (Objects created by the factory are compatible with each other).

Example:

```java
// Abstract Factory
public interface GUIFactory {
    Button createButton();
    Checkbox createCheckbox();
}

// Concrete Factory
public class WindowsFactory implements GUIFactory {
    public Button createButton() {
        return new WindowsButton();
    }
    public Checkbox createCheckbox() {
        return new WindowsCheckbox();
    }
}

// Abstract Product
public interface Button {
    void render();
}

// Concrete Product
public class WindowsButton implements Button {
    public void render() {
        System.out.println("Render a button in a Windows style");
    }
}

// Client Code
public class Application {
    private GUIFactory factory;
    private Button button;

    public Application(GUIFactory factory) {
        this.factory = factory;
        this.button = factory.createButton();
    }

    public void render() {
        button.render();
    }
}
```

#### Dependency Injection

Dependency Injection basically means delegating the creation of objects, rather than constructing them directly.

It is a way of achieving 'Inversion of Control' (IoC) between classes and their dependencies, which means the control flow is delegated to decoupled components.

Benefits:

- Decoupling (A class isn't tied to a specific implementation).
- Testability (It is very easy to mock a dependent type that is passed in, rather that constructed internally).

The most common example of this is specifying an interface as a constructor parameter, although this can also apply to methods.

Example (No Dependency Injection):

```typescript
class App {
  run() {
    console.log("App is running");
  }
}

const app = new App();
app.run();
```

Example (With Dependency Injection):

```typescript
interface Logger {
  log(message: string): void;
}

class ConsoleLogger implements Logger {
  log(message: string) {
    console.log(message);
  }
}

class App {
  private logger: Logger;

  constructor(logger: Logger) {
    this.logger = logger;
  }

  run() {
    this.logger.log("App is running");
  }
}

const logger = new ConsoleLogger();
const app = new App(logger);
app.run();
```

## Code Smells

### Definition

A code smell is a surface-level indication in the source code that usually corresponds to a deeper problem in the system. It's not an error as such, but more a 'feeling' that good practices and fundamental design principles have been violated.

### Types

#### Structural Issues

- **Code Duplication**: Repeating the same code in multiple places.
- **Redundant Coupling**: Unnecessary dependencies between classes or modules.
- **Large Vertical Separation**: Related code should be close together for readability and maintainability.
- **Clutter**: Includes things like default constructors, redundant comments, and any other 'noise' in the code.

#### Naming and Syntax

- **Ambiguous Names**: Poorly named variables, functions, classes, etc., that don't convey their purpose or functionality.
- **Magic Numbers/Strings**: Using unnamed constants that lack context.
- **Not Following Standard Conventions**: This can include naming conventions, file structures, or any other established best practices.
- **Negative Conditionals**: This adds an extra layer of thinking and increases verbosity.

Good:

```javascript
if (shouldFoo()) { ... }
```

Bad:

```javascript
if (!shouldNotFoo()) { ... }
```

#### Abstraction and Encapsulation

- **Implementation Code at the Wrong Level of Abstraction**: Code that is too low-level or too high-level for its current context.
- **Feature Envy**: Methods that seem more interested in a different class than the one they actually belong to.
- **Use of if/else or switch over Polymorphism**: Using conditional statements instead of leveraging polymorphism.
- **Un-encapsulated Conditionals**: Conditional logic that should be wrapped in a function for clarity and reusability.

#### Responsibility and Cohesion

- **Misplaced Responsibility**: Code that doesn't belong in its current location, often violating the Single Responsibility Principle.
- **Lack of Consideration of Edge Cases**: Not handling or even considering special cases that might not be immediately obvious.

#### Compliance and Overrides

- **Overriding Rules**: Ignoring or disabling linters, compilers, or other rules, often indicating a workaround or a potential misunderstanding.
- **Inconsistency**: Inconsistent code styles or practices across the codebase, making it hard to read and maintain.

#### Comments

- **Needless/Obsolete Information**: Comments that no longer serve a purpose or are misleading.
- **Poorly-Written**: Comments that are hard to understand or that confuse more than they clarify.
- **Commented-Out Code**: Old code that's been commented out rather than removed.

#### Environment

- **Building the Environment is Not Easy**: Requires multiple steps to set up.
- **Running All Tests Requires Multiple Steps**: Makes it difficult to maintain a test-driven development approach.

#### Functions

- **Too Many Arguments**: Functions that take more arguments than they should, making them hard to understand and use.
- **Output Arguments**: Functions that modify external variables, making them harder to reason about.
- **Boolean Arguments**: Using boolean flags as arguments, which can make the function's behaviour less clear.
- **Unused Functions**: Functions that are no longer called from anywhere.

#### Surprise

- **Unexpected Function behaviour**: A function's implementation should not be surprising. If it does something that wasn't anticipated, something is wrong.
