# Comprehensive Software Development Style Guidelines

This document synthesizes fundamental coding principles from clean code practices, practical programming wisdom, and industry standards. These guidelines will help you write code that is readable, maintainable, and robust.

## Table of Contents
1. [Code Organization](#code-organization)
2. [Naming Conventions](#naming-conventions)
3. [Functions and Methods](#functions-and-methods)
4. [Classes and Objects](#classes-and-objects)
5. [Comments and Documentation](#comments-and-documentation)
6. [Error Handling](#error-handling)
7. [Testing Practices](#testing-practices)
8. [Version Control](#version-control)
9. [Code Layout and Formatting](#code-layout-and-formatting)
10. [Complexity Management](#complexity-management)
11. [Concurrency](#concurrency)
12. [Refactoring](#refactoring)

## Code Organization

### Do:
- **Divide programs into subsystems to manage complexity**
   - *Example:* Separate your application into user interface, business logic, and data access layers.

- **Keep files small and focused**
   - *Example:* Most files should be under 500 lines, with many under 200 lines.

- **Minimize interconnections between subsystems**
   - *Example:* Let your UI subsystem interact with the business logic layer, but not directly with the data access layer.

- **Design for change by isolating areas likely to change**
   - *Example:* Isolate hardware-dependent code into specific classes that can be modified without changing other code.

- **Use encapsulation to hide implementation details**
   - *Example:* Create a class that manages database connections but only exposes high-level methods like `GetCustomerInfo()`.

- **Place abstractions in code, details in metadata**
   - *Example:* Store configuration parameters, URLs, and other environment-specific data in config files rather than hardcoding them.

### Don't:
- **Create excessive distribution of information**
   - *Example:* Don't scatter user interface code throughout your system—keep it in a designated UI layer.

- **Create circular dependencies**
   - *Example:* Don't let Class A depend on Class B while Class B also depends on Class A.

- **Optimize prematurely for performance**
   - *Example:* Don't break good design principles just because you think a different approach might be faster before measuring performance.

- **Create deeply nested subsystems**
   - *Example:* Don't create subsystems within subsystems within subsystems without a clear organizational principle.

- **Mix code with dramatically different levels of abstraction**
   - *Example:* Don't include low-level file I/O operations in a high-level business logic class.

- **Create "God" classes that do everything**
   - *Example:* Don't create a `SystemManager` class that handles user interface, business logic, and data access.

## Naming Conventions

### Do:
- **Use intention-revealing names**
   - *Example:* `int elapsedTimeInDays` instead of `int d`

- **Use pronounceable names**
   - *Example:* `generationTimestamp` instead of `genymdhms`

- **Use searchable names**
   - *Example:* `WORK_DAYS_PER_WEEK = 5` instead of just the number `5`

- **Use class names that are nouns or noun phrases**
   - *Example:* `Customer`, `WikiPage`, `Account`, `AddressParser`

- **Use method names that are verbs or verb phrases**
   - *Example:* `postPayment()`, `deletePage()`, `save()`

- **Include units in names of numeric variables**
   - *Example:* `timeoutInSeconds` or `fileSizeInBytes` rather than just `timeout` or `size`

### Don't:
- **Use misleading names or abbreviations**
   - *Example:* Don't use `hp` (could mean hypotenuse or horsepower)

- **Use names with subtle differences**
   - *Example:* Avoid nearly identical names like `XYZControllerForEfficientHandlingOfStrings` and `XYZControllerForEfficientStorageOfStrings`

- **Use lowercase `l` or uppercase `O` as variable names**
   - *Example:* They look like the numbers 1 and 0

- **Use noise words like Info, Data, a, an, the**
   - *Example:* `ProductInfo` and `ProductData` don't add meaningful distinctions

- **Use Hungarian notation or encoding schemes**
   - *Example:* Modern IDEs make `strName` or `m_description` unnecessary

- **Create variable names with minimal psychological distance**
   - *Example:* Don't use both `inputData` and `inputDate` in the same scope

## Functions and Methods

### Do:
- **Keep functions small and focused on a single task**
   - *Example:* Write a `validateUser()` function that only validates user data, not one that also saves to a database and sends emails.

- **Use descriptive function names**
   - *Example:* `calculateMonthlyPayment()` rather than `calc()` or `doIt()`

- **Limit the number of parameters (aim for 0-2)**
   - *Example:* Use a parameter object instead of four separate parameters

- **Make functions do one thing well**
   - *Example:* `createBankAccount()` should only create an account, not validate, notify, etc.

- **Use consistent argument patterns**
   - *Example:* List input-only parameters first, followed by modified parameters

- **Return early from functions for special cases**
   - *Example:* 
   ```java
   public Result processItem(Item item) {
       if (item == null) {
           return Result.failure("Item cannot be null");
       }
       if (!item.isValid()) {
           return Result.failure("Invalid item");
       }
       // Process valid item
       return Result.success(item.process());
   }
   ```

### Don't:
- **Create routines with low cohesion**
   - *Example:* Don't create a `HandleData()` routine that writes to a file, updates a database, and changes screen colors.

- **Use meaningless, vague, or wishy-washy verbs in routine names**
   - *Example:* Avoid names like `ProcessData()`, `HandleCalculation()`, or `PerformServices()`

- **Create functions that modify input parameters**
   - *Example:* Don't use an input parameter as a working variable within a routine

- **Mix different levels of abstraction within a routine**
   - *Example:* Don't mix high-level business logic with low-level bit manipulation code in the same routine

- **Create excessively long parameter lists**
   - *Example:* Don't create a routine with 12 parameters when many could be grouped into a parameter object

- **Use flag parameters that change function behavior**
   - *Example:* Instead of `render(boolean isSuite)`, create `renderForSuite()` and `renderForSingleTest()`

## Classes and Objects

### Do:
- **Create classes that present consistent abstraction levels**
   - *Example:* A `Customer` class should provide methods like `PlaceOrder()` and `GetOrderHistory()`, not a mix of high-level operations and low-level data manipulation.

- **Follow the Single Responsibility Principle (SRP)**
   - *Example:* A class should have only one reason to change.

- **Use containment for "has a" relationships**
   - *Example:* A `Car` class contains `Engine`, `Transmission`, and `Wheels` objects.

- **Use inheritance only for "is a" relationships**
   - *Example:* `CheckingAccount` and `SavingsAccount` inherit from a base `BankAccount` class.

- **Hide implementation details by minimizing accessibility**
   - *Example:* Make all data members private and provide public methods only for operations that other classes need to perform.

- **Design orthogonal components**
   - *Example:* Changes to a UI component shouldn't require changes to the database layer.

### Don't:
- **Create "god classes" that do everything**
   - *Example:* Don't create a `SystemManager` class that handles UI, business logic, and data access.

- **Overuse inheritance**
   - *Example:* Don't create deep inheritance hierarchies with more than 2-3 levels of inheritance.

- **Expose member data in public interfaces**
   - *Example:* Don't make class fields public; instead provide getter and setter methods if needed.

- **Use multiple inheritance except for simple "mixins"**
   - *Example:* Don't create complex diamond inheritance patterns where a class inherits from multiple classes that share a common ancestor.

- **Create classes with weak cohesion**
   - *Example:* Don't create a `Utility` class with unrelated methods like `CalculateTax()`, `FormatHTML()`, and `ConnectToDatabase()`.

- **Include more than about 7 data members in a class**
   - *Example:* If your class has 15+ data members, consider whether it should be decomposed into multiple smaller classes.

## Comments and Documentation

### Do:
- **Comment the "why," not the "what"**
   - *Example:* Comment on why a particular algorithm was chosen rather than explaining what each line does.

- **Use comments for legal information**
   - *Example:* Include copyright notices and license information

- **Use comments to warn of consequences**
   - *Example:* "// Don't run unless you have some time to kill."

- **Document assumptions and preconditions**
   - *Example:* "// This function assumes input array is sorted in ascending order"

- **Write comments at a higher level of abstraction than the code**
   - *Example:* For a line that reads `i = i + 1`, write "// Advance to next employee" not "// Add 1 to i"

- **Write informative javadocs/docstrings for public APIs**
   - *Example:* Clear documentation for methods that will be used by other developers

### Don't:
- **Write comments that merely repeat the code**
   - *Example:* Don't write `i++; // increment i` as this adds no value.

- **Write redundant comments that just restate the code**
   - *Example:* `i++; // increment i` is unnecessary

- **Write misleading or outdated comments**
   - *Example:* Comments that no longer match what the code actually does

- **Write journal comments or revision history**
   - *Example:* Use source control systems instead of commenting code changes

- **Comment out code**
   - *Example:* Delete unused code instead of commenting it out

- **Write noisy comments that don't add new information**
   - *Example:* `// The day of the month` for a variable named `dayOfMonth`

## Error Handling

### Do:
- **Use exceptions rather than return codes**
   - *Example:* Throw a `StorageException` instead of returning an error code

- **Provide context with exceptions**
   - *Example:* Include the operation that failed and why it failed in the exception message

- **Write try-catch-finally statements first**
   - *Example:* Define the scope and the expected error conditions before coding the implementation

- **Clean up resources in all error paths**
   - *Example:* Use try-finally blocks to ensure files are closed even when exceptions occur

- **Handle errors at the appropriate level**
   - *Example:* Catch file system exceptions near where file operations occur, but propagate business rule violations to the business layer

- **Fail fast and crash early when problems are detected**
   - *Example:* If a required configuration setting is missing, fail immediately during startup rather than waiting until it's needed

### Don't:
- **Return null**
   - *Example:* Return an empty collection or a special case object instead

- **Pass null to methods**
   - *Example:* Use overloaded methods or optional parameters instead

- **Use raw exception catches**
   - *Example:* Don't catch `Exception` when you can catch specific exception types

- **Ignore exceptions**
   - *Example:* Don't have empty catch blocks without comments explaining why

- **Let exceptions escape at inappropriate levels**
   - *Example:* Don't let low-level exceptions bubble up to UI layers

- **Use exceptions for normal flow control**
   - *Example:* Don't use try/catch as a substitute for if/else logic for expected conditions

## Testing Practices

### Do:
- **Follow the three laws of TDD**
   - *Example:* Write a failing test before writing implementation code

- **Keep tests clean and maintain them like production code**
   - *Example:* Refactor tests to keep them readable and maintainable

- **Make tests readable**
   - *Example:* Follow the Build-Operate-Check pattern for clear test structure
   ```java
   @Test
   public void shouldTransferMoney() {
       // given
       Account source = createAccountWithBalance(100);
       Account target = createAccountWithBalance(0);
       
       // when
       transferService.transfer(source, target, 50);
       
       // then
       assertEquals(50, source.getBalance());
       assertEquals(50, target.getBalance());
   }
   ```

- **Create a domain-specific testing language**
   - *Example:* Create helper methods that make tests express intent clearly

- **Make tests independent**
   - *Example:* Each test should be able to run alone

- **Test a single concept per test function**
   - *Example:* Test one aspect of behavior in each test method

### Don't:
- **Let test code rot**
   - *Example:* Don't ignore failing tests or let test quality deteriorate

- **Write tests with multiple assertions that test multiple concepts**
   - *Example:* Don't test both normal behavior and error handling in the same test

- **Create tests that depend on other tests**
   - *Example:* Don't make tests that must run in a specific order

- **Create tests with non-deterministic results**
   - *Example:* Avoid tests that sometimes pass and sometimes fail without code changes

- **Write tests that depend on specific environments**
   - *Example:* Don't create tests that only work on one developer's machine or with specific configuration

- **Skip tests when under pressure**
   - *Example:* Don't abandon testing when deadlines approach; this is when tests are most valuable

## Version Control

### Do:
- **Commit small, logical changes**
   - *Example:* Make separate commits for distinct changes rather than bundling unrelated modifications
   ```
   git commit -m "Fix validation bug in user registration"
   git commit -m "Update documentation for API endpoints"
   ```

- **Write meaningful commit messages**
   - *Example:* Describe what was changed and why
   ```
   git commit -m "Add password reset functionality for improved security"
   ```

- **Tag releases**
   - *Example:* Create version tags for significant releases
   ```
   git tag -a v1.2.3 -m "Version 1.2.3"
   ```

- **Use branches for features and fixes**
   - *Example:* Create a new branch for each feature or bugfix
   ```
   git checkout -b feature/user-profiles
   ```

- **Keep everything in source control**
   - *Example:* Store all code, configuration, documentation, and scripts in your repository

- **Review changes before committing**
   - *Example:* Check differences to ensure you're only committing intended changes

### Don't:
- **Commit broken code**
   - *Example:* Don't commit code that doesn't compile or pass tests

- **Make giant, sweeping commits**
   - *Example:* Don't bundle dozens of unrelated changes into a single commit with a vague message like "Updates"

- **Commit generated files**
   - *Example:* Don't add compiler output, temporary files, or build artifacts to version control

- **Work without version control**
   - *Example:* Don't develop on a local machine without backing up your work in a repository

- **Store credentials in the repository**
   - *Example:* Don't commit passwords, API keys, or other sensitive information directly in code

- **Leave commented-out code in commits**
   - *Example:* Don't commit code with large blocks of commented-out functionality—remove it or track it differently

## Code Layout and Formatting

### Do:
- **Use vertical formatting to separate concepts**
   - *Example:* Add blank lines between methods and between logical sections of code

- **Maintain vertical density for related concepts**
   - *Example:* Keep related lines of code close together without interruptions

- **Keep vertical distance between related items small**
   - *Example:* Define variables close to their first usage

- **Use horizontal white space to associate related things**
   - *Example:* `a = b + c` shows the association between variables with spacing

- **Keep lines short enough to read easily**
   - *Example:* Limit lines to 80-120 characters

- **Use consistent indentation**
   - *Example:* 2-4 spaces for each level of indentation, applied consistently

### Don't:
- **Break indentation**
   - *Example:* Don't try to fit small if statements on one line without proper indentation

- **Use horizontal alignment for variable declarations**
   - *Example:* Don't try to align all variable names in a column

- **Add too many blank lines**
   - *Example:* Don't add excessive vertical spacing that forces scrolling

- **Put multiple statements on the same line**
   - *Example:* Don't use semicolons to put multiple actions on one line

- **Use inconsistent styles within the same codebase**
   - *Example:* Don't mix different brace styles or indentation patterns

- **Create "walls of code" without visual breaks**
   - *Example:* Don't write 50 lines of code without any blank lines between logical sections

## Complexity Management

### Do:
- **Keep control structures simple and readable**
   - *Example:* Break complex conditionals into separate boolean variables or methods

- **Put the normal case after the if rather than after the else**
   - *Example:* 
   ```java
   if (error) {
       handleError();
   } else {
       normalProcessing();
   }
   ```

- **Make each loop perform one and only one function**
   - *Example:* Separate data loading from data processing into different loops

- **Use guards or early returns to reduce nesting**
   - *Example:* `if (!isValid) return;` at the beginning of a method to avoid wrapping the entire method in an `if` block

- **Simplify complicated tests with boolean function calls**
   - *Example:* 
   ```java
   if (IsLetter(inputCharacter)) {
       characterType = CharacterType_Letter;
   }
   else if (IsPunctuation(inputCharacter)) {
       characterType = CharacterType_Punctuation;
   }
   else if (IsDigit(inputCharacter)) {
       characterType = CharacterType_Digit;
   }
   ```

- **Include a final else clause to catch unexpected cases**
   - *Example:* 
   ```java
   if (condition1) {
       // handle condition1
   } else if (condition2) {
       // handle condition2
   } else {
       // handle unexpected case
   }
   ```

### Don't:
- **Create deeply nested control structures**
   - *Example:* Don't nest more than 3-4 levels of if statements—refactor into separate routines instead

- **Use complex boolean expressions**
   - *Example:* Don't create expressions like `if ((a && b) || (c && d && (e || f)))`. Break it into simpler conditions or create boolean functions

- **Create loops with multiple exit points**
   - *Example:* Avoid having multiple `break` statements within a single loop

- **Use gotos**
   - *Example:* Don't use gotos for normal flow control—use structured programming constructs instead

- **Overengineer solutions for hypothetical future problems**
   - *Example:* Don't build complex generic systems when only one specific use case is currently needed

- **Make code more complex than necessary**
   - *Example:* Don't use a complex design pattern when a simple approach would work well

## Concurrency

### Do:
- **Keep concurrency-related code separate from other code**
   - *Example:* Isolate threading concerns from business logic

- **Limit the scope of data shared between threads**
   - *Example:* Minimize the amount of shared mutable data

- **Use thread-safe collections properly**
   - *Example:* Use `ConcurrentHashMap` instead of `HashMap` in multithreaded contexts

- **Learn thread safety techniques and patterns**
   - *Example:* Understand and apply patterns like producer-consumer and readers-writers

- **Write tests specifically for concurrency issues**
   - *Example:* Create tests that expose race conditions and deadlocks

- **Acquire locks in a consistent order to prevent deadlocks**
   - *Example:* Always acquire lock A before lock B if both are needed

### Don't:
- **Assume code is thread-safe**
   - *Example:* Don't make assumptions about thread safety without verifying

- **Share data between threads without synchronization**
   - *Example:* Don't access shared mutable state without proper locks or atomic operations

- **Ignore the complexities of concurrency**
   - *Example:* Don't add threading as an afterthought

- **Create excessive synchronization**
   - *Example:* Don't lock more than necessary, which can lead to deadlocks

- **Neglect proper shutdown logic for concurrent activities**
   - *Example:* Don't leave threads running when they should be terminated

- **Use more threads than processors/cores for CPU-bound tasks**
   - *Example:* Don't create 100 threads on a 4-core machine for CPU-bound tasks

## Refactoring

### Do:
- **Refactor incrementally**
   - *Example:* Improve code in small, testable steps rather than massive rewrites

- **Use TDD to guide refactoring**
   - *Example:* Write failing tests for existing code, then refactor until they pass

- **Apply design patterns appropriately**
   - *Example:* Recognize when to apply patterns like Factory, Strategy, or Observer

- **Eliminate duplication**
   - *Example:* Extract common code into shared methods or superclasses

- **Create cohesive components**
   - *Example:* Group related functionality together in the same class or module

- **Look for and fix code smells**
   - *Example:* Address long methods, large classes, and excessive parameters

### Don't:
- **Refactor without tests**
   - *Example:* Don't make significant changes without test coverage to verify correctness

- **Apply patterns indiscriminately**
   - *Example:* Don't use design patterns just for the sake of using them

- **Accept technical debt without tracking it**
   - *Example:* Don't leave code issues unaddressed without at least documenting them

- **Allow large-scale refactorings to go on for too long**
   - *Example:* Don't start a three-month refactoring project without delivering value incrementally

- **Optimize prematurely**
   - *Example:* Don't complicate code for performance reasons without evidence that it's needed

- **Refactor and add features simultaneously**
   - *Example:* When refactoring, don't also change functionality; make separate commits for each
