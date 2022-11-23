Ref: [https://towardsdatascience.com/local-development-set-up-of-postgresql-with-docker-c022632f13ea]
Start Postgres Instance
```
docker run -d --name dev-postgres -e POSTGRES_PASSWORD=<password> -v ${HOME}/postgres-data/:/var/lib/postgresql/data -p 5432:5432 postgres
```

Install pgAdmin
```
docker pull dpage/pgadmin4
docker run -p 80:80 -e 'PGADMIN_DEFAULT_EMAIL=<email>' -e 'PGADMIN_DEFAULT_PASSWORD=<password>' --name dev-pgadmin -d dpage/pgadmin4
```
To get the IP address of the Postgres container, run:
`docker inspect dev-postgres -f "{{json .NetworkSettings.Networks }}"`
output:
```
{
    "bridge": {
	"IPAMConfig":null,
	"Links":null,
	"Aliases":null,
	"NetworkID":"d8333300005c9e3c30b7aaf63821a241511bc64a9285034e7547467fb681c2b9",
	"EndpointID":"5499f72fbcea774b7906340c4099c1715e115aab8eede7b2ff49b7c1e996c94e",
	"Gateway":"172.17.0.1",
	"IPAddress":"172.17.0.2",
	"IPPrefixLen":16,
	"IPv6Gateway":"",
	"GlobalIPv6Address":"",
	"GlobalIPv6PrefixLen":0,
	"MacAddress":"02:42:ac:11:00:02",
	"DriverOpts":null
    }
}
```

Access the container's command line:
`docker exec -it dev-postgres bash`

Edit conf file:
`nano /var/lib/postgresql/data/postgresql.conf`

Connect with psql
```
psql -h localhost -U postgres
```

Show listen addresses
`show listen_addresses;`

verify docker server listen values

`sudo lsof -i -P -n`

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
host    all             all             :/0                     md5
```

Restart
`sudo systemctl restart postgresql`

Open port 5432
`sudo ufw allow 5432`

