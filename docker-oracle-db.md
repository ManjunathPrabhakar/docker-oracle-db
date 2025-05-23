# docker-oracle-db
# How to pull and use oracle db with Docker

Youtube : https://www.youtube.com/watch?v=56dSXI2PbCQ&t=148s

# ORACLE REGISTRY
* Navigate to [Oracle Registry](https://container-registry.oracle.com/) and Click on Database => [Oracle Registry - Database](https://container-registry.oracle.com/ords/f?p=113:1:109357956445442:::1:P1_BUSINESS_AREA:3&cs=35BGwXWAd4f7zWW_1dXUxhR8-cyfhq5oa1Zwa35p9qI3moDqVy_y5VNifGa8D_wbl1tGILl9wWy3e1GUuGqqKXA)
* Select the "Free" Repository "Oracle Database Free" => [Oracle Database Free - Repository Detail](https://container-registry.oracle.com/ords/f?p=113:4:109357956445442:::4:P4_REPOSITORY,AI_REPOSITORY,AI_REPOSITORY_NAME,P4_REPOSITORY_NAME,P4_EULA_ID,P4_BUSINESS_AREA_ID:1863,1863,Oracle%20Database%20Free,Oracle%20Database%20Free,1,0&cs=36i9xwf9rwgs-jSciryRk6wugewGURfJoDq1p5nLtMLQ9yYGnOhpeAd11JD5Lstt5t4h_pZ4NOPa-VlPcD_EVyQ)
* Copy the pull command : docker pull container-registry.oracle.com/database/free:latest
* VERSION AT THE TIME OF THIS : Version 23.7.0.25.01

# DOCKER STEPS
* Go to Docker Terminal enter
  > docker login container-registry.oracle.com
* it'll ask for username and password (Credentials used to sign in to container-registry.oracle.com) - you'll see login succeeded
* paste command
  > docker pull container-registry.oracle.com/database/free:latest
* wait for the pull to be successful and complete
* Now youll see an image in DOcker Desktop with name "container-registry.oracle.com/database/free:latest"
* Click on Run on that Image, and Give a container name(oracle-free-db), set host port(use same as it shows i.e 1521), In Environment Variables (enter ORACLE_PWD = password(or your own password))
* More Details on the Oracle Database Free Docker Image : [Oracle Database Free - Repository Detail](https://container-registry.oracle.com/ords/f?p=113:4:109357956445442:::4:P4_REPOSITORY,AI_REPOSITORY,AI_REPOSITORY_NAME,P4_REPOSITORY_NAME,P4_EULA_ID,P4_BUSINESS_AREA_ID:1863,1863,Oracle%20Database%20Free,Oracle%20Database%20Free,1,0&cs=36i9xwf9rwgs-jSciryRk6wugewGURfJoDq1p5nLtMLQ9yYGnOhpeAd11JD5Lstt5t4h_pZ4NOPa-VlPcD_EVyQ)
* The Oracle Database 23ai Free Container Image contains a pre-built database, so the startup time is very fast. Fast startup can be helpful in CI/CD scenarios. To start an Oracle Database Free container, run the following command where,<oracle-db> can be any custom name for the container:
  > docker run -d --name oracle-free-db container-registry.oracle.com/database/free:latest
* When the container is started, a random password is generated for the <b>SYS</b>, <b>SYSTEM</b> and <b>PDBADMIN</b> users. This is termed the default password.

# To connect to the Oracle Database by running a SQL*Plus command from within the container using one of the following commands: (once you have started the Container)
## This opens sqlplus in the terminal
* docker exec -it <oracle-db> sqlplus sys/<your_password>@FREE as sysdba
* docker exec -it <oracle-db> sqlplus system/<your_password>@FREE
* docker exec -it <oracle-db> sqlplus pdbadmin/<your_password>@FREEPDB1

# To connect from outside of the container example SQLDeveloper, run the following connection string:
 ## To connect to the database at the CDB$ROOT level as sysdba:
   > sqlplus sys/<your password>@//localhost:<port mapped to 1521>/FREE as sysdba
   >> username: sys as SYSDBA, password: password, host: localhost, port: 1521. servicename: FREE
 ## To connect as non sysdba at the CDB$ROOT level:
   > sqlplus system/<your password>@//localhost:<port mapped to 1521>/FREE
   >> username: system, password: password, host: localhost, port: 1521. servicename: FREE
 ## To connect to the default Pluggable Database (PDB) within the FREE Database:
   > sqlplus pdbadmin/<your password>@//localhost:<port mapped to 1521>/FREEPDB1
   >> username: pdbadmin, password: password, host: localhost, port: 1521. servicename: FREEPDB1


# ########################################################################

* In SQL Developer use, you can create individual connects for all 3
  > username: sys as SYSDBA, password: password, host: localhost, port: 1521. servicename: FREE
  > username: system, password: password, host: localhost, port: 1521. servicename: FREE
  > username: pdbadmin, password: password, host: localhost, port: 1521. servicename: FREEPDB1

### Example: 
<img width="570" alt="image" src="https://github.com/user-attachments/assets/2b8291ca-bc48-4160-bb0b-5b64d272d80e" />
Similarly, use above users SYSTEM / PDBADMIN to create connections in SQL Developer if needed.

## TO CREATE TEST DATABASE / USERS

### OT USER CREATION [ORDER TRANSACTIONS] ![image](https://github.com/user-attachments/assets/1c622a80-e13a-4870-a04a-eff8042efa58)
* Login to "sys as SYSDBA"
* REFER : https://www.oracletutorial.com/getting-started/create-oracle-sample-database-for-practice/
* Switch to Pluggable DB from SYSDBA:
  > ALTER SESSION SET CONTAINER = FREEPDB1;
* verify
  > SHOW con_name; >>result is "FREEPDB1"
* create user
  > CREATE USER OT IDENTIFIED BY Orcl1234;
* and grant priviliges
  > GRANT CONNECT, RESOURCE, DBA TO OT;
  > # DONT FORGET TO COMMIT
* Now create another connection in SQL Developer with
  >> username: OT, password: Orcl1234, host: localhost, port: 1521. servicename: FREEPDB1
* Download the same Table and Data : [Order Transactions Zip](https://oracletutorial.com/wp-content/uploads/2019/01/oracle-sample-database.zip)
* And Run the sql files.
* # DONT FORGET TO COMMIT

### HR USER CREATION [HUMAN RESOURCES]
* Similarly, we can create HR
* Login to "sys as SYSDBA"
* Switch to Pluggable DB from SYSDBA:
  > ALTER SESSION SET CONTAINER = FREEPDB1;
* verify
  > SHOW con_name; >>result is "FREEPDB1"
* create user
  > CREATE USER HR IDENTIFIED BY Orcl1234;
* and grant priviliges
  > GRANT CONNECT, RESOURCE, DBA TO HR;
  > # DONT FORGET TO COMMIT
* Now create another connection in SQL Developer with
  >> username: HR, password: Orcl1234, host: localhost, port: 1521. servicename: FREEPDB1
* Download the same Table and Data : [https://www.oracletutorial.com/getting-started/oracle-sample-database/](https://github.com/oracle-samples/db-sample-schemas/tree/main/human_resources)
* And Run the sql files.
* # DONT FORGET TO COMMIT
  

#### SIMILARLY
##### * CUSTOMER ORDERS[CO] => [https://github.com/oracle-samples/db-sample-schemas/tree/main/customer_orders](https://github.com/oracle-samples/db-sample-schemas/tree/main/customer_orders)
##### * SALES HISTORY[SH] => [https://github.com/oracle-samples/db-sample-schemas/tree/main/customer_orders](https://github.com/oracle-samples/db-sample-schemas/tree/main/sales_history)
##### * ORDER ENTRY[OR] => [https://github.com/oracle-samples/db-sample-schemas/tree/main/customer_orders](https://github.com/oracle-samples/db-sample-schemas/tree/main/sales_history)](https://github.com/oracle-samples/db-sample-schemas/tree/main/order_entry)
##### * PRODUCT MEDIA[PM] => https://github.com/oracle-samples/db-sample-schemas/tree/main/product_media




