## Miscellaneous Daily Records about tech, work, etc.

### 2019-7-2

#### tail
1. Print  the last 10 lines of each FILE to standard output.  With more than one FILE, precede each with a header giving the file name.
2. tail -f filename
   + output appended data as the file grows
3. -n, --lines=K
   + output the last K lines, instead of the last 10; or use -n +K to output lines starting with the Kth
4. -v, --verbose
   + always output headers giving file names

#### [revisit higher-order functions](https://danielwestheide.com/blog/2013/01/23/the-neophytes-guide-to-scala-part-10-staying-dry-with-higher-order-functions.html)
1. [what is the deference between method and function in scala](https://stackoverflow.com/questions/2529184/difference-between-method-and-function-in-scala)
2. [Scala functions vs methods](http://jim-mcbeath.blogspot.com/2009/05/scala-functions-vs-methods.html)
3. my brief understanding
   + a scala method is a part of a class; it has a name, a signature, optionally some annotations, and some bytecode.
   + a function in scala is a complete object. There are a series of traits in Scala to represent functions with various numbers of arguments: Function0, Function1, Function2, etc. As an instance of a class that implements one of these traits, a function object has methods. One of these methods is the apply method, which contains the code that implements the body of the function. Scala has special "apply" syntax: if you write a symbol name followed by an argument list in parentheses (or just a pair of parentheses for an empty argument list), Scala converts that into a call to the apply method for the named object. When we create a variable whose value is a function object and we then reference that variable followed by parentheses, that gets converted into a call to the apply method of the function object. 
### 2019-7-4
#### the difference between tinyint(1) and tinyint(2) in MySQL
1. table

   |type    | Storage(Bytes)|min value signed|min value unsigned|max value signed|max value unsigned|
   |--------|---------------|----------------|------------------|----------------|------------------|
   |tinyint | 1             |    -128        |     0            |   127          |        255       |
2. when used together with zerofill:
   ``` 
   CREATE TABLE `test` (                                  
            `id` int(11) NOT NULL AUTO_INCREMENT,                
            `state` tinyint(1) unsigned zerofill DEFAULT NULL,   
            `state2` tinyint(2) unsigned zerofill DEFAULT NULL,  
            `state3` tinyint(3) unsigned zerofill DEFAULT NULL,  
            `state4` tinyint(4) unsigned zerofill DEFAULT NULL,  
            PRIMARY KEY (`id`)                                   
          ) ENGINE=InnoDB AUTO_INCREMENT=6 DEFAULT CHARSET=utf8  
   
   insert into test (state,state2,state3,state4) values(4,4,4,4);
   select * from test; 
   
   //output
   state state2 state3 state4
   4       04    004   0004
   
   ```
### 2019-7-5
#### var initialization
   ``` 
   var s: Whatever = _
   ```
1. This will initialize ```s``` to the default value for ```Whatever``` (null for reference types, 0 for numbers, false for bools etc.)
#### [What is a DSL and where should I use it?](https://stackoverflow.com/questions/41724/what-is-a-dsl-and-where-should-i-use-it)

### 2019-7-8
#### configure a remote for a fork
1. List the current configured remote repository for your fork.
   + ```$ git remote -v```
2. Specify a new remote upstream repository that will be synced with the fork.
   + ```$ git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git```
3. Verify the new upstream repository you've specified for your fork.
   + ```$ git remote -v```
#### set git global user.name and user.email
1. ```git config --global user.name```
2. ```git config --global user.email```
3. ```git config --global user.name "username"```
4. ```git config --global user.email "useremail"```

#### MySQL update a bunch of rows which meet a certain request
   + ```update `tablename` set `columnname` ='newvalue' where `anothercolumnname` ='someRequest'; ```
### 2019-7-9
#### rename a directory
   + ```$ mv thisDirectory/ Directorywithnewname/```
### 2019-7-10
#### ```$ jps |grep -v Jps``` command
   + ```jps``` is a terminal command; it is the Java Virtual Machine Process Status Tool
   + The jps tool lists the instrumented HotSpot Java Virtual Machines (JVMs) on the target system. 
   + SYNOPSIS: ```$ jps [options] [hostid]```
   + if hostid is omitted, jps lists localhost JVM processes status
   +```$ jps |grep -v Jps``` omits the Jps process itself which is also running on JVM.
#### ```sftp``` command
   + ```sftp username@remotehost``` default port 22
   + ```sftp -oPort=22 username@remotehost``` -oPort option for specifying a port
   + ```> get pathtoRemoteFile pathtoLocalDirectory``` for downloading a remote file
   + ```> get -r pathtoRemoteDirectory pathtoLocalDirectory``` for downloading a remote Directory
   + ```> put pathtoLocalFileOrDirectory pathtoRemoteDirectory``` for uploading a local file or directory
   + in a sftp session, use ```!command``` to run commands on local machine
   + ```> exit``` or ```> quit``` for exiting sftp program.
### 2019-7-16
#### To get a value out of a scala ```Option```: 
1. Directly extracting the value out
   + match expression
   ``` 
   val res = option match {
       case Some(i) => i
       case None => default
   }
   ```
   + ```getOrElse```
   ``` 
   val res = option.getOrElse(default)
   ```
2. Applying a function while extracting the value from the Option
   + match expression
   ``` 
   def f(...) = { ... }
   
   val res = option match {
       case Some(i) => f(i)
       case None => default
   }
   ```
   + use ```map``` followed by ```getOrElse```
   ``` 
   val res = option.map(f).getOrElse(default)
   ```
   + call ```fold``` on the ```Option```
   ```
   //applies the function f to the value in a Some, or returns the default value if option is really a None.
   val res = option.fold(default)(f)
   ```
#### [What is the relation between Iterable and Iterator?](https://stackoverflow.com/questions/11302270/what-is-the-relation-between-iterable-and-iterator)
### 2019-7-17
#### [Check what port MySQL is running on](https://serverfault.com/questions/116100/how-to-check-what-port-mysql-is-running-on)
1. In MySQL session:

   ``` mysql> SHOW GLOBAL VARIABLES LIKE 'PORT';```
2. The best way to actually know what application is listening to which interface and on what port is to use ```netstat```

   ``` 
   //you can do this as root, option -p does not work on Mac OS X
   $ netstat -tlnp
   //sample output
   Active Internet connections (only servers)
   Proto Recv-Q Send-Q Local Address               Foreign Address             State       PID/Program name
   tcp        0      0 127.0.0.1:9003              0.0.0.0:*                   LISTEN      16992/java
   tcp        0      0 127.0.0.1:9999              0.0.0.0:*                   LISTEN      17233/php-fpm
   tcp        0      0 0.0.0.0:80                  0.0.0.0:*                   LISTEN      5136/nginx
   tcp        0      0 0.0.0.0:22                  0.0.0.0:*                   LISTEN      -
   tcp        0      0 0.0.0.0:12345               0.0.0.0:*                   LISTEN      -
   tcp        0      0 127.0.0.1:15770             0.0.0.0:*                   LISTEN      -
   tcp        0      0 0.0.0.0:443                 0.0.0.0:*                   LISTEN      5136/nginx
   tcp        0      0 127.0.0.1:9000              0.0.0.0:*                   LISTEN      4184/java
   tcp        0      0 :::22                       :::*                        LISTEN      -
   tcp        0      0 :::12345                    :::*                        LISTEN      -
   ```
####[What is the utf8mb4_0900_ai_ci Collation?](https://www.monolune.com/what-is-the-utf8mb4_0900_ai_ci-collation/)
1. ```utf8mb4``` has become the default character set, with ```utf8mb4_0900_ai_ci``` as the default collation in MySQL 8.0.1 and later.
   + ```uft8mb4``` means that each character is stored as a maximum of 4 bytes in the UTF-8 encoding scheme.
   + ```0900``` refers to the Unicode Collation Algorithm version. (The Unicode Collation Algorithm is the method used to compare two Unicode strings that conforms to the requirements of the Unicode Standard).
   + ```ai``` refers accent insensitivity. That is, there is no difference between e, ??, ??, ?? and ?? when sorting.
   + ```ci``` refers to case insensitivity. This is, there is no difference between p and P when sorting.
2. If accent sensitivity and case sensitivity are required, you may use ```utf8mb4_0900_as_cs``` instead.
### 2019-7-18
#### sbt session, type in ```console``` command to enter console mode, then type in ```:paste``` to enter paste mode.

###2019-7-19
#### [How to zip or unzip files and folders using command line on Ubuntu server](https://www.wpoven.com/tutorial/how-to-zip-or-unzip-files-and-folders-using-command-line-on-ubuntu-server/)
1. First get ```zip``` and ```unzip``` installed
   + ```sudo apt-get install zip```
   + ```sudo apt-get install unzip```
2. ```-r``` option for zipping folder having more than one file or folder and do not use ```-r``` for single file
   + ```zip -r pathtoDirectory/zippedname.zip orignial_folder```
   + ```zip singlefile.zip original_file```

#### To check out the amount of free or used disk space in Ubuntu
1. Command line interface
   + ```$ df -h```
2. [refer to this ask ubuntu question](https://askubuntu.com/questions/73160/how-do-i-find-the-amount-of-free-space-on-my-hard-drive)
#### Check out system specifications in Ubuntu
1. ```$ lshw | less``` which means ls(list)hw(hardware)
2. [refer to this ask ubuntu question](https://askubuntu.com/questions/55609/how-do-i-check-system-specifications)
### 2019-7-22
#### [Execute Command Line Java program in background](https://stackoverflow.com/questions/3030605/execute-command-line-java-program-in-background)
1. You can add ```&``` at the end of the command line to run it in the background.
#### To check out CPU MEM usage in Ubuntu
1. ```$ sudo apt-get install htop```
2. run ```$ htop```
### 2019-7-23
#### [SQL LIMIT](http://www.sqltutorial.org/sql-limit/)
1. To retrieve a portion of rows returned by a query, use the ```LIMIT``` and ```OFFSET``` clauses.
   ``` 
   SELECT 
       column_list
   FROM
       table1
   ORDER BY column_list
   LIMIT row_count OFFSET offset;
   ```
   + The ```row_count``` determines the number of rows that will be returned.
   + The ```OFFSET```  clause skips the ```offset``` rows before beginning to return the rows. The ```OFFSET``` clause is optional so you can skip it. If you use both ```LIMIT``` and ```OFFSET``` clauses the ```OFFSET```  skips offset rows first before the ```LIMIT```  constrains the number of rows.
2. When you use the ```LIMIT``` clause, it is important to use an ```ORDER BY``` clause to make sure that the rows in the returned are in a specified order.

### 2019-7-25
#### Representing 1-element ```Seq``` and 1-element ```List```
   ``` 
   //1-element Seq
   element +: Nil
   //2-element Seq
   e1 +: e2 +: Nil
   //1-element List
   element :: Nil
   ```
### 2019-7-29
#### [How to get date and time using command line interface?](https://askubuntu.com/questions/634173/how-to-get-date-and-time-using-command-line-interface)
1. ```date "+%H:%M:%S   %d/%m/%y"```
2. OR simply ```date```

### 2019-8-14
#### IntelliJ IDEA: to go a specific line on OSX 'command + L'

#### [Vim search and replace](https://vim.fandom.com/wiki/Search_and_replace)
1. ```:%s/foo/bar/g```: Find each occurrence of 'foo' (in all lines), and replace it with 'bar'.
2. ```:s/foo/bar/g```: Find each occurrence of 'foo' (in the current line only), and replace it with 'bar'.

### 2019-8-17
#### ```drop``` and ```dropWhile```
1. ```drop``` drops the first ```i``` elements; ```dropWhile``` removes the first elements that match a predicate function.
   ```
   val a = List(1, 3, 5, 6, 5, 4, 3, 1)
   a.drop(5)    //List(4, 3, 1)
   
   a.dropWhile(_ % 2 != 0)  //List(6, 5, 4, 3, 1) the 5 behind 6 is shielded by 6
   ```
2. Trimming Strings in scala
   ``` 
   val str = "   foo     "
   str.trim         //trim to remove leading and trailing spaces
   ```
3. ```stripPrefix``` and ```stripSuffix```
   ``` 
   /** Returns this string with the given `prefix` stripped. If this string does not
      *  start with `prefix`, it is returned unchanged.
      */
     def stripPrefix(prefix: String) =
       if (toString.startsWith(prefix)) toString.substring(prefix.length)
       else toString
   
   /** Returns this string with the given `suffix` stripped. If this string does not
      *  end with `suffix`, it is returned unchanged.
      */
     def stripSuffix(suffix: String) =
       if (toString.endsWith(suffix)) toString.substring(0, toString.length() - suffix.length)
       else toString
   ```
   
   ``` 
   val str = ",,faker"
   str.stripPrefix(",,")  // "faker"
   
   str.stripSuffix("faker")     //",,"
   
   str.stripSuffix("cer")       //",,faker"
   ```
4. [another stackoverflow example](https://stackoverflow.com/questions/17995260/trimming-strings-in-scala)
### 2019-8-19
#### To force quit apps: command + opt + esc

### 2019-8-20
#### mac terminal alias: edit .bash_profile:
   ``` 
   $ vim .bash_profile

     #set alias
     alias cdProjects = 'cd ~/projects'
   $ source .bash_profile
   ```
### 2019-8-27
#### Soft link to a Directory in Linux/Mac OS X: 
1. To create a soft link: ```$ ln -s /my/long/path/to/the/directory easyPath```
2. To remove a soft link: ```$ rm easyPath``` or ```$ unlink easyPath```. Note that it removes only the soft link that you created, it does not remove the original directory/file that you soft linked.
### 2019-8-29
#### To delete from cursor to end of line on Vim:
1. The command ```dw``` will delete from the current cursor position to the beginning of the next word character. The command ```d$``` will delete from the current cursor position to the end of the current line. ```D``` is a synonym for ```d$```.
### 2019-8-30
#### To change ssh login password for linux server:
1. login to server
2. ```$ passwd``` and then type in current password then new password

### 2019-10-22
#### mac keyboard shortcut 'command ??? + q' to quit current program.

### 2019-11-1
#### PostgreSQL UPDATE SQLs:
   ``` 
   UPDATE table_name
   SET column_name1 = value1, column_name2 = value2,??????
   [WHERE <condition>]; 
   ```

### 2019-12-4
#### Vim: Deleting up to a certain line [link](https://stackoverflow.com/questions/2250546/deleting-up-to-a-certain-line-in-vim)
1. delete from line 26 to line 199
   + ``` :26, 199d```       cursor being anywhere
   + ``` :.,199d```   ```.``` means the current line
   + ``` d126G```     cursor on the current line 26

### 2019-12-22
#### PostgreSQL: NOT Condition
1. combined with IN Condition
   ```
   SELECT *
   FROM employees
   WHERE last_name NOT IN ('Anderson', 'Johnson', 'Smith');
   ```
2. IS NOT NULL Condition
   ``` 
   SELECT *
   FROM contacts
   WHERE address IS NOT NULL;
   ```

### 2020-03-23
#### PostgreSQL LIKE pattern matching
1. Percent (%)  for matching any sequence of characters.(zero or more number of characters)
2. Underscore (_)  for matching any single character.
   ``` 
   SELECT 'foo' LIKE 'f%',  --- true
          'f' LIKE 'f%',  --- true
          'foo' LIKE 'foo',  ---true
          'foo' LIKE 'f_',  ---false
          'fa' LIKE 'f_',   ---true
          'f' LIKE '%f%',   ---true
          'foo' LIKE '_f_';   ---true
   ```
   
### 2020-04-15
#### Use the standurd output of one command as arguments of another command.
   ``` 
   # cat RUNNING_PID
     1999
   # kill -9 1999

   # kill -9 $(cat RUNNING_PID)
   ```

### 2020-04-21
#### [how to check out JDK JRE version on macOS](https://www.java.com/en/download/help/version_manual.xml)
   ``` 
   // JRE Version Command Line on Mac
   $ /Library/Internet\ Plug-Ins/JavaAppletPlugin.plugin/Contents/Home/bin/java -version
   java version "1.8.0_251"
   Java(TM) SE Runtime Environment (build 1.8.0_251-b08)
   Java HotSpot(TM) 64-Bit Server VM (build 25.251-b08, mixed mode)
   
   // Determining the Default Version of the JDK on Mac
   $ java -version
   java 11.0.7 2020-04-14 LTS
   Java(TM) SE Runtime Environment 18.9 (build 11.0.7+8-LTS)
   Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.7+8-LTS, mixed mode)
   ```
#### [Installation of the JDK and the JRE on macOS(and uninstallation)](https://docs.oracle.com/javase/9/install/installation-jdk-and-jre-macos.htm#JSJIG-GUID-2FE451B0-9572-4E38-A1A5-568B77B146DE)

### 2020-04-27
#### [PostgreSQL REPLACE Function](https://www.postgresqltutorial.com/postgresql-replace/)
1. Syntax for the PostgreSQL REPLACE() function:
   ``` REPLACE(source, old_text, new_text );```
   + example:
   ``` SELECT
        REPLACE('ABC AA', 'A', 'D');
   ```
   + result:
   ``` 
   DBC DD
   ```
2. If you want to search and replace a substring in a table column, you use the following syntax:
   ``` 
   UPDATE
    table_name
   SET 
    column_name = REPLACE(column, old_text,new_text)
   WHERE
    condition
   ```
   + example:
   ``` 
   table customer
   first_name | last_name | email
   Alice      | bob       | alice@gmail.com
   Bob        | Cart      | bob@gmail.com
   Cart       | David     | cart@foxmail.com
   ```
   + SQL:
   ``` 
   UPDATE customer
   SET email =
    REPLACE(email, 'gmail.com', 'yahoo.com');
   ```
   + result:
   ``` 
   table customer
      first_name | last_name | email
      Alice      | bob       | alice@yahoo.com
      Bob        | Cart      | bob@yahoo.com
      Cart       | David     | cart@foxmail.com
   ```
3. Regular expression replace syntax:
   ```REGEXP_REPLACE(source, pattern, new_text [,flags])```
   + example:
   ``` 
    SELECT
    	regexp_replace(
    		'foo bar foobar barfoo',
    		'foo',
    		'bar'
    	);
   ```
   + result:
   ``` bar bar foobar barfoo```
   + example, ```i``` flag ignores case
   ``` 
   SELECT
   	regexp_replace(
   		'bar foobar bar bars',
   		'Bar',
   		'foo',
   		'i'
   	);
   ```
   + result:
   ``` foo foobar bar bars```
   + example, ```g``` flag means global:
   ```
   SELECT
   	regexp_replace(
   		'Bar sheepbar bar bars barsheep',
   		'bar',
   		'foo',
   		'g'
   	);
   ```
   + result:
   ```Bar sheepfoo foo foos foosheep```
   + example:
   ``` 
   SELECT
   	regexp_replace(
   		'Bar sheepbar bar bars barsheep',
   		'bar',
   		'foo',
   		'gi'
   	);
   ```
   + result:
   ```foo sheepfoo foo foos foosheep```

#### Mr. Liutao's sharing on Event Sourcing and CQRS
Tao???s sharing on ES & CQRS

1. ES, Event Souring, focuses on events that are applied to Entities. The state of an Entity is calculated by replaying a series of events. Events can be in large quantity, thus making it a time-consuming poccess for calculating the state of an entity; therefore snapshots are used for period states in the series of events making it easier to calculate nearby subsequent states of a snapshot.
2. CQRS is short for Command Query Responsibility Segregation.
3. With Kafka, we can program with an ES style, a project model publishes events to Kafka, then other projects can consume the events the previous one published anyway as they like it, which is a CQRS style.

### 2020-05-08
#### Nginx reload conf
   ``` 
   sudo -s
   nginx -t         // check whether conf file is malformed
   nginx -s reload
   ```

### 2020-08-13
#### [PostgreSQL SUM Function](https://www.postgresqltutorial.com/postgresql-sum-function/)
   ``` 
   SELECT SUM(amount) AS total
   FROM payment
   WHERE customer_id = 2000;
   ```

### 2021-04-16
#### ```scp``` command [reference](https://linuxize.com/post/how-to-use-scp-command-to-securely-transfer-files/)
+ SCP (secure copy) is a command-line utility that allows you to securely copy files and directories between two locations.
  + local to remote; remote to local; remote to remote
  + when remote to remote, you don???t have to log in to one of the servers to transfer files from one to another remote machine. The data will be transfer directly from one remote host to the other. You will be prompted to enter the passwords for both remote accounts.
+ SCP Command Syntax
  ```scp [OPTION] [user@]SOURCE_HOST:]file1 [user@]DESTINATION_HOST:]file2```
  + ```-r``` option to copy directory 
+ example:
  ```
  local to remote directory
  > scp file.txt remote_username@10.10.0.2:/remote/directory
  ```
  ```
  local to remote directory with a different name
  > scp file.txt remote_username@10.10.0.2:/remote/directory/nexName.txt
  ```
  ```
  If SSH on the remote host is listening on a port other than the default 22 then you can specify the port using the -P argument:
  > scp -P 2322 file.txt remote_username@10.10.0.2:/remote/directory
  ```
  ```
  local directory to remote directory
  > scp -r /local/directory remote_username@10.10.0.2:/remote/directory
  ```
  ```
  remote file to local directory
  > scp remote_username@10.10.0.2:/remote/file.txt /local/directory
  ```
  ```
  remote1 to remote2
  > scp user1@host1.com:/files/file.txt user2@host2.com:/files
  ```
  ```
  remote1 to remote 2
  To route the traffic through the machine on which the command is issued, use the -3 option
  > scp -3 user1@host1.com:/files/file.txt user2@host2.com:/files
  ```

### 2021-04-20
#### maven run specified main class
#### [SO reference](https://stackoverflow.com/questions/1089285/maven-run-project)
  ```
  mvn exec:java -Dexec.mainClass="com.example.Main" [-Dexec.args="argument1"] ...
  ```

### 2021-04-21
#### SSH login without password (reference)[http://www.linuxproblem.org/art_9.html]
+ Beside the steps mentioned in the reference, on macOS, I have to specify which key file as IdentitydFile to use in ~/.ssh/config file as follows:
  ```
  Host *
      ServerAliveInterval 60
      ServerAliveCountMax 60
      ControlMaster auto
      ControlPath ~/.ssh/%h-%p-%r
      ControlPersist 4h
      Compression yes
      AddKeysToAgent yes
      UseKeychain yes
      IdentityFile ~/.ssh/id_rsa
  ```
+ or I would have to enter the password after sometime again.

#### SQL Comments
+ any character after ```--``` in a single line will be commented out
+ ```/* ... */``` any character between will be commented out
+ example:
  ```
  --SELECT * FROM Customers;
  SELECT * FROM Products;

  SELECT * FROM Customers -- WHERE City='Berlin';

  /*Select all the columns
  of all the records
  in the Customers table:*/
  SELECT * FROM Customers;

  SELECT * FROM Customers WHERE (CustomerName LIKE 'L%'
  OR CustomerName LIKE 'R%' /*OR CustomerName LIKE 'S%'
  OR CustomerName LIKE 'T%'*/ OR CustomerName LIKE 'W%')
  AND Country='USA'
  ORDER BY CustomerName;
  ```

### SQL print table structure
+ ```DESCRIBE table_name;```

### psql commands to use some database, show tables
+ [reference](https://www.postgresqltutorial.com)
+ Switch over to the postgres account
  ```$ sudo -i -u postgres```
+ access the PostgreSQL prompt
  ```psql```
+ Exit out of the PostgreSQL prompt by typing:
  ```postgres=# \q```
+ List Your PostgreSQL databases
  ```postgres=# \list```
  ``` output
                                     List of databases
       Name     |  Owner   | Encoding |   Collate   |    Ctype    |   Access privileges
  --------------+----------+----------+-------------+-------------+-----------------------
   sample_tbl1  | postgres | UTF8     | en_HK.UTF-8 | en_HK.UTF-8 |
   sample_tbl2  | postgres | UTF8     | en_HK.UTF-8 | en_HK.UTF-8 |
   fake_tbl3    | postgres | UTF8     | en_HK.UTF-8 | en_HK.UTF-8 |
   postgres     | postgres | UTF8     | en_HK.UTF-8 | en_HK.UTF-8 |
   template0    | postgres | UTF8     | en_HK.UTF-8 | en_HK.UTF-8 | =c/postgres          +
                |          |          |             |             | postgres=CTc/postgres
   template1    | postgres | UTF8     | en_HK.UTF-8 | en_HK.UTF-8 | =c/postgres          +
                |          |          |             |             | postgres=CTc/postgres
  (6 rows)
  ```
+ Drop database:
  ``` postgres=# DROP DATABASE IF EXISTS database_name;```
+ Switching Between Databases in PostgreSQL
  ```
  postgres=# \connect fake_tbl3
  OR
  postgres=# \c fake_tbl3
  ```
  ```output
  You are now connected to database "fake_tbl3" as user "postgres".
  ```
+ show all tables in the ```fake_tbl3``` database
  ```fake_tbl3=# \dt```
+ describe some table:
  ```
  fake_tbl3=# \d sample_tbl1
  OR
  fake_tbl3=# \d+ sample_tbl1
  ```

### count how many files there are in a directory
+ ```ls | wc -l```

### ```df``` command ```-B``` option for output block size
+ to print output in GB
  ```df -BG```
+ to print output in MB
  ```df -BM```

### How to display files sizes in MB in Linux/Ubuntu
+ To display the file sizes in units like 7K, 5M, 8.2G, etc. :
  ```ls -lh```
+ To display the sizes rounded up to the nearest MiB (2^20 bytes): 
  ``` ls -l --block-size=M ```
  ``` ls -l --block-size=1M ```
+ To display the sizes in MB (10^6 bytes): 
  ``` ls -l --block-size=MB ```

### How to get full path of a file in linux cli?
+ ```readlink -f filename```
+ ```realpath filename```  it is an external command (not a shell built-in) and may not be present in every system by default, i.e. you may need to install it.
  + in OSX, ```realpath``` belongs to ```coreutil```; so install coreutil first: ```brew install coreutil```

### minikube add a local docker image
+ ```minikube cache add $local_image_name```
+ "minikube cache" will be deprecated in upcoming versions, please switch to 
  ```minikube image load```

### minikube check out images
+ ``` minikube image list```

### kubectl delete pod
+ ```kubectl delete pod $pod_name```

### kubectl Deleting (almost) all resources in a namespace
+ ``` kubectl delete all --all```

### kubectl create pod from yaml file
+ ```kubectl create -f file_name.yaml```

### kubectl get all pods
+ ``` kubectl get pods```

### kubectl get all jobs
+ ``` kubectl get jobs```
+ trying to delete a pod which is a job causes another pod of the same job to be created and run
  ``` kubectl delete pod $name_of_pod_of_Job```
+ by deleting some job, all corresponding pods of the job will be deleted as well:
  ``` kubectl delete job $job_name```
+ in the job's yaml description


### kubectl to view logs in a pod
+ ```kubectl logs $pod_name```

### docker run command arg to automatically remove the container when it exits
+ ``` docker run --rm $image_name```

### tmux commands
+ creating named sessions
  ``` tmux new -s SESSION_NAME```
+ To detach from a current Tmux session, just press Ctrl+b and d. You don't need to press this both Keyboard shortcut at a time. First press "Ctrl+b" and then press "d".
+ To view the list of open Tmux sessions:
  ``` tmux list```
+ creating detached sessions; creating a session and don't attach to it automatically.
  ``` tmux new -s SESSION_NAME -d```
+ attach to a specify session:
  ``` tmux attach -t SESSION_NAME```
  ``` tmux a -t SESSION_NAME```
+ to kill a session:
  ``` tmux kill-session -t SESSION_NAME```
+ to kill a session in attached mode, press Ctrl+b and x, then type y to confirm.


### kubectl logs continously print log to console
+ `kubectl logs -f $POD_NAME`

### kubectl show label option
+ `kubectl get jobs --show-labels=true`

### kubectl get jobs with some specific label
+ `kubectl get jobs -l $SOME_LABEL_NAME`

### kubectl delete jobs with some specific label
+ `kubectl delete jobs -l $SOME_LABEL_NAME`

### kubectl enter postgres pod
+ `kubectl exec -it postgresql-0 -n kube-system -- psql -U postgres`
+ or
   + `kubectl exec -it postgresql-0 -n kube-system -- bash`
   + in postgres pod
   + `env | grep PASSWORD`

### docker run enter a running image
+ `docker run --rm -it -v /host/path:/container/path $IMAGE_NAME bash`

### tar command `tar -cvzf $DEST_FILE_NAME.tar.gz -C /path/to/src/directory .`
+ this command compresses files in /path/to/src/directory to DEST_FILE_NAME.tar.gz 
+ `-z` option tells `tar` to compress archive with gzip

### psql drop several tabels
+ `DROP TABLE `

### psql turn on or off expanded display
+ `postgres=# \x`  then psql prints: Expanded display is on.
+ when query tables records, they will be displayed vertically:
+ `\c SOME_DATABASE`
+ `select * from SOME_TABLE limit 2;`
   ```
   -[ RECORD 1 ]------------------------------------------
   id         | 1
   created_at | 2021-06-22 07:42:39.667342+00
   updated_at | 2021-06-22 07:42:39.727877+00
   deleted_at |
   -[ RECORD 2 ]------------------------------------------
   id         | 2
   created_at | 2021-06-22 07:42:39.667342+00
   updated_at | 2021-06-22 07:42:39.727877+00
   deleted_at |
   ```

### linux cli delete characters until the last none space character
+ `ctrl + w`

### kubectl sort pods by age
+ `kubectl get pods --sort-by=.metadata.creationTimestamp`

### kubectl get watch option
   ```
   -w, --watch=false: After listing/getting the requested object, watch for changes. Uninitialized objects are excluded if no object name is provided.
      --watch-only=false: Watch for changes to the requested object(s), without listing/getting first.
   ```


### `ls` `-h` option
+ When used with the -l option, use unit suffixes: Byte, Kilobyte, Megabyte, Gigabyte, Terabyte and Petabyte in order to reduce the number of dig- its to three or less using base 2 for sizes.

### List columns with indexes in PostgreSQL
+ `SELECT * FROM pg_indexes WHERE tablename = 'mytable';`

### How do you list volumes in docker containers?
+ refer: https://stackoverflow.com/questions/30133664/how-do-you-list-volumes-in-docker-containers
+ `docker ps` to get container id
+ `docker inspect -f '{{ .Mounts }}' containerid`

### maven run java application
+ `mvn exec:java -Dexec.mainClass="com.example.myapp.App"`

### git add a submodule to a sub-directory
+ refer: https://stackoverflow.com/questions/9035895/how-do-i-add-a-submodule-to-a-sub-directory
+ `git submodule add <git@github ...> relative-path/submodule-directory-name`

### delete kubernetes resource with a name pattern
+ refer: https://faun.pub/delete-kubernetes-pods-with-a-regex-f396291bba0e
+ example:
   + delete sparkapp in namespace org04 with name starting with 'spark-create'
   + `kubectl get sparkapp -n org04 | awk '/spark-create/{print $1}' | xargs kubectl delete sparkapplications.sparkoperator.k8s.io -n org04`