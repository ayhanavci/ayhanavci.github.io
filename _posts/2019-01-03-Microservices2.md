---
title: "Microservices, a prototype from scratch Part 2"
excerpt_separator: "<!--more-->"
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

This is Part 2 of the Microservices article which explains the source code. [The theoric Part 1 is here]({{ site.baseurl }}{% link _posts/2019-01-03-Microservices.md %})

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
|As a customer, I want to view my profile information so that I make adjustments. |View user information and credit<br>Edit name, password, email|
|As a product manager, I want to add/edit/delete products so that customers can buy them|View/Add/Edit/Delete products. Names, prices, suppliers, units in stock |
|As a product manager, I want to view all customer order states so that I can assist if needed|View all orders and their states history in the system|
|As a product manager, I want to edit customer credit so that they can purchase products|View all users and their credit<br>Edit credits|

## The Design

(TODO)

## Communication Model

(TODO)

## Event Bus Models

(TODO)

## Docker Containers

(TODO)

### Docker Networks

(TODO)

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

(TODO)
RabbitMQ

### Event Store Implementation

(TODO)
C# .NET Core 

### Reverse Proxy Instance

(TODO)
Nginx

## Databases

### Event Store Database

(TODO)
MongoDB

### Customer Database

(TODO)
Redis

### Product Database

(TODO)
SQLite

### Order Database

(TODO)
PostgreSQL

### Accounting Database

(TODO)
CouchDB

## Consumers

(TODO)

### ECommerce Website

(TODO)
Python + Flask

### Management Website

(TODO)
Python + Flask

### ECommerce Native Android App

(TODO)
Java - Native Android

## The Implementation

(TODO)

## Putting it together

(TODO)

## Technologies

(TODO)

## Links

(TODO)

This is Part 2 of the Microservices article which explains the source code. [The theoric Part 1 is here]({{ site.baseurl }}{% link _posts/2019-01-03-Microservices.md %})