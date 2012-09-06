### [Live Demo] (http://netmera.com/netmeraJsTest)

# What is that?

[Netmera] (http://netmera.com) is a cloud platform (PaaS) optimized for mobile applications. Netmera offers a cloud based content & data repository. With simple APIs and mobile SDKs it is easy to store, retrieve, search and query data & content on the cloud.

JS-Netmera is a JavaScript library. With JS-Netmera you can use Netmera Rest API via javascript objects and methods. Thus it is easier and more advantageous to use JS-Netmera library for pure javascript applications, rather than wrapping Netmera Rest API in your application.
One of the intended usage of JS-Netmera is for HTML5 applications running both on mobile devices and desktops.
A demo application is also available. Because of [cross-site scripting] (https://developer.mozilla.org/en/http_access_control) limitations in browsers, you can run the demo in modern mobile browsers (such as iPhone Safari). Developers are free to copy and use the code in their applications.

# Usage

	<script type="text/javascript" src="http://netmera.com/netmeraJsTest/jquery-netmera-1.0.js"></script>
	<script type="text/javascript">
		NetmeraClient.init("<your api key>");
	</script>

## NetmeraContent Object

Storing content in Netmera mobile service is done by creating NetmeraContent object which contains key-value pairs of data.

## NetmeraService Ojbect

NetmeraService object is used to retrieve content by its search and get methods. Many query options defined to help finding exact object easily. There are also search methods which get content within the given range of geo-locations.

### Create Content

Following code is used to create content. First, it adds data to the NetmeraContent object as key-value pairs and then calls the create() method to insert data into Netmera repository.

	var content = new NetmeraContent("Blog");
	content.add("title", "My First Blog");
	content.add("description", "This is my first blog content.");
	content.create(function () {
		console.log("Path = " + content.getPath());
		console.log("Title = " + content.get("title"));
		console.log("Description = " + content.get("description"));
	}, function (error) {
		// error is an instance of NetmeraException
		console.log(error.getMessage());
	});

### Delete Content

Following code can be used to delete content from the Netmera repository. In order to delete content either set path to find and delete content or first call get() or search() methods and then delete the retrieved NetmeraContent object.

	var content = new NetmeraContent("Blog");
	content.setPath("/mobimeracontents/_531913427");
	content.delete(function () {
		// Content is deleted
	}, function (error) {
		// error is an instance of NetmeraException
		console.log(error.getMessage());
	});

### Get Content

In order to get the data you need to know the path. In Netmera repository each content has a unique path.

	var service = new NetmeraService("Blog");
	service.setPath("/mobimeracontents/_531913427");
	service.get(function(content) {
		console.log("Path = " + content.getPath());
		console.log("Title = " + content.get("title"));
		console.log("Description = " + content.get("description"));
	}, function(error) {
		// error is an instance of NetmeraException
		console.log(error.getMessage());
	});

### Search

The following code searches the content and returns the list of NetmeraContent objects. If you add searchText then it will search the content repository and retrieve the results which contains the searchText. If searchText is not set then it returns all the content that matches with the objectName.

	var service = new NetmeraService("Blog");
	service.setMax(15); // Get 15 results
	service.setPage(0); // Get the first page
	service.addSearchText("first");
	service.search(function (contents) {
		$.each(contents, function (index, content) {
			console.log("Path = " + content.getPath());
			console.log("Title = " + content.get("title"));
			console.log("Description = " + content.get("description"));
		});
	}, function (error) {
		// error is an instance of NetmeraException
		console.log(error.getMessage());
	});

### Search and Update

In order to update content, first find content and add data to update as key-value pair and then call update() method.

	var service = new NetmeraService("Blog");
	service.setMax(15);
	service.setPage(0);
	service.addSearchText("first");
	service.search(function (contents) {
		$.each(contents, function (index, content) {
			content.add("title", "Updated Blog title");
			content.update(function () {
				console.log("Path = " + content.getPath());
				console.log("Title = " + content.get("title"));
			}, function (error) {
				console.log("Error while updating " + error.getMessage());
			});
		});
	}, function (error) {
		console.log("Error while searching " + error.getMessage());
	});

### Search Query Options

There are different ways to add options to the NetmeraService object. They are used to filter contents.

It filters the content whose "name" is "John".

	service.whereEqual("name", "John");
	
It filters the content whose "name" is not "Tom".

	service.whereNotEqual("name", "John");

It filters the content whose "age" is greater than 20.

	service.whereGreaterThan("age", 20);

It filters the content whose "name" is greater than or equal to 20.

	service.whereGreaterThanOrEqual("age", 20);

It filters the content whose "age" is less than 30.

	service.whereLessThan("age", 30);

It filters the content whose "name" is less than or equal to 30.

	service.whereLessThanOrEqual("age", 30);

It filters the content which contains "email" key.

	service.whereExists("email", true);

It filters the content which does not contain "email" key.

	service.whereExists("email", false);

It filters the content whose "name" starts with "J".

	service.whereStartsWith("name", "J");

It filters the content whose "name" is ends with "hn".

	service.whereEndsWith("name", "hn");

Say we have a list of names.
It filters the content whose "name" is any name in the list.

	var list = new Array();
	list.push("John");
	list.push("Tom");
	service.whereContainedIn("name", list);

This method filters the content that matches with the query which contains all of the values in the list above.

	var list = new Array();
	list.push("John");
	list.push("Tom");
	service.whereAllContainedIn("name", list);
	
You can set the maximum number of results returning from the search() method. Default value is 10.

	service.setMax(100);
	
This is used for the pagination. It skips page*max results and return the max result. For example, if page = 2, and max = 100 then it skips 2*100=200 results and return the 100 results.

	service.setPage(2);

# TODO
* Netmera Location Documentation
* Netmera User Documentation