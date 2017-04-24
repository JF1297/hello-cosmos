

useful terminal commands

* display current os with version:
`uname -rs`

* useful docker commands

`docker --version`

webserver:
`docker run -d -p 80:80 --name webserver nginx`
view html from server at: http://localhost/
(not necessary to append :80 to URL since default HTTP port specified
Nginx is a very high performant web server / (reverse)-proxy) / light-weight / capable of handling unlimited? requests


* view current running containers:
`docker ps`

* stop <container name>
`docker stop <container name>`

* start container
`docker start <container name>`

* see all stopped and currently running containers:
`docker ps -a`

* to stop and remove a running container with a single command:
`docker rm -f <container name>`

*list all local docker images:
`docker images`

* remover docker image:
`docker rmi <image name>`

* run in detached mode
`-d `

* expose port 80 of container, which is now mapped to port 4000 for your macbook
`-p 4000:80`
* you can view this at http://localhost:4000

* full example
`$ docker run -d -p 4000:80 friendlyhello`



*running anaconda:
`$ docker run -i -t continuumio/anaconda3 /bin/bash`
`$ docker run -i -t continuumio/miniconda3 /bin/bash`


 * start a Jupyter Notebook server with Anaconda from a Docker image:
 `$ docker run -i -t -p 8888:8888 continuumio/anaconda3 /bin/bash -c "/opt/conda/bin/conda install jupyter -y --quiet && mkdir /opt/notebooks && /opt/conda/bin/jupyter notebook --notebook-dir=/opt/notebooks --ip='*' --port=8888 --no-browser"`
 
 *you can view above jupyter notebook here:
 `http://localhost:8888`
 Once you are inside of the running notebook, you can import libraries from Anaconda, perform interactive computations and visualize your data.
 
 
 * running just python:
 `$ docker run --rm -it python:3.5 bash`
 * type <$python> to go into REPL (Read Evaluate Print Loop)
 
 # Python commands:
* `print("Hello World")`
* `print("There are %s planets" % 9)`
* `print("There are %s planets, plus %s" % (8, "pluto"))`
 `phrase = input("Enter a phrase: ")`

 `mixed_list = [1, 'a', 2.5, True]`
 
 `my_list = [1, 2, 3, 4]`
 my_list[0] # this would output --> 1
 my_list[1:3] # Includes index 1, up to, but not including index 3; so output ---> [2, 3]
 my_list[:3] # From the beginning up to index 3; so output ---> [1, 2, 3]
 my_list[2:] # From index 2 to the end (including the end); so output ---> [3, 4, 5]
 my_list[::2] # From beginning to end, every other item; so output ---> [1, 3, 5]
 my_list.append(6) # so list is now ---> [1, 2, 3, 4, 5, 6]
 len(my_list) # Get the length of the list; so output ---> 7
 my_list   ['a', 'b', 'c'] # append muliplitple items; so list is now ---> [1, 2, 3, 4, 5, 6, 'a', 'b', 'c']
 starting over so my_list = [1, 2, 3]
 my_list[0] = 'a' # Replacing a single value, at index 0; so list is now ---> [a, 2, 3]
 my_list[1:] = ['b', 'c'] # Replacing a slice; so list is now ---> [a, b, c]
 my_list[:] = [1] # replace all of list's contents; so list is now ---> [1]
 starting over so my_list = [1, 2, 3]
 my_list.pop() # Removes the last item in the list; so list is now ---> [1 , 2]
 my_list.pop(0) # Given an argument removes item(s) at the given indexes; so list is now ---> [2]


 running a python file:
 $ python planets.py
 
 
 comment: I want to run notebooks and interact with data outside of the docker container. I add the -v option and this allows my to use docker this way.  I set the docker container to point to my local directory titled Code and created a folder within the docker container where it would be visible. My command string is: 
 $ docker run -i -t -p 8888:8888 -v /Users/rajivshah/Code/:/opt/Code continuumio/anaconda3 /bin/bash -c "/opt/conda/bin/conda install jupyter -y --quiet && /opt/conda/bin/jupyter notebook --notebook-dir=/opt --ip='*' --port=8888 --no-browser"  
 
 
 
 
