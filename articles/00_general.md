# General Best practices

Best practices center around different scopes:
- Performance
- Readability
- Code Quality
- Testability

## Performance

The following best practices are ordered by importance.

### Try to solve a problem in SQL first

Solve your problem in this order: SQL first, then PL/SQL, then Java, then C.
Using SQL is the fastest way to operate with data in the Oracle Database, followed by PL/SQL. If your problem is not solveable by either SQL nor PL/SQL, try Java or C. If C is also not option, you will have to rethink your problem. :)

Solving problems in SQL does not mean to write complicated SQL rather than simple PL/SQL stored procedures, but to elaborate on SQL and utilize it's power and versatility. In most of the cases, if not any, it's easier, shorter and faster to have the database collect the data you require than to do it yourself.

Another point to remember is: Tell the database as precisely and completely as you can which data you need. It's by far better to write a Top-N-Analysis with an analytic function or the new Row Limiting Clause in version 12c than to simply order your data and only fetch the first N rows and discard the cursor. The more the database knows about the data you want, the more the database will do for you.

Put the other way round, it reads: The less you tell the database about the data you require, the more you're on your own in regard to performance tuning. If you dig into it, you won't believe how smart the optimizer is in supporting you, should you give it a chance.

### Really know your SQL

This is close to the first recommendation but this is to stress it's importance.

Stay informed about the newest development in SQL such as Multi Table Insert, Analytic Functions, Row Pattern Matching or the other possibilities. To write elegant and good performing SQL is the basis for all PL/SQL enhancements that follow. Digging into SQL is not a one time process but requires you to stay informed about latest improvements of new database versions, reading best practices from other SQL developers and getting to know nice SQL tricks that simplify your life.

### Control environment switches

This is a very important recommendation. Since database version 9, SQL and PL/SQL run in different environments. If you write a loop in PL/SQL and insert data into a table within that loop, the database must switch between the SQL and the PL/SQL environment. This is a tremendous cost factor you should try to avoid if it happens often. There are basically two technologies to avoid unnecessary environment switches:

- In PL/SQL you want to use bulk operations (`FORALL`, `BULK COLLECT INTO`) to reduce environment switches
- In SQL you want to include PL/SQL function calls in scalar subqueries, if possible [` ... where ename = (select v('P10_ENAME') from dual)`] to reduce the number of environment switches
- If at all possible, you want to avoid row triggers completely

A database trigger might seem appealing to use, but try to keep your fingers away from it. It causes a context switch and it executes row-by-row, which we learned not to do. If a trigger is unavoidable, at least make sure that there is no business logic in there and the lines of code are kept small. If possible, use a Table API instead of a trigger.

### Use bind variables

If you create a SQL statement outside the database or dynamically inside PL/SQL, you want to assure that you don't pipe together elements of the query that could be passed in by using bind variables as well. If you have to use dynamic SQL, you can't pass column or table names as bind variables, but you can use bind variables for search criteria and the like. 

Make sure you do. Reason is that any select or DML statement is stored in the library cache  within the SGA after it has been parsed to avoid unnecessary parses (which may last as long as the execution of the statement). In order to recognize a statement, a hash code is computed over the statetement text. If the text of the statement changes, it creates a different hash code and won't be recognized again. This not only leads to unnecessary parsing but wipes other reusable statements off the library cache, harming performance of other statements as well.

Full story: http://www.akadia.com/services/ora_bind_variables.html

### Let the database help you

PL/SQL has emerged from a simple macro language to a sophisticated, data oriented programming language. Version 11g brought an all new optimizer with many options you should utilize in your code. Letting the database help you means: let the compiler help you optimize your code. This can be achieved by switching the `PLSQL_OPTIMIZE_LEVEL` flag to it's maximum value (which is 3 in Version 12c). 

Doing so, the optimizer inlines helper methods into the code, removing unnecessary method calls, converts `for record in cursor` loops to bulk-optimized loops, restructures your code to remove unnecessary variable assignments and many more. The best is that you don't need to be aware of all the optimizations, it »just happens«!

Full story: http://www.oracle.com/technetwork/issue-archive/2012/12-sep/o52plsql-1709862.html

### Tell the database about your code

There are numerous possibilities to give the compiler extra information about the code you write. A function might be `deterministic`. If so, tell it. In prior versions, the deterministic clause had no big effect, but in the meantime it has. Therefore your code will benefit from these improvements without you having to do anything. The same will hold true for the new pragma `UDF` in version 12c, telling the database that you wrote a function that will be called from SQL most of the times. Telling the database about your code opens a possibility for the database to help you get maximum performance.

### Caching

