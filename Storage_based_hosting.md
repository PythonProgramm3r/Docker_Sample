## Storage based hosting 


### Guide :
* [link](https://medium.com/tensult/aws-hosting-static-website-on-s3-using-a-custom-domain-cd2782758b2c)

---

## Requirements :

* Domain name on godaddy
* S3 bucket with same name as domain name, S3 bucket with www subdomain name 
* AWS Route53 Public zone 
* Webpage (static)

---


# Background / Tools
- What is [S3](https://s3.console.aws.amazon.com/s3/get-started?region=us-east-1)
	- S3, simple storage service, is an object storage service that offers industry-leading scalability, data availability, security, and performance. S3 is used to store and protect any amount of data for virtually any use case, such as data lakes, cloud-native applications, and mobile apps.
		
		### Benefits
			- Data performance and durability
			- Security, compliance, and auditing
			- Flexible storage options
			- Granular data control
		### Uses
			- Backup and restore
			- Disaster recovery
			- Archive
			- Data lakes and big data analytics
			- Hybrid cloud storage
			- Cloud-native application data (Our primary usage on this task)
		
		
- What is [Route53](https://us-east-1.console.aws.amazon.com/route53/v2/home?region=us-east-1#Home)
	- Route53 is a highly available and scalable cloud Domain Name System (DNS) web service.
	
		### Products
			- Domain names : A domain is the name, such as example.com, that your users use to access your application.
			- Hosted zones : Specify how you want Route 53 to respond to DNS queries for a domain such as example.com.
			- Health checks : Monitor your applications and web resources, and direct DNS queries to healthy resources.
			- Traffic flow : Use a visual tool to create policies for multiple endpoints in complex configurations.
			- Resolver : Route DNS queries between your VPCs and your network.
		### Benefits
			- Highly available and reliable
			- Designed for use with other AWS services
			- Simple
			- Flexible
		### Uses
			- Global traffic management
			- Alias to AWS resources (Our primary usage on this task)
			
- Notes on cost
	- $0.54 per month total, traffic seems to impact negligibly at roughly 20 visits per month
### Where does content go + bucket setup
- S3 buckets will store website content such as main webpage and your assets

### What is a [publicly accessible bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-control-block-public-access.html#access-control-block-public-access-policy-status)?
- The Amazon S3 Block Public Access feature provides settings for access points, buckets, and accounts to help you manage public access to Amazon S3 resources. By default, new buckets, access points, and objects don't allow public access. However, users can modify bucket policies, access point policies, or object permissions to allow public access. S3 Block Public Access settings override these policies and permissions so that you can limit public access to these resources. 
- ACLs : Amazon S3 considers a bucket or object ACL public if it grants any permissions to members of the predefined AllUsers or AuthenticatedUsers groups.
- Policies : When evaluating a bucket policy, Amazon S3 begins by assuming that the policy is public. It then evaluates the policy to determine whether it qualifies as non-public. To be considered non-public, a bucket policy must grant access only to fixed values (values that don't contain a wildcard or an AWS Identity and Access Management Policy Variable) of one or more of the following :
- A set of Classless Inter-Domain Routings (CIDRs), using aws:SourceIp
- s3:DataAccessPointArn **(Our primary usage on this task)**  
	- [Sample Bucket Policies](https://docs.aws.amazon.com/AmazonS3/latest/userguide/example-bucket-policies.html)
	- [S3 Operations & Actions](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-with-s3-actions.html)
	- [S3 Actions, resources, and condition keys list](https://docs.aws.amazon.com/AmazonS3/latest/userguide/list_amazons3.html)
	- This is showing a Data Access point to that is described as s3 bucket resource that is allowing public access to get contents
- Other policy types:
	- An AWS principal, user, role, or service principal (e.g. aws:PrincipalOrgID)
	- aws:SourceArn
	- aws:SourceVpc
	- aws:SourceVpce
	- aws:SourceOwner
	- aws:SourceAccount
	- s3:x-amz-server-side-encryption-aws-kms-key-id
	- aws:userid, outside the pattern "AROLEID:*"
- [Website Permission Documentation](https://docs.aws.amazon.com/AmazonS3/latest/userguide/WebsiteAccessPermissionsReqd.html)


### Where does the Policy get added?  
- Amazon's [add bucket guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/add-bucket-policy.html)
- AWS route53 public hosted zone with the same name as the domain name.
- This referes to a public 
- Update GoDaddy NS with AWS provided NS. NS = Named Server
- In AWS Route53 create a recordset for the main domain and select Alias Checkbox to Yes and point it to S3 main bucket.
- In AWS Route53 create a recordset for www subdomain and point to S3 www bucket.
- Access the website using your domain name. 
  
---	
# Steps : 

### Associating bucket content with domain name on GoDaddy

### Setting up S3 Bucket
- Create 2 buckets named: 
	- *******.com
	- www.*******.com (set as public accessible)
* on your main domain bucket (*******.com), on the properties tab, redirect to your www subdomain (www.*******.com)
	- this allows you to redirect traffic requests to your content
* ensure your bucket is public by adding this policy 

---

```

{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "PublicReadGetObject",
			"Effect":"Allow",
			"Principal": "*",
			"Action": "s3:GetObject",
			"Resource": "arn:aws:s3:::www.dataknightsltd.com/*"
		}
	]
}

```
---
### Policy details: 
- "Sid": "PublicReadGetObject"
	- Grants permission to return configuration information about the specified access point
- "Effect":"Allow"
	- Options to set group and permissions 
- "Action": "s3:GetObject"
	- Grants permission to retrieve objects from Amazon S3
	- Other actions such as put, post, delete are used also on templates which allows roles,groups,etc. permissions to modify s3 buckets
	- Each action in the Actions table identifies the resource types that can be specified with that action. s
- "Resource": "arn:aws:s3:::www.dataknightsltd.com/*"
	- This is what resource AWS is pointing to
	- [Resource Types & ARN Formatting](https://docs.aws.amazon.com/AmazonS3/latest/userguide/list_amazons3.html#amazons3-resources-for-iam-policies)
### Setting up AWS Route53
* AWS route53 public hosted zone with the same name as the domain name.
* Update GoDaddy NS with AWS provided NS. NS = Named Server
	- This is used to redirect incoming traffic requests to AWS, to get the stored webpage content
* In AWS Route53 create a recordset for the main domain and select Alias Checkbox to Yes and point it to S3 main bucket.	
* In AWS Route53 create a recordset for www subdomain and point to S3 www bucket.
 - This gives instructions to route53 to direct traffic request to bucket content
* Access the website using your domain name.

