# Tutorial: Connecting to IBM DB2 Database in App Connect Enterprise

## Overview
This tutorial teaches you how to connect IBM App Connect Enterprise to an IBM DB2 database. You'll learn to configure database policies, import database-enabled projects, execute SQL queries, and retrieve data from DB2 through REST API endpoints.

## Learning Objectives
By the end of this tutorial, you will be able to:
- Import pre-configured database projects
- Configure DB2 database policies
- Set up JDBC connections to DB2
- Deploy database-enabled applications
- Execute SQL queries from message flows
- Test database operations through REST APIs
- Handle database responses in JSON format

## Prerequisites
Before starting this tutorial, ensure you have:
- Completed Tutorial 1: Create REST API
- Completed Tutorial 2: Data Transformation
- IBM App Connect Enterprise Toolkit 13.0.6.0 installed
- Access to an IBM DB2 database instance
- DB2 JDBC driver (db2jcc4.jar)
- Database credentials (username, password, server, port)
- Basic understanding of SQL queries
- Postman or similar REST client for testing

## Tutorial Duration
Approximately 45-60 minutes

---

## Part 1: Understanding Database Integration

### Why Connect to Databases?
Integrating ACE with databases enables:
- **Data Persistence**: Store and retrieve business data
- **Real-time Queries**: Access current data for processing
- **CRUD Operations**: Create, Read, Update, Delete records
- **Data Validation**: Verify information against stored data
- **Reporting**: Generate reports from database queries
- **Transaction Management**: Ensure data consistency

### Database Integration Architecture

```
REST API Request
      ↓
ACE Integration Server
      ↓
Database Policy (Connection Config)
      ↓
JDBC Driver
      ↓
DB2 Database
      ↓
Query Results
      ↓
JSON Response
```

---

## Part 2: Importing the Database Project

### Step 1: Access Import Function

![File Menu - Import](image/image-1.png)

1. In ACE Toolkit, go to **File** menu
2. Select **Import...**
3. The Import wizard will open

**What's happening:** The Import function allows you to bring existing projects, libraries, and resources into your workspace. This is useful for sharing projects between team members or using pre-configured templates.

---

### Step 2: Select Project Interchange

![Import Project Interchange](image/image-3.png)

1. In the Import wizard, expand **IBM Integration**
2. Select **Project Interchange**
3. Click **Next**

**What's happening:** Project Interchange is ACE's format for exporting and importing complete projects with all their resources, configurations, and dependencies. It packages everything needed to run the integration.

---

### Step 3: Browse for Project File

![Select Import File](image/image-3.png)

1. Click **Browse** to locate the project file
2. Navigate to the workshop materials folder
3. Select the database project ZIP file (e.g., `ExampleDatabaseRefine.zip`)
4. Click **Next** to proceed

**What's happening:** The project file contains pre-configured message flows, database queries, and policy configurations that demonstrate database connectivity patterns.

---

### Step 4: Complete Import

1. Review the projects to be imported
2. Ensure all required projects are selected
3. Click **Finish**
4. Wait for the import to complete
5. The project will appear in your Application Development view

**What's happening:** ACE extracts the project contents and adds them to your workspace, making all resources available for editing and deployment.

---

## Part 3: Configuring Database Policy

### Step 5: Open Database Policy

![Database Policy Configuration](image/image-7.png)

1. In the Application Development view, expand your imported project
2. Navigate to **Policies** folder
3. Locate **MyJDBCPolicy.policyxml**
4. Double-click to open the policy editor

**What's happening:** A database policy defines connection parameters for accessing databases. It centralizes configuration, making it easy to change database settings without modifying message flows.

---

### Step 6: Configure Policy Properties

![Policy Properties](image/image-7.png)

Configure the following properties in the policy editor:

**Basic Information:**
- **Name**: MyJDBCPolicy
- **Type**: JDBC Providers
- **Template**: DB2_91_Windows

**Database Configuration:**
- **Name of the database**: USERS
- **Type of the database**: DB2 Universal Database
- **Version of the database**: 11.1
- **JDBC driver class name**: com.ibm.db2.jcc.DB2Driver

