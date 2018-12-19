.. title:: Introduction to containers on Kubernetes

.. _lab2:

Lab2 - Container Operations
---------------------------

The objective of this lab is to show you what are the common tasks to operate Docker containers.

* List running containers
* Inspect
* Logs
* Stop
* Start
* Exec
* Create/Run
* Remove


List running containers
+++++++++++++++++++++++

Let's list the running containers.

.. code-block:: bash
  :name: inline-code-example

  docker container ps


You should get an output as follow:

.. code-block:: bash
  :name: inline-code-example

  CONTAINER ID   IMAGE                     COMMAND                  CREATED           STATUS         PORTS                  NAMES
  49ccc8d9d52a   reg.ntnxdemo.local/l...   "nginx -g 'daemon of…"   52 seconds ago    Up 51seconds   0.0.0.0:8080->80/tcp   myfirstapp


If you want to check if you have stopped containers, you can run the following command:

.. code-block:: bash
  :name: inline-code-example

  docker container ps -a


Inspect
+++++++

With this command you get the metadata information for the running container. You will need the **container name** or **container ID** from the previous task.

.. code-block:: bash
  :name: inline-code-example

  docker container inspect <container_id or container_name>


Logs
++++

With this command you can check the application logs running in the container. Run first the following command for the existing running container. Leave it running with the logs opened, go back to the browser where you have the website opened. If you closed the window, open a new one with the IP of your virtual machine and port **80808*

.. code-block:: bash
  :name: inline-code-example

  docker container logs -f <container_id or container_name>


Press **ENTER** few times to leave some space. Now you can go to the browser and refresh your website few times. Come back to the logs and check you got new log output.

To close the logs press **CTRL+C**.

Stop
++++

Let's stop the container. When you stop a container this doesn't get removed unless during running the command you specified **--rm** (remove).

.. code-block:: bash
  :name: inline-code-example

  docker container stop <container_id or container_name>


To check if the container has been stopped and not removed, run the following command:

.. code-block:: bash
  :name: inline-code-example

  docker container ps -a

You should get an output as follow:

.. code-block:: bash
  :name: inline-code-example

  CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS                     PORTS   NAMES
  4dffd2202f93   reg.ntnxdemo.local/l..   "nginx -g 'daemon of…"   9 seconds ago   Exited (0) 5 seconds ago           myfirstapp


Before move to the next task check the web application is not working if you refresh your web browser.

Start
+++++

Let's start the stopped container.

.. code-block:: bash
  :name: inline-code-example

  docker container start <container_id or container_name>


Check the website is back and running again.

Exec
++++

Let's login into the container to run some troubleshooting.

The **-it** flag is to access on interactive mode (-i) and allocate a TTY (-t)

.. code-block:: bash
  :name: inline-code-example

  docker container exec -it <container_id or container_name> bash


If you gained access to the container you should see the prompt has changed to username@container_id, i.e.: **root@4dffd2202f93:/#**

To logout from the container just type **exit**

Create/Run
++++++++++

Let's create a container with the same characteristics but different host port, **8081**.

.. code-block:: bash
  :name: inline-code-example

  docker container run -d --name myfirstapp2 -p 8081:80 <container_image>


For example:

.. code-block:: bash
  :name: inline-code-example

  docker run -d --name myfirstapp2 -p 8081:80 jose.gomez/myfirstapp


List the running containers to check you have two containers running now.

.. code-block:: bash
  :name: inline-code-example

  docker container ps


Go back to the web browser, open a new window and access to the virtual machine IP address port **8081**.

Remove
++++++

Let's remove both running containers.

.. code-block:: bash
  :name: inline-code-example

  docker container rm $(docker ps -q) -f

.. note:: Lab finished