There are new possibilities for caching select statements with the Result Cache or PL/SQL results with the Function Cache. Although being an Enterprise Edition option for now, if you have access to it, use it. Other »caching options« may include *Function Based Indexes* over deterministic functions or *Scalar Subqueries*, allowing SQL to only compute a function once per select statement.

### Native Compilation

Since version 9 it was possible to have the database compile your PL/SQL code in native C-code. It has been possible, but it wasn't easy. Starting with version 11, it's easy as well. The only thing you need to do is set session parameter `PLSQL_OPTIMIZE_LEVEL` to 2 at least (which should have happened after the last best practice anyway) and `PLSQL_CODE_TYPE` to `NATIVE`. Plus, there is a script to have the database recompile all internal packages natively. Ask your favourite DBA to help you on that.

### Review your Code

There are some instruments that help you to create efficient and good performing code. Look into the *Hierarchical Profiler* to gather an understanding of what is going on in your code, review execution plans by tracing them with *SQLTrace*, review code quality by collecting metadata about your code with *PL/Scope*. Finally, switch compiler warning on and review what they have to tell you. Remove unneeded variables, unreachable code etc.

Don't wait for performance problems to arise but code pro-actively with these best practices in mind. Ask yourself, if the code you write is short, precise and convincing. If it is not, it's almost certain sub optimal. If your procedures grow in length and complexity, re-think whether you have the chance to solve the problem from a different point of view. Sometimes it may turn out that you should adjust your data model, sometimes you may be able to prepare portions of the data with SQL. I recall having thrown away 600 lines of XML-DOM code in PL/SQL by replacing it with a five line `updatexml` statement. Look out for these opportunities

### Take time to refactor

If code grows older and older, chances are that there are improvements possible which could not have been taken into account in the days the code was created. In version 12 you may have heared already about the possibility of accessing sequences with *Identity Columns* or enhanced default column value possibilities without being forced into row triggers. If you can rely that your software runs on 12c, refactor unneeded row triggers away!

The same holds true for enhancements in SQL. Here are some possible candidates: `listagg` in Version 11.2, *Multi Table Insert*, `returning` clause, `log errors` claue, analytical functions, SQL/XML, XQuery support etc. Being informed about all these possibilities is only half the duty, the other half is to refactor this into the existing code. In PL/SQL, you want to have a look into features like object orientation, `case` statements and others.

## Readability

An underestimated best practice is to enforce readability of your code. One of the main task you will have over time after having coded a program is to refactor, fix or simply understand it. This is only possible if you do your maximum to write readable code. This holds true for SQL and PL/SQL as well. I don't want to tell you how exactly your code needs to be formatted to achieve this goal, but share some thoughts on creating readable code I found worth mentioning.

### SQL formatting
In SQL, any clause belongs to a new line. Avoid long rows with all clauses just after another. It may be acceptable with trivial statements but the fact is: You won't stick to a formatting convention if you don't do it ever. Therefore, even the most trivial statement should be formatted with care and readibility in mind.
Other important aspects include:
- Decide whether your key words shall be uppercase or lowercase and stick to this convention
- Define a pattern for whitespace. You may want to have your clauses left or right bound, upper- or lowercase or whatever else, but define a consistent system for you and your colleagues and make sure that no statement gets committed that does not adhere to these standards. You may want to use a beautifier tool for this is not a strict advice. If you format your code with care, this may be even more readable than an automatically formatted code.
- Put logical clauses on a new line. It's very easy to overlook an additional `and` if it hides in a long row
- Use brackets to avoid misunderstanding. Even if it's not strictly required, write brackets for your own clarity. This is important especially in the `where` clause when using `and`, `or` and `not` intermixed but also for mathematical calculations.
- Do your upmost to format complicated calculations as readable as possible. It will be almost impossible for you after just some days to exactly follow your flow of thoughts if the formatting is not readable
- Use comments if required, but only then. Comments should be used for points of interest only, not as a basic strategy for each and everything. If used too often, they start tiring the reader and important details will not be read. Avoid jokes and overly long sentences in comments. Simply put: Be precise and professional.

### PL/SQL formatting
In regard to formatting, the same best practices applies than for SQL. But there are some specific points to remember when working in PL/SQL.

#### Keep your methods short

The most important aspect to add to the rules given for SQL is that your methods shouldn't be too long. In object oriented programming, the goal has been set to not have methods longer than one screen size. You may want to encrease your screen now, but the fact remains: If a verbose language like Java sets a goal of one screen page, a compact language like PL/SQL should allow for even shorter methods.
Don't be forced into too long methods because you think that this is faster. If you create helper methods within your code, the compiler will incorporate them into the compiled code if possible (`PLSQL_OPTIMIZE_LEVEL = 2` or greater assumed). Plus, creating helper methods allows for a speaking name. A good chosen helper function name to me is better than a comment that explains what the following code is intended to do. Compare this comment:
```
  -- Check whether the username is correct
  <do something lengthy here>
  -- Check whether the user ahs necessary privileges
  <do something lengthy here>
```

