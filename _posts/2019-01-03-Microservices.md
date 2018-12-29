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

I have been exploring software architecture patterns, and paid some attention to microservices which has been a hot topic for the past couple of years. I took some notes while researching and wrote some pseudo code. As I went deeper into subject, I kept on writing some small pieces of code along with my notes. Eventually I thought, why not design and code a working microservice? And here we are. I do not intend to explain everything about microservices. Just summarize some topics. This is not a theoric article but I am going to explain and document the design and briefly explain some of the technologies and the source code.

## Goals

* Design and implement common Microservice concepts. Implementation should be a fully functional.

* Lean, proof of concept grade source code as opposed to production grade.

* Keep the business model simple. This is about the technology, not the business domain.

* Use wide variety of technologies, programming languages to demonstrate the atomic and flexible nature of microservices.

* Avoid using any technology or ready solutions specific to microservices. Only use general purpose technologies. All libraries and platforms should also be free to use.

* Implement a basic event store, api gateway, various web services and multiple consumers.

* Test and upload the source code with its documentation.

## Structure

* The software patterns and then the Microservices concepts are going to be summarized.

* The design of the prototype will be explained with visuals.

* The modules will be explained.

## Source Code

(TODO)

# The Concepts

## Software Architecture Patterns

Architecture pattern is a reusable solution to a commonly occurring problem in software architecture. These are not to be confused with [Design Patterns](https://www.goodreads.com/book/show/85009.Design_Patterns). Design patterns are a piece of code in your project, architectural pattern is the design and structure of your software as a whole. There could be multiple design patterns and architectural patterns in your project.

Various architecture patterns are:

* **Monolithic**: The software is composed in one piece, with tightly coupled modules.
* **Layered**: Same as monolithic but the application is divided into components, developed in a horizontal fashion. The layers usually only communicate with the adjacent components. Great for embedded.
* **N-Tier**: Very similar to layered architecture. But implies that it can be distributed so the components can run on different machines as different instances. Most typical is 3-tiered architecture with Presentation - Business - Data layers.
* **Event driven architecture**: Event driven defines the communication model between components of the software. Events could be in the form of user events, socket events, IO completion ports, pipes etc. Event driven architecture could be in Broker pattern or in Mediator pattern. Broker pattern provides a medium in which the components can talk to each other but doesn't interfere with the business logic. Mediator pattern usually includes a controller which orchestrates events between components. Event driven architecture also includes patterns such as Publish/Subscribe and Topic. Most of these patterns are used within the prototype project.
* **Microkernel**: Your software has a bare skeleton that works, and it allows seperate modules get attached to it. In essence, this is a plugin system. Like the old Winamp plugins, or Eclipse, Visual Studio etc. all of which has a plugin market. There are also operating systems based on microkernel architecture.
* **Model-View-Controller (and Model-View-Template)**: A very popular pattern mostly used in web development. Popular frameworks such as .NET and Django employ these patterns. The code is seperated into model, view and controller components. Model deals with the database, usually with an ORM like Entity Framework. View with the presentation to the user, usually html with a syntax like Razor. And the Controller has the business logic in it. This pattern could be considered monolithic & layered in nature since it is usually within a single project and source code is seperated into layers. But the layers in MVC/MVT are not as strict since they all tend communicate with each other.
* Various other patterns are; Blackboard, Pipes and Filters, Peer-to-Peer, Web Queue Worker, Client-Server, Space-based, Event Bus, CQRS, Microservices. Last three will further be explained below.

(TODO: Visual representations of some of the patterns)

Some of the patterns naturally includes others. Some are obselete, some are modern. I don't think there is a consensus on the whole list, even among the same organization. For instance some Microsoft articles have very different pattern list than the others.

## Microservices Architecture

Microservice architecture is a software architectural style focusing on building single function modules with well defined interfaces and operations.
Microservices divides the solution into business atomic modules, and helps create a flexible and scalable product. In microservices, the modules of the end product (services) are distinct and loosely coupled or even completely decoupled. Since there is no single monolithic application, and thanks to decoupling nature, it is easier to continue evolving your product by adding more seperately developed components. Compared to more traditional styles, microservices approach tend to work better for agile and devops.

### Monolithic Architecture

Monolithic architecture is developing your solution as self-contained, one piece application and with tightly coupled components. They have their advantages over microservices or other change friendly systems if your product is very well defined, and doesn't need changes. This is usually the opposite of Microservice requirements.

### Distributed Architecture

Distributed systems is an umbrella term for various software solutions. In essence it means your solution has distinct parts and modules running on seperate machines / platforms. They could be loosely coupled or decoupled. Microservices are distributed in essence. You could have 20 databases and 20 Rest services running on 80 machines, load balancing with each other.

### Domain Driven Design

Domain Driven Design is an approach to developing software systems, and in particular systems that are complex, that have ever-changing business rules, and that you expect to last for the long term within the enterprise. In addition to standart software development literature, you add concepts like Ubiquitous Language that help with communication, Aggregates that helps with seperation of modules, Domain Events, Value Objects etc. It is an approach that can be used for any project, but particularly well suited for Microservices since it is mostly about atomizing the solution.

### Decomposition and Aggregates

Divide and conquer strategy for domain driven design. You break the domain down into subdomains and define the relationship between them. This helps understanding and defining the parts and the steps of your software solution, and create a stable, modular system. In the end, decomposition outputs aggregates and services.

## Service Discovery and Service Registration

Microservices are a combination of service aggregates. The architecture being modular and flexible creates a context that any service could be added to the system from any location at any time. The consumers of these services should be able to find these services and use them for their intended purposes. This requires an API gateway in which consumer applications can meet with the services.
To solve this requirement, a service needs to be discovered. You could either design your environment so that each and every service is dicovered from a central mechanism, or each service sends their information (ip, port, etc.) whenever they are up and running. There are several premade solutions for this such as [Apache Zookeeper](https://zookeeper.apache.org) or [Apache Curator](https://curator.apache.org/curator-x-discovery/index.html).

## Reverse Proxy

One of my goals was building a lean and concept grade Microservice. So I wouldn't use any ready to use service discovery or registry solutions. One could be implemented from scratch but I just wasn't interested. So reverse proxy seemed like a good enough replacement for such technologies. Basicly you use an http server as your central API gateway and configure it so that distributed consumer requests are delivered to appropriate services that could be running on anywhere on the network / internet. More details later.

## Event Bus

Event bus is a software architecture pattern which allows the parts of your solution communicate with each other without having to know each other's location (or even existence). Event bus is a general purpose medium with a well defined protocol and communication model which parts of your software agrees on. Instead of directly sending events to each other's location, they send it to the event bus and whoever is interested in the event retrieves it. [RabbitMQ](https://www.rabbitmq.com) is perfect for such solutions. And it was within my goals criteria since it is totally a generic purpose, popular and free solution. More on that later.

## CQRS

At its core, Command & Query Responsibility Segregation is seperating read and write operations. The idea is that recording a data should have no side effect on how you read it and vice versa. I am not going to discuss it here since it is not the goal and I am not an expert. But in practice this approach may seperate read and write databases themselves and the structure of the data on each database. When recording data, Event Sourcing usually goes together with CQRS pattern.

![classicdb]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/cqrs.png)

Figure X: CQRS pattern simplified

### Event Sourcing

Event sourcing is a way of persisting your application's state by storing the history that determines the current state of your application. You record everything that has occured in a stack fashion, like a log file. So instead of physically updating an existing data, you make a new record with the updated fields. You gather the state of your objects / business logic, by combining these recorded events. This allows new ways to perform rollback transactions and data audit. Recording the data becomes very simple and high performance. 

### Event Store Pattern

Event store is a type of database system, optimized for storage of events. The data stored here never gets updated or deleted. The state changes of the objects required by the domain is handled by adding more events on top of the stack. For our purposes, it is the implementation of the Event Sourcing pattern. It has a record focused database and provides API for the services that may want to use the store.

### Database Per Service

Each service having its own database helps decoupling them. The output of decomposition are the service aggregates. And the database related to each aggregate only needs some of the whole data of the system. So it makes sense to create a small part of the full database. CQRS pattern along with an event bus helps us keep the data distributed and relevant to each service. One implementation could be like this;

* Whenever a record event occurs, it gets broadcast through event bus.

* Event Store records all the data and we use it only for recording.

* If any service needs the data, it registers to catch the record event on the event bus. And record it to its own local database.

* When a service needs to retrieve the data, it uses its own local, read database instead of having to resort to event store.

Seperation of read & write databases makes the process fast and lean.

## Saga

Business transactions spanning multiple services require a mechanism to ensure data consistency across services. The Saga pattern manages failures, ensures consistency and correctness across microservices. A saga is a sequence of local transactions. Each local transaction updates the database and publishes a message or event to trigger the next local transaction in the saga. Sagas are used as state machines that coordinate components of the whole. There are two types of saga implementation, both of these scenarios typically uses an Event Bus for communication, as the media for the event traffic. In my case, I used a choreography saga over an event bus.

### Event Broker Pattern - Choreography, Controller or Processor

There is no central coordination. Each service produces events when a certain action occurs. Each service knows how to respond to related events produced by other services. So the responsibility of the full transaction is distributed among relevant services. This is aided by an implementation of the Broker software architectural pattern.

### Event Mediator Pattern - Orchestration

There is a central service responsible solely for coordination of other services and the workflow. This is related to an implementation of Mediator software architectural pattern.

## Containers

Containers are brilliant technology that fits microservices architecture like a glove. From Docker website:
Docker containers are a key enabling technology for microservices, providing a lightweight encapsulation of each component so that it is easier to maintain and update independently. With Docker Enterprise, you can independently deploy and scale each microservice, coordinate their deployment through Swarm or Kubernetes orchestration and collaborate across teams through a consistent way of defining applications.
In my implementation, each and every database, tool, web service and consumer is encapsulated inside a docker container.

## RESTful Web Services

Representational State Transfer is a software architectural style that defines a set of constraints to be used for creating web services. REST is web standards based architecture and uses HTTP Protocol. It revolves around resource where every component is a resource and a resource is accessed by a common interface using HTTP standard methods. In my implementation, web services provide GET and POST methods only accepting and producing Json data.

## Platform Agnostic Integration

In microservices, it is important to be flexible and dynamic in your continuous development and integration. With that in mind, I designed my development environment so that:

* Any module can be implemented in any programming language as long as the established communication is satisfied.

* All services provide Rest API with Json objects. So virtually any consumer can communicate with them.

* The communication of the Event Bus uses Amqp protocol with RabbitMQ as the broker. Any module can communicate with the others as long as it talks Amqp.

* Any module can use whatever query database it wants to.

* There is a single Event Store, modules send write requests to this event store through Event Bus. So they don't need to know how, when and where the data is written.

* All modules are dockerized with Dockerfile and Docker Compose file provided. So they can be run on any host machine without having to know anything about it.

* All modules have a defined Docker network name. So they have their own network in which they communicate with each other. The host network is irrelevant as long as Docker runs on it.

To demonstrate that, I have implemented web services and consumers on a wide variety of platforms. The full list will be given later.

# The Software

The business domain I chose is one of the most classic ones so it is very easy to grasp: An ecommerce website. There are products and categories in it. Customers can register, login and browse products. Check their prices and purchase products. If their credit is sufficient and there are enough units in stock, the order succeeds. Product manager can log in to seperate website to edit products, categories and user credits.  
It is important to note that this sort of solution could be implemented, say, as a monolithic MVC application way faster and simpler. But we are going to use Microservices architecture anyway.

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

## The Data Model

If this was a monolithic application with a single, relational data representation. The model below would be sufficient. 

![classicdb]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/classicdb.png)
Figure X: Classic Database Entities

But we are not using that. We have implemented a type of CQRS pattern. So we have a seperate database for each service and each with a different model. 

Event Sourcing Database: This is just a stack of every significant event that has happened in the system as a whole. The database is document oriented as opposed to RDBMS. It only adds and it never updates or deletes any data.

Aside from Event Store database, each service has its own database which merely includes just some related portions of the data inside Event Store. Service databases typically hold the latest states of the data. Event Store database on the other hand, keeps every single change and isn't interested in the latest state. Service databases are included in the Aggregates Diagram.

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

In most images, I used Alpine Linux when available. It is the most lightweight option works well. For each and every one of the modules, I wrote a docker compose file. In most docker compose files, you can see "msdemo" name. It is the prefix I came up with, meaning: **"Micro Services Demonstration"**. The list of modules and the docker images I used are as follows;

* ECommerce Website: ```python:alpine```
* Manager Website: ```python:alpine```
* Accounting Database: ```couchdb:latest```
* Customer Database: ```redis:alpine```
* Event Store Database: ```mongo``` and ```mongo-express``` to manage database.
* Order Database: ```postgres:alpine```
* Event Bus: ```rabbitmq:management```
* Event Store: This one is two phased. I used ```microsoft/dotnet:2.1-sdk-alpine``` to compile the code and ```microsoft/dotnet:2.1-aspnetcore-runtime-alpine``` for deployment.
* Reverse Proxy: ```nginx:alpine```
* Accounting Web Service: ```node:alpine```
* Customer Web Service: ```python:alpine```
* Order Web Service: ```maven:3.6-jdk-8-alpine```
* Product Web Service: ```python:alpine```

In most of the cases I used shell scripts to automate building the source code & executing the binaries. Most of them are named ```run.sh``` and some of them are ```build.sh```. Each module should be up and ready simply by calling ```docker-compose up``` from terminal on root folder of the module. The only exception is CouchDB which requires creating the user database explicitly.

### Networks

I have created more than necessary amount of docker networks just to demonstrate how granular and flexible the networking can be done. For instance, Order service and Order database have their own private network so that nothing outside can access the Order database. But also, it can be placed anywhere as long as the service can access it with its network name.

![networks]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/networks.png)

Here is a list of docker networks on my machine. A good amount of them are used for this project.
![networkslist]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/networkslist.png)

## Services

(TODO)

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

All components are decoupled and they do not know the location of other services. They are not even aware of their existence. Each service just responds to the events it receives and never directly contacts other services. Web services do NOT and should not call each other's API.

![eventbus]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/eventbus.png)

Figure X: Event Bus

### Event Store Implementation

(TODO)
C# .NET Core 

### Reverse Proxy Instance

The reverse proxy acts as a basic API gateway. I have dockerized and configured a Nginx server and placed it inside the microservice network habitat. Here is how it works.

![reverseproxy]({{ site.url }}{{ site.baseurl }}/assets/images/microservices/reverseproxy.png)
Figure X: Classic Database Entities

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

(TODO)

## Conclusion

(TODO)

## Technologies

(TODO)

## Links

(TODO)