The REST API
============

Overview
========
At the lowest level, Minus exposes its API with a REST, http-accessible, API. To make it easier for application builders, we will be providing Python, .NET, Ruby, and other API's in the near future.

This file documents the REST API.

The API
=======
We have designed our API to allow you to easily create galleries and upload files programmatically. There are currently 7 API’s published with more to come in the near future. The API is open and no registration is required.

Please subscribe to our http://blog.min.us for any updates regarding our API and contact us if you have any questions.

We have designed our API to allow you to easily create galleries and upload files programmatically. There are currently 7 API’s published with more to come in the near future. The API is open and no registration is required.

Please subscribe to our http://blog.min.us for any updates regarding our API and contact us if you have any questions.

CreateGallery
====================

* http://min.us/api/CreateGallery

Description
----------------

* creates a gallery on the server side and returns the editor and reader id. The editor id combined with the key can be used to upload images into the gallery. 
* On return, the gallery can be immediately viewed on the web site.

Example
-------------
A call returns the below HttpResponse.

	{"editor_id": "ej0rg", "reader_id": "vodiX5"}
	
This gallery can be edited in any browser by going to http://min.us/mej0rg.
The gallery can be viewed via http://min.us/mvodiX5. 

UploadItem
===================

* http://min.us/api/UploadItem

Parameters
--------------
* URL should be structured as follows: http://min.us/api/UploadItem?editor_id=enRlK&key=OK&filename=jcm3U.png
* Body: A standard multipart file POST.

Result
----------
You can immediately view this picture in any browser by: http://min.us/icBpkM.

	{"id": "cBpkM", "height": 64, "width": 500, "filesize": "1010 bytes"}

Note that you must prefix the id with a /i (if it's an item) or a /m (if it's a gallery) to see access the uploaded item or new gallery.

Example
----------
Note that the editor_id does not have the leading m that is in the page url.

	curl "http://min.us/api/UploadItem?editor_id=dn48vKBiP3q9&key=OK&filename=min.png" -F "file=@min.png" 

Example Code:
http://min.us/mvwUFP

SaveGallery
===========
* http://min.us/api/SaveGallery

Description
----------------
* Use this to update the gallery name or change sort order.
* application/x-www-form-urlencoded
* items=%5B%22A1Q%22%5D&name=test2&key=OK&id=bgZrGCaapOSL2
* items is a json encoded list of the gallery items: {"A1Q", "B1Q"}

Example
----------

    import httplib, urllib, urllib2, json
    params = {"name":"test2","id":"bgZrGCaapOSL2","key":"OK","items":json.dumps(["A1Q"])}
    encoded = urllib.urlencode(params)
    f = urllib2.urlopen('http://min.us/api/SaveGallery ', encoded)

GetItems
============
* http://min.us/api/GetItems

Description
----------------

* Use this to retrieve items inside gallery programmatically

Example
----------

http://min.us/api/GetItems/mvph5BW

	{
		"READ_ONLY_URL_FOR_GALLERY": "vph5BW", 
		"GALLERY_TITLE": "Minus for WP7 Screens", 
		"ITEMS_GALLERY": [
			"http://i.min.us/jlZEMU.PNG", 
			"http://i.min.us/jlVqay.PNG", 
			"http://i.min.us/jlVwyM.PNG", 
			"http://i.min.us/jlZCEM.PNG", 
			"http://i.min.us/jmarjo.PNG", 
			"http://i.min.us/jlVnSq.PNG", 
			"http://i.min.us/jlVQoA.PNG", 
			"http://i.min.us/jlVSwI.PNG", 
			"http://i.min.us/jmavz4.PNG", 
			"http://i.min.us/jmatrw.PNG"
		]
	}

SignIn
============
* http://min.us/api/SignIn

Description
----------------

* Use this to sign in a user and get a cookie to use to add galleries to the user's "My Galleries" page.
* POST request type, parameters are **username** and **password1**, with the already registered username and password for the user.
* The reply is a json of a dictionary with a key you should check: "success": True/False. Other values in the dictionary give error information telling the user about invalid username, password, or username-password combination.
* Retain cookie to place subsequent visited or created galleries in the user's "My Galleries" page.

SignOut
============
* http://min.us/api/SignOut

Description
----------------

* GET request. No params.
* Clears cookie, signs out user.

My Galleries
============
* http://min.us/api/MyGalleries.json

Description
----------------

* GET request. No params.
* Exposes My Galleries page as a convenient json call.
* If the edit url is not available, instead of starting with 'm', it will be "Unavailable". If the gallery has been deleted, the value is "Deleted". 

Example
----------

	{
		"galleries": [
		{ "last_visit": "7 minutes ago", "name": "test", "item_count": 1, "clicks": 4, "reader_id": "vgkRZC", "editor_id": "bgZrGCaapOSL2" },
		{ "last_visit": "5 hours ago", "name": "test2", "item_count": 1, "clicks": 0, "reader_id": "vgkRZB", "editor_id": "bgZrGCaapOSL1" } 
		]
	}
	



