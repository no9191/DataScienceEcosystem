<p style="text-align:center">
    <a href="https://skills.network" target="_blank">
    <img src="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/assets/logos/SN_web_lightmode.png" width="200" alt="Skills Network Logo">
    </a>
</p>

<h1 align=center><font size = 5>Assignment: SQL Notebook for Peer Assignment</font></h1>

Estimated time needed: **60** minutes.

## Introduction
Using this Python notebook you will:

1.  Understand the Spacex DataSet
2.  Load the dataset  into the corresponding table in a Db2 database
3.  Execute SQL queries to answer assignment questions 


## Overview of the DataSet

SpaceX has gained worldwide attention for a series of historic milestones. 

It is the only private company ever to return a spacecraft from low-earth orbit, which it first accomplished in December 2010.
SpaceX advertises Falcon 9 rocket launches on its website with a cost of 62 million dollars wheras other providers cost upward of 165 million dollars each, much of the savings is because Space X can reuse the first stage. 


Therefore if we can determine if the first stage will land, we can determine the cost of a launch. 

This information can be used if an alternate company wants to bid against SpaceX for a rocket launch.

This dataset includes a record for each payload carried during a SpaceX mission into outer space.


### Download the datasets

This assignment requires you to load the spacex dataset.

In many cases the dataset to be analyzed is available as a .CSV (comma separated values) file, perhaps on the internet. Click on the link below to download and save the dataset (.CSV file):

 <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/labs/module_2/data/Spacex.csv" target="_blank">Spacex DataSet</a>




```python
pip install sqlalchemy==1.4.54

```

    Requirement already satisfied: sqlalchemy==1.4.54 in /opt/conda/lib/python3.11/site-packages (1.4.54)
    Requirement already satisfied: greenlet!=0.4.17 in /opt/conda/lib/python3.11/site-packages (from sqlalchemy==1.4.54) (3.0.3)
    Note: you may need to restart the kernel to use updated packages.


### Connect to the database

Let us first load the SQL extension and establish a connection with the database



```python
!pip install ipython-sql
```

    Requirement already satisfied: ipython-sql in /opt/conda/lib/python3.11/site-packages (0.5.0)
    Requirement already satisfied: prettytable in /opt/conda/lib/python3.11/site-packages (from ipython-sql) (3.12.0)
    Requirement already satisfied: ipython in /opt/conda/lib/python3.11/site-packages (from ipython-sql) (8.22.2)
    Collecting sqlalchemy>=2.0 (from ipython-sql)
      Using cached SQLAlchemy-2.0.36-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (9.7 kB)
    Requirement already satisfied: sqlparse in /opt/conda/lib/python3.11/site-packages (from ipython-sql) (0.5.2)
    Requirement already satisfied: six in /opt/conda/lib/python3.11/site-packages (from ipython-sql) (1.16.0)
    Requirement already satisfied: ipython-genutils in /opt/conda/lib/python3.11/site-packages (from ipython-sql) (0.2.0)
    Requirement already satisfied: typing-extensions>=4.6.0 in /opt/conda/lib/python3.11/site-packages (from sqlalchemy>=2.0->ipython-sql) (4.12.2)
    Requirement already satisfied: greenlet!=0.4.17 in /opt/conda/lib/python3.11/site-packages (from sqlalchemy>=2.0->ipython-sql) (3.0.3)
    Requirement already satisfied: decorator in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (5.1.1)
    Requirement already satisfied: jedi>=0.16 in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (0.19.1)
    Requirement already satisfied: matplotlib-inline in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (0.1.7)
    Requirement already satisfied: prompt-toolkit<3.1.0,>=3.0.41 in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (3.0.42)
    Requirement already satisfied: pygments>=2.4.0 in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (2.18.0)
    Requirement already satisfied: stack-data in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (0.6.2)
    Requirement already satisfied: traitlets>=5.13.0 in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (5.14.3)
    Requirement already satisfied: pexpect>4.3 in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (4.9.0)
    Requirement already satisfied: wcwidth in /opt/conda/lib/python3.11/site-packages (from prettytable->ipython-sql) (0.2.13)
    Requirement already satisfied: parso<0.9.0,>=0.8.3 in /opt/conda/lib/python3.11/site-packages (from jedi>=0.16->ipython->ipython-sql) (0.8.4)
    Requirement already satisfied: ptyprocess>=0.5 in /opt/conda/lib/python3.11/site-packages (from pexpect>4.3->ipython->ipython-sql) (0.7.0)
    Requirement already satisfied: executing>=1.2.0 in /opt/conda/lib/python3.11/site-packages (from stack-data->ipython->ipython-sql) (2.0.1)
    Requirement already satisfied: asttokens>=2.1.0 in /opt/conda/lib/python3.11/site-packages (from stack-data->ipython->ipython-sql) (2.4.1)
    Requirement already satisfied: pure-eval in /opt/conda/lib/python3.11/site-packages (from stack-data->ipython->ipython-sql) (0.2.2)
    Using cached SQLAlchemy-2.0.36-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (3.2 MB)
    Installing collected packages: sqlalchemy
      Attempting uninstall: sqlalchemy
        Found existing installation: SQLAlchemy 1.4.54
        Uninstalling SQLAlchemy-1.4.54:
          Successfully uninstalled SQLAlchemy-1.4.54
    Successfully installed sqlalchemy-2.0.36



