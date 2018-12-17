Docker:

Docker allows software services to run inside of containers. For example, MongoDB or MySQL can be run inside containers that are separate from your OS and whole local machine environment. 

Instead of building an app on one machine and then trying to mimic this environment on another machine, that may have a different OS or version, you can run it inside a container and it will be the same no matter what machine you run it on.

Docker can sometimes be viewed as another virtual machine. However, when compared to virtual machines, such as VMware, they are quiet different.

For instance, VMware has a single server from which when an application is created it gets its own OS and VM to run from. This is repeated for each new application added.
Docker has a single server and single OS from which containers are built on where the app is stored. So no multiple OS systems and VM's running.

Companies, such as Google, are moving towards Containerisation and emerging parts of their infrastructure will be using Docker.

When you install and are logged into Docker, open the terminal and type in the following:
'docker'
This will run a list of commands to perform tasks.

'docker version'
Displays a list of information detailing docker installation information.

'docker info'
Displays information relating to the containers created and files included.

Creating your first container:
This section will demonstrate how to create an nginx container. nginx is a web server, similar to Apache. 
To create an nginx container we type:

'docker container run -it -p 80:80 nginx'

'docker container run' - creates the container using a management command.
'-it' - interactive mode, to run in the foreground and not background.
'-p' - publish flag.
'80:80' - mapping the ports, 1st port is the one on the local machine. The 2nd is what is exposed from the container. 80 is default for nginx.
'nginx' - name of the image.


If we go to 'localhost:80' in the browser, with docker running, the nginx default screen will appear. This is not stored locally on my machine but in a container. 
Due to us including interactive mode, when we refresh 'localhost:80', the terminal information will update.

This is the image we imported:
https://hub.docker.com/_/nginx

It has a similar look to a GitHub page. It gives a variety of information on how to use nginx in Docker.

To turn off the web server, go 'ctrl c' in the terminal.

'docker container ls' - shows us our running containers.

'docker container ls -a' - shows all containers, running or not

To delete a container type:
'docker container rm ca4e' - ca4e is the first 4 digits of the container ID

To view all the images on Docker go:
'docker images'
Note: even after deleting the container, the image 'nginx' remains.
To delete the image go:
'docker image rm 568c' - 568c is the ID of the image.

Now, when we create a nginx container, it will have to pull down the image with the container creation. 
Alternatively, you can pull the image down yourself separately with the command:
'docker pull nginx'


Running nginx in the background:

'docker container run -d -p 8080:80 --name mynginx nginx'

'docker container run' - management commands
'-d' - stands for detached meaning it will run in the background
'-p 8080:80' - mapping a port, 8080 on the local machine and 80 for the container
'--name mynginx' - naming the container
'nginx' - getting the image

Creating an Apache container:
'docker container run -d -p 8081:80 --name myapache httpd'

If we run 'docker ps' we can see both servers running simultaneously.

Environment Variables:
How to create environment variables, specifically using MySQL, as seen in the link:
https://hub.docker.com/_/mysql

The following creates a MySQL container on port 3307 and uses the 'Environment Password' to set it to the password '12345'
'docker container run -d -p 3307:3307 --name mysql --env MYSQL_ROOT_PASSWORD=12345 mysql'

To stop a container from running go:
'docker container stop mysql'

Remove a container that is actually running (by force):
'docker container rm myapache -f'

Remove all containers:
'docker rm $(docker ps -aq) -f'

How to edit files in our container/on the server:
To enter a container:
'docker container exec -it mynginx bash'

To exit a container:
'exit'

To view the folders/files in the container:
'ls'

To view the HTML we see at port 8080:
'cd usr/share/nginx/html'




Adding a container with bind mount:
'docker container run -d -p 8080:80 -v $(pwd):/usr/share/nginx/html --name nginx-website nginx'

The container created is linked to the folder currently opened the terminal, i.e. the test folder. However, this test folder is currently empty. But when you add a new HTML file to this folder, it will override the original one stored at above location.
If we add a second HTML file and call it about.html. We can access it through 'localhost:8080/about.html'.





Create an image from scratch:
Create a Dockerfile file and include what is stored in the example attached.
Then in the terminal go:

'docker image build -t alancampbell/nginx-website .'

This will create an image that can be used to create a container.

To view all images go:
'docker images'

To create a container based on the 'alancampbell/nginx-website' image:
'docker container run -d -p 8082:80 alancampbell/nginx-website'

If you now go to 'localhost:8082', you will see the custom made HTML file.


To push this image to Dockerhub go:
'docker push alancampbell/nginx-website'
 





