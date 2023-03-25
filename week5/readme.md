# Week 5

## Table of Contents
  - link 1
  - link 2
  - link 3
  - link 4

## Objectives
- Databases
- MongoDB
- Mongoose


## Databases

Databases let us work with large amounts of data efficiently. They make updating data easy and reliable, and they help to ensure accuracy. 
They offer security features to control access to information, and they help us avoid redundancy.

[NoSQL databases](https://www.mongodb.com/nosql-explained) are a type of database that stores data in a format other than the traditional tabular format of relational databases.

### Brief History

NoSQL databases emerged in the late 2000s as the cost of storage dramatically decreased. Gone were the days of needing to create a complex, difficult-to-manage data model in order to avoid data duplication. Developers (rather than storage) were becoming the primary cost of software development, so NoSQL databases optimized for developer productivity.

Additionally, the Agile Manifesto was rising in popularity, and software engineers were rethinking the way they developed software. They were recognizing the need to rapidly adapt to changing requirements. They needed the ability to iterate quickly and make changes throughout their software stack — all the way down to the database. NoSQL databases gave them this flexibility.

The [relational DB model](https://www.ibm.com/topics/relational-databases) is a table-based model. Each table has a fixed number of columns, and each row has a value for each column. The columns are defined in the table schema, and the rows are the data. The data is stored in a tabular format, with each row representing a single record. The columns are related to each other through a primary key.
This model can be rigid and inflexible. It requires a lot of planning and forethought to create a schema that will work for all the data you want to store. It also requires a lot of work to update the schema if you need to add or remove columns. If you need to add a new column, you have to update the schema for every table that uses it. If you need to remove a column, you have to update the schema for every table that uses it, and you have to update every row in the table to remove the data from that column.

Cloud computing also rose in popularity, and developers began using public clouds to host their applications and data. They wanted the ability to distribute data across multiple servers and regions to make their applications resilient, to scale out instead of scale up, and to intelligently geo-place their data. Some NoSQL databases like MongoDB provide these capabilities.




## MongoDB