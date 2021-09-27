---
layout: post
title: "Building A Real Estate Application"
date: 2021-09-27
categories: blog
---

# A Refresher On The Real Estate Application

A property management application must handle the following:
1. Keep tracks of payments coming in and out of the agency. Breaking this down further, we get:
    - Tenants making single payments to the agency,
    - The agency making single or batch payments bills or invoices,
    - The agency making single or batch payments to landlords.
2. Keep a record of the landlords, properties and tenants.
    - In particular a record of the maintanence of a property.
3. Generate statements such as the tenant ledger and the owner statement.

These three items will form the basis of the minimum viable product for this project.

Other potential features include:
- Batch payment processing from tenants,
- An Email & SMS API to send single or batch emails or sms,
- Creating work orders for maintainence (potentially using the Email & SMS API),
- Individual Landlord and Tenant portals where they can view their documents.

***

# System Architecture

## Ten Thousand Foot View

I plan on building the following three APIs for my backend. 
- The Core API - Handles the core property management functionality outlined above,
- The Users API - AuthN & AuthZ,
- The Mailing API - Allows our users to send single or batch emails or sms to others. This should only be available to real estate agents and not available to landlords and tenants.

And in the future, if my mind and body are willing:
- Landlords API - Allows landlords to view information and documents relating to their properties,
- Tenants API - Allows tenants to view information and documents relating to their tenancy.

## The Core API

The heart of the application is the payments system. Since we are dealing with financial transactions, payment records should be immutable. To delete a payment is to say that the payment never existed and that is a problem. For example, when a payment is accidentally credited to your bank account. The bank doesn't delete the record, instead the bank creates a new payment record as debit instead. This is a very, very strong case for the event driven Apache Kafka and it's immutable logs.

For now though, it's currently set to be a Loopback 4 server application with a PostgreSQL database. I won't rule out a rewrite with Kafka. I have been spending some time familiarizing myself with the Kafka concepts. I might even learn Java.

## The Users API

When the user successfully authenticates, the Users API will send JWT token along with a refresh token. The user then authorizes using the JWT token when calling other APIs in the system. For more information, the talk ["Authentication as a Microservice" by Brian Pontarelli](https://www.youtube.com/watch?v=SLc3cTlypwM) was my inspiration.

Currently set to be an Express Server Application using Firebase Auth. I will definitely rewrite this when I come back to the Users API.

## The Mailing API

I haven't thought too much about this as it is not part of my minimum viable product. I intend on this being an Express application that consumes the Twilio/SendGrid services.

## A Note on the Landlord API and Tenant API

Haven't thought too much about this as well. But I think a key-value NoSQL database like Cassandra seems to be a good fit for the document system here.

***

# Journal

## What I Have Learnt So Far
- Accidentally created integration tests with Mocha and Chai by trying to do unit testing.
- How to use Firebase in my application and hide the configurations in a .env file.
- About JWT tokens and refresh tokens.

## What I Like So Far
- Loopback 4: The generated code is extremely well built and well laid out. I'm in love with the OpenAPI explorer and it's "API Design First" approach. It's documentation is amazing as well, I love how they outline concepts like dependency injection, extensions & extension points. It has really solidified ideas such as controllers and models for me.
- Firebase Auth Emulator: Amazing for integration testing. There are a few kinks like the reset password not working with chai-http, but I believe it's a relatively new project from Firebase.
- Testing APIs with Postman
- Unit and Integration Testing

## What I Need To Work On
- Unit Testing
- Domain Driven Design
- SQL and Database Structuring
- Core Concepts of Loopback 4 such as Dependency Injection, Inversion of Control, etc.
- OAuth2.0 Flow and Validating Signed JWT Tokens
- React and Redux

And maybe the following
- Apache Kafka and maybe Java
- TypeORM