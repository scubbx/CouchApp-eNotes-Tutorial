Write the Interface
===================


As said previously, the interface layout is defined by HTML and CSS. This HTML and CSS are stored as attachments in the design document of our application. When we want to execute our CouchApp, we simply call the HTML. We can access attachments just like normal web-pages via its URL.

The following graphic gives an overview of the files we have to download and add to our CouchApp:

.. graphviz::

    digraph interface {
        node [fontname=Verdana,fontsize=10]
        node [style=filled]
        node [fillcolor="#EEEEEE"]
        node [color="#EEEEEE"]
        edge [color="#31CEF0"]
        
        "enotes" [shape=box, color=grey]
        "vendor"  [color="grey"]
        "couchapp"  [color="grey"]
        "_attachments"  [color="grey"]
        "_attachments "  [color="grey"]
        "style"  [color="grey"]
        
        "enotes" -> "vendor"
        "vendor" -> "couchapp"
        "couchapp" -> "_attachments"
        "_attachments" -> "jquery-1.6.2.min.js"
        "_attachments" -> "jquery-ui-1.8.16.custom.min.js"
        
        "enotes" -> "_attachments "
        "_attachments " -> "style"
        "style" -> "jquery-ui-1.8.16.custom.css"
        "style" -> "image"
        "image" -> "*.png"
        
        "style" -> "jquery.mobile-1.0.css"
    }
    
Elements not encircled by a gray line are files and folders we will create in the next sections.

So, let's just open the file ``index.html`` in the folder ``./_attachments`` for editing.

It is already filled with code which we will remove completely. So we will be left with an empty file. Now it's time to start filling in our own code!

HTML Structure & <head>
-----------------------

First, we enter a basic HTML structure. In the ``<head>`` section, we will fill in **meta-information** and the page title. Additionally we enter references to **two css stylesheets** we will download afterwards:

.. code-block:: html

    <!DOCTYPE html>
    <html>
        <head>
            <meta charset="utf-8" />
            <meta name="viewport" content="width=device-width,minimum-scale=1,maximum-scale=1">
            <title>eNotes</title>
            
            <link rel="stylesheet" href="style/jquery.mobile-1.0.css" type="text/css"/>
            <link rel="stylesheet" href="style/jquery-ui-1.8.16.custom.css" type="text/css"/>
        </head>
        <body>
        
        </body>
    </html>
    
The "meta-information" consists of the ``charset`` used to encode the html and the definition of the ``viewport``. The viewport variable is important for mobile browsers so that they can resize our layout accordingly.

CSS
---

We have to download the two css stylesheet files we just referenced to and move them to their appropriate location in our CouchApp directory tree.

jQuery UI CSS
_____________

* You can get the **jQuery UI CSS** at http://jqueryui.com/download .
* Open the downloaded archive and go to ``/css``. Depending on the version you downloaded, you will see a file like ``jquery-ui-1.8.16.custom.css`` and the directory ``/image`` a sub-folder further. Extract both of them to the ``/enotes/_attachments/style/`` folder.
* In the archive at ``/js/`` you will find two *.js* files. These need to be extracted to ``/enotes/vendor/couchapp/_attachments/``.

jQuery Mobile CSS
_________________

Download the file http://code.jquery.com/mobile/1.0/jquery.mobile-1.0.css to ``/enotes/_attachments/style/``.

JavaScript Library
--------------------

While we are at it, we may already download **jQuery mobile**. You can find it at http://code.jquery.com/mobile/1.0/jquery.mobile-1.0.min.js .
Place it at ``/enotes/vendor/couchapp/_attachments/``.



HTML <body>
-----------

Now, we will add some actually visible content to our ``index.html``. We will use jQuery Mobile and its specialized functions a lot because this CouchApp should be usable with mobile devices.

Pages
_____

To speed up loading times when switching between different "windows" of our CouchApp, we will define *subpages* within our HTML document. So, the complete application layout is already loaded when ``index.html`` is called.
*Subpages* are a speciality of jQuery Mobile and are defined by adding the attribute ``data-role="page"`` to a div-element. These pages can be linked to by a ``href = #idOfThePage``. For more information on pages, take a look at http://jquerymobile.com/test/docs/pages/page-anatomy.html.

"Tags" Window
_____________

