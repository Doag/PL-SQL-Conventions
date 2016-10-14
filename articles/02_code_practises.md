# Code Practises

## Create a stub first
```
CREATE OR REPLACE PACKAGE str_pkg
IS
   /* Description 
   *  Author
   *  Parameter
   */
   PROCEDURE between
   ( pi_str_in   IN  VARCHAR2
   , pi_start_in IN  PLS_INTEGER
   , pi_end_in   IN  PLS_INTEGER
   , po_str_out  OUT VARCHAR2
   );
END str_pkg;
```
Next, I add only the following code to the package body:
```
CREATE OR REPLACE PACKAGE BODY str_pkg
IS
   PROCEDURE between
   ( pi_str_in   IN  VARCHAR2
   , pi_start_in IN  PLS_INTEGER
   , pi_end_in   IN  PLS_INTEGER
   , po_str_out  OUT VARCHAR2
   )
   IS
   BEGIN
     # Start your work here and comment it
     null;
   
   EXCEPTION
     WHEN OTHERS
     THEN
       # use your favorite logger tool here before raising an exception for system errors
       raise;
   END between;
END str_pkg;
```

## Avoid hardcoding of system variables, IDs and VARCHAR2 length
- System variables that depend on your environment should never be hardcoded in PL/SQL
- Columns that end with _ID usually contain technical IDs. Don't hardcode these in your PL/SQL, but use a procedure to get them
- Make use of %type and %rowtype. (avoid hardcoding the length of a varchar2 column)

## When writing dynamic queries, making use of "q" will lead to much more maintainable code:
```
CREATE OR REPLACE PACKAGE BODY query_builder_pkg
IS

PROCEDURE dynamic_query
IS

  v_sql  VARCHAR2(1024);
  v_cnt  PLS_INTEGER;

BEGIN
  v_sql := q'[SELECT COUNT(*) FROM user_objects WHERE object_type = 'TABLE']';
  EXECUTE IMMEDIATE v_sql INTO v_cnt;
  DBMS_OUTPUT.PUT_LINE(TO_CHAR(v_cnt) || ' tables in USER_OBJECTS.');

EXCEPTION
  WHEN OTHERS
  THEN
    # use your favorite logger tool here before raising an exception for system errors
    raise;
END dynamic_query;

END query_builder_pkg;
```