**Connection Details:**
- **Connection URL format attribute 1**: jdbc:db2://[serverName]:[portNumber]/[databaseName]/currentSchema=MYSCHEMA;user=[user];password=[password]
- **Connection URL format attribute 2**: (Additional connection parameters)
- **Connection URL format attribute 3**: (Additional connection parameters)
- **Connection URL format attribute 4**: (Additional connection parameters)
- **Connection URL format attribute 5**: (Additional connection parameters)

**Server Configuration:**
- **Database server name**: 52.118.195.00
- **Database server port number**: 8080
- **Type 4 driver class JAR URL**: C:\Program Files\IBM\SQLLIB\java\db2jcc4.jar

**Additional Settings:**
- **Name of the database schema**: MYSCHEMA
- **Data source description**: (Optional description)
- **Maximum size of connection pool**: 0 (unlimited)
- **Security identity (DSN)**: mydbidentity
- **Environment parameters**: (Optional)
- **Supports XA coordinated transactions**: false
- **Use JAR files that have been deployed in a BAR file**: false

**What's happening:** These settings tell ACE how to connect to your DB2 database, including the server location, authentication method, and JDBC driver location.

---

### Step 7: Understanding Connection URL Format

The connection URL follows this pattern:
```
jdbc:db2://[serverName]:[portNumber]/[databaseName]/currentSchema=[schema];user=[user];password=[password]
```

**Components:**
- **jdbc:db2://**: Protocol for DB2 JDBC connection
- **serverName**: Database server hostname or IP (e.g., 52.118.195.00)
- **portNumber**: Database port (e.g., 8080)
- **databaseName**: Name of the database (e.g., USERS)
- **currentSchema**: Default schema to use (e.g., MYSCHEMA)
- **user**: Database username
- **password**: Database password

**Example:**
```
jdbc:db2://52.118.195.00:8080/USERS/currentSchema=MYSCHEMA;user=admin;password=secret123
```

---

### Step 8: Configure JDBC Driver Path

**Important:** Ensure the JDBC driver JAR file exists at the specified path:
```
C:\Program Files\IBM\SQLLIB\java\db2jcc4.jar
```

**If the driver is in a different location:**
1. Update the **Type 4 driver class JAR URL** field
2. Use the full path to your db2jcc4.jar file
3. Ensure the Integration Server has read access to this file

**Where to get the DB2 JDBC driver:**
- Included with DB2 client installation
- Download from IBM website
- Available in DB2 installation directory: `<DB2_HOME>/java/db2jcc4.jar`

---

### Step 9: Save Policy Configuration

1. After configuring all properties, click **File** → **Save** (or Ctrl+S)
2. Verify no validation errors appear
3. The policy is now ready to use

**What's happening:** Saving the policy makes it available for message flows to reference. Multiple flows can use the same policy, promoting reusability and consistency.

---

## Part 4: Deploying the Database Application

### Step 10: Deploy to Integration Server

![Deployment Progress](image/image-12.png)

1. Right-click on your database project (e.g., **ExampleDatabaseRefine**)
2. Select **Deploy** → **[Your Integration Server]**
3. A deployment progress dialog will appear
4. Monitor the deployment messages

**Deployment Messages:**
```
Begin running task [Deploying 'ExampleDatabaseRefine' to integration server 'server']

Successfully deployed '/Users/rafaelsoriano/IBM/ACE/12/workspace/GeneratedBarFiles/
ExampleDatabaseRefine/project.generated.bar' to integration server 'TEST_SER'

'ExampleDatabaseRefine' is successfully deployed.

End running task [Deploying 'ExampleDatabaseRefine' to integration server 'TEST_SER']
```

**What's happening:** Deployment packages your application with the database policy and deploys it to the Integration Server, making it ready to process requests and execute database queries.

---

### Step 11: Verify Deployment Success

![Deployment Success](image/image-12.png)

1. Check the deployment log for success messages
2. Look for:
   - **BIP9332I**: Application 'ExampleDatabaseRefine' has been changed successfully
   - **BIP9326I**: The source has been successfully deployed