Let's add our first application window - a view that will list all tags applied to any posts. This is done by defining a *page* inside the ``<body>`` of our html file (we will use comments to make the start and end of a page more visual):

.. code-block:: html
    
    ...
    ...
    <body>
        <!-- ====== tagListPage =====  -->
        <div data-role="page" data-theme="b" id="tagListPage">
            
        </div>
        <!-- tagListPage --> 
    </body>
    ...
    ...
    
Let's take a look at the entry that defines our page ``<div data-role="page" data-theme="b" id="tagListPage">``. As previously said, ``data-role="page"`` defines a new page. By specifying ``data-theme="b"`` we select the jQuery theme named *"b"* for our page. This mainly defines the colour scheme (for more information on this tag, take a look at http://jquerymobile.com/demos/1.0/docs/pages/pages-themes.html. At last, ``id="tagListPage"`` gives our page a name.

Now, we need to add some content to the "tagListPage". Actually, we could just enter some html, but jQuery mobile gives us the possibility to define a *header*, the *content* and a *footer*. This is done again by a ``<div>`` element with e.g. the attribute ``data-role="header"``. We should add these three sections to our newly created page:

.. code-block:: html

    ...
    ...
    <!-- ====== tagListPage =====  -->
    <div data-role="page" data-theme="b"id="tagListPage">
    
        <div data-role="header" data-position="fixed">
            
        </div>
        <div data-role="content" id="tagListContent" >
            
        </div>
        <div data-role="footer" id="tagListFooter" data-position="fixed">
            
        </div>
        
    </div>
    <!-- tagListPage --> 
    ...
    ...

For the time being, we just need to enter actual content to the ``header`` and ``footer``. Data displayed in the ``content`` section will be generated later programmatically.

For the data-role **header**, fill in:

.. code-block:: html

    <div data-role="controlgroup" data-type="horizontal">
        <a>0.1.5 select a tag</a>
        <a href="#addPage" data-transition="slideup" data-role="button"  >Add Item</a> 
    </div> 
    
..
    <div data-role="controlgroup" data-type="horizontal">
        <a>0.1.5 select a tag</a>
        <a href="#addPage" data-transition="slideup" data-role="button"  >Add Item</a> 
        <a href="#titleSearchPage" data-icon="grid" data-role="button">Search for Word in Title</a> 
        <a href="#textSearchPage" data-icon="grid" data-role="button">Search for Word in Text</a> 
    </div> 
    
The data-role **footer** has to be filled with:

.. code-block:: html

    <div data-role="controlgroup" data-type="horizontal">
        <a href="#addPage" data-transition="slideup" data-role="button" >Add Item</a> 
        <a href="#tagListPage" data-transition="slideup" data-role="button" >TagList</a>  
    </div>
    
..
    <div data-role="controlgroup" data-type="horizontal">
    <a href="#addPage" data-transition="slideup" data-role="button" >Add Item</a> 
    <a href="#tagListPage" data-transition="slideup" data-role="button" >TagList</a>  
    <a op="startReplication" href="#tagListPage" data-transition="slideup"  data-role="button">startReplication</a>  
    </div>
    
If we would push our CouchApp directory tree to our CouchDB right now with the command ``enotes$ couchapp push enotes``, we wold generate a CouchApp with the changes we have made so far.
Then, when we would open the link ``http://localhost:5984/enotes/_design/enotes/index.html`` in our browser, an image like this would be displayed:

    .. image:: images/3_main.png
    
Here we can see exactly the content of our ``index.html``. Since we have not added any styling information or program logic, all these are displayed as plain text or hyperlinks. Nothing would happen yet, if we were to click on those links.

"List" Window
_____________

When we click on any tag listed, we want to be presented with all notes that are tagged with this specific tag on a new page. This "list" window also has to be described in our ``index.html``. Add the following lines:

.. code-block:: html

    ...
    ...
    <!-- ====== titleListPage =====  -->
    <div data-role="page" data-theme="b"id="titleListPage">
            
        <div data-role="header" data-position="fixed">
            <h1>select a note</h1>
            <a href="#addPage" data-transition="slideup" >Add Item</a>
            <a href="#tagListPage" data-icon="grid" >TagList</a>
        </div>
        <div data-role="content" id="titleListContent" >
                
        </div>
        <div data-role="footer" data-position="fixed">
            <a href="#addPage" data-transition="slideup" >  Add Item  </a>
            <a href="#tagListPage" data-icon="grid" >  TagList   </a>
        </div>
            
    </div>
    <!-- titleListPage -->
    ...
    ...