with:

```
  check_username(p_username);
  check_user_has_privileges(p_username)
```

Most IDE allow to directly navigate to a helper method and finding a bug is easier as the helper method concentrates on one task only which normally makes its code more readable.

#### Avoid long SQL in PL/SQL
Incorporating SQL in PL/SQL feels a bit wrong: You can't develop a statement in PL/SQL, as you can't run it quickly. If you write a lengthy `select` statement in PL/SQL and put constants, bind variables etc into it, it's almost impossible to test it separately. Of course there are many situations where you can't avoid writing SQL in PL/SQL and I don't advocate against doing this at all. But you should take into consideration to remove a long SQL from PL/SQL by storing it in the database as a view and reference that view from within PL/SQL: Views are part of normal PL/SQL code and shouldn't be avoided just to have one selfcontained package.

A very close recommendation to that one is to have explicit `cursor` declarations instead of writing cursor loops with indented select statements. Rather do this:
```
declare
  cursor table_cur is
    select table_name
      from user_tables;
begin
  for tbl in table_cur loop
    .. do something
  end loop;
end;
```

than this:

```
begin
  for tbl in (select table_name from user_tables) loop
    .. do something
  end loop;
end;
```

This is especially true if your `select` statement grows in length and complexity. Think about using parameterized cursor to make them even more useful.

## Code Quality
Code quality refers to things you should implement in a specific way rather than another to get maximum performance and ease of understanding of the resulting code.

### Stick to SQL in SQL and to PL/SQL in PL/SQL
One general rule is to try and solve a problem in SQL first and then in PL/SQL. But if you are in PL/SQL anyway, changing to SQL to solve a problem incurs an environment switch (or many of them) which we try to avoid. As an example, here's a cool trick to split CSV text into table rows with SQL:

```
   with val as (
        select 'A:B:C:D:E' csv
          from dual)
 select regexp_substr(csv, '[^:]+', 1, level) t
   from val
connect by level <= regexp_count(csv, ':') + 1;
```

It's very tempting to implement this pattern in a PL/SQL pipelined function. But, as you are in PL/SQL already, it's better to implement it this way:

```
begin
  for i in 1 .. regexp_count(p_csv, ':') + 1 loop
    pipe row(regexp_substr(p_csv, '[^:]+', 1, i));
  end loop;
end;
```
The same holds true if you change your angle of view: It's very tempting for many programmers to write a little pipelined function to extract text from a csv-column. But here the SQL implementation would be the way to go ...

#### Use global variables carefully

In a package oder package body, variables may be defined at package level. Be careful with these variables, as they keep their value during their whole lifetime (if pragma `serially reusable` is not set, that is). This aspect of global variables may lead to bugs which are hard to find. Plus, if used inflationary, they will sum up in terms of memory consumption, as they will be stored per session. Don't ever used global variables to reduce the amount of parameters you have to pass around between methods and helper methods if you're not absolutely sure that no dangling values may be contained in these global variables.

Another reason to avoid global variables is that they make testing more complicated. If you expose a procedure or function for testing, you must make sure that no global variable is used, except when it is initialized _deterministically_ or in that procedure or function itself. Otherwise your tests could give different results just by starting them in a different sequence.

Don't use global variables or constants at package specification level directly. If a constant value needs to be changed later, it will force all packages referencing this package to recompile as well. Plus, a constant in a package is not accessible from SQL, as SQL is not aware of variables. Consider creating getter functions for a constant or store constants within database tables if you require them from within SQL. Alternatively, you may define constants as global variables and set their value during package initialization but in this case you loose their constant behaviour.

The same recommendation applies to any global cursor defined in a package. If you use them at all, make sure that you close them afterwards. Global cursors remain open for the whole lifetime of the package, just as any other global variable will do, consuming loads of memory if not carefully closed after usage.

#### Prefer *Cursor For Loops* over explicit cursor handling

Times have changed: It might have been advisable in earlier releases of the database to use explicit cursors (`declare - open - fetch - close`) over *Cursor For Loops*, but those times have passed by. Working with Cursor For Loops is not only faster (as the compiler implicitly converts them bulk operations) than explicit cursor but more readable as well. So if there is no strong reason for working with explicit cursors (A `cursor` expression in a `select` statement may be such a reason) then write the short, more elegant and in most cases faster Cursor For Loop.
