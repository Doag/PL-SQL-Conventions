# Top 5 list of things you should keep in mind when programming in the Oracle Database

## 1. Solve your problem in this order: SQL first, then PL/SQL, then Java, then C.
Using SQL is the fastest way to operate with data in the Oracle Database, followed by PL/SQL. If your problem is not solveable by either SQL nor PL/SQL, try Java or C. If C also is not option, you will have to rethink about your problem. :)

## 2. Avoid context switches between SQL and PL/SQL
There are separate engines for SQL and PL/SQL in the Oracle Database. In certain occasions the database will switch from one engine to the other and these switches will cost you performance. For example, if you use a PL/SQL function in the WHERE clause of a SQL statement, the context switch will take place for each row! 

## 3. Use bind variables in SQL
Although it might seem tempting to write something like "SELECT * FROM emp WHERE empno = 10", be aware that this statement has to be parsed each time should empno change. By using a bind variable, the parsed query can be reused, so you better write something like "SELECT * FROM emp WHERE empno = pi_empno"

## 4. Use bulk operations
Remember: row-by-row, slow-by-slow. The database was designed to handle bulk data operations, so we better use it whenever possible. For example, forget looping through all rows row-by-row, better use FORALL.
http://www.oracle.com/technetwork/issue-archive/2012/12-sep/o52plsql-1709862.html

## 5. Avoid triggers
A database trigger might seem appealing to use, but try to keep your fingers away from it. It causes a context switch and it executes row-by-row, which we learned not to do. If a trigger is unavoidable, at least make sure that there is no business logic in there and the lines of code are kept small. If possible, use a Table API instead of a trigger.
