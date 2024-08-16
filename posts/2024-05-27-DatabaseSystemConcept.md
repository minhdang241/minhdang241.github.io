---
categories:
- BOOKS
date: '2024-05-25'
description: My journey of learning Database System Concept
image: images/DBSC.jpg
layout: post
title: Database System Concept
toc: true
draft: true

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

# Chapter 12
## Physical Storage System
- volatile vs non-volatile storage.
- factor affects choosing storage:
    - access speed.
    - cost.
    - reliability.

- <b>primary storage</b>: cache/main-memory > <b>secondary storage</b>: flask storage, magnetic disk > <b>tertiary storage</b>: magnetic tape.

> Disk Controller: interface between the computer system and the disk drive hardware.

## Performance measure of disks
- Access time: time it takes from when a read/write request is issued to when data transfer begins.
    - Seek time: time it takes to reposition the arm over the correct track.
    - Rotational latency: time it takes for the requested sector to appear under the head.

- Data-transfer rate: the rate at which data can be retrieved from / stored to the disk.
- Disk block: a logical unit for strage allocation and retrieval.
- Sequential access pattern: 
    - Successive requests are for successive disk blocks.
    - Disk seek required only for first block.

- Random access pattern:
    - Successive requests are for blocks that can be anywhere on disk.
    - Each access requires a seek.
    - Transfer rates are low since a lot of time is wasted in seeks.
- I/O operations per second: no random block reads that a disk can support per second.
- Mean time to failure (MTTF): the average time the disk is expected to run continuously without any failure.

## RAID: Redundant Arrays of Independent Disks.
- Disk organization techniques that manage a large numbers of disks, providing a view of a single disk.
- The failure rate of a specific single disk is higher than one of some disk out of set of N disks.
=> Technique for using redundancy to avoid data loss are critical with large number of disks.
RAID: Software RAID vs Hardware RAID

## Optimization of Disk-Block Access
- Buffering: in-memory buffer to cache disk blocks.
- Read-ahead: Read extra blocks from a track in anticipation that they will be requested soon.
- Disk-arm-scheduling: re-order block requests so that disk arm movement is minimized.

# Chapter 13
> Database is stored as a collection of files. Each file is a sequence of records. A record is a sequence of fields.
> 
## Page
There are three notions of pages in DBMS:
- Hardware page (4KB): is the largest block that the disk can assure an atomic write.
- OS page (4KB)
- DBMS page (512B-32KB): is the smallest unit of data storage and transfer between disk and memory.

## Fixed-length records
Assumption:
- Record size is fixed.
- Each file has records of one particular type only.
- Different files are used for different relations.

## Variable-length records
- Record includes variable field(s) (varchar) or repetitive field(s).
- File includes different kinds of records.

Glossary:
- File == Table, which contains multiple pages.
- Page == smallest unit of I/O operation between the DB and disk, which contains multiple disk blocks.
  - It can contains tuples, meta-data, indexes, log records, etc
  - Most systems do not mix page types.
  - Some systems require a page to be self-contained.
- Organization vs Structure: 
    - Organization: high-level approach for arranging records.
    - Structure: internal layout of a specific page.
> Slotted Page Structure is a Data Structure that is used to manage records in a single file.