This defines the layout of the page displaying a list of notes.

"Add" Window
____________

Now, we want to add an additional page to our ``index.html``. Add the following code inside the ``<body>``:

.. code-block:: html

    ...
    ...
    <!-- ====== add ===== -->  
    <div data-role="page" data-theme="b" id="addPage">  
    
        <div data-role="header"  >
            <p>add a single item - addPage</p>
            <a href="#tagListPage" data-icon="grid" class="ui-btn-right">tag list</a>
        </div>
        
        <div data-role="content" id="addContent" >
            <a>add content in index.html </a>
        </div>
        
        <div data-role="footer" id="addFooter" >
            <div data-role="controlgroup" data-type="horizontal">
            <a href="#tagListPage" id="addCancelButton" data-role="button" data-theme="d">Cancel and go to taglist</a>
            <input type="submit" value="add new - Submit" data-role="button" data-theme="a">
            </div>
        </div>
    
    </div>
    <!-- add -->
    ...
    ...
    
This page is shown when we want to add a new note to our application. When you take a look at the code for the other pages, you will find a link to ``#addPage``. This link refers to this very ``id="addPage``.

"Edit" Window
_____________

To view or edit a note, we also need a special window. Add this code to ``index.html``:

.. code-block:: html

        ...
        ...
        <!-- ====== show and edit ===== -->  
        <div data-role="page" data-theme="b" id="editPage">  
            <!--    <form id="editNote"> -->
            <div data-role="header"  >
                <p>edit a single item - editpage</p>
                <a href="#tagListPage" data-icon="grid" class="ui-btn-right">tag list</a>  
            </div>  
            <div data-role="content" id="editContent" >   
                <a>edit content in index.html </a>
            </div>  
            <div data-role="footer" id="editFooter"  >
                <div data-role="controlgroup" data-type="horizontal">
                    <a op="delete" href="#titleListPage" data-role="button" data-theme="b" >DELETE Note</a>  
                    <a href="#titleListPage" data-role="button" data-theme="d" >Select other Note</a>
                    <a href="#addPage" data-role="button" data-theme="d" >Add Item</a>
                    <a href="#tagListPage" data-role="button" data-theme="d" >TagList</a>
                    <a op="save" href="#titleListPage"  data-role="button" data-theme="a" >SAVE Note</a>  
                </div>
            </div>  
        </div>
        <!-- edit -->
        ...
        ...

This page is not very different from the others because the actual content within the ``id=editContent`` will be added programmatically.

"Error" Window
______________

The last page we will add is a special window for error messages:

.. code-block:: html

    ...
    ...
    <!-- ====== error =====  -->
    <div data-role="page" data-theme="b" id="errorPage">  
        <div data-role="header" id="errorHeader" data-nobackbtn="true">
            <a href="#tagListPage" data-icon="grid" class="ui-btn-right">tag list</a>  
        </div>  
        <div data-role="content" id="errorContent" >  
            error content 
        </div>  
        <div data-role="footer" id="errorFooter"  >
            <div class="ui-body ui-body-b">
                <ul class="ui-block-b">
                    <li> <a href="#titleListPage"  data-role="button" data-theme="d">  Select other Note  </a> </li>
                    <li> <a href="#addPage"  data-role="button" data-theme="d">   Add Item   </a> </li>
                    <li> <a href="#tagListPage"  data-role="button" data-theme="d">   TagList   </a> </li>
                </ul>
            </div>
        </div>  
    </div>
    <!-- error -->  
    ...
    ...

A Look at our CouchApp
----------------------

If you want to take a look at our CouchApp so far, you have to export the dictionary tree of the CouchApp to CouchDB with: ``$enotes couchapp push enotes``. After that, open our application in any web browser with the url ``http://127.0.0.1:5984/enotes/_design/enotes/index.html``.
You should see the following page:

.. image:: images/3_final.png

Since we have not implemented any jQuery or CSS, the content of ``index.html`` is displayed as it is. Each sub page we have entered is rendered at once. This will change with the next chapter.
