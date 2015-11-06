.. _post-create-a-cluster-v2:

Create a cluster
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

.. code::

    POST /v2/{tenant_id}/clusters

This operation creates a new cluster.

.. note::
   You must create a stack before you create a cluster.
   
   

The ``postInitScript`` request parameter specifies a URL that downloads a script that runs 
after the cluster is created. The status of the run is shown in the `
`postInitScriptStatus``  response parameter.

The ``postInitScriptStatus`` variable can have any of the following values: 
``FAILED``, ``PENDING``, ``DELIVERED``, ``RUNNING``, ``SUCCEEDED``, and ``None``.

The ``progress`` response parameter is calculated based on the number of nodes in the cluster 
and their progress through configuration. Currently, the calculation is as follows but is subject to 
change:

-  BUILDING: progress = 0.5 * configuring_count /len(self.nodes)
-  CONFIGURING/RESIZING: progress = 0.5 + 0.5 * active_count / len(self.nodes))
-  ACTIVE: progress = 1.0


The 400 error code might indicate any of the following issues:

-  The response body is invalid.
-  You need to create a stack.
-  The node count is invalid.
-  The flavor is invalid.
-  The data is malformed.


The 413 error code might indicate that the resource limit is exceeded.

This table shows the possible response codes for this operation:

+--------------------------+-------------------------+-------------------------+
|Response Code             |Name                     |Description              |
+==========================+=========================+=========================+
|200                       |OK                       |Success.                 |
+--------------------------+-------------------------+-------------------------+
|400                       |Bad Request              |The user-provided        |
|                          |                         |request contained an     |
|                          |                         |error.                   |
+--------------------------+-------------------------+-------------------------+
|413                       |Request Entity Too Large |The resource quota was   |
|                          |                         |exceeded.                |
+--------------------------+-------------------------+-------------------------+


Request
""""""""""""""""

This table shows the URI parameters for the request:

+--------------------------+-------------------------+-------------------------+
|Name                      |Type                     |Description              |
+==========================+=========================+=========================+
|{tenant_id}               |String                   |The tenant ID in a multi-|
|                          |                         |tenancy cloud.           |
+--------------------------+-------------------------+-------------------------+

This table shows the body parameters for the request:

+---------------------------------+----------------------+---------------------+
|Name                             |Type                  |Description          |
+=================================+======================+=====================+
|cluster.\ **name**               |*(Required)*          |Name for the cluster.|
+---------------------------------+----------------------+---------------------+
|cluster.\ **username**           |*(Required)*          |Cluster SSH login    |
|                                 |                      |username.            |
+---------------------------------+----------------------+---------------------+
|cluster.\ **ssh_keys**           |*(Required)*          |List of ssh          |
|                                 |                      |credential key names.|
+---------------------------------+----------------------+---------------------+
|cluster.\ **stack_id**           |*(Required)*          |Stack ID to use for  |
|                                 |                      |the cluster.         |
+---------------------------------+----------------------+---------------------+
|cluster.\ **node_groups**        |*(Optional)*          |List of node groups  |
|                                 |                      |where various        |
|                                 |                      |components reside.   |
|                                 |                      |Note: This overrides |
|                                 |                      |any values already   |
|                                 |                      |defined as part of   |
|                                 |                      |the stack's node     |
|                                 |                      |groups.              |
+---------------------------------+----------------------+---------------------+
|cluster.node_groups.\ **id**     |*(Required)*          |A node group ID or   |
|                                 |                      |name.                |
+---------------------------------+----------------------+---------------------+
|cluster.node_groups.\ **count**  |*(Optional)*          |Number of instances  |
|                                 |                      |of the node group.   |
+---------------------------------+----------------------+---------------------+
|cluster.node_groups.\            |*(Optional)*          |Flavor ID for the    |
|**flavor_id**                    |                      |node group.          |
+---------------------------------+----------------------+---------------------+
|cluster.\ **connectors**         |*(Optional)*          |List of connector    |
|                                 |                      |credentials. Note:   |
|                                 |                      |This also enables    |
|                                 |                      |the connector for    |
|                                 |                      |use with the cluster.|
+---------------------------------+----------------------+---------------------+
|cluster.connectors.\ **type**    |*(Required)*          |Type of connector.   |
|                                 |                      |Ex: cloud_files      |
+---------------------------------+----------------------+---------------------+
|cluster.connectors.credential.\  |*(Required)*          |Name of credential   |
|**name**                         |                      |for specific type of |
|                                 |                      |connector.           |
+---------------------------------+----------------------+---------------------+
|cluster.\ **scripts**            |*(Optional)*          |List of scripts to   |
|                                 |                      |run post install of  |
|                                 |                      |cluster.             |
+---------------------------------+----------------------+---------------------+
|cluster.scripts.\ **id**         |*(Required)*          |A script ID.         |
+---------------------------------+----------------------+---------------------+


**Example Create a cluster: JSON request**


.. code::

   {
       "cluster": {
           "name": "test",
           "username": "cbduser",
           "ssh_keys": ["cbdkey"],
           "stack_id": "HDP2.1_Hadoop",
           "node_groups": [
               {
                   "count": 10,
                   "flavor_id": "hadoop1-7",
                   "id": "slave"
               }
           ],
           "connectors": [
               {
                   "type": "cloud_files",
                   "credential": {
                       "name": "cfuser"
                   }
               }
           ],
           "scripts": [
               {
                   "id": "c5033423-97ff-4215-9552-19d425e1f9dd"
               }
           ]
       }
   }
   





Response
""""""""""""""""

