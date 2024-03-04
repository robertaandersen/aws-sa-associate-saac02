# Accounts
* A way to group focus areas (dev, prod, shared etc.).
* Can also be used to group togeather resources
* Have one ROOT user with full control (USE MFA!)
* Create IAM users (admin user) don't use ROOT

## IAM
* No cost
* GLobal
* Authentication (WHO)
* Authorization (WHAT)
* IDP

### Users
* Individuals / Applications

### Groups
* Users sharing similar attributes or related use cases

### Roles
* Can be used by AWS Services or for granting external access to accounts
  * E.g. EC2 instances can be put in a role which has access to S3
* Think - Use when the number of entities are not known

### Policies
* Granular access (e.g. R or W) to AWS services

Acount alias -> Alias for account ID

# Access Keys

* Long term credential
* Used or CLI
* Similar to User:Pass
* IAM user can have MAX of two keys
* Actions CREATE|DELETE|INACTIVE|ACTIVE
* Two parts : AccesKeyID and SecretAccessKey -> no retrieve
* Only for IAM users


## CLI
* `aws configure --profile iamadmin-general`
* Creates a named profile with AccessKeyId and SecretAccessKey (plus default region)
* use `aws <command> --profile iamadmin-general` to invoke profile

# Public v.s. Private services
* Referes to networking only
* Public uses public endpoints
* Private runs in a VPC
```
    +-------------------+    +-------------------+    +-----+
    |  Public Internet  | -> |  AWS public Zone  | -> | VPC |
    +-------------------+    +-------------------+    +-----+
```
* E.g. S3 public ec2 private
* Private services can have a public IP translated by IGW
* Private services also access public services via IGW

# Global Infrastructure
* Regions
  * AWS infrastructure deployed in a geographical region
* Edge location
  * Local distribution point
  * Smaller - bonly have content distribution services and edge computing services
* Availability Zone
  * Multiple zones withing a region (eu-west-1a, eu-west-1b, eu-west-1c)
  * Isolated infrastructure within a region
* Resiliance
  * Global - Data replicated accross multiple regions (IAM, route53)
  * Region resilliance - Infrastructure spread across multiple AZ's within a region
  * AZ resillient - No contingency

# VPC (The default CPV)
* Virtual network within AWS
* Resides in 1 account & 1 region
* Private / Isolated by default
* Two types: Default and Custom (Multiple)
* Default VPC
  * Initially created by AWS
  * Only one per region
  * Regionally resilliant
  * Gets VPC CIDR (172.31.0.0/16)
  * Subnets configurable to AZ, by default on SN per AZ
  * CIDR devided btw subnets:
    * eu-west-1a: SN-A: 172.31.0.0/20 (172.31.0.0 - 172.31.15.255)
    * eu-west-1b: SN-B: 172.31.16.0/20 (172.31.16.0 - 172.31.31.255)
    * eu-west-1c: SN-C: 172.31.32.0/20 (172.31.32.0 - 172.31.47.255)
  * Can be deleted and recreated
  * Internet Gateway (IGW) -> Access to and from internet
  * Security Group (SC) and  Network ACL (NACL)  -> Limit incoming and outcoming traffic
  * By default anything placed in the default VPC get's a public IP address assigned by subnet (are in the AWS public zone)

# EC2
* IAAS (Instance as a service)
* Is a private service -> launches into a single VPC subnet
* Public access controlled via VPC (default VPC makes a default public AP)
* AZ resilient
* Storage
  * Local on-host storage (on the instance)
  * EBS network storage (Elastic Block Storage)
* States
  * Running (Charges)
    * Stopping
    * Shutting Down
    * Pending
  * Stopped (RAM and CPU not charged, storage still charged)
  * Terminated (Instance will be gone - no charge)
* Charges based on DISK, RAM and CPU
* AMI: Amazon Machine Image
    * AMI permissions
      * Public: Everyone allowed access
      * Owner: Implicit allow acess
      * Explicit: Specific AWS accounts allowed access
    * Root Volume
    * Block Device Mapping (Links volumes to OS)
* Connectivity
  * WIN - RDS: 3389
  * Linux - SSH: 22


# S3

* Objects & Buckets
* Object is key value based. +(version, id, metadata, access control, subresources)
* Buckets are created in regions -> Data has primary home region and does not leave region (unless configured)
* Bucket name needs to be GLOBALLY unique (REMEMBER)
* Has a flat structure (everything stored on a root level) -> UI precents this differently where the </prefix> part of the key is interpreted as folders
* Remember:
  * Name globally unique
  * 3-63 chars
  * Has to start with lowercase letter or a number
  * Can't be ip formatted
  * Max no of buckets is 100 soft limit and 1000 hard limit per account -> bucket must be broken up into prefixes/folders
  * Unlimited objects in a bucket (0 to 5TB in size)
  * Key = Name, Value = Data make up an object

## S3 Patterns and anti patterns

* Object store - not file or block store -> no mounting
* Good for large scale data storage or distribution or upload
* Great for offload
* Input and or output to Many AWS products

# Cloundformation

* Takes a template and spins up infrastructure

````
AWSTemplateFormatVersion: "version date"

# MUST come after the AWSTemplateFormatVersion
Description:
  string

Metadata:
  template metadata

Parameters:
  set of parameters

Mappings:
  Set of mappings

Transform:
  set of transforms

# Mandatory
Resources
  set of resources

Outputs
  set of outputs
````


* Resources the only mandatory part