3. Verify no error messages appear
4. Click **Close** to dismiss the dialog

**What's happening:** These messages confirm that your database-enabled application is deployed and the Integration Server can now accept requests that will query the database.

---

## Part 5: Understanding the Message Flow

### Database Query Flow Architecture

A typical database query flow includes:

```
HTTP Input
    ↓
Compute Node (Build SQL Query)
    ↓
Database Node (Execute Query)
    ↓
Compute Node (Format Response)
    ↓
HTTP Reply
```

### Common Database Nodes

**1. Database Node**
- Executes SQL statements
- Supports SELECT, INSERT, UPDATE, DELETE
- Returns result sets
- Handles transactions

**2. Compute Node (Pre-Query)**
- Builds dynamic SQL queries
- Sets query parameters
- Validates input data
- Prepares database connection

**3. Compute Node (Post-Query)**
- Formats query results
- Transforms data to JSON/XML
- Handles errors
- Logs results

---

## Part 6: Testing Database Connectivity

### Step 12: Prepare Test Message

![Send Message Dialog](image/image-17.png)

1. In ACE Toolkit, locate the **Flow Exerciser** or test client
2. Click **Send Message**
3. In the Send Message dialog:
   - **Name**: InputMessageUS
   - **Input Location**: HTTP Input
   - **Message Details**: Select or create test message
   - **Country**: Select **"US"** from dropdown

**What's happening:** The test message simulates an HTTP request to your REST API endpoint. The "Country" parameter will be used in the SQL query to filter database results.

---

### Step 13: Send Test Request

1. Verify the message configuration
2. Click **Send** to execute the request
3. Wait for the response
4. The Integration Server will:
   - Receive the HTTP request
   - Execute the SQL query with Country='US'
   - Retrieve matching records from DB2
   - Format results as JSON
   - Return the response

**What's happening:** ACE processes your request through the message flow, connects to DB2 using the configured policy, executes the query, and returns the results.

---

### Step 14: Test with Postman

![Postman Test](image/image-22.png)

**Alternative Testing Method:**

1. Open Postman
2. Create a new request:
   - **Method**: POST
   - **URL**: `http://localhost:7800/DatabaseRetrieveFlow`
   - **Headers**: 
     - Content-Type: application/json
   - **Body** (JSON):
     ```json
     {
       "Country": "US"
     }
     ```

3. Click **Send**

**Expected Response:**
```json
{
  "Country": "US",
  "Employee": [
    {
      "FIRSTNAME": "John",
      "LASTNAME": "Pho",
      "COUNTRY": "US"
    }
  ]
}
```

**What's happening:** Postman sends an HTTP POST request to your ACE endpoint. The message flow queries the database for employees in the US and returns the results in JSON format.

---

### Step 15: Test Different Countries

Try testing with different country codes:

**Test Case 1: United Kingdom**
```json
{
  "Country": "UK"
}
```

**Test Case 2: Canada**
```json
{
  "Country": "CA"
}
```

**Test Case 3: Multiple Results**
```json
{
  "Country": "US"
}
```

**What's happening:** Each request executes a different SQL query based on the country parameter, demonstrating dynamic query execution.

---

## Part 7: Understanding SQL Queries in ACE

### Writing SQL in Compute Nodes

**Example: Simple SELECT Query**
```sql
SET OutputRoot.XMLNSC.Query = 
    'SELECT FIRSTNAME, LASTNAME, COUNTRY 
     FROM MYSCHEMA.EMPLOYEES 
     WHERE COUNTRY = ''' || InputRoot.JSON.Data.Country || '''';
```

**Example: Parameterized Query**
```sql
DECLARE country CHARACTER InputRoot.JSON.Data.Country;

SET OutputRoot.XMLNSC.Query = 
    'SELECT * FROM MYSCHEMA.EMPLOYEES WHERE COUNTRY = ?';
    
SET OutputRoot.XMLNSC.Parameters.Parameter[1] = country;
```

