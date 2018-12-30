---
title: "Microservices, a prototype from scratch"
excerpt_separator: "<!--more-->"
description: "Microservices, with concepts implemented. (by Ayhan Avcı)"
date: "2019-01-01"
excerpt: "Microservices summarized. A fully working prototype is presented with the source code and documentation."
header:
    og_image: /assets/images/simplicity3.png
categories:
  - Projects
tags:
  - Software Architecture Patterns
  - C Sharp
  - Node.js
  - Python
  - Java
  - Android
  - Docker
  - Flask
  - PostgreSql
  - SQLite
  - CouchDB
  - Redis
  - RabbitMQ
  - Web Services    
  - Event Bus
  - Event Store  
  - CQRS
  - Domain Driven Design
  - Event Driven Architecture    
  - API Gateway

---
{% include toc title="Contents" icon="list-ul" %}

# Introduction

I have been exploring software architecture patterns, and paid some attention to microservices which has been a hot topic for the past couple of years. I took some notes while researching and wrote some pseudo code. As I went deeper into subject, I kept on writing some small pieces of code along with my notes. Eventually I thought, why not design and code a working microservice? And here we are. I do not intend to explain everything about microservices. Just summarize some topics.

# Goals

* Design and implement common Microservice concepts. Implementation should be a fully functional.

* Lean, proof of concept grade source code as opposed to production grade.

* Keep the business model simple. This is about the technology, not the business domain.

* Use wide variety of technologies, programming languages to demonstrate the atomic and flexible nature of microservices.

* Avoid using any technology or ready solutions specific to microservices. Only use general purpose technologies. All libraries and platforms should also be free to use.

* Implement a basic event store, api gateway, various web services and multiple consumers.

* Test and upload the source code with its documentation.

# Microservices Architecture

Microservice architecture is a software architectural style focusing on building single function modules with well defined interfaces and operations.
Microservices divides the solution into business atomic modules, and helps create a flexible and scalable product. In microservices, the modules of the end product (services) are distinct and loosely coupled or even completely decoupled.

## Domain Driven Design

Domain Driven Design is an approach to developing software systems, and in particular systems that are complex, that have ever-changing business rules, and that you expect to last for the long term within the enterprise.

## Decomposition and Aggregates

Divide and conquer strategy for domain driven design. You break the domain down into subdomains and define the relationship between them. This helps understanding and defining the parts and the steps of your software solution, and create a stable, modular system. 

## Service Discovery / Registration

Microservices are a combination of service aggregates. The architecture being modular and flexible creates a context that any service could be added to the system from any location at any time. The consumers of these services should be able to find these services and use them for their intended purposes. This requires an API gateway in which consumer applications can meet with the services.
To solve this requirement, a service needs to be discovered. Each and every service could be discovered from a central mechanism (Discovery), or each service could send their location information whenever they are up and running (Registration).

## CQRS

At its core, Command & Query Responsibility Segregation is seperating read and write operations. The idea is that recording a data should have no side effect on how you read it and vice versa. In practice, this approach seperates read and write databases. Event Sourcing usually goes together with CQRS pattern.

![classicdb]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/cqrs.png)

Figure X: CQRS pattern simplified

### Event Sourcing

Event sourcing is a way of persisting your application's state by storing the history that determines the current state of your application. You record everything that has occured in a stack fashion, like a log file. So instead of physically updating an existing data, you make a new record with the updated fields. You gather the state of your objects / business logic, by combining these recorded events. This allows new ways to perform rollback transactions and data audit. Recording the data becomes very simple and high performance.

### Event Store Pattern

Event store is a type of database system, optimized for storage of events. The data stored here never gets updated or deleted. The state changes of the objects required by the domain is handled by adding more events on top of the stack. It has a record focused database and provides API for the services that may want to use the store.

### Database Per Service

Each service having its own database helps decoupling them. The output of decomposition are the service aggregates. And the database related to each aggregate only needs some of the whole data of the system. So it makes sense to create a small part of the full database. CQRS pattern along with an event bus helps us keep the data distributed and relevant to each service.

