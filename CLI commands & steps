# 1. Save the service credentials info from the IBM DB2 instance

# 2. Create a db2cli dsn (data source name - a simple name that refers to a data source)

You can use ```db2cli``` to access from the command line

First connect to the database using the following command (note this is a dummy command, replace with your credentials)
```
db2cli writecfg add -database dbname -host hostname -port 50001 -parameter "SecurityTransportMode=SSL"
```

Give a name to the data source you just created (again this is a dummy command)
```
db2cli writecfg add -dsn dsn_name -database dbname -host hostname -port 50001
```