**Example: INSERT Statement**
```sql
SET OutputRoot.XMLNSC.Query = 
    'INSERT INTO MYSCHEMA.EMPLOYEES (FIRSTNAME, LASTNAME, COUNTRY) 
     VALUES (?, ?, ?)';
     
SET OutputRoot.XMLNSC.Parameters.Parameter[1] = InputRoot.JSON.Data.firstName;
SET OutputRoot.XMLNSC.Parameters.Parameter[2] = InputRoot.JSON.Data.lastName;
SET OutputRoot.XMLNSC.Parameters.Parameter[3] = InputRoot.JSON.Data.country;
```

**Example: UPDATE Statement**
```sql
SET OutputRoot.XMLNSC.Query = 
    'UPDATE MYSCHEMA.EMPLOYEES 
     SET LASTNAME = ? 
     WHERE FIRSTNAME = ?';
     
SET OutputRoot.XMLNSC.Parameters.Parameter[1] = InputRoot.JSON.Data.newLastName;
SET OutputRoot.XMLNSC.Parameters.Parameter[2] = InputRoot.JSON.Data.firstName;
```

**Example: DELETE Statement**
```sql
SET OutputRoot.XMLNSC.Query = 
    'DELETE FROM MYSCHEMA.EMPLOYEES 
     WHERE COUNTRY = ?';
     
SET OutputRoot.XMLNSC.Parameters.Parameter[1] = InputRoot.JSON.Data.country;
```

---

### Processing Query Results

**Reading Result Set:**
```sql
-- After Database node execution
DECLARE resultRef REFERENCE TO OutputRoot.DATABASEREAD.EMPLOYEES;

-- Loop through results
WHILE LASTMOVE(resultRef) DO
    SET OutputRoot.JSON.Data.Employee[].firstName = resultRef.FIRSTNAME;
    SET OutputRoot.JSON.Data.Employee[].lastName = resultRef.LASTNAME;
    SET OutputRoot.JSON.Data.Employee[].country = resultRef.COUNTRY;
    MOVE resultRef NEXTSIBLING;
END WHILE;
```

**Counting Results:**
```sql
DECLARE rowCount INTEGER;
SET rowCount = CARDINALITY(OutputRoot.DATABASEREAD.EMPLOYEES[]);

IF rowCount = 0 THEN
    SET OutputRoot.JSON.Data.message = 'No records found';
ELSE
    SET OutputRoot.JSON.Data.recordCount = rowCount;
END IF;
```

---

## Part 8: Database Node Configuration

### Database Node Properties

**Basic Properties:**
- **Data source name**: Reference to database policy
- **SQL statement**: Can be specified here or from message
- **Result set destination**: Where to store query results

**Advanced Properties:**
- **Transaction mode**: Automatic or Manual
- **Timeout**: Query execution timeout
- **Fetch size**: Number of rows to fetch at once
- **Result set type**: Forward-only or Scrollable

### Configuring Database Node

1. **Drag Database node** onto message flow canvas
2. **Double-click** to open properties
3. **Set Data source name**: `{MyJDBCPolicy}`
4. **Configure SQL statement source**:
   - From message: Read from InputRoot.XMLNSC.Query
   - Fixed: Enter SQL directly in properties
5. **Set Result set destination**: `OutputRoot.DATABASEREAD`

---

## Part 9: Best Practices

### 1. Security Best Practices

**Use Security Identity:**
```
Instead of hardcoding credentials in connection URL:
jdbc:db2://server:port/database;user=admin;password=secret

Use security identity in policy:
Security identity (DSN): mydbidentity
```

**Configure Security Identity:**
1. Use `mqsisetdbparms` command
2. Store credentials securely
3. Reference in policy

```bash
mqsisetdbparms TEST_SERVER -n jdbc::mydbidentity -u admin -p secret123
```

**What's happening:** Credentials are encrypted and stored separately from the policy, improving security.

---

### 2. Connection Pooling

**Configure Connection Pool:**
- **Maximum size**: Set based on expected load
- **Minimum size**: Keep connections ready
- **Idle timeout**: Close unused connections
- **Validation query**: Test connection health

