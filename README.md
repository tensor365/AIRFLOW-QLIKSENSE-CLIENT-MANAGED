<p align="center" style="vertical-align:center;">
  <a href="https://www.qlik.com/us/products/qlik-sense">
    <img src="https://github.com/tensor365/AIRFLOW-QLIKSENSE-CLOUD/assets/13502563/780adaa3-9e0b-40e9-862f-7b0a3bb787a9" alt="qlik">
  </a>
</p>

<h1 align="center">
  Airflow: Qlik Sense Client Managed Provider
</h1>
  <h3 align="center">
    Qlik Sense Client Managed Provider to reload application, tasks from Airflow.
</h3>

<br/>

This repository provides basic qlik sense client managed hooks and operators to trigger reloads of applications, tasks available in a Qlik Sense Client Managed Site.

## Requirements

The package has been tested with Python 3.7, Python 3.8.

|  Package  |  Version  |
|-----------|-----------|
| apache-airflow | >2.0 |


## How to install it ?

By using Pypi, 

```bash
pip install airflow-provider-qlik-sense-client-managed
```

In Local,

To install it, download and unzip source and launch the following pip install command: 

```bash
pip install .
```

You can also use 

```bash
python setup.py install
```

## How to use it ?
<br/>

On this provider, you can use three way to let Airflow connecting to your Qlik Sense Client Managed Site.

• <b>NTLM Authentification</b>: Authentification with a username and password used in Qlik in everyday life. Warning: Airflow will have the same rights of the user you provide.
<br/> <br/>
• <b>JWT Authentification</b>: Authentification with a jwt token that you provide to Airflow. Warning: A JWT virtual proxy will have to be create on your Qlik Sense Site. To do this you can following the step of the section Appendix: How to create a JWT Virtual Proxy in Qlik or this article from Qlik Community: https://community.qlik.com/t5/Official-Support-Articles/Qlik-Sense-How-to-set-up-JWT-authentication/ta-p/1716226
<br/> <br/>
• <b>Certificates Authentification</b>: Authentification with Certificates. Warning: In this case, you'll have to take client certificate generated by your Qlik Sense site during installation. The port **4242** MUST be open on your Qlik Sense Site from Airflow Server. With this type of authentifcation, Airflow will use sa_api user to trigger tasks, or application reloading.
<br/><br/>

### 1. NTLM Authentification Example
<br/>

**Prerequisites**:  
<br>
• A login account with password
• URL of your Qlik Sense Site (with the virtual proxy using NTLM auth)

**Step 1**: Login in your Airflow Server. 

**Step 2**: Go into Admin > Connections > Add A New Record. 

**Step 3**: Select [NTLM] Qlik Sense Client Managed.

**Step 4** Provide following informations:
    
           • Connection Id of your choise
           • Qlik Sense Url using the NTLM Virtual Proxy
           • Qlik Username (DOMAIN\USERNAME)
           • Qlik Password

**Step 5** Save and your connection to Qlik Client Managed using NTLM auth is ready to use !


### 2. JWT Authentification Example
<br/>

**Prerequisites**:  
<br>
• A JWT token of the user that you want to trigger action with
• URL of your Qlik Sense Site (with the virtual proxy using JWT auth). If you don't have any JWT virtual proxy on your Qlik Sense Site, please follow: https://community.qlik.com/t5/Official-Support-Articles/Qlik-Sense-How-to-set-up-JWT-authentication/ta-p/1716226

**Step 1**: Login in your Airflow Server. 

**Step 2**: Go into Admin > Connections > Add A New Record. 

**Step 3**: Select [JWT] Qlik Sense Client Managed.

**Step 4** Provide following informations:
    
           • Connection Id of your choise
           • Qlik Sense Url using the JWT Virtual Proxy
           • Qlik JWT Token

**Step 5** Save and your connection to Qlik Client Managed using NTLM auth is ready to use !

### 3. Certificate Authentifcation Example
<br/>
Builing the section. No available yet.

### 4. Example: Creating a DAG with Qlik Sense Operator to reload App 

You can now use the operators in your dags to trigger a reload of an app in Qlik Sense from Airflow

Example: 

```python

from airflow.providers.qlik_sense.operators.reload_app_operator import QlikSenseReloadAppOperator
from airflow import DAG
from airflow.utils.dates import days_ago
from datetime import timedelta

default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'email': ['airflow@example.com'],
    'email_on_failure': False,
    'email_on_retry': False,
    'retries': 1,
    'retry_delay': timedelta(minutes=5)
}

idApp="" #Fill the id of your application you want to reload
connId="" #Fill the connection id you gave when creating the connection in airflow

with DAG(
    'QlikSenseReloadAppExample',
    default_args=default_args,
    description='A simple tutorial DAG reloading Qlik Sense App',
    schedule_interval=timedelta(days=1),
    start_date=days_ago(2),
    tags=['Qlik', 'Example'],
) as dag:
    
    op = QlikSenseReloadAppOperator(app_id=idApp, conn_id=connId, task_id="QlikReloadTask")
    
    op

```

<br/>

### 6. (Appendix) Configuration of JWT Virtual Proxy

Building Section not available yet

