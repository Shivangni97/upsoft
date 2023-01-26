# upsoft
# importing the libraries
import numpy as np
import pandas as pd
from sklearn.ensemble import IsolationForest
# Reading the file
df=pd.read_csv(r'C:\Users\Shivangni\Downloads\data112.txt')
df.head()
print(df.T)
# Transposing the dataframe
dff=df.T
dff.head()
model=IsolationForest(n_estimators=100,max_samples='auto',contamination=float(0.2),max_features=1.0)
dff.shape
dff.info()
dff['values']=dff.index
dff.head(20)
dff.reset_index(level=0, inplace=True)
dff.head()
dd=dff.drop(['index'],axis=1)
dd.info()
dd.head()
dd['values'].value_counts()
# Deleting the last record
print(dd.iloc[[10000]])
dd.drop([10000],axis=0,inplace=True)
dd['values'].value_counts()
dd['values'].astype(float)
model.fit(dd[['values']])
dd['anomalies_scores']=model.decision_function(dd[['values']])
dd['anomaly']=model.predict(dd[['values']])
dd.head()
dd['anomaly'].value_counts()
# 1 means no anomaly, -1 means anomaly present
# Saving the file
datatoexcel = pd.ExcelWriter('motor.xlsx')
dd.to_excel(datatoexcel)
datatoexcel.save()