**Example Configuration:**
```
Maximum size of connection pool: 10
Minimum size of connection pool: 2
Connection idle timeout: 300 seconds
Validation query: SELECT 1 FROM SYSIBM.SYSDUMMY1
```

---

### 3. Error Handling

**Handle Database Errors:**
```sql
BEGIN
    -- Execute database operation
    CALL CopyMessageHeaders();
    
    -- Build query
    SET OutputRoot.XMLNSC.Query = 'SELECT * FROM EMPLOYEES WHERE COUNTRY = ?';
    SET OutputRoot.XMLNSC.Parameters.Parameter[1] = InputRoot.JSON.Data.Country;
    
EXCEPTION
    WHEN DATABASE SQLCODE = -803 THEN
        -- Duplicate key error
        SET OutputRoot.JSON.Data.error = 'Record already exists';
        SET OutputRoot.JSON.Data.sqlcode = SQLCODE;
    WHEN DATABASE SQLCODE = -204 THEN
        -- Table not found
        SET OutputRoot.JSON.Data.error = 'Table does not exist';
        SET OutputRoot.JSON.Data.sqlcode = SQLCODE;
    WHEN DATABASE THEN
        -- General database error
        SET OutputRoot.JSON.Data.error = 'Database error occurred';
        SET OutputRoot.JSON.Data.sqlcode = SQLCODE;
        SET OutputRoot.JSON.Data.sqlstate = SQLSTATE;
        SET OutputRoot.JSON.Data.message = SQLERRORTEXT;
END;
```

---

### 4. Performance Optimization

**Use Prepared Statements:**
```sql
-- Efficient: Uses prepared statement
SET OutputRoot.XMLNSC.Query = 'SELECT * FROM EMPLOYEES WHERE COUNTRY = ?';
SET OutputRoot.XMLNSC.Parameters.Parameter[1] = country;

-- Inefficient: Concatenates SQL
SET OutputRoot.XMLNSC.Query = 'SELECT * FROM EMPLOYEES WHERE COUNTRY = ''' || country || '''';
```

**Limit Result Sets:**
```sql
-- Add LIMIT clause
SET OutputRoot.XMLNSC.Query = 
    'SELECT * FROM EMPLOYEES WHERE COUNTRY = ? LIMIT 100';
```

**Use Indexes:**
- Ensure database tables have appropriate indexes
- Index columns used in WHERE clauses
- Monitor query performance

**Batch Operations:**
```sql
-- Insert multiple records in one transaction
SET OutputRoot.XMLNSC.Query = 
    'INSERT INTO EMPLOYEES (FIRSTNAME, LASTNAME, COUNTRY) VALUES (?, ?, ?)';

-- Loop through input array
DECLARE i INTEGER 1;
WHILE i <= CARDINALITY(InputRoot.JSON.Data.employees[]) DO
    SET OutputRoot.XMLNSC.Parameters.Parameter[1] = InputRoot.JSON.Data.employees[i].firstName;
    SET OutputRoot.XMLNSC.Parameters.Parameter[2] = InputRoot.JSON.Data.employees[i].lastName;
    SET OutputRoot.XMLNSC.Parameters.Parameter[3] = InputRoot.JSON.Data.employees[i].country;
    -- Execute insert
    SET i = i + 1;
END WHILE;
```

---

### 5. Transaction Management

**Automatic Transactions:**
```
Database node property: Transaction mode = Automatic
- ACE manages commit/rollback
- Suitable for simple operations
```

**Manual Transactions:**
```sql
-- Begin transaction
CALL BeginTransaction();

-- Execute multiple operations
-- Operation 1
SET OutputRoot.XMLNSC.Query = 'INSERT INTO ORDERS ...';
-- Execute

-- Operation 2
SET OutputRoot.XMLNSC.Query = 'UPDATE INVENTORY ...';
-- Execute

-- Commit or rollback
IF success THEN
    CALL CommitTransaction();
ELSE
    CALL RollbackTransaction();
END IF;
```

---

## Troubleshooting

### Common Issues and Solutions