```python
%load_ext sql
```

    The sql extension is already loaded. To reload it, use:
      %reload_ext sql



```python
import csv, sqlite3

con = sqlite3.connect("my_data1.db")
cur = con.cursor()
```


```python
!pip install -q pandas
```


```python
%sql sqlite:///my_data1.db
```


```python
import pandas as pd
df = pd.read_csv("https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DS0321EN-SkillsNetwork/labs/module_2/data/Spacex.csv")
df.to_sql("SPACEXTBL", con, if_exists='replace', index=False,method="multi")
```




    101



**Note:This below code is added to remove blank rows from table**



```python
#DROP THE TABLE IF EXISTS

%sql DROP TABLE IF EXISTS SPACEXTABLE;
```

     * sqlite:///my_data1.db
    Done.





    []




```python
%sql create table SPACEXTABLE as select * from SPACEXTBL where Date is not null
```

     * sqlite:///my_data1.db
    Done.





    []



## Tasks

Now write and execute SQL queries to solve the assignment tasks.

**Note: If the column names are in mixed case enclose it in double quotes
   For Example "Landing_Outcome"**

### Task 1




##### Display the names of the unique launch sites  in the space mission



```python
df.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Date</th>
      <th>Time (UTC)</th>
      <th>Booster_Version</th>
      <th>Launch_Site</th>
      <th>Payload</th>
      <th>PAYLOAD_MASS__KG_</th>
      <th>Orbit</th>
      <th>Customer</th>
      <th>Mission_Outcome</th>
      <th>Landing_Outcome</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2010-06-04</td>
      <td>18:45:00</td>
      <td>F9 v1.0  B0003</td>
      <td>CCAFS LC-40</td>
      <td>Dragon Spacecraft Qualification Unit</td>
      <td>0</td>
      <td>LEO</td>
      <td>SpaceX</td>
      <td>Success</td>
      <td>Failure (parachute)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2010-12-08</td>
      <td>15:43:00</td>
      <td>F9 v1.0  B0004</td>
      <td>CCAFS LC-40</td>
      <td>Dragon demo flight C1, two CubeSats, barrel of...</td>
      <td>0</td>
      <td>LEO (ISS)</td>
      <td>NASA (COTS) NRO</td>
      <td>Success</td>
      <td>Failure (parachute)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2012-05-22</td>
      <td>7:44:00</td>
      <td>F9 v1.0  B0005</td>
      <td>CCAFS LC-40</td>
      <td>Dragon demo flight C2</td>
      <td>525</td>
      <td>LEO (ISS)</td>
      <td>NASA (COTS)</td>
      <td>Success</td>
      <td>No attempt</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2012-10-08</td>
      <td>0:35:00</td>
      <td>F9 v1.0  B0006</td>
      <td>CCAFS LC-40</td>
      <td>SpaceX CRS-1</td>
      <td>500</td>
      <td>LEO (ISS)</td>
      <td>NASA (CRS)</td>
      <td>Success</td>
      <td>No attempt</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2013-03-01</td>
      <td>15:10:00</td>
      <td>F9 v1.0  B0007</td>
      <td>CCAFS LC-40</td>
      <td>SpaceX CRS-2</td>
      <td>677</td>
      <td>LEO (ISS)</td>
      <td>NASA (CRS)</td>
      <td>Success</td>
      <td>No attempt</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2013-09-29</td>
      <td>16:00:00</td>
      <td>F9 v1.1  B1003</td>
      <td>VAFB SLC-4E</td>
      <td>CASSIOPE</td>
      <td>500</td>
      <td>Polar LEO</td>
      <td>MDA</td>
      <td>Success</td>
      <td>Uncontrolled (ocean)</td>
    </tr>
    <tr>
      <th>6</th>
      <td>2013-12-03</td>
      <td>22:41:00</td>
      <td>F9 v1.1</td>
      <td>CCAFS LC-40</td>
      <td>SES-8</td>
      <td>3170</td>
      <td>GTO</td>
      <td>SES</td>
      <td>Success</td>
      <td>No attempt</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2014-01-06</td>
      <td>22:06:00</td>
      <td>F9 v1.1</td>
      <td>CCAFS LC-40</td>
      <td>Thaicom 6</td>
      <td>3325</td>
      <td>GTO</td>
      <td>Thaicom</td>
      <td>Success</td>
      <td>No attempt</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2014-04-18</td>
      <td>19:25:00</td>
      <td>F9 v1.1</td>
      <td>CCAFS LC-40</td>
      <td>SpaceX CRS-3</td>
      <td>2296</td>
      <td>LEO (ISS)</td>
      <td>NASA (CRS)</td>
      <td>Success</td>
      <td>Controlled (ocean)</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2014-07-14</td>
      <td>15:15:00</td>
      <td>F9 v1.1</td>
      <td>CCAFS LC-40</td>
      <td>OG2 Mission 1  6 Orbcomm-OG2 satellites</td>
      <td>1316</td>
      <td>LEO</td>
      <td>Orbcomm</td>
      <td>Success</td>
      <td>Controlled (ocean)</td>
    </tr>
  </tbody>
</table>
</div>




```python
!pip install --upgrade ipython-sql

