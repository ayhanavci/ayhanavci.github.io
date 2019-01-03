---
title: "Microservices, a prototype from scratch"
excerpt_separator: "<!--more-->"
description: "Microservices, with concepts implemented. (by Ayhan Avcı)"
date: "2019-01-01"
excerpt: "Microservices summarized. A fully working prototype is presented with the source code and documentation."
header:
    og_image: /assets/images/simplicity1.png
categories:
  - Projects
tags:
  - Software Architectural Patterns
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

I have been exploring software architectural patterns and I've spent some time on microservices. It has been a hot topic for the past couple of years. I took some notes while researching and wrote some pseudo code. As I went deeper, I kept on writing some small pieces of code along with my notes. Eventually I have decided to turn it into a working software. And here we are. This article and the software attached to it are the products of my self study journey.

# Goals

* Design and implement common Microservice concepts. Implementation should be a fully functional.

* Lean, proof of concept grade source code. Not production grade.

* Keep the business model simple. This is about the technology, not the business domain.

* Use wide variety of technologies, programming languages to demonstrate the atomic and flexible nature of microservices.

* Avoid using any technology or ready solutions specific to microservices. Only use general purpose technologies. All libraries and platforms should also be free to use.

* Implement a basic event store, api gateway, various web services and multiple consumers.

* Test and upload the source code with its documentation.

# Microservices Architecture

Microservice architecture focuses on building single function modules with well defined interfaces and operations. Microservices divides the solution into atomic business modules, and helps create a flexible and scalable product. In microservices, the modules of the end product are distinct and loosely coupled / decoupled.

**Domain Driven Design**: Domain Driven Design is an approach to developing software systems, and in particular systems that are complex, that have ever-changing business rules, and that you expect to last for the long term within the enterprise.

**Decomposition and Aggregates**: Divide and conquer strategy for domain driven design. You break the domain down into subdomains and define the relationship between them. This helps understanding and defining the parts and the steps of your software solution, and create a stable, modular system. 

**Service Discovery / Registration**: The architecture being modular and flexible creates a context that any service could be added to the system from any location at any time and they should be accessible. This requires an API gateway in which consumer applications can meet with the services. The services could be discovered from a central mechanism (Discovery), or each service could send their location information whenever they are up and running (Registration).

## CQRS

At its core, Command & Query Responsibility Segregation is separating read and write operations. The idea is that recording a data should have no side effect on how you read it and vice versa. In practice, this approach separates read and write databases. Event Sourcing usually goes together with CQRS pattern.

![classicdb]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/cqrs.png)

Figure 1. CQRS pattern simplified

**Event Sourcing**: Event sourcing is a way of persisting your application's state by storing the history that determines the current state of your application. You record everything that has occured in a stack fashion (only appends). This allows easy rollback transactions and data audit. Recording the data becomes very simple and high performance. Microsoft has a great [ebook here](https://docs.microsoft.com/en-us/azure/architecture/patterns/event-sourcing)

**Event Store Pattern**: Event store is a type of database system, optimized for storage of events. It has a record focused database and provides API for the services that may want to use the store.

**Database Per Service**: Each service having its own private database helps decoupling them. Each aggregate only needs some part of the data. So it makes sense to create a small part of the full database. CQRS pattern along with an event bus helps us keep the data distributed and relevant to each service.

## Saga

Business transactions spanning multiple services require a mechanism to ensure data consistency across services. The Saga pattern manages failures, ensures consistency and correctness across microservices. A saga is a sequence of local transactions. Sagas are used as state machines that coordinate components of the whole. There are two types;

**Event Broker Pattern - Choreography**: There is no central coordination. Each service produces events when a certain action occurs and knows how to respond to events. So the responsibility of the full transaction is distributed among services. 

**Event Mediator Pattern - Orchestration / Controller / Processor**: There is a central service responsible solely for coordination of other services and the workflow.

# The Software

The business domain I chose is one of the most classic ones so it is very easy to grasp: A shopping app.

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

Table 1. User Stories

## Topology

Modules are scalable and distributed. The design allows decoupled integration of any number of modules developed in any language and deployed anywhere. A new service only needs to be able to access the Event Bus. You can pick the modules and deploy them anywhere as long as it is accessible to Docker network. For instance, I added Accounting service after the system was completed already.

