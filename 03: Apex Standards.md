## Apex Trigger Framework

### The "One Trigger Per Object" Design Pattern

The "One Trigger per Object" design pattern is the cornerstone of modern, enterprise-grade Apex development. The practice is to have a single, master trigger file for each object that is configured to run on every possible trigger scenario (e.g.,

before insert, after update, before delete). All business logic is then moved out of the trigger itself and into dedicated handler classes.

The rationale for this standard is based on a number of critical benefits:

- Predictable Execution Order: Salesforce does not guarantee the order of execution for multiple triggers on the same object. By consolidating all logic into one master trigger, developers gain total control over the sequence in which operations are performed, preventing inconsistent data states and unexpected behavior.
- Simplified Recursion Control: When multiple triggers exist, managing recursion (an infinite loop where a trigger updates a record, which in turn causes the trigger to fire again) becomes a complex and error-prone task. With a single trigger, a static Boolean flag in a helper class can be used to prevent re-entry, making recursion management simple and centralized.
- Modularity and Reusability: The business logic, now encapsulated in a handler class, can be reused in other contexts beyond triggers, such as in Batch Apex, Scheduled Apex, or even from a Flow via an invocable method.

### The Evolution of Frameworks: From First-Gen to Third-Gen

The "One Trigger per Object" pattern has evolved significantly over time.

- First Generation (Simple Triggers): This era was characterized by placing all logic directly inside a single trigger file, which led to code duplication and unpredictable execution.
- Second Generation (Trigger Handlers): This generation introduced the concept of handler classes, where the trigger file simply delegated logic to a separate class. This was a major leap forward, promoting the principle of separation of concerns.
- Third Generation (Framework-Driven Design): The modern standard is a framework that not only separates trigger and business logic but also decouples them entirely. These frameworks, such as Salesforce.org's Table-Driven Trigger Management (TDTM) and others, leverage Custom Metadata to store the handler class name in a configuration record. When the trigger runs, it dynamically instantiates the class name from the metadata record. This architectural approach is crucial for enterprise development because it breaks the technical dependency between the trigger and its business logic. This allows for cleaner deployments using unlocked packages, as trigger functionality can be distributed across multiple packages without causing hard dependencies between them.

### Core Components of a Modern Framework

A robust, third-generation trigger framework typically includes three core components:

- Trigger Entry Point: A lean, single trigger file per object that acts as a simple entry point for all DML operations. It should contain minimal code, primarily to call the handler class.
- Base Trigger Handler (Abstract Class): An abstract class that provides a standardized interface for all handler classes. It includes virtual methods for each trigger event (beforeInsert, afterInsert, beforeUpdate, etc.) and a single run method to orchestrate which method to call based on the trigger context.
- Specific Trigger Handler Class: A concrete class that extends the abstract base handler and contains the actual business logic for a specific object. It overrides the virtual methods to implement the required automation for each trigger event.

## Apex Code Style Guide

Consistency in code style is crucial for maintainability and reducing technical debt. A consistent style guide ensures that code is readable, predictable, and easy for any developer to understand and navigate.

### Naming Conventions

- Classes and Interfaces: Names should be in UpperCamelCase and are typically nouns or noun phrases (e.g., Character, ImmutableList). Test classes should follow the same convention but end with
- Test (e.g., AccountTriggerTest).
- Methods: Names should be in lowerCamelCase and are typically verbs or verb phrases (e.g., sendMessage).
- Constants: Names should use CONSTANT_CASE, which is all uppercase letters with words separated by underscores (e.g., NUMBER_OF_ACCOUNTS).
- Variables and Parameters: Names should be in lowerCamelCase and should be meaningful (e.g., computedValues). Single-character names should be avoided unless a standard loop iterator.
- Properties: Names should be in UpperCamelCase.

### Code Formatting

- Braces: Braces should always be used with if, else, for, do, and while statements, even when the body contains only a single statement. The recommended style is Kernighan and Ritchie (K&R), where the opening brace is on the same line as the statement, followed by a line break.
- Indentation: A consistent indentation of 4 spaces is recommended for each new block or block-like construct.
- Column Limit: A column limit of 120 characters is recommended to ensure code readability and prevent horizontal scrolling.

## Bulkification and Governor Limit Management

Bulkifying Apex code is the practice of designing the code to efficiently handle multiple records in a single transaction. This is a fundamental principle for avoiding governor limits, which are strict runtime limits imposed by the Salesforce platform. A common anti-pattern is to place SOQL queries or DML statements inside a

for loop, as this will result in a query or DML operation for every record in the loop, quickly exceeding limits.

The correct approach is to move database operations outside the loop. First, iterate through the records to collect the necessary data, such as Ids, into a Set. Then, perform a single SOQL query on that Set. The same principle applies to DML operations: collect the records to be updated or created into a List and then perform a single update or insert statement on the entire list.
