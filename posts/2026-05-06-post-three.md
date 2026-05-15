---
title: Developing the database for Critique Canvas
date: 2026-05-06
author: Hung Nguyen
summary: Explaining the two tables of our database, the functionality of each field, and the lessons learned from linking them to the application
tags: [database, design, sql, models]
---

## The Database

The database is crucial because it is the data storage for the Critique Canvas. A poorly developed data storage system will result in annotations not being saved properly, images not being showcased, and comments not being saved. This post is dedicated to delivering an explanation of the two tables of the concept, the functionality of each field, the reasoning behind it and what we have learned from linking this feature to other applications.

## The Two Tables

There are two tables in the database, which are artworks and annotations. The artwork table saved the information of the uploaded images. This includes the image's title, description, filename, user ID, etc. The annotation table will store the data of the pin in the artwork, such as the person who added the annotation, the comment itself, the category of the comment, where the pin is, etc.

For each table, there will be a unique ID to distinguish them without confusion. The tables are linked together through a foreign key, by referencing the ID of the image to the pins that belong to it.

## Why Two Tables and Not One

One artwork can consist of multiple annotations. Storing this data in a single table will result in having to repeat the information of the artwork, which includes title, filename and username every time an annotation is added. This costs a lot of space in the storage and also creates an issue where all the rows will have to be updated if, for example, the title of the artwork changes.

By separating them into two tables and linking them with a foreign key, we follow a standard database principle called normalisation. Each piece of information is stored exactly once and referenced wherever it is needed. This makes the database smaller, more reliable, and easier to update.

## The x and y Fields

The most crucial consensus of our team is to store x and y values and real numbers, ranging from 0 to 100, instead of pixel integers. This decision was linked to a technical issue that was stated in the previous blog, where the pin will be misplaced if stored in pixel integers, since multiple screens can have distinct sizes. Storing in percentages will ensure that the pin will be in the exact location of the image on different platforms. The numbers will be changed back to pixels by CSS when it needs to be showcased.

## Separating Models From Controllers

Instead of combining the database and page codes, we separated the files for database work. This stage is called separation of concerns, since each file will have its distinct function without any concern for another task, which will enable easier maintenance and debugging, as well as make the code more neat and presentable.

## What Building the Database Taught Us

Developing the database before writing the controllers requires our team to focus on brainstorming what functions are actually necessary for the application. An example scenario is where the field username is in both tables, so that the gallery and annotation list can showcase the uploader and commenter without having to search the database each time. These minor details can improve the efficiency of the whole system.
