# Code Practises

## Some code practices

### Create a stub first
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

### Never hardcode system variables
