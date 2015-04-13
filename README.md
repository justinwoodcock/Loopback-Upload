# LoopBack Upload

This project is for demonstrating how to implement file upload functionality using the [NodeJS](https://nodejs.org/) framework [LoopBack](http://loopback.io), creating a reference on how to recreate it step by step.

## Dependencies
1. [NodeJS](https://nodejs.org/)
2. [LoopBack](http://loopback.io/getting-started/)

## Steps
1. Using the StrongLoop CLI, create a Loopback scaffold:
	* `slc loopback`
2. Using NPM, install the [loopback-component-storage](https://github.com/strongloop/loopback-component-storage) module
	* `npm install loopback-component-storage --save`
3. Using the StrongLoop CLI, create a new datasource
	1. `slc loopback:datasource`
	2. You will be prompted to name your datasource, name it `container`
	3. Select the connector for **container**, choose "other" from list of options
	4. Enter the connector name `loopback-component-storage`
	5. The new datasource will be added to `server/datasources.json` and is now available in the explorer as a new datasource
4. Configure the **container** datasource with the desired provider and options. For this example we will be using the filesystem provider, but others (Amazon, Rackspace, Azure, etc) are available and documented [here](http://docs.strongloop.com/display/public/LB/Storage+service)
	1. Create a folder in the root of your project named `uploads`
	2. In `server/datasources.json` locate the **container** object and add the `provider` & `root` attributes, setting the **provider** to `filesystem`, and the **root** to your desired filesystem location. For this example we will be setting **root** to `./uploads`.
	
		```
			"container": {
    			"name": "container",
    			"connector": "loopback-component-storage",
    			"provider": "filesystem",
    			"root": "./uploads"
  			}
  		```
 5. Using the StrongLoop CLI, create a new `Container` model / REST API endpoint & configure it to utilize the new **container** datasource
 	1. `slc loopback:model`
 	2. Enter the model name: `Container`
 	3. Select the data-source to attach **Container** to: choose `container (loopback-component-storage)` from the list
 	4. Select model's base class: choose `PersistedModel` from the list
	5. Expose **Container** via the REST API, choose `Yes`
	6. Custom plural form: leave blank
	7. Add additional properties: leave blank
6. Nice work, our upload API is now ready to consume!

## Consuming the API

Now that our endpoint is configured and ready to be consumed you can fire up the app and test it out. For this example we'll demonstrate how to create a new container folder and upload a file to that container. However, in addition to the common LoopBack auto generated endpoints you'll find that the loopback-component-storage module exposes a few new endpoints that are [documented here](https://strongloop.com/strongblog/managing-nodejs-loopback-storage-service-provider/).

1. Fire up your StrongLoop app `slc run .`
2. Open your favorite REST client ([POSTMAN](https://www.getpostman.com/), [Paw](https://luckymarmot.com/paw), etc)
3. Using your REST client, POST to `http://localhost:3000/api/Containers/`, passing a data object with a name attribute (which is the name for the new container) 
	
	```
		{
			"name": "images"
		}
	```
4. You will now see a new location: `./uploads/images/`
5. Create another POST, this time to `http://localhost:3000/api/Containers/images/upload`, passing a `file` attribute which is your file to upload.
	
	```
		{
			"file": [fileobject]
		}
	```
6. Success! Navigate to `./uploads/images` and you will see your uploaded file.

## Don't have time, hate reading, can't follow directions or just lazy?

1. Make sure you have the dependencies listed above installed.
2. `git clone git@github.com:justinwoodcock/Loopback-Upload.git`
3. `npm install`
4. `slc run .`
5. Refer to the **Consuming the API** section above


 