**Issue: Connection Failed**
- **Symptom**: "Unable to connect to database" error
- **Solutions**:
  - Verify database server is running
  - Check server name and port in policy
  - Confirm firewall allows connection
  - Test connection with DB2 client tools
  - Verify credentials are correct
  - Check JDBC driver path is correct

**Issue: JDBC Driver Not Found**
- **Symptom**: "ClassNotFoundException: com.ibm.db2.jcc.DB2Driver"
- **Solutions**:
  - Verify db2jcc4.jar exists at specified path
  - Check file permissions
  - Ensure Integration Server can access the file
  - Restart Integration Server after adding driver

**Issue: SQL Syntax Error**
- **Symptom**: "SQLCODE: -104" or similar
- **Solutions**:
  - Review SQL statement syntax
  - Check table and column names
  - Verify schema name is correct
  - Test query in DB2 client first
  - Check for missing quotes or parameters

**Issue: Authentication Failed**
- **Symptom**: "SQLCODE: -1403" or "Invalid credentials"
- **Solutions**:
  - Verify username and password
  - Check security identity configuration
  - Use mqsisetdbparms to set credentials
  - Confirm user has database access permissions

**Issue: Table Not Found**
- **Symptom**: "SQLCODE: -204"
- **Solutions**:
  - Verify table name spelling
  - Check schema name in query
  - Confirm table exists in database
  - Verify user has SELECT permission on table

**Issue: No Results Returned**
- **Symptom**: Empty result set
- **Solutions**:
  - Verify data exists in table
  - Check WHERE clause conditions
  - Test query directly in DB2
  - Review parameter values being passed

**Issue: Connection Pool Exhausted**
- **Symptom**: "No connections available"
- **Solutions**:
  - Increase connection pool size
  - Check for connection leaks
  - Reduce connection idle timeout
  - Monitor connection usage

---

## Understanding DB2 Error Codes

### Common SQLCODE Values

| SQLCODE | Meaning | Solution |
|---------|---------|----------|
| -104 | Syntax error | Check SQL statement syntax |
| -204 | Object not found | Verify table/view exists |
| -206 | Column not found | Check column name spelling |
| -407 | NULL value not allowed | Provide value for NOT NULL column |
| -530 | Foreign key violation | Check referenced table data |
| -803 | Duplicate key | Record with key already exists |
| -911 | Deadlock | Retry transaction |
| -1403 | Authentication failed | Check credentials |

### Checking Error Details in ESQL

```sql
EXCEPTION
    WHEN DATABASE THEN
        -- Log error details
        SET OutputRoot.JSON.Data.error.code = SQLCODE;
        SET OutputRoot.JSON.Data.error.state = SQLSTATE;
        SET OutputRoot.JSON.Data.error.text = SQLERRORTEXT;
        SET OutputRoot.JSON.Data.error.nativeError = SQLNATIVEERROR;
END;
```

---

## Advanced Database Operations

### Calling Stored Procedures

```sql
-- Call stored procedure
SET OutputRoot.XMLNSC.Query = 'CALL MYSCHEMA.GET_EMPLOYEE_DETAILS(?, ?)';
SET OutputRoot.XMLNSC.Parameters.Parameter[1] = employeeId;
SET OutputRoot.XMLNSC.Parameters.Parameter[2] = NULL; -- OUT parameter
```

### Using Database Functions

```sql
-- Use DB2 functions
SET OutputRoot.XMLNSC.Query = 
    'SELECT UPPER(FIRSTNAME) AS NAME, 
            CURRENT_DATE AS TODAY,
            DAYS(CURRENT_DATE) - DAYS(HIREDATE) AS DAYS_EMPLOYED
     FROM MYSCHEMA.EMPLOYEES
     WHERE COUNTRY = ?';
```

### Complex Joins

```sql
SET OutputRoot.XMLNSC.Query = 
    'SELECT e.FIRSTNAME, e.LASTNAME, d.DEPARTMENT_NAME
     FROM MYSCHEMA.EMPLOYEES e
     INNER JOIN MYSCHEMA.DEPARTMENTS d ON e.DEPT_ID = d.DEPT_ID
     WHERE e.COUNTRY = ?';
```

