## Host a django app with postgresql database

- Install postgresql 
If you are a debian user install postgresql with the following commands
```
sudo apt install postgresql-9.6
```

- Install the Virtual Environment and launch it
```
python3 -m venv shit
cd shit
source ./bin/activate
cd ..
```

- Clone the demo repo
```
https://github.com/nightwarrior-xxx/djangoapp_with_postgresql_sample.git
```
(Even I have cloned this from someone else and credit of this repo goes to Azure team and contributors)

- Install the requirements
```
cd djangoapp_with_postgresql_sample
pip install -r requirements.txt
```

- Create a database in postgresql
```
sudo -u postgres psql postgres
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;
```

- Run the website locally
```
python3 manage.py runserver <port>
```

- Create an azure postgresql database
```
az postgres server create \
--resource-group <resource group> \
--name <postgresql-name> \
--admin-username <admin-name> \
--admin-password <password> \
--sku-name B_Gen5_1 \
--location "West Europe"
```

- Create firewall rule for postgresql to allow access to all
```
az postgres server firewall-rule create \
--resource-group <resource-group> \
--server-name <server-name> \
--start-ip-address=<ip> \
--end-ip-address=<ip> \
--name <AllowAllIps>
```

- Allow access to yourself in postgresql server with you local [IPV4 address](https://www.whatsmyip.org/)
```
az postgres server firewall-rule create \
--resource-group <resource-group> \
--server-name <postgresql-name> \
--start-ip-address=<your-ip-address> \
--end-ip-address=<your-ip-address> \
--name AllowLocalClient
```
*Choose the second option of firewall rule*

- Create an empty database and username
```
psql -h <postgresql-name>.postgres.database.azure.com -U <admin-username>@<postgresql-name> postgres
```

In postgres run the following commands
```
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;

```
