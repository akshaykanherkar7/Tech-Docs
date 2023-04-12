# Swagger API setup

1. **Setup and Installation**

   1. **``` npm install -D swagger yamljs swagger-ui-express ```**

2. In each of the service we need to do the following. 
   1. Create a seperate swaggerConfig.js file.
   2. In the swaggerConfig.js file paste the below code. Eg
   ```js
      const swaggerUI = require("swagger-ui-express");
      const YAML = require("yamljs");
      const swaggerJSDocs = YAML.load("api.yaml");

      const options = {
            customSiteTitle: "Code Improve API Doc",
        };

    module.exports = { swaggerServe: swaggerUI.serve, swaggerSetup: swaggerUI.setup(swaggerJSDocs,options) };

3. Now we need to import swaggerServe & swaggerSetup. 
   1. Go to app.js/index.js/server.js file.
   2. and do the same as below. Eg
   ```js
      const { swaggerServe, swaggerSetup } = require('./Config/swaggerConfig')
 
      app.use("/api-docs", swaggerServe, swaggerSetup); 

4. Now create the api.yaml file. 
   1. Inside api.yaml file we have to write HTTP Methods req.
   2. You can find some req examples here:
   ```js
      openapi: 3.0.0
      info:
      title: Code Improve API
      description: Optional multiline or single-line description in [CommonMark](http://commonmark.org/help/) or HTML.
      version: 1.0 
  

      servers:
      - url: http://localhost:8080/api/products
        description:  Local server 
      - url: https://prod.com/
        description:  Pre Production server
      - url: https://test.com/
        description:  Production server
      
        paths:
        /allProduct:
            get:
            tags:
                - AllProducts List API
            summary: Returns all Products.
            responses: 
                '200':
                description: OK
                default:
                description: Unexpected error
        

        paths:
        /addProduct:
            post:
            tags:
                - Add Product API 
            summary: Adding the Product Detail.       
            post:
            requestBody:
                required: true
                content:
                application/json:
                    schema:
                    type: object
                    properties: 
                        title: 
                        type: string
                        example: "Iphone"
                        price: 
                        type: integer
                        example: 500
                        description: 
                        type: string
                        example: "Iphone"
                        published: 
                        type: boolean
                        example: true
                
            responses:
                '200':
                description: A user object. 
                '400':
                description: The specified user ID is invalid (not a number).
                '404':
                description: A user with the specified ID was not found.
                default:
                description: Unexpected error


        paths:
        /published:
            get:
            tags:
                - All Published Product List API
            summary: Returns all Published Products.
            responses: 
                '200':
                description: OK
                default:
                description: Unexpected error
                
        
        paths:
        /{id}:
            get:
            tags:
                - Search Product by Id API
            summary: Returns The Product Details by id.
            parameters:
                - name: id
                in: path
                required: true
                description: Parameter description in CommonMark or HTML.
                schema:
                    type : integer
                    minimum: 1
            responses: 
                '200':
                description: OK
                '404':
                description: A user with the specified ID was not found.
                default:
                description: Unexpected error

        paths:
        /delete/{id}:
            delete:
            tags:
                - Delete Product by Id API
            summary: Returns The deleted Product.
            parameters:
                - name: id
                in: path
                required: true
                description: Parameter description in CommonMark or HTML.
                schema:
                    type : integer
                    minimum: 1
            responses: 
                '200':
                description: OK
                '404':
                description: A user with the specified ID was not found.
                default:
                description: Unexpected error


        paths:
        /update/{id}:
            put:
            tags:
                - Edit Product API 
            summary: Adding the Product Detail. 
            parameters:
                - name: id
                in: path
                required: true
                description: Parameter description in CommonMark or HTML.
                schema:
                    type : integer
                    minimum: 1      
            requestBody:
                required: true
                content:
                application/json:
                    schema:
                    type: object
                    properties: 
                        title: 
                        type: string
                        example: "Iphone"
                        price: 
                        type: integer
                        example: 500
                        description: 
                        type: string
                        example: "Iphone"
                        published: 
                        type: boolean
                        example: true
            responses:
                '200':
                description: A user object. 
                '400':
                description: The specified user ID is invalid (not a number).
                '404':
                description: A user with the specified ID was not found.
                default:
                description: Unexpected error 

5. For more on swagger API you can refer below GitHub Repositories. 
   - [CRUD Using Swagger](https://githubcomakshaykanherkarzypeUnit-Testing_Node-Jest-Sequelize-Supertest) 
   - [Node-Swagger](https://github.com/akshaykanherkarzype/node-swagger) 