```

    Requirement already satisfied: ipython-sql in /opt/conda/lib/python3.11/site-packages (0.5.0)
    Requirement already satisfied: prettytable in /opt/conda/lib/python3.11/site-packages (from ipython-sql) (3.12.0)
    Requirement already satisfied: ipython in /opt/conda/lib/python3.11/site-packages (from ipython-sql) (8.22.2)
    Requirement already satisfied: sqlalchemy>=2.0 in /opt/conda/lib/python3.11/site-packages (from ipython-sql) (2.0.36)
    Requirement already satisfied: sqlparse in /opt/conda/lib/python3.11/site-packages (from ipython-sql) (0.5.2)
    Requirement already satisfied: six in /opt/conda/lib/python3.11/site-packages (from ipython-sql) (1.16.0)
    Requirement already satisfied: ipython-genutils in /opt/conda/lib/python3.11/site-packages (from ipython-sql) (0.2.0)
    Requirement already satisfied: typing-extensions>=4.6.0 in /opt/conda/lib/python3.11/site-packages (from sqlalchemy>=2.0->ipython-sql) (4.12.2)
    Requirement already satisfied: greenlet!=0.4.17 in /opt/conda/lib/python3.11/site-packages (from sqlalchemy>=2.0->ipython-sql) (3.0.3)
    Requirement already satisfied: decorator in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (5.1.1)
    Requirement already satisfied: jedi>=0.16 in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (0.19.1)
    Requirement already satisfied: matplotlib-inline in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (0.1.7)
    Requirement already satisfied: prompt-toolkit<3.1.0,>=3.0.41 in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (3.0.42)
    Requirement already satisfied: pygments>=2.4.0 in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (2.18.0)
    Requirement already satisfied: stack-data in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (0.6.2)
    Requirement already satisfied: traitlets>=5.13.0 in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (5.14.3)
    Requirement already satisfied: pexpect>4.3 in /opt/conda/lib/python3.11/site-packages (from ipython->ipython-sql) (4.9.0)
    Requirement already satisfied: wcwidth in /opt/conda/lib/python3.11/site-packages (from prettytable->ipython-sql) (0.2.13)
    Requirement already satisfied: parso<0.9.0,>=0.8.3 in /opt/conda/lib/python3.11/site-packages (from jedi>=0.16->ipython->ipython-sql) (0.8.4)
    Requirement already satisfied: ptyprocess>=0.5 in /opt/conda/lib/python3.11/site-packages (from pexpect>4.3->ipython->ipython-sql) (0.7.0)
    Requirement already satisfied: executing>=1.2.0 in /opt/conda/lib/python3.11/site-packages (from stack-data->ipython->ipython-sql) (2.0.1)
    Requirement already satisfied: asttokens>=2.1.0 in /opt/conda/lib/python3.11/site-packages (from stack-data->ipython->ipython-sql) (2.4.1)
    Requirement already satisfied: pure-eval in /opt/conda/lib/python3.11/site-packages (from stack-data->ipython->ipython-sql) (0.2.2)



```python
conn = sqlite3.connect("my_data1.db")

