# ChatQL: An AWS AppSync Chat Starter App written in Angular

### Quicklinks
 - [Introduction](#introduction)
 - [Features](#features)
 - [Getting Started](#getting-started)
   - [Prerequisites](#prerequisites)
   - [Backend setup](#backend-setup)
   - [Application Walkthrough](#application-walkthrough)
 - [Building, Deploying and Publishing](#building-deploying-and-publishing)
 - [Clean Up](#clean-up)

## Introduction

This is a Starter Angular Progressive Web Application (PWA) that uses AWS AppSync to implement offline and real-time capabilities in a chat application as part of the blog post https://aws.amazon.com/blogs/mobile/building-a-serverless-real-time-chat-application-with-aws-appsync/. In the chat app, users can have conversations with other users and exchange messages. The application demonstrates GraphQL Mutations, Queries and Subscriptions using AWS AppSync. You can use this for learning purposes or adapt either the application or the GraphQL Schema to meet your needs.

![ChatQL Overview](/media/chatql.png)

**10/3/2018**: The project has been updated to follow the latest techniques for developing mobile and web applications on AWS, including the use of Serverless Framework for deploying your application.

## Features

- PWA: A Progressive Web Application takes advantage of the latest technologies such as Service Workers to combine the best of web and mobile apps. Think of it as a website built using web technologies but that acts and feels like an app in a mobile device.

- GraphQL Mutations (`src/app/graphql/mutations`)
  - Create users
  - Create conversations and link them to users
  - Create messages in a conversations

- GraphQL Queries (`src/app/graphql/queries`)
  - Get all users
  - Get all messages in a conversation
  - Get all user related conversations

- GraphQL Subscriptions (`src/app/graphql/subscriptions`)
  - Real time updates for new messages in conversations

- Authentication
  - The app uses Cognito User Pools to onboard users and authenticate them into the app
- Authorization
  - The app uses JWT Tokens from Cognito User Pools as the authorization mechanism

## Getting Started

#### Prerequisites

* [AWS Account](https://aws.amazon.com/mobile/details) with appropriate permissions to create the related resources
* [NodeJS](https://nodejs.org/en/download/) with [NPM](https://docs.npmjs.com/getting-started/installing-node)
* [AWS CLI](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) `(pip install awscli --upgrade --user)`
* [Angular CLI](https://github.com/angular/angular-cli) `(npm install -g @angular/cli)`
* [Serverless Framework](https://serverless.com) `(npm install -g serverless)`


### Backend Setup

1. First, clone this repository and navigate to the created folder:
    ```bash
    $ git clone https://github.com/aws-samples/aws-mobile-appsync-chat-starter-angular.git
    $ cd aws-mobile-appsync-chat-starter-angular
    ```

1. Set up your AWS resources with the Serverless Framework:
    ```bash
    $ sls deploy -v
    ```

1. Run your application locally:
    ```bash
    $ npm start
    ```

1. Access your ChatQL app at http://localhost:4200. Sign up different users and test real-time/offline messaging using different browsers.

### Application Walkthrough

The application is composed of the main app module and 2 independent feature modules:

* auth.module
* chat-app.module

Both modules make use of AWS Amplify, which is initialized with the `./src/aws-exports.js` configuration (created when you deploy the backend resources) in `./src/app.module.ts`. The auth.module allows to easily onboard users in the app. To sign up to use the app, a user provides his username, password and email address. Upon succesful sign-up, a confirmation code is sent to the user's email address to confirm the account. The code can be used in the app to confirm the account, after which a user can sign in and access the chat portion of the app.

In the chat, a user can see a list of other users who have registered after sign in to the app for the first time. A user can click on a name to initiate a new conversation (`./src/app/chat-app/chat-user-list`) or can click on an existing conversation to resume it (`./src/app/chat-app/chat-convo-list`). Inside of a conversation, a user automatically receives new messages via subscriptions (`./src/app/chat-app/chat-message-view/`). When a user sends a message, the message is initially displayed with a grey check mark. This indicates that receipt confirmation from the backend server has not been received. The checkmark turns green upon confirmation the message was received by the backend. This is easily implemented by including the `isSent` flag, set to false, when we send the `createMessage` mutation. In the mutation, we request for the `isSent` flag to be returned from the backend. The flag is set to true on the backend and when returned, our template updates the css class associated with the checkmark (see `./src/src/app/chat-app/chat-message`).

![Application Overview](/media/chatql-app.png)

## Building, Deploying and Publishing

** THIS SECTION HAS BEEN REMOVED SINCE IT NEEDS TO BE RE-WRITTEN **

** TO DO: **
* Add S3 hosting to the serverless.yml
* Use something like serverless-build-client to build the client
* Use something like serverless-finch to deploy the client to the S3 bucket

## Clean Up

To clean up the project, you can simply delete the stack you created.

```bash
$ sls remove -v
```
