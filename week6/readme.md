# Week 6

## Table of Contents
  - link 1
  - link 2
  - link 3
  - link 4

## Objectives
- 


### MongoDB

Whether your a back-end or full-stack development you'll need to know your way around a database.
MongoDB is a popular NoSQL document-oriented database management system. 

NoSQL, or "not only SQL", databases are fast and flexible, scalable, and quick to get started with. 
They're becoming increasingly popular in the industry, currently MongoDB is used by companies like Google, Verizon, eBay, and many more.

**Database**

In software engineering, databases are systems that store, modify, and access collections of information electronically.

Almost any type of software application needs a way of storing data. Whether it is user account information for social media websites like Twitter or entire user-generated videos on websites like YouTube, databases are a crucial component of any application we use today. 
Without them, developers would have no way to meaningfully persist information that is important for the application’s functionality

Suppose a database is a bucket that stores our data. In that case, a DBMS is the software that encapsulates said bucket (our database(s)) and lets us work with the database using a programming language or easy-to-use graphical interface (GUI).

One of the most common classes of databases is a relational database. Relational databases, commonly referred to as SQL databases, structure data in tabular form. 
This means data is grouped into tables, using rows and columns to organize and store individual records of data. 

Relational databases are unique because they are based on presenting data in terms of relationships. To accomplish this, relational databases enable associations between tables to be defined, the most common associations being “one-to-one”, “one-to-many”, and “many-to-many”

Properties of a relational database:

  - Pre-defined Schema: Relational databases are unique because the database schema - the “blueprint” of the database structure - is typically determined before any data is ever inserted. This would mean we would likely decide the specific tables, and their associated relationships, before even inserting any data into them.
  - SQL Use: Developers communicate with a relational database using SQL (Structured Query Language). SQL is an industry-standard database language that has been used for decades. Extensive documentation and readable syntax make it approachable for beginners. The dependence of relational databases on SQL is why some developers and documentation sometimes refer to relational databases as SQL databases.
  - Relational Database Management System: Any relational database is managed by a relational database management system (RDBMS). This type of DBMS allows the data to follow a relational model (e.g., setup relationships) and manage the data using SQL. Two of the most popular RDBMSs are PostgreSQL and MySQL.
  - Unique Disadvantages: At the enterprise level, where data sets are massive, setting up a relational database can be costly, and the expenses required to maintain and scale it can compound significantly over time. Furthermore, rows and columns consume a great deal of physical space which can lead to implications for performance and cost.

The second most common class of databases is non-relational databases. A non-relational database, commonly referred to as a NoSQL database, is any database that does not follow the relational model. This means these types of databases typically don’t store data in tables, but more importantly, data isn’t strictly represented with relationships. Under the umbrella of non-relational databases are many different types of databases, each with its own framework for organizing data. Some examples are document databases, graph databases, and key-value databases. Collectively, non-relational databases specialize in storing unstructured data that doesn’t fit neatly into rows and columns.

Properties of a non-relational database:

- Flexibility and Scalability: Non-relational database’s unstructured nature facilitates the design of flexible schemas (schemas that do not need to be defined beforehand) and makes these types of databases highly adaptable to the changing needs of an application. Additionally, non-relational databases are well suited for expansion or scalability and are relatively inexpensive to maintain compared to relational databases.
- Custom Query Language: Unlike relational databases that all use SQL as a standard query language, most NoSQL databases have their own custom language.
- Unique Disadvantages: Since the data in non-relational databases is mainly unstructured, data can often become hard to maintain and keep track of. Additionally, since every NoSQL database uses its own custom query language, there is a new learning curve for each one we choose to work with.

### NoSQL

