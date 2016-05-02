.. _update-an-image:

Update an image
---------------

.. code::

    PATCH /images/{image_id}

This operation updates the specified image. You can update an image that you own. 

You can use the HTTP PATCH method to update certain standard properties, and to add, update, or remove custom, user-defined image properties. For more information, see HTTP PATCH method. 

Request parameters
~~~~~~~~~~~~~~~~~~

The following table shows the URI parameter for the request.

.. list-table:: 
   :widths: 15 10 30
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - {image_id}
     - UUID
     - Image ID stored through the Images API, typically a UUID.

The following table shows the body parameters for the request.

.. list-table:: 
   :widths: 15 10 30
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - **op**
     - String
     - *(Required)* The operation to be executed: ``add``, ``remove``, or ``replace``. For more information about updating image properties, see `Update image properties`_.
   * - **path**
     - String
     - *(Required)* The location within the image where the operation is to be performed.
   * - **value**
     - String
     - The actual value to be added or replaced. This parameter is not required for the ``remove`` operation.

The request body must conform to the ``application/openstack-images-v2.1-json-patch`` media type.

Update image properties
~~~~~~~~~~~~~~~~~~~~~~~

Following are some guidelines for updating custom, user-defined properties.

Add properties
^^^^^^^^^^^^^^

You can add custom properties to your image. We recommend that you name a custom property by prefixing your domain or name. You may not use ``com.rackspace`` as the prefix. For example, ``com.mycompany.myproperty`` and ``myname.myproperty`` are valid, but ``com.rackspace.myproperty`` is not allowed.

Do not use the prefix ``org.openstack`` because OpenStack might add a property with the same name.

Remove properties
^^^^^^^^^^^^^^^^^

You can delete any custom property that you previously added to your image.

Replace properties
^^^^^^^^^^^^^^^^^^

You can update any custom properties that you previously added to an image that you own, and you can update the following standard properties:

- ``name``
- ``tags``
- ``os_distro``
- ``os_version``
- ``protected``
- ``container_format`` (Changing this value might render your image unusable.)
- ``disk_format`` (Changing this value might render your image unusable.)
- ``min_disk`` (Changing this value affects what flavors you can use with the image.)
- ``min_ram`` (Changing this value affects what flavors you can use with the image.)
- ``ramdisk_id`` (Applies only when ``disk_format`` has a value of ``ami``.)
- ``kernel_id`` (Applies only when ``disk_format`` has a value of ``ami``.)

In general, you can update any properties that you own, but do not expect to be able to update anyone else's properties. For example, you can't update any properties starting with ``com.rackspace``, and you might not be able to update some properties starting with ``org.openstack``.

Request example
~~~~~~~~~~~~~~~
The following example updates two properties for the image: ``name`` and ``tags``. 

.. code::

(?cyr: Include actual HTTP request part of this operations?)

    [
        {"op": "replace", "path": "/name", "value": "Fedora 17"},
        {"op": "replace", "path": "/tags", "value": ["fedora", "beefy"]}
    ]

Response parameters
~~~~~~~~~~~~~~~~~~~

The response conforms to the schema found in Get image schema.

The following table shows the body parameters for the response.

(?cyr: It looks like these are not all in alphabetical order. Should they be? Would probably be good to show an example with nested parameters, too?)

.. list-table:: 
   :widths: 15 10 30
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - **id**
     - String
     - The UUID of the image.
   * - **name**
     - String
     - The name of the image.
   * - **status**
     - String
     - The status of the image. For possible image statuses, see Image statuses.
   * - **visibility**
     - String
     - Specifies image visibility as ``public``, ``private``, or ``shared``.
   * - **checksum**
     - String
     - The checksum of the image.
   * - **minRam**
     - String
     - The minimum server RAM required for this image.
   * - **minDisk**
     - String
     - The minimum server disk size required for this image.
   * - **tags[ ]**
     - Array
     - An array of user-defined image tags.
   * - **created**
     - String
     - The date and time that the image was created.
   * - **updated**
     - String
     - The date and time that the image was updated.
   * - **schema**
     - String
     - The schema of the image.

Response example
~~~~~~~~~~~~~~~~

The following example shows the JSON response for retrieving the backup for a project.

.. code::

(?cyr: Include HTTP output, which is usually the response code?)

    {
      "id":"e7db3b45-8db7-47ad-8109-3fb55c2c24fd",
      "name":"Fedora 17",
      "status":"queued",
      "visibility":"public",
      "tags": ["fedora", "beefy"],
      "created_at":"2012-08-11T17:15:52Z",
      "updated_at":"2012-08-11T17:15:52Z",
      "self":"/v2/images/e7db3b45-8db7-47ad-8109-3fb55c2c24fd",
      "file":"/v2/images/e7db3b45-8db7-47ad-8109-3fb55c2c24fd/file",
      "schema":"/v2/schemas/image"
    }

Response codes
~~~~~~~~~~~~~~

The following table shows the possible response codes for this operation.

.. list-table:: 
   :widths: 15 10 30
   :header-rows: 1

   * - Code
     - Name
     - Description
   * - 200
     - Success
     - The request succeeded.
   * - 400 
     - Error
     - A general error has occurred. 
   * - 401
     - Unauthorized
     - The request has not been applied because it lacks valid authentication credentials for the target resource. The credentials are either expired or invalid.
   * - 403
     - Forbidden
     - The server understood the request but is not authorizing it.
   * - 405
     - Method Not Allowed
     - The method received in the request line is known by the origin server but is not supported by the target resource.
   * - 413
     - Over Limit
     - The number of items returned is above the allowed limit.
   * - 415
     - Bad Media Type
     - This error might result if the wrong media type is used in the cURL request.
   * - 500
     - API Fault
     - The server encountered an unexpected condition that prevented it from fulfilling the request.
   * - 503
     - Service Unavailable
     - The server is currently unable to handle the request because of a temporary overload or scheduled maintenance, which will likely be alleviated after some delay.
