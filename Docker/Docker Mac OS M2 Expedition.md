So, today I wanted to see if I could get a docker container running SqlServer 2022 with CLR enabled. I was able to do so with the following instructions:

Sources:
[Twilio SqlServer On MacOS (THIS IS A GREAT GUIDE)](https://www.twilio.com/blog/using-sql-server-on-macos-with-docker)
[StackExchange Post on M1 Mac CLR Functions](https://dba.stackexchange.com/questions/307157/is-there-a-way-to-use-standard-clr-functions-on-azure-sql-edge-for-ubuntu-docker)
[Microsoft Post on Development with SQL in Containers on MACOS](https://devblogs.microsoft.com/azure-sql/development-with-sql-in-containers-on-macos/)


1. Install Docker for Mac Apple Chip:
   [Install Docker on Mac](https://docs.docker.com/desktop/install/mac-install/)
2. Install Python
   - For this you have to use the `macOS 64-bit universal2 isntaller`  [Here](https://www.python.org/downloads/release/python-3114/)
   - The above link may be outdated so be sure to get the latest release
3. Just to make sure you have installed after it says completed installation use the following command: 
   `python3-intel64`
   This should bring you to the cli tool for python
4. View the above twilio link for setting up Docker Desktop correctly. It will should you how to setup the emulation for x64/x86
5. Install SQL CLI for ease of use and checking you can connect:
   `sudo npm install -g sql-cli`
6. Now we can install and run the mssql docker container available for us
```
sudo docker run -e 'ACCEPT_EULA=1' -e 'MSSQL_SA_PASSWORD=St0ngP4ssw0rd!' -p 1433:1433 --name sql-1 --hostname sql1 -d mcr.microsoft.com/mssql/server:2022-latest --platform linux/amd64
```
7. From here this should be able to connect to it via the mssql cli tool:
   `mssql -u sa -p St0ngP4ssw0rd!`
8. If you receive:
```
Connecting to localhost...done

sql-cli version <whatever version>
Enter "help." for usage hints.
mssql>
```
You are golden.
9. Now if you want to set it up on [Azure Data Studio](https://azure.microsoft.com/en-us/products/data-studio) you can do the following settings:

Connection Type: Microsoft Sql Server 
Authentication Type:  SQL Login       
Server: localhost
User name: sa 
Password : {{your pass}}

10. Now you will see you have connected to your local containerized database!
11. Note* You can enable CLR but you will not get SQLSysClrTypes