it’s important to note that relational and non-relational (NoSQL) databases each offer distinct advantages and disadvantages. 

 -  Scalability: NoSQL was designed with scalability as a priority. NoSQL can be an excellent choice for massive datasets that need to be distributed across multiple servers and locations.
 - Flexibility: Unlike a relational database, NoSQL databases don’t require a schema. This means that NoSQL can handle unstructured or semi-structured data in different formats.
 - Data Integrity: Relational databases are typically ACID compliant, ensuring high data integrity. NoSQL databases follow BASE principles (basic availability, soft state, and eventual consistency) and can often sacrifice integrity for increased data distribution and availability. However, some NoSQL databases do offer ACID compliance.
 
 There are four common types of NoSQL databases that store data in slightly different ways. Each type will provide distinct advantages and disadvantages depending on the datas

 - Key-Value - A key-value database consists of individual records organized via key-value pairs. In this model, keys and values can be any type of data, ranging from numbers to complex objects. However, keys must be unique. 
 - Document - A document-based (also called document-oriented) database consists of data stored in hierarchical structures. Some supported document formats include JSON, BSON, XML, and YAML. The document-based model is considered an extension of the key-value database and provides querying capabilities not solely based on unique keys
 - Graph - A graph database stores data using a graph structure. In a graph structure, data is stored in individual nodes (also called vertices) and establishes relationships via edges (also called links or lines). The advantage of the relationships built using a graph database as opposed to a relational database is that they are much simpler to set up, manage, and query
 - Column Oriented - A column-oriented NoSQL database stores data similar to a relational database. However, instead of storing data as rows, it is stored as columns. Column-oriented databases aim to provide faster read speeds by being able to quickly aggregate data for a specific column

 ### MongoDB

 First released in 2009 and updated regularly with new releases, MongoDB is a database system that allows users to store data using the document model.

 The document model is a term used to describe a database that primarily stores data in documents and collections. The data stored inside documents is typically stored in hierarchical structures like JSON, BSON, and YAML.

 MongoDB Atlas is MongoDB’s multi-cloud database service. Atlas allows developers to create, manage, and deploy MongoDB databases with just a few clicks. All of the databases are stored in the cloud, and Atlas does not require developers to have MongoDB set up on their computers to use it. Developers can interface with a database using an online dashboard.

 **Collections and Documents**

 Recall that MongoDB uses the document model. This means that data stored in a MongoDB database resides in a document within a collection. But what does that actually look like? To help better visualize the document model, let’s imagine we are using MongoDB to run our camera store. Naturally, we need to keep track of purchases, our customers, etc. Let’s break down each layer of the store’s database.

At the highest level, we have our database – an instance of MongoDB that contains all the various data our store needs to operate.

Within this instance of MongoDB are collections. Collections are subsets of our data. So, assuming our database contains three types of data – purchase data, inventory, and customer info – each of these would have its own collection.

Within each collection, we store individual records called documents. These documents all belong to that particular subset of our data. So, for example, within the customer collection, we could store personal information about each of our customers. Every customer would have their own document within the collection.

To summarize, a document is simply a record that stores information about a particular entity. A collection, in turn, is just a group of documents containing similar information. And finally, a MongoDB database is just a number of collections assembled together to store data for a specific use case – in this case running our camera store.

**BSON**

Binary Javascript Object Notation, or BSON (bee-sahn), is the format that MongoDB uses to store data. BSON is different than JSON in three fundamental ways:\

 - BSON is not human-readable.
 - BSON is far more efficient storage-wise.
 -  BSON supports a number of data formats that JSON does not - like dates.

 While it may not be legible, MongoDB wrote the BSON specification and invented the format to bridge the gap between JSON’s flexibility and readability and the required performance of a large database. MongoDB stores data as BSON internally but allows users to create and manipulate database data as JSON

 https://www.mongodb.com/json-and-bson
 https://www.mongodb.com/basics/bson

 **Data Modeling**

 When working with databases, we typically are given a blank slate with regards to what data we want to store. However, one of our most important decisions is not just the data we store but also how the data will be organized. Recall that with MongoDB, as opposed to a relational database, we have the flexibility to organize our data without the constraint of a predefined schema. The absence of a schema-based structure means we will have to think even more deeply about how we will structure our data. This concept is called data modeling.

Data modeling as a practice is the process of developing and choosing a way to structure our data and its relationships. The way we choose to organize our data has long-lasting implications on a database’s scalability, maintainability, and performance.

In addition to deciding the overall structure of our collections, another consideration is how to represent relationships between data. First, let’s think about why relationships between data are important. Take the example of a database that stores data about cars.

