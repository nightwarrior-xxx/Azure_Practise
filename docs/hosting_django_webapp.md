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
--admin-user <admin-name> \
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

- Create an empty database and user access
```
psql -h <postgresql-name>.postgres.database.azure.com -U <admin-username>@<postgresql-name> postgres
```

In postgres run the following commands
```
CREATE DATABASE pollsdb;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE pollsdb TO manager;

```
- Change the following in ```setting.py```

```
DBHOST="<postgresql-name>.postgres.database.azure.com"
DBUSER="manager@<postgresql-name>"
DBNAME="pollsdb"
DBPASS="supersecretpass"
```

- Run the server locally again to test

```
python3 manage.py migrate
python3 manage.py createsuperuser (create superuser)
python3 manage.py runserver <port>
```

- Deploy to Azure

**Configuring repository**

Django validates the HTTP_HOST header in incoming requests. For your Django app to work in App Service, you need to add the full-qualified domain name of the app to the allowed hosts. Open ```settings.py``` and find the ALLOWED_HOSTS setting. Change the ```Allowed host = []```
to:-
```
ALLOWED_HOSTS = [os.environ['WEBSITE_SITE_NAME'] + '.azurewebsites.net', '127.0.0.1'] if 'WEBSITE_SITE_NAME' in os.environ else []
```

Also, Django doesn't support serving static files on production. So you need to enable this manually. For this we will use [```WhiteNoise```](https://whitenoise.evans.io/en/stable/). WhiteNoide allows an app to serve its own static files. So you don't need to depend to nginx and Amazon S3. It makes your work easier because now you don't have to worry about performance of you website. WhiteNoise works with any ```[WSGI-compatible]```(https://www.fullstackpython.com/wsgi-servers.html) app but has some special auto-configuration features for Django.

*Quick Start for WhiteNoise*

Add the following lines to your ```setting.py``` in the ```MIDDLEWARE``` classes.
```
MIDDLEWARE = [
      'whitenoise.middleware.WhiteNoiseMiddleware',
      ...
]
```
For forever-cacheable files and compression support

```
STATICFILES_STORAGE = 'whitenoise.storage.CompressedManifestStaticFilesStorage'
```
