---
layout: post
title:  "Database Update"
date:   2026-11-22 01:18:00 -0600
categories: UML
---

## Developing a Databse

Having a completely selfhosted database is difficult because sending and receiving packages directly between two different machines requires careful planning. The database server must be reachable by the web application, but it should not be exposed to every network or unauthorized user. Proper authentication, encrypted connections, firewall rules, and regular security updates all need to be considered.

The next step is to define how the application and database communicate. This includes choosing a database system, setting up its network configuration, and deciding how data will be validated before it is stored or retrieved. I will need to test not only basic reads and writes, but also connection failures, incorrect credentials, and unexpected input. Building these checks into the application early should make the database more secure and reliable as the web app continues to develop.