store your images in docker cloud (forman):  all individual users can create one private repository for free and unlimited public repositories

	
from: https://news.ycombinator.com/item?id=8470206 :
"Actually, we have Nginx configured with Health Check (http://nginx.org/en/docs/http/load_balancing.html#nginx_load...). Hence, it will automatically take a given appserver out of the pool when it stops responding. Once the node is back again, Nginx will automatically bring it back into rotation.  Also, we actually use a volume/bind-mount to store the source code on the host machine (mounted read-only). That way we can roll out changes with `rsync` and just restart the container if needed.  The only time we need to pull a new update is if the dependencies/configuration of the actual container change. ... I will add that if you're using Docker (which we weren't) it might be easier to deploy a new set of Docker containers with updated code and just throw away the old ones."

"Docker, CoreOS, fleet, and etcd have completely changed how I build projects. It's made me much more productive.
I'm working on Strata, which is a building management & commissioning system for property owners of high-rise smart buildings. It's currently deployed in a single building in downtown Toronto, and it's pulling in data from thousands of devices, and presenting it in real-time via an API and a dashboard.  So in this building, I have a massive server. 2 CPUs, 10 cores each, 128GB of RAM, the works. It came with VMWare ESXi.  I have 10 instances of CoreOS running, each identical, but with multiple NICs provisioned for each so that they can communicate with the building subsystems. I built every "app" in its own Docker container. That means PostgreSQL, Redis, RabbitMQ, my Django app, my websocket server, even Nginx, all run in their own containers. They advertise themselves into etcd, and any dependencies are pulled from etcd. That means that the Django app gets the addresses for the PostgreSQL and Redis servers from etcd, and connects that way. If these values change, each container restarts itself as needed.  I also have a number of workers to crawl the network and pull in data. Deployment is just a matter of running 'fleetctl start overlord@{1..9}.service', and it's deployed across every machine in my cluster.  With this setup, adding machines, or adding containers is straightforward and flexible.  Furthermore, for development, I run the same Docker containers locally via Vagrant, building and pushing as needed. And when I applied for YC, I spun up 3 CoreOS instances on DigitalOcean and ran the fleet files there.  As I said, I've been able to streamline development and make it super agile with Docker & CoreOS. Oh, and I'm the only one working on this. I figure if I can do it on my own, imagine what a team of engineers can do.  Very powerful stuff. ... Take Postgres for example. I use the official Docker image, and run it via a fleet .service file, having it store its data in a Docker data-only container, also launched from a fleet unit file. Finally, there's a third unit file that runs a slightly customized version of the docker-register image from CoreOS's polvi. This Dockerized app polls the Docker socket every 10s or so to get the IP and port information for the Docker container it's monitoring, in this case Postgres. It PUTs this value into etcd.
Afterwards, when my Django containers start, they pull their configuration from etcd using python-etcd. It will get the IPs and ports for the services, including Postgres, from etcd, and configure itself using that. Finally, it will keep these values in a dictionary. If these values ever differ from what's currently in etcd, then my Django app will send a signal to cause uwsgi to gracefully restart the Django app, so that it can reload itself with the new configuration.
For other Dockerized apps like nginx, I use kelseyhightower's confd-watch to monitor the etcd keys I'm interested in. confd-watch is great because it will use golang's text/template package to allow me to generate configuration files, like nginx.conf, with the values from etcd. So as these values get updated, so will nginx as confd-watch will force a graceful reload.
Hope that helps."

"We use the Google Compute Engine container optimised VMs, which make deployment a breeze. Most of our docker containers are static, apart from our application containers (Node.js) that are automatically built from github commits. Declaring the processes that should run on a node via a manifest makes things really easy; servers hold no state, so they can be replaced fresh with every new deployment and it's impossible to end up with manual configuration, which means that there is never a risk of losing some critical server and not being able to replicate the environment."

"we never patch a running box, we just bring the new container up and remove the old one. Super easy, super reliable, and best of all, totally scriptable."



open web page for 9000 port local host from terminal:
open http://localhost:9000/





switching tabs in chrome:
CTRL + TAB = switch to tab on right
CTRL + SHIFT + TAB = switch to tab on left
