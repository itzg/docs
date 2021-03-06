---
title: Java Development Overview
keywords: java
last_updated: March 2017
tags: [java]
summary: "Overview guide for Java developers."
sidebar: documentation_sidebar
permalink: /documentation/developing-with-java/java-setup.html
folder: documentation
---

## Basic Setup

This section will discuss how to start developing with Grakn using the Java API. 
All Grakn applications require the following Maven dependency: 

```xml
<dependency>
    <groupId>ai.grakn</groupId>
    <artifactId>grakn-graph</artifactId>
    <version>${project.version}</version>
    </dependency>
```
    
This dependency will give you access to the Core API as well as an in-memory graph, which serves as a toy graph, should you wish to use the stack without having to have an instance of the Grakn server running.

## Server Dependent Setup

If you require persistence and would like to access the entirety of the Grakn stack, then it is vital to have an instance of engine running.  
Please see the [Setup Guide](../get-started/setup-guide.html) on more details on how to set up a Grakn server.

Depending on the configuration of the Grakn server, your Java application will require one of the following dependencies. When your server is running against a Titan backend: 

```xml   
<dependency>
    <groupId>ai.grakn</groupId>
    <artifactId>titan-factory</artifactId>
    <version>${project.version}</version> 
</dependency>
```    

When your server is running against a OrientDB backend:

```xml
<dependency>
    <groupId>ai.grakn</groupId>
    <artifactId>orientdb-factory</artifactId>
    <version>${project.version}</version> 
</dependency>
```    
    

{% include note.html content="The distribution package comes with a Titan backend configured out of the box. OrientDB support is still in early stages of development. " %}


The JAR files you will need to develop with Grakn can be found inside the `lib` directory of the distribution zip file. All the JARs are provided with no dependencies, so using these requires you to use Maven to acquire dependencies.

Here are some links to guides for adding external jars using different IDEs:

- [IntelliJ](https://www.jetbrains.com/help/idea/2016.1/configuring-module-dependencies-and-libraries.html)
- [Eclipse](http://www.tutorialspoint.com/eclipse/eclipse_java_build_path.htm)
- [Netbeans](http://oopbook.com/java-classpath-2/classpath-in-netbeans/)


## Initialising a Transaction on The Graph

You can initialise an in memory graph without having the Grakn server running with:  

```java
GraknGraph graph = Grakn.session(Grakn.IN_MEMORY, "keyspace").open(GraknTxType.Write);
```    
    
If you are running the Grakn server locally then you can initialise a graph with:

```java    
GraknGraph graph = Grakn.factory(Grakn.DEFAULT_URI, "keyspace").getGraph();
```
    
If you are running the Grakn server remotely you must initialise the graph by providing the IP address of your server:

```java
GraknGraph graph = Grakn.session("127.6.21.2", "keyspace").open(GraknTxType.Write);
```
    
The string "keyspace" uniquely identifies the graph and allows you to create different graphs.  

Please note that graph keyspaces are **not** case sensitive so the following two graphs are actually the same graph:

```java
    GraknGraph graph1 = Grakn.session("127.6.21.2", "keyspace").open(GraknTxType.Write);
    GraknGraph graph2 = Grakn.session("127.6.21.2", "KeYsPaCe").open(GraknTxType.Write);
```
   
All graphs are also singletons specific to their keyspaces so be aware that in the following case:

```java
   GraknGraph graph1 = Grakn.session("127.6.21.2", "keyspace").open(GraknTxType.Write);
   GraknGraph graph2 = Grakn.session("127.6.21.2", "keyspace").open(GraknTxType.Write);
   GraknGraph graph3 = Grakn.session("127.6.21.2", "keyspace").open(GraknTxType.Write);
```
  
any changes to `graph1`, `graph2`, or `graph3` will all be persisted to the same graph.

## Controlling The Behaviour of Graph Transactions
  
When initialising a transaction on a graph it is possible to define the type of transaction with `GraknTxType`.      
We currently support three types of transactions:

* `GraknTxType.WRITE` - A transaction that allows mutations to be performed on the graph
* `GraknTxType.READ` - Prohibits any mutations to be performed to the graph 
* `GraknTxType.BATCH` - Allows faster mutations to be performed to the graph at the cost of switching off some internal consistency checks. This option should only be used if you are certain that you are loading a clean dataset. 

## Where Next?

The pages in this section of the documentation cover some of the public APIs available to Java developers:

* [Graph API](./graph-api.html)
* [Java Graql](./java-graql.html)
* [Migration API](./migration-api.html)
* [Loader API](./loader-api.html)

There is also a page (in progress) that discusses advanced topics in Java development, such as transactions and multi-threading.

There is an example described in our [blog](https://blog.grakn.ai/working-with-grakn-ai-using-java-5f13f24f1269#.8df3991rw) that discusses how to get set up to develop using Java, and how to work with the Graph API and Java Graql.

## Comments
Want to leave a comment? Visit <a href="https://github.com/graknlabs/docs/issues/23" target="_blank">the issues on Github for this page</a> (you'll need a GitHub account). You are also welcome to contribute to our documentation directly via the "Edit me" button at the top of the page.


{% include links.html %}

