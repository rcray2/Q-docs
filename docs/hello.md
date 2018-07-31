# Maana Q

#

#

#

#

#

# Knowledge Technology Development Guide



Table of Contents

Introduction        3

Welcome to Maana        3

Better and Faster        3

Scope        3

The Maana Platform        3

Microservices as Peers        3

The Computational Knowledge Graph™        4

Graph Concepts        4

1.1.        Maana platform architecture and GraphQL        6

1.2.        About Maana Kinds and other useful terms        7

1.3.        Knowledge Microservices        8

1.4.        Knowledge Applications        10

1.5.        MAANA catalogue        10

2.        Defining a model in Maana        11

3.        Hydrating the model with data        12

4.        Querying a model        15

4.1.        Query through service end points        15

4.2.        Query using global entry points        15

4.3.        Query using allinstances        16

4.4.        Query across kinds        17

5.        Developing knowledge microservices        18

5.1.        Project Setup        18

5.2.        Development        19

5.3.        Packaging        19

5.4.        Adding a knowledge microservice to the MAANA catalogue        19

6.        Developing knowledge applications        20

7.        Troubleshooting        20

8.        Troubleshooting        20





## Introduction

## Welcome to Maana

Maana was founded with the vision of using technology to systematize the world&#39;s industrial expertise and data into digital knowledge that could significantly advance the global economy.  Our mission is to facilitate the tens of millions of experts working in industrial companies around the world, and give them the ability to make better decisions, faster.  We are devoted to putting the power of artificial intelligence (AI) into the hands of millions of industry experts, offering enhanced decision-making tools in a rapid response environment.

## Better and Faster

In 2013 Maana invented a new way to represent industrial knowledge mathematically, using the Maana Patented Computational Knowledge Graph™.  This unique technology enables industrial companies to encode human expertise and data from across silos into digital knowledge, eliminating the need to move data and enabling the creation of thousands of models at scale, through the re-usability of models across the enterprise.

## Scope

This guide is intended for the use of Solution Developers and Data Scientists, and describes how to develop such services (aka &quot;bots&quot;) and applications and operationalize them in a production setting.

## The Maana Platform

The Maana platform is used by solutions teams to deliver knowledge applications to their end users (i.e., business users, SMEs, managers).  This &quot;solution team&quot; approach involves the collaboration of analysts, engineers, and data scientists.  The Maana platform is designed to build an enterprise-wide knowledge layer, utilizing search and exploration for solution development and delivery.  It is built from a network of GraphQL-based microservices.

## Microservices as Peers

Unlike pure client-server or n-tier architectures, Maana&#39;s microservices act as peers in an asynchronous and loosely-coupled arrangement that promotes independent scaling and extensibility.  They provide identity and access controls, graph persistence, search, machine learning, and natural language processing.  As a consequence, these peer microservices provide reasoning capabilities to Knowledge Applications, which help solve domain-specific problems and support optimal decision-making that is capable of learning over time.

## The Computational Knowledge Graph (CKG)

The Computational Knowledge Graph (itself a microservice), provides automatic persistence, boilerplate queries and mutations, and service orchestration capabilities.  Taken together, these services allow the solution developer to focus on designing GraphQL schemas and implementing computational resolvers only where needed.

### GraphQL

Maana uses GraphQL to represent and expose its Knowledge Graph.  GraphQL is a data query language created by Facebook and open-sourced in 2015 as an alternative to REST interfaces.

GraphQL provides a complete and understandable description of the data in the API, gives clients the power to ask for exactly what they need and nothing more, makes it easier to evolve APIs over time, and enables powerful developer tools.  With the GraphQL type system, the developers can access the full capabilities of your data from a single endpoint.

GraphQL creates a uniform API across your entire application without being limited by a specific storage engine.  The developer provides functions for each field in the type system, and GraphQL calls them with optimal concurrency.

#### Maana Platform Architecture and GraphQL

 
 
 
 
 
 
 
For more information on using on Graph QL, please refer to the link provided below:

- . [How to GraphQL](https://www.howtographql.com/)

## The Maana Solution Core

At the core of any Maana solution sits our Computational Knowledge Graph, a network of models built using machine learning techniques and artificial intelligence that powers AI-driven applications used to digitize decision support and operations.  It allows the knowledge of the business to be incrementally captured and grown, evolving and becoming increasingly sophisticated as more projects are developed.

### CKG Features

With the Computational Knowledge Graph:

- .The Graph is dynamic.  Nodes, which represent concepts in the graph, are not static containers, but rather act as computational vessels, allowing algorithms to be stored and executed.
- .Reusable Models - Knowledge Graph flexibility enables groups across the organization to leverage and build-upon models created by other groups, dramatically accelerating the speed at which models are created throughout the organization.  These models are dynamic, and once operationalized into applications, they learn and adapt based on the user behaviors and provide continuous intelligence for day-to-day operations.

- .The structure of data is separated from its content.  This separation enables a fluidity of modeling - data from any source or format can be seamlessly integrated, modeled, searched, analyzed, operationalized and re-purposed.

- .Data remains at the source - only the most relevant data, within the context of what is being optimized, is indexed and brought into the graph.
- .Each resulting Data Model is a unique combination of three key components, which are instrumental in optimizing assets and decision flows:

1. Subject Matter Expertise
2. Relevant Data from Silos
3. The Right Algorithm

**Note** :  These algorithms could be as simple as pulling in new data from an external source, to as complicated as classification of documents through machine learning.

- .As source data is updated in real time, so are the nodes and the computational models that act on that data, permitting more complex relationships to be modeled, and enhancing the graph&#39;s ability to understand the connections between concepts (rather than simple strings of data), and encouraging optimization of decisions and operations.

The CKG consists of **concepts** and **properties** , **instances** and **values** , and **relations** and **links**.

| Consider the concept of a _ContainerShip_with the properties of _name_, _length_, _position_, etc.  A specific instance (entity) for our ContainerShip might have values for each of these properties, such as the vessel Maersk Viking having a length of _400 meters_.  Such properties can be scalar (e.g., numbers, strings, dates) or might refer to other concepts/instances - such as the _Hold Cargo_ of the vessel. |
| --- |

In some cases, property values for an instance may simply be stored, as they are rarely subject to change.  In other cases, they will be dynamically computed, due to constantly changing values - such as a ship&#39;s _weight_ (which is dependent upon its cargo) or variables like the vessel&#39;s _current position_ (which requires getting a GPS reading).

Unlike a traditional graph database, Maana incorporates arbitrary computation (through custom GraphQL resolvers) and distributes the graph into subgraphs managed by different microservices, optionally with their own dedicated persistence mechanism.  This separation enables a fluidity of modeling, allowing data from any source and in any format to be seamlessly integrated, modeled, searched, analyzed, operationalized and re-purposed.  It also places more responsibility on the microservices to provide their own storage.

### Data Model Split

To address this, Maana proposes an explicit split between the data models (i.e., GraphQL type definitions) that a service uses and its operations (i.e., GraphQL resolvers).  Maana will generate the appropriate managed service for such models using KindDB, Prisma, neo4j, etc.  With this, the solution developer only provides the logic they care about and let the system take care of all the CRUD/ORM-like operations on the data.

## Useful Terms and Concepts

Prior to creating a working computational knowledge graph, it is suggested that you familiarize yourself with some basic terms and concepts that will help the make most out of the MAANA portal experience.  For a complete Glossary of Terms, please refer to the Appendix found at the back of this document.

| Term | Definition | Example |
| --- | --- | --- |
| Kinds | Kinds are concepts.   | People, Ships, Oil Wells, Invoices  |
| Fields | Fields are properties within a certain concept | For People:  age, sex, height, weight, etc. |
| Instances | A set of values for entities within a concept. | Paul, 40 yrs. old, male, 6&#39;, 180 pounds |
| Values | A particular size, measure, number of an entity. | Example of a Value:  40 yrs. old |
| Relations | Connections /dependencies that can be established between fields belonging to different Kinds | A Kind describing an oil well may contain a field showing the name of the company operating that well.  That field has a relation with the Kind containing Company Names. |
















  1.1.Knowledge Microservices

Knowledge microservices are a class of microservices that are developed for the MAANA platform.

Maana Knowledge Services form a network of GraphQL endpoints, exposing their types, queries, and mutations for direct access, as well as publishing and subscribing to network events.  The Maana platform manages these services, providing authentication, reliable messaging, (automatic) graph persistence (with search and querying), scaling, monitoring, and a rich UX.



Examples of knowledge microservices include:

| Indexers• Text• Number• Time• Geospatial• Geometric   | Miners• Statistics• Probabilities• NER/NLP  | Classifiers• Entity• Field• Document• Image  |
| --- | --- | --- |

Knowledge Services can become Knowledge &quot;Bots&quot; by offering and using &quot;bot actions,&quot; which allows the service to interact more directly to users.

Users can configure, start, stop, and schedule bot actions.  Services can report on status and progress for potentially long-running, asynchronous operations, as well as report any errors or messages. Bots are microservices that &quot;listen&quot; for specific events on a system bus – Maana uses RabbitMQ for system bus – and act automatically when such events happen to mine and enrich the graph (i.e. when a raw data file is loaded into MAANA, a bot automatically analyzes it to identify mentions of entities like persons, phone numbers, values, and facts).

There are two primary scenarios to consider here: event handling and direct query/mutation.

1. Event Handling

When a Knowledge Service subscribes to and handles an event, such as &quot;fileAdded,&quot; it can (optionally) create an instance of a BotAction Kind by mutating the CKG.  As the service performs its operation, it can periodically update the progress (if it is deterministic) and update the status and report errors.

1. Queries and Mutations

A user can invoke a query or mutation either explicitly (e.g., train a classifier on labeled data) or implicitly (i.e., as part of query graph).  In such cases, the service exposes such queries and mutations as returning a BotAction.

1.
  1. 1.2.Knowledge Applications

Knowledge Applications are applications that are built on top of the Maana CKG to help subject matter experts make better decisions faster.

A Knowledge application presents a custom interface to the end user and optimizes interaction for most common actions.

Generally a knowledge application generally provides the following functionality:

- .Allow user to specify question inputs
- .Generate recommendations
- .Visualize recommendations and rationale
- .Allow user to explore and tune inputs
- .Accept user decision

Knowledge applications can be developed in any language. Most languages have some level of GraphQL support.

1.
  1. 1.3.MAANA catalogue

Maana catalogue is the repository for all components that can be used as part of MAANA knowledge solutions

- .How do you add/publish/update/delete components in the catalogue?



1.
## 2.Defining a model in Maana

All of the following documentation uses XXX.XXX.XXX.XXX facing to represent the public URL of the system. All of the following examples use the Graphiql endpoint which is interactive, programmatic access conducted through the Graphql endpoint.

GraphQL API is organized around three main building blocks: the schema, queries, and resolvers.

Queries can be executed via GET or POST, with either the body of the query as the query string or in the body of the POST. All mutations must use the POST endpoint and have the header content type set to application/json. Also, the GraphQL server needs a resolver to know what to do with an incoming query.

 
 
 
 
 

        Anatomy of a GraphQL Query ( [_source_](https://dev-blog.apollodata.com/the-anatomy-of-a-graphql-query-6dffa9e9e747))



The following is an example of how to create a graph with no mutation specified, using the cURL command line tool for getting or sending files using URL syntax.

curl -v -H &quot;Content-Type: application/json&quot; -X POST -d &quot;{\&quot;query\&quot;: \&quot;mutation { ..... }\&quot;}&quot; http://XXX.XXX.XXX.XXX:8003/graphql

Maana engineering team is currently implementing authentication functionality, which will be required for all clients.



- .How to create a graph from a GQL definition ( [https://confluence.corp.maana.io/display/RD/Maana+Q+programmatic+interface](https://confluence.corp.maana.io/display/RD/Maana+Q+programmatic+interface))
- .How to add instance data
- .How to query instance data
- .How to perform complex queries ( [https://confluence.corp.maana.io/display/RD/Technical+Design+Notes#TechnicalDesignNotes-Queries](https://confluence.corp.maana.io/display/RD/Technical+Design+Notes#TechnicalDesignNotes-Queries))
- .How to perform search ( [https://confluence.corp.maana.io/display/RD/Maana+Q+Search+-+Technical+Design](https://confluence.corp.maana.io/display/RD/Maana+Q+Search+-+Technical+Design))



1.
## 3.Hydrating the model with data

Steps to hydrate a model with data:

1. **Create a service source with graphql schema**. Types must correspond to Kinds on the portal endpoint XXX.XXX.XXX.XXX:8003/graphiql. Note that the service can have many Kind definitions.


mutation {

  addServiceSource(input: {

    name: &quot;TestService&quot;,

    schema: &quot;type TestKind { \n id: ID \n name: String! \n something: Int! }&quot;

  })

}

Creating a Service Source will return a Service ID:



{

  &quot;data&quot;: {

    &quot;addServiceSource&quot;: &quot;54769aae-2c63-4415-9b34-74e9ceb1d789&quot;

  }

}



This ID is used to establish the Service endpoint for subsequent interactions

XXX.XXX.XXX.XXX:8003/service/54769aae-2c63-4415-9b34-74e9ceb1d789/graphiql

Note: If the service is re-created by updating definitions, its service ID will change, while the old service will still be available.

1. Adding a single instance of the kind

mutation {

  addTestKind(input: {

    name: &quot;testname&quot;,

    something: 42

  })

}



An ID is assigned and returned for the instance itself



{

  &quot;data&quot;: {

    &quot;addTestKind&quot;: &quot;431f9338-95be-4f89-968c-4f2bb197965c&quot;

  }

}



Once the instance data was added, we can query it using:

query {

  allTestKinds {

    id

    name

    something

  }

}



1. Adding multiple data instances simultaneously to a kind

mutation addSomethings {

  addTestKinds(input: [{

    name: &quot;testname2&quot;,

    something: 45

  }, {

    name: &quot;testname3&quot;,

    something: 43

  }

  ])

}

Larger data volumes should be broken into reasonably sized chunks (i.e. up to 5,000 instances), and submitted sequentially through the endpoint.

Data can be queried using:

query {

  allTestKinds {

    id

    name

    something

  }

}





1.
## 4.Querying a model

1.
  1. 4.1.Query through service end points

This is the most common way to query the data once ingested in the graph



query {

  allTestKinds {

    id

    name

    something

  }

}



1.
  1. 4.2.Query using global entry points

There are more generic ways to read Kinds from a model than through service entry points.

However when using the global entry points (rather than the service entry points), Kinds should be referenced by ID. The IDs for each kind can be obtained from the Service ID by running a query against XXX.XXX.XXX.XXX:8003

Here is an example on how to obtain a Kind IDs from the Service ID:

query foo {

  serviceSource(id:&quot;54769aae-2c63-4415-9b34-74e9ceb1d789&quot;) {

    name

    kinds {

      name

      id

    }

  }

}



The query above should return

{

  &quot;data&quot;: {

    &quot;serviceSource&quot;: {

      &quot;name&quot;: &quot;TestService&quot;,

      &quot;kinds&quot;: [

        {

          &quot;name&quot;: &quot;54769aae-2c63-4415-9b34-74e9ceb1d789\_TestKind&quot;,

          &quot;id&quot;: &quot;a745e4d7-74e9-4023-8f39-bc63d6076326&quot;

        }

      ]

    }

  }

}

1.
  1. 4.3.Query using allinstances

This type of query can be used to get instances for a kind in the same format as the query entry point:

query {

  allInstances(tenantId:0, kindId:&quot;a745e4d7-74e9-4023-8f39-bc63d6076326&quot;) {

    records {

      STRING

      ID

    }

  }

}



1.
  1. 4.4.Query across kinds

More complex queries can be used to more efficiently query across kinds. Many internal services can use this endpoint to reduce the number of DB requests required to fulfil a request.

Here is an example:

query {

    kindDBQuery(kindQuery:{

      kindId:&quot;fcd39416-337c-4566-b5bf-56cc8ff725d2&quot;

      fieldFilters: [{

        op: &quot;==&quot;

        fieldName: &quot;State&quot;

        value: {

          STRING:&quot;WA&quot;

        }

      }]

      and: [{

        kindId:&quot;fcd39416-337c-4566-b5bf-56cc8ff725d2&quot;

        toFieldName:&quot;City&quot;

        fromFieldName: &quot;City&quot;

        fieldFilters:[{

          op: &quot;==&quot;

          fieldName: &quot;Zipcode&quot;

          value: {

            STRING: &quot;98053&quot;

          }

        }]

      }]

    }) {

      records {

        ID

        STRING

        INT

        FLOAT

    }

  }

}

The outer most kind is the type returned, while inner kinds form a DAG where each inner kind constrains its outer kinds instances. The DAG should be considered to be a graph walk, where constraints are applied at each step in the walk, data is returned only if all constraints are true.

Filters are a collection of {op, fieldname, value} triples that constrain the Kind on which they are applied.

Operators that can be used:

- .for Strings : &quot;==&quot; , &quot;!=&quot; , &quot;oneof&quot;
- .for Numbers (Int and Float) :  &quot;&lt;&quot;, &quot;&lt;=&quot;, &quot;&gt;&quot;, &quot;&gt;=&quot;

1.
## 5.Developing knowledge microservices

  1. 5.1.Project Setup

start with a project template in your preferred language

follow the instructions on customizing it

Maana Knowledge Service Templates

In an attempt to develop best-practices, code base coherence (learn once), and to ease maintenance burden, we&#39;ve tried to standardize a number of things that a productive developer should not have to worry about and instead focus on adding real features.

Every project should follow a common, consistent pattern in order to reduce the tax imposed by infrastructure, such as web services and middleware, GraphQL services, logging, build, deployment, etc. As such, we recommend always  **cloning** a new project from the  [Maana Knowledge Service template](https://github.com/maana-io/h4-ksvc-templates) for your preferred language.

Once cloned, perform the customizations recommended in the template&#39;s README.md.

If your language isn&#39;t represented, we&#39;d be happy to work with you to develop a template or just send a pull request to have it included.

1.
  1. 5.2.Development

- .define GraphQL schema for the service endpoint
- .implement appropriate resolvers
- .be client (query, mutate, subscribe) to other Knowledge Services (dependencies)
- .stitch together existing services, providing an aggregated endpoint (to simplify client access)

1.
  1. 5.3.Packaging

prepare deployment

**managed service** : create a docker container

**unmanaged service** : not our problem

1.
  1. 5.4.Adding a knowledge microservice to the MAANA catalogue



- .prepare a service manifest (basically a serialized instance of Kind Service)
- .register the service in the catalog

Maana will:

 introspect the service

 create all the Kinds in KindDB

 along with boilerplate for  **Managed Kinds**

 this includes types, queries, mutations, and subscriptions

 register with CKG?

 boilerplate query and mutation resolvers and event emitters

 schema stitching between synthetic and service-provided resolvers



1.
## 6. Developing knowledge applications
2.
## 7.Troubleshooting

1.
## 8.Troubleshooting



Development Scenarios

local: configure and run a local docker machine Maana system and custom services (i.e., a &quot;deployment&quot;)

this is how the Maana R&amp;D work

emote: mixed environments - a Maana deployment is hosted elsewhere (e,g, Azure) and developing a service locally

not supported in v3.0?

if local and developing an a (locally) deployed service, then &quot;docker stop&quot; it and run/debug it manually

Project Setup

start with a project template in your preferred language

follow the instructions on customizing it

Develop

define GraphQL schema for the service endpoint

implement appropriate resolvers

be client (query, mutate, subscribe) to other Knowledge Services (dependencies)

stitch together existing services, providing an aggregated endpoint (to simplify client access)

Package

prepare deployment

**managed service** : create a docker container

**unmanaged service** : not our problem

Catalog

prepare a service manifest (basically a serialized instance of Kind Service)

register the service in the catalog

Maana will:

 introspect the service

 create all the Kinds in KindDB

 along with boilerplate for  **Managed Kinds**

 this includes types, queries, mutations, and subscriptions

 register with CKG?

 boilerplate query and mutation resolvers and event emitters

 schema stitching between synthetic and service-provided resolvers