## Saga

Business transactions spanning multiple services require a mechanism to ensure data consistency across services. The Saga pattern manages failures, ensures consistency and correctness across microservices. A saga is a sequence of local transactions. Each local transaction updates the database and publishes a message or event to trigger the next local transaction in the saga. Sagas are used as state machines that coordinate components of the whole. There are two types of saga implementation;

### Event Broker Pattern - Choreography

There is no central coordination. Each service produces events when a certain action occurs. Each service knows how to respond to related events produced by other services. So the responsibility of the full transaction is distributed among relevant services. This is aided by an implementation of the Broker software architectural pattern.

### Event Mediator Pattern - Orchestration, Controller or Processor

There is a central service responsible solely for coordination of other services and the workflow. This is related to an implementation of Mediator software architectural pattern.

# The Software

The business domain I chose is one of the most classic ones so it is very easy to grasp: An ecommerce website. 

## User Stories

Template: As a [ USER ] I want to [ ACTION ] So that [ REASONING ]

User Personas:

1. Customer
2. Product Manager

Features:

1. View Products
2. Shop
3. View Order Status
4. View User Profile
5. Get Invoice
6. Modify products, prices, stocks
7. Modify user credits

| User Story | Acceptance Criteria  |
| ---------- | -------------------- |
|As a customer, I want to browse for products so that I can place an order.| View product categories<br>View Products<br>Ability to order each product<br>Get an Invoice|
|As a customer, I want to view my past orders so that it guides my future purposes. |View orders. Display order status, product info, order date|
|As a customer, I want to view my profile information so that I can make adjustments. |View user information and credit<br>Edit name, password, email|
|As a product manager, I want to add/edit/delete products so that customers can buy them|View/Add/Edit/Delete products. Names, prices, suppliers, units in stock |
|As a product manager, I want to view all customer order states so that I can assist if needed|View all orders and their states history in the system|
|As a product manager, I want to edit customer credit so that they can purchase products|View all users and their credit<br>Edit credits|

## The Design

(TODO)

![allsystem]({{ site.url }}{{ site.baseurl }}/assets/img/microservices/allsystem.png)
Figure X: Communication Topography

## Modules

I used Python with Flask often because it is arguably one of the fastest and cleanest ways to prototype anything. I would implement everything in Python if I didn't want to introduce some variety. For Event Store, I used two docker containers. SDK to build for Linux, Runtime for deployment. 

