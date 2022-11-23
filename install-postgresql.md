Uninstall and remove PostgreSQL on Debian Linux
You can use the apt-get command to completely remove PostgreSQL on a Debian-based distribution of Linux such as Linux Mint or Ubuntu:
```
sudo apt-get --purge remove postgresql
sudo apt-get purge postgresql*
sudo apt-get --purge remove postgresql postgresql-doc postgresql-common
```

Grep for all PostgreSQL packages in Debian Linux
You can use the dpkg command for managing Debian packages, in conjunction with grep, to search for all the package names installed that contain the sub-string postgres. An example of this command is shown below:
```
dpkg -l | grep postgres
1
sudo apt-get --purge remove {POSTGRESS-PACKAGE NAME}
```

Remove all of the PostgreSQL data and directories
Use the rm command with the -rf options to recursively remove all of the directories and data for the postgresql packages:
```
sudo rm -rf /var/lib/postgresql/
sudo rm -rf /var/log/postgresql/
sudo rm -rf /etc/postgresql/
```

setup postgrsql
```
sudo apt update
sudo apt install postgresql postgresql-contrib
sudo systemctl start postgresql.service
```

sign in as postgres user account
`sudo -i -u postgres`

`psql`

or Another way to connect to the Postgres prompt is to run the psql command as the postgres account directly with sudo:
`sudo -u postgres psql`

change password for user 'postgres'
`ALTER USER postgres PASSWORD 'myPassword';`

Find database config file:
```
psql
SHOW config_file;
```
output:
`/etc/postgresql/14/main/postgresql.conf`

`nano /etc/postgresql/14/main/postgresql.conf`
change 
```
#listen_addresses = 'localhost'
listen_addresses = '*'
```

`sudo nano /etc/postgresql/14/main/pg_hba.conf`

Add the following:
```
# TYPE  DATABASE        USER            ADDRESS                 METHOD
host    all             all             0.0.0.0/0               md5
host    all             all             ::0/0                   md5
```

Restart
`sudo systemctl restart postgresql`

Open port 5432
`sudo ufw allow 5432`

