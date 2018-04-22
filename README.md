# How to connect RDBMS Data to Ipython and Profile it (Also, find unique values in each field)

import pyodbc
import pandas as pd

#test the connection
cursor = connection.cursor()

#try to get results into pandas data frame...Pulling Claims Data ~2.7Million records into a data frame (It could take a while)
sql = ("SELECT * FROM CLAIMS_DATA_2017")
cnn = pyodbc.connect('Driver={Oracle in OraClient11g_home1};DBQ=Schema_Name;Uid=userid;Pwd=password')
data = pd.read_sql(sql, cnn)

# View the Data
data.tail()

# Basic Profiling in terms of Counts, Unique, Frequency, value with Top Frequenct 
#(Calculate Additional metrics in Excel if  needed,  for e.g. % Null etc)

data_describe=data.describe(include='all')

data_describe.to_csv('C:/Users/user/Documents/OUT/data_describe_claims.csv', sep="|")

# Obtain All Unique Values in a Dataframe in the same order as the Columns occur in the Data

# Process:
#1. While looping over each column 
#2. Assign Unique Values to a List
#3. Add this column to a temporary DataFrame
#4. Concatenate this DataFrame to a final DataFrame

#As for loop runs, all distinct values in each field get appended to the final DataFrame in the same order that the fields occur in Data  

df=pd.DataFrame()
dummy=[]

for c in data.columns:
    dummy=data[c].value_counts().index.tolist()
    df_temp = pd.DataFrame({c:dummy})
    frames = [df, df_temp]
    df = pd.concat(frames, axis=1)
    
df
df.to_csv('C:/Users/USER/Documents/OUT/unique_values_claims_data.csv', sep="|")