| Module | Category  | Programming Language | Sdk | Docker |
| ------ | --------- | -------------------- | -------- | ------ |
|ECommerce Website|Consumer|Python|Flask|[python:alpine](https://hub.docker.com/_/python/)|
|Manager Website|Consumer|Python|Flask|[python:alpine](https://hub.docker.com/_/python/)|
|Native Android Application|Consumer|Java|Android SDK|-|
|Accounting Database|Database|-|-|[couchdb:latest](https://hub.docker.com/_/couchdb)|
|Customer Database|Database|-|-|[redis:alpine](https://hub.docker.com/_/redis)|
|Event Store Database|Database|-|-|[mongo](https://hub.docker.com/_/mongo)<br>[mongo-express](https://hub.docker.com/_/mongo-express)|
|Order Database|Database|-|-|[postgres:alpine](https://hub.docker.com/_/postgres)|
|Event Bus|Support Tool|-|-|[rabbitmq:management](https://hub.docker.com/_/rabbitmq)|
|Event Store|Support Tool|C#|.NET Core 2.1|[dotnet:2.1-sdk](https://hub.docker.com/r/microsoft/dotnet) <br> [dotnet:2.1-aspnetcore-runtime](https://hub.docker.com/r/microsoft/dotnet)|
|Reverse Proxy|Support Tool|-|-|[nginx:alpine](https://hub.docker.com/_/nginx)|
|Accounting Web Service|Web Service|Javascript|Node|[node:alpine](https://hub.docker.com/_/node)|
|Customer Web Service|Web Service|Python|Flask|[python:alpine](https://hub.docker.com/_/python/)|
|Order Web Service|Web Service|Java|JDK-Jersey|[maven:3.6-jdk-8-alpine](https://hub.docker.com/_/maven)|
|Product Web Service|Web Service|Python|Flask|[python:alpine](https://hub.docker.com/_/python/)|


## Platform Agnostic Design

In microservices, it is important to be flexible and dynamic. These are the design principles related to that.

* Any module can be implemented in any programming language as long as the established communication is satisfied. There can be any number of modules attached to the system later on.

* All services provide Rest API with Json objects. So virtually any consumer can communicate with them.

* The Event Bus uses Amqp protocol with RabbitMQ as the broker. Any module can communicate with the others as long as it talks Amqp.

* Any module can use whatever query database it wants to.

* There is a single Event Store, modules send write requests to this event store through Event Bus. So they don't need to know how, when and where the data is written.

* All modules are dockerized with Dockerfile and Docker Compose file provided. So they can be run on any host machine without having to know anything about it.

* All modules have a defined Docker network name. So they have their own network in which they communicate with each other. The host network is irrelevant as long as Docker runs on it.

To demonstrate these, I have implemented web services and consumers on a variety of platforms and languages.

## The Data Model

If this was a monolithic application with a single, relational data representation. The model below would be sufficient. 

![classicdb]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/classicdb.png)
Figure X: Classic Database Entities

But we are not using that. We have implemented a type of CQRS pattern. So we have a seperate database for each service and each with a different model. 

Event Sourcing Database: This is just a stack of every significant event that has happened in the system as a whole. The database is document oriented as opposed to RDBMS. It only adds and it never updates or deletes any data.

Aside from Event Store database, each service has its own database which merely includes just some related portions of the data inside Event Store. Service databases typically hold the latest states of the data. Event Store database on the other hand, keeps every single change and isn't interested in the latest state. Instead of a relational database, here is what Event Store records when events occur when users take the following actions.

1. Product Manager logs in, __Adds__ a new product.
2. Product Manager decides he made a mistake in parameters, __Updates__ the product he just created
3. A Customer opens the app on his smart phone, takes a look at the products and decides to __Order__ the product that was previously recorded.
4. The Order Saga communications occur and the system __Approves__ the order and it is now ready to get shipped.
5. The Customer decides he needs more money after the purchase and asks Product Manager to increase his credit. Product Manager __Updates__ the Customer's credit.

![addproduct]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/addproduct.png){:height="335px" width="256px"}

Figure X: Product manager adds a product

![orders1]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/customerorders1.png){:height="480px" width="256px"}

Figure X: Customer orders the product

Event Store after these events occur:

![eventstoredata]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/eventstoredata.png)

Figure X: Event Store after the story.

As you can see, there are NO updates or deletes actually happening in the Event Store. Only inserts. Every event is just stacked on top of each other with its Json Object. Below is how a stored json looks like for adding a product.

![storedjsonsample]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/storedjsonsample.png)

Figure X: Stored Json for Add Product event.

This is how synchronized, actual data is actually stored. The Query databases for each service (database per service & Query portion of CQRS pattern) is a different story. After Event Store secures the data, it fires an event through the Event Bus, notifying all interested parties that a new record is made. And they also update their local databases, if they need to do so. 

For instance, the add product event is fired and the Product Service records it into its local SQLite database. And when Update Record event occurs, it simply updates the state of its product. Event Store keeps all the changes but ignores the state, Service database on the other hand just keeps the latest state.

![productlocaldb]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/productlocaldb.png)
Figure X: Product Database after the story

All other local databases are also effected by our little story. Order Service keeps the latest state of the Order and Customer service keeps the latest Credit score of the Customer.

Below is the Order database. It just keeps the latest state of the order. Each web service uses these when their GET methods are invoked. They never have to resort to the actual data in the Event Store.

![orderlocaldb]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/orderlocaldb.png)
Figure X: Order Database after the story

Customer database is on Redis and keeps key - value pairs in which the key is the Customer id and value is a Json object holding the details of the Customer. It also just keeps the latest information of the Customer. Looks weird, but I took the screenshot using Redis-Cli.

![customerlocaldb]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/customerlocaldb.png)
Figure X: Customer Database after the story

Service databases are displayed in the Aggregates Diagram below.

## Aggregates

Business model that is sufficiently decomposed, simplified and satisfies all of the user stories is as follows.

![Aggregates]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/aggregates.png)
Figure X: Aggregates Diagram

## Communication Model

The web services are all Restful Http Web Services, so they accept API calls through Http over TCP/IP protocol either with GET or POST. They also communicate with their databases depending on what database is used. Communicating to CouchDB is wildly different than communicating with PostgreSQL or SQLite. All are handled by official language adapters provided by database vendors. The services also communicate with each other through an Event Bus. The Event Bus I used is RabbitMQ which uses Amqp. This is also handled by recommended adapters, varying for each programming language used (C#, Java, NodeJS and Python). All of these are happening within docker containers so there is also Docker networking involved in all of these communications.

### Event Bus

(TODO: RabbitMQ. Pub/Sub pattern. Topic pattern. Queues, Exchange Names, Routing keys. Visuals)

## Order Saga

This is the one and only saga that spans over all services. Everything begins when a customer likes a product and places an order. To keep messages simple, he can place 1 product at a time so there is no shopping chart.

Prelude:

1. Customer places an order on a product
2. eCommerce website or eCommerce android application sends the request to reverse proxy.
3. Reverse proxy redirects the requests to Order web service
4. Order Web service starts the Order Saga

The Order choreography:

![Order Saga]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/ordersaga.png)
Figure X: Order Saga Sequence Diagram

I have excluded some of the rollback transactions. The only checks here are if the product is in store and if the user has enough credits to buy it.

1. Order service sends a message to Event Store, saying that it wants to place an order.
2. Event Store records the request. Tells Order service that the request is approved.
3. Since data is now atomicly recorded in the Event Store, Order service is now allowed to record the purchase request into its own database.
4. Order service starts the saga. Through Event Bus, it makes a broadcast to everything that listens to the saga events.
5. Product, Customer and Accounting services are interested in the saga. They asynchronously receive the event and respond to it.
* Product service checks if the product is in stock. Sends the price & stock information response back to the Order Service.
* Customer service checks if the customer has enough credit for the purchase. Sends the credit response back to Order Service.
* Accounting service does nothing. It is only interested in the final outcome.
6. Order service checks the responses and if product is in stock and the customer has enough credits, it approves the purchase but doesn't finalize yet. Sends the finalize request to Event Store.
7. Event Store records the purchase and responds to Order Service.
8. Order Service updates it Query database that the order suceeded. Then broadcasts to all recepients that an Order is successfully placed. The following events occur asynchronously.
* Product service reduces the units in stock.
* Customer service reduces the customer credit.
* Accounting service creates an invoice.

Before any real change to product, credit or invoice information, everything is first approved and recorded by the Event Store. It could cancel the transaction at any point with whatever criteria it wants. But I just made it a dummy service that records and approves everything.

In a very basic, old school representation, if you remove everything related to distributed microservice architecture and asynchronous working, this flowchart is what the order saga does.

![Order Flow]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/orderflowchart.png)

Figure X: Old school flowchart

## Docker Containers

Containers are brilliant technology that fits microservices architecture like a glove. From Docker website:
Docker containers are a key enabling technology for microservices, providing a lightweight encapsulation of each component so that it is easier to maintain and update independently. With Docker Enterprise, you can independently deploy and scale each microservice, coordinate their deployment through Swarm or Kubernetes orchestration and collaborate across teams through a consistent way of defining applications.
In my implementation, each and every database, tool, web service and consumer is encapsulated inside a docker container.

In most images, I used Alpine Linux when available. It is the most lightweight option works well. For each and every one of the modules, I wrote a docker compose file. In most docker compose files, you can see "msdemo" name. It is the prefix I came up with, meaning: **"Micro Services Demonstration"**. 

In most of the cases I used shell scripts to automate building the source code & executing the binaries. Most of them are named ```run.sh``` and some of them are ```build.sh```. Each module should be up and ready simply by calling ```docker-compose up``` from terminal on root folder of the module. The only exception is CouchDB which requires creating the user database explicitly.

### Networks

I have created more than necessary amount of docker networks just to demonstrate how granular and flexible the networking can be done. For instance, Order service and Order database have their own private network so that nothing outside can access the Order database. But also, it can be placed anywhere as long as the service can access it with its network name.

![networks]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/networks.png)

Here is a list of docker networks on my machine. A good amount of them are used for this project.
![networkslist]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/networkslist.png)

