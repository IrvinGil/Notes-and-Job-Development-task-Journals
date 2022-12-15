#database

> About [[Flyway migrate]] 

Context about why are we using flyway and database migration on development practice is listed here.

> [!Question]
> Why are we using flyway migrate in our development practice?
> 	"We use flyway as a version control for our databases. For the reason that we can trace and have create a git-like record of the history of the database that are used in development"

### How Flyway works | Laymans term
The version of flyway migration files is executed by flyway, and it records and stores the migration scripts that it has executed (finished executing) on the "`schema_version`" table of the DB.
The reason for this is that flyway compares the migration script on the application that uses the database, and if that application has migration scripts, flyway will no longer run the script if it is recorded on the "`schema_version`" table (to not be redundant).

### Reasons on why errors on the flyway occur
Mainly on whitelabel main app:
- **Scenario**: You have run an old version of the code on your local machine, and then new changes are pushed on the remote machine, and then you pull the recent changes on your local machine.
	**Possible causes:**
	1. The previous version of the migration script was either changed (or its contents changed). This causes a mismatch in the migration scripts that you are trying to run since what is recorded in the "`schema_version`" of the local database does not match the script of the application that you are trying to run.
	2. Another cause of this is that there might hav been a db migration version (take for example V2) on the first branch you have checkout (then you have already run this branch on you local so the migration have been recorded on the `schema_version`) and then on the other there is also a V2 migration script. Considering these two V2 migration files has different contents so there will be an error.


### Context on the migration script organization by directory:

#### dev:
migration scripts that are under this directory is applied only to the local machine and is not applied to prod and staging. Some of the migration script here would be data insertion (data on testing on local: tenantzero, monopond, rise).


#### global:
db migration script that are under this directory is applied to all whether to dev, prod, and staging (as indicated by the keyword global).

#### prod:
db migration scripts under this directory is applied to prod only

#### staging:
db migration scripts under this directory is applied to staging only