query = "SELECT * FROM SPACEXTBL LIMIT 1;"
df = pd.read_sql_query(query, conn)
print(df)
```

             Date Time (UTC) Booster_Version  Launch_Site  \
    0  2010-06-04   18:45:00  F9 v1.0  B0003  CCAFS LC-40   
    
                                    Payload  PAYLOAD_MASS__KG_ Orbit Customer  \
    0  Dragon Spacecraft Qualification Unit                  0   LEO   SpaceX   
    
      Mission_Outcome      Landing_Outcome  
    0         Success  Failure (parachute)  



### Task 2


#####  Display 5 records where launch sites begin with the string 'CCA' 



```python
%sql SELECT Launch_Site LIKE "CCA%" FROM SPACEXTBL LIMIT 5;
```

     * sqlite:///my_data1.db
    Done.



    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    Cell In[42], line 1
    ----> 1 get_ipython().run_line_magic('sql', 'SELECT Launch_Site LIKE "CCA%" FROM SPACEXTBL LIMIT 5;')


    File /opt/conda/lib/python3.11/site-packages/IPython/core/interactiveshell.py:2480, in InteractiveShell.run_line_magic(self, magic_name, line, _stack_depth)
       2478     kwargs['local_ns'] = self.get_local_scope(stack_depth)
       2479 with self.builtin_trap:
    -> 2480     result = fn(*args, **kwargs)
       2482 # The code below prevents the output from being displayed
       2483 # when using magics with decorator @output_can_be_silenced
       2484 # when the last Python token in the expression is a ';'.
       2485 if getattr(fn, magic.MAGIC_OUTPUT_CAN_BE_SILENCED, False):


    File /opt/conda/lib/python3.11/site-packages/sql/magic.py:219, in SqlMagic.execute(self, line, cell, local_ns)
        216     return
        218 try:
    --> 219     result = sql.run.run(conn, parsed["sql"], self, user_ns)
        221     if (
        222         result is not None
        223         and not isinstance(result, str)
       (...)
        226         # Instead of returning values, set variables directly in the
        227         # user's namespace. Variable names given by column names
        229         if self.autopandas:


    File /opt/conda/lib/python3.11/site-packages/sql/run.py:374, in run(conn, sql, config, user_namespace)
        372     if result and config.feedback:
        373         print(interpret_rowcount(result.rowcount))
    --> 374 resultset = ResultSet(result, config)
        375 if config.autopandas:
        376     return resultset.DataFrame()


    File /opt/conda/lib/python3.11/site-packages/sql/run.py:116, in ResultSet.__init__(self, sqlaproxy, config)
        114         list.__init__(self, sqlaproxy.fetchall())
        115     self.field_names = unduplicate_field_names(self.keys)
    --> 116     self.pretty = PrettyTable(self.field_names, style=prettytable.__dict__[config.style.upper()])
        117 else:
        118     list.__init__(self, [])


    KeyError: 'TABLE'


### Task 3




##### Display the total payload mass carried by boosters launched by NASA (CRS)



```python
%sql SELECT SUM("PAYLOAD_MASS__KG_") AS total_payload_mass FROM SPACEXTBL;
```

     * sqlite:///my_data1.db
    Done.



    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    Cell In[24], line 1
    ----> 1 get_ipython().run_line_magic('sql', 'SELECT SUM("PAYLOAD_MASS__KG_") AS total_payload_mass FROM SPACEXTBL;')


    File /opt/conda/lib/python3.11/site-packages/IPython/core/interactiveshell.py:2480, in InteractiveShell.run_line_magic(self, magic_name, line, _stack_depth)
       2478     kwargs['local_ns'] = self.get_local_scope(stack_depth)
       2479 with self.builtin_trap:
    -> 2480     result = fn(*args, **kwargs)
       2482 # The code below prevents the output from being displayed
       2483 # when using magics with decorator @output_can_be_silenced
       2484 # when the last Python token in the expression is a ';'.
       2485 if getattr(fn, magic.MAGIC_OUTPUT_CAN_BE_SILENCED, False):


    File /opt/conda/lib/python3.11/site-packages/sql/magic.py:219, in SqlMagic.execute(self, line, cell, local_ns)
        216     return
        218 try:
    --> 219     result = sql.run.run(conn, parsed["sql"], self, user_ns)
        221     if (
        222         result is not None
        223         and not isinstance(result, str)
       (...)
        226         # Instead of returning values, set variables directly in the
        227         # user's namespace. Variable names given by column names
        229         if self.autopandas:


    File /opt/conda/lib/python3.11/site-packages/sql/run.py:374, in run(conn, sql, config, user_namespace)
        372     if result and config.feedback:
        373         print(interpret_rowcount(result.rowcount))
    --> 374 resultset = ResultSet(result, config)
        375 if config.autopandas:
        376     return resultset.DataFrame()


    File /opt/conda/lib/python3.11/site-packages/sql/run.py:116, in ResultSet.__init__(self, sqlaproxy, config)
        114         list.__init__(self, sqlaproxy.fetchall())
        115     self.field_names = unduplicate_field_names(self.keys)
    --> 116     self.pretty = PrettyTable(self.field_names, style=prettytable.__dict__[config.style.upper()])
        117 else:
        118     list.__init__(self, [])


    KeyError: 'DEFAULT'


### Task 4




##### Display average payload mass carried by booster version F9 v1.1



```python
%sql SELECT AVG("PAYLOAD_MASS__KG_") AS average_payload_mass FROM SPACEXTBL WHERE Booster_Version ="F9 v1.1";
```

     * sqlite:///my_data1.db
    Done.



    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    Cell In[25], line 1
    ----> 1 get_ipython().run_line_magic('sql', 'SELECT AVG("PAYLOAD_MASS__KG_") AS average_payload_mass FROM SPACEXTBL WHERE Booster_Version ="F9 v1.1";')


    File /opt/conda/lib/python3.11/site-packages/IPython/core/interactiveshell.py:2480, in InteractiveShell.run_line_magic(self, magic_name, line, _stack_depth)
       2478     kwargs['local_ns'] = self.get_local_scope(stack_depth)
       2479 with self.builtin_trap:
    -> 2480     result = fn(*args, **kwargs)
       2482 # The code below prevents the output from being displayed
       2483 # when using magics with decorator @output_can_be_silenced
       2484 # when the last Python token in the expression is a ';'.
       2485 if getattr(fn, magic.MAGIC_OUTPUT_CAN_BE_SILENCED, False):


    File /opt/conda/lib/python3.11/site-packages/sql/magic.py:219, in SqlMagic.execute(self, line, cell, local_ns)
        216     return
        218 try:
    --> 219     result = sql.run.run(conn, parsed["sql"], self, user_ns)
        221     if (
        222         result is not None
        223         and not isinstance(result, str)
       (...)
        226         # Instead of returning values, set variables directly in the
        227         # user's namespace. Variable names given by column names
        229         if self.autopandas:


    File /opt/conda/lib/python3.11/site-packages/sql/run.py:374, in run(conn, sql, config, user_namespace)
        372     if result and config.feedback:
        373         print(interpret_rowcount(result.rowcount))
    --> 374 resultset = ResultSet(result, config)
        375 if config.autopandas:
        376     return resultset.DataFrame()


    File /opt/conda/lib/python3.11/site-packages/sql/run.py:116, in ResultSet.__init__(self, sqlaproxy, config)
        114         list.__init__(self, sqlaproxy.fetchall())
        115     self.field_names = unduplicate_field_names(self.keys)
    --> 116     self.pretty = PrettyTable(self.field_names, style=prettytable.__dict__[config.style.upper()])
        117 else:
        118     list.__init__(self, [])


    KeyError: 'DEFAULT'


### Task 5

##### List the date when the first succesful landing outcome in ground pad was acheived.


_Hint:Use min function_ 



```python
%sql SELECT MIN(date) AS first_successfull_landing_date FROM SPACEXTBL WHERE "Landing_Outcome" = "Success (ground pad)";
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>first_successfull_landing_date</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>2015-12-22</td>
        </tr>
    </tbody>
