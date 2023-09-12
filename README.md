# SBSPS-Challenge-9930-1690182892
Media Monitoring Multilabel Classification :Multi-label classification of printed media articles to topics
import numpy as np
import pandas as pd

import os, types
import pandas as pd
from botocore.client import Config
import ibm_boto3

def __iter__(self): return 0

# @hidden_cell
# The following code accesses a file in your IBM Cloud Object Storage. It includes your credentials.
# You might want to remove those credentials before you share the notebook.
cos_client = ibm_boto3.client(service_name='s3',
    ibm_api_key_id='aClTeKqRPg7wYTyKGYjrlFdPCTX807eEtlOJUD0ULeTT',
    ibm_auth_endpoint="https://iam.cloud.ibm.com/oidc/token",
    config=Config(signature_version='oauth'),
    endpoint_url='https://s3.private.eu-de.cloud-object-storage.appdomain.cloud')

bucket = 'project1-donotdelete-pr-jainrcttz9iwxl'
object_key = 'Social_Network_Ads.csv'

body = cos_client.get_object(Bucket=bucket,Key=object_key)['Body']
# add missing __iter__ method, so pandas accepts body as file-like object
if not hasattr(body, "__iter__"): body.__iter__ = types.MethodType( __iter__, body )

df = pd.read_csv(body)
df.head()


'''df = pd.read_csv('/content/Social_Network_Ads.csv')
df.head()'''

df.shape
df.info()
df.isnull().sum()
df.isnull().sum()

df.columns
df.drop(['User ID'],axis=1,inplace=True)
df['Gender'].unique()
df['Gender'] = df['Gender'].replace({'Male':0,'Female':1})
df.info()
x = df.iloc[:,0:3]
x

y = df.iloc[:,3:]
y
from sklearn.model_selection import train_test_split
xtrain,xtest,ytrain,ytest = train_test_split(x,y,test_size=0.2,random_state=10)
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier
lr = LogisticRegression()
dt = DecisionTreeClassifier()
rf = RandomForestClassifier()
lr.fit(xtrain,ytrain)
dt.fit(xtrain,ytrain)
rf.fit(xtrain,ytrain)
ypred_lr = lr.predict(xtest)
ypred_dt = dt.predict(xtest)
ypred_rf = rf.predict(xtest)
ypred_lr
ypred_dt
ypred_rf
from sklearn.metrics import confusion_matrix
confusion_matrix(ytest,ypred_lr)
confusion_matrix(ytest,ypred_dt)
confusion_matrix(ytest,ypred_rf)
import pickle
pickle.dump(rf,open('classification_rf.pkl','wb'))
!pip install -U ibm-watson-machine-learning
from ibm_watson_machine_learning import APIClient
import json
import numpy as np
wml_credentials = {
    "apikey":"JDO_vmJfYTUdKabRgIDnatN5RVYNrK_zisldEAmNfoEO",
    "url":"https://eu-gb.ml.cloud.ibm.com"
}
wml_client = APIClient(wml_credentials)
wml_client.spaces.list()
wml_client.set.default_space(SPACE_ID)
