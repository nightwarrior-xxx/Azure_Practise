## Host a django app with postgresql database

- Install postgresql 
If you are a debian user install postgresql with the following commands
```
sudo apt install postgresql-9.6
```

- Clone the demo repo
```
https://github.com/nightwarrior-xxx/djangoapp_with_postgresql_sample.git
```
(Even I have cloned this from someone else and credit of this repo goes to Azure team and contributors)

- Launch the website locally
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

