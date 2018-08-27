Research and Development : Developer Guide  

1.  [Research and Development](index.html)

Research and Development : Developer Guide
==========================================

Created by Marius Marcu, last modified by Donald Thompson on Jul 12, 2018

Old Word Doc: [OLD Developer Guide OLD](https://maanaio-my.sharepoint.com/:w:/g/personal/mmarcu_maana_io/ETNlfNIDxs1LkjOnN_0kkGQBRVxo1RsCZ9LdRFwXKTmwsw?e=mctYOb)

  

Document name

Audience

Description

Knowledge Technology Development Guide

Solution Engineers and Data Scientists

details the architecture of the platform, how to build a full solution and/or reusable components

Table of Contents
=================

/*<!\[CDATA\[*/ div.rbtoc1532523907698 {padding: 0px;} div.rbtoc1532523907698 ul {list-style: disc;margin-left: 0px;} div.rbtoc1532523907698 li {margin-left: 0px;padding-left: 0px;} /*\]\]>*/

*   [Table of Contents](#DeveloperGuide-TableofContents)
*   [Introduction](#DeveloperGuide-Introduction)
*   [GraphQL and Microservices](#DeveloperGuide-GraphQLandMicroservices)
    *   [GraphQL Learning Resources](#DeveloperGuide-GraphQLLearningResources)
*   [Knowledge Microservices and Bots](#DeveloperGuide-KnowledgeMicroservicesandBots)
    *   [Development Stages](#DeveloperGuide-DevelopmentStages)
        *   [Stage: Design](#DeveloperGuide-Stage:Design)
        *   [Stage: Local Service (Standalone)](#DeveloperGuide-Stage:LocalService(Standalone))
        *   [Stage: Local Service (Maana)](#DeveloperGuide-Stage:LocalService(Maana))
        *   [Stage: Unmanaged Service](#DeveloperGuide-Stage:UnmanagedService)
        *   [Stage: Managed Service](#DeveloperGuide-Stage:ManagedService)
    *   [Debugging a Knowledge Microservice](#DeveloperGuide-DebuggingaKnowledgeMicroservice)
    *   [The BotAction Protocol](#DeveloperGuide-TheBotActionProtocol)
*   [Knowledge Applications](#DeveloperGuide-KnowledgeApplications)
*   [Development Environments](#DeveloperGuide-DevelopmentEnvironments)
    *   [Visual Studio Code](#DeveloperGuide-VisualStudioCode)
        *   [Command Line](#DeveloperGuide-CommandLine)
        *   [Recommended Plugins](#DeveloperGuide-RecommendedPlugins)
        *   [Settings](#DeveloperGuide-Settings)

Introduction
Welcome to Maana
Maana was founded with the vision of using technology to systematize the world’s industrial expertise and data into digital knowledge that could significantly advance the global economy. Our mission is to facilitate the tens of millions of experts working in industrial companies around the world, and give them the ability to make better decisions, faster. We are devoted to putting the power of artificial intelligence (AI) into the hands of millions of industry experts, offering enhanced decision-making tools in a rapid response environment.

Better and Faster
In 2013 Maana invented a new way to represent industrial knowledge mathematically, using the patented Maana Computational Knowledge Graph™. This unique technology enables industrial companies to take human expertise and data from across silos and encode it into digital knowledge, eliminating the need to move data and enabling the creation of thousands of models at scale, through the re-usability of models across the enterprise.  

Scope
This guide is intended for the use of Solution Developers and Data Scientists, and describes how to develop services (aka "bots") and applications, and operationalize them in a production setting.

Glossary
Prior to working with the Maana Platform, it is suggested that you familiarize yourself with some basic terms and concepts that will help the make most out of your Maana portal experience. For a complete Glossary of Terms, please refer to the Glossary section of the Maana Corporate site.

Maana 101
This section is meant to provide a high-level introduction to the Maana Platform.  It introduces key concepts and language, and provides a brief overview of the training topics. 

In a nutshell, Maana is a platform for building an enterprise-wide knowledge layer for search, exploration, and solution development and delivery.

add 01_Maana _Introduction.pdf graphic here

Knowledge and Computational Models
Knowledge is an answer to a question. A knowledge layer is not just a relational database or data warehouse view of a complex business.  It is about things - their properties, and complex, dynamic relationships.

We represent such knowledge through structural and computational models.
Vessel Cargo vs. Vessel ETA.  A problem question is of the form: 
Given X, what is Y?  
This is also just finding a function f:X: → Y
Composition: &=((ℎ!+)

Maana Application and Users
Who uses Maana?
The Maana platform is used by solutions teams to deliver knowledge applications to their end users (i.e., business users, SMEs, managers). 

The Maana solution team approach involves the collaboration of: 

Analysts

Engineers

Data scientists

The Maana platform supports this collaboration using a simple process of problem decomposition. The Maana Knowledge Portal provides a shared environment for visual and programmatic development of a solution.

The Maana Solution
Maana solutions take a knowledge-centric approach to modeling and reasoning about various interconnected problem domains.  

Maana solutions generally follow a simple pattern:

Observe the problem domain.

Reason over the problem domain to make (explicable) recommendations.

Decide (assist the user) with what action to take.

Learn from the outcome of the decision in order to make better recommendations

Real World Examples
Given a set of invoices, what is the optimal collection plan?

Given a set of malware victims, who are the most vulnerable likely victims?

Given a patient history and a patient condition, what is the most relevant history?

Given a set of Bitcoin wallets, what are the most likely fraudulent transactions?

Given a customer equipment failure, what is the recommended repair action?

Given a set of insurance claims, which are most likely fraudulent?

Given a job activity, what are the relevant Health & Safety considerations?

Composing and Reusing Models and Solutions offers the Maana user:

Predictive Maintenance:  Planned Maintenance, Supply Chain Optimization

Trade Finance:  Shipping and Port Operations

Upstream:  Planning, Production and Capital Allocations

Solution Team Roles
Analysts

Data Scientists

Developers

Product Owner Consultant, etc.

Statistician, Optimization Engineer, etc.Provides expertise on how to connect the dots laid out by the Analyst

Software Engineers, Front-end & Backend Devs, etc.

Defines the requirements and success criteria

Build services, models, and algorithms to capture knowledge computations

Provide integration and automationEnable solutions to operate at production-levels

Represents the SMEs, drives problem decomposition, and builds the domain model

​

Create front-end applications over the CKG for end users

Primarily interacts through the Knowledge Portal

​

​

Installation and Setup
We use Auth0 to provide authentication and authorization.  Auth0 provides:

Account configuration with a source of users

Sufficient privileges to add a callback address

Single pages application in Auth0

Note:  If you are upgrading from v3.1.1-
Clusters prior to 3.1.2 did not set some client flags when mounting the gluster volumes, these are required on later deployments. Not setting them can result in corrupted data. To update an existing v3.1.1 cluster, the gluster volume mount must be updated in /etc/fstab for each node as follows:
:/saq_volume /maana/saq glusterfs defaults,_netdev,attribute-timeout=0,entry-timeout=0,direct-io-mode=enable 0 The only change here is adding ,attribute-timeout=0,entry-timeout=0,direct-io-mode=enable 
to the existing entry. Once this has been changed on all of the nodes, with the cluster stopped, the following should be run to remount the drives with the correct options.
ansible all -i scripts/d.py -m shell -a "sudo umount /maana/saq" 
ansible all -i scripts/d.py -m shell -a "sudo mount /maana/saq"

Create Free Accounts
The accounts shown below are all free to create, and are recommended to be set up prior to using Maana:

​Github – Utilized for cloning repos and logging into Launchpad. web-based hosting

​Heroku – Utilized for hosting the service we code on the cloud.  cloud platform as a service (PaaS)

Recommended Installs
The following installs are recommended prior to using Maana:

Google Chrome (preferred browser)

Python 3.6

NodeJs 9.3 (and NPM)

GitHub Repos to Clone
The command line interface we use to programmatically interact with the platform: https://github.com/maana-io/Q-tutorials.git​

The Python template for creating a Maana service: https://github.com/maana-io/Q-ksvc-templates.git​

Launchpad Examples to Fork (requires above GitHub account)
Introductory Example: https://launchpad.graphql.com/mpjk0plp9​

Weather Example:  https://launchpad.graphql.com/r9xzw8zxmn​

Subjects for Prior Review
It is suggested that a review of the following areas be made prior to utilizing Maana: 

GraphQL intro concepts:

High level introduction: https://www.howtographql.com/basics/0-introduction/​

Developer focused: https://www.youtube.com/watch?v=Y0lDGjwRYKw&list=PL4cUxeGkcC9iK6Qhn-QLcXCXPQUov1U7f​

Readme for Graphene Library.  This is the Python tool used to create GraphQL endpoints for Python services: https://github.com/graphql-python/graphene​

Provisioning the Cluster
Azure Docker Container 
The Azure Docker Container is not required if you have the Azure CLI tools installed, but guarantees the Azure commands will behave as described in the following sections.

Starting the Azure CLI Docker Container and Login 
From the directory containing this file:

docker run -t -d --rm --name azcli_template -v pwd:/data azuresdk/azure-cli-python 
docker exec -it azcli_template /bin/bash 

Login and Select the Subscription:
az login
az account set --subscription <Subscription ID or Name>  

Create the Resource Group for the Deployment 
Export the environment variables to make it easier to use them in the latter steps, and create the group.

export RESOURCEGROUP= <resource group name>
export LOCATION= <Region Name (example: centralUS)>
az group create --name $RESOURCEGROUP --location $LOCATION 

Cluster Configuration 
Edit the Parameters File 
vi parametersFile.json 

We recommend machines with at least 50 GB's of RAM and available SSD, currently we test internally on "Standard_E8s_v3". By default a single data disk is added to each agent node, the size of which can be adjusted in the parameter file, additional disks can be added to the agent boxes, and to gluster as required. 

The default configuration is for a distributed Gluster cluster (no replication).  If failover is required, the configuration can be adjusted in the Ansible Playbook that mounts Gluster. See the Gluster Administration Guides for configuration options, and general administration guidelines.

Start the Cluster 
This will usually take between 5 and 15 minutes.

az group deployment create -g $RESOURCEGROUP --template-file "./template.json" --parameters "./parametersFile.json" 

Add the Firewall Rules
If you don't need to whitelist IP address ranges, you can omit the creation of the security group. It can also be created in the UI later.  The only requirement is that both the loadbalancer public IPs must be whitelisted for all incoming ports in the incoming rules.

Note:  If you do not need to install this, please skip to the Install the Docker Azure:Cloud Plugin section that follows.

Get the IP address's of the load balancers 
You can do this from the Azure UI, but the following commands will return them:

az network public-ip list -g $RESOURCEGROUP --query "[].[name, ipAddress]" 

Create a Network Security Group for the Network 
Again we'll use an environment variable to make it easier to specify consistently:

export SECURITYGROUP= <security group name>
az network nsg create --resource-group $RESOURCEGROUP --name $SECURITYGROUP 

Whitelist the Machines in the Cluster 
Edit the IP addresses below to match the addresses returned in the previous step. 
TO DO - Check names of loadbalancer

az network nsg rule create --resource-group $RESOURCEGROUP --nsg-name $SECURITYGROUP --name externalLB1 --access allow --protocol "*" --direction inbound --priority 100 --source-address-prefix <SSH IP address>/32 --source-port-range "*" --destination-address-prefix "*" --destination-port-range "*"
az network nsg rule create --resource-group $RESOURCEGROUP --nsg-name $SECURITYGROUP --name externalLB2 --access allow --protocol "*" --direction inbound --priority 101 --source-address-prefix <public IP addres>/32 --source-port-range "*" --destination-address-prefix "*" --destination-port-range "*"
Whitelist Maana Bellevue 
Skip if unnecessary.  Add any additional IP ranges as required.

az network nsg rule create --resource-group $RESOURCEGROUP --nsg-name $SECURITYGROUP --name Bellevue --access allow --protocol "*" --direction inbound --priority 150 --source-address-prefix 198.233.204.202/32 --source-port-range "*" --destination-address-prefix "*" --destination-port-range "*" 
Add the Security Group to the VNet 
The VNet name can be obtained with the following:

az network vnet list -g $RESOURCEGROUP --query "[].name" 

Then add the sec group to the agents subnet:

az network vnet subnet update --vnet-name  --resource-group $RESOURCEGROUP --name swarmm-agentpublicsubnet --network-security-group $SECURITYGROUP 

Install the Docker Azure:Cloud Plugin
Create the Storage Account
Note:  The name of a Storage account must be globally unique within Azure - so select a name that is unlikely to have a collision.

az storage account create -g $RESOURCEGROUP -n <storage account name> --sku Standard_LRS --kind Storage
Get the Credentials to Connect to it
az storage account keys list -n <storage account name> -g $RESOURCEGROUP --query "[0].value"
Exit the Container 
Add the Secure Shell (SSH) private key to the agent.  This allows you to forward the private key, so you can add the SSH to agent nodes on the cluster.

ssh-add <private key file name>
Warning:  If for some reason you do not wish to do this, you must copy the private key to the master node.  If you do not copy the private key to the master node, the remaining provisioning steps will fail.

Install the Docker Cloud Driver on all of the Nodes
ssh -A docker@<ssh lb ip address> "ansible all -i scripts/d.py -m shell -a 'docker plugin install --alias cloudstor:azure --grant-all-permissions docker4x/cloudstor:18.03.1-ce-azure1 CLOUD_PLATFORM=AZURE AZURE_STORAGE_ACCOUNT_KEY=\"<Account Key>\" AZURE_STORAGE_ACCOUNT=\"<storage account name>\"'"
Verify the Install
ssh -A docker@<ssh lb ip address> "ansible all -i scripts/d.py -m shell -a 'docker plugin ls'"
All Nodes should show the Plugin enabled as indicated below:
swarmm-agentpublic-15097170000003 | SUCCESS | rc=0 >>
ID                  NAME                DESCRIPTION                       ENABLED
18df342986d2        cloudstor:azure     cloud storage plugin for Docker   true
swarmm-agentpublic-15097170000004 | SUCCESS | rc=0 >>
ID                  NAME                DESCRIPTION                       ENABLED
e16821ad5629        cloudstor:azure     cloud storage plugin for Docker   true
swarmm-agentpublic-15097170000002 | SUCCESS | rc=0 >>
ID                  NAME                DESCRIPTION                       ENABLED
4d983ba58907        cloudstor:azure     cloud storage plugin for Docker   true
swarmm-master-15097170-0 | SUCCESS | rc=0 >>
ID                  NAME                DESCRIPTION                       ENABLED
6ccd18e0db46        cloudstor:azure     cloud storage plugin for Docker   true
Install Gluster/Label Swarm Nodes with Roles
This is done through an Ansible Playbook .ssh to Master Node, and run the Ansible Playbook.

Note:  You must specify agent forwarding (-A), or copy the private key to the master.

ssh -A docker@<ssh lb ip address>
ansible-playbook -i scripts/d.py scripts/mountdisks.yml
Validate as Follows:
cat /proc/mounts
Your line should appear to be similar to the one shown below: 

swarmm-agentpublic-15097170000004:/saq_volume /maana/saq fuse.glusterfs rw,relatime,user_id=0,group_id=0,default_permissions,allow_other,max_read=131072 0 0
Install the SSL Cert and NGinX Configuration
Note:  You cannot use a self-signed certification.  Many of the services will not accept it.

Copy the certs to the cluster in the /etc/keys directory.  Copy the NGinX Config file to the /etc/NGinX directory, then edit the NGinX config file.

OUTSTANDING - TO DO

SSL can be disabled in the NGinX Config if a cert is unavailable. 

OUTSTANDING - TO DO

Add a DNS Entry that matches the Cert for the Public IP address
This depends greatly on corporate infrastructure, for internal testing we use Azure to host DNS zones, and add a new cluster public IP address to a zone that it matches a wildcard cert. It can take up to 30 minutes for a DNS entry to become visible, but usually only requires a few seconds. 

Verify that the DNS entry you have added is visible by pinging the cluster.  It will not respond, but you should see the IP address resolve.

ping <cluster name>
Platform Deployment
Preparations
Copy the Compose file to the Cluster:

scp <compose file> docker@<SSH IP address>:
SSH to the Cluster:

ssh -A docker@<ssh lb ip address>
Login to Docker:

docker login
Deploy the Platform
The stack can be named anything you want (we use azureDeploy, as shown  below). 

Note:  Please remember the Docker requirement that every command must be typed in.

REACT_APP_PORTAL_AUTH_DOMAIN=  REACT_APP_PORTAL_AUTH_IDENTIFIER=  REACT_APP_PORTAL_AUTH_CLIENT_ID=  REACT_APP_CLI_AUTH_CLIENT_ID=  REACT_APP_CLI_AUTH_DOMAIN<=CLI App Domain - usually the same as prtal Auth domain>  SERVICE_AUTH_CLIENT_ID=  SERVICE_AUTH_CLIENT_SECRET=  SERVICE_AUTH_IDENTIFIER=  API_AUTH_DOMAIN=  API_AUTH_CLIENT_ID<=id of service app>  API_AUTH_CLIENT_IDENTIFIER=  PUBLICNAME=  PUBLIC_BACKEND_URI=https://:8443  docker stack deploy -c docker-compose.yml --with-registry-auth azureDeploy

Be Patient
Wait for everything to come up.  Services will fail during startup.  This is normal, as long as there is at least one copy of the service in a running state at the end of startup. You can look at the current state with either:

docker service ls
or:

docker stack ps azureDeploy
Determine the System Stability
Go to the prometheus page at:

http://<cluster name>:9090
Under status/targets, verify that saq is reporting green.  It is normal for Docker to report unavailable.

Metrics
Available on publicIP address port 4000, default login is admin/admin, I suggest you login and change it.

http://<cluster name>:4000
OUTSTANDING - TO DO - Update with import of dashboards
Bootstrap the System with a GraphQL Request
Find the ID of the Consul Node running on the Master, and start a shell in it.

docker ps 
docker exec -it  /bin/sh

Run the bootstrap command:

curl -v -H "Content-Type: application/json" -X POST -d "{\"query\": \"mutation { bootstrapSystem }\"}" http://maana-admin:8020/graphql
Be Patient

This takes a few minutes, watch for completion in the maana-admin logs:

docker service logs azureDeploy_maana-admin -f
The logs will end with something like:

azureDeploy_maana-admin.1.tkz0nuqy7llg@swarmm-master-15097170-0    | [io.maana.admin] Finished bootstrapping Maana!! (139.207 seconds)
Verify the Installation
https:<cluster name>
Update Auth0
Login and add the callback it complains about to the Auth0 Callback section (you can just click on the link).   Reopen the page and login.

​

​

The Maana Platform
The Maana platform is used by solutions teams to deliver knowledge applications to their end users (i.e., business users, SMEs, managers). This “solution team” approach involves the collaboration of analysts, engineers, and data scientists. The platform itself is designed to build an enterprise-wide knowledge layer, utilizing search and exploration for solution development and delivery.

The Computational Knowledge Graph
At the core of any Maana solution sits our Computational Knowledge Graph (CKG), a network of models built using machine learning techniques and artificial intelligence that powers the AI-driven applications used to digitize decision support and operations. It allows for the knowledge of any business to be incrementally captured and grown, coupling that with ongoing evolution and ever increasing sophistication as more projects are developed.

Utilizing GraphQL
Maana uses GraphQL to represent and expose its Computational Knowledge Graph. GraphQL is a data query language created by Facebook and open-sourced in 2015 as an alternative to REST interfaces.

GraphQL provides a complete and understandable description of the data in the API, gives clients the power to ask for exactly what they need and nothing more, makes it easier to evolve APIs over time, and enables powerful developer tools. With the GraphQL-type system, developers can access the full capabilities of your data from a single endpoint.

GraphQL creates a uniform API across your entire application without being limited by a specific storage engine. The developer provides functions for each field in the type system, and GraphQL calls them with optimal concurrency.

GraphQL Features
GraphQl query language is maintainable, with the ability to evolve an application program interface (API) without multiple versioning.   With GraphQL, old Clients remain unaffected by new features and deprecating fields discourage new uses.  It is an open specification language offering a lean core, multiple implementations and encourages ongoing innovation.  Utilizing GraphQL:

The server is decoupled from the Client

The server knows almost nothing about Clients

Apps request exactly the data they need

It is mush easier to pull data into the Client models.

 GraphQL models data in an intuitive way:
Nodes have Attributes

Nodes have links to other Nodes

It suits our model of the world

Meshes well with object-oriented programming

GraphQL is Composable:
It is capable of fetching everything need with only one request.

It fetches only the data that is needed.

It has the ability to specify data requirements locally.

GraphQL is strongly typed and introspective, featuring documentation that is built-in, powerful tooling, and the ability to can catch errors easily.  

GraphQL acts as a single source of truth:
It supports multiple back ends.

It can pull data from anything.

It is resilient.

It uses existing application logic.

It is not a storage engine.

Maana Platform Architecture and GraphQL

For more information on using on Graph QL, please refer to the link provided below:

​How to GraphQL​

Graph QL Schema Language
The Type System
Object Type
Represents a node within the GraphQL graph:

type Human {
    name:  String
    currentlocation:  String

Note:  Must have at least one field.

Object Type:  Field Resolvers
Resolvers are passed with 3 main Arguments:

Object - The Object on which the Field is being resolved.

Args - The Arguments passed to the Field.

Context - A Context object useful for authentication and caching.

Object Type:  Summary
This represents a Node within the GraphQL graph:

type Human {
    name:  String
    currentlocation:  (approximate: Boolean): String
favouritePosts(first: Int = 5 after: Cursor): [Post}
}


For each Node:

They must have at least one Field.

Each Field must return a defined Type.

Fields are backed by a function called a Resolver.

Fields can have Arguments.

Arguments can have Defaults.

Union Types
Union Types are assumed to have no Fields in common.

{
  search(terms:  "Star Wars")  {
       ... on Film  {
          title
          duration
      }
      ... on Publisher  {
          name
          address
       }
     }
    }   

GraphQL Schema Language - Writing a Schema
The Mutation Type
A typical example might look like this:

type mutation  {
    setGreeting(input:  SetGreetingInput):  SetGreetingPayLoad
}
input  SetGreetingInput  {
   newGreeting:  String
}
type  SetGreetingPayLoad  {
   greeting:  String

Schema Example
scalar  JSON
enum Color  {  RED,  GREEN,  BLUE,  OCTARINE  }
input AgeRange  {  min:  Float!,  max:  Float!  }
interface NamedEntity  {  name:  String!  }

"""Someone who can log in"""
type User implements NamedEntity  {
   firstName:  String @deprecated(reason:  "Please use 'name' instead")
   name:  String!
   #  To the nearest nanaosecond
   age:  Float
   meta:  JSON
   favoriteColor:  Color
   friends:  [User}
}
type Query  {
   viewer:  User
   usersByAge(range:  AgeRange):  [User!]
}

 Anatomy of a GraphQL Query
This is an example of a simple GraphQL query; 

{
   currentUser  {
   name
   website
  }
}

query ProfileQuery {
    currentUser  {
         name
         website
      }
 }

The name (ProfileQuery) is an optional, arbitrary name for the query.  It is useful for debugging.

Directives
Directives can be used to change how the server processes the query.  The GraphQL specification defines two directives:

@include

@skip

query ProfileQuery(
  $id: Int!,
  $toggle: Boolean = true
) {                                                              {
     user(id:  $id)  {                                              "user": {
        name          @include(if: $toggle)                          "name": "Bob Smith"
        website        @skip(if: $toggle)                          }
     }                                                              }
  }

Recap:
query  ProfileQuery($id:  Int!)  {
  user(id: $id) {
     fullname: name
     website
  }
}
query                                #Operation type
ProfileQuery                         #Operation name, for debugging
($id:  Int!)                         #Variables and their types
{                                    #Selection set
     user(id:  $id)                  #Field with arguments, variable reference
     {                               #Nested selection set
          fullName:  name            #Aliased field, fetches 'name', calls it 'fullName'
          website                    #Field sans alias
     }              
}               

GraphQL Fragments
The Anatomy of a Fragment
fragment GreetUserFragment on User {
  id
  name                    query MyQuery {
}                           currentUser {    
                               id
query MyQuery {                id
currentUser {                  name
  id                          }
  ...GreetUserFragment      }
     }
}     

Fragments can Compose other Fragments:
quey MyQuery {
  currentUser {
     ...UserProfileFragment
  }
}
fragment UserProfileFragment on User {
  id
  name
  bio
  ...LargeUserAvatarFragment
}
fragment LargeUserAvatarFragment on User {
   largeAvatar: avatar(size: 300, retina: true)
}

Note:  Simple string concatenation, as shown here, will not allow the same fragment twice.  It is suggested that a tool such as Apollo or Relay be used to do this.

Inline Fragments
A variant of these named fragments are anonymous inline fragments.

{
   search(phrase: "cream"){
     ...on Food{
       manufacturer
       calories
    }
    ...on BeautyProduct {
      manufacturer
      spf
    }
    ...on Paint {
      manufacturer
      hexColor
     }
    }
  }

GraphQL Mutations
Performing Multiple Mutations in One Query
mutation  {
  # Mutation  1
updateCurrentUser(input:  {
  userPatch:  {
    website:  "https://twitter.com/benjie"
  }
 }) {
  user { website }
 }
  # Mutation 2
  setGreeting(input:  {
   newGreeting:  "Hello Universe!"
 }) {
    greeting
  }
}

Computational Knowledge Graph Features
Key Features:
Upload structured and unstructured content

Bots will automatically interpret, index, mine, and classify content

Analysts can curate and extend the graph

Analysts can train and employ text classifiers

Bots will automatically generate associative networks

Analysts can search, explore, query, filter, and visualize the graph

Developers can create and publish knowledge services

Scaled, multi-tenant Azure deployments

Computational Knowledge Graph Functionality
Maana uses and builds on the core GraphQL technology to make developing solutions a simpler and more intuitive process:

The Graph is dynamic - Nodes, which represent concepts in the graph, are not static containers, but rather act as computational vessels, allowing algorithms to be stored and executed. 

Reusable Models - Knowledge Graph flexibility enables groups across the organization to leverage and build-upon models created by other groups, dramatically accelerating the speed at which models are created throughout the organization. These models are dynamic, and once operationalized into applications, they learn and adapt based on the user behaviors and provide continuous intelligence for day-to-day operations. 

The structure of data is separated from its content - This separation enables a fluidity of modeling, and data from any source or format can be seamlessly integrated, modeled, searched, analyzed, operationalized and re-purposed. 

Data remains at the source - Only the most relevant data, within the context of what is being optimized, is indexed and brought into the graph. 

Each resulting Data Model is a unique combination of three key components, which are instrumental in optimizing assets and decision flows:
1.  Subject Matter Expertise 
2.  Relevant Data From Silos
3.  The  Right Algorithm

Note:  Algorithms may range from being as simple as pulling in new external source data, to being as complicated as classification of documents through machine learning.

The CKG consists of concepts and properties, instances and values, and relations and links.

Consider the concept of a ContainerShip with the properties of name, length, position, etc. A specific instance (entity) for our ContainerShip might have values for each of these properties - such as the vessel Maersk Viking having a length of 400 meters. Such properties can be "scale-able" (e.g., numbers, strings, dates) or might refer to other concepts/instances - such as the Hold Cargo of the vessel.

In some cases, property values for an instance may simply be stored, as they are rarely subject to change. In other cases, they will be dynamically computed due to constantly changing values - such as a ship's weight (which is dependent upon its cargo) or variables like the vessel’s current position (which requires getting a GPS reading).

Unlike a traditional graph database, Maana incorporates arbitrary computation (through custom GraphQL resolvers) and distributes the graph into subgraphs managed by different microservices, optionally with their own dedicated persistence mechanism. This separation enables a fluidity of modeling, permitting data from any source and in any format to be seamlessly integrated, modeled, searched, analyzed, operationalized and re-purposed. It also places more responsibility on the microservices to provide their own storage.

Data Model Split
To address this, Maana proposes an explicit split between the data models (i.e., GraphQL type definitions) that a service uses and its operations (i.e., GraphQL resolvers). Maana will generate the appropriate managed service for such models using KindDB, Prisma, neo4j, etc. As a result, the solution developer only has to provide the logic they require, and then lets the system handle of all the CRUD/ORM-like operations on the data.

Knowledge Microservices
Knowledge microservices are a class of GraphQL services developed for the Maana platform. Unlike pure client-server or n-tier architectures, Maana's microservices act as peers in an asynchronous and loosely-coupled arrangement that promotes independent scaling and extensibility. They provide identity and access controls, graph persistence, search, machine learning, and natural language processing.

As a consequence, these peer microservices provide reasoning capabilities to Knowledge Applications, which help solve domain-specific problems and support optimal decision-making that is capable of learning over time. Taken together, these services allow the solution developer to focus on designing GraphQL schemas and implementing computational resolvers only where needed.

The Maana platform manages these services - providing authentication, reliable messaging, (automatic) graph persistence (with search and querying), scaling, monitoring, and a rich UX. These GraphQL services include:

Authenticated Access

Client/Server Boilerplate

Reliable Messaging using RabbitMQ

Lifecycle Management (info, register, deregister)

Docker Containerization and Automatic Scaling/Load Balancing

Maana Knowledge Services form a network of GraphQL endpoints, exposing their types, queries, and mutations for direct access, as well as publishing and subscribing to network events. A GraphQL service (or endpoint) consists of:

Types

Queries

Mutations

Events (pub/sub)

Examples of Knowledge Microservices include:

Indexers

Miners

Classifiers

Text

Statistics

Entity

Number

Probabilities

Field

Time

NER/NLP

Document

Geospatial

​

Image

Geometric

​

​

Bots and Bot Actions
A Bot is a Knowledge Microservice that “listens” for specific events on a system bus, and acts automatically when such events happen to mine and enrich the graph. They provide specialized queries and mutations that perform BotActions, allowing the bot to provide asynchronous status updates. 

Knowledge Services can become Knowledge Bots by offering and using these Bot Actions, which allows the service to interact more directly with users.

Example:  When a raw data file is loaded into MAANA, a bot automatically analyzes it to identify mentions of entities like persons, phone numbers, values, and facts.

Users can configure, start, stop, and schedule bot actions. This enables user interface components to immediately return the latest status: 

Throughout the duration of long-running, asynchronous operations.

By events that act as automatic triggers (such as entity recognition, new concept creation and classification).

Report any errors or messages.

There are two primary scenarios to consider here: 

Event Handling

Direct Query/Mutations 

Event Handling 
When a Knowledge Service subscribes to and handles an event, such as "fileAdded," it can (optionally) create an instance of a BotAction Kind by mutating the Computational Knowledge Graph.

Note:  As the service performs its operation, it can periodically update the progress (if it is deterministic), and update the status and report errors.

Queries and Mutations 
A user can invoke a query or mutation either explicitly (e.g., train a classifier on labeled data) or implicitly (i.e., as part of query graph). In such cases, the service exposes such queries and mutations as returning a BotAction.

Kinds
All Kinds (concepts, types) are associated with a service. This service is said to provide the (entities of) the Kind. Many Kinds are purely extensional (i.e., data) and do not have custom CRUD behavior. These Kinds are automatically managed by the Computational Knowledge Graph (CKG) and stored in the KindDB, where they are indexed for efficient search and querying (including sophisticated entity concurrences).

Services also depend on existing Kinds and queries, mutations, and events. The services that provide these Kinds (which may be fully managed by CKG/KindDB) can be imported into a new service purely through GraphQL and specified in a manifest that is used to create and register a new service. This process will result in a merged schema on a service-specific endpoint that the newly developed service uses for all Maana GraphQL communication (non-pub/sub).

The User Experience
The UI (Explorer, KGE, Context) will query for active BotActions and indicate progress and status on relevant Kinds and instances. Using stock functionality, service queries and mutations can be connected to inputs and outputs, and static parameters can be set. Additionally, users can stop/cancel long-running operations, and possibly resume, if the service supports it.

Asynchronous Results 
One of the primary use cases for BotActions is to support potentially long-running, asynchronous operations. The underlying GraphQL model for queries and mutations is synchronous. 

For example, the operation:

​ Code Block

language    js

search(term: String!): [Document!]!

can be queried as:

​ Code Block

languagejs

const SEARCH_QUERY = gql`query search($term: String!) {  

     search(term: $term) {    

          id    

          name     

          ...  

          }

}`

​

...

const documents = await client.query({  
    query: SEARCH_QUERY,  

    variables: {term: "Kratos"}

})

IIt is thus expected that the set of documents found by the search operation will be returned as part of a single call, despite it being Invoked asynchronously. 

This is fine for short-lived operations (i.e., < 1 minute), but, in practice, some queries (or mutations) can take several minutes, hours, or even days (e.g., complex Spark or TensorFlow jobs). In such cases, we'd like a future-like mechanism in which we are given some sort of token that can be used to coordinate access to progress and results.

To accomplish this, we propose the following pattern:

​ Code Block

language    js

searchAction(term: String!, resultKey: String!): BotAction!searchResult(resultKey: String!): [Document!]! 

In this case, the original operation has been extended to accept an additional argument, resultKey, which is used to retrieve the results for a particular invocation. A BotAction instance (generic) is returned (if the operation is initiated successfully) and can be used to track the progress of the operation (polling or event-based, errors, etc.). It also records the resultkey value that was used for convenience. 

The service must also provide a separate query that returns the actual results that have been generated asynchronously. (Paging, skips, etc. can be added here as appropriate, but are not part of the BotAction protocol.)

Schema
​

​

​

Development Stages
The development of a Maana Knowledge Application solution can be best described in five stages: 
1.  Design
2.  Local Service (Standalone)
3.  Local Service  (Maana)
4.  Unmanaged Service
5.  Managed Service

Design Stage
The primary focus of this stage is: 

GraphQL types (Kinds, Properties, Relations)

Queries and Mutations (often based on Problem Questions)

Events (consumed and produced)

During this Design Stage, the overall problem requiring a solution is analyzed, and a domain model is generated with a set of decomposed problem questions. Some discussion of entity sources and data science has taken place, and the point within the Development timeline has been reached where one or more Knowledge Microservices should be coded to provide some new concepts (Types, Kinds) along with Queries, Mutations, and Events that involve them.

Focus on the GraphQL
For the Knowledge Service/Bot, this is the entire description of (and interface to) their world. Define a file in the GraphQL SDL, such as model.gql, and include custom queries, mutations, and publications/subscriptions. 

Plan the custom resolvers.

Consider such things as the nature of their core logic.

Local Service Stage (Standalone)
The primary focus of this stage is: 

Selecting the programming language

Identifying dependencies - Libraries, existing services and domain models (often based on Problem Questions)

Core Logic – Used to satisfy GraphQL interface contract

With at least an initial design complete, the following implementation decisions must be made: 

Are there existing Kinds and Services that can be reused?

Are their existing code libraries or ML models that can be reused?

What is the best programming language for this task?

Is there reference data or domain data?

Are there long-running tasks?

What are the scale, performance and capacity factors?

Once a programming language is chosen, then an  existing project template  can be used to scaffold a new Knowledge Microservice/Bot in Scala/JVM, Python, JavaScript, etc.

Note: The development of core logic, or a machine learning solution occurs (as it normally should): writing, testing, and debugging code or improving model accuracy.

The Knowledge Service is a standard GraphQL endpoint, which can be run and tested within the development environment, and used via Maana CLI, its own exposed GraphiQL, a standalone GraphQL Playground, etc.

At this point, the overall problem to be solved has been analyzed and a domain model and set of decomposed problem questions has been generated, some discussion of entity sources and data science has taken place, and it is now time to code one or more Knowledge Microservices to provide some new concepts (types, Kinds) along with queries, mutations, and events that involve them.

Local Service Stage (Maana)
In the previous stage, it is likely that various calls/services were stubbed or mocked because they required accessing a Maana cluster. It is now time to interact with a Maana deployment, typically one dedicated to development.

Communication to a Maana system requires the use of authentication. This is configured differently, based on the language/framework being used. Refer to the project template documentation used to scaffold the Knowledge service project.

The service being development must be registered with Maana. This is typically done programmatically using the Maana CLI by specifying a manifest that describes the service and its dependencies. For example:

{
  id: "io.maana.azure.crawler",
  name: "Maana Azure Storage Crawler Service",
  dockerRegistry: null,
  hostedUrl: null,
}
The result of registering a new service with Maana is that CKG will generate a dedicated service endpoint for the new service, e.g., https://knowledge.acme.com/service/io.maana.azure.crawler:7331. This enables the standalone service to communicate to Maana, but does not allow Maana to dispatch calls to the standalone service due to network connectivity restrictions. (This will be overcome in the next stages.)

The full GraphQL schema for a service manifest is:

# from io.maana.system
type ServiceManifest {
  id: ID!
  name: String
  description: Text
  registeredOn: DateTime
  registeredBy: User
  dockerRegistry: Url
  hostedUrl: Url
  modelSdl: String
}
Stage: Unmanaged Service
When it is time to have full participation in the Maana processing network, i.e., having its endpoint services consumed by other services, user interfaces, or automatically based on event subscription, then Maana's CKG must be able to communicate to the service. This means that the service must itself be deployed to a host that is accessible from the Maana cluster, e.g., Heroku, Azure, AWS, on-premise. In this configuration, the service owner is responsible for deploying, monitoring, scaling, securing, etc. the service, since Maana only has knowledge of a GraphQL endpoint URL and the schema it provides.

See the Deploy to Heroku tutorial.

Stage: Managed Service
Maana can completely manage a containerized Knowledge Microservice/Bot, by specifying a Docker registry that the Maana deployment has access to (e.g., DockerHub, Azure). The following additional information is required when configuring a service to be managed by Maana:

scale stuff ??

?? Andrey Batyuk​

Debugging a Knowledge Microservice
​Andrey Batyuk​

The BotAction Protocol
See technical design note.

Knowledge Applications
Solution developers can develop knowledge applications on top of the MAANA Knowledge Graph.

Who is the audience for the applications?

SME searching for answers to specific questions (i.e. Given a ship, a ship route and an omitted port, what re-route options can be considered to minimize trip duration for the loads onboard?)

What type of applications can be developed?

Custom web apps, Power BI apps, Tableau, Spotfire and others?

What are the steps to build an application?

how does a developer publish and manage/share/update/delete an app?

The MANA Catalog
The Maana Catalog is the repository for all components that can be used as part of MAANA knowledge solutions.  The following section discusses how to  add/publish/update/delete components in the catalogue.

Defining a Model in Maana
The following documentation uses XXX.XXX.XXX.XXX facing to represent the public URL of the system. All of the following examples use the GraphiQL endpoint - which is interactive, programmatic access conducted through the GraphQL endpoint.  GraphQL API is organized around three main building blocks:

 Schema

 Queries

 Resolvers

Queries can be executed via GET or POST, with either the body of the query as the query string or in the body of the POST. All mutations must use the POST endpoint and have the header content type set to application/json. Also, the GraphQL server needs a resolver to know what to do with an incoming query.


Anatomy of a GraphQL Query (source)  

​

The following is an example of how to create a graph with no mutation specified, using the cURL command line tool for getting or sending files using URL syntax.

curl -v -H "Content-Type: application/json" -X POST -d "{\"query\": \"mutation 
{ ..... }\"}" http://XXX.XXX.XXX.XXX:8003/graphql 
Maana engineering team is currently implementing authentication functionality, which will be required for all clients.  For additional information on this subject, please use the links provided below:

How to create a graph from a GQL definition: 
https://confluence.corp.maana.io/display/RD/Maana+Q+programmatic+interface


How to add instance data:


How to query instance data:


How to perform complex queries:
https://confluence.corp.maana.io/display/RD/Technical+Design+Notes#TechnicalDesignNotes-Queries


How to perform search:
https://confluence.corp.maana.io/display/RD/Maana+Q+Search+-+Technical+Design​

Hydrating the Model with Data
To hydrate a model with data:

Create a service source with GraphQL schema. 

Types must correspond to Kinds on the portal endpoint XXX.XXX.XXX.XXX:8003/graphiql. 

Note:  The service can have many Kind definitions.

mutation { 
  addServiceSource(input: { 
    name: "TestService", 
    schema: "type TestKind { \n id: ID \n name: String! \n something: Int! }" 
  }) 
 }

Once the Service Source has been created, it will return a Service ID:

{ 
  "data": { 
    "addServiceSource": "54769aae-2c63-4415-9b34-74e9ceb1d789" 
   } 
}

Use the ID to establish the Service endpoint for subsequent interactions:

XXX.XXX.XXX.XXX:8003/service/54769aae-2c63-4415-9b34-74e9ceb1d789/graphiql

Note:  If the service is re-created by updating definitions, its service ID will change, while the old service will still be available.

Adding a Single Instance of a Kind
mutation { 
  addTestKind(input: { 
    name: "testname", 
    something: 42 
   }) 
}

An ID is assigned and returned for the instance itself.

{ 
  "data": {    
"addTestKind": "431f9338-95be-4f89-968c-4f2bb197965c" 
  } 
} 

Once the instance data has been added, we can query it using:

query { 
   allTestKinds { 
    id 
    name 
    something 
  } 
} 

Adding Multiple Data Instances Simultaneously to a Kind
mutation addSomethings { 
  addTestKinds(input: [{ 
    name: "testname2", 
    something: 45 
  }, { 
    name: "testname3", 
    something: 43 
  } 
  ])
} 

Note:  Larger data volumes should be broken into reasonably sized chunks (i.e. up to 5,000 instances), and submitted sequentially through the endpoint.

Data can be queried using:

query { 
  allTestKinds { 
    id 
    name 
    something 
  } 
} 

Querying a Model
Query through Service End Points
This is the most common way to query the data once ingested in the graph:

query { 
  allTestKinds { 
    id 
    name 
    something 
  } 
} 

Query using Global Entry Points
There are more generic ways to read Kinds from a model than through service entry points.  However,  when using the Global Entry Points (rather than the Service Entry Points), Kinds should be referenced by ID. 

Note:  The IDs for each Kind can be obtained from the Service ID by running a query against XXX.XXX.XXX.XXX:8003

Here is an example on how to obtain an ID for a Kind from the Service ID:

query foo { 
  serviceSource(id:"54769aae-2c63-4415-9b34-74e9ceb1d789") { 
    name 
    kinds { 
      name 
      id 
    } 
  } 
} 

The query above should return:

{ 
  "data": { 
    "serviceSource": { 
      "name": "TestService", 
      "kinds": [ 
        {           
     "name": "54769aae-2c63-4415-9b34-74e9ceb1d789_TestKind", 
          "id": "a745e4d7-74e9-4023-8f39-bc63d6076326" 
        } 
      ] 
    } 
  } 
} 

Query using Allinstances
This type of query can be used to obtain instances for a Kind in the same format as the Query Entry Point:

query { 
  allInstances(tenantId:0, kindId:"a745e4d7-74e9-4023-8f39-bc63d6076326") { 
    records { 
      STRING 
      ID 
    } 
  } 
} 

Query across kinds
More complex queries can be used to more efficiently query across Kinds. Many internal services can use this endpoint to reduce the number of DB requests required to fulfill a request.

Here is an example:

query { 
    kindDBQuery(kindQuery:{ 
      kindId:"fcd39416-337c-4566-b5bf-56cc8ff725d2" 
      fieldFilters: [{ 
        op: "==" 
        fieldName: "State" 
        value: { 
          STRING:"WA" 
        } 
      }] 
      and: [{ 
        kindId:"fcd39416-337c-4566-b5bf-56cc8ff725d2" 
        toFieldName:"City" 
        fromFieldName: "City" 
        fieldFilters:[{ 
          op: "==" 
          fieldName: "Zipcode" 
          value: { 
            STRING: "98053"
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

The outer most Kind is the type returned, while inner Kinds form a DAG where each inner kind constrains its outer kinds instances. The DAG should be considered to be a graph walk, where constraints are applied at each step in the walk, data is returned only if all constraints are true.

Filters are a collection of {op, fieldname, value} triples that constrain the Kind on which they are applied.

Operators that can be used:

for Strings : "==" , "!=" , "oneof"

for Numbers (Int and Float) : "<", "<=", ">", ">="

5.Developing knowledge microservices 1. 5.1.Project Setup

start with a project template in your preferred language

follow the instructions on customizing it

Developing Knowledge Microservices
Project Setup
To begin a project setup, begin by selecting a preferred language, and follow the instructions provided in the following sections for customization.

Maana Knowledge Service Templates
In an effort to maintain best practices, encourage code base coherence, and minimize maintenance issues, standardized procedures are offered for a number of things that a productive developer should not have to worry about and instead focus on adding real features.

Every project should follow a common, consistent pattern to reduce the tax imposed by infrastructure, such as web services and middleware, GraphQL services, logging, build, deployment, etc. We recommend always cloning a new project from the Maana Knowledge Service template for your preferred language.  Once cloned, perform the customizations recommended in the template's README.md.

Note:  If your language isn't represented, we'd be happy to work with you to develop a template -  or simply send a pull request to have it included.

Development
Steps for Deployment:

Define the GraphQL schema for the Service Endpoint.

Implement the appropriate Resolvers.

Be the client (Query, Mutate, Subscribe) to other Knowledge Services (Dependencies).

Stitch together existing Services, providing an aggregated Endpoint (to simplify client access).

Packaging
Steps for Packaging

Prepare deployment.

For a Managed Service:  Create a Docker container.

Note:  Unmanaged Services are not covered by Maana.   

Addition of a Knowledge Microservice to the MAANA Catalog
To add a Knowledge Microservice in the Maana Catalog:

Prepare a Service Manifest (basically a serialized instance of Kind Service).

Register the Service in the Catalog.

t this point, Maana will:

Examine the Service.

Create all the Kinds in KindD.

Create the boilerplate for Managed Kinds, including:

Types

Queries

Mutations

Subscriptions

Register with CGK

Boilerplate:

Queries

Mutation Resolvers

Event Emitters

Stitch Resolver schema (Synthetic to Service-provided)

Development Environments
Visual Studio Code
Most people run the "insiders" edition to have access to latest features.

Command Line
Add VS Code to your path so you can invoke it from the command line

export PATH="$PATH:/Applications/Visual Studio Code - Insiders.app/Contents/Resources/app/bin"
The 'code' command will now be available in your terminal.

Recommended Plugins
code --install-extension PeterJausovec.vscode-docker
code --install-extension christian-kohler.npm-intellisense
code --install-extension christian-kohler.path-intellisense
code --install-extension dbaeumer.vscode-eslint
code --install-extension eamodio.gitlens
code --install-extension eg2.vscode-npm-script
code --install-extension esbenp.prettier-vscode
code --install-extension kumar-harsh.graphql-for-vscode
code --install-extension mikestead.dotenv
code --install-extension ms-azuretools.vscode-azurefunctions
code --install-extension ms-vscode.azure-account
code --install-extension msjsdiag.debugger-for-chrome
code --install-extension robertohuertasm.vscode-icons
Settings
Access user settings from "Code Insiders / Preferences / Settings" and paste the following settings in the "User Settings" tab.

{
    "workbench.iconTheme": "vscode-icons",
    "workbench.colorTheme": "Default High Contrast",
    "editor.fontFamily": "'Roboto Mono Light For Powerline', Menlo, Monaco, 'Courier New', italic",
    "editor.formatOnSave": true,
    "editor.tabSize": 2,
    "editor.insertSpaces": true,
    "eslint.autoFixOnSave": true,
    "files.associations": {
        "*.js": "javascriptreact"
    },
    "explorer.confirmDragAndDrop": false,
    "explorer.confirmDelete": false,
    "window.zoomLevel": 0,
    "gitlens.advanced.messages": {
        "suppressCommitHasNoPreviousCommitWarning": false,
        "suppressCommitNotFoundWarning": false,
        "suppressFileNotUnderSourceControlWarning": false,
        "suppressGitVersionWarning": false,
        "suppressLineUncommittedWarning": false,
        "suppressNoRepositoryWarning": false,
        "suppressUpdateNotice": false,
        "suppressWelcomeNotice": true
    },
    "search.exclude": {
        "**/node_modules": true,
        "**/.git": true
    },
    "git.autofetch": true
}
Some of these are team style, some are personal preference (e.g., workbench.colorTheme).

​Atlassian​

Troubleshooting
Development Scenarios
Local: 
Configure and run a local Docker machine Maana system and custom services (i.e., a "deployment").

Maana R&D Activities:
Remote: Mixed Environments - A Maana deployment is hosted elsewhere (e.g., Azure), developing a service locally.

Not supported in Version 3.0?

If local and developing an a (locally) deployed service, then "docker stop" it and run/debug it manually.
