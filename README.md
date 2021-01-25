# hive2.1.1
Hive Exmples

import os
import pandas as pd
from google.cloud import bigquery

#setting credentials
os.environ['GOOGLE_APPLICATION_CREDENTIALS'] = 'C:/GCP/fit_credentials.json'
#variable
gcp_project = 'fit-cs-ops-gcp-etl-devel'
gcp_dataset = 'bqtest'
table = 'test_salesforce'


#connection
conn = bigquery.Client(project=gcp_project)
dataset_ref = conn.dataset(gcp_dataset)


def checking_connection():
    dataset_ref = conn.dataset('gcp_dataset')
    print("connection successfull")
    print("dataset_ref :",dataset_ref)

def to_gcp_load(df):
        df.to_gbq(destination_table=str(gcp_dataset+'.'+table),project_id=gcp_project,if_exists='append')

def gcp2df(sql):
        query = conn.query(sql)
        data = query.result()
        return data.to_dataframe()

query ="""select * from `fit-cs-ops-gcp-etl-devel.bqtest.test_salesforce`"""

#read it from httprespose output
data = {'TABLE_NAME': {'0':'table1'},
        'REFRESH_FREQUENCY': {'0':'10 min'},
        'LAST_PROCESSED_INFO':{'0':'02-01-2021'},
        'IS_ACTIVE':{'0':'Y'},
        'COLUMN_LIST':{'0':'Column1,column2'}
        }



df = pd.DataFrame(data)
print("data Frame",df)
checking_connection()
print("\nLOADING DATA")
to_gcp_load(df)
print("\nREADING DATA")
print(gcp2df(query))