## RESTful Web Services

Representational State Transfer is a software architectural style that defines a set of constraints to be used for creating web services. REST is web standards based architecture and uses HTTP Protocol. It revolves around resource where every component is a resource and a resource is accessed by a common interface using HTTP standard methods. In my implementation, web services provide GET and POST methods only accepting and producing Json data.

### Customer Service

(TODO)
Python - Flask

### Order Service

(TODO)
Java - Jersey

### Product Service

(TODO)
Python - Flask

### Accounting Service

(TODO)
Javascript - NodeJS

## Tools

(TODO)

### Event Bus 

Event bus is a software architecture pattern which allows the parts of your solution communicate with each other without having to know each other's location (or even existence). Event bus is a general purpose medium with a well defined protocol and communication model which parts of your software agrees on. Instead of directly sending events to each other's location, they send it to the event bus and whoever is interested in the event retrieves it. [RabbitMQ](https://www.rabbitmq.com) is perfect for such solutions. And it was within my goals criteria since it is totally a generic purpose, popular and free solution. More on that later.

All components are decoupled and they do not know the location of other services. They are not even aware of their existence. Each service just responds to the events it receives and never directly contacts other services. Web services do NOT and should not call each other's API.

![eventbus]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/eventbus.png)

Figure X: Event Bus

