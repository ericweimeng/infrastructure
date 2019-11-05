# AWS Security Essentials

## ‌AWS Secure Global Infrastructure and Compliance

‌

### Regions <a id="regions"></a>

‌

* The largest organizational unit in AWS
* Represent a geographical area that houses two or more independent AWS data centers

‌

#### As it relates to security <a id="as-it-relates-to-security"></a>

‌

* Store data in a specific region is not replicated by default
* Use regions to
  * manage compliance with regulations
  * manage network latency

‌

### Availability Zones \(AZs\) <a id="availability-zones-azs"></a>

‌

* The independent data centers in an AWS region
* High-speed links connect them together \(LAN-type connectivity\)
* There are at least two in every region

‌

#### Regarding Security <a id="regarding-security"></a>

‌

* AZs are designed for fault isolation
* Up to the user to configure their systems to take advantage of this fault isolation
  * Surviving a temporary or prolonged failure

‌

### Endpoints <a id="endpoints"></a>

‌

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

