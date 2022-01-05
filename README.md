# MARIADB
COMANDOS DOCKER + MARIADB

## Crear network

``` 
docker network create lab
```

## Crear contenedor de pruebas con mariadb:latest1

```
docker run -net lab --name db01 -e MYSQL_ROOT_PASSWORD=password -d mariadb:latest
```

## Conectarnos al contenedor y comprobar la variable de entorno
```
docker exec -it db01 bash
env
```

### Conectarnos a mysql y listar bases de datos
```
mysql -uroot -ppassw0rd
show databases;
```
### conectar a la base de datos desde otro contenedor y listar bases de datos

```
docker run -it --name test --network lab --rm mariadb:latest mysql -hdb01 -uroot -p
show databases;
```
### Importamos la base de datos

```
docker exec -i db01 sh -c 'exec mysql -uroot -p"$MYSQL_ROOT_PASSWORD"' < dump/users.sql
```


### conectar a la base de datos desde otro contenedor y listar los cambios

```
docker run -it --name test --network lab --rm mariadb:latest mysql -hdb01 -uroot -p
show databases;
use users;
show tables;
select * from users;
```
## Crear la imagen de nuestra aplicacion

``` 
cd app
docker build -t python . 
```

## Ejecutar nuestra imagen

```
docker run --network lab -e db_host=db01 -e db_user=root -e MYSQL_ROOT_PASSWORD=passw0rd -e db_name=users --name python --rm -p 5000:5000 python:latest
```

## Conectamos a la base de datos e insertamos nuevos datos

```
docker exec -i db01 bash
mysql -uroot -ppassw0rd
use users;
select * from user;
insert into user values(2,"Warren")
select * from user;
```
