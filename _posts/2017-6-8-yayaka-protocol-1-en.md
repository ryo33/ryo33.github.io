---
layout: post
title: Yayaka Protocol and Highly Distributed Social Blogging
---

[日本語で読む]({% post_url 2017-6-8-yayaka-protocol-1-ja %})

Yayaka Protocol is a yet another protocol for distributed social blogging.
I explain "Why I create", "Why we love", and "Why you will love" in this article.

## Purposes

Yayaka Protocol has following purposes roughly:

- Decentralization
- Democratization
- Participation with minimum servers
- High extensibility with layered protocols

*Decentralization* is *the first step* to get our freedom.
Monopolized platform may take your freedom.

*Democratization* is *the second step* to do that.
It means Yayaka Protocol doesn't divide a monopoly service into many authoritarian services.

*Participation with minimum servers* means we don't have to implement all functions of distributed social blogging for each server.
Each server can have a minimum implementation.

*High extensibility with layered protocols* means we can extend our web services uniquely in cooperation with other services which have a common view.

## Features

Here are the features of Yayaka Protocol to perform the purposes.
In order to carry out the idea above, the following features will be implemented in the protocol.

- Easily migration between servers.
- Each server only has to implement a partial set of functions of distributed social blogging.
- Any way to connect two different hosts is allowed. (Socket, HTTPS with token, etc.)
- Additional profile attributes, type of social blogging events, and types of contents are specified in higher-level protocols.


## Concepts

### Yayaka Services

There are 4 yayaka services, identity service, repository service, social graph service, presentation service.
Each user registers themselves with identity services and authorizes other services to use.
Repository services store events like posts, favorites, reposts, etc.
Social graph services store users' relations and deliver events properly.
Presentation services provide interfaces to be used by users like front-end UI, API, etc.
Each user can have multiple repository services to post, social graph services to broadcast, and presentation services to use UIs which match for each purpose.
If a repository service censors unjustifiably or a presentation service regress its UI or API, each user can exit right away.


### Yayaka Meta/Messaging Protocol

Yayaka Meta/Messaging Protocol (YMP) is a messaging protocol for distributed Web applications.
Each server can implement multiple connection protocols and message protocols as higher-level protocols of YMP.
Connection protocols specify how to establish a connection between two different hosts and message protocols specify how the application works with messaging between services.
Yayaka Protocol is a message protocol of YMP (Yayaka Meta/Messaging Protocol).


## How Yayaka Protocol works

### Registration

1. A user chooses a server to register him/herself.
2. The user enters registration information on one of presentation services trusted by the server.
3. The presentation service sends a message to the identity service of the server.
4. The identity service creates a user.

### Following

1. A user enters who to follow on a presentation service.
2. The presentation service sends a message to one of social graph services authorized by the user.
3. The social graph service sends a message to target social graph.
4. Both two social graph services store the following relation.

### Creating an event

1. A user enters data to submit on a presentation service.
2. The presentation services send a message to one of repository services authorized by the user.
3. The repository service stores the data.
4. The repository service broadcasts the event to social graph services which subscribe the repository service.
5. The each social graph services broadcast the event to services which subscribe them.


## Present condition

Writing the YMP specification is almost finished.
Currently, I'm simultaneously writing Yayaka Protocol specification and a reference implementation with Elixir.


## Plans

Here are the plans of Yayaka Protocol and YMP:

- Create a first connection protocol
- Create a message protocol for distributed registry to register higher-layer protocols of YMP.
- Create a message protocol for distributed git hosting service.


## How to contribute to Yayaka Protocol

There is [an organization of Yayaka Protocol Suite](https://github.com/Yayaka).
Please feel free to open issues.
