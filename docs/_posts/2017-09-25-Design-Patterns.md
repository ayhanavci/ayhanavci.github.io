---
title: "Design Patterns"
excerpt_separator: "<!--more-->"
excerpt: "Gang of Four Design Patterns With World of Warcraft Examples"
description: "Gang of Four Design Patterns With World of Warcraft Examples"
header:
    og_image: /assets/images/simplicity1.png
categories:
  - Concepts
tags:
  - C++
  - Visual Studio Code
  - CMake
---
{% include toc title="Contents" icon="list-ul" %}

## Introduction

Design Patterns - With World of Warcraft examples

I have been wanting to refresh my memory of classic design patterns (gang of four) and i had some free time at hand. So i decided to read them again and code some samples of my own. While at it, i thought why not give a ride to some recent c++ features, visual studio code and cmake too ? If you add being one of the oldschool world of warcraft players on top of having time and ambition, it was clear what i was going to do and this small piece of code happened. 

If you wish to learn about design patterns, there are far simpler, cleaner and straightforward examples out there on the internet. This is not one of those. What i did here is using a lot of world of warcraft metaphors for implementing design patterns. 
I implemented these patterns just for having a bit of fun and for no serious purpose but if someone reads and enjoys them too, suit yourselves.

## Structure

* Every design pattern tells a story about how (part of) World of Warcraft game could be explained.
* There are three main categories of patterns. Creational, Structural and Behavioral.
* Every individual pattern is listed under the related category.
* No file in any pattern has external dependencies. Patterns don't try to reuse each others code. You can just cut a folder and it should work alone.
* Every individual pattern has its own namespace.
* Every design patter has [PatternName]Test.h and [PatternName]Test.cpp files with only one void [PatternName]Test() function in it. [PatternName]Test.h files are included in parent directory Testers.h, which is included in main.cpp
* There are three libraries, one for each pattern category. I opted to create objects for each pattern and then combining these objects in their related category library.
* I used a lazy version of hungarian notation.
* I tried to keep names of the files, variables, class names and functions clear.
* Except a few, every class has its own file.

## Prerequisites

* CMake
* Any modern `C++ (14)` compiler. No external dependencies or libraries.

## Installing

I started with Visual Studio 2017 and then switched to Visual Studio Code 1.16. Since Visual Studio now supports CMake, it should compile this out of the box. For VS Code, you need to setup extensions and json files.

To work with Visual Studio Code

Install;

* [C/C++ for VS Code (Preview)](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) - C++ extension by microsoft
* [CMake For VisualStudio Code](https://marketplace.visualstudio.com/items?itemName=twxs.cmake) - CMake Support extension
* [CMake Tools](https://marketplace.visualstudio.com/items?itemName=vector-of-bool.cmake-tools) - Fancy CMake tools extension

Make sure these are setup properly
* Create a `c_cpp_properties.json` file with appropriate include paths.
* Setup a `launch.json` file like provided in `.vscode` directory. 
* settings.json and `.cmaketools.json` should get automatically created.

## Running the tests

Root directory contains a `main.cpp` file. Just uncomment the related line to test the pattern. 

## Source Code

[Source on Github](https://github.com/ayhanavci/DesignPatterns)

## License

This project is licensed under the MIT License

## History

Versions

### 1.0

* Implemented the following patterns
    * Behavioral
        * Chain Of Responsibility
        * Command
        * Interpreter
        * Iterator
        * Mediator
        * Memento
        * Observer
        * State
        * Strategy
        * Template Method
        * Visitor
    * Structural
        * Adapter
        * Bridge
        * Composite
        * Decorator
        * Facade
        * Flyweight
        * Proxy
    * Creational
        * Builder
        * Factory
        * Prototype
        * Singleton
    
## To Do

* Add Concurrency Patterns
* Add Architectural Patterns
* Add Uncategorized Patterns

## Stories

Chain of Responsibility

Various parties with different weaknesses enter a dungeon with multiple bosses. Each boss can wipe a certain party.
{: .notice}

Command

Player decides to modify his new armor pieces with various options of different professions.
{: .notice}

Interpreter

A translator learns various kalimdor languages and uses them to translate.
{: .notice}

Iterator

Raid leader checks out every single player in the raid.
{: .notice}

Mediator

The raid engages in fight. Raid leader tells everyone what to do when an event occurs.
{: .notice}

Memento

Transmogrifier screen.
{: .notice}

Observer

Boss fight ensues and Deadly Boss Mods notifies everyone when events happen.
{: .notice}

State

Warrior stance dance.
{: .notice}

Strategy

How different classes kill mobs.
{: .notice}

Template Method

Similar and differentiating things while leveling professions.
{: .notice}

Visitor

Players visit fruit and armor vendors.
{: .notice}

Adapter

You think only healers can heal ? Rogue's can heal too.
{: .notice}

Bridge

How Druids, Monks and Paladins fullfill all three roles.
{: .notice}

Composite

Parties and raids are both player groups. Raids can also be defined with multiple parties.
{: .notice}

Decorator

An armor piece can be modified to have multiple properties.
{: .notice}

Facade

Would be nice if a giant macro button could kill every boss.
{: .notice}

Flyweight

When a new character is created, it has the base same base stats with all other new characters of the same race.
{: .notice}

Proxy

Stopcasting healing macro button replaces normal healing button.
{: .notice}

Builder

A spawn point which builds various classes.
{: .notice}

Factory

Client asks "looking for group tool" to create a party for them. Client can ask for a stealth or a plate team.
{: .notice}

Prototype

Client asks "looking for group" tool for a copy of a player class.
{: .notice}

Singleton

Druid is the ultimate answer to all questions.
{: .notice}

## Acknowledgments

World of Warcraft, Warcraft and Blizzard Entertainment are trademarks or registered trademarks of Blizzard Entertainment, Inc. in the U.S. and/or other countries.

Based on patterns in the book; 
"Design Patterns: Elements of Reusable Object-Oriented Software Book by Erich Gamma, John Vlissides, Ralph Johnson, and Richard Helm"