# Deployment of PL/SQL

## Continue processing even if the object already exists. Explicitly check if the object already exists and don't just try to execute a certain statement.
```
begin
  select count(1)
  from   user_indexes
  where  t.table_name = c_table
  and    i.index_name = c_index_name
  ;

  if l_count = 0
  then
    execute immediate ('CREATE...');
    dbms_output.put_line('INFO:' || ' was executed');
  else
    dbms_output.put_line('WARNING: ' || c_action || ' already executed');
  end if;

exception
  when others
  then
    dbms_output.put_line('ERROR: ' || c_action || ' raised an error');
end;
```

## Zero downtime requirement? Use Edition-Based Redefinition (EBR)
A good starting point for EBR can be found here: https://blogs.oracle.com/plsql-and-ebr/entry/welcome_to_the_pl_sql