### Event Store Implementation

(TODO)
C# .NET Core 

### Reverse Proxy

One of my goals was building a lean and concept grade Microservice. So I wouldn't use any premade service discovery or registry solutions. One could be implemented from scratch but I just wasn't interested. So reverse proxy seemed like a good enough replacement for such technologies. Basicly you use an http server as your central API gateway and configure it so that distributed consumer requests are delivered to appropriate services that could be running on anywhere on the network / internet. More details later.

The reverse proxy acts as a basic API gateway. I have dockerized and configured a Nginx server and placed it inside the microservice network habitat. Here is how it works.

Here is the configuration file. It resolves and redirects each request to its related docker container. For instance, a website makes a request to ```http://[my-api-gateway]/order/place-order```. The reverse proxy is sits on root and catches all requests on port 80. In this case, it detects that the request is made to ```/order``` and redirects it to the related docker container address with all its parameters and http body. The interesting part here is that even the Reverse Proxy itself doesn't know actual locations of the services. They could be configured to be anywhere with technologies like Docker Swarm or Kubernetes. (Although I ran them all on my local development machine)

```
worker_processes 1;
events { worker_connections 1024; }

http {
  sendfile off;
  server {
      listen 80 default_server;  
      server_name  localhost;          
      resolver 127.0.0.11 valid=30s;    
                  
      location / {
        index index.html;
        include /etc/nginx/mime.types;
      }
      location /customer {                
        rewrite /customer/(.*) /$1 break;        
        proxy_pass  http://msdemo-service-customer/$1;  
      }  
      location /order {                
        rewrite /order/(.*) /$1 break;        
        proxy_pass  http://msdemo-service-order/$1;  
      }
      location /product {                
        rewrite /product/(.*) /$1 break;        
        proxy_pass  http://msdemo-service-product/$1;  
      }   
       location /accounting {                
        rewrite /accounting/(.*) /$1 break;        
        proxy_pass  http://msdemo-service-accounting/$1;  
      }     
              
  }  
  
}
```

