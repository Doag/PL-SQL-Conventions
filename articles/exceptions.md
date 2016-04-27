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

## Best Practices

### Always rollback for unexpected exceptions
This should be part of the aforementioned logging utility. Normally you do not expect exceptions, you should never misuse exceptions to jump out of a block or loop for instance.  

### Always define a name for more or less expected exceptions 
- Normally exceptions should be just that: unexpected
- The name you give it should show why this exception warrants non-standard treatment 
- Always explicitly specify either Rollback or Commit
- Never define exceptions in the body of a package 
- Create one dedicated package specification for all named exceptions
 
### Logging 
- Use a logging package that uses DBMS_UTILITY.FORMAT_ERROR_STACK and DBMS_UTILITY.FORMAT_ERROR_BACKTRACE
- Log errors only at the last possible point, these DBMS_UTILITY procedures can handle that
- Do not reraise, that is only needed when you must log at intermediate points
- Never use SQLERRM and SQLCODE
- Never use WHEN OTHERS THEN NULL;
- Never use WHEN OTHERS THEN DBMS_OUTPUT.PUT_LINE.. without a reraise

### Second best logging practices
Sometimes you don't have the luxury to use DBMS_UTILITY. Either an older database, before Oracle 10g Release 2, or the local standards force you to use a logging package that still uses SQLERRM. In that case the rules to log as little as possible and not reraise must be replaced by these four rules:
- Reraise in any intermediate exception handler
- Always create an exception handler for public package components, to log relevant data before leaving the package
- Create exception handlers for all internal components that are used by more than one public component
- Create exception handlers for all deeply nested internal components, as SQLERRM can return only 512 bytes. Rule-of-thumb: use an exception handler when the nested call is 5 levels away from the closest enclosing exception handler.

## Links for further information and details:
[Feuerstein on PLSQL Errors](http://stevenfeuersteinonplsql.blogspot.de/2016/03/nine-good-to-knows-about-plsql-error.html)
