---
category: General
title: Infrastructure
order: 1
---

## Components

Certificate Master acts as a Registration Authority (RA) in front of Certification Authorities (CAs) running in the background. The following diagram shows the main components of Certificate Master and their interaction with the IT environment.

![Certificate Master Architecture](media/cm-architecture.png)  

The components are the following:

* Website (runs on IIS); therefore supports Windows Authentication
   * Certificate Management
   * Certificate Requests
* SQL database
   * Holds data about certificates
* Windows Service
   * Fetches data from MS CAs and store them in the SQL database
* Optional component: Policy Module
   * Runs as Policy Module plug-in in the Microsoft CA to check requests for compliance

### Website

The GK Certificate Master website runs on IIS and is the single user interface for Certificate Officers, Certificate Administrators, and self-service users. Service owners can request and download certificates from the website for the applications they manage, e.g. TLS certificates. Certificate Officers can approve requests from service owners or request certificates on behalf of them. Certificate Administrators see reports of certificates, for example approaching expirations, and may revoke certificates.

Roles are based on AD group membership. When the web site is installed on a domain member server, users may log on with Integrated Authentication to have a seamless sign-in experience.

### SQL Database

Certificate Master supports Microsoft SQL Server in all editions. The recommended minimum version is SQL Server 2016.

Certificate Master caches all issued certificates in the database. This allows fast queries using a multitude of certificate properties as search parameters and is much faster than the slow native querying interface of Microsoft CAs. It also stores pending requests and enriches certificate data with additional attributes like the end recipient of a certificate.

### Windows Service

The Certificate Master Windows Service executes regular tasks:

* It fetches certificate data from backend Microsoft CAs to make them available in the web interface.
* It maintains and cleans the database.
* It executes customer-specific jobs like joining additional data sources to enrich the certificate data.
* It creates reports of expiring certificates.

### Policy Module

The Certificate Master Policy Module checks certificate requests with three possible outcomes:

* The request is immediately accepted without further intervention.
* The request is put on hold for a manual check by a Certificate Officer.
* The request is rejected, giving a readable explanation for the rejection.

The Policy Module is integrated in the Website, but may optionally also be added directly to a Microsoft CA. In this case, requests may be submitted directly do the Microsoft CA, but are still subject to the same type of validation the web site uses. For example, this allows controlling the properties of certificates distributed via Microsoft Autoenrollment or when using the CA's RPC to interface to request certificates.