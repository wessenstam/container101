.. title:: Demo of Containerisation WordPress in k8s


----------------------------------------
Demo - Deploying Wordpress in Kubernetes
----------------------------------------

This tutorial shows you how to deploy a WordPress site and a MySQL database using Karbon. Both applications use **PersistentVolumes** and **PersistentVolumeClaims** to store data with Nutanix Volumes.

Before you can run the demo move to the right folder.

.. code-block:: bash
  :name: inline-code-example

  cd ~/containers101/demo2-deploying_wordpress


Create a Secret for MySQL Password
++++++++++++++++++++++++++++++++++

Create the Secret object from the following command. You will need to replace **YOUR_PASSWORD** with the password you want to use.

.. code-block:: bash
  :name: inline-code-example

  kubectl create secret generic mysql-pass --from-literal=password=YOUR_PASSWORD


Verify that the Secret exists by running the following command:

.. code-block:: bash
  :name: inline-code-example

  kubectl get secrets


The response should be like this:

.. code-block:: bash
  :name: inline-code-example

  NAME                  TYPE                    DATA      AGE
  mysql-pass            Opaque                  1         42s


Deploy MySQL
++++++++++++

The following manifest describes a single-instance MySQL Deployment. The MySQL container mounts the **PersistentVolume** at **/var/lib/mysql**. The **MYSQL_ROOT_PASSWORD** environment variable sets the database password from the Secret.

Deploy MySQL using the :download:`mysql-deployment.yaml <mysql-deployment.yaml>` file:

.. code-block:: bash
  :name: inline-code-example

  kubectl apply -f mysql-deployment.yaml


Verify that a **PersistentVolume** got dynamically provisioned. 

.. code-block:: bash
  :name: inline-code-example

  kubectl get pvc

.. note::
  It can take up to a few minutes for the PVs to be provisioned and bound.

The response should be like this:

.. code-block:: bash
  :name: inline-code-example

  NAME           STATUS  VOLUME                                   CAPACITY ACCESS MODES  STORAGECLASS  AGE
  mysql-pv-claim Bound   pvc-91e44fbf-d477-11e7-ac6a-42010a800002 20Gi     RWO           standard      29s


Verify that the Pod is running by running the following command:

.. code-block:: bash
  :name: inline-code-example

  kubectl get pods

.. note::
  Wait until the status is **RUNNING** before you move to the next section.

Deploy WordPress
++++++++++++++++

The following manifest describes a single-instance WordPress Deployment and Service. It uses many of the same features like a PVC for persistent storage and a Secret for the password. But it also uses a different setting: **type: NodePort**. This setting exposes WordPress to traffic from outside of the cluster.

Create a WordPress Service and Deployment from the :download:`wordpress-deployment.yaml <wordpress-deployment.yaml>` file:

.. code-block:: bash
  :name: inline-code-example

  kubectl apply -f wordpress-deployment.yaml


Verify that a PersistentVolume got dynamically provisioned:

.. code-block:: bash
  :name: inline-code-example

  kubectl get pvc


The response should be like this:

.. code-block:: bash
  :name: inline-code-example

  NAME         STATUS  VOLUME                                     CAPACITY ACCESS MODES  STORAGECLASS  AGE
  wp-pv-claim  Bound   pvc-e69d834d-d477-11e7-ac6a-42010a800002   20Gi     RWO           standard      7s


Verify that the Service is running by running the following command:

.. code-block:: bash
  :name: inline-code-example

  kubectl get services wordpress


The response should be like this:

.. code-block:: bash
  :name: inline-code-example

  NAME        TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)        AGE
  wordpress   ClusterIP   10.0.0.89    <pending>     80:32406/TCP   4m


Copy the IP address of one of your workers, and load the page in your browser to view your site with the port provided on the previous command.