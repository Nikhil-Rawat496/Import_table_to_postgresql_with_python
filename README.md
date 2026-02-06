# Import_table_to_postgresql_with_python
In this repo i'm sharing the python program which will import full table with data in postgresql without creating table in postgresql manually. I am using three libraries (pandas:to get data in python, create_engine of sqlalchemy: to make connection and create now table in postgres, getpass:to securey access passowrd) to do this task.

import pandas as pd
from sqlalchemy import create_engine
import getpass

def connection_with_action(data):
    password = getpass.getpass("Enter the password you entered while downoading postgresql: ")   # to get password in secure way
    
    #necessory parameters for connection with postgresql
    username = 'postgres'  #Dafault username 
    password = password    #get from getpass
    host = 'localhost'     #default in every SQL databases
    port = '5432'          #the default port in postgres to access the database
    database = 'Practice'  #database we want to connect and create table in

    try:
        engine = create_engine(f"postgresql://{username}:{password}@{host}:{port}/{database}")   #function to connecting with database
        
        data.to_sql('New_table', engine, index=False, if_exists='replace')  #function of create_engine for create new table in database
        
        print("Connection successful and table created in database")
    except Exception as e:
        print(f"Connection failed dur to: {e} Error.")
    finally:
        print("Exiting..")
        
data_to_export = pd.read_csv("C:/Users/dell/OneDrive/Desktop/SQL_Full_Dataset.csv")      #path of the data file with changing the '\' to '/' 
connection_with_action(data_to_export)





A small example script that imports a CSV into a PostgreSQL table using pandas and SQLAlchemy. The script creates the table automatically (using DataFrame.to_sql).

## Requirements

- Python 3.8+
- pandas
- SQLAlchemy
- psycopg2-binary (Postgres driver)

Install dependencies:
run in cmd to get libraries 
pip install pandas sqlalchemy psycopg2-binary


## Files

- `import_to_postgres.py` — script to import a CSV into PostgreSQL.
- `README.md` — this file with usage instructions.

## Usage

Interactive (prompts for password):
```bash
python import_to_postgres.py --csv data.csv --table my_table --database Practice
```

Non-interactive (use environment variable):
```bash
export DATABASE_URL="postgresql+psycopg2://username:password@host:5432/dbname"
python import_to_postgres.py --csv data.csv --table my_table --if-exists replace
```

## Notes

- `--if-exists` options: `fail`, `replace`, `append`. Default is `replace`.
- Avoid committing credentials to the repository.
- The script will attempt to create the table based on the CSV columns and types.
