# 1. Save the service credentials info from the IBM DB2 instance

# 2. Create a db2cli dsn (data source name - a simple name that refers to a data source)

You can use ```db2cli``` to access from the command line

First connect to the database using the following command (note this is a dummy command, replace with your credentials)
```
db2cli writecfg add -database dbname -host hostname -port 50001 -parameter "SecurityTransportMode=SSL"
```

Give a name to the data source you just created (again this is a dummy command)
This dsn name helps us to easily point to the IBM DB2 instance
```
db2cli writecfg add -dsn *this is where dsn_name goes* -database dbname -host hostname -port 50001
```

Below is a sample command - replace with credential values. In this case we the dsn name is 'production'
```
db2cli writecfg add -database BLUDB -host 0c77d6f2-5da9-48a9-81f8-86b520b87518.bs2io90l08kqb1od8lcg.databases.appdomain.cloud -port 31198 -parameter "SecurityTransportMode=SSL"

db2cli writecfg add -dsn production -database BLUDB -host 0c77d6f2-5da9-48a9-81f8-86b520b87518.bs2io90l08kqb1od8lcg.databases.appdomain.cloud -port 31198
```

Verify a db2cli dsn with the following generics syntax
```
db2cli validate -dsn alias -connect -user userid -passwd password
```

-----------------------------------------------------------------------------------------------------------------------------------------------------------

Once validation is complete, we can create a schema on the 'production' data warehouse

In this case we'll use a Star Schema from the following source - Download it using the following command
```
wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0260EN-SkillsNetwork/labs/Populating%20a%20Data%20Warehouse/star-schema.sql
```

Create the schema - use the following generic syntax
```
db2cli execsql -dsn dsn_name -user username -passwd password -inputsql sql_file.sql
```

Now we can populate the 'production' data warehouse - Download data files from the following links
```
wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0260EN-SkillsNetwork/labs/Populating%20a%20Data%20Warehouse/DimCustomer.sql

wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0260EN-SkillsNetwork/labs/Populating%20a%20Data%20Warehouse/DimMonth.sql

wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0260EN-SkillsNetwork/labs/Populating%20a%20Data%20Warehouse/FactBilling.sql

ls *.sql
```
We now have the following files downloaded and ready to be uploaded on to the 'production' data warehosue
```
DimCustomer.sql
DimMonth.sql
FactBilling.sql
```
Use the same command as above with for each sql files
```
db2cli execsql -dsn dsn_name -user username -passwd password -inputsql DimCustomer.sql
db2cli execsql -dsn dsn_name -user username -passwd password -inputsql DimMonth.sql
db2cli execsql -dsn dsn_name -user username -passwd password -inputsql FactBilling.sql
```

To verify the data in the data warehouse we'll use the following sql statement downloaded from the following link
```
wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DB0260EN-SkillsNetwork/labs/Populating%20a%20Data%20Warehouse/verify.sql
```

Use the generic syntax below to verify
```
db2cli execsql -dsn dsn_name -user username -passwd password -inputsql verify.sql
```
---------------------------------------------------------------------------------------------------------------------------------

To work with db2cli interactive command line direct use the following commands

Open the interactive command line
```
db2cli execsql -dsn dsn_name -user username -passwd password
```

SAMPLE MQT (MATERIALIZED VIEW) on the DB2 Cloud UI
The SQL syntax below is creating a 
```
CREATE TABLE avg_customer_bill (customerid, averagebillamount) AS
(select customerid, avg(billedamount)
from factbilling
group by customerid
)
     DATA INITIALLY DEFERRED
     REFRESH DEFERRED
     MAINTAINED BY SYSTEM;
``` 

To refresh the MQT
```
refresh table avg_customer_bill;
```

Quick check
```
select * from avg_customer_bill where averagebillamount > 11000;
```