![allsystem]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/allsystem.png)
Figure 2. Communication Topology

## Modules

I used Python with Flask often because it is arguably one of the fastest and cleanest ways to prototype anything. I used others just for the sake of having a variety. For Event Store, I used two docker containers. SDK to build for Linux, Runtime for deployment

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

Table 2. Modules

## Platform Agnostic Design

In microservices, it is important to be flexible and dynamic. These are the design principles related to that.

* Any module can be implemented in any programming language as long as the established communication is satisfied. There can be any number of modules attached to the system later on.

* All services provide Rest API with Json objects. So virtually any consumer can communicate with them.

* Any module can use whatever database it wants.

* There is a single Event Store (can be clustered). Modules send write requests there through Event Bus. They don't need to know how, when and where the data is written.

* All modules are dockerized with Dockerfile and Docker Compose file provided. So they can be run on any host machine without having to know anything about it.

* All modules have a defined Docker network name. So they have their own network in which they communicate with each other. The host network is irrelevant as long as Docker runs on it.

## Aggregates

Business model that is sufficiently decomposed, simplified and satisfies all of the user stories is as follows.

![Aggregates]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/aggregates.png)
Figure 3. Aggregates Diagram

## The Data Model

* Event Store Database: MongoDB. Keeps Json data. Never updates or deletes. Just appends (insert).
* Customer Database: Redis. Keeps key-value pairs. Key is username, value is user data.
* Product Database: SQLite. Category - Product relational model. Since this is an SQLite database, it has limitations on scaling.
* Order Database: PostgreSQL. Records Orders and states.
* Accounting Database: CouchDB. Records Invoices after an Order is finalized.

The key here is that everything worth recording must be first recorded on the Event Store since this is the source of all real data. The other databases are all just for reading. Below is a small sample on how it works. If a client asks for products (GET method), then the service queries its local database and returns the results without having to deal with the event store. But if the client performs an add/update/delete operation (POST method) then the service first ensure the Event Store records it first. Then it updates its own database as well.

![cqrssequence]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/cqrssequence.png)
Figure 4. CQRS in action

**How Data Looks Like**: To demonstrate how data is stored on each database, consider the following scenario;

1. Product Manager logs in, __Adds__ a new product.
2. Product Manager decides he made a mistake in parameters, __Updates__ the product he just created
3. A Customer opens the app on his smart phone, takes a look at the products and decides to __Order__ the product that was previously recorded.
4. The Order Saga communications occur and the system __Approves__ the order and it is now ready to get shipped.
5. The Customer decides he needs more money after the purchase and asks Product Manager to increase his credit. Product Manager __Updates__ the Customer's credit.

![addproduct]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/addproduct.png){:height="335px" width="256px"}

Figure 5. Product manager adds a product

Event Store after these events occur:

![eventstoredata]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/eventstoredata.png)

Figure 6. Event Store after the story.

There are NO updates or deletes actually happening in the Event Store. Only inserts. The Update Json message here doesn't update any previous record. Every event is just stacked on top of each other with its Json Object. Below is how a stored json looks like for adding a product.

![storedjsonsample]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/storedjsonsample.png)

Figure 7. Stored Json for Add Product event.

This is how synchronized data is stored. After Event Store secures the data, it fires an event through the Event Bus, notifying all interested parties that a new record is made. And they also update their local databases, if they require to do so.

![productlocaldb]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/productlocaldb.png)
Figure 8. Product Database after the story

Order Service keeps the latest state of the Order and Customer service keeps the latest Credit score of the Customer. It just keeps the latest state of the order. Each web service uses their local database when their GET methods are invoked.

![orderlocaldb]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/orderlocaldb.png)
Figure 9. Order Database after the story

Customer database on Redis.

![customerlocaldb]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/customerlocaldb.png)

Figure 10. Customer Database after the story

## Communication Model

* The web services provide Restful Http Web Services to communicate with the consumers.
* The services also communicate with each other through an Event Bus. The Event Bus I used is RabbitMQ, uses Amqp.
* These are happening within docker containers so there is also Docker networking involved in all of these communications.

