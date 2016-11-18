# Deployment of PL/SQL

## Continue processing even if the object already exists.
```
begin
  execute immediate
    'CREATE SEQUENCE SEQ1 INCREMENT BY 1 START WITH 1 MAXVALUE 9999 MINVALUE 1';

exception
  when others
  then
    if sqlcode = -955 then
      null;
    else
      raise;
    end if;
end;
```

## Edition-Based Redefinition (EBR)
TODO