### Aggregate Queries

```sql
SET OutputRoot.XMLNSC.Query = 
    'SELECT COUNTRY, COUNT(*) AS EMPLOYEE_COUNT, AVG(SALARY) AS AVG_SALARY
     FROM MYSCHEMA.EMPLOYEES
     GROUP BY COUNTRY
     HAVING COUNT(*) > 5
     ORDER BY EMPLOYEE_COUNT DESC';
```

---

## Integration with Other Tutorials

### Building on Previous Tutorials

**From Tutorial 1 (REST API):**
- REST endpoints now query database instead of returning static data
- HTTP requests trigger database operations
- Responses contain real data from DB2

**From Tutorial 2 (Transformation):**
- Transform database results to JSON
- Map database columns to API response fields
- Route based on database query results

**Preparing for Tutorial 4 (SOAP):**
- Database data can be exposed via SOAP services
- Query results can be transformed to SOAP XML
- Combine database and SOAP operations

---

## Summary

In this tutorial, you learned how to:
- ✅ Import database-enabled projects into ACE
- ✅ Configure DB2 database policies with connection details
- ✅ Set up JDBC drivers for DB2 connectivity
- ✅ Deploy applications with database dependencies
- ✅ Execute SQL queries from message flows
- ✅ Process database result sets
- ✅ Test database operations via REST APIs
- ✅ Handle database errors gracefully
- ✅ Apply best practices for security and performance
- ✅ Troubleshoot common database connectivity issues

You now have the skills to integrate ACE with DB2 databases, enabling your integration flows to persist and retrieve data from enterprise databases.

---

## Next Steps

After completing this tutorial, you can:

1. **Implement CRUD Operations**: Create complete Create, Read, Update, Delete flows
2. **Add Transaction Management**: Implement multi-step database transactions
3. **Optimize Performance**: Tune connection pools and query performance
4. **Proceed to Tutorial 4**: Learn SOAP service integration
5. **Explore Other Databases**: Connect to Oracle, SQL Server, PostgreSQL
6. **Implement Caching**: Add caching layer for frequently accessed data

---

## Additional Resources

- [IBM DB2 Documentation](https://www.ibm.com/docs/en/db2)
- [ACE Database Connectivity](https://www.ibm.com/docs/en/app-connect/13.0?topic=nodes-database-node)
- [JDBC Configuration Guide](https://www.ibm.com/docs/en/app-connect/13.0?topic=policies-jdbc-providers-policy)
- [SQL Reference for DB2](https://www.ibm.com/docs/en/db2/11.5?topic=reference-sql)
- [mqsisetdbparms Command](https://www.ibm.com/docs/en/app-connect/13.0?topic=commands-mqsisetdbparms)

---

## Quick Reference: Database Commands

### mqsisetdbparms Commands

```bash
# Set database credentials
mqsisetdbparms <server> -n jdbc::mydbidentity -u username -p password

# List configured credentials
mqsilist <server> -o SecurityIdentity

# Remove credentials
mqsideletecredentials <server> -n jdbc::mydbidentity
```

### Testing Database Connection

```bash
# Test JDBC connection
mqsitestjdbc <server> -n jdbc::mydbidentity -d USERS

# View server properties
mqsireportproperties <server> -o AllReportableEntityNames -r
```

---

## Glossary

- **JDBC**: Java Database Connectivity - API for connecting to databases
- **DB2**: IBM's relational database management system
- **Policy**: Configuration file defining connection parameters
- **Data Source**: Named database connection configuration
- **Connection Pool**: Reusable database connections for performance
- **SQLCODE**: Numeric code indicating SQL operation result
- **SQLSTATE**: Standard SQL error state code
- **Schema**: Logical grouping of database objects
- **Prepared Statement**: Pre-compiled SQL statement for efficiency
- **Result Set**: Data returned from a database query
- **Transaction**: Unit of work that must complete entirely or not at all

---

**Workshop Version**: 1.0  
**Last Updated**: January 2026  
**ACE Version**: 13.0.6.0  
**DB2 Version**: 11.1+