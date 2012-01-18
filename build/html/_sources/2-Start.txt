First Steps with CouchAPP
=========================

Here you will learn some details of the folder structure and the meaning of specific files inside your CouchApp directory. If you feel familiar enough with this, you may already proceed to the next topic, but bear in mind to at least generate an empty CouchApp before proceeding any further.

Start with CouchApp
-------------------

Create working directories and files
________________________________________

To set up a CouchApp, we start the actual CouchApp program to generate a directory structure where all of our code will be saved to at corresponding places. After the upload of our CouchApp into a CouchDB, most of these files and folders will form the design document of our application.

Move to any directory you like. Don't worry, a sub directory for our project will be created in our next step automatically. For this example I will go to my *./Documents* folder and start the ``couchapp`` program with it's appropriate arguments. You may use whatever directory you like.::

    $ cd Documents
    
    /Documents$ couchapp generate enotes
    
The ``couchapp`` command is launched with the ``generate`` option. This will command couchapp to generate a new CouchApp project and trigger the creation of all necessary directories. The ``enotes`` part describes the name of our application. Acutally, you may change it to your liking, but for reasons of compatibility to this tutorial I recommend you to stick with this proposition.

If we take a look at the newly generated file tree we see the following directories and files (there are also hidden files!): ::
    
    enotes$ ls
    
    ./_attachments
    ./lists
    ./shows
    ./updates
    ./vendor
    ./views
    .chouchappignore
    .couchapprc
    couchapp.json
    _id
    language
    README.md

We will go through each of this entries and explain what they are. If necessary we will also fill in certain values.

.couchappignore
_______________

This file defines files or folders that may be located inside our tree but should not be uploaded to the CouchDB. Regular Expressions (RegEx) are used to define these.
The ``.couchappignore`` default looks like this:

.. code-block:: js

    [
    // filenames matching these regexps will not be pushed to the database
    // uncomment to activate; separate entries with ","
    // ".*~$"
    // ".*\\.swp$"
    // ".*\\.bak$"
    ]
    
As we can see, all possible pre-defined values are switched off by the leading ``//``. If you ever produce objects that are not necessary for your CouchApp to work (like a SVN control directory), you should include a line for them here. We do not need to do this for our CouchApp tutorial.

.couchapprc
___________

The hidden ``.couchapprc`` file contains information of the CouchDB (or CouchDBs) our application will be integrated into in *JSON* format. The file looks like this when it is initially generated:

.. code-block:: js

    {}
    
We don't need to fill anything in here, but it could be very helpful when finally uploading the CouchApp to a CouchDB. It may look like this:

.. code-block:: js

    {
        "env" : {
            "testDB" : {
                "db" : "http://localhost:5984/test-database"
            },
            "finalDB" : {
                "db" : "http://username:password@anyserver.com/final-database"
            },
            "default" : {
                "db" : "http://localhost:5984/default-database"
            }
        }
    }

In the example above (which we *don't* enter in our file right now), there are three different databases specified by JSON notation in the ``env`` section. When uploading the CouchApp later on we don't have to specify the database connections but can simply use the terms ``testDB`` or ``finalDB``. If no database is named when uplading the CouchApp, the one defined by the value ``default`` is used.

So let's enter the following code to our ``.couchapprc`` file:

.. code-block:: js

    {
        "env" : {
            "default" : {
                "db" : "http://localhost:5984/enotes"
            }
        }
    }

By this, our default database named ``enotes`` is defined to be on our local machine (``localhost:5894``).
There are more options that can be included in this file which we will ignore for the time being.

couchapp.json
_____________

This file contains basic information about our project.

.. code-block :: js

    {
        "name": "Basic CouchApp",
        "description": "CouchApp with changes feed and form support."
    }

Change it to your liking.

_id
___

Most of the files we edit will form the design document of our CouchApp. Every document in CouchApp needs an ``_id`` field. Design documents are identified by an id with the value ``_design/`` followed by it's name. Hereby, this is also the default value for this file: ::

    _design/enotes

language
________

This defines the pragramming language which is used for this project. Since we will only resort to ``javascript``, we leave the default entry as it is: ::

    javascript

README.md
_________

This is some information about what CouchApp is and how to access it.

./_attachments
______________

Here, files that define the layout, interface and basic functionality of our application are stored. Files ald folders located within its directory are included as attachments to the design document. I want to draw your attention to the file ``index.html``. It will contain our interface and is the file that is called when starting our CouchApp.

./lists   ./shows   ./updates   
_____________________________

These folders are not used by our CouchApp.


./vendor
________

Files stored in this directory are also attached to the CouchDB design document. Usually, libraries from sources other than our manually created ones are put here, but also functions connecting the CouchApp programming interface to other libraries are located here.

./views
_______

All data views that are necessary are stored in this directory. Each sub folder describes one view and contains at least a ``map.js`` file, but most of the time also a ``reduce.js``. (the latter one is not yet present in our example)
As you might have guessed, these two files correspond to the *map* and *reduce* functions used by CouchDb.

Upload the CouchApp
-------------------

Without any further canges, we could upload this example CouchApp to our CouchDB. For this, we would use the couchapp program again. ::

    enotes$ couchapp push enotes
    
This command uses the option ``push`` to initiate the upload to the database named ``enotes``. Note that this database will be created automatically.

If you remember, we have defined a database connection in the ``.couchapprc`` file. So we could also just enter ::

    enotes$ couchapp push
    
Review the _design document
---------------------------

To gain a better understanding of the principles behind CouchApp, let's take a look at the database created by our upload.
To do so, we enter the CouchDB gui via a web browser: ::

    http://localhost:5984/_utils
    
You should see a list of all your CouchDB databases of which one should be named *"enotes"*.

    .. image:: images/2_firstdb.png

Klick on it and you will see all documents that are currently stored inside this database (which contains our enotes application).

    .. image:: images/2_documents.jpg
    
Don't be surprised, there is only one document present. This is the design document. When adding content later on, it will also be stored as additional documents at this very location. Open the design document.
You should be presented with a view similar to this:

    .. image:: images/2_design.png
        :width: 886 px

Take a look at the ``Field`` column. Most of these entries represent files or folders from our CouchApp directory tree. The files have been converted to entries in the design document of our CouchApp.
Especially interesting is the ``_attachments`` entry. These are attached files that can be accessed directly by their URL (but are stored inside the CouchDB). The other fields are using JSON for formatting and storing information (functions, tags, ...)

Test the CouchApp
-----------------

To start our CouchApp, we simply have to access the CouchDB. As we know, this is done via a simple HTTP command.::

    http://localhost:5984/enotes/_design/enotes/index.html
    
This is exaclty the same link as the one accessed when clicking on ``index.html`` in the ``_attachments`` entry in our design document.

    .. image:: images/2_index.jpg
    
Let's take a closer look at this url. It is quite clear that ``http://localhost:5984`` points to the CouchDB server on our local machine. The following ``/enotes`` describes the database where our CouchApp is stored. Appending ``/_design/enotes`` tells the server to open the design document with the Id *_design/enotes*. The final string ``/index.html`` accesses the appended *index.html* and therefore starts our CouchApp.
