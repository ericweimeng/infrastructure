# ‌AWS Global Infrastructure and Compliance

## Networking & Compute‌

### Regions

* The largest organizational unit in AWS
* Represent a geographical area that houses two or more independent AWS data centers

#### As it relates to security <a id="as-it-relates-to-security"></a>

* Store data in a specific region is **not replicated by default**
* Use region to
  * manage compliance with regulations
  * manage network latency

### Availability Zones \(AZs\)

* The independent data centers in an AWS region
* High-speed links connect them together \(LAN-type connectivity\)
* There are at least two in every region

#### Regarding Security <a id="regarding-security"></a>

* AZs are designed for fault isolation
* Up to the user to configure their systems to take advantage of this fault isolation
  * Surviving a temporary or prolonged failure

### Edge Locations

* CDN endpoints for CloudFront
* There are many more Edge Locations than Regions

### VPC

* Virtual data center

### Route53

* Amazon's DNS service

### Cloud Front

* Part of the content delivery network
* A whole bunch of edge locations that will cache your assets

### Direct Connect

* Connecting up your office or physical data centers to AWS directly using a dedicated line

### EC2

* Virtual machines in the cloud

### Elastic Beanstalk

* Analyze your code and provision required resources for your app

### Lambda

* Serverless

### Lightsail

## Storage, Databases, Migration & Analytics

### Storage

#### S3 

* Virtual disk in the cloud, storing objects

#### Glacier

* Archive files from S3 off

#### EFS \(Elastic File Service\)

* Install applications or databases

#### Storage Gateway

### Databases

#### RDS

* Relational database services

#### DynamoDB

* Non-relational database

#### Redshift

* Data warehousing solution
  * Store big data
  * transfer data copy from prod DB to here

#### Elastic cache

* A way of caching your data in the cloud

### Migration

#### Snowball

* Send a disk or a whole bunch of disks to amazon

## Endpoints

* AWS provides several different ways to connect to its services
  * Called endpoints
* For access outside a region
  * Users can connect using three different methods
    * AWS console \(Web console launched through the browser\)
    * AWS CLI \(Command line interface, connects through a terminal program\)
    * AWS APIs \(Application Programming Interfaces\)
  * Content Delivery Network \(CDN\) Endpoints
    * CloudFront Edge Locations
* Examples of endpoints that exist inside a region and use web URLs
  * S3
  * DynamoDB
* Examples of services inside Availability Zones that use endpoints
  * EC2 \(public subnet\)
  * Elastic Load Balancer
* VPC Endpoint

‌

### Identity and Access Management \(IAM\) <a id="identity-and-access-management-iam"></a>

‌

* Global scope across all AWS \(all regions\)
  * Allows for large-scale granularity
  * For example, a user can be granted EC2 access in all regions, one region, or multiple regions
* Allows for central management of
  * Users
  * Passwords
  * Access Keys
  * Permissions
  * Groups
  * Roles

‌

### AWS Cloud Compliance <a id="aws-cloud-compliance"></a>

‌

* Programs for finance, healthcare, government and more
* Documents that AWS meets regulatory, audit and security standards
  * HIPAA
  * ISO Standards
  * Various regulatory and security agencies around the world
* Only applies to the services and infrastructure that AWS is responsible for
* Does not mean that the applications and data that you deploy in your AWS environment are compliant

