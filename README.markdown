Introduction
============
poodledo is a Python library for working with the web-based task management software [Toodledo](http://www.toodledo.com).

This particular version is an amalgam of two previous versions ([Felix Riedel's](http://code.google.com/p/poodledo/) and [Martin Treusch von Buttlar's](https://github.com/martint17r/poodledo)), substantially rewritten to use the [Toodledo API v2.0](http://api.toodledo.com/2/index.php).

Requirements
============
- Python 2.5+ (or, an older version with the ElementTree module installed)

Usage
=====
poodledo handles almost all of the API calls described in the [Toodledo docs](http://api.toodledo.com/2/index.php).

Initialization
--------------

Create an `ApiClient` object and call `authenticate()` to get a session key. This key is re-used in subsequent API calls until it expires.

    from apiclient import ApiClient
    c = ApiClient()
    c.authenticate('username@email.dom', 'password')

API Structure
-------------
Toodledo's API is broken down into six basic object types:

- Folders
- Contexts
- Goals
- Locations
- Notebooks
- Tasks

along with several other miscellaneous methods (`getAccountInfo`, `getToken`, etc.).

Each object type supports the same basic operations: "add", "delete", "edit", and "get". I'll use Folders for these examples, but the other object types have equivalent methods with the appropriate names (i.e., `getContexts`, `editLocation`, `deleteGoal`, etc.).

Retrieving
----------
Call `getFolders` to retrieve a list of folder objects from the API. This list is cached by the ApiClient until a change is made with another API call.

    c.getFolders()

Call `getFolder` to retrieve a specific folder object. This method takes one argument, which can be either the object's ID or its name/title.

    c.getFolder('My name is')
    c.getFolder(3472958)

Adding
------
Call `addFolder` to add a new folder with the API.

    c.addFolder('New Folder Name')

You can also specify object parameters as keyword arguments.

    c.addFolder('Private, Archived Folder', archived=True, private=True)

Deleting
--------
Call `deleteFolder` to delete a folder with the API. This method uses `getFolder` to identify its argument, so you can specify either the folder's name or its ID.

    c.deleteFolder('Doomed Folder')

Editing
-------
Call `editFolder` to change a folder's characteristics. Use the same arguments as `getFolder` to identify the target, and pass the changes as keyword arguments.

    c.editFolder('Current Folder Name', name='New Folder Name', private=True)

Notes
=====
- In order to use this library, you will need to [get your own API token](http://api.toodledo.com/2/account/doc_register.php). I have not included mine in the code. Add it to `ApiClient.__init__`.
- The test harness `test_poodledo.py` does not function at all currently.
- Pull requests always welcome!

TODO
====
- Update the test harness `test_toodledo.py` for API 2.0
- Implement a generic account/key storage system
- Write several sample scripts for reference
- Add batch processing for notes and tasks
- Write a "pythonic" wrapper that makes the returned objects smart (e.g., doing `task_object.name = "New Name"` would actually update the task's name with the API)
- Make objects which have an ordering (folders and subtasks) a) honor that ordering, and b) be re-orderable

License
=======
poodledo is released under a **BSD License**. See the LICENSE file for details.

Contact
=======
You can email me at comptona@gmail.com.

To report bugs or request features, please use the **[Issues](https://github.com/handyman5/poodledo/issues)** feature.
