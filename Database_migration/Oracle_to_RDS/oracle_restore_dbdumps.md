# How to restore dmp files on Oracle
First get the files into the datapump directory

## Pre-create table spaces

```SQL
 create tablespace TABLESPACE_NAME datafile size 10G autoextend on maxsize 20G;
 ```

 **NOTE:** kind of a rule of thumb for index size allocation: 
 """so you can reserve 80% but create smaller (I think about 50%) with extend"""