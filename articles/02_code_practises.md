# Code Practises

## Some code practices

### Create a stub first
```
PACKAGE tb_string_utils
     IS
        /* Lots of other, existing programs above. */
        /* Return substring between start and end locations */
        FUNCTION betwnstr (
           string_in IN VARCHAR2
         , start_in IN PLS_INTEGER
         , end_in IN PLS_INTEGER
        )
           RETURN VARCHAR2;
     END tb_string_utils;
```
Next, I add only the following code to the package body:
```
     PACKAGE BODY tb_string_utils
     IS
        /* Lots of other, existing programs above. */
        /* Return substring between start and end locations */
        FUNCTION betwnstr (
           string_in IN VARCHAR2
         , start_in IN PLS_INTEGER
         , end_in IN PLS_INTEGER
        )
           RETURN VARCHAR2
IS
```

### Never hardcode system variables
