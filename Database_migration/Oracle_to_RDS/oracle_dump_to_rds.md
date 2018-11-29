# Oracle Dump to RDS
Status of the doc: (work in progress)

## High level overview

 - Get DMP files
 - Setup a temporary Oracle DB
 - Copy DMP files into: /rdsdbdata/datapump
 - Create DB Link between the tempdb and the RDS
 - Copy the DMP files via the DB Link
 - Restore the DMP files on the RDS

## Setup a temporary Oracle DB

 - I used a ready cooked AMI from CLKWRK: https://aws.amazon.com/marketplace/pp/B07BMKB9Z3?ref=cns_1clkPro
 - Login to the instance
 - Switch to the user advised in the motd
 - source the DB.env from the scripts folder, run the start_all.sh
 - Copy the dump files to the default DATAPUMP directory

## Find out the DATAPUMP directory
 - On the Oracle Instance switch to the user as adviced at the motd
 - Source the DB.env from the scripts folder
 - sqlplus / as sysdba

```SQL
SELECT directory_name, directory_path from dba_directories ;
```

## List users
```SQL
SELECT * FROM all_users ;
```

## Download and install Oracle SQL Developer
Helps a lot when you noob like me:

https://www.oracle.com/database/technologies/appdev/sql-developer.html

## Create a DB Link to RDS
```SQL
create database link to_rds connect to <MASTER RDS USER> identified by "<MASTER RDS PASS>" using '(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=<RDS ENDPOINT>)(PORT=1521))(CONNECT_DATA=(SID=ORCL)))';
```

## Copy files
```SQL
BEGIN
DBMS_FILE_TRANSFER.PUT_FILE(
source_directory_object       => 'DATA_PUMP_DIR',
source_file_name              => '<SOURCE FILE>.dmp',
destination_directory_object  => 'DATA_PUMP_DIR',
destination_file_name         => '<DESTINATION FILE>.dmp', 
destination_database          => 'to_rds' 
);
END;
/ 
```

I just made a small hack with bash to copy all files from the DATAPUMP directory:

```bash
#!/bin/bash

for i in $(ls /oracle/CWDB01/admin/CWDB01/dpdump/*.dmp | xargs -i{} basename {}) ; do 
sqlplus / as sysdba << EOF
BEGIN
DBMS_FILE_TRANSFER.PUT_FILE(
source_directory_object       => 'DATA_PUMP_DIR',
source_file_name              => '${i}',
destination_directory_object  => 'DATA_PUMP_DIR',
destination_file_name         => '${i}', 
destination_database          => 'to_rds' 
);
END;
/ 
EOF
done
```
