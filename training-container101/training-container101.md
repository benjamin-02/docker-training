1: First, see all containers, images and volumes in the system. For this, enter separate listing commands. And then clean up all the containers, images and volumes on your machine. There are two ways to do this. see if you choose the easy one. 

2: Pull centos, alpine, nginx, httpd:alpine, ozgurozturknet/adanzyedocker, ozgurozturknet/hello-app, ozgurozturknet/app1 to the system you are working on. 

	`docker pull ...`
	
3: Create a container from the image named ozgurozturknet/app1.
	
	`docker container run ozgurozturknet/app1`
	
4: Create a detached container from the image named httpd:alpine. See the name and id of the container we created. 

	```
	docker container run -d httpd:alpine
	docker ps
	#or
	docker container ls
	docker container inspect <name>	
	```

5: See the logs of this container.

	`docker logs <name>`

6: Stop the container, then run it again and finally remove the container from the system. 

	```
	docker container stop hopeful_jackson
	docker container start hopeful_jackson
	docker logs hopeful_jackson
	docker container rm -f hopeful_jackson
	```

7: From the image named ozgurozturknet/adanzyedocker, create a detached container named "web_server" and publish the port with "-p 80:80". access this website from our computer's browser.

	```
	docker container run -d -p 80:80 --name web_server ozgurozturknet/adanzyedocker
	$(curl  http://127.0.0.1).Content      #With powershell
	```

8: Connect to this container called web_server, go under the /usr/local/apache2/htdocs folder and add the text "trial" to the file index.html. Switch to the web browser and refresh to see that we can add to the file. Then exit the container with exit.

	```
	docker container exec -it web_server sh
	cd /usr/local/apache2/htdocs
	echo "trial" >> index.html
	exit
	```

9: Delete the container named webserver while it is running.

	`docker container rm -f web_server`

10: Create a container from the image named alpine. But make the "ls" application run instead of the application that should run by default.

	```
	# syntax: docker container run alpine <command to be default process>
	docker container run alpine ls
	```

11: Create a volume named "vol_training1". 

	```
	docker volume create vol_training1
	docker volume ls
	```

12: Create a container named "first" in interactive mode from the image named alpine and connect to it. At the same time, mount the volume named "vol_training1" in the folder named "/test" of this container. go into this folder and create a file "abc.txt" and then add text into this file. 

	```
	docker container run -it --name first -v vol_training1:/test alpine
	cd /test
	echo "trial from first" > abc.txt
	```

13: Create a container named "second" in interactive mode from the image named alpine and connect to it.  At the same time, mount the volume named "vol_training1" in the folder named "/test" of this container. list the files in this folder and see that there is an abc.txt file. check the contents of the file. 

	```
	docker container run -it --name second -v vol_training1:/test alpine
	cat /test/abc.txt
	### trial from first
	```

14: Create a container named "third" in interactive mode from the image named alpine and connect to it.  At the same time, mount the volume named "vol_training1" in the folder named "/test" of this container but mount it as Read Only. go into this folder and try to create a file "abc1.txt". see that we cannot create it.

	```
	docker container run -it --name third -v vol_training1:/test:ro alpine                                                ─╯
	cd /test
	touch abc1.txt
	### touch: abc1.txt: Read-only file system
	```

15: create a folder on our computer "for example c:\try" and inside this folder create a file called index.html and add some text inside this file.

	`echo "hellooooooooo" > c:\try\index.html`
	
16: create a container named web_server1 from the image named ozgurozturknet/adanzyedocker, which is detached and whose port is published with "-p 80:80". Mount the folder we created on our computer to the /usr/local/apache2/htdocs folder inside the container. Open a web browser and navigate to 127.0.0.1 and see our website. Then edit the index.html file in the folder we created on our computer and add new text. refresh the web page and see them coming up.

	```
	docker container run -d -p 80:80 --name web_server1 -v c:\try:/usr/local/apache2/htdocs ozgurozturknet/adanzyedocker
	curl http://127.0.0.1
	echo "edit after creating the container" >> C:\try\index.html
	curl http://127.0.0.1
	```

17: Delete all containers. 

	```
	docker stop $(docker ps -aq)
	docker rm $(docker ps -aq)
	```
	
	