</table>



### Task 6

##### List the names of the boosters which have success in drone ship and have payload mass greater than 4000 but less than 6000



```python
%sql SELECT Booster_Version AS new_list FROM SPACEXTBL WHERE PAYLOAD_MASS__KG_ BETWEEN 4000 AND 6000;
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>new_list</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>F9 v1.1</td>
        </tr>
        <tr>
            <td>F9 v1.1 B1011</td>
        </tr>
        <tr>
            <td>F9 v1.1 B1014</td>
        </tr>
        <tr>
            <td>F9 v1.1 B1016</td>
        </tr>
        <tr>
            <td>F9 FT B1020</td>
        </tr>
        <tr>
            <td>F9 FT B1022</td>
        </tr>
        <tr>
            <td>F9 FT B1026</td>
        </tr>
        <tr>
            <td>F9 FT B1030</td>
        </tr>
        <tr>
            <td>F9 FT  B1021.2</td>
        </tr>
        <tr>
            <td>F9 FT B1032.1</td>
        </tr>
        <tr>
            <td>F9 B4 B1040.1</td>
        </tr>
        <tr>
            <td>F9 FT  B1031.2</td>
        </tr>
        <tr>
            <td>F9 B4 B1043.1</td>
        </tr>
        <tr>
            <td>F9 FT  B1032.2</td>
        </tr>
        <tr>
            <td>F9 B4  B1040.2</td>
        </tr>
        <tr>
            <td>F9 B5 B1046.2</td>
        </tr>
        <tr>
            <td>F9 B5 B1047.2</td>
        </tr>
        <tr>
            <td>F9 B5 B1046.3</td>
        </tr>
        <tr>
            <td>F9 B5B1054</td>
        </tr>
        <tr>
            <td>F9 B5 B1048.3</td>
        </tr>
        <tr>
            <td>F9 B5 B1051.2 </td>
        </tr>
        <tr>
            <td>F9 B5B1060.1</td>
        </tr>
        <tr>
            <td>F9 B5 B1058.2 </td>
        </tr>
        <tr>
            <td>F9 B5B1062.1</td>
        </tr>
    </tbody>
</table>



### Task 7




##### List the total number of successful and failure mission outcomes



```python
%sql SELECT Mission_Outcome, COUNT(*) AS total_count FROM SPACEXTBL GROUP BY Mission_Outcome;
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Mission_Outcome</th>
            <th>total_count</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Failure (in flight)</td>
            <td>1</td>
        </tr>
        <tr>
            <td>Success</td>
            <td>98</td>
        </tr>
        <tr>
            <td>Success </td>
            <td>1</td>
        </tr>
        <tr>
            <td>Success (payload status unclear)</td>
            <td>1</td>
        </tr>
    </tbody>
