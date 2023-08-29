# Data-Science-with-Python-new
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import datetime
dataset = pd.read_csv('311_Service_Requests_from_2010_to_Present.csv')
dataset.head(10)
dataset.shape
dataset.info()
dataset.columns
drop_columns = ['Agency Name','Incident Address','Street Name','Cross Street 1','Cross Street 2','Intersection Street 1',
'Intersection Street 2','Address Type','Park Facility Name','Park Borough','School Name',
'School Number','School Region','School Code','School Phone Number','School Address','School City',
'School State','School Zip','School Not Found','School or Citywide Complaint','Vehicle Type',
'Taxi Company Borough','Taxi Pick Up Location','Bridge Highway Name','Bridge Highway Direction',
'Road Ramp','Bridge Highway Segment','Garage Lot Name','Ferry Direction','Ferry Terminal Name','Landmark',
'X Coordinate (State Plane)','Y Coordinate (State Plane)','Due Date','Resolution Action Updated Date','Community Board','Facility Type',
'Location']
dataset = dataset.drop(drop_columns,axis=1)
dataset.shape
dataset.isnull().sum()
dataset = dataset[dataset['Status']== 'Closed']
dataset.isnull().sum()
dataset = dataset.drop('Status',axis=1)
dataset.shape
dataset.isnull().sum()
dataset.dropna(subset=['Descriptor','Location Type','Incident Zip','City','Latitude','Longitude'],inplace = True)
dataset.info()
dataset.shape
dataset.dtypes
cols = ['Created Date','Closed Date']
for col in cols:
dataset[col] = pd.to_datetime(dataset[col],infer_datetime_format=True)
dataset.dtypes
dataset['Request_Closing_Time'] = dataset[cols[1]] - dataset[cols[0]]
dataset.info()
dataset.describe()
dataset.columns
dataset['Agency'].value_counts()
dataset['Complaint Type'].value_counts()
dataset['Complaint Type'].value_counts().plot(kind = 'bar',figsize=(20,10),title='Complant Type',ylabel = 'Count')
dataset['Descriptor'].value_counts()
dataset['Descriptor'].value_counts().head(10)
dataset['Descriptor'].value_counts().head(10).plot(kind = 'bar',figsize=(20,10),title='Top 10 descriptors')
dataset['Location Type'].value_counts()
dataset['Location Type'].value_counts().plot(kind='bar', figsize=(20, 10), title='Top 10 Location Type')
dataset['City'].value_counts()
dataset['City'].value_counts().head(20).plot(kind='bar',figsize=(20,10), title='Complaints in Cities', ylabel='Complaint Counts')
plt.xlabel('Cities')
dataset['Borough'].value_counts()
dataset['Borough'].value_counts().plot(kind='bar',figsize=(20,10) ,title='Borough Column', ylabel='Complaint Counts')
plt.xlabel('Borough')
top_5_complaints = dataset['Complaint Type'].value_counts()[:5].keys()
top_5_complaints
borough_complaints = dataset.groupby(['Borough', 'Complaint Type']).size().unstack()
borough_complaints = borough_complaints[top_5_complaints]
borough_complaints
top_borough = dataset['Borough'].value_counts().keys()
complaint_per_borough = dataset.groupby(['Complaint Type', 'Borough']).size().unstack()
complaint_per_borough = complaint_per_borough[top_borough]
complaint_per_borough
dataset.info()
dataset.head(5)
dataset['Request_Closing_Time_in_Hours'] = dataset['Request_Closing_Time'].astype('timedelta64[h]')+1
dataset[['Request_Closing_Time', 'Request_Closing_Time_in_Hours']]
data_avg_time_in_hrs = dataset.groupby(['City','Complaint Type'])['Request_Closing_Time_in_Hours'].mean()
data_avg_time_in_hrs.head(10)
dataset['Request_Closing_Time'].describe()
mean_hrs = dataset['Request_Closing_Time_in_Hours'].mean()
std_hrs = dataset['Request_Closing_Time_in_Hours'].std()
mean_hrs
std_hrs
