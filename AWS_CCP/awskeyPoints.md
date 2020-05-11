Security group basics
The following are the basic characteristics of security groups for your VPC:

There are quotas on the number of security groups that you can create per VPC, the number of rules that you can add to each security group, and the number of security groups that you can associate with a network interface. For more information, see Amazon VPC quotas.

You can specify allow rules, but not deny rules.

You can specify separate rules for inbound and outbound traffic.

When you create a security group, it has no inbound rules. Therefore, no inbound traffic originating from another host to your instance is allowed until you add inbound rules to the security group.

By default, a security group includes an outbound rule that allows all outbound traffic. You can remove the rule and add outbound rules that allow specific outbound traffic only. If your security group has no outbound rules, no outbound traffic originating from your instance is allowed.

Security groups are stateful — if you send a request from your instance, the response traffic for that request is allowed to flow in regardless of inbound security group rules. Responses to allowed inbound traffic are allowed to flow out, regardless of outbound rules.

Instances associated with a security group can't talk to each other unless you add rules allowing the traffic (exception: the default security group has these rules by default).


Regardless the method your ISP uses to get internet to your home, you will always be faced with the same problem. The signals traveling through the telephone wires your ISP owns are analog signals (wave frequencies), while your computer runs on digital signals (1s and 0s). This is where the modem comes in.

The modem takes that analog signal, converts it to a digital signal, and sends it over to the router. And this is where the fun begins, because the router has a lot of responsibility for making your home network functional. Before reading one more word, you need to remember one important fact

An IP address is a 32 bit number that identifies a specific network or computer.Today, most IP addresses are IPV4 addresses, which means that they are 32 bit rather than 64 bit IPV6 addresses. The reason we need IPV6 addresses is because we are running out of the former. If you calculate how many combinations you can have with 32 bits, it is 4.2 billion (2³²). This seems like a large number, but with a world population between 7 and 8 billion, you can see how this could be constraining. With IPV6, we have 1.8x10¹⁹ addresses (2⁶⁴), which will never pose a capacity issue.

**AWS X-Ray** can be used to troubleshoot errors while hosting microservices applications in the cloud. X-Ray collects data about requests that your application serves, and provides tools you can use to view, filter, and gain insights into that data to identify issues and opportunities for optimization. It uses “trace data” to generate the “service graph” of AWS resources.

**CloudWatch** you gain system-wide visibility into resource utilization, application performance, and operational health. CloudWatch is used to collect and track metrics, which are variables you can measure for your resources and applications.

we can use the **AWS Launch Configuration** to efficiently automate the replication and deployment of a specific software configuration existent on one EC2 instance onto others. Launch configuration already has the configuration details of the existing EC2 instances and therefore can be used to replicate the same even across different environments.

A **launch configuration** is an instance configuration template that an Auto Scaling group uses to launch EC2 instances. When you create a launch configuration, you specify information for the instances. Include the ID of the Amazon Machine Image (AMI), the instance type, a key pair, one or more security groups, and a block device mapping. If you've launched an EC2 instance before, you specified the same information in order to launch the instance.

Intelligent Tiering moves data between frequent and infrequent access.

**QucikSight** provides dynamic and interactive visuals. Dashboards can be accessed from any device, and embedded into your applications, portals, and websites. You pay only for what you use.

**trusted advisor** provides recommendations on performance, cost optimization, security, fault tolerance, and service limits.

 multi-node deployment architecture of Amazon Redshift data warehouse, what is the role of a leader node?
 leader node manages communications with client programs and all communication with compute nodes. The leader node distributes SQL to the compute nodes when a query references user-created tables or system tables. A query that does not reference any tables, runs exclusively on the leader node.

 **AWS CodeDeploy** is used to automate the installation, updating of a set of applications on a series of EC2 instances and on-premise servers. With CodeDeploy you can deploy scripts, packages, multi-media files, Lambda functions, code, etc.

 **CodeDeploy** is a deployment service that automates application deployments to Amazon EC2 instances,
on-premises instances, serverless Lambda functions, or Amazon ECS services.
You can deploy a nearly unlimited variety of application content, including:
• code
• serverless AWS Lambda functions
• web and configuration files
• executables
• packages
• scripts
• multimedia files
CodeDeploy can deploy application content that runs on a server and is stored in Amazon S3 buckets,
GitHub repositories, or Bitbucket repositories. CodeDeploy can also deploy a serverless Lambda function.
You do not need to make changes to your existing code before you can use CodeDeploy.

**EBS volumes** can be attached to EC2 instances to store data. They are also used for maintaining database backups. If you add instance store volumes or EBS volumes to your instance in addition to the root device volume, the block device mapping for the new AMI contains information for these volumes and the block device mappings for instances that you launch from the new AMI automatically contain information for these volumes.

**S3** is an object-based storage and cannot be attached to EC2 instances. This is highly durable and secure storage for storing “objects” and retrieval of the same. Although S3 is a global service, the data stored in it is regional.

**Route 53** can be used to divert traffic to other AWS resources in cases of disaster scenarios and most useful in a disaster recovery method. Users can configure a “failover routing policy” in Route53 for failover scenarios, (i.e) where they want the traffic to be diverted in case of a failure of the current domain.

 **AWS Shield Standard** is automatically included at no extra and for additional protection against DDoS attacks, AWS also provides AWS Shield Standard and AWS Shield Advanced.

  to increase the availability of an application you deploy the resources across multiple availability zones.

  disk disposal, hardware patching, and physical security fall under the purview of AWS. AWS is also responsible for environmental controls in a data center. Disk disposal is done as per industry standards.

   Lambda, SNS, and DynamoDb are serverless components of AWS.

    Lambda, SNS, and DynamoDb are serverless components of AWS.

    **penetration testing** includes both “permitted services” and “prohibited services”. For the permitted services no prior approval is needed but for the “prohibited services”, the customer needs to get prior approval and consent from AWS to run the vulnerability and penetration tests ( Also this is a general practice )


