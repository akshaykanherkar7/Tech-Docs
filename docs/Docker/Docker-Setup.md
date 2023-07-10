# Docker Setup

# Docker-NodeJs-MongoDB

* ***Build your Node image***
    * **Sample Application**
        * Run following commands on terminal
            ```js
            cd [path to your node-docker directory]
            npm init -y
            npm install ronin-server ronin-mocks
            touch server.js

        * Create **server.js** file and add below code
            ```js
            const ronin = require('ronin-server')
            const mocks = require('ronin-mocks')
            
            const server = ronin.server()
            
            server.use('/', mocks.server(server.Router(), false, true))
            server.start()

      * **Test the Application**
        * Run this command on terminal
            ```js
            node server.js

        * Run this curl request on terminal
            ```js
            curl --request POST \
            --url http://localhost:8000/test \
            --header 'content-type: application/json' \
            --data '{"msg": "testing" }'

            curl http://localhost:8000/test

    * **Docker Setup**
      * Create a **Dockerfile** for Node.js
          ```js
          # syntax=docker/dockerfile:1
    
          FROM node:18-alpine
          ENV NODE_ENV=production
          
          WORKDIR /app
          
          COPY ["package.json", "package-lock.json*", "./"]
          
          RUN npm install --production
          
          COPY . .
          
          CMD ["node", "server.js"]
    
      * Create a **.dockerignore** file
          ```js
          node_modules
    
      * Builde Image
          ```js
          docker build --tag node-docker .
    
      * View local images
          ```js
          docker images
    
      * Tag/Update created Image
          ```js
          docker tag node-docker:latest node-docker:v1.0.0
    
      * Remove docker image
          ```js
          docker rmi node-docker:v1.0.0
          
* ***Run your Image as a Container***
    * Run in detached mode
        ```js
        docker run -d -p 8000:8000 node-docker

    * List running containers
        ```js
        docker ps

    * List all containers (running+non-running)
        ```js
        docker ps -a

    * Restart container by name
        ```js
        docker restart container-name

    * Stop running container
        ```js
        docker stop container-name

    * You can give container-name as you want
        ```js
        docker run -d -p 8000:8000 --name container-name image-name

* ***Use Container for developement***
    * **Local database and containers**
      *  Let’s create our volumes now
          ```js
          docker volume create mongodb
          docker volume create mongodb_config
  
      * Create a network
          ```js
          docker network create mongodb
  
      * Now we can run MongoDB in a container and attach to the volumes and network we created above.
        ```js
        docker run -it --rm -d -v mongodb:/data/db \
        -v mongodb_config:/data/configdb -p 27017:27017 \
        --network mongodb \
        --name mongodb \
        mongo

     * Let’s update **server.js** to use MongoDB and not an in-memory data store.
        ```js
        const ronin 		= require( 'ronin-server' )
        const database  = require( 'ronin-database' )
        const mocks 		= require( 'ronin-mocks' )

        async function main() {

            try {
            await database.connect( process.env.CONNECTIONSTRING )
            
            const server = ronin.server({
                    port: process.env.SERVER_PORT
                })

                server.use( '/', mocks.server( server.Router()) )

            const result = await server.start()
                console.info( result )
            
            } catch( error ) {
                console.error( error )
            }
        }

        main()

     * First let’s add the ronin-database module to our application using npm
        ```js
        npm install ronin-database

     * Now we can build our image again.
        ```js
        docker build --tag node-docker .

     * Run the following command in terminal
        ```js
        docker run \
        -it --rm -d \
        --network mongodb \
        --name rest-server \
        -p 8000:8000 \
        -e CONNECTIONSTRING=mongodb://mongodb:27017/notes \
        node-docker

     * Now hit one request in terminal to check weather our container is running or not
        ```js
        curl --request POST \
        --url http://localhost:8000/notes \
        --header 'content-type: application/json' \
        --data '{"name": "this is a note", "text": "this is a note that I wanted to take while I was working on writing a blog post.", "owner": "peter"}'

    * **Use Compose to develop locally*
      * Create a **docker-compose.dev.yml** file and add following code
        ```js
        version: '3.8'

        services:
        notes:
        build:
        context: .
        ports:
        - 8000:8000
        - 9229:9229
        environment:
        - SERVER_PORT=8000
        - CONNECTIONSTRING=mongodb://mongo:27017/notes
        volumes:
        - ./:/app
        - /app/node_modules
        command: npm run debug

        mongo:
        image: mongo:4.2.8
        ports:
        - 27017:27017
        volumes:
        - mongodb:/data/db
        - mongodb_config:/data/configdb
        volumes:
        mongodb:
        mongodb_config:

     * Open the **package.json* file and add the following line to the scripts section:
       ```js
       "debug": "nodemon --inspect=0.0.0.0:9229 -L server.js"

     * Install following npm package
        ```js
        npm install nodemon

     * Let’s start our application and confirm that it is running properly.
       ```js
       docker compose -f docker-compose.dev.yml up --build

     * Make a new curl request on terminal
       ```js
       curl --request GET --url http://localhost:8000/notes

     

    