**Event Bus**:  Event bus is a software architectural pattern which allows the parts of your solution communicate with each other without having to know each other's location or even existence. Instead of directly sending events to its recipient, modules send it to the event bus. Each service just responds to the events it subscribed to. Web services do NOT and should not call each other's API otherwise you are going to get a very messy, tightly coupled design. [RabbitMQ](https://www.rabbitmq.com) is perfect for all of this.

![eventbus]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/eventbus.png)

Figure 11. Event Bus

Rabbit MQ supports several software patterns such as Publish/Subscribe, Topic, Work Queues and RPCs. It is best to check the [official web site](https://www.rabbitmq.com/getstarted.html) to learn about how these patterns work. But here is an intro:

![rabbit1]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/rabbit1.png)

Figure 12. Producer - Consumer model with Queues, Exchanges and Routing keys

Any application can publish events into any routing key through any exchange. Interested consumers can create their own queues and wait for the messages they have subscribed into. In this example Consumer 1 is interested in Key 1, Consumer 2 is interested in Key 2 and Consumer 3 is interested in both types of events.

In the source code, I made all the messages persistent. So even if one of the services crashes, the message stays in the event bus and the service is going to retrieve it when it is up and running.

Below is how Event Store communicates with services to record and publish events.

![eventbusrouting1]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/eventbusrouting1.png)

Figure 13. Event Store communication with Services over Event Bus

First one is when services POST api are called so each service decides to record the data on the Event Store. The following three are the responses. And the last one is event broadcast on the Event Bus.

And below is how communication for Order Saga occurs. Notice that queues and Exchange names are re-used but routing keys are not. 

![eventbusrouting2]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/eventbusrouting2.png)

Figure 14. Event Saga communication between Services over Event Bus

## Order Saga

This is the one and only saga that spans over all services. Everything begins when a customer likes a product and places an order. To keep messages simple, he can place one product at a time so there is no shopping chart.

Prelude:

1. Customer places an order on a product
2. eCommerce website or eCommerce android application sends the request to reverse proxy.
3. Reverse proxy redirects the requests to Order web service
4. Order Web service starts the Order Saga

The Order choreography:

![Order Saga]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/ordersaga.png)

Figure 15: Order Saga Sequence Diagram

I have excluded some of the rollback transactions. The only checks here are if the product is in store and if the user has enough credits to buy it.

1. Order service sends a place order message to Event Store.
2. Event Store records the request. Tells Order service that the request is approved. Order service updates its query db.
3. Order service starts the saga. Through Event Bus, it makes a broadcast to everything that listens to the saga events.
4. Product, Customer and Accounting services are interested in the saga.
* Product service sends the price & stock information to the Order Service.
* Customer service sends the credit information to the Order Service.
5. Order service checks if the product is in stock and the customer has enough credits and approves/denies the purchase.
6. Event Store records the purchase and responds to Order Service to finalize.
7. Order Service updates it Query database. Broadcasts to all recepients that an Order is finalized.
* Product service reduces the units in stock.
* Customer service reduces the customer credit.
* Accounting service creates an invoice.

In a very basic representation, if you remove everything related to distributed microservice architecture and asynchronous working, this flowchart is what the order saga essentially does.

![Order Flow]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/orderflowchart.png)

Figure 16: Old school flowchart

## Docker Containers

From the Docker website: Docker containers are a key enabling technology for microservices, providing a lightweight encapsulation of each component so that it is easier to maintain and update independently. With Docker Enterprise, you can independently deploy and scale each microservice, coordinate their deployment through Swarm or Kubernetes orchestration and collaborate across teams through a consistent way of defining applications. 

I encapsulated all modules in Linux dockers except for the android application. I used Alpine when available. It is the most lightweight option. For each and every one of the modules, there is a docker compose file. In most docker compose files, you can see "msdemo" name. It is the prefix I came up with, meaning: **"Micro Services Demonstration"**.

In most of the cases I used shell scripts to automate building the source code & executing the binaries. Most of them are named ```run.sh``` and some of them are ```build.sh```. Each module should be up and ready simply by calling ```docker-compose up``` from terminal on root folder of the module. The only exception is CouchDB which requires creating the user database explicitly.

**Networks**: I have created more than necessary amount of docker networks just to demonstrate how granular and flexible the networking can be done. For instance, Order service and Order database have their own private network so that nothing outside can access the Order database.

Here is a list of docker networks on my machine. A good amount of them are used for this project.
![networkslist]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/networkslist.png)