**Example Create a cluster: JSON response**


.. code::

   {
       "cluster": {
           "created": "2014-06-14T10:10:10Z",
           "id": "aaa-bbbb-cccc",
           "name": "test",
           "username": "cbduser",
           "ssh_keys": ["cbdkey"],
           "status": "BUILDING",
           "progress": "5",
           "links": [
               {
                   "href": "https://dfw.bigdata.api.rackspacecloud.com/v2/1234/clusters/aaa-bbbb-cccc",
                   "rel": "self"
               },
               {
                   "href": "https://dfw.bigdata.api.rackspacecloud.com/1234/clusters/aaa-bbbb-cccc",
                   "rel": "bookmark"
               }
           ],
           "stack_id": "HDP2.1_Hadoop",
           "node_groups": [
               {
                   "components": [
                       {
                           "name": "Namenode"
                       },
                       {
                           "name": "ResourceManager"
                       },
                       {
                           "name": "YarnTimelineServer"
                       },
                       {
                           "name": "JobHistoryServer"
                       }
                   ],
                   "count": 1,
                   "flavor_id": "hadoop1-7",
                   "id": "master"
               },
               {
                   "components": [
                       {
                           "name": "Namenode"
                       }
                   ],
                   "count": 1,
                   "flavor_id": "hadoop1-7",
                   "id": "standby-namenode"
               },
               {
                   "components": [
                       {
                           "name": "JournalNode"
                       }
                   ],
                   "count": 3,
                   "flavor_id": "hadoop1-1",
                   "id": "journalnodes"
               },
               {
                   "components": [
                       {
                           "name": "Datanode"
                       },
                       {
                           "name": "NodeManager"
                       }
                   ],
                   "count": 10,
                   "flavor_id": "hadoop1-7",
                   "id": "slave",
               },
               {
                   "components": [
                       {
                           "name": "HiveServer2"
                       },
                       {
                           "name": "HiveMetastore"
                       },
                       {
                           "name": "HiveClient"
                       },
                       {
                           "name": "HiveAPI"
                       },
                       {
                           "name": "PigClient"
                       }
                   ],
                   "count": 1,
                   "flavor_id": "hadoop1-2",
                   "id": "gateway"
               }
           ],
           "updated": "",
           "connectors": [
               {
                   "type": "cloud_files",
                   "credential": {
                       "name": "cfuser"
                   }
               }
           ],
           "scripts": [
               {
                   "id": "c5033423-97ff-4215-9552-19d425e1f9dd",
                   "name": "Mongo Connector",
                   "status": "PENDING"
               }
           ]
       }
   }
   




