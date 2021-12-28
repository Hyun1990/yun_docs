Install
=============

使用官方版本自动安装
--------------------

	``$ curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun``


**测试是否安装成功**


	``$ sudo docker run hello-world``

	打印出以下信息则安装成功

	::

		Unable to find image 'hello-world:latest' locally
		latest: Pulling from library/hello-world
		0e03bdcc26d7: Pull complete 
		Digest: sha256:4cf9c47f86df71d48364001ede3a4fcd85ae80ce02ebad74156906caff5378bc
		Status: Downloaded newer image for hello-world:latest

		Hello from Docker!
		This message shows that your installation appears to be working correctly.

		To generate this message, Docker took the following steps:
		 1. The Docker client contacted the Docker daemon.
		 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
			(amd64)
		 3. The Docker daemon created a new container from that image which runs the
			executable that produces the output you are currently reading.
		 4. The Docker daemon streamed that output to the Docker client, which sent it
			to your terminal.

		To try something more ambitious, you can run an Ubuntu container with:
		 $ docker run -it ubuntu bash

		Share images, automate workflows, and more with a free Docker ID:
		 https://hub.docker.com/

		For more examples and ideas, visit:
		 https://docs.docker.com/get-started/

Docker容器
-----------

	* 查看所有容器

		``$ sudo docker ps -a``

	* 启动一个已经停止的容器

		``$ sudo docker start <CONTAINER ID>``

	* 停止一个容器

		``$ sudo docker stop <CONTAINER ID>``

	* 停止的容器可以通过 docker restart 重启

		``$ sudo docker restart <CONTAINER ID>``

	* 退出容器终端

		``$ exit``

	* 进入容器

		**attach** 命令

		``$ sudo docker attach <CONTAINER ID>``
		
		**exec** 命令

		``$ sudo docker exec -it <CONTAINER ID> /bin/bash``
	
	.. note::
		attach 从容器退出会导致容器停止，推荐使用exec命令，退出容器终端，不会导致容器的停止。

	
	* 删除容器(容器必须是停止的)

		``$ sudo docker rm -f <CONTAINER ID>``
	

Docker 镜像
------------

	* 显示本地镜像

		``$ sudo docker images``

	* 查找镜像
	
		``$ sudo docker search hello-world``

	* 获取一个新的镜像
	
		``$ sudo docker pull ubuntu:13.10``


	* 删除镜像

		``$ sudo docker rmi hello-world``


参考文档
--------

	`Docker 教程 <https://www.runoob.com/docker/docker-tutorial.html>`_