**AWS Elasticbeanstalk** helps in deploying applications built in .Net and Java quickly. User needs to just upload the code in the console and Elastic BeanStalk automatically takes care of provisioning and scaling.

**on-demand EC2** instances are priced on the region that is selected (Asia Pacific), the instance type and the AMI type.

Elasticache can improve performance when there are frequent reads of static content on a web application. Elasticache supports two engines namely the “redis” and “memcached” and both are used for caching static content. By caching, the load on the application is reduced

**CloudTrail** event logs contain the access calls to S3 from CloudFront and the logs can be filtered out for CloudFront calls. Event history allows you to view, search, and download the past 90 days of activity in your AWS account. In addition, you can create a CloudTrail trail to archive, analyze, and respond to changes in your AWS resources.

Security Groups and Network Access Control Lists act as a firewall to the VPC and the instances in it. Security groups act as the first line of defence for the ec2 instances, while the NACL acts as the line of defence for the subnets.

an AMI is used to launch EC2 instances. AMI would include: EBS snapshots, Launch permissions that control which AWS accounts can use the AMI to launch instances and block device mapping that specifies the volumes to attach to the instance when it's launched. This is sometimes referred to as the “golden image”.

the AWS Security Hub which is part of the AWS Organizations provides a comprehensive view of your security state within AWS that helps you check your compliance with security industry standards and best practices.

Elastic File System ( EFS ) provides shared file access to store petabytes of data. The user pays only for the storage used by the file system. There is no setup cost. EFS comes in two flavors of storage classes - Standard and Infrequent access.

S3 uses lifecycle policies for archival storage. You can use lifecycle policies to define actions you want Amazon S3 to take during an object's lifetime (for example, transition objects to another storage class, archive them, or delete them after a specified period of time). You can define a lifecycle policy for all objects or a subset of objects in the bucket by using a shared prefix

Define **S3 Lifecycle configuration rules** for objects that have a well-defined lifecycle. For example:
• If you upload periodic logs to a bucket, your application might need them for a week or a month. After
that, you might want to delete them.
• Some documents are frequently accessed for a limited period of time. After that, they are infrequently
accessed. At some point, you might not need real-time access to them, but your organization or
regulations might require you to archive them for a specific period. After that, you can delete them.
• You might upload some types of data to Amazon S3 primarily for archival purposes. For example, you
might archive digital media, financial and healthcare records, raw genomics sequence data, long-term
database backups, and data that must be retained for regulatory compliance.
With S3 Lifecycle configuration rules, you can tell Amazon S3 to transition objects to less expensive
storage classes, or archive or delete them.

SQS is a messaging service to store messages in queues. This can also be used to “decouple” components of an application. The “dead letter queues” can be used for the storage of messages for analysis at a later time.

AWS is responsible for global infrastructure while it is the user’s responsibility for the security of the EC2 instances. AWS is responsible for all the activities related to data centers, such as physical security, environmental control, etc. Since the question is asking about the features of **Amazon Aurora**, it a fully managed service from AWS end. It is a component of the “Platform As A Service” cloud model and therefore Amazon is responsible for managing the operating system and patching of software for the underlying EC2 instances.

a fault-tolerant system is available even if some of the AWS EC2 instances are degraded. This will make the web application highly available.


* You have 2 accounts in your AWS account. One for the Dev and the other for QA. All are part of consolidated billing. The master account has purchased 3 reserved instances. The Dev department is currently using 2 reserved instances. The QA team is planning on using 3 EC2 instance launch types. What is the pricing tier of the instances that can be used by the QA Team?

There is one reserved instance that is still not used by any team and therefore we can avail the advantage of using consolidated billing by using this balance one reserved instance that is not being used by any team.



Since the QA team is planning to use 3 EC2 instances the remaining two EC2 instances can be on-demand instances.



Therefore the combination “1 RI + 2 On-demand” would be the most recommended pricing tier that can be used by the QA team.

AWS has a global presence in terms of its’ worldwide regions, availability zones, and edge locations. AWS automatically distributes the data globally for higher durability. By deploying the application across availability zones one can make the application highly available and therefore AWS makes it easy to architect for high availability.

Intelligent tiering is useful for customers who want to optimize storage costs automatically when data access patterns change. Lifecycle policies need to be manually triggered.

Failover routing routes the traffic to the web application that is up and running when the other co-located web application is experience degradation.

SQS is used to build a decoupled architecture based on the requirements. By decoupling, it takes the operating burden off of a web application. SQS is also used to store messages and analyze them at a later time.

Geolocation routing is used when you want to route traffic based on the location of the users.

When you create a record, you choose a routing policy, which determines how Amazon Route 53
responds to queries:
• Simple routing policy – Use for a single resource that performs a given function for your domain, for
example, a web server that serves content for the example.com website.
• Failover routing policy – Use when you want to configure active-passive failover.
• Geolocation routing policy – Use when you want to route traffic based on the location of your users.
• Geoproximity routing policy – Use when you want to route traffic based on the location of your
resources and, optionally, shift traffic from resources in one location to resources in another.
• Latency routing policy – Use when you have resources in multiple AWS Regions and you want to route
traffic to the region that provides the best latency.
• Multivalue answer routing policy – Use when you want Route 53 to respond to DNS queries with up
to eight healthy records selected at random.
• Weighted routing policy – Use to route traffic to multiple resources in proportions that you specify.

**AWS Autoscaling** allows the user to increase the number of resources on demand.

AWS Aurora is 5 times faster than the traditional MySQL database.

