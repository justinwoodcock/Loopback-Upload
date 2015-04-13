# LoopBack Upload

This project is for demonstrating how to implement file upload functionality using the [**NodeJS**](https://nodejs.org/) framework [**LoopBack**](http://loopback.io), creating a reference on how to recreate it step by step.

## Dependencies
1. **NodeJS:** [nodejs.org](https://nodejs.org/)
2. **LoopBack:** `npm install -g strongloop`
3. **loopback-component-storage:** [github.com/strongloop/loopback-component-storage](https://github.com/strongloop/loopback-component-storage)

## Steps
1. Create Loopback scaffold: `slc loopback`
2. Install loopback-component-storage module: `npm install loopback-component-storage --save`
3. Create new datasource
	1. `slc loopback:datasource`
	2. You will be prompted to name your datasource, standard naming is `container`
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
 5. Create a `Container` model using the StrongLoop CLI, and configure it to utilize the new **container** datasource
 	1. `slc loopback:model` Enter the model name: `Container`
 	2. Select the data-source to attach **Container** to, choose `container (loopback-component-storage)` from the list
 	3. Select model's base class, choose `PersistedModel` from the list
	4. Expose **Container** via the REST API, choose `Yes`
	5. Custom plural form: just leave blank
	6. For the final "add additional properties" question, just leave blank for now
6. Nice work, our upload API is now ready to consume!

## Consuming the API

Now that our endpoint is configured and ready to be consumed you can fire up the app and test it out. For this example we'll demonstrate how to create a new container folder and upload a file to that container. However, in addition to the common LoopBack auto generated endpoints you'll find that the loopback-component-storage module exposes a few new endpoints that are [documented here](https://strongloop.com/strongblog/managing-nodejs-loopback-storage-service-provider/).

1. Fire up your StrongLoop app `slc run .`
2. Open your favorite REST client ([POSTMAN](https://www.getpostman.com/), [Paw](https://luckymarmot.com/paw), etc)
3. Using your REST client, POST to `http://localhost:3000/api/Containers/`, passing a data object with the name attribute (which is the name for the new container) 
	
	```
		{
			"name": "images"
		}
	```
4. You will now see a new location: `./uploads/images/`
5. Create another POST, this time to `http://localhost:3000/api/Containers/images/upload`, passing a file object with the name of `file`.
	
	```
		{
			"file": [fileobject]
		}
	```
6. Success! Navigate to `./uploads/images` and you will see your uploaded file.

 

