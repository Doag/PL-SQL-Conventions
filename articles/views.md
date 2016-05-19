# Views

## General

Access to data is either given through views or a table API, but never directly on the tables.

If the view is for read only purposes only, always add ```with read only```. 
To make sure that the where clause is respected when doing an update on the view, use ```with check option```.

```
create or replace view a00424_p00200_vw
as
 select emp.emp_name emp_name
     , dep.dep_name dep_name
  from employees    emp
  join departments  dep on (emp.dep_id = dep.dep_id)
 where upper(dep.dep_name) = 'ACCOUNTING'
 with check option
 ;
```
