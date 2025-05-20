# ADF-RealtimeScenarios

## OnPremise data-copy to ADLS SQL-DB
    
### Steps to do a local setup

#### 1. Enable TCP/IP Protocol

    1. Open SQL Server Configuration Manager
    2. Go to SQL Server Network Configuration > Protocols for MSSQLSERVER
    3. Right-click TCP/IP > Enable
    4. Also ensure Named Pipes is enabled
    5. Restart the SQL Server service:
       * Go to SQL Server Services
       * Right-click on SQL Server (MSSQLSERVER) > Restart

#### 2. Allow SQL Server and SQL Browser through Firewall

    1. Open Windows Defender Firewall with Advanced Security
    2. Allow the following inbound rules:
        * SQL Server (TCP 1433) → Enable
        * SQL Server Browser (UDP 1434) → Enable
    3. Or manually add firewall rules for:
        * Port 1433 (TCP)
        * Port 1434 (UDP)

#### 3. Check SQL Server Authentication Mode

    Make sure Mixed Mode is enabled:
    1. In SSMS (if you can log in with Windows Authentication):
        * Right-click on server > Properties > Security
        * Select SQL Server and Windows Authentication mode
        * Click OK, then Restart SQL Server

#### 4. Ensure sa Login is Enabled

    In SSMS:
    1. Expand Security > Logins
    2. Right-click on sa > Properties
    3. On Status tab:
        * Permission to connect to database engine: Grant
        * Login: Enabled
    4. Click OK and restart SQL Server.

#### 5. Try Connecting Again Using TCP/IP

    1. Use this in SSMS or CMD:
```
  sqlcmd -S 127.0.0.1,1433 -U sa -P yourpassword
```
    2. Or in SSMS:
        * Server name: 127.0.0.1,1433
        * Auth: SQL Server Auth
        * Login: sa
        * Password: yourpassword

### Error: public network access is disabled on your Azure SQL Server 

    1. Go to the Azure portal: https://portal.azure.com
    2. Navigate to:
      * Resource Group → your SQL Server (e.g., demosqlserver7)
    3. In the SQL Server panel, go to:
      * Networking
    4. Under Public network access, select:
      * Selected networks
    5. Under Firewall rules, add:
      * Client IP address
      * Also add ADF’s IP ranges (see note below) 
    6. Click Save

## COPY OnPremise local file/folder to ADLS-Gen2

1. [AZCopy](https://learn.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10?tabs=dnf)
2. SFTP
    ##### prerequistie
   
    * local machine -- Open SSH client enabled
    * port 22 is allowed
  
    * Azure storage -- enable sftp connection
    * user - authentication  
   