Figure 17: Docker Networks

## RESTful Web Services

Representational State Transfer is a software architectural style that defines a set of constraints to be used for creating web services. REST is web standards based architecture and uses HTTP Protocol. It revolves around resource where every component is a resource and a resource is accessed by a common interface using HTTP standard methods. In my implementation, web services provide GET and POST methods only accepting and producing Json data.

| Customer (Python) | Order Service(Java)  | Product Service(Python)  | Accounting Service(JS) |
| ---------------- | -------------- | ---------------- | ------------------ |
| login-user | place-order | get-products | get-revenue |
| add-user | get-orders | get-all-products ||
| update-user || get-product-details ||
| get-user || add-new-product ||
| get-all-users || update-product ||
| get-credit || delete-product ||
| set-credit || get-categories ||
||| add-new-category ||
||| update-category ||
||| delete-category ||

Table 3. Services and API

## Tools

These provide the system ability to communicate.

**Event Bus**: Previously explained on the communication model. RabbitMQ on a docker container acts as the Event Bus.

**Event Store Implementation**: C# .NET Core application that does the following;

1. Receives write requests from services
2. Stores the request
3. Sends write success to the requesting service
4. Publishes the change

**Reverse Proxy**: I wouldn't use any premade service discovery or registry solutions. One could be implemented from scratch but I just wasn't interested. Reverse proxy seemed like a good enough replacement.
The reverse proxy acts as an API gateway. I have dockerized and configured a Nginx server and placed it inside the docker network. It detects the requests redirects it to the related docker container. The interesting part here is that even the Reverse Proxy itself doesn't know actual locations of the services. They could be configured to be anywhere with technologies like Docker Swarm or Kubernetes.

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

Note that addresses such as ```http://msdemo-service-customer/``` exist within the docker networks, defined inside docker compose files. Below is the customer service compose file, check the network aliases;

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

## Consumers

The consumers are any number of clients who use the webservices for whatever logic. Three of them are implemented but more can be added. 

**ECommerce Website**: Developed using Python + Flask. Containerized. Using this website, users can Register, Login, Logout, View Categories, View Products, Order Products, View and update their profile, view their order history and order status.

**ECommerce Native Android App**: Developed as a native Android apk using Java. The purpose is the same as ECommerce website but with few options missing (you can't update user details or view categories)

![android1]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/android1.png){:height="320px" width="170px"}
![android2]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/android2.png){:height="320px" width="170px"}

Figure 18. Android application screenshots

**Management Website**: Developed using Python + Flask. Containerized. Product admins can Login, Add/Update/Delete Categories and Products, Edit customer credits, View all Orders history and their status.

## Running the Project

All modules are prepared for Docker Compose. The classic option is running compose from terminal. You need to repeat the following process for each module.

1. Open a terminal. Change directory to the module
2. Type ```docker-compose up```

That's it. Everything should install inside its own container and run in there. Aside from Docker itself, you don't need to explicitly install anything else.

Only couchDB requires the following command after it starts:

```curl -X PUT http://accounting_usr:accounting_pass@127.0.0.1:5984/_users```

Below is how all the modules running looks like. 

![alldockersrunning]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/alldockersrunning.png)

Figure 19. All dockers running on terminal

Both websites run on ports 5001 and 5002 both of which you can edit from their yml files. On Android project, you need to open the settings (upper right corner) inside the app and change the IP / Host of the reverse proxy server.

# Source Code

[https://github.com/ayhanavci/Microservices](https://github.com/ayhanavci/Microservices){:height="175px" width="333px"}

```git pull https://github.com/ayhanavci/Microservices.git```

# Conclusion

This article and the source code were a product of my self study journey. Microservices may seem to get things complicated but it is targeting relatively big and evolving projects. If you don't need it, don't use it. The limited business domain introduced here could be solved by a simple MVC application with a single database. On the other hand, giant companies such as Google, Microsoft and Amazon have all moved their environment into microservice architecture. To use the products of these companies, you don't need to know the architecture itself but it is my personal belief that it is important to learn the basics of any technology you use.
Along the way, I have studied over 20 different software architectural patterns, took notes and coded them too. Maybe I will write about them some other time. At the end of the day, it was great fun and satisfying. Apologies for any grammar or technical mistakes and thank you for reading!
