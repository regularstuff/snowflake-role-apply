* Project Goal - Role Maintenance
 
Script a process to deploy roles to a Snowflake database and run tests to verify that given roles do/do not have access to specific objects.

Possible approach for loading roles:

Updating a given file in S3 bucket triggers a lambda which applies the roles to snowflake instance:

  -- connection thru https://docs.snowflake.com/en/developer-guide/python-connector/python-connectorP

Alternately:
  call a lambda via http with a role document in call body (does lambda take PUT request?)

Process should drop a success/failure message in SQS

Probably the roles are just a string of GRANT and CREATE OR REPLACE sql statements, and idempotent logic.

------

Testing roles is more complex.

Say we write specs like:

```
{expected: fail, role: accounting_director_role, script: "CREATE TABLE ACTGDB.FDIC.FOO(id int);"}
```

Needs a throwaway environment where it can run statements.

Start in a test-specific  account by  dropping all customer-owned databases, 
   then apply the role document
   create the databases/tables
   and run the tests "
   
Can apply with 
 -  snowsql 
 -  https://github.com/Snowflake-Labs/snowcli
 -  regular old "snowflake connector for python"
   
   

SF Functional/Access Roles

Snowflake recommends arranging roles into two categories: 
	https://docs.snowflake.com/en/user-guide/security-access-control-considerations
	section: Aligning Object Access with Business Functions
  
  

--- secondary goals

goals for practicing with "Cloud Stuff"

Can Testing be orchestrated by step functions?

Can lambda permissions, step functions, buckets all be set up by IAC.
