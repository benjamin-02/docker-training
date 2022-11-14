1: First, clean the system so that there is no conflict with our exercises. Delete all containers and user defined bridge networks in the system, if any.



2: Create a user-defined bridge network named "training-network" on subnet 10.10.0.0/16 with ip-range 10.10.10.0/24 and gateway 10.10.10.10 (if this is a network range you use in your local network, you can use another cidr). Check the properties of this network with the inspect command. 

```
docker network create --driver=bridge --subnet=10.10.0.0/16 --ip-range=10.10.10.0/24 --gateway=10.10.10.10 training-network
docker network ls
docker network inspect training-network
```

3: Create a detached container named web1 from version 1.16 of the nginx image. Create this container connected to the training-network and not to the default bridge network. Make sure that requests to the 8080 tcp port of the host go to port 80 of this container.

```
docker container run -d --name web1 --network training-network -p 8080:80 nginx:1.16
curl 127.0.0.1
```

4: Access this system via browser and then refresh the page a few times. Then access the logs of this container and check the logs of your previous accesses. 

```
curl 127.0.0.1
curl 127.0.0.1
docker logs web1
```

5: While following the container logs in follow mode, try to navigate from the browser to a url that is not on this website. For example xyz.html Follow the error this generates live in the logs. 



6: Create a container named test1 from ozgurozturknet/adanzyedocker image. Create it with -dit and open the sh shell. Let this container be connected to the default bridge network. 

`docker container run -dit --name test1 ozgurozturknet/adanzyedocker sh`

7: Connect this container to the "alistirma-agi" network as well.

`docker network connect training-network`

8: Connect to the container with attach and try to ping web1 from inside the container. Exit the container without closing it. 

```
docker attach test1
ping web1
# success. bc they are on the same bridge network. they can reach each other with their dns name. 
# exit with Ctrl+pq escape sequence. 
```

9: Check that these containers are running and then delete them while they are running. 

```
docker ps
docker stop $(docker ps -aq)
docker rm $(docker ps -aq)
```

10: Switch to the kisim4/bolum43 folder under the education folder in the terminal. 



11: Create a detached container named websrv from the ozgurozturknet/webkayit image. Let it be connected to the "alistirma-agi" network. Restrict it to use maximum 2 logical cpu. Publish port 80 with port 80 on the host. Make sure that env.list file is passed to this container as enviroment variable. 

```
docker container run -d -p 80:80 --name websrv --network training-network --cpus="2" --env-file .\env.list ozgurozturknet/webkayit
```

12: Create a detached container named mysqldb from ozgurozturknet/webdb image. Let it be connected to the "alistirma-agi" network. Restrict it to use maximum 1gb memory. Make sure that envmysql.list file is passed to this container as enviroment variable. 

```
docker container run -d --name mysqldb --network training-network --memory=1g --env-file .\envmysql.list ozgurozturknet/webdb
```

13: Check the logs of the mysqldb container to verify that it can be properly initialized. 

```
docker logs mysqldb
###
# ...
# 2022-11-07T22:29:04.944773Z 0 [Note] Event Scheduler: Loaded 0 events
# 2022-11-07T22:29:04.945196Z 0 [Note] mysqld: ready for connections.
# Version: '5.7.28'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server (GPL)
```

14: From the browser, connect to the website published by the websrv container. Fill in the form that appears, select a jpg file and press add. Then confirm that the operation was successful by clicking see records. 

``

15: Delete the containers and training-network you created.

```
docker stop $(docker ps -aq)
docker rm $(docker ps -aq)
docker ps -a
docker network rm training-network
```


