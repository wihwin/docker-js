#  mydockerjs
A  javascript utility library  for docker and docker-compose, it provides a simple javascript wrapper in order to execute docker commands and an api in order to use Docker with a node server. 
It's already in beta version, documentation , examples and functionality will be done soon.  

## Getting Started

```
npm install --save mydockerjs 
``` 

to use in your projects. 
 
The library provides : 
* docker : a docker js library in order to execute docker commands 
* imageMgr : an image util library to retrieve informations about installed docker images and push / pull images
* dockerCompose : a docker compose library to run docker compose commands and parse json objects in yaml docker-compose syntax   





If you want to test, to edit or to try example:   
``` 
git clone https://github.com/giper45/docker-js.git 
```  

in examples dir you'll find some example:
``` 
cd examples; 
node <namejs> 

```
## Prerequisites

In order to use you should first install **docker** engine and **docker-compose**  . The library also uses **curl** .


# Examples 

All the api provides a simple syntax with callback function and params options : 
```javascript
function nameF(dependentFunctionParameters, callback, paramsInput)
```


## Docker utils  
Some utility function
Import: 
```javascript
const du = require('mydockerjs').dockerUtils;
```
Check if dockerEngine is running: 
```javascript
  du.isDockerEngineRunning((err, isRunning) => {
      //DO SOMETHING
  }); 
```
Check if docker-cli / docker-compose commands are installed: 
```javascript
//Docker CLI
 // ASYNC MODE 
 du.isDockerCliInstalled((err, isInstalled) => {
       // DO SOMETHING
  }); 
 // SYNC MODE
   du.isDockerCliInstalledSync() //Return true if docker Cli is installed 
   
// Docker COMPOSE
 //ASYNC MODE
 du.isDockerComposeInstalled((err, isInstalled) => {
      // DO SOMETHING
   }); 
 //SYNC MODE
 du.isDockerComposeInstalledSync()

```
## Docker images   
Import : 
```javascript
const dockerImages = require('mydockerjs').imageMgr
``` 

Get images name : 
```javascript


//Get image names  
dockerImages.getNames(function(err, json) {
        utils.print(err,json)
}, {onlytagged:true})

```


Get detailed list : 
```javascript
//Get detailed list of images in javascript object
dockerImages.getDetailedList(function(err, data) {
        utils.print(err,data)

})

```

Get jsonList: 
```javascript
//Get json list of images
dockerImages.getJSONList(function(err, data) {
        utils.print(err, data)
})

``` 

Remove untagged images : 

```javascript
dockerImages.removeUntagged(function(err, data) {
        utils.print(err, data)
})

```

Each get function receives a callback and a paramsInput   

Available options for the paramsInput are :  
* onlytagged: if true returns only tagged images 
* filterLabel : TBD




## Docker library 

Import : 
```javascript
const dockerJS = require('mydockerjs').docker 

``` 

Get active containers : 
```javascript

  dockerJS.ps(function(err, dockerContainers) {
                                if(err) 
                                        console.log(err) 
                                else 
                                        console.log(dockerContainers)
                        })  


```
Remove all unactive containers : 

```javascript
dockerJS.rmAll(function(err, data) {
        utils.print(err, data) 
})

```
Run a container : 

```javascript
dockerJS.run('hello-world', function(err, data) {
        if(err) 
        {   
                console.log("Some err:") 
                console.log(data) 
        }   

        else { 
                console.log(data)
        }   

})


```

run function accept imageName, callback and paramsInput. 
Available options : 
* detached : if true add -d flag to docker run 
* cmd : add a command to the end docker run  
* name : add a name to running container 

Example : 
```javascript
dockerJS.run('daindragon2/debian_useradd', function(err, data) {
        if(err) 
        {   
                console.log("some error") 
                console.log(err) 
        }   
        else {   
                        console.log("runned") 
                        console.log(data) 
                        //Print running containers
                        dockerJS.ps(function(err, dockerContainers) {
                                if(err) 
                                        console.log(err) 
                                else 
                                        console.log(dockerContainers)
                        })  
                            
            }   

}, {name:"theContainer", detached:true, cmd:'bash'})


```

Docker exec a command : 

```javascript
dockerJS.exec(nameContainer, command, callback, paramsInput) 
```

Available options : 
* detached: if true run the command in detached mode 



Stop all containers: 
```javascript
  dockerJS.stopAll(function(err, data) {
                utils.print(err, data)  
        })  
 ```
 
 Start all containers : 
 ```javascript
   dockerJS.startAll(function(err, data) {
                utils.print(err, data)  
        })  
 
 ```

Create a new network : 
```javascript
      var flags = { 
                     driver : 'bridge',
                     subnet : '192.168.1.1/24'
                        }   


        dockerJS.createNetwork("testRete", function(err, data) {
                utils.print(err, data) 
        }, flags) 
        
```

Remove network : 
```javascript
   var name = 'testRete' 
        dockerJS.removeNetwork(name, utils.print)
```
Network prune (destroy all inactive networks) : 
```javascript
        dockerJS.networkPrune(utils.print) 
```

Network List : 
```javascript
        dockerJS.networkList(utils.print)
```
Get infos about a container : 
```javascript
  //Select an existsent container
        name="existentContainer"
        dockerJS.getInfoContainer(name, utils.print)
```

## docker-compose 
To use : 
```javascript
  dockerComposer = require('mydockerjs').dockerComposer
```
The functions follows this convention : 
```javascript
dockerComposer.functionName(
        pathContainingDockerComposeYaml, 
        callback,
        dockerComposeLogF
        )
```
where pathContainingDockerComposeYaml where is located the docker-compose yaml that you want to up / down, callback is called when the  docker-compose command finishes , dockerComposeLogF is a function called each time that docker-compose write a newline on the console (docker-compose writes all logs on **stderr** )  



docker-compose up : 
```javascript
 dockerComposer.up(pathExample, utils.print, function(dataline) {                                                                                                                             
                        console.log(dataline)                                                                                                                                                                
                })     
```

docker-compose down: 
```javascript
 dockerComposer.down(pathExample, utils.print, function(dataline) {                                                                                                                             
                        console.log(dataline)                                                                                                                                                                
                })     
```

docker-compose start : 
TBD

docker-compose stop : 
TBD


## Contributing : 

* Fork it!
* Create your feature branch: git checkout -b my-new-feature
* Commit your changes: git commit -am 'Add some feature'
* Push to the branch: git push origin my-new-feature
* Submit a pull request :D


## Running the tests

TBD


## License 

MIT Licensed
