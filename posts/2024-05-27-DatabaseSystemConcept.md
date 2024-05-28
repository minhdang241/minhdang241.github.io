---
categories:
- BOOKS
date: '2024-05-25'
description: My journey of learning Database System Concept
image: images/CPP-Language.png
layout: post
title: Database System Concept
toc: true
draft: false

---

How to read the book?
- Read the slides first, only read the book when things become abstruse.

# Introduction
> Def: Database management system = collection of data (database) + a set of programs to access database.

Databases have 2 modes:
- Online transaction processing: retrive/modify small data.
- Data analytics: process data to draw conclusions.

> Def: Data model is a collection of tools to describe data.
Data models can be classified into 4 catagories:
- <b>Relational Model</b> -> foundation for most database applications.
- Entity-Relationship Model
- Semi-structured Data Model
- Object-based Data Model

> Def: Instance is the collection of data in database at a particular moment.
> Def: Schema is the design of the database. Physical schema describes database at physical level while Logical schema describes database at logical level. -> Logical schemas are important to Application Developer since they construct those schema.

> Def: use Data Definition Language (DDL) to specify the schema. 
> Def: use Data Manipulation Language (DML) to access/update data.

Open Database Connectivity (ODBC) standard defines the application interfaces 
- Java Database Connectivity (JDBC) defines the application interfaces in Java Programming Language.

Database Engine has multiple modules:
- Storage Manager: stores, retrives and updates data effectively.
- Query Processor: simplifies and facilitates access to data.
- Transaction Manager: ensures the consistency of the database.

# Relational Language
- Superkey is used to uniquely identify a row.
- Candidate key = minimal Superkey.
- Primary key = The chosen Candidate key.

# Physical Storage System
- volatile vs non-volatile storage.
- factor affects choosing storage:
    - access speed.
    - cost.
    - reliability.

- primary storage: cache/main-memory > secondary storage: magnetic disk > tertiary storage: magnetic tape.












