A document containing information about a car will likely have information like the color and size, which are attributes of the car itself. However, it may also contain information about the car’s engine.

The engine, being its own entity, possesses attributes separate from the overall car. If we want to store data about how powerful the engine is, it wouldn’t then seem quite right to make engine_power an attribute of the car since it is an attribute of the engine instead. In addition, we would have to ponder what the relationship between the car data and the engine are in the context of our whole database. We might ask, “Is the engine being shared amongst other cars in our database, or does it belong to only a single car?”

Our data modeling challenge would be to decide how best to represent the engine as a separate entity, its relationship to the car, and it’s relationship across the collection. To establish these types of relationships in MongoDB, we have two options: embedded documents or references. Let’s explore each of these options!

Embedded documents
One way to establish a relationship in MongoDB is to use embedded documents. This method allows us to nest data related to a document directly inside of it! These nested documents are called sub-documents. We already saw an example of this style when we looked at Model B of the photography database (feel free to pause and take a look again). Here is our car and engine example, modeled with an embedded document:

// Car Document
{
  car_id: 48273
  model_name: "Corvette",
  engine: {
    engine_power: 490,
    engine_type: "V8",
    acceleration: "High"
  }
}

In the above example, notice how the engine data is nested inside the car document. This type of data model where we find related data lumped together into a single collection is known as a denormalized data model.

Additionally, the following scenarios are good use-cases for embedded documents:

Modeling relationships where one entity contains another, also known as a one-to-one relationship. For example, we can think of a database storing data with a relationship between a car and its unique license plate. Each record of a car has only one license plate.

Modeling relationships that map one entity to many sub-entities, also known as an one-to-many relationship. For example, we can think of a database storing data with a relationship between a car owner and their multiple-owned cars. Each record of a car owner can own multiple instances of a car.

References
In addition to embedded documents, we can define relationships by creating links between data. These links are called references. Using references, we can split our data into multiple documents and maintain their relationships. 

//Car Document
{
  car_id: 48273
  model_name: "Corvette",
  engine_id: 2165
}
 
// Engine Document
{
  id: 2165
  engine_power: 490,
  engine_type: "V8",
  acceleration: "High"
}

In the above illustration, notice how the engine data is in a separate collection but is linked (via engine_id) into the car collection. This type of data model where we find related data via a link is known as a normalized data model and typically mimics how a relational database creates relationships between data.

Additionally, the following scenario is a good use case for references:

Modeling relationships where many instances of one entity can be mapped to many instances of another entity, also known as many-to-many relationships. For example, we can think of the relationship between car rentals and individuals renting the cars. A car can be rented by multiple individuals, and an individual can rent multiple cars.

https://www.mongodb.com/docs/manual/core/data-modeling-introduction/

https://www.mongodb.com/docs/manual/tutorial/model-embedded-one-to-one-relationships-between-documents/

https://www.mongodb.com/docs/manual/tutorial/model-embedded-one-to-many-relationships-between-documents/

https://www.mongodb.com/docs/manual/tutorial/model-referenced-one-to-many-relationships-between-documents/

https://learn.mongodb.com/courses/m320-mongodb-data-modeling

**MongoDB Atlas**

MongoDB Atlas is a developer data platform. It includes a suite of cloud databases and data services. For the purposes of working with databases, Atlas hosts a variety of features that help us quickly set up, deploy, and maintain a MongoDB database. Atlas allows us to store and manage our data in the cloud through an easy-to-use website interface. With Atlas, we can have a MongoDB database set up and running in just a few clicks!

On top of simply storing our data, Atlas offers several different integrated features to help us make the most of our data. A few of these are:

Atlas Search - which allows for quick and easy text-based queries of data stored in the cloud.
Atlas Charts - provides data visualization, which is fully integrated with the data we store with Atlas.

Atlas Data Storage
In Atlas, we interact with our data in what is known as a cluster. We can think of clusters as a unit of storage that MongoDB uses to house data. Depending on the plan we choose for our account, we can end up using clusters in a slightly different way.

https://www.mongodb.com/basics/clusters




