# Oracle Dump to RDS

## High level overview

 - Get DMP files
 - Setup a temporary Oracle DB
 - Copy DMP files into: /rdsdbdata/datapump
 - Create DB Link between the tempdb and the RDS
 - Copy the DMP files via the DB Link
 - Restore the DMP files on the RDS

 ## Setup a temporary Oracle DB

  - I used AWS Linux
  - Oracle Express Edition:  [LINK](https://www.oracle.com/technetwork/database/database-technologies/express-edition/downloads/index.html)

