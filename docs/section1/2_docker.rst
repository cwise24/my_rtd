Docker
======

This `link <https://www.codementor.io/djangostars/tutorial-what-is-docker-and-how-to-use-it-with-python-wowos9qte>`_ is a very good high level cover of most basic Docker commands you will use.

One thing not covered, and is a bit more advanced, is creating your own Docker network versus using the ``docker0`` bridge assigned IP address. I will cover the steps to build a custom
Docker network and assign that network and an IP address to a container.

First you have to create the network and give it an alias name

.. code-block:: bash
   :caption: Docker network creation
   
    docker network create --subnet=172.10.1.0/24 mynet

To run a new container within this new network

.. code-block:: bash
   :caption: Docker run
   
   docker run  --net mynet --ip 172.10.1.3 -p 81:80 -h web1.local.com --name web1 --restart unless-stopped -dit nginx

Where ``-h`` sets the hostname and ``--name`` sets an alias name for the container

Some other useful docker commands

::

    docker rm -f <container_name>                                       { remove container; also forces removal of running container }
    docker rmi <container image>                                        { remove image }
    docker cp /local/file <container_name>:/path/file                   { copy local file to container; helpful if container has NO shell }
    docker start                                                        { start a stopped container }
    docker stop                                                         { stop a running container }
    docker restart                                                      { restart a running container }
    docker ps -a                                                        { show all containers; default is only running }

More of a Linux feature, but one of my favorite aliases to use for Docker to show the IP address using the container name

::

    alias dip='docker inspect -f '\''{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}'\'' '

Usage

::

    # dip mycontainer
    # 172.17.0.3