## Install Guide
Git project to run a Docker NodeJS application bound to a Docker Redis database instance, exposing an express port on 9191.

- Install vagrant
- cd into the root of the directory
- execute `vagrant up`, please allow 5-10mins for setup to complete
- once finished open a browser and go to http://localhost:9191

### Dev Notes
The file `/dockerNode/src/redis/app.js` shows how the redis binds to the VM Redis database port;
```
var redisHost  = process.env.REDIS_PORT_6379_TCP_ADDR;
var redisPort  = process.env.REDIS_PORT_6379_TCP_PORT;
```
these variables are set by the Redis container when started. Please note the `REDIS_*` comes from the name of the alias set when linking the nodes i.e. 
`docker run -d -P -p 9191:9191 -p 8181:8181 --name node --link redis:redis longie/node`, if `redis:redis` was renamed to `redis:rs` then the you would need to change the redisHost and redisPort to the following;
```
var redisHost  = process.env.RS_PORT_6379_TCP_ADDR;
var redisPort  = process.env.RS_PORT_6379_TCP_PORT;
```
