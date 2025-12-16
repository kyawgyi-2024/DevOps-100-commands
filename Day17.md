# DevOps Day-17: PostgreSQL – User & Database Setup

PostgreSQL is best for complex, enterprise-grade workloads; MySQL is best for fast, read-heavy web applications.
PostgreSQL = powerful & strict, MySQL = simple & fast.

1. Login to Database Server
ssh peter@stdb01                              # SSH login to PostgreSQL DB server
# Enter password

2. Switch to PostgreSQL Admin User
sudo su - postgres                            # Switch to postgres OS user
# Enter sudo password if prompted
(postgres is the default superuser for PostgreSQL.)

3. Enter PostgreSQL Shell
psql                                          # Open PostgreSQL CLI
# psql 13.14

4. Create Database User (Role)
CREATE USER koko WITH PASSWORD '2026kkz';       # CREATE ROLE
(Creates a PostgreSQL role/user named koko.)

5. Create Database
CREATE DATABASE kokodb_01;                      # CREATE DATABASE
(Creates a new database named kokodb_01.)

6. Grant Permissions to User
GRANT ALL PRIVILEGES ON DATABASE kokodb_01 TO koko; # GRANT(Means: Permissions granted successfully.)
(Gives full access on database kokodb_01 to user koko.)

7. Verify User & Database
\du                                          # List all database users (roles)
\l                                           # List all databases

8. Connect to Database & Check Tables
\c kokodb_01                                 # Connect to database(You are now connected to database "kokodb_01" as user "postgres".
)
\dt                                          # List tables (empty if new DB)

9. Exit PostgreSQL & Logout
\q                                           # Exit psql shell
exit                                          # Exit postgres user
exit                                          # Logout from server

===============================================================================================================
# Key Concepts (Interview Ready)

User (Role): Controls authentication & permissions

Database: Logical container for tables & data

Grant: Assigns privileges to users

psql → PostgreSQL command-line tool

postgres → Default superuser

===============================================================================================================
# Interview One-Liners

Create user: CREATE USER name WITH PASSWORD 'pwd';

Create DB: CREATE DATABASE db_name;

Grant access: GRANT ALL PRIVILEGES ON DATABASE db TO user;

Verification: \du, \l, \dt

===============================================================================================================
PostgreSQL vs MySQL (Interview Comparison)

| Feature                | **PostgreSQL**                               | **MySQL**                         |
| ---------------------- | -------------------------------------------- | --------------------------------- |
| Type                   | Advanced Object-Relational DB (ORDBMS)       | Relational DB (RDBMS)             |
| License                | Open-source (PostgreSQL License)             | Open-source (Oracle owned)        |
| ACID Compliance        | **Full ACID**, very strict                   | ACID (InnoDB only)                |
| Performance            | Best for **complex queries & analytics**     | Best for **read-heavy workloads** |
| JSON Support           | **Very strong** (JSONB, indexing)            | Basic JSON support                |
| Concurrency            | Excellent (MVCC)                             | Good, but less advanced           |
| Extensibility          | Highly extensible (custom types, extensions) | Limited extensibility             |
| Replication            | Logical + Physical replication               | Master-slave, group replication   |
| Index Types            | B-tree, Hash, GIN, GiST, BRIN                | Mostly B-tree                     |
| Default Storage Engine | Single powerful engine                       | Multiple engines (InnoDB default) |
| Compliance             | Enterprise-grade (banking, govt)             | Widely used for web apps          |

===================================================================================================================
When to Use PostgreSQL

✅ Complex queries
✅ Large datasets
✅ High data integrity requirements
✅ Analytics & reporting
✅ JSON / semi-structured data
✅ Enterprise-grade systems

Examples: Banking, ERP, SaaS backends, data platforms

==================================================================================================================

When to Use MySQL

✅ Simple & fast setups
✅ Read-heavy applications
✅ Traditional web apps
✅ LAMP stack projects
✅ Easier learning curve

Examples: Blogs, CMS, small-to-medium websites

==============================================================================================================
# DevOps Perspective

| Area          | PostgreSQL                     | MySQL               |
| ------------- | ------------------------------ | ------------------- |
| Backup        | `pg_dump`, `pg_basebackup`     | `mysqldump`         |
| Monitoring    | pg_stat views                  | Performance Schema  |
| Scaling       | Vertical + logical replication | Easy read replicas  |
| Cloud Support | AWS RDS, GCP, Azure            | AWS RDS, GCP, Azure |

==============================================================================================================

