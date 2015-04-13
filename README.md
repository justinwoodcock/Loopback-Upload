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
 
	

	