</table>



### Task 8



##### List the   names of the booster_versions which have carried the maximum payload mass. Use a subquery



```python
%sql SELECT Booster_Version FROM SPACEXTBL WHERE PAYLOAD_MASS__KG_ = (SELECT MAX(PAYLOAD_MASS__KG_) FROM SPACEXTBL);
```

     * sqlite:///my_data1.db
    Done.





<table>
    <thead>
        <tr>
            <th>Booster_Version</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>F9 B5 B1048.4</td>
        </tr>
        <tr>
            <td>F9 B5 B1049.4</td>
        </tr>
        <tr>
            <td>F9 B5 B1051.3</td>
        </tr>
        <tr>
            <td>F9 B5 B1056.4</td>
        </tr>
        <tr>
            <td>F9 B5 B1048.5</td>
        </tr>
        <tr>
            <td>F9 B5 B1051.4</td>
        </tr>
        <tr>
            <td>F9 B5 B1049.5</td>
        </tr>
        <tr>
            <td>F9 B5 B1060.2 </td>
        </tr>
        <tr>
            <td>F9 B5 B1058.3 </td>
        </tr>
        <tr>
            <td>F9 B5 B1051.6</td>
        </tr>
        <tr>
            <td>F9 B5 B1060.3</td>
        </tr>
        <tr>
            <td>F9 B5 B1049.7 </td>
        </tr>
    </tbody>
