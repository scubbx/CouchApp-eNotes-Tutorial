Needed Software
===============

Here will be explained what software to use and why. This page gives only a small overview of the capabilities of software used for CouchApps. For a more detailed explanation please refer to tutorials written specifically for each individual software.

For our application, we need three types of information:

	* Layout
	* Data
	* Program Logic

For this we will only use software and software-principles that are also used in current state of the art web development projects:

	* HTML & CSS
	* JavaScript (including JSON, jQuery and evently)
	* moustache
	* CouchDB
	
The responsibilities of different pieces of software can be seen like this:

==================== ==============================
Part of Application   Techniques used
==================== ==============================
Layout               HTML, CSS, jQuery, mustache
Data                 JSON, evently
Program Logic        JavaScript, jQuery, evently
==================== ==============================
	


HTML & CSS
----------

Everyone involved in web-programming knows HTML at least to some extent. In the CouchApp context, this `HyperTextMarkupLanguage` is used to describe the user interface and design of the application.
It can easily be stored inside the CouchDB as an **attachment** and accessed by a single URL.

Example:

.. code-block:: html

	<html>
		<head>
			
		</head>
		<body>
			<a>Lore ipsum</a>
		</body>
	</html>

HTML holds the **layout** of the CouchApp.

JavaScript
----------

Since HTML does not allow any program 
logic or multidirectional interaction to be implemented by itself, JavaScript is used to accieve this goal. This is only logical because it is natively supported by various browsers on different systems.
JavaScript code can be stored inside the CouchDB as **part of CouchDB - documents** and accessed via the REST interface.

Example:

.. code-block:: js

	function testFunction() {
		alert("This is just a test.");
	}

JavaScript is responsible for **storing the logic** of the program.

JSON
____

There exists a standard notation for describing data in JavaScript. It is called `JavaScriptObjectNotation`.
Actually, this is just a data container (variable) in pure JavaScript.

Example:

.. code-block:: js

	{
		"Entry1" : "Value1",
		"Entry2" : "Value2",
		"Entry3" : "2649",
		"Entry4" : true,
		"Entry5" : [ "blue", "green", "red" ],
		"Entry6" : null
	}

JSON entries can also be nested:

.. code-block:: js

	{
		"Entry1" : "Value1"
		"Entry2" : "Value2",
		"Entry3" : {
			"SubEntry1" : "Value3",
			"SubEntry2" : 342
		},
		"Entry4" : "Value4"
	}

JSON is used whenever information is processed. This covers **data** or **program logic**.

jQuery (mobile)
_______________

jQuery is a JavaScript library that simplifies access to the DOM structure of HTML documents. It also offeres standardised ways to access functions that differ with different browsers and offeres additional effects for the design of HTML pages.

jQuery syntax example:

.. code-block:: js

	$("a").click(
		function() {
			alert("This is just a test.");
		}
	);
	
This code means that whenever you click on an element of the HTML page that is formatted with an ``<a>`` tag (basically every normal text or hyperlink) the function ``function(){ alert("This is just a test."); }`` is executed.
The useage of jQuery with CouchApp is mainly supported by its standardising and graphical powers. It is mainly responsible for the **layout**.
We will use a specialized mobile - version of jQuery called *jQuery mobile*.

jQuery UI CSS
_____________

The jQuery UI CSS is a simple css template which facilitates the creation of custom control elements in a html page.
There is also a mobile - version available which will be used in our context.

http://jqueryui.com/

evently
_______

Evently is a wrapper for jQuery functions. It facilitates the manipulation and addition of event handlers to DOM elements. It also has a mustache parser and allows to apply them via ajax requests.

Basic Example:

.. code-block:: js

	$("exampleDOMobject").evently( {
		click : function() {
			alert("This is a Test.");
			},
		mouseenter : function() {
			alert("This is ANOTHER Test.");
			}
		}
	);

Here we can see a structure similar to a JSON object that defines different functions for different events.
The purpose of evently with CouchApps lies within the **program logic** and **data** handling.

mustache
---------

Mustache is another JavaScript library that lets us insert placeholders in HTML (or any other text). These are specified by ``{{`` at the beginning of the placeholder and ``}}`` at the end.
Take this example for a template text with placeholders like ``{{city}}`` or ``{{temperature_c}}``:

.. code-block:: html

	You are at {{city}}.
	The temperature is {{temperature_c}} degrees C!
	{{#in_f}}
		In Fahrenheit this would be {{temperature_f}}
	{{/in_f}}

The reason this tool is used is that it takes JSON objects as input and inserts its values into placeholders of corresponding name.

The lines between ``{{#in_f}}`` and ``{{/in_f}}`` are only rendered when the variable ``"in_f"`` is set to ``true``.

This is the JSON object that is fed into the mustache parser:

.. code-block:: js

	{
		"city": "Vienna",
		"temperature": 22,
		"temperature_f": ( (22 * 9) / 5 ) + 32,
		"in_f": true
	}
	
Every ``{{city}}`` in our template HTML is replaced by the value of the ``city`` tag in the JSON object. In this case, the substitute would be ``Vienna``.
As you can see by ``"temperature_f"``, you may also put simple formulas as values (like ``( (22 * 9) / 5 ) + 32`` in this case) .
The final output of the template and the JSON object would look like this:

.. code-block:: html
	
	You are at Vienna.
	The temperature is 22 degrees C!
	In Fahrenheit this would be 71.6

Mustache is perfectly suited for our needs because we will receive data our CouchApp requests from the CouchDB as JSON objects.

CouchDB
-------

CouchDB is the heart of every CouchApp. It is the storage of and interface to every bit of information. One does not need to gain knowledge of or implement a specific API since CouchDB uses simple HTTP requests to deliver data in JSON format. Attached files (like HTML or images) can be accessed by a specifically formatted URL.

.. code-block:: html

	http://127.0.0.1:5984/
	
This will give you a welcome message, the version of the database and possibly various other meta information in JSON format:

.. code-block:: js

	{
		"couchdb" : "Welcome",
		"version" : "1.1.0",
	}
	
To get a list of all available databases, one would request:

.. code-block:: html

	http://127.0.0.1:5984/_all_dbs
	
To access previously attached files, only a specific URL is necessary. It is composed of the base URL (``http://127.0.0.1:5984``), the database name (``/albums``), the ID of the database document the file is attached to (``/6e1295ed6c29495e54cc05947f18c8af``) and, at last, the filename itself (``/artwork.jpg``):

.. code-block:: html

	http://127.0.0.1:5984/albums/6e1295ed6c29495e54cc05947f18c8af/artwork.jpg
	
CouchDB will not be explained in detail. For more information please refer to the excellent free online book `"The Definitive Guide to CouchDB" <http://guide.couchdb.org/index.html>`_.

CouchApp
--------

CouchApp itself is not only the name for the concept of storing an application inside an CouchApp, but also an application that helps us create CouchApps.

http://couchapp.org/page/what-is-couchapp
