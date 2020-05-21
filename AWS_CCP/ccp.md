We create iAM role so that we can use sessions

AMI: snapshot or saving an entire copy of your server
An AMI is a template that contains the software configuration (operating system, application server, and applications) required to launch your instance. You can select an AMI provided by AWS, our user community, or the AWS Marketplace; or you can select one of your own AMIs.

### Auto scaling Groups
Multiple servers running
Rules to check if one is not running then another will balance the load
Websites lot of traffic, determines if application needs more server and when demand is low the servers are reduced
Deleting ASG shuts the attached instances as well

### Elastic load balancer(ELB)
instance might be running on different availability zones. One Azs become unavailable . Dont experience downtime

**A security group is a set of firewall rules that control the traffic for your instance. For example, if you want to set up a web server and allow Internet traffic to reach your instance, add rules that allow unrestricted access to the HTTP and HTTPS ports. You can create a new security group or select from an existing one below.**

evenly distribute loads among
Target groups: Step 4: Configure Routing

Cancel Previous Next: Register Targets
Your load balancer routes requests to the targets in this target group using the protocol and port that you specify, and performs health checks on the targets using these health check settings. Note that each target group can be associated with only one load balancer.

CloudFront(Content distribution network): is used as a CDN to make the shortest route to the enduser. it take the conetent and copy to multiple edge location

###RDS
Virtual Private Cloud (VPC)
**Performance Insights** is an advanced database performance monitoring feature that makes it easy to diagnose and solve performance challenges on Amazon RDS databases. It offers a free tier with 7 days of rolling data retention, and a paid long-term data retention option.

used postgreSQL: free
Template: Production (670USD/mth), dev/test(200/mth), free tier

The Amazon RDS **Free Tier** is available to you for 12 months. Each calendar month, the free tier will allow you to use the Amazon RDS resources listed below for free:
750 hrs of Amazon RDS in a Single-AZ db.t2.micro Instance.
20 GB of General Purpose Storage (SSD).
20 GB for automated backup storage and any user-initiated DB Snapshots.

When you free usage expires or if your application use exceeds the free usage tiers, you simply pay standard, pay-as-you-go service rates as described in the Amazon RDS Pricing page.

Enable storage autoscaling: Choose the dynamic storage scaling setting required by planned workload of this DB instance

Storage autoscalingInfo
Provides dynamic scaling support for your database’s storage based on your application’s needs.

Enable storage autoscaling
Enabling this feature will allow the storage to increase once the specified threshold is exceeded. (We will disable coz using it for test purpose)

**Backup retention period**
Choose the number of days that Amazon RDS should retain automatic backups of this DB instance.
The backup retention period determines the period for which you can perform a point-in-time recovery
Aurora offers 1-day backup retention for free!

Backup
Creates a point in time snapshot of your database

Enable automatic backups
Enabling backups will automatically create backups of your database during a certain time window.
Backup retention periodInfo
Choose the number of days that RDS should retain automatic backups for this instance. (We will make it 0 days)

**Performance Insights**
Performance Insights is an advanced database performance monitoring feature that makes it easy to diagnose and solve performance challenges on Amazon RDS databases. It offers a free tier with 7 days of rolling data retention, and a paid long-term data retention option. For more information about Performance Insights, please visit the

Enabling Enhanced monitoring metrics are useful when you want to see how different processes or threads use the CPU

After creating RDS see the credential option

#####Connection details to your database database-1
This is the only time you will be able to view this password. Copy and save the password for your reference, otherwise you will need to modify the database to change it. You can use a SQL client application or utility to connect to your database.

In order to access database we will require Endpoint url, security groups etc

#### Lambda function
create function using any language

    exports.handler = async (event) => {
        // TODO implement
        const response = {
            statusCode: 200,
            body: JSON.stringify('Pragya'),
        };
        console.log('event', event);
        return response;
    };

we can trigger that by adding triggers

#### AWS quick start
Quick Starts are built by AWS solutions architects and partners to help you deploy popular technologies on AWS, based on AWS best practices for security and high availability. These accelerators reduce hundreds of manual procedures into just a few steps, so you can build your production environment quickly and start using it immediately.

Each Quick Start includes **AWS CloudFormation** templates that automate the deployment and a guide that discusses the architecture and provides step-by-step deployment instructions.

**Amazon S3 Intelligent-Tiering** is the most cost-effective object class to store frequently and infrequently accessed data. INTELLIGENT_TIERING delivers automatic cost savings by moving data on a granular object level. Also, you can use this storage class to optimize storage costs by automatically moving data to the most cost-effective storage access tier, without performance impact or operational overhead.

the **S3** standard is cost-effective only for frequently accessed data. This is the default storage class.

**S3 Glacier** is cost-effective for storing archive data and most infrequently accessed data. The retrieval time for data is a few hours, but this is the cheapest storage option cost-wise.

**S3 One Zone-Infrequently** Accessed storage class is cost-effective for storing data in a single zone. The data is not resilient to the physical loss of the Availability Zone resulting from disasters, such as earthquakes and floods. This is also less available.

**AWS Redshift** is a data warehouse service. Redshift supports client connections with many types of applications, including business intelligence (BI), reporting, data, and analytics tools.

The following are the key concepts for VPCs:

A **virtual private cloud (VPC)** is a virtual network dedicated to your AWS account.

* A subnet is a range of IP addresses in your VPC.
* A route table contains a set of rules, called routes, that are used to determine where network traffic is directed.

* An internet gateway is a horizontally scaled, redundant, and highly available VPC component that allows communication between instances in your VPC and the internet. It therefore imposes no availability risks or bandwidth constraints on your network traffic.

* A VPC endpoint enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by PrivateLink without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection. Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic between your VPC and the other service does not leave the Amazon network.

A **security group** acts as a virtual firewall for your instance to control inbound and outbound traffic. When you launch an instance in a VPC, you can assign up to five security groups to the instance. Security groups act at the instance level, not the subnet level. Therefore, each instance in a subnet in your VPC can be assigned to a different set of security groups.

If you launch an instance using the Amazon EC2 API or a command line tool and you don't specify a security group, the instance is automatically assigned to the default security group for the VPC. If you launch an instance using the Amazon EC2 console, you have an option to create a new security group for the instance.