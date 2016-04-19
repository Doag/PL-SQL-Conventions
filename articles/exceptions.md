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

PL-SQL programs **must** take care of all Oracle exceptions, they can also use User-Defined exceptions.

First Rule: Avoid User-Defined exceptions at all *reasonable* costs.

Second Rule: Always use a logging utility package to take care of the bulk of exception handling. For an example see:
[Logging](https://github.com/Doag/DOAG-PL-SQL-Coding-Conventions/blob/gh-pages/articles/logging.md).

## Specifics

### Always rollback for unexpected exceptions
This should be part of the aforementioned logging utility. Normally you do not expect exceptions, you should never misuse exceptions to jump out of a block or loop for instance.  

### Always define a name for more or less expected exceptions 
- Normally exceptions should be just that: unexpected
- The name you give it should show why this exception warrants non-standard treatment 
- Always explicitly specify either Rollback or Commit
- Never define exceptions in the body of a package 
 
### Best practices
- Create one dedicated package specification for all named exceptions
- Always create an exception handler for public package components, to log relevant data before leaving the package
- Never use WHEN OTHERS THEN NULL;
- Never use WHEN OTHERS THEN DBMS_OUTPUT.PUT_LINE.. without a reraise

#### Exception handlers for internal package components
Use exception handlers for all internal components:
- that are used by more than one public component
- that pass calls to such an internal component


## Links for further information and details:
[Feuerstein on PLSQL Errors](http://stevenfeuersteinonplsql.blogspot.de/2016/03/nine-good-to-knows-about-plsql-error.html)