</table>



### Task 9


##### List the records which will display the month names, failure landing_outcomes in drone ship ,booster versions, launch_site for the months in year 2015.

**Note: SQLLite does not support monthnames. So you need to use  substr(Date, 6,2) as month to get the months and substr(Date,0,5)='2015' for year.**



```python
%sql SELECT 
    CASE substr(Date, 6, 2)
        WHEN '01' THEN 'January'
        WHEN '02' THEN 'February'
        WHEN '03' THEN 'March'
        WHEN '04' THEN 'April'
        WHEN '05' THEN 'May'
        WHEN '06' THEN 'June'
        WHEN '07' THEN 'July'
        WHEN '08' THEN 'August'
        WHEN '09' THEN 'September'
        WHEN '10' THEN 'October'
        WHEN '11' THEN 'November'
        WHEN '12' THEN 'December'
        ELSE 'Unknown'
    END AS month_name,
    Landing_Outcome,
    Booster_Version,
    Launch_Site
FROM SPACEXTBL
WHERE landing_outcome = 'Failure'
  AND landing_location = 'Drone Ship'
  AND substr(Date, 1, 4) = '2015';
```


      Cell In[1], line 28
        CASE
        ^
    IndentationError: unexpected indent



### Task 10




##### Rank the count of landing outcomes (such as Failure (drone ship) or Success (ground pad)) between the date 2010-06-04 and 2017-03-20, in descending order.



```python
%sql
SELECT Date, 
       Time, 
       Booster_Version, 
       Launch_Site, 
       Landing_Outcome
FROM SPACEXTBL
WHERE Landing_Outcome = 'Drone Ship'
  AND Booster_Version LIKE 'F9%'
  AND substr(Date, 1, 4) BETWEEN '2010' AND '2017'
ORDER BY Date DESC;
```


      Cell In[43], line 2
        SELECT Date,
               ^
    SyntaxError: invalid syntax



### Reference Links

* <a href ="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Labs_Coursera_V5/labs/Lab%20-%20String%20Patterns%20-%20Sorting%20-%20Grouping/instructional-labs.md.html?origin=www.coursera.org">Hands-on Lab : String Patterns, Sorting and Grouping</a>  

*  <a  href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Labs_Coursera_V5/labs/Lab%20-%20Built-in%20functions%20/Hands-on_Lab__Built-in_Functions.md.html?origin=www.coursera.org">Hands-on Lab: Built-in functions</a>

*  <a  href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Labs_Coursera_V5/labs/Lab%20-%20Sub-queries%20and%20Nested%20SELECTs%20/instructional-labs.md.html?origin=www.coursera.org">Hands-on Lab : Sub-queries and Nested SELECT Statements</a>

*   <a href="https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Module%205/DB0201EN-Week3-1-3-SQLmagic.ipynb">Hands-on Tutorial: Accessing Databases with SQL magic</a>

*  <a href= "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/Module%205/DB0201EN-Week3-1-4-Analyzing.ipynb">Hands-on Lab: Analyzing a real World Data Set</a>




## Author(s)

<h4> Lakshmi Holla </h4>


## Other Contributors

<h4> Rav Ahuja </h4>


<!--
## Change log
| Date | Version | Changed by | Change Description |
|------|--------|--------|---------|
| 2024-07-10 | 1.1 |Anita Verma | Changed Version|
| 2021-07-09 | 0.2 |Lakshmi Holla | Changes made in magic sql|
| 2021-05-20 | 0.1 |Lakshmi Holla | Created Initial Version |
-->


## <h3 align="center"> © IBM Corporation 2021. All rights reserved. <h3/>

