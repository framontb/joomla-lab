
# CREANDO LABORATORIO DOCKER PARA JOOMLA 4: COM_PENHA

1. Se usará la ya creada red: `joomla_net`
```
docker network create --driver bridge joomla_net --subnet=172.18.0.0/24

docker network inspect joomla_net

framontb@Aura-15:~$ docker network ls
NETWORK ID     NAME           DRIVER    SCOPE
1628e17feb10   joomla_net     bridge    local

```

2. mariadb_penha
```
docker run -e MYSQL_ROOT_PASSWORD=S0cr4R00t -e MYSQL_USER=jumer -e MYSQL_PASSWORD=S0cr4Mysql -e MYSQL_DATABASE=penhaDB -v /home/framontb/Documentos/dev/joomla4/labs/penha/mariadb:/var/lib/mysql --net joomla_net --ip 172.18.0.50 --name mariadb_penha -d mariadb
```

3. joomla_penha
```
docker run -e JOOMLA_DB_HOST=172.18.0.50 -e JOOMLA_DB_USER=jumer -e JOOMLA_DB_PASSWORD=S0cr4tes -e JOOMLA_DB_NAME=penhaDB -v /home/framontb/Documentos/dev/joomla4/labs/penha:/var/www/html --net joomla_net --ip 172.18.0.51 --name joomla_penha -d joomla
```

4. Attach (para ver mensajes en consola de debug)
```
docker container attach joomla_penha

detach: CTRL + p CTRL + q
```


## Acceso lab

```
```

* Hosting:      http://joomla_penha/administrator/
    * Hosting:      http://172.18.0.51/administrator/
    * Usuario:      socrates
    * Pass:         S0cr4tesS0cr4tes


## User access to files: Visual Studio Code

```
usermod -a -G www-data framontb
sudo chmod g+w tmp

framontb@Aura-15:~/Documentos/dev/joomla4/labs$ sudo chmod g+w penha/ -R
```

# PHPMYADMIN

* [PHPMYADMIN](http://mariadb_penha/phpmyadmin/index.php)

## INSTALAR phpmyadmin en CONTENEDOR MARIADB > la primera vez

* [Cómo instalar y proteger phpMyAdmin en Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-phpmyadmin-on-ubuntu-20-04-es)

0. docker exec -it mariadb_penha bash
1. `apt update`
2. `apt install phpmyadmin`
    * apache2
    * Configure database for phpmyadmin with dbconfig-common? [yes/no] => no
The phpmyadmin package must have a database installed and configured before it can be used. This can be optionally handled with dbconfig-common.
If you are an advanced database administrator and know that you want to perform this configuration manually, or if your database has already been installed
and configured, you should refuse this option. Details on what needs to be done should most likely be provided in /usr/share/doc/phpmyadmin.
Otherwise, you should probably choose this option.

3. Configurar apache
    *  apt install vim
    *  vi /etc/apache2/apache2.conf
        * ServerName mariadb_penha
    * root@d34584e0959e:/etc/apache2# apachectl restart

4. Acceso navegador:
    * http://mariadb_penha/phpmyadmin/index.php
    * root / ***


# Addenda

* [Networking with standalone containers](https://docs.docker.com/network/network-tutorial-standalone/)

* [Post-installation steps for Linux](https://docs.docker.com/engine/install/linux-postinstall/)


