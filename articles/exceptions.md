---
layout: default
published: true
---

# Exceptions

## General

[An exception is an event, which occurs during the execution of a program, that disrupts the normal flow of the program's instructions](https://docs.oracle.com/javase/tutorial/essential/exceptions/definition.html).

The purpose of exceptions is to signal that human intervention *might* be necessary. Therefor all exceptions should be propagated to a point where human interaction is possible:
- Until they can be presented as a comprehensible error message to the enduser in the GUI
- Until all potentially usefull details have been registered in a logging table or file

PL-SQL programs **must** take care of all Oracle exceptions thrown by the database, they can also raise User-Defined exceptions to signal business rule violations.

First Rule: Avoid User-Defined exceptions at all *reasonable* costs. Use them only where the way a business rule violation must be handled will sometimes be different.

Second Rule: Where possible, use a logging utility package to take care of the bulk of exception handling. For an example see:
[Logging](https://github.com/Doag/DOAG-PL-SQL-Coding-Conventions/blob/gh-pages/articles/logging.md).

## Best Practices

### Rollback or commit as soon as possible
- Do neither in program code that might be used in a GUI, always try to leave the end-user the option to correct a problem.
- Rollback for unexpected exceptions. This should be default for the aforementioned logging utility. Normally you do not expect exceptions, you should never misuse exceptions to jump out of a block or loop for instance.
- Use savepoints to enable partial rollbacks in batch processes
- Specify either Rollback or Commit _before_ you raise a business rule violation in a batch process. At that point you as developer should know which.

### Choose a business rule strategy
The best way to handle business rules depends on the projekt scope:
- Small projects where the end-users are themselves developers: use named exceptions
- All other projekts: use an error message table

### Using an error message table
Errors are always raised with the combination of error number -20000, an error-code and optionally the identifying information from the _end-user_ perspective.
The error message table should contain a concise message from the end-user perspective and an optional longer explanation for new end-users.

### Using named exceptions
- The name you give it should show the reason for this exception
- Create one dedicated package specification for all named exceptions

### Things to avoid
- SQLCODE as it doesn't tell you anything directly
- SQLERRM as it is restricted to 512 bytes, which often leads to truncation of the error stack
- Never use WHEN OTHERS THEN NULL, because it makes all unexpected errors invisible
- Never use WHEN OTHERS THEN DBMS_OUTPUT.PUT_LINE(...), because that is equivalent to WHEN OTHERS THEN NULL when its output is discarded

### Logging 
- Use DBMS_UTILITY procedures FORMAT_ERROR_STACK, FORMAT_ERROR_BACKTRACE and FORMAT_CALL_STACK
- Log errors only at the last possible point, these DBMS_UTILITY procedures can handle that
- Do not reraise, that is only needed when you must log at intermediate points

### Second best logging practices
Sometimes you don't have the luxury to use DBMS_UTILITY. Either an older database, before Oracle 10g Release 2, or the local standards force you to use a logging package that still uses SQLERRM. In that case the rules to log as little as late as possible and not reraise must be replaced by these four rules:
- Reraise in any intermediate exception handler
- Always create an exception handler for public package components, to log relevant data before leaving the package
- Create exception handlers for all internal components that are used by more than one public component
- Create exception handlers for all deeply nested internal components, as SQLERRM can return only 512 bytes. Rule-of-thumb: use an exception handler when the nested call is 5 levels away from the closest enclosing exception handler.

## Links for further information and details:
[Feuerstein on PLSQL Errors](http://stevenfeuersteinonplsql.blogspot.de/2016/03/nine-good-to-knows-about-plsql-error.html)
