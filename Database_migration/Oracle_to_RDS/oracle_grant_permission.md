# Grant permission for whatever on whatever

```SQL
CREATE USER books_admin IDENTIFIED BY MyPassword;
GRANT CONNECT TO books_admin;
GRANT CONNECT, RESOURCE, DBA TO books_admin;
GRANT CREATE SESSION GRANT ANY PRIVILEGE TO books_admin;
GRANT UNLIMITED TABLESPACE TO books_admin;
GRANT SELECT, INSERT, UPDATE, DELETE ON schema.books TO books_admin;
```