Note that addresses such as ```http://msdemo-service-customer/``` in this configuration are not dummies, they actually exist within the docker networks. They are defined inside docker compose files. Below is the customer service compose file, check the network aliases; 

```yml
version: '3.7'
services:
  web-service-customer:  
    build:
      context: .
      target: web-service-customer
    image: msdemo-web-service-customer:v1.0  
    volumes:          
      - type: bind
        source: ./
        target: /customer    
    command:
      - /customer/run.sh
    ports:
      - 6002:80    
    environment:
      PYTHONUNBUFFERED: 1
      INSTALL_PATH: /customer
      EVENT_BUS_IP: msdemo-event-bus      
      EVENT_BUS_USERNAME: msdemo_usr
      EVENT_BUS_PASSWORD: msdemo_pass
      DATABASE_IP: msdemo-db-customer
      DATABASE_PORT: 6379
      DATABASE_PASSWORD: customer_pass
    networks:
      default: {}
      msdemo-nw:
        aliases:
          - msdemo-service-customer
      msdemo-nw-order:
        aliases:
          - msdemo-service-customer
networks:
  msdemo-nw:
    external:
      name: msdemo-nw
  msdemo-nw-order:
    external:
      name: msdemo-nw-customer
```

## Databases

* Event Store Database: MongoDB. Keeps Json data. Never updates or deletes. Just inserts.
* Customer Database: Redis. Keeps key-value pairs. Key is username, value is user data.
* Product Database: SQLite. Category - Product relational model. Since this is an SQLite database, it has limitations on scaling.
* Order Database: PostgreSQL. Records Orders and states.
* Accounting Database: CouchDB. Records Invoices after an Order is finalized.

The key here is that everything worth recording must be first recorded on the Event Store since this is the source of all real data. The other databases are all just for reading. Below is a small sample on how it works. If a client asks for products (GET method), then the service queries its local database and returns the results without having to deal with the event store. But if the client performs an add/update/delete operation (POST method) then the service first ensure the Event Store records it first. Then it updates its own database as well.

![cqrssequence]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/cqrssequence.png)
Figure X: CQRS in action

## Consumers

The consumers are any number of clients who use the webservices for whatever logic. Three of them are implemented but more can be added. 

### ECommerce Website

Developed using Python + Flask. Containerized. Using this website, users can Register, Login, Logout, View Categories, View Products, Order Products, View and update their profile, view their order history and order status.

(TODO: Screenshots)

### ECommerce Native Android App

Developed as a native Android apk using Java. The purpose is the same as ECommerce website but with few options missing.

(TODO: Screenshots)

### Management Website

Developed using Python + Flask. Containerized. Product admins can Login, Add/Update/Delete Categories and Products, Edit customer credits, View all Orders history and their status.

(TODO: Screenshots)

## The Implementation

(TODO)

## Running the Project

All modules are prepared for Docker Compose. The classic option is running compose from terminal. You need to repeat the following process for each module. 

1. Open a terminal. Change directory to the module
2. Type ```docker-compose up```

That's it. Everything should install inside its own container and run in there. Aside from Docker itself, you don't need to explicitly install anything else.

Only couchDB requires the following command after it starts:

```curl -X PUT http://accounting_usr:accounting_pass@127.0.0.1:5984/_users```

Below is how all the modules running looks like. First column is database dockers: db_eventstore, db_customer, db_order, db_accounting. Second column is web services: service_product, service_customer, service_order, service_accounting. Third column are tools: event_bus, event_store, reverse_proxy. Fourth column is websites: website_ecommerce, website_manager.


Both websites run on ports 5001 and 5002 both of which you can edit from their yml files. On Android project, you need to open the settings (upper right corner) inside the app and change the IP / Host of the reverse proxy server. 

## Conclusion

(TODO)

## Technologies

(TODO)

## Source Code

[https://github.com/ayhanavci/Microservices](https://github.com/ayhanavci/Microservices){:height="175px" width="333px"}

```git pull https://github.com/ayhanavci/Microservices.git```

## Links